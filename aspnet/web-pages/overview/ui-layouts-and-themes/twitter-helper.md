---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Rozhraní ASP.NET Web Pages pomocná rutina twitteru | Dokumentace Microsoftu
author: tfitzmac
description: V tomto tématu a aplikace ukazují, jak přidat do projektu služby WebMatrix 3 Pomocník Twitter. Obsahuje kód Pomocník Twitter a ukazuje způsob volání pomocné rutiny...
ms.author: aspnetcontent
ms.date: 02/07/2014
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 1824b677a7ba96ea6fc5119610725a30d472764e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817652"
---
<a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="d02cf-104">Pomocník Twitter s webovými stránkami ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d02cf-104">Twitter Helper with ASP.NET Web Pages</span></span>
====================
<span data-ttu-id="d02cf-105">podle [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d02cf-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d02cf-106">V tomto tématu a aplikace ukazují, jak přidat do projektu služby WebMatrix 3 Pomocník Twitter.</span><span class="sxs-lookup"><span data-stu-id="d02cf-106">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="d02cf-107">Obsahuje kód Pomocník Twitter a ukazuje, jak volat metodu helper.</span><span class="sxs-lookup"><span data-stu-id="d02cf-107">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="d02cf-108">Tento kód pro soubor Twitter.cshtml vyvinula společnost **Tian Pan** společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="d02cf-108">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d02cf-109">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="d02cf-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="d02cf-110">Webové stránky ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="d02cf-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="d02cf-111">V tomto kurzu se také pracuje s ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="d02cf-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="introduction"></a><span data-ttu-id="d02cf-112">Úvod</span><span class="sxs-lookup"><span data-stu-id="d02cf-112">Introduction</span></span>

<span data-ttu-id="d02cf-113">Toto téma ukazuje, jak přidat Pomocník Twitter pro vaši aplikaci a pomocí syntaxe Razor pro volání metody helper.</span><span class="sxs-lookup"><span data-stu-id="d02cf-113">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="d02cf-114">Pomocník Twitter umožňuje snadno začlenit tlačítka Twitteru a pomůcky ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d02cf-114">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="d02cf-115">Chcete-li pomocí widgetu Twitter, jako je například timeline uživatele nebo výsledky hledání pro hashtagu, musíte nejdřív vytvořit [widgetů na Twitteru](https://twitter.com/settings/widgets).</span><span class="sxs-lookup"><span data-stu-id="d02cf-115">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="d02cf-116">Po vytvoření vaší widgetů, zobrazí se id widgetu. Toto id widgetu předat jako parametr při volání pomocné metody, které ukazují widgetu.</span><span class="sxs-lookup"><span data-stu-id="d02cf-116">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="d02cf-117">Toto téma bylo napsáno pro verzi 1.1 rozhraní Twitter API.</span><span class="sxs-lookup"><span data-stu-id="d02cf-117">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="d02cf-118">Přímo do projektu přidáte kód Pomocník Twitter, můžete aktualizovat kód pomocného objektu, pokud se změní rozhraní Twitter API.</span><span class="sxs-lookup"><span data-stu-id="d02cf-118">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="d02cf-119">Informace o instalaci služby WebMatrix najdete v tématu [Úvod do ASP.NET Web Pages 2 – Začínáme](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="d02cf-119">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="d02cf-120">Pomocník Twitter přidat do projektu</span><span class="sxs-lookup"><span data-stu-id="d02cf-120">Add Twitter Helper to your project</span></span>

<span data-ttu-id="d02cf-121">Chcete-li přidat Pomocník Twitter, nejprve přidejte složku s názvem **aplikace\_kód** do projektu.</span><span class="sxs-lookup"><span data-stu-id="d02cf-121">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="d02cf-122">Vytvořte soubor s názvem **Twitter.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="d02cf-122">Then, create a file named **Twitter.cshtml**.</span></span>

![Složku App_Code](twitter-helper/_static/image1.png)

<span data-ttu-id="d02cf-124">Nahraďte kód v Twitter.cshtml následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="d02cf-124">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="d02cf-125">Volání metody Twitteru z webových stránek</span><span class="sxs-lookup"><span data-stu-id="d02cf-125">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="d02cf-126">Následující příklad ukazuje způsob použití metody Pomocník Twitter ze stránky ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="d02cf-126">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="d02cf-127">Ve vašem projektu můžete k nahrazení hodnoty parametrů s hodnotami, které jsou relevantní pro vaše potřeby.</span><span class="sxs-lookup"><span data-stu-id="d02cf-127">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="d02cf-128">ID zadané widgetu můžete prozkoumat, jak fungují metody, ale budete chtít vytvořit vlastní pomůcky pro váš projekt.</span><span class="sxs-lookup"><span data-stu-id="d02cf-128">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="d02cf-129">Ne všechny parametry uvedené níže jsou požadovány.</span><span class="sxs-lookup"><span data-stu-id="d02cf-129">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="d02cf-130">Volitelné parametry umožňují přizpůsobit, jak se zobrazí tlačítko nebo widgetu.</span><span class="sxs-lookup"><span data-stu-id="d02cf-130">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="d02cf-131">Například použijte tlačítko pouze vyžaduje uživatelské jméno dodržovat, ale tento příklad ukazuje, jak zahrnují počet sledujících a jak určit velikost tlačítka a jazyk.</span><span class="sxs-lookup"><span data-stu-id="d02cf-131">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="d02cf-132">Zobrazit výsledky</span><span class="sxs-lookup"><span data-stu-id="d02cf-132">See the results</span></span>

<span data-ttu-id="d02cf-133">Ve výše uvedeném kódu vytvoří následující tlačítka a pomůcky.</span><span class="sxs-lookup"><span data-stu-id="d02cf-133">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="d02cf-134">Tato tlačítka a pomůcky jsou plně funkční, není snímky obrazovky.</span><span class="sxs-lookup"><span data-stu-id="d02cf-134">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="d02cf-135">Je zobrazeno tlačítko postupujte ve španělštině, protože parametr jazyka byla nastavena na hodnotu **es**.</span><span class="sxs-lookup"><span data-stu-id="d02cf-135">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="d02cf-136">Použijte tlačítko</span><span class="sxs-lookup"><span data-stu-id="d02cf-136">Follow Button</span></span>

<span data-ttu-id="d02cf-137">[Postupujte podle @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="d02cf-137">[Follow @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="tweet-button"></a><span data-ttu-id="d02cf-138">Tlačítko tweetu</span><span class="sxs-lookup"><span data-stu-id="d02cf-138">Tweet Button</span></span>

<span data-ttu-id="d02cf-139">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="d02cf-139">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="user-timeline-profile"></a><span data-ttu-id="d02cf-140">Časová osa uživatele (profil)</span><span class="sxs-lookup"><span data-stu-id="d02cf-140">User Timeline (Profile)</span></span>

<span data-ttu-id="d02cf-141">[Tweety podle @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="d02cf-141">[Tweets by @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="favorites"></a><span data-ttu-id="d02cf-142">Oblíbené položky</span><span class="sxs-lookup"><span data-stu-id="d02cf-142">Favorites</span></span>

<span data-ttu-id="d02cf-143">[Oblíbené Tweety podle @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="d02cf-143">[Favorite Tweets by @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="list"></a><span data-ttu-id="d02cf-144">Seznam</span><span class="sxs-lookup"><span data-stu-id="d02cf-144">List</span></span>

<span data-ttu-id="d02cf-145">[Tweetuje z @Microsoft/MS \_příjemce\_pruhy](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="d02cf-145">[Tweets from @Microsoft/MS\_Consumer\_Bands](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="search"></a><span data-ttu-id="d02cf-146">Hledat</span><span class="sxs-lookup"><span data-stu-id="d02cf-146">Search</span></span>

<span data-ttu-id="d02cf-147">[Tweetuje o &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="d02cf-147">[Tweets about &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>
