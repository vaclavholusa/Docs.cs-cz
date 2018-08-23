---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
title: Použití zařízení Extender ovládacího prvku ColorPicker (VB) | Dokumentace Microsoftu
author: microsoft
description: ColorPicker je extenderu ASP.NET AJAX, která poskytuje funkce vybere barvu na straně klienta s uživatelským rozhraním v ovládacím prvku popup. Může být připojen k žádné ASP.NET...
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 577ae07b-a872-4818-a804-bca489b40ad0
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 2fa3804411cb553de242a503f57e247efc990156
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752617"
---
<a name="using-the-colorpicker-control-extender-vb"></a>Použití zařízení Extender ovládacího prvku ColorPicker (VB)
====================
podle [Microsoft](https://github.com/microsoft)

> ColorPicker je extenderu ASP.NET AJAX, která poskytuje funkce vybere barvu na straně klienta s uživatelským rozhraním v ovládacím prvku popup. Může být připojen na libovolný ovládací prvek TextBox technologie ASP.NET. Ho.


Cílem tohoto kurzu je vysvětlují, jak můžete použít zařízení extender ovládacího prvku Toolkit ColorPicker ovládacího prvku AJAX. Extender ovládacího prvku ColorPicker zobrazí dialogové okno automaticky otevíraného okna, která umožňuje vybrat barvu. ColorPicker je vhodný v každé byste chtěli poskytnout intuitivní uživatelské rozhraní pro uživatele pro výběr barvy.

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>Rozšíří ovládací prvek TextBox s zařízení Extender ovládacího prvku ColorPicker

Představte si například, že chcete vytvořit web, který umožňuje návštěvníkům vytváření přizpůsobených obchodních karet. Návštěvníci můžete zadat text pro kartu firmy a vybrat barvu. Na stránce technologie ASP.NET v informacích 1 obsahuje dva ovládací prvky textového pole s názvem txtCardText a txtCardColor. Při odeslání formuláře se zobrazí vybrané hodnoty (viz obrázek 1).


[![Jednoduchý formulář pro vytvoření karty firmy](using-the-colorpicker-control-extender-vb/_static/image1.jpg)](using-the-colorpicker-control-extender-vb/_static/image1.png)

**Obrázek 01**: jednoduchý formulář pro vytvoření vizitky ([kliknutím ji zobrazíte obrázek v plné velikosti](using-the-colorpicker-control-extender-vb/_static/image2.png))


**Výpis 1 - CreateCard.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample1.aspx)]

Formulář v nástrojích pro výpis 1 funguje, ale neposkytuje skvělé uživatelské prostředí. Uživatel musí zadat barvu do textového pole. Pokud uživatel požaduje specializované barvy – například právě správné odstín pea zelená - pak uživatel musí zjistit kód HTML bez pomoci.

Extender ovládacího prvku ColorPicker slouží k vytvoření lepší uživatelské prostředí. ColorPicker zobrazí dialogové okno Barva, když se přesunete fokus na ovládací prvek textového pole (viz obrázek 2).


[![Extender ovládacího prvku ColorPicker](using-the-colorpicker-control-extender-vb/_static/image2.jpg)](using-the-colorpicker-control-extender-vb/_static/image3.png)

**Obrázek 02**: The extenderu ovládacího prvku ColorPicker ([kliknutím ji zobrazíte obrázek v plné velikosti](using-the-colorpicker-control-extender-vb/_static/image4.png))


Je třeba provést dva kroky pro použití s formulář v nástrojích pro výpis 1 extender ovládacího prvku ColorPicker:

1. Přidání ovládacího prvku ScriptManager na stránce
2. Extender ovládacího prvku ColorPicker přidat na stránku

Před použitím ColorPicker, je nutné přidat ovládací prvek ScriptManager na stránku. Je vhodné místo pro přidání ScriptManager přímo pod levou serverové &lt;formuláře&gt; značky. Prvek ScriptManager na stránku můžete přetáhnout z panelu nástrojů (prvek ScriptManager se nachází na kartě Rozšíření AJAX). Alternativně můžete zadat následující značku do zobrazení zdroje pod počáteční značka pro formulář na straně serveru:

&lt;ASP: ScriptManager ID = "ScriptManager1" runat = "server" /&gt;

Nejjednodušší způsob, jak přidat na stránku – extender ovládacího prvku ColorPicker je v zobrazení Návrh. Pokud myší najedete myší txtCardColor textové pole, inteligentní úloh možnost bude nabídnuta povoluje můžete přidat zařízení extender (viz obrázek 3). Pokud vyberete tuto možnost, zobrazí se Průvodce zařízení Extender (viz obrázek 4).


