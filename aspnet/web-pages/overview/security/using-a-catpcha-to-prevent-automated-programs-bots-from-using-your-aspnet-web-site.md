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
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>Lokality pomocí test CAPTCHA robotů zabránit v používání vaší Razor rozhraní ASP.NET Web)
====================
podle [Microsoft](https://github.com/microsoft)

> Tento článek vysvětluje, jak zabránit provádění úloh na webu technologie ASP.NET Web Pages (Razor) automatizovaným programům (robotů) pomocí nástroje ReCaptcha (z důvodu zabezpečení).
> 
> **Získáte informace:** 
> 
> - Postup přidání testu CAPTCHA na váš web.
> 
> Toto jsou nové v článku funkce ASP.NET:
> 
> - `ReCaptcha` Pomocné rutiny.
> 
> > [!NOTE]
> > Informace v tomto článku se vztahují na 1.0 webových stránek ASP.NET a webové stránky 2.


## <a name="about-captchas"></a>O CAPTCHAs

Vždy, když umožníte uživatelům zaregistrovat ve vaší lokalitě nebo i právě zadejte název a adresu URL (jako pro komentář blog), může se zobrazit zahlcení falešných názvy. Často jsou ponechána automatizované programy (robotů), které se pokusí o ponechte adresy URL v každému webu, který může najít. (Běžné motivace je o vystavení adresy URL produktů pro prodej.)

Vám může pomoct, ujistěte se, že uživatel je skutečná osoba a není počítačový program pomocí *CAPTCHA* k ověření uživatelů při registraci nebo v opačném případě zadejte své jméno a lokality. Test CAPTCHA je zkratka pro test zcela automatizovat veřejné Turing říct počítače a od sebe člověka. Je test CAPTCHA *výzvy a odezvy* ve které je uživatel vyzván, aby dělala něco test, který je snadné pro osobu uděláte, ale pevný pro automatizovaný program udělat. Nejběžnějším typem CAPTCHA je jedním kde najdete některé zkreslený písmena a vyzváni k jejich zadání. (Narušení by měla je těžké robotů dekódovat písmena.)

## <a name="adding-a-recaptcha-test"></a>Přidání nástroje ReCaptcha testu

Na stránkách ASP.NET, můžete použít `ReCaptcha` pomocná rutina pro vykreslení testu CAPTCHA, která je založená na službě nástroje ReCaptcha ([http://recaptcha.net](http://recaptcha.net)). `ReCaptcha` Pomocník zobrazí obrázek dvě zkreslený slova, která uživatelé mají správně zadat, aby byl ověřen stránky. Pomocí služby ReCaptcha.Net je ověřit odpověď uživatele.

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. Zaregistrovat váš web v ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Po dokončení registrace budete získat veřejný klíč a soukromý klíč.
2. Přidejte knihovnu ASP.NET Web Helpers na váš web, jak je popsáno v [instalaci pomocné rutiny v stránku ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), pokud jste tak ještě neučinili.
3. Pokud ještě nemáte  *\_AppStart.cshtml* souboru, v kořenové složce webu, vytvořte soubor s názvem  *\_AppStart.cshtml*.
4. Přidejte následující `Recaptcha` nastavení pomocníka v  *\_AppStart.cshtml* souboru: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. Nastavte `PublicKey` a `PrivateKey` vlastností pomocí vlastní veřejné a soukromé klíče.
6. Uložit  *\_AppStart.cshtml* souboru a zavřete ho.
7. V kořenové složce webu, vytvořte novou stránku s názvem *Recaptcha.cshtml*.
8. Nahradí existující obsah s následujícími službami: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. Spustit *Recaptcha.cshtml* stránku v prohlížeči. Pokud `PrivateKey` hodnota je platná, že stránka zobrazuje ovládacího prvku nástroje ReCaptcha a tlačítko. Pokud jste nenastavili klíče globálně v  *\_AppStart.html*, na stránce se zobrazí chybu. 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. Zadejte slova pro test. Pokud předáte test nástroje ReCaptcha, zobrazí se zpráva k tomuto účelu. V opačném případě se zobrazí chybová zpráva a ovládacího prvku nástroje ReCaptcha se zobrazí znovu.

> [!NOTE]
> Pokud je počítač v doméně, která používá proxy server, může být potřeba nakonfigurovat `defaultproxy` element *Web.config* souboru. Následující příklad ukazuje *Web.config* soubor s `defaultproxy` element nakonfigurovaný tak, aby povolení nástroje ReCaptcha službu, kterou chcete pracovat.
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Další prostředky


- [Přizpůsobení chování na webu pro ASP.NET – webové stránky servery](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Lokality nástroje ReCaptcha](https://www.google.com/recaptcha)
