---
uid: mvc/overview/releases/mvc51-release-notes
title: Co je nového v architektuře ASP.NET MVC 5.1 | Dokumentace Microsoftu
author: microsoft
description: ''
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 6cdf5ae42457ab10e30693a1b77b4aee65570be5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751831"
---
<a name="whats-new-in-aspnet-mvc-51"></a>Co je nového v architektuře ASP.NET MVC 5.1
====================
podle [Microsoft](https://github.com/microsoft)

Toto téma popisuje, co je nového v ASP.NET Web MVC 5.1.

- [Požadavky na software](#SoftwareRequirements)
- [Stáhnout](#download)
- [Dokumentace](#documentation)
- [Nové funkce v architektuře ASP.NET MVC 5.1](#new-features)

    - [Atribut směrování vylepšení](#AttributeRouting)
    - [Zavedení podpory pro editor šablon](#Bootstrap)
    - [Podpora výčtu v zobrazeních.](#Enum)
    - [Nerušivý ověření pro atributy MinLength nebo MaxLength](#Unobtrusive)
    - [Podpora 'this' kontextu v Nerušivého jazyka Ajax](#thisContext)
- [Známé problémy a změny způsobující chyby](#KnownBreakingChanges)- [opravy chyb](#bug-fixes)

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a>Požadavky na software

- Visual Studio 2012: Stáhněte si [technologie ASP.NET a webové nástroje 2013.1 pro Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).
- Visual Studio 2013: Stáhněte si [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064). Tato aktualizace je potřeba pro úpravu zobrazení ASP.NET MVC 5.1 syntaxe Razor.

<a id="download"></a>
## <a name="download"></a>Stáhnout

Funkce modulu runtime jsou vydané jako balíčky NuGet v galerii NuGet. Postupujte podle všech balíčků modulu runtime [Semantic Versioning](http://semver.org/) specifikace. Nejnovější balíček RTM technologie ASP.NET MVC 5.1 má následující verzi: "5.1.2". Můžete nainstalovat nebo aktualizovat tyto balíčky prostřednictvím [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/). Tato verze rovněž obsahuje odpovídající lokalizované balíčky NuGet.

Můžete nainstalovat nebo aktualizovat vydané balíčky NuGet pomocí konzoly Správce balíčků NuGet:

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentace

Kurzy a další informace o ASP.NET MVC 5.1 RTM jsou k dispozici z webu technologie ASP.NET ( https://www.asp.net). 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a>Nové funkce v architektuře ASP.NET MVC 5.1

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a>Atribut směrování vylepšení

 Směrování nyní podporuje omezení, povolení správy verzí a záhlaví atributů na základě výběru trasy. Mnoho aspektů trasy atributů jsou nyní přizpůsobit prostřednictvím `IDirectRouteFactory` rozhraní a `RouteFactoryAttribute` třídy. Předpona trasy je rozšiřitelný prostřednictvím `IRoutePrefix` rozhraní a `RoutePrefixAttribute` třídy. 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a>Podpora výčtu v zobrazeních.

1. Nové `@Html.EnumDropDownListFor()` pomocné metody. Ty by měla sloužit oblíbili pomocných rutin HTML s výstrahou, která se musí vyhodnotit výraz [výčtu](https://msdn.microsoft.com/en-us/library/cc138362.aspx) typu nebo [Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) kde `T` je [výčtu](https://msdn.microsoft.com/en-us/library/cc138362.aspx) typu. Použití `EnumHelper.IsValidForEnumHelper()` Zkontrolujte tyto požadavky.
2. Nové `EnumHelper.GetSelectList()` metody, které vracejí `IList<SelectListItem>`. To je užitečné, když budete chtít pracovat s seznamu výběru před voláním, například `@Html.DropDownListFor()`, nebo pokud chcete zobrazit názvy který `@Html.EnumDropDownListFor()` ukazuje.

Následující kód ukazuje tato rozhraní API.

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

Zobrazí se kompletní příklad [tady](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a>Zavedení podpory pro editor šablon

Nyní jsme atributy HTML v povolení předávání [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) jako [anonymní objekt](https://msdn.microsoft.com/en-us/library/bb397696.aspx).

Příklad:

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a>Ověření nerušivého MinLengthAttribute a MaxLengthAttribute

Ověřování na straně klienta pro typy řetězce a pole se teď bude podporovat pro vlastnosti upravené pomocí [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) a [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) atributy.

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a>Podpora 'this' kontextu v Nerušivého jazyka Ajax

Funkce zpětného volání (`OnBegin, OnComplete, OnFailure, OnSuccess`) teď budou moct vyhledejte prvek vyvolání prostřednictvím `this` kontextu. Příklad:

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a>Známé problémy a změny způsobující chyby

### <a name="attribute-routing"></a>Směrování atributů

Nejednoznačnosti v atributu směrování odpovídá teď oznámí chybu spíše než vyberete první shoda.

Atribut trasy mají zakázáno používat `{controller}` parametr a používat `{action}` umístí parametr trasy na akce. Použití těchto parametrů velmi pravděpodobné, že by mohlo dojít k nejednoznačnosti. 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Generování uživatelského rozhraní MVC nebo webového rozhraní API do projektu s 5.1 balíčků následek 5.0 balíčky, které už neexistují v projektu

Aktualizují se balíčky NuGet pro ASP.NET MVC 5.1 RTM nelze aktualizovat nástroje sady Visual Studio, jako je například technologie ASP.NET generování uživatelského rozhraní nebo šablonu projektu webové aplikace ASP.NET. Používají předchozí verze balíčky runtime ASP.NET (5.0.0.0). Generování uživatelského rozhraní ASP.NET v důsledku toho nainstaluje předchozí verzi (5.0.0.0) požadované balíčky, pokud již nejsou k dispozici ve vašich projektech. ASP.NET generování uživatelského rozhraní v aplikaci Visual Studio 2013 RTM a Update 1, ale nepřepisuje nejnovější balíčky v projektech. Pokud používáte ASP.NET generování uživatelského rozhraní po aktualizaci balíčků vaše projekty webového rozhraní API 2.1 nebo technologie ASP.NET MVC 5.1, ujistěte se, že verze webového rozhraní API a architektura ASP.NET MVC jsou konzistentní vzhledem k aplikacím. 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Zvýraznění syntaxe pro zobrazení syntaxe Razor v sadě Visual Studio 2013

Pokud aktualizujete na verzi ASP.NET MVC 5.1 RTM bez aktualizace sady Visual Studio 2013, nezískáte editoru Visual Studio – podpora pro zvýraznění syntaxe při úpravách zobrazení syntaxe Razor. Je potřeba aktualizovat Visual Studio 2013 k získání této podpory. 

### <a name="type-renames"></a>Typ přejmenování

Některé typy používané pro rozšíření směrování atributů jsou přejmenovány 5.1 RTM.

| **Starý název typu (5.1 RC)** | **Název nového typu (5.1 RTM)** |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Opravy chyb

Tato verze rovněž obsahuje několik oprav chyb. Úplný seznam najdete:

- [5.1.0 balíček](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 balíček](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

5.1.2 balíček obsahuje aktualizace IntelliSense, ale žádné opravy chyb.
