# <a name="custom-webhost-service-sample"></a>Ukázka služby vlastní tomuto webovému hostiteli

Tento příklad ukazuje doporučeným způsobem, jak hostovat aplikace ASP.NET Core v systému Windows bez použití služby IIS jako služby systému Windows. Tento příklad znázorňuje funkce popsané v [hostovat aplikace ASP.NET Core ve službě Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

## <a name="instructions"></a>Pokyny

Ukázková aplikace je upravit podle pokynů v jednoduché webové aplikace MVC [hostovat aplikace ASP.NET Core ve službě Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

Ke spuštění aplikace ve službě, proveďte následující kroky:

1. Vytvořte složku na *c:\svc*.

1. Publikujte aplikaci do složky s `dotnet publish --configuration Release --output c:\\svc`. Příkaz přesune do složky, včetně požadované prostředky aplikace `appsettings.json` souboru a `wwwroot` složku s jeho obsahem.

1. Otevřete **správce** příkazové prostředí.

1. Spuštěním následujících příkazů:

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

1. V prohlížeči přejděte na `http://localhost:5000` k ověření, že je služba spuštěná.

1. K zastavení služby, použijte příkaz:

   ```console
   sc stop MyService
   ```

Pokud aplikace nelze spustit očekávaným způsobem, při spuštění ve službě, je rychlý způsob, jak zpřístupnit chybové zprávy pro přidání poskytovatele protokolování, například [zprostředkovatele protokolu událostí systému Windows](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog). Další možností je naleznete v protokolu událostí aplikace pomocí prohlížeče událostí systému. Zde je například k neošetřené výjimce FileNotFound chyby v protokolu událostí aplikace:

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
