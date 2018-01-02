---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-cs
title: "Používání ovládacího prvku ComboBox (C#) | Microsoft Docs"
author: microsoft
description: "Pole se seznamem je ovládací prvek ASP.NET AJAX, který kombinuje flexibilitu textové pole se seznamem možnosti, ze kterých můžete vybírat uživatelé."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 0bbf4134-04df-4226-8930-d5bb99e27128
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7913affb73c1c314944782ff80cf6c5558502ee9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-use-the-combobox-control-c"></a>Používání ovládacího prvku ComboBox (C#)
====================
podle [Microsoft](https://github.com/microsoft)

> Pole se seznamem je ovládací prvek ASP.NET AJAX, který kombinuje flexibilitu textové pole se seznamem možnosti, ze kterých můžete vybírat uživatelé.


Cílem tohoto kurzu je vysvětlit ovládacího prvku Toolkit ComboBox řízení AJAX. Komponenty ComboBox funguje jako kombinace mezi standardní ovládací prvek rozevírací seznam ASP.NET a ovládací prvek textové pole. Můžete vybrat z předem existující seznam položek, nebo zadejte novou položku.

Komponenty ComboBox je podobná rozšiřujícího objektu řízení automatického dokončování, ale ovládací prvky se používají v různých scénářích. Automatické dokončování rozšiřujícího objektu dotazuje webové služby, chcete-li získat odpovídající položky. ComboBox – ovládací prvek, je naopak inicializován sadu položek. Pomocí díky rozšiřujícího objektu automatického dokončování smysl při práci s velké sady dat (miliony car částí) při použití ovládacího prvku ComboBox dává smysl při práci s malou sadu dat (desítek car části).

## <a name="selecting-from-a-static-list-of-items"></a>Výběr ze statické seznam položek

Umožní s začínat jednoduchého ukázkového použití ComboBox – ovládací prvek. Představte si, že chcete zobrazit statický seznam položky v rozevíracím seznamu. Chcete zůstat otevřeno, možnost, že seznam není úplný. Chcete povolit uživatelům zadat vlastní hodnotu do seznamu.

Jsme budete vytvářet nové stránky webových formulářů ASP.NET a používat ComboBox – ovládací prvek na stránce. Do projektu přidejte novou stránku ASP.NET a přepněte do zobrazení návrhu.

Pokud chcete použít ComboBox – ovládací prvek na stránce musíte přidat ovládací prvek ScriptManager na stránku. Přetáhněte ovládací prvek ScriptManager from beneath karty rozšíření AJAX na plochu návrháře. Měli byste přidat ovládací prvek ScriptManager v horní části stránky. můžete ho přidat okamžitě níže serverovou otevírání &lt;formuláře&gt; značky.

ComboBox – ovládací prvek pak přetáhněte na stránku. ComboBox – ovládací prvek můžete najít v panelu nástrojů s jinými prvky AJAX Control Toolkit a řízení Extender (viz figure1).


[![Jednoduchý formulář pro vytvoření vizitky](how-do-i-use-the-combobox-control-cs/_static/image1.jpg)](how-do-i-use-the-combobox-control-cs/_static/image1.png)

**Obrázek 01**: výběr ComboBox – ovládací prvek z panelu nástrojů ([Kliknutím zobrazit obrázek v plné velikosti](how-do-i-use-the-combobox-control-cs/_static/image2.png))


Jsme udou ComboBox – ovládací prvek použít k zobrazení statických seznamu voleb. Uživatele můžete vybrat konkrétní úroveň spiciness pro jejich jídlo ze seznamu tři možnosti: mírné, střední a aktivní (viz obrázek 2).


[![Výběr ze statické seznam položek](how-do-i-use-the-combobox-control-cs/_static/image2.jpg)](how-do-i-use-the-combobox-control-cs/_static/image3.png)

**Obrázek 02**: výběr ze statické seznam položek ([Kliknutím zobrazit obrázek v plné velikosti](how-do-i-use-the-combobox-control-cs/_static/image4.png))


Tyto možnosti můžete přidat do ovládacího prvku ComboBox dvěma způsoby. Nejprve možnost úloh upravit možnosti vyberte, když ukazatel myši ukazatele myši na ovládací prvek v zobrazení návrhu a otevřete Editor položky (viz obrázek 3).


[![Úprava položek pole se seznamem](how-do-i-use-the-combobox-control-cs/_static/image3.jpg)](how-do-i-use-the-combobox-control-cs/_static/image5.png)

**Obrázek 03**: úpravy ComboBox položky ([Kliknutím zobrazit obrázek v plné velikosti](how-do-i-use-the-combobox-control-cs/_static/image6.png))


Druhou možností je přidat seznam položek mezi počáteční a koncovou &lt;asp: ComboBox&gt; značky v zobrazení zdroje. Tato stránka v výpis 1 obsahuje aktualizované ComboBox, který má seznam položek.

**Výpis 1 - Static.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-cs/samples/sample1.aspx)]

