---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
title: Konfigurování serveru databáze webu nasadit publikování | Dokumentace Microsoftu
author: jrjlee
description: Toto téma popisuje, jak konfigurovat databázový server SQL Server 2008 R2 pro podporu nasazení webu a publikování. Úlohy popsané v tomto tématu jsou co...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: e7c447f9-eddf-4bbe-9f18-3326d965d093
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
msc.type: authoredcontent
ms.openlocfilehash: 0f8639efcfcd02c9fdb65ce3ed25272965be8aa8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753429"
---
<a name="configuring-a-database-server-for-web-deploy-publishing"></a>Konfigurace databázového serveru pro publikování nasazeného webu
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje, jak konfigurovat databázový server SQL Server 2008 R2 pro podporu nasazení webu a publikování.
> 
> Úlohy popsané v tomto tématu jsou společné pro každý scénář nasazení&#x2014;nezáleží, jestli vaše webové servery jsou nakonfigurovány pro použití služby vzdálený Agent nástroje nasazení webu služby IIS (Web Deploy), obslužné rutiny webu nasadit nebo offline nasazení nebo aplikace běží na jednom webovém serveru nebo v serverové farmě. Způsob nasazení databáze se může změnit podle požadavků na zabezpečení a další důležité informace. Například můžete nasadit databázi s nebo bez něj ukázková data, a můžete nasadit mapování uživatelů na role nebo ruční konfigurace po nasazení. Způsob, jakým konfigurování serveru databáze však zůstala stejná.


Není nutné instalovat žádné další produkty a nástroje do konfigurace databázového serveru pro podporu nasazení webu. Za předpokladu, že databázový server a webový server běží na různých počítačích, stačí jednoduše:

- Povolení SQL serveru na komunikaci pomocí protokolu TCP/IP.
- Povolte provoz serveru SQL Server přes všechny brány firewall.
- Zadejte účet počítače webového serveru přihlášení serveru SQL Server.
- Přihlášení k účtu počítače mapování na všechny požadované databázové role.
- Zadejte účet, který se spustí nasazení oprávnění pro tvůrce přihlášení a databáze systému SQL Server.
- Pro podporu nasazení na opakování, mapování nasazení účtu přihlášení k **db\_vlastníka** databázové role.

Postup pro každý z těchto postupů se zobrazí v tomto tématu. Úlohy a názorné postupy v tomto tématu se předpokládá, že začínáte s výchozí instanci systému SQL Server 2008 R2 a systémem Windows Server 2008 R2. Než budete pokračovat, ujistěte se, že:

- Windows Server 2008 R2 Service Pack 1 a všechny dostupné aktualizace jsou nainstalovány.
- Server není připojený k doméně.
- Server má statickou IP adresu.
- SQL Server 2008 R2 Service Pack 1 a všechny dostupné aktualizace jsou nainstalovány.

Pouze musí obsahovat instanci systému SQL Server **služby databázového stroje** role, který je automaticky zahrnut v žádné instalaci systému SQL Server. Pro usnadnění konfigurace a údržby, doporučujeme však, že složku zahrnujete **nástroje pro správu – základní** a **nástroje pro správu – základní** role serveru.

