---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: "Vlastní akce filtrech rozhraní ASP.NET MVC 4 | Microsoft Docs"
author: rick-anderson
description: "ASP.NET MVC poskytuje filtry akce pro provádění filtrování logiku před i po zavolání metody akce. Zadaná vlastní atributy jsou filtry akce..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: 6362f0506934ca3b3cc86e1a927af75e7bc4e1d3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4-custom-action-filters"></a><span data-ttu-id="05af9-104">Vlastní akce filtrech rozhraní ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="05af9-104">ASP.NET MVC 4 Custom Action Filters</span></span>
====================
<span data-ttu-id="05af9-105">podle [webové táborech Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="05af9-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="05af9-106">ASP.NET MVC poskytuje filtry akce pro provádění filtrování logiku před i po zavolání metody akce.</span><span class="sxs-lookup"><span data-stu-id="05af9-106">ASP.NET MVC provides Action Filters for executing filtering logic either before or after an action method is called.</span></span> <span data-ttu-id="05af9-107">Filtry akce jsou vlastní atributy, které poskytují deklarativní způsob, jak přidat chování akce před a po akce do metody akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="05af9-107">Action Filters are custom attributes that provide declarative means to add pre-action and post-action behavior to the controller's action methods.</span></span>
> 
> <span data-ttu-id="05af9-108">V tomto testovacím prostředí Hands-on vytvoříte vlastní akce atribut filtru do řešení MvcMusicStore catch požadavky řadiče a protokolovat činnost serveru do databázové tabulky.</span><span class="sxs-lookup"><span data-stu-id="05af9-108">In this Hands-on Lab you will create a custom action filter attribute into MvcMusicStore solution to catch controller's requests and log the activity of a site into a database table.</span></span> <span data-ttu-id="05af9-109">Bude moct přidat filtr protokolování injektáží do jakékoli kontroler nebo akce.</span><span class="sxs-lookup"><span data-stu-id="05af9-109">You will be able to add your logging filter by injection to any controller or action.</span></span> <span data-ttu-id="05af9-110">Nakonec zobrazí se zobrazení protokolu, který zobrazuje seznam návštěvníky.</span><span class="sxs-lookup"><span data-stu-id="05af9-110">Finally, you will see the log view that shows the list of visitors.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="05af9-111">Toto testovací prostředí Hands-on předpokládá, že máte základní znalosti o **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="05af9-111">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="05af9-112">Pokud jste nepoužili **ASP.NET MVC** před, doporučujeme si projít **ASP.NET MVC 4 Základy** Hands-on testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="05af9-112">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="05af9-113">Cíle</span><span class="sxs-lookup"><span data-stu-id="05af9-113">Objectives</span></span>

<span data-ttu-id="05af9-114">V tomto testovacím prostředí Hands-On se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="05af9-114">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="05af9-115">Vytvořit vlastní akce atribut filtru rozšířit možnosti filtrování</span><span class="sxs-lookup"><span data-stu-id="05af9-115">Create a custom action filter attribute to extend filtering capabilities</span></span>
- <span data-ttu-id="05af9-116">Použijte vlastní atribut filtru tak, že vkládání konkrétní úroveň</span><span class="sxs-lookup"><span data-stu-id="05af9-116">Apply a custom filter attribute by injection to a specific level</span></span>
- <span data-ttu-id="05af9-117">Zaregistrovat globální filtry vlastní akce</span><span class="sxs-lookup"><span data-stu-id="05af9-117">Register a custom action filters globally</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="05af9-118">Požadavky</span><span class="sxs-lookup"><span data-stu-id="05af9-118">Prerequisites</span></span>

<span data-ttu-id="05af9-119">Musíte mít následující položky k dokončení tohoto testovacího prostředí:</span><span class="sxs-lookup"><span data-stu-id="05af9-119">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="05af9-120">[Microsoft Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) nebo i vyšší (čtení [příloha A](#AppendixA) pokyny o tom, jak ji nainstalovat).</span><span class="sxs-lookup"><span data-stu-id="05af9-120">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="05af9-121">Instalace</span><span class="sxs-lookup"><span data-stu-id="05af9-121">Setup</span></span>

<span data-ttu-id="05af9-122">**Instalace fragmenty kódu**</span><span class="sxs-lookup"><span data-stu-id="05af9-122">**Installing Code Snippets**</span></span>

<span data-ttu-id="05af9-123">Pro usnadnění práce většinu kódu, který bude spravovat podél toto testovací prostředí je k dispozici jako fragmenty kódu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="05af9-123">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="05af9-124">K instalaci fragmenty kódu spustit **.\Source\Setup\CodeSnippets.vsi** souboru.</span><span class="sxs-lookup"><span data-stu-id="05af9-124">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="05af9-125">Pokud si nejste obeznámeni s fragmenty kódu Visual Studio a chcete se dozvíte, jak používat, můžete odkazovat na příloze z tohoto dokumentu &quot; [fragmenty kódu pomocí příloha C:](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="05af9-125">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="05af9-126">Cvičení</span><span class="sxs-lookup"><span data-stu-id="05af9-126">Exercises</span></span>

<span data-ttu-id="05af9-127">Toto testovací prostředí Hands-On se skládá ve cvičeních následující:</span><span class="sxs-lookup"><span data-stu-id="05af9-127">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="05af9-128">Cvičení 1: Protokolování akce</span><span class="sxs-lookup"><span data-stu-id="05af9-128">Exercise 1: Logging actions</span></span>](#Exercise1)
2. [<span data-ttu-id="05af9-129">Cvičení 2: Správa více filtrů Akce</span><span class="sxs-lookup"><span data-stu-id="05af9-129">Exercise 2: Managing Multiple Action Filters</span></span>](#Exercise2)

<span data-ttu-id="05af9-130">Odhadovaný čas dokončení tohoto testovacího prostředí: **30 minut**.</span><span class="sxs-lookup"><span data-stu-id="05af9-130">Estimated time to complete this lab: **30 minutes**.</span></span>

> [!NOTE]
> <span data-ttu-id="05af9-131">Každý úkol je přiložena **End** složku obsahující výsledné řešení by si měly opatřit po dokončení cvičeních.</span><span class="sxs-lookup"><span data-stu-id="05af9-131">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="05af9-132">Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc při procházení cvičeních.</span><span class="sxs-lookup"><span data-stu-id="05af9-132">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a><span data-ttu-id="05af9-133">Cvičení 1: Protokolování akce</span><span class="sxs-lookup"><span data-stu-id="05af9-133">Exercise 1: Logging Actions</span></span>

<span data-ttu-id="05af9-134">V tomto cvičení se dozvíte, jak vytvořit vlastní akce filtru protokolu s použitím technologie ASP.NET MVC 4 filtru zprostředkovatelů.</span><span class="sxs-lookup"><span data-stu-id="05af9-134">In this exercise, you will learn how to create a custom action log filter by using ASP.NET MVC 4 Filter Providers.</span></span> <span data-ttu-id="05af9-135">K tomuto účelu použijete filtr protokolování MusicStore lokalitu, která bude zaznamenávat všechny aktivity v vybrané řadiče.</span><span class="sxs-lookup"><span data-stu-id="05af9-135">For that purpose you will apply a logging filter to the MusicStore site that will record all the activities in the selected controllers.</span></span>

<span data-ttu-id="05af9-136">Filtr se rozšířit **ActionFilterAttributeClass** a přepsání **OnActionExecuting** metoda catch každého požadavku a provádět akce protokolování.</span><span class="sxs-lookup"><span data-stu-id="05af9-136">The filter will extend **ActionFilterAttributeClass** and override **OnActionExecuting** method to catch each request and then perform the logging actions.</span></span> <span data-ttu-id="05af9-137">Kontextové informace o požadavcích HTTP, provádění metody, výsledky a parametry budou poskytovat ASP.NET MVC **ActionExecutingContext** třída **.**</span><span class="sxs-lookup"><span data-stu-id="05af9-137">The context information about HTTP requests, executing methods, results and parameters will be provided by ASP.NET MVC **ActionExecutingContext** class **.**</span></span>

> [!NOTE]
> <span data-ttu-id="05af9-138">ASP.NET MVC 4 má také výchozí zprostředkovatele filtrů, které můžete použít bez vytvoření vlastního filtru.</span><span class="sxs-lookup"><span data-stu-id="05af9-138">ASP.NET MVC 4 also has default filters providers you can use without creating a custom filter.</span></span> <span data-ttu-id="05af9-139">ASP.NET MVC 4 poskytuje následující typy filtrů:</span><span class="sxs-lookup"><span data-stu-id="05af9-139">ASP.NET MVC 4 provides the following types of filters:</span></span>
> 
> - <span data-ttu-id="05af9-140">**Autorizace** filtrovat, takže zabezpečení rozhodnutí o tom, jestli spuštění metody akce, jako je například ověřování nebo ověřování vlastnosti požadavku.</span><span class="sxs-lookup"><span data-stu-id="05af9-140">**Authorization** filter, which makes security decisions about whether to execute an action method, such as performing authentication or validating properties of the request.</span></span>
> - <span data-ttu-id="05af9-141">**Akce** filtr, který zabalí provádění metody akce.</span><span class="sxs-lookup"><span data-stu-id="05af9-141">**Action** filter, which wraps the action method execution.</span></span> <span data-ttu-id="05af9-142">Tento filtr můžete provádět další zpracování, např. poskytuje doplňující data na metody akce, kontrola návratovou hodnotu nebo zrušení provádění metody akce</span><span class="sxs-lookup"><span data-stu-id="05af9-142">This filter can perform additional processing, such as providing extra data to the action method, inspecting the return value, or canceling execution of the action method</span></span>
> - <span data-ttu-id="05af9-143">**Výsledek** filtr, který zabalí provádění objektu ActionResult.</span><span class="sxs-lookup"><span data-stu-id="05af9-143">**Result** filter, which wraps execution of the ActionResult object.</span></span> <span data-ttu-id="05af9-144">Tento filtr můžete provést další zpracování výsledku, jako je například úprava odpověď HTTP.</span><span class="sxs-lookup"><span data-stu-id="05af9-144">This filter can perform additional processing of the result, such as modifying the HTTP response.</span></span>
> - <span data-ttu-id="05af9-145">**Výjimka** filtr, který provede, pokud dojde k neošetřené výjimce vyvolána někde v metodě akce, počínaje filtry autorizace a konče provádění výsledku.</span><span class="sxs-lookup"><span data-stu-id="05af9-145">**Exception** filter, which executes if there is an unhandled exception thrown somewhere in action method, starting with the authorization filters and ending with the execution of the result.</span></span> <span data-ttu-id="05af9-146">Filtry výjimek lze použít pro úlohy, jako je protokolování nebo zobrazení chybovou stránku.</span><span class="sxs-lookup"><span data-stu-id="05af9-146">Exception filters can be used for tasks such as logging or displaying an error page.</span></span>
> 
> <span data-ttu-id="05af9-147">Další informace o poskytovatelích filtry navštivte tento odkaz MSDN: ([https://msdn.microsoft.com/en-us/library/dd410209.aspx](https://msdn.microsoft.com/en-us/library/dd410209.aspx)).</span><span class="sxs-lookup"><span data-stu-id="05af9-147">For more information about Filters Providers please visit this MSDN link: ([https://msdn.microsoft.com/en-us/library/dd410209.aspx](https://msdn.microsoft.com/en-us/library/dd410209.aspx)) .</span></span>


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a><span data-ttu-id="05af9-148">O funkci protokolování aplikace MVC Hudba úložiště</span><span class="sxs-lookup"><span data-stu-id="05af9-148">About MVC Music Store Application logging feature</span></span>

<span data-ttu-id="05af9-149">Toto řešení úložiště Hudba má do nové tabulky datového modelu pro protokolování serveru **ActionLog**, s následující pole: název kontroleru, který přijal žádost, volané akce, IP adresa klienta a časové razítko.</span><span class="sxs-lookup"><span data-stu-id="05af9-149">This Music Store solution has a new data model table for site logging, **ActionLog**, with the following fields: Name of the controller that received a request, Called action, Client IP and Time stamp.</span></span>

<span data-ttu-id="05af9-150">![Datový model. Tabulka ActionLog. ] (aspnet-mvc-4-custom-action-filters/_static/image1.png "Datového modelu. Tabulka ActionLog.")</span><span class="sxs-lookup"><span data-stu-id="05af9-150">![Data model. ActionLog table.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Data model. ActionLog table.")</span></span>

<span data-ttu-id="05af9-151">*Datový model - ActionLog tabulky*</span><span class="sxs-lookup"><span data-stu-id="05af9-151">*Data model - ActionLog table*</span></span>

<span data-ttu-id="05af9-152">Řešení poskytuje pro protokol akcí, který se nachází v zobrazení ASP.NET MVC **MvcMusicStores nebo zobrazení/ActionLog**:</span><span class="sxs-lookup"><span data-stu-id="05af9-152">The solution provides an ASP.NET MVC View for the Action log that can be found at **MvcMusicStores/Views/ActionLog**:</span></span>

<span data-ttu-id="05af9-153">![Akce protokolu zobrazení](aspnet-mvc-4-custom-action-filters/_static/image2.png "zobrazení protokolu akcí")</span><span class="sxs-lookup"><span data-stu-id="05af9-153">![Action Log view](aspnet-mvc-4-custom-action-filters/_static/image2.png "Action Log view")</span></span>

<span data-ttu-id="05af9-154">*Zobrazení protokolu akcí*</span><span class="sxs-lookup"><span data-stu-id="05af9-154">*Action Log view*</span></span>

<span data-ttu-id="05af9-155">S tímto zadané struktura všechny práce se zaměří na přerušení řadiče požadavku a provedení protokolování pomocí vlastní filtrování.</span><span class="sxs-lookup"><span data-stu-id="05af9-155">With this given structure, all the work will be focused on interrupting controller's request and performing the logging by using custom filtering.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a><span data-ttu-id="05af9-156">Úloha 1 – Vytvoření vlastního filtru k zachycení žádosti řadiče</span><span class="sxs-lookup"><span data-stu-id="05af9-156">Task 1 - Creating a Custom Filter to Catch a Controller's Request</span></span>

<span data-ttu-id="05af9-157">V této úloze vytvoříte třídu atributu vlastního filtru, která bude obsahovat logiku protokolování.</span><span class="sxs-lookup"><span data-stu-id="05af9-157">In this task you will create a custom filter attribute class that will contain the logging logic.</span></span> <span data-ttu-id="05af9-158">K tomuto účelu rozšíříte ASP.NET MVC **ActionFilterAttribute** třídy a implementace rozhraní **IActionFilter**.</span><span class="sxs-lookup"><span data-stu-id="05af9-158">For that purpose you will extend ASP.NET MVC **ActionFilterAttribute** Class and implement the interface **IActionFilter**.</span></span>

> [!NOTE]
> <span data-ttu-id="05af9-159">**ActionFilterAttribute** je základní třída pro všechny filtry atribut.</span><span class="sxs-lookup"><span data-stu-id="05af9-159">The **ActionFilterAttribute** is the base class for all the attribute filters.</span></span> <span data-ttu-id="05af9-160">Poskytuje následující metody, které spustit určitou logiku po a před spuštěním akce kontroleru:</span><span class="sxs-lookup"><span data-stu-id="05af9-160">It provides the following methods to execute a specific logic after and before controller action's execution:</span></span>
> 
> - <span data-ttu-id="05af9-161">**OnActionExecuting**(ActionExecutingContext filterContext): těsně před akci metoda je volána.</span><span class="sxs-lookup"><span data-stu-id="05af9-161">**OnActionExecuting**(ActionExecutingContext filterContext): Just before the action method is called.</span></span>
> - <span data-ttu-id="05af9-162">**OnActionExecuted**(ActionExecutedContext filterContext): po zavolání metody akce a před provedením výsledku (před vykreslením zobrazení).</span><span class="sxs-lookup"><span data-stu-id="05af9-162">**OnActionExecuted**(ActionExecutedContext filterContext): After the action method is called and before the result is executed (before view render).</span></span>
> - <span data-ttu-id="05af9-163">**OnResultExecuting**(ResultExecutingContext filterContext): těsně před je výsledek spuštěn (před vykreslením zobrazení).</span><span class="sxs-lookup"><span data-stu-id="05af9-163">**OnResultExecuting**(ResultExecutingContext filterContext): Just before the result is executed (before view render).</span></span>
> - <span data-ttu-id="05af9-164">**OnResultExecuted**(ResultExecutedContext filterContext): Po provedení výsledku (po je zobrazení vykresleno).</span><span class="sxs-lookup"><span data-stu-id="05af9-164">**OnResultExecuted**(ResultExecutedContext filterContext): After the result is executed (after the view is rendered).</span></span>
> 
> <span data-ttu-id="05af9-165">Pomocí některé z těchto metod přepsání v odvozené třídě, můžete provést filtrování kódu.</span><span class="sxs-lookup"><span data-stu-id="05af9-165">By overriding any of these methods into a derived class, you can execute your own filtering code.</span></span>


1. <span data-ttu-id="05af9-166">Otevřete **začít** řešení nacházející se v **\Source\Ex01-LoggingActions\Begin** složky.</span><span class="sxs-lookup"><span data-stu-id="05af9-166">Open the **Begin** solution located at **\Source\Ex01-LoggingActions\Begin** folder.</span></span>

    1. <span data-ttu-id="05af9-167">Musíte se ke stažení některé chybějící balíčky NuGet, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="05af9-167">You will need to download some missing NuGet packages before you continue.</span></span> <span data-ttu-id="05af9-168">Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="05af9-168">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="05af9-169">V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="05af9-169">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="05af9-170">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="05af9-170">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="05af9-171">Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="05af9-171">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="05af9-172">Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="05af9-172">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="05af9-173">Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="05af9-173">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
    > 
    > <span data-ttu-id="05af9-174">Další informace najdete v tomto článku: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="05af9-174">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="05af9-175">Přidejte novou C# třídu do **filtry** složku a pojmenujte ji *CustomActionFilter.cs*.</span><span class="sxs-lookup"><span data-stu-id="05af9-175">Add a new C# class into the **Filters** folder and name it *CustomActionFilter.cs*.</span></span> <span data-ttu-id="05af9-176">Tato složka se uloží všechny vlastní filtry.</span><span class="sxs-lookup"><span data-stu-id="05af9-176">This folder will store all the custom filters.</span></span>
3. <span data-ttu-id="05af9-177">Otevřete **CustomActionFilter.cs** a přidejte odkaz na **System.Web.Mvc** a **MvcMusicStore.Models** oborů názvů:</span><span class="sxs-lookup"><span data-stu-id="05af9-177">Open **CustomActionFilter.cs** and add a reference to **System.Web.Mvc** and **MvcMusicStore.Models** namespaces:</span></span>

    <span data-ttu-id="05af9-178">(Code fragment kódu - *vlastní akce filtrech rozhraní ASP.NET MVC 4 - Ex1 CustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="05af9-178">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-CustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. <span data-ttu-id="05af9-179">Zdědit **CustomActionFilter** třídy z **ActionFilterAttribute** a pak proveďte **CustomActionFilter** implementaci třídy **IActionFilter** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="05af9-179">Inherit the **CustomActionFilter** class from **ActionFilterAttribute** and then make **CustomActionFilter** class implement **IActionFilter** interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. <span data-ttu-id="05af9-180">Ujistěte se, **CustomActionFilter** třída potlačení metody **OnActionExecuting** a přidejte pomocí potřebné logiky do protokolu jeho spuštění.</span><span class="sxs-lookup"><span data-stu-id="05af9-180">Make **CustomActionFilter** class override the method **OnActionExecuting** and add the necessary logic to log the filter's execution.</span></span> <span data-ttu-id="05af9-181">K tomu, přidejte následující zvýrazněný kód v rámci **CustomActionFilter** třídy.</span><span class="sxs-lookup"><span data-stu-id="05af9-181">To do this, add the following highlighted code within **CustomActionFilter** class.</span></span>

    <span data-ttu-id="05af9-182">(Code fragment kódu - *vlastní akce filtrech rozhraní ASP.NET MVC 4 - Ex1 LoggingActions*)</span><span class="sxs-lookup"><span data-stu-id="05af9-182">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-LoggingActions*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > <span data-ttu-id="05af9-183">**OnActionExecuting** používá metoda **Entity Framework** přidat nové ActionLog registrace.</span><span class="sxs-lookup"><span data-stu-id="05af9-183">**OnActionExecuting** method is using **Entity Framework** to add a new ActionLog register.</span></span> <span data-ttu-id="05af9-184">Vytvoří a vyplní novou instanci entity kontextové informace z **filterContext**.</span><span class="sxs-lookup"><span data-stu-id="05af9-184">It creates and fills a new entity instance with the context information from **filterContext**.</span></span>
    > 
    > <span data-ttu-id="05af9-185">Další informace o **parametrem ControllerContext** třídy v [msdn](https://msdn.microsoft.com/en-us/library/system.web.mvc.controllercontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="05af9-185">You can read more about **ControllerContext** class at [msdn](https://msdn.microsoft.com/en-us/library/system.web.mvc.controllercontext.aspx).</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a><span data-ttu-id="05af9-186">Úloha 2 – vložení kódu interceptoru do třídy Kontroleru úložiště</span><span class="sxs-lookup"><span data-stu-id="05af9-186">Task 2 - Injecting a Code Interceptor into the Store Controller Class</span></span>

<span data-ttu-id="05af9-187">V této úloze přidáte vlastní filtr vložením pro všechny třídy kontroleru a akce kontroleru, které bude do protokolu.</span><span class="sxs-lookup"><span data-stu-id="05af9-187">In this task you will add the custom filter by injecting it to all controller classes and controller actions that will be logged.</span></span> <span data-ttu-id="05af9-188">Pro účely tohoto cvičení bude mít třídy Kontroleru úložiště protokolu.</span><span class="sxs-lookup"><span data-stu-id="05af9-188">For the purpose of this exercise, the Store Controller class will have a log.</span></span>

<span data-ttu-id="05af9-189">Metoda **OnActionExecuting** z **ActionLogFilterAttribute** vlastní filtr se spouští při volání vloženého elementu.</span><span class="sxs-lookup"><span data-stu-id="05af9-189">The method **OnActionExecuting** from **ActionLogFilterAttribute** custom filter runs when an injected element is called.</span></span>

<span data-ttu-id="05af9-190">Je také možné zachytit metoda konkrétní řadič.</span><span class="sxs-lookup"><span data-stu-id="05af9-190">It is also possible to intercept a specific controller method.</span></span>

1. <span data-ttu-id="05af9-191">Otevřete **StoreController** v **MvcMusicStore\Controllers** a přidejte odkaz na **filtry** obor názvů:</span><span class="sxs-lookup"><span data-stu-id="05af9-191">Open the **StoreController** at **MvcMusicStore\Controllers** and add a reference to the **Filters** namespace:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. <span data-ttu-id="05af9-192">Vložení vlastní filtr **CustomActionFilter** do **StoreController** třída přidáním **[CustomActionFilter]** atribut před deklaraci třídy.</span><span class="sxs-lookup"><span data-stu-id="05af9-192">Inject the custom filter **CustomActionFilter** into **StoreController** class by adding **[CustomActionFilter]** attribute before the class declaration.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

    > [!NOTE]
    > <span data-ttu-id="05af9-193">Pokud je filtr vloženy do třídy kontroleru, jsou také vložit všechny jeho akce.</span><span class="sxs-lookup"><span data-stu-id="05af9-193">When a filter is injected into a controller class, all its actions are also injected.</span></span> <span data-ttu-id="05af9-194">Pokud chcete použít filtr pouze pro sadu akcí, budete muset vložit **[CustomActionFilter]** na každém z nich:</span><span class="sxs-lookup"><span data-stu-id="05af9-194">If you would like to apply the filter only for a set of actions, you would have to inject **[CustomActionFilter]** to each one of them:</span></span>
    > 
    > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="05af9-195">Úloha 3 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="05af9-195">Task 3 - Running the Application</span></span>

<span data-ttu-id="05af9-196">V této úloze budete testovat, protokolování filtru funguje.</span><span class="sxs-lookup"><span data-stu-id="05af9-196">In this task, you will test that the logging filter is working.</span></span> <span data-ttu-id="05af9-197">Bude spustit aplikaci a navštívit obchod, a poté zkontroluje protokolu aktivit.</span><span class="sxs-lookup"><span data-stu-id="05af9-197">You will start the application and visit the store, and then you will check logged activities.</span></span>

1. <span data-ttu-id="05af9-198">Stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="05af9-198">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="05af9-199">Přejděte do **/ActionLog** zobrazíte počáteční stav zobrazení protokolu:</span><span class="sxs-lookup"><span data-stu-id="05af9-199">Browse to **/ActionLog** to see log view initial state:</span></span>

    <span data-ttu-id="05af9-200">![Sledování stavu před stránky aktivity protokolu](aspnet-mvc-4-custom-action-filters/_static/image3.png "sledovací modul stavu před stránky aktivity protokolu")</span><span class="sxs-lookup"><span data-stu-id="05af9-200">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image3.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="05af9-201">*Stav sledovací modul protokolu před stránky aktivity*</span><span class="sxs-lookup"><span data-stu-id="05af9-201">*Log tracker status before page activity*</span></span>

    > [!NOTE]
    > <span data-ttu-id="05af9-202">Ve výchozím nastavení je vždy zobrazí jednu položku, který se vygeneruje, když načítání existující žánry pro v nabídce.</span><span class="sxs-lookup"><span data-stu-id="05af9-202">By default, it will always show one item that is generated when retrieving the existing genres for the menu.</span></span>
    > 
    > <span data-ttu-id="05af9-203">Pro účely jednoduchost jsme čištění **ActionLog** tabulky pokaždé, když je aplikace spuštěná, zobrazí pouze protokoly ověřování každý konkrétní úkol.</span><span class="sxs-lookup"><span data-stu-id="05af9-203">For simplicity purposes we're cleaning up the **ActionLog** table each time the application runs so it will only show the logs of each particular task's verification.</span></span>
    > 
    > <span data-ttu-id="05af9-204">Možná budete muset odebrat následující kód z **relace\_spustit** – metoda (v **Global.asax** třída), aby bylo možné uložit Historický protokol pro všechny akce provést v úložišti Řadiče.</span><span class="sxs-lookup"><span data-stu-id="05af9-204">You might need to remove the following code from the **Session\_Start** method (in the **Global.asax** class), in order to save an historical log for all the actions executed within the Store Controller.</span></span>
    > 
    > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. <span data-ttu-id="05af9-205">Klikněte na jednu z **žánry** z nabídky a provádět některé akce, jako je k dispozici album procházení.</span><span class="sxs-lookup"><span data-stu-id="05af9-205">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
4. <span data-ttu-id="05af9-206">Přejděte do **/ActionLog** a pokud je protokol prázdný stiskněte **F5** obnovíte stránku.</span><span class="sxs-lookup"><span data-stu-id="05af9-206">Browse to **/ActionLog** and if the log is empty press **F5** to refresh the page.</span></span> <span data-ttu-id="05af9-207">Zkontrolujte, že byly sledovány vaší návštěvy:</span><span class="sxs-lookup"><span data-stu-id="05af9-207">Check that your visits were tracked:</span></span>

    <span data-ttu-id="05af9-208">![Protokol akcí s aktivitou protokolována](aspnet-mvc-4-custom-action-filters/_static/image4.png "protokolu akcí s aktivitou přihlášení")</span><span class="sxs-lookup"><span data-stu-id="05af9-208">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image4.png "Action log with activity logged")</span></span>

    <span data-ttu-id="05af9-209">*Protokol akcí s aktivitou přihlášení*</span><span class="sxs-lookup"><span data-stu-id="05af9-209">*Action log with activity logged*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a><span data-ttu-id="05af9-210">Cvičení 2: Správa více filtrů Akce</span><span class="sxs-lookup"><span data-stu-id="05af9-210">Exercise 2: Managing Multiple Action Filters</span></span>

<span data-ttu-id="05af9-211">V tomto cvičení přidáte druhý filtr akce vlastní třídu StoreController a definovat konkrétní pořadí, ve kterém bude proveden oba filtry.</span><span class="sxs-lookup"><span data-stu-id="05af9-211">In this exercise you will add a second Custom Action Filter to the StoreController class and define the specific order in which both filters will be executed.</span></span> <span data-ttu-id="05af9-212">Potom aktualizujte kód k registraci globální filtr.</span><span class="sxs-lookup"><span data-stu-id="05af9-212">Then you will update the code to register the filter Globally.</span></span>

<span data-ttu-id="05af9-213">Existují různé možnosti vzít v úvahu při definování pořadí zpracování pro filtry.</span><span class="sxs-lookup"><span data-stu-id="05af9-213">There are different options to take into account when defining the Filters' execution order.</span></span> <span data-ttu-id="05af9-214">Například vlastnost pořadí a oboru pro filtry:</span><span class="sxs-lookup"><span data-stu-id="05af9-214">For example, the Order property and the Filters' scope:</span></span>

<span data-ttu-id="05af9-215">Můžete definovat **oboru** pro každou z filtrů, například může oboru všechny filtry akce ke spuštění v rámci **řadiče oboru**a všechny filtry autorizace ke spuštění **globální obor** .</span><span class="sxs-lookup"><span data-stu-id="05af9-215">You can define a **Scope** for each of the Filters, for example, you could scope all the Action Filters to run within the **Controller Scope**, and all Authorization Filters to run in **Global scope**.</span></span> <span data-ttu-id="05af9-216">Obory mít definovaný provádění pořadí.</span><span class="sxs-lookup"><span data-stu-id="05af9-216">The scopes have a defined execution order.</span></span>

<span data-ttu-id="05af9-217">Kromě toho každý filtr akce má vlastnost pořadí sloužící k určení pořadí spouštění v oboru filtru.</span><span class="sxs-lookup"><span data-stu-id="05af9-217">Additionally, each action filter has an Order property which is used to determine the execution order in the scope of the filter.</span></span>

<span data-ttu-id="05af9-218">Další informace o pořadí spuštění filtrů Akce vlastní, navštivte tohoto článku na webu MSDN: ([https://msdn.microsoft.com/en-us/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/en-us/library/dd381609(v=vs.98).aspx)).</span><span class="sxs-lookup"><span data-stu-id="05af9-218">For more information about Custom Action Filters execution order, please visit this MSDN article: ([https://msdn.microsoft.com/en-us/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/en-us/library/dd381609(v=vs.98).aspx)).</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a><span data-ttu-id="05af9-219">Úloha 1: Vytvoření nové vlastní filtr akce</span><span class="sxs-lookup"><span data-stu-id="05af9-219">Task 1: Creating a new Custom Action Filter</span></span>

<span data-ttu-id="05af9-220">V této úloze vytvoříte nový filtr vlastní akce se vložit do třídy StoreController naučit, jak spravovat pořadí spuštění filtrů.</span><span class="sxs-lookup"><span data-stu-id="05af9-220">In this task, you will create a new Custom Action Filter to inject into the StoreController class, learning how to manage the execution order of the filters.</span></span>

1. <span data-ttu-id="05af9-221">Otevřete **začít** řešení nacházející se v **\Source\Ex02-ManagingMultipleActionFilters\Begin** složky.</span><span class="sxs-lookup"><span data-stu-id="05af9-221">Open the **Begin** solution located at **\Source\Ex02-ManagingMultipleActionFilters\Begin** folder.</span></span> <span data-ttu-id="05af9-222">Jinak, může pokračovat, pomocí **End** řešení získat provedením předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="05af9-222">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="05af9-223">Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="05af9-223">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="05af9-224">Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="05af9-224">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="05af9-225">V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="05af9-225">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="05af9-226">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="05af9-226">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

        > [!NOTE]
        > <span data-ttu-id="05af9-227">Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="05af9-227">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="05af9-228">Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="05af9-228">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="05af9-229">Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="05af9-229">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
        > 
        > <span data-ttu-id="05af9-230">Další informace najdete v tomto článku: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="05af9-230">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="05af9-231">Přidejte novou C# třídu do **filtry** složku a pojmenujte ji *MyNewCustomActionFilter.cs*</span><span class="sxs-lookup"><span data-stu-id="05af9-231">Add a new C# class into the **Filters** folder and name it *MyNewCustomActionFilter.cs*</span></span>
3. <span data-ttu-id="05af9-232">Otevřete **MyNewCustomActionFilter.cs** a přidejte odkaz na **System.Web.Mvc** a **MvcMusicStore.Models** obor názvů:</span><span class="sxs-lookup"><span data-stu-id="05af9-232">Open **MyNewCustomActionFilter.cs** and add a reference to **System.Web.Mvc** and the **MvcMusicStore.Models** namespace:</span></span>

    <span data-ttu-id="05af9-233">(Code fragment kódu - *vlastní akce filtrech rozhraní ASP.NET MVC 4 - Ex2 MyNewCustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="05af9-233">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. <span data-ttu-id="05af9-234">Deklarace třídy výchozí nahraďte následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="05af9-234">Replace the default class declaration with the following code.</span></span>

    <span data-ttu-id="05af9-235">(Code fragment kódu - *vlastní akce filtrech rozhraní ASP.NET MVC 4 - Ex2 MyNewCustomActionFilterClass*)</span><span class="sxs-lookup"><span data-stu-id="05af9-235">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterClass*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="05af9-236">Tato vlastní akce filtru je téměř stejný než ten, který jste vytvořili v předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="05af9-236">This Custom Action Filter is almost the same than the one you created in the previous exercise.</span></span> <span data-ttu-id="05af9-237">Hlavní rozdíl je, že je  *&quot;přihlášení pomocí&quot;*  aktualizováno tato nová třída název pro identifikaci požadovaly filtru atributu zaregistrován v protokolu.</span><span class="sxs-lookup"><span data-stu-id="05af9-237">The main difference is that it has the *&quot;Logged By&quot;* attribute updated with this new class' name to identify wich filter registered the log.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a><span data-ttu-id="05af9-238">Úloha 2: Vložení nové interceptoru kódu do StoreController – třída</span><span class="sxs-lookup"><span data-stu-id="05af9-238">Task 2: Injecting a new Code Interceptor into the StoreController Class</span></span>

<span data-ttu-id="05af9-239">V této úloze přidáte nový vlastní filtr do třídy StoreController a spuštění řešení Chcete-li ověřit, jak oba filtry spolupracují.</span><span class="sxs-lookup"><span data-stu-id="05af9-239">In this task, you will add a new custom filter into the StoreController Class and run the solution to verify how both filters work together.</span></span>

1. <span data-ttu-id="05af9-240">Otevřete **StoreController** třída nacházející se v **MvcMusicStore\Controllers** a vložit nový vlastní filtr **MyNewCustomActionFilter** do  **StoreController** třídy, jako je znázorněno v následujícím kódu.</span><span class="sxs-lookup"><span data-stu-id="05af9-240">Open the **StoreController** class located at **MvcMusicStore\Controllers** and inject the new custom filter **MyNewCustomActionFilter** into **StoreController** class like is shown in the following code.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. <span data-ttu-id="05af9-241">Nyní spusťte aplikaci chcete-li zobrazit, jak tyto dva filtry akce vlastní fungují.</span><span class="sxs-lookup"><span data-stu-id="05af9-241">Now, run the application in order to see how these two Custom Action Filters work.</span></span> <span data-ttu-id="05af9-242">Chcete-li to provést, stiskněte **F5** a počkejte, než aplikaci spustí.</span><span class="sxs-lookup"><span data-stu-id="05af9-242">To do this, press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="05af9-243">Přejděte do **/ActionLog** zobrazíte počáteční stav zobrazení protokolu.</span><span class="sxs-lookup"><span data-stu-id="05af9-243">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="05af9-244">![Sledování stavu před stránky aktivity protokolu](aspnet-mvc-4-custom-action-filters/_static/image5.png "sledovací modul stavu před stránky aktivity protokolu")</span><span class="sxs-lookup"><span data-stu-id="05af9-244">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image5.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="05af9-245">*Stav sledovací modul protokolu před stránky aktivity*</span><span class="sxs-lookup"><span data-stu-id="05af9-245">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="05af9-246">Klikněte na jednu z **žánry** z nabídky a provádět některé akce, jako je k dispozici album procházení.</span><span class="sxs-lookup"><span data-stu-id="05af9-246">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="05af9-247">Zkontrolujte, zda tento čas; vaše návštěvy byly sledovány dvakrát: jednou pro každou z vlastní filtry akce, který jste přidali v **StorageController** třídy.</span><span class="sxs-lookup"><span data-stu-id="05af9-247">Check that this time; your visits were tracked twice: once for each of the Custom Action Filters you added in the **StorageController** class.</span></span>

    <span data-ttu-id="05af9-248">![Protokol akcí s aktivitou protokolována](aspnet-mvc-4-custom-action-filters/_static/image6.png "protokolu akcí s aktivitou přihlášení")</span><span class="sxs-lookup"><span data-stu-id="05af9-248">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image6.png "Action log with activity logged")</span></span>

    <span data-ttu-id="05af9-249">*Protokol akcí s aktivitou přihlášení*</span><span class="sxs-lookup"><span data-stu-id="05af9-249">*Action log with activity logged*</span></span>
6. <span data-ttu-id="05af9-250">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="05af9-250">Close the browser.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a><span data-ttu-id="05af9-251">Úloha 3: Správa řazení filtrů</span><span class="sxs-lookup"><span data-stu-id="05af9-251">Task 3: Managing Filter Ordering</span></span>

<span data-ttu-id="05af9-252">V této úloze se dozvíte, jak spravovat pořadí spuštění filtrů se pomocí určeno pořadí.</span><span class="sxs-lookup"><span data-stu-id="05af9-252">In this task, you will learn how to manage the filters' execution order by using the Order propery.</span></span>

1. <span data-ttu-id="05af9-253">Otevřete **StoreController** třída nacházející se v **MvcMusicStore\Controllers** a zadejte **pořadí** vlastnost v obou filtrů, jako vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="05af9-253">Open the **StoreController** class located at **MvcMusicStore\Controllers** and specify the **Order** property in both filters like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. <span data-ttu-id="05af9-254">Teď zkontrolujte, jak filtry jsou spouštěny v závislosti na hodnotě jeho vlastnost pořadí.</span><span class="sxs-lookup"><span data-stu-id="05af9-254">Now, verify how the filters are executed depending on its Order property's value.</span></span> <span data-ttu-id="05af9-255">Bude zjistíte, že filtr s nejmenší hodnotou pořadí (**CustomActionFilter**) je první ten, který se spustí.</span><span class="sxs-lookup"><span data-stu-id="05af9-255">You will find that the filter with the smallest Order value (**CustomActionFilter**) is the first one that is executed.</span></span> <span data-ttu-id="05af9-256">Stiskněte klávesu **F5** a počkejte, než aplikaci spustí.</span><span class="sxs-lookup"><span data-stu-id="05af9-256">Press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="05af9-257">Přejděte do **/ActionLog** zobrazíte počáteční stav zobrazení protokolu.</span><span class="sxs-lookup"><span data-stu-id="05af9-257">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="05af9-258">![Sledování stavu před stránky aktivity protokolu](aspnet-mvc-4-custom-action-filters/_static/image7.png "sledovací modul stavu před stránky aktivity protokolu")</span><span class="sxs-lookup"><span data-stu-id="05af9-258">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image7.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="05af9-259">*Stav sledovací modul protokolu před stránky aktivity*</span><span class="sxs-lookup"><span data-stu-id="05af9-259">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="05af9-260">Klikněte na jednu z **žánry** z nabídky a provádět některé akce, jako je k dispozici album procházení.</span><span class="sxs-lookup"><span data-stu-id="05af9-260">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="05af9-261">Zkontrolujte, že byly sledovány této doby vaší návštěvy seřazené podle hodnota pořadí pro filtry: **CustomActionFilter** protokoly se první.</span><span class="sxs-lookup"><span data-stu-id="05af9-261">Check that this time, your visits were tracked ordered by the filters' Order value: **CustomActionFilter** logs' first.</span></span>

    <span data-ttu-id="05af9-262">![Protokol akcí s aktivitou protokolována](aspnet-mvc-4-custom-action-filters/_static/image8.png "protokolu akcí s aktivitou přihlášení")</span><span class="sxs-lookup"><span data-stu-id="05af9-262">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image8.png "Action log with activity logged")</span></span>

    <span data-ttu-id="05af9-263">*Protokol akcí s aktivitou přihlášení*</span><span class="sxs-lookup"><span data-stu-id="05af9-263">*Action log with activity logged*</span></span>
6. <span data-ttu-id="05af9-264">Nyní provede aktualizaci hodnota pořadí pro filtry a ověřte, jak se změní pořadí protokolování.</span><span class="sxs-lookup"><span data-stu-id="05af9-264">Now, you will update the Filters' order value and verify how the logging order changes.</span></span> <span data-ttu-id="05af9-265">V **StoreController** třídy, aktualizaci hodnoty pořadí pro filtry jako vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="05af9-265">In the **StoreController** class, update the Filters' Order value like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. <span data-ttu-id="05af9-266">Znovu spusťte aplikaci stisknutím **F5**.</span><span class="sxs-lookup"><span data-stu-id="05af9-266">Run the application again by pressing **F5**.</span></span>
8. <span data-ttu-id="05af9-267">Klikněte na jednu z **žánry** z nabídky a provádět některé akce, jako je k dispozici album procházení.</span><span class="sxs-lookup"><span data-stu-id="05af9-267">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
9. <span data-ttu-id="05af9-268">Zkontrolujte, že tentokrát protokoly vytvořené **MyNewCustomActionFilter** filtru objeví jako první.</span><span class="sxs-lookup"><span data-stu-id="05af9-268">Check that this time, the logs created by **MyNewCustomActionFilter** filter appears first.</span></span>

    <span data-ttu-id="05af9-269">![Protokol akcí s aktivitou protokolována](aspnet-mvc-4-custom-action-filters/_static/image9.png "protokolu akcí s aktivitou přihlášení")</span><span class="sxs-lookup"><span data-stu-id="05af9-269">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image9.png "Action log with activity logged")</span></span>

    <span data-ttu-id="05af9-270">*Protokol akcí s aktivitou přihlášení*</span><span class="sxs-lookup"><span data-stu-id="05af9-270">*Action log with activity logged*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a><span data-ttu-id="05af9-271">Úloha 4: Registrace globální filtry.</span><span class="sxs-lookup"><span data-stu-id="05af9-271">Task 4: Registering Filters Globally</span></span>

<span data-ttu-id="05af9-272">V této úloze budete aktualizovat řešení zaregistrovat nový filtr (**MyNewCustomActionFilter**) jako globální filtr.</span><span class="sxs-lookup"><span data-stu-id="05af9-272">In this task, you will update the solution to register the new filter (**MyNewCustomActionFilter**) as a global filter.</span></span> <span data-ttu-id="05af9-273">Díky tomu se spustí ve všech perfomed akce v aplikaci a ne jenom v StoreController ty, které jsou jako v předchozí úloze.</span><span class="sxs-lookup"><span data-stu-id="05af9-273">By doing this, it will be triggered by all the actions perfomed in the application and not only in the StoreController ones as in the previous task.</span></span>

1. <span data-ttu-id="05af9-274">V **StoreController** třídy, odeberte **[MyNewCustomActionFilter]** atribut a vlastnost pořadí z **[CustomActionFilter]**.</span><span class="sxs-lookup"><span data-stu-id="05af9-274">In **StoreController** class, remove **[MyNewCustomActionFilter]** attribute and the order property from **[CustomActionFilter]**.</span></span> <span data-ttu-id="05af9-275">By měl vypadat jako následující:</span><span class="sxs-lookup"><span data-stu-id="05af9-275">It should look like the following:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. <span data-ttu-id="05af9-276">Otevřete **Global.asax** souborů a vyhledejte **aplikace\_spustit** metoda.</span><span class="sxs-lookup"><span data-stu-id="05af9-276">Open **Global.asax** file and locate the **Application\_Start** method.</span></span> <span data-ttu-id="05af9-277">Všimněte si, že každý thime spuštění aplikace se registrují globální filtry voláním **RegisterGlobalFilters** metoda v rámci **FilterConfig** třídy.</span><span class="sxs-lookup"><span data-stu-id="05af9-277">Notice that each thime the application starts it is registering the global filters by calling **RegisterGlobalFilters** method within **FilterConfig** class.</span></span>

    <span data-ttu-id="05af9-278">![Registrace globálních filtrů v souboru Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "registrace v souboru Global.asax globálních filtrů")</span><span class="sxs-lookup"><span data-stu-id="05af9-278">![Registering Global Filters in Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registering Global Filters in Global.asax")</span></span>

    <span data-ttu-id="05af9-279">*Registrace v souboru Global.asax globálních filtrů*</span><span class="sxs-lookup"><span data-stu-id="05af9-279">*Registering Global Filters in Global.asax*</span></span>
3. <span data-ttu-id="05af9-280">Otevřete **FilterConfig.cs** souboru v rámci **aplikace\_spustit** složky.</span><span class="sxs-lookup"><span data-stu-id="05af9-280">Open **FilterConfig.cs** file within **App\_Start** folder.</span></span>
4. <span data-ttu-id="05af9-281">Přidat odkaz na pomocí System.Web.Mvc; pomocí MvcMusicStore.Filters; obor názvů.</span><span class="sxs-lookup"><span data-stu-id="05af9-281">Add a reference to using System.Web.Mvc; using MvcMusicStore.Filters; namespace.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. <span data-ttu-id="05af9-282">Aktualizace **RegisterGlobalFilters** metoda přidání vlastního filtru.</span><span class="sxs-lookup"><span data-stu-id="05af9-282">Update **RegisterGlobalFilters** method adding your custom filter.</span></span> <span data-ttu-id="05af9-283">Chcete-li to provést, přidejte zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="05af9-283">To do this, add the highlighted code:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. <span data-ttu-id="05af9-284">Spusťte aplikaci stisknutím **F5**.</span><span class="sxs-lookup"><span data-stu-id="05af9-284">Run the application by pressing **F5**.</span></span>
7. <span data-ttu-id="05af9-285">Klikněte na jednu z **žánry** z nabídky a provádět některé akce, jako je k dispozici album procházení.</span><span class="sxs-lookup"><span data-stu-id="05af9-285">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
8. <span data-ttu-id="05af9-286">Zkontrolujte, že nyní **[MyNewCustomActionFilter]** je právě v HomeController a ActionLogController vložit příliš.</span><span class="sxs-lookup"><span data-stu-id="05af9-286">Check that now **[MyNewCustomActionFilter]** is being injected in HomeController and ActionLogController too.</span></span>

    <span data-ttu-id="05af9-287">![Protokol akcí s aktivitou protokolována](aspnet-mvc-4-custom-action-filters/_static/image11.png "protokolu akcí s aktivitou přihlášení")</span><span class="sxs-lookup"><span data-stu-id="05af9-287">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image11.png "Action log with activity logged")</span></span>

    <span data-ttu-id="05af9-288">*Protokol akcí s globální aktivitou přihlášení*</span><span class="sxs-lookup"><span data-stu-id="05af9-288">*Action log with global activity logged*</span></span>

> [!NOTE]
> <span data-ttu-id="05af9-289">Kromě toho můžete nasadit tuto aplikaci do následující weby systému Windows Azure [příloha B: publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="05af9-289">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="05af9-290">Souhrn</span><span class="sxs-lookup"><span data-stu-id="05af9-290">Summary</span></span>

<span data-ttu-id="05af9-291">Provedením tohoto testovacího prostředí Hands-On jste se naučili, jak rozšířit filtr akce k provedení vlastní akce.</span><span class="sxs-lookup"><span data-stu-id="05af9-291">By completing this Hands-On Lab you have learned how to extend an action filter to execute custom actions.</span></span> <span data-ttu-id="05af9-292">Naučili jste postup vložit libovolný filtr k vašim řadičům stránky.</span><span class="sxs-lookup"><span data-stu-id="05af9-292">You have also learned how to inject any filter to your page controllers.</span></span> <span data-ttu-id="05af9-293">Následující koncepty se používaly:</span><span class="sxs-lookup"><span data-stu-id="05af9-293">The following concepts were used:</span></span>

- <span data-ttu-id="05af9-294">Jak vytvořit vlastní akce filtry pomocí třídy ASP.NET MVC ActionFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="05af9-294">How to create Custom Action filters with the ASP.NET MVC ActionFilterAttribute class</span></span>
- <span data-ttu-id="05af9-295">Postup vložení filtry do řadiče ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="05af9-295">How to inject filters into ASP.NET MVC controllers</span></span>
- <span data-ttu-id="05af9-296">Jak spravovat pomocí vlastnosti pořadí řazení filtrů</span><span class="sxs-lookup"><span data-stu-id="05af9-296">How to manage filter ordering using the Order property</span></span>
- <span data-ttu-id="05af9-297">Postup registrace globální filtry</span><span class="sxs-lookup"><span data-stu-id="05af9-297">How to register filters globally</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="05af9-298">Příloha A: instalaci sady Visual Studio Express 2012 pro Web</span><span class="sxs-lookup"><span data-stu-id="05af9-298">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="05af9-299">Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiný &quot;Express&quot; pomocí verze  **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="05af9-299">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="05af9-300">Následující pokyny vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="05af9-300">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="05af9-301">Přejděte na [ [https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="05af9-301">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="05af9-302">Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produktu &quot; *Visual Studio Express 2012 pro Web se sadou Windows Azure SDK*&quot;.</span><span class="sxs-lookup"><span data-stu-id="05af9-302">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="05af9-303">Klikněte na **nyní nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="05af9-303">Click on **Install Now**.</span></span> <span data-ttu-id="05af9-304">Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.</span><span class="sxs-lookup"><span data-stu-id="05af9-304">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="05af9-305">Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** zahájíte instalaci.</span><span class="sxs-lookup"><span data-stu-id="05af9-305">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="05af9-306">![Nainstalovat Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "nainstalovat Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="05af9-306">![Install Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="05af9-307">*Nainstalovat Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="05af9-307">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="05af9-308">Číst všechny produkty se licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="05af9-308">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Vyjádření souhlasu s podmínkami licence](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    <span data-ttu-id="05af9-310">*Vyjádření souhlasu s podmínkami licence*</span><span class="sxs-lookup"><span data-stu-id="05af9-310">*Accepting the license terms*</span></span>
5. <span data-ttu-id="05af9-311">Počkejte na dokončení procesu stahování a instalaci.</span><span class="sxs-lookup"><span data-stu-id="05af9-311">Wait until the downloading and installation process completes.</span></span>

    ![Průběh instalace](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    <span data-ttu-id="05af9-313">*Průběh instalace*</span><span class="sxs-lookup"><span data-stu-id="05af9-313">*Installation progress*</span></span>
6. <span data-ttu-id="05af9-314">Po dokončení instalace, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="05af9-314">When the installation completes, click **Finish**.</span></span>

    ![Instalace byla dokončena.](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    <span data-ttu-id="05af9-316">*Instalace byla dokončena.*</span><span class="sxs-lookup"><span data-stu-id="05af9-316">*Installation completed*</span></span>
7. <span data-ttu-id="05af9-317">Klikněte na tlačítko **ukončení** ukončíte instalační program webové platformy.</span><span class="sxs-lookup"><span data-stu-id="05af9-317">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="05af9-318">Chcete-li spustit nástroj Visual Studio Express pro Web, přejděte na **spustit** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express pro Web** dlaždice.</span><span class="sxs-lookup"><span data-stu-id="05af9-318">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express pro Web dlaždice](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    <span data-ttu-id="05af9-320">*VS Express pro Web dlaždice*</span><span class="sxs-lookup"><span data-stu-id="05af9-320">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="05af9-321">Příloha B: publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu</span><span class="sxs-lookup"><span data-stu-id="05af9-321">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="05af9-322">Tento dodatek vám ukáže, jak vytvořit nový web z portálu Windows Azure Management Portal a publikovat aplikace, kterou jste získali podle testovacím prostředí, využívat výhod nasazení webu publikování funkce poskytované služby Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="05af9-322">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="05af9-323">Úloha 1 – Vytvoření nového webu ze systému Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="05af9-323">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="05af9-324">Přejděte na [Windows Azure Management Portal](https://manage.windowsazure.com/) a přihlaste se pomocí přihlašovacích údajů společnosti Microsoft, které jsou spojené s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="05af9-324">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="05af9-325">S Windows Azure můžete bezplatné hostování 10 webů ASP.NET a pak škálujte podle rozšiřujícího se provozu.</span><span class="sxs-lookup"><span data-stu-id="05af9-325">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="05af9-326">Můžete si zaregistrovat [zde](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="05af9-326">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="05af9-327">![Přihlaste se k portálu Windows Azure](aspnet-mvc-4-custom-action-filters/_static/image17.png "Přihlaste se k portálu Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="05af9-327">![Log on to Windows Azure portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="05af9-328">*Přihlaste se k Windows Azure Management Portal*</span><span class="sxs-lookup"><span data-stu-id="05af9-328">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="05af9-329">Klikněte na tlačítko **nový** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="05af9-329">Click **New** on the command bar.</span></span>

    <span data-ttu-id="05af9-330">![Vytvoření nového webu](aspnet-mvc-4-custom-action-filters/_static/image18.png "vytváření nového webu")</span><span class="sxs-lookup"><span data-stu-id="05af9-330">![Creating a new Web Site](aspnet-mvc-4-custom-action-filters/_static/image18.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="05af9-331">*Vytvoření nového webu*</span><span class="sxs-lookup"><span data-stu-id="05af9-331">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="05af9-332">Klikněte na tlačítko **výpočetní** | **webu**.</span><span class="sxs-lookup"><span data-stu-id="05af9-332">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="05af9-333">Potom vyberte **rychle vytvořit** možnost.</span><span class="sxs-lookup"><span data-stu-id="05af9-333">Then select **Quick Create** option.</span></span> <span data-ttu-id="05af9-334">Zadejte adresu URL k dispozici pro nový web a klikněte na tlačítko **vytvoření webu**.</span><span class="sxs-lookup"><span data-stu-id="05af9-334">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="05af9-335">Web systému Windows Azure je hostitel pro spouštění v cloudu, ve kterém můžete řídit a spravovat webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="05af9-335">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="05af9-336">Možnost rychle vytvořit můžete nasadit hotové webové aplikace na Windows Azure web z mimo portál.</span><span class="sxs-lookup"><span data-stu-id="05af9-336">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="05af9-337">Postup pro nastavení databáze neobsahuje.</span><span class="sxs-lookup"><span data-stu-id="05af9-337">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="05af9-338">![Vytvoření nového webu pomocí metody rychlého vytvoření](aspnet-mvc-4-custom-action-filters/_static/image19.png "vytváření nového webu pomocí metody rychlého vytvoření")</span><span class="sxs-lookup"><span data-stu-id="05af9-338">![Creating a new Web Site using Quick Create](aspnet-mvc-4-custom-action-filters/_static/image19.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="05af9-339">*Vytvoření nového webu pomocí metody rychlého vytvoření*</span><span class="sxs-lookup"><span data-stu-id="05af9-339">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="05af9-340">Počkejte, dokud nové **webu** je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="05af9-340">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="05af9-341">Po vytvoření webu klikněte na odkaz v části **URL** sloupce.</span><span class="sxs-lookup"><span data-stu-id="05af9-341">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="05af9-342">Zkontrolujte, zda je funkční nový web.</span><span class="sxs-lookup"><span data-stu-id="05af9-342">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="05af9-343">![Procházení na nový web](aspnet-mvc-4-custom-action-filters/_static/image20.png "procházení na nový web")</span><span class="sxs-lookup"><span data-stu-id="05af9-343">![Browsing to the new web site](aspnet-mvc-4-custom-action-filters/_static/image20.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="05af9-344">*Procházení na nový web*</span><span class="sxs-lookup"><span data-stu-id="05af9-344">*Browsing to the new web site*</span></span>

    <span data-ttu-id="05af9-345">![Webový server spuštěn](aspnet-mvc-4-custom-action-filters/_static/image21.png "webu systémem")</span><span class="sxs-lookup"><span data-stu-id="05af9-345">![Web site running](aspnet-mvc-4-custom-action-filters/_static/image21.png "Web site running")</span></span>

    <span data-ttu-id="05af9-346">*Spuštění webu*</span><span class="sxs-lookup"><span data-stu-id="05af9-346">*Web site running*</span></span>
6. <span data-ttu-id="05af9-347">Přejděte zpět na portál a klikněte na název webu v části **název** sloupec pro zobrazení stránky pro správu.</span><span class="sxs-lookup"><span data-stu-id="05af9-347">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="05af9-348">![Otevření stránky Správa webu](aspnet-mvc-4-custom-action-filters/_static/image22.png "otevření stránek správu webového serveru")</span><span class="sxs-lookup"><span data-stu-id="05af9-348">![Opening the web site management pages](aspnet-mvc-4-custom-action-filters/_static/image22.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="05af9-349">*Otevření stránek správu webového serveru*</span><span class="sxs-lookup"><span data-stu-id="05af9-349">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="05af9-350">V **řídicí panel** v části **rychlého přehledu** klikněte na položku **stažení profilu publikování** odkaz.</span><span class="sxs-lookup"><span data-stu-id="05af9-350">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="05af9-351">*Profilu publikování* obsahuje všechny informace požadované pro publikování webové aplikace na web služby Windows Azure pro každou metodu povoleno publikace.</span><span class="sxs-lookup"><span data-stu-id="05af9-351">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="05af9-352">Profil publikování obsahuje adresy URL, přihlašovací údaje uživatele a řetězců databází, které jsou potřebné k připojení k a ověřování na základě těchto koncových bodů, pro které je metoda publikace povolena.</span><span class="sxs-lookup"><span data-stu-id="05af9-352">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="05af9-353">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pro Web** a **sadu Microsoft Visual Studio 2012** podporu čtení publikační profily k automatické konfiguraci těchto programů pro publikování webové aplikace na weby služby Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="05af9-353">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="05af9-354">![Na webu stažení profilu publikování](aspnet-mvc-4-custom-action-filters/_static/image23.png "stahování webové stránky profilu publikování")</span><span class="sxs-lookup"><span data-stu-id="05af9-354">![Downloading the web site publish profile](aspnet-mvc-4-custom-action-filters/_static/image23.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="05af9-355">*Na webu stažení profilu publikování*</span><span class="sxs-lookup"><span data-stu-id="05af9-355">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="05af9-356">Stáhněte si soubor profil publikování do vhodného umístění.</span><span class="sxs-lookup"><span data-stu-id="05af9-356">Download the publish profile file to a known location.</span></span> <span data-ttu-id="05af9-357">Dále v tomto cvičení uvidíte jak používat tento soubor k publikování webové aplikace na weby systému Windows Azure ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="05af9-357">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="05af9-358">![Ukládání souboru profilu publikování](aspnet-mvc-4-custom-action-filters/_static/image24.png "ukládání profilu publikování")</span><span class="sxs-lookup"><span data-stu-id="05af9-358">![Saving the publish profile file](aspnet-mvc-4-custom-action-filters/_static/image24.png "Saving the publish profile")</span></span>

    <span data-ttu-id="05af9-359">*Ukládání souboru profilu publikování*</span><span class="sxs-lookup"><span data-stu-id="05af9-359">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="05af9-360">Úloha 2 – konfigurování serveru databáze</span><span class="sxs-lookup"><span data-stu-id="05af9-360">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="05af9-361">Pokud vaše aplikace využívá systému SQL Server, databáze, budete muset vytvořit databázi SQL server.</span><span class="sxs-lookup"><span data-stu-id="05af9-361">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="05af9-362">Pokud chcete nasadit jednoduchou aplikaci, která nepoužívá systém SQL Server může tuto úlohu přeskočit.</span><span class="sxs-lookup"><span data-stu-id="05af9-362">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="05af9-363">Budete potřebovat databázi SQL serveru pro ukládání databázi aplikace.</span><span class="sxs-lookup"><span data-stu-id="05af9-363">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="05af9-364">Databáze SQL servery můžete zobrazit ze svého předplatného na portál Windows Azure Management **databází Sql** | **servery** | **serveru Řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="05af9-364">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="05af9-365">Pokud nemáte server vytvořeno, můžete vytvořit jeden pomocí **přidat** tlačítka na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="05af9-365">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="05af9-366">Poznamenejte si **název serveru a adresa URL, správce přihlašovací jméno a heslo**, jako je použijete v další úkoly.</span><span class="sxs-lookup"><span data-stu-id="05af9-366">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="05af9-367">Nevytvářejte databáze ještě, jak bude vytvořen v pozdější fázi.</span><span class="sxs-lookup"><span data-stu-id="05af9-367">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="05af9-368">![Řídicí panel serveru databáze SQL](aspnet-mvc-4-custom-action-filters/_static/image25.png "řídicího panelu serveru databáze SQL")</span><span class="sxs-lookup"><span data-stu-id="05af9-368">![SQL Database Server Dashboard](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="05af9-369">*Řídicí panel serveru databáze SQL*</span><span class="sxs-lookup"><span data-stu-id="05af9-369">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="05af9-370">V dalším úkolem budete testovat připojení k databázi ze sady Visual Studio, proto je nutné zahrnout místní IP adresa serveru seznamu **povolené IP adresy**.</span><span class="sxs-lookup"><span data-stu-id="05af9-370">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="05af9-371">To lze provést, klikněte na tlačítko **konfigurace**, vyberte IP adresu z **aktuální IP adresa klienta** a vkládání na **počáteční IP adresa** a **Koncová IP adresa** textová pole a kliknutím ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) tlačítko.</span><span class="sxs-lookup"><span data-stu-id="05af9-371">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) button.</span></span>

    ![Přidávání IP adresy klienta](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    <span data-ttu-id="05af9-373">*Přidávání IP adresy klienta*</span><span class="sxs-lookup"><span data-stu-id="05af9-373">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="05af9-374">Jednou **IP adresa klienta** je povolené IP adresy do seznamu, klikněte na **Uložit** potvrďte změny.</span><span class="sxs-lookup"><span data-stu-id="05af9-374">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Potvrzení změn](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    <span data-ttu-id="05af9-376">*Potvrzení změn*</span><span class="sxs-lookup"><span data-stu-id="05af9-376">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="05af9-377">Úloha 3 – publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu</span><span class="sxs-lookup"><span data-stu-id="05af9-377">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="05af9-378">Přejděte zpět na ASP.NET MVC 4 řešení.</span><span class="sxs-lookup"><span data-stu-id="05af9-378">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="05af9-379">V **Průzkumníku řešení**, klikněte pravým tlačítkem na webový projekt a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="05af9-379">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="05af9-380">![Publikování aplikace](aspnet-mvc-4-custom-action-filters/_static/image29.png "publikování aplikace")</span><span class="sxs-lookup"><span data-stu-id="05af9-380">![Publishing the Application](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publishing the Application")</span></span>

    <span data-ttu-id="05af9-381">*Publikování webu*</span><span class="sxs-lookup"><span data-stu-id="05af9-381">*Publishing the web site*</span></span>
2. <span data-ttu-id="05af9-382">Umožňuje naimportujte profil publikování, který jste uložili v první úloze.</span><span class="sxs-lookup"><span data-stu-id="05af9-382">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="05af9-383">![Import profilu publikování](aspnet-mvc-4-custom-action-filters/_static/image30.png "import profilu publikování")</span><span class="sxs-lookup"><span data-stu-id="05af9-383">![Importing the publish profile](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importing the publish profile")</span></span>

    <span data-ttu-id="05af9-384">*Import profilu publikování*</span><span class="sxs-lookup"><span data-stu-id="05af9-384">*Importing publish profile*</span></span>
3. <span data-ttu-id="05af9-385">Klikněte na tlačítko **ověření připojení**.</span><span class="sxs-lookup"><span data-stu-id="05af9-385">Click **Validate Connection**.</span></span> <span data-ttu-id="05af9-386">Po dokončení ověření klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="05af9-386">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="05af9-387">Ověření je hotová, jakmile se zobrazí zelené zaškrtnutí zobrazí vedle tlačítko ověřit připojení.</span><span class="sxs-lookup"><span data-stu-id="05af9-387">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="05af9-388">![Ověření připojení](aspnet-mvc-4-custom-action-filters/_static/image31.png "ověřování připojení")</span><span class="sxs-lookup"><span data-stu-id="05af9-388">![Validating connection](aspnet-mvc-4-custom-action-filters/_static/image31.png "Validating connection")</span></span>

    <span data-ttu-id="05af9-389">*Ověření připojení*</span><span class="sxs-lookup"><span data-stu-id="05af9-389">*Validating connection*</span></span>
4. <span data-ttu-id="05af9-390">V **nastavení** v části **databáze** části, klikněte na tlačítko vedle připojení databáze textové pole (tj. **objekt DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="05af9-390">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="05af9-391">![Konfigurace nasazení webu](aspnet-mvc-4-custom-action-filters/_static/image32.png "konfigurace nasazení webu")</span><span class="sxs-lookup"><span data-stu-id="05af9-391">![Web deploy configuration](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web deploy configuration")</span></span>

    <span data-ttu-id="05af9-392">*Konfigurace nasazení webu*</span><span class="sxs-lookup"><span data-stu-id="05af9-392">*Web deploy configuration*</span></span>
5. <span data-ttu-id="05af9-393">Připojení k databázi nakonfigurujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="05af9-393">Configure the database connection as follows:</span></span>

    - <span data-ttu-id="05af9-394">V **název serveru** zadejte vaše databáze SQL serveru adresu URL pomocí *tcp:* předponu.</span><span class="sxs-lookup"><span data-stu-id="05af9-394">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
    - <span data-ttu-id="05af9-395">V **uživatelské jméno** zadejte vaše přihlašovací jméno správce serveru.</span><span class="sxs-lookup"><span data-stu-id="05af9-395">In **User name** type your server administrator login name.</span></span>
    - <span data-ttu-id="05af9-396">V **heslo** zadejte přihlašovací heslo správce serveru.</span><span class="sxs-lookup"><span data-stu-id="05af9-396">In **Password** type your server administrator login password.</span></span>
    - <span data-ttu-id="05af9-397">Zadejte nový název databáze.</span><span class="sxs-lookup"><span data-stu-id="05af9-397">Type a new database name.</span></span>

    <span data-ttu-id="05af9-398">![Konfigurace cílový připojovací řetězec](aspnet-mvc-4-custom-action-filters/_static/image33.png "konfigurace cílový připojovací řetězec")</span><span class="sxs-lookup"><span data-stu-id="05af9-398">![Configuring destination connection string](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configuring destination connection string")</span></span>

    <span data-ttu-id="05af9-399">*Konfigurace cílový připojovací řetězec*</span><span class="sxs-lookup"><span data-stu-id="05af9-399">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="05af9-400">Pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="05af9-400">Then click **OK**.</span></span> <span data-ttu-id="05af9-401">Po zobrazení výzvy k vytvoření databáze, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="05af9-401">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="05af9-402">![Vytvoření databáze](aspnet-mvc-4-custom-action-filters/_static/image34.png "vytváření řetězec databáze")</span><span class="sxs-lookup"><span data-stu-id="05af9-402">![Creating the database](aspnet-mvc-4-custom-action-filters/_static/image34.png "Creating the database string")</span></span>

    <span data-ttu-id="05af9-403">*Vytvoření databáze*</span><span class="sxs-lookup"><span data-stu-id="05af9-403">*Creating the database*</span></span>
7. <span data-ttu-id="05af9-404">Připojovací řetězec, který budete používat pro připojení k databázi SQL v systému Windows Azure je uvedené v rámci textbox výchozí připojení.</span><span class="sxs-lookup"><span data-stu-id="05af9-404">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="05af9-405">Pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="05af9-405">Then click **Next**.</span></span>

    <span data-ttu-id="05af9-406">![Připojovací řetězec odkazující na databázi SQL](aspnet-mvc-4-custom-action-filters/_static/image35.png "připojovací řetězec odkazující na databázi SQL")</span><span class="sxs-lookup"><span data-stu-id="05af9-406">![Connection string pointing to SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="05af9-407">*Připojovací řetězec odkazující na databázi SQL*</span><span class="sxs-lookup"><span data-stu-id="05af9-407">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="05af9-408">V **Preview** klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="05af9-408">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="05af9-409">![Publikování webové aplikace](aspnet-mvc-4-custom-action-filters/_static/image36.png "publikování webové aplikace")</span><span class="sxs-lookup"><span data-stu-id="05af9-409">![Publishing the web application](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publishing the web application")</span></span>

    <span data-ttu-id="05af9-410">*Publikování webové aplikace*</span><span class="sxs-lookup"><span data-stu-id="05af9-410">*Publishing the web application*</span></span>
9. <span data-ttu-id="05af9-411">Jakmile proces publikování dokončí, otevře se výchozí prohlížeč publikované webové stránky.</span><span class="sxs-lookup"><span data-stu-id="05af9-411">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="05af9-412">Příloha C: používání fragmentů kódu</span><span class="sxs-lookup"><span data-stu-id="05af9-412">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="05af9-413">S fragmenty kódu máte všechny kód, který je nutné na dosah ruky.</span><span class="sxs-lookup"><span data-stu-id="05af9-413">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="05af9-414">Dokument testovacího prostředí vás bude informovat přesně Pokud můžete, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="05af9-414">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="05af9-415">![Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu](aspnet-mvc-4-custom-action-filters/_static/image37.png "fragmenty kódu pomocí sady Visual Studio pro vložení kódu do projektu")</span><span class="sxs-lookup"><span data-stu-id="05af9-415">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-custom-action-filters/_static/image37.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="05af9-416">*Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu*</span><span class="sxs-lookup"><span data-stu-id="05af9-416">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="05af9-417">***Chcete-li přidat fragment kódu pomocí klávesnice (C# pouze)***</span><span class="sxs-lookup"><span data-stu-id="05af9-417">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="05af9-418">Umístěte kurzor, kam chcete vložit kód.</span><span class="sxs-lookup"><span data-stu-id="05af9-418">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="05af9-419">Začněte psát název fragmentu kódu (bez mezery nebo spojovníky).</span><span class="sxs-lookup"><span data-stu-id="05af9-419">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="05af9-420">Podívejte se na jako IntelliSense zobrazí odpovídající fragmenty názvy.</span><span class="sxs-lookup"><span data-stu-id="05af9-420">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="05af9-421">Vyberte správný fragment kódu (nebo ponechte zadáním dokud je vybraný název celý fragmentu).</span><span class="sxs-lookup"><span data-stu-id="05af9-421">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="05af9-422">Stisknutím klávesy Tab dvakrát můžete vložit fragment v umístění kurzoru.</span><span class="sxs-lookup"><span data-stu-id="05af9-422">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="05af9-423">![Začněte psát název fragmentu](aspnet-mvc-4-custom-action-filters/_static/image38.png "začněte psát název fragmentu kódu")</span><span class="sxs-lookup"><span data-stu-id="05af9-423">![Start typing the snippet name](aspnet-mvc-4-custom-action-filters/_static/image38.png "Start typing the snippet name")</span></span>

<span data-ttu-id="05af9-424">*Začněte psát název fragmentu kódu*</span><span class="sxs-lookup"><span data-stu-id="05af9-424">*Start typing the snippet name*</span></span>

<span data-ttu-id="05af9-425">![Stisknutím klávesy Tab vyberte fragmentu zvýrazněná](aspnet-mvc-4-custom-action-filters/_static/image39.png "stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu")</span><span class="sxs-lookup"><span data-stu-id="05af9-425">![Press Tab to select the highlighted snippet](aspnet-mvc-4-custom-action-filters/_static/image39.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="05af9-426">*Stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu*</span><span class="sxs-lookup"><span data-stu-id="05af9-426">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="05af9-427">![Stisknutím klávesy Tab znovu a fragmentu rozšíří](aspnet-mvc-4-custom-action-filters/_static/image40.png "stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.")</span><span class="sxs-lookup"><span data-stu-id="05af9-427">![Press Tab again and the snippet will expand](aspnet-mvc-4-custom-action-filters/_static/image40.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="05af9-428">*Stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.*</span><span class="sxs-lookup"><span data-stu-id="05af9-428">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="05af9-429">***Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="05af9-429">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="05af9-430">Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="05af9-430">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="05af9-431">Vyberte **Vložit fragment** následuje **Moje fragmenty kódu**.</span><span class="sxs-lookup"><span data-stu-id="05af9-431">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="05af9-432">Vyberte relevantní fragment kódu ze seznamu, kliknutím na.</span><span class="sxs-lookup"><span data-stu-id="05af9-432">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="05af9-433">![Klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment](aspnet-mvc-4-custom-action-filters/_static/image41.png "klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment")</span><span class="sxs-lookup"><span data-stu-id="05af9-433">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-custom-action-filters/_static/image41.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="05af9-434">*Klikněte pravým tlačítkem myši, kam chcete vložit fragment kódu a vyberte Vložit fragment*</span><span class="sxs-lookup"><span data-stu-id="05af9-434">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="05af9-435">![Vyberte relevantní fragment kódu ze seznamu, kliknutím na](aspnet-mvc-4-custom-action-filters/_static/image42.png "vyberte relevantní fragment kódu ze seznamu, kliknutím na")</span><span class="sxs-lookup"><span data-stu-id="05af9-435">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-custom-action-filters/_static/image42.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="05af9-436">*Vyberte relevantní fragment kódu ze seznamu, kliknutím na*</span><span class="sxs-lookup"><span data-stu-id="05af9-436">*Pick the relevant snippet from the list, by clicking on it*</span></span>
