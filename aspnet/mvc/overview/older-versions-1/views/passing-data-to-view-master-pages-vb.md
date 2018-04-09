---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
title: Předávání dat zobrazit stránky předlohy (VB) | Microsoft Docs
author: microsoft
description: Cílem tohoto kurzu je vysvětlují, jak můžete předat data z řadiče na hlavní stránku zobrazení. Jsme zkontrolujte dvě strategie pro předávání dat zobrazení m...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: 37a1ebae-8773-408f-8645-d21da7ff9ae1
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: fcd7c5baacc00490720d1f82252d81e40c097c88
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="passing-data-to-view-master-pages-vb"></a>Předání dat zobrazit stránky předlohy (VB)
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_VB.pdf)

> Cílem tohoto kurzu je vysvětlují, jak můžete předat data z řadiče na hlavní stránku zobrazení. Jsme zkontrolujte dvě strategie pro předávání dat na hlavní stránku zobrazení. Nejprve probereme snadno řešení, která vyústí v aplikaci, která je obtížné. Dále zkontrolujte jsme mnohem lepší řešení, které vyžaduje trochu další počáteční práci, ale má za následek víc udržovatelný aplikace.


## <a name="passing-data-to-view-master-pages"></a>Předání dat zobrazit stránky předlohy

Cílem tohoto kurzu je vysvětlují, jak můžete předat data z řadiče na hlavní stránku zobrazení. Jsme zkontrolujte dvě strategie pro předávání dat na hlavní stránku zobrazení. Nejprve probereme snadno řešení, která vyústí v aplikaci, která je obtížné. Dále zkontrolujte jsme mnohem lepší řešení, které vyžaduje trochu další počáteční práci, ale má za následek víc udržovatelný aplikace.

### <a name="the-problem"></a>Problém

Představte si, že vytváříte databázovou aplikaci film a chcete zobrazit seznam kategorií film na každé stránce v aplikaci (viz obrázek 1). Představte si kromě toho, že seznam kategorií film je uložený v tabulce databáze. V takovém případě ji by mít smysl pro načtení daných kategorií z databáze a vykreslení seznamu kategorií film v rámci zobrazení stránky předlohy.


[![Zobrazení kategorií film v zobrazení stránky předlohy](passing-data-to-view-master-pages-vb/_static/image2.png)](passing-data-to-view-master-pages-vb/_static/image1.png)

**Obrázek 01**: zobrazení kategorií film na hlavní stránce zobrazení ([Kliknutím zobrazit obrázek v plné velikosti](passing-data-to-view-master-pages-vb/_static/image3.png))


Zde je problém. Jak můžete načíst seznam kategorií film na hlavní stránce? Je tempting pro volání metod tříd modelu přímo na hlavní stránce. Jinými slovy je tempting zahrnout kód pro načítání dat z databáze vpravo na hlavní stránce. Obcházení vaše řadiče MVC pro přístup k databázi by porušení čistou oddělené oblasti zájmu, který je jedním z primární výhody sestavení aplikace MVC.

V aplikaci MVC chcete všechny interakce mezi zobrazení v rozhraní MVC a modelu MVC zpracovávat své řadiče MVC. Tato oddělené oblasti zájmu výsledkem udržovatelný, přizpůsobivé a možností intenzivního testování aplikace.

V aplikaci MVC by měla být předána do zobrazení – včetně zobrazení stránky předlohy – všechna data předána zobrazení pomocí akce kontroleru. Kromě toho mají být předány data a využívají data zobrazení. Ve zbývající části tohoto kurzu zkontrolujte I dvě metody předávání dat zobrazení na hlavní stránku zobrazení.

### <a name="the-simple-solution"></a>Jednoduchým řešením

Začněme nejjednodušší řešení předávání zobrazení dat z řadiče zobrazení stránky předlohy. Nejjednodušší řešení je předat data zobrazení pro stránku předlohy v každé akce kontroleru.

Vezměte v úvahu řadiče v výpis 1. Poskytuje dvě akce s názvem `Index()` a `Details()`. `Index()` Metoda akce vrací každých film filmy databázové tabulky. `Details()` Metoda akce vrací každých film v konkrétní film kategorii.

