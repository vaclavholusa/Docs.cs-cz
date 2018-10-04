---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Použití rozhraní Web API 2 s Entity Framework 6 | Dokumentace Microsoftu
author: MikeWasson
description: V tomto kurzu se dozvíte, že se se základy vytváření webovou aplikaci s webovým rozhraním API technologie ASP.NET back-endu. Tento kurz používá Entity Framework 6 pro uspořádání dat...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: d65c0ea35ec766ef9d9093c6502230f9de72a3f3
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795207"
---
<a name="using-web-api-2-with-entity-framework-6"></a><span data-ttu-id="c5417-104">Použití rozhraní Web API 2 s Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="c5417-104">Using Web API 2 with Entity Framework 6</span></span>
====================
<span data-ttu-id="c5417-105">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c5417-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="c5417-106">Stáhnout dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="c5417-106">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

> <span data-ttu-id="c5417-107">V tomto kurzu se dozvíte, že se se základy vytváření webovou aplikaci s webovým rozhraním API technologie ASP.NET back-endu.</span><span class="sxs-lookup"><span data-stu-id="c5417-107">This tutorial will teach you the basics of creating a web application with an ASP.NET Web API back end.</span></span> <span data-ttu-id="c5417-108">Tento kurz používá Entity Framework 6 pro datová vrstva a knihovnou Knockout.js pro aplikace JavaScript na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="c5417-108">The tutorial uses Entity Framework 6 for the data layer, and Knockout.js for the client-side JavaScript application.</span></span> <span data-ttu-id="c5417-109">Tento kurz také ukazuje, jak nasadit aplikaci do Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="c5417-109">The tutorial also shows how to deploy the app to Azure App Service Web Apps.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c5417-110">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="c5417-110">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="c5417-111">Webové rozhraní API 2.1</span><span class="sxs-lookup"><span data-stu-id="c5417-111">Web API 2.1</span></span>
> - <span data-ttu-id="c5417-112">Visual Studio 2013 (stáhněte si Visual Studio 2017 [tady](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span><span class="sxs-lookup"><span data-stu-id="c5417-112">Visual Studio 2013 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="c5417-113">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="c5417-113">Entity Framework 6</span></span>
> - <span data-ttu-id="c5417-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="c5417-114">.NET 4.5</span></span>
> - <span data-ttu-id="c5417-115">[Rozhraní Knockout.js](http://knockoutjs.com/) 3.1</span><span class="sxs-lookup"><span data-stu-id="c5417-115">[Knockout.js](http://knockoutjs.com/) 3.1</span></span>

<span data-ttu-id="c5417-116">Tento kurz používá prostředí ASP.NET Web API 2 s Entity Framework 6 a vytvořte webovou aplikaci, která zpracovává back-end databáze.</span><span class="sxs-lookup"><span data-stu-id="c5417-116">This tutorial uses ASP.NET Web API 2 with Entity Framework 6 to create a web application that manipulates a back-end database.</span></span> <span data-ttu-id="c5417-117">Zde je snímek obrazovky aplikace, kterou vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="c5417-117">Here is a screen shot of the application that you will create.</span></span>

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

<span data-ttu-id="c5417-118">Aplikace používá návrhu jednostránkové aplikace (SPA).</span><span class="sxs-lookup"><span data-stu-id="c5417-118">The app uses a single-page application (SPA) design.</span></span> <span data-ttu-id="c5417-119">"Jednostránkovou aplikaci" je obecný termín pro webovou aplikaci, která načte jednu stránku HTML a následně aktualizuje dynamicky, místo načtení nové stránky na stránku.</span><span class="sxs-lookup"><span data-stu-id="c5417-119">"Single-page application" is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="c5417-120">Po načtení počáteční stránky aplikace komunikuje se serverem přes odesílání požadavků AJAX.</span><span class="sxs-lookup"><span data-stu-id="c5417-120">After the initial page load, the app talks with the server through AJAX requests.</span></span> <span data-ttu-id="c5417-121">AJAX žádosti vrácená data JSON, který aplikace používá k aktualizaci uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c5417-121">The AJAX requests return JSON data, which the app uses to update the UI.</span></span>

<span data-ttu-id="c5417-122">AJAX není novinka, ale ještě dnes existují architektury JavaScriptu, které usnadňují tvorbu a udržování rozsáhlé sofistikované aplikace SPA.</span><span class="sxs-lookup"><span data-stu-id="c5417-122">AJAX isn't new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="c5417-123">Tento kurz používá [knihovnou Knockout.js](http://knockoutjs.com/), ale můžete použít libovolnou architekturu JavaScript klienta.</span><span class="sxs-lookup"><span data-stu-id="c5417-123">This tutorial uses [Knockout.js](http://knockoutjs.com/), but you can use any JavaScript client framework.</span></span>

<span data-ttu-id="c5417-124">Tady jsou hlavní stavební bloky pro tuto aplikaci:</span><span class="sxs-lookup"><span data-stu-id="c5417-124">Here are the main building blocks for this app:</span></span>

- <span data-ttu-id="c5417-125">ASP.NET MVC vytvoří na stránce HTML.</span><span class="sxs-lookup"><span data-stu-id="c5417-125">ASP.NET MVC creates the HTML page.</span></span>
- <span data-ttu-id="c5417-126">Rozhraní ASP.NET Web API zpracovává požadavky AJAX a vrací JSON data.</span><span class="sxs-lookup"><span data-stu-id="c5417-126">ASP.NET Web API handles the AJAX requests and returns JSON data.</span></span>
- <span data-ttu-id="c5417-127">Rozhraní Knockout.js data vazbu elementů HTML JSON data.</span><span class="sxs-lookup"><span data-stu-id="c5417-127">Knockout.js data-binds the HTML elements to the JSON data.</span></span>
- <span data-ttu-id="c5417-128">Entity Framework a k databázi.</span><span class="sxs-lookup"><span data-stu-id="c5417-128">Entity Framework talks to the database.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="c5417-129">Zobrazit tuto aplikaci běžící v Azure</span><span class="sxs-lookup"><span data-stu-id="c5417-129">See this App Running on Azure</span></span>

<span data-ttu-id="c5417-130">Chcete zobrazit dokončené web spuštěný jako živou webovou aplikaci?</span><span class="sxs-lookup"><span data-stu-id="c5417-130">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="c5417-131">Kompletní verze aplikace můžete nasadit ke svému účtu Azure jednoduše kliknutím na následující tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c5417-131">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

<span data-ttu-id="c5417-132">Potřebujete účet Azure k nasazení tohoto řešení do Azure.</span><span class="sxs-lookup"><span data-stu-id="c5417-132">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="c5417-133">Pokud ještě nemáte účet, máte následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="c5417-133">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="c5417-134">[Zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) – budete dostávat kredity můžete použít k vyzkoušení placených služeb Azure a dokonce i po jejich použití až můžete účet ponechat a dál používat bezplatné služby Azure.</span><span class="sxs-lookup"><span data-stu-id="c5417-134">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="c5417-135">[Aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) – vaše předplatné MSDN vám dává kredity každý měsíc, můžete použít k placení za služby Azure.</span><span class="sxs-lookup"><span data-stu-id="c5417-135">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="c5417-136">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="c5417-136">Create the Project</span></span>

<span data-ttu-id="c5417-137">Otevřít Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c5417-137">Open Visual Studio.</span></span> <span data-ttu-id="c5417-138">Z **souboru** nabídce vyberte možnost **nový**a pak vyberte **projektu**.</span><span class="sxs-lookup"><span data-stu-id="c5417-138">From the **File** menu, select **New**, then select **Project**.</span></span> <span data-ttu-id="c5417-139">(Nebo klikněte na tlačítko **nový projekt** na úvodní stránce.)</span><span class="sxs-lookup"><span data-stu-id="c5417-139">(Or click **New Project** on the Start page.)</span></span>

<span data-ttu-id="c5417-140">V **nový projekt** dialogového okna, klikněte na tlačítko **webové** v levém podokně a **webová aplikace ASP.NET** v prostředním podokně.</span><span class="sxs-lookup"><span data-stu-id="c5417-140">In the **New Project** dialog, click **Web** in the left pane and **ASP.NET Web Application** in the middle pane.</span></span> <span data-ttu-id="c5417-141">Pojmenujte projekt BookService a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="c5417-141">Name the project BookService and click **OK**.</span></span>

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

<span data-ttu-id="c5417-142">V **nový projekt ASP.NET** dialogového okna, vyberte **webového rozhraní API** šablony.</span><span class="sxs-lookup"><span data-stu-id="c5417-142">In the **New ASP.NET Project** dialog, select the **Web API** template.</span></span>

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

<span data-ttu-id="c5417-143">Pokud chcete k hostování projektu ve službě Azure App Service, nechat **hostovat v cloudu** zaškrtnuté políčko.</span><span class="sxs-lookup"><span data-stu-id="c5417-143">If you want to host the project in a Azure App Service, leave the **Host in the cloud** box checked.</span></span>

<span data-ttu-id="c5417-144">Klikněte na tlačítko **OK** pro vytvoření projektu.</span><span class="sxs-lookup"><span data-stu-id="c5417-144">Click **OK** to create the project.</span></span>

## <a name="configure-azure-settings-optional"></a><span data-ttu-id="c5417-145">Konfigurace nastavení služby Azure (volitelné)</span><span class="sxs-lookup"><span data-stu-id="c5417-145">Configure Azure Settings (Optional)</span></span>

<span data-ttu-id="c5417-146">Pokud jste nechali **hostitel v cloudu** zaškrtnutá možnost, sada Visual Studio vás vyzve k přihlášení k Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="c5417-146">If you left the **Host in Cloud** option checked, Visual Studio will prompt you to sign in to Microsoft Azure</span></span>

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

<span data-ttu-id="c5417-147">Po přihlášení do Azure, Visual Studio vás vyzve k konfigurace webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c5417-147">After you sign in to Azure, Visual Studio prompts you to configure the web app.</span></span> <span data-ttu-id="c5417-148">Zadejte název lokality, vyberte své předplatné Azure a vyberte zeměpisnou oblast.</span><span class="sxs-lookup"><span data-stu-id="c5417-148">Enter a name for the site, select your Azure subscription, and select a geographical region.</span></span> <span data-ttu-id="c5417-149">V části **databázový server**vyberte **vytvořit nový server**.</span><span class="sxs-lookup"><span data-stu-id="c5417-149">Under **Database server**, select **Create new server**.</span></span> <span data-ttu-id="c5417-150">Zadejte uživatelské jméno správce a heslo.</span><span class="sxs-lookup"><span data-stu-id="c5417-150">Enter an administrator username and password.</span></span>

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="c5417-151">Next</span><span class="sxs-lookup"><span data-stu-id="c5417-151">Next</span></span>](part-2.md)
