---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Používat k poskytování dynamické aktualizace AJAX | Microsoft Docs
author: microsoft
description: Krok 10 implementuje podporu pro přihlášeného uživatele k zasílání zpráv rysy jejich zájmu účastí večeři, pomocí integrované v rámci podrobností večeři přístup na základě Ajax na...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 7cea3ee2ec52261521941efac484e91a53f6310b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="use-ajax-to-deliver-dynamic-updates"></a>Používat k poskytování dynamické aktualizace AJAX
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Toto je krok 10 bezplatný [kurz aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , nevystavíte slabé stránky zabezpečení – prostřednictvím postup sestavení malá, ale dokončení, webové aplikace pomocí ASP.NET MVC 1.
> 
> Krok 10 implementuje podporu pro přihlášeného uživatele k zasílání zpráv rysy jejich zájmu účastí večeři, pomocí integrované v rámci stránce s podrobnostmi o večeři přístup na základě Ajax na.
> 
> Pokud používáte ASP.NET MVC 3, doporučujeme provedením [získávání spuštěna s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Hudba úložiště](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a>NerdDinner krok 10: Přijímá AJAX povolení RSVPs

Teď umožňuje implementovat podporu pro uživatele přihlášeného k RSVP jejich zájmu účastí večeři na. Jsme budete to umožňují přístup na základě AJAX integrovaná v rámci dané stránky večeři podrobnosti.

### <a name="indicating-whether-the-user-is-rsvpd"></a>Označující, zda je na které odpověděl uživatele

Uživatelé můžou navštívit */Dinners/podrobnosti / [id*] zobrazit podrobnosti o konkrétní večeři adresa URL:

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

Details() je implementována metoda akce takto:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

Naše prvním krokem k implementaci podpory zasílání zpráv rysy bude přidat metodu helper "IsUserRegistered(username)" naše objekt večeři (v rámci Dinner.cs třídu, kterou jsme vytvořili výše). Tato pomocná metoda vrátí hodnotu PRAVDA nebo NEPRAVDA v závislosti na tom, zda uživatel je aktuálně na které odpověděl večeře:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

Naše Details.aspx zobrazit šablonu k zobrazení odpovídající zprávu, která udává, zda je uživatel zaregistrován nebo není pro událost jsme můžete přidejte následující kód:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

A teď Pokud uživatel navštíví večeři, jsou registrované pro se zobrazí tato zpráva:

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

A při návštěvě večeři, že nejsou registrované budete najdete v tématu následující zpráva:

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a>Implementace metody akce registrace

Nyní Pojďme přidat funkce, které jsou nutné k povolení uživatelům k zasílání zpráv rysy večeře na stránce podrobností.

Chcete-li tuto funkci implementovat, vytvoříme novou třídu "RSVPController" pravým tlačítkem myši na adresář \Controllers a zvolením Add -&gt;příkazu nabídky řadiče.

Jsme budete implementovat metodu akce "Register" v rámci novou třídu RSVPController, která přebírá id večeře jako argument, načte příslušný objekt večeři, zkontroluje, pokud právě přihlášeného uživatele v seznamu uživatelů, kteří zaregistrovali pro něj a pokud není přidá objekt zasílání zpráv rysy pro ně:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

Všimněte si výše jak jednoduchým řetězcem se vrátí jako výstup metody akce. Jsme může vložených, tato zpráva v šabloně na zobrazení – ale vzhledem k tomu, že je proto malá právě použijeme pomocnou metodu Content() na základní třídy kontroleru a vrátí zprávu řetězec jako výše.

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a>Volání metody akce RSVPForEvent pomocí rozhraní AJAX

Použijeme AJAX k vyvolání metody akce registrace z našich zobrazení podrobností. Implementace to je velmi snadné. Nejprve přidáme dvě reference knihovny skriptu:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

První knihovny odkazuje na základní knihovny ASP.NET AJAX klientský skript. Tento soubor obsahuje základní funkce AJAX na straně klienta a je přibližně 24 kb velikostí (komprimované). Druhý knihovna obsahuje nástroj funkce, které integrovat s ASP.NET MVC předdefinované AJAX pomocné metody (které použijeme krátce).

Můžeme pak aktualizace zobrazení Kód šablony, kterou jsme přidali dříve tak, aby místo outputing zprávu "Je nejsou registrované pro tuto událost" jsme místo vykreslit odkaz při stisknutí. provádí volání AJAX, která volá metodu akce naše RSVPForEvent na zasílání zpráv rysy kontroleru a RSVPs uživatele:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

Pomocnou metodu Ajax.ActionLink() používá výše je integrovaný do technologie ASP.NET MVC a je podobný Html.ActionLink() pomocnou metodu s tím rozdílem, že namísto provádění standardní navigační umožňuje volání AJAX na metodu akce při kliknutí na odkaz. Vyšší jsme se volání metody akce "Register" na "Zasílání zpráv rysy" řadiči a předáním DinnerID jako parametr "id". Konečný parametr AjaxOptions jsme předávání označuje, že chceme trvat obsah vrátila z metody akce a aktualizovat HTML &lt;div&gt; elementu na stránce, jejíž id je "rsvpmsg".

A teď když uživatel prochází večeři nejsou registrované pro ještě se zobrazí odkaz na zasílání zpráv rysy pro ni:

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

Pokud kliknou na odkaz "Pro tuto událost zasílání zpráv rysy" budete provádění volání AJAX na metodu akce registrace na řadiči zasílání zpráv rysy, a po dokončení se zobrazí zprávu aktualizované jako níže:

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

Je skutečně lightweight šířku pásma sítě a provoz související se situací při provádění tohoto volání AJAX. Když uživatel klikne na odkaz "Pro tuto událost zasílání zpráv rysy", malé síťový požadavek HTTP POST přišla */Dinners/Register/1* adresa URL, která vypadá jako níže v drátové síti:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

A odpověď z našich metoda akce registrace je jednoduše:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

Toto volání lightweight je rychlá a bude fungovat i přes pomalé síti.

### <a name="adding-a-jquery-animation"></a>Přidání jQuery animace

Funkce AJAX jsme implementováno funguje dobře a rychlé. Někdy tomu může dojít tak rychle, ale, že uživatel nemusí Všimněte si, že odkaz zasílání zpráv rysy nahrazen novým textem. Chcete-li výsledek trochu zřejmější jsme přidat jednoduchou animaci k upozornění zpráva aktualizace.

Výchozí šablona projektu ASP.NET MVC zahrnuje jQuery – knihovna JavaScript vynikající (a velmi oblíbených) s otevřeným zdrojem, který taky podporuje Microsoft. jQuery poskytuje celou řadu funkcí, včetně dobrý model HTML DOM výběr a efekty knihovny.

Použít jQuery nejprve přidáme odkaz na skript k němu. Vzhledem k tomu, že budeme používat jQuery v rámci různých místech v rámci našeho webu, přidáme odkaz na skript v rámci naší soubor Site.master hlavní stránky tak, aby všechny stránky můžete používat.

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

*Tip: Ujistěte se, že jste nainstalovali opravu hotfix JavaScript intellisense pro VS 2008 SP1, která umožňuje bohatší podporu technologie intellisense pro soubory JavaScript (včetně jQuery). Můžete stáhnout z: http://tinyurl.com/vs2008javascripthotfix*

Kód napsaný pomocí JQuery často používá globální $ ()"metodu JavaScript, který načte jeden nebo více elementů HTML pomocí selektor šablon stylů CSS. Například <em>$("#rsvpmsg")</em> vybere libovolný prvek HTML s id rsvpmsg, zatímco <em>$(".something")</em> by vybrat všechny elementy s "něco co" šablon stylů CSS název třídy. Je také možné zapsat složitější dotazy jako "vrátí všechny zaškrtnuté přepínačů" pomocí dotazu pro výběr jako: <em>$("vstup [@type= přepínač] [@checked]")</em>.

Po dokončení výběru prvky, můžete volat metody na jejich provedení akce, jako je skrytí: *$("#rsvpmsg").hide();*

Pro náš scénář zasílání zpráv rysy budeme definovat jednoduchý funkce jazyka JavaScript s názvem "AnimateRSVPMessage", která vybere položku "rsvpmsg" &lt;div&gt; a animuje velikost jeho textového obsahu. Níže uvedeného kódu spustí malé text a pak příčiny pro zvýšení přes 400 milisekund období:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

Jsme můžete pak navázání této funkce JavaScript, která má být volána po naše volání AJAX úspěšně dokončí pomocí předání názvu naše Pomocná metoda Ajax.ActionLink() (prostřednictvím AjaxOptions "OnSuccess" události vlastnost):

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

A teď až po kliknutí na odkaz "Pro tuto událost zasílání zpráv rysy" a naše volání AJAX dokončí úspěšně, obsah zprávy odeslané zpět bude animace růst velké:

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

Kromě událost "OnSuccess", zpřístupní objekt AjaxOptions OnBegin OnFailure – a onComplete – události, které může zpracovat (spolu s celou řadu dalších vlastností a užitečné možnosti).

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a>Vyčištění - Refaktorovat out RSVP částečné zobrazení

Naše podrobnosti zobrazit šablonu spouští získat trochu dlouhý, které přesčasová znamená, že trochu těžší pochopit. K vylepšení čitelnosti kódu, můžeme nakonec vytvořením částečné zobrazení – RSVPStatus.ascx – zapouzdřující všechny kód zasílání zpráv rysy zobrazení pro naší stránce s podrobnostmi o.

Jsme to můžete udělat pravým tlačítkem na složku \Views\Dinners a pak vyberete Add -&gt;příkazu v nabídce zobrazení. Jsme si ho trvat objekt večeři jako jeho ViewModel silného typu. Jsme můžete pak zkopírujte a vložte obsah zasílání zpráv rysy z našich zobrazením Details.aspx do ní.

Když jsme tento krok, umožňuje taky vytvořit jinou částečné zobrazení – EditAndDeleteLinks.ascx -, který zapouzdřuje kód naše upravit a odstranit zobrazení odkaz. Také jsme budete mít trvat objekt večeři jako jeho ViewModel silného typu a zkopírujte a vložte logiky upravit a odstranit z našich zobrazením Details.aspx do ní.

Naše podrobnosti a zobrazit šablonu poté zahrnout pouze dvě volání metod Html.RenderPartial() v dolní části:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

Díky tomu kód čisticí ke čtení a údržbu.

### <a name="next-step"></a>Další krok

Nyní podíváme, jak jsme můžete použít i další AJAX a přidat podporu interaktivní mapování do naší aplikace.

> [!div class="step-by-step"]
> [Předchozí](secure-applications-using-authentication-and-authorization.md)
> [další](use-ajax-to-implement-mapping-scenarios.md)
