---
uid: mvc/overview/releases/mvc51-release-notes
title: "Co je nového v architektuře ASP.NET MVC 5.1 | Microsoft Docs"
author: microsoft
description: 
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
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-mvc-51"></a><span data-ttu-id="37a4b-102">Co je nového v architektuře ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="37a4b-102">What's New in ASP.NET MVC 5.1</span></span>
====================
<span data-ttu-id="37a4b-103">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="37a4b-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="37a4b-104">Toto téma popisuje, co je nového pro ASP.NET Web MVC 5.1.</span><span class="sxs-lookup"><span data-stu-id="37a4b-104">This topic describes what's new for ASP.NET Web MVC 5.1.</span></span>

- [<span data-ttu-id="37a4b-105">Požadavky na software</span><span class="sxs-lookup"><span data-stu-id="37a4b-105">Software Requirements</span></span>](#SoftwareRequirements)
- [<span data-ttu-id="37a4b-106">Stáhnout</span><span class="sxs-lookup"><span data-stu-id="37a4b-106">Download</span></span>](#download)
- [<span data-ttu-id="37a4b-107">Dokumentace</span><span class="sxs-lookup"><span data-stu-id="37a4b-107">Documentation</span></span>](#documentation)
- [<span data-ttu-id="37a4b-108">Nové funkce v architektuře ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="37a4b-108">New Features in ASP.NET MVC 5.1</span></span>](#new-features)

    - [<span data-ttu-id="37a4b-109">Atribut směrování vylepšení</span><span class="sxs-lookup"><span data-stu-id="37a4b-109">Attribute routing improvements</span></span>](#AttributeRouting)
    - [<span data-ttu-id="37a4b-110">Zavedení podpory pro editor šablony</span><span class="sxs-lookup"><span data-stu-id="37a4b-110">Bootstrap support for editor templates</span></span>](#Bootstrap)
    - [<span data-ttu-id="37a4b-111">Podpora výčtu v zobrazeních.</span><span class="sxs-lookup"><span data-stu-id="37a4b-111">Enum support in views</span></span>](#Enum)
    - [<span data-ttu-id="37a4b-112">Nerušivý ověření pro atributy MinLength/MaxLength</span><span class="sxs-lookup"><span data-stu-id="37a4b-112">Unobtrusive validation for MinLength/MaxLength Attributes</span></span>](#Unobtrusive)
    - [<span data-ttu-id="37a4b-113">Podpora kontext "this" v Nerušivý Ajax</span><span class="sxs-lookup"><span data-stu-id="37a4b-113">Supporting the ‘this' context in Unobtrusive Ajax</span></span>](#thisContext)
- <span data-ttu-id="37a4b-114">[Známé problémy a nejnovější změny](#KnownBreakingChanges)- [opravy chyb](#bug-fixes)</span><span class="sxs-lookup"><span data-stu-id="37a4b-114">[Known Issues and Breaking Changes](#KnownBreakingChanges)- [Bug Fixes](#bug-fixes)</span></span>

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="37a4b-115">Požadavky na software</span><span class="sxs-lookup"><span data-stu-id="37a4b-115">Software Requirements</span></span>

- <span data-ttu-id="37a4b-116">Sadu Visual Studio 2012: Stáhněte si [ASP.NET a webové nástroje pro sadu Visual Studio 2012 2013.1](https://go.microsoft.com/fwlink/?LinkId=390062).</span><span class="sxs-lookup"><span data-stu-id="37a4b-116">Visual Studio 2012: Download [ASP.NET and Web Tools 2013.1 for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).</span></span>
- <span data-ttu-id="37a4b-117">Visual Studio 2013: Stáhněte si [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064).</span><span class="sxs-lookup"><span data-stu-id="37a4b-117">Visual Studio 2013: Download [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064).</span></span> <span data-ttu-id="37a4b-118">Tato aktualizace je potřeba pro úpravu zobrazení syntaxe Razor 5.1 ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="37a4b-118">This update is needed for editing ASP.NET MVC 5.1 Razor Views.</span></span>

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="37a4b-119">Stáhnout</span><span class="sxs-lookup"><span data-stu-id="37a4b-119">Download</span></span>

<span data-ttu-id="37a4b-120">Funkce modulu runtime vydávají jako balíčků NuGet v galerii NuGet.</span><span class="sxs-lookup"><span data-stu-id="37a4b-120">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="37a4b-121">Všechny balíčky modulu runtime použijte [sémantické verze](http://semver.org/) specifikace.</span><span class="sxs-lookup"><span data-stu-id="37a4b-121">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="37a4b-122">Nejnovější balíček ASP.NET MVC 5.1 RTM má následující verze: "5.1.2".</span><span class="sxs-lookup"><span data-stu-id="37a4b-122">The latest ASP.NET MVC 5.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="37a4b-123">Můžete instalovat nebo aktualizovat tyto balíčky prostřednictvím [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span><span class="sxs-lookup"><span data-stu-id="37a4b-123">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span></span> <span data-ttu-id="37a4b-124">Verze také zahrnuje odpovídající lokalizované balíčky na NuGet.</span><span class="sxs-lookup"><span data-stu-id="37a4b-124">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="37a4b-125">Můžete instalovat nebo aktualizovat balíčky NuGet vydaná pomocí konzoly Správce balíčků NuGet:</span><span class="sxs-lookup"><span data-stu-id="37a4b-125">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="37a4b-126">Dokumentace</span><span class="sxs-lookup"><span data-stu-id="37a4b-126">Documentation</span></span>

<span data-ttu-id="37a4b-127">Kurzy a další informace o ASP.NET MVC 5.1 RTM jsou dostupné na webu technologie ASP.NET (https://www.asp.net).</span><span class="sxs-lookup"><span data-stu-id="37a4b-127">Tutorials and other information about ASP.NET MVC 5.1 RTM are available from the ASP.NET web site ( https://www.asp.net).</span></span> 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a><span data-ttu-id="37a4b-128">Nové funkce v architektuře ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="37a4b-128">New Features in ASP.NET MVC 5.1</span></span>

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a><span data-ttu-id="37a4b-129">Atribut směrování vylepšení</span><span class="sxs-lookup"><span data-stu-id="37a4b-129">Attribute routing improvements</span></span>

 <span data-ttu-id="37a4b-130">Atribut směrování nyní podporuje omezení, povolení správy verzí a hlavičky na základě výběru trasy.</span><span class="sxs-lookup"><span data-stu-id="37a4b-130">Attribute routing now supports constraints, enabling versioning and header based route selection.</span></span> <span data-ttu-id="37a4b-131">Mnoho aspektů trasy atributů jsou nyní přizpůsobit pomocí `IDirectRouteFactory` rozhraní a `RouteFactoryAttribute` třídy.</span><span class="sxs-lookup"><span data-stu-id="37a4b-131">Many aspects of attribute routes are now customizable via the `IDirectRouteFactory` interface and `RouteFactoryAttribute` class.</span></span> <span data-ttu-id="37a4b-132">Předpona trasy je nyní extensible prostřednictvím `IRoutePrefix` rozhraní a `RoutePrefixAttribute` třídy.</span><span class="sxs-lookup"><span data-stu-id="37a4b-132">The route prefix is now extensible via the `IRoutePrefix` interface and `RoutePrefixAttribute` class.</span></span> 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a><span data-ttu-id="37a4b-133">Podpora výčtu v zobrazeních.</span><span class="sxs-lookup"><span data-stu-id="37a4b-133">Enum support in views</span></span>

1. <span data-ttu-id="37a4b-134">Nové `@Html.EnumDropDownListFor()` pomocné metody.</span><span class="sxs-lookup"><span data-stu-id="37a4b-134">New `@Html.EnumDropDownListFor()` helper methods.</span></span> <span data-ttu-id="37a4b-135">Ty se mají používat jako většinu pomocné rutiny HTML s výstrahou, který se musí vyhodnotit výraz [výčtu](https://msdn.microsoft.com/en-us/library/cc138362.aspx) typu nebo [Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) kde `T` je [výčtu](https://msdn.microsoft.com/en-us/library/cc138362.aspx) typu.</span><span class="sxs-lookup"><span data-stu-id="37a4b-135">These should be used like most of the HTML helpers with the caveat that the expression must evaluate to an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type or a [Nullable&lt;T&gt;](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) where `T` is an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type.</span></span> <span data-ttu-id="37a4b-136">Použití `EnumHelper.IsValidForEnumHelper()` Zkontrolujte tyto požadavky.</span><span class="sxs-lookup"><span data-stu-id="37a4b-136">Use `EnumHelper.IsValidForEnumHelper()` to check these requirements.</span></span>
2. <span data-ttu-id="37a4b-137">Nové `EnumHelper.GetSelectList()` metody, které vrací `IList<SelectListItem>`.</span><span class="sxs-lookup"><span data-stu-id="37a4b-137">New `EnumHelper.GetSelectList()` methods which return an `IList<SelectListItem>`.</span></span> <span data-ttu-id="37a4b-138">To je užitečné, když potřebujete upravit seznam pro výběr před volání, například `@Html.DropDownListFor()`, nebo pokud chcete zobrazit názvy který `@Html.EnumDropDownListFor()` zobrazuje.</span><span class="sxs-lookup"><span data-stu-id="37a4b-138">This is useful when you need to manipulate a select list prior to calling, for example, `@Html.DropDownListFor()`, or when you wish to display the names which `@Html.EnumDropDownListFor()` shows.</span></span>

<span data-ttu-id="37a4b-139">Následující kód ukazuje tato rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="37a4b-139">The following code shows these APIs.</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

<span data-ttu-id="37a4b-140">Zobrazí úplný příklad [zde](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).</span><span class="sxs-lookup"><span data-stu-id="37a4b-140">You can see a complete example [here](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).</span></span>

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a><span data-ttu-id="37a4b-141">Zavedení podpory pro editor šablony</span><span class="sxs-lookup"><span data-stu-id="37a4b-141">Bootstrap support for editor templates</span></span>

<span data-ttu-id="37a4b-142">Jsme teď povolí předávání v atributech HTML v [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) jako [anonymní objekt](https://msdn.microsoft.com/en-us/library/bb397696.aspx).</span><span class="sxs-lookup"><span data-stu-id="37a4b-142">We now allow passing in HTML attributes in [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) as an [anonymous object](https://msdn.microsoft.com/en-us/library/bb397696.aspx).</span></span>

<span data-ttu-id="37a4b-143">Příklad:</span><span class="sxs-lookup"><span data-stu-id="37a4b-143">For example:</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a><span data-ttu-id="37a4b-144">Nerušivý ověření pro MinLengthAttribute a MaxLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="37a4b-144">Unobtrusive validation for MinLengthAttribute and MaxLengthAttribute</span></span>

<span data-ttu-id="37a4b-145">Ověřování na straně klienta pro typy řetězec a pole se teď podporuje pro vlastnosti označených pomocí [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) a [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) atributy.</span><span class="sxs-lookup"><span data-stu-id="37a4b-145">Client-side validation for string and array types will now be supported for properties decorated with the [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) and [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) attributes.</span></span>

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a><span data-ttu-id="37a4b-146">Podpora kontext "this" v Nerušivý Ajax</span><span class="sxs-lookup"><span data-stu-id="37a4b-146">Supporting the ‘this' context in Unobtrusive Ajax</span></span>

<span data-ttu-id="37a4b-147">Funkce zpětného volání (`OnBegin, OnComplete, OnFailure, OnSuccess`) teď bude moct vyhledat volajícím element prostřednictvím `this` kontextu.</span><span class="sxs-lookup"><span data-stu-id="37a4b-147">The callback functions (`OnBegin, OnComplete, OnFailure, OnSuccess`) will now be able to locate the invoking element via the `this` context.</span></span> <span data-ttu-id="37a4b-148">Příklad:</span><span class="sxs-lookup"><span data-stu-id="37a4b-148">For example:</span></span>

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="37a4b-149">Známé problémy a nejnovější změny</span><span class="sxs-lookup"><span data-stu-id="37a4b-149">Known Issues and Breaking Changes</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="37a4b-150">Atribut směrování</span><span class="sxs-lookup"><span data-stu-id="37a4b-150">Attribute Routing</span></span>

<span data-ttu-id="37a4b-151">Mnohoznačnosti v atributu směrování odpovídá teď oznámí chybu místo výběru na první shodu.</span><span class="sxs-lookup"><span data-stu-id="37a4b-151">Ambiguities in attribute routing matches will now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="37a4b-152">Atribut trasy mají zakázáno používat `{controller}` parametr a pomocí `{action}` parametr v směrování umístit na akce.</span><span class="sxs-lookup"><span data-stu-id="37a4b-152">Attribute routes are prohibited from using the `{controller}` parameter, and from using the `{action}` parameter on routes placed on actions.</span></span> <span data-ttu-id="37a4b-153">Používá tyto parametry velmi pravděpodobně povede k nejednoznačnosti.</span><span class="sxs-lookup"><span data-stu-id="37a4b-153">Uses of these parameters would very likely lead to ambiguities.</span></span> 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="37a4b-154">Generování uživatelského rozhraní MVC nebo webového rozhraní API do projektu s 5.1 výsledky balíčky v 5.0 balíčky pro šablony, které již nejsou v projektu</span><span class="sxs-lookup"><span data-stu-id="37a4b-154">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="37a4b-155">Aktualizace balíčků NuGet pro ASP.NET MVC 5.1 RTM nelze aktualizovat, jako je ASP.NET generování uživatelského rozhraní nástroje sady Visual Studio nebo šablona projektu webové aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="37a4b-155">Updating NuGet packages for ASP.NET MVC 5.1 RTM does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="37a4b-156">Používají předchozí verzi modulu runtime ASP.NET balíčky (5.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="37a4b-156">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="37a4b-157">Generování uživatelského rozhraní ASP.NET v důsledku toho nainstaluje předchozí verzi (5.0.0.0) požadované balíčky, pokud již nejsou k dispozici v projektech.</span><span class="sxs-lookup"><span data-stu-id="37a4b-157">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="37a4b-158">ASP.NET generování uživatelského rozhraní v Visual Studio 2013 RTM nebo Update 1, ale nepřepíše nejnovější balíčků ve vašich projektů.</span><span class="sxs-lookup"><span data-stu-id="37a4b-158">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="37a4b-159">Pokud používáte generování uživatelského rozhraní ASP.NET po aktualizaci balíčků projekty webového rozhraní API 2.1 nebo ASP.NET MVC 5.1, ujistěte se, že verze webového rozhraní API a rozhraní ASP.NET MVC jsou konzistentní.</span><span class="sxs-lookup"><span data-stu-id="37a4b-159">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span> 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a><span data-ttu-id="37a4b-160">Zvýraznění syntaxe Razor zobrazení v sadě Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="37a4b-160">Syntax Highlighting for Razor Views in Visual Studio 2013</span></span>

<span data-ttu-id="37a4b-161">Pokud aktualizujete k ASP.NET MVC 5.1 RTM bez aktualizace Visual Studio 2013, získáte nebude editor Visual Studio – podpora pro zvýraznění syntaxe při úpravách zobrazení syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="37a4b-161">If you update to ASP.NET MVC 5.1 RTM without updating Visual Studio 2013, you will not get Visual Studio editor support for syntax highlighting while editing the Razor views.</span></span> <span data-ttu-id="37a4b-162">Musíte se k aktualizaci Visual Studio 2013 získat tuto podporu.</span><span class="sxs-lookup"><span data-stu-id="37a4b-162">You will need to update Visual Studio 2013 to get this support.</span></span> 

### <a name="type-renames"></a><span data-ttu-id="37a4b-163">Přejmenuje typu</span><span class="sxs-lookup"><span data-stu-id="37a4b-163">Type Renames</span></span>

<span data-ttu-id="37a4b-164">Některé typy používané pro atribut směrování rozšíření jsou přejmenovat v 5.1 RTM.</span><span class="sxs-lookup"><span data-stu-id="37a4b-164">Some of the types used for attribute routing extensibility are renamed in 5.1 RTM.</span></span>

| <span data-ttu-id="37a4b-165">**Původní název typu (5.1 RC)**</span><span class="sxs-lookup"><span data-stu-id="37a4b-165">**Old Type Name (5.1 RC)**</span></span> | <span data-ttu-id="37a4b-166">**Název nového typu (5.1 RTM)**</span><span class="sxs-lookup"><span data-stu-id="37a4b-166">**New Type Name (5.1 RTM)**</span></span> |
| --- | --- |
| <span data-ttu-id="37a4b-167">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="37a4b-167">IDirectRouteProvider</span></span> | <span data-ttu-id="37a4b-168">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="37a4b-168">IDirectRouteFactory</span></span> |
| <span data-ttu-id="37a4b-169">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="37a4b-169">RouteProviderAttribute</span></span> | <span data-ttu-id="37a4b-170">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="37a4b-170">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="37a4b-171">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="37a4b-171">DirectRouteProviderContext</span></span> | <span data-ttu-id="37a4b-172">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="37a4b-172">DirectRouteFactoryContext</span></span> |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="37a4b-173">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="37a4b-173">Bug Fixes</span></span>

<span data-ttu-id="37a4b-174">Tato verze rovněž obsahuje několik oprav chyb.</span><span class="sxs-lookup"><span data-stu-id="37a4b-174">This release also includes several bug fixes.</span></span> <span data-ttu-id="37a4b-175">Úplný seznam najdete:</span><span class="sxs-lookup"><span data-stu-id="37a4b-175">You can find the complete list here:</span></span>

- [<span data-ttu-id="37a4b-176">5.1.0 balíček</span><span class="sxs-lookup"><span data-stu-id="37a4b-176">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="37a4b-177">5.1.1 balíčku</span><span class="sxs-lookup"><span data-stu-id="37a4b-177">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<span data-ttu-id="37a4b-178">5.1.2 balíček obsahuje aktualizace IntelliSense, ale žádné opravy chyb.</span><span class="sxs-lookup"><span data-stu-id="37a4b-178">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