[![Přidání zařízení extender](using-the-colorpicker-control-extender-vb/_static/image3.jpg)](using-the-colorpicker-control-extender-vb/_static/image5.png)

**Obrázek 03**: přidání zařízení extender ([kliknutím ji zobrazíte obrázek v plné velikosti](using-the-colorpicker-control-extender-vb/_static/image6.png))


[![Extender ovládacího prvku pomocí Průvodce rozšiřující výběr](using-the-colorpicker-control-extender-vb/_static/image4.jpg)](using-the-colorpicker-control-extender-vb/_static/image7.png)

**Obrázek 04**: výběr – extender ovládacího prvku pomocí Průvodce zařízení Extender ([kliknutím ji zobrazíte obrázek v plné velikosti](using-the-colorpicker-control-extender-vb/_static/image8.png))


Můžete si vybrat zařízení extender ColorPicker rozšířit txtCardColor textové pole s ColorPicker zařízení extender. Klikněte na tlačítko OK zavřete dialogové okno.

Po provedení těchto změn zdroje stránky vypadá výpis 2.

**Výpis 2 - CreateCard.aspx (s ColorPicker)**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample2.aspx)]

Všimněte si, že stránka nyní obsahuje ColorPickerExtender ovládací prvek, který se zobrazí přímo pod txtCardColor TextBox – ovládací prvek. Ovládací prvek ColorPickerExtender rozšiřuje txtCardColor ovládacího prvku tak, aby zobrazil dialogové okno Výběr barvy.

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>Pomocí tlačítka Spustit dialogové okno Výběr barvy

Zařízení extender ColorPicker podporuje následující vlastnosti:

- PopupButtonId - ID na stránce, která způsobí, že dialogové okno Výběr barvy se zobrazí tlačítko.
- PopupPosition – pozice relativně k cílovému ovládacímu prvku dialogového okna pro výběr barvy. Možné hodnoty jsou absolutní, System Center, BottomLeft, BottomRight, TopLeft, TopRight vpravo a vlevo (výchozí hodnota je BottomLeft).
- SampleControlId - ID pro ovládací prvek zobrazující vybranou barvu.
- SelectedColor – Počáteční barva zvolila ColorPicker.

Tyto vlastnosti můžete přizpůsobit, jak je zobrazeno dialogové okno Výběr barvy a jak se zobrazí vybranou barvu. Na stránce v informacích 3 ukazuje, jak můžete používat některé z těchto vlastností.

**Výpis 3 - CreateCardButton.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample3.aspx)]

Na stránce v informacích 3 zahrnuje výběr barvy tlačítka (viz obrázek 5). Po kliknutí na toto tlačítko, zobrazí se dialogové okno Výběr barvy vyšší než textovém poli. Je-li vybrat barvu z tohoto dialogového okna vybraná barva zobrazí jako barva pozadí lblSample ovládacího prvku popisku.

Vlastnost ColorPicker PopupButtonID je slouží k přidružení zařízení extender ColorPicker tlačítko Vybrat barvu. Pokud zadáte hodnotu pro vlastnost PopupButtonID, dialogové okno Výběr barvy už se zobrazí, když cílový ovládací prvek má fokus. Musíte kliknout na tlačítko pro zobrazení dialogového okna.

Vlastnost SampleControlID je použito k přidružení ovládací prvek, který zobrazuje vybrané barvy s ColorPicker. Barva pozadí tohoto ovládacího prvku ColorPicker změní na aktuálně vybraná barva.


[![Zobrazení dialogového okna pro výběr barvy s tlačítkem](using-the-colorpicker-control-extender-vb/_static/image5.jpg)](using-the-colorpicker-control-extender-vb/_static/image9.png)

**Obrázek 05**: zobrazení dialogového okna pro výběr barvy s tlačítkem ([kliknutím ji zobrazíte obrázek v plné velikosti](using-the-colorpicker-control-extender-vb/_static/image10.png))


## <a name="summary"></a>Souhrn

V tomto kurzu jste zjistili, jak použít zařízení extender ovládacího prvku ColorPicker zobrazíte dialogové okno Výběr barvy automaticky otevíraného okna. Nejprve jsme prozkoumat, jak můžete zobrazit dialogové okno když fokus se přesune na ovládací prvek textového pole. Dále jste zjistili, jak vytvořit tlačítko, které při kliknutí na tlačítko se zobrazí dialogové okno Výběr barvy.

> [!div class="step-by-step"]
> [Předchozí](using-the-colorpicker-control-extender-cs.md)
