---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
title: Vytváření reálných cloudových aplikací s Azure | Microsoft Docs
author: MikeWasson
description: Tato e kniha vás provede prostřednictvím přístupu na základě vzory pro vytváření reálných cloudových řešení. Vzory použít pro proces vývoj stejně jako na...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: accfa16a-ab15-4c26-9ad4-babdc2a77d2e
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
msc.type: authoredcontent
ms.openlocfilehash: 5a62818a2dc21128bb0a42a8b296ade460e7b060
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870528"
---
<a name="building-real-world-cloud-apps-with-azure"></a>Vytváření reálných cloudových aplikací s Azure
====================
podle [Karel Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [tní Dykstra](https://github.com/tdykstra)

[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronická kniha](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Tato e kniha vás provede prostřednictvím přístupu na základě vzory pro vytváření reálných cloudových řešení. Vzory použít k procesu vývoje, a také architektura a postupy kódování.
> 
> Obsah je založena na prezentaci vyvinuté Scott Guthrie a doručil mu v Norská konferenční vývojáři (NDC) v červen 2013 ([část 1](http://vimeo.com/68215538), [část 2](http://vimeo.com/68215602)) a v Microsoft Technická Ed Austrálie v Září 2013 ([část 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324), [část 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325)). [Mnoho dalších](more-patterns-and-guidance.md#acknowledgments) aktualizován a rozšířen obsahu při přechodu z video napsané formuláře.


## <a name="intended-audience"></a>Cílová skupina

Vývojáři, kteří jsou zvědaví o vývoji pro cloud, vzhledem k tomu, přesunout do cloudu, nebo jsou nové pro vývoj v cloudu zde najdete stručný přehled nejdůležitější koncepty a postupy, které potřebují vědět. Koncepty jsou zobrazené s konkrétní příklady a každý kapitoly odkazy na další zdroje pro podrobnější informace. Jsou příklady a odkazy na další zdroje pro rozhraní Microsoft a služby, ale zásady ilustrovaný použít jiné architektury pro vývoj webových a prostředí i v cloudu.

Vývojáři, kteří jsou již vývoji pro cloud zjistit, že je zde nápady, které vám pomůžou provést více úspěšné. Každý kapitoly v řadě lze číst nezávisle, abyste mohli vybrat a vyberte témata, která vás zajímá.

Každý, kdo sledovaná Scott Guthrie *vytváření reálného světa cloudových aplikací s Azure* prezentace a chce další podrobnosti a aktualizované informace zjistí, které zde.

<a id="patterns"></a>
## <a name="cloud-development-patterns"></a>Vývojové vzory cloudu

Tato e kniha vysvětluje že třináct doporučená vzory pro vývoj cloudové. "Vzor" se používá v tomto poli v širokém smyslu rozumí doporučený způsob, jak provádět následující akce: jak nejlepší přejděte o vývoji, navrhování a kódování cloudových aplikací. Toto jsou klíčové vzorců, které vám pomůžou "patří do pit úspěchu" Pokud budete postupovat podle nich.

- [Automatizovat všechno, co](automate-everything.md).

    - Pomocí skriptů maximalizovat efektivitu a minimalizovat chyby v opakovaných procesy.
    - Ukázka: Skripty správy Azure.
- [Správa zdrojů](source-control.md). 

    - Nastavení větvení struktury ve správě zdrojového kódu pro usnadnění DevOps pracovního postupu.
    - Ukázka: přidáte skripty k řízení zdrojů.
    - Ukázka: Uchovávejte citlivá data ze zdrojového kódu.
    - Ukázka: pomocí Git v sadě Visual Studio.
- [Průběžnou integraci a doručení](continuous-integration-and-continuous-delivery.md). 

    - Automatizovat sestavení a nasazení s každý zdroj ovládacího prvku vrácení se změnami.
- [Osvědčené postupy pro vývoj webových](web-development-best-practices.md). 

    - Zachovat webová vrstva bezstavové.
    - Ukázka: škálování a automatické škálování ve službě Web Apps v Azure App Service.
    - Vyhněte se stav relace.
    - Když není k dispozici CDN, používají název CDN s zálohu.
    - Použijte asynchronní programovací model.
    - Ukázka: asynchronní v architektuře ASP.NET MVC a Entity Framework.
- [Jednotné přihlašování](single-sign-on.md). 

    - Úvod do služby Azure Active Directory.
    - Ukázka: Vytvořte aplikaci ASP.NET, která využívá Azure Active Directory.
- [Možnosti ukládání dat](data-storage-options.md). 

    - Typy dat úložiště.
    - Jak zvolit úložiště správná data.
    - Ukázka: Azure SQL Database.
- [Data dělení strategie](data-partitioning-strategies.md). 

    - Rozdělení dat ve svislém směru, vodorovně, nebo obojí usnadňuje škálování relační databáze.
- [Úložiště objektů blob nestrukturovaných](unstructured-blob-storage.md). 

    - Ukládat soubory v cloudu pomocí služby objektů blob.
    - Ukázka: pomocí úložiště objektů blob v aplikaci opravit.
- [Návrh při selhání](design-to-survive-failures.md). 

    - Typy chyb.
    - Selhání oboru.
    - Principy SLA.
- [Monitorování a telemetrie](monitoring-and-telemetry.md). 

    - Proč by měl jak koupit telemetrie aplikace a napsat vlastní kód k instrumentaci vaší aplikace.
    - Ukázka: New Relic pro Azure.
    - Ukázka: protokolování kódu v aplikaci opravit.
    - Ukázka: vkládání závislostí v aplikaci opravit.
    - Ukázka: Podpora integrované protokolování v Azure.
- [Přechodná chyba zpracování](transient-fault-handling.md). 

    - Použijte logiku inteligentní opakování nebo back vypnout zmírnit účinku přechodných chyb.
    - Ukázka: opakování nebo back vypnout na Entity Framework 6.
- [Distribuované ukládání do mezipaměti](distributed-caching.md). 

    - Zlepšit škálovatelnost a snížit náklady na databázi transakce s použitím distribuované ukládání do mezipaměti.
- [Fronty způsob práce zaměřený](queue-centric-work-pattern.md). 

    - Povolit vysokou dostupnost a zlepšit škálovatelnost volně spojovacích webové a pracovní vrstev.
    - Ukázka: Fronty úložiště Azure v aplikaci opravit.
- [Více cloudové aplikace vzory a pokyny](more-patterns-and-guidance.md).
- [Příloha: ukázková aplikace Fix It](the-fix-it-sample-application.md)

    - Známé problémy
    - Doporučené postupy
    - Jak ke stažení, vytvoření, spuštění a nasazení.

Tyto vzory platí pro všechna prostředí cloudu, ale nemůžeme budete ilustrují pomocí příklady založené na technologiích společnosti Microsoft a služby, jako je Visual Studio, Team Foundation Service, ASP.NET a Azure.

Tento zbytek této kapitoly představuje ukázkovou aplikaci opravte ji a webové aplikace v prostředí cloudu Azure App Service, které aplikace opravte běží v.

<a id="fixit"></a>
## <a name="the-fix-it-sample-application"></a>Opravte ji ukázkové aplikace

Většina snímky obrazovky a ukázky kódu, které jsou uvedené v této e knihy jsou založené na opravte ji aplikace vyvinuté původně [Scott Guthrie](https://weblogs.asp.net/scottgu/) k předvedení doporučené cloudové aplikace vývojové vzory a postupy.

![Automatická oprava aplikace domovské stránky](introduction/_static/image1.png)

Ukázková aplikace je používán systém jednoduché pracovní položky. Pokud budete potřebovat něco opravit, vytvoření lístku a přiřaďte ho pro někdo a ostatní uživatele přihlásit a zobrazit lístky přiřazené do je a označit lístků, jako při práci.

Je standardní webového projektu sady Visual Studio. To je založený na rozhraní ASP.NET MVC a používá databázi systému SQL Server. Ho můžete spustit místně v IIS Express a dá se nasadit na web Azure ke spuštění v cloudu. Může přihlásit pomocí ověřování pomocí formulářů a místní databázi nebo pomocí poskytovatele sociálních třeba Google. (Později jsme budete také ukazují, jak se přihlásit pomocí účtu organizace služby Active Directory.)

![Přihlaste se na stránce](introduction/_static/image2.png)

Jakmile se protokolují v můžete vytvořit lístek, přiřadit někomu a nahrát snímek co chcete získat pevné.

![Vytvoření úlohy, opravte ji](introduction/_static/image3.png)

![Automatická oprava vytvořen úkol](introduction/_static/image4.png)

Můžete sledovat průběh pracovních položek, které jste vytvořili, najdete v části lístky přiřazen vám, zobrazit podrobnosti lístku a označit položky jako dokončit.

Toto je velmi jednoduché aplikace z hlediska funkce, ale uvidíte, jak ji od sestavit tak, aby možné škálovat na miliony uživatelů a bude odolné vůči třeba chyb databáze a ukončení připojení. Dozvíte se taky, jak vytvořit pracovní postup automatizované a agilní vývoj, která umožňuje spustit jednoduché a vytvoření lepší a lepší aplikace rychle a efektivně iterací cyklu vývoje.

<a id="waws"></a>
## <a name="web-apps-in-azure-app-service"></a>Webové aplikace v Azure App Service

Cloudové prostředí používá pro aplikaci opravte je služba Azure, kterému říkáme webů. Tato služba je způsob, jak můžete hostovat vlastní webové aplikace v Azure bez nutnosti vytvoření virtuálních počítačů a jejich aktualizaci, nainstalovat a nakonfigurovat službu IIS, atd. Jsme můžete umístit na našem virtuální počítače a automaticky získávat zálohování a obnovení a další služby. Služba Web Sites service funguje s ASP.NET, Node.js, PHP a Python. Umožňuje nasadit velmi rychle pomocí sady Visual Studio, Web Deploy, FTP, Git nebo TFS. Je obvykle na několik sekund mezi časem spuštění nasazení a čas, kdy aktualizace je dostupná přes Internet. Všechny zdarma začít pracovat, a můžete postupně škálovat podle rozšiřujícího se provozu.

Na pozadí webových aplikací ve službě Azure App Service poskytuje mnoho architektury komponenty a funkce, které budete muset vytvořit sami, pokud se chystáte hostovat webu pomocí služby IIS pro vlastní virtuální počítače. Jedna součást je koncový bod nasazení, který automaticky nakonfiguruje službu IIS a nainstaluje aplikaci na libovolný počet virtuálních počítačů, jak chcete spustit váš web.

![Službu pro nasazení](introduction/_static/image5.png)

Pokud se uživatel dotkne webu, není přímo dosáhl virtuálních počítačů služby IIS, přejde [aplikace směrování žádostí na](https://www.iis.net/downloads/microsoft/application-request-routing) nástroje pro vyrovnávání zatížení. Pomocí těchto se vlastní servery, ale tady výhoda spočívá v tom, že jste nastavili pro vás automaticky. Používají inteligentní heuristiky, která přebírá do účtu faktorech, například spřažení relace, hloubka fronty ve službě IIS, a využití procesoru na každém počítače přímé provoz na virtuální počítače tohoto hostitele webu.

![Nástroj pro vyrovnávání zatížení směrování žádostí na aplikace](introduction/_static/image6.png)

Pokud je počítač přestane fungovat, Azure automaticky vyžaduje od otáčení, otáčí si nové instance virtuálního počítače a spustí směrují přenosy na novou instanci – všechny bez dolů doby pro vaši aplikaci.

![Automatické obnovení po selhání počítače](introduction/_static/image7.png)

Všechny tyto probíhá automaticky. Všechny, které musíte udělat je vytvoření webu a nasazení aplikace, pomocí prostředí Windows PowerShell, sady Visual Studio nebo portál pro správu Azure.

Rychlé a snadné podrobný návod, jak vytvořit webovou aplikaci v sadě Visual Studio a nasadit ho na web Azure, najdete v části [Začínáme s Azure a ASP.NET](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

<a id="summary"></a>
## <a name="summary"></a>Souhrn

Tento úvod poskytl seznam témata, která se bude zabývat knihy, snímky obrazovky ukázkové aplikace a stručný přehled Web Apps ve službě Azure App Service cloudovém prostředí. Jeden z velkých výhod vývoj aplikací v a pro cloud je snadno automatizovat opakované vývojářské úlohy, jako je vytvoření testovacího prostředí a nasazení kódu do ní. Jak to provést který je předmětem [další kapitoly](automate-everything.md).

## <a name="resources"></a>Prostředky

Další informace o tématech, zahrnuté v této kapitole najdete v následujících zdrojích informací.

Dokumentace:

- [Webové aplikace v Azure App Service](https://azure.microsoft.com/services/app-service/web/). Stránky portálu Azure dokumentaci o webové aplikace.
- [Webové aplikace, cloudové služby a virtuální počítače: volba aplikace?](https://azure.microsoft.com/documentation/articles/choose-web-site-cloud-service-vm/) WAWS, jak je znázorněno v této kapitole je jen jednou ze tří způsobů, které webové aplikace můžete spustit v Azure. Tento článek vysvětluje rozdíly mezi třemi způsoby a poskytuje pokyny o tom, jak zvolit, které z nich je pro váš scénář. Cloudové služby jako webové servery, je funkce PaaS Azure. Virtuální počítače se o funkci IaaS. Další informace o PaaS versus IaaS, najdete v článku [možnosti dat](data-storage-options.md#paasiaas) kapitoly.

Videa:

- [Scott Guthrie spustí v kroku 0 - co je operační systém cloudu Azure?](https://azure.microsoft.com/documentation/videos/what-is-the-cloud-os-scottgu/)
- [Architektura weby - s Stefan Schackow](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/).
- [Interní informace o Azure webové stránky s Nir Mashkowski](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski).

> [!div class="step-by-step"]
> [Next](automate-everything.md)
