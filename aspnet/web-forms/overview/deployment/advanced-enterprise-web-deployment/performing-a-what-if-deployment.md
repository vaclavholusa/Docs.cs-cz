---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: "Provádění a jaké Pokud nasazení | Microsoft Docs"
author: jrjlee
description: "Toto téma popisuje, co když' provádění (nebo simulated) pomocí nástroj nasazení webu Internetové informační služby (IIS) (Web Deploy) a V nasazení..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: 62be7c9636fb74c40bec812e9ac76b360995da50
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="performing-a-what-if-deployment"></a>Provádění nasazení "Co když"
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje, jak provést "Co když" (nebo simulated) pomocí nástroj nasazení webu Internetové informační služby (IIS) (Web Deploy) a VSDBCMD nasazení. Díky tomu můžete určit důsledky logika nasazení v prostředí s konkrétní cílový před svou aplikaci nasazujete ve skutečnosti.


Toto téma je součástí ze série kurzů na základě kolem podnikové požadavky nasazení fiktivní společnost s názvem Fabrikam, Inc. Tento kurz series používá ukázkové řešení & #x 2014; [řešení obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; představující webovou aplikaci s úrovní realistické složitější, včetně aplikace ASP.NET MVC 3 systému Windows Komunikační služby Foundation (WCF) a projekt databáze.

