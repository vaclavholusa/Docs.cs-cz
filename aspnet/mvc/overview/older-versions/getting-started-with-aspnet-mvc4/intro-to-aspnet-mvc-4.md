---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
title: "Úvod do architektury ASP.NET MVC 4 | Microsoft Docs"
author: Rick-Anderson
description: "Aktualizovaná verze, pokud v tomto kurzu je k dispozici zde pomocí sady Visual Studio 2013. Nový kurz používá ASP.NET MVC 5, který nabízí mnoho vylepšení v porovnání s t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: ed66530a-04d5-49eb-b76a-85be1f57c437
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 92d9e583b6c26fa8c928d33e14593d280702a269
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="intro-to-aspnet-mvc-4"></a>Úvod do architektury ASP.NET MVC 4
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> Pokud v tomto kurzu je k dispozici aktualizovaná verze [sem](../../getting-started/introduction/getting-started.md) pomocí [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Nový kurz používá ASP.NET MVC 5, který obsahuje mnoho vylepšení v tomto kurzu.
> 
> V tomto kurzu se naučit se základy vytváření ASP.NET MVC 4 webovou aplikaci pomocí Microsoft [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) nebo Visual Web Developer 2010 Express Service Pack 1. Doporučuje se Visual Studio 2012, nebudete muset nic k dokončení tohoto kurzu instalovat. Pokud používáte Visual Studio 2010 je nutné nainstalovat následující součásti. Všechny z nich můžete nainstalovat pomocí následujících odkazů:
> 
> - [Visual Studio Web Developer Express SP1 požadavky](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Instalační program identitu pracovního procesu pro technologii ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)
> - [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)
> - [SSDT](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)
> 
> Pokud používáte Visual Studio 2010 místo Visual Web Developer 2010, nainstalujte [identitu pracovního procesu instalační služby pro technologii ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392) a: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
> 
> Projekt Visual Web Developer se zdrojový kód C# je k dispozici v tomto tématu. [Stáhnout verzi jazyka C#](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip).
> 
> V tomto kurzu spusťte aplikaci v sadě Visual Studio. Můžete provést také aplikace dostupné přes Internet pomocí nasazení do hostujícího zprostředkovatele. Společnost Microsoft nabízí bezplatné webových hostitelských služeb pro až 10 webových serverů ve [Bezplatný zkušební účet systému Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Informace o tom, jak nasadit webový projekt sady Visual Studio na webu systému Windows Azure najdete v tématu [vytvořit a nasadit webu technologie ASP.NET a SQL Database pomocí sady Visual Studio](https://docs.microsoft.com/dotnet/azure/). Tento kurz také ukazuje způsob použití migrace Entity Framework Code First pro nasazení databáze systému SQL Server do systému Windows Azure SQL Database (dříve SQL Azure).
> 
> V tomto kurzu byla zapsána od Ricka Andersona ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="what-youll-build"></a>Co budete sestavení

> [!NOTE]
> Pokud v tomto kurzu je k dispozici aktualizovaná verze [sem](../../getting-started/introduction/getting-started.md) pomocí [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Nový kurz používá ASP.NET MVC 5, který obsahuje mnoho vylepšení v tomto kurzu.


Budete implementovat jednoduchou aplikaci seznamu film, která podporuje vytváření, úpravy, vyhledávání a výpis filmy z databáze. Níže jsou dva snímky obrazovky aplikace, kterou budete sestavení. Obsahuje stránky, který zobrazí seznam filmy z databáze:

![](intro-to-aspnet-mvc-4/_static/image1.png)

Aplikace také umožňuje přidávat, upravovat a odstraňovat filmy, jakož i najdete podrobnosti o jednotlivých ty. Všechny scénáře zadávání dat zahrnují ověření a zkontrolujte, že data uložená v databázi správné.

![](intro-to-aspnet-mvc-4/_static/image2.png)

## <a name="getting-started"></a>Začínáme

Spusťte spuštěním Visual Studio Express 2012 nebo Visual Web Developer 2010 Express. Většina snímky obrazovky v této pomocí řady Visual Studio Express 2012, ale můžete dokončit tento kurz pomocí sady Visual Studio 2010 nebo SP1, Visual Studio 2012, Visual Studio Express 2012 nebo Visual Web Developer 2010 Express. Vyberte **nový projekt** z **spustit** stránky.

Visual Studio je IDE, nebo integrované vývojové prostředí. Stejně jako zápisu dokumentů pomocí aplikace Microsoft Word, použijete k vytvoření aplikací rozhraní IDE. V sadě Visual Studio je panelu nástrojů v horní zobrazuje různé možnosti, které jsou k dispozici pro vás. Je také nabídku, která obsahuje jiný způsob, jak provádět úlohy v prostředí IDE. (Například místo výběru **nový projekt** z **spustit** stránky, můžete použít nabídku a vyberte **soubor** &gt; **nový projekt**.)

![](intro-to-aspnet-mvc-4/_static/image3.png)

## <a name="creating-your-first-application"></a>Vytvoření vaší první aplikace

Můžete vytvořit pomocí jazyka Visual Basic a Visual C# jako programovací jazyk aplikace. Vyberte Visual C# na levé straně a pak vyberte **webové aplikace ASP.NET MVC 4**. Název projektu &quot;MvcMovie&quot; a pak klikněte na **OK**.

![](intro-to-aspnet-mvc-4/_static/image4.png)

V **nový ASP.NET MVC 4 projekt** dialogové okno, vyberte **Internetové aplikace**. Nechte **Razor** jako výchozí modul zobrazení.

![](intro-to-aspnet-mvc-4/_static/image5.png)

Click **OK**. Visual Studio použít výchozí šablonu pro projektu ASP.NET MVC, který jste právě vytvořili, takže budete mít funkční aplikaci, ihned bez jakékoli akce! Toto je jednoduchý &quot;Hello, World!&quot; projektu ale je dobrým místem, kde spustit aplikaci.

![](intro-to-aspnet-mvc-4/_static/image6.png)

Z **ladění** nabídce vyberte možnost **spustit ladění**.

![](intro-to-aspnet-mvc-4/_static/image7.png)

Všimněte si, že je klávesové zkratky pro spuštění ladění F5.

F5 způsobí, že chcete spustit službu IIS Express a spuštění webové aplikace Visual Studio. Visual Studio pak spustí prohlížeč a otevře domovskou stránku aplikace. Všimněte si, že panelu Adresa prohlížeče uvádí `localhost` a není něco jako `example.com`. Je to způsobeno `localhost` vždycky směřuje na vlastní místní počítač, který běží v tomto případě aplikace, které jste právě vytvořili. Po spuštění webového projektu sady Visual Studio náhodný port se používá pro webový server. Na obrázku níže číslo portu je 41788. Při spuštění aplikace, pravděpodobně uvidíte jiné číslo portu.

![](intro-to-aspnet-mvc-4/_static/image8.png)

Okamžitě po nasazení této výchozí šablony vám dává domácí, kontaktů a o stránky. Také poskytuje podporu registrace a přihlášení a obsahuje odkazy na Facebook nebo Twitter. Dalším krokem je a změnit tak, jak tato aplikace funguje trochu Další informace o architektuře ASP.NET MVC. Zavřete prohlížeč a zkuste změnit nějaký kód.

>[!div class="step-by-step"]
[Next](adding-a-controller.md)
