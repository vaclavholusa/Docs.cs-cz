---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: Úvod do ASP.NET MVC 3 (C#) | Dokumentace Microsoftu
author: Rick-Anderson
description: V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, což je...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: fe98ad8a9f7891b3bf17d2da840d0c333d96a210
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756394"
---
<a name="intro-to-aspnet-mvc-3-c"></a>Úvod do ASP.NET MVC 3 (C#)
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Je k dispozici aktualizovaná verze tohoto kurzu [tady](../../../getting-started/introduction/getting-started.md) , která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.
> 
> 
> V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, což je bezplatná verze sady Microsoft Visual Studio. Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže. Nainstalujte všechny z nich kliknutím na následující odkaz: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:
> 
> - [Visual Studio Web Developer Express SP1 požadavky](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 nástroje Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podpora modulu runtime a nástroje)
> 
> Pokud používáte Visual Studio 2010 namísto Visual Web Developer 2010, nainstalujte příslušné požadované součásti po kliknutí na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt aplikace Visual Web Developer se zdrojovým kódem jazyka C# je k dispozici v tomto tématu. [Stáhněte si verzi C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Pokud dáváte přednost jazyka Visual Basic, přejděte [verze jazyka Visual Basic](../vb/intro-to-aspnet-mvc-3.md) tohoto kurzu.


## <a name="what-youll-build"></a>Co budete vytvářet

Budete implementujte jednoduchou aplikaci výpis film, který podporuje vytváření, úpravy a výpisu videa z databáze. Níže jsou uvedeny dva snímky obrazovky z aplikace, kterou vytvoříte. Stránka zobrazující seznam videa z databáze obsahuje:

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

Aplikace také umožňuje přidávat, upravovat a odstraňovat filmy, jakož i najdete v podrobnostech o jednotlivé značky. Všechny scénáře zadávání dat zahrnout ověřování k ověření, že data uložená v databázi je správná.

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a>Dovednosti, které se dozvíte

Zde je, co se dozvíte:

- Postup vytvoření nového projektu ASP.NET MVC.
- Jak vytvořit rozhraní ASP.NET MVC, kontrolerů a zobrazení.
- Jak vytvořit novou databázi pomocí Entity Framework Code First paradigma.
- Jak načíst a zobrazit data.
- Postup úpravy dat a povolení ověření dat.

## <a name="getting-started"></a>Začínáme

Začněte tím, že spuštění Visual Web Developer 2010 Express ("Visual Web Developer" zkráceně) a vyberte **nový projekt** z **Start** stránky.

Visual Web Developer je integrované vývojové prostředí, nebo integrovaného vývojového prostředí. Stejným způsobem, jako používáte k tvorbě dokumenty Microsoft Wordu, budete používat integrované vývojové prostředí pro vytváření aplikací. V aplikaci Visual Web Developer je panel nástrojů v horní zobrazuje různé možnosti, které jsou k dispozici pro vás. Je také nabídka, která nabízí další způsob, jak provádět úkoly v integrovaném vývojovém prostředí. (Například místo výběru **nový projekt** z **Start** stránky, můžete použít nabídku a vyberte **souboru** &gt; **nový projekt**.)

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a>Vytvoření vaší první aplikace

Můžete vytvářet aplikace pomocí jazyka Visual Basic nebo Visual C# jako programovací jazyk. Vyberte jazyk Visual C# na levé straně a pak vyberte **webové aplikace ASP.NET MVC 3**. Pojmenujte svůj projekt "MvcMovie" a potom klikněte na tlačítko **OK**. (Pokud dáváte přednost jazyka Visual Basic, přejděte [verze jazyka Visual Basic](../vb/intro-to-aspnet-mvc-3.md) tohoto kurzu.)

![](intro-to-aspnet-mvc-3/_static/image5.png)

V **nového projektu ASP.NET MVC 3** dialogu **internetovou aplikaci**. Zkontrolujte **značek HTML5 použití** a nechat **Razor** jako výchozí modul zobrazení.

![](intro-to-aspnet-mvc-3/_static/image6.png)

Klikněte na tlačítko **OK**. Visual Web Developer používá výchozí šablonu projektu ASP.NET MVC, které jste právě vytvořili, takže Teď máte funkční aplikaci bez teď zrovna nic nedělá! Jde jednoduchý "Hello World!" projekt a je vhodné místo pro spuštění vaší aplikace.

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

Z **ladění** nabídce vyberte možnost **spustit ladění**.

![](intro-to-aspnet-mvc-3/_static/image9.png)

Všimněte si, že klávesové zkratky pro spuštění ladění F5.

F5 způsobí, že spuštění vývojovému webovému serveru a spuštění webové aplikace Visual Web Developer. Visual Web Developer pak spustí prohlížeč a otevře domovskou stránku aplikace. Všimněte si, že na panelu Adresa prohlížeče `localhost` a nemít něco podobného `example.com`. Důvodem je, že `localhost` vždy odkazuje na vaše vlastní místní počítače v tomto případě je spuštěna aplikace právě jste vytvořili. Při spuštění projektu webové aplikace Visual Web Developer náhodný port se používá pro webový server. Na následujícím obrázku je náhodný port číslo 43246. Když aplikaci spouštíte, pravděpodobně uvidíte jiné číslo portu.

![](intro-to-aspnet-mvc-3/_static/image10.png)

Předem připravené tuto výchozí šablonu nabízí dvě stránky na web a základní přihlašovací stránku. Dalším krokem je změnit, jak tato aplikace funguje a ještě něco architekturu ASP.NET MVC hlouběji v procesu. Zavřete okno prohlížeče a Změníme kód.

> [!div class="step-by-step"]
> [Next](adding-a-controller.md)
