---
uid: mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
title: Vytvoření vlastních pomocných rutin HTML (VB) | Dokumentace Microsoftu
author: microsoft
description: Cílem tohoto kurzu je předvést, jak můžete vytvořit vlastní pomocné rutiny HTML, který vám pomůže v rámci zobrazení v rozhraní MVC. S využitím pomocné rutiny HTML...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: f96f4800-19ef-44c0-b457-55e777eb5de8
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: e62e47bceddc516af7aa18fc66ed4ca4d704d277
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755430"
---
<a name="creating-custom-html-helpers-vb"></a>Vytvoření vlastních pomocných rutin HTML (VB)
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_VB.pdf)

> Cílem tohoto kurzu je předvést, jak můžete vytvořit vlastní pomocné rutiny HTML, který vám pomůže v rámci zobrazení v rozhraní MVC. S využitím pomocné rutiny HTML, můžete snížit množství tedious zadáním značky jazyka HTML, je nutné provést pro vytvoření standardní stránky HTML.


Cílem tohoto kurzu je předvést, jak můžete vytvořit vlastní pomocné rutiny HTML, který vám pomůže v rámci zobrazení v rozhraní MVC. S využitím pomocné rutiny HTML, můžete snížit množství tedious zadáním značky jazyka HTML, je nutné provést pro vytvoření standardní stránky HTML.

V první části tohoto kurzu můžu popisují některé existující pomocných rutin HTML součástí rozhraní ASP.NET MVC. V dalším kroku můžu popisují dva způsoby vytvoření vlastních pomocných rutin HTML: Mohu vysvětluje postup při vytvoření vlastních pomocných rutin HTML tak, že vytvoříte sdílené metody a tak, že vytvoříte metody rozšíření.

## <a name="understanding-html-helpers"></a>Principy pomocných rutin HTML

Pomocné rutiny HTML je právě metodu, která vrátí hodnotu typu string. Řetězec může představovat jakýkoli typ obsahu, které chcete. Například můžete použít pomocných rutin HTML pro vykreslení standardní značky HTML, jako je HTML `<input>` a `<img>` značky. Pomocné rutiny HTML můžete použít také k vykreslení složitější obsah, jako jsou pruhu karet nebo HTML tabulku dat z databáze.

Architektura ASP.NET MVC zahrnuje následující sadu standardních pomocných rutin HTML (Toto není kompletní seznam):

- Html.ActionLink()
- Html.BeginForm()
- Html.CheckBox()
- Html.DropDownList()
- Html.EndForm()
- Html.Hidden()
- Html.ListBox()
- Html.Password()
- Html.RadioButton()
- Html.TextArea()
- Html.TextBox()

Představte si třeba formulář v nástrojích pro výpis 1. Tento formulář je vykreslen pomocí dvou standardní pomocných rutin HTML (viz obrázek 1). Tento formulář používá `Html.BeginForm()` a `Html.TextBox()` pomocné metody.


[![Vykreslí stránku s pomocných rutin HTML](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)

**Obrázek 01**: stránka vybarvením pomocných rutin HTML ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-custom-html-helpers-vb/_static/image3.png))


**Výpis 1 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample1.aspx)]

`Html.BeginForm()` Pomocná metoda se používá k vytvoření HTML otevírací a zavírací `<form>` značky. Všimněte si, že `Html.BeginForm()` metoda je volána v rámci pomocí příkazu. Příkaz zajistí, že `<form>` značky zavřena na konci pomocí bloku.

Pokud dáváte přednost, místo vytvořit pomocí bloku, můžete volat metodu Html.EndForm() Helper zavřete `<form>` značky. Použít v obou případech vytváření otevření a zavření `<form>` značky, co nejvíce intuitivní pro vás.

`Html.TextBox()` Pomocné metody se používají v 1 zobrazení k vykreslení HTML `<input>` značky. Pokud zvolíte možnost Zobrazit zdroj ve vašem prohlížeči zobrazí zdroji HTML ve výpisu 2. Všimněte si, že zdroj obsahuje standardní značky HTML.

> [!IMPORTANT]
> Všimněte si, že `Html.TextBox()`pomocné rutiny HTML – je vykreslen pomocí `<%= %>` značek namísto `<% %>` značky. Pokud nechcete zahrnout znaménka rovnosti, pak nic získá zobrazí v prohlížeči.

