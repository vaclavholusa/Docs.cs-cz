---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
title: Použití ovládacího prvku ModalPopup v ovládacím prvku Repeater (C#) | Dokumentace Microsoftu
author: wenz
description: Ovládací prvek ovládacího prvku ModalPopup v sadou nástrojů AJAX Control Toolkit nabízí jednoduchý způsob, jak vytvořit modální místní nabídky pomocí znamená, že na straně klienta. Je také možné použít tuto sml...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: d686d84a-1c58-492e-8a77-3eb5a0cfe918
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 72d0f16d22911d867f9a91faf273e236453e7b3a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833621"
---
<a name="using-modalpopup-with-a-repeater-control-c"></a>Použití ovládacího prvku ModalPopup v ovládacím prvku Repeater (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.cs.zip) nebo [stahovat PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2CS.pdf)

> Ovládací prvek ovládacího prvku ModalPopup v sadou nástrojů AJAX Control Toolkit nabízí jednoduchý způsob, jak vytvořit modální místní nabídky pomocí znamená, že na straně klienta. Je také možné použít tento ovládací prvek v rámci prvku repeater.


## <a name="overview"></a>Přehled

Ovládací prvek ovládacího prvku ModalPopup v sadou nástrojů AJAX Control Toolkit nabízí jednoduchý způsob, jak vytvořit modální místní nabídky pomocí znamená, že na straně klienta. Je také možné použít tento ovládací prvek v rámci prvku repeater.

## <a name="steps"></a>Kroky

Za prvé zdroj dat je povinný. Tato ukázka používá databázi AdventureWorks a Microsoft SQL Server 2005 Express Edition. Databáze je volitelná součást instalace sady Visual Studio (včetně express edition) a jsou také dostupné jako samostatný soubor ke stažení v rámci [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Databáze AdventureWorks je součástí sad SQL Server 2005 ukázky a Sample Databases (stáhnout na [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Nejjednodušší způsob, jak nastavit databázi je použít Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) a připojit `AdventureWorks.mdf` databázový soubor. V tomto příkladu předpokládáme, že název instance systému SQL Server 2005 Express Edition `SQLEXPRESS` a je umístěn ve stejném počítači jako webový server; to je taky výchozí nastavení. Pokud vaše nastavení se liší, je nutné upravit informace o připojení pro databázi. K aktivaci funkce technologie ASP.NET AJAX a Control Toolkit `ScriptManager` ovládací prvek je třeba umístit kdekoli na stránce (ale v rámci `<form>` element):

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample1.aspx)]

Zadejte zdroj dat na stránku. Chcete-li použít omezené množství dat, vybereme pouze prvních pět záznamů v tabulce dodavatele databáze AdventureWorks. Pokud používáte Pomocníka s nastavením Visual Studio k vytvoření zdroje dat, mějte na paměti, že chyby v aktuální verzi předponu názvu tabulky (`Vendor`) s `Purchasing`. Následující kód ukazuje správná syntaxe:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample2.aspx)]

V dalším kroku přidáte panel, který slouží jako modální místní nabídky. Obsahuje `Button` ovládací prvek znova uzavřít automaticky otevírané okno:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample3.aspx)]

Aby bylo možné pracovat v opakovači, automaticky otevíraného okna `ModalPopupExtender` ovládací prvek musí být vložen v rámci `<ItemTemplate>` části opakovače. Na panelu je mimo opakovači, takže zařízení extender je uvnitř příkazu. Toto je zápis pro opakovače:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample4.aspx)]

Pak se zobrazí všechny položky ve zdroji dat s tlačítkem vedle sebe, vyvolá Modální místní nabídky.


[![Pro každou položku, zdroj dat se dá spouštět Modální místní nabídky](using-modalpopup-with-a-repeater-control-cs/_static/image2.png)](using-modalpopup-with-a-repeater-control-cs/_static/image1.png)

Pro každou položku, zdroj dat se dá spouštět Modální místní nabídky ([kliknutím ji zobrazíte obrázek v plné velikosti](using-modalpopup-with-a-repeater-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](launching-a-modal-popup-window-from-server-code-cs.md)
> [další](handling-postbacks-from-a-modalpopup-cs.md)
