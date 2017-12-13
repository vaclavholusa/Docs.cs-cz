---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: "Model 2.0 stránky ASP.NET | Microsoft Docs"
author: microsoft
description: "V technologii ASP.NET 1.x, vývojáři měli volba mezi model vloženého kódu a model kódu kódu. Kódu může být implementovaná pomocí buď Line Src..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: e008f197cf08bec81c560018f2d42306598f9e6d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="the-aspnet-20-page-model"></a><span data-ttu-id="51b43-104">Model 2.0 stránky ASP.NET</span><span class="sxs-lookup"><span data-stu-id="51b43-104">The ASP.NET 2.0 Page Model</span></span>
====================
<span data-ttu-id="51b43-105">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="51b43-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="51b43-106">V technologii ASP.NET 1.x, vývojáři měli volba mezi model vloženého kódu a model kódu kódu.</span><span class="sxs-lookup"><span data-stu-id="51b43-106">In ASP.NET 1.x, developers had a choice between an inline code model and a code-behind code model.</span></span> <span data-ttu-id="51b43-107">Kódu může být implementovaná pomocí atribut Src nebo atribut CodeBehind @Page – direktiva.</span><span class="sxs-lookup"><span data-stu-id="51b43-107">Code-behind could be implemented using either the Src attribute or the CodeBehind attribute of the @Page directive.</span></span> <span data-ttu-id="51b43-108">V aplikaci ASP.NET 2.0 vývojáři stále mají volba mezi vloženého kódu a kódu, ale byla významná vylepšení modelu kódu na pozadí.</span><span class="sxs-lookup"><span data-stu-id="51b43-108">In ASP.NET 2.0, developers still have a choice between inline code and code-behind, but there have been significant enhancements to the code-behind model.</span></span>


<span data-ttu-id="51b43-109">V technologii ASP.NET 1.x, vývojáři měli volba mezi model vloženého kódu a model kódu kódu.</span><span class="sxs-lookup"><span data-stu-id="51b43-109">In ASP.NET 1.x, developers had a choice between an inline code model and a code-behind code model.</span></span> <span data-ttu-id="51b43-110">Kódu může být implementovaná pomocí atribut Src nebo atribut CodeBehind @Page – direktiva.</span><span class="sxs-lookup"><span data-stu-id="51b43-110">Code-behind could be implemented using either the Src attribute or the CodeBehind attribute of the @Page directive.</span></span> <span data-ttu-id="51b43-111">V aplikaci ASP.NET 2.0 vývojáři stále mají volba mezi vloženého kódu a kódu, ale byla významná vylepšení modelu kódu na pozadí.</span><span class="sxs-lookup"><span data-stu-id="51b43-111">In ASP.NET 2.0, developers still have a choice between inline code and code-behind, but there have been significant enhancements to the code-behind model.</span></span>

## <a name="improvements-in-the-code-behind-model"></a><span data-ttu-id="51b43-112">Vylepšení v modelu kódu</span><span class="sxs-lookup"><span data-stu-id="51b43-112">Improvements in the Code-Behind Model</span></span>

<span data-ttu-id="51b43-113">Chcete-li plně pochopit změny v modelu kódu v aplikaci ASP.NET 2.0, je nejlepší rychle zkontrolovat modelu jak existovalo v ASP.NET 1.x.</span><span class="sxs-lookup"><span data-stu-id="51b43-113">In order to fully understand the changes in the code-behind model in ASP.NET 2.0, its best to quickly review the model as it existed in ASP.NET 1.x.</span></span>

## <a name="the-code-behind-model-in-aspnet-1x"></a><span data-ttu-id="51b43-114">Model kódu technologie ASP.NET 1.x</span><span class="sxs-lookup"><span data-stu-id="51b43-114">The Code-Behind Model in ASP.NET 1.x</span></span>

<span data-ttu-id="51b43-115">V technologii ASP.NET 1.x, modelu kódu na pozadí se skládal z soubor ASPX (webového formuláře) a souboru kódu, který obsahuje programového kódu.</span><span class="sxs-lookup"><span data-stu-id="51b43-115">In ASP.NET 1.x, the code-behind model consisted of an ASPX file (the Webform) and a code-behind file containing programming code.</span></span> <span data-ttu-id="51b43-116">Dva soubory byly připojené pomocí @Page direktivy v souboru ASPX.</span><span class="sxs-lookup"><span data-stu-id="51b43-116">The two files were connected using the @Page directive in the ASPX file.</span></span> <span data-ttu-id="51b43-117">Každý ovládací prvek na stránce ASPX měl deklaraci odpovídající v souboru kódu na pozadí jako proměnnou instance.</span><span class="sxs-lookup"><span data-stu-id="51b43-117">Each control on the ASPX page had a corresponding declaration in the code-behind file as an instance variable.</span></span> <span data-ttu-id="51b43-118">Soubor kódu také obsahuje kód pro vazbu událostí a generovaného kódu, které jsou nezbytné pro návrháře Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="51b43-118">The code-behind file also contained code for event binding and generated code necessary for the Visual Studio designer.</span></span> <span data-ttu-id="51b43-119">Tento model poměrně osvědčil, ale protože každý ASP.NET element na stránce ASPX vyžaduje odpovídající kód v souboru kódu na pozadí, byl žádné true oddělení kódu a obsahu.</span><span class="sxs-lookup"><span data-stu-id="51b43-119">This model worked fairly well, but because every ASP.NET element in the ASPX page required corresponding code in the code-behind file, there was no true separation of code and content.</span></span> <span data-ttu-id="51b43-120">Například pokud návrháře přidali nový ovládací prvek serveru do souboru ASPX mimo prostředí Visual Studio IDE, aplikace by došlo k přerušení kvůli absenci deklaraci pro ovládací prvek v souboru kódu na pozadí.</span><span class="sxs-lookup"><span data-stu-id="51b43-120">For example, if a designer added a new server control to an ASPX file outside of the Visual Studio IDE, the application would break due to the absence of a declaration for that control in the code-behind file.</span></span>

## <a name="the-code-behind-model-in-aspnet-20"></a><span data-ttu-id="51b43-121">Model kódu v technologii ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="51b43-121">The Code-Behind Model in ASP.NET 2.0</span></span>

<span data-ttu-id="51b43-122">ASP.NET 2.0 se výrazně zlepšuje při tomto modelu.</span><span class="sxs-lookup"><span data-stu-id="51b43-122">ASP.NET 2.0 greatly improves upon this model.</span></span> <span data-ttu-id="51b43-123">V aplikaci ASP.NET 2.0, je kódu implementovaná pomocí nové *částečné třídy* uvedené v technologii ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="51b43-123">In ASP.NET 2.0, code-behind is implemented using the new *partial classes* provided in ASP.NET 2.0.</span></span> <span data-ttu-id="51b43-124">Třída kódu v aplikaci ASP.NET 2.0 je definován jako částečné třídy, což znamená, že obsahuje pouze část definice třídy.</span><span class="sxs-lookup"><span data-stu-id="51b43-124">The code-behind class in ASP.NET 2.0 is definied as a partial class meaning that it contains only part of the class definition.</span></span> <span data-ttu-id="51b43-125">Zbývající část definice třídy dynamicky vygeneruje technologii ASP.NET 2.0 pomocí stránky ASPX za běhu, nebo když je předkompilovaných webu.</span><span class="sxs-lookup"><span data-stu-id="51b43-125">The remaining part of the class definition is dynamically generated by ASP.NET 2.0 using the ASPX page at runtime or when the Web site is precompiled.</span></span> <span data-ttu-id="51b43-126">Propojení mezi souboru kódu na pozadí a stránky ASPX bude vytvořeno pomocí direktivy @ Page.</span><span class="sxs-lookup"><span data-stu-id="51b43-126">The link between the code-behind file and the ASPX page is still established using the @ Page directive.</span></span> <span data-ttu-id="51b43-127">Místo atribut CodeBehind nebo Src, je však technologii ASP.NET 2.0 teď používá atribut CodeFile.</span><span class="sxs-lookup"><span data-stu-id="51b43-127">However, instead of a CodeBehind or Src attribute, ASP.NET 2.0 now uses the CodeFile attribute.</span></span> <span data-ttu-id="51b43-128">Atribut Inherits slouží také k určení název třídy pro stránku.</span><span class="sxs-lookup"><span data-stu-id="51b43-128">The Inherits attribute is also used to specify the class name for the page.</span></span>

<span data-ttu-id="51b43-129">Typické @ Page – direktiva může vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="51b43-129">A typical @ Page directive might look like this:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

<span data-ttu-id="51b43-130">Typické – třída definice v souboru kódu ASP.NET 2.0 může vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="51b43-130">A typical class definition in an ASP.NET 2.0 code-behind file might look like this:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="51b43-131">C# a Visual Basic jsou pouze spravované jazyky, které aktuálně podporují částečné třídy.</span><span class="sxs-lookup"><span data-stu-id="51b43-131">C# and Visual Basic are the only managed languages that currently support partial classes.</span></span> <span data-ttu-id="51b43-132">Vývojáře, kteří používají J# proto nebudou moci používat model kódu v aplikaci ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="51b43-132">Therefore, developers using J# will not be able to use the code-behind model in ASP.NET 2.0.</span></span>


<span data-ttu-id="51b43-133">Nový model vylepšuje model kódu, protože vývojáři teď bude mít soubory kódu, které obsahují pouze kód, který jste vytvořili jejich.</span><span class="sxs-lookup"><span data-stu-id="51b43-133">The new model enhances the code-behind model because developers will now have code files that contain only the code that they have created.</span></span> <span data-ttu-id="51b43-134">Umožňuje také true oddělit kódu a obsahu protože nejsou k dispozici žádné instance deklarace proměnných v souboru kódu na pozadí.</span><span class="sxs-lookup"><span data-stu-id="51b43-134">It also provides for a true separation of code and content because there are no instance variable declarations in the code-behind file.</span></span>

> [!NOTE]
> <span data-ttu-id="51b43-135">Protože třídu pro stránky ASPX je, kde probíhá událost vazby, vývojáře jazyka Visual Basic můžou realizovat zvýšit výkon mírnou pomocí zpracovává klíčové slovo v kódu pro vazbu události.</span><span class="sxs-lookup"><span data-stu-id="51b43-135">Because the partial class for the ASPX page is where event binding takes place, Visual Basic developers can realize a slight performance increase by using the Handles keyword in code-behind to bind events.</span></span> <span data-ttu-id="51b43-136">C# nemá žádné ekvivalentní – klíčové slovo.</span><span class="sxs-lookup"><span data-stu-id="51b43-136">C# has no equivalent keyword.</span></span>


## <a name="new--page-directive-attributes"></a><span data-ttu-id="51b43-137">Nové atributy @ Page – direktiva</span><span class="sxs-lookup"><span data-stu-id="51b43-137">New @ Page Directive Attributes</span></span>

<span data-ttu-id="51b43-138">ASP.NET 2.0 přidá nové atributy mnoho @ Page – direktiva.</span><span class="sxs-lookup"><span data-stu-id="51b43-138">ASP.NET 2.0 adds many new attributes to the @ Page directive.</span></span> <span data-ttu-id="51b43-139">Následující atributy jsou v technologii ASP.NET 2.0 nové.</span><span class="sxs-lookup"><span data-stu-id="51b43-139">The following attributes are new in ASP.NET 2.0.</span></span>

## <a name="async"></a><span data-ttu-id="51b43-140">Async</span><span class="sxs-lookup"><span data-stu-id="51b43-140">Async</span></span>

<span data-ttu-id="51b43-141">Atribut asynchronní umožňuje nakonfigurovat stránky se spustí asynchronně.</span><span class="sxs-lookup"><span data-stu-id="51b43-141">The Async attribute allows you to configure page to be executed asynchronously.</span></span> <span data-ttu-id="51b43-142">Dobře titulní asynchronní stránky později v tomto modulu.</span><span class="sxs-lookup"><span data-stu-id="51b43-142">Well cover asynchronous pages later in this module.</span></span>

## <a name="asynctimeout"></a><span data-ttu-id="51b43-143">Hodnota vlastnosti AsyncTimeout</span><span class="sxs-lookup"><span data-stu-id="51b43-143">AsyncTimeout</span></span>

<span data-ttu-id="51b43-144">Zadaný časový limit pro asynchronní stránky.</span><span class="sxs-lookup"><span data-stu-id="51b43-144">Specified the timeout for asynchronous pages.</span></span> <span data-ttu-id="51b43-145">Výchozí hodnota je 45 sekund.</span><span class="sxs-lookup"><span data-stu-id="51b43-145">The default is 45 seconds.</span></span>

## <a name="codefile"></a><span data-ttu-id="51b43-146">CodeFile</span><span class="sxs-lookup"><span data-stu-id="51b43-146">CodeFile</span></span>

<span data-ttu-id="51b43-147">Atribut CodeFile je náhradou atribut CodeBehind v Visual Studio 2002 nebo 2003.</span><span class="sxs-lookup"><span data-stu-id="51b43-147">The CodeFile attribute is the replacement for the CodeBehind attribute in Visual Studio 2002/2003.</span></span>

### <a name="codefilebaseclass"></a><span data-ttu-id="51b43-148">CodeFileBaseClass</span><span class="sxs-lookup"><span data-stu-id="51b43-148">CodeFileBaseClass</span></span>

<span data-ttu-id="51b43-149">Atribut CodeFileBaseClass se používá v případech, kde je potřeba více stránek k odvozování z jedné základní třídy.</span><span class="sxs-lookup"><span data-stu-id="51b43-149">The CodeFileBaseClass attribute is used in cases where you want multiple pages to derive from a single base class.</span></span> <span data-ttu-id="51b43-150">Z důvodu provedení částečné třídy v technologii ASP.NET, aniž by tento atribut základní třídu, která používá sdílené běžné pole tak, aby odkazovaly ovládací prvky deklarované v stránky ASPX nebude správně fungovat protože ASP. Modul kompilace sítěmi automaticky vytvoří nové členy na základě na ovládací prvky na stránce.</span><span class="sxs-lookup"><span data-stu-id="51b43-150">Because of the implementation of partial classes in ASP.NET, without this attribute, a base class that uses shared common fields to reference controls declared in an ASPX page would not work properly because ASP.NETs compilation engine will automatically create new members based on controls in the page.</span></span> <span data-ttu-id="51b43-151">Proto, pokud chcete obecná základní třída pro dva nebo více stránek technologie ASP.NET, je nutné definovat zadejte základní třídu v atributu CodeFileBaseClass a pak každá třída stránky odvozena od základní třídy.</span><span class="sxs-lookup"><span data-stu-id="51b43-151">Therefore, if you want a common base class for two or more pages in ASP.NET, you will need to define specify your base class in the CodeFileBaseClass attribute and then derive each pages class from that base class.</span></span> <span data-ttu-id="51b43-152">Atribut CodeFile je také nutný, pokud tento atribut slouží.</span><span class="sxs-lookup"><span data-stu-id="51b43-152">The CodeFile attribute is also required when this attribute is used.</span></span>

