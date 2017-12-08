---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: "Vytváření Pomocníci vlastní HTML (C#) | Microsoft Docs"
author: microsoft
description: "Cílem tohoto kurzu je ukazují, jak můžete vytvořit vlastní pomocné rutiny HTML, který můžete použít v rámci zobrazení v rozhraní MVC. Díky pomocné rutiny HTML..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: a0b6d67eb7aab51ba2b422fab0788e34255f2c8c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="creating-custom-html-helpers-c"></a>Vytváření Pomocníci vlastní HTML (C#)
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> Cílem tohoto kurzu je ukazují, jak můžete vytvořit vlastní pomocné rutiny HTML, který můžete použít v rámci zobrazení v rozhraní MVC. S využitím pomocné rutiny HTML, můžete snížit množství zdlouhavé zadáním značky HTML, musí provést k vytvoření standardní stránky HTML.


Cílem tohoto kurzu je ukazují, jak můžete vytvořit vlastní pomocné rutiny HTML, který můžete použít v rámci zobrazení v rozhraní MVC. S využitím pomocné rutiny HTML, můžete snížit množství zdlouhavé zadáním značky HTML, musí provést k vytvoření standardní stránky HTML.

V první části tohoto kurzu I popisují některé z existujících pomocné objekty HTML součástí rozhraní ASP.NET MVC. V dalším kroku I popisují dvě metody vytváření vlastní pomocné rutiny HTML: I vysvětlují, jak vytvořit vlastní pomocné rutiny HTML a vytváření statickou metodu metody rozšíření.

## <a name="understanding-html-helpers"></a>Principy pomocné objekty HTML

Pomocné rutiny HTML je právě metoda, která vrací řetězec. Řetězec může představovat jakýkoli typ obsahu, které chcete. Pomocné rutiny HTML můžete například použít k vykreslení standardní značky HTML jako HTML `<input>` a `<img>` značky. Pomocné rutiny HTML také můžete použít k vykreslení složitější obsah, jako jsou karty pruhu nebo tabulky HTML dat z databáze.

Rozhraní ASP.NET MVC zahrnuje následující sadu standardní pomocné rutiny HTML (Toto není úplný seznam):

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

Představte si třeba formuláře v výpis 1. Tento formulář je vykreslen pomocí dva standardní pomocné objekty HTML (viz obrázek 1). Tento formulář používá `Html.BeginForm()` a `Html.TextBox()` pomocné metody pro vykreslení jednoduché formuláře HTML.


