---
title: Hostitel v ASP.NET Core
author: guardrex
description: Další informace o ASP.NET Core webového hostitele a obecné hostitele .NET, které jsou zodpovědní za správu spuštění a životního cyklu aplikací.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/index
ms.openlocfilehash: 7ad059e39866f59040c12b7ac15e9fa3405a9aad
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/17/2018
---
# <a name="host-in-aspnet-core"></a>Hostitel v ASP.NET Core

Konfigurace aplikace .NET a spusťte *hostitele*. Hostitel je zodpovědná za spuštění a životního cyklu správy aplikací. Dva hostitele rozhraní API jsou k dispozici pro použití:

* [Webové hostitele](xref:fundamentals/host/web-host) &ndash; vhodná pro hostování webových aplikací.
* [Obecné hostitele](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 nebo vyšší) &ndash; vhodná pro hostování jiných webových aplikací (například aplikace, které běží úlohy na pozadí). V budoucí verzi bude obecné hostitele vhodný pro hostování jakékoliv aplikace, včetně webových aplikací. Obecné hostitel nakonec nahradí webového hostitele.

V tomto okamžiku by vývojáři použít [webového hostitele](xref:fundamentals/host/web-host) na základě [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) pro hostování aplikací ASP.NET Core.
