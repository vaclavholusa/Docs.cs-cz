---
title: DevOps s využitím ASP.NET Core a Azure | Další kroky
author: CamSoper
description: Průvodce, který poskytuje pokyny k začátku do konce na vytváření procesních toků pro DevOps pro aplikace ASP.NET Core hostované v Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/next-steps
ms.openlocfilehash: c1b0b52295e2795e608399637dc9d864e2f572de
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722610"
---
# <a name="next-steps"></a>Další kroky

V tomto průvodci vytvořit kanál DevOps pro ukázkové aplikace ASP.NET Core. Blahopřejeme! Doufáme, že se vám líbilo učení k publikování webových aplikací ASP.NET Core do služby Azure App Service a automatizovat průběžné integrace změny.

Kromě hostování webů a DevOps Azure nabízí širokou škálu služeb Platform-as-a-Service (PaaS) je užitečný pro vývojáře ASP.NET Core. Tato část poskytuje stručný přehled o některé z nejčastěji používaných služeb.

## <a name="storage-and-databases"></a>Úložiště a databází

[Redis Cache](https://docs.microsoft.com/azure/redis-cache/) je vysokou propustností a nízkou latencí dat, ukládání do mezipaměti k dispozici jako službu. Je možné pro ukládání výstupu stránek, snížení požadavků na databázi a poskytování stavu relace ASP.NET do několika instancí aplikace.

[Azure Storage](https://docs.microsoft.com/azure/storage/) je široce škálovatelné cloudové úložiště Azure. Vývojáři můžou využít výhod [Queue Storage](https://docs.microsoft.com/azure/storage/queues/storage-queues-introduction) spolehlivé front a [Table Storage](https://docs.microsoft.com/azure/storage/tables/table-storage-overview) je úložiště dvojic klíč hodnota NoSQL navržená pro rychlý vývoj pomocí rozsáhlé, částečně strukturovaných datových sad.

[Azure SQL Database](https://docs.microsoft.com/azure/sql-database/) poskytuje funkce známé relační databáze jako služby pomocí stroje Microsoft SQL Server.

[Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/) globálně distribuovaná a vícemodelová databázová služba NoSQL. Několik rozhraní API jsou dostupná, včetně rozhraní SQL API (dříve se označovaly jako DocumentDB) a Cassandra, MongoDB.

## <a name="identity"></a>Identita

[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) a [Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/) jsou obě služby identit. Azure Active Directory je určená pro podnikové scénáře a umožňuje spolupráci Azure AD B2B (business-to-business), zatímco Azure Active Directory B2C je zamýšlené firmy zákazníka scénářů, včetně přihlášení sociálních sítí.

## <a name="mobile"></a>Mobilní zařízení

[Notification Hubs](https://docs.microsoft.com/azure/notification-hubs/) je modul pro multiplatformní a škálovatelnou nabízených oznámení k rychlému rozesílání milionů zpráv aplikacím spuštěným na různých typech zařízení.

## <a name="web-infrastructure"></a>Infrastruktura webové

[Služba Azure Container Service](https://docs.microsoft.com/azure/aks/) spravuje vaše hostované prostředí Kubernetes, tak rychle a snadno nasazovat a spravovat kontejnerizované aplikace bez znalosti Orchestrace kontejnerů.

[Služba Azure Search](https://docs.microsoft.com/azure/search/) slouží k vytvoření podniková řešení pro hledání nad privátním heterogenním obsahem.

[Service Fabric](https://docs.microsoft.com/azure/service-fabric/) je platforma distribuovaných systémů, která usnadňuje balení, nasazování a spravování škálovatelných a spolehlivých mikroslužeb a kontejnerů.
