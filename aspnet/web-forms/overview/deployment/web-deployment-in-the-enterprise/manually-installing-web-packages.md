---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
title: Ruční instalace webových balíčků | Dokumentace Microsoftu
author: jrjlee
description: Toto téma popisuje, jak k ručnímu importu balíčku pro nasazení webu v Internetové informační služby (IIS). Vytváření tématu a balení webového instalováním...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f11d22a7-5d32-4ad0-8a9b-276460a61c06
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
msc.type: authoredcontent
ms.openlocfilehash: ca85db5cfb30bc06d6d3b94001a3668088461b87
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752230"
---
<a name="manually-installing-web-packages"></a>Ruční instalace webových balíčků
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje, jak k ručnímu importu balíčku pro nasazení webu v Internetové informační služby (IIS).
> 
> Téma [sestavení a balení projektů webových aplikací](building-and-packaging-web-application-projects.md) popisuje způsob, jakým služba IIS nástroj pro nasazení webu (nasazení webu), v kombinaci s Microsoft Build Engine (MSBuild) a webových publikování kanálu (WPP), umožňuje zabalit vaše projekty webových aplikací do souboru zip jeden. Tento soubor, obvykle označuje jako balíčku pro nasazení webu (nebo jednoduše pomocí balíčku nasazení), obsahuje všechny obsah a konfigurační informace, že služba IIS potřebuje, aby znovu vytvořit webové aplikace na webovém serveru.
> 
> Po vytvoření balíčku pro nasazení webu, můžete ji publikovat na server služby IIS různými způsoby. V možných scénářů budete chtít využít výhod bodů integrace mezi MSBuild, WPP a nasazení webu k vytvoření a instalace webových balíčků vzdáleně jako součást krokování nebo automatizovaného sestavení a nasazení procesu. Tento proces je popsán v [nasazení webových balíčků](deploying-web-packages.md). Ale to není vždy možné. Předpokládejme, že chcete nasadit webovou aplikaci do produkčního prostředí přístupem k Internetu. Z bezpečnostních důvodů produkčním prostředí je na velmi nejméně pravděpodobně být za bránou firewall v podsíti oddělené od serveru sestavení, v hraniční síti (označované také jako DMZ, demilitarizovaná zóna a monitorovaná podsíť). V mnoha případech bude produkčního prostředí v samostatné doméně nebo fyzicky izolované sítě.
> 
> V těchto scénářích může být vaší jedinou možností k portu webový balíček na cílový server a ji manuálně naimportovat do služby IIS. Přestože tento přístup vylučuje automatického nasazení, je stále vysoce efektivní technikou pro publikování webové aplikace&#x2014;jednoduše zkopírovat soubor zip jeden webový server a pomocí Průvodce vás provede procesem importu.


Toto téma je součástí série kurzů podle požadavků na nasazení enterprise fiktivní společnosti s názvem společnosti Fabrikam, Inc. V této sérii kurzů používá ukázkové řešení&#x2014; [řešení Správce kontaktů](the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s realistické úroveň složitosti, včetně aplikace ASP.NET MVC 3, komunikace Windows Služba Foundation (WCF) a databázový projekt.

## <a name="task-overview"></a>Přehled úloh

Budete potřebovat k dokončení těchto úloh k importu balíčku pro nasazení webu do služby IIS:

- Vytvoření balíčku pro nasazení webu pomocí nástroje MSBuild příkazového řádku, Team Build nebo Visual Studio 2010.
- Webový balíček zkopírujte na cílový webový server.
- Pomocí Průvodce importem balíčku aplikace k instalaci balíčku webu a zadejte hodnoty pro proměnné, jako jsou připojovací řetězce a koncových bodů služby ve Správci služby IIS.

Toto téma se ukazují, jak provést tyto postupy. Úlohy a názorné postupy v tomto tématu se předpokládá, že jste již obeznámeni s principy webových balíčků, Web Deploy a WPP. Další informace najdete v tématu [sestavení a balení projektů webových aplikací](building-and-packaging-web-application-projects.md).

