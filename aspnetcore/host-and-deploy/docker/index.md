---
title: ASP.NET Core hostitele v kontejnerech Docker
author: rick-anderson
description: Zjistit odkazy na zdroje informací řešení pro hostování aplikací ASP.NET Core v Docker kontejnery.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/08/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/docker/index
ms.openlocfilehash: 12a179287ec302994380e0faf4b843596f8c2f4e
ms.sourcegitcommit: d45d766504c2c5aad2453f01f089bc6b696b5576
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/30/2018
ms.locfileid: "30280092"
---
# <a name="host-aspnet-core-in-docker-containers"></a>ASP.NET Core hostitele v kontejnerech Docker

Jsou k dispozici pro získání informací o hostování aplikací ASP.NET Core v Docker v následujících článcích:

[Úvod ke kontejnerům a Dockeru](/dotnet/standard/microservices-architecture/container-docker-introduction/index)  
V tématu Jak rozdělení do kontejnerů je přístup k vývoji softwaru, ve kterém aplikace nebo služby, jeho závislosti a jeho konfigurace instalace balíčku jako obrázek na kontejneru. Bitové kopie můžete otestovat a pak nasazená na hostitele.

[Co je Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-defined)  
Zjistit, jak Docker je otevřený projekt pro automatizaci nasazení aplikace jako přenosné, soběstačný kontejnery, které můžou běžet v cloudu nebo místně.

[Terminologie docker](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-terminology)  
Další termíny a definice pro Docker technologii.

[Kontejnery, obrázky a registry Dockeru](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-containers-images-registries)  
Zjistěte, jak Docker kontejneru Image jsou uložené v registru bitovou kopii pro konzistentní nasazení prostředích.

[Vytvoření Imagí Dockeru pro aplikace .NET Core](/dotnet/articles/core/docker/building-net-docker-images)  
Naučte se vytvářet a dockerize aplikace ASP.NET Core. Prozkoumejte imagí Dockeru spravován společností Microsoft a prozkoumat případy použití.

[Nástroje sady Visual Studio pro Docker](xref:host-and-deploy/docker/visual-studio-tools-for-docker)  
Zjistit, jak Visual Studio 2017 podporuje vytváření, ladění a spouštění ASP.NET Core aplikací cílení na rozhraní .NET Framework nebo .NET Core na Docker pro systém Windows. Kontejnery Windows a Linux jsou podporovány.

[Publikování do image Dockeru](/azure/vs-azure-tools-docker-hosting-web-apps-in-docker)  
Zjistěte, jak pomocí sady Visual Studio Tools for Docker rozšíření nasazení aplikace ASP.NET Core do hostitelů Docker v Azure pomocí prostředí PowerShell.

[Konfigurace ASP.NET Core k práci s proxy servery a nástroje pro vyrovnávání zatížení](xref:host-and-deploy/proxy-load-balancer)  
Další konfigurace může být potřeba pro aplikace, které jsou hostovány za proxy servery a nástroje pro vyrovnávání zatížení. Předávání žádosti přes proxy server často skryje informace o původní žádost, jako je například IP schématu a klienta. Může být potřeba předávat některé informace o požadavku ručně do aplikace.
