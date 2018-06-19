---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: Přidání ASP.NET Identity pro prázdný nebo existující webových formulářů projekt | Microsoft Docs
author: raquelsa
description: V tomto kurzu se dozvíte, jak přidat do aplikace ASP.NET ASP.NET Identity (nový systém členství pro technologii ASP.NET). Při vytváření nové webové formuláře nebo MVC...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/23/2013
ms.topic: article
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: 8961e596f0d6cc4810e2439be1ec2915bddb8c78
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874652"
---
<a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a>Přidání ASP.NET Identity pro prázdný nebo existující webových formulářů projektu
====================
podle [Raquel Soares De Almeida](https://github.com/raquelsa)

> V tomto kurzu se dozvíte, jak přidat [ASP.NET Identity](introduction-to-aspnet-identity.md) (nový systém členství pro technologii ASP.NET) pro aplikace ASP.NET.
> 
> Když vytvoříte nový projekt webové formuláře nebo MVC v Visual Studio 2013 RTM s jednotlivých účtů, Visual Studio se nainstalujte všechny požadované balíčky a přidat všechny potřebné třídy pro vás. V tomto kurzu bude znázorňují postup pro přidání podpory ASP.NET Identity existující projekt webové formuláře nebo nový prázdný projekt. I se popisují všechny balíčky NuGet, které je třeba nainstalovat a třídy, je nutné přidat. Pro nové uživatele registrace a přihlášení při zvýraznění všechna rozhraní API hlavní vstupní bod pro správu uživatelů a ověřování budou přejít přes ukázkové webové formuláře. Tato ukázka použije výchozí implementace ASP.NET Identity pro úložiště dat SQL, který je založený na rozhraní Entity Framework. V tomto kurzu budeme používat LocalDB pro databázi SQL.
> 
> V tomto kurzu byla zapsána Raquel Soares De Almeida a Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="getting-started-aspnet-identity"></a>Začínáme s ASP.NET Identity

1. Začněte tím, že instalace a spuštění [Visual Studio Express 2013 pro Web](https://go.microsoft.com/fwlink/?LinkId=299058) nebo [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).
2. Klikněte na tlačítko **nový projekt** od začátku stránky, nebo můžete použít nabídku a vyberte **soubor**a potom **nový projekt**.
3. Vyberte **Visual C# i** n v levém podokně, pak **webové** a pak vyberte **webové aplikace ASP.NET**. Název projektu "WebFormsIdentity" a pak klikněte na **OK**.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image1.png)
4. V **nový projekt ASP.NET** dialogovém okně, vyberte **prázdný** šablony.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
   Upozornění **změna ověřování** tlačítko je zakázané a žádné ověřování podpora je k dispozici v této šabloně. Šablony webových formulářů, MVC a webového rozhraní API umožňují vyberte metodu ověřování. Další informace najdete v tématu [Přehled ověřování](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth) .

## <a name="adding-identity-packages-to-your-app"></a>Přidání Identity balíčky do vaší aplikace

V Průzkumníku řešení klikněte pravým tlačítkem na projekt a vyberte **spravovat balíčky NuGet**. V dialogovém okně hledání textového pole zadejte "*Identity.E*". Kliknutím na tlačítko Instalovat pro tento balíček.   
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image3.png)  
  
Všimněte si, že tento balíček nainstaluje balíčky závislost: EntityFramework a Microsoft ASP.NET Identity Core.

## <a name="adding-web-forms-to-register-users"></a>Přidání webové formuláře k registraci uživatelů

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a klikněte na **přidat**a potom **webového formuláře**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. V **zadejte název položky** dialogové okno, název nového webového formuláře **zaregistrovat**a pak klikněte na tlačítko **OK**
3. Nahraďte kód v vygenerovaného *Register.aspx* souboru pomocí kódu níže. Změny kódu jsou vyznačené.   

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > Toto je právě zjednodušené verzi *Register.aspx* soubor, který se vytvoří při vytvoření nového projektu webových formulářů ASP.NET. Výše uvedený kód přidá pole formuláře a tlačítko k registraci nového uživatele.
4. Otevřete *Register.aspx.cs* a obsah souboru nahraďte následujícím kódem:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. Výše uvedený kód je zjednodušenou verzi *Register.aspx.cs* soubor, který se vytvoří při vytvoření nového projektu webových formulářů ASP.NET.
    > 2. *IdentityUser* třída je výchozí implementace objektu EntityFramework *IUser* rozhraní. *IUser* rozhraní je minimální rozhraní pro uživatele na ASP.NET Identity Core.
    > 3. *Objektu UserStore* třída je výchozí implementace objektu EntityFramework úložiště uživatele. Tato třída implementuje minimální rozhraní ASP.NET Identity Core: *IUserStore*, *IUserLoginStore*, *IUserClaimStore* a *IUserRoleStore* .
    > 4. *Objekt UserManager* rozhraní API, které automaticky uloží změny související s uživateli zpřístupňuje – třída *objektu UserStore*.
    > 5. *IdentityResult* třída reprezentuje výsledek operace identity.
5. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a klikněte na **přidat**, **přidat složku ASP.NET** a potom **aplikace\_Data**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. Otevřete *Web.config* souboru a přidejte připojovací řetězec pro databázi použijeme uložit informace o uživateli. Databázi vytvoří za běhu pomocí objektu EntityFramework pro Identity entity. Připojovací řetězec je podobná vytvořené pro vás, když vytvoříte nový projekt webové formuláře. Zvýrazněný kód ukazuje značky, že měli byste přidat:

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > Pro Visual Studio 2015 nebo vyšší, nahraďte `(localdb)\v11.0` s `(localdb)\MSSQLLocalDB` v připojovacím řetězci.
    
7. Klikněte pravým tlačítkem na soubor *Register.aspx* v projektu a vyberte **nastavit jako úvodní stránku**. Stisknutím kombinace kláves Ctrl + F5 sestavení a spuštění webové aplikace. Zadejte nové uživatelské jméno a heslo a potom klikněte na **zaregistrovat**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > ASP.NET Identity má podporu pro ověření a v této ukázce můžete ověřit, výchozí chování na uživatele a heslo validátory, které pocházejí z Identity základní balíček. Výchozí validátor pro uživatele (`UserValidator`) má vlastnost `AllowOnlyAlphanumericUserNames` , nemá výchozí hodnotu nastavit na `true`. Výchozí validátor pro heslo (`MinimumLengthValidator`) zajišťuje, že heslo má alespoň 6 znaků. Tyto validátory jsou vlastnosti na `UserManager` , je možné přepsat, pokud chcete mít vlastního ověřování

## <a name="verifying-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a>Ověření LocalDb Identity databáze a tabulky generované rozhraní Entity Framework

1. V **zobrazení** nabídky, klikněte na tlačítko **Průzkumníka serveru**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. Rozbalte položku **objekt DefaultConnection (WebFormsIdentity)**, rozbalte položku **tabulky**, klikněte pravým tlačítkem na **AspNetUsers** a klikněte na tlačítko **zobrazit Data tabulky**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configuring-the-application-for-owin-authentication"></a>Konfigurace aplikace pro ověřování OWIN.

V tuto chvíli jsme doplnili jenom podporu pro vytváření uživatelů. Teď přidáme ukazují, jak jsme přidat ověřování se přihlášení uživatele. ASP.NET Identity používá Microsoft OWIN ověřovací middleware pro ověřování pomocí formulářů. Soubor cookie ověřování souborů Cookie OWIN a deklarace identity ověřování založené na mechanismu, který může používat libovolnou architekturu hostované na [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) nebo služby IIS. V tomto modelu lze použít stejné balíčky ověřování napříč více rozhraní, včetně ASP.NET MVC a webových formulářů. Další informace o projektu Katana a jak spustit middlewaru v lhostejné najdete v tématu hostitele [Začínáme s projektem Katana](https://msdn.microsoft.com/magazine/dn451439.aspx).

## <a name="installing-authentication-packages-to-your-application"></a>Instalace balíčků ověřování do aplikace

1. V Průzkumníku řešení klikněte pravým tlačítkem na projekt a vyberte **spravovat balíčky NuGet**. V dialogovém okně hledání textového pole zadejte "*Identity.Owin*". Kliknutím na tlačítko Instalovat pro tento balíček.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image10.png)
2. Vyhledejte balíček ***Microsoft.Owin.Host.SystemWeb*** a nainstalujte ji.   

    > [!NOTE]
    > **Microsoft.Aspnet.Identity.Owin** balíček obsahuje sadu OWIN rozšíření třídy pro správu a konfiguraci middleware ověřování OWIN, který se má používat balíčky ASP.NET Identity Core.  
    > **Microsoft.Owin.Host.SystemWeb** balíček obsahuje serveru OWIN, která umožňuje aplikacím na základě OWIN běží ve službě IIS pomocí kanál požadavku. Další informace najdete v části [integrované kanálu OWIN Middleware v IIS](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md).

## <a name="adding-owin-startup-and-authentication-configuration-classes"></a>Přidání OWIN při spuštění a třídy konfigurace ověřování

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt, klikněte na **přidat**a potom **přidat novou položku**. V dialogovém okně hledání textového pole zadejte "*owin*". Název třídy "*spuštění*" a klikněte na tlačítko **přidat**.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. V souboru Startup.cs přidáte zvýrazněný uvedené níže, aby konfigurace ověřování souborů cookie OWIN.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > Tato třída obsahuje `OwinStartup` atribut pro zadání třídy pro spuštění OWIN. Každá aplikace OWIN obsahuje třídu, spuštění, kde můžete určit komponenty pro kanálu aplikace. V tématu [OWIN při spuštění třída detekce](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) Další informace o tomto modelu.

## <a name="adding-web-forms-for-registering-and-logging-in-users"></a>Přidání webové formuláře pro registraci a přihlašování uživatelů

1. Otevřete *Register.cs* souboru a přidejte následující kód, který bude přihlášení uživatele, pokud registrace úspěšná. Změny se zvýrazněnou níže.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - Vzhledem k tomu, že jsou deklarace identity na základě systému ASP.NET Identity a ověřování souborů Cookie OWIN, vyžaduje rozhraní vývojáři aplikace k vygenerování [ClaimsIdentity](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.claimsidentity.aspx) pro uživatele. ClaimsIdentity obsahuje informace o všech deklarací identity pro uživatele, například jaké role uživatel patří. V této fázi můžete také přidat další deklarace pro uživatele.
    > - Uživatel může přihlásit pomocí třídě z OWIN a volání `SignIn` a předávání v ClaimsIdentity, jak je uvedeno výše. Tento kód se přihlásit uživatele a vygenerujte soubor cookie také. Toto volání se podobá [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) používané [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modulu.
2. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši klikněte na váš projekt **přidat**a potom **webového formuláře**. Název webového formuláře **přihlášení**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. Nahraďte obsah *Login.aspx* soubor s následujícím kódem:  

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. Nahraďte obsah *Login.aspx.cs* soubor s následující:  

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - `Page_Load` Nyní kontroluje stav aktuálního uživatele a provede akci na základě jeho `Context.User.Identity.IsAuthenticated` stavu.  
    >     **Zobrazit protokolováno v uživatelské jméno** : Microsoft ASP.NET Identity Framework přidala rozšiřující metody na [System.Security.Principal.IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx) který umožňuje získat `UserName` a `UserId` pro přihlášeného uživatele. Tyto rozšiřující metody jsou definovány v `Microsoft.AspNet.Identity.Core` sestavení. Tyto rozšiřující metody jsou náhradou [HttpContext.User.Identity.Name](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx) .
    > - Metoda přihlášení:   
    >     `This` Metoda nahrazuje předchozí `CreateUser_Click` metoda v této ukázkové a nyní přihlásí uživatel po úspěšném vytvoření uživatele.   
    >  Rozhraní Microsoft OWIN přidala rozšiřující metody na `System.Web.HttpContext` , které umožňuje získat odkaz na `IOwinContext`. Tyto rozšiřující metody jsou definovány v `Microsoft.Owin.Host.SystemWeb` sestavení. `OwinContext` Třídy zpřístupňuje `IAuthenticationManager` vlastnost, která představuje funkce middlewaru ověřování dostupné u aktuálního požadavku.  
    >  Uživatel může přihlásit pomocí `AuthenticationManager` z OWIN a volání `SignIn` a předejte `ClaimsIdentity` jako v příkladu nahoře.   
    >  Protože ASP.NET Identity a ověřování souborů Cookie OWIN jsou založené na deklaracích identity systému rozhraní framework vyžaduje, aby aplikace ke generování `ClaimsIdentity` pro uživatele.   
    >  `ClaimsIdentity` Nemá informace o všech deklarací identity pro uživatele, například jaké role uživatel patří. Můžete také přidat další deklarace pro uživatele v této fázi  
    >  Tento kód se přihlásit uživatele a vygenerujte soubor cookie také. Toto volání se podobá [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) používané [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modulu.
    > - `SignOut` Metoda:   
    >  Získá odkaz na `AuthenticationManager` z OWIN a volání `SignOut`. Toto je obdobou [FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) metodu používanou [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modulu.
5. Stiskněte klávesu **kombinaci kláves Ctrl + F5** sestavení a spuštění webové aplikace. Zadejte nové uživatelské jméno a heslo a potom klikněte na **zaregistrovat**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
   Poznámka: V tomto okamžiku nového uživatele je vytvořen a přihlášení.
6. Klikněte na **odhlášení** tlačítko. Budete přesměrováni do protokolu v podobě.
7. Zadejte neplatné uživatelské jméno nebo heslo a klikněte na tlačítko **přihlásit** tlačítko.   
   `UserManager.Find` Metoda vrátí hodnotu null a chybová zpráva: " *neplatné uživatelské jméno nebo heslo* " se zobrazí.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)
