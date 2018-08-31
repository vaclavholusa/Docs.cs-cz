---
title: Hostitel v ASP.NET Core
author: guardrex
description: Další informace o webového hostitele ASP.NET Core a .NET obecný hostitele, která jsou odpovědná za spuštění a životního cyklu správy aplikací.
ms.author: riande
ms.custom: mvc
ms.date: 08/28/2018
uid: fundamentals/host/index
ms.openlocfilehash: 9927722b5080beb94e5628d9e7b54e6d50a5bff8
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336047"
---
# <a name="host-in-aspnet-core"></a>Hostitel v ASP.NET Core

Konfigurace aplikací .NET a spouštění *hostitele*. Hostitel je zodpovědný za spouštění a životního cyklu správy aplikací. Dva hostitele rozhraní API se dají používat:

* [Webového hostitele](xref:fundamentals/host/web-host) &ndash; vhodná pro hostování webových aplikací.
* [Obecný hostitele](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 nebo novější) &ndash; vhodná pro hostování jiných webových aplikací (například aplikace, na kterých běží úlohy na pozadí). V budoucí verzi bude obecný hostitele vhodný pro hostování jakékoliv aplikace, včetně webových aplikací. Obecný hostitel nakonec nahradí webového hostitele.

Pro hostování ASP.NET Core *webové aplikace*, vývojáři měli použít webového hostitele na základě <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>. Pro hostování *mimo web apps*, vývojáři měli použít obecný hostitele na základě <xref:Microsoft.Extensions.Hosting.HostBuilder>.

<xref:fundamentals/host/hosted-services>  
Zjistěte, jak implementovat úlohy na pozadí s hostovanými službami v ASP.NET Core.

<xref:fundamentals/configuration/platform-specific-configuration>  
Objevte, jak vylepšit aplikaci ASP.NET Core z odkazované nebo neodkazovaná sestavení pomocí <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementace.
