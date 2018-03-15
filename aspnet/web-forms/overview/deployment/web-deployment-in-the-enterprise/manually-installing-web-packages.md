---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
title: "Ruční instalaci webových balíčků | Microsoft Docs"
author: jrjlee
description: "Toto téma popisuje, jak k ručnímu importu balíčku pro nasazení webu v Internetové informační služby (IIS). Vytváření tématu a balení webové aplikace..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f11d22a7-5d32-4ad0-8a9b-276460a61c06
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
msc.type: authoredcontent
ms.openlocfilehash: e06d37c01ab66f0723b687f4ed1ee72561099aef
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
<a name="manually-installing-web-packages"></a>Ruční instalaci webových balíčků
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje, jak k ručnímu importu balíčku pro nasazení webu v Internetové informační služby (IIS).
> 
> Téma [budova a balení projekty webových aplikací](building-and-packaging-web-application-projects.md) popisuje způsob, jakým služba IIS nástroj pro nasazení webu (Web Deploy), ve spojení s Microsoft Build Engine (MSBuild) a Knihovna webových publikování kanálu (WPP), umožňuje balíček vaší projekty webových aplikací do souboru zip jeden. Tento soubor, obvykle označuje jako balíčku pro nasazení webu (nebo jednoduše balíčku pro nasazení), obsahuje všechny obsah a konfiguraci informace, že služba IIS potřebuje k znovu vytvoření webové aplikace na webovém serveru.
> 
> Po vytvoření balíčku pro nasazení webu, můžete ho publikovat na server služby IIS různými způsoby. V celá řada možných scénářů budete chtít využívat výhod body integrace mezi MSBuild, WPP a nasazení webu k vytvoření a vzdáleně instalovat webových balíčků v rámci procesu automatické nebo krokování sestavení a nasazení. Tento proces je popsán v [nasazení webových balíčků](deploying-web-packages.md). Ale tato akce není vždy možné. Předpokládejme, že chcete nasadit webovou aplikaci pro produkční prostředí internetového. Z bezpečnostních důvodů provozním prostředí je na velmi alespoň pravděpodobně být za bránou firewall v podsíti, která je oddělená od serveru sestavení v hraniční síti (označované také jako DMZ, demilitarizovaná zóna a monitorovaná podsíť). V mnoha případech bude produkčním prostředí na samostatné doméně nebo fyzicky izolované sítě.
> 
> V těchto scénářích může být jedinou možností k portu webového balíčku na cílový server a ji manuálně naimportovat do služby IIS. I když tento přístup vylučuje automatického nasazení, je stále vysoce efektivní technika pro publikování webové aplikace & #x 2014; jednoduše zkopírovat soubor zip jeden webový server a pomocí Průvodce vás provede procesu importu.


Toto téma je součástí ze série kurzů na základě kolem podnikové požadavky nasazení fiktivní společnost s názvem Fabrikam, Inc. Tento kurz series používá ukázkové řešení & #x 2014; [řešení obraťte se na správce](the-contact-manager-solution.md)& #x 2014; představující webovou aplikaci s úrovní realistické složitější, včetně aplikace ASP.NET MVC 3 systému Windows Komunikační služby Foundation (WCF) a projekt databáze.

## <a name="task-overview"></a>Přehled úloh

Budete potřebovat k dokončení těchto úloh vysoké úrovně pro import balíčku pro nasazení webu do služby IIS:

- Vytvoření balíčku pro nasazení webu pomocí nástroje MSBuild příkazového řádku, Team Build nebo Visual Studio 2010.
- Zkopírujte balíček webové na cílový webový server.
- Pomocí Průvodce importem balíčku aplikace ve Správci služby IIS pro instalaci webového balíčku a zadejte hodnoty pro proměnné jako koncové body služby a připojovací řetězce.

Toto téma vám ukáže, jak k provedení těchto postupů. Úlohy a postupy v tomto tématu předpokládat, že jste již obeznámeni s principy webových balíčků, Web Deploy a jako. Další informace najdete v tématu [budova a projekty webových aplikací balení](building-and-packaging-web-application-projects.md).