## <a name="compilationmode"></a><span data-ttu-id="51b43-153">compilationMode</span><span class="sxs-lookup"><span data-stu-id="51b43-153">CompilationMode</span></span>

<span data-ttu-id="51b43-154">Tento atribut umožňuje nastavit vlastnost CompilationMode stránky ASPX.</span><span class="sxs-lookup"><span data-stu-id="51b43-154">This attribute allows you to set the CompilationMode property of the ASPX page.</span></span> <span data-ttu-id="51b43-155">Vlastnost CompilationMode je výčet obsahující hodnoty **vždy**, **automaticky**, a **nikdy**.</span><span class="sxs-lookup"><span data-stu-id="51b43-155">The CompilationMode property is an enumeration containing the values **Always**, **Auto**, and **Never**.</span></span> <span data-ttu-id="51b43-156">Výchozí hodnota je **vždy**.</span><span class="sxs-lookup"><span data-stu-id="51b43-156">The default is **Always**.</span></span> <span data-ttu-id="51b43-157">**Automaticky** nastavení zabrání ASP.NET Dynamická kompilace stránky, pokud je to možné.</span><span class="sxs-lookup"><span data-stu-id="51b43-157">The **Auto** setting will prevent ASP.NET from dynamically compiling the page if possible.</span></span> <span data-ttu-id="51b43-158">Vyloučení stránky z dynamická kompilace zvyšuje výkon.</span><span class="sxs-lookup"><span data-stu-id="51b43-158">Excluding pages from dynamic compilation increases performance.</span></span> <span data-ttu-id="51b43-159">Ale pokud na stránce, která je vyloučená obsahuje tento kód, který musí být zkompilovány, chybu bude vyvolána při zobrazení stránky v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="51b43-159">However, if a page that is excluded contains that code that must be compiled, an error will be thrown when the page is browsed.</span></span>

## <a name="enableeventvalidation"></a><span data-ttu-id="51b43-160">EnableEventValidation</span><span class="sxs-lookup"><span data-stu-id="51b43-160">EnableEventValidation</span></span>

<span data-ttu-id="51b43-161">Tento atribut určuje, zda se ověřují zpětné volání a zpětného volání události.</span><span class="sxs-lookup"><span data-stu-id="51b43-161">This attribute specifies whether or not postback and callback events are validated.</span></span> <span data-ttu-id="51b43-162">Pokud je tato možnost povolena, argumenty odeslání nebo zpětného volání události se kontroluje, ujistěte se, že pochází z ovládacího prvku serveru, který původně je vykreslen.</span><span class="sxs-lookup"><span data-stu-id="51b43-162">When this is enabled, arguments to postback or callback events are checked to ensure that they originated from the server control that originally rendered them.</span></span>

## <a name="enabletheming"></a><span data-ttu-id="51b43-163">EnableTheming</span><span class="sxs-lookup"><span data-stu-id="51b43-163">EnableTheming</span></span>

<span data-ttu-id="51b43-164">Tento atribut určuje, zda jsou na stránce používá motivy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="51b43-164">This attribute specifies whether or not ASP.NET themes are used on a page.</span></span> <span data-ttu-id="51b43-165">Výchozí hodnota je **false**.</span><span class="sxs-lookup"><span data-stu-id="51b43-165">The default is **false**.</span></span> <span data-ttu-id="51b43-166">ASP.NET motivy jsou popsané v [modulu 10](profiles-themes-and-web-parts.md).</span><span class="sxs-lookup"><span data-stu-id="51b43-166">ASP.NET themes are covered in [Module 10](profiles-themes-and-web-parts.md).</span></span>

## <a name="linepragmas"></a><span data-ttu-id="51b43-167">LinePragmas</span><span class="sxs-lookup"><span data-stu-id="51b43-167">LinePragmas</span></span>

<span data-ttu-id="51b43-168">Tento atribut určuje, zda direktivy řádku by měl být přidán během kompilace.</span><span class="sxs-lookup"><span data-stu-id="51b43-168">This attribute specifies whether line pragmas should be added during compilation.</span></span> <span data-ttu-id="51b43-169">Direktivy řádku jsou možnosti používané ladicí programy označit určité části kódu.</span><span class="sxs-lookup"><span data-stu-id="51b43-169">Line pragmas are options used by debuggers to mark specific sections of code.</span></span>

## <a name="maintainscrollpositiononpostback"></a><span data-ttu-id="51b43-170">MaintainScrollPositionOnPostback</span><span class="sxs-lookup"><span data-stu-id="51b43-170">MaintainScrollPositionOnPostback</span></span>

<span data-ttu-id="51b43-171">Tento atribut určuje, zda je JavaScript vloženy do stránce kvůli zachování pozici posunutí mezi postback.</span><span class="sxs-lookup"><span data-stu-id="51b43-171">This attribute specifies whether or not JavaScript is injected into the page in order to maintain scroll position between postbacks.</span></span> <span data-ttu-id="51b43-172">Tento atribut je **false** ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="51b43-172">This attribute is **false** by default.</span></span>

<span data-ttu-id="51b43-173">Když tento atribut je **true**, přidá ASP.NET &lt;skriptu&gt; bloku na zpětné volání, které vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="51b43-173">When this attribute is **true**, ASP.NET will add a &lt;script&gt; block on postback that looks like this:</span></span>

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

<span data-ttu-id="51b43-174">Všimněte si, že blokovat src pro tento skript je WebResource.axd.</span><span class="sxs-lookup"><span data-stu-id="51b43-174">Note that the src for this script block is WebResource.axd.</span></span> <span data-ttu-id="51b43-175">Tento prostředek není na fyzickou cestu.</span><span class="sxs-lookup"><span data-stu-id="51b43-175">This resource is not a physical path.</span></span> <span data-ttu-id="51b43-176">Pokud se požaduje tento skript, ASP.NET dynamicky vytvoří skript.</span><span class="sxs-lookup"><span data-stu-id="51b43-176">When this script is requested, ASP.NET dynamically builds the script.</span></span>

### <a name="masterpagefile"></a><span data-ttu-id="51b43-177">MasterPageFile</span><span class="sxs-lookup"><span data-stu-id="51b43-177">MasterPageFile</span></span>

<span data-ttu-id="51b43-178">Tento atribut určuje soubor předlohové stránky pro aktuální stránku.</span><span class="sxs-lookup"><span data-stu-id="51b43-178">This attribute specifies the master page file for the current page.</span></span> <span data-ttu-id="51b43-179">Cesta může být relativní nebo absolutní.</span><span class="sxs-lookup"><span data-stu-id="51b43-179">The path can be relative or absolute.</span></span> <span data-ttu-id="51b43-180">Hlavní stránky jsou popsané v [modulu 4](master-pages.md).</span><span class="sxs-lookup"><span data-stu-id="51b43-180">Master pages are covered in [Module 4](master-pages.md).</span></span>

## <a name="stylesheettheme"></a><span data-ttu-id="51b43-181">StyleSheetTheme</span><span class="sxs-lookup"><span data-stu-id="51b43-181">StyleSheetTheme</span></span>

<span data-ttu-id="51b43-182">Tento atribut umožňuje potlačit vlastnosti vzhled uživatelského rozhraní definované motiv technologie ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="51b43-182">This attribute allows you to override user-interface appearance properties defined by an ASP.NET 2.0 theme.</span></span> <span data-ttu-id="51b43-183">Motivy jsou popsané v [modulu 10](profiles-themes-and-web-parts.md).</span><span class="sxs-lookup"><span data-stu-id="51b43-183">Themes are covered in [Module 10](profiles-themes-and-web-parts.md).</span></span>

## <a name="theme"></a><span data-ttu-id="51b43-184">Motiv</span><span class="sxs-lookup"><span data-stu-id="51b43-184">Theme</span></span>

<span data-ttu-id="51b43-185">Určuje motivu pro stránku.</span><span class="sxs-lookup"><span data-stu-id="51b43-185">Specifies the theme for the page.</span></span> <span data-ttu-id="51b43-186">Pokud není zadaná hodnota pro atribut StyleSheetTheme atributu motivu přepíše všechny styly aplikovat na ovládací prvky na stránce.</span><span class="sxs-lookup"><span data-stu-id="51b43-186">If a value is not specified for the StyleSheetTheme attribute, the Theme attribute overrides all styles applied to controls on the page.</span></span>

## <a name="title"></a><span data-ttu-id="51b43-187">Název</span><span class="sxs-lookup"><span data-stu-id="51b43-187">Title</span></span>

<span data-ttu-id="51b43-188">Nastaví název webové stránky.</span><span class="sxs-lookup"><span data-stu-id="51b43-188">Sets the title for the page.</span></span> <span data-ttu-id="51b43-189">Hodnota zadaná v tomto poli se zobrazí v &lt;název&gt; element vykreslené stránky.</span><span class="sxs-lookup"><span data-stu-id="51b43-189">The value specified here will appear in the &lt;title&gt; element of the rendered page.</span></span>

### <a name="viewstateencryptionmode"></a><span data-ttu-id="51b43-190">ViewStateEncryptionMode</span><span class="sxs-lookup"><span data-stu-id="51b43-190">ViewStateEncryptionMode</span></span>

<span data-ttu-id="51b43-191">Nastaví hodnotu pro ViewStateEncryptionMode výčtu.</span><span class="sxs-lookup"><span data-stu-id="51b43-191">Sets the value for the ViewStateEncryptionMode enumeration.</span></span> <span data-ttu-id="51b43-192">Dostupné hodnoty jsou **vždy**, **automaticky**, a **nikdy**.</span><span class="sxs-lookup"><span data-stu-id="51b43-192">The available values are **Always**, **Auto**, and **Never**.</span></span> <span data-ttu-id="51b43-193">Výchozí hodnota je **automaticky**. Pokud je tento atribut nastavíte na hodnotu **automaticky**, se šifrují viewstate je ovládací prvek požaduje voláním **RegisterRequiresViewStateEncryption** metoda.</span><span class="sxs-lookup"><span data-stu-id="51b43-193">The default value is **Auto**. When this attribute is set to a value of **Auto**, viewstate is encrypted is a control requests it by calling the **RegisterRequiresViewStateEncryption** method.</span></span>

## <a name="setting-public-property-values-via-the--page-directive"></a><span data-ttu-id="51b43-194">Nastavení hodnot veřejná vlastnost prostřednictvím @ Page – direktiva</span><span class="sxs-lookup"><span data-stu-id="51b43-194">Setting Public Property Values via the @ Page Directive</span></span>

<span data-ttu-id="51b43-195">Další nová funkce @ Page – direktiva v technologii ASP.NET 2.0 je možnost nastavit počáteční hodnotu veřejné vlastnosti základní třídy.</span><span class="sxs-lookup"><span data-stu-id="51b43-195">Another new capability of the @ Page directive in ASP.NET 2.0 is the ability to set the initial value of public properties of a base class.</span></span> <span data-ttu-id="51b43-196">Předpokládejme například, že máte veřejná vlastnost názvem **SomeText** ve základní třídy a d jako ho k inicializována tak, aby **Hello** při načtení stránky.</span><span class="sxs-lookup"><span data-stu-id="51b43-196">Suppose, for example, that you have a public property called **SomeText** in your base class and you d like it to be initialized to **Hello** when a page is loaded.</span></span> <span data-ttu-id="51b43-197">Můžete to provést pomocí jednoduše nastavením hodnoty v direktivě @ Page takto:</span><span class="sxs-lookup"><span data-stu-id="51b43-197">You can accomplish this by simply setting the value in the @ Page directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

<span data-ttu-id="51b43-198">**SomeText** atribut @ Page – direktiva nastaví počáteční hodnotu vlastnosti SomeText v základní třídě a *Hello!*.</span><span class="sxs-lookup"><span data-stu-id="51b43-198">The **SomeText** attribute of the @ Page directive sets the initial value of the SomeText property in the base class to *Hello!*.</span></span> <span data-ttu-id="51b43-199">Níže je návod, nastavení počáteční hodnota veřejné vlastnosti v základní třídě using – direktiva @ stránky.</span><span class="sxs-lookup"><span data-stu-id="51b43-199">The video below is a walkthrough of setting the initial value of a public property in a base class using the @ Page directive.</span></span>


![](the-asp-net-2-0-page-model/_static/image1.png)


[<span data-ttu-id="51b43-200">Otevřete Video přes celou obrazovku</span><span class="sxs-lookup"><span data-stu-id="51b43-200">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/setprop1.wmv)


## <a name="new-public-properties-of-the-page-class"></a><span data-ttu-id="51b43-201">Nové veřejné vlastnosti třídy stránky</span><span class="sxs-lookup"><span data-stu-id="51b43-201">New Public Properties of the Page Class</span></span>

<span data-ttu-id="51b43-202">Následující veřejné vlastnosti jsou v technologii ASP.NET 2.0 nové.</span><span class="sxs-lookup"><span data-stu-id="51b43-202">The following public properties are new in ASP.NET 2.0.</span></span>

## <a name="apprelativetemplatesourcedirectory"></a><span data-ttu-id="51b43-203">AppRelativeTemplateSourceDirectory</span><span class="sxs-lookup"><span data-stu-id="51b43-203">AppRelativeTemplateSourceDirectory</span></span>

<span data-ttu-id="51b43-204">Vrací aplikace relativní cestu k stránka nebo ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="51b43-204">Returns the application-relative path to the page or control.</span></span> <span data-ttu-id="51b43-205">Například pro stránku, který se nachází na http://app/folder/page.aspx, vlastnost vrací ~ / složky /.</span><span class="sxs-lookup"><span data-stu-id="51b43-205">For example, for a page located at http://app/folder/page.aspx, the property returns ~/folder/.</span></span>

## <a name="apprelativevirtualpath"></a><span data-ttu-id="51b43-206">AppRelativeVirtualPath</span><span class="sxs-lookup"><span data-stu-id="51b43-206">AppRelativeVirtualPath</span></span>

