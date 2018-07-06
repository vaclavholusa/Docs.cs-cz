---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
title: 'Dodatek: Oprava ji ukázkovou aplikaci (sestavování skutečných cloudových aplikací s Azure) | Dokumentace Microsoftu'
author: MikeWasson
description: Vytváření reálného světa cloudových aplikací s Azure e kniha je založená na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které se dají mu...
ms.author: aspnetcontent
ms.date: 06/12/2014
ms.assetid: 1bc333c5-f096-4ea7-b170-779accc21c1a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
msc.type: authoredcontent
ms.openlocfilehash: 6a0c17f37ed426c04d2b21fd864337d4339fe573
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824079"
---
<a name="appendix-the-fix-it-sample-application-building-real-world-cloud-apps-with-azure"></a>Dodatek: Oprava ji ukázkovou aplikaci (sestavování skutečných cloudových aplikací s Azure)
====================
podle [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Petr Dykstra](https://github.com/tdykstra)

[Stáhněte si opravu ho projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)

> **Vytváření reálného světa cloudových aplikací s Azure** e knihy je založena na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které vám pomůžou být úspěšný vývoj webových aplikací v cloudu. Informace o e kniha najdete v tématu [první kapitoly](introduction.md).


Tento dodatek ke Cloudovým aplikacím reálného světa vytváření s Azure e kniha obsahuje následující oddíly, které poskytují další informace o Fix It ukázkové aplikace, můžete si stáhnout:

- [Známé problémy](#knownissues)
- [Osvědčené postupy](#bestpractices)
- [Jak spustit aplikaci v sadě Visual Studio v místním počítači](#run-in-vs)
- [Jak nasadit základní aplikaci do Azure App Service Web Apps pomocí skriptů Windows Powershellu](#deploybase)
- [Řešení potíží se skripty prostředí Windows PowerShell](#troubleshooting)
- [Jak nasadit aplikaci s frontou zpracování pro Azure App Service Web Apps a Azure Cloud Service](#deployqueues)

<a id="knownissues"></a>
## <a name="known-issues"></a>Známé problémy

Aplikace Fix It byla původně vyvinuta k ilustraci co nejsnáze některé vzory uvedené v této e kniha. Ale protože elektronickou knihu o vytváření skutečných aplikací, jsme podroben kód opravit kontrolu a testování procesu podobný co jsme byste to udělali pro verze softwaru. Našli jsme několik problémů a stejně jako u všech aplikací v reálném světě, některé z nich jsme vyřešili a některé z nich jsme odložena na novější verzi.

Následující seznam obsahuje problémy, které mělo by se řešit v produkční aplikace, ale jeden z důvodu nebo jiného, který jsme se rozhodli adresu v počáteční verzi ukázková aplikace Fix It.

### <a name="security"></a>Zabezpečení

- Ujistěte se, že úlohy nelze přiřadit k neexistující vlastníka.
- Ujistěte se, že můžete jenom zobrazit a upravit úlohy, které jste vytvořili nebo které jsou vám přiřazeny.
- Pro přihlašovací stránky a soubory cookie pro ověřování použijte protokol HTTPS.
- Zadejte časový limit pro soubory cookie pro ověřování.

### <a name="input-validation"></a>Ověření vstupu

Obecně byste udělali produkční aplikace více ověřování vstupu než aplikace Fix It. Například velikost obrázku / povolený pro nahrávání by měla být omezena velikost souboru obrázku.

### <a name="administrator-functionality"></a>Funkce správce

Správce by měl být změnit vlastnictví na úlohy existující. Tvůrce úkolu může například ponechte společnost opuštění nikdo s autoritou udržovat úkolu, pokud je povolený přístup pro správu.

### <a name="queue-message-processing"></a>Zpracování zpráv fronty

Zpracování v aplikaci opravit fronty zpráv bylo navrženo jako jednoduchá k ilustraci vzor založený na frontě práce s minimální část kódu. Tento jednoduchý kód by být dostačující pro skutečné produkční aplikace.

- Kód není zaručeno, že každá zpráva fronty bude zpracováno nejvýše jednou. Když dostanete zprávu z fronty, je časový limit, během které je neviditelné do jiných naslouchacích procesů fronty zprávu. Pokud časový limit vyprší před odstraněním zprávy, se zpráva zobrazí znovu. Proto pokud se instance role pracovního procesu, kterou zůstane delší dobu zpracování zprávy, je teoreticky může pro tutéž zprávu zpracovat dvakrát, což vede k duplicitní úloh v databázi. Další informace o tomto problému najdete v tématu [pomocí front Azure Storage](https://msdn.microsoft.com/library/ff803365.aspx#sec7).
- Logiku dotazování fronta může být cenově výhodnější podle dávkování načítání zpráv. Pokaždé, když voláte [CloudQueue.GetMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx), je transakční náklady. Místo toho můžete volat [CloudQueue.GetMessagesAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (Poznámka: množném čísle "), který získá více zpráv v rámci jedné transakce. Náklady na transakce pro front Azure Storage jsou velmi nízký, proto dopad na náklady není podstatné ve většině scénářů.
- Těsné smyčce v kódu zpracování zpráv fronty způsobí, že spřažení procesoru, která nevyužívá efektivně vícejádrových virtuálních počítačů. Návrh s lepší byste použili paralelismus úloh k paralelnímu spuštění několika úloh s modifikátorem async.
- Zpracování fronty zpráv má pouze základní výjimku zpracování. Například kód nezpracuje [nezpracovatelné zprávy](https://msdn.microsoft.com/library/ms789028.aspx). (Při zpracování zprávy způsobí výjimku, je nutné protokolovat chyby a odstranění zprávy, nebo roli pracovního procesu se pokusí znovu zpracován a smyčka bude pokračovat po neomezenou dobu.)

### <a name="sql-queries-are-unbounded"></a>Dotazy SQL jsou bez vazby

Aktuální Fix It kód umístí bez omezení na tom, kolik řádků může vrátit dotazech pro Index stránky. Pokud velkého objemu úloh je zadán do databáze, velikost výsledný přijetí seznamů může způsobit problémy s výkonem. Toto řešení je k implementaci stránkování. Příklad najdete v tématu [řazení, filtrování a stránkování s rozhraním Entity Framework v aplikaci ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md).

### <a name="view-models-recommended"></a>Zobrazit modely doporučuje

Aplikace Fix It používá třídu entity FixItTask k předávání informací mezi kontrolerem a zobrazení. Osvědčeným postupem je použití modelů zobrazení. Navržené s ohledem na co je potřeba pro trvalost dat, zatímco model zobrazení nelze navrhovat pro prezentaci dat model domény (například třídy entit FixItTask). Další informace najdete v tématu [12 osvědčených postupů technologie ASP.NET MVC](http://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx).

### <a name="secure-image-blob-recommended"></a>Doporučuje objekt blob zabezpečené image

Obchody Fix It nahraných obrázků jako public, což znamená, že každý, kdo najde adrese URL přístup k bitové kopie. Image šlo zabezpečit namísto veřejná.

### <a name="no-powershell-automation-scripts-for-queues"></a>Žádný automatizační skripty prostředí PowerShell pro fronty

Ukázkové skripty Powershellu automation byly vytvořeny pouze pro základní verzi Fix It, který spouští kompletně v Azure App Service Web Apps. Jsme neposkytovaly skripty pro nastavení a nasazení do webové aplikace a cloudové služby prostředí požadovanou ke zpracování fronty.

### <a name="special-handling-for-html-codes-in-user-input"></a>Zvláštní zacházení HTML kódy vstup uživatele

ASP.NET automaticky brání mnoho způsobů, ve kterých uživatelé se zlými úmysly se může pokusit útoky skriptování napříč weby tak, že zadáte skript uživatele textová vstupní pole. A MVC `DisplayFor` pomocné rutiny, které slouží k zobrazení úkolů produkty a poznámky automaticky umístí kódování HTML hodnoty, které se odešle do prohlížeče. Ale v produkční aplikaci můžete chtít provést další míry. Další informace najdete v tématu [žádost o ověření v technologii ASP.NET](https://msdn.microsoft.com/library/hh882339.aspx).

<a id="bestpractices"></a>
## <a name="best-practices"></a>Osvědčené postupy

Toto jsou některé problémy, které byly odstraněny po se objeví ve službě revize kódu a testování k původní verzi aplikace Fix It. Některé byly způsobeny původní kodér nebude vědět, zejména osvědčeným postupem je některé jednoduše protože kód byl zapsán rychle a není určen pro verze softwaru. V případě, že je něco, co jsme se naučili v této revizi jsme už výpis problémy většinou neřeší a testování, které mohou být užitečné ostatním uživatelům, kteří jsou také vývoji webových aplikací.

### <a name="dispose-the-database-repository"></a>Uvolnění úložiště databáze

`FixItTaskRepository` Třídy musí disponovat Entity Framework `DbContext` instance. To jsme implementací `IDisposable` v `FixItTaskRepository` třídy:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample1.cs)]

Všimněte si, že bude automaticky dispose AutoFac `FixItTaskRepository` instance, proto nepotřebujeme explicitně uvolnit.

Další možností je odebrat `DbContext` členské proměnné z `FixItTaskRepository`a místo toho vytvořte místní `DbContext` proměnné v rámci každé metody úložiště uvnitř `using` příkazu. Příklad:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample2.cs)]

### <a name="register-singletons-as-such-with-di"></a>Registraci jednotlivých prvků v důsledku DI

Od jenom jednu instanci `PhotoService` třídy a `Logger` třídy je potřeba, by měly být tyto třídy [zaregistrovaný jako jediné instance pro vkládání závislostí](https://code.google.com/p/autofac/wiki/InstanceScope) v *DependenciesConfig.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample3.cs?highlight=1,3)]

### <a name="security-dont-show-error-details-to-users"></a>Zabezpečení: Není uživatelům zobrazit podrobnosti o chybě

Původní aplikace Fix It neměli mít obecné chybové stránky a právě umožní všechny výjimky bublinu až do uživatelského rozhraní, takže některé výjimky, jako jsou chyby připojení databáze by mohlo způsobit úplné trasování zásobníku je zobrazená v prohlížeči. Podrobné informace o chybě můžete někdy usnadnění útoky uživateli se zlými úmysly. Toto řešení je protokolovat podrobnosti o výjimce a zobrazit chybovou stránku pro uživatele, který neobsahuje podrobnosti o chybě. Aplikace Fix It byl již protokolování a aby bylo možné zobrazit chybovou stránku, přidali jsme `<customErrors mode=On>` v souboru Web.config.

[!code-xml[Main](the-fix-it-sample-application/samples/sample4.xml?highlight=2)]

Ve výchozím nastavení to způsobí, že *Views\Shared\Error.cshtml* má být zobrazen pro chyby. Můžete přizpůsobit *Error.cshtml* nebo vytvořte zobrazení vlastní chybové stránky a přidejte `defaultRedirect` atribut. Můžete také určit různé chybové stránky pro konkrétní chyby.

### <a name="security-only-allow-a-task-to-be-edited-by-its-creator"></a>Zabezpečení: Povolte pouze na upravit jeho tvůrcem úlohy

Na stránce rejstříku řídicího panelu zobrazuje pouze úlohy vytvořené pomocí přihlášeného uživatele, ale uživatel se zlými úmysly může vytvořit adresu URL s ID úkolu jiného uživatele. Přidali jsme kód v *DashboardController.cs* vrátit kód 404 v takovém případě:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample5.cs?highlight=9-14,24-29)]

### <a name="dont-swallow-exceptions"></a>Není spolknout výjimky

Původní aplikace Fix It právě vrátila hodnotu null po přihlášení výjimku, která je výsledkem dotazu SQL:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample6.cs?highlight=4)]

To by být zobrazovat uživateli, jako kdyby dotazu bylo úspěšné, ale právě nevrátil žádné řádky. Řešení je pro opětovné vyvolání výjimky po zachycení a protokolování:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample7.cs)]

### <a name="catch-all-exceptions-in-worker-roles"></a>Zachytit všechny výjimky v rolích pracovního procesu

Způsobí, že všechny neošetřené výjimky v roli pracovního procesu virtuálního počítače k provedení recyklace, proto chcete zabalit všechno, co dělat v bloku try-catch a zpracovávat všechny výjimky.

### <a name="specify-length-for-string-properties-in-entity-classes"></a>Zadejte hodnotu pro vlastnosti řetězce v tříd entit

Aby bylo možné zobrazit jednoduchého kódu, původní verze aplikace Fix It neurčili délek polí entity FixItTask a výsledkem byly definované jako varchar(max) v databázi. Uživatelské rozhraní díky tomu bude přijímat téměř jakýkoli objem vstup. Určení sad omezení délky, které se vztahují jak na uživatele vstup webové stránky a velikost sloupce v databázi:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample8.cs?highlight=4,7,10,12,14)]

### <a name="mark-private-members-as-readonly-when-they-arent-expected-to-change"></a>Soukromé členy označit jako jen pro čtení se neočekává změna

Například v `DashboardController` instance třídy `FixItTaskRepository` se vytvoří a neočekává se změnit, takže jsme definovali jako [jen pro čtení](https://msdn.microsoft.com/library/acdd6hb7.aspx).

[!code-csharp[Main](the-fix-it-sample-application/samples/sample9.cs?highlight=3)]

### <a name="use-listany-instead-of-listcount-gt-0"></a>Pomocí seznamu. Metoda any() namísto seznamu. Count() &gt; 0

Pokud budete vás zajímá, zda jeden nebo více položek v seznamu podle zadaných kritérií, použijte [všechny](https://msdn.microsoft.com/library/bb534972.aspx) metody, protože se vrací jako položky kritériím přizpůsobení nenajde, že `Count` metoda má vždy pro iteraci přes všechny položky. Řídicí panel *Index.cshtml* soubor původně měli tento kód:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample10.cshtml)]

Jsme změnili na toto:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample11.cshtml?highlight=1)]

### <a name="generate-urls-in-mvc-views-using-mvc-helpers"></a>Generování adresy URL v zobrazení MVC pomocí pomocné rutiny MVC

Pro **opravte ji vytvořit** tlačítko na domovské stránce Fix It aplikace pevné programový anchor element:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample12.cshtml)]

