---
title: "Visual Studio Tools pro Docker základní technologie ASP.NET"
description: "Tento článek vás provede pomocí nástrojů Visual Studio 2017 a Docker pro systém Windows, které containerize aplikace ASP.NET Core."
keywords: Kontejner Docker,ASP.NET Core, Visual Studio
author: spboyer
ms.author: scaddie
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.prod: asp.net-core
ms.technology: aspnet
ms.assetid: 1f3b9a68-4dea-4b60-8cb3-f46164eedbbf
uid: publishing/vs-tools-for-docker
ms.openlocfilehash: bc436e2c02b05475b84cf9f8bdedf9463a673c4a
ms.sourcegitcommit: b861bab71ea6945f673c62223ae2cba3aa74cb6b
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/28/2017
---
# <a name="visual-studio-tools-for-docker"></a>Visual Studio Tools pro Docker

[Microsoft Visual Studio 2017](https://www.visualstudio.com/) s [Docker pro systém Windows](https://docs.docker.com/docker-for-windows/install/) podporuje vytváření, ladění a spouštění rozhraní .NET Framework a .NET Core web a konzoly aplikací pomocí kontejnery Windows a Linux.

## <a name="prerequisites"></a>Požadavky

- [Microsoft Visual Studio 2017](https://www.visualstudio.com/) zatížení .NET Core
- [Docker pro Windows](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a>Instalace a nastavení

Nainstalujte [Microsoft Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) se zatížením, .NET Core.

Pro instalaci Docker, přečtěte si informace v [Docker pro Windows: co potřebujete vědět před instalací](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) a nainstalujte [Docker pro systém Windows](https://docs.docker.com/docker-for-windows/install/).

Požadovaná konfigurace je na instalaci  **[sdílené disky](https://docs.docker.com/docker-for-windows/#shared-drives)**  v Docker pro systém Windows. Nastavení je povinné pro svazek mapování a ladění podpory.

Klikněte pravým tlačítkem na ikonu Docker na hlavním panelu systému, klikněte na tlačítko **nastavení**a vyberte **sdílené disky**. Vyberte jednotku, kde bude Docker ukládat soubory a změny.

![Sdílené disky](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

## <a name="create-an-aspnet-web-application-and-add-docker-support"></a>Vytvoření webové aplikace ASP.NET a přidání podpory Docker

Pomocí sady Visual Studio vytvořte novou webovou aplikaci ASP.NET Core. Pokud aplikace je načtena, vyberte buď **přidat podporu Docker** z **nabídky projektu** nebo klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **přidat**  >  **Docker podporu**.

*Nabídky projektu*

![Projekt přidat podporu Docker](./visual-studio-tools-for-docker/_static/project-add-docker-support.png)

*Místní nabídky projektu*

![Pravým tlačítkem klikněte na Přidat podporu Docker](./visual-studio-tools-for-docker/_static/right-click-add-docker-support.png)

Když přidáte podporu Docker do projektu, můžete Windows nebo Linux kontejnerů. (Hostitelů Docker musí používat stejný typ kontejneru. Pokud je třeba změnit typ kontejneru v běžící instance Docker, klikněte pravým tlačítkem **Docker** ikonu na hlavním panelu a vyberte **přepnout do kontejnerů Windows** nebo **přepnout do systému Linux kontejnery**.) 

Následující soubory budou přidány do projektu:

- **Soubor Docker**: soubor Docker pro aplikace ASP.NET Core vychází [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) bitové kopie. Tento image obsahuje balíčků ASP.NET Core NuGet, které byly před jitted vylepšení výkonu při spuštění. Při vytváření aplikace konzoly .NET Core, soubor Docker ze bude odkazovat poslední [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet) bitové kopie.   
- **docker-compose.yml**: základního souboru Docker Compose používá k definování kolekce obrázků být vytvořené a spustit s docker-vytvořit sestavení a spuštění.   
- **docker compose.dev.debug.yml**: Další docker compose soubor s iterativní změny konfiguraci nastavena na ladění. Visual Studio bude volat -f docker-compose.yml - f docker-compose.dev.debug.yml na sloučení těchto společně. Tento soubor compose je používán vývojářské nástroje Visual Studio.   
- **docker compose.dev.release.yml**: další soubor Docker Compose k ladění vaší verze definice. Zruší připojení svazku ladicího programu, se nezmění obsah produkční bitové kopie.  

*Docker-compose.yml* soubor obsahuje název obrázku, který se vytvoří při spuštění projektu. 

```
version '2'

services:
  hellodockertools:
    image:  user/hellodockertools${TAG}
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "80"
``` 

V tomto příkladu `image: user/hellodockertools${TAG}` generuje bitovou kopii `user/hellodockertools:dev` při spuštění aplikace **ladění** režimu a `user/hellodockertools:latest` v **verze** režimu v uvedeném pořadí. 

Můžete změnit `user` k vaší [úložiště Docker Hub](https://hub.docker.com/) uživatelské jméno, pokud máte v úmyslu push bitovou kopii do registru. Například `spboyer/hellodockertools`, nebo změňte adresu URL vašeho privátní registru `privateregistry.domain.com/` v závislosti na konfiguraci.

### <a name="debugging"></a>Ladění

Vyberte **Docker** z rozevíracího seznamu v panelu nástrojů a použití F5 spusťte ladění aplikace ladění. 

- *Microsoft/aspnetcore* bitové kopie je získali (pokud ještě není v mezipaměti)
- *ASPNETCORE_ENVIRONMENT* se nastaví na vývoji v kontejneru
- PORT 80 je VYSTAVEN a mapovat na dynamicky přiřazené port pro místního hostitele. Port je dáno hostitelů docker a může být dotazován s docker ps. 
- Aplikace se zkopíruje do kontejneru
- Spuštění výchozího prohlížeče s ladicím programem připojené ke kontejneru, pomocí dynamicky přiřazeného portu. 

Je výsledný obraz Docker postavené *dev* bitové kopie aplikace s *microsoft/aspnetcore* bitové kopie jako základní bitovou kopii.

**Poznámka:** bitovou kopii vývojářů je prázdný obsah vaší aplikace, jako ladění konfigurace používají připojení svazku a poskytuje iterativní prostředí. Chcete-li push bitovou kopii, použijte konfigurace verze.

```console
REPOSITORY                  TAG         IMAGE ID            CREATED         SIZE
spboyer/hellodockertools    dev         0b6e2e44b3df        4 minutes ago   268.9 MB
microsoft/aspnetcore        1.0.1       189ad4312ce7        5 days ago      268.9 MB
```

Je aplikace spuštěna pomocí kontejneru, který se zobrazí spuštěním `docker ps` příkaz.

```console
CONTAINER ID        IMAGE                          COMMAND               CREATED             STATUS              PORTS                   NAMES
3f240cf686c9        spboyer/hellodockertools:dev   "tail -f /dev/null"   4 minutes ago       Up 4 minutes        0.0.0.0:32769->80/tcp   hellodockertools_hellodockertools_1
```

### <a name="edit-and-continue"></a>Upravit a pokračovat

Změní na statické soubory nebo soubory šablon razor (*.cshtml*) se automaticky aktualizují, aniž by bylo potřeba kompilační krok. Proveďte požadovanou změnu, uložit a klepněte na aktualizace v prohlížeči zobrazit aktualizace.  

Úpravy, aby se soubory kódu vyžadují kompilování a restartování Kestrel v kontejneru. Po provedení změny, pomocí kombinace kláves CTRL + F5 provést proces a spusťte aplikaci v kontejneru. Kontejner Docker není znovu sestavit nebo zastavená; pomocí `docker ps` v příkazovém řádku, uvidíte, že původní kontejner je stále spuštěná od 10 minutami. 

```console
CONTAINER ID        IMAGE                          COMMAND               CREATED             STATUS              PORTS                   NAMES
3f240cf686c9        spboyer/hellodockertools:dev   "tail -f /dev/null"   10 minutes ago      Up 10 minutes       0.0.0.0:32769->80/tcp   hellodockertools_hellodockertools_1
```

### <a name="publishing-docker-images"></a>Publikování imagí Dockeru

Po dokončení cyklu vývoj a ladění aplikace sady Visual Studio Tools for Docker vám pomůže vytvořit bitovou kopii produkčního vaší aplikace. Změnit rozevírací nabídce ladění na **verze** a sestavení aplikace. Nástroji vytvoří bitovou kopii s `:latest` značky, které můžete posouvat privátní registru nebo úložiště Docker Hub. 

Pomocí `docker images` příkaz, zobrazí se v seznamu bitových kopií.

```console
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
spboyer/hellodockertools   latest              8184ae38ba91        5 seconds ago       278.4 MB
spboyer/hellodockertools   dev                 0b6e2e44b3df        About an hour ago   268.9 MB
microsoft/aspnetcore       1.0.1               189ad4312ce7        5 days ago          268.9 MB
```

Může být očekávání pro produkční nebo verzi obrázek, který má být menší velikost oproti **dev** obraz; však prostřednictvím mapování svazku, aplikace a ladicí program byly ve skutečnosti spuštěná z vaší místní počítače a ne do kontejneru. **Nejnovější** image má zabalí kód celá aplikace potřebné ke spuštění aplikace na hostitelském počítači, proto rozdíl je velikost kódu aplikace.
