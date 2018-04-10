---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
title: ASP.NET MVC zobrazení přehled (VB) | Microsoft Docs
author: StephenWalther
description: Co je zobrazení ASP.NET MVC a jak ho se liší od stránku HTML? V tomto kurzu Stephen Walther vás seznámí s zobrazení a ukazuje, jak můžete t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: c28ba88d-3a93-47f5-a306-049bd766714d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: a64c70851d13b923964dfd1cf3bad55612ae0d0f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-mvc-views-overview-vb"></a><span data-ttu-id="51513-104">Zobrazení ASP.NET MVC (VB) – přehled</span><span class="sxs-lookup"><span data-stu-id="51513-104">ASP.NET MVC Views Overview (VB)</span></span>
====================
<span data-ttu-id="51513-105">podle [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="51513-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="51513-106">Co je zobrazení ASP.NET MVC a jak ho se liší od stránku HTML?</span><span class="sxs-lookup"><span data-stu-id="51513-106">What is an ASP.NET MVC View and how does it differ from a HTML page?</span></span> <span data-ttu-id="51513-107">V tomto kurzu Stephen Walther vás seznámí s zobrazení a ukazuje, jak můžete využít zobrazení dat a pomocné objekty HTML v rámci zobrazení.</span><span class="sxs-lookup"><span data-stu-id="51513-107">In this tutorial, Stephen Walther introduces you to Views and demonstrates how you can take advantage of View Data and HTML Helpers within a View.</span></span>


<span data-ttu-id="51513-108">Účelem tohoto kurzu je poskytnout stručný úvod do architektury ASP.NET MVC zobrazení, data zobrazení a pomocné rutiny HTML.</span><span class="sxs-lookup"><span data-stu-id="51513-108">The purpose of this tutorial is to provide you with a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="51513-109">Na konci tohoto kurzu musíte vědět, jak vytvořit nové zobrazení, předat data z řadiče zobrazení a použití pomocné objekty HTML ke generování obsahu v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="51513-109">By the end of this tutorial, you should understand how to create new views, pass data from a controller to a view, and use HTML Helpers to generate content in a view.</span></span>

## <a name="understanding-views"></a><span data-ttu-id="51513-110">Principy zobrazení</span><span class="sxs-lookup"><span data-stu-id="51513-110">Understanding Views</span></span>

<span data-ttu-id="51513-111">Na rozdíl od Active Server Pages nebo ASP.NET rozhraní ASP.NET MVC nezahrnuje všechny položky, které odpovídá přímo na stránku.</span><span class="sxs-lookup"><span data-stu-id="51513-111">Unlike ASP.NET or Active Server Pages, ASP.NET MVC does not include anything that directly corresponds to a page.</span></span> <span data-ttu-id="51513-112">V aplikaci ASP.NET MVC není na stránce na disk, který odpovídá cestě v adrese URL, kterou zadáte do panelu Adresa prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="51513-112">In an ASP.NET MVC application, there is not a page on disk that corresponds to the path in the URL that you type into the address bar of your browser.</span></span> <span data-ttu-id="51513-113">Je nejblíže si na stránce v aplikaci ASP.NET MVC je něco názvem *zobrazení*.</span><span class="sxs-lookup"><span data-stu-id="51513-113">The closest thing to a page in an ASP.NET MVC application is something called a *view*.</span></span>

<span data-ttu-id="51513-114">V aplikaci ASP.NET MVC jsou příchozí požadavky na prohlížeč namapované na akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="51513-114">In an ASP.NET MVC application, incoming browser requests are mapped to controller actions.</span></span> <span data-ttu-id="51513-115">Akce kontroleru může vrátit zobrazení.</span><span class="sxs-lookup"><span data-stu-id="51513-115">A controller action might return a view.</span></span> <span data-ttu-id="51513-116">Akce kontroleru se ale může provést jiný typ akce, jako je přesměrování na jiný akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="51513-116">However, a controller action might perform some other type of action such as redirecting you to another controller action.</span></span>

<span data-ttu-id="51513-117">Výpis 1 obsahuje řadič jednoduché s názvem HomeController.</span><span class="sxs-lookup"><span data-stu-id="51513-117">Listing 1 contains a simple controller named the HomeController.</span></span> <span data-ttu-id="51513-118">HomeController zpřístupní s názvem Index() a Details() dvě akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="51513-118">The HomeController exposes two controller actions named Index() and Details().</span></span>

<span data-ttu-id="51513-119">**Výpis 1 - HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="51513-119">**Listing 1 - HomeController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample1.vb)]

