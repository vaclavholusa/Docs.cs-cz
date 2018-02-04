---
title: "Vytvoření aplikace ASP.NET Core s uživatelskými daty chráněn autorizace"
author: rick-anderson
description: "Postup vytvoření aplikace pro stránky Razor s uživatelskými daty chráněn autorizace. Zahrnuje protokol SSL, ověřování, zabezpečení, ASP.NET Core Identity."
manager: wpickett
ms.author: riande
ms.date: 01/24/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/secure-data
ms.openlocfilehash: 6333082a2b2b4f6d3f1ce2afc600b4203a0f5dca
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/03/2018
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="7e4b9-104">Vytvoření aplikace ASP.NET Core s uživatelskými daty chráněn autorizace</span><span class="sxs-lookup"><span data-stu-id="7e4b9-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="7e4b9-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Audette Jan](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="7e4b9-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="7e4b9-106">Tento kurz ukazuje postup vytvoření webové aplikace ASP.NET Core s uživatelskými daty chráněn autorizace.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-106">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="7e4b9-107">Zobrazí seznam kontakty, které ověřené uživatele (registrovaný) jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-107">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="7e4b9-108">Existují tři skupiny zabezpečení:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-108">There are three security groups:</span></span>

* <span data-ttu-id="7e4b9-109">**Registrované uživatele** můžete zobrazit všechny schválené data a můžete upravit nebo odstranit svá vlastní data.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-109">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="7e4b9-110">**Správci** můžete schválit nebo odmítnout kontaktní údaje.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-110">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="7e4b9-111">Pouze schválené kontakty jsou viditelné pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-111">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="7e4b9-112">**Správci** můžete schválit nebo odmítnout a upravit nebo odstranit všechna data.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-112">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="7e4b9-113">Na následujícím obrázku, uživatel Rick (`rick@example.com`) je přihlášený.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-113">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="7e4b9-114">Rick lze zobrazit pouze schválené kontakty a **upravit**/**odstranit**/**vytvořit nový** odkazy pro jeho kontakty.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-114">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="7e4b9-115">Pouze poslední záznam vytvořené Rick, zobrazí **upravit** a **odstranit** odkazy.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-115">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="7e4b9-116">Ostatní uživatelé neuvidí poslední záznam tak, aby manažer nebo správce se změnil stav do "Schváleno".</span><span class="sxs-lookup"><span data-stu-id="7e4b9-116">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Obrázek popsané předcházející](secure-data/_static/rick.png)

<span data-ttu-id="7e4b9-118">Na následujícím obrázku `manager@contoso.com` je přihlášen a v roli správce:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-118">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![Obrázek popsané předcházející](secure-data/_static/manager1.png)

<span data-ttu-id="7e4b9-120">Následující obrázek znázorňuje vybraných manažerů zobrazení podrobností kontaktu:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-120">The following image shows the managers details view of a contact:</span></span>

![Obrázek popsané předcházející](secure-data/_static/manager.png)

<span data-ttu-id="7e4b9-122">**Schválit** a **odmítnout** tlačítka se zobrazí pouze správci a správci.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-122">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="7e4b9-123">Na následujícím obrázku `admin@contoso.com` je přihlášen a v roli správce:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-123">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![Obrázek popsané předcházející](secure-data/_static/admin.png)

<span data-ttu-id="7e4b9-125">Správce má všechna oprávnění.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-125">The administrator has all privileges.</span></span> <span data-ttu-id="7e4b9-126">Jana můžete pro čtení, úpravy nebo odstranění všechny kontakty a změnit stav kontaktů.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-126">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="7e4b9-127">Aplikace byla vytvořená [generování uživatelského rozhraní](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) následující `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-127">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="7e4b9-128">Ukázka obsahuje následující rutiny autorizace:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-128">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="7e4b9-129">`ContactIsOwnerAuthorizationHandler`: Zajistí uživatele můžete upravit pouze svá data.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-129">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="7e4b9-130">`ContactManagerAuthorizationHandler`: Umožňuje správcům schválit nebo odmítnout kontakty.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-130">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="7e4b9-131">`ContactAdministratorsAuthorizationHandler`: Umožňuje správcům schválit nebo odmítnout kontakty a upravit nebo odstranit kontakty.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-131">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7e4b9-132">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7e4b9-132">Prerequisites</span></span>

<span data-ttu-id="7e4b9-133">V tomto kurzu je rozšířený.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-133">This tutorial is advanced.</span></span> <span data-ttu-id="7e4b9-134">Měli byste se seznámit s:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-134">You should be familiar with:</span></span>

* [<span data-ttu-id="7e4b9-135">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7e4b9-135">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="7e4b9-136">Ověřování</span><span class="sxs-lookup"><span data-stu-id="7e4b9-136">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="7e4b9-137">Potvrzení účtu a obnovení hesla</span><span class="sxs-lookup"><span data-stu-id="7e4b9-137">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="7e4b9-138">Autorizace</span><span class="sxs-lookup"><span data-stu-id="7e4b9-138">Authorization</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="7e4b9-139">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="7e4b9-139">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="7e4b9-140">V tématu [tento PDF soubor](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) pro ASP.NET MVC základní verzi.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-140">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="7e4b9-141">Verze 1.1 jádro ASP.NET v tomto kurzu je v [to](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) složky.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-141">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="7e4b9-142">1.1 ASP.NET Core ukázka je v [ukázky](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="7e4b9-142">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="7e4b9-143">Spuštění a dokončené aplikace</span><span class="sxs-lookup"><span data-stu-id="7e4b9-143">The starter and completed app</span></span>

<span data-ttu-id="7e4b9-144">[Stáhněte si](xref:tutorials/index#how-to-download-a-sample) [Dokončit](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) aplikace.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-144">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="7e4b9-145">[Test](#test-the-completed-app) dokončené aplikace, takže seznámit se s jeho funkce zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-145">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="7e4b9-146">Úvodní aplikaci</span><span class="sxs-lookup"><span data-stu-id="7e4b9-146">The starter app</span></span>

<span data-ttu-id="7e4b9-147">[Stáhněte si](xref:tutorials/index#how-to-download-a-sample) [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) aplikace.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-147">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="7e4b9-148">Spusťte aplikaci, klepněte **ContactManager** propojit a ověřte, můžete vytvořit, upravit a odstranit a obraťte se na.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-148">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="7e4b9-149">Zabezpečení dat uživatele</span><span class="sxs-lookup"><span data-stu-id="7e4b9-149">Secure user data</span></span>

<span data-ttu-id="7e4b9-150">V následujících částech mít všechny hlavní kroky k vytvoření aplikace dat zabezpečení uživatele.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-150">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="7e4b9-151">Vám může být užitečné k odkazování na dokončený projekt.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-151">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="7e4b9-152">Tie – kontaktních údajů pro uživatele</span><span class="sxs-lookup"><span data-stu-id="7e4b9-152">Tie the contact data to the user</span></span>

<span data-ttu-id="7e4b9-153">Pomocí technologie ASP.NET [Identity](xref:security/authentication/identity) ID uživatele, aby uživatelé můžete upravit svá data, ale ne další data uživatele.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-153">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="7e4b9-154">Přidat `OwnerID` a `ContactStatus` k `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-154">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="7e4b9-155">`OwnerID`ID uživatele z `AspNetUser` tabulky v [Identity](xref:security/authentication/identity) databáze.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-155">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="7e4b9-156">`Status` Pole určuje, zda kontakt zobrazit obecné uživatele.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-156">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="7e4b9-157">Vytvořte nové migrace a aktualizaci databáze:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-157">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-ssl-and-authenticated-users"></a><span data-ttu-id="7e4b9-158">Vyžadovat protokol SSL a ověření uživatelé</span><span class="sxs-lookup"><span data-stu-id="7e4b9-158">Require SSL and authenticated users</span></span>

<span data-ttu-id="7e4b9-159">Přidat [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) k `Startup`:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-159">Add [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) to `Startup`:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_env)]

<span data-ttu-id="7e4b9-160">V `ConfigureServices` metodu *Startup.cs* soubor, přidejte [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) filtr autorizace:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-160">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) authorization filter:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=19-999)]

<span data-ttu-id="7e4b9-161">Pokud používáte Visual Studio, povolte protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-161">If you're using Visual Studio, enable SSL.</span></span>

<span data-ttu-id="7e4b9-162">Přesměrování požadavků HTTP do HTTPS, najdete v části [URL přepisování Middleware](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="7e4b9-162">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="7e4b9-163">Pokud jste pomocí Visual Studio Code nebo testování na místní platformu, která neobsahuje testovací certifikát pro protokol SSL:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-163">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for SSL:</span></span>

  <span data-ttu-id="7e4b9-164">Nastavit `"LocalTest:skipSSL": true` v *appsettings. Developement.JSON* souboru.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-164">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="7e4b9-165">Vyžadovat ověření uživatelé</span><span class="sxs-lookup"><span data-stu-id="7e4b9-165">Require authenticated users</span></span>

<span data-ttu-id="7e4b9-166">Nastavte výchozí zásady ověřování tak, aby vyžadovala uživatele k ověření.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-166">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="7e4b9-167">Můžete vyjádření výslovného nesouhlasu ověřování na úrovni stránky Razor, kontroler nebo akce metoda s `[AllowAnonymous]` atribut.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-167">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="7e4b9-168">Nastavení výchozích zásad ověřování budou muset uživatelé ověřit chrání nově přidané stránky Razor a řadiče.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-168">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="7e4b9-169">Má ve výchozím nastavení je vyžadováno ověření je bezpečnější než spoléhat na nových řadičů a stránky Razor zahrnout `[Authorize]` atribut.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-169">Having authentication required by default is safer than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span> <span data-ttu-id="7e4b9-170">Přidejte následující `ConfigureServices` metodu *Startup.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-170">Add the following to the `ConfigureServices` method of the *Startup.cs* file:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=31-999)]

<span data-ttu-id="7e4b9-171">Přidat [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) do indexu, o a kontaktní stránky, mohou anonymní uživatelé získat informace o lokalitě, před jejich registraci.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-171">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span> 

[!code-csharp[Main](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

<span data-ttu-id="7e4b9-172">Přidat `[AllowAnonymous]` k [LoginModel a RegisterModel](https://github.com/aspnet/templating/issues/238).</span><span class="sxs-lookup"><span data-stu-id="7e4b9-172">Add `[AllowAnonymous]` to the [LoginModel and RegisterModel](https://github.com/aspnet/templating/issues/238).</span></span>

### <a name="configure-the-test-account"></a><span data-ttu-id="7e4b9-173">Nakonfigurujte účet pro test</span><span class="sxs-lookup"><span data-stu-id="7e4b9-173">Configure the test account</span></span>

<span data-ttu-id="7e4b9-174">`SeedData` Třída vytvoří dva účty: správce a správce.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-174">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="7e4b9-175">Použití [nástroj tajný klíč správce](xref:security/app-secrets) nastavení hesla pro tyto účty.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-175">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="7e4b9-176">Nastavení hesla z adresáře projektu (adresář obsahující *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="7e4b9-176">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="7e4b9-177">Aktualizace `Main` používat heslo testu:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-177">Update `Main` to use the test password:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="7e4b9-178">Vytvoření testovacích účtů a aktualizovat kontaktů</span><span class="sxs-lookup"><span data-stu-id="7e4b9-178">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="7e4b9-179">Aktualizace `Initialize` metoda v `SeedData` třídy za účelem vytvoření testovacích účtů:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-179">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="7e4b9-180">Přidat ID uživatele správce a `ContactStatus` kontaktům.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-180">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="7e4b9-181">Si ho kontaktů "Odesláno" a jedné "Zamítnutá".</span><span class="sxs-lookup"><span data-stu-id="7e4b9-181">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="7e4b9-182">Přidáte ID uživatele a stav pro všechny kontakty.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-182">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="7e4b9-183">Je zobrazena pouze jeden kontakt:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-183">Only one contact is shown:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="7e4b9-184">Vytvoření vlastníka, správce a Správce autorizací obslužné rutiny</span><span class="sxs-lookup"><span data-stu-id="7e4b9-184">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="7e4b9-185">Vytvoření `ContactIsOwnerAuthorizationHandler` třídy v *autorizace* složky.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-185">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="7e4b9-186">`ContactIsOwnerAuthorizationHandler` Ověřuje, že uživatel, který funguje na prostředku vlastníkem prostředku.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-186">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="7e4b9-187">`ContactIsOwnerAuthorizationHandler` Volání [kontextu. Úspěšné](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) Pokud ověřený aktuální uživatel je jeho vlastníkem.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-187">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="7e4b9-188">Obslužné rutiny autorizace obecně:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-188">Authorization handlers generally:</span></span>

* <span data-ttu-id="7e4b9-189">Vrátí `context.Succeed` Pokud jsou splněny požadavky na.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-189">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="7e4b9-190">Vrátí `Task.CompletedTask` Pokud nejsou splněné požadavky.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-190">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="7e4b9-191">`Task.CompletedTask`je ani úspěch nebo neúspěch&mdash;umožňuje ostatních obslužných rutin autorizaci ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-191">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="7e4b9-192">Pokud potřebujete explicitně nezdaří, vrátí [kontextu. Selhání](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="7e4b9-192">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="7e4b9-193">Aplikace umožňuje kontaktní vlastníky úpravy, odstranění nebo vytvořit svá vlastní data.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-193">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="7e4b9-194">`ContactIsOwnerAuthorizationHandler`není potřeba zkontrolovat operaci předán v parametru požadavek.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-194">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="7e4b9-195">Vytvořte obslužnou rutinu Správce autorizací</span><span class="sxs-lookup"><span data-stu-id="7e4b9-195">Create a manager authorization handler</span></span>

<span data-ttu-id="7e4b9-196">Vytvoření `ContactManagerAuthorizationHandler` třídy v *autorizace* složky.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-196">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="7e4b9-197">`ContactManagerAuthorizationHandler` Ověřuje uživatele, který funguje na prostředku je správce.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-197">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="7e4b9-198">Pouze správci můžete schválit nebo odmítnout změny obsahu (nové nebo změněné).</span><span class="sxs-lookup"><span data-stu-id="7e4b9-198">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="7e4b9-199">Vytvoření obslužné rutiny Správce autorizací</span><span class="sxs-lookup"><span data-stu-id="7e4b9-199">Create an administrator authorization handler</span></span>

<span data-ttu-id="7e4b9-200">Vytvoření `ContactAdministratorsAuthorizationHandler` třídy v *autorizace* složky.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-200">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="7e4b9-201">`ContactAdministratorsAuthorizationHandler` Ověřuje uživatele, který funguje na prostředku je správce.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-201">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="7e4b9-202">Správce může provádět všechny operace.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-202">Administrator can do all operations.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="7e4b9-203">Registraci obslužných rutin autorizace</span><span class="sxs-lookup"><span data-stu-id="7e4b9-203">Register the authorization handlers</span></span>

<span data-ttu-id="7e4b9-204">Musí být pro registrované služby pomocí Entity Framework Core [vkládání závislostí](xref:fundamentals/dependency-injection) pomocí [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="7e4b9-204">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="7e4b9-205">`ContactIsOwnerAuthorizationHandler` Používá ASP.NET Core [Identity](xref:security/authentication/identity), který je založený na Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-205">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="7e4b9-206">Registraci obslužných rutin s kolekcí služby, takže jsou k dispozici na `ContactsController` prostřednictvím [vkládání závislostí](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7e4b9-206">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="7e4b9-207">Přidejte následující kód do konce `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-207">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-999)]

<span data-ttu-id="7e4b9-208">`ContactAdministratorsAuthorizationHandler`a `ContactManagerAuthorizationHandler` jsou přidány jako jednotlivé prvky.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-208">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="7e4b9-209">Jsou jednotlivé prvky, protože se nepoužívají EF a veškeré informace potřebné v `Context` parametr `HandleRequirementAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-209">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="7e4b9-210">Povolení podpory</span><span class="sxs-lookup"><span data-stu-id="7e4b9-210">Support authorization</span></span>

<span data-ttu-id="7e4b9-211">V této části se aktualizace stránky Razor a přidejte třídu operations požadavky.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-211">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="7e4b9-212">Zkontrolujte požadavky na třídy kontaktní operace</span><span class="sxs-lookup"><span data-stu-id="7e4b9-212">Review the contact operations requirements class</span></span>

<span data-ttu-id="7e4b9-213">Zkontrolujte `ContactOperations` třídy.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-213">Review the `ContactOperations` class.</span></span> <span data-ttu-id="7e4b9-214">Tato třída obsahuje požadavky na aplikace podporuje:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-214">This class contains the requirements the app supports:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a><span data-ttu-id="7e4b9-215">Vytvořte základní třídu pro stránky Razor</span><span class="sxs-lookup"><span data-stu-id="7e4b9-215">Create a base class for the Razor Pages</span></span>

<span data-ttu-id="7e4b9-216">Vytvořte základní třídu, která obsahuje služeb v rámci kontakty stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-216">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="7e4b9-217">Základní třída vloží tento kód inicializace na jednom místě:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-217">The base class puts that initialization code in one location:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="7e4b9-218">Předchozí kód:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-218">The preceding code:</span></span>

* <span data-ttu-id="7e4b9-219">Přidá `IAuthorizationService` služby s přístupem k autorizaci obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-219">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="7e4b9-220">Přidá identitu `UserManager` služby.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-220">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="7e4b9-221">Přidat `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-221">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="7e4b9-222">Aktualizace CreateModel</span><span class="sxs-lookup"><span data-stu-id="7e4b9-222">Update the CreateModel</span></span>

<span data-ttu-id="7e4b9-223">Aktualizovat konstruktor vytvořit stránku modelu používat `DI_BasePageModel` základní třídy:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-223">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="7e4b9-224">Aktualizace `CreateModel.OnPostAsync` metodu:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-224">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="7e4b9-225">Přidat ID uživatele `Contact` modelu.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-225">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="7e4b9-226">Ověřte, zda že má uživatel oprávnění k vytvoření kontakty autorizace obslužná rutina volejte.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-226">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="7e4b9-227">Aktualizace IndexModel</span><span class="sxs-lookup"><span data-stu-id="7e4b9-227">Update the IndexModel</span></span>

<span data-ttu-id="7e4b9-228">Aktualizace `OnGetAsync` metoda tak uživatelům obecné jsou zobrazeny pouze schválené kontaktů:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-228">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="7e4b9-229">Aktualizace EditModel</span><span class="sxs-lookup"><span data-stu-id="7e4b9-229">Update the EditModel</span></span>

<span data-ttu-id="7e4b9-230">Přidejte obslužnou rutinu ověřování k ověření, že uživatel kontakt vlastní.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-230">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="7e4b9-231">Protože je ověřován autorizace prostředků, `[Authorize]` atribut není dost.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-231">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="7e4b9-232">Aplikace nemá přístup k prostředku, při hodnocení atributy.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-232">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="7e4b9-233">Ověření na základě prostředků musí být nutné.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-233">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="7e4b9-234">Jakmile načítání v modelu stránky nebo načítání v rámci obslužná rutina samotné má aplikace přístup k prostředku, musí provést kontroly.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-234">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="7e4b9-235">Často přístup k prostředku předáním v klíči prostředků.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-235">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="7e4b9-236">Aktualizace DeleteModel</span><span class="sxs-lookup"><span data-stu-id="7e4b9-236">Update the DeleteModel</span></span>

<span data-ttu-id="7e4b9-237">Aktualizace modelu stránky odstranit pomocí obslužná rutina ověřování ověřte, zda že má uživatel oprávnění odstranit na kontakt.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-237">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="7e4b9-238">Vložit služby autorizace do zobrazení</span><span class="sxs-lookup"><span data-stu-id="7e4b9-238">Inject the authorization service into the views</span></span>

<span data-ttu-id="7e4b9-239">Zobrazuje uživatelské rozhraní v současné době upravovat a odstraňovat odkazy na data, která uživatele nelze upravit.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-239">Currently, the UI shows edit and delete links for data the user can't modify.</span></span> <span data-ttu-id="7e4b9-240">Uživatelské rozhraní je opraven použití obslužná rutina ověřování pro zobrazení.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-240">The UI is fixed by applying the authorization handler to the views.</span></span>

<span data-ttu-id="7e4b9-241">Vložit služby ověřování *Views/_ViewImports.cshtml* souboru tak, aby byl k dispozici pro všechna zobrazení:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-241">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

<span data-ttu-id="7e4b9-242">Předchozí kód přidá několik `using` příkazy.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-242">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="7e4b9-243">Aktualizace **upravit** a **odstranit** odkazů v *Pages/Contacts/Index.cshtml* , pouze se vykresluje pro uživatele s příslušnými oprávněními:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-243">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-999)]

> [!WARNING]
> <span data-ttu-id="7e4b9-244">Skrytí odkazy z uživatelů, kteří nemají oprávnění ke změně dat nepodporuje zabezpečit aplikace.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-244">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="7e4b9-245">Skrytí odkazy díky aplikaci přívětivější zobrazením pouze platné odkazy.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-245">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="7e4b9-246">Uživatelé mohou zabezpečení generované adresy URL pro vyvolání upravit a odstranit operací na data, která budou nevlastníte.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-246">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="7e4b9-247">Kontroly přístupu k zabezpečení dat musí vynucovat stránky Razor nebo kontroleru.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-247">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="7e4b9-248">Podrobné informace o aktualizaci</span><span class="sxs-lookup"><span data-stu-id="7e4b9-248">Update Details</span></span>

<span data-ttu-id="7e4b9-249">Zobrazení podrobností aktualizujte, aby správci můžete schválit nebo odmítnout kontaktů:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-249">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-999)]

<span data-ttu-id="7e4b9-250">Aktualizace modelu stránky podrobnosti:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-250">Update the details page model:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a><span data-ttu-id="7e4b9-251">Testování dokončená aplikace</span><span class="sxs-lookup"><span data-stu-id="7e4b9-251">Test the completed app</span></span>

<span data-ttu-id="7e4b9-252">Pokud jste pomocí Visual Studio Code nebo testování na místní platformu, která neobsahuje testovací certifikát pro protokol SSL:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-252">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for SSL:</span></span>

* <span data-ttu-id="7e4b9-253">Nastavit `"LocalTest:skipSSL": true` v *appsettings. Developement.JSON* souboru tak, aby přeskočil požadavek na protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-253">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file to skip the SSL requirement.</span></span> <span data-ttu-id="7e4b9-254">Přeskočit SSL pouze na vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-254">Skip SSL only on a development machine.</span></span>

<span data-ttu-id="7e4b9-255">Pokud má kontaktů:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-255">If the app has contacts:</span></span>

* <span data-ttu-id="7e4b9-256">Odstranit všechny záznamy v `Contact` tabulky.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-256">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="7e4b9-257">Restartujte aplikaci počáteční hodnoty databáze.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-257">Restart the app to seed the database.</span></span>

<span data-ttu-id="7e4b9-258">Registrace uživatele pro procházení kontaktů.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-258">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="7e4b9-259">Snadný způsob, jak testování dokončená aplikace je spustíte tři různé prohlížeče (nebo incognito/InPrivate verze).</span><span class="sxs-lookup"><span data-stu-id="7e4b9-259">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="7e4b9-260">V jedné prohlížeče registraci nového uživatele (například `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="7e4b9-260">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="7e4b9-261">Přihlaste se do každé prohlížeče s jiným uživatelem.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-261">Sign in to each browser with a different user.</span></span> <span data-ttu-id="7e4b9-262">Ověřte následující operace:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-262">Verify the following operations:</span></span>

* <span data-ttu-id="7e4b9-263">Registrovaní uživatelé můžete zobrazit všechny schválené kontaktní data.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-263">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="7e4b9-264">Registrovaní uživatelé můžete upravit nebo odstranit svá vlastní data.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-264">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="7e4b9-265">Správce můžete schválit nebo odmítnout kontaktní údaje.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-265">Managers can approve or reject contact data.</span></span> <span data-ttu-id="7e4b9-266">`Details` Zobrazení ukazuje **schválit** a **odmítnout** tlačítka.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-266">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="7e4b9-267">Správci mohou schválit či odmítnout a upravit nebo odstranit všechna data.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-267">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="7e4b9-268">Uživatel</span><span class="sxs-lookup"><span data-stu-id="7e4b9-268">User</span></span>| <span data-ttu-id="7e4b9-269">Možnosti</span><span class="sxs-lookup"><span data-stu-id="7e4b9-269">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="7e4b9-270">Můžete upravit nebo odstranit vlastní data</span><span class="sxs-lookup"><span data-stu-id="7e4b9-270">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="7e4b9-271">Můžete schválit nebo odmítnout a upravit nebo odstranit vlastní data</span><span class="sxs-lookup"><span data-stu-id="7e4b9-271">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="7e4b9-272">Můžete upravit nebo odstranit a schválit či odmítnout všechna data</span><span class="sxs-lookup"><span data-stu-id="7e4b9-272">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="7e4b9-273">Vytvoření kontaktu v prohlížeči na správce.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-273">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="7e4b9-274">Zkopírujte adresu URL pro odstranění a upravit z kontaktujte správce.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-274">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="7e4b9-275">Tyto odkazy vložte do prohlížeče se zobrazil uživatel k ověření, že se zobrazil uživatel nemůže provést tyto operace.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-275">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="7e4b9-276">Vytvořit úvodní aplikaci</span><span class="sxs-lookup"><span data-stu-id="7e4b9-276">Create the starter app</span></span>

* <span data-ttu-id="7e4b9-277">Vytvoření stránky Razor aplikace s názvem "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="7e4b9-277">Create a Razor Pages app named "ContactManager"</span></span>

  * <span data-ttu-id="7e4b9-278">Vytvoření aplikace s **jednotlivých uživatelských účtů**.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-278">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="7e4b9-279">Pojmenujte ji "ContactManager", obor názvů odpovídá oboru názvů používaný v ukázce.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-279">Name it "ContactManager" so your namespace matches the namespace used in the sample.</span></span>

  ```console
  dotnet new razor -o ContactManager -au Individual -uld
  ```

  * <span data-ttu-id="7e4b9-280">`-uld`Určuje LocalDB místo SQLite</span><span class="sxs-lookup"><span data-stu-id="7e4b9-280">`-uld` specifies LocalDB instead of SQLite</span></span>

* <span data-ttu-id="7e4b9-281">Přidejte následující `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-281">Add the following `Contact` model:</span></span>

  [!code-csharp[Main](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="7e4b9-282">Vygenerované uživatelské rozhraní `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-282">Scaffold the `Contact` model:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* <span data-ttu-id="7e4b9-283">Aktualizace **ContactManager** ukotvení v *Pages/_Layout.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-283">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="7e4b9-284">Vygenerovat počáteční migraci a aktualizaci databáze:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-284">Scaffold the initial migration and update the database:</span></span>

```console
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="7e4b9-285">Aplikaci otestovat a vytváření, úpravy a odstranění kontaktu</span><span class="sxs-lookup"><span data-stu-id="7e4b9-285">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="7e4b9-286">Počáteční hodnoty databáze</span><span class="sxs-lookup"><span data-stu-id="7e4b9-286">Seed the database</span></span>

<span data-ttu-id="7e4b9-287">Přidat `SeedData` třídy k *Data* složky.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-287">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="7e4b9-288">Pokud jste stáhli ukázku, můžete zkopírovat *SeedData.cs* do souboru *Data* složky starter projektu.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-288">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

<span data-ttu-id="7e4b9-289">Volání `SeedData.Initialize` z `Main`:</span><span class="sxs-lookup"><span data-stu-id="7e4b9-289">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[Main](secure-data/samples/starter2/Program.cs?name=snippet)]

<span data-ttu-id="7e4b9-290">Otestujte, že aplikace nasadí databázi.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-290">Test that the app seeded the database.</span></span> <span data-ttu-id="7e4b9-291">Pokud jsou všechny řádky v kontakt DB, metodu počáteční hodnoty nefunguje.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-291">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="7e4b9-292">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="7e4b9-292">Additional resources</span></span>

* <span data-ttu-id="7e4b9-293">[Prostředí ASP.NET Core autorizace](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="7e4b9-293">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="7e4b9-294">Toto testovací prostředí obsahuje více podrobností o funkcích zabezpečení byla zavedená v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="7e4b9-294">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="7e4b9-295">Autorizace v ASP.NET Core: jednoduchý, na základě deklarace a vlastní role</span><span class="sxs-lookup"><span data-stu-id="7e4b9-295">Authorization in ASP.NET Core: Simple, role, claims-based, and custom</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="7e4b9-296">Autorizace uživatele na základě zásad</span><span class="sxs-lookup"><span data-stu-id="7e4b9-296">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