> [!NOTE]
> Další informace o připojení počítače k doméně najdete v tématu [připojení počítače k doméně a protokolování na](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Další informace o konfiguraci statických IP adres najdete v tématu [nakonfigurujte statickou IP adresu](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx). Další informace o instalaci systému SQL Server, naleznete v tématu [instalace systému SQL Server 2008 R2](https://technet.microsoft.com/library/bb500395.aspx).


## <a name="enable-remote-access-to-sql-server"></a>Povolit vzdálený přístup k systému SQL Server

SQL Server používá protokol TCP/IP pro komunikaci se vzdálenými počítači. Pokud je váš databázový server a webový server na různých počítačích, musíte:

- Konfigurace nastavení sítě systému SQL Server má povolit komunikaci přes protokol TCP/IP.
- Nakonfigurujte všechny hardware nebo software brány firewall pro povolení přenosů TCP (a v některých případech protokolu UDP (User Datagram) provozu) na portech, které používá instanci systému SQL Server.

K povolení SQL serveru ke komunikaci přes protokol TCP/IP, pomocí Správce konfigurace systému SQL Server můžete změnit konfiguraci sítě pro vaši instanci SQL serveru.

**K povolení SQL serveru na komunikaci pomocí protokolu TCP/IP**

1. Na **Start** nabídky, přejděte k **všechny programy**, klikněte na tlačítko **Microsoft SQL Server 2008 R2**, klikněte na tlačítko **konfigurační nástroje**a potom klikněte na **Správce konfigurace systému SQL Server**.
2. V podokně se stromovým zobrazením, rozbalte **síťová konfigurace systému SQL Server**a potom klikněte na tlačítko **protokoly pro MSSQLSERVER**.

   > [!NOTE]
   > Pokud jste nainstalovali více instancí systému SQL Server, zobrazí se vám <strong>protokoly pro</strong><em>[název instance]</em> položky pro každou instanci. Musíte nakonfigurovat nastavení sítě na základě instance instance.
3. V podokně podrobností klikněte pravým tlačítkem myši **TCP/IP** řádku a potom klikněte na tlačítko **povolit**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image1.png)
4. V **upozornění** dialogové okno, klikněte na tlačítko **OK**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image2.png)
5. Budete muset restartovat službu MSSQLSERVER, předtím, než se projeví nová konfigurace sítě. Uděláte to příkazového řádku, z konzoly služby, nebo z SQL Server Management Studio. V tomto postupu použijete SQL Server Management Studio.
6. Zavřete Správce konfigurace systému SQL Server.
7. Na **Start** nabídky, přejděte k **všechny programy**, klikněte na tlačítko **Microsoft SQL Server 2008 R2**a potom klikněte na tlačítko **SQL Server Management Studio**.
8. V **připojit k serveru** v dialogu **název serveru** zadejte název databázového serveru a potom klikněte na tlačítko **připojit**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image3.png)
9. V **Průzkumník objektů systému** podokně klikněte pravým tlačítkem na nadřazený uzel serveru (například **TESTDB1**) a potom klikněte na tlačítko **restartovat**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image4.png)
10. V **Microsoft SQL Server Management Studio** dialogové okno, klikněte na tlačítko **Ano**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image5.png)
11. Pokud byla restartována, zavřete SQL Server Management Studio.

Pro povolení provozu systému SQL Server přes bránu firewall, je potřeba nejdřív vědět porty, které používá vaše instance systému SQL Server. Bude to záviset na jak instance systému SQL Server byl vytvořen a nakonfigurován:

- A *výchozí instanci* systému SQL Server naslouchá (a reaguje na) žádosti na portu TCP 1433.
- A *pojmenovanou instanci* systému SQL Server naslouchá (a reaguje na) žádosti o dynamicky přidělovanou portu TCP.
- Pokud je povolena služba SQL Server Browser, můžou klienti dotazovat službu na portu UDP port 1434 a zjistěte, který port TCP pro konkrétní instanci systému SQL Server. Tato služba je často zakázána z důvodu zabezpečení.

Za předpokladu, že používáte výchozí instanci systému SQL Server, budete muset nakonfigurovat bránu firewall pro povolení provozu.

| Směr | Z portu | Port | Typ portu |
| --- | --- | --- | --- |
| Příchozí | Všechny | 1433 | TCP |
| Odchozí | 1433 | Všechny | TCP |
  

