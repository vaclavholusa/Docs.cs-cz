---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
title: 'Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio: Úvod - 1 12 | Microsoft Docs'
author: tdykstra
description: Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET projektu webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí Visual samostatného...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a2d7f33b-8c4a-4b48-9fb1-9139cf9b9878
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
msc.type: authoredcontent
ms.openlocfilehash: 3f1572bb890ee136cdd746040a5efae2ce537116
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30883281"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-introduction---1-of-12"></a>Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio: Úvod - 1 12
====================
Podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si úvodní projekt](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET projektu webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí sady Visual Studio 2012 RC nebo Visual Studio Express 2012 RC pro Web. Visual Studio 2010 můžete také použít při instalaci aktualizace Publikovat Web.
> 
> Kurz, který ukazuje nasazení funkce zavedená po vydání sady Visual Studio 2012 RC, ukazuje, jak nasadit edicích systému SQL Server než SQL Server Compact a ukazuje, jak nasadit do Azure App Service Web Apps, naleznete v části [nasazení webu ASP.NET pomocí sady Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).
> 
> Tyto kurzy vás provede nasazením nejprve do služby IIS ve svém místním vývojovém počítači pro testování a k poskytovateli hostingu třetích stran. Aplikace, která budete nasazovat používá databázi aplikace a databáze členství technologie ASP.NET. Můžete začít používat systém SQL Server Compact a nasazení do systému SQL Server Compact a novější kurzy vám ukážou, jak nasadit změny v databázi a jak migrovat do systému SQL Server.
> 
> Kurzů k předpokládá, že víte, jak pracovat s technologií ASP.NET v sadě Visual Studio. Pokud to neuděláte, je vhodné oddělení na zahájení [základní technologie ASP.NET Web Forms kurzu](../tailspin-spyworks/tailspin-spyworks-part-1.md) nebo [základní kurz k ASP.NET MVC](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md).
> 
> Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum nasazení ASP.NET](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment).


## <a name="overview"></a>Přehled

Tyto kurzy vás provede nasazením nejprve do služby IIS ve svém místním vývojovém počítači pro testování a k poskytovateli hostingu třetích stran. Aplikace, která budete nasazovat používá databázi aplikace a databáze členství technologie ASP.NET. Můžete začít používat systém SQL Server Compact a nasazení do systému SQL Server Compact a novější kurzy vám ukážou, jak nasadit změny v databázi a jak migrovat do systému SQL Server.

Číslo kurzů – 11 ve všech a řešení potíží stránky – provést proces nasazení pravděpodobně složitý. Ve skutečnosti ale základní postupy pro nasazení webu tvoří relativně malou část kurzu sady. Ale v situacích, reálného, často potřebujete informace o některé další aspekt malý, ale důležité nasazení – například nastavení oprávnění složky na cílovém serveru. Mnoho z těchto postupů další jsme zahrnuli v kurzech, s naděje, který kurzů k nenechávejte informace, které mohou zabránit úspěšně nasazení reálné aplikaci.

Kurzy jsou navrženy pro spouštění v pořadí, a každou část vytvoří v předchozí části. Nicméně, můžete přeskočit částí, které nejsou relevantní pro vaši situaci. (Přeskočení částí může vyžadovat upravit postupy v dalších kurzech.)

## <a name="intended-audience"></a>Cílová skupina

Kurzy jsou zaměřené na vývojáře využívající technologii ASP.NET, kteří pracují v malé organizace nebo jiných prostředích kde:

- Proces průběžnou integraci (automatizovaných sestaveních a nasazení), se nepoužije.
- V provozním prostředí je poskytovatele hostitelských služeb třetích stran.
- Jedna osoba obvykle doplní víc rolí (stejná osoba sama vyvinula, testuje a nasadí).

V podnikových prostředích je typičtější pro implementaci procesů průběžnou integraci a provozním prostředí je obvykle hostované servery vlastní společnosti. Jiné osoby také běžně provádět různé role. Informace o podnikové nasazení najdete v tématu [nasazení webové aplikace v podnikové scénáře](../../deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).

