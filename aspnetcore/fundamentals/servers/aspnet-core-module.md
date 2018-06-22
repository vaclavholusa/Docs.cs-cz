---
title: Modul ASP.NET Core
author: rick-anderson
description: Zjistěte, jak modul základní technologie ASP.NET umožňuje Kestrel webového serveru použít jako reverzní proxy server služby IIS nebo IIS Express.
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/23/2018
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 319ab80f20f3917dae921f43566e368b51c29f0b
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272332"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="d02b8-103">Modul ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d02b8-103">ASP.NET Core Module</span></span>

<span data-ttu-id="d02b8-104">Podle [tní Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), a [Ross Jan](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="d02b8-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), and [Chris Ross](https://github.com/Tratcher)</span></span> 

<span data-ttu-id="d02b8-105">Základní modul ASP.NET umožňuje ASP.NET Core aplikace pro spouštění za služby IIS v konfiguraci reverzní proxy server.</span><span class="sxs-lookup"><span data-stu-id="d02b8-105">The ASP.NET Core Module allows ASP.NET Core apps to run behind IIS in a reverse proxy configuration.</span></span> <span data-ttu-id="d02b8-106">Služba IIS poskytuje pokročilé webové funkce zabezpečení a možnosti správy aplikací.</span><span class="sxs-lookup"><span data-stu-id="d02b8-106">IIS provides advanced web app security and manageability features.</span></span>

<span data-ttu-id="d02b8-107">Podporované verze systému Windows:</span><span class="sxs-lookup"><span data-stu-id="d02b8-107">Supported Windows versions:</span></span>

* <span data-ttu-id="d02b8-108">Windows 7 nebo novější</span><span class="sxs-lookup"><span data-stu-id="d02b8-108">Windows 7 or later</span></span>
* <span data-ttu-id="d02b8-109">Windows Server 2008 R2 nebo novější</span><span class="sxs-lookup"><span data-stu-id="d02b8-109">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="d02b8-110">Základní modul ASP.NET pracuje pouze s Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d02b8-110">The ASP.NET Core Module only works with Kestrel.</span></span> <span data-ttu-id="d02b8-111">Modul není kompatibilní s [HTTP.sys](xref:fundamentals/servers/httpsys) (dříve se označovaly jako [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="d02b8-111">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

## <a name="aspnet-core-module-description"></a><span data-ttu-id="d02b8-112">Popis modulu jádro ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d02b8-112">ASP.NET Core Module description</span></span>

<span data-ttu-id="d02b8-113">Základní modul ASP.NET je nativní modul služby IIS, která po zapojení do kanálu služby IIS pro přesměrování požadavků na webu na back-end aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d02b8-113">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to redirect web requests to backend ASP.NET Core apps.</span></span> <span data-ttu-id="d02b8-114">Množství nativních modulů, jako je například ověřování systému Windows, zůstávají aktivní.</span><span class="sxs-lookup"><span data-stu-id="d02b8-114">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="d02b8-115">Další informace o moduly služby IIS active s modulem najdete v tématu [moduly služby IIS](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="d02b8-115">To learn more about IIS modules active with the module, see [IIS modules](xref:host-and-deploy/iis/modules).</span></span>

<span data-ttu-id="d02b8-116">Protože samostatné aplikace ASP.NET Core spustit v procesu z pracovní proces služby IIS, modul také zpracovává proces správy.</span><span class="sxs-lookup"><span data-stu-id="d02b8-116">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="d02b8-117">Modul zahájí proces pro aplikace ASP.NET Core při prvním požadavku dorazí a restartuje aplikace, pokud ji dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="d02b8-117">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="d02b8-118">Toto je v podstatě stejné chování jako aplikace ASP.NET 4.x, které běží v procesu v IIS, které jsou spravovány [služby Aktivace procesů systému Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="d02b8-118">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="d02b8-119">Následující diagram znázorňuje vztah mezi službou IIS, modul ASP.NET Core a ASP.NET Core aplikace:</span><span class="sxs-lookup"><span data-stu-id="d02b8-119">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and ASP.NET Core apps:</span></span>

![Modul ASP.NET Core](aspnet-core-module/_static/ancm.png)

<span data-ttu-id="d02b8-121">Požadavky přicházejí z webu pro ovladač HTTP.sys režimu jádra.</span><span class="sxs-lookup"><span data-stu-id="d02b8-121">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="d02b8-122">Ovladač směrovat požadavky na služby IIS na konfigurovaném portu webu, obvykle 80 (HTTP) nebo 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="d02b8-122">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="d02b8-123">Modul předává požadavky Kestrel na náhodných portu pro aplikaci, která není portu 80/443.</span><span class="sxs-lookup"><span data-stu-id="d02b8-123">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="d02b8-124">Modul určuje port, přes proměnné prostředí při spuštění a Middleware integrační služby IIS nakonfiguruje server tak, aby naslouchala na `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="d02b8-124">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="d02b8-125">Jsou-li provést další kontroly a odmítne požadavky, které nejsou pocházejí z modulu.</span><span class="sxs-lookup"><span data-stu-id="d02b8-125">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="d02b8-126">Modul nepodporuje předávání protokolu HTTPS, takže jsou předávány požadavky prostřednictvím protokolu HTTP i v případě, že přijatých službou IIS prostřednictvím protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d02b8-126">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="d02b8-127">Po Kestrel převezme žádost z modulu, požadavek se posune do ASP.NET Core middlewaru v řadě.</span><span class="sxs-lookup"><span data-stu-id="d02b8-127">After Kestrel picks up a request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="d02b8-128">Middlewaru v řadě zpracuje požadavek a předává je jako `HttpContext` instance aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="d02b8-128">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="d02b8-129">Odpověď aplikace je předán zpět do služby IIS, které nabízených oznámení zpátky klienta HTTP, který žádost iniciovala.</span><span class="sxs-lookup"><span data-stu-id="d02b8-129">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="d02b8-130">Základní modul ASP.NET obsahuje několik dalších funkcí.</span><span class="sxs-lookup"><span data-stu-id="d02b8-130">The ASP.NET Core Module has a few other functions.</span></span> <span data-ttu-id="d02b8-131">Modul může:</span><span class="sxs-lookup"><span data-stu-id="d02b8-131">The module can:</span></span>

* <span data-ttu-id="d02b8-132">Nastavení proměnných prostředí pro pracovní proces.</span><span class="sxs-lookup"><span data-stu-id="d02b8-132">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="d02b8-133">Přihlaste se stdout výstup do souboru úložiště pro řešení potíží při spuštění.</span><span class="sxs-lookup"><span data-stu-id="d02b8-133">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="d02b8-134">Předat dál tokeny ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="d02b8-134">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="d02b8-135">Jak nainstalovat a použít modul jádro ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d02b8-135">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="d02b8-136">Podrobné pokyny o tom, jak nainstalovat a používat modul jádro ASP.NET najdete v tématu [hostitele v systému Windows pomocí služby IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="d02b8-136">For detailed instructions on how to install and use the ASP.NET Core Module, see [Host on Windows with IIS](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="d02b8-137">Informace o konfiguraci modulu najdete v tématu [odkazu na modul jádro ASP.NET konfigurace](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="d02b8-137">For information on configuring the module, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d02b8-138">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="d02b8-138">Additional resources</span></span>

* [<span data-ttu-id="d02b8-139">Hostování ve Windows se službou IIS</span><span class="sxs-lookup"><span data-stu-id="d02b8-139">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="d02b8-140">Referenční dokumentace k modulu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d02b8-140">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="d02b8-141">Úložiště GitHub modulu jádra ASP.NET (zdrojový kód)</span><span class="sxs-lookup"><span data-stu-id="d02b8-141">ASP.NET Core Module GitHub repository (source code)</span></span>](https://github.com/aspnet/AspNetCoreModule)