<span data-ttu-id="51b43-207">Vrátí cesta relativní virtuálního adresáře na stránce nebo ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="51b43-207">Returns the relative virtual directory path to the page or control.</span></span> <span data-ttu-id="51b43-208">Například pro stránku, který se nachází na http://app/folder/page.aspx, vlastnost vrací ~ / folder/page.aspx.</span><span class="sxs-lookup"><span data-stu-id="51b43-208">For example for a page located at http://app/folder/page.aspx, the property returns ~/folder/page.aspx.</span></span>

## <a name="asynctimeout"></a><span data-ttu-id="51b43-209">Hodnota vlastnosti AsyncTimeout</span><span class="sxs-lookup"><span data-stu-id="51b43-209">AsyncTimeout</span></span>

<span data-ttu-id="51b43-210">Získá nebo nastaví časový limit použitý pro zpracování asynchronní stránky.</span><span class="sxs-lookup"><span data-stu-id="51b43-210">Gets or sets the timeout used for asynchronous page handling.</span></span> <span data-ttu-id="51b43-211">(Asynchronní stránky bude zmínka v dalších částech tohoto modulu.)</span><span class="sxs-lookup"><span data-stu-id="51b43-211">(Asynchronous pages will be covered later in this module.)</span></span>

## <a name="clientquerystring"></a><span data-ttu-id="51b43-212">ClientQueryString</span><span class="sxs-lookup"><span data-stu-id="51b43-212">ClientQueryString</span></span>

<span data-ttu-id="51b43-213">Vlastnosti jen pro čtení, která vrátí část řetězce dotazu požadovanou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="51b43-213">A read-only property that returns the query string portion of the requested URL.</span></span> <span data-ttu-id="51b43-214">Tato hodnota je kódovaná adresou URL.</span><span class="sxs-lookup"><span data-stu-id="51b43-214">This value is URL encoded.</span></span> <span data-ttu-id="51b43-215">Metoda UrlDecode třídy HttpServerUtility můžete dekódovat.</span><span class="sxs-lookup"><span data-stu-id="51b43-215">You can use the UrlDecode method of the HttpServerUtility class to decode it.</span></span>

## <a name="clientscript"></a><span data-ttu-id="51b43-216">ClientScript</span><span class="sxs-lookup"><span data-stu-id="51b43-216">ClientScript</span></span>

<span data-ttu-id="51b43-217">Tato vlastnost vrátí ClientScriptManager objekt, který můžete použít ke správě ASP.NETs emisí klientský skript.</span><span class="sxs-lookup"><span data-stu-id="51b43-217">This property returns a ClientScriptManager object that can be used to manage ASP.NETs emission of client-side script.</span></span> <span data-ttu-id="51b43-218">(Třída ClientScriptManager je zmínka v dalších částech tohoto modulu.)</span><span class="sxs-lookup"><span data-stu-id="51b43-218">(The ClientScriptManager class is covered later in this module.)</span></span>

## <a name="enableeventvalidation"></a><span data-ttu-id="51b43-219">EnableEventValidation</span><span class="sxs-lookup"><span data-stu-id="51b43-219">EnableEventValidation</span></span>

<span data-ttu-id="51b43-220">Tato vlastnost určuje, zda je povoleno ověření události pro události se zpětné volání a zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="51b43-220">This property controls whether or not event validation is enabled for postback and callback events.</span></span> <span data-ttu-id="51b43-221">Když je povolené, je zajistit, že pochází z ovládacího prvku serveru, který původně je vykreslen ověřeno argumentů odeslání nebo události zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="51b43-221">When enabled, arguments to postback or callback events are verified to ensure that they originated from the server control that originally rendered them.</span></span>

## <a name="enabletheming"></a><span data-ttu-id="51b43-222">EnableTheming</span><span class="sxs-lookup"><span data-stu-id="51b43-222">EnableTheming</span></span>

<span data-ttu-id="51b43-223">Tato vlastnost získá nebo nastaví logická hodnota, která určuje, zda motiv technologie ASP.NET 2.0 se vztahuje na stránku.</span><span class="sxs-lookup"><span data-stu-id="51b43-223">This property gets or sets a Boolean that specifies whether or not an ASP.NET 2.0 theme applies to the page.</span></span>

## <a name="form"></a><span data-ttu-id="51b43-224">Formulář</span><span class="sxs-lookup"><span data-stu-id="51b43-224">Form</span></span>

<span data-ttu-id="51b43-225">Tato vlastnost vrátí formuláře HTML na stránce ASPX jako objekt HtmlForm.</span><span class="sxs-lookup"><span data-stu-id="51b43-225">This property returns the HTML form on the ASPX page as an HtmlForm object.</span></span>

## <a name="header"></a><span data-ttu-id="51b43-226">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="51b43-226">Header</span></span>

<span data-ttu-id="51b43-227">Tato vlastnost vrátí odkaz na objekt HtmlHead, který obsahuje záhlaví stránky.</span><span class="sxs-lookup"><span data-stu-id="51b43-227">This property returns a reference to an HtmlHead object that contains the page header.</span></span> <span data-ttu-id="51b43-228">Vrácený objekt HtmlHead můžete použít k získání nebo nastavení stylů, značky Meta atd.</span><span class="sxs-lookup"><span data-stu-id="51b43-228">You can use the returned HtmlHead object to get/set style sheets, Meta tags, etc.</span></span>

## <a name="idseparator"></a><span data-ttu-id="51b43-229">IdSeparator</span><span class="sxs-lookup"><span data-stu-id="51b43-229">IdSeparator</span></span>

<span data-ttu-id="51b43-230">Tato vlastnost jen pro čtení získá znak, který slouží k oddělení řízení identifikátory při ASP.NET je sestavování jedinečné ID pro ovládací prvky na stránce.</span><span class="sxs-lookup"><span data-stu-id="51b43-230">This read-only property gets the character that is used to separate control identifiers when ASP.NET is building a unique ID for controls on a page.</span></span> <span data-ttu-id="51b43-231">Rozhraní není určeno pro použití přímo z vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="51b43-231">It is not intended to be used directly from your code.</span></span>

## <a name="isasync"></a><span data-ttu-id="51b43-232">IsAsync</span><span class="sxs-lookup"><span data-stu-id="51b43-232">IsAsync</span></span>

<span data-ttu-id="51b43-233">Tato vlastnost umožňuje pro asynchronní stránky.</span><span class="sxs-lookup"><span data-stu-id="51b43-233">This property allows for asynchronous pages.</span></span> <span data-ttu-id="51b43-234">Asynchronní stránky jsou popsány dále v tomto modulu.</span><span class="sxs-lookup"><span data-stu-id="51b43-234">Asynchronous pages are discussed later in this module.</span></span>

## <a name="iscallback"></a><span data-ttu-id="51b43-235">IsCallback</span><span class="sxs-lookup"><span data-stu-id="51b43-235">IsCallback</span></span>

<span data-ttu-id="51b43-236">Tato vlastnost jen pro čtení vrací **true** Pokud stránky je výsledkem volání zpět.</span><span class="sxs-lookup"><span data-stu-id="51b43-236">This read-only property returns **true** if the page is the result of a call back.</span></span> <span data-ttu-id="51b43-237">Zpětnými voláními jsou popsány dále v tomto modulu.</span><span class="sxs-lookup"><span data-stu-id="51b43-237">Call backs are discussed later in this module.</span></span>

## <a name="iscrosspagepostback"></a><span data-ttu-id="51b43-238">IsCrossPagePostBack</span><span class="sxs-lookup"><span data-stu-id="51b43-238">IsCrossPagePostBack</span></span>

<span data-ttu-id="51b43-239">Tato vlastnost jen pro čtení vrací **true** Pokud stránky je součástí zpětné volání mezi stránkami.</span><span class="sxs-lookup"><span data-stu-id="51b43-239">This read-only property returns **true** if the page is part of a cross-page postback.</span></span> <span data-ttu-id="51b43-240">Mezi stránkami postback jsou popsané dále v tomto modulu.</span><span class="sxs-lookup"><span data-stu-id="51b43-240">Cross-page postbacks are covered later in this module.</span></span>

## <a name="items"></a><span data-ttu-id="51b43-241">Položky</span><span class="sxs-lookup"><span data-stu-id="51b43-241">Items</span></span>

<span data-ttu-id="51b43-242">Vrátí odkaz na instanci IDictionary, který obsahuje všechny objekty, které jsou uložené v kontextu stránky.</span><span class="sxs-lookup"><span data-stu-id="51b43-242">Returns a reference to an IDictionary instance that contains all objects stored in the pages context.</span></span> <span data-ttu-id="51b43-243">Položky můžete přidat na tento objekt IDictionary a bude k dispozici po celou dobu kontextu.</span><span class="sxs-lookup"><span data-stu-id="51b43-243">You can add items to this IDictionary object and they will be available to you throughout the lifetime of the context.</span></span>

## <a name="maintainscrollpositiononpostback"></a><span data-ttu-id="51b43-244">MaintainScrollPositionOnPostBack</span><span class="sxs-lookup"><span data-stu-id="51b43-244">MaintainScrollPositionOnPostBack</span></span>

<span data-ttu-id="51b43-245">Tato vlastnost určuje, zda technologie ASP.NET vysílá JavaScript, která zajišťuje, že na stránkách posuňte pozici v prohlížeči se potom, co dojde zpětné volání.</span><span class="sxs-lookup"><span data-stu-id="51b43-245">This property controls whether or not ASP.NET emits JavaScript that maintains the pages scroll position in the browser after a postback occurs.</span></span> <span data-ttu-id="51b43-246">(Podrobnosti o této vlastnosti byly popsané dříve v tomto modulu.)</span><span class="sxs-lookup"><span data-stu-id="51b43-246">(Details of this property were discussed earlier in this module.)</span></span>

## <a name="master"></a><span data-ttu-id="51b43-247">hlavní server</span><span class="sxs-lookup"><span data-stu-id="51b43-247">Master</span></span>

<span data-ttu-id="51b43-248">Tato vlastnost jen pro čtení vrátí odkaz na instanci MasterPage pro stránku, do které byl použit stránky předlohy.</span><span class="sxs-lookup"><span data-stu-id="51b43-248">This read-only property returns a reference to the MasterPage instance for a page to which a master page has been applied.</span></span>

## <a name="masterpagefile"></a><span data-ttu-id="51b43-249">MasterPageFile</span><span class="sxs-lookup"><span data-stu-id="51b43-249">MasterPageFile</span></span>

<span data-ttu-id="51b43-250">Získá nebo nastaví název souboru stránky předlohy pro stránku.</span><span class="sxs-lookup"><span data-stu-id="51b43-250">Gets or sets the master page filename for the page.</span></span> <span data-ttu-id="51b43-251">Tuto vlastnost lze nastavit pouze v metodě PreInit.</span><span class="sxs-lookup"><span data-stu-id="51b43-251">This property can only be set in the PreInit method.</span></span>

## <a name="maxpagestatefieldlength"></a><span data-ttu-id="51b43-252">MaxPageStateFieldLength</span><span class="sxs-lookup"><span data-stu-id="51b43-252">MaxPageStateFieldLength</span></span>

<span data-ttu-id="51b43-253">Tato vlastnost získá nebo nastaví maximální délku stav stránky v bajtech.</span><span class="sxs-lookup"><span data-stu-id="51b43-253">This property gets or sets the maximum length for the pages state in bytes.</span></span> <span data-ttu-id="51b43-254">Pokud je vlastnost nastavena na kladné číslo, stav zobrazení stránky bude rozdělit do několika skrytá pole nemá být vyšší než počet bajtů určený.</span><span class="sxs-lookup"><span data-stu-id="51b43-254">If the property is set to a positive number, the pages view state will be broken up into multiple hidden fields so that it doesnt exceed the number of bytes specified.</span></span> <span data-ttu-id="51b43-255">Pokud je vlastnost nastavena na záporné číslo, nebude se stav zobrazení rozdělen do bloků.</span><span class="sxs-lookup"><span data-stu-id="51b43-255">If the property is a negative number, the view state will not be broken into chunks.</span></span>

## <a name="pageadapter"></a><span data-ttu-id="51b43-256">PageAdapter</span><span class="sxs-lookup"><span data-stu-id="51b43-256">PageAdapter</span></span>

<span data-ttu-id="51b43-257">Vrátí odkaz na objekt PageAdapter, která upraví na stránce pro prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="51b43-257">Returns a reference to the PageAdapter object that modifies the page for the requesting browser.</span></span>

## <a name="previouspage"></a><span data-ttu-id="51b43-258">PreviousPage</span><span class="sxs-lookup"><span data-stu-id="51b43-258">PreviousPage</span></span>

<span data-ttu-id="51b43-259">Vrátí odkaz na předchozí stránku v případě Server.Transfer nebo zpětné volání mezi stránkami.</span><span class="sxs-lookup"><span data-stu-id="51b43-259">Returns a reference to the previous page in cases of a Server.Transfer or a cross-page postback.</span></span>

## <a name="skinid"></a><span data-ttu-id="51b43-260">SkinID</span><span class="sxs-lookup"><span data-stu-id="51b43-260">SkinID</span></span>

<span data-ttu-id="51b43-261">Určuje vzhledu technologii ASP.NET 2.0 pro použití na stránku.</span><span class="sxs-lookup"><span data-stu-id="51b43-261">Specifies the ASP.NET 2.0 skin to apply to the page.</span></span>

## <a name="stylesheettheme"></a><span data-ttu-id="51b43-262">StyleSheetTheme</span><span class="sxs-lookup"><span data-stu-id="51b43-262">StyleSheetTheme</span></span>

<span data-ttu-id="51b43-263">Tato vlastnost získá nebo nastaví šablony stylů, který se použije na stránku.</span><span class="sxs-lookup"><span data-stu-id="51b43-263">This property gets or sets the style sheet that is applied to a page.</span></span>

## <a name="templatecontrol"></a><span data-ttu-id="51b43-264">Třída TemplateControl</span><span class="sxs-lookup"><span data-stu-id="51b43-264">TemplateControl</span></span>

<span data-ttu-id="51b43-265">Vrátí odkaz na nadřazeného ovládacího prvku pro stránku.</span><span class="sxs-lookup"><span data-stu-id="51b43-265">Returns a reference to the containing control for the page.</span></span>

## <a name="theme"></a><span data-ttu-id="51b43-266">Motiv</span><span class="sxs-lookup"><span data-stu-id="51b43-266">Theme</span></span>

