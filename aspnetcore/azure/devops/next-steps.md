---
title: DevOps s využitím ASP.NET Core a Azure | Další kroky
author: CamSoper
description: Průvodce, který poskytuje pokyny k začátku do konce na vytváření procesních toků pro DevOps pro aplikace ASP.NET Core hostované v Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/next-steps
ms.openlocfilehash: 7a0f1b1b56a33b1870e0657d8ba465adb84f5a02
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756743"
---
# <a name="next-steps"></a><span data-ttu-id="c9ba9-103">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c9ba9-103">Next steps</span></span>

<span data-ttu-id="c9ba9-104">V tomto průvodci vytvořit kanál DevOps pro ukázkové aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c9ba9-104">In this guide, you created a DevOps pipeline for an ASP.NET Core sample app.</span></span> <span data-ttu-id="c9ba9-105">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="c9ba9-105">Congratulations!</span></span> <span data-ttu-id="c9ba9-106">Doufáme, že se vám líbilo učení k publikování webových aplikací ASP.NET Core do služby Azure App Service a automatizovat průběžné integrace změny.</span><span class="sxs-lookup"><span data-stu-id="c9ba9-106">We hope you enjoyed learning to publish ASP.NET Core web apps to Azure App Service and automate the continuous integration of changes.</span></span>

<span data-ttu-id="c9ba9-107">Kromě hostování webů a DevOps Azure nabízí širokou škálu služeb Platform-as-a-Service (PaaS) je užitečný pro vývojáře ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c9ba9-107">Beyond web hosting and DevOps, Azure has a wide array of Platform-as-a-Service (PaaS) services useful to ASP.NET Core developers.</span></span> <span data-ttu-id="c9ba9-108">Tato část poskytuje stručný přehled o některé z nejčastěji používaných služeb.</span><span class="sxs-lookup"><span data-stu-id="c9ba9-108">This section gives a brief overview of some of the most commonly used services.</span></span>

## <a name="storage-and-databases"></a><span data-ttu-id="c9ba9-109">Úložiště a databází</span><span class="sxs-lookup"><span data-stu-id="c9ba9-109">Storage and databases</span></span>

