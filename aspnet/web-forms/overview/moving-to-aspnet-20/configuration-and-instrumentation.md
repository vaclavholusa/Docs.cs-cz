---
uid: web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
title: Konfigurace a instrumentace | Dokumentace Microsoftu
author: microsoft
description: Byly zjištěny hlavní změny v konfiguraci a instrumentace v technologii ASP.NET 2.0. Nové rozhraní API technologie ASP.NET konfigurace umožňuje provést změny konfigurace má být provedeno žádosti o přijetí změn...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 21ebbaee-7ed8-45ae-b6c1-c27c88342e48
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
msc.type: authoredcontent
ms.openlocfilehash: dc4e75e8c97228bf14935d6bf4242a036d513816
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399302"
---
<a name="configuration-and-instrumentation"></a>Konfigurace a instrumentace
====================
podle [Microsoft](https://github.com/microsoft)

> Byly zjištěny hlavní změny v konfiguraci a instrumentace v technologii ASP.NET 2.0. Nové rozhraní API technologie ASP.NET konfigurace umožňuje změny konfigurace provedli programově. Kromě toho existují spoustu nových nastavení konfigurace pro nové konfigurace a instrumentace.


Byly zjištěny hlavní změny v konfiguraci a instrumentace v technologii ASP.NET 2.0. Nové rozhraní API technologie ASP.NET konfigurace umožňuje změny konfigurace provedli programově. Kromě toho existují spoustu nových nastavení konfigurace pro nové konfigurace a instrumentace.

V tomto modulu se budeme zabývat ASP.NET rozhraní API konfigurace má vztah k čtení a zápis do konfiguračních souborů ASP.NET, a také vám nabídneme instrumentaci ASP.NET. Také obsahuje nové funkce, které jsou k dispozici v trasování rozhraní ASP.NET.

## <a name="aspnet-configuration-api"></a>Konfigurace rozhraní API technologie ASP.NET

Konfigurace technologie ASP.NET, rozhraní API umožňuje vyvíjet, nasazovat a spravovat konfigurační data aplikace s použitím jednoho programovacího rozhraní. Rozhraní API konfigurace můžete použít vyvíjejí a upravují dokončete konfigurace ASP.NET prostřednictvím kódu programu bez Přímá úprava XML v konfiguračních souborech. Kromě toho můžete použít konfigurační rozhraní API v konzolové aplikace a skripty, které vyvíjíte v nástrojích pro správu webových a modulů snap-in konzoly Microsoft Management Console (MMC).

Následující dva nástroje pro správu konfigurace pomocí rozhraní API konfigurace a jsou součástí rozhraní .NET Framework verze 2.0:

- Modulu snap-in konzoly MMC technologie ASP.NET, který používá rozhraní API konfigurace pro zjednodušení úloh správy, poskytuje ucelený místních konfiguračních dat ze všech úrovních hierarchie konfigurace.
- Nástroje pro správu webu, který vám umožní spravovat nastavení konfigurace pro místní a vzdálené aplikace, včetně hostovaných webů.

Rozhraní API konfigurace ASP.NET obsahuje sadu objektů správy technologie ASP.NET, které můžete použít ke konfiguraci webové stránky a aplikace prostřednictvím kódu programu. Objekty správy systému jsou implementovány jako knihovna tříd rozhraní .NET Framework. Programovací model rozhraní API konfigurace pomáhá zajistit konzistenci kódu a spolehlivost postará se o datové typy v době kompilace. Aby bylo snazší spravovat konfigurace aplikací, rozhraní API konfigurace umožňuje uživateli zobrazit data, která se slučuje ze všech bodů v hierarchii konfigurace jako jedinou kolekci, ne jako samostatné kolekce z různých zobrazení dat konfigurační soubory. Kromě toho konfigurace rozhraní API umožňuje manipulovat s konfigurací celé aplikace bez Přímá úprava XML v konfiguračních souborech. A konečně rozhraní API zjednodušuje úlohy konfigurace díky podpoře nástrojů pro správu, jako jsou nástroje pro správu webu. Rozhraní API konfigurace zjednodušuje nasazení díky podpoře vytváření konfigurační soubory v počítači a spouštění konfiguračních skriptů mezi několik počítačů.

> [!NOTE]
> Konfigurace rozhraní API nepodporuje vytváření aplikací služby IIS.


## <a name="working-with-local-and-remote-configuration-settings"></a>Práce s místními a vzdálenými konfigurační nastavení

Objekt konfigurace představuje sloučené zobrazení nastavení konfigurace, které se vztahují na konkrétní fyzické entity, například počítač, nebo logická entita, jako je například aplikace nebo webovou stránku. Zadaná logická entita může existovat v místním počítači nebo na vzdáleném serveru. Pokud neexistuje žádný konfigurační soubor pro zadaná entita, představuje objekt konfigurace výchozího nastavení konfigurace definované v souboru Machine.config.

Objekt konfigurace můžete získat pomocí jedné z metod otevřít konfiguraci z následující třídy:

1. Třída ConfigurationManager, pokud je vaše entita klientské aplikace.
2. Třída WebConfigurationManager, pokud je vaše entita webové aplikace.

Tyto metody vrátí objekt konfigurace, který zase obsahuje požadované metody a vlastnosti zpracování základní konfigurační soubory. Tyto soubory pro čtení nebo zápis můžete přistupovat.

### <a name="reading"></a>Čtení

Použijte metodu GetSection nebo GetSectionGroup číst informace o konfiguraci. Uživatel nebo proces, který čte musí mít oprávnění ke čtení pro všechny konfigurační soubory v hierarchii.

> [!NOTE]
> Pokud používáte statické metody GetSection přijímající parametr cesty, parametr path musí odkazovat na aplikace, ve kterém kód běží. V opačném případě parametr je ignorován a vrátí informace o konfiguraci pro aktuálně spuštěné aplikaci.


### <a name="writing"></a>Zápis

Použijte jednu z metod uložit zapisovat informace o konfiguraci. Uživatel nebo proces, který zapíše musí mít oprávnění na konfigurační soubor a adresáře na aktuální úrovni konfigurace hierarchii k zápisu, jakož i oprávnění ke čtení u všech konfiguračních souborů v hierarchii.

Pokud chcete vygenerovat konfigurační soubor, který představuje zděděná nastavení pro zadanou entitu, použijte jednu z následujících metod ukládání konfigurace:

1. Uložit metodu pro vytvoření nové konfigurační soubor.
2. Metoda SaveAs vygenerujte nový soubor konfigurace na jiném místě.

## <a name="configuration-classes-and-namespaces"></a>Konfigurace třídy a obory názvů

Mnoho konfigurace třídy a metody jsou podobné k sobě navzájem. Následující tabulka popisuje konfigurace běžně používané třídy a obory názvů.

| **Konfigurace třídy nebo oboru názvů** | **Popis** |
| --- | --- |
| [System.Configuration](https://msdn.microsoft.com/library/system.configuration.aspx) obor názvů | Obsahuje třídy hlavní konfigurace pro všechny aplikace rozhraní .NET Framework. Části třídy obslužné rutiny umožňují získat konfigurační data pro část z metod, jako je například GetSection a GetSectionGroup. Tyto dvě metody jsou nestatická. |
| Třída System.Configuration.Configuration | Představuje sadu konfigurační data pro počítače, aplikace, webový adresář nebo jiný prostředek. Tato třída obsahuje užitečných metod, jako je například GetSection a GetSectionGroup, pro aktualizaci konfiguračních nastavení a získání odkazů na oddíly a skupin oddílů. Tato třída se používá jako návratový typ metody, které získávají návrhu konfiguračních dat, jako jsou metody třídy WebConfigurationManager a ConfigurationManager. |
| Obor názvů System.Web.Configuration | Obsahuje části třídy obslužné rutiny pro ASP.NET konfigurační oddíly definované v [nastavení konfigurace ASP.NET](https://msdn.microsoft.com/library/b5ysx397.aspx). Části třídy obslužné rutiny umožňují získat konfigurační data pro část z metod, jako je například GetSection a GetSectionGroup. |
| Třída System.Web.Configuration.WebConfigurationManager | Poskytuje užitečné metody pro získání odkazů na za běhu a návrhu nastavení konfigurace. Tyto metody použít třídu System.Configuration.Configuration jako typ vrácené hodnoty. Můžete statickou metodu GetSection této třídy nebo metody GetSection nestatické třídy System.Configuration.ConfigurationManager Zaměnitelně. Ke konfiguraci webových aplikací se doporučuje System.Web.Configuration.WebConfigurationManager třídy místo System.Configuration.ConfigurationManager třídy. |
| [System.Configuration.Provider](https://msdn.microsoft.com/library/system.configuration.provider.aspx) obor názvů | Poskytuje způsob, jak přizpůsobit a rozšířit poskytovatele konfigurace. Toto je základní třída pro všechny třídy zprostředkovatelů v konfiguraci systému. |
| [System.Web.Management](https://msdn.microsoft.com/library/system.web.management.aspx) namespace | Obsahuje třídy a rozhraní pro správu a sledování stavu webových aplikací. Přesněji řečeno tento obor názvů není považováno za součást konfigurace rozhraní API. Trasování a událost se provádí v rámci tříd v tomto oboru názvů. |
| [System.Management.Instrumentation](https://msdn.microsoft.com/library/system.management.instrumentation.aspx) obor názvů | Obsahuje třídy, které jsou nezbytné pro instrumentaci aplikací k vystavení události prostřednictvím Windows Management Instrumentation (WMI) k potenciálním zákazníkům a informace pro správu. Monitorování stavu ASP.NET používá k zajištění události rozhraní WMI. Přesněji řečeno tento obor názvů není považováno za součást konfigurace rozhraní API. |

## <a name="reading-from-aspnet-configuration-files"></a>Čtení z konfiguračních souborů ASP.NET

Třída WebConfigurationManager je základní třídou pro čtení z konfiguračních souborů ASP.NET. Existují čtení konfiguračních souborů ASP.NET v podstatě tři kroky:

1. Získáte objekt konfigurace, pomocí metody OpenWebConfiguration.
2. Získání odkazu na požadovaný oddíl v konfiguračním souboru.
3. Načíst požadované informace z konfiguračního souboru.

Konfigurace, který představuje objekt nepředstavuje konkrétní konfigurační soubor. Místo toho představuje sloučené zobrazení konfigurace počítače, aplikace nebo webu. Následující příklad kódu vytvoří objekt konfigurace představující nastavení konfigurace webové aplikace s názvem *ProductInfo*.

[!code-csharp[Main](configuration-and-instrumentation/samples/sample1.cs)]

> [!NOTE]
> Všimněte si, že pokud /ProductInfo cesta neexistuje, výše uvedený kód vrátí výchozí konfiguraci, jak je uvedeno v souboru machine.config.


Až budete mít objekt konfigurace, můžete pak použít metodu GetSection nebo GetSectionGroup zobrazit podrobné informace o nastavení konfigurace. Následující příklad získá odkaz na nastavení zosobnění pro výše uvedené ProductInfo žádosti:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample2.cs)]

## <a name="writing-to-aspnet-configuration-files"></a>Zápis do konfiguračních souborů ASP.NET

Stejně jako v čtení z konfiguračních souborů, je třída WebConfigurationManager jádro pro zápis do konfiguračních souborů Asp.NET. Existují tři kroky pro zápis do konfiguračních souborů ASP.NET.

1. Získáte objekt konfigurace, pomocí metody OpenWebConfiguration.
2. Získání odkazu na požadovaný oddíl v konfiguračním souboru.
3. Zapsat požadované informace z konfiguračního souboru uložit nebo uložit jako metodu.

Následující změny kódu **ladění** atribut &lt;kompilace&gt; prvku na hodnotu false:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample3.cs)]

Při spuštění tohoto kódu **ladění** atribut &lt;kompilace&gt; element bude nastaven na hodnotu false pro *webApp* souboru web.config aplikace.

## <a name="systemwebmanagement-namespace"></a>System.Web.Management Namespace

Obor názvů System.Web.Management poskytuje třídy a rozhraní pro správu a monitorování stavu aplikace ASP.NET.

Protokolování se provádí tak, že definujete pravidla, která přidruží ke zprostředkovateli událostí. Pravidlo definuje typ události, které se odesílají do zprostředkovatele. Následující základní události jsou k dispozici pro přihlášení:

| **WebBaseEvent** | Základní událost třída pro všechny události. Obsahuje požadované vlastnosti pro všechny události, jako je například kód události, Kód podrobností o události, datum a čas, byla vyvolána událost, pořadové číslo, zpráva o události a podrobnosti o události. |
| --- | --- |
| **WebManagementEvent** | Základní událost třídy pro správu událostí, jako například životního cyklu aplikací, požadavek, chyba a událostí auditu. |
| **WebHeartbeatEvent** | Události vygenerované aplikace v pravidelných intervalech zachycení informací o stavu užitečné modulu runtime. |
| **WebAuditEvent** | Základní třída pro události auditu zabezpečení, které slouží k označení podmínek, jako je ověření se nezdařilo, selhání dešifrování *atd.* |
| **WebRequestEvent** | Základní třída pro všechny události informační požadavku. |
| **WebBaseErrorEvent** | Základní třída pro všechny události indikující chybové stavy. |

Typy poskytovatelů, které jsou k dispozici umožnit si odesílat události výstup do prohlížeče událostí, SQL Server, Windows Management Instrumentation (WMI) a e-mailu. Předem nakonfigurované poskytovatelů a mapování události snížit množství práce potřebné k získání výstupu události přihlášení.

Technologie ASP.NET 2.0 používá v protokolu událostí zprostředkovatele out-of-the-box pro protokolování událostí podle domény aplikace spouští a zastavuje, jakož i protokolování všechny neošetřené výjimky. To pomáhá se věnují několika základních scénářů. Například Řekněme, že aplikace vyvolá výjimku, ale uživatel neukládá chybu a nelze ji reprodukovat. Výchozí pravidlo protokolu událostí bude moct shromážděte informace o výjimka a zásobník získat lepší představu o jaký druh chyby došlo k chybě. Další příklad platí v případě, že vaše aplikace je došlo ke ztrátě stavu relace. V takovém případě se můžete podívat v protokolu událostí k určení, zda se recykluje domény aplikace a proč domény aplikace zastavila na prvním místě.

Navíc je rozšiřitelný systém monitorování stavu. Například můžete definovat vlastní webové události, aktivuje v rámci vaší aplikace a pak definovat pravidlo k odesílání informací o události do poskytovatelů, jako je třeba vaše e-mailu. To umožňuje snadno spojit váš Instrumentační stav monitorování poskytovatelů. Další příklad může vyvolat událost pokaždé, když je pořadí zpracování a nastavit pravidlo, které odesílá každé události do databáze serveru SQL Server. Může také vyvolat událost, když se uživateli nepodaří přihlásit více než jednou za sebou a nastavení události pro použití e-mailu podle poskytovatelů.

Konfigurace pro výchozí poskytovatele a události je uložená v globálním souboru Web.config. Globální soubor Web.config uchovává všechny webové nastavení, které byly uloženy v souboru Machine.config v ASP.NET 1 x. Globální soubor Web.config se nachází v následujícím adresáři:

`%windir%\Microsoft.Net\Framework\v2.0.*\config\Web.config`

&lt;HealthMonitoring&gt; část globální soubor Web.config obsahuje výchozí nastavení konfigurace. Můžete přepsat tato nastavení nebo vlastní konfiguraci prostřednictvím implementace &lt;healthMonitoring&gt; oddílu v souboru Web.config pro vaši aplikaci.

&lt;HealthMonitoring&gt; části globální soubor Web.config obsahuje následující položky:

| **Zprostředkovatelé** | Obsahuje poskytovatele nastavení pro Prohlížeč událostí, WMI a SQL Server. |
| --- | --- |
| **eventMappings** | Obsahuje mapování pro různé třídy WebBase. Tento seznam můžete rozšířit, pokud generovat vlastní třídy událostí. Generování třídy vlastní události vám rozlišovací schopnosti prostřednictvím poskytovatelů, odesílat informace do. Můžete například nakonfigurovat neošetřené výjimky k odeslání do serveru SQL Server, při odesílání vlastních událostí k e-mailu. |
| **pravidla** | Odkazy eventMappings k poskytovateli. |
| **ukládání do vyrovnávací paměti** | Použít s zprostředkovatele SQL Server a e-mailu a zjistěte, jak často se má vyprázdnit události k poskytovateli. |

Níže je příklad kódu z globálního souboru Web.config.

[!code-xml[Main](configuration-and-instrumentation/samples/sample4.xml)]

## <a name="how-to-store-events-to-event-viewer"></a>Ukládání událostí do prohlížeče událostí

Jak už bylo zmíněno dříve, zprostředkovatele pro protokolování událostí události prohlížeč konfigurován pro vás v globálním souboru Web.config. Ve výchozím nastavení, všechny události na základě **WebBaseErrorEvent** a **WebFailureAuditEvent** přihlášeni. Můžete přidat další pravidla do protokolu Další informace do protokolu událostí. Například Kdybyste chtěli, aby se protokolovaly všechny události (*tedy*, každou událost na základě **WebBaseEvent**), můžete přidat do souboru Web.config následující pravidlo:

[!code-xml[Main](configuration-and-instrumentation/samples/sample5.xml)]

Toto pravidlo se propojit **všechny události** mapa událostí do protokolu událostí zprostředkovatele. EventMapping a zprostředkovateli jsou zahrnuty v globálním souboru Web.config.

## <a name="how-to-store-events-to-sql-server"></a>Ukládání událostí do systému SQL Server

Tato metoda používá **ASPNETDB** databáze, která se generují pomocí Aspnet\_regsql.exe nástroj. Výchozí poskytovatel použije LocalSqlServer připojovací řetězec, který používá buď databáze založené na souboru v aplikaci\_složka dat nebo místní instanci systému SQL Server SQLExpress. LocalSqlServer připojovací řetězec a SqlProvider jsou nakonfigurované v globálním souboru Web.config.

LocalSqlServer připojovací řetězec v souboru Web.config globální vypadá takto:

[!code-xml[Main](configuration-and-instrumentation/samples/sample6.xml)]

Pokud chcete použít jinou instanci systému SQL Server, budete muset použít Aspnet\_regsql.exe nástroj, který se nachází v % windir%\Microsoft.Net\Framework\v2.0.\* složky. Použít Aspnet\_regsql.exe nástroj pro generování vlastní **ASPNETDB** databáze v instanci SQL serveru, pak přidat připojovací řetězec do konfiguračního souboru aplikace a pak přidat pomocí nového poskytovatele připojovací řetězec. Jakmile budete mít **ASPNETDB** databáze vytvořena, bude nutné nastavit pravidlo, které eventMapping propojit sqlProvider.

Ať už se používají výchozí hodnoty SqlProvider nebo vlastního poskytovatele konfigurace, budete muset přidat pravidla propojení zprostředkovatele s mapu událostí. Následující pravidla propojení nového poskytovatele, který jste vytvořili výše na **všechny události** mapa událostí. Toto pravidlo se budou protokolovat všechny události na základě **WebBaseEvent** a odeslat je do MySqlWebEventProvider, který použije připojovací řetězec MYASPNETDB. Následující kód přidává pravidlo, které propojte poskytovatele s mapou události:

[!code-xml[Main](configuration-and-instrumentation/samples/sample7.xml)]

Pokud byste chtěli provést odeslání pouze chyby na serveru SQL Server, můžete přidat následující pravidla:

[!code-xml[Main](configuration-and-instrumentation/samples/sample8.xml)]

## <a name="how-to-forward-events-to-wmi"></a>Jak události rozhraní WMI

Události můžete předávat také ke službě WMI. Zprostředkovatel rozhraní WMI nakonfigurované ve výchozím nastavení za vás do globálního souboru Web.config.

Následující příklad kódu přidá pravidlo pro předávání událostí služby WMI:

[!code-xml[Main](configuration-and-instrumentation/samples/sample9.xml)]

Je potřeba přidat pravidlo pro přidružení eventMapping k poskytovateli a také aplikace naslouchací proces služby WMI pro naslouchání události. Následující příklad kódu přidá na pravidlo můžete propojit zprostředkovatelem rozhraní WMI **všechny události** mapa událostí:

[!code-xml[Main](configuration-and-instrumentation/samples/sample10.xml)]

## <a name="how-to-forward-events-to-email"></a>Jak události e-mailu

Můžete také přeposílat události k e-mailu. Dejte pozor, o které pravidel událostí je namapovat na poskytovatele e-mailu, jak vám můžou neúmyslně pošlete sami sobě velké množství informací, které může být vhodnější pro SQL Server nebo v protokolu událostí. Existují dva poskytovatelé e-mailu; SimpleMailWebEventProvider a TemplatedMailWebEventProvider. Každá má stejné atributy konfigurace, s výjimkou "Šablona" a "detailedTemplateErrors" atributy, které jsou pouze k dispozici na TemplatedMailWebEventProvider.

> [!NOTE]
> Ani jeden z těchto zprostředkovatelů e-mailu je nakonfigurován za vás. Bude potřeba je přidat do souboru Web.config.


Hlavní rozdíl mezi poskytovateli tyto dva e-mailu je, že SimpleMailWebEventProvider odešle e-mailů v obecné šabloně, která nelze upravit. Ukázkový soubor Web.config tohoto poskytovatele e-mailu přidá do seznamu zprostředkovatelů nakonfigurované pomocí následující pravidla:

[!code-xml[Main](configuration-and-instrumentation/samples/sample11.xml)]

Následující pravidlo je taky přidaný ke tie poskytovateli e-mailu **všechny události** mapa událostí:

[!code-xml[Main](configuration-and-instrumentation/samples/sample12.xml)]

## <a name="aspnet-20-tracing"></a>ASP.NET 2.0 trasování

Existují tři hlavní vylepšení trasování v technologii ASP.NET 2.0.

1. Funkce integrované trasování
2. Programový přístup k trasování zpráv
3. Vylepšené trasování na úrovni aplikace

## <a name="integrated-tracing-functionality"></a>Integrovaná funkce trasování

Teď můžete směrovat zprávy, protože ho vygeneroval System.Diagnostics.Trace – třída do výstupu trasování technologie ASP.NET a směrování zpráv, protože ho vygeneroval trasování rozhraní ASP.NET System.Diagnostics.Trace. ASP.NET – události instrumentace můžete také předat System.Diagnostics.Trace. Tato funkce je poskytována nové **writeToDiagnosticsTrace** atribut &lt;trasování&gt; elementu. Pokud je logická hodnota true, ASP.NET trasovací zprávy jsou předány infrastruktury trasování System.Diagnostics pro použití podle jakékoli naslouchacích procesů, které jsou registrovány k zobrazení trasovacích zpráv.

## <a name="programmatic-access-to-trace-messages"></a>Programový přístup k trasování zprávy

ASP.NET 2.0 umožňuje programový přístup k všechny zprávy trasování prostřednictvím **TraceContextRecord** třídy a **TraceRecords** kolekce. Nejúčinnější způsob přístupu k zprávy trasování je zaregistrovat **TraceContextEventHandler** delegáta (Další Novinky v technologii ASP.NET 2.0) ke zpracování nového **TraceFinished** událostí. Trasovací zprávy lze potom projít podle potřeby.

Následující vzorový kód to znázorňuje:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample13.cs)]

