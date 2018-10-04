---
uid: identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
title: Migrace dat od univerzálního zprostředkovatele pro členství a uživatelských profilů na ASP.NET Identity (C#) | Dokumentace Microsoftu
author: rustd
description: Tento kurz popisuje kroky, které jsou potřebné k migraci uživatelů a role data a data uživatelského profilu vytvořeného Universal Providers existující aplikaci...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: 2e260430-d13c-4658-bd05-e256fc0d63b8
msc.legacyurl: /identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: a91bb6ac51819d7dbb8eb3c63bd36a9d830eecce
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578222"
---
<a name="migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity-c"></a>Migrace dat od univerzálního zprostředkovatele pro členství a uživatelských profilů na ASP.NET Identity (C#)
====================
podle [Pranav Rastogi](https://github.com/rustd), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Robert McMurray](https://github.com/rmcmurray), [Suhas Joshi](https://github.com/suhasj)

> Tento kurz popisuje kroky, které jsou potřebné k migraci uživatelů a role data a data uživatelského profilu vytvořeného Universal Providers existující aplikace do modelu ASP.NET Identity. Tento přístup uvedeným k migraci dat profilu uživatele, je použít v aplikaci s také členství SQL.


S vydáním sady Visual Studio 2013, tým ASP.NET zavedly nový systém ASP.NET Identity, které si můžete přečíst další informace o této verzi [tady](../../index.md). Na článek migrace webových aplikací před [členství SQL na nový systém identit](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md), tento článek ukazuje postup při migraci stávajících aplikací, které se řídí modelem poskytovatelé pro správu uživatelů a rolí Nový model Identity. Fokus v tomto kurzu budou primárně na migraci dat profilu uživatele do bez problémů integrovat do nového systému. Migrace informací o uživatelích a role je podobná pro členství v SQL. Postup pro migraci dat profilu je možné v aplikaci s také členství SQL.

Jako příklad Začneme s webovou aplikací vytvořených pomocí Visual Studio 2012, která používá model poskytovatelů. Přidáme pak přidejte kód pro správu profilů, registrace uživatele, data profilu pro uživatele, migraci schématu databáze a potom změňte aplikace pro systém identit pro správu uživatelů a rolí. Jako test migrace vytvořené pomocí Universal Providers uživatelé měli být schopni přihlášení a noví uživatelé by se moci zaregistrovat.

> [!NOTE]
> Můžete najít úplnou ukázku v [ https://github.com/suhasj/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations).


## <a name="profile-data-migration-summary"></a>Shrnutí migrace dat profilu

Před zahájením s migrace, Podívejme se na možnosti ukládání dat profilu v modelu poskytovatelů. Data profilu pro aplikaci, kterou uživatelé mohou být uloženy několika různými způsoby, nejběžnější mezi nimi probíhá pomocí integrované profilech dodávaná Universal Providers. Zahrnuje kroky

1. Přidáte třídu, která má vlastnosti sloužící k ukládání dat profilu.
2. Přidáte třídu, která rozšiřuje "ProfileBase" a implementuje metody k získání výše uvedená data profilu pro uživatele.
3. Povolení s využitím výchozí profil poskytovatele v *web.config* souborů a definování třídy deklarován v kroku 2 #, který se má použít v získávání informací o profilu.

Informace o profilu se ukládá jako serializovaná xml a binární data v tabulce "Profily" v databázi.

Po migraci aplikaci, aby používala nový systém identit technologie ASP.NET, je informace o profilu deserializovat a uloženy jako vlastnosti na třídu uživatelů. Každou vlastnost je pak mapovat na sloupce v tabulce uživatelských. Výhoda zde spočívá v tom, že vlastnosti je možné pracovat přímo pomocí třídy uživatelů kromě nebudete muset serializovat a deserializovat data informace při přístupu.

## <a name="getting-started"></a>Začínáme

1. Vytvoření nové aplikace webových formulářů ASP.NET 4.5 v sadě Visual Studio 2012. Aktuální Ukázka používá šablony webových formulářů, ale můžete také použít aplikaci MVC.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.jpg)
2. Vytvoření nové složky "modelů ukládání informací o profilu  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.png)
3. Jako příklad dejte nám uložte datum narození, Město, výšku a váhy uživatele v profilu. Výška a váhy jsou uloženy jako vlastní třídu s názvem "PersonalStats". K ukládání a načítání profilu, potřebujeme třídu, která rozšiřuje "ProfileBase". Vytvoříme novou třídu 'AppProfile' k získání a ukládání informací o profilu.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample1.cs)]
4. Povolte profil v *web.config* souboru. Zadejte název třídy, který se použije k uloží nebo načtou informace o uživateli, vytvořený v kroku #3.

    [!code-xml[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample2.xml)]
5. Přidejte stránku webové formuláře ve složce "Účet" získat data profilu uživatele a uloží je. Klikněte pravým tlačítkem na projekt a. Vyberte Přidat novou položku. Přidání nové stránky webových formulářů s hlavní stránkou "AddProfileData.aspx". V části "MainContent" zkopírujte do něj následující:

    [!code-html[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample3.html)]

   V kódu přidejte následující kód:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample4.cs)]

   Přidáte obor názvů v rámci které AppProfile třída je definována odebrat chyby kompilace.