<span data-ttu-id="c9ba9-110">[Redis Cache](https://docs.microsoft.com/azure/redis-cache/) je vysokou propustností a nízkou latencí dat, ukládání do mezipaměti k dispozici jako službu.</span><span class="sxs-lookup"><span data-stu-id="c9ba9-110">[Redis Cache](https://docs.microsoft.com/azure/redis-cache/) is high-throughput, low-latency data caching available as a service.</span></span> <span data-ttu-id="c9ba9-111">Je možné pro ukládání výstupu stránek, snížení požadavků na databázi a poskytování stavu relace ASP.NET Core do několika instancí aplikace.</span><span class="sxs-lookup"><span data-stu-id="c9ba9-111">It can be used for caching page output, reducing database requests, and providing ASP.NET Core session state across multiple instances of an app.</span></span>

<span data-ttu-id="c9ba9-112">[Azure Storage](https://docs.microsoft.com/azure/storage/) je široce škálovatelné cloudové úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="c9ba9-112">[Azure Storage](https://docs.microsoft.com/azure/storage/) is Azure's massively scalable cloud storage.</span></span> <span data-ttu-id="c9ba9-113">Vývojáři můžou využít výhod [Queue Storage](https://docs.microsoft.com/azure/storage/queues/storage-queues-introduction) spolehlivé front a [Table Storage](https://docs.microsoft.com/azure/storage/tables/table-storage-overview) je úložiště dvojic klíč hodnota NoSQL navržená pro rychlý vývoj pomocí rozsáhlé, částečně strukturovaných datových sad.</span><span class="sxs-lookup"><span data-stu-id="c9ba9-113">Developers can take advantage of [Queue Storage](https://docs.microsoft.com/azure/storage/queues/storage-queues-introduction) for reliable message queuing, and [Table Storage](https://docs.microsoft.com/azure/storage/tables/table-storage-overview) is a NoSQL key-value store designed for rapid development using massive, semi-structured data sets.</span></span>

<span data-ttu-id="c9ba9-114">[Azure SQL Database](https://docs.microsoft.com/azure/sql-database/) poskytuje funkce známé relační databáze jako služby pomocí stroje Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c9ba9-114">[Azure SQL Database](https://docs.microsoft.com/azure/sql-database/) provides familiar relational database functionality as a service using the Microsoft SQL Server Engine.</span></span>

<span data-ttu-id="c9ba9-115">[Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/) globálně distribuovaná a vícemodelová databázová služba NoSQL.</span><span class="sxs-lookup"><span data-stu-id="c9ba9-115">[Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/) globally distributed, multi-model NoSQL database service.</span></span> <span data-ttu-id="c9ba9-116">Několik rozhraní API jsou dostupná, včetně rozhraní SQL API (dříve se označovaly jako DocumentDB) a Cassandra, MongoDB.</span><span class="sxs-lookup"><span data-stu-id="c9ba9-116">Multiple APIs are available, including SQL API (formerly called DocumentDB), Cassandra, and MongoDB.</span></span>

## <a name="identity"></a><span data-ttu-id="c9ba9-117">Identita</span><span class="sxs-lookup"><span data-stu-id="c9ba9-117">Identity</span></span>

<span data-ttu-id="c9ba9-118">[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) a [Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/) jsou obě služby identit.</span><span class="sxs-lookup"><span data-stu-id="c9ba9-118">[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) and [Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/) are both identity services.</span></span> <span data-ttu-id="c9ba9-119">Azure Active Directory je určená pro podnikové scénáře a umožňuje spolupráci Azure AD B2B (business-to-business), zatímco Azure Active Directory B2C je zamýšlené firmy zákazníka scénářů, včetně přihlášení sociálních sítí.</span><span class="sxs-lookup"><span data-stu-id="c9ba9-119">Azure Active Directory is designed for enterprise scenarios and enables Azure AD B2B (business-to-business) collaboration, while Azure Active Directory B2C is intended business-to-customer scenarios, including social network sign-in.</span></span>

## <a name="mobile"></a><span data-ttu-id="c9ba9-120">Mobilní zařízení</span><span class="sxs-lookup"><span data-stu-id="c9ba9-120">Mobile</span></span>

<span data-ttu-id="c9ba9-121">[Notification Hubs](https://docs.microsoft.com/azure/notification-hubs/) je modul pro multiplatformní a škálovatelnou nabízených oznámení k rychlému rozesílání milionů zpráv aplikacím spuštěným na různých typech zařízení.</span><span class="sxs-lookup"><span data-stu-id="c9ba9-121">[Notification Hubs](https://docs.microsoft.com/azure/notification-hubs/) is a multi-platform, scalable push-notification engine to quickly send millions of messages to apps running on various types of devices.</span></span>

## <a name="web-infrastructure"></a><span data-ttu-id="c9ba9-122">Infrastruktura webové</span><span class="sxs-lookup"><span data-stu-id="c9ba9-122">Web infrastructure</span></span>

<span data-ttu-id="c9ba9-123">[Služba Azure Container Service](https://docs.microsoft.com/azure/aks/) spravuje vaše hostované prostředí Kubernetes, tak rychle a snadno nasazovat a spravovat kontejnerizované aplikace bez znalosti Orchestrace kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="c9ba9-123">[Azure Container Service](https://docs.microsoft.com/azure/aks/) manages your hosted Kubernetes environment, making it quick and easy to deploy and manage containerized apps without container orchestration expertise.</span></span>

<span data-ttu-id="c9ba9-124">[Služba Azure Search](https://docs.microsoft.com/azure/search/) slouží k vytvoření podniková řešení pro hledání nad privátním heterogenním obsahem.</span><span class="sxs-lookup"><span data-stu-id="c9ba9-124">[Azure Search](https://docs.microsoft.com/azure/search/) is used to create an enterprise search solution over private, heterogenous content.</span></span>

<span data-ttu-id="c9ba9-125">[Service Fabric](https://docs.microsoft.com/azure/service-fabric/) je platforma distribuovaných systémů, která usnadňuje balení, nasazování a spravování škálovatelných a spolehlivých mikroslužeb a kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="c9ba9-125">[Service Fabric](https://docs.microsoft.com/azure/service-fabric/) is a distributed systems platform that makes it easy to package, deploy, and manage scalable and reliable microservices and containers.</span></span>
