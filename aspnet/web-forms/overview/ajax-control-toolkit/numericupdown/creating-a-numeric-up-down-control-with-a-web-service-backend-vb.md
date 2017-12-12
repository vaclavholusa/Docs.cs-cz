---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
title: "Vytvoření číselný nahoru/dolů ovládacího prvku pomocí webové služby back-end (VB) | Microsoft Docs"
author: wenz
description: "Místo uživatele zadejte hodnotu do zaškrtávacího políčka, by mohly být číselný nahoru/dolů ovládací prvek (který existuje ve Windows a jinými operačními systémy) jako další c..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: afa59dfa-fef1-43d3-8fdd-aea3be36ed3c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
msc.type: authoredcontent
ms.openlocfilehash: 5ceefd6c18761c2abe3f3a4298d340642a0951d6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-vb"></a>Vytvoření číselný nahoru/dolů ovládacího prvku pomocí webové služby back-end (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)

> Místo uživatele zadejte hodnotu do zaškrtávacího políčka, by mohly být číselný nahoru/dolů ovládací prvek (který existuje ve Windows a jinými operačními systémy) jako další možnost. Ve výchozím nastavení ovládacího prvku NumericUpDown vždy zvýší nebo sníží hodnotu 1, ale webová služba prokáže větší flexibilitu.


## <a name="overview"></a>Přehled

Místo uživatele zadejte hodnotu do zaškrtávacího políčka, by mohly být číselný nahoru/dolů ovládací prvek (který existuje ve Windows a jinými operačními systémy) jako další možnost. Ve výchozím nastavení `NumericUpDown` řízení vždy zvýší nebo sníží hodnotu 1, ale webová služba prokáže větší flexibilitu.

## <a name="steps"></a>Kroky

Obsahuje sadu ovládacího prvku ASP.NET AJAX `NumericUpDown` rozšiřujícího objektu, který automaticky přidá dvě tlačítka do textového pole: jeden pro zvýšení jeho hodnoty, jeden pro snížení ho. Ovládací prvek ale také podporuje volání webové služby (nebo volání metody stránky). Vždy, když nahoru nebo dolů tlačítko po kliknutí na, JavaScript, kód se připojí k webovému serveru a provede metodu existuje. Podpis metody je následující:

[!code-vb[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample1.vb)]

`current` Argument je aktuální hodnota v textovém poli; `tag` atribut je další kontext data, která lze nastavit jako vlastnost `NumericUpDown` rozšiřujícího objektu (ale není povinen).

Tato ukázka číselný nahoru/dolů řízení pouze povolit hodnoty, které jsou zajišťuje dva: 1, 2, 4, 8, 16, 32, 64 a tak dále. Proto musí metoda spustit, když chce uživatel zvýšit hodnotu double původní hodnoty; jiné metody musí dělení hodnoty ve dvou. Tady je proto dokončení webové služby:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample2.aspx)]

Nakonec vytvořte novou stránku ASP.NET. Obvyklým způsobem, budete potřebovat `ScriptManager` řízení, `TextBox` řízení a `NumericUpDownExtender` ovládacího prvku. K tomu je nutné zadat informace o webové služby:

- `ServiceDownMethod`název na nabídku webové metody nebo stránky – metoda
- `ServiceDownPath`Cesta k webové službě s dolů metody služby; vynechat, pokud používáte metodu stránky
- `ServiceUpMethod`Název nahoru webové metody nebo stránky – metoda
- `ServiceUpPath`Cesta k webové službě s aktuálním metody služby; vynechat, pokud používáte metodu stránky

Tady je kompletní kód pro stránky:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample3.aspx)]

Pokud spustíte stránky, Všimněte si, jak se hodnota v textovém poli vždy zdvojnásobí, klikněte na tlačítko horní, a je oproti jiným poloviční po kliknutí na tlačítko nižší.


[![Zobrazí pouze čísla, které jsou power 2](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)

Zobrazí pouze čísla, které jsou power 2 ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))

>[!div class="step-by-step"]
[Předchozí](creating-a-numeric-up-down-control-with-a-web-service-backend-cs.md)
