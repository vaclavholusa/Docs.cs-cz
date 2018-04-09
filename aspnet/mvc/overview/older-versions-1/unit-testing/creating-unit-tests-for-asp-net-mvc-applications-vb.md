---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
title: Vytváření testů jednotek pro aplikace ASP.NET MVC (VB) | Microsoft Docs
author: StephenWalther
description: Naučte se vytvářet testy částí pro akce kontroleru. V tomto kurzu Stephen Walther ukazuje, jak otestovat, zda akce kontroleru vrátí sloupce části...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: eb35710d-1d99-44ac-b61f-e50af8cb328a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
msc.type: authoredcontent
ms.openlocfilehash: 299665f45d72fee33f92344ed53c87dfb1a76d60
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-vb"></a>Vytváření testů jednotek pro aplikace ASP.NET MVC (VB)
====================
podle [Stephen Walther](https://github.com/StephenWalther)

[Stáhnout PDF](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_VB.pdf)

> Naučte se vytvářet testy částí pro akce kontroleru. V tomto kurzu Stephen Walther ukazuje, jak otestovat, zda akce kontroleru vrátí konkrétní zobrazení, vrátí sadu dat nebo vrátí jiný typ výsledku akce.


Cílem tohoto kurzu je ukázka, jak můžete psát testování částí pro řadiče v aplikaci ASP.NET MVC aplikace. Probereme, jak vytvořit tři různé typy testů jednotek. Zjistíte, jak testovat zobrazení vrácené akce kontroleru, postup testování zobrazení dat vrácených akce kontroleru a jak otestovat, zda jeden akce kontroleru vás přesměruje na druhou akci kontroleru.

## <a name="creating-the-controller-under-test"></a>Vytvoření řadiče testovaného

Začněme vytvořením kontroler, který jsme chcete otestovat. Řadič, s názvem `ProductController`, je součástí výpis 1.

**Výpis 1 – `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample1.vb)]

`ProductController` Obsahuje dvě metody akce s názvem `Index()` a `Details()`. Obě metody akce vracejí zobrazení. Všimněte si, že `Details()` akce přijme parametr s názvem ID.

## <a name="testing-the-view-returned-by-a-controller"></a>Testování zobrazení vrácený řadič

Představte si, že nám chcete otestovat, jestli `ProductController` vrátí pravého zobrazení. Chceme, abyste měli jistotu, že pokud `ProductController.Details()` akce je volána, je vrácen zobrazení podrobností. Třída testu v výpis 2 obsahuje testů jednotek pro testování zobrazení vrácené `ProductController.Details()` akce.

**Výpis 2 – `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample2.vb)]

Třída v výpis 2 zahrnuje testovací metodu s názvem `TestDetailsView()`. Tato metoda obsahuje tři řádky kódu. První řádek kódu vytvoří novou instanci třídy `ProductController` třídy. Druhý řádek kódu vyvolá kontroleru `Details()` metody akce. Nakonec poslední řádek kódu kontroly, jestli zobrazení vrácené `Details()` zobrazení podrobností není akce.

`ViewResult.ViewName` Vlastnost představuje název zobrazení, které vrácený řadič. Jeden velký upozornění o testování tuto vlastnost. Existují dva způsoby, že řadič můžete vrátit zobrazení. Řadič explicitně vrátit zobrazení takto:

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample3.vb)]

Název zobrazení Alternativně lze odvodit z názvu akce kontroleru takto:

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample4.vb)]

Tato akce kontroleru vrátí také zobrazení s názvem `Details`. Název zobrazení, je však odvodit z název akce. Pokud chcete testovat název zobrazení, pak musí explicitně vrátíte název zobrazení z akce kontroleru.

Testování částí v výpis 2 můžete spustit tak, že zadáte kombinace kláves **Ctrl-R, A** nebo kliknutím **spustit všechny testy v řešení** tlačítko (viz obrázek 1). Pokud testovací úspěšně projde, se zobrazí okno výsledky testu na obrázku 2.


