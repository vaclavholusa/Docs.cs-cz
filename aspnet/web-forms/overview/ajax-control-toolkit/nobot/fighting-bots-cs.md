---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
title: Boj s roboty (C#) | Dokumentace Microsoftu
author: wenz
description: Automatizované robotů sádra webové protokoly a další weby s nevyžádanou poštou, odesílání formulářů komentář bez nutnosti zásahu uživatele. Ovládací prvek NoBot v Con technologie ASP.NET AJAX...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0a1917e0-884a-4576-8e93-9ed660faae51
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
msc.type: authoredcontent
ms.openlocfilehash: 41e8fda5138e4a94e7b8c4af0a5c2bd68e50e9e1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402717"
---
<a name="fighting-bots-c"></a>Boj s roboty (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip) nebo [stahovat PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)

> Automatizované robotů sádra webové protokoly a další weby s nevyžádanou poštou, odesílání formulářů komentář bez nutnosti zásahu uživatele. Ovládací prvek NoBot v ASP.NET AJAX Control Toolkit může pomoct bojují těchto robotů.


## <a name="overview"></a>Přehled

Automatizované robotů sádra webové protokoly a další weby s nevyžádanou poštou, odesílání formulářů komentář bez nutnosti zásahu uživatele. Ovládací prvek NoBot v ASP.NET AJAX Control Toolkit může pomoct bojují těchto robotů.

## <a name="steps"></a>Kroky

Jeden běžný postup, se kterými porazíte robotů je použití CAPTCHAs zcela automatizovat veřejné Turing test zjistit počítače a od sebe lidí. Turing test byl původně test kde někdo museli rozhodovat, jestli je partnerem komunikace člověk nebo počítač. Na webu testu CAPTCHA obvykle obsahuje bitovou kopii s některé zkreslený písmena v něm. Cílem je, že může číst pouze lidských písmena v imagi, zatímco OCR algoritmy se nezdaří.

Existuje několik výhod a nevýhod tohoto přístupu diskusi o to je však nad rámec tohoto kurzu. Je však ovládacího prvku v ASP.NET AJAX Control Toolkit, který nabízí podobný přístup: `NoBot`. Je snazší překonat než testu CAPTCHA, ale je velmi snadno používá a tarify velmi dobře ve službě websites jako blogy, kde bude považován za úspěšné Pokud většinu nevyžádané pošty pokusy jsou zrušena, což `NoBot` ovládacího prvku můžete provést.

`NoBot` zachycuje postback aktuální webové formuláře ASP.NET, pokud je splněna alespoň jedna z těchto podmínek:

- Prohlížeči nepodaří vyřešit díl stavebnice JavaScript (například při deaktivaci JavaScript)
- Formulář pro rychlé odeslané uživatele
- IP adresa klienta odeslání formuláře příliš často v určitou dobu.

Za účelem ověření pro tyto podmínky `NoBot` ovládací prvek požaduje tyto atributy (všechny z nich volitelný):

- `ResponseMinimumDelaySeconds` minimální velikost sady sekund mezi jednotlivými zpětnými odesláními
- `CutoffWindowSeconds` Délka Časový interval, ve kterém jsou postbacků extenderu jedna IP adresa míry
- `CutoffMaximumInstances` maximální velikost sady sekund na časový interval

Následující kód požadavky tohoto aspoň 2 sekundy uplynout mezi jednotlivými zpětnými odesláními a, které existují pouze pět zpětného odeslání nebo nižší v rámci intervalu 30 sekund:

[!code-aspx[Main](fighting-bots-cs/samples/sample1.aspx)]

Potom jako obvykle, nezapomeňte zahrnout `ScriptManager` na stránce tak, aby je načtena knihovna ASP.NET AJAX a Control Toolkit je možné:

[!code-aspx[Main](fighting-bots-cs/samples/sample2.aspx)]

Protože většina kontrol `NoBot` dělá dojít na straně serveru, je potřeba zkontrolovat výsledek tyto ověření. To lze provést zavoláním `NoBot`společnosti `IsValid()` metody. Má jeden argument (jako `out` parametr /`ByRef` parametrů) typu `NoBotState`. Řetězcové vyjádření obsahuje příčinu selhání kontroly a `Valid` jinak. Následující kód vracel zprávu podle `NoBot`je výsledek:

[!code-aspx[Main](fighting-bots-cs/samples/sample3.aspx)]

Nakonec musíte formuláře pro odeslání a prvku popisku na výstup zprávu, a vy budete hotovi!

[!code-aspx[Main](fighting-bots-cs/samples/sample4.aspx)]

Při spouštění tohoto skriptu a deaktivovat JavaScript nebo odesláním formuláře v prvních dvou sekund nebo odesláním formuláře sedminásobně během 30 sekund, zobrazí se chybová zpráva. Ale pomocí tohoto ovládacího prvku obezřetně, protože pouze 90 95 % uživatelé mít JavaScript aktivovat, proto se nezdaří % 5 až 10 uživatelů `NoBot`v testu.


[![Tato chybová zpráva by mohla být způsobena robota](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)

Tato chybová zpráva by mohla být způsobena robota ([kliknutím ji zobrazíte obrázek v plné velikosti](fighting-bots-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](fighting-bots-vb.md)