**Výpis 1 – `Controllers\HomeController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample1.vb)]

Všimněte si, že jak `Index()` a `Details()` dvě položky k zobrazení dat přidat akce. `Index()` Akce přidá dva klíče: kategorií a filmy. Klíč kategorie představuje seznam kategorií film zobrazí při zobrazení stránky předlohy. Klíč filmy představuje seznam filmy zobrazí stránku zobrazení indexu.

`Details()` Akce také přidá dva klíče s názvem kategorie a filmy. Kategorie klíč znovu, představuje seznam kategorií film zobrazí při zobrazení stránky předlohy. Klíč filmy představuje seznam filmy v určité kategorii zobrazí stránku zobrazení podrobností (viz obrázek 2).


[![Zobrazení podrobností](passing-data-to-view-master-pages-vb/_static/image5.png)](passing-data-to-view-master-pages-vb/_static/image4.png)

**Obrázek 02**: zobrazení podrobností ([Kliknutím zobrazit obrázek v plné velikosti](passing-data-to-view-master-pages-vb/_static/image6.png))


Zobrazení indexu je obsažený v výpis 2. Ho jednoduše prochází seznam filmy reprezentována filmy položky v dat zobrazení.

**Výpis 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample2.aspx)]

Hlavní stránka zobrazení je součástí výpis 3. Hlavní stránka zobrazení opakuje a vykreslí všechny kategorie film reprezentována kategorie položku z dat zobrazení.

**Výpis 3 – `Views\Shared\Site.master`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample3.aspx)]

Všechna data předána do zobrazení a zobrazení stránky předlohy dat zobrazení. Který je správný způsob, jak předat data do stránky předlohy.

Ano co je problém s tímto řešením? Problém je, že toto řešení porušíte Princip suchého (nemáte opakujte sami). Velmi stejný seznam kategorií film k zobrazení dat musíte přidat každé akce kontroleru. S duplicitní kódu v aplikaci je vaše aplikace mnohem víc obtížné spravovat, přizpůsobit a upravovat.

### <a name="the-good-solution"></a>Dobrým řešením

V této části jsme zkontrolujte alternativní a lepší, řešení předávání dat z akce kontroleru na hlavní stránku zobrazení. Místo přidávání kategorií film stránky předlohy v každé akce kontroleru, přidáme kategorií film k zobrazení dat pouze jednou. Všechna data zobrazení, které jsou používané na hlavní stránce zobrazení je přidaný do řadič aplikace.

Třída ApplicationController je součástí výpis 4.

Třída ApplicationController je součástí výpis 4.

**Výpis 4 – `Controllers\ApplicationController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample4.vb)]

Existují tři věci, které byste měli zaznamenat o kontroleru aplikace v výpis 4. První, Všimněte si, že třídy dědí vlastnosti ze základní třídy System.Web.Mvc.Controller. Řadič aplikace je třídy kontroleru.

Druhý Všimněte si, že třída controller aplikace je MustInherit – třída. Třídu MustInherit je třída, která musí být implementována podle konkrétní třídy. Protože řadičem aplikace je třída MustInherit, nejde volat žádné metody definovaný ve třídě přímo. Pokud se pokusíte přímo volat třída aplikace získáte chybová zpráva prostředek nelze nalézt.

Třetí Všimněte si, že řadič aplikace obsahuje konstruktor, který se přidá seznam kategorií film k zobrazení dat. Každá třída řadiče, který dědí z aplikace řadiče volá konstruktor řadič aplikace automaticky. Vždy, když zavoláte na žádném řadiči, který dědí z aplikace řadiče žádnou akci, kategorie film je součástí data zobrazení automaticky.

Řadič filmy v výpis 5 dědí z aplikace řadiče.

**Výpis 5 – `Controllers\MoviesController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample5.vb)]

Řadič filmy, stejně jako řadič domovské popsané v předchozí části, zpřístupní dvě metody akce s názvem `Index()` a `Details()`. Všimněte si, že seznam kategorií film zobrazí při zobrazení stránky předlohy není přidán do zobrazení dat buď `Index()` nebo `Details()` metoda. Protože řadičem filmy dědí z aplikace řadiče, ze seznamu kategorií film se přidá do zobrazení dat automaticky.

Všimněte si, že toto řešení přidání zobrazit data pro zobrazení stránky předlohy nesplňují zásady suchého (nemáte opakujte sami). Kód pro přidání seznamu kategorií film k zobrazení dat je součástí pouze jedno umístění: v konstruktoru pro řadič aplikace.

### <a name="summary"></a>Souhrn

V tomto kurzu jsme probrali dva přístupy k předávání dat zobrazení z řadiče zobrazení stránky předlohy. Nejdřív jsme se zaměřili na jednoduchý, ale poměrně složitá přístup. V první části jsme se bavili, jak přidat data zobrazení pro stránku předlohy pro zobrazení v každé každou akci kontroleru ve vaší aplikaci. Jsme k závěru, to byl chybný přístup, protože ho porušíte Princip suchého (nemáte opakujte sami).

V dalším kroku jsme se zaměřili mnohem lepší strategie pro přidání dat vyžaduje zobrazení stránky předlohy pro zobrazení dat. Místo přidávání zobrazení dat v každé akce kontroleru, jsme přidali zobrazení dat pouze jednou v rámci řadič aplikace. Tímto způsobem, při předávání dat na hlavní stránku zobrazení v aplikaci ASP.NET MVC se můžete vyhnout duplicitní kódu.

> [!div class="step-by-step"]
> [Předchozí](creating-page-layouts-with-view-master-pages-vb.md)
