# <a name="custom-webhost-service-sample"></a><span data-ttu-id="f5f3c-101">Ukázka služby vlastního webového hostitele</span><span class="sxs-lookup"><span data-stu-id="f5f3c-101">Custom WebHost Service Sample</span></span>

<span data-ttu-id="f5f3c-102">Tento příklad ukazuje, jak hostovat aplikace ASP.NET Core jako služba Windows bez použití služby IIS.</span><span class="sxs-lookup"><span data-stu-id="f5f3c-102">This sample shows how to host an ASP.NET Core app as a Windows Service without using IIS.</span></span> <span data-ttu-id="f5f3c-103">V této ukázce podle scénáře popsaného v [hostovat aplikace ASP.NET Core ve službě Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="f5f3c-103">This sample demonstrates the scenario described in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

## <a name="instructions"></a><span data-ttu-id="f5f3c-104">Pokyny</span><span class="sxs-lookup"><span data-stu-id="f5f3c-104">Instructions</span></span>

<span data-ttu-id="f5f3c-105">Ukázková aplikace je webová aplikace Razor Pages upravit podle pokynů v [hostovat aplikace ASP.NET Core ve službě Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="f5f3c-105">The sample app is a Razor Pages web app modified according to the instructions in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

<span data-ttu-id="f5f3c-106">Spusťte aplikaci ve službě, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f5f3c-106">To run the app in a service, perform the following steps:</span></span>

1. <span data-ttu-id="f5f3c-107">Vytvořte složku na *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="f5f3c-107">Create a folder at *c:\svc*.</span></span>

1. <span data-ttu-id="f5f3c-108">Publikujte aplikaci do složky s `dotnet publish --configuration Release --output c:\\svc`.</span><span class="sxs-lookup"><span data-stu-id="f5f3c-108">Publish the app to the folder with `dotnet publish --configuration Release --output c:\\svc`.</span></span> <span data-ttu-id="f5f3c-109">Příkaz přesune prostředků aplikace, aby *svc* složky, včetně požadované `appsettings.json` souboru a `wwwroot` složky.</span><span class="sxs-lookup"><span data-stu-id="f5f3c-109">The command moves the app's assets to the *svc* folder, including the required `appsettings.json` file and the `wwwroot` folder.</span></span>

1. <span data-ttu-id="f5f3c-110">Otevřít **správce** příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="f5f3c-110">Open an **administrator** command prompt.</span></span>

1. <span data-ttu-id="f5f3c-111">Spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="f5f3c-111">Execute the following commands:</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

  <span data-ttu-id="f5f3c-112">*Mezera mezi znaménko rovná se a začněte řetězec cesty je povinný.*</span><span class="sxs-lookup"><span data-stu-id="f5f3c-112">*The space between the equal sign and the start of the path string is required.*</span></span>

1. <span data-ttu-id="f5f3c-113">V prohlížeči přejděte na `http://localhost:5000` a ověřte, zda je spuštěna služba.</span><span class="sxs-lookup"><span data-stu-id="f5f3c-113">In a browser, navigate to `http://localhost:5000` and verify that the service is running.</span></span> <span data-ttu-id="f5f3c-114">Aplikace přesměruje do zabezpečeného koncového bodu `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="f5f3c-114">The app redirects to the secure endpoint `https://localhost:5001`.</span></span>

1. <span data-ttu-id="f5f3c-115">K zastavení služby použijte příkaz:</span><span class="sxs-lookup"><span data-stu-id="f5f3c-115">To stop the service, use the command:</span></span>

   ```console
   sc stop MyService
   ```

<span data-ttu-id="f5f3c-116">Pokud se aplikace nespustí podle očekávání, je rychlý způsob, jak zpřístupnit chybové zprávy pro přidání poskytovatele protokolování, například [zprostředkovatele protokolu událostí Windows](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span><span class="sxs-lookup"><span data-stu-id="f5f3c-116">If the app doesn't start as expected, a quick way to make error messages accessible is to add a logging provider, such as the [Windows EventLog provider](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span></span> <span data-ttu-id="f5f3c-117">Další možností je kontrola v protokolu událostí aplikace pomocí prohlížeče událostí systému.</span><span class="sxs-lookup"><span data-stu-id="f5f3c-117">Another option is to check the Application Event Log using the Event Viewer on the system.</span></span> <span data-ttu-id="f5f3c-118">Tady je příklad, neošetřené výjimce FileNotFound chyby v protokolu událostí aplikace:</span><span class="sxs-lookup"><span data-stu-id="f5f3c-118">For example, here's an unhandled exception for a FileNotFound error in the Application Event Log:</span></span>

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
