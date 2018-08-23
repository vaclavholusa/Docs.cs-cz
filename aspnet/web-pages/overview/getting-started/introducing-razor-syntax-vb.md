---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: Úvod k programování v rozhraní ASP.NET Web používající syntaxi Razor (Visual Basic) | Dokumentace Microsoftu
author: tfitzmac
description: Tento dodatek poskytuje přehled o programování s webovými stránkami ASP.NET v jazyce Visual Basic pomocí syntaxe Razor.
ms.author: riande
ms.date: 02/07/2014
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: cbec035533c37723afcd5bf4aa0c6e1c83dbae23
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756843"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a><span data-ttu-id="2a6b8-103">Úvod k programování v rozhraní ASP.NET Web používající syntaxi Razor (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="2a6b8-103">Introduction to ASP.NET Web Programming Using the Razor Syntax (Visual Basic)</span></span>
====================
<span data-ttu-id="2a6b8-104">podle [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2a6b8-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="2a6b8-105">Tento článek obsahuje přehled programování s webovými stránkami ASP.NET pomocí syntaxe Razor a Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-105">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax and Visual Basic.</span></span> <span data-ttu-id="2a6b8-106">ASP.NET je technologie od Microsoftu pro spouštění dynamické webové stránky na webové servery.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-106">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span>
> 
> <span data-ttu-id="2a6b8-107">**Co se dozvíte**:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-107">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="2a6b8-108">Začátek 8 Programovací tipy pro zahájení práce s programování pomocí syntaxe Razor rozhraní ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-108">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="2a6b8-109">Základní koncepty programování, které budete potřebovat.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-109">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="2a6b8-110">Jaký kód serveru technologie ASP.NET a syntaxe Razor je všechno o.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-110">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="2a6b8-111">Verze softwaru</span><span class="sxs-lookup"><span data-stu-id="2a6b8-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="2a6b8-112">Webové stránky ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="2a6b8-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="2a6b8-113">V tomto kurzu se také pracuje s ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="2a6b8-114">Většina příkladů použití rozhraní ASP.NET Web Pages se syntaxí Razor pomocí C#.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-114">Most examples of using ASP.NET Web Pages with Razor syntax use C#.</span></span> <span data-ttu-id="2a6b8-115">Ale syntaxe Razor také podporuje jazyka Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-115">But the Razor syntax also supports Visual Basic.</span></span> <span data-ttu-id="2a6b8-116">Programování v jazyce Visual Basic webové stránky ASP.NET, vytvoříte webovou stránku pomocí *.vbhtml* příponu názvu souboru a poté přidejte kód jazyka Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-116">To program an ASP.NET web page in Visual Basic, you create a web page with a *.vbhtml* filename extension, and then add Visual Basic code.</span></span> <span data-ttu-id="2a6b8-117">Tento článek obsahuje přehled práce s jazykem Visual Basic a syntaxe pro vytvoření webových stránek ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-117">This article gives you an overview of working with the Visual Basic language and syntax to create ASP.NET Webpages.</span></span>

> [!NOTE]
> <span data-ttu-id="2a6b8-118">Výchozí šablony webu pro Microsoft WebMatrix (**pekařství**, **Fotogalerie**, a **Starter Site**atd) jsou k dispozici v jazyce C# a Visual Basic verze.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-118">The default website templates for Microsoft WebMatrix (**Bakery**, **Photo Gallery**, and **Starter Site**, etc.) are available in C# and Visual Basic versions.</span></span> <span data-ttu-id="2a6b8-119">Šablony jazyka Visual Basic, tak můžete nainstalovat jako balíčky NuGet.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-119">You can install the Visual Basic templates by as NuGet packages.</span></span> <span data-ttu-id="2a6b8-120">Šablony webu jsou nainstalovány v kořenové složce webu do složky s názvem *Templates Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-120">Website templates are installed in the root folder of your site in a folder named *Microsoft Templates*.</span></span>


## <a name="the-top-8-programming-tips"></a><span data-ttu-id="2a6b8-121">Začátek 8 Tipy pro programování</span><span class="sxs-lookup"><span data-stu-id="2a6b8-121">The Top 8 Programming Tips</span></span>

<span data-ttu-id="2a6b8-122">Tato část uvádí několik tipů, které je nezbytně potřeba ví, jak můžete začít psát kód serveru ASP.NET pomocí syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-122">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="2a6b8-123">1. Přidejte kód do stránky pomocí znak @</span><span class="sxs-lookup"><span data-stu-id="2a6b8-123">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="2a6b8-124">`@` Začne znak vložené výrazy, jedním příkazem bloky a bloky více příkazů:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-124">The `@` character starts inline expressions, single-statement blocks, and multi-statement blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

<span data-ttu-id="2a6b8-125">Výsledek zobrazí v prohlížeči:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-125">The result displayed in a browser:</span></span>

![Razor Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> <span data-ttu-id="2a6b8-127">**Kódování HTML**</span><span class="sxs-lookup"><span data-stu-id="2a6b8-127">**HTML Encoding**</span></span>
> 
> <span data-ttu-id="2a6b8-128">Při zobrazení obsahu do stránky pomocí `@` znaků, stejně jako v předchozích příkladech ASP.NET umístí kódování HTML výstup.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-128">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="2a6b8-129">To nahrazuje vyhrazené znaky HTML (například `<` a `>` a `&`) s kódy, které umožňují znaků, který se má zobrazit jako znaků na webové stránce nebude interpretován jako značku jazyka HTML nebo entity.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-129">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="2a6b8-130">Bez kódování HTML výstup z kódu serveru se nemusí zobrazit správně a může zpřístupnit stránky na bezpečnostní rizika.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-130">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="2a6b8-131">Pokud je vaším cílem je výstup kód HTML, který vykreslí značky jako značka (třeba `<p></p>` odstavce nebo `<em></em>` pro zvýraznění textu), najdete v části [kombinaci textu, značek a kódu v blocích kódu](#BM_CombiningTextMarkupAndCode) dále v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-131">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="2a6b8-132">Další informace o kódování HTML v [práce s formuláři HTML v ASP.NET Web Pages Sites](https://go.microsoft.com/fwlink/?LinkId=202892).</span><span class="sxs-lookup"><span data-stu-id="2a6b8-132">You can read more about HTML encoding in [Working with HTML Forms in ASP.NET Web Pages Sites](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>


### <a name="2-you-enclose-code-blocks-with-codeend-code"></a><span data-ttu-id="2a6b8-133">2. Použijte bloky kódu s kódem... Kód konce</span><span class="sxs-lookup"><span data-stu-id="2a6b8-133">2. You enclose code blocks with Code...End Code</span></span>

<span data-ttu-id="2a6b8-134">Blok kódu obsahuje jeden nebo více příkazů kódu a je uzavřené s klíčovými slovy `Code` a `End Code`.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-134">A code block includes one or more code statements and is enclosed with the keywords `Code` and `End Code`.</span></span> <span data-ttu-id="2a6b8-135">Umístit otevírání `Code` – klíčové slovo ihned po `@` znak &#8212; nemůže být prázdné znaky mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-135">Place the opening `Code` keyword immediately after the `@` character &#8212; there can't be whitespace between them.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

<span data-ttu-id="2a6b8-136">Výsledek zobrazí v prohlížeči:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-136">The result displayed in a browser:</span></span>

![Razor Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a><span data-ttu-id="2a6b8-138">3. Uvnitř bloku ukončit každý příkaz kódu se konec řádku</span><span class="sxs-lookup"><span data-stu-id="2a6b8-138">3. Inside a block, you end each code statement with a line break</span></span>

<span data-ttu-id="2a6b8-139">V bloku kódu jazyka Visual Basic každý příkaz končí konec řádku.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-139">In a Visual Basic code block, each statement ends with a line break.</span></span> <span data-ttu-id="2a6b8-140">(Dále v tomto článku uvidíte způsob, jak zabalit příkaz dlouhé kódu do více řádků v případě potřeby.)</span><span class="sxs-lookup"><span data-stu-id="2a6b8-140">(Later in the article you'll see a way to wrap a long code statement into multiple lines if needed.)</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="2a6b8-141">4. Použití proměnných k ukládání hodnot</span><span class="sxs-lookup"><span data-stu-id="2a6b8-141">4. You use variables to store values</span></span>

<span data-ttu-id="2a6b8-142">Můžete ukládat hodnoty *proměnnou*, včetně řetězců, čísel a data, atd. Vytvoření nové proměnné pomocí `Dim` – klíčové slovo.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-142">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `Dim` keyword.</span></span> <span data-ttu-id="2a6b8-143">Hodnoty proměnných můžete vložit přímo do stránky pomocí `@`.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-143">You can insert variable values directly in a page using `@`.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

<span data-ttu-id="2a6b8-144">Výsledek zobrazí v prohlížeči:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-144">The result displayed in a browser:</span></span>

![Razor Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="2a6b8-146">5. Použijte hodnoty řetězcový literál v uvozovkách</span><span class="sxs-lookup"><span data-stu-id="2a6b8-146">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="2a6b8-147">A *řetězec* je posloupnost znaků, které jsou považovány za text.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-147">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="2a6b8-148">Pokud chcete zadat řetězec, uzavřete do dvojitých uvozovek:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-148">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

<span data-ttu-id="2a6b8-149">K vložení uvozovek v rámci hodnotu řetězce, vložte dva znaky uvozovek.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-149">To embed double quotation marks within a string value, insert two double quotation mark characters.</span></span> <span data-ttu-id="2a6b8-150">Pokud chcete znak dvojité uvozovky, které chcete zobrazit jednou v výstup stránky, zadejte ho jako `""` v rámci citovaný řetězec a pokud chcete, aby se dvakrát zobrazí, zadejte ho jako `""""` v rámci řetězec v uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-150">If you want the double quotation character to appear once in the page output, enter it as `""` within the quoted string, and if you want it to appear twice, enter it as `""""` within the quoted string.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

<span data-ttu-id="2a6b8-151">Výsledek zobrazí v prohlížeči:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-151">The result displayed in a browser:</span></span>

![Razor Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a><span data-ttu-id="2a6b8-153">6. Kód jazyka Visual Basic není malá a velká písmena</span><span class="sxs-lookup"><span data-stu-id="2a6b8-153">6. Visual Basic code is not case sensitive</span></span>

<span data-ttu-id="2a6b8-154">Jazyk Visual Basic není malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-154">The Visual Basic language is not case sensitive.</span></span> <span data-ttu-id="2a6b8-155">Programování klíčová slova (stejně jako `Dim`, `If`, a `True`) a názvy proměnných (například `myString`, nebo `subTotal`) může být napsán v každém případě.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-155">Programming keywords (like `Dim`, `If`, and `True`) and variable names (like `myString`, or `subTotal`) can be written in any case.</span></span>

<span data-ttu-id="2a6b8-156">Následující řádky kódu přiřadit hodnotu k proměnné `lastname` pomocí malé název a potom výstupní hodnoty proměnné stránky pomocí názvu velká písmena.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-156">The following lines of code assign a value to the variable `lastname` using a lowercase name, and then output the variable value to the page using an uppercase name.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

<span data-ttu-id="2a6b8-157">Výsledek zobrazí v prohlížeči:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-157">The result displayed in a browser:</span></span>

![vb-syntax-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a><span data-ttu-id="2a6b8-159">7. Velká část psaní kódu zahrnuje práci s objekty</span><span class="sxs-lookup"><span data-stu-id="2a6b8-159">7. Much of your coding involves working with objects</span></span>

<span data-ttu-id="2a6b8-160">Objekt představuje věc, kterou můžete programovat s &#8212; stránku, textové pole, soubor, obrázek, webový požadavek, e-mailovou zprávu, záznam zákazníka (řádek databáze), atd. Objekty mají vlastnosti, které popisují jejich vlastnosti &#8212; textový objekt pole má `Text` vlastnost, má objekt žádosti `Url` vlastnost, nemá e-mailovou zprávu `From` vlastnost a objekt zákazníka má `FirstName` Vlastnost.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-160">An object represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics &#8212; a text box object has a `Text` property, a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="2a6b8-161">Objekty mají také metody, které jsou &quot;příkazy&quot; mohou provádět.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-161">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="2a6b8-162">Mezi příklady patří objektu soubor `Save` metoda, objektu image `Rotate` metoda a objekt e-mailu `Send` – metoda.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-162">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="2a6b8-163">Často budete pracovat `Request` pole objektu, který poskytuje informace, například hodnoty formuláře na stránce (textových polí atd.), jaký typ prohlížeče přišel požadavek, adresa URL stránky, identita uživatele, atd. Tento příklad ukazuje způsob pro přístup k vlastnostem `Request` objekt a jak volat `MapPath` metodu `Request` objektu, který obsahuje absolutní cestu na stránce na serveru:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-163">You'll often work with the `Request` object, which gives you information like the values of form fields on the page (text boxes, etc.), what type of browser made the request, the URL of the page, the user identity, etc. This example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

<span data-ttu-id="2a6b8-164">Výsledek zobrazí v prohlížeči:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-164">The result displayed in a browser:</span></span>

![Razor Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="2a6b8-166">8. Můžete napsat kód, který provádí rozhodnutí</span><span class="sxs-lookup"><span data-stu-id="2a6b8-166">8. You can write code that makes decisions</span></span>

<span data-ttu-id="2a6b8-167">Klíčovou funkcí dynamické webové stránky je, že určíte, co se provedou v závislosti na podmínkách.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-167">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="2a6b8-168">Nejběžnější způsob je pomocí `If` – příkaz (a volitelné `Else` příkaz).</span><span class="sxs-lookup"><span data-stu-id="2a6b8-168">The most common way to do this is with the `If` statement (and optional `Else` statement).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

<span data-ttu-id="2a6b8-169">Příkaz `If IsPost` je zjednodušený způsob psaní `If IsPost = True`.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-169">The statement `If IsPost` is a shorthand way of writing `If IsPost = True`.</span></span> <span data-ttu-id="2a6b8-170">Spolu s `If` příkazy, existuje široká škála způsobů, jak otestovat podmínky, při opakovaném bloky kódu, a tak dále, které jsou popsány dále v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-170">Along with `If` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="2a6b8-171">Výsledek zobrazí v prohlížeči (po kliknutí na tlačítko **odeslat**):</span><span class="sxs-lookup"><span data-stu-id="2a6b8-171">The result displayed in a browser (after clicking **Submit**):</span></span>

![Razor Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> <span data-ttu-id="2a6b8-173">**HTTP GET a POST metody a vlastnosti IsPost**</span><span class="sxs-lookup"><span data-stu-id="2a6b8-173">**HTTP GET and POST Methods and the IsPost Property**</span></span>
> 
> <span data-ttu-id="2a6b8-174">Protokol použitý pro webové stránky (HTTP) podporuje velmi omezený počet metod (&quot;příkazy&quot;), která se používají k provádění požadavků na server.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-174">The protocol used for web pages (HTTP) supports a very limited number of methods (&quot;verbs&quot;) that are used to make requests to the server.</span></span> <span data-ttu-id="2a6b8-175">Nichž dva nejčastější jsou GET, který slouží k načtení stránky a příspěvku, který se používá k odeslání stránky.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-175">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="2a6b8-176">Obecně platí uživatel požádá o stránku při prvním požadavku na stránku pomocí GET.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-176">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="2a6b8-177">Pokud uživatel vyplní ve formuláři a poté klikněte na tlačítko **odeslat**, prohlížeč odešle požadavek POST na server.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-177">If the user fills in a form and then clicks **Submit**, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="2a6b8-178">Ve webovém programování často je užitečné vědět, jestli na stránce jsou požadovány jako GET nebo POST, abyste věděli, jak zpracování stránky.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-178">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="2a6b8-179">ASP.NET Web Pages, můžete použít `IsPost` vlastnosti chcete zobrazit, zda je požadavek GET nebo POST.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-179">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="2a6b8-180">Pokud je příspěvek, požadavek `IsPost` vlastnost vrátí hodnotu PRAVDA, a můžete provádět věci, jako je čtení hodnoty polí ve formuláři.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-180">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="2a6b8-181">Mnoho příkladů, zobrazí se vám ukazují, jak zpracovat stránce odlišně v závislosti na hodnotě `IsPost`.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-181">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>


## <a name="a-simple-code-example"></a><span data-ttu-id="2a6b8-182">Jednoduchým příkladem kódu</span><span class="sxs-lookup"><span data-stu-id="2a6b8-182">A Simple Code Example</span></span>

<span data-ttu-id="2a6b8-183">Tento postup ukazuje, jak vytvořit stránku, která ukazuje základní programovací techniky.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-183">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="2a6b8-184">V tomto příkladu vytvoříte stránky, která umožňuje uživatelům zadat dvě čísla, potom se přidají a zobrazí výsledek.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-184">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="2a6b8-185">V editoru, vytvořte nový soubor s názvem *AddNumbers.vbhtml*.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-185">In your editor, create a new file and name it *AddNumbers.vbhtml*.</span></span>
2. <span data-ttu-id="2a6b8-186">Zkopírujte následující kód a kód na stránku, nahrazení nic již na stránce.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-186">Copy the following code and markup into the page, replacing anything already in the page.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    <span data-ttu-id="2a6b8-187">Tady jsou některé věci za vás mějte na paměti:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-187">Here are some things for you to note:</span></span>

    - <span data-ttu-id="2a6b8-188">`@` Znak spustí první blok kódu ve stránce, a předchází `totalMessage` vložené proměnné dole.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-188">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable embedded near the bottom.</span></span>
    - <span data-ttu-id="2a6b8-189">Bloku v horní části stránky je ohraničen `Code...End Code`.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-189">The block at the top of the page is enclosed in `Code...End Code`.</span></span>
    - <span data-ttu-id="2a6b8-190">Proměnné `total`, `num1`, `num2`, a `totalMessage` uložení několika čísla a řetězce.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="2a6b8-191">Řetězcový literál hodnota přiřazená `totalMessage` proměnná je do dvojitých uvozovek.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="2a6b8-192">Protože kód jazyka Visual Basic není velká a malá písmena, když `totalMessage` proměnná se používá v dolní části stránky, jeho název potřebuje pouze tak, aby odpovídaly pravopisu deklarace proměnné v horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-192">Because Visual Basic code is not case sensitive, when the `totalMessage` variable is used near the bottom of the page, its name only needs to match the spelling of the variable declaration at the top of the page.</span></span> <span data-ttu-id="2a6b8-193">Malých a velkých písmen nezáleží.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-193">The casing doesn't matter.</span></span>
    - <span data-ttu-id="2a6b8-194">Výraz `num1.AsInt()`  +  `num2.AsInt()` ukazuje, jak pracovat s objekty a metody.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-194">The expression `num1.AsInt()` + `num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="2a6b8-195">`AsInt` Metodu na každou proměnnou převede řetězec zadaný uživatelem na celé číslo (integer), které mohou být přidány.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-195">The `AsInt` method on each variable converts the string entered by a user to a whole number (an integer) that can be added.</span></span>
    - <span data-ttu-id="2a6b8-196">`<form>` Značka zahrnuje `method="post"` atribut.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-196">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="2a6b8-197">Toto určuje, že když uživatel klikne **přidat**, stránky se odešlou na server pomocí metody POST protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-197">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="2a6b8-198">Při odeslání stránky, kód `If IsPost` vyhodnotí na hodnotu true a podmíněný kód spuštění zobrazení výsledek přidání čísla.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-198">When the page is submitted, the code `If IsPost` evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="2a6b8-199">Uložit na stránku a spustíte ji v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-199">Save the page and run it in a browser.</span></span> <span data-ttu-id="2a6b8-200">(Ujistěte se, že je vybrána na stránce v **soubory** pracovního prostoru před jeho spuštěním.) Zadejte dvěma celými čísly a poté klikněte **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-200">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span>

    ![Razor Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a><span data-ttu-id="2a6b8-202">Syntaxe a jazyk Visual Basic</span><span class="sxs-lookup"><span data-stu-id="2a6b8-202">Visual Basic Language and Syntax</span></span>

<span data-ttu-id="2a6b8-203">Dříve jste viděli základní příklad, jak vytvořit webovou stránku ASP.NET a jak můžete přidat kód serveru do kódu HTML.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-203">Earlier you saw a basic example of how to create an ASP.NET web page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="2a6b8-204">Zde se dozvíte základní informace o používání jazyka Visual Basic k zápisu kódu serveru ASP.NET pomocí syntaxe Razor &#8212; tedy programovací jazyk pravidla.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-204">Here you'll learn the basics of using Visual Basic to write ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="2a6b8-205">Pokud máte zkušenosti s programování (zejména pokud jste použili C, C++, C#, Visual Basic nebo JavaScript), velkou část můžete přečíst tady bude známé.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-205">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="2a6b8-206">Budete pravděpodobně potřebovat pouze s jak služba WebMatrix kód přidá značku v *.vbhtml* soubory.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-206">You'll probably need to familiarize yourself only with how WebMatrix code is added to markup in *.vbhtml* files.</span></span>

### <a id="BM_CombiningTextMarkupAndCode"></a>  <span data-ttu-id="2a6b8-207">Kombinování textu, značek a kódu v blocích kódu</span><span class="sxs-lookup"><span data-stu-id="2a6b8-207">Combining text, markup, and code in code blocks</span></span>

<span data-ttu-id="2a6b8-208">V blocích kódu serveru budete často chcete výstup text a značku na stránku.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-208">In server code blocks, you'll often want to output text and markup to the page.</span></span> <span data-ttu-id="2a6b8-209">Pokud blok kódu serveru obsahuje text, který není kódu a který místo toho má být vykreslen jako je, musí být nedokáží rozlišit tento text z kódu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-209">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="2a6b8-210">Chcete-li to provést několika způsoby.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-210">There are several ways to do this.</span></span>

- <span data-ttu-id="2a6b8-211">Uzavřete text do bloku element HTML jako `<p></p>` nebo `<em></em>`:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-211">Enclose the text in an HTML block element like `<p></p>` or `<em></em>`:</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    <span data-ttu-id="2a6b8-212">HTML element mohou zahrnovat text, další prvky jazyka HTML a výrazy kódu serveru.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-212">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="2a6b8-213">Když ASP.NET uvidí počáteční značky HTML (například `<p>`), všechno, co se vykreslí element a její obsah, jako je prohlížeče (a odstraňuje výrazů kódu na serveru).</span><span class="sxs-lookup"><span data-stu-id="2a6b8-213">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything the element and its content as is to the browser (and resolves the server-code expressions).</span></span>

- <span data-ttu-id="2a6b8-214">Použití `@:` operátor nebo `<text>` elementu.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-214">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="2a6b8-215">`@:` Výstupy jednořádkový obsah, který obsahuje prostý text nebo nespárované značky jazyka HTML; `<text>` element vloží několik řádků výstupu.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-215">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="2a6b8-216">Tyto možnosti jsou užitečné, pokud nechcete, aby k vykreslení elementu HTML jako část ve výstupu.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-216">These options are useful when you don't want to render an HTML element as part of the output.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    <span data-ttu-id="2a6b8-217">V následujícím příkladu se opakuje v předchozím příkladu, ale používá jednu dvojici `<text>` značky uzavřít text k vykreslení.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-217">The following example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    <span data-ttu-id="2a6b8-218">V následujícím příkladu `<text>` a `</text>` značky uzavřít tři řádky, které mají některé uncontained text a bezkonkurenční značky HTML (`<br />`), spolu s kódu serveru a odpovídající značky jazyka HTML.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-218">In the following example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="2a6b8-219">Znovu, může také předcházet každý řádek zvlášť `@:` operátor; buď způsob, jak funguje.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-219">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > <span data-ttu-id="2a6b8-220">Když text výstupu jak je znázorněno v této části &#8212; pomocí HTML elementu, `@:` operátor nebo `<text>` element &#8212; ASP.NET není s kódováním HTML výstup.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-220">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="2a6b8-221">(Jak je uvedeno výše, ASP.NET kódování výstup výrazů kódu na server a server bloky kódu, které předchází `@`, s výjimkou zvláštní případy, které jste si poznamenali v této části.)</span><span class="sxs-lookup"><span data-stu-id="2a6b8-221">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="2a6b8-222">Prázdné znaky</span><span class="sxs-lookup"><span data-stu-id="2a6b8-222">Whitespace</span></span>

<span data-ttu-id="2a6b8-223">Nadbytečné mezery v příkazu (i mimo něj řetězcový literál) nemají vliv – příkaz:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-223">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a><span data-ttu-id="2a6b8-224">Rozdělení dlouhé příkazy do více řádků</span><span class="sxs-lookup"><span data-stu-id="2a6b8-224">Breaking long statements into multiple lines</span></span>

<span data-ttu-id="2a6b8-225">Příkaz dlouhé kódu lze rozdělit na více řádků pomocí znaku podtržítka `_` (v jazyce Visual Basic tzv. *znak pro pokračování*) po každém řádku kódu.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-225">You can break a long code statement into multiple lines by using the underscore character `_` (which in Visual Basic is called the *continuation character*) after each line of code.</span></span> <span data-ttu-id="2a6b8-226">Chcete-li break – příkaz na další řádek, na konci řádku přidejte mezeru a potom znak pro pokračování.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-226">To break a statement onto the next line, at the end of the line add a space and then the continuation character.</span></span> <span data-ttu-id="2a6b8-227">Příkaz pokračujte na dalším řádku.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-227">Continue the statement on the next line.</span></span> <span data-ttu-id="2a6b8-228">Můžete zabalit příkazy na libovolný počet řádků, kolik potřebujete, aby se zlepšila čitelnost.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-228">You can wrap statements onto as many lines as you need to improve readability.</span></span> <span data-ttu-id="2a6b8-229">Následující příkazy jsou stejné:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-229">The following statements are the same:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

<span data-ttu-id="2a6b8-230">Nelze však zalomení řádku uprostřed řetězcový literál.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-230">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="2a6b8-231">Nebude fungovat v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-231">The following example doesn't work:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

<span data-ttu-id="2a6b8-232">Kombinování dlouhý řetězec, který obtéká stejně jako ve výše uvedeném kódu více řádků, je třeba použít *operátoru pro zřetězení* (`&`), což uvidíte dále v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-232">To combine a long string that wraps to multiple lines like the above code, you would need to use the *concatenation operator* (`&`), which you'll see later in this article.</span></span>

### <a name="code-comments"></a><span data-ttu-id="2a6b8-233">Komentáře kódu</span><span class="sxs-lookup"><span data-stu-id="2a6b8-233">Code comments</span></span>

<span data-ttu-id="2a6b8-234">Komentáře umožňují ponechte poznámky k sobě nebo jiné.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-234">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="2a6b8-235">Komentáře syntaxe Razor mají předponu `@*` a na konci `*@`.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-235">Razor syntax comments are prefixed with `@*` and end with `*@`.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

<span data-ttu-id="2a6b8-236">V rámci bloků kódu můžete použít komentáře syntaxe Razor, nebo můžete použít běžný znak komentář jazyka Visual Basic, což je jednoduchá uvozovka (`'`) předponu pro každý řádek.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-236">Within code blocks you can use the Razor syntax comments, or you can use ordinary Visual Basic comment character, which is a single quote (`'`) prefixed to each line.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a><span data-ttu-id="2a6b8-237">Proměnné</span><span class="sxs-lookup"><span data-stu-id="2a6b8-237">Variables</span></span>

<span data-ttu-id="2a6b8-238">Proměnná je pojmenovaný objekt, který použijete k ukládání dat.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-238">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="2a6b8-239">Můžete pojmenovat proměnné cokoli, ale název musí začínat znakem abecedy a nesmí obsahovat prázdné znaky nebo vyhrazené znaky.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-239">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span> <span data-ttu-id="2a6b8-240">V jazyce Visual Basic protože jste viděli dříve, malá a velká písmena v názvu proměnné nezáleží.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-240">In Visual Basic, as you saw earlier, the case of the letters in a variable name doesn't matter.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="2a6b8-241">Proměnné a datové typy</span><span class="sxs-lookup"><span data-stu-id="2a6b8-241">Variables and data types</span></span>

<span data-ttu-id="2a6b8-242">Proměnná může mít určitý datový typ, který označuje, jaký druh dat je uložen v proměnné.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-242">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="2a6b8-243">Můžete použít proměnné řetězce, které ukládají hodnoty řetězce (například &quot;Hello world&quot;), celočíselné proměnné, které ukládají hodnoty celé číslo (např. 3 nebo 79) a proměnné pro datum, které ukládají data v různých formátech (např. 4/12/2012 nebo března 2009 ).</span><span class="sxs-lookup"><span data-stu-id="2a6b8-243">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="2a6b8-244">A existuje mnoho dalších typů dat, která vám pomůže.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-244">And there are many other data types you can use.</span></span>

<span data-ttu-id="2a6b8-245">Však není nutné zadat typ pro proměnnou.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-245">However, you don't have to specify a type for a variable.</span></span> <span data-ttu-id="2a6b8-246">Ve většině případů technologie ASP.NET můžete zjistit typ závislosti na tom, jak se používá data v proměnné.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-246">In most cases ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="2a6b8-247">(Někdy je nutné zadat typ; uvidíte příkladech, kde je hodnota true.)</span><span class="sxs-lookup"><span data-stu-id="2a6b8-247">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="2a6b8-248">Chcete-li deklarovat proměnnou bez určení typu, použijte `Dim` plus název proměnné (například `Dim myVar`).</span><span class="sxs-lookup"><span data-stu-id="2a6b8-248">To declare a variable without specifying a type, use `Dim` plus the variable name (for instance, `Dim myVar`).</span></span> <span data-ttu-id="2a6b8-249">Chcete-li deklarovat proměnnou s typem, použijte `Dim` plus název proměnné, za nímž následuje `As` a pak zadejte název (například `Dim myVar As String`).</span><span class="sxs-lookup"><span data-stu-id="2a6b8-249">To declare a variable with a type, use `Dim` plus the variable name, followed by `As` and then the type name (for instance, `Dim myVar As String`).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

<span data-ttu-id="2a6b8-250">Následující příklad ukazuje některé vložené výrazy, které používají proměnné na webové stránce.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-250">The following example shows some inline expressions that use the variables in a web page.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

<span data-ttu-id="2a6b8-251">Výsledek zobrazí v prohlížeči:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-251">The result displayed in a browser:</span></span>

![Razor Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="2a6b8-253">Převod a testování datových typů</span><span class="sxs-lookup"><span data-stu-id="2a6b8-253">Converting and testing data types</span></span>

<span data-ttu-id="2a6b8-254">Přestože ASP.NET obvykle můžete automaticky určit typ dat, někdy se to neplatí.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-254">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="2a6b8-255">Proto můžete potřebovat pomoc ASP.NET pomocí provádí explicitní převod.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-255">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="2a6b8-256">I když nemáte k dispozici pro převod typů, někdy je vhodné otestovat, jaký typ dat je možné, že pracujete s.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-256">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="2a6b8-257">Nejběžnější případ je, že budete muset převést řetězec na jiný typ, například na celé číslo nebo datum.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-257">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="2a6b8-258">Následující příklad ukazuje typické případy, kdy je nutné převést řetězec na číslo.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-258">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

<span data-ttu-id="2a6b8-259">Jako pravidlo uživatelský vstup je pro vás řetězce.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-259">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="2a6b8-260">I v případě, že jste výzva, aby uživatel zadal číslo, a i v případě, že jste zadali číslici, při odeslání vstup uživatele a čtení v kódu, data jsou ve formátu řetězce.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-260">Even if you've prompted the user to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="2a6b8-261">Proto je nutné převést řetězec na číslo.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-261">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="2a6b8-262">V příkladu při pokusu o provedení aritmetické operace na hodnotách bez převodu, chybová zpráva výsledky, protože ASP.NET nejde přidat dva řetězce:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-262">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

`Cannot implicitly convert type 'string' to 'int'.`

<span data-ttu-id="2a6b8-263">K převodu hodnoty na celá čísla, volání `AsInt` metody.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-263">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="2a6b8-264">Pokud převod není úspěšné, pak přidáte čísla.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-264">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="2a6b8-265">Následující tabulka uvádí některé běžné metody převodu a testování pro proměnné.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-265">The following table lists some common conversion and test methods for variables.</span></span>


:::row:::
    :::column:::
        <span data-ttu-id="2a6b8-266"><strong>– Metoda</strong></span><span class="sxs-lookup"><span data-stu-id="2a6b8-266"><strong>Method</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="2a6b8-267"><strong>Popis</strong></span><span class="sxs-lookup"><span data-stu-id="2a6b8-267"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="2a6b8-268"><strong>Příklad</strong></span><span class="sxs-lookup"><span data-stu-id="2a6b8-268"><strong>Example</strong></span></span>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="2a6b8-269">Převede řetězec představující celé číslo (například &quot;593&quot;) na celé číslo.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-269">Converts a string that represents a whole number (like &quot;593&quot;) to an integer.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="2a6b8-270">Převede řetězec jako &quot;true&quot; nebo &quot;false&quot; s typem Boolean.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-270">Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="2a6b8-271">Převede řetězec obsahující desetinná hodnota jako &quot;1.3&quot; nebo &quot;7.439&quot; na číslo s plovoucí desetinnou čárkou.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-271">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="2a6b8-272">Převede řetězec obsahující desetinná hodnota jako &quot;1.3&quot; nebo &quot;7.439&quot; na desetinné číslo.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-272">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number.</span></span> <span data-ttu-id="2a6b8-273">(V technologii ASP.NET je přesnější než číslo s plovoucí desetinnou čárkou desetinné číslo.)</span><span class="sxs-lookup"><span data-stu-id="2a6b8-273">(In ASP.NET, a decimal number is more precise than a floating-point number.)</span></span> :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="2a6b8-274">Převede řetězec představující hodnotu data a času na ASP.NET `DateTime` typu.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-274">Converts a string that represents a date and time value to the ASP.NET `DateTime` type.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="2a6b8-275">Převede jakýkoli jiný typ dat na řetězec.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-275">Converts any other data type to a string.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    :::column-end:::
:::row-end:::


## <a name="operators"></a><span data-ttu-id="2a6b8-276">Operátory</span><span class="sxs-lookup"><span data-stu-id="2a6b8-276">Operators</span></span>

<span data-ttu-id="2a6b8-277">Operátor je klíčové slovo nebo znak, který říká technologie ASP.NET, jaký druh příkaz k provedení ve výrazu.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-277">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="2a6b8-278">Visual Basic podporuje mnoho operátorů, ale je potřeba jenom rozpoznat pár, abyste mohli začít vyvíjet webové stránky ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-278">Visual Basic supports many operators, but you only need to recognize a few to get started developing ASP.NET web pages.</span></span> <span data-ttu-id="2a6b8-279">Následující tabulka shrnuje nejčastější operátory.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-279">The following table summarizes the most common operators.</span></span>


:::row:::
    :::column:::
        <span data-ttu-id="2a6b8-280"><strong>– Operátor</strong></span><span class="sxs-lookup"><span data-stu-id="2a6b8-280"><strong>Operator</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="2a6b8-281"><strong>Popis</strong></span><span class="sxs-lookup"><span data-stu-id="2a6b8-281"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="2a6b8-282"><strong>Příklady</strong></span><span class="sxs-lookup"><span data-stu-id="2a6b8-282"><strong>Examples</strong></span></span>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+ - * /`
    :::column-end:::
    :::column:::
        <span data-ttu-id="2a6b8-283">Matematické operátory používat ve výrazech pro číselná.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-283">Math operators used in numerical expressions.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
        <span data-ttu-id="2a6b8-284">Přiřazení a rovnosti.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-284">Assignment and equality.</span></span> <span data-ttu-id="2a6b8-285">V závislosti na kontextu buď přiřadí hodnotu na pravé straně příkazu na objekt na levé straně nebo zkontroluje hodnoty na rovnost.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-285">Depending on context, either assigns the value on the right side of a statement to the object on the left side, or checks the values for equality.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `<>`
    :::column-end:::
    :::column:::
        <span data-ttu-id="2a6b8-286">Nerovnost.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-286">Inequality.</span></span> <span data-ttu-id="2a6b8-287">Vrátí `True` Pokud hodnoty nejsou shodné.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-287">Returns `True` if the values are not equal.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
        <span data-ttu-id="2a6b8-288">Menší než, větší než, menší než nebo rovno a větší než nebo rovno.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-288">Less than, greater than, less than or equal, and greater than or equal.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `&`
    :::column-end:::
    :::column:::
        <span data-ttu-id="2a6b8-289">Zřetězení, který se používá pro připojení řetězce.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-289">Concatenation, which is used to join strings.</span></span>
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+= -=`
    :::column-end:::
    :::column:::
        <span data-ttu-id="2a6b8-290">Přírůstek a snížení operátory, které operátorů sčítání a odečítání 1 (v uvedeném pořadí) z proměnné.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-290">The increment and decrement operators, which add and subtract 1 (respectively) from a variable.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
        <span data-ttu-id="2a6b8-291">Tečka.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-291">Dot.</span></span> <span data-ttu-id="2a6b8-292">Použít k rozlišení objekty a jejich vlastnosti a metody.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-292">Used to distinguish objects and their properties and methods.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="2a6b8-293">Závorky.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-293">Parentheses.</span></span> <span data-ttu-id="2a6b8-294">Skupinové výrazy, lze pro předání parametrů k metodám a chcete získat přístup ke členům pole a kolekce.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-294">Used to group expressions, to pass parameters to methods, and to access members of arrays and collections.</span></span>
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `Not`
    :::column-end:::
    :::column:::
        <span data-ttu-id="2a6b8-295">Není.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-295">Not.</span></span> <span data-ttu-id="2a6b8-296">Vrátí hodnotu true na false a naopak.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-296">Reverses a true value to false and vice versa.</span></span> <span data-ttu-id="2a6b8-297">Obvykle se používá jako zjednodušený způsob, jak otestovat pro `False` (to znamená pro není `True`).</span><span class="sxs-lookup"><span data-stu-id="2a6b8-297">Typically used as a shorthand way to test for `False` (that is, for not `True`).</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AndAlso OrElse`
    :::column-end:::
    :::column:::
        <span data-ttu-id="2a6b8-298">Logický operátor AND a které se používají k propojení podmínky společně.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-298">Logical AND and OR, which are used to link conditions together.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    :::column-end:::
:::row-end:::

## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="2a6b8-299">Práce s souboru a cesty ke složkám v kódu</span><span class="sxs-lookup"><span data-stu-id="2a6b8-299">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="2a6b8-300">Často budete pracovat s cestami k souborům a složkám v kódu.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-300">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="2a6b8-301">Tady je příklad struktury fyzické složce pro web, jak se může zobrazovat ve svém vývojovém počítači:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-301">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="2a6b8-302">Tady jsou některé důležité podrobnosti o adresy URL a cesty:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-302">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="2a6b8-303">Adresa URL začíná na buď název domény (`http://www.example.com`) nebo název serveru (`http://localhost`, `http://mycomputer`).</span><span class="sxs-lookup"><span data-stu-id="2a6b8-303">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="2a6b8-304">Adresa URL odpovídá fyzickou cestu na hostitelském počítači.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-304">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="2a6b8-305">Například `http://myserver` může odpovídat složce *C:\websites\mywebsite* na serveru.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-305">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="2a6b8-306">Virtuální cesta je zkratka pro reprezentaci cest v kódu bez nutnosti zadávat úplnou cestu.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-306">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="2a6b8-307">Obsahuje části adresy URL, která následuje název domény nebo serveru.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-307">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="2a6b8-308">Při použití virtuální cesty, můžete přesunout kód do jiné domény nebo serveru bez nutnosti aktualizovat cesty.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-308">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="2a6b8-309">Tady je příklad, který vám pomůže pochopit rozdíly:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-309">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="2a6b8-310">Úplnou adresu URL</span><span class="sxs-lookup"><span data-stu-id="2a6b8-310">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="2a6b8-311">Název serveru</span><span class="sxs-lookup"><span data-stu-id="2a6b8-311">Server name</span></span> | <span data-ttu-id="2a6b8-312">*mycompanyserver*</span><span class="sxs-lookup"><span data-stu-id="2a6b8-312">*mycompanyserver*</span></span> |
| <span data-ttu-id="2a6b8-313">Virtuální cesta</span><span class="sxs-lookup"><span data-stu-id="2a6b8-313">Virtual path</span></span> | <span data-ttu-id="2a6b8-314">*/humanresources/CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="2a6b8-314">*/humanresources/CompanyPolicy.htm*</span></span> |
| <span data-ttu-id="2a6b8-315">Fyzická cesta</span><span class="sxs-lookup"><span data-stu-id="2a6b8-315">Physical path</span></span> | <span data-ttu-id="2a6b8-316">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="2a6b8-316">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span></span> |

<span data-ttu-id="2a6b8-317">Virtuální kořenový adresář je /, stejně jako kořenový C: jednotka je \.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-317">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="2a6b8-318">(Virtuální složka cesty vždy používat lomítka.) Virtuální cesta ke složce nemusí mít stejný název jako fyzické složce. může se jednat jako alias.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-318">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="2a6b8-319">(Na provozních serverech virtuální cesta zřídka odpovídá přesně fyzickou cestu.)</span><span class="sxs-lookup"><span data-stu-id="2a6b8-319">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="2a6b8-320">Při práci se soubory a složkami v kódu, někdy potřebujete odkazovat na fyzickou cestu a někdy virtuální cesty, v závislosti na tom, jaké objekty pracujete s.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-320">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="2a6b8-321">Technologie ASP.NET poskytuje tyto nástroje pro práci s cestami k souborům a složkám v kódu: `Server.MapPath` metody a `~` operátor a `Href` metoda.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-321">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="2a6b8-322">Převod virtuálních a fyzických cest: Server.MapPath – metoda</span><span class="sxs-lookup"><span data-stu-id="2a6b8-322">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="2a6b8-323">`Server.MapPath` Metoda převede virtuální cestu (například */default.cshtml*) na absolutní cestu, fyzické (jako *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="2a6b8-323">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="2a6b8-324">Tuto metodu použijte kdykoli potřebujete úplná fyzická cesta.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-324">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="2a6b8-325">Typickým příkladem je při čtení nebo zápis textový soubor nebo soubor obrázku na webovém serveru.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-325">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="2a6b8-326">Obvykle neznáte absolutní fyzická cesta k webu na serveru pro hostování lokality, abyste této metody můžete převést cestu si jisti, virtuální cestu – do příslušné cesty na serveru za vás.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-326">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="2a6b8-327">Předat virtuální cestu k souboru nebo složky na metodu a vrátí fyzickou cestu:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-327">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="2a6b8-328">Odkazování na virtuální kořenový adresář: ~ – operátor a metoda Href</span><span class="sxs-lookup"><span data-stu-id="2a6b8-328">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="2a6b8-329">V *.cshtml* nebo *.vbhtml* soubor, můžete odkazovat pomocí cesty kořenové složky `~` operátor.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-329">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="2a6b8-330">To je velmi užitečný, protože se můžete pohybovat stránky na webu a jakékoli odkazy, které obsahují další stránky nesmí být nefunkční.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-330">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="2a6b8-331">Je taky užitečný v případě, kdy přesunout svůj web do jiného umístění.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-331">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="2a6b8-332">Následuje několik příkladů:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-332">Here are some examples:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

<span data-ttu-id="2a6b8-333">Pokud webová stránka se `http://myserver/myapp`, zde je, jak ASP.NET bude zpracovávat tyto cesty při spuštění stránky:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-333">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- <span data-ttu-id="2a6b8-334">`myImagesFolder`: `http://myserver/myapp/images`</span><span class="sxs-lookup"><span data-stu-id="2a6b8-334">`myImagesFolder`: `http://myserver/myapp/images`</span></span>
- <span data-ttu-id="2a6b8-335">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span><span class="sxs-lookup"><span data-stu-id="2a6b8-335">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span></span>

<span data-ttu-id="2a6b8-336">(Ve skutečnosti neuvidíte těchto cest jako hodnotu proměnné, ale ASP.NET bude zacházet s cesty, jak, pokud je to, co byly.)</span><span class="sxs-lookup"><span data-stu-id="2a6b8-336">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="2a6b8-337">Můžete použít `~` operátor v serverovém kódu (jak je uvedeno výše) a v kódu, například takto:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-337">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

<span data-ttu-id="2a6b8-338">V kódu, můžete použít `~` operátor k vytvoření cesty k prostředkům, jako jsou soubory obrázků, jiné webové stránky a souborů CSS.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-338">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="2a6b8-339">Při spuštění stránky ASP.NET vypadat prostřednictvím stránky (kódu a kódu) a řeší všechny `~` odkazy na příslušné cesty.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-339">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="2a6b8-340">Podmíněnou logiku a smyčky</span><span class="sxs-lookup"><span data-stu-id="2a6b8-340">Conditional Logic and Loops</span></span>

<span data-ttu-id="2a6b8-341">Kód serveru ASP.NET umožňuje provádět úkoly na základě podmínky a zápisu kód, který bude opakovat příkazy s konkrétním počtem opakování tedy kód, který běží smyčku).</span><span class="sxs-lookup"><span data-stu-id="2a6b8-341">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="2a6b8-342">Testování podmínek</span><span class="sxs-lookup"><span data-stu-id="2a6b8-342">Testing conditions</span></span>

<span data-ttu-id="2a6b8-343">K otestování jednoduché podmínky použití `If...Then` příkaz, který vrací `True` nebo `False` na základě testu zadáte:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-343">To test a simple condition you use the `If...Then` statement, which returns `True` or `False` based on a test you specify:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

<span data-ttu-id="2a6b8-344">`If` – Klíčové slovo začíná bloku.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-344">The `If` keyword starts a block.</span></span> <span data-ttu-id="2a6b8-345">Skutečný test (podmínky) následuje `If` – klíčové slovo a vrátí hodnotu true nebo false.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-345">The actual test (condition) follows the `If` keyword and returns true or false.</span></span> <span data-ttu-id="2a6b8-346">`If` Příkaz končí `Then`.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-346">The `If` statement ends with `Then`.</span></span> <span data-ttu-id="2a6b8-347">Příkazy, které se spustí při splnění testu jsou uzavřeny ve `If` a `End If`.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-347">The statements that will run if the test is true are enclosed by `If` and `End If`.</span></span> <span data-ttu-id="2a6b8-348">`If` Výraz může obsahovat `Else` blok, který určuje příkazy ke spuštění, pokud podmínka není splněna:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-348">An `If` statement can include an `Else` block that specifies statements to run if the condition is false:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

<span data-ttu-id="2a6b8-349">Pokud `If` příkaz spustí bloku kódu, není nutné použít normální `Code...End Code` příkazy k zahrnutí bloků.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-349">If an `If` statement starts a code block, you don't have to use the normal `Code...End Code` statements to include the blocks.</span></span> <span data-ttu-id="2a6b8-350">Lze přidat `@` na blok, a bude fungovat.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-350">You can just add `@` to the block, and it will work.</span></span> <span data-ttu-id="2a6b8-351">Tento přístup funguje s `If` stejně jako ostatní jazyka Visual Basic programování klíčová slova, která následuje bloky kódu, včetně `For`, `For Each`, `Do While`atd.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-351">This approach works with `If` as well as other Visual Basic programming keywords that are followed by code blocks, including `For`, `For Each`, `Do While`, etc.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

<span data-ttu-id="2a6b8-352">Můžete přidat více podmínek použití jednoho nebo více `ElseIf` bloků:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-352">You can add multiple conditions using one or more `ElseIf` blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

<span data-ttu-id="2a6b8-353">V tomto příkladu, pokud první podmínka vyhodnocena jako v `If` blok není true, `ElseIf` podmínka je zaškrtnuté políčko.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-353">In this example, if the first condition in the `If` block is not true, the `ElseIf` condition is checked.</span></span> <span data-ttu-id="2a6b8-354">Pokud tato podmínka je splněna, příkazy v `ElseIf` jsou spuštěny.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-354">If that condition is met, the statements in the `ElseIf` block are executed.</span></span> <span data-ttu-id="2a6b8-355">Pokud jsou splněna žádná z podmínek, příkazy v `Else` jsou spuštěny.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-355">If none of the conditions are met, the statements in the `Else` block are executed.</span></span> <span data-ttu-id="2a6b8-356">Můžete přidat libovolný počet `ElseIf` blokuje a pak zavřete s `Else` blokovat, jako &quot;všechno ostatní&quot; podmínku.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-356">You can add any number of `ElseIf` blocks, and then close with an `Else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="2a6b8-357">Chcete-li otestovat velký počet podmínek, použijte `Select Case` blok:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-357">To test a large number of conditions, use a `Select Case` block:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

<span data-ttu-id="2a6b8-358">K otestování hodnotu v závorkách (v příkladu proměnná den v týdnu).</span><span class="sxs-lookup"><span data-stu-id="2a6b8-358">The value to test is in parentheses (in the example, the weekday variable).</span></span> <span data-ttu-id="2a6b8-359">Používá každý samostatný test `Case` příkaz, který obsahuje hodnotu.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-359">Each individual test uses a `Case` statement that lists a value.</span></span> <span data-ttu-id="2a6b8-360">Pokud hodnota `Case` příkazu se shoduje s hodnotou testu, kód, který `Case` je blok proveden.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-360">If the value of a `Case` statement matches the test value, the code in that `Case` block is executed.</span></span>

<span data-ttu-id="2a6b8-361">Výsledek poslední dva podmíněné bloky zobrazí v prohlížeči:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-361">The result of the last two conditional blocks displayed in a browser:</span></span>

![Razor Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="2a6b8-363">Opakování ve smyčce kódu</span><span class="sxs-lookup"><span data-stu-id="2a6b8-363">Looping code</span></span>

<span data-ttu-id="2a6b8-364">Často je potřeba spustit stejné příkazy opakovaně.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-364">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="2a6b8-365">To provedete tak, že opakování ve smyčce.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-365">You do this by looping.</span></span> <span data-ttu-id="2a6b8-366">Například můžete často spustit stejné příkazy pro každou položku v kolekci dat.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-366">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="2a6b8-367">Pokud víte přesně kolikrát chcete opakovat, můžete použít `For` smyčky.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-367">If you know exactly how many times you want to loop, you can use a `For` loop.</span></span> <span data-ttu-id="2a6b8-368">Tento druh smyčky je užitečné zejména pro počítání nebo počítání:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-368">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

<span data-ttu-id="2a6b8-369">Smyčky začíná `For` – klíčové slovo, za nímž následuje tři prvky:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-369">The loop begins with the `For` keyword, followed by three elements:</span></span>

- <span data-ttu-id="2a6b8-370">Ihned po `For` prohlášení, deklarujete proměnnou čítače (není nutné používat `Dim`) a pak poznáte, rozsah, jako v `i = 10 to 20`.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-370">Immediately after the `For` statement, you declare a counter variable (you don't have to use `Dim`) and then indicate the range, as in `i = 10 to 20`.</span></span> <span data-ttu-id="2a6b8-371">To znamená, že proměnná `i` spustí, počítací na 10 a pokračovat, dokud nedosáhne 20 (včetně).</span><span class="sxs-lookup"><span data-stu-id="2a6b8-371">This means the variable `i` will start counting at 10 and continue until it reaches 20 (inclusive).</span></span>
- <span data-ttu-id="2a6b8-372">Mezi `For` a `Next` příkazy je obsah bloku.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-372">Between the `For` and `Next` statements is the content of the block.</span></span> <span data-ttu-id="2a6b8-373">Tento název může obsahovat jeden nebo více příkazy kódu, které fungují s každou smyčku.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-373">This can contain one or more code statements that execute with each loop.</span></span>
- <span data-ttu-id="2a6b8-374">`Next i` Příkazu ukončení smyčky.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-374">The `Next i` statement ends the loop.</span></span> <span data-ttu-id="2a6b8-375">Zvýší čítač a spustí další iteraci smyčky.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-375">It increments the counter and starts the next iteration of the loop.</span></span>

<span data-ttu-id="2a6b8-376">Řádek kódu mezi `For` a `Next` řádky obsahuje kód, který běží u jednotlivých iterací smyčky.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-376">The line of code between the `For` and `Next` lines contains the code that runs for each iteration of the loop.</span></span> <span data-ttu-id="2a6b8-377">Kód vytvoří nový odstavec (`<p>` element) každý čas a přidá nový řádek do výstupu, zobrazení hodnoty i (Čítač).</span><span class="sxs-lookup"><span data-stu-id="2a6b8-377">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of i (the counter).</span></span> <span data-ttu-id="2a6b8-378">Když spustíte tuto stránku, tento příklad vytvoří 11 řádky zobrazení výstupu, s textem na každém řádku označující počet položek.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-378">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor Img11](introducing-razor-syntax-vb/_static/image11.jpg)

<span data-ttu-id="2a6b8-380">Pokud pracujete s kolekce nebo pole, často používají `For Each` smyčky.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-380">If you're working with a collection or array, you often use a `For Each` loop.</span></span> <span data-ttu-id="2a6b8-381">Kolekce je skupina podobných objektů a `For Each` smyčky umožňuje provést úlohu pro každou položku v kolekci.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-381">A collection is a group of similar objects, and the `For Each` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="2a6b8-382">Tento druh smyčky je vhodné pro kolekce, protože na rozdíl od `For` smyčky, není nutné zvýší čítač nebo nastavit omezení.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-382">This type of loop is convenient for collections, because unlike a `For` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="2a6b8-383">Místo toho `For Each` kód smyčky jednoduše pokračuje přes kolekci, dokud se nedokončí.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-383">Instead, the `For Each` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="2a6b8-384">Vrátí položky v tomto příkladu `Request.ServerVariables` kolekce (který obsahuje informace o webovém serveru).</span><span class="sxs-lookup"><span data-stu-id="2a6b8-384">This example returns the items in the `Request.ServerVariables` collection (which contains information about your web server).</span></span> <span data-ttu-id="2a6b8-385">Použije `For Each` smyčky zobrazovaný název každé položky tak, že vytvoříte nový `<li>` prvek v seznamu s odrážkami HTML.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-385">It uses a `For Each` loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

<span data-ttu-id="2a6b8-386">`For Each` Klíčovým slovem následovalo proměnná, která představuje jednu položku v kolekci (v tomto příkladu `myItem`) a po něm `In` – klíčové slovo, za nímž následuje kolekci chcete projít.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-386">The `For Each` keyword is followed by a variable that represents a single item in the collection (in the example, `myItem`), followed by the `In` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="2a6b8-387">V těle `For Each` smyčky, dostanete aktuální položky pomocí proměnné, která je deklarována jako dříve.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-387">In the body of the `For Each` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Razor Img12](introducing-razor-syntax-vb/_static/image12.jpg)

<span data-ttu-id="2a6b8-389">Chcete-li vytvořit smyčku více pro obecné účely, použijte `Do While` – příkaz:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-389">To create a more general-purpose loop, use the `Do While` statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

<span data-ttu-id="2a6b8-390">Tato smyčka začíná `Do While` – klíčové slovo, za nímž následuje podmínky, za nímž následuje blok opakovat.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-390">This loop begins with the `Do While` keyword, followed by a condition, followed by the block to repeat.</span></span> <span data-ttu-id="2a6b8-391">Obvykle zvýšit smyčky (přidání do) nebo dekrementace (odečíst od) proměnné nebo objektu se používá pro počítání.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-391">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="2a6b8-392">V tomto příkladu `+=` operátor přičte 1 k hodnotě proměnné při každém spuštění smyčky.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-392">In the example, the `+=` operator adds 1 to the value of a variable each time the loop runs.</span></span> <span data-ttu-id="2a6b8-393">(Chcete-li snížení proměnná ve smyčce počítá směrem dolů, použijte operátor dekrementace `-=`.)</span><span class="sxs-lookup"><span data-stu-id="2a6b8-393">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`.)</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="2a6b8-394">Objekty a kolekce</span><span class="sxs-lookup"><span data-stu-id="2a6b8-394">Objects and Collections</span></span>

<span data-ttu-id="2a6b8-395">Téměř vše na webu technologie ASP.NET je objekt, včetně webové stránky.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-395">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="2a6b8-396">Tato část popisuje některé důležité objekty, které budete pracovat se často ve vašem kódu.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-396">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="2a6b8-397">Objekty stránky</span><span class="sxs-lookup"><span data-stu-id="2a6b8-397">Page objects</span></span>

<span data-ttu-id="2a6b8-398">Na stránce je nejzákladnější objekt v technologii ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-398">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="2a6b8-399">Můžete přistupovat k vlastnosti objektu page přímo bez žádné opravňující objektu.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-399">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="2a6b8-400">Následující kód načte cestu k souboru na stránce, pomocí `Request` objektu stránky:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-400">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

<span data-ttu-id="2a6b8-401">Můžete použít vlastnosti `Page` můžete získat velké množství informací, jako například:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-401">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- <span data-ttu-id="2a6b8-402">`Request`.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-402">`Request`.</span></span> <span data-ttu-id="2a6b8-403">Jak už víte, to je kolekce informací o aktuálním požadavku, včetně typu prohlížeče přišel požadavek, adresa URL stránky, identita uživatele, atd.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-403">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- <span data-ttu-id="2a6b8-404">`Response`.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-404">`Response`.</span></span> <span data-ttu-id="2a6b8-405">Toto je kolekce informací o odpovědi (stránky), které se odešlou do prohlížeče při dokončení kódu serveru.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-405">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="2a6b8-406">Tuto vlastnost můžete například použít při zápisu informací do odpovědi.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-406">For example, you can use this property to write information into the response.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="2a6b8-407">Kolekce objektů (polí a slovníky)</span><span class="sxs-lookup"><span data-stu-id="2a6b8-407">Collection objects (arrays and dictionaries)</span></span>

<span data-ttu-id="2a6b8-408">Kolekce je skupina objektů stejného typu, jako je například kolekce `Customer` objekty z databáze.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-408">A collection is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="2a6b8-409">Technologie ASP.NET obsahuje mnoho předdefinovaných kolekcí, jako `Request.Files` kolekce.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-409">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="2a6b8-410">Často budete pracovat s daty v kolekcích.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-410">You'll often work with data in collections.</span></span> <span data-ttu-id="2a6b8-411">Jsou dvě běžné typy kolekcí *pole* a *slovníku*.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-411">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="2a6b8-412">Pole je užitečné, když chcete uložit kolekci podobných položek, ale nebudete chtít vytvořit samostatné proměnnou pro uchování položky:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-412">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

<span data-ttu-id="2a6b8-413">S poli, je třeba deklarovat určitý datový typ. `String`, `Integer`, nebo `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-413">With arrays, you declare a specific data type, such as `String`, `Integer`, or `DateTime`.</span></span> <span data-ttu-id="2a6b8-414">K označení, že proměnné můžou obsahovat pole, přidat závorky k názvu v deklaraci proměnné (jako například `Dim myVar() As String`).</span><span class="sxs-lookup"><span data-stu-id="2a6b8-414">To indicate that the variable can contain an array, you add parentheses to the variable name in the declaration (such as `Dim myVar() As String`).</span></span> <span data-ttu-id="2a6b8-415">Můžete přístup k položkám v poli pomocí jejich pozice (index) nebo pomocí `For Each` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-415">You can access items in an array using their position (index) or by using the `For Each` statement.</span></span> <span data-ttu-id="2a6b8-416">Indexy pole jsou počítány od nuly &#8212; tedy první položka je na pozici 0, druhá položka je na pozici 1 a tak dále.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-416">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

<span data-ttu-id="2a6b8-417">Můžete určit počet položek v poli získáním jeho `Length` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-417">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="2a6b8-418">Chcete-li získat pozice konkrétní položku v poli (to znamená, k vyhledání pole), použijte `Array.IndexOf` – metoda.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-418">To get the position of a specific item in the array (that is, to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="2a6b8-419">Můžete také provést třeba zpětného obsah pole ( `Array.Reverse` metoda) nebo řazení obsahu ( `Array.Sort` metoda).</span><span class="sxs-lookup"><span data-stu-id="2a6b8-419">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="2a6b8-420">Výstupní řetězec pole Kód zobrazený v prohlížeči:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-420">The output of the string array code displayed in a browser:</span></span>

![Razor Img13](introducing-razor-syntax-vb/_static/image13.jpg)

<span data-ttu-id="2a6b8-422">Slovník je kolekce párů klíč/hodnota, ve kterém zadat klíč (nebo název) k nastavení nebo načtení odpovídající hodnotě:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-422">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

<span data-ttu-id="2a6b8-423">Chcete-li vytvořit slovník, použijte `New` – klíčové slovo k označení, že vytváříte novou `Dictionary` objektu.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-423">To create a dictionary, you use the `New` keyword to indicate that you're creating a new `Dictionary` object.</span></span> <span data-ttu-id="2a6b8-424">Slovník můžete přiřadit k proměnné pomocí `Dim` – klíčové slovo.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-424">You can assign a dictionary to a variable using the `Dim` keyword.</span></span> <span data-ttu-id="2a6b8-425">Označení datové typy položek ve slovníku pomocí závorek ( `( )` ).</span><span class="sxs-lookup"><span data-stu-id="2a6b8-425">You indicate the data types of the items in the dictionary using parentheses ( `( )` ).</span></span> <span data-ttu-id="2a6b8-426">Na konci deklarace je nutné přidat jinou dvojici závorek, protože je to ve skutečnosti metodou, která vytvoří nový slovník.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-426">At the end of the declaration, you must add another pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="2a6b8-427">Přidání položek do slovníku, můžete volat `Add` metoda proměnnou slovníku (`myScores` v tomto případě) a pak zadejte klíč a hodnotu.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-427">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="2a6b8-428">Alternativně můžete použít závorky k označení klíče a provádět jednoduché přiřazení, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-428">Alternatively, you can use parentheses to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

<span data-ttu-id="2a6b8-429">Pokud chcete získat hodnotu ze slovníku, můžete zadat klíč v závorkách:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-429">To get a value from the dictionary, you specify the key in parentheses:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="2a6b8-430">Volání metody s parametry</span><span class="sxs-lookup"><span data-stu-id="2a6b8-430">Calling Methods with Parameters</span></span>

<span data-ttu-id="2a6b8-431">Jak už jste viděli dříve v tomto článku, mají objekty, které můžete naprogramovat pomocí metody.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-431">As you saw earlier in this article, the objects that you program with have methods.</span></span> <span data-ttu-id="2a6b8-432">Například `Database` objekt může mít `Database.Connect` metody.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-432">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="2a6b8-433">Mnoho metody mají také jeden nebo více parametrů.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-433">Many methods also have one or more parameters.</span></span> <span data-ttu-id="2a6b8-434">A *parametr* je hodnota, která můžete předat metodě pro povolení metody a dokončení úkolu.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-434">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="2a6b8-435">Podívejte se například na deklaraci `Request.MapPath` metodu, která přijímá tři parametry:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-435">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

<span data-ttu-id="2a6b8-436">Tato metoda vrací fyzickou cestu na serveru, který odpovídá pro zadanou virtuální cestu.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-436">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="2a6b8-437">Jsou tři parametry pro metodu `virtualPath`, `baseVirtualDir`, a `allowCrossAppMapping`.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-437">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="2a6b8-438">(Všimněte si, že v deklaraci, parametry jsou uvedeny s datovými typy dat, který bude přijímat.) Při volání této metody, je třeba zadat hodnoty pro všemi třemi parametry.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-438">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="2a6b8-439">Pokud používáte Visual Basic se syntaxí Razor, máte dvě možnosti pro předání parametrů metodě: *poziční parametry* nebo *pojmenované parametry*.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-439">When you're using Visual Basic with the Razor syntax, you have two options for passing parameters to a method: *positional parameters* or *named parameters*.</span></span> <span data-ttu-id="2a6b8-440">Volání metody pomocí poziční parametry, předání parametrů v případě přísného pořadí zadaném v deklaraci metody.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-440">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="2a6b8-441">(By obvykle znáte toto pořadí najdete dokumentaci k metodě.) Je nutné postupovat podle pořadí, a přeskočíte nelze některé z parametrů &#8212; Pokud potřeby předáte prázdný řetězec (`""`) nebo pro pozičních parametrů, které nemají hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-441">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or null for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="2a6b8-442">Následující příklad předpokládá, že máte složku s názvem *skripty* na vašem webu.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-442">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="2a6b8-443">Kód volá `Request.MapPath` metoda a předá hodnoty pro tři parametry ve správném pořadí.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-443">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="2a6b8-444">Pak zobrazí výslednou cestu namapované.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-444">It then displays the resulting mapped path.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

<span data-ttu-id="2a6b8-445">Pokud existuje velký počet parametrů pro metodu, můžete svůj kód uchováváte přehlednější a čitelnější pomocí pojmenovaných parametrů.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-445">When there are many parameters for a method, you can keep your code cleaner and more readable by using named parameters.</span></span> <span data-ttu-id="2a6b8-446">Volání metody pomocí pojmenované parametry, zadejte název parametru, za nímž následuje `:=` a pak zadejte hodnotu.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-446">To call a method using named parameters, specify the parameter name followed by `:=` and then provide the value.</span></span> <span data-ttu-id="2a6b8-447">Výhoda pojmenovaných parametrů je, že můžete je přidat do požadovaného pořadí.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-447">An advantage of named parameters is that you can add them in any order you want.</span></span> <span data-ttu-id="2a6b8-448">(Nevýhodou je, že volání metody není co nejkompaktnější.)</span><span class="sxs-lookup"><span data-stu-id="2a6b8-448">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="2a6b8-449">Následující příklad volá stejnou metodu, jak je uvedeno výše, ale používá pojmenované parametry k zadání hodnot:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-449">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

<span data-ttu-id="2a6b8-450">Jak je vidět, parametry jsou předány v jiném pořadí.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-450">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="2a6b8-451">Pokud spustíte z předchozího příkladu a v tomto příkladu, ale budete vrátí stejnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-451">However, if you run the previous example and this example, they'll return the same value.</span></span>

## <a name="handling-errors"></a><span data-ttu-id="2a6b8-452">Zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="2a6b8-452">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="2a6b8-453">Try-Catch – příkazy</span><span class="sxs-lookup"><span data-stu-id="2a6b8-453">Try-Catch statements</span></span>

<span data-ttu-id="2a6b8-454">Často musíte příkazů v kódu, který může selhat z důvodu mimo vaši kontrolu.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-454">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="2a6b8-455">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2a6b8-455">For example:</span></span>

- <span data-ttu-id="2a6b8-456">Pokud váš kód se pokusí otevřít, vytvoření, čtení nebo zápis do souboru, může dojít ke všem typům chyb.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-456">If your code tries to open, create, read, or write a file, all sorts of errors might occur.</span></span> <span data-ttu-id="2a6b8-457">Požadovaný soubor nemusí existovat, může být zablokován, kód nemusí mít oprávnění a tak dále.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-457">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="2a6b8-458">Podobně pokud váš kód se pokouší aktualizovat záznamy v databázi, může být problémy s oprávněními, může dojít ke ztrátě připojení k databázi, data a šetřit tak může být neplatný a tak dále.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-458">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="2a6b8-459">V programovacím prostředí, se nazývají těmito situacemi *výjimky*.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-459">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="2a6b8-460">Pokud váš kód narazí na výjimku, generuje (vyvolá výjimku) chybové zprávy, který je, v nejlepším případě obtěžující uživatelům.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-460">If your code encounters an exception, it generates (throws) an error message that is, at best, annoying to users.</span></span>

![Razor Img14](introducing-razor-syntax-vb/_static/image14.jpg)

<span data-ttu-id="2a6b8-462">V situacích, kdy váš kód může nastat výjimky a pokud se chcete vyhnout chybové zprávy tohoto typu, můžete použít `Try/Catch` příkazy.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-462">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `Try/Catch` statements.</span></span> <span data-ttu-id="2a6b8-463">V `Try` příkazu spustit kód, který při kontrole.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-463">In the `Try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="2a6b8-464">V jedné nebo více `Catch` příkazy, můžete vyhledat konkrétní chyby (konkrétní typy výjimek), které mohly nastat.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-464">In one or more `Catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="2a6b8-465">Můžete vytvořit tolik `Catch` příkazy jako vy, musíme se podívat chyb, které jste očekávání.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-465">You can include as many `Catch` statements as you need to look for errors that you're anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="2a6b8-466">Doporučujeme, abyste je velmi riskantní používat `Response.Redirect` metoda `Try/Catch` příkazy, protože to může způsobit výjimku na stránce.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-466">We recommend that you avoid using the `Response.Redirect` method in `Try/Catch` statements, because it can cause an exception in your page.</span></span>


<span data-ttu-id="2a6b8-467">Následující příklad ukazuje stránka, která vytváří textový soubor na první žádost o a poté zobrazí tlačítko, které umožňuje uživateli otevřít soubor.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-467">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="2a6b8-468">V příkladu záměrně používá chybný název souboru tak, aby způsobí výjimku.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-468">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="2a6b8-469">Tento kód obsahuje `Catch` příkazy pro dvě výjimky: `FileNotFoundException`, která nastane, pokud název souboru je chybný, a `DirectoryNotFoundException`, která nastane, pokud ASP.NET i nelze najít složku.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-469">The code includes `Catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="2a6b8-470">(Příkaz v tomto příkladu můžete odkomentovat Chcete-li zobrazit, jak se spustí při všechno funguje správně.)</span><span class="sxs-lookup"><span data-stu-id="2a6b8-470">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="2a6b8-471">Pokud váš kód nebyl zpracovat výjimky, zobrazí se chybová stránka jako na předchozím snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-471">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="2a6b8-472">Ale `Try/Catch` část pomůže zabránit uživateli v zobrazení tyto typy chyb.</span><span class="sxs-lookup"><span data-stu-id="2a6b8-472">However, the `Try/Catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a><span data-ttu-id="2a6b8-473">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="2a6b8-473">Additional Resources</span></span>

### <a name="reference-documentation"></a><span data-ttu-id="2a6b8-474">Referenční dokumentace</span><span class="sxs-lookup"><span data-stu-id="2a6b8-474">Reference Documentation</span></span>

- [<span data-ttu-id="2a6b8-475">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2a6b8-475">ASP.NET</span></span>](https://msdn.microsoft.com/library/ee532866.aspx)
- [<span data-ttu-id="2a6b8-476">Jazyk Visual Basic</span><span class="sxs-lookup"><span data-stu-id="2a6b8-476">Visual Basic Language</span></span>](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
