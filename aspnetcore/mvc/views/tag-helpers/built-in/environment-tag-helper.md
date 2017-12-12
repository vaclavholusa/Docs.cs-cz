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
# <a name="environment-tag-helper-in-aspnet-core"></a>Pomocník značku prostředí ASP.NET Core

Podle [Petr Kellner](http://peterkellner.net) a [Ateya Hisham Koš](https://twitter.com/hishambinateya)

Pomocník značku prostředí podmíněně vykreslí jeho závorkách obsah na základě aktuální hostování prostředí. Jeho jeden atribut `names` je čárkami oddělený seznam prostředí názvy, které pokud žádné shodovat s aktuálním prostředí, aktivuje se v závorkách obsah k vykreslení.

## <a name="environment-tag-helper-attributes"></a>Atributy pomocné rutiny značky prostředí

### <a name="names"></a>názvy

Přijme jeden hostitelský název prostředí nebo seznam oddělený čárkami hostování prostředí názvy, které aktivují vykreslování závorkách obsahu.

Tyto hodnoty budou porovnány s aktuální hodnota vrácená z statické vlastnosti ASP.NET Core `HostingEnvironment.EnvironmentName`.  Tato hodnota je jeden z následujících: **pracovní**; **Vývoj** nebo **produkční**. Porovnání se ignoruje velikost písmen.

Příklad platné `environment` Pomocník značky:

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a>Zahrnout a vyloučit atributy

ASP.NET Core 2.x přidá `include`  &  `exclude` atributy. Tyto atributy určují vykreslování závorkách obsahu na základě zahrnuté ani vyloučené hostování prostředí názvů.

### <a name="include-aspnet-core-20-and-later"></a>Zahrnout ASP.NET Core 2.0 nebo novější

`include` Vlastnost má podobné chování `names` atribut v ASP.NET Core 1.0.

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a>vyloučit jádro ASP.NET 2.0 a vyšší

Naproti tomu `exclude` vlastnost umožňuje `EnvironmentTagHelper` vykreslení závorkách obsahu pro všechny názvy hostitelských prostředí s výjimkou vymazáním, který jste zadali.

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a>Další zdroje

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
