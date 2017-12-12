---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: "Zobrazení mapy v rozhraní ASP.NET Web Pages lokality (Razor) | Microsoft Docs"
author: tfitzmac
description: "Tento článek vysvětluje, jak zobrazit interaktivní mapy na stránkách na webu technologie ASP.NET Web Pages (Razor) podle mapování služeb poskytovaných Bing, Google, Ma..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: a802e4fed81fadca195c8aa83c37c7100ac5cbec
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="55b53-103">Zobrazení mapy v Web Pages (Razor) technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="55b53-103">Displaying Maps in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="55b53-104">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="55b53-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="55b53-105">Tento článek vysvětluje, jak zobrazit interaktivní mapy na stránkách na webu technologie ASP.NET Web Pages (Razor) podle mapování služeb poskytovaných Bing, Google, MapQuest a Yahoo.</span><span class="sxs-lookup"><span data-stu-id="55b53-105">This article explains how to display interactive maps on pages in an ASP.NET Web Pages (Razor) website based on mapping services provided by Bing, Google, MapQuest, and Yahoo.</span></span>
> 
> <span data-ttu-id="55b53-106">Získáte informace:</span><span class="sxs-lookup"><span data-stu-id="55b53-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="55b53-107">Popisuje, jak Generovat mapu založené na adresu.</span><span class="sxs-lookup"><span data-stu-id="55b53-107">How to generate a map based on an address.</span></span>
> - <span data-ttu-id="55b53-108">Popisuje, jak Generovat mapu podle zeměpisné šířky a délky.</span><span class="sxs-lookup"><span data-stu-id="55b53-108">How to generate a map based on latitude and longitude coordinates.</span></span>
> - <span data-ttu-id="55b53-109">Jak zaregistrovat účet služby Bing Maps Developer a získat klíč pro použití s mapy Bing.</span><span class="sxs-lookup"><span data-stu-id="55b53-109">How to register a Bing Maps Developer Account and get a key to use with Bing Maps.</span></span>
> 
> <span data-ttu-id="55b53-110">Toto je funkce technologie ASP.NET byla zavedená v článku:</span><span class="sxs-lookup"><span data-stu-id="55b53-110">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="55b53-111">`Maps` Pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="55b53-111">The `Maps` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="55b53-112">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="55b53-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="55b53-113">Rozhraní ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="55b53-113">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="55b53-114">Služba WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="55b53-114">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="55b53-115">V tomto kurzu taky spolupracuje se službou WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="55b53-115">This tutorial also works with WebMatrix 3.</span></span>


<span data-ttu-id="55b53-116">Ve webových stránkách, můžete zobrazení mapy na stránce pomocí `Maps` pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="55b53-116">In Web Pages, you can display maps on a page by using `Maps` helper.</span></span> <span data-ttu-id="55b53-117">Můžete vygenerovat založené na adresu nebo na sadu zeměpisné šířky a délky souřadnice mapy.</span><span class="sxs-lookup"><span data-stu-id="55b53-117">You can generate maps based either on an address or on a set of longitude and latitude coordinates.</span></span> <span data-ttu-id="55b53-118">`Maps` Třída umožňuje volání do oblíbených mapy moduly včetně Bing, Google, MapQuest a Yahoo.</span><span class="sxs-lookup"><span data-stu-id="55b53-118">The `Maps` class lets you call into popular map engines including Bing, Google, MapQuest, and Yahoo.</span></span>

<span data-ttu-id="55b53-119">Postup přidání mapování na stránku jsou stejné bez ohledu na to, které z modulů mapy volání.</span><span class="sxs-lookup"><span data-stu-id="55b53-119">The steps for adding mapping to a page are the same regardless of which of the map engines you call.</span></span> <span data-ttu-id="55b53-120">Stačí přidat odkaz na soubor JavaScript, který umožňuje dostupné metody pro zobrazení mapy, a pak volání metody `Maps` pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="55b53-120">You just add a JavaScript file reference that makes available methods to display the map, and then you call methods of the `Maps` helper.</span></span>

<span data-ttu-id="55b53-121">Vybrat službu mapy závislosti na němž `Maps` Pomocná metoda, která používáte.</span><span class="sxs-lookup"><span data-stu-id="55b53-121">You choose a map service based on which `Maps` helper method you use.</span></span> <span data-ttu-id="55b53-122">Můžete použít některou z těchto:</span><span class="sxs-lookup"><span data-stu-id="55b53-122">You can use any of these:</span></span>

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a><span data-ttu-id="55b53-123">Instalace součásti, které potřebujete</span><span class="sxs-lookup"><span data-stu-id="55b53-123">Installing the Pieces You Need</span></span>

