---
title: ASP.NET Core hostitele v kontejnerech Docker
author: rick-anderson
description: Zjistit odkazy na zdroje informací řešení pro hostování aplikací ASP.NET Core v Docker kontejnery.
ms.author: riande
ms.custom: mvc
ms.date: 01/08/2018
uid: host-and-deploy/docker/index
ms.openlocfilehash: 272bd0a0dad2fb62c33dcedd1ce8430eefb2c238
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276085"
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
