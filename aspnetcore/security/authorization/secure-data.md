---
title: "Vytvoření aplikace ASP.NET Core s uživatelskými daty chráněn autorizace"
author: rick-anderson
description: "Postup vytvoření aplikace pro stránky Razor s uživatelskými daty chráněn autorizace. Zahrnuje protokol HTTPS, ověřování, zabezpečení, ASP.NET Core Identity."
manager: wpickett
ms.author: riande
ms.date: 01/24/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/secure-data
ms.openlocfilehash: 5acb65be078fd39b9e7a17ce2d8167b8f7b7db22
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>Vytvoření aplikace ASP.NET Core s uživatelskými daty chráněn autorizace

Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Audette Jan](https://twitter.com/joeaudette)

Tento kurz ukazuje postup vytvoření webové aplikace ASP.NET Core s uživatelskými daty chráněn autorizace. Zobrazí seznam kontakty, které ověřené uživatele (registrovaný) jste vytvořili. Existují tři skupiny zabezpečení:

* **Registrované uživatele** můžete zobrazit všechny schválené data a můžete upravit nebo odstranit svá vlastní data.
* **Správci** můžete schválit nebo odmítnout kontaktní údaje. Pouze schválené kontakty jsou viditelné pro uživatele.
* **Správci** můžete schválit nebo odmítnout a upravit nebo odstranit všechna data.

Na následujícím obrázku, uživatel Rick (`rick@example.com`) je přihlášený. Rick lze zobrazit pouze schválené kontakty a **upravit**/**odstranit**/**vytvořit nový** odkazy pro jeho kontakty. Pouze poslední záznam vytvořené Rick, zobrazí **upravit** a **odstranit** odkazy. Ostatní uživatelé neuvidí poslední záznam tak, aby manažer nebo správce se změnil stav do "Schváleno".

![Obrázek popsané předcházející](secure-data/_static/rick.png)

Na následujícím obrázku `manager@contoso.com` je přihlášen a v roli správce:

![Obrázek popsané předcházející](secure-data/_static/manager1.png)

Následující obrázek znázorňuje vybraných manažerů zobrazení podrobností kontaktu:

![Obrázek popsané předcházející](secure-data/_static/manager.png)

**Schválit** a **odmítnout** tlačítka se zobrazí pouze správci a správci.

Na následujícím obrázku `admin@contoso.com` je přihlášen a v roli správce:

![Obrázek popsané předcházející](secure-data/_static/admin.png)

Správce má všechna oprávnění. Jana můžete pro čtení, úpravy nebo odstranění všechny kontakty a změnit stav kontaktů.

Aplikace byla vytvořená [generování uživatelského rozhraní](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) následující `Contact` modelu:

[!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

Ukázka obsahuje následující rutiny autorizace:

* `ContactIsOwnerAuthorizationHandler`: Zajistí uživatele můžete upravit pouze svá data.
* `ContactManagerAuthorizationHandler`: Umožňuje správcům schválit nebo odmítnout kontakty.
* `ContactAdministratorsAuthorizationHandler`: Umožňuje správcům schválit nebo odmítnout kontakty a upravit nebo odstranit kontakty.

## <a name="prerequisites"></a>Požadavky

V tomto kurzu je rozšířený. Měli byste se seznámit s:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Ověřování](xref:security/authentication/index)
* [Potvrzení účtu a obnovení hesla](xref:security/authentication/accconfirm)
* [Autorizace](xref:security/authorization/index)
* [Entity Framework Core](xref:data/ef-mvc/intro)

V tématu [tento PDF soubor](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) pro ASP.NET MVC základní verzi. Verze 1.1 jádro ASP.NET v tomto kurzu je v [to](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) složky. 1.1 ASP.NET Core ukázka je v [ukázky](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).

## <a name="the-starter-and-completed-app"></a>Spuštění a dokončené aplikace

[Stáhněte si](xref:tutorials/index#how-to-download-a-sample) [Dokončit](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) aplikace. [Test](#test-the-completed-app) dokončené aplikace, takže seznámit se s jeho funkce zabezpečení.

### <a name="the-starter-app"></a>Úvodní aplikaci

[Stáhněte si](xref:tutorials/index#how-to-download-a-sample) [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) aplikace.

Spusťte aplikaci, klepněte **ContactManager** propojit a ověřte, můžete vytvořit, upravit a odstranit a obraťte se na.

## <a name="secure-user-data"></a>Zabezpečení dat uživatele

V následujících částech mít všechny hlavní kroky k vytvoření aplikace dat zabezpečení uživatele. Vám může být užitečné k odkazování na dokončený projekt.

### <a name="tie-the-contact-data-to-the-user"></a>Tie – kontaktních údajů pro uživatele

Pomocí technologie ASP.NET [Identity](xref:security/authentication/identity) ID uživatele, aby uživatelé můžete upravit svá data, ale ne další data uživatele. Přidat `OwnerID` a `ContactStatus` k `Contact` modelu:

[!code-csharp[](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

`OwnerID` ID uživatele z `AspNetUser` tabulky v [Identity](xref:security/authentication/identity) databáze. `Status` Pole určuje, zda kontakt zobrazit obecné uživatele.

Vytvořte nové migrace a aktualizaci databáze:

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-https-and-authenticated-users"></a>Vyžadovat protokol HTTPS a ověření uživatelé

Přidat [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) k `Startup`:

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_env)]

V `ConfigureServices` metodu *Startup.cs* soubor, přidejte [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) filtr autorizace:

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=10-999)]

Pokud používáte Visual Studio, povolte protokol HTTPS.

Přesměrování požadavků HTTP do HTTPS, najdete v části [URL přepisování Middleware](xref:fundamentals/url-rewriting). Pokud jste pomocí Visual Studio Code nebo testování na místní platformu, která neobsahuje testovací certifikát pro protokol HTTPS:

  Nastavit `"LocalTest:skipSSL": true` v *appsettings. Developement.JSON* souboru.

### <a name="require-authenticated-users"></a>Vyžadovat ověření uživatelé

Nastavte výchozí zásady ověřování tak, aby vyžadovala uživatele k ověření. Můžete vyjádření výslovného nesouhlasu ověřování na úrovni stránky Razor, kontroler nebo akce metoda s `[AllowAnonymous]` atribut. Nastavení výchozích zásad ověřování budou muset uživatelé ověřit chrání nově přidané stránky Razor a řadiče. Má ve výchozím nastavení je vyžadováno ověření je bezpečnější než spoléhat na nových řadičů a stránky Razor zahrnout `[Authorize]` atribut. 

Tento požadavek ověřen, všichni uživatelé [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) a [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage?view=aspnetcore-2.0) volání nejsou potřeba.

Aktualizace `ConfigureServices` s následujícími změnami:

* Komentář `AuthorizeFolder` a `AuthorizePage`.
* Nastavte výchozí zásady ověřování tak, aby vyžadovala uživatele k ověření.

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=23-27,31-999)]

