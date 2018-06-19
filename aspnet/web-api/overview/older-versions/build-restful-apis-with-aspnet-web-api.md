---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Sestavení rozhraní RESTful API s rozhraním ASP.NET Web API | Microsoft Docs
author: rick-anderson
description: V posledních letech se stal jasné, že HTTP není je jen pro poskytovat stránky HTML. Je také efektivní platformu pro sestavování rozhraní Web API, pomocí o několik...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: cb02288e93be801a1e55852741ed1443d8d3617d
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/18/2018
ms.locfileid: "34306881"
---
<a name="build-restful-apis-with-aspnet-web-api"></a><span data-ttu-id="0439b-104">Sestavení rozhraní RESTful API s rozhraním ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="0439b-104">Build RESTful APIs with ASP.NET Web API</span></span>
====================
<span data-ttu-id="0439b-105">Podle [webové táborech Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="0439b-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="0439b-106">V posledních letech se stal jasné, že HTTP není je jen pro poskytovat stránky HTML.</span><span class="sxs-lookup"><span data-stu-id="0439b-106">In recent years, it has become clear that HTTP is not just for serving up HTML pages.</span></span> <span data-ttu-id="0439b-107">Je také efektivní platformu pro sestavování rozhraní Web API, například pomocí několik příkazů (GET, POST a tak dále) plus pár jednoduchými koncepty *identifikátory URI* a *hlavičky*.</span><span class="sxs-lookup"><span data-stu-id="0439b-107">It is also a powerful platform for building Web APIs, using a handful of verbs (GET, POST, and so forth) plus a few simple concepts such as *URIs* and *headers*.</span></span> <span data-ttu-id="0439b-108">Rozhraní ASP.NET Web API je sada komponent, které zjednodušují programování HTTP.</span><span class="sxs-lookup"><span data-stu-id="0439b-108">ASP.NET Web API is a set of components that simplify HTTP programming.</span></span> <span data-ttu-id="0439b-109">Protože je založen na modul runtime rozhraní ASP.NET MVC, webového rozhraní API automaticky zpracovává podrobnosti nízké úrovně přenos HTTP.</span><span class="sxs-lookup"><span data-stu-id="0439b-109">Because it is built on top of the ASP.NET MVC runtime, Web API automatically handles the low-level transport details of HTTP.</span></span> <span data-ttu-id="0439b-110">Současně zpřístupní webového rozhraní API přirozeně programovací model protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="0439b-110">At the same time, Web API naturally exposes the HTTP programming model.</span></span> <span data-ttu-id="0439b-111">Ve skutečnosti jeden cílem webového rozhraní API je *není* abstraktní rychle když ve skutečnosti HTTP.</span><span class="sxs-lookup"><span data-stu-id="0439b-111">In fact, one goal of Web API is to *not* abstract away the reality of HTTP.</span></span> <span data-ttu-id="0439b-112">V důsledku toho webového rozhraní API je flexibilní a snadné rozšíření.</span><span class="sxs-lookup"><span data-stu-id="0439b-112">As a result, Web API is both flexible and easy to extend.</span></span> <span data-ttu-id="0439b-113">V tomto testovacím prostředí praktických použije k vytvoření jednoduché rozhraní REST API pro kontaktujte správce aplikace webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0439b-113">In this hands-on lab, you will use Web API to build a simple REST API for a contact manager application.</span></span> <span data-ttu-id="0439b-114">Pokud vytvoříte klienta využívají rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0439b-114">You will also build a client to consume the API.</span></span> <span data-ttu-id="0439b-115">Styl architektury REST ukázal jako efektivní způsob, jak využít HTTP -, i když není určitě pouze platný přístup k protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="0439b-115">The REST architectural style has proven to be an effective way to leverage HTTP - although it is certainly not the only valid approach to HTTP.</span></span> <span data-ttu-id="0439b-116">Kontaktujte správce zveřejní dosáhl standardu RESTful pro výpis, přidávání a odebírání kontakty, mimo jiné.</span><span class="sxs-lookup"><span data-stu-id="0439b-116">The contact manager will expose the RESTful for listing, adding and removing contacts, among others.</span></span> <span data-ttu-id="0439b-117">Toto testovací prostředí vyžaduje základní znalosti protokolu HTTP, REST a předpokládá, že máte základní praktické znalosti jazyka HTML, JavaScript a jQuery.</span><span class="sxs-lookup"><span data-stu-id="0439b-117">This lab requires a basic understanding of HTTP, REST, and assumes you have a basic working knowledge of HTML, JavaScript, and jQuery.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="0439b-118">Má oblast vyhrazený pro rozhraní ASP.NET Web API na webu technologie ASP.NET [ https://asp.net/web-api ](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="0439b-118">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [https://asp.net/web-api](https://asp.net/web-api).</span></span> <span data-ttu-id="0439b-119">Tento web bude pokračovat v poskytování nejnovější informace, ukázky a zprávy související s webového rozhraní API, tak to zkuste ho často Pokud byste chtěli pustíte hlubší do obrázky vytváření vlastní webová rozhraní API k dispozici pro prakticky libovolný zařízení nebo vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="0439b-119">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>
> > 
> > <span data-ttu-id="0439b-120">Rozhraní ASP.NET Web API, podobné technologie ASP.NET MVC 4, má flexibilitu z hlediska oddělení vrstvě služby z řadičů budete moci použít některé z dostupných vkládání závislostí architektury velmi snadná.</span><span class="sxs-lookup"><span data-stu-id="0439b-120">ASP.NET Web API, similar to ASP.NET MVC 4, has great flexibility in terms of separating the service layer from the controllers allowing you to use several of the available Dependency Injection frameworks fairly easy.</span></span> <span data-ttu-id="0439b-121">Je dobré vzorku v MSDN, který ukazuje způsob použití Ninject pro vkládání závislostí v projektu webového rozhraní API ASP.NET, které si můžete stáhnout z [zde](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span><span class="sxs-lookup"><span data-stu-id="0439b-121">There is a good sample in MSDN that shows how to use Ninject for dependency injection in an ASP.NET Web API project that you can download it from [here](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span></span>
> 
> 
> <span data-ttu-id="0439b-122">Všechny ukázky kódu a fragmenty kódu jsou součástí webové táborech školení sady, k dispozici na [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="0439b-122">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="0439b-123">Cíle</span><span class="sxs-lookup"><span data-stu-id="0439b-123">Objectives</span></span>

<span data-ttu-id="0439b-124">V tomto testovacím prostředí praktických se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="0439b-124">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="0439b-125">Implementace RESTful webová rozhraní API</span><span class="sxs-lookup"><span data-stu-id="0439b-125">Implement a RESTful Web API</span></span>
- <span data-ttu-id="0439b-126">Volání rozhraní API z klienta HTML</span><span class="sxs-lookup"><span data-stu-id="0439b-126">Call the API from an HTML client</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="0439b-127">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0439b-127">Prerequisites</span></span>

<span data-ttu-id="0439b-128">Pro dokončení této praktické cvičení je vyžadován následující text:</span><span class="sxs-lookup"><span data-stu-id="0439b-128">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="0439b-129">[Microsoft Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) nebo i vyšší (čtení [příloha B](#AppendixB) pokyny o tom, jak ji nainstalovat).</span><span class="sxs-lookup"><span data-stu-id="0439b-129">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="0439b-130">Instalace</span><span class="sxs-lookup"><span data-stu-id="0439b-130">Setup</span></span>

<span data-ttu-id="0439b-131">**Instalace fragmenty kódu**</span><span class="sxs-lookup"><span data-stu-id="0439b-131">**Installing Code Snippets**</span></span>

<span data-ttu-id="0439b-132">Pro usnadnění práce většinu kódu, který bude spravovat podél toto testovací prostředí je k dispozici jako fragmenty kódu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0439b-132">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="0439b-133">K instalaci fragmenty kódu spustit **.\Source\Setup\CodeSnippets.vsi** souboru.</span><span class="sxs-lookup"><span data-stu-id="0439b-133">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="0439b-134">Pokud si nejste obeznámeni s fragmenty kódu Visual Studio a chcete se dozvíte, jak používat, můžete odkazovat na příloze z tohoto dokumentu &quot; [fragmenty kódu pomocí příloha A:](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="0439b-134">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="0439b-135">Cvičení</span><span class="sxs-lookup"><span data-stu-id="0439b-135">Exercises</span></span>

<span data-ttu-id="0439b-136">Toto praktické cvičení zahrnuje následující cvičení:</span><span class="sxs-lookup"><span data-stu-id="0439b-136">This hands-on lab includes the following exercise:</span></span>

1. [<span data-ttu-id="0439b-137">Cvičení 1: Vytvoření jen pro čtení webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="0439b-137">Exercise 1: Create a Read-Only Web API</span></span>](#Exercise1)
2. [<span data-ttu-id="0439b-138">Cvičení 2: Vytvoření webové rozhraní API pro čtení i zápis</span><span class="sxs-lookup"><span data-stu-id="0439b-138">Exercise 2: Create a Read/Write Web API</span></span>](#Exercise2)
3. [<span data-ttu-id="0439b-139">Cvičení 3: Využívají webové rozhraní API z klienta HTML</span><span class="sxs-lookup"><span data-stu-id="0439b-139">Exercise 3: Consume the Web API from an HTML Client</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="0439b-140">Každý úkol je přiložena **End** složku obsahující výsledné řešení by si měly opatřit po dokončení cvičeních.</span><span class="sxs-lookup"><span data-stu-id="0439b-140">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="0439b-141">Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc při procházení cvičeních.</span><span class="sxs-lookup"><span data-stu-id="0439b-141">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="0439b-142">Odhadovaný čas dokončení tohoto testovacího prostředí: **60 minut**.</span><span class="sxs-lookup"><span data-stu-id="0439b-142">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a><span data-ttu-id="0439b-143">Cvičení 1: Vytvoření jen pro čtení webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="0439b-143">Exercise 1: Create a Read-Only Web API</span></span>

<span data-ttu-id="0439b-144">V tomto cvičení budete implementovat metodu GET jen pro čtení pro kontaktujte správce.</span><span class="sxs-lookup"><span data-stu-id="0439b-144">In this exercise, you will implement the read-only GET methods for the contact manager.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a><span data-ttu-id="0439b-145">Úloha 1 – Vytvoření projektu rozhraní API</span><span class="sxs-lookup"><span data-stu-id="0439b-145">Task 1 - Creating the API Project</span></span>

<span data-ttu-id="0439b-146">Nové šablony webových projektů ASP.NET v této úloze, použije k vytvoření webové aplikace webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0439b-146">In this task, you will use the new ASP.NET web project templates to create a Web API web application.</span></span>

1. <span data-ttu-id="0439b-147">Spustit **Visual Studio 2012 Express pro Web**, přejděte k **spustit** a typ **VS Express pro Web** stiskněte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="0439b-147">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="0439b-148">Z **soubor** nabídce vyberte možnost **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="0439b-148">From the **File** menu, select **New Project**.</span></span> <span data-ttu-id="0439b-149">Vyberte **Visual C# | Webové** projektu typu ze zobrazení stromu typ projektu a pak vyberte **webové aplikace ASP.NET MVC 4** typ projektu.</span><span class="sxs-lookup"><span data-stu-id="0439b-149">Select the **Visual C# | Web** project type from the project type tree view, then select the **ASP.NET MVC 4 Web Application** project type.</span></span> <span data-ttu-id="0439b-150">Nastavení projektu **název** k *ContactManager* a **název řešení** k *začít*, pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="0439b-150">Set the project's **Name** to *ContactManager* and the **Solution name** to *Begin*, then click **OK**.</span></span>

    <span data-ttu-id="0439b-151">![Vytvoření nové webové aplikace projektu 4.0 MVC ASP.NET](build-restful-apis-with-aspnet-web-api/_static/image1.png "vytváření nové technologie ASP.NET MVC 4.0 projekt webové aplikace")</span><span class="sxs-lookup"><span data-stu-id="0439b-151">![Creating a new ASP.NET MVC 4.0 Web Application Project](build-restful-apis-with-aspnet-web-api/_static/image1.png "Creating a new ASP.NET MVC 4.0 Web Application Project")</span></span>

    <span data-ttu-id="0439b-152">*Vytvoření nové webové aplikace projektu 4.0 MVC ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="0439b-152">*Creating a new ASP.NET MVC 4.0 Web Application Project*</span></span>
3. <span data-ttu-id="0439b-153">V dialogu typ projektu ASP.NET MVC 4, vyberte **webového rozhraní API** typ projektu.</span><span class="sxs-lookup"><span data-stu-id="0439b-153">In the ASP.NET MVC 4 project type dialog, select the **Web API** project type.</span></span> <span data-ttu-id="0439b-154">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="0439b-154">Click **OK**.</span></span>

    <span data-ttu-id="0439b-155">![Určení typu projekt webového rozhraní API](build-restful-apis-with-aspnet-web-api/_static/image2.png "určující typ projektu webového rozhraní API")</span><span class="sxs-lookup"><span data-stu-id="0439b-155">![Specifying the Web API project type](build-restful-apis-with-aspnet-web-api/_static/image2.png "Specifying the Web API project type")</span></span>

    <span data-ttu-id="0439b-156">*Určení typu projekt webového rozhraní API*</span><span class="sxs-lookup"><span data-stu-id="0439b-156">*Specifying the Web API project type*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a><span data-ttu-id="0439b-157">Úloha 2 – Vytvoření řadičů API obraťte se na správce</span><span class="sxs-lookup"><span data-stu-id="0439b-157">Task 2 - Creating the Contact Manager API Controllers</span></span>

<span data-ttu-id="0439b-158">V této úloze vytvoří řadič třídy, ve kterých se bude nacházet metody rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0439b-158">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="0439b-159">Odstraňte soubor s názvem **ValuesController.cs** v rámci **řadiče** složku z projektu.</span><span class="sxs-lookup"><span data-stu-id="0439b-159">Delete the file named **ValuesController.cs** within **Controllers** folder from the project.</span></span>
2. <span data-ttu-id="0439b-160">Klikněte pravým tlačítkem myši **řadiče** složky v projektu a vyberte **přidat | Řadič** v místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="0439b-160">Right-click the **Controllers** folder in the project and select **Add | Controller** from the context menu.</span></span>

    <span data-ttu-id="0439b-161">![Přidávání nového řadiče do projektu](build-restful-apis-with-aspnet-web-api/_static/image3.png "přidávání nového řadiče do projektu")</span><span class="sxs-lookup"><span data-stu-id="0439b-161">![Adding a new controller to the project](build-restful-apis-with-aspnet-web-api/_static/image3.png "Adding a new controller to the project")</span></span>

    <span data-ttu-id="0439b-162">*Přidávání nového řadiče do projektu*</span><span class="sxs-lookup"><span data-stu-id="0439b-162">*Adding a new controller to the project*</span></span>
3. <span data-ttu-id="0439b-163">V **přidat kontroler** dialog, který se zobrazí, vyberte **prázdný kontroler API** z nabídky šablony.</span><span class="sxs-lookup"><span data-stu-id="0439b-163">In the **Add Controller** dialog that appears, select **Empty API Controller** from the Template menu.</span></span> <span data-ttu-id="0439b-164">Název třídy kontroleru **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="0439b-164">Name the controller class **ContactController**.</span></span> <span data-ttu-id="0439b-165">Potom klikněte na **přidat.**</span><span class="sxs-lookup"><span data-stu-id="0439b-165">Then, click **Add.**</span></span>

    <span data-ttu-id="0439b-166">![V dialogovém okně Přidat kontroler k vytvoření nového řadiče webového rozhraní API](build-restful-apis-with-aspnet-web-api/_static/image4.png "pomocí dialogu přidat kontroler k vytvoření nového řadiče webového rozhraní API")</span><span class="sxs-lookup"><span data-stu-id="0439b-166">![Using the Add Controller dialog to create a new Web API controller](build-restful-apis-with-aspnet-web-api/_static/image4.png "Using the Add Controller dialog to create a new Web API controller")</span></span>

    <span data-ttu-id="0439b-167">*V dialogovém okně Přidat kontroler k vytvoření nového řadiče webového rozhraní API*</span><span class="sxs-lookup"><span data-stu-id="0439b-167">*Using the Add Controller dialog to create a new Web API controller*</span></span>
4. <span data-ttu-id="0439b-168">Přidejte následující kód, který **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="0439b-168">Add the following code to the **ContactController**.</span></span>

    <span data-ttu-id="0439b-169">(Code fragment kódu - *webové rozhraní API prostředí - Ex01 - Get – metoda API*)</span><span class="sxs-lookup"><span data-stu-id="0439b-169">(Code Snippet - *Web API Lab - Ex01 - Get API Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. <span data-ttu-id="0439b-170">Stiskněte klávesu **F5** k ladění aplikace.</span><span class="sxs-lookup"><span data-stu-id="0439b-170">Press **F5** to debug the application.</span></span> <span data-ttu-id="0439b-171">By se zobrazit výchozí domovskou stránku pro projekt webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0439b-171">The default home page for a Web API project should appear.</span></span>

    <span data-ttu-id="0439b-172">![Výchozí domovskou stránku aplikace rozhraní ASP.NET Web API](build-restful-apis-with-aspnet-web-api/_static/image5.png "výchozí domovskou stránku aplikace rozhraní ASP.NET Web API")</span><span class="sxs-lookup"><span data-stu-id="0439b-172">![The default home page of an ASP.NET Web API application](build-restful-apis-with-aspnet-web-api/_static/image5.png "The default home page of an ASP.NET Web API application")</span></span>

    <span data-ttu-id="0439b-173">*Výchozí domovskou stránku aplikace rozhraní ASP.NET Web API*</span><span class="sxs-lookup"><span data-stu-id="0439b-173">*The default home page of an ASP.NET Web API application*</span></span>
6. <span data-ttu-id="0439b-174">V okně Internet Exploreru, stiskněte **F12** klíč otevřete **Developer Tools** okno.</span><span class="sxs-lookup"><span data-stu-id="0439b-174">In the Internet Explorer window, press the **F12** key to open the **Developer Tools** window.</span></span> <span data-ttu-id="0439b-175">Klikněte **sítě** a pak klikněte **spustit zachytávání** tlačítko Začněte zachytávající síťový přenos do okna.</span><span class="sxs-lookup"><span data-stu-id="0439b-175">Click the **Network** tab, and then click the **Start Capturing** button to begin capturing network traffic into the window.</span></span>

    <span data-ttu-id="0439b-176">![Otevřete kartu síť a inicializaci sítě zaznamenat](build-restful-apis-with-aspnet-web-api/_static/image6.png "otevírání na síťové kartě a inicializaci sítě zachycení")</span><span class="sxs-lookup"><span data-stu-id="0439b-176">![Opening the network tab and initiating network capture](build-restful-apis-with-aspnet-web-api/_static/image6.png "Opening the network tab and initiating network capture")</span></span>

    <span data-ttu-id="0439b-177">*Otevřete kartu síť a inicializaci zachycení dat ze sítě*</span><span class="sxs-lookup"><span data-stu-id="0439b-177">*Opening the network tab and initiating network capture*</span></span>
7. <span data-ttu-id="0439b-178">Připojit adresu URL v adresním řádku prohlížeče s **/api/kontakt** a stiskněte klávesu enter.</span><span class="sxs-lookup"><span data-stu-id="0439b-178">Append the URL in the browser's address bar with **/api/contact** and press enter.</span></span> <span data-ttu-id="0439b-179">Podrobnosti o přenos se zobrazí v okně Sběr sítě.</span><span class="sxs-lookup"><span data-stu-id="0439b-179">The transmission details will appear in the network capture window.</span></span> <span data-ttu-id="0439b-180">Všimněte si, že je typ MIME do odpovědi **application/json**.</span><span class="sxs-lookup"><span data-stu-id="0439b-180">Note that the response's MIME type is **application/json**.</span></span> <span data-ttu-id="0439b-181">Tento příklad ukazuje, jak je výchozí formát výstupu JSON.</span><span class="sxs-lookup"><span data-stu-id="0439b-181">This demonstrates how the default output format is JSON.</span></span>

    <span data-ttu-id="0439b-182">![Zobrazení výstupu webového rozhraní API žádosti v zobrazení sítě](build-restful-apis-with-aspnet-web-api/_static/image7.png "zobrazení výstupu webového rozhraní API žádosti v zobrazení sítě")</span><span class="sxs-lookup"><span data-stu-id="0439b-182">![Viewing the output of the Web API request in the Network view](build-restful-apis-with-aspnet-web-api/_static/image7.png "Viewing the output of the Web API request in the Network view")</span></span>

    <span data-ttu-id="0439b-183">*Zobrazení výstupu webového rozhraní API žádosti v zobrazení sítě*</span><span class="sxs-lookup"><span data-stu-id="0439b-183">*Viewing the output of the Web API request in the Network view*</span></span>

    > [!NOTE]
    > <span data-ttu-id="0439b-184">Internet Explorer 10 výchozí chování v tomto okamžiku bude požádat, pokud by uživatel chtěl uložit nebo otevřít datový proud vyplývající z volání webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0439b-184">Internet Explorer 10's default behavior at this point will be to ask if the user would like to save or open the stream resulting from the Web API call.</span></span> <span data-ttu-id="0439b-185">Výstup bude textového souboru obsahujícího JSON výsledek volání adresy URL webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0439b-185">The output will be a text file containing the JSON result of the Web API URL call.</span></span> <span data-ttu-id="0439b-186">Aby bylo možné sledovat obsah do odpovědi prostřednictvím okno nástroje vývojáři zrušit není dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0439b-186">Do not cancel the dialog in order to be able to watch the response's content through Developers Tool window.</span></span>
8. <span data-ttu-id="0439b-187">Klikněte **přejít na podrobné zobrazení** tlačítko zobrazíte další podrobnosti o odpovědi toto volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0439b-187">Click the **Go to detailed view** button to see more details about the response of this API call.</span></span>

    <span data-ttu-id="0439b-188">![Přepnout na podrobné zobrazení](build-restful-apis-with-aspnet-web-api/_static/image8.png "přepnout na zobrazení podrobností")</span><span class="sxs-lookup"><span data-stu-id="0439b-188">![Switch to Detailed View](build-restful-apis-with-aspnet-web-api/_static/image8.png "Switch to Details View")</span></span>

    <span data-ttu-id="0439b-189">*Přepnout na podrobné zobrazení*</span><span class="sxs-lookup"><span data-stu-id="0439b-189">*Switch to Detailed View*</span></span>
9. <span data-ttu-id="0439b-190">Klikněte **odpovědi** zobrazíte skutečná text JSON odpovědi.</span><span class="sxs-lookup"><span data-stu-id="0439b-190">Click the **Response body** tab to view the actual JSON response text.</span></span>

    <span data-ttu-id="0439b-191">![Zobrazení kódu JSON výstup textu v programu Sledování sítě](build-restful-apis-with-aspnet-web-api/_static/image9.png "zobrazení JSON výstup textu v programu Sledování sítě")</span><span class="sxs-lookup"><span data-stu-id="0439b-191">![Viewing the JSON output text in the network monitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "Viewing the JSON output text in the network monitor")</span></span>

    <span data-ttu-id="0439b-192">*Zobrazení textu výstup JSON v programu Sledování sítě*</span><span class="sxs-lookup"><span data-stu-id="0439b-192">*Viewing the JSON output text in the network monitor*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a><span data-ttu-id="0439b-193">Úloha 3 – vytvoření kontaktní modely a posílení kontaktní řadiče</span><span class="sxs-lookup"><span data-stu-id="0439b-193">Task 3 - Creating the Contact Models and Augment the Contact Controller</span></span>

<span data-ttu-id="0439b-194">V této úloze vytvoří řadič třídy, ve kterých se bude nacházet metody rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0439b-194">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="0439b-195">Klikněte pravým tlačítkem myši **modely** složky a vyberte **přidat | Třída...**  v místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="0439b-195">Right-click the **Models** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="0439b-196">![Přidání nového modelu k webové aplikaci](build-restful-apis-with-aspnet-web-api/_static/image10.png "přidávání nový model do webové aplikace")</span><span class="sxs-lookup"><span data-stu-id="0439b-196">![Adding a new model to the web application](build-restful-apis-with-aspnet-web-api/_static/image10.png "Adding a new model to the web application")</span></span>

    <span data-ttu-id="0439b-197">*Přidání nového modelu do webové aplikace*</span><span class="sxs-lookup"><span data-stu-id="0439b-197">*Adding a new model to the web application*</span></span>
2. <span data-ttu-id="0439b-198">V **přidat novou položku** dialogové okno, název nový soubor **Contact.cs** a klikněte na tlačítko **přidat.**</span><span class="sxs-lookup"><span data-stu-id="0439b-198">In the **Add New Item** dialog, name the new file **Contact.cs** and click **Add.**</span></span>

    <span data-ttu-id="0439b-199">![Vytvoření nového souboru třídy obraťte se na](build-restful-apis-with-aspnet-web-api/_static/image11.png "vytváření nového souboru kontakt – třída")</span><span class="sxs-lookup"><span data-stu-id="0439b-199">![Creating the new Contact class file](build-restful-apis-with-aspnet-web-api/_static/image11.png "Creating the new Contact class file")</span></span>

    <span data-ttu-id="0439b-200">*Vytváření nového souboru kontakt – třída*</span><span class="sxs-lookup"><span data-stu-id="0439b-200">*Creating the new Contact class file*</span></span>
3. <span data-ttu-id="0439b-201">Přidejte následující zvýrazněný kód, který **kontaktujte** třídy.</span><span class="sxs-lookup"><span data-stu-id="0439b-201">Add the following highlighted code to the **Contact** class.</span></span>

    <span data-ttu-id="0439b-202">(Code fragment kódu - *webové rozhraní API prostředí - Ex01 - kontaktní třída*)</span><span class="sxs-lookup"><span data-stu-id="0439b-202">(Code Snippet - *Web API Lab - Ex01 - Contact Class*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. <span data-ttu-id="0439b-203">V **ContactController** třídy, vyberte slovo **řetězec** v definici metoda **získat** metoda a zadejte slovo *kontaktujte*.</span><span class="sxs-lookup"><span data-stu-id="0439b-203">In the **ContactController** class, select the word **string** in method definition of the **Get** method, and type the word *Contact*.</span></span> <span data-ttu-id="0439b-204">Po slovo je zadán v, slouží jako ukazatel zobrazí na začátku slova **kontaktujte**.</span><span class="sxs-lookup"><span data-stu-id="0439b-204">Once the word is typed in, an indicator will appear at the beginning of the word **Contact**.</span></span> <span data-ttu-id="0439b-205">Buď podržte stisknutou **Ctrl** klíče a stiskněte klávesu tečka (.) nebo klikněte na ikonu pomocí myši otevřete dialog pomoc v editoru kódu vyplnit automaticky **pomocí** direktivy pro modely obor názvů.</span><span class="sxs-lookup"><span data-stu-id="0439b-205">Either hold down the **Ctrl** key and press the period (.) key or click the icon using your mouse to open up the assistance dialog in the code editor, to automatically fill in the **using** directive for the Models namespace.</span></span>

    ![Pomocí Intellisense pomoc pro deklarace oborů názvů](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    <span data-ttu-id="0439b-207">*Pomocí Intellisense pomoc pro deklarace oborů názvů*</span><span class="sxs-lookup"><span data-stu-id="0439b-207">*Using Intellisense assistance for namespace declarations*</span></span>
5. <span data-ttu-id="0439b-208">Upravte kód pro **získat** metoda tak to vrátí pole obraťte se na modelu instancí.</span><span class="sxs-lookup"><span data-stu-id="0439b-208">Modify the code for the **Get** method so that it returns an array of Contact model instances.</span></span>

    <span data-ttu-id="0439b-209">(Code fragment kódu - *webové rozhraní API prostředí - Ex01 - vrácení seznamu kontaktů*)</span><span class="sxs-lookup"><span data-stu-id="0439b-209">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. <span data-ttu-id="0439b-210">Stiskněte klávesu **F5** k ladění webové aplikace v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="0439b-210">Press **F5** to debug the web application in the browser.</span></span> <span data-ttu-id="0439b-211">Chcete-li zobrazit změny provedené v výstup odezvy rozhraní API, proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="0439b-211">To view the changes made to the response output of the API, perform the following steps.</span></span>

   1. <span data-ttu-id="0439b-212">Jakmile se prohlížeči se otevře, stiskněte klávesu **F12** Pokud nástrojů pro vývojáře ještě nejsou otevřené.</span><span class="sxs-lookup"><span data-stu-id="0439b-212">Once the browser opens, press **F12** if the developer tools are not open yet.</span></span>
   2. <span data-ttu-id="0439b-213">Klikněte **sítě** kartě.</span><span class="sxs-lookup"><span data-stu-id="0439b-213">Click the **Network** tab.</span></span>
   3. <span data-ttu-id="0439b-214">Stiskněte **spustit zachytávání** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0439b-214">Press the **Start Capturing** button.</span></span>
   4. <span data-ttu-id="0439b-215">Přidat adresu URL příponu **/api/kontakt** adresy URL v adresním řádku a stiskněte klávesu **Enter** klíč.</span><span class="sxs-lookup"><span data-stu-id="0439b-215">Add the URL suffix **/api/contact** to the URL in the address bar and press the **Enter** key.</span></span>
   5. <span data-ttu-id="0439b-216">Stiskněte **přejít na podrobné zobrazení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0439b-216">Press the **Go to detailed view** button.</span></span>
   6. <span data-ttu-id="0439b-217">Vyberte **odpovědi** kartě. Měli byste vidět JSON řetězec představující serializovaný formu pole Kontakt instancí.</span><span class="sxs-lookup"><span data-stu-id="0439b-217">Select the **Response body** tab. You should see a JSON string representing the serialized form of an array of Contact instances.</span></span>

      <span data-ttu-id="0439b-218">![JSON serializovat výstup komplexní metoda volání webového rozhraní API](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serializovat výstup komplexní volání webového rozhraní API – metoda")</span><span class="sxs-lookup"><span data-stu-id="0439b-218">![JSON serialized output of a complex Web API method call](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serialized output of a complex Web API method call")</span></span>

      <span data-ttu-id="0439b-219">*Výstup serializován do formátu JSON komplexní volání webového rozhraní API – metoda*</span><span class="sxs-lookup"><span data-stu-id="0439b-219">*JSON serialized output of a complex Web API method call*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a><span data-ttu-id="0439b-220">Úloha 4 - extrakce funkce do vrstvy služby</span><span class="sxs-lookup"><span data-stu-id="0439b-220">Task 4 - Extracting Functionality into a Service Layer</span></span>

<span data-ttu-id="0439b-221">Tato úloha bude ukazují, jak funkce do vrstvy služby pro vývojáře k oddělení jejich funkcí služby z vrstvy řadiče, a tím umožní – opětovné použití služeb, které ve skutečnosti práci snadno rozbalte.</span><span class="sxs-lookup"><span data-stu-id="0439b-221">This task will demonstrate how to extract functionality into a Service layer to make it easy for developers to separate their service functionality from the controller layer, thereby allowing reusability of the services that actually do the work.</span></span>

1. <span data-ttu-id="0439b-222">Vytvořte novou složku v kořenovém řešení a pojmenujte ji **služby**.</span><span class="sxs-lookup"><span data-stu-id="0439b-222">Create a new folder in the solution root and name it **Services**.</span></span> <span data-ttu-id="0439b-223">Chcete-li to provést, klikněte pravým tlačítkem **ContactManager** projekt, vyberte **přidat** | **novou složku**, pojmenujte ji *služby*.</span><span class="sxs-lookup"><span data-stu-id="0439b-223">To do this, right-click **ContactManager** project, select **Add** | **New Folder**, name it *Services*.</span></span>

    <span data-ttu-id="0439b-224">![Vytváření služeb složky](build-restful-apis-with-aspnet-web-api/_static/image14.png "složky vytváření služeb")</span><span class="sxs-lookup"><span data-stu-id="0439b-224">![Creating Services folder](build-restful-apis-with-aspnet-web-api/_static/image14.png "Creating Services folder")</span></span>

    <span data-ttu-id="0439b-225">*Vytváření složky služby*</span><span class="sxs-lookup"><span data-stu-id="0439b-225">*Creating Services folder*</span></span>
2. <span data-ttu-id="0439b-226">Klikněte pravým tlačítkem myši **služby** složky a vyberte **přidat | Třída...**  v místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="0439b-226">Right-click the **Services** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="0439b-227">![Přidání nové třídy do složky služby](build-restful-apis-with-aspnet-web-api/_static/image15.png "přidání nové třídy do složky služby")</span><span class="sxs-lookup"><span data-stu-id="0439b-227">![Adding a new class to the Services folder](build-restful-apis-with-aspnet-web-api/_static/image15.png "Adding a new class to the Services folder")</span></span>

    <span data-ttu-id="0439b-228">*Přidání nové třídy do složky služby*</span><span class="sxs-lookup"><span data-stu-id="0439b-228">*Adding a new class to the Services folder*</span></span>
3. <span data-ttu-id="0439b-229">Když **přidat novou položku** otevře se dialogové okno, pojmenujte novou třídu **ContactRepository** a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="0439b-229">When the **Add New Item** dialog appears, name the new class **ContactRepository** and click **Add**.</span></span>

    <span data-ttu-id="0439b-230">![Vytvoření souboru třída obsahuje kód pro vrstvu služby úložiště kontaktujte](build-restful-apis-with-aspnet-web-api/_static/image16.png "vytváření souboru třída obsahuje kód pro vrstvu služby úložiště kontakt")</span><span class="sxs-lookup"><span data-stu-id="0439b-230">![Creating a class file to contain the code for the Contact Repository service layer](build-restful-apis-with-aspnet-web-api/_static/image16.png "Creating a class file to contain the code for the Contact Repository service layer")</span></span>

    <span data-ttu-id="0439b-231">*Vytvoření souboru třída obsahuje kód pro vrstvu služby úložiště kontakt*</span><span class="sxs-lookup"><span data-stu-id="0439b-231">*Creating a class file to contain the code for the Contact Repository service layer*</span></span>
4. <span data-ttu-id="0439b-232">Přidat pomocí direktivy k **ContactRepository.cs** souboru obor názvů modelů.</span><span class="sxs-lookup"><span data-stu-id="0439b-232">Add a using directive to the **ContactRepository.cs** file to include the models namespace.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. <span data-ttu-id="0439b-233">Přidejte následující zvýrazněný kód, který **ContactRepository.cs** souboru k implementaci GetAllContacts metoda.</span><span class="sxs-lookup"><span data-stu-id="0439b-233">Add the following highlighted code to the **ContactRepository.cs** file to implement GetAllContacts method.</span></span>

    <span data-ttu-id="0439b-234">(Code fragment kódu - *webové rozhraní API prostředí - Ex01 - kontaktní úložiště*)</span><span class="sxs-lookup"><span data-stu-id="0439b-234">(Code Snippet - *Web API Lab - Ex01 - Contact Repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. <span data-ttu-id="0439b-235">Otevřete **ContactController.cs** soubor, pokud ještě není otevřené.</span><span class="sxs-lookup"><span data-stu-id="0439b-235">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="0439b-236">Přidejte následující příkaz using do části deklarace oboru názvů souboru.</span><span class="sxs-lookup"><span data-stu-id="0439b-236">Add the following using statement to the namespace declaration section of the file.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. <span data-ttu-id="0439b-237">Přidejte následující zvýrazněný kód, který **ContactController.cs** třída přidat soukromé pole představující instanci úložiště, tak, aby zbývající členy provádět třídy používat implementace služby.</span><span class="sxs-lookup"><span data-stu-id="0439b-237">Add the following highlighted code to the **ContactController.cs** class to add a private field to represent the instance of the repository, so that the rest of the class members can make use of the service implementation.</span></span>

    <span data-ttu-id="0439b-238">(Code fragment kódu - *webové rozhraní API prostředí - Ex01 - kontaktní řadiče*)</span><span class="sxs-lookup"><span data-stu-id="0439b-238">(Code Snippet - *Web API Lab - Ex01 - Contact Controller*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. <span data-ttu-id="0439b-239">Změnit **získat** metoda, díky které se využívají službu kontaktní úložiště.</span><span class="sxs-lookup"><span data-stu-id="0439b-239">Change the **Get** method so that it makes use of the contact repository service.</span></span>

    <span data-ttu-id="0439b-240">(Code fragment kódu - *webové rozhraní API prostředí - Ex01 - vrácení seznamu kontaktů prostřednictvím úložiště*)</span><span class="sxs-lookup"><span data-stu-id="0439b-240">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts via the repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. <span data-ttu-id="0439b-241">Umístí zarážku **ContactController**na **získat** definici metody.</span><span class="sxs-lookup"><span data-stu-id="0439b-241">Put a breakpoint on the **ContactController**'s **Get** method definition.</span></span>

   <span data-ttu-id="0439b-242">![Přidání zarážky kontaktní řadiče](build-restful-apis-with-aspnet-web-api/_static/image17.png "přidání zarážky kontaktní řadiče")</span><span class="sxs-lookup"><span data-stu-id="0439b-242">![Adding breakpoints to the contact controller](build-restful-apis-with-aspnet-web-api/_static/image17.png "Adding breakpoints to the contact controller")</span></span>

   <span data-ttu-id="0439b-243">*Přidání zarážky kontaktní řadiče*</span><span class="sxs-lookup"><span data-stu-id="0439b-243">*Adding breakpoints to the contact controller*</span></span>
11. <span data-ttu-id="0439b-244">Stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="0439b-244">Press **F5** to run the application.</span></span>
12. <span data-ttu-id="0439b-245">Když se otevře v prohlížeči, stiskněte klávesu **F12** otevření nástrojů pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="0439b-245">When the browser opens, press **F12** to open the developer tools.</span></span>
13. <span data-ttu-id="0439b-246">Klikněte **sítě** kartě.</span><span class="sxs-lookup"><span data-stu-id="0439b-246">Click the **Network** tab.</span></span>
14. <span data-ttu-id="0439b-247">Klikněte **spustit zachytávání** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0439b-247">Click the **Start Capturing** button.</span></span>
15. <span data-ttu-id="0439b-248">Připojte na adresu URL v adresním řádku s příponou **/api/kontakt** a stiskněte klávesu **Enter** načtení kontroleru rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0439b-248">Append the URL in the address bar with the suffix **/api/contact** and press **Enter** to load the API controller.</span></span>
16. <span data-ttu-id="0439b-249">Visual Studio 2012 by mělo dojít jednou **získat** metoda zahájí spuštění.</span><span class="sxs-lookup"><span data-stu-id="0439b-249">Visual Studio 2012 should break once **Get** method begins execution.</span></span>

   <span data-ttu-id="0439b-250">![Pozastavení v rámci metody Get](build-restful-apis-with-aspnet-web-api/_static/image18.png "dopadem na dřívější kód v rámci metody Get")</span><span class="sxs-lookup"><span data-stu-id="0439b-250">![Breaking within the Get method](build-restful-apis-with-aspnet-web-api/_static/image18.png "Breaking within the Get method")</span></span>

   <span data-ttu-id="0439b-251">*Ukončování řádků v rámci metody Get*</span><span class="sxs-lookup"><span data-stu-id="0439b-251">*Breaking within the Get method*</span></span>
17. <span data-ttu-id="0439b-252">Stiskněte klávesu **F5** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="0439b-252">Press **F5** to continue.</span></span>
18. <span data-ttu-id="0439b-253">Vraťte se zpátky do Internet Exploreru Pokud ještě není aktivní.</span><span class="sxs-lookup"><span data-stu-id="0439b-253">Go back to Internet Explorer if it is not already in focus.</span></span> <span data-ttu-id="0439b-254">Poznámka: okno sběr sítě.</span><span class="sxs-lookup"><span data-stu-id="0439b-254">Note the network capture window.</span></span>

    <span data-ttu-id="0439b-255">![Sítě zobrazit v aplikaci Internet Explorer zobrazuje výsledky volání webového rozhraní API](build-restful-apis-with-aspnet-web-api/_static/image19.png "sítě zobrazit v aplikaci Internet Explorer zobrazuje výsledky volání webového rozhraní API")</span><span class="sxs-lookup"><span data-stu-id="0439b-255">![Network view in Internet Explorer showing results of the Web API call](build-restful-apis-with-aspnet-web-api/_static/image19.png "Network view in Internet Explorer showing results of the Web API call")</span></span>

    <span data-ttu-id="0439b-256">*Zobrazení sítě v Internet Exploreru zobrazující výsledků volání webového rozhraní API*</span><span class="sxs-lookup"><span data-stu-id="0439b-256">*Network view in Internet Explorer showing results of the Web API call*</span></span>
19. <span data-ttu-id="0439b-257">Klikněte **přejít na podrobné zobrazení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0439b-257">Click the **Go to detailed view** button.</span></span>
20. <span data-ttu-id="0439b-258">Klikněte **odpovědi** kartě. Všimněte si výstup JSON volání rozhraní API a jak ho reprezentuje dva kontakty načíst vrstvě služby.</span><span class="sxs-lookup"><span data-stu-id="0439b-258">Click the **Response body** tab. Note the JSON output of the API call, and how it represents the two contacts retrieved by the service layer.</span></span>

    <span data-ttu-id="0439b-259">![Zobrazení výstup JSON ze webového rozhraní API v okně nástroje pro vývojáře](build-restful-apis-with-aspnet-web-api/_static/image20.png "zobrazení výstup JSON ze webového rozhraní API v okně nástroje pro vývojáře")</span><span class="sxs-lookup"><span data-stu-id="0439b-259">![Viewing the JSON output from the Web API in the developer tools window](build-restful-apis-with-aspnet-web-api/_static/image20.png "Viewing the JSON output from the Web API in the developer tools window")</span></span>

    <span data-ttu-id="0439b-260">*Zobrazení výstup JSON ze webového rozhraní API v okně nástroje pro vývojáře*</span><span class="sxs-lookup"><span data-stu-id="0439b-260">*Viewing the JSON output from the Web API in the developer tools window*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a><span data-ttu-id="0439b-261">Cvičení 2: Vytvoření webové rozhraní API pro čtení i zápis</span><span class="sxs-lookup"><span data-stu-id="0439b-261">Exercise 2: Create a Read/Write Web API</span></span>

<span data-ttu-id="0439b-262">V tomto cvičení budete implementovat POST a PUT metody pro kontaktujte správce k povolení s funkce pro úpravu dat.</span><span class="sxs-lookup"><span data-stu-id="0439b-262">In this exercise, you will implement POST and PUT methods for the contact manager to enable it with data-editing features.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a><span data-ttu-id="0439b-263">Úloha 1 - otevření projektu webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="0439b-263">Task 1 - Opening the Web API Project</span></span>

<span data-ttu-id="0439b-264">V této úloze se připravíte na vylepšení projekt webového rozhraní API, které jsou vytvořené v cvičení 1 tak, aby přijímal vstup uživatele.</span><span class="sxs-lookup"><span data-stu-id="0439b-264">In this task, you will prepare to enhance the Web API project created in Exercise 1 so that it can accept user input.</span></span>

1. <span data-ttu-id="0439b-265">Spustit **Visual Studio 2012 Express pro Web**, přejděte k **spustit** a typ **VS Express pro Web** stiskněte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="0439b-265">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="0439b-266">Otevřete **začít** řešení nacházející se v **zdroj/Ex02-ReadWriteWebAPI/počáteční/** složky.</span><span class="sxs-lookup"><span data-stu-id="0439b-266">Open the **Begin** solution located at **Source/Ex02-ReadWriteWebAPI/Begin/** folder.</span></span> <span data-ttu-id="0439b-267">Jinak, může pokračovat, pomocí **End** řešení získat provedením předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="0439b-267">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="0439b-268">Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="0439b-268">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="0439b-269">Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="0439b-269">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="0439b-270">V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="0439b-270">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="0439b-271">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="0439b-271">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="0439b-272">Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="0439b-272">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="0439b-273">Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="0439b-273">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="0439b-274">Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="0439b-274">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="0439b-275">Otevřete **Services/ContactRepository.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="0439b-275">Open the **Services/ContactRepository.cs** file.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a><span data-ttu-id="0439b-276">Úloha 2 – přidání trvalosti dat funkce pro implementaci kontaktní úložiště</span><span class="sxs-lookup"><span data-stu-id="0439b-276">Task 2 - Adding Data-Persistence Features to the Contact Repository Implementation</span></span>

<span data-ttu-id="0439b-277">V této úloze bude posílení ContactRepository třídu projekt webového rozhraní API, které jsou vytvořené v cvičení 1, aby mohli zachovat a přijímají vstup uživatele a nové instance kontakt.</span><span class="sxs-lookup"><span data-stu-id="0439b-277">In this task, you will augment the ContactRepository class of the Web API project created in Exercise 1 so that it can persist and accept user input and new Contact instances.</span></span>

1. <span data-ttu-id="0439b-278">Přidejte následující konstanty, která **ContactRepository** třída představující název webového serveru mezipaměti klíče název položky později v tomto cvičení.</span><span class="sxs-lookup"><span data-stu-id="0439b-278">Add the following constant to the **ContactRepository** class to represent the name of the web server cache item key name later in this exercise.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. <span data-ttu-id="0439b-279">Přidejte konstruktor k **ContactRepository** obsahující následující kód.</span><span class="sxs-lookup"><span data-stu-id="0439b-279">Add a constructor to the **ContactRepository** containing the following code.</span></span>

    <span data-ttu-id="0439b-280">(Code fragment kódu - *webové rozhraní API prostředí - Ex02 - kontaktní úložiště konstruktor*)</span><span class="sxs-lookup"><span data-stu-id="0439b-280">(Code Snippet - *Web API Lab - Ex02 - Contact Repository Constructor*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. <span data-ttu-id="0439b-281">Upravte kód pro **GetAllContacts** metoda, jak je ukázáno níže.</span><span class="sxs-lookup"><span data-stu-id="0439b-281">Modify the code for the **GetAllContacts** method as demonstrated below.</span></span>

    <span data-ttu-id="0439b-282">(Code fragment kódu - *webové rozhraní API prostředí - Ex02 - Get All Contacts*)</span><span class="sxs-lookup"><span data-stu-id="0439b-282">(Code Snippet - *Web API Lab - Ex02 - Get All Contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="0439b-283">Tento příklad je pro demonstrační účely a použije mezipaměti webový server jako úložné médium, tak, aby hodnoty budou být k dispozici pro více klientů současně, místo použití mechanismus relace úložiště nebo úložiště životnost požadavku.</span><span class="sxs-lookup"><span data-stu-id="0439b-283">This example is for demonstration purposes and will use the web server's cache as a storage medium, so that the values will be available to multiple clients simultaneously, rather than use a Session storage mechanism or a Request storage lifetime.</span></span> <span data-ttu-id="0439b-284">Jeden nebo použít rozhraní Entity Framework, XML úložiště nebo jiné různých místo mezipaměti webového serveru.</span><span class="sxs-lookup"><span data-stu-id="0439b-284">One could use Entity Framework, XML storage, or any other variety in place of the web server cache.</span></span>
4. <span data-ttu-id="0439b-285">Implementovat novou metodu s názvem **SaveContact** k **ContactRepository** třída udělat práci při ukládání a kontaktovat.</span><span class="sxs-lookup"><span data-stu-id="0439b-285">Implement a new method named **SaveContact** to the **ContactRepository** class to do the work of saving a contact.</span></span> <span data-ttu-id="0439b-286">**SaveContact** metoda provést jeden **kontaktujte** parametr a vrátit logickou hodnotu hodnotu, která udává úspěch nebo selhání.</span><span class="sxs-lookup"><span data-stu-id="0439b-286">The **SaveContact** method should take a single **Contact** parameter and return a Boolean value indicating success or failure.</span></span>

    <span data-ttu-id="0439b-287">(Code fragment kódu - *webové rozhraní API prostředí - Ex02 – implementace metodu SaveContact*)</span><span class="sxs-lookup"><span data-stu-id="0439b-287">(Code Snippet - *Web API Lab - Ex02 - Implementing the SaveContact Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a><span data-ttu-id="0439b-288">Cvičení 3: Využívají webové rozhraní API z klienta HTML</span><span class="sxs-lookup"><span data-stu-id="0439b-288">Exercise 3: Consume the Web API from an HTML Client</span></span>

<span data-ttu-id="0439b-289">V tomto cvičení vytvoříte klientovi HTML pro volání webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0439b-289">In this exercise, you will create an HTML client to call the Web API.</span></span> <span data-ttu-id="0439b-290">Tento klient usnadní výměna dat s použitím jazyka JavaScript webového rozhraní API a výsledky se zobrazí ve webovém prohlížeči pomocí kódu HTML.</span><span class="sxs-lookup"><span data-stu-id="0439b-290">This client will facilitate data exchange with the Web API using JavaScript and will display the results in a web browser using HTML markup.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a><span data-ttu-id="0439b-291">Úloha 1 - úprava zobrazení indexu zajistit grafickým uživatelským rozhraním pro zobrazení kontaktů</span><span class="sxs-lookup"><span data-stu-id="0439b-291">Task 1 - Modifying the Index View to Provide a GUI for Displaying Contacts</span></span>

<span data-ttu-id="0439b-292">V této úloze budete upravovat výchozí zobrazení indexu webové aplikace pro podporu požadavek na zobrazení seznamu existující kontaktů v prohlížeči HTML.</span><span class="sxs-lookup"><span data-stu-id="0439b-292">In this task, you will modify the default Index view of the web application to support the requirement of displaying the list of existing contacts in an HTML browser.</span></span>

1. <span data-ttu-id="0439b-293">Otevřete **Visual Studio 2012 Express pro Web** Pokud ještě není otevřené.</span><span class="sxs-lookup"><span data-stu-id="0439b-293">Open **Visual Studio 2012 Express for Web** if it is not already open.</span></span>
2. <span data-ttu-id="0439b-294">Otevřete **začít** řešení nacházející se v **zdroj/Ex03-ConsumingWebAPI/počáteční/** složky.</span><span class="sxs-lookup"><span data-stu-id="0439b-294">Open the **Begin** solution located at **Source/Ex03-ConsumingWebAPI/Begin/** folder.</span></span> <span data-ttu-id="0439b-295">Jinak, může pokračovat, pomocí **End** řešení získat provedením předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="0439b-295">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="0439b-296">Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="0439b-296">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="0439b-297">Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="0439b-297">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="0439b-298">V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="0439b-298">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="0439b-299">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="0439b-299">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="0439b-300">Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="0439b-300">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="0439b-301">Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="0439b-301">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="0439b-302">Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="0439b-302">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="0439b-303">Otevřete **Index.cshtml** soubor umístěný ve **zobrazení Domů** složky.</span><span class="sxs-lookup"><span data-stu-id="0439b-303">Open the **Index.cshtml** file located at **Views/Home** folder.</span></span>
4. <span data-ttu-id="0439b-304">Nahraďte kód HTML v rámci div element id **textu** tak, aby vypadal jako následující kód.</span><span class="sxs-lookup"><span data-stu-id="0439b-304">Replace the HTML code within the div element with id **body** so that it looks like the following code.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. <span data-ttu-id="0439b-305">Přidejte následující kód v JavaScriptu v dolní části souboru k provedení této žádosti HTTP do webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0439b-305">Add the following Javascript code at the bottom of the file to perform the HTTP request to the Web API.</span></span>

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. <span data-ttu-id="0439b-306">Otevřete **ContactController.cs** soubor, pokud ještě není otevřené.</span><span class="sxs-lookup"><span data-stu-id="0439b-306">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="0439b-307">Umístěte zarážku na **získat** metodu **ContactController** třídy.</span><span class="sxs-lookup"><span data-stu-id="0439b-307">Place a breakpoint on the **Get** method of the **ContactController** class.</span></span>

    <span data-ttu-id="0439b-308">![Uvedení zarážku na metodu Get řadiče rozhraní API](build-restful-apis-with-aspnet-web-api/_static/image21.png "umístění zarážku u metody Get řadiče rozhraní API")</span><span class="sxs-lookup"><span data-stu-id="0439b-308">![Placing a breakpoint on the Get method of the API controller](build-restful-apis-with-aspnet-web-api/_static/image21.png "Placing a breakpoint on the Get method of the API controller")</span></span>

    <span data-ttu-id="0439b-309">*Uvedení zarážku na metodu Get řadiče rozhraní API*</span><span class="sxs-lookup"><span data-stu-id="0439b-309">*Placing a breakpoint on the Get method of the API controller*</span></span>
8. <span data-ttu-id="0439b-310">Stiskněte klávesu **F5** spustit projekt.</span><span class="sxs-lookup"><span data-stu-id="0439b-310">Press **F5** to run the project.</span></span> <span data-ttu-id="0439b-311">Prohlížeč načte dokumentu HTML.</span><span class="sxs-lookup"><span data-stu-id="0439b-311">The browser will load the HTML document.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0439b-312">Ujistěte se, že procházíte adresy URL kořenového adresáře aplikace.</span><span class="sxs-lookup"><span data-stu-id="0439b-312">Ensure that you are browsing to the root URL of your application.</span></span>
9. <span data-ttu-id="0439b-313">Po načtení stránky a provede jazyka JavaScript, bude dosaženo zarážce a v kontroleru bude pozastavit provádění kódu.</span><span class="sxs-lookup"><span data-stu-id="0439b-313">Once the page loads and the JavaScript executes, the breakpoint will be hit and the code execution will pause in the controller.</span></span>

    <span data-ttu-id="0439b-314">![Ladění do volání webového rozhraní API pomocí VS Express pro Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "ladění do volání webového rozhraní API pomocí VS Express pro Web")</span><span class="sxs-lookup"><span data-stu-id="0439b-314">![Debugging into the Web API calls using VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debugging into the Web API calls using VS Express for Web")</span></span>

    <span data-ttu-id="0439b-315">*Ladění do volání webového rozhraní API pomocí sady Visual Studio 2012 Express pro Web*</span><span class="sxs-lookup"><span data-stu-id="0439b-315">*Debugging into the Web API call using Visual Studio 2012 Express for Web*</span></span>
10. <span data-ttu-id="0439b-316">Odebrat zarážek a stiskněte klávesu **F5** nebo ladění nástrojů **pokračovat** tlačítko Pokračovat v načítání zobrazení v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="0439b-316">Remove the breakpoint and press **F5** or the debugging toolbar's **Continue** button to continue loading the view in the browser.</span></span> <span data-ttu-id="0439b-317">Po dokončení volání webového rozhraní API byste měli vidět vrácená z webového rozhraní API kontaktů volání zobrazit jako položky seznamu v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="0439b-317">Once the Web API call completes you should see the contacts returned from the Web API call displayed as list items in the browser.</span></span>

    <span data-ttu-id="0439b-318">![Výsledky volání rozhraní API v prohlížeči zobrazí jako položky seznamu](build-restful-apis-with-aspnet-web-api/_static/image23.png "výsledků volání rozhraní API v prohlížeči zobrazí jako položky seznamu")</span><span class="sxs-lookup"><span data-stu-id="0439b-318">![Results of the API call displayed in the browser as list items](build-restful-apis-with-aspnet-web-api/_static/image23.png "Results of the API call displayed in the browser as list items")</span></span>

    <span data-ttu-id="0439b-319">*Výsledky volání rozhraní API v prohlížeči zobrazí jako položky seznamu*</span><span class="sxs-lookup"><span data-stu-id="0439b-319">*Results of the API call displayed in the browser as list items*</span></span>
11. <span data-ttu-id="0439b-320">Zastavte ladění.</span><span class="sxs-lookup"><span data-stu-id="0439b-320">Stop debugging.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a><span data-ttu-id="0439b-321">Úloha 2 - Úprava zobrazení indexu zajistit grafickým uživatelským rozhraním pro vytvoření kontaktů</span><span class="sxs-lookup"><span data-stu-id="0439b-321">Task 2 - Modifying the Index View to Provide a GUI for Creating Contacts</span></span>

<span data-ttu-id="0439b-322">V této úloze bude pokračovat ke změně zobrazení indexu aplikace MVC.</span><span class="sxs-lookup"><span data-stu-id="0439b-322">In this task, you will continue to modify the Index view of the MVC application.</span></span> <span data-ttu-id="0439b-323">Formulář bude přidán na stránku HTML, který bude zachycení uživatelského vstupu a odeslat do webového rozhraní API pro vytvoření nového kontaktu a vytvoří se nové metody kontroleru webového rozhraní API shromažďovat data z grafického uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="0439b-323">A form will be added to the HTML page that will capture user input and send it to the Web API to create a new Contact, and a new Web API controller method will be created to collect date from the GUI.</span></span>

1. <span data-ttu-id="0439b-324">Otevřete **ContactController.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="0439b-324">Open the **ContactController.cs** file.</span></span>
2. <span data-ttu-id="0439b-325">Přidat novou metodu do třídy kontroleru s názvem **Post** jak je znázorněno v následujícím kódu.</span><span class="sxs-lookup"><span data-stu-id="0439b-325">Add a new method to the controller class named **Post** as shown in the following code.</span></span>

    <span data-ttu-id="0439b-326">(Code fragment kódu - *webové rozhraní API prostředí - Ex03 - metodu Post*)</span><span class="sxs-lookup"><span data-stu-id="0439b-326">(Code Snippet - *Web API Lab - Ex03 - Post Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. <span data-ttu-id="0439b-327">Otevřete **Index.cshtml** souborů v sadě Visual Studio, pokud ještě není otevřené.</span><span class="sxs-lookup"><span data-stu-id="0439b-327">Open the **Index.cshtml** file in Visual Studio if it is not already open.</span></span>
4. <span data-ttu-id="0439b-328">Kód HTML níže přidejte do souboru právě po neuspořádaného seznamu, které jste přidali v předchozí úloze.</span><span class="sxs-lookup"><span data-stu-id="0439b-328">Add the HTML code below to the file just after the unordered list you added in the previous task.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. <span data-ttu-id="0439b-329">V rámci elementu skript v dolní části dokumentu přidejte následující zvýrazněný kód pro zpracování událostí kliknutí na tlačítko, které bude odeslána data do webového rozhraní API pomocí volání HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="0439b-329">Within the script element at the bottom of the document, add the following highlighted code to handle button-click events, which will post the data to the Web API using an HTTP POST call.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. <span data-ttu-id="0439b-330">V **ContactController.cs**, umístěte zarážku na **Post** metoda.</span><span class="sxs-lookup"><span data-stu-id="0439b-330">In **ContactController.cs**, place a breakpoint on the **Post** method.</span></span>
7. <span data-ttu-id="0439b-331">Stiskněte klávesu **F5** ke spuštění aplikace v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="0439b-331">Press **F5** to run the application in the browser.</span></span>
8. <span data-ttu-id="0439b-332">Po načtení stránky v prohlížeči, zadejte název nového kontaktu a Id a klikněte na **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0439b-332">Once the page is loaded in the browser, type in a new contact name and Id and click the **Save** button.</span></span>

    <span data-ttu-id="0439b-333">![Klient HTML dokumentu načítání do prohlížeče](build-restful-apis-with-aspnet-web-api/_static/image24.png "klienta HTML dokumentu načítání do prohlížeče")</span><span class="sxs-lookup"><span data-stu-id="0439b-333">![The client HTML document loaded in the browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "The client HTML document loaded in the browser")</span></span>

    <span data-ttu-id="0439b-334">*Dokumentu HTML klienta načítání do prohlížeče*</span><span class="sxs-lookup"><span data-stu-id="0439b-334">*The client HTML document loaded in the browser*</span></span>
9. <span data-ttu-id="0439b-335">Když okna ladicího programu zalomení řádků **Post** metoda, proveďte a podívejte se na vlastnosti **obraťte se na** parametr.</span><span class="sxs-lookup"><span data-stu-id="0439b-335">When the debugger window breaks in the **Post** method, take a look at the properties of the **contact** parameter.</span></span> <span data-ttu-id="0439b-336">Hodnoty musí odpovídat data, která jste zadali ve formuláři.</span><span class="sxs-lookup"><span data-stu-id="0439b-336">The values should match the data you entered in the form.</span></span>

    <span data-ttu-id="0439b-337">![Obraťte se na objekt odesílána webového rozhraní API z klienta](build-restful-apis-with-aspnet-web-api/_static/image25.png "objekt kontakt odesílána webového rozhraní API z klienta")</span><span class="sxs-lookup"><span data-stu-id="0439b-337">![The Contact object being sent to the Web API from the client](build-restful-apis-with-aspnet-web-api/_static/image25.png "The Contact object being sent to the Web API from the client")</span></span>

    <span data-ttu-id="0439b-338">*Obraťte se na objekt odesílána webového rozhraní API z klienta*</span><span class="sxs-lookup"><span data-stu-id="0439b-338">*The Contact object being sent to the Web API from the client*</span></span>
10. <span data-ttu-id="0439b-339">Krok prostřednictvím metody v ladicím programu až **odpovědi** proměnná byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="0439b-339">Step through the method in the debugger until the **response** variable has been created.</span></span> <span data-ttu-id="0439b-340">Při kontrole v **místní hodnoty –** okno v ladicím programu, uvidíte, že byly nastaveny všechny vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="0439b-340">Upon inspection in the **Locals** window in the debugger, you'll see that all the properties have been set.</span></span>

   <span data-ttu-id="0439b-341">![Odpověď po vytvoření v ladicím programu](build-restful-apis-with-aspnet-web-api/_static/image26.png "odpověď po vytvoření v ladicím programu")</span><span class="sxs-lookup"><span data-stu-id="0439b-341">![The response following creation in the debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "The response following creation in the debugger")</span></span>

   <span data-ttu-id="0439b-342">*Odpověď po vytvoření v ladicím programu*</span><span class="sxs-lookup"><span data-stu-id="0439b-342">*The response following creation in the debugger*</span></span>
11. <span data-ttu-id="0439b-343">Pokud vyberete **F5** nebo klikněte na tlačítko **pokračovat** v ladicím programu bude žádost dokončena.</span><span class="sxs-lookup"><span data-stu-id="0439b-343">If you press **F5** or click **Continue** in the debugger the request will complete.</span></span> <span data-ttu-id="0439b-344">Když přepnete zpět do prohlížeče, nového kontaktu se přidal do seznamu kontaktů uložené **ContactRepository** implementace.</span><span class="sxs-lookup"><span data-stu-id="0439b-344">Once you switch back to the browser, the new contact has been added to the list of contacts stored by the **ContactRepository** implementation.</span></span>

   <span data-ttu-id="0439b-345">![V prohlížeči odráží úspěšné vytvoření nové instance kontaktní](build-restful-apis-with-aspnet-web-api/_static/image27.png "prohlížeče odráží úspěšné vytvoření nové instance kontaktů")</span><span class="sxs-lookup"><span data-stu-id="0439b-345">![The browser reflects successful creation of the new contact instance](build-restful-apis-with-aspnet-web-api/_static/image27.png "The browser reflects successful creation of the new contact instance")</span></span>

   <span data-ttu-id="0439b-346">*Úspěšné vytvoření nové instance kontaktní odráží prohlížeče*</span><span class="sxs-lookup"><span data-stu-id="0439b-346">*The browser reflects successful creation of the new contact instance*</span></span>

> [!NOTE]
> <span data-ttu-id="0439b-347">Kromě toho můžete nasadit tuto aplikaci do Azure následující [příloha C: publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu](#AppendixC).</span><span class="sxs-lookup"><span data-stu-id="0439b-347">Additionally, you can deploy this application to Azure following [Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixC).</span></span>


* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="0439b-348">Souhrn</span><span class="sxs-lookup"><span data-stu-id="0439b-348">Summary</span></span>

<span data-ttu-id="0439b-349">Toto testovací prostředí obsahuje zavedla nové architektury ASP.NET Web API a provádění RESTful webová rozhraní API pomocí rozhraní.</span><span class="sxs-lookup"><span data-stu-id="0439b-349">This lab has introduced you to the new ASP.NET Web API framework and to the implementation of RESTful Web APIs using the framework.</span></span> <span data-ttu-id="0439b-350">Z tohoto místa může vytvořit nové úložiště, která usnadňuje trvalosti dat pomocí libovolný počet mechanismy a propojit služby si místo jednoduché jeden jako příklad v tomto testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="0439b-350">From here, you could create a new repository that facilitates data persistence using any number of mechanisms and wire that service up rather than the simple one provided as an example in this lab.</span></span> <span data-ttu-id="0439b-351">Webové rozhraní API podporuje mnoho dalších funkcí, například povolení komunikaci od klientů jiného typu než HTML, které jsou napsané v libovolném jazyce, který podporuje protokol HTTP a XML nebo JSON.</span><span class="sxs-lookup"><span data-stu-id="0439b-351">Web API supports a number of additional features, such as enabling communication from non-HTML clients written in any language that supports HTTP and JSON or XML.</span></span> <span data-ttu-id="0439b-352">Schopnost hostitele webového rozhraní API mimo typické webové aplikace je také možné, je možnost vytvářet vlastní formáty serializace.</span><span class="sxs-lookup"><span data-stu-id="0439b-352">The ability to host a Web API outside of a typical web application is also possible, as well as is the ability to create your own serialization formats.</span></span>

<span data-ttu-id="0439b-353">Má oblast vyhrazený pro rozhraní ASP.NET Web API na webu technologie ASP.NET [ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="0439b-353">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span></span> <span data-ttu-id="0439b-354">Tento web bude pokračovat v poskytování nejnovější informace, ukázky a zprávy související s webového rozhraní API, tak to zkuste ho často Pokud byste chtěli pustíte hlubší do obrázky vytváření vlastní webová rozhraní API k dispozici pro prakticky libovolný zařízení nebo vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="0439b-354">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="0439b-355">Příloha A: používání fragmentů kódu</span><span class="sxs-lookup"><span data-stu-id="0439b-355">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="0439b-356">S fragmenty kódu máte všechny kód, který je nutné na dosah ruky.</span><span class="sxs-lookup"><span data-stu-id="0439b-356">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="0439b-357">Dokument testovacího prostředí vás bude informovat přesně Pokud můžete, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="0439b-357">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="0439b-358">![Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu](build-restful-apis-with-aspnet-web-api/_static/image28.png "fragmenty kódu pomocí sady Visual Studio pro vložení kódu do projektu")</span><span class="sxs-lookup"><span data-stu-id="0439b-358">![Using Visual Studio code snippets to insert code into your project](build-restful-apis-with-aspnet-web-api/_static/image28.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="0439b-359">*Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu*</span><span class="sxs-lookup"><span data-stu-id="0439b-359">*Using Visual Studio code snippets to insert code into your project*</span></span>

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a><span data-ttu-id="0439b-360">Chcete-li přidat fragment kódu pomocí klávesnice (C# pouze)</span><span class="sxs-lookup"><span data-stu-id="0439b-360">To add a code snippet using the keyboard (C# only)</span></span>

1. <span data-ttu-id="0439b-361">Umístěte kurzor, kam chcete vložit kód.</span><span class="sxs-lookup"><span data-stu-id="0439b-361">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="0439b-362">Začněte psát název fragmentu kódu (bez mezery nebo spojovníky).</span><span class="sxs-lookup"><span data-stu-id="0439b-362">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="0439b-363">Podívejte se na jako IntelliSense zobrazí odpovídající fragmenty názvy.</span><span class="sxs-lookup"><span data-stu-id="0439b-363">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="0439b-364">Vyberte správný fragment kódu (nebo ponechte zadáním dokud je vybraný název celý fragmentu).</span><span class="sxs-lookup"><span data-stu-id="0439b-364">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="0439b-365">Stisknutím klávesy Tab dvakrát můžete vložit fragment v umístění kurzoru.</span><span class="sxs-lookup"><span data-stu-id="0439b-365">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

    <span data-ttu-id="0439b-366">![Začněte psát název fragmentu](build-restful-apis-with-aspnet-web-api/_static/image29.png "začněte psát název fragmentu kódu")</span><span class="sxs-lookup"><span data-stu-id="0439b-366">![Start typing the snippet name](build-restful-apis-with-aspnet-web-api/_static/image29.png "Start typing the snippet name")</span></span>

    <span data-ttu-id="0439b-367">*Začněte psát název fragmentu kódu*</span><span class="sxs-lookup"><span data-stu-id="0439b-367">*Start typing the snippet name*</span></span>

    <span data-ttu-id="0439b-368">![Stisknutím klávesy Tab vyberte fragmentu zvýrazněná](build-restful-apis-with-aspnet-web-api/_static/image30.png "stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu")</span><span class="sxs-lookup"><span data-stu-id="0439b-368">![Press Tab to select the highlighted snippet](build-restful-apis-with-aspnet-web-api/_static/image30.png "Press Tab to select the highlighted snippet")</span></span>

    <span data-ttu-id="0439b-369">*Stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu*</span><span class="sxs-lookup"><span data-stu-id="0439b-369">*Press Tab to select the highlighted snippet*</span></span>

    <span data-ttu-id="0439b-370">![Stisknutím klávesy Tab znovu a fragmentu rozšíří](build-restful-apis-with-aspnet-web-api/_static/image31.png "stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.")</span><span class="sxs-lookup"><span data-stu-id="0439b-370">![Press Tab again and the snippet will expand](build-restful-apis-with-aspnet-web-api/_static/image31.png "Press Tab again and the snippet will expand")</span></span>

    <span data-ttu-id="0439b-371">*Stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.*</span><span class="sxs-lookup"><span data-stu-id="0439b-371">*Press Tab again and the snippet will expand*</span></span>

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a><span data-ttu-id="0439b-372">Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)</span><span class="sxs-lookup"><span data-stu-id="0439b-372">To add a code snippet using the mouse (C#, Visual Basic and XML)</span></span>

1. <span data-ttu-id="0439b-373">Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="0439b-373">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="0439b-374">Vyberte **Vložit fragment** následuje **Moje fragmenty kódu**.</span><span class="sxs-lookup"><span data-stu-id="0439b-374">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="0439b-375">Vyberte relevantní fragment kódu ze seznamu, kliknutím na.</span><span class="sxs-lookup"><span data-stu-id="0439b-375">Pick the relevant snippet from the list, by clicking on it.</span></span>

    <span data-ttu-id="0439b-376">![Klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment](build-restful-apis-with-aspnet-web-api/_static/image32.png "klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment")</span><span class="sxs-lookup"><span data-stu-id="0439b-376">![Right-click where you want to insert the code snippet and select Insert Snippet](build-restful-apis-with-aspnet-web-api/_static/image32.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

    <span data-ttu-id="0439b-377">*Klikněte pravým tlačítkem myši, kam chcete vložit fragment kódu a vyberte Vložit fragment*</span><span class="sxs-lookup"><span data-stu-id="0439b-377">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

    <span data-ttu-id="0439b-378">![Vyberte relevantní fragment kódu ze seznamu, kliknutím na](build-restful-apis-with-aspnet-web-api/_static/image33.png "vyberte relevantní fragment kódu ze seznamu, kliknutím na")</span><span class="sxs-lookup"><span data-stu-id="0439b-378">![Pick the relevant snippet from the list, by clicking on it](build-restful-apis-with-aspnet-web-api/_static/image33.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

    <span data-ttu-id="0439b-379">*Vyberte relevantní fragment kódu ze seznamu, kliknutím na*</span><span class="sxs-lookup"><span data-stu-id="0439b-379">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="0439b-380">Příloha B: instalaci sady Visual Studio Express 2012 pro Web</span><span class="sxs-lookup"><span data-stu-id="0439b-380">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="0439b-381">Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiný &quot;Express&quot; pomocí verze **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="0439b-381">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="0439b-382">Následující pokyny vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="0439b-382">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="0439b-383">Přejděte na [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="0439b-383">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="0439b-384">Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produktu &quot; <em>Visual Studio Express 2012 pro Web se sadou Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="0439b-384">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="0439b-385">Klikněte na **nyní nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="0439b-385">Click on **Install Now**.</span></span> <span data-ttu-id="0439b-386">Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.</span><span class="sxs-lookup"><span data-stu-id="0439b-386">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="0439b-387">Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** zahájíte instalaci.</span><span class="sxs-lookup"><span data-stu-id="0439b-387">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="0439b-388">![Nainstalovat Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "nainstalovat Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="0439b-388">![Install Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="0439b-389">*Nainstalovat Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="0439b-389">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="0439b-390">Číst všechny produkty se licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="0439b-390">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Vyjádření souhlasu s podmínkami licence](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    <span data-ttu-id="0439b-392">*Vyjádření souhlasu s podmínkami licence*</span><span class="sxs-lookup"><span data-stu-id="0439b-392">*Accepting the license terms*</span></span>
5. <span data-ttu-id="0439b-393">Počkejte na dokončení procesu stahování a instalaci.</span><span class="sxs-lookup"><span data-stu-id="0439b-393">Wait until the downloading and installation process completes.</span></span>

    ![Průběh instalace](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    <span data-ttu-id="0439b-395">*Průběh instalace*</span><span class="sxs-lookup"><span data-stu-id="0439b-395">*Installation progress*</span></span>
6. <span data-ttu-id="0439b-396">Po dokončení instalace, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="0439b-396">When the installation completes, click **Finish**.</span></span>

    ![Instalace byla dokončena.](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    <span data-ttu-id="0439b-398">*Instalace byla dokončena.*</span><span class="sxs-lookup"><span data-stu-id="0439b-398">*Installation completed*</span></span>
7. <span data-ttu-id="0439b-399">Klikněte na tlačítko **ukončení** ukončíte instalační program webové platformy.</span><span class="sxs-lookup"><span data-stu-id="0439b-399">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="0439b-400">Chcete-li spustit nástroj Visual Studio Express pro Web, přejděte na **spustit** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express pro Web** dlaždice.</span><span class="sxs-lookup"><span data-stu-id="0439b-400">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express pro Web dlaždice](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    <span data-ttu-id="0439b-402">*VS Express pro Web dlaždice*</span><span class="sxs-lookup"><span data-stu-id="0439b-402">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="0439b-403">Příloha C: publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu</span><span class="sxs-lookup"><span data-stu-id="0439b-403">Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="0439b-404">Tento dodatek vám ukáže, jak vytvořit nový web z portálu Azure a publikování aplikace, kterou jste získali podle testovacím prostředí, využívat výhod nasazení webu publikování funkce poskytované platformou Azure.</span><span class="sxs-lookup"><span data-stu-id="0439b-404">This appendix will show you how to create a new web site from the Azure Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Azure.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a><span data-ttu-id="0439b-405">Úloha 1 – Vytvoření nového webu z portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0439b-405">Task 1 - Creating a New Web Site from the Azure Portal</span></span>

1. <span data-ttu-id="0439b-406">Přejděte na [portálu pro správu Azure](https://manage.windowsazure.com/) a přihlaste se pomocí přihlašovacích údajů společnosti Microsoft, které jsou spojené s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="0439b-406">Go to the [Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0439b-407">S Azure můžete bezplatné hostování 10 webů ASP.NET a pak škálujte podle rozšiřujícího se provozu.</span><span class="sxs-lookup"><span data-stu-id="0439b-407">With Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="0439b-408">Můžete si zaregistrovat [zde](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="0439b-408">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="0439b-409">![Přihlaste se k portálu Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image39.png "Přihlaste se k portálu Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="0439b-409">![Log on to Windows Azure portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="0439b-410">*Přihlaste se k portálu*</span><span class="sxs-lookup"><span data-stu-id="0439b-410">*Log on to Portal*</span></span>
2. <span data-ttu-id="0439b-411">Klikněte na tlačítko **nový** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="0439b-411">Click **New** on the command bar.</span></span>

    <span data-ttu-id="0439b-412">![Vytvoření nového webu](build-restful-apis-with-aspnet-web-api/_static/image40.png "vytváření nového webu")</span><span class="sxs-lookup"><span data-stu-id="0439b-412">![Creating a new Web Site](build-restful-apis-with-aspnet-web-api/_static/image40.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="0439b-413">*Vytvoření nového webu*</span><span class="sxs-lookup"><span data-stu-id="0439b-413">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="0439b-414">Klikněte na tlačítko **výpočetní** | **webu**.</span><span class="sxs-lookup"><span data-stu-id="0439b-414">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="0439b-415">Potom vyberte **rychle vytvořit** možnost.</span><span class="sxs-lookup"><span data-stu-id="0439b-415">Then select **Quick Create** option.</span></span> <span data-ttu-id="0439b-416">Zadejte adresu URL k dispozici pro nový web a klikněte na tlačítko **vytvoření webu**.</span><span class="sxs-lookup"><span data-stu-id="0439b-416">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0439b-417">Azure je hostitel pro spouštění v cloudu, ve kterém můžete řídit a spravovat webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0439b-417">Azure is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="0439b-418">Možnost rychle vytvořit můžete nasadit hotové webové aplikace do Azure z mimo portál.</span><span class="sxs-lookup"><span data-stu-id="0439b-418">The Quick Create option allows you to deploy a completed web application to the Azure from outside the portal.</span></span> <span data-ttu-id="0439b-419">Postup pro nastavení databáze neobsahuje.</span><span class="sxs-lookup"><span data-stu-id="0439b-419">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="0439b-420">![Vytvoření nového webu pomocí metody rychlého vytvoření](build-restful-apis-with-aspnet-web-api/_static/image41.png "vytváření nového webu pomocí metody rychlého vytvoření")</span><span class="sxs-lookup"><span data-stu-id="0439b-420">![Creating a new Web Site using Quick Create](build-restful-apis-with-aspnet-web-api/_static/image41.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="0439b-421">*Vytvoření nového webu pomocí metody rychlého vytvoření*</span><span class="sxs-lookup"><span data-stu-id="0439b-421">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="0439b-422">Počkejte, dokud nové **webu** je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="0439b-422">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="0439b-423">Po vytvoření webu klikněte na odkaz v části **URL** sloupce.</span><span class="sxs-lookup"><span data-stu-id="0439b-423">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="0439b-424">Zkontrolujte, zda je funkční nový web.</span><span class="sxs-lookup"><span data-stu-id="0439b-424">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="0439b-425">![Procházení na nový web](build-restful-apis-with-aspnet-web-api/_static/image42.png "procházení na nový web")</span><span class="sxs-lookup"><span data-stu-id="0439b-425">![Browsing to the new web site](build-restful-apis-with-aspnet-web-api/_static/image42.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="0439b-426">*Procházení na nový web*</span><span class="sxs-lookup"><span data-stu-id="0439b-426">*Browsing to the new web site*</span></span>

    <span data-ttu-id="0439b-427">![Webový server spuštěn](build-restful-apis-with-aspnet-web-api/_static/image43.png "webu systémem")</span><span class="sxs-lookup"><span data-stu-id="0439b-427">![Web site running](build-restful-apis-with-aspnet-web-api/_static/image43.png "Web site running")</span></span>

    <span data-ttu-id="0439b-428">*Spuštění webu*</span><span class="sxs-lookup"><span data-stu-id="0439b-428">*Web site running*</span></span>
6. <span data-ttu-id="0439b-429">Přejděte zpět na portál a klikněte na název webu v části **název** sloupec pro zobrazení stránky pro správu.</span><span class="sxs-lookup"><span data-stu-id="0439b-429">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="0439b-430">![Otevření stránky Správa webu](build-restful-apis-with-aspnet-web-api/_static/image44.png "otevření stránek správu webového serveru")</span><span class="sxs-lookup"><span data-stu-id="0439b-430">![Opening the web site management pages](build-restful-apis-with-aspnet-web-api/_static/image44.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="0439b-431">*Otevření stránek správu webového serveru*</span><span class="sxs-lookup"><span data-stu-id="0439b-431">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="0439b-432">V **řídicí panel** v části **rychlého přehledu** klikněte na položku **stažení profilu publikování** odkaz.</span><span class="sxs-lookup"><span data-stu-id="0439b-432">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0439b-433">*Profilu publikování* obsahuje všechny informace požadované pro publikování webové aplikace do Azure pro každou metodu povoleno publikace.</span><span class="sxs-lookup"><span data-stu-id="0439b-433">The *publish profile* contains all of the information required to publish a web application to a Azure for each enabled publication method.</span></span> <span data-ttu-id="0439b-434">Profil publikování obsahuje adresy URL, přihlašovací údaje uživatele a řetězců databází, které jsou potřebné k připojení k a ověřování na základě těchto koncových bodů, pro které je metoda publikace povolena.</span><span class="sxs-lookup"><span data-stu-id="0439b-434">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="0439b-435">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pro Web** a **sadu Microsoft Visual Studio 2012** podporu čtení publikační profily k automatické konfiguraci těchto programů pro publikování webových aplikací do Azure.</span><span class="sxs-lookup"><span data-stu-id="0439b-435">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Azure.</span></span>

    <span data-ttu-id="0439b-436">![Na webu stažení profilu publikování](build-restful-apis-with-aspnet-web-api/_static/image45.png "stahování webové stránky profilu publikování")</span><span class="sxs-lookup"><span data-stu-id="0439b-436">![Downloading the web site publish profile](build-restful-apis-with-aspnet-web-api/_static/image45.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="0439b-437">*Na webu stažení profilu publikování*</span><span class="sxs-lookup"><span data-stu-id="0439b-437">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="0439b-438">Stáhněte si soubor profil publikování do vhodného umístění.</span><span class="sxs-lookup"><span data-stu-id="0439b-438">Download the publish profile file to a known location.</span></span> <span data-ttu-id="0439b-439">Dále v tomto cvičení uvidíte jak používat tento soubor k publikování webové aplikace do Azure ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0439b-439">Further in this exercise you will see how to use this file to publish a web application to Azure from Visual Studio.</span></span>

    <span data-ttu-id="0439b-440">![Ukládání souboru profilu publikování](build-restful-apis-with-aspnet-web-api/_static/image46.png "ukládání profilu publikování")</span><span class="sxs-lookup"><span data-stu-id="0439b-440">![Saving the publish profile file](build-restful-apis-with-aspnet-web-api/_static/image46.png "Saving the publish profile")</span></span>

    <span data-ttu-id="0439b-441">*Ukládání souboru profilu publikování*</span><span class="sxs-lookup"><span data-stu-id="0439b-441">*Saving the publish profile file*</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="0439b-442">Úloha 2 – konfigurování serveru databáze</span><span class="sxs-lookup"><span data-stu-id="0439b-442">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="0439b-443">Pokud vaše aplikace využívá systému SQL Server, databáze, budete muset vytvořit databázi SQL server.</span><span class="sxs-lookup"><span data-stu-id="0439b-443">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="0439b-444">Pokud chcete nasadit jednoduchou aplikaci, která nepoužívá systém SQL Server může tuto úlohu přeskočit.</span><span class="sxs-lookup"><span data-stu-id="0439b-444">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="0439b-445">Budete potřebovat databázi SQL serveru pro ukládání databázi aplikace.</span><span class="sxs-lookup"><span data-stu-id="0439b-445">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="0439b-446">Databáze SQL servery můžete zobrazit ze svého předplatného v portálu Azure Management portal na **databází Sql** | **servery** | **řídicího panelu serveru**.</span><span class="sxs-lookup"><span data-stu-id="0439b-446">You can view the SQL Database servers from your subscription in the Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="0439b-447">Pokud nemáte server vytvořeno, můžete vytvořit jeden pomocí **přidat** tlačítka na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="0439b-447">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="0439b-448">Poznamenejte si **název serveru a adresa URL, správce přihlašovací jméno a heslo**, jako je použijete v další úkoly.</span><span class="sxs-lookup"><span data-stu-id="0439b-448">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="0439b-449">Nevytvářejte databáze ještě, jak bude vytvořen v pozdější fázi.</span><span class="sxs-lookup"><span data-stu-id="0439b-449">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="0439b-450">![Řídicí panel serveru databáze SQL](build-restful-apis-with-aspnet-web-api/_static/image47.png "řídicího panelu serveru databáze SQL")</span><span class="sxs-lookup"><span data-stu-id="0439b-450">![SQL Database Server Dashboard](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="0439b-451">*Řídicí panel serveru databáze SQL*</span><span class="sxs-lookup"><span data-stu-id="0439b-451">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="0439b-452">V dalším úkolem budete testovat připojení k databázi ze sady Visual Studio, proto je nutné zahrnout místní IP adresa serveru seznamu **povolené IP adresy**.</span><span class="sxs-lookup"><span data-stu-id="0439b-452">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="0439b-453">To lze provést, klikněte na tlačítko **konfigurace**, vyberte IP adresu z **aktuální IP adresa klienta** a vkládání na **počáteční IP adresa** a **Koncová IP adresa** textová pole a kliknutím ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0439b-453">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) button.</span></span>

    ![Přidávání IP adresy klienta](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    <span data-ttu-id="0439b-455">*Přidávání IP adresy klienta*</span><span class="sxs-lookup"><span data-stu-id="0439b-455">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="0439b-456">Jednou **IP adresa klienta** je povolené IP adresy do seznamu, klikněte na **Uložit** potvrďte změny.</span><span class="sxs-lookup"><span data-stu-id="0439b-456">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Potvrzení změn](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    <span data-ttu-id="0439b-458">*Potvrzení změn*</span><span class="sxs-lookup"><span data-stu-id="0439b-458">*Confirm Changes*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="0439b-459">Úloha 3 – publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu</span><span class="sxs-lookup"><span data-stu-id="0439b-459">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="0439b-460">Přejděte zpět na ASP.NET MVC 4 řešení.</span><span class="sxs-lookup"><span data-stu-id="0439b-460">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="0439b-461">V **Průzkumníku řešení**, klikněte pravým tlačítkem na webový projekt a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="0439b-461">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="0439b-462">![Publikování aplikace](build-restful-apis-with-aspnet-web-api/_static/image51.png "publikování aplikace")</span><span class="sxs-lookup"><span data-stu-id="0439b-462">![Publishing the Application](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publishing the Application")</span></span>

    <span data-ttu-id="0439b-463">*Publikování webu*</span><span class="sxs-lookup"><span data-stu-id="0439b-463">*Publishing the web site*</span></span>
2. <span data-ttu-id="0439b-464">Umožňuje naimportujte profil publikování, který jste uložili v první úloze.</span><span class="sxs-lookup"><span data-stu-id="0439b-464">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="0439b-465">![Import profilu publikování](build-restful-apis-with-aspnet-web-api/_static/image52.png "import profilu publikování")</span><span class="sxs-lookup"><span data-stu-id="0439b-465">![Importing the publish profile](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importing the publish profile")</span></span>

    <span data-ttu-id="0439b-466">*Import profilu publikování*</span><span class="sxs-lookup"><span data-stu-id="0439b-466">*Importing publish profile*</span></span>
3. <span data-ttu-id="0439b-467">Klikněte na tlačítko **ověření připojení**.</span><span class="sxs-lookup"><span data-stu-id="0439b-467">Click **Validate Connection**.</span></span> <span data-ttu-id="0439b-468">Po dokončení ověření klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="0439b-468">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0439b-469">Ověření je hotová, jakmile se zobrazí zelené zaškrtnutí zobrazí vedle tlačítko ověřit připojení.</span><span class="sxs-lookup"><span data-stu-id="0439b-469">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="0439b-470">![Ověření připojení](build-restful-apis-with-aspnet-web-api/_static/image53.png "ověřování připojení")</span><span class="sxs-lookup"><span data-stu-id="0439b-470">![Validating connection](build-restful-apis-with-aspnet-web-api/_static/image53.png "Validating connection")</span></span>

    <span data-ttu-id="0439b-471">*Ověření připojení*</span><span class="sxs-lookup"><span data-stu-id="0439b-471">*Validating connection*</span></span>
4. <span data-ttu-id="0439b-472">V **nastavení** v části **databáze** části, klikněte na tlačítko vedle připojení databáze textové pole (tj. **objekt DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="0439b-472">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="0439b-473">![Konfigurace nasazení webu](build-restful-apis-with-aspnet-web-api/_static/image54.png "konfigurace nasazení webu")</span><span class="sxs-lookup"><span data-stu-id="0439b-473">![Web deploy configuration](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web deploy configuration")</span></span>

    <span data-ttu-id="0439b-474">*Konfigurace nasazení webu*</span><span class="sxs-lookup"><span data-stu-id="0439b-474">*Web deploy configuration*</span></span>
5. <span data-ttu-id="0439b-475">Připojení k databázi nakonfigurujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="0439b-475">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="0439b-476">V **název serveru** zadejte vaše databáze SQL serveru adresu URL pomocí *tcp:* předponu.</span><span class="sxs-lookup"><span data-stu-id="0439b-476">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="0439b-477">V **uživatelské jméno** zadejte vaše přihlašovací jméno správce serveru.</span><span class="sxs-lookup"><span data-stu-id="0439b-477">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="0439b-478">V **heslo** zadejte přihlašovací heslo správce serveru.</span><span class="sxs-lookup"><span data-stu-id="0439b-478">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="0439b-479">Zadejte nový název databáze, například: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="0439b-479">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="0439b-480">![Konfigurace cílový připojovací řetězec](build-restful-apis-with-aspnet-web-api/_static/image55.png "konfigurace cílový připojovací řetězec")</span><span class="sxs-lookup"><span data-stu-id="0439b-480">![Configuring destination connection string](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="0439b-481">*Konfigurace cílový připojovací řetězec*</span><span class="sxs-lookup"><span data-stu-id="0439b-481">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="0439b-482">Pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="0439b-482">Then click **OK**.</span></span> <span data-ttu-id="0439b-483">Po zobrazení výzvy k vytvoření databáze, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="0439b-483">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="0439b-484">![Vytvoření databáze](build-restful-apis-with-aspnet-web-api/_static/image56.png "vytváření řetězec databáze")</span><span class="sxs-lookup"><span data-stu-id="0439b-484">![Creating the database](build-restful-apis-with-aspnet-web-api/_static/image56.png "Creating the database string")</span></span>

    <span data-ttu-id="0439b-485">*Vytvoření databáze*</span><span class="sxs-lookup"><span data-stu-id="0439b-485">*Creating the database*</span></span>
7. <span data-ttu-id="0439b-486">Připojovací řetězec, který budete používat pro připojení k databázi SQL v systému Windows Azure je uvedené v rámci textbox výchozí připojení.</span><span class="sxs-lookup"><span data-stu-id="0439b-486">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="0439b-487">Pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="0439b-487">Then click **Next**.</span></span>

    <span data-ttu-id="0439b-488">![Připojovací řetězec odkazující na databázi SQL](build-restful-apis-with-aspnet-web-api/_static/image57.png "připojovací řetězec odkazující na databázi SQL")</span><span class="sxs-lookup"><span data-stu-id="0439b-488">![Connection string pointing to SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="0439b-489">*Připojovací řetězec odkazující na databázi SQL*</span><span class="sxs-lookup"><span data-stu-id="0439b-489">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="0439b-490">V **Preview** klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="0439b-490">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="0439b-491">![Publikování webové aplikace](build-restful-apis-with-aspnet-web-api/_static/image58.png "publikování webové aplikace")</span><span class="sxs-lookup"><span data-stu-id="0439b-491">![Publishing the web application](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publishing the web application")</span></span>

    <span data-ttu-id="0439b-492">*Publikování webové aplikace*</span><span class="sxs-lookup"><span data-stu-id="0439b-492">*Publishing the web application*</span></span>
9. <span data-ttu-id="0439b-493">Jakmile proces publikování dokončí, otevře se výchozí prohlížeč publikované webové stránky.</span><span class="sxs-lookup"><span data-stu-id="0439b-493">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="0439b-494">![Aplikace publikována do služby Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "aplikace publikována do služby Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="0439b-494">![Application published to Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="0439b-495">*Aplikace, které jsou publikovány do služby Azure*</span><span class="sxs-lookup"><span data-stu-id="0439b-495">*Application published to Azure*</span></span>
