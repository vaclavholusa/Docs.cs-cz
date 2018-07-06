---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
title: Úvod do ASP.NET MVC 4 | Dokumentace Microsoftu
author: Rick-Anderson
description: Najít aktualizovanou verzi, pokud v tomto kurzu je k dispozici zde prostřednictvím sady Visual Studio 2013. Nové kurz používá ASP.NET MVC 5, která nabízí mnoho vylepšení v porovnání s t...
ms.author: aspnetcontent
ms.date: 08/15/2012
ms.assetid: ed66530a-04d5-49eb-b76a-85be1f57c437
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: a718eea17458fa810897b35371085848f8dca6fe
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810018"
---
<a name="intro-to-aspnet-mvc-4"></a>Úvod do ASP.NET MVC 4
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> Pokud v tomto kurzu je k dispozici aktualizovaná verze [tady](../../getting-started/introduction/getting-started.md) pomocí [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Nové kurz používá ASP.NET MVC 5, která nabízí mnoho vylepšení v porovnání s v tomto kurzu.
> 
> V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC 4 webovou aplikaci pomocí Microsoft [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) nebo Visual Web Developer 2010 Express Service Pack 1. Doporučuje se Visual Studio 2012, nebudete muset nic k dokončení tohoto kurzu instalovat. Pokud používáte Visual Studio 2010 je nutné nainstalovat následující komponenty. Můžete nainstalovat všechny z nich kliknutím na následujících odkazech:
> 
> - [Visual Studio Web Developer Express SP1 požadavky](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Instalační program identitu pracovního procesu pro technologii ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)
> - [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)
> - [SSDT](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)
> 
> Pokud používáte Visual Studio 2010 Visual Web Developer 2010, nainstalujte [instalační program identitu pracovního procesu pro technologii ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392) a: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
> 
> Projekt aplikace Visual Web Developer se zdrojovým kódem jazyka C# je k dispozici v tomto tématu. [Stáhněte si verzi C#](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip).
> 
> V tomto kurzu spustíte aplikaci v sadě Visual Studio. Můžete také zpřístupnit aplikace přes Internet nasazením k poskytovateli hostingu. Společnost Microsoft nabízí bezplatné webových hostitelských služeb pro až 10 webových serverů ve [Bezplatný zkušební účet Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Informace o tom, jak nasadit webový projekt sady Visual Studio na webu Windows Azure naleznete v tématu [vytvoření a nasazení webu ASP.NET a služby SQL Database pomocí sady Visual Studio](https://docs.microsoft.com/dotnet/azure/). Tento kurz také ukazuje způsob použití migrace Entity Framework Code First pro nasazení databáze SQL serveru do služby Windows Azure SQL Database (dřív SQL Azure).
> 
> V tomto kurzu byla zapsána od Ricka Andersona ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="what-youll-build"></a>Co budete vytvářet

> [!NOTE]
> Pokud v tomto kurzu je k dispozici aktualizovaná verze [tady](../../getting-started/introduction/getting-started.md) pomocí [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Nové kurz používá ASP.NET MVC 5, která nabízí mnoho vylepšení v porovnání s v tomto kurzu.


Budete implementujte jednoduchou aplikaci výpis film, který podporuje vytváření, úprav, vyhledávání a zobrazení videa z databáze. Níže jsou uvedeny dva snímky obrazovky z aplikace, kterou vytvoříte. Stránka zobrazující seznam videa z databáze obsahuje:

![](intro-to-aspnet-mvc-4/_static/image1.png)

Aplikace také umožňuje přidávat, upravovat a odstraňovat filmy, jakož i najdete v podrobnostech o jednotlivé značky. Všechny scénáře zadávání dat zahrnout ověřování k ověření, že data uložená v databázi je správná.

![](intro-to-aspnet-mvc-4/_static/image2.png)

## <a name="getting-started"></a>Začínáme

Začněte tím, že Visual Studio Express 2012 nebo Visual Web Developer 2010 Express. Většina snímky obrazovky v tomto použití řady Visual Studio Express 2012, ale můžete dokončit v tomto kurzu pomocí Visual Studio 2010 nebo SP1, Visual Studio 2012, Visual Studio Express 2012 nebo Visual Web Developer 2010 Express. Vyberte **nový projekt** z **Start** stránky.

Visual Studio je integrované vývojové prostředí, nebo integrovaného vývojového prostředí. Stejným způsobem, jako používáte k tvorbě dokumenty Microsoft Wordu, budete používat integrované vývojové prostředí pro vytváření aplikací. V sadě Visual Studio je panel nástrojů v horní zobrazuje různé možnosti, které jsou k dispozici pro vás. Je také nabídka, která nabízí další způsob, jak provádět úkoly v integrovaném vývojovém prostředí. (Například místo výběru **nový projekt** z **Start** stránky, můžete použít nabídku a vyberte **souboru** &gt; **nový projekt**.)

![](intro-to-aspnet-mvc-4/_static/image3.png)

## <a name="creating-your-first-application"></a>Vytvoření vaší první aplikace

Můžete vytvářet aplikace pomocí jazyka Visual Basic nebo Visual C# jako programovací jazyk. Vyberte jazyk Visual C# na levé straně a pak vyberte **webové aplikace ASP.NET MVC 4**. Pojmenujte svůj projekt &quot;MvcMovie&quot; a potom klikněte na tlačítko **OK**.

![](intro-to-aspnet-mvc-4/_static/image4.png)

V **nového projektu ASP.NET MVC 4** dialogu **internetovou aplikaci**. Ponechte **Razor** jako výchozí modul zobrazení.

![](intro-to-aspnet-mvc-4/_static/image5.png)

Klikněte na tlačítko **OK**. Visual Studio použít výchozí šablonu projektu ASP.NET MVC, které jste právě vytvořili, takže Teď máte funkční aplikaci bez teď zrovna nic nedělá! Toto je jednoduchý &quot;Hello World!&quot; projektu a je vhodné místo pro spuštění vaší aplikace.

![](intro-to-aspnet-mvc-4/_static/image6.png)

Z **ladění** nabídce vyberte možnost **spustit ladění**.

![](intro-to-aspnet-mvc-4/_static/image7.png)

Všimněte si, že klávesové zkratky pro spuštění ladění F5.

F5 způsobí, že Visual Studio ke spuštění služby IIS Express a spuštění webové aplikace. Visual Studio pak spustí prohlížeč a otevře domovskou stránku aplikace. Všimněte si, že na panelu Adresa prohlížeče `localhost` a nemít něco podobného `example.com`. Důvodem je, že `localhost` vždy odkazuje na vaše vlastní místní počítače v tomto případě je spuštěna aplikace právě jste vytvořili. Při spuštění webový projekt sady Visual Studio náhodný port se používá pro webový server. Na následujícím obrázku je číslo portu 41788. Když aplikaci spouštíte, pravděpodobně uvidíte jiné číslo portu.

![](intro-to-aspnet-mvc-4/_static/image8.png)

Předem připravené tuto výchozí šablonu vám Home, kontaktů a o stránky. Také poskytuje podporu pro registraci a přihlaste se a obsahuje odkazy na Facebook a Twitter. Dalším krokem je změnit, jak tato aplikace funguje a přečtěte si ještě něco o architektuře ASP.NET MVC. Zavřete okno prohlížeče a Změníme kód.

> [!div class="step-by-step"]
> [Next](adding-a-controller.md)
