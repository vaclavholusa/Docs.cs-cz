---
title: Řešení potíží s projekty ASP.NET Core
author: Rick-Anderson
description: Pochopení a odstraňování potíží upozornění a chyby s projekty ASP.NET Core.
ms.author: riande
ms.date: 04/05/2018
uid: test/troubleshoot
ms.openlocfilehash: c72dd93f6ba705d7f03ade556c7a037dadeb6295
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938391"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="cec4f-103">Řešení potíží s projekty ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cec4f-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="cec4f-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cec4f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="cec4f-105">Následující odkazy obsahují pokyny k odstraňování problémů:</span><span class="sxs-lookup"><span data-stu-id="cec4f-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="cec4f-106">Řešení potíží s ASP.NET Core ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="cec4f-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="cec4f-107">Řešení potíží s ASP.NET Core ve službě IIS</span><span class="sxs-lookup"><span data-stu-id="cec4f-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="cec4f-108">Referenční informace o běžných chybách pro Azure App Service a IIS s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cec4f-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="cec4f-109">Konference Norwegian (Praha, 2018): Diagnostika problémů v aplikacích ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cec4f-109">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="cec4f-110">Blog o ASP.NET: Řešení potíží s ASP.NET Core problémy s výkonem</span><span class="sxs-lookup"><span data-stu-id="cec4f-110">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="cec4f-111">Upozornění na .NET core SDK</span><span class="sxs-lookup"><span data-stu-id="cec4f-111">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="cec4f-112">Jsou nainstalovány 32bitové a 64bitové verze sady .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="cec4f-112">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="cec4f-113">V **nový projekt** dialogové okno pro ASP.NET Core, může zobrazit následující upozornění:</span><span class="sxs-lookup"><span data-stu-id="cec4f-113">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="cec4f-114">32bitová i 64bitová verze sady .NET Core SDK jsou nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="cec4f-114">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="cec4f-115">Pouze šablony z 64bitové verze nainstalované na "C:\\Program Files\\dotnet\\sdk\\" se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="cec4f-115">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Snímek obrazovky dialogového okna OneASP.NET zprávou upozornění](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="cec4f-117">Toto upozornění se zobrazí, když (x86) 32bitové i 64bitovou (x 64) verze [.NET Core SDK](https://www.microsoft.com/net/download/all) jsou nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="cec4f-117">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="cec4f-118">Běžné důvody, které mohou být nainstalovány obě verze zahrnují:</span><span class="sxs-lookup"><span data-stu-id="cec4f-118">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="cec4f-119">Původně stáhnout instalační program sady SDK .NET Core s použitím 32bitový počítač, ale potom zkopírování napříč a nainstalována na 64bitovém počítači.</span><span class="sxs-lookup"><span data-stu-id="cec4f-119">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="cec4f-120">32bitová verze .NET Core SDK byla nainstalována jiná aplikace.</span><span class="sxs-lookup"><span data-stu-id="cec4f-120">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="cec4f-121">Chybná verze se stáhnul a nainstaloval.</span><span class="sxs-lookup"><span data-stu-id="cec4f-121">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="cec4f-122">Odinstalujte 32-bit .NET Core SDK, aby se zabránilo toto upozornění.</span><span class="sxs-lookup"><span data-stu-id="cec4f-122">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="cec4f-123">Odinstalovat z **ovládací panely** > **programy a funkce** > **odinstalovat nebo změnit program**.</span><span class="sxs-lookup"><span data-stu-id="cec4f-123">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="cec4f-124">Pokud budete rozumět tomu, proč dojde k upozornění a jeho dopady, můžete upozornění ignorovat.</span><span class="sxs-lookup"><span data-stu-id="cec4f-124">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="cec4f-125">.NET Core SDK je nainstalována v několika umístěních</span><span class="sxs-lookup"><span data-stu-id="cec4f-125">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="cec4f-126">V **nový projekt** dialogové okno pro ASP.NET Core, může zobrazit následující upozornění:</span><span class="sxs-lookup"><span data-stu-id="cec4f-126">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="cec4f-127">.NET Core SDK je nainstalována na více místech.</span><span class="sxs-lookup"><span data-stu-id="cec4f-127">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="cec4f-128">Pouze šablony ze sad SDK nainstalovaných v ' C:\\Program Files\\dotnet\\sdk\\"se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="cec4f-128">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Snímek obrazovky dialogového okna OneASP.NET zprávou upozornění](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="cec4f-130">Pokud máte alespoň jedna instalace sady .NET Core SDK do adresáře mimo se zobrazí tato zpráva *C:\\Program Files\\dotnet\\sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="cec4f-130">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="cec4f-131">K tomu obvykle dochází při .NET Core SDK je nasazený na počítači místo kopírovat/vložit instalační službu MSI.</span><span class="sxs-lookup"><span data-stu-id="cec4f-131">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="cec4f-132">Odinstalujte 32-bit .NET Core SDK, aby se zabránilo toto upozornění.</span><span class="sxs-lookup"><span data-stu-id="cec4f-132">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="cec4f-133">Odinstalovat z **ovládací panely** > **programy a funkce** > **odinstalovat nebo změnit program**.</span><span class="sxs-lookup"><span data-stu-id="cec4f-133">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="cec4f-134">Pokud budete rozumět tomu, proč dojde k upozornění a jeho dopady, můžete upozornění ignorovat.</span><span class="sxs-lookup"><span data-stu-id="cec4f-134">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="cec4f-135">Nezjistily se žádné sady SDK .NET Core</span><span class="sxs-lookup"><span data-stu-id="cec4f-135">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="cec4f-136">V **nový projekt** dialogové okno pro ASP.NET Core, může zobrazit následující upozornění:</span><span class="sxs-lookup"><span data-stu-id="cec4f-136">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="cec4f-137">Nezjistily se žádné sady .NET Core SDK, ujistěte se, že jsou součástí proměnné prostředí 'PATH'.</span><span class="sxs-lookup"><span data-stu-id="cec4f-137">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![Snímek obrazovky dialogového okna OneASP.NET zprávou upozornění](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="cec4f-139">Toto upozornění se zobrazí, když je proměnná prostředí `PATH` neodkazuje na žádné .NET Core SDK na počítači.</span><span class="sxs-lookup"><span data-stu-id="cec4f-139">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="cec4f-140">Chcete-li tento problém vyřešit:</span><span class="sxs-lookup"><span data-stu-id="cec4f-140">To resolve this problem:</span></span>

* <span data-ttu-id="cec4f-141">Nainstalujte nebo ověřte, že je nainstalovaná sada .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="cec4f-141">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="cec4f-142">Ověřte, že `PATH` proměnnou prostředí odkazuje na umístění, ve kterém je nainstalována sada SDK.</span><span class="sxs-lookup"><span data-stu-id="cec4f-142">Verify that the `PATH` environment variable points to the location in which the SDK is installed.</span></span> <span data-ttu-id="cec4f-143">Instalační program obvykle nastavuje `PATH`.</span><span class="sxs-lookup"><span data-stu-id="cec4f-143">The installer normally sets the `PATH`.</span></span>