V příkladu výše můžu projít TraceRecords kolekce a pak zápis do datového proudu odpovědi každou zprávu.

## <a name="improved-application-level-tracing"></a>Vylepšené trasování na úrovni aplikace

Zlepší se trasování na úrovni aplikace prostřednictvím zavedení nového **mostRecent** atribut &lt;trasování&gt; elementu. Tento atribut určuje, zda se zobrazí nejnovější výstup trasování na úrovni aplikace a data trasování starší než omezení, které jsou označeny requestLimit se zahodí. Pokud má hodnotu false, data trasování se nezobrazí, pokud pro požadavky na atribut requestLimit je dosaženo.

## <a name="aspnet-command-line-tools"></a>Nástroje příkazového řádku technologie ASP.NET

Existuje několik nástrojů příkazového řádku pro zjednodušení konfigurace technologie ASP.NET. Vývoj v ASP.NET měli seznámit s aspnet\_regiis.exe nástroj. ASP.NET 2.0 obsahuje tři další příkazového řádku nástroje pro usnadnění v konfiguraci.

K dispozici jsou nástroje příkazového řádku následující:

| **Nástroj** | **Použití** |
| --- | --- |
| **aspnet\_regiis.exe** | Umožňuje zaregistrovat technologii ASP.NET se službou IIS. Existují dvě verze tohoto nástroje nastavených s prostředím ASP.NET 2.0, jednu pro systémy 32-bit (ve složce Framework) a jeden pro 64bitové systémy (ve složce Framework64.) 64bitová verze nenainstalují na 32bitovém operačním systému. |
| **aspnet\_regsql.exe** | Nástroj registrace technologie ASP.NET SQL serveru se používá k vytvoření databáze serveru Microsoft SQL Server pro použití u poskytovatele systému SQL Server v technologii ASP.NET, nebo chcete přidat nebo odebrat možnosti z existující databáze. Aspnet\_regsql.exe soubor se nachází v [drive:]\WINDOWS\Microsoft.NET\Framework\versionNumber složky na vašem webovém serveru. |
| **aspnet\_regbrowsers.exe** | Nástroje pro registraci prohlížeče technologie ASP.NET analyzuje a všechny definice prohlížeče systémová do sestavení zkompiluje a nainstaluje sestavení do globální mezipaměti sestavení. Nástroj používá definiční soubory prohlížeče (. Soubory prohlížeče) z podadresáře prohlížeče rozhraní .NET Framework. Nástroj můžete najít v adresáři %SystemRoot%\Microsoft.NET\Framework\version\. |
| **aspnet\_compiler.exe** | Nástroj ASP.NET kompilace umožňuje zkompilovat aplikaci rozhraní ASP.NET Web, na místě nebo pro nasazení do cílového umístění, jako je například provozní server. Kompilace v místě zlepšuje výkon aplikací, protože koncoví uživatelé nevšimnete zpoždění první žádosti o aplikace, zatímco bude uložena zkompilovaná aplikace. |