<span data-ttu-id="51b43-267">Získá nebo nastaví název motivu technologii ASP.NET 2.0, použité pro stránku.</span><span class="sxs-lookup"><span data-stu-id="51b43-267">Gets or sets the name of the ASP.NET 2.0 theme applied to the page.</span></span> <span data-ttu-id="51b43-268">Tato hodnota musí být nastavena dříve než metoda PreInit.</span><span class="sxs-lookup"><span data-stu-id="51b43-268">This value must be set prior to the PreInit method.</span></span>

## <a name="title"></a><span data-ttu-id="51b43-269">Název</span><span class="sxs-lookup"><span data-stu-id="51b43-269">Title</span></span>

<span data-ttu-id="51b43-270">Tato vlastnost získá nebo nastaví název pro stránku získaný záhlaví stránky.</span><span class="sxs-lookup"><span data-stu-id="51b43-270">This property gets or sets the title for the page as obtained from the pages header.</span></span>

## <a name="viewstateencryptionmode"></a><span data-ttu-id="51b43-271">ViewStateEncryptionMode</span><span class="sxs-lookup"><span data-stu-id="51b43-271">ViewStateEncryptionMode</span></span>

<span data-ttu-id="51b43-272">Získá nebo nastaví ViewStateEncryptionMode stránky.</span><span class="sxs-lookup"><span data-stu-id="51b43-272">Gets or sets the ViewStateEncryptionMode of the page.</span></span> <span data-ttu-id="51b43-273">Najdete podrobnou diskuzi o tuto vlastnost dříve v tomto modulu.</span><span class="sxs-lookup"><span data-stu-id="51b43-273">See a detailed discussion of this property earlier in this module.</span></span>

## <a name="new-protected-properties-of-the-page-class"></a><span data-ttu-id="51b43-274">Nové vlastnosti protokolu Protected třídy stránky</span><span class="sxs-lookup"><span data-stu-id="51b43-274">New Protected Properties of the Page Class</span></span>

<span data-ttu-id="51b43-275">Níže jsou nové chráněné vlastnosti třídy stránky v aplikaci ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="51b43-275">The following are the new protected properties of the Page class in ASP.NET 2.0.</span></span>

## <a name="adapter"></a><span data-ttu-id="51b43-276">Adaptér</span><span class="sxs-lookup"><span data-stu-id="51b43-276">Adapter</span></span>

<span data-ttu-id="51b43-277">Vrátí odkaz na ControlAdapter vykreslující danou stránku na zařízení, která je požadována.</span><span class="sxs-lookup"><span data-stu-id="51b43-277">Returns a reference to the ControlAdapter that renders the page on the device that requested it.</span></span>

## <a name="asyncmode"></a><span data-ttu-id="51b43-278">AsyncMode</span><span class="sxs-lookup"><span data-stu-id="51b43-278">AsyncMode</span></span>

<span data-ttu-id="51b43-279">Tato vlastnost určuje, zda asynchronně zpracování stránky.</span><span class="sxs-lookup"><span data-stu-id="51b43-279">This property indicates whether or not the page is processed asynchronously.</span></span> <span data-ttu-id="51b43-280">Je určený pro použití modulem runtime a ne přímo v kódu.</span><span class="sxs-lookup"><span data-stu-id="51b43-280">It is intended for use by the runtime and not directly in code.</span></span>

## <a name="clientidseparator"></a><span data-ttu-id="51b43-281">ClientIDSeparator</span><span class="sxs-lookup"><span data-stu-id="51b43-281">ClientIDSeparator</span></span>

<span data-ttu-id="51b43-282">Tato vlastnost vrátí znak používá jako oddělovač při vytváření klienta jedinečné ID pro ovládací prvky.</span><span class="sxs-lookup"><span data-stu-id="51b43-282">This property returns the character used as a separator when creating unique client IDs for controls.</span></span> <span data-ttu-id="51b43-283">Je určený pro použití modulem runtime a ne přímo v kódu.</span><span class="sxs-lookup"><span data-stu-id="51b43-283">It is intended for use by the runtime and not directly in code.</span></span>

## <a name="pagestatepersister"></a><span data-ttu-id="51b43-284">PageStatePersister</span><span class="sxs-lookup"><span data-stu-id="51b43-284">PageStatePersister</span></span>

<span data-ttu-id="51b43-285">Tato vlastnost vrací objekt PageStatePersister pro stránku.</span><span class="sxs-lookup"><span data-stu-id="51b43-285">This property returns the PageStatePersister object for the page.</span></span> <span data-ttu-id="51b43-286">Tato vlastnost slouží především vývojáři ovládací prvek ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="51b43-286">This property is primarily used by ASP.NET control developers.</span></span>

## <a name="uniquefilepathsuffix"></a><span data-ttu-id="51b43-287">UniqueFilePathSuffix</span><span class="sxs-lookup"><span data-stu-id="51b43-287">UniqueFilePathSuffix</span></span>

<span data-ttu-id="51b43-288">Tato vlastnost vrátí jedinečný suffic, který se připojí k cesta k souboru pro ukládání do mezipaměti prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="51b43-288">This property returns a unique suffic that is appended to the file path for caching browsers.</span></span> <span data-ttu-id="51b43-289">Výchozí hodnota je \_ \_ufps = a 6 číslice.</span><span class="sxs-lookup"><span data-stu-id="51b43-289">The default value is \_\_ufps= and a 6-digit number.</span></span>

## <a name="new-public-methods-for-the-page-class"></a><span data-ttu-id="51b43-290">Nové veřejné metody pro třídu stránky</span><span class="sxs-lookup"><span data-stu-id="51b43-290">New Public Methods for the Page Class</span></span>

<span data-ttu-id="51b43-291">Následující veřejné metody jsou nové třídy stránky v aplikaci ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="51b43-291">The following public methods are new to the Page class in ASP.NET 2.0.</span></span>

## <a name="addonprerendercompleteasync"></a><span data-ttu-id="51b43-292">AddOnPreRenderCompleteAsync</span><span class="sxs-lookup"><span data-stu-id="51b43-292">AddOnPreRenderCompleteAsync</span></span>

<span data-ttu-id="51b43-293">Tato metoda zaregistruje delegátů obslužných rutin událostí pro provádění asynchronní stránky.</span><span class="sxs-lookup"><span data-stu-id="51b43-293">This method registers event handler delegates for asynchronous page execution.</span></span> <span data-ttu-id="51b43-294">Asynchronní stránky jsou popsány dále v tomto modulu.</span><span class="sxs-lookup"><span data-stu-id="51b43-294">Asynchronous pages are discussed later in this module.</span></span>

## <a name="applystylesheetskin"></a><span data-ttu-id="51b43-295">ApplyStyleSheetSkin</span><span class="sxs-lookup"><span data-stu-id="51b43-295">ApplyStyleSheetSkin</span></span>

<span data-ttu-id="51b43-296">Vlastnosti v šabloně stylů stránky se vztahuje na stránce.</span><span class="sxs-lookup"><span data-stu-id="51b43-296">Applies the properties in a pages style sheet to the page.</span></span>

## <a name="executeregisteredasynctasks"></a><span data-ttu-id="51b43-297">ExecuteRegisteredAsyncTasks</span><span class="sxs-lookup"><span data-stu-id="51b43-297">ExecuteRegisteredAsyncTasks</span></span>

<span data-ttu-id="51b43-298">Tato metoda lidé asynchronní úlohu.</span><span class="sxs-lookup"><span data-stu-id="51b43-298">This method beings an asynchronous task.</span></span>

### <a name="getvalidators"></a><span data-ttu-id="51b43-299">GetValidators</span><span class="sxs-lookup"><span data-stu-id="51b43-299">GetValidators</span></span>

<span data-ttu-id="51b43-300">Vrátí kolekci validátory pro ověření zadané skupiny nebo výchozí ověřování, pokud není zadaný žádný.</span><span class="sxs-lookup"><span data-stu-id="51b43-300">Returns a collection of validators for the specified validation group or the default validation group if none is specified.</span></span>

## <a name="registerasynctask"></a><span data-ttu-id="51b43-301">RegisterAsyncTask</span><span class="sxs-lookup"><span data-stu-id="51b43-301">RegisterAsyncTask</span></span>

<span data-ttu-id="51b43-302">Tato metoda zaregistruje nový asynchronní úlohy.</span><span class="sxs-lookup"><span data-stu-id="51b43-302">This method registers a new async task.</span></span> <span data-ttu-id="51b43-303">Asynchronní stránky jsou popsané dále v tomto modulu.</span><span class="sxs-lookup"><span data-stu-id="51b43-303">Asynchronous pages are covered later in this module.</span></span>

## <a name="registerrequirescontrolstate"></a><span data-ttu-id="51b43-304">RegisterRequiresControlState</span><span class="sxs-lookup"><span data-stu-id="51b43-304">RegisterRequiresControlState</span></span>

<span data-ttu-id="51b43-305">Tato metoda informuje technologii ASP.NET, musí být trvalý stav řízení stránek.</span><span class="sxs-lookup"><span data-stu-id="51b43-305">This method tells ASP.NET that the pages control state must be persisted.</span></span>

## <a name="registerrequiresviewstateencryption"></a><span data-ttu-id="51b43-306">RegisterRequiresViewStateEncryption</span><span class="sxs-lookup"><span data-stu-id="51b43-306">RegisterRequiresViewStateEncryption</span></span>

<span data-ttu-id="51b43-307">Tato metoda informuje ASP.NET, že stav zobrazení stránky vyžaduje šifrování.</span><span class="sxs-lookup"><span data-stu-id="51b43-307">This method tells ASP.NET that the pages viewstate requires encryption.</span></span>

## <a name="resolveclienturl"></a><span data-ttu-id="51b43-308">ResolveClientUrl</span><span class="sxs-lookup"><span data-stu-id="51b43-308">ResolveClientUrl</span></span>

<span data-ttu-id="51b43-309">Vrátí relativní adresu URL, která lze použít pro požadavky klientů pro obrázky, atd.</span><span class="sxs-lookup"><span data-stu-id="51b43-309">Returns a relative URL that can be used for client requests for images, etc.</span></span>

## <a name="setfocus"></a><span data-ttu-id="51b43-310">Akce SetFocus</span><span class="sxs-lookup"><span data-stu-id="51b43-310">SetFocus</span></span>

<span data-ttu-id="51b43-311">Tato metoda nastaví fokus na ovládací prvek, který je zadán při počátečním načtení stránky.</span><span class="sxs-lookup"><span data-stu-id="51b43-311">This method will set the focus to the control that is specified when the page is initially loaded.</span></span>

## <a name="unregisterrequirescontrolstate"></a><span data-ttu-id="51b43-312">UnregisterRequiresControlState</span><span class="sxs-lookup"><span data-stu-id="51b43-312">UnregisterRequiresControlState</span></span>

<span data-ttu-id="51b43-313">Tato metoda zruší registraci ovládací prvek, který je předán jako už vyžadující trvalost stavu ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="51b43-313">This method will unregister the control that is passed to it as no longer requiring control state persistence.</span></span>

## <a name="changes-to-the-page-lifecycle"></a><span data-ttu-id="51b43-314">Změny v životním cyklu stránky</span><span class="sxs-lookup"><span data-stu-id="51b43-314">Changes to the Page Lifecycle</span></span>

<span data-ttu-id="51b43-315">Životní cyklus stránky v aplikaci ASP.NET 2.0 nezměnil výrazně, ale existují některé nové metody, které byste měli vědět.</span><span class="sxs-lookup"><span data-stu-id="51b43-315">The page lifecycle in ASP.NET 2.0 hasnt changed dramatically, but there are some new methods that you should be aware of.</span></span> <span data-ttu-id="51b43-316">Níže jsou uvedeny informace o životním cyklu stránky ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="51b43-316">The ASP.NET 2.0 page lifecycle is outlined below.</span></span>

## <a name="preinit-new-in-aspnet-20"></a><span data-ttu-id="51b43-317">PreInit (nového v technologii ASP.NET 2.0)</span><span class="sxs-lookup"><span data-stu-id="51b43-317">PreInit (New in ASP.NET 2.0)</span></span>

<span data-ttu-id="51b43-318">Událost PreInit je do nejdřívější fáze životního cyklu, kterým může přistupovat vývojář.</span><span class="sxs-lookup"><span data-stu-id="51b43-318">The PreInit event is the earliest stage in the lifecycle that a developer can access.</span></span> <span data-ttu-id="51b43-319">Přidání této události je umožněno prostřednictvím kódu programu změnit technologii ASP.NET 2.0 motivy, hlavní stránky, přístup k vlastnostem pro technologii ASP.NET 2.0 profilu atd. Pokud jste v postback stavu, jeho důležité si uvědomit, že Viewstate nebyla dosud nebyly použity k ovládacím prvkům v tomto okamžiku v životním cyklu.</span><span class="sxs-lookup"><span data-stu-id="51b43-319">The addition of this event makes it possible to programmatically change ASP.NET 2.0 themes, master pages, access properties for an ASP.NET 2.0 profile, etc. If you are in a postback state, its important to realize that Viewstate has not yet been applied to controls at this point in the lifecycle.</span></span> <span data-ttu-id="51b43-320">Proto pokud vývojář změní vlastnost ovládacího prvku v této fázi, bude pravděpodobně přepsán později v průběhu životního cyklu stránky.</span><span class="sxs-lookup"><span data-stu-id="51b43-320">Therefore, if a developer changes a property of a control at this stage, it will likely be overwritten later in the pages lifecycle.</span></span>

## <a name="init"></a><span data-ttu-id="51b43-321">Init</span><span class="sxs-lookup"><span data-stu-id="51b43-321">Init</span></span>

<span data-ttu-id="51b43-322">Události Init nebylo změněno z ASP.NET 1.x.</span><span class="sxs-lookup"><span data-stu-id="51b43-322">The Init event has not changed from ASP.NET 1.x.</span></span> <span data-ttu-id="51b43-323">Toto je místo by pro čtení nebo inicializace vlastností ovládacích prvků na stránku.</span><span class="sxs-lookup"><span data-stu-id="51b43-323">This is where you would want to read or initialize properties of controls on your page.</span></span> <span data-ttu-id="51b43-324">V této fázi, hlavní stránky, motivy jsou použité pro stránku již atd.</span><span class="sxs-lookup"><span data-stu-id="51b43-324">At this stage, master pages, themes, etc. are already applied to the page.</span></span>

## <a name="initcomplete-new-in-20"></a><span data-ttu-id="51b43-325">InitComplete (nové ve verzi 2.0)</span><span class="sxs-lookup"><span data-stu-id="51b43-325">InitComplete (New in 2.0)</span></span>

