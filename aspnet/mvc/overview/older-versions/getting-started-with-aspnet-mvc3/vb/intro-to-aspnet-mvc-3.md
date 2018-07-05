---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
title: Úvod do ASP.NET MVC 3 (VB) | Dokumentace Microsoftu
author: Rick-Anderson
description: V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, což je...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: a1b3d789-93b4-487f-b90d-80c9c9b4f8fa
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 1a870f10d39980335c7acb4a543907fecd408eb2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372350"
---
<a name="intro-to-aspnet-mvc-3-vb"></a>Úvod do ASP.NET MVC 3 (VB)
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, což je bezplatná verze sady Microsoft Visual Studio. Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže. Nainstalujte všechny z nich kliknutím na následující odkaz: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:
> 
> - [Visual Studio Web Developer Express SP1 požadavky](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 nástroje Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podpora modulu runtime a nástroje)
> 
> Pokud používáte Visual Studio 2010 namísto Visual Web Developer 2010, nainstalujte příslušné požadované součásti po kliknutí na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt aplikace Visual Web Developer se zdrojovým kódem VB.NET je k dispozici v tomto tématu. [Stáhněte si verzi VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Pokud dáváte přednost C#, přejděte [verze jazyka C#](../cs/intro-to-aspnet-mvc-3.md) tohoto kurzu.


V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, což je bezplatná verze sady Microsoft Visual Studio. Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže. Nainstalujte všechny z nich kliknutím na následující odkaz: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:

- [Visual Studio Web Developer Express SP1 požadavky](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [ASP.NET MVC 3 nástroje Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podpora modulu runtime a nástroje)

Pokud používáte Visual Studio 2010 namísto Visual Web Developer 2010, nainstalujte příslušné požadované součásti po kliknutí na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

Projekt aplikace Visual Web Developer se zdrojovým kódem jazyka Visual Basic je k dispozici v tomto tématu. [Stáhněte si verzi jazyka Visual Basic zde](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=14824). Pokud dáváte přednost CSharp, přepněte [CSharp verze](../cs/intro-to-aspnet-mvc-3.md) tohoto kurzu.

## <a name="what-youll-build"></a>Co budete vytvářet

Budete implementujte jednoduchou aplikaci výpis film, který podporuje vytváření, úpravy a výpisu videa z databáze. Níže jsou uvedeny dva snímky obrazovky z aplikace, kterou vytvoříte. Stránka zobrazující seznam videa z databáze obsahuje:

[![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image2.png)](intro-to-aspnet-mvc-3/_static/image1.png)

Aplikace také umožňuje přidávat, upravovat a odstraňovat filmy, jakož i najdete v podrobnostech o jednotlivé značky. Všechny scénáře zadávání dat zahrnout ověřování k ověření, že data uložená v databázi je správná.

[![CreateFormSo](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="skills-youll-learn"></a>Dovednosti, které se dozvíte

Zde je, co se dozvíte:

- Postup vytvoření nového projektu ASP.NET MVC
- Jak vytvořit novou databázi pomocí založeno na kódu Entity Framework
- Jak vytvořit rozhraní ASP.NET MVC, kontrolerů a zobrazení
- Načtení a zobrazení dat
- Úprava dat a povolení ověření dat

## <a name="getting-started"></a>Začínáme

Začněte tím, že spuštění Visual Web Developer 2010 Express ("VWD" zkráceně) a vyberte **nový projekt** z **Start** stránky.

Visual Web Developer je integrované vývojové prostředí, nebo integrovaného vývojového prostředí. Stejným způsobem, jako používáte k tvorbě dokumenty Microsoft Wordu, budete používat integrované vývojové prostředí pro vytváření aplikací. V aplikaci Visual Web Developer je panel nástrojů v horní zobrazuje různé možnosti, které jsou k dispozici pro vás. Je také nabídka, která nabízí další způsob, jak provádět úkoly v integrovaném vývojovém prostředí. (Například místo výběru **nový projekt** z **Start** stránky, můžete použít nabídku a vyberte **souboru** &gt; **nový projekt**.)

[![](intro-to-aspnet-mvc-3/_static/image6.png)](intro-to-aspnet-mvc-3/_static/image5.png)

## <a name="creating-your-first-application"></a>Vytvoření vaší první aplikace

Můžete vytvářet aplikace pomocí podle vašeho výběru jazyka Visual Basic nebo Visual C# jako programovací jazyk. Pro účely tohoto kurzu vyberte jazyka Visual Basic na levé straně a pak vyberte **webové aplikace ASP.NET MVC 3**. Pojmenujte svůj projekt "MvcMovie" a potom klikněte na tlačítko **OK**.

![1NewMVCproj_sm](intro-to-aspnet-mvc-3/_static/image7.png)

V **nového projektu ASP.NET MVC 3** dialogu **internetovou aplikaci**. Ponechte **Razor** jako výchozí modul zobrazení.

![1InternetAppRazor_SM](intro-to-aspnet-mvc-3/_static/image8.png)

Klikněte na tlačítko **OK**. Visual Web Developer používá výchozí šablonu projektu ASP.NET MVC, které jste právě vytvořili, takže Teď máte funkční aplikaci bez teď zrovna nic nedělá! Jde jednoduchý "Hello World!" projekt a je vhodné místo pro spuštění vaší aplikace.

[![](intro-to-aspnet-mvc-3/_static/image10.png)](intro-to-aspnet-mvc-3/_static/image9.png)

Z **ladění** nabídce vyberte možnost **spustit ladění**.

![](intro-to-aspnet-mvc-3/_static/image11.png)

Všimněte si, že klávesové zkratky pro spuštění ladění F5.

F5 způsobí, že spuštění vývojovému webovému serveru a spuštění webové aplikace Visual Web Developer. VWD pak spustí prohlížeč a otevře domovskou stránku aplikace. Všimněte si, že na panelu Adresa prohlížeče `localhost` a nemít něco podobného `example.com`. Důvodem je, že `localhost` vždy odkazuje na vaše vlastní místní počítače v tomto případě je spuštěna aplikace právě jste vytvořili. Při spuštění webový projekt VWD náhodný port se používá pro projekt. Na následujícím obrázku je náhodný port číslo 43246. Váš projekt bude pravděpodobně používat jiné číslo portu.

![](intro-to-aspnet-mvc-3/_static/image12.png)

Ihned poskytuje tuto výchozí šablonu můžete dvě stránky přejděte a základní přihlašovací stránku. Umožňuje změnit, jak tato aplikace funguje a ještě něco architekturu ASP.NET MVC hlouběji v procesu. Zavřete okno prohlížeče a Změníme kód.

> [!div class="step-by-step"]
> [Next](adding-a-controller.md)
