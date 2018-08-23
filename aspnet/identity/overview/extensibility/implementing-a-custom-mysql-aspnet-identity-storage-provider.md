---
uid: identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
title: Implementace zprostředkovatele úložiště vlastních MySQL ASP.NET Identity | Dokumentace Microsoftu
author: raquelsa
description: ASP.NET Identity je rozšiřitelný systém, který vám umožní vytvořit poskytovatele úložiště a zařadit ho do své aplikace bez znovu práce aplikace...
ms.author: riande
ms.date: 05/22/2015
ms.assetid: 248f5fe7-39ba-40ea-ab1e-71a69b0bd649
msc.legacyurl: /identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
msc.type: authoredcontent
ms.openlocfilehash: 358b1a3b7277f21c63a1d395f2a5fce79bbe0d56
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755020"
---
<a name="implementing-a-custom-mysql-aspnet-identity-storage-provider"></a>Implementace zprostředkovatele úložiště vlastních MySQL ASP.NET Identity
====================
podle [Raquel Soares De Almeida](https://github.com/raquelsa), [Suhas Joshi](https://github.com/suhasj), [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity je rozšiřitelný systém, který vám umožní vytvořit poskytovatele úložiště a připojte ho do své aplikace bez přepracování aplikace. Toto téma popisuje, jak vytvořit poskytovatele úložiště MySQL ASP.NET identity. Přehled vytváření poskytovatelé vlastního úložiště najdete v tématu [přehled poskytovatelů vlastního úložiště pro ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).
> 
> Abyste mohli absolvovat tento kurz, musíte mít Visual Studio 2013 s aktualizací Update 2.
> 
> V tomto kurzu se:
> 
> - Ukazují, jak vytvořit instance databáze MySQL v Azure.
> - Ukazují, jak pomocí nástroje MySQL klienta (MySQL Workbench) vytvářet tabulky a spravovat vaše vzdálené databáze v Azure.
> - Ukazují, jak nahradit výchozí implementace úložiště ASP.NET Identity naší vlastní implementaci na projekt aplikace MVC.
> 
> V tomto kurzu byl původně zapsán Raquel Soares De Almeida a Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ). Ukázkový projekt byla aktualizována pro Identity 2.0 Suhas Joshi. Tom FitzMacken aktualizoval 2.0 identit tématu.


## <a name="download-completed-project"></a>Stažení se dokončilo projektu

Na konci tohoto kurzu budete mít projekt aplikace MVC s ASP.NET Identity práci s databází MySQL hostovanou v Azure.

Můžete si stáhnout dokončený poskytovatele úložiště MySQL v [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).

## <a name="the-steps-you-will-perform"></a>Kroky, které budete provádět

V tomto kurzu provedete následující:

1. Vytvořit databázi MySQL v Azure
2. Vytvoření tabulek v ASP.NET Identity v MySQL
3. Vytvoření aplikace MVC a nakonfigurujte ho na použití poskytovatele MySQL
4. Spuštění aplikace

Toto téma nepopisuje architekturu ASP.NET Identity a rozhodnutí, která musí provést při implementaci zprostředkovatele úložiště zákazníka. Informace najdete v tématu [přehled o vlastní poskytovatelé úložiště pro ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).

## <a name="review-mysql-storage-provider-classes"></a>Zkontrolujte třídy poskytovatele úložiště MySQL

Než si řekneme kroky k vytvoření poskytovatele úložiště MySQL, Podívejme se na třídy, které tvoří poskytovatele úložiště. Budete potřebovat třídy, které spravují databázových operací a tříd, které se volají z aplikace pro správu uživatelů a rolí.

### <a name="storage-classes"></a>Třídy úložiště

- [IdentityUser](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs) – obsahuje vlastnosti pro daného uživatele.
- [Úložiště UserStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs) – obsahuje operace pro přidání, aktualizaci nebo načítání uživatelů.
- [IdentityRole](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs) – obsahuje vlastnosti pro role.
- [Úložiště RoleStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) – obsahuje operace pro přidávání, odstraňování, aktualizaci nebo načtení rolí.

