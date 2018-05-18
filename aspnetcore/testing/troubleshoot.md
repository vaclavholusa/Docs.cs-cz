---
title: Řešení potíží pro ASP.NET Core
author: Rick-Anderson
description: Rady pro pochopení a řešení potíží s upozornění a chyby s projekty ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 04/05/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: testing/troubleshoot
ms.openlocfilehash: 3bba085c69ee96b5725331b14dcf15350d66e4a4
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/17/2018
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="8ac83-103">Řešení potíží s projekty ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8ac83-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="8ac83-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8ac83-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8ac83-105">Následující odkazy obsahují pokyny k odstraňování problémů:</span><span class="sxs-lookup"><span data-stu-id="8ac83-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="8ac83-106">Řešení potíží s ASP.NET Core ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="8ac83-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="8ac83-107">Řešení potíží s ASP.NET Core ve službě IIS</span><span class="sxs-lookup"><span data-stu-id="8ac83-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="8ac83-108">Běžné chyby referenční dokumentace pro Azure App Service a IIS s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8ac83-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="8ac83-109">YouTube: Diagnostika problémů s v aplikacích ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8ac83-109">YouTube: Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)

<a name="sdk"></a>
## <a name="net-core-sdk-warnings"></a><span data-ttu-id="8ac83-110">Upozornění na .NET core SDK</span><span class="sxs-lookup"><span data-stu-id="8ac83-110">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="8ac83-111">Instalaci 32bitové a 64bitové verze rozhraní .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="8ac83-111">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>
<span data-ttu-id="8ac83-112">V **nový projekt** dialogové okno pro ASP.NET Core, může se zobrazit následující varování:</span><span class="sxs-lookup"><span data-stu-id="8ac83-112">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span> 

    Both 32 and 64 bit versions of the .NET Core SDK are installed. Only templates from the 64 bit version(s) installed at C:\Program Files\dotnet\sdk\" will be displayed.