<span data-ttu-id="51b43-326">Na konci fáze inicializace stránky se nazývá InitComplete události.</span><span class="sxs-lookup"><span data-stu-id="51b43-326">The InitComplete event is called at the end of the pages initialization stage.</span></span> <span data-ttu-id="51b43-327">V tomto okamžiku v průběhu životního cyklu, dostanete ovládací prvky na stránce, ale jejich stavu nebyl ještě vyplněna.</span><span class="sxs-lookup"><span data-stu-id="51b43-327">At this point in the lifecycle, you can access controls on the page, but their state has not yet been populated.</span></span>

## <a name="preload-new-in-20"></a><span data-ttu-id="51b43-328">Předběžné načtení (nové ve verzi 2.0)</span><span class="sxs-lookup"><span data-stu-id="51b43-328">PreLoad (New in 2.0)</span></span>

<span data-ttu-id="51b43-329">Tato událost je volána po odeslání všech dat a těsně před stránky\_zatížení.</span><span class="sxs-lookup"><span data-stu-id="51b43-329">This event is called after all postback data has been applied and just prior to Page\_Load.</span></span>

## <a name="load"></a><span data-ttu-id="51b43-330">Zatížení</span><span class="sxs-lookup"><span data-stu-id="51b43-330">Load</span></span>

<span data-ttu-id="51b43-331">Událost zatížení nebylo změněno z ASP.NET 1.x.</span><span class="sxs-lookup"><span data-stu-id="51b43-331">The Load event has not changed from ASP.NET 1.x.</span></span>

## <a name="loadcomplete-new-in-20"></a><span data-ttu-id="51b43-332">LoadComplete (nové ve verzi 2.0)</span><span class="sxs-lookup"><span data-stu-id="51b43-332">LoadComplete (New in 2.0)</span></span>

<span data-ttu-id="51b43-333">Událost LoadComplete je poslední události ve fázi zatížení stránky.</span><span class="sxs-lookup"><span data-stu-id="51b43-333">The LoadComplete event is the last event in the pages load stage.</span></span> <span data-ttu-id="51b43-334">V této fázi se všechna data zpětné volání a stavu zobrazení použilo na stránku.</span><span class="sxs-lookup"><span data-stu-id="51b43-334">At this stage, all postback and viewstate data has been applied to the page.</span></span>

## <a name="prerender"></a><span data-ttu-id="51b43-335">PreRender</span><span class="sxs-lookup"><span data-stu-id="51b43-335">PreRender</span></span>

<span data-ttu-id="51b43-336">Pokud chcete pro stav zobrazení pro ovládací prvky, které jsou přidány na stránku dynamicky správně udržovat, událost PreRender je poslední možnost k je přidat.</span><span class="sxs-lookup"><span data-stu-id="51b43-336">If you would like for viewstate to be properly maintained for controls that are added to the page dynamically, the PreRender event is the last opportunity to add them.</span></span>

## <a name="prerendercomplete-new-in-20"></a><span data-ttu-id="51b43-337">PreRenderComplete (nové ve verzi 2.0)</span><span class="sxs-lookup"><span data-stu-id="51b43-337">PreRenderComplete (New in 2.0)</span></span>

<span data-ttu-id="51b43-338">Ve fázi PreRenderComplete byly přidány všechny ovládací prvky na stránku a je připravena k vykreslení stránky.</span><span class="sxs-lookup"><span data-stu-id="51b43-338">At the PreRenderComplete stage, all controls have been added to the page and the page is ready to be rendered.</span></span> <span data-ttu-id="51b43-339">Událost PreRenderComplete je poslední událost vyvolána před uložením stavu zobrazení stránky.</span><span class="sxs-lookup"><span data-stu-id="51b43-339">The PreRenderComplete event is the last event raised before the pages viewstate is saved.</span></span>

## <a name="savestatecomplete-new-in-20"></a><span data-ttu-id="51b43-340">SaveStateComplete (nové ve verzi 2.0)</span><span class="sxs-lookup"><span data-stu-id="51b43-340">SaveStateComplete (New in 2.0)</span></span>

<span data-ttu-id="51b43-341">Událost SaveStateComplete je volána hned po byl uložen stav viewstate a řízení všechny stránky.</span><span class="sxs-lookup"><span data-stu-id="51b43-341">The SaveStateComplete event is called immediately after all page viewstate and control state has been saved.</span></span> <span data-ttu-id="51b43-342">Toto je poslední událost před ve skutečnosti vykreslení stránky v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="51b43-342">This is the last event before the page is actually rendered to the browser.</span></span>

## <a name="render"></a><span data-ttu-id="51b43-343">Vykreslení</span><span class="sxs-lookup"><span data-stu-id="51b43-343">Render</span></span>

<span data-ttu-id="51b43-344">Metoda vykreslení nezměnil od ASP.NET 1.x.</span><span class="sxs-lookup"><span data-stu-id="51b43-344">The Render method has not changed since ASP.NET 1.x.</span></span> <span data-ttu-id="51b43-345">Toto je kde je inicializován HtmlTextWriter a vykreslení stránky v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="51b43-345">This is where the HtmlTextWriter is initialized and the page is rendered to the browser.</span></span>

## <a name="cross-page-postback-in-aspnet-20"></a><span data-ttu-id="51b43-346">Zpětné volání mezi stránkami v technologii ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="51b43-346">Cross-Page Postback in ASP.NET 2.0</span></span>

<span data-ttu-id="51b43-347">V technologii ASP.NET 1.x, postback musely k vystavování příspěvků na stejné stránce.</span><span class="sxs-lookup"><span data-stu-id="51b43-347">In ASP.NET 1.x, postbacks were required to post to the same page.</span></span> <span data-ttu-id="51b43-348">Mezi stránkami postback nebyly povoleny.</span><span class="sxs-lookup"><span data-stu-id="51b43-348">Cross-page postbacks were not allowed.</span></span> <span data-ttu-id="51b43-349">ASP.NET 2.0 přidává možnost odeslat zpět na jinou stránku prostřednictvím rozhraní IButtonControl.</span><span class="sxs-lookup"><span data-stu-id="51b43-349">ASP.NET 2.0 adds the ability to post back to a different page via the IButtonControl interface.</span></span> <span data-ttu-id="51b43-350">Libovolný ovládací prvek, který implementuje nové rozhraní IButtonControl (tlačítko, LinkButton a ImageButton kromě vlastní ovládací prvky třetích stran) můžete využít výhod tyto nové funkce prostřednictvím použití atributu PostBackUrl.</span><span class="sxs-lookup"><span data-stu-id="51b43-350">Any control that implements the new IButtonControl interface (Button, LinkButton, and ImageButton in addition to third-party custom controls) can take advantage of this new functionality via the use of the PostBackUrl attribute.</span></span> <span data-ttu-id="51b43-351">Následující kód ukazuje ovládacího prvku tlačítka, který odesílá zpět na druhé stránce.</span><span class="sxs-lookup"><span data-stu-id="51b43-351">The following code shows a Button control that posts back to a second page.</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

<span data-ttu-id="51b43-352">Když je stránka vrácena zpět, je přístupná prostřednictvím Vlastnost PreviousPage na druhé stránce stránka, která zahájí zpětné volání.</span><span class="sxs-lookup"><span data-stu-id="51b43-352">When the page is posted back, the Page that initiates the postback is accessible via the PreviousPage property on the second page.</span></span> <span data-ttu-id="51b43-353">Tato funkce se implementuje prostřednictvím nového webového formuláře\_DoPostBackWithOptions klientské funkce, která vykreslí technologii ASP.NET 2.0 na stránku při ovládacího prvku odešle zpět na jinou stránku.</span><span class="sxs-lookup"><span data-stu-id="51b43-353">This functionality is implemented via the new WebForm\_DoPostBackWithOptions client-side function that ASP.NET 2.0 renders to the page when a control posts back to a different page.</span></span> <span data-ttu-id="51b43-354">Tato funkce jazyka JavaScript je poskytovaný nové WebResource.axd obslužná rutina, která vydá skriptu do klienta.</span><span class="sxs-lookup"><span data-stu-id="51b43-354">This JavaScript function is provided by the new WebResource.axd handler which emits script to the client.</span></span>

<span data-ttu-id="51b43-355">Níže je návod, zpětné volání mezi stránkami.</span><span class="sxs-lookup"><span data-stu-id="51b43-355">The video below is a walkthrough of a cross-page postback.</span></span>


![](the-asp-net-2-0-page-model/_static/image2.png)


[<span data-ttu-id="51b43-356">Otevřete Video přes celou obrazovku</span><span class="sxs-lookup"><span data-stu-id="51b43-356">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/xpage1.wmv)


## <a name="more-details-on-cross-page-postbacks"></a><span data-ttu-id="51b43-357">Další informace o postback mezi stránkami</span><span class="sxs-lookup"><span data-stu-id="51b43-357">More Details on Cross-Page Postbacks</span></span>

### <a name="viewstate"></a><span data-ttu-id="51b43-358">Stav zobrazení</span><span class="sxs-lookup"><span data-stu-id="51b43-358">Viewstate</span></span>

<span data-ttu-id="51b43-359">Můžete být vyzváni, sami již o co se stane vlastnost viewstate od první stránky ve scénáři mezi stránkami zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="51b43-359">You may have asked yourself already about what happens to the viewstate from the first page in a cross-page postback scenario.</span></span> <span data-ttu-id="51b43-360">Po všech libovolný ovládací prvek, který neimplementuje rozhraní IPostBackDataHandler zachová stav prostřednictvím viewstate, takže tak, aby měl přístup k vlastnostem tohoto ovládacího prvku na druhé stránce zpětné volání mezi stránkami, musí mít přístup k vlastnost viewstate pro stránku.</span><span class="sxs-lookup"><span data-stu-id="51b43-360">After all, any control that does not implement IPostBackDataHandler will persist its state via viewstate, so to have access to the properties of that control on the second page of a cross-page postback, you must have access to the viewstate for the page.</span></span> <span data-ttu-id="51b43-361">Tento scénář pomocí nové skrytá pole na druhé stránce názvem postará technologie ASP.NET 2.0 \_ \_PREVIOUSPAGE.</span><span class="sxs-lookup"><span data-stu-id="51b43-361">ASP.NET 2.0 takes care of this scenario using a new hidden field in the second page called \_\_PREVIOUSPAGE.</span></span> <span data-ttu-id="51b43-362">\_ \_PREVIOUSPAGE pole formuláře obsahuje viewstate na první stránce tak, že máte přístup k vlastnosti všech ovládacích prvků na druhé stránce.</span><span class="sxs-lookup"><span data-stu-id="51b43-362">The \_\_PREVIOUSPAGE form field contains the viewstate for the first page so that you can have access to the properties of all controls in the second page.</span></span>

### <a name="circumventing-findcontrol"></a><span data-ttu-id="51b43-363">Obcházení FindControl</span><span class="sxs-lookup"><span data-stu-id="51b43-363">Circumventing FindControl</span></span>

<span data-ttu-id="51b43-364">V video s návodem zpětné volání mezi stránkami I metodu FindControl použít k získání odkazu do ovládacího prvku textového pole na první stránce.</span><span class="sxs-lookup"><span data-stu-id="51b43-364">In the video walkthrough of a cross-page postback, I used the FindControl method to get a reference to the TextBox control on the first page.</span></span> <span data-ttu-id="51b43-365">Tato metoda funguje dobře k tomuto účelu, ale FindControl je nákladné a vyžaduje zápis další kód.</span><span class="sxs-lookup"><span data-stu-id="51b43-365">That method works well for that purpose, but FindControl is expensive and it requires writing additional code.</span></span> <span data-ttu-id="51b43-366">Naštěstí technologii ASP.NET 2.0 představuje alternativu ke FindControl pro tento účel, který bude fungovat v řadě scénářů.</span><span class="sxs-lookup"><span data-stu-id="51b43-366">Fortunately, ASP.NET 2.0 provides an alternative to FindControl for this purpose that will work in many scenarios.</span></span> <span data-ttu-id="51b43-367">Direktiva PreviousPageType umožňuje mít silného typu odkaz na předchozí stránku s použitím TypeName nebo atribut VirtualPath.</span><span class="sxs-lookup"><span data-stu-id="51b43-367">The PreviousPageType directive allows you to have a strongly-typed reference to the previous page by using either the TypeName or the VirtualPath attribute.</span></span> <span data-ttu-id="51b43-368">Atribut TypeName umožňuje určit typ na předchozí stránku, přestože Atribut VirtualPath umožňuje odkazovat na předchozí stránku pomocí virtuální cesty.</span><span class="sxs-lookup"><span data-stu-id="51b43-368">The TypeName attribute allows you to specify the type of the previous page while the VirtualPath attribute allows you to refer to the previous page using a virtual path.</span></span> <span data-ttu-id="51b43-369">Jakmile nastavíte PreviousPageType direktiva, musí pak vystavit ovládací prvky, ke kterému chcete povolit přístup pomocí veřejné vlastnosti atd.</span><span class="sxs-lookup"><span data-stu-id="51b43-369">After you've set the PreviousPageType directive, you must then expose the controls, etc. to which you want to allow access using public properties.</span></span>

## <a name="lab-1-cross-page-postback"></a><span data-ttu-id="51b43-370">Zpětné volání mezi stránkami testovacím 1</span><span class="sxs-lookup"><span data-stu-id="51b43-370">Lab 1 Cross-Page Postback</span></span>

<span data-ttu-id="51b43-371">V tomto testovacím prostředí vytvoříte aplikaci, která používá nové funkce zpětného volání mezi stránkami ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="51b43-371">In this lab, you will create an application that uses the new cross-page postback functionality of ASP.NET 2.0.</span></span>

1. <span data-ttu-id="51b43-372">Otevřete Visual Studio 2005 a vytvořte nový web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="51b43-372">Open Visual Studio 2005 and create a new ASP.NET Web site.</span></span>
2. <span data-ttu-id="51b43-373">Přidáte nový webový formulář, názvem page2.aspx.</span><span class="sxs-lookup"><span data-stu-id="51b43-373">Add a new Webform called page2.aspx.</span></span>
3. <span data-ttu-id="51b43-374">Otevřete Default.aspx v zobrazení návrhu a přidání ovládacího prvku tlačítko a ovládacího prvku textového pole.</span><span class="sxs-lookup"><span data-stu-id="51b43-374">Open the Default.aspx in Design view and add a Button control and a TextBox control.</span></span> 

    1. <span data-ttu-id="51b43-375">Zadejte ID ovládacího prvku tlačítko **odeslat** a do textového pole řídit ID **uživatelské jméno**.</span><span class="sxs-lookup"><span data-stu-id="51b43-375">Give the Button control an ID of **SubmitButton** and the TextBox control an ID of **UserName**.</span></span>
    2. <span data-ttu-id="51b43-376">Nastavte vlastnost PostBackUrl tlačítka na page2.aspx.</span><span class="sxs-lookup"><span data-stu-id="51b43-376">Set the PostBackUrl property of the Button to page2.aspx.</span></span>
