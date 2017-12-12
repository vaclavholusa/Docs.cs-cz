---
title: "Sestavení projektů s Yeoman v ASP.NET Core"
author: spboyer
description: "Tento článek vás provede vytváření webové aplikace pomocí Yeoman ASP.NET Core generátor v systému macOS."
keywords: "ASP.NET Core, Yeoman, křížové platformy yo aspnet"
ms.author: spboyer
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.assetid: fda0c2a8-1743-4505-be1a-7f8ceeef8647
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/yeoman
ms.openlocfilehash: d7411c1635e9fef2857f9a03e7310224ee8d7344
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-building-projects-with-yeoman-in-aspnet-core"></a>Úvod do vytváření projektů s Yeoman v ASP.NET Core

[Yeoman](http://yeoman.io/) je systém generování uživatelského rozhraní projektu pro vytváření mnoho typů aplikací. Yeoman generátor pro ASP.NET Core obsahuje různé šablony projektů pro spuštění nové webové, MVC nebo konzolové aplikace.

## <a name="install-nodejs-npm-and-yeoman"></a>Nainstalovat Node.js, npm a Yeoman

### <a name="prerequisites"></a>Požadavky

Jsou vyžadovány pro Yeoman Node.js a npm. Stáhnout z [Node.js](https://nodejs.org/). Instalační program obsahuje [Node.js](https://nodejs.org/) a [npm](https://www.npmjs.com/). Také je vyžadována pro instalaci architektury uživatelského rozhraní, jako je Bootstrap bower.

Chcete-li nainstalovat Yeoman a Bower, spusťte následující příkaz:

```console
npm install -g yo bower
```

>[!Note]
>Pokud dojde k chybě `npm ERR! Please try running this command again as root/Administrator.` v systému macOS, spusťte následující příkaz pomocí [sudo](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/sudo.8.html):`sudo npm install -g yo bower`

Z příkazového řádku nainstalujte generátor ASP.NET:

```console
npm install -g generator-aspnet
```

> [!NOTE]
> Pokud dojde k chybě oprávnění, spusťte příkaz pod `sudo` jak bylo popsáno výše.

`–g` Příznak nainstaluje generátor globálně, takže lze z cesty.

## <a name="create-an-aspnet-app"></a>Vytvoření aplikace ASP.NET

Spusťte generátor na základě Yeoman ASP.NET:

```console
yo aspnet
```

Generátor zobrazí nabídku. Šipka dolů **webové aplikace základní [bez členství a autorizace]** projektu a klepněte na **Enter**:

![Příkazové okno: Jaký typ aplikace chcete vytvořit? Nabídky typy aplikací](yeoman/_static/yeoman-yo-aspnet.png)

Vyberte Bootstrap jako rozhraní uživatelského rozhraní a klepněte na **Enter**.

Použití "**MyWebApp**" pro název aplikace a potom klepněte na **Enter**.

Yeoman bude vygenerovat projektu a jeho podpůrné soubory. Navrhované další kroky jsou také uvedeny ve formě příkazy.

![Příkazové okno: co je název vaší aplikace ASP.NET? Příkazový řádek](yeoman/_static/yeoman-yo-aspnet-created.png)

[ASP.NET generátor](https://www.npmjs.com/package/generator-aspnet) vytvoří ASP.NET Core projekty, které mohou být načtena do Visual Studio Code, Visual Studio, nebo spusťte z příkazového řádku.

## <a name="restore-build-and-run"></a>Obnovení, vytvoření a spuštění

Postupujte podle navrhované příkazy změnou do adresáře `MyWebApp` adresáře. Spusťte `dotnet restore`.

![Příkazové okno](yeoman/_static/dotnet-restore.png)

Sestavení a spuštění aplikace pomocí `dotnet build` a `dotnet run`:

![Příkazové okno](yeoman/_static/dotnet-build-run.png)

V tomto okamžiku můžete přejít na adresu URL v k testování aplikace ASP.NET Core nově vytvořený.

## <a name="client-side-packages"></a>Balíčky na straně klienta

Front-endové prostředky jsou poskytovány šablony z Yeoman generátor pomocí [Bower](xref:client-side/bower) Správce balíčků na straně klienta, přidání *bower.json* a *.bowerrc* soubory obnovení klientské balíčky pomocí Bower.

[BundlerMinifier](xref:client-side/bundling-and-minification) součásti jsou také obsaženy ve výchozím nastavení pro snadné zřetězení (sdružování) a minimalizace šablon stylů CSS, JavaScript a HTML.

## <a name="building-and-running-from-visual-studio"></a>Vytváření a spouštění ze sady Visual Studio

Můžete načíst generovaného webový projekt ASP.NET Core přímo do sady Visual Studio, pak sestavit a spustit projekt z ní. Postupujte podle pokynů výše a vygenerovat novou aplikaci ASP.NET Core pomocí Yeoman. Tentokrát vyberte **webové aplikace** z nabídky a název aplikace `MyWebApp`.

Otevřete Visual Studio. V nabídce Soubor vyberte otevřete ‣ projekt nebo řešení.

V dialogovém okně Otevřít projekt, přejděte na *.csproj* souboru, vyberte ho a klikněte **otevřete** tlačítko. V Průzkumníku řešení projekt by měl vypadat podobně jako na následující snímek obrazovky.

![Soubory a složky na nový projekt v Průzkumníku řešení](yeoman/_static/yeoman-solution.png)

Yeoman scaffolds webové aplikace MVC, dokončení s oběma straně serveru a klienta sestavení podpory. Serverové závislosti jsou uvedeny v seznamu **závislosti nebo NuGet** uzel a závislostí na straně klienta v **závislosti/Bower** uzlu v Průzkumníku řešení. Závislosti jsou obnoveny automaticky při načtení projektu.

![V uzlu závislosti ve stromovém zobrazení Průzkumník řešení složce Bower je otevřený, výpis jeho závislé součásti.](yeoman/_static/yeoman-loading-dependencies.png)

Když se obnoví všechny závislosti, stiskněte klávesu **F5** spustit projekt. Zobrazí výchozí domovskou stránku v prohlížeči.

![Webové aplikace, otevřete v Microsoft Edge](yeoman/_static/yeoman-home-page.png)

## <a name="restoring-building-and-hosting-from-a-command-line"></a>Obnovení, vytvoření a hostování z příkazového řádku

Můžete připravit a hostitele webové aplikace pomocí rozhraní příkazového řádku .NET Core.

Na příkazovém řádku změňte aktuální adresář na složku obsahující projekt (to znamená, složku, která obsahuje *.csproj* souborů):

```console
cd src\MyWebApp
```

Obnovte závislosti balíčků NuGet projektu:

```console
dotnet restore
```

Spusťte aplikaci:

```console
dotnet run
```

Napříč platformami [Kestrel](xref:fundamentals/servers/kestrel) webový server bude zahájeno naslouchání na portu 5000.

Otevřete webový prohlížeč a přejděte na `http://localhost:5000`.

![Webové aplikace, otevřete v Microsoft Edge](yeoman/_static/yeoman-home-page_5000.png)

## <a name="adding-to-your-project-with-sub-generators"></a>Přidání do projektu s dílčí generátory

Pomocí Yeoman [sub generátory](https://github.com/omnisharp/generator-aspnet), můžete přidat buď `nuget.config` nebo `web.config` po vytvoření projektu. Například spusťte následující příkaz z adresáře, ve kterém by vytvořit soubor:

```console
yo aspnet:nugetconfig
```

Výsledkem je NuGet konfigurační soubor s názvem `nuget.config` s následujícím obsahem:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
 <packageSources>
    <!--To inherit the global NuGet package sources remove the <clear/> line below -->
    <clear />
 </packageSources>
</configuration>
```

## <a name="additional-resources"></a>Další zdroje

* [Servery (Kestrel a WebListener)](xref:fundamentals/servers/index)
* [Základy](xref:fundamentals/index)
