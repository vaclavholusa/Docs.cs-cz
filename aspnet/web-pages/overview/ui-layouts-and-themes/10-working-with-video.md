---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: Zobrazení Video v rozhraní ASP.NET Web Pages lokality (Razor) | Microsoft Docs
author: tfitzmac
description: Tato kapitola vysvětluje postup zobrazení videa na webových stránkách ASP.NET stránky syntaxe Razor.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 4e7dc50fb60546d1e1f10a16ed863c0b812ec82b
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/12/2018
ms.locfileid: "29153677"
---
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="021d8-103">Zobrazení Video v Web Pages (Razor) technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="021d8-103">Displaying Video in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="021d8-104">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="021d8-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="021d8-105">Tento článek vysvětluje, jak pokud chcete, aby uživatelům prohlížet videa, které jsou uloženy v lokalitě použijte přehrávač video (médium) na webu technologie ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="021d8-105">This article explains how to use a video (media) player in an ASP.NET Web Pages (Razor) website to let users view videos that are stored on the site.</span></span> <span data-ttu-id="021d8-106">Technologie ASP.NET Web Pages se syntaxí Razor umožňuje hrát Flash (*SWF*), přehrávač médií (*.wmv*) a Silverlight (*.xap*) videa.</span><span class="sxs-lookup"><span data-stu-id="021d8-106">ASP.NET Web Pages with Razor syntax lets you play Flash (*.swf*), Media Player (*.wmv*), and Silverlight (*.xap*) videos.</span></span>
> 
> <span data-ttu-id="021d8-107">Získáte informace:</span><span class="sxs-lookup"><span data-stu-id="021d8-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="021d8-108">Jak vybrat přehrávání videa.</span><span class="sxs-lookup"><span data-stu-id="021d8-108">How to choose a video player.</span></span>
> - <span data-ttu-id="021d8-109">Jak přidat video na webovou stránku.</span><span class="sxs-lookup"><span data-stu-id="021d8-109">How to add video to a web page.</span></span>
> - <span data-ttu-id="021d8-110">Jak nastavit atributy přehrávání videa.</span><span class="sxs-lookup"><span data-stu-id="021d8-110">How to set video player attributes.</span></span>
> 
> <span data-ttu-id="021d8-111">Jedná se o syntaxi ASP.NET Razor stránky v článku nové funkce:</span><span class="sxs-lookup"><span data-stu-id="021d8-111">These are the ASP.NET Razor pages features introduced in the article:</span></span>
> 
> - <span data-ttu-id="021d8-112">`Video` Pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="021d8-112">The `Video` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="021d8-113">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="021d8-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="021d8-114">Rozhraní ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="021d8-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="021d8-115">Služba WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="021d8-115">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="021d8-116">V tomto kurzu taky spolupracuje se službou WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="021d8-116">This tutorial also works with WebMatrix 3.</span></span>


## <a name="introduction"></a><span data-ttu-id="021d8-117">Úvod</span><span class="sxs-lookup"><span data-stu-id="021d8-117">Introduction</span></span>

<span data-ttu-id="021d8-118">Můžete chtít zobrazit video na váš web.</span><span class="sxs-lookup"><span data-stu-id="021d8-118">You might want to display a video on your site.</span></span> <span data-ttu-id="021d8-119">Jedním ze způsobů, které má odkaz k lokalitě, která již na video, jako je YouTube.</span><span class="sxs-lookup"><span data-stu-id="021d8-119">One way to do that is to link to a site that already has the video, like YouTube.</span></span> <span data-ttu-id="021d8-120">Pokud chcete vložit video z těchto webů přímo do vlastní stránky, můžete obvykle získat kód HTML z lokality a zkopírujte jej do stránku.</span><span class="sxs-lookup"><span data-stu-id="021d8-120">If you want to embed a video from these sites directly in your own pages, you can usually get HTML markup from the site and then copy it into your page.</span></span> <span data-ttu-id="021d8-121">Například následující příklad ukazuje, jak pro vložení YouTube video:</span><span class="sxs-lookup"><span data-stu-id="021d8-121">For example, the following example shows how to embed a YouTube video:</span></span>

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