Při otevření stránky v výpis 1, můžete vybrat jednu z předchozích možností z komponenty ComboBox. Jinými slovy komponenty ComboBox funguje stejně jako ovládací prvek rozevírací seznam.

Ale máte také možnost zadat novou volbu (například Super Spicy), není v seznamu existující. Ano komponenty ComboBox funguje taky jako ovládací prvek textové pole.

Bez ohledu na to, jestli vyberete existující položky nebo můžete zadat vlastní položky při odeslání formuláře, vybraná možnost se zobrazí v ovládacím prvku popisek. Po odeslání formuláře btnSubmit\_klikněte na tlačítko obslužná rutina spustí a aktualizuje popisek (viz obrázek 4).


[![Zobrazení vybrané položky](how-do-i-use-the-combobox-control-cs/_static/image4.jpg)](how-do-i-use-the-combobox-control-cs/_static/image7.png)

**Obrázek 04**: zobrazení vybrané položky ([Kliknutím zobrazit obrázek v plné velikosti](how-do-i-use-the-combobox-control-cs/_static/image8.png))


Komponenty ComboBox podporuje stejné vlastnosti jako ovládací prvek rozevírací seznam pro načítání vybrané položky po odeslání formuláře:

- SelectedItem.Text - zobrazí hodnotu vlastnosti Text vybrané položky.
- SelectedItem.Value - zobrazí hodnotu, hodnotu vlastnosti vybrané položky nebo zobrazí text zadaný do komponenty ComboBox.
- SelectedValue - stejný jako SelectedItem.Value s tím rozdílem, že tato vlastnost umožňuje určit výchozí (počáteční) vybranou položku.

Pokud zadáte vlastní volba do ComboBox potom vlastní volba je přiřazen SelectedItem.Text i SelectedItem.Value vlastnosti.

## <a name="selecting-the-list-of-items-from-the-database"></a>Výběr seznam položek z databáze

Seznam položek, které zobrazuje komponenty ComboBox můžete načíst z databáze. Například můžete vázat ComboBox na ovládací prvek SqlDataSource ovládacího prvku ObjectDataSource, LinqDataSource nebo EntityDataSource.

Představte si, že chcete zobrazit seznam filmy v komponentě ComboBox. Chcete-li seznam filmy z tabulky databáze filmy získat. Postupujte podle těchto kroků:

1. Vytvoření stránky s názvem Movies.aspx
2. Přidáte ovládací prvek ScriptManager na stránku přetažením ScriptManager získáte karty rozšíření AJAX v panelu nástrojů na stránku.
3. ComboBox – ovládací prvek přidejte na stránku tak, že přetáhnete komponenty ComboBox na stránku.
4. V návrhovém zobrazení, umístěte ukazatel myši nad ComboBox – ovládací prvek a vyberte **zvolit zdroj dat** úloh možnost (viz obrázek 5). Průvodce konfigurací zdroje dat se spustí.
5. V **zvolit zdroj dat** krok, vyberte &lt;nový zdroj dat&gt; možnost.
6. V **zvolte typ zdroje dat** kroku, vyberte databázi.
7. V **vybrat datové připojení** kroku, vyberte svou databázi (například MoviesDB.mdf).
8. V **uložit připojovací řetězec do konfiguračního souboru aplikace** kroku, vyberte možnost Uložit připojovací řetězec.
9. V **konfigurovat příkaz Select** krok, vyberte v tabulce databáze filmy a vyberte všechny sloupce.
10. V **Test dotazu** kroku, klikněte na tlačítko Dokončit.
11. Zpět v **zvolit zdroj dat** krok, vyberte název sloupce pro pole, které chcete zobrazit a ve sloupci Id pro data pole (viz obrázek).
12. Klikněte na tlačítko OK ukončete průvodce.


[![Výběr zdroje dat](how-do-i-use-the-combobox-control-cs/_static/image5.jpg)](how-do-i-use-the-combobox-control-cs/_static/image9.png)

**Obrázek 05**: Výběr zdroje dat ([Kliknutím zobrazit obrázek v plné velikosti](how-do-i-use-the-combobox-control-cs/_static/image10.png))


[![Výběr datových polí hodnoty a text](how-do-i-use-the-combobox-control-cs/_static/image6.jpg)](how-do-i-use-the-combobox-control-cs/_static/image11.png)

**Obrázek 06**: Výběr datových polí hodnoty a text ([Kliknutím zobrazit obrázek v plné velikosti](how-do-i-use-the-combobox-control-cs/_static/image12.png))


Po dokončení kroků výše komponenty ComboBox je vázána na SqlDataSource ovládací prvek, který představuje videa z tabulky databáze filmy. Zdroj pro stránku vypadá výpis 2 (I vyčistit formátování chvíli).

**Výpis 2 - Movies.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-cs/samples/sample2.aspx)]

