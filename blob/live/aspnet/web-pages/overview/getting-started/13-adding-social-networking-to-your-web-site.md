---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: "Přidání sociálních sítí k rozhraní ASP.NET Web Pages lokalit (Razor) | Microsoft Docs"
author: tfitzmac
description: "Tato kapitola vysvětluje, jak integrovat svůj web se služby sociálních sítí. V této kapitole se dozvíte, jak uživatelům záložku/propojení webu umožňuje..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 2c43fa7d286e43f3a4581662ce421c7435e1871f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a><span data-ttu-id="8aca6-104">Přidání sociální sítě na webové stránky ASP.NET lokalit (Razor)</span><span class="sxs-lookup"><span data-stu-id="8aca6-104">Adding Social Networking to ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="8aca6-105">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="8aca6-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="8aca6-106">Tento článek vysvětluje, jak přidat sociálních sítí odkazy pro Facebook, Twitter, Reddit a Digg na stránky na webu technologie ASP.NET Web Pages (Razor) a jak se zahrnuje Twitter kanály, Xbox hráči karty a Gravatar obrázků.</span><span class="sxs-lookup"><span data-stu-id="8aca6-106">This article explains how to add social networking links for Facebook, Twitter, Reddit, and Digg to pages in an ASP.NET Web Pages (Razor) website, and how to include Twitter feeds, Xbox gamer cards, and Gravatar images.</span></span>
> 
> <span data-ttu-id="8aca6-107">Získáte informace:</span><span class="sxs-lookup"><span data-stu-id="8aca6-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="8aca6-108">Jak uživatelé mohou záložku/propojení vaší lokality.</span><span class="sxs-lookup"><span data-stu-id="8aca6-108">How to let people bookmark/link your site.</span></span>
> - <span data-ttu-id="8aca6-109">Postup přidání informační kanál sítě Twitter.</span><span class="sxs-lookup"><span data-stu-id="8aca6-109">How to add a Twitter feed.</span></span>
> - <span data-ttu-id="8aca6-110">Postup přidání Facebooku **jako** tlačítko stránky.</span><span class="sxs-lookup"><span data-stu-id="8aca6-110">How to add a Facebook **Like** button to pages.</span></span>
> - <span data-ttu-id="8aca6-111">Jak vykreslení obrázků Gravatar.com.</span><span class="sxs-lookup"><span data-stu-id="8aca6-111">How to render Gravatar.com images.</span></span>
> - <span data-ttu-id="8aca6-112">Postupy: zobrazení karty hráči Xbox na váš web.</span><span class="sxs-lookup"><span data-stu-id="8aca6-112">How to display an Xbox gamer card on your site.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8aca6-113">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="8aca6-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="8aca6-114">Rozhraní ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="8aca6-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="8aca6-115">ASP.NET webové pomocné knihovny (balíček NuGet)</span><span class="sxs-lookup"><span data-stu-id="8aca6-115">ASP.NET Web Helper Library (NuGet package)</span></span>
>   
> 
> <span data-ttu-id="8aca6-116">V tomto kurzu funguje taky s ASP.NET Web Pages 3, s výjimkou částí, které používají pomocné knihovny ASP.NET Web.</span><span class="sxs-lookup"><span data-stu-id="8aca6-116">This tutorial also works with ASP.NET Web Pages 3, except for parts that use the ASP.NET Web Helper Library.</span></span>


<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a><span data-ttu-id="8aca6-117">Propojování vašeho webu na weby sociálních sítí</span><span class="sxs-lookup"><span data-stu-id="8aca6-117">Linking Your Website on Social Networking Sites</span></span>

<span data-ttu-id="8aca6-118">Pokud uživatelé jako něco na svém webu, často chtějí sdílet s přátel.</span><span class="sxs-lookup"><span data-stu-id="8aca6-118">If people like something on your site, they often want to share it with friends.</span></span> <span data-ttu-id="8aca6-119">Můžete nastavit to snadno zobrazením glyfů (ikon), které lze kliknout a sdílet na stránku v rámci rozhraní Digg, Reddit, Facebook, Twitter nebo podobné weby.</span><span class="sxs-lookup"><span data-stu-id="8aca6-119">You can make this easy by displaying glyphs (icons) that people can click to share a page on Digg, Reddit, Facebook, Twitter, or similar sites.</span></span>

<span data-ttu-id="8aca6-120">Chcete-li zobrazit tyto glyfů, přidejte `LinkSharecode` pomocná na stránku.</span><span class="sxs-lookup"><span data-stu-id="8aca6-120">To display these glyphs, add the `LinkSharecode` helper to a page.</span></span> <span data-ttu-id="8aca6-121">Uživateli, kteří navštíví stránku kliknutím na jednotlivé glyfů.</span><span class="sxs-lookup"><span data-stu-id="8aca6-121">People who visit your page can click an individual glyph.</span></span> <span data-ttu-id="8aca6-122">Pokud budou mít účet s sociálních sítí lokality, se pak zveřejnit odkaz na stránku na dané lokalitě.</span><span class="sxs-lookup"><span data-stu-id="8aca6-122">If they have an account with that social networking site, they can then post a link to your page on that site.</span></span>

