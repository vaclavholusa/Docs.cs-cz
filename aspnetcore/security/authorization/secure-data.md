---
title: "Vytvoření aplikace ASP.NET Core s uživatelskými daty chráněn autorizace"
author: rick-anderson
keywords: "Jádro ASP.NET, MVC, autorizace, role, zabezpečení, správce"
ms.author: riande
manager: wpickett
ms.date: 05/22/2017
ms.topic: article
ms.assetid: abeb2f8e-dfbf-4398-a04c-338a613a65bc
ms.technology: aspnet
ms.prod: aspnet-core
uid: security/authorization/secure-data
ms.openlocfilehash: 8eeb5d71575fd819239da6dd63dd31e323fb0556
ms.sourcegitcommit: 96af03c9f44f7c206e68ae3ef8596068e6b4e5fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="4f5a6-103">Vytvoření aplikace ASP.NET Core s uživatelskými daty chráněn autorizace</span><span class="sxs-lookup"><span data-stu-id="4f5a6-103">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="4f5a6-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Audette Jan](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="4f5a6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="4f5a6-105">Tento kurz ukazuje, jak vytvořit webovou aplikaci s uživatelskými daty chráněn autorizace.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-105">This tutorial shows how to create a web app with user data protected by authorization.</span></span> <span data-ttu-id="4f5a6-106">Zobrazí seznam kontakty, které ověřené uživatele (registrovaný) jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-106">It  displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="4f5a6-107">Existují tři skupiny zabezpečení:</span><span class="sxs-lookup"><span data-stu-id="4f5a6-107">There are three security groups:</span></span>

* <span data-ttu-id="4f5a6-108">Registrovaní uživatelé můžete zobrazit všechny schválené kontaktní data.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-108">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="4f5a6-109">Registrovaní uživatelé můžete upravit nebo odstranit svá vlastní data.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-109">Registered users can edit/delete their own data.</span></span> 
* <span data-ttu-id="4f5a6-110">Správce můžete schválit nebo odmítnout kontaktní údaje.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-110">Managers can approve or reject contact data.</span></span> <span data-ttu-id="4f5a6-111">Pouze schválené kontakty jsou viditelné pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-111">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="4f5a6-112">Správci mohou schválit či odmítnout a upravit nebo odstranit všechna data.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-112">Administrators can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="4f5a6-113">Na následujícím obrázku, uživatel Rick (`rick@example.com`) je přihlášený.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-113">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="4f5a6-114">Jenom zobrazení může uživatel Rick schválena kontakty a upravit nebo odstranit jeho kontakty.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-114">User Rick can only view approved contacts and edit/delete his contacts.</span></span> <span data-ttu-id="4f5a6-115">Pouze poslední záznam vytvořené Rick, zobrazí upravit a odstranit odkazy</span><span class="sxs-lookup"><span data-stu-id="4f5a6-115">Only the last record, created by Rick, displays edit and delete links</span></span>

![Obrázek popsané výše](secure-data/_static/rick.png)

<span data-ttu-id="4f5a6-117">Na následujícím obrázku `manager@contoso.com` je přihlášen a v roli správce.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-117">In the following image, `manager@contoso.com` is signed in and in the managers role.</span></span> 

![Obrázek popsané výše](secure-data/_static/manager1.png)

<span data-ttu-id="4f5a6-119">Následující obrázek ukazuje vybraných manažerů, zobrazení podrobností kontaktu.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-119">The following image shows the  managers details view of a contact.</span></span>

![Obrázek popsané výše](secure-data/_static/manager.png)

<span data-ttu-id="4f5a6-121">Pouze správci a správci mají schválit a budou odmítat tlačítka.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-121">Only managers and administrators have the approve and reject buttons.</span></span>

<span data-ttu-id="4f5a6-122">Na následujícím obrázku `admin@contoso.com` je přihlášen a v roli správce.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-122">In the following image, `admin@contoso.com` is signed in and in the administrator’s role.</span></span> 

![Obrázek popsané výše](secure-data/_static/admin.png)

