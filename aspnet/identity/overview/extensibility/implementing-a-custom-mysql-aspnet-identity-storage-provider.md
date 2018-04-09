---
uid: identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
title: Implementace zprostředkovatele úložiště ASP.NET Identity vlastní MySQL | Microsoft Docs
author: raquelsa
description: ASP.NET Identity je rozšiřitelný systém díky tomu můžete vytvořit vlastního poskytovatele úložiště a zařadit ho do aplikace bez znovu práce aplika...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: 248f5fe7-39ba-40ea-ab1e-71a69b0bd649
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
msc.type: authoredcontent
ms.openlocfilehash: d843b31e011fe520aad6cfdab0beca2d12477f12
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="implementing-a-custom-mysql-aspnet-identity-storage-provider"></a>Implementace zprostředkovatele úložiště ASP.NET Identity vlastní MySQL
====================
podle [Raquel Soares De Almeida](https://github.com/raquelsa), [Suhas Joshi](https://github.com/suhasj), [tní FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity je rozšiřitelný systém díky tomu můžete vytvořit vlastního poskytovatele úložiště a zařadit ho do aplikace bez znovu práce aplikace. Toto téma popisuje postup vytvoření poskytovatele úložiště MySQL pro ASP.NET Identity. Přehled vytváření zprostředkovatelů vlastní úložiště najdete v tématu [přehled poskytovatelů vlastní úložiště pro ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).
> 
> K dokončení tohoto kurzu, musíte mít Visual Studio 2013 s aktualizací 2.
> 
> V tomto kurzu bude:
> 
> - Ukazují, jak vytvořit instance databáze MySQL na Azure.
> - Ukazují, jak použít klienta nástroj MySQL (MySQL Workbench) vytvářet tabulky a správu vzdálené databáze v Azure.
> - Ukazují, jak nahradit výchozí implementace úložiště ASP.NET Identity naše vlastní implementaci na projekt aplikace MVC.
> 
> Tento kurz byl původně zapsán Raquel Soares De Almeida a Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ). Ukázkový projekt byl aktualizován pro Identity 2.0 Suhas Joshi. Téma byl aktualizován pro Identity 2.0 tní FitzMacken.


## <a name="download-completed-project"></a>Stažení dokončit projektu

Na konci tohoto kurzu budete mít projekt aplikace MVC s ASP.NET Identity práci s databází MySQL hostovanou v Azure.

Si můžete stáhnout dokončené poskytovatele úložiště MySQL v [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).

## <a name="the-steps-you-will-perform"></a>Kroky, které budete provádět

V tomto kurzu provedete následující:

1. Vytvoření databáze MySQL na Azure
2. Vytváření tabulek ASP.NET Identity v MySQL
3. Vytvoření aplikace MVC a nakonfigurujte ho na použití zprostředkovatele MySQL
4. Spuštění aplikace

Toto téma nepopisuje architekturu ASP.NET Identity a rozhodnutí, která musí provést při implementaci zprostředkovatele úložiště zákazníka. Podrobnosti naleznete v tématu [přehled z vlastních poskytovatelů úložiště pro ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).

## <a name="review-mysql-storage-provider-classes"></a>Zkontrolujte MySQL třídy zprostředkovatele úložiště

Před přechod do kroky k vytvoření databáze MySQL zprostředkovatele úložiště, podíváme se na třídy, které tvoří zprostředkovatele úložiště. Budete potřebovat třídy, které spravují databázových operací a tříd, které se nazývají z aplikace pro správu uživatelů a rolí.

### <a name="storage-classes"></a>Třídy úložiště

- [IdentityUser](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs) – obsahuje vlastnosti pro uživatele.
- [Úložiště UserStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs) – obsahuje operace přidávání, aktualizace nebo načítání uživatelů.
- [IdentityRole](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs) – obsahuje vlastnosti pro role.
- [Úložiště RoleStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) – obsahuje operace přidávání, odstraňování, aktualizaci nebo načtení role.

### <a name="data-access-layer-classes"></a>Data access layer třídy

