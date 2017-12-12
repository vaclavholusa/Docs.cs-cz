---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: "Lokality pomocí test CAPTCHA robotů zabránit v používání vaší Razor rozhraní ASP.NET Web) | Microsoft Docs"
author: microsoft
description: "Tento článek vysvětluje, jak používat nástroje ReCaptcha (z důvodu zabezpečení) k automatizovaným programům (robotů) bránit v provádění úloh na webových stránkách ASP.NET (Razor) jsme..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2012
ms.topic: article
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: 75e80a3e7ebe787852152404bf2e0bf88a1a6a56
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a><span data-ttu-id="6c0b2-103">Lokality pomocí test CAPTCHA robotů zabránit v používání vaší Razor rozhraní ASP.NET Web)</span><span class="sxs-lookup"><span data-stu-id="6c0b2-103">Using a CAPTCHA to Prevent Bots from Using Your ASP.NET Web Razor) Site</span></span>
====================
<span data-ttu-id="6c0b2-104">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="6c0b2-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="6c0b2-105">Tento článek vysvětluje, jak zabránit provádění úloh na webu technologie ASP.NET Web Pages (Razor) automatizovaným programům (robotů) pomocí nástroje ReCaptcha (z důvodu zabezpečení).</span><span class="sxs-lookup"><span data-stu-id="6c0b2-105">This article explains how to use ReCaptcha (a security measure) to prevent automated programs (bots) from performing tasks in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="6c0b2-106">**Získáte informace:**</span><span class="sxs-lookup"><span data-stu-id="6c0b2-106">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="6c0b2-107">Postup přidání testu CAPTCHA na váš web.</span><span class="sxs-lookup"><span data-stu-id="6c0b2-107">How to add a CAPTCHA test to your site.</span></span>
> 
> <span data-ttu-id="6c0b2-108">Toto jsou nové v článku funkce ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="6c0b2-108">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="6c0b2-109">`ReCaptcha` Pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="6c0b2-109">The `ReCaptcha` helper.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="6c0b2-110">Informace v tomto článku se vztahují na 1.0 webových stránek ASP.NET a webové stránky 2.</span><span class="sxs-lookup"><span data-stu-id="6c0b2-110">The information in this article applies to ASP.NET Web Pages 1.0 and Web Pages 2.</span></span>


## <a name="about-captchas"></a><span data-ttu-id="6c0b2-111">O CAPTCHAs</span><span class="sxs-lookup"><span data-stu-id="6c0b2-111">About CAPTCHAs</span></span>

<span data-ttu-id="6c0b2-112">Vždy, když umožníte uživatelům zaregistrovat ve vaší lokalitě nebo i právě zadejte název a adresu URL (jako pro komentář blog), může se zobrazit zahlcení falešných názvy.</span><span class="sxs-lookup"><span data-stu-id="6c0b2-112">Any time you let people register in your site, or even just enter a name and URL (like for a blog comment), you might get a flood of fake names.</span></span> <span data-ttu-id="6c0b2-113">Často jsou ponechána automatizované programy (robotů), které se pokusí o ponechte adresy URL v každému webu, který může najít.</span><span class="sxs-lookup"><span data-stu-id="6c0b2-113">These are often left by automated programs (bots) that try to leave URLs in every website they can find.</span></span> <span data-ttu-id="6c0b2-114">(Běžné motivace je o vystavení adresy URL produktů pro prodej.)</span><span class="sxs-lookup"><span data-stu-id="6c0b2-114">(A common motivation is to post the URLs of products for sale.)</span></span>

