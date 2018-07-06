---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: Přidání ASP.NET Identity do prázdného nebo stávajícího Web Forms projektu | Dokumentace Microsoftu
author: raquelsa
description: V tomto kurzu se dozvíte, jak přidat do aplikace ASP.NET ASP.NET Identity (nový systém členství technologie ASP.NET). Při vytváření nového webového formuláře nebo MVC...
ms.author: aspnetcontent
ms.date: 10/23/2013
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: 1e7508fc2431f4e1e3c4509fbe705daf42686e8d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832659"
---
<a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a>Přidání ASP.NET Identity do prázdného nebo stávajícího Web Forms projektu
====================
podle [Raquel Soares De Almeida](https://github.com/raquelsa)

> V tomto kurzu se dozvíte, jak přidat [ASP.NET Identity](introduction-to-aspnet-identity.md) (nový systém členství technologie ASP.NET) pro aplikace ASP.NET.
> 
> Když vytvoříte nový projekt webové formuláře nebo MVC v sadě Visual Studio 2013 RTM individuálními účty, Visual Studio nainstalujte požadované balíčky a přidat všechny potřebné třídy za vás. Tento kurz popisuje postup přidání podpory ASP.NET Identity do existujícího projektu webových formulářů nebo nový prázdný projekt. Můžu se popisují všechny balíčky, které je potřeba nainstalovat a třídy, které je třeba přidat. Přejdu bude přes ukázkové webové formuláře pro registraci nových uživatelů a přihlášení při zvýraznění všechna rozhraní API hlavní vstupní bod pro správu uživatelů a ověřování. Tento příklad použije výchozí implementace ASP.NET Identity pro úložiště dat SQL, které je postavené na rozhraní Entity Framework. Tomto kurzu budeme používat LocalDB pro službu SQL database.
> 
> V tomto kurzu zapsal Raquel Soares De Almeida a Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="getting-started-aspnet-identity"></a>Začínáme s ASP.NET Identity

1. Začněte tím, že instalaci a používání [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) nebo [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).
2. Klikněte na tlačítko **nový projekt** od samého začátku stránky, nebo můžete použít v nabídce a vyberte **souboru**a potom **nový projekt**.
3. Vyberte **Visual C# i** n levém podokně, pak **webové** a pak vyberte **webová aplikace ASP.NET**. Pojmenujte svůj projekt "WebFormsIdentity" a potom klikněte na tlačítko **OK**.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image1.png)
4. V **nový projekt ASP.NET** dialogového okna, vyberte **prázdný** šablony.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
   Všimněte si, že **změna ověřování** je tlačítko neaktivní a nenabízí žádnou podporu ověřování v této šabloně. Šablony webových formulářů, MVC a webového rozhraní API umožňuje zvolit přístup ověřování. Další informace najdete v tématu [Přehled ověřování](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth) .

## <a name="adding-identity-packages-to-your-app"></a>Přidávají se balíčky Identity do aplikace

V Průzkumníku řešení klikněte pravým tlačítkem myši na projekt a vyberte **spravovat balíčky NuGet**. V dialogovém okně hledání textového pole zadejte "*Identity.E*". Kliknutím na tlačítko Instalovat pro tento balíček.   
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image3.png)  
  
Všimněte si, že tento balíček nainstaluje balíčky závislostí: EntityFramework a Microsoft, ASP.NET Core Identity.

## <a name="adding-web-forms-to-register-users"></a>Přidání webových formulářů k registraci uživatelů

1. V **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt a klikněte na tlačítko **přidat**a potom **webový formulář**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. V **zadat název pro položku** dialogové okno, název nového webového formuláře **zaregistrovat**a potom klikněte na tlačítko **OK**
3. Nahraďte kód v generované *Register.aspx* soubor s následujícím kódem. Změny kódu jsou zvýrazněné.   

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > Toto je jenom zjednodušenou verzi *Register.aspx* soubor, který se vytvoří při vytvoření nového projektu ASP.NET webové formuláře. Výše uvedené značek přidá pole formuláře a tlačítko k registraci nového uživatele.
4. Otevřít *Register.aspx.cs* souboru a nahraďte jeho obsah souboru následujícím kódem:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. Výše uvedený kód je zjednodušenou verzi *Register.aspx.cs* soubor, který se vytvoří při vytvoření nového projektu ASP.NET webové formuláře.
    > 2. *IdentityUser* třída je výchozí implementace objektu EntityFramework *IUser* rozhraní. *IUser* rozhraní je minimální rozhraní pro uživatele v ASP.NET Identity Core.
    > 3. *Objektu UserStore* třída je výchozí implementace objektu EntityFramework úložiště uživatele. Tato třída implementuje minimální rozhraní ASP.NET Identity Core: *úložiště IUserStore*, *IUserLoginStore*, *IUserClaimStore* a *IUserRoleStore* .
    > 4. *Objektu UserManager* rozhraní API, které automaticky uloží změny související s uživateli zpřístupní třídu *objektu UserStore*.
    > 5. *IdentityResult* třída reprezentuje výsledek operace identity.
