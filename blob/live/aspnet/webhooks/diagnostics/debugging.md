---
uid: webhooks/diagnostics/debugging
title: "Ladění Webhooky ASP.NET | Microsoft Docs"
author: rick-anderson
description: "Postup ladění ASP.NET Webhooky."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 566ee353f6a947e3ef0efdfd0af3a81dff2147c7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-debugging"></a><span data-ttu-id="81602-103">ASP.NET Webhooky ladění</span><span class="sxs-lookup"><span data-stu-id="81602-103">ASP.NET WebHooks debugging</span></span>  

## <a name="debugging-in-azure"></a><span data-ttu-id="81602-104">Ladění v Azure</span><span class="sxs-lookup"><span data-stu-id="81602-104">Debugging in Azure</span></span>

<span data-ttu-id="81602-105">K ladění webové aplikace při spuštění v Azure, naleznete v kurzu [řešení potíží s webovou aplikaci v Azure App Service pomocí sady Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="81602-105">To debug your Web Application while running in Azure, please see the tutorial [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="debugging-with-source-and-symbols"></a><span data-ttu-id="81602-106">Ladění se zdrojem a symboly.</span><span class="sxs-lookup"><span data-stu-id="81602-106">Debugging with Source and Symbols</span></span>

<span data-ttu-id="81602-107">Kromě ladění vlastní kód, je možné k ladění přímo do WebHooks Microsoft ASP.NET a ve skutečnosti všechny .NET.</span><span class="sxs-lookup"><span data-stu-id="81602-107">In addition to debugging your own code, it is possible to debug directly into Microsoft ASP.NET WebHooks, and in fact all of .NET.</span></span> <span data-ttu-id="81602-108">Tento postup funguje bez ohledu na to, jestli při ladění místně nebo vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="81602-108">This works regardless of whether you debug locally or remotely.</span></span> <span data-ttu-id="81602-109">Nejprve nakonfigurujte Visual Studio najít zdroj a symboly přechodem na **ladění** a potom **možnosti a nastavení**.</span><span class="sxs-lookup"><span data-stu-id="81602-109">First, configure Visual Studio to find the source and symbols by going to **Debug** and then **Options and Settings**.</span></span> <span data-ttu-id="81602-110">Nastavení možností takto:</span><span class="sxs-lookup"><span data-stu-id="81602-110">Set the options like this:</span></span>

![Možnosti a nastavení](_static/SourceSymbols.png)

<span data-ttu-id="81602-112">Pak přidejte odkaz na [symbolsource.org](http://symbolsource.org) pro stahování zdroj a symboly.</span><span class="sxs-lookup"><span data-stu-id="81602-112">Then add a link to [symbolsource.org](http://symbolsource.org) for downloading the source and symbols.</span></span> <span data-ttu-id="81602-113">Přejděte na **symboly** kartě nabídce výše a přidejte následující jako umístění symbolu:</span><span class="sxs-lookup"><span data-stu-id="81602-113">Go to the **Symbols** tab of the menu above and add the following as a symbol location:</span></span>

```
http://srv.symbolsource.org/pdb/Public
```

<span data-ttu-id="81602-114">Kromě toho se ujistěte, že adresář mezipaměti obsahuje krátký název; názvy souborů v opačném případě můžete získat příliš dlouho které způsobí symboly, které mají se nenačetl.</span><span class="sxs-lookup"><span data-stu-id="81602-114">In addition, make sure that the cache directory has a short name; otherwise the file names can get too long which will cause the symbols to not load.</span></span> <span data-ttu-id="81602-115">Ukázka cesta je:</span><span class="sxs-lookup"><span data-stu-id="81602-115">A sample path is:</span></span>

```
C:\SymCache
```

<span data-ttu-id="81602-116">Nastavení by měl vypadat podobně jako tento:</span><span class="sxs-lookup"><span data-stu-id="81602-116">The settings should look similar to this:</span></span>

![Ukázka umístění souboru možnosti Symbol](_static/SymSource.png)
