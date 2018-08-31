---
title: Získání knihoven na straně klienta v ASP.NET Core s LibMan
author: scottaddie
description: Informace o instalaci knihoven na straně klienta v projektu aplikace ASP.NET Core pomocí Správce knihovny (LibMan).
ms.author: scaddie
ms.custom: mvc
ms.date: 08/14/2018
uid: client-side/libman/index
ms.openlocfilehash: a6ff0cc3342cfac74739387aa17046ed5050232f
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312356"
---
# <a name="client-side-library-acquisition-in-aspnet-core-with-libman"></a><span data-ttu-id="8f500-103">Získání knihoven na straně klienta v ASP.NET Core s LibMan</span><span class="sxs-lookup"><span data-stu-id="8f500-103">Client-side library acquisition in ASP.NET Core with LibMan</span></span>

<span data-ttu-id="8f500-104">Podle [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="8f500-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="8f500-105">Správce knihovny (LibMan) je nástroj pro získání zjednodušené, na straně klienta knihovny.</span><span class="sxs-lookup"><span data-stu-id="8f500-105">Library Manager (LibMan) is a lightweight, client-side library acquisition tool.</span></span> <span data-ttu-id="8f500-106">LibMan stáhne oblíbených knihoven a architektur ze systému souborů nebo z [síť pro doručování obsahu (CDN)](https://wikipedia.org/wiki/Content_delivery_network).</span><span class="sxs-lookup"><span data-stu-id="8f500-106">LibMan downloads popular libraries and frameworks from the file system or from a [content delivery network (CDN)](https://wikipedia.org/wiki/Content_delivery_network).</span></span> <span data-ttu-id="8f500-107">Podporované sítě CDN patří [CDNJS](https://cdnjs.com/) a [unpkg](https://unpkg.com/#/).</span><span class="sxs-lookup"><span data-stu-id="8f500-107">The supported CDNs include [CDNJS](https://cdnjs.com/) and [unpkg](https://unpkg.com/#/).</span></span> <span data-ttu-id="8f500-108">Soubory vybrané knihovny jsou vyvolány a najdete v příslušné umístění v rámci projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8f500-108">The selected library files are fetched and placed in the appropriate location within the ASP.NET Core project.</span></span>

## <a name="libman-use-cases"></a><span data-ttu-id="8f500-109">LibMan případy použití</span><span class="sxs-lookup"><span data-stu-id="8f500-109">LibMan use cases</span></span>

<span data-ttu-id="8f500-110">LibMan nabízí následující výhody:</span><span class="sxs-lookup"><span data-stu-id="8f500-110">LibMan offers the following benefits:</span></span>

* <span data-ttu-id="8f500-111">Budou staženy pouze soubory knihovny, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="8f500-111">Only the library files you need are downloaded.</span></span>
* <span data-ttu-id="8f500-112">Další nástroje, jako například [Node.js](https://nodejs.org), [npm](https://www.npmjs.com), a [Webpacku](https://webpack.js.org), není nutné získat podmnožinu souborů v knihovně.</span><span class="sxs-lookup"><span data-stu-id="8f500-112">Additional tooling, such as [Node.js](https://nodejs.org), [npm](https://www.npmjs.com), and [WebPack](https://webpack.js.org), isn't necessary to acquire a subset of files in a library.</span></span>
* <span data-ttu-id="8f500-113">Soubory mohou být umístěny v konkrétním umístění bez použití svislých úlohy sestavení nebo ručního kopírování.</span><span class="sxs-lookup"><span data-stu-id="8f500-113">Files can be placed in a specific location without resorting to build tasks or manual file copying.</span></span>

<span data-ttu-id="8f500-114">Podívejte se na další informace o výhodách společnosti LibMan [front-endu moderního webového vývoje v sadě Visual Studio 2017: LibMan segment](https://channel9.msdn.com/Events/Build/2017/B8073#time=43m34s).</span><span class="sxs-lookup"><span data-stu-id="8f500-114">For more information about LibMan's benefits, watch [Modern front-end web development in Visual Studio 2017: LibMan segment](https://channel9.msdn.com/Events/Build/2017/B8073#time=43m34s).</span></span>

<span data-ttu-id="8f500-115">LibMan není systém správy balíčků.</span><span class="sxs-lookup"><span data-stu-id="8f500-115">LibMan isn't a package management system.</span></span> <span data-ttu-id="8f500-116">Pokud už používáte Správce balíčků, jako je například npm nebo [yarn](https://yarnpkg.com), tím pokračovat i.</span><span class="sxs-lookup"><span data-stu-id="8f500-116">If you're already using a package manager, such as npm or [yarn](https://yarnpkg.com), continue doing so.</span></span> <span data-ttu-id="8f500-117">LibMan nebyl vyvinuté nahradit těchto nástrojů.</span><span class="sxs-lookup"><span data-stu-id="8f500-117">LibMan wasn't developed to replace those tools.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8f500-118">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="8f500-118">Additional resources</span></span>

* <xref:client-side/libman/libman-vs>
* <xref:client-side/libman/libman-cli>
* [<span data-ttu-id="8f500-119">Úložiště LibMan GitHub</span><span class="sxs-lookup"><span data-stu-id="8f500-119">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