Pro zobrazení nebo akce odkazy tímto způsobem je vhodnější použít [Url.Action](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx) pomocné rutiny HTML, například:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample13.cshtml)]

### <a name="use-taskdelay-instead-of-threadsleep-in-worker-role"></a>Nahrazujícím Task.Delay Thread.Sleep v roli pracovního procesu

Vloží nový projekt šablony `Thread.Sleep` v ukázce kódu pro roli pracovního procesu, ale způsobuje vlákna do režimu spánku, může způsobit fondu vláken nejde vytvořit podřízený další zbytečné vlákna. Který můžete vyhnout použitím [Task.Delay](https://msdn.microsoft.com/library/hh139096.aspx) místo.

[!code-csharp[Main](the-fix-it-sample-application/samples/sample14.cs?highlight=11)]

### <a name="avoid-async-void"></a>Vyhněte se async void

Pokud asynchronní metody nemusí vracet hodnotu, vraťte se `Task` typ spíše než `void`.

Tento příklad je z `FixItQueueManager` třídy: 

[!code-csharp[Main](the-fix-it-sample-application/samples/sample15.cs)]

Měli byste použít `async void` pouze pro obslužné rutiny události nejvyšší úrovně. Pokud definujete metodu jako `async void`, nelze volající **await** metodu nebo zachytit žádné výjimky, metoda vyvolá. Další informace najdete v tématu [osvědčené postupy v Asynchronous Programming](https://msdn.microsoft.com/magazine/jj991977.aspx). 

### <a name="use-a-cancellation-token-to-break-from-worker-role-loop"></a>Použít token zrušení pro zrušení volání ze smyčky role pracovního procesu

Obvykle **spustit** nekonečná smyčka obsahuje metodu role pracovního procesu. Při zastavení role pracovního procesu [RoleEntryPoint.OnStop](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) metoda je volána. By pomocí této metody můžete zrušit práce, která se provádí uvnitř **spustit** metoda a ukončení bez výpadku. V opačném případě může být proces ukončen provádí operaci.

### <a name="opt-out-of-automatic-mime-sniffing-procedure"></a>Vyjádřit výslovný nesouhlas MIME pro analýzu sítě automatický

V některých případech se aplikace Internet Explorer hlásí typu MIME liší od typu určeného webového serveru. Například pokud aplikace Internet Explorer najde HTML obsahu v souboru dodávají s odpovědi hlavičku HTTP Content-Type: text/plain, Internet Explorer určuje, že obsah má být vykreslen jako kódu HTML. Bohužel tento "MIME-pro analýzu sítě" může také vést k problémům zabezpečení pro servery, které hostují nedůvěryhodný obsah. K boji proti tomuto problému, Internet Explorer 8 provedl řadu změn do kódu stanovení typ MIME a umožňuje vývojářům aplikací [vyjádřit výslovný nesouhlas MIME pro analýzu sítě](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx). Následující kód byl přidán do *Web.config* souboru.

[!code-xml[Main](the-fix-it-sample-application/samples/sample16.xml?highlight=2-7)]

### <a name="enable-bundling-and-minification"></a>Povolit sdružování a minifikace

Když Visual Studio vytvoří nový webový projekt, není ve výchozím nastavení povoleno sdružování a minifikace souborů JavaScriptu. Přidali jsme psát kód v BundleConfig.cs:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample17.cs?highlight=9)]