<span data-ttu-id="021d8-122">Pokud chcete přehrát video, které je na vlastní web (nikoli na veřejný web sdílení video), nelze přímo propojit se pomocí vložených značek takto.</span><span class="sxs-lookup"><span data-stu-id="021d8-122">If you want to play a video that's on your own website (not on a public video-sharing site), you can't directly link to it using embedded markup like this.</span></span> <span data-ttu-id="021d8-123">Ale přehrávání videa z webu pomocí `Video` pomocné rutiny, která vykreslí přehrávač médií přímo v stránky.</span><span class="sxs-lookup"><span data-stu-id="021d8-123">However, you can play videos from your site by using the `Video` helper, which renders a media player directly in a page.</span></span>

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a><span data-ttu-id="021d8-124">Výběr přehrávání videa</span><span class="sxs-lookup"><span data-stu-id="021d8-124">Choosing a Video Player</span></span>

<span data-ttu-id="021d8-125">Mnoho z formátů pro videosoubory a každý formát obvykle vyžaduje jiný přehrávač a jiný způsob, jak nakonfigurovat přehrávač.</span><span class="sxs-lookup"><span data-stu-id="021d8-125">There are lots of formats for video files, and each format typically requires a different player and a different way to configure the player.</span></span> <span data-ttu-id="021d8-126">Na stránkách ASP.NET Razor můžete přehrát video webovou stránku pomocí `Video` pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="021d8-126">In ASP.NET Razor pages, you can play a video in a web page using the `Video` helper.</span></span> <span data-ttu-id="021d8-127">`Video` Pomocná zjednodušuje proces vložení videa na webové stránce, protože automaticky vygeneruje `object` a `embed` elementy HTML, které se obvykle používají k přidat video na stránku.</span><span class="sxs-lookup"><span data-stu-id="021d8-127">The `Video` helper simplifies the process of embedding videos in a web page because it automatically generates the `object` and `embed` HTML elements that are normally used to add video to the page.</span></span>

<span data-ttu-id="021d8-128">`Video` Pomocná podporuje následující přehrávače médií:</span><span class="sxs-lookup"><span data-stu-id="021d8-128">The `Video` helper supports the following media players:</span></span>

- <span data-ttu-id="021d8-129">Adobe Flash</span><span class="sxs-lookup"><span data-stu-id="021d8-129">Adobe Flash</span></span>
- <span data-ttu-id="021d8-130">Windows Media Player</span><span class="sxs-lookup"><span data-stu-id="021d8-130">Windows MediaPlayer</span></span>
- <span data-ttu-id="021d8-131">Microsoft Silverlight</span><span class="sxs-lookup"><span data-stu-id="021d8-131">Microsoft Silverlight</span></span>

### <a name="the-flash-player"></a><span data-ttu-id="021d8-132">Flash Player</span><span class="sxs-lookup"><span data-stu-id="021d8-132">The Flash Player</span></span>

<span data-ttu-id="021d8-133">`Flash` Přehrávač dle `Video` pomocné rutiny umožňují přehrávání videa Flash (*SWF* soubory) na webové stránce.</span><span class="sxs-lookup"><span data-stu-id="021d8-133">The `Flash` player of the `Video` helper let you play Flash videos (*.swf* files) in a web page.</span></span> <span data-ttu-id="021d8-134">Minimálně je třeba zadat cestu k souboru videa.</span><span class="sxs-lookup"><span data-stu-id="021d8-134">At a minimum, you have to provide a path to the video file.</span></span> <span data-ttu-id="021d8-135">Pokud zadáte nic ale cestu, přehrávač používá výchozí hodnoty, které jsou nastavené jako aktuální verze Flash.</span><span class="sxs-lookup"><span data-stu-id="021d8-135">If you specify nothing but the path, the player uses default values that are set by the current version of Flash.</span></span> <span data-ttu-id="021d8-136">Typický výchozí nastavení je:</span><span class="sxs-lookup"><span data-stu-id="021d8-136">Typical default settings are:</span></span>