4. <span data-ttu-id="51b43-377">Otevřete page2.aspx v zobrazení zdroje.</span><span class="sxs-lookup"><span data-stu-id="51b43-377">Open page2.aspx in Source view.</span></span>
5. <span data-ttu-id="51b43-378">Přidáte direktivu @ PreviousPageType, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="51b43-378">Add a @ PreviousPageType directive as shown below:</span></span>
6. <span data-ttu-id="51b43-379">Přidejte následující kód na stránku\_zatížení na page2.aspx kódu:</span><span class="sxs-lookup"><span data-stu-id="51b43-379">Add the following code to the Page\_Load of page2.aspx's code-behind:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. <span data-ttu-id="51b43-380">Sestavte projekt kliknutím na sestavení v nabídce sestavení.</span><span class="sxs-lookup"><span data-stu-id="51b43-380">Build the project by clicking on Build on the Build menu.</span></span>
8. <span data-ttu-id="51b43-381">Přidejte následující kód do kódu pro Default.aspx:</span><span class="sxs-lookup"><span data-stu-id="51b43-381">Add the following code to the code-behind for Default.aspx:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. <span data-ttu-id="51b43-382">Změnit stránce\_zatížení v page2.aspx pro následující:</span><span class="sxs-lookup"><span data-stu-id="51b43-382">Change the Page\_Load in page2.aspx to the following:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. <span data-ttu-id="51b43-383">Sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="51b43-383">Build the project.</span></span>
11. <span data-ttu-id="51b43-384">Spusťte projekt.</span><span class="sxs-lookup"><span data-stu-id="51b43-384">Run the project.</span></span>
12. <span data-ttu-id="51b43-385">Zadejte název do textového pole a klikněte na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="51b43-385">Enter your name in the TextBox and click the button.</span></span>
13. <span data-ttu-id="51b43-386">Co je výsledkem?</span><span class="sxs-lookup"><span data-stu-id="51b43-386">What is the result?</span></span>

## <a name="asynchronous-pages-in-aspnet-20"></a><span data-ttu-id="51b43-387">Asynchronní stránky v technologii ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="51b43-387">Asynchronous Pages in ASP.NET 2.0</span></span>

<span data-ttu-id="51b43-388">Mnoho problémů kolizí v technologii ASP.NET jsou způsobeny latence externí volání (jako například volání webové služby nebo databáze), latence vstupně-výstupní operace souboru atd. Po odeslání žádosti na aplikace ASP.NET používá jeden z jeho pracovních vláken ASP.NET ke zpracování tohoto požadavku.</span><span class="sxs-lookup"><span data-stu-id="51b43-388">Many contention problems in ASP.NET are caused by latency of external calls (such as Web service or database calls), file IO latency, etc. When a request is made against an ASP.NET application, ASP.NET uses one of its worker threads to service that request.</span></span> <span data-ttu-id="51b43-389">Tento požadavek vlastní daném vláknu až po dokončení požadavku a byl odeslán do odpovědi.</span><span class="sxs-lookup"><span data-stu-id="51b43-389">That request owns that thread until the request is complete and the response has been sent.</span></span> <span data-ttu-id="51b43-390">ASP.NET 2.0 se snaží vyřešit problémy s latencí s těmito typy problémů přidáním možnosti spouštění stránek asynchronně.</span><span class="sxs-lookup"><span data-stu-id="51b43-390">ASP.NET 2.0 seeks to resolve latency issues with these types of issues by adding the capability to execute pages asynchronously.</span></span> <span data-ttu-id="51b43-391">Která znamená, že pracovní vlákno může spuštění požadavku sady a pak přebírají další provádění na jiné vlákno, a tím vrácením fondu k dispozici vláken rychle.</span><span class="sxs-lookup"><span data-stu-id="51b43-391">That means that a worker thread can start the request and then hand off additional execution to another thread, thereby returning to the available thread pool quickly.</span></span> <span data-ttu-id="51b43-392">Po dokončení soubor vstupně-výstupní operace, volání databáze, atd., nové vlákno se získávají z fondu podprocesů na dokončení požadavku.</span><span class="sxs-lookup"><span data-stu-id="51b43-392">When the file IO, database call, etc. has completed, a new thread is obtained from the thread pool to finish the request.</span></span>

<span data-ttu-id="51b43-393">Prvním krokem při vytváření stránky asynchronní je nastavit **asynchronní** direktivy stránky takto:</span><span class="sxs-lookup"><span data-stu-id="51b43-393">The first step in making a page execute asynchronously is to set the **Async** attribute of the page directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

<span data-ttu-id="51b43-394">Tento atribut informuje technologii ASP.NET pro implementaci IHttpAsyncHandler pro stránku.</span><span class="sxs-lookup"><span data-stu-id="51b43-394">This attribute tells ASP.NET to implement the IHttpAsyncHandler for the page.</span></span>

<span data-ttu-id="51b43-395">Dalším krokem je třeba volat metodu AddOnPreRenderCompleteAsync na místo v životním cyklu stránky před PreRender.</span><span class="sxs-lookup"><span data-stu-id="51b43-395">The next step is to call the AddOnPreRenderCompleteAsync method at a point in the lifecycle of the page prior to PreRender.</span></span> <span data-ttu-id="51b43-396">(Tato metoda je volána obvykle stránce\_zatížení.) Metoda AddOnPreRenderCompleteAsync přebírá dva parametry; BeginEventHandler a EndEventHandler.</span><span class="sxs-lookup"><span data-stu-id="51b43-396">(This method is typically called in Page\_Load.) The AddOnPreRenderCompleteAsync method takes two parameters; a BeginEventHandler and an EndEventHandler.</span></span> <span data-ttu-id="51b43-397">BeginEventHandler vrátí IAsyncResult, který se potom předá jako parametr pro EndEventHandler.</span><span class="sxs-lookup"><span data-stu-id="51b43-397">The BeginEventHandler returns an IAsyncResult which is then passed as a parameter to the EndEventHandler.</span></span>

<span data-ttu-id="51b43-398">Níže je návod, požadavek asynchronní stránky.</span><span class="sxs-lookup"><span data-stu-id="51b43-398">The video below is a walkthrough of an asynchronous page request.</span></span>


![](the-asp-net-2-0-page-model/_static/image3.png)


[<span data-ttu-id="51b43-399">Otevřete Video přes celou obrazovku</span><span class="sxs-lookup"><span data-stu-id="51b43-399">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/async1.wmv)


> [!NOTE]
> <span data-ttu-id="51b43-400">Stránku asynchronní nevykresluje do prohlížeče, dokud se nedokončí EndEventHandler.</span><span class="sxs-lookup"><span data-stu-id="51b43-400">An async page does not render to the browser until the EndEventHandler has completed.</span></span> <span data-ttu-id="51b43-401">Žádné nejistých ale si bude myslet někteří vývojáři asynchronních požadavků, že je podobná asynchronních zpětných volání.</span><span class="sxs-lookup"><span data-stu-id="51b43-401">No doubt but that some developers will think of async requests as being similar to async callbacks.</span></span> <span data-ttu-id="51b43-402">Je důležité si uvědomit, že nejsou.</span><span class="sxs-lookup"><span data-stu-id="51b43-402">It's important to realize that they are not.</span></span> <span data-ttu-id="51b43-403">Výhodou na asynchronní požadavky je, že první pracovní vlákno se může vracet do fondu vláken pro zpracování nových požadavků, a tím snižuje kolize, protože nejsou vstupně-výstupní operace vázaný atd.</span><span class="sxs-lookup"><span data-stu-id="51b43-403">The benefit to asynchronous requests is that the first worker thread can be returned to the thread pool to service new requests, thereby reducing contention due to being IO bound, etc.</span></span>


## <a name="script-callbacks-in-aspnet-20"></a><span data-ttu-id="51b43-404">Zpětná volání skriptu v technologii ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="51b43-404">Script Callbacks in ASP.NET 2.0</span></span>

<span data-ttu-id="51b43-405">Vývojáři webů vždy vyhledávaly způsoby, jak zabránit blikání přidružené zpětné volání.</span><span class="sxs-lookup"><span data-stu-id="51b43-405">Web developers have always looked for ways to prevent the flickering associated with a callback.</span></span> <span data-ttu-id="51b43-406">V technologii ASP.NET 1.x SmartNavigation byl nejběžnější metodou pro vyloučení blikání, ale SmartNavigation způsobeno problémy pro vývojáře v některé z důvodu složitosti jeho implementace na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="51b43-406">In ASP.NET 1.x, SmartNavigation was the most common method for avoiding flickering, but SmartNavigation caused problems for some developers because of the complexity of its implementation on the client.</span></span> <span data-ttu-id="51b43-407">ASP.NET 2.0 řeší tento problém s zpětná volání skriptu.</span><span class="sxs-lookup"><span data-stu-id="51b43-407">ASP.NET 2.0 addresses this issue with script callbacks.</span></span> <span data-ttu-id="51b43-408">Zpětná volání skriptu využívat XMLHttp provádět požadavky na webový server prostřednictvím jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="51b43-408">Script callbacks utilize XMLHttp to make requests against the Web server via JavaScript.</span></span> <span data-ttu-id="51b43-409">Žádost o XMLHttp vrací data XML, který lze potom manipulovat prostřednictvím prohlížeče DOM</span><span class="sxs-lookup"><span data-stu-id="51b43-409">The XMLHttp request returns XML data that can then be manipulated via the browser's DOM.</span></span> <span data-ttu-id="51b43-410">Uživatel skrytá XMLHttp kód nové WebResource.axd obslužnou rutinou.</span><span class="sxs-lookup"><span data-stu-id="51b43-410">XMLHttp code is hidden from the user by the new WebResource.axd handler.</span></span>

<span data-ttu-id="51b43-411">Existuje několik kroků, které jsou nezbytné, aby se konfigurace zpětné volání skriptu v technologii ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="51b43-411">There are several steps that are necessary in order to configure a script callback in ASP.NET 2.0.</span></span>

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a><span data-ttu-id="51b43-412">Krok 1: Implementace rozhraní ICallbackEventHandler</span><span class="sxs-lookup"><span data-stu-id="51b43-412">Step 1 : Implement the ICallbackEventHandler Interface</span></span>

<span data-ttu-id="51b43-413">Aby pro technologii ASP.NET rozpoznat stránku jako účastní zpětné volání skriptu musí implementovat rozhraní ICallbackEventHandler.</span><span class="sxs-lookup"><span data-stu-id="51b43-413">In order for ASP.NET to recognize your page as participating in a script callback, you must implement the ICallbackEventHandler interface.</span></span> <span data-ttu-id="51b43-414">Můžete to provést v souboru kódu takto:</span><span class="sxs-lookup"><span data-stu-id="51b43-414">You can do this in your code-behind file like so:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

<span data-ttu-id="51b43-415">Můžete to můžete provést taky pomocí direktivy jako @ implementuje proto:</span><span class="sxs-lookup"><span data-stu-id="51b43-415">You can also do this using the @ Implements directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

<span data-ttu-id="51b43-416">Direktiva @ implementuje by obvykle používají při použití vloženého kódu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="51b43-416">You would typically use the @ Implements directive when using inline ASP.NET code.</span></span>

## <a name="step-2--call-getcallbackeventreference"></a><span data-ttu-id="51b43-417">Krok 2: Volání GetCallbackEventReference</span><span class="sxs-lookup"><span data-stu-id="51b43-417">Step 2 : Call GetCallbackEventReference</span></span>

<span data-ttu-id="51b43-418">Jak je uvedeno nahoře, je v rutině WebResource.axd zapouzdřený XMLHttp volání.</span><span class="sxs-lookup"><span data-stu-id="51b43-418">As mentioned previously, the XMLHttp call is encapsulated in the WebResource.axd handler.</span></span> <span data-ttu-id="51b43-419">Když se zobrazí vaše stránka ASP.NET přidá volání webového formuláře\_DoCallback, klientský skript, který zajišťuje WebResource.axd.</span><span class="sxs-lookup"><span data-stu-id="51b43-419">When your page is rendered, ASP.NET will add a call to WebForm\_DoCallback, a client script that is provided by WebResource.axd.</span></span> <span data-ttu-id="51b43-420">Webovém formuláři\_DoCallback funkce nahrazuje \_ \_doPostBack funkce pro zpětné volání.</span><span class="sxs-lookup"><span data-stu-id="51b43-420">The WebForm\_DoCallback function replaces the \_\_doPostBack function for a callback.</span></span> <span data-ttu-id="51b43-421">Nezapomeňte, že \_ \_doPostBack prostřednictvím kódu programu odešle formuláře na stránce.</span><span class="sxs-lookup"><span data-stu-id="51b43-421">Remember that \_\_doPostBack programmatically submits the form on the page.</span></span> <span data-ttu-id="51b43-422">Ve scénáři zpětného volání, ale chcete zabránit zpětné volání, takže \_ \_doPostBack nestačí.</span><span class="sxs-lookup"><span data-stu-id="51b43-422">In a callback scenario, you want to prevent a postback, so \_\_doPostBack will not suffice.</span></span>

> [!NOTE]
> <span data-ttu-id="51b43-423">\_\_doPostBack je stále vykreslen na stránku ve scénáři klientský skript zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="51b43-423">\_\_doPostBack is still rendered to the page in a client script callback scenario.</span></span> <span data-ttu-id="51b43-424">Nicméně se nepoužije pro zpětné volání.</span><span class="sxs-lookup"><span data-stu-id="51b43-424">However, it's not used for the callback.</span></span>


