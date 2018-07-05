---
uid: mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
title: ASP.NET MVC – přehled Kontrolerů (C#) | Dokumentace Microsoftu
author: StephenWalther
description: V tomto kurzu Stephen Walther vás seznámí s kontrolery ASP.NET MVC. Zjistíte, jak vytvořit nové řadiče a vracet různé druhy res akce...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: b985c49a-3668-455c-a366-f85f6bc64b12
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: e1834498d5a0f5d84f650faf32bb6352f2ad3e70
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370084"
---
<a name="aspnet-mvc-controller-overview-c"></a>ASP.NET MVC – přehled Kontrolerů (C#)
====================
podle [Stephen Walther](https://github.com/StephenWalther)

> V tomto kurzu Stephen Walther vás seznámí s kontrolery ASP.NET MVC. Zjistíte, jak vytvořit nové řadiče a vracet různé typy výsledků akcí.


Tento kurz se věnuje téma kontrolery ASP.NET MVC, akce kontroleru a výsledky akce. Po dokončení tohoto kurzu budete rozumět, jak se řadiče používají k řízení způsobu, jakým návštěvník komunikuje se službou Web ASP.NET MVC.

## <a name="understanding-controllers"></a>Vysvětlení Kontrolerů

Kontrolery MVC je zodpovědná za odpověď na požadavky na web ASP.NET MVC. Každý požadavek prohlížeče je namapována na určitý kontroler. Představte si například, zadejte následující adresu URL do adresního řádku svého prohlížeče:

`http://localhost/Product/Index/3`

V takovém případě je vyvolána s názvem ProductController kontroleru. Je zodpovědná za generování odpověď na žádost prohlížeče ProductController. Například kontroler může vrátit konkrétní zobrazení zpět do prohlížeče nebo kontroler může přesměruje uživatele na jiný kontroler.

Výpis 1 obsahuje jednoduché ProductController řadič.

**Listing1 - Controllers\ProductController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample1.cs)]

Jak je vidět z výpisu 1 kontroleru je právě třídy (třídy jazyka Visual Basic .NET nebo C#). Kontroler je třída, která je odvozena ze základní třídy System.Web.Mvc.Controller. Protože kontroler dědí z této základní třídy, řadič zdarma zdědí několik užitečných metod (můžeme probírat tyto metody za chvíli).

## <a name="understanding-controller-actions"></a>Principy akce Kontroleru

Kontroler zpřístupní akce kontroleru. Akce je metoda na řadiči, která je volána při zadání určité adresy URL v adresním řádku prohlížeče. Představte si například, že zadáte požadavek na následující adresu URL:

`http://localhost/Product/Index/3`

V takovém případě je volána metoda Index() ProductController třídy. Metoda Index() je příklad akce kontroleru.

Akce kontroleru musí být veřejné metody třídy kontroleru. C# metody, ve výchozím nastavení, jsou privátní metody. Uvědomte si, že všechny veřejné metody, který přidáte do třídu kontroleru je automaticky vystavena jako akce kontroleru (musíte být opatrní při to od akce kontroleru můžete vyvolat kdokoli v universe jednoduše tak, že zadáte správné adresy URL do adresního řádku prohlížeče).

Existují některé další požadavky, které musí být splněno akce kontroleru. Metoda používá jako akce kontroleru nemohou být přetíženy. Kromě toho akce kontroleru nemůže být statickou metodu. Kromě toho můžete použít prakticky jakoukoli metodu jako akce kontroleru.

## <a name="understanding-action-results"></a>Principy výsledky akcí

Akce kontroleru vrátí hodnotu s názvem *výsledek akce*. Výsledek akce se akce kontroleru vrátí v reakci na žádost prohlížeče.

Architektura ASP.NET MVC podporuje několik typů výsledky akcí, včetně:

1. ViewResult – představuje HTML a kódu.
2. EmptyResult – představuje žádný výsledek.
3. RedirectResult – představuje přesměrování na novou adresu URL.
4. JsonResult – představuje JavaScript Object Notation výsledek, který lze použít v aplikaci AJAX.
5. JavaScriptResult – představuje skriptu JavaScript.
6. ContentResult – představuje výsledek text.
7. FileContentResult – představuje soubor ke stažení (s binární obsah).
8. FilePathResult – představuje soubor ke stažení (s cestou).
9. FileStreamResult – představuje soubor ke stažení (s datový proud souboru).

Všechny tyto akce výsledky dědí ze základní třídy ActionResult.

Ve většině případů se akce kontroleru vrátí ViewResult. Například vrátí Index akce kontroleru v zobrazení 2 ViewResult.

**Výpis 2 - Controllers\BookController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample2.cs)]

