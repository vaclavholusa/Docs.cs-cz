---
uid: signalr/overview/performance/scaleout-with-sql-server
title: "Škálování SignalR s SQL serverem | Microsoft Docs"
author: MikeWasson
description: "Verze softwaru použitým v tomto tématu Visual Studio 2013 .NET 4.5 SignalR verze 2 předchozí verze tohoto tématu informace o předchozích verzí aplikace..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 5bf625a1ef8cc8ceab0014fadfab0c8a23dbc8da
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="signalr-scaleout-with-sql-server"></a>Škálování SignalR s SQL serverem
====================
podle [Karel Wasson](https://github.com/MikeWasson), [Patrik Fletcher](https://github.com/pfletcher)

> ## <a name="software-versions-used-in-this-topic"></a>Verze softwaru použitým v tomto tématu
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR verze 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Předchozí verze tohoto tématu
> 
> Informace o předchozích verzích SignalR najdete v tématu [starší verze funkce SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Dotazy a připomínky
> 
> Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky. Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com/).


V tomto kurzu použijete SQL Server k distribuci zpráv mezi aplikací SignalR, která je nasazena v dvě samostatné instance služby IIS. V tomto kurzu můžete spustit také na jeden testovací počítač, ale pokud chcete získat plný vliv, je potřeba nasadit aplikaci SignalR na dva nebo více serverů. Musíte také nainstalovat SQL Server na jednom ze serverů nebo na samostatný vyhrazený server. Další možností je spustit kurz pomocí virtuálních počítačů v Azure.

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a>Požadavky

Microsoft SQL Server 2005 nebo novější. Propojovacího rozhraní podporuje stolní počítače a servery edice systému SQL Server. Nepodporuje se SQL Server Compact Edition nebo Azure SQL Database. (Pokud je vaše aplikace hostována v Azure, zvažte propojovacího rozhraní služby Service Bus místo.)

## <a name="overview"></a>Přehled

Předtím, než se nám získat podrobný kurz, zde je rychlý přehled toho, co jste se dělat.

1. Vytvoření nové prázdné databáze. Propojovacího rozhraní vytvoří nezbytné tabulky v této databázi.
2. Přidejte tyto balíčky NuGet do vaší aplikace: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.SqlServer](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. Vytvoření aplikace SignalR.
4. Přidejte následující kód do souboru Startup.cs konfigurace propojovacího rozhraní: 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

 Tento kód konfiguruje propojovacího rozhraní s výchozími hodnotami u [TableCount](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) a [MaxQueueLength](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx). Informace o změně těchto hodnot najdete v tématu [výkonu SignalR: metriky škálování](signalr-performance.md#scaleout_metrics). 

## <a name="configure-the-database"></a>Konfigurace databáze

Rozhodněte, jestli aplikaci pomocí ověřování systému Windows nebo ověřování serveru SQL Server pro přístup k databázi. Bez ohledu na to Ujistěte se, že databáze uživatel má oprávnění k přihlášení, vytvořte schémata a vytvářet tabulky.

Vytvořte novou databázi pro propojovacího rozhraní používat. Databázi můžete přiřadit libovolný název. Nemusíte vytvářet všechny tabulky v databázi. propojovacího rozhraní vytvoří nezbytné tabulky.

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a>Povolte službu Service Broker

Doporučujeme povolit služby Service Broker pro databázi propojovacího rozhraní. Service Broker poskytuje nativní podporu pro zasílání zpráv a služby Řízení front v systému SQL Server, který umožňuje získat aktualizace efektivněji propojovacího rozhraní. (Však propojovacího rozhraní také funguje bez služby Service Broker.)

Chcete-li zkontrolovat, zda je povoleno služby Service Broker, dotaz **je\_zprostředkovatele\_povoleno** sloupec v **zobrazení sys.databases** katalogu zobrazení.

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

Pokud chcete povolit služby Service Broker, pomocí následujícího dotazu SQL:

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> Pokud tento dotaz zobrazí zablokování, ujistěte se, nejsou žádné aplikace, připojení k databázi.


Pokud jste povolili trasování, trasování také zobrazit, zda je povoleno služby Service Broker.

## <a name="create-a-signalr-application"></a>Vytvoření aplikace SignalR

Vytvoření aplikace SignalR podle buď tyto kurzy:

- [Začínáme s SignalR 2.0](../getting-started/tutorial-getting-started-with-signalr.md)
- [Začínáme s SignalR 2.0 a MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

V dalším kroku upravíme chatovací aplikace pro podporu škálování s SQL serverem. Nejprve přidejte balíček SignalR.SqlServer NuGet do projektu. V sadě Visual Studio z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**, pak vyberte **Konzola správce balíčků**. V okně konzoly Správce balíčků zadejte následující příkaz:

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

Dále otevřete soubor Startup.cs. Přidejte následující kód, který **konfigurace** metoda:

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a>Nasazení a spuštění aplikace

Připravte vaší instance serveru Windows k nasazení aplikace SignalR.

Přidejte roli služby IIS. Obsahují funkce "Vývoj aplikací", včetně protokol WebSocket.

![](scaleout-with-sql-server/_static/image4.png)

Zahrnují taky službu správy (uvedené v části "Nástroje pro správu").

![](scaleout-with-sql-server/_static/image5.png)

**Instalaci Web Deploy 3.0.** Při spuštění Správce služby IIS, vás vyzve k instalaci webové platformy společnosti Microsoft nebo můžete [stáhnout intstaller](https://go.microsoft.com/fwlink/?LinkId=255386). V instalačním programu platformy vyhledávání pro nasazení webu a nainstalovat nasazení webu 3.0

![](scaleout-with-sql-server/_static/image6.png)

Zkontrolujte, zda je spuštěna služba webové správy. V opačném případě spusťte službu. (Pokud nevidíte v seznamu služeb systému Windows služby webové správy, ujistěte se, že jste nainstalovali službu správy při přidání IIS role.)

Nakonec otevřete port 8172 pro protokol TCP. Toto je port, který používá nástroj pro nasazení webu.

Nyní jste připraveni k nasazení projektu sady Visual Studio v počítači pro vývoj na server. V Průzkumníku řešení klikněte pravým tlačítkem na řešení a klikněte na tlačítko **publikovat**.

Podrobnější dokumentaci k nasazení webu, najdete v článku [webové mapa obsahu nasazení pro Visual Studio a ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).

Pokud nasadíte aplikaci do dvou serverů, můžete otevřít každou instanci v samostatném okně prohlížeče a zjistit, každá z jiných přijímat zprávy SignalR. (Samozřejmě v produkčním prostředí, oba servery by nacházejí za službou Vyrovnávání zatížení.)

![](scaleout-with-sql-server/_static/image7.png)

Po spuštění aplikace, můžete zjistit, že SignalR vytvořil automaticky tabulky v databázi:

![](scaleout-with-sql-server/_static/image8.png)

SignalR spravuje tabulky. Tak dlouho, dokud vaše aplikace je nasazená, nemáte odstranit řádky, úprava v tabulce a tak dále.
