---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: "Úprava animace z na straně serveru (VB) | Microsoft Docs"
author: wenz
description: "V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Animací může také..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: c5b23cce529be24157a8a3f9136de7ad7bafc1ea
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="modifying-animations-from-the-server-side-vb"></a>Úprava animace z na straně serveru (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)

> V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Animací pravděpodobně se změní také na straně serveru


## <a name="overview"></a>Přehled

V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Animací pravděpodobně se změní také na straně serveru

## <a name="steps"></a>Kroky

První řadě zahrnují `ScriptManager` na stránce; potom knihovny ASP.NET AJAX je načtena, aby bylo možné použít Toolkitu:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

Animace se použijí na panel textu, který vypadá takto:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

Související třídy CSS pro panel definovat barvu pozadí dobrý a také nastavit pevnou šířku pro panel:

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

Zbytek kód běží na straně serveru a nepoužívá značek; Místo toho použije k vytvoření kódu `AnimationExtender` ovládacího prvku:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

Ale Toolkitu aktuálně neposkytuje rozhraní API pro přístup k vytvoření jednotlivých animace. Je však možné nastavit `AnimationExtender`vlastnost animací na řetězec obsahující kód XML používá při přiřazování animací deklarativně. Chcete-li vytvořit XML, který nesmí obsahovat `<Animations>` element můžete použít rozhraní .NET Framework XML podporují, nebo jako v následujícím kódu, stačí zadat řetězec:

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

Nakonec přidejte `AnimationExtender` v řízení na aktuální stránku, `<form runat="server">` elementu, a ujistěte se, že animace je součástí a spustí:

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]


[![Animace je vytvořený pomocí kódu na straně serveru C# / VB.](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)

Animace je vytvořený pomocí kódu na straně serveru C# / VB. ([Kliknutím zobrazit obrázek v plné velikosti](modifying-animations-from-the-server-side-vb/_static/image3.png))

>[!div class="step-by-step"]
[Předchozí](triggering-an-animation-in-another-control-vb.md)
[další](executing-animations-using-client-side-code-vb.md)