Organizací všech velikostí můžete taky nasadit webové aplikace do Azure a většina postupy uvedené v těchto kurzech platí také pro Azure App Services Web Apps. Úvod do Azure, najdete v části [ https://azure.microsoft.com ](https://azure.microsoft.com).

## <a name="the-hosting-provider-shown-in-the-tutorials"></a>Poskytovatel hostingu ukazuje kurzů k

Kurzy vás provede procesem vytvoření účtu s hostingové společnosti a nasazení aplikace do tohoto poskytovatele hostitelských služeb. Konkrétní hostingové společnosti jste vybrali, takže může kurzů k objasnění dokončení prostředí nasazení na živý web. Každý hostující společnost poskytuje různých funkcí a možností nasazení serverů se liší poněkud; proces popsaný v tomto kurzu se ale typické pro celý proces.

Poskytovatel hostingu používá pro účely tohoto kurzu, Cytanium.com, je jedním z mnoha, které jsou k dispozici, a jeho použití v tomto kurzu nepředstavuje ke schválení nebo doporučení.

## <a name="deploying-web-site-projects"></a>Nasazení webové projekty

Contoso univerzity je projekt webové aplikace Visual Studio. Většina metody nasazení a nástroje ukázáno v tomto kurzu se nevztahují na [webové projekty](https://msdn.microsoft.com/library/dd547590.aspx). Informace o tom, jak nasadit webové projekty najdete v tématu [mapa obsahu nasazení ASP.NET](https://msdn.microsoft.com/library/bb386521.aspx#deployment_for_web_site_projects).

## <a name="deploying-aspnet-mvc-projects"></a>Nasazení projekty ASP.NET MVC

V tomto kurzu nasazujete projekt webových formulářů ASP.NET, ale všechno, co zjistíte, jak to provést je také použít na rozhraní ASP.NET MVC. Projekt Visual Studio MVC je právě jinou formu projekt webové aplikace. Jediným rozdílem je, že pokud nasazujete do poskytovatele hostitelských služeb, která nepodporuje rozhraní ASP.NET MVC nebo vaší je cílové verzi, je nutné vybrat jistotu, že jste nainstalovali odpovídající ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0) nebo [MVC 4](http://nuget.org/packages/aspnetmvc)) Balíček NuGet do projektu.

## <a name="programming-language"></a>Programovací jazyk

Ukázková aplikace používá C#, ale kurzů k nevyžadují znalosti jazyka C# a techniky nasazení zobrazí kurzů k nejsou specifické pro jazyk.

## <a name="troubleshooting-during-this-tutorial"></a>Řešení potíží s při tomto kurzu

Když dojde k chybě během nasazení, nebo bude web nefunguje správně, neposkytují chybové zprávy vždy řešení. Které vám pomohou s některé běžné scénáře problém, [stránka s referencemi k řešení potíží s](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md) je k dispozici. Pokud se zobrazí chybové hlášení, nebo něco nefunguje tak, jak projděte následující kurzy, nezapomeňte zkontrolovat stránce řešení potíží.

## <a name="comments-welcome"></a>Vítá komentáře

Komentáře k kurzů k jsou úvodní a při aktualizaci kurzu veškeré úsilí, budou provedeny brát v účtu opravy nebo návrhy na vylepšení, které jsou k dispozici v kurzu komentáře.

## <a name="prerequisites"></a>Požadavky

Než začnete, ujistěte se, že máte Windows 7 nebo novější a ve vašem počítači nainstalována jedna z následujících produktů:

- [Visual Studio 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
- [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VWD2010SP1Pack)
- [Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web](https://go.microsoft.com/fwlink/?LinkId=240162)

Pokud máte Visual Studio 2010 SP1 nebo Visual Web Developer Express 2010 SP1, nainstalujte také následující produkty:

- [Azure SDK pro platformu .NET (VS 2010 SP1)](https://go.microsoft.com/fwlink/?LinkID=208120) (zahrnuje aktualizace Publikovat Web)
- [Nástroje Microsoft Visual Studio 2010 SP1 pro systém SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCEVSTools)

Některé další software je vyžadováno k dokončení tohoto kurzu, ale nemáte nastaven, který dosud načtena. Tento kurz vás provede kroky pro instalaci se v případě potřeby.

## <a name="downloading-the-sample-application"></a>Stažení ukázkové aplikace

Aplikace, které budete nasazovat jmenuje Contoso univerzity a již bylo vytvořeno za vás. Je zjednodušenou verzi webu univerzity, volně podle aplikace Contoso univerzity podrobněji popsaná [Entity Framework – kurzy na webu technologie ASP.NET](https://asp.net/entity-framework/tutorials).

Až budete mít nainstalovány požadované součásti, stáhněte si [webové aplikace Contoso univerzity](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b). *.Zip* soubor obsahuje více verzí projekt a soubor PDF, který obsahuje všechny 12 kurzy. Chcete-li provede kroky kurzu, začněte ContosoUniversity Begin. Pokud chcete zjistit, co projektu vypadá na konci kurzů k, otevřete ContosoUniversity-End. Pokud chcete zobrazit, co projektu vypadá jako před migrací na plnou instalaci systému SQL Server v kurzu 10, otevřete ContosoUniversity AfterTutorial09.

Pokud chcete připravit na kurz kroky spolupracovat, uložte ContosoUniversity Begin do libovolné složky použijete pro práci s projektů sady Visual Studio. Ve výchozím nastavení je to následující složku:

`C:\Users\<username>\Documents\Visual Studio 2012\Projects`

(Pro snímky obrazovky v tomto kurzu, složce projektu se nachází v kořenovém adresáři na `C`: jednotku.)

Spuštění sady Visual Studio, otevřete projekt a stiskněte klávesu CTRL + F5 ji spustit.

[![Home_page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image2.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image1.png)

Webových stránek jsou přístupné z řádku nabídek a umožňují provádět následující funkce:

- Zobrazí statistické údaje o student (stránku o).
- Zobrazit, upravit, odstranit a přidejte studenty.
- Zobrazit a upravit kurzy.
- Zobrazení a úpravě vyučující.
- Zobrazení a úpravě oddělení.

Tady jsou snímky obrazovky několik reprezentativní stránek.

[![Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image4.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image3.png)

[![Add_Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image6.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image5.png)

## <a name="reviewing-application-features-that-affect-deployment"></a>Kontrola funkce aplikace, které ovlivňují nasazení

Následující funkce aplikace budou mít vliv nasazení nebo co musíte udělat, aby ho nasadit. Každý z nich je vysvětlené podrobněji v následujících kurzech v řadě.

- Contoso univerzity používá systém SQL Server Compact databázi k ukládání dat aplikací, například studenty a lektorem názvy. Databáze obsahuje směs testovací data a provozními daty, a při nasazení do produkčního prostředí je potřeba vyloučit v testovacích datech. Později v kurzu řady budete migraci ze systému SQL Server Compact do systému SQL Server.
- Aplikace používá systém členství technologie ASP.NET, které jsou uloženy informace o uživatelském účtu v databázi systému SQL Server Compact. Aplikace definuje uživatel s oprávněním správce, který má přístup k určité omezené informace. Je třeba nasadit bez testovací účty, ale jeden účet správce databáze členství.
- Vzhledem k databázi aplikace a databáze členství používá systém SQL Server Compact jako databázový stroj, je nutné nasadit databázový stroj pro poskytovatele hostingu, jakož i samotných databázích.
- Aplikace používá zprostředkovatelů universal členství prostředí ASP.NET, aby systém členství můžete uložit svá data v databázi systému SQL Server Compact. Sestavení, které obsahuje universal členství zprostředkovatele musí být nasazen s aplikací.
- Aplikace používá Entity Framework 5.0 pro přístup k datům v databázi aplikace. Sestavení, které obsahuje Entity Framework 5.0 musí být nasazen s aplikací.
- Aplikace používá chyba třetích stran, protokolování a vytváření sestav nástroje. Tento nástroj je součástí sestavení, které musí být nasazeno s aplikací.
- Nástroj protokolování chyba zapisuje informace o chybě v souborů XML do sdílené složky. Máte a ujistěte se, že účet, který technologie ASP.NET spuštěna pod v nasazené lokality má oprávnění k zápisu do této složky a musíte tuto složku vyloučit z nasazení. (, Jinak chyba data protokolu z testovacího prostředí je možno nasadit do produkčního prostředí nebo soubory protokolů chyba produkční může odstranit.)
- Aplikace zahrnuje některá nastavení, které je potřeba změnit v nasazené *Web.config* souboru v závislosti na cílovém prostředí (testu nebo produkční) a další nastavení, které je potřeba změnit v závislosti na sestavení konfigurace (ladění nebo verze).
- Řešení nástroje Visual Studio obsahuje projektu knihovny tříd. Pouze sestavení, které generuje tento projekt by měl být nasazen, nikoli projekt.

V tomto prvním kurzu v řadě byly staženy ukázkový projekt sady Visual Studio a zkontrolovat lokality funkce, které ovlivňují nasazení aplikace. V následujících kurzech připravíte nasazení nastavením některé z těchto věcí automaticky zpracovávat. Ostatní můžete postará o ručně.

> [!div class="step-by-step"]
> [Next](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
