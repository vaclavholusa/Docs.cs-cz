# <a name="custom-webhost-service-sample"></a><span data-ttu-id="4763f-101">Ukázka služby vlastní tomuto webovému hostiteli</span><span class="sxs-lookup"><span data-stu-id="4763f-101">Custom WebHost Service Sample</span></span>

<span data-ttu-id="4763f-102">Tento příklad ukazuje doporučeným způsobem, jak hostovat aplikace ASP.NET Core v systému Windows bez použití služby IIS jako služby systému Windows.</span><span class="sxs-lookup"><span data-stu-id="4763f-102">This sample shows the recommended way to host an ASP.NET Core app on Windows without using IIS as a Windows Service.</span></span> <span data-ttu-id="4763f-103">Tento příklad znázorňuje funkce popsané v [hostovat aplikace ASP.NET Core ve službě Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="4763f-103">This sample demonstrates the features described in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

## <a name="instructions"></a><span data-ttu-id="4763f-104">Pokyny</span><span class="sxs-lookup"><span data-stu-id="4763f-104">Instructions</span></span>

<span data-ttu-id="4763f-105">Ukázková aplikace je upravit podle pokynů v jednoduché webové aplikace MVC [hostovat aplikace ASP.NET Core ve službě Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="4763f-105">The sample app is a simple MVC web app modified according to the instructions in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

<span data-ttu-id="4763f-106">Ke spuštění aplikace ve službě, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4763f-106">To run the app in a service, perform the following steps:</span></span>

1. <span data-ttu-id="4763f-107">Vytvořte složku na *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="4763f-107">Create a folder at *c:\svc*.</span></span>

1. <span data-ttu-id="4763f-108">Publikujte aplikaci do složky s `dotnet publish --configuration Release --output c:\\svc`.</span><span class="sxs-lookup"><span data-stu-id="4763f-108">Publish the app to the folder with `dotnet publish --configuration Release --output c:\\svc`.</span></span> <span data-ttu-id="4763f-109">Příkaz přesune do složky, včetně požadované prostředky aplikace `appsettings.json` souboru a `wwwroot` složku s jeho obsahem.</span><span class="sxs-lookup"><span data-stu-id="4763f-109">The command will move the app's assets to the folder, including the required `appsettings.json` file and the `wwwroot` folder with its contents.</span></span>

1. <span data-ttu-id="4763f-110">Otevřete **správce** příkazové prostředí.</span><span class="sxs-lookup"><span data-stu-id="4763f-110">Open an **administrator** command shell.</span></span>

1. <span data-ttu-id="4763f-111">Spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="4763f-111">Execute the following commands:</span></span>

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

1. <span data-ttu-id="4763f-112">V prohlížeči přejděte na `http://localhost:5000` k ověření, že je služba spuštěná.</span><span class="sxs-lookup"><span data-stu-id="4763f-112">In a browser, go to `http://localhost:5000` to verify that the service is running.</span></span>

1. <span data-ttu-id="4763f-113">K zastavení služby, použijte příkaz:</span><span class="sxs-lookup"><span data-stu-id="4763f-113">To stop the service, use the command:</span></span>

   ```console
   sc stop MyService
   ```

<span data-ttu-id="4763f-114">Pokud aplikace nelze spustit očekávaným způsobem, při spuštění ve službě, je rychlý způsob, jak zpřístupnit chybové zprávy pro přidání poskytovatele protokolování, například [zprostředkovatele protokolu událostí systému Windows](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span><span class="sxs-lookup"><span data-stu-id="4763f-114">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider, such as the [Windows EventLog provider](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span></span> <span data-ttu-id="4763f-115">Další možností je naleznete v protokolu událostí aplikace pomocí prohlížeče událostí systému.</span><span class="sxs-lookup"><span data-stu-id="4763f-115">Another option is to check the Application Event Log using the Event Viewer on the system.</span></span> <span data-ttu-id="4763f-116">Zde je například k neošetřené výjimce FileNotFound chyby v protokolu událostí aplikace:</span><span class="sxs-lookup"><span data-stu-id="4763f-116">For example, here's an unhandled exception for a FileNotFound error in the Application Event Log:</span></span>

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
