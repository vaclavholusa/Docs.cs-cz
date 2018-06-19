---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
title: 'Dodatek: Opravu ho ukázkovou aplikaci (vytváření reálných cloudových aplikací s Azure) | Microsoft Docs'
author: MikeWasson
description: Cloudové aplikace skutečné World sestavení s Azure elektronická kniha je založena na prezentace vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které můžete mu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 1bc333c5-f096-4ea7-b170-779accc21c1a
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
msc.type: authoredcontent
ms.openlocfilehash: 9a1fa36b34c4783b101bb27bc6931241e9251e10
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876472"
---
<a name="appendix-the-fix-it-sample-application-building-real-world-cloud-apps-with-azure"></a>Dodatek: Opravu ho ukázkovou aplikaci (vytváření reálných cloudových aplikací s Azure)
====================
podle [Karel Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si opravu ho projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)

> **Vytváření reálného světa cloudových aplikací s Azure** elektronická kniha je založena na prezentaci vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které vám mohou pomoci být úspěšná, vývoj webových aplikací pro cloud. Informace o elektronická kniha najdete v tématu [první kapitoly](introduction.md).


Tento dodatek pro cloudové aplikace skutečné World sestavení s Azure elektronická kniha obsahuje následující části, které poskytují další informace o opravte ji ukázkové aplikace, můžete si stáhnout:

