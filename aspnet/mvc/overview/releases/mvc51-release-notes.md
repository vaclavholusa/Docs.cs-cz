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
<a name="whats-new-in-aspnet-mvc-51"></a><span data-ttu-id="f43ef-102">Co je nového v architektuře ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="f43ef-102">What's New in ASP.NET MVC 5.1</span></span>
====================
<span data-ttu-id="f43ef-103">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f43ef-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="f43ef-104">Toto téma popisuje, co je nového v ASP.NET Web MVC 5.1.</span><span class="sxs-lookup"><span data-stu-id="f43ef-104">This topic describes what's new for ASP.NET Web MVC 5.1.</span></span>

- [<span data-ttu-id="f43ef-105">Požadavky na software</span><span class="sxs-lookup"><span data-stu-id="f43ef-105">Software Requirements</span></span>](#SoftwareRequirements)
- [<span data-ttu-id="f43ef-106">Stáhnout</span><span class="sxs-lookup"><span data-stu-id="f43ef-106">Download</span></span>](#download)
- [<span data-ttu-id="f43ef-107">Dokumentace</span><span class="sxs-lookup"><span data-stu-id="f43ef-107">Documentation</span></span>](#documentation)
- [<span data-ttu-id="f43ef-108">Nové funkce v architektuře ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="f43ef-108">New Features in ASP.NET MVC 5.1</span></span>](#new-features)

    - [<span data-ttu-id="f43ef-109">Atribut směrování vylepšení</span><span class="sxs-lookup"><span data-stu-id="f43ef-109">Attribute routing improvements</span></span>](#AttributeRouting)
    - [<span data-ttu-id="f43ef-110">Zavedení podpory pro editor šablon</span><span class="sxs-lookup"><span data-stu-id="f43ef-110">Bootstrap support for editor templates</span></span>](#Bootstrap)
    - [<span data-ttu-id="f43ef-111">Podpora výčtu v zobrazeních.</span><span class="sxs-lookup"><span data-stu-id="f43ef-111">Enum support in views</span></span>](#Enum)
    - [<span data-ttu-id="f43ef-112">Nerušivý ověření pro atributy MinLength nebo MaxLength</span><span class="sxs-lookup"><span data-stu-id="f43ef-112">Unobtrusive validation for MinLength/MaxLength Attributes</span></span>](#Unobtrusive)
    - [<span data-ttu-id="f43ef-113">Podpora 'this' kontextu v Nerušivého jazyka Ajax</span><span class="sxs-lookup"><span data-stu-id="f43ef-113">Supporting the ‘this' context in Unobtrusive Ajax</span></span>](#thisContext)
- <span data-ttu-id="f43ef-114">[Známé problémy a změny způsobující chyby](#KnownBreakingChanges)- [opravy chyb](#bug-fixes)</span><span class="sxs-lookup"><span data-stu-id="f43ef-114">[Known Issues and Breaking Changes](#KnownBreakingChanges)- [Bug Fixes](#bug-fixes)</span></span>

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="f43ef-115">Požadavky na software</span><span class="sxs-lookup"><span data-stu-id="f43ef-115">Software Requirements</span></span>

- <span data-ttu-id="f43ef-116">Visual Studio 2012: Stáhněte si [technologie ASP.NET a webové nástroje 2013.1 pro Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).</span><span class="sxs-lookup"><span data-stu-id="f43ef-116">Visual Studio 2012: Download [ASP.NET and Web Tools 2013.1 for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).</span></span>
- <span data-ttu-id="f43ef-117">Visual Studio 2013: Stáhněte si [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064).</span><span class="sxs-lookup"><span data-stu-id="f43ef-117">Visual Studio 2013: Download [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064).</span></span> <span data-ttu-id="f43ef-118">Tato aktualizace je potřeba pro úpravu zobrazení ASP.NET MVC 5.1 syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="f43ef-118">This update is needed for editing ASP.NET MVC 5.1 Razor Views.</span></span>

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="f43ef-119">Stáhnout</span><span class="sxs-lookup"><span data-stu-id="f43ef-119">Download</span></span>

<span data-ttu-id="f43ef-120">Funkce modulu runtime jsou vydané jako balíčky NuGet v galerii NuGet.</span><span class="sxs-lookup"><span data-stu-id="f43ef-120">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="f43ef-121">Postupujte podle všech balíčků modulu runtime [Semantic Versioning](http://semver.org/) specifikace.</span><span class="sxs-lookup"><span data-stu-id="f43ef-121">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="f43ef-122">Nejnovější balíček RTM technologie ASP.NET MVC 5.1 má následující verzi: "5.1.2".</span><span class="sxs-lookup"><span data-stu-id="f43ef-122">The latest ASP.NET MVC 5.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="f43ef-123">Můžete nainstalovat nebo aktualizovat tyto balíčky prostřednictvím [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span><span class="sxs-lookup"><span data-stu-id="f43ef-123">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span></span> <span data-ttu-id="f43ef-124">Tato verze rovněž obsahuje odpovídající lokalizované balíčky NuGet.</span><span class="sxs-lookup"><span data-stu-id="f43ef-124">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="f43ef-125">Můžete nainstalovat nebo aktualizovat vydané balíčky NuGet pomocí konzoly Správce balíčků NuGet:</span><span class="sxs-lookup"><span data-stu-id="f43ef-125">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="f43ef-126">Dokumentace</span><span class="sxs-lookup"><span data-stu-id="f43ef-126">Documentation</span></span>

<span data-ttu-id="f43ef-127">Kurzy a další informace o ASP.NET MVC 5.1 RTM jsou k dispozici z webu technologie ASP.NET ( https://www.asp.net).</span><span class="sxs-lookup"><span data-stu-id="f43ef-127">Tutorials and other information about ASP.NET MVC 5.1 RTM are available from the ASP.NET web site ( https://www.asp.net).</span></span> 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a><span data-ttu-id="f43ef-128">Nové funkce v architektuře ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="f43ef-128">New Features in ASP.NET MVC 5.1</span></span>

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a><span data-ttu-id="f43ef-129">Atribut směrování vylepšení</span><span class="sxs-lookup"><span data-stu-id="f43ef-129">Attribute routing improvements</span></span>

 <span data-ttu-id="f43ef-130">Směrování nyní podporuje omezení, povolení správy verzí a záhlaví atributů na základě výběru trasy.</span><span class="sxs-lookup"><span data-stu-id="f43ef-130">Attribute routing now supports constraints, enabling versioning and header based route selection.</span></span> <span data-ttu-id="f43ef-131">Mnoho aspektů trasy atributů jsou nyní přizpůsobit prostřednictvím `IDirectRouteFactory` rozhraní a `RouteFactoryAttribute` třídy.</span><span class="sxs-lookup"><span data-stu-id="f43ef-131">Many aspects of attribute routes are now customizable via the `IDirectRouteFactory` interface and `RouteFactoryAttribute` class.</span></span> <span data-ttu-id="f43ef-132">Předpona trasy je rozšiřitelný prostřednictvím `IRoutePrefix` rozhraní a `RoutePrefixAttribute` třídy.</span><span class="sxs-lookup"><span data-stu-id="f43ef-132">The route prefix is now extensible via the `IRoutePrefix` interface and `RoutePrefixAttribute` class.</span></span> 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a><span data-ttu-id="f43ef-133">Podpora výčtu v zobrazeních.</span><span class="sxs-lookup"><span data-stu-id="f43ef-133">Enum support in views</span></span>

1. <span data-ttu-id="f43ef-134">Nové `@Html.EnumDropDownListFor()` pomocné metody.</span><span class="sxs-lookup"><span data-stu-id="f43ef-134">New `@Html.EnumDropDownListFor()` helper methods.</span></span> <span data-ttu-id="f43ef-135">Ty by měla sloužit oblíbili pomocných rutin HTML s výstrahou, která se musí vyhodnotit výraz [výčtu](https://msdn.microsoft.com/en-us/library/cc138362.aspx) typu nebo [Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) kde `T` je [výčtu](https://msdn.microsoft.com/en-us/library/cc138362.aspx) typu.</span><span class="sxs-lookup"><span data-stu-id="f43ef-135">These should be used like most of the HTML helpers with the caveat that the expression must evaluate to an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type or a [Nullable&lt;T&gt;](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) where `T` is an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type.</span></span> <span data-ttu-id="f43ef-136">Použití `EnumHelper.IsValidForEnumHelper()` Zkontrolujte tyto požadavky.</span><span class="sxs-lookup"><span data-stu-id="f43ef-136">Use `EnumHelper.IsValidForEnumHelper()` to check these requirements.</span></span>
2. <span data-ttu-id="f43ef-137">Nové `EnumHelper.GetSelectList()` metody, které vracejí `IList<SelectListItem>`.</span><span class="sxs-lookup"><span data-stu-id="f43ef-137">New `EnumHelper.GetSelectList()` methods which return an `IList<SelectListItem>`.</span></span> <span data-ttu-id="f43ef-138">To je užitečné, když budete chtít pracovat s seznamu výběru před voláním, například `@Html.DropDownListFor()`, nebo pokud chcete zobrazit názvy který `@Html.EnumDropDownListFor()` ukazuje.</span><span class="sxs-lookup"><span data-stu-id="f43ef-138">This is useful when you need to manipulate a select list prior to calling, for example, `@Html.DropDownListFor()`, or when you wish to display the names which `@Html.EnumDropDownListFor()` shows.</span></span>

<span data-ttu-id="f43ef-139">Následující kód ukazuje tato rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f43ef-139">The following code shows these APIs.</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

<span data-ttu-id="f43ef-140">Zobrazí se kompletní příklad [tady](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).</span><span class="sxs-lookup"><span data-stu-id="f43ef-140">You can see a complete example [here](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).</span></span>

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a><span data-ttu-id="f43ef-141">Zavedení podpory pro editor šablon</span><span class="sxs-lookup"><span data-stu-id="f43ef-141">Bootstrap support for editor templates</span></span>

<span data-ttu-id="f43ef-142">Nyní jsme atributy HTML v povolení předávání [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) jako [anonymní objekt](https://msdn.microsoft.com/en-us/library/bb397696.aspx).</span><span class="sxs-lookup"><span data-stu-id="f43ef-142">We now allow passing in HTML attributes in [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) as an [anonymous object](https://msdn.microsoft.com/en-us/library/bb397696.aspx).</span></span>

<span data-ttu-id="f43ef-143">Příklad:</span><span class="sxs-lookup"><span data-stu-id="f43ef-143">For example:</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a><span data-ttu-id="f43ef-144">Ověření nerušivého MinLengthAttribute a MaxLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="f43ef-144">Unobtrusive validation for MinLengthAttribute and MaxLengthAttribute</span></span>

<span data-ttu-id="f43ef-145">Ověřování na straně klienta pro typy řetězce a pole se teď bude podporovat pro vlastnosti upravené pomocí [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) a [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) atributy.</span><span class="sxs-lookup"><span data-stu-id="f43ef-145">Client-side validation for string and array types will now be supported for properties decorated with the [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) and [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) attributes.</span></span>

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a><span data-ttu-id="f43ef-146">Podpora 'this' kontextu v Nerušivého jazyka Ajax</span><span class="sxs-lookup"><span data-stu-id="f43ef-146">Supporting the ‘this' context in Unobtrusive Ajax</span></span>

<span data-ttu-id="f43ef-147">Funkce zpětného volání (`OnBegin, OnComplete, OnFailure, OnSuccess`) teď budou moct vyhledejte prvek vyvolání prostřednictvím `this` kontextu.</span><span class="sxs-lookup"><span data-stu-id="f43ef-147">The callback functions (`OnBegin, OnComplete, OnFailure, OnSuccess`) will now be able to locate the invoking element via the `this` context.</span></span> <span data-ttu-id="f43ef-148">Příklad:</span><span class="sxs-lookup"><span data-stu-id="f43ef-148">For example:</span></span>

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="f43ef-149">Známé problémy a změny způsobující chyby</span><span class="sxs-lookup"><span data-stu-id="f43ef-149">Known Issues and Breaking Changes</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="f43ef-150">Směrování atributů</span><span class="sxs-lookup"><span data-stu-id="f43ef-150">Attribute Routing</span></span>

<span data-ttu-id="f43ef-151">Nejednoznačnosti v atributu směrování odpovídá teď oznámí chybu spíše než vyberete první shoda.</span><span class="sxs-lookup"><span data-stu-id="f43ef-151">Ambiguities in attribute routing matches will now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="f43ef-152">Atribut trasy mají zakázáno používat `{controller}` parametr a používat `{action}` umístí parametr trasy na akce.</span><span class="sxs-lookup"><span data-stu-id="f43ef-152">Attribute routes are prohibited from using the `{controller}` parameter, and from using the `{action}` parameter on routes placed on actions.</span></span> <span data-ttu-id="f43ef-153">Použití těchto parametrů velmi pravděpodobné, že by mohlo dojít k nejednoznačnosti.</span><span class="sxs-lookup"><span data-stu-id="f43ef-153">Uses of these parameters would very likely lead to ambiguities.</span></span> 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="f43ef-154">Generování uživatelského rozhraní MVC nebo webového rozhraní API do projektu s 5.1 balíčků následek 5.0 balíčky, které už neexistují v projektu</span><span class="sxs-lookup"><span data-stu-id="f43ef-154">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="f43ef-155">Aktualizují se balíčky NuGet pro ASP.NET MVC 5.1 RTM nelze aktualizovat nástroje sady Visual Studio, jako je například technologie ASP.NET generování uživatelského rozhraní nebo šablonu projektu webové aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f43ef-155">Updating NuGet packages for ASP.NET MVC 5.1 RTM does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="f43ef-156">Používají předchozí verze balíčky runtime ASP.NET (5.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="f43ef-156">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="f43ef-157">Generování uživatelského rozhraní ASP.NET v důsledku toho nainstaluje předchozí verzi (5.0.0.0) požadované balíčky, pokud již nejsou k dispozici ve vašich projektech.</span><span class="sxs-lookup"><span data-stu-id="f43ef-157">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="f43ef-158">ASP.NET generování uživatelského rozhraní v aplikaci Visual Studio 2013 RTM a Update 1, ale nepřepisuje nejnovější balíčky v projektech.</span><span class="sxs-lookup"><span data-stu-id="f43ef-158">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="f43ef-159">Pokud používáte ASP.NET generování uživatelského rozhraní po aktualizaci balíčků vaše projekty webového rozhraní API 2.1 nebo technologie ASP.NET MVC 5.1, ujistěte se, že verze webového rozhraní API a architektura ASP.NET MVC jsou konzistentní vzhledem k aplikacím.</span><span class="sxs-lookup"><span data-stu-id="f43ef-159">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span> 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a><span data-ttu-id="f43ef-160">Zvýraznění syntaxe pro zobrazení syntaxe Razor v sadě Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="f43ef-160">Syntax Highlighting for Razor Views in Visual Studio 2013</span></span>

<span data-ttu-id="f43ef-161">Pokud aktualizujete na verzi ASP.NET MVC 5.1 RTM bez aktualizace sady Visual Studio 2013, nezískáte editoru Visual Studio – podpora pro zvýraznění syntaxe při úpravách zobrazení syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="f43ef-161">If you update to ASP.NET MVC 5.1 RTM without updating Visual Studio 2013, you will not get Visual Studio editor support for syntax highlighting while editing the Razor views.</span></span> <span data-ttu-id="f43ef-162">Je potřeba aktualizovat Visual Studio 2013 k získání této podpory.</span><span class="sxs-lookup"><span data-stu-id="f43ef-162">You will need to update Visual Studio 2013 to get this support.</span></span> 

### <a name="type-renames"></a><span data-ttu-id="f43ef-163">Typ přejmenování</span><span class="sxs-lookup"><span data-stu-id="f43ef-163">Type Renames</span></span>

<span data-ttu-id="f43ef-164">Některé typy používané pro rozšíření směrování atributů jsou přejmenovány 5.1 RTM.</span><span class="sxs-lookup"><span data-stu-id="f43ef-164">Some of the types used for attribute routing extensibility are renamed in 5.1 RTM.</span></span>

| <span data-ttu-id="f43ef-165">**Starý název typu (5.1 RC)**</span><span class="sxs-lookup"><span data-stu-id="f43ef-165">**Old Type Name (5.1 RC)**</span></span> | <span data-ttu-id="f43ef-166">**Název nového typu (5.1 RTM)**</span><span class="sxs-lookup"><span data-stu-id="f43ef-166">**New Type Name (5.1 RTM)**</span></span> |
| --- | --- |
| <span data-ttu-id="f43ef-167">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="f43ef-167">IDirectRouteProvider</span></span> | <span data-ttu-id="f43ef-168">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="f43ef-168">IDirectRouteFactory</span></span> |
| <span data-ttu-id="f43ef-169">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="f43ef-169">RouteProviderAttribute</span></span> | <span data-ttu-id="f43ef-170">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="f43ef-170">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="f43ef-171">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="f43ef-171">DirectRouteProviderContext</span></span> | <span data-ttu-id="f43ef-172">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="f43ef-172">DirectRouteFactoryContext</span></span> |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="f43ef-173">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="f43ef-173">Bug Fixes</span></span>

<span data-ttu-id="f43ef-174">Tato verze rovněž obsahuje několik oprav chyb.</span><span class="sxs-lookup"><span data-stu-id="f43ef-174">This release also includes several bug fixes.</span></span> <span data-ttu-id="f43ef-175">Úplný seznam najdete:</span><span class="sxs-lookup"><span data-stu-id="f43ef-175">You can find the complete list here:</span></span>

- [<span data-ttu-id="f43ef-176">5.1.0 balíček</span><span class="sxs-lookup"><span data-stu-id="f43ef-176">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="f43ef-177">5.1.1 balíček</span><span class="sxs-lookup"><span data-stu-id="f43ef-177">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<span data-ttu-id="f43ef-178">5.1.2 balíček obsahuje aktualizace IntelliSense, ale žádné opravy chyb.</span><span class="sxs-lookup"><span data-stu-id="f43ef-178">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
