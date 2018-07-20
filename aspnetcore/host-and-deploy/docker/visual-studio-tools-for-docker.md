---
title: Visual Studio Tools for Docker s ASP.NET Core
author: spboyer
description: Zjistěte, jak kontejnerizovat aplikace ASP.NET Core pomocí nástroje Visual Studio 2017 a Docker pro Windows.
ms.author: scaddie
ms.custom: mvc
ms.date: 07/18/2018
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: afa7b05820ba021c50d9c23804095f7edd8b71f1
ms.sourcegitcommit: ee2b26c7d08b38c908c668522554b52ab8efa221
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/19/2018
ms.locfileid: "39146881"
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a>Visual Studio Tools for Docker s ASP.NET Core

[Visual Studio 2017](https://www.visualstudio.com/) podporuje vytváření, ladění a spouštění kontejnerizovaných ASP.NET Core aplikace cílí na .NET Core. Jsou podporovány kontejnery Windows i Linuxu.

## <a name="prerequisites"></a>Požadavky

* [Visual Studio 2017](https://www.visualstudio.com/) s **vývoj pro různé platformy .NET Core** pracovního vytížení
* [Docker pro Windows](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a>Instalace a nastavení

Instalace Dockeru, přečtěte si informace na [Docker pro Windows: co potřebujete vědět před instalací](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) a nainstalujte [Docker pro Windows](https://docs.docker.com/docker-for-windows/install/).

**[Sdílené jednotky](https://docs.docker.com/docker-for-windows/#shared-drives)**  v Docker pro Windows musí být nakonfigurované pro podporu mapování svazku a ladění. Klikněte pravým tlačítkem na ikonu Dockeru na hlavním panelu, vyberte **nastavení...** a vyberte **sdílené jednotky**. Vyberte jednotku, kde Docker ukládá soubory. Vyberte **použít**.

![Sdílené jednotky](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> Visual Studio 2017 verze 15.6 a vyšší výzvu, při **sdílené jednotky** nejsou nakonfigurované.

## <a name="add-docker-support-to-an-app"></a>Do aplikace přidat podporu Dockeru

Do projektu aplikace ASP.NET Core přidat podporu Dockeru, musí projekt cílit na .NET Core. Jsou podporovány kontejnery Linux i Windows.

Při přidávání podpory Docker do projektu, zvolte Windows nebo Linuxem kontejneru. Hostitele Docker musí běžet stejný typ kontejneru. Chcete-li změnit typ kontejneru v běžící instanci Dockeru, klikněte pravým tlačítkem na ikonu Dockeru na hlavním panelu a vyberte **přepnout na kontejnery Windows...**  nebo **přepnout na kontejnery Linuxu...** .

### <a name="new-app"></a>Nová aplikace

Při vytváření nové aplikace s **webové aplikace ASP.NET Core** šablony projektu, vyberte **povolit podporu Dockeru** zaškrtávací políčko:

![Povolit podporu Dockeru zaškrtávací políčko](visual-studio-tools-for-docker/_static/enable-docker-support-check box.png)

Pokud je verze cílového rozhraní .NET Core **OS** umožňuje rozevírací seznam pro výběr typu kontejneru.

### <a name="existing-app"></a>Existující aplikace

Visual Studio Tools for Docker nepodporuje přidávání Dockeru do existujícího projektu ASP.NET Core cílí na rozhraní .NET Framework. Pro projekty ASP.NET Core, jejichž cílem je .NET Core existují dvě možnosti pro přidání podpory Dockeru pomocí nástrojů. Otevřete projekt v sadě Visual Studio a zvolte jednu z následujících možností:

* Vyberte **podporu Dockeru** z **projektu** nabídky.
* Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **přidat** > **podporu Dockeru**.

## <a name="docker-assets-overview"></a>Přehled prostředků dockeru

Přidejte Visual Studio Tools for Docker *docker-compose* projektu do řešení, který obsahuje následující soubory:

* *.dockerignore*: obsahuje seznam vzorů souborů a adresářů pro vyloučení při generování kontext sestavení.
* *docker-compose.yml*: základní [Docker Compose](https://docs.docker.com/compose/overview/) soubor se používá k definování kolekce imagí k sestavení a spuštění pomocí `docker-compose build` a `docker-compose run`v uvedeném pořadí.
* *docker-compose.override.yml*: volitelný soubor číst pomocí Docker Compose, obsahující konfiguraci přepsání pro služby. Visual Studio provede `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` sloučit tyto soubory.

A *soubor Dockerfile*, předpisu pro vytvoření finální image Dockeru, je přidán do kořenového adresáře projektu. Odkazovat na [odkaz na soubor Dockerfile](https://docs.docker.com/engine/reference/builder/) pro pochopení příkazy v rámci něj. Tento konkrétní *soubor Dockerfile* používá [vícefázových sestavení](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) obsahující čtyři různé, s názvem fáze sestavení umísťují:

[!code-dockerfile[](visual-studio-tools-for-docker/samples/HelloDockerTools/HelloDockerTools/Dockerfile?highlight=1,5,14,17)]

*Soubor Dockerfile* vychází [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) bitové kopie. Tento základní image obsahuje balíčky ASP.NET Core NuGet, které byly zpracované kompilátorem JIT předem zlepšit výkon při spouštění.

*Docker-compose.yml* soubor obsahuje název obrázku, který je vytvořen při spuštění projektu:

[!code-yaml[](visual-studio-tools-for-docker/samples/HelloDockerTools/docker-compose.yml?highlight=5)]

V předchozím příkladu `image: hellodockertools` generuje obrázek `hellodockertools:dev` při spuštění aplikace **ladění** režimu. `hellodockertools:latest` Image se vygeneruje, když aplikace běží v **vydání** režimu.

Před názvem obrázku [Docker Hubu](https://hub.docker.com/) uživatelské jméno (třeba `dockerhubusername/hellodockertools`) Pokud bitovou kopii se vloží do registru. Můžete také změnit název image přidat adresu URL privátního registru (například `privateregistry.domain.com/hellodockertools`) v závislosti na konfiguraci.

## <a name="debug"></a>Ladit

Vyberte **Docker** z ladění rozevírací seznam v panelu nástrojů a spusťte ladění aplikace. **Docker** zobrazení **výstup** okno zobrazuje probíhají následující akce:

* *Microsoft/aspnetcore* bitové kopie modulu runtime je získali (pokud ještě není v mezipaměti).
* *Microsoft/aspnetcore-build* kompilace/publikování image je získali (pokud ještě není v mezipaměti).
* *ASPNETCORE_ENVIRONMENT* proměnná prostředí je nastavená na `Development` v kontejneru.
* Port 80 je vystavena a mapují na dynamicky přidělovanou port pro místního hostitele. Port, který je určený hostitelem Dockeru a může být dotázán pomocí `docker ps` příkazu.
* Aplikace se zkopíruje do kontejneru.
* Spustí výchozí prohlížeč se ladicí program připojený ke kontejneru pomocí dynamicky přiřazeného portu.

Je výsledný image Dockeru *dev* image aplikace s využitím *microsoft/aspnetcore* bitové kopie jako základní image. Spustit `docker images` v příkaz **Konzola správce balíčků** okno (PMC). Image na počítači se zobrazí:

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                f8f9d6c923e2        About an hour ago   391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        15 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> Obrázek dev chybí obsah aplikace, jako **ladění** konfigurace používají připojení svazku pro zajištění iterativní prostředí. Chcete-li odeslat image, použijte **vydání** konfigurace.

Spustit `docker ps` příkazu v konzole PMC. Všimněte si, že je aplikace spuštěna pomocí kontejneru:

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a>Upravit a pokračovat

Změny statických souborů a zobrazeními Razor se automaticky aktualizují bez nutnosti kompilační krok. Proveďte požadovanou změnu, uložte a aktualizujte prohlížeč, chcete-li zobrazit aktualizace.

Úpravy souborů kódu vyžadují kompilace a restartování Kestrel v kontejneru. Po provedení změn, použijte `CTRL+F5` provést proces a spusťte aplikaci v rámci kontejneru. Kontejner Dockeru se znovu sestavit nebo zastavená. Spustit `docker ps` příkazu v konzole PMC. Všimněte si, že původní kontejneru je stále spuštěna od 10 minut před:

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a>Publikování Image Dockeru

Po dokončení cyklu vývoje a ladění aplikace Visual Studio Tools for Docker pomáhají při vytváření bitové kopie produkční aplikace. Změna konfigurace rozevíracího seznamu **vydání** a sestavení aplikace. Bitová kopie se vytváří nástroje *nejnovější* značky, který může doručit bez vyžádání do privátního registru nebo centra Dockeru.

Spustit `docker images` příkazu v konzole PMC zobrazíte seznam imagí:

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                4cb1fca533f0        19 seconds ago      391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        16 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> `docker images` Příkaz vrátí zprostředkující obrázků s názvy úložišť a značky identifikována jako  *\<žádný >* (které nejsou uvedené výše). Tyto nepojmenované bitové kopie jsou vytvářeny [vícefázových sestavení](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *soubor Dockerfile*. Pomáhají zvýšit efektivitu vytváření konečném obrazu&mdash;při změnách se znovu sestavit pouze nezbytné vrstvy. Pokud zprostředkující bitové kopie jsou už je nepotřebujete, odstraňte je pomocí [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) příkazu.

Může být očekávání pro produkci nebo verze image, která se má menší velikosti oproti tomu na *dev* bitové kopie. Z důvodu mapování svazku byly spuštěné ladicího programu a aplikace z místního počítače a není v rámci kontejneru. *Nejnovější* image se zabalit kód potřebné aplikace a spusťte tak aplikaci na hostitelském počítači. Delta proto je velikost kódu aplikace.

## <a name="additional-resources"></a>Další zdroje

* [Řešení potíží s Visual Studio 2017 vývoj aplikací pomocí Dockeru](/azure/vs-azure-tools-docker-troubleshooting-docker-errors)
* [Visual Studio Tools pro úložiště GitHub Dockeru](https://github.com/Microsoft/DockerTools)