- [Známé problémy](#knownissues)
- [Doporučené postupy](#bestpractices)
- [Postup spuštění aplikace v místním počítači ze sady Visual Studio](#run-in-vs)
- [Postup nasazení základní aplikace do Azure App Service Web Apps pomocí skriptů prostředí Windows PowerShell](#deploybase)
- [Řešení potíží se skripty prostředí Windows PowerShell](#troubleshooting)
- [Postup nasazení aplikace s frontou zpracování Azure App Service Web Apps a cloudové služby Azure](#deployqueues)

<a id="knownissues"></a>
## <a name="known-issues"></a>Známé problémy

Aplikace opravte vyvinula původně k objasnění co nejsnáze některé vzory uvedené v této příručce e. Ale totiž o vytváření aplikací reálného elektronická kniha jsme podrobí kód opravte zkontrolujte a testování procesu podobná co by provedeme pro verze softwaru. Byl nalezen řadu problémů, stejně jako u jakékoli reálné aplikaci některá z nich vyřešili a některá z nich jsme odložení na novější verzi.

Následující seznam obsahuje problémy, které je třeba zvážit v případě produkční aplikace, ale pro jednu důvod nebo jiného, který jsme se rozhodli, ne na adresu počáteční verze ukázkovou aplikaci opravte ji.

### <a name="security"></a>Zabezpečení

- Ujistěte se, že úlohu nelze přiřadit k neexistující vlastníka.
- Zajistěte, aby vám mohla jenom zobrazovat a upravovat úlohy, které jste vytvořili nebo přiřazené vám.
- Pro přihlašovací stránky a soubory cookie pro ověřování použijte protokol HTTPS.
- Zadejte časový limit pro soubory cookie pro ověřování.

### <a name="input-validation"></a>Ověření vstupu

Obecně by se produkční aplikace více vstupních ověření než aplikace opravit. Například velikost obrázku / image pro nahrávání by měla být omezená povolená velikost souboru.

### <a name="administrator-functionality"></a>Funkce správce

Správce by měl být moct změnit vlastnictví na stávající úlohy. Například tvůrce úloha může ze společnosti odešel, nechá nikdo s autoritou udržovat úlohu, pokud je povolen přístup pro správu.

### <a name="queue-message-processing"></a>Zpracování zprávy fronty

Zpracování v aplikaci opravte fronty zpráv byla navržená tak, že byly jednoduché k objasnění vzoru fronty způsob práce s minimální velikostí kódu. Tento jednoduchý kód by stačit pro skutečné produkční aplikaci.

- Kód není zaručeno, že každá zpráva fronty budou zpracovány maximálně jednou. Pokud dostanete zprávu z fronty, je časový limit, během které je zpráva neviditelná pro další moduly pro naslouchání fronty. Pokud vyprší časový limit před odstraněním zprávy, se zpráva zobrazí znovu. Proto pokud instanci role pracovního procesu stráví dlouhou dobu zpracování zprávy, je teoreticky může pro stejnou zprávu zpracovat dvakrát, výsledkem je duplicitní úloh v databázi. Další informace o tomto problému najdete v tématu [pomocí fronty úložiště Azure](https://msdn.microsoft.com/library/ff803365.aspx#sec7).
- Logika dotazování fronty může být cenově výhodnější podle dávkování načítání zpráv. Pokaždé, když zavoláte [CloudQueue.GetMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx), a náklady transakce. Místo toho můžete volat [CloudQueue.GetMessagesAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (Poznámka: množném čísle '), který získá více zpráv v rámci jedné transakce. Transakce náklady na úložiště front Azure jsou velmi nízké, takže dopad na náklady není podstatný ve většině scénářů.
- Úzkou smyčky v kódu zpracování zprávy fronty způsobí, že spřažení procesoru, který nevyužívá efektivně vícejádrových virtuálních počítačů. Návrh s lepší využije paralelismus spustit několik úloh s modifikátorem async paralelně.
- Zpracování zpráv fronty má pouze elementární výjimek. Například nemůže pracovat s kód [škodlivých zpráv](https://msdn.microsoft.com/library/ms789028.aspx). (Při zpracování zprávy způsobí výjimku, budete muset přihlásit k chybě a odstranění zprávy, nebo roli pracovního procesu se pokusí znovu zpracovat, a že opakování ve smyčce bude pokračovat po neomezenou dobu.)

### <a name="sql-queries-are-unbounded"></a>Dotazy SQL jsou bez vazby

Aktuální opravte ji kód umístí na tom, kolik řádků dotazy pro Index stránky může vracet žádné omezení. Pokud velkého objemu úlohy je zadán do databáze, velikost výsledných seznamů přijatých mohly způsobit problémy s výkonem. Řešení je k implementaci stránkování. Příklad, naleznete v části [třídění, filtrování a stránkování s platformou Entity Framework v aplikaci ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md).

### <a name="view-models-recommended"></a>Zobrazit modely doporučená

Aplikace opravte používá třídu entity FixItTask k předávání informací mezi kontrolerem a zobrazení. Osvědčeným postupem je použití modelů zobrazení. Kolem co je potřeba pro trvalosti dat, během zobrazení modelu může být navrženy pro prezentace dat slouží k modelu domény (například třída FixItTask entity). Další informace najdete v tématu [12 ASP.NET MVC osvědčené postupy](http://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx).

### <a name="secure-image-blob-recommended"></a>Doporučuje objekt blob zabezpečené image

Obchody opravte ji nahrát bitové kopie jako veřejné, což znamená, že každý, kdo najde adresu URL přístup k bitové kopie. Bitové kopie může být zabezpečená místo veřejné.

### <a name="no-powershell-automation-scripts-for-queues"></a>Žádné skripty pro automatizaci prostředí PowerShell pro fronty

Ukázkové skripty pro automatizaci prostředí PowerShell byly napsány pouze pro základní verze opravit to, které běží zcela v Azure App Service Web Apps. Pro nastavení a nasazení do webové aplikace a prostředí cloudové služby požadovanou ke zpracování fronty jsme nebyly k dispozici skripty.

### <a name="special-handling-for-html-codes-in-user-input"></a>Zvláštní zpracování pro kódy HTML vstup uživatele

ASP.NET automaticky brání mnoho způsobů, ve kterých uživatelé se zlými úmysly může pokus útoky skriptování mezi weby zadáním skriptu do polí vstupu uživatele. A MVC `DisplayFor` pomocné rutiny, které slouží k zobrazení úloh produkty a poznámky k automaticky umístí kódování HTML hodnoty, které odešle do prohlížeče. Ale v produkční aplikace můžete chtít použít další opatření. Další informace najdete v tématu [žádosti o ověření v technologii ASP.NET](https://msdn.microsoft.com/library/hh882339.aspx).

<a id="bestpractices"></a>
## <a name="best-practices"></a>Doporučené postupy

Toto jsou některé problémy, které jsme vyřešili poté, co byla zjištěna ve revize kódu a testování původní verze aplikace opravit. Některé byla způsobena původní kodér nebude vědět konkrétní osvědčený postup, některé jednoduše protože kód byla zapsána rychle a nebyla určená verze softwaru. V případě, že je něco, co jste se dozvěděli z této kontrole jsme se výpis sem problémy a testování, které mohou být užitečné ostatním uživatelům, kteří jsou také vývoj webových aplikací.

### <a name="dispose-the-database-repository"></a>Uvolnění úložiště databáze

`FixItTaskRepository` Třída musí dispose rozhraní Entity Framework `DbContext` instance. Jsme to nebyla implementací `IDisposable` v `FixItTaskRepository` třídy:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample1.cs)]

Všimněte si, že bude automaticky dispose AutoFac `FixItTaskRepository` instance, proto jsme není nutné explicitně dispose ho.

Další možností je odstranit `DbContext` členské proměnné z `FixItTaskRepository`a místo toho vytvořte místní `DbContext` proměnné v rámci každé metody úložiště uvnitř `using` příkaz. Příklad:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample2.cs)]

### <a name="register-singletons-as-such-with-di"></a>Registraci jednotlivých prvků jako takový DI

Od pouze jedna instance `PhotoService` třídy a `Logger` třída je potřeba, by měly být tyto třídy [registrován jako jedné instance pro vkládání závislostí](https://code.google.com/p/autofac/wiki/InstanceScope) v *DependenciesConfig.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample3.cs?highlight=1,3)]

### <a name="security-dont-show-error-details-to-users"></a>Zabezpečení: Nezobrazovat podrobnosti o chybě pro uživatele

Původní opravte ji aplikace nebyla na stránce Obecná chyba a právě umožněte všechny výjimky bublin až uživatelského rozhraní, tak některé výjimky, jako je například k chybám připojení databáze může mít za následek úplnou zásobníku se zobrazuje v prohlížeči. Podrobné informace o chybě můžete někdy usnadnění útoky uživateli se zlými úmysly. Řešení je protokolu podrobnosti o výjimce a pro uživatele, který neobsahuje podrobnosti o chybě zobrazit chybovou stránku. Aplikace opravte byl již protokolování a aby bylo možné zobrazit chybovou stránku, jsme přidali `<customErrors mode=On>` v souboru Web.config.

[!code-xml[Main](the-fix-it-sample-application/samples/sample4.xml?highlight=2)]

Ve výchozím nastavení to způsobí, že *Views\Shared\Error.cshtml* má být zobrazen pro chyby. Můžete přizpůsobit *Error.cshtml* nebo vytvořte vlastní chyba zobrazení stránky a přidejte `defaultRedirect` atribut. Můžete také zadat různé chybové stránky pro konkrétní chyby.

### <a name="security-only-allow-a-task-to-be-edited-by-its-creator"></a>Zabezpečení: Povolte jenom úloha je nelze upravovat pomocí jeho autorovi

Řídicí panel indexovou stránku se zobrazí pouze úlohy vytvořené pomocí přihlášeného uživatele, ale uživatel se zlými úmysly může vytvořit adresu URL s ID úkolu jiným uživatelem. Jsme přidali kód v *DashboardController.cs* vrátit 404 v takovém případě:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample5.cs?highlight=9-14,24-29)]

### <a name="dont-swallow-exceptions"></a>Nemáte swallow výjimky

Původní aplikace opravte jej právě vrátil hodnotu null po protokolování výjimka, která je výsledkem dotazu SQL:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample6.cs?highlight=4)]

Proto by být zobrazovat uživateli, jako kdyby dotaz byl úspěšný, avšak pouze nevrátil žádné řádky. Řešení je znovu vyvolání výjimky po zachytávání a protokolování:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample7.cs)]

### <a name="catch-all-exceptions-in-worker-roles"></a>Zachycení všechny výjimky v rolí pracovního procesu

Virtuální počítač recyklace způsobí, že všechny neošetřenými výjimkami v roli pracovního procesu, kterou chcete zabalit všechno, co dělat v bloku try-catch – zpracování a zpracování všech výjimek.

### <a name="specify-length-for-string-properties-in-entity-classes"></a>Zadat délku řetězce vlastnosti tříd entit

Aby bylo možné zobrazit jednoduchého kódu, původní verze aplikace opravte nezadal délky pole FixItTask entity, a v důsledku byly definované jako varchar(max) v databázi. Uživatelské rozhraní v důsledku toho by přijmout téměř jakoukoli množství vstup. Zadání omezení délky sady, které se vztahují na uživatele, zadejte v webové stránky a velikost sloupce v databázi:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample8.cs?highlight=4,7,10,12,14)]

### <a name="mark-private-members-as-readonly-when-they-arent-expected-to-change"></a>Soukromé členy označit jako jen pro čtení, když nejsou očekávané změnit

Například v `DashboardController` třídy instanci `FixItTaskRepository` je vytvořen a neočekává se, změnit, takže jsme definovali jej jako [jen pro čtení](https://msdn.microsoft.com/library/acdd6hb7.aspx).

[!code-csharp[Main](the-fix-it-sample-application/samples/sample9.cs?highlight=3)]

### <a name="use-listany-instead-of-listcount-gt-0"></a>Pomocí seznamu. Any() místo seznamu. Count() &gt; 0

Pokud jste se zajímáte o tom, zda jeden nebo více položek v seznamu podle zadaným kritériím, použijte [žádné](https://msdn.microsoft.com/library/bb534972.aspx) metoda, protože vrátí také položku hodí kritéria nenajde, zatímco `Count` metoda má vždy k iteraci v prostřednictvím každá položka. Řídicí panel *Index.cshtml* soubor původně měl tento kód:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample10.cshtml)]

Jsme změnili k tomuto:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample11.cshtml?highlight=1)]

### <a name="generate-urls-in-mvc-views-using-mvc-helpers"></a>Generování adresy URL v MVC zobrazeními s využitím pomocné rutiny MVC

Pro **opravte ji vytvořit** tlačítko na domovské stránce, opravte ji aplikace pevný programového anchor element:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample12.cshtml)]

