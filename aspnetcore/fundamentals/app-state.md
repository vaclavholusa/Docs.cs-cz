---
title: Stav relace a aplikace v ASP.NET Core
author: rick-anderson
description: Zjistit přístupy k zachování stavu relace a aplikace mezi požadavky.
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2018
uid: fundamentals/app-state
ms.openlocfilehash: 9c63d9313acb055e6c692a7fef3d28e94cb37093
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272880"
---
# <a name="session-and-app-state-in-aspnet-core"></a><span data-ttu-id="f63a9-103">Stav relace a aplikace v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f63a9-103">Session and app state in ASP.NET Core</span></span>

<span data-ttu-id="f63a9-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), [Diana LaRose](https://github.com/DianaLaRose), a [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f63a9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), [Diana LaRose](https://github.com/DianaLaRose), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f63a9-105">HTTP je bezstavový.</span><span class="sxs-lookup"><span data-stu-id="f63a9-105">HTTP is a stateless protocol.</span></span> <span data-ttu-id="f63a9-106">Bez nutnosti převádět další kroky, požadavky HTTP jsou nezávislé zprávy, které nezachovají hodnoty uživatele nebo stav aplikace.</span><span class="sxs-lookup"><span data-stu-id="f63a9-106">Without taking additional steps, HTTP requests are independent messages that don't retain user values or app state.</span></span> <span data-ttu-id="f63a9-107">Tento článek popisuje několik přístupů, které chcete zachovat stav aplikací a dat uživatele mezi požadavky.</span><span class="sxs-lookup"><span data-stu-id="f63a9-107">This article describes several approaches to preserve user data and app state between requests.</span></span>

<span data-ttu-id="f63a9-108">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f63a9-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="state-management"></a><span data-ttu-id="f63a9-109">Stav správy</span><span class="sxs-lookup"><span data-stu-id="f63a9-109">State management</span></span>

<span data-ttu-id="f63a9-110">Stav může být uložen několik přístupů.</span><span class="sxs-lookup"><span data-stu-id="f63a9-110">State can be stored using several approaches.</span></span> <span data-ttu-id="f63a9-111">Každý postup je popsán dále v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="f63a9-111">Each approach is described later in this topic.</span></span>

