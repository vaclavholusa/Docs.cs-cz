---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
title: Provádění několik animací ve stejnou dobu (VB) | Microsoft Docs
author: wenz
description: V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Umožňuje spustit severa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2469f7ea-1489-42fb-a8e1-414c90141ce9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
msc.type: authoredcontent
ms.openlocfilehash: 823764abd4444b5cb8d9bc6e8e8ed34d6f670310
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879436"
---
<a name="executing-several-animations-at-the-same-time-vb"></a>Provádění několik animací ve stejnou dobu (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2VB.pdf)

> V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Umožňuje spustit několik animací paralelní způsobem.


## <a name="overview"></a>Přehled

V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Umožňuje spustit několik animací paralelní způsobem.

## <a name="steps"></a>Kroky

První řadě zahrnují `ScriptManager` na stránce; potom knihovny ASP.NET AJAX je načtena, aby bylo možné použít Toolkitu:

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample1.aspx)]

Animace se použijí na panel textu, který vypadá takto:

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample2.aspx)]

Související třídy CSS pro panel definovat barvu pozadí dobrý a také nastavit pevnou šířku pro panel:

[!code-css[Main](executing-several-animations-at-the-same-time-vb/samples/sample3.css)]

Poté, přidejte `AnimationExtender` na stránku, poskytuje `ID`, `TargetControlID` atribut a povinný údaj `runat="server"`:

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample4.aspx)]

V rámci `<Animations>` uzlu, použijte `<OnLoad>` ke spuštění animací, jakmile byla úplným načtením stránky. Obecně platí `<OnLoad>` přijímá pouze jeden animace. Rozhraní framework animace umožňuje připojení k několika animace do jednoho pomocí `<Parallel>` elementu. Všechny animace v rámci `<Parallel>` jsou spouštěny ve stejnou dobu.

Tady je je možné kód pro `AnimationExtender` řízení, pozvolného vysouvání a změna velikosti panelu ve stejnou dobu:

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample5.aspx)]

A skutečně: při spuštění tohoto skriptu se zobrazí na panelu a potom změní (více než threefolding svou šířku a halfing výšku) a setmívá ve stejnou dobu.


[![Na panelu je pozvolného vysouvání a změna velikosti (včetně jeho obsahu, díky modul vykreslování prohlížeče)](executing-several-animations-at-the-same-time-vb/_static/image2.png)](executing-several-animations-at-the-same-time-vb/_static/image1.png)

Na panelu je pozvolného vysouvání a změna velikosti (včetně jeho obsahu, díky modul vykreslování prohlížeče) ([Kliknutím zobrazit obrázek v plné velikosti](executing-several-animations-at-the-same-time-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](adding-animation-to-a-control-vb.md)
> [další](executing-several-animations-after-each-other-vb.md)
