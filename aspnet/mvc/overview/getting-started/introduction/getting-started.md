---
uid: mvc/overview/getting-started/introduction/getting-started
title: "Začínáme s ASP.NET MVC 5 | Microsoft Docs"
author: Rick-Anderson
description: "Poznámka: Aktualizovanou verzi tohoto kurzu je k dispozici zde pomocí sady Visual Studio 2015. Nový kurz používá ASP.NET Core MVC 6, která poskytuje mnoho improvem..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 8f2cb9b3f14ad95acd9e4c7a0ed26228d3c480e0
ms.sourcegitcommit: ec9371e2fbfcb8d62e7e7cae69e7752f3f205385
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/23/2017
---
<a name="getting-started-with-aspnet-mvc-5"></a><span data-ttu-id="29df9-104">Začínáme s ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="29df9-104">Getting Started with ASP.NET MVC 5</span></span>
====================
<span data-ttu-id="29df9-105">Podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="29df9-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[consider RP](../../../../includes/razor.md)]

 
 <span data-ttu-id="29df9-106">V tomto kurzu naučit, základní informace o vytváření ASP.NET MVC 5 webovou aplikaci pomocí [Visual Studio 2017](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="29df9-106">This tutorial will teach you the basics of building an ASP.NET MVC 5 web app using [Visual Studio 2017](https://www.visualstudio.com/).</span></span> <span data-ttu-id="29df9-107">Poslední zdroje pro kurz umístěn na [Githubu](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)</span><span class="sxs-lookup"><span data-stu-id="29df9-107">Final Source for tutorial located on [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)</span></span>
 
 
 <span data-ttu-id="29df9-108">V tomto kurzu napsal [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , a [Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )</span><span class="sxs-lookup"><span data-stu-id="29df9-108">This tutorial was written by [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[@scottgu](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](https://twitter.com/shanselman) ), and [Rick Anderson](https://twitter.com/RickAndMSFT) ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) )</span></span>
 
 <span data-ttu-id="29df9-109">Potřebujete účet Azure k nasazení této aplikace na Azure:</span><span class="sxs-lookup"><span data-stu-id="29df9-109">You need an Azure account to deploy this app to Azure:</span></span>
 
 - <span data-ttu-id="29df9-110">Můžete [zdarma otevřít účet Azure](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604) -získáte kredity, můžete použít k vyzkoušení placených služeb Azure a i po jejich použití až můžete účet ponechat a používat bezplatné služby Azure.</span><span class="sxs-lookup"><span data-stu-id="29df9-110">You can [open an Azure account for free](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
 - <span data-ttu-id="29df9-111">Můžete [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -vaše předplatné MSDN vám dává kredity každý měsíc, které můžete použít pro placené služby Azure.</span><span class="sxs-lookup"><span data-stu-id="29df9-111">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>


## <a name="getting-started"></a><span data-ttu-id="29df9-112">Začínáme</span><span class="sxs-lookup"><span data-stu-id="29df9-112">Getting Started</span></span>

<span data-ttu-id="29df9-113">Začněte tím, že instalace a spuštění [Visual Studio 2017](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="29df9-113">Start by installing and running [Visual Studio 2017](https://www.visualstudio.com/).</span></span>

<span data-ttu-id="29df9-114">Visual Studio je IDE, nebo integrované vývojové prostředí.</span><span class="sxs-lookup"><span data-stu-id="29df9-114">Visual Studio is an IDE, or integrated development environment.</span></span> <span data-ttu-id="29df9-115">Stejně jako zápisu dokumentů pomocí aplikace Microsoft Word, použijete k vytvoření aplikací rozhraní IDE.</span><span class="sxs-lookup"><span data-stu-id="29df9-115">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="29df9-116">V sadě Visual Studio je seznam podél dolního zobrazuje různé možnosti, které jsou k dispozici pro vás.</span><span class="sxs-lookup"><span data-stu-id="29df9-116">In Visual Studio there's a list along the bottom showing various options available to you.</span></span> <span data-ttu-id="29df9-117">Je také nabídku, která obsahuje jiný způsob, jak provádět úlohy v prostředí IDE.</span><span class="sxs-lookup"><span data-stu-id="29df9-117">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="29df9-118">(Například místo výběru **nový projekt** z **spustit** stránky, můžete použít nabídku a vyberte **soubor** &gt; **nový projekt**.)</span><span class="sxs-lookup"><span data-stu-id="29df9-118">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>

   
![](getting-started/_static/image1.png)  
 

## <a name="creating-your-first-application"></a><span data-ttu-id="29df9-119">Vytvoření vaší první aplikace</span><span class="sxs-lookup"><span data-stu-id="29df9-119">Creating Your First Application</span></span>

<span data-ttu-id="29df9-120">Klikněte na tlačítko **nový projekt**, pak vyberte Visual C# na levé straně, potom **webové** a pak vyberte **webové aplikace ASP.NET (rozhraní .NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="29df9-120">Click **New Project**, then select Visual C# on the left, then **Web** and then select **ASP.NET Web Application (.NET Framework)**.</span></span> <span data-ttu-id="29df9-121">Název projektu "MvcMovie" a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="29df9-121">Name your project "MvcMovie" and then click **OK**.</span></span>

![](getting-started/_static/image2.png)

<span data-ttu-id="29df9-122">V **nový projekt ASP.NET** dialogové okno, klikněte na tlačítko **MVC** a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="29df9-122">In the **New ASP.NET Project** dialog, click **MVC** and then click **OK**.</span></span>

![](getting-started/_static/image3.png)

<span data-ttu-id="29df9-123">Visual Studio použít výchozí šablonu pro projektu ASP.NET MVC, který jste právě vytvořili, takže budete mít funkční aplikaci, ihned bez jakékoli akce!</span><span class="sxs-lookup"><span data-stu-id="29df9-123">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="29df9-124">Jedná se jednoduché "Hello, World!"</span><span class="sxs-lookup"><span data-stu-id="29df9-124">This is a simple "Hello World!"</span></span> <span data-ttu-id="29df9-125">projektu ale je dobrým místem, kde spustit aplikaci.</span><span class="sxs-lookup"><span data-stu-id="29df9-125">project, and it's a good place to start your application.</span></span>

![](getting-started/_static/image4.png)

<span data-ttu-id="29df9-126">Klikněte na tlačítko F5 spusťte ladění.</span><span class="sxs-lookup"><span data-stu-id="29df9-126">Click F5 to start debugging.</span></span> <span data-ttu-id="29df9-127">F5 způsobí, že chcete spustit Visual Studio [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) a spouštění vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="29df9-127">F5 causes Visual Studio to start [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) and run your web app.</span></span> <span data-ttu-id="29df9-128">Visual Studio pak spustí prohlížeč a otevře domovskou stránku aplikace.</span><span class="sxs-lookup"><span data-stu-id="29df9-128">Visual Studio then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="29df9-129">Všimněte si, že panelu Adresa prohlížeče uvádí `localhost:port#` a není něco jako `example.com`.</span><span class="sxs-lookup"><span data-stu-id="29df9-129">Notice that the address bar of the browser says `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="29df9-130">Je to způsobeno `localhost` vždycky směřuje na vlastní místní počítač, který běží v tomto případě aplikace, které jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="29df9-130">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="29df9-131">Po spuštění webového projektu sady Visual Studio náhodný port se používá pro webový server.</span><span class="sxs-lookup"><span data-stu-id="29df9-131">When Visual Studio runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="29df9-132">Na obrázku níže číslo portu je 1234.</span><span class="sxs-lookup"><span data-stu-id="29df9-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="29df9-133">Při spuštění aplikace se zobrazí jiné číslo portu.</span><span class="sxs-lookup"><span data-stu-id="29df9-133">When you run the application, you'll see a different port number.</span></span>

![](getting-started/_static/image5.png)

<span data-ttu-id="29df9-134">Okamžitě po nasazení této výchozí šablony vám dává domácí, kontaktů a o stránky.</span><span class="sxs-lookup"><span data-stu-id="29df9-134">Right out of the box this default template gives you Home, Contact and About pages.</span></span> <span data-ttu-id="29df9-135">Na obrázku výše nezobrazí **Domů**, **o** a **kontaktujte** odkazy.</span><span class="sxs-lookup"><span data-stu-id="29df9-135">The image above doesn't show the **Home**, **About** and **Contact** links.</span></span> <span data-ttu-id="29df9-136">V závislosti na velikosti okna prohlížeče možná budete muset klikněte na ikonu navigace zobrazíte tyto odkazy.</span><span class="sxs-lookup"><span data-stu-id="29df9-136">Depending on the size of your browser window, you might need to click the navigation icon to see these links.</span></span>

![](getting-started/_static/image6.png)  

<span data-ttu-id="29df9-137">Aplikace také poskytuje podporu pro registraci a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="29df9-137">The application also provides support to register and log in.</span></span> <span data-ttu-id="29df9-138">Dalším krokem je a změnit tak, jak tato aplikace funguje trochu Další informace o architektuře ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="29df9-138">The next step is to change how this application works and learn a little bit about ASP.NET MVC.</span></span> <span data-ttu-id="29df9-139">Zavřete aplikaci ASP.NET MVC a změňte nějaký kód.</span><span class="sxs-lookup"><span data-stu-id="29df9-139">Close the ASP.NET MVC application and let's change some code.</span></span>

<span data-ttu-id="29df9-140">Seznam aktuální kurzy najdete v tématu [MVC Doporučené články](../mvc-learning-sequence.md).</span><span class="sxs-lookup"><span data-stu-id="29df9-140">For a list of current tutorials, see [MVC recommended articles](../mvc-learning-sequence.md).</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="29df9-141">Tato aplikace spuštěné v Azure</span><span class="sxs-lookup"><span data-stu-id="29df9-141">See this App Running on Azure</span></span>

<span data-ttu-id="29df9-142">Chcete zobrazit dokončení web spuštěný jako živou webovou aplikaci?</span><span class="sxs-lookup"><span data-stu-id="29df9-142">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="29df9-143">Úplnou verzi aplikace můžete nasadit ke svému účtu Azure jednoduše kliknutím na následující tlačítko.</span><span class="sxs-lookup"><span data-stu-id="29df9-143">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

<span data-ttu-id="29df9-144">Potřebujete účet Azure k nasazení tohoto řešení do Azure.</span><span class="sxs-lookup"><span data-stu-id="29df9-144">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="29df9-145">Pokud již účet nemáte, máte následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="29df9-145">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="29df9-146">[Zdarma otevřít účet Azure](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604) -získáte kredity, můžete použít k vyzkoušení placených služeb Azure a i po jejich použití až můžete účet ponechat a používat bezplatné služby Azure.</span><span class="sxs-lookup"><span data-stu-id="29df9-146">[Open an Azure account for free](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="29df9-147">[Aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -vaše předplatné MSDN vám dává kredity každý měsíc, které můžete použít pro placené služby Azure.</span><span class="sxs-lookup"><span data-stu-id="29df9-147">[Activate MSDN subscriber benefits](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="29df9-148">Další</span><span class="sxs-lookup"><span data-stu-id="29df9-148">Next</span></span>](adding-a-controller.md)
