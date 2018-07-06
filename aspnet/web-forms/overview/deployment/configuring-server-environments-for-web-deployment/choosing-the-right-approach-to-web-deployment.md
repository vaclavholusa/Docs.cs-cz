---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
title: Výběr správného přístupu k nasazení webu | Dokumentace Microsoftu
author: jrjlee
description: Při práci s Internetové informační služby (IIS) nástroj pro nasazení webu (nasazení webu) 2.0 nebo novější, existují tři hlavní přístupy, které vám umožní získat...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 787a53fd-9901-4a11-9d58-61e0509cda45
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: eb1b7d50e5d7461d760ad7a963cc70369b7a4513
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807048"
---
<a name="choosing-the-right-approach-to-web-deployment"></a>Výběr správného přístupu k nasazení webu
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Při práci s Internetové informační služby (IIS) nástroj pro nasazení webu (nasazení webu) 2.0 nebo novější, existují tři hlavní přístupy, které vám umožní získat zabalené webové aplikace na webovém serveru. Máte tyto možnosti:
> 
> - Nasadit aplikaci ze vzdáleného umístění pomocí cílení *webová služba agenta nasazení* (označované také jako "vzdáleného agenta") na cílovém serveru.
> - Nasazení aplikace ze vzdáleného umístění pomocí nasazení webu na vyžádání (označované také jako "temp agenta").
> - Nasadit aplikaci ze vzdáleného umístění pomocí cílení *obslužné rutiny služby IIS webu nasadit* na cílovém serveru.
> - Nasaďte aplikaci ručně zkopírováním webový balíček na cílový server a jeho import pomocí Správce služby IIS.
> 
> Konfigurace webových serverů cíl bude záviset na jaký přístup k nasazení, který chcete použít. Toto téma vám pomůže rozhodnout, jaký přístup k nasazení je pro vás nejvhodnější.


Tato tabulka zobrazuje hlavní výhody a nevýhody obou těchto nasazení přístupů, společně s scénáře, které nejvíce obvykle vyhovují jednotlivým přístupům.

| Přístup | Android – systém cesta vrácená procedurou  je přijatelné umístění pro uložení souboru databáze. | Universal Windows Platform – používá  rozhraní API. | Typické scénáře |
| --- | --- | --- | --- |
| Vzdálený Agent | Je snadné nastavení. Je vhodný pro pravidelné aktualizace webové aplikace a obsah. | Uživatel musí být správce na cílovém serveru. uživatele nejde zadat alternativní přihlašovací údaje. | Vývojových prostředích. Testovací prostředí. |
| Dočasné agenta | Není nutné nainstalovat nasazení webu v cílovém počítači. Automaticky se používá nejnovější verzi nástroje nasazení webu. | Uživatel musí být správce na cílovém serveru. Uživatele nejde zadat alternativní přihlašovací údaje. | Vývojových prostředích. Testovací prostředí. |
| Obslužná rutina nasazení webu | Uživatelé bez oprávnění správce můžou nasazovat obsah. Je vhodný pro pravidelné aktualizace webové aplikace a obsah. | Je mnohem složitější nastavení. | Pracovní prostředí. Produkční prostředí intranetu. Hostované prostředí. |
| Offline nasazení | Je velmi snadné nastavení. Je vhodná pro izolované prostředí. | Správce serveru musíte ručně zkopírovat a importovat pokaždé, když webového balíčku. | Přístupem k Internetu produkční prostředí. Izolované prostředí sítě. |
  

## <a name="using-the-remote-agent"></a>Pomocí vzdáleného agenta

Když instalujete nasazení webu pomocí výchozích nastavení na cílovém serveru, je automaticky nainstalována a spuštěna Služba agenta nasazení webu ("vzdáleného agenta"). Ve výchozím nastavení vzdálený agent zpřístupňuje koncový bod HTTP na této adrese:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample1.cmd)]


> [!NOTE]
> Můžete nahradit [*server*] s názvem počítače webového serveru, IP adresu pro váš webový server nebo název hostitele, který překládá na vašem webovém serveru.