> [!NOTE]
> Technicky klientský počítač použije náhodně přidělenému portu TCP 1024 až 5000 ke komunikaci s SQL serverem a pravidla brány firewall můžete omezit odpovídajícím způsobem. Další informace o portech SQL serveru a brány firewall najdete v tématu [čísla portů TCP/IP potřebných pro komunikaci jazyka SQL přes bránu firewall](https://go.microsoft.com/?linkid=9805125) a [postupy: Konfigurace serveru tak, aby naslouchal specifickému portu TCP (SQL Server Configuration Správce)](https://msdn.microsoft.com/library/ms177440.aspx).


Ve většině prostředí systému Windows Server bude pravděpodobně nutné konfigurace brány Windows Firewall na databázovém serveru. Ve výchozím nastavení brány Windows Firewall umožňuje veškerý odchozí provoz, není-li pravidlo výslovně zakazuje. Chcete-li webový server pro přístup k databázi, musíte nakonfigurovat příchozí pravidlo, které umožní provoz TCP na číslo portu, který používá instanci systému SQL Server. Pokud používáte výchozí instanci systému SQL Server, můžete použít následující postup konfigurace tohoto pravidla.

**Konfigurace brány Windows Firewall umožňující navázat komunikaci s výchozí instanci SQL serveru**

1. Na databázovém serveru na **Start** nabídky, přejděte k **nástroje pro správu**a potom klikněte na tlačítko **brány Windows Firewall s pokročilým zabezpečením**.
2. V podokně se stromovým zobrazením, klikněte na tlačítko **příchozí pravidla**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image6.png)
3. V **akce** podokně v části **příchozí pravidla**, klikněte na tlačítko **nové pravidlo**.
4. V příchozí pravidla Průvodce vytvořením na **typ pravidla** stránce **Port**a potom klikněte na tlačítko **Další**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image7.png)
5. Na **protokol a porty** stránky, ujistěte se, že **TCP** je vybraná a **určité místní porty** zadejte **1433**a potom klikněte na **Další**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image8.png)
6. Na **akce** ponechte **povolit připojení** vybraný a klikněte na tlačítko **Další**.
7. Na **profilu** ponechte **domény** vybrané, vymažte **privátní** a **veřejné** zaškrtávacích políček a pak klikněte na tlačítko  **Další**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image9.png)
8. Na **název** stránky, zadejte pro pravidlo vhodně popisný název (například **výchozí instanci systému SQL Server – přístup k síti**) a potom klikněte na tlačítko **Dokončit**.

Další informace o konfiguraci brány Windows Firewall pro SQL Server, zejména v případě, že budete muset nestandardní nebo dynamických portů komunikují se SQL Server naleznete v tématu [postupy: Konfigurace brány Windows Firewall pro přístup k databázovému stroji](https://technet.microsoft.com/library/ms175043.aspx).

## <a name="configure-logins-and-database-permissions"></a>Konfigurovat přihlášení a databáze oprávnění

Když nasadíte webovou aplikaci do Internetové informační služby (IIS), aplikace bude spuštěna pod identitou fondu aplikací. V prostředí domény identity fondu aplikací použijte účet počítače serveru, na které se použijí pro přístup k síťovým prostředkům. Účty počítače mít podobu <em>[název domény]</em><strong>\</ strong ><em>[název počítače]</em><strong>$</strong>&#x2014;například <strong>FABRIKAM\TESTWEB1$</strong>. Povolit webové aplikace k přístup k databázi v síti, budete muset:

- Přidáte přihlašovací údaje pro účet počítače webového serveru do instance systému SQL Server.
- Mapování přihlášení k účtu počítače k žádné roli požadovaná databáze (obvykle **db\_datareader** a **db\_datawriter nesmí být**).

Pokud vaše webová aplikace běží na serverové farmy, nikoli na jeden server, musíte opakovat tyto postupy pro všechny webové servery v serverové farmě.

> [!NOTE]
> Další informace o identity fondu aplikací a přístup k síťovým prostředkům, naleznete v tématu [identity fondu aplikací součásti](https://go.microsoft.com/?linkid=9805123).


Tyto úlohy v různých způsobů, jak můžete přistupovat. Chcete-li vytvořit přihlášení, můžete buď:

- Vytvořte přihlašovací jméno ručně na databázovém serveru pomocí příkazů jazyka Transact-SQL nebo SQL Server Management Studio.
- Pomocí SQL Server 2008 serverový projekt v sadě Visual Studio k vytvoření a nasazení přihlášení.

Přihlášení serveru SQL Server je objekt úrovni serveru, nikoli objekt na úrovni databáze, takže není závislá na databázi, kterou chcete nasadit. V důsledku toho v libovolném okamžiku můžete vytvořit přihlášení a nejjednodušší způsob je často vytvořit přihlašovací jméno ručně na databázovém serveru před zahájením nasazování databází. Následující postup slouží k vytvoření přihlášení v aplikaci SQL Server Management Studio.

**Jak vytvořit přihlášení systému SQL Server pro účet počítače webového serveru**

1. Na databázovém serveru na **Start** nabídky, přejděte k **všechny programy**, klikněte na tlačítko **Microsoft SQL Server 2008 R2**a potom klikněte na tlačítko **SQL Server Management Studio** .
2. V **připojit k serveru** v dialogu **název serveru** zadejte název databázového serveru a potom klikněte na tlačítko **připojit**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image10.png)
3. V **Průzkumník objektů** podokně klikněte pravým tlačítkem na **zabezpečení**, přejděte na **nový**a potom klikněte na tlačítko **přihlášení**.
4. V **přihlášení – nové** v dialogu **přihlašovací jméno** zadejte název vašeho účet počítače webového serveru (například **FABRIKAM\TESTWEB1$**).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image11.png)
5. Klikněte na tlačítko **OK**.

