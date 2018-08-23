---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: Model 2.0 stránky ASP.NET | Dokumentace Microsoftu
author: microsoft
description: V technologii ASP.NET 1.x, vývojáři měli možnost volby mezi model pomocí vloženého kódu a model kódu použití modelu code-behind. Použití modelu Code-behind může implementovat s využitím buď Src attr...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: 4452169a01276cbc60f2a2057e6b560022ccd7c0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756397"
---
<a name="the-aspnet-20-page-model"></a><span data-ttu-id="6cbf3-104">Model 2.0 stránky ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6cbf3-104">The ASP.NET 2.0 Page Model</span></span>
====================
<span data-ttu-id="6cbf3-105">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="6cbf3-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="6cbf3-106">V technologii ASP.NET 1.x, vývojáři měli možnost volby mezi model pomocí vloženého kódu a model kódu použití modelu code-behind.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-106">In ASP.NET 1.x, developers had a choice between an inline code model and a code-behind code model.</span></span> <span data-ttu-id="6cbf3-107">Použití modelu Code-behind může být implementovaná pomocí atributu Src nebo atribut CodeBehind @Page směrnice.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-107">Code-behind could be implemented using either the Src attribute or the CodeBehind attribute of the @Page directive.</span></span> <span data-ttu-id="6cbf3-108">V technologii ASP.NET 2.0 vývojáři stále mít možnost volby mezi vloženého kódu a použití modelu code-behind, ale došlo k použití modelu code-behind modelu významná vylepšení.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-108">In ASP.NET 2.0, developers still have a choice between inline code and code-behind, but there have been significant enhancements to the code-behind model.</span></span>


<span data-ttu-id="6cbf3-109">V technologii ASP.NET 1.x, vývojáři měli možnost volby mezi model pomocí vloženého kódu a model kódu použití modelu code-behind.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-109">In ASP.NET 1.x, developers had a choice between an inline code model and a code-behind code model.</span></span> <span data-ttu-id="6cbf3-110">Použití modelu Code-behind může být implementovaná pomocí atributu Src nebo atribut CodeBehind @Page směrnice.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-110">Code-behind could be implemented using either the Src attribute or the CodeBehind attribute of the @Page directive.</span></span> <span data-ttu-id="6cbf3-111">V technologii ASP.NET 2.0 vývojáři stále mít možnost volby mezi vloženého kódu a použití modelu code-behind, ale došlo k použití modelu code-behind modelu významná vylepšení.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-111">In ASP.NET 2.0, developers still have a choice between inline code and code-behind, but there have been significant enhancements to the code-behind model.</span></span>

## <a name="improvements-in-the-code-behind-model"></a><span data-ttu-id="6cbf3-112">Vylepšení v modelu kódu</span><span class="sxs-lookup"><span data-stu-id="6cbf3-112">Improvements in the Code-Behind Model</span></span>

<span data-ttu-id="6cbf3-113">Chcete-li plně pochopit změny v modelu kódu v technologii ASP.NET 2.0, je nejlepší rychle prohlédnout modelu jako jeho existoval v technologii ASP.NET 1.x.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-113">In order to fully understand the changes in the code-behind model in ASP.NET 2.0, its best to quickly review the model as it existed in ASP.NET 1.x.</span></span>

## <a name="the-code-behind-model-in-aspnet-1x"></a><span data-ttu-id="6cbf3-114">Model použití modelu Code-Behind v technologii ASP.NET 1.x</span><span class="sxs-lookup"><span data-stu-id="6cbf3-114">The Code-Behind Model in ASP.NET 1.x</span></span>

<span data-ttu-id="6cbf3-115">V technologii ASP.NET 1.x, model použití modelu code-behind se skládal z souboru ASPX (webového formuláře) a použití modelu code-behind soubor obsahující programového kódu.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-115">In ASP.NET 1.x, the code-behind model consisted of an ASPX file (the Webform) and a code-behind file containing programming code.</span></span> <span data-ttu-id="6cbf3-116">Příslušné dva soubory byly připojené pomocí @Page direktivy v souboru ASPX.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-116">The two files were connected using the @Page directive in the ASPX file.</span></span> <span data-ttu-id="6cbf3-117">Každý ovládací prvek na stránce ASPX měl odpovídající deklarace v souboru kódu na pozadí jako proměnná instance.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-117">Each control on the ASPX page had a corresponding declaration in the code-behind file as an instance variable.</span></span> <span data-ttu-id="6cbf3-118">Soubor kódu na pozadí také obsahovala kód pro navázání událostí a generovaného kódu, které jsou nezbytné pro návrháře aplikace Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-118">The code-behind file also contained code for event binding and generated code necessary for the Visual Studio designer.</span></span> <span data-ttu-id="6cbf3-119">Tento model poměrně dobře fungovalo, ale protože každý prvek technologie ASP.NET na stránce ASPX vyžaduje odpovídající kód v souboru kódu na pozadí, neexistuje žádná true oddělení kódu a obsahu.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-119">This model worked fairly well, but because every ASP.NET element in the ASPX page required corresponding code in the code-behind file, there was no true separation of code and content.</span></span> <span data-ttu-id="6cbf3-120">Například pokud návrháře do souboru ASPX mimo rozhraní IDE sady Visual Studio přidá nový serverový ovládací prvek, aplikace by narušil vzhledem k absenci deklarace pro tento ovládací prvek v souboru kódu na pozadí.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-120">For example, if a designer added a new server control to an ASPX file outside of the Visual Studio IDE, the application would break due to the absence of a declaration for that control in the code-behind file.</span></span>

## <a name="the-code-behind-model-in-aspnet-20"></a><span data-ttu-id="6cbf3-121">Model použití modelu Code-Behind v technologii ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="6cbf3-121">The Code-Behind Model in ASP.NET 2.0</span></span>

<span data-ttu-id="6cbf3-122">ASP.NET 2.0 výrazně vylepšuje tento model.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-122">ASP.NET 2.0 greatly improves upon this model.</span></span> <span data-ttu-id="6cbf3-123">V technologii ASP.NET 2.0, je implementováno pomocí nového modelu code-behind *částečné třídy* v technologii ASP.NET 2.0 k dispozici.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-123">In ASP.NET 2.0, code-behind is implemented using the new *partial classes* provided in ASP.NET 2.0.</span></span> <span data-ttu-id="6cbf3-124">Použití modelu code-behind třída v technologii ASP.NET 2.0 je definován jako částečné třídy, což znamená, že obsahuje pouze část definice třídy.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-124">The code-behind class in ASP.NET 2.0 is definied as a partial class meaning that it contains only part of the class definition.</span></span> <span data-ttu-id="6cbf3-125">Zbývající část definice třídy generuje dynamicky pomocí technologie ASP.NET 2.0 pomocí stránky ASPX za běhu nebo na webu je předkompilována.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-125">The remaining part of the class definition is dynamically generated by ASP.NET 2.0 using the ASPX page at runtime or when the Web site is precompiled.</span></span> <span data-ttu-id="6cbf3-126">Propojení mezi souborem kódu na pozadí a stránku ASPX stále se naváže s využitím – Direktiva @ Page.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-126">The link between the code-behind file and the ASPX page is still established using the @ Page directive.</span></span> <span data-ttu-id="6cbf3-127">Místo atribut CodeBehind nebo Src, je však ASP.NET 2.0 nyní používá atribut CodeFile.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-127">However, instead of a CodeBehind or Src attribute, ASP.NET 2.0 now uses the CodeFile attribute.</span></span> <span data-ttu-id="6cbf3-128">Atribut Inherits je také použít k určení názvu třídy stránky.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-128">The Inherits attribute is also used to specify the class name for the page.</span></span>

<span data-ttu-id="6cbf3-129">Typické – Direktiva @ Page může vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="6cbf3-129">A typical @ Page directive might look like this:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

<span data-ttu-id="6cbf3-130">Definice typické třídy v souboru kódu ASP.NET 2.0 může vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="6cbf3-130">A typical class definition in an ASP.NET 2.0 code-behind file might look like this:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="6cbf3-131">C# a Visual Basic jsou pouze spravované jazyky, které aktuálně podporují částečné třídy.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-131">C# and Visual Basic are the only managed languages that currently support partial classes.</span></span> <span data-ttu-id="6cbf3-132">Proto vývojáři, kteří používají J# nebude možné použít model použití modelu code-behind v technologii ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-132">Therefore, developers using J# will not be able to use the code-behind model in ASP.NET 2.0.</span></span>


<span data-ttu-id="6cbf3-133">Nový model zvyšuje model použití modelu code-behind, protože vývojáři budou mít soubory kódu, které obsahují pouze kód, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-133">The new model enhances the code-behind model because developers will now have code files that contain only the code that they have created.</span></span> <span data-ttu-id="6cbf3-134">Také nabízí true oddělení kódu a obsahu vzhledem k tomu, že neexistují žádné instance deklarace proměnných v souboru kódu na pozadí.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-134">It also provides for a true separation of code and content because there are no instance variable declarations in the code-behind file.</span></span>

> [!NOTE]
> <span data-ttu-id="6cbf3-135">Částečné třídy pro stránku ASPX je, kde vazby události dojde, vývojáře jazyka Visual Basic můžete realizovat zvýšení snížený výkon pomocí klíčového slova popisovače v modelu code-behind svázat události.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-135">Because the partial class for the ASPX page is where event binding takes place, Visual Basic developers can realize a slight performance increase by using the Handles keyword in code-behind to bind events.</span></span> <span data-ttu-id="6cbf3-136">C# nemá žádný ekvivalent klíčového slova.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-136">C# has no equivalent keyword.</span></span>


## <a name="new--page-directive-attributes"></a><span data-ttu-id="6cbf3-137">Nové atributy @ Page – direktiva</span><span class="sxs-lookup"><span data-stu-id="6cbf3-137">New @ Page Directive Attributes</span></span>

<span data-ttu-id="6cbf3-138">ASP.NET 2.0 přidá mnoho nových atributů – Direktiva @ Page.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-138">ASP.NET 2.0 adds many new attributes to the @ Page directive.</span></span> <span data-ttu-id="6cbf3-139">Následující atributy jsou v technologii ASP.NET verze 2.0 nový.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-139">The following attributes are new in ASP.NET 2.0.</span></span>

## <a name="async"></a><span data-ttu-id="6cbf3-140">Async</span><span class="sxs-lookup"><span data-stu-id="6cbf3-140">Async</span></span>

<span data-ttu-id="6cbf3-141">Atribut Async umožňuje nakonfigurovat stránky, který se spustí asynchronně.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-141">The Async attribute allows you to configure page to be executed asynchronously.</span></span> <span data-ttu-id="6cbf3-142">Dobře titulní asynchronní stránky později v tomto modulu.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-142">Well cover asynchronous pages later in this module.</span></span>

## <a name="asynctimeout"></a><span data-ttu-id="6cbf3-143">Hodnota vlastnosti AsyncTimeout</span><span class="sxs-lookup"><span data-stu-id="6cbf3-143">AsyncTimeout</span></span>

<span data-ttu-id="6cbf3-144">Zadaný časový limit pro asynchronní stránky.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-144">Specified the timeout for asynchronous pages.</span></span> <span data-ttu-id="6cbf3-145">Výchozí hodnota je 45 sekund.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-145">The default is 45 seconds.</span></span>

## <a name="codefile"></a><span data-ttu-id="6cbf3-146">CodeFile</span><span class="sxs-lookup"><span data-stu-id="6cbf3-146">CodeFile</span></span>

<span data-ttu-id="6cbf3-147">Atribut CodeFile je náhradou pro atribut CodeBehind v aplikaci Visual Studio 2002/2003.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-147">The CodeFile attribute is the replacement for the CodeBehind attribute in Visual Studio 2002/2003.</span></span>

### <a name="codefilebaseclass"></a><span data-ttu-id="6cbf3-148">CodeFileBaseClass</span><span class="sxs-lookup"><span data-stu-id="6cbf3-148">CodeFileBaseClass</span></span>

<span data-ttu-id="6cbf3-149">Atribut CodeFileBaseClass se používá v případech, kde má více stránky, které jsou odvozeny z jediné základní třídy.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-149">The CodeFileBaseClass attribute is used in cases where you want multiple pages to derive from a single base class.</span></span> <span data-ttu-id="6cbf3-150">Z důvodu implementace částečné třídy v technologii ASP.NET, bez tohoto atributu základní třídu, která používá sdílené společných polí tak, aby odkazovaly ovládací prvky deklarován na stránce ASPX nebude fungovat správně, protože ASP. Modul kompilace sítě automaticky vytvoří nové členy v závislosti na ovládací prvky na stránce.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-150">Because of the implementation of partial classes in ASP.NET, without this attribute, a base class that uses shared common fields to reference controls declared in an ASPX page would not work properly because ASP.NETs compilation engine will automatically create new members based on controls in the page.</span></span> <span data-ttu-id="6cbf3-151">Proto pokud chcete obecná základní třída pro dva nebo více stránek v ASP.NET, budete muset definovat v Atribut CodeFileBaseClass zadat základní třídy a pak jsou odvozeny jednotlivé třídy stránky z této základní třídy.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-151">Therefore, if you want a common base class for two or more pages in ASP.NET, you will need to define specify your base class in the CodeFileBaseClass attribute and then derive each pages class from that base class.</span></span> <span data-ttu-id="6cbf3-152">Atribut CodeFile je také nutný, pokud tento atribut se používá.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-152">The CodeFile attribute is also required when this attribute is used.</span></span>

