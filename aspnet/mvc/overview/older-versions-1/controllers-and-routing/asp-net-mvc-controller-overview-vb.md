---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
title: Přehled řadiče ASP.NET MVC (VB) | Microsoft Docs
author: StephenWalther
description: V tomto kurzu Stephen Walther vás seznámí s ASP.NET MVC řadiče. Zjistíte, jak vytvořit nové řadiče a vrátíte se různé typy res akce...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: 94c3e5d9-a904-445e-a34e-d92fd1ca108a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: 0a45630e8f2d5ae0548bb6b8496df08ca5877a40
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868981"
---
<a name="aspnet-mvc-controller-overview-vb"></a>Přehled řadiče ASP.NET MVC (VB)
====================
podle [Stephen Walther](https://github.com/StephenWalther)

> V tomto kurzu Stephen Walther vás seznámí s ASP.NET MVC řadiče. Zjistíte, jak vytvořit nové řadiče a vrátíte se různé typy výsledky akce.


V tomto kurzu prozkoumá téma řadiče ASP.NET MVC, akce kontroleru a výsledky akce. Po dokončení tohoto kurzu bude pochopit, jak se řadiče používají k řízení způsobu, jakým návštěvníka komunikuje s webem ASP.NET MVC.

## <a name="understanding-controllers"></a>Vysvětlení Kontrolerů

Řadiče MVC je zodpovědná za neodpovídá na požadavky vytvořené pro stránku ASP.NET MVC. Každý požadavek prohlížeče je namapována na určitý kontroler. Představte si například, zadejte následující adresu URL do adresního řádku prohlížeče:

`http://localhost/Product/Index/3`

V takovém případě je volána řadič ProductController. ProductController je zodpovědný za generování odpovědi na požadavek prohlížeče. Například kontroler může vrátit konkrétní zobrazení zpět do prohlížeče nebo řadičem může přesměruje uživatele na jiný řadič.

Výpis 1 obsahuje řadič jednoduché s názvem ProductController.

**Listing1 - Controllers\ProductController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample1.vb)]

Jak je vidět na výpisu 1, řadič je právě třídy (třídy jazyka Visual Basic .NET nebo C#). Řadič je třída, která je odvozena ze základní třídy System.Web.Mvc.Controller. Protože řadič dědí z této základní třídy, řadič dědí několik užitečných metod zdarma (probereme tyto metody za chvíli).

## <a name="understanding-controller-actions"></a>Pochopení akce Kontroleru

Řadič zpřístupní akce kontroleru. Akce je metoda na řadič, která je volána, když zadáte konkrétní adresu URL do adresního řádku prohlížeče. Představte si například, že zadáte požadavek na následující adresu URL:

`http://localhost/Product/Index/3`

V takovém případě je na třídě ProductController volána metoda Index(). Metoda Index() je příklad akce kontroleru.

Akce kontroleru musí být veřejné metody třídy kontroleru. Visual Basic.NET metody, ve výchozím nastavení, jsou veřejné metody. Uvědomte si, že všechny veřejná metoda, která přidáte do třídy kontroleru je k dispozici jako akce kontroleru automaticky (je třeba dbát o to vzhledem k tomu, že akce kontroleru můžete vyvolat každý, kdo universe jednoduše tak, že adresa URL pravého psaní do adresního řádku prohlížeče).

Existují některé další požadavky, které musí splnit akce kontroleru. Nemohou být přetíženy metodu použít jako akce kontroleru. Kromě toho akce kontroleru nemůže být statickou metodu. Než tu, která můžete použít jenom o libovolnou metodu jako akce kontroleru.

## <a name="understanding-action-results"></a>Seznámení s výsledky akce

Akce kontroleru vrátí takzvaný *výsledek akce*. Výsledek akce je akce kontroleru vrátí v reakci na žádost prohlížeče.

Rozhraní ASP.NET MVC podporuje několik typů výsledky akcí, včetně:

1. ViewResult - představuje HTML a značek.
2. EmptyResult - představuje žádný výsledek.
3. RedirectResult - představuje požadavek na přesměrování na novou adresu URL.
4. JsonResult - představuje JavaScript Object Notation výsledek, který lze použít v aplikaci AJAX.
5. JavaScriptResult - představuje skript jazyka JavaScript.
6. ContentResult - představuje výsledek text.
7. FileContentResult - představuje soubor ke stažení (s binární obsah).
8. FilePathResult - představuje soubor ke stažení (s cestou).
9. FileStreamResult - představuje soubor ke stažení (s datový proud souboru).

Všechny tyto výsledky akce dědí ze základní třídy ActionResult.

Ve většině případů se akce kontroleru vrátí ViewResult. Například akce kontroleru indexu v výpis 2 vrátí ViewResult.

**Výpis 2 - Controllers\BookController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample2.vb)]

