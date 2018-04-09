---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
title: Úprava animace z na straně serveru (C#) | Microsoft Docs
author: wenz
description: V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Animací může také...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b0abec39-a1c9-422d-ba9a-ef16f6185af8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
msc.type: authoredcontent
ms.openlocfilehash: 6946875552c885ffb1f2a2eb7e728b85d7dd3973
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="modifying-animations-from-the-server-side-c"></a>Úprava animace z na straně serveru (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)

> V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Animací pravděpodobně se změní také na straně serveru


## <a name="overview"></a>Přehled

V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Animací pravděpodobně se změní také na straně serveru

## <a name="steps"></a>Kroky

První řadě zahrnují `ScriptManager` na stránce; potom knihovny ASP.NET AJAX je načtena, aby bylo možné použít Toolkitu:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample1.aspx)]

Animace se použijí na panel textu, který vypadá takto:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample2.aspx)]

Související třídy CSS pro panel definovat barvu pozadí dobrý a také nastavit pevnou šířku pro panel:

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample3.css)]

Zbytek kód běží na straně serveru a nepoužívá značek; Místo toho použije k vytvoření kódu `AnimationExtender` ovládacího prvku:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample4.aspx)]

Ale Toolkitu aktuálně neposkytuje rozhraní API pro přístup k vytvoření jednotlivých animace. Je však možné nastavit `AnimationExtender`vlastnost animací na řetězec obsahující kód XML používá při přiřazování animací deklarativně. Chcete-li vytvořit XML, který nesmí obsahovat `<Animations>` element můžete použít rozhraní .NET Framework XML podporují, nebo jako v následujícím kódu, stačí zadat řetězec:

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample5.css)]

Nakonec přidejte `AnimationExtender` v řízení na aktuální stránku, `<form runat="server">` elementu, a ujistěte se, že animace je součástí a spustí:

[!code-html[Main](modifying-animations-from-the-server-side-cs/samples/sample6.html)]


[![Animace je vytvořený pomocí kódu na straně serveru C# / VB.](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)

Animace je vytvořený pomocí kódu na straně serveru C# / VB. ([Kliknutím zobrazit obrázek v plné velikosti](modifying-animations-from-the-server-side-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](triggering-an-animation-in-another-control-cs.md)
> [další](executing-animations-using-client-side-code-cs.md)
