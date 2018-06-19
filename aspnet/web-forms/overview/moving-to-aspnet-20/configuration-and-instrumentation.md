---
uid: web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
title: Konfigurace a instrumentace | Microsoft Docs
author: microsoft
description: Existují hlavní změny v konfiguraci a instrumentaci v technologii ASP.NET 2.0. Nové rozhraní API ASP.NET konfigurace umožňuje provedení pr změn konfigurace...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 21ebbaee-7ed8-45ae-b6c1-c27c88342e48
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
msc.type: authoredcontent
ms.openlocfilehash: 16dfe3c899dfa028d8a52b4b5f9c2868887e8fa9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/10/2018
ms.locfileid: "28886014"
---
<a name="configuration-and-instrumentation"></a>Konfigurace a instrumentace
====================
by [Microsoft](https://github.com/microsoft)

> Existují hlavní změny v konfiguraci a instrumentaci v technologii ASP.NET 2.0. Nové rozhraní API ASP.NET konfigurace umožňuje změny konfigurace, které má být provedeno prostřednictvím kódu programu. Kromě toho existuje mnoho nové nastavení konfigurace povolit pro nové konfigurace a instrumentace.


Existují hlavní změny v konfiguraci a instrumentaci v technologii ASP.NET 2.0. Nové rozhraní API ASP.NET konfigurace umožňuje změny konfigurace, které má být provedeno prostřednictvím kódu programu. Kromě toho existuje mnoho nové nastavení konfigurace povolit pro nové konfigurace a instrumentace.

V tomto modulu se budeme zabývat ASP.NET rozhraní API konfigurace se týká čtení a zápis na konfigurační soubory technologie ASP.NET a také obsahuje instrumentace technologie ASP.NET. Také obsahuje nové funkce, které jsou k dispozici v trasování rozhraní ASP.NET.

## <a name="aspnet-configuration-api"></a>Rozhraní API konfigurace ASP.NET

Rozhraní API konfigurace technologie ASP.NET umožňuje vyvíjet, nasadit a spravovat konfigurační data aplikací pomocí jediné programovací rozhraní. Rozhraní API konfigurace můžete použít k vývoji a úpravě veškerou konfiguraci technologie ASP.NET prostřednictvím kódu programu bez provádění úprav v XML v konfiguračních souborech. Kromě toho můžete použít rozhraní API konfigurace v konzolové aplikace a skripty, které vyvíjíte, v nástrojích pro správu webových a modulů snap-in konzoly Microsoft Management Console (MMC).

Následující dva nástroje pro správu konfigurace pomocí rozhraní API konfigurace a jsou součástí rozhraní .NET Framework verze 2.0:

- Modulu snap-in konzoly MMC technologie ASP.NET, který používá rozhraní API konfigurace ke zjednodušení úloh správy, poskytuje ucelený místní konfiguračních dat ze všech úrovní v konfigurační hierarchii.
- Nástroj Správa webu, který umožňuje spravovat nastavení konfigurace pro místní a vzdálené aplikace, včetně hostované servery.

Rozhraní API konfigurace ASP.NET obsahuje sadu objektů správy ASP.NET, které můžete použít ke konfiguraci webové stránky a aplikace prostřednictvím kódu programu. Objekty pro správu jsou implementovány jako knihovny tříd rozhraní .NET Framework. Programovací model rozhraní API konfigurace pomáhá zajistit konzistenci kódu a spolehlivost vynucením datových typů v době kompilace. Aby bylo snazší správa konfigurací aplikace, rozhraní API konfigurace můžete zobrazit data, která se slučuje ze všech bodů v hierarchii konfigurace jako jedinou kolekci, namísto zobrazení dat jako samostatné kolekce z různých konfigurační soubory. Kromě toho konfigurace rozhraní API umožňuje manipulovat s konfiguracemi celé aplikace bez provádění úprav v XML v konfiguračních souborech. Nakonec rozhraní API zjednodušuje úkoly konfigurace podporou nástrojů pro správu, jako je například nástroj pro správu webu. Rozhraní API konfigurace zjednodušuje nasazení tak, že podpora vytváření konfigurační soubory v počítači a spouštění konfiguračních skriptů ve více počítačích.

> [!NOTE]
> Konfigurace rozhraní API nepodporuje vytváření aplikací služby IIS.


## <a name="working-with-local-and-remote-configuration-settings"></a>Práce s místní a vzdálené nastavení

Objekt konfigurace představuje sloučené zobrazení nastavení konfigurace, které se vztahují na konkrétní fyzická entita, například počítač, nebo na logická entita, jako je například aplikace nebo webu. Zadaná logická entita může existovat v místním počítači nebo na vzdáleném serveru. Pokud neexistuje žádný konfigurační soubor pro zadanou entitu, objekt konfigurace představuje výchozí nastavení konfigurace definované v souboru Machine.config.

Objekt konfigurace můžete získat pomocí jedné z metod otevřené konfigurace z následující třídy:

1. Třídu ConfigurationManager, pokud je vaše entita klientské aplikace.
2. Třída WebConfigurationManager, pokud je vaše entita webové aplikace.

Tyto metody vrátí objekt konfigurace, která zase poskytuje požadované metody a vlastnosti zpracování základní konfigurační soubory. Tyto souborům pro čtení nebo zápis.

### <a name="reading"></a>Čtení

Můžete použít metodu GetSection nebo GetSectionGroup číst informace o konfiguraci. Uživatel nebo proces, který čte musí mít oprávnění ke čtení na všechny konfigurační soubory v hierarchii.

> [!NOTE]
> Pokud používáte metodu statické GetSection, která přebírá parametr cesty, parametr path musí odkazovat na aplikaci, ve kterém kód běží. Jinak je ignorován parametr a se vrátí informace o konfiguraci pro aktuálně běžící aplikaci.


### <a name="writing"></a>Zápis

Použijte jednu z metod uložit zapsat informace o konfiguraci. Uživatel nebo proces, který zápisy musí mít oprávnění na konfigurační soubor a adresáře na aktuální úrovni konfigurace hierarchie pro zápis, jakož i oprávnění ke čtení v všechny konfigurační soubory v hierarchii.

Chcete-li vygenerovat konfiguračního souboru, který představuje zděděné konfigurační nastavení pro zadanou entitu, použijte jednu z následujících metod ukládání konfigurace:

1. Metoda uložit vytvořit nový soubor konfigurace.
2. Uložit jako metoda vygenerovat nový soubor konfigurace v jiném umístění.

## <a name="configuration-classes-and-namespaces"></a>Konfigurace třídy a obory názvů

Mnoho konfigurace třídy a metody jsou podobné navzájem. Následující tabulka popisuje třídy nejčastěji používané konfigurace a obory názvů.

| **Konfigurace třída nebo obor názvů** | **Popis** |
| --- | --- |
| [System.Configuration](https://msdn.microsoft.com/library/system.configuration.aspx) namespace | Obsahuje třídy hlavní konfigurace pro všechny aplikace rozhraní .NET Framework. Třídy obslužných rutin oddílu se používají k získání konfiguračních dat pro oddíl z metod, jako je například GetSection a GetSectionGroup. Tyto dvě metody jsou nestatické. |
| System.Configuration.Configuration – třída | Představuje sadu konfiguračních dat pro počítače, aplikace, adresáře webové nebo jiný prostředek. Tato třída obsahuje užitečné metody, jako je například GetSection a GetSectionGroup, aktualizuje se nastavení konfigurace a získání odkazů na oddíly a skupiny oddílů. Tato třída se používá jako návratový typ pro metody, které získávají data návrhu konfigurace, jako je například metody třídy WebConfigurationManager a ConfigurationManager. |
| Obor názvů System.Web.Configuration | Obsahuje třídy obslužné rutiny oddílu pro konfigurační oddíly ASP.NET definované v [nastavení konfigurace ASP.NET](https://msdn.microsoft.com/library/b5ysx397.aspx). Třídy obslužných rutin oddílu se používají k získání konfiguračních dat pro oddíl z metod, jako je například GetSection a GetSectionGroup. |
| System.Web.Configuration.WebConfigurationManager – třída | Poskytuje užitečné metody pro získání odkazy na nastavení konfigurace spuštění a návrhu. Tyto metody používat třídu System.Configuration.Configuration jako návratový typ. Můžete vytvořit statickou metodu GetSection této třídy nebo nestatickou metodu GetSection třídy System.Configuration.ConfigurationManager zcela zaměnitelným významem. Ke konfiguraci webových aplikací se doporučuje třídě System.Web.Configuration.WebConfigurationManager místo System.Configuration.ConfigurationManager třídy. |
| [System.Configuration.Provider](https://msdn.microsoft.com/library/system.configuration.provider.aspx) namespace | Poskytuje způsob, jak přizpůsobit a rozšířit zprostředkovatele konfigurace. Toto je základní třída pro všechny třídy zprostředkovatele v konfiguraci systému. |
| [System.Web.Management](https://msdn.microsoft.com/library/system.web.management.aspx) namespace | Obsahuje třídy a rozhraní pro správu a sledování stavu webových aplikací. Tento obor názvů přesněji řečeno, nepovažuje za součást konfigurace rozhraní API. Sledování a spouštění událostí se provádí pomocí třídy v tomto oboru názvů. |
| [System.Management.Instrumentation](https://msdn.microsoft.com/library/system.management.instrumentation.aspx) obor názvů | Poskytuje třídy, které jsou nezbytné pro instrumentaci aplikací ke zveřejnění své informace o správě a události prostřednictvím Windows Management Instrumentation (WMI) pro potenciální spotřebitelů. Monitorování stavu ASP.NET využívá rozhraní WMI k poskytování události. Tento obor názvů přesněji řečeno, nepovažuje za součást konfigurace rozhraní API. |

## <a name="reading-from-aspnet-configuration-files"></a>Čtení z konfigurační soubory technologie ASP.NET

Třída WebConfigurationManager je základní třídou pro čtení z konfigurační soubory technologie ASP.NET. Existují čtení souborů konfigurace ASP.NET v podstatě tři kroky:

1. Získáte pomocí metody OpenWebConfiguration objekt konfigurace.
2. Získáte odkaz na požadovaný oddíl v konfiguračním souboru.
3. Načíst požadované informace z konfiguračního souboru.

Konfigurace, který představuje objekt nepředstavuje konkrétní konfiguračního souboru. Místo toho představuje sloučené zobrazení konfigurace počítače, aplikace nebo webu. Následující příklad kódu vytvoří objekt konfigurace představující konfiguraci webové aplikace s názvem *ProductInfo*.

[!code-csharp[Main](configuration-and-instrumentation/samples/sample1.cs)]

> [!NOTE]
> Všimněte si, že pokud /ProductInfo cesta neexistuje, výše uvedený kód vrátí výchozí konfigurace jako zadaný v souboru machine.config.


Jakmile máte objekt konfigurace, potom můžete metodu GetSection nebo GetSectionGroup přejdete do nastavení konfigurace. Následující příklad získá odkaz na nastavení zosobnění pro výše uvedené ProductInfo aplikaci:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample2.cs)]

## <a name="writing-to-aspnet-configuration-files"></a>Zápis do souborů konfigurace ASP.NET

Stejně jako u čtení z konfiguračních souborů, je třída WebConfigurationManager jádra pro zápis do konfigurační soubory technologie Asp.NET. Existují také tři kroky k zápisu do konfigurační soubory technologie ASP.NET.

1. Získáte pomocí metody OpenWebConfiguration objekt konfigurace.
2. Získáte odkaz na požadovaný oddíl v konfiguračním souboru.
3. Zapsat požadované informace z konfiguračního souboru pomocí uložit nebo uložit jako metoda.

Následující kód změny **ladění** atribut &lt;kompilace&gt; element na hodnotu false:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample3.cs)]

Při spuštění tohoto kódu **ladění** atribut &lt;kompilace&gt; element bude nastaven na hodnotu false pro *webApp* souboru web.config aplikace.

## <a name="systemwebmanagement-namespace"></a>System.Web.Management Namespace

System.Web.Management obor názvů obsahuje třídy a rozhraní pro správu a monitorování stavu aplikace ASP.NET.

Protokolování se provádí tak, že definujete pravidlo, které přidružuje události zprostředkovatele. Pravidlo definuje typ události, které se odesílají do zprostředkovatele. Tyto základní události jsou k dispozici pro přihlásit se:

| **WebBaseEvent** | Událost základní třída pro všechny události. Obsahuje požadované vlastnosti pro všechny události, jako je například kód události, kód podrobnosti události, datum a čas byla vyvolána událost, pořadové číslo, zpráva o události a podrobnosti události. |
| --- | --- |
| **WebManagementEvent** | Třída základní události pro události správy, například životního cyklu aplikací, požadavku, chyby a události auditu. |
| **WebHeartbeatEvent** | Událost generovaný aplikací v pravidelných intervalech, k zaznamenání informací o stavu užitečné runtime. |
| **WebAuditEvent** | Základní třída pro události auditu zabezpečení, které slouží k označení podmínky, jako je ověření se nezdařilo, Chyba při dešifrování, *atd.* |
| **WebRequestEvent** | Základní třída pro všechny události informační žádostí. |
| **WebBaseErrorEvent** | Základní třída pro všechny události indikující chybové stavy. |

Typy poskytovatelů, které jsou k dispozici umožňují odesílat události výstup do prohlížeče událostí, SQL Server, Windows Management Instrumentation (WMI) a e-mailu. Předem nakonfigurovaná poskytovatelů a mapování události snížit množství práce potřebné k získání výstupu událostí přihlášení.

Technologie ASP.NET 2.0 používá v protokolu událostí zprostředkovatele out-of-the-box do protokolu událostí podle aplikační domény spouštění a zastavování, jakož i protokolování jakékoli neošetřené výjimky. Pomůžete tak, aby pokrývalo některé základní scénáře. Řekněme například, že aplikace vyvolá výjimku, ale uživatel nebude uložen chyba a ho nelze reprodukovat. Výchozí pravidlo protokolu událostí bude moct shromažďovat informace zásobníku výjimek a získat lepší představu o jaký typ chyby došlo k chybě. Další příklad platí v případě, že aplikace je došlo ke ztrátě stavu relace. V takovém případě můžete zobrazit v protokolu událostí k určení, zda je recyklace domény aplikace a proč domény aplikace zastavena na prvním místě.

Navíc je rozšiřitelný systém stavu sledování. Můžete například definovat vlastní webové události, je provést v rámci vaší aplikace a pak definovat pravidlo k odeslání informací o události do zprostředkovatele například e-mailu. To umožňuje snadno tie vaše instrumentace na poskytovatele sledování stavu. Například může aktivovat událost pokaždé, když je pořadí zpracování a nastavit pravidlo, které odesílá všechny události do databáze SQL serveru. Může také aktivovat událost v případě, že se uživateli nepodaří přihlásit několikrát za sebou, a nastavte událost používat e mailové zprostředkovatele.

Konfigurace pro výchozí zprostředkovatele a události je uložená v globální souboru Web.config. Globální souboru Web.config uloží všechny webové nastavení, které byly uloženy v souboru Machine.config. technologie ASP.NET 1 x. Globální soubor Web.config je umístěný v následujícím adresáři:

`%windir%\Microsoft.Net\Framework\v2.0.*\config\Web.config`

&lt;HealthMonitoring&gt; část globální soubor Web.config obsahuje výchozí nastavení konfigurace. Můžete změnit tato nastavení nebo konfigurace vlastního nastavení implementací &lt;healthMonitoring&gt; oddíl v souboru Web.config vaší aplikace.

&lt;HealthMonitoring&gt; část globální soubor Web.config obsahuje následující položky:

| **Zprostředkovatelé** | Obsahuje nastavení pro Prohlížeč událostí, rozhraní WMI a SQL Server zprostředkovatele. |
| --- | --- |
| **eventMappings** | Obsahuje mapování pro různé třídy WebBase. Tento seznam můžete rozšířit, pokud generovat třídě události. Generování třídě události získáte podrobnější přes zprostředkovatele, které odesílají informace na. Například může nakonfigurovat neošetřené výjimky k odeslání do systému SQL Server při odesílání vlastních událostí k e-mailu. |
| **Pravidla** | Odkazy eventMappings poskytovatele. |
| **Ukládání do vyrovnávací paměti** | Určit, jak často se vyprázdnit události k poskytovateli použít se systému SQL Server a e-mailu. |

Níže je příklad kódu z globální souboru Web.config.

[!code-xml[Main](configuration-and-instrumentation/samples/sample4.xml)]

## <a name="how-to-store-events-to-event-viewer"></a>Tom, jak ukládat události do prohlížeče událostí

Jak už bylo zmíněno dříve, zprostředkovatele pro protokolování událostí události prohlížeč je nakonfigurován pro vás v globální souboru Web.config. Ve výchozím nastavení, všechny události na základě **WebBaseErrorEvent** a **WebFailureAuditEvent** přihlášeni. Můžete přidat další pravidla do protokolu Další informace v protokolu událostí. Například, pokud chcete protokolovat všechny události (*tj*, každé události na základě **WebBaseEvent**), můžete přidat do souboru Web.config následující pravidlo:

[!code-xml[Main](configuration-and-instrumentation/samples/sample5.xml)]

Toto pravidlo by odkaz **všechny události** mapa událostí zprostředkovatele protokolu událostí. EventMapping a zprostředkovatele jsou zahrnuty v globální souboru Web.config.

## <a name="how-to-store-events-to-sql-server"></a>Jak k uložení událostí k systému SQL Server

Tato metoda používá **ASPNETDB** databáze, která je generován Aspnet\_regsql.exe nástroj. Výchozí zprostředkovatel používá připojovací řetězec, který používá buď databází na základě souborů v aplikaci LocalSqlServer\_data složka nebo místní SQLExpress instance systému SQL Server. Připojovací řetězec LocalSqlServer i SqlProvider jsou nakonfigurované v globální souboru Web.config.

LocalSqlServer připojovací řetězec v souboru Web.config globální vypadá takto:

[!code-xml[Main](configuration-and-instrumentation/samples/sample6.xml)]

Pokud chcete použít jinou instanci systému SQL Server, budete muset použít Aspnet\_regsql.exe nástroj, který naleznete v % windir%\Microsoft.Net\Framework\v2.0.\* složky. Použít Aspnet\_regsql.exe nástroj pro generování vlastní **ASPNETDB** databáze v instanci systému SQL Server, pak přidat připojovací řetězec do konfiguračního souboru aplikace a poté přidejte zprostředkovatele použitím nové připojovací řetězec. Jakmile máte **ASPNETDB** databázi vytvořit, budete muset nastavit pravidlo, které eventMapping propojit sqlProvider.

Ať už použijte výchozí SqlProvider nebo konfigurovat vlastní zprostředkovatele, budete muset přidat pravidlo propojení zprostředkovatele s mapou události. Následující pravidlo propojí nového poskytovatele, kterou jste vytvořili výše **všechny události** mapa událostí. Toto pravidlo bude protokolovat všechny události na základě **WebBaseEvent** a odešlete je MySqlWebEventProvider, který bude používat MYASPNETDB připojovací řetězec. Následující kód přidá pravidlo propojení zprostředkovatele s mapou události:

[!code-xml[Main](configuration-and-instrumentation/samples/sample7.xml)]

Pokud jste chtěli pouze odeslání chyb systému SQL Server, můžete přidat následující pravidlo:

[!code-xml[Main](configuration-and-instrumentation/samples/sample8.xml)]

## <a name="how-to-forward-events-to-wmi"></a>Jak předávání událostí rozhraní WMI

Události můžete dál k rozhraní WMI. Ve výchozím nastavení je nakonfigurovaná zprostředkovatele rozhraní WMI pro vás v globální souboru Web.config.

Následující příklad kódu přidá pravidlo pro předávání událostí rozhraní WMI:

[!code-xml[Main](configuration-and-instrumentation/samples/sample9.xml)]

Musíte přidat pravidlo pro přidružení eventMapping do zprostředkovatele a také k aplikaci naslouchací proces služby WMI pro naslouchání událostem. Následující příklad kódu přidá pravidlo propojení zprostředkovatele rozhraní WMI, který má **všechny události** mapa událostí:

[!code-xml[Main](configuration-and-instrumentation/samples/sample10.xml)]

## <a name="how-to-forward-events-to-email"></a>Jak přesměrovat události k e-mailu

Události k e-mailu můžete dál. Dávejte pozor, o které pravidel událostí je mapovat do svého poskytovatele e-mailu jako neúmyslně odesláním sami velké množství informací, může být vhodnější pro SQL Server nebo v protokolu událostí. Existují dva poskytovatelé e-mailu; SimpleMailWebEventProvider a TemplatedMailWebEventProvider. Každý má stejné atributy konfigurace, s výjimkou "Šablona" a "detailedTemplateErrors" atributy, které jsou dostupné jenom na TemplatedMailWebEventProvider.

> [!NOTE]
> Ani jeden z těchto poskytovatelů e-mailu je nakonfigurován pro vás. Budete muset přidat je do souboru Web.config.


Hlavní rozdíl mezi tyto dva e-mailu zprostředkovatele je, že SimpleMailWebEventProvider pošle e-mailů v obecné šablony, který nelze změnit. Ukázkový soubor Web.config tohoto zprostředkovatele e-mailu přidá do seznamu zprostředkovatelů nakonfigurované pomocí následující pravidlo:

[!code-xml[Main](configuration-and-instrumentation/samples/sample11.xml)]

Následující pravidlo je taky přidaný ke svázání zprostředkovatele e-mailu, který má **všechny události** mapa událostí:

[!code-xml[Main](configuration-and-instrumentation/samples/sample12.xml)]

## <a name="aspnet-20-tracing"></a>ASP.NET 2.0 trasování

Existují tři hlavní vylepšení trasování v technologii ASP.NET 2.0.

1. Funkce integrované trasování
2. Programový přístup k trasování zpráv
3. Vylepšené trasování na úrovni aplikace

## <a name="integrated-tracing-functionality"></a>Integrovaná funkce trasování

Teď můžete směrovat vysílaných System.Diagnostics.Trace – třída ASP.NET výstup trasování zpráv a směrování zpráv vysílaných ASP.NET trasování System.Diagnostics.Trace. K System.Diagnostics.Trace, můžete dál události instrumentace technologie ASP.NET. Tato funkce poskytuje nové **writeToDiagnosticsTrace** atribut &lt;trasování&gt; elementu. Když je tato logická hodnota PRAVDA, ASP.NET trasování zprávy jsou předávány infrastruktury System.Diagnostics trasování pro použití ve všech naslouchací procesy, které jsou registrovány k zobrazení trasování zpráv.

## <a name="programmatic-access-to-trace-messages"></a>Programový přístup k trasování zprávy

ASP.NET 2.0 umožňuje programový přístup k všechny trasovací zprávy prostřednictvím **TraceContextRecord** třídy a **TraceRecords** kolekce. Nejúčinnější způsob přístupu k trasování zprávy je k registraci **TraceContextEventHandler** delegáta (také nového v technologii ASP.NET 2.0) pro zpracování nové **TraceFinished** událostí. Podle potřeby můžete trasovací zprávy pak projít.

Následující příklad kódu zobrazuje toto:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample13.cs)]

