---
uid: mvc/overview/deployment/docker
title: Migrace aplikací ASP.NET MVC do kontejnerů s Windows
description: Zjistěte, jak využít existující aplikaci ASP.NET MVC a spustíte ho v systému Windows kontejner Docker
keywords: Windows Containers,Docker,ASP.NET MVC
author: BillWagner
ms.author: wiwagn
ms.date: 02/01/2017
ms.topic: article
ms.prod: .net-framework
ms.technology: dotnet-mvc
ms.devlang: dotnet
ms.assetid: c9f1d52c-b4bd-4b5d-b7f9-8f9ceaf778c4
ms.openlocfilehash: 7a580c6c6236b375ea54ef4e9978fff6993d885a
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/11/2018
ms.locfileid: "29143186"
---
# <a name="migrating-aspnet-mvc-applications-to-windows-containers"></a>Migrace aplikací ASP.NET MVC do kontejnerů s Windows

Spuštění existující aplikace založené na rozhraní .NET Framework v kontejneru Windows navíc nevyžaduje žádné změny do vaší aplikace. Ke spouštění vaší aplikace v kontejneru systému Windows vytvořit bitovou kopii Docker obsahující vaší aplikace a spusťte kontejner. Toto téma vysvětluje, jak využít existující [aplikace ASP.NET MVC](http://www.asp.net/mvc) a nasadit ji v kontejneru systému Windows.

Začněte s existující aplikaci ASP.NET MVC a potom vytvořit publikované prostředky pomocí sady Visual Studio. Docker použijete k vytvoření bitové kopie, který obsahuje a spuštění aplikace. Budete přejděte na web spuštěn v systému Windows kontejneru a ověřte, zda že aplikace pracuje.

Tento článek předpokládá základní znalosti o Docker. Můžete další informace o Docker načtením [Docker přehled](https://docs.docker.com/engine/understanding-docker/).

Aplikace, které spustíte v kontejneru je jednoduchý web, který obsahuje odpovědi na dotazy náhodně. Tato aplikace je základní aplikace MVC bez ověřování a úložiště databáze; umožňuje zaměřit se na přesun se webová úroveň do kontejneru. Budoucí témata ukazují, jak přesunout a spravovat trvalé úložiště v kontejnerizované aplikacích.

Přesunutí aplikace zahrnuje tyto kroky:

1. [Vytvoření úlohy Publikovat k vytváření prostředků pro bitovou kopii.](#publish-script)
1. [Vytváření obrazem Docker, které se spustí aplikace.](#build-the-image)
1. [Spouštění kontejner Docker, který spouští bitové kopie.](#start-a-container)
1. [Ověření aplikace pomocí prohlížeče.](#verify-in-the-browser)

[Dokončení aplikace](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator) je na Githubu.

## <a name="prerequisites"></a>Požadavky

Vývojovém počítači musí být spuštěna.

- [Windows 10 Anniversary Update](https://www.microsoft.com/software-download/windows10/) (nebo vyšší) nebo [systému Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (nebo vyšší).
- [Docker pro systém Windows](https://docs.docker.com/docker-for-windows/) -Beta 26 verze stabilní 1.13.0 nebo 1.12 (nebo novější verze)
- [Visual Studio 2017](https://www.visualstudio.com/visual-studio-homepage-vs.aspx).

> [!IMPORTANT]
> Pokud používáte systém Windows Server 2016, postupujte podle pokynů pro [nasazení hostitele kontejneru - Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).

Po instalaci a spuštění Docker, klikněte pravým tlačítkem na ikonu na hlavním panelu a vyberte **přepnout do kontejnerů Windows**. To se vyžaduje ke spuštění Docker Image založené na systému Windows. Tento příkaz má několik sekund provést:

![Windows Container][windows-container]

## <a name="publish-script"></a>Publikování skriptu

Shromážděte všechny prostředky, které je potřeba načíst do bitové kopie Docker na jednom místě. Visual Studio můžete použít **publikovat** příkazu vytvoříte profil publikování pro vaši aplikaci. Tento profil se uveďte všechny prostředky v jedné stromu adresářů, který je zkopírovat do bitové kopie cíl později v tomto kurzu.

**Postup publikování**

1. Klikněte pravým tlačítkem na webového projektu v sadě Visual Studio a vyberte **publikovat**.
1. Klikněte **tlačítko vlastní profil**a potom vyberte **systém souborů** jako metodu.
1. Vyberte adresář. Podle konvence stažené Ukázka používá `bin\Release\PublishOutput`.

![Publikování připojení][publish-connection]

Otevřete **možností publikování souboru** části **nastavení** kartě. Vyberte **Precompile během publikování**. Tato optimalizace znamená, že budete kompilování zobrazení v kontejner Docker, kopírujete předkompilovaných zobrazení.

![Nastavení publikování][publish-settings]

Klikněte na tlačítko **publikovat**, a Visual Studio zkopíruje všechny potřebné prostředky do cílové složky.

## <a name="build-the-image"></a>Vytvořit bitovou kopii

Definujte Docker image v soubor Docker. Soubor Docker obsahuje pokyny pro základní bitovou kopii, další součásti, aplikace, kterou chcete spustit a ostatní konfigurace Image.  Soubor Docker je vstup `docker build` příkaz, který vytvoří bitovou kopii.

Vytvoříte bitovou kopii na základě `microsoft/aspnet` image uložená na [úložiště Docker Hub](https://hub.docker.com/r/microsoft/aspnet/).
Základní image `microsoft/aspnet`, je bitová kopie systému Windows Server. Obsahuje jádro systému Windows Server, IIS a ASP.NET 4.6.2. Když spustíte tuto bitovou kopii do vašeho kontejneru, automaticky spustí službu IIS a nainstalované weby.

Soubor Docker, která vytvoří image vypadá takto:

```console
# The `FROM` instruction specifies the base image. You are
# extending the `microsoft/aspnet` image.

FROM microsoft/aspnet

# The final instruction copies the site you published earlier into the container.
COPY ./bin/Release/PublishOutput/ /inetpub/wwwroot
```

Neexistuje žádné `ENTRYPOINT` v tento soubor Docker. Nepotřebujete jeden. Při spuštění systému Windows Server se službou IIS, proces služby IIS je vstupní bod, který je nakonfigurován na spuštění v základní image aspnet.

Spusťte příkaz sestavení Docker vytvoření bitové kopie, který spouští aplikace ASP.NET. Chcete-li to provést, v adresáři projektu otevřete okno prostředí PowerShell a zadejte následující příkaz v adresáři řešení:

```console
docker build -t mvcrandomanswers .
```

Tento příkaz vytvoří novou bitovou kopii pomocí pokynů v váš soubor Docker pojmenování (-t označování) bitové kopie jako mvcrandomanswers. To může zahrnovat stahování základní bitovou kopii z [úložiště Docker Hub](http://hub.docker.com)a pak přidání aplikace do této bitové kopie.

Po dokončení tohoto příkazu můžete spustit `docker images` příkazu zobrazte informace na novou bitovou kopii:

```console
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
mvcrandomanswers              latest              86838648aab6        2 minutes ago       10.1 GB
```

ID OBRÁZKU se liší v počítači. Teď umožňuje spuštění aplikace.

## <a name="start-a-container"></a>Spusťte kontejner

Spusťte kontejner spuštěním následujícího `docker run` příkaz:

```console
docker run -d --name randomanswers mvcrandomanswers
```

`-d` Argument určuje Docker zahájíte bitovou kopii v odpojeném režimu. To znamená, že bitovou kopii Docker spouští odpojené z aktuální prostředí.

V mnoha docker příklady může se zobrazit -p mapování portů kontejneru a hostitele. Výchozí image aspnet již nakonfigurována kontejneru zveřejnění a naslouchat na portu 80. 

`--name randomanswers` Poskytuje název ke kontejneru spuštěné. Tento název může používat místo ID kontejneru v většina příkazů.

`mvcrandomanswers` Je název bitovou kopii k spouštění.

## <a name="verify-in-the-browser"></a>Ověřte v prohlížeči

> [!NOTE]
> V aktuální verzi Windows kontejner nelze vyhledat `http://localhost`.
> Toto je známé chování v WinNAT a bude v budoucnu řešit. Dokud, která je určena, budete muset použít IP adresu kontejneru.

Jakmile se spustí kontejneru, najděte jeho IP adresu, můžete připojit k vaší spuštěné kontejneru z prohlížeče:

```console
docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" randomanswers
172.31.194.61
```

Připojení ke kontejneru spuštěná pomocí adresu IPv4 `http://172.31.194.61` ukazuje příklad. Zadejte tuto adresu URL do prohlížeče a měli byste vidět web spuštěný.

> [!NOTE]
> Navigace do vaší lokality mohou zabránit určitý software VPN nebo proxy server.
> Můžete ji a ujistěte se, že vaše kontejneru funguje dočasně zakázat.

Obsahuje ukázkové adresáře na Githubu [skript prostředí PowerShell](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator/run.ps1) který pro vás provede tyto příkazy. Otevřete okno prostředí PowerShell, změňte adresář na adresář řešení a typ:

```console
./run.ps1
```

Výše uvedeného příkazu vytvoří bitovou kopii, zobrazí se seznam obrázků na váš počítač, spustí kontejner a zobrazí IP adresu pro tento kontejner.

Pokud chcete zastavit vašeho kontejneru, vydávání `docker
stop` příkaz:

```console
docker stop randomanswers
```

Pokud chcete odebrat kontejner, vydávání `docker rm` příkaz:

```console
docker rm randomanswers
```

[windows-container]: media/aspnetmvc/SwitchContainer.png "Přepněte do kontejneru systému Windows"
[publish-connection]: media/aspnetmvc/PublishConnection.png "Publikovat do systému souborů"
[publish-settings]: media/aspnetmvc/PublishSettings.png "Nastavení publikování"
