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
ms.openlocfilehash: 861ac619c7f5fb19a56c59536e20724d96bbddca
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="45c2b-102">Vytvoření aplikace ASP.NET Core s uživatelskými daty chráněn autorizace</span><span class="sxs-lookup"><span data-stu-id="45c2b-102">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="45c2b-103">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Audette Jan](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="45c2b-103">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="45c2b-104">Tento kurz ukazuje, jak vytvořit webovou aplikaci s uživatelskými daty chráněn autorizace.</span><span class="sxs-lookup"><span data-stu-id="45c2b-104">This tutorial shows how to create a web app with user data protected by authorization.</span></span> <span data-ttu-id="45c2b-105">Zobrazí seznam kontakty, které ověřené uživatele (registrovaný) jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="45c2b-105">It  displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="45c2b-106">Existují tři skupiny zabezpečení:</span><span class="sxs-lookup"><span data-stu-id="45c2b-106">There are three security groups:</span></span>

* <span data-ttu-id="45c2b-107">Registrovaní uživatelé můžete zobrazit všechny schválené kontaktní data.</span><span class="sxs-lookup"><span data-stu-id="45c2b-107">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="45c2b-108">Registrovaní uživatelé můžete upravit nebo odstranit svá vlastní data.</span><span class="sxs-lookup"><span data-stu-id="45c2b-108">Registered users can edit/delete their own data.</span></span> 
* <span data-ttu-id="45c2b-109">Správce můžete schválit nebo odmítnout kontaktní údaje.</span><span class="sxs-lookup"><span data-stu-id="45c2b-109">Managers can approve or reject contact data.</span></span> <span data-ttu-id="45c2b-110">Pouze schválené kontakty jsou viditelné pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="45c2b-110">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="45c2b-111">Správci mohou schválit či odmítnout a upravit nebo odstranit všechna data.</span><span class="sxs-lookup"><span data-stu-id="45c2b-111">Administrators can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="45c2b-112">Na následujícím obrázku, uživatel Rick (`rick@example.com`) je přihlášený.</span><span class="sxs-lookup"><span data-stu-id="45c2b-112">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="45c2b-113">Jenom zobrazení může uživatel Rick schválena kontakty a upravit nebo odstranit jeho kontakty.</span><span class="sxs-lookup"><span data-stu-id="45c2b-113">User Rick can only view approved contacts and edit/delete his contacts.</span></span> <span data-ttu-id="45c2b-114">Pouze poslední záznam vytvořené Rick, zobrazí upravit a odstranit odkazy</span><span class="sxs-lookup"><span data-stu-id="45c2b-114">Only the last record, created by Rick, displays edit and delete links</span></span>

![Obrázek popsané výše](secure-data/_static/rick.png)

<span data-ttu-id="45c2b-116">Na následujícím obrázku `manager@contoso.com` je přihlášen a v roli správce.</span><span class="sxs-lookup"><span data-stu-id="45c2b-116">In the following image, `manager@contoso.com` is signed in and in the managers role.</span></span> 

![Obrázek popsané výše](secure-data/_static/manager1.png)

<span data-ttu-id="45c2b-118">Následující obrázek ukazuje vybraných manažerů, zobrazení podrobností kontaktu.</span><span class="sxs-lookup"><span data-stu-id="45c2b-118">The following image shows the  managers details view of a contact.</span></span>

![Obrázek popsané výše](secure-data/_static/manager.png)

<span data-ttu-id="45c2b-120">Pouze správci a správci mají schválit a budou odmítat tlačítka.</span><span class="sxs-lookup"><span data-stu-id="45c2b-120">Only managers and administrators have the approve and reject buttons.</span></span>

<span data-ttu-id="45c2b-121">Na následujícím obrázku `admin@contoso.com` je přihlášen a v roli správce.</span><span class="sxs-lookup"><span data-stu-id="45c2b-121">In the following image, `admin@contoso.com` is signed in and in the administrator’s role.</span></span> 

![Obrázek popsané výše](secure-data/_static/admin.png)

<span data-ttu-id="45c2b-123">Správce má všechna oprávnění.</span><span class="sxs-lookup"><span data-stu-id="45c2b-123">The administrator has all privileges.</span></span> <span data-ttu-id="45c2b-124">Jana můžete pro čtení, úpravy nebo odstranění všechny kontakty a změnit stav kontaktů.</span><span class="sxs-lookup"><span data-stu-id="45c2b-124">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="45c2b-125">Aplikace byla vytvořená [generování uživatelského rozhraní](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) následující `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="45c2b-125">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller)  the following `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="45c2b-126">A `ContactIsOwnerAuthorizationHandler` obslužná rutina ověřování zajišťuje uživatele můžete upravit pouze svá data.</span><span class="sxs-lookup"><span data-stu-id="45c2b-126">A `ContactIsOwnerAuthorizationHandler` authorization handler ensures that a user can only edit their data.</span></span> <span data-ttu-id="45c2b-127">A `ContactManagerAuthorizationHandler` obslužná rutina ověřování umožňuje správcům schválit nebo odmítnout kontakty.</span><span class="sxs-lookup"><span data-stu-id="45c2b-127">A `ContactManagerAuthorizationHandler` authorization handler allows managers to approve or reject contacts.</span></span>  <span data-ttu-id="45c2b-128">A `ContactAdministratorsAuthorizationHandler` obslužná rutina ověřování umožňuje správcům schválit nebo odmítnout kontakty a upravit nebo odstranit kontakty.</span><span class="sxs-lookup"><span data-stu-id="45c2b-128">A `ContactAdministratorsAuthorizationHandler` authorization handler allows administrators to approve or reject contacts and to edit/delete contacts.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="45c2b-129">Požadavky</span><span class="sxs-lookup"><span data-stu-id="45c2b-129">Prerequisites</span></span>

<span data-ttu-id="45c2b-130">Toto není začátku kurzu.</span><span class="sxs-lookup"><span data-stu-id="45c2b-130">This is not a beginning tutorial.</span></span> <span data-ttu-id="45c2b-131">Měli byste se seznámit s:</span><span class="sxs-lookup"><span data-stu-id="45c2b-131">You should be familiar with:</span></span>

* [<span data-ttu-id="45c2b-132">Jádro ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="45c2b-132">ASP.NET Core MVC</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="45c2b-133">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="45c2b-133">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="45c2b-134">Spuštění a dokončené aplikace</span><span class="sxs-lookup"><span data-stu-id="45c2b-134">The starter and completed app</span></span>

<span data-ttu-id="45c2b-135">[Stáhněte si](xref:tutorials/index#how-to-download-a-sample) [Dokončit](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) aplikace.</span><span class="sxs-lookup"><span data-stu-id="45c2b-135">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) app.</span></span> <span data-ttu-id="45c2b-136">[Test](#test-the-completed-app) dokončené aplikace, takže seznámit se s jeho funkce zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="45c2b-136">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span> 

### <a name="the-starter-app"></a><span data-ttu-id="45c2b-137">Úvodní aplikaci</span><span class="sxs-lookup"><span data-stu-id="45c2b-137">The starter app</span></span>

<span data-ttu-id="45c2b-138">Je užitečné k porovnání s je hotová ukázka kódu.</span><span class="sxs-lookup"><span data-stu-id="45c2b-138">It's helpful to compare your code with the completed sample.</span></span>

<span data-ttu-id="45c2b-139">[Stáhněte si](xref:tutorials/index#how-to-download-a-sample) [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) aplikace.</span><span class="sxs-lookup"><span data-stu-id="45c2b-139">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) app.</span></span> 

<span data-ttu-id="45c2b-140">V tématu [vytvořit úvodní aplikaci](#create-the-starter-app) Pokud chcete vytvořit od začátku.</span><span class="sxs-lookup"><span data-stu-id="45c2b-140">See [Create the starter app](#create-the-starter-app) if you'd like to create it from scratch.</span></span>

<span data-ttu-id="45c2b-141">Aktualizace databáze:</span><span class="sxs-lookup"><span data-stu-id="45c2b-141">Update the database:</span></span>

```none
   dotnet ef database update
```

<span data-ttu-id="45c2b-142">Spusťte aplikaci, klepněte **ContactManager** propojit a ověřte, můžete vytvořit, upravit a odstranit a obraťte se na.</span><span class="sxs-lookup"><span data-stu-id="45c2b-142">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

<span data-ttu-id="45c2b-143">V tomto kurzu má všechny hlavní kroky k vytvoření aplikace dat zabezpečení uživatele.</span><span class="sxs-lookup"><span data-stu-id="45c2b-143">This tutorial has all the major steps to create the secure user data app.</span></span> <span data-ttu-id="45c2b-144">Vám může být užitečné k odkazování na dokončený projekt.</span><span class="sxs-lookup"><span data-stu-id="45c2b-144">You may find it helpful to refer to the completed project.</span></span>

## <a name="modify-the-app-to-secure-user-data"></a><span data-ttu-id="45c2b-145">Upravit aplikaci k zabezpečení dat uživatele</span><span class="sxs-lookup"><span data-stu-id="45c2b-145">Modify the app to secure user data</span></span>

<span data-ttu-id="45c2b-146">V následujících částech mít všechny hlavní kroky k vytvoření aplikace dat zabezpečení uživatele.</span><span class="sxs-lookup"><span data-stu-id="45c2b-146">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="45c2b-147">Vám může být užitečné k odkazování na dokončený projekt.</span><span class="sxs-lookup"><span data-stu-id="45c2b-147">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="45c2b-148">Tie – kontaktních údajů pro uživatele</span><span class="sxs-lookup"><span data-stu-id="45c2b-148">Tie the contact data to the user</span></span>

<span data-ttu-id="45c2b-149">Pomocí technologie ASP.NET [Identity](xref:security/authentication/identity) ID uživatele, aby uživatelé můžete upravit svá data, ale ne další data uživatele.</span><span class="sxs-lookup"><span data-stu-id="45c2b-149">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="45c2b-150">Přidat `OwnerID` k `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="45c2b-150">Add `OwnerID` to the `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]

<span data-ttu-id="45c2b-151">`OwnerID`ID uživatele z `AspNetUser` tabulky v [Identity](xref:security/authentication/identity) databáze.</span><span class="sxs-lookup"><span data-stu-id="45c2b-151">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="45c2b-152">`Status` Pole určuje, zda kontakt zobrazit obecné uživatele.</span><span class="sxs-lookup"><span data-stu-id="45c2b-152">The `Status` field determines if a contact is viewable by general users.</span></span> 

<span data-ttu-id="45c2b-153">Vygenerovat nový migraci a aktualizaci databáze:</span><span class="sxs-lookup"><span data-stu-id="45c2b-153">Scaffold a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
 ```

### <a name="require-ssl-and-authenticated-users"></a><span data-ttu-id="45c2b-154">Vyžadovat protokol SSL a ověření uživatelé</span><span class="sxs-lookup"><span data-stu-id="45c2b-154">Require SSL and authenticated users</span></span>

<span data-ttu-id="45c2b-155">V `ConfigureServices` metodu *Startup.cs* soubor, přidejte [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) filtr autorizace:</span><span class="sxs-lookup"><span data-stu-id="45c2b-155">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) authorization filter:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]

<span data-ttu-id="45c2b-156">Přesměrování požadavků HTTP do HTTPS, najdete v části [URL přepisování Middleware](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="45c2b-156">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="45c2b-157">Pokud používáte Visual Studio Code nebo testování na místní platforma, která neobsahuje testovací certifikát pro protokol SSL:</span><span class="sxs-lookup"><span data-stu-id="45c2b-157">If you are using Visual Studio Code or testing on local platform that doesn't include a test certificate for SSL:</span></span>

- <span data-ttu-id="45c2b-158">Nastavit `"LocalTest:skipSSL": true` v *appSettings.JSON určený* souboru.</span><span class="sxs-lookup"><span data-stu-id="45c2b-158">Set `"LocalTest:skipSSL": true` in the *appsettings.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="45c2b-159">Vyžadovat ověření uživatelé</span><span class="sxs-lookup"><span data-stu-id="45c2b-159">Require authenticated users</span></span>

<span data-ttu-id="45c2b-160">Nastavte výchozí zásady ověřování tak, aby vyžadovala uživatele k ověření.</span><span class="sxs-lookup"><span data-stu-id="45c2b-160">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="45c2b-161">Můžete vyjádření výslovného nesouhlasu ověřování ke kontroleru nebo metodě akce s `[AllowAnonymous]` atribut.</span><span class="sxs-lookup"><span data-stu-id="45c2b-161">You can opt out of authentication at the controller or action method with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="45c2b-162">S tímto přístupem všech nových řadičů přidán automaticky vyžadují ověřování, což je bezpečnější než spoléhat na nových řadičů zahrnout `[Authorize]` atribut.</span><span class="sxs-lookup"><span data-stu-id="45c2b-162">With this approach, any new controllers added will automatically require authentication, which is safer than relying on new controllers to include the `[Authorize]` attribute.</span></span> <span data-ttu-id="45c2b-163">Přidejte následující `ConfigureServices` metodu *Startup.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="45c2b-163">Add the following to  the `ConfigureServices` method of the *Startup.cs* file:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]

<span data-ttu-id="45c2b-164">Přidat `[AllowAnonymous]` domácí řadiče, mohou anonymní uživatelé získat informace o lokalitě, před jejich registraci.</span><span class="sxs-lookup"><span data-stu-id="45c2b-164">Add `[AllowAnonymous]` to the home controller so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="45c2b-165">Nakonfigurujte účet pro test</span><span class="sxs-lookup"><span data-stu-id="45c2b-165">Configure the test account</span></span>

<span data-ttu-id="45c2b-166">`SeedData` Třída vytvoří dva účty, správce a správce.</span><span class="sxs-lookup"><span data-stu-id="45c2b-166">The `SeedData` class creates two accounts,  administrator and manager.</span></span> <span data-ttu-id="45c2b-167">Použití [nástroj tajný klíč správce](xref:security/app-secrets) nastavení hesla pro tyto účty.</span><span class="sxs-lookup"><span data-stu-id="45c2b-167">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="45c2b-168">To můžete udělat z adresáře projektu (adresář obsahující *Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="45c2b-168">Do this from the project directory (the directory containing *Program.cs*).</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="45c2b-169">Aktualizace `Configure` používat heslo testu:</span><span class="sxs-lookup"><span data-stu-id="45c2b-169">Update `Configure` to use the test password:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]

<span data-ttu-id="45c2b-170">Přidat ID uživatele správce a `Status = ContactStatus.Approved` kontaktům.</span><span class="sxs-lookup"><span data-stu-id="45c2b-170">Add the administrator user ID and `Status = ContactStatus.Approved` to the contacts.</span></span> <span data-ttu-id="45c2b-171">ID uživatele se zobrazí pouze jeden kontakt, přidejte všechny kontakty:</span><span class="sxs-lookup"><span data-stu-id="45c2b-171">Only one contact is shown, add the user ID to all contacts:</span></span>

[!code-csharp[Main](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="45c2b-172">Vytvoření vlastníka, správce a Správce autorizací obslužné rutiny</span><span class="sxs-lookup"><span data-stu-id="45c2b-172">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="45c2b-173">Vytvoření `ContactIsOwnerAuthorizationHandler` třídy v *autorizace* složky.</span><span class="sxs-lookup"><span data-stu-id="45c2b-173">Create a `ContactIsOwnerAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="45c2b-174">`ContactIsOwnerAuthorizationHandler` Ověří uživatele, který funguje na prostředku vlastníkem prostředku.</span><span class="sxs-lookup"><span data-stu-id="45c2b-174">The `ContactIsOwnerAuthorizationHandler` will verify the user acting on the resource owns the resource.</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="45c2b-175">`ContactIsOwnerAuthorizationHandler` Volání `context.Succeed` Pokud ověřený aktuální uživatel je jeho vlastníkem.</span><span class="sxs-lookup"><span data-stu-id="45c2b-175">The `ContactIsOwnerAuthorizationHandler` calls `context.Succeed` if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="45c2b-176">Obslužné rutiny autorizace obecně vrátit `context.Succeed` Pokud jsou splněny požadavky na.</span><span class="sxs-lookup"><span data-stu-id="45c2b-176">Authorization handlers generally return `context.Succeed` when the requirements are met.</span></span> <span data-ttu-id="45c2b-177">Vracejí `Task.FromResult(0)` Pokud nejsou splněné požadavky.</span><span class="sxs-lookup"><span data-stu-id="45c2b-177">They return `Task.FromResult(0)` when requirements are not met.</span></span> <span data-ttu-id="45c2b-178">`Task.FromResult(0)`je ani úspěch nebo neúspěch, umožňuje jiných autorizace obslužná rutina spouštěla.</span><span class="sxs-lookup"><span data-stu-id="45c2b-178">`Task.FromResult(0)` is neither success or failure, it allows other authorization handler to run.</span></span> <span data-ttu-id="45c2b-179">Pokud potřebujete explicitně nezdaří, vrátí `context.Fail()`.</span><span class="sxs-lookup"><span data-stu-id="45c2b-179">If you need to explicitly fail, return `context.Fail()`.</span></span>

<span data-ttu-id="45c2b-180">Jsme vlastníky kontaktní upravit nebo odstranit svá vlastní data, není třeba zkontrolujte provozní předán v parametru požadavek.</span><span class="sxs-lookup"><span data-stu-id="45c2b-180">We allow contact owners to edit/delete their own data, so we don't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="45c2b-181">Vytvořte obslužnou rutinu Správce autorizací</span><span class="sxs-lookup"><span data-stu-id="45c2b-181">Create a manager authorization handler</span></span>

<span data-ttu-id="45c2b-182">Vytvoření `ContactManagerAuthorizationHandler` třídy v *autorizace* složky.</span><span class="sxs-lookup"><span data-stu-id="45c2b-182">Create a `ContactManagerAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="45c2b-183">`ContactManagerAuthorizationHandler` Ověří uživatele, který funguje na prostředku je správce.</span><span class="sxs-lookup"><span data-stu-id="45c2b-183">The `ContactManagerAuthorizationHandler` will verify the user acting on the resource is a manager.</span></span> <span data-ttu-id="45c2b-184">Pouze správci můžete schválit nebo odmítnout změny obsahu (nové nebo změněné).</span><span class="sxs-lookup"><span data-stu-id="45c2b-184">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="45c2b-185">Vytvoření obslužné rutiny Správce autorizací</span><span class="sxs-lookup"><span data-stu-id="45c2b-185">Create an administrator authorization handler</span></span>

<span data-ttu-id="45c2b-186">Vytvoření `ContactAdministratorsAuthorizationHandler` třídy v *autorizace* složky.</span><span class="sxs-lookup"><span data-stu-id="45c2b-186">Create a `ContactAdministratorsAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="45c2b-187">`ContactAdministratorsAuthorizationHandler` Ověří, jestli uživatel, který funguje na prostředku je správcem.</span><span class="sxs-lookup"><span data-stu-id="45c2b-187">The `ContactAdministratorsAuthorizationHandler` will verify the user acting on the resource is a administrator.</span></span> <span data-ttu-id="45c2b-188">Správce může provádět všechny operace.</span><span class="sxs-lookup"><span data-stu-id="45c2b-188">Administrator can do all operations.</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="45c2b-189">Registraci obslužných rutin autorizace</span><span class="sxs-lookup"><span data-stu-id="45c2b-189">Register the authorization handlers</span></span>

<span data-ttu-id="45c2b-190">Musí být pro registrované služby pomocí Entity Framework Core [vkládání závislostí](xref:fundamentals/dependency-injection) pomocí [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="45c2b-190">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="45c2b-191">`ContactIsOwnerAuthorizationHandler` Používá ASP.NET Core [Identity](xref:security/authentication/identity), který je založený na Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="45c2b-191">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="45c2b-192">Zaregistrujte obslužných rutin s kolekcí služby, aby byly k dispozici `ContactsController` prostřednictvím [vkládání závislostí](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="45c2b-192">Register the handlers with the service collection so they will be available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="45c2b-193">Přidejte následující kód do konce `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="45c2b-193">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]

<span data-ttu-id="45c2b-194">`ContactAdministratorsAuthorizationHandler`a `ContactManagerAuthorizationHandler` jsou přidány jako jednotlivé prvky.</span><span class="sxs-lookup"><span data-stu-id="45c2b-194">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="45c2b-195">Jsou jednotlivé prvky, protože se nepoužívají EF a veškeré informace potřebné v `Context` parametr `HandleRequirementAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="45c2b-195">They are singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

<span data-ttu-id="45c2b-196">Kompletní `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="45c2b-196">The complete `ConfigureServices`:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=ConfigureServices)]

## <a name="update-the-code-to-support-authorization"></a><span data-ttu-id="45c2b-197">Aktualizujte kód pro podporu ověřování</span><span class="sxs-lookup"><span data-stu-id="45c2b-197">Update the code to support authorization</span></span>

<span data-ttu-id="45c2b-198">V této části aktualizace řadiče a zobrazení a přidejte třídu operations požadavky.</span><span class="sxs-lookup"><span data-stu-id="45c2b-198">In this section, you update the controller and views and add an operations requirements class.</span></span>

### <a name="update-the-contacts-controller"></a><span data-ttu-id="45c2b-199">Aktualizaci řadiče kontaktů</span><span class="sxs-lookup"><span data-stu-id="45c2b-199">Update the Contacts controller</span></span>

<span data-ttu-id="45c2b-200">Aktualizace `ContactsController` konstruktor:</span><span class="sxs-lookup"><span data-stu-id="45c2b-200">Update the `ContactsController` constructor:</span></span>

* <span data-ttu-id="45c2b-201">Přidat `IAuthorizationService` služby s přístupem k autorizaci obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="45c2b-201">Add the `IAuthorizationService` service to  access to the authorization handlers.</span></span> 
* <span data-ttu-id="45c2b-202">Přidat `Identity` `UserManager` služby:</span><span class="sxs-lookup"><span data-stu-id="45c2b-202">Add the `Identity` `UserManager` service:</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]

### <a name="add-a-contact-operations-requirements-class"></a><span data-ttu-id="45c2b-203">Přidání třídy požadavky kontaktní operací</span><span class="sxs-lookup"><span data-stu-id="45c2b-203">Add a contact operations requirements class</span></span>

<span data-ttu-id="45c2b-204">Přidat `ContactOperations` třídy k *autorizace* složky.</span><span class="sxs-lookup"><span data-stu-id="45c2b-204">Add the `ContactOperations` class to the *Authorization* folder.</span></span> <span data-ttu-id="45c2b-205">Tato třída obsahovat požadavky na naše aplikace podporuje:</span><span class="sxs-lookup"><span data-stu-id="45c2b-205">This class  contain the requirements our app supports:</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

### <a name="update-create"></a><span data-ttu-id="45c2b-206">Vytvoření aktualizace</span><span class="sxs-lookup"><span data-stu-id="45c2b-206">Update Create</span></span>

<span data-ttu-id="45c2b-207">Aktualizace `HTTP POST Create` metodu:</span><span class="sxs-lookup"><span data-stu-id="45c2b-207">Update the `HTTP POST Create` method to:</span></span>

* <span data-ttu-id="45c2b-208">Přidat ID uživatele `Contact` modelu.</span><span class="sxs-lookup"><span data-stu-id="45c2b-208">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="45c2b-209">Obslužná rutina ověřování k ověření, že uživatel kontakt vlastní volání.</span><span class="sxs-lookup"><span data-stu-id="45c2b-209">Call the authorization handler to verify the user owns the contact.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]

### <a name="update-edit"></a><span data-ttu-id="45c2b-210">Upravit aktualizaci</span><span class="sxs-lookup"><span data-stu-id="45c2b-210">Update Edit</span></span>

<span data-ttu-id="45c2b-211">Aktualizovat i `Edit` kontakt vlastní metody sloužící obslužná rutina ověřování k ověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="45c2b-211">Update both `Edit` methods to use the authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="45c2b-212">Protože jsme se provádí autorizace prostředků nemůžeme použít `[Authorize]` atribut.</span><span class="sxs-lookup"><span data-stu-id="45c2b-212">Because we are performing resource authorization we cannot use the `[Authorize]` attribute.</span></span> <span data-ttu-id="45c2b-213">Při hodnocení atributy nemáme přístup k prostředku.</span><span class="sxs-lookup"><span data-stu-id="45c2b-213">We don't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="45c2b-214">Autorizace prostředků na základě musí být nutné.</span><span class="sxs-lookup"><span data-stu-id="45c2b-214">Resource based authorization must be imperative.</span></span> <span data-ttu-id="45c2b-215">Jakmile jsme načtením v kontroleru nebo načtením v rámci samotné obslužná rutina mít přístup k prostředku, musí provést kontroly.</span><span class="sxs-lookup"><span data-stu-id="45c2b-215">Checks must be performed once we have access to the resource, either by loading it in our controller, or by loading it within the handler itself.</span></span> <span data-ttu-id="45c2b-216">Často se přístup k prostředku předáním v klíči prostředků.</span><span class="sxs-lookup"><span data-stu-id="45c2b-216">Frequently you will access the resource by passing in the resource key.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]

### <a name="update-the-delete-method"></a><span data-ttu-id="45c2b-217">Aktualizujte metodu Delete</span><span class="sxs-lookup"><span data-stu-id="45c2b-217">Update the Delete method</span></span>

<span data-ttu-id="45c2b-218">Aktualizovat i `Delete` kontakt vlastní metody sloužící obslužná rutina ověřování k ověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="45c2b-218">Update both `Delete` methods to use the authorization handler to verify the user owns the contact.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="45c2b-219">Vložit služby autorizace do zobrazení</span><span class="sxs-lookup"><span data-stu-id="45c2b-219">Inject the authorization service into the views</span></span>

<span data-ttu-id="45c2b-220">Uživatelské rozhraní zobrazí aktuálně upravovat a odstraňovat odkazy na data, která uživatele nelze upravit.</span><span class="sxs-lookup"><span data-stu-id="45c2b-220">Currently the UI shows edit and delete links for data the user cannot modify.</span></span> <span data-ttu-id="45c2b-221">To jsme budete opravíme použitím obslužná rutina ověřování na zobrazení.</span><span class="sxs-lookup"><span data-stu-id="45c2b-221">We'll fix that by applying the authorization handler to the views.</span></span>

<span data-ttu-id="45c2b-222">Vložit služby ověřování *Views/_ViewImports.cshtml* souboru, bude k dispozici u všech zobrazení:</span><span class="sxs-lookup"><span data-stu-id="45c2b-222">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it will be available to all views:</span></span>

[!code-html[Main](secure-data/samples/final/Views/_ViewImports.cshtml)]

<span data-ttu-id="45c2b-223">Aktualizace *Views/Contacts/Index.cshtml* zobrazení syntaxe Razor pouze zobrazení upravit a odstranit odkazy pro uživatele, kteří mohou upravit nebo odstranit kontakt.</span><span class="sxs-lookup"><span data-stu-id="45c2b-223">Update the *Views/Contacts/Index.cshtml* Razor view to only display the edit and delete links for users who can edit/delete the contact.</span></span>

<span data-ttu-id="45c2b-224">Přidat`@using ContactManager.Authorization;`</span><span class="sxs-lookup"><span data-stu-id="45c2b-224">Add `@using ContactManager.Authorization;`</span></span>

<span data-ttu-id="45c2b-225">Aktualizace `Edit` a `Delete` odkazy tak vykresleny pouze pro uživatele s oprávnění upravovat a odstraňovat kontakt.</span><span class="sxs-lookup"><span data-stu-id="45c2b-225">Update the `Edit` and `Delete` links so they are only rendered for users with permission to edit and delete the contact.</span></span>

[!code-html[Main](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]

<span data-ttu-id="45c2b-226">Upozornění: Skrytí odkazy z uživatelů, kteří nemají oprávnění k úpravě nebo odstranění dat nezabezpečuje aplikace.</span><span class="sxs-lookup"><span data-stu-id="45c2b-226">Warning: Hiding links from users that do not have permission to edit or delete data does not secure the app.</span></span> <span data-ttu-id="45c2b-227">Skrytí odkazy díky aplikaci další uživatele popisný zobrazením pouze platné odkazy.</span><span class="sxs-lookup"><span data-stu-id="45c2b-227">Hiding links makes the app more user friendly by displaying only valid links.</span></span> <span data-ttu-id="45c2b-228">Uživatelé mohou zabezpečení generované adresy URL pro vyvolání upravit a odstranit operací na data, která budou nevlastníte.</span><span class="sxs-lookup"><span data-stu-id="45c2b-228">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span>  <span data-ttu-id="45c2b-229">Kontroler nutné opakovat, že přístup kontroly zabezpečená.</span><span class="sxs-lookup"><span data-stu-id="45c2b-229">The controller must repeat the access checks to be secure.</span></span>

### <a name="update-the-details-view"></a><span data-ttu-id="45c2b-230">Aktualizace zobrazení podrobností</span><span class="sxs-lookup"><span data-stu-id="45c2b-230">Update the Details view</span></span>

<span data-ttu-id="45c2b-231">Zobrazení podrobností aktualizujte, aby správci můžete schválit nebo odmítnout kontaktů:</span><span class="sxs-lookup"><span data-stu-id="45c2b-231">Update the details view so managers can approve or reject contacts:</span></span>

[!code-html[Main](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]

## <a name="test-the-completed-app"></a><span data-ttu-id="45c2b-232">Testování dokončená aplikace</span><span class="sxs-lookup"><span data-stu-id="45c2b-232">Test the completed app</span></span>

<span data-ttu-id="45c2b-233">Pokud používáte Visual Studio Code nebo testování na místní platforma, která neobsahuje testovací certifikát pro protokol SSL:</span><span class="sxs-lookup"><span data-stu-id="45c2b-233">If you are using Visual Studio Code or testing on local platform that doesn't include a test certificate for SSL:</span></span>

- <span data-ttu-id="45c2b-234">Nastavit `"LocalTest:skipSSL": true` v *appSettings.JSON určený* souboru.</span><span class="sxs-lookup"><span data-stu-id="45c2b-234">Set `"LocalTest:skipSSL": true` in the *appsettings.json* file.</span></span>

<span data-ttu-id="45c2b-235">Pokud jste spustili aplikaci a máte kontakty, odstraňte všechny záznamy v `Contact` tabulky a restartujte aplikaci počáteční hodnoty databáze.</span><span class="sxs-lookup"><span data-stu-id="45c2b-235">If you have run the app and have contacts, delete all the records in the `Contact` table and restart the app to seed the database.</span></span> <span data-ttu-id="45c2b-236">Pokud používáte Visual Studio, musíte ukončit a restartovat službu IIS Express na počáteční hodnoty databáze.</span><span class="sxs-lookup"><span data-stu-id="45c2b-236">If you are using Visual Studio, you need to exit and restart IIS Express to seed the database.</span></span>

<span data-ttu-id="45c2b-237">Zaregistrujte uživatelům procházet kontaktů.</span><span class="sxs-lookup"><span data-stu-id="45c2b-237">Register a user to browse the contacts.</span></span>

<span data-ttu-id="45c2b-238">Snadný způsob, jak testování dokončená aplikace je spustíte tři různé prohlížeče (nebo incognito/InPrivate verze).</span><span class="sxs-lookup"><span data-stu-id="45c2b-238">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="45c2b-239">V jedné prohlížeče registraci nového uživatele, například `test@example.com`.</span><span class="sxs-lookup"><span data-stu-id="45c2b-239">In one browser, register a new user, for example, `test@example.com`.</span></span> <span data-ttu-id="45c2b-240">Přihlaste se do každé prohlížeče s jiným uživatelem.</span><span class="sxs-lookup"><span data-stu-id="45c2b-240">Sign in to each browser with a different user.</span></span> <span data-ttu-id="45c2b-241">Ověřte následující:</span><span class="sxs-lookup"><span data-stu-id="45c2b-241">Verify the following:</span></span>

* <span data-ttu-id="45c2b-242">Registrovaní uživatelé můžete zobrazit všechny schválené kontaktní data.</span><span class="sxs-lookup"><span data-stu-id="45c2b-242">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="45c2b-243">Registrovaní uživatelé můžete upravit nebo odstranit svá vlastní data.</span><span class="sxs-lookup"><span data-stu-id="45c2b-243">Registered users can edit/delete their own data.</span></span> 
* <span data-ttu-id="45c2b-244">Správce můžete schválit nebo odmítnout kontaktní údaje.</span><span class="sxs-lookup"><span data-stu-id="45c2b-244">Managers can approve or reject contact data.</span></span> <span data-ttu-id="45c2b-245">`Details` Zobrazení ukazuje **schválit** a **odmítnout** tlačítka.</span><span class="sxs-lookup"><span data-stu-id="45c2b-245">The `Details` view shows **Approve** and **Reject** buttons.</span></span> 
* <span data-ttu-id="45c2b-246">Správci mohou schválit či odmítnout a upravit nebo odstranit všechna data.</span><span class="sxs-lookup"><span data-stu-id="45c2b-246">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="45c2b-247">Uživatel</span><span class="sxs-lookup"><span data-stu-id="45c2b-247">User</span></span>| <span data-ttu-id="45c2b-248">Možnosti</span><span class="sxs-lookup"><span data-stu-id="45c2b-248">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="45c2b-249">Můžete upravit nebo odstranit vlastní data</span><span class="sxs-lookup"><span data-stu-id="45c2b-249">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="45c2b-250">Můžete schválit nebo odmítnout a upravit nebo odstranit vlastní data</span><span class="sxs-lookup"><span data-stu-id="45c2b-250">Can approve/reject and edit/delete own data</span></span>  |
| admin@contoso.com | <span data-ttu-id="45c2b-251">Můžete upravit nebo odstranit a schválit či odmítnout všechna data</span><span class="sxs-lookup"><span data-stu-id="45c2b-251">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="45c2b-252">Vytvoření kontaktu v prohlížeči správci.</span><span class="sxs-lookup"><span data-stu-id="45c2b-252">Create a contact in the administrators browser.</span></span> <span data-ttu-id="45c2b-253">Zkopírujte adresu URL pro odstranění a upravit z kontaktujte správce.</span><span class="sxs-lookup"><span data-stu-id="45c2b-253">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="45c2b-254">Tyto odkazy vložte do prohlížeče se zobrazil uživatel k ověření, že se zobrazil uživatel nemůže provést tyto operace.</span><span class="sxs-lookup"><span data-stu-id="45c2b-254">Paste these links into the test user's browser to verify the test user cannot perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="45c2b-255">Vytvořit úvodní aplikaci</span><span class="sxs-lookup"><span data-stu-id="45c2b-255">Create the starter app</span></span>

<span data-ttu-id="45c2b-256">Postupujte podle těchto pokynů můžete vytvořit úvodní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="45c2b-256">Follow these instructions to create the starter app.</span></span>

* <span data-ttu-id="45c2b-257">Vytvoření **webové aplikace ASP.NET Core** pomocí [Visual Studio 2017](https://www.visualstudio.com/) s názvem "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="45c2b-257">Create an **ASP.NET Core Web Application** using [Visual Studio 2017](https://www.visualstudio.com/) named "ContactManager"</span></span>

  * <span data-ttu-id="45c2b-258">Vytvoření aplikace s **jednotlivých uživatelských účtů**.</span><span class="sxs-lookup"><span data-stu-id="45c2b-258">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="45c2b-259">Pojmenujte ji "ContactManager", vašeho oboru názvů bude odpovídat oboru názvů používají ve vzorku.</span><span class="sxs-lookup"><span data-stu-id="45c2b-259">Name it "ContactManager" so your namespace will match the namespace use in the sample.</span></span>

* <span data-ttu-id="45c2b-260">Přidejte následující `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="45c2b-260">Add the following `Contact` model:</span></span>

  [!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="45c2b-261">Vygenerované uživatelské rozhraní `Contact` model pomocí Entity Framework Core a `ApplicationDbContext` data kontextu.</span><span class="sxs-lookup"><span data-stu-id="45c2b-261">Scaffold the `Contact` model using Entity Framework Core and the `ApplicationDbContext` data context.</span></span> <span data-ttu-id="45c2b-262">Přijměte všechny výchozí hodnoty generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="45c2b-262">Accept all the scaffolding defaults.</span></span> <span data-ttu-id="45c2b-263">Pomocí `ApplicationDbContext` pro kontext dat třída vloží tabulky kontaktů [Identity](xref:security/authentication/identity) databáze.</span><span class="sxs-lookup"><span data-stu-id="45c2b-263">Using `ApplicationDbContext` for the data context class  puts the contact table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="45c2b-264">V tématu [přidání model](xref:tutorials/first-mvc-app/adding-model) Další informace.</span><span class="sxs-lookup"><span data-stu-id="45c2b-264">See [Adding a model](xref:tutorials/first-mvc-app/adding-model) for more information.</span></span>

* <span data-ttu-id="45c2b-265">Aktualizace **ContactManager** ukotvení v *Views/Shared/_Layout.cshtml* souboru z `asp-controller="Home"` k `asp-controller="Contacts"` tak klepnutím **ContactManager** odkaz Vyvolá řadičem kontakty.</span><span class="sxs-lookup"><span data-stu-id="45c2b-265">Update the **ContactManager** anchor in the *Views/Shared/_Layout.cshtml* file from `asp-controller="Home"` to `asp-controller="Contacts"` so tapping the **ContactManager** link will invoke the Contacts controller.</span></span> <span data-ttu-id="45c2b-266">Původní kód:</span><span class="sxs-lookup"><span data-stu-id="45c2b-266">The original markup:</span></span>

```html
   <a asp-area="" asp-controller="Home" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

<span data-ttu-id="45c2b-267">Aktualizované značek:</span><span class="sxs-lookup"><span data-stu-id="45c2b-267">The updated markup:</span></span>

```html
   <a asp-area="" asp-controller="Contacts" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

* <span data-ttu-id="45c2b-268">Vygenerovat počáteční migraci a aktualizaci databáze</span><span class="sxs-lookup"><span data-stu-id="45c2b-268">Scaffold the initial migration and update the database</span></span>

```none
   dotnet ef migrations add initial
   dotnet ef database update
   ```

* <span data-ttu-id="45c2b-269">Aplikaci otestovat a vytváření, úpravy a odstranění kontaktu</span><span class="sxs-lookup"><span data-stu-id="45c2b-269">Test the app by creating, editing and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="45c2b-270">Počáteční hodnoty databáze</span><span class="sxs-lookup"><span data-stu-id="45c2b-270">Seed the database</span></span>

<span data-ttu-id="45c2b-271">Přidat `SeedData` třídy k *Data* složky.</span><span class="sxs-lookup"><span data-stu-id="45c2b-271">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="45c2b-272">Pokud jste stáhli ukázku, můžete zkopírovat *SeedData.cs* do souboru *Data* složky starter projektu.</span><span class="sxs-lookup"><span data-stu-id="45c2b-272">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

[!code-csharp[Main](secure-data/samples/starter/Data/SeedData.cs)]

<span data-ttu-id="45c2b-273">Zvýrazněný kód přidejte na konec `Configure` metoda v *Startup.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="45c2b-273">Add the highlighted code to the end of the `Configure` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]

<span data-ttu-id="45c2b-274">Otestujte, že aplikace nasadí databázi.</span><span class="sxs-lookup"><span data-stu-id="45c2b-274">Test that the app seeded the database.</span></span> <span data-ttu-id="45c2b-275">Metoda počáteční hodnoty nejde spustit, pokud jsou všechny řádky v kontakt DB.</span><span class="sxs-lookup"><span data-stu-id="45c2b-275">The seed method does not run if there are any rows in the contact DB.</span></span>

### <a name="create-a-class-used-in-the-tutorial"></a><span data-ttu-id="45c2b-276">Vytvoření třídy v tomto kurzu použili</span><span class="sxs-lookup"><span data-stu-id="45c2b-276">Create a class used in the tutorial</span></span>

* <span data-ttu-id="45c2b-277">Vytvořte složku s názvem *autorizace*.</span><span class="sxs-lookup"><span data-stu-id="45c2b-277">Create a folder named *Authorization*.</span></span>
* <span data-ttu-id="45c2b-278">Kopírování *Authorization\ContactOperations.cs* soubor ze stažení dokončené projektu nebo zkopírujte následující kód:</span><span class="sxs-lookup"><span data-stu-id="45c2b-278">Copy the *Authorization\ContactOperations.cs* file from the completed project download, or copy the following code:</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="45c2b-279">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="45c2b-279">Additional resources</span></span>

* <span data-ttu-id="45c2b-280">[Prostředí ASP.NET Core autorizace](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="45c2b-280">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="45c2b-281">Toto testovací prostředí obsahuje více podrobností o funkcích zabezpečení byla zavedená v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="45c2b-281">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="45c2b-282">Autorizace v ASP.NET Core: jednoduchý, na základě deklarace a vlastních rolí</span><span class="sxs-lookup"><span data-stu-id="45c2b-282">Authorization in ASP.NET Core : Simple, role, claims-based and custom</span></span>](index.md)
* [<span data-ttu-id="45c2b-283">Autorizace uživatele na základě zásad</span><span class="sxs-lookup"><span data-stu-id="45c2b-283">Custom policy-based authorization</span></span>](policies.md)