<span data-ttu-id="4f5a6-124">Správce má všechna oprávnění.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-124">The administrator has all privileges.</span></span> <span data-ttu-id="4f5a6-125">Jana můžete pro čtení, úpravy nebo odstranění všechny kontakty a změnit stav kontaktů.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-125">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="4f5a6-126">Aplikace byla vytvořená [generování uživatelského rozhraní](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) následující `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="4f5a6-126">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller)  the following `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="4f5a6-127">A `ContactIsOwnerAuthorizationHandler` obslužná rutina ověřování zajišťuje uživatele můžete upravit pouze svá data.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-127">A `ContactIsOwnerAuthorizationHandler` authorization handler ensures that a user can only edit their data.</span></span> <span data-ttu-id="4f5a6-128">A `ContactManagerAuthorizationHandler` obslužná rutina ověřování umožňuje správcům schválit nebo odmítnout kontakty.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-128">A `ContactManagerAuthorizationHandler` authorization handler allows managers to approve or reject contacts.</span></span>  <span data-ttu-id="4f5a6-129">A `ContactAdministratorsAuthorizationHandler` obslužná rutina ověřování umožňuje správcům schválit nebo odmítnout kontakty a upravit nebo odstranit kontakty.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-129">A `ContactAdministratorsAuthorizationHandler` authorization handler allows administrators to approve or reject contacts and to edit/delete contacts.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="4f5a6-130">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4f5a6-130">Prerequisites</span></span>

<span data-ttu-id="4f5a6-131">Toto není začátku kurzu.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-131">This is not a beginning tutorial.</span></span> <span data-ttu-id="4f5a6-132">Měli byste se seznámit s:</span><span class="sxs-lookup"><span data-stu-id="4f5a6-132">You should be familiar with:</span></span>

