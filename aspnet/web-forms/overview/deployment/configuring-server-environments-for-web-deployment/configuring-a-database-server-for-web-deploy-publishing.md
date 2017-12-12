---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
title: "Konfigurace databáze serveru pro Web nasazení publikování | Microsoft Docs"
author: jrjlee
description: "Toto téma popisuje postup konfigurace databáze serveru SQL Server 2008 R2 pro podporu nasazení webu a publikování. Úlohy popsané v tomto tématu se co..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: e7c447f9-eddf-4bbe-9f18-3326d965d093
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
msc.type: authoredcontent
ms.openlocfilehash: b225d9911246b3e2be1679b73a9f31d9f8577ba5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="configuring-a-database-server-for-web-deploy-publishing"></a>Konfigurace databáze serveru pro publikování nasazení webu
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje postup konfigurace databáze serveru SQL Server 2008 R2 pro podporu nasazení webu a publikování.
> 
> Úlohy popsané v tomto tématu jsou společné pro každý scénář nasazení & #x 2014, nezávisle na tom, zda webové servery jsou nakonfigurované na používání vzdálené služby agenta nástroj nasazení webu služby IIS (Web Deploy), obslužné rutiny nasazení webu nebo v režimu offline nasazení nebo vaše aplikace běží na jednom webovém serveru nebo serverové farmy. Způsob nasazení databáze může změnit závislosti na požadavcích zabezpečení a další důležité informace. Například můžete nasadit databázi s nebo bez ukázková data, a můžete nasadit mapování role uživatelů a nakonfigurovat ručně po nasazení. Způsob, jak konfigurovat databázový server ale zůstává stejná.


Nemusíte instalovat veškeré další produkty a nástroje pro konfiguraci databázový server pro podporu nasazení webu. Za předpokladu, že databázový server a webový server běžet na různých počítačích, jednoduše je potřeba:

- Povolit systému SQL Server na komunikaci pomocí protokolu TCP/IP.
- Povolí provoz systému SQL Server přes všechny brány firewall.
- Zadejte účet počítače webového serveru přihlášení systému SQL Server.
- Mapy připojení k účtu počítače k žádné roli požadovaná databáze.
- Zadejte účet, který se spustí nasazení oprávnění pro tvůrce přihlášení a databáze serveru SQL Server.
- Pro podporu nasazení na opakování, mapy nasazení účet přihlášení k **db\_vlastníka** role databáze.

Toto téma vám ukáže, jak provést všechny tyto postupy. Úlohy a postupy v tomto tématu předpokládat, že začínáte s výchozí instance systému SQL Server 2008 R2 se systémem Windows Server 2008 R2. Než budete pokračovat, zajistěte, aby:

- Windows Server 2008 R2 Service Pack 1 a všechny dostupné aktualizace, které jsou nainstalovány.
- Server je připojený k doméně.
- Server má statickou IP adresu.
- SQL Server 2008 R2 Service Pack 1 a všechny dostupné aktualizace, které jsou nainstalovány.

Instance systému SQL Server pouze musí obsahovat **služby databázového stroje** role, která je automaticky součástí všechny instalace systému SQL Server. Pro usnadnění konfigurace a údržby, doporučujeme však zahrnout **nástroje pro správu – základní** a **nástroje pro správu – dokončeno** role serveru.