<span data-ttu-id="51513-120">Je první akcí Index() akce, můžete vyvolat zadáním následující adresu URL do adresního řádku prohlížeče:</span><span class="sxs-lookup"><span data-stu-id="51513-120">You can invoke the first action, the Index() action, by typing the following URL into your browser address bar:</span></span>

<span data-ttu-id="51513-121">/ Home nebo Index</span><span class="sxs-lookup"><span data-stu-id="51513-121">/Home/Index</span></span>

<span data-ttu-id="51513-122">Druhou akci Details() akce, můžete vyvolat tak, že do prohlížeče zadáte tuto adresu:</span><span class="sxs-lookup"><span data-stu-id="51513-122">You can invoke the second action, the Details() action, by typing this address into your browser:</span></span>

<span data-ttu-id="51513-123">/ Home/podrobnosti</span><span class="sxs-lookup"><span data-stu-id="51513-123">/Home/Details</span></span>

<span data-ttu-id="51513-124">Akce Index() vrátí zobrazení.</span><span class="sxs-lookup"><span data-stu-id="51513-124">The Index() action returns a view.</span></span> <span data-ttu-id="51513-125">Většinu akcí, které vytvoříte vrátí zobrazení.</span><span class="sxs-lookup"><span data-stu-id="51513-125">Most actions that you create will return views.</span></span> <span data-ttu-id="51513-126">Akce však může vrátit jiné typy výsledky akce.</span><span class="sxs-lookup"><span data-stu-id="51513-126">However, an action can return other types of action results.</span></span> <span data-ttu-id="51513-127">Například Details() akce vrátí RedirectToActionResult, který přesměruje příchozí požadavek na akci Index().</span><span class="sxs-lookup"><span data-stu-id="51513-127">For example, the Details() action returns a RedirectToActionResult that redirects incoming request to the Index() action.</span></span>

<span data-ttu-id="51513-128">Akce Index() obsahuje následující jediný řádek kódu:</span><span class="sxs-lookup"><span data-stu-id="51513-128">The Index() action contains the following single line of code:</span></span>

<span data-ttu-id="51513-129">View()</span><span class="sxs-lookup"><span data-stu-id="51513-129">View()</span></span>

<span data-ttu-id="51513-130">Tento řádek kódu vrátí zobrazení, které musí být v následujícím umístění na vašem webovém serveru:</span><span class="sxs-lookup"><span data-stu-id="51513-130">This line of code returns a view that must be located at the following path on your web server:</span></span>

<span data-ttu-id="51513-131">\Views\Home\Index.aspx</span><span class="sxs-lookup"><span data-stu-id="51513-131">\Views\Home\Index.aspx</span></span>

<span data-ttu-id="51513-132">Cesta k zobrazení je odvozeno z názvu řadiče a názvu akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="51513-132">The path to the view is inferred from the name of the controller and the name of the controller action.</span></span>

<span data-ttu-id="51513-133">Pokud dáváte přednost, může být explicitní o zobrazení.</span><span class="sxs-lookup"><span data-stu-id="51513-133">If you prefer, you can be explicit about the view.</span></span> <span data-ttu-id="51513-134">Následující kód vrátí zobrazení s názvem František:</span><span class="sxs-lookup"><span data-stu-id="51513-134">The following line of code returns a view named Fred :</span></span>

<span data-ttu-id="51513-135">Zobrazení (František)</span><span class="sxs-lookup"><span data-stu-id="51513-135">View( Fred )</span></span>

<span data-ttu-id="51513-136">Po provedení tohoto řádku kódu zobrazení vrácených následující cestu:</span><span class="sxs-lookup"><span data-stu-id="51513-136">When this line of code is executed, a view is returned from the following path:</span></span>

<span data-ttu-id="51513-137">\Views\Home\Fred.aspx</span><span class="sxs-lookup"><span data-stu-id="51513-137">\Views\Home\Fred.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="51513-138">Pokud máte v plánu vytvářet testy částí pro aplikace ASP.NET MVC je vhodné být explicitní o názvech zobrazení.</span><span class="sxs-lookup"><span data-stu-id="51513-138">If you plan to create unit tests for your ASP.NET MVC application then it is a good idea to be explicit about view names.</span></span> <span data-ttu-id="51513-139">Tímto způsobem můžete vytvořit testů jednotek k ověření, že očekávané zobrazení vrátil akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="51513-139">That way, you can create a unit test to verify that the expected view was returned by a controller action.</span></span>