### <a name="set-an-expiration-time-out-for-authentication-cookies"></a>Nastavení vypršení časového limitu pro soubory cookie pro ověřování

Ve výchozím nastavení soubory cookie pro ověřování vyprší za dva týdny. Kratší doba je bezpečnější. Můžete změnit toto nastavení v *StartupAuth.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample18.cs?highlight=4-5)]

<a id="run-in-vs"></a>
## <a name="how-to-run-the-app-from-visual-studio-on-your-local-computer"></a>Jak spustit aplikaci v sadě Visual Studio v místním počítači

Existují dva způsoby, jak spouštět aplikace Fix It:

- Spusťte základní aplikaci, která zapisuje přímo do SQL database nové úkoly.
- Spuštění aplikace pomocí fronty a back-end službu k vytvoření úlohy. Model fronty je popsáno v kapitole [vzor založený na frontě pracovní](queue-centric-work-pattern.md).

<a id="runbase"></a>
### <a name="run-the-base-application"></a>Spustit základní aplikace

1. Nainstalujte [Visual Studio 2013 nebo Visual Studio Express 2013 for Web](https://www.visualstudio.com/downloads).
2. Nainstalujte [Azure SDK pro .NET pro Visual Studio 2013.](https://go.microsoft.com/fwlink/p/?linkid=323510&amp;clcid=0x409)
3. Stáhněte si soubor .zip z [galerie kódů MSDN](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4).
4. V Průzkumníku souborů klikněte pravým tlačítkem na soubor .zip a klikněte na tlačítko Vlastnosti a pak v okně Vlastnosti klikněte na odblokovat.
5. Rozbalte tento soubor.
6. Poklikejte na soubor .sln a spusťte sadu Visual Studio.
7. V nabídce Nástroje klikněte na správce balíčků knihoven a pak Konzola správce balíčků.
8. V balíčku správce konzoly (konzolu PMC), kliknutím na tlačítko Obnovit.
9. Ukončení sady Visual Studio.
10. Spustit [emulátoru úložiště Azure](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx).
11. Restartujte sadu Visual Studio, otevřete soubor řešení uzavřeny v předchozím kroku.
12. Ujistěte se, že projekt automatickou je nastaven jako spouštěný projekt a pak stiskněte CTRL + F5 ke spuštění projektu.

<a id="queueslocal"></a>
### <a name="run-the-application-with-queue-processing"></a>Spuštění aplikace se zpracováním fronty

1. Postupujte podle pokynů pro [spustit základní aplikace](#runbase)a pak zavřete prohlížeč a zavřete sadu Visual Studio.
2. Spusťte Visual Studio s oprávněními správce. (Budete používat emulátor výpočtů v Azure, a, která vyžaduje oprávnění správce.)
3. V aplikaci *Web.config* soubor *MyFixIt* project (webový projekt), změňte hodnotu vlastnosti `appSettings/UseQueues` "true": 

    [!code-console[Main](the-fix-it-sample-application/samples/sample19.cmd?highlight=3)]
4. Pokud [emulátoru úložiště Azure](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx) není stále spuštěna, spusťte ji znovu.
5. Spusťte automatickou webový projekt a projekt MyFixItCloudService současně.

    Pomocí sady Visual Studio 2013:

   1. Stisknutím klávesy F5 spusťte automatickou projektu.
   2. V **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt MyFixItCloudService a potom klikněte na tlačítko **ladění** -- **spustit novou instanci**.

      Pomocí Visual Studio 2013 Express pro Web:

   3. V Průzkumníku řešení klikněte pravým tlačítkem myši na automatickou řešení a vyberte **vlastnosti**.
   4. Vyberte **více projektů po spuštění**...
   5. V **akce** vyberte rozevírací seznam v části MyFixIt a MyFixItCloudService, **Start**.
   6. Klikněte na tlačítko **OK**.
   7. Stisknutím klávesy F5 spusťte obou projektů.

      Při spuštění projektu MyFixItCloudService Visual Studio spustí emulátoru Azure compute. V závislosti na konfiguraci brány firewall můžete povolit emulátor přes bránu firewall.

<a id="deploybase"></a>
## <a name="how-to-deploy-the-base-app-to-azure-app-service-web-apps-by-using-the-windows-powershell-scripts"></a>Jak nasadit základní aplikaci do Azure App Service Web Apps pomocí skriptů Windows Powershellu

Pro ilustraci [automatizovat všechno](automate-everything.md) se vzor, aplikace Fix It dodává s skripty, které nastavení prostředí v Azure a nasazení projektu do nového prostředí. Následující pokyny vysvětlují, jak pomocí skriptů.

Pokud chcete spustit v Azure bez použití fronty a provedené změny místní spuštění s frontami, nezapomeňte že nastavit hodnotu nastavení aplikace UseQueues zpět na hodnotu false, než budete pokračovat v následujících pokynech.

Tyto pokyny předpokládají jste si stáhli a spustit řešení vyřešit místně, a že máte Azure účtu nebo máte předplatné Azure, které máte oprávnění ke správě.

1. Nainstalujte **prostředí Azure PowerShell** konzoly. Pokyny najdete v tématu [instalace a konfigurace Azure Powershellu](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).

    Tato vlastní konzola je nakonfigurováno pro práci s vaším předplatným Azure. Modul Azure je nainstalovaný v *Program Files* adresáře a je automaticky importován při každé použití konzoly Azure Powershellu.

    Pokud chcete raději pracovat v různých hostitelského programu, jako je Windows PowerShell ISE, nezapomeňte použít [Import-Module](https://go.microsoft.com/fwlink/?LinkID=141553) rutiny pro import modulu Azure nebo pomocí příkazu v modulu Azure můžete aktivovat automatické importování modulu.
2. Otevřete prostředí Azure PowerShell s **spustit jako správce** možnost.
3. Spustit [Set-ExecutionPolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) rutiny nastavte zásady spouštění prostředí Azure PowerShell `RemoteSigned`. Zadejte **Y** (pro Ano) k dokončení změny zásad.

    [!code-console[Main](the-fix-it-sample-application/samples/sample20.cmd)]

    Toto nastavení umožňuje spustit místní skripty, které nejsou digitálně podepsané. (Můžete také nastavit zásady spouštění `Unrestricted`, což by eliminuje nutnost krok odblokovat později, ale z bezpečnostních důvodů to nedoporučujeme.)
4. Spustit `Add-AzureAccount` rutina pro nastavení prostředí PowerShell pomocí přihlašovacích údajů pro váš účet.

    [!code-console[Main](the-fix-it-sample-application/samples/sample21.cmd)]

    Tyto přihlašovací údaje vyprší po určitou dobu a budete muset znovu spustit `Add-AzureAccount` rutiny. Tato elektronická kniha se zápisem, časový limit před vypršením platnosti přihlašovacích údajů je 12 hodin.
5. Pokud máte více předplatných, pomocí rutiny Select-AzureSubscription určete předplatné, které chcete vytvořit testovací prostředí v.
6. Importovat certifikát správy pro stejné předplatné Azure s použitím `Get-AzurePublishSettingsFile` a `Import-AzurePublishSettingsFile` rutiny. První z těchto rutin stáhne soubor certifikátu, a v druhý zadejte umístění souborů aby bylo možné importovat. > [!IMPORTANT]
   > Zachovat stažený soubor do bezpečného umístění nebo ho odstranit až budete hotovi s ním, protože obsahuje certifikát, který slouží ke správě služeb Azure.

    [!code-console[Main](the-fix-it-sample-application/samples/sample22.cmd)]

    Tento certifikát se používá pro volání rozhraní REST API, které zjistí vývojového počítače IP adresu nastavit pravidlo brány firewall na serveru SQL Database.
7. Spustit [nastavení umístění](https://go.microsoft.com/fwlink/p/?linkid=293912) rutiny (aliasy jsou `cd`, `chdir`, a `sl`) přejděte do adresáře, který obsahuje skripty. (Nacházejí v *automatizace* složku ve složce řešení opravte ji.) Pozastavena cesty v uvozovkách, pokud se některé názvy adresářů obsahovat mezery. Například, přejděte `c:\Sample Apps\FixIt\Automation` adresáře můžete například zadat následující příkaz:

    [!code-console[Main](the-fix-it-sample-application/samples/sample23.cmd)]
8. Chcete-li povolit Windows Powershellu spusťte tyto skripty, použijte [odblokovat soubor](https://go.microsoft.com/fwlink/p/?linkid=294021) rutiny. (Tyto skripty jsou blokovány, protože byly staženy z Internetu.)

    > [!WARNING]
    > Zabezpečení – dřív, než spustíte `Unblock-File` na skript nebo spustitelný soubor, otevřete soubor v poznámkovém bloku, zkontrolujte příkazy a ověřte, že neobsahuje žádný škodlivý kód.

    Například následující příkaz spustí `Unblock-File` rutiny v všechny skripty v aktuálním adresáři.

    [!code-console[Main](the-fix-it-sample-application/samples/sample24.cmd)]
9. Vytvoření webové aplikace pro základní (žádné fronty zpracování) opravit aplikaci spuštěním skriptu pro vytváření prostředí.

    Požadované `Name` parametr určuje název databáze a slouží také pro účet úložiště, které skript vytvoří. Název musí být globálně jedinečný v rámci domény azurewebsites.net. Pokud zadáte název, který není jedinečný, jako je automatickou nebo testovací (nebo i jako v příkladu fixitdemo) `New-AzureWebsite` rutina selže s interní chybou, která se ohlásila konflikt. Skript převede název malými písmeny – pro dosažení souladu s požadavky na název pro webové aplikace, účty úložiště a databáze.

    Požadované `SqlDatabasePassword` parametr určuje heslo pro účet správce, která bude vytvořena pro službu SQL Database. Nezahrnují speciální znaky XML v hesle (&amp; &lt; &gt; ;). Jedná se omezení tak, jak tyto skripty byly napsány, ne o omezení Azure.

    Například pokud chcete vytvořit webovou aplikaci s názvem "fixitdemo" a použijte heslo správce systému SQL Server "Passw0rd1", můžete například zadat následující příkaz:

    [!code-console[Main](the-fix-it-sample-application/samples/sample25.cmd)]

    Název musí být jedinečný v doméně azurewebsites.net, a heslo musí splňovat požadavky na SQL Database pro složitost hesla. (V příkladu Passw0rd1 splňuje požadavky.)

    Všimněte si, že příkaz začíná řetězcem ". \". Aby se zabránilo škodlivým spouštění skriptů, vyžaduje prostředí Windows PowerShell zadejte plně kvalifikovanou cestu k souboru skriptu při spuštění skriptu. Označuje aktuální adresář můžete použít tečku (".\") nebo zadejte plně kvalifikovanou cestu, například:

    [!code-console[Main](the-fix-it-sample-application/samples/sample26.cmd)]

    Další informace o tomto skriptu, použijte `Get-Help` rutiny.

    [!code-console[Main](the-fix-it-sample-application/samples/sample27.cmd)]

    Můžete použít `Detailed`, `Full`, `Parameters`, a `Examples` parametry rutiny Get-Help pro filtrování nápovědy, která je vrácena.

    Pokud skript selže nebo generuje chyby, jako je například "New-AzureWebsite: volání Set-AzureSubscription a Select-AzureSubscription první," nemusí dokončení konfigurace Azure Powershellu.

    Po dokončení skriptu, můžete použít portál pro správu Azure a zobrazit prostředky, které byly vytvořeny, jak je znázorněno [automatizovat všechno](automate-everything.md) kapitoly.
10. K nasazení projektu automatickou do nového prostředí Azure, použijte *AzureWebsite.ps1* skriptu. Příklad:

    [!code-console[Main](the-fix-it-sample-application/samples/sample28.cmd)]

    Po dokončení nasazení se otevře v prohlížeči s Fix It běžící v Azure.

<a id="troubleshooting"></a>
## <a name="troubleshooting-the-windows-powershell-scripts"></a>Řešení potíží se skripty prostředí Windows PowerShell

Nejběžnějších chyb zjištěných při spouštění těchto skriptů jsou souvisejících s oprávněními. Ujistěte se, že `Add-AzureAccount` a `Import-AzurePublishSettingsFile` proběhly úspěšně a že jste použili pro stejné předplatné Azure. I když `Add-AzureAccount` byla úspěšná možná budete muset spustit znovu. Oprávnění přidal(a) `Add-AzureAccount` vyprší za 12 hodin.

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a>Odkaz na objekt není nastaven na instanci objektu.

Pokud skript vrátí chyby, jako například "Není odkaz objektu nastaven na instanci objektu," to znamená, že prostředí Windows PowerShell nelze najít objekt k procesu (Toto je výjimka nulového odkazu), spusťte `Add-AzureAccount` rutiny a opakujte akci skriptu.

[!code-console[Main](the-fix-it-sample-application/samples/sample29.cmd)]

### <a name="internalerror-the-server-encountered-an-internal-error"></a>Vnitřní chyba: Na serveru došlo k vnitřní chybě.

`New-AzureWebsite` Rutina vrací vnitřní chybu, pokud název není v doméně azurewebsites.net jedinečný. Chcete-li vyřešit chybu, použít jinou hodnotu pro název, který se nachází v parametru Name *New-AzureWebsiteEnv.ps1*.

[!code-console[Main](the-fix-it-sample-application/samples/sample30.cmd)]

### <a name="restarting-the-script"></a>Restartovat skript

Pokud je potřeba restartovat *New-AzureWebsiteEnv.ps1* skript vzhledem k tomu, že došlo k selhání předtím, než ho vytisknout zprávu "Mít skript hotový", můžete chtít odstranit prostředky, které skript vytvořený dříve, než se zastavila. Například pokud skript už vytvořený ContosoFixItDemo webovou aplikaci a znovu spusťte skript se stejným názvem, skript selže, protože název se používá.

K určení, které prostředky skript vytvořený dříve, než ji zastavit, použijte následující rutiny:

- `Get-AzureWebsite`
- `Get-AzureSqlDatabaseServer`
- `Get-AzureSqlDatabase`: Ke spuštění této rutiny, předat název databázového serveru do `Get-AzureSqlDatabase`:  
    `Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`

Pokud chcete odstranit tyto prostředky, použijte následující příkazy. Všimněte si, že při odstranění databázového serveru, je automaticky odstraňovat databáze přidružené k serveru.

- `Get-AzureWebsite -Name <WebsiteName> | Remove-AzureWebsite`
- `Get-AzureSqlDatabase -Name <DatabaseName> -ServerName <DatabaseServerName> | Remove-SqlAzureDatabase`
- `Get-AzureSqlDatabaseServer | Remove-AzureSqlDatabaseServer`

<a id="deployqueues"></a>
## <a name="how-to-deploy-the-app-with-queue-processing-to-azure-app-service-web-apps-and-an-azure-cloud-service"></a>Jak nasadit aplikaci s frontou zpracování pro Azure App Service Web Apps a Azure Cloud Service

Pokud chcete povolit front, proveďte následující změnu v souboru MyFixIt\Web.config. V části `appSettings`, změňte hodnotu vlastnosti `UseQueues` "true": 

[!code-xml[Main](the-fix-it-sample-application/samples/sample31.xml)]

Pak nasadíte aplikaci MVC do webové aplikace ve službě Azure App Service, jak je popsáno [starší](#deploybase).

Dále vytvořte novou službu Azure cloud. Skripty, které jsou součástí aplikace Fix It nevytvářejí ani nasadit cloudovou službu, tak k tomu musíte pomocí webu Azure portal. Na portálu klikněte na tlačítko **nový** -- **Compute** – **Cloudovou službu** -- **rychle vytvořit**a potom zadejte adresu URL. a umístěním datového centra. Pomocí stejných datacentrech, kde jste nasadili webovou aplikaci.

![](the-fix-it-sample-application/_static/image1.png)

Než bude možné nasadit cloudovou službu, je potřeba aktualizovat některé konfigurační soubory.

V MyFixIt.WorkerRoler\app.config v části `connectionStrings`, nahraďte hodnotu `appdb` připojovací řetězec skutečným připojovacím řetězcem pro službu SQL Database. Získání připojovacího řetězce z portálu. Na portálu klikněte na tlačítko **databází SQL** - **appdb** - **zobrazení SQL databázové připojovací řetězce pro ADO .net, ODBC, JDBC a PHP**. Zkopírujte připojovací řetězec ADO.NET a vložte tuto hodnotu do souboru app.config. Nahraďte "{vaše\_heslo\_tady}" se heslo k databázi. (Za předpokladu, že jste použili skripty k nasazení aplikace MVC, jste zadali heslo databáze v `SqlDatabasePassword` parametr skriptu.)

Výsledek by měl vypadat nějak takto:

[!code-xml[Main](the-fix-it-sample-application/samples/sample32.xml)]

Ve stejném souboru MyFixIt.WorkerRoler\app.config pod `appSettings`, nahradit dva zástupné hodnoty pro účet úložiště Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample33.xml?highlight=2-3)]

Získání přístupového klíče z portálu. Zobrazit [Správa účtů úložiště](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).

V MyFixItCloudService\ServiceConfiguration.Cloud.cscfg nahraďte stejné hodnoty dva zástupné symboly pro účet úložiště Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample34.xml?highlight=3)]

Nyní jste připraveni nasadit cloudovou službu. Prozkoumat řešení, klikněte pravým tlačítkem na projekt MyFixItCloudService a vyberte **publikovat**. Další informace najdete v tématu "[nasazení aplikace do Azure](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)", což je v části 2 [v tomto kurzu](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36).

> [!div class="step-by-step"]
> [Předchozí](more-patterns-and-guidance.md)
