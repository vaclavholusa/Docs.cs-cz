---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
title: "Výběr správný přístup k nasazení webu | Microsoft Docs"
author: jrjlee
description: "Při práci s Internetové informační služby (IIS) nástroj pro nasazení webu (Web Deploy) 2.0 nebo novější, existují tři hlavní přístupy, které můžete použít k získání..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 787a53fd-9901-4a11-9d58-61e0509cda45
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: b77aa37160f3822f58908866e44497aea3d3bdc8
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
<a name="choosing-the-right-approach-to-web-deployment"></a>Výběr správný přístup k nasazení webu
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Při práci s Internetové informační služby (IIS) nástroj pro nasazení webu (Web Deploy) 2.0 nebo novější, existují tři hlavní přístupy, které můžete použít k získání zabalené webových aplikací na webovém serveru. Máte tyto možnosti:
> 
> - Nasaďte aplikaci ze vzdáleného umístění cílením *Služba agenta pro nasazení webu* (také označované jako "vzdáleného agenta") na cílovém serveru.
> - Nasaďte aplikaci ze vzdáleného umístění pomocí nasazení webu na vyžádání (také označované jako "temp agenta").
> - Nasaďte aplikaci ze vzdáleného umístění cílením *rutiny nasazení webové služby IIS* na cílovém serveru.
> - Nasaďte aplikaci s ruční kopírování webového balíčku na cílový server a jeho import pomocí Správce služby IIS.
> 
> Jak nakonfigurovat cílový webových serverů bude záviset na jaký přístup do nasazení, kterou chcete použít. Toto téma vám pomůže rozhodnout, jaký přístup k nasazení je pro vás nejvhodnější.


Tato tabulka ukazuje hlavní výhody a nevýhody jednotlivých přístupů nasazení, společně s scénáře, které obvykle podle jednotlivých přístupů.

| Přístup | Výhody | Nevýhody | Typické scénáře |
| --- | --- | --- | --- |
| Vzdáleného agenta | Je snadné vytvořit. Je vhodné pro pravidelné aktualizace pro webové aplikace a obsah. | Uživatel musí být správce na cílovém serveru. Uživatele nelze zadat alternativní přihlašovací údaje. | Vývojové prostředí. Testovací prostředí. |
| Dočasné agenta | Není nutné k instalaci nástroje nasazení webu na cílovém počítači. Automaticky se používá nejnovější verzi nástroje nasazení webu. | Uživatel musí být správce na cílovém serveru. Uživatele nelze zadat alternativní přihlašovací údaje. | Vývojové prostředí. Testovací prostředí. |
| Obslužné rutiny pro nasazení webu | Uživatelé bez oprávnění správce, můžete nasadit obsah. Je vhodné pro pravidelné aktualizace pro webové aplikace a obsah. | Je mnohem složitější nastavit. | Přípravná prostředí. Provozní prostředí intranetu. Hostované prostředí. |
| Offline nasazení | Je velmi snadné nastavit. Je vhodná pro izolované prostředí. | Správce serveru musíte ručně zkopírovat a importovat pokaždé, když webového balíčku. | Internetový provozní prostředí. Izolované prostředí sítě. |
  

## <a name="using-the-remote-agent"></a>Použití vzdáleného agenta

Když instalujete nasazení webu pomocí výchozích nastavení na cílovém serveru, je automaticky nainstalována a spuštěna Služba agenta pro nasazení webu ("vzdáleného agenta"). Ve výchozím nastavení vzdáleného agenta zpřístupní koncový bod HTTP na této adrese:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample1.cmd)]


> [!NOTE]
> Můžete nahradit [*server*] s názvem počítače webového serveru, IP adresu pro váš webový server nebo název hostitele, přeloží na vašem webovém serveru.


Správci serveru můžete nasadit webové balíčky ze vzdáleného umístění, jako vývojáře počítač nebo server sestavení tak, že zadáte tuto adresu koncového bodu. Předpokládejme například, že Matt Hink společnosti Fabrikam, Inc. má integrovaný ContactManager.Mvc projekt webové aplikace na svém počítači vývojáře. Proces sestavení generuje webový balíček, společně s *. deploy.cmd* soubor, který obsahuje nástroje nasazení webu příkazy potřebné k instalaci balíčku. Pokud Matt správce serveru na serveru TESTWEB1, mohl nasazovat webové aplikace na webovém serveru testovací spuštěním tohle příkazu na svém počítači developer:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample2.cmd)]