## <a name="adding-content-to-a-view"></a><span data-ttu-id="51513-140">Přidávání obsahu do zobrazení</span><span class="sxs-lookup"><span data-stu-id="51513-140">Adding Content to a View</span></span>

<span data-ttu-id="51513-141">Zobrazení je standard (dokumentu HTML, která může obsahovat skripty X).</span><span class="sxs-lookup"><span data-stu-id="51513-141">A view is a standard (X)HTML document that can contain scripts.</span></span> <span data-ttu-id="51513-142">Pomocí skriptů dynamický obsah, přidejte do zobrazení.</span><span class="sxs-lookup"><span data-stu-id="51513-142">You use scripts to add dynamic content to a view.</span></span>

<span data-ttu-id="51513-143">Zobrazení v výpis 2 se například zobrazí aktuální datum a čas.</span><span class="sxs-lookup"><span data-stu-id="51513-143">For example, the view in Listing 2 displays the current date and time.</span></span>

<span data-ttu-id="51513-144">**Listing 2 - \Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="51513-144">**Listing 2 - \Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample2.aspx)]

<span data-ttu-id="51513-145">Všimněte si, že tělo stránky HTML v výpis 2 obsahuje následující skript:</span><span class="sxs-lookup"><span data-stu-id="51513-145">Notice that the body of the HTML page in Listing 2 contains the following script:</span></span>

<span data-ttu-id="51513-146">&lt;% Response.Write(DateTime.Now)%&gt;</span><span class="sxs-lookup"><span data-stu-id="51513-146">&lt;% Response.Write(DateTime.Now)%&gt;</span></span>

<span data-ttu-id="51513-147">Použít oddělovače skriptu &lt;% a %&gt; označit začátek a konec skriptu.</span><span class="sxs-lookup"><span data-stu-id="51513-147">You use the script delimiters &lt;% and %&gt; to mark the beginning and end of a script.</span></span> <span data-ttu-id="51513-148">Tento skript je napsán v jazyce Visual basic.</span><span class="sxs-lookup"><span data-stu-id="51513-148">This script is written in Visual basic.</span></span> <span data-ttu-id="51513-149">Zobrazí aktuální datum a čas voláním metody Response.Write() k vykreslení obsahu v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="51513-149">It displays the current date and time by calling the Response.Write() method to render content to the browser.</span></span> <span data-ttu-id="51513-150">Oddělovače skriptu &lt;% a %&gt; lze použít k provedení jednoho nebo více příkazů.</span><span class="sxs-lookup"><span data-stu-id="51513-150">The script delimiters &lt;% and %&gt; can be used to execute one or more statements.</span></span>

<span data-ttu-id="51513-151">Vzhledem k tomu, že zavoláte Response.Write() tak často, Microsoft vám poskytne zástupce pro volání metody Response.Write().</span><span class="sxs-lookup"><span data-stu-id="51513-151">Since you call Response.Write() so often, Microsoft provides you with a shortcut for calling the Response.Write() method.</span></span> <span data-ttu-id="51513-152">Zobrazení v výpis 3 používá oddělovače &lt;% = a %&gt; jako zástupce pro volání Response.Write().</span><span class="sxs-lookup"><span data-stu-id="51513-152">The view in Listing 3 uses the delimiters &lt;%= and %&gt; as a shortcut for calling Response.Write().</span></span>

<span data-ttu-id="51513-153">**Listing 3 - Views\Home\Index2.aspx**</span><span class="sxs-lookup"><span data-stu-id="51513-153">**Listing 3 - Views\Home\Index2.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample3.aspx)]

<span data-ttu-id="51513-154">Kterémkoli jazyce platformy .NET můžete použít ke generování dynamický obsah v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="51513-154">You can use any .NET language to generate dynamic content in a view.</span></span> <span data-ttu-id="51513-155">Za normálních okolností udou můžete použít buď Visual Basic .NET nebo C# k zápisu kontrolery a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="51513-155">Normally, you�ll use either Visual Basic .NET or C# to write your controllers and views.</span></span>

## <a name="using-html-helpers-to-generate-view-content"></a><span data-ttu-id="51513-156">Použití pomocných rutinách HTML pro generování zobrazení obsahu</span><span class="sxs-lookup"><span data-stu-id="51513-156">Using HTML Helpers to Generate View Content</span></span>

