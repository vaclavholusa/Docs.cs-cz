---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
title: Použití ovládacího prvku ComboBox (VB) | Dokumentace Microsoftu
author: microsoft
description: Pole se seznamem je ovládací prvek ASP.NET AJAX, který kombinuje flexibilitu textové pole se seznamem možností, ze kterých mohou uživatelé vybrat.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: e887e7b2-a6e7-4a28-a134-ba334494badb
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
msc.type: authoredcontent
ms.openlocfilehash: ec00a58581f36f87ecdca2fbd96fcea645f75f48
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753471"
---
<a name="how-do-i-use-the-combobox-control-vb"></a>Použití ovládacího prvku ComboBox (VB)
====================
podle [Microsoft](https://github.com/microsoft)

> Pole se seznamem je ovládací prvek ASP.NET AJAX, který kombinuje flexibilitu textové pole se seznamem možností, ze kterých mohou uživatelé vybrat.


Cílem tohoto kurzu je vysvětlit ovládacího prvku Toolkit ComboBox ovládacího prvku AJAX. Pole se seznamem funguje jako kombinace standardní ovládací prvek DropDownList s ASP.NET a ovládací prvek textového pole. Můžete vybrat z existující seznam položek, nebo zadejte novou položku.

Pole se seznamem se podobá – extender ovládacího prvku automatické dokončování, ale ovládací prvky se používají v různých scénářích. Zařízení extender automatické dokončování dotazů webovou službu, která najít odpovídající položky. ComboBox – ovládací prvek naproti tomu je inicializována s sadu položek. Umožňuje zařízení extender automatické dokončování pomocí smysl při práci s rozsáhlou sadou dat (miliony částí auta) při používání ovládacího prvku ComboBox dává smysl při práci s malou sadu dat (desítky car části).

## <a name="selecting-from-a-static-list-of-items"></a>Výběr ze statické seznam položek

Umožní s začínat jednoduchý příklad použití ovládacího prvku pole se seznamem. Představte si, že chcete zobrazit statický seznam položek v rozevíracím seznamu. Ale budete chtít zůstat otevřeno, možnost, že seznam není úplný. Budete chtít umožnit uživateli zadat vlastní hodnoty do seznamu.

Jsme ll vytvoření nové stránky webových formulářů ASP.NET a pomocí ovládacího prvku pole se seznamem na stránce. Do projektu přidejte novou stránku ASP.NET a přepněte do zobrazení návrhu.

Pokud chcete pomocí ovládacího prvku pole se seznamem na stránce musíte přidat ovládací prvek ScriptManager na stránce. Přetáhněte ovládací prvek ScriptManager from beneath na kartě Rozšíření AJAX na plochu návrháře. Měli byste přidat ovládací prvek ScriptManager v horní části stránky. můžete ho přidat bezprostředně pod levou serverové &lt;formuláře&gt; značky.

V dalším kroku přetáhněte ovládací prvek pole se seznamem na stránku. ComboBox – ovládací prvek můžete najít v panelu nástrojů s jinými sadou nástrojů AJAX Control Toolkit ovládacích prvků a extenderů (viz obrázek 1).


[![Jednoduchý formulář pro vytvoření karty firmy](how-do-i-use-the-combobox-control-vb/_static/image1.jpg)](how-do-i-use-the-combobox-control-vb/_static/image1.png)

**Obrázek 01**: výběr ComboBox – ovládací prvek z panelu nástrojů ([kliknutím ji zobrazíte obrázek v plné velikosti](how-do-i-use-the-combobox-control-vb/_static/image2.png))


Jsme ll pomocí ovládacího prvku pole se seznamem pro zobrazení statický seznam voleb. Uživatel můžete vybrat určitou úroveň spiciness pro jejich potravin ze seznamu tři možnosti: mírné, střední a aktivní (viz obrázek 2).


[![Výběr ze statické seznam položek](how-do-i-use-the-combobox-control-vb/_static/image2.jpg)](how-do-i-use-the-combobox-control-vb/_static/image3.png)

**Obrázek 02**: výběr z statický seznam položek ([kliknutím ji zobrazíte obrázek v plné velikosti](how-do-i-use-the-combobox-control-vb/_static/image4.png))


Existují dva způsoby, že tyto možnosti můžete přidat do ovládacího prvku pole se seznamem. Nejprve vyberte možnost upravit možnosti úkolu, když myší najedete myší na ovládací prvek v návrhovém zobrazení a otevřete Editor položek (viz obrázek 3).


[![Úprava pole se seznamem položek](how-do-i-use-the-combobox-control-vb/_static/image3.jpg)](how-do-i-use-the-combobox-control-vb/_static/image5.png)

**Obrázek 03**: úpravy pole se seznamem položek ([kliknutím ji zobrazíte obrázek v plné velikosti](how-do-i-use-the-combobox-control-vb/_static/image6.png))


Druhou možností je přidat seznam položek mezi otevírací a zavírací &lt;asp: pole se seznamem&gt; značky v zobrazení zdroje. Na stránce v informacích 1 obsahuje aktualizované pole se seznamem, který má seznam položek.

**Výpis 1 - Static.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample1.aspx)]

