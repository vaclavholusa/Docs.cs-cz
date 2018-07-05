---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
title: Předání dat stránkám předlohy pro zobrazení (VB) | Dokumentace Microsoftu
author: microsoft
description: Cílem tohoto kurzu je vysvětlují, jak můžete předat data z kontroleru na hlavní stránku zobrazení. Prozkoumáme dvou strategií pro předávání dat k zobrazení m...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: 37a1ebae-8773-408f-8645-d21da7ff9ae1
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: e70f15d98101336dbef31b4f9d8b958632e46c01
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388514"
---
<a name="passing-data-to-view-master-pages-vb"></a>Předání dat stránkám předlohy pro zobrazení (VB)
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_VB.pdf)

> Cílem tohoto kurzu je vysvětlují, jak můžete předat data z kontroleru na hlavní stránku zobrazení. Prozkoumáme dvou strategií pro předávání dat na hlavní stránku zobrazení. Nejprve si popíšeme jednoduchém řešení, která vede aplikaci, která je obtížné udržovat. V dalším kroku prozkoumáme mnohem lepší řešení, která vyžaduje trochu více práce počáteční ale výsledky v mnohem jednodušší údržbu aplikace.


## <a name="passing-data-to-view-master-pages"></a>Předání dat stránkám předlohy pro zobrazení

Cílem tohoto kurzu je vysvětlují, jak můžete předat data z kontroleru na hlavní stránku zobrazení. Prozkoumáme dvou strategií pro předávání dat na hlavní stránku zobrazení. Nejprve si popíšeme jednoduchém řešení, která vede aplikaci, která je obtížné udržovat. V dalším kroku prozkoumáme mnohem lepší řešení, která vyžaduje trochu více práce počáteční ale výsledky v mnohem jednodušší údržbu aplikace.

### <a name="the-problem"></a>Problém

Představte si, že vytváříte aplikace movie database a chcete zobrazit seznam kategorií video na každé stránce v aplikaci (viz obrázek 1). Představte si kromě toho, že seznam kategorií video je uložena v tabulce databáze. V takovém případě to dává smysl pro načtení daných kategorií z databáze a zobrazit seznam kategorií filmu v rámci hlavní stránky zobrazení.


[![Zobrazení kategorií filmu v zobrazení stránky předlohy](passing-data-to-view-master-pages-vb/_static/image2.png)](passing-data-to-view-master-pages-vb/_static/image1.png)

**Obrázek 01**: zobrazení kategorií filmu v zobrazení stránky předlohy ([kliknutím ji zobrazíte obrázek v plné velikosti](passing-data-to-view-master-pages-vb/_static/image3.png))


Tady je problém. Jak načtete seznam kategorií video na stránce předlohy Je lákavé přímo volat metody třídy modelu na hlavní stránce. Jinými slovy je lákavé obsahovalo kód pro načítání dat z databáze přímo v hlavní stránku. Ale obcházení vaše řadiče MVC pro přístup k databázi by mohla narušit jasně oddělit oblasti zájmu, který je jedním z hlavních výhod sestavení aplikace MVC.

V aplikaci MVC chcete veškerou interakci mezi zobrazení v rozhraní MVC a modelu MVC zpracovávat vaše řadiče MVC. Toto oddělení oblastí zájmu výsledkem aplikace snadněji udržovatelné, přizpůsobitelné a možností intenzivního testování.

V aplikaci MVC všechna data předaná do zobrazení – včetně zobrazení stránky předlohy – by měly být předány zobrazení podle akce kontroleru. Kromě toho data předat s využitím dat zobrazení. Ve zbývající části tohoto kurzu můžu zkontrolovat dva způsoby předávání zobrazení dat na hlavní stránku zobrazení.

### <a name="the-simple-solution"></a>Jednoduché řešení

Začněme nejjednodušší řešení k předávání dat zobrazení z kontroleru na hlavní stránku zobrazení. Nejjednodušším řešením je předat data zobrazení pro stránku předlohy v každé akce kontroleru.

Vezměte v úvahu kontroleru v zobrazení 1. Poskytuje dvě akce s názvem `Index()` a `Details()`. `Index()` Metoda akce vrací každý film v tabulce databáze filmů. `Details()` Metoda akce vrací každé video v konkrétním film kategorii.

