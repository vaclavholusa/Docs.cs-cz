---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
title: Vytvoření Kontroleru (C#) | Dokumentace Microsoftu
author: StephenWalther
description: V tomto kurzu Stephen Walther ukazuje, jak přidat řadič do aplikace ASP.NET MVC.
ms.author: aspnetcontent
ms.date: 03/02/2009
ms.assetid: 719d50d4-2305-454c-98b4-bae64937c48f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
msc.type: authoredcontent
ms.openlocfilehash: 3b3b663f76b6271a213e5980756ba63372ef6fe9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804262"
---
<a name="creating-a-controller-c"></a>Vytvoření Kontroleru (C#)
====================
podle [Stephen Walther](https://github.com/StephenWalther)

> V tomto kurzu Stephen Walther ukazuje, jak přidat řadič do aplikace ASP.NET MVC.


Cílem tohoto kurzu je vysvětlují, jak vytvořit nové technologie ASP.NET MVC řadiče. Zjistíte, jak vytvořit kontrolery, pomocí nabídky přidat kontroler Visual Studio a tím, že ručně vytvoříte soubor třídy.

### <a name="using-the-add-controller-menu-option"></a>Použití přidání Kontroleru nabídky

Klikněte pravým tlačítkem na složku řadiče v okně Průzkumník řešení Visual Studio a vybrat je nejjednodušší způsob, jak vytvořit nový řadič **přidat, řadič** nabídky (viz obrázek 1). Výběrem této možnosti se otevře **přidat kontroler** dialogového okna (viz obrázek 2).


[![Dialogové okno Nový projekt](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)

**Obrázek 01**: Přidání nového řadiče ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-controller-cs/_static/image2.png))


[![Dialogové okno Nový projekt](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)

**Obrázek 02**: dialogové okno pro přidání Kontroleru ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-controller-cs/_static/image4.png))


Všimněte si, že první část názvu kontroleru je zvýrazněn **přidat kontroler** dialogového okna. Každý název kontroleru musí končit příponou *řadič*. Můžete například vytvořit řadič s názvem *ProductController* , ale ne řadič s názvem *produktu*.


Pokud vytvoříte kontroler, který nebyl nalezen *řadič* příponu pak nebudete mít k vyvolání kontroleru. Neumožňuje tuto--mohu jste plýtvat nespočet hodin život označíte tuto chybu.


**Výpis 1 - Controllers\ProductController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample1.cs)]

Vždy byste měli vytvořit řadiče ve složce řadiče. V opačném případě budete porušení konvence ASP.NET MVC a jinými vývojáři budou mít obtížnější čas pochopení vaší aplikace.

### <a name="scaffolding-action-methods"></a>Metody akce generování uživatelského rozhraní

Když vytvoříte řadič, máte možnost automaticky vygenerovat metody akce vytvoření, aktualizace a podrobnosti (viz obrázek 3). Pokud vyberete tuto možnost je vygenerována třída kontroleru v zobrazení 2.


[![Automatické vytváření metody akce](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)

**Obrázek 03**: vytvoření metody akce automaticky ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-controller-cs/_static/image6.png))


**Výpis 2 - Controllers\CustomerController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample2.cs)]

Tyto vygenerované metody jsou metody zástupných procedur. Je nutné přidat skutečné logiku pro vytváření, aktualizaci a zobrazení podrobností pro zákazníka sami. Ale se zakázaným inzerováním metody poskytují dobrý výchozí bod.

### <a name="creating-a-controller-class"></a>Vytvoření třídy Kontroleru

Kontroler ASP.NET MVC je jenom třídy. Pokud dáváte přednost, můžete ignorovat generování uživatelského rozhraní vhodné sady Visual Studio kontroleru a ručně vytvořit třídu kontroleru. Postupujte podle těchto kroků:

1. Klikněte pravým tlačítkem na složku řadiče a vyberte možnost nabídky **přidat, nová položka** a vyberte **třídy** šablony (viz obrázek 4).
2. Pojmenujte novou třídu PersonController.cs a klikněte na tlačítko **přidat** tlačítko.
3. Upravte soubor výsledné třídy tak, aby třída dědí ze základní třídy System.Web.Mvc.Controller (viz seznam 3).


[![Vytvoření nové třídy](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)

**Obrázek 04**: vytvoření nové třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-controller-cs/_static/image8.png))


**Výpis 3 - Controllers\PersonController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample3.cs)]

Kontroler v informacích 3 poskytuje jednu akci s názvem Index(), který vrátí řetězec "Hello World!". Tato akce kontroleru můžete vyvolat spuštěním vaší aplikace a požaduje adresu URL takto:

`http://localhost:40071/Person`

> [!NOTE]
> 
> Vývojový Server ASP.NET používá náhodný port číslo (například 40071). Při zadávání adresy URL pro vyvolání kontroleru, budete muset zadat číslo správný port. Je-li určit číslo portu, najedete myší na ikonu pro vývojový Server ASP.NET v oznamovací oblasti Windows (vpravo dole na obrazovce).
> 
> [!div class="step-by-step"]
> [Předchozí](adding-dynamic-content-to-a-cached-page-cs.md)
> [další](creating-an-action-cs.md)