V předchozím příkladu I projděte TraceRecords kolekci a pak každá zpráva zápisu do datového proudu odpovědi.

## <a name="improved-application-level-tracing"></a>Vylepšené trasování na úrovni aplikace

Lepší trasování na aplikační úrovni prostřednictvím zavedení nového **mostRecent** atribut &lt;trasování&gt; elementu. Tento atribut určuje, zda se zobrazí nejnovější výstup trasování na úrovni aplikace a starší data trasování mimo limity, které jsou vypsány requestLimit se zahodí. Když má hodnotu false, zobrazí se data trasování pro požadavky dokud nebude dosaženo requestLimit atribut.

## <a name="aspnet-command-line-tools"></a>Nástroje příkazového řádku ASP.NET

Existuje několik nástroje příkazového řádku, které pomáhají při konfiguraci technologie ASP.NET. Vývojáři ASP.NET měli seznámit s aspnet\_regiis.exe nástroj. Technologie ASP.NET 2.0 poskytuje tři další nástroje pro příkazový řádek pro zjednodušení konfigurace.

K dispozici jsou následující nástroje příkazového řádku:

| **Nástroj** | **Použití** |
| --- | --- |
| **aspnet\_regiis.exe** | Umožňuje zaregistrovat technologii ASP.NET se službou IIS. Existují dvě verze tohoto nástroje nastavených s prostředím ASP.NET 2.0, jeden pro 32bitové systémy (ve složce Framework) a jeden pro 64bitové systémy (ve složce Framework64.) 64bitová verze nebude nainstalován do 32bitového operačního systému. |
| **aspnet\_regsql.exe** | Nástroj registrace SQL serveru ASP.NET se používá k vytvoření databáze Microsoft SQL Server pro použití systému SQL Server zprostředkovatelé technologie ASP.NET, nebo můžete přidat nebo odebrat možnosti z existující databáze. Aspnet\_regsql.exe soubor je umístěný ve [drive:]\WINDOWS\Microsoft.NET\Framework\versionNumber složky na vašem webovém serveru. |
| **aspnet\_regbrowsers.exe** | Nástroj registrace prohlížeče technologie ASP.NET analyzuje a zkompiluje všech prohlížečů definic do sestavení a nainstaluje sestavení do globální mezipaměti sestavení. Nástroj používá definiční soubory prohlížeče (. Soubory prohlížeče) z podadresáře rozhraní .NET Framework prohlížeče. Tento nástroj naleznete v adresáři %SystemRoot%\Microsoft.NET\Framework\version\. |
| **aspnet\_compiler.exe** | Nástroj kompilace technologie ASP.NET umožňuje kompilovat webovou aplikaci ASP.NET, buď v místě, nebo pro nasazení do cílového umístění, jako je například provozní server. Kompilace v místě zlepšuje výkon aplikací, protože koncoví uživatelé nevšimnete zpoždění na první požadavek na aplikaci, zatímco kompiluje aplikace. |