## <a name="compilationmode"></a><span data-ttu-id="6cbf3-153">Vlastnost CompilationMode</span><span class="sxs-lookup"><span data-stu-id="6cbf3-153">CompilationMode</span></span>

<span data-ttu-id="6cbf3-154">Tento atribut umožňuje nastavit vlastnost CompilationMode stránky ASPX.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-154">This attribute allows you to set the CompilationMode property of the ASPX page.</span></span> <span data-ttu-id="6cbf3-155">Vlastnost CompilationMode je výčet, který obsahuje hodnoty **vždy**, **automaticky**, a **nikdy**.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-155">The CompilationMode property is an enumeration containing the values **Always**, **Auto**, and **Never**.</span></span> <span data-ttu-id="6cbf3-156">Výchozí hodnota je **vždy**.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-156">The default is **Always**.</span></span> <span data-ttu-id="6cbf3-157">**Automaticky** nastavení zabrání ASP.NET Dynamická kompilace na stránce, pokud je to možné.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-157">The **Auto** setting will prevent ASP.NET from dynamically compiling the page if possible.</span></span> <span data-ttu-id="6cbf3-158">Kromě stránek v dynamických kompilačních zvyšuje výkon.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-158">Excluding pages from dynamic compilation increases performance.</span></span> <span data-ttu-id="6cbf3-159">Ale stránka, která je vyloučená obsahuje tento kód, který musí být zkompilovány, chybu bude vyvolána výjimka při prohlížení stránky.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-159">However, if a page that is excluded contains that code that must be compiled, an error will be thrown when the page is browsed.</span></span>

## <a name="enableeventvalidation"></a><span data-ttu-id="6cbf3-160">EnableEventValidation</span><span class="sxs-lookup"><span data-stu-id="6cbf3-160">EnableEventValidation</span></span>

<span data-ttu-id="6cbf3-161">Tento atribut určuje, zda jsou ověřeny události zpětného odeslání a zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-161">This attribute specifies whether or not postback and callback events are validated.</span></span> <span data-ttu-id="6cbf3-162">Pokud je tato možnost povolena, argumenty odeslání nebo zpětného volání události jsou kontrola, že pocházejí z ovládacího prvku serveru, který je původně vykreslil.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-162">When this is enabled, arguments to postback or callback events are checked to ensure that they originated from the server control that originally rendered them.</span></span>

## <a name="enabletheming"></a><span data-ttu-id="6cbf3-163">EnableTheming</span><span class="sxs-lookup"><span data-stu-id="6cbf3-163">EnableTheming</span></span>

<span data-ttu-id="6cbf3-164">Tento atribut určuje, zda jsou na stránce použít motivů ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-164">This attribute specifies whether or not ASP.NET themes are used on a page.</span></span> <span data-ttu-id="6cbf3-165">Výchozí hodnota je **false**.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-165">The default is **false**.</span></span> <span data-ttu-id="6cbf3-166">Motivů ASP.NET jsou popsané v [modulu 10](profiles-themes-and-web-parts.md).</span><span class="sxs-lookup"><span data-stu-id="6cbf3-166">ASP.NET themes are covered in [Module 10](profiles-themes-and-web-parts.md).</span></span>

## <a name="linepragmas"></a><span data-ttu-id="6cbf3-167">LinePragmas</span><span class="sxs-lookup"><span data-stu-id="6cbf3-167">LinePragmas</span></span>

<span data-ttu-id="6cbf3-168">Tento atribut určuje, zda řádkové direktivy pragma by měl být přidány během kompilace.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-168">This attribute specifies whether line pragmas should be added during compilation.</span></span> <span data-ttu-id="6cbf3-169">Řádkové direktivy pragma jsou možnosti, které používají ladicí programy k označení určité části kódu.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-169">Line pragmas are options used by debuggers to mark specific sections of code.</span></span>

## <a name="maintainscrollpositiononpostback"></a><span data-ttu-id="6cbf3-170">MaintainScrollPositionOnPostback</span><span class="sxs-lookup"><span data-stu-id="6cbf3-170">MaintainScrollPositionOnPostback</span></span>

<span data-ttu-id="6cbf3-171">Tento atribut určuje, zda jazyka JavaScript se vloží do stránky pro zachování pozice posuvníku mezi jednotlivými zpětnými odesláními.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-171">This attribute specifies whether or not JavaScript is injected into the page in order to maintain scroll position between postbacks.</span></span> <span data-ttu-id="6cbf3-172">Tento atribut je **false** ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-172">This attribute is **false** by default.</span></span>

<span data-ttu-id="6cbf3-173">Pokud tento atribut je **true**, přidá technologie ASP.NET &lt;skript&gt; blok na zpětné volání, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="6cbf3-173">When this attribute is **true**, ASP.NET will add a &lt;script&gt; block on postback that looks like this:</span></span>

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

<span data-ttu-id="6cbf3-174">Všimněte si, že je src pro tento blok skriptu WebResource.axd.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-174">Note that the src for this script block is WebResource.axd.</span></span> <span data-ttu-id="6cbf3-175">Tento prostředek není fyzická cesta.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-175">This resource is not a physical path.</span></span> <span data-ttu-id="6cbf3-176">Pokud tento skript se požaduje, ASP.NET dynamicky vytvoří skript.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-176">When this script is requested, ASP.NET dynamically builds the script.</span></span>

### <a name="masterpagefile"></a><span data-ttu-id="6cbf3-177">MasterPageFile</span><span class="sxs-lookup"><span data-stu-id="6cbf3-177">MasterPageFile</span></span>

<span data-ttu-id="6cbf3-178">Tento atribut určuje soubor předlohové stránky pro aktuální stránku.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-178">This attribute specifies the master page file for the current page.</span></span> <span data-ttu-id="6cbf3-179">Cesta může být relativní nebo absolutní.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-179">The path can be relative or absolute.</span></span> <span data-ttu-id="6cbf3-180">Stránky předlohy se věnují [modulu 4](master-pages.md).</span><span class="sxs-lookup"><span data-stu-id="6cbf3-180">Master pages are covered in [Module 4](master-pages.md).</span></span>

## <a name="stylesheettheme"></a><span data-ttu-id="6cbf3-181">Vlastnost StyleSheetTheme</span><span class="sxs-lookup"><span data-stu-id="6cbf3-181">StyleSheetTheme</span></span>

<span data-ttu-id="6cbf3-182">Tento atribut umožňuje přepsat vlastnosti vzhled uživatelského rozhraní definované motiv ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-182">This attribute allows you to override user-interface appearance properties defined by an ASP.NET 2.0 theme.</span></span> <span data-ttu-id="6cbf3-183">Motivy jsou popsané v [modulu 10](profiles-themes-and-web-parts.md).</span><span class="sxs-lookup"><span data-stu-id="6cbf3-183">Themes are covered in [Module 10](profiles-themes-and-web-parts.md).</span></span>

## <a name="theme"></a><span data-ttu-id="6cbf3-184">Motiv</span><span class="sxs-lookup"><span data-stu-id="6cbf3-184">Theme</span></span>

<span data-ttu-id="6cbf3-185">Určuje motivu stránky.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-185">Specifies the theme for the page.</span></span> <span data-ttu-id="6cbf3-186">Pokud není zadána hodnota pro atribut StyleSheetTheme, přepíše atribut motivu všechny styly použít na ovládací prvky na stránce.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-186">If a value is not specified for the StyleSheetTheme attribute, the Theme attribute overrides all styles applied to controls on the page.</span></span>

## <a name="title"></a><span data-ttu-id="6cbf3-187">Název</span><span class="sxs-lookup"><span data-stu-id="6cbf3-187">Title</span></span>

<span data-ttu-id="6cbf3-188">Nastaví název stránky.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-188">Sets the title for the page.</span></span> <span data-ttu-id="6cbf3-189">Hodnota zadaná v tomto poli se zobrazí v &lt;název&gt; prvek vykreslené stránky.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-189">The value specified here will appear in the &lt;title&gt; element of the rendered page.</span></span>

### <a name="viewstateencryptionmode"></a><span data-ttu-id="6cbf3-190">ViewStateEncryptionMode</span><span class="sxs-lookup"><span data-stu-id="6cbf3-190">ViewStateEncryptionMode</span></span>

<span data-ttu-id="6cbf3-191">Nastaví hodnotu pro výčet ViewStateEncryptionMode.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-191">Sets the value for the ViewStateEncryptionMode enumeration.</span></span> <span data-ttu-id="6cbf3-192">Dostupné hodnoty jsou **vždy**, **automaticky**, a **nikdy**.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-192">The available values are **Always**, **Auto**, and **Never**.</span></span> <span data-ttu-id="6cbf3-193">Výchozí hodnota je **automaticky**. Když tento atribut je nastaven na hodnotu **automaticky**, stav zobrazení je zašifrovaný je ovládací prvek požaduje voláním **RegisterRequiresViewStateEncryption** metoda.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-193">The default value is **Auto**. When this attribute is set to a value of **Auto**, viewstate is encrypted is a control requests it by calling the **RegisterRequiresViewStateEncryption** method.</span></span>

## <a name="setting-public-property-values-via-the--page-directive"></a><span data-ttu-id="6cbf3-194">Nastavení hodnot veřejnou vlastnost prostřednictvím @ direktiva stránky</span><span class="sxs-lookup"><span data-stu-id="6cbf3-194">Setting Public Property Values via the @ Page Directive</span></span>

<span data-ttu-id="6cbf3-195">Další nová funkce – Direktiva @ Page v technologii ASP.NET 2.0 je možnost nastavit počáteční hodnotu veřejných vlastností základní třídy.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-195">Another new capability of the @ Page directive in ASP.NET 2.0 is the ability to set the initial value of public properties of a base class.</span></span> <span data-ttu-id="6cbf3-196">Předpokládejme například, že máte veřejnou vlastnost názvem **SomeText** v základní třídy a d líbí se vám mají být inicializovány na **Hello** při načtení stránky.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-196">Suppose, for example, that you have a public property called **SomeText** in your base class and you d like it to be initialized to **Hello** when a page is loaded.</span></span> <span data-ttu-id="6cbf3-197">Toho lze dosáhnout jednoduše nastavením hodnoty v Direktiva @ Page takto:</span><span class="sxs-lookup"><span data-stu-id="6cbf3-197">You can accomplish this by simply setting the value in the @ Page directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

<span data-ttu-id="6cbf3-198">**SomeText** atribut – Direktiva @ Page nastaví počáteční hodnotu vlastnosti SomeText v základní třídě pro *Hello!*.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-198">The **SomeText** attribute of the @ Page directive sets the initial value of the SomeText property in the base class to *Hello!*.</span></span> <span data-ttu-id="6cbf3-199">Následující video je návod, nastaví počáteční hodnotu veřejnou vlastnost v základní třídě – Direktiva @ Page.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-199">The video below is a walkthrough of setting the initial value of a public property in a base class using the @ Page directive.</span></span>


![](the-asp-net-2-0-page-model/_static/image1.png)


[<span data-ttu-id="6cbf3-200">Otevřít Video na celou obrazovku</span><span class="sxs-lookup"><span data-stu-id="6cbf3-200">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/setprop1.wmv)


## <a name="new-public-properties-of-the-page-class"></a><span data-ttu-id="6cbf3-201">Nové veřejné vlastnosti třídy stránky</span><span class="sxs-lookup"><span data-stu-id="6cbf3-201">New Public Properties of the Page Class</span></span>

<span data-ttu-id="6cbf3-202">Následující veřejné vlastnosti jsou v technologii ASP.NET verze 2.0 nový.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-202">The following public properties are new in ASP.NET 2.0.</span></span>

## <a name="apprelativetemplatesourcedirectory"></a><span data-ttu-id="6cbf3-203">AppRelativeTemplateSourceDirectory</span><span class="sxs-lookup"><span data-stu-id="6cbf3-203">AppRelativeTemplateSourceDirectory</span></span>

<span data-ttu-id="6cbf3-204">Stránka nebo ovládací prvek vrátí aplikace relativní cestu.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-204">Returns the application-relative path to the page or control.</span></span> <span data-ttu-id="6cbf3-205">Například pro stránku se nachází na http://app/folder/page.aspx, vrátí vlastnost ~ / slozka /.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-205">For example, for a page located at http://app/folder/page.aspx, the property returns ~/folder/.</span></span>

## <a name="apprelativevirtualpath"></a><span data-ttu-id="6cbf3-206">AppRelativeVirtualPath</span><span class="sxs-lookup"><span data-stu-id="6cbf3-206">AppRelativeVirtualPath</span></span>

