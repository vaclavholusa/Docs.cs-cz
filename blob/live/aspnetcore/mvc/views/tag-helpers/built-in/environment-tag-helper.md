---
title: "Pomocník značku prostředí ASP.NET Core"
author: pkellner
description: "Pomocník značku prostředí ASP.NET Core definované, včetně všech vlastností"
keywords: "ASP.NET Core, značka pomocné rutiny"
ms.author: riande
manager: wpickett
ms.date: 07/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 2639e4d7494e752462a1a2cb0648042a2d2d06ec
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="8964c-104">Pomocník značku prostředí ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8964c-104">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="8964c-105">Podle [Petr Kellner](http://peterkellner.net) a [Ateya Hisham Koš](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="8964c-105">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="8964c-106">Pomocník značku prostředí podmíněně vykreslí jeho závorkách obsah na základě aktuální hostování prostředí.</span><span class="sxs-lookup"><span data-stu-id="8964c-106">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="8964c-107">Jeho jeden atribut `names` je čárkami oddělený seznam prostředí názvy, které pokud žádné shodovat s aktuálním prostředí, aktivuje se v závorkách obsah k vykreslení.</span><span class="sxs-lookup"><span data-stu-id="8964c-107">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="8964c-108">Atributy pomocné rutiny značky prostředí</span><span class="sxs-lookup"><span data-stu-id="8964c-108">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="8964c-109">názvy</span><span class="sxs-lookup"><span data-stu-id="8964c-109">names</span></span>

<span data-ttu-id="8964c-110">Přijme jeden hostitelský název prostředí nebo seznam oddělený čárkami hostování prostředí názvy, které aktivují vykreslování závorkách obsahu.</span><span class="sxs-lookup"><span data-stu-id="8964c-110">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="8964c-111">Tyto hodnoty budou porovnány s aktuální hodnota vrácená z statické vlastnosti ASP.NET Core `HostingEnvironment.EnvironmentName`.</span><span class="sxs-lookup"><span data-stu-id="8964c-111">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="8964c-112">Tato hodnota je jeden z následujících: **pracovní**; **Vývoj** nebo **produkční**.</span><span class="sxs-lookup"><span data-stu-id="8964c-112">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="8964c-113">Porovnání se ignoruje velikost písmen.</span><span class="sxs-lookup"><span data-stu-id="8964c-113">The comparison ignores case.</span></span>

<span data-ttu-id="8964c-114">Příklad platné `environment` Pomocník značky:</span><span class="sxs-lookup"><span data-stu-id="8964c-114">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="8964c-115">Zahrnout a vyloučit atributy</span><span class="sxs-lookup"><span data-stu-id="8964c-115">include and exclude attributes</span></span>

<span data-ttu-id="8964c-116">ASP.NET Core 2.x přidá `include`  &  `exclude` atributy.</span><span class="sxs-lookup"><span data-stu-id="8964c-116">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="8964c-117">Tyto atributy určují vykreslování závorkách obsahu na základě zahrnuté ani vyloučené hostování prostředí názvů.</span><span class="sxs-lookup"><span data-stu-id="8964c-117">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="8964c-118">Zahrnout ASP.NET Core 2.0 nebo novější</span><span class="sxs-lookup"><span data-stu-id="8964c-118">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="8964c-119">`include` Vlastnost má podobné chování `names` atribut v ASP.NET Core 1.0.</span><span class="sxs-lookup"><span data-stu-id="8964c-119">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="8964c-120">vyloučit jádro ASP.NET 2.0 a vyšší</span><span class="sxs-lookup"><span data-stu-id="8964c-120">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="8964c-121">Naproti tomu `exclude` vlastnost umožňuje `EnvironmentTagHelper` vykreslení závorkách obsahu pro všechny názvy hostitelských prostředí s výjimkou vymazáním, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="8964c-121">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="8964c-122">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="8964c-122">Additional resources</span></span>

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
