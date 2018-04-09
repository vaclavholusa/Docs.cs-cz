---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: Úvod do architektury ASP.NET MVC 3 (C#) | Microsoft Docs
author: Rick-Anderson
description: V tomto kurzu naučit se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, který je...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 9b965df6175051a084de35627160161c116be42d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="intro-to-aspnet-mvc-3-c"></a>Úvod do architektury ASP.NET MVC 3 (C#)
====================
podle [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Je k dispozici aktualizovaná verze tohoto kurzu [sem](../../../getting-started/introduction/getting-started.md) používající ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.
> 
> 
> V tomto kurzu naučit se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, který je bezplatnou verzi sady Microsoft Visual Studio. Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže. Kliknutím na následující odkaz můžete nainstalovat všechny z nich: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:
> 
> - [Visual Studio Web Developer Express SP1 požadavky](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizace nástrojů rozhraní ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podporu runtime + nástroje)
> 
> Pokud používáte Visual Studio 2010 místo Visual Web Developer 2010, nainstalujte součásti kliknutím na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer se zdrojový kód C# je k dispozici v tomto tématu. [Stáhnout verzi jazyka C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Pokud dáváte přednost jazyka Visual Basic, přepnout [verze jazyka Visual Basic](../vb/intro-to-aspnet-mvc-3.md) tohoto kurzu.


## <a name="what-youll-build"></a>Co budete sestavení

Budete implementovat jednoduchou aplikaci seznamu film, která podporuje vytváření, úpravy a výpis filmy z databáze. Níže jsou dva snímky obrazovky aplikace, kterou budete sestavení. Obsahuje stránky, který zobrazí seznam filmy z databáze:

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

Aplikace také umožňuje přidávat, upravovat a odstraňovat filmy, jakož i najdete podrobnosti o jednotlivých ty. Všechny scénáře zadávání dat zahrnují ověření a zkontrolujte, že data uložená v databázi správné.

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a>Dovedností, které se dozvíte

Zde je, co se dozvíte:

- Postup vytvoření nového projektu ASP.NET MVC.
- Jak vytvořit rozhraní ASP.NET MVC, kontrolery a zobrazení.
- Jak vytvořit novou databázi pomocí Entity Framework Code First zlepší.
- Jak načíst a zobrazit data.
- Jak upravit data a povolit data ověření.

## <a name="getting-started"></a>Začínáme

Spusťte spuštěním Visual Web Developer 2010 Express ("Visual Web Developer" pro zkrácení) a vyberte **nový projekt** z **spustit** stránky.

Visual Web Developer je IDE, nebo integrované vývojové prostředí. Stejně jako zápisu dokumentů pomocí aplikace Microsoft Word, použijete k vytvoření aplikací rozhraní IDE. V aplikaci Visual Web Developer je panelu nástrojů v horní zobrazuje různé možnosti, které jsou k dispozici pro vás. Je také nabídku, která obsahuje jiný způsob, jak provádět úlohy v prostředí IDE. (Například místo výběru **nový projekt** z **spustit** stránky, můžete použít nabídku a vyberte **soubor** &gt; **nový projekt**.)

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a>Vytvoření vaší první aplikace

Můžete vytvořit pomocí jazyka Visual Basic a Visual C# jako programovací jazyk aplikace. Vyberte Visual C# na levé straně a pak vyberte **webové aplikace ASP.NET MVC 3**. Název projektu "MvcMovie" a pak klikněte na **OK**. (Pokud dáváte přednost jazyka Visual Basic, přepnout [verze jazyka Visual Basic](../vb/intro-to-aspnet-mvc-3.md) tohoto kurzu.)

![](intro-to-aspnet-mvc-3/_static/image5.png)

V **nový ASP.NET MVC 3 projekt** dialogové okno, vyberte **Internetové aplikace**. Zkontrolujte **HTML5 použití značek** a nechte **Razor** jako výchozí modul zobrazení.

![](intro-to-aspnet-mvc-3/_static/image6.png)

Click **OK**. Visual Web Developer použít výchozí šablonu pro projektu ASP.NET MVC, který jste právě vytvořili, takže budete mít funkční aplikaci, ihned bez jakékoli akce! Jedná se jednoduché "Hello, World!" projektu ale je dobrým místem, kde spustit aplikaci.

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

Z **ladění** nabídce vyberte možnost **spustit ladění**.

![](intro-to-aspnet-mvc-3/_static/image9.png)

Všimněte si, že je klávesové zkratky pro spuštění ladění F5.

F5 způsobí, že Visual Web Developer spustí webový server vývoj a spuštění webové aplikace. Visual Web Developer pak spustí prohlížeč a otevře domovskou stránku aplikace. Všimněte si, že panelu Adresa prohlížeče uvádí `localhost` a není něco jako `example.com`. Je to způsobeno `localhost` vždycky směřuje na vlastní místní počítač, který běží v tomto případě aplikace, které jste právě vytvořili. Po spuštění webového projektu Visual Web Developer náhodný port se používá pro webový server. Na následujícím obrázku je číslo portu náhodných 43246. Při spuštění aplikace, pravděpodobně uvidíte jiné číslo portu.

![](intro-to-aspnet-mvc-3/_static/image10.png)

Okamžitě po nasazení této výchozí šablony vám dává dvě stránky přejděte a základní přihlašovací stránku. Dalším krokem je a změnit tak, jak tato aplikace funguje trochu informace o architektuře ASP.NET MVC v procesu. Zavřete prohlížeč a zkuste změnit nějaký kód.

> [!div class="step-by-step"]
> [Next](adding-a-controller.md)