[![Spustit všechny testy v řešení](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)

**Obrázek 01**: spustit všechny testy v řešení ([Kliknutím zobrazit obrázek v plné velikosti](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))


[![Úspěšné!](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)

**Obrázek 02**: Úspěch! ([Kliknutím zobrazit obrázek v plné velikosti](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))


## <a name="testing-the-view-data-returned-by-a-controller"></a>Testování zobrazení dat vrácených řadič

Kontroler MVC předá dat zobrazení pomocí takzvaný *`View Data`*. Představte si například, že chcete zobrazit podrobnosti pro konkrétní produkt při vyvolání `ProductController Details()` akce. V takovém případě můžete vytvořit instanci `Product` – třída (definovanou v modelu) a předejte instanci systému na `Details` zobrazení využitím `View Data`.

Upravenou `ProductController` ve výpisu 3 zahrnuje aktualizované `Details()` akce, která vrací produktu.

**Výpis 3 – `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample5.vb)]

Nejdřív `Details()` akce vytvoří novou instanci třídy `Product` třídu, která představuje přenosný počítač. Další, instanci systému `Product` třída je předán jako druhý parametr `View()` metoda.

Můžete napsat testy jednotek a otestovat, jestli je očekávaná data obsažená v zobrazení data. Jednotkové testování v testech výpis 4 zda produkt představující přenosný počítač je vrácena v případě, že zavoláte `ProductController Details()` metody akce.

**Výpis 4 – `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample6.vb)]

Výpis 4 `TestDetailsView()` metoda otestováním zobrazení dat vrácených vyvoláním `Details()` metoda. `ViewData` Je k dispozici jako vlastnost na `ViewResult` vrácený vyvolání `Details()` metoda. `ViewData.Model` Vlastnost obsahuje produktu předaná do zobrazení. Test jednoduše ověřuje, že produkt obsažené v zobrazení dat má název přenosný počítač.

## <a name="testing-the-action-result-returned-by-a-controller"></a>Testování výsledek akce vrácené řadič

Složitější akce kontroleru může vracet různé typy výsledků akcí v závislosti na hodnoty parametrů předaných pracovnímu akce kontroleru. Akce kontroleru vrátit celou řadu typů výsledky akce včetně `ViewResult`, `RedirectToRouteResult`, nebo `JsonResult`.

Například upravené `Details()` vrátí akce v výpis 5 `Details` zobrazit při předání platný produkt Id akci. Pokud předáte o neplatný produkt Id – Id s hodnotou, menší než 1 – pak budete přesměrováni `Index()` akce.

**Výpis 5 – `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample7.vb)]

Můžete otestovat chování `Details()` akce s testování částí v výpis 6. Testování částí v výpis 6 ověřuje, že budete přesměrováni na `Index` zobrazit, když je předána Id s hodnotou -1 `Details()` metoda.

**Výpis 6 – `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample8.vb)]

Při volání `RedirectToAction()` metoda akce kontroleru akce kontroleru vrátí `RedirectToRouteResult`. Test kontroluje jestli `RedirectToRouteResult` přesměruje uživatele na akce kontroleru s názvem `Index`.

## <a name="summary"></a>Souhrn

V tomto kurzu jste se naučili vytvářet testy částí pro akce kontroleru MVC. Nejdříve jste zjistili, jak ověřit, zda akce kontroleru vrátí pravého zobrazení. Jste se dozvěděli, jak používat `ViewResult.ViewName` vlastnost ověřte název zobrazení.

V dalším kroku jsme se zaměřili na tom, jak můžete otestovat obsah `View Data`. Jste se dozvěděli, jak zkontrolovat, zda je správný produkt vrátil v `View Data` po volání metody akce kontroleru.

Nakonec jsme probrali, jak můžete zkontrolovat, zda jsou různé typy výsledků akce vrácené z akce kontroleru. Jste zjistili, jak k ověření, zda řadič vrátí `ViewResult` nebo `RedirectToRouteResult`.

> [!div class="step-by-step"]
> [Předchozí](creating-unit-tests-for-asp-net-mvc-applications-cs.md)