> [!NOTE]
> Další informace o připojení počítače k doméně, najdete v části [připojení počítače k doméně a protokolování na](https://technet.microsoft.com/en-us/library/cc725618(v=WS.10).aspx). Další informace o konfiguraci statických IP adres najdete v tématu [nakonfigurovat statickou IP adresu](https://technet.microsoft.com/en-us/library/cc754203(v=ws.10).aspx). Další informace o instalaci systému SQL Server najdete v tématu [instalace systému SQL Server 2008 R2](https://technet.microsoft.com/en-us/library/bb500395.aspx).


## <a name="enable-remote-access-to-sql-server"></a>Povolte vzdálený přístup k systému SQL Server

SQL Server používá protokol TCP/IP pro komunikaci se vzdálenými počítači. Pokud databázový server a webový server na různé počítače, potřebujete:

- Nakonfigurujte nastavení sítě systému SQL Server komunikaci přes protokol TCP/IP.
- Nakonfigurujte všechny hardware nebo software brány firewall povolit provoz protokolu TCP (a v některých případech protokolu UDP (User Datagram) provozu) na porty, které používá instanci systému SQL Server.

K povolení SQL serveru pro komunikaci pomocí protokolu TCP/IP, použijte Správce konfigurace systému SQL Server a změňte konfiguraci sítě pro instanci serveru SQL.

**K povolení SQL serveru na komunikaci pomocí protokolu TCP/IP**

1. Na **spustit** nabídky, přejděte na příkaz **všechny programy**, klikněte na tlačítko **Microsoft SQL Server 2008 R2**, klikněte na tlačítko **nástroje pro konfiguraci**a potom klikněte na **Správce konfigurace systému SQL Server**.
2. V podokně stromu, rozbalte položku **konfigurace sítě serveru SQL Server**a potom klikněte na **protokoly pro MSSQLSERVER**.

    > [!NOTE]
    > Pokud jste nainstalovali více instancí systému SQL Server, zobrazí se **protokoly pro***[název instance]* položku pro každou instanci. Musíte nakonfigurovat nastavení sítě na základě instance instance.
3. V podokně podrobností klikněte pravým tlačítkem myši **TCP/IP** řádek a potom klikněte na **povolit**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image1.png)
4. V **upozornění** dialogové okno, klikněte na tlačítko **OK**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image2.png)
5. Budete muset restartovat službu MSSQLSERVER, se projeví až konfiguraci sítě. Můžete to udělat na příkazovém řádku, z konzoly služby nebo SQL Server Management Studio. V tomto postupu budete používat SQL Server Management Studio.
6. Zavřete Správce konfigurace systému SQL Server.
7. Na **spustit** nabídky, přejděte na příkaz **všechny programy**, klikněte na tlačítko **Microsoft SQL Server 2008 R2**a potom klikněte na **SQL Server Management Studio**.
8. V **připojit k serveru** dialogu **název serveru** pole, zadejte název databázového serveru a pak klikněte na tlačítko **připojit**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image3.png)
9. V **Průzkumník objektů** podokně klikněte pravým tlačítkem na nadřazený uzel serveru (například **TESTDB1**) a potom klikněte na **restartujte**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image4.png)
10. V **Microsoft SQL Server Management Studio** dialogové okno, klikněte na tlačítko **Ano**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image5.png)
11. Pokud byla restartována, zavřete SQL Server Management Studio.

Povolit provoz systému SQL Server přes bránu firewall, musíte nejprve potřebujete vědět, které porty používá instanci serveru SQL. To závisí na tom, jak instance systému SQL Server byl vytvořen a nakonfigurován:

- A *výchozí instanci* systému SQL Server naslouchá (a reaguje na) žádosti na TCP port 1433.
- A *pojmenovanou instanci* systému SQL Server naslouchá (a reaguje na) požadavky na dynamicky přiřazeného portu TCP.
- Pokud je povolena služba SQL Server Browser, klienti mohou odesílat dotazy na službu na portu UDP 1434 a zjistěte, port TCP, který chcete použít pro konkrétní instanci systému SQL Server. Tato služba je však zakázána často z bezpečnostních důvodů.

Za předpokladu, že používáte výchozí instanci systému SQL Server, budete muset nakonfigurovat bránu firewall, aby umožňovaly přenosy.

| Směr | Z portu | K portu | Typ portu |
| --- | --- | --- | --- |
| Příchozí | všechny | 1433 | TCP |
| Odchozí | 1433 | všechny | TCP |
  