> [!NOTE]
> Toto téma je nevhodnější používat ve spojení s [konfigurace webového serveru pro nasazení publikování na webu (Offline nasazení)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md), která vysvětluje, jak nainstalovat požadované součásti a příprava webu IIS pro import balíčku.


## <a name="create-a-web-deployment-package"></a>Vytvoření balíčku pro nasazení webu

První úlohou je vytvoření balíčku pro nasazení webu pro projekt webové aplikace, které chcete nasadit. Webové balíčky lze vytvořit v mnoha různými způsoby.

**Způsob 1: Vytvoření balíčku jako součást procesu sestavení pomocí sady Visual Studio**

Můžete nakonfigurovat projektu webové aplikace k vytvoření balíčku pro nasazení webu po každé sestavení prostřednictvím **balení/publikování webu** karty na stránkách vlastností projektu. Tento proces je popsán v [budova a projekty webových aplikací balení](building-and-packaging-web-application-projects.md).

**Způsob 2: Vytvoření balíčku jako součást procesu sestavení pomocí nástroje MSBuild**

Pokud vytvoříte projektu webové aplikace pomocí přímo, MSBuild, buď prostřednictvím vlastní soubor projektu nástroje MSBuild nebo z příkazového řádku, můžete vytvořit balíček nasazení webu jako součást procesu sestavení zahrnutím **DeployOnBuild = true** a **DeployTarget = balíček** vlastnosti v příkazu. Tento proces je popsán v [Principy procesu sestavení](understanding-the-build-process.md).

**Způsob 3: Vytvoření balíčku na vyžádání v sadě Visual Studio**

Kdykoli v sadě Visual Studio 2010 můžete vytvořit balíček nasazení webu pro projekt webové aplikace. Chcete-li to provést, v **Průzkumníku řešení** oken, klikněte pravým tlačítkem na projekt vaší webové aplikace a pak klikněte na tlačítko **sestavení balíčku pro nasazení**.

![](manually-installing-web-packages/_static/image1.png)

**Způsob 4: Vytvoření balíčku na vyžádání z příkazového řádku**

Vytvořením balíčku pro nasazení webu z příkazového řádku vyvoláním **balíček** cíl na vaší webové aplikace pomocí nástroje MSBuild. Příkaz by měl vypadat takto:


[!code-console[Main](manually-installing-web-packages/samples/sample1.cmd)]


Podle toho, co přístupu můžete použít, konečný výsledek je stejný. JAKO vytvoří balíček nasazení webu jako soubor zip, společně s různými doprovodné materiály do výstupní složky pro váš projekt webové aplikace.

![](manually-installing-web-packages/_static/image2.png)

Při plánování ručně importovat webový balíček, je třeba pouze soubor zip. Tento soubor zkopírovat na cílový webový server a můžete zahájit proces importu.

## <a name="import-a-web-package-into-iis"></a>Import balíčku Web do služby IIS

Následující postup slouží k importu balíčku pro nasazení webu z místního systému souborů na webu IIS. Před provedením tohoto postupu se ujistěte, že máte:

- Kopírovat balíčku pro nasazení webu na webový server.
- Nakonfigurovat webový server služby IIS pro hostování vaší aplikace.

