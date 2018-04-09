---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Část 1: Přehled a soubor -> Nový projekt | Microsoft Docs'
author: jongalloway
description: Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště. Část 1 obsahuje přehled a soubor -> Nový projekt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 2082927d18c95563893da199d60347fa15952446
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="part-1-overview-and-file-new-project"></a>Část 1: Přehled a soubor -> Nový projekt
====================
podle [Jon Galloway](https://github.com/jongalloway)

> Úložiště Hudba MVC je kurz aplikace, která představuje a vysvětluje krok za krokem, jak používat rozhraní ASP.NET MVC a Visual Studio pro vývoj webů.  
>   
> Úložiště Hudba MVC je implementace úložiště lightweight ukázkové, který prodává hudebních alb online a implementuje základní Správa serveru, přihlášení uživatele a nákupního košíku funkce.  
>   
> Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště. Část 1 obsahuje přehled a soubor -&gt;nový projekt.


## <a name="overview"></a>Přehled

Úložiště Hudba MVC je kurz aplikace, která představuje a vysvětluje podrobné postupy pro vývoj webů používat rozhraní ASP.NET MVC a aplikace Visual Web Developer. Jsme budete od pomalu, takže vývoj webů úrovni začátečník prostředí je to v pořádku.

Aplikace, kterou jsme budete sestavení je jednoduchý Hudba úložiště. Existují tři hlavní části aplikace: nákupy, najdete v článku věnovaném a správu.

![](mvc-music-store-part-1/_static/image1.jpg)

Návštěvníky můžete procházet alba Genre:

![](mvc-music-store-part-1/_static/image2.jpg)

Mohou zobrazovat jednoho alba a přidat jej do jejich košíku:

![](mvc-music-store-part-1/_static/image3.jpg)

Jejich můžete zkontrolovat jejich košíku odebrání všech položek, které už mají:

![](mvc-music-store-part-1/_static/image4.jpg)

Přistoupíte k ověření se výzva k přihlášení nebo registraci pro uživatelský účet.

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

Po vytvoření účtu, mohou být provedeny pořadí tak, že vyplníte informace o přesouvání a platba. Pro zjednodušení jsme používáte úžasné povýšení: vše je zdarma, pokud se kód povýšení "FREE"!

![](mvc-music-store-part-1/_static/image5.jpg)

Po změně pořadí, uvidí jednoduché potvrzovací obrazovce:

![](mvc-music-store-part-1/_static/image6.jpg)

Kromě stránek faceing zákazníka jsme také sestavíte oddílu správce, který obsahuje seznam alb, ze kterých mohou správci vytvořit, upravit a odstranit alb:

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a>1. Soubor -&gt; nový projekt

### <a name="installing-the-software"></a>Instalace softwaru

V tomto kurzu začneme vytvořením nového projektu ASP.NET MVC 3 pomocí volné Visual Web Developer 2010 Express (což je bezplatná) a pak postupně přidáme funkce k vytvoření kompletní funkční aplikaci. Na této cestě jsme zaměříme přístup k databázi, scénáře příspěvků formuláře, ověřování dat pomocí stránky předlohy pro rozložení konzistentní stránky, pomocí rozhraní AJAX pro aktualizace stránky a ověřování, přihlášení uživatele a další.

Můžete absolvovat krok za krokem, nebo si můžete stáhnout hotová aplikace z [MVC. Hudba úložiště](https://github.com/evilDave/MVC-Music-Store).

Visual Studio 2010 SP1 nebo Visual Web Developer 2010 Express SP1 (bezplatné verze sady Visual Studio 2010) slouží k sestavení aplikace. Budeme používat systému SQL Server Compact (také zdarma) k hostování databáze. Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže.


- [Visual Studio Web Developer Express SP1 požadavky]
- [Aktualizace nástrojů rozhraní ASP.NET MVC 3]
- [SQL Server Compact 4.0] - včetně modul runtime a nástroje podpory


### <a name="creating-a-new-aspnet-mvc-3-project"></a>Vytvoření nového projektu ASP.NET MVC 3

Začneme výběrem "Nový projekt" v nabídce Soubor v aplikaci Visual Web Developer. Otevře dialogové okno Nový projekt.

![](mvc-music-store-part-1/_static/image5.png)

Jsme vyberte Visual C# -&gt; webové šablony skupiny na levé straně, a potom vyberte šablonu "ASP.NET MVC 3 webové aplikace" v prostředním sloupci. Název projektu MvcMusicStore a klepněte na tlačítko OK.

![](mvc-music-store-part-1/_static/image8.jpg)

Tato akce zobrazí sekundární dialog, který umožňuje, aby některých specifických nastavení MVC pro naše projekt. Vyberte následující položky:

Šablona projektu – vyberte prázdný

Zobrazit modul – vyberte Razor

Použití sémantického kód HTML5 - zaškrtnutí

Ověřte, zda jsou následující nastavení, poté na tlačítko OK.

![](mvc-music-store-part-1/_static/image9.jpg)

Tím se vytvoří naše projektu. Podívejme se na složky, které jsou přidané do naší aplikaci v Průzkumníku řešení na pravé straně.

![](mvc-music-store-part-1/_static/image10.jpg)

Není úplně prázdné šablonu prázdný MVC 3 – přidá do základní složky struktury:

![](mvc-music-store-part-1/_static/image6.png)

ASP.NET MVC využívá některé základní zásady vytváření názvů pro názvy složek:

| **Složka** | **Účel** |
| --- | --- |
| **/ Řadiče** | Řadiče reagovat na vstup z prohlížeče, rozhodněte, jak ho použít a vrátí odpověď uživatele. |
| **Nebo zobrazení.** | Zobrazení uložení naše šablony uživatelského rozhraní |
| **/ Modely** | Modely uložení a pracovat s daty |
| **A obsah** | Tato složka obsahuje naše obrázky, CSS a dalšího statického obsahu |
| **A skriptů** | Tato složka obsahuje soubory naše JavaScript |

Tyto složky jsou zahrnuty i v aplikaci ASP.NET MVC prázdný, protože rozhraní ASP.NET MVC ve výchozím nastavení používá přístup "konvence přes konfigurace" a některé výchozí předpokladů podle konvence pojmenování složek. Pro instanci řadiče vyhledejte zobrazení ve složce zobrazení ve výchozím nastavení bez nutnosti explicitně určovat to ve vašem kódu. Provedením vykrvovacího vpichu s výchozích konvencí snižuje množství kód, který potřebujete k zápisu, a můžete také usnadňují jinými vývojáři pochopit projektu. Vysvětlíme tyto konvence další, jak jsme sestavení aplikace.

> [!div class="step-by-step"]
> [Next](mvc-music-store-part-2.md)