Architektura ASP.NET MVC obsahuje malou sadu pomocné rutiny. Pravděpodobně je potřeba rozšířit rozhraní MVC pomocí vlastních pomocných rutin HTML. Ve zbývající části tohoto kurzu přečtěte si dvě metody vytvoření vlastních pomocných rutin HTML.

**Výpis 2 – `Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-shared-methods"></a>Vytvoření sdílené metody pomocných rutin HTML

Nejjednodušší způsob, jak vytvořit nové pomocné rutiny HTML je vytvoření sdíleného metodu, která vrátí hodnotu typu string. Představte si třeba, abyste se rozhodli vytvořit nové pomocné rutiny HTML, který vykreslí HTML `<label>` značky. Třídy v informacích 2 můžete použít k vykreslení `<label>`.

**Výpis 2 – `Helpers\LabelHelper.vb`**

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample3.vb)]

Není nic zvláštního o třídě v informacích 2. `Label()` Metoda jednoduše vrací řetězec.

Používá upravený zobrazení indexu v informacích 3 `LabelHelper` k vykreslení HTML `<label>` značky. Všimněte si, že zobrazení zahrnuje `<%@ imports %>` – direktiva, která importuje Application1.Helpers oboru názvů.

**Výpis 2 – `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>Vytváření pomocných rutin HTML pomocí metod rozšíření

Pokud chcete vytvořit pomocné rutiny HTML, které fungují stejně jako standardní pomocných rutin HTML zahrnuty v rozhraní ASP.NET MVC, pak je potřeba vytvořit metody rozšíření. Rozšiřující metody umožňují přidat nové metody do existující třídy. Při vytváření metodu pomocné rutiny HTML, přidat nové metody `HtmlHelper` třída představovaná typem vlastnosti zobrazení Html.

Modul Visual Basic v informacích 3 přidá metodu rozšíření s názvem `Label()` k `HtmlHelper` třídy. Existuje několik věcí, které byste si měli všimnout o tomto modulu. Napřed si všimněte, že je doplněn modulu `<Extension()>` atribut. Chcete-li použít tento atribut, je nutné naimportovat `System.Runtime.CompilerServices` obor názvů

Za druhé, Všimněte si, že první parametr `Label()` představuje metodu `HtmlHelper` třídy. První parametr rozšiřující metody Určuje třídu, která rozšiřující metoda rozšiřuje.

**Výpis 3 – `Helpers\LabelExtensions.vb`**

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample5.vb)]

Po vytvoření rozšiřující metodu a sestavení aplikace úspěšně, metoda rozšíření se zobrazí v Intellisense ve Visual Studio jako všechny ostatní metody třídy (viz obrázek 2). Jediným rozdílem je tohoto rozšíření, které metody mají speciální symbol vedle sebe (ikonu šipky dolů).


[![Pomocí metody rozšíření Html.Label()](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)

**Obrázek 02**: použití metody rozšíření Html.Label() ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-custom-html-helpers-vb/_static/image6.png))


Upravené zobrazení indexu v informacích 4 používá metodu rozšíření Html.Label() k vykreslení všech jeho &lt;popisek&gt; značky.

**Část 4 – `Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample6.aspx)]

## <a name="summary"></a>Souhrn

V tomto kurzu jste zjistili, dva způsoby vytvoření vlastních pomocných rutin HTML. Nejdřív jste zjistili, jak můžete vytvořit vlastní `Label()` pomocné rutiny HTML tak, že vytvoříte sdílenou metodu, která vrátí hodnotu typu string. Dále jste zjistili, jak můžete vytvořit vlastní `Label()` metody pomocné rutiny HTML, vytvořte rozšiřující metody pro `HtmlHelper` třídy.

V tomto kurzu můžu soustředit na vytváření velmi jednoduchý způsob pomocné rutiny HTML. Uvědomte si, že pomocné rutiny HTML, může být složité, jak chcete. Pomocné rutiny HTML, vykreslení formátovaného obsahu, jako je například stromová zobrazení, nabídky nebo tabulky databázových dat můžete vytvořit.

> [!div class="step-by-step"]
> [Předchozí](asp-net-mvc-views-overview-vb.md)
> [další](using-the-tagbuilder-class-to-build-html-helpers-vb.md)
