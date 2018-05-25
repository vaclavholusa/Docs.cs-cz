---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Pomocník s webovými stránkami ASP.NET Twitter | Microsoft Docs
author: tfitzmac
description: Toto téma a aplikace ukazují, jak přidat do projektu služby WebMatrix 3 pomocné rutiny služby Twitter. Obsahuje kód Pomocník Twitter a ukazuje způsob volání pomocné rutiny...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 07d9c4d485c42b78a42c54c9740af5f67cb44763
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/25/2018
---
<a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="dddbb-104">Pomocník s webovými stránkami ASP.NET Twitter</span><span class="sxs-lookup"><span data-stu-id="dddbb-104">Twitter Helper with ASP.NET Web Pages</span></span>
====================
<span data-ttu-id="dddbb-105">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="dddbb-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="dddbb-106">Toto téma a aplikace ukazují, jak přidat do projektu služby WebMatrix 3 pomocné rutiny služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="dddbb-106">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="dddbb-107">Obsahuje kód Pomocník Twitter a ukazuje způsob volání pomocné metody.</span><span class="sxs-lookup"><span data-stu-id="dddbb-107">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="dddbb-108">Tento kód pro soubor Twitter.cshtml vyvinula **Tian Panoramování** společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="dddbb-108">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="dddbb-109">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="dddbb-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="dddbb-110">Rozhraní ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="dddbb-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="dddbb-111">V tomto kurzu taky spolupracuje se službou ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="dddbb-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="introduction"></a><span data-ttu-id="dddbb-112">Úvod</span><span class="sxs-lookup"><span data-stu-id="dddbb-112">Introduction</span></span>