6. Spusťte aplikaci a vytvořit nového uživatele s uživatelským jménem "**olduser".** Přejděte na stránku "AddProfileData" a přidejte informace o profilu uživatele.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.png)

Můžete ověřit, že jsou data uložená jako serializovaná xml v tabulce "Profily" pomocí Průzkumníka serveru. V sadě Visual Studio v nabídce "Zobrazit" zvolte "Průzkumníka serveru". Měla by existovat datového připojení pro databázi podle *web.config* souboru. Kliknutím na datové připojení zobrazuje různých podkategorií. Rozbalte položku "Tables" k zobrazení různých tabulek v databázi, potom pravým tlačítkem myši klikněte na "Profily" a zvolte "Zobrazit Data tabulky' Chcete-li zobrazit data profilu uložená v tabulce profilů.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.png)

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image4.png)

## <a name="migrating-database-schema"></a>Migrace schématu databáze

Chcete-li existující databáze, pracovat s systém identit, musíme aktualizovat schéma databáze Identity pro podporu polí, kterou jsme přidali do původní databáze. To lze provést pomocí skriptů SQL k vytvoření nových tabulek a zkopírovat existující informace. V okně 'Průzkumníka serveru' rozbalte možnost "DefaultConnection" k zobrazení tabulek. Klikněte pravým tlačítkem na tabulky a vyberte "nový dotaz.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image5.png)

Vložte skript jazyka SQL ze [ https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt ](https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt) a spustíme ji. "Možnost DefaultConnection" aktualizaci, vidíme, že jsou přidány nové tabulky. Můžete zkontrolovat data uvnitř tabulek, které chcete zobrazit, že se migroval informace.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image6.png)

## <a name="migrating-the-application-to-use-aspnet-identity"></a>Migrace aplikace pro používání technologie ASP.NET Identity

1. Instalace balíčků Nuget, které jsou pro ASP.NET Identity:

    - Microsoft.AspNet.Identity.EntityFramework
    - Microsoft.AspNet.Identity.Owin
    - Microsoft.Owin.Host.SystemWeb
    - Microsoft.Owin.Security.Facebook
    - Microsoft.Owin.Security.Google
    - Microsoft.Owin.Security.MicrosoftAccount
    - Microsoft.Owin.Security.Twitter

   Další informace o správě balíčků Nuget najdete [zde](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog)
2. Pro práci s existujícími daty v tabulce, potřebujeme vytvořit model tříd, které mapují zpět do tabulek a připojení je v systému identit. Jako součást smlouvy Identity tříd modelu by měla být buď implementovat rozhraní definované v knihovně dll Identity.Core nebo můžete rozšířit stávající implementaci těchto rozhraní, které jsou k dispozici v Microsoft.AspNet.Identity.EntityFramework. Použijeme existujících tříd pro roli, přihlášení uživatele a deklarace identity uživatelů. Musíme pro naši ukázku s použitím vlastní uživatele. Klikněte pravým tlačítkem na projekt a vytvořte novou složku "IdentityModels". Přidejte novou třídu "Uživatel", jak je znázorněno níže:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample5.cs)]

   Všimněte si, že "ProfileInfo" je nyní vlastnost ve třídě uživatele. Proto jsme můžete použít třídu uživatele přímo pracovat s daty profilu.

