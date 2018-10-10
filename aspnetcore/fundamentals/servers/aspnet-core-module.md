---
title: Modul ASP.NET Core
author: rick-anderson
description: Zjistěte, jak modul ASP.NET Core umožňuje webovému serveru Kestrel k použití jako reverzní proxy server služby IIS nebo IIS Express.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/21/2018
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 2f73a34b7d311c9e98ad2ecba11894d27bb2aa4d
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910886"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="ea21a-103">Modul ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ea21a-103">ASP.NET Core Module</span></span>

<span data-ttu-id="ea21a-104">Podle [Petr Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), a [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="ea21a-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), and [Chris Ross](https://github.com/Tratcher)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ea21a-105">Modul ASP.NET Core umožňuje aplikacím pro spuštění v pracovním procesu služby IIS, ASP.NET Core (*vnitroprocesové*) nebo za služby IIS v konfigurace reverzního proxy serveru (*mimo proces*).</span><span class="sxs-lookup"><span data-stu-id="ea21a-105">The ASP.NET Core Module allows ASP.NET Core apps to run in an IIS worker process (*in-process*) or behind IIS in a reverse proxy configuration (*out-of-process*).</span></span> <span data-ttu-id="ea21a-106">Služba IIS poskytuje funkce zabezpečení a správy rozšířené webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ea21a-106">IIS provides advanced web app security and manageability features.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="ea21a-107">Modul ASP.NET Core umožňuje spouštění za služby IIS v konfigurace reverzního proxy aplikací pro ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ea21a-107">The ASP.NET Core Module allows ASP.NET Core apps to run behind IIS in a reverse proxy configuration.</span></span> <span data-ttu-id="ea21a-108">Služba IIS poskytuje funkce zabezpečení a správy rozšířené webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ea21a-108">IIS provides advanced web app security and manageability features.</span></span>

::: moniker-end

<span data-ttu-id="ea21a-109">Podporované verze Windows:</span><span class="sxs-lookup"><span data-stu-id="ea21a-109">Supported Windows versions:</span></span>

* <span data-ttu-id="ea21a-110">Windows 7 nebo novější</span><span class="sxs-lookup"><span data-stu-id="ea21a-110">Windows 7 or later</span></span>
* <span data-ttu-id="ea21a-111">Windows Server 2008 R2 nebo novější</span><span class="sxs-lookup"><span data-stu-id="ea21a-111">Windows Server 2008 R2 or later</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ea21a-112">Při hostování v procesu, modul obsahuje implementaci serveru pro svůj vlastní `IISHttpServer`.</span><span class="sxs-lookup"><span data-stu-id="ea21a-112">When hosting in-process, the module has its own server implementation, `IISHttpServer`.</span></span>

<span data-ttu-id="ea21a-113">Při hostování mimo proces, modul funguje pouze s Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ea21a-113">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="ea21a-114">Modul je nekompatibilní s [HTTP.sys](xref:fundamentals/servers/httpsys) (dříve se označovaly jako [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="ea21a-114">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="ea21a-115">Modul funguje pouze s Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ea21a-115">The module only works with Kestrel.</span></span> <span data-ttu-id="ea21a-116">Modul je nekompatibilní s [HTTP.sys](xref:fundamentals/servers/httpsys) (dříve se označovaly jako [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="ea21a-116">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

::: moniker-end

## <a name="aspnet-core-module-description"></a><span data-ttu-id="ea21a-117">Popis modulu jádra ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ea21a-117">ASP.NET Core Module description</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ea21a-118">Modul ASP.NET Core je nativní modul služby IIS, která zpřístupní kanálu IIS buď:</span><span class="sxs-lookup"><span data-stu-id="ea21a-118">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="ea21a-119">Hostování aplikace v ASP.NET Core v rámci pracovní proces služby IIS (`w3wp.exe`), označované jako [model hostingu v procesu](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="ea21a-119">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>

* <span data-ttu-id="ea21a-120">Předání požadavku na webové aplikace ASP.NET Core s back-end systémem [Kestrel server](xref:fundamentals/servers/kestrel), označované jako [model hostingu mimo proces](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="ea21a-120">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="ea21a-121">Model hostingu v procesu</span><span class="sxs-lookup"><span data-stu-id="ea21a-121">In-process hosting model</span></span>

<span data-ttu-id="ea21a-122">Proces hostování, ASP.NET Core pomocí app běží ve stejném procesu jako jeho pracovní proces služby IIS.</span><span class="sxs-lookup"><span data-stu-id="ea21a-122">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="ea21a-123">Tím snížení výkonu v důsledku žádosti o připojení přes server proxy prostřednictvím adaptéru zpětné smyčky, při použití model hostingu mimo proces.</span><span class="sxs-lookup"><span data-stu-id="ea21a-123">This removes the performance penalty of proxying requests over the loopback adapter when using the out-of-process hosting model.</span></span> <span data-ttu-id="ea21a-124">Služba IIS zpracovává proces správy pomocí [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="ea21a-124">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="ea21a-125">Modul ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="ea21a-125">The ASP.NET Core Module:</span></span>

* <span data-ttu-id="ea21a-126">Provede inicializaci aplikace.</span><span class="sxs-lookup"><span data-stu-id="ea21a-126">Performs app initialization.</span></span>
  * <span data-ttu-id="ea21a-127">Načtení [CoreCLR](/dotnet/standard/glossary#coreclr).</span><span class="sxs-lookup"><span data-stu-id="ea21a-127">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="ea21a-128">Volání `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="ea21a-128">Calls `Program.Main`.</span></span>
* <span data-ttu-id="ea21a-129">Zpracovává životnost nativní požadavků služby IIS.</span><span class="sxs-lookup"><span data-stu-id="ea21a-129">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="ea21a-130">Následující diagram znázorňuje vztah mezi služby IIS, že modul ASP.NET Core a aplikace hostované v procesu:</span><span class="sxs-lookup"><span data-stu-id="ea21a-130">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![Modul ASP.NET Core](aspnet-core-module/_static/ancm-inprocess.png)

<span data-ttu-id="ea21a-132">Žádost o přijetí z webu pro ovladač HTTP.sys režimu jádra.</span><span class="sxs-lookup"><span data-stu-id="ea21a-132">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="ea21a-133">Ovladač přesměruje požadavek na nativní služby IIS na webu nakonfigurovaný port, obvykle 80 (HTTP) nebo 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="ea21a-133">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="ea21a-134">Modul obdrží požadavek na nativní a předá řízení k `IISHttpServer`, což je co převede žádosti z nativní do spravované.</span><span class="sxs-lookup"><span data-stu-id="ea21a-134">The module receives the native request and passes control to `IISHttpServer`, which is what converts the request from native to managed.</span></span>

<span data-ttu-id="ea21a-135">Po `IISHttpServer` příjmem požadavku, požadavek se vloží do kanálu middleware ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ea21a-135">After `IISHttpServer` picks up the request, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="ea21a-136">Zpracuje požadavek a předává je jako middleware kanálu `HttpContext` instanci aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="ea21a-136">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="ea21a-137">Odpověď aplikace je předán zpět do služby IIS, které nabízených oznámení je zpět klienta HTTP, který inicioval žádost.</span><span class="sxs-lookup"><span data-stu-id="ea21a-137">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="ea21a-138">Model hostingu mimo proces</span><span class="sxs-lookup"><span data-stu-id="ea21a-138">Out-of-process hosting model</span></span>

<span data-ttu-id="ea21a-139">Protože aplikace ASP.NET Core spuštění v procesu oddělit od pracovní proces služby IIS, zpracovává modul Správa procesů.</span><span class="sxs-lookup"><span data-stu-id="ea21a-139">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="ea21a-140">V modulu začne proces pro aplikace ASP.NET Core, když první žádost o přijetí a restartuje aplikaci, pokud se vypne nebo dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="ea21a-140">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="ea21a-141">Toto je v podstatě stejné chování jako aplikace, které běží v procesu, která jsou spravována [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="ea21a-141">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="ea21a-142">Následující diagram znázorňuje vztah mezi služby IIS, že modul ASP.NET Core a aplikace hostované na více instancí procesu:</span><span class="sxs-lookup"><span data-stu-id="ea21a-142">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![Modul ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="ea21a-144">Požadavky přicházejí z webu pro ovladač HTTP.sys režimu jádra.</span><span class="sxs-lookup"><span data-stu-id="ea21a-144">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="ea21a-145">Ovladač směruje požadavky do služby IIS na webu nakonfigurovaný port, obvykle 80 (HTTP) nebo 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="ea21a-145">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="ea21a-146">V modulu předává požadavky Kestrel na náhodný port pro aplikaci, která není port 80 a 443.</span><span class="sxs-lookup"><span data-stu-id="ea21a-146">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="ea21a-147">V modulu určuje port, přes proměnnou prostředí při spuštění a Middleware pro integraci služby IIS nakonfiguruje server tak, aby naslouchala na `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="ea21a-147">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="ea21a-148">Další kontroly jsou prováděny, a odmítne požadavky, které není pocházejí z modulu.</span><span class="sxs-lookup"><span data-stu-id="ea21a-148">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="ea21a-149">Modul nepodporuje předávání protokolu HTTPS, takže žádosti se předávají prostřednictvím protokolu HTTP i v případě, že byla přijata službou IIS přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ea21a-149">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="ea21a-150">Po Kestrel převezme žádosti z modulu, požadavek se vloží do kanálu middleware ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ea21a-150">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="ea21a-151">Zpracuje požadavek a předává je jako middleware kanálu `HttpContext` instanci aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="ea21a-151">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="ea21a-152">Middleware, které jsou přidány pomocí integrace služby IIS aktualizuje schéma, vzdálenou IP adresu a pathbase pro předání požadavku do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ea21a-152">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="ea21a-153">Odpověď aplikace je předán zpět do služby IIS, které nabízených oznámení je zpět klienta HTTP, který inicioval žádost.</span><span class="sxs-lookup"><span data-stu-id="ea21a-153">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="ea21a-154">Modul ASP.NET Core je nativní modul služby IIS, která zpřístupní kanálu IIS, aby předával aplikace webové požadavky do back-endu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ea21a-154">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="ea21a-155">Protože aplikace ASP.NET Core spuštění v procesu oddělit od pracovní proces služby IIS, zpracovává modul také procesu správy.</span><span class="sxs-lookup"><span data-stu-id="ea21a-155">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="ea21a-156">V modulu začne proces pro aplikace ASP.NET Core, když první žádost o přijetí a restartuje aplikaci, pokud ho dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="ea21a-156">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="ea21a-157">Toto je v podstatě stejné chování jako aplikace ASP.NET 4.x, na kterých běží v procesu ve službě IIS, která jsou spravována [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="ea21a-157">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="ea21a-158">Následující diagram znázorňuje vztah mezi službou IIS, že modul ASP.NET Core a aplikace:</span><span class="sxs-lookup"><span data-stu-id="ea21a-158">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![Modul ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="ea21a-160">Požadavky přicházejí z webu pro ovladač HTTP.sys režimu jádra.</span><span class="sxs-lookup"><span data-stu-id="ea21a-160">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="ea21a-161">Ovladač směruje požadavky do služby IIS na webu nakonfigurovaný port, obvykle 80 (HTTP) nebo 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="ea21a-161">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="ea21a-162">V modulu předává požadavky Kestrel na náhodný port pro aplikaci, která není port 80 a 443.</span><span class="sxs-lookup"><span data-stu-id="ea21a-162">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="ea21a-163">V modulu určuje port, přes proměnnou prostředí při spuštění a Middleware pro integraci služby IIS nakonfiguruje server tak, aby naslouchala na `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="ea21a-163">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="ea21a-164">Další kontroly jsou prováděny, a odmítne požadavky, které není pocházejí z modulu.</span><span class="sxs-lookup"><span data-stu-id="ea21a-164">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="ea21a-165">Modul nepodporuje předávání protokolu HTTPS, takže žádosti se předávají prostřednictvím protokolu HTTP i v případě, že byla přijata službou IIS přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ea21a-165">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="ea21a-166">Po Kestrel převezme žádosti z modulu, požadavek se vloží do kanálu middleware ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ea21a-166">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="ea21a-167">Zpracuje požadavek a předává je jako middleware kanálu `HttpContext` instanci aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="ea21a-167">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="ea21a-168">Middleware, které jsou přidány pomocí integrace služby IIS aktualizuje schéma, vzdálenou IP adresu a pathbase pro předání požadavku do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ea21a-168">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="ea21a-169">Odpověď aplikace je předán zpět do služby IIS, které nabízených oznámení je zpět klienta HTTP, který inicioval žádost.</span><span class="sxs-lookup"><span data-stu-id="ea21a-169">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="ea21a-170">Množství nativních modulů, jako je například ověřování Windows, zůstanou aktivní.</span><span class="sxs-lookup"><span data-stu-id="ea21a-170">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="ea21a-171">Další informace o moduly IIS aktivní s modulem, naleznete v tématu <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="ea21a-171">To learn more about IIS modules active with the module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="ea21a-172">Modul ASP.NET Core má několik dalších funkcí.</span><span class="sxs-lookup"><span data-stu-id="ea21a-172">The ASP.NET Core Module has a few other functions.</span></span> <span data-ttu-id="ea21a-173">Modul můžete:</span><span class="sxs-lookup"><span data-stu-id="ea21a-173">The module can:</span></span>

* <span data-ttu-id="ea21a-174">Nastavení proměnných prostředí pro pracovní proces.</span><span class="sxs-lookup"><span data-stu-id="ea21a-174">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="ea21a-175">Zaznamená stdout výstup do služby file storage pro řešení potíží při spuštění.</span><span class="sxs-lookup"><span data-stu-id="ea21a-175">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="ea21a-176">Předávání ověřovacích tokenů Windows.</span><span class="sxs-lookup"><span data-stu-id="ea21a-176">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="ea21a-177">Jak nainstalovat a používat modul ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ea21a-177">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="ea21a-178">Podrobné pokyny o tom, jak nainstalovat a používat modul ASP.NET Core najdete v tématu <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="ea21a-178">For detailed instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span> <span data-ttu-id="ea21a-179">Informace o konfiguraci modulu najdete v tématu <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="ea21a-179">For information on configuring the module, see the <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ea21a-180">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ea21a-180">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* [<span data-ttu-id="ea21a-181">Úložiště GitHub modulu jádra ASP.NET (zdrojového kódu)</span><span class="sxs-lookup"><span data-stu-id="ea21a-181">ASP.NET Core Module GitHub repository (source code)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
