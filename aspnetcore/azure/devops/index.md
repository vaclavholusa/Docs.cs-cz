---
title: DevOps s využitím ASP.NET Core a Azure
author: CamSoper
description: Průvodce, který poskytuje pokyny k začátku do konce na vytváření procesních toků pro DevOps pro aplikace ASP.NET Core hostované v Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/index
ms.openlocfilehash: 09ca835e908e81c6f38f9430fb40638ba6dc3350
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722607"
---
# <a name="devops-with-aspnet-core-and-azure"></a>DevOps s využitím ASP.NET Core a Azure

Vítejte v Průvodci životního cyklu vývoje Azure pro .NET Tato příručka představuje základní koncepty vytváření životního cyklu vývoje kolem Azure s využitím .NET nástrojů a procesů. Po dokončení tohoto průvodce, budete těžit z výhod až po zralé sadu nástrojů DevOps.

## <a name="who-this-guide-is-for"></a>Kdo tento průvodce je na

Zkušení vývojáři ASP.NET (úroveň 200 300) by měl být. Nemusíte vědět nic o Azure, jak tom dozvíme v tomto úvodu. Tento průvodce může být také užitečné pro DevOps techniky, kteří se zaměřují na operace než vývoje.

Tato příručka cílí na vývojáře Windows. Systémy Linux a macOS se ale plně podporují pomocí .NET Core. Přizpůsobit tento průvodce pro Linux nebo macOS, podívejte se na pro popisky pro Linux nebo macOS rozdíly.

## <a name="what-this-guide-doesnt-cover"></a>Co tato příručka nepopisuje

Tato příručka se zaměřuje na začátku do konce průběžné nasazování prostředí pro vývojáře na platformě .NET. Není vyčerpávající příručka k vše, co Azure, a to není zaměřit výrazně na rozhraní .NET API pro služby Azure. Důraz je po průběžnou integraci, nasazení, monitorování a ladění. Na konci Průvodce nabízí doporučení pro další kroky. Součástí návrhy jsou služeb platformy Azure, které jsou užitečné pro vývojáře využívající technologii ASP.NET.

## <a name="whats-in-this-guide"></a>Co je v této příručce

### <a name="tools-and-downloadsxrefazuredevopstools-and-downloads"></a>[Nástroje a soubory ke stažení](xref:azure/devops/tools-and-downloads)

Zjistěte, kde získat nástroje používané v tomto průvodci.

### <a name="deploy-to-app-servicexrefazuredevopsdeploy-to-app-service"></a>[Nasazení do služby App Service](xref:azure/devops/deploy-to-app-service)

Přečtěte si různé metody pro nasazení aplikace ASP.NET Core do služby Azure App Service.

### <a name="continuous-integration-and-deploymentxrefazuredevopscicd"></a>[Průběžná integrace a nasazování](xref:azure/devops/cicd)

Sestavení začátku do konce průběžnou integraci a nasazení řešení pro aplikace ASP.NET Core s využitím Githubu, VSTS a Azure.

### <a name="monitor-and-debugxrefazuredevopsmonitor"></a>[Monitorování a ladění](xref:azure/devops/monitor)

Pomocí nástrojů Azure můžete sledovat, řešení potíží a ladění aplikací.

### <a name="next-stepsxrefazuredevopsnext-steps"></a>[Další kroky](xref:azure/devops/next-steps)

Další postupy výuky pro vývojáře v ASP.NET Core seznámení s Azure.

## <a name="acknowledgments"></a>Potvrzení

Děkujeme, že jste pro všechny uživatele v komunitě .NET, který uživatel do tohoto průvodce užitečné návrhy! Rádi bychom se konkrétně Děkujeme, že následující členy komunity, kteří se podílejí na konečnou kontrolu těchto materiálů:

* [SAM Wronski](https://www.youtube.com/c/worldofzerodevelopment)
* [Jeffrey Palermo](https://twitter.com/jeffreypalermo)

## <a name="conclusion"></a>Závěr

Připraví Tento průvodce vám umožní vytvářet životního cyklu vývoje kontinuální integrace postavené na technologii ASP.NET Core a Azure App Service.

## <a name="additional-reading"></a>Další čtení

* [Co je Cloud Computing?](https://azure.microsoft.com/overview/what-is-cloud-computing/)
* [Příklady Cloud computingu](https://azure.microsoft.com/overview/examples-of-cloud-computing/)
* [Co je IaaS?](https://azure.microsoft.com/overview/what-is-iaas/)
* [Co je PaaS?](https://azure.microsoft.com/overview/what-is-paas/)
