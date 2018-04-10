---
uid: mvc/overview/releases/mvc51-release-notes
title: Co je nového v architektuře ASP.NET MVC 5.1 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: be10486c9fd39738f44cdda4fedb409058017601
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/10/2018
---
<a name="whats-new-in-aspnet-mvc-51"></a>Co je nového v architektuře ASP.NET MVC 5.1
====================
by [Microsoft](https://github.com/microsoft)

Toto téma popisuje, co je nového pro ASP.NET Web MVC 5.1.

- [Požadavky na software](#SoftwareRequirements)
- [Stáhnout](#download)
- [Dokumentace](#documentation)
- [Nové funkce v architektuře ASP.NET MVC 5.1](#new-features)

    - [Atribut směrování vylepšení](#AttributeRouting)
    - [Zavedení podpory pro editor šablony](#Bootstrap)
    - [Podpora výčtu v zobrazeních.](#Enum)
    - [Nerušivý ověření pro atributy MinLength/MaxLength](#Unobtrusive)
    - [Podpora kontext "this" v Nerušivý Ajax](#thisContext)
- [Známé problémy a nejnovější změny](#KnownBreakingChanges)- [opravy chyb](#bug-fixes)

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a>Požadavky na software

- Sadu Visual Studio 2012: Stáhněte si [ASP.NET a webové nástroje pro sadu Visual Studio 2012 2013.1](https://go.microsoft.com/fwlink/?LinkId=390062).
- Visual Studio 2013: Stáhněte si [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064). Tato aktualizace je potřeba pro úpravu zobrazení syntaxe Razor 5.1 ASP.NET MVC.

<a id="download"></a>
## <a name="download"></a>Stáhnout

Funkce modulu runtime vydávají jako balíčků NuGet v galerii NuGet. Všechny balíčky modulu runtime použijte [sémantické verze](http://semver.org/) specifikace. Nejnovější balíček ASP.NET MVC 5.1 RTM má následující verze: "5.1.2". Můžete instalovat nebo aktualizovat tyto balíčky prostřednictvím [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/). Verze také zahrnuje odpovídající lokalizované balíčky na NuGet.

Můžete instalovat nebo aktualizovat balíčky NuGet vydaná pomocí konzoly Správce balíčků NuGet:

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentace

Kurzy a další informace o ASP.NET MVC 5.1 RTM jsou dostupné na webu technologie ASP.NET ( https://www.asp.net). 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a>Nové funkce v architektuře ASP.NET MVC 5.1

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a>Atribut směrování vylepšení

 Atribut směrování nyní podporuje omezení, povolení správy verzí a hlavičky na základě výběru trasy. Mnoho aspektů trasy atributů jsou nyní přizpůsobit pomocí `IDirectRouteFactory` rozhraní a `RouteFactoryAttribute` třídy. Předpona trasy je nyní extensible prostřednictvím `IRoutePrefix` rozhraní a `RoutePrefixAttribute` třídy. 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a>Podpora výčtu v zobrazeních.

1. Nové `@Html.EnumDropDownListFor()` pomocné metody. Ty se mají používat jako většinu pomocné rutiny HTML s výstrahou, který se musí vyhodnotit výraz [výčtu](https://msdn.microsoft.com/en-us/library/cc138362.aspx) typu nebo [Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) kde `T` je [výčtu](https://msdn.microsoft.com/en-us/library/cc138362.aspx) typu. Použití `EnumHelper.IsValidForEnumHelper()` Zkontrolujte tyto požadavky.
2. Nové `EnumHelper.GetSelectList()` metody, které vrací `IList<SelectListItem>`. To je užitečné, když potřebujete upravit seznam pro výběr před volání, například `@Html.DropDownListFor()`, nebo pokud chcete zobrazit názvy který `@Html.EnumDropDownListFor()` zobrazuje.

Následující kód ukazuje tato rozhraní API.

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

Zobrazí úplný příklad [zde](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a>Zavedení podpory pro editor šablony

Jsme teď povolí předávání v atributech HTML v [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) jako [anonymní objekt](https://msdn.microsoft.com/en-us/library/bb397696.aspx).

Příklad:

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a>Nerušivý ověření pro MinLengthAttribute a MaxLengthAttribute

Ověřování na straně klienta pro typy řetězec a pole se teď podporuje pro vlastnosti označených pomocí [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) a [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) atributy.

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a>Podpora kontext "this" v Nerušivý Ajax

Funkce zpětného volání (`OnBegin, OnComplete, OnFailure, OnSuccess`) teď bude moct vyhledat volajícím element prostřednictvím `this` kontextu. Příklad:

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a>Známé problémy a nejnovější změny

### <a name="attribute-routing"></a>Atribut směrování

Mnohoznačnosti v atributu směrování odpovídá teď oznámí chybu místo výběru na první shodu.

Atribut trasy mají zakázáno používat `{controller}` parametr a pomocí `{action}` parametr v směrování umístit na akce. Používá tyto parametry velmi pravděpodobně povede k nejednoznačnosti. 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Generování uživatelského rozhraní MVC nebo webového rozhraní API do projektu s 5.1 výsledky balíčky v 5.0 balíčky pro šablony, které již nejsou v projektu

Aktualizace balíčků NuGet pro ASP.NET MVC 5.1 RTM nelze aktualizovat, jako je ASP.NET generování uživatelského rozhraní nástroje sady Visual Studio nebo šablona projektu webové aplikace ASP.NET. Používají předchozí verzi modulu runtime ASP.NET balíčky (5.0.0.0). Generování uživatelského rozhraní ASP.NET v důsledku toho nainstaluje předchozí verzi (5.0.0.0) požadované balíčky, pokud již nejsou k dispozici v projektech. ASP.NET generování uživatelského rozhraní v Visual Studio 2013 RTM nebo Update 1, ale nepřepíše nejnovější balíčků ve vašich projektů. Pokud používáte generování uživatelského rozhraní ASP.NET po aktualizaci balíčků projekty webového rozhraní API 2.1 nebo ASP.NET MVC 5.1, ujistěte se, že verze webového rozhraní API a rozhraní ASP.NET MVC jsou konzistentní. 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Zvýraznění syntaxe Razor zobrazení v sadě Visual Studio 2013

Pokud aktualizujete k ASP.NET MVC 5.1 RTM bez aktualizace Visual Studio 2013, získáte nebude editor Visual Studio – podpora pro zvýraznění syntaxe při úpravách zobrazení syntaxe Razor. Musíte se k aktualizaci Visual Studio 2013 získat tuto podporu. 

### <a name="type-renames"></a>Přejmenuje typu

Některé typy používané pro atribut směrování rozšíření jsou přejmenovat v 5.1 RTM.

| **Původní název typu (5.1 RC)** | **Název nového typu (5.1 RTM)** |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Opravy chyb

Tato verze rovněž obsahuje několik oprav chyb. Úplný seznam najdete:

- [5.1.0 balíček](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 balíčku](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

5.1.2 balíček obsahuje aktualizace IntelliSense, ale žádné opravy chyb.
