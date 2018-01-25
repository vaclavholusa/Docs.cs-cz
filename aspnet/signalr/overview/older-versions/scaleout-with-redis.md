---
uid: signalr/overview/older-versions/scaleout-with-redis
title: "Škálování SignalR s Redisem (SignalR 1.x) | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 6abecf80-8ffa-41ba-b0d9-1d9edbe7687b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 8376c6537d693841a621158358cc8f69cda0a1d6
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="signalr-scaleout-with-redis-signalr-1x"></a>Škálování SignalR s Redisem (SignalR 1.x)
====================
podle [Karel Wasson](https://github.com/MikeWasson), [Patrik Fletcher](https://github.com/pfletcher)

V tomto kurzu budete používat [Redis](http://redis.io/) distribuci zprávy přes SignalR aplikace, který je nasazen na dvě samostatné instance služby IIS.

Redis je úložišti klíč hodnota v paměti. Mimoto podporuje i systém zasílání zpráv s modelem publikování a přihlášení k odběru. SignalR Redis propojovacího rozhraní používá protokol pub nebo sub funkci k předávání zpráv na jiné servery.

![](scaleout-with-redis/_static/image1.png)

V tomto kurzu budete používat tři servery:

- Dva servery se systémem Windows, který použijete k nasazení aplikace SignalR.
- Jeden server se systémem Linux, který použijete ke spuštění Redis. Snímky obrazovky v tomto kurzu byl použit Ubuntu 12.04 TLS.

Pokud nemáte tří fyzických serverů používat, můžete vytvořit virtuální počítače v Hyper-V. Další možností je vytvořit virtuální počítače na platformě Azure.

I když tento kurz používá oficiální implementace Redis, k dispozici je také [Windows port Redis](https://github.com/MSOpenTech/redis) z MSOpenTech. Instalace a konfigurace se liší, ale jinak kroky jsou stejné.

> [!NOTE] 
> 
> Škálování SignalR s Redis nepodporuje clustery Redis.


## <a name="overview"></a>Přehled

Předtím, než se nám získat podrobný kurz, zde je rychlý přehled toho, co jste se dělat.

1. Instalace Redis a spuštění serveru Redis.
2. Přidejte tyto balíčky NuGet do vaší aplikace: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.Redis](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. Vytvoření aplikace SignalR.
4. Přidejte následující kód do souboru Global.asax konfigurace propojovacího rozhraní: 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a>Ubuntu na technologie Hyper-V

Pomocí Windows Hyper-V, můžete snadno vytvořit virtuálního počítače s Ubuntu v systému Windows Server.

Stáhnout soubor ISO Ubuntu od [http://www.ubuntu.com](http://www.ubuntu.com/).

Technologie Hyper-V přidejte nový virtuální počítač. V **připojit virtuální pevný Disk** krok, vyberte **vytvořit virtuální pevný disk**.

![](scaleout-with-redis/_static/image2.png)

V **možnosti instalace** krok, vyberte **soubor bitové kopie (.iso)**, klikněte na tlačítko **Procházet**a přejděte do instalace Ubuntu ISO.

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a>Nainstalujte Redis

Postupujte podle kroků v [http://redis.io/download](http://redis.io/download) ke stažení a sestavení Redis.

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

Toto sestavení binárních souborů Redis `src` adresáře.

Ve výchozím nastavení Redis nevyžaduje heslo. Chcete-li nastavit heslo, upravte `redis.conf` souboru, který se nachází v kořenovém adresáři zdrojového kódu. (Před zahájením úprav, vytvořili záložní kopii souboru!) Přidejte následující direktivu pro `redis.conf`:

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

Nyní spusťte serveru Redis:

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

Otevřete port 6379, což je výchozí port, který Redis naslouchá. (Můžete změnit číslo portu v konfiguračním souboru.)

## <a name="create-the-signalr-application"></a>Vytvoření aplikace nástroje SignalR

Vytvoření aplikace SignalR podle buď tyto kurzy:

- [Začínáme s SignalR](../getting-started/tutorial-getting-started-with-signalr.md)
- [Začínáme s SignalR a MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md)

V dalším kroku upravíme chatovací aplikace pro podporu škálování s Redis. Nejprve přidejte balíček SignalR.Redis NuGet do projektu. V sadě Visual Studio z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**, pak vyberte **Konzola správce balíčků**. V okně konzoly Správce balíčků zadejte následující příkaz:

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

Dále otevřete soubor Global.asax. Přidejte následující kód, který **aplikace\_spustit** metoda:

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- "server" je název serveru, který běží Redis.
- *port* je číslo portu
- "password" je heslo, které jste zadali v souboru redis.conf.
- "AppName" je libovolný řetězec. SignalR vytvoří kanál pub nebo sub Redis s tímto názvem.

Příklad:

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a>Nasazení a spuštění aplikace

Připravte vaší instance serveru Windows k nasazení aplikace SignalR.

Přidejte roli služby IIS. Obsahují funkce "Vývoj aplikací", včetně protokol WebSocket.

![](scaleout-with-redis/_static/image5.png)

Zahrnují taky službu správy (uvedené v části "Nástroje pro správu").

![](scaleout-with-redis/_static/image6.png)

**Instalaci Web Deploy 3.0.** Při spuštění Správce služby IIS, vás vyzve k instalaci webové platformy společnosti Microsoft nebo můžete [stáhnout intstaller](https://go.microsoft.com/fwlink/?LinkId=255386). V instalačním programu platformy vyhledávání pro nasazení webu a nainstalovat nasazení webu 3.0

![](scaleout-with-redis/_static/image7.png)

Zkontrolujte, zda je spuštěna služba webové správy. V opačném případě spusťte službu. (Pokud nevidíte v seznamu služeb systému Windows služby webové správy, ujistěte se, že jste nainstalovali službu správy při přidání IIS role.)

Ve výchozím nastavení služba webové správy naslouchá na portu TCP 8172. V bráně Windows Firewall vytvořte nové příchozí pravidlo umožňující přenos TCP na portu 8172. Další informace najdete v tématu [konfigurace pravidel brány Firewall](https://technet.microsoft.com/library/dd448559(WS.10).aspx). (Pokud jsou hostiteli virtuálních počítačů v Azure, musíte přímo na portálu Azure. V tématu [jak nastavit koncové body k virtuálnímu počítači](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)

Nyní jste připraveni k nasazení projektu sady Visual Studio v počítači pro vývoj na server. V Průzkumníku řešení klikněte pravým tlačítkem na řešení a klikněte na tlačítko **publikovat**.

Podrobnější dokumentaci k nasazení webu, najdete v článku [webové mapa obsahu nasazení pro Visual Studio a ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).

Pokud nasadíte aplikaci do dvou serverů, můžete otevřít každou instanci v samostatném okně prohlížeče a zjistit, každá z jiných přijímat zprávy SignalR. (Samozřejmě v produkčním prostředí, oba servery by nacházejí za službou Vyrovnávání zatížení.)

![](scaleout-with-redis/_static/image8.png)

Pokud jste zvědaví zobrazíte zprávy, které se odesílají do Redis, můžete použít **rozhraní příkazového řádku redis** klienta, který se instaluje s Redis.

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