Metoda nasazení jádrem tyto kurzy je založena na soubor projektu rozdělení metody uvedené v [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve které dva soubory projektu & #x 2014 je řízena procesem sestavení a nasazení; o Ne, obsahující sestavení pokyny, které platí pro každé cílové prostředí a jeden, který obsahuje nastavení pro konkrétní prostředí sestavení a nasazení. V okamžiku sestavení souboru projektu konkrétní prostředí sloučeny do souboru projektu bez ohledu na prostředí a vytvořit úplnou sadu pokynů sestavení.

## <a name="performing-a-what-if-deployment-for-web-packages"></a>Provádění pro balíčky webového nasazení "Co když"

Nasazení webu obsahuje funkce, které umožňuje provádění nasazení v "Co když" (nebo zkušební verze) režimu. Když nasadíte artefakty v režimu "Co když", Web Deploy vygeneruje soubor protokolu, jako kdyby měl provést nasazení, ale se nic nezmění. ve skutečnosti na cílovém serveru. Kontrola souboru protokolu vám můžou pomoct pochopit, jaký dopad bude mít vaše nasazení na cílovém serveru, zejména:

- Co se se přidají.
- Co se aktualizovat.
- Co bude odstraněn.

Protože je nasazení "Co když" nic nezmění. ve skutečnosti na cílovém serveru se nemůže vždy provádět předpovědi, zda bude úspěšné nasazení.

Jak je popsáno v [nasazení webových balíčků](../web-deployment-in-the-enterprise/deploying-web-packages.md), bude možné nasadit balíčky webového pomocí v dva způsoby & #x 2014; pomocí příkazový řádek MSDeploy.exe přímo nebo spuštěním nástroje nasazení webu *. deploy.cmd* soubor, který generuje procesu sestavení.

Pokud používáte MSDeploy.exe přímo, můžete spustit nasazení "Co když" tak, že přidáte **– whatif** příznak do příkazu. Například k vyhodnocení, co by mohlo dojít, pokud jste nasadili balíček ContactManager.Mvc.zip pracovní prostředí, příkaz MSDeploy by měl vypadat přibližně takto:


[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]


Až budete spokojeni s výsledky nasazení "Co když", můžete odebrat **– whatif** příznak pro spuštění nasazení za provozu.

> [!NOTE]
> Další informace o parametrech příkazového řádku pro MSDeploy.exe najdete v tématu [nastavení operace nasazení webu](https://technet.microsoft.com/en-us/library/dd569089(WS.10).aspx).


Pokud používáte *. deploy.cmd* souboru, můžete spustit nasazení "Co když" zahrnutím **/t** příznak příznak (zkušební režim) místo **/y** příznak ("Ano" nebo režim aktualizace) v váš příkaz. Například k vyhodnocení, co by mohlo dojít, pokud jste nasadili balíček ContactManager.Mvc.zip spuštěním *. deploy.cmd* souboru příkazu by měl vypadat takto:


[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]


Až budete spokojeni s výsledky nasazení "zkušební režim", můžete nahradit **/t** příznak s **/y** příznak pro spuštění nasazení za provozu:


[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]


> [!NOTE]
> Další informace o parametrech příkazového řádku pro *. deploy.cmd* soubory, najdete v části [postupy: instalace nasazení balíčku pomocí souboru deploy.cmd](https://msdn.microsoft.com/en-us/library/ff356104.aspx). Pokud spustíte *. deploy.cmd* souboru bez zadání žádné příznaky, příkazovém řádku se zobrazí seznam dostupných příznaků.


## <a name="performing-a-what-if-deployment-for-databases"></a>Provádění nasazení "Co když" pro databáze

V této části se předpokládá, že používáte nástroj VSDBCMD k provedení nasazení přírůstkové, na základě schématu databáze. Tento postup je popsán v podrobněji [nasazení databázové projekty](../web-deployment-in-the-enterprise/deploying-database-projects.md). Doporučujeme vám, že je seznámit s tímto tématem před použitím konceptů popsaných v tomto poli.

Při použití VSDBCMD v **nasadit** režimu, můžete použít **/dd** (nebo **/DeployToDatabase**) příznak pro ovládací prvek, ať už VSDBCMD ve skutečnosti nasadí databázi nebo právě generuje skript nasazení. Pokud nasazujete soubor .dbschema, jedná se o chování:

- Pokud zadáte **/dd+** nebo **/dd**, VSDBCMD vygenerovat skript nasazení a nasazení databáze.
- Pokud zadáte **/dd-** nebo vynechejte přepínač, VSDBCMD vydá skript nasazení.

> [!NOTE]
> Pokud nasazujete souboru .deploymanifest místo soubor .dbschema chování **/dd** přepínač je mnohem složitější. V podstatě VSDBCMD bude ignorovat hodnotu **/dd** přepínače, pokud soubor .deploymanifest obsahuje **DeployToDatabase** element s hodnotou **True**. [Nasazení databázové projekty](../web-deployment-in-the-enterprise/deploying-database-projects.md) popisuje toto chování v plném znění.


Například pro generování skriptu pro nasazení **ContactManager** databáze bez ve skutečnosti nasazení databáze, VSDBCMD příkazu by měl vypadat takto:


[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]


VSDBCMD je nástroj pro nasazení rozdílové databáze a jako takový se dynamicky vygeneruje skript nasazení tak, aby obsahovala všechny příkazy SQL potřebnými k aktualizaci aktuální databázi, pokud existuje, zadaný schématu. Kontrola skript nasazení je užitečný způsob, jak určit, jaký vliv má vaše nasazení bude mít na aktuální databázi a data, která obsahuje. Například můžete chtít zjistit:

- Určuje, zda bude odebrána veškeré stávající tabulky a jestli, dojde ke ztrátě dat.
- Jestli pořadí operací s sebou nese riziko ztráty dat, například pokud jste rozdělení nebo sloučení tabulky.

Pokud budete spokojeni s skript nasazení, můžete opakovat VSDBCMD s **/dd+** příznak provést změny. Alternativně můžete upravit skript nasazení k plnění požadavků a pak ji ručně spustit na serveru databáze.

## <a name="integrating-what-if-functionality-into-custom-project-files"></a>Integrace funkce "Co když" do vlastních projektů souborů

Ve složitějších scénářích nasazení, budete chtít použít vlastní soubor projektu Microsoft Build Engine (MSBuild) k zapouzdření logika sestavení a nasazení, jak je popsáno v [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Například v [obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) ukázkové řešení, *Publish.proj* souboru:

- Sestavení řešení.
- Zabalení a nasazení aplikace ContactManager.Mvc používá nasazení webu.
- Zabalení a nasazení aplikace ContactManager.Service používá nasazení webu.
- Nasadí **ContactManager** databáze.

Při integraci nasazení více webových balíčků nebo databáze do procesu krokování tímto způsobem, můžete také možnost provedení celé nasazení v režimu "Co když".

*Publish.proj* souboru ukazuje, jak můžete to provést. Nejprve musíte vytvořit vlastnost pro uložení hodnotu "Co když":


[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]


V takovém případě jste vytvořili vlastnost s názvem **WhatIf** s výchozí hodnotou **false**. Uživatelé mohou tuto hodnotu přepsat nastavením vlastnosti **true** parametr příkazového řádku, jako je zobrazí za chvíli.

Další fáze je o parametrizaci nasazení webu a VSDBCMD příkazů tak, aby odrážela příznaků **WhatIf** hodnotu vlastnosti. Například Další cíl (převzaty z *Publish.proj* souborů a zjednodušená) běží *. deploy.cmd* souboru k nasazení webového balíčku. Ve výchozím nastavení, příkaz obsahuje **/Y** přepínače ("Ano" nebo režim aktualizace). Pokud **WhatIf** je nastaven na **true**, to je nahrazena **/T** přepínače (zkušební verze nebo režim "Co když").


[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]


Podobně Další cíl používá nástroj VSDBCMD nasazení databáze. Ve výchozím nastavení **/dd** přepínač není zahrnutý. To znamená, že VSDBCMD vygeneruje skript nasazení, ale nebude nasadit databázi & #x 2014; jinými slovy, "Co když" scénáři. Pokud **WhatIf** vlastnost není nastavena na **true**, **/dd** přidán přepínač a VSDBCMD se nasazení databáze.


[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]


Můžete použít stejný způsob o parametrizaci všechny relevantní příkazy v souboru projektu. Pokud chcete spustit nasazení "Co když", můžete pak stačí zadat **WhatIf** hodnotu vlastnosti z příkazového řádku:


[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]


Tímto způsobem můžete spustit "Co když" nasazení všech součástí projektu v jediném kroku.

## <a name="conclusion"></a>Závěr

Toto téma popisuje postup spouštět "Co když" nasazení pomocí nástroje nasazení webu, VSDBCMD a MSBuild. Nasazení "Co když" umožňuje hodnotit dopad navrhované nasazení před provedením jakýchkoli změn ve skutečnosti na cílovém prostředí.

## <a name="further-reading"></a>Další čtení

Další informace o nasazení webu syntaxe příkazového řádku najdete v tématu [nastavení operace nasazení webu](https://technet.microsoft.com/en-us/library/dd569089(WS.10).aspx). Pokyny k možnosti příkazového řádku při použití *. deploy.cmd* souborů najdete v tématu [postup: instalace nasazení balíčku pomocí souboru deploy.cmd](https://msdn.microsoft.com/en-us/library/ff356104.aspx). Pokyny v VSDBCMD syntaxe příkazového řádku najdete v tématu [Reference k příkazovému řádku pro VSDBCMD. EXE (nasazení a Import schématu)](https://msdn.microsoft.com/en-us/library/dd193283.aspx).

>[!div class="step-by-step"]
[Předchozí](advanced-enterprise-web-deployment.md)
[další](customizing-database-deployments-for-multiple-environments.md)
