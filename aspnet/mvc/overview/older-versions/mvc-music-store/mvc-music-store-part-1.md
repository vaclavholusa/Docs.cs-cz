---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Část 1: Přehled a soubor -> Nový projekt | Dokumentace Microsoftu'
author: jongalloway
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store. Část 1 zahrnuje přehled a soubor -> Nový projekt.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 57856a4a78a650e4abe872004e5be5f8f3b2dbcd
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816336"
---
<a name="part-1-overview-and-file-new-project"></a>Část 1: Přehled a soubor -> Nový projekt
====================
podle [Jon Galloway](https://github.com/jongalloway)

> MVC Music Store jde o kurz, který se seznámíte, podrobné postupy pro vývoj pro web pomocí ASP.NET MVC a sady Visual Studio.  
>   
> Music Store MVC je jednoduché ukázku implementace úložiště prodává hudebních alb online, který implementuje správu základního webu, přihlášení uživatele a nákupního košíku funkce.  
>   
> V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store. 1. část obsahuje přehled a soubor -&gt;nový projekt.


## <a name="overview"></a>Přehled

MVC Music Store jde o kurz, který se seznámíte, podrobné postupy pro vývoj pro web pomocí ASP.NET MVC a aplikace Visual Web Developer. Jsme budete spuštění pomalu, takže Začátečník úrovně webové vývojové prostředí je v pořádku.

Aplikace, kterou jsme vám sestavení je jednoduché music store. Existují tři hlavní části aplikace: nákupní, registrace a správa.

![](mvc-music-store-part-1/_static/image1.jpg)

Návštěvníci můžete procházet alb podle žánru:

![](mvc-music-store-part-1/_static/image2.jpg)

Mohou zobrazit jednu alba a přidat do košíku jejich:

![](mvc-music-store-part-1/_static/image3.jpg)

Jejich košíku odebrat všechny položky, které už nechcete, můžete zkontrolovat:

![](mvc-music-store-part-1/_static/image4.jpg)

Přistoupíte k ověření je vyzve k přihlásit nebo zaregistrovat u určitého uživatelského účtu.

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

Po vytvoření účtu, můžete provést pořadí vyplněním dopravě a platební informace. Abychom si to nekomplikovali, máme spuštěnou úžasné propagační akce: vše, co je zdarma po zadání propagační kód, který "FREE"!

![](mvc-music-store-part-1/_static/image5.jpg)

Po změně pořadí, se zobrazí obrazovka s potvrzením jednoduchý:

![](mvc-music-store-part-1/_static/image6.jpg)

Kromě stránek faceing zákazníka budete také vytváříme správce oddílu, který se zobrazí seznam objektů alb, ze kterých můžou správci vytvářet, upravovat a odstranit alb:

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a>1. Soubor –&gt; nový projekt

### <a name="installing-the-software"></a>Instalace softwaru

V tomto kurzu se začne tím, že vytvoříte nový projekt ASP.NET MVC 3 s použitím na bezplatné Visual Web Developer 2010 Express (které je zdarma) a pak postupně přidáme funkce, které chcete vytvořit kompletní funkční aplikaci. Cestou se budeme zabývat přístup k databázi, scénáře účtování formuláře a ověřování dat pomocí stránek předlohy pro rozložení konzistentní stránky pomocí rozhraní AJAX pro aktualizace stránky a ověřování, přihlašování uživatelů a další.

Podrobný postup sledovat, nebo si můžete stáhnout hotovou aplikaci [MVC. Music Store](https://github.com/evilDave/MVC-Music-Store).

Visual Studio 2010 SP1 nebo Visual Web Developer 2010 Express SP1 (bezplatná verze sady Visual Studio 2010) můžete použít k sestavení aplikace. Budeme používat systému SQL Server Compact (také zdarma) k hostování databáze. Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže.


- [Požadavky na visual Studio Web Developer Express SP1]
- [Technologie ASP.NET MVC 3 nástroje Update]
- [SQL Server Compact 4.0] – včetně podpory modulu runtime a nástroje


### <a name="creating-a-new-aspnet-mvc-3-project"></a>Vytvoření nového projektu ASP.NET MVC 3

Začneme tak, že vyberete "Nový projekt" nabídka soubor v aplikaci Visual Web Developer. Tím se zobrazí dialogové okno Nový projekt.

![](mvc-music-store-part-1/_static/image5.png)

Vybereme Visual C# -&gt; webové šablony skupinu na levé straně, a potom v prostředním sloupci zvolte šablonu "Aplikace pro Web ASP.NET MVC 3". Pojmenujte svůj projekt MvcMusicStore a kliknutím na tlačítko OK.

![](mvc-music-store-part-1/_static/image8.jpg)

Zobrazí se sekundární dialogové okno, které umožňuje provádět některé konkrétní nastavení MVC pro náš projekt. Vyberte následující položky:

Šablona projektu – výběr možnosti prázdná

Zobrazit modul – Výběr Razor

Použití sémantického značek HTML5 - checked

Zkontrolujte, že vaše nastavení se, jak je znázorněno níže, a kliknutím na tlačítko OK.

![](mvc-music-store-part-1/_static/image9.jpg)

Tím se vytvoří našem projektu. Pojďme se podívat na složky, které byly přidány do naší aplikace v Průzkumníku řešení na pravé straně.

![](mvc-music-store-part-1/_static/image10.jpg)

Není úplně prázdná šablona prázdná MVC 3 – přidá strukturu základní složky:

![](mvc-music-store-part-1/_static/image6.png)

ASP.NET MVC používá některé základní konvence pojmenování pro názvy složek:

| **Složka** | **Účel** |
| --- | --- |
| **/ Řadiče** | Kontrolery reagovat na vstup z prohlížeče, rozhodněte, jak ho použít a vrátit odpověď uživatele. |
| **/ Zobrazení** | Zobrazení obsahovat naše šablony uživatelského rozhraní |
| **/ Modelů** | Modely uchování a manipulaci s daty |
| **/ Obsahu** | Tato složka obsahuje naše Image, šablon stylů CSS a dalšího statického obsahu |
| **/ Skripty** | Tato složka obsahuje naši soubory jazyka JavaScript |

Tyto složky jsou zahrnuta i v aplikaci ASP.NET MVC prázdný, protože rozhraní ASP.NET MVC ve výchozím nastavení používá přístup "konvence nad konfigurací" a některé výchozí předpokladů podle konvence pojmenování složek. Například řadiče hledat zobrazení v zobrazení složky ve výchozím nastavení aniž by bylo nutné explicitně určit ve vašem kódu. Nastolit pomocí výchozích konvencí snižuje množství kódu, které musíte napsat, a také usnadnit pro jiné vývojáře k pochopení vašeho projektu. Vysvětlíme, tato konvence další, jako jsme integrovali naši aplikaci.

> [!div class="step-by-step"]
> [Next](mvc-music-store-part-2.md)