> [!NOTE]
> Toto téma je nejlepší použít ve spojení s [nakonfigurovat webový Server pro nasazení publikování na webu (Offline nasazení)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md), který vysvětluje, jak nainstalovat požadované součásti a přípravě webu služby IIS k importu balíčku naimportovány.


## <a name="create-a-web-deployment-package"></a>Vytvoření balíčku pro nasazení webu

První úloha je vytvoření balíčku pro nasazení webu pro projekt webové aplikace, kterou chcete nasadit. Webových balíčků můžete vytvořit mnoha různými způsoby.

**Způsob 1: Vytvoření balíčku jako součást procesu sestavení pomocí sady Visual Studio**

Můžete nakonfigurovat projektu webové aplikace k vytvoření balíčku pro nasazení webu po každém sestavení prostřednictvím **balení/publikování webu** karty na stránkách vlastností projektu. Tento proces je popsán v [sestavení a balení projektů webových aplikací](building-and-packaging-web-application-projects.md).

**Způsob 2: Vytvoření balíčku jako součást procesu sestavení pomocí nástroje MSBuild**

Při sestavování projektu webové aplikace s použitím přímo, MSBuild, buď prostřednictvím vlastního souboru projektu nástroje MSBuild nebo z příkazového řádku, můžete vytvořit balíček nasazení webu jako součást procesu sestavení zahrnutím **DeployOnBuild = true** a **DeployTarget = balíček** vlastnosti ve svých rukou. Tento proces je popsán v [Principy procesu sestavení](understanding-the-build-process.md).

**Způsob 3: Vytvoření balíčku na vyžádání v sadě Visual Studio**

Kdykoli v sadě Visual Studio 2010 můžete vytvořit balíček nasazení webu pro webové aplikace. Chcete-li to provést, v **Průzkumníka řešení** okna, klikněte pravým tlačítkem na projekt webové aplikace a pak klikněte na tlačítko **sestavení balíčku pro nasazení**.

![](manually-installing-web-packages/_static/image1.png)

**Způsob 4: Vytvoření balíčku na vyžádání z příkazového řádku**

Můžete vytvořit balíček nasazení webu z příkazového řádku vyvoláním **balíčku** cíl v projektu webové aplikace pomocí nástroje MSBuild. Příkaz by měl vypadat takto:


[!code-console[Main](manually-installing-web-packages/samples/sample1.cmd)]


Podle toho, co můžete přistupovat použít, konečný výsledek je stejný. WPP vytvoří balíčku pro nasazení webu jako soubor zip, spolu s různými Podpůrné prostředky ve výstupní složce pro váš projekt webové aplikace.

![](manually-installing-web-packages/_static/image2.png)

Pokud plánujete Ruční import webový balíček, budete potřebovat pouze soubor zip. Tento soubor zkopírovat na cílový webový server a můžete začít proces importu.

## <a name="import-a-web-package-into-iis"></a>Importovat webový balíček do služby IIS

Následující postup slouží k importu balíčku pro nasazení webu z místního systému souborů do webu služby IIS. Před provedením tohoto postupu, ujistěte se, že máte:

- Zkopírovat balíčku pro nasazení webu na webový server.
- Nakonfigurovat webový server služby IIS pro hostování vaší aplikace.

