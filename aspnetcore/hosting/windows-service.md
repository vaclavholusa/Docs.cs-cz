---
title: "Hostitele ve službě Windows"
author: tdykstra
description: "Zjistěte, jak hostovat aplikace ASP.NET Core ve službě Windows."
keywords: "Hostování ASP.NET Core, služba systému Windows"
ms.author: tdykstra
manager: wpickett
ms.date: 03/30/2017
ms.topic: article
ms.assetid: d9a65066-d7cb-47df-b046-64629c4d2c6f
ms.technology: aspnet
ms.prod: aspnet-core
uid: hosting/windows-service
ms.openlocfilehash: a6d1acf5ab8f40b0b4d487a6f34cd83d13907852
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a>Hostování aplikace ASP.NET Core ve službě Windows

podle [tní Dykstra](https://github.com/tdykstra)

Doporučený způsob k hostování aplikace ASP.NET Core v systému Windows, když nepoužíváte služby IIS je chcete-li používat [služba systému Windows](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications). Tímto způsobem ji může automaticky spustit po restartování počítače a dojde k chybě, bez čekání na uživatele k přihlášení.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample)). Najdete v článku [další kroky](#next-steps) pokyny o tom, jak ji spustit.

## <a name="prerequisites"></a>Požadavky

* Aplikace musíte spustit na modul runtime rozhraní .NET Framework.  V *.csproj* souboru, zadejte odpovídající hodnoty pro [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) a [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog). Tady je příklad:

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  Při vytváření projektu v sadě Visual Studio, použijte **aplikace ASP.NET Core (rozhraní .NET Framework)** šablony.

* Pokud aplikace se zobrazí požadavky z Internetu (nikoli pouze z interní sítě), musíte použít [WebListener](xref:fundamentals/servers/weblistener) webový server místo [Kestrel](xref:fundamentals/servers/kestrel).  Kestrel musí použít se službou IIS pro nasazení okraj.  Další informace najdete v tématu [použití Kestrel s reverzní proxy server](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

## <a name="getting-started"></a>Začínáme

Tato část popisuje minimální změny, které jsou nezbytné k nastavení existujícího projektu ASP.NET Core ke spuštění ve službě.

* Nainstalujte balíček NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).

* Proveďte následující změny v `Program.Main`:
  
  * Volání `host.RunAsService` místo `host.Run`.
  
  * Pokud váš kód volá `UseContentRoot`, použijte cestu k umístění pro publikování místo`Directory.GetCurrentDirectory()` 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* Publikování aplikace do složky.

  Použití [dotnet publikování](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) nebo [profil publikování se Visual Studio](xref:publishing/web-publishing-vs) který publikuje do složky.

* Otestujte vytváření a spouštění služby.

  Otevřete okno příkazového řádku správce používat [sc.exe](https://technet.microsoft.com/library/bb490995) nástroj příkazového řádku k vytvoření a spuštění služby.  
  
  Pokud zadáte název vaší služby Moje_služba, můžete publikovat aplikaci, kterou chcete `c:\svc`a aplikace jmenuje AspNetCoreService, příkazy bude vypadat takto:

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```
  `binPath` Hodnota je cesta ke spustitelnému souboru vaší aplikace, včetně názvu spustitelného souboru, sám sebe.

  ![Okno konzoly vytvořte a spusťte příklad](windows-service/_static/create-start.png)

  Po dokončení těchto příkazů, můžete vyhledat stejnou cestu jako při spuštění jako konzolové aplikace (ve výchozím nastavení, `http://localhost:5000`)

  ![Spuštěná ve službě](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a>Zadejte způsob, jak spustit mimo službu

Je snazší testování a ladění při používáte mimo službu, takže se obvykle přidávat kód, který volá `host.RunAsService` pouze za určitých podmínek.  Například můžete spustit jako konzolovou aplikaci pokud dojde `--console` argument příkazového řádku nebo pokud je připojen ladicí program.

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a>Zpracování ukončení a spuštění události

Pokud chcete zpracovávat `OnStarting`, `OnStarted`, a `OnStopping` události, proveďte následující další změny:

* Vytvořte třídu, která je odvozena z `WebHostService`.

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* Vytvoření metody rozšíření pro `IWebHost` , předá vaše vlastní `WebHostService` k `ServiceBase.Run`.

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* V `Program.Main` změnu volání nové metody rozšíření místo `host.RunAsService`.

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

Pokud vaše vlastní `WebHostService` kódu je potřeba získat službu z vkládání závislostí (například protokolovač), můžete získat z `Services` vlastnost `IWebHost`.

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a>Další kroky

[Ukázkové aplikace](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) Tento doprovodný článek je jednoduché webové aplikace MVC, která se změnila, jak je znázorněno v předchozích příklady kódu.  Chcete-li používat službu, proveďte následující kroky:

* Publikovat do *c:\svc*.

* Otevřete okno správce.

* Zadejte následující příkazy:

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * V prohlížeči přejděte na http://localhost: 5000 ověřit, zda je spuštěna.

Pokud aplikace nelze spustit očekávaným způsobem, při spuštění ve službě, je rychlý způsob, jak zpřístupnit chybové zprávy pro přidání poskytovatele protokolování, jako [zprostředkovatele protokolu událostí systému Windows](xref:fundamentals/logging/index#eventlog).

## <a name="acknowledgments"></a>Potvrzování

Tento článek byl napsán pomocí zdroje, které již byly publikovány. Nejdřívější a nejvhodnější z nich byly tyto:

* [Hostování ASP.NET Core jako služby systému Windows](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [Jak hostovat vaše základní technologie ASP.NET ve službě Windows](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
