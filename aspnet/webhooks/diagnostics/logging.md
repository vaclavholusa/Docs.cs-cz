---
uid: webhooks/diagnostics/logging
title: "Protokolování Webhooky ASP.NET | Microsoft Docs"
author: rick-anderson
description: "Postup přihlášení Webhooky ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 042d20e38a9bc4f1e9792f6e3ff5be11a1eaa882
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="aspnet-webhooks-logging"></a>ASP.NET Webhooky protokolování

Microsoft ASP.NET WebHooks používá protokolování jako způsob hlášení problémů a problémů. Ve výchozím nastavení se protokoly zapisují pomocí [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) kde je lze spravovat pomocí [trasování – moduly naslouchání](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) jako jakékoli jiné datový proud protokolu.

Při nasazení webové aplikace jako webové aplikace Azure, protokoly jsou automaticky převzata a lze je spravovat společně s jakýmikoli jiným [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) protokolování. Podrobnosti najdete v tématu [povolit protokolování diagnostiky pro webové aplikace v Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)

Kromě toho protokoly můžete získat přímo v prostředí Visual Studio podle popisu v [řešení potíží s webovou aplikaci v Azure App Service pomocí sady Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="redirecting-logs"></a>Přesměrování protokoly

Místo psaní protokoly [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), je možné poskytnout alternativní protokolování implementace, která můžete připojit přímo k správce protokolu, jako například [Log4Net](http://logging.apache.org/log4net/) a [NLog ](http://nlog-project.org/). Jednoduše zadejte implementaci [objektu ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) a zaregistrovat ji pomocí modul vkládání závislostí podle svého výběru a bude získat převzata podle WebHooks Microsoft ASP.NET. Najdete v tématu [vkládání závislostí ve webovém rozhraní API 2 ASP.NET](https://www.asp.net/web-api/overview/advanced/dependency-injection) podrobnosti.