<span data-ttu-id="51b43-425">Argumenty pro webovém formuláři\_DoCallback klientské funkce jsou k dispozici prostřednictvím funkce serverové GetCallbackEventReference, která by za normálních okolností volána stránce\_zatížení.</span><span class="sxs-lookup"><span data-stu-id="51b43-425">The arguments for the WebForm\_DoCallback client-side function are provided via the server-side function GetCallbackEventReference which would normally be called in Page\_Load.</span></span> <span data-ttu-id="51b43-426">Typické volání GetCallbackEventReference může vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="51b43-426">A typical call to GetCallbackEventReference might look like this:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> <span data-ttu-id="51b43-427">V takovém případě cm představuje instanci ClientScriptManager.</span><span class="sxs-lookup"><span data-stu-id="51b43-427">In this case, cm is an instance of ClientScriptManager.</span></span> <span data-ttu-id="51b43-428">Třída ClientScriptManager bude zmínka v dalších částech tohoto modulu.</span><span class="sxs-lookup"><span data-stu-id="51b43-428">The ClientScriptManager class will be covered later in this module.</span></span>


<span data-ttu-id="51b43-429">Existuje několik přetížené verzích GetCallbackEventReference.</span><span class="sxs-lookup"><span data-stu-id="51b43-429">There are several overloaded versions of GetCallbackEventReference.</span></span> <span data-ttu-id="51b43-430">V takovém případě argumenty jsou následující:</span><span class="sxs-lookup"><span data-stu-id="51b43-430">In this case, the arguments are as follows:</span></span>

`this`

<span data-ttu-id="51b43-431">Odkaz na ovládací prvek, kde je volána GetCallbackEventReference.</span><span class="sxs-lookup"><span data-stu-id="51b43-431">A reference to the control where GetCallbackEventReference is being called.</span></span> <span data-ttu-id="51b43-432">V takovém případě je vlastní stránky.</span><span class="sxs-lookup"><span data-stu-id="51b43-432">In this case, it's the page itself.</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

<span data-ttu-id="51b43-433">Argument řetězce, který bude předán z kódu na straně klienta pro událost na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="51b43-433">A string argument that will be passed from the client-side code to the server-side event.</span></span> <span data-ttu-id="51b43-434">V tomto případě Im předávání hodnotu rozevírací název ddlCompany.</span><span class="sxs-lookup"><span data-stu-id="51b43-434">In this case, Im passing the value of a dropdown called ddlCompany.</span></span>

`ShowCompanyName`

<span data-ttu-id="51b43-435">Název funkce klienta, která bude přijímat návratovou hodnotu (jako řetězec) z události zpětného volání na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="51b43-435">The name of the client-side function that will accept the return value (as string) from the server-side callback event.</span></span> <span data-ttu-id="51b43-436">Tato funkce bude volat pouze po úspěšné zpětného volání na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="51b43-436">This function will only be called when the server-side callback is successful.</span></span> <span data-ttu-id="51b43-437">Proto v zájmu odolnosti, obvykle doporučujeme používat přetížené verzi GetCallbackEventReference, který používá argument další řetězec určující název klienta funkce provést v případě chyby.</span><span class="sxs-lookup"><span data-stu-id="51b43-437">Therefore, for the sake of robustness, it is generally recommended to use the overloaded version of GetCallbackEventReference that takes an additional string argument specifying the name of a client-side function to execute in the event of an error.</span></span>

`null`

<span data-ttu-id="51b43-438">Řetězec představující klientské funkce, která ho inicializovat před zpětného volání k serveru.</span><span class="sxs-lookup"><span data-stu-id="51b43-438">A string representing a client-side function that it initiated before the callback to the server.</span></span> <span data-ttu-id="51b43-439">V takovém případě není žádný skript, tak argument má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="51b43-439">In this case, there is no such script, so the argument is null.</span></span>

`true`

<span data-ttu-id="51b43-440">Logická hodnota určující, zda má být asynchronně chování zpětné volání.</span><span class="sxs-lookup"><span data-stu-id="51b43-440">A Boolean specifying whether or not to conduct the callback asynchronously.</span></span>

<span data-ttu-id="51b43-441">Volání webového formuláře\_DoCallback v klientovi předá tyto argumenty.</span><span class="sxs-lookup"><span data-stu-id="51b43-441">The call to WebForm\_DoCallback on the client will pass these arguments.</span></span> <span data-ttu-id="51b43-442">Proto při této stránky na straně klienta, tento kód bude vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="51b43-442">Therefore, when this page is rendered on the client, that code will look like so:</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

<span data-ttu-id="51b43-443">Všimněte si, že podpis funkce na straně klienta je trochu liší.</span><span class="sxs-lookup"><span data-stu-id="51b43-443">Notice that the signature of the function on the client is a bit different.</span></span> <span data-ttu-id="51b43-444">Funkce klienta předá 5 řetězce a logickou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="51b43-444">The client-side function passes 5 strings and a Boolean.</span></span> <span data-ttu-id="51b43-445">Další řetězec (která je null v předchozím příkladu) obsahuje klientské funkce, která zpracuje všechny chyby ze zpětného volání na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="51b43-445">The additional string (which is null in the above example) contains the client-side function that will handle any errors from the server-side callback.</span></span>

## <a name="step-3--hook-the-client-side-control-event"></a><span data-ttu-id="51b43-446">Krok 3: Připojení událostí prvek na straně klienta</span><span class="sxs-lookup"><span data-stu-id="51b43-446">Step 3 : Hook the Client-Side Control Event</span></span>

<span data-ttu-id="51b43-447">Všimněte si, že návratová hodnota GetCallbackEventReference výše byl přiřazen na proměnnou string.</span><span class="sxs-lookup"><span data-stu-id="51b43-447">Notice that the return value of GetCallbackEventReference above was assigned to a string variable.</span></span> <span data-ttu-id="51b43-448">Tento řetězec se používá k připojení na straně klienta událostí pro ovládací prvek, který spouští zpětné volání.</span><span class="sxs-lookup"><span data-stu-id="51b43-448">That string is used to hook a client-side event for the control that initiates the callback.</span></span> <span data-ttu-id="51b43-449">V tomto příkladu rozevíracího seznamu na stránce iniciované zpětné volání, chcete připojit *při změně* událostí.</span><span class="sxs-lookup"><span data-stu-id="51b43-449">In this example, the callback is initiated by a dropdown on the page, so I want to hook the *OnChange* event.</span></span>

<span data-ttu-id="51b43-450">Napojit událost na straně klienta, jednoduše přidejte obslužnou rutinu kód klienta následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="51b43-450">To hook the client-side event, simply add a handler to the client-side markup as follows:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

<span data-ttu-id="51b43-451">Odvolat, *cbRef* je vrácená hodnota z volání GetCallbackEventReference.</span><span class="sxs-lookup"><span data-stu-id="51b43-451">Recall that *cbRef* is the return value from the call to GetCallbackEventReference.</span></span> <span data-ttu-id="51b43-452">Obsahuje volání webového formuláře\_DoCallback zobrazená výše.</span><span class="sxs-lookup"><span data-stu-id="51b43-452">It contains the call to WebForm\_DoCallback that was shown above.</span></span>

## <a name="step-4--register-the-client-side-script"></a><span data-ttu-id="51b43-453">Krok 4: Registrace klientský skript</span><span class="sxs-lookup"><span data-stu-id="51b43-453">Step 4 : Register the Client-Side Script</span></span>

<span data-ttu-id="51b43-454">Odvolání, který volání GetCallbackEventReference zadali, že klientský skript volá **ShowCompanyName** by byl proveden, pokud úspěšná zpětného volání na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="51b43-454">Recall that the call to GetCallbackEventReference specified that a client-side script called **ShowCompanyName** would be executed when the server-side callback succeeds.</span></span> <span data-ttu-id="51b43-455">Tento skript je nutné přidat na stránku pomocí ClientScriptManager instance.</span><span class="sxs-lookup"><span data-stu-id="51b43-455">That script needs to be added to the page using a ClientScriptManager instance.</span></span> <span data-ttu-id="51b43-456">(Třída ClientScriptManager bude dicussed později v tomto modulu.) Proto je provést tento jako:</span><span class="sxs-lookup"><span data-stu-id="51b43-456">(The ClientScriptManager class will be dicussed later in this module.) You do that like so:</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a><span data-ttu-id="51b43-457">Krok 5: Volání metody rozhraní ICallbackEventHandler</span><span class="sxs-lookup"><span data-stu-id="51b43-457">Step 5 : Call the Methods of the ICallbackEventHandler Interface</span></span>

<span data-ttu-id="51b43-458">ICallbackEventHandler obsahuje dvě metody, které je nutné implementovat v kódu.</span><span class="sxs-lookup"><span data-stu-id="51b43-458">The ICallbackEventHandler contains two methods that you need to implement in your code.</span></span> <span data-ttu-id="51b43-459">Jsou **RaiseCallbackEvent** a **GetCallbackEvent**.</span><span class="sxs-lookup"><span data-stu-id="51b43-459">They are **RaiseCallbackEvent** and **GetCallbackEvent**.</span></span>

<span data-ttu-id="51b43-460">**RaiseCallbackEvent** přijímá jako argument řetězec a vrátí nic.</span><span class="sxs-lookup"><span data-stu-id="51b43-460">**RaiseCallbackEvent** takes a string as an argument and returns nothing.</span></span> <span data-ttu-id="51b43-461">Argument řetězce předaný z na straně klienta volání webového formuláře\_DoCallback.</span><span class="sxs-lookup"><span data-stu-id="51b43-461">The string argument is passed from the client-side call to WebForm\_DoCallback.</span></span> <span data-ttu-id="51b43-462">V takovém případě je tato hodnota *hodnotu* atribut názvem ddlCompany rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="51b43-462">In this case, that value is the *value* attribute of the dropdown called ddlCompany.</span></span> <span data-ttu-id="51b43-463">Serverový kód musí být umístěny v metodě RaiseCallbackEvent.</span><span class="sxs-lookup"><span data-stu-id="51b43-463">Your server-side code should be placed in the RaiseCallbackEvent method.</span></span> <span data-ttu-id="51b43-464">Například pokud je vaše zpětné volání WebRequest proti externí prostředek, tento kód musí být umístěny v RaiseCallbackEvent.</span><span class="sxs-lookup"><span data-stu-id="51b43-464">For example, if your callback is making a WebRequest against an external resource, that code should be placed in RaiseCallbackEvent.</span></span>

<span data-ttu-id="51b43-465">**GetCallbackEvent** je zodpovědná za zpracování vrácení zpětné volání do klienta.</span><span class="sxs-lookup"><span data-stu-id="51b43-465">**GetCallbackEvent** is responsible for processing the return of the callback to the client.</span></span> <span data-ttu-id="51b43-466">Přebírá bez argumentů a vrátí řetězec.</span><span class="sxs-lookup"><span data-stu-id="51b43-466">It takes no arguments and returns a string.</span></span> <span data-ttu-id="51b43-467">Řetězec, který vrátí budou předány jako argument funkce na straně klienta v tomto případě *ShowCompanyName*.</span><span class="sxs-lookup"><span data-stu-id="51b43-467">The string that it returns will be passed as an argument to the client-side function, in this case *ShowCompanyName*.</span></span>

<span data-ttu-id="51b43-468">Po dokončení výše uvedené kroky, jste připraveni k provedení skriptu zpětné volání v technologii ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="51b43-468">Once you have completed the above steps, you are ready to perform a script callback in ASP.NET 2.0.</span></span>


![](the-asp-net-2-0-page-model/_static/image4.png)


[<span data-ttu-id="51b43-469">Otevřete Video přes celou obrazovku</span><span class="sxs-lookup"><span data-stu-id="51b43-469">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/callback1.wmv)


<span data-ttu-id="51b43-470">Zpětná volání skriptu v technologii ASP.NET jsou podporovány v žádný prohlížeč, který podporuje vytváření volání XMLHttp.</span><span class="sxs-lookup"><span data-stu-id="51b43-470">Script callbacks in ASP.NET are supported in any browser that supports making XMLHttp calls.</span></span> <span data-ttu-id="51b43-471">Který zahrnuje všechny moderní prohlížeče ve použití ještě dnes.</span><span class="sxs-lookup"><span data-stu-id="51b43-471">That includes all of the modern browsers in use today.</span></span> <span data-ttu-id="51b43-472">Internet Explorer používá objekt XMLHttp ActiveX při vnitřní objekt XMLHttp používat další moderní prohlížeče (včetně nadcházející aplikaci Internet Explorer 7).</span><span class="sxs-lookup"><span data-stu-id="51b43-472">Internet Explorer uses the XMLHttp ActiveX object while other modern browsers (including the upcoming IE 7) use an intrinsic XMLHttp object.</span></span> <span data-ttu-id="51b43-473">K určení prostřednictvím kódu programu, pokud prohlížeč podporuje zpětná volání, můžete použít **Request.Browser.SupportCallback** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="51b43-473">To programmatically determine if a browser supports callbacks, you can use the **Request.Browser.SupportCallback** property.</span></span> <span data-ttu-id="51b43-474">Tato vlastnost vrátí **true** Pokud klienta, který podporuje zpětná volání skriptu.</span><span class="sxs-lookup"><span data-stu-id="51b43-474">This property will return **true** if the requesting client supports script callbacks.</span></span>

## <a name="working-with-client-script-in-aspnet-20"></a><span data-ttu-id="51b43-475">Práce s klientského skriptu v technologii ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="51b43-475">Working with Client Script in ASP.NET 2.0</span></span>

<span data-ttu-id="51b43-476">Skripty klienta v technologii ASP.NET 2.0 jsou spravované prostřednictvím použití třídy ClientScriptManager.</span><span class="sxs-lookup"><span data-stu-id="51b43-476">Client scripts in ASP.NET 2.0 are managed via the use of the ClientScriptManager class.</span></span> <span data-ttu-id="51b43-477">Třída ClientScriptManager uchovává informace o klientských skriptů pomocí typu a název.</span><span class="sxs-lookup"><span data-stu-id="51b43-477">The ClientScriptManager class keeps track of client scripts using a type and a name.</span></span> <span data-ttu-id="51b43-478">To brání stejného skriptu prostřednictvím kódu programu vkládání na stránce více než jednou.</span><span class="sxs-lookup"><span data-stu-id="51b43-478">This prevents the same script from being programmatically inserted on a page more than once.</span></span>

> [!NOTE]
> <span data-ttu-id="51b43-479">Po skript byl úspěšně registrován na stránce, bude další pokus o registraci stejného skriptu jednoduše výsledkem skript není registrovaný ještě jednou.</span><span class="sxs-lookup"><span data-stu-id="51b43-479">After a script has been successfully registered on a page, any subsequent attempt to register the same script will simply result in the script not being registered a second time.</span></span> <span data-ttu-id="51b43-480">Budou přidány žádné duplicitní skripty a dojde k žádná výjimka.</span><span class="sxs-lookup"><span data-stu-id="51b43-480">No duplicate scripts are added and no exception occurs.</span></span> <span data-ttu-id="51b43-481">Aby se zabránilo zbytečným výpočetní, jsou metody, které můžete použít k určení, zda je již zaregistrován skript tak, aby vám nepokoušejte a zaregistrujte ho více než jednou.</span><span class="sxs-lookup"><span data-stu-id="51b43-481">To avoid unnecessary computation, there are methods that you can use to determine if a script is already registered so that you do not attempt to register it more than once.</span></span>