Pro zobrazení nebo akce odkazy takto je vhodnější použít [Url.Action](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx) pomocné rutiny HTML, například:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample13.cshtml)]

### <a name="use-taskdelay-instead-of-threadsleep-in-worker-role"></a>Použití Task.Delay místo Thread.Sleep v roli pracovního procesu

Vloží nový projekt šablony `Thread.Sleep` v ukázce kód pro roli pracovního procesu, ale způsobuje vlákno do režimu spánku, může způsobit fondu vláken vytváření další nepotřebné vlákna. Se můžete vyhnout pomocí [Task.Delay](https://msdn.microsoft.com/library/hh139096.aspx) místo.

[!code-csharp[Main](the-fix-it-sample-application/samples/sample14.cs?highlight=11)]

### <a name="avoid-async-void"></a>Vyhněte se asynchronní void

Při použití asynchronní metody nemusí vracet hodnotu, vrátí `Task` typ místo `void`.

V tomto příkladu je z `FixItQueueManager` třídy: 

[!code-csharp[Main](the-fix-it-sample-application/samples/sample15.cs)]

Měli byste použít `async void` pouze pro obslužné rutiny událostí nejvyšší úrovně. Pokud definujete jako metodu `async void`, volající nelze **await** metodu nebo catch jakékoli výjimky, vyvolá metoda. Další informace najdete v tématu [osvědčené postupy v asynchronní programování](https://msdn.microsoft.com/magazine/jj991977.aspx). 

### <a name="use-a-cancellation-token-to-break-from-worker-role-loop"></a>Použít token zrušení pro přerušení smyčky role pracovního procesu

Obvykle **spustit** metoda na roli pracovního procesu obsahuje nekonečné smyčce. Při zastavení role pracovního procesu [RoleEntryPoint.OnStop](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) metoda je volána. By měl použít tuto metodu zrušit práci, kterou provádí uvnitř **spustit** metoda a ukončete řádně. Proces, jinak hodnota může být ukončen uprostřed operace.

### <a name="opt-out-of-automatic-mime-sniffing-procedure"></a>Zamítnutí automatického MIME postup sledování toku dat

V některých případech aplikace Internet Explorer hlásí typ MIME, který je jiný než v typu zadaném pomocí webového serveru. Například pokud aplikace Internet Explorer najde HTML obsahu v souboru doručit s odpovědi hlavičku HTTP Content-Type: text/plain, Internet Explorer určuje, že obsah má být vykreslen jako kód HTML. Bohužel se tento "MIME-sledování toku dat" může vést k problémy se zabezpečením pro servery, které hostují nedůvěryhodný obsah. Boje proti tomuto problému, Internet Explorer 8 udělal několik změn kódu rozhodnutí typ MIME a umožňuje vývojářům aplikací [vyjádření výslovného nesouhlasu s sledování toku dat MIME](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx). Následující kód byl přidán do *Web.config* souboru.

[!code-xml[Main](the-fix-it-sample-application/samples/sample16.xml?highlight=2-7)]

### <a name="enable-bundling-and-minification"></a>Povolit sdružování a minimalizace

Když Visual Studio vytvoří nový webový projekt, není ve výchozím nastavení povoleno sdružování a minimalizace soubory JavaScript. Jsme přidali řádek kódu v BundleConfig.cs:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample17.cs?highlight=9)]