> [!NOTE]
> Technicky klientský počítač bude používat náhodně přidělenému portu TCP 1024 až 5000 pro komunikaci se serverem SQL Server a vašich pravidlech brány firewall můžete omezit odpovídajícím způsobem. Další informace o porty systému SQL Server a brány firewall najdete v tématu [čísla portů TCP/IP vyžadované pro komunikaci přes bránu firewall pro SQL](https://go.microsoft.com/?linkid=9805125) a [postupy: Konfigurace serveru pro naslouchání na specifickém portu TCP (Konfigurace serveru SQL Server Správce)](https://msdn.microsoft.com/en-us/library/ms177440.aspx).


Ve většině prostředí systému Windows Server budete pravděpodobně muset konfigurace brány Windows Firewall na databázovém serveru. Ve výchozím nastavení brány Windows Firewall umožňuje všechny odchozí přenosy, pokud pravidlo výslovně nezakáže. Pokud chcete povolit přístup k databázi webovému serveru, musíte nakonfigurovat příchozí pravidlo, které umožňuje přenos TCP na číslo portu, který používá instanci systému SQL Server. Pokud používáte výchozí instanci systému SQL Server, můžete konfigurovat toto pravidlo v dalším postupu.

**Konfigurace brány Windows Firewall umožňující navázat komunikaci s výchozí instanci SQL serveru**

1. Na serveru databáze na **spustit** nabídky, přejděte na příkaz **nástroje pro správu**a potom klikněte na **brány Windows Firewall s pokročilým zabezpečením**.
2. V podokně stromu, klikněte na tlačítko **příchozí pravidla**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image6.png)
3. V **akce** podokně v části **příchozí pravidla**, klikněte na tlačítko **nové pravidlo**.
4. V nástroji příchozí pravidlo Průvodce novou na **typ pravidla** vyberte **Port**a potom klikněte na **Další**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image7.png)
5. Na **protokol a porty** zkontrolujte, že **TCP** je vybrána a v **určité místní porty** zadejte **1433**a potom klikněte na **Další**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image8.png)
6. Na **akce** ponechte **povolit připojení** vybraný a klikněte na tlačítko **Další**.
7. Na **profil** ponechte **domény** vybrána, zrušte **privátní** a **veřejné** a pak klikněte na tlačítko  **Další**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image9.png)
8. Na **název** stránky, zadejte pro pravidlo vhodně popisný název (například **výchozí instanci systému SQL Server – přístup k síti**) a potom klikněte na **Dokončit**.

Další informace o konfiguraci brány Windows Firewall pro SQL Server, zvlášť pokud je nutné komunikovat se serverem SQL Server přes nestandardní nebo dynamické porty, najdete v části [postupy: Konfigurace brány Windows Firewall pro přístup k databázovému stroji](https://technet.microsoft.com/en-us/library/ms175043.aspx).

## <a name="configure-logins-and-database-permissions"></a>Konfigurace přihlášení a databáze oprávnění

Když nasadíte webovou aplikaci k Internetové informační služby (IIS), je aplikace spuštěná pomocí identity fondu aplikací. V prostředí domény identity fondu aplikací použijte účet počítače serveru, na kterém poběží přístup k síťovým prostředkům. Účty počítače ve formě *[název domény]***\***[název počítače] ***$**& #x 2014, například **FABRIKAM\ TESTWEB1$**. Chcete-li povolit webovou aplikaci na přístup k databázi v síti, je potřeba:

- Přidáte přihlašovací jméno pro účet počítače webového serveru do instance systému SQL Server.
- Mapy připojení k účtu počítače k žádné roli požadovaná databáze (obvykle **db\_DataReader –** a **db\_datawriter**).

Pokud webová aplikace běží na serverové farmy, nikoli na jednom serveru, musíte pro každého webového serveru v serverové farmě, opakujte tyto postupy.

> [!NOTE]
> Další informace o identity fondu aplikací a přístupu k síťovým prostředkům, najdete v části [identity fondu aplikací součásti](https://go.microsoft.com/?linkid=9805123).


Můžete postupovat tyto úlohy různými způsoby. Chcete-li vytvořit přihlášení, můžete provést jeden z následujících kroků:

- Ručně vytvořte přihlášení na serveru databáze pomocí jazyka Transact-SQL nebo SQL Server Management Studio.
- Použijte SQL Server 2008 serverový projekt v sadě Visual Studio k vytvoření a nasazení přihlášení.

Přihlášení systému SQL Server je objekt na úrovni serveru, nikoli o objekt na úrovni databáze, takže není závislá na databázi, kterou chcete nasadit. Takto můžete kdykoli vytvořit přihlášení a nejjednodušší způsob je často vytvoření přihlášení ručně na serveru databáze před zahájením nasazení databází. Následující postup slouží k vytvoření přihlášení v systému SQL Server Management Studio.

**Jak vytvořit přihlášení systému SQL Server pro účet počítače webového serveru**

1. Na serveru databáze na **spustit** nabídky, přejděte na příkaz **všechny programy**, klikněte na tlačítko **Microsoft SQL Server 2008 R2**a potom klikněte na **SQL Server Management Studio** .
2. V **připojit k serveru** dialogu **název serveru** pole, zadejte název databázového serveru a pak klikněte na tlačítko **připojit**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image10.png)
3. V **Průzkumník objektů** podokně klikněte pravým tlačítkem na **zabezpečení**, přejděte na příkaz **nový**a potom klikněte na **přihlášení**.
4. V **přihlášení – nové** dialogu **přihlašovací jméno** pole, zadejte název účtu počítače webového serveru (například **FABRIKAM\TESTWEB1$**).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image11.png)
5. Click **OK**.

