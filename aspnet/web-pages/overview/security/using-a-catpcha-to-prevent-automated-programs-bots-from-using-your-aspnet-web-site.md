---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Lokality pomocí testu CAPTCHA robotů zabránit v používání vašich Razor rozhraní ASP.NET Web) | Dokumentace Microsoftu
author: microsoft
description: Tento článek vysvětluje, jak pomocí nástroje ReCaptcha (v rámci bezpečnostních opatření) automatizovaným programům (robotům) zabránit v provádění úloh na webových stránkách ASP.NET (Razor) jsme...
ms.author: aspnetcontent
ms.date: 05/21/2012
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: f67eb60c23e0eec46089ceea9b04779492dfa15e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803068"
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a><span data-ttu-id="b6358-103">Použití testu CAPTCHA pro roboty zabránit v používání vašich Razor rozhraní ASP.NET Web) webu</span><span class="sxs-lookup"><span data-stu-id="b6358-103">Using a CAPTCHA to Prevent Bots from Using Your ASP.NET Web Razor) Site</span></span>
====================
<span data-ttu-id="b6358-104">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="b6358-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="b6358-105">Tento článek vysvětluje, jak pomocí nástroje ReCaptcha (v rámci bezpečnostních opatření) automatizovaným programům (robotům) zabránit v provádění úloh na webu rozhraní ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="b6358-105">This article explains how to use ReCaptcha (a security measure) to prevent automated programs (bots) from performing tasks in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="b6358-106">**Co se dozvíte:**</span><span class="sxs-lookup"><span data-stu-id="b6358-106">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="b6358-107">Jak přidat na váš web CAPTCHA test.</span><span class="sxs-lookup"><span data-stu-id="b6358-107">How to add a CAPTCHA test to your site.</span></span>
> 
> <span data-ttu-id="b6358-108">Toto jsou funkce technologie ASP.NET v následujícím článku:</span><span class="sxs-lookup"><span data-stu-id="b6358-108">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="b6358-109">`ReCaptcha` Pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="b6358-109">The `ReCaptcha` helper.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="b6358-110">Informace v tomto článku se vztahují na 1.0 webových stránek ASP.NET a Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="b6358-110">The information in this article applies to ASP.NET Web Pages 1.0 and Web Pages 2.</span></span>


## <a name="about-captchas"></a><span data-ttu-id="b6358-111">O CAPTCHAs</span><span class="sxs-lookup"><span data-stu-id="b6358-111">About CAPTCHAs</span></span>

<span data-ttu-id="b6358-112">Kdykoli umožňují uživatelům registraci na vašem webu nebo jenom zadat název a adresu URL (například pro komentáře blogu), může se zobrazit zahlcení falešné názvy.</span><span class="sxs-lookup"><span data-stu-id="b6358-112">Any time you let people register in your site, or even just enter a name and URL (like for a blog comment), you might get a flood of fake names.</span></span> <span data-ttu-id="b6358-113">Tyto jsou často zanechaný automatizovaným programům (robotům), které se pokoušejí necháte adresy URL v každému webu, který může najít.</span><span class="sxs-lookup"><span data-stu-id="b6358-113">These are often left by automated programs (bots) that try to leave URLs in every website they can find.</span></span> <span data-ttu-id="b6358-114">(Běžné motivace je publikovat adresy URL produktů pro prodej.)</span><span class="sxs-lookup"><span data-stu-id="b6358-114">(A common motivation is to post the URLs of products for sale.)</span></span>

<span data-ttu-id="b6358-115">Může pomoct, ujistěte se, že uživatel je skutečná osoba a není počítačový program pomocí *test CAPTCHA* k ověření uživatelů při registraci nebo jinak zadejte své jméno a lokality.</span><span class="sxs-lookup"><span data-stu-id="b6358-115">You can help make sure that a user is real person and not a computer program by using a *CAPTCHA* to validate users when they register or otherwise enter their name and site.</span></span> <span data-ttu-id="b6358-116">Test CAPTCHA. zkratka pro plně automatizované veřejné Turing test zjistit počítače a od sebe lidí.</span><span class="sxs-lookup"><span data-stu-id="b6358-116">CAPTCHA stands for Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="b6358-117">Je testu CAPTCHA *– výzva odezva* test ve kterém je uživatel vyzván k provedení nějaké akce, který je jednoduché pro osobu, která chcete, ale obtížné pro automatizované program udělat.</span><span class="sxs-lookup"><span data-stu-id="b6358-117">A CAPTCHA is a *challenge-response* test in which the user is asked to do something that is easy for a person to do but hard for an automated program to do.</span></span> <span data-ttu-id="b6358-118">Nejběžnějším typem test CAPTCHA. je jedním kde zobrazit některé zkreslený písmena a vyzváni k zadávání.</span><span class="sxs-lookup"><span data-stu-id="b6358-118">The most common type of CAPTCHA is one where you see some distorted letters and are asked to type them.</span></span> <span data-ttu-id="b6358-119">(Narušení by měl být obtížné pro roboty k dešifrování písmena.)</span><span class="sxs-lookup"><span data-stu-id="b6358-119">(The distortion is supposed to make it hard for bots to decipher the letters.)</span></span>

## <a name="adding-a-recaptcha-test"></a><span data-ttu-id="b6358-120">Přidání nástroje ReCaptcha testu</span><span class="sxs-lookup"><span data-stu-id="b6358-120">Adding a ReCaptcha Test</span></span>