<span data-ttu-id="6cbf3-207">Stránka nebo ovládací prvek vrátí relativní virtuální adresář.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-207">Returns the relative virtual directory path to the page or control.</span></span> <span data-ttu-id="6cbf3-208">Například pro stránku se nachází na http://app/folder/page.aspx, vrátí vlastnost ~ / folder/page.aspx.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-208">For example for a page located at http://app/folder/page.aspx, the property returns ~/folder/page.aspx.</span></span>

## <a name="asynctimeout"></a><span data-ttu-id="6cbf3-209">Hodnota vlastnosti AsyncTimeout</span><span class="sxs-lookup"><span data-stu-id="6cbf3-209">AsyncTimeout</span></span>

<span data-ttu-id="6cbf3-210">Získá nebo nastaví časový limit používá pro zpracování asynchronní stránky.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-210">Gets or sets the timeout used for asynchronous page handling.</span></span> <span data-ttu-id="6cbf3-211">(Asynchronní stránky se budeme dále v tomto modulu.)</span><span class="sxs-lookup"><span data-stu-id="6cbf3-211">(Asynchronous pages will be covered later in this module.)</span></span>

## <a name="clientquerystring"></a><span data-ttu-id="6cbf3-212">ClientQueryString</span><span class="sxs-lookup"><span data-stu-id="6cbf3-212">ClientQueryString</span></span>

<span data-ttu-id="6cbf3-213">Vlastnosti jen pro čtení, která vrací část řetězce dotazu požadovanou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-213">A read-only property that returns the query string portion of the requested URL.</span></span> <span data-ttu-id="6cbf3-214">Tato hodnota je kódování URL.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-214">This value is URL encoded.</span></span> <span data-ttu-id="6cbf3-215">Metoda UrlDecode třídy HttpServerUtility můžete dekódovat.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-215">You can use the UrlDecode method of the HttpServerUtility class to decode it.</span></span>

## <a name="clientscript"></a><span data-ttu-id="6cbf3-216">ClientScript</span><span class="sxs-lookup"><span data-stu-id="6cbf3-216">ClientScript</span></span>

<span data-ttu-id="6cbf3-217">Tato vlastnost vrátí objekt ClientScriptManager, který slouží ke správě ASP.NETs emise skriptu na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-217">This property returns a ClientScriptManager object that can be used to manage ASP.NETs emission of client-side script.</span></span> <span data-ttu-id="6cbf3-218">(Třída ClientScriptManager je popsané dále v tomto modulu.)</span><span class="sxs-lookup"><span data-stu-id="6cbf3-218">(The ClientScriptManager class is covered later in this module.)</span></span>

## <a name="enableeventvalidation"></a><span data-ttu-id="6cbf3-219">EnableEventValidation</span><span class="sxs-lookup"><span data-stu-id="6cbf3-219">EnableEventValidation</span></span>

<span data-ttu-id="6cbf3-220">Tato vlastnost určuje, jestli ověření události je povoleno pro události zpětného odeslání a zpětným voláním.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-220">This property controls whether or not event validation is enabled for postback and callback events.</span></span> <span data-ttu-id="6cbf3-221">Pokud povolená, argumenty odeslání nebo zpětného volání události se ověřit, ujistěte se, že pocházejí z ovládacího prvku serveru, který je původně vykreslil.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-221">When enabled, arguments to postback or callback events are verified to ensure that they originated from the server control that originally rendered them.</span></span>

## <a name="enabletheming"></a><span data-ttu-id="6cbf3-222">EnableTheming</span><span class="sxs-lookup"><span data-stu-id="6cbf3-222">EnableTheming</span></span>

<span data-ttu-id="6cbf3-223">Tato vlastnost získá nebo nastaví logickou hodnotu, která určuje, zda motiv ASP.NET 2.0 se vztahuje na stránce.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-223">This property gets or sets a Boolean that specifies whether or not an ASP.NET 2.0 theme applies to the page.</span></span>

## <a name="form"></a><span data-ttu-id="6cbf3-224">Formulář</span><span class="sxs-lookup"><span data-stu-id="6cbf3-224">Form</span></span>

<span data-ttu-id="6cbf3-225">Tato vlastnost vrátí formuláře HTML na stránce ASPX jako objekt HtmlForm.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-225">This property returns the HTML form on the ASPX page as an HtmlForm object.</span></span>

## <a name="header"></a><span data-ttu-id="6cbf3-226">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="6cbf3-226">Header</span></span>

<span data-ttu-id="6cbf3-227">Tato vlastnost vrátí odkaz na objekt HtmlHead, který obsahuje záhlaví stránky.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-227">This property returns a reference to an HtmlHead object that contains the page header.</span></span> <span data-ttu-id="6cbf3-228">Vrácený objekt HtmlHead slouží k get/set šablony stylů, metaznaček atd.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-228">You can use the returned HtmlHead object to get/set style sheets, Meta tags, etc.</span></span>

## <a name="idseparator"></a><span data-ttu-id="6cbf3-229">IdSeparator</span><span class="sxs-lookup"><span data-stu-id="6cbf3-229">IdSeparator</span></span>

<span data-ttu-id="6cbf3-230">Tato vlastnost jen pro čtení získá znak, který se používá k oddělení identifikátory ovládacího prvku při sestavuje jedinečné ID pro ovládací prvky na stránce ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-230">This read-only property gets the character that is used to separate control identifiers when ASP.NET is building a unique ID for controls on a page.</span></span> <span data-ttu-id="6cbf3-231">Není určena pro použití přímo v kódu.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-231">It is not intended to be used directly from your code.</span></span>

## <a name="isasync"></a><span data-ttu-id="6cbf3-232">Isasync:</span><span class="sxs-lookup"><span data-stu-id="6cbf3-232">IsAsync</span></span>

<span data-ttu-id="6cbf3-233">Tato vlastnost umožňuje asynchronní stránky.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-233">This property allows for asynchronous pages.</span></span> <span data-ttu-id="6cbf3-234">Asynchronní stránky jsou popsány dále v tomto modulu.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-234">Asynchronous pages are discussed later in this module.</span></span>

## <a name="iscallback"></a><span data-ttu-id="6cbf3-235">IsCallback</span><span class="sxs-lookup"><span data-stu-id="6cbf3-235">IsCallback</span></span>

<span data-ttu-id="6cbf3-236">Vrátí tato vlastnost jen pro čtení **true** Pokud na stránce je výsledkem zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-236">This read-only property returns **true** if the page is the result of a call back.</span></span> <span data-ttu-id="6cbf3-237">Zpětnými voláními jsou popsány dále v tomto modulu.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-237">Call backs are discussed later in this module.</span></span>

## <a name="iscrosspagepostback"></a><span data-ttu-id="6cbf3-238">IsCrossPagePostBack</span><span class="sxs-lookup"><span data-stu-id="6cbf3-238">IsCrossPagePostBack</span></span>

<span data-ttu-id="6cbf3-239">Vrátí tato vlastnost jen pro čtení **true** Pokud na stránce je součástí zpětné volání mezi stránkami.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-239">This read-only property returns **true** if the page is part of a cross-page postback.</span></span> <span data-ttu-id="6cbf3-240">Mezi stránkami postbacky jsou popsané dále v tomto modulu.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-240">Cross-page postbacks are covered later in this module.</span></span>

## <a name="items"></a><span data-ttu-id="6cbf3-241">Položky</span><span class="sxs-lookup"><span data-stu-id="6cbf3-241">Items</span></span>

<span data-ttu-id="6cbf3-242">Vrátí odkaz na rozhraní IDictionary instance, která obsahuje všechny objekty uložené v rámci stránky.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-242">Returns a reference to an IDictionary instance that contains all objects stored in the pages context.</span></span> <span data-ttu-id="6cbf3-243">Můžete přidat položky na tento objekt IDictionary a budou k dispozici v celém životním kontextu.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-243">You can add items to this IDictionary object and they will be available to you throughout the lifetime of the context.</span></span>

## <a name="maintainscrollpositiononpostback"></a><span data-ttu-id="6cbf3-244">MaintainScrollPositionOnPostBack</span><span class="sxs-lookup"><span data-stu-id="6cbf3-244">MaintainScrollPositionOnPostBack</span></span>

<span data-ttu-id="6cbf3-245">Tato vlastnost určuje, zda technologie ASP.NET generuje jazyka JavaScript, který udržuje, že na stránkách Posune pozici v prohlížeči po zpětném odeslání.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-245">This property controls whether or not ASP.NET emits JavaScript that maintains the pages scroll position in the browser after a postback occurs.</span></span> <span data-ttu-id="6cbf3-246">(Podrobnosti o této vlastnosti bylo popsáno dříve v tomto modulu.)</span><span class="sxs-lookup"><span data-stu-id="6cbf3-246">(Details of this property were discussed earlier in this module.)</span></span>

## <a name="master"></a><span data-ttu-id="6cbf3-247">Hlavní</span><span class="sxs-lookup"><span data-stu-id="6cbf3-247">Master</span></span>

<span data-ttu-id="6cbf3-248">Tato vlastnost jen pro čtení vrátí odkaz na instanci MasterPage pro stránku, do které byl použit na stránku předlohy.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-248">This read-only property returns a reference to the MasterPage instance for a page to which a master page has been applied.</span></span>

## <a name="masterpagefile"></a><span data-ttu-id="6cbf3-249">MasterPageFile</span><span class="sxs-lookup"><span data-stu-id="6cbf3-249">MasterPageFile</span></span>

<span data-ttu-id="6cbf3-250">Získá nebo nastaví název souboru hlavní stránky pro stránku.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-250">Gets or sets the master page filename for the page.</span></span> <span data-ttu-id="6cbf3-251">Tuto vlastnost lze nastavit pouze v metodě PreInit.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-251">This property can only be set in the PreInit method.</span></span>

## <a name="maxpagestatefieldlength"></a><span data-ttu-id="6cbf3-252">Vlastnost MaxPageStateFieldLength</span><span class="sxs-lookup"><span data-stu-id="6cbf3-252">MaxPageStateFieldLength</span></span>

<span data-ttu-id="6cbf3-253">Tato vlastnost získá nebo nastaví maximální délku stavu stránky v bajtech.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-253">This property gets or sets the maximum length for the pages state in bytes.</span></span> <span data-ttu-id="6cbf3-254">Pokud je nastavena na kladné číslo, stav zobrazení stránky bude možné rozdělit do více skrytá pole tak, aby nepřekračuje počet bajtů.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-254">If the property is set to a positive number, the pages view state will be broken up into multiple hidden fields so that it doesnt exceed the number of bytes specified.</span></span> <span data-ttu-id="6cbf3-255">Pokud je vlastnost nastavena na záporné číslo, nebude stav zobrazení rozdělen do bloků.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-255">If the property is a negative number, the view state will not be broken into chunks.</span></span>

## <a name="pageadapter"></a><span data-ttu-id="6cbf3-256">PageAdapter</span><span class="sxs-lookup"><span data-stu-id="6cbf3-256">PageAdapter</span></span>

<span data-ttu-id="6cbf3-257">Vrátí odkaz na objekt PageAdapter změní na stránce pro požadujícího prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-257">Returns a reference to the PageAdapter object that modifies the page for the requesting browser.</span></span>

## <a name="previouspage"></a><span data-ttu-id="6cbf3-258">PreviousPage</span><span class="sxs-lookup"><span data-stu-id="6cbf3-258">PreviousPage</span></span>

<span data-ttu-id="6cbf3-259">Vrátí odkaz na předchozí stránku v případech Server.Transfer nebo zpětné volání mezi stránkami.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-259">Returns a reference to the previous page in cases of a Server.Transfer or a cross-page postback.</span></span>

## <a name="skinid"></a><span data-ttu-id="6cbf3-260">Identifikátor SkinID</span><span class="sxs-lookup"><span data-stu-id="6cbf3-260">SkinID</span></span>

<span data-ttu-id="6cbf3-261">Určuje skinu ASP.NET 2.0 použít na stránce.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-261">Specifies the ASP.NET 2.0 skin to apply to the page.</span></span>

## <a name="stylesheettheme"></a><span data-ttu-id="6cbf3-262">Vlastnost StyleSheetTheme</span><span class="sxs-lookup"><span data-stu-id="6cbf3-262">StyleSheetTheme</span></span>

<span data-ttu-id="6cbf3-263">Tato vlastnost získá nebo nastaví šablony stylů, která se uplatňuje na stránku.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-263">This property gets or sets the style sheet that is applied to a page.</span></span>

## <a name="templatecontrol"></a><span data-ttu-id="6cbf3-264">Třída TemplateControl</span><span class="sxs-lookup"><span data-stu-id="6cbf3-264">TemplateControl</span></span>

<span data-ttu-id="6cbf3-265">Vrátí odkaz na nadřazený ovládací prvek pro stránku.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-265">Returns a reference to the containing control for the page.</span></span>

