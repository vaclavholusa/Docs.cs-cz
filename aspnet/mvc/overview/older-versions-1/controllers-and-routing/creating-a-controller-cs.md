---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
title: "Vytvoření řadiče (C#) | Microsoft Docs"
author: StephenWalther
description: "V tomto kurzu Stephen Walther ukazuje, jak přidat řadič pro aplikaci ASP.NET MVC."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 719d50d4-2305-454c-98b4-bae64937c48f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
msc.type: authoredcontent
ms.openlocfilehash: 9faaff1e00998ef9a77c4928a9eb36fc93ab97f4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-controller-c"></a>Vytvoření řadiče (C#)
====================
podle [Stephen Walther](https://github.com/StephenWalther)

> V tomto kurzu Stephen Walther ukazuje, jak přidat řadič pro aplikaci ASP.NET MVC.


Cílem tohoto kurzu je vysvětlují, jak vytvořit nové rozhraní ASP.NET MVC řadiče. Zjistíte, jak vytvořit řadiče pomocí příkazu přidat kontroler Visual Studio i tak, že ručně vytvoříte soubor třídy.

### <a name="using-the-add-controller-menu-option"></a>Pomocí přidejte řadiče nabídky

Nejjednodušší způsob, jak vytvořit nový řadič je složce řadiče v okně Průzkumníka řešení Visual Studio klikněte pravým tlačítkem a vyberte **přidat, řadiče** nabídky (viz obrázek 1). Výběrem této možnosti nabídky otevře **přidat kontroler** dialogové okno (viz obrázek 2).


[![Dialogové okno Nový projekt](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)

**Obrázek 01**: přidávání nového řadiče ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-controller-cs/_static/image2.png))


[![Dialogové okno Nový projekt](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)

**Obrázek 02**: dialogové okno Přidat kontroler ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-controller-cs/_static/image4.png))


Všimněte si, že první část názvu kontroleru je označený na **přidat kontroler** dialogové okno. Každý název kontroleru musí končit příponou *řadič*. Například můžete vytvořit řadič s názvem *ProductController* , ale není řadič s názvem *produktu*.


Pokud vytvoříte kontroler, který nebyl nalezen *řadič* přípona pak nebude možné k vyvolání kontroleru. Není to – I jste ke znehodnocení části obrovském množství hodin život po provedení této chybě.


**Výpis 1 - Controllers\ProductController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample1.cs)]

Vždy byste měli vytvořit řadiče ve složce řadiče. Jinak budete narušení konvence rozhraní ASP.NET MVC a jinými vývojáři bude mít obtížnější čas pochopení vaší aplikace.

### <a name="scaffolding-action-methods"></a>Metody akce generování uživatelského rozhraní

Když vytvoříte řadič, máte možnost automaticky vygenerovat vytvoření, aktualizace a informace metody akce (viz obrázek 3). Pokud vyberete tuto možnost se vygeneruje třídy kontroleru v výpis 2.


[![Automatické vytváření metody akce](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)

**Obrázek 03**: vytvoření metody akce automaticky ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-controller-cs/_static/image6.png))


**Výpis 2 - Controllers\CustomerController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample2.cs)]

Tyto metody generované jsou se zakázaným inzerováním metody. Je nutné přidat skutečné logiku pro vytváření, aktualizaci a zobrazení podrobností pro zákazníka sami. Ale se zakázaným inzerováním metody poskytují dobrý výchozí bod.

### <a name="creating-a-controller-class"></a>Vytvoření třídy Kontroleru

Kontroler ASP.NET MVC je právě třída. Pokud dáváte přednost, můžete ignorovat generování uživatelského rozhraní pohodlný Visual Studio řadiče a ručně vytvořit třídu kontroleru. Postupujte podle těchto kroků:

1. Klikněte pravým tlačítkem na složku řadiče a vyberte možnost nabídky **přidat, nové položky** a vyberte **třída** šablony (viz obrázek 4).
2. Pojmenujte novou třídu PersonController.cs a klikněte na **přidat** tlačítko.
3. Výsledný soubor třída upravte tak, aby třídy dědí vlastnosti ze základní třídy System.Web.Mvc.Controller (viz seznam 3).


[![Vytvoření nové třídy](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)

**Obrázek 04**: vytvoření nové třídy ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-controller-cs/_static/image8.png))


**Výpis 3 - Controllers\PersonController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample3.cs)]

Řadič v výpis 3 zpřístupní jednu akci s názvem Index(), která vrací řetězec "Hello, World!". Tato akce kontroleru můžete vyvolat spuštěním aplikace a vyžaduje adresu URL podobnou následující:

`http://localhost:40071/Person`

> [!NOTE] 
> 
> Vývojový Server ASP.NET používá číslo portu náhodné (například 40071). Při zadávání adresy URL pro vyvolání řadič, budete muset zadat číslo pravý port. Ukázáním myší na ikonu pro vývojový Server ASP.NET v oznamovací oblasti systému Windows (pravém dolním rohu obrazovky) můžete zjistit číslo portu.

>[!div class="step-by-step"]
[Předchozí](adding-dynamic-content-to-a-cached-page-cs.md)
[další](creating-an-action-cs.md)
