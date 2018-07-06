---
uid: webhooks/source
title: ASP.NET – Webhooky zdrojový kód a balíčky NuGet | Dokumentace Microsoftu
author: rick-anderson
description: Odkazy na zdrojový kód ASP.NET – Webhooky a balíčky NuGet
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.openlocfilehash: 1ee4adc2a28054ed1f856d7e4e991b34972a70e8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802170"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a><span data-ttu-id="de0c1-103">ASP.NET – Webhooky zdrojový kód a balíčky NuGet</span><span class="sxs-lookup"><span data-stu-id="de0c1-103">ASP.NET WebHooks source code and NuGet packages</span></span>

<span data-ttu-id="de0c1-104">Microsoft ASP.NET WebHooks je součástí Microsoft ASP.NET řadu modulů a je hostovaný jako [Open Source projekt na Githubu](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="de0c1-104">Microsoft ASP.NET WebHooks is part of the Microsoft ASP.NET family of modules and is hosted as an [Open Source Project on GitHub](https://github.com/aspnet/WebHooks).</span></span> <span data-ttu-id="de0c1-105">To znamená, že jsme přijímat příspěvky, ale podívejte se prosím na [příspěvek pokyny](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) před odesláním žádosti o přijetí změn.</span><span class="sxs-lookup"><span data-stu-id="de0c1-105">This means that we accept contributions, but please look at the [Contribution Guidelines](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) before submitting a pull request.</span></span>

<span data-ttu-id="de0c1-106">Tuto dokumentaci online, které právě čtete nyní také hostována jako [Open Source na Githubu](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) a také přijímá příspěvky.</span><span class="sxs-lookup"><span data-stu-id="de0c1-106">This online documentation which you are reading now is also hosted as [Open Source on GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) and also accepts contributions.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="de0c1-107">Balíčky NuGet</span><span class="sxs-lookup"><span data-stu-id="de0c1-107">NuGet packages</span></span>

<span data-ttu-id="de0c1-108">[Balíčky NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) jsou rozdělené do tří částí:</span><span class="sxs-lookup"><span data-stu-id="de0c1-108">The [NuGet packages](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) are divided into three parts:</span></span>

* <span data-ttu-id="de0c1-109">[Běžné](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): common balíček, který je sdílen mezi odesílateli a příjemci.</span><span class="sxs-lookup"><span data-stu-id="de0c1-109">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): A common package that is shared between senders and receivers.</span></span>

* <span data-ttu-id="de0c1-110">[Odesílatel](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): sadu balíčků, které podporuje odesílání vlastních Webhooků do jiné.</span><span class="sxs-lookup"><span data-stu-id="de0c1-110">[Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): A set of packages supporting sending your own WebHooks to others.</span></span> <span data-ttu-id="de0c1-111">Funkce pro odesílání Webhooky jsou popsány podrobněji [odesílání Webhooky](sending/index.md).</span><span class="sxs-lookup"><span data-stu-id="de0c1-111">The functionality for sending WebHooks is described in more detail in [Sending WebHooks](sending/index.md).</span></span>

* <span data-ttu-id="de0c1-112">[Příjemci](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): sadu balíčků, podpora Webhooků příjem od ostatních.</span><span class="sxs-lookup"><span data-stu-id="de0c1-112">[Receivers](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): A set of packages supporting receiving WebHooks from others.</span></span> <span data-ttu-id="de0c1-113">Funkce pro příjem Webhooky jsou popsány podrobněji [přijímá Webhooky](receiving/index.md).</span><span class="sxs-lookup"><span data-stu-id="de0c1-113">The functionality for receiving WebHooks is described in more detail in [Receiving WebHooks](receiving/index.md).</span></span>
