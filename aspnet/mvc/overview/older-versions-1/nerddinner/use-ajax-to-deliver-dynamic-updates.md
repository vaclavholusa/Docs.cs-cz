---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Použití jazyka AJAX k dynamickým aktualizacím | Dokumentace Microsoftu
author: microsoft
description: Krok 10 implementuje podporu pro přihlášeného uživatele RSVP jejich zájmu o účast na akci dinner, pomocí přístupu na základě Ajax integrovaný v rámci podrobností dinner...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 9f11c4c15c0ac9bab8d53b18a4e07be4b864b2c7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825198"
---
<a name="use-ajax-to-deliver-dynamic-updates"></a>Použití jazyka AJAX k dynamickým aktualizacím
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Toto je krok 10 bezplatného [kurz vývoje aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , který procházení procházení po tom, jak sestavit malý, ale bylo možné provést, webové aplikace pomocí ASP.NET MVC 1.
> 
> Krok 10 implementuje podporu pro přihlášeného uživatele RSVP jejich zájmu o účast na akci dinner, pomocí přístupu na základě Ajax integrovaný v rámci stránce s podrobnostmi o dinner.
> 
> Pokud používáte ASP.NET MVC 3, doporučujeme je provést [získávání začít s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a>NerdDinner krok 10: Přijímá AJAX povolení RSVPs

Teď můžeme implementovat podporu pro uživatele přihlášený potvrďte svou účast ještě jejich zájmu o účast webu dinner. Jsme vám umožňují přístup na základě AJAX integrovaný v rámci stránce s podrobnostmi o dinner.

### <a name="indicating-whether-the-user-is-rsvpd"></a>Označující, zda je uživatel RSVP'd

Uživatelé můžou navštívit */Dinners/podrobnosti / [id*] adresa URL, které chcete zobrazit podrobnosti o konkrétní společnosti dinner:

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

Details() implementované metody akce takto:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

Naše prvním krokem k implementaci podpory RSVP budete moct přidat metodu helper "IsUserRegistered(username)" naše společnost Dinner objektu (v rámci Dinner.cs částečné třídy, které jsme vytvořili výše). Tato pomocná metoda vrátí hodnotu PRAVDA nebo NEPRAVDA v závislosti na tom, jestli je uživatel aktuálně RSVP'd pro společnosti Dinner:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

Naše Details.aspx zobrazit šablonu k zobrazení odpovídající zprávu označující, zda je uživatel zaregistrován nebo není pro událost jsme můžete přidejte následující kód:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

A teď když uživatel navštíví Dinner jsou registrované pro zobrazí tato zpráva:

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

A při návštěvě Dinner nejsou zaregistrovány pro zobrazí následující zpráva:

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a>Implementace Registrovací metoda akce

Přidejme funkci nutná pro povolení uživatelů ze stránky podrobností na reakce večeře nyní.

K provedení vytvoříme novou třídu "RSVPController" pravým tlačítkem myši na adresáři \Controllers a zvolením přidat -&gt;příkazu nabídky Kontroleru.

Jsme budete implementovat metodu akce "Register" v rámci nové RSVPController třídu, která přijímá id večeře jako argument, načte příslušný objekt Dinner kontroluje, zda přihlášený uživatel je aktuálně v seznamu uživatelů, kteří zaregistrovali, a pokud není přidá objekt RSVP pro ně:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

Všimněte si, že nad jak jednoduchým řetězcem se vrátí jako výstup metody akce. Jsme může vložit, tato zpráva v rámci zobrazení šablony – ale protože je to small právě použijeme Content() pomocnou metodu na základní třídu kontroleru a vrátit zprávu řetězce, jako je výše.

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a>Volání metody akce RSVPForEvent pomocí rozhraní AJAX

Použijeme AJAX k vyvolání metody akce registrace z našich zobrazení podrobností. Implementace to je poměrně snadné. Nejdřív přidáme dva odkazy na knihovnu skriptu:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

První knihovny odkazuje na základní knihovny ASP.NET AJAX na straně klienta skriptů. Tento soubor je přibližně 24 kb (komprimované) a obsahuje základní funkce jazyka AJAX na straně klienta. Druhý knihovna obsahuje funkce nástrojů, které se integrují s ASP.NET MVC integrované AJAX pomocné metody (které použijeme za chvíli).

Můžeme pak aktualizace kód zobrazit šablonu, kterou jsme přidali dříve, tak, aby místo outputing zprávu "Jste se zaregistrovali pro tuto událost", můžeme místo vykreslení odkaz, který při vložení provede volání AJAX, která volá metodu naše RSVPForEvent akce v kontroleru reakce a RSVPs uživatele:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

Pomocná metoda Ajax.ActionLink() využité nad je integrovaná do ASP.NET MVC a je podobný Html.ActionLink() pomocnou metodu s tím rozdílem, takže nemusíte provádět standardní navigace je volání AJAX na metodu akce po kliknutí na odkaz. Výše jsme volání metody akce "Register" u "RSVP" kontroleru a předáním DinnerID jako parametr "id". Poslední parametr AjaxOptions jsme prochází označuje, že má být obsah vrácené z metody akce a aktualizovat kód HTML &lt;div&gt; element na stránce, jehož id je "rsvpmsg".

A teď když uživatel prochází večeři nejsou registrovány pro ještě zobrazí odkaz na RSVP pro něj:

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

Kliknou na odkaz "RSVP pro tuto událost", budete-li volání AJAX na metodu akce registru na řadiči RSVP a po dokončení se zobrazí aktualizovaná zpráva podobná níže uvedenému příkladu:

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

Šířka pásma sítě a provoz používané při kontrole toto volání AJAX je opravdu jednoduché. Když uživatel klikne na odkaz "RSVP pro tuto událost", je provedené malé síťový požadavek HTTP POST */Dinners/Register/1* adresu URL, která vypadá jako níže na lince:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

A jednoduše je odpověď na naše metoda akce registrace:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

Toto jednoduché volání je rychlý a budou fungovat i v pomalé síti.

### <a name="adding-a-jquery-animation"></a>Přidání jQuery animace

Funkce AJAX, která jsme implementovali funguje dobře a rychlé. V některých případech může se stát tak rychle, ale, že uživatel nemusí Všimněte si, že odkaz RSVP se nahradil novým textem. Abyste viděli výsledek trochu zřejmé, můžeme přidat jednoduché animace k přitažení pozornosti ke zpráva aktualizace.

Výchozí šablona projektu ASP.NET MVC zahrnuje jQuery – skvělé (a velmi populární) open source knihovna jazyka JavaScript, který je také podporován společností Microsoft. jQuery poskytuje řadu funkcí, včetně vylepšení vzhledu knihovny výběr a důsledky modelu DOM jazyka HTML.

Použití jQuery nejprve přidáme na ni odkaz skriptu. Vzhledem k tomu, že budeme používat jQuery v rámci různých místech v rámci našeho webu, přidáme odkaz na skript v rámci naší soubor Site.master předlohové stránky tak, aby ho může použít všechny stránky.

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

*Tip: Ujistěte se, že je nainstalována oprava hotfix technologie intellisense jazyka JavaScript pro VS 2008 SP1, která umožňuje bohatší podporu technologie intellisense pro soubory JavaScriptu (včetně jQuery). Stáhněte si ho: http://tinyurl.com/vs2008javascripthotfix*

Kód napsané s využitím JQuery často používá globální "$ ()" metodu JavaScript, který načte jeden nebo více elementů HTML pomocí selektor šablon stylů CSS. Například <em>$("#rsvpmsg")</em> vybere libovolný prvek HTML s id rsvpmsg, zatímco <em>$(".something")</em> by vybrat všechny elementy s "něco co uživatel" šablon stylů CSS název třídy. Můžete taky psát složitější dotazy jako "vrácení všech přepínačů checked" pomocí selektoru dotazu jako: <em>$("vstupu [@type= přepínač] [@checked]")</em>.

Jakmile vyberete prvky, může volat metody na nich provádět akce, jako je skrytí: *$("#rsvpmsg").hide();*

Pro náš scénář RSVP budeme definovat jednoduchou funkci JavaScriptu s názvem "AnimateRSVPMessage", který vybere "rsvpmsg" &lt;div&gt; a animuje velikosti svého obsahu textu. Níže uvedeného kódu spustí text malé a poté způsobí, že ho chcete zvýšit nad časový rámec 400 milisekund:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

Jsme můžete pak navázání tato funkce JavaScriptu, která se má volat po úspěšném dokončení našich volání jazyka AJAX předáním názvu naše Ajax.ActionLink() pomocnou metodu (prostřednictvím AjaxOptions "OnSuccess" události vlastnost):

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

A teď při kliknutí na odkaz "RSVP pro tuto událost" a naše volání jazyka AJAX dokončí úspěšně, obsah zprávy zpět se animace a rozvíjet velké:

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

Kromě toho, že událost "OnSuccess" objekt AjaxOptions poskytuje OnBegin OnFailure a OnComplete události, které dokáže zpracovat (spolu s celou řadu dalších vlastností a užitečné možnosti).

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a>Čištění – Refaktorujte si potvrďte svou účast ještě částečné zobrazení

Naše podrobnosti zobrazit šablonu spouští získat mírně dlouhý, které přesčas bude obtížnější trochu pochopit. Chcete-li zlepšit čitelnost kódu, Pojďme nakonec vytvořit částečné zobrazení – RSVPStatus.ascx – zapouzdření veškerých RSVP zobrazit kód pro naší stránce s podrobnostmi o.

Můžeme to udělat tak, že pravým tlačítkem na složku \Views\Dinners a následným výběrem možnosti Přidat -&gt;zobrazit příkaz nabídky. Musíme ji získat objekt Dinner jako jeho ViewModel silného typu. Jsme pak zkopírovat a vložit obsah RSVP z našich Details.aspx nabízí pohled na to.

Po jsme provedli, který, můžeme také vytvořit další částečné zobrazení – EditAndDeleteLinks.ascx –, který zapouzdřuje naše úpravu a odstranění odkazu zobrazit kód. Také budete moct získat objekt Dinner jako jeho ViewModel silného typu a upravit a odstranit logiku z našich Details.aspx nabízí pohled na to, kopírování a vkládání.

Naše podrobnosti zobrazení šablony může pak obsahuje pouze dva Html.RenderPartial() volání metody v dolní části:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

Díky tomu kód čistější ke čtení a udržovat.

### <a name="next-step"></a>Dalším krokem

Nyní Podívejme se na jak můžeme ještě více použití jazyka AJAX a přidává interaktivní mapování na naši aplikaci.

> [!div class="step-by-step"]
> [Předchozí](secure-applications-using-authentication-and-authorization.md)
> [další](use-ajax-to-implement-mapping-scenarios.md)
