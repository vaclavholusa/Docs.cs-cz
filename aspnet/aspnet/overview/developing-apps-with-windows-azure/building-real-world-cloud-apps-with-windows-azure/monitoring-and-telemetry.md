---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
title: Monitorování a Telemetrie (vytváření reálných cloudových aplikací s Azure) | Microsoft Docs
author: MikeWasson
description: Cloudové aplikace skutečné World sestavení s Azure elektronická kniha je založena na prezentace vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které můžete mu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2015
ms.topic: article
ms.assetid: 7e986ab5-6615-4638-add7-4614ce7b51db
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
msc.type: authoredcontent
ms.openlocfilehash: d58c495b3888c146a2a9bc831865cf7cc0d94c7b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877356"
---
<a name="monitoring-and-telemetry-building-real-world-cloud-apps-with-azure"></a>Monitorování a Telemetrie (vytváření reálných cloudových aplikací s Azure)
====================
podle [Karel Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [tní Dykstra](https://github.com/tdykstra)

[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronická kniha](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Vytváření reálného světa cloudových aplikací s Azure** elektronická kniha je založena na prezentaci vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které vám mohou pomoci být úspěšná, vývoj webových aplikací pro cloud. Informace o elektronická kniha najdete v tématu [první kapitoly](introduction.md).


Velký počet uživatelů se spoléhají na zákazníky a dát jim vědět, pokud je jejich aplikace dolů. Který není skutečně osvědčeným postupem odkudkoli a hlavně není v cloudu. Neexistuje žádná záruka rychlé oznámení, a když se zobrazí se upozornění, zobrazí často minimální nebo zavádějící data, o co se stalo. S funkčním telemetrie a protokolování systémy, které budete vědět, co se děje s vaší aplikací a pokud něco udělat chybu zjistit hned a mít užitečné informace o odstraňování potíží pro práci s.

## <a name="buy-or-rent-a-telemetry-solution"></a>Koupit nebo pronajímat řešení telemetrie

> [!NOTE]
> Tento článek byla zapsána před [Application Insights](https://azure.microsoft.com/services/application-insights/) byl vydán. Application Insights je žádoucí telemetrie řešení v Azure. V tématu [nastavte Application Insights pro váš web ASP.NET](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net) Další informace.


Jednou z věcí, které je skvělým o cloudovém prostředí je, že je opravdu snadné koupit nebo pronajímat moct vítězství. Telemetrie je příklad. Bez mnoho úsilí můžete získat dobře telemetrie systému aktivní a běží, velmi cenově výhodnou. Existují ve svazku skvělé partnery, které se integrují s Azure a některá z nich, kde získáte základní telemetrie pro nic žádné volné vrstvy –. Zde jsou uvedeno několik těm, které jsou aktuálně k dispozici v Azure:

- [New Relic.](http://newrelic.com/)
- [AppDynamics](http://www.appdynamics.com/)
- [Dynatrace](https://datamarket.azure.com/application/b4011de2-1212-4375-9211-e882766121ff)

Od verze března 2015 [Microsoft Application Insights pro Visual Studio Online](https://azure.microsoft.com/documentation/articles/app-insights-get-started/) se ještě neuvolní, ale je k dispozici ve verzi preview pro vyzkoušení. [Microsoft System Center](http://www.petri.co.il/microsoft-system-center-introduction.htm#) také zahrnuje monitorovací funkce.

Rychle projdeme nastavení zobrazit, jak snadno může být použít systém telemetrie New Relic.

Na portálu Azure management portal zaregistrujte se pro danou službu. Klikněte na tlačítko **nový**a potom klikněte na **úložiště**. **Zvolte doplněk** zobrazí se dialogové okno. Posuňte se dolů a klikněte na tlačítko **New Relic**.

![Vyberte doplněk](monitoring-and-telemetry/_static/image1.png)

Klikněte na šipku vpravo a zvolte úroveň služby, které chcete. V této ukázce používáme úroveň free.

![Přizpůsobení rozšíření](monitoring-and-telemetry/_static/image2.png)

Klikněte na šipku vpravo, potvrďte "nákupu" a New Relic se zobrazí jako doplněk na portálu.

![Zkontrolujte nákupu](monitoring-and-telemetry/_static/image3.png)

![Nové rozšíření New Relic na portálu pro správu](monitoring-and-telemetry/_static/image4.png)

Klikněte na tlačítko **informace o připojení**a zkopírujte licenční klíč.

![Informace o připojení](monitoring-and-telemetry/_static/image5.png)

Přejděte na **konfigurace** kartě pro webové aplikace na portálu, nastavte **monitorování výkonu** k **rozšíření**a nastavte **zvolte rozšíření** rozevíracím seznamu **New Relic**. Pak klikněte na tlačítko **Uložit**.

![Na kartě Konfigurace New Relic.](monitoring-and-telemetry/_static/image6.png)

V sadě Visual Studio nainstalujte balíček NuGet nové Relic ve vaší aplikaci.

![Developer analytics na kartě Konfigurace](monitoring-and-telemetry/_static/image7.png)

Nasazení aplikace do Azure a jej začít používat. Vytvořte několik úloh zajistit některé aktivity pro New Relic ke sledování do opravte ji.

Poté přejděte zpět na **New Relic** stránky v **doplňky** portál a klikněte na kartu **spravovat**. Na portálu odešle New Relic portálu pro správu pomocí jednotného přihlašování pro ověřování, nemusíte znovu zadejte své přihlašovací údaje. Stránce Přehled poskytuje množství různých statistiku výkonu. (Kliknutím na obrázek v plné velikosti stránky Přehled tématu.)

[![Novou kartu Relic monitorování](monitoring-and-telemetry/_static/image9.png)](monitoring-and-telemetry/_static/image8.png)

Zde je uvedeno několik statistických údajů, které se zobrazí:

- Průměrná doba odezvy v různou denní dobu.

    ![Doba odezvy](monitoring-and-telemetry/_static/image10.png)
- Propustnosti (v požadavky za minutu) v různou denní dobu.

    ![Propustnost](monitoring-and-telemetry/_static/image11.png)
- Doba využití procesoru serveru stráví zpracování různých žádostí HTTP.

    ![Časy webové transakce](monitoring-and-telemetry/_static/image12.png)
- Čas procesoru v různých částech kód aplikace:

    ![Podrobnosti o trasování](monitoring-and-telemetry/_static/image13.png)
- Historie výkonu statistiky.

    ![Historie výkonu](monitoring-and-telemetry/_static/image14.png)
- Volání externí služby, jako je například služby objektů Blob a statistické údaje o tom, jak spolehlivé a reaguje byl službu.

    ![Externí služby](monitoring-and-telemetry/_static/image15.png)

    ![Externí služby](monitoring-and-telemetry/_static/image16.png)

    ![Externí služby](monitoring-and-telemetry/_static/image17.png)
- Informace o tom, kde v celém světě nebo kde ve webové aplikaci USA provoz pochází.

    ![Geography](monitoring-and-telemetry/_static/image18.png)

Můžete také nastavit sestavy a události. Můžete například můžete kdykoli spustit zobrazuje chyby, odeslat e-mail pracovníci podpory v upozornění na problém.

![Sestavy](monitoring-and-telemetry/_static/image19.png)

New Relic je pouze jeden z příkladů telemetrie systému; všechny tyto můžete získat z jiných služeb také. Výhodou cloudu je, že bez nutnosti psaní jakéhokoli kódu a pro minimální nebo žádný výdajů najednou můžete získat mnoho další informace o tom, jak vaše aplikace používá a co vaši zákazníci ve skutečnosti dochází.

<a id="log"></a>
## <a name="log-for-insight"></a>Přehled protokolu

Telemetrická data balíčku je dobré první krok, ale máte k instrumentaci vlastní kód. Službu telemetrie zjistíte při došlo k potížím a říká co dochází zákazníků, ale jeho nemusí poskytnout spoustu aspekty co se děje ve vašem kódu.

Nechcete mít na vzdálené na provozním serveru zobrazíte činnosti vaší aplikace. Který může být praktické, máte k dispozici jeden server, ale co v případě, že jste škálovat na stovky serverů a nevíte, ty, které je nutné do vzdáleného? Vaše protokolování by měl poskytovat dostatek informací, která není nutné vzdáleného do provozních serverů k analýze a ladění problémů. Musí být protokolování dostatek informací, aby můžete izolovat problémy výhradně prostřednictvím protokolů.

### <a name="log-in-production"></a>Přihlaste se v produkčním prostředí

Velké množství uživatelů zapnout trasování v provozním prostředí jenom v případě, že došlo k potížím a chtějí mít k ladění. To může způsobit významné zpoždění mezi dobu, kterou jste si vědomi na problém a čas, kdy získat užitečné informace o řešení potíží o něm. A informace, které získáte nemusí být užitečné při občasné chyby.

Doporučujeme v cloudovém prostředí, kde je levných úložiště je vždycky ponechte přihlašování v produkčním prostředí. Tímto způsobem při chyby dojít je přihlášení už máte a máte historických dat, který vám pomůže analyzovat problémy, které vyvíjet v čase nebo stát pravidelně v různých časech. Může automatizovat proces vyprázdnění odstranění starých protokolů, ale můžete zjistit, že se jedná o nákladnější nastavit tento proces, než je udržovat v protokolech.

Vytvářet protokolování je jednoduchá ve srovnání s množství řešení potíží s čas a peníze, můžete uložit tak, že jsou všechny informace, které budete potřebovat již k dispozici, když dojde k chybě. Pak když někdo zjistíte měly náhodné chyby kolem 8:00 poslední noci, ale jejich nepamatujete chyba, můžete snadno zjistit co byl problém.

Pro menší než 4 $ měsíc, které můžete ponechat 50 GB protokoly na straně a dopad na výkon protokolování je jednoduchá tak dlouho, dokud zachovat jednou z věcí v paměti – aby bylo možné vyhnout se kritickým bodům výkon, ujistěte se, že vaše knihovna protokolování je asynchronní.

### <a name="differentiate-logs-that-inform-from-logs-that-require-action"></a>Rozlišit protokoly, které obsahují informace z protokolů, které vyžadují akci

Protokoly jsou určené INFORMUJTE (chci, aby vám vědět něco) nebo ACT (chci musíte udělat něco). Dávejte pozor, pouze zápis ACT protokolů pro problémy, které skutečně vyžadují osoby nebo automatizovaného procesu provést akci. Příliš mnoho ACT protokoly se vytvoří šumu, vyžadování příliš mnoho práce při třídění přes něj všechny najít originální problémy. A pokud protokolů ACT automaticky spouštět některé akce, například odeslání e-mailu pro pracovníky odborné pomoci, vyhněte se umožníte tisíce takové akce aktivovat jeden problém.

V rozhraní .NET System.Diagnostics trasování, můžete protokoly přiřadit chyby, upozornění, informace a ladění nebo podrobné úrovni. AKCE může odlišovat z protokolů INFORMUJTE rezervování úroveň chyb pro akce protokoly a použitím nižších úrovních INFORMUJTE protokolů.

![Úrovně protokolování](monitoring-and-telemetry/_static/image20.png)

### <a name="configure-logging-levels-at-run-time"></a>Konfigurace úrovně protokolování v době běhu

I když je smysl mít vždy protokolování v produkčním prostředí, jiné osvědčeným postupem je implementace rozhraní protokolování, které lze upravit úroveň podrobností, které jste protokolování, bez opětovného nasazení nebo restartování aplikace za běhu. Například když použijete nástroj trasování v `System.Diagnostics` můžete vytvořit chyby, upozornění, informace a ladění nebo podrobné protokoly. Doporučujeme vždy protokolu chyba, upozornění, informace o protokoly v produkčním prostředí a budete chtít dynamicky přidávat ladění nebo podrobné protokolování pro řešení problémů na případ od případu.

Webové aplikace v Azure App Service musí mít integrovanou podporu pro zápis `System.Diagnostics` protokoly na systém souborů, úložiště Table nebo úložiště objektů Blob. Můžete vybrat úrovně různých protokolování pro jednotlivé cíle úložiště, a můžete změnit úroveň protokolování za chodu bez restartování aplikace. Úložiště objektů Blob s podporou usnadňuje spustit [HDInsight](https://docs.microsoft.com/azure/hdinsight/) analysis úlohy na vaše protokoly aplikací, protože HDInsight umí pracovat přímo s úložištěm Blob.

### <a name="log-exceptions"></a>Výjimky protokolu

Právě, umístěte *výjimka. ToString()* ve vašem kódu protokolování. Která zůstane kontextové informace. V případě chyby SQL zůstanou na číslo chyby SQL. Všechny výjimky zahrnují informace o kontextu, výjimky, samotné a vnitřním výjimkám a ujistěte se, že zadáváte všechno, které budou potřebovat pro řešení potíží. Například informace o kontextu může zahrnovat název serveru, identifikátor transakce a uživatelské jméno (ale ne heslo nebo všech tajných klíčů!).

Pokud byste tedy spoléhat na každý vývojář udělat správné věci s výjimkou protokolování, nebudou některé z nich. Chcete-li zajistit, aby ho získá právo způsobem pokaždé, když, sestavení zpracování přímo do vašeho rozhraní protokolovače výjimek: předat objekt výjimky k třídě protokolovacího nástroje a protokolovat data výjimky správně ve třídě protokolovacího nástroje.

### <a name="log-calls-to-services"></a>Protokol volání služby

Důrazně doporučujeme, že napíšete protokolu pokaždé, když vaše aplikace volá ke službě, zda je do databáze nebo rozhraní REST API nebo žádné externí služby. Zahrnout do protokolů nejen údaj úspěch nebo selhání ale jak dlouho trvalo každý požadavek. V cloudovém prostředí se často zobrazí problémy související s slow-downs spíše než dokončení výpadků. Něco, co obvykle trvá 10 milisekund. najednou může spustit, s ohledem na druhou. Když někdo sdělí, že aplikace je pomalá, chcete být schopni prohlížet New Relic nebo ať telemetrie služby máte a ověřit jejich prostředí a pak chtějí mít možnost vypadat jsou vlastní protokoly můžete začít na podrobné informace o důvody, proč je pomalé.

### <a name="use-an-ilogger-interface"></a>Pomocí objektu ILogger rozhraní

Co doporučujeme, abyste ve chvíli, kdy vytvořit produkční aplikace je vytvořit jednoduchou *objektu ILogger* rozhraní a přilepit některé metody v ní. To usnadňuje implementace protokolování později změnit a nemusí být absolvovat celý kód to udělat. Může být používáme `System.Diagnostics.Trace` třídy v celé aplikaci opravit, ale místo toho jsme se používá v pozadí v třídě protokolování, který implementuje *objektu ILogger*, a ujistěte se, jsme *objektu ILogger* metoda volá v průběhu aplikace.

Tímto způsobem, pokud někdy budete chtít zkontrolujte vaše protokolování bohatší, můžete nahradit [ `System.Diagnostics.Trace` ](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio#apptracelogs) s jakýmkoli protokolování chcete. Například s růstem aplikace můžete se rozhodnout, kterou chcete použít komplexnější balíček protokolování, jako [NLog](http://nlog-project.org/) nebo [blokem protokolování aplikace aplikace Enterprise knihovny](https://msdn.microsoft.com/library/dn440731(v=pandp.60).aspx). ([Log4Net](http://logging.apache.org/log4net/) je jiný oblíbených protokolování framework, ale neprovádí asynchronní protokolování.)

Jedním z možných důvodů pro používání rozhraní například NLog je usnadnit dělení protokolování výstup do samostatné vysoký počet a citlivých dat úložiště. Který umožňuje efektivně ukládat velké objemy dat INFORMUJTE, který nemusíte spouštět rychlé dotazy na, při zachování rychlý přístup k datům Application Compatibility Toolkit.

### <a name="semantic-logging"></a>Sémantické protokolování

Relativně nové způsob, jak provést protokolování, jejichž výsledkem mohou být užitečnější diagnostické informace, najdete v části [Enterprise knihovny sémantické protokolování aplikace blok (SLAB)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Používá SLAB [trasování událostí pro Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) a [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) podpora v rozhraní .NET 4.5 na vám umožní vytvořit další protokoly strukturovaných a dotazovatelnosti. Můžete definovat jinou metodu pro každý typ událost, která přihlášení, která umožňuje přizpůsobit informace, které můžete psát. Například protokolu databáze SQL chyba, může zavolat `LogSQLDatabaseError` metoda. Pro tento typ výjimky víte, že je klíčovou součástí informace je číslo chyby tak, aby může zahrnout číslo parametru chyba podpis metody a poznamenejte si číslo chyby jako samostatné pole v záznamu protokolu, kterou píšete. Protože číslo je do samostatných polí můžete snadno a spolehlivě získat sestavy založené na čísla chyby SQL, než může Pokud číslo chyby byly právě zřetězení do řetězec zprávy.

## <a name="logging-in-the-fix-it-app"></a>Protokolování v opravě aplikace

### <a name="the-ilogger-interface"></a>Rozhraní objektu ILogger

Tady je *objektu ILogger* rozhraní v aplikaci opravit.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample1.cs)]

Tyto metody umožňují zápis protokolů na stejné úrovni čtyři nepodporuje *System.Diagnostics*. Metody TraceApi jsou pro protokolování volání externí služby s informacemi o latence. Můžete také přidat sadu metody pro ladění nebo podrobné úrovni.

### <a name="the-logger-implementation-of-the-ilogger-interface"></a>Implementace rozhraní objektu ILogger protokolovacího nástroje

Implementace rozhraní je skutečně jednoduché. V podstatě právě zavolá do standardní *System.Diagnostics* metody. Následující fragment kódu ukazuje všechny tři metody informace a jeden z jiné.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample2.cs)]

### <a name="calling-the-ilogger-methods"></a>Volání metody objektu ILogger

Pokaždé, když kód v aplikaci opravte zachytí výjimku, zavolá *objektu ILogger* metoda do protokolu podrobnosti o výjimce. A pokaždé, když se provede volání databáze, služby objektů Blob nebo rozhraní REST API, spustí stopky před voláním, zastaví stopky, když služba vrátí a protokoly uplynulý čas společně s informacemi o úspěchu nebo neúspěchu.

Všimněte si, že zprávy protokolu obsahuje název třídy a název metody. Je vhodné zajistit, že zprávy protokolu určit, jaká část kódu aplikace jejich zápisu.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample3.cs?highlight=6,14,20-21,25)]

Proto nyní pro každý době, kdy aplikace opravte udělal volání databáze SQL uvidíte volání metody, která volá, a přesně kolik času ho trvalo.

![Dotaz SQL Database v protokolech](monitoring-and-telemetry/_static/image21.png)

![](monitoring-and-telemetry/_static/image22.png)

Pokud přejděte projdete protokoly, uvidíte, že doba trvat volání databáze je proměnná. Informace může být užitečné: protože protokolů aplikace, to vše můžete analyzovat historických trendů v tom, jak služba databáze provádí v čase. Pro instanci služby může být rychlé ve většině případů, ale požadavky může selhat nebo může zpomalit odpovědi v určité denní dobu.

Můžete to samé služby objektů Blob – pokaždé, když aplikace se uloží nový soubor, je do protokolu a uvidíte, přesně jak dlouho trvalo kvůli nahrání každého souboru.

![Objekt BLOB nahrání protokolu](monitoring-and-telemetry/_static/image23.png)

Jsou to jenom několik další řádky kódu k zápisu při každém volání služby, a teď vždy, když někdo informacemi o tom, že jejich narazili problém, víte, přesně jak problém se, zda byla chybu nebo i v případě, že byl právě běží pomalu. Můžete nalézt zdroj problému bez nutnosti vzdáleného do serveru nebo zapnout protokolování poté, co se stane, chyba a naděje znovu vytvořit.

## <a name="dependency-injection-in-the-fix-it-app"></a>Vkládání závislostí v opravu jeho aplikace

Může vás zajímat, jak konstruktor úložiště pro výše uvedený příklad získá protokolovač implementace rozhraní:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample4.cs?highlight=6)]

Propojit se rozhraní pro implementaci aplikace používá [vkládání závislostí](http://en.wikipedia.org/wiki/Dependency_injection)(DI) s [AutoFac](http://autofac.org/). DI umožňuje použít objekt založené na rozhraní na mnoha místech v kódu a nutné uvést na jednom místě, implementace, který se používá při vytváření instance rozhraní. To usnadňuje změňte implementaci: například můžete chtít nahradit protokolovač NLog System.Diagnostics protokolovacího nástroje. Nebo pro automatizované testování můžete chtít nahradit verzi mock protokolovacího nástroje.

Aplikace opravte ji používá DI ve všech úložišť a všech řadičů. Získat konstruktory třídy kontroleru *ITaskRepository* rozhraní stejným způsobem jako úložiště získá protokolovač rozhraní:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample5.cs?highlight=5)]

Aplikace používá knihovnu AutoFac DI automaticky poskytnout *TaskRepository* a *Protokolovač* instancí těchto konstruktory.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample6.cs?highlight=8,10)]