<span data-ttu-id="b6358-121">Stránek v ASP.NET, můžete použít `ReCaptcha` pomocné rutiny pro vykreslení testu CAPTCHA, která je založená na službě nástroje ReCaptcha ([http://recaptcha.net](http://recaptcha.net)).</span><span class="sxs-lookup"><span data-stu-id="b6358-121">In ASP.NET pages, you can use the `ReCaptcha` helper to render a CAPTCHA test that is based on the ReCaptcha service ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="b6358-122">`ReCaptcha` Pomocné rutiny zobrazí obrázek ze dvou zkreslený slov, která uživatelé mají k zadání správně předtím, než se ověří na stránce.</span><span class="sxs-lookup"><span data-stu-id="b6358-122">The `ReCaptcha` helper displays an image of two distorted words that users have to enter correctly before the page is validated.</span></span> <span data-ttu-id="b6358-123">Odpověď uživatele je potvrzen v ReCaptcha.Net služby.</span><span class="sxs-lookup"><span data-stu-id="b6358-123">The user response is validated by the ReCaptcha.Net service.</span></span>

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. <span data-ttu-id="b6358-124">Zaregistrujte svůj web najdete tady ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span><span class="sxs-lookup"><span data-stu-id="b6358-124">Register your website at ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="b6358-125">Po dokončení registrace, zobrazí se veřejný klíč a soukromý klíč.</span><span class="sxs-lookup"><span data-stu-id="b6358-125">When you've completed registration, you'll get a public key and a private key.</span></span>
2. <span data-ttu-id="b6358-126">Přidejte knihovnu ASP.NET Web Helpers na váš web, jak je popsáno v [instalace pomocné rutiny na webu technologie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), pokud jste tak již neučinili.</span><span class="sxs-lookup"><span data-stu-id="b6358-126">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
3. <span data-ttu-id="b6358-127">Pokud ještě nemáte  *\_AppStart.cshtml* souboru, v kořenové složce webu vytvořte soubor s názvem  *\_AppStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b6358-127">If you don't already have a *\_AppStart.cshtml* file, in the root folder of a website create a file named *\_AppStart.cshtml*.</span></span>
4. <span data-ttu-id="b6358-128">Přidejte následující `Recaptcha` nastavení pomocné rutiny  *\_AppStart.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="b6358-128">Add the following `Recaptcha` helper settings in the *\_AppStart.cshtml* file:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. <span data-ttu-id="b6358-129">Nastavte `PublicKey` a `PrivateKey` vlastností použitím vlastní veřejné a soukromé klíče.</span><span class="sxs-lookup"><span data-stu-id="b6358-129">Set the `PublicKey` and `PrivateKey` properties using your own public and private keys.</span></span>
6. <span data-ttu-id="b6358-130">Uložit  *\_AppStart.cshtml* soubor a zavřete ho.</span><span class="sxs-lookup"><span data-stu-id="b6358-130">Save the *\_AppStart.cshtml* file and close it.</span></span>
7. <span data-ttu-id="b6358-131">V kořenové složce webu, vytvořte novou stránku s názvem *Recaptcha.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b6358-131">In the root folder of a website, create new page named *Recaptcha.cshtml*.</span></span>
8. <span data-ttu-id="b6358-132">Nahraďte existující obsah následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="b6358-132">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. <span data-ttu-id="b6358-133">Spustit *Recaptcha.cshtml* stránku v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="b6358-133">Run the *Recaptcha.cshtml* page in a browser.</span></span> <span data-ttu-id="b6358-134">Pokud `PrivateKey` je hodnota platná, na stránce se zobrazí ovládací prvek nástroje ReCaptcha a tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b6358-134">If the `PrivateKey` value is valid, the page displays the ReCaptcha control and a button.</span></span> <span data-ttu-id="b6358-135">Pokud jste nenastavili klíče globálně v  *\_AppStart.html*, na stránce zobrazí chybu.</span><span class="sxs-lookup"><span data-stu-id="b6358-135">If you had not set the keys globally in *\_AppStart.html*, the page would display an error.</span></span> 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. <span data-ttu-id="b6358-136">Zadejte slova pro test.</span><span class="sxs-lookup"><span data-stu-id="b6358-136">Enter the words for the test.</span></span> <span data-ttu-id="b6358-137">Pokud předáte testovací nástroje ReCaptcha, zobrazí se příslušná zpráva.</span><span class="sxs-lookup"><span data-stu-id="b6358-137">If you pass the ReCaptcha test, you see a message to that effect.</span></span> <span data-ttu-id="b6358-138">V opačném případě se zobrazí chybová zpráva a nástroje ReCaptcha ovládacího prvku se zobrazí znovu.</span><span class="sxs-lookup"><span data-stu-id="b6358-138">Otherwise you see an error message and the ReCaptcha control is redisplayed.</span></span>

> [!NOTE]
> <span data-ttu-id="b6358-139">Pokud je počítač připojen k doméně, která používá proxy server, může být nutné nakonfigurovat `defaultproxy` elementu *Web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="b6358-139">If your computer is on a domain that uses proxy server, you might need to configure the `defaultproxy` element of the *Web.config* file.</span></span> <span data-ttu-id="b6358-140">Následující příklad ukazuje *Web.config* souboru `defaultproxy` element nakonfigurované tak, aby služba nástroje ReCaptcha pracovat.</span><span class="sxs-lookup"><span data-stu-id="b6358-140">The following example shows a *Web.config* file with the `defaultproxy` element configured to enable the ReCaptcha service to work.</span></span>
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="b6358-141">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="b6358-141">Additional Resources</span></span>


- [<span data-ttu-id="b6358-142">Přizpůsobení chování v celém webu pro weby stránky technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b6358-142">Customizing Site-Wide Behavior for ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="b6358-143">Lokality nástroje ReCaptcha</span><span class="sxs-lookup"><span data-stu-id="b6358-143">ReCaptcha site</span></span>](https://www.google.com/recaptcha)
