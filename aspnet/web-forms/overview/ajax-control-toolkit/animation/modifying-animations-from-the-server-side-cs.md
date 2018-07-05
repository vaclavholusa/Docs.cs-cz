---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
title: Úpravy animací na straně serveru (C#) | Dokumentace Microsoftu
author: wenz
description: Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Animace může také...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b0abec39-a1c9-422d-ba9a-ef16f6185af8
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
msc.type: authoredcontent
ms.openlocfilehash: 83ce54b1cd2c226db36be75f61321a0fb710e0ca
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376659"
---
<a name="modifying-animations-from-the-server-side-c"></a>Úpravy animací na straně serveru (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)

> Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Animace nejspíš se změní taky na straně serveru


## <a name="overview"></a>Přehled

Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Animace nejspíš se změní taky na straně serveru

## <a name="steps"></a>Kroky

Za prvé, zahrnout `ScriptManager` na stránce; potom technologie ASP.NET AJAX je načíst knihovnu, což umožňuje použití Control Toolkit:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample1.aspx)]

Animace se použijí pro panel text, který vypadá takto:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample2.aspx)]

V přidružené třídy šablony stylů CSS pro panel definovat barvu pozadí nice a také nastavit Pevná šířka panelu:

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample3.css)]

Zbytek kódu běží na straně serveru a nepoužívá značky Místo toho používá kód k vytvoření `AnimationExtender` ovládacího prvku:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample4.aspx)]

Ale Control Toolkit aktuálně neposkytuje přístup rozhraní API k vytvoření jednotlivých animace. Je však možné nastavit `AnimationExtender`animace vlastností na řetězec obsahující kód XML, použít při přiřazování animací deklarativně. Chcete-li vytvořit XML, který nesmí obsahovat `<Animations>` element můžete použít XML rozhraní .NET Framework podporují, nebo jako v následujícím kódu, stačí zadat řetězec:

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample5.css)]

Nakonec přidejte `AnimationExtender` ovládací prvek na aktuální stránku v rámci `<form runat="server">` elementu, ujistěte se, že animaci je součástí a spustí:

[!code-html[Main](modifying-animations-from-the-server-side-cs/samples/sample6.html)]


[![Animace se vytvoří pomocí kódu na straně serveru C# /VB](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)

Animace se vytvoří pomocí kódu na straně serveru C# /VB ([kliknutím ji zobrazíte obrázek v plné velikosti](modifying-animations-from-the-server-side-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](triggering-an-animation-in-another-control-cs.md)
> [další](executing-animations-using-client-side-code-cs.md)
