---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: Co provádění, pokud nasazení | Dokumentace Microsoftu
author: jrjlee
description: Toto téma popisuje, jak provést "Co když" (nebo simulovaných) pomocí nástroje nasazení webu Internetové informační služby (IIS) (nasazení webu) a V nasazení...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: 054b15e1e58164ec7e85c77c39fb47bcb47879b9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753223"
---
<a name="performing-a-what-if-deployment"></a>Provádění nasazení "What If"
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje, jak provést "what if" (nebo simulovaných) pomocí nástroje nasazení webu Internetové informační služby (IIS) (nasazení webu) a VSDBCMD nasazení. To umožňuje zjistit dopady logiky nasazení na konkrétní cílové prostředí před při skutečném nasazení vaší aplikace.


Toto téma je součástí série kurzů podle požadavků na nasazení enterprise fiktivní společnosti s názvem společnosti Fabrikam, Inc. V této sérii kurzů používá ukázkové řešení&#x2014; [řešení Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s realistické úroveň složitosti, včetně aplikace ASP.NET MVC 3, komunikace Windows Služba Foundation (WCF) a databázový projekt.

Metody nasazení v srdci těchto kurzů je založen na rozdělení přístupu soubor projektu je popsáno v [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve kterém je řízena procesem sestavení a nasazení dva soubory projektu&#x2014;jeden obsahuje pokyny pro sestavení, které platí pro všechny cílové prostředí a jeden obsahuje nastavení pro konkrétní prostředí sestavení a nasazení. V okamžiku sestavení souboru projektu specifických pro prostředí se sloučí do souboru projektu bez ohledu na prostředí a vytvoří kompletní sadu pokynů sestavení.

## <a name="performing-a-what-if-deployment-for-web-packages"></a>Probíhá nasazení "What If" webových balíčků

Nástroj nasazení webu obsahuje funkce, které umožňuje provádět nasazení v "what if" (nebo zkušební verze) režimu. Při nasazování artefaktů v režimu "what if" Webdeploy vygeneruje soubor protokolu, jako kdyby jste provedli nasazení, ale nic se nezmění ve skutečnosti na cílovém serveru. Kontrola souboru protokolu vám můžou pomoct pochopit, jaký vliv budou mít vaše nasazení na cílovém serveru, zejména:

- Co se přidají.
- Co bude aktualizován.
- Co bude odstraněn.

Protože nasazení "what if" ve skutečnosti nic nezmění na cílovém serveru, se nemůže vždy proveďte je Předvídejte, jestli nasazení proběhne úspěšně.

Jak je popsáno v [nasazení webových balíčků](../web-deployment-in-the-enterprise/deploying-web-packages.md), můžete nasadit pomocí nasazení webu dvěma způsoby webových balíčků&#x2014;pomocí nástroje pro příkazový řádek MSDeploy.exe přímo nebo spuštěním *. deploy.cmd* souboru který generuje procesu sestavení.

Pokud používáte MSDeploy.exe přímo, můžete spustit nasazení "what if" tak, že přidáte **– whatif** příznak do svých rukou. Například k vyhodnocení, co by mohlo dojít, pokud jste nasadili ContactManager.Mvc.zip balíček do přípravného prostředí, příkaz MSDeploy by měl vypadat takto:


[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]


Jakmile budete spokojeni s výsledky nasazení "what if", můžete odebrat **– whatif** příznak pro spuštění nasazení za provozu.

> [!NOTE]
> Další informace o možnostech příkazového řádku pro MSDeploy.exe najdete v tématu [nastavení operace nasazení webu](https://technet.microsoft.com/library/dd569089(WS.10).aspx).


Pokud používáte *. deploy.cmd* soubor, můžete spustit nasazení "what if" zahrnutím **/t** příznak příznak (zkušební režim) místo **/y** příznak ("Ano", nebo režim aktualizace) v příkaz. Například chcete vyhodnotit, co by mohlo dojít, pokud jste nasadili balíček ContactManager.Mvc.zip spuštěním *. deploy.cmd* souboru příkazu by měl vypadat takto:


[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]


Jakmile budete spokojeni s výsledky nasazení "zkušební režim", můžete nahradit **/t** s příznakem **/y** příznak pro spuštění nasazení za provozu:


[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]


> [!NOTE]
> Další informace o možnostech příkazového řádku pro *. deploy.cmd* soubory, naleznete v tématu [postupy: instalace nasazení balíčku pomocí souboru deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx). Pokud spustíte *. deploy.cmd* souboru bez zadání jakékoli příznaky příkazového řádku se zobrazí seznam dostupných příznaků.


## <a name="performing-a-what-if-deployment-for-databases"></a>Probíhá nasazení "What If" pro databáze

V této části se předpokládá, že používáte nástroj VSDBCMD provádět nasazení přírůstkové, založené na schématu databáze. Tento přístup je popsána podrobněji [nasazení databázové projekty](../web-deployment-in-the-enterprise/deploying-database-projects.md). Doporučujeme, aby měli seznámit s tímto tématem před použitím koncepty popsané tady.

Při použití VSDBCMD v **nasadit** režimu, můžete použít **/dd** (nebo **/DeployToDatabase**) příznak, který ovládací prvek, zda VSDBCMD provede samotné nasazení databáze nebo právě generuje skript nasazení. Pokud nasazujete .dbschema souboru, toto chování je:

- Pokud zadáte **/dd+** nebo **/dd**, VSDBCMD vygenerovat skript nasazení a nasazení databáze.
- Pokud zadáte **/dd-** nebo vynechejte přepínač, VSDBCMD vygeneruje jenom skript nasazení.

> [!NOTE]
> Pokud nasazujete .deploymanifest souboru spíše než soubor .dbschema, chování **/dd** přepínač je mnohem složitější. V podstatě VSDBCMD bude ignorovat hodnoty **/dd** Pokud .deploymanifest soubor obsahuje, přejděte **DeployToDatabase** element s hodnotou **True**. [Nasazení projektu databáze](../web-deployment-in-the-enterprise/deploying-database-projects.md) popisuje toto chování v plném rozsahu.


Například chcete vygenerovat skript pro nasazení **ContactManager** databáze bez skutečného nasazení databáze VSDBCMD příkazu by měl vypadat takto:


[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]


VSDBCMD je nástroj pro nasazení rozdílovými a jako takový nasazovací skript generuje dynamicky tak, aby obsahovala všechny příkazy SQL potřebnými k aktualizaci aktuální databázi, pokud existuje, zadané schéma. Kontrola skriptu nasazení je užitečný způsob, jak určit, jaký vliv má vaše nasazení bude mít v aktuální databázi a data, která ho obsahuje. Můžete například chtít určit:

- Určuje, zda se odeberou veškeré stávající tabulky a určuje, zda povede ke ztrátě dat.
- Určuje, zda pořadí operací s sebou nese riziko ztráty dat, například pokud rozdělení nebo sloučení tabulky.

Pokud jste spokojení s skript nasazení, můžete opakovat VSDBCMD s **/dd+** příznak provést změny. Případně můžete upravit skript nasazení podle svých požadavků a pak ji spustit ručně na databázovém serveru.

## <a name="integrating-what-if-functionality-into-custom-project-files"></a>Integrace "What If" funkce do vlastní projektových souborů

Ve složitějších scénářích nasazení, budete chtít použít vlastní soubor projektu Microsoft Build Engine (MSBuild) k zapouzdření svoji logiku sestavení a nasazení, jak je popsáno v [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Například v [Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) ukázkové řešení, *Publish.proj* souboru:

- Sestavení řešení.
- Balení a nasazení aplikace ContactManager.Mvc pomocí nasazení webu.
- Balení a nasazení aplikace ContactManager.Service pomocí nasazení webu.
- Nasadí **ContactManager** databáze.

Při integraci nasazení více webových balíčků a/nebo databází do procesu krokování tímto způsobem můžete také možnost provedení celé nasazení v režimu "what if".

*Publish.proj* soubor ukazuje, jak to udělat. Nejprve je potřeba vytvořit vlastnost pro uložení hodnoty "what if":


[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]


V tomto případě vytvoříte vlastnost s názvem **WhatIf** s výchozí hodnotou **false**. Uživatelé mohou přepsat tuto hodnotu tak, že nastavíte vlastnost **true** v parametr příkazového řádku, jako je ukážeme za chvíli.

Další fází je parametrizovat Web Deploy a VSDBCMD příkazy, aby odrážely příznaků **WhatIf** hodnotu vlastnosti. Například další cíle (na základě *Publish.proj* souboru a zjednodušená čínština) běží *. deploy.cmd* uvést pro nasazení webového balíčku. Ve výchozím nastavení, příkaz zahrnuje **/Y** přepínače ("Ano" nebo režim aktualizace). Pokud **WhatIf** je nastavena na **true**, to je nahrazena **/T** přepínače (zkušební verze nebo režim "what if").


[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]


Další cílový obdobně, používá nástroj VSDBCMD nasazení databáze. Ve výchozím nastavení **/dd** přepínač není zahrnutý. To znamená, že VSDBCMD bude generovat skript nasazení, ale nenasadí databáze&#x2014;jinými slovy, "what if" scénáři. Pokud **WhatIf** vlastnost není nastavena na **true**, **/dd** přidán přepínač a VSDBCMD se nasazení databáze.


[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]


Můžete použít stejný přístup k parametrizaci všechny relevantní příkazy v souboru projektu. Pokud chcete spustit nasazení "what if", můžete pak stačí zadat **WhatIf** hodnota vlastnosti z příkazového řádku:


[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]


Tímto způsobem můžete spustit nasazení "what if" pro všechny součásti projektu v jediném kroku.

## <a name="conclusion"></a>Závěr

Toto téma popisuje, jak spustit "what if" nasazení pomocí nasazení webu, VSDBCMD a MSBuild. Nasazení "what if" umožňuje vyhodnotit její dopad navrhované nasazení před provedením jakýchkoli změn ve skutečnosti na cílovém prostředí.

## <a name="further-reading"></a>Další čtení

Další informace o nasazení webu syntaxe příkazového řádku najdete v tématu [nastavení operace nasazení webu](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Informace o možnostech příkazového řádku při použití *. deploy.cmd* souborů naleznete v tématu [postupy: instalace nasazení balíčku pomocí souboru deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx). Informace o syntaxi příkazového řádku VSDBCMD najdete v tématu [Reference k příkazovému řádku pro VSDBCMD. Soubor EXE (nasazení a Import schématu)](https://msdn.microsoft.com/library/dd193283.aspx).

> [!div class="step-by-step"]
> [Předchozí](advanced-enterprise-web-deployment.md)
> [další](customizing-database-deployments-for-multiple-environments.md)
