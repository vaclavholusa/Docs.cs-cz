# <a name="custom-webhost-service-sample"></a>Ukázka služby vlastní tomuto webovému hostiteli

Tento příklad ukazuje, jak hostovat aplikace ASP.NET Core jako služby systému Windows bez použití služby IIS. Tento příklad znázorňuje podle scénáře popsaného v [hostovat aplikace ASP.NET Core ve službě Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

## <a name="instructions"></a>Pokyny

Ukázková aplikace je stránky Razor webové aplikace upravit podle pokynů v [hostovat aplikace ASP.NET Core ve službě Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

Ke spuštění aplikace ve službě, proveďte následující kroky:

1. Vytvořte složku na *c:\svc*.

1. Publikujte aplikaci do složky s `dotnet publish --configuration Release --output c:\\svc`. Příkaz přesune prostředky aplikace na *svc* složky, včetně požadované `appsettings.json` souboru a `wwwroot` složky.

1. Otevřete **správce** příkazového řádku.

1. Spuštěním následujících příkazů:

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

  *Mezery mezi znaménkem rovnosti a začátkem řetězec cesty je požadovaná.*

1. V prohlížeči přejděte na `http://localhost:5000` a ověřte, zda je spuštěna služba. Aplikace přesměruje na zabezpečený koncový bod `https://localhost:5001`.

1. K zastavení služby, použijte příkaz:

   ```console
   sc stop MyService
   ```

Pokud aplikace se nespustí podle očekávání, je rychlý způsob, jak zpřístupnit chybové zprávy pro přidání poskytovatele protokolování, například [zprostředkovatele protokolu událostí systému Windows](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog). Další možností je naleznete v protokolu událostí aplikace pomocí prohlížeče událostí systému. Zde je například k neošetřené výjimce FileNotFound chyby v protokolu událostí aplikace:

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