Při otevření stránky v informacích 1, můžete vybrat jednu z již existujících možností z pole se seznamem. Jinými slovy pole se seznamem funguje stejně jako ovládací prvek DropDownList.

Nicméně máte také možnost zadat novou volbu (třeba Super Spicy), který není v seznamu existujících. Ano pole se seznamem funguje také jako ovládací prvek textového pole.

Bez ohledu na to, jestli si vybrat existující položky nebo můžete zadat vlastní položky při odeslání formuláře, podle vašeho výběru se zobrazí v ovládacím prvku popisek. Po odeslání formuláře btnSubmit\_kliknutím obslužná rutina spustí a aktualizuje popisek (viz obrázek 4).


[![Zobrazení vybrané položky](how-do-i-use-the-combobox-control-vb/_static/image4.jpg)](how-do-i-use-the-combobox-control-vb/_static/image7.png)

**Obrázek 04**: zobrazení vybrané položky ([kliknutím ji zobrazíte obrázek v plné velikosti](how-do-i-use-the-combobox-control-vb/_static/image8.png))


Pole se seznamem podporuje stejné vlastnosti jako ovládací prvek DropDownList pro načtení vybrané položky po odeslání formuláře:

- SelectedItem.Text – zobrazí hodnoty vlastnosti Text vybrané položky.
- SelectedItem.Value – zobrazí hodnoty vlastnosti hodnotu vybrané položky nebo zobrazuje text zadaný do pole se seznamem.
- SelectedValue – stejný jako SelectedItem.Value s tím rozdílem, že tato vlastnost umožňuje určit výchozí (počáteční) vybranou položku.

Pokud zadáte vlastní možnost do pole se seznamem pak podle vlastního výběru přiřazená SelectedItem.Text i SelectedItem.Value vlastnosti.

## <a name="selecting-the-list-of-items-from-the-database"></a>Výběr v seznamu položek z databáze

Seznam položek, které zobrazí pole se seznamem můžete načíst z databáze. Například lze svázat pole se seznamem ovládacím prvkem SqlDataSource, ovládací prvek ObjectDataSource, zdroje dat LinqDataSource nebo EntityDataSource.

Představte si, že chcete zobrazit seznam filmy v pole se seznamem. Chcete načíst seznam videa z databázové tabulky videa. Postupujte podle těchto kroků:

1. Vytvoření stránky s názvem Movies.aspx
2. Přidání ovládacího prvku ScriptManager na stránce přetažením ScriptManager získáte na kartě Rozšíření AJAX na panelu nástrojů na stránku.
3. Přidání ovládacího prvku pole se seznamem na stránku tak, že přetáhnete pole se seznamem na stránku.
4. V návrhovém zobrazení přesunutí ukazatele myši nad ovládací prvek pole se seznamem a vyberte **zvolit zdroj dat** úloh možnost (viz obrázek 5). Průvodce konfigurací zdroje dat není spuštěn.
5. V **vyberte zdroj dat** kroku, vyberte &lt;nový zdroj dat&gt; možnost.
6. V **zvolte typ zdroje dat** kroku, vyberte databázi.
7. V **vyberte datové připojení** kroku, vyberte svou databázi (například MoviesDB.mdf).
8. V **uložit připojovací řetězec do konfiguračního souboru aplikace** kroku, vyberte možnost Uložit připojovací řetězec.
9. V **konfigurovat příkaz Select** kroku, vyberte v tabulce databáze filmů a vybrat všechny sloupce.
10. V **testovat dotaz** krok, kliknutím na tlačítko Dokončit.
11. Zpátky **zvolit zdroj dat** kroku, vyberte sloupec název pro pole k zobrazení a ve sloupci Id pro data polí (viz obrázek).
12. Klikněte na tlačítko OK ukončete průvodce.


[![Výběr zdroje dat](how-do-i-use-the-combobox-control-vb/_static/image5.jpg)](how-do-i-use-the-combobox-control-vb/_static/image9.png)

**Obrázek 05**: Výběr zdroje dat ([kliknutím ji zobrazíte obrázek v plné velikosti](how-do-i-use-the-combobox-control-vb/_static/image10.png))


[![Výběr datových polí hodnoty a text](how-do-i-use-the-combobox-control-vb/_static/image6.jpg)](how-do-i-use-the-combobox-control-vb/_static/image11.png)

**Obrázek 06**: Výběr datových polí hodnoty a text ([kliknutím ji zobrazíte obrázek v plné velikosti](how-do-i-use-the-combobox-control-vb/_static/image12.png))


Po dokončení výše uvedených kroků, pole se seznamem je svázána s ovládacím prvkem SqlDataSource, představující videa z databázové tabulky filmy. Zdroj pro stránku vypadá výpis 2 (mám vyčištění formátování trochu).