<span data-ttu-id="51513-157">Aby bylo snazší přidání obsahu do zobrazení, můžete využít výhod takzvaný *pomocné rutiny HTML*.</span><span class="sxs-lookup"><span data-stu-id="51513-157">To make it easier to add content to a view, you can take advantage of something called an *HTML Helper*.</span></span> <span data-ttu-id="51513-158">Pomocné rutiny HTML, je obvykle metodu, která generuje řetězec.</span><span class="sxs-lookup"><span data-stu-id="51513-158">An HTML Helper, typically, is a method that generates a string.</span></span> <span data-ttu-id="51513-159">Pomocné rutiny HTML můžete použít ke generování standardní elementů HTML například textových polí, odkazů, rozevírací seznamy a seznamy.</span><span class="sxs-lookup"><span data-stu-id="51513-159">You can use HTML Helpers to generate standard HTML elements such as textboxes, links, dropdown lists, and list boxes.</span></span>

<span data-ttu-id="51513-160">Například zobrazení v výpis 4 trvá výhod tři pomocné rutiny HTML--BeginForm(), TextBox() a Password() pomocníky – ke generování přihlášení formuláři (viz obrázek 1).</span><span class="sxs-lookup"><span data-stu-id="51513-160">For example, the view in Listing 4 takes advantage of three HTML Helpers -- the BeginForm(), the TextBox() and Password() helpers -- to generate a Login form (see Figure 1).</span></span>

<span data-ttu-id="51513-161">**Listing 4 -- \Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="51513-161">**Listing 4 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample4.aspx)]