Databázový server je nyní připraven pro publikování nasazení webu. Žádné řešení, které můžete nasadit však nebude fungovat, dokud mapování přihlašovací účet počítače na požadované databázové role. Mapování přihlášení do role databáze vyžaduje více představit mnoho, jako je nelze mapy rolí až po jste jste nasadili databáze. Chcete-li mapovat přihlášení k účtu počítače role požadované databáze, můžete provést jeden z následujících kroků:

- Přiřadíte role databáze přihlášení ručně, poté, co nasadíte databázi poprvé.
- Pomocí skriptu po nasazení chcete přihlášení přiřadit role databáze.

Další informace o automatické vytváření přihlášení a mapování role databáze najdete v tématu [nasazení členství Role databáze pro testovací prostředí](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Alternativně můžete v dalším postupu mapovat přihlášení k účtu počítače role požadovaná databáze ručně. Mějte na paměti, že nelze provést tento postup, dokud *po* nasadíte databázi.

**Pro mapování databázové role na připojení k účtu počítače webového serveru**

1. SQL Server Management Studio otevřete jako předtím.
2. V **Průzkumník objektů** podokně rozbalte **zabezpečení** uzlu, rozbalte **přihlášení** uzel a potom dvakrát klikněte na přihlášení k účtu počítače (například  **FABRIKAM\TESTWEB1$**).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image12.png)
3. V **vlastnosti přihlášení** dialogové okno, klikněte na tlačítko **mapování uživatelských**.
4. V **mapovat uživatele na toto přihlášení** tabulky, vyberte název databáze (například **ContactManager**).
5. V **databáze členství role pro:** *[název databáze]* vyberte oprávnění potřebná. V případě správce kontaktujte ukázkové řešení, je nutné vybrat **db\_DataReader –** a **db\_datawriter** role.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image13.png)
6. Click **OK**.

Když ručně mapování databázové role je často více než dostačující pro testovací prostředí, je méně vhodné pro nasazení automatizované nebo jedním kliknutím k pracovním nebo produkčním prostředí. Můžete najít další informace o automatizaci tento druh úloh pomocí skriptů po nasazení v [nasazení členství Role databáze pro testovací prostředí](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md).