**Výpis 2 - Movies.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample2.aspx)]

Všimněte si, že ovládací prvek pole se seznamem má vlastnost DataSourceID, která odkazuje na ovládacím prvkem SqlDataSource. Když otevřete stránku v prohlížeči se zobrazí seznam videa z databáze (viz obrázek 7). Můžete vybrat video ze seznamu nebo zadejte nový film zadáním videa do pole se seznamem.


[![Zobrazení seznamu filmy](how-do-i-use-the-combobox-control-vb/_static/image7.jpg)](how-do-i-use-the-combobox-control-vb/_static/image13.png)

**Obrázek 07**: zobrazení seznamu filmy ([kliknutím ji zobrazíte obrázek v plné velikosti](how-do-i-use-the-combobox-control-vb/_static/image14.png))


## <a name="setting-the-dropdownstyle"></a>Nastavení DropDownStyle

Pokud chcete změnit chování pole se seznamem můžete použít vlastnost DropDownStyle – pole se seznamem. Tato vlastnost přijímá není možné hodnoty:

- Rozevírací seznam – (výchozí hodnota) zobrazí The – pole se seznamem rozevírací seznam, když kliknete na šipku a vy můžete zadat vlastní hodnotu.
- Jednoduché – pole se seznamem se automaticky zobrazí rozevírací seznam a můžete zadat vlastní hodnoty.
- DropDownList – pole se seznamem funguje stejně jako ovládací prvek DropDownList.

Rozdíly mezi rozevírací seznam a jednoduché je, když se zobrazí seznam položek. V případě jednoduchého zobrazí se seznam okamžitě při přesunutí fokusu do pole se seznamem. V případě rozevírací seznam musíte kliknout na šipku zobrazte seznam položek.

Hodnota DropDownList způsobí, že ovládací prvek ComboBox pracovat stejně jako standardní ovládací prvek DropDownList. Ale je tady důležitý rozdíl. Starší verze aplikace Internet Explorer zobrazit ovládací prvek DropDownList s nekonečnou z-index, proto se ovládací prvek zobrazí před libovolný ovládací prvek umístěn před jeho. Vzhledem k tomu, že pole se seznamem vykreslí HTML &lt;div&gt; značek namísto HTML &lt;vyberte&gt; značky, pole se seznamem správně respektuje pořadí z-ordering.

## <a name="setting-the-autocompletemode"></a>Nastavení vlastnost AutoCompleteMode

Chcete-li určit, co se stane, když uživatel zadá text do pole se seznamem použijete vlastnost AutoCompleteMode – pole se seznamem vlastností. Tato vlastnost přijímá následující možné hodnoty:

- None – (výchozí hodnota), která pole se seznamem neposkytuje žádné chování automatického dokončení.
- Navrhnout – pole se seznamem se zobrazí v seznamu a zvýrazní odpovídající položku v seznamu (viz obrázek 8).
- Append – pole se seznamem nezobrazuje v seznamu a přidá odpovídající položky ze seznamu do co jste zadali (viz obrázek 9).
- SuggestAppend – pole se seznamem se zobrazí v seznamu a přidá odpovídající položky ze seznamu do co jste zadali (viz obrázek 10).


[![Pole se seznamem umožňuje návrh](how-do-i-use-the-combobox-control-vb/_static/image8.jpg)](how-do-i-use-the-combobox-control-vb/_static/image15.png)

**Obrázek 08**: pole se seznamem umožňuje návrh ([kliknutím ji zobrazíte obrázek v plné velikosti](how-do-i-use-the-combobox-control-vb/_static/image16.png))


[![Pole se seznamem připojí odpovídající text](how-do-i-use-the-combobox-control-vb/_static/image9.jpg)](how-do-i-use-the-combobox-control-vb/_static/image17.png)

**Obrázek 09**: pole se seznamem připojí odpovídající text ([kliknutím ji zobrazíte obrázek v plné velikosti](how-do-i-use-the-combobox-control-vb/_static/image18.png))


[![Pole se seznamem navrhuje a připojí](how-do-i-use-the-combobox-control-vb/_static/image10.jpg)](how-do-i-use-the-combobox-control-vb/_static/image19.png)

**Obrázek 10**: pole se seznamem navrhuje a připojí ([kliknutím ji zobrazíte obrázek v plné velikosti](how-do-i-use-the-combobox-control-vb/_static/image20.png))


## <a name="summary"></a>Souhrn

V tomto kurzu jste zjistili, jak zobrazit pevnou sadu položek pomocí ovládacího prvku ComboBox. Jsme vázaného ovládacího prvku pole se seznamem, jak na statickou sadu položek a do databázové tabulky. Nakonec jste zjistili, jak upravit nastavením jeho vlastnosti DropDownStyle a vlastnost AutoCompleteMode chování pole se seznamem.

> [!div class="step-by-step"]
> [Předchozí](how-do-i-use-the-combobox-control-cs.md)