## <a name="theme"></a><span data-ttu-id="6cbf3-266">Motiv</span><span class="sxs-lookup"><span data-stu-id="6cbf3-266">Theme</span></span>

<span data-ttu-id="6cbf3-267">Získá nebo nastaví název motivu ASP.NET 2.0 použité pro stránku.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-267">Gets or sets the name of the ASP.NET 2.0 theme applied to the page.</span></span> <span data-ttu-id="6cbf3-268">Tato hodnota musí být nastavena před PreInit – metoda.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-268">This value must be set prior to the PreInit method.</span></span>

## <a name="title"></a><span data-ttu-id="6cbf3-269">Název</span><span class="sxs-lookup"><span data-stu-id="6cbf3-269">Title</span></span>

<span data-ttu-id="6cbf3-270">Tato vlastnost získá nebo nastaví název stránky získaný ze záhlaví stránky.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-270">This property gets or sets the title for the page as obtained from the pages header.</span></span>

## <a name="viewstateencryptionmode"></a><span data-ttu-id="6cbf3-271">ViewStateEncryptionMode</span><span class="sxs-lookup"><span data-stu-id="6cbf3-271">ViewStateEncryptionMode</span></span>

<span data-ttu-id="6cbf3-272">Získá nebo nastaví ViewStateEncryptionMode stránky.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-272">Gets or sets the ViewStateEncryptionMode of the page.</span></span> <span data-ttu-id="6cbf3-273">Podívejte se na podrobnou diskuzi o tuto vlastnost dříve v tomto modulu.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-273">See a detailed discussion of this property earlier in this module.</span></span>

## <a name="new-protected-properties-of-the-page-class"></a><span data-ttu-id="6cbf3-274">Nové chráněné vlastnosti třídy stránky</span><span class="sxs-lookup"><span data-stu-id="6cbf3-274">New Protected Properties of the Page Class</span></span>

<span data-ttu-id="6cbf3-275">Toto jsou nové chráněné vlastnosti třídy stránky v technologii ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-275">The following are the new protected properties of the Page class in ASP.NET 2.0.</span></span>

## <a name="adapter"></a><span data-ttu-id="6cbf3-276">Adaptér</span><span class="sxs-lookup"><span data-stu-id="6cbf3-276">Adapter</span></span>

<span data-ttu-id="6cbf3-277">Vrátí odkaz na ControlAdapter vykreslující danou stránku na zařízení, která je požadovaná.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-277">Returns a reference to the ControlAdapter that renders the page on the device that requested it.</span></span>

## <a name="asyncmode"></a><span data-ttu-id="6cbf3-278">AsyncMode</span><span class="sxs-lookup"><span data-stu-id="6cbf3-278">AsyncMode</span></span>

<span data-ttu-id="6cbf3-279">Tato vlastnost určuje, zda je na stránce zpracovány asynchronně.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-279">This property indicates whether or not the page is processed asynchronously.</span></span> <span data-ttu-id="6cbf3-280">Je určena pro použití modulem runtime a ne přímo v kódu.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-280">It is intended for use by the runtime and not directly in code.</span></span>

## <a name="clientidseparator"></a><span data-ttu-id="6cbf3-281">ClientIDSeparator</span><span class="sxs-lookup"><span data-stu-id="6cbf3-281">ClientIDSeparator</span></span>

<span data-ttu-id="6cbf3-282">Tato vlastnost vrátí znak, který slouží jako oddělovač při vytváření klienta Jedinečný ID pro ovládací prvky.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-282">This property returns the character used as a separator when creating unique client IDs for controls.</span></span> <span data-ttu-id="6cbf3-283">Je určena pro použití modulem runtime a ne přímo v kódu.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-283">It is intended for use by the runtime and not directly in code.</span></span>

## <a name="pagestatepersister"></a><span data-ttu-id="6cbf3-284">PageStatePersister</span><span class="sxs-lookup"><span data-stu-id="6cbf3-284">PageStatePersister</span></span>

<span data-ttu-id="6cbf3-285">Tato vlastnost vrátí objekt PageStatePersister stránky.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-285">This property returns the PageStatePersister object for the page.</span></span> <span data-ttu-id="6cbf3-286">Tato vlastnost se používá především vývojáři ovládací prvek technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-286">This property is primarily used by ASP.NET control developers.</span></span>

## <a name="uniquefilepathsuffix"></a><span data-ttu-id="6cbf3-287">UniqueFilePathSuffix</span><span class="sxs-lookup"><span data-stu-id="6cbf3-287">UniqueFilePathSuffix</span></span>

<span data-ttu-id="6cbf3-288">Tato vlastnost vrátí jedinečný suffic, která se připojuje k souboru na cestě pro ukládání do mezipaměti prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-288">This property returns a unique suffic that is appended to the file path for caching browsers.</span></span> <span data-ttu-id="6cbf3-289">Výchozí hodnota je \_ \_ufps = a 6 číslic.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-289">The default value is \_\_ufps= and a 6-digit number.</span></span>

## <a name="new-public-methods-for-the-page-class"></a><span data-ttu-id="6cbf3-290">Nové veřejné metody pro třídu stránky</span><span class="sxs-lookup"><span data-stu-id="6cbf3-290">New Public Methods for the Page Class</span></span>

<span data-ttu-id="6cbf3-291">Nová třída stránky v technologii ASP.NET 2.0 jsou tyto veřejné metody.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-291">The following public methods are new to the Page class in ASP.NET 2.0.</span></span>

## <a name="addonprerendercompleteasync"></a><span data-ttu-id="6cbf3-292">AddOnPreRenderCompleteAsync</span><span class="sxs-lookup"><span data-stu-id="6cbf3-292">AddOnPreRenderCompleteAsync</span></span>

<span data-ttu-id="6cbf3-293">Metoda registruje delegátů obslužných rutin událostí pro provádění asynchronní stránky.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-293">This method registers event handler delegates for asynchronous page execution.</span></span> <span data-ttu-id="6cbf3-294">Asynchronní stránky jsou popsány dále v tomto modulu.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-294">Asynchronous pages are discussed later in this module.</span></span>

## <a name="applystylesheetskin"></a><span data-ttu-id="6cbf3-295">ApplyStyleSheetSkin</span><span class="sxs-lookup"><span data-stu-id="6cbf3-295">ApplyStyleSheetSkin</span></span>

<span data-ttu-id="6cbf3-296">Vlastnosti v šabloně stylů stránky se vztahuje na stránce.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-296">Applies the properties in a pages style sheet to the page.</span></span>

## <a name="executeregisteredasynctasks"></a><span data-ttu-id="6cbf3-297">ExecuteRegisteredAsyncTasks</span><span class="sxs-lookup"><span data-stu-id="6cbf3-297">ExecuteRegisteredAsyncTasks</span></span>

<span data-ttu-id="6cbf3-298">Tato metoda bytostí asynchronní úlohu.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-298">This method beings an asynchronous task.</span></span>

### <a name="getvalidators"></a><span data-ttu-id="6cbf3-299">GetValidators</span><span class="sxs-lookup"><span data-stu-id="6cbf3-299">GetValidators</span></span>

<span data-ttu-id="6cbf3-300">Vrátí kolekci validátory pro skupiny ověření zadaný nebo výchozí skupiny ověření, pokud není zadaný žádný.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-300">Returns a collection of validators for the specified validation group or the default validation group if none is specified.</span></span>

## <a name="registerasynctask"></a><span data-ttu-id="6cbf3-301">RegisterAsyncTask</span><span class="sxs-lookup"><span data-stu-id="6cbf3-301">RegisterAsyncTask</span></span>

<span data-ttu-id="6cbf3-302">Metoda registruje nový asynchronní úkol.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-302">This method registers a new async task.</span></span> <span data-ttu-id="6cbf3-303">Asynchronní stránky jsou popsané dále v tomto modulu.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-303">Asynchronous pages are covered later in this module.</span></span>

## <a name="registerrequirescontrolstate"></a><span data-ttu-id="6cbf3-304">RegisterRequiresControlState</span><span class="sxs-lookup"><span data-stu-id="6cbf3-304">RegisterRequiresControlState</span></span>

<span data-ttu-id="6cbf3-305">Tato metoda říká technologie ASP.NET, musí jako trvalý stav ovládacího prvku stránky.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-305">This method tells ASP.NET that the pages control state must be persisted.</span></span>

## <a name="registerrequiresviewstateencryption"></a><span data-ttu-id="6cbf3-306">RegisterRequiresViewStateEncryption</span><span class="sxs-lookup"><span data-stu-id="6cbf3-306">RegisterRequiresViewStateEncryption</span></span>

<span data-ttu-id="6cbf3-307">Tato metoda říká technologie ASP.NET, že stav zobrazení stránky vyžaduje šifrování.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-307">This method tells ASP.NET that the pages viewstate requires encryption.</span></span>

## <a name="resolveclienturl"></a><span data-ttu-id="6cbf3-308">ResolveClientUrl</span><span class="sxs-lookup"><span data-stu-id="6cbf3-308">ResolveClientUrl</span></span>

<span data-ttu-id="6cbf3-309">Vrátí relativní adresu URL, které lze použít pro požadavky klientů pro obrázky, atd.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-309">Returns a relative URL that can be used for client requests for images, etc.</span></span>

## <a name="setfocus"></a><span data-ttu-id="6cbf3-310">SetFocus</span><span class="sxs-lookup"><span data-stu-id="6cbf3-310">SetFocus</span></span>

<span data-ttu-id="6cbf3-311">Tato metoda nastaví fokus na ovládací prvek, který je zadán při počátečním načtení stránky.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-311">This method will set the focus to the control that is specified when the page is initially loaded.</span></span>

## <a name="unregisterrequirescontrolstate"></a><span data-ttu-id="6cbf3-312">UnregisterRequiresControlState</span><span class="sxs-lookup"><span data-stu-id="6cbf3-312">UnregisterRequiresControlState</span></span>

<span data-ttu-id="6cbf3-313">Tato metoda zruší registraci ovládacího prvku, který je předán jako už vyžadující trvalost stavu ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-313">This method will unregister the control that is passed to it as no longer requiring control state persistence.</span></span>

## <a name="changes-to-the-page-lifecycle"></a><span data-ttu-id="6cbf3-314">Změny životního cyklu stránky</span><span class="sxs-lookup"><span data-stu-id="6cbf3-314">Changes to the Page Lifecycle</span></span>

<span data-ttu-id="6cbf3-315">Životní cyklus stránky v technologii ASP.NET 2.0 nedošlo ke změně výrazně, ale existují některé nové metody, které byste měli vědět.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-315">The page lifecycle in ASP.NET 2.0 hasnt changed dramatically, but there are some new methods that you should be aware of.</span></span> <span data-ttu-id="6cbf3-316">Životní cyklus stránky technologie ASP.NET 2.0 je popsaný níže.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-316">The ASP.NET 2.0 page lifecycle is outlined below.</span></span>

## <a name="preinit-new-in-aspnet-20"></a><span data-ttu-id="6cbf3-317">PreInit (v technologii ASP.NET 2.0)</span><span class="sxs-lookup"><span data-stu-id="6cbf3-317">PreInit (New in ASP.NET 2.0)</span></span>

<span data-ttu-id="6cbf3-318">Událost PreInit je nejstarší fázi životního cyklu, které může vývojář přístup.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-318">The PreInit event is the earliest stage in the lifecycle that a developer can access.</span></span> <span data-ttu-id="6cbf3-319">Díky přidání této události je možné programově změnit ASP.NET 2.0 motivy, hlavní stránky, přístup k vlastnostem pro ASP.NET 2.0 profilu atd. Pokud jste v postback stavu, navíc je důležité si uvědomit, že vlastnost Viewstate nebyla dosud nebyly použity k ovládacím prvkům v tuto chvíli v životní cyklus.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-319">The addition of this event makes it possible to programmatically change ASP.NET 2.0 themes, master pages, access properties for an ASP.NET 2.0 profile, etc. If you are in a postback state, its important to realize that Viewstate has not yet been applied to controls at this point in the lifecycle.</span></span> <span data-ttu-id="6cbf3-320">Proto pokud vývojář změní vlastnost ovládacího prvku v této fázi, bude pravděpodobně přepsána dále v cyklu stránky.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-320">Therefore, if a developer changes a property of a control at this stage, it will likely be overwritten later in the pages lifecycle.</span></span>

## <a name="init"></a><span data-ttu-id="6cbf3-321">Init</span><span class="sxs-lookup"><span data-stu-id="6cbf3-321">Init</span></span>

<span data-ttu-id="6cbf3-322">Z ASP.NET nedošlo ke změně události Init 1.x.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-322">The Init event has not changed from ASP.NET 1.x.</span></span> <span data-ttu-id="6cbf3-323">To je, kde chcete číst nebo inicializace vlastností ovládacích prvků na stránce.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-323">This is where you would want to read or initialize properties of controls on your page.</span></span> <span data-ttu-id="6cbf3-324">V této fázi, hlavní stránky, motivy se použijí na stránce již atd.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-324">At this stage, master pages, themes, etc. are already applied to the page.</span></span>