Po návratu akce ViewResult, HTML se vrátí do prohlížeče. Metoda Index() ve výpisu 2 vrátí zobrazení s názvem Index do prohlížeče.

Všimněte si, že akce Index() ve výpisu 2 nevrací ViewResult(). Místo toho je volána metoda View() základní třídy Kontroleru. Za normálních okolností je nevrátí výsledek akce přímo. Místo toho můžete volat jednu z následujících metod základní třídy Kontroleru:

1. Zobrazit – vrátí výsledek akce ViewResult.
2. Přesměrování - vrací výsledek akce RedirectResult.
3. RedirectToAction – vrátí výsledek akce RedirectToRouteResult.
4. Metodu RedirectToRoute – vrátí výsledek akce RedirectToRouteResult.
5. JSON – vrátí výsledek akce JsonResult.
6. JavaScriptResult – vrátí JavaScriptResult.
7. Obsah – vrátí výsledek akce ContentResult.
8. Soubor – vrátí FileContentResult, FilePathResult nebo FileStreamResult v závislosti na parametrech předaný metodě.

Ano Pokud chcete vrátit zobrazení v prohlížeči, zavolejte metodu View(). Pokud chcete přesměrovat uživatele z jednoho řadiče akce do druhého, zavolejte metodu RedirectToAction(). Například akce Details() ve výpisu 3 zobrazuje zobrazení nebo přesměruje uživatele na Index() akci v závislosti na tom, zda má parametr Id hodnotu.

**Výpis 3 - CustomerController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample3.cs)]

Výsledek akce ContentResult je speciální. Výsledek akce ContentResult můžete použít k vrácení výsledku akce jako prostý text. Metoda Index() ve výpisu 4 například vrátí zprávu jako prostý text a ne jako HTML.

**Část 4 – Controllers\StatusController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample4.cs)]

Při vyvolání akce StatusController.Index() zobrazení nevrátí. Místo toho nezpracovaný text "Hello World!" se vrátí do prohlížeče.

Pokud akce kontroleru vrátí výsledek, který není výsledek akce – například datum nebo celé číslo – potom výsledek je zabalen ContentResult automaticky. Například při vyvolání akce Index() WorkController výpis 5 datum se vrátí jako ContentResult automaticky.

**Výpis 5 - WorkController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample5.cs)]

Akce Index() ve výpisu 5 vrátí objekt DateTime. Architektura ASP.NET MVC převede objekt DateTime na řetězec a automaticky zabalí ContentResult hodnotu data a času. Prohlížeč obdrží data a času jako prostý text.

## <a name="summary"></a>Souhrn

Účelem tohoto kurzu bylo seznámí s koncepty kontrolery ASP.NET MVC, akce kontroleru a výsledky akce kontroleru. V prvním oddílu jste zjistili, jak přidat nové řadiče do projektu aplikace ASP.NET MVC. Dále jste zjistili, jak veřejné metody řadiče jsou vystaveny rozhraní universe jako akce kontroleru. Nakonec jsme probírali různé druhy výsledky akcí, které mohou být vráceny z akce kontroleru. Konkrétně jsme probírali jak vrátit ViewResult, RedirectToActionResult a ContentResult z akce kontroleru.

> [!div class="step-by-step"]
> [Předchozí](creating-an-action-vb.md)
> [další](creating-custom-routes-cs.md)