<span data-ttu-id="6c0b2-115">Vám může pomoct, ujistěte se, že uživatel je skutečná osoba a není počítačový program pomocí *CAPTCHA* k ověření uživatelů při registraci nebo v opačném případě zadejte své jméno a lokality.</span><span class="sxs-lookup"><span data-stu-id="6c0b2-115">You can help make sure that a user is real person and not a computer program by using a *CAPTCHA* to validate users when they register or otherwise enter their name and site.</span></span> <span data-ttu-id="6c0b2-116">Test CAPTCHA je zkratka pro test zcela automatizovat veřejné Turing říct počítače a od sebe člověka.</span><span class="sxs-lookup"><span data-stu-id="6c0b2-116">CAPTCHA stands for Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="6c0b2-117">Je test CAPTCHA *výzvy a odezvy* ve které je uživatel vyzván, aby dělala něco test, který je snadné pro osobu uděláte, ale pevný pro automatizovaný program udělat.</span><span class="sxs-lookup"><span data-stu-id="6c0b2-117">A CAPTCHA is a *challenge-response* test in which the user is asked to do something that is easy for a person to do but hard for an automated program to do.</span></span> <span data-ttu-id="6c0b2-118">Nejběžnějším typem CAPTCHA je jedním kde najdete některé zkreslený písmena a vyzváni k jejich zadání.</span><span class="sxs-lookup"><span data-stu-id="6c0b2-118">The most common type of CAPTCHA is one where you see some distorted letters and are asked to type them.</span></span> <span data-ttu-id="6c0b2-119">(Narušení by měla je těžké robotů dekódovat písmena.)</span><span class="sxs-lookup"><span data-stu-id="6c0b2-119">(The distortion is supposed to make it hard for bots to decipher the letters.)</span></span>

## <a name="adding-a-recaptcha-test"></a><span data-ttu-id="6c0b2-120">Přidání nástroje ReCaptcha testu</span><span class="sxs-lookup"><span data-stu-id="6c0b2-120">Adding a ReCaptcha Test</span></span>

