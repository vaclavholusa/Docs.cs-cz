---
uid: identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
title: Migrace dat Universal zprostředkovatele členství a uživatelské profily mají být ASP.NET Identity (C#) | Microsoft Docs
author: rustd
description: Tento kurz popisuje kroky, které jsou potřebné k migraci uživatelů a rolí dat a data uživatelského profilu vytvořený Universal Providers stávající aplikace...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/13/2013
ms.topic: article
ms.assetid: 2e260430-d13c-4658-bd05-e256fc0d63b8
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: f65f93b20543d06ea70a9009b6921e297477c99e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity-c"></a>Migrace dat Universal zprostředkovatele členství a uživatelské profily mají být ASP.NET Identity (C#)
====================
podle [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Roberta Mcmurrayho](https://github.com/rmcmurray), [Suhas Joshi](https://github.com/suhasj)

> Tento kurz popisuje kroky, které jsou potřebné k migraci uživatelů a rolí dat a data uživatelského profilu vytvořené pomocí stávající aplikace do modelu identitu ASP.NET Universal Providers. Přístup se zde uvedeným k migraci dat profilu uživatele lze v aplikaci s také členství SQL.


S verzí sady Visual Studio 2013, ASP.NET team zavedly nový systém ASP.NET Identity, které si můžete přečíst informace o tomto vydání [zde](../../index.md). Následující na článek migrace webových aplikací z [členství SQL do nového systému Identity](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md), tento článek ukazuje kroky při migraci stávajících aplikací, které následují modelu poskytovatelé pro správu uživatelů a rolí do nového modelu Identity. Fokus tohoto kurzu bude hlavně na migraci dat profilu uživatele se bezproblémově připojí do nového systému. Informace o migraci uživatelů a rolí je podobné pro členství SQL. Přístup k migraci dat profilu a potom lze v aplikaci s také členství SQL.

Jako příklad spustíme s webové aplikace vytvořený pomocí Visual Studio 2012, která používá model poskytovatelů. Jsme budete pak přidat kód pro správu profilů, zaregistrovat uživatele, přidat data profilu pro uživatele, migraci schématu databáze a poté změňte aplikaci používat systém identit pro správu uživatelů a rolí. Jako test migrace uživatelé vytvořený Universal Providers by měli být schopni se přihlásit a noví uživatelé musí být možné zaregistrovat.

> [!NOTE]
> Můžete najít úplnou ukázku najdete na adrese [ https://github.com/suhasj/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations).


## <a name="profile-data-migration-summary"></a>Souhrn migrace dat profilu

Před zahájením s byla migrace, dejte nám se podívejte na možnosti ukládání dat profilu v modelu poskytovatelů. Data profilu pro aplikaci, kterou uživatelé mohou být uloženy ve více způsobů, nejběžnější mezi nimi probíhá pomocí integrované profilu zprostředkovatele dodaný spolu s Universal Providers. By mělo zahrnovat kroky

1. Přidejte třídu, která obsahuje vlastnosti, které používají k ukládání dat profilu.
2. Přidejte třídu, která rozšiřuje 'ProfileBase' a implementuje metody k získání výše uvedená data profilu pro uživatele.
3. Povolit použití výchozí profil zprostředkovatele v *web.config* souboru a definovat třídu deklarované v kroku 2 #, který se má použít při přístupu ke informace o profilu.

Informace o profilu se ukládají jako serializovaný xml a binární data v tabulce 'profily, v databázi.

Po migraci aplikaci, aby používala nový systém ASP.NET Identity, je informace o profilu deserializovat a uloženy jako vlastnosti na třídu uživatelů. Každou vlastnost pak lze mapovat na sloupce v tabulce uživatelských. Výhoda zde spočívá v tom, že vlastnosti bylo možné pracovat přímo pomocí třídu uživatelů kromě nemá k serializaci nebo deserializaci dat informace čas při k ní přistupují.

## <a name="getting-started"></a>Začínáme

1. Vytvoření nové aplikace webových formulářů ASP.NET 4.5 v sadě Visual Studio 2012. Aktuální Ukázka používá šablony webových formulářů, ale můžete také použít aplikaci MVC.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.jpg)
2. Vytvořte novou složku 'modely, k ukládání informací o profilu  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.png)
3. Jako příklad dejte nám uložte datum narození, Město, výšky a váhu uživatele v profilu. Výška a váhy jsou uloženy jako vlastní třídu s názvem 'PersonalStats'. K ukládání a načítání profil, potřebujeme třídu, která rozšiřuje 'ProfileBase'. Umožňuje vytvořit novou třídu, AppProfile' Pokud chcete získat a ukládání informací o profilu.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample1.cs)]
4. Povolit v profilu *web.config* souboru. Zadejte název třídy, který se má použít pro ukládání a načítání informace o uživateli, které jsou vytvořené v kroku #3.

    [!code-xml[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample2.xml)]
5. Přidáte stránku webové formuláře ve složce "Účtem" získat data profilu od uživatele a uložte ho. Klikněte pravým tlačítkem na projekt a vyberte možnost "Přidat novou položku". Přidání nové stránky webových formulářů s stránky předlohy 'AddProfileData.aspx'. V části "MainContent, zkopírujte následující:

    [!code-html[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample3.html)]

   Přidejte následující kód v kódu:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample4.cs)]

   Přidáte obor názvů, pod které AppProfile třída definována, chcete-li odebrat chyby kompilace.