Protože aspnet\_regiis.exe nástroj není nové na technologii ASP.NET 2.0, nebude probereme ho sem.

## <a name="aspnet-sql-server-registration-tool---aspnetregsqlexe"></a>Nástroj pro registraci serveru SQL technologie ASP.NET - aspnet\_regsql.exe

Můžete nastavit několik typů možností pomocí nástroje registrace SQL serveru ASP.NET. Můžete určit připojení SQL, určit, které aplikační služby používají SQL Server pro správu informací, zadat, které databázi nebo tabulku se používá pro závislost SQL mezipaměti a přidat nebo odebrat podporu pro uložení procedury a stavu relace pomocí SQL Server.

Některé služby aplikací ASP.NET využívají poskytovatele ke správě ukládání a načítání dat ze zdroje dat. Každý poskytovatel je specifická pro zdroj dat. Technologie ASP.NET obsahuje zprostředkovatele SQL Server pro následující funkce ASP.NET:

- Členství ( [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) třídy).
- Správa rolí ( [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) třídy).
- Profil ( [SqlProfileProvider](https://msdn.microsoft.com/library/system.web.profile.sqlprofileprovider.aspx) třídy).
- Přizpůsobení webových částí ( [SqlPersonalizationProvider](https://msdn.microsoft.com/library/system.web.ui.webcontrols.webparts.sqlpersonalizationprovider.aspx) třídy).
- Webové události ( [SqlWebEventProvider](https://msdn.microsoft.com/library/system.web.management.sqlwebeventprovider.aspx) třídy).

Při instalaci ASP.NET souboru Machine.config. pro váš server obsahuje konfigurační prvky, které určují poskytovatele serveru SQL Server pro každou funkci ASP.NET, které jsou závislé na zprostředkovatele. Tyto zprostředkovatele jsou nakonfigurované ve výchozím nastavení pro připojení k místní uživatelskou instanci systému SQL Server Express 2005. Pokud změníte výchozí připojovací řetězec používaný zprostředkovatele, abyste mohli používat všechny funkce ASP.NET, které jsou nakonfigurované v konfiguraci počítače, je nutné nainstalovat databázi systému SQL Server a prvky databáze pro vaši zvolenou funkci pomocí Aspnet\_regsql.exe. Pokud databáze, které zadáte pomocí nástroje registrace SQL již neexistuje (aspnetdb bude výchozí databáze Pokud není zadaná na příkazovém řádku), pak je aktuální uživatel musí mít oprávnění k vytváření databází v systému SQL Server také jako k vytvoření schématu e lements v databázi.

### <a name="sql-cache-dependency"></a>SQL Cache Dependency

Pokročilá funkce ukládání výstupu do mezipaměti technologie ASP.NET je závislost SQL mezipaměti. Závislost mezipaměti SQL podporuje dva různé režimy činnosti: jeden, který používá ASP.NET implementací cyklického dotazování tabulky a druhý režim, který používá funkce oznámení dotazů systému SQL Server 2005. Nástroj registrace SQL slouží ke konfiguraci režimu cyklického dotazování tabulky operace.

### <a name="session-state"></a>Stav relace

Ve výchozím nastavení hodnot stavu relace a informace jsou uloženy v paměti v rámci procesu ASP.NET. Alternativně můžete uložit data relace v databázi systému SQL Server, kde může být sdílen více webových serverů. Pokud databáze, který zadáte pro stav relace pomocí nástroje registrace SQL již neexistuje, musí mít aktuální uživatel oprávnění k vytvoření databáze v systému SQL Server a vytvoření prvků schématu v databázi. Pokud databáze neexistuje, musí mít aktuální uživatel oprávnění k vytváření prvků schématu ve stávající databázi.

Pro instalaci databáze stavu relace v systému SQL Server, spusťte Aspnet\_regsql.exe nástroje a zadejte následující informace pomocí příkazu:

- Název systému SQL Server instance, pomocí **-S** možnost.
- Přihlašovací pověření pro účet, který má oprávnění k vytvoření databáze v počítači se systémem SQL Server. Použít **-E** možnost používat aktuálně přihlášeného uživatele, nebo použít **- U** možnost zadat spolu s ID uživatele **-P** možnost a zadejte heslo.
- **- Ssadd** možnost příkazového řádku, přidejte databáze stavu relace.

Ve výchozím nastavení, nelze použít Aspnet\_regsql.exe nástroj pro instalaci databáze stavu relace v počítači se systémem SQL Server 2005 Express Edition.

### <a name="the-aspnet-browser-registration-tool---aspnetregbrowsersexe"></a>Nástroj registrace prohlížeče ASP.NET - aspnet\_regbrowsers.exe

V technologii ASP.NET verze 1.1, obsažené souboru Machine.config části s názvem &lt;browserCaps&gt;. Tato část obsahuje řadu XML položky, které definované na konfiguraci pro různé prohlížeče založené na regulární výraz. Pro technologii ASP.NET verze 2.0 nový. Soubor prohlížeče definuje parametry určitého prohlížeče pomocí XML položek. Informace o přidáte na nový prohlížeč tak, že přidáte nový. PROHLÍŽEČ soubor do složky umístěné na %SystemRoot%\Microsoft.NET\Framework\version\CONFIG\Browsers ve vašem systému.

Vzhledem k tomu, že aplikace není čtení souboru .config pokaždé, když potřebuje informace o prohlížeči, můžete vytvořit nový. PROHLÍŽEČ souborů a spuštění Aspnet\_regbrowsers.exe přidat požadované změny do sestavení. To umožňuje serveru přístup nové informace o prohlížeči okamžitě, takže není nutné vypnout některé z vašich aplikací a vyberte si informace. Aplikace může použít možnosti prohlížeče prostřednictvím prohlížeče vlastnost aktuálního požadavku HTTP.

Při spuštění aspnet jsou k dispozici následující možnosti\_regbrowser.exe:

| **Možnost** | **Popis** |
| --- | --- |
| **-?** | Zobrazí Aspnet\_regbbrowsers.exe textu nápovědy v okně příkazu. |
| **-i** | Vytvoří sestavení běhových možností prohlížeče a nainstaluje v globální mezipaměti sestavení. |
| **-u** | Odinstaluje sestavení běhových možností prohlížeče z globální mezipaměti sestavení. |

## <a name="the-aspnet-compilation-tool---aspnetcompilerexe"></a>Nástroj kompilace technologie ASP.NET - aspnet\_compiler.exe

Nástroj kompilace technologie ASP.NET lze použít dvěma způsoby: pro kompilaci na místě a kompilace pro nasazení, tam, kde je zadán cílový výstupní adresář.

### <a name="compiling-an-application-in-placehttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Kompilování aplikace na místě](https://msdn.microsoft.com/library/ms229863.aspx)

Nástroj kompilace technologie ASP.NET můžete zkompilovat aplikaci na místě, který je napodobuje chování zasílání více požadavků na aplikaci, což způsobuje regulární kompilace. Uživatelé předem kompilovaném lokality nebude docházet ke zpoždění způsobené kompilace stránky na první požadavek.

Při předkompilaci webu v místě, platí následující položky:

- Web zachová své soubory a struktura adresářů.
- Musíte mít kompilátory pro všechny programovací jazyky používané na serveru lokality.
- Pokud se všechny soubory nezdaří kompilace, selže celá lokalita kompilace.

Můžete také znovu zkompiluje po přidání nové zdrojové soubory k němu aplikace na místě. Nástroj zkompiluje pouze nové nebo změněné soubory, není-li je zahrnout **- c** možnost.

> [!NOTE]
> Kompilace aplikace, která obsahuje vnořené aplikaci nekompiluje vnořené aplikace. Vnořené aplikace musí být zkompilovány samostatně.


### <a name="compiling-an-application-for-deploymenthttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Kompilace aplikace pro nasazení](https://msdn.microsoft.com/library/ms229863.aspx)

Zadáním parametru targetDir zkompilujete aplikace pro nasazení (kompilace do cílového umístění). TargetDir může být do konečného umístění pro webovou aplikaci nebo kompilované aplikace můžete další nasazení. Pomocí **-u** možnost kompilaci aplikace tak, že můžete provádět změny určité soubory kompilované aplikace bez nutnosti rekompilace se. ASPNET\_compiler.exe rozlišuje statické a dynamické typy souborů a zpracovává je jiným způsobem, při vytváření výsledné aplikace.

- Statické typy souborů jsou ty, které jsou asociovány s kompilátorem nebo poskytovatelem, například soubory, jehož sestavení s názvem mají přípony .css, .gif, htm, HTML, JPG, JS a tak dále. Tyto soubory jsou jednoduše zkopírovat do cílového umístění, jejich relativní umístění struktury adresářů zachovaná.
- Dynamické typy souborů jsou ty, které jsou asociovány s kompilátorem nebo sestavení zprostředkovatele, včetně souborů s příponami názvů souborů ASP.NET konkrétní například .asax, .ascx, .ashx, .aspx, Browser, .master a tak dále. Nástroj kompilace technologie ASP.NET generuje sestavení z těchto souborů. Pokud **-u** není zadán parametr, nástroj také vytvoří soubory s příponou názvu souboru. ZKOMPILOVANÉ, které mapují původní zdrojové soubory k jejich sestavení. Aby se zajistilo, že se zachová, i strukturu adresáře aplikace zdroje, Nástroj generuje soubory zástupný symbol v odpovídajících umístění v cílové aplikaci.

Je nutné použít **-u** možnost upravovat obsah kompilované aplikace. Následné úpravy, jinak se ignorují nebo způsobit chyby za běhu.

Následující tabulka popisuje, jak kompilace technologie ASP.NET nástroj zpracovává různé typy souborů, když **-u** je zadán parametr.

| **Typ souboru** | **Akce kompilátoru** |
| --- | --- |
| .ascx, .aspx, .master | Tyto soubory jsou rozděleny do značkami a zdrojový kód, který obsahuje soubory kódu a kód, který je součástí &lt;skript runat = "server"&gt; elementy. Zdrojový kód je zkompilován do sestavení s názvy, které jsou odvozeny od algoritmu hash a sestavení jsou umístěny v adresáři Bin. Všechny vloženého kódu, který je, uzavřen mezi značkami kódu **&lt; %** a **% &gt;** závorky, je součástí značek a není zkompilovat. Nové soubory se stejným názvem jako zdrojové soubory jsou vytvořeny tak, aby obsahovala kód a umístit do odpovídajících výstupních adresářů. |
| .ashx, .asmx | Tyto soubory nejsou kompilovány a přesunou do výstupního adresáře je a není zkompilovat. Pokud chcete mít zkompilován kód obslužné rutiny, umístěte kód do soubory zdrojového kódu v aplikaci\_adresář kódu. |
| .cs, VB, .jsl, sada (včetně není soubory kódu pro výše uvedených typů souborů) | Tyto soubory jsou kompilované a zahrnuty jako prostředek v sestavení, která na ně odkazovat. Zdrojové soubory nebudou zkopírovány do výstupního adresáře. Pokud není soubor kódu odkazována, není zkompilovat. |
| Vlastní typy souborů | Tyto soubory nejsou zkompilovat. Tyto soubory se zkopírují do odpovídajících výstupních adresářů. |
| Zdrojové soubory kódu v aplikaci\_podadresáře kódu | Tyto soubory jsou zkompilovány do sestavení a umístit do adresáře Bin. |
| soubory RESX a Resource v aplikaci\_GlobalResources podadresáři | Tyto soubory jsou zkompilovány do sestavení a umístit do adresáře Bin. Žádná aplikace\_v hlavní výstupní adresář vytvořen podadresář GlobalResources a umístěný v adresáři zdrojové soubory RESX a .resources se zkopírují do výstupního adresáře. |
| soubory RESX a Resource v aplikaci\_LocalResources podadresáři | Tyto soubory nejsou kompilovány a zkopírují do odpovídajících výstupních adresářů. |
| soubory SKIN v aplikaci\_podadresáři motivů | Soubory skin a statické soubory motivů nejsou kompilovány a zkopírují do odpovídajících výstupních adresářů. |
| Browser souboru Web.config statické typy již existuje v adresáři Bin sestavení | Tyto soubory jsou zkopírovány do výstupního adresáře. |

Následující tabulka popisuje, jak kompilace technologie ASP.NET nástroj zpracovává různé typy souborů, když **-u** není zadán parametr.

| **Typ souboru** | **Akce kompilátoru** |
| --- | --- |
| .aspx, .asmx, .ashx, .master | Tyto soubory jsou rozděleny do značkami a zdrojový kód, který obsahuje soubory kódu a kód, který je součástí &lt;skript runat = "server"&gt; elementy. Zdrojový kód je zkompilovat do sestavení s názvy, které jsou odvozeny od algoritmu hash. Výsledná sestavení jsou umístěny v adresáři Bin. Všechny vloženého kódu, který je, uzavřen mezi značkami kódu **&lt; %** a **% &gt;** závorky, je součástí značek a není zkompilovat. Kompilátor vytvoří nové soubory tak, aby obsahovala kód se stejným názvem jako zdrojové soubory. Tyto výsledné soubory jsou umístěny v adresáři Bin. Kompilátor také vytvoří soubory se stejným názvem jako zdrojové soubory, ale s příponou. ZKOMPILOVANÉ, které obsahují informace o mapování. Na. ZKOMPILOVANÉ soubory jsou umístěny v adresáři výstup odpovídající do původního umístění zdrojových souborů. |
| .ascx | Tyto soubory jsou rozděleny do značkami a zdrojový kód. Zdrojový kód je zkompilovány do sestavení a umístěn v adresáři Bin, s názvy, které jsou odvozeny od algoritmu hash. Jsou generovány žádné soubory značek. |
| .cs, VB, .jsl, sada (včetně není soubory kódu pro výše uvedených typů souborů) | Zdrojový kód, který je odkazován generovaná z .ascx, .ashx nebo soubory .aspx sestavení je zkompilovány do sestavení a umístit do adresáře Bin. Žádné zdrojové soubory jsou kopírovány. |
| Vlastní typy souborů | Tyto soubory jsou kompilovat, jako jsou dynamické soubory. V závislosti na typu souboru, které jsou založené na kompilátor mapování soubory umístit do výstupního adresáře. |
| Soubory v aplikaci\_podadresáře kódu | Soubory zdrojového kódu v tomto podadresáři jsou zkompilovány do sestavení a umístit do adresáře Bin. |
| Soubory v aplikaci\_GlobalResources podadresáři | Tyto soubory jsou zkompilovány do sestavení a umístit do adresáře Bin. Žádná aplikace\_GlobalResources podadresáři se vytvořil v rámci hlavní výstupnímu adresáři. Pokud konfigurační soubor Určuje appliesTo = "Vše", soubory RESX a Resources se zkopírují do výstupního adresáře. Pokud se odkazuje nejsou zkopírovány [zprostředkovatele BuildProvider](https://msdn.microsoft.com/library/system.web.configuration.buildprovider.aspx). |
| soubory RESX a Resource v aplikaci\_LocalResources podadresáři | Tyto soubory jsou zkompilovány do sestavení s jedinečnými názvy a umístit do adresáře Bin. Žádné soubory RESX a Resource se zkopírují do výstupního adresáře. |
| soubory SKIN v aplikaci\_podadresáři motivů | Motivy jsou zkompilovány do sestavení a umístit do adresáře Bin. Soubory se zakázaným inzerováním jsou vytvořené pro soubory skin a umístěn v adresáři odpovídající výstup. Statické soubory (například .css) se zkopírují do výstupního adresáře. |
| Browser souboru Web.config statické typy již existuje v adresáři Bin sestavení | Tyto soubory budou zkopírovány do výstupního adresáře. |

### <a name="fixed-assembly-nameshttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Sestavení s pevnými názvy](https://msdn.microsoft.com/library/ms229863.aspx##)

Některé scénáře, jako je nasazení webové aplikace pomocí Instalační služby MSI, vyžadují použití konzistentních názvů souborů a obsah, jakož i konzistentní adresářovou strukturu k identifikaci sestavení nebo konfigurace nastavení pro aktualizace. V takových případech můžete použít **- fixednames** můžete určit, že nástroj kompilace technologie ASP.NET má kompilace sestavení pro každý zdrojový soubor, místo použití where více stránek kompilované do sestavení. To může vést k velký počet sestavení, a proto pokud máte obavy s škálovatelnost použijte tuto možnost používejte opatrně.

### <a name="strong-name-compilationhttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Kompilace se silnými názvy](https://msdn.microsoft.com/library/ms229863.aspx##)

**- Aptca**, **- delaysign**, **- keycontainer** a **- keyfile** možnosti jsou k dispozici, aby můžete Aspnet\_ Compiler.exe vytvořit silně pojmenované sestavení bez použití [Strong Name Tool (Sn.exe)](https://msdn.microsoft.com/library/k5b5tt23.aspx) samostatně. Tyto parametry odpovídají do **AllowPartiallyTrustedCallersAttribute**, **AssemblyDelaySignAttribute**, **AssemblyKeyNameAttribute**a  **AssemblyKeyFileAttribute**.

Informace o těchto atributů jsou mimo rozsah tohoto kurzu.

## <a name="labs"></a>Labs

Každý z následujících labs založený na předchozí laboratoře. Musíte se jejich provedení v pořadí.

## <a name="lab-1-using-the-configuration-api"></a>Laboratoř 1: Použití rozhraní API konfigurace

1. Vytvořit nový webový server s názvem *mod9lab*.
2. Do lokality přidáte nový soubor webové konfigurace.
3. Přidejte následující v souboru web.config:


[!code-xml[Main](configuration-and-instrumentation/samples/sample14.xml)]

Tím bude zajištěno, že máte oprávnění ukládat změny do souboru web.config.

1. Přidejte nové ovládací prvek popisek do Default.aspx a změnit ID na **lblDebugStatus**.
2. Přidejte nový ovládací prvek tlačítko do Default.aspx.
3. Změnit ID ovládacího prvku tlačítko na **btnToggleDebug** a Text, který se **ladění stav přepnutí**.
4. Otevřete zobrazení kódu pro kód na pozadí soubor Default.aspx a přidejte **pomocí** příkaz pro **System.Web.Configuration** následujícím způsobem:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample15.cs)]

1. Přidejte dva privátní proměnné třídy a na stránce\_init – metoda, jak je uvedeno níže:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample16.cs)]

1. Přidejte následující kód na stránku\_zatížení:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample17.cs)]

1. Uložte a procházet default.aspx. Všimněte si, že ovládací prvek popisek zobrazuje aktuální stav ladění.
2. Dvakrát klikněte na tlačítko ovládací prvek v návrháři a klikněte na události pro ovládací prvek tlačítko přidejte následující kód:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample18.cs)]