## <a name="initcomplete-new-in-20"></a><span data-ttu-id="6cbf3-325">InitComplete (nově ve verzi 2.0)</span><span class="sxs-lookup"><span data-stu-id="6cbf3-325">InitComplete (New in 2.0)</span></span>

<span data-ttu-id="6cbf3-326">Událost InitComplete je volána na konci fáze inicializace stránky.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-326">The InitComplete event is called at the end of the pages initialization stage.</span></span> <span data-ttu-id="6cbf3-327">V tomto okamžiku v životního cyklu, dostanete ovládacích prvků na stránce, ale jejich stavu ještě není naplněná.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-327">At this point in the lifecycle, you can access controls on the page, but their state has not yet been populated.</span></span>

## <a name="preload-new-in-20"></a><span data-ttu-id="6cbf3-328">Předběžné načtení (nově ve verzi 2.0)</span><span class="sxs-lookup"><span data-stu-id="6cbf3-328">PreLoad (New in 2.0)</span></span>

<span data-ttu-id="6cbf3-329">Tato událost je volána po odeslání všech dat a před stránku\_zatížení.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-329">This event is called after all postback data has been applied and just prior to Page\_Load.</span></span>

## <a name="load"></a><span data-ttu-id="6cbf3-330">Zatížení</span><span class="sxs-lookup"><span data-stu-id="6cbf3-330">Load</span></span>

<span data-ttu-id="6cbf3-331">Událost načtení nedošlo ke změně z ASP.NET 1.x.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-331">The Load event has not changed from ASP.NET 1.x.</span></span>

## <a name="loadcomplete-new-in-20"></a><span data-ttu-id="6cbf3-332">LoadComplete (nově ve verzi 2.0)</span><span class="sxs-lookup"><span data-stu-id="6cbf3-332">LoadComplete (New in 2.0)</span></span>

<span data-ttu-id="6cbf3-333">Událost LoadComplete je poslední události ve fázi načtení stránky.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-333">The LoadComplete event is the last event in the pages load stage.</span></span> <span data-ttu-id="6cbf3-334">V této fázi byl připsán všechna data zpětné volání a stav zobrazení na stránce.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-334">At this stage, all postback and viewstate data has been applied to the page.</span></span>

## <a name="prerender"></a><span data-ttu-id="6cbf3-335">Provedení operace preRender</span><span class="sxs-lookup"><span data-stu-id="6cbf3-335">PreRender</span></span>

<span data-ttu-id="6cbf3-336">Pokud chcete pro stav zobrazení správně udržovat pro ovládací prvky, které jsou dynamicky přidat na stránku, událost PreRender je poslední příležitostí je přidat.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-336">If you would like for viewstate to be properly maintained for controls that are added to the page dynamically, the PreRender event is the last opportunity to add them.</span></span>

## <a name="prerendercomplete-new-in-20"></a><span data-ttu-id="6cbf3-337">PreRenderComplete (nově ve verzi 2.0)</span><span class="sxs-lookup"><span data-stu-id="6cbf3-337">PreRenderComplete (New in 2.0)</span></span>

<span data-ttu-id="6cbf3-338">Ve fázi PreRenderComplete všechny ovládací prvky byly přidány na stránku a je připravený k vykreslení stránky.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-338">At the PreRenderComplete stage, all controls have been added to the page and the page is ready to be rendered.</span></span> <span data-ttu-id="6cbf3-339">Událostí PreRenderComplete je poslední událost vyvolanou před uložením stav zobrazení stránky.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-339">The PreRenderComplete event is the last event raised before the pages viewstate is saved.</span></span>

## <a name="savestatecomplete-new-in-20"></a><span data-ttu-id="6cbf3-340">SaveStateComplete (nově ve verzi 2.0)</span><span class="sxs-lookup"><span data-stu-id="6cbf3-340">SaveStateComplete (New in 2.0)</span></span>

<span data-ttu-id="6cbf3-341">Událost SaveStateComplete je volána hned po uložení všech stav viewstate a ovládací prvek stránky.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-341">The SaveStateComplete event is called immediately after all page viewstate and control state has been saved.</span></span> <span data-ttu-id="6cbf3-342">Toto je poslední událostí před skutečně vykreslení stránky v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-342">This is the last event before the page is actually rendered to the browser.</span></span>

## <a name="render"></a><span data-ttu-id="6cbf3-343">Vykreslení</span><span class="sxs-lookup"><span data-stu-id="6cbf3-343">Render</span></span>

<span data-ttu-id="6cbf3-344">Metody vykreslení nebyl změněn od ASP.NET 1.x.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-344">The Render method has not changed since ASP.NET 1.x.</span></span> <span data-ttu-id="6cbf3-345">Toto je ve kterém je inicializován HtmlTextWriter a na stránce se zobrazí v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-345">This is where the HtmlTextWriter is initialized and the page is rendered to the browser.</span></span>

## <a name="cross-page-postback-in-aspnet-20"></a><span data-ttu-id="6cbf3-346">Zpětné volání mezi stránkami v ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="6cbf3-346">Cross-Page Postback in ASP.NET 2.0</span></span>

<span data-ttu-id="6cbf3-347">V technologii ASP.NET 1.x, postbacků byly zapotřebí k odeslání na stejné stránce.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-347">In ASP.NET 1.x, postbacks were required to post to the same page.</span></span> <span data-ttu-id="6cbf3-348">Zpětná volání mezi stránkami nebyly povoleny.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-348">Cross-page postbacks were not allowed.</span></span> <span data-ttu-id="6cbf3-349">ASP.NET 2.0 přidává možnost publikovat zpět na jinou stránku prostřednictvím rozhraní IButtonControl.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-349">ASP.NET 2.0 adds the ability to post back to a different page via the IButtonControl interface.</span></span> <span data-ttu-id="6cbf3-350">Libovolný ovládací prvek, který implementuje rozhraní IButtonControl (tlačítko, odkazem (LinkButton) a ImageButton kromě vlastní ovládací prvky třetích stran) můžete využít tyto nové funkce prostřednictvím použití atributu PostBackUrl.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-350">Any control that implements the new IButtonControl interface (Button, LinkButton, and ImageButton in addition to third-party custom controls) can take advantage of this new functionality via the use of the PostBackUrl attribute.</span></span> <span data-ttu-id="6cbf3-351">Následující kód ukazuje ovládací prvek tlačítka, který odešle zpět na druhé stránce.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-351">The following code shows a Button control that posts back to a second page.</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

<span data-ttu-id="6cbf3-352">Na stránce, když se pošle zpět na stránku, která zahájí zpětné volání, jsou přístupná přes vlastnost PreviousPage na druhé stránce.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-352">When the page is posted back, the Page that initiates the postback is accessible via the PreviousPage property on the second page.</span></span> <span data-ttu-id="6cbf3-353">Tato funkce se implementuje prostřednictvím nového webového formuláře\_DoPostBackWithOptions funkce na straně klienta, která ASP.NET 2.0 vykresluje na stránku, když ovládací prvek odeslat zpět na jiné stránce.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-353">This functionality is implemented via the new WebForm\_DoPostBackWithOptions client-side function that ASP.NET 2.0 renders to the page when a control posts back to a different page.</span></span> <span data-ttu-id="6cbf3-354">Tato funkce jazyka JavaScript poskytuje nové obslužná rutina WebResource.axd, který generuje skript do klienta.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-354">This JavaScript function is provided by the new WebResource.axd handler which emits script to the client.</span></span>

<span data-ttu-id="6cbf3-355">Následující video je návod, zpětné volání mezi stránkami.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-355">The video below is a walkthrough of a cross-page postback.</span></span>


![](the-asp-net-2-0-page-model/_static/image2.png)


[<span data-ttu-id="6cbf3-356">Otevřít Video na celou obrazovku</span><span class="sxs-lookup"><span data-stu-id="6cbf3-356">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/xpage1.wmv)


## <a name="more-details-on-cross-page-postbacks"></a><span data-ttu-id="6cbf3-357">Další podrobnosti o postbacků mezi stránkami</span><span class="sxs-lookup"><span data-stu-id="6cbf3-357">More Details on Cross-Page Postbacks</span></span>

### <a name="viewstate"></a><span data-ttu-id="6cbf3-358">Stav zobrazení</span><span class="sxs-lookup"><span data-stu-id="6cbf3-358">Viewstate</span></span>

<span data-ttu-id="6cbf3-359">Můžete mít vyzváni sami už o co se stane vlastnost viewstate od první stránky ve scénáři postback mezi stránkami.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-359">You may have asked yourself already about what happens to the viewstate from the first page in a cross-page postback scenario.</span></span> <span data-ttu-id="6cbf3-360">Po všech libovolný ovládací prvek, který neimplementuje rozhraní IPostBackDataHandler zachová svůj stav prostřednictvím vlastnosti viewstate, tak přístup k vlastnostem ovládacího prvku na druhé stránce zpětné volání mezi stránkami, musí mít přístup k vlastnost viewstate pro stránku.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-360">After all, any control that does not implement IPostBackDataHandler will persist its state via viewstate, so to have access to the properties of that control on the second page of a cross-page postback, you must have access to the viewstate for the page.</span></span> <span data-ttu-id="6cbf3-361">Tento scénář s využitím nového skrytého pole na druhé stránce volá postará technologie ASP.NET 2.0 \_ \_PREVIOUSPAGE.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-361">ASP.NET 2.0 takes care of this scenario using a new hidden field in the second page called \_\_PREVIOUSPAGE.</span></span> <span data-ttu-id="6cbf3-362">\_ \_PREVIOUSPAGE pole formuláře obsahuje vlastnost viewstate na první stránce tak, že máte přístup k vlastnostem všechny ovládací prvky na druhé stránce.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-362">The \_\_PREVIOUSPAGE form field contains the viewstate for the first page so that you can have access to the properties of all controls in the second page.</span></span>

### <a name="circumventing-findcontrol"></a><span data-ttu-id="6cbf3-363">Obcházení FindControl</span><span class="sxs-lookup"><span data-stu-id="6cbf3-363">Circumventing FindControl</span></span>

<span data-ttu-id="6cbf3-364">V video s názorným postupem postbacku mezi stránkami můžu FindControl metoda používá k získání odkazu na ovládací prvek TextBox na první stránce.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-364">In the video walkthrough of a cross-page postback, I used the FindControl method to get a reference to the TextBox control on the first page.</span></span> <span data-ttu-id="6cbf3-365">Tato metoda funguje dobře pro tento účel, ale FindControl je nákladné a psaním dalšího kódu, vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-365">That method works well for that purpose, but FindControl is expensive and it requires writing additional code.</span></span> <span data-ttu-id="6cbf3-366">Naštěstí ASP.NET 2.0 poskytuje alternativu k FindControl pro tento účel, který bude fungovat v mnoha scénářích.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-366">Fortunately, ASP.NET 2.0 provides an alternative to FindControl for this purpose that will work in many scenarios.</span></span> <span data-ttu-id="6cbf3-367">Direktiva PreviousPageType umožňuje mít silný odkaz na předchozí stránku pomocí TypeName nebo VirtualPath atribut.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-367">The PreviousPageType directive allows you to have a strongly-typed reference to the previous page by using either the TypeName or the VirtualPath attribute.</span></span> <span data-ttu-id="6cbf3-368">Atribut TypeName umožňuje určit typ na předchozí stránku, zatímco Atribut VirtualPath umožňuje odkazovat na předchozí stránku pomocí virtuální cesty.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-368">The TypeName attribute allows you to specify the type of the previous page while the VirtualPath attribute allows you to refer to the previous page using a virtual path.</span></span> <span data-ttu-id="6cbf3-369">Po direktivě PreviousPageType, musí pak vystavit ovládací prvky, ke kterému chcete povolit přístup pomocí veřejné vlastnosti atd.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-369">After you've set the PreviousPageType directive, you must then expose the controls, etc. to which you want to allow access using public properties.</span></span>

## <a name="lab-1-cross-page-postback"></a><span data-ttu-id="6cbf3-370">Praktické cvičení 1 mezi stránkami postbacku</span><span class="sxs-lookup"><span data-stu-id="6cbf3-370">Lab 1 Cross-Page Postback</span></span>

<span data-ttu-id="6cbf3-371">V tomto testovacím prostředí vytvoříte aplikaci, která využívá nové funkce zpětného volání mezi stránkami ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-371">In this lab, you will create an application that uses the new cross-page postback functionality of ASP.NET 2.0.</span></span>

