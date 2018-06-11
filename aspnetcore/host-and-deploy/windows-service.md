---
title: Jádro ASP.NET hostitele ve službě Windows
author: guardrex
description: Zjistěte, jak hostovat aplikace ASP.NET Core ve službě Windows.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: 33493e1ff674cd81544a7d14a7fd758c1e68bf9a
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252344"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Jádro ASP.NET hostitele ve službě Windows

Podle [Luke Latham](https://github.com/guardrex) a [tní Dykstra](https://github.com/tdykstra)

Aplikace ASP.NET Core může být hostovaný v systému Windows bez použití služby IIS jako [služba systému Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Pokud je hostováno jako služby systému Windows, aplikace může automaticky spustit po restartování počítače a dojde k chybě bez nutnosti lidského zásahu.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="get-started"></a>Začínáme

Následující minimální změny jsou nezbytné pro nastavení existujícího projektu ASP.NET Core spuštění ve službě:

1. Přidat odkaz na balíček pro [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).

1. Proveďte následující změny v `Program.Main`:

   * Volání [hostitele. RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) místo `host.Run`.

   * Pokud kód volá `UseContentRoot`, cesta k aplikaci je publikována umístění Místo použití `Directory.GetCurrentDirectory()`.

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,11)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. Publikujte aplikaci do složky. Použití [dotnet publikování](/dotnet/articles/core/tools/dotnet-publish) nebo [profil publikování se Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) který publikuje do složky.

   K publikování ukázkové aplikace z příkazového řádku, spusťte následující příkaz v okně konzoly ze složky projektu:

   ```console
   dotnet publish --configuration Release --output c:\svc
   ```

1. Použití [sc.exe](https://technet.microsoft.com/library/bb490995) nástroj příkazového řádku k vytvoření služby (`sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"`). `binPath` Hodnota je cesta ke spustitelnému souboru aplikace, která zahrnuje název spustitelného souboru. **Mezera mezi znaménkem rovnosti a uvozovky, která se spouští cesta je požadovaná.**

   Pro ukázkovou aplikaci a příkaz, který následuje služba je:

   * S názvem **Moje_služba**.
   * Publikovaná *c:\\svc* složky.
   * Má aplikaci spustitelný soubor s názvem *AspNetCoreService.exe*.

   Otevřete příkazové okno s oprávněními správce a spusťte následující příkaz:

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   ```

   **Zajistěte, aby místo nachází mezi `binPath=` argument a její hodnotu.**

1. Spusťte službu s `sc start <SERVICE_NAME>` příkaz.

   Spusťte službu ukázkové aplikace, použijte následující příkaz:

   ```console
   sc start MyService
   ```

   Příkaz přijímá několik sekund, spusťte službu.

1. `sc query <SERVICE_NAME>` Příkaz lze použít ke kontrole stavu služby určit její stav:

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   Použijte následující příkaz a zkontrolujte stav služby ukázkové aplikaci:

   ```console
   sc query MyService
   ```

1. Pokud služba je v `RUNNING` stav a pokud služba je webová aplikace, vyhledejte aplikaci v jeho cestu (ve výchozím nastavení, `http://localhost:5000`, který přesměruje na `https://localhost:5001` při použití [HTTPS přesměrování Middleware](xref:security/enforcing-ssl)).

   Ukázka app service, vyhledat aplikaci v `http://localhost:5000`.

1. Zastavte službu s `sc stop <SERVICE_NAME>` příkaz.

   Následující příkaz zastaví službu ukázkové aplikaci:

   ```console
   sc stop MyService
   ```

1. Po chvíli trvat kdykoli zastavit službu, odinstalujte službu s `sc delete <SERVICE_NAME>` příkaz.

   Zkontrolujte stav služby ukázkové aplikaci:

   ```console
   sc query MyService
   ```

   Když služba ukázkové aplikace je v `STOPPED` stavu, odinstalujte službu ukázkové aplikace použijte následující příkaz:

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a>Zadejte způsob, jak spustit mimo službu

Je snazší testování a ladění při spuštění mimo službu, takže se obvykle přidávat kód, který volá `RunAsService` pouze za určitých podmínek. Například můžete spouštět aplikace jako konzolové aplikace s `--console` argument příkazového řádku nebo pokud je připojen ladicí program:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

Vzhledem k tomu, že konfigurace ASP.NET Core vyžaduje páry název hodnota pro argumenty příkazového řádku, `--console` přepínače se odebere před argumenty do [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a>Zpracování ukončení a spuštění události

Pro zpracování [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), a [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) události, proveďte následující další změny:

1. Vytvořte třídu, která je odvozena z [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. Vytvoření metody rozšíření pro [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) , předá vlastní `WebHostService` k [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. V `Program.Main`, zavolejte metodu nové rozšíření, `RunAsCustomService`, místo [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

Pokud vlastní `WebHostService` kódu vyžaduje službu z vkládání závislostí (například protokolovač), získat ze [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) vlastnost:

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Proxy server a scénáře pro vyrovnávání zatížení

Služby, které interakci s požadavky z Internetu nebo podnikové síti a jsou za proxy server nebo službu Vyrovnávání zatížení může vyžadovat další konfiguraci. Další informace najdete v tématu [konfigurace ASP.NET Core k práci s proxy servery a nástroje pro vyrovnávání zatížení](xref:host-and-deploy/proxy-load-balancer).

## <a name="acknowledgments"></a>Potvrzování

Tento článek byl napsán pomocí publikovaných zdrojů:

* [Hostování ASP.NET Core jako služby systému Windows](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [Jak hostovat vaše základní technologie ASP.NET ve službě Windows](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
