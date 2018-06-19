---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
title: Provádění jednoduché ověření (C#) | Microsoft Docs
author: StephenWalther
description: Zjistěte, jak provádět ověření v aplikaci ASP.NET MVC. V tomto kurzu Stephen Walther zavádí můžete do stavu modelu a pomocná rutina pro ověření HTML...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 21383c9d-6aea-4bad-a99b-b5f2c9d6503f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: 7fc1dcc6935841382215f67a519cd241ac68931a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869657"
---
<a name="performing-simple-validation-c"></a>Provádění jednoduché ověření (C#)
====================
podle [Stephen Walther](https://github.com/StephenWalther)

> Zjistěte, jak provádět ověření v aplikaci ASP.NET MVC. V tomto kurzu Stephen Walther představuje můžete do stavu modelu a ověření HTML pomocné rutiny.


Cílem tohoto kurzu je vysvětlují, jak lze provést ověření v rámci aplikace ASP.NET MVC. Například zjistíte, jak zabránit odeslání formuláře, který neobsahuje hodnotu pro povinné pole. Zjistíte, jak používat stav modelu a pomocné rutiny HTML ověření.

## <a name="understanding-model-state"></a>Principy stavu modelu

Používáte stav modelu – nebo přesněji, slovníku stavů modelu - představují chyby ověření. Například akce Create() ve výpisu 1 ověří vlastnosti třídy produktu před přidáním třídy produktu do databáze.


I není doporučujeme přidat logika ověřování nebo databáze do kontroleru. Řadič by měl obsahovat pouze logiku týkající se řízení toku aplikací. Jsme trvá zástupce zjednodušení.


**Výpis 1 - Controllers\ProductController.cs**

[!code-csharp[Main](performing-simple-validation-cs/samples/sample1.cs)]

Ve výpisu 1 se ověří název, popis a JednotkyNaSkladě vlastnosti třídy produktu. Pokud některý z těchto vlastností selže ověřovací test chybu se přidá do slovníku stavů modelu (představované ModelState vlastnost třídy Controller).

Pokud nejsou žádné chyby v modelu stavu vlastnost ModelState.IsValid vrátí hodnotu false. V takovém případě se zobrazí znovu formuláře HTML pro vytvoření nového produktu. Jinak pokud nejsou žádné chyby ověření, nového produktu se přidá do databáze.

## <a name="using-the-validation-helpers"></a>Pomocí ověření pomocné rutiny

Rozhraní ASP.NET MVC zahrnuje dvě pomocné rutiny ověření: pomocné rutiny Html.ValidationMessage() a Html.ValidationSummary() pomocné rutiny. K zobrazení chybových zpráv ověření používáte tyto dvě pomocné rutiny v zobrazení.

Pomocníci Html.ValidationMessage() a Html.ValidationSummary() se používají v zobrazení vytvořit a upravit, které automaticky generuje služba generování uživatelského rozhraní ASP.NET MVC. Postupujte podle těchto kroků ke generování zobrazení pro vytváření:

1. Klikněte pravým tlačítkem na Create() akce v kontroleru produktu a vyberte možnost nabídky **přidat zobrazení** (viz obrázek 1).
2. V **přidat zobrazení** dialogové okno, zaškrtněte políčko s názvem bez přípony **vytvořit zobrazení silného typu** (viz obrázek 2).
3. Z **zobrazit třída dat** rozevíracího seznamu, vyberte třídu produktu.
4. Z **zobrazit obsah** rozevíracího seznamu, vyberte možnost vytvořit.
5. Klikněte **přidat** tlačítko.


Ujistěte se, že vytváříte aplikace před přidáním zobrazení. Jinak se v seznamu tříd nezobrazí **zobrazit třída dat** rozevíracího seznamu.


[![Dialogové okno Nový projekt](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)

**Obrázek 01**: Přidání zobrazení ([Kliknutím zobrazit obrázek v plné velikosti](performing-simple-validation-cs/_static/image2.png))


[![Dialogové okno Nový projekt](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)

**Obrázek 02**: vytváření zobrazení silného typu ([Kliknutím zobrazit obrázek v plné velikosti](performing-simple-validation-cs/_static/image4.png))


Po dokončení těchto kroků, získáte v výpis 2 zobrazení pro vytváření.

**Listing 2 - Views\Product\Create.aspx**

[!code-aspx[Main](performing-simple-validation-cs/samples/sample2.aspx)]

V výpis 2 nazývá pomocná Html.ValidationSummary() okamžitě výše formuláře HTML. Tato pomocná slouží k zobrazení seznamu chybových zpráv ověření. Pomocník Html.ValidationSummary() vykreslí chyby v seznamu s odrážkami.

Pomocník Html.ValidationMessage() nazývá vedle každého pole formuláře HTML. Tento pomocník se používá k zobrazení chybovou zprávu právo vedle pole formuláře. V případě výpis 2 zobrazí pomocná Html.ValidationMessage() hvězdičku, když dojde k chybě.

Stránka na obrázku 3 znázorňuje chybové zprávy, které při odeslání formuláře s chybějící pole a neplatné hodnoty pro vykreslení ověření pomocné rutiny.


[![Dialogové okno Nový projekt](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)

**Obrázek 03**: vytvořit zobrazení odeslána s problémy ([Kliknutím zobrazit obrázek v plné velikosti](performing-simple-validation-cs/_static/image6.png))


Všimněte si, že vzhled HTML vstupní pole jsou také upravovat, když dojde k chybě ověření. Vykreslí pomocná Html.TextBox() *třídy = "input-validation-error"* atribut, když dojde k chybě ověření přidružená vlastnost vykreslen metodou Html.TextBox() helper.

Existují tři kaskádových třídy List stylu použít k řízení vzhledu chyby ověření:

- Input-validation-error - použít &lt;vstupní&gt; vykreslen metodou Html.TextBox() helper značky.
- pole validation-error - použít &lt;span&gt; vykreslen metodou Html.ValidationMessage() helper značky.
- ověření summary-errors - použít &lt;ul&gt; vykreslen metodou Html.ValidationSumamry() helper značky.

Můžete upravit tyto kaskádových stylů třídy styl a proto změnit vzhled chyby ověření úpravou souboru Site.css umístěný ve složce obsahu.

> [!NOTE] 
> 
> Třída HtmlHelper obsahuje statické vlastnosti jen pro čtení pro načítání názvů ověření šablon stylů CSS související třídy. Tyto statické vlastnosti nejsou s názvem ValidationInputCssClassName, ValidationFieldCssClassName a ValidationSummaryCssClassName.


## <a name="prebinding-validation-and-postbinding-validation"></a>Prebinding ověření a Postbinding ověření

Pokud odeslání formuláře HTML pro vytváření produktu, a zadáte neplatnou hodnotu pro toto pole price a žádná hodnota pro pole JednotkyNaSkladě, získáte ověřovacích zpráv zobrazí obrázek 4. Odkud pocházejí těchto chybových zpráv ověření?


[![Dialogové okno Nový projekt](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)

**Obrázek 04**: chyby ověření Prebinding ([Kliknutím zobrazit obrázek v plné velikosti](performing-simple-validation-cs/_static/image8.png))


Existují ve skutečnosti dva typy chybových zpráv ověření - hodnoty vytvořené před pole formuláře HTML je vázána na třídu a ty vygenerováno po pole formuláře je vázána na třídu. Jinými slovy, jsou prebinding chyby ověření a postbinding chyby ověření.

Akce Create() vystavené kontroler produktu v výpis 1 přijímá instanci třídy produktu. Podpis metodu Create vypadá takto:

[!code-csharp[Main](performing-simple-validation-cs/samples/sample3.cs)]

Hodnoty polí formuláře HTML z formuláře vytvořit jsou vázány na třídu productToCreate takzvaný vazač modelu. Výchozí vazač modelu přidá chybovou zprávu do stavu modelu automaticky při pole formuláře nemůže vytvořit vazbu na vlastnost formuláře.

Výchozí vazač modelu nelze vytvořit vazbu řetězec "apple" cena vlastnosti třídy produktu. Nelze přiřadit řetězec pro vlastnost typu desetinného čísla. Proto přidá vazač modelu chybu do stavu modelu.

Výchozí vazač modelu také nelze přiřadit hodnotu null vlastnosti, která nepřijímá hodnoty Null. Na konkrétní vazač modelu nelze přiřadit hodnotu null pro vlastnost JednotkyNaSkladě. Ještě jednou vazač modelu poskytuje a přidá chybovou zprávu do stavu modelu.

Pokud chcete přizpůsobit vzhled tyto prebinding chybové zprávy budete muset vytvořit řetězce prostředků pro tyto zprávy.

## <a name="summary"></a>Souhrn

Cílem tohoto kurzu bylo popisují základní mechanismy ověřování v rozhraní ASP.NET MVC. Jste zjistili, jak používat stav modelu a pomocné rutiny HTML ověření. Také jsme probrali rozdíl mezi prebinding a postbinding ověření. V dalších kurzech probereme různé strategie pro přesun ověřovací kód z vašich řadičů a do třídy modelu.

> [!div class="step-by-step"]
> [Předchozí](displaying-a-table-of-database-data-cs.md)
> [další](validating-with-the-idataerrorinfo-interface-cs.md)
