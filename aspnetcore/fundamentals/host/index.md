---
title: Hostitel v ASP.NET Core
author: guardrex
description: Další informace o ASP.NET Core webového hostitele a obecné hostitele .NET, které jsou zodpovědní za správu spuštění a životního cyklu aplikací.
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/index
ms.openlocfilehash: 365c679e789c07818c6eb007f40f6aef43b82c44
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276614"
---
# <a name="host-in-aspnet-core"></a>Hostitel v ASP.NET Core

Konfigurace aplikace .NET a spusťte *hostitele*. Hostitel je zodpovědná za spuštění a životního cyklu správy aplikací. Dva hostitele rozhraní API jsou k dispozici pro použití:

* [Webové hostitele](xref:fundamentals/host/web-host) &ndash; vhodná pro hostování webových aplikací.
* [Obecné hostitele](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 nebo vyšší) &ndash; vhodná pro hostování jiných webových aplikací (například aplikace, které běží úlohy na pozadí). V budoucí verzi bude obecné hostitele vhodný pro hostování jakékoliv aplikace, včetně webových aplikací. Obecné hostitel nakonec nahradí webového hostitele.

Pro hostování ASP.NET Core *webové aplikace*, vývojáři využít webového hostitele na základě [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder). Pro hostování *jiných webových aplikací*, vývojáři měli použít obecné hostitele na základě [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).