Všimněte si, že ComboBox – ovládací prvek má vlastnost DataSourceID, která odkazuje na ovládací prvek SqlDataSource. Když otevřete stránku v prohlížeči, zobrazí se seznam filmy z databáze (viz obrázek 7). Můžete vybrat filmu ze seznamu nebo zadejte nový film zadáním film do komponenty ComboBox.


[![Zobrazení seznamu filmy](how-do-i-use-the-combobox-control-cs/_static/image7.jpg)](how-do-i-use-the-combobox-control-cs/_static/image13.png)

**Obrázek 07**: zobrazení seznamu filmy ([Kliknutím zobrazit obrázek v plné velikosti](how-do-i-use-the-combobox-control-cs/_static/image14.png))


## <a name="setting-the-dropdownstyle"></a>Nastavení DropDownStyle

Vlastnost ComboBox DropDownStyle můžete změnit chování komponenty ComboBox. Tato vlastnost přijímá existuje možné hodnoty:

- Rozevírací seznam - zobrazí ComboBox (výchozí hodnota) rozevírací seznam po kliknutí na šipku a vy můžete zadat vlastní hodnotu.
- Jednoduchá - komponenty ComboBox automaticky zobrazí rozevírací seznam a můžete zadat vlastní hodnoty.
- Rozevírací seznam - komponenty ComboBox funguje stejně jako ovládací prvek rozevírací seznam.

Rozdíly mezi rozevíracího seznamu a jednoduché je, když se zobrazí seznam položek. V případě jednoduchý v seznamu se zobrazí okamžitě po přesunout fokus na komponenty ComboBox. V případě rozevíracího seznamu musíte kliknout na šipku zobrazíte seznam položek.

Rozevírací seznam hodnotu způsobí, že ComboBox – ovládací prvek pracovat stejně jako standardní ovládací prvek rozevírací seznam. Je však tady důležitý rozdíl. Starší verze aplikace Internet Explorer zobrazí ovládací prvek rozevírací seznam s neomezenou z-index, tak se ovládací prvek zobrazí před libovolný ovládací prvek umístěn úrovních před ním. Protože komponenty ComboBox vykreslí HTML &lt;div&gt; značky místo HTML &lt;vyberte&gt; štítku komponenty ComboBox správně respektuje pořadí z-ordering.

## <a name="setting-the-autocompletemode"></a>Nastavení vlastnost AutoCompleteMode

Chcete-li určit, co se stane, když někdo typy text do komponenty ComboBox pomocí vlastnosti vlastnost AutoCompleteMode pole se seznamem. Tato vlastnost přijímá následující hodnoty:

- Žádný - (výchozí hodnota), které komponenty ComboBox neposkytuje žádné chování automatického dokončování.
- Navrhněte - komponenty ComboBox zobrazí v seznamu a zvýrazní odpovídající položku v seznamu (viz obrázek 8).
- Append – komponenty ComboBox nezobrazuje v seznamu a přidá odpovídající položku v seznamu na zadanému (viz obrázek 9).
- SuggestAppend - komponenty ComboBox zobrazí v seznamu a připojí odpovídající položku v seznamu na zadanému (viz obrázek 10).


[![Komponenty ComboBox díky zlepšení](how-do-i-use-the-combobox-control-cs/_static/image8.jpg)](how-do-i-use-the-combobox-control-cs/_static/image15.png)

**Obrázek 08**: ComboBox díky zlepšení ([Kliknutím zobrazit obrázek v plné velikosti](how-do-i-use-the-combobox-control-cs/_static/image16.png))


[![ComboBox – připojí odpovídající text](how-do-i-use-the-combobox-control-cs/_static/image9.jpg)](how-do-i-use-the-combobox-control-cs/_static/image17.png)

**Obrázek 09**: ComboBox připojí odpovídající text ([Kliknutím zobrazit obrázek v plné velikosti](how-do-i-use-the-combobox-control-cs/_static/image18.png))


[![Komponenty ComboBox navrhuje a připojí](how-do-i-use-the-combobox-control-cs/_static/image10.jpg)](how-do-i-use-the-combobox-control-cs/_static/image19.png)

**Obrázek 10**: ComboBox navrhuje a připojí ([Kliknutím zobrazit obrázek v plné velikosti](how-do-i-use-the-combobox-control-cs/_static/image20.png))


## <a name="summary"></a>Souhrn

V tomto kurzu jste zjistili, jak používat ComboBox – ovládací prvek zobrazíte pevnou sadu položek. Jsme vázán ComboBox – ovládací prvek, jak do statického sada položek a do databázové tabulky. Nakonec jste zjistili, jak změnit chování komponenty ComboBox nastavením jeho DropDownStyle a vlastnost AutoCompleteMode vlastnosti.

>[!div class="step-by-step"]
[Další](how-do-i-use-the-combobox-control-vb.md)
