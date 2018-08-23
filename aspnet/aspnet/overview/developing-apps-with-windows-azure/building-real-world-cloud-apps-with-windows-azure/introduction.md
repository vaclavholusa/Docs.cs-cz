---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
title: Sestavování skutečných cloudových aplikací s Azure | Dokumentace Microsoftu
author: MikeWasson
description: Tato elektronická kniha vás provede přístup založený na základě vzorů vytvořením skutečných cloudových řešení. Tyto vzory se dají použít jak na proces vývoje stejně jako k...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: accfa16a-ab15-4c26-9ad4-babdc2a77d2e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
msc.type: authoredcontent
ms.openlocfilehash: eade14bc27e2bface84fb0bdd2f3c5bf8ef28432
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752511"
---
<a name="building-real-world-cloud-apps-with-azure"></a>Sestavování skutečných cloudových aplikací s Azure
====================
podle [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Petr Dykstra](https://github.com/tdykstra)

[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronickou knihu](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Tato elektronická kniha vás provede přístup založený na základě vzorů vytvořením skutečných cloudových řešení. Tyto vzory se dají použít jak na proces vývoje, tak na architekturu a kódování.
> 
> Obsah je založený na prezentaci Scotta guthrie připravil a přednesl jím na Ndc Norwegian Developers Conference () v červnu 2013 ([1. část](http://vimeo.com/68215538), [2. část](http://vimeo.com/68215602)) a na Microsoft Tech Ed Austrálie v Září 2013 ([1. část](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324), [2. část](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325)). [Řada dalších](more-patterns-and-guidance.md#acknowledgments) aktualizovala a vylepšila obsahu při převodu z videa do psané formy.


## <a name="intended-audience"></a>Cílová skupina

Vývojáři, kteří jsou zajímá vás vývoj pro cloud, zvažujete Přesun do cloudu nebo vývoj cloudu úplná novinka zde najdete stručný přehled ty nejdůležitější koncepty a postupy, které potřebují vědět. Koncepty se popisují na konkrétních příkladech a každá kapitola odkazy na další zdroje informací pro podrobnější informace. Příklady a odkazy na další informace se vztahují na platformy a služby Microsoftu, ale zásady znázorněno platí pro další webové vývojářské platformy a Cloudová prostředí také.

Vývojáři, kteří už vyvíjíte pro cloud, možná zjistíte, že je zde nápady, které vám pomůže provádět větších úspěchů. Každá kapitola z této série lze číst nezávisle na sobě, můžete vybrat a zvolte témata, která vás zajímá.

Každý, kdo sledovali vysílání televizní Scott Guthrie *vytváření reálného světa cloudových aplikací s Azure* prezentace a potřebuje více podrobností a aktualizované informace najdete, které tady.

<a id="patterns"></a>
## <a name="cloud-development-patterns"></a>Cloudové vývojové vzory

Tato elektronická příručka vysvětluje že třináct doporučuje vzory pro cloudový vývoj. "Vzor" se zde používá v širokém smyslu k doporučený způsob, jak provádět akce na mysli: jak nejlepší postupovali při vývoji, návrh a kódování cloudových aplikací. Toto jsou klíčové vzorce, které vám pomůžou "spadají do pit úspěchu" Pokud budete postupovat podle nich.

- [Plná automatizace](automate-everything.md).

    - Pro maximalizaci efektivity a minimalizovat chyby v opakované procesy pomocí skriptů.
    - Ukázka: Skripty správy Azure.
- [Správa zdrojového kódu](source-control.md). 

    - Nastavení struktury větvení ve správě zdrojového kódu pro usnadnění pracovních postupů DevOps.
    - Ukázka: přidáte skripty do správy zdrojového kódu.
    - Ukázka: uchovávat citlivá data ze správy zdrojového kódu.
    - Ukázka: pomocí systému Git v sadě Visual Studio.
- [Průběžná integrace a doručování](continuous-integration-and-continuous-delivery.md). 

    - Automatizace sestavení a nasazení s každou zdrojového ovládacího prvku vrácení se změnami.
- [Osvědčené postupy při vývoji webových](web-development-best-practices.md). 

    - Zachovejte bezstavové webové vrstvy.
    - Ukázka: škálování a automatické škálování ve službě Web Apps ve službě Azure App Service.
    - Vyhněte se stav relace.
    - Používejte síť CDN v záložní CDN není k dispozici.
    - Použití asynchronního programovacího modelu.
    - Ukázka: asynchronní v ASP.NET MVC a Entity Framework.
- [Jednotné přihlašování](single-sign-on.md). 

    - Úvod do služby Azure Active Directory.
    - Ukázka: vytvoření aplikace ASP.NET využívající Azure Active Directory.
- [Možnosti úložiště dat](data-storage-options.md). 

    - Typy úložiště dat
    - Jak zvolit správného úložiště dat došlo k chybě.
    - Ukázka: Azure SQL Database.
- [Strategie dělení dat](data-partitioning-strategies.md). 

    - Rozdělení dat svisle, vodorovně, nebo obojí pro usnadnění škálování relační databáze.
- [Nestrukturované úložiště objektů blob](unstructured-blob-storage.md). 

    - Store soubory v cloudu pomocí služby blob service.
    - Ukázka: použití úložiště objektů blob v aplikace Fix It.
- [Návrh odolný proti chybám](design-to-survive-failures.md). 

    - Typy selhání.
    - Rozsah selhání.
    - Principy smlouvy o úrovni služeb.
- [Monitorování a telemetrie](monitoring-and-telemetry.md). 

    - Proč by měl jak koupit aplikaci telemetrie a psaní vlastního kódu k instrumentaci vaší aplikace.
    - Ukázka: Špičkové funkce New Relicu pro Azure
    - Ukázka: protokolování kódu v aplikaci opravit.
    - Ukázka: injektáž závislostí v aplikaci opravit.
    - Ukázka: podpora vestavěné protokolování v Azure.
- [Zpracování přechodných chyb](transient-fault-handling.md). 

    - Použijte inteligentní opakování/regresní logiku ke zmírnění dopadu přechodná selhání.
    - Ukázka: opakování/Regrese v Entity Framework 6.
- [Distribuované ukládání do mezipaměti](distributed-caching.md). 

    - Škálovatelnost a snižuje náklady na transakce databáze s použitím distribuované ukládání do mezipaměti.
- [Pracovní postup založený na frontě](queue-centric-work-pattern.md). 

    - Povolit vysokou dostupnost a zlepšit škálovatelnost volně párování webové a pracovní vrstvu.
    - Ukázka: Fronty úložiště Azure v aplikaci opravit.
- [Používejte víc cloudových aplikační postupy a doprovodné materiály](more-patterns-and-guidance.md).
- [Příloha: ukázková aplikace Fix It](the-fix-it-sample-application.md)

    - Známé problémy
    - Doporučené postupy
    - Jak ke stažení, vytvoření, spuštění a nasazení.

Tyto vzory se dají použít pro všechna prostředí v cloudu, ale jsme vám ukazují pomocí příklady založené na technologiích Microsoftu a služby, jako je Visual Studio, Team Foundation Service, ASP.NET a Azure.

Tato zbývající část této kapitole zavádí ukázková aplikace Fix It a webové aplikace v cloudovém prostředí Azure App Service, na kterém běží aplikace Fix It v.

<a id="fixit"></a>
## <a name="the-fix-it-sample-application"></a>Oprava ukázkové aplikace

Snímky obrazovky a kód příkladů uvedených v této e kniha maximum jsou založeny na aplikace Fix It, který se původně vyvinutý [Scott Guthrie](https://weblogs.asp.net/scottgu/) k předvedení doporučené cloudové aplikace vývojové vzory a postupy.

![Oprava Domovská stránka aplikace](introduction/_static/image1.png)

Ukázková aplikace je systém lístkování jednoduchý pracovní položky. Pokud potřebujete něco opravit, vytvořit lístek a přiřaďte ji pro uživatele a ostatní uživatele přihlásit a zobrazit lístky přiřazené k nim a označit lístky, jako je dokončena po dokončení práce.

Je standardní webový projekt sady Visual Studio. Je založena na rozhraní ASP.NET MVC a používá databázi serveru SQL Server. Můžete spustit místně v rámci služby IIS Express a je možné nasadit na web Azure ke spuštění v cloudu. Můžete protokolovat v použití ověřování pomocí formulářů a místní databázi nebo pomocí poskytovatele sociálních sítí, jako je Google. (Později vám také ukážeme postup přihlášení pomocí účtu organizace služby Active Directory.)

![S přihlašovací stránkou](introduction/_static/image2.png)

Jakmile budete přihlášeni v můžete vytvořit lístek, přiřadit někomu a nahrát obrázek má dojít k opravě.

![Vytvořte úlohu Fix It](introduction/_static/image3.png)

![Oprava úlohy vytvořené](introduction/_static/image4.png)

Můžete sledovat průběh pracovních položek, které jste vytvořili naleznete v tématu lístky přiřazena vám, najdete v podrobnostech lístků a označit položky jako dokončené.

To je velmi jednoduchá aplikace z hlediska funkcí, ale uvidíte, jak ji sestavit tak, aby možné škálovat na miliony uživatelů a bude odolný vůči věci, jako je chyb databáze a ukončení připojení. Uvidíte také tom, jak vytvořit pracovní postup automatizované a agilního vývoje, který umožňuje začít jednoduchými věcmi a aby se aplikace vyšší a vyšší iterací vývojového cyklu efektivně a rychle.

<a id="waws"></a>
## <a name="web-apps-in-azure-app-service"></a>Web Apps ve službě Azure App Service

Cloudové prostředí, používá pro aplikace Fix It je služba Azure, kterému říkáme webové stránky. Tato služba je tak, že můžete hostovat webové aplikace v Azure bez nutnosti vytvářet virtuální počítače a jejich aktualizaci, nainstalovat a nakonfigurovat službu IIS atd. Budeme hostovat svůj web na naše virtuální počítače a automaticky poskytují zálohování a obnovení a další služby. Web Sites service funguje s ASP.NET, Node.js, PHP a Pythonu. Umožňuje nasadit velice rychle pomocí sady Visual Studio, Webdeploy, FTP, Gitu nebo TFS. Je to obvykle jen několik sekund mezi časem spuštění nasazení a čas, který je k dispozici vaše aktualizace přes Internet. Je pro začátek vše zdarma a můžete vertikálně navýšit kapacitu při nárůstu provozu.

Na pozadí Web Apps ve službě Azure App Service poskytuje mnoho architekturálních komponentách a funkce, které budete muset sestavit sami, pokud se chystáte hostovat webovou stránku pomocí služby IIS na vlastní virtuální počítače. Jedna komponenta je koncový bod nasazení, který automaticky nakonfiguruje službu IIS a nainstaluje aplikaci na libovolný počet virtuálních počítačů, jak chcete spustit webu.

![Nasazení služby](introduction/_static/image5.png)

Když uživatel dosáhne webovou stránku, není přímo spuštění virtuálních počítačů služby IIS, které procházejí [požádat o směrování žádostí na aplikace](https://www.iis.net/downloads/microsoft/application-request-routing) nástroje pro vyrovnávání zatížení. Můžete je použít pomocí vašich vlastních serverech, ale výhoda zde spočívá v tom, že máte nastavené pro vás automaticky. Používají inteligentní heuristiky, který bere v úvahu faktory, jako je spřažení relace, hloubka fronty ve službě IIS, a využití procesoru na každém počítači směrovat přenos dat do virtuálních počítačů, které jsou hostiteli vaše webová stránka.

![Nástroj pro vyrovnávání zatížení směrování žádostí na aplikace](introduction/_static/image6.png)

Pokud se počítač ocitne mimo provoz, Azure automaticky ji stáhne z otočení, spuštění nové instance virtuálních počítačů a začne směrovat provoz do nové instance – vše bez výpadkům pro vaši aplikaci.

![Automatické obnovení po selhání počítače](introduction/_static/image7.png)

To vše probíhá automaticky. Všechno, co je třeba provést je vytvořit webovou stránku a nasazení aplikace, pomocí prostředí Windows PowerShell, sady Visual Studio nebo portálu pro správu Azure.

Rychlé a snadné podrobný kurz, který ukazuje, jak vytvořit webovou aplikaci v sadě Visual Studio a nasadit ho na web Azure, najdete v tématu [Začínáme s Azure a ASP.NET](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

<a id="summary"></a>
## <a name="summary"></a>Souhrn

Tento úvod poskytuje seznam témata, která bude zahrnovat knihy, snímky obrazovky ukázkové aplikace a stručný přehled služby Web Apps ve službě Azure App Service cloudovém prostředí. Jeden z velkých výhod vývoj aplikací v cloudu je, že se jedná o usnadňuje automatizaci opakovaných vývojářských úloh, jako jsou vytvoření testovacího prostředí a nasazení kódu do něj. Postup, který je předmětem [další kapitolu](automate-everything.md).

## <a name="resources"></a>Prostředky

Další informace o tématech zahrnuté v této kapitole najdete v následující prostředky.

Dokumentace ke službě:

- [Webové aplikace ve službě Azure App Service](https://azure.microsoft.com/services/app-service/web/). Stránka portálu Azure dokumentaci ke službě Web Apps.
- [Webové aplikace, služby Cloud Services a virtuální počítače: kdy co použít?](https://azure.microsoft.com/documentation/articles/choose-web-site-cloud-service-vm/) Jak je znázorněno v této kapitole WAWS je pouze jedním ze tří způsobů, které můžete spouštět webové aplikace v Azure. Tento článek vysvětluje rozdíly mezi třemi způsoby a poskytuje pokyny o tom, jak zvolit, která z nich je pro váš scénář nejvhodnější. Podobně jako weby Cloud Services je funkce Azure PaaS. Virtuální počítače jsou funkce služby IaaS. Vysvětlení model PaaS a kdy IaaS, najdete v článku [možnosti dat](data-storage-options.md#paasiaas) kapitoly.

Videa:

- [Scott Guthrie začíná v kroku 0 – co je Azure Cloud OS?](https://azure.microsoft.com/documentation/videos/what-is-the-cloud-os-scottgu/)
- [Architektura webové servery – s Stefan Schackow](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/).
- [Interní informace o Azure Web Sites se nir a jsem Mashkowski](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski).

> [!div class="step-by-step"]
> [Next](automate-everything.md)
