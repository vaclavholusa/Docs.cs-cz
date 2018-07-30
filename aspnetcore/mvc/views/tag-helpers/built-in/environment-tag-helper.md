---
title: Pomocná rutina značky prostředí v ASP.NET Core
author: pkellner
description: Pomocná rutina značky prostředí ASP.NET Core definované, včetně všech vlastností
ms.author: riande
ms.date: 07/14/2017
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 4a283a3a03aa6cac228ec6effd02e3f1095be260
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342221"
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="b01e8-103">Pomocná rutina značky prostředí v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b01e8-103">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="b01e8-104">Podle [Peter Kellner](http://peterkellner.net) a [Ateya Hisham Bin](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="b01e8-104">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="b01e8-105">Pomocná rutina značky prostředí podmíněně vykreslí obsah uzavřené podle aktuální hostitelského prostředí.</span><span class="sxs-lookup"><span data-stu-id="b01e8-105">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="b01e8-106">Jeho jediný atribut `names` je čárkami oddělený seznam prostředí názvů, který pokud všechny odpovídaly aktuálním prostředí, aktivují uzavřené obsah k vykreslení.</span><span class="sxs-lookup"><span data-stu-id="b01e8-106">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="b01e8-107">Atributy pomocné rutiny značky prostředí</span><span class="sxs-lookup"><span data-stu-id="b01e8-107">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="b01e8-108">názvy</span><span class="sxs-lookup"><span data-stu-id="b01e8-108">names</span></span>

<span data-ttu-id="b01e8-109">Přijímá jediný název hostitelské prostředí nebo čárkami oddělený seznam hostování názvy prostředí, které aktivují vykreslování uzavřené obsah.</span><span class="sxs-lookup"><span data-stu-id="b01e8-109">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="b01e8-110">Tyto hodnoty jsou ve srovnání s aktuální hodnota vrácená z ASP.NET Core statickou vlastnost `HostingEnvironment.EnvironmentName`.</span><span class="sxs-lookup"><span data-stu-id="b01e8-110">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="b01e8-111">Tato hodnota je jeden z následujících: **pracovní**; **Vývoj** nebo **produkční**.</span><span class="sxs-lookup"><span data-stu-id="b01e8-111">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="b01e8-112">Porovnání ignoruje velikost písmen.</span><span class="sxs-lookup"><span data-stu-id="b01e8-112">The comparison ignores case.</span></span>

<span data-ttu-id="b01e8-113">Příkladem platné `environment` je pomocná rutina značky:</span><span class="sxs-lookup"><span data-stu-id="b01e8-113">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="b01e8-114">zahrnutí a vyloučení atributy</span><span class="sxs-lookup"><span data-stu-id="b01e8-114">include and exclude attributes</span></span>

<span data-ttu-id="b01e8-115">ASP.NET Core 2.x přidá `include`  &  `exclude` atributy.</span><span class="sxs-lookup"><span data-stu-id="b01e8-115">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="b01e8-116">Tyto atributy určují, vykreslování uzavřené obsahu na základě zahrnuté ani vyloučené hostování prostředí názvů.</span><span class="sxs-lookup"><span data-stu-id="b01e8-116">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="b01e8-117">Zahrnout ASP.NET Core 2.0 a vyšší</span><span class="sxs-lookup"><span data-stu-id="b01e8-117">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="b01e8-118">`include` Vlastnost má podobné chování `names` atribut v ASP.NET Core 1.0.</span><span class="sxs-lookup"><span data-stu-id="b01e8-118">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="b01e8-119">vyloučit ASP.NET Core 2.0 a vyšší</span><span class="sxs-lookup"><span data-stu-id="b01e8-119">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="b01e8-120">Naproti tomu `exclude` vlastnost `EnvironmentTagHelper` vykreslení uzavřené obsahu pro všechny názvy hostitelských prostředí s výjimkou vymazáním, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="b01e8-120">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="b01e8-121">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b01e8-121">Additional resources</span></span>

* <xref:fundamentals/environments>
