---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
title: Průběžná integrace a průběžné doručování (vytváření skutečných cloudových aplikací s Azure) | Dokumentace Microsoftu
author: MikeWasson
description: Vytváření reálného světa cloudových aplikací s Azure e kniha je založená na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které se dají mu...
ms.author: aspnetcontent
ms.date: 06/12/2014
ms.assetid: eaece9f5-f80c-428b-b771-5db66d275b7d
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
msc.type: authoredcontent
ms.openlocfilehash: 40687c490fdfb7764ac9ca8af6fffd054362d552
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819410"
---
<a name="continuous-integration-and-continuous-delivery-building-real-world-cloud-apps-with-azure"></a>Průběžná integrace a průběžné doručování (vytváření skutečných cloudových aplikací s Azure)
====================
podle [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Petr Dykstra](https://github.com/tdykstra)

[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronickou knihu](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Vytváření reálného světa cloudových aplikací s Azure** e knihy je založena na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které vám pomůžou být úspěšný vývoj webových aplikací v cloudu. Informace o e kniha najdete v tématu [první kapitoly](introduction.md).


První dva vhodné vývojové vzory procesu byly [automatizovat všechno](automate-everything.md) a [správy zdrojového kódu](source-control.md), a třetí model procesu sloučí. Kontinuální integrace (CI) znamená, že pokaždé, když vývojář vrátí kód do úložiště zdrojového kódu, sestavení se automaticky aktivuje. Průběžné doručování (CD) trvá o krok dál: Po úspěšném sestavení a automatizované testy jednotky automaticky nasadit aplikaci do prostředí, kde můžete provést další podrobnější testování.

Cloudu umožňuje minimalizovat náklady na údržbu testovací prostředí, protože platíte jenom za prostředky, prostředí za předpokladu, že je používáte. Váš proces průběžného Doručování můžete nastavit testovací prostředí, když je potřebujete, a prostředí může trvat až to budete mít testování.

## <a name="continuous-integration-and-continuous-delivery-workflow"></a>Pracovní postup průběžné integrace a průběžné doručování

Obecně doporučujeme, abyste udělali průběžné doručování do vývoje a přípravných prostředí. Většina týmů, dokonce i v Microsoftu, vyžadují ruční kontrolu a schválení proces pro produkční nasazení. Pro produkční nasazení budete chtít přesvědčit, že se stane, když jsou k dispozici informace o podpoře nebo během období s nízkým provozem klíčových osob na vývojový tým. Existuje ale, že nic a znemožnit vám tak, aby všechno, co vývojář musí provést změnu a prostředí vrátit se změnami zcela automatizace vývojová a testovací prostředí je nastavený pro akceptační testování.

Následující diagram z [Microsoft Patterns and Practices e knihy o průběžné doručování](http://aka.ms/ReleasePipeline) znázorňuje typický pracovní postup. Kliknutím na obrázek zobrazíte jeho plnou velikost v jeho původního kontextu.

[![Pracovní postup průběžného doručování](continuous-integration-and-continuous-delivery/_static/image1.png)](https://msdn.microsoft.com/library/dn449955.aspx)

## <a name="how-the-cloud-enables-cost-effective-ci-and-cd"></a>Jak cloud umožňuje nákladově efektivní CI a CD

Automatizace těchto procesů v Azure je snadné. Vzhledem k tomu, že všechno, co máte spuštěnou v cloudu, není nutné kupovat nebo spravovat servery pro sestavení nebo testovací prostředí. A není nutné čekat serveru tak, aby jej bylo možné provést testování na. S každým sestavením, které uděláte můžete uveďte do provozu testovacího prostředí v Azure pomocí skriptu pro automatizaci, spuštění akceptační testy nebo další podrobné testy proti ho a pak po dokončení právě dovolí ho. A pokud spustíte pouze tento server pro 2 hodiny nebo 8 hodin denně, je množství peněz, které budete muset dál za něj platit minimální, vzhledem k tomu, že Platíte pouze za čas, který ve skutečnosti běží na počítači. Například prostředí vyžaduje pro opravu že aplikace v podstatě nás stojí přibližně 1 cent za hodinu budete-li jednu úroveň z bezplatné úrovně. V průběhu měsíce, pokud jste ji spustili jenom prostředí za hodinu v době, testovací prostředí by pravděpodobně méně nákladný než provoz latte, které zakoupíte na Starbucks.

## <a name="visual-studio-team-services-vsts"></a>Visual Studio Team Services (VSTS)

VSTS poskytuje několik funkcí, které vám pomáhají s vývojem aplikací, od plánování až po nasazení.

- Podporuje Git (distribuovaný) a správy zdrojového kódu TFVC (centralizovaná).
- Nabízí služby elastické sestavování, což znamená, že ji dynamicky vytvoří buildovací servery, když je budete potřebovat a provede je mimo provoz, když budete mít. Když někdo kontroluje změny zdrojového kódu a není nutné mít přidělit a platit za vlastních serverů sestavení, které se nacházejí ve většině případů nečinnosti může sestavení automaticky aktivovat. Sestavovací služba je zdarma za předpokladu, abyste nepřekročili počet sestavení. Pokud plánujete udělat velký počet sestavení, můžete platit trochu více servery vyhrazené sestavení.
- Podporuje průběžné doručování do Azure.
- Podporuje automatické zátěžové testování. Zátěžové testování je zásadní pro cloudové aplikace, ale je často opominul, dokud je příliš pozdě. Zátěžový test simuluje hojně používají aplikaci, kterou tisíce uživatelů, která vám umožní najít kritické body a zlepšit propustnost – ještě před vydáním aplikace do produkčního prostředí.
- Podporuje spolupráce pomocí týmové místnosti, což usnadňuje komunikaci v reálném čase a spolupráci v malých týmů agile.
- Podporuje agilního řízení projektů.


Další informace o průběžnou integraci a doručování funkce VSTS najdete v tématu [Visual Studio Team Services](https://www.visualstudio.com/team-services/).

Pokud hledáte klíč řízení projektů, týmovou spolupráci a řešení řízení zdroje, prohlédněte si VSTS. Tato služba je zdarma až pro 5 uživatelů a si můžete zaregistrovat na [Visual Studio Team Services](https://www.visualstudio.com/team-services/).

## <a name="summary"></a>Souhrn

První tři cloudové vývojové vzory se o tom, jak implementovat proces opakovatelných, spolehlivá a předvídatelná vývoje s nízkou cyklu. V [další kapitolu](web-development-best-practices.md) začneme podívat se na architekturu a kódování.

## <a name="resources"></a>Prostředky

Další informace najdete v tématu [nasazení webové aplikace ve službě Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-deploy/).

Další informace najdete v článku na následujících odkazech:

- [Vytváření procesních toků pro verzi serveru Team Foundation Server 2012](http://aka.ms/ReleasePipeline). Elektronická kniha, praktických cvičení a ukázky kódu podle Microsoft Patterns and Practices, poskytuje podrobný Úvod do průběžné doručování. Popisuje použití sady Visual Studio Lab Management a správa vydaných verzí Visual Studio.
- [ALM Rangers DevOps nástrojů a pokynů](https://aka.ms/vsarsolutions/). ALM Rangers zavedené řešení doprovodné ukázkové aplikace DevOps Workbench a praktické pokyny ve spolupráci se tyto vzory se dají &amp; postupy knihy *sestavování vydávání s TFS 2012*, jako skvělý způsob, jak začít základními koncepty DevOps &amp; Release Management pro TFS 2012 a spustit pytli. Návod ukazuje, jak po sestavení a nasazení do různých prostředí.
- [Testování pro nepřetržité dodávky s Visual Studio 2012](https://msdn.microsoft.com/library/jj159345.aspx). E-kniha od Microsoft Patterns and Practices, vysvětluje, jak integrace, automatizovaného testování, průběžné doručování.
- [WindowsAzureDeploymentTracker](https://github.com/RyanTBerry/WindowsAzureDeploymentTracker). Zdrojový kód pro nástroje určené pro zachycení sestavení ze serveru TFS (podle popisku), sestavte ho, zabalíte ji, povolit někdo v roli DevOps konfigurace specifických aspektů a zápis do Azure. Nástroj sleduje proces nasazení Chcete-li povolit operace "vrácení zpět" pro už nasazenou verzi. Nástroj nemá žádné externí závislosti a může pracovat samostatný pomocí rozhraní API pro TFS a sady Azure SDK.
- [Průběžné nasazování: Spolehlivé softwaru uvolní prostřednictvím automatizace nasazení, testování a sestavení](https://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912/ref=sr_1_1?s=books&amp;ie=UTF8&amp;qid=1377126361). Kniha od Jez Humble.
- [Uvolnění! Návrh a nasazení softwaru připravené pro produkční prostředí](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213). Kniha zpopularizoval Michael T. Nygard.

> [!div class="step-by-step"]
> [Předchozí](source-control.md)
> [další](web-development-best-practices.md)
