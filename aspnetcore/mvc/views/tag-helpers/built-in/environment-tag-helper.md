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
# <a name="environment-tag-helper-in-aspnet-core"></a>Pomocná rutina značky prostředí v ASP.NET Core

Podle [Peter Kellner](http://peterkellner.net) a [Ateya Hisham Bin](https://twitter.com/hishambinateya)

Pomocná rutina značky prostředí podmíněně vykreslí obsah uzavřené podle aktuální hostitelského prostředí. Jeho jediný atribut `names` je čárkami oddělený seznam prostředí názvů, který pokud všechny odpovídaly aktuálním prostředí, aktivují uzavřené obsah k vykreslení.

## <a name="environment-tag-helper-attributes"></a>Atributy pomocné rutiny značky prostředí

### <a name="names"></a>názvy

Přijímá jediný název hostitelské prostředí nebo čárkami oddělený seznam hostování názvy prostředí, které aktivují vykreslování uzavřené obsah.

Tyto hodnoty jsou ve srovnání s aktuální hodnota vrácená z ASP.NET Core statickou vlastnost `HostingEnvironment.EnvironmentName`.  Tato hodnota je jeden z následujících: **pracovní**; **Vývoj** nebo **produkční**. Porovnání ignoruje velikost písmen.

Příkladem platné `environment` je pomocná rutina značky:

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a>zahrnutí a vyloučení atributy

ASP.NET Core 2.x přidá `include`  &  `exclude` atributy. Tyto atributy určují, vykreslování uzavřené obsahu na základě zahrnuté ani vyloučené hostování prostředí názvů.

### <a name="include-aspnet-core-20-and-later"></a>Zahrnout ASP.NET Core 2.0 a vyšší

`include` Vlastnost má podobné chování `names` atribut v ASP.NET Core 1.0.

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a>vyloučit ASP.NET Core 2.0 a vyšší

Naproti tomu `exclude` vlastnost `EnvironmentTagHelper` vykreslení uzavřené obsahu pro všechny názvy hostitelských prostředí s výjimkou vymazáním, který jste zadali.

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a>Další zdroje

* <xref:fundamentals/environments>
