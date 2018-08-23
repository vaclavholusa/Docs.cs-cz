---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
title: Monitorování a Telemetrie (vytváření skutečných cloudových aplikací s Azure) | Dokumentace Microsoftu
author: MikeWasson
description: Vytváření reálného světa cloudových aplikací s Azure e kniha je založená na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které se dají mu...
ms.author: riande
ms.date: 07/09/2015
ms.assetid: 7e986ab5-6615-4638-add7-4614ce7b51db
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
msc.type: authoredcontent
ms.openlocfilehash: cec29e27e834317661f80eed93cb8b81a0786644
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756880"
---
<a name="monitoring-and-telemetry-building-real-world-cloud-apps-with-azure"></a>Monitorování a Telemetrie (vytváření skutečných cloudových aplikací s Azure)
====================
podle [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Petr Dykstra](https://github.com/tdykstra)

[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronickou knihu](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Vytváření reálného světa cloudových aplikací s Azure** e knihy je založena na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které vám pomůžou být úspěšný vývoj webových aplikací v cloudu. Informace o e kniha najdete v tématu [první kapitoly](introduction.md).


Velké množství lidí Spolehněte se na zákazníky a dát jim vědět, že jejich aplikace je mimo provoz. Který není ve skutečnosti osvědčený postup kdekoli a zvlášť není v cloudu. Neexistuje žádná záruka rychlé oznámení, a Pokud dostanete, často získáte minimální nebo zavádějící data o co se stalo. Dobré systémy telemetrie a přihlašování, které lze vědět, co se děje s vaší aplikací a když se něco pokazí vám zjistíte to hned a budete mít užitečné informace pro práci s.

## <a name="buy-or-rent-a-telemetry-solution"></a>Koupit nebo poskytovat do nájmu řešení telemetrie

> [!NOTE]
> Tento článek byl zapsán před [Application Insights](https://azure.microsoft.com/services/application-insights/) byl uvolněn. Application Insights je řešení telemetrie upřednostňovaný přístup v Azure. Zobrazit [nastavení Application Insights pro váš web ASP.NET](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net) Další informace.


Jednou z věcí, to je skvělé o cloudovém prostředí je, že je velmi snadné hodit kupovat nebo poskytovat do nájmu moct vítězství. Telemetrie je příklad. Nevyžaduje spoustu úsilí můžete získat systém velmi se mi líbí telemetrie aktivní a spuštěné, nákladově velmi efektivní. Spousta skvělé partnerů, které se integrují s Azure a některé z nich mít bezplatných vrstev – takže získáte základní telemetrie pro žádnou akci. Tady je několik příkladů z těch, které jsou aktuálně dostupné v Azure:

- [Nástroj společnosti New Relic](http://newrelic.com/)
- [AppDynamics](http://www.appdynamics.com/)
- [Dynatrace](https://datamarket.azure.com/application/b4011de2-1212-4375-9211-e882766121ff)

Od března 2015 [Microsoft Application Insights pro Visual Studio Online](https://azure.microsoft.com/documentation/articles/app-insights-get-started/) neuvolní ještě ale je k dispozici ve verzi preview vyzkoušet. [Microsoft System Center](http://www.petri.co.il/microsoft-system-center-introduction.htm#) rovněž zahrnuje monitorovací funkce.

Rychle provedeme procesem nastavení špičkové funkce New Relicu pro ukazují, jak snadno může být systém telemetrie.

V portálu pro správu Azure zaregistrujte si službu. Klikněte na tlačítko **nový**a potom klikněte na tlačítko **Store**. **Vyberte doplněk** zobrazí se dialogové okno. Posuňte se dolů a klikněte na tlačítko **špičkové funkce New Relicu**.

![Vyberte doplněk.](monitoring-and-telemetry/_static/image1.png)

Klikněte na šipku vpravo a vyberte úroveň služby, které chcete. Pro tuto ukázku použijeme na úrovni free.

![Přizpůsobení doplněk](monitoring-and-telemetry/_static/image2.png)

Klikněte na šipku vpravo, potvrďte "koupit" a New Relic se teď zobrazí jako doplněk na portálu.

![Projděte si koupit](monitoring-and-telemetry/_static/image3.png)

![Nový doplněk Relic portálu pro správu](monitoring-and-telemetry/_static/image4.png)

Klikněte na tlačítko **informace o připojení**a zkopírujte licenční klíč.

![Informace o připojení](monitoring-and-telemetry/_static/image5.png)

Přejděte na **konfigurovat** kartu pro webové aplikace na portálu nastavit **Performance Monitoring pro aplikace** k **doplněk**a nastavte **vyberte doplněk** rozevírací seznam **špičkové funkce New Relicu**. Pak klikněte na tlačítko **Uložit**.

![Nástroj společnosti New Relic na kartě Konfigurace](monitoring-and-telemetry/_static/image6.png)

V sadě Visual Studio nainstalujte balíček NuGet nové Relic ve vaší aplikaci.

![Analýzy pro vývojáře na kartě Konfigurace](monitoring-and-telemetry/_static/image7.png)

Nasazení aplikace do Azure a začít ji používat. Vytvoření určitým postupem zajistit nějakou aktivitu pro špičkové funkce New Relicu pro monitorování do Fix It.

Pak přejděte zpátky do **špičkové funkce New Relicu** stránku **doplňky** portálu a klikněte na kartu **spravovat**. Na portálu odešle špičkové funkce New Relicu portálu pro správu pomocí jednotného přihlašování pro ověřování, takže není nutné znovu zadejte svoje přihlašovací údaje. Na stránce s přehledem nabízí širokou škálu statistiky výkonu. (Kliknutím na obrázek v plné velikosti stránky Přehled tématu.)

[![Nová karta Relic monitorování](monitoring-and-telemetry/_static/image9.png)](monitoring-and-telemetry/_static/image8.png)

Tady je několik příkladů statistických informací, které se zobrazí:

- Průměrná doba odezvy v různých časech den.

    ![Doba odezvy](monitoring-and-telemetry/_static/image10.png)
- Propustnosti (v žádostí za minutu) v různou dobu.

    ![Propustnost](monitoring-and-telemetry/_static/image11.png)
- Server Procesorový čas strávený zpracování různých požadavků HTTP.

    ![Časy transakcí webové](monitoring-and-telemetry/_static/image12.png)
- Procesorový čas strávený v různých částech kód aplikace:

    ![Podrobnosti o trasování](monitoring-and-telemetry/_static/image13.png)
- Historie výkonu statistiky.

    ![Historie výkonu](monitoring-and-telemetry/_static/image14.png)
- Volání s externími službami, jako je například služby Blob service a statistiky o spolehlivé a responzivní byla služba.

    ![Externí služby](monitoring-and-telemetry/_static/image15.png)

    ![Externí služby](monitoring-and-telemetry/_static/image16.png)

    ![Externí služby](monitoring-and-telemetry/_static/image17.png)
- Informace o tom, kde ve světě nebo ve webové aplikaci USA provoz, odkud.

    ![Zeměpisné oblasti](monitoring-and-telemetry/_static/image18.png)

Můžete také nastavit sestavy a události. Můžete například Řekněme, že kdykoli spustit chyby, odešlete e-mail pracovníkům podpora upozornění na problém.

![Sestavy](monitoring-and-telemetry/_static/image19.png)

New Relic je pouze jeden příklad systém telemetrie; To všechno můžete získat z jiných služeb také. Výhodou cloudu je, že bez nutnosti psát jakýkoli kód a pro minimální nebo žádné výdajů náhle můžete získat mnohem víc informace o využití vaší aplikace a co vaši zákazníci skutečně dochází k problému.

<a id="log"></a>
## <a name="log-for-insight"></a>Přehled protokolu

Telemetrie balíčku je dobrý první krok, ale stále musíte provádět instrumentaci váš vlastní kód. Telemetrické službě zjistíte když dojde k nějakému problému a zjistíte, co zákazníci dochází, ale to nemusí poskytnout velké množství přehled o tom, co se děje ve vašem kódu.

Nechcete mít vzdáleném připojení k provozní server, pokud chcete zobrazit, co vaše aplikace stojí. To může být praktické, když máte jeden server, ale co když jsme škálovat na stovky serverů a si nejste jisti, které potřebujete vzdáleně se přihlaste? Vaše přihlášení by měla poskytnout dostatek informací, které si už nikdy nemusíte vzdáleném připojení k produkční servery a analýzu a ladění problémů. By měl být protokolování dostatek informací, tak, aby můžete izolovat problémy výhradně prostřednictvím protokolů.

### <a name="log-in-production"></a>Protokolování v produkčním prostředí

Velké množství lidí zapnout trasování v produkčním prostředí pouze v případě, že dojde k nějakému problému a chtějí pro ladění. To může způsobovat významné zpoždění mezi čas, kdy budete vědět, problém a čas, který jste získali užitečné informace o odstraňování potíží o něm. A informace, které se zobrazí, nemusí být užitečná pro občasné chyby.

Doporučujeme v cloudovém prostředí, kde je levné úložiště je vždycky ponechte protokolování v produkčním prostředí. Tímto způsobem při chybách už je zaznamenána a máte historických dat, který může pomoct analyzovat problémy, které vývoj v průběhu času nebo dochází pravidelně v různých časech. Může automatizovat proces vymazání odstranit staré protokoly, ale je možné, že se jedná o dražší, nastavit odpovídající proces, než je protokoly.

Přidání výdajů protokolování je jednoduchá porovnání s množstvím problémů čas a peníze, můžete ušetřit tím, že všechny informace, které potřebujete již k dispozici, když dojde k chybě. Potom když vám někdo řekne měli k náhodné chybě kolem 8:00, poslední noc, ale jejich nepamatujete chybu, můžete snadno zjistit problém byl.

Pro méně než 4 USD za měsíc, které můžete ponechat 50 gigabajtů protokoly na straně a dopad na výkon protokolování je triviální tak dlouho, dokud je jednou z věcí v úvahu – Chcete-li zabránit problémových míst výkonu, ujistěte se, že je vaše knihovna protokolování asynchronní.

### <a name="differentiate-logs-that-inform-from-logs-that-require-action"></a>Protokoly, které informují o odlišil od protokolů, které vyžadují nějakou akci

Protokoly jsou určené k INFORM (chci, aby vám něco vědět) nebo ACT (chci, aby vám něco udělat). Buďte opatrní, pouze zápis ACT protokolů pro problémy, které skutečně vyžadují osoba nebo provedete akce vedoucí automatizovaného procesu. Příliš mnoho ACT protokoly se vytvoří šum, které vyžadují příliš mnoho práce probrat se všechno a najít problémy, pravý. A pokud vaše protokoly ACT automaticky aktivovat některé akce, jako je odeslání e-mailu na pracovníci podpory, vyhnout, umožníte tím tisíce tyto akce spouštět jen jeden problém.

V trasování .NET System.Diagnostics, protokoly je možné přiřadit úroveň chyby, upozornění, informace a ladění/Verbose. ACT můžete odlišil od protokolů INFORM rezervace úroveň chyb ACT protokolů a použitím nižších úrovních INFORM protokolů.

![Úroveň protokolování](monitoring-and-telemetry/_static/image20.png)

### <a name="configure-logging-levels-at-run-time"></a>Konfigurace úrovně protokolování v době běhu

I když je vhodné mít vždy protokolování v produkčním prostředí, jiné osvědčeným postupem je implementace rozhraní protokolování, které umožňuje nastavit úroveň podrobností, které se přihlašujete, bez opětovného nasazení nebo restartování vaší aplikace v době běhu. Třeba když použijete nástroj trasování v `System.Diagnostics` můžete vytvořit chyby, upozornění, informace a protokolů ladění/Verbose. Doporučujeme vždy protokolovat chyby, upozornění, informace o protokoly v produkčním prostředí a budete chtít mít možnost dynamicky přidávat ladění/podrobné protokolování pro řešení potíží s případ od případu.

Web Apps ve službě Azure App Service mají integrovanou podporu pro zápis `System.Diagnostics` protokoly systému souborů, Table storage nebo Blob storage. Můžete vybrat jiný protokolování úrovní pro každý cíl úložiště a můžete změnit úroveň protokolování v reálném čase bez restartování aplikace. Podpora úložiště blobů usnadňuje spouštění [HDInsight](https://docs.microsoft.com/azure/hdinsight/) analýzy úloh na protokolů aplikace, protože HDInsight umí pracovat přímo s úložištěm objektů Blob.

### <a name="log-exceptions"></a>Výjimky protokolu

Právě neumisťujte *výjimky. ToString()* ve vašem kódu protokolování. Který vynechány kontextové informace. V případě chyb SQL ponechá si číslo chyby SQL. Pro všechny výjimky zahrnují informace o kontextu, výjimky, vlastní a vnitřních výjimek, abyste měli jistotu, že zadáváte všechno, co, které se mají provést při řešení potíží. Informace o kontextu může obsahovat třeba název serveru, identifikátor transakce a uživatelské jméno (ale není heslo nebo jakýchkoli tajných kódů!).

Pokud se spoléháte na každý vývojář, že správně dělají, s výjimkou protokolování, některé z nich nebudou. Aby bylo zajištěno, že to proběhne vpravo způsobem pokaždé, když, sestavení zpracování přímo do vašeho rozhraní protokolovače výjimek: předání objektu výjimky a tříd protokolovacího nástroje a protokolovat data výjimky správně ve třídě protokolovacího nástroje.

### <a name="log-calls-to-services"></a>Protokol volání služeb

Důrazně doporučujeme, že napíšete protokol pokaždé, když vaše aplikace zdůrazňuje ke službě, ať už jde o databázi nebo rozhraní REST API nebo všechny externí služby. Zahrnout do protokolů nejen údaj o úspěchu nebo selhání, ale jak dlouho trvalo každý požadavek. V cloudovém prostředí se často zobrazí problémy související s slow-downs spíše než úplné výpadků. To obvykle trvá 10 milisekund může najednou začít přijímat sekundy. Když někdo řekne vám, že vaše aplikace je pomalá, chtějí mít možnost se podívat na špičkové funkce New Relicu nebo libovolné telemetrické službě máte a ověřit jejich prostředí a potom chcete mít možnost sledovat jsou vlastní protokoly přejít k podrobnostem, proč je pomalé.

### <a name="use-an-ilogger-interface"></a>Použití objektu ILogger rozhraní

Co doporučováno při vytváření produkční aplikace, je vytvořit jednoduchou *ILogger* rozhraní a zůstaňte některé metody v ní. To umožňuje snadno změnit později implementace protokolování a není nutné absolvovat celý kód na to. Společnost Microsoft může používat `System.Diagnostics.Trace` třídy v rámci aplikace Fix It, ale místo toho používáme ho na pozadí v třídě protokolování, který implementuje *ILogger*, a My *ILogger* metoda se volá v průběhu aplikace.

Tímto způsobem, pokud někdy budete chtít vytvořit vaše protokolování bohatší, můžete nahradit [ `System.Diagnostics.Trace` ](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio#apptracelogs) pomocí libovolné mechanismus protokolování chcete. Například s růstem aplikace můžete se rozhodnout, že chcete použít jako komplexnější balíček protokolování [NLog](http://nlog-project.org/) nebo [Enterprise knihovny Logging Application Block](https://msdn.microsoft.com/library/dn440731(v=pandp.60).aspx). ([Log4Net](http://logging.apache.org/log4net/) je další oblíbené protokolovací rozhraní, ale nedělá asynchronního protokolování.)

Jedním z možných důvodů pro pomocí architektury, jako je NLog je usnadnit rozdělením protokolování výstup do úložišť samostatná data pro vysoké objemy a vysoké hodnoty. Který pomáhá efektivně ukládejte velké objemy dat INFORM, která není nutné spouštět rychlé dotazy, a přitom rychlý přístup k datům ACT.

### <a name="semantic-logging"></a>Sémantické protokolování

Relativně nový způsob, jak provádět protokolování, které můžete vytvářet další užitečné diagnostické informace, najdete v části [Enterprise knihovny sémantické protokolování aplikace blok (SLAB)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Používá SLAB [události trasování pro Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) a [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) podporují v rozhraní .NET 4.5 umožňující vytvářet více protokolů strukturovaných a zadávat dotazy. Můžete definovat jinou metodu pro každý typ události, které můžete protokolovat, který umožňuje přizpůsobit informace, které napíšete. Například pro přihlášení k chybě SQL Database může volat `LogSQLDatabaseError` metody. Pro tento druh výjimky víte, že je klíčovou informací je číslo chyby, takže můžete zahrnout číselný parametr chyby v podpisu metody a poznamenejte si číslo chyby jako samostatné pole v záznamu protokolu, který napíšete. Vzhledem k tomu, že číslo je v samostatné pole můžete snadno a spolehlivě získat sestavy podle čísla chyby SQL, než by byla právě zřetězení číslo chyby do řetězec zprávy.

## <a name="logging-in-the-fix-it-app"></a>Protokolování v opravě aplikace

### <a name="the-ilogger-interface"></a>Rozhraní objektu ILogger

Tady je *ILogger* rozhraní v aplikaci opravit.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample1.cs)]

Tyto metody umožňují zapisovat protokoly na úrovni stejného čtyři nepodporuje *System.Diagnostics*. Metody TraceApi jsou pro protokolování volání externích služeb s informacemi o latenci. Můžete také přidat sadu metod pro ladění/Verbose úroveň.

### <a name="the-logger-implementation-of-the-ilogger-interface"></a>Protokolovací nástroj implementací rozhraní ILogger

Implementace rozhraní je opravdu jednoduché. V podstatě jen volá do standardu *System.Diagnostics* metody. Následující fragment kódu ukazuje všechny tři metody informace a jedna z ostatních.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample2.cs)]

### <a name="calling-the-ilogger-methods"></a>Volání metody ILogger

Pokaždé, když kód v aplikaci opravit zachytí výjimku, volá *ILogger* metoda protokolovat podrobnosti o výjimce. A pokaždé, když se zavolá k databázi, službu Blob service nebo rozhraní REST API, spustí stopky před voláním, skončí služba vrátí, stopky a zaznamená uplynulý čas spolu s informacemi o úspěchu nebo neúspěchu.

Všimněte si, že zprávy protokolu obsahuje název třídy a název metody. Je dobrým zvykem, abyste měli jistotu, že zprávy protokolu určit, jaká část kódu aplikací napsali je.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample3.cs?highlight=6,14,20-21,25)]

Takže teď pro pokaždé, když aplikace Fix It provedl volání do služby SQL Database můžete zobrazit volání metody, která se volá, a přesně kolik času trvalo.

![Dotaz SQL Database v protokolech](monitoring-and-telemetry/_static/image21.png)

![](monitoring-and-telemetry/_static/image22.png)

Pokud přejít procházení protokoly, uvidíte, že čas trvat volání databáze je proměnná. Informace může být užitečné: protože aplikace přihlásí, to vše lze analyzovat historické trendy ve výkonu služby databáze v čase. Pro instanci služby může být rychlé ve většině případů ale žádostí může selhat nebo může zpomalit odpovědi v určitých časech den.

Můžete provést totéž pro službu Blob service – pokaždé, když se aplikace nahraje nový soubor, je do protokolu a vy vidíte, přesně jak dlouho trvalo kvůli nahrání každého souboru.

![Objekt BLOB nahrání protokolu](monitoring-and-telemetry/_static/image23.png)

Je jenom pár řádky kódu k zápisu pokaždé, když volání služby, a nyní pokaždé, když se někdo řekne, že jejich narazili na problém, víte přesně co problém se, zda došlo k chybě, nebo i v případě, že se právě běží pomalu. Můžete přesně určit zdroj problému bez nutnosti vzdáleném připojení k serveru nebo zapnout protokolování poté, co se stane chyba a Doufáme, že ji znovu vytvořit.

## <a name="dependency-injection-in-the-fix-it-app"></a>Injektáž závislostí v opravě je aplikace

Může vás zajímat, jak konstruktoru úložiště v předchozím příkladu získá protokolovač implementace rozhraní:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample4.cs?highlight=6)]

Propojí rozhraní k implementaci aplikace používá [injektáž závislostí](http://en.wikipedia.org/wiki/Dependency_injection)(DI) s [AutoFac](http://autofac.org/). DI umožňuje použít objekt založené na rozhraní na mnoha místech v kódu a mít jenom k určení na jednom místě implementace, která se používá při vytváření instance rozhraní. To usnadňuje změňte implementaci: například můžete chtít nahradit protokolovač NLog System.Diagnostics protokolovacího nástroje. Nebo pro automatizované testování můžete chtít nahradit verzi mock protokolovacího nástroje.

Aplikace Fix It používá DI na všechna úložiště a všech řadičů. Získat konstruktorů třídy kontroleru *ITaskRepository* rozhraní stejným způsobem jako úložiště získá protokolovač rozhraní:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample5.cs?highlight=5)]

Aplikace používá knihovnu AutoFac DI automaticky poskytnout *TaskRepository* a *protokolovací nástroj* instancí těchto konstruktorů.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample6.cs?highlight=8,10)]