Vzhledem k tomu, aspnet\_regiis.exe nástroj není nové technologie ASP.NET 2.0, nebude probereme ji sem.

## <a name="aspnet-sql-server-registration-tool---aspnetregsqlexe"></a>Nástroj pro registraci serveru SQL technologie ASP.NET – aspnet\_regsql.exe

Můžete nastavit několik typů možností použití nástroje registrace technologie ASP.NET SQL Server. Můžete určit připojení SQL, určete, které aplikační služby použít systém SQL Server pro správu informací, označení, která databáze nebo tabulka slouží pro závislost mezipaměti SQL a přidat nebo odebrat podporu pro ukládání postupy a stavu relace pomocí SQL serveru.

Spolehněte se na poskytovatele ke správě ukládání a načítání dat ze zdroje dat několik aplikačních služeb technologie ASP.NET. Každý poskytovatel je specifické pro zdroj dat. Technologie ASP.NET obsahuje zprostředkovatele SQL Server pro následující funkce ASP.NET:

- Členství ( [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) třídy).
- Správa rolí ( [zprostředkovatele SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) třídy).
- Profil ( [SqlProfileProvider](https://msdn.microsoft.com/library/system.web.profile.sqlprofileprovider.aspx) třídy).
- Přizpůsobení webových částí ( [SqlPersonalizationProvider](https://msdn.microsoft.com/library/system.web.ui.webcontrols.webparts.sqlpersonalizationprovider.aspx) třídy).
- Webové události ( [SqlWebEventProvider](https://msdn.microsoft.com/library/system.web.management.sqlwebeventprovider.aspx) třídy).

Při instalaci ASP.NET obsahuje soubor Machine.config pro váš server konfiguračních elementů určujících poskytovatele systému SQL Server pro každou funkcí technologie ASP.NET, které závisí na zprostředkovateli. Tito zprostředkovatelé jsou nakonfigurované ve výchozím nastavení pro připojení k místní uživatelskou instanci systému SQL Server Express 2005. Pokud změníte výchozí připojovací řetězec používaný zprostředkovatele, abyste mohli používat všechny funkce technologie ASP.NET, které jsou nakonfigurované v konfiguraci počítače je nutné nainstalovat databáze serveru SQL Server a prvky databáze pro vaši zvolenou funkci pomocí Aspnet\_regsql.exe. Pokud databáze, kterou zadáte pomocí nástroje pro registraci SQL již neexistuje (aspnetdb bude výchozí databázi pokud jeden není zadán v příkazovém řádku), pak je aktuální uživatel musí mít oprávnění k vytváření databází v systému SQL Server i pro vytváření schémat lements v databázi.

### <a name="sql-cache-dependency"></a>Závislosti mezipaměti SQL

O pokročilou funkci ukládání výstupu ASP.NET do mezipaměti je závislosti mezipaměti SQL. Závislosti mezipaměti SQL podporuje dva různé režimy fungování: jednu, která používá ASP.NET implementace cyklického dotazování tabulky a druhý režim, který využívá funkce oznámení dotazů SQL Server 2005. Registrační nástroj SQL můžete použít ke konfiguraci režimu cyklického dotazování tabulky operace.

### <a name="session-state"></a>Stav relace

Ve výchozím nastavení hodnoty stavu relace a informace jsou uloženy v paměti v rámci procesu ASP.NET. Alternativně můžete uložit data relace v databázi serveru SQL Server, kde může být sdílen více webových serverů. Pokud databáze, kterou zadáte pro stav relace pomocí nástroje pro registraci SQL ještě neexistuje, musí mít aktuální uživatel oprávnění k vytváření databází v SQL serveru také vytvoření prvků schématu v databázi. Pokud databáze neexistuje, je aktuální uživatel musí mít práva k vytváření prvků schématu ve stávající databázi.

Chcete-li nainstalovat databázi stavu relací v systému SQL Server, spusťte Aspnet\_regsql.exe nástroje a zadejte následující informace pomocí příkazu:

- Název systému SQL Server instance pomocí **-S** možnost.
- Přihlašovací údaje k účtu, který má oprávnění k vytvoření databáze v počítači se systémem SQL Server. Použít **-e.** možnost využít aktuálně přihlášeného uživatele, nebo použít **- U** můžete zadat ID uživatele spolu s **-P** můžete zadat heslo.
- **- Ssadd** možnost příkazového řádku, chcete-li přidat databázi stavu relací.

Ve výchozím nastavení, nelze použít Aspnet\_regsql.exe nástroj pro instalaci databáze stavu relace na počítači se systémem SQL Server 2005 Express Edition.

### <a name="the-aspnet-browser-registration-tool---aspnetregbrowsersexe"></a>Registrační nástroj prohlížeče technologie ASP.NET – aspnet\_regbrowsers.exe

V technologii ASP.NET verze 1.1, soubor Machine.config obsahoval část s názvem &lt;browserCaps&gt;. Tato část obsahuje řadu záznamů jazyka XML, které definované konfigurace pro různé prohlížeče na základě regulárního výrazu. Pro technologii ASP.NET verze 2.0 nový. Soubor prohlížeče definuje parametry určitého prohlížeče pomocí záznamů jazyka XML. Přidat informace o novém prohlížeči tak, že přidáte nový. Soubor prohlížeče do složky c:\ %SystemRoot%\Microsoft.NET\Framework\version\CONFIG\Browsers ve vašem systému.

Protože aplikace není čtení souboru .config, pokaždé, když se vyžaduje informace o prohlížeči, můžete vytvořit nový. Soubor prohlížeče a spuštění Aspnet\_regbrowsers.exe přidat požadované změny do sestavení. To umožňuje serveru okamžitě přístup k nové informace o prohlížeči, není potřeba vypnout libovolnou z vašich aplikací, aby se získaly informace. Aplikace můžete přistupovat prostřednictvím prohlížeče vlastnost aktuální HttpRequest možnosti prohlížeče.

Tyto možnosti jsou k dispozici při spuštění aspnet\_regbrowser.exe:

| **Možnost** | **Popis** |
| --- | --- |
| **-?** | Zobrazí Aspnet\_regbbrowsers.exe text nápovědy v příkazovém okně. |
| **-i** | Vytvoří sestavení možností prohlížeče modulu runtime a nainstaluje do globální mezipaměti sestavení. |
| **-u** | Odinstaluje sestavení možností prohlížeče modulu runtime z globální mezipaměti sestavení. |

## <a name="the-aspnet-compilation-tool---aspnetcompilerexe"></a>Nástroj kompilace ASP.NET – aspnet\_compiler.exe

Nástroj ASP.NET kompilace lze použít dvěma způsoby: pro kompilaci v místě a kompilace pro nasazení, kde je zadán cílový adresář výstupu.

### <a name="compiling-an-application-in-placehttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Kompilace aplikace na místě](https://msdn.microsoft.com/library/ms229863.aspx)

Nástroj ASP.NET kompilace můžete zkompilovat aplikaci v místě, tedy napodobuje chování zasílání více požadavků na aplikaci, což způsobuje regulární kompilace. Uživatelé předem kompilovaných lokality nebude docházet ke zpoždění způsobené kompilace stránky při prvním požadavku.

Při předkompilaci lokality na místě, platí následující položky:

- Web zachová své soubory a strukturu adresářů.
- Kompilátory pro všechny programovací jazyky používané na serveru lokality musí mít.
- Pokud jakýkoli soubor nepodaří kompilace, selže celá lokalita kompilace.

Můžete také znovu zkompilovat aplikaci na místě po přidání nové zdrojové soubory k němu. Nástroj zkompiluje jenom nové nebo změněné soubory, pokud zahrnete **- c** možnost.

> [!NOTE]
> Kompilace aplikace, která obsahuje vnořené aplikace nebude zkompilován vnořené aplikace. Vnořené aplikace musí být kompilován samostatně.


### <a name="compiling-an-application-for-deploymenthttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Kompilace aplikace pro nasazení](https://msdn.microsoft.com/library/ms229863.aspx)

Zkompilovat aplikaci pro nasazení (kompilace do cílového umístění) tak, že zadáte parametr targetDir. Parametr targetDir může být konečné umístění pro webovou aplikaci nebo kompilovanou aplikaci je možné nasadit další. Použití **-u** možnost zkompiluje aplikaci tak, že můžete provádět změny určité soubory kompilované aplikace bez opětovné kompilace. ASPNET\_compiler.exe rozlišuje mezi typy souborů statické a dynamické a zpracovává je jiným způsobem, při vytváření výsledné aplikace.

- Statické typy souborů jsou ty, které nemají přidružený kompilátoru nebo poskytovatelem, například soubory, jejichž sestavení s názvem mít rozšíření, jako je například .css, GIF, htm, HTML, .jpg, JS a tak dále. Tyto soubory budou zkopírovány pouze do cílového umístění, jejich relativní umístění v adresářové struktuře je zachováno.
- Dynamické typy souborů jsou ty, které jsou asociovány s kompilátorem nebo sestavení zprostředkovatele, včetně souborů s příponami názvů souborů specifické pro ASP.NET například asax, .ascx, .ashx, .aspx, Browser, .master a tak dále. Nástroj pro kompilaci ASP.NET generuje sestavení z těchto souborů. Pokud **-u** možnost vynecháte, nástroj také vytvoří soubory s příponou názvu souboru. ZKOMPILOVANÉ, které mapují původním zdrojovým souborům na jejich sestavení. Pokud chcete mít jistotu, že se zachová strukturu adresářů ze zdroje aplikace, nástroj vygeneruje soubory zástupných symbolů v odpovídajících umístěních v cílové aplikaci.

Je nutné použít **-u** možnost k označení, že je možné upravit obsah kompilované aplikace. Následné změny v opačném případě se ignorují nebo způsobit chyby za běhu.

Následující tabulka popisuje, jak kompilace technologie ASP.NET nástroj popisovače různé typy souborů, když **-u** je zadán parametr.

| **Typ souboru** | **Kompilátor akce** |
| --- | --- |
| .ascx, .aspx, .master | Tyto soubory jsou rozděleny na kód a zdrojový kód, který obsahuje soubory kódu na pozadí a jakýkoli kód, který je uzavřen v &lt;skriptu runat = "server"&gt; elementy. Zdrojový kód je zkompilován sestavení s názvy, které jsou odvozeny z algoritmu hash a sestavení jsou umístěny v adresáři Bin. Žádný vložený kód, který je, uzavřen mezi značkami kód **&lt; %** a **% &gt;** hranaté závorky, jsou zahrnuty v kódu a není zkompilován. Nové soubory se stejným názvem jako zdrojové soubory jsou vytvořeny tak, aby obsahovala kód a je umístěn v odpovídající výstupní adresáře. |
| .ashx, .asmx | Tyto soubory nejsou zkompilovány a jsou přesunuty do výstupních adresářů, jak jsou a ne zkompilován. Pokud chcete mít kód obslužné rutiny, které jsou kompilovány, umístěte kód do souborů zdrojového kódu v aplikaci\_adresář kódu. |
| cs, VB, .jsl, .cpp (bez použití modelu code-behind souborů pro typy souborů uvedené výše) | Tyto soubory jsou zkompilovány a zahrnutý jako prostředek v sestavení, která na ně odkazují. Zdrojové soubory nejsou zkopírovány do výstupního adresáře. Pokud není soubor kódu odkazuje, není zkompilován. |
| Vlastní typy souborů | Tyto soubory nejsou zkompilovány. Tyto soubory se zkopírují do odpovídající výstupní adresáře. |
| Souborů zdrojového kódu v aplikaci\_podadresář s kódem | Tyto soubory jsou zkompilovány do sestavení a umístěn v adresáři Bin. |
| soubory RESX a Resource v aplikaci\_GlobalResources podadresáře | Tyto soubory jsou zkompilovány do sestavení a umístěn v adresáři Bin. Žádná aplikace\_GlobalResources podadresáře se vytvoří v rámci hlavního výstupního adresáře a žádné soubory .resx nebo .resources nachází ve zdrojovém adresáři jsou zkopírovány do výstupního adresáře. |
| soubory RESX a Resource v aplikaci\_LocalResources podadresáře | Tyto soubory nejsou zkompilovány a zkopírují do odpovídající výstupní adresáře. |
| soubory SKIN v aplikaci\_podadresář motivy | Skin soubory a statické soubory motivů nejsou zkompilovány a zkopírují do odpovídající výstupní adresáře. |
| Browser soubor Web.config statické typy sestavení, které jsou již v adresáři Bin | Tyto soubory se zkopírují jako výstupní adresáře. |

Následující tabulka popisuje, jak kompilace technologie ASP.NET nástroj popisovače různé typy souborů, když **-u** není zadán parametr.

| **Typ souboru** | **Kompilátor akce** |
| --- | --- |
| .aspx, .asmx, .ashx, .master | Tyto soubory jsou rozděleny na kód a zdrojový kód, který obsahuje soubory kódu na pozadí a jakýkoli kód, který je uzavřen v &lt;skriptu runat = "server"&gt; elementy. Zdrojový kód je zkompilován sestavení s názvy, které jsou odvozeny z algoritmu hash. Výsledné sestavení se umístí do adresáře Bin. Žádný vložený kód, který je, uzavřen mezi značkami kód **&lt; %** a **% &gt;** hranaté závorky, jsou zahrnuty v kódu a není zkompilován. Kompilátor vytvoří nové soubory tak, aby obsahovala kód se stejným názvem jako zdrojové soubory. Tyto výsledné soubory jsou umístěny v adresáři Bin. Kompilátor vytvoří také soubory se stejným názvem jako zdrojové soubory, ale s příponou. ZKOMPILOVANÉ, které obsahují informace o mapování. Na. ZKOMPILOVANÉ soubory jsou umístěny ve výstupní adresáře odpovídá původní umístění zdrojových souborů. |
| .ascx | Tyto soubory jsou rozděleny na kód a zdrojový kód. Zdrojový kód je zkompilován sestavení a umístěn v adresáři Bin, s názvy, které jsou odvozeny z algoritmu hash. Žádné značky soubory jsou vygenerovány. |
| cs, VB, .jsl, .cpp (bez použití modelu code-behind souborů pro typy souborů uvedené výše) | Zdrojový kód, který se odkazuje sestavení generována z .ascx, .ashx nebo soubory .aspx je zkompilovat do sestavení a umístěn v adresáři Bin. Nejsou zkopírovány žádné zdrojové soubory. |
| Vlastní typy souborů | Tyto soubory jsou kompilovány jako dynamické soubory. V závislosti na typu souboru, které jsou založeny na kompilátor mapování soubory umístit do výstupního adresáře. |
| Soubory v aplikaci\_podadresář s kódem | Soubory zdrojového kódu v tomto podadresáři jsou zkompilovány do sestavení a umístěn v adresáři Bin. |
| Soubory v aplikaci\_GlobalResources podadresáře | Tyto soubory jsou zkompilovány do sestavení a umístěn v adresáři Bin. Žádná aplikace\_GlobalResources podadresáře se vytvoří v rámci hlavního výstupního adresáře. Pokud konfigurační soubor má appliesTo = "All", soubory RESX a .resources jsou zkopírovány do výstupního adresáře. Pokud je odkazováno dle nejsou zkopírovány [zprostředkovatele BuildProvider](https://msdn.microsoft.com/library/system.web.configuration.buildprovider.aspx). |
| soubory RESX a Resource v aplikaci\_LocalResources podadresáře | Tyto soubory jsou zkompilovány do sestavení s jedinečnými názvy a umístit do adresáře Bin. Žádné soubory RESX a Resource jsou zkopírovány do výstupního adresáře. |
| soubory SKIN v aplikaci\_podadresář motivy | Motivy jsou zkompilovány do sestavení a umístěn v adresáři Bin. Soubory zástupných procedur jsou vytvořené pro soubory skin a umístěné v odpovídající výstupní adresář. Statické soubory (například .css) jsou zkopírovány do výstupního adresáře. |
| Browser soubor Web.config statické typy sestavení, které jsou již v adresáři Bin | Tyto soubory se zkopírují, jako je do výstupního adresáře. |

### <a name="fixed-assembly-nameshttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Názvy sestavení s pevnými](https://msdn.microsoft.com/library/ms229863.aspx##)

Některé scénáře, jako je nasazení webové aplikace s využitím MSI Instalační služby Windows, vyžadují použití konzistentní názvy a obsah, jakož i konzistentní adresářovou strukturu pro identifikaci sestavení nebo konfigurace nastavení pro aktualizace. V takových případech můžete použít **- fixednames** možnost určit, že by měl nástroj kompilace technologie ASP.NET kompilovat sestavení pro každý zdrojový soubor namísto použití where více stránek kompilovány do sestavení. To může vést k velký počet sestavení, takže pokud jste obeznámeni s škálovatelnost můžete tuto možnost používejte opatrně.

### <a name="strong-name-compilationhttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Kompilace silného názvu](https://msdn.microsoft.com/library/ms229863.aspx##)

**- Aptca**, **- delaysign**, **- keycontainer** a **- keyfile** možnosti jsou k dispozici tak, aby vám Aspnet\_ Compiler.exe vytvoření silně pojmenované sestavení bez použití [nástroj Strong Name (Sn.exe)](https://msdn.microsoft.com/library/k5b5tt23.aspx) samostatně. Tyto parametry odpovídají na **AllowPartiallyTrustedCallersAttribute**, **AssemblyDelaySignAttribute**, **AssemblyKeyNameAttribute**a  **AssemblyKeyFileAttribute**.

Informace o těchto atributů je mimo rozsah tohoto kurzu.

## <a name="labs"></a>Praktická cvičení

Každý z následujících labs navazuje na předchozí labs. Je potřeba provést v pořadí.

## <a name="lab-1-using-the-configuration-api"></a>Praktické cvičení 1: Použití rozhraní API konfigurace

1. Vytvořit nový web volá *mod9lab*.
2. Přidáte nový soubor webové konfigurace do lokality.
3. V souboru web.config přidejte následující:


[!code-xml[Main](configuration-and-instrumentation/samples/sample14.xml)]

Tím se zajistí, že máte oprávnění ukládat změny v souboru web.config.

1. Přidání nového ovládacího prvku popisku na Default.aspx a změnit ID **lblDebugStatus**.
2. Přidáte nový ovládací prvek tlačítko na Default.aspx.
3. Změnit ID ovládacího prvku Button **btnToggleDebug** a Text, který má **změnit stav ladění**.
4. Otevřete zobrazení kódu pro použití modelu code-behind soubor Default.aspx a přidejte **pomocí** příkaz pro **System.Web.Configuration** následujícím způsobem:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample15.cs)]

1. Přidejte dvě soukromé proměnné třídy a na stránce\_metody Init, jak je znázorněno níže:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample16.cs)]

1. Přidejte následující kód na stránku\_zatížení:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample17.cs)]

1. Uložit a procházet default.aspx. Všimněte si, že ovládací prvek Label zobrazí aktuální stav ladění.
2. Dvakrát klikněte na ovládací prvek tlačítko v návrháři a přidejte následující kód pro událost Click pro ovládací prvek tlačítka:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample18.cs)]