<span data-ttu-id="51513-162">[![Dialogové okno Nový projekt](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="51513-162">[![The New Project dialog box](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)</span></span>

<span data-ttu-id="51513-163">**Obrázek 01**: standardní přihlašovací formulář ([Kliknutím zobrazit obrázek v plné velikosti](asp-net-mvc-views-overview-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="51513-163">**Figure 01**: A standard Login form ([Click to view full-size image](asp-net-mvc-views-overview-vb/_static/image2.png))</span></span>


<span data-ttu-id="51513-164">Všechny metody pomocné rutiny HTML, se nazývají na vlastnost Html zobrazení.</span><span class="sxs-lookup"><span data-stu-id="51513-164">All of the HTML Helpers methods are called on the Html property of the view.</span></span> <span data-ttu-id="51513-165">Například voláním metody Html.TextBox() vykreslit textové pole.</span><span class="sxs-lookup"><span data-stu-id="51513-165">For example, you render a TextBox by calling the Html.TextBox() method.</span></span>

<span data-ttu-id="51513-166">Všimněte si, že používáte oddělovače skriptu &lt;% = a %&gt; při volání metody Html.TextBox() i Html.Password() pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="51513-166">Notice that you use the script delimiters &lt;%= and %&gt; when calling both the Html.TextBox() and Html.Password() helpers.</span></span> <span data-ttu-id="51513-167">Tyto pomocné rutiny jednoduše vrátí řetězec.</span><span class="sxs-lookup"><span data-stu-id="51513-167">These helpers simply return a string.</span></span> <span data-ttu-id="51513-168">Je třeba volat Response.Write() k vykreslení řetězec do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="51513-168">You need to call Response.Write() in order to render the string to the browser.</span></span>

<span data-ttu-id="51513-169">Pomocí metody HTML Helper je volitelný.</span><span class="sxs-lookup"><span data-stu-id="51513-169">Using HTML Helper methods is optional.</span></span> <span data-ttu-id="51513-170">Jejich usnadnit vám život snížením HTML a skript, který budete muset napsat.</span><span class="sxs-lookup"><span data-stu-id="51513-170">They make your life easier by reducing the amount of HTML and script that you need to write.</span></span> <span data-ttu-id="51513-171">Zobrazení v výpis 5 vykreslí přesný stejného formuláře jako zobrazení v výpis 4 bez použití pomocné rutiny HTML.</span><span class="sxs-lookup"><span data-stu-id="51513-171">The view in Listing 5 renders the exact same form as the view in Listing 4 without using HTML Helpers.</span></span>

<span data-ttu-id="51513-172">**Listing 5 -- \Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="51513-172">**Listing 5 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample5.aspx)]

<span data-ttu-id="51513-173">Máte také možnost vytvořit vlastní pomocné rutiny HTML.</span><span class="sxs-lookup"><span data-stu-id="51513-173">You also have the option of creating your own HTML Helpers.</span></span> <span data-ttu-id="51513-174">Například můžete vytvořit GridView() Pomocná metoda, která zobrazuje sadu záznamů databáze automaticky do tabulky HTML.</span><span class="sxs-lookup"><span data-stu-id="51513-174">For example, you can create a GridView() helper method that displays a set of database records in an HTML table automatically.</span></span> <span data-ttu-id="51513-175">Toto téma jsme prozkoumat v tomto kurzu **vytváření Pomocníci HTML vlastní**.</span><span class="sxs-lookup"><span data-stu-id="51513-175">We explore this topic in the tutorial **Creating Custom HTML Helpers**.</span></span>

## <a name="using-view-data-to-pass-data-to-a-view"></a><span data-ttu-id="51513-176">Pomocí zobrazení dat k předávání dat zobrazení</span><span class="sxs-lookup"><span data-stu-id="51513-176">Using View Data to Pass Data to a View</span></span>

<span data-ttu-id="51513-177">Zobrazení dat slouží k předávání dat z řadiče zobrazení.</span><span class="sxs-lookup"><span data-stu-id="51513-177">You use view data to pass data from a controller to a view.</span></span> <span data-ttu-id="51513-178">Vezměte v úvahu zobrazení dat, jako je balíček, který odeslat pomocí e-mailu.</span><span class="sxs-lookup"><span data-stu-id="51513-178">Think of view data like a package that you send through the mail.</span></span> <span data-ttu-id="51513-179">Pomocí tohoto balíčku, musí se poslat všechna data z řadič předána do zobrazení.</span><span class="sxs-lookup"><span data-stu-id="51513-179">All data passed from a controller to a view must be sent using this package.</span></span> <span data-ttu-id="51513-180">Například řadič v výpis 6 přidá zprávu zobrazovat data.</span><span class="sxs-lookup"><span data-stu-id="51513-180">For example, the controller in Listing 6 adds a message to view data.</span></span>

<span data-ttu-id="51513-181">**Výpis 6 - ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="51513-181">**Listing 6 - ProductController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample6.vb)]

<span data-ttu-id="51513-182">Řadič ViewData vlastnost představuje kolekci dvojic název a hodnotu.</span><span class="sxs-lookup"><span data-stu-id="51513-182">The controller ViewData property represents a collection of name and value pairs.</span></span> <span data-ttu-id="51513-183">Metoda Index() v výpis 6, přidá položku do kolekce zobrazení dat s názvem zprávy s hodnotou Hello, World!.</span><span class="sxs-lookup"><span data-stu-id="51513-183">In Listing 6, the Index() method adds an item to the view data collection named message with the value Hello World!.</span></span> <span data-ttu-id="51513-184">Při zobrazení vráceném metodou Index(), data zobrazení jsou automaticky předána do zobrazení.</span><span class="sxs-lookup"><span data-stu-id="51513-184">When the view is returned by the Index() method, the view data is passed to the view automatically.</span></span>

<span data-ttu-id="51513-185">Zobrazení v výpis 7 načte zprávu z dat zobrazení a vykreslí zprávu, která se v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="51513-185">The view in Listing 7 retrieves the message from the view data and renders the message to the browser.</span></span>

<span data-ttu-id="51513-186">**Listing 7 -- \Views\Product\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="51513-186">**Listing 7 -- \Views\Product\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample7.aspx)]

<span data-ttu-id="51513-187">Všimněte si, že zobrazení využívá metody pomocné rutiny HTML Html.Encode() při vykreslování zprávy.</span><span class="sxs-lookup"><span data-stu-id="51513-187">Notice that the view takes advantage of the Html.Encode() HTML Helper method when rendering the message.</span></span> <span data-ttu-id="51513-188">Pomocné rutiny HTML Html.Encode() kóduje zvláštní znaky, jako &lt; a &gt; do znaky, které jsou bezpečné pro zobrazení na webové stránce.</span><span class="sxs-lookup"><span data-stu-id="51513-188">The Html.Encode() HTML Helper encodes special characters such as &lt; and &gt; into characters that are safe to display in a web page.</span></span> <span data-ttu-id="51513-189">Vždy, když jste vykreslení obsahu, který uživatel odeslal na web, by měl kódování obsahu zabránit útokům vkládání JavaScript.</span><span class="sxs-lookup"><span data-stu-id="51513-189">Whenever you render content that a user submits to a website, you should encode the content to prevent JavaScript injection attacks.</span></span>