<span data-ttu-id="dddbb-113">Toto téma ukazuje, jak přidat Twitter pomocné rutiny do vaší aplikace a používat syntaxi Razor k volání pomocné metody.</span><span class="sxs-lookup"><span data-stu-id="dddbb-113">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="dddbb-114">Pomocník Twitter usnadňuje začlenění Twitter tlačítka a pomůcky ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="dddbb-114">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="dddbb-115">Chcete-li použít modul widget Twitter, jako je například časová osa uživatele nebo výsledky hledání hashtagu, musíte nejdřív vytvořit [pomůcky na Twitteru](https://twitter.com/settings/widgets).</span><span class="sxs-lookup"><span data-stu-id="dddbb-115">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="dddbb-116">Po vytvoření vaší pomůcky, obdržíte id pomůcky. Toto id pomůcky předejte jako parametr při volání pomocné metody, které se zobrazí pomůcky.</span><span class="sxs-lookup"><span data-stu-id="dddbb-116">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="dddbb-117">Toto téma je určen pro verze 1.1 rozhraní API služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="dddbb-117">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="dddbb-118">Přidáním přímo kód Pomocník Twitter do projektu, můžete aktualizovat kód pomocného objektu, pokud se změní rozhraní API služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="dddbb-118">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="dddbb-119">Informace o instalaci služby WebMatrix najdete v tématu [Představení technologie ASP.NET Web Pages 2 – Začínáme](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="dddbb-119">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="dddbb-120">Do projektu přidejte Pomocník Twitter</span><span class="sxs-lookup"><span data-stu-id="dddbb-120">Add Twitter Helper to your project</span></span>

<span data-ttu-id="dddbb-121">Pokud chcete přidat Pomocník Twitter, nejprve přidejte složku s názvem **aplikace\_kód** do projektu.</span><span class="sxs-lookup"><span data-stu-id="dddbb-121">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="dddbb-122">Pak vytvořte soubor s názvem **Twitter.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="dddbb-122">Then, create a file named **Twitter.cshtml**.</span></span>

![Složka App_Code](twitter-helper/_static/image1.png)

<span data-ttu-id="dddbb-124">Ve výchozím kódu v Twitter.cshtml nahraďte následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="dddbb-124">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="dddbb-125">Volání metody Twitter z webových stránek</span><span class="sxs-lookup"><span data-stu-id="dddbb-125">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="dddbb-126">Následující příklad ukazuje, jak používat služby Twitter pomocné metody ze stránky ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="dddbb-126">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="dddbb-127">V projektu můžete k nahrazení hodnoty parametrů s hodnotami, které jsou relevantní pro vaše potřeby.</span><span class="sxs-lookup"><span data-stu-id="dddbb-127">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="dddbb-128">ID zadané pomůcky můžete prozkoumat fungování metody, ale budete chtít vygenerovat vlastní pomůcky pro váš projekt.</span><span class="sxs-lookup"><span data-stu-id="dddbb-128">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="dddbb-129">Ne všechny následující parametry jsou povinné.</span><span class="sxs-lookup"><span data-stu-id="dddbb-129">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="dddbb-130">Volitelné parametry slouží k přizpůsobení zobrazení tlačítka nebo pomůcky.</span><span class="sxs-lookup"><span data-stu-id="dddbb-130">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="dddbb-131">Například použijte tlačítko pouze vyžaduje uživatelské jméno pro postupujte podle, ale příklad ukazuje, jak chcete zahrnout počet délky a jak určit velikost tlačítko a jazyk.</span><span class="sxs-lookup"><span data-stu-id="dddbb-131">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="dddbb-132">Zobrazit výsledky</span><span class="sxs-lookup"><span data-stu-id="dddbb-132">See the results</span></span>

<span data-ttu-id="dddbb-133">Výše uvedený kód vytvoří následující tlačítka a pomůcky.</span><span class="sxs-lookup"><span data-stu-id="dddbb-133">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="dddbb-134">Tato tlačítka a pomůcky jsou plně funkční, není snímky obrazovky.</span><span class="sxs-lookup"><span data-stu-id="dddbb-134">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="dddbb-135">Použijte tlačítko se zobrazí ve španělštině, protože parametr language byla nastavena na **es**.</span><span class="sxs-lookup"><span data-stu-id="dddbb-135">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="dddbb-136">Použijte tlačítko</span><span class="sxs-lookup"><span data-stu-id="dddbb-136">Follow Button</span></span>

[<span data-ttu-id="dddbb-137">Postupujte podle @aspnet)</span><span class="sxs-lookup"><span data-stu-id="dddbb-137">Follow @aspnet)</span></span>](https://twitter.com/aspnet)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>

### <a name="tweet-button"></a><span data-ttu-id="dddbb-138">Tlačítko tweet</span><span class="sxs-lookup"><span data-stu-id="dddbb-138">Tweet Button</span></span>

[<span data-ttu-id="dddbb-139">Tweet</span><span class="sxs-lookup"><span data-stu-id="dddbb-139">Tweet</span></span>](https://twitter.com/share)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>

### <a name="user-timeline-profile"></a><span data-ttu-id="dddbb-140">Časová osa uživatele (profil)</span><span class="sxs-lookup"><span data-stu-id="dddbb-140">User Timeline (Profile)</span></span>

[<span data-ttu-id="dddbb-141">Tweetů podle @aspnet</span><span class="sxs-lookup"><span data-stu-id="dddbb-141">Tweets by @aspnet</span></span>](https://twitter.com/aspnet)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>

### <a name="favorites"></a><span data-ttu-id="dddbb-142">Oblíbené položky</span><span class="sxs-lookup"><span data-stu-id="dddbb-142">Favorites</span></span>

[<span data-ttu-id="dddbb-143">Pomocí Oblíbené Tweetů @Microsoft</span><span class="sxs-lookup"><span data-stu-id="dddbb-143">Favorite Tweets by @Microsoft</span></span>](https://twitter.com/Microsoft/favorites)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>

### <a name="list"></a><span data-ttu-id="dddbb-144">Seznam</span><span class="sxs-lookup"><span data-stu-id="dddbb-144">List</span></span>

[<span data-ttu-id="dddbb-145">Tweetů z @Microsoft/MS \_příjemce\_pruhy</span><span class="sxs-lookup"><span data-stu-id="dddbb-145">Tweets from @Microsoft/MS\_Consumer\_Bands</span></span>](https://twitter.com/microsoft/ms-consumer-brands/)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>

### <a name="search"></a><span data-ttu-id="dddbb-146">Hledat</span><span class="sxs-lookup"><span data-stu-id="dddbb-146">Search</span></span>

[<span data-ttu-id="dddbb-147">Tweety o &quot;#asp.net&quot;</span><span class="sxs-lookup"><span data-stu-id="dddbb-147">Tweets about &quot;#asp.net&quot;</span></span>](https://twitter.com/search?q=%23asp.net)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>