Tento kód v podstatě říká, že kamkoli konstruktor potřebuje *objektu ILogger* rozhraní, předejte v instanci systému *protokolovacího nástroje* třída, a vždy, když je nutné *IFixItTaskRepository*rozhraní, předejte v instanci systému *FixItTaskRepository* třídy.

[AutoFac](http://autofac.org/) je jedním z mnoha architektur vkládání závislostí, které můžete použít. Jiné oblíbených z nich je [Unity](https://blogs.msdn.com/b/agile/archive/2013/08/20/new-guide-dependency-injection-with-unity.aspx), který je doporučeno a podporuje Microsoft Patterns and Practices.

## <a name="built-in-logging-support-in-azure"></a>Podpora integrované protokolování v Azure

Azure podporuje následující typy z [protokolování pro Web Apps v Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio):

- System.Diagnostics trasování (můžete zapnout a vypnout a nastavit úrovně za chodu bez restartování webu).
- Windows Events.
- Protokoly služby IIS (HTTP/FREB).

Azure podporuje následující typy z [protokolování v cloudové služby](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics):

- System.Diagnostics trasování.
- Čítače výkonu.
- Windows Events.
- Protokoly služby IIS (HTTP/FREB).
- Vlastní directory monitorování.

Aplikace opravte ji používá System.Diagnostics trasování. Všechny, které je třeba provést pro povolení protokolování ve webové aplikaci System.Diagnostics je překlopit přepínač na portálu nebo volání rozhraní REST API. Na portálu, klikněte na tlačítko **konfigurace** kartě pro vaši lokalitu a přejděte dolů a v tématu **rozhraní Application Diagnostics** části. Můžete zapnout protokolování nebo vypnout a vyberte požadovanou úroveň protokolování. Můžete mít zápisu protokoly do systému souborů nebo k účtu úložiště Azure.

![Diagnostiky aplikací a Diagnostika lokality na kartě Konfigurace](monitoring-and-telemetry/_static/image24.png)

Po povolení protokolování v Azure, uvidíte protokolů v okně výstupu Visual Studio, při jejich vytvoření.

![Streamování protokoly nabídky](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-viewlogsmenu.png)

![Streamování protokoly nabídky](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-nologsyet.png)

Můžete si taky protokoly zapisují do účtu úložiště a zobrazení je veškeré nástroje, které mají přístup ke službě Azure Storage Table, jako **Průzkumníka serveru** v sadě Visual Studio nebo [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/).

![Protokoly v Průzkumníku serveru](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-storagelogs.png)

## <a name="summary"></a>Souhrn

Je opravdu snadné implementaci systému se na pole telemetrie, instrumentace protokolování v kódu a nakonfigurovat protokolování v Azure. Pokud máte problémy s provozním, kombinace systému telemetrie a vlastní protokoly vám a předtím, než vstoupí významných problémech pro vaše zákazníky rychle vyřešit problémy.

V [další kapitoly](transient-fault-handling.md) podíváme postup zpracovat přechodné chyby, takže budou nemáte provozní problémy, které budete muset prozkoumat.

## <a name="resources"></a>Prostředky

Další informace najdete v následujících zdrojích informací.

Dokumentace hlavně o telemetrická data:

- [Microsoft Patterns and Practices - Azure pokyny](https://msdn.microsoft.com/library/dn568099.aspx). Zobrazit instrumentace a Telemetrie pokyny, pokyny pro monitorování míry využívání služby, vzor monitorování stavu koncového bodu a Runtime Rekonfigurace vzor.
- [Hodnotu ve formátu Měna roztáhnout v cloudu: povolení na weby Azure monitorování výkonu aplikací New Relic](http://www.hanselman.com/blog/PennyPinchingInTheCloudEnablingNewRelicPerformanceMonitoringOnWindowsAzureWebsites.aspx).
- [Osvědčené postupy pro návrh rozsáhlých služeb v cloudu Azure služeb](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Dokument White paper moduly SIMM značky a Michael Thomassy. Najdete v části Telemetrie a Diagnostika.
- [Vývoj generace s Application Insights](https://msdn.microsoft.com/magazine/dn683794.aspx). Článek v časopise MSDN.

Dokumentace hlavně o protokolování:

- [Blokování aplikací sémantické protokolování (SLAB)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Neil Mackenzie uvede případ sémantické protokolování s SLAB.
- [Vytváření strukturovaných a smysluplný protokoly s sémantické protokolování](https://channel9.msdn.com/Events/Build/2013/3-336). (Video) Juliánský Dominguez uvede případ sémantické protokolování s SLAB.
- [EF6 Protokolování SQL – část 1: jednoduchý protokolování](http://blog.oneunicorn.com/2013/05/08/ef6-sql-logging-part-1-simple-logging/). Podle Arthur Vickerse ukazuje, jak se přihlásit dotazy provedený Entity Framework v EF 6.
- [Odolnost připojení a zachycením příkaz s použitím Entity Framework v aplikaci ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Čtvrtý v kurzu řadě devět částí, ukazuje, jak používat funkci zachycení příkaz EF 6 do protokolu příkazy SQL odeslal do databáze pomocí rozhraní Entity Framework.
- [Zlepšení protokolování pomocí jazyka C# 5.0 volající informace atributů](http://www.dotnetcurry.com/showarticle.aspx?ID=972). Postup jednoduše zaznamenávat název volání metody bez nezakódovávejte ho do literály nebo pomocí reflexe se dá stáhnout ručně.

Dokumentace hlavně o řešení problémů:

- [Poradce při potížích s Azure &amp; ladění blog](https://blogs.msdn.com/b/kwill/).
- [AzureTools – diagnostiky nástroj používaný tým podpory Azure vývojáře](https://blogs.msdn.com/b/kwill/archive/2013/08/26/azuretools-the-diagnostic-utility-used-by-the-windows-azure-developer-support-team.aspx?Redirected=true). Uvádí a poskytuje odkaz ke stažení pro nástroj, který lze použít na virtuální počítač Azure ke stažení a spuštění celou řadu nástrojů pro diagnostiku a monitorování. Je užitečné, když potřebujete diagnostikovat problém pro konkrétní virtuální počítač.
- [Řešení potíží s webovou aplikaci v Azure App Service pomocí sady Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Podrobný návod k Začínáme s System.Diagnostics trasování a vzdálené ladění.

Videa:

- [Bezporuchový: Vytváření škálovatelné, odolné cloudové služby](https://channel9.msdn.com/Series/FailSafe). Řada devět částí Ulrich Homann, Mercuri matolin a moduly SIMM značky. Uvede základními koncepty a architektury zásady způsobem, velmi přístupné a zajímavé, s scénářů čerpají z prostředí Microsoft zákazníka poradní tým (CAT) s skutečným zákazníkům. Díly 4 a 9 se o monitorování a telemetrie. Díl 9 obsahuje přehled monitorování služeb MetricsHub, AppDynamics, New Relic a PagerDuty.
- [Vytváření Big: Poučení vyplývající z Azure zákazníků – část II](https://channel9.msdn.com/Events/Build/2012/3-030). Moduly SIMM označit zmíněn návrhu pro selhání a instrumentace vše. Podobně jako u řady bezporuchový ale přejde do další postupy podrobnosti.

Ukázka kódu:

- [Základy služby ve službě Azure Cloud](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Ukázková aplikace vytvořené Azure poradní tým Microsoftu. Demonstruje telemetrie a protokolování postupy, jak je popsáno v následujících článcích. Ukázka implementuje protokolování aplikací pomocí [NLog](http://nlog-project.org/). Související dokumentace, najdete v článku [řadu čtyři články na webu TechNet wiki o telemetrie a protokolování](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx#Telemetry_coming_soon).

> [!div class="step-by-step"]
> [Předchozí](design-to-survive-failures.md)
> [další](transient-fault-handling.md)