| <span data-ttu-id="f63a9-112">Přístup k ukládání</span><span class="sxs-lookup"><span data-stu-id="f63a9-112">Storage approach</span></span> | <span data-ttu-id="f63a9-113">Mechanismus úložiště</span><span class="sxs-lookup"><span data-stu-id="f63a9-113">Storage mechanism</span></span> |
| ---------------- | ----------------- |
| [<span data-ttu-id="f63a9-114">Soubory cookie</span><span class="sxs-lookup"><span data-stu-id="f63a9-114">Cookies</span></span>](#cookies) | <span data-ttu-id="f63a9-115">Soubory cookie protokolu HTTP (mohou zahrnovat data uložená pomocí kódu aplikace na straně serveru)</span><span class="sxs-lookup"><span data-stu-id="f63a9-115">HTTP cookies (may include data stored using server-side app code)</span></span> |
| [<span data-ttu-id="f63a9-116">Stav relace</span><span class="sxs-lookup"><span data-stu-id="f63a9-116">Session state</span></span>](#session-state) | <span data-ttu-id="f63a9-117">Soubory cookie protokolu HTTP a kód aplikace na straně serveru</span><span class="sxs-lookup"><span data-stu-id="f63a9-117">HTTP cookies and server-side app code</span></span> |
| [<span data-ttu-id="f63a9-118">TempData</span><span class="sxs-lookup"><span data-stu-id="f63a9-118">TempData</span></span>](#tempdata) | <span data-ttu-id="f63a9-119">Soubory cookie protokolu HTTP nebo stav relace</span><span class="sxs-lookup"><span data-stu-id="f63a9-119">HTTP cookies or session state</span></span> |
| [<span data-ttu-id="f63a9-120">Řetězce dotazů</span><span class="sxs-lookup"><span data-stu-id="f63a9-120">Query strings</span></span>](#query-strings) | <span data-ttu-id="f63a9-121">Řetězce dotazu HTTP</span><span class="sxs-lookup"><span data-stu-id="f63a9-121">HTTP query strings</span></span> |
| [<span data-ttu-id="f63a9-122">Skrytá pole</span><span class="sxs-lookup"><span data-stu-id="f63a9-122">Hidden fields</span></span>](#hidden-fields) | <span data-ttu-id="f63a9-123">Pole formuláře HTTP</span><span class="sxs-lookup"><span data-stu-id="f63a9-123">HTTP form fields</span></span> |
| [<span data-ttu-id="f63a9-124">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="f63a9-124">HttpContext.Items</span></span>](#httpcontextitems) | <span data-ttu-id="f63a9-125">Kód aplikace na straně serveru</span><span class="sxs-lookup"><span data-stu-id="f63a9-125">Server-side app code</span></span> |
| [<span data-ttu-id="f63a9-126">Mezipaměti</span><span class="sxs-lookup"><span data-stu-id="f63a9-126">Cache</span></span>](#cache) | <span data-ttu-id="f63a9-127">Kód aplikace na straně serveru</span><span class="sxs-lookup"><span data-stu-id="f63a9-127">Server-side app code</span></span> |
| [<span data-ttu-id="f63a9-128">Injektáž závislostí</span><span class="sxs-lookup"><span data-stu-id="f63a9-128">Dependency Injection</span></span>](#dependency-injection) | <span data-ttu-id="f63a9-129">Kód aplikace na straně serveru</span><span class="sxs-lookup"><span data-stu-id="f63a9-129">Server-side app code</span></span> |

## <a name="cookies"></a><span data-ttu-id="f63a9-130">Soubory cookie</span><span class="sxs-lookup"><span data-stu-id="f63a9-130">Cookies</span></span>

<span data-ttu-id="f63a9-131">Soubory cookie ukládat data napříč požadavky.</span><span class="sxs-lookup"><span data-stu-id="f63a9-131">Cookies store data across requests.</span></span> <span data-ttu-id="f63a9-132">Soubory cookie jsou odesílány s každou žádost, jejich velikost měli omezit na minimum.</span><span class="sxs-lookup"><span data-stu-id="f63a9-132">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="f63a9-133">V ideálním případě by měly jenom identifikátor uložené v souboru cookie s daty uloženými aplikací.</span><span class="sxs-lookup"><span data-stu-id="f63a9-133">Ideally, only an identifier should be stored in a cookie with the data stored by the app.</span></span> <span data-ttu-id="f63a9-134">Většina prohlížečů omezit velikost souboru cookie na 4096 bajtů.</span><span class="sxs-lookup"><span data-stu-id="f63a9-134">Most browsers restrict cookie size to 4096 bytes.</span></span> <span data-ttu-id="f63a9-135">Pouze omezený počet souborů cookie jsou k dispozici pro každou doménu.</span><span class="sxs-lookup"><span data-stu-id="f63a9-135">Only a limited number of cookies are available for each domain.</span></span>

<span data-ttu-id="f63a9-136">Protože soubory cookie se vztahují manipulaci, musí být ověřený aplikací.</span><span class="sxs-lookup"><span data-stu-id="f63a9-136">Because cookies are subject to tampering, they must be validated by the app.</span></span> <span data-ttu-id="f63a9-137">Soubory cookie může odstranit uživatele a vyprší na klientských počítačích.</span><span class="sxs-lookup"><span data-stu-id="f63a9-137">Cookies can be deleted by users and expire on clients.</span></span> <span data-ttu-id="f63a9-138">Soubory cookie jsou však obecně nejvíce trvanlivá formu trvalosti dat na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="f63a9-138">However, cookies are generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="f63a9-139">Soubory cookie se často používají pro přizpůsobení, kde je obsah přizpůsobit pro známé uživatele.</span><span class="sxs-lookup"><span data-stu-id="f63a9-139">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="f63a9-140">Uživatel je pouze identifikovat a ve většině případů není ověřen.</span><span class="sxs-lookup"><span data-stu-id="f63a9-140">The user is only identified and not authenticated in most cases.</span></span> <span data-ttu-id="f63a9-141">Soubor cookie může ukládat jméno uživatele, název účtu nebo jedinečné ID uživatele (například identifikátor GUID).</span><span class="sxs-lookup"><span data-stu-id="f63a9-141">The cookie can store the user's name, account name, or unique user ID (such as a GUID).</span></span> <span data-ttu-id="f63a9-142">Pak můžete soubor cookie pro přístup k individuální nastavení uživatele, jako je například jejich barvu pozadí upřednostňované webu.</span><span class="sxs-lookup"><span data-stu-id="f63a9-142">You can then use the cookie to access the user's personalized settings, such as their preferred website background color.</span></span>

<span data-ttu-id="f63a9-143">Mělo pamatovat [Evropské unie obecné Data Protection předpisy (GDPR)](https://ec.europa.eu/info/law/law-topic/data-protection) při vydávání soubory cookie a týkajících se ochrany osobních údajů týká.</span><span class="sxs-lookup"><span data-stu-id="f63a9-143">Be mindful of the [European Union General Data Protection Regulations (GDPR)](https://ec.europa.eu/info/law/law-topic/data-protection) when issuing cookies and dealing with privacy concerns.</span></span> <span data-ttu-id="f63a9-144">Další informace najdete v tématu [podporu obecné Data Protection nařízení (GDPR) v ASP.NET Core](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="f63a9-144">For more information, see [General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr).</span></span>

## <a name="session-state"></a><span data-ttu-id="f63a9-145">Stav relace</span><span class="sxs-lookup"><span data-stu-id="f63a9-145">Session state</span></span>

<span data-ttu-id="f63a9-146">Stav relace se o scénář ASP.NET Core pro ukládání dat uživatele, když uživatel prohlíží webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f63a9-146">Session state is an ASP.NET Core scenario for storage of user data while the user browses a web app.</span></span> <span data-ttu-id="f63a9-147">Stav relace používá úložiště spravuje pomocí aplikace pro uložení dat napříč požadavky klientů.</span><span class="sxs-lookup"><span data-stu-id="f63a9-147">Session state uses a store maintained by the app to persist data across requests from a client.</span></span> <span data-ttu-id="f63a9-148">Data relace je zálohovaný mezipaměti a považována za dočasných dat&mdash;webu by měly být nadále fungovat bez data relace.</span><span class="sxs-lookup"><span data-stu-id="f63a9-148">The session data is backed by a cache and considered ephemeral data&mdash;the site should continue to function without the session data.</span></span>

> [!NOTE]
> <span data-ttu-id="f63a9-149">Relace není podporována v [SignalR](xref:signalr/index) aplikace protože [rozbočovače SignalR](xref:signalr/hubs) může spustit nezávisle na kontextu HTTP.</span><span class="sxs-lookup"><span data-stu-id="f63a9-149">Session isn't supported in [SignalR](xref:signalr/index) apps because a [SignalR Hub](xref:signalr/hubs) may execute independent of an HTTP context.</span></span> <span data-ttu-id="f63a9-150">Například to může dojít, když typ long požadavku dotazování je otevřena podle rozbočovač za dobu životnosti kontext žádosti HTTP.</span><span class="sxs-lookup"><span data-stu-id="f63a9-150">For example, this can occur when a long polling request is held open by a hub beyond the lifetime of the request's HTTP context.</span></span>

<span data-ttu-id="f63a9-151">ASP.NET Core Udržovat stav relace tím, že poskytuje soubor cookie klientovi, který obsahuje ID relace, které se odesílají do aplikace s každou žádostí.</span><span class="sxs-lookup"><span data-stu-id="f63a9-151">ASP.NET Core maintains session state by providing a cookie to the client that contains a session ID, which is sent to the app with each request.</span></span> <span data-ttu-id="f63a9-152">Aplikace používá ID relace se načíst data relace.</span><span class="sxs-lookup"><span data-stu-id="f63a9-152">The app uses the session ID to fetch the session data.</span></span>

<span data-ttu-id="f63a9-153">Stav relace projevuje následující chování:</span><span class="sxs-lookup"><span data-stu-id="f63a9-153">Session state exhibits the following behaviors:</span></span>

* <span data-ttu-id="f63a9-154">Vzhledem k tomu, že soubor cookie relace prohlížeč, relace nejsou sdílená mezi prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="f63a9-154">Because the session cookie is specific to the browser, sessions aren't shared across browsers.</span></span>
* <span data-ttu-id="f63a9-155">Soubory cookie relací jsou odstraněny při ukončení relace prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="f63a9-155">Session cookies are deleted when the browser session ends.</span></span>
* <span data-ttu-id="f63a9-156">Pokud je soubor cookie pro relaci s ukončenou platností, je vytvořit novou relaci, který používá stejný soubor cookie relace.</span><span class="sxs-lookup"><span data-stu-id="f63a9-156">If a cookie is received for an expired session, a new session is created that uses the same session cookie.</span></span>
* <span data-ttu-id="f63a9-157">Prázdný relace nejsou uchována&mdash;relace musí mít alespoň jednu hodnotu nastavit do ní k uchování relace napříč požadavky.</span><span class="sxs-lookup"><span data-stu-id="f63a9-157">Empty sessions aren't retained&mdash;the session must have at least one value set into it to persist the session across requests.</span></span> <span data-ttu-id="f63a9-158">Když relace není zachována, nové ID relace se generuje pro každý nový požadavek.</span><span class="sxs-lookup"><span data-stu-id="f63a9-158">When a session isn't retained, a new session ID is generated for each new request.</span></span>
* <span data-ttu-id="f63a9-159">Aplikace uchovává relace po omezenou dobu od poslední žádosti.</span><span class="sxs-lookup"><span data-stu-id="f63a9-159">The app retains a session for a limited time after the last request.</span></span> <span data-ttu-id="f63a9-160">Aplikace načte časový limit relace nebo výchozí hodnotu 20 minut.</span><span class="sxs-lookup"><span data-stu-id="f63a9-160">The app either sets the session timeout or uses the default value of 20 minutes.</span></span> <span data-ttu-id="f63a9-161">Stav relace je ideální pro ukládání uživatelských dat, která je specifická pro konkrétní relace, ale kde data nevyžaduje trvalé úložiště napříč relacemi.</span><span class="sxs-lookup"><span data-stu-id="f63a9-161">Session state is ideal for storing user data that's specific to a particular session but where the data doesn't require permanent storage across sessions.</span></span>
* <span data-ttu-id="f63a9-162">Data relace se odstraní buď když [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) je volána implementace nebo vypršení platnosti relace.</span><span class="sxs-lookup"><span data-stu-id="f63a9-162">Session data is deleted either when the [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) implementation is called or when the session expires.</span></span>
* <span data-ttu-id="f63a9-163">Neexistuje žádný výchozí mechanismus k informování kód aplikace bylo ukončeno prohlížeče klienta nebo když je soubor cookie relace odstranit nebo platnost na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="f63a9-163">There's no default mechanism to inform app code that a client browser has been closed or when the session cookie is deleted or expired on the client.</span></span>

> [!WARNING]
> <span data-ttu-id="f63a9-164">Citlivá data neukládejte do stavu relace.</span><span class="sxs-lookup"><span data-stu-id="f63a9-164">Don't store sensitive data in session state.</span></span> <span data-ttu-id="f63a9-165">Uživatel nemusí zavřete prohlížeč a zrušte souboru cookie relace.</span><span class="sxs-lookup"><span data-stu-id="f63a9-165">The user might not close the browser and clear the session cookie.</span></span> <span data-ttu-id="f63a9-166">Některé prohlížeče soubory cookie relace platná udržovat napříč okna prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="f63a9-166">Some browsers maintain valid session cookies across browser windows.</span></span> <span data-ttu-id="f63a9-167">Relaci nemusí být omezen na jednoho uživatele&mdash;další uživatel může pokračovat ve procházet aplikace pomocí stejného souboru cookie relace.</span><span class="sxs-lookup"><span data-stu-id="f63a9-167">A session might not be restricted to a single user&mdash;the next user might continue to browse the app with the same session cookie.</span></span>

<span data-ttu-id="f63a9-168">Poskytovatel mezipaměti v paměti ukládá data relace v paměti serveru, na kterém se nachází aplikace.</span><span class="sxs-lookup"><span data-stu-id="f63a9-168">The in-memory cache provider stores session data in the memory of the server where the app resides.</span></span> <span data-ttu-id="f63a9-169">Ve scénáři farmy serveru:</span><span class="sxs-lookup"><span data-stu-id="f63a9-169">In a server farm scenario:</span></span>

* <span data-ttu-id="f63a9-170">Použití *trvalé relace* ke svázání každou relaci do instance konkrétní aplikaci na individuálním serveru.</span><span class="sxs-lookup"><span data-stu-id="f63a9-170">Use *sticky sessions* to tie each session to a specific app instance on an individual server.</span></span> <span data-ttu-id="f63a9-171">[Aplikační služba Azure](https://azure.microsoft.com/services/app-service/) používá [aplikace směrování žádostí na](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) vynutit trvalé relace ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="f63a9-171">[Azure App Service](https://azure.microsoft.com/services/app-service/) uses [Application Request Routing (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) to enforce sticky sessions by default.</span></span> <span data-ttu-id="f63a9-172">Trvalé relace však může mít vliv na škálovatelnost a zkomplikovat aktualizace webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f63a9-172">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="f63a9-173">Lepší možných přístupů je použít Redis nebo SQL Server distribuované mezipaměti, která nevyžaduje trvalé relace.</span><span class="sxs-lookup"><span data-stu-id="f63a9-173">A better approach is to use a Redis or SQL Server distributed cache, which doesn't require sticky sessions.</span></span> <span data-ttu-id="f63a9-174">Další informace najdete v tématu [pracovat s distribuované mezipaměti](xref:performance/caching/distributed).</span><span class="sxs-lookup"><span data-stu-id="f63a9-174">For more information, see [Work with a distributed cache](xref:performance/caching/distributed).</span></span>
* <span data-ttu-id="f63a9-175">Soubor cookie relace se šifruje pomocí [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector).</span><span class="sxs-lookup"><span data-stu-id="f63a9-175">The session cookie is encrypted via [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector).</span></span> <span data-ttu-id="f63a9-176">Ochrana dat musí být správně nakonfigurované ke čtení souborů cookie relací na každém počítači.</span><span class="sxs-lookup"><span data-stu-id="f63a9-176">Data Protection must be properly configured to read session cookies on each machine.</span></span> <span data-ttu-id="f63a9-177">Další informace najdete v tématu [ochrany dat v ASP.NET Core](xref:security/data-protection/index) a [klíče poskytovatelů úložiště](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="f63a9-177">For more information, see [Data Protection in ASP.NET Core](xref:security/data-protection/index) and [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

### <a name="configure-session-state"></a><span data-ttu-id="f63a9-178">Nakonfigurovat stav relace</span><span class="sxs-lookup"><span data-stu-id="f63a9-178">Configure session state</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f63a9-179">[Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) balíček, který je součástí [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), poskytuje middleware pro správu stavu relace.</span><span class="sxs-lookup"><span data-stu-id="f63a9-179">The [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), provides middleware for managing session state.</span></span> <span data-ttu-id="f63a9-180">Povolit middleware relace `Startup` musí obsahovat:</span><span class="sxs-lookup"><span data-stu-id="f63a9-180">To enable the session middleware, `Startup` must contain:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f63a9-181">[Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) balíček poskytuje middleware pro správu stavu relace.</span><span class="sxs-lookup"><span data-stu-id="f63a9-181">The [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package provides middleware for managing session state.</span></span> <span data-ttu-id="f63a9-182">Povolit middleware relace `Startup` musí obsahovat:</span><span class="sxs-lookup"><span data-stu-id="f63a9-182">To enable the session middleware, `Startup` must contain:</span></span>

::: moniker-end

* <span data-ttu-id="f63a9-183">Některé z [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) mezipaměti paměti.</span><span class="sxs-lookup"><span data-stu-id="f63a9-183">Any of the [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="f63a9-184">`IDistributedCache` Implementace slouží jako úložiště zálohování pro relaci.</span><span class="sxs-lookup"><span data-stu-id="f63a9-184">The `IDistributedCache` implementation is used as a backing store for session.</span></span> <span data-ttu-id="f63a9-185">Další informace najdete v tématu [pracovat s distribuované mezipaměti](xref:performance/caching/distributed).</span><span class="sxs-lookup"><span data-stu-id="f63a9-185">For more information, see [Work with a distributed cache](xref:performance/caching/distributed).</span></span>
* <span data-ttu-id="f63a9-186">Volání [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) v `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f63a9-186">A call to [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) in `ConfigureServices`.</span></span>
* <span data-ttu-id="f63a9-187">Volání [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) v `Configure`.</span><span class="sxs-lookup"><span data-stu-id="f63a9-187">A call to [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) in `Configure`.</span></span>

<span data-ttu-id="f63a9-188">Následující kód ukazuje, jak nastavit Zprostředkovatel relací v paměti s výchozí implementace v paměti `IDistributedCache`:</span><span class="sxs-lookup"><span data-stu-id="f63a9-188">The following code shows how to set up the in-memory session provider with a default in-memory implementation of `IDistributedCache`:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f63a9-189">[!code-csharp[](app-state/samples/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13-18,39)]</span><span class="sxs-lookup"><span data-stu-id="f63a9-189">[!code-csharp[](app-state/samples/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13-18,39)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f63a9-190">[!code-csharp[](app-state/samples/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5,7-12,19)]</span><span class="sxs-lookup"><span data-stu-id="f63a9-190">[!code-csharp[](app-state/samples/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5,7-12,19)]</span></span>

::: moniker-end

<span data-ttu-id="f63a9-191">Je důležité pořadí middleware.</span><span class="sxs-lookup"><span data-stu-id="f63a9-191">The order of middleware is important.</span></span> <span data-ttu-id="f63a9-192">V předchozím příkladu `InvalidOperationException` došlo k výjimce při `UseSession` je volána poté, co `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="f63a9-192">In the preceding example, an `InvalidOperationException` exception occurs when `UseSession` is invoked after `UseMvc`.</span></span> <span data-ttu-id="f63a9-193">Další informace najdete v tématu [řazení Middleware](xref:fundamentals/middleware/index#ordering).</span><span class="sxs-lookup"><span data-stu-id="f63a9-193">For more information, see [Middleware Ordering](xref:fundamentals/middleware/index#ordering).</span></span>

<span data-ttu-id="f63a9-194">[HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) je k dispozici po dokončení konfigurace stavu relace.</span><span class="sxs-lookup"><span data-stu-id="f63a9-194">[HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) is available after session state is configured.</span></span>

<span data-ttu-id="f63a9-195">`HttpContext.Session` Nelze získat přístup před `UseSession` byla volána.</span><span class="sxs-lookup"><span data-stu-id="f63a9-195">`HttpContext.Session` can't be accessed before `UseSession` has been called.</span></span>

<span data-ttu-id="f63a9-196">Nelze vytvořit novou relaci pomocí souboru cookie relace, po aplikaci byl zahájen zápis do datového proudu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="f63a9-196">A new session with a new session cookie can't be created after the app has begun writing to the response stream.</span></span> <span data-ttu-id="f63a9-197">Výjimka je zaznamená do protokolu webového serveru a nezobrazuje se v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="f63a9-197">The exception is recorded in the web server log and not displayed in the browser.</span></span>

### <a name="load-session-state-asynchronously"></a><span data-ttu-id="f63a9-198">Asynchronně načíst stav relace</span><span class="sxs-lookup"><span data-stu-id="f63a9-198">Load session state asynchronously</span></span>

<span data-ttu-id="f63a9-199">Výchozí zprostředkovatel relace v ASP.NET Core načte relace záznamů z základní [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) asynchronně pouze v případě zálohování úložiště [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) explicitně volána metoda před [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue), [nastavit](/dotnet/api/microsoft.aspnetcore.http.isession.set), nebo [odebrat](/dotnet/api/microsoft.aspnetcore.http.isession.remove) metody.</span><span class="sxs-lookup"><span data-stu-id="f63a9-199">The default session provider in ASP.NET Core loads session records from the underlying [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) backing store asynchronously only if the [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) method is explicitly called before the [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue), [Set](/dotnet/api/microsoft.aspnetcore.http.isession.set), or [Remove](/dotnet/api/microsoft.aspnetcore.http.isession.remove) methods.</span></span> <span data-ttu-id="f63a9-200">Pokud `LoadAsync` není jako první, základní záznam relace je načtena synchronně, což může způsobit snížení výkonu ve velkém měřítku.</span><span class="sxs-lookup"><span data-stu-id="f63a9-200">If `LoadAsync` isn't called first, the underlying session record is loaded synchronously, which can incur a performance penalty at scale.</span></span>

<span data-ttu-id="f63a9-201">Pokud chcete, aby aplikace vynutit tento vzor, zabalení [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) a [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) implementace s verzemi, které způsobí výjimku, pokud `LoadAsync` metoda není volána před provedením `TryGetValue`, `Set`, nebo `Remove`.</span><span class="sxs-lookup"><span data-stu-id="f63a9-201">To have apps enforce this pattern, wrap the [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method isn't called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="f63a9-202">Zaregistrujte zabalené verze v kontejneru služby.</span><span class="sxs-lookup"><span data-stu-id="f63a9-202">Register the wrapped versions in the services container.</span></span>

### <a name="session-options"></a><span data-ttu-id="f63a9-203">Možnosti relace</span><span class="sxs-lookup"><span data-stu-id="f63a9-203">Session options</span></span>

<span data-ttu-id="f63a9-204">Chcete-li přepsat výchozí hodnoty relace, použijte [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions).</span><span class="sxs-lookup"><span data-stu-id="f63a9-204">To override session defaults, use [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions).</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="f63a9-205">Možnost</span><span class="sxs-lookup"><span data-stu-id="f63a9-205">Option</span></span> | <span data-ttu-id="f63a9-206">Popis</span><span class="sxs-lookup"><span data-stu-id="f63a9-206">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="f63a9-207">Soubor cookie</span><span class="sxs-lookup"><span data-stu-id="f63a9-207">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookie) | <span data-ttu-id="f63a9-208">Určuje nastavení používaná k vytvoření souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="f63a9-208">Determines the settings used to create the cookie.</span></span> <span data-ttu-id="f63a9-209">[Název](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) výchozí [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span><span class="sxs-lookup"><span data-stu-id="f63a9-209">[Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) defaults to [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span></span> <span data-ttu-id="f63a9-210">[Cesta](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) výchozí [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span><span class="sxs-lookup"><span data-stu-id="f63a9-210">[Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) defaults to [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span></span> <span data-ttu-id="f63a9-211">[SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) výchozí [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`).</span><span class="sxs-lookup"><span data-stu-id="f63a9-211">[SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) defaults to [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`).</span></span> <span data-ttu-id="f63a9-212">[HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) výchozí `true`.</span><span class="sxs-lookup"><span data-stu-id="f63a9-212">[HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) defaults to `true`.</span></span> <span data-ttu-id="f63a9-213">[IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) výchozí `false`.</span><span class="sxs-lookup"><span data-stu-id="f63a9-213">[IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) defaults to `false`.</span></span> |
| [<span data-ttu-id="f63a9-214">IdleTimeout</span><span class="sxs-lookup"><span data-stu-id="f63a9-214">IdleTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | <span data-ttu-id="f63a9-215">`IdleTimeout` Ukazuje, jak dlouho relace může být nečinnosti, než jsou opuštění její obsah.</span><span class="sxs-lookup"><span data-stu-id="f63a9-215">The `IdleTimeout` indicates how long the session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="f63a9-216">Každý přístup relace obnoví časový limit.</span><span class="sxs-lookup"><span data-stu-id="f63a9-216">Each session access resets the timeout.</span></span> <span data-ttu-id="f63a9-217">Všimněte si, že to platí jenom pro obsah relace, není soubor cookie.</span><span class="sxs-lookup"><span data-stu-id="f63a9-217">Note this only applies to the content of the session, not the cookie.</span></span> <span data-ttu-id="f63a9-218">Výchozí hodnota je 20 minut.</span><span class="sxs-lookup"><span data-stu-id="f63a9-218">The default is 20 minutes.</span></span> |
| [<span data-ttu-id="f63a9-219">IOTimeout</span><span class="sxs-lookup"><span data-stu-id="f63a9-219">IOTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.iotimeout) | <span data-ttu-id="f63a9-220">Maximální množství času povoleny relace načíst z úložiště nebo provést zpět do úložiště.</span><span class="sxs-lookup"><span data-stu-id="f63a9-220">The maximim amount of time allowed to load a session from the store or to commit it back to the store.</span></span> <span data-ttu-id="f63a9-221">Všimněte si, že toto může platit pouze pro asynchronní operace.</span><span class="sxs-lookup"><span data-stu-id="f63a9-221">Note this may only apply to asynchronous operations.</span></span> <span data-ttu-id="f63a9-222">Tento časový limit můžete zakázat pomocí [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan).</span><span class="sxs-lookup"><span data-stu-id="f63a9-222">This timeout can be disabled using [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan).</span></span> <span data-ttu-id="f63a9-223">Výchozí hodnota je 1 minuta.</span><span class="sxs-lookup"><span data-stu-id="f63a9-223">The default is 1 minute.</span></span> |

<span data-ttu-id="f63a9-224">Relace používá ke sledování a identifikaci požadavků z jednoho prohlížeče do souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="f63a9-224">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="f63a9-225">Ve výchozím nastavení, je tento soubor cookie s názvem `.AspNetCore.Session`, a používá cestu `/`.</span><span class="sxs-lookup"><span data-stu-id="f63a9-225">By default, this cookie is named `.AspNetCore.Session`, and it uses a path of `/`.</span></span> <span data-ttu-id="f63a9-226">Protože výchozí soubor cookie není zadejte doménu, neprovede klientský skript k dispozici na stránce (protože [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) výchozí `true`).</span><span class="sxs-lookup"><span data-stu-id="f63a9-226">Because the cookie default doesn't specify a domain, it isn't made available to the client-side script on the page (because [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) defaults to `true`).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="f63a9-227">Možnost</span><span class="sxs-lookup"><span data-stu-id="f63a9-227">Option</span></span> | <span data-ttu-id="f63a9-228">Popis</span><span class="sxs-lookup"><span data-stu-id="f63a9-228">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="f63a9-229">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="f63a9-229">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiedomain) | <span data-ttu-id="f63a9-230">Určuje doménu použitou k vytvoření souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="f63a9-230">Determines the domain used to create the cookie.</span></span> <span data-ttu-id="f63a9-231">`CookieDomain` ve výchozím nastavení není nastaven.</span><span class="sxs-lookup"><span data-stu-id="f63a9-231">`CookieDomain` isn't set by default.</span></span> |
| [<span data-ttu-id="f63a9-232">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="f63a9-232">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiehttponly) | <span data-ttu-id="f63a9-233">Určuje, pokud v prohlížeči by měl povolit souboru cookie, ke kterým přistupují kódu JavaScript na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="f63a9-233">Determines if the browser should allow the cookie to be accessed by client-side JavaScript.</span></span> <span data-ttu-id="f63a9-234">Výchozí hodnota je `true`, což znamená, že soubor cookie je pouze předáván požadavkům HTTP a nebude zpřístupněn pro skript na stránce.</span><span class="sxs-lookup"><span data-stu-id="f63a9-234">The default is `true`, which means the cookie is only passed to HTTP requests and isn't made available to script on the page.</span></span> |
| [<span data-ttu-id="f63a9-235">CookieName</span><span class="sxs-lookup"><span data-stu-id="f63a9-235">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiename) | <span data-ttu-id="f63a9-236">Určuje název souboru cookie použitý k zachování ID relace.</span><span class="sxs-lookup"><span data-stu-id="f63a9-236">Determines the cookie name used to persist the session ID.</span></span> <span data-ttu-id="f63a9-237">Výchozí hodnota je [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span><span class="sxs-lookup"><span data-stu-id="f63a9-237">The default is [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span></span> |
| [<span data-ttu-id="f63a9-238">CookiePath</span><span class="sxs-lookup"><span data-stu-id="f63a9-238">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiepath) | <span data-ttu-id="f63a9-239">Určuje cestu použitou k vytvoření souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="f63a9-239">Determines the path used to create the cookie.</span></span> <span data-ttu-id="f63a9-240">Použije se výchozí hodnota [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span><span class="sxs-lookup"><span data-stu-id="f63a9-240">Defaults to [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span></span> |
| [<span data-ttu-id="f63a9-241">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="f63a9-241">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiesecure) | <span data-ttu-id="f63a9-242">Určuje, pokud je soubor cookie měl být přenášen pouze na požadavky HTTPS.</span><span class="sxs-lookup"><span data-stu-id="f63a9-242">Determines if the cookie should only be transmitted on HTTPS requests.</span></span> <span data-ttu-id="f63a9-243">Výchozí hodnota je [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`).</span><span class="sxs-lookup"><span data-stu-id="f63a9-243">The default is [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`).</span></span> |
| [<span data-ttu-id="f63a9-244">IdleTimeout</span><span class="sxs-lookup"><span data-stu-id="f63a9-244">IdleTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | <span data-ttu-id="f63a9-245">`IdleTimeout` Ukazuje, jak dlouho relace může být nečinnosti, než jsou opuštění její obsah.</span><span class="sxs-lookup"><span data-stu-id="f63a9-245">The `IdleTimeout` indicates how long the session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="f63a9-246">Každý přístup relace obnoví časový limit.</span><span class="sxs-lookup"><span data-stu-id="f63a9-246">Each session access resets the timeout.</span></span> <span data-ttu-id="f63a9-247">Všimněte si, že to platí jenom pro obsah relace, není soubor cookie.</span><span class="sxs-lookup"><span data-stu-id="f63a9-247">Note this only applies to the content of the session, not the cookie.</span></span> <span data-ttu-id="f63a9-248">Výchozí hodnota je 20 minut.</span><span class="sxs-lookup"><span data-stu-id="f63a9-248">The default is 20 minutes.</span></span> |

<span data-ttu-id="f63a9-249">Relace používá ke sledování a identifikaci požadavků z jednoho prohlížeče do souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="f63a9-249">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="f63a9-250">Ve výchozím nastavení, je tento soubor cookie s názvem `.AspNet.Session`, a používá cestu `/`.</span><span class="sxs-lookup"><span data-stu-id="f63a9-250">By default, this cookie is named `.AspNet.Session`, and it uses a path of `/`.</span></span>

::: moniker-end

<span data-ttu-id="f63a9-251">Chcete-li přepsat výchozí hodnoty souboru cookie relace, použijte `SessionOptions`:</span><span class="sxs-lookup"><span data-stu-id="f63a9-251">To override cookie session defaults, use `SessionOptions`:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f63a9-252">[!code-csharp[](app-state/samples_snapshot/2.x/SessionSample/Startup.cs?name=snippet1&highlight=13-18)]</span><span class="sxs-lookup"><span data-stu-id="f63a9-252">[!code-csharp[](app-state/samples_snapshot/2.x/SessionSample/Startup.cs?name=snippet1&highlight=13-18)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f63a9-253">[!code-csharp[](app-state/samples_snapshot/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5-9)]</span><span class="sxs-lookup"><span data-stu-id="f63a9-253">[!code-csharp[](app-state/samples_snapshot/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5-9)]</span></span>

::: moniker-end

<span data-ttu-id="f63a9-254">Aplikace používá [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) vlastnosti k určení, jak dlouho může být relace nečinnosti před jeho obsah do mezipaměti serveru jsou opuštění.</span><span class="sxs-lookup"><span data-stu-id="f63a9-254">The app uses the [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) property to determine how long a session can be idle before its contents in the server's cache are abandoned.</span></span> <span data-ttu-id="f63a9-255">Tato vlastnost je nezávislá na vypršení platnosti souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="f63a9-255">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="f63a9-256">Každý požadavek, který prochází [relace Middleware](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware) obnoví časový limit.</span><span class="sxs-lookup"><span data-stu-id="f63a9-256">Each request that passes through the [Session Middleware](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware) resets the timeout.</span></span>

<span data-ttu-id="f63a9-257">Stav relace je *bez uzamčení*.</span><span class="sxs-lookup"><span data-stu-id="f63a9-257">Session state is *non-locking*.</span></span> <span data-ttu-id="f63a9-258">Pokud dva požadavky současně pokusí změnit obsah relace, poslední žádosti přednost před první.</span><span class="sxs-lookup"><span data-stu-id="f63a9-258">If two requests simultaneously attempt to modify the contents of a session, the last request overrides the first.</span></span> <span data-ttu-id="f63a9-259">`Session` je implementovaný jako *souvislý relace*, což znamená, že se veškerý obsah ukládat společně.</span><span class="sxs-lookup"><span data-stu-id="f63a9-259">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="f63a9-260">Když dva požadavky požadovat upravte hodnoty různé relace, poslední žádosti může přepsat relace změny provedené při první.</span><span class="sxs-lookup"><span data-stu-id="f63a9-260">When two requests seek to modify different session values, the last request may override session changes made by the first.</span></span>

### <a name="set-and-get-session-values"></a><span data-ttu-id="f63a9-261">Nastavování a získávání hodnoty relace</span><span class="sxs-lookup"><span data-stu-id="f63a9-261">Set and get Session values</span></span>

<span data-ttu-id="f63a9-262">Stav relace je k němu přistupovat z stránky Razor [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) třídu nebo MVC [řadič](/dotnet/api/microsoft.aspnetcore.mvc.controller) třídy s [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session).</span><span class="sxs-lookup"><span data-stu-id="f63a9-262">Session state is accessed from a Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) class or MVC [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controller) class with [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session).</span></span> <span data-ttu-id="f63a9-263">Tato vlastnost je [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) implementace.</span><span class="sxs-lookup"><span data-stu-id="f63a9-263">This property is an [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f63a9-264">`ISession` Implementace poskytuje několik metod rozšíření pro sadu a načíst hodnoty celé číslo a řetězec.</span><span class="sxs-lookup"><span data-stu-id="f63a9-264">The `ISession` implementation provides several extension methods to set and retreive integer and string values.</span></span> <span data-ttu-id="f63a9-265">Rozšiřující metody jsou v [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) obor názvů (Přidat `using Microsoft.AspNetCore.Http;` příkaz k získání přístupu k rozšiřující metody) při [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) balíček je odkazován objektem projektu.</span><span class="sxs-lookup"><span data-stu-id="f63a9-265">The extension methods are in the [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) namespace (add a `using Microsoft.AspNetCore.Http;` statement to gain access to the extension methods) when the [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) package is referenced by the project.</span></span> <span data-ttu-id="f63a9-266">Oba balíčky jsou součástí [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="f63a9-266">Both packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f63a9-267">`ISession` Implementace poskytuje několik metod rozšíření pro sadu a načíst hodnoty celé číslo a řetězec.</span><span class="sxs-lookup"><span data-stu-id="f63a9-267">The `ISession` implementation provides several extension methods to set and retreive integer and string values.</span></span> <span data-ttu-id="f63a9-268">Rozšiřující metody jsou v [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) obor názvů (Přidat `using Microsoft.AspNetCore.Http;` příkaz k získání přístupu k rozšiřující metody) při [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) balíček je odkazován objektem projektu.</span><span class="sxs-lookup"><span data-stu-id="f63a9-268">The extension methods are in the [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) namespace (add a `using Microsoft.AspNetCore.Http;` statement to gain access to the extension methods) when the [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) package is referenced by the project.</span></span>

::: moniker-end

<span data-ttu-id="f63a9-269">`ISession` rozšiřující metody:</span><span class="sxs-lookup"><span data-stu-id="f63a9-269">`ISession` extension methods:</span></span>

* [<span data-ttu-id="f63a9-270">Get (ISession, řetězec)</span><span class="sxs-lookup"><span data-stu-id="f63a9-270">Get(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.get)
* [<span data-ttu-id="f63a9-271">GetInt32(ISession, String)</span><span class="sxs-lookup"><span data-stu-id="f63a9-271">GetInt32(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getint32)
* [<span data-ttu-id="f63a9-272">GetString – (ISession, řetězec)</span><span class="sxs-lookup"><span data-stu-id="f63a9-272">GetString(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getstring)
* [<span data-ttu-id="f63a9-273">Setint32 – (ISession, řetězec, Int32)</span><span class="sxs-lookup"><span data-stu-id="f63a9-273">SetInt32(ISession, String, Int32)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setint32)
* [<span data-ttu-id="f63a9-274">SetString – (ISession, řetězec, řetězec)</span><span class="sxs-lookup"><span data-stu-id="f63a9-274">SetString(ISession, String, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setstring)

<span data-ttu-id="f63a9-275">Následující příklad načte hodnotu relace `IndexModel.SessionKeyName` klíč (`_Name` v ukázkové aplikace) na stránce pro stránky Razor:</span><span class="sxs-lookup"><span data-stu-id="f63a9-275">The following example retrieves the session value for the `IndexModel.SessionKeyName` key (`_Name` in the sample app) in a Razor Pages page:</span></span>

```csharp
@page
@using Microsoft.AspNetCore.Http
@model IndexModel

...

Name: @HttpContext.Session.GetString(IndexModel.SessionKeyName)
```

<span data-ttu-id="f63a9-276">Následující příklad ukazuje, jak nastavit a získat celé číslo a řetězec:</span><span class="sxs-lookup"><span data-stu-id="f63a9-276">The following example shows how to set and get an integer and a string:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f63a9-277">[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=18-19,22-23)]</span><span class="sxs-lookup"><span data-stu-id="f63a9-277">[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=18-19,22-23)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f63a9-278">[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet1&highlight=10-11,18-19)]</span><span class="sxs-lookup"><span data-stu-id="f63a9-278">[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet1&highlight=10-11,18-19)]</span></span>

::: moniker-end

<span data-ttu-id="f63a9-279">Všechna data relace musí být serializované povolit scénáři distribuované mezipaměti, i když se používá mezipaměť v paměti.</span><span class="sxs-lookup"><span data-stu-id="f63a9-279">All session data must be serialized to enable a distributed cache scenario, even when using the in-memory cache.</span></span> <span data-ttu-id="f63a9-280">Jsou k dispozici minimální řetězec a číslo serializátorů (Viz metody a metodami rozšíření [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)).</span><span class="sxs-lookup"><span data-stu-id="f63a9-280">Minimal string and number serializers are provided (see the methods and extension methods of [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)).</span></span> <span data-ttu-id="f63a9-281">Komplexní typy musí být serializované uživatelem pomocí jiný mechanismus, jako je například JSON.</span><span class="sxs-lookup"><span data-stu-id="f63a9-281">Complex types must be serialized by the user using another mechanism, such as JSON.</span></span>

<span data-ttu-id="f63a9-282">Přidejte následující metody rozšíření nastavit a získat serializovatelné objekty:</span><span class="sxs-lookup"><span data-stu-id="f63a9-282">Add the following extension methods to set and get serializable objects:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f63a9-283">[!code-csharp[](app-state/samples/2.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="f63a9-283">[!code-csharp[](app-state/samples/2.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f63a9-284">[!code-csharp[](app-state/samples/1.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="f63a9-284">[!code-csharp[](app-state/samples/1.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]</span></span>

::: moniker-end

<span data-ttu-id="f63a9-285">Následující příklad ukazuje, jak nastavit a získat serializovatelný objekt pomocí metod rozšíření:</span><span class="sxs-lookup"><span data-stu-id="f63a9-285">The following example shows how to set and get a serializable object with the extension methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f63a9-286">[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="f63a9-286">[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet2)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f63a9-287">[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet2&highlight=4,12)]</span><span class="sxs-lookup"><span data-stu-id="f63a9-287">[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet2&highlight=4,12)]</span></span>

::: moniker-end

## <a name="tempdata"></a><span data-ttu-id="f63a9-288">TempData</span><span class="sxs-lookup"><span data-stu-id="f63a9-288">TempData</span></span>

<span data-ttu-id="f63a9-289">ASP.NET Core zpřístupní [TempData vlastnost modelu stránky stránky Razor](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata) nebo [TempData kontroler MVC](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata).</span><span class="sxs-lookup"><span data-stu-id="f63a9-289">ASP.NET Core exposes the [TempData property of a Razor Pages page model](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata) or [TempData of an MVC controller](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata).</span></span> <span data-ttu-id="f63a9-290">Tato vlastnost ukládá data, dokud je pro čtení.</span><span class="sxs-lookup"><span data-stu-id="f63a9-290">This property stores data until it's read.</span></span> <span data-ttu-id="f63a9-291">[Zachovat](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) a [prohlížet](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek) metody můžete použít k prozkoumání dat bez odstranění.</span><span class="sxs-lookup"><span data-stu-id="f63a9-291">The [Keep](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) and [Peek](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek) methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="f63a9-292">TempData je obzvláště užitečné pro přesměrování, pokud dat je potřeba pro více než jeden požadavek.</span><span class="sxs-lookup"><span data-stu-id="f63a9-292">TempData is particularly useful for redirection when data is required for more than a single request.</span></span> <span data-ttu-id="f63a9-293">TempData je implementováno modulem TempData zprostředkovatelů pomocí souborů cookie nebo stav relace.</span><span class="sxs-lookup"><span data-stu-id="f63a9-293">TempData is implemented by TempData providers using either cookies or session state.</span></span>

### <a name="tempdata-providers"></a><span data-ttu-id="f63a9-294">Zprostředkovatelé TempData</span><span class="sxs-lookup"><span data-stu-id="f63a9-294">TempData providers</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f63a9-295">V technologii ASP.NET Core 2.0 nebo novější TempData zprostředkovatele na základě souborů cookie se používá ve výchozím nastavení k uložení TempData v souborech cookie.</span><span class="sxs-lookup"><span data-stu-id="f63a9-295">In ASP.NET Core 2.0 or later, the cookie-based TempData provider is used by default to store TempData in cookies.</span></span>

<span data-ttu-id="f63a9-296">Cookie data se šifruje pomocí [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), kódovaného s [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), pak blokové.</span><span class="sxs-lookup"><span data-stu-id="f63a9-296">The cookie data is encrypted using [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), encoded with [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), then chunked.</span></span> <span data-ttu-id="f63a9-297">Vzhledem k tomu, že je soubor cookie blokové, velikost jednoho souboru cookie limit v ASP.NET Core 1.x netýká nalezen.</span><span class="sxs-lookup"><span data-stu-id="f63a9-297">Because the cookie is chunked, the single cookie size limit found in ASP.NET Core 1.x doesn't apply.</span></span> <span data-ttu-id="f63a9-298">Není komprimována cookie data, protože kompresi šifrovaná data může způsobit problémy se zabezpečením, jako [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) a [porušení](https://wikipedia.org/wiki/BREACH_(security_exploit)) útoky.</span><span class="sxs-lookup"><span data-stu-id="f63a9-298">The cookie data isn't compressed because compressing encrypted data can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="f63a9-299">Další informace o poskytovateli TempData na základě souborů cookie najdete v tématu [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider).</span><span class="sxs-lookup"><span data-stu-id="f63a9-299">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f63a9-300">TempData poskytovatele stavu relace ASP.NET Core 1.0 a 1.1, je výchozím zprostředkovatelem.</span><span class="sxs-lookup"><span data-stu-id="f63a9-300">In ASP.NET Core 1.0 and 1.1, the session state TempData provider is the default provider.</span></span>

::: moniker-end

### <a name="choose-a-tempdata-provider"></a><span data-ttu-id="f63a9-301">Vyberte poskytovatele TempData</span><span class="sxs-lookup"><span data-stu-id="f63a9-301">Choose a TempData provider</span></span>

<span data-ttu-id="f63a9-302">Výběr zprostředkovatele TempData zahrnuje několik důležité informace, například:</span><span class="sxs-lookup"><span data-stu-id="f63a9-302">Choosing a TempData provider involves several considerations, such as:</span></span>

1. <span data-ttu-id="f63a9-303">Aplikace už používá stav relace?</span><span class="sxs-lookup"><span data-stu-id="f63a9-303">Does the app already use session state?</span></span> <span data-ttu-id="f63a9-304">Pokud ano, použití TempData poskytovatele stavu relace má bez dalších nákladů na aplikaci (kromě zajištění dostatečného množství dat).</span><span class="sxs-lookup"><span data-stu-id="f63a9-304">If so, using the session state TempData provider has no additional cost to the app (aside from the size of the data).</span></span>
2. <span data-ttu-id="f63a9-305">Aplikace používá TempData pouze pouze pro relativně malé množství dat (až 500 bajtů)?</span><span class="sxs-lookup"><span data-stu-id="f63a9-305">Does the app use TempData only sparingly for relatively small amounts of data (up to 500 bytes)?</span></span> <span data-ttu-id="f63a9-306">Pokud ano, zprostředkovatele TempData souboru cookie přidá malé náklady na každý požadavek, který představuje TempData.</span><span class="sxs-lookup"><span data-stu-id="f63a9-306">If so, the cookie TempData provider adds a small cost to each request that carries TempData.</span></span> <span data-ttu-id="f63a9-307">Pokud ne, může být výhodné, aby se zabránilo odezvy velké množství dat v každé žádosti o, dokud TempData obsazením TempData poskytovatele stavu relace.</span><span class="sxs-lookup"><span data-stu-id="f63a9-307">If not, the session state TempData provider can be beneficial to avoid round-tripping a large amount of data in each request until the TempData is consumed.</span></span>
3. <span data-ttu-id="f63a9-308">Aplikace spouští v serverové farmě na několika serverech?</span><span class="sxs-lookup"><span data-stu-id="f63a9-308">Does the app run in a server farm on multiple servers?</span></span> <span data-ttu-id="f63a9-309">Pokud ano, neexistuje žádná další konfigurace vyžaduje použití zprostředkovatele TempData souboru cookie mimo ochrany dat (v tématu [ochrany dat](xref:security/data-protection/index) a [klíče poskytovatelů úložiště](xref:security/data-protection/implementation/key-storage-providers)).</span><span class="sxs-lookup"><span data-stu-id="f63a9-309">If so, there's no additional configuration required to use the cookie TempData provider outside of Data Protection (see [Data Protection](xref:security/data-protection/index) and [Key storage providers](xref:security/data-protection/implementation/key-storage-providers)).</span></span>

> [!NOTE]
> <span data-ttu-id="f63a9-310">Většina webovými klienty (například webové prohlížeče) vynutit omezení pro maximální velikost každého souboru cookie, celkový počet souborů cookie nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="f63a9-310">Most web clients (such as web browsers) enforce limits on the maximum size of each cookie, the total number of cookies, or both.</span></span> <span data-ttu-id="f63a9-311">Při použití zprostředkovatele TempData souboru cookie, ověřte, že aplikace nesmí být větší než tato omezení.</span><span class="sxs-lookup"><span data-stu-id="f63a9-311">When using the cookie TempData provider, verify the app won't exceed these limits.</span></span> <span data-ttu-id="f63a9-312">Vezměte v úvahu celková velikost data.</span><span class="sxs-lookup"><span data-stu-id="f63a9-312">Consider the total size of the data.</span></span> <span data-ttu-id="f63a9-313">Účet pro zvyšování velikost souboru cookie z důvodu šifrování a rozdělování.</span><span class="sxs-lookup"><span data-stu-id="f63a9-313">Account for increases in cookie size due to encryption and chunking.</span></span>

### <a name="configure-the-tempdata-provider"></a><span data-ttu-id="f63a9-314">Konfigurace zprostředkovatele TempData</span><span class="sxs-lookup"><span data-stu-id="f63a9-314">Configure the TempData provider</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f63a9-315">Ve výchozím nastavení je povolen TempData zprostředkovatel na základě souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="f63a9-315">The cookie-based TempData provider is enabled by default.</span></span>

<span data-ttu-id="f63a9-316">Pokud chcete povolit TempData zprostředkovatele na bázi relace, použijte [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider) metoda rozšíření:</span><span class="sxs-lookup"><span data-stu-id="f63a9-316">To enable the session-based TempData provider, use the [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider) extension method:</span></span>

<span data-ttu-id="f63a9-317">[!code-csharp[](app-state/samples_snapshot_2/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13,32)]</span><span class="sxs-lookup"><span data-stu-id="f63a9-317">[!code-csharp[](app-state/samples_snapshot_2/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13,32)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f63a9-318">Následující `Startup` kód třídy nakonfiguruje TempData zprostředkovatele na bázi relace:</span><span class="sxs-lookup"><span data-stu-id="f63a9-318">The following `Startup` class code configures the session-based TempData provider:</span></span>

<span data-ttu-id="f63a9-319">[!code-csharp[](app-state/samples_snapshot_2/1.x/SessionSample/Startup.cs?name=snippet1&highlight=4,9)]</span><span class="sxs-lookup"><span data-stu-id="f63a9-319">[!code-csharp[](app-state/samples_snapshot_2/1.x/SessionSample/Startup.cs?name=snippet1&highlight=4,9)]</span></span>

::: moniker-end

<span data-ttu-id="f63a9-320">Je důležité pořadí middleware.</span><span class="sxs-lookup"><span data-stu-id="f63a9-320">The order of middleware is important.</span></span> <span data-ttu-id="f63a9-321">V předchozím příkladu `InvalidOperationException` došlo k výjimce při `UseSession` je volána poté, co `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="f63a9-321">In the preceding example, an `InvalidOperationException` exception occurs when `UseSession` is invoked after `UseMvc`.</span></span> <span data-ttu-id="f63a9-322">Další informace najdete v tématu [řazení Middleware](xref:fundamentals/middleware/index#ordering).</span><span class="sxs-lookup"><span data-stu-id="f63a9-322">For more information, see [Middleware Ordering](xref:fundamentals/middleware/index#ordering).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f63a9-323">Pokud cílení na rozhraní .NET Framework a pomocí TempData zprostředkovatele na bázi relace, přidejte [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) balíčku do projektu.</span><span class="sxs-lookup"><span data-stu-id="f63a9-323">If targeting .NET Framework and using the session-based TempData provider, add the [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package to the project.</span></span>

## <a name="query-strings"></a><span data-ttu-id="f63a9-324">Řetězce dotazů</span><span class="sxs-lookup"><span data-stu-id="f63a9-324">Query strings</span></span>

<span data-ttu-id="f63a9-325">Omezené množství dat může být předán z jednoho požadavku na jiný přidáním do novou žádost o řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="f63a9-325">A limited amount of data can be passed from one request to another by adding it to the new request's query string.</span></span> <span data-ttu-id="f63a9-326">To je užitečné pro zaznamenání stavu trvalé způsobem, který umožňuje propojení s embedded stavu sdílení e-mailu nebo sociálních sítí.</span><span class="sxs-lookup"><span data-stu-id="f63a9-326">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="f63a9-327">Pro citlivá data, protože adresa URL řetězce dotazu veřejné, nikdy nepoužívejte řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="f63a9-327">Because URL query strings are public, never use query strings for sensitive data.</span></span>

<span data-ttu-id="f63a9-328">Kromě neúmyslnému sdílení, včetně dat v řetězcích dotazů můžete vytvořit příležitosti pro [webů požadavku padělání (proti útokům CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) útoků, které můžete obelstít návštěvou škodlivé weby při ověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="f63a9-328">In addition to unintended sharing, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="f63a9-329">Útočníci mohou pak ukrást uživatelská data z aplikace nebo trvat škodlivé akce jménem uživatele.</span><span class="sxs-lookup"><span data-stu-id="f63a9-329">Attackers can then steal user data from the app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="f63a9-330">Všechny zachovaných aplikace nebo stav relace musí zajistit ochranu proti útokům proti útokům CSRF.</span><span class="sxs-lookup"><span data-stu-id="f63a9-330">Any preserved app or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="f63a9-331">Další informace najdete v tématu [zabránit webů požadavku padělání (XSRF/proti útokům CSRF) před útoky](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="f63a9-331">For more information, see [Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks](xref:security/anti-request-forgery).</span></span>

## <a name="hidden-fields"></a><span data-ttu-id="f63a9-332">Skrytá pole</span><span class="sxs-lookup"><span data-stu-id="f63a9-332">Hidden fields</span></span>

<span data-ttu-id="f63a9-333">Data můžete uložit ve skrytého pole a odeslány zpět na další požadavek.</span><span class="sxs-lookup"><span data-stu-id="f63a9-333">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="f63a9-334">To je běžné ve formulářích s více stránkami.</span><span class="sxs-lookup"><span data-stu-id="f63a9-334">This is common in multi-page forms.</span></span> <span data-ttu-id="f63a9-335">Vzhledem k tomu, že klient může potenciálně manipulovat s daty, aplikace musí vždy znovu ověřit data uložená v skrytá pole.</span><span class="sxs-lookup"><span data-stu-id="f63a9-335">Because the client can potentially tamper with the data, the app must always revalidate the data stored in hidden fields.</span></span>

## <a name="httpcontextitems"></a><span data-ttu-id="f63a9-336">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="f63a9-336">HttpContext.Items</span></span>

<span data-ttu-id="f63a9-337">[HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) kolekce se používá k ukládání dat při zpracování jedné žádosti.</span><span class="sxs-lookup"><span data-stu-id="f63a9-337">The [HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) collection is used to store data while processing a single request.</span></span> <span data-ttu-id="f63a9-338">Obsah kolekce jsou zahozeny po zpracování požadavku.</span><span class="sxs-lookup"><span data-stu-id="f63a9-338">The collection's contents are discarded after a request is processed.</span></span> <span data-ttu-id="f63a9-339">`Items` Kolekci často používá k povolení součásti nebo middleware pro komunikaci, když budou fungovat v různých okamžicích v době žádost a mají přímý způsob, jak předat parametry.</span><span class="sxs-lookup"><span data-stu-id="f63a9-339">The `Items` collection is often used to allow components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span>

<span data-ttu-id="f63a9-340">V následujícím příkladu [middleware](xref:fundamentals/middleware/index) přidá `isVerified` k `Items` kolekce.</span><span class="sxs-lookup"><span data-stu-id="f63a9-340">In the following example, [middleware](xref:fundamentals/middleware/index) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="f63a9-341">Později v kanálu, další middleware přístup k hodnotě `isVerified`:</span><span class="sxs-lookup"><span data-stu-id="f63a9-341">Later in the pipeline, another middleware can access the value of `isVerified`:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync($"Verified: {context.Items["isVerified"]}");
});
```

<span data-ttu-id="f63a9-342">Pro middleware, který se používá pouze v jedné aplikaci `string` klíče jsou přijatelné.</span><span class="sxs-lookup"><span data-stu-id="f63a9-342">For middleware that's only used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="f63a9-343">Middleware sdílené mezi instancemi aplikace používejte jedinečný objekt předejdete klíče kolizí.</span><span class="sxs-lookup"><span data-stu-id="f63a9-343">Middleware shared between app instances should use unique object keys to avoid key collisions.</span></span> <span data-ttu-id="f63a9-344">Následující příklad ukazuje, jak chcete použít klíč jedinečný objekt definovaný ve třídě middleware:</span><span class="sxs-lookup"><span data-stu-id="f63a9-344">The following example shows how to use a unique object key defined in a middleware class:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f63a9-345">[!code-csharp[](app-state/samples/2.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=4,13)]</span><span class="sxs-lookup"><span data-stu-id="f63a9-345">[!code-csharp[](app-state/samples/2.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=4,13)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f63a9-346">[!code-csharp[](app-state/samples/1.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=5,14)]</span><span class="sxs-lookup"><span data-stu-id="f63a9-346">[!code-csharp[](app-state/samples/1.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=5,14)]</span></span>

::: moniker-end

<span data-ttu-id="f63a9-347">Jiný kód, můžete přístup s hodnotou uloženou v `HttpContext.Items` pomocí klíče vystavené middleware třídy:</span><span class="sxs-lookup"><span data-stu-id="f63a9-347">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f63a9-348">[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet3)]</span><span class="sxs-lookup"><span data-stu-id="f63a9-348">[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet3)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f63a9-349">[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet3)]</span><span class="sxs-lookup"><span data-stu-id="f63a9-349">[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet3)]</span></span>

::: moniker-end

<span data-ttu-id="f63a9-350">Tento přístup má také výhod odstraňuje použití klíče řetězců v kódu.</span><span class="sxs-lookup"><span data-stu-id="f63a9-350">This approach also has the advantage of eliminating the use of key strings in the code.</span></span>

## <a name="cache"></a><span data-ttu-id="f63a9-351">Mezipaměti</span><span class="sxs-lookup"><span data-stu-id="f63a9-351">Cache</span></span>

<span data-ttu-id="f63a9-352">Ukládání do mezipaměti je účinný způsob, jak ukládat a načítat data.</span><span class="sxs-lookup"><span data-stu-id="f63a9-352">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="f63a9-353">Aplikace můžete řídit životnost položek v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="f63a9-353">The app can control the lifetime of cached items.</span></span>

<span data-ttu-id="f63a9-354">Data uložená v mezipaměti není spojen s konkrétní žádost, uživatele nebo relace.</span><span class="sxs-lookup"><span data-stu-id="f63a9-354">Cached data isn't associated with a specific request, user, or session.</span></span> <span data-ttu-id="f63a9-355">**Dávejte pozor, aby mezipaměti uživatelská data, která může načíst žádostmi o jiných uživatelů.**</span><span class="sxs-lookup"><span data-stu-id="f63a9-355">**Be careful not to cache user-specific data that may be retrieved by other users' requests.**</span></span>

<span data-ttu-id="f63a9-356">Další informace najdete v tématu [odpovědí do mezipaměti](xref:performance/caching/index) tématu.</span><span class="sxs-lookup"><span data-stu-id="f63a9-356">For more information, see the [Cache responses](xref:performance/caching/index) topic.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="f63a9-357">Vkládání závislostí</span><span class="sxs-lookup"><span data-stu-id="f63a9-357">Dependency Injection</span></span>

<span data-ttu-id="f63a9-358">Použití [vkládání závislostí](xref:fundamentals/dependency-injection) jak zpřístupnit data pro všechny uživatele:</span><span class="sxs-lookup"><span data-stu-id="f63a9-358">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="f63a9-359">Definujte službu, která obsahuje data.</span><span class="sxs-lookup"><span data-stu-id="f63a9-359">Define a service containing the data.</span></span> <span data-ttu-id="f63a9-360">Například třídy s názvem `MyAppData` je definována:</span><span class="sxs-lookup"><span data-stu-id="f63a9-360">For example, a class named `MyAppData` is defined:</span></span>

    ```csharp
    public class MyAppData
    {
        // Declare properties and methods
    }
    ```

2. <span data-ttu-id="f63a9-361">Přidání třídy pro službu `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f63a9-361">Add the service class to `Startup.ConfigureServices`:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<MyAppData>();
    }
    ```

3. <span data-ttu-id="f63a9-362">Využívat třídě dat služby:</span><span class="sxs-lookup"><span data-stu-id="f63a9-362">Consume the data service class:</span></span>

    ::: moniker range=">= aspnetcore-2.0"

    ```csharp
    public class IndexModel : PageModel
    {
        public IndexModel(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

    ::: moniker range="< aspnetcore-2.0"

    ```csharp
    public class HomeController : Controller
    {
        public HomeController(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

## <a name="common-errors"></a><span data-ttu-id="f63a9-363">Běžné chyby</span><span class="sxs-lookup"><span data-stu-id="f63a9-363">Common errors</span></span>

* <span data-ttu-id="f63a9-364">"Nelze přeložit služby pro typ 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' při pokusu o aktivaci 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span><span class="sxs-lookup"><span data-stu-id="f63a9-364">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="f63a9-365">To je obvykle způsobeno nedaří nakonfigurovat alespoň jednu `IDistributedCache` implementace.</span><span class="sxs-lookup"><span data-stu-id="f63a9-365">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="f63a9-366">Další informace najdete v tématu [pracovat s distribuované mezipaměti](xref:performance/caching/distributed) a [mezipaměti v paměti](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="f63a9-366">For more information, see [Work with a distributed cache](xref:performance/caching/distributed) and [Cache in-memory](xref:performance/caching/memory).</span></span>

* <span data-ttu-id="f63a9-367">V události, která relace, které middleware nepodaří zachovat relace (například pokud záložní úložiště není k dispozici), middleware protokoluje výjimku a požadavek pokračuje normálním způsobem.</span><span class="sxs-lookup"><span data-stu-id="f63a9-367">In the event that the session middleware fails to persist a session (for example, if the backing store isn't available), the middleware logs the exception and the request continues normally.</span></span> <span data-ttu-id="f63a9-368">To vede k nepředvídatelné chování.</span><span class="sxs-lookup"><span data-stu-id="f63a9-368">This leads to unpredictable behavior.</span></span>

  <span data-ttu-id="f63a9-369">Například uživatel ukládá nákupní košík v relaci.</span><span class="sxs-lookup"><span data-stu-id="f63a9-369">For example, a user stores a shopping cart in session.</span></span> <span data-ttu-id="f63a9-370">Uživatel přidá položku do košíku ale potvrzení se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="f63a9-370">The user adds an item to the cart but the commit fails.</span></span> <span data-ttu-id="f63a9-371">Aplikace neví o selhání, proto ji sestavy pro uživatele, že položka byla přidána do jejich košíku, která není pravda.</span><span class="sxs-lookup"><span data-stu-id="f63a9-371">The app doesn't know about the failure so it reports to the user that the item was added to their cart, which isn't true.</span></span>

  <span data-ttu-id="f63a9-372">Je doporučeným přístupem ke kontrole chyb volání `await feature.Session.CommitAsync();` z kódu aplikace, pokud aplikace se provádí zápis do relace.</span><span class="sxs-lookup"><span data-stu-id="f63a9-372">The recommended approach to check for errors is to call `await feature.Session.CommitAsync();` from app code when the app is done writing to the session.</span></span> <span data-ttu-id="f63a9-373">`CommitAsync` vyvolá výjimku, pokud záložní úložiště není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="f63a9-373">`CommitAsync` throws an exception if the backing store is unavailable.</span></span> <span data-ttu-id="f63a9-374">Pokud `CommitAsync` nezdaří, aplikace může zpracovat výjimku.</span><span class="sxs-lookup"><span data-stu-id="f63a9-374">If `CommitAsync` fails, the app can process the exception.</span></span> <span data-ttu-id="f63a9-375">`LoadAsync` vyvolá za stejných podmínek, kde je úložiště dat není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="f63a9-375">`LoadAsync` throws under the same conditions where the data store is unavailable.</span></span>