Další informace o konfiguraci webový server služby IIS pro podporu balíčky nasazení webu, naleznete v tématu [nakonfigurovat webový Server pro nasazení publikování na webu (Offline nasazení)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

**K importu balíčku pro nasazení webu pomocí Správce služby IIS**

1. Ve Správci služby IIS v **připojení** podokně klikněte pravým tlačítkem na váš web služby IIS, přejděte na **nasadit**a potom klikněte na tlačítko **importovat aplikaci**.

    ![](manually-installing-web-packages/_static/image3.png)
2. V Průvodci importem aplikace balíčku na **vyberte balíček** stránce, přejděte do umístění balíčku pro nasazení webu a pak klikněte na tlačítko **Další**.
3. Na **vybrat obsah balíčku** stránce, zrušte zaškrtnutí políčka veškerý obsah, který není mají a potom klikněte na tlačítko **Další**.

    ![](manually-installing-web-packages/_static/image4.png)

    > [!NOTE]
    > V mnoha případech nemusíte chtít importovat, vše, který je součástí balíčku pro nasazení webu. Například možná chcete povolit nasazení webu k nahrazení příslušné databáze.  
    > **Udělit oprávnění** položky nastavit oprávnění pro cílový systém souborů k Ujistěte se, že identita fondu aplikací přístup k fyzické složce, která ukládá obsah webu. Kromě toho uživatel anonymní ověřování je udělení oprávnění ke čtení do složky umožňuje aplikaci poskytovat soubory typu Multipurpose Internet Mail Extensions (MIME). Pokud dáváte přednost, můžete odebrat tyto položky a ruční konfigurace oprávnění.
4. Na **zadání informací o balíčku aplikace** stránky, zadejte požadované informace.

    ![](manually-installing-web-packages/_static/image5.png)
5. Při vytváření webového balíčku WPP analyzuje konfiguračního souboru pro vaši aplikaci a detekuje všechny proměnné, jako jsou připojovací řetězce a koncových bodů služby. V tomto případě:

    1. **Cesta k aplikaci** je cesta služby IIS, ve které chcete nainstalovat aplikaci. Toto nastavení je společné pro všechny balíčky pro nasazení, které vytvoří WPP.
    2. **Adresa koncového bodu služby ContactService** je adresa, která by aplikace měla použít pro komunikaci s nasazenou službu WCF. Toto nastavení odpovídá položce v *web.config* souboru.
    3. První **připojovací řetězec** nastavení je připojovací řetězec, nasazení webu by měl použít k nasazení databáze přidružené k aplikaci (v tomto případě členské databáze ASP.NET). Toto nastavení odpovídá nastavení na **balení/publikování kódu SQL** kartu v sadě Visual Studio.
    4. Druhá **připojovací řetězec** nastavení je připojovací řetězec, který bude aplikace ve skutečnosti používat pro komunikaci s databází, když je zprovozněný. To odpovídá připojovací řetězec v *web.config* souboru.

        > [!NOTE]
        > Další informace o odkud pochází těchto parametrů najdete v tématu [konfigurace parametrů nasazení webového balíčku](configuring-parameters-for-web-package-deployment.md).
6. Klikněte na tlačítko **Další**.
7. Pokud to není při prvním nasazení aplikace na tento web, budete vyzváni k určení, zda chcete odstranit veškerý existující obsah před instalací. Zvolte možnost, která je vhodná pro vaše požadavky a pak klikněte na **Další**.

    ![](manually-installing-web-packages/_static/image6.png)
8. Po dokončení instalace balíčku služby IIS klikněte na tlačítko **Dokončit**.

    ![](manually-installing-web-packages/_static/image7.png)

V tomto okamžiku jste úspěšně publikovala webové aplikace do služby IIS.

## <a name="conclusion"></a>Závěr

Toto téma popisuje postup při importu balíčku pro nasazení webu na webu služby IIS pomocí Správce služby IIS. Tento přístup k publikování aplikace na webu je vhodné, pokud omezení zabezpečení nebo infrastrukturou vzdálené nasazení nemožné nebo nežádoucí.

## <a name="further-reading"></a>Další čtení

Pokyny o tom, jak nakonfigurovat webový server služby IIS pro podporu ručním importu webového balíčku najdete v tématu [nakonfigurovat webový Server pro nasazení publikování na webu (Offline nasazení)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Další obecné informace o nasazení webových balíčků naleznete v tématu [návod: nasazení webové aplikace projektu s využitím balíčku pro nasazení webu (část 1 ze 4)](https://msdn.microsoft.com/library/dd483479.aspx).

> [!div class="step-by-step"]
> [Předchozí](creating-and-running-a-deployment-command-file.md)
