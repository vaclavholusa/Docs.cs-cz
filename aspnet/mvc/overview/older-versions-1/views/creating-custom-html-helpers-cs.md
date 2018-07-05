---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: Vytvoření vlastních pomocných rutin HTML (C#) | Dokumentace Microsoftu
author: microsoft
description: Cílem tohoto kurzu je předvést, jak můžete vytvořit vlastní pomocné rutiny HTML, který vám pomůže v rámci zobrazení v rozhraní MVC. S využitím pomocné rutiny HTML...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 1e5bb247f52162aba02e0d5775bced73f76d2081
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365169"
---
<a name="creating-custom-html-helpers-c"></a>Vytvoření vlastních pomocných rutin HTML (C#)
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> Cílem tohoto kurzu je předvést, jak můžete vytvořit vlastní pomocné rutiny HTML, který vám pomůže v rámci zobrazení v rozhraní MVC. S využitím pomocné rutiny HTML, můžete snížit množství tedious zadáním značky jazyka HTML, je nutné provést pro vytvoření standardní stránky HTML.


Cílem tohoto kurzu je předvést, jak můžete vytvořit vlastní pomocné rutiny HTML, který vám pomůže v rámci zobrazení v rozhraní MVC. S využitím pomocné rutiny HTML, můžete snížit množství tedious zadáním značky jazyka HTML, je nutné provést pro vytvoření standardní stránky HTML.

V první části tohoto kurzu můžu popisují některé existující pomocných rutin HTML součástí rozhraní ASP.NET MVC. V dalším kroku můžu popisují dva způsoby vytvoření vlastních pomocných rutin HTML: Mohu vysvětluje postup při vytvoření vlastních pomocných rutin HTML tak, že vytvoříte statické metody a tak, že vytvoříte metody rozšíření.

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

Představte si třeba formulář v nástrojích pro výpis 1. Tento formulář je vykreslen pomocí dvou standardní pomocných rutin HTML (viz obrázek 1). Tento formulář používá `Html.BeginForm()` a `Html.TextBox()` pomocné metody pro vykreslení jednoduchý formulář HTML.


[![Vykreslí stránku s pomocných rutin HTML](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)

**Obrázek 01**: stránka vybarvením pomocných rutin HTML ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-custom-html-helpers-cs/_static/image3.png))


**Výpis 1 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

Html.BeginForm() Pomocná metoda se používá k vytvoření HTML otevírací a zavírací `<form>` značky. Všimněte si, že `Html.BeginForm()` metoda je volána v rámci pomocí příkazu. Příkaz zajistí, že `<form>` značky zavřena na konci pomocí bloku.

Pokud dáváte přednost, místo vytvořit pomocí bloku, můžete volat metodu Html.EndForm() Helper zavřete `<form>` značky. Použít v obou případech vytváření otevření a zavření `<form>` značky, co nejvíce intuitivní pro vás.

`Html.TextBox()` Pomocné metody se používají v 1 zobrazení k vykreslení HTML `<input>` značky. Pokud zvolíte možnost Zobrazit zdroj ve vašem prohlížeči zobrazí zdroji HTML ve výpisu 2. Všimněte si, že zdroj obsahuje standardní značky HTML.

> [!IMPORTANT]
> Všimněte si, že `Html.TextBox()`pomocné rutiny HTML – je vykreslen pomocí `<%= %>` značek namísto `<% %>` značky. Pokud nechcete zahrnout znaménka rovnosti, pak nic získá zobrazí v prohlížeči.

Architektura ASP.NET MVC obsahuje malou sadu pomocné rutiny. Pravděpodobně je potřeba rozšířit rozhraní MVC pomocí vlastních pomocných rutin HTML. Ve zbývající části tohoto kurzu přečtěte si dvě metody vytvoření vlastních pomocných rutin HTML.

**Výpis 2 – `Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a>Vytváření pomocných rutin HTML pomocí statické metody

Nejjednodušší způsob, jak vytvořit nové pomocné rutiny HTML je vytvoření statická metoda, která vrátí hodnotu typu string. Představte si třeba, abyste se rozhodli vytvořit nové pomocné rutiny HTML, který vykreslí HTML `<label>` značky. Třídy v informacích 2 můžete použít k vykreslení `<label>` .

**Výpis 2 – `Helpers\LabelHelper.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

Není nic zvláštního o třídě v informacích 2. `Label()` Metoda jednoduše vrací řetězec.

Používá upravený zobrazení indexu v informacích 3 `LabelHelper` k vykreslení HTML `<label>` značky. Všimněte si, že zobrazení zahrnuje `<%@ imports %>` – direktiva, která importuje `Application1.Helpers` oboru názvů.

**Výpis 2 – `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>Vytváření pomocných rutin HTML pomocí metod rozšíření

Pokud chcete vytvořit pomocné rutiny HTML, které fungují stejně jako standardní pomocných rutin HTML zahrnuty v rozhraní ASP.NET MVC, pak je potřeba vytvořit metody rozšíření. Rozšiřující metody umožňují přidat nové metody do existující třídy. Při vytváření metodu pomocné rutiny HTML, přidat nové metody pro třídu HtmlHelper reprezentována vlastnosti zobrazení Html.

Třída ve výpisu 3 přidá metodu rozšíření k `HtmlHelper` třídu s názvem `Label()`. Existuje několik věcí, které byste si měli všimnout o této třídy. Napřed si všimněte, že třída je statická třída. Je nutné definovat rozšiřující metodu se statické třídě.

Za druhé, Všimněte si, že první parametr `Label()` metoda je před klíčové slovo `this`. První parametr rozšiřující metody Určuje třídu, která rozšiřující metoda rozšiřuje.

**Výpis 3 – `Helpers\LabelExtensions.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

Po vytvoření rozšiřující metodu a sestavení aplikace úspěšně, metoda rozšíření se zobrazí v Intellisense ve Visual Studio jako všechny ostatní metody třídy (viz obrázek 2). Jediným rozdílem je tohoto rozšíření, které metody mají speciální symbol vedle sebe (ikonu šipky dolů).


[![Pomocí metody rozšíření Html.Label()](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)

**Obrázek 02**: použití metody rozšíření Html.Label() ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-custom-html-helpers-cs/_static/image6.png))


Upravené zobrazení indexu v informacích 4 používá metodu rozšíření Html.Label() k vykreslení všech jeho `<label>` značky.

**Část 4 – `Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a>Souhrn

V tomto kurzu jste zjistili, dva způsoby vytvoření vlastních pomocných rutin HTML. Nejdřív jste zjistili, jak můžete vytvořit vlastní `Label()` pomocné rutiny HTML tak, že vytvoříte statická metoda, která vrátí hodnotu typu string. Dále jste zjistili, jak můžete vytvořit vlastní `Label()` metody pomocné rutiny HTML, vytvořte rozšiřující metody pro `HtmlHelper` třídy.

V tomto kurzu můžu soustředit na vytváření velmi jednoduchý způsob pomocné rutiny HTML. Uvědomte si, že pomocné rutiny HTML, může být složité, jak chcete. Pomocné rutiny HTML, vykreslení formátovaného obsahu, jako je například stromová zobrazení, nabídky nebo tabulky databázových dat můžete vytvořit.

> [!div class="step-by-step"]
> [Předchozí](asp-net-mvc-views-overview-cs.md)
> [další](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
