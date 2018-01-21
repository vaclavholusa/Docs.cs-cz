---
title: "Pomocník značku prostředí ASP.NET Core"
author: pkellner
description: "Pomocník značku prostředí ASP.NET Core definované, včetně všech vlastností"
ms.author: riande
manager: wpickett
ms.date: 07/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 32646f1fdaf840f796da1ec573459157a41a86d1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="de8fe-103">Pomocník značku prostředí ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="de8fe-103">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="de8fe-104">Podle [Petr Kellner](http://peterkellner.net) a [Ateya Hisham Koš](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="de8fe-104">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="de8fe-105">Pomocník značku prostředí podmíněně vykreslí jeho závorkách obsah na základě aktuální hostování prostředí.</span><span class="sxs-lookup"><span data-stu-id="de8fe-105">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="de8fe-106">Jeho jeden atribut `names` je čárkami oddělený seznam prostředí názvy, které pokud žádné shodovat s aktuálním prostředí, aktivuje se v závorkách obsah k vykreslení.</span><span class="sxs-lookup"><span data-stu-id="de8fe-106">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="de8fe-107">Atributy pomocné rutiny značky prostředí</span><span class="sxs-lookup"><span data-stu-id="de8fe-107">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="de8fe-108">názvy</span><span class="sxs-lookup"><span data-stu-id="de8fe-108">names</span></span>

<span data-ttu-id="de8fe-109">Přijme jeden hostitelský název prostředí nebo seznam oddělený čárkami hostování prostředí názvy, které aktivují vykreslování závorkách obsahu.</span><span class="sxs-lookup"><span data-stu-id="de8fe-109">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="de8fe-110">Tyto hodnoty budou porovnány s aktuální hodnota vrácená z statické vlastnosti ASP.NET Core `HostingEnvironment.EnvironmentName`.</span><span class="sxs-lookup"><span data-stu-id="de8fe-110">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="de8fe-111">Tato hodnota je jeden z následujících: **pracovní**; **Vývoj** nebo **produkční**.</span><span class="sxs-lookup"><span data-stu-id="de8fe-111">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="de8fe-112">Porovnání se ignoruje velikost písmen.</span><span class="sxs-lookup"><span data-stu-id="de8fe-112">The comparison ignores case.</span></span>

<span data-ttu-id="de8fe-113">Příklad platné `environment` Pomocník značky:</span><span class="sxs-lookup"><span data-stu-id="de8fe-113">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="de8fe-114">Zahrnout a vyloučit atributy</span><span class="sxs-lookup"><span data-stu-id="de8fe-114">include and exclude attributes</span></span>

<span data-ttu-id="de8fe-115">ASP.NET Core 2.x přidá `include`  &  `exclude` atributy.</span><span class="sxs-lookup"><span data-stu-id="de8fe-115">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="de8fe-116">Tyto atributy určují vykreslování závorkách obsahu na základě zahrnuté ani vyloučené hostování prostředí názvů.</span><span class="sxs-lookup"><span data-stu-id="de8fe-116">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="de8fe-117">Zahrnout ASP.NET Core 2.0 nebo novější</span><span class="sxs-lookup"><span data-stu-id="de8fe-117">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="de8fe-118">`include` Vlastnost má podobné chování `names` atribut v ASP.NET Core 1.0.</span><span class="sxs-lookup"><span data-stu-id="de8fe-118">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="de8fe-119">vyloučit jádro ASP.NET 2.0 a vyšší</span><span class="sxs-lookup"><span data-stu-id="de8fe-119">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="de8fe-120">Naproti tomu `exclude` vlastnost umožňuje `EnvironmentTagHelper` vykreslení závorkách obsahu pro všechny názvy hostitelských prostředí s výjimkou vymazáním, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="de8fe-120">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="de8fe-121">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="de8fe-121">Additional resources</span></span>

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