1. Uložit a procházet default.aspx a klikněte na tlačítko.
2. Otevřete soubor web.config po každé tlačítko klikněte na tlačítko a podívejte se **ladění** atribut &lt;kompilace&gt; oddílu.

## <a name="lab-2-logging-application-restarts"></a>Praktické cvičení 2: Protokolování restartování aplikace

V tomto testovacím prostředí vytvoří kód, který vám umožní zapínat protokolování vypnutí aplikace, Start-upů a předkompilací v prohlížeči událostí.

1. Přidat DropDownList default.aspx a změňte ID ddlLogAppEvents.
2. Nastavte **AutoPostBack** vlastnost DropDownList k **true**.
3. Přidejte tři položky do kolekce položek pro DropDownList. Ujistěte se, **Text** první položky *Select Value* a hodnota -1. Ujistěte se, **Text** a **hodnotu** druhé položky **True** a **Text** a **hodnotu** třetí položky **False**.
4. Přidáte nový popisek na stránku default.aspx. ID se má změnit **lblLogAppEvents**.
5. Otevřete zobrazení použití modelu code-behind default.aspx a přidejte novou deklaraci proměnné typu HealthMonitoringSection, jak je znázorněno níže:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample19.cs)]

1. Přidejte následující kód do existující kód ve třídě stránky\_Init:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample20.cs)]