Ve skutečnosti můžete Web Deploy spustitelný soubor odvození adresa koncového bodu vzdáleného agenta, pokud zadáte název počítače, takže Matt musí pouze na tento typ:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample3.cmd)]


> [!NOTE]
> Další informace o nasazení webu syntaxe příkazového řádku a *. deploy.cmd* soubory, najdete v části [postupy: instalace nasazení balíčku pomocí souboru deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx).


Vzdáleného agenta nabízí snadný způsob, jak nasadit obsah ze vzdáleného umístění, a tento přístup může fungovat i s jedním kliknutím nebo automatické nasazení. Uživatel, který spouští příkaz nasazení však také musí být správce domény nebo členem místní skupiny administrators na cílovém serveru. Kromě toho vzdáleného agenta nepodporuje základní ověřování, takže nemůžete předat alternativní přihlašovací údaje na příkazovém řádku.

Poskytuje užitečné přístup k nasazení v vývoj nebo testovací scénáře, kde je běžné pro vývojáře správce s úplnými oprávněními kontrolu nad prostředí testovací server, a aplikace jsou obvykle znovu sestavit a znovu nasadit velmi vzdáleného agenta často. Tento přístup je však obvykle méně přijatelné pro pracovním nebo produkčním prostředí.

Příklad začátku do konce scénáře, který používá přístup vzdáleného agenta naleznete v části [scénář: konfigurace testovacího prostředí pro nasazení webu](scenario-configuring-a-test-environment-for-web-deployment.md).

## <a name="using-the-temp-agent"></a>Pomocí dočasného agenta

Dočasné agent přístup k nasazení je podobná přístup vzdáleného agenta. Na rozdíl od agent vzdáleného přístupu, nepotřebujete však nainstaluje Web Deploy na cílový webový server. Místo toho když provedete nasazení, nasazení webu nainstaluje dočasné verzi agenta služba pro nasazení webu na cílovém serveru a bude používat toto nasazení obsahu do služby IIS. Po dokončení nasazení se odeberou všechny dočasné soubory.

Pokud chcete použít nastavení poskytovatele dočasného agenta, přidejte **/g** příznak do příkazu nasazení:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample4.cmd)]


> [!NOTE]
> Dočasné agenta nelze použít pokud služba agenta pro nasazení webu je nainstalován v cílovém počítači, i v případě, že služba není spuštěná.


Výhodou tohoto přístupu je, že nemusíte udržovat instalace nasazení webu na cílových serverech. Kromě toho není nutné zajistit, že zdrojový a cílový počítač používají stejnou verzi nástroje nasazení webu. Tento přístup, ale vykazuje hlavní stejná omezení jako přístup vzdáleného agenta, konkrétně musí být místní správce na cílovém serveru za účelem nasazení obsahu, a je podporována pouze ověřování NTLM. Postup dočasného agenta taky vyžaduje mnohem víc počáteční konfigurace cílového prostředí.

Další informace o použití dočasného agenta, najdete v části [postupy: instalace nasazení balíčku pomocí souboru deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx) a [nasazení webu na vyžádání](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

## <a name="using-the-web-deploy-handler"></a>Pomocí webu nasadit obslužné rutiny

Pro službu IIS 7 a vyšší Web Deploy nabízí alternativní nasazení přístup prostřednictvím rutiny nasazení webové služby IIS. Obslužné rutiny nasazení webu je úzce integrovaná spolu se službou IIS webové správy (WMSvc), která je navržená tak, aby uživatelé mohli spravovat weby služby IIS ze vzdálených umístění.

Ve výchozím nastavení vzdáleného agenta zpřístupní koncový bod HTTP na této adrese:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample5.cmd)]


> [!NOTE]
> Můžete nahradit [*server*] s názvem počítače webového serveru, IP adresu pro váš webový server nebo název hostitele, přeloží na vašem webovém serveru.