> [!NOTE]
> Další informace o serveru projekty a databázové projekty najdete v tématu [Visual Studio 2010 SQL serveru databázové projekty](https://msdn.microsoft.com/en-us/library/ff678491.aspx).


## <a name="configure-permissions-for-the-deployment-account"></a>Konfigurace oprávnění pro účet nasazení

Pokud účet, který použijete ke spuštění nasazení není správce systému SQL Server, musíte taky vytvořit přihlašovací údaje pro tento účet. Aby bylo možné vytvořit databázi, musí být účet členem **dbcreator** role serveru nebo mít ekvivalentní oprávnění.

> [!NOTE]
> Když používáte Web Deploy nebo VSDBCMD nasazení databáze, můžete pověření systému Windows nebo pověření systému SQL Server (Pokud je vaše instance systému SQL Server je nakonfigurovaná pro podporu smíšený režim ověřování). Další postupu se předpokládá, že chcete použít přihlašovací údaje systému Windows, ale není nic zastavení můžete zadat uživatelské jméno SQL serveru a heslo v připojovacím řetězci při konfiguraci nasazení.


**Nastavit oprávnění k nasazení účtu**

1. SQL Server Management Studio otevřete jako předtím.
2. V **Průzkumník objektů** podokně klikněte pravým tlačítkem na **zabezpečení**, přejděte na příkaz **nový**a potom klikněte na **přihlášení**.
3. V **přihlášení – nové** dialogu **přihlašovací jméno** zadejte název svého účtu nasazení (například **FABRIKAM\matt**).
4. V **vybrat stránku** podokně klikněte na tlačítko **role serveru**.
5. Vyberte **dbcreator**a potom klikněte na **OK**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image14.png)

Pro podporu dalších nasazení, budete také potřebovat nasazení účet, který chcete přidat **db\_vlastníka** role na databázi po prvním nasazení. Je to proto, že na následné nasazení můžete jste úpravách schématu stávající databáze, místo vytvoření nové databáze. Jak je popsáno v předchozí části, nemůžete přidat uživatele do role databáze dokud jste vytvořili databázi, zřejmé důvodů.

**Pro mapování nasazení účet přihlášení k databázi\_roli vlastníka databáze**

1. SQL Server Management Studio otevřete jako předtím.
2. V **Průzkumník objektů** okno, rozbalte **zabezpečení** uzlu, rozbalte **přihlášení** uzel a potom dvakrát klikněte na přihlášení k účtu počítače (například  **FABRIKAM\matt**).
3. V **vlastnosti přihlášení** dialogové okno, klikněte na tlačítko **mapování uživatelských**.
4. V **mapovat uživatele na toto přihlášení** tabulky, vyberte název databáze (například **ContactManager**).
5. V **databáze členství role pro:** *[název databáze]* seznamu, vyberte **db\_vlastníka** role.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image15.png)
6. Click **OK**.

## <a name="conclusion"></a>Závěr

Databázový server by teď měly být připravené přijmout vzdálenou databázi nasazení a povolit vzdálené webové servery služby IIS pro přístup k vaší databáze. Před dalším pokusem o nasazení a používání databáze, můžete zkontrolovat tyto klíčové body:

- Nakonfigurovali jste systému SQL Server tak, aby přijímal vzdálená připojení TCP/IP?
- Nakonfigurovali jste žádné brány firewall pro povolení komunikace systému SQL Server?
- Vytvořili jste přihlášení účtu počítače pro každého webového serveru, který bude přistupovat k systému SQL Server?
- Nezahrnuje nasazení databáze skript pro vytvoření uživatelské role mapování, nebo potřebujete vytvořit ručně po prvním nasazení databáze?
- Jste vytvořili přihlašovací jméno pro účet nasazení a přidat jej do **dbcreator** roli serveru?

## <a name="further-reading"></a>Další čtení

Pokyny k nasazení databázové projekty najdete v tématu [nasazení databázové projekty](../web-deployment-in-the-enterprise/deploying-database-projects.md). Pokyny k vytvoření databáze členství v rolích spuštěním skriptu po nasazení najdete v tématu [nasazení členství Role databáze pro testovací prostředí](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Pokyny k nápravě jedinečného nasazení, které představují členství databáze najdete v tématu [nasazení databáze členství v podnikových prostředích](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

>[!div class="step-by-step"]
[Předchozí](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
[další](creating-a-server-farm-with-the-web-farm-framework.md)