V tomto příkladu obsahovat data přístupové vrstvy třídy příkazů SQL pro práci s tabulkami; ale ve vašem kódu můžete chtít použít například Entity Framework nebo NHibernate objekt relační mapování (ORM). Konkrétně vaše aplikace může nízký výkon bez ORM zahrnující opožděného načítání a ukládání do mezipaměti objektů. Další informace najdete v tématu [ASP.NET Identity 2.0 bez Entity Framework?](https://aspnetidentity.codeplex.com/discussions/561828)

- [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) – obsahuje připojení k databázi MySQL a metody pro provádění databázových operací. Úložiště UserStore a úložiště RoleStore jsou vytvořena s instance této třídy.
- [RoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) – obsahuje databázových operací pro tabulku, která ukládá role.
- [UserClaimsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) – obsahuje databázových operací pro tabulka, která obsahuje deklarace identity uživatelů.
- [UserLoginsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) – obsahuje databázových operací pro tabulku, která ukládá přihlašovací informace uživatele.
- [UserRoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) – obsahuje databázových operací pro tabulka, která obsahuje uživatele, kteří jsou přiřazeny k jednotlivé role.
- [UserTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) – obsahuje databázových operací pro tabulka, která obsahuje uživatele.

## <a name="create-a-mysql-database-instance-on-azure"></a>Vytvoření instance databáze MySQL na Azure

1. Přihlaste se k [portál Azure](https://manage.windowsazure.com/).
2. Klikněte na tlačítko **+ nový** v dolní části stránky a potom vyberte **úložiště**.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.png)
3. V **zvolte a rozšíření** průvodci vyberte **databáze MySQL ClearDB** a klikněte na šipku další v pravém dolním rohu dialogu.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.png)
4. Ponechte výchozí **volné** plán a změnit **název** k **IdentityMySQLDatabase**. Vyberte oblast nejbližší je a pak klikněte na šipku Další.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.png)
5. Kliknutím na značku zaškrtnutí dokončete tvorbu databáze.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image4.png)
6. Po vytvoření databáze, můžete spravovat z **doplňky** kartě v portálu pro správu.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image5.png)
7. Informace o připojení databáze můžete získat kliknutím na **informace o připojení** v dolní části stránky.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image6.png)
8. Zkopírujte připojovací řetězec kliknutím na tlačítko Kopírovat a ji uložit, abyste mohli používat později v aplikaci MVC.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image7.png)

## <a name="create-the-aspnet-identity-tables-in-a-mysql-database"></a>ASP.NET Identity vytváření tabulek v databázi MySQL

### <a name="install-mysql-workbench-tool-to-connect-and-manage-mysql-database"></a>Instalace nástroje MySQL Workbench k připojení a správě databáze MySQL

1. Nainstalujte **MySQL Workbench** nástroje z [MySQL stáhne stránky](http://dev.mysql.com/downloads/windows/installer/)
2. Spusťte aplikaci a přidejte kliknutím na **MySQLConnections +** tlačítko Přidat nové připojení. Použijte data řetězec připojení, které jste zkopírovali z databáze MySQL na Azure, kterou jste vytvořili dříve v tomto kurzu.
3. Po navázání připojení, otevřete nové **dotazu** kartě; vkládání příkazů z [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql) do dotazu a provést k vytvoření databázových tabulek.
4. Nyní máte všechny ASP.NET Identity nezbytné tabulky vytvořit v databázi MySQL hostovanou v Azure, jak je uvedeno níže.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.jpg)

## <a name="create-an-mvc-application-project-from-template-and-configure-it-to-use-mysql-provider"></a>Vytvoření projektu aplikace MVC z šablony a nakonfigurujte ho na použití zprostředkovatele MySQL