Správci serveru můžete nasadit webové balíčky ze vzdáleného umístění, jako je počítač vývojář nebo serveru sestavení, tak, že zadáte tuto adresu koncového bodu. Předpokládejme například, že Matt Hink společnosti Fabrikam, Inc. vytvořený projekt ContactManager.Mvc webové aplikace na svém vývojovém počítači. Proces sestavení generuje webový balíček, spolu s *. deploy.cmd* soubor, který obsahuje příkazy Webdeploy požadovaná k instalaci balíčku. Pokud Matt je správce serveru na serveru TESTWEB1, si můžete nasadit webové aplikace na webovém serveru test spuštěním následujícího příkazu na svém vývojovém počítači:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample2.cmd)]


Ve skutečnosti může spustitelný soubor pro nasazení webu odvodit adresu koncového bodu vzdáleného agenta, pokud zadáte název počítače, takže Matt pouze na tento typ musí:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample3.cmd)]


> [!NOTE]
> Další informace o nasazení webu syntaxe příkazového řádku a *. deploy.cmd* soubory, naleznete v tématu [postupy: instalace nasazení balíčku pomocí souboru deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx).


Vzdálený agent nabízí jednoduchý způsob, jak nasadit obsah ze vzdáleného umístění, a tento přístup můžete pracovat s jedním kliknutím nebo automatizované nasazení. Uživatel, který spouští příkaz nasazení však také musí být správce domény nebo členem místní skupiny administrators na cílovém serveru. Kromě toho vzdálený agent nepodporuje základní ověřování, takže nelze předat alternativní přihlašovací údaje na příkazovém řádku.

Vzdálený agent poskytuje užitečné přístup k nasazení vývojové nebo testovací scénáře, kde není nic neobvyklého pro vývojáře, správce s úplnými oprávněními kontrolu nad testovacího prostředí serveru a aplikace jsou obvykle znovu sestavit a znovu nasadil velmi často. Tento přístup je však obvykle méně přijatelné pro pracovní nebo produkční prostředí.

Příklad začátku do konce scénář, který používá vzdálený agent přístup, najdete v části [scénář: konfigurace testovacího prostředí pro nasazení webu](scenario-configuring-a-test-environment-for-web-deployment.md).

## <a name="using-the-temp-agent"></a>Pomocí dočasného agenta

Dočasné agent přístup k nasazení je podobný přístup vzdáleného agenta. Ale na rozdíl od agenta vzdáleného přístupu, nemusíte na cílový webový server nainstalujte nástroj nasazení webu. Místo toho když provádíte nasazení, Web Deploy nainstaluje dočasné verzi webová služba agenta nasazení na cílovém serveru a použije to nasazení obsahu do služby IIS. Po dokončení nasazení se odeberou všechny dočasné soubory.

Pokud chcete použít nastavení poskytovatele temp agenta, přidejte **/g** příznak, který váš příkaz nasazení:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample4.cmd)]


> [!NOTE]
> Pokud nelze použít dočasný agenta v cílovém počítači, je instalována webová služba agenta nasazení i v případě, že služba není spuštěná.


Výhodou tohoto přístupu je, že není nutné udržovat instalace rozšíření nasazení webu na cílových serverech. Kromě toho není nutné zajistit, že zdrojový a cílový počítač používají stejnou verzi nástroje nasazení webu. Tento přístup však vykazuje stejná omezení instančního objektu jako vzdálený agent přístup, a to, že pokud chcete nasadit obsah musí být místní správce na cílovém serveru a je podporován pouze ověřování NTLM. Přístup temp agent také vyžaduje mnohem více počáteční konfiguraci cílového prostředí.

Další informace o použití dočasného agenta, najdete v části [postupy: instalace nasazení balíčku pomocí souboru deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx) a [nasazení webu na vyžádání](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

## <a name="using-the-web-deploy-handler"></a>Pomocí webu nasadit obslužné rutiny

Pro službu IIS 7 a vyšší Web Deploy nabízí alternativní nasazení přístup prostřednictvím rutiny nasazení webové služby IIS. Obslužné rutiny nasazení webu je úzce integrována se služby IIS služby webové správy (WMSvc), která je navržena k umožnění uživatelům spravovat weby služby IIS ze vzdálených umístění.

Ve výchozím nastavení vzdálený agent zpřístupňuje koncový bod HTTP na této adrese:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample5.cmd)]


> [!NOTE]
> Můžete nahradit [*server*] s názvem počítače webového serveru, IP adresu pro váš webový server nebo název hostitele, který překládá na vašem webovém serveru.


