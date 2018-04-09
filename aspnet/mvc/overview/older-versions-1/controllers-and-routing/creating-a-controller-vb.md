---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
title: Vytvoření řadiče (VB) | Microsoft Docs
author: StephenWalther
description: V tomto kurzu Stephen Walther ukazuje, jak přidat řadič pro aplikaci ASP.NET MVC.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 204b7e86-f560-4611-8adb-785b33e777b9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
msc.type: authoredcontent
ms.openlocfilehash: e9a2bbcb09672f5247429064908cd4d2ef67f518
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-controller-vb"></a>Vytvoření řadiče (VB)
====================
podle [Stephen Walther](https://github.com/StephenWalther)

> V tomto kurzu Stephen Walther ukazuje, jak přidat řadič pro aplikaci ASP.NET MVC.


Cílem tohoto kurzu je vysvětlují, jak vytvořit nové rozhraní ASP.NET MVC řadiče. Zjistíte, jak vytvořit řadiče pomocí příkazu přidat kontroler Visual Studio i tak, že ručně vytvoříte soubor třídy.

### <a name="using-the-add-controller-menu-option"></a>Pomocí přidejte řadiče nabídky

Nejjednodušší způsob, jak vytvořit nový řadič je složce řadiče v okně Průzkumníka řešení Visual Studio klikněte pravým tlačítkem a vyberte **přidat, řadiče** nabídky (viz obrázek 1). Výběrem této možnosti nabídky otevře **přidat kontroler** dialogové okno (viz obrázek 2).


[![Dialogové okno Nový projekt](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)

**Obrázek 01**: přidávání nového řadiče ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-controller-vb/_static/image2.png))


[![Dialogové okno Nový projekt](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)

**Obrázek 02**: dialogové okno Přidat kontroler ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-controller-vb/_static/image4.png))


Všimněte si, že první část názvu kontroleru je označený na **přidat kontroler** dialogové okno. Každý název kontroleru musí končit příponou *řadič*. Například můžete vytvořit řadič s názvem *ProductController* , ale není řadič s názvem *produktu*.


Pokud vytvoříte kontroler, který nebyl nalezen *řadič* přípona pak nebude možné k vyvolání kontroleru. Není to – I jste ke znehodnocení části obrovském množství hodin život po provedení této chybě.


**Výpis 1 - Controllers\ProductController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample1.vb)]

Vždy byste měli vytvořit řadiče ve složce řadiče. Jinak budete narušení konvence rozhraní ASP.NET MVC a jinými vývojáři bude mít obtížnější čas pochopení vaší aplikace.

### <a name="scaffolding-action-methods"></a>Metody akce generování uživatelského rozhraní

Když vytvoříte řadič, máte možnost automaticky vygenerovat vytvoření, aktualizace a informace metody akce (viz obrázek 3). Pokud vyberete tuto možnost se vygeneruje třídy kontroleru v výpis 2.


[![Automatické vytváření metody akce](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)

**Obrázek 03**: vytvoření metody akce automaticky ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-controller-vb/_static/image6.png))


**Výpis 2 - Controllers\CustomerController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample2.vb)]

Tyto metody generované jsou se zakázaným inzerováním metody. Je nutné přidat skutečné logiku pro vytváření, aktualizaci a zobrazení podrobností pro zákazníka sami. Ale se zakázaným inzerováním metody poskytují dobrý výchozí bod.

### <a name="creating-a-controller-class"></a>Vytvoření třídy Kontroleru

Kontroler ASP.NET MVC je právě třída. Pokud dáváte přednost, můžete ignorovat generování uživatelského rozhraní pohodlný Visual Studio řadiče a ručně vytvořit třídu kontroleru. Postupujte podle těchto kroků:

1. Klikněte pravým tlačítkem na složku řadiče a vyberte možnost nabídky **přidat, nové položky** a vyberte **třída** šablony (viz obrázek 4).
2. Pojmenujte novou třídu PersonController.vb a klikněte na **přidat** tlačítko.
3. Výsledný soubor třída upravte tak, aby třídy dědí vlastnosti ze základní třídy System.Web.Mvc.Controller (viz seznam 3).


[![Vytvoření nové třídy](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)

**Obrázek 04**: vytvoření nové třídy ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-controller-vb/_static/image8.png))


**Výpis 3 - Controllers\PersonController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample3.vb)]

Řadič v výpis 3 zpřístupní jednu akci s názvem Index(), která vrací řetězec "Hello, World!". Tato akce kontroleru můžete vyvolat spuštěním aplikace a vyžaduje adresu URL podobnou následující:

`http://localhost:40071/Person`

> [!NOTE]
> 
> Vývojový Server ASP.NET používá číslo portu náhodné (například 40071). Při zadávání adresy URL pro vyvolání řadič, budete muset zadat číslo pravý port. Ukázáním myší na ikonu pro vývojový Server ASP.NET v oznamovací oblasti systému Windows (pravém dolním rohu obrazovky) můžete zjistit číslo portu.
> 
> [!div class="step-by-step"]
> [Předchozí](adding-dynamic-content-to-a-cached-page-vb.md)
> [další](creating-an-action-vb.md)
