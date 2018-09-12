---
title: Vytvoření aplikace ASP.NET Core s uživatelskými daty chráněnými autorizací
author: rick-anderson
description: Zjistěte, jak vytvořit aplikace Razor Pages s uživatelskými daty chráněnými autorizací. Zahrnuje HTTPS, ověřování, zabezpečení ASP.NET Core Identity.
ms.author: riande
ms.date: 7/24/2018
uid: security/authorization/secure-data
ms.openlocfilehash: a263b092194763ae4ff3360fc0d76e8ee494b5a6
ms.sourcegitcommit: e7e1e531b80b3f4117ff119caadbebf4dcf5dcb7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/12/2018
ms.locfileid: "44510360"
---
::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="1db61-104">Zobrazit [tento PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) pro verzi technologie ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="1db61-104">See [this PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="1db61-105">ASP.NET Core 1.1 verzi tohoto kurzu je v [to](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) složky.</span><span class="sxs-lookup"><span data-stu-id="1db61-105">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="1db61-106">1.1 ukázka ASP.NET Core je v [ukázky](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="1db61-106">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="1db61-107">Zobrazit [tento pdf](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="1db61-107">See [this pdf](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="1db61-108">Vytvoření aplikace ASP.NET Core s uživatelskými daty chráněnými autorizací</span><span class="sxs-lookup"><span data-stu-id="1db61-108">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="1db61-109">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="1db61-109">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="1db61-110">Tento kurz ukazuje, jak vytvořit webovou aplikaci ASP.NET Core s uživatelskými daty chráněnými autorizací.</span><span class="sxs-lookup"><span data-stu-id="1db61-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="1db61-111">Zobrazí seznam kontakty, které ověřeným uživatelům (registrovaných) jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="1db61-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="1db61-112">Existují tři skupiny zabezpečení:</span><span class="sxs-lookup"><span data-stu-id="1db61-112">There are three security groups:</span></span>

* <span data-ttu-id="1db61-113">**Zaregistrované uživatele** můžete zobrazit všechny schválené data a můžete upravit nebo odstranit svá vlastní data.</span><span class="sxs-lookup"><span data-stu-id="1db61-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="1db61-114">**Správci** můžete schválit nebo odmítnout kontaktní údaje.</span><span class="sxs-lookup"><span data-stu-id="1db61-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="1db61-115">Uživatelé vidí pouze schválené kontakty.</span><span class="sxs-lookup"><span data-stu-id="1db61-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="1db61-116">**Správci** můžete schvalovat a odmítat a upravit nebo odstranit všechna data.</span><span class="sxs-lookup"><span data-stu-id="1db61-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="1db61-117">Na následujícím obrázku, uživatel Rick (`rick@example.com`) je přihlášený.</span><span class="sxs-lookup"><span data-stu-id="1db61-117">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="1db61-118">Rick může zobrazit jenom schválené kontakty a **upravit**/**odstranit**/**vytvořit nový** odkazy pro jeho kontakty.</span><span class="sxs-lookup"><span data-stu-id="1db61-118">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="1db61-119">Pouze poslední záznam vytvořil Rick, zobrazí **upravit** a **odstranit** odkazy.</span><span class="sxs-lookup"><span data-stu-id="1db61-119">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="1db61-120">Ostatní uživatelé neuvidí poslední záznam, dokud správce nebo správce změní stav na "Schváleno".</span><span class="sxs-lookup"><span data-stu-id="1db61-120">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![obrázek popisuje předchozí](secure-data/_static/rick.png)

<span data-ttu-id="1db61-122">Na následujícím obrázku `manager@contoso.com` je podepsán v a v roli správce:</span><span class="sxs-lookup"><span data-stu-id="1db61-122">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![obrázek popisuje předchozí](secure-data/_static/manager1.png)

<span data-ttu-id="1db61-124">Následující obrázek ukazuje vedoucí zobrazení podrobností o kontaktu:</span><span class="sxs-lookup"><span data-stu-id="1db61-124">The following image shows the managers details view of a contact:</span></span>

![obrázek popisuje předchozí](secure-data/_static/manager.png)

<span data-ttu-id="1db61-126">**Schválit** a **odmítnout** tlačítek se zobrazí pouze správci a správci.</span><span class="sxs-lookup"><span data-stu-id="1db61-126">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="1db61-127">Na následujícím obrázku `admin@contoso.com` je podepsán v a v roli správce:</span><span class="sxs-lookup"><span data-stu-id="1db61-127">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![obrázek popisuje předchozí](secure-data/_static/admin.png)

<span data-ttu-id="1db61-129">Správce má všechna oprávnění.</span><span class="sxs-lookup"><span data-stu-id="1db61-129">The administrator has all privileges.</span></span> <span data-ttu-id="1db61-130">Může číst/upravovat/odstraňovat všechny kontakty a změnit stav kontakty.</span><span class="sxs-lookup"><span data-stu-id="1db61-130">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="1db61-131">Aplikace byla vytvořena pomocí [generování uživatelského rozhraní](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) následující `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="1db61-131">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet)]

<span data-ttu-id="1db61-132">Ukázka obsahuje následující rutiny autorizace:</span><span class="sxs-lookup"><span data-stu-id="1db61-132">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="1db61-133">`ContactIsOwnerAuthorizationHandler`: Zajistí, že uživatel může upravovat jenom svá data.</span><span class="sxs-lookup"><span data-stu-id="1db61-133">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="1db61-134">`ContactManagerAuthorizationHandler`: Umožňuje správcům schválit nebo odmítnout kontakty.</span><span class="sxs-lookup"><span data-stu-id="1db61-134">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="1db61-135">`ContactAdministratorsAuthorizationHandler`: Umožňuje správcům schválit nebo odmítnout kontakty a úpravy nebo odstranění kontaktů.</span><span class="sxs-lookup"><span data-stu-id="1db61-135">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1db61-136">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1db61-136">Prerequisites</span></span>

<span data-ttu-id="1db61-137">V tomto kurzu je advanced.</span><span class="sxs-lookup"><span data-stu-id="1db61-137">This tutorial is advanced.</span></span> <span data-ttu-id="1db61-138">Měli byste se seznámit s:</span><span class="sxs-lookup"><span data-stu-id="1db61-138">You should be familiar with:</span></span>

* [<span data-ttu-id="1db61-139">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1db61-139">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="1db61-140">Ověřování</span><span class="sxs-lookup"><span data-stu-id="1db61-140">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="1db61-141">Potvrzení účtu a obnovení hesla</span><span class="sxs-lookup"><span data-stu-id="1db61-141">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="1db61-142">Autorizace</span><span class="sxs-lookup"><span data-stu-id="1db61-142">Authorization</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="1db61-143">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="1db61-143">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

::: moniker-end
::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="1db61-144">V ASP.NET Core 2.1 `User.IsInRole` selže při použití `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="1db61-144">In ASP.NET Core 2.1, `User.IsInRole` fails when using `AddDefaultIdentity`.</span></span> <span data-ttu-id="1db61-145">Tento kurz používá `AddDefaultIdentity` a vyžaduje tudíž ASP.NET Core 2.2 ve verzi preview 1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="1db61-145">This tutorial uses `AddDefaultIdentity` and therefore requires ASP.NET Core 2.2 preview 1 or later.</span></span> <span data-ttu-id="1db61-146">Zobrazit [tento problém Githubu](https://github.com/aspnet/Identity/issues/1813#issuecomment-394543909) pro vyřešit.</span><span class="sxs-lookup"><span data-stu-id="1db61-146">See [this GitHub issue](https://github.com/aspnet/Identity/issues/1813#issuecomment-394543909) for a work-around.</span></span>

::: moniker-end
::: moniker range=">= aspnetcore-2.1"

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="1db61-147">Starter a dokončené aplikace</span><span class="sxs-lookup"><span data-stu-id="1db61-147">The starter and completed app</span></span>

<span data-ttu-id="1db61-148">[Stáhněte si](xref:tutorials/index#how-to-download-a-sample) [Dokončit](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) aplikace.</span><span class="sxs-lookup"><span data-stu-id="1db61-148">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="1db61-149">[Test](#test-the-completed-app) dokončené aplikace tak, že jste se seznámili s jeho funkcí zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="1db61-149">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="1db61-150">Úvodní aplikaci</span><span class="sxs-lookup"><span data-stu-id="1db61-150">The starter app</span></span>

<span data-ttu-id="1db61-151">[Stáhněte si](xref:tutorials/index#how-to-download-a-sample) [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) aplikace.</span><span class="sxs-lookup"><span data-stu-id="1db61-151">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="1db61-152">Spusťte aplikaci, klepněte **ContactManager** propojit a ověření můžete vytvářet, upravovat a odstraňovat kontakt.</span><span class="sxs-lookup"><span data-stu-id="1db61-152">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="1db61-153">Zabezpečení dat uživatele</span><span class="sxs-lookup"><span data-stu-id="1db61-153">Secure user data</span></span>

<span data-ttu-id="1db61-154">Následující oddíly mají všechny hlavní kroky k vytvoření aplikace pro data zabezpečení uživatele.</span><span class="sxs-lookup"><span data-stu-id="1db61-154">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="1db61-155">Vám může být užitečné k odkazování na dokončení projektu.</span><span class="sxs-lookup"><span data-stu-id="1db61-155">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="1db61-156">Tie kontaktní údaje pro uživatele</span><span class="sxs-lookup"><span data-stu-id="1db61-156">Tie the contact data to the user</span></span>

<span data-ttu-id="1db61-157">Použití technologie ASP.NET [Identity](xref:security/authentication/identity) ID uživatele, aby tak uživatelé můžete upravit jejich data, ale ne data jiných uživatelů.</span><span class="sxs-lookup"><span data-stu-id="1db61-157">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="1db61-158">Přidat `OwnerID` a `ContactStatus` k `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="1db61-158">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="1db61-159">`OwnerID` je ID uživatele z `AspNetUser` v tabulku [Identity](xref:security/authentication/identity) databáze.</span><span class="sxs-lookup"><span data-stu-id="1db61-159">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="1db61-160">`Status` Pole určuje, zda je kontakt zobrazitelné obecný uživatel.</span><span class="sxs-lookup"><span data-stu-id="1db61-160">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="1db61-161">Vytvořte novou migraci a aktualizaci databáze:</span><span class="sxs-lookup"><span data-stu-id="1db61-161">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="1db61-162">Přidání služby Role na identitu</span><span class="sxs-lookup"><span data-stu-id="1db61-162">Add Role services to Identity</span></span>

<span data-ttu-id="1db61-163">Připojit [kliknutím na Přidat role](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) přidat služby rolí:</span><span class="sxs-lookup"><span data-stu-id="1db61-163">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="1db61-164">Vyžadovat ověření uživatelé</span><span class="sxs-lookup"><span data-stu-id="1db61-164">Require authenticated users</span></span>

<span data-ttu-id="1db61-165">Nastavte výchozí zásady ověřování tak, aby vyžadovala ověření uživatelů:</span><span class="sxs-lookup"><span data-stu-id="1db61-165">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="1db61-166">Můžete zrušit ověřování na úrovni metody stránky Razor, kontroler nebo akce s `[AllowAnonymous]` atribut.</span><span class="sxs-lookup"><span data-stu-id="1db61-166">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="1db61-167">Nastavení výchozí zásady ověřování tak, aby vyžadovala ověření uživatelů chrání nově přidané Razor Pages a kontrolery.</span><span class="sxs-lookup"><span data-stu-id="1db61-167">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="1db61-168">Ve výchozím nastavení je vyžadováno ověření je bezpečnější než spoléhání se na nové řadiče a Razor Pages zahrnout s `[Authorize]` atribut.</span><span class="sxs-lookup"><span data-stu-id="1db61-168">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="1db61-169">Přidat [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) index stránky o a kontaktní anonymní uživatelé získali informace o webu předtím, než aby se zaregistrovali.</span><span class="sxs-lookup"><span data-stu-id="1db61-169">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="1db61-170">Konfigurace testovací účet</span><span class="sxs-lookup"><span data-stu-id="1db61-170">Configure the test account</span></span>

<span data-ttu-id="1db61-171">`SeedData` Třída vytvoří dva účty: správce a správce.</span><span class="sxs-lookup"><span data-stu-id="1db61-171">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="1db61-172">Použití [nástroj tajný klíč správce](xref:security/app-secrets) nastavení hesla pro tyto účty.</span><span class="sxs-lookup"><span data-stu-id="1db61-172">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="1db61-173">Nastavte heslo z adresáře projektu (adresáře, který obsahuje *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="1db61-173">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="1db61-174">Pokud není zadán silné heslo, je vyvolána výjimka, když `SeedData.Initialize` je volána.</span><span class="sxs-lookup"><span data-stu-id="1db61-174">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="1db61-175">Aktualizace `Main` test heslo budete používat:</span><span class="sxs-lookup"><span data-stu-id="1db61-175">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="1db61-176">Vytvoření testovacích účtů a aktualizovat kontakty</span><span class="sxs-lookup"><span data-stu-id="1db61-176">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="1db61-177">Aktualizace `Initialize` metodu `SeedData` třídy za účelem vytvoření testovacích účtů:</span><span class="sxs-lookup"><span data-stu-id="1db61-177">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="1db61-178">Přidat správce ID uživatele a `ContactStatus` kontaktům.</span><span class="sxs-lookup"><span data-stu-id="1db61-178">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="1db61-179">Proveďte jednu z kontakty "Submitted" a jedné "byl odmítnut".</span><span class="sxs-lookup"><span data-stu-id="1db61-179">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="1db61-180">Přidáte ID uživatele a stav pro všechny kontakty.</span><span class="sxs-lookup"><span data-stu-id="1db61-180">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="1db61-181">Je zobrazena pouze jeden kontakt:</span><span class="sxs-lookup"><span data-stu-id="1db61-181">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="1db61-182">Vytvořit vlastníka, správce a Správce autorizace obslužné rutiny</span><span class="sxs-lookup"><span data-stu-id="1db61-182">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="1db61-183">Vytvoření `ContactIsOwnerAuthorizationHandler` třídy v *autorizace* složky.</span><span class="sxs-lookup"><span data-stu-id="1db61-183">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="1db61-184">`ContactIsOwnerAuthorizationHandler` Ověřuje, že uživatel na prostředek vlastníkem prostředku.</span><span class="sxs-lookup"><span data-stu-id="1db61-184">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="1db61-185">`ContactIsOwnerAuthorizationHandler` Volání [kontextu. Úspěšné](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) aktuálně ověřeného uživatele při jeho vlastníkem.</span><span class="sxs-lookup"><span data-stu-id="1db61-185">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="1db61-186">Obslužné rutiny autorizace obecně:</span><span class="sxs-lookup"><span data-stu-id="1db61-186">Authorization handlers generally:</span></span>

* <span data-ttu-id="1db61-187">Vrátí `context.Succeed` Pokud jsou splněny.</span><span class="sxs-lookup"><span data-stu-id="1db61-187">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="1db61-188">Vrátí `Task.CompletedTask` Pokud požadavky nejsou splněny.</span><span class="sxs-lookup"><span data-stu-id="1db61-188">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="1db61-189">`Task.CompletedTask` je ani úspěch nebo neúspěch&mdash;umožňuje ostatních obslužných rutin autorizaci ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="1db61-189">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="1db61-190">Pokud je potřeba explicitně nezdaří, vrátí [kontextu. Selhání](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="1db61-190">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="1db61-191">Aplikaci umožňuje kontaktní vlastníkům upravit, odstranit a vytvořit svoje vlastní data.</span><span class="sxs-lookup"><span data-stu-id="1db61-191">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="1db61-192">`ContactIsOwnerAuthorizationHandler` není nutné ke kontrole operace předané v parametru požadavku.</span><span class="sxs-lookup"><span data-stu-id="1db61-192">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="1db61-193">Vytvořte obslužnou rutinu Správce autorizací</span><span class="sxs-lookup"><span data-stu-id="1db61-193">Create a manager authorization handler</span></span>

<span data-ttu-id="1db61-194">Vytvoření `ContactManagerAuthorizationHandler` třídy v *autorizace* složky.</span><span class="sxs-lookup"><span data-stu-id="1db61-194">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="1db61-195">`ContactManagerAuthorizationHandler` Ověřuje uživatele na prostředek je správce.</span><span class="sxs-lookup"><span data-stu-id="1db61-195">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="1db61-196">Pouze vedoucí mohli schválit nebo odmítnout změny obsahu (nové nebo změněné).</span><span class="sxs-lookup"><span data-stu-id="1db61-196">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="1db61-197">Vytvořte obslužnou rutinu Správce autorizací</span><span class="sxs-lookup"><span data-stu-id="1db61-197">Create an administrator authorization handler</span></span>

<span data-ttu-id="1db61-198">Vytvoření `ContactAdministratorsAuthorizationHandler` třídy v *autorizace* složky.</span><span class="sxs-lookup"><span data-stu-id="1db61-198">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="1db61-199">`ContactAdministratorsAuthorizationHandler` Ověřuje uživatele na prostředek je správce.</span><span class="sxs-lookup"><span data-stu-id="1db61-199">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="1db61-200">Správce může provádět všechny operace.</span><span class="sxs-lookup"><span data-stu-id="1db61-200">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="1db61-201">Zaregistrujte obslužné rutiny autorizace</span><span class="sxs-lookup"><span data-stu-id="1db61-201">Register the authorization handlers</span></span>

<span data-ttu-id="1db61-202">Pomocí Entity Framework Core Services musí být zaregistrovaný pro [injektáž závislostí](xref:fundamentals/dependency-injection) pomocí [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="1db61-202">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="1db61-203">`ContactIsOwnerAuthorizationHandler` Používá ASP.NET Core [Identity](xref:security/authentication/identity), která je založená na Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="1db61-203">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="1db61-204">Obslužné rutiny zaregistrovat u kolekce služby jsou k dispozici na `ContactsController` prostřednictvím [injektáž závislostí](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="1db61-204">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="1db61-205">Přidejte následující kód do konce `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1db61-205">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="1db61-206">`ContactAdministratorsAuthorizationHandler` a `ContactManagerAuthorizationHandler` jsou přidány jako jednotlivé prvky.</span><span class="sxs-lookup"><span data-stu-id="1db61-206">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="1db61-207">Jednotlivé prvky jsou vzhledem k tomu, že se nepoužívají EF a všechny informace potřebné musí být `Context` parametr `HandleRequirementAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="1db61-207">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="1db61-208">Povolení podpory</span><span class="sxs-lookup"><span data-stu-id="1db61-208">Support authorization</span></span>

<span data-ttu-id="1db61-209">V této části se aktualizace stránky Razor a přidejte třídu požadavků operace.</span><span class="sxs-lookup"><span data-stu-id="1db61-209">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="1db61-210">Zkontrolujte požadavky na třídy kontaktní operace</span><span class="sxs-lookup"><span data-stu-id="1db61-210">Review the contact operations requirements class</span></span>

<span data-ttu-id="1db61-211">Zkontrolujte `ContactOperations` třídy.</span><span class="sxs-lookup"><span data-stu-id="1db61-211">Review the `ContactOperations` class.</span></span> <span data-ttu-id="1db61-212">Tato třída obsahuje požadavky na aplikace podporuje:</span><span class="sxs-lookup"><span data-stu-id="1db61-212">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="1db61-213">Vytvoření základní třídu pro stránky Razor kontakty</span><span class="sxs-lookup"><span data-stu-id="1db61-213">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="1db61-214">Vytvořte základní třídu, která obsahuje služby využité v kontaktech stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="1db61-214">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="1db61-215">Základní třída vloží kód inicializace na jednom místě:</span><span class="sxs-lookup"><span data-stu-id="1db61-215">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="1db61-216">Předchozí kód:</span><span class="sxs-lookup"><span data-stu-id="1db61-216">The preceding code:</span></span>

* <span data-ttu-id="1db61-217">Přidá `IAuthorizationService` služby pro přístup k rutinám autorizace.</span><span class="sxs-lookup"><span data-stu-id="1db61-217">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="1db61-218">Přidá identitu `UserManager` služby.</span><span class="sxs-lookup"><span data-stu-id="1db61-218">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="1db61-219">Přidat `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="1db61-219">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="1db61-220">Aktualizace CreateModel</span><span class="sxs-lookup"><span data-stu-id="1db61-220">Update the CreateModel</span></span>

<span data-ttu-id="1db61-221">Aktualizovat konstruktoru vytvořit stránku model použití `DI_BasePageModel` základní třídy:</span><span class="sxs-lookup"><span data-stu-id="1db61-221">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="1db61-222">Aktualizace `CreateModel.OnPostAsync` metodu:</span><span class="sxs-lookup"><span data-stu-id="1db61-222">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="1db61-223">Přidat ID uživatele, `Contact` modelu.</span><span class="sxs-lookup"><span data-stu-id="1db61-223">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="1db61-224">Ověřte, zda že má uživatel oprávnění k vytvoření kontakty obslužná rutina ověřování volejte.</span><span class="sxs-lookup"><span data-stu-id="1db61-224">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="1db61-225">Aktualizace IndexModel</span><span class="sxs-lookup"><span data-stu-id="1db61-225">Update the IndexModel</span></span>

<span data-ttu-id="1db61-226">Aktualizace `OnGetAsync` metody, jsou uvedeny pouze schválené kontakty obecné uživatelům:</span><span class="sxs-lookup"><span data-stu-id="1db61-226">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="1db61-227">Aktualizace EditModel</span><span class="sxs-lookup"><span data-stu-id="1db61-227">Update the EditModel</span></span>

<span data-ttu-id="1db61-228">Přidejte obslužnou rutinu ověřování k ověření, že uživatel je vlastníkem kontaktu.</span><span class="sxs-lookup"><span data-stu-id="1db61-228">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="1db61-229">Protože ra se ověřuje, `[Authorize]` atribut není dostatek.</span><span class="sxs-lookup"><span data-stu-id="1db61-229">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="1db61-230">Aplikace nemá přístup k prostředku, když jsou vyhodnocovány atributy.</span><span class="sxs-lookup"><span data-stu-id="1db61-230">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="1db61-231">Autorizace na základě prostředků musí být nezbytné.</span><span class="sxs-lookup"><span data-stu-id="1db61-231">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="1db61-232">Kontroly musí provést, když má aplikace přístup k prostředku načtením v modelu stránky nebo načtením v rámci samotná obslužná rutina.</span><span class="sxs-lookup"><span data-stu-id="1db61-232">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="1db61-233">Často přistupovat k prostředku předáním klíč prostředku.</span><span class="sxs-lookup"><span data-stu-id="1db61-233">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="1db61-234">Aktualizace DeleteModel</span><span class="sxs-lookup"><span data-stu-id="1db61-234">Update the DeleteModel</span></span>

<span data-ttu-id="1db61-235">Aktualizace modelu stránku odstranit sloužící k ověření, že uživatel má oprávnění odstranit u kontaktu obslužnou rutinu ověřování.</span><span class="sxs-lookup"><span data-stu-id="1db61-235">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="1db61-236">Vložit do zobrazení autorizační službu</span><span class="sxs-lookup"><span data-stu-id="1db61-236">Inject the authorization service into the views</span></span>

<span data-ttu-id="1db61-237">V současné době ukazuje uživatelského rozhraní upravovat a odstraňovat kontakty, které uživatel nemůže upravovat odkazy.</span><span class="sxs-lookup"><span data-stu-id="1db61-237">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="1db61-238">Vloží autorizační službu v *Views/_ViewImports.cshtml* souboru tak, aby byl k dispozici pro všechna zobrazení:</span><span class="sxs-lookup"><span data-stu-id="1db61-238">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="1db61-239">Předchozí kód přidá několik `using` příkazy.</span><span class="sxs-lookup"><span data-stu-id="1db61-239">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="1db61-240">Aktualizace **upravit** a **odstranit** odkazů v *Pages/Contacts/Index.cshtml* tak, že pouze vykreslen pro uživatele s příslušnými oprávněními:</span><span class="sxs-lookup"><span data-stu-id="1db61-240">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="1db61-241">Skrytí propojení od uživatele, kteří nemají oprávnění ke změně dat nebude zabezpečení aplikace.</span><span class="sxs-lookup"><span data-stu-id="1db61-241">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="1db61-242">Skrytí propojení nastaví aplikaci jako přívětivější tím, že zobrazuje pouze platné odkazy.</span><span class="sxs-lookup"><span data-stu-id="1db61-242">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="1db61-243">Uživatelé můžou hack generované adres URL pro vyvolání funkce upravit a odstranit operací s daty, které není vlastní.</span><span class="sxs-lookup"><span data-stu-id="1db61-243">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="1db61-244">Stránka Razor nebo kontroleru musí vynucovat kontroly přístupu pro data zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="1db61-244">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="1db61-245">Podrobné informace o aktualizaci</span><span class="sxs-lookup"><span data-stu-id="1db61-245">Update Details</span></span>

<span data-ttu-id="1db61-246">Aktualizujte zobrazení podrobností, aby vedoucí mohli schválit nebo odmítnout kontaktů:</span><span class="sxs-lookup"><span data-stu-id="1db61-246">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="1db61-247">Model stránky podrobnosti aktualizace:</span><span class="sxs-lookup"><span data-stu-id="1db61-247">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-a-user-to-a-role"></a><span data-ttu-id="1db61-248">Přidání uživatele do role</span><span class="sxs-lookup"><span data-stu-id="1db61-248">Add a user to a role</span></span>

<span data-ttu-id="1db61-249">Role jsou uloženy v souboru cookie Identity.</span><span class="sxs-lookup"><span data-stu-id="1db61-249">Roles are stored in the Identity cookie.</span></span> <span data-ttu-id="1db61-250">Změny provedené na uživatelské role nejsou trvale uložena do souboru cookie, dokud je znova vygeneroval soubor cookie nebo uživatele odhlášení a přihlášení.</span><span class="sxs-lookup"><span data-stu-id="1db61-250">Changes made to user roles are not persisted to the cookie until the cookie is regenerated or the user signs out and signs in.</span></span> <span data-ttu-id="1db61-251">Aplikace, které se přidání uživatele k roli by měly volat `SignInManager.RefreshSignInAsync(user)` k aktualizaci souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="1db61-251">Applications that add users to a role should call `SignInManager.RefreshSignInAsync(user)` to update the cookie.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="1db61-252">Testování dokončené aplikace</span><span class="sxs-lookup"><span data-stu-id="1db61-252">Test the completed app</span></span>

<span data-ttu-id="1db61-253">Pokud má kontaktů:</span><span class="sxs-lookup"><span data-stu-id="1db61-253">If the app has contacts:</span></span>

* <span data-ttu-id="1db61-254">Odstranění všech záznamů v `Contact` tabulky.</span><span class="sxs-lookup"><span data-stu-id="1db61-254">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="1db61-255">Restartujte aplikaci k přidání dat do databáze.</span><span class="sxs-lookup"><span data-stu-id="1db61-255">Restart the app to seed the database.</span></span>

<span data-ttu-id="1db61-256">Zaregistrujte pro procházení kontakty uživatele.</span><span class="sxs-lookup"><span data-stu-id="1db61-256">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="1db61-257">Snadný způsob, jak otestovat dokončená aplikace je spustíte tři různé prohlížeče (nebo incognito/InPrivate verze).</span><span class="sxs-lookup"><span data-stu-id="1db61-257">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="1db61-258">V jeden prohlížeč, registraci nového uživatele (například `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="1db61-258">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="1db61-259">Přihlaste se k každým prohlížečem s jiným uživatelem.</span><span class="sxs-lookup"><span data-stu-id="1db61-259">Sign in to each browser with a different user.</span></span> <span data-ttu-id="1db61-260">Ověřte následující operace:</span><span class="sxs-lookup"><span data-stu-id="1db61-260">Verify the following operations:</span></span>

* <span data-ttu-id="1db61-261">Registrovaných uživatelů můžete zobrazit všechny schválené kontaktní údaje.</span><span class="sxs-lookup"><span data-stu-id="1db61-261">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="1db61-262">Registrovaných uživatelů můžete upravit nebo odstranit svá vlastní data.</span><span class="sxs-lookup"><span data-stu-id="1db61-262">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="1db61-263">Vedoucí mohli schválit nebo odmítnout kontaktní údaje.</span><span class="sxs-lookup"><span data-stu-id="1db61-263">Managers can approve or reject contact data.</span></span> <span data-ttu-id="1db61-264">`Details` Zobrazení ukazuje **schválit** a **odmítnout** tlačítka.</span><span class="sxs-lookup"><span data-stu-id="1db61-264">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="1db61-265">Správci můžou schvalovat a odmítat a upravit nebo odstranit všechna data.</span><span class="sxs-lookup"><span data-stu-id="1db61-265">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="1db61-266">Uživatel</span><span class="sxs-lookup"><span data-stu-id="1db61-266">User</span></span>| <span data-ttu-id="1db61-267">Možnosti</span><span class="sxs-lookup"><span data-stu-id="1db61-267">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="1db61-268">Můžete upravit nebo odstranit vlastní data</span><span class="sxs-lookup"><span data-stu-id="1db61-268">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="1db61-269">Můžete schvalovat a odmítat a upravit nebo odstranit data jsou vaše vlastnictví</span><span class="sxs-lookup"><span data-stu-id="1db61-269">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="1db61-270">Můžete upravit nebo odstranit a schvalovat a odmítat všechna data</span><span class="sxs-lookup"><span data-stu-id="1db61-270">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="1db61-271">Vytvoření kontaktu v prohlížeči na správce.</span><span class="sxs-lookup"><span data-stu-id="1db61-271">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="1db61-272">Zkopírujte adresu URL pro odstranění a upravit z kontaktujte správce.</span><span class="sxs-lookup"><span data-stu-id="1db61-272">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="1db61-273">Vložte tyto odkazy do testů webového prohlížeče k ověření, že testovací uživatel nemůže provádět tyto operace.</span><span class="sxs-lookup"><span data-stu-id="1db61-273">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="1db61-274">Vytvořit úvodní aplikaci</span><span class="sxs-lookup"><span data-stu-id="1db61-274">Create the starter app</span></span>

* <span data-ttu-id="1db61-275">Vytvoření aplikace stránky Razor s názvem "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="1db61-275">Create a Razor Pages app named "ContactManager"</span></span>
   * <span data-ttu-id="1db61-276">Vytvoření aplikace s **jednotlivých uživatelských účtů**.</span><span class="sxs-lookup"><span data-stu-id="1db61-276">Create the app with **Individual User Accounts**.</span></span>
   * <span data-ttu-id="1db61-277">Pojmenujte ji "ContactManager" tak obor názvů odpovídá oboru názvů použitého v ukázce.</span><span class="sxs-lookup"><span data-stu-id="1db61-277">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
   * <span data-ttu-id="1db61-278">`-uld` Určuje LocalDB místo SQLite</span><span class="sxs-lookup"><span data-stu-id="1db61-278">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="1db61-279">Přidat *Models\Contact.cs*:</span><span class="sxs-lookup"><span data-stu-id="1db61-279">Add *Models\Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="1db61-280">Vygenerované uživatelské rozhraní `Contact` modelu.</span><span class="sxs-lookup"><span data-stu-id="1db61-280">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="1db61-281">Vytvořte počáteční migraci a aktualizaci databáze:</span><span class="sxs-lookup"><span data-stu-id="1db61-281">Create initial migration and update the database:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="1db61-282">Aktualizace **ContactManager** ukotvit v *Pages/_Layout.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="1db61-282">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="1db61-283">Testování aplikace pomocí vytváření, úpravy a odstranění kontaktu</span><span class="sxs-lookup"><span data-stu-id="1db61-283">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="1db61-284">Přidání dat do databáze</span><span class="sxs-lookup"><span data-stu-id="1db61-284">Seed the database</span></span>

<span data-ttu-id="1db61-285">Přidat [SeedData](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) třídu *Data* složky.</span><span class="sxs-lookup"><span data-stu-id="1db61-285">Add the [SeedData](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="1db61-286">Volání `SeedData.Initialize` z `Main`:</span><span class="sxs-lookup"><span data-stu-id="1db61-286">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="1db61-287">Otestujte, že aplikace naplnila databázi.</span><span class="sxs-lookup"><span data-stu-id="1db61-287">Test that the app seeded the database.</span></span> <span data-ttu-id="1db61-288">Pokud existují nějaké řádky v kontaktu DB, metoda počáteční hodnoty není spuštěna.</span><span class="sxs-lookup"><span data-stu-id="1db61-288">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="1db61-289">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="1db61-289">Additional resources</span></span>

* <span data-ttu-id="1db61-290">[ASP.NET Core povolení Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="1db61-290">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="1db61-291">Toto testovací prostředí obsahuje větší podrobnosti o funkcích zabezpečení v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="1db61-291">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="1db61-292">Autorizace v ASP.NET Core: jednoduchý, role, založené na deklaracích a vlastní</span><span class="sxs-lookup"><span data-stu-id="1db61-292">Authorization in ASP.NET Core: Simple, role, claims-based, and custom</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="1db61-293">Autorizace na základě zásad</span><span class="sxs-lookup"><span data-stu-id="1db61-293">Custom policy-based authorization</span></span>](xref:security/authorization/policies)

::: moniker-end