1. Dvakrát klikněte na DropDownList a událost SelectedIndexChanged. přidejte následující kód:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample21.cs)]

1. Procházejte default.aspx.
2. Nastavte rozevírací seznam na **False**.
3. Vymažte protokol aplikace v prohlížeči událostí.
4. Klikněte na tlačítko změnit atribut ladění pro aplikace.
5. Aktualizujte protokol aplikací v prohlížeči událostí. 

    1. Byly zaznamenány žádné události?
    2. Proč nebo proč ne?
6. Nastavte rozevírací seznam na **hodnotu True.**
7. Klikněte na tlačítko k přepnutí atribut ladění pro aplikace.
8. Aktualizujte aplikaci přihlášení v prohlížeči událostí. 

    1. Byly zaznamenány žádné události?
    2. Jak se z důvodu vypnutí aplikace?
9. Experimentujte s zapnutí a vypnutí protokolování a podívejte se na změny provedené v souboru web.config.

## <a name="more-information"></a>Další informace:

ASP.NET 2.0 na model poskytovatele umožňuje vytvořit vaše vlastní zprostředkovatele pro nejen instrumentace aplikací, ale pro mnoho jiné účely stejně jako je členství, profily atd. Podrobné informace o psaní vlastního zprostředkovatele pro protokolování událostí, které aplikace do textového souboru, navštivte [tento odkaz](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/ASPNETProvMod_Prt6.asp).
