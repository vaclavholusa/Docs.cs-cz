---
title: Řešení potíží s projekty ASP.NET Core
author: Rick-Anderson
description: Rady pro pochopení a řešení potíží s upozornění a chyby s projekty ASP.NET Core.
ms.author: riande
ms.date: 04/05/2018
uid: test/troubleshoot
ms.openlocfilehash: ae4e6f191d8f856de60ecf21cb882b5ee9b02064
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274590"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="100aa-103">Řešení potíží s projekty ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="100aa-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="100aa-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="100aa-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="100aa-105">Následující odkazy obsahují pokyny k odstraňování problémů:</span><span class="sxs-lookup"><span data-stu-id="100aa-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="100aa-106">Řešení potíží s ASP.NET Core ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="100aa-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="100aa-107">Řešení potíží s ASP.NET Core ve službě IIS</span><span class="sxs-lookup"><span data-stu-id="100aa-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="100aa-108">Běžné chyby referenční dokumentace pro Azure App Service a IIS s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="100aa-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="100aa-109">NDC konference (Londýn, 2018): Diagnostika problémů s v aplikacích ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="100aa-109">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="100aa-110">ASP.NET Blog: Řešení potíží s problémy s výkonem ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="100aa-110">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="100aa-111">Upozornění na .NET core SDK</span><span class="sxs-lookup"><span data-stu-id="100aa-111">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="100aa-112">Instalaci 32bitové a 64bitové verze rozhraní .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="100aa-112">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="100aa-113">V **nový projekt** dialogové okno pro ASP.NET Core, může se zobrazit následující varování:</span><span class="sxs-lookup"><span data-stu-id="100aa-113">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="100aa-114">32 a 64 bitových verzích .NET Core SDK jsou nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="100aa-114">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="100aa-115">Pouze šablony z verze 64bitová verze nainstalované v ' C:\\Program Files\\dotnet\\sdk\\, se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="100aa-115">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Snímek obrazovky dialogového okna OneASP.NET zobrazující upozornění](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="100aa-117">Toto upozornění se zobrazí, když (x86) 32bitovou i 64bitovou (x 64) verze [.NET Core SDK](https://www.microsoft.com/net/download/all) jsou nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="100aa-117">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="100aa-118">Obě verze může být nainstalována obvyklé příčiny patří:</span><span class="sxs-lookup"><span data-stu-id="100aa-118">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="100aa-119">Jste původně stáhnout instalační program rozhraní .NET Core SDK s použitím 32bitový počítač, ale pak zkopírovali ho napříč a nainstalován na 64bitový počítač.</span><span class="sxs-lookup"><span data-stu-id="100aa-119">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="100aa-120">32bitová verze rozhraní .NET Core SDK byla nainstalována jiná aplikace.</span><span class="sxs-lookup"><span data-stu-id="100aa-120">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="100aa-121">Nesprávné verze byla stáhli a nainstalovali.</span><span class="sxs-lookup"><span data-stu-id="100aa-121">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="100aa-122">Odinstalujte 32bitová verze rozhraní .NET Core SDK aby toto upozornění.</span><span class="sxs-lookup"><span data-stu-id="100aa-122">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="100aa-123">Odinstalovat z **ovládací panely** > **programy a funkce** > **odinstalovat nebo změnit program**.</span><span class="sxs-lookup"><span data-stu-id="100aa-123">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="100aa-124">Pokud budete rozumět tomu, proč dochází upozornění a jeho dopad, můžete upozornění ignorovat.</span><span class="sxs-lookup"><span data-stu-id="100aa-124">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="100aa-125">.NET Core SDK je nainstalován na více místech</span><span class="sxs-lookup"><span data-stu-id="100aa-125">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="100aa-126">V **nový projekt** dialogové okno pro ASP.NET Core, může se zobrazit následující varování:</span><span class="sxs-lookup"><span data-stu-id="100aa-126">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="100aa-127">.NET Core SDK je nainstalována na více místech.</span><span class="sxs-lookup"><span data-stu-id="100aa-127">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="100aa-128">Pouze šablony z SDK(s) nainstalovaným v ' C:\\Program Files\\dotnet\\sdk\\, se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="100aa-128">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Snímek obrazovky dialogového okna OneASP.NET zobrazující upozornění](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="100aa-130">Se zobrazí tato zpráva, když máte alespoň jedna instalace rozhraní .NET Core SDK v adresáři mimo *C:\\Program Files\\dotnet\\sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="100aa-130">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="100aa-131">Obvykle se to stane, když .NET Core SDK nasazený na počítači použití, kopírování a vkládání namísto Instalační služby MSI.</span><span class="sxs-lookup"><span data-stu-id="100aa-131">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="100aa-132">Odinstalujte 32bitová verze rozhraní .NET Core SDK aby toto upozornění.</span><span class="sxs-lookup"><span data-stu-id="100aa-132">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="100aa-133">Odinstalovat z **ovládací panely** > **programy a funkce** > **odinstalovat nebo změnit program**.</span><span class="sxs-lookup"><span data-stu-id="100aa-133">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="100aa-134">Pokud budete rozumět tomu, proč dochází upozornění a jeho dopad, můžete upozornění ignorovat.</span><span class="sxs-lookup"><span data-stu-id="100aa-134">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="100aa-135">Nebyly zjištěny žádné .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="100aa-135">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="100aa-136">V **nový projekt** dialogové okno pro ASP.NET Core, může se zobrazit následující varování:</span><span class="sxs-lookup"><span data-stu-id="100aa-136">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="100aa-137">Nebyly zjištěny žádné .NET Core SDK, ujistěte se, že jsou zahrnuty v proměnné prostředí 'PATH'.</span><span class="sxs-lookup"><span data-stu-id="100aa-137">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![Snímek obrazovky dialogového okna OneASP.NET zobrazující upozornění](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="100aa-139">Toto upozornění se zobrazí, když proměnnou prostředí `PATH` neodkazuje na žádné rozhraní .NET Core SDK na počítači.</span><span class="sxs-lookup"><span data-stu-id="100aa-139">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="100aa-140">Chcete-li tento problém vyřešit:</span><span class="sxs-lookup"><span data-stu-id="100aa-140">To resolve this problem:</span></span>

* <span data-ttu-id="100aa-141">Nainstalujte nebo ověřte, že je nainstalováno rozhraní .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="100aa-141">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="100aa-142">Ověřte `PATH` proměnnou prostředí odkazuje na umístění, je nainstalována sada SDK.</span><span class="sxs-lookup"><span data-stu-id="100aa-142">Verify the `PATH` environment variable points to the location the SDK is installed.</span></span> <span data-ttu-id="100aa-143">Instalační program za normálních okolností nastaví `PATH`.</span><span class="sxs-lookup"><span data-stu-id="100aa-143">The installer normally sets the `PATH`.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="use-of-ihtmlhelperpartial-may-result-in-app-deadlocks"></a><span data-ttu-id="100aa-144">Použití IHtmlHelper.Partial může vést k zablokování aplikace</span><span class="sxs-lookup"><span data-stu-id="100aa-144">Use of IHtmlHelper.Partial may result in app deadlocks</span></span>

<span data-ttu-id="100aa-145">V technologii ASP.NET Core 2.1 nebo novější, volání `Html.Partial` vede upozornění analyzátor kvůli možným zablokování.</span><span class="sxs-lookup"><span data-stu-id="100aa-145">In ASP.NET Core 2.1 and later, calling `Html.Partial` results in an analyzer warning due to the potential for deadlocks.</span></span> <span data-ttu-id="100aa-146">Upozornění je:</span><span class="sxs-lookup"><span data-stu-id="100aa-146">The warning message is:</span></span>

> <span data-ttu-id="100aa-147">Použití IHtmlHelper.Partial může vést k zablokování aplikace.</span><span class="sxs-lookup"><span data-stu-id="100aa-147">Use of IHtmlHelper.Partial may result in application deadlocks.</span></span> <span data-ttu-id="100aa-148">Zvažte použití `<partial>` pomocná značka nebo `IHtmlHelper.PartialAsync`.</span><span class="sxs-lookup"><span data-stu-id="100aa-148">Consider using `<partial>` Tag Helper or `IHtmlHelper.PartialAsync`.</span></span>

<span data-ttu-id="100aa-149">Volání `@Html.Partial` by měl být nahrazen `@await Html.PartialAsync` nebo částečné značky pomocná `<partial name="_Partial" />`.</span><span class="sxs-lookup"><span data-stu-id="100aa-149">Calls to `@Html.Partial` should be replaced by `@await Html.PartialAsync` or the partial tag helper `<partial name="_Partial" />`.</span></span>

::: moniker-end