1. Uložte a procházet default.aspx a klikněte na tlačítko.
2. Otevřete soubor web.config po každé tlačítko klikněte a sledovat **ladění** atribut &lt;kompilace&gt; části.

## <a name="lab-2-logging-application-restarts"></a>Laboratoř 2: Protokolování restartování aplikace

V tomto testovacím prostředí vytvoříte kód, který vám umožní přepnutí protokolování aplikace vypínání, startupy a předkompilací v prohlížeči událostí.

1. Přidejte do default.aspx rozevírací seznam a změňte ID ddlLogAppEvents.
2. Nastavte **automatické odeslání** vlastnost pro rozevírací seznam pro **true**.
3. Přidejte tři položky do kolekce položky pro rozevírací seznam. Ujistěte se, **Text** pro první položku *Select Value* a hodnota -1. Ujistěte se, **Text** a **hodnotu** druhý položky **True** a **Text** a **hodnotu** třetí položky **False**.
4. Přidání nového štítku do default.aspx. Změnit ID na **lblLogAppEvents**.
5. Otevřete zobrazení kódu pro default.aspx a přidejte novou deklaraci proměnné typu HealthMonitoringSection, jak je uvedeno níže:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample19.cs)]

1. Přidejte následující kód do existujícího kódu na stránce\_Init:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample20.cs)]