6. Spusťte aplikaci a vytvořit nového uživatele s uživatelským jménem "**olduser'.** Přejděte na stránku 'AddProfileData' a přidejte informace o profilu pro uživatele.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.png)

Můžete ověřit, že jsou data uložena jako serializovaný xml v tabulce 'profily, pomocí okna Průzkumníka serveru. V sadě Visual Studio z nabídky, zobrazení, zvolte 'Průzkumníka serveru'. Měla by existovat datové připojení pro databázi definované v *web.config* souboru. Kliknutím na datové připojení zobrazí různé dílčí kategorie. Rozbalte 'Tabulky' zobrazit různých tabulek v databázi, potom klikněte pravým tlačítkem na 'profily, a zvolte 'Zobrazit Data tabulky' Chcete-li zobrazit data profilu uložená v tabulce profilů.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.png)

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image4.png)

## <a name="migrating-database-schema"></a>Migrace schématu databáze

Chcete-li pracovat s systém identit existující databázi, je potřeba aktualizovat schéma v databázi Identity pro podporu pole, kterou jsme přidali do původní databáze. To lze provést pomocí skriptů SQL k vytvoření nové tabulky a zkopírovat informace. V okně 'Průzkumníka serveru, rozbalte možnost "DefaultConnection' k zobrazení tabulek. Klikněte pravým tlačítkem na tabulky a vyberte možnost 'nový dotaz.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image5.png)

Vložte skript SQL z [ https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt ](https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt) a potom ho spusťte. Pokud 'objekt DefaultConnection, aktualizaci, vidíme, aby byly přidány nové tabulky. Můžete zkontrolovat data uvnitř tabulek, které chcete zobrazit, že byla migrována informace.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image6.png)

## <a name="migrating-the-application-to-use-aspnet-identity"></a>Migrace aplikace pro používání ASP.NET Identity

1. Instalace balíčků Nuget, které jsou potřebné pro ASP.NET Identity:

    - Microsoft.AspNet.Identity.EntityFramework
    - Microsoft.AspNet.Identity.Owin
    - Microsoft.Owin.Host.SystemWeb
    - Microsoft.Owin.Security.Facebook
    - Microsoft.Owin.Security.Google
    - Microsoft.Owin.Security.MicrosoftAccount
    - Microsoft.Owin.Security.Twitter

   Další informace o spravovat balíčky Nuget najdete [sem](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog)