<span data-ttu-id="55b53-124">Chcete-li zobrazit mapy, je třeba tyto údaje:</span><span class="sxs-lookup"><span data-stu-id="55b53-124">To display maps, you need these pieces:</span></span>

- <span data-ttu-id="55b53-125">`Maps` Pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="55b53-125">The `Maps` helper.</span></span> <span data-ttu-id="55b53-126">Verze 2 knihovnu ASP.NET Web Helpers se tohoto pomocníka.</span><span class="sxs-lookup"><span data-stu-id="55b53-126">This helper is in version 2 of the ASP.NET Web Helpers Library.</span></span> <span data-ttu-id="55b53-127">Pokud jste ještě nepřidali knihovny, můžete ho nainstalovat ve vaší lokalitě jako balíčku NuGet.</span><span class="sxs-lookup"><span data-stu-id="55b53-127">If you haven't already added the library, you can install it in your site as a NuGet package.</span></span> <span data-ttu-id="55b53-128">Podrobnosti najdete v tématu [instalaci pomocné rutiny v stránku ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372).</span><span class="sxs-lookup"><span data-stu-id="55b53-128">For details, see [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372).</span></span> <span data-ttu-id="55b53-129">(V galerii, vyhledejte `microsoft-web-helpers` balíčku.)</span><span class="sxs-lookup"><span data-stu-id="55b53-129">(In the Gallery, search for the `microsoft-web-helpers` package.)</span></span>
- <span data-ttu-id="55b53-130">Knihovna jQuery.</span><span class="sxs-lookup"><span data-stu-id="55b53-130">The jQuery library.</span></span> <span data-ttu-id="55b53-131">Několik šablon webu služby WebMatrix již zahrnují knihovny jQuery v jejich *skriptu* složek.</span><span class="sxs-lookup"><span data-stu-id="55b53-131">Several of the WebMatrix site templates already include jQuery libraries in their *Script* folders.</span></span> <span data-ttu-id="55b53-132">Pokud jste tyto knihovny, si můžete stáhnout nejnovější knihovny jQuery přímo z [jQuery.org](http://jQuery.org) lokality.</span><span class="sxs-lookup"><span data-stu-id="55b53-132">If you do not have these libraries, you can download the latest jQuery library directly from the [jQuery.org](http://jQuery.org) site.</span></span> <span data-ttu-id="55b53-133">Nebo můžete vytvořit nové lokality pomocí šablony (například **Starter Site** šablony) a poté zkopírujte soubory jQuery z této lokality k aktuální lokalitě.</span><span class="sxs-lookup"><span data-stu-id="55b53-133">Or you can create a new site using a template (for example, the **Starter Site** template) and then copy the jQuery files from that site to your current site.</span></span>

<span data-ttu-id="55b53-134">Nakonec pokud chcete použít mapy Bing, musíte nejprve vytvořit účet (zdarma) a získat klíč.</span><span class="sxs-lookup"><span data-stu-id="55b53-134">Finally, if you want to use Bing maps, you must first create a (free) account and get a key.</span></span> <span data-ttu-id="55b53-135">Získat klíč, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="55b53-135">To get a key, follow these steps:</span></span>

1. <span data-ttu-id="55b53-136">Vytvoření účtu na [Bing Maps vývojářský účet](https://www.microsoft.com/maps/developers/web.aspx).</span><span class="sxs-lookup"><span data-stu-id="55b53-136">Create an account on the [Bing Maps Developer Account](https://www.microsoft.com/maps/developers/web.aspx).</span></span> <span data-ttu-id="55b53-137">Musíte mít účet Microsoft (Windows Live ID) také.</span><span class="sxs-lookup"><span data-stu-id="55b53-137">You must have a Microsoft account (Windows Live ID) as well.</span></span>

    <span data-ttu-id="55b53-138">Můžete zadat, že chcete použít klíč pro **vyhodnocení a testovací**.</span><span class="sxs-lookup"><span data-stu-id="55b53-138">You can specify that you want to use the key for **Evaluation/Test**.</span></span> <span data-ttu-id="55b53-139">Pokud testujete mapování funkce ve vašem počítači pomocí nástroje WebMatrix a služby IIS Express, přejděte **lokality** prostoru a poznamenejte si adresu URL vašeho webu (například `http://localhost:50408`, i když vaše číslo portu bude pravděpodobně lišit).</span><span class="sxs-lookup"><span data-stu-id="55b53-139">If you are testing the mapping function on your own computer using WebMatrix and IIS Express, go the **Site** workspace and note the URL of your site (for example, `http://localhost:50408`, although your port number will probably be different).</span></span> <span data-ttu-id="55b53-140">Můžete to použít *localhost* adresu jako lokalita při registraci.</span><span class="sxs-lookup"><span data-stu-id="55b53-140">You can use this *localhost* address as the site when you register.</span></span>
2. <span data-ttu-id="55b53-141">Po registraci pro účet, přejděte na účet Center pro mapy Bing a klikněte na tlačítko **vytvořit nebo zobrazení klíče**:</span><span class="sxs-lookup"><span data-stu-id="55b53-141">After you have registered for an account, go to the Bing Maps Account Center and click **Create or view keys**:</span></span>

    ![mapování-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. <span data-ttu-id="55b53-143">Záznam klíč, který vytvoří Bing.</span><span class="sxs-lookup"><span data-stu-id="55b53-143">Record the key that Bing creates.</span></span>

## <a name="creating-a-map-based-on-an-address-using-google"></a><span data-ttu-id="55b53-144">Vytváření mapy založené na adresu (pomocí Google)</span><span class="sxs-lookup"><span data-stu-id="55b53-144">Creating a Map Based on an Address (Using Google)</span></span>

<span data-ttu-id="55b53-145">Následující příklad ukazuje, jak vytvořit stránku, která vykreslí mapu založené na adresu.</span><span class="sxs-lookup"><span data-stu-id="55b53-145">The following example shows how to create a page that renders a map based on an address.</span></span> <span data-ttu-id="55b53-146">Tento příklad ukazuje, jak používat službu mapy Google.</span><span class="sxs-lookup"><span data-stu-id="55b53-146">This example shows how to use Google Maps.</span></span>

1. <span data-ttu-id="55b53-147">Vytvořte soubor s názvem *MapAddress.cshtml* v kořenovém adresáři serveru.</span><span class="sxs-lookup"><span data-stu-id="55b53-147">Create a file named *MapAddress.cshtml* in the root of the site.</span></span> <span data-ttu-id="55b53-148">Tato stránka bude Generovat mapu založené na adresu, která můžete předat.</span><span class="sxs-lookup"><span data-stu-id="55b53-148">This page will generate a map based on an address that you pass to it.</span></span>
2. <span data-ttu-id="55b53-149">Zkopírujte následující kód do souboru, přepsání existujícího obsahu.</span><span class="sxs-lookup"><span data-stu-id="55b53-149">Copy the following code into the file, overwriting the existing content.</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="55b53-150">Všimněte si následujících funkcí stránky:</span><span class="sxs-lookup"><span data-stu-id="55b53-150">Notice the following features of the page:</span></span>

    - <span data-ttu-id="55b53-151">`<script>` Element v `<head>` elementu.</span><span class="sxs-lookup"><span data-stu-id="55b53-151">The `<script>` element in the `<head>` element.</span></span> <span data-ttu-id="55b53-152">V příkladu `<script>` element odkazy *jquery 1.6.4.min.js* souboru, který je minifikovaný (komprimované) verze knihovny jQuery, verze 1.6.4.</span><span class="sxs-lookup"><span data-stu-id="55b53-152">In the example, the `<script>` element references the *jquery-1.6.4.min.js* file, which is a minified (compressed) version of the jQuery library, version 1.6.4.</span></span> <span data-ttu-id="55b53-153">Všimněte si, že odkaz na předpokládá, že *.js* soubor *skripty* složku vašeho webu.</span><span class="sxs-lookup"><span data-stu-id="55b53-153">Note that the reference assumes that the *.js* file is in the *Scripts* folder of your site.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="55b53-154">Pokud používáte jinou verzi knihovny jQuery, ujistěte se, že nastavíte ukazatel na příslušnou verzi správně.</span><span class="sxs-lookup"><span data-stu-id="55b53-154">If you're using a different version of the jQuery library, just make sure that you're pointing to that version correctly.</span></span>
    - <span data-ttu-id="55b53-155">Volání `@Maps.GetGoogleHtml` v těle stránky.</span><span class="sxs-lookup"><span data-stu-id="55b53-155">The call to the `@Maps.GetGoogleHtml` in the body of the page.</span></span> <span data-ttu-id="55b53-156">Chcete-li mapování adresy, musíte zadat řetězec adresy.</span><span class="sxs-lookup"><span data-stu-id="55b53-156">To map an address, you must pass an address string.</span></span> <span data-ttu-id="55b53-157">Metody pro jiné moduly mapy fungovat podobným způsobem (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span><span class="sxs-lookup"><span data-stu-id="55b53-157">The methods for the other map engines work in a similar way (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span></span>
- <span data-ttu-id="55b53-158">Spuštění stránky a zadejte adresu.</span><span class="sxs-lookup"><span data-stu-id="55b53-158">Run the page and enter an address.</span></span> <span data-ttu-id="55b53-159">Na stránce zobrazuje mapě, podle Google Maps, který ukazuje umístění, které jste zadali.</span><span class="sxs-lookup"><span data-stu-id="55b53-159">The page displays a map, based on Google Maps, that shows the location that you specified.</span></span>

    ![mapování 1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a><span data-ttu-id="55b53-161">Vytváření mapy podle zeměpisnou šířku a délku koordinuje (pomocí Bing)</span><span class="sxs-lookup"><span data-stu-id="55b53-161">Creating a Map Based on Latitude and Longitude Coordinates (Using Bing)</span></span>

<span data-ttu-id="55b53-162">Tento příklad ukazuje postup vytvoření mapy podle souřadnice.</span><span class="sxs-lookup"><span data-stu-id="55b53-162">This example shows how to create a map based on coordinates.</span></span> <span data-ttu-id="55b53-163">Tento příklad ukazuje, jak používat mapy Bing a jak se zahrnuje klíč Bing.</span><span class="sxs-lookup"><span data-stu-id="55b53-163">This example shows how to use Bing maps and how to include your Bing key.</span></span> <span data-ttu-id="55b53-164">(Můžete vytvořit mapu podle souřadnice také pomocí jiné mapy motory bez použití klíče služby Bing.)</span><span class="sxs-lookup"><span data-stu-id="55b53-164">(You can create a map based on coordinates using the other map engines also, without using a Bing key.)</span></span>

1. <span data-ttu-id="55b53-165">Vytvořte soubor s názvem *MapCoordinates.cshtml* v kořenu lokality a nahradit existující obsah s následující kód a značky:</span><span class="sxs-lookup"><span data-stu-id="55b53-165">Create a file named *MapCoordinates.cshtml* in the root of the site and replace the existing content with the following code and markup:</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. <span data-ttu-id="55b53-166">Nahraďte `your-key-here` klíčem mapy Bing, který jste vygenerovali dříve.</span><span class="sxs-lookup"><span data-stu-id="55b53-166">Replace `your-key-here` with the Bing Maps key that you generated earlier.</span></span>
3. <span data-ttu-id="55b53-167">Spustit *MapCoordinates.cshtml* zadejte zeměpisné šířky a délky a pak klikněte na tlačítko **mapy ji!**</span><span class="sxs-lookup"><span data-stu-id="55b53-167">Run the *MapCoordinates.cshtml* page, enter latitude and longitude coordinates, and then click the **Map It!**</span></span> <span data-ttu-id="55b53-168">tlačítko.</span><span class="sxs-lookup"><span data-stu-id="55b53-168">button.</span></span> <span data-ttu-id="55b53-169">(Pokud neznáte žádné souřadnice, zkuste následující postup.</span><span class="sxs-lookup"><span data-stu-id="55b53-169">(If you don't know any coordinates, try the following.</span></span> <span data-ttu-id="55b53-170">Toto je umístění v Redmondu Microsoft kanceláře.)</span><span class="sxs-lookup"><span data-stu-id="55b53-170">This is a location on the Microsoft Redmond campus.)</span></span>

    - <span data-ttu-id="55b53-171">Zeměpisná šířka: 47.6781005859375</span><span class="sxs-lookup"><span data-stu-id="55b53-171">Latitude: 47.6781005859375</span></span>
    - <span data-ttu-id="55b53-172">Zeměpisná délka:-122.158317565918</span><span class="sxs-lookup"><span data-stu-id="55b53-172">Longitude: -122.158317565918</span></span>

    <span data-ttu-id="55b53-173">Stránka se zobrazí pomocí souřadnice, které jste zadali.</span><span class="sxs-lookup"><span data-stu-id="55b53-173">The page is displayed using the coordinates that you specified.</span></span>

    ![mapování 3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="55b53-175">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="55b53-175">Additional Resources</span></span>


[<span data-ttu-id="55b53-176">Referenční dokumentace rozhraní API Microsoft.Maps</span><span class="sxs-lookup"><span data-stu-id="55b53-176">Microsoft.Maps API Reference</span></span>](https://msdn.microsoft.com/en-us/library/gg427611.aspx)