1. Dvakrát klikněte na rozevírací seznam a přidejte následující kód do SelectedIndexChanged událostí:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample21.cs)]

1. Procházejte default.aspx.
2. Nastavte rozevírací nabídce na **False**.
3. Vymažte protokol aplikace v prohlížeči událostí.
4. Klikněte na tlačítko změnit atribut ladění pro aplikace.
5. Aktualizujte protokolu aplikace v prohlížeči událostí. 

    1. Byly zaznamenány žádné události?
    2. Proč nebo proč ne?
6. Nastavte rozevírací nabídce na **hodnotu True.**
7. Kliknutím na tlačítko Zapnout atribut ladění pro aplikace.
8. Aktualizujte aplikaci přihlášení v prohlížeči událostí. 

    1. Byly zaznamenány žádné události?
    2. Jaká byla z důvodu pro vypnutí aplikací?
9. Vyzkoušejte zapnutí a vypnutí protokolování a podívejte se na změny provedené v souboru web.config.

## <a name="more-information"></a>Další informace:

ASP.NET 2.0 je model poskytovatelů umožňuje vytvářet vlastní zprostředkovatele, ne jenom instrumentace aplikací, ale pro mnoho dalších situací například členství, profily atd. Podrobné informace o psaní vlastního zprostředkovatele do protokolu událostí aplikace do textového souboru, najdete v článku [tento odkaz](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/ASPNETProvMod_Prt6.asp).