[![Stránka vykresluje se pomocné objekty HTML](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)

**Obrázek 01**: stránka vykresluje se pomocné objekty HTML ([Kliknutím zobrazit obrázek v plné velikosti](creating-custom-html-helpers-cs/_static/image3.png))


**Výpis 1 –`Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

Html.BeginForm() Pomocná metoda se používá k vytvoření HTML otevřením a uzavření `<form>` značky. Všimněte si, že `Html.BeginForm()` metoda je volána v rámci pomocí příkazu. Pomocí příkazu zajišťuje, že `<form>` značky zavřena na konci použití bloku.

Pokud dáváte přednost, místo vytvoření použití bloku, můžete volat metodu Html.EndForm() Helper zavřete `<form>` značky. Použít libovolný přístup k vytváření počáteční a uzavírání `<form>` značky, které by vám nejvíce intuitivní.

`Html.TextBox()` Pomocné metody se používají v výpis 1 k vykreslení HTML `<input>` značky. Pokud vyberete zobrazení Zdroj v prohlížeči zobrazí zdroji HTML ve výpisu 2. Všimněte si, že zdroj obsahuje standardní značky HTML.

> [!IMPORTANT]
> Všimněte si, že `Html.TextBox()`-HTML Helper je vykreslen pomocí `<%= %>` značky místo `<% %>` značky. Pokud nezadáte znaménku rovná, pak nic získá zobrazí v prohlížeči.

Rozhraní ASP.NET MVC obsahuje malou sadu pomocné rutiny. S největší pravděpodobností musíte rozšířit rozhraní MVC s vlastní pomocné rutiny HTML. Ve zbývající části tohoto kurzu další dvě metody vytváření vlastní pomocné rutiny HTML.

**Výpis 2 –`Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a>Vytváření Pomocníci HTML pomocí statických metod

Nejjednodušší způsob, jak vytvořit nový pomocné rutiny HTML, je vytvoření statickou metodu, která vrací řetězec. Představte si například, že se rozhodnete vytvořit nový pomocné rutiny HTML, který vykreslí HTML `<label>` značky. Třída v výpis 2 můžete použít k vykreslení `<label>` .

**Výpis 2 –`Helpers\LabelHelper.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

Není co speciální o třídy ve výpisu 2. `Label()` Metoda jednoduše vrátí řetězec.

Používá upravené zobrazení indexu v výpis 3 `LabelHelper` k vykreslení HTML `<label>` značky. Všimněte si, že zobrazení zahrnuje `<%@ imports %>` direktiva, která importuje `Application1.Helpers` oboru názvů.

**Výpis 2 –`Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>Vytváření Pomocníci HTML pomocí metod rozšíření

Pokud chcete vytvořit pomocné rutiny HTML, které fungují stejně jako standardní pomocné objekty HTML součástí rozhraní ASP.NET MVC, pak je potřeba vytvořit rozšiřující metody. Rozšiřující metody umožňují přidat nové metody do existující třídy. Při vytváření metody pomocné rutiny HTML, přidáte nové metody pro třídu HtmlHelper reprezentována vlastnost Html zobrazení.

Třídy ve výpisu 3 přidá metody rozšíření pro `HtmlHelper` třídu s názvem `Label()`. Existuje několik věcí, které byste měli zaznamenat o tuto třídu. První Všimněte si, že třída je statická třída. Je nutné zadat metody rozšíření pomocí statické třídy.

Druhý, Všimněte si, že první parametr `Label()` metoda je zadáno klíčové slovo `this`. První parametr metody rozšíření Určuje třídu, která rozšíření metoda rozšiřuje.

**Výpis 3 –`Helpers\LabelExtensions.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

Po vytvoření metody rozšíření a úspěšně sestavit aplikaci, metoda rozšíření se zobrazí v aplikaci Visual Studio Intellisense jako všechny ostatní metody třídy (viz obrázek 2). Jediným rozdílem je, že rozšiřující metody zobrazují se speciální symbolem vedle sebe (ikonu šipky dolů).


[![Pomocí metody rozšíření Html.Label()](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)

**Obrázek 02**: použití metody rozšíření Html.Label() ([Kliknutím zobrazit obrázek v plné velikosti](creating-custom-html-helpers-cs/_static/image6.png))


Upravené zobrazení indexu v výpis 4 používá metodu Html.Label() rozšíření pro vykreslení všechny jeho `<label>` značky.

**Výpis 4 –`Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a>Souhrn

V tomto kurzu jste se dozvěděli dvě metody vytváření vlastní pomocné rutiny HTML. Nejdříve jste zjistili, jak vytvořit vlastní `Label()` pomocné rutiny HTML tak, že vytvoříte statickou metodu, která vrací řetězec. V dalším kroku jste zjistili, jak vytvořit vlastní `Label()` metodu HTML Helper vytvořením metody rozšíření na `HtmlHelper` třídy.

V tomto kurzu I zaměřuje na budování metodu velmi jednoduché pomocné rutiny HTML. Uvědomte si, že může být složité, protože chcete, aby pomocné rutiny HTML. Můžete vytvořit pomocné rutiny HTML, která vykreslit bohaté obsahu například stromové zobrazení, nabídky nebo tabulky dat z databáze.

>[!div class="step-by-step"]
[Předchozí](asp-net-mvc-views-overview-cs.md)
[další](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
