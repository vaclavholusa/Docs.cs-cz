---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: Začínáme s AJAX Control Toolkit (C#) | Dokumentace Microsoftu
author: microsoft
description: Přečtěte si všechno, co potřebujete vědět, abyste mohli začít používat sadou nástrojů AJAX Control Toolkit.
ms.author: aspnetcontent
ms.date: 05/12/2009
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: 3682af50eb2b9052ac0b15c2cec084deec10031d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832156"
---
<a name="get-started-with-the-ajax-control-toolkit-c"></a>Začínáme s AJAX Control Toolkit (C#)
====================
podle [Microsoft](https://github.com/microsoft)

> Přečtěte si všechno, co potřebujete vědět, abyste mohli začít používat sadou nástrojů AJAX Control Toolkit.


Sada nástrojů AJAX Control Toolkit obsahuje více než 30 bezplatné ovládací prvky, které můžete použít ve svých aplikacích technologie ASP.NET. V tomto kurzu se dozvíte, jak stáhnout sadou nástrojů AJAX Control Toolkit a přidání ovládacích prvků nástrojů do vašich nástrojů Visual Studio nebo Visual Web Developer Express.

## <a name="downloading-the-ajax-control-toolkit"></a>Stahuje se sadou nástrojů AJAX Control Toolkit

[Sadou nástrojů AJAX Control Toolkit](http://devexpress.com/act) projekt open source vyvinutý členy komunity technologie ASP.NET a tým ASP.NET. 


[![Stahuje se sadou nástrojů AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)

**Obrázek 01**: stahuje se sadou nástrojů AJAX Control Toolkit ([kliknutím ji zobrazíte obrázek v plné velikosti](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))


Po stažení souboru, budete muset odblokovat soubor. Klikněte pravým tlačítkem na soubor, vyberte vlastnosti a klikněte na tlačítko **Odblokovat** tlačítko (viz obrázek 2).


[![Odblokování souboru ZIP se sada nástrojů AJAX ovládacího prvku](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)

**Obrázek 02**: odblokování souboru ZIP se sada nástrojů AJAX ovládacího prvku ([kliknutím ji zobrazíte obrázek v plné velikosti](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))


Po odblokování ho mohli rozbalit soubor: klikněte pravým tlačítkem na soubor a vyberte **Extrahovat vše** nabídky. Nyní jsme připraveni přidat sadu nástrojů do panelu nástrojů Visual Studio nebo Visual Web Developer.

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>Přidávání do panelu nástrojů AJAX Control Toolkit

Přidat sadu nástrojů do vašich nástrojů Visual Studio nebo Visual Web Developer je nejjednodušší způsob, jak používat sadou nástrojů AJAX Control Toolkit (viz obrázek 3). Tímto způsobem můžete jednoduše přetáhnout toolkit ovládacího prvku do stránky Pokud chcete použít.


[![Sada nástrojů AJAX Control Toolkit se zobrazí v panelu nástrojů](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)

**Obrázek 03**: se zobrazí v panelu nástrojů AJAX Control Toolkit ([kliknutím ji zobrazíte obrázek v plné velikosti](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))


Nejprve budete muset přidat kartu sadou nástrojů AJAX Control Toolkit do panelu nástrojů. Postupujte podle těchto kroků.

1. Vytvoření nového webu ASP.NET tak, že vyberete možnost nabídky soubor, nový web. Klikněte dvakrát na Default.aspx v okně Průzkumník řešení otevřete soubor v editoru.
2. Panel pod na kartě Obecné klikněte pravým tlačítkem a vyberte možnost nabídky **přidat kartu** (viz obrázek 4).
3. Zadejte novou kartu s názvem sadou nástrojů AJAX Control Toolkit.


[![Přidání nové karty](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)

**Obrázek 04**: Přidání nové karty ([kliknutím ji zobrazíte obrázek v plné velikosti](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))


Dále je třeba k přidávání ovládacích prvků AJAX Control Toolkit na nové kartě. Postupujte podle těchto kroků:

- Klikněte pravým tlačítkem na pod kartě sadou nástrojů AJAX Control Toolkit a vyberte možnost nabídky **zvolit položky (viz obrázek 5)**.
- Přejděte do umístění, kde rozzipoval. Sada nástrojů AJAX Control Toolkit a vyberte AjaxControlToolkit.dll sestavení.


[![Zvolte položky, které chcete přidat do panelu nástrojů](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)

**Obrázek 05**: Zvolte položky, které chcete přidat do panelu nástrojů ([kliknutím ji zobrazíte obrázek v plné velikosti](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))


Po dokončení těchto kroků všechny ovládací prvky nástrojů se zobrazí ve vašich dovedností.

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>Upgrade na novou verzi sady nástrojů

Pokud byly pomocí starší verze sady nástrojů a teď potřebujete přesunout do novější verze jsou doporučené kroky:

- Binární soubory – odstranit starou verzi AjaxControlToolkit.dll sestavení ze složky koše vašeho webu.
- Položky panelu nástrojů – odstranit kartu sadou nástrojů AJAX Control Toolkit a postupujte podle výše uvedené kroky a znovu vytvořit na kartu s novou verzi AjaxControlToolkit.dll sestavení.

> [!div class="step-by-step"]
> [Next](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