Databázový server je v tomto okamžiku připravené pro publikování nasazení webu. Všechna řešení, které nasadíte, ale nebudou fungovat, dokud mapování přihlášení k účtu počítače na požadované databázové role. Mapování přihlášení do databázových rolí vyžaduje hodně více si mysleli, že, jako je nelze role mapy až po vás nasadili jste databázi. Chcete-li namapovat přihlášení k účtu počítače vyžaduje databázové role, můžete buď:

- Databázové role ruční přiřazení k přihlášení, po prvním nasazení databáze.
- Použijte skript po nasazení chcete přihlášení přiřadit role databáze.

Další informace o automatizaci vytváření přihlašovacích údajů a mapování role databáze, najdete v části [nasazení členstvím v databázových rolích do testovacího prostředí](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Alternativně můžete další postup přihlášení k účtu počítače přiřadit role vyžaduje databázi ručně. Mějte na paměti, že nelze provést tento postup, dokud *po* nasadili jste databázi.

**K mapování databázové role pro přihlášení k účtu počítače webového serveru**

1. Otevřete SQL Server Management Studio stejně jako předtím.
2. V **Průzkumník objektů** podokně rozbalte **zabezpečení** uzlu, rozbalte **přihlášení** uzel a potom dvakrát klikněte na přihlášení k účtu počítače (například  **FABRIKAM\TESTWEB1$**).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image12.png)
3. V **vlastnosti přihlášení** dialogové okno, klikněte na tlačítko **u mapování uživatele**.
4. V **mapovat uživatele na toto přihlášení** tabulku, vyberte název databáze (například **ContactManager**).
5. V **databáze členství v roli pro:** *[název databáze]* vyberte oprávnění nutná. V případě ukázkové řešení Správce kontaktů, je nutné vybrat **db\_datareader** a **db\_datawriter nesmí být** role.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image13.png)
6. Klikněte na tlačítko **OK**.

Ruční mapování databázových rolí je často více než odpovídající pro testovací prostředí, je méně vhodné pro automatizované nebo jedním kliknutím nasazení pro pracovní nebo produkční prostředí. Další informace o automatizaci tento druh úloh pomocí skriptů po nasazení v [nasazení členstvím v databázových rolích do testovacího prostředí](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md).

