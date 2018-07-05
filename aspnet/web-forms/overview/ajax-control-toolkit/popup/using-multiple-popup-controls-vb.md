---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: Použití několika překryvných ovládacích prvků (VB) | Dokumentace Microsoftu
author: wenz
description: Zařízení extender PopupControl v sadou nástrojů AJAX Control Toolkit nabízí snadný způsob, jak aktivovat automaticky otevíraného okna, když se aktivuje jiný ovládací prvek. Je také možné použít m...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: accb9134cc355e0c042114873973bcbfe9a4a0a4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384558"
---
<a name="using-multiple-popup-controls-vb"></a>Použití několika překryvných ovládacích prvků (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)

> Zařízení extender PopupControl v sadou nástrojů AJAX Control Toolkit nabízí snadný způsob, jak aktivovat automaticky otevíraného okna, když se aktivuje jiný ovládací prvek. Je také možné použít více než jeden ovládací prvek popup na jedné stránce.


## <a name="overview"></a>Přehled

Zařízení extender PopupControl v sadou nástrojů AJAX Control Toolkit nabízí snadný způsob, jak aktivovat automaticky otevíraného okna, když se aktivuje jiný ovládací prvek. Je také možné použít více než jeden ovládací prvek popup na jedné stránce.

## <a name="steps"></a>Kroky

K aktivaci funkce technologie ASP.NET AJAX a Control Toolkit `ScriptManager` ovládací prvek je třeba umístit kdekoli na stránce (ale v rámci `<form>` element):

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

V dalším kroku přidáte panel, který slouží jako automaticky otevíraného okna. V aktuální scénář obsahuje panel `Calendar` ovládacího prvku. Pokud se chcete vyhnout aktualizuje stránku způsobené postbacků kalendáře, se zařadí na panelu v rámci `UpdatePanel` ovládacího prvku:

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

Stránka také obsahuje dvě textová pole. Pro každé pole kalendář automaticky otevírané okno se zobrazí po aktivaci do textového pole.

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

Rozšiřte každou z tato dvě textová pole se teď `PopupControlExtender`. `TargetControlID` Atribut obsahuje ID vázané na zařízení extender ovládacího prvku. `PopupControlID` Atribut obsahuje ID panelu automaticky otevíraného okna. V takovém případě obou zařízení Extender zobrazit stejný panel, ale jiné panely je to možné, jsou také.

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

Nyní pokaždé, když kliknete do textového pole, kalendář zobrazí pod polem, což vám umožní vybrat datum. (Načítání vybraného data zpět do textových polí se budeme v různých kurz.)


[![Když uživatel klikne do textového pole, zobrazí se v kalendáři](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)

Když uživatel klikne do textového pole, zobrazí se v kalendáři ([kliknutím ji zobrazíte obrázek v plné velikosti](using-multiple-popup-controls-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [další](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