1. <span data-ttu-id="6cbf3-372">Otevřít Visual Studio 2005 a vytvořit nový web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-372">Open Visual Studio 2005 and create a new ASP.NET Web site.</span></span>
2. <span data-ttu-id="6cbf3-373">Přidání nového webového formuláře volá page2.aspx.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-373">Add a new Webform called page2.aspx.</span></span>
3. <span data-ttu-id="6cbf3-374">V návrhovém zobrazení otevřete Default.aspx a přidejte ovládací prvek Button a ovládací prvek textového pole.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-374">Open the Default.aspx in Design view and add a Button control and a TextBox control.</span></span> 

    1. <span data-ttu-id="6cbf3-375">Zadejte ID ovládacího prvku tlačítko **tlacitkoOdeslat** a textovém poli ID **uživatelské jméno**.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-375">Give the Button control an ID of **SubmitButton** and the TextBox control an ID of **UserName**.</span></span>
    2. <span data-ttu-id="6cbf3-376">Nastavte vlastnost PostBackUrl tlačítka na page2.aspx.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-376">Set the PostBackUrl property of the Button to page2.aspx.</span></span>
4. <span data-ttu-id="6cbf3-377">Otevřete page2.aspx v zobrazení zdroje.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-377">Open page2.aspx in Source view.</span></span>
5. <span data-ttu-id="6cbf3-378">Direktiva @ PreviousPageType přidáte, jak je znázorněno níže:</span><span class="sxs-lookup"><span data-stu-id="6cbf3-378">Add a @ PreviousPageType directive as shown below:</span></span>
6. <span data-ttu-id="6cbf3-379">Přidejte následující kód na stránku\_zatížení vaší page2.aspx kódu:</span><span class="sxs-lookup"><span data-stu-id="6cbf3-379">Add the following code to the Page\_Load of page2.aspx's code-behind:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. <span data-ttu-id="6cbf3-380">Sestavte projekt kliknutím na sestavení v nabídce sestavení.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-380">Build the project by clicking on Build on the Build menu.</span></span>
8. <span data-ttu-id="6cbf3-381">Přidejte následující kód do kódu pro Default.aspx:</span><span class="sxs-lookup"><span data-stu-id="6cbf3-381">Add the following code to the code-behind for Default.aspx:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. <span data-ttu-id="6cbf3-382">Změnit na stránce\_zatížení v page2.aspx takto:</span><span class="sxs-lookup"><span data-stu-id="6cbf3-382">Change the Page\_Load in page2.aspx to the following:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. <span data-ttu-id="6cbf3-383">Sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-383">Build the project.</span></span>
11. <span data-ttu-id="6cbf3-384">Spusťte projekt.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-384">Run the project.</span></span>
12. <span data-ttu-id="6cbf3-385">Zadejte název do textového pole a klikněte na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-385">Enter your name in the TextBox and click the button.</span></span>
13. <span data-ttu-id="6cbf3-386">Co je výsledkem?</span><span class="sxs-lookup"><span data-stu-id="6cbf3-386">What is the result?</span></span>

## <a name="asynchronous-pages-in-aspnet-20"></a><span data-ttu-id="6cbf3-387">Asynchronní stránek v ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="6cbf3-387">Asynchronous Pages in ASP.NET 2.0</span></span>

<span data-ttu-id="6cbf3-388">Mnohé problémy kolizí v technologii ASP.NET jsou způsobeny latence externí volání (například volání webové služby nebo databáze), souboru vstupně-výstupní latence, atd. Po odeslání žádosti se pro aplikaci ASP.NET, ASP.NET používá jednu z jeho pracovní vlákna ke zpracování této žádosti.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-388">Many contention problems in ASP.NET are caused by latency of external calls (such as Web service or database calls), file IO latency, etc. When a request is made against an ASP.NET application, ASP.NET uses one of its worker threads to service that request.</span></span> <span data-ttu-id="6cbf3-389">Tento požadavek na vlastní bylo vlákno až do dokončení požadavku a byl odeslán odpovědi.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-389">That request owns that thread until the request is complete and the response has been sent.</span></span> <span data-ttu-id="6cbf3-390">ASP.NET 2.0 vyhledá vyřešit problémy s latencí s těmito typy chyb tak, že přidáte funkci stránky spustit asynchronně.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-390">ASP.NET 2.0 seeks to resolve latency issues with these types of issues by adding the capability to execute pages asynchronously.</span></span> <span data-ttu-id="6cbf3-391">To znamená, že můžete začít požadavek a pak předá další provádění do jiného vlákna, a tím vrátí do fondu vláken k dispozici rychle pracovní podproces.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-391">That means that a worker thread can start the request and then hand off additional execution to another thread, thereby returning to the available thread pool quickly.</span></span> <span data-ttu-id="6cbf3-392">Po dokončení vstupně-výstupní operace souboru, volání databáze a další, nové vlákno se získávají z fondu vláken k dokončení požadavku.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-392">When the file IO, database call, etc. has completed, a new thread is obtained from the thread pool to finish the request.</span></span>

<span data-ttu-id="6cbf3-393">Prvním krokem při vytváření stránky spustit asynchronně, je nastavena **asynchronní** atribut direktivy stránky takto:</span><span class="sxs-lookup"><span data-stu-id="6cbf3-393">The first step in making a page execute asynchronously is to set the **Async** attribute of the page directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

<span data-ttu-id="6cbf3-394">Tento atribut oznamuje implementovat IHttpAsyncHandler stránky technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-394">This attribute tells ASP.NET to implement the IHttpAsyncHandler for the page.</span></span>

<span data-ttu-id="6cbf3-395">Dalším krokem je volání metody AddOnPreRenderCompleteAsync v určitém bodě životní cyklus stránky před provedením operace PreRender.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-395">The next step is to call the AddOnPreRenderCompleteAsync method at a point in the lifecycle of the page prior to PreRender.</span></span> <span data-ttu-id="6cbf3-396">(Tato metoda je obvykle volána na stránce\_zatížení.) Metoda AddOnPreRenderCompleteAsync přebírá dva parametry. BeginEventHandler a EndEventHandler.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-396">(This method is typically called in Page\_Load.) The AddOnPreRenderCompleteAsync method takes two parameters; a BeginEventHandler and an EndEventHandler.</span></span> <span data-ttu-id="6cbf3-397">BeginEventHandler vrátí objekt IAsyncResult, který je pak předán jako parametr EndEventHandler.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-397">The BeginEventHandler returns an IAsyncResult which is then passed as a parameter to the EndEventHandler.</span></span>

<span data-ttu-id="6cbf3-398">Video níže je návod požadavku asynchronní stránky.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-398">The video below is a walkthrough of an asynchronous page request.</span></span>


![](the-asp-net-2-0-page-model/_static/image3.png)


[<span data-ttu-id="6cbf3-399">Otevřít Video na celou obrazovku</span><span class="sxs-lookup"><span data-stu-id="6cbf3-399">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/async1.wmv)


> [!NOTE]
> <span data-ttu-id="6cbf3-400">Stránku asynchronní nevykresluje do prohlížeče, dokud se nedokončí EndEventHandler.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-400">An async page does not render to the browser until the EndEventHandler has completed.</span></span> <span data-ttu-id="6cbf3-401">Žádné nejisté, ale někteří vývojáři budou představit asynchronních jako podobný zpětná volání asynchronní.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-401">No doubt but that some developers will think of async requests as being similar to async callbacks.</span></span> <span data-ttu-id="6cbf3-402">Je důležité si uvědomit, že nejsou.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-402">It's important to realize that they are not.</span></span> <span data-ttu-id="6cbf3-403">Výhoda pro asynchronní požadavků je, že první pracovní vlákno mohou být vráceny do fondu vláken zpracování nových požadavků, a tím snižuje kolize kvůli se vstupně-výstupních operací, které jsou vázány, atd.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-403">The benefit to asynchronous requests is that the first worker thread can be returned to the thread pool to service new requests, thereby reducing contention due to being IO bound, etc.</span></span>


## <a name="script-callbacks-in-aspnet-20"></a><span data-ttu-id="6cbf3-404">Zpětná volání skriptu v ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="6cbf3-404">Script Callbacks in ASP.NET 2.0</span></span>

<span data-ttu-id="6cbf3-405">Weboví vývojáři se vždy podívat způsoby, jak zabránit blikání přidružené zpětné volání.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-405">Web developers have always looked for ways to prevent the flickering associated with a callback.</span></span> <span data-ttu-id="6cbf3-406">V technologii ASP.NET 1.x SmartNavigation bylo nejběžnější metodou pro předcházení blikání, ale SmartNavigation způsobil problémy pro vývojáře v některé z důvodu složitosti implementace na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-406">In ASP.NET 1.x, SmartNavigation was the most common method for avoiding flickering, but SmartNavigation caused problems for some developers because of the complexity of its implementation on the client.</span></span> <span data-ttu-id="6cbf3-407">ASP.NET 2.0 řeší tento problém s zpětná volání skriptu.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-407">ASP.NET 2.0 addresses this issue with script callbacks.</span></span> <span data-ttu-id="6cbf3-408">Zpětná volání skriptu využívat XMLHttp k podání žádostí o u webového serveru pomocí jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-408">Script callbacks utilize XMLHttp to make requests against the Web server via JavaScript.</span></span> <span data-ttu-id="6cbf3-409">Žádost o XMLHttp vrátí data XML, který lze potom manipulovat prostřednictvím prohlížeče modelu DOM.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-409">The XMLHttp request returns XML data that can then be manipulated via the browser's DOM.</span></span> <span data-ttu-id="6cbf3-410">XMLHttp kód je pro uživatele skryté nová obslužná rutina WebResource.axd.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-410">XMLHttp code is hidden from the user by the new WebResource.axd handler.</span></span>

<span data-ttu-id="6cbf3-411">Existuje několik kroků, které jsou nezbytné, aby se konfigurace zpětného volání skriptu v technologii ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-411">There are several steps that are necessary in order to configure a script callback in ASP.NET 2.0.</span></span>

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a><span data-ttu-id="6cbf3-412">Krok 1: Implementace rozhraní ICallbackEventHandler</span><span class="sxs-lookup"><span data-stu-id="6cbf3-412">Step 1 : Implement the ICallbackEventHandler Interface</span></span>

<span data-ttu-id="6cbf3-413">Aby ASP.NET rozpoznat stránce jako účasti ve zpětném volání skriptu musí implementovat rozhraní ICallbackEventHandler.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-413">In order for ASP.NET to recognize your page as participating in a script callback, you must implement the ICallbackEventHandler interface.</span></span> <span data-ttu-id="6cbf3-414">Můžete to provést v souboru modelu code-behind takto:</span><span class="sxs-lookup"><span data-stu-id="6cbf3-414">You can do this in your code-behind file like so:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

<span data-ttu-id="6cbf3-415">Můžete také Uděláte to pomocí direktiv jako @ implementuje:</span><span class="sxs-lookup"><span data-stu-id="6cbf3-415">You can also do this using the @ Implements directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

<span data-ttu-id="6cbf3-416">Obvykle použijete – direktiva @ implementuje při použití vloženého kódu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-416">You would typically use the @ Implements directive when using inline ASP.NET code.</span></span>

## <a name="step-2--call-getcallbackeventreference"></a><span data-ttu-id="6cbf3-417">Krok 2: Volání GetCallbackEventReference</span><span class="sxs-lookup"><span data-stu-id="6cbf3-417">Step 2 : Call GetCallbackEventReference</span></span>

<span data-ttu-id="6cbf3-418">Jak už bylo zmíněno dříve, je zapouzdřena volání XMLHttp v obslužná rutina WebResource.axd.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-418">As mentioned previously, the XMLHttp call is encapsulated in the WebResource.axd handler.</span></span> <span data-ttu-id="6cbf3-419">Při vykreslování stránky ASP.NET se přidejte volání do webového formuláře\_DoCallback klientský skript, který je poskytován WebResource.axd.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-419">When your page is rendered, ASP.NET will add a call to WebForm\_DoCallback, a client script that is provided by WebResource.axd.</span></span> <span data-ttu-id="6cbf3-420">Webového formuláře\_nahrazuje funkce DoCallback \_ \_doPostBack funkce pro zpětné volání.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-420">The WebForm\_DoCallback function replaces the \_\_doPostBack function for a callback.</span></span> <span data-ttu-id="6cbf3-421">Nezapomeňte, že \_ \_doPostBack programově odeslání formuláře na stránce.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-421">Remember that \_\_doPostBack programmatically submits the form on the page.</span></span> <span data-ttu-id="6cbf3-422">Ve scénáři zpětné volání, ale chcete zabránit zpětné volání, takže \_ \_doPostBack nestačí.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-422">In a callback scenario, you want to prevent a postback, so \_\_doPostBack will not suffice.</span></span>

> [!NOTE]
> <span data-ttu-id="6cbf3-423">\_\_doPostBack se zobrazí stránku ve scénáři skript zpětného volání klienta.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-423">\_\_doPostBack is still rendered to the page in a client script callback scenario.</span></span> <span data-ttu-id="6cbf3-424">Ale není použit pro zpětné volání.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-424">However, it's not used for the callback.</span></span>


