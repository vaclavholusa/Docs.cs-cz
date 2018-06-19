---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
title: Průběžnou integraci a nastavené průběžné doručování (vytváření reálných cloudových aplikací s Azure) | Microsoft Docs
author: MikeWasson
description: Cloudové aplikace skutečné World sestavení s Azure elektronická kniha je založena na prezentace vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které můžete mu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: eaece9f5-f80c-428b-b771-5db66d275b7d
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
msc.type: authoredcontent
ms.openlocfilehash: 4d482aaa0d25d6e6baaf196df4b4bb9335408e46
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869410"
---
<a name="continuous-integration-and-continuous-delivery-building-real-world-cloud-apps-with-azure"></a>Průběžnou integraci a nastavené průběžné doručování (vytváření reálných cloudových aplikací s Azure)
====================
podle [Karel Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [tní Dykstra](https://github.com/tdykstra)

[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronická kniha](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Vytváření reálného světa cloudových aplikací s Azure** elektronická kniha je založena na prezentaci vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které vám mohou pomoci být úspěšná, vývoj webových aplikací pro cloud. Informace o elektronická kniha najdete v tématu [první kapitoly](introduction.md).


První dvě doporučená byly vývojové vzory proces [automatizovat všechno](automate-everything.md) a [správy zdrojového kódu](source-control.md), a třetí vzor proces kombinuje je. Průběžnou integraci (CI) znamená, že vždy, když vývojář zkontroluje v kódu na zdrojové úložiště, sestavení automaticky spuštěn. Nastavené průběžné doručování (CD) trvá tento krok jeden další: po sestavení a testů jednotek automatizované úspěšné, automaticky nasadíte aplikaci do prostředí, kde můžete provést další podrobné testování.

Cloudu můžete minimalizovat náklady na údržbu testovacím prostředí, protože platíte jenom pro prostředí prostředky tak dlouho, dokud je používáte. Váš proces CD můžete nastavit testovací prostředí, pokud to potřebujete, a prostředí můžete vypnout, když jste hotovi testování.

## <a name="continuous-integration-and-continuous-delivery-workflow"></a>Průběžnou integraci a nastavené průběžné doručování pracovního postupu

Obecně doporučujeme, abyste provedli nastavené průběžné doručování váš vývojový a přípravných prostředí. Většina týmy v Microsoftu, a to i vyžadují ruční kontrola a schválení proces pro produkční nasazení. Pro provozní nasazení můžete chtít Ujistěte se, zda se stane, když jsou k dispozici pro podporu nebo během období špičky klíčových osob vývojového týmu. Ale je, že žádná data k vám zabrání automatizaci úplně vývojová a testovací prostředí, tak, aby všechny, kterým je udělat změnu a prostředí, vrátit se změnami je nastavený pro testování přijetí.

Následující diagram z [Microsoft Patterns and Practices elektronická kniha o nastavené průběžné doručování](http://aka.ms/ReleasePipeline) znázorňuje obvyklý pracovní postup. Kliknutím na obrázek najdete ji v plné velikosti v jeho původní kontextu.

[![Pracovní postup nastavené průběžné doručování](continuous-integration-and-continuous-delivery/_static/image1.png)](https://msdn.microsoft.com/library/dn449955.aspx)

## <a name="how-the-cloud-enables-cost-effective-ci-and-cd"></a>Jak cloud umožňuje nákladově efektivní konfigurace a disku CD

Automatizace tyto procesy v Azure je snadné. Vzhledem k tomu, že používáte všechno, co v cloudu, nemusíte se koupit nebo spravovat servery pro vaše buildy nebo testovací prostředí. A nemusíte čekat na server jako dostupné pro testování v. Každé sestavení, které uděláte může číselníku testovací prostředí v Azure pomocí skriptu pro automatizaci, spusťte přijetí testů nebo další podrobné testů proti ho a pak při dokončení právě přerušit ho. A pokud spustíte pouze tento server pro 2 hodiny nebo 8 hodin za den, množství peněz, které budete muset platit pro něj je minimální, protože jste věnujeme jenom čas, kdy je počítač je ve skutečnosti spuštěna. Například prostředí vyžaduje pro opravu že aplikace v podstatě náklady na něj o 1 centů za hodinu Pokud přechod jedné vrstvy od bezplatná úroveň. V průběhu měsíce v případě pouze spustil prostředí na hodinu současně, vaše testovací prostředí by pravděpodobně nákladů menší než latte, který si zakoupíte v Starbucks.

## <a name="visual-studio-team-services-vsts"></a>Visual Studio Team Services (VSTS)

Služby VSTS poskytuje celou řadu funkcí a pomůže vám při vývoji aplikací z plánování nasazení.

- Podporuje Git (distribuované) a TFVC (centralizované) zdrojového kódu.
- Nabízí služby elastické sestavení, což znamená dynamicky vytvoří servery sestavení, když jste potřeba a přejdou dolů při dokončení akce. Můžete automaticky ji sestavení když někdo změnami změny zdrojového kódu a nemusíte mít přidělit a platit pro vlastní sestavení servery, které jsou ve většině případů nečinnosti. Služba sestavení je volné, dokud nepřekračují počet sestavení. Pokud budete chtít udělat k velkému počtu sestavení, můžete platíte trochu více servery vyhrazené sestavení.
- Podporuje nastavené průběžné doručování do Azure.
- Podporuje automatické zátěžové testování. Zátěžové testování je důležité pro cloudové aplikace, ale je často nevrátil, dokud je příliš pozdě. Zátěžové testování simuluje výrazně využívá aplikace podle tisíce uživatelů, což umožňuje najít kritická místa a zlepšit propustnost – před uvolněním aplikace do produkčního prostředí.
- Podporuje spolupráce pomocí týmové místnosti, což usnadňuje komunikaci v reálném čase a spolupráce pro malé agilní týmy.
- Podporuje správu agilní projektu.


Další informace o průběžnou integraci a doručení funkce služby VSTS najdete v tématu [Visual Studio Team Services](https://www.visualstudio.com/team-services/).

Pokud hledáte klíč projektu správy Týmová spolupráce a řešením pro řízení zdrojového, podívejte se na služby VSTS. Služba je zdarma až 5 uživatelů a můžete si zaregistrovat pro něj v [Visual Studio Team Services](https://www.visualstudio.com/team-services/).

## <a name="summary"></a>Souhrn

První tři cloudu vývojové vzory se o tom, jak implementovat proces opakovatelných, spolehlivou a předvídatelný vývoj s nízkou interval. V [další kapitoly](web-development-best-practices.md) začneme podívat se na kódování a architektuře vzory.

## <a name="resources"></a>Prostředky

Další informace najdete v tématu [nasazení webové aplikace v Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-deploy/).

Další informace v následujících zdrojích informací:

- [Vytvoření kanálu verze sadou Team Foundation Server 2012](http://aka.ms/ReleasePipeline). Elektronická kniha, praktických labs a ukázkový kód podle Microsoft Patterns and Practices, poskytuje podrobný Úvod do nastavené průběžné doručování. Zahrnuje použití Visual Studio – správa testovacího prostředí a Visual Studio – správa verzí.
- [DevOps Rangers ALM nástrojů a pokyny](https://aka.ms/vsarsolutions/). ALM Rangers zavedená DevOps Workbench ukázkové doprovodné řešení a praktické informace ve spolupráci se vzory &amp; postupy kniha *vytváření kanálu verze sadu TFS 2012*, jako skvělý způsob, jak začít učení koncepty DevOps &amp; správy verzí sady TFS 2012 a chcete vykázat a otestovat. Pokyny ukazuje, jak vytvářet jednou a nasazení do několika prostředí.
- [Testování pro nastavené průběžné doručování sadou Visual Studio 2012](https://msdn.microsoft.com/library/jj159345.aspx). Elektronická kniha podle Microsoft Patterns and Practices, vysvětluje, jak integrovat automatizované testování s nastavené průběžné doručování.
- [WindowsAzureDeploymentTracker](https://github.com/RyanTBerry/WindowsAzureDeploymentTracker). Zdrojový kód pro nástroj určený k zaznamenání sestavení ze sady TFS (podle štítek), sestavte jej, balíček je, někdo povolit v roli DevOps ke konfiguraci konkrétních aspektů a poslat ho do Azure. Nástroj sleduje proces nasazení k povolení operací "vrácení" dříve nasazené verzi. Tento nástroj nemá žádné externí závislosti a může pracovat samostatné pomocí rozhraní API sady TFS a sady Azure SDK.
- [Nastavené průběžné doručování: Spolehlivé softwaru uvolní prostřednictvím sestavení, testování a nasazení automatizace](https://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912/ref=sr_1_1?s=books&amp;ie=UTF8&amp;qid=1377126361). Seznamu pomocí Jez Humble.
- [Verze ji! Navrhování a nasazení softwaru produkční prostředí](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213). Seznamu pomocí Michael T. Nygard.

> [!div class="step-by-step"]
> [Předchozí](source-control.md)
> [další](web-development-best-practices.md)