- <span data-ttu-id="021d8-137">Přehrávání videa se zobrazí pomocí jeho výchozí šířky a výšky a bez barvu pozadí.</span><span class="sxs-lookup"><span data-stu-id="021d8-137">The video is displayed using its default width and height and without a background color.</span></span>
- <span data-ttu-id="021d8-138">Video hraje automaticky při načtení stránky.</span><span class="sxs-lookup"><span data-stu-id="021d8-138">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="021d8-139">Video opakovací nepřetržitě explicitně je zastavena.</span><span class="sxs-lookup"><span data-stu-id="021d8-139">The video loops continuously until it's explicitly stopped.</span></span>
- <span data-ttu-id="021d8-140">Video je škálovat zobrazíte všechna videa, nikoli oříznutí video podle konkrétní velikost.</span><span class="sxs-lookup"><span data-stu-id="021d8-140">The video is scaled to show all of the video, rather than cropping the video to fit a specific size.</span></span>
- <span data-ttu-id="021d8-141">Přehrávání videa v okně.</span><span class="sxs-lookup"><span data-stu-id="021d8-141">The video plays in a window.</span></span>

### <a name="the-mediaplayer-player"></a><span data-ttu-id="021d8-142">Přehrávač Media Player</span><span class="sxs-lookup"><span data-stu-id="021d8-142">The MediaPlayer Player</span></span>

<span data-ttu-id="021d8-143">`MediaPlayer` Přehrávač dle `Video` Pomocník umožňuje přehrávání videa Windows Media (*.wmv* soubory), Windows Media zvuk (*WMA* soubory) a MP3 (*MP3* soubory) na webové stránce.</span><span class="sxs-lookup"><span data-stu-id="021d8-143">The `MediaPlayer` player of the `Video` helper lets you play Windows Media videos (*.wmv* files), Windows Media audio (*.wma* files), and MP3 (*.mp3* files) in a web page.</span></span> <span data-ttu-id="021d8-144">Musí zahrnovat cestu k souboru média k přehrání; všechny ostatní parametry jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="021d8-144">You must include path of the media file to play; all other parameters are optional.</span></span> <span data-ttu-id="021d8-145">Pokud zadáte pouze cestu, přehrávač používá výchozí nastavení, nastavte pomocí aktuální verze Media Player, jako je třeba:</span><span class="sxs-lookup"><span data-stu-id="021d8-145">If you specify only a path, the player uses default settings set by the current version of MediaPlayer, such as:</span></span>