### <a name="data-access-layer-classes"></a>Datové třídy vrstva přístupu

V tomto příkladu obsahují vrstvu tříd pro přístup k data SQL příkazy pro práci s tabulkami; v kódu však může být vhodné pro použití objektově relační mapování (ORM), jako je například rozhraní Entity Framework nebo NHibernate. Konkrétně se vaše aplikace může nízký výkon bez ORM, která obsahuje objekt ukládání do mezipaměti a opožděná načítání. Další informace najdete v tématu [ASP.NET Identity 2.0 bez rozhraní Entity Framework?](https://aspnetidentity.codeplex.com/discussions/561828)

- [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) – obsahuje metody pro provádění databázových operací a připojení k databázi MySQL. Úložiště UserStore a úložiště RoleStore jsou vytvořena pomocí instance této třídy.
- [RoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) – obsahuje databázové operace pro tabulku, která ukládá role.
- [UserClaimsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) – obsahuje databázové operace pro tabulku, která ukládá deklarace identity uživatelů.
- [UserLoginsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) – obsahuje databázové operace pro tabulku, která ukládá přihlašovací informace uživatele.
- [UserRoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) – databázové operace pro tabulky, který ukládá, kteří uživatelé jsou přiřazení rolím, které obsahuje.
- [UserTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) – obsahuje databázové operace pro tabulku, která obsahuje uživatele.

## <a name="create-a-mysql-database-instance-on-azure"></a>Vytvoření instance databáze MySQL v Azure

1. Přihlaste se k [webu Azure Portal](https://manage.windowsazure.com/).
2. Klikněte na tlačítko **+ nová** v dolní části stránky a pak vyberte **úložiště**.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.png)
3. V **zvolit a doplněk** průvodce, vyberte **databázi ClearDB MySQL** a klikněte na šipku v pravém dolním rohu dialogového okna.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.png)
4. Ponechte výchozí **Free** plánu a změňte **název** k **IdentityMySQLDatabase**. Vyberte oblast nejbližší a potom klikněte na šipku Další.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.png)
5. Kliknutím na značku zaškrtnutí dokončete vytváření databáze.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image4.png)
6. Po vytvoření vaší databáze ji můžete spravovat z **doplňky** karta na portálu management portal.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image5.png)
7. Informace o připojení k databázi můžete získat kliknutím na **informace o připojení** v dolní části stránky.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image6.png)
8. Kliknutím na tlačítko Kopírovat zkopírujte připojovací řetězec a uložte ho, abyste mohli použít později v aplikaci MVC.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image7.png)

## <a name="create-the-aspnet-identity-tables-in-a-mysql-database"></a>ASP.NET Identity vytváření tabulek v databázi MySQL

### <a name="install-mysql-workbench-tool-to-connect-and-manage-mysql-database"></a>Instalace nástroje MySQL Workbench k připojení a správě databáze MySQL

1. Nainstalujte **aplikace MySQL Workbench** nástroj z [stránky pro MySQL stažení](http://dev.mysql.com/downloads/windows/installer/)
2. Spusťte aplikaci a přidejte kliknutím na **MySQLConnections +** tlačítko pro přidání nového připojení. Data řetězce připojení, který jste zkopírovali z databáze Azure MySQL, kterou jste vytvořili dříve v tomto kurzu použijte.
3. Po vytvoření připojení se otevře nová **dotazu** kartě; vkládání příkazů z [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql) do dotazu a proveďte jej chcete-li vytvořit tabulky databáze.
4. Nyní máte všechny ASP.NET Identity nezbytné tabulky vytvořené v databázi MySQL hostovanou na Azure, jak je znázorněno níže.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.jpg)

## <a name="create-an-mvc-application-project-from-template-and-configure-it-to-use-mysql-provider"></a>Vytvořte projekt aplikace MVC ze šablony a nakonfigurujte ho na použití poskytovatele MySQL

V případě potřeby nainstalujte buď [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) nebo [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) s aktualizací Update 2.

### <a name="download-the-aspnetidentitymysql-project-from-codeplex"></a>Stáhněte si ASP.NET.Identity.MySQL projekt z webu CodePlex