<span data-ttu-id="6c0b2-121">Na stránkách ASP.NET, můžete použít `ReCaptcha` pomocná rutina pro vykreslení testu CAPTCHA, která je založená na službě nástroje ReCaptcha ([http://recaptcha.net](http://recaptcha.net)).</span><span class="sxs-lookup"><span data-stu-id="6c0b2-121">In ASP.NET pages, you can use the `ReCaptcha` helper to render a CAPTCHA test that is based on the ReCaptcha service ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="6c0b2-122">`ReCaptcha` Pomocník zobrazí obrázek dvě zkreslený slova, která uživatelé mají správně zadat, aby byl ověřen stránky.</span><span class="sxs-lookup"><span data-stu-id="6c0b2-122">The `ReCaptcha` helper displays an image of two distorted words that users have to enter correctly before the page is validated.</span></span> <span data-ttu-id="6c0b2-123">Pomocí služby ReCaptcha.Net je ověřit odpověď uživatele.</span><span class="sxs-lookup"><span data-stu-id="6c0b2-123">The user response is validated by the ReCaptcha.Net service.</span></span>

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. <span data-ttu-id="6c0b2-124">Zaregistrovat váš web v ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span><span class="sxs-lookup"><span data-stu-id="6c0b2-124">Register your website at ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="6c0b2-125">Po dokončení registrace budete získat veřejný klíč a soukromý klíč.</span><span class="sxs-lookup"><span data-stu-id="6c0b2-125">When you've completed registration, you'll get a public key and a private key.</span></span>
2. <span data-ttu-id="6c0b2-126">Přidejte knihovnu ASP.NET Web Helpers na váš web, jak je popsáno v [instalaci pomocné rutiny v stránku ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), pokud jste tak ještě neučinili.</span><span class="sxs-lookup"><span data-stu-id="6c0b2-126">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
3. <span data-ttu-id="6c0b2-127">Pokud ještě nemáte  *\_AppStart.cshtml* souboru, v kořenové složce webu, vytvořte soubor s názvem  *\_AppStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6c0b2-127">If you don't already have a *\_AppStart.cshtml* file, in the root folder of a website create a file named *\_AppStart.cshtml*.</span></span>
4. <span data-ttu-id="6c0b2-128">Přidejte následující `Recaptcha` nastavení pomocníka v  *\_AppStart.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="6c0b2-128">Add the following `Recaptcha` helper settings in the *\_AppStart.cshtml* file:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. <span data-ttu-id="6c0b2-129">Nastavte `PublicKey` a `PrivateKey` vlastností pomocí vlastní veřejné a soukromé klíče.</span><span class="sxs-lookup"><span data-stu-id="6c0b2-129">Set the `PublicKey` and `PrivateKey` properties using your own public and private keys.</span></span>
6. <span data-ttu-id="6c0b2-130">Uložit  *\_AppStart.cshtml* souboru a zavřete ho.</span><span class="sxs-lookup"><span data-stu-id="6c0b2-130">Save the *\_AppStart.cshtml* file and close it.</span></span>
7. <span data-ttu-id="6c0b2-131">V kořenové složce webu, vytvořte novou stránku s názvem *Recaptcha.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6c0b2-131">In the root folder of a website, create new page named *Recaptcha.cshtml*.</span></span>
8. <span data-ttu-id="6c0b2-132">Nahradí existující obsah s následujícími službami:</span><span class="sxs-lookup"><span data-stu-id="6c0b2-132">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. <span data-ttu-id="6c0b2-133">Spustit *Recaptcha.cshtml* stránku v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="6c0b2-133">Run the *Recaptcha.cshtml* page in a browser.</span></span> <span data-ttu-id="6c0b2-134">Pokud `PrivateKey` hodnota je platná, že stránka zobrazuje ovládacího prvku nástroje ReCaptcha a tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6c0b2-134">If the `PrivateKey` value is valid, the page displays the ReCaptcha control and a button.</span></span> <span data-ttu-id="6c0b2-135">Pokud jste nenastavili klíče globálně v  *\_AppStart.html*, na stránce se zobrazí chybu.</span><span class="sxs-lookup"><span data-stu-id="6c0b2-135">If you had not set the keys globally in *\_AppStart.html*, the page would display an error.</span></span> 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. <span data-ttu-id="6c0b2-136">Zadejte slova pro test.</span><span class="sxs-lookup"><span data-stu-id="6c0b2-136">Enter the words for the test.</span></span> <span data-ttu-id="6c0b2-137">Pokud předáte test nástroje ReCaptcha, zobrazí se zpráva k tomuto účelu.</span><span class="sxs-lookup"><span data-stu-id="6c0b2-137">If you pass the ReCaptcha test, you see a message to that effect.</span></span> <span data-ttu-id="6c0b2-138">V opačném případě se zobrazí chybová zpráva a ovládacího prvku nástroje ReCaptcha se zobrazí znovu.</span><span class="sxs-lookup"><span data-stu-id="6c0b2-138">Otherwise you see an error message and the ReCaptcha control is redisplayed.</span></span>

> [!NOTE]
> <span data-ttu-id="6c0b2-139">Pokud je počítač v doméně, která používá proxy server, může být potřeba nakonfigurovat `defaultproxy` element *Web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="6c0b2-139">If your computer is on a domain that uses proxy server, you might need to configure the `defaultproxy` element of the *Web.config* file.</span></span> <span data-ttu-id="6c0b2-140">Následující příklad ukazuje *Web.config* soubor s `defaultproxy` element nakonfigurovaný tak, aby povolení nástroje ReCaptcha službu, kterou chcete pracovat.</span><span class="sxs-lookup"><span data-stu-id="6c0b2-140">The following example shows a *Web.config* file with the `defaultproxy` element configured to enable the ReCaptcha service to work.</span></span>
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="6c0b2-141">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="6c0b2-141">Additional Resources</span></span>


- [<span data-ttu-id="6c0b2-142">Přizpůsobení chování na webu pro ASP.NET – webové stránky servery</span><span class="sxs-lookup"><span data-stu-id="6c0b2-142">Customizing Site-Wide Behavior for ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="6c0b2-143">Lokality nástroje ReCaptcha</span><span class="sxs-lookup"><span data-stu-id="6c0b2-143">ReCaptcha site</span></span>](https://www.google.com/recaptcha)
