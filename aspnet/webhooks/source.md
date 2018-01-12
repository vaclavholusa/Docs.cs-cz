---
uid: webhooks/source
title: "ASP.NET Webhooky zdrojového kódu a balíčky NuGet | Microsoft Docs"
author: rick-anderson
description: "Odkazy na ASP.NET Webhooky zdrojového kódu a balíčky NuGet"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 733a0839c77bcfc96214bdf235ce8fe22ee2d3cf
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/09/2018
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a><span data-ttu-id="ca8c2-103">ASP.NET Webhooky zdrojového kódu a balíčky NuGet</span><span class="sxs-lookup"><span data-stu-id="ca8c2-103">ASP.NET WebHooks source code and NuGet packages</span></span>

<span data-ttu-id="ca8c2-104">Microsoft ASP.NET WebHooks je součástí rodiny Microsoft ASP.NET modulů a je hostovaná jako [otevřený zdroj projektu na Githubu](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="ca8c2-104">Microsoft ASP.NET WebHooks is part of the Microsoft ASP.NET family of modules and is hosted as an [Open Source Project on GitHub](https://github.com/aspnet/WebHooks).</span></span> <span data-ttu-id="ca8c2-105">To znamená, že jsme přijímat příspěvky, ale podívejte se prosím na [příspěvku pokyny](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) před odesláním žádost o přijetí změn.</span><span class="sxs-lookup"><span data-stu-id="ca8c2-105">This means that we accept contributions, but please look at the [Contribution Guidelines](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) before submitting a pull request.</span></span>

<span data-ttu-id="ca8c2-106">Tato dokumentace online, které právě čtete teď se hostují taky jako [Open Source na Githubu](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) a také přijímá příspěvky.</span><span class="sxs-lookup"><span data-stu-id="ca8c2-106">This online documentation which you are reading now is also hosted as [Open Source on GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) and also accepts contributions.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="ca8c2-107">Balíčky NuGet</span><span class="sxs-lookup"><span data-stu-id="ca8c2-107">NuGet packages</span></span>

<span data-ttu-id="ca8c2-108">[Balíčky NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) jsou rozdělené do tří částí:</span><span class="sxs-lookup"><span data-stu-id="ca8c2-108">The [NuGet packages](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) are divided into three parts:</span></span>

* <span data-ttu-id="ca8c2-109">[Běžné](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): běžné balíčku, jež jsou sdílena mezi odesílateli a příjemci.</span><span class="sxs-lookup"><span data-stu-id="ca8c2-109">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): A common package that is shared between senders and receivers.</span></span>

* <span data-ttu-id="ca8c2-110">[Odesílatel](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): Sada balíčky podpora odeslání vlastní Webhooky ostatním.</span><span class="sxs-lookup"><span data-stu-id="ca8c2-110">[Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): A set of packages supporting sending your own WebHooks to others.</span></span> <span data-ttu-id="ca8c2-111">Funkce pro odesílání Webhooky je podrobně popsaná v další v [odesílání Webhooky](sending/index.md).</span><span class="sxs-lookup"><span data-stu-id="ca8c2-111">The functionality for sending WebHooks is described in more detail in [Sending WebHooks](sending/index.md).</span></span>

* <span data-ttu-id="ca8c2-112">[Příjemci](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): Sada balíčky podpora přijímání Webhooky od ostatních uživatelů.</span><span class="sxs-lookup"><span data-stu-id="ca8c2-112">[Receivers](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): A set of packages supporting receiving WebHooks from others.</span></span> <span data-ttu-id="ca8c2-113">Funkce pro příjem Webhooky jsou popsány podrobněji v [přijetí Webhooky](receiving/index.md).</span><span class="sxs-lookup"><span data-stu-id="ca8c2-113">The functionality for receiving WebHooks is described in more detail in [Receiving WebHooks](receiving/index.md).</span></span>