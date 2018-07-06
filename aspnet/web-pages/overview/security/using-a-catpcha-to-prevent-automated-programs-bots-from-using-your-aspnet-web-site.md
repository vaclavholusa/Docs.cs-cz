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
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>Použití testu CAPTCHA pro roboty zabránit v používání vašich Razor rozhraní ASP.NET Web) webu
====================
podle [Microsoft](https://github.com/microsoft)

> Tento článek vysvětluje, jak pomocí nástroje ReCaptcha (v rámci bezpečnostních opatření) automatizovaným programům (robotům) zabránit v provádění úloh na webu rozhraní ASP.NET Web Pages (Razor).
> 
> **Co se dozvíte:** 
> 
> - Jak přidat na váš web CAPTCHA test.
> 
> Toto jsou funkce technologie ASP.NET v následujícím článku:
> 
> - `ReCaptcha` Pomocné rutiny.
> 
> > [!NOTE]
> > Informace v tomto článku se vztahují na 1.0 webových stránek ASP.NET a Web Pages 2.


## <a name="about-captchas"></a>O CAPTCHAs

Kdykoli umožňují uživatelům registraci na vašem webu nebo jenom zadat název a adresu URL (například pro komentáře blogu), může se zobrazit zahlcení falešné názvy. Tyto jsou často zanechaný automatizovaným programům (robotům), které se pokoušejí necháte adresy URL v každému webu, který může najít. (Běžné motivace je publikovat adresy URL produktů pro prodej.)

Může pomoct, ujistěte se, že uživatel je skutečná osoba a není počítačový program pomocí *test CAPTCHA* k ověření uživatelů při registraci nebo jinak zadejte své jméno a lokality. Test CAPTCHA. zkratka pro plně automatizované veřejné Turing test zjistit počítače a od sebe lidí. Je testu CAPTCHA *– výzva odezva* test ve kterém je uživatel vyzván k provedení nějaké akce, který je jednoduché pro osobu, která chcete, ale obtížné pro automatizované program udělat. Nejběžnějším typem test CAPTCHA. je jedním kde zobrazit některé zkreslený písmena a vyzváni k zadávání. (Narušení by měl být obtížné pro roboty k dešifrování písmena.)

## <a name="adding-a-recaptcha-test"></a>Přidání nástroje ReCaptcha testu

Stránek v ASP.NET, můžete použít `ReCaptcha` pomocné rutiny pro vykreslení testu CAPTCHA, která je založená na službě nástroje ReCaptcha ([http://recaptcha.net](http://recaptcha.net)). `ReCaptcha` Pomocné rutiny zobrazí obrázek ze dvou zkreslený slov, která uživatelé mají k zadání správně předtím, než se ověří na stránce. Odpověď uživatele je potvrzen v ReCaptcha.Net služby.

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. Zaregistrujte svůj web najdete tady ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Po dokončení registrace, zobrazí se veřejný klíč a soukromý klíč.
2. Přidejte knihovnu ASP.NET Web Helpers na váš web, jak je popsáno v [instalace pomocné rutiny na webu technologie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), pokud jste tak již neučinili.
3. Pokud ještě nemáte  *\_AppStart.cshtml* souboru, v kořenové složce webu vytvořte soubor s názvem  *\_AppStart.cshtml*.
4. Přidejte následující `Recaptcha` nastavení pomocné rutiny  *\_AppStart.cshtml* souboru: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. Nastavte `PublicKey` a `PrivateKey` vlastností použitím vlastní veřejné a soukromé klíče.
6. Uložit  *\_AppStart.cshtml* soubor a zavřete ho.
7. V kořenové složce webu, vytvořte novou stránku s názvem *Recaptcha.cshtml*.
8. Nahraďte existující obsah následujícím kódem: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. Spustit *Recaptcha.cshtml* stránku v prohlížeči. Pokud `PrivateKey` je hodnota platná, na stránce se zobrazí ovládací prvek nástroje ReCaptcha a tlačítko. Pokud jste nenastavili klíče globálně v  *\_AppStart.html*, na stránce zobrazí chybu. 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. Zadejte slova pro test. Pokud předáte testovací nástroje ReCaptcha, zobrazí se příslušná zpráva. V opačném případě se zobrazí chybová zpráva a nástroje ReCaptcha ovládacího prvku se zobrazí znovu.

> [!NOTE]
> Pokud je počítač připojen k doméně, která používá proxy server, může být nutné nakonfigurovat `defaultproxy` elementu *Web.config* souboru. Následující příklad ukazuje *Web.config* souboru `defaultproxy` element nakonfigurované tak, aby služba nástroje ReCaptcha pracovat.
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Další prostředky


- [Přizpůsobení chování v celém webu pro weby stránky technologie ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Lokality nástroje ReCaptcha](https://www.google.com/recaptcha)