- <span data-ttu-id="021d8-146">Zobrazí se na video, pomocí jeho výchozí šířka a výška.</span><span class="sxs-lookup"><span data-stu-id="021d8-146">The video is displayed using its default width and height.</span></span>
- <span data-ttu-id="021d8-147">Video hraje automaticky při načtení stránky.</span><span class="sxs-lookup"><span data-stu-id="021d8-147">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="021d8-148">Video přehraje jednou (ho nebude cykly).</span><span class="sxs-lookup"><span data-stu-id="021d8-148">The video plays once (it doesn't loop).</span></span>
- <span data-ttu-id="021d8-149">Přehrávač zobrazí úplnou sadu ovládacích prvků v uživatelském rozhraní.</span><span class="sxs-lookup"><span data-stu-id="021d8-149">The player displays the full set of controls in the user interface.</span></span>
- <span data-ttu-id="021d8-150">Přehrávání videa v okně.</span><span class="sxs-lookup"><span data-stu-id="021d8-150">The video plays in a window.</span></span>

### <a name="the-silverlight-player"></a><span data-ttu-id="021d8-151">Přehrávač Silverlight</span><span class="sxs-lookup"><span data-stu-id="021d8-151">The Silverlight Player</span></span>

<span data-ttu-id="021d8-152">`Silverlight` Přehrávač dle `Video` Pomocník umožňuje přehrát Video média systému Windows (*.wmv* soubory), Windows Media Audio (*WMA* soubory) a MP3 (*MP3* soubory).</span><span class="sxs-lookup"><span data-stu-id="021d8-152">The `Silverlight` player of the `Video` helper lets you play Windows Media Video (*.wmv* files), Windows Media Audio (*.wma* files), and MP3 (*.mp3* files).</span></span> <span data-ttu-id="021d8-153">Je nutné nastavit parametr path tak, aby odkazoval na balíček aplikace založená na technologii Silverlight (*.xap* souboru).</span><span class="sxs-lookup"><span data-stu-id="021d8-153">You must set the path parameter to point to a Silverlight-based application package (*.xap* file).</span></span> <span data-ttu-id="021d8-154">Je třeba také nastavit parametry šířky a výšky.</span><span class="sxs-lookup"><span data-stu-id="021d8-154">You also must set the width and height parameters.</span></span> <span data-ttu-id="021d8-155">Všechny ostatní parametry jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="021d8-155">All other parameters are optional.</span></span> <span data-ttu-id="021d8-156">Při použití Silverlight player pro video, pokud jste nastavili pouze požadované parametry, zobrazí přehrávač Silverlight video bez barvu pozadí.</span><span class="sxs-lookup"><span data-stu-id="021d8-156">When you use the Silverlight player for video, if you set only the required parameters, the Silverlight player displays the video without a background color.</span></span>

> [!NOTE]
> <span data-ttu-id="021d8-157">V případě, že již neznáte Silverlight: *.xap* soubor je komprimovaný soubor, který obsahuje rozložení pokyny v *XAML* souboru spravovaným kódem v sestavení a volitelné prostředky.</span><span class="sxs-lookup"><span data-stu-id="021d8-157">In case you don't already know Silverlight: the *.xap* file is a compressed file that contains layout instructions in a *.xaml* file, managed code in assemblies, and optional resources.</span></span> <span data-ttu-id="021d8-158">Můžete vytvořit *.xap* souborů v sadě Visual Studio jako projekt aplikace Silverlight.</span><span class="sxs-lookup"><span data-stu-id="021d8-158">You can create a *.xap* file in Visual Studio as a Silverlight application project.</span></span>


<span data-ttu-id="021d8-159">`Silverlight` Přehrávání videa používá jak nastavení, které zadáte pro přehrávač a nastavení, které jsou součástí *.xap* souboru.</span><span class="sxs-lookup"><span data-stu-id="021d8-159">The `Silverlight` video player uses both the settings that you provide for the player and the settings that are provided in the *.xap* file.</span></span>

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a><span data-ttu-id="021d8-160">Typy MIME</span><span class="sxs-lookup"><span data-stu-id="021d8-160">MIME Types</span></span>
> 
> <span data-ttu-id="021d8-161">Pokud prohlížeč stáhne soubor, v prohlížeči zajišťuje, že typ souboru odpovídá typ MIME, který je zadán pro dokument, který je vykreslován.</span><span class="sxs-lookup"><span data-stu-id="021d8-161">When a browser downloads a file, the browser makes sure that the file type matches the MIME type that's specified for the document that's being rendered.</span></span> <span data-ttu-id="021d8-162">Typ MIME je typ obsahu typu nebo médium souboru.</span><span class="sxs-lookup"><span data-stu-id="021d8-162">The MIME type is the content type or media type of a file.</span></span> <span data-ttu-id="021d8-163">`Video` Pomocné rutiny používá následující typy MIME:</span><span class="sxs-lookup"><span data-stu-id="021d8-163">The `Video` helper uses the following MIME types:</span></span>
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a><span data-ttu-id="021d8-164">Přehrávání videa Flash (SWF)</span><span class="sxs-lookup"><span data-stu-id="021d8-164">Playing Flash (.swf) Videos</span></span>

<span data-ttu-id="021d8-165">Tento postup ukazuje, jak k přehrávání videa Flash s názvem *sample.swf*.</span><span class="sxs-lookup"><span data-stu-id="021d8-165">This procedure shows you how to play a Flash video named *sample.swf*.</span></span> <span data-ttu-id="021d8-166">Postupu se předpokládá, že máte k dispozici složku s názvem *média* na váš web a který *SWF* soubor je v této složce.</span><span class="sxs-lookup"><span data-stu-id="021d8-166">The procedure assumes that you've got a folder named *Media* on your site and that the *.swf* file is in that folder.</span></span>

1. <span data-ttu-id="021d8-167">Přidejte knihovnu ASP.NET Web Helpers na váš web, jak je popsáno v [instalaci pomocné rutiny v stránku ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), pokud jste to ještě už přidali.</span><span class="sxs-lookup"><span data-stu-id="021d8-167">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="021d8-168">Na webu, přidat stránku a pojmenujte ji *FlashVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="021d8-168">In the website, add a page and name it *FlashVideo.cshtml*.</span></span>
3. <span data-ttu-id="021d8-169">Přidejte následující kód na stránku:</span><span class="sxs-lookup"><span data-stu-id="021d8-169">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. <span data-ttu-id="021d8-170">Spusťte stránku v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="021d8-170">Run the page in a browser.</span></span> <span data-ttu-id="021d8-171">(Ujistěte se, že je vybraný stránky v **soubory** pracovního prostoru, než ji spustit.) Stránka se zobrazí a video hraje automaticky.</span><span class="sxs-lookup"><span data-stu-id="021d8-171">(Make sure the page is selected in the **Files** workspace before you run it.) The page is displayed and the video plays automatically.</span></span> 

    <span data-ttu-id="021d8-172">![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span><span class="sxs-lookup"><span data-stu-id="021d8-172">![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span></span>

<span data-ttu-id="021d8-173">Můžete nastavit `quality` parametr Flash Video k `low`, `autolow`, `autohigh`, `medium`, `high`, a `best`:</span><span class="sxs-lookup"><span data-stu-id="021d8-173">You can set the `quality` parameter for a Flash video to `low`, `autolow`, `autohigh`, `medium`, `high`, and `best`:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

<span data-ttu-id="021d8-174">Můžete změnit na Flash video přehrávání na konkrétní velikost pomocí `scale` parametr, který můžete nastavit na následující:</span><span class="sxs-lookup"><span data-stu-id="021d8-174">You can change the Flash video to play at a specific size using the `scale` parameter, which you can set to the following:</span></span>

- <span data-ttu-id="021d8-175">`showall`.</span><span class="sxs-lookup"><span data-stu-id="021d8-175">`showall`.</span></span> <span data-ttu-id="021d8-176">To zviditelní celé video původního poměru stran obrázku je zachován.</span><span class="sxs-lookup"><span data-stu-id="021d8-176">This makes the entire video visible while maintaining the original aspect ratio.</span></span> <span data-ttu-id="021d8-177">Však může skončili s ohraničení na každé straně.</span><span class="sxs-lookup"><span data-stu-id="021d8-177">However, you might end up with borders on each side.</span></span>
- <span data-ttu-id="021d8-178">`noorder`.</span><span class="sxs-lookup"><span data-stu-id="021d8-178">`noorder`.</span></span> <span data-ttu-id="021d8-179">Toto video škáluje původního poměru stran obrázku je zachován, ale může být oříznuty.</span><span class="sxs-lookup"><span data-stu-id="021d8-179">This scales the video while maintaining the original aspect ratio, but it might be cropped.</span></span>
- <span data-ttu-id="021d8-180">`exactfit`.</span><span class="sxs-lookup"><span data-stu-id="021d8-180">`exactfit`.</span></span> <span data-ttu-id="021d8-181">Díky tomu celé video viditelné bez zachování původní poměr stran, ale může dojít k narušení.</span><span class="sxs-lookup"><span data-stu-id="021d8-181">This makes the entire video visible without preserving the original aspect ratio, but distortion may occur.</span></span>

<span data-ttu-id="021d8-182">Pokud nezadáte `scale` parametr celé video budou viditelné, bez jakékoli oříznutí se zachová původní poměr stran.</span><span class="sxs-lookup"><span data-stu-id="021d8-182">If you don't specify a `scale` parameter, the entire video will be visible and the original aspect ratio will be maintained without any cropping.</span></span> <span data-ttu-id="021d8-183">Následující příklad ukazuje způsob použití `scale` parametr:</span><span class="sxs-lookup"><span data-stu-id="021d8-183">The following example shows how to use the `scale` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

<span data-ttu-id="021d8-184">Flash player podporuje video režim nastavení s názvem `windowMode`.</span><span class="sxs-lookup"><span data-stu-id="021d8-184">The Flash player supports a video mode setting named `windowMode`.</span></span> <span data-ttu-id="021d8-185">Můžete nastavit na `window`, `opaque`, a `transparent`.</span><span class="sxs-lookup"><span data-stu-id="021d8-185">You can set this to `window`, `opaque`, and `transparent`.</span></span> <span data-ttu-id="021d8-186">Ve výchozím nastavení `windowMode` je nastaven na `window`, který zobrazuje videa v samostatném okně na webové stránce.</span><span class="sxs-lookup"><span data-stu-id="021d8-186">By default, the `windowMode` is set to `window`, which displays the video in a separate window on the web page.</span></span> <span data-ttu-id="021d8-187">`opaque` Nastavení skryje vše za video na webové stránce.</span><span class="sxs-lookup"><span data-stu-id="021d8-187">The `opaque` setting hides everything behind the video on the web page.</span></span> <span data-ttu-id="021d8-188">`transparent` Nastavení umožňuje na pozadí webové stránky, zobrazit prostřednictvím video, za předpokladu, že je transparentní, jakékoliv části videa.</span><span class="sxs-lookup"><span data-stu-id="021d8-188">The `transparent` setting lets the background of the web page show through the video, assuming any part of the video is transparent.</span></span>

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a><span data-ttu-id="021d8-189">Přehrávání Media Player (*.wmv*) videa</span><span class="sxs-lookup"><span data-stu-id="021d8-189">Playing MediaPlayer (*.wmv*) Videos</span></span>

<span data-ttu-id="021d8-190">Následující postup ukazuje, jak přehrát video média okno s názvem *sample.wmv* který se v *média* složky.</span><span class="sxs-lookup"><span data-stu-id="021d8-190">The following procedure shows you how to play a Window Media video named *sample.wmv* that's in the *Media* folder.</span></span>

1. <span data-ttu-id="021d8-191">Přidejte knihovnu ASP.NET Web Helpers na váš web, jak je popsáno v [instalaci pomocné rutiny v stránku ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), pokud jste tak ještě neučinili.</span><span class="sxs-lookup"><span data-stu-id="021d8-191">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="021d8-192">Vytvořit novou stránku s názvem *MediaPlayerVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="021d8-192">Create a new page named *MediaPlayerVideo.cshtml*.</span></span>
3. <span data-ttu-id="021d8-193">Přidejte následující kód na stránku:</span><span class="sxs-lookup"><span data-stu-id="021d8-193">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. <span data-ttu-id="021d8-194">Spusťte stránku v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="021d8-194">Run the page in a browser.</span></span> <span data-ttu-id="021d8-195">Video načte a hraje automaticky.</span><span class="sxs-lookup"><span data-stu-id="021d8-195">The video loads and plays automatically.</span></span> 

    <span data-ttu-id="021d8-196">![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span><span class="sxs-lookup"><span data-stu-id="021d8-196">![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span></span>

<span data-ttu-id="021d8-197">Můžete nastavit `playCount` na celé číslo, která určuje, jak často má přehrát video, automaticky:</span><span class="sxs-lookup"><span data-stu-id="021d8-197">You can set `playCount` to an integer that indicates how many times to play the video automatically:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

<span data-ttu-id="021d8-198">`uiMode` Parametr slouží k určení ovládacích prvků, které zobrazí v uživatelském rozhraní.</span><span class="sxs-lookup"><span data-stu-id="021d8-198">The `uiMode` parameter lets you specify which controls show up in the user interface.</span></span> <span data-ttu-id="021d8-199">Můžete nastavit `uiMode` k `invisible`, `none`, `mini`, nebo `full`.</span><span class="sxs-lookup"><span data-stu-id="021d8-199">You can set `uiMode` to `invisible`, `none`, `mini`, or `full`.</span></span> <span data-ttu-id="021d8-200">Pokud nezadáte `uiMode` parametr, bude se na video zobrazí se stavové okno, Hledat panelu, řídit tlačítek a ovládacích prvků svazku kromě okně videa.</span><span class="sxs-lookup"><span data-stu-id="021d8-200">If you don't specify a `uiMode` parameter, the video will be displayed with the status window, seek bar, control buttons, and volume controls in addition to the video window.</span></span> <span data-ttu-id="021d8-201">Tyto ovládací prvky se zobrazí také v případě použití přehrávače pro přehrání zvukového souboru.</span><span class="sxs-lookup"><span data-stu-id="021d8-201">These controls will also be displayed if you use the player to play an audio file.</span></span> <span data-ttu-id="021d8-202">Tady je příklad použití `uiMode` parametr:</span><span class="sxs-lookup"><span data-stu-id="021d8-202">Here's an example of how to use the `uiMode` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

<span data-ttu-id="021d8-203">Ve výchozím nastavení je zvuk na při přehrávání videa.</span><span class="sxs-lookup"><span data-stu-id="021d8-203">By default, audio is on when the video plays.</span></span> <span data-ttu-id="021d8-204">Zvuk můžete ztlumení nastavením `mute` parametr na hodnotu true:</span><span class="sxs-lookup"><span data-stu-id="021d8-204">You can mute the audio by setting the `mute` parameter to true:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

<span data-ttu-id="021d8-205">Můžete řídit úroveň audio video Media Player nastavením `volume` parametr na hodnotu mezi 0 a 100.</span><span class="sxs-lookup"><span data-stu-id="021d8-205">You can control the audio level of the MediaPlayer video by setting the `volume` parameter to a value between 0 and 100.</span></span> <span data-ttu-id="021d8-206">Výchozí hodnota je 50.</span><span class="sxs-lookup"><span data-stu-id="021d8-206">The default value is 50.</span></span> <span data-ttu-id="021d8-207">Tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="021d8-207">Here's an example:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a><span data-ttu-id="021d8-208">Přehrávání videa technologie Silverlight</span><span class="sxs-lookup"><span data-stu-id="021d8-208">Playing Silverlight Videos</span></span>

<span data-ttu-id="021d8-209">Tento postup ukazuje, jak přehrát video, které jsou součástí Silverlight *.xap* stránky, která je ve složce s názvem *média*.</span><span class="sxs-lookup"><span data-stu-id="021d8-209">This procedure shows you how to play video contained in a Silverlight *.xap* page that's in a folder named *Media*.</span></span>

1. <span data-ttu-id="021d8-210">Přidejte knihovnu ASP.NET Web Helpers na váš web, jak je popsáno v [instalaci pomocné rutiny v stránku ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), pokud jste tak ještě neučinili.</span><span class="sxs-lookup"><span data-stu-id="021d8-210">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already .</span></span>
2. <span data-ttu-id="021d8-211">Vytvořit novou stránku s názvem *SilverlightVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="021d8-211">Create a new page named *SilverlightVideo.cshtml*.</span></span>
3. <span data-ttu-id="021d8-212">Přidejte následující kód na stránku:</span><span class="sxs-lookup"><span data-stu-id="021d8-212">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. <span data-ttu-id="021d8-213">Spusťte stránku v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="021d8-213">Run the page in a browser.</span></span> 

    <span data-ttu-id="021d8-214">![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span><span class="sxs-lookup"><span data-stu-id="021d8-214">![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="021d8-215">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="021d8-215">Additional Resources</span></span>


<span data-ttu-id="021d8-216">[Přehled technologie Silverlight](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span><span class="sxs-lookup"><span data-stu-id="021d8-216">[Silverlight Overview](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span></span>

[<span data-ttu-id="021d8-217">Flash OBJEKT a vložení atributů značky</span><span class="sxs-lookup"><span data-stu-id="021d8-217">Flash OBJECT and EMBED tag attributes</span></span>](http://kb2.adobe.com/cps/127/tn_12701.html)

<span data-ttu-id="021d8-218">[Windows Media Player 11 SDK PARAM značky](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="021d8-218">[Windows Media Player 11 SDK PARAM Tags](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span></span>