Velkou výhodou obslužné rutiny nasazení webu přes vzdálený agent a temp agenta, je můžete konfigurovat služby IIS umožňuje uživatelům bez oprávnění správce k nasazení aplikace a obsah pro určité weby služby IIS. Obslužné rutiny nasazení webu také podporuje základní ověřování, takže můžete zadat alternativní přihlašovací údaje jako parametry v příkazům nasazení webu. Hlavní nevýhodou je, že obslužné rutiny nasazení webu je zpočátku mnohem složitější nastavení a konfigurace.

V případě uživatelů bez oprávnění správce služby webové správy (WMSvc) pouze umožní uživateli k připojení do služby IIS pomocí připojení úrovni webu, nikoli připojení úrovni serveru. Pro přístup k určité lokalitě, můžete zahrnout řetězec dotazu jednotlivých lokalit na adresu koncového bodu:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample6.cmd)]


Předpokládejme například, že procesu sestavení je nakonfigurovaný pro automatické nasazení do přípravného prostředí po každém úspěšném sestavení webové aplikace. Pokud jste použili postup vzdáleného agenta, je třeba vytvořit identitu procesu sestavení správce na cílovém serveru. Naproti tomu obslužné rutiny webu nasadit přístup můžete poskytnout uživatele bez oprávnění správce&#x2014;**FABRIKAM\stagingdeployer** v tomto případě&#x2014;oprávnění pouze konkrétní web služby IIS a proces sestavení může poskytnout tyto přihlašovací údaje pro nasazení webového balíčku.


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample7.cmd)]


> [!NOTE]
> Další informace o syntaxi a nasazení webu operace příkazového řádku najdete v tématu [webové nasazení Command Line Reference](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Další informace o používání *. deploy.cmd* souborů naleznete v tématu [postupy: instalace nasazení balíčku pomocí souboru deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx).


Obslužné rutiny nasazení webu poskytuje užitečné přístup k nasazení v pracovním prostředí, hostované prostředí a intranetoví produkční prostředí, kde je k dispozici vzdálený přístup k serveru, ale nejsou přihlašovací údaje správce.

Příklad začátku do konce scénář, který používá metodu obslužné rutiny nasazení webu, naleznete v tématu [scénář: Konfigurace přípravného prostředí pro nasazení webu](scenario-configuring-a-staging-environment-for-web-deployment.md).

## <a name="using-offline-deployment"></a>Pomocí Offline nasazení

V některých případech to není možné nebo praktické pro nasazení aplikace a obsah na webu služby IIS ze vzdáleného umístění. Například může být zdrojový a cílový počítač v izolované sítě nebo segmentů sítě nebo zásady brány firewall může povolit vzdálený přístup.

V takových případech stále můžete balení a publikování funkce rozšíření nasazení webu; právě nelze je použít ze vzdáleného umístění. Místo toho musí správce na cílovém serveru zkopírujte webový balíček na server a importovat pomocí Správce služby IIS.

![](choosing-the-right-approach-to-web-deployment/_static/image1.png)

Přístup offline nasazení je obvykle vhodné v produkčních prostředích přístupem k Internetu, kde serverů v hraniční síti můžou mít omezenou připojení k počítačům v interní síti.

Začátku do konce příklad scénáře, které využívá přístup offline nasazení najdete v tématu [scénář: Konfigurace provozního prostředí pro nasazení webu](scenario-configuring-a-production-environment-for-web-deployment.md).

## <a name="further-reading"></a>Další čtení

Další informace o syntaxi a nasazení webu operace příkazového řádku najdete v tématu [webové nasazení Command Line Reference](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Další informace o používání *. deploy.cmd* souborů naleznete v tématu [postupy: instalace nasazení balíčku pomocí souboru deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx).

Další obecné informace o různých způsobech, ve kterém můžete nasazovat webových balíčků ze vzdáleného počítače najdete v tématu [pomocí webové nasazení vzdáleně](https://technet.microsoft.com/library/ee461175(WS.10).aspx). Další informace o použití nasazení webu na vyžádání najdete v tématu [nasazení webu na vyžádání](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

> [!div class="step-by-step"]
> [Předchozí](configuring-server-environments-for-web-deployment.md)
> [další](scenario-configuring-a-test-environment-for-web-deployment.md)
