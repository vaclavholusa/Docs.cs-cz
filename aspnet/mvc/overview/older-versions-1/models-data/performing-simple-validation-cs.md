---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
title: Provedení jednoduchého ověření (C#) | Dokumentace Microsoftu
author: StephenWalther
description: Zjistěte, jak provést ověření v aplikaci ASP.NET MVC. V tomto kurzu se zavádí Stephen Walther vám stav modelu a pomocná rutina pro ověření HTML...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 21383c9d-6aea-4bad-a99b-b5f2c9d6503f
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: 14d7857c64268df3b998e05797f749f03509dd4b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753693"
---
<a name="performing-simple-validation-c"></a>Provedení jednoduchého ověření (C#)
====================
podle [Stephen Walther](https://github.com/StephenWalther)

> Zjistěte, jak provést ověření v aplikaci ASP.NET MVC. V tomto kurzu se zavádí Stephen Walther vám stav modelu a pomocných rutin HTML ověření.


Cílem tohoto kurzu je vysvětlují, jak můžete provést ověření v rámci aplikace ASP.NET MVC. Například se dozvíte, jak zabránit odeslání formuláře, který neobsahuje hodnotu pro požadované pole. Zjistíte, jak používat stav modelu a pomocných rutin HTML ověření.

## <a name="understanding-model-state"></a>Porozumění stavu modelu

Používáte stav modelu - nebo přesněji, slovník stavu modelu – pro reprezentaci chyby ověření. Například akce Create() ve výpisu 1 ověří vlastnosti třídy produktu před přidáním třídy produktu do databáze.


Můžu mi doporučujeme přidat logiku ověřování nebo databáze do kontroleru. Kontroler může obsahovat pouze logiku související řízení toku aplikace. Zástupce pro zjednodušení jsme se rozhodli.


**Výpis 1 - Controllers\ProductController.cs**

[!code-csharp[Main](performing-simple-validation-cs/samples/sample1.cs)]

V informacích 1 se ověří název, popis a UnitsInStock vlastnosti třídy produktu. Pokud selže některý z těchto vlastností ověřovací test chybu se přidá do slovníku stavů modelu (reprezentovaný identifikátorem ModelState vlastnost třídy Controller).

Pokud nejsou žádné chyby v modelu stavu ModelState.IsValid vlastnost vrátí hodnotu false. V takovém případě se zobrazí znovu formuláře HTML pro vytvoření nového produktu. Jinak pokud nejsou žádné chyby ověření, se přidá nového produktu do databáze.

## <a name="using-the-validation-helpers"></a>Použití pomocných rutin ověření

Architektura ASP.NET MVC zahrnuje dvě pomocné rutiny ověření: pomocné rutiny Html.ValidationMessage() a Html.ValidationSummary() pomocné rutiny. Pomocí těchto dvou pomocných rutin v zobrazení zobrazují chybové zprávy ověření.

Pomocné rutiny Html.ValidationMessage() a Html.ValidationSummary() se používají ve vytvoření a úprava zobrazení, která jsou automaticky generovány generování uživatelského rozhraní ASP.NET MVC. Postupujte podle těchto kroků vygenerujete vytvořit zobrazení:

1. Klikněte pravým tlačítkem na Create() akce v kontroleru produktu a vyberte možnost nabídky **přidat zobrazení** (viz obrázek 1).
2. V **přidat zobrazení** dialogového okna, zaškrtněte políčko s popiskem **vytvoření zobrazení se silnými typy** (viz obrázek 2).
3. Z **zobrazení dat třídy** rozevíracího seznamu vyberte třídu produktu.
4. Z **zobrazit obsah** rozevíracího seznamu vyberte možnost vytvořit.
5. Klikněte na tlačítko **přidat** tlačítko.


Ujistěte se, že vytváříte aplikaci před přidáním zobrazení. V opačném případě nebude zobrazovat seznam tříd **zobrazení dat třídy** rozevíracího seznamu.


[![Dialogové okno Nový projekt](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)

**Obrázek 01**: Přidání zobrazení ([kliknutím ji zobrazíte obrázek v plné velikosti](performing-simple-validation-cs/_static/image2.png))


[![Dialogové okno Nový projekt](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)

**Obrázek 02**: vytvoření zobrazení se silnými typy ([kliknutím ji zobrazíte obrázek v plné velikosti](performing-simple-validation-cs/_static/image4.png))


Po dokončení těchto kroků, získáte v informacích 2 zobrazení pro vytváření.

**Listing 2 - Views\Product\Create.aspx**

[!code-aspx[Main](performing-simple-validation-cs/samples/sample2.aspx)]

V informacích 2 nazývá Html.ValidationSummary() pomocné rutiny okamžitě nad formuláře HTML. Tato pomocná slouží k zobrazení seznam chybových zpráv ověření. Pomocná rutina Html.ValidationSummary() vykreslí chyby v seznamu s odrážkami.

Pomocná rutina Html.ValidationMessage() se nazývá vedle každého pole formuláře HTML. Tato pomocná slouží k zobrazení chybové zprávy přímo vedle pole formuláře. V případě výpis 2 zobrazí pomocná Html.ValidationMessage() hvězdičky, když dojde k chybě.

Na stránce na obrázku 3 znázorňuje chybové zprávy, který je vykreslen metodou ověřování pomocné rutiny, když se odešle formulář, chybějící pole a neplatné hodnoty.


[![Dialogové okno Nový projekt](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)

**Obrázek 03**: vytvořit zobrazení odeslanou s problémy ([kliknutím ji zobrazíte obrázek v plné velikosti](performing-simple-validation-cs/_static/image6.png))


Všimněte si, že vzhled HTML vstupní pole se nezmění, i když dojde k chybě ověřování. Pomocné rutiny vykreslení Html.TextBox() *třídy = "Chyba vstupu ověřování"* atribut, když dojde k chybě ověřování přidružený k vlastnosti vykreslen metodou Html.TextBox() helper.

Existují tři CSS třídy List stylu používat k ovládání výskyt chyb ověřování:

- vstup-– Chyba ověřování – použít &lt;vstupní&gt; vykreslen metodou Html.TextBox() helper značky.
- pole – – Chyba ověřování – použít &lt;span&gt; vykreslen metodou Html.ValidationMessage() helper značky.
- summary – chyby ověřování - použít &lt;ul&gt; vykreslen metodou Html.ValidationSumamry() helper značky.

Můžete upravit tyto šablony třídy List stylu a proto upravit vzhled chyby ověření tak, že upravíte soubor Site.css umístěný ve složce obsahu.

> [!NOTE] 
> 
> Třída HtmlHelper obsahuje statické vlastnosti jen pro čtení pro načítání názvů ověřování související s CSS třídy. Tyto statické vlastnosti jsou pojmenovány ValidationInputCssClassName ValidationFieldCssClassName a ValidationSummaryCssClassName.


## <a name="prebinding-validation-and-postbinding-validation"></a>Prebinding ověřování a ověřování Postbinding

Pokud odeslání formuláře HTML pro vytváření produktu, a zadáte neplatnou hodnotu pro pole price a žádná hodnota pro pole UnitsInStock, získáte ověřovacích zpráv, který zobrazí obrázek 4. Odkud pocházejí tyto chybových zpráv ověření ze?


[![Dialogové okno Nový projekt](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)

**Obrázek 04**: chyby ověření Prebinding ([kliknutím ji zobrazíte obrázek v plné velikosti](performing-simple-validation-cs/_static/image8.png))


Existují ve skutečnosti dva typy chybových zpráv ověření – těch, které generuje předtím, než pole formuláře HTML, které jsou vázány na třídu a jsou generovány po polí formuláře, které jsou vázány na třídu. Jinými slovy, existují prebinding chyby ověření a postbinding chyby ověření.

Akce Create() vystavené kontroler produktu v informacích 1 přijímá instanci třídy produktu. Podpis metody Create vypadá takto:

[!code-csharp[Main](performing-simple-validation-cs/samples/sample3.cs)]

Hodnoty polí formuláře HTML z formuláře vytvořit jsou vázány na třídu productToCreate se nazývá vazač modelu. Výchozí vazač modelu přidá chybovou zprávu do stavu modelu automaticky při pole formuláře nemůže vytvořit vazbu na vlastnost formuláře.

Výchozí vazač modelu nelze vytvořit vazbu řetězec "apple" Price vlastnosti třídy produktu. Vlastnost typu desetinného čísla nelze přiřadit řetězec. Proto přidá vazač modelu chybu do stavu modelu.

Výchozí vazač modelu také nelze přiřadit hodnotu null pro vlastnost, která nepřijímá žádné hodnoty Null. Zejména vazače modelu nelze přiřadit hodnotu null pro vlastnost UnitsInStock. Vazač modelu zase vrací a přidá chybovou zprávu do stavu modelu.

Pokud chcete přizpůsobit vzhled tyto prebinding chybové zprávy je potřeba vytvořit prostředek řetězce pro tyto zprávy.

## <a name="summary"></a>Souhrn

Cílem tohoto kurzu bylo popisují základní mechanismy ověřování v rozhraní ASP.NET MVC. Jste zjistili, jak používat stav modelu a pomocných rutin HTML ověření. Také jsme probírali rozdíl mezi prebinding a postbinding ověření. V dalších kurzech používání probereme různé strategie pro přesun kód pro ověření vašeho řadiče a do vaší třídy modelu.

> [!div class="step-by-step"]
> [Předchozí](displaying-a-table-of-database-data-cs.md)
> [další](validating-with-the-idataerrorinfo-interface-cs.md)
