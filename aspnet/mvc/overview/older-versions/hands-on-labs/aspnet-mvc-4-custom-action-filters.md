---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: Vlastní akce filtrech rozhraní ASP.NET MVC 4 | Dokumentace Microsoftu
author: rick-anderson
description: ASP.NET MVC poskytuje filtry akcí pro provádění logiku filtrování před nebo po zavolání metody akce. Filtry akce jsou vlastní atributy tha...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: 0170fda6849c1dfb53b44908ea55ba2cad0dd067
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755039"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a><span data-ttu-id="58918-104">ASP.NET MVC 4 – filtr vlastních akcí</span><span class="sxs-lookup"><span data-stu-id="58918-104">ASP.NET MVC 4 Custom Action Filters</span></span>

<span data-ttu-id="58918-105">podle [Campy Web týmu](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="58918-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="58918-106">Stáhněte si Web Campy školení Kit</span><span class="sxs-lookup"><span data-stu-id="58918-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="58918-107">ASP.NET MVC poskytuje filtry akcí pro provádění logiku filtrování před nebo po zavolání metody akce.</span><span class="sxs-lookup"><span data-stu-id="58918-107">ASP.NET MVC provides Action Filters for executing filtering logic either before or after an action method is called.</span></span> <span data-ttu-id="58918-108">Filtry akce jsou vlastní atributy, které poskytují deklarativní způsob, jak přidat akci před a po akci chování metody akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="58918-108">Action Filters are custom attributes that provide declarative means to add pre-action and post-action behavior to the controller's action methods.</span></span>

<span data-ttu-id="58918-109">Ve tohoto praktického testovacího prostředí vytvoříte vlastní akci atribut filtru do řešení MvcMusicStore zachytit kontroleru požadavků a protokolování aktivity webu do databázové tabulky.</span><span class="sxs-lookup"><span data-stu-id="58918-109">In this Hands-on Lab you will create a custom action filter attribute into MvcMusicStore solution to catch controller's requests and log the activity of a site into a database table.</span></span> <span data-ttu-id="58918-110">Budete moct přidat filtr protokolování injektáží kontroler nebo akce.</span><span class="sxs-lookup"><span data-stu-id="58918-110">You will be able to add your logging filter by injection to any controller or action.</span></span> <span data-ttu-id="58918-111">A konečně zobrazí se zobrazení protokolu, který zobrazuje seznam návštěvníků.</span><span class="sxs-lookup"><span data-stu-id="58918-111">Finally, you will see the log view that shows the list of visitors.</span></span>

<span data-ttu-id="58918-112">Tohoto praktického testovacího prostředí předpokládá, že máte základní znalosti o **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="58918-112">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="58918-113">Pokud jste ještě nepoužívali **ASP.NET MVC** před, doporučujeme si projít **základy ASP.NET MVC 4** praktického testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="58918-113">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="58918-114">Všechny ukázky kódu a fragmenty kódu jsou součástí této webové Campy školicí sady, k dispozici na [verzích Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="58918-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="58918-115">Projekt specifické pro toto testovací prostředí je k dispozici na [ASP.NET MVC 4 vlastní akce filtry](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).</span><span class="sxs-lookup"><span data-stu-id="58918-115">The project specific to this lab is available at [ASP.NET MVC 4 Custom Action Filters](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="58918-116">Cíle</span><span class="sxs-lookup"><span data-stu-id="58918-116">Objectives</span></span>

<span data-ttu-id="58918-117">V tomto praktického testovacího prostředí se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="58918-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="58918-118">Vytvoření vlastní akce atribut filtru rozšířit možnosti filtrování</span><span class="sxs-lookup"><span data-stu-id="58918-118">Create a custom action filter attribute to extend filtering capabilities</span></span>
- <span data-ttu-id="58918-119">Použít vlastní atribut filtru injektáží konkrétní úroveň</span><span class="sxs-lookup"><span data-stu-id="58918-119">Apply a custom filter attribute by injection to a specific level</span></span>
- <span data-ttu-id="58918-120">Registrace, který globálně – filtr vlastních akcí</span><span class="sxs-lookup"><span data-stu-id="58918-120">Register a custom action filters globally</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="58918-121">Požadavky</span><span class="sxs-lookup"><span data-stu-id="58918-121">Prerequisites</span></span>

<span data-ttu-id="58918-122">Musíte mít následující položky k dokončení tohoto testovacího prostředí:</span><span class="sxs-lookup"><span data-stu-id="58918-122">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="58918-123">[Microsoft Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) nebo i vyšší (čtení [příloha A](#AppendixA) pokyny k jeho instalaci).</span><span class="sxs-lookup"><span data-stu-id="58918-123">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="58918-124">Instalace</span><span class="sxs-lookup"><span data-stu-id="58918-124">Setup</span></span>

<span data-ttu-id="58918-125">**Instalace fragmenty kódu**</span><span class="sxs-lookup"><span data-stu-id="58918-125">**Installing Code Snippets**</span></span>

<span data-ttu-id="58918-126">Pro usnadnění práce velkou část kódu, které budete spravovat podél tohoto testovacího prostředí je k dispozici jako fragmenty kódu sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="58918-126">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="58918-127">K instalaci spustit fragmenty kódu **.\Source\Setup\CodeSnippets.vsi** souboru.</span><span class="sxs-lookup"><span data-stu-id="58918-127">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="58918-128">Pokud nejste obeznámeni s fragmenty kódu Visual Studio a chcete další informace o jejich použití, najdete dodatku z tohoto dokumentu &quot; [fragmenty kódu pomocí příloha C:](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="58918-128">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="58918-129">Cvičení</span><span class="sxs-lookup"><span data-stu-id="58918-129">Exercises</span></span>

<span data-ttu-id="58918-130">Podle následující praktická cvičení se skládá tohoto praktického testovacího prostředí:</span><span class="sxs-lookup"><span data-stu-id="58918-130">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="58918-131">Cvičení 1: Protokolování akcí</span><span class="sxs-lookup"><span data-stu-id="58918-131">Exercise 1: Logging actions</span></span>](#Exercise1)
2. [<span data-ttu-id="58918-132">Cvičení 2: Správa více filtrů Akce</span><span class="sxs-lookup"><span data-stu-id="58918-132">Exercise 2: Managing Multiple Action Filters</span></span>](#Exercise2)

<span data-ttu-id="58918-133">Odhadovaný čas dokončení tohoto testovacího prostředí: **30 minut**.</span><span class="sxs-lookup"><span data-stu-id="58918-133">Estimated time to complete this lab: **30 minutes**.</span></span>

> [!NOTE]
> <span data-ttu-id="58918-134">Se sadou každý cvičení **koncové** složku, která obsahuje výsledný řešení byste měli získat po dokončení cvičení.</span><span class="sxs-lookup"><span data-stu-id="58918-134">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="58918-135">Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc prostřednictvím praktická cvičení.</span><span class="sxs-lookup"><span data-stu-id="58918-135">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a><span data-ttu-id="58918-136">Cvičení 1: Protokolování akcí</span><span class="sxs-lookup"><span data-stu-id="58918-136">Exercise 1: Logging Actions</span></span>

<span data-ttu-id="58918-137">V tomto cvičení se dozvíte, jak vytvořit filtr protokolu vlastní akci s použitím technologie ASP.NET MVC 4 filtr zprostředkovatelů.</span><span class="sxs-lookup"><span data-stu-id="58918-137">In this exercise, you will learn how to create a custom action log filter by using ASP.NET MVC 4 Filter Providers.</span></span> <span data-ttu-id="58918-138">K tomuto účelu použijete filtr přihlašování k lokalitě MusicStore, zaznamená všechny aktivity v seznamu vybraných zařízení.</span><span class="sxs-lookup"><span data-stu-id="58918-138">For that purpose you will apply a logging filter to the MusicStore site that will record all the activities in the selected controllers.</span></span>

<span data-ttu-id="58918-139">Filtr se rozšíří **ActionFilterAttributeClass** a přepsat **OnActionExecuting** metoda zachytit každého požadavku a provádět protokolování.</span><span class="sxs-lookup"><span data-stu-id="58918-139">The filter will extend **ActionFilterAttributeClass** and override **OnActionExecuting** method to catch each request and then perform the logging actions.</span></span> <span data-ttu-id="58918-140">Kontextové informace o požadavcích HTTP, provedení metody, výsledky a parametry budou poskytovat ASP.NET MVC **ActionExecutingContext** třídy **.**</span><span class="sxs-lookup"><span data-stu-id="58918-140">The context information about HTTP requests, executing methods, results and parameters will be provided by ASP.NET MVC **ActionExecutingContext** class **.**</span></span>

> [!NOTE]
> <span data-ttu-id="58918-141">ASP.NET MVC 4 obsahuje také výchozí filtry poskytovatele, které lze použít bez vytvoření vlastního filtru.</span><span class="sxs-lookup"><span data-stu-id="58918-141">ASP.NET MVC 4 also has default filters providers you can use without creating a custom filter.</span></span> <span data-ttu-id="58918-142">ASP.NET MVC 4 poskytuje následující typy filtrů:</span><span class="sxs-lookup"><span data-stu-id="58918-142">ASP.NET MVC 4 provides the following types of filters:</span></span>
> 
> - <span data-ttu-id="58918-143">**Autorizace** filtru, který představuje bezpečnostní rozhodnutí o provedení metody akce, jako je například ověřování nebo ověřování vlastnosti požadavku.</span><span class="sxs-lookup"><span data-stu-id="58918-143">**Authorization** filter, which makes security decisions about whether to execute an action method, such as performing authentication or validating properties of the request.</span></span>
> - <span data-ttu-id="58918-144">**Akce** filtru, která zabalí provádění metody akce.</span><span class="sxs-lookup"><span data-stu-id="58918-144">**Action** filter, which wraps the action method execution.</span></span> <span data-ttu-id="58918-145">Tento filtr můžete provést další zpracování, jako jsou například poskytuje doplňující data na metodu akce, kontrola vrácené hodnoty nebo zrušil provádění metody akce</span><span class="sxs-lookup"><span data-stu-id="58918-145">This filter can perform additional processing, such as providing extra data to the action method, inspecting the return value, or canceling execution of the action method</span></span>
> - <span data-ttu-id="58918-146">**Výsledek** filtru, která zabalí provádění objektu ActionResult.</span><span class="sxs-lookup"><span data-stu-id="58918-146">**Result** filter, which wraps execution of the ActionResult object.</span></span> <span data-ttu-id="58918-147">Tento filtr můžete provést další zpracování výsledku, jako je třeba změna odpověď HTTP.</span><span class="sxs-lookup"><span data-stu-id="58918-147">This filter can perform additional processing of the result, such as modifying the HTTP response.</span></span>
> - <span data-ttu-id="58918-148">**Výjimka** filtr, který se spustí, pokud je neošetřená výjimka vyvolaná někde v metodě akce, počínaje filtry autorizace a konče spuštění výsledku.</span><span class="sxs-lookup"><span data-stu-id="58918-148">**Exception** filter, which executes if there is an unhandled exception thrown somewhere in action method, starting with the authorization filters and ending with the execution of the result.</span></span> <span data-ttu-id="58918-149">Filtry výjimek je možné pro úlohy, jako je protokolování nebo zobrazení chybové stránky.</span><span class="sxs-lookup"><span data-stu-id="58918-149">Exception filters can be used for tasks such as logging or displaying an error page.</span></span>
> 
> <span data-ttu-id="58918-150">Další informace o poskytovatelích filtry najdete v tomto odkazu MSDN: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).</span><span class="sxs-lookup"><span data-stu-id="58918-150">For more information about Filters Providers please visit this MSDN link: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)) .</span></span>


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a><span data-ttu-id="58918-151">Informace o funkci protokolování aplikace MVC Music Store</span><span class="sxs-lookup"><span data-stu-id="58918-151">About MVC Music Store Application logging feature</span></span>

<span data-ttu-id="58918-152">Toto řešení Music Store má nové tabulky datového modelu pro protokolování serveru **ActionLog**, u následujících polí: název kontroleru, který obdržel požadavek, volá akci, IP adresa klienta a časové razítko.</span><span class="sxs-lookup"><span data-stu-id="58918-152">This Music Store solution has a new data model table for site logging, **ActionLog**, with the following fields: Name of the controller that received a request, Called action, Client IP and Time stamp.</span></span>

<span data-ttu-id="58918-153">![Datový model. Tabulka ActionLog. ](aspnet-mvc-4-custom-action-filters/_static/image1.png "Datového modelu. ActionLog tabulky.")</span><span class="sxs-lookup"><span data-stu-id="58918-153">![Data model. ActionLog table.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Data model. ActionLog table.")</span></span>

<span data-ttu-id="58918-154">*Datový model - ActionLog tabulky*</span><span class="sxs-lookup"><span data-stu-id="58918-154">*Data model - ActionLog table*</span></span>

<span data-ttu-id="58918-155">Řešení poskytuje zobrazení ASP.NET MVC do protokolu akcí, které lze nalézt v **MvcMusicStores/zobrazení/ActionLog**:</span><span class="sxs-lookup"><span data-stu-id="58918-155">The solution provides an ASP.NET MVC View for the Action log that can be found at **MvcMusicStores/Views/ActionLog**:</span></span>

<span data-ttu-id="58918-156">![Zobrazení protokolu akce](aspnet-mvc-4-custom-action-filters/_static/image2.png "zobrazit protokol akcí")</span><span class="sxs-lookup"><span data-stu-id="58918-156">![Action Log view](aspnet-mvc-4-custom-action-filters/_static/image2.png "Action Log view")</span></span>

<span data-ttu-id="58918-157">*Zobrazení protokolu akcí*</span><span class="sxs-lookup"><span data-stu-id="58918-157">*Action Log view*</span></span>

<span data-ttu-id="58918-158">S tímto zadané struktury bude se veškerá práce, zaměřuje na by bylo třeba přerušit kontrolor žádosti a provedení protokolování pomocí vlastní filtrování.</span><span class="sxs-lookup"><span data-stu-id="58918-158">With this given structure, all the work will be focused on interrupting controller's request and performing the logging by using custom filtering.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a><span data-ttu-id="58918-159">Úloha 1 – Vytvoření vlastního filtru pro zachycení požadavek Kontroleru</span><span class="sxs-lookup"><span data-stu-id="58918-159">Task 1 - Creating a Custom Filter to Catch a Controller's Request</span></span>

<span data-ttu-id="58918-160">V této úloze vytvoříte třídu atributu vlastní filtr, který bude obsahovat logiky protokolování.</span><span class="sxs-lookup"><span data-stu-id="58918-160">In this task you will create a custom filter attribute class that will contain the logging logic.</span></span> <span data-ttu-id="58918-161">K tomuto účelu rozšíříte ASP.NET MVC **ActionFilterAttribute** třídy a implementovat rozhraní **IActionFilter**.</span><span class="sxs-lookup"><span data-stu-id="58918-161">For that purpose you will extend ASP.NET MVC **ActionFilterAttribute** Class and implement the interface **IActionFilter**.</span></span>

> [!NOTE]
> <span data-ttu-id="58918-162">**ActionFilterAttribute** je základní třídou pro všechny filtry atribut.</span><span class="sxs-lookup"><span data-stu-id="58918-162">The **ActionFilterAttribute** is the base class for all the attribute filters.</span></span> <span data-ttu-id="58918-163">Poskytuje následující metody k provedení konkrétní logiky po a před spuštěním akce kontroleru:</span><span class="sxs-lookup"><span data-stu-id="58918-163">It provides the following methods to execute a specific logic after and before controller action's execution:</span></span>
> 
> - <span data-ttu-id="58918-164">**OnActionExecuting**(ActionExecutingContext filterContext): těsně před akce je volána metoda.</span><span class="sxs-lookup"><span data-stu-id="58918-164">**OnActionExecuting**(ActionExecutingContext filterContext): Just before the action method is called.</span></span>
> - <span data-ttu-id="58918-165">**OnActionExecuted**(ActionExecutedContext filterContext): po zavolání metody akce a před provedením výsledku (před vykreslením zobrazení).</span><span class="sxs-lookup"><span data-stu-id="58918-165">**OnActionExecuted**(ActionExecutedContext filterContext): After the action method is called and before the result is executed (before view render).</span></span>
> - <span data-ttu-id="58918-166">**OnResultExecuting**(ResultExecutingContext filterContext): těsně před je výsledek spuštěn (před vykreslením zobrazení).</span><span class="sxs-lookup"><span data-stu-id="58918-166">**OnResultExecuting**(ResultExecutingContext filterContext): Just before the result is executed (before view render).</span></span>
> - <span data-ttu-id="58918-167">**OnResultExecuted**(ResultExecutedContext filterContext): Po provedení výsledku (po je zobrazení vykresleno).</span><span class="sxs-lookup"><span data-stu-id="58918-167">**OnResultExecuted**(ResultExecutedContext filterContext): After the result is executed (after the view is rendered).</span></span>
> 
> <span data-ttu-id="58918-168">Tak, že přepíšete některé z těchto metod na odvozenou třídu, můžete provést filtrování kódu.</span><span class="sxs-lookup"><span data-stu-id="58918-168">By overriding any of these methods into a derived class, you can execute your own filtering code.</span></span>


1. <span data-ttu-id="58918-169">Otevřít **začít** řešení nachází v **\Source\Ex01-LoggingActions\Begin** složky.</span><span class="sxs-lookup"><span data-stu-id="58918-169">Open the **Begin** solution located at **\Source\Ex01-LoggingActions\Begin** folder.</span></span>

   1. <span data-ttu-id="58918-170">Je potřeba předtím, než budete pokračovat, stáhněte si některé chybějící balíčky NuGet.</span><span class="sxs-lookup"><span data-stu-id="58918-170">You will need to download some missing NuGet packages before you continue.</span></span> <span data-ttu-id="58918-171">Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="58918-171">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="58918-172">V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="58918-172">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="58918-173">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="58918-173">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="58918-174">Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="58918-174">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="58918-175">Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="58918-175">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="58918-176">To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="58918-176">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="58918-177">Další informace najdete v tomto článku: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="58918-177">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="58918-178">Přidání nové třídy C# do **filtry** složku a pojmenujte ho *CustomActionFilter.cs*.</span><span class="sxs-lookup"><span data-stu-id="58918-178">Add a new C# class into the **Filters** folder and name it *CustomActionFilter.cs*.</span></span> <span data-ttu-id="58918-179">Tato složka uloží všechny vlastní filtry.</span><span class="sxs-lookup"><span data-stu-id="58918-179">This folder will store all the custom filters.</span></span>
3. <span data-ttu-id="58918-180">Otevřít **CustomActionFilter.cs** a přidejte odkaz na **System.Web.Mvc** a **MvcMusicStore.Models** oborů názvů:</span><span class="sxs-lookup"><span data-stu-id="58918-180">Open **CustomActionFilter.cs** and add a reference to **System.Web.Mvc** and **MvcMusicStore.Models** namespaces:</span></span>

    <span data-ttu-id="58918-181">(Fragment - kódu *filtry vlastní akce architektury ASP.NET MVC 4 – Ex1 CustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="58918-181">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-CustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. <span data-ttu-id="58918-182">Dědí **CustomActionFilter** třídy z **ActionFilterAttribute** a pak proveďte **CustomActionFilter** implementaci třídy **IActionFilter** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="58918-182">Inherit the **CustomActionFilter** class from **ActionFilterAttribute** and then make **CustomActionFilter** class implement **IActionFilter** interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. <span data-ttu-id="58918-183">Ujistěte se, **CustomActionFilter** třídě přepsat metodu **OnActionExecuting** a přidat logiku potřebnou k protokolu spuštění filtru.</span><span class="sxs-lookup"><span data-stu-id="58918-183">Make **CustomActionFilter** class override the method **OnActionExecuting** and add the necessary logic to log the filter's execution.</span></span> <span data-ttu-id="58918-184">Chcete-li to provést, přidejte následující zvýrazněný kód v rámci **CustomActionFilter** třídy.</span><span class="sxs-lookup"><span data-stu-id="58918-184">To do this, add the following highlighted code within **CustomActionFilter** class.</span></span>

    <span data-ttu-id="58918-185">(Fragment - kódu *filtry vlastní akce architektury ASP.NET MVC 4 – Ex1 LoggingActions*)</span><span class="sxs-lookup"><span data-stu-id="58918-185">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-LoggingActions*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > <span data-ttu-id="58918-186">**OnActionExecuting** pomocí metody **Entity Framework** přidáte nový ActionLog registru.</span><span class="sxs-lookup"><span data-stu-id="58918-186">**OnActionExecuting** method is using **Entity Framework** to add a new ActionLog register.</span></span> <span data-ttu-id="58918-187">Vytvoří a vyplní novou instanci entity kontextové informace z **filterContext**.</span><span class="sxs-lookup"><span data-stu-id="58918-187">It creates and fills a new entity instance with the context information from **filterContext**.</span></span>
    > 
    > <span data-ttu-id="58918-188">Další informace o **parametrem ControllerContext** třídy v [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="58918-188">You can read more about **ControllerContext** class at [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a><span data-ttu-id="58918-189">Úloha 2 – injektáž kódu zachycování do třídy Kontroleru Store</span><span class="sxs-lookup"><span data-stu-id="58918-189">Task 2 - Injecting a Code Interceptor into the Store Controller Class</span></span>

<span data-ttu-id="58918-190">V této úloze přidáte vlastní filtr vložením na všechny třídy kontroleru a akce kontroleru, které budou protokolovány.</span><span class="sxs-lookup"><span data-stu-id="58918-190">In this task you will add the custom filter by injecting it to all controller classes and controller actions that will be logged.</span></span> <span data-ttu-id="58918-191">Pro účely tohoto cvičení bude mít třídy Kontroleru Store protokolu.</span><span class="sxs-lookup"><span data-stu-id="58918-191">For the purpose of this exercise, the Store Controller class will have a log.</span></span>

<span data-ttu-id="58918-192">Metoda **OnActionExecuting** z **ActionLogFilterAttribute** vlastní filtr se spouští při volání vloženého elementu.</span><span class="sxs-lookup"><span data-stu-id="58918-192">The method **OnActionExecuting** from **ActionLogFilterAttribute** custom filter runs when an injected element is called.</span></span>

<span data-ttu-id="58918-193">Je také možné zachytit metodu konkrétní ovladač.</span><span class="sxs-lookup"><span data-stu-id="58918-193">It is also possible to intercept a specific controller method.</span></span>

1. <span data-ttu-id="58918-194">Otevřít **StoreController** na **MvcMusicStore\Controllers** a přidejte odkaz na **filtry** obor názvů:</span><span class="sxs-lookup"><span data-stu-id="58918-194">Open the **StoreController** at **MvcMusicStore\Controllers** and add a reference to the **Filters** namespace:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. <span data-ttu-id="58918-195">Vložit vlastní filtr **CustomActionFilter** do **StoreController** třídy tak, že přidáte **[CustomActionFilter]** atribut před deklaraci třídy.</span><span class="sxs-lookup"><span data-stu-id="58918-195">Inject the custom filter **CustomActionFilter** into **StoreController** class by adding **[CustomActionFilter]** attribute before the class declaration.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > <span data-ttu-id="58918-196">Když filtr se vloží do třídy kontroleru, jsou také vloží všechny jeho akce.</span><span class="sxs-lookup"><span data-stu-id="58918-196">When a filter is injected into a controller class, all its actions are also injected.</span></span> <span data-ttu-id="58918-197">Pokud chcete použít filtr jenom pro sadu akcí, je třeba vložit **[CustomActionFilter]** na každém z nich:</span><span class="sxs-lookup"><span data-stu-id="58918-197">If you would like to apply the filter only for a set of actions, you would have to inject **[CustomActionFilter]** to each one of them:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="58918-198">Úloha 3 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="58918-198">Task 3 - Running the Application</span></span>

<span data-ttu-id="58918-199">V této úloze budete testovat, že filtr přihlašování funguje.</span><span class="sxs-lookup"><span data-stu-id="58918-199">In this task, you will test that the logging filter is working.</span></span> <span data-ttu-id="58918-200">Bude spusťte aplikaci a přejděte úložiště, a poté zkontroluje protokolu aktivit.</span><span class="sxs-lookup"><span data-stu-id="58918-200">You will start the application and visit the store, and then you will check logged activities.</span></span>

1. <span data-ttu-id="58918-201">Stisknutím klávesy **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="58918-201">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="58918-202">Přejděte do **/ActionLog** zobrazíte počáteční stav zobrazení protokolu:</span><span class="sxs-lookup"><span data-stu-id="58918-202">Browse to **/ActionLog** to see log view initial state:</span></span>

    <span data-ttu-id="58918-203">![Sledování stavu před aktivity stránku protokolu](aspnet-mvc-4-custom-action-filters/_static/image3.png "sledování stavu před aktivity stránku protokolu")</span><span class="sxs-lookup"><span data-stu-id="58918-203">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image3.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="58918-204">*Protokolu sledování stavu před aktivity stránku*</span><span class="sxs-lookup"><span data-stu-id="58918-204">*Log tracker status before page activity*</span></span>

   > [!NOTE]
   > <span data-ttu-id="58918-205">Ve výchozím nastavení se vždy zobrazí jednu položku, který je generován při získávání existujících žánry nabídky.</span><span class="sxs-lookup"><span data-stu-id="58918-205">By default, it will always show one item that is generated when retrieving the existing genres for the menu.</span></span>
   > 
   > <span data-ttu-id="58918-206">Pro účely jednoduchosti čistíme **ActionLog** tabulky pokaždé, když je aplikace spuštěná, zobrazí pouze protokoly ověřování každý konkrétní úkol.</span><span class="sxs-lookup"><span data-stu-id="58918-206">For simplicity purposes we're cleaning up the **ActionLog** table each time the application runs so it will only show the logs of each particular task's verification.</span></span>
   > 
   > <span data-ttu-id="58918-207">Možná budete muset odebrat následující kód z **relace\_Start** – metoda (v **Global.asax** třídy), aby bylo možné uložit Historický protokol pro všechny akce provést v rámci Store Kontroler.</span><span class="sxs-lookup"><span data-stu-id="58918-207">You might need to remove the following code from the **Session\_Start** method (in the **Global.asax** class), in order to save an historical log for all the actions executed within the Store Controller.</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. <span data-ttu-id="58918-208">Klikněte na některou **žánry** z nabídky a provést některé akce, třeba k procházení alba k dispozici.</span><span class="sxs-lookup"><span data-stu-id="58918-208">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
4. <span data-ttu-id="58918-209">Přejděte do **/ActionLog** a pokud je protokol prázdný stiskněte **F5** aktualizovat stránku.</span><span class="sxs-lookup"><span data-stu-id="58918-209">Browse to **/ActionLog** and if the log is empty press **F5** to refresh the page.</span></span> <span data-ttu-id="58918-210">Zkontrolujte, že byly sledovány vaší návštěvy:</span><span class="sxs-lookup"><span data-stu-id="58918-210">Check that your visits were tracked:</span></span>

    <span data-ttu-id="58918-211">![Protokol akcí s aktivitou protokoluje](aspnet-mvc-4-custom-action-filters/_static/image4.png "protokol akcí s aktivitou přihlášení")</span><span class="sxs-lookup"><span data-stu-id="58918-211">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image4.png "Action log with activity logged")</span></span>

    <span data-ttu-id="58918-212">*Protokol akcí s aktivita přihlášení*</span><span class="sxs-lookup"><span data-stu-id="58918-212">*Action log with activity logged*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a><span data-ttu-id="58918-213">Cvičení 2: Správa více filtrů Akce</span><span class="sxs-lookup"><span data-stu-id="58918-213">Exercise 2: Managing Multiple Action Filters</span></span>

<span data-ttu-id="58918-214">V tomto cvičení přidáte druhý filtr vlastních akcí na třídu StoreController a definovat konkrétní pořadí, ve kterém se spustí i filtry.</span><span class="sxs-lookup"><span data-stu-id="58918-214">In this exercise you will add a second Custom Action Filter to the StoreController class and define the specific order in which both filters will be executed.</span></span> <span data-ttu-id="58918-215">Pak aktualizujete kód pro registraci filtr globálně.</span><span class="sxs-lookup"><span data-stu-id="58918-215">Then you will update the code to register the filter Globally.</span></span>

<span data-ttu-id="58918-216">Vezměte v úvahu při definování pořadí spuštění filtrů se různými způsoby.</span><span class="sxs-lookup"><span data-stu-id="58918-216">There are different options to take into account when defining the Filters' execution order.</span></span> <span data-ttu-id="58918-217">Například vlastnost pořadí a oboru filtrů:</span><span class="sxs-lookup"><span data-stu-id="58918-217">For example, the Order property and the Filters' scope:</span></span>

<span data-ttu-id="58918-218">Můžete definovat **oboru** pro každý z těchto filtrů, třeba rozsahu všechny filtry akce ke spuštění v rámci **řadič oboru**a všechny filtry autorizace pro spuštění v **globálního rozsahu** .</span><span class="sxs-lookup"><span data-stu-id="58918-218">You can define a **Scope** for each of the Filters, for example, you could scope all the Action Filters to run within the **Controller Scope**, and all Authorization Filters to run in **Global scope**.</span></span> <span data-ttu-id="58918-219">Obory mít definované provádění pořadí.</span><span class="sxs-lookup"><span data-stu-id="58918-219">The scopes have a defined execution order.</span></span>

<span data-ttu-id="58918-220">Kromě toho má každý filtr akce pořadí vlastnost, která se používá k určení pořadí spouštění v oboru filtru.</span><span class="sxs-lookup"><span data-stu-id="58918-220">Additionally, each action filter has an Order property which is used to determine the execution order in the scope of the filter.</span></span>

<span data-ttu-id="58918-221">Další informace o pořadí spuštění filtrů vlastní akce, najdete v článku na webu MSDN: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span><span class="sxs-lookup"><span data-stu-id="58918-221">For more information about Custom Action Filters execution order, please visit this MSDN article: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a><span data-ttu-id="58918-222">Úloha 1: Vytvoření nové filtr vlastních akcí</span><span class="sxs-lookup"><span data-stu-id="58918-222">Task 1: Creating a new Custom Action Filter</span></span>

<span data-ttu-id="58918-223">V této úloze vytvoříte nový filtr vlastních akcí vložit do třídy StoreController učit, jak spravovat pořadí spuštění filtrů.</span><span class="sxs-lookup"><span data-stu-id="58918-223">In this task, you will create a new Custom Action Filter to inject into the StoreController class, learning how to manage the execution order of the filters.</span></span>

1. <span data-ttu-id="58918-224">Otevřít **začít** řešení nachází v **\Source\Ex02-ManagingMultipleActionFilters\Begin** složky.</span><span class="sxs-lookup"><span data-stu-id="58918-224">Open the **Begin** solution located at **\Source\Ex02-ManagingMultipleActionFilters\Begin** folder.</span></span> <span data-ttu-id="58918-225">V opačném případě může nadále používat **End** řešení získat provedením předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="58918-225">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="58918-226">Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="58918-226">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="58918-227">Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="58918-227">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="58918-228">V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="58918-228">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="58918-229">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="58918-229">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

        > [!NOTE]
        > <span data-ttu-id="58918-230">Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="58918-230">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="58918-231">Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="58918-231">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="58918-232">To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="58918-232">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
        > 
        > <span data-ttu-id="58918-233">Další informace najdete v tomto článku: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="58918-233">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="58918-234">Přidání nové třídy C# do **filtry** složku a pojmenujte ho *MyNewCustomActionFilter.cs*</span><span class="sxs-lookup"><span data-stu-id="58918-234">Add a new C# class into the **Filters** folder and name it *MyNewCustomActionFilter.cs*</span></span>
3. <span data-ttu-id="58918-235">Otevřít **MyNewCustomActionFilter.cs** a přidejte odkaz na **System.Web.Mvc** a **MvcMusicStore.Models** obor názvů:</span><span class="sxs-lookup"><span data-stu-id="58918-235">Open **MyNewCustomActionFilter.cs** and add a reference to **System.Web.Mvc** and the **MvcMusicStore.Models** namespace:</span></span>

    <span data-ttu-id="58918-236">(Fragment - kódu *filtry vlastní akce architektury ASP.NET MVC 4 – Ex2 MyNewCustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="58918-236">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. <span data-ttu-id="58918-237">Deklarace třídy výchozí nahraďte následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="58918-237">Replace the default class declaration with the following code.</span></span>

    <span data-ttu-id="58918-238">(Fragment - kódu *filtry vlastní akce architektury ASP.NET MVC 4 – Ex2 MyNewCustomActionFilterClass*)</span><span class="sxs-lookup"><span data-stu-id="58918-238">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterClass*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="58918-239">Tento filtr vlastních akcí je téměř stejný než ta, kterou jste vytvořili v předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="58918-239">This Custom Action Filter is almost the same than the one you created in the previous exercise.</span></span> <span data-ttu-id="58918-240">Hlavní rozdíl je, že na něm *&quot;protokolovány podle&quot;* atribut aktualizovat tato nová třída název pro identifikaci začínajícího filtr registrován v protokolu.</span><span class="sxs-lookup"><span data-stu-id="58918-240">The main difference is that it has the *&quot;Logged By&quot;* attribute updated with this new class' name to identify wich filter registered the log.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a><span data-ttu-id="58918-241">Úloha 2: Vkládá nový sběrač kód do třídy StoreController</span><span class="sxs-lookup"><span data-stu-id="58918-241">Task 2: Injecting a new Code Interceptor into the StoreController Class</span></span>

<span data-ttu-id="58918-242">V této úloze se přidat nový vlastní filtr do třídy StoreController a spuštění řešení k ověření, jak oba filtry spolupracují.</span><span class="sxs-lookup"><span data-stu-id="58918-242">In this task, you will add a new custom filter into the StoreController Class and run the solution to verify how both filters work together.</span></span>

1. <span data-ttu-id="58918-243">Otevřít **StoreController** třídy umístění **MvcMusicStore\Controllers** a vložit nový vlastní filtr **MyNewCustomActionFilter** do  **StoreController** třídy, jako je znázorněno v následujícím kódu.</span><span class="sxs-lookup"><span data-stu-id="58918-243">Open the **StoreController** class located at **MvcMusicStore\Controllers** and inject the new custom filter **MyNewCustomActionFilter** into **StoreController** class like is shown in the following code.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. <span data-ttu-id="58918-244">Dál spuštěním aplikace, abyste zjistili, jak tyto dva vlastní filtry akce fungují.</span><span class="sxs-lookup"><span data-stu-id="58918-244">Now, run the application in order to see how these two Custom Action Filters work.</span></span> <span data-ttu-id="58918-245">Chcete-li to provést, stiskněte **F5** a počkejte na spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="58918-245">To do this, press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="58918-246">Přejděte do **/ActionLog** zobrazíte počáteční stav zobrazení protokolu.</span><span class="sxs-lookup"><span data-stu-id="58918-246">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="58918-247">![Sledování stavu před aktivity stránku protokolu](aspnet-mvc-4-custom-action-filters/_static/image5.png "sledování stavu před aktivity stránku protokolu")</span><span class="sxs-lookup"><span data-stu-id="58918-247">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image5.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="58918-248">*Protokolu sledování stavu před aktivity stránku*</span><span class="sxs-lookup"><span data-stu-id="58918-248">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="58918-249">Klikněte na některou **žánry** z nabídky a provést některé akce, třeba k procházení alba k dispozici.</span><span class="sxs-lookup"><span data-stu-id="58918-249">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="58918-250">Zkontrolujte, že tento čas; vaší návštěvy byly sledovány dvakrát: jednou pro každý vlastní filtry akce, který jste přidali v kroku **kontroler úložiště** třídy.</span><span class="sxs-lookup"><span data-stu-id="58918-250">Check that this time; your visits were tracked twice: once for each of the Custom Action Filters you added in the **StorageController** class.</span></span>

    <span data-ttu-id="58918-251">![Protokol akcí s aktivitou protokoluje](aspnet-mvc-4-custom-action-filters/_static/image6.png "protokol akcí s aktivitou přihlášení")</span><span class="sxs-lookup"><span data-stu-id="58918-251">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image6.png "Action log with activity logged")</span></span>

    <span data-ttu-id="58918-252">*Protokol akcí s aktivita přihlášení*</span><span class="sxs-lookup"><span data-stu-id="58918-252">*Action log with activity logged*</span></span>
6. <span data-ttu-id="58918-253">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="58918-253">Close the browser.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a><span data-ttu-id="58918-254">Úloha 3: Správa řazení filtrů</span><span class="sxs-lookup"><span data-stu-id="58918-254">Task 3: Managing Filter Ordering</span></span>

<span data-ttu-id="58918-255">V této úloze se dozvíte, jak spravovat pořadí spuštění filtrů se s použitím určeno pořadí.</span><span class="sxs-lookup"><span data-stu-id="58918-255">In this task, you will learn how to manage the filters' execution order by using the Order propery.</span></span>

1. <span data-ttu-id="58918-256">Otevřít **StoreController** třídy nachází v **MvcMusicStore\Controllers** a zadejte **pořadí** vlastnost v obou filtrů, jako jsou uvedené dole.</span><span class="sxs-lookup"><span data-stu-id="58918-256">Open the **StoreController** class located at **MvcMusicStore\Controllers** and specify the **Order** property in both filters like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. <span data-ttu-id="58918-257">Ověřte nyní, jak filtry jsou provedeny v závislosti na hodnotě vlastnosti jeho pořadí.</span><span class="sxs-lookup"><span data-stu-id="58918-257">Now, verify how the filters are executed depending on its Order property's value.</span></span> <span data-ttu-id="58918-258">Zjistíte, že filtr s nejmenší hodnotou pořadí (**CustomActionFilter**) je první z nich, který se spouští.</span><span class="sxs-lookup"><span data-stu-id="58918-258">You will find that the filter with the smallest Order value (**CustomActionFilter**) is the first one that is executed.</span></span> <span data-ttu-id="58918-259">Stisknutím klávesy **F5** a počkejte na spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="58918-259">Press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="58918-260">Přejděte do **/ActionLog** zobrazíte počáteční stav zobrazení protokolu.</span><span class="sxs-lookup"><span data-stu-id="58918-260">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="58918-261">![Sledování stavu před aktivity stránku protokolu](aspnet-mvc-4-custom-action-filters/_static/image7.png "sledování stavu před aktivity stránku protokolu")</span><span class="sxs-lookup"><span data-stu-id="58918-261">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image7.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="58918-262">*Protokolu sledování stavu před aktivity stránku*</span><span class="sxs-lookup"><span data-stu-id="58918-262">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="58918-263">Klikněte na některou **žánry** z nabídky a provést některé akce, třeba k procházení alba k dispozici.</span><span class="sxs-lookup"><span data-stu-id="58918-263">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="58918-264">Zkontrolujte, že byly sledovány tentokrát vaší návštěvy seřazené podle hodnoty pořadí filtrů.: **CustomActionFilter** protokoly první.</span><span class="sxs-lookup"><span data-stu-id="58918-264">Check that this time, your visits were tracked ordered by the filters' Order value: **CustomActionFilter** logs' first.</span></span>

    <span data-ttu-id="58918-265">![Protokol akcí s aktivitou protokoluje](aspnet-mvc-4-custom-action-filters/_static/image8.png "protokol akcí s aktivitou přihlášení")</span><span class="sxs-lookup"><span data-stu-id="58918-265">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image8.png "Action log with activity logged")</span></span>

    <span data-ttu-id="58918-266">*Protokol akcí s aktivita přihlášení*</span><span class="sxs-lookup"><span data-stu-id="58918-266">*Action log with activity logged*</span></span>
6. <span data-ttu-id="58918-267">Nyní aktualizujte hodnotu pořadí filtrů a ověření, jak se mění pořadí protokolování.</span><span class="sxs-lookup"><span data-stu-id="58918-267">Now, you will update the Filters' order value and verify how the logging order changes.</span></span> <span data-ttu-id="58918-268">V **StoreController** třídy, aktualizujte hodnotu pořadí filtrů jako vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="58918-268">In the **StoreController** class, update the Filters' Order value like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. <span data-ttu-id="58918-269">Znovu spusťte aplikaci stisknutím klávesy **F5**.</span><span class="sxs-lookup"><span data-stu-id="58918-269">Run the application again by pressing **F5**.</span></span>
8. <span data-ttu-id="58918-270">Klikněte na některou **žánry** z nabídky a provést některé akce, třeba k procházení alba k dispozici.</span><span class="sxs-lookup"><span data-stu-id="58918-270">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
9. <span data-ttu-id="58918-271">Zkontrolujte, že současné době protokoly vytvořené **MyNewCustomActionFilter** filtru se zobrazí jako první.</span><span class="sxs-lookup"><span data-stu-id="58918-271">Check that this time, the logs created by **MyNewCustomActionFilter** filter appears first.</span></span>

    <span data-ttu-id="58918-272">![Protokol akcí s aktivitou protokoluje](aspnet-mvc-4-custom-action-filters/_static/image9.png "protokol akcí s aktivitou přihlášení")</span><span class="sxs-lookup"><span data-stu-id="58918-272">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image9.png "Action log with activity logged")</span></span>

    <span data-ttu-id="58918-273">*Protokol akcí s aktivita přihlášení*</span><span class="sxs-lookup"><span data-stu-id="58918-273">*Action log with activity logged*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a><span data-ttu-id="58918-274">Úloha 4: Registrace globálně filtry.</span><span class="sxs-lookup"><span data-stu-id="58918-274">Task 4: Registering Filters Globally</span></span>

<span data-ttu-id="58918-275">V této úloze budete aktualizovat řešení k registraci nového filtru (**MyNewCustomActionFilter**) jako globální filtr.</span><span class="sxs-lookup"><span data-stu-id="58918-275">In this task, you will update the solution to register the new filter (**MyNewCustomActionFilter**) as a global filter.</span></span> <span data-ttu-id="58918-276">Tímto způsobem se aktivuje ve všech akce základě nastaveného v aplikaci a ne jenom v StoreController ty stejně jako v předchozí úloze.</span><span class="sxs-lookup"><span data-stu-id="58918-276">By doing this, it will be triggered by all the actions perfomed in the application and not only in the StoreController ones as in the previous task.</span></span>

1. <span data-ttu-id="58918-277">V **StoreController** třídy, odeberte **[MyNewCustomActionFilter]** atribut a vlastnosti prostředí z **[CustomActionFilter]**.</span><span class="sxs-lookup"><span data-stu-id="58918-277">In **StoreController** class, remove **[MyNewCustomActionFilter]** attribute and the order property from **[CustomActionFilter]**.</span></span> <span data-ttu-id="58918-278">By měl vypadat nějak takto:</span><span class="sxs-lookup"><span data-stu-id="58918-278">It should look like the following:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. <span data-ttu-id="58918-279">Otevřít **Global.asax** souboru a vyhledejte **aplikace\_Start** metody.</span><span class="sxs-lookup"><span data-stu-id="58918-279">Open **Global.asax** file and locate the **Application\_Start** method.</span></span> <span data-ttu-id="58918-280">Všimněte si, že pokaždé, když spuštění aplikace se registruje globální filtry voláním **RegisterGlobalFilters** metody v rámci **FilterConfig** třídy.</span><span class="sxs-lookup"><span data-stu-id="58918-280">Notice that each time the application starts it is registering the global filters by calling **RegisterGlobalFilters** method within **FilterConfig** class.</span></span>

    <span data-ttu-id="58918-281">![Registrace globální filtry v souboru Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "registrace globální filtry v souboru Global.asax")</span><span class="sxs-lookup"><span data-stu-id="58918-281">![Registering Global Filters in Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registering Global Filters in Global.asax")</span></span>

    <span data-ttu-id="58918-282">*Registrace v souboru Global.asax globálních filtrů*</span><span class="sxs-lookup"><span data-stu-id="58918-282">*Registering Global Filters in Global.asax*</span></span>
3. <span data-ttu-id="58918-283">Otevřít **FilterConfig.cs** souborů v rámci **aplikace\_Start** složky.</span><span class="sxs-lookup"><span data-stu-id="58918-283">Open **FilterConfig.cs** file within **App\_Start** folder.</span></span>
4. <span data-ttu-id="58918-284">Přidejte odkaz na použití System.Web.Mvc; pomocí MvcMusicStore.Filters; obor názvů.</span><span class="sxs-lookup"><span data-stu-id="58918-284">Add a reference to using System.Web.Mvc; using MvcMusicStore.Filters; namespace.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. <span data-ttu-id="58918-285">Aktualizace **RegisterGlobalFilters** metoda přidání vlastního filtru.</span><span class="sxs-lookup"><span data-stu-id="58918-285">Update **RegisterGlobalFilters** method adding your custom filter.</span></span> <span data-ttu-id="58918-286">Chcete-li to provést, přidejte zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="58918-286">To do this, add the highlighted code:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. <span data-ttu-id="58918-287">Spusťte aplikaci stisknutím klávesy **F5**.</span><span class="sxs-lookup"><span data-stu-id="58918-287">Run the application by pressing **F5**.</span></span>
7. <span data-ttu-id="58918-288">Klikněte na některou **žánry** z nabídky a provést některé akce, třeba k procházení alba k dispozici.</span><span class="sxs-lookup"><span data-stu-id="58918-288">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
8. <span data-ttu-id="58918-289">Kontrola, která teď **[MyNewCustomActionFilter]** se se vloží v HomeController a ActionLogController příliš.</span><span class="sxs-lookup"><span data-stu-id="58918-289">Check that now **[MyNewCustomActionFilter]** is being injected in HomeController and ActionLogController too.</span></span>

    <span data-ttu-id="58918-290">![Protokol akcí s aktivitou protokoluje](aspnet-mvc-4-custom-action-filters/_static/image11.png "protokol akcí s aktivitou přihlášení")</span><span class="sxs-lookup"><span data-stu-id="58918-290">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image11.png "Action log with activity logged")</span></span>

    <span data-ttu-id="58918-291">*Protokol akcí s globální aktivita přihlášení*</span><span class="sxs-lookup"><span data-stu-id="58918-291">*Action log with global activity logged*</span></span>

> [!NOTE]
> <span data-ttu-id="58918-292">Kromě toho můžete nasadit tuto aplikaci následující weby Windows Azure [příloha B: publikování aplikace ASP.NET MVC 4 pomocí nasazení webu](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="58918-292">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="58918-293">Souhrn</span><span class="sxs-lookup"><span data-stu-id="58918-293">Summary</span></span>

<span data-ttu-id="58918-294">Po dokončení tohoto praktického testovacího prostředí jste se dozvěděli, jak rozšířit filtru akce k provedení vlastní akce.</span><span class="sxs-lookup"><span data-stu-id="58918-294">By completing this Hands-On Lab you have learned how to extend an action filter to execute custom actions.</span></span> <span data-ttu-id="58918-295">Můžete také se naučili, jak vložit libovolný filtr k vašim řadičům stránky.</span><span class="sxs-lookup"><span data-stu-id="58918-295">You have also learned how to inject any filter to your page controllers.</span></span> <span data-ttu-id="58918-296">Byly použity následující pojmy:</span><span class="sxs-lookup"><span data-stu-id="58918-296">The following concepts were used:</span></span>

- <span data-ttu-id="58918-297">Vytvoření vlastní akce filtry k třídě ASP.NET MVC ActionFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="58918-297">How to create Custom Action filters with the ASP.NET MVC ActionFilterAttribute class</span></span>
- <span data-ttu-id="58918-298">Jak vložit filtry do kontrolery ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="58918-298">How to inject filters into ASP.NET MVC controllers</span></span>
- <span data-ttu-id="58918-299">Správa pomocí vlastnosti pořadí řazení filtrů</span><span class="sxs-lookup"><span data-stu-id="58918-299">How to manage filter ordering using the Order property</span></span>
- <span data-ttu-id="58918-300">Postup při registraci filtry globálně</span><span class="sxs-lookup"><span data-stu-id="58918-300">How to register filters globally</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="58918-301">Příloha A: instalaci sady Visual Studio Express 2012 pro Web</span><span class="sxs-lookup"><span data-stu-id="58918-301">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="58918-302">Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiném &quot;Express&quot; verzí pomocí **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="58918-302">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="58918-303">Postupujte podle následujících pokynů vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="58918-303">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="58918-304">Přejděte na [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="58918-304">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="58918-305">Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produkt &quot; <em>Visual Studio Express 2012 pro Web se sadou Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="58918-305">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="58918-306">Klikněte na **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="58918-306">Click on **Install Now**.</span></span> <span data-ttu-id="58918-307">Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.</span><span class="sxs-lookup"><span data-stu-id="58918-307">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="58918-308">Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** spustit instalační program.</span><span class="sxs-lookup"><span data-stu-id="58918-308">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="58918-309">![Instalace sady Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "instalace sady Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="58918-309">![Install Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="58918-310">*Instalace sady Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="58918-310">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="58918-311">Čtení všech produktů licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="58918-311">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Přijetí podmínek licence](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    <span data-ttu-id="58918-313">*Přijetí podmínek licence*</span><span class="sxs-lookup"><span data-stu-id="58918-313">*Accepting the license terms*</span></span>
5. <span data-ttu-id="58918-314">Počkejte na dokončení procesu stahování a instalaci.</span><span class="sxs-lookup"><span data-stu-id="58918-314">Wait until the downloading and installation process completes.</span></span>

    ![Průběh instalace](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    <span data-ttu-id="58918-316">*Průběh instalace*</span><span class="sxs-lookup"><span data-stu-id="58918-316">*Installation progress*</span></span>
6. <span data-ttu-id="58918-317">Až instalace skončí, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="58918-317">When the installation completes, click **Finish**.</span></span>

    ![Instalace byla dokončena.](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    <span data-ttu-id="58918-319">*Instalace byla dokončena.*</span><span class="sxs-lookup"><span data-stu-id="58918-319">*Installation completed*</span></span>
7. <span data-ttu-id="58918-320">Klikněte na tlačítko **ukončovací** zavřete instalačního programu webové platformy.</span><span class="sxs-lookup"><span data-stu-id="58918-320">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="58918-321">Chcete-li spustit nástroj Visual Studio Express for Web, přejděte **Start** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express for Web** dlaždice.</span><span class="sxs-lookup"><span data-stu-id="58918-321">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web dlaždice](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    <span data-ttu-id="58918-323">*VS Express for Web dlaždice*</span><span class="sxs-lookup"><span data-stu-id="58918-323">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="58918-324">Příloha B: publikování aplikace ASP.NET MVC 4 pomocí nasazení webu</span><span class="sxs-lookup"><span data-stu-id="58918-324">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="58918-325">Tento dodatek se ukazují, jak vytvořit nový web z portálu správy Windows Azure a publikovat aplikace, kterou jste získali podle testovacího prostředí, využít Webdeploy funkce publikování ve Windows Azure k dispozici.</span><span class="sxs-lookup"><span data-stu-id="58918-325">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="58918-326">Úloha 1 – Vytvoření nového webu z Windows webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="58918-326">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="58918-327">Přejděte [Windows Azure Management Portal](https://manage.windowsazure.com/) a přihlaste se pomocí přihlašovacích údajů Microsoft spojených s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="58918-327">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="58918-328">Windows Azure můžete zadarmo hostovat 10 webů ASP.NET a pak škálujte podle rozšiřujícího se provozu.</span><span class="sxs-lookup"><span data-stu-id="58918-328">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="58918-329">Můžete se zaregistrovat [tady](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="58918-329">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="58918-330">![Přihlaste se k portálu Windows Azure](aspnet-mvc-4-custom-action-filters/_static/image17.png "Přihlaste se k portálu Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="58918-330">![Log on to Windows Azure portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="58918-331">*Přihlaste se k portálu pro správu Azure Windows*</span><span class="sxs-lookup"><span data-stu-id="58918-331">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="58918-332">Klikněte na tlačítko **nový** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="58918-332">Click **New** on the command bar.</span></span>

    <span data-ttu-id="58918-333">![Vytvoření nového webu](aspnet-mvc-4-custom-action-filters/_static/image18.png "vytváření nového webu")</span><span class="sxs-lookup"><span data-stu-id="58918-333">![Creating a new Web Site](aspnet-mvc-4-custom-action-filters/_static/image18.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="58918-334">*Vytvoření nového webu*</span><span class="sxs-lookup"><span data-stu-id="58918-334">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="58918-335">Klikněte na tlačítko **Compute** | **webu**.</span><span class="sxs-lookup"><span data-stu-id="58918-335">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="58918-336">Potom vyberte **rychlé vytvoření** možnost.</span><span class="sxs-lookup"><span data-stu-id="58918-336">Then select **Quick Create** option.</span></span> <span data-ttu-id="58918-337">Zadejte adresu URL k dispozici pro nový web a klikněte na tlačítko **vytvořit web**.</span><span class="sxs-lookup"><span data-stu-id="58918-337">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="58918-338">Hostitel pro webovou aplikaci spuštěnou v cloudu, který může řídit a spravovat je web Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="58918-338">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="58918-339">Možnost rychle vytvořit můžete nasadit hotové webové aplikace na Windows Azure web z mimo portál.</span><span class="sxs-lookup"><span data-stu-id="58918-339">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="58918-340">Nezahrnuje kroky pro vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="58918-340">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="58918-341">![Vytvoření nového webu pomocí metody rychlého vytvoření](aspnet-mvc-4-custom-action-filters/_static/image19.png "vytváření nového webu pomocí metody rychlého vytvoření")</span><span class="sxs-lookup"><span data-stu-id="58918-341">![Creating a new Web Site using Quick Create](aspnet-mvc-4-custom-action-filters/_static/image19.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="58918-342">*Vytvoření nového webu pomocí metody rychlého vytvoření*</span><span class="sxs-lookup"><span data-stu-id="58918-342">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="58918-343">Počkejte, dokud nové **webu** se vytvoří.</span><span class="sxs-lookup"><span data-stu-id="58918-343">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="58918-344">Po vytvoření webové stránky, klikněte na odkaz v části **URL** sloupce.</span><span class="sxs-lookup"><span data-stu-id="58918-344">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="58918-345">Zkontrolujte, jestli funguje nový web.</span><span class="sxs-lookup"><span data-stu-id="58918-345">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="58918-346">![Na nový web](aspnet-mvc-4-custom-action-filters/_static/image20.png "přechodu na nový web")</span><span class="sxs-lookup"><span data-stu-id="58918-346">![Browsing to the new web site](aspnet-mvc-4-custom-action-filters/_static/image20.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="58918-347">*Procházení na nový web*</span><span class="sxs-lookup"><span data-stu-id="58918-347">*Browsing to the new web site*</span></span>

    <span data-ttu-id="58918-348">![Spuštění webu](aspnet-mvc-4-custom-action-filters/_static/image21.png "spuštění webové stránky")</span><span class="sxs-lookup"><span data-stu-id="58918-348">![Web site running](aspnet-mvc-4-custom-action-filters/_static/image21.png "Web site running")</span></span>

    <span data-ttu-id="58918-349">*Spuštění webové stránky*</span><span class="sxs-lookup"><span data-stu-id="58918-349">*Web site running*</span></span>
6. <span data-ttu-id="58918-350">Přejděte zpět na portál a klikněte na název webové stránky v části **název** sloupec, který se zobrazí na stránkách správy.</span><span class="sxs-lookup"><span data-stu-id="58918-350">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="58918-351">![Otevřete správu webových stránek](aspnet-mvc-4-custom-action-filters/_static/image22.png "otevřete správu webových stránek")</span><span class="sxs-lookup"><span data-stu-id="58918-351">![Opening the web site management pages](aspnet-mvc-4-custom-action-filters/_static/image22.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="58918-352">*Otevřete správu webových stránek*</span><span class="sxs-lookup"><span data-stu-id="58918-352">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="58918-353">V **řídicí panel** stránce v části **rychlý přehled** klikněte na tlačítko **stáhnout profil publikování** odkaz.</span><span class="sxs-lookup"><span data-stu-id="58918-353">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="58918-354">*Profil publikování* obsahuje všechny informace požadované pro publikování webových aplikací na webu Windows Azure pro každou metodu povoleno publikování.</span><span class="sxs-lookup"><span data-stu-id="58918-354">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="58918-355">Profil publikování obsahuje adresy URL, přihlašovací údaje uživatele a databázové řetězce požadované k připojení a ověřování na základě jednotlivých koncových bodů, u kterých je povolena metoda publikace.</span><span class="sxs-lookup"><span data-stu-id="58918-355">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="58918-356">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** a **Microsoft Visual Studio 2012** podporují čtení publikační profily k automatizaci konfigurace z těchto programů pro publikování webových aplikací na Windows Azure websites.</span><span class="sxs-lookup"><span data-stu-id="58918-356">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="58918-357">![Stahování webové stránky publikovat profil](aspnet-mvc-4-custom-action-filters/_static/image23.png "stahování webové stránky profil publikování")</span><span class="sxs-lookup"><span data-stu-id="58918-357">![Downloading the web site publish profile](aspnet-mvc-4-custom-action-filters/_static/image23.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="58918-358">*Na webu stažení profilu publikování*</span><span class="sxs-lookup"><span data-stu-id="58918-358">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="58918-359">Stáhněte si soubor profilu publikování do vhodného umístění.</span><span class="sxs-lookup"><span data-stu-id="58918-359">Download the publish profile file to a known location.</span></span> <span data-ttu-id="58918-360">Dále v tomto cvičení uvidíte jak publikovat webovou aplikaci na weby Windows Azure ze sady Visual Studio pomocí tohoto souboru.</span><span class="sxs-lookup"><span data-stu-id="58918-360">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="58918-361">![Ukládání souboru profilu publikování](aspnet-mvc-4-custom-action-filters/_static/image24.png "ukládá se profil publikování")</span><span class="sxs-lookup"><span data-stu-id="58918-361">![Saving the publish profile file](aspnet-mvc-4-custom-action-filters/_static/image24.png "Saving the publish profile")</span></span>

    <span data-ttu-id="58918-362">*Ukládá se profil publikování*</span><span class="sxs-lookup"><span data-stu-id="58918-362">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="58918-363">Úloha 2 – konfigurování serveru databáze</span><span class="sxs-lookup"><span data-stu-id="58918-363">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="58918-364">Pokud vaše aplikace využívá SQL Server databáze, budete muset vytvořit server služby SQL Database.</span><span class="sxs-lookup"><span data-stu-id="58918-364">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="58918-365">Pokud chcete nasadit jednoduchou aplikaci, která nepoužívá SQL Server může tuto úlohu přeskočit.</span><span class="sxs-lookup"><span data-stu-id="58918-365">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="58918-366">Pro uložení databáze aplikace budete potřebovat databázi SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="58918-366">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="58918-367">Servery SQL Database můžete zobrazit ze svého předplatného na portálu správy Windows Azure na **databází Sql** | **servery** | **serveru Řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="58918-367">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="58918-368">Pokud nemáte server vytvořili, můžete vytvořit jednu **přidat** tlačítko na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="58918-368">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="58918-369">Poznamenejte si **název serveru a adresu URL, správce přihlašovací jméno a heslo**, jak je použijete v další úkoly.</span><span class="sxs-lookup"><span data-stu-id="58918-369">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="58918-370">Nevytvářet databáze, protože vytvoří se v pozdější fázi.</span><span class="sxs-lookup"><span data-stu-id="58918-370">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="58918-371">![Řídicí panel serveru SQL Database](aspnet-mvc-4-custom-action-filters/_static/image25.png "řídicího panelu serveru SQL Database")</span><span class="sxs-lookup"><span data-stu-id="58918-371">![SQL Database Server Dashboard](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="58918-372">*Řídicí panel serveru SQL Database*</span><span class="sxs-lookup"><span data-stu-id="58918-372">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="58918-373">V dalším úkolem budete testovat připojení k databázi ze sady Visual Studio z tohoto důvodu je nutné zahrnout místní IP adresa serveru seznamu **povolené IP adresy**.</span><span class="sxs-lookup"><span data-stu-id="58918-373">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="58918-374">Chcete-li to mohli udělat, klikněte na tlačítko **konfigurovat**, vyberte IP adresu z **aktuální IP adresa klienta** a vložte ho na **počáteční IP adresa** a **Koncová IP adresa** textová pole a klikněte na tlačítko ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) tlačítko.</span><span class="sxs-lookup"><span data-stu-id="58918-374">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) button.</span></span>

    ![Přidat IP adresu klienta](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    <span data-ttu-id="58918-376">*Přidat IP adresu klienta*</span><span class="sxs-lookup"><span data-stu-id="58918-376">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="58918-377">Jednou **IP adresa klienta** je přidat do povolených IP adres klikněte na tlačítko na **Uložit** potvrďte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="58918-377">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Potvrzení změn](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    <span data-ttu-id="58918-379">*Potvrzení změn*</span><span class="sxs-lookup"><span data-stu-id="58918-379">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="58918-380">Úloha 3 – publikování aplikace ASP.NET MVC 4 pomocí nasazení webu</span><span class="sxs-lookup"><span data-stu-id="58918-380">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="58918-381">Vraťte se do řešení ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="58918-381">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="58918-382">V **Průzkumníka řešení**, klikněte pravým tlačítkem na webový projekt a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="58918-382">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="58918-383">![Publikování aplikace](aspnet-mvc-4-custom-action-filters/_static/image29.png "publikování aplikace")</span><span class="sxs-lookup"><span data-stu-id="58918-383">![Publishing the Application](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publishing the Application")</span></span>

    <span data-ttu-id="58918-384">*Publikování na webu*</span><span class="sxs-lookup"><span data-stu-id="58918-384">*Publishing the web site*</span></span>
2. <span data-ttu-id="58918-385">Importujte profil publikování, který jste uložili v první úloze.</span><span class="sxs-lookup"><span data-stu-id="58918-385">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="58918-386">![Import profilu publikování](aspnet-mvc-4-custom-action-filters/_static/image30.png "import profilu publikování")</span><span class="sxs-lookup"><span data-stu-id="58918-386">![Importing the publish profile](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importing the publish profile")</span></span>

    <span data-ttu-id="58918-387">*Import publikačního profilu*</span><span class="sxs-lookup"><span data-stu-id="58918-387">*Importing publish profile*</span></span>
3. <span data-ttu-id="58918-388">Klikněte na tlačítko **ověřit připojení**.</span><span class="sxs-lookup"><span data-stu-id="58918-388">Click **Validate Connection**.</span></span> <span data-ttu-id="58918-389">Po dokončení ověření klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="58918-389">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="58918-390">Ověření bylo dokončeno, jakmile se zobrazí zelené zaškrtnutí vedle tlačítka ověřit připojení.</span><span class="sxs-lookup"><span data-stu-id="58918-390">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="58918-391">![Ověřuje se připojení](aspnet-mvc-4-custom-action-filters/_static/image31.png "ověřuje se připojení")</span><span class="sxs-lookup"><span data-stu-id="58918-391">![Validating connection](aspnet-mvc-4-custom-action-filters/_static/image31.png "Validating connection")</span></span>

    <span data-ttu-id="58918-392">*Ověřuje se připojení*</span><span class="sxs-lookup"><span data-stu-id="58918-392">*Validating connection*</span></span>
4. <span data-ttu-id="58918-393">V **nastavení** stránce v části **databází** klikněte na tlačítko vedle textového pole připojení k databázi (to znamená **objekt DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="58918-393">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="58918-394">![Konfigurace nasazení webu](aspnet-mvc-4-custom-action-filters/_static/image32.png "konfigurace nasazení webu")</span><span class="sxs-lookup"><span data-stu-id="58918-394">![Web deploy configuration](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web deploy configuration")</span></span>

    <span data-ttu-id="58918-395">*Konfigurace nasazení webu*</span><span class="sxs-lookup"><span data-stu-id="58918-395">*Web deploy configuration*</span></span>
5. <span data-ttu-id="58918-396">Konfigurace připojení k databázi následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="58918-396">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="58918-397">V **název serveru** zadejte vaše pomocí adresy URL databáze SQL serveru *tcp:* předponu.</span><span class="sxs-lookup"><span data-stu-id="58918-397">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="58918-398">V **uživatelské jméno** zadejte vaše přihlašovací jméno správce serveru.</span><span class="sxs-lookup"><span data-stu-id="58918-398">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="58918-399">V **heslo** zadejte heslo pro přihlašovací jméno správce serveru.</span><span class="sxs-lookup"><span data-stu-id="58918-399">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="58918-400">Zadejte nový název databáze.</span><span class="sxs-lookup"><span data-stu-id="58918-400">Type a new database name.</span></span>

     <span data-ttu-id="58918-401">![Konfigurace cílový připojovací řetězec](aspnet-mvc-4-custom-action-filters/_static/image33.png "konfigurace cílový připojovací řetězec")</span><span class="sxs-lookup"><span data-stu-id="58918-401">![Configuring destination connection string](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="58918-402">*Konfigurace cílový připojovací řetězec*</span><span class="sxs-lookup"><span data-stu-id="58918-402">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="58918-403">Pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="58918-403">Then click **OK**.</span></span> <span data-ttu-id="58918-404">Po zobrazení výzvy k vytvoření databáze klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="58918-404">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="58918-405">![Vytvoření databáze](aspnet-mvc-4-custom-action-filters/_static/image34.png "vytvoření řetězce databáze")</span><span class="sxs-lookup"><span data-stu-id="58918-405">![Creating the database](aspnet-mvc-4-custom-action-filters/_static/image34.png "Creating the database string")</span></span>

    <span data-ttu-id="58918-406">*Vytvoření databáze*</span><span class="sxs-lookup"><span data-stu-id="58918-406">*Creating the database*</span></span>
7. <span data-ttu-id="58918-407">Připojovací řetězec, který budete používat pro připojení k databázi SQL ve službě Windows Azure se zobrazí v textovém poli výchozí připojení.</span><span class="sxs-lookup"><span data-stu-id="58918-407">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="58918-408">Pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="58918-408">Then click **Next**.</span></span>

    <span data-ttu-id="58918-409">![Připojovací řetězec odkazující na SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "připojovací řetězec odkazující na SQL Database")</span><span class="sxs-lookup"><span data-stu-id="58918-409">![Connection string pointing to SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="58918-410">*Připojovací řetězec odkazující na SQL Database*</span><span class="sxs-lookup"><span data-stu-id="58918-410">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="58918-411">V **ve verzi Preview** klikněte na **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="58918-411">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="58918-412">![Publikování webové aplikace](aspnet-mvc-4-custom-action-filters/_static/image36.png "publikování webové aplikace")</span><span class="sxs-lookup"><span data-stu-id="58918-412">![Publishing the web application](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publishing the web application")</span></span>

    <span data-ttu-id="58918-413">*Publikování webové aplikace*</span><span class="sxs-lookup"><span data-stu-id="58918-413">*Publishing the web application*</span></span>
9. <span data-ttu-id="58918-414">Až se proces publikování dokončí, otevře se váš výchozí prohlížeč publikovaného webu.</span><span class="sxs-lookup"><span data-stu-id="58918-414">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="58918-415">Příloha C: používání fragmentů kódu</span><span class="sxs-lookup"><span data-stu-id="58918-415">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="58918-416">Pomocí fragmentů kódu máte všechny kód, který je třeba na dosah ruky.</span><span class="sxs-lookup"><span data-stu-id="58918-416">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="58918-417">Testovací prostředí dokumentu zjistíte přesně kdy je můžete využít, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="58918-417">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="58918-418">![Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu](aspnet-mvc-4-custom-action-filters/_static/image37.png "pomocí sady Visual Studio fragmenty kódu pro vložení kódu do projektu")</span><span class="sxs-lookup"><span data-stu-id="58918-418">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-custom-action-filters/_static/image37.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="58918-419">*Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu*</span><span class="sxs-lookup"><span data-stu-id="58918-419">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="58918-420">***Chcete-li přidat fragment kódu pomocí klávesnice (C# jenom)***</span><span class="sxs-lookup"><span data-stu-id="58918-420">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="58918-421">Umístěte kurzor, kam chcete vložit kód.</span><span class="sxs-lookup"><span data-stu-id="58918-421">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="58918-422">Začněte psát název fragmentu kódu (bez mezer nebo pomlčky).</span><span class="sxs-lookup"><span data-stu-id="58918-422">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="58918-423">Podívejte se na jako IntelliSense zobrazí odpovídající názvy fragmenty kódu.</span><span class="sxs-lookup"><span data-stu-id="58918-423">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="58918-424">Vyberte správný fragment kódu (nebo pokračujte v psaní dokud nebude vybraný celý fragment název).</span><span class="sxs-lookup"><span data-stu-id="58918-424">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="58918-425">Stiskněte klávesu Tabulátor dvakrát pro vložení fragmentu do umístění kurzoru.</span><span class="sxs-lookup"><span data-stu-id="58918-425">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="58918-426">![Začněte psát název fragmentu kódu](aspnet-mvc-4-custom-action-filters/_static/image38.png "začněte psát název fragmentu kódu")</span><span class="sxs-lookup"><span data-stu-id="58918-426">![Start typing the snippet name](aspnet-mvc-4-custom-action-filters/_static/image38.png "Start typing the snippet name")</span></span>

<span data-ttu-id="58918-427">*Začněte psát název fragmentu kódu*</span><span class="sxs-lookup"><span data-stu-id="58918-427">*Start typing the snippet name*</span></span>

<span data-ttu-id="58918-428">![Stiskněte klávesu Tab k vybrání fragmentu zvýrazněné](aspnet-mvc-4-custom-action-filters/_static/image39.png "stisknutím klávesy Tab k výběru zvýrazněné fragment kódu")</span><span class="sxs-lookup"><span data-stu-id="58918-428">![Press Tab to select the highlighted snippet](aspnet-mvc-4-custom-action-filters/_static/image39.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="58918-429">*Stiskněte klávesu Tab k výběru zvýrazněné fragment kódu*</span><span class="sxs-lookup"><span data-stu-id="58918-429">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="58918-430">![Stisknutím klávesy Tab znovu a fragment kódu se rozbalí](aspnet-mvc-4-custom-action-filters/_static/image40.png "znovu stisknutím klávesy Tab a fragment kódu se rozbalí.")</span><span class="sxs-lookup"><span data-stu-id="58918-430">![Press Tab again and the snippet will expand](aspnet-mvc-4-custom-action-filters/_static/image40.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="58918-431">*Stisknutím klávesy Tab znovu a fragment kódu se rozbalí.*</span><span class="sxs-lookup"><span data-stu-id="58918-431">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="58918-432">***Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="58918-432">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="58918-433">Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="58918-433">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="58918-434">Vyberte **Vložit fragment** následovaný **Moje fragmenty kódu**.</span><span class="sxs-lookup"><span data-stu-id="58918-434">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="58918-435">Kliknutím na vyberte relevantní fragment kódu ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="58918-435">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="58918-436">![Klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment](aspnet-mvc-4-custom-action-filters/_static/image41.png "klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment")</span><span class="sxs-lookup"><span data-stu-id="58918-436">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-custom-action-filters/_static/image41.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="58918-437">*Klikněte pravým tlačítkem na, ve které chcete vložit fragment kódu a vyberte Vložit fragment*</span><span class="sxs-lookup"><span data-stu-id="58918-437">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="58918-438">![Vyberte si relevantní fragment kódu ze seznamu, kliknutím na](aspnet-mvc-4-custom-action-filters/_static/image42.png "relevantní fragment kódu ze seznamu vyberte kliknutím na")</span><span class="sxs-lookup"><span data-stu-id="58918-438">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-custom-action-filters/_static/image42.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="58918-439">*Vyberte si relevantní fragment kódu ze seznamu, kliknutím na*</span><span class="sxs-lookup"><span data-stu-id="58918-439">*Pick the relevant snippet from the list, by clicking on it*</span></span>