![Snímek obrazovky dialogového okna OneASP.NET zobrazující upozornění](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="8ac83-114">Toto upozornění se zobrazí, když (x86) 32bitovou i 64bitovou (x 64) verze [.NET Core SDK](https://www.microsoft.com/net/download/all) jsou nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="8ac83-114">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="8ac83-115">Běžné obě verze lze nainstalovat z těchto důvodů:</span><span class="sxs-lookup"><span data-stu-id="8ac83-115">Common reasons both versions can be installed include:</span></span>

* <span data-ttu-id="8ac83-116">Jste původně stáhnout instalační program rozhraní .NET Core SDK s použitím 32bitový počítač, ale pak zkopírovali ho napříč a nainstalován na 64bitový počítač.</span><span class="sxs-lookup"><span data-stu-id="8ac83-116">You originally downloaded the .NET Core SDK installer using a 32-bit machine, but then copied it across and installed it on a 64-bit machine.</span></span> 
* <span data-ttu-id="8ac83-117">32bitová verze rozhraní .NET Core SDK byla nainstalována jiná aplikace.</span><span class="sxs-lookup"><span data-stu-id="8ac83-117">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="8ac83-118">Nesprávné verze byla stáhli a nainstalovali.</span><span class="sxs-lookup"><span data-stu-id="8ac83-118">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="8ac83-119">Odinstalujte 32bitová verze rozhraní .NET Core SDK aby toto upozornění.</span><span class="sxs-lookup"><span data-stu-id="8ac83-119">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="8ac83-120">Odinstalovat z **ovládací panely** > **programy a funkce** > **odinstalovat nebo změnit program**.</span><span class="sxs-lookup"><span data-stu-id="8ac83-120">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="8ac83-121">Pokud budete rozumět tomu, proč dochází upozornění a jeho dopad, můžete upozornění ignorovat.</span><span class="sxs-lookup"><span data-stu-id="8ac83-121">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="8ac83-122">.NET Core SDK je nainstalován na více místech</span><span class="sxs-lookup"><span data-stu-id="8ac83-122">The .NET Core SDK is installed in multiple locations</span></span>
<span data-ttu-id="8ac83-123">V **nový projekt** dialogové okno pro ASP.NET Core může se zobrazit následující varování:</span><span class="sxs-lookup"><span data-stu-id="8ac83-123">In the **New Project** dialog for ASP.NET Core you may see the following warning:</span></span> 

 <span data-ttu-id="8ac83-124">.NET Core SDK je nainstalována na více místech.</span><span class="sxs-lookup"><span data-stu-id="8ac83-124">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="8ac83-125">Pouze šablony z SDK(s) nainstalovaným v "C:\Program Files\dotnet\sdk\' se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="8ac83-125">Only templates from the SDK(s) installed at 'C:\Program Files\dotnet\sdk\' will be displayed.</span></span>

![Snímek obrazovky dialogového okna OneASP.NET zobrazující upozornění](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="8ac83-127">Se zobrazí tato zpráva, když máte alespoň jedna instalace rozhraní .NET Core SDK v adresáři mimo * C:\Program Files\dotnet\sdk\*.</span><span class="sxs-lookup"><span data-stu-id="8ac83-127">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\Program Files\dotnet\sdk\*.</span></span> <span data-ttu-id="8ac83-128">Obvykle k tomu dojde, když .NET Core SDK nasazený na počítači použití, kopírování a vkládání namísto Instalační služby MSI.</span><span class="sxs-lookup"><span data-stu-id="8ac83-128">Usually that happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="8ac83-129">Odinstalujte 32bitová verze rozhraní .NET Core SDK aby toto upozornění.</span><span class="sxs-lookup"><span data-stu-id="8ac83-129">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="8ac83-130">Odinstalovat z **ovládací panely** > **programy a funkce** > **odinstalovat nebo změnit program**.</span><span class="sxs-lookup"><span data-stu-id="8ac83-130">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="8ac83-131">Pokud budete rozumět tomu, proč dochází upozornění a jeho dopad, můžete upozornění ignorovat.</span><span class="sxs-lookup"><span data-stu-id="8ac83-131">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="8ac83-132">Nebyly zjištěny žádné .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="8ac83-132">No .NET Core SDKs were detected</span></span>
<span data-ttu-id="8ac83-133">V **nový projekt** dialogové okno pro ASP.NET Core může se zobrazit následující varování:</span><span class="sxs-lookup"><span data-stu-id="8ac83-133">In the **New Project** dialog for ASP.NET Core you may see the following warning:</span></span> 

<span data-ttu-id="8ac83-134">**Nebyly zjištěny žádné .NET Core SDK, ujistěte se, že jsou zahrnuty v proměnné prostředí 'PATH'**</span><span class="sxs-lookup"><span data-stu-id="8ac83-134">**No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'**</span></span>

![Snímek obrazovky dialogového okna OneASP.NET zobrazující upozornění](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="8ac83-136">Toto upozornění se zobrazí, když proměnnou prostředí `PATH` neodkazuje na žádné rozhraní .NET Core SDK na počítači.</span><span class="sxs-lookup"><span data-stu-id="8ac83-136">This warning appears when the environment variable `PATH` doesn’t point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="8ac83-137">Chcete-li tento problém vyřešit:</span><span class="sxs-lookup"><span data-stu-id="8ac83-137">To resolve this problem:</span></span>

* <span data-ttu-id="8ac83-138">Nainstalujte nebo ověřte, že je nainstalováno rozhraní .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="8ac83-138">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="8ac83-139">Ověřte `PATH` proměnnou prostředí odkazuje na umístění, je nainstalována sada SDK.</span><span class="sxs-lookup"><span data-stu-id="8ac83-139">Verify the `PATH` environment variable points to the location the SDK is installed.</span></span> <span data-ttu-id="8ac83-140">Instalační program za normálních okolností nastaví `PATH`.</span><span class="sxs-lookup"><span data-stu-id="8ac83-140">The installer normally sets the `PATH`.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="use-of-ihtmlhelperpartial-may-result-in-application-deadlocks"></a><span data-ttu-id="8ac83-141">Použití IHtmlHelper.Partial může vést k zablokování aplikace</span><span class="sxs-lookup"><span data-stu-id="8ac83-141">Use of IHtmlHelper.Partial may result in application deadlocks</span></span>

<span data-ttu-id="8ac83-142">V technologii ASP.NET Core 2.1 nebo novější, volání `Html.Partial` vede upozornění analyzátor kvůli možným zablokování.</span><span class="sxs-lookup"><span data-stu-id="8ac83-142">In ASP.NET Core 2.1 and later, calling `Html.Partial` results in an analyzer warning due to the potential for deadlocks.</span></span> <span data-ttu-id="8ac83-143">Upozornění je:</span><span class="sxs-lookup"><span data-stu-id="8ac83-143">The warning message is:</span></span>

<span data-ttu-id="8ac83-144">*Použití IHtmlHelper.Partial může vést k zablokování aplikace. Zvažte použití `<partial>` pomocná značka nebo `IHtmlHelper.PartialAsync`.*</span><span class="sxs-lookup"><span data-stu-id="8ac83-144">*Use of IHtmlHelper.Partial may result in application deadlocks. Consider using `<partial>` Tag Helper or `IHtmlHelper.PartialAsync`.*</span></span>

<span data-ttu-id="8ac83-145">Volání `@Html.Partial` by měl být nahrazen `@await Html.PartialAsync` nebo částečné značky pomocná `<partial name="_Partial" />`.</span><span class="sxs-lookup"><span data-stu-id="8ac83-145">Calls to `@Html.Partial` should be replaced by `@await Html.PartialAsync` or the partial tag helper `<partial name="_Partial" />`.</span></span>

::: moniker-end
