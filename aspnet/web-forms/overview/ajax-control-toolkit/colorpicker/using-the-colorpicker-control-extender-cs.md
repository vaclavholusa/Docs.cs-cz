---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
title: Pomocí rozšiřujícího objektu ColorPicker řízení (C#) | Microsoft Docs
author: microsoft
description: ColorPicker je prvku ASP.NET AJAX rozšiřujícího objektu, která poskytuje funkci Barva výdej straně klienta pomocí uživatelského rozhraní v ovládacím prvku místní. Je možné připojit k žádné ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 0d86a1e7-a910-4ab2-b85c-7a9ea6906c39
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 4d44fc81305e668b545246cf044dce275563d81a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="using-the-colorpicker-control-extender-c"></a>Pomocí rozšiřujícího objektu ColorPicker řízení (C#)
====================
podle [Microsoft](https://github.com/microsoft)

> ColorPicker je prvku ASP.NET AJAX rozšiřujícího objektu, která poskytuje funkci Barva výdej straně klienta pomocí uživatelského rozhraní v ovládacím prvku místní. Se lze připojit k žádné ASP.NET TextBox – ovládací prvek. It.


Cílem tohoto kurzu je vysvětlují, jak můžete použít rozšiřujícího objektu řízení Toolkit ColorPicker řízení AJAX. Rozšiřujícího objektu řízení ColorPicker zobrazí automaticky otevřeném okně. dialog, který vám umožní vybrat barvu. ColorPicker je užitečné, kdykoli budete chtít poskytnout intuitivní uživatelské rozhraní pro uživatele a vybrat barvu.

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>Rozšíření ovládacího prvku textového pole s rozšiřujícího objektu ColorPicker ovládací prvek

Představte si například, že chcete vytvořit web, který umožňuje uživatelům serveru k vytvoření vlastní karty firmy. Návštěvníky můžete zadejte text pro vizitky a vybrat barvu. Stránka ASP.NET v výpis 1 obsahuje dva ovládací prvky textové pole s názvem txtCardText a txtCardColor. Při odesílání formuláře, vybrané hodnoty jsou zobrazeny (viz obrázek 1).


[![Jednoduchý formulář pro vytvoření vizitky](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)

**Obrázek 01**: jednoduchý formulář pro vytvoření vizitky ([Kliknutím zobrazit obrázek v plné velikosti](using-the-colorpicker-control-extender-cs/_static/image2.png))


**Listing 1 - CreateCard.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample1.aspx)]

Formuláře v výpis 1 funguje, ale neposkytuje vysoký výkon uživatele. Do textového pole Zadejte barvu, která má uživatel. Pokud uživatel chce specializované barev – například právě správné odstín pea zelená - pak uživatel musí rozmyslete si kód HTML bez pomoci.

Rozšiřujícího objektu řízení ColorPicker slouží k vytvoření lepší uživatelské prostředí. ColorPicker zobrazí dialogové okno barvy, když se přesunout fokus na ovládací prvek textové pole (viz obrázek 2).


[![Ovládací prvek ColorPicker rozšiřujícího objektu](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)

**Obrázek 02**: rozšiřujícího objektu ColorPicker ovládací prvek ([Kliknutím zobrazit obrázek v plné velikosti](using-the-colorpicker-control-extender-cs/_static/image4.png))


Je potřeba provést dva kroky pro použití rozšíření řízení ColorPicker s formuláři v výpis 1:

1. Přidání ovládacího prvku ScriptManager na stránku
2. Přidání rozšiřujícího objektu řízení ColorPicker na stránku

Než budete moct použít ColorPicker, je nutné přidat na stránku ovládací prvek ScriptManager. Je vhodná k přidání prvek ScriptManager přímo pod serverovou otevírání &lt;formuláře&gt; značky. Můžete přetáhnout prvek ScriptManager na stránce z panelu nástrojů (prvek ScriptManager se nachází na kartě Rozšíření AJAX). Alternativně můžete zadat následující značka do zobrazení zdroje pod počáteční značka formuláře na straně serveru:

&lt;ASP: ScriptManager ID = "ScriptManager1" runat = "server" /&gt;

V návrhovém zobrazení je nejjednodušší způsob, jak přidat rozšiřujícího objektu řízení ColorPicker na stránku. Pokud umístěte ukazatel myši nad txtCardColor textové pole, možnost Inteligentní úloh, zobrazí se povoluje můžete přidat rozšiřujícího objektu (viz obrázek 3). Pokud vyberete tuto možnost, zobrazí se Průvodce rozšiřujícího objektu (viz obrázek 4).


[![Přidání extender](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)

**Obrázek 03**: Přidání extender ([Kliknutím zobrazit obrázek v plné velikosti](using-the-colorpicker-control-extender-cs/_static/image6.png))


[![Výběr ovládacího prvku extender pomocí Průvodce rozšiřujícího objektu](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)

**Obrázek 04**: výběr extender ovládacího prvku pomocí Průvodce rozšiřujícího objektu ([Kliknutím zobrazit obrázek v plné velikosti](using-the-colorpicker-control-extender-cs/_static/image8.png))


Můžete si vybrat rozšiřujícího objektu ColorPicker rozšířit txtCardColor textové pole s ColorPicker rozšiřujícího objektu. Kliknutím na OK zavřete dialogové okno.

Po provedení těchto změn zdroj pro stránku vypadá jako výpis 2.

Výpis 2 - CreateCard.aspx (s ColorPicker)

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample2.aspx)]

Všimněte si, zda tato stránka obsahuje teď ColorPickerExtender ovládací prvek, který se zobrazí pod txtCardColor TextBox – ovládací prvek. Ovládací prvek ColorPickerExtender rozšiřuje ovládacího prvku txtCardColor tak, aby se zobrazí dialogové okno pro výběr barev.

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>Pomocí tlačítka Spustit dialogové okno pro výběr barev

Rozšiřujícího objektu ColorPicker podporuje následující vlastnosti:

- PopupButtonId - ID tlačítka na stránku, která způsobí, že dialogového okna pro výběr barev zobrazí.
- PopupPosition - pozice relativně k cílový ovládací prvek dialogového okna pro výběr barev. Možné hodnoty jsou absolutní, Center, BottomLeft, BottomRight, TopLeft, TopRight práva a doleva (výchozí hodnota je BottomLeft).
- SampleControlId - ID ovládacího prvku, který zobrazí vybrané barvy.
- SelectedColor – počáteční barvu vybraná ColorPicker.

Tyto vlastnosti můžete přizpůsobit zobrazení dialogového okna pro výběr barvy a zobrazení vybrané barvy. Stránka v výpis 3 znázorňuje, jak můžete použít několik z těchto vlastností.

**Výpis 3 - CreateCardButton.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample3.aspx)]

Na stránku výpis 3 zahrnuje barvu vyberte tlačítko (viz obrázek 5). Kliknutí na toto tlačítko se zobrazí dialogové okno pro výběr barev nad textové pole. Pokud vyberete barvu, která z tohoto dialogového okna vybrané barvy zobrazí jako barvu pozadí lblSample popisek – ovládací prvek.

Vlastnost ColorPicker PopupButtonID slouží k Barva vyberte tlačítko přidružit ColorPicker rozšiřujícího objektu. Pokud zadáte hodnotu pro vlastnost PopupButtonID, dialogové okno pro výběr barev už se zobrazí, když má právě fokus, cílový ovládací prvek. Musíte kliknout na tlačítko zobrazit dialogové okno.

Vlastnost SampleControlID je použito k přidružení ovládacího prvku, který zobrazí vybrané barvy s ColorPicker. ColorPicker změní barvu pozadí tohoto ovládacího prvku na aktuálně vybrané barvy.


[![Zobrazení dialogového okna pro výběr barvy s tlačítkem na](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)

**Obrázek 05**: zobrazení dialogového okna pro výběr barvy s tlačítkem na ([Kliknutím zobrazit obrázek v plné velikosti](using-the-colorpicker-control-extender-cs/_static/image10.png))


## <a name="summary"></a>Souhrn

V tomto kurzu jste zjistili, jak používat rozšiřujícího objektu řízení ColorPicker zobrazíte dialogové okno pro výběr barev místní. Nejdřív jsme se zaměřili jak můžete zobrazit dialogové okno když fokus se přesune do ovládacího prvku textového pole. V dalším kroku jste zjistili, jak vytvořit tlačítko, které se zobrazí dialogové okno pro výběr barev, když se po kliknutí na tlačítko.

> [!div class="step-by-step"]
> [Next](using-the-colorpicker-control-extender-vb.md)