* [<span data-ttu-id="4f5a6-133">Jádro ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="4f5a6-133">ASP.NET Core MVC</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="4f5a6-134">Základní Entity Framework</span><span class="sxs-lookup"><span data-stu-id="4f5a6-134">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="4f5a6-135">Spuštění a dokončené aplikace</span><span class="sxs-lookup"><span data-stu-id="4f5a6-135">The starter and completed app</span></span>

<span data-ttu-id="4f5a6-136">[Stáhněte si](xref:tutorials/index#how-to-download-a-sample) [Dokončit](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-136">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) app.</span></span> <span data-ttu-id="4f5a6-137">[Test](#test-the-completed-app) dokončené aplikace, takže seznámit se s jeho funkce zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-137">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span> 

### <a name="the-starter-app"></a><span data-ttu-id="4f5a6-138">Úvodní aplikaci</span><span class="sxs-lookup"><span data-stu-id="4f5a6-138">The starter app</span></span>

<span data-ttu-id="4f5a6-139">Je užitečné k porovnání s je hotová ukázka kódu.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-139">It's helpful to compare your code with the completed sample.</span></span>

<span data-ttu-id="4f5a6-140">[Stáhněte si](xref:tutorials/index#how-to-download-a-sample) [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-140">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) app.</span></span> 

<span data-ttu-id="4f5a6-141">V tématu [vytvořit úvodní aplikaci](#create-the-starter-app) Pokud chcete vytvořit od začátku.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-141">See [Create the starter app](#create-the-starter-app) if you'd like to create it from scratch.</span></span>

<span data-ttu-id="4f5a6-142">Aktualizace databáze:</span><span class="sxs-lookup"><span data-stu-id="4f5a6-142">Update the database:</span></span>

```none
   dotnet ef database update
```

<span data-ttu-id="4f5a6-143">Spusťte aplikaci, klepněte **ContactManager** propojit a ověřte, můžete vytvořit, upravit a odstranit a obraťte se na.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-143">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

<span data-ttu-id="4f5a6-144">V tomto kurzu má všechny hlavní kroky k vytvoření aplikace dat zabezpečení uživatele.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-144">This tutorial has all the major steps to create the secure user data app.</span></span> <span data-ttu-id="4f5a6-145">Vám může být užitečné k odkazování na dokončený projekt.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-145">You may find it helpful to refer to the completed project.</span></span>

## <a name="modify-the-app-to-secure-user-data"></a><span data-ttu-id="4f5a6-146">Upravit aplikaci k zabezpečení dat uživatele</span><span class="sxs-lookup"><span data-stu-id="4f5a6-146">Modify the app to secure user data</span></span>

<span data-ttu-id="4f5a6-147">V následujících částech mít všechny hlavní kroky k vytvoření aplikace dat zabezpečení uživatele.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-147">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="4f5a6-148">Vám může být užitečné k odkazování na dokončený projekt.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-148">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="4f5a6-149">Tie – kontaktních údajů pro uživatele</span><span class="sxs-lookup"><span data-stu-id="4f5a6-149">Tie the contact data to the user</span></span>

<span data-ttu-id="4f5a6-150">Pomocí technologie ASP.NET [Identity](xref:security/authentication/identity) ID uživatele, aby uživatelé můžete upravit svá data, ale ne další data uživatele.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-150">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="4f5a6-151">Přidat `OwnerID` k `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="4f5a6-151">Add `OwnerID` to the `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]

<span data-ttu-id="4f5a6-152">`OwnerID`ID uživatele z `AspNetUser` tabulky v [Identity](xref:security/authentication/identity) databáze.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-152">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="4f5a6-153">`Status` Pole určuje, zda kontakt zobrazit obecné uživatele.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-153">The `Status` field determines if a contact is viewable by general users.</span></span> 

<span data-ttu-id="4f5a6-154">Vygenerovat nový migraci a aktualizaci databáze:</span><span class="sxs-lookup"><span data-stu-id="4f5a6-154">Scaffold a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
 ```

### <a name="require-ssl-and-authenticated-users"></a><span data-ttu-id="4f5a6-155">Vyžadovat protokol SSL a ověření uživatelé</span><span class="sxs-lookup"><span data-stu-id="4f5a6-155">Require SSL and authenticated users</span></span>

<span data-ttu-id="4f5a6-156">V `ConfigureServices` metodu *Startup.cs* soubor, přidejte [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) filtr autorizace:</span><span class="sxs-lookup"><span data-stu-id="4f5a6-156">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) authorization filter:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]

<span data-ttu-id="4f5a6-157">Přesměrování požadavků HTTP do HTTPS, najdete v části [URL přepisování Middleware](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="4f5a6-157">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="4f5a6-158">Pokud používáte Visual Studio Code nebo testování na místní platforma, která neobsahuje testovací certifikát pro protokol SSL:</span><span class="sxs-lookup"><span data-stu-id="4f5a6-158">If you are using Visual Studio Code or testing on local platform that doesn't include a test certificate for SSL:</span></span>

- <span data-ttu-id="4f5a6-159">Nastavit `"LocalTest:skipSSL": true` v *appSettings.JSON určený* souboru.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-159">Set `"LocalTest:skipSSL": true` in the *appsettings.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="4f5a6-160">Vyžadovat ověření uživatelé</span><span class="sxs-lookup"><span data-stu-id="4f5a6-160">Require authenticated users</span></span>

<span data-ttu-id="4f5a6-161">Nastavte výchozí zásady ověřování tak, aby vyžadovala uživatele k ověření.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-161">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="4f5a6-162">Můžete vyjádření výslovného nesouhlasu ověřování ke kontroleru nebo metodě akce s `[AllowAnonymous]` atribut.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-162">You can opt out of authentication at the controller or action method with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="4f5a6-163">S tímto přístupem všech nových řadičů přidán automaticky vyžadují ověřování, což je bezpečnější než spoléhat na nových řadičů zahrnout `[Authorize]` atribut.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-163">With this approach, any new controllers added will automatically require authentication, which is safer than relying on new controllers to include the `[Authorize]` attribute.</span></span> <span data-ttu-id="4f5a6-164">Přidejte následující `ConfigureServices` metodu *Startup.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="4f5a6-164">Add the following to  the `ConfigureServices` method of the *Startup.cs* file:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]

<span data-ttu-id="4f5a6-165">Přidat `[AllowAnonymous]` domácí řadiče, mohou anonymní uživatelé získat informace o lokalitě, před jejich registraci.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-165">Add `[AllowAnonymous]` to the home controller so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="4f5a6-166">Nakonfigurujte účet pro test</span><span class="sxs-lookup"><span data-stu-id="4f5a6-166">Configure the test account</span></span>

<span data-ttu-id="4f5a6-167">`SeedData` Třída vytvoří dva účty, správce a správce.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-167">The `SeedData` class creates two accounts,  administrator and manager.</span></span> <span data-ttu-id="4f5a6-168">Použití [nástroj tajný klíč správce](xref:security/app-secrets) nastavení hesla pro tyto účty.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-168">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="4f5a6-169">To můžete udělat z adresáře projektu (adresář obsahující *Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="4f5a6-169">Do this from the project directory (the directory containing *Program.cs*).</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="4f5a6-170">Aktualizace `Configure` používat heslo testu:</span><span class="sxs-lookup"><span data-stu-id="4f5a6-170">Update `Configure` to use the test password:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]

<span data-ttu-id="4f5a6-171">Přidat ID uživatele správce a `Status = ContactStatus.Approved` kontaktům.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-171">Add the administrator user ID and `Status = ContactStatus.Approved` to the contacts.</span></span> <span data-ttu-id="4f5a6-172">ID uživatele se zobrazí pouze jeden kontakt, přidejte všechny kontakty:</span><span class="sxs-lookup"><span data-stu-id="4f5a6-172">Only one contact is shown, add the user ID to all contacts:</span></span>

[!code-csharp[Main](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="4f5a6-173">Vytvoření vlastníka, správce a Správce autorizací obslužné rutiny</span><span class="sxs-lookup"><span data-stu-id="4f5a6-173">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="4f5a6-174">Vytvoření `ContactIsOwnerAuthorizationHandler` třídy v *autorizace* složky.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-174">Create a `ContactIsOwnerAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="4f5a6-175">`ContactIsOwnerAuthorizationHandler` Ověří uživatele, který funguje na prostředku vlastníkem prostředku.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-175">The `ContactIsOwnerAuthorizationHandler` will verify the user acting on the resource owns the resource.</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="4f5a6-176">`ContactIsOwnerAuthorizationHandler` Volání `context.Succeed` Pokud ověřený aktuální uživatel je jeho vlastníkem.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-176">The `ContactIsOwnerAuthorizationHandler` calls `context.Succeed` if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="4f5a6-177">Obslužné rutiny autorizace obecně vrátit `context.Succeed` Pokud jsou splněny požadavky na.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-177">Authorization handlers generally return `context.Succeed` when the requirements are met.</span></span> <span data-ttu-id="4f5a6-178">Vracejí `Task.FromResult(0)` Pokud nejsou splněné požadavky.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-178">They return `Task.FromResult(0)` when requirements are not met.</span></span> <span data-ttu-id="4f5a6-179">`Task.FromResult(0)`je ani úspěch nebo neúspěch, umožňuje jiných autorizace obslužná rutina spouštěla.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-179">`Task.FromResult(0)` is neither success or failure, it allows other authorization handler to run.</span></span> <span data-ttu-id="4f5a6-180">Pokud potřebujete explicitně nezdaří, vrátí `context.Fail()`.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-180">If you need to explicitly fail, return `context.Fail()`.</span></span>

<span data-ttu-id="4f5a6-181">Jsme vlastníky kontaktní upravit nebo odstranit svá vlastní data, není třeba zkontrolujte provozní předán v parametru požadavek.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-181">We allow contact owners to edit/delete their own data, so we don't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="4f5a6-182">Vytvořte obslužnou rutinu Správce autorizací</span><span class="sxs-lookup"><span data-stu-id="4f5a6-182">Create a manager authorization handler</span></span>

<span data-ttu-id="4f5a6-183">Vytvoření `ContactManagerAuthorizationHandler` třídy v *autorizace* složky.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-183">Create a `ContactManagerAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="4f5a6-184">`ContactManagerAuthorizationHandler` Ověří uživatele, který funguje na prostředku je správce.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-184">The `ContactManagerAuthorizationHandler` will verify the user acting on the resource is a manager.</span></span> <span data-ttu-id="4f5a6-185">Pouze správci můžete schválit nebo odmítnout změny obsahu (nové nebo změněné).</span><span class="sxs-lookup"><span data-stu-id="4f5a6-185">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="4f5a6-186">Vytvoření obslužné rutiny Správce autorizací</span><span class="sxs-lookup"><span data-stu-id="4f5a6-186">Create an administrator authorization handler</span></span>

<span data-ttu-id="4f5a6-187">Vytvoření `ContactAdministratorsAuthorizationHandler` třídy v *autorizace* složky.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-187">Create a `ContactAdministratorsAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="4f5a6-188">`ContactAdministratorsAuthorizationHandler` Ověří, jestli uživatel, který funguje na prostředku je správcem.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-188">The `ContactAdministratorsAuthorizationHandler` will verify the user acting on the resource is a administrator.</span></span> <span data-ttu-id="4f5a6-189">Správce může provádět všechny operace.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-189">Administrator can do all operations.</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="4f5a6-190">Registraci obslužných rutin autorizace</span><span class="sxs-lookup"><span data-stu-id="4f5a6-190">Register the authorization handlers</span></span>

<span data-ttu-id="4f5a6-191">Musí být pro registrované služby pomocí Entity Framework Core [vkládání závislostí](xref:fundamentals/dependency-injection) pomocí [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="4f5a6-191">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="4f5a6-192">`ContactIsOwnerAuthorizationHandler` Používá ASP.NET Core [Identity](xref:security/authentication/identity), který je založený na Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-192">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="4f5a6-193">Zaregistrujte obslužných rutin s kolekcí služby, aby byly k dispozici `ContactsController` prostřednictvím [vkládání závislostí](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="4f5a6-193">Register the handlers with the service collection so they will be available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="4f5a6-194">Přidejte následující kód do konce `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="4f5a6-194">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]

<span data-ttu-id="4f5a6-195">`ContactAdministratorsAuthorizationHandler`a `ContactManagerAuthorizationHandler` jsou přidány jako jednotlivé prvky.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-195">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="4f5a6-196">Jsou jednotlivé prvky, protože se nepoužívají EF a veškeré informace potřebné v `Context` parametr `HandleRequirementAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-196">They are singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

<span data-ttu-id="4f5a6-197">Kompletní `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="4f5a6-197">The complete `ConfigureServices`:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=ConfigureServices)]

## <a name="update-the-code-to-support-authorization"></a><span data-ttu-id="4f5a6-198">Aktualizujte kód pro podporu ověřování</span><span class="sxs-lookup"><span data-stu-id="4f5a6-198">Update the code to support authorization</span></span>

<span data-ttu-id="4f5a6-199">V této části aktualizace řadiče a zobrazení a přidejte třídu operations požadavky.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-199">In this section, you update the controller and views and add an operations requirements class.</span></span>

### <a name="update-the-contacts-controller"></a><span data-ttu-id="4f5a6-200">Aktualizaci řadiče kontaktů</span><span class="sxs-lookup"><span data-stu-id="4f5a6-200">Update the Contacts controller</span></span>

<span data-ttu-id="4f5a6-201">Aktualizace `ContactsController` konstruktor:</span><span class="sxs-lookup"><span data-stu-id="4f5a6-201">Update the `ContactsController` constructor:</span></span>

* <span data-ttu-id="4f5a6-202">Přidat `IAuthorizationService` služby s přístupem k autorizaci obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-202">Add the `IAuthorizationService` service to  access to the authorization handlers.</span></span> 
* <span data-ttu-id="4f5a6-203">Přidat `Identity` `UserManager` služby:</span><span class="sxs-lookup"><span data-stu-id="4f5a6-203">Add the `Identity` `UserManager` service:</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]

### <a name="add-a-contact-operations-requirements-class"></a><span data-ttu-id="4f5a6-204">Přidání třídy požadavky kontaktní operací</span><span class="sxs-lookup"><span data-stu-id="4f5a6-204">Add a contact operations requirements class</span></span>

<span data-ttu-id="4f5a6-205">Přidat `ContactOperations` třídy k *autorizace* složky.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-205">Add the `ContactOperations` class to the *Authorization* folder.</span></span> <span data-ttu-id="4f5a6-206">Tato třída obsahovat požadavky na naše aplikace podporuje:</span><span class="sxs-lookup"><span data-stu-id="4f5a6-206">This class  contain the requirements our app supports:</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

### <a name="update-create"></a><span data-ttu-id="4f5a6-207">Vytvoření aktualizace</span><span class="sxs-lookup"><span data-stu-id="4f5a6-207">Update Create</span></span>

<span data-ttu-id="4f5a6-208">Aktualizace `HTTP POST Create` metodu:</span><span class="sxs-lookup"><span data-stu-id="4f5a6-208">Update the `HTTP POST Create` method to:</span></span>

* <span data-ttu-id="4f5a6-209">Přidat ID uživatele `Contact` modelu.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-209">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="4f5a6-210">Obslužná rutina ověřování k ověření, že uživatel kontakt vlastní volání.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-210">Call the authorization handler to verify the user owns the contact.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]

### <a name="update-edit"></a><span data-ttu-id="4f5a6-211">Upravit aktualizaci</span><span class="sxs-lookup"><span data-stu-id="4f5a6-211">Update Edit</span></span>

<span data-ttu-id="4f5a6-212">Aktualizovat i `Edit` kontakt vlastní metody sloužící obslužná rutina ověřování k ověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-212">Update both `Edit` methods to use the authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="4f5a6-213">Protože jsme se provádí autorizace prostředků nemůžeme použít `[Authorize]` atribut.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-213">Because we are performing resource authorization we cannot use the `[Authorize]` attribute.</span></span> <span data-ttu-id="4f5a6-214">Při hodnocení atributy nemáme přístup k prostředku.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-214">We don't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="4f5a6-215">Autorizace prostředků na základě musí být nutné.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-215">Resource based authorization must be imperative.</span></span> <span data-ttu-id="4f5a6-216">Jakmile jsme načtením v kontroleru nebo načtením v rámci samotné obslužná rutina mít přístup k prostředku, musí provést kontroly.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-216">Checks must be performed once we have access to the resource, either by loading it in our controller, or by loading it within the handler itself.</span></span> <span data-ttu-id="4f5a6-217">Často se přístup k prostředku předáním v klíči prostředků.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-217">Frequently you will access the resource by passing in the resource key.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]

### <a name="update-the-delete-method"></a><span data-ttu-id="4f5a6-218">Aktualizujte metodu Delete</span><span class="sxs-lookup"><span data-stu-id="4f5a6-218">Update the Delete method</span></span>

<span data-ttu-id="4f5a6-219">Aktualizovat i `Delete` kontakt vlastní metody sloužící obslužná rutina ověřování k ověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-219">Update both `Delete` methods to use the authorization handler to verify the user owns the contact.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="4f5a6-220">Vložit služby autorizace do zobrazení</span><span class="sxs-lookup"><span data-stu-id="4f5a6-220">Inject the authorization service into the views</span></span>

<span data-ttu-id="4f5a6-221">Uživatelské rozhraní zobrazí aktuálně upravovat a odstraňovat odkazy na data, která uživatele nelze upravit.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-221">Currently the UI shows edit and delete links for data the user cannot modify.</span></span> <span data-ttu-id="4f5a6-222">To jsme budete opravíme použitím obslužná rutina ověřování na zobrazení.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-222">We'll fix that by applying the authorization handler to the views.</span></span>

<span data-ttu-id="4f5a6-223">Vložit služby ověřování *Views/_ViewImports.cshtml* souboru, bude k dispozici u všech zobrazení:</span><span class="sxs-lookup"><span data-stu-id="4f5a6-223">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it will be available to all views:</span></span>

[!code-html[Main](secure-data/samples/final/Views/_ViewImports.cshtml)]

<span data-ttu-id="4f5a6-224">Aktualizace *Views/Contacts/Index.cshtml* zobrazení syntaxe Razor pouze zobrazení upravit a odstranit odkazy pro uživatele, kteří mohou upravit nebo odstranit kontakt.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-224">Update the *Views/Contacts/Index.cshtml* Razor view to only display the edit and delete links for users who can edit/delete the contact.</span></span>

<span data-ttu-id="4f5a6-225">Přidat`@using ContactManager.Authorization;`</span><span class="sxs-lookup"><span data-stu-id="4f5a6-225">Add `@using ContactManager.Authorization;`</span></span>

<span data-ttu-id="4f5a6-226">Aktualizace `Edit` a `Delete` odkazy tak vykresleny pouze pro uživatele s oprávnění upravovat a odstraňovat kontakt.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-226">Update the `Edit` and `Delete` links so they are only rendered for users with permission to edit and delete the contact.</span></span>

[!code-html[Main](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]

<span data-ttu-id="4f5a6-227">Upozornění: Skrytí odkazy z uživatelů, kteří nemají oprávnění k úpravě nebo odstranění dat nezabezpečuje aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-227">Warning: Hiding links from users that do not have permission to edit or delete data does not secure the app.</span></span> <span data-ttu-id="4f5a6-228">Skrytí odkazy díky aplikaci další uživatele popisný zobrazením pouze platné odkazy.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-228">Hiding links makes the app more user friendly by displaying only valid links.</span></span> <span data-ttu-id="4f5a6-229">Uživatelé mohou zabezpečení generované adresy URL pro vyvolání upravit a odstranit operací na data, která budou nevlastníte.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-229">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span>  <span data-ttu-id="4f5a6-230">Kontroler nutné opakovat, že přístup kontroly zabezpečená.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-230">The controller must repeat the access checks to be secure.</span></span>

### <a name="update-the-details-view"></a><span data-ttu-id="4f5a6-231">Aktualizace zobrazení podrobností</span><span class="sxs-lookup"><span data-stu-id="4f5a6-231">Update the Details view</span></span>

<span data-ttu-id="4f5a6-232">Zobrazení podrobností aktualizujte, aby správci můžete schválit nebo odmítnout kontaktů:</span><span class="sxs-lookup"><span data-stu-id="4f5a6-232">Update the details view so managers can approve or reject contacts:</span></span>

[!code-html[Main](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]

## <a name="test-the-completed-app"></a><span data-ttu-id="4f5a6-233">Testování dokončená aplikace</span><span class="sxs-lookup"><span data-stu-id="4f5a6-233">Test the completed app</span></span>

<span data-ttu-id="4f5a6-234">Pokud používáte Visual Studio Code nebo testování na místní platforma, která neobsahuje testovací certifikát pro protokol SSL:</span><span class="sxs-lookup"><span data-stu-id="4f5a6-234">If you are using Visual Studio Code or testing on local platform that doesn't include a test certificate for SSL:</span></span>

- <span data-ttu-id="4f5a6-235">Nastavit `"LocalTest:skipSSL": true` v *appSettings.JSON určený* souboru.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-235">Set `"LocalTest:skipSSL": true` in the *appsettings.json* file.</span></span>

<span data-ttu-id="4f5a6-236">Pokud jste spustili aplikaci a máte kontakty, odstraňte všechny záznamy v `Contact` tabulky a restartujte aplikaci počáteční hodnoty databáze.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-236">If you have run the app and have contacts, delete all the records in the `Contact` table and restart the app to seed the database.</span></span> <span data-ttu-id="4f5a6-237">Pokud používáte Visual Studio, musíte ukončit a restartovat službu IIS Express na počáteční hodnoty databáze.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-237">If you are using Visual Studio, you need to exit and restart IIS Express to seed the database.</span></span>

<span data-ttu-id="4f5a6-238">Zaregistrujte uživatelům procházet kontaktů.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-238">Register a user to browse the contacts.</span></span>

<span data-ttu-id="4f5a6-239">Snadný způsob, jak testování dokončená aplikace je spustíte tři různé prohlížeče (nebo incognito/InPrivate verze).</span><span class="sxs-lookup"><span data-stu-id="4f5a6-239">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="4f5a6-240">V jedné prohlížeče registraci nového uživatele, například `test@example.com`.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-240">In one browser, register a new user, for example, `test@example.com`.</span></span> <span data-ttu-id="4f5a6-241">Přihlaste se do každé prohlížeče s jiným uživatelem.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-241">Sign in to each browser with a different user.</span></span> <span data-ttu-id="4f5a6-242">Ověřte následující:</span><span class="sxs-lookup"><span data-stu-id="4f5a6-242">Verify the following:</span></span>

* <span data-ttu-id="4f5a6-243">Registrovaní uživatelé můžete zobrazit všechny schválené kontaktní data.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-243">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="4f5a6-244">Registrovaní uživatelé můžete upravit nebo odstranit svá vlastní data.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-244">Registered users can edit/delete their own data.</span></span> 
* <span data-ttu-id="4f5a6-245">Správce můžete schválit nebo odmítnout kontaktní údaje.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-245">Managers can approve or reject contact data.</span></span> <span data-ttu-id="4f5a6-246">`Details` Zobrazení ukazuje **schválit** a **odmítnout** tlačítka.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-246">The `Details` view shows **Approve** and **Reject** buttons.</span></span> 
* <span data-ttu-id="4f5a6-247">Správci mohou schválit či odmítnout a upravit nebo odstranit všechna data.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-247">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="4f5a6-248">Uživatel</span><span class="sxs-lookup"><span data-stu-id="4f5a6-248">User</span></span>| <span data-ttu-id="4f5a6-249">Možnosti</span><span class="sxs-lookup"><span data-stu-id="4f5a6-249">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="4f5a6-250">Můžete upravit nebo odstranit vlastní data</span><span class="sxs-lookup"><span data-stu-id="4f5a6-250">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="4f5a6-251">Můžete schválit nebo odmítnout a upravit nebo odstranit vlastní data</span><span class="sxs-lookup"><span data-stu-id="4f5a6-251">Can approve/reject and edit/delete own data</span></span>  |
| admin@contoso.com | <span data-ttu-id="4f5a6-252">Můžete upravit nebo odstranit a schválit či odmítnout všechna data</span><span class="sxs-lookup"><span data-stu-id="4f5a6-252">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="4f5a6-253">Vytvoření kontaktu v prohlížeči správci.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-253">Create a contact in the administrators browser.</span></span> <span data-ttu-id="4f5a6-254">Zkopírujte adresu URL pro odstranění a upravit z kontaktujte správce.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-254">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="4f5a6-255">Tyto odkazy vložte do prohlížeče se zobrazil uživatel k ověření, že se zobrazil uživatel nemůže provést tyto operace.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-255">Paste these links into the test user's browser to verify the test user cannot perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="4f5a6-256">Vytvořit úvodní aplikaci</span><span class="sxs-lookup"><span data-stu-id="4f5a6-256">Create the starter app</span></span>

<span data-ttu-id="4f5a6-257">Postupujte podle těchto pokynů můžete vytvořit úvodní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-257">Follow these instructions to create the starter app.</span></span>

* <span data-ttu-id="4f5a6-258">Vytvoření **webové aplikace ASP.NET Core** pomocí [Visual Studio 2017](https://www.visualstudio.com/) s názvem "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="4f5a6-258">Create an **ASP.NET Core Web Application** using [Visual Studio 2017](https://www.visualstudio.com/) named "ContactManager"</span></span>

  * <span data-ttu-id="4f5a6-259">Vytvoření aplikace s **jednotlivých uživatelských účtů**.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-259">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="4f5a6-260">Pojmenujte ji "ContactManager", vašeho oboru názvů bude odpovídat oboru názvů používají ve vzorku.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-260">Name it "ContactManager" so your namespace will match the namespace use in the sample.</span></span>

* <span data-ttu-id="4f5a6-261">Přidejte následující `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="4f5a6-261">Add the following `Contact` model:</span></span>

  [!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="4f5a6-262">Vygenerované uživatelské rozhraní `Contact` model pomocí Entity Framework Core a `ApplicationDbContext` data kontextu.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-262">Scaffold the `Contact` model using Entity Framework Core and the `ApplicationDbContext` data context.</span></span> <span data-ttu-id="4f5a6-263">Přijměte všechny výchozí hodnoty generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-263">Accept all the scaffolding defaults.</span></span> <span data-ttu-id="4f5a6-264">Pomocí `ApplicationDbContext` pro kontext dat třída vloží tabulky kontaktů [Identity](xref:security/authentication/identity) databáze.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-264">Using `ApplicationDbContext` for the data context class  puts the contact table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="4f5a6-265">V tématu [přidání model](xref:tutorials/first-mvc-app/adding-model) Další informace.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-265">See [Adding a model](xref:tutorials/first-mvc-app/adding-model) for more information.</span></span>

* <span data-ttu-id="4f5a6-266">Aktualizace **ContactManager** ukotvení v *Views/Shared/_Layout.cshtml* souboru z `asp-controller="Home"` k `asp-controller="Contacts"` tak klepnutím **ContactManager** odkaz Vyvolá řadičem kontakty.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-266">Update the **ContactManager** anchor in the *Views/Shared/_Layout.cshtml* file from `asp-controller="Home"` to `asp-controller="Contacts"` so tapping the **ContactManager** link will invoke the Contacts controller.</span></span> <span data-ttu-id="4f5a6-267">Původní kód:</span><span class="sxs-lookup"><span data-stu-id="4f5a6-267">The original markup:</span></span>

```html
   <a asp-area="" asp-controller="Home" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

<span data-ttu-id="4f5a6-268">Aktualizované značek:</span><span class="sxs-lookup"><span data-stu-id="4f5a6-268">The updated markup:</span></span>

```html
   <a asp-area="" asp-controller="Contacts" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

* <span data-ttu-id="4f5a6-269">Vygenerovat počáteční migraci a aktualizaci databáze</span><span class="sxs-lookup"><span data-stu-id="4f5a6-269">Scaffold the initial migration and update the database</span></span>

```none
   dotnet ef migrations add initial
   dotnet ef database update
   ```

* <span data-ttu-id="4f5a6-270">Aplikaci otestovat a vytváření, úpravy a odstranění kontaktu</span><span class="sxs-lookup"><span data-stu-id="4f5a6-270">Test the app by creating, editing and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="4f5a6-271">Počáteční hodnoty databáze</span><span class="sxs-lookup"><span data-stu-id="4f5a6-271">Seed the database</span></span>

<span data-ttu-id="4f5a6-272">Přidat `SeedData` třídy k *Data* složky.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-272">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="4f5a6-273">Pokud jste stáhli ukázku, můžete zkopírovat *SeedData.cs* do souboru *Data* složky starter projektu.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-273">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

[!code-csharp[Main](secure-data/samples/starter/Data/SeedData.cs)]

<span data-ttu-id="4f5a6-274">Zvýrazněný kód přidejte na konec `Configure` metoda v *Startup.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="4f5a6-274">Add the highlighted code to the end of the `Configure` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]

<span data-ttu-id="4f5a6-275">Otestujte, že aplikace nasadí databázi.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-275">Test that the app seeded the database.</span></span> <span data-ttu-id="4f5a6-276">Metoda počáteční hodnoty nejde spustit, pokud jsou všechny řádky v kontakt DB.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-276">The seed method does not run if there are any rows in the contact DB.</span></span>

### <a name="create-a-class-used-in-the-tutorial"></a><span data-ttu-id="4f5a6-277">Vytvoření třídy v tomto kurzu použili</span><span class="sxs-lookup"><span data-stu-id="4f5a6-277">Create a class used in the tutorial</span></span>

* <span data-ttu-id="4f5a6-278">Vytvořte složku s názvem *autorizace*.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-278">Create a folder named *Authorization*.</span></span>
* <span data-ttu-id="4f5a6-279">Kopírování *Authorization\ContactOperations.cs* soubor ze stažení dokončené projektu nebo zkopírujte následující kód:</span><span class="sxs-lookup"><span data-stu-id="4f5a6-279">Copy the *Authorization\ContactOperations.cs* file from the completed project download, or copy the following code:</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="4f5a6-280">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="4f5a6-280">Additional resources</span></span>

* <span data-ttu-id="4f5a6-281">[Prostředí ASP.NET Core autorizace](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="4f5a6-281">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="4f5a6-282">Toto testovací prostředí obsahuje více podrobností o funkcích zabezpečení byla zavedená v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="4f5a6-282">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="4f5a6-283">Autorizace v ASP.NET Core: jednoduchý, na základě deklarace a vlastních rolí</span><span class="sxs-lookup"><span data-stu-id="4f5a6-283">Authorization in ASP.NET Core : Simple, role, claims-based and custom</span></span>](index.md)
* [<span data-ttu-id="4f5a6-284">Autorizace uživatele na základě zásad</span><span class="sxs-lookup"><span data-stu-id="4f5a6-284">Custom Policy-Based Authorization</span></span>](policies.md)