Tento kód v podstatě říká, že kamkoli konstruktor by měl *ILogger* rozhraní, předejte instanci *protokolovací nástroj* třídy, a vždy, když potřebuje *IFixItTaskRepository*rozhraní, předejte instanci *FixItTaskRepository* třídy.

[AutoFac](http://autofac.org/) je jednou z mnoha architektur vkládání závislostí, které můžete použít. Je jiný oblíbených [Unity](https://blogs.msdn.com/b/agile/archive/2013/08/20/new-guide-dependency-injection-with-unity.aspx), což je doporučená a podporuje Microsoft Patterns and Practices.

## <a name="built-in-logging-support-in-azure"></a>Podpora vestavěné protokolování v Azure

Azure podporuje následující typy z [protokolování pro webové aplikace ve službě Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio):

- Trasování System.Diagnostics (můžete zapnout a vypnout a nastavit úrovně za chodu bez restartování webu).
- Události Windows.
- Protokoly služby IIS (HTTP/FREB).

Azure podporuje následující typy z [protokolování v cloudových službách](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics):

- Trasování System.Diagnostics.
- Čítače výkonu.
- Události Windows.
- Protokoly služby IIS (HTTP/FREB).
- Sledování vlastních adresářích.

Aplikace Fix It používá trasování System.Diagnostics. Vše, co potřebujete udělat, aby povolit System.Diagnostics protokolování ve webové aplikaci se Převrátit přepínač na portálu nebo volat rozhraní REST API. Na portálu klikněte na tlačítko **konfigurace** kartu pro váš web a posuňte se dolů zobrazíte **konzole Application Diagnostics** části. Můžete zapnout nebo vypnout protokolování a vyberte požadovanou úroveň protokolování. Můžete mít zapisovat protokoly do systému souborů nebo k účtu úložiště Azure.

![Aplikace diagnostiku a lokality na kartě Konfigurace](monitoring-and-telemetry/_static/image24.png)

Po povolení protokolování v Azure, uvidíte protokolů v okně Výstup Visual Studia při jejich vytvoření.

![Streamování protokolů nabídky](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-viewlogsmenu.png)

![Streamování protokolů nabídky](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-nologsyet.png)

Můžete mít také zapsána do vašeho účtu úložiště protokolů a zobrazení jim všechny nástroje, který má přístup k službě Azure Storage Table, jako například **Průzkumníka serveru** v sadě Visual Studio nebo [Průzkumníka služby Azure Storage](https://azure.microsoft.com/features/storage-explorer/).

![Protokoly v Průzkumníku serveru](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-storagelogs.png)

## <a name="summary"></a>Souhrn

Je velmi snadno implementovat systém telemetrie out-of-the-box, nástroje protokolování ve svém vlastním kódu a nakonfigurovat protokolování v Azure. A až budete mít problémy v produkčním prostředí, kombinace systém telemetrie a vlastní protokoly vám pomůžou rychle vyřešit problémy, než bude hlavní problémy pro vaše zákazníky.

V [další kapitolu](transient-fault-handling.md) podíváme na způsob zpracování přechodných chyb, se nebudou problémy v produkčním prostředí, které máte k prozkoumání.

## <a name="resources"></a>Prostředky

Další informace najdete v tématu následující prostředky.

Dokumentace ke službě hlavně o telemetrii:

- [Microsoft Vzory a postupy – doprovodné materiály k Azure](https://msdn.microsoft.com/library/dn568099.aspx). Zobrazit pokyny pro instrumentaci a Telemetrii, pokynech k měření služby, model monitorování stavu koncových bodů a způsob konfigurace modulu Runtime.
- [Pritzker sevření v cloudu: povolení monitorování na Azure websites na úrovni výkonu nové Relic](http://www.hanselman.com/blog/PennyPinchingInTheCloudEnablingNewRelicPerformanceMonitoringOnWindowsAzureWebsites.aspx).
- [Osvědčené postupy pro navrhování rozsáhlých služeb v Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Dokument White paper Mark Simms a Michael Thomassy. V části telemetrických dat a diagnostiky.
- [Generace vývoj s využitím Application Insights](https://msdn.microsoft.com/magazine/dn683794.aspx). Článek v časopise MSDN Magazine.

Dokumentace ke službě hlavně o protokolování:

- [Blok aplikací sémantické protokolování (SLAB)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Neil Mackenzie uvede platí pro sémantické protokolování s SLAB.
- [Vytváření strukturovaných a smysluplná protokolů s sémantického protokolování](https://channel9.msdn.com/Events/Build/2013/3-336). (Video) Juliánský Dominguez uvede platí pro sémantické protokolování s SLAB.
- [EF6 Protokolování SQL – část 1: jednoduché protokolování](http://blog.oneunicorn.com/2013/05/08/ef6-sql-logging-part-1-simple-logging/). Podle Arthur Vickerse ukazuje, jak protokolovat dotazů rozhraním Entity Framework v EF 6.
- [Odolnost připojení a zachycení příkazů s Entity Framework v aplikaci ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Čtvrtý z devět částí série, ukazuje, jak použít funkci EF 6 příkaz zachycení do protokolu SQL příkazy, odeslané do databáze rozhraním Entity Framework.
- [Zlepšení pomocí jazyka C# 5.0 volající atributy informace o protokolování](http://www.dotnetcurry.com/showarticle.aspx?ID=972). Jak můžete snadno protokolovat název volání metody bez kódu ho do literály nebo se dá stáhnout ručně pomocí operace reflection.

Dokumentace ke službě hlavně o řešení potíží:

- [Poradce při potížích s Azure &amp; ladění blogu](https://blogs.msdn.com/b/kwill/).
- [AzureTools – diagnostické nástroje používané na tým podpory Azure Developer](https://blogs.msdn.com/b/kwill/archive/2013/08/26/azuretools-the-diagnostic-utility-used-by-the-windows-azure-developer-support-team.aspx?Redirected=true). Představuje a poskytuje odkaz ke stažení pro nástroj, který je možné ke stažení a spuštění celou řadu nástrojů pro diagnostiku a sledování na Virtuálním počítači Azure. Je užitečné, když budete potřebovat k diagnostice problému konkrétního virtuálního počítače.
- [Řešení potíží s webovou aplikací ve službě Azure App Service pomocí sady Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Podrobný kurz pro zahájení práce s vzdálené ladění a trasování System.Diagnostics.

Videa:

- [Bezporuchový: Sestavování škálovatelných, odolných cloudových služeb](https://channel9.msdn.com/Series/FailSafe). Ulrich Homann, Marc Mercuri a Mark Simms devět částí série. Nabídne základními koncepty a Principy architektury tak vysoce dostupné a zajímavé příběhy z prostředí Microsoft zákazníka poradní tým (CAT) se skutečným zákazníkům. Epizody 4 a 9 jsou informace o monitorování a telemetrie. Epizoda 9 jsou základní informace o sledování služby MetricsHub, AppDynamics, New Relic a PagerDuty.
- [Vytváření Big: Získané od zákazníků Azure – část II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms se hovoří o návrh pro selhání a všechno, co instrumentace. Podobně jako řady bezporuchový ale přejde do další postupy podrobnosti.

Ukázka kódu:

- [Cloud Service Fundamentals v Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Ukázková aplikace vytvořené v Microsoft Azure zákaznického poradního týmu. Telemetrie a postupy pro protokolování, ukazuje, jak je popsáno v následujících článcích. Ukázka implementuje protokolování aplikací pomocí [NLog](http://nlog-project.org/). Související dokumentace, najdete v článku [řadu čtyři články na webu TechNet wiki o telemetrii a protokolování](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx#Telemetry_coming_soon).

> [!div class="step-by-step"]
> [Předchozí](design-to-survive-failures.md)
> [další](transient-fault-handling.md)