V případě potřeby nainstalujte buď [Visual Studio Express 2013 pro Web](https://go.microsoft.com/fwlink/?LinkId=299058) nebo [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) s aktualizací 2.

### <a name="download-the-aspnetidentitymysql-project-from-codeplex"></a>Stáhněte si projekt ASP.NET.Identity.MySQL na webu CodePlex

1. Přejděte na adresu URL úložiště v [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).
2. Stáhněte si zdrojového kódu.
3. Extrahujte soubor ZIP do místní složky.
4. Otevřete řešení AspNet.Identity.MySQL a sestavte jej.

### <a name="create-a-new-mvc-application-project-from-template"></a>Vytvořte nový projekt aplikace MVC ze šablony

1. Klikněte pravým tlačítkem **AspNet.Identity.MySQL** řešení a **přidat**, **nový projekt**
2. V **přidat nový projekt** dialogovém okně vyberte **Visual C#** na levé straně, pak **webové** a pak vyberte **webové aplikace ASP.NET**. Název projektu **IdentityMySQLDemo**; a potom klikněte na tlačítko OK.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.jpg)
3. V **nový projekt ASP.NET** dialogovém okně, vyberte šablonu MVC s výchozími možnostmi (který obsahuje **jednotlivé uživatelské účty** jako metodu ověřování) a klikněte na tlačítko **OK** .![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.jpg)
4. V Průzkumníku řešení klikněte pravým tlačítkem na projekt IdentityMySQLDemo a vyberte **spravovat balíčky NuGet**. V dialogovém okně hledání textového pole zadejte **Identity.EntityFramework**. V seznamu výsledků vyberte tento balíček a klikněte na tlačítko **odinstalovat**. Zobrazí se výzva k odinstalaci balíčku závislosti objektu EntityFramework. Klikněte na Ano, jak jsme již nebude možné tento balíček do této aplikace.
5. Klikněte pravým tlačítkem na projekt IdentityMySQLDemo, vyberte **přidat**, **odkaz na řešení, projekty;** vyberte AspNet.Identity.MySQL projekt a klikněte na tlačítko **OK**.
6. V projektu IdentityMySQLDemo nahraďte všechny odkazy na  
    `using Microsoft.AspNet.Identity.EntityFramework;`  
   with  
     `using AspNet.Identity.MySQL;`
7. V IdentityModels.cs, nastavte **ApplicationDbContext** k odvozování z **MySqlDatabase** a zahrnují contructor, který trvat jeden parametr s názvem připojení.  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample1.cs)]
8. Otevřete soubor IdentityConfig.cs. V **ApplicationUserManager.Create** metoda, nahraďte vytváření instancí objekt UserManager následujícím kódem:  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample2.cs)]
9. Otevřete soubor web.config a nahraďte řetězec objekt DefaultConnection tato položka nahrazení zvýrazněné hodnoty připojovacího řetězce databáze MySQL, kterou jste vytvořili v předchozích krocích:  

    [!code-xml[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample3.xml?highlight=2)]

## <a name="run-the-app-and-connect-to-the-mysql-db"></a>Spusťte aplikaci a připojte se k databázi MySQL

1. Klikněte pravým tlačítkem **IdentityMySQLDemo** projektu a vyberte **nastavit jako spouštěný projekt**
2. Stiskněte klávesu **kombinaci kláves Ctrl + F5** sestavení a spuštění aplikace.
3. Klikněte na **zaregistrovat** karty v horní části stránky.
4. Zadejte nové uživatelské jméno a heslo a potom klikněte na **zaregistrovat**.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image8.png)
5. Nový uživatel je teď zaregistrované a přihlášení.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image9.png)
6. Přejděte zpět do nástroje MySQL Workbench a zkontrolovat **IdentityMySQLDatabase** obsahu tabulky. Zkontrolujte položky v tabulce uživatelů při registraci noví uživatelé.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image10.png)

## <a name="next-steps"></a>Další kroky

Další informace o tom, jak povolit další metody ověřování na tuto aplikaci, najdete v části [vytvořit aplikaci ASP.NET MVC 5 s použitím Facebook a Google OAuth2 a přihlašování OpenID](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

Zjistěte, jak integrovat váš DB OAuth a nastavit role omezit přístup uživatele k vaší aplikaci, najdete v tématu [nasazení aplikace zabezpečené ASP.NET MVC 5 s členství, OAuth a databáze SQL Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).