### <a name="set-an-expiration-time-out-for-authentication-cookies"></a>Nastavení vypršení časového limitu pro ověřovací soubory cookie

Ve výchozím nastavení soubory cookie pro ověřování vyprší za dva týdny. Kratší dobu je bezpečnější. Toto nastavení můžete změnit *StartupAuth.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample18.cs?highlight=4-5)]

<a id="run-in-vs"></a>
## <a name="how-to-run-the-app-from-visual-studio-on-your-local-computer"></a>Postup spuštění aplikace v místním počítači ze sady Visual Studio

Spuštění aplikace vyřešit dvěma způsoby:

- Spusťte základní aplikace, která zapisuje nové úkoly přímo do databáze SQL.
- Spuštění aplikace pomocí fronty a back-end službu k vytvoření úlohy. Vzor fronty je popsané v kapitole [zaměřené na fronty práci vzor](queue-centric-work-pattern.md).

<a id="runbase"></a>
### <a name="run-the-base-application"></a>Spustit základní aplikace

1. Nainstalujte [Visual Studio 2013 nebo Visual Studio 2013 Express pro Web](https://www.visualstudio.com/downloads).
2. Nainstalujte [Azure SDK pro .NET pro Visual Studio 2013.](https://go.microsoft.com/fwlink/p/?linkid=323510&amp;clcid=0x409)
3. Stáhněte si soubor .zip z [galerie kódů MSDN](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4).
4. V Průzkumníku souborů klikněte pravým tlačítkem na soubor .zip a klikněte na tlačítko Vlastnosti a v okně vlastností klikněte na odblokovat.
5. Rozbalte soubor.
6. Poklikejte na soubor .sln ke spuštění sady Visual Studio.
7. V nabídce Nástroje klikněte na tlačítko Správce balíčků knihoven a potom konzola Správce balíčků.
8. V balíček správce konzoly (pomocí PMC), kliknutím na tlačítko Obnovit.
9. Ukončete aplikaci Visual Studio.
10. Spuštění [emulátoru úložiště Azure](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx).
11. Restartujte Visual Studio, otevřete soubor řešení uzavřený v předchozím kroku.
12. Ujistěte se, že projekt automatickou je nastavit jako spouštěný projekt a potom stiskněte klávesu CTRL + F5 a spusťte projekt.

<a id="queueslocal"></a>
### <a name="run-the-application-with-queue-processing"></a>Spuštění aplikace s zpracování fronty

1. Postupujte podle pokynů pro [spustit základní aplikace](#runbase)a pak zavřete prohlížeč a zavřete Visual Studio.
2. Spuštění sady Visual Studio s oprávněními správce. (Které budete používat emulátor výpočtů v Azure, a který vyžaduje oprávnění správce.)
3. V aplikaci *Web.config* v soubor *MyFixIt* projektu (webový projekt), změňte hodnotu `appSettings/UseQueues` hodnotu "true": 

    [!code-console[Main](the-fix-it-sample-application/samples/sample19.cmd?highlight=3)]
4. Pokud [emulátoru úložiště Azure](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx) není stále spuštěná, spusťte ji znovu.
5. Spusťte automatickou webový projekt a projekt MyFixItCloudService současně.

    Pomocí sady Visual Studio 2013:

   1. Stisknutím klávesy F5 spusťte projekt automatickou.
   2. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt MyFixItCloudService a pak klikněte na tlačítko **ladění** -- **spustit novou instanci**.

      Pomocí Visual Studio 2013 Express pro Web:

   3. V Průzkumníku řešení klikněte pravým tlačítkem na řešení automatickou a vyberte **vlastnosti**.
   4. Vyberte **více projektů po spuštění**...
   5. V **akce** rozevíracího seznamu v části MyFixIt a MyFixItCloudService, vyberte **spustit**.
   6. Click **OK**.
   7. Stisknutím klávesy F5 spusťte obou projektů.

      Při spuštění MyFixItCloudService projektu sady Visual Studio spustí emulátor výpočtů v Azure. V závislosti na konfiguraci brány firewall může být nutné povolit emulátoru přes bránu firewall.

<a id="deploybase"></a>
## <a name="how-to-deploy-the-base-app-to-azure-app-service-web-apps-by-using-the-windows-powershell-scripts"></a>Postup nasazení základní aplikace do Azure App Service Web Apps pomocí skriptů prostředí Windows PowerShell

Pro ilustraci [automatizovat všechno](automate-everything.md) vzor, opravte ji aplikace se dodává s skripty, které nastavení prostředí v Azure a nasazení projektu do nové prostředí. Následující pokyny popisují, jak používat skripty.

Pokud chcete spustit v Azure bez použití front a provedené změny se spustit místně v případě front, ujistěte se, že nastavíte hodnotu appSetting UseQueues zpět na hodnotu false před pokračováním podle následujících pokynů.

Tyto pokyny předpokládají jste si stáhli a řešení opravte ji spustit místně, a že máte Azure účet nebo předplatné služby Azure, máte oprávnění ke správě.

1. Nainstalujte **prostředí Azure PowerShell** konzoly. Pokyny najdete v tématu [postup instalace a konfigurace prostředí Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).

    Tato vlastní konzola je nakonfigurováno pro práci s předplatným Azure. Je nainstalován modul Azure v *Program Files* adresáře a je automaticky importován při každé použití konzoly Azure PowerShell.

    Pokud dáváte přednost práci v programu jiného hostitele, jako je například Windows PowerShell ISE, je nutné použít [Import-Module](https://go.microsoft.com/fwlink/?LinkID=141553) rutiny import modulu Azure nebo použijte příkaz v modulu Azure pro spuštění automatického importu modulu.
2. Otevřete prostředí Azure PowerShell s **spustit jako správce** možnost.
3. Spustit [Set-ExecutionPolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) rutiny nastavte zásady spouštění prostředí Azure PowerShell `RemoteSigned`. Zadejte **Y** (pro Ano) dokončení změny zásad.

    [!code-console[Main](the-fix-it-sample-application/samples/sample20.cmd)]

    Toto nastavení umožňuje spustit místní skripty, které nejsou digitálně podepsané. (Můžete také nastavit zásady spouštění `Unrestricted`, který by eliminují nutnost použití kroku odblokovat později, ale z bezpečnostních důvodů to nedoporučujeme.)
4. Spustit `Add-AzureAccount` rutiny prostředí PowerShell nastavit přihlašovací údaje pro svůj účet.

    [!code-console[Main](the-fix-it-sample-application/samples/sample21.cmd)]

    Tyto přihlašovací údaje vyprší po určitou dobu a budete muset znovu spustit `Add-AzureAccount` rutiny. Tato e kniha probíhá zápis, je časový limit před vypršením platnosti pověření 12 hodin.
5. Pokud máte více předplatných, použijte rutinu Select-AzureSubscription pro určení předplatného, kterou chcete vytvořit v testovacím prostředí.
6. Importovat certifikát správy pro stejné předplatné Azure pomocí `Get-AzurePublishSettingsFile` a `Import-AzurePublishSettingsFile` rutiny. První z těchto rutin stáhne soubor certifikátu a v druhém zadejte umístění tohoto souboru, aby bylo možné importovat. > [!IMPORTANT]
   > Zachovat stažený soubor do bezpečného umístění, nebo odstranit když jste hotovi s ní, protože obsahuje certifikát, který lze použít ke správě služeb Azure.

    [!code-console[Main](the-fix-it-sample-application/samples/sample22.cmd)]

    Certifikát se používá pro volání rozhraní REST API, který zjišťuje vývojovém počítači IP adresu, aby bylo možné nastavit pravidlo brány firewall na serveru služby SQL Database.
7. Spustit [nastavení umístění](https://go.microsoft.com/fwlink/p/?linkid=293912) rutiny (aliasy jsou `cd`, `chdir`, a `sl`) a přejděte do adresáře, který obsahuje skripty. (Se nachází v *automatizace* složky ve složce řešení opravte ji.) Cesta uvést v uvozovkách, pokud některé názvy directory obsahovat mezery. Chcete-li například přejděte do `c:\Sample Apps\FixIt\Automation` adresář můžete zadat následující příkaz:

    [!code-console[Main](the-fix-it-sample-application/samples/sample23.cmd)]
8. Chcete-li povolit spusťte tyto skripty prostředí Windows PowerShell, použijte [odblokovat soubor](https://go.microsoft.com/fwlink/p/?linkid=294021) rutiny. (Tyto skripty jsou zablokovány, protože byly staženy z Internetu.)

    > [!WARNING]
    > Zabezpečení – dřív, než spustíte `Unblock-File` skript nebo spustitelný soubor, v poznámkovém bloku otevřete soubor, zkontrolujte příkazy a ověřte, že neobsahuje žádný škodlivý kód.

    Například následující příkaz spustí `Unblock-File` na všechny skripty v aktuálním adresáři rutinu.

    [!code-console[Main](the-fix-it-sample-application/samples/sample24.cmd)]
9. K vytvoření webové aplikace pro základ (žádné fronty zpracování) opravu aplikace, spusťte skript vytvoření prostředí.

    Požadované `Name` parametr určuje název databáze a také se používá pro účet úložiště, který vytvoří skript. Název musí být globálně jedinečný v doméně azurewebsites.net. Pokud zadáte název, který není jedinečný, jako je automatickou nebo testovací (nebo i jako v příkladu fixitdemo) `New-AzureWebsite` rutina selže k interní chybě, která generuje sestavy ke konfliktu. Skript převede název malými písmeny, abyste dosáhli souladu s požadavky na název pro webové aplikace, účty úložiště a databáze.

    Požadované `SqlDatabasePassword` parametr určuje heslo pro účet správce, který se vytvoří pro databázi SQL. Neobsahují speciální znaky XML v hesle (&amp; &lt; &gt; ;). Jedná se o omezení toho, jak byly napsány skripty, nikoliv omezení Azure.

    Pokud chcete vytvořit webovou aplikaci s názvem "fixitdemo" a použijete heslo správce systému SQL Server, které "Passw0rd1", můžete například zadat následující příkaz:

    [!code-console[Main](the-fix-it-sample-application/samples/sample25.cmd)]

    Název musí být jedinečný v doméně azurewebsites.net a heslo musí splňovat požadavky na databázi SQL pro složitost hesla. (V příkladu Passw0rd1 splňovat požadavky.)

    Všimněte si, že příkaz začíná ". \". Abyste líp zabránili škodlivý spouštění skriptů, vyžaduje prostředí Windows PowerShell, zadejte plně kvalifikovanou cestu k souboru skriptu, když spustíte skript. Tečku můžete použít k označení aktuálnímu adresáři (".\") nebo zadejte plně kvalifikovanou cestu, jako třeba:

    [!code-console[Main](the-fix-it-sample-application/samples/sample26.cmd)]

    Další informace o skriptu, použijte `Get-Help` rutiny.

    [!code-console[Main](the-fix-it-sample-application/samples/sample27.cmd)]

    Můžete použít `Detailed`, `Full`, `Parameters`, a `Examples` parametry rutiny Get-Help vyfiltrujete v nápovědě, která je vrácena.

    Pokud skript selže nebo generuje chyby, jako je například "New-AzureWebsite: volání Set-AzureSubscription a Select-AzureSubscription nejdřív" nemusí dokončili konfigurace prostředí Azure PowerShell.

    Po dokončení skriptu portálu pro správu Azure můžete najdete v části prostředky, které byly vytvořeny, jak je znázorněno [automatizovat všechno](automate-everything.md) kapitoly.
10. Chcete-li nasadit automatickou projektu do nové prostředí Azure, použijte *AzureWebsite.ps1* skriptu. Příklad:

    [!code-console[Main](the-fix-it-sample-application/samples/sample28.cmd)]

    Při nasazení se provádí, otevře se prohlížeč s opravte ji běžící v Azure.

<a id="troubleshooting"></a>
## <a name="troubleshooting-the-windows-powershell-scripts"></a>Řešení potíží se skripty prostředí Windows PowerShell

Oprávnění souvisejí většiny běžných chyb zjištěných při spuštění těchto skriptů. Ujistěte se, že `Add-AzureAccount` a `Import-AzurePublishSettingsFile` proběhly úspěšně a že jste použili pro stejné předplatné Azure. I když `Add-AzureAccount` byla úspěšná možná budete muset znovu spustit. Oprávnění přidal `Add-AzureAccount` vyprší za 12 hodin.

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a>Odkaz na objekt není nastaven na instanci objektu.

Pokud skript vrátí chyby, například "Odkaz na objekt není nastavený na instanci objektu," to znamená, že prostředí Windows PowerShell nelze najít objekt procesu (Toto je výjimka odkazu s hodnotou null), spusťte `Add-AzureAccount` rutiny a zkuste to znovu skript.

[!code-console[Main](the-fix-it-sample-application/samples/sample29.cmd)]

### <a name="internalerror-the-server-encountered-an-internal-error"></a>InternalError: Serveru došlo k vnitřní chybě.

`New-AzureWebsite` Rutina vrátí k vnitřní chybě, pokud název není v doméně azurewebsites.net jedinečný. Chcete-li vyřešit chyby, použijte jinou hodnotu pro název, který je v parametru Name *New-AzureWebsiteEnv.ps1*.

[!code-console[Main](the-fix-it-sample-application/samples/sample30.cmd)]

### <a name="restarting-the-script"></a>Restartovat skript

Pokud budete muset restartovat *New-AzureWebsiteEnv.ps1* skriptu protože předtím, než ho vytisknout zpráva "Skriptu byla dokončena", můžete chtít odstranit prostředky, které skript vytvořený dříve, než se zastavila. Například pokud skript již vytvořili ContosoFixItDemo webovou aplikaci a znovu spusťte skript se stejným názvem, skript selže, protože název je používán.

Pokud chcete zjistit prostředky, ke kterým skript vytvořený předtím, než ho zastavit, použijte následující rutiny:

- `Get-AzureWebsite`
- `Get-AzureSqlDatabaseServer`
- `Get-AzureSqlDatabase`: Ke spuštění této rutiny, prostřednictvím kanálu název databázového serveru do `Get-AzureSqlDatabase`:  
    `Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`

Pokud chcete odstranit tyto prostředky, použijte následující příkazy. Všimněte si, že pokud odstraníte databázový server, je automaticky odstraňovat databáze přidružené k serveru.

- `Get-AzureWebsite -Name <WebsiteName> | Remove-AzureWebsite`
- `Get-AzureSqlDatabase -Name <DatabaseName> -ServerName <DatabaseServerName> | Remove-SqlAzureDatabase`
- `Get-AzureSqlDatabaseServer | Remove-AzureSqlDatabaseServer`

<a id="deployqueues"></a>
## <a name="how-to-deploy-the-app-with-queue-processing-to-azure-app-service-web-apps-and-an-azure-cloud-service"></a>Postup nasazení aplikace s frontou zpracování Azure App Service Web Apps a cloudové služby Azure

Povolit front, proveďte následující změny v souboru MyFixIt\Web.config. V části `appSettings`, změňte hodnotu `UseQueues` hodnotu "true": 

[!code-xml[Main](the-fix-it-sample-application/samples/sample31.xml)]

Pak nasadíte aplikaci MVC do webové aplikace v Azure App Service, jak je popsáno [starší](#deploybase).

Dál vytvořte novou službu cloudu Azure. Skripty, které jsou součástí aplikace opravte nevytvářejí ani nasazení cloudové služby, je nutné použít pro tento portál Azure. Na portálu, klikněte na tlačítko **nový** -- **výpočetní** – **Cloudová služba** -- **rychle vytvořit**a potom zadejte adresu URL a umístění datového centra. Použijte stejné datové centrum, kde nasadíte webovou aplikaci.

![](the-fix-it-sample-application/_static/image1.png)

Před nasazením cloudové služby, je potřeba aktualizovat některé konfigurační soubory.

V MyFixIt.WorkerRoler\app.config v části `connectionStrings`, nahraďte hodnotu `appdb` připojovací řetězec skutečným připojovacím řetězcem pro databázi SQL. Z portálu můžete získat připojovací řetězec. Na portálu, klikněte na tlačítko **databází SQL** - **appdb** - **zobrazení SQL databázové připojovací řetězce pro ADO .net, rozhraní ODBC, PHP a JDBC**. Připojovací řetězec ADO.NET zkopírujte a vložte hodnotu do souboru app.config. Nahraďte "{vaší\_heslo\_zde}" pomocí hesla databáze. (Za předpokladu, že jste použili skripty nasazení aplikace MVC, jste zadali heslo databáze v `SqlDatabasePassword` parametr skriptu.)

Výsledek by měl vypadat asi takto:

[!code-xml[Main](the-fix-it-sample-application/samples/sample32.xml)]

Ve stejném souboru MyFixIt.WorkerRoler\app.config pod `appSettings`, nahraďte tyto dvě hodnoty zástupný symbol pro účet úložiště Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample33.xml?highlight=2-3)]

Můžete získat přístupový klíč z portálu. V tématu [jak spravovat účty úložiště](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).

V MyFixItCloudService\ServiceConfiguration.Cloud.cscfg nahraďte stejné dvě hodnoty zástupné symboly pro účet úložiště Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample34.xml?highlight=3)]

Nyní jste připraveni k nasazení cloudové služby. V řešení prozkoumat, klikněte pravým tlačítkem na projekt MyFixItCloudService a vyberte **publikovat**. Další informace najdete v tématu "[Nasaďte aplikaci do Azure](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)", což je v rámci 2 [v tomto kurzu](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36).

> [!div class="step-by-step"]
> [Předchozí](more-patterns-and-guidance.md)