![Obrázek 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. <span data-ttu-id="8aca6-124">Přidejte knihovnu ASP.NET Web Helpers na váš web, jak je popsáno v [instalaci pomocné rutiny v stránku ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), pokud jste ještě nepřidali jej - vytvořit stránku s názvem *ListLinkShare.cshtml* a přidejte Následující kód:</span><span class="sxs-lookup"><span data-stu-id="8aca6-124">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.- Create a page named *ListLinkShare.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    <span data-ttu-id="8aca6-125">V tomto příkladu když `LinkShare` spustí pomocné rutiny, název stránky se předá jako parametr, který pak předá název stránky na web sociálních sítí.</span><span class="sxs-lookup"><span data-stu-id="8aca6-125">In this example, when the `LinkShare` helper runs, the page title is passed as a parameter, which in turn passes the page title to the social networking site.</span></span> <span data-ttu-id="8aca6-126">Však může předávat v libovolný řetězec, který má být.</span><span class="sxs-lookup"><span data-stu-id="8aca6-126">However, you could pass in any string you want.</span></span> <span data-ttu-id="8aca6-127">Tento příklad také určuje, které weby sociálních sítí pro zahrnutí do seznamu.</span><span class="sxs-lookup"><span data-stu-id="8aca6-127">This example also specifies which social networking sites to include in the list.</span></span> <span data-ttu-id="8aca6-128">Můžete určit weby sociálních sítí, které jsou relevantní pro váš web.</span><span class="sxs-lookup"><span data-stu-id="8aca6-128">You can specify the social networking sites that are relevant to your site.</span></span>
- <span data-ttu-id="8aca6-129">Spustit *ListLinkShare.cshtml* stránku v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="8aca6-129">Run the *ListLinkShare.cshtml* page in a browser.</span></span> <span data-ttu-id="8aca6-130">(Ujistěte se, že je vybraný stránky v **soubory** pracovního prostoru, než ji spustit.)</span><span class="sxs-lookup"><span data-stu-id="8aca6-130">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
- <span data-ttu-id="8aca6-131">Klikněte na tlačítko glyf jeden z webů, které jste si pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="8aca6-131">Click a glyph for one of the sites that you're signed up for.</span></span> <span data-ttu-id="8aca6-132">Odkaz vás zavede na stránku na webu vybrané sociálních sítí, kde můžete sdílet odkaz.</span><span class="sxs-lookup"><span data-stu-id="8aca6-132">The link takes you to the page on the selected social network site where you can share a link.</span></span> <span data-ttu-id="8aca6-133">Například pokud kliknete na odkaz Reddit, jste jste udělali na `submit to reddit` na Reddit webu.</span><span class="sxs-lookup"><span data-stu-id="8aca6-133">For example, if you click the Reddit link, you're taken to the `submit to reddit` page on the Reddit website.</span></span>

    ![Obrázek 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a><span data-ttu-id="8aca6-135">Přidání Twitter kanálu</span><span class="sxs-lookup"><span data-stu-id="8aca6-135">Adding a Twitter Feed</span></span>

<span data-ttu-id="8aca6-136">Informace o používání Pomocník Twitter, který je kompatibilní s aktuální verzí API serveru Twitter najdete v tématu [Pomocník Twitter](../ui-layouts-and-themes/twitter-helper.md).</span><span class="sxs-lookup"><span data-stu-id="8aca6-136">For information about using a Twitter helper that is compatible with the current version of the Twitter API, see [Twitter helper](../ui-layouts-and-themes/twitter-helper.md).</span></span> <span data-ttu-id="8aca6-137">Tento příklad ukazuje, jak psát vlastní pomocné rutiny, můžete snadno opakovaně použít kód z mnoha stránky.</span><span class="sxs-lookup"><span data-stu-id="8aca6-137">This example shows how to write your own helper so you can easily reuse the code from many pages.</span></span>

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a><span data-ttu-id="8aca6-138">Zobrazení Facebook &quot;jako&quot; tlačítko</span><span class="sxs-lookup"><span data-stu-id="8aca6-138">Displaying a Facebook &quot;Like&quot; Button</span></span>

<span data-ttu-id="8aca6-139">V některých případech je nejlepší možnost získat kód přímo z poskytovateli sociální sítě, namísto spoléhání na pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="8aca6-139">In some cases, your best option is to get the code directly from the social networking provider rather than relying on a helper.</span></span> <span data-ttu-id="8aca6-140">To platí hlavně pokud poskytovateli sociální sítě aktualizace její možnosti rychleji, než se aktualizuje pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="8aca6-140">This is especially true if the social network provider updates its options more quickly than the helper is updated.</span></span>

<span data-ttu-id="8aca6-141">Chcete-li přidat funkce Facebook (jako je například tlačítko) na váš web, můžete načíst fragmenty kódu z [developers.facebook.com](https://developers.facebook.com/) lokality.</span><span class="sxs-lookup"><span data-stu-id="8aca6-141">To add Facebook features (such as the Like button) to your site, you can retrieve code snippets from the [developers.facebook.com](https://developers.facebook.com/) site.</span></span> <span data-ttu-id="8aca6-142">Na webu Facebook používat své nástroje pro generování fragment kódu, které se týkají vašeho webu.</span><span class="sxs-lookup"><span data-stu-id="8aca6-142">On the Facebook site, you use their tools to generate a code snippet that is relevant to your site.</span></span>

<span data-ttu-id="8aca6-143">Následující zvýrazněný kód je kód, který byl získán z tlačítko jako nástroj na webu developers.facebook.com.</span><span class="sxs-lookup"><span data-stu-id="8aca6-143">The following highlighted code is the code that was retrieved from the Like Button tool on the developers.facebook.com site.</span></span> <span data-ttu-id="8aca6-144">Je nutné zadat vlastní ID aplikace.</span><span class="sxs-lookup"><span data-stu-id="8aca6-144">You must provide your own app ID.</span></span>

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a><span data-ttu-id="8aca6-145">Vykreslování obrázku Gravatar</span><span class="sxs-lookup"><span data-stu-id="8aca6-145">Rendering a Gravatar Image</span></span>

<span data-ttu-id="8aca6-146">A *Gravatar* ( &quot;globálně rozpoznaný miniatury&quot;) je image, které lze použít na více webech, jako miniatury &#8212; který je obrázek, který představuje můžete.</span><span class="sxs-lookup"><span data-stu-id="8aca6-146">A *Gravatar* (a &quot;globally recognized avatar&quot;) is an image that can be used on multiple websites as your avatar &#8212; that is, an image that represents you.</span></span> <span data-ttu-id="8aca6-147">Například můžete Gravatar identifikaci osoby v příspěvek ve fóru v blogu komentář a tak dále.</span><span class="sxs-lookup"><span data-stu-id="8aca6-147">For example, a Gravatar can identify a person in a forum post, in a blog comment, and so on.</span></span> <span data-ttu-id="8aca6-148">(Můžete zaregistrovat vlastní Gravatar na webu Gravatar na [http://www.gravatar.com/](http://www.gravatar.com/).) Pokud chcete pro zobrazení obrázků vedle jména nebo e-mailové adresy uživatelů na vašem webu, můžete použít Gravatar pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="8aca6-148">(You can register your own Gravatar at the Gravatar website at [http://www.gravatar.com/](http://www.gravatar.com/).) If you want to display images next to people's names or email addresses on your website, you can use the Gravatar helper.</span></span>

<span data-ttu-id="8aca6-149">V tomto příkladu používáte jeden Gravatar, který představuje sami.</span><span class="sxs-lookup"><span data-stu-id="8aca6-149">In this example, you're using a single Gravatar that represents yourself.</span></span> <span data-ttu-id="8aca6-150">Další způsob použití Gravatar je umožnit osoby, zadejte jeho adresu Gravatar po jejich registraci na váš web.</span><span class="sxs-lookup"><span data-stu-id="8aca6-150">Another way to use a Gravatar is to let people specify their Gravatar address when they register on your site.</span></span> <span data-ttu-id="8aca6-151">(Můžete naučit umožníte uživatelům zaregistrovat v [členství na web webových stránek ASP.NET a přidání zabezpečení](https://go.microsoft.com/fwlink/?LinkId=202904).) Potom vždy, když zobrazíte informace pro tohoto uživatele, můžete přidat Gravatar právě kde zobrazit uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="8aca6-151">(You can learn how to let people register in [Adding Security and Membership to an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=202904).) Then whenever you display information for that user, you can just add the Gravatar to where you display the user's name.</span></span>

1. <span data-ttu-id="8aca6-152">Přidejte knihovnu ASP.NET Web Helpers na váš web, jak je popsáno v [instalaci pomocné rutiny v stránku ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), pokud jste tak ještě neučinili.</span><span class="sxs-lookup"><span data-stu-id="8aca6-152">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="8aca6-153">Vytvořit novou webovou stránku s názvem *Gravatar.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8aca6-153">Create a new web page named *Gravatar.cshtml*.</span></span>
3. <span data-ttu-id="8aca6-154">Přidejte následující kód do souboru:</span><span class="sxs-lookup"><span data-stu-id="8aca6-154">Add the following markup to the file:</span></span> 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    <span data-ttu-id="8aca6-155">`Gravatar.GetHtml` Metoda zobrazí obrázek Gravatar na stránce.</span><span class="sxs-lookup"><span data-stu-id="8aca6-155">The `Gravatar.GetHtml` method displays the Gravatar image on the page.</span></span> <span data-ttu-id="8aca6-156">Chcete-li změnit velikost bitové kopie, můžete zahrnout číslo jako druhý parametr.</span><span class="sxs-lookup"><span data-stu-id="8aca6-156">To change the size of the image, you can include a number as a second parameter.</span></span> <span data-ttu-id="8aca6-157">Výchozí velikost je 80.</span><span class="sxs-lookup"><span data-stu-id="8aca6-157">The default size is 80.</span></span> <span data-ttu-id="8aca6-158">Čísla menší než 80 zkontrolujte bitovou kopii menší.</span><span class="sxs-lookup"><span data-stu-id="8aca6-158">Numbers less than 80 make the image smaller.</span></span> <span data-ttu-id="8aca6-159">Čísla větší než 80 zvětšit bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="8aca6-159">Numbers greater than 80 make the image larger.</span></span>
4. <span data-ttu-id="8aca6-160">V `Gravatar.GetHtml` metody, nahraďte `<Your Gravatar account here>` s e-mailovou adresu, který používáte pro váš účet Gravatar.</span><span class="sxs-lookup"><span data-stu-id="8aca6-160">In the `Gravatar.GetHtml` methods, replace `<Your Gravatar account here>` with the email address that you use for your Gravatar account.</span></span> <span data-ttu-id="8aca6-161">(Pokud nemáte účet Gravatar, můžete e-mailovou adresu osoby, který.)</span><span class="sxs-lookup"><span data-stu-id="8aca6-161">(If you don't have a Gravatar account, you can use the email address of someone who does.)</span></span>
5. <span data-ttu-id="8aca6-162">Spusťte stránku v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="8aca6-162">Run the page in your browser.</span></span> <span data-ttu-id="8aca6-163">Na stránce se zobrazí dvě bitové kopie Gravatar pro e-mailovou adresu, kterou jste zadali.</span><span class="sxs-lookup"><span data-stu-id="8aca6-163">The page displays two Gravatar images for the email address you specified.</span></span> <span data-ttu-id="8aca6-164">Druhý image je menší než první.</span><span class="sxs-lookup"><span data-stu-id="8aca6-164">The second image is smaller than the first.</span></span> 

    ![Obrázek 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a><span data-ttu-id="8aca6-166">Zobrazení hráči karty Xbox</span><span class="sxs-lookup"><span data-stu-id="8aca6-166">Displaying an Xbox Gamer Card</span></span>

<span data-ttu-id="8aca6-167">Když uživatelé hry pro Microsoft Xbox online, každý uživatel má jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="8aca6-167">When people play Microsoft Xbox games online, each user has a unique ID.</span></span> <span data-ttu-id="8aca6-168">Statistiky jsou v každé player ve formě hráči kartu, která zobrazuje jejich reputaci, hráči skóre a nedávno přehrávají hry.</span><span class="sxs-lookup"><span data-stu-id="8aca6-168">Statistics are kept for each player in the form of a gamer card, which shows their reputation, gamer score, and recently played games.</span></span> <span data-ttu-id="8aca6-169">Pokud jste Xbox hráči, můžete zobrazit hráči karty na stránkách ve vaší lokalitě pomocí `GamerCard` pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="8aca6-169">If you're an Xbox gamer, you can show your gamer card on pages in your site by using the `GamerCard` helper.</span></span>

1. <span data-ttu-id="8aca6-170">Přidejte knihovnu ASP.NET Web Helpers na váš web, jak je popsáno v [instalaci pomocné rutiny v stránku ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), pokud jste tak ještě neučinili.</span><span class="sxs-lookup"><span data-stu-id="8aca6-170">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="8aca6-171">Vytvořit novou stránku s názvem *XboxGamer.cshtml* a přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="8aca6-171">Create a new page named *XboxGamer.cshtml* and add the following markup.</span></span>

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    <span data-ttu-id="8aca6-172">Můžete použít `GamerCard.GetHtml` vlastnosti k určení aliasu pro hráči karty, který se má zobrazit.</span><span class="sxs-lookup"><span data-stu-id="8aca6-172">You use the `GamerCard.GetHtml` property to specify the alias for the gamer card to be displayed.</span></span>
3. <span data-ttu-id="8aca6-173">Spusťte stránku v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="8aca6-173">Run the page in your browser.</span></span> <span data-ttu-id="8aca6-174">Na stránce se zobrazí karta hráči Xbox, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="8aca6-174">The page displays the Xbox gamer card that you specified.</span></span>

    ![Obrázek 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