1. Přejděte na adresu URL úložiště na [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).
2. Stáhněte si zdrojový kód.
3. Soubor .zip extrahujte do místní složky.
4. Otevřete řešení AspNet.Identity.MySQL a sestavte ho.

### <a name="create-a-new-mvc-application-project-from-template"></a>Vytvoření nového projektu aplikace MVC ze šablony

1. Klikněte pravým tlačítkem myši **AspNet.Identity.MySQL** řešení a **přidat**, **nový projekt**
2. V **přidat nový projekt** dialogové okno Vybrat **Visual C#** na levé straně, pak **webové** a pak vyberte **webová aplikace ASP.NET**. Pojmenujte svůj projekt **IdentityMySQLDemo**; a potom klikněte na tlačítko OK.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.jpg)
3. V **nový projekt ASP.NET** dialogového okna, vyberte šablonu MVC s výchozími možnostmi (zahrnující **jednotlivé uživatelské účty** jako metodu ověřování) a klikněte na tlačítko **OK** .![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.jpg)
4. V Průzkumníku řešení klikněte pravým tlačítkem myši na projekt IdentityMySQLDemo a vyberte **spravovat balíčky NuGet**. V dialogovém okně hledání textového pole zadejte **Identity.EntityFramework**. V seznamu výsledků vyberte tento balíček a klikněte na tlačítko **odinstalovat**. Zobrazí výzva pro odinstalaci balíčku závislosti objektu EntityFramework. Klikněte na Ano, jak to uděláme už tento balíček na tuto aplikaci.
5. Klikněte pravým tlačítkem na projekt IdentityMySQLDemo, vyberte **přidat**, **odkaz, řešení, projekty;** vyberte AspNet.Identity.MySQL projekt a klikněte na tlačítko **OK**.
6. V projektu IdentityMySQLDemo nahraďte všechny odkazy na  
    `using Microsoft.AspNet.Identity.EntityFramework;`  
   with  
     `using AspNet.Identity.MySQL;`
7. V IdentityModels.cs, nastavte **ApplicationDbContext** odvodit z **MySqlDatabase** a zahrnují konstruktoru, který obsahovat jeden parametr s názvem připojení.  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample1.cs)]
8. Otevřete soubor IdentityConfig.cs. V **ApplicationUserManager.Create** metody, nahraďte vytvoření instance objektu UserManager následujícím kódem:  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample2.cs)]
9. Otevřete soubor web.config a nahraďte řetězec objekt DefaultConnection tuto položku zadejte zvýrazněné hodnoty připojovacího řetězce databáze MySQL, kterou jste vytvořili v předchozích krocích:  

    [!code-xml[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample3.xml?highlight=2)]

## <a name="run-the-app-and-connect-to-the-mysql-db"></a>Spusťte aplikaci a připojte se k databázi MySQL

1. Klikněte pravým tlačítkem myši **IdentityMySQLDemo** projektu a vyberte **nastavit jako spouštěný projekt**
2. Stisknutím klávesy **Ctrl + F5** sestavíte a spustíte aplikaci.
3. Klikněte na **zaregistrovat** kartě v horní části stránky.
4. Zadejte nové uživatelské jméno a heslo a potom klikněte na **zaregistrovat**.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image8.png)
5. Nový uživatel je teď zaregistrované a přihlášení.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image9.png)
6. Přejděte zpět do nástroje MySQL Workbench a zkontrolujte **IdentityMySQLDatabase** obsah této tabulky. Kontrola tabulce uživatele pro položky při registraci nového uživatele.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image10.png)

## <a name="next-steps"></a>Další kroky

Další informace o tom, jak povolit další metody ověřování v této aplikaci, najdete [vytvoření aplikace ASP.NET MVC 5 pomocí Facebooku a Google OAuth2 nebo OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

Zjistěte, jak integrovat vaše databáze s OAuth a nastavit role a omezit přístup uživatele k vaší aplikaci, najdete v článku [Nasaďte aplikaci zabezpečené aplikace ASP.NET MVC 5 s nástroji Membership, OAuth a SQL Database do Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).