2. Pro práci s existujícími daty v tabulce, potřebujeme vytvořit model třídy, které mapování zpět do tabulky a propojte je v systému Identity. V rámci Identity kontraktu třídy modelu buď musí implementovat rozhraní definované v knihovně dll Identity.Core nebo můžete rozšířit stávající provádění těchto rozhraní, které jsou k dispozici v Microsoft.AspNet.Identity.EntityFramework. Použijeme existujících tříd pro roli, přihlášení uživatele a deklarace identity uživatelů. Je potřeba použít vlastního uživatele pro naše ukázka. Klikněte pravým tlačítkem na projekt a vytvořte novou složku 'IdentityModels'. Přidejte novou třídu "Uživatelem", jak je uvedeno níže:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample5.cs)]

   Všimněte si, ProfileInfo je nyní vlastnost v třídě uživatele. Proto jsme můžete použít třídu uživatele přímo pracovat s daty profilu.

Zkopírujte soubory v **IdentityModels** a **IdentityAccount** složek ze zdroje stahování ( [ https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations) ). Tyto mají zbývající třídy modelu a potřebné pro správu rolí pomocí rozhraní API ASP.NET Identity uživatelů a nové stránky. Metoda používaná je podobný členství SQL a naleznete podrobné vysvětlení [zde](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md).

## <a name="copying-profile-data-to-the-new-tables"></a>Kopírování dat profilu do nové tabulky

Jak už bylo zmíněno dříve, je potřeba deserializovat xml data v tabulkách profily a uložte ho sloupce tabulky AspNetUsers. Nové sloupce byly vytvořeny v tabulce uživatelů v předchozím kroku, takže již zbývá k naplnění tyto sloupce s potřebná data. K tomuto účelu použijeme konzolovou aplikaci, která se spustí jednou k naplnění nově vytvořený sloupců v tabulce uživatelů.

1. Vytvořte novou konzolovou aplikaci v existujícímu řešení.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.jpg)
2. Nainstalujte nejnovější verzi balíčku Entity Framework.
3. Přidáte webovou aplikaci vytvořili výše jako odkaz na konzolové aplikace. Uděláte to klikněte pravým tlačítkem na projekt a potom "Přidat odkazy", pak řešení, potom klikněte na projekt a klikněte na tlačítko OK.
4. Kopírování níže uvedeného kódu ve třídě Program.cs. Tato logika čte data profilu pro každého uživatele, serializuje jako 'ProfileInfo' objekt a uloží je zpět do databáze.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample6.cs)]

   Některé modely použité jsou definovány ve složce 'IdentityModels' projekt webové aplikace, musí obsahovat odpovídající obory názvů.
5. Ve výše uvedeném kódu funguje na soubor databáze v aplikaci\_vytvořit složku Data projektu webové aplikace v předchozích krocích. Chcete-li který, aktualizujte připojovací řetězec v souboru app.config v konzolové aplikace s připojovací řetězec v souboru web.config webové aplikace. Zadejte také úplnou fyzickou cestu ve vlastnosti 'AttachDbFilename'.
6. Otevřete příkazový řádek a přejděte do složky bin výše konzolové aplikace. Spuštění spustitelného souboru a prohlédněte si výstup protokolu, jak je znázorněno na následujícím obrázku.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.jpg)
7. Otevřete v tabulce 'AspNetUsers' v Průzkumníku serveru a ověřit data v nové sloupce, které mají vlastnosti. Je třeba aktualizovat s odpovídající hodnoty vlastností.

## <a name="verify-functionality"></a>Ověřte funkčnost

Pomocí stránky nově přidané členství, které jsou implementovány pomocí ASP.NET Identity k přihlášení uživatele z databáze staré. Uživatel by měl být moct přihlásit pomocí stejných přihlašovacích údajů. Vyzkoušejte jiné funkce jako je přidání OAuth, vytváření nového uživatele, změna hesla, přidání rolí, přidání uživatelů do rolí, atd.

Data profilu pro původního uživatele a noví uživatelé musí načíst a uložené v tabulce uživatelů. Staré tabulky by už odkazovat.

## <a name="conclusion"></a>Závěr

Článek popisuje proces migrace webových aplikací, které používají model zprostředkovatele pro členství ASP.NET Identity. Článek dále uvedených migrace data profilu pro uživatele k jazyka do systém identit. Nechejte prosím komentáře níže odpovědi na otázky a problémy došlo při migraci vaší aplikace.

*Díky Rick Anderson a Roberta Mcmurrayho kontroly článek.*
