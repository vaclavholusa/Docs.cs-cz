---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
title: "Začínáme s Toolkitu AJAX (VB) | Microsoft Docs"
author: microsoft
description: "Přečtěte si vše, co potřebujete vědět, abyste mohli začít používat sadu ovládacích prvků AJAX."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 9f8fa166-49a2-402c-b236-20caef0c658f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
msc.type: authoredcontent
ms.openlocfilehash: 0bbf6dc0be8a96ecd47b8620a6ba3220b50f10d4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="get-started-with-the-ajax-control-toolkit-vb"></a>Začínáme s Toolkitu AJAX (VB)
====================
podle [Microsoft](https://github.com/microsoft)

> Přečtěte si vše, co potřebujete vědět, abyste mohli začít používat sadu ovládacích prvků AJAX.


Toolkitu AJAX obsahuje více než 30 volné ovládací prvky, které můžete použít v aplikacích ASP.NET. V tomto kurzu zjistěte, jak stáhnout sadu ovládacích prvků AJAX a přidání nástrojů ovládacích prvků do vaší sady nástrojů Visual Studio nebo Visual Web Developer Express.

## <a name="downloading-the-ajax-control-toolkit"></a>Stahování Toolkitu AJAX

[Sadu ovládacích prvků AJAX](http://devexpress.com/act) opensourcový projekt vyvinutý členy komunity ASP.NET a týmem ASP.NET.


[![Stahování Toolkitu AJAX](get-started-with-the-ajax-control-toolkit-vb/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image1.png)

**Obrázek 01**: stahování Toolkitu AJAX ([Kliknutím zobrazit obrázek v plné velikosti](get-started-with-the-ajax-control-toolkit-vb/_static/image2.png))


Po stažení souboru, budete muset odblokovat soubor. Klikněte pravým tlačítkem na soubor, vyberte vlastnosti a klikněte **Odblokovat** tlačítko (viz obrázek 2).


[![Odblokování soubor ZIP Toolkit AJAX ovládací prvek](get-started-with-the-ajax-control-toolkit-vb/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image3.png)

**Obrázek 02**: odblokování soubor ZIP Toolkit řízení AJAX ([Kliknutím zobrazit obrázek v plné velikosti](get-started-with-the-ajax-control-toolkit-vb/_static/image4.png))


Po odblokování souboru můžete rozbalte soubor: klikněte pravým tlačítkem na soubor a vyberte **Extrahovat vše** možnost nabídky. Nyní jsme připraveni pro přidání do sady nástrojů Visual Studio nebo Visual Web Developer sady nástrojů.

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>Přidání Toolkitu AJAX do sady nástrojů

Nejjednodušší způsob, jak používat sadu ovládacích prvků AJAX je přidání sady nástrojů do vaší sady nástrojů Visual Studio nebo Visual Web Developer (viz obrázek 3). Tímto způsobem můžete jednoduše přetáhnout toolkit ovládacího prvku na stránku Pokud chcete použít.


[![Sadu ovládacích prvků AJAX se zobrazí v sadě nástrojů](get-started-with-the-ajax-control-toolkit-vb/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image5.png)

**Obrázek 03**: sadu ovládacích prvků AJAX se zobrazí v sadě nástrojů ([Kliknutím zobrazit obrázek v plné velikosti](get-started-with-the-ajax-control-toolkit-vb/_static/image6.png))


Nejdřív musíte přidat kartě sadu ovládacích prvků AJAX do sady nástrojů. Postupujte podle těchto kroků.

1. Vytvoření nového webu ASP.NET tak, že vyberete možnost nabídky soubor, nový web. Dvakrát klikněte na Default.aspx v okně Průzkumníka řešení k otevření souboru v editoru.
2. Sada nástrojů pod na kartě Obecné klikněte pravým tlačítkem a vyberte možnost nabídky **přidat karta** (viz obrázek 4).
3. Zadejte novou kartu s názvem sadu ovládacích prvků AJAX.


[![Přidání novou kartu](get-started-with-the-ajax-control-toolkit-vb/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image7.png)

**Obrázek 04**: Přidání novou kartu ([Kliknutím zobrazit obrázek v plné velikosti](get-started-with-the-ajax-control-toolkit-vb/_static/image8.png))


Dále musíte přidat ovládací prvky sadu ovládacích prvků AJAX na novou kartu. Postupujte podle těchto kroků:

- Klikněte pravým tlačítkem na pod kartě sadu ovládacích prvků AJAX a vyberte možnost nabídky **zvolit položky (viz obrázek 5)**.
- Přejděte do umístění, kde unzipped Toolkitu AJAX a vyberte AjaxControlToolkit.dll sestavení.


[![Zvolte položky, které chcete přidat do sady nástrojů](get-started-with-the-ajax-control-toolkit-vb/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image9.png)

**Obrázek 05**: Zvolte položky, které chcete přidat do sady nástrojů ([Kliknutím zobrazit obrázek v plné velikosti](get-started-with-the-ajax-control-toolkit-vb/_static/image10.png))


Po dokončení těchto kroků se ve vašem panelu nástrojů zobrazí všechny ovládací prvky toolkit.

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>Upgrade na novou verzi sady nástrojů

Pokud jste používali starší verze sady nástrojů a teď potřebujete přesunout do novější verze jsou doporučené kroky:

- Binární soubory – odstranit stará verze sestavení AjaxControlToolkit.dll ze složky koše webu.
- Položek sady nástrojů – odstranit kartě sadu ovládacích prvků AJAX a znovu vytvořit na kartě s novou verzi sestavení AjaxControlToolkit.dll výše uvedených kroků.

>[!div class="step-by-step"]
[Předchozí](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
[další](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
