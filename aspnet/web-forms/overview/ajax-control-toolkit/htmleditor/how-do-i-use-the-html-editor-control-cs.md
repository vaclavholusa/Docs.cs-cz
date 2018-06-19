---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-cs
title: Používání ovládacího prvku HTML Editor (C#) | Microsoft Docs
author: microsoft
description: HTMLEditor je ovládací prvek ASP.NET AJAX, který umožňuje snadno vytvářet a upravovat obsah HTML pomocí tlačítek na panelu nástrojů.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: f47e6224-c2e5-4472-b069-b6c7b6115200
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-cs
msc.type: authoredcontent
ms.openlocfilehash: fca18948c0e4f1323f214dc0033f19fa44efad47
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879774"
---
<a name="how-do-i-use-the-html-editor-control-c"></a><span data-ttu-id="55a91-104">Používání ovládacího prvku HTML Editor</span><span class="sxs-lookup"><span data-stu-id="55a91-104">How do I use the HTML Editor Control?</span></span> <span data-ttu-id="55a91-105">(C#)</span><span class="sxs-lookup"><span data-stu-id="55a91-105">(C#)</span></span>
====================
<span data-ttu-id="55a91-106">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="55a91-106">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="55a91-107">HTMLEditor je ovládací prvek ASP.NET AJAX, který umožňuje snadno vytvářet a upravovat obsah HTML pomocí tlačítek na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="55a91-107">HTMLEditor is an ASP.NET AJAX Control that allows you to easily create and edit HTML content via buttons in a toolbar.</span></span>


<span data-ttu-id="55a91-108">Cílem tohoto kurzu je poskytnout Přehled ovládacího prvku HTML Editor součástí Toolkitu AJAX.</span><span class="sxs-lookup"><span data-stu-id="55a91-108">The goal of this tutorial is to provide you with an overview of the HTML Editor control included with the AJAX Control Toolkit.</span></span> <span data-ttu-id="55a91-109">HTML Editor obsahuje možnosti pro změnu velikosti písma, výběr písmo, změna barvy pozadí, úprava barvu popředí přidávání odkazů, přidávání obrázků, Změna zarovnání textu a provádění operací vyjmutí, kopírování a vložení operací (viz obrázek 1).</span><span class="sxs-lookup"><span data-stu-id="55a91-109">The HTML Editor includes options for changing font size, selecting a font, changing background color, modifying the foreground color, adding links, adding images, changing text alignment, and performing cut, copy, and paste operations (see Figure 1).</span></span>


<span data-ttu-id="55a91-110">[![HTML Editor](how-do-i-use-the-html-editor-control-cs/_static/image1.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="55a91-110">[![The HTML Editor](how-do-i-use-the-html-editor-control-cs/_static/image1.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="55a91-111">**Obrázek 01**: editoru HTML ([Kliknutím zobrazit obrázek v plné velikosti](how-do-i-use-the-html-editor-control-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="55a91-111">**Figure 01**: The HTML Editor([Click to view full-size image](how-do-i-use-the-html-editor-control-cs/_static/image2.png))</span></span>


<span data-ttu-id="55a91-112">HTML editor můžete zadat obsah pomocí režimu návrhu nebo HTML můžete zadat přímo.</span><span class="sxs-lookup"><span data-stu-id="55a91-112">The HTML editor enables you to enter content using a design mode or you can enter HTML directly.</span></span> <span data-ttu-id="55a91-113">Rovněž jsou k dispozici možnost zobrazte náhled vašeho obsahu HTML (viz obrázek 2).</span><span class="sxs-lookup"><span data-stu-id="55a91-113">You also are provided with the option to preview your HTML content (see Figure 2).</span></span>


<span data-ttu-id="55a91-114">[![Návrh, HTML a náhled tlačítka](how-do-i-use-the-html-editor-control-cs/_static/image2.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="55a91-114">[![Design, HTML, and Preview buttons](how-do-i-use-the-html-editor-control-cs/_static/image2.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image3.png)</span></span>

<span data-ttu-id="55a91-115">**Obrázek 02**: návrh, HTML a náhled tlačítka ([Kliknutím zobrazit obrázek v plné velikosti](how-do-i-use-the-html-editor-control-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="55a91-115">**Figure 02**: Design, HTML, and Preview buttons([Click to view full-size image](how-do-i-use-the-html-editor-control-cs/_static/image4.png))</span></span>


<span data-ttu-id="55a91-116">V tomto kurzu zjistíte, jak zobrazit HTML Editor, postup přizpůsobení tlačítka panelu nástrojů, který se zobrazí v editoru HTML a jak se vyhnout skriptování útoků webů.</span><span class="sxs-lookup"><span data-stu-id="55a91-116">In this tutorial, you learn how to display the HTML Editor, how to customize the toolbar buttons that appear in the HTML Editor, and how to avoid Cross-Site Scripting Attacks.</span></span>

## <a name="displaying-the-html-editor"></a><span data-ttu-id="55a91-117">Zobrazování editoru HTML</span><span class="sxs-lookup"><span data-stu-id="55a91-117">Displaying the HTML Editor</span></span>

<span data-ttu-id="55a91-118">Předtím, než můžete použít HTML Editor na stránce technologie ASP.NET, je nejprve nutno přidat ovládací prvek ScriptManager na stránku.</span><span class="sxs-lookup"><span data-stu-id="55a91-118">Before you can use the HTML Editor in an ASP.NET page, you must first add a ScriptManager control to the page.</span></span> <span data-ttu-id="55a91-119">Ovládací prvek ScriptManager se nachází pod kartě Rozšíření AJAX v sadě nástrojů Visual Studio nebo Visual Web Developer Express.</span><span class="sxs-lookup"><span data-stu-id="55a91-119">The ScriptManager control is located beneath the AJAX Extensions tab in the Visual Studio/Visual Web Developer Express toolbox.</span></span>

<span data-ttu-id="55a91-120">V horní části stránky před všechny ovládací prvky na stránce by měl umístěte ovládací prvek ScriptManager.</span><span class="sxs-lookup"><span data-stu-id="55a91-120">You should place the ScriptManager control at the top of the page before any other controls on the page.</span></span> <span data-ttu-id="55a91-121">Například můžete umístit ji okamžitě níže serverovou otevírání &lt;formuláře&gt; značky.</span><span class="sxs-lookup"><span data-stu-id="55a91-121">For example, you can place it immediately below the opening server-side &lt;form&gt; tag.</span></span>

<span data-ttu-id="55a91-122">Ovládací prvek HTML Editor se nachází v sadě nástrojů se zbytkem prvky AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="55a91-122">The HTML Editor control is located in the toolbox with the rest of the AJAX Control Toolkit controls.</span></span> <span data-ttu-id="55a91-123">Je název ovládacího prvku Editor (viz obrázek 3).</span><span class="sxs-lookup"><span data-stu-id="55a91-123">It is named the Editor control (see Figure 3).</span></span>


<span data-ttu-id="55a91-124">[![Ovládací prvek HTML Editor](how-do-i-use-the-html-editor-control-cs/_static/image3.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="55a91-124">[![The HTML Editor control](how-do-i-use-the-html-editor-control-cs/_static/image3.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image5.png)</span></span>

<span data-ttu-id="55a91-125">**Obrázek 03**: ovládací prvek editoru HTML ([Kliknutím zobrazit obrázek v plné velikosti](how-do-i-use-the-html-editor-control-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="55a91-125">**Figure 03**: The HTML Editor control([Click to view full-size image](how-do-i-use-the-html-editor-control-cs/_static/image6.png))</span></span>


<span data-ttu-id="55a91-126">Po přetáhnete HTML Editor na stránce, můžete nastavit jeho vlastnosti v seznamu vlastností.</span><span class="sxs-lookup"><span data-stu-id="55a91-126">After you drag the HTML Editor onto a page, you can set its properties in the property sheet.</span></span> <span data-ttu-id="55a91-127">Například chcete normálně nastavit vlastnosti Šířka a výška.</span><span class="sxs-lookup"><span data-stu-id="55a91-127">For example, you normally want to set the Width and Height properties.</span></span> <span data-ttu-id="55a91-128">Výpis 1 obsahuje daný zdroj pro stránku ASP.NET, který obsahuje HTML editor.</span><span class="sxs-lookup"><span data-stu-id="55a91-128">Listing 1 contains the source for an ASP.NET page that contains an HTML editor.</span></span>

<span data-ttu-id="55a91-129">**Výpis 1 - SimpleEditor.aspx**</span><span class="sxs-lookup"><span data-stu-id="55a91-129">**Listing 1 - SimpleEditor.aspx**</span></span>

[!code-aspx[Main](how-do-i-use-the-html-editor-control-cs/samples/sample1.aspx)]

<span data-ttu-id="55a91-130">Stránka v výpis 1 obsahuje ovládací prvek HTML, ovládacího prvku tlačítko a prvku Literal control.</span><span class="sxs-lookup"><span data-stu-id="55a91-130">The page in Listing 1 contains an HTML Editor control, a Button control, and a Literal control.</span></span> <span data-ttu-id="55a91-131">Když kliknete na tlačítko, obsah HTML Editor se zobrazí v ovládacím prvku Literal control (viz obrázek 4).</span><span class="sxs-lookup"><span data-stu-id="55a91-131">When you click the button, the contents of the HTML Editor appear in the Literal control (see Figure 4).</span></span>


<span data-ttu-id="55a91-132">[![Odeslání formuláře se Editor HTML](how-do-i-use-the-html-editor-control-cs/_static/image4.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="55a91-132">[![Submitting a form with an HTML Editor](how-do-i-use-the-html-editor-control-cs/_static/image4.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image7.png)</span></span>

<span data-ttu-id="55a91-133">**Obrázek 04**: odeslání formuláře se Editor HTML ([Kliknutím zobrazit obrázek v plné velikosti](how-do-i-use-the-html-editor-control-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="55a91-133">**Figure 04**: Submitting a form with an HTML Editor([Click to view full-size image](how-do-i-use-the-html-editor-control-cs/_static/image8.png))</span></span>


<span data-ttu-id="55a91-134">HTML Editor obsah vlastnost se používá k získání obsahu HTML zadali do editoru HTML.</span><span class="sxs-lookup"><span data-stu-id="55a91-134">The HTML Editor Content property is used to retrieve the HTML content entered into the HTML Editor.</span></span> <span data-ttu-id="55a91-135">Upozorňujeme, že tento obsah HTML může obsahovat JavaScript.</span><span class="sxs-lookup"><span data-stu-id="55a91-135">Be aware that this HTML content can contain JavaScript.</span></span> <span data-ttu-id="55a91-136">V další části probereme, jak můžete zabránit útokům vkládání JavaScript.</span><span class="sxs-lookup"><span data-stu-id="55a91-136">In the next section, we discuss how you can prevent JavaScript Injection Attacks.</span></span>

## <a name="customizing-the-html-editor-toolbar"></a><span data-ttu-id="55a91-137">Přizpůsobení panelu nástrojů editoru HTML</span><span class="sxs-lookup"><span data-stu-id="55a91-137">Customizing the HTML Editor Toolbar</span></span>

<span data-ttu-id="55a91-138">Můžete přizpůsobit přesně tlačítek, která se zobrazí v editoru.</span><span class="sxs-lookup"><span data-stu-id="55a91-138">You can customize exactly which buttons appear in the editor.</span></span> <span data-ttu-id="55a91-139">Například můžete chtít odebrat Karta HTML uživatelům zabránit v editoru HTML přepnout do režimu HTML.</span><span class="sxs-lookup"><span data-stu-id="55a91-139">For example, you might want to remove the HTML tab to prevent users from switching the HTML Editor into HTML mode.</span></span> <span data-ttu-id="55a91-140">Nebo můžete chtít odebrat rozevíracího seznamu velikost písma uživatelům zabránit ve vytváření příliš velké textu ve fóru zprávy post (viz obrázek 5).</span><span class="sxs-lookup"><span data-stu-id="55a91-140">Or, you might want to remove the font size dropdown list to prevent users from creating overly large text in a forum message post (see Figure 5).</span></span>


<span data-ttu-id="55a91-141">[![Vlastní Editor HTML](how-do-i-use-the-html-editor-control-cs/_static/image5.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="55a91-141">[![A customized HTML Editor](how-do-i-use-the-html-editor-control-cs/_static/image5.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image9.png)</span></span>

<span data-ttu-id="55a91-142">**Obrázek 05**: A přizpůsobit HTML Editor ([Kliknutím zobrazit obrázek v plné velikosti](how-do-i-use-the-html-editor-control-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="55a91-142">**Figure 05**: A customized HTML Editor([Click to view full-size image](how-do-i-use-the-html-editor-control-cs/_static/image10.png))</span></span>


<span data-ttu-id="55a91-143">Tlačítka panelu nástrojů můžete přizpůsobit odvozením nové Editor HTML ze základní třídy Editor.</span><span class="sxs-lookup"><span data-stu-id="55a91-143">You customize the toolbar buttons by deriving a new HTML Editor from the base Editor class.</span></span> <span data-ttu-id="55a91-144">Například vlastní editor v výpis 2 obsahuje jenom tlačítka panelu nástrojů pro tučné písmo a kurzíva.</span><span class="sxs-lookup"><span data-stu-id="55a91-144">For example, the custom editor in Listing 2 only contains toolbar buttons for bold and italic.</span></span> <span data-ttu-id="55a91-145">Byly odebrány všechny další tlačítka panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="55a91-145">All other toolbar buttons have been removed.</span></span> <span data-ttu-id="55a91-146">Kromě toho Karta HTML byla odebrána v dolní části editoru (ale karty návrh a Preview stále existují).</span><span class="sxs-lookup"><span data-stu-id="55a91-146">Furthermore, the HTML tab has been removed from the bottom of the editor (but the Design and Preview tabs are still there).</span></span>

<span data-ttu-id="55a91-147">**Výpis 2 – aplikace\_Code\CustomEditor.cs**</span><span class="sxs-lookup"><span data-stu-id="55a91-147">**Listing 2 - App\_Code\CustomEditor.cs**</span></span>

[!code-csharp[Main](how-do-i-use-the-html-editor-control-cs/samples/sample2.cs)]

<span data-ttu-id="55a91-148">Třída výpis 2 musíte přidat do vaší aplikace\_Code složky tak, aby se automaticky zkompiluje třídy.</span><span class="sxs-lookup"><span data-stu-id="55a91-148">You must add the class in Listing 2 to your App\_Code folder so that the class will be compiled automatically.</span></span> <span data-ttu-id="55a91-149">Pokud aplikace\_kód složka neexistuje ve vašem webu potom můžete jednoduše přidat složku.</span><span class="sxs-lookup"><span data-stu-id="55a91-149">If the App\_Code folder does not exist in your website then you can simply add the folder.</span></span>

<span data-ttu-id="55a91-150">Po vytvoření vlastního editoru, můžete ho přidat do stránky ASP.NET stejným způsobem, jak přidat normální Editor HTML (viz seznam 3).</span><span class="sxs-lookup"><span data-stu-id="55a91-150">After you create a custom editor, you can add it to an ASP.NET page in the same way as you add the normal HTML Editor (see Listing 3).</span></span>

<span data-ttu-id="55a91-151">**Listing 3 - ShowCustomEditor.aspx**</span><span class="sxs-lookup"><span data-stu-id="55a91-151">**Listing 3 - ShowCustomEditor.aspx**</span></span>

[!code-aspx[Main](how-do-i-use-the-html-editor-control-cs/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a><span data-ttu-id="55a91-152">Vyhněte útoky skriptování (XSS)</span><span class="sxs-lookup"><span data-stu-id="55a91-152">Avoiding Cross-Site Scripting (XSS) Attacks</span></span>

<span data-ttu-id="55a91-153">Vždy, když přijímají vstup od uživatele a znovu zobrazit tento vstup na vašem webu, potenciálně otevřete webovou stránku webů Skriptování útoky.</span><span class="sxs-lookup"><span data-stu-id="55a91-153">Whenever you accept input from a user, and redisplay that input on your website, you potentially open your website to Cross-Site Scripting (XSS) Attacks.</span></span> <span data-ttu-id="55a91-154">Teoreticky může kyberzločinci odeslat kód jazyka JavaScript, která se provede při vstupu se zobrazí znovu.</span><span class="sxs-lookup"><span data-stu-id="55a91-154">In theory, a malicious hacker could submit JavaScript code that gets executed when the input is redisplayed.</span></span> <span data-ttu-id="55a91-155">Jazyk JavaScript může vám ukrást hesla uživatele a dalších citlivých údajů.</span><span class="sxs-lookup"><span data-stu-id="55a91-155">The JavaScript could be used to steal user passwords or other sensitive information.</span></span>

<span data-ttu-id="55a91-156">Za normálních okolností vůbec nemělo útoky XSS pomocí kódování ať vstup načíst od uživatele před jejich zobrazením na webové stránce HTML.</span><span class="sxs-lookup"><span data-stu-id="55a91-156">Normally, you can defeat XSS attacks by HTML encoding whatever input you retrieve from a user before displaying it in a web page.</span></span> <span data-ttu-id="55a91-157">Však nebude pouze kódování HTML kódování výstup HTML Editor &lt;skriptu&gt; značky, by ho také kódování všechny značky HTML.</span><span class="sxs-lookup"><span data-stu-id="55a91-157">However, HTML encoding the output of the HTML Editor would not only encode &lt;script&gt; tags, it would also encode all HTML tags.</span></span> <span data-ttu-id="55a91-158">Jinými slovy by přijdete o veškeré formátování například typ písma, velikosti písma a barvy pozadí.</span><span class="sxs-lookup"><span data-stu-id="55a91-158">In other words, you would lose all of the formatting such as the font type, font size, and background color.</span></span>

<span data-ttu-id="55a91-159">Pokud shromažďujete citlivé informace od uživatelů – třeba hesla, čísla platebních karet a čísel sociálního pojištění - nesmí zobrazit zrušení kódovaného obsah, který je načíst z uživatele s Editor HTML.</span><span class="sxs-lookup"><span data-stu-id="55a91-159">If you are collecting sensitive information from your users -- such as passwords, credit-card numbers, and social security numbers - then you should not display un-encoded content that you retrieve from a user with the HTML Editor.</span></span> <span data-ttu-id="55a91-160">HTML Editor používejte jenom v situacích, ve kterých nejsou opakované zobrazování obsah HTML nebo obsah HTML, který je odesílán na váš web důvěryhodná strana.</span><span class="sxs-lookup"><span data-stu-id="55a91-160">You should use the HTML Editor only in situations in which you are not redisplaying the HTML content, or the HTML content is being submitted to your website by a trusted party.</span></span>

<span data-ttu-id="55a91-161">Představte si například, že vytváříte aplikace blogu.</span><span class="sxs-lookup"><span data-stu-id="55a91-161">Imagine, for example, that you are creating a blog application.</span></span> <span data-ttu-id="55a91-162">V takovém případě má smysl pro použití editoru HTML při sestavování příspěvcích na blogu.</span><span class="sxs-lookup"><span data-stu-id="55a91-162">In this situation, it makes sense to use the HTML Editor when composing blog posts.</span></span> <span data-ttu-id="55a91-163">Jsou pouze jeden, který odešle příspěvku na blogu a pravděpodobně z důvodu, můžete důvěřovat sami nechcete odeslat škodlivý JavaScript.</span><span class="sxs-lookup"><span data-stu-id="55a91-163">You are the only one who submits a blog post and, presumably, you can trust yourself not to submit malicious JavaScript.</span></span> <span data-ttu-id="55a91-164">Nedává však smysl pro použití editoru HTML při povolení anonymní uživatelé odeslat komentáře.</span><span class="sxs-lookup"><span data-stu-id="55a91-164">However, it does not make sense to use the HTML Editor when allowing anonymous users to post comments.</span></span> <span data-ttu-id="55a91-165">Byste měli být opatrní hlavně v situacích, ve kterých uživatelé odesílat citlivé informace, jako jsou hesla.</span><span class="sxs-lookup"><span data-stu-id="55a91-165">You should be especially careful in situations in which users submit sensitive information such as passwords.</span></span> <span data-ttu-id="55a91-166">Uživatel se zlými úmysly může potenciálně, post komentář, který obsahuje správné JavaScript pro krádež hesla.</span><span class="sxs-lookup"><span data-stu-id="55a91-166">Potentially, a malicious user could post a comment that contains the right JavaScript for stealing a password.</span></span>

## <a name="summary"></a><span data-ttu-id="55a91-167">Souhrn</span><span class="sxs-lookup"><span data-stu-id="55a91-167">Summary</span></span>

<span data-ttu-id="55a91-168">V tomto kurzu byly poskytnuty s stručný přehled ovládacího prvku HTML Editor součástí Toolkitu AJAX.</span><span class="sxs-lookup"><span data-stu-id="55a91-168">In this tutorial, you were provided with a brief overview of the HTML Editor control included in the AJAX Control Toolkit.</span></span> <span data-ttu-id="55a91-169">Jste zjistili, jak pomocí editoru HTML přijmout bohaté obsah od uživatele a odeslání obsahu na server.</span><span class="sxs-lookup"><span data-stu-id="55a91-169">You learned how to use the HTML Editor to accept rich content from a user and submit the content to the server.</span></span> <span data-ttu-id="55a91-170">Také popsané, jak můžete přizpůsobit tlačítka panelu nástrojů, která se zobrazí HTML Editor.</span><span class="sxs-lookup"><span data-stu-id="55a91-170">We also discussed how you can customize the toolbar buttons that are displayed by the HTML Editor.</span></span> <span data-ttu-id="55a91-171">Nakonec jste zjistili, jak se vyhnout skriptování útoků webů při použití editoru HTML tak, aby přijímal potenciálně škodlivý vstup.</span><span class="sxs-lookup"><span data-stu-id="55a91-171">Finally, you learned how to avoid Cross-Site Scripting Attacks when using the HTML Editor to accept potentially malicious input.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="55a91-172">Next</span><span class="sxs-lookup"><span data-stu-id="55a91-172">Next</span></span>](how-do-i-use-the-html-editor-control-vb.md)