**Výpis 1 – `Controllers\HomeController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample1.vb)]

Všimněte si, jak `Index()` a `Details()` akce přidání dvou položek, chcete-li zobrazit data. `Index()` Akce ho přidá dva klíče: kategorie a videa. Klíč kategorie představuje seznam kategorií video zobrazí při zobrazení stránky předlohy. Klíč filmy představuje seznam filmy zobrazený indexovou stránku zobrazení.

`Details()` Akce také přidá dva klíče s názvem kategorie a videa. Kategorie klíč znovu, představuje seznamu kategorií video zobrazí při zobrazení stránky předlohy. Klíč filmy představuje seznam video v konkrétní kategorii, zobrazí na stránce podrobností zobrazení (viz obrázek 2).


[![Zobrazení podrobností](passing-data-to-view-master-pages-vb/_static/image5.png)](passing-data-to-view-master-pages-vb/_static/image4.png)

**Obrázek 02**: zobrazení The podrobností ([kliknutím ji zobrazíte obrázek v plné velikosti](passing-data-to-view-master-pages-vb/_static/image6.png))


Zobrazení indexu je obsažen v informacích 2. Jednoduše Iteruje přes seznam filmy reprezentována filmy položku v zobrazení data.

**Výpis 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample2.aspx)]

Hlavní stránka zobrazení je součástí výpis 3. Hlavní stránka zobrazení iterace a vykreslí všechny kategorie film reprezentována kategorií položku z dat zobrazení.

**Výpis 3 – `Views\Shared\Site.master`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample3.aspx)]

Všechna data, data zobrazení předána do zobrazení a zobrazení stránky předlohy. Který je správný způsob, jak předat data do stránky předlohy.

To co je špatně pomocí tohoto řešení? Problém je, že toto řešení porušuje Princip suchého (není opakujte sami). Velmi stejný seznam kategorií video k zobrazení dat musíte přidat každého akce kontroleru. S duplicitního kódu v aplikaci je aplikace mnohem obtížnější spravovat, přizpůsobovat a upravovat.

### <a name="the-good-solution"></a>Dobrým řešením

V této části Zkoumáme, alternativní a lepší, řešení pro předání dat z akce kontroleru na hlavní stránku zobrazení. Místo přidávání kategorií film stránky předlohy v každé akce kontroleru, přidáme kategorie video k zobrazení dat pouze jednou. Řadič služby aplikace přidá všechny zobrazení dat používaných zobrazení stránky předlohy.

Třída ApplicationController je obsažena v informacích 4.

Třída ApplicationController je obsažena v informacích 4.

**Část 4 – `Controllers\ApplicationController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample4.vb)]

Existují tři věci, které byste si měli všimnout o kontroleru aplikace v informacích 4. Napřed si všimněte, že třída dědí ze základní třídy System.Web.Mvc.Controller. Kontroler aplikací je třídu kontroleru.

Za druhé Všimněte si, že třída kontroleru aplikace MustInherit – třídy. MustInherit – třída je třída, která se musí implementovat konkrétní třídou. Protože kontroler aplikací je třída MustInherit, nejde vyvolat jakékoli metody definované ve třídě přímo. Pokud se budete pokoušet vyvolat – třída aplikace přímo získáte chybovou zprávu prostředek nejde najít.

Třetí, Všimněte si, že kontroler aplikací obsahuje konstruktor, který přidá seznam kategorií video Chcete-li zobrazit data. Každý řadič třídu, která dědí z kontroleru aplikace volá konstruktor kontroleru aplikace automaticky. Pokaždé, když se bude volat žádnou akci na žádném řadiči, která dědí z kontroleru aplikace, kategorie video je součástí dat zobrazení automaticky.

Kontroler filmy výpis 5 dědí z kontroleru aplikace.

**Výpis 5 – `Controllers\MoviesController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample5.vb)]

Kontroler filmy, stejně jako kontroler Home, které jsou popsané v předchozí části, poskytuje dvě metody akce s názvem `Index()` a `Details()`. Všimněte si, že seznam kategorií video zobrazí na hlavní stránce zobrazení není přidán do zobrazení dat v jednom `Index()` nebo `Details()` metody. Protože kontroler filmy dědí z kontroleru aplikace, seznam kategorií film se přidá do zobrazení dat. automaticky.

Všimněte si, že toto řešení k přidání dat zobrazení pro stránku předlohy pro zobrazení nebudou porušovat zásady suchého (není opakujte sami). Kód pro přidání seznamu kategorií video k zobrazení dat nachází pouze v jednom umístění: konstruktor pro kontroler aplikací.

### <a name="summary"></a>Souhrn

V tomto kurzu jsme probírali dva přístupy k předávání dat zobrazení z kontroleru na hlavní stránku zobrazení. Nejprve jsme se zaměřili na jednoduchý, ale obtížné udržovat přístup. V první části jsme zmínili, jak přidat data zobrazení pro stránku předlohy pro zobrazení v jednotlivých každé akce kontroleru ve vaší aplikaci. Jsme k závěru, že to bylo chybné přístup, protože porušuje Princip suchého (není opakujte sami).

Dále jsme se zaměřili na mnohem lepší strategie pro přidání data požadovaná k zobrazení dat na stránku předlohy pro zobrazení. Nepřidávat zobrazit data v každé akce kontroleru, přidali jsme zobrazit data pouze jednou v rámci řadič aplikace. Tímto způsobem se vyhnete duplicitního kódu při předávání dat na hlavní stránku zobrazení v aplikaci ASP.NET MVC.

> [!div class="step-by-step"]
> [Předchozí](creating-page-layouts-with-view-master-pages-vb.md)