Když akce vrátí ViewResult, HTML se vrátí do prohlížeče. Vrátí metoda Index() v výpis 2 zobrazení s názvem Index do prohlížeče.

Všimněte si, že akce Index() ve výpisu 2 nevrací ViewResult(). Místo toho je volána metoda View() základní třídy Controller. Za normálních okolností je nevrátí výsledek akce přímo. Místo toho volání jednoho z následujících metod základní třídy Controller:

1. Zobrazení – vrátí výsledek akce ViewResult.
2. Přesměrování – vrátí výsledek akce RedirectResult.
3. RedirectToAction - vrací výsledek akce RedirectToRouteResult.
4. RedirectToRoute - vrací výsledek akce RedirectToRouteResult.
5. JSON – vrátí výsledek akce JsonResult.
6. JavaScriptResult – vrátí JavaScriptResult.
7. Obsah – vrátí výsledek akce ContentResult.
8. Soubor – vrátí FileContentResult, FilePathResult nebo FileStreamResult v závislosti na parametrech předaný metodě.

Ano Pokud chcete vrátit zobrazení do prohlížeče, volejte metodu View(). Pokud chcete přesměrovat uživatele z jednoho řadiče akce do druhého, lze volat metodu RedirectToAction(). Například akce Details() ve výpisu 3 buď zobrazení nebo přesměruje uživatele na Index() akci v závislosti na tom, jestli parametr Id má hodnotu.

**Výpis 3 - CustomerController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample3.vb)]

Výsledek akce ContentResult je speciální. Výsledek akce ContentResult můžete použít k návratu výsledku akce jako prostý text. Například metoda Index() výpis 4 vrátí zprávu jako prostý text a ne jako HTML.

**Výpis 4 - Controllers\StatusController.vb**

> StatusController
> 
> 
> System.Web.Mvc.Controller


[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample4.vb)]

Po vyvolání akce StatusController.Index() zobrazení nevrátí. Místo toho nezpracovaný text "Hello, World!" se vrátí do prohlížeče.

Pokud akce kontroleru vrátí výsledek, který není výsledek akce - je například datum nebo celé číslo – pak výsledek je uzavřen do ContentResult automaticky. Například po vyvolání akce Index() WorkController v výpis 5 datum se vrátí jako ContentResult automaticky.

**Výpis 5 - WorkController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample5.vb)]

Akce Index() ve výpisu 5 vrátí objekt data a času. Rozhraní ASP.NET MVC převádí data a času objekt na řetězec a zabalí hodnoty DateTime ve ContentResult automaticky. Prohlížeč obdrží datum a čas jako prostý text.

## <a name="summary"></a>Souhrn

Účelem tohoto kurzu bylo seznámí s koncepty řadičů ASP.NET MVC, akce kontroleru a výsledky akce kontroleru. V první části jste zjistili, jak k přidávání nových řadičů do projektu aplikace ASP.NET MVC. V dalším kroku jste se dozvěděli, jak veřejné metody řadiče jsou zveřejněné na základní soubor jako akce kontroleru. Nakonec jsme probrali různé typy výsledků akcí, které mohou být vráceny z akce kontroleru. Konkrétně jsme se bavili jak vracet ViewResult, RedirectToActionResult a ContentResult z akce kontroleru.

> [!div class="step-by-step"]
> [Předchozí](creating-a-custom-route-constraint-cs.md)
> [další](creating-custom-routes-vb.md)