Kopírovat soubory **IdentityModels** a **IdentityAccount** složek ze zdroje stahování ( [ https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations) ). Ty se musí zbývající tříd modelu a potřebné pro správu role pomocí rozhraní API pro ASP.NET Identity uživatelů a nové stránky. Tato metoda je podobný členství SQL a podrobné vysvětlení najdete [tady](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md).

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

## <a name="copying-profile-data-to-the-new-tables"></a>Kopírování dat profilu do nové tabulky

Jak už bylo zmíněno dříve, potřebujeme k deserializaci dat xml do tabulky profilů a jeho uložení ve sloupcích tabulky AspNetUsers. Nové sloupce byly vytvořeny v tabulce uživatelů v předchozím kroku, stačí je k naplnění těchto sloupců se potřebná data. K tomuto účelu použijeme konzolovou aplikaci, která se spustí jednou k naplnění nově vytvořené sloupce v tabulce uživatelů.

1. Vytvořte novou konzolovou aplikaci v rámci stávající řešení.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.jpg)
2. Nainstalujte nejnovější verzi balíčku Entity Framework.
3. Přidáte webovou aplikaci vytvořili výše jako odkaz na aplikaci konzoly. Provedete to klikněte pravým tlačítkem na projekt a potom "Přidat odkaz", pak řešení, pak klikněte na projekt a klikněte na tlačítko OK.
4. Kopírování níže uvedeného kódu ve třídě Program.cs. Tuto logiku čte data profilu pro každého uživatele, serializuje jako "ProfileInfo" objekt a uloží je zpět do databáze.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample6.cs)]

   Některé modely použité jsou definovány ve složce "IdentityModels" projektu webové aplikace, proto musí obsahovat odpovídající obory názvů.
5. Výše uvedený kód funguje na databázový soubor v aplikaci\_Data složky projekt webové aplikace vytvořené v předchozích krocích. Pokud chcete odkázat, aktualizujte připojovací řetězec v souboru app.config konzolové aplikace s připojovacím řetězcem v souboru web.config webové aplikace. Také poskytují úplnou fyzickou cestu ve vlastnosti "AttachDbFilename".
6. Otevřete příkazový řádek a přejděte do složky bin výše konzolové aplikace. Spusťte spustitelný soubor a prohlédněte si výstup protokolu, jak je znázorněno na následujícím obrázku.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.jpg)
7. Tabulku "AspNetUsers" Otevřít v Průzkumníku serveru a ověřit data v nové sloupce, které obsahují vlastnosti. Je třeba aktualizovat odpovídající hodnoty vlastnosti.

## <a name="verify-functionality"></a>Ověřte funkčnost

Použijte členství v nově přidaném stránky, které jsou implementovány pomocí technologie ASP.NET Identity přihlášení uživatele z původní databáze. Uživatel by měl být moct přihlásit pomocí stejných přihlašovacích údajů. Zkuste jiné funkce, jako je přidání OAuth, vytváření nového uživatele, změna hesel, přidání rolí, přidání uživatelů do rolí, atd.

Data profilu pro uživatele staré a nové uživatele by měla načíst a uložená v tabulce uživatelů. Původní tabulka už být odkazována.

## <a name="conclusion"></a>Závěr

Tento článek popisuje proces migrace webových aplikací, které používají model zprostředkovatele pro členství v ASP.NET Identity. Článek dále uvedených migrace data profilu pro uživatele využívat do systému identit. Napište prosím komentáře níže pro dotazy a problémů při migraci vaší aplikace.

*Děkujeme, že Rick Anderson a Robert McMurray revize článku.*