Přidat [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) do indexu, o a kontaktní stránky, mohou anonymní uživatelé získat informace o lokalitě, před jejich registraci. 

[!code-csharp[](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

Přidat `[AllowAnonymous]` k [LoginModel a RegisterModel](https://github.com/aspnet/templating/issues/238).

### <a name="configure-the-test-account"></a>Nakonfigurujte účet pro test

`SeedData` Třída vytvoří dva účty: správce a správce. Použití [nástroj tajný klíč správce](xref:security/app-secrets) nastavení hesla pro tyto účty. Nastavení hesla z adresáře projektu (adresář obsahující *Program.cs*):

```console
dotnet user-secrets set SeedUserPW <PW>
```

Aktualizace `Main` používat heslo testu:

[!code-csharp[](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>Vytvoření testovacích účtů a aktualizovat kontaktů

Aktualizace `Initialize` metoda v `SeedData` třídy za účelem vytvoření testovacích účtů:

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

Přidat ID uživatele správce a `ContactStatus` kontaktům. Si ho kontaktů "Odesláno" a jedné "Zamítnutá". Přidáte ID uživatele a stav pro všechny kontakty. Je zobrazena pouze jeden kontakt:

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Vytvoření vlastníka, správce a Správce autorizací obslužné rutiny

Vytvoření `ContactIsOwnerAuthorizationHandler` třídy v *autorizace* složky. `ContactIsOwnerAuthorizationHandler` Ověřuje, že uživatel, který funguje na prostředku vlastníkem prostředku.

[!code-csharp[](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

`ContactIsOwnerAuthorizationHandler` Volání [kontextu. Úspěšné](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) Pokud ověřený aktuální uživatel je jeho vlastníkem. Obslužné rutiny autorizace obecně:

* Vrátí `context.Succeed` Pokud jsou splněny požadavky na.
* Vrátí `Task.CompletedTask` Pokud nejsou splněné požadavky. `Task.CompletedTask` je ani úspěch nebo neúspěch&mdash;umožňuje ostatních obslužných rutin autorizaci ke spuštění.

Pokud potřebujete explicitně nezdaří, vrátí [kontextu. Selhání](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).

Aplikace umožňuje kontaktní vlastníky úpravy, odstranění nebo vytvořit svá vlastní data. `ContactIsOwnerAuthorizationHandler` není potřeba zkontrolovat operaci předán v parametru požadavek.

### <a name="create-a-manager-authorization-handler"></a>Vytvořte obslužnou rutinu Správce autorizací

Vytvoření `ContactManagerAuthorizationHandler` třídy v *autorizace* složky. `ContactManagerAuthorizationHandler` Ověřuje uživatele, který funguje na prostředku je správce. Pouze správci můžete schválit nebo odmítnout změny obsahu (nové nebo změněné).

[!code-csharp[](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Vytvoření obslužné rutiny Správce autorizací

Vytvoření `ContactAdministratorsAuthorizationHandler` třídy v *autorizace* složky. `ContactAdministratorsAuthorizationHandler` Ověřuje uživatele, který funguje na prostředku je správce. Správce může provádět všechny operace.

[!code-csharp[](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Registraci obslužných rutin autorizace

Musí být pro registrované služby pomocí Entity Framework Core [vkládání závislostí](xref:fundamentals/dependency-injection) pomocí [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). `ContactIsOwnerAuthorizationHandler` Používá ASP.NET Core [Identity](xref:security/authentication/identity), který je založený na Entity Framework Core. Registraci obslužných rutin s kolekcí služby, takže jsou k dispozici na `ContactsController` prostřednictvím [vkládání závislostí](xref:fundamentals/dependency-injection). Přidejte následující kód do konce `ConfigureServices`:

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-999)]

`ContactAdministratorsAuthorizationHandler` a `ContactManagerAuthorizationHandler` jsou přidány jako jednotlivé prvky. Jsou jednotlivé prvky, protože se nepoužívají EF a veškeré informace potřebné v `Context` parametr `HandleRequirementAsync` metoda.

## <a name="support-authorization"></a>Povolení podpory

V této části se aktualizace stránky Razor a přidejte třídu operations požadavky.

### <a name="review-the-contact-operations-requirements-class"></a>Zkontrolujte požadavky na třídy kontaktní operace

Zkontrolujte `ContactOperations` třídy. Tato třída obsahuje požadavky na aplikace podporuje:

[!code-csharp[](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a>Vytvořte základní třídu pro stránky Razor

Vytvořte základní třídu, která obsahuje služeb v rámci kontakty stránky Razor. Základní třída vloží tento kód inicializace na jednom místě:

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

Předchozí kód:

* Přidá `IAuthorizationService` služby s přístupem k autorizaci obslužné rutiny.
* Přidá identitu `UserManager` služby.
* Přidat `ApplicationDbContext`.

### <a name="update-the-createmodel"></a>Aktualizace CreateModel

Aktualizovat konstruktor vytvořit stránku modelu používat `DI_BasePageModel` základní třídy:

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

Aktualizace `CreateModel.OnPostAsync` metodu:

* Přidat ID uživatele `Contact` modelu.
* Ověřte, zda že má uživatel oprávnění k vytvoření kontakty autorizace obslužná rutina volejte.

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>Aktualizace IndexModel

Aktualizace `OnGetAsync` metoda tak uživatelům obecné jsou zobrazeny pouze schválené kontaktů:

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>Aktualizace EditModel

Přidejte obslužnou rutinu ověřování k ověření, že uživatel kontakt vlastní. Protože je ověřován autorizace prostředků, `[Authorize]` atribut není dost. Aplikace nemá přístup k prostředku, při hodnocení atributy. Ověření na základě prostředků musí být nutné. Jakmile načítání v modelu stránky nebo načítání v rámci obslužná rutina samotné má aplikace přístup k prostředku, musí provést kontroly. Často přístup k prostředku předáním v klíči prostředků.

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>Aktualizace DeleteModel

Aktualizace modelu stránky odstranit pomocí obslužná rutina ověřování ověřte, zda že má uživatel oprávnění odstranit na kontakt.

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>Vložit služby autorizace do zobrazení

Zobrazuje uživatelské rozhraní v současné době upravovat a odstraňovat odkazy na data, která uživatele nelze upravit. Uživatelské rozhraní je opraven použití obslužná rutina ověřování pro zobrazení.

Vložit služby ověřování *Views/_ViewImports.cshtml* souboru tak, aby byl k dispozici pro všechna zobrazení:

[!code-cshtml[](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

Předchozí kód přidá několik `using` příkazy.

Aktualizace **upravit** a **odstranit** odkazů v *Pages/Contacts/Index.cshtml* , pouze se vykresluje pro uživatele s příslušnými oprávněními:

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-999)]

> [!WARNING]
> Skrytí odkazy z uživatelů, kteří nemají oprávnění ke změně dat nepodporuje zabezpečit aplikace. Skrytí odkazy díky aplikaci přívětivější zobrazením pouze platné odkazy. Uživatelé mohou zabezpečení generované adresy URL pro vyvolání upravit a odstranit operací na data, která budou nevlastníte. Kontroly přístupu k zabezpečení dat musí vynucovat stránky Razor nebo kontroleru.

### <a name="update-details"></a>Podrobné informace o aktualizaci

Zobrazení podrobností aktualizujte, aby správci můžete schválit nebo odmítnout kontaktů:

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-999)]

Aktualizace modelu stránky podrobnosti:

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a>Testování dokončená aplikace

Pokud jste pomocí Visual Studio Code nebo testování na místní platformu, která neobsahuje testovací certifikát pro protokol HTTPS:

* Nastavit `"LocalTest:skipSSL": true` v *appsettings. Developement.JSON* souboru tak, aby přeskočil požadavek HTTPS. Přeskočit HTTPS pouze na vývojovém počítači.

Pokud má kontaktů:

* Odstranit všechny záznamy v `Contact` tabulky.
* Restartujte aplikaci počáteční hodnoty databáze.

Registrace uživatele pro procházení kontaktů.

Snadný způsob, jak testování dokončená aplikace je spustíte tři různé prohlížeče (nebo incognito/InPrivate verze). V jedné prohlížeče registraci nového uživatele (například `test@example.com`). Přihlaste se do každé prohlížeče s jiným uživatelem. Ověřte následující operace:

* Registrovaní uživatelé můžete zobrazit všechny schválené kontaktní data.
* Registrovaní uživatelé můžete upravit nebo odstranit svá vlastní data.
* Správce můžete schválit nebo odmítnout kontaktní údaje. `Details` Zobrazení ukazuje **schválit** a **odmítnout** tlačítka.
* Správci mohou schválit či odmítnout a upravit nebo odstranit všechna data.

| Uživatel| Možnosti |
| ------------ | ---------|
| test@example.com | Můžete upravit nebo odstranit vlastní data |
| manager@contoso.com | Můžete schválit nebo odmítnout a upravit nebo odstranit vlastní data |
| admin@contoso.com | Můžete upravit nebo odstranit a schválit či odmítnout všechna data|

Vytvoření kontaktu v prohlížeči na správce. Zkopírujte adresu URL pro odstranění a upravit z kontaktujte správce. Tyto odkazy vložte do prohlížeče se zobrazil uživatel k ověření, že se zobrazil uživatel nemůže provést tyto operace.

## <a name="create-the-starter-app"></a>Vytvořit úvodní aplikaci

* Vytvoření stránky Razor aplikace s názvem "ContactManager"

  * Vytvoření aplikace s **jednotlivých uživatelských účtů**.
  * Pojmenujte ji "ContactManager", obor názvů odpovídá oboru názvů používaný v ukázce.

  ```console
  dotnet new razor -o ContactManager -au Individual -uld
  ```

  * `-uld` Určuje LocalDB místo SQLite

* Přidejte následující `Contact` modelu:

  [!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* Vygenerované uživatelské rozhraní `Contact` modelu:

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* Aktualizace **ContactManager** ukotvení v *Pages/_Layout.cshtml* souboru:

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* Vygenerovat počáteční migraci a aktualizaci databáze:

```console
dotnet ef migrations add initial
dotnet ef database update
```

* Aplikaci otestovat a vytváření, úpravy a odstranění kontaktu

### <a name="seed-the-database"></a>Počáteční hodnoty databáze

Přidat `SeedData` třídy k *Data* složky. Pokud jste stáhli ukázku, můžete zkopírovat *SeedData.cs* do souboru *Data* složky starter projektu.

Volání `SeedData.Initialize` z `Main`:

[!code-csharp[](secure-data/samples/starter2/Program.cs?name=snippet)]

Otestujte, že aplikace nasadí databázi. Pokud jsou všechny řádky v kontakt DB, metodu počáteční hodnoty nefunguje.

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a>Další zdroje

* [Prostředí ASP.NET Core autorizace](https://github.com/blowdart/AspNetAuthorizationWorkshop). Toto testovací prostředí obsahuje více podrobností o funkcích zabezpečení byla zavedená v tomto kurzu.
* [Autorizace v ASP.NET Core: jednoduchý, na základě deklarace a vlastní role](xref:security/authorization/index)
* [Autorizace uživatele na základě zásad](xref:security/authorization/policies)
