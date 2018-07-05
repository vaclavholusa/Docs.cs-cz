---
uid: webhooks/diagnostics/logging
title: ASP.NET – Webhooky protokolování | Dokumentace Microsoftu
author: rick-anderson
description: Postup přihlášení ASP.NET – Webhooky.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.technology: ''
ms.openlocfilehash: 5501d250ea6dd86c0cfddec8d9941ec16b4f57ba
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364103"
---
# <a name="aspnet-webhooks-logging"></a>ASP.NET – Webhooky protokolování

Microsoft ASP.NET WebHooks používá protokolování jako způsob hlášení chyb a problémů. Ve výchozím nastavení se protokoly zapisují pomocí [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) kde může být spravovaných pomocí [naslouchacích procesů trasování](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) jako libovolný jiný datový proud protokolu.

Při nasazení webové aplikace jako webové aplikace Azure, protokoly se automaticky rozpoznat a lze je spravovat společně s jinými [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) protokolování. Podrobnosti najdete v tématu [povolení protokolování diagnostiky pro webové aplikace ve službě Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)

Kromě toho protokoly můžete získat přímo v prostředí Visual Studio, jak je popsáno v [řešení problémů s webovou aplikací ve službě Azure App Service pomocí sady Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="redirecting-logs"></a>Přesměrování protokolů

Místo psaní protokoly [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), je možné poskytnout alternativní protokolování implementace, která můžete připojit přímo k log manager například [Log4Net](http://logging.apache.org/log4net/) a [NLog ](http://nlog-project.org/). Stačí zadat implementace [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) a zaregistrovat modul injektáž závislostí podle vašeho výběru a jeho se získat vyzvednou WebHooks Microsoft ASP.NET. Podrobnosti najdete na [injektáž závislostí v ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) podrobnosti.