5. V **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt a klikněte na tlačítko **přidat**, **přidat složku ASP.NET** a potom **aplikace\_Data**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. Otevřít *Web.config* a přidejte připojovací řetězec pro databázi, budeme používat k ukládání informací o uživateli. Databáze se vytvoří za běhu pomocí objektu EntityFramework pro entity, které Identity. Připojovací řetězec je podobně jako když vytvoříte nový projekt webových formulářů vytvořené za vás. Zvýrazněný kód ukazuje značky, že měli byste přidat:

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > Pro Visual Studio 2015 nebo vyšší, nahraďte `(localdb)\v11.0` s `(localdb)\MSSQLLocalDB` v připojovacím řetězci.
    
7. Klikněte pravým tlačítkem na soubor *Register.aspx* v projektu a vyberte **nastavit jako úvodní stránku**. Stisknutím kláves Ctrl + F5 sestavte a spusťte webovou aplikaci. Zadejte nové uživatelské jméno a heslo a potom klikněte na **zaregistrovat**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > ASP.NET Identity obsahuje podporu pro ověření a v této ukázce můžete zkontrolovat výchozí chování na uživatele a heslo validátory, které pocházejí z balíčku Identity Core. Výchozí validátor pro uživatele (`UserValidator`) má vlastnost `AllowOnlyAlphanumericUserNames` , který má výchozí hodnotu nastavte na `true`. Výchozí validátor pro heslo (`MinimumLengthValidator`) zajišťuje, že heslo obsahuje alespoň 6 znaků. Tyto validátory jsou vlastnosti na `UserManager` , který lze přepsat, pokud chcete mít vlastní ověřování

## <a name="verifying-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a>Ověření LocalDb Identity databáze a tabulky vygenerovaným rozhraním Entity Framework

1. V **zobrazení** nabídky, klikněte na tlačítko **Průzkumníka serveru**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. Rozbalte **objekt DefaultConnection (WebFormsIdentity)**, rozbalte **tabulky**, klikněte pravým tlačítkem myši **AspNetUsers** a klikněte na tlačítko **zobrazit Data tabulky**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configuring-the-application-for-owin-authentication"></a>Konfigurace aplikace pro ověřování OWIN.

V tomto okamžiku pouze přidali jsme podporu pro vytváření uživatelů. Nyní se budeme ukazují, jak můžeme přidat ověřování k přihlášení uživatele. ASP.NET Identity používá Microsoft OWIN ověřovací middleware pro ověřování pomocí formulářů. Ověřování souborů Cookie OWIN soubor cookie a deklarace identity na základě ověřovací mechanismus, který je možné pomocí libovolné architektury hostitelem [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) nebo služby IIS. V tomto modelu stejné ověřování balíčky je možné napříč více platforem, včetně ASP.NET MVC a webového formuláře. Další informace o projektu Katana a spuštění middlewaru v nezávislá viz hostitele [Začínáme se službou projektu Katana](https://msdn.microsoft.com/magazine/dn451439.aspx).

## <a name="installing-authentication-packages-to-your-application"></a>Instalace balíčků ověřování do aplikace

1. V Průzkumníku řešení klikněte pravým tlačítkem myši na projekt a vyberte **spravovat balíčky NuGet**. V dialogovém okně hledání textového pole zadejte "*Identity.Owin*". Kliknutím na tlačítko Instalovat pro tento balíček.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image10.png)
2. Vyhledejte balíček ***Microsoft.Owin.Host.SystemWeb*** a nainstalujte ho.   

    > [!NOTE]
    > **Microsoft.Aspnet.Identity.Owin** balíček obsahuje sadu rozšíření třídy OWIN pro správu a konfiguraci middleware ověřování OWIN, který se má používat balíčky, ASP.NET Identity Core.  
    > **Microsoft.Owin.Host.SystemWeb** balíček obsahuje serveru OWIN, který umožňuje aplikacím na základě OWIN pro spuštění ve službě IIS pomocí kanálu požadavku ASP.NET. Další informace najdete v části [Middleware OWIN v IIS integrovaný kanál](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md).

## <a name="adding-owin-startup-and-authentication-configuration-classes"></a>Přidání ověřování konfigurace třídy a OWIN Startup