<span data-ttu-id="51b43-482">Metody ClientScriptManager by měla být pro všechny aktuální vývojáře využívající technologii ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="51b43-482">The methods of the ClientScriptManager should be familiar to all current ASP.NET developers:</span></span>

## <a name="registerclientscriptblock"></a><span data-ttu-id="51b43-483">RegisterClientScriptBlock</span><span class="sxs-lookup"><span data-stu-id="51b43-483">RegisterClientScriptBlock</span></span>

<span data-ttu-id="51b43-484">Tato metoda skript přidá do horní části na vykreslené stránce.</span><span class="sxs-lookup"><span data-stu-id="51b43-484">This method adds a script to the top of the rendered page.</span></span> <span data-ttu-id="51b43-485">To je užitečné pro přidání funkcí, které bude explicitně volána na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="51b43-485">This is useful for adding functions that will be explicitly called on the client.</span></span>

<span data-ttu-id="51b43-486">Existují dvě přetížené verze této metody.</span><span class="sxs-lookup"><span data-stu-id="51b43-486">There are two overloaded versions of this method.</span></span> <span data-ttu-id="51b43-487">Tři ze čtyř argumentů jsou běžné mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="51b43-487">Three of four arguments are common among them.</span></span> <span data-ttu-id="51b43-488">Možnosti jsou následující:</span><span class="sxs-lookup"><span data-stu-id="51b43-488">They are:</span></span>

`type (string)`

<span data-ttu-id="51b43-489">***Typ*** argument identifikuje typ skriptu.</span><span class="sxs-lookup"><span data-stu-id="51b43-489">The ***type*** argument identifies a type for the script.</span></span> <span data-ttu-id="51b43-490">Obecně je vhodné použít typ stránky (to. GetType()) typu.</span><span class="sxs-lookup"><span data-stu-id="51b43-490">It is generally a good idea to use the page's type (this.GetType()) for the type.</span></span>

`key (string)`

<span data-ttu-id="51b43-491">***Klíč*** argument je klíč uživatelem definované pro skript.</span><span class="sxs-lookup"><span data-stu-id="51b43-491">The ***key*** argument is a user-defined key for the script.</span></span> <span data-ttu-id="51b43-492">To by měl být jedinečný pro každý skript.</span><span class="sxs-lookup"><span data-stu-id="51b43-492">This should be unique for each script.</span></span> <span data-ttu-id="51b43-493">Pokud se pokusíte přidat skript se stejným klíčem a typ skriptu, který již přidané, nebude přidáno.</span><span class="sxs-lookup"><span data-stu-id="51b43-493">If you attempt to add a script with the same key and type of an already added script, it will not be added.</span></span>

`script (string)`

<span data-ttu-id="51b43-494">***Skriptu*** argument je řetězec obsahující skutečné skript, který chcete přidat.</span><span class="sxs-lookup"><span data-stu-id="51b43-494">The ***script*** argument is a string containing the actual script to add.</span></span> <span data-ttu-id="51b43-495">Doporučujeme použít k vytvoření skriptu a potom použijte metodu ToString() na StringBuilder přiřadit StringBuilder ***skriptu*** argument.</span><span class="sxs-lookup"><span data-stu-id="51b43-495">It's recommended that you use a StringBuilder to create the script and then use the ToString() method on the StringBuilder to assign the ***script*** argument.</span></span>

<span data-ttu-id="51b43-496">Pokud používáte přetížené RegisterClientScriptBlock, který má jenom tři argumenty, je nutné zahrnout elementů skriptu (&lt;skriptu&gt; a &lt;/script&gt;) ve vašem skriptu.</span><span class="sxs-lookup"><span data-stu-id="51b43-496">If you use the overloaded RegisterClientScriptBlock that only takes three arguments, you must include script elements (&lt;script&gt; and &lt;/script&gt;) in your script.</span></span>

<span data-ttu-id="51b43-497">Můžete použít přetížení RegisterClientScriptBlock, která přebírá čtvrtý argument.</span><span class="sxs-lookup"><span data-stu-id="51b43-497">You may choose to use the overload of RegisterClientScriptBlock that takes a fourth argument.</span></span> <span data-ttu-id="51b43-498">Poslední argument je logická hodnota, která určuje, zda technologie ASP.NET měli přidat elementů skriptu pro vás.</span><span class="sxs-lookup"><span data-stu-id="51b43-498">The fourth argument is a Boolean that specifies whether or not ASP.NET should add script elements for you.</span></span> <span data-ttu-id="51b43-499">Pokud tento argument je **true**, váš skript nesmí obsahovat elementy skriptu explicitně.</span><span class="sxs-lookup"><span data-stu-id="51b43-499">If this argument is **true**, your script should not include the script elements explicitly.</span></span>

<span data-ttu-id="51b43-500">Použijte metodu IsClientScriptBlockRegistered k určení, pokud skript už je zaregistrovaný.</span><span class="sxs-lookup"><span data-stu-id="51b43-500">Use the IsClientScriptBlockRegistered method to determine if a script has already been registered.</span></span> <span data-ttu-id="51b43-501">To umožňuje vyhnout pokus znovu registrovat skript, který je již zaregistrován.</span><span class="sxs-lookup"><span data-stu-id="51b43-501">This allows you to avoid an attempt to re-register a script that has already been registered.</span></span>

### <a name="registerclientscriptinclude-new-in-20"></a><span data-ttu-id="51b43-502">RegisterClientScriptInclude (nové ve verzi 2.0)</span><span class="sxs-lookup"><span data-stu-id="51b43-502">RegisterClientScriptInclude (New in 2.0)</span></span>

<span data-ttu-id="51b43-503">Značky RegisterClientScriptInclude vytvoří blok skriptu, který odkazuje na soubor externího skriptu.</span><span class="sxs-lookup"><span data-stu-id="51b43-503">The RegisterClientScriptInclude tag creates a script block that links to an external script file.</span></span> <span data-ttu-id="51b43-504">Má dva přetížení.</span><span class="sxs-lookup"><span data-stu-id="51b43-504">It has two overloads.</span></span> <span data-ttu-id="51b43-505">Má klíč a adresu URL.</span><span class="sxs-lookup"><span data-stu-id="51b43-505">One takes a key and a URL.</span></span> <span data-ttu-id="51b43-506">Přidá druhý třetí argument určení typu.</span><span class="sxs-lookup"><span data-stu-id="51b43-506">The second adds a third argument specifying the type.</span></span>

<span data-ttu-id="51b43-507">Následující kód například vygeneruje blok skriptu odkazující na jsfunctions.js v kořenové složce skripty aplikace:</span><span class="sxs-lookup"><span data-stu-id="51b43-507">For example, the following code generates a script block that links to jsfunctions.js in the root of the scripts folder of the application:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

<span data-ttu-id="51b43-508">Tento kód vytvoří následující kód na vykreslené stránce:</span><span class="sxs-lookup"><span data-stu-id="51b43-508">This code produces the following code in the rendered page:</span></span>

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> <span data-ttu-id="51b43-509">Blok skriptu je vykreslen v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="51b43-509">The script block is rendered at the bottom of the page.</span></span>


<span data-ttu-id="51b43-510">Použijte metodu IsClientScriptIncludeRegistered k určení, pokud skript už je zaregistrovaný.</span><span class="sxs-lookup"><span data-stu-id="51b43-510">Use the IsClientScriptIncludeRegistered method to determine if a script has already been registered.</span></span> <span data-ttu-id="51b43-511">To umožňuje vyhnout pokus znovu registrovat skript.</span><span class="sxs-lookup"><span data-stu-id="51b43-511">This allows you to avoid an attempt to re-register a script.</span></span>

## <a name="registerstartupscript"></a><span data-ttu-id="51b43-512">RegisterStartupScript</span><span class="sxs-lookup"><span data-stu-id="51b43-512">RegisterStartupScript</span></span>

<span data-ttu-id="51b43-513">Metoda RegisterStartupScript přijímá stejné argumenty jako metodu RegisterClientScriptBlock.</span><span class="sxs-lookup"><span data-stu-id="51b43-513">The RegisterStartupScript method takes the same arguments as the RegisterClientScriptBlock method.</span></span> <span data-ttu-id="51b43-514">Skript zaregistrována RegisterStartupScript spustí po načtení stránky, ale před události při načtení straně klienta.</span><span class="sxs-lookup"><span data-stu-id="51b43-514">A script registered with RegisterStartupScript executes after the page loads but before the OnLoad client-side event.</span></span> <span data-ttu-id="51b43-515">V 1.X, skripty, které jsou registrovány RegisterStartupScript byly umístěny těsně před uzavírací &lt;/form&gt; značky při skripty, které jsou registrovány RegisterClientScriptBlock byly umístěny ihned po otevření &lt;formuláře&gt; značky.</span><span class="sxs-lookup"><span data-stu-id="51b43-515">In 1.X, scripts registered with RegisterStartupScript were placed just before the closing &lt;/form&gt; tag while scripts registered with RegisterClientScriptBlock were placed immediately after the opening &lt;form&gt; tag.</span></span> <span data-ttu-id="51b43-516">V aplikaci ASP.NET 2.0, jak jsou umisťovaný bezprostředně před uzavírací &lt;/form&gt; značky.</span><span class="sxs-lookup"><span data-stu-id="51b43-516">In ASP.NET 2.0, both are placed immediately before the closing &lt;/form&gt; tag.</span></span>

> [!NOTE]
> <span data-ttu-id="51b43-517">Když si zaregistrujete funkci s RegisterStartupScript, nebude této funkce spustit, dokud ji explicitně volání v kódu na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="51b43-517">If you register a function with RegisterStartupScript, that function will not execute until you explicitly call it in client-side code.</span></span>


<span data-ttu-id="51b43-518">Použijte metodu IsStartupScriptRegistered k určení, pokud již byl zaregistrován skript a vyhnout se pokus znovu registrovat skript.</span><span class="sxs-lookup"><span data-stu-id="51b43-518">Use the IsStartupScriptRegistered method to determine if a script has already been registered and avoid an attempt to re-register a script.</span></span>

## <a name="other-clientscriptmanager-methods"></a><span data-ttu-id="51b43-519">Ostatní metody ClientScriptManager</span><span class="sxs-lookup"><span data-stu-id="51b43-519">Other ClientScriptManager Methods</span></span>

<span data-ttu-id="51b43-520">Zde jsou některé další užitečné metody třídy ClientScriptManager.</span><span class="sxs-lookup"><span data-stu-id="51b43-520">Here are some of the other useful methods of the ClientScriptManager class.</span></span>

| <span data-ttu-id="51b43-521">**GetCallbackEventReference**</span><span class="sxs-lookup"><span data-stu-id="51b43-521">**GetCallbackEventReference**</span></span> | <span data-ttu-id="51b43-522">Viz zpětná volání skriptu dříve v tomto modulu.</span><span class="sxs-lookup"><span data-stu-id="51b43-522">See script callbacks earlier in this module.</span></span> |
| --- | --- |
| <span data-ttu-id="51b43-523">**GetPostBackClientHyperlink**</span><span class="sxs-lookup"><span data-stu-id="51b43-523">**GetPostBackClientHyperlink**</span></span> | <span data-ttu-id="51b43-524">Získá referenční dokumentace technologie JavaScript (javascript:&lt;volání&gt;), lze použít ke zveřejnění zpět z událostí na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="51b43-524">Gets a JavaScript reference (javascript:&lt;call&gt;) that can be used to post back from a client-side event.</span></span> |
| <span data-ttu-id="51b43-525">**GetPostBackEventReference**</span><span class="sxs-lookup"><span data-stu-id="51b43-525">**GetPostBackEventReference**</span></span> | <span data-ttu-id="51b43-526">Získá řetězec, který slouží k zahájení post zpět od klienta.</span><span class="sxs-lookup"><span data-stu-id="51b43-526">Gets a string that can be used to initiate a post back from the client.</span></span> |
| <span data-ttu-id="51b43-527">**GetWebResourceUrl**</span><span class="sxs-lookup"><span data-stu-id="51b43-527">**GetWebResourceUrl**</span></span> | <span data-ttu-id="51b43-528">Vrátí adresu URL k prostředku, který je součástí sestavení.</span><span class="sxs-lookup"><span data-stu-id="51b43-528">Returns a URL to a resource that is embedded in an assembly.</span></span> <span data-ttu-id="51b43-529">Musí použít ve spojení s **RegisterClientScriptResource**.</span><span class="sxs-lookup"><span data-stu-id="51b43-529">Must be used in conjunction with **RegisterClientScriptResource**.</span></span> |
| <span data-ttu-id="51b43-530">**RegisterClientScriptResource**</span><span class="sxs-lookup"><span data-stu-id="51b43-530">**RegisterClientScriptResource**</span></span> | <span data-ttu-id="51b43-531">Registruje prostředek webové stránky.</span><span class="sxs-lookup"><span data-stu-id="51b43-531">Registers a Web resource with the page.</span></span> <span data-ttu-id="51b43-532">Toto jsou prostředky vložené do sestavení a zpracovávat nové WebResource.axd obslužnou rutinou.</span><span class="sxs-lookup"><span data-stu-id="51b43-532">These are resources embedded in an assembly and handled by the new WebResource.axd handler.</span></span> |
| <span data-ttu-id="51b43-533">**RegisterHiddenField**</span><span class="sxs-lookup"><span data-stu-id="51b43-533">**RegisterHiddenField**</span></span> | <span data-ttu-id="51b43-534">Zaregistruje skryté pole formuláře se stránkou.</span><span class="sxs-lookup"><span data-stu-id="51b43-534">Registers a hidden form field with the page.</span></span> |
| <span data-ttu-id="51b43-535">**RegisterOnSubmitStatement**</span><span class="sxs-lookup"><span data-stu-id="51b43-535">**RegisterOnSubmitStatement**</span></span> | <span data-ttu-id="51b43-536">Zaregistruje kód na straně klienta, který se spustí po odeslání formuláře HTML.</span><span class="sxs-lookup"><span data-stu-id="51b43-536">Registers client-side code that executes when the HTML form is submitted.</span></span> |