<span data-ttu-id="6cbf3-425">Argumenty pro webovém formuláři\_DoCallback funkce na straně klienta jsou k dispozici prostřednictvím funkce na straně serveru GetCallbackEventReference, která by obvykle nazývat stránce\_zatížení.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-425">The arguments for the WebForm\_DoCallback client-side function are provided via the server-side function GetCallbackEventReference which would normally be called in Page\_Load.</span></span> <span data-ttu-id="6cbf3-426">Typické volání GetCallbackEventReference může vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="6cbf3-426">A typical call to GetCallbackEventReference might look like this:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> <span data-ttu-id="6cbf3-427">V takovém případě je cm instancí ClientScriptManager.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-427">In this case, cm is an instance of ClientScriptManager.</span></span> <span data-ttu-id="6cbf3-428">Třída ClientScriptManager se budeme dále v tomto modulu.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-428">The ClientScriptManager class will be covered later in this module.</span></span>


<span data-ttu-id="6cbf3-429">Existuje několik přetížené verze GetCallbackEventReference.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-429">There are several overloaded versions of GetCallbackEventReference.</span></span> <span data-ttu-id="6cbf3-430">V takovém případě argumenty jsou následující:</span><span class="sxs-lookup"><span data-stu-id="6cbf3-430">In this case, the arguments are as follows:</span></span>

`this`

<span data-ttu-id="6cbf3-431">Odkaz na ovládací prvek, ve kterém je volána GetCallbackEventReference.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-431">A reference to the control where GetCallbackEventReference is being called.</span></span> <span data-ttu-id="6cbf3-432">V takovém případě je samotné stránky.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-432">In this case, it's the page itself.</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

<span data-ttu-id="6cbf3-433">Argument řetězce, které budou předány z kódu na straně klienta pro událost na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-433">A string argument that will be passed from the client-side code to the server-side event.</span></span> <span data-ttu-id="6cbf3-434">V tomto případě zasílání rychlých zpráv, předejte hodnotu z rozevíracího seznamu název ddlCompany.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-434">In this case, Im passing the value of a dropdown called ddlCompany.</span></span>

`ShowCompanyName`

<span data-ttu-id="6cbf3-435">Název funkce na straně klienta, který bude přijímat návratovou hodnotu (jako řetězce) z události zpětného volání na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-435">The name of the client-side function that will accept the return value (as string) from the server-side callback event.</span></span> <span data-ttu-id="6cbf3-436">Tato funkce bude volat pouze po úspěšné zpětného volání na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-436">This function will only be called when the server-side callback is successful.</span></span> <span data-ttu-id="6cbf3-437">Proto z důvodu odolnosti, obecně doporučujeme používat přetížené verze GetCallbackEventReference, která přebírá argument další řetězec určující název funkce na straně klienta pro spuštění v případě chyby.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-437">Therefore, for the sake of robustness, it is generally recommended to use the overloaded version of GetCallbackEventReference that takes an additional string argument specifying the name of a client-side function to execute in the event of an error.</span></span>

`null`

<span data-ttu-id="6cbf3-438">Řetězec představující funkce na straně klienta, která bylo zahájeno před zpětného volání k serveru.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-438">A string representing a client-side function that it initiated before the callback to the server.</span></span> <span data-ttu-id="6cbf3-439">V takovém případě není žádný skript, takže má argument hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-439">In this case, there is no such script, so the argument is null.</span></span>

`true`

<span data-ttu-id="6cbf3-440">Logická hodnota určující, jestli se mají asynchronně provádění zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-440">A Boolean specifying whether or not to conduct the callback asynchronously.</span></span>

<span data-ttu-id="6cbf3-441">Volání webového formuláře\_DoCallback na straně klienta předá tyto argumenty.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-441">The call to WebForm\_DoCallback on the client will pass these arguments.</span></span> <span data-ttu-id="6cbf3-442">Proto se při vykreslení této stránky na straně klienta, tento kód bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="6cbf3-442">Therefore, when this page is rendered on the client, that code will look like so:</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

<span data-ttu-id="6cbf3-443">Všimněte si, že podpis funkce na straně klienta je trochu jiná.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-443">Notice that the signature of the function on the client is a bit different.</span></span> <span data-ttu-id="6cbf3-444">Funkce na straně klienta předá 5 řetězce a logickou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-444">The client-side function passes 5 strings and a Boolean.</span></span> <span data-ttu-id="6cbf3-445">Další řetězec (který má hodnotu null v příkladu výše) obsahuje funkce na straně klienta, který bude zpracovávat chyby ze zpětného volání na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-445">The additional string (which is null in the above example) contains the client-side function that will handle any errors from the server-side callback.</span></span>

## <a name="step-3--hook-the-client-side-control-event"></a><span data-ttu-id="6cbf3-446">Krok 3: Připojení událostí ovládacího prvku na straně klienta</span><span class="sxs-lookup"><span data-stu-id="6cbf3-446">Step 3 : Hook the Client-Side Control Event</span></span>

<span data-ttu-id="6cbf3-447">Všimněte si, že návratová hodnota GetCallbackEventReference výše byl přiřazen k proměnné řetězce.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-447">Notice that the return value of GetCallbackEventReference above was assigned to a string variable.</span></span> <span data-ttu-id="6cbf3-448">Tento řetězec se používá k připojení události na straně klienta pro ovládací prvek, který iniciuje zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-448">That string is used to hook a client-side event for the control that initiates the callback.</span></span> <span data-ttu-id="6cbf3-449">V tomto příkladu rozevíracího seznamu na stránce inicioval zpětné volání, chci se zapojit *OnChange* událostí.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-449">In this example, the callback is initiated by a dropdown on the page, so I want to hook the *OnChange* event.</span></span>

<span data-ttu-id="6cbf3-450">K připojení události na straně klienta, jednoduše přidejte obslužnou rutinu kód na straně klienta následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="6cbf3-450">To hook the client-side event, simply add a handler to the client-side markup as follows:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

<span data-ttu-id="6cbf3-451">Vzpomeňte si, že *cbRef* je návratová hodnota z volání GetCallbackEventReference.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-451">Recall that *cbRef* is the return value from the call to GetCallbackEventReference.</span></span> <span data-ttu-id="6cbf3-452">Obsahuje volání do webového formuláře\_DoCallback zobrazená výše.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-452">It contains the call to WebForm\_DoCallback that was shown above.</span></span>

## <a name="step-4--register-the-client-side-script"></a><span data-ttu-id="6cbf3-453">Krok 4: Registrace skriptu na straně klienta</span><span class="sxs-lookup"><span data-stu-id="6cbf3-453">Step 4 : Register the Client-Side Script</span></span>

<span data-ttu-id="6cbf3-454">Vzpomínáte, který volání GetCallbackEventReference zadán, skript na straně klienta volána **ShowCompanyName** by byl proveden po úspěšném zpětného volání na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-454">Recall that the call to GetCallbackEventReference specified that a client-side script called **ShowCompanyName** would be executed when the server-side callback succeeds.</span></span> <span data-ttu-id="6cbf3-455">Tento skript je potřeba přidat na stránku pro použití ClientScriptManager instance.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-455">That script needs to be added to the page using a ClientScriptManager instance.</span></span> <span data-ttu-id="6cbf3-456">(Třída ClientScriptManager bude dicussed později v tomto modulu.) To provedete jako v tomto:</span><span class="sxs-lookup"><span data-stu-id="6cbf3-456">(The ClientScriptManager class will be dicussed later in this module.) You do that like so:</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a><span data-ttu-id="6cbf3-457">Krok 5: Volání metody rozhraní ICallbackEventHandler</span><span class="sxs-lookup"><span data-stu-id="6cbf3-457">Step 5 : Call the Methods of the ICallbackEventHandler Interface</span></span>

<span data-ttu-id="6cbf3-458">Rozhraní ICallbackEventHandler obsahuje dvě metody, které je nutné implementovat v kódu.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-458">The ICallbackEventHandler contains two methods that you need to implement in your code.</span></span> <span data-ttu-id="6cbf3-459">Jsou **RaiseCallbackEvent** a **GetCallbackEvent**.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-459">They are **RaiseCallbackEvent** and **GetCallbackEvent**.</span></span>

<span data-ttu-id="6cbf3-460">**RaiseCallbackEvent** přijímá řetězec jako argument a vrátí hodnotu nothing.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-460">**RaiseCallbackEvent** takes a string as an argument and returns nothing.</span></span> <span data-ttu-id="6cbf3-461">Řetězcový argument je předán z volání na straně klienta webového formuláře\_DoCallback.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-461">The string argument is passed from the client-side call to WebForm\_DoCallback.</span></span> <span data-ttu-id="6cbf3-462">V takovém případě je tato hodnota *hodnotu* atribut volá ddlCompany rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-462">In this case, that value is the *value* attribute of the dropdown called ddlCompany.</span></span> <span data-ttu-id="6cbf3-463">Váš kód na straně serveru musí být umístěné ve RaiseCallbackEvent metody.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-463">Your server-side code should be placed in the RaiseCallbackEvent method.</span></span> <span data-ttu-id="6cbf3-464">Například pokud je vaše zpětné volání žádosti WebRequest proti externímu prostředku, tento kód musí být umístěné ve RaiseCallbackEvent.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-464">For example, if your callback is making a WebRequest against an external resource, that code should be placed in RaiseCallbackEvent.</span></span>

<span data-ttu-id="6cbf3-465">**GetCallbackEvent** je zodpovědná za zpracování vrácení zpětné volání do klienta.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-465">**GetCallbackEvent** is responsible for processing the return of the callback to the client.</span></span> <span data-ttu-id="6cbf3-466">Nepřijímá žádné argumenty a vrátí hodnotu typu string.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-466">It takes no arguments and returns a string.</span></span> <span data-ttu-id="6cbf3-467">Řetězec, který vrátí se předá jako argument pro funkci na straně klienta v tomto případě *ShowCompanyName*.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-467">The string that it returns will be passed as an argument to the client-side function, in this case *ShowCompanyName*.</span></span>

<span data-ttu-id="6cbf3-468">Po dokončení výše uvedené kroky, jste připraveni k provádění zpětného volání skriptu v ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-468">Once you have completed the above steps, you are ready to perform a script callback in ASP.NET 2.0.</span></span>


![](the-asp-net-2-0-page-model/_static/image4.png)


[<span data-ttu-id="6cbf3-469">Otevřít Video na celou obrazovku</span><span class="sxs-lookup"><span data-stu-id="6cbf3-469">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/callback1.wmv)


<span data-ttu-id="6cbf3-470">Zpětná volání skriptu v ASP.NET jsou podporovány v jakémkoli prohlížeči, který podporuje provedete XMLHttp volání.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-470">Script callbacks in ASP.NET are supported in any browser that supports making XMLHttp calls.</span></span> <span data-ttu-id="6cbf3-471">To zahrnuje všechny moderní prohlížeče používá ještě dnes.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-471">That includes all of the modern browsers in use today.</span></span> <span data-ttu-id="6cbf3-472">Aplikace Internet Explorer používá objektu XMLHttp ActiveX při použití vnitřního objektu XMLHttp dalších moderních prohlížečů (včetně nadcházející aplikace Internet Explorer 7).</span><span class="sxs-lookup"><span data-stu-id="6cbf3-472">Internet Explorer uses the XMLHttp ActiveX object while other modern browsers (including the upcoming IE 7) use an intrinsic XMLHttp object.</span></span> <span data-ttu-id="6cbf3-473">K určení prostřednictvím kódu programu, pokud je prohlížeč podporuje zpětná volání, můžete použít **Request.Browser.SupportCallback** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-473">To programmatically determine if a browser supports callbacks, you can use the **Request.Browser.SupportCallback** property.</span></span> <span data-ttu-id="6cbf3-474">Tato vlastnost vrátí **true** Pokud klienta, který podporuje zpětná volání skriptu.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-474">This property will return **true** if the requesting client supports script callbacks.</span></span>

## <a name="working-with-client-script-in-aspnet-20"></a><span data-ttu-id="6cbf3-475">Práce s klientským skriptem v technologii ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="6cbf3-475">Working with Client Script in ASP.NET 2.0</span></span>

<span data-ttu-id="6cbf3-476">Skripty klienta v technologii ASP.NET 2.0 jsou spravované prostřednictvím použití třídy ClientScriptManager.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-476">Client scripts in ASP.NET 2.0 are managed via the use of the ClientScriptManager class.</span></span> <span data-ttu-id="6cbf3-477">Třída ClientScriptManager uchovává informace o klientských skriptů pomocí typu a název.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-477">The ClientScriptManager class keeps track of client scripts using a type and a name.</span></span> <span data-ttu-id="6cbf3-478">Díky tomu stejný skript programově vloženého na stránce více než jednou.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-478">This prevents the same script from being programmatically inserted on a page more than once.</span></span>

> [!NOTE]
> <span data-ttu-id="6cbf3-479">Skript po úspěšné registraci na stránce, jakékoli následné pokusy o registraci stejný skript jednoduše výsledkem skriptu není zaregistrovaný podruhé.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-479">After a script has been successfully registered on a page, any subsequent attempt to register the same script will simply result in the script not being registered a second time.</span></span> <span data-ttu-id="6cbf3-480">Budou přidány žádné duplicitní skripty a dojde k žádné výjimce.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-480">No duplicate scripts are added and no exception occurs.</span></span> <span data-ttu-id="6cbf3-481">Aby se zabránilo zbytečným výpočtu, jsou metody, které můžete použít k určení, jestli skript je už zaregistrovaný, takže není pokusí zaregistrovat víc než jednou.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-481">To avoid unnecessary computation, there are methods that you can use to determine if a script is already registered so that you do not attempt to register it more than once.</span></span>