Další informace o konfiguraci webový server služby IIS pro podporu balíčky nasazení webu, najdete v části [konfigurace webového serveru pro nasazení publikování na webu (Offline nasazení)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

**K importu balíčku pro nasazení webu pomocí Správce služby IIS**

1. Ve Správci služby IIS v **připojení** podokně klikněte pravým tlačítkem na váš web služby IIS, přejděte na **nasadit**a potom klikněte na **importovat aplikaci**.

    ![](manually-installing-web-packages/_static/image3.png)
2. V Průvodci importem aplikace balíčku na **vyberte balíček** stránky, přejděte do umístění vašeho balíčku pro nasazení webu a pak klikněte na tlačítko **Další**.
3. Na **vybrat obsah balíčku** zrušte zaškrtnutí veškerý obsah, který není vyžadují a pak klikněte na tlačítko **Další**.

    ![](manually-installing-web-packages/_static/image4.png)

    > [!NOTE]
    > V mnoha případech nemusíte chtít importovat všechno, která je součástí balíčku pro nasazení webu. Například nemusíte chtít povolit nasazení webu k nahrazení přidružené databáze.  
    > **Udělit oprávnění** položky nastavit oprávnění pro cílový systém souborů na Ujistěte se, že identita fondu aplikací přístup se fyzická složka, která ukládá obsah webu. Kromě toho uživatel anonymní ověřování je uděleno oprávnění ke čtení do složky, které umožní aplikaci sloužit soubory typu Multipurpose Internet Mail Extensions (MIME). Pokud dáváte přednost, můžete odebrat tyto položky a ruční konfigurace oprávnění.
4. Na **zadání informací o balíčku aplikace** stránky, zadejte požadované informace.

    ![](manually-installing-web-packages/_static/image5.png)
5. Při vytváření balíčku webu jako analyzuje konfiguračního souboru pro vaši aplikaci a zjistí všechny proměnné, jako koncové body služby a připojovací řetězce. V tomto případě:

    1. **Cesta k aplikaci** je IIS cesta, kam chcete nainstalovat aplikaci. Toto nastavení je společné pro všechny balíčky pro nasazení, které vytvoří jako.
    2. **Adresa koncového bodu služby ContactService** je adresa, která by aplikace měla použít pro komunikaci s nasazené služby WCF. Toto nastavení odpovídá položce v *web.config* souboru.
    3. První **připojovací řetězec** nastavení je připojovací řetězec, který Web Deploy měla použít pro nasazení databáze přidružené k aplikaci (v tomto případě členské databáze ASP.NET). Toto nastavení odpovídá nastavení na **balení/publikování kódu SQL** kartě v sadě Visual Studio.
    4. Druhý **připojovací řetězec** nastavení je připojovací řetězec, který vaše aplikace bude ve skutečnosti používat ke komunikaci s databází, když je spuštěná. To odpovídá připojovací řetězec v *web.config* souboru.

        > [!NOTE]
        > Další informace na místo, odkud tyto parametry pocházet najdete v tématu [parametry konfigurace pro nasazení webového balíčku](configuring-parameters-for-web-package-deployment.md).
6. Klikněte na tlačítko **Další**.
7. Pokud to není při prvním nasazení aplikace na tento web, budete vyzváni k určení, zda chcete odstranit všechny existující obsah před instalací. Zvolte možnost, která je vhodné pro vaše požadavky a pak klikněte na tlačítko **Další**.

    ![](manually-installing-web-packages/_static/image6.png)
8. Po dokončení instalace balíčku služby IIS klikněte na tlačítko **Dokončit**.

    ![](manually-installing-web-packages/_static/image7.png)

V tomto okamžiku jste úspěšně publikovala webové aplikace do služby IIS.

## <a name="conclusion"></a>Závěr

Toto téma popisuje postup importování balíčku pro nasazení webu na webu IIS pomocí Správce služby IIS. Tento přístup k publikování webových aplikací je vhodné při omezení zabezpečení nebo infrastruktury zajistit vzdálené nasazení obtížné nebo nežádoucí.

## <a name="further-reading"></a>Další čtení

Pokyny o tom, jak nakonfigurovat webový server služby IIS pro podporu Ruční import balíčku webu najdete v tématu [konfigurace webového serveru pro nasazení publikování na webu (Offline nasazení)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Další obecné pokyny pro nasazení webových balíčků najdete v tématu [návod: nasazení webové aplikace projektu pomocí balíčku pro nasazení webu (část 1 / 4)](https://msdn.microsoft.com/library/dd483479.aspx).

>[!div class="step-by-step"]
[Předchozí](creating-and-running-a-deployment-command-file.md)
