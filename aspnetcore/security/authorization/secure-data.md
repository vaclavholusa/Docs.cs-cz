---
title: "Vytvoření aplikace ASP.NET Core s uživatelskými daty chráněn autorizace"
author: rick-anderson
ms.author: riande
manager: wpickett
ms.date: 05/22/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: security/authorization/secure-data
ms.openlocfilehash: 7404b8ec20ed6a00554c8a7ade9a282362b9a186
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>Vytvoření aplikace ASP.NET Core s uživatelskými daty chráněn autorizace

Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Audette Jan](https://twitter.com/joeaudette)

Tento kurz ukazuje, jak vytvořit webovou aplikaci s uživatelskými daty chráněn autorizace. Zobrazí seznam kontakty, které ověřené uživatele (registrovaný) jste vytvořili. Existují tři skupiny zabezpečení:

* Registrovaní uživatelé můžete zobrazit všechny schválené kontaktní data.
* Registrovaní uživatelé můžete upravit nebo odstranit svá vlastní data. 
* Správce můžete schválit nebo odmítnout kontaktní údaje. Pouze schválené kontakty jsou viditelné pro uživatele.
* Správci mohou schválit či odmítnout a upravit nebo odstranit všechna data.

Na následujícím obrázku, uživatel Rick (`rick@example.com`) je přihlášený. Jenom zobrazení může uživatel Rick schválena kontakty a upravit nebo odstranit jeho kontakty. Pouze poslední záznam vytvořené Rick, zobrazí upravit a odstranit odkazy

![Obrázek popsané výše](secure-data/_static/rick.png)

Na následujícím obrázku `manager@contoso.com` je přihlášen a v roli správce. 

![Obrázek popsané výše](secure-data/_static/manager1.png)

Následující obrázek ukazuje vybraných manažerů, zobrazení podrobností kontaktu.

![Obrázek popsané výše](secure-data/_static/manager.png)

Pouze správci a správci mají schválit a budou odmítat tlačítka.

Na následujícím obrázku `admin@contoso.com` je přihlášen a v roli správce. 

![Obrázek popsané výše](secure-data/_static/admin.png)

Správce má všechna oprávnění. Jana můžete pro čtení, úpravy nebo odstranění všechny kontakty a změnit stav kontaktů.

Aplikace byla vytvořená [generování uživatelského rozhraní](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) následující `Contact` modelu:

[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

A `ContactIsOwnerAuthorizationHandler` obslužná rutina ověřování zajišťuje uživatele můžete upravit pouze svá data. A `ContactManagerAuthorizationHandler` obslužná rutina ověřování umožňuje správcům schválit nebo odmítnout kontakty.  A `ContactAdministratorsAuthorizationHandler` obslužná rutina ověřování umožňuje správcům schválit nebo odmítnout kontakty a upravit nebo odstranit kontakty. 

## <a name="prerequisites"></a>Požadavky

Tato akce není začátku kurzu. Měli byste se seznámit s:

* [Jádro ASP.NET MVC](xref:tutorials/first-mvc-app/start-mvc)
* [Entity Framework Core](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a>Spuštění a dokončené aplikace

[Stáhněte si](xref:tutorials/index#how-to-download-a-sample) [Dokončit](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) aplikace. [Test](#test-the-completed-app) dokončené aplikace, takže seznámit se s jeho funkce zabezpečení. 

### <a name="the-starter-app"></a>Úvodní aplikaci

Je užitečné k porovnání s je hotová ukázka kódu.

[Stáhněte si](xref:tutorials/index#how-to-download-a-sample) [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) aplikace. 

V tématu [vytvořit úvodní aplikaci](#create-the-starter-app) Pokud chcete vytvořit od začátku.

Aktualizace databáze:

```none
   dotnet ef database update
```

Spusťte aplikaci, klepněte **ContactManager** propojit a ověřte, můžete vytvořit, upravit a odstranit a obraťte se na.

V tomto kurzu má všechny hlavní kroky k vytvoření aplikace dat zabezpečení uživatele. Vám může být užitečné k odkazování na dokončený projekt.

## <a name="modify-the-app-to-secure-user-data"></a>Upravit aplikaci k zabezpečení dat uživatele

V následujících částech mít všechny hlavní kroky k vytvoření aplikace dat zabezpečení uživatele. Vám může být užitečné k odkazování na dokončený projekt.

### <a name="tie-the-contact-data-to-the-user"></a>Tie – kontaktních údajů pro uživatele

Pomocí technologie ASP.NET [Identity](xref:security/authentication/identity) ID uživatele, aby uživatelé můžete upravit svá data, ale ne další data uživatele. Přidat `OwnerID` k `Contact` modelu:

[!code-csharp[Main](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]

`OwnerID`ID uživatele z `AspNetUser` tabulky v [Identity](xref:security/authentication/identity) databáze. `Status` Pole určuje, zda kontakt zobrazit obecné uživatele. 

Vygenerovat nový migraci a aktualizaci databáze:

```console
dotnet ef migrations add userID_Status
dotnet ef database update
 ```

### <a name="require-ssl-and-authenticated-users"></a>Vyžadovat protokol SSL a ověření uživatelé

V `ConfigureServices` metodu *Startup.cs* soubor, přidejte [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) filtr autorizace:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]

Přesměrování požadavků HTTP do HTTPS, najdete v části [URL přepisování Middleware](xref:fundamentals/url-rewriting). Pokud používáte Visual Studio Code nebo testování na místní platforma, která neobsahuje testovací certifikát pro protokol SSL:

- Nastavit `"LocalTest:skipSSL": true` v *appSettings.JSON určený* souboru.

### <a name="require-authenticated-users"></a>Vyžadovat ověření uživatelé

Nastavte výchozí zásady ověřování tak, aby vyžadovala uživatele k ověření. Můžete vyjádření výslovného nesouhlasu ověřování ke kontroleru nebo metodě akce s `[AllowAnonymous]` atribut. S tímto přístupem všech nových řadičů přidán automaticky vyžadují ověřování, což je bezpečnější než spoléhat na nových řadičů zahrnout `[Authorize]` atribut. Přidejte následující `ConfigureServices` metodu *Startup.cs* souboru:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]

Přidat `[AllowAnonymous]` domácí řadiče, mohou anonymní uživatelé získat informace o lokalitě, před jejich registraci.

[!code-csharp[Main](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]

### <a name="configure-the-test-account"></a>Nakonfigurujte účet pro test

`SeedData` Třída vytvoří dva účty, správce a správce. Použití [nástroj tajný klíč správce](xref:security/app-secrets) nastavení hesla pro tyto účty. To můžete udělat z adresáře projektu (adresář obsahující *Program.cs*).

```console
dotnet user-secrets set SeedUserPW <PW>
```

Aktualizace `Configure` používat heslo testu:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]

Přidat ID uživatele správce a `Status = ContactStatus.Approved` kontaktům. ID uživatele se zobrazí pouze jeden kontakt, přidejte všechny kontakty:

[!code-csharp[Main](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Vytvoření vlastníka, správce a Správce autorizací obslužné rutiny

Vytvoření `ContactIsOwnerAuthorizationHandler` třídy v *autorizace* složky. `ContactIsOwnerAuthorizationHandler` Ověří uživatele, který funguje na prostředku vlastníkem prostředku.

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

`ContactIsOwnerAuthorizationHandler` Volání `context.Succeed` Pokud ověřený aktuální uživatel je jeho vlastníkem. Obslužné rutiny autorizace obecně vrátit `context.Succeed` Pokud jsou splněny požadavky na. Vracejí `Task.FromResult(0)` Pokud nejsou splněné požadavky. `Task.FromResult(0)`je ani úspěch nebo neúspěch, umožňuje jiných autorizace obslužná rutina spouštěla. Pokud potřebujete explicitně nezdaří, vrátí `context.Fail()`.

Jsme vlastníky kontaktní upravit nebo odstranit svá vlastní data, není třeba zkontrolujte provozní předán v parametru požadavek.

### <a name="create-a-manager-authorization-handler"></a>Vytvořte obslužnou rutinu Správce autorizací

Vytvoření `ContactManagerAuthorizationHandler` třídy v *autorizace* složky. `ContactManagerAuthorizationHandler` Ověří uživatele, který funguje na prostředku je správce. Pouze správci můžete schválit nebo odmítnout změny obsahu (nové nebo změněné).

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Vytvoření obslužné rutiny Správce autorizací

Vytvoření `ContactAdministratorsAuthorizationHandler` třídy v *autorizace* složky. `ContactAdministratorsAuthorizationHandler` Ověří, jestli uživatel, který funguje na prostředku je správcem. Správce může provádět všechny operace.

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Registraci obslužných rutin autorizace

Musí být pro registrované služby pomocí Entity Framework Core [vkládání závislostí](xref:fundamentals/dependency-injection) pomocí [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). `ContactIsOwnerAuthorizationHandler` Používá ASP.NET Core [Identity](xref:security/authentication/identity), který je založený na Entity Framework Core. Zaregistrujte obslužných rutin s kolekcí služby, aby byly k dispozici `ContactsController` prostřednictvím [vkládání závislostí](xref:fundamentals/dependency-injection). Přidejte následující kód do konce `ConfigureServices`:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]

`ContactAdministratorsAuthorizationHandler`a `ContactManagerAuthorizationHandler` jsou přidány jako jednotlivé prvky. Jsou jednotlivé prvky, protože se nepoužívají EF a veškeré informace potřebné v `Context` parametr `HandleRequirementAsync` metoda.

Kompletní `ConfigureServices`:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=ConfigureServices)]

## <a name="update-the-code-to-support-authorization"></a>Aktualizujte kód pro podporu ověřování

V této části aktualizace řadiče a zobrazení a přidejte třídu operations požadavky.

### <a name="update-the-contacts-controller"></a>Aktualizaci řadiče kontaktů

Aktualizace `ContactsController` konstruktor:

* Přidat `IAuthorizationService` služby s přístupem k autorizaci obslužné rutiny. 
* Přidat `Identity` `UserManager` služby:

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]

### <a name="add-a-contact-operations-requirements-class"></a>Přidání třídy požadavky kontaktní operací

Přidat `ContactOperations` třídy k *autorizace* složky. Tato třída obsahovat požadavky na naše aplikace podporuje:

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

### <a name="update-create"></a>Vytvoření aktualizace

Aktualizace `HTTP POST Create` metodu:

* Přidat ID uživatele `Contact` modelu.
* Obslužná rutina ověřování k ověření, že uživatel kontakt vlastní volání.

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]

### <a name="update-edit"></a>Upravit aktualizaci

Aktualizovat i `Edit` kontakt vlastní metody sloužící obslužná rutina ověřování k ověření uživatele. Protože jsme se provádí autorizace prostředků nemůžeme použít `[Authorize]` atribut. Při hodnocení atributy nemáme přístup k prostředku. Autorizace prostředků na základě musí být nutné. Jakmile jsme načtením v kontroleru nebo načtením v rámci samotné obslužná rutina mít přístup k prostředku, musí provést kontroly. Často se přístup k prostředku předáním v klíči prostředků.

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]

### <a name="update-the-delete-method"></a>Aktualizujte metodu Delete

Aktualizovat i `Delete` kontakt vlastní metody sloužící obslužná rutina ověřování k ověření uživatele.

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]

