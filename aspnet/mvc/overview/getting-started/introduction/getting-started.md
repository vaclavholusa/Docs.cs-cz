---
uid: mvc/overview/getting-started/introduction/getting-started
title: Začínáme s ASP.NET MVC 5 | Dokumentace Microsoftu
author: Rick-Anderson
description: 'Poznámka: Aktualizovanou verzi tohoto kurzu je k dispozici zde pomocí sady Visual Studio 2015. Nové kurzu se používá technologie ASP.NET Core MVC 6, které poskytuje mnoho improvem...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 0585e3a841aef72a17d966041029ff7be129a2b3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402120"
---
<a name="getting-started-with-aspnet-mvc-5"></a><span data-ttu-id="60b2e-104">Začínáme s ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="60b2e-104">Getting Started with ASP.NET MVC 5</span></span>
====================
<span data-ttu-id="60b2e-105">Podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="60b2e-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [consider RP](../../../../includes/razor.md)]

 <span data-ttu-id="60b2e-106">V tomto kurzu se seznámíte se základy vytváření webové aplikace ASP.NET MVC 5 pomocí [Visual Studio 2017](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="60b2e-106">This tutorial will teach you the basics of building an ASP.NET MVC 5 web app using [Visual Studio 2017](https://www.visualstudio.com/).</span></span> <span data-ttu-id="60b2e-107">Konečné zdroj kurzu [Githubu](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)</span><span class="sxs-lookup"><span data-stu-id="60b2e-107">Final Source for tutorial located on [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)</span></span>


 <span data-ttu-id="60b2e-108">V tomto kurzu zapsal [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , a [Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )</span><span class="sxs-lookup"><span data-stu-id="60b2e-108">This tutorial was written by [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[@scottgu](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](https://twitter.com/shanselman) ), and [Rick Anderson](https://twitter.com/RickAndMSFT) ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) )</span></span>

 <span data-ttu-id="60b2e-109">Potřebujete účet Azure tak, aby tuto aplikaci nasadit do Azure:</span><span class="sxs-lookup"><span data-stu-id="60b2e-109">You need an Azure account to deploy this app to Azure:</span></span>

 - <span data-ttu-id="60b2e-110">Je možné [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) – budete dostávat kredity můžete použít k vyzkoušení placených služeb Azure a dokonce i po jejich použití až můžete účet ponechat a dál používat bezplatné služby Azure.</span><span class="sxs-lookup"><span data-stu-id="60b2e-110">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
 - <span data-ttu-id="60b2e-111">Je možné [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) – vaše předplatné MSDN vám dává kredity každý měsíc, můžete použít k placení za služby Azure.</span><span class="sxs-lookup"><span data-stu-id="60b2e-111">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>


## <a name="getting-started"></a><span data-ttu-id="60b2e-112">Začínáme</span><span class="sxs-lookup"><span data-stu-id="60b2e-112">Getting Started</span></span>

<span data-ttu-id="60b2e-113">Začněte tím, že instalaci a používání [Visual Studio 2017](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="60b2e-113">Start by installing and running [Visual Studio 2017](https://www.visualstudio.com/).</span></span>

<span data-ttu-id="60b2e-114">Visual Studio je integrované vývojové prostředí, nebo integrovaného vývojového prostředí.</span><span class="sxs-lookup"><span data-stu-id="60b2e-114">Visual Studio is an IDE, or integrated development environment.</span></span> <span data-ttu-id="60b2e-115">Stejným způsobem, jako používáte k tvorbě dokumenty Microsoft Wordu, budete používat integrované vývojové prostředí pro vytváření aplikací.</span><span class="sxs-lookup"><span data-stu-id="60b2e-115">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="60b2e-116">V sadě Visual Studio je seznam v dolní části zobrazující různé možnosti, které jsou k dispozici pro vás.</span><span class="sxs-lookup"><span data-stu-id="60b2e-116">In Visual Studio there's a list along the bottom showing various options available to you.</span></span> <span data-ttu-id="60b2e-117">Je také nabídka, která nabízí další způsob, jak provádět úkoly v integrovaném vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="60b2e-117">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="60b2e-118">(Například místo výběru **nový projekt** z **Start** stránky, můžete použít nabídku a vyberte **souboru** &gt; **nový projekt**.)</span><span class="sxs-lookup"><span data-stu-id="60b2e-118">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>


![](getting-started/_static/image1.png)  


## <a name="creating-your-first-application"></a><span data-ttu-id="60b2e-119">Vytvoření vaší první aplikace</span><span class="sxs-lookup"><span data-stu-id="60b2e-119">Creating Your First Application</span></span>

<span data-ttu-id="60b2e-120">Klikněte na tlačítko **nový projekt**, vyberte jazyk Visual C# na levé straně, pak **webové** a pak vyberte **webová aplikace ASP.NET (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="60b2e-120">Click **New Project**, then select Visual C# on the left, then **Web** and then select **ASP.NET Web Application (.NET Framework)**.</span></span> <span data-ttu-id="60b2e-121">Pojmenujte svůj projekt "MvcMovie" a potom klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="60b2e-121">Name your project "MvcMovie" and then click **OK**.</span></span>

![](getting-started/_static/image2.png)

<span data-ttu-id="60b2e-122">V **nový projekt ASP.NET** dialogového okna, klikněte na tlačítko **MVC** a potom klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="60b2e-122">In the **New ASP.NET Project** dialog, click **MVC** and then click **OK**.</span></span>

![](getting-started/_static/image3.png)

<span data-ttu-id="60b2e-123">Visual Studio použít výchozí šablonu projektu ASP.NET MVC, které jste právě vytvořili, takže Teď máte funkční aplikaci bez teď zrovna nic nedělá!</span><span class="sxs-lookup"><span data-stu-id="60b2e-123">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="60b2e-124">Jde jednoduchý "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="60b2e-124">This is a simple "Hello World!"</span></span> <span data-ttu-id="60b2e-125">projekt a je vhodné místo pro spuštění vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="60b2e-125">project, and it's a good place to start your application.</span></span>

![](getting-started/_static/image4.png)

<span data-ttu-id="60b2e-126">Klikněte na tlačítko F5 spusťte ladění.</span><span class="sxs-lookup"><span data-stu-id="60b2e-126">Click F5 to start debugging.</span></span> <span data-ttu-id="60b2e-127">F5 způsobí, že Visual Studio spustíte [služby IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) a spouštění vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="60b2e-127">F5 causes Visual Studio to start [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) and run your web app.</span></span> <span data-ttu-id="60b2e-128">Visual Studio pak spustí prohlížeč a otevře domovskou stránku aplikace.</span><span class="sxs-lookup"><span data-stu-id="60b2e-128">Visual Studio then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="60b2e-129">Všimněte si, že na panelu Adresa prohlížeče `localhost:port#` a nemít něco podobného `example.com`.</span><span class="sxs-lookup"><span data-stu-id="60b2e-129">Notice that the address bar of the browser says `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="60b2e-130">Důvodem je, že `localhost` vždy odkazuje na vaše vlastní místní počítače v tomto případě je spuštěna aplikace právě jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="60b2e-130">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="60b2e-131">Při spuštění webový projekt sady Visual Studio náhodný port se používá pro webový server.</span><span class="sxs-lookup"><span data-stu-id="60b2e-131">When Visual Studio runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="60b2e-132">Na následujícím obrázku je číslo portu 1234.</span><span class="sxs-lookup"><span data-stu-id="60b2e-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="60b2e-133">Při spuštění aplikace se zobrazí jiné číslo portu.</span><span class="sxs-lookup"><span data-stu-id="60b2e-133">When you run the application, you'll see a different port number.</span></span>

![](getting-started/_static/image5.png)

<span data-ttu-id="60b2e-134">Předem připravené tuto výchozí šablonu vám Home, kontaktů a o stránky.</span><span class="sxs-lookup"><span data-stu-id="60b2e-134">Right out of the box this default template gives you Home, Contact and About pages.</span></span> <span data-ttu-id="60b2e-135">Na obrázku výše nezobrazuje **Domů**, **o** a **kontakt** odkazy.</span><span class="sxs-lookup"><span data-stu-id="60b2e-135">The image above doesn't show the **Home**, **About** and **Contact** links.</span></span> <span data-ttu-id="60b2e-136">V závislosti na velikosti okna prohlížeče můžete potřebovat kliknutím na ikonu navigační naleznete pod následujícími odkazy.</span><span class="sxs-lookup"><span data-stu-id="60b2e-136">Depending on the size of your browser window, you might need to click the navigation icon to see these links.</span></span>

![](getting-started/_static/image6.png)  

<span data-ttu-id="60b2e-137">Aplikace také poskytuje podporu pro registraci a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="60b2e-137">The application also provides support to register and log in.</span></span> <span data-ttu-id="60b2e-138">Dalším krokem je změnit, jak tato aplikace funguje a přečtěte si ještě něco o architektuře ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="60b2e-138">The next step is to change how this application works and learn a little bit about ASP.NET MVC.</span></span> <span data-ttu-id="60b2e-139">Ukončete aplikaci ASP.NET MVC a Změníme kód.</span><span class="sxs-lookup"><span data-stu-id="60b2e-139">Close the ASP.NET MVC application and let's change some code.</span></span>

<span data-ttu-id="60b2e-140">Seznam aktuální kurzy najdete v tématu [MVC Doporučené články](../mvc-learning-sequence.md).</span><span class="sxs-lookup"><span data-stu-id="60b2e-140">For a list of current tutorials, see [MVC recommended articles](../mvc-learning-sequence.md).</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="60b2e-141">Zobrazit tuto aplikaci běžící v Azure</span><span class="sxs-lookup"><span data-stu-id="60b2e-141">See this App Running on Azure</span></span>

<span data-ttu-id="60b2e-142">Chcete zobrazit dokončené web spuštěný jako živou webovou aplikaci?</span><span class="sxs-lookup"><span data-stu-id="60b2e-142">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="60b2e-143">Kompletní verze aplikace můžete nasadit ke svému účtu Azure jednoduše kliknutím na následující tlačítko.</span><span class="sxs-lookup"><span data-stu-id="60b2e-143">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

<span data-ttu-id="60b2e-144">Potřebujete účet Azure k nasazení tohoto řešení do Azure.</span><span class="sxs-lookup"><span data-stu-id="60b2e-144">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="60b2e-145">Pokud ještě nemáte účet, máte následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="60b2e-145">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="60b2e-146">[Zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) – budete dostávat kredity můžete použít k vyzkoušení placených služeb Azure a dokonce i po jejich použití až můžete účet ponechat a dál používat bezplatné služby Azure.</span><span class="sxs-lookup"><span data-stu-id="60b2e-146">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="60b2e-147">[Aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) – vaše předplatné MSDN vám dává kredity každý měsíc, můžete použít k placení za služby Azure.</span><span class="sxs-lookup"><span data-stu-id="60b2e-147">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="60b2e-148">Next</span><span class="sxs-lookup"><span data-stu-id="60b2e-148">Next</span></span>](adding-a-controller.md)
