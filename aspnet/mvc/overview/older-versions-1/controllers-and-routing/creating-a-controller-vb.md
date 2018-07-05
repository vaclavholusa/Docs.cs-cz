---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
title: Vytvoření Kontroleru (VB) | Dokumentace Microsoftu
author: StephenWalther
description: V tomto kurzu Stephen Walther ukazuje, jak přidat řadič do aplikace ASP.NET MVC.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 204b7e86-f560-4611-8adb-785b33e777b9
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
msc.type: authoredcontent
ms.openlocfilehash: 619fff7a78526cae6c3db0f3c414215bf7db9bb5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393886"
---
<a name="creating-a-controller-vb"></a>Vytvoření Kontroleru (VB)
====================
podle [Stephen Walther](https://github.com/StephenWalther)

> V tomto kurzu Stephen Walther ukazuje, jak přidat řadič do aplikace ASP.NET MVC.


Cílem tohoto kurzu je vysvětlují, jak vytvořit nové technologie ASP.NET MVC řadiče. Zjistíte, jak vytvořit kontrolery, pomocí nabídky přidat kontroler Visual Studio a tím, že ručně vytvoříte soubor třídy.

### <a name="using-the-add-controller-menu-option"></a>Použití přidání Kontroleru nabídky

Klikněte pravým tlačítkem na složku řadiče v okně Průzkumník řešení Visual Studio a vybrat je nejjednodušší způsob, jak vytvořit nový řadič **přidat, řadič** nabídky (viz obrázek 1). Výběrem této možnosti se otevře **přidat kontroler** dialogového okna (viz obrázek 2).


[![Dialogové okno Nový projekt](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)

**Obrázek 01**: Přidání nového řadiče ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-controller-vb/_static/image2.png))


[![Dialogové okno Nový projekt](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)

**Obrázek 02**: dialogové okno pro přidání Kontroleru ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-controller-vb/_static/image4.png))


Všimněte si, že první část názvu kontroleru je zvýrazněn **přidat kontroler** dialogového okna. Každý název kontroleru musí končit příponou *řadič*. Můžete například vytvořit řadič s názvem *ProductController* , ale ne řadič s názvem *produktu*.


Pokud vytvoříte kontroler, který nebyl nalezen *řadič* příponu pak nebudete mít k vyvolání kontroleru. Neumožňuje tuto--mohu jste plýtvat nespočet hodin život označíte tuto chybu.


**Výpis 1 - Controllers\ProductController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample1.vb)]

Vždy byste měli vytvořit řadiče ve složce řadiče. V opačném případě budete porušení konvence ASP.NET MVC a jinými vývojáři budou mít obtížnější čas pochopení vaší aplikace.

### <a name="scaffolding-action-methods"></a>Metody akce generování uživatelského rozhraní

Když vytvoříte řadič, máte možnost automaticky vygenerovat metody akce vytvoření, aktualizace a podrobnosti (viz obrázek 3). Pokud vyberete tuto možnost je vygenerována třída kontroleru v zobrazení 2.


[![Automatické vytváření metody akce](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)

**Obrázek 03**: vytvoření metody akce automaticky ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-controller-vb/_static/image6.png))


**Výpis 2 - Controllers\CustomerController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample2.vb)]

Tyto vygenerované metody jsou metody zástupných procedur. Je nutné přidat skutečné logiku pro vytváření, aktualizaci a zobrazení podrobností pro zákazníka sami. Ale se zakázaným inzerováním metody poskytují dobrý výchozí bod.

### <a name="creating-a-controller-class"></a>Vytvoření třídy Kontroleru

Kontroler ASP.NET MVC je jenom třídy. Pokud dáváte přednost, můžete ignorovat generování uživatelského rozhraní vhodné sady Visual Studio kontroleru a ručně vytvořit třídu kontroleru. Postupujte podle těchto kroků:

1. Klikněte pravým tlačítkem na složku řadiče a vyberte možnost nabídky **přidat, nová položka** a vyberte **třídy** šablony (viz obrázek 4).
2. Pojmenujte novou třídu PersonController.vb a klikněte na tlačítko **přidat** tlačítko.
3. Upravte soubor výsledné třídy tak, aby třída dědí ze základní třídy System.Web.Mvc.Controller (viz seznam 3).


[![Vytvoření nové třídy](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)

**Obrázek 04**: vytvoření nové třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-controller-vb/_static/image8.png))


**Výpis 3 - Controllers\PersonController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample3.vb)]

Kontroler v informacích 3 poskytuje jednu akci s názvem Index(), který vrátí řetězec "Hello World!". Tato akce kontroleru můžete vyvolat spuštěním vaší aplikace a požaduje adresu URL takto:

`http://localhost:40071/Person`

> [!NOTE]
> 
> Vývojový Server ASP.NET používá náhodný port číslo (například 40071). Při zadávání adresy URL pro vyvolání kontroleru, budete muset zadat číslo správný port. Je-li určit číslo portu, najedete myší na ikonu pro vývojový Server ASP.NET v oznamovací oblasti Windows (vpravo dole na obrazovce).
> 
> [!div class="step-by-step"]
> [Předchozí](adding-dynamic-content-to-a-cached-page-vb.md)
> [další](creating-an-action-vb.md)