## <a name="inject-the-authorization-service-into-the-views"></a>Vložit služby autorizace do zobrazení

Uživatelské rozhraní zobrazí aktuálně upravovat a odstraňovat odkazy na data, která uživatele nelze upravit. To jsme budete opravíme použitím obslužná rutina ověřování na zobrazení.

Vložit služby ověřování *Views/_ViewImports.cshtml* souboru, bude k dispozici u všech zobrazení:

[!code-html[Main](secure-data/samples/final/Views/_ViewImports.cshtml)]

Aktualizace *Views/Contacts/Index.cshtml* zobrazení syntaxe Razor pouze zobrazení upravit a odstranit odkazy pro uživatele, kteří mohou upravit nebo odstranit kontakt.

Přidat`@using ContactManager.Authorization;`

Aktualizace `Edit` a `Delete` odkazy, takže se jenom vykresluje pro uživatele s oprávnění upravovat a odstraňovat kontakt.

[!code-html[Main](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]

Upozornění: Skrytí odkazy z uživatelů, kteří nemají oprávnění k úpravě nebo odstranění dat nepodporuje zabezpečit aplikace. Skrytí odkazy díky aplikaci další uživatele popisný zobrazením pouze platné odkazy. Uživatelé mohou zabezpečení generované adresy URL pro vyvolání upravit a odstranit operací na data, která budou nevlastníte.  Kontroler nutné opakovat, že přístup kontroly zabezpečená.

### <a name="update-the-details-view"></a>Aktualizace zobrazení podrobností

Zobrazení podrobností aktualizujte, aby správci můžete schválit nebo odmítnout kontaktů:

[!code-html[Main](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]

## <a name="test-the-completed-app"></a>Testování dokončená aplikace

Pokud používáte Visual Studio Code nebo testování na místní platforma, která neobsahuje testovací certifikát pro protokol SSL:

- Nastavit `"LocalTest:skipSSL": true` v *appSettings.JSON určený* souboru.

Pokud jste spustili aplikaci a máte kontakty, odstraňte všechny záznamy v `Contact` tabulky a restartujte aplikaci počáteční hodnoty databáze. Pokud používáte Visual Studio, musíte ukončit a restartovat službu IIS Express na počáteční hodnoty databáze.

Zaregistrujte uživatelům procházet kontaktů.

Snadný způsob, jak testování dokončená aplikace je spustíte tři různé prohlížeče (nebo incognito/InPrivate verze). V jedné prohlížeče registraci nového uživatele, například `test@example.com`. Přihlaste se do každé prohlížeče s jiným uživatelem. Ověřte následující:

* Registrovaní uživatelé můžete zobrazit všechny schválené kontaktní data.
* Registrovaní uživatelé můžete upravit nebo odstranit svá vlastní data. 
* Správce můžete schválit nebo odmítnout kontaktní údaje. `Details` Zobrazení ukazuje **schválit** a **odmítnout** tlačítka. 
* Správci mohou schválit či odmítnout a upravit nebo odstranit všechna data.

| Uživatel| Možnosti |
| ------------ | ---------|
| test@example.com | Můžete upravit nebo odstranit vlastní data |
| manager@contoso.com | Můžete schválit nebo odmítnout a upravit nebo odstranit vlastní data  |
| admin@contoso.com | Můžete upravit nebo odstranit a schválit či odmítnout všechna data|

Vytvoření kontaktu v prohlížeči správci. Zkopírujte adresu URL pro odstranění a upravit z kontaktujte správce. Tyto odkazy vložte do prohlížeče se zobrazil uživatel k ověření, že se zobrazil uživatel nemůže provést tyto operace.

## <a name="create-the-starter-app"></a>Vytvořit úvodní aplikaci

Postupujte podle těchto pokynů můžete vytvořit úvodní aplikaci.

* Vytvoření **webové aplikace ASP.NET Core** pomocí [Visual Studio 2017](https://www.visualstudio.com/) s názvem "ContactManager"

  * Vytvoření aplikace s **jednotlivých uživatelských účtů**.
  * Pojmenujte ji "ContactManager", vašeho oboru názvů bude odpovídat oboru názvů používají ve vzorku.

* Přidejte následující `Contact` modelu:

  [!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

* Vygenerované uživatelské rozhraní `Contact` model pomocí Entity Framework Core a `ApplicationDbContext` data kontextu. Přijměte všechny výchozí hodnoty generování uživatelského rozhraní. Pomocí `ApplicationDbContext` pro kontext dat třída vloží tabulky kontaktů [Identity](xref:security/authentication/identity) databáze. V tématu [přidání model](xref:tutorials/first-mvc-app/adding-model) Další informace.

* Aktualizace **ContactManager** ukotvení v *Views/Shared/_Layout.cshtml* souboru z `asp-controller="Home"` k `asp-controller="Contacts"` tak klepnutím **ContactManager** odkaz Vyvolá řadičem kontakty. Původní kód:

```html
   <a asp-area="" asp-controller="Home" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

Aktualizované značek:

```html
   <a asp-area="" asp-controller="Contacts" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

* Vygenerovat počáteční migraci a aktualizaci databáze

```none
   dotnet ef migrations add initial
   dotnet ef database update
   ```

* Aplikaci otestovat a vytváření, úpravy a odstranění kontaktu

### <a name="seed-the-database"></a>Počáteční hodnoty databáze

Přidat `SeedData` třídy k *Data* složky. Pokud jste stáhli ukázku, můžete zkopírovat *SeedData.cs* do souboru *Data* složky starter projektu.

[!code-csharp[Main](secure-data/samples/starter/Data/SeedData.cs)]

Zvýrazněný kód přidejte na konec `Configure` metoda v *Startup.cs* souboru:

[!code-csharp[Main](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]

Otestujte, že aplikace nasadí databázi. Metoda počáteční hodnoty nefunguje, pokud jsou všechny řádky v kontakt DB.

### <a name="create-a-class-used-in-the-tutorial"></a>Vytvoření třídy v tomto kurzu použili

* Vytvořte složku s názvem *autorizace*.
* Kopírování *Authorization\ContactOperations.cs* soubor ze stažení dokončené projektu nebo zkopírujte následující kód:

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a>Další zdroje

* [Prostředí ASP.NET Core autorizace](https://github.com/blowdart/AspNetAuthorizationWorkshop). Toto testovací prostředí obsahuje více podrobností o funkcích zabezpečení byla zavedená v tomto kurzu.
* [Autorizace v ASP.NET Core: jednoduchý, na základě deklarace a vlastních rolí](index.md)
* [Autorizace uživatele na základě zásad](policies.md)