<span data-ttu-id="51513-190">(Protože jsme vytvořili zprávu sebe v ProductController, jsme nejsou zobrazeny t skutečně potřebujete ke kódování zprávy.</span><span class="sxs-lookup"><span data-stu-id="51513-190">(Because we created the message ourselves in the ProductController, we don�t really need to encode the message.</span></span> <span data-ttu-id="51513-191">Je však dobré která podporují vždy volat metodu Html.Encode() při zobrazování obsahu načteny z zobrazení dat v rámci zobrazení.)</span><span class="sxs-lookup"><span data-stu-id="51513-191">However, it is a good habit to always call the Html.Encode() method when displaying content retrieved from view data within a view.)</span></span>

<span data-ttu-id="51513-192">Ve výpisu 7 vzali jsme výhod předat zprávu jednoduchým řetězcem z řadiče zobrazení dat zobrazení.</span><span class="sxs-lookup"><span data-stu-id="51513-192">In Listing 7, we took advantage of view data to pass a simple string message from a controller to a view.</span></span> <span data-ttu-id="51513-193">Zobrazení dat můžete použít taky jiné typy dat, jako jsou kolekce záznamů databáze, z řadiče zobrazení.</span><span class="sxs-lookup"><span data-stu-id="51513-193">You also can use view data to pass other types of data, such as a collection of database records, from a controller to a view.</span></span> <span data-ttu-id="51513-194">Například pokud chcete zobrazit obsah databázové tabulky produktů v zobrazení, pak by úspěšně prošel zpracováním kolekci databáze záznamů v zobrazení data.</span><span class="sxs-lookup"><span data-stu-id="51513-194">For example, if you want to display the contents of the Products database table in a view, then you would pass the collection of database records in view data.</span></span>

<span data-ttu-id="51513-195">Máte také možnost předat data silného typu zobrazení z řadiče zobrazení.</span><span class="sxs-lookup"><span data-stu-id="51513-195">You also have the option of passing strongly typed view data from a controller to a view.</span></span> <span data-ttu-id="51513-196">Toto téma jsme prozkoumat v tomto kurzu **Principy silného typu zobrazení dat a zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="51513-196">We explore this topic in the tutorial **Understanding Strongly Typed View Data and Views**.</span></span>

## <a name="summary"></a><span data-ttu-id="51513-197">Souhrn</span><span class="sxs-lookup"><span data-stu-id="51513-197">Summary</span></span>

<span data-ttu-id="51513-198">V tomto kurzu poskytuje stručný úvod do architektury ASP.NET MVC zobrazení, data zobrazení a pomocné rutiny HTML.</span><span class="sxs-lookup"><span data-stu-id="51513-198">This tutorial provided a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="51513-199">V první části jste zjistili, jak přidat nové zobrazení do projektu.</span><span class="sxs-lookup"><span data-stu-id="51513-199">In the first section, you learned how to add new views to your project.</span></span> <span data-ttu-id="51513-200">Jste zjistili, že je nutné přidat zobrazení do správné složky k volání z určitý kontroler.</span><span class="sxs-lookup"><span data-stu-id="51513-200">You learned that you must add a view to the right folder in order to call it from a particular controller.</span></span> <span data-ttu-id="51513-201">V dalším kroku jsme popsané v tématu pomocné rutiny HTML.</span><span class="sxs-lookup"><span data-stu-id="51513-201">Next, we discussed the topic of HTML Helpers.</span></span> <span data-ttu-id="51513-202">Jste se dozvěděli, jak pomocné objekty HTML umožňují snadno vygenerovat standardní obsah HTML.</span><span class="sxs-lookup"><span data-stu-id="51513-202">You learned how HTML Helpers enable you to easily generate standard HTML content.</span></span> <span data-ttu-id="51513-203">Nakonec jste zjistili, jak chcete využít výhod předat data z řadiče zobrazení dat zobrazení.</span><span class="sxs-lookup"><span data-stu-id="51513-203">Finally, you learned how to take advantage of view data to pass data from a controller to a view.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="51513-204">[Předchozí](passing-data-to-view-master-pages-cs.md)
> [další](creating-custom-html-helpers-vb.md)</span><span class="sxs-lookup"><span data-stu-id="51513-204">[Previous](passing-data-to-view-master-pages-cs.md)
[Next](creating-custom-html-helpers-vb.md)</span></span>
