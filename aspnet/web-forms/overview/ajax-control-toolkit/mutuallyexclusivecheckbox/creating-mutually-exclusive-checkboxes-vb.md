---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: Vytváření vzájemně se vylučuje políček (VB) | Microsoft Docs
author: wenz
description: 'Když může vybrat pouze jeden ze sady možností, přepínače se obvykle používají. Nevýhodou, když je: Jakmile jeden ve skupině přepínače,...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: cdb93a080fb62cfdc7e3ff0604a1447e2bb99071
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874249"
---
<a name="creating-mutually-exclusive-checkboxes-vb"></a>Vytváření vzájemně se vylučuje políček (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)

> Když může vybrat pouze jeden ze sady možností, přepínače se obvykle používají. Nevýhodou, když je: Jakmile jeden ve skupině přepínače, není možné zrušte zaškrtnutí všech přepínačů. Zaškrtávací políčka může být kdykoli není zaškrtnuto, ale nejsou vzájemně vylučují. V tomto kurzu poskytuje nejlepší z obou přístupů: zaškrtávací políčka, které se vzájemně vylučují.


## <a name="overview"></a>Přehled

Když může vybrat pouze jeden ze sady možností, přepínače se obvykle používají. Nevýhodou, když je: Jakmile jeden ve skupině přepínače, není možné zrušte zaškrtnutí všech přepínačů. Zaškrtávací políčka může být kdykoli není zaškrtnuto, ale nejsou vzájemně vylučují. V tomto kurzu poskytuje nejlepší z obou přístupů: zaškrtávací políčka, které se vzájemně vylučují.

## <a name="steps"></a>Kroky

Ovládací prvek ASP.NET AJAX Toolkit obsahuje MutuallyExclusiveCheckBox rozšiřujícího objektu. To umožňuje programátorům přiřadit název skupiny libovolný zaškrtávací políčko (`Key` atributu). Z všechna zaškrtávací políčka v rámci stejné skupiny může být pouze jeden vybraný najednou.

Začněme ukládání dvě zaškrtávací políčka na novou stránku ASP.NET. Může být více, ale dvě z nich stačí k předvedení se zásadou:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

Pro zaškrtnutí obou políček musíte být na stránce umístit MutuallyExclusiveCheckBoxExtender ovládacího prvku. Oba atributy klíč musí mít stejnou hodnotu, stejně jako hodnota, kterou atributy prvků HTML přepínač tlačítko musí být identické a označují skupinu, ke které patří. Vlastnost TargetControlID body rozšiřujícího objektu ID zaškrtávacího políčka.

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

Nakonec zahrnout ASP.NET AJAX `ScriptManager` které je požadované pro všechny elementy sady nástrojů ovládacího prvku ASP.NET AJAX:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

Uložte a spuštění stránky: můžete zkontrolovat a zrušit zaškrtnutí obou políček, ale současně lze obě políčka zkontrolovat.


[![Současně lze zkontrolovat pouze jedno zaškrtávací políčko.](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)

Může být pouze jeden zaškrtnuto současně ([Kliknutím zobrazit obrázek v plné velikosti](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](creating-mutually-exclusive-checkboxes-cs.md)