1. V **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt, klikněte na tlačítko **přidat**a potom **přidat novou položku**. V dialogovém okně hledání textového pole zadejte "*owin*". Název třídy "*spuštění*" a klikněte na tlačítko **přidat**.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. V souboru Startup.cs přidejte zvýrazněný kód níže ke konfiguraci ověřování souborů cookie OWIN.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > Tato třída obsahuje `OwinStartup` atributu pro určení třídy pro spuštění OWIN. Každá aplikace OWIN obsahuje třídu spuštění zadávat komponenty pro kanál aplikací. Zobrazit [rozpoznání spouštěcí třídy OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) Další informace o tomto modelu.

## <a name="adding-web-forms-for-registering-and-logging-in-users"></a>Přidání webového formuláře pro registraci a přihlašování uživatelů

1. Otevřít *Register.cs* a přidejte následující kód, který se přihlásit uživatele po úspěšné registraci. Změny jsou jejichž přehled najdete níže.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - Protože ASP.NET Identity a ověřování souborů Cookie OWIN systému na základě deklarací identity, rozhraní framework vyžaduje pro vývojáře aplikací pro generování [ClaimsIdentity](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.claimsidentity.aspx) pro daného uživatele. ClaimsIdentity obsahuje informace o všech deklarací identity pro uživatele, jako je například kterým rolím uživatel patří. Můžete také přidat další deklarace identity pro uživatele v této fázi.
    > - Můžete přihlásit uživatele pomocí Správce AuthenticationManager z OWIN a volání `SignIn` a předávání ClaimsIdentity, jak je znázorněno výše. Tento kód se přihlásit uživatele a generovat také do souboru cookie. Toto volání je obdobou [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) používané [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modulu.
2. V **Průzkumníka řešení**, klikněte pravým tlačítkem na váš projekt kliknutím **přidat**a potom **webový formulář**. Název webového formuláře **přihlášení**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. Nahraďte obsah *Login.aspx* souboru následujícím kódem:  

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. Nahraďte obsah *Login.aspx.cs* souboru následujícím kódem:  

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - `Page_Load` Teď kontroluje stav aktuálního uživatele a provede akci závislou na jeho `Context.User.Identity.IsAuthenticated` stav.  
    >     **Zobrazit protokolováno v uživatelské jméno** : rozhraní Framework identitu Microsoft ASP.NET přidala na metody rozšíření [System.Security.Principal.IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx) , který umožňuje získat `UserName` a `UserId` pro přihlášený uživatel. Tyto rozšiřující metody jsou definovány v `Microsoft.AspNet.Identity.Core` sestavení. Tyto rozšiřující metody jsou náhrada za [HttpContext.User.Identity.Name](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx) .
    > - Metoda přihlášení:   
    >     `This` Metoda nahrazuje předchozí `CreateUser_Click` metody v této ukázkové a nyní přihlásí uživatele po úspěšném vytvoření uživatele.   
    >  Rozhraní Microsoft OWIN přidal rozšiřující metody na `System.Web.HttpContext` , který umožňuje získat odkaz na `IOwinContext`. Tyto rozšiřující metody jsou definovány v `Microsoft.Owin.Host.SystemWeb` sestavení. `OwinContext` Třídy zpřístupňuje `IAuthenticationManager` vlastnost, která představuje funkce middlewaru ověřování dostupné u aktuálního požadavku.  
    >  Uživatel může přihlásit pomocí `AuthenticationManager` z OWIN a volání `SignIn` a předejte `ClaimsIdentity` jak je znázorněno výše.   
    >  Protože ASP.NET Identity a ověřování souborů Cookie OWIN jsou založené na deklaracích systém, rozhraní framework vyžaduje, aby aplikace k vygenerování `ClaimsIdentity` pro daného uživatele.   
    >  `ClaimsIdentity` Nemá informace o všech deklarací identity pro uživatele, například ke kterým rolím uživatel patří. Můžete také přidat další deklarace identity pro uživatele v této fázi  
    >  Tento kód se přihlásit uživatele a generovat také do souboru cookie. Toto volání je obdobou [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) používané [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modulu.
    > - `SignOut` Metoda:   
    >  Získá odkaz na `AuthenticationManager` z OWIN a volání `SignOut`. To je obdobou [FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) metodu používanou [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modulu.
5. Stisknutím klávesy **Ctrl + F5** sestavíte a spustíte webovou aplikaci. Zadejte nové uživatelské jméno a heslo a potom klikněte na **zaregistrovat**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
   Poznámka: V tomto okamžiku nového uživatele je vytvořen a přihlášení.
6. Klikněte na **Odhlásit** tlačítko. Budete přesměrováni na přihlašovací formulář.
7. Zadejte neplatné uživatelské jméno nebo heslo a klikněte na **přihlášení** tlačítko.   
   `UserManager.Find` Metoda vrátí hodnotu null a chybová zpráva: " *neplatné uživatelské jméno nebo heslo* " se zobrazí.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)
