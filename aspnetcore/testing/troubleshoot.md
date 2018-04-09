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
ms.openlocfilehash: 98077081409949db14b19c7934bc162990ffc302
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="5cf1a-103">Řešení potíží s projekty ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5cf1a-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="5cf1a-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5cf1a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5cf1a-105">Následující odkazy obsahují pokyny k odstraňování problémů:</span><span class="sxs-lookup"><span data-stu-id="5cf1a-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="5cf1a-106">Řešení potíží s ASP.NET Core ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5cf1a-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="5cf1a-107">Řešení potíží s ASP.NET Core ve službě IIS</span><span class="sxs-lookup"><span data-stu-id="5cf1a-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="5cf1a-108">Běžné chyby referenční dokumentace pro Azure App Service a IIS s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5cf1a-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)

<a name="sdk"></a>
## <a name="net-core-sdk-warnings"></a><span data-ttu-id="5cf1a-109">Upozornění na .NET core SDK</span><span class="sxs-lookup"><span data-stu-id="5cf1a-109">.NET Core SDK warnings</span></span>

### <a name="both-the-32-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="5cf1a-110">Obě 32 až 64 bit verze rozhraní .NET Core SDK jsou nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="5cf1a-110">Both the 32 and 64 bit versions of the .NET Core SDK are installed</span></span>
<span data-ttu-id="5cf1a-111">V **nový projekt** dialogové okno pro ASP.NET Core, může se zobrazit následující varování zobrazí v horní části:</span><span class="sxs-lookup"><span data-stu-id="5cf1a-111">In the **New Project** dialog for ASP.NET Core, you may see the following warning appear at the top:</span></span> 

    Both 32 and 64 bit versions of the .NET Core SDK are installed. Only templates from the 64 bit version(s) installed at C:\Program Files\dotnet\sdk\" will be displayed.

![Snímek obrazovky dialogového okna OneASP.NET zobrazující upozornění](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="5cf1a-113">Toto upozornění se zobrazí, když (x86) 32bitovou i 64bitovou (x 64) verze [.NET Core SDK](https://www.microsoft.com/net/download/all) jsou nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="5cf1a-113">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="5cf1a-114">Běžné obě verze lze nainstalovat z těchto důvodů:</span><span class="sxs-lookup"><span data-stu-id="5cf1a-114">Common reasons both versions can be installed include:</span></span>

* <span data-ttu-id="5cf1a-115">Jste původně stáhnout instalační program rozhraní .NET Core SDK s použitím 32bitový počítač, ale pak zkopírovali ho napříč a nainstalován na 64bitový počítač.</span><span class="sxs-lookup"><span data-stu-id="5cf1a-115">You originally downloaded the .NET Core SDK installer using a 32-bit machine, but then copied it across and installed it on a 64-bit machine.</span></span> 
* <span data-ttu-id="5cf1a-116">32bitová verze rozhraní .NET Core SDK byla nainstalována jiná aplikace.</span><span class="sxs-lookup"><span data-stu-id="5cf1a-116">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="5cf1a-117">Nesprávné verze byla stáhli a nainstalovali.</span><span class="sxs-lookup"><span data-stu-id="5cf1a-117">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="5cf1a-118">Odinstalujte 32bitová verze rozhraní .NET Core SDK aby toto upozornění.</span><span class="sxs-lookup"><span data-stu-id="5cf1a-118">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="5cf1a-119">Odinstalovat z **ovládací panely** > **programy a funkce** > **odinstalovat nebo změnit program**.</span><span class="sxs-lookup"><span data-stu-id="5cf1a-119">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="5cf1a-120">Pokud budete rozumět tomu, proč dochází upozornění a jeho dopad, můžete upozornění ignorovat.</span><span class="sxs-lookup"><span data-stu-id="5cf1a-120">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="5cf1a-121">.NET Core SDK je nainstalován na více místech</span><span class="sxs-lookup"><span data-stu-id="5cf1a-121">The .NET Core SDK is installed in multiple locations</span></span>
<span data-ttu-id="5cf1a-122">V **nový projekt** dialogové okno pro ASP.NET Core, mohou se zobrazit následující upozornění se zobrazí v horní části:</span><span class="sxs-lookup"><span data-stu-id="5cf1a-122">In the **New Project** dialog for ASP.NET Core you may see the following warning appear at the top:</span></span> 

 <span data-ttu-id="5cf1a-123">.NET Core SDK je nainstalována na více místech.</span><span class="sxs-lookup"><span data-stu-id="5cf1a-123">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="5cf1a-124">Pouze šablony z SDK(s) nainstalovaným v "C:\Program Files\dotnet\sdk\' se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="5cf1a-124">Only templates from the SDK(s) installed at 'C:\Program Files\dotnet\sdk\' will be displayed.</span></span>

![Snímek obrazovky dialogového okna OneASP.NET zobrazující upozornění](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="5cf1a-126">Tato zpráva se zobrazuje, protože máte alespoň jedna instalace rozhraní .NET Core SDK v adresáři mimo * C:\Program Files\dotnet\sdk\*.</span><span class="sxs-lookup"><span data-stu-id="5cf1a-126">You are seeing this message because you have at least one installation of the .NET Core SDK in a directory outside of *C:\Program Files\dotnet\sdk\*.</span></span> <span data-ttu-id="5cf1a-127">Obvykle k tomu dojde, když .NET Core SDK nasazený na počítači použití, kopírování a vkládání namísto Instalační služby MSI.</span><span class="sxs-lookup"><span data-stu-id="5cf1a-127">Usually that happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="5cf1a-128">Odinstalujte 32bitová verze rozhraní .NET Core SDK aby toto upozornění.</span><span class="sxs-lookup"><span data-stu-id="5cf1a-128">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="5cf1a-129">Odinstalovat z **ovládací panely** > **programy a funkce** > **odinstalovat nebo změnit program**.</span><span class="sxs-lookup"><span data-stu-id="5cf1a-129">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="5cf1a-130">Pokud budete rozumět tomu, proč dochází upozornění a jeho dopad, můžete upozornění ignorovat.</span><span class="sxs-lookup"><span data-stu-id="5cf1a-130">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>
