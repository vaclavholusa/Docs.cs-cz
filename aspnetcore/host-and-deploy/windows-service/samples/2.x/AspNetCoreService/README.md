# <a name="custom-webhost-service-sample"></a>Ukázka služby vlastního webového hostitele

Tento příklad ukazuje, jak hostovat aplikace ASP.NET Core jako služba Windows bez použití služby IIS. V této ukázce podle scénáře popsaného v [hostovat aplikace ASP.NET Core ve službě Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

## <a name="instructions"></a>Pokyny

Ukázková aplikace je webová aplikace Razor Pages upravit podle pokynů v [hostovat aplikace ASP.NET Core ve službě Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

Spusťte aplikaci ve službě, proveďte následující kroky:

1. Vytvořte složku na *c:\svc*.

1. Publikujte aplikaci do složky s `dotnet publish --configuration Release --output c:\\svc`. Příkaz přesune prostředků aplikace, aby *svc* složky, včetně požadované `appsettings.json` souboru a `wwwroot` složky.

1. Otevřít **správce** příkazového řádku.

1. Spusťte následující příkazy:

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

  *Mezera mezi znaménko rovná se a začněte řetězec cesty je povinný.*

1. V prohlížeči přejděte na `http://localhost:5000` a ověřte, zda je spuštěna služba. Aplikace přesměruje do zabezpečeného koncového bodu `https://localhost:5001`.

1. K zastavení služby použijte příkaz:

   ```console
   sc stop MyService
   ```

Pokud se aplikace nespustí podle očekávání, je rychlý způsob, jak zpřístupnit chybové zprávy pro přidání poskytovatele protokolování, například [zprostředkovatele protokolu událostí Windows](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog). Další možností je kontrola v protokolu událostí aplikace pomocí prohlížeče událostí systému. Tady je příklad, neošetřené výjimce FileNotFound chyby v protokolu událostí aplikace:

```console
Application: AspNetCoreService.exe
Framework Version: v4.0.30319
Description: The process was terminated due to an unhandled exception.
Exception Info: System.IO.FileNotFoundException
   at Microsoft.Extensions.Configuration.FileConfigurationProvider.Load(Boolean)
   at Microsoft.Extensions.Configuration.ConfigurationRoot..ctor(System.Collections.Generic.IList`1<Microsoft.Extensions.Configuration.IConfigurationProvider>)
   at Microsoft.Extensions.Configuration.ConfigurationBuilder.Build()
   ...
```