<span data-ttu-id="6cbf3-482">Metody ClientScriptManager by měl být všechny aktuální vývojáře využívající technologii ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="6cbf3-482">The methods of the ClientScriptManager should be familiar to all current ASP.NET developers:</span></span>

## <a name="registerclientscriptblock"></a><span data-ttu-id="6cbf3-483">RegisterClientScriptBlock</span><span class="sxs-lookup"><span data-stu-id="6cbf3-483">RegisterClientScriptBlock</span></span>

<span data-ttu-id="6cbf3-484">Tato metoda přidá do horní části na vykreslené stránce skriptu.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-484">This method adds a script to the top of the rendered page.</span></span> <span data-ttu-id="6cbf3-485">To je užitečné pro přidávání funkcí, které se explicitně volat na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-485">This is useful for adding functions that will be explicitly called on the client.</span></span>

<span data-ttu-id="6cbf3-486">Existují dvě přetížené verze této metody.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-486">There are two overloaded versions of this method.</span></span> <span data-ttu-id="6cbf3-487">Tři ze čtyř argumentů jsou běžné mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-487">Three of four arguments are common among them.</span></span> <span data-ttu-id="6cbf3-488">Možnosti jsou následující:</span><span class="sxs-lookup"><span data-stu-id="6cbf3-488">They are:</span></span>

`type (string)`

<span data-ttu-id="6cbf3-489">***Typ*** argument určuje typ skriptu.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-489">The ***type*** argument identifies a type for the script.</span></span> <span data-ttu-id="6cbf3-490">Obecně je vhodné použít typ stránky (this. GetType()) typu.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-490">It is generally a good idea to use the page's type (this.GetType()) for the type.</span></span>

`key (string)`

<span data-ttu-id="6cbf3-491">***Klíč*** argument je uživatelský klíč pro skript.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-491">The ***key*** argument is a user-defined key for the script.</span></span> <span data-ttu-id="6cbf3-492">To by měl být jedinečný pro každý skript.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-492">This should be unique for each script.</span></span> <span data-ttu-id="6cbf3-493">Pokud se pokusíte přidat skript se stejným klíčem a typ již přidané skriptu, nebude přidán.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-493">If you attempt to add a script with the same key and type of an already added script, it will not be added.</span></span>

`script (string)`

<span data-ttu-id="6cbf3-494">***Skript*** argument je řetězec obsahující skutečné skript pro přidání.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-494">The ***script*** argument is a string containing the actual script to add.</span></span> <span data-ttu-id="6cbf3-495">Je doporučeno použít StringBuilder vytvořit skript a pak použijte metodu ToString() ve třídě StringBuilder nebyla přiřadit ***skript*** argument.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-495">It's recommended that you use a StringBuilder to create the script and then use the ToString() method on the StringBuilder to assign the ***script*** argument.</span></span>

<span data-ttu-id="6cbf3-496">Pokud používáte přetížené RegisterClientScriptBlock, který zabere jenom tři argumenty, musí obsahovat prvky skriptu (&lt;skript&gt; a &lt;/script&gt;) ve vašem skriptu.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-496">If you use the overloaded RegisterClientScriptBlock that only takes three arguments, you must include script elements (&lt;script&gt; and &lt;/script&gt;) in your script.</span></span>

<span data-ttu-id="6cbf3-497">Můžete použít přetížení RegisterClientScriptBlock, která přijímá čtvrtý argument.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-497">You may choose to use the overload of RegisterClientScriptBlock that takes a fourth argument.</span></span> <span data-ttu-id="6cbf3-498">Čtvrtý argument je logická hodnota, která určuje, zda technologie ASP.NET přidala prvky skriptu za vás.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-498">The fourth argument is a Boolean that specifies whether or not ASP.NET should add script elements for you.</span></span> <span data-ttu-id="6cbf3-499">Pokud je tento argument **true**, váš skript by neměla obsahovat prvky skriptu explicitně.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-499">If this argument is **true**, your script should not include the script elements explicitly.</span></span>

<span data-ttu-id="6cbf3-500">Pomocí této metody IsClientScriptBlockRegistered určit, jestli je skript už zaregistrovaný.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-500">Use the IsClientScriptBlockRegistered method to determine if a script has already been registered.</span></span> <span data-ttu-id="6cbf3-501">To umožňuje vyhnout se znovu zaregistrovat skript, který už je zaregistrovaný.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-501">This allows you to avoid an attempt to re-register a script that has already been registered.</span></span>

### <a name="registerclientscriptinclude-new-in-20"></a><span data-ttu-id="6cbf3-502">RegisterClientScriptInclude (nově ve verzi 2.0)</span><span class="sxs-lookup"><span data-stu-id="6cbf3-502">RegisterClientScriptInclude (New in 2.0)</span></span>

<span data-ttu-id="6cbf3-503">Značka RegisterClientScriptInclude vytvoří blok skriptu odkazující na externí soubor skriptu.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-503">The RegisterClientScriptInclude tag creates a script block that links to an external script file.</span></span> <span data-ttu-id="6cbf3-504">Má dvě přetížení.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-504">It has two overloads.</span></span> <span data-ttu-id="6cbf3-505">Přijímá jeden klíč a adresu URL.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-505">One takes a key and a URL.</span></span> <span data-ttu-id="6cbf3-506">Druhá přidá třetí argument určující typ.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-506">The second adds a third argument specifying the type.</span></span>

<span data-ttu-id="6cbf3-507">Následující kód například vygeneruje blok skriptu, který odkazuje na jsfunctions.js v kořenové složce skriptů aplikace:</span><span class="sxs-lookup"><span data-stu-id="6cbf3-507">For example, the following code generates a script block that links to jsfunctions.js in the root of the scripts folder of the application:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

<span data-ttu-id="6cbf3-508">Tento kód vytvoří následující kód na vykreslené stránce:</span><span class="sxs-lookup"><span data-stu-id="6cbf3-508">This code produces the following code in the rendered page:</span></span>

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> <span data-ttu-id="6cbf3-509">Blok skriptu se vykreslí v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-509">The script block is rendered at the bottom of the page.</span></span>


<span data-ttu-id="6cbf3-510">Pomocí této metody IsClientScriptIncludeRegistered určit, jestli je skript už zaregistrovaný.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-510">Use the IsClientScriptIncludeRegistered method to determine if a script has already been registered.</span></span> <span data-ttu-id="6cbf3-511">To umožňuje vyhnout se znovu zaregistrovat skript.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-511">This allows you to avoid an attempt to re-register a script.</span></span>

## <a name="registerstartupscript"></a><span data-ttu-id="6cbf3-512">RegisterStartupScript</span><span class="sxs-lookup"><span data-stu-id="6cbf3-512">RegisterStartupScript</span></span>

<span data-ttu-id="6cbf3-513">Metoda RegisterStartupScript používá stejné argumenty jako metodu RegisterClientScriptBlock.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-513">The RegisterStartupScript method takes the same arguments as the RegisterClientScriptBlock method.</span></span> <span data-ttu-id="6cbf3-514">Skript zaregistrovaného RegisterStartupScript provádí po načtení stránky, ale před událostí načtení na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-514">A script registered with RegisterStartupScript executes after the page loads but before the OnLoad client-side event.</span></span> <span data-ttu-id="6cbf3-515">V 1.X, skripty, které jsou zaregistrované RegisterStartupScript zadal těsně před uzavírací &lt;/form&gt; označit při skripty zaregistrovaného RegisterClientScriptBlock byly umístěny ihned po otevření &lt;formuláře&gt; značky.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-515">In 1.X, scripts registered with RegisterStartupScript were placed just before the closing &lt;/form&gt; tag while scripts registered with RegisterClientScriptBlock were placed immediately after the opening &lt;form&gt; tag.</span></span> <span data-ttu-id="6cbf3-516">V technologii ASP.NET 2.0, obě jsou umístěné bezprostředně před uzavírací &lt;/form&gt; značky.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-516">In ASP.NET 2.0, both are placed immediately before the closing &lt;/form&gt; tag.</span></span>

> [!NOTE]
> <span data-ttu-id="6cbf3-517">Když si zaregistrujete funkce s RegisterStartupScript, tato funkce nebude spuštěno, dokud ho explicitně volat v kódu na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-517">If you register a function with RegisterStartupScript, that function will not execute until you explicitly call it in client-side code.</span></span>


<span data-ttu-id="6cbf3-518">Pomocí metody IsStartupScriptRegistered můžete určit, jestli je skript už zaregistrovaný a vyhnout se znovu zaregistrovat skript.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-518">Use the IsStartupScriptRegistered method to determine if a script has already been registered and avoid an attempt to re-register a script.</span></span>

## <a name="other-clientscriptmanager-methods"></a><span data-ttu-id="6cbf3-519">Jiné metody ClientScriptManager</span><span class="sxs-lookup"><span data-stu-id="6cbf3-519">Other ClientScriptManager Methods</span></span>

<span data-ttu-id="6cbf3-520">Tady jsou některé z dalších užitečných metod ClientScriptManager třídy.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-520">Here are some of the other useful methods of the ClientScriptManager class.</span></span>


|  <span data-ttu-id="6cbf3-521"><strong>GetCallbackEventReference</strong></span><span class="sxs-lookup"><span data-stu-id="6cbf3-521"><strong>GetCallbackEventReference</strong></span></span>   |                                                 <span data-ttu-id="6cbf3-522">Viz zpětná volání skriptu dříve v tomto modulu.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-522">See script callbacks earlier in this module.</span></span>                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <span data-ttu-id="6cbf3-523"><strong>GetPostBackClientHyperlink</strong></span><span class="sxs-lookup"><span data-stu-id="6cbf3-523"><strong>GetPostBackClientHyperlink</strong></span></span>  |                <span data-ttu-id="6cbf3-524">Získá odkaz JavaScript (jazyk javascript:&lt;volání&gt;), který je možné publikovat zpět z události na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-524">Gets a JavaScript reference (javascript:&lt;call&gt;) that can be used to post back from a client-side event.</span></span>                 |
|  <span data-ttu-id="6cbf3-525"><strong>GetPostBackEventReference</strong></span><span class="sxs-lookup"><span data-stu-id="6cbf3-525"><strong>GetPostBackEventReference</strong></span></span>   |                                   <span data-ttu-id="6cbf3-526">Získá řetězec, který slouží k zahájení příspěvek zpět od klienta.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-526">Gets a string that can be used to initiate a post back from the client.</span></span>                                    |
|      <span data-ttu-id="6cbf3-527"><strong>GetWebResourceUrl</strong></span><span class="sxs-lookup"><span data-stu-id="6cbf3-527"><strong>GetWebResourceUrl</strong></span></span>       | <span data-ttu-id="6cbf3-528">Vrátí adresu URL k prostředku, který je vložen do sestavení.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-528">Returns a URL to a resource that is embedded in an assembly.</span></span> <span data-ttu-id="6cbf3-529">Je potřeba použít ve spojení s <strong>RegisterClientScriptResource</strong>.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-529">Must be used in conjunction with <strong>RegisterClientScriptResource</strong>.</span></span> |
| <span data-ttu-id="6cbf3-530"><strong>RegisterClientScriptResource</strong></span><span class="sxs-lookup"><span data-stu-id="6cbf3-530"><strong>RegisterClientScriptResource</strong></span></span> |     <span data-ttu-id="6cbf3-531">Zaregistruje prostředek webové stránky.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-531">Registers a Web resource with the page.</span></span> <span data-ttu-id="6cbf3-532">Toto jsou prostředky součástí sestavení a zpracovat nová obslužná rutina WebResource.axd.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-532">These are resources embedded in an assembly and handled by the new WebResource.axd handler.</span></span>      |
|     <span data-ttu-id="6cbf3-533"><strong>RegisterHiddenField</strong></span><span class="sxs-lookup"><span data-stu-id="6cbf3-533"><strong>RegisterHiddenField</strong></span></span>      |                                                 <span data-ttu-id="6cbf3-534">Zaregistruje skryté pole formuláře se stránkou.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-534">Registers a hidden form field with the page.</span></span>                                                 |
|  <span data-ttu-id="6cbf3-535"><strong>RegisterOnSubmitStatement</strong></span><span class="sxs-lookup"><span data-stu-id="6cbf3-535"><strong>RegisterOnSubmitStatement</strong></span></span>   |                                  <span data-ttu-id="6cbf3-536">Zaregistruje kód na straně klienta, který se spustí, když se odešle formulář HTML.</span><span class="sxs-lookup"><span data-stu-id="6cbf3-536">Registers client-side code that executes when the HTML form is submitted.</span></span>                                   |