Big výhodou obslužné rutiny nasazení webu prostřednictvím vzdáleného agenta a dočasného agentem, je, můžete nakonfigurovat službu IIS umožníte uživatelům bez oprávnění správce nasadit aplikace a obsah pro určité weby služby IIS. Obslužné rutiny nasazení webu také podporuje základní ověřování, takže můžete zadat alternativní přihlašovací údaje jako parametry v příkazech nasazení webu. Hlavní nevýhodou je původně mnohem složitější nastavení a konfigurace obslužné rutiny nasazení webu.

V případě uživatelé bez oprávnění správce služba webové správy (WMSvc) se pouze povolí uživateli k připojení do služby IIS pomocí připojení úrovni webu, nikoli úrovni serveru připojení. Pro přístup k určité lokalitě, můžete zahrnout řetězec dotazu pro danou lokalitu adresa koncového bodu:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample6.cmd)]


Předpokládejme například, že procesu sestavení nastaven tak, aby automaticky nasadit webovou aplikaci pro pracovní prostředí po každé úspěšné sestavení. Pokud jste použili přístup vzdáleného agenta, potřebovali byste aby identitě procesu sestavení správce na cílovém serveru. Naproti tomu přístup obslužné rutiny nasazení webu můžete udělit uživatel není správcem & #x 2014; **FABRIKAM\stagingdeployer** tento případ & #x 2014; oprávnění pouze konkrétní web služby IIS a procesu sestavení může poskytnout tyto přihlašovací údaje pro nasazení webového balíčku.


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample7.cmd)]


> [!NOTE]
> Další informace o syntaxi a nasazení webu operací příkazového řádku najdete v tématu [příkazového řádku pro nasazení webového odkazu](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Další informace o používání *. deploy.cmd* souborů najdete v tématu [postupy: instalace nasazení balíčku pomocí souboru deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx).


Obslužné rutiny nasazení webu poskytuje užitečné přístup k nasazení v pracovním prostředí, hostované prostředí a na základě intranetu provozní prostředí, kde vzdálený přístup k serveru je k dispozici, ale nejsou přihlašovací údaje správce.

Příklad začátku do konce scénáře, který používá přístup obslužné rutiny nasazení webu naleznete v části [scénář: Konfigurace pracovní prostředí pro nasazení webu](scenario-configuring-a-staging-environment-for-web-deployment.md).

## <a name="using-offline-deployment"></a>Pomocí Offline nasazení

V některých případech není možné nebo praktické k nasazení aplikace a obsah na webu IIS ze vzdáleného umístění. Například zdrojový a cílový počítač může být v izolovaných sítích nebo síťové segmenty nebo zásady brány firewall může povolit vzdálený přístup.

V takovýchto scénářů stále můžete balení a publikování možnosti nasazení webu; právě nemůžete použít je ze vzdáleného umístění. Místo toho musí správce na cílovém serveru zkopírujte webového balíčku na server a ho importovat pomocí Správce služby IIS.

![](choosing-the-right-approach-to-web-deployment/_static/image1.png)

Přístup v režimu offline nasazení je obvykle užitečný v produkčním prostředí internetového, kde servery v hraniční síti může mít omezený připojení k počítačům v interní síti.

Příklad začátku do konce scénáře, který používá přístup offline nasazení naleznete v části [scénář: Konfigurace produkčním prostředí pro nasazení webu](scenario-configuring-a-production-environment-for-web-deployment.md).

## <a name="further-reading"></a>Další čtení

Další informace o syntaxi a nasazení webu operací příkazového řádku najdete v tématu [příkazového řádku pro nasazení webového odkazu](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Další informace o používání *. deploy.cmd* souborů najdete v tématu [postupy: instalace nasazení balíčku pomocí souboru deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx).

Další obecné informace o různých způsobech, ve kterém bude možné nasadit balíčky web ze vzdáleného počítače, najdete v části [vzdálené použití nasazení webu](https://technet.microsoft.com/library/ee461175(WS.10).aspx). Další informace o použití nasazení webu na vyžádání najdete v tématu [nasazení webu na vyžádání](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

>[!div class="step-by-step"]
[Předchozí](configuring-server-environments-for-web-deployment.md)
[další](scenario-configuring-a-test-environment-for-web-deployment.md)
