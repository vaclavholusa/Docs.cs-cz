---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
title: "Použití třídy TagBuilder k sestavení pomocné objekty HTML (VB) | Microsoft Docs"
author: StephenWalther
description: "Stephen Walther vás seznámí s třídy užitečné nástroj v rozhraní ASP.NET MVC s názvem třídy TagBuilder. Třídy pro TagBuilder můžete snadno..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: ec26f264-d0ea-4031-9943-825505a3ac4b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: 8d0b3665e9bac6856a3fe1b50b05215f2747e354
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-vb"></a>Použití třídy TagBuilder k sestavení pomocné objekty HTML (VB)
====================
podle [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther vás seznámí s třídy užitečné nástroj v rozhraní ASP.NET MVC s názvem třídy TagBuilder. Třída TagBuilder můžete snadno vytvářet značky HTML.


Rozhraní ASP.NET MVC zahrnuje třídy užitečné nástroj s názvem TagBuilder třídu, která můžete použít při vytváření Pomocníci HTML. Třída TagBuilder, jako název třídy naznačuje, umožňuje snadno vytvářet značky HTML. V tomto kurzu stručný jsou k dispozici přehled TagBuilder třídy a další informace o použití této třídy, při vytváření jednoduchých pomocné rutiny HTML, který vykreslí HTML &lt;img&gt; značky.

## <a name="overview-of-the-tagbuilder-class"></a>Přehled třídy TagBuilder

Třída TagBuilder je obsažena v oboru názvů System.Web.Mvc. Má pět metody:

- AddCssClass() – umožňuje přidat nový *třídy = ""* atribut značku.
- GenerateId() – umožňuje přidat atribut id značky. Tato metoda automaticky nahrazuje tečky v id (ve výchozím nastavení, tečky jsou nahrazovány podtržítka)
- MergeAttribute() – umožňuje přidat atributy pro značku. Existuje více přetížení této metody.
- SetInnerText() – umožňuje nastavit vnitřní text značky. Vnitřní text je automaticky použije kódování HTML.
- ToString() – umožňuje vykreslovat značky. Můžete zadat, zda chcete vytvořit značku normální úvodní značky, koncová značka nebo samouzavírací značky.
  

Třída TagBuilder má čtyři důležité vlastnosti:

- Atributy – reprezentuje všechny atributy značky.
- IdAttributeDotReplacement – představuje znak metodou GenerateId() používá k nahrazení tečky (výchozí hodnota je podtržítko).
- InnerHTML – představuje vnitřní obsah značky. Přiřazení řetězec na tuto vlastnost *nemá* řetězec kódování HTML.
- TagName – představuje název značky.

Tyto metody a vlastnosti, získáte všechny základní metody a vlastnosti, které potřebujete k vytváření až značky jazyka HTML. Nepotřebujete skutečně použití třídy TagBuilder. Třídy StringBuilder můžete místo toho použít. Však třídu TagBuilder usnadňuje život trochu.

## <a name="creating-an-image-html-helper"></a>Vytváření pomocné rutiny HTML bitové kopie

Při vytváření instance třídy TagBuilder předáte název značky, který chcete vytvořit TagBuilder konstruktoru. Dále můžete volat metody, třeba metody AddCssClass a MergeAttribute() upravit atributy značky. Nakonec zavolejte metodu ToString() k vykreslení značky.

Například obsahuje výpis 1 pomocné rutiny HTML bitové kopie. Pomocník bitové kopie je implementováno interně s TagBuilder, který představuje HTML &lt;img&gt; značky.

**Výpis 1 – Helpers\ImageHelper.vb**

[!code-vb[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample1.vb)]

Tento modul v výpis 1 obsahuje dvě přetížené metody s názvem Image(). Při volání metody Image(), můžete předat objekt, který představuje sadu atributů HTML, nebo ne.

Všimněte si, jak TagBuilder.MergeAttribute() metoda se používá k přidání jednotlivé atributy, například atribut src k TagBuilder. Všimněte si kromě toho, jak TagBuilder.MergeAttributes() metoda se používá k přidání kolekce atributů k TagBuilder. Slovník akceptuje metodu MergeAttributes()&lt;řetězec, objekt&gt; parametr. Třída RouteValueDictionary slouží převést objekt představující kolekci atributů do slovníku&lt;řetězec, objekt&gt;.

Po vytvoření bitové kopie pomocné rutiny, můžete v zobrazení v rozhraní ASP.NET MVC stejně jako všechny ostatní standardní pomocníků HTML pomocné rutiny. Zobrazení v výpis 2 pomocí pomocníka Image dvakrát zobrazí stejné bitové kopie Xbox (viz obrázek 1). Pomocník Image() je volána s i bez zadání kolekce atributů HTML.

**Výpis 2 – Home\Index.aspx**

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample2.aspx)]


[![Dialogové okno Nový projekt](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.png)

**Obrázek 01**: využitím pomocné rutiny bitové kopie ([Kliknutím zobrazit obrázek v plné velikosti](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image2.png))


Všimněte si, že je nutné naimportovat obor názvů související s pomocnou rutinou bitové kopie v horní části Index.aspx zobrazení. Pomocné rutiny importu s direktivou následující:

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample3.aspx)]

V aplikaci Visual Basic výchozí obor názvů je stejný jako název aplikace.

>[!div class="step-by-step"]
[Předchozí](creating-custom-html-helpers-vb.md)
[další](creating-page-layouts-with-view-master-pages-vb.md)