> [!NOTE]
> Další informace o projekty serveru a databázových projektů, naleznete v tématu [Visual Studio 2010 SQL Server databázové projekty](https://msdn.microsoft.com/library/ff678491.aspx).


## <a name="configure-permissions-for-the-deployment-account"></a>Konfigurace oprávnění pro účet nasazení

Pokud účet, který použijete ke spuštění nasazení není správcem SQL serveru, musíte také vytvořit přihlašovací údaje pro tento účet. Chcete-li vytvořit databázi, musí být účet členem skupiny **dbcreator** role serveru nebo mít ekvivalentní oprávnění.

> [!NOTE]
> Pokud použijete nasazení webu nebo VSDBCMD nasadit databázi, můžete přihlašovací údaje Windows nebo přihlašovací údaje SQL serveru (Pokud je vaše instance systému SQL Server je nakonfigurovaný pro podporu ověřování ve smíšeném režimu). Následující postup předpokládá, že chcete použít přihlašovací údaje Windows, ale není nic vám zastavení ze systému SQL Server uživatelské jméno a heslo v připojovacím řetězci při konfiguraci nasazení.


**Nastavení oprávnění pro účet nasazení**

1. Otevřete SQL Server Management Studio stejně jako předtím.
2. V **Průzkumník objektů** podokně klikněte pravým tlačítkem na **zabezpečení**, přejděte na **nový**a potom klikněte na tlačítko **přihlášení**.
3. V **přihlášení – nové** v dialogu **přihlašovací jméno** zadejte název účtu nasazení (například **FABRIKAM\matt**).
4. V **vybrat stránku** podokně klikněte na tlačítko **role serveru**.
5. Vyberte **dbcreator**a potom klikněte na tlačítko **OK**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image14.png)

Pro podporu dalších nasazení, bude také potřeba přidat nasazení účtu, který má **db\_vlastníka** role pro databázi po prvním nasazení. Je to proto, že na následné nasazení můžete už upravit schéma existující databázi, místo vytvoření nové databáze. Jak je popsáno v předchozí části, nelze přidat uživatele do role databáze dokud jste vytvořili databázi, zřejmé z důvodů.

**K mapování nasazení účtu přihlášení k databázi\_vlastník databázové role**

1. Otevřete SQL Server Management Studio stejně jako předtím.
2. V **Průzkumník objektů** okna, rozbalte **zabezpečení** uzlu, rozbalte **přihlášení** uzel a potom dvakrát klikněte na přihlášení k účtu počítače (například  **FABRIKAM\matt**).
3. V **vlastnosti přihlášení** dialogové okno, klikněte na tlačítko **u mapování uživatele**.
4. V **mapovat uživatele na toto přihlášení** tabulku, vyberte název databáze (například **ContactManager**).
5. V **databáze členství v roli pro:** *[název databáze]* seznamu, vyberte **db\_vlastníka** role.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image15.png)
6. Klikněte na tlačítko **OK**.

## <a name="conclusion"></a>Závěr

Databázový server by měl být nyní připraven k potvrzení nasazení vzdálené databáze a umožňuje vzdálené webové servery služby IIS pro přístup k vaší databáze. Před pokusem o nasazení a používání databáze, můžete chtít zkontrolovat tyto klíčové body:

- Nakonfigurovali jste systému SQL Server tak, aby přijímal vzdálená připojení TCP/IP?
- Nakonfigurovali jste všechny brány firewall tak, aby povolovala provoz serveru SQL Server?
- Vytvořili jste přihlášení k účtu počítače pro každého webového serveru, který bude mít přístup k systému SQL Server?
- Nasazení databáze obsahuje skript k vytvoření mapování uživatelů na role, nebo je třeba vytvořit ručně po prvním nasazení databáze?
- Jste vytvořili přihlašovací údaje pro účet nasazení a přidat ho do **dbcreator** roli serveru?

## <a name="further-reading"></a>Další čtení

Doprovodné materiály k nasazování databázových projektů, naleznete v tématu [nasazení databázové projekty](../web-deployment-in-the-enterprise/deploying-database-projects.md). Pokyny k vytvoření databáze členství v rolích spuštěním skriptu po nasazení najdete v tématu [nasazení členstvím v databázových rolích do testovacího prostředí](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Pokyny k nápravě jedinečného nasazení, které představují členství databáze najdete v tématu [nasazení databází členství do podnikových prostředí](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

> [!div class="step-by-step"]
> [Předchozí](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
> [další](creating-a-server-farm-with-the-web-farm-framework.md)
