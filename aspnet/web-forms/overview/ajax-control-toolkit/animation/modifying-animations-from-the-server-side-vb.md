---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: Úpravy animací na straně serveru (VB) | Dokumentace Microsoftu
author: wenz
description: Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Animace může také...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: ef32c8f4846b18f11d816a64a3e4292b67b232e9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754286"
---
<a name="modifying-animations-from-the-server-side-vb"></a>Úpravy animací na straně serveru (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)

> Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Animace nejspíš se změní taky na straně serveru


## <a name="overview"></a>Přehled

Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Animace nejspíš se změní taky na straně serveru

## <a name="steps"></a>Kroky

Za prvé, zahrnout `ScriptManager` na stránce; potom technologie ASP.NET AJAX je načíst knihovnu, což umožňuje použití Control Toolkit:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

Animace se použijí pro panel text, který vypadá takto:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

V přidružené třídy šablony stylů CSS pro panel definovat barvu pozadí nice a také nastavit Pevná šířka panelu:

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

Zbytek kódu běží na straně serveru a nepoužívá značky Místo toho používá kód k vytvoření `AnimationExtender` ovládacího prvku:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

Ale Control Toolkit aktuálně neposkytuje přístup rozhraní API k vytvoření jednotlivých animace. Je však možné nastavit `AnimationExtender`animace vlastností na řetězec obsahující kód XML, použít při přiřazování animací deklarativně. Chcete-li vytvořit XML, který nesmí obsahovat `<Animations>` element můžete použít XML rozhraní .NET Framework podporují, nebo jako v následujícím kódu, stačí zadat řetězec:

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

Nakonec přidejte `AnimationExtender` ovládací prvek na aktuální stránku v rámci `<form runat="server">` elementu, ujistěte se, že animaci je součástí a spustí:

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]


[![Animace se vytvoří pomocí kódu na straně serveru C# /VB](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)

Animace se vytvoří pomocí kódu na straně serveru C# /VB ([kliknutím ji zobrazíte obrázek v plné velikosti](modifying-animations-from-the-server-side-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](triggering-an-animation-in-another-control-vb.md)
> [další](executing-animations-using-client-side-code-vb.md)
