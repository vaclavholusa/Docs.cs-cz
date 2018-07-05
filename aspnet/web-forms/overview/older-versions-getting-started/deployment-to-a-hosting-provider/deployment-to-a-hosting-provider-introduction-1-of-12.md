---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
title: 'Nasazení webové aplikace ASP.NET s SQL serverem Compact pomocí sady Visual Studio: Úvod - 1 z 12 | Dokumentace Microsoftu'
author: tdykstra
description: Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET, která obsahuje databázi systému SQL Server Compact pomocí Visual samostatného projektu webové aplikace...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a2d7f33b-8c4a-4b48-9fb1-9139cf9b9878
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
msc.type: authoredcontent
ms.openlocfilehash: 4a790fa72568caafdb2fab5efd9f334919c23719
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398268"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-introduction---1-of-12"></a>Nasazení webové aplikace ASP.NET s SQL serverem Compact pomocí sady Visual Studio: Úvod - 1 z 12
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout počáteční projekt](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí sady Visual Studio 2012 RC nebo Visual Studio Express 2012 RC pro Web. Můžete také použít Visual Studio 2010 při instalaci aktualizace Publikovat Web.
> 
> Kurz ukazuje nasazení funkce zavedená po verzi RC sady Visual Studio 2012, ukazuje, jak nasadit edicích systému SQL Server než SQL Server Compact a ukazuje, jak nasadit do Azure App Service Web Apps, najdete v tématu [nasazení webu ASP.NET pomocí sady Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).
> 
> Tyto kurzy vás provede nasazením nejprve do služby IIS na místním počítači pro vývoj pro testování a k poskytovateli hostingu třetích stran. Aplikace, které budete nasazovat používá aplikační databáze a členské databáze ASP.NET. Začněte používat SQL Server Compact a nasazení do systému SQL Server Compact a novější kurzy vám ukážou, jak nasazení změn databází a migrace na SQL Server.
> 
> V kurzech předpokládají, že víte, jak pracovat s ASP.NET v sadě Visual Studio. Pokud to neuděláte, je vhodné oddělení na zahájení [základní technologie ASP.NET webové formuláře kurzu](../tailspin-spyworks/tailspin-spyworks-part-1.md) nebo [základní kurz ASP.NET MVC](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md).
> 
> Pokud máte nějaké otázky, které přímo nesouvisejí, najdete v tomto kurzu, můžete je publikovat [fórum ASP.NET nasazení](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment).


## <a name="overview"></a>Přehled

Tyto kurzy vás provede nasazením nejprve do služby IIS na místním počítači pro vývoj pro testování a k poskytovateli hostingu třetích stran. Aplikace, které budete nasazovat používá aplikační databáze a členské databáze ASP.NET. Začněte používat SQL Server Compact a nasazení do systému SQL Server Compact a novější kurzy vám ukážou, jak nasazení změn databází a migrace na SQL Server.

Číslo kurzů – 11 ve všech plus stránku pro řešení potíží – by mohlo způsobit nepoužitelnost vypadá to, že složitý proces nasazení. Ve skutečnosti ale základní postupy pro nasazení webu tvoří relativně malou část této sérii kurzů. Nicméně v reálných situacích, často potřebujete informace o některé další aspekty malou, avšak důležitou nasazení – třeba nastavení oprávnění pro složky na cílovém serveru. Mnohé z těchto dalších technik jsme zahrnuli v kurzech se naději, že v kurzech nenechávejte si informace, které mohou bránit úspěšné nasazení aplikace skutečný.

V kurzech jsou navrženy pro spouštění v pořadí a každá část vychází z předchozí části. Však můžete přeskočit části, které nejsou relevantní pro konkrétní situaci. (Vynechání části může vyžadovat upravit postupy v budoucích kurzech.)

## <a name="intended-audience"></a>Cílová skupina

V kurzech jsou zaměřené na vývojáře využívající technologii ASP.NET, kteří pracují v malých organizacích nebo jiných prostředích kde:

- Proces průběžné integrace (automatizované sestavování a nasazování) se nepoužívá.
- Produkčním prostředí je poskytovatele hostitelských služeb třetích stran.
- Jedna osoba obvykle vyplní více rolí (stejné osobě vyvíjí, testy a nasadí).

V podnikovém prostředí je obvyklejší k implementaci nepřetržité integrace procesů a produkčním prostředí je obvykle hostované servery společnosti. Různí lidé také obvykle provádí různé role. Informace o nasazení v podniku najdete v tématu [nasazení webové aplikace v podnikových scénářích](../../deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).

Organizace všech velikostí můžete taky nasadit webové aplikace do Azure a většina postupy uvedené v následujících kurzech platí také pro Azure App Services Web Apps. Úvod do Azure, najdete v článku [ https://azure.microsoft.com ](https://azure.microsoft.com).

## <a name="the-hosting-provider-shown-in-the-tutorials"></a>Poskytovatel hostingu je znázorněno v kurzech

Kurzy vás provedou procesem vytvoření účtu s hostingové společnosti a nasazení aplikace do tohoto poskytovatele hostitelských služeb. Konkrétní hostingové společnosti byla vybrána tak, aby v kurzech může ukazuje kompletní prostředí nasazení na živý web. Každý hostingové společnosti poskytuje různé funkce a prostředí nasazení na servery se liší, trochu; proces popsaný v tomto kurzu ale typické pro celý proces.

Poskytovatel hostingu použitou v tomto kurzu Cytanium.com, je jedním z mnoha, které jsou k dispozici a jeho použití v tomto kurzu nepředstavuje k potvrzení nebo doporučení.

## <a name="deploying-web-site-projects"></a>Nasazení webových projektů

Contoso University je projekt webové aplikace Visual Studio. Většina metod nasazení a nástroje, které jsme vám ukázali v tomto kurzu se nevztahují na [webových projektů](https://msdn.microsoft.com/library/dd547590.aspx). Informace o tom, jak nasadit webových projektů, naleznete v tématu [mapa obsahu nasazení ASP.NET](https://msdn.microsoft.com/library/bb386521.aspx#deployment_for_web_site_projects).

## <a name="deploying-aspnet-mvc-projects"></a>Nasazení projekty ASP.NET MVC

Pro účely tohoto kurzu nasadíte projekt webových formulářů ASP.NET, ale všechno, co se dozvíte, jak provést se také vztahuje na ASP.NET MVC. Projekt Visual Studio MVC je stejně jiná forma projektu webové aplikace. Jediným rozdílem je, že pokud nasazujete k poskytovateli hostingu, který nepodporuje rozhraní ASP.NET MVC nebo ho vaši cílovou verzi, musíte, že jste nainstalovali odpovídající ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0) nebo [MVC 4](http://nuget.org/packages/aspnetmvc)) Balíčku NuGet ve vašem projektu.

## <a name="programming-language"></a>Programovací jazyk

Ukázková aplikace používá C#, ale v kurzech nevyžadují žádné znalosti jazyka C# a techniky nasazení zobrazené v kurzech nejsou specifické pro jazyk.

## <a name="troubleshooting-during-this-tutorial"></a>Řešení potíží během tohoto kurzu

Pokud dojde k chybě během nasazení nebo nasazené lokality nespustí správně, chybové zprávy není vždy poskytují řešení. Aby vám pomohl některé běžné scénáře problém [Poradce při potížích s referenční stránce](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md) je k dispozici. Pokud se zobrazí chybová zpráva nebo něco nefunguje tak, jak projděte následující kurzy, nezapomeňte se podívat na stránce řešení potíží.

## <a name="comments-welcome"></a>Vítejte komentáře

Komentáře na kurzy jsou úvodní a když dojde k aktualizaci kurzu veškeré úsilí, bude proveden rozdělení účet opravy nebo návrhy na vylepšení, která jsou k dispozici v kurzu komentáře.

## <a name="prerequisites"></a>Požadavky

Než začnete, ujistěte se, že máte Windows 7 nebo novější a v počítači nainstalována jedna z následujících produktů:

- [Visual Studio 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
- [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VWD2010SP1Pack)
- [Visual Studio 2012 RC nebo Visual Studio Express 2012 RC pro Web](https://go.microsoft.com/fwlink/?LinkId=240162)

Pokud máte Visual Studio 2010 SP1 nebo Visual Web Developer Express 2010 SP1, nainstalujte také následující produkty:

- [Azure SDK pro platformu .NET (VS 2010 SP1)](https://go.microsoft.com/fwlink/?LinkID=208120) (zahrnuje aktualizace Publikovat Web)
- [Nástroje Microsoft Visual Studio 2010 SP1 pro systém SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCEVSTools)

Některý software se vyžaduje k dokončení tohoto kurzu, ale není nutné mít, který dosud načteny. Tento kurz vás provede kroky pro instalaci, když je potřebujete.

## <a name="downloading-the-sample-application"></a>Stažení ukázkové aplikace

Aplikace, které budete nasazovat jmenuje Contoso University a už se vytvořila za vás. Je zjednodušenou verzi web university, volně založený na aplikaci Contoso University podle [kurzy Entity Frameworku na webu ASP.NET](https://asp.net/entity-framework/tutorials).

Pokud máte nainstalovány požadované součásti, stáhněte si [webové aplikace Contoso University](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b). *ZIP* soubor obsahuje více verzí tohoto projektu a souboru PDF, který obsahuje všechny kurzy 12. Pro seznámení se základními kroky tohoto kurzu, začněte s ContosoUniversity Begin. Pokud chcete zobrazit, co bude projekt vypadat na konci v kurzech, otevřete ContosoUniversity-End. Chcete-li zobrazit, co bude projekt vypadat před migrací na plnou instalaci systému SQL Server v kurzu 10, otevřete ContosoUniversity AfterTutorial09.

Příprava pro seznámení se základními kroků kurzu ContosoUniversity-začátek ukládání do libovolné složky použijete pro práci s projekty sady Visual Studio. Ve výchozím nastavení je to následující složka:

`C:\Users\<username>\Documents\Visual Studio 2012\Projects`

(Pro snímky obrazovky v tomto kurzu, složky projektu se nachází v kořenovém adresáři na `C`: jednotku.)

Spusťte sadu Visual Studio, otevřete projekt a stisknutím kláves CTRL-F5 spusťte ho.

[![Home_page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image2.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image1.png)

Stránky webu jsou přístupné z řádku nabídek a umožňují provádět následující funkce:

- Zobrazení statistiky student (stránku o).
- Zobrazit, upravit, odstranit a přidejte studenty.
- Zobrazení a úpravě kurzů.
- Zobrazení a úpravě Instruktoři.
- Zobrazení a úpravě oddělení.

Tady jsou snímky obrazovky několik reprezentativní stránek.

[![Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image4.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image3.png)

[![Add_Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image6.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image5.png)

## <a name="reviewing-application-features-that-affect-deployment"></a>Kontrola funkce aplikací, které ovlivňují nasazení

Následující funkce aplikaci vliv na způsob nasazení nebo je nutné udělat, abyste ho nasadit. Každá z těchto je vysvětleno podrobněji v následujících kurzech v řadě.

- Contoso University používá k ukládání dat aplikací, jako jsou jména studentů a kurzů vedených databázi systému SQL Server Compact. Databáze obsahuje kombinaci testovací data a provozními daty, a při nasazení do produkčního prostředí je třeba vyloučit testovací data. Později v této sérii kurzů provedeme migraci z SQL Server Compact do systému SQL Server.
- Aplikace používá systém členství technologie ASP.NET, která ukládá informace o uživatelském účtu v databázi SQL Server Compact. Aplikace definuje uživatel s oprávněním správce, který má přístup k určité omezené informace. Je třeba nasadit bez testovací účty, ale jeden účet správce databáze členství.
- Protože aplikační databáze a databáze členství použít SQL Server Compact jako databázový stroj, budete muset nasadit databázový stroj pro poskytovatele hostingu, jakož i samotných databázích.
- Aplikace používá zprostředkovatelů univerzální členství prostředí ASP.NET tak, aby systém členství můžete uložit svá data v databázi SQL Server Compact. Sestavení obsahující univerzální členství zprostředkovatele musí být nasazeny s aplikací.
- Aplikace používá pro přístup k datům v databázi aplikace Entity Framework 5.0. Sestavení, které obsahuje Entity Framework 5.0 musí být nasazeny s aplikací.
- Aplikace používá chybu třetích stran, protokolování a vytváření sestav nástroje. Tento nástroj je součástí sestavení, které musí být nasazeno společně s aplikací.
- Nástroj protokolování chyba zapisuje informace o chybě v souborech XML do složky souboru. Máte, abyste měli jistotu, že účet, který ASP.NET spuštěna v nasazené lokality má oprávnění k zápisu do této složky a budete muset tuto složku vyloučit z nasazení. (V opačném případě data protokolu chyby z testovacího prostředí je možno nasadit do produkčního prostředí a/nebo soubory protokolů produkční chyba mohla být odstraněna.)
- Aplikace obsahuje některá nastavení, které musí být změněny v nasazených *Web.config* soubor v závislosti na cílové prostředí (testovací nebo produkční) a další nastavení, která se musí změnit v závislosti na sestavení konfigurace (ladění nebo vydání).
- Řešení sady Visual Studio obsahuje projekt knihovny tříd. Pouze sestavení, která generuje tento projekt by měl být nasazen, ne projekt.

V tomto prvním kurzu v této sérii mají stáhnout ukázkový projekt sady Visual Studio a zkontrolovat funkce webů, které ovlivňují, jak nasadit aplikaci. V následujících kurzech se připravíte nasazení zařiďte si některé z těchto věcí automaticky zpracovávat. Ostatní vás postará o ručně.

> [!div class="step-by-step"]
> [Next](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
