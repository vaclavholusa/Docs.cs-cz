---
title: Visual Studio Tools pro Docker základní technologie ASP.NET
author: spboyer
description: Zjistěte, jak pomocí nástrojů Visual Studio 2017 a Docker pro systém Windows containerize aplikace ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/12/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: b2a3c369a22d50fcefdb96914f5bf84bfafab7cb
ms.sourcegitcommit: 6fa546140575b3eb279eabae12d9acad966f70e0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a>Visual Studio Tools pro Docker základní technologie ASP.NET

[Visual Studio 2017](https://www.visualstudio.com/) podporuje vytváření, ladění a spouštění kontejnerizované ASP.NET Core aplikace cílený na .NET Core. Kontejnery Windows a Linux jsou podporovány.

## <a name="prerequisites"></a>Požadavky

* [Visual Studio 2017](https://www.visualstudio.com/) s **vývoj pro různé platformy .NET Core** pracovního vytížení
* [Docker pro Windows](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a>Instalace a nastavení

Pro instalaci Docker, přečtěte si informace v [Docker pro Windows: co potřebujete vědět před instalací](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) a nainstalujte [Docker pro systém Windows](https://docs.docker.com/docker-for-windows/install/).

**[Sdílené disky](https://docs.docker.com/docker-for-windows/#shared-drives)**  v Docker pro systém Windows musí být nakonfigurované pro podporu mapování svazku a ladění. Klikněte pravým tlačítkem na ikonu na hlavním panelu Docker, vyberte **nastavení...** a vyberte **sdílené disky**. Vyberte jednotku, kde Docker ukládá soubory. Vyberte **použít**.

![Sdílené disky](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> Visual Studio 2017 verze 15.6 a novější výzvu, při **sdílené disky** nejsou nakonfigurována.

## <a name="add-docker-support-to-an-app"></a>Přidání podpory Docker do aplikace

Chcete-li přidat podporu Docker do projektu ASP.NET Core, musí být projektu .NET Core. Kontejnery pro Linux a Windows jsou podporovány.

Když přidáváte podporu Docker do projektu, zvolte Windows nebo Linux kontejneru. Hostitelů Docker musí používat stejný typ kontejneru. Chcete-li změnit typ kontejneru v běžící instance Docker, klikněte pravým tlačítkem na ikonu Docker na hlavním panelu a vyberte **přepnout do kontejnerů Windows...**  nebo **přepnout do kontejnerů Linux...** .

### <a name="new-app"></a>Nové aplikace

Při vytváření nové aplikace s **webové aplikace ASP.NET Core** šablony projektů, vyberte **povolení podpory Docker** políčko:

![Povolit podporu Docker zaškrtávací políčko](visual-studio-tools-for-docker/_static/enable-docker-support-checkbox.png)

Pokud je cílový framework .NET Core **OS** umožňuje rozevíracího seznamu pro výběr typu kontejneru.

### <a name="existing-app"></a>Stávající aplikace

Visual Studio Tools for Docker nepodporují, přidání Docker do existujícího projektu ASP.NET Core cílení na rozhraní .NET Framework. Pro projekty ASP.NET Core cílení na .NET Core existují dvě možnosti pro přidání podpory Docker prostřednictvím nástrojů. Otevřete projekt v sadě Visual Studio a zvolte jednu z následujících možností:

* Vyberte **Docker podporu** z **projektu** nabídky.
* Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **přidat** > **Docker podporu**.

## <a name="docker-assets-overview"></a>Přehled prostředků docker

Přidat sady Visual Studio Tools for Docker *docker-tvoří* projektu a řešení, která obsahuje následující:

* *.dockerignore*: obsahuje seznam vzorů souborů a adresářů pro vyloučení při generování sestavení kontextu.
* *docker-compose.yml*: základní [Docker Compose](https://docs.docker.com/compose/overview/) soubor používá k definování kolekce bitové kopie vytvořené a spusťte s `docker-compose build` a `docker-compose run`, v uvedeném pořadí.
* *docker compose.override.yml*: volitelný soubor, přečtěte si pomocí Docker Compose, obsahující konfiguraci přepsání pro služby. Visual Studio provede `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` na sloučení těchto souborů.

A *soubor Docker*, recepturách pro vytvoření finální image Docker, se přidá do kořenového adresáře projektu. Odkazovat na [odkaz na soubor Docker](https://docs.docker.com/engine/reference/builder/) pro pochopení příkazy v něm. Tato konkrétní *soubor Docker* používá [více fáze sestavení](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) obsahující jedinečné, čtyři fáze sestavení s názvem:

[!code-dockerfile[](visual-studio-tools-for-docker/samples/HelloDockerTools/HelloDockerTools/Dockerfile?highlight=1,5,14,17)]

*Soubor Docker* je založena na [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) bitové kopie. Tato základní image obsahuje balíčků ASP.NET Core NuGet, které byly před jitted ke zlepšení výkonu při spuštění.

*Docker-compose.yml* soubor obsahuje název obrázku, který se vytvoří při spuštění projektu:

[!code-yaml[](visual-studio-tools-for-docker/samples/HelloDockerTools/docker-compose.yml?highlight=5)]

V předchozím příkladu `image: hellodockertools` generuje bitovou kopii `hellodockertools:dev` při spuštění aplikace **ladění** režimu. `hellodockertools:latest` Bitové kopie se vygeneruje, když aplikace běží v **verze** režimu.

Zadejte před název bitové kopie [úložiště Docker Hub](https://hub.docker.com/) uživatelské jméno (například `dockerhubusername/hellodockertools`) Pokud bitovou kopii se vloží do registru. Můžete taky změnit název bitové kopie přidat URL privátní registru (například `privateregistry.domain.com/hellodockertools`) v závislosti na konfiguraci.

## <a name="debug"></a>Ladit

Vyberte **Docker** z rozevíracího seznamu v panelu nástrojů a spuštění ladění aplikace ladění. **Docker** zobrazení **výstup** v okně se zobrazí probíhají následující akce:

* *Microsoft/aspnetcore* runtime image je získali (pokud ještě není v mezipaměti).
* *Microsoft/aspnetcore sestavení* kompilace nebo publikování bitové kopie je získali (pokud ještě není v mezipaměti).
* *ASPNETCORE_ENVIRONMENT* proměnná prostředí je nastavená na `Development` v kontejneru.
* Port 80 je vystavený a mapovat na port dynamicky přiřadit pro místního hostitele. Port je dáno hostitelů Docker a lze dotazovat se `docker ps` příkaz.
* Aplikace se zkopíruje do kontejneru.
* Spuštění výchozího prohlížeče s ladicím programem připojené ke kontejneru dynamicky přiřadit port je používán. 

Je výsledný obraz Docker *dev* bitové kopie aplikace pomocí *microsoft/aspnetcore* bitové kopie jako základní bitovou kopii. Spustit `docker images` v příkazu **Konzola správce balíčků** okno (pomocí PMC). Image na počítači se zobrazí:

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                f8f9d6c923e2        About an hour ago   391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        15 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> Bitovou kopii dev chybí obsah aplikace, jako **ladění** konfigurace používají připojení svazku a poskytuje iterativní prostředí. Chcete-li push bitovou kopii, použijte **verze** konfigurace.

Spustit `docker ps` v pomocí PMC. Všimněte si, že je že aplikace spuštěná pomocí kontejneru:

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a>Upravit a pokračovat

Změny statické soubory a zobrazení syntaxe Razor se automaticky aktualizují bez nutnosti kompilační krok. Proveďte požadovanou změnu, uložte a aktualizujte stránku prohlížeče zobrazíte aktualizace.  

Úpravy, aby se soubory kódu vyžaduje kompilování a restartování Kestrel v kontejneru. Po provedení změny, použijte kombinaci kláves CTRL + F5 k provedení proces a spusťte aplikaci v kontejneru. Kontejner Docker není znovu sestavit nebo zastavená. Spustit `docker ps` v pomocí PMC. Všimněte si původní kontejner je stále spuštěná od 10 minut před:

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a>Publikování imagí Dockeru

Po dokončení cyklu vývoj a ladění aplikace sady Visual Studio Tools for Docker pomůžou při vytváření bitové kopie produkční aplikace. Změnit konfiguraci rozevírací seznam pro **verze** a sestavení aplikace. Nástroji vytvoří bitovou kopii s *nejnovější* značku, která se můžou poslat privátní registru nebo úložiště Docker Hub. 

Spustit `docker images` v pomocí PMC zobrazíte seznam bitové kopie:

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                4cb1fca533f0        19 seconds ago      391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        16 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> `docker images` Příkaz vrátí zprostředkující bitové kopie s názvy úložiště a tagy identifikovaného jako  *\<žádné >* (které nejsou uvedené výše). Tyto bitové kopie nepojmenované vznikají pomocí funkcí [více fáze sestavení](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *soubor Docker*. Jejich zlepšit efektivitu vytváření finální image&mdash;pouze nezbytné vrstvy se znovu sestavit při změnách. Když zprostředkující bitové kopie již nejsou potřebné, je odstranit pomocí [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) příkaz.

Může být očekávání pro produkční nebo verzi obrázek, který má být menší velikost oproti *dev* bitové kopie. Z důvodu mapování svazku, aplikace a ladicí program běžely z místního počítače a ne do kontejneru. *Nejnovější* image má zabalí kód potřebné aplikace a spusťte aplikaci na hostitelském počítači. Rozdíl je proto velikost kódu aplikace.
