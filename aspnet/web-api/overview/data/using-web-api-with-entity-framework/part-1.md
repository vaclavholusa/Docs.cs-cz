---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Použití rozhraní Web API 2 s platformou Entity Framework 6 | Microsoft Docs
author: MikeWasson
description: V tomto kurzu se naučit, že základní informace o vytvoření webové aplikace pomocí rozhraní ASP.NET Web API back-end. Tento kurz používá Entity Framework 6 pro uspořádání dat...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 8e6d381509a121e3036ca3af91ea3b9bd0be33c2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871971"
---
<a name="using-web-api-2-with-entity-framework-6"></a><span data-ttu-id="05cdd-104">Použití rozhraní Web API 2 s platformou Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="05cdd-104">Using Web API 2 with Entity Framework 6</span></span>
====================
<span data-ttu-id="05cdd-105">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="05cdd-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="05cdd-106">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="05cdd-106">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

> <span data-ttu-id="05cdd-107">V tomto kurzu se naučit, že základní informace o vytvoření webové aplikace pomocí rozhraní ASP.NET Web API back-end.</span><span class="sxs-lookup"><span data-stu-id="05cdd-107">This tutorial will teach you the basics of creating a web application with an ASP.NET Web API back end.</span></span> <span data-ttu-id="05cdd-108">Tento kurz používá Entity Framework 6 Datová vrstva a Knockout.js aplikace JavaScript na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="05cdd-108">The tutorial uses Entity Framework 6 for the data layer, and Knockout.js for the client-side JavaScript application.</span></span> <span data-ttu-id="05cdd-109">Tento kurz také ukazuje, jak nasadit aplikace do Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="05cdd-109">The tutorial also shows how to deploy the app to Azure App Service Web Apps.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="05cdd-110">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="05cdd-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="05cdd-111">Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="05cdd-111">Web API 2.1</span></span>
> - [<span data-ttu-id="05cdd-112">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="05cdd-112">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="05cdd-113">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="05cdd-113">Entity Framework 6</span></span>
> - <span data-ttu-id="05cdd-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="05cdd-114">.NET 4.5</span></span>
> - <span data-ttu-id="05cdd-115">[Knockout.js](http://knockoutjs.com/) 3.1</span><span class="sxs-lookup"><span data-stu-id="05cdd-115">[Knockout.js](http://knockoutjs.com/) 3.1</span></span>


<span data-ttu-id="05cdd-116">Tento kurz používá k vytvoření webové aplikace, která zpracovává databázi back-end ASP.NET Web API 2 s Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="05cdd-116">This tutorial uses ASP.NET Web API 2 with Entity Framework 6 to create a web application that manipulates a back-end database.</span></span> <span data-ttu-id="05cdd-117">Zde je snímek obrazovky aplikace, které vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="05cdd-117">Here is a screen shot of the application that you will create.</span></span>

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

<span data-ttu-id="05cdd-118">Aplikace používá návrh jednostránkové aplikace (SPA).</span><span class="sxs-lookup"><span data-stu-id="05cdd-118">The app uses a single-page application (SPA) design.</span></span> <span data-ttu-id="05cdd-119">"Jednostránkové aplikace" je obecný termín pro webovou aplikaci, která načte jediné stránce HTML a pak aktualizuje dynamicky, místo načítání stránky, nové stránky.</span><span class="sxs-lookup"><span data-stu-id="05cdd-119">"Single-page application" is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="05cdd-120">Po úvodní stránky zatížení aplikace komunikuje se serverem prostřednictvím požadavky AJAX.</span><span class="sxs-lookup"><span data-stu-id="05cdd-120">After the initial page load, the app talks with the server through AJAX requests.</span></span> <span data-ttu-id="05cdd-121">AJAX požadavků načíst data JSON, které aplikace používá k aktualizaci uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="05cdd-121">The AJAX requests return JSON data, which the app uses to update the UI.</span></span>

<span data-ttu-id="05cdd-122">AJAX není nový, ale existují ještě dnes JavaScript rozhraní, které usnadňují tvorbu a udržování rozsáhlé sofistikované SPA aplikace.</span><span class="sxs-lookup"><span data-stu-id="05cdd-122">AJAX isn't new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="05cdd-123">Tento kurz používá [Knockout.js](http://knockoutjs.com/), ale můžete použít libovolnou architekturu JavaScript klienta.</span><span class="sxs-lookup"><span data-stu-id="05cdd-123">This tutorial uses [Knockout.js](http://knockoutjs.com/), but you can use any JavaScript client framework.</span></span>

<span data-ttu-id="05cdd-124">Zde jsou hlavní stavební bloky pro tuto aplikaci:</span><span class="sxs-lookup"><span data-stu-id="05cdd-124">Here are the main building blocks for this app:</span></span>

- <span data-ttu-id="05cdd-125">ASP.NET MVC vytvoří stránku HTML.</span><span class="sxs-lookup"><span data-stu-id="05cdd-125">ASP.NET MVC creates the HTML page.</span></span>
- <span data-ttu-id="05cdd-126">Rozhraní ASP.NET Web API zpracovává požadavky AJAX a vrací JSON data.</span><span class="sxs-lookup"><span data-stu-id="05cdd-126">ASP.NET Web API handles the AJAX requests and returns JSON data.</span></span>
- <span data-ttu-id="05cdd-127">Knockout.js data vazby elementů HTML pro JSON data.</span><span class="sxs-lookup"><span data-stu-id="05cdd-127">Knockout.js data-binds the HTML elements to the JSON data.</span></span>
- <span data-ttu-id="05cdd-128">Rozhraní Entity Framework komunikuje do databáze.</span><span class="sxs-lookup"><span data-stu-id="05cdd-128">Entity Framework talks to the database.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="05cdd-129">Tato aplikace spuštěné v Azure</span><span class="sxs-lookup"><span data-stu-id="05cdd-129">See this App Running on Azure</span></span>

<span data-ttu-id="05cdd-130">Chcete zobrazit dokončení web spuštěný jako živou webovou aplikaci?</span><span class="sxs-lookup"><span data-stu-id="05cdd-130">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="05cdd-131">Úplnou verzi aplikace můžete nasadit ke svému účtu Azure jednoduše kliknutím na následující tlačítko.</span><span class="sxs-lookup"><span data-stu-id="05cdd-131">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

<span data-ttu-id="05cdd-132">Potřebujete účet Azure k nasazení tohoto řešení do Azure.</span><span class="sxs-lookup"><span data-stu-id="05cdd-132">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="05cdd-133">Pokud již účet nemáte, máte následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="05cdd-133">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="05cdd-134">[Zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -získáte kredity, můžete použít k vyzkoušení placených služeb Azure a i po jejich použití až můžete účet ponechat a používat bezplatné služby Azure.</span><span class="sxs-lookup"><span data-stu-id="05cdd-134">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="05cdd-135">[Aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -vaše předplatné MSDN vám dává kredity každý měsíc, které můžete použít pro placené služby Azure.</span><span class="sxs-lookup"><span data-stu-id="05cdd-135">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="05cdd-136">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="05cdd-136">Create the Project</span></span>

<span data-ttu-id="05cdd-137">Otevřete Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="05cdd-137">Open Visual Studio.</span></span> <span data-ttu-id="05cdd-138">Z **soubor** nabídce vyberte možnost **nový**, pak vyberte **projektu**.</span><span class="sxs-lookup"><span data-stu-id="05cdd-138">From the **File** menu, select **New**, then select **Project**.</span></span> <span data-ttu-id="05cdd-139">(Nebo klikněte na tlačítko **nový projekt** na stránce Start.)</span><span class="sxs-lookup"><span data-stu-id="05cdd-139">(Or click **New Project** on the Start page.)</span></span>

<span data-ttu-id="05cdd-140">V **nový projekt** dialogové okno, klikněte na tlačítko **webové** v levém podokně a **webové aplikace ASP.NET** v prostředním podokně.</span><span class="sxs-lookup"><span data-stu-id="05cdd-140">In the **New Project** dialog, click **Web** in the left pane and **ASP.NET Web Application** in the middle pane.</span></span> <span data-ttu-id="05cdd-141">Název projektu BookService a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="05cdd-141">Name the project BookService and click **OK**.</span></span>

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

<span data-ttu-id="05cdd-142">V **nový projekt ASP.NET** dialogovém okně, vyberte **webového rozhraní API** šablony.</span><span class="sxs-lookup"><span data-stu-id="05cdd-142">In the **New ASP.NET Project** dialog, select the **Web API** template.</span></span>

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

<span data-ttu-id="05cdd-143">Pokud chcete k hostování projektu v Azure App Service, ponechte **hostitel v cloudu** zaškrtnutým políčkem.</span><span class="sxs-lookup"><span data-stu-id="05cdd-143">If you want to host the project in a Azure App Service, leave the **Host in the cloud** box checked.</span></span>

<span data-ttu-id="05cdd-144">Klikněte na tlačítko **OK** a vytvořte tak projekt.</span><span class="sxs-lookup"><span data-stu-id="05cdd-144">Click **OK** to create the project.</span></span>

## <a name="configure-azure-settings-optional"></a><span data-ttu-id="05cdd-145">Konfigurace nastavení Azure (volitelné)</span><span class="sxs-lookup"><span data-stu-id="05cdd-145">Configure Azure Settings (Optional)</span></span>

<span data-ttu-id="05cdd-146">Pokud jste nechali **hostitel v cloudu** zaškrtnutá možnost Visual Studio zobrazí výzvu k přihlášení do služby Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="05cdd-146">If you left the **Host in Cloud** option checked, Visual Studio will prompt you to sign in to Microsoft Azure</span></span>

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

<span data-ttu-id="05cdd-147">Po přihlášení do Azure, Visual Studio zobrazí výzvu k konfigurace webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="05cdd-147">After you sign in to Azure, Visual Studio prompts you to configure the web app.</span></span> <span data-ttu-id="05cdd-148">Zadejte název lokality, vyberte předplatné Azure a vyberte zeměpisnou oblast.</span><span class="sxs-lookup"><span data-stu-id="05cdd-148">Enter a name for the site, select your Azure subscription, and select a geographical region.</span></span> <span data-ttu-id="05cdd-149">V části **databázový server**, vyberte **vytvořit nový server**.</span><span class="sxs-lookup"><span data-stu-id="05cdd-149">Under **Database server**, select **Create new server**.</span></span> <span data-ttu-id="05cdd-150">Zadejte uživatelské jméno správce a heslo.</span><span class="sxs-lookup"><span data-stu-id="05cdd-150">Enter an administrator username and password.</span></span>

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="05cdd-151">Next</span><span class="sxs-lookup"><span data-stu-id="05cdd-151">Next</span></span>](part-2.md)
