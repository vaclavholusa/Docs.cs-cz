---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Vytvoření rozhraní RESTful API s rozhraním ASP.NET Web API | Dokumentace Microsoftu
author: rick-anderson
description: V posledních letech se stal jasné, že protokol HTTP není jen pro poskytovat stránky HTML. Je také výkonnou platformu pro vytváření webových rozhraní API, pomocí několika málo o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 88ac5102a1cf14050412abc336e7a8260a9fa80d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363517"
---
<a name="build-restful-apis-with-aspnet-web-api"></a><span data-ttu-id="2a5f1-104">Vytvoření rozhraní RESTful API s rozhraním ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="2a5f1-104">Build RESTful APIs with ASP.NET Web API</span></span>
====================
<span data-ttu-id="2a5f1-105">podle [Campy Web týmu](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="2a5f1-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="2a5f1-106">V posledních letech se stal jasné, že protokol HTTP není jen pro poskytovat stránky HTML.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-106">In recent years, it has become clear that HTTP is not just for serving up HTML pages.</span></span> <span data-ttu-id="2a5f1-107">Je také výkonnou platformu pro vytváření webových rozhraní API, pomocí několika příkazů (GET, POST a tak dále) a navíc pár jednoduchými koncepty, jako *identifikátory URI* a *záhlaví*.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-107">It is also a powerful platform for building Web APIs, using a handful of verbs (GET, POST, and so forth) plus a few simple concepts such as *URIs* and *headers*.</span></span> <span data-ttu-id="2a5f1-108">ASP.NET Web API je sada komponent, které zjednodušují programování HTTP.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-108">ASP.NET Web API is a set of components that simplify HTTP programming.</span></span> <span data-ttu-id="2a5f1-109">Protože je postavený na modul runtime ASP.NET MVC, webové rozhraní API automaticky zpracovává podrobnosti nízké úrovně přenos HTTP.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-109">Because it is built on top of the ASP.NET MVC runtime, Web API automatically handles the low-level transport details of HTTP.</span></span> <span data-ttu-id="2a5f1-110">Ve stejnou dobu přirozeně webového rozhraní API poskytuje programovací model protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-110">At the same time, Web API naturally exposes the HTTP programming model.</span></span> <span data-ttu-id="2a5f1-111">Ve skutečnosti je jedním z cílů webového rozhraní API *není* abstrakci ve skutečnosti je http.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-111">In fact, one goal of Web API is to *not* abstract away the reality of HTTP.</span></span> <span data-ttu-id="2a5f1-112">V důsledku toho webového rozhraní API je flexibilní a snadno rozšířit.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-112">As a result, Web API is both flexible and easy to extend.</span></span> <span data-ttu-id="2a5f1-113">V této praktické testovací prostředí použijete webového rozhraní API k vytvoření jednoduchého rozhraní REST API pro aplikace požádejte správce.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-113">In this hands-on lab, you will use Web API to build a simple REST API for a contact manager application.</span></span> <span data-ttu-id="2a5f1-114">Pokud vytvoříte klienta k využití rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-114">You will also build a client to consume the API.</span></span> <span data-ttu-id="2a5f1-115">Styl architektury REST ukázal být účinný způsob, jak využít HTTP – i když to určitě není platné pouze přístup k protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-115">The REST architectural style has proven to be an effective way to leverage HTTP - although it is certainly not the only valid approach to HTTP.</span></span> <span data-ttu-id="2a5f1-116">Požádejte správce, bude vystavovat RESTful pro výpis, přidávání a odebírání kontakty, mimo jiné.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-116">The contact manager will expose the RESTful for listing, adding and removing contacts, among others.</span></span> <span data-ttu-id="2a5f1-117">Toto testovací prostředí vyžaduje základní znalost HTTP, REST a předpokládá, že máte základní znalosti pracovních HTML, JavaScript a jQuery.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-117">This lab requires a basic understanding of HTTP, REST, and assumes you have a basic working knowledge of HTML, JavaScript, and jQuery.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="2a5f1-118">Webu technologie ASP.NET je vyhrazený pro rozhraní ASP.NET Web API v oblasti [ https://asp.net/web-api ](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="2a5f1-118">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [https://asp.net/web-api](https://asp.net/web-api).</span></span> <span data-ttu-id="2a5f1-119">Tento web i nadále bude poskytovat nejnovější informace, ukázky a zpráv týkající se webového rozhraní API, takže vrácení se často Pokud byste chtěli delve hlouběji do obrázky vytváření vlastních webových rozhraní API k dispozici na prakticky jakékoli zařízení nebo vývoj rozhraní framework.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-119">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>
> > 
> > <span data-ttu-id="2a5f1-120">Rozhraní ASP.NET Web API, podobně jako na ASP.NET MVC 4, má velkou flexibilitu z hlediska oddělení vrstva služby z řadiče, abyste mohli používat některé z dostupných injektáž závislostí architektury poměrně snadné.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-120">ASP.NET Web API, similar to ASP.NET MVC 4, has great flexibility in terms of separating the service layer from the controllers allowing you to use several of the available Dependency Injection frameworks fairly easy.</span></span> <span data-ttu-id="2a5f1-121">Na webu MSDN, který ukazuje, jak používat Ninject pro injektáž závislostí v projektu webového rozhraní API ASP.NET, který si můžete stáhnout z je dobrý vzorek [tady](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span><span class="sxs-lookup"><span data-stu-id="2a5f1-121">There is a good sample in MSDN that shows how to use Ninject for dependency injection in an ASP.NET Web API project that you can download it from [here](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span></span>
> 
> 
> <span data-ttu-id="2a5f1-122">Všechny ukázky kódu a fragmenty kódu jsou součástí této webové Campy školicí sady, k dispozici na [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="2a5f1-122">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="2a5f1-123">Cíle</span><span class="sxs-lookup"><span data-stu-id="2a5f1-123">Objectives</span></span>

<span data-ttu-id="2a5f1-124">V této praktická cvičení se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="2a5f1-124">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="2a5f1-125">Implementovat rozhraní RESTful webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="2a5f1-125">Implement a RESTful Web API</span></span>
- <span data-ttu-id="2a5f1-126">Volání rozhraní API z klienta HTML</span><span class="sxs-lookup"><span data-stu-id="2a5f1-126">Call the API from an HTML client</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="2a5f1-127">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2a5f1-127">Prerequisites</span></span>

<span data-ttu-id="2a5f1-128">K dokončení této praktické testovací prostředí jsou vyžadovány následující položky:</span><span class="sxs-lookup"><span data-stu-id="2a5f1-128">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="2a5f1-129">[Microsoft Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) nebo i vyšší (čtení [dodatku B](#AppendixB) pokyny k jeho instalaci).</span><span class="sxs-lookup"><span data-stu-id="2a5f1-129">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="2a5f1-130">Instalace</span><span class="sxs-lookup"><span data-stu-id="2a5f1-130">Setup</span></span>

<span data-ttu-id="2a5f1-131">**Instalace fragmenty kódu**</span><span class="sxs-lookup"><span data-stu-id="2a5f1-131">**Installing Code Snippets**</span></span>

<span data-ttu-id="2a5f1-132">Pro usnadnění práce velkou část kódu, které budete spravovat podél tohoto testovacího prostředí je k dispozici jako fragmenty kódu sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-132">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="2a5f1-133">K instalaci spustit fragmenty kódu **.\Source\Setup\CodeSnippets.vsi** souboru.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-133">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="2a5f1-134">Pokud nejste obeznámeni s fragmenty kódu Visual Studio a chcete další informace o jejich použití, najdete dodatku z tohoto dokumentu &quot; [fragmenty kódu pomocí dodatku A:](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-134">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="2a5f1-135">Cvičení</span><span class="sxs-lookup"><span data-stu-id="2a5f1-135">Exercises</span></span>

<span data-ttu-id="2a5f1-136">Toto praktické testovací prostředí obsahuje následující cvičení:</span><span class="sxs-lookup"><span data-stu-id="2a5f1-136">This hands-on lab includes the following exercise:</span></span>

1. [<span data-ttu-id="2a5f1-137">Cvičení 1: Vytvoření webového rozhraní API jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="2a5f1-137">Exercise 1: Create a Read-Only Web API</span></span>](#Exercise1)
2. [<span data-ttu-id="2a5f1-138">Cvičení 2: Vytvoření webového rozhraní API pro čtení a zápis</span><span class="sxs-lookup"><span data-stu-id="2a5f1-138">Exercise 2: Create a Read/Write Web API</span></span>](#Exercise2)
3. [<span data-ttu-id="2a5f1-139">Cvičení 3: Využívají webové rozhraní API z klienta HTML</span><span class="sxs-lookup"><span data-stu-id="2a5f1-139">Exercise 3: Consume the Web API from an HTML Client</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="2a5f1-140">Se sadou každý cvičení **koncové** složku, která obsahuje výsledný řešení byste měli získat po dokončení cvičení.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-140">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="2a5f1-141">Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc prostřednictvím praktická cvičení.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-141">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="2a5f1-142">Odhadovaný čas dokončení tohoto testovacího prostředí: **60 minut**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-142">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a><span data-ttu-id="2a5f1-143">Cvičení 1: Vytvoření webového rozhraní API jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="2a5f1-143">Exercise 1: Create a Read-Only Web API</span></span>

<span data-ttu-id="2a5f1-144">V tomto cvičení implementujete metody GET jen pro čtení pro kontaktujte správce.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-144">In this exercise, you will implement the read-only GET methods for the contact manager.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a><span data-ttu-id="2a5f1-145">Úloha 1 – Vytvoření projektu rozhraní API</span><span class="sxs-lookup"><span data-stu-id="2a5f1-145">Task 1 - Creating the API Project</span></span>

<span data-ttu-id="2a5f1-146">V této úloze použijete nové šablony webových projektů ASP.NET a vytvořte webovou aplikaci webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-146">In this task, you will use the new ASP.NET web project templates to create a Web API web application.</span></span>

1. <span data-ttu-id="2a5f1-147">Spustit **Visual Studio 2012 Express pro Web**, přejděte k **Start** a typ **VS Express for Web** stiskněte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-147">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="2a5f1-148">Z **souboru** nabídce vyberte možnost **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-148">From the **File** menu, select **New Project**.</span></span> <span data-ttu-id="2a5f1-149">Vyberte **Visual C# | Web** typ ve stromovém zobrazení projektu typu projektu a pak vyberte **webové aplikace ASP.NET MVC 4** typ projektu.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-149">Select the **Visual C# | Web** project type from the project type tree view, then select the **ASP.NET MVC 4 Web Application** project type.</span></span> <span data-ttu-id="2a5f1-150">Nastavení projektu **název** k *ContactManager* a **název řešení** k *začít*, pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-150">Set the project's **Name** to *ContactManager* and the **Solution name** to *Begin*, then click **OK**.</span></span>

    <span data-ttu-id="2a5f1-151">![Vytváří se nové technologie ASP.NET MVC 4.0 projekt webové aplikace](build-restful-apis-with-aspnet-web-api/_static/image1.png "vytváření nové technologie ASP.NET MVC 4.0 projekt webové aplikace")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-151">![Creating a new ASP.NET MVC 4.0 Web Application Project](build-restful-apis-with-aspnet-web-api/_static/image1.png "Creating a new ASP.NET MVC 4.0 Web Application Project")</span></span>

    <span data-ttu-id="2a5f1-152">*Vytváří se nové technologie ASP.NET MVC 4.0 projekt webové aplikace*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-152">*Creating a new ASP.NET MVC 4.0 Web Application Project*</span></span>
3. <span data-ttu-id="2a5f1-153">V dialogu typu projektu ASP.NET MVC 4, vyberte **webového rozhraní API** typ projektu.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-153">In the ASP.NET MVC 4 project type dialog, select the **Web API** project type.</span></span> <span data-ttu-id="2a5f1-154">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-154">Click **OK**.</span></span>

    <span data-ttu-id="2a5f1-155">![Určení typu projektu webového rozhraní API](build-restful-apis-with-aspnet-web-api/_static/image2.png "určující typ projektu webového rozhraní API")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-155">![Specifying the Web API project type](build-restful-apis-with-aspnet-web-api/_static/image2.png "Specifying the Web API project type")</span></span>

    <span data-ttu-id="2a5f1-156">*Určení typu projektu webového rozhraní API*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-156">*Specifying the Web API project type*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a><span data-ttu-id="2a5f1-157">Úloha 2 – Vytvoření Kontrolery rozhraní API Správce kontaktů</span><span class="sxs-lookup"><span data-stu-id="2a5f1-157">Task 2 - Creating the Contact Manager API Controllers</span></span>

<span data-ttu-id="2a5f1-158">V této úloze vytvoříte třídy kontroleru, ve kterých se bude nacházet metody rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-158">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="2a5f1-159">Odstranit soubor s názvem **ValuesController.cs** v rámci **řadiče** složku z projektu.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-159">Delete the file named **ValuesController.cs** within **Controllers** folder from the project.</span></span>
2. <span data-ttu-id="2a5f1-160">Klikněte pravým tlačítkem myši **řadiče** složky v projektu a vyberte **přidat | Kontroler** v místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-160">Right-click the **Controllers** folder in the project and select **Add | Controller** from the context menu.</span></span>

    <span data-ttu-id="2a5f1-161">![Přidání do projektu nový kontroler](build-restful-apis-with-aspnet-web-api/_static/image3.png "přidání nového řadiče do projektu")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-161">![Adding a new controller to the project](build-restful-apis-with-aspnet-web-api/_static/image3.png "Adding a new controller to the project")</span></span>

    <span data-ttu-id="2a5f1-162">*Přidání nového řadiče do projektu*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-162">*Adding a new controller to the project*</span></span>
3. <span data-ttu-id="2a5f1-163">V **přidat kontroler** dialogové okno, které se zobrazí, vyberte **prázdný kontroler API** z nabídky šablon.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-163">In the **Add Controller** dialog that appears, select **Empty API Controller** from the Template menu.</span></span> <span data-ttu-id="2a5f1-164">Název třídy kontroleru **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-164">Name the controller class **ContactController**.</span></span> <span data-ttu-id="2a5f1-165">Potom klikněte na **přidat.**</span><span class="sxs-lookup"><span data-stu-id="2a5f1-165">Then, click **Add.**</span></span>

    <span data-ttu-id="2a5f1-166">![Chcete-li vytvořit nový kontroler webového rozhraní API pomocí dialogového okna Přidat kontroler](build-restful-apis-with-aspnet-web-api/_static/image4.png "pomocí dialogového okna Přidat kontroler vytvořit nový kontroler Web API")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-166">![Using the Add Controller dialog to create a new Web API controller](build-restful-apis-with-aspnet-web-api/_static/image4.png "Using the Add Controller dialog to create a new Web API controller")</span></span>

    <span data-ttu-id="2a5f1-167">*Chcete-li vytvořit nový kontroler webového rozhraní API pomocí dialogového okna Přidat kontroler*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-167">*Using the Add Controller dialog to create a new Web API controller*</span></span>
4. <span data-ttu-id="2a5f1-168">Přidejte následující kód, který **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-168">Add the following code to the **ContactController**.</span></span>

    <span data-ttu-id="2a5f1-169">(Fragment - kódu *webové rozhraní API Lab – Ex01 - Get – metoda API*)</span><span class="sxs-lookup"><span data-stu-id="2a5f1-169">(Code Snippet - *Web API Lab - Ex01 - Get API Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. <span data-ttu-id="2a5f1-170">Stisknutím klávesy **F5** k ladění aplikace.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-170">Press **F5** to debug the application.</span></span> <span data-ttu-id="2a5f1-171">By se zobrazit výchozí domovskou stránku projektu webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-171">The default home page for a Web API project should appear.</span></span>

    <span data-ttu-id="2a5f1-172">![Výchozí domovskou stránku aplikace ASP.NET Web API](build-restful-apis-with-aspnet-web-api/_static/image5.png "výchozí domovskou stránku aplikace ASP.NET Web API")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-172">![The default home page of an ASP.NET Web API application](build-restful-apis-with-aspnet-web-api/_static/image5.png "The default home page of an ASP.NET Web API application")</span></span>

    <span data-ttu-id="2a5f1-173">*Výchozí domovskou stránku aplikace ASP.NET Web API*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-173">*The default home page of an ASP.NET Web API application*</span></span>
6. <span data-ttu-id="2a5f1-174">V okně Internet Exploreru, stiskněte **F12** klávesy otevřete **vývojářské nástroje** okna.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-174">In the Internet Explorer window, press the **F12** key to open the **Developer Tools** window.</span></span> <span data-ttu-id="2a5f1-175">Klikněte na tlačítko **sítě** kartu a potom klikněte na tlačítko **spustit zachytávání** spusťte zachytávání síťového provozu do okna.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-175">Click the **Network** tab, and then click the **Start Capturing** button to begin capturing network traffic into the window.</span></span>

    <span data-ttu-id="2a5f1-176">![Otevřete kartu síť a inicializaci sítě zachycení](build-restful-apis-with-aspnet-web-api/_static/image6.png "otevřete kartu síť a inicializaci sítě zachytávání")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-176">![Opening the network tab and initiating network capture](build-restful-apis-with-aspnet-web-api/_static/image6.png "Opening the network tab and initiating network capture")</span></span>

    <span data-ttu-id="2a5f1-177">*Otevřete kartu síť a inicializaci zachytávání sítě*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-177">*Opening the network tab and initiating network capture*</span></span>
7. <span data-ttu-id="2a5f1-178">Připojte k adrese URL v adresním řádku prohlížeče s **/api/contact** a stiskněte klávesu enter.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-178">Append the URL in the browser's address bar with **/api/contact** and press enter.</span></span> <span data-ttu-id="2a5f1-179">Přenos podrobností se zobrazí v okně zachytávání sítě.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-179">The transmission details will appear in the network capture window.</span></span> <span data-ttu-id="2a5f1-180">Všimněte si, že je typ MIME odpovědi **application/json**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-180">Note that the response's MIME type is **application/json**.</span></span> <span data-ttu-id="2a5f1-181">Tento příklad ukazuje, jak je výchozí formát výstupu JSON.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-181">This demonstrates how the default output format is JSON.</span></span>

    <span data-ttu-id="2a5f1-182">![Zobrazení výstupu požadavek webového rozhraní API v zobrazení sítě](build-restful-apis-with-aspnet-web-api/_static/image7.png "zobrazení výstupu požadavek webového rozhraní API v okně sítě")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-182">![Viewing the output of the Web API request in the Network view](build-restful-apis-with-aspnet-web-api/_static/image7.png "Viewing the output of the Web API request in the Network view")</span></span>

    <span data-ttu-id="2a5f1-183">*Zobrazení výstupu požadavek webového rozhraní API v okně sítě*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-183">*Viewing the output of the Web API request in the Network view*</span></span>

    > [!NOTE]
    > <span data-ttu-id="2a5f1-184">Výchozí chování Internet Explorer 10 v tomto okamžiku bude dotaz, jestli uživatel chtěli uložit nebo otevřít datový proud výsledek volání webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-184">Internet Explorer 10's default behavior at this point will be to ask if the user would like to save or open the stream resulting from the Web API call.</span></span> <span data-ttu-id="2a5f1-185">Výstup bude textový soubor obsahující JSON výsledek volání adresy URL webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-185">The output will be a text file containing the JSON result of the Web API URL call.</span></span> <span data-ttu-id="2a5f1-186">Nerušit dialogového okna, aby bylo možné sledovat obsah odpovědi pomocí panelu nástrojů pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-186">Do not cancel the dialog in order to be able to watch the response's content through Developers Tool window.</span></span>
8. <span data-ttu-id="2a5f1-187">Klikněte na tlačítko **přejít na podrobnosti přehledu** zobrazíte další podrobnosti o odpovědi toto volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-187">Click the **Go to detailed view** button to see more details about the response of this API call.</span></span>

    <span data-ttu-id="2a5f1-188">![Přepnout do zobrazení podrobné](build-restful-apis-with-aspnet-web-api/_static/image8.png "přepnout na zobrazení podrobností")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-188">![Switch to Detailed View](build-restful-apis-with-aspnet-web-api/_static/image8.png "Switch to Details View")</span></span>

    <span data-ttu-id="2a5f1-189">*Přepnout na podrobné zobrazení*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-189">*Switch to Detailed View*</span></span>
9. <span data-ttu-id="2a5f1-190">Klikněte na tlačítko **tělo odpovědi** kartu k zobrazení vlastní text JSON odpovědi.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-190">Click the **Response body** tab to view the actual JSON response text.</span></span>

    <span data-ttu-id="2a5f1-191">![Zobrazení JSON výstupu textu v nástroji Sledování sítě](build-restful-apis-with-aspnet-web-api/_static/image9.png "zobrazení ve formátu JSON výstupu textu v nástroji Sledování sítě")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-191">![Viewing the JSON output text in the network monitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "Viewing the JSON output text in the network monitor")</span></span>

    <span data-ttu-id="2a5f1-192">*Zobrazení textového výstupu JSON v nástroji Sledování sítě*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-192">*Viewing the JSON output text in the network monitor*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a><span data-ttu-id="2a5f1-193">Úloha 3 – vytvoření kontaktu modely a rozšiřují Contactcontroller</span><span class="sxs-lookup"><span data-stu-id="2a5f1-193">Task 3 - Creating the Contact Models and Augment the Contact Controller</span></span>

<span data-ttu-id="2a5f1-194">V této úloze vytvoříte třídy kontroleru, ve kterých se bude nacházet metody rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-194">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="2a5f1-195">Klikněte pravým tlačítkem myši **modely** a pak zvolte položku **přidat | Třída...**  v místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-195">Right-click the **Models** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="2a5f1-196">![Přidání nového modelu k webové aplikaci](build-restful-apis-with-aspnet-web-api/_static/image10.png "přidání nového modelu do webové aplikace")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-196">![Adding a new model to the web application](build-restful-apis-with-aspnet-web-api/_static/image10.png "Adding a new model to the web application")</span></span>

    <span data-ttu-id="2a5f1-197">*Přidání nového modelu do webové aplikace*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-197">*Adding a new model to the web application*</span></span>
2. <span data-ttu-id="2a5f1-198">V **přidat novou položku** dialogového okna, název nového souboru **Contact.cs** a klikněte na tlačítko **přidat.**</span><span class="sxs-lookup"><span data-stu-id="2a5f1-198">In the **Add New Item** dialog, name the new file **Contact.cs** and click **Add.**</span></span>

    <span data-ttu-id="2a5f1-199">![Vytváří se nový soubor třídy kontakt](build-restful-apis-with-aspnet-web-api/_static/image11.png "vytváří se nový soubor třídy kontaktu")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-199">![Creating the new Contact class file](build-restful-apis-with-aspnet-web-api/_static/image11.png "Creating the new Contact class file")</span></span>

    <span data-ttu-id="2a5f1-200">*Vytváří se nový soubor třídy kontaktu*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-200">*Creating the new Contact class file*</span></span>
3. <span data-ttu-id="2a5f1-201">Přidejte následující zvýrazněný kód do **kontakt** třídy.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-201">Add the following highlighted code to the **Contact** class.</span></span>

    <span data-ttu-id="2a5f1-202">(Fragment - kódu *webové API Lab – Ex01 - kontaktní třídy*)</span><span class="sxs-lookup"><span data-stu-id="2a5f1-202">(Code Snippet - *Web API Lab - Ex01 - Contact Class*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. <span data-ttu-id="2a5f1-203">V **ContactController** vyberte slovo **řetězec** v definici metody **získat** metody a typu slovo *kontakt*.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-203">In the **ContactController** class, select the word **string** in method definition of the **Get** method, and type the word *Contact*.</span></span> <span data-ttu-id="2a5f1-204">Jakmile je zadali slovo, indikátor se zobrazí na začátku slova **kontakt**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-204">Once the word is typed in, an indicator will appear at the beginning of the word **Contact**.</span></span> <span data-ttu-id="2a5f1-205">Buď podržte stisknutou klávesu **Ctrl** klíče a stiskněte klávesu tečka (.) nebo klikněte na ikonu myší a otevřete dialogové okno pomoc v editoru kódu k automatickému vyplnění **pomocí** směrnice pro modely obor názvů.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-205">Either hold down the **Ctrl** key and press the period (.) key or click the icon using your mouse to open up the assistance dialog in the code editor, to automatically fill in the **using** directive for the Models namespace.</span></span>

    ![Pomocí technologie IntelliSense pro deklarace oboru názvů](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    <span data-ttu-id="2a5f1-207">*Pomocí technologie IntelliSense pro deklarace oboru názvů*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-207">*Using Intellisense assistance for namespace declarations*</span></span>
5. <span data-ttu-id="2a5f1-208">Upravte kód **získat** metodu tak, že vrátí pole instancí modelu kontaktu.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-208">Modify the code for the **Get** method so that it returns an array of Contact model instances.</span></span>

    <span data-ttu-id="2a5f1-209">(Fragment - kódu *webové rozhraní API testovacího prostředí – Ex01 - vrácení seznamu kontaktů*)</span><span class="sxs-lookup"><span data-stu-id="2a5f1-209">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. <span data-ttu-id="2a5f1-210">Stisknutím klávesy **F5** ladění webové aplikace v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-210">Press **F5** to debug the web application in the browser.</span></span> <span data-ttu-id="2a5f1-211">Chcete-li zobrazit změny provedené v výstup odezvy rozhraní API, postupujte následovně.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-211">To view the changes made to the response output of the API, perform the following steps.</span></span>

   1. <span data-ttu-id="2a5f1-212">Až se otevře v prohlížeči, klikněte na **F12** Pokud nástroje pro vývojáře zatím nejsou otevřené.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-212">Once the browser opens, press **F12** if the developer tools are not open yet.</span></span>
   2. <span data-ttu-id="2a5f1-213">Klikněte na tlačítko **sítě** kartu.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-213">Click the **Network** tab.</span></span>
   3. <span data-ttu-id="2a5f1-214">Stisknutím klávesy **spustit zachytávání** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-214">Press the **Start Capturing** button.</span></span>
   4. <span data-ttu-id="2a5f1-215">Přidat přípona adresy URL **/api/contact** adresy URL v adresním řádku a stisknutím klávesy **Enter** klíč.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-215">Add the URL suffix **/api/contact** to the URL in the address bar and press the **Enter** key.</span></span>
   5. <span data-ttu-id="2a5f1-216">Stisknutím klávesy **přejít na podrobnou zobrazení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-216">Press the **Go to detailed view** button.</span></span>
   6. <span data-ttu-id="2a5f1-217">Vyberte **tělo odpovědi** kartu. Měli byste vidět řetězec JSON představující serializovanou formu pole instancí kontaktu.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-217">Select the **Response body** tab. You should see a JSON string representing the serialized form of an array of Contact instances.</span></span>

      <span data-ttu-id="2a5f1-218">![JSON serializovat výstup komplexní volání metody webového rozhraní API](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serializovat výstup komplexní volání webového rozhraní API – metoda")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-218">![JSON serialized output of a complex Web API method call](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serialized output of a complex Web API method call")</span></span>

      <span data-ttu-id="2a5f1-219">*Výstup ve formátu JSON serializovat komplexní volání metody webového rozhraní API*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-219">*JSON serialized output of a complex Web API method call*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a><span data-ttu-id="2a5f1-220">Úloha 4 – extrahování funkce do vrstvy služeb</span><span class="sxs-lookup"><span data-stu-id="2a5f1-220">Task 4 - Extracting Functionality into a Service Layer</span></span>

<span data-ttu-id="2a5f1-221">Tato úloha popisuje, jak extrahovat funkci do vrstvy služby k tomu, aby pro vývojáře k oddělení jejich funkcí služby z řadiče vrstvy, a tím umožní opětovné použití služeb, které skutečně pracovat.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-221">This task will demonstrate how to extract functionality into a Service layer to make it easy for developers to separate their service functionality from the controller layer, thereby allowing reusability of the services that actually do the work.</span></span>

1. <span data-ttu-id="2a5f1-222">Vytvořte novou složku v kořenovém adresáři řešení s názvem **služby**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-222">Create a new folder in the solution root and name it **Services**.</span></span> <span data-ttu-id="2a5f1-223">Chcete-li to provést, klikněte pravým tlačítkem na **ContactManager** projekt, vyberte **přidat** | **novou složku**, pojmenujte ho *služby*.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-223">To do this, right-click **ContactManager** project, select **Add** | **New Folder**, name it *Services*.</span></span>

    <span data-ttu-id="2a5f1-224">![Vytvoření složky služby](build-restful-apis-with-aspnet-web-api/_static/image14.png "složku vytvoření služby")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-224">![Creating Services folder](build-restful-apis-with-aspnet-web-api/_static/image14.png "Creating Services folder")</span></span>

    <span data-ttu-id="2a5f1-225">*Vytváření složky služby*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-225">*Creating Services folder*</span></span>
2. <span data-ttu-id="2a5f1-226">Klikněte pravým tlačítkem myši **služby** a pak zvolte položku **přidat | Třída...**  v místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-226">Right-click the **Services** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="2a5f1-227">![Přidání nové třídy do složky služby](build-restful-apis-with-aspnet-web-api/_static/image15.png "přidání nové třídy do složky služby")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-227">![Adding a new class to the Services folder](build-restful-apis-with-aspnet-web-api/_static/image15.png "Adding a new class to the Services folder")</span></span>

    <span data-ttu-id="2a5f1-228">*Přidání nové třídy do složky služby*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-228">*Adding a new class to the Services folder*</span></span>
3. <span data-ttu-id="2a5f1-229">Když **přidat novou položku** se zobrazí dialogové okno, pojmenujte novou třídu **ContactRepository** a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-229">When the **Add New Item** dialog appears, name the new class **ContactRepository** and click **Add**.</span></span>

    <span data-ttu-id="2a5f1-230">![Vytváří se soubor třídy tak, aby obsahovala kód pro vrstvu služby úložiště kontakt](build-restful-apis-with-aspnet-web-api/_static/image16.png "vytváření souboru třídy tak, aby obsahovala kód pro vrstvu služby úložiště kontaktu")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-230">![Creating a class file to contain the code for the Contact Repository service layer](build-restful-apis-with-aspnet-web-api/_static/image16.png "Creating a class file to contain the code for the Contact Repository service layer")</span></span>

    <span data-ttu-id="2a5f1-231">*Vytváří se soubor třídy tak, aby obsahovala kód pro vrstvu služby úložiště kontaktu*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-231">*Creating a class file to contain the code for the Contact Repository service layer*</span></span>
4. <span data-ttu-id="2a5f1-232">Přidat using direktivu **ContactRepository.cs** soubor obsahoval obor názvů modelů.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-232">Add a using directive to the **ContactRepository.cs** file to include the models namespace.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. <span data-ttu-id="2a5f1-233">Přidejte následující zvýrazněný kód do **ContactRepository.cs** souboru o implementaci GetAllContacts metody.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-233">Add the following highlighted code to the **ContactRepository.cs** file to implement GetAllContacts method.</span></span>

    <span data-ttu-id="2a5f1-234">(Fragment - kódu *webové kontaktní úložiště API Lab – Ex01 -*)</span><span class="sxs-lookup"><span data-stu-id="2a5f1-234">(Code Snippet - *Web API Lab - Ex01 - Contact Repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. <span data-ttu-id="2a5f1-235">Otevřete **ContactController.cs** souboru, pokud již není otevřen.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-235">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="2a5f1-236">Přidejte následující příkaz using do části deklarace oboru názvů souboru.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-236">Add the following using statement to the namespace declaration section of the file.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. <span data-ttu-id="2a5f1-237">Přidejte následující zvýrazněný kód do **ContactController.cs** třídy přidat soukromé pole k reprezentaci instance úložiště, tak, aby zbývající část třídy členové mohou provádět pomocí implementace služby.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-237">Add the following highlighted code to the **ContactController.cs** class to add a private field to represent the instance of the repository, so that the rest of the class members can make use of the service implementation.</span></span>

    <span data-ttu-id="2a5f1-238">(Fragment - kódu *webové rozhraní API Lab – Ex01 - Contactcontroller*)</span><span class="sxs-lookup"><span data-stu-id="2a5f1-238">(Code Snippet - *Web API Lab - Ex01 - Contact Controller*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. <span data-ttu-id="2a5f1-239">Změnit **získat** metodu tak, že díky využívání služby Kontaktní úložiště.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-239">Change the **Get** method so that it makes use of the contact repository service.</span></span>

    <span data-ttu-id="2a5f1-240">(Fragment - kódu *webové rozhraní API testovacího prostředí – Ex01 - vrácení seznamu kontaktů prostřednictvím úložiště*)</span><span class="sxs-lookup"><span data-stu-id="2a5f1-240">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts via the repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. <span data-ttu-id="2a5f1-241">Přidejte zarážku na **ContactController**společnosti **získat** definice metody.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-241">Put a breakpoint on the **ContactController**'s **Get** method definition.</span></span>

   <span data-ttu-id="2a5f1-242">![Přidání zarážky contactcontroller](build-restful-apis-with-aspnet-web-api/_static/image17.png "přidání zarážky contactcontroller")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-242">![Adding breakpoints to the contact controller](build-restful-apis-with-aspnet-web-api/_static/image17.png "Adding breakpoints to the contact controller")</span></span>

   <span data-ttu-id="2a5f1-243">*Přidání zarážky contactcontroller*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-243">*Adding breakpoints to the contact controller*</span></span>
11. <span data-ttu-id="2a5f1-244">Stisknutím klávesy **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-244">Press **F5** to run the application.</span></span>
12. <span data-ttu-id="2a5f1-245">Když se otevře v prohlížeči, stiskněte klávesu **F12** otevřete vývojářské nástroje.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-245">When the browser opens, press **F12** to open the developer tools.</span></span>
13. <span data-ttu-id="2a5f1-246">Klikněte na tlačítko **sítě** kartu.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-246">Click the **Network** tab.</span></span>
14. <span data-ttu-id="2a5f1-247">Klikněte na tlačítko **spustit zachytávání** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-247">Click the **Start Capturing** button.</span></span>
15. <span data-ttu-id="2a5f1-248">Připojte k adrese URL do adresního řádku s příponou **/api/contact** a stiskněte klávesu **Enter** načtení kontroleru rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-248">Append the URL in the address bar with the suffix **/api/contact** and press **Enter** to load the API controller.</span></span>
16. <span data-ttu-id="2a5f1-249">Visual Studio 2012 by měl přerušení jednou **získat** metoda zahájí vykonávání.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-249">Visual Studio 2012 should break once **Get** method begins execution.</span></span>

   <span data-ttu-id="2a5f1-250">![Dopadem na dřívější kód v rámci metody Get](build-restful-apis-with-aspnet-web-api/_static/image18.png "dopadem na dřívější kód v rámci metody Get")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-250">![Breaking within the Get method](build-restful-apis-with-aspnet-web-api/_static/image18.png "Breaking within the Get method")</span></span>

   <span data-ttu-id="2a5f1-251">*Dopadem na dřívější kód v rámci metody Get*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-251">*Breaking within the Get method*</span></span>
17. <span data-ttu-id="2a5f1-252">Stisknutím klávesy **F5** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-252">Press **F5** to continue.</span></span>
18. <span data-ttu-id="2a5f1-253">Vraťte se do Internet Exploreru Pokud již není aktivní.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-253">Go back to Internet Explorer if it is not already in focus.</span></span> <span data-ttu-id="2a5f1-254">Poznámka: v okně zachytávání sítě.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-254">Note the network capture window.</span></span>

    <span data-ttu-id="2a5f1-255">![Sítě zobrazit v aplikaci Internet Explorer zobrazující výsledky volání webového rozhraní API](build-restful-apis-with-aspnet-web-api/_static/image19.png "sítě zobrazit v aplikaci Internet Explorer zobrazující výsledky volání webového rozhraní API")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-255">![Network view in Internet Explorer showing results of the Web API call](build-restful-apis-with-aspnet-web-api/_static/image19.png "Network view in Internet Explorer showing results of the Web API call")</span></span>

    <span data-ttu-id="2a5f1-256">*Zobrazení sítě v aplikaci Internet Explorer zobrazující výsledky volání webového rozhraní API*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-256">*Network view in Internet Explorer showing results of the Web API call*</span></span>
19. <span data-ttu-id="2a5f1-257">Klikněte na tlačítko **přejít na podrobnou zobrazení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-257">Click the **Go to detailed view** button.</span></span>
20. <span data-ttu-id="2a5f1-258">Klikněte na tlačítko **tělo odpovědi** kartu. Všimněte si, výstup JSON volání rozhraní API a jak ho reprezentuje dva kontakty načítána pro vrstvu služby.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-258">Click the **Response body** tab. Note the JSON output of the API call, and how it represents the two contacts retrieved by the service layer.</span></span>

    <span data-ttu-id="2a5f1-259">![Zobrazení výstupu JSON z webového rozhraní API v okně nástroje pro vývojáře](build-restful-apis-with-aspnet-web-api/_static/image20.png "zobrazení výstupu JSON z webového rozhraní API v okně nástroje pro vývojáře")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-259">![Viewing the JSON output from the Web API in the developer tools window](build-restful-apis-with-aspnet-web-api/_static/image20.png "Viewing the JSON output from the Web API in the developer tools window")</span></span>

    <span data-ttu-id="2a5f1-260">*Zobrazení výstupu JSON z webového rozhraní API v okně nástroje pro vývojáře*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-260">*Viewing the JSON output from the Web API in the developer tools window*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a><span data-ttu-id="2a5f1-261">Cvičení 2: Vytvoření webového rozhraní API pro čtení a zápis</span><span class="sxs-lookup"><span data-stu-id="2a5f1-261">Exercise 2: Create a Read/Write Web API</span></span>

<span data-ttu-id="2a5f1-262">V tomto cvičení budete implementovat POST a PUT metody ho povolit funkce pro úpravu dat kontaktujte správce.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-262">In this exercise, you will implement POST and PUT methods for the contact manager to enable it with data-editing features.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a><span data-ttu-id="2a5f1-263">Úloha 1 – otevření projektu webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="2a5f1-263">Task 1 - Opening the Web API Project</span></span>

<span data-ttu-id="2a5f1-264">V této úloze si připravit k vylepšení projekt webového rozhraní API vytvořené v cvičení 1 tak, aby mohl přijímat uživatelský vstup.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-264">In this task, you will prepare to enhance the Web API project created in Exercise 1 so that it can accept user input.</span></span>

1. <span data-ttu-id="2a5f1-265">Spustit **Visual Studio 2012 Express pro Web**, přejděte k **Start** a typ **VS Express for Web** stiskněte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-265">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="2a5f1-266">Otevřít **začít** řešení nachází v **zdroj/Ex02-ReadWriteWebAPI/počáteční/** složky.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-266">Open the **Begin** solution located at **Source/Ex02-ReadWriteWebAPI/Begin/** folder.</span></span> <span data-ttu-id="2a5f1-267">V opačném případě může nadále používat **End** řešení získat provedením předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-267">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="2a5f1-268">Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-268">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="2a5f1-269">Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-269">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="2a5f1-270">V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-270">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="2a5f1-271">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-271">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="2a5f1-272">Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-272">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="2a5f1-273">Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-273">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="2a5f1-274">To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-274">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="2a5f1-275">Otevřít **Services/ContactRepository.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-275">Open the **Services/ContactRepository.cs** file.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a><span data-ttu-id="2a5f1-276">Úloha 2 – Přidání funkce trvalosti dat pro implementaci kontaktní úložiště</span><span class="sxs-lookup"><span data-stu-id="2a5f1-276">Task 2 - Adding Data-Persistence Features to the Contact Repository Implementation</span></span>

<span data-ttu-id="2a5f1-277">V této úloze se rozšířit třídu ContactRepository projekt webového rozhraní API tak, aby ho zachovat a přijímat uživatelský vstup a nový kontakt instance vytvořené v cvičení 1.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-277">In this task, you will augment the ContactRepository class of the Web API project created in Exercise 1 so that it can persist and accept user input and new Contact instances.</span></span>

1. <span data-ttu-id="2a5f1-278">Přidejte následující konstantu pro **ContactRepository** třídy představující název je název webového serveru mezipaměti položky klíče dále v tomto cvičení.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-278">Add the following constant to the **ContactRepository** class to represent the name of the web server cache item key name later in this exercise.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. <span data-ttu-id="2a5f1-279">Přidejte konstruktor k **ContactRepository** obsahující následující kód.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-279">Add a constructor to the **ContactRepository** containing the following code.</span></span>

    <span data-ttu-id="2a5f1-280">(Fragment - kódu *webové konstruktor kontaktní úložiště API Lab – Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="2a5f1-280">(Code Snippet - *Web API Lab - Ex02 - Contact Repository Constructor*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. <span data-ttu-id="2a5f1-281">Upravte kód **GetAllContacts** způsob, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-281">Modify the code for the **GetAllContacts** method as demonstrated below.</span></span>

    <span data-ttu-id="2a5f1-282">(Fragment - kódu *webové rozhraní API Lab – Ex02 - Get All Contacts*)</span><span class="sxs-lookup"><span data-stu-id="2a5f1-282">(Code Snippet - *Web API Lab - Ex02 - Get All Contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="2a5f1-283">V tomto příkladu je pro demonstrační účely a použije mezipaměť webový server jako úložné médium, tak, aby hodnoty budou mít k dispozici víc klientů současně, místo použití mechanismus pro ukládání relace nebo životnost úložiště požadavku.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-283">This example is for demonstration purposes and will use the web server's cache as a storage medium, so that the values will be available to multiple clients simultaneously, rather than use a Session storage mechanism or a Request storage lifetime.</span></span> <span data-ttu-id="2a5f1-284">Entity Framework, XML úložiště nebo jiných různých jeden může použít místo mezipaměti webového serveru.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-284">One could use Entity Framework, XML storage, or any other variety in place of the web server cache.</span></span>
4. <span data-ttu-id="2a5f1-285">Implementovat novou metodu s názvem **SaveContact** k **ContactRepository** třídy práci uložit kontaktu.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-285">Implement a new method named **SaveContact** to the **ContactRepository** class to do the work of saving a contact.</span></span> <span data-ttu-id="2a5f1-286">**SaveContact** metoda by měla přijímat jeden **kontakt** parametry a návratovým logická hodnota označující úspěšné nebo neúspěšné.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-286">The **SaveContact** method should take a single **Contact** parameter and return a Boolean value indicating success or failure.</span></span>

    <span data-ttu-id="2a5f1-287">(Fragment - kódu *webové rozhraní API Lab – Ex02 – implementace metody SaveContact*)</span><span class="sxs-lookup"><span data-stu-id="2a5f1-287">(Code Snippet - *Web API Lab - Ex02 - Implementing the SaveContact Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a><span data-ttu-id="2a5f1-288">Cvičení 3: Využívají webové rozhraní API z klienta HTML</span><span class="sxs-lookup"><span data-stu-id="2a5f1-288">Exercise 3: Consume the Web API from an HTML Client</span></span>

<span data-ttu-id="2a5f1-289">V tomto cvičení vytvoříte klienta HTML do volání webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-289">In this exercise, you will create an HTML client to call the Web API.</span></span> <span data-ttu-id="2a5f1-290">Tento klient usnadní výměna dat s webovým rozhraním API prostřednictvím JavaScriptu a výsledky se zobrazí ve webovém prohlížeči pomocí značky jazyka HTML.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-290">This client will facilitate data exchange with the Web API using JavaScript and will display the results in a web browser using HTML markup.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a><span data-ttu-id="2a5f1-291">Úloha 1 - úprava zobrazení Index k poskytnutí grafické uživatelské rozhraní pro zobrazení Kontakty</span><span class="sxs-lookup"><span data-stu-id="2a5f1-291">Task 1 - Modifying the Index View to Provide a GUI for Displaying Contacts</span></span>

<span data-ttu-id="2a5f1-292">V této úloze budete upravovat výchozí Index zobrazení webové aplikaci, aby podporovala požadavky na zobrazení seznamu existující kontaktů v prohlížeče HTML.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-292">In this task, you will modify the default Index view of the web application to support the requirement of displaying the list of existing contacts in an HTML browser.</span></span>

1. <span data-ttu-id="2a5f1-293">Otevřete **Visual Studio 2012 Express pro Web** Pokud ještě není otevřený.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-293">Open **Visual Studio 2012 Express for Web** if it is not already open.</span></span>
2. <span data-ttu-id="2a5f1-294">Otevřít **začít** řešení nachází v **zdroj/Ex03-ConsumingWebAPI/počáteční/** složky.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-294">Open the **Begin** solution located at **Source/Ex03-ConsumingWebAPI/Begin/** folder.</span></span> <span data-ttu-id="2a5f1-295">V opačném případě může nadále používat **End** řešení získat provedením předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-295">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="2a5f1-296">Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-296">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="2a5f1-297">Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-297">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="2a5f1-298">V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-298">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="2a5f1-299">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-299">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="2a5f1-300">Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-300">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="2a5f1-301">Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-301">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="2a5f1-302">To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-302">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="2a5f1-303">Otevřít **Index.cshtml** se nachází v **zobrazení Domů** složky.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-303">Open the **Index.cshtml** file located at **Views/Home** folder.</span></span>
4. <span data-ttu-id="2a5f1-304">Nahraďte identifikátorem kódu HTML do elementu div **tělo** tak, aby vypadal jako v následujícím kódu.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-304">Replace the HTML code within the div element with id **body** so that it looks like the following code.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. <span data-ttu-id="2a5f1-305">Přidejte následující kód jazyka Javascript v dolní části souboru, který se provést požadavek HTTP do webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-305">Add the following Javascript code at the bottom of the file to perform the HTTP request to the Web API.</span></span>

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. <span data-ttu-id="2a5f1-306">Otevřete **ContactController.cs** souboru, pokud již není otevřen.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-306">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="2a5f1-307">Umístit zarážky na **získat** metodu **ContactController** třídy.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-307">Place a breakpoint on the **Get** method of the **ContactController** class.</span></span>

    <span data-ttu-id="2a5f1-308">![Umístění zarážky na metodu Get v kontroleru rozhraní API](build-restful-apis-with-aspnet-web-api/_static/image21.png "umístěním zarážky na metodu Get v kontroleru rozhraní API")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-308">![Placing a breakpoint on the Get method of the API controller](build-restful-apis-with-aspnet-web-api/_static/image21.png "Placing a breakpoint on the Get method of the API controller")</span></span>

    <span data-ttu-id="2a5f1-309">*Umístění zarážky na metodu Get v kontroleru rozhraní API*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-309">*Placing a breakpoint on the Get method of the API controller*</span></span>
8. <span data-ttu-id="2a5f1-310">Stisknutím klávesy **F5** spusťte projekt.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-310">Press **F5** to run the project.</span></span> <span data-ttu-id="2a5f1-311">Prohlížeč načte dokument HTML.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-311">The browser will load the HTML document.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2a5f1-312">Ujistěte se, že přecházíte kořenovou adresu URL vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-312">Ensure that you are browsing to the root URL of your application.</span></span>
9. <span data-ttu-id="2a5f1-313">Jakmile stránka načte a spustí jazyka JavaScript, bude dosaženo zarážkou a pozastaví provádění kódu v kontroleru.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-313">Once the page loads and the JavaScript executes, the breakpoint will be hit and the code execution will pause in the controller.</span></span>

    <span data-ttu-id="2a5f1-314">![Ladění do volání webového rozhraní API pomocí VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "ladění do volání webového rozhraní API pomocí VS Express for Web")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-314">![Debugging into the Web API calls using VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debugging into the Web API calls using VS Express for Web")</span></span>

    <span data-ttu-id="2a5f1-315">*Ladění do volání webového rozhraní API pomocí sady Visual Studio 2012 Express pro Web*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-315">*Debugging into the Web API call using Visual Studio 2012 Express for Web*</span></span>
10. <span data-ttu-id="2a5f1-316">Odebrat zarážku a stisknutím klávesy **F5** nebo ladění nástrojů **pokračovat** tlačítka pokračovat v načítání zobrazení v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-316">Remove the breakpoint and press **F5** or the debugging toolbar's **Continue** button to continue loading the view in the browser.</span></span> <span data-ttu-id="2a5f1-317">Po dokončení volání webového rozhraní API byste měli vidět kontakty vrácená z webového rozhraní API volat jako seznam položek v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-317">Once the Web API call completes you should see the contacts returned from the Web API call displayed as list items in the browser.</span></span>

    <span data-ttu-id="2a5f1-318">![Výsledky volání rozhraní API, zobrazí v prohlížeči jako položky seznamu](build-restful-apis-with-aspnet-web-api/_static/image23.png "výsledky volání rozhraní API, zobrazí v prohlížeči jako položky seznamu")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-318">![Results of the API call displayed in the browser as list items](build-restful-apis-with-aspnet-web-api/_static/image23.png "Results of the API call displayed in the browser as list items")</span></span>

    <span data-ttu-id="2a5f1-319">*Výsledky volání rozhraní API, zobrazí v prohlížeči jako položky seznamu*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-319">*Results of the API call displayed in the browser as list items*</span></span>
11. <span data-ttu-id="2a5f1-320">Zastavte ladění.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-320">Stop debugging.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a><span data-ttu-id="2a5f1-321">Úloha 2 - Úprava zobrazení Index k poskytnutí grafickým uživatelským rozhraním pro vytváření kontaktů</span><span class="sxs-lookup"><span data-stu-id="2a5f1-321">Task 2 - Modifying the Index View to Provide a GUI for Creating Contacts</span></span>

<span data-ttu-id="2a5f1-322">V této úloze budete i nadále upravovat zobrazení indexu aplikace MVC.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-322">In this task, you will continue to modify the Index view of the MVC application.</span></span> <span data-ttu-id="2a5f1-323">Formuláře bude přidán na stránku HTML, který bude zachycení uživatelského vstupu a odeslat do webového rozhraní API k vytvoření nového kontaktu a vytvoří se nové metody kontroleru webového rozhraní API shromažďovat data z grafického uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-323">A form will be added to the HTML page that will capture user input and send it to the Web API to create a new Contact, and a new Web API controller method will be created to collect date from the GUI.</span></span>

1. <span data-ttu-id="2a5f1-324">Otevřít **ContactController.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-324">Open the **ContactController.cs** file.</span></span>
2. <span data-ttu-id="2a5f1-325">Přidejte novou metodu s názvem třídy kontroleru **příspěvek** jak je znázorněno v následujícím kódu.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-325">Add a new method to the controller class named **Post** as shown in the following code.</span></span>

    <span data-ttu-id="2a5f1-326">(Fragment - kódu *webové rozhraní API Lab – Ex03 – Metoda Post*)</span><span class="sxs-lookup"><span data-stu-id="2a5f1-326">(Code Snippet - *Web API Lab - Ex03 - Post Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. <span data-ttu-id="2a5f1-327">Otevřete **Index.cshtml** souboru v sadě Visual Studio, pokud již není otevřen.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-327">Open the **Index.cshtml** file in Visual Studio if it is not already open.</span></span>
4. <span data-ttu-id="2a5f1-328">Přidejte následující kód HTML do souboru bezprostředně po Neseřazený seznam, který jste přidali v předchozím úkolu.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-328">Add the HTML code below to the file just after the unordered list you added in the previous task.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. <span data-ttu-id="2a5f1-329">V rámci elementu skript v dolní části dokumentu přidejte následující zvýrazněný kód pro zpracování události kliknutí na tlačítko, které bude publikovat data k webovému rozhraní API pomocí volání rozhraní HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-329">Within the script element at the bottom of the document, add the following highlighted code to handle button-click events, which will post the data to the Web API using an HTTP POST call.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. <span data-ttu-id="2a5f1-330">V **ContactController.cs**, umístit zarážky na **příspěvek** metody.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-330">In **ContactController.cs**, place a breakpoint on the **Post** method.</span></span>
7. <span data-ttu-id="2a5f1-331">Stisknutím klávesy **F5** ke spuštění aplikace v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-331">Press **F5** to run the application in the browser.</span></span>
8. <span data-ttu-id="2a5f1-332">Po načtení stránky v prohlížeči zadejte jméno nového kontaktu a Id a klikněte **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-332">Once the page is loaded in the browser, type in a new contact name and Id and click the **Save** button.</span></span>

    <span data-ttu-id="2a5f1-333">![Klient HTML dokumentu načítání do prohlížeče](build-restful-apis-with-aspnet-web-api/_static/image24.png "načíst dokument klienta HTML v prohlížeči")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-333">![The client HTML document loaded in the browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "The client HTML document loaded in the browser")</span></span>

    <span data-ttu-id="2a5f1-334">*Dokument HTML klienta načítání do prohlížeče*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-334">*The client HTML document loaded in the browser*</span></span>
9. <span data-ttu-id="2a5f1-335">Při přestane fungovat okně ladicího programu **příspěvek** způsob zobrazení vlastností **kontaktovat** parametr.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-335">When the debugger window breaks in the **Post** method, take a look at the properties of the **contact** parameter.</span></span> <span data-ttu-id="2a5f1-336">Hodnoty musí odpovídat data, která jste zadali ve formuláři.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-336">The values should match the data you entered in the form.</span></span>

    <span data-ttu-id="2a5f1-337">![Objekt kontakt odesílány do webového rozhraní API z klienta](build-restful-apis-with-aspnet-web-api/_static/image25.png "Contact objekt odesílány do webového rozhraní API z klienta")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-337">![The Contact object being sent to the Web API from the client](build-restful-apis-with-aspnet-web-api/_static/image25.png "The Contact object being sent to the Web API from the client")</span></span>

    <span data-ttu-id="2a5f1-338">*Objekt kontakt odesílány do webového rozhraní API z klienta*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-338">*The Contact object being sent to the Web API from the client*</span></span>
10. <span data-ttu-id="2a5f1-339">Krok prostřednictvím metody v ladicím programu, dokud **odpovědi** proměnná byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-339">Step through the method in the debugger until the **response** variable has been created.</span></span> <span data-ttu-id="2a5f1-340">Při kontrole v **lokální** okno v ladicím programu, uvidíte, že jsou nastavené všechny vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-340">Upon inspection in the **Locals** window in the debugger, you'll see that all the properties have been set.</span></span>

   <span data-ttu-id="2a5f1-341">![Odpověď po vytvoření v ladicím programu](build-restful-apis-with-aspnet-web-api/_static/image26.png "odpověď po vytvoření v ladicím programu")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-341">![The response following creation in the debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "The response following creation in the debugger")</span></span>

   <span data-ttu-id="2a5f1-342">*Odpověď po vytvoření v ladicím programu*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-342">*The response following creation in the debugger*</span></span>
11. <span data-ttu-id="2a5f1-343">Pokud stisknete **F5** nebo klikněte na tlačítko **pokračovat** v ladicím programu se požadavek dokončí.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-343">If you press **F5** or click **Continue** in the debugger the request will complete.</span></span> <span data-ttu-id="2a5f1-344">Když přepnete zpět do prohlížeče, nový kontakt je přidaný do seznamu kontaktů uložené **ContactRepository** implementace.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-344">Once you switch back to the browser, the new contact has been added to the list of contacts stored by the **ContactRepository** implementation.</span></span>

   <span data-ttu-id="2a5f1-345">![Po úspěšném vytvoření nové instance kontaktní odráží v prohlížeči](build-restful-apis-with-aspnet-web-api/_static/image27.png "odráží prohlížeče po úspěšném vytvoření nové instance kontaktu")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-345">![The browser reflects successful creation of the new contact instance](build-restful-apis-with-aspnet-web-api/_static/image27.png "The browser reflects successful creation of the new contact instance")</span></span>

   <span data-ttu-id="2a5f1-346">*Po úspěšném vytvoření nové instance kontaktní odráží v prohlížeči*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-346">*The browser reflects successful creation of the new contact instance*</span></span>

> [!NOTE]
> <span data-ttu-id="2a5f1-347">Kromě toho můžete tuto aplikaci nasadíte do Azure následujícím [příloha C: publikování aplikace ASP.NET MVC 4 pomocí nasazení webu](#AppendixC).</span><span class="sxs-lookup"><span data-stu-id="2a5f1-347">Additionally, you can deploy this application to Azure following [Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixC).</span></span>


* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="2a5f1-348">Souhrn</span><span class="sxs-lookup"><span data-stu-id="2a5f1-348">Summary</span></span>

<span data-ttu-id="2a5f1-349">Toto testovací prostředí představil novou architekturou ASP.NET Web API a provádění RESTful webová rozhraní API pomocí rozhraní.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-349">This lab has introduced you to the new ASP.NET Web API framework and to the implementation of RESTful Web APIs using the framework.</span></span> <span data-ttu-id="2a5f1-350">Z tohoto místa můžete vytvořit nové úložiště, která usnadňuje trvalost dat pomocí libovolného počtu mechanismy a nastavit tuto službu místo jednoduchých zadal jako příklad v tomto testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-350">From here, you could create a new repository that facilitates data persistence using any number of mechanisms and wire that service up rather than the simple one provided as an example in this lab.</span></span> <span data-ttu-id="2a5f1-351">Webové rozhraní API podporuje řadu dalších funkcí, jako jsou povolení komunikace z klientů jiného typu než HTML, které jsou napsané v libovolném jazyce, který podporuje protokol HTTP a JSON nebo XML.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-351">Web API supports a number of additional features, such as enabling communication from non-HTML clients written in any language that supports HTTP and JSON or XML.</span></span> <span data-ttu-id="2a5f1-352">Možnost hostování webového rozhraní API mimo typické webové aplikace je také možné, jakož i je schopnost vytvářet vlastní formáty serializace.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-352">The ability to host a Web API outside of a typical web application is also possible, as well as is the ability to create your own serialization formats.</span></span>

<span data-ttu-id="2a5f1-353">Webu technologie ASP.NET je vyhrazený pro rozhraní ASP.NET Web API v oblasti [ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="2a5f1-353">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span></span> <span data-ttu-id="2a5f1-354">Tento web i nadále bude poskytovat nejnovější informace, ukázky a zpráv týkající se webového rozhraní API, takže vrácení se často Pokud byste chtěli delve hlouběji do obrázky vytváření vlastních webových rozhraní API k dispozici na prakticky jakékoli zařízení nebo vývoj rozhraní framework.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-354">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="2a5f1-355">Příloha A: používání fragmentů kódu</span><span class="sxs-lookup"><span data-stu-id="2a5f1-355">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="2a5f1-356">Pomocí fragmentů kódu máte všechny kód, který je třeba na dosah ruky.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-356">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="2a5f1-357">Testovací prostředí dokumentu zjistíte přesně kdy je můžete využít, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-357">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="2a5f1-358">![Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu](build-restful-apis-with-aspnet-web-api/_static/image28.png "pomocí sady Visual Studio fragmenty kódu pro vložení kódu do projektu")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-358">![Using Visual Studio code snippets to insert code into your project](build-restful-apis-with-aspnet-web-api/_static/image28.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="2a5f1-359">*Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-359">*Using Visual Studio code snippets to insert code into your project*</span></span>

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a><span data-ttu-id="2a5f1-360">Chcete-li přidat fragment kódu pomocí klávesnice (C# jenom)</span><span class="sxs-lookup"><span data-stu-id="2a5f1-360">To add a code snippet using the keyboard (C# only)</span></span>

1. <span data-ttu-id="2a5f1-361">Umístěte kurzor, kam chcete vložit kód.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-361">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="2a5f1-362">Začněte psát název fragmentu kódu (bez mezer nebo pomlčky).</span><span class="sxs-lookup"><span data-stu-id="2a5f1-362">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="2a5f1-363">Podívejte se na jako IntelliSense zobrazí odpovídající názvy fragmenty kódu.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-363">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="2a5f1-364">Vyberte správný fragment kódu (nebo pokračujte v psaní dokud nebude vybraný celý fragment název).</span><span class="sxs-lookup"><span data-stu-id="2a5f1-364">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="2a5f1-365">Stiskněte klávesu Tabulátor dvakrát pro vložení fragmentu do umístění kurzoru.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-365">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

    <span data-ttu-id="2a5f1-366">![Začněte psát název fragmentu kódu](build-restful-apis-with-aspnet-web-api/_static/image29.png "začněte psát název fragmentu kódu")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-366">![Start typing the snippet name](build-restful-apis-with-aspnet-web-api/_static/image29.png "Start typing the snippet name")</span></span>

    <span data-ttu-id="2a5f1-367">*Začněte psát název fragmentu kódu*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-367">*Start typing the snippet name*</span></span>

    <span data-ttu-id="2a5f1-368">![Stiskněte klávesu Tab k vybrání fragmentu zvýrazněné](build-restful-apis-with-aspnet-web-api/_static/image30.png "stisknutím klávesy Tab k výběru zvýrazněné fragment kódu")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-368">![Press Tab to select the highlighted snippet](build-restful-apis-with-aspnet-web-api/_static/image30.png "Press Tab to select the highlighted snippet")</span></span>

    <span data-ttu-id="2a5f1-369">*Stiskněte klávesu Tab k výběru zvýrazněné fragment kódu*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-369">*Press Tab to select the highlighted snippet*</span></span>

    <span data-ttu-id="2a5f1-370">![Stisknutím klávesy Tab znovu a fragment kódu se rozbalí](build-restful-apis-with-aspnet-web-api/_static/image31.png "znovu stisknutím klávesy Tab a fragment kódu se rozbalí.")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-370">![Press Tab again and the snippet will expand](build-restful-apis-with-aspnet-web-api/_static/image31.png "Press Tab again and the snippet will expand")</span></span>

    <span data-ttu-id="2a5f1-371">*Stisknutím klávesy Tab znovu a fragment kódu se rozbalí.*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-371">*Press Tab again and the snippet will expand*</span></span>

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a><span data-ttu-id="2a5f1-372">Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)</span><span class="sxs-lookup"><span data-stu-id="2a5f1-372">To add a code snippet using the mouse (C#, Visual Basic and XML)</span></span>

1. <span data-ttu-id="2a5f1-373">Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-373">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="2a5f1-374">Vyberte **Vložit fragment** následovaný **Moje fragmenty kódu**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-374">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="2a5f1-375">Kliknutím na vyberte relevantní fragment kódu ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-375">Pick the relevant snippet from the list, by clicking on it.</span></span>

    <span data-ttu-id="2a5f1-376">![Klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment](build-restful-apis-with-aspnet-web-api/_static/image32.png "klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-376">![Right-click where you want to insert the code snippet and select Insert Snippet](build-restful-apis-with-aspnet-web-api/_static/image32.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

    <span data-ttu-id="2a5f1-377">*Klikněte pravým tlačítkem na, ve které chcete vložit fragment kódu a vyberte Vložit fragment*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-377">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

    <span data-ttu-id="2a5f1-378">![Vyberte si relevantní fragment kódu ze seznamu, kliknutím na](build-restful-apis-with-aspnet-web-api/_static/image33.png "relevantní fragment kódu ze seznamu vyberte kliknutím na")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-378">![Pick the relevant snippet from the list, by clicking on it](build-restful-apis-with-aspnet-web-api/_static/image33.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

    <span data-ttu-id="2a5f1-379">*Vyberte si relevantní fragment kódu ze seznamu, kliknutím na*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-379">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="2a5f1-380">Příloha B: instalaci sady Visual Studio Express 2012 pro Web</span><span class="sxs-lookup"><span data-stu-id="2a5f1-380">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="2a5f1-381">Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiném &quot;Express&quot; verzí pomocí **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-381">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="2a5f1-382">Postupujte podle následujících pokynů vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-382">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="2a5f1-383">Přejděte na [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="2a5f1-383">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="2a5f1-384">Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produkt &quot; <em>Visual Studio Express 2012 pro Web se sadou Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-384">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="2a5f1-385">Klikněte na **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-385">Click on **Install Now**.</span></span> <span data-ttu-id="2a5f1-386">Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-386">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="2a5f1-387">Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** spustit instalační program.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-387">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="2a5f1-388">![Instalace sady Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "instalace sady Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-388">![Install Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="2a5f1-389">*Instalace sady Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-389">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="2a5f1-390">Čtení všech produktů licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-390">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Přijetí podmínek licence](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    <span data-ttu-id="2a5f1-392">*Přijetí podmínek licence*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-392">*Accepting the license terms*</span></span>
5. <span data-ttu-id="2a5f1-393">Počkejte na dokončení procesu stahování a instalaci.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-393">Wait until the downloading and installation process completes.</span></span>

    ![Průběh instalace](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    <span data-ttu-id="2a5f1-395">*Průběh instalace*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-395">*Installation progress*</span></span>
6. <span data-ttu-id="2a5f1-396">Až instalace skončí, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-396">When the installation completes, click **Finish**.</span></span>

    ![Instalace byla dokončena.](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    <span data-ttu-id="2a5f1-398">*Instalace byla dokončena.*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-398">*Installation completed*</span></span>
7. <span data-ttu-id="2a5f1-399">Klikněte na tlačítko **ukončovací** zavřete instalačního programu webové platformy.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-399">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="2a5f1-400">Chcete-li spustit nástroj Visual Studio Express for Web, přejděte **Start** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express for Web** dlaždice.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-400">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web dlaždice](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    <span data-ttu-id="2a5f1-402">*VS Express for Web dlaždice*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-402">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="2a5f1-403">Příloha C: publikování aplikace ASP.NET MVC 4 pomocí nasazení webu</span><span class="sxs-lookup"><span data-stu-id="2a5f1-403">Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="2a5f1-404">Tento dodatek se ukazují, jak vytvořit nový web z portálu Azure Portal a publikovat aplikace, kterou jste získali podle testovacího prostředí, využít Webdeploy publikační funkci poskytovaný platformou Azure.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-404">This appendix will show you how to create a new web site from the Azure Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Azure.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a><span data-ttu-id="2a5f1-405">Úloha 1 – Vytvoření nového webu na webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2a5f1-405">Task 1 - Creating a New Web Site from the Azure Portal</span></span>

1. <span data-ttu-id="2a5f1-406">Přejděte [portálu pro správu Azure](https://manage.windowsazure.com/) a přihlaste se pomocí přihlašovacích údajů Microsoft spojených s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-406">Go to the [Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2a5f1-407">S Azure můžete zadarmo hostovat 10 webů ASP.NET a pak škálujte podle rozšiřujícího se provozu.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-407">With Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="2a5f1-408">Můžete se zaregistrovat [tady](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="2a5f1-408">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="2a5f1-409">![Přihlaste se k portálu Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image39.png "Přihlaste se k portálu Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-409">![Log on to Windows Azure portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="2a5f1-410">*Přihlaste se k portálu*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-410">*Log on to Portal*</span></span>
2. <span data-ttu-id="2a5f1-411">Klikněte na tlačítko **nový** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-411">Click **New** on the command bar.</span></span>

    <span data-ttu-id="2a5f1-412">![Vytvoření nového webu](build-restful-apis-with-aspnet-web-api/_static/image40.png "vytváření nového webu")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-412">![Creating a new Web Site](build-restful-apis-with-aspnet-web-api/_static/image40.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="2a5f1-413">*Vytvoření nového webu*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-413">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="2a5f1-414">Klikněte na tlačítko **Compute** | **webu**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-414">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="2a5f1-415">Potom vyberte **rychlé vytvoření** možnost.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-415">Then select **Quick Create** option.</span></span> <span data-ttu-id="2a5f1-416">Zadejte adresu URL k dispozici pro nový web a klikněte na tlačítko **vytvořit web**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-416">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2a5f1-417">Azure je hostitel pro webovou aplikaci spuštěnou v cloudu, který může řídit a spravovat.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-417">Azure is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="2a5f1-418">Možnost rychle vytvořit můžete nasadit hotové webové aplikace do Azure z mimo portál.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-418">The Quick Create option allows you to deploy a completed web application to the Azure from outside the portal.</span></span> <span data-ttu-id="2a5f1-419">Nezahrnuje kroky pro vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-419">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="2a5f1-420">![Vytvoření nového webu pomocí metody rychlého vytvoření](build-restful-apis-with-aspnet-web-api/_static/image41.png "vytváření nového webu pomocí metody rychlého vytvoření")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-420">![Creating a new Web Site using Quick Create](build-restful-apis-with-aspnet-web-api/_static/image41.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="2a5f1-421">*Vytvoření nového webu pomocí metody rychlého vytvoření*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-421">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="2a5f1-422">Počkejte, dokud nové **webu** se vytvoří.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-422">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="2a5f1-423">Po vytvoření webové stránky, klikněte na odkaz v části **URL** sloupce.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-423">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="2a5f1-424">Zkontrolujte, jestli funguje nový web.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-424">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="2a5f1-425">![Na nový web](build-restful-apis-with-aspnet-web-api/_static/image42.png "přechodu na nový web")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-425">![Browsing to the new web site](build-restful-apis-with-aspnet-web-api/_static/image42.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="2a5f1-426">*Procházení na nový web*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-426">*Browsing to the new web site*</span></span>

    <span data-ttu-id="2a5f1-427">![Spuštění webu](build-restful-apis-with-aspnet-web-api/_static/image43.png "spuštění webové stránky")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-427">![Web site running](build-restful-apis-with-aspnet-web-api/_static/image43.png "Web site running")</span></span>

    <span data-ttu-id="2a5f1-428">*Spuštění webové stránky*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-428">*Web site running*</span></span>
6. <span data-ttu-id="2a5f1-429">Přejděte zpět na portál a klikněte na název webové stránky v části **název** sloupec, který se zobrazí na stránkách správy.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-429">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="2a5f1-430">![Otevřete správu webových stránek](build-restful-apis-with-aspnet-web-api/_static/image44.png "otevřete správu webových stránek")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-430">![Opening the web site management pages](build-restful-apis-with-aspnet-web-api/_static/image44.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="2a5f1-431">*Otevřete správu webových stránek*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-431">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="2a5f1-432">V **řídicí panel** stránce v části **rychlý přehled** klikněte na tlačítko **stáhnout profil publikování** odkaz.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-432">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2a5f1-433">*Profil publikování* obsahuje všechny informace požadované pro publikování webových aplikací do Azure pro každou metodu povoleno publikování.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-433">The *publish profile* contains all of the information required to publish a web application to a Azure for each enabled publication method.</span></span> <span data-ttu-id="2a5f1-434">Profil publikování obsahuje adresy URL, přihlašovací údaje uživatele a databázové řetězce požadované k připojení a ověřování na základě jednotlivých koncových bodů, u kterých je povolena metoda publikace.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-434">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="2a5f1-435">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** a **Microsoft Visual Studio 2012** podporují čtení publikační profily k automatizaci konfigurace z těchto programů pro publikování webových aplikací do Azure.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-435">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Azure.</span></span>

    <span data-ttu-id="2a5f1-436">![Stahování webové stránky publikovat profil](build-restful-apis-with-aspnet-web-api/_static/image45.png "stahování webové stránky profil publikování")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-436">![Downloading the web site publish profile](build-restful-apis-with-aspnet-web-api/_static/image45.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="2a5f1-437">*Na webu stažení profilu publikování*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-437">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="2a5f1-438">Stáhněte si soubor profilu publikování do vhodného umístění.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-438">Download the publish profile file to a known location.</span></span> <span data-ttu-id="2a5f1-439">Dále v tomto cvičení uvidíte jak publikovat webovou aplikaci do Azure ze sady Visual Studio pomocí tohoto souboru.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-439">Further in this exercise you will see how to use this file to publish a web application to Azure from Visual Studio.</span></span>

    <span data-ttu-id="2a5f1-440">![Ukládání souboru profilu publikování](build-restful-apis-with-aspnet-web-api/_static/image46.png "ukládá se profil publikování")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-440">![Saving the publish profile file](build-restful-apis-with-aspnet-web-api/_static/image46.png "Saving the publish profile")</span></span>

    <span data-ttu-id="2a5f1-441">*Ukládá se profil publikování*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-441">*Saving the publish profile file*</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="2a5f1-442">Úloha 2 – konfigurování serveru databáze</span><span class="sxs-lookup"><span data-stu-id="2a5f1-442">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="2a5f1-443">Pokud vaše aplikace využívá SQL Server databáze, budete muset vytvořit server služby SQL Database.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-443">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="2a5f1-444">Pokud chcete nasadit jednoduchou aplikaci, která nepoužívá SQL Server může tuto úlohu přeskočit.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-444">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="2a5f1-445">Pro uložení databáze aplikace budete potřebovat databázi SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-445">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="2a5f1-446">Servery SQL Database můžete zobrazit ze svého předplatného na portálu Azure Management portal na **databází Sql** | **servery** | **řídicího panelu serveru**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-446">You can view the SQL Database servers from your subscription in the Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="2a5f1-447">Pokud nemáte server vytvořili, můžete vytvořit jednu **přidat** tlačítko na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-447">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="2a5f1-448">Poznamenejte si **název serveru a adresu URL, správce přihlašovací jméno a heslo**, jak je použijete v další úkoly.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-448">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="2a5f1-449">Nevytvářet databáze, protože vytvoří se v pozdější fázi.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-449">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="2a5f1-450">![Řídicí panel serveru SQL Database](build-restful-apis-with-aspnet-web-api/_static/image47.png "řídicího panelu serveru SQL Database")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-450">![SQL Database Server Dashboard](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="2a5f1-451">*Řídicí panel serveru SQL Database*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-451">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="2a5f1-452">V dalším úkolem budete testovat připojení k databázi ze sady Visual Studio z tohoto důvodu je nutné zahrnout místní IP adresa serveru seznamu **povolené IP adresy**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-452">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="2a5f1-453">Chcete-li to mohli udělat, klikněte na tlačítko **konfigurovat**, vyberte IP adresu z **aktuální IP adresa klienta** a vložte ho na **počáteční IP adresa** a **Koncová IP adresa** textová pole a klikněte na tlačítko ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-453">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) button.</span></span>

    ![Přidat IP adresu klienta](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    <span data-ttu-id="2a5f1-455">*Přidat IP adresu klienta*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-455">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="2a5f1-456">Jednou **IP adresa klienta** je přidat do povolených IP adres klikněte na tlačítko na **Uložit** potvrďte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-456">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Potvrzení změn](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    <span data-ttu-id="2a5f1-458">*Potvrzení změn*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-458">*Confirm Changes*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="2a5f1-459">Úloha 3 – publikování aplikace ASP.NET MVC 4 pomocí nasazení webu</span><span class="sxs-lookup"><span data-stu-id="2a5f1-459">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="2a5f1-460">Vraťte se do řešení ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-460">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="2a5f1-461">V **Průzkumníka řešení**, klikněte pravým tlačítkem na webový projekt a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-461">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="2a5f1-462">![Publikování aplikace](build-restful-apis-with-aspnet-web-api/_static/image51.png "publikování aplikace")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-462">![Publishing the Application](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publishing the Application")</span></span>

    <span data-ttu-id="2a5f1-463">*Publikování na webu*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-463">*Publishing the web site*</span></span>
2. <span data-ttu-id="2a5f1-464">Importujte profil publikování, který jste uložili v první úloze.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-464">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="2a5f1-465">![Import profilu publikování](build-restful-apis-with-aspnet-web-api/_static/image52.png "import profilu publikování")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-465">![Importing the publish profile](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importing the publish profile")</span></span>

    <span data-ttu-id="2a5f1-466">*Import publikačního profilu*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-466">*Importing publish profile*</span></span>
3. <span data-ttu-id="2a5f1-467">Klikněte na tlačítko **ověřit připojení**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-467">Click **Validate Connection**.</span></span> <span data-ttu-id="2a5f1-468">Po dokončení ověření klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-468">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2a5f1-469">Ověření bylo dokončeno, jakmile se zobrazí zelené zaškrtnutí vedle tlačítka ověřit připojení.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-469">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="2a5f1-470">![Ověřuje se připojení](build-restful-apis-with-aspnet-web-api/_static/image53.png "ověřuje se připojení")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-470">![Validating connection](build-restful-apis-with-aspnet-web-api/_static/image53.png "Validating connection")</span></span>

    <span data-ttu-id="2a5f1-471">*Ověřuje se připojení*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-471">*Validating connection*</span></span>
4. <span data-ttu-id="2a5f1-472">V **nastavení** stránce v části **databází** klikněte na tlačítko vedle textového pole připojení k databázi (to znamená **objekt DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="2a5f1-472">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="2a5f1-473">![Konfigurace nasazení webu](build-restful-apis-with-aspnet-web-api/_static/image54.png "konfigurace nasazení webu")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-473">![Web deploy configuration](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web deploy configuration")</span></span>

    <span data-ttu-id="2a5f1-474">*Konfigurace nasazení webu*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-474">*Web deploy configuration*</span></span>
5. <span data-ttu-id="2a5f1-475">Konfigurace připojení k databázi následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2a5f1-475">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="2a5f1-476">V **název serveru** zadejte vaše pomocí adresy URL databáze SQL serveru *tcp:* předponu.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-476">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="2a5f1-477">V **uživatelské jméno** zadejte vaše přihlašovací jméno správce serveru.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-477">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="2a5f1-478">V **heslo** zadejte heslo pro přihlašovací jméno správce serveru.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-478">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="2a5f1-479">Zadejte nový název databáze, například: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-479">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="2a5f1-480">![Konfigurace cílový připojovací řetězec](build-restful-apis-with-aspnet-web-api/_static/image55.png "konfigurace cílový připojovací řetězec")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-480">![Configuring destination connection string](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="2a5f1-481">*Konfigurace cílový připojovací řetězec*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-481">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="2a5f1-482">Pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-482">Then click **OK**.</span></span> <span data-ttu-id="2a5f1-483">Po zobrazení výzvy k vytvoření databáze klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-483">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="2a5f1-484">![Vytvoření databáze](build-restful-apis-with-aspnet-web-api/_static/image56.png "vytvoření řetězce databáze")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-484">![Creating the database](build-restful-apis-with-aspnet-web-api/_static/image56.png "Creating the database string")</span></span>

    <span data-ttu-id="2a5f1-485">*Vytvoření databáze*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-485">*Creating the database*</span></span>
7. <span data-ttu-id="2a5f1-486">Připojovací řetězec, který budete používat pro připojení k databázi SQL ve službě Windows Azure se zobrazí v textovém poli výchozí připojení.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-486">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="2a5f1-487">Pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-487">Then click **Next**.</span></span>

    <span data-ttu-id="2a5f1-488">![Připojovací řetězec odkazující na SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "připojovací řetězec odkazující na SQL Database")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-488">![Connection string pointing to SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="2a5f1-489">*Připojovací řetězec odkazující na SQL Database*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-489">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="2a5f1-490">V **ve verzi Preview** klikněte na **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-490">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="2a5f1-491">![Publikování webové aplikace](build-restful-apis-with-aspnet-web-api/_static/image58.png "publikování webové aplikace")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-491">![Publishing the web application](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publishing the web application")</span></span>

    <span data-ttu-id="2a5f1-492">*Publikování webové aplikace*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-492">*Publishing the web application*</span></span>
9. <span data-ttu-id="2a5f1-493">Až se proces publikování dokončí, otevře se váš výchozí prohlížeč publikovaného webu.</span><span class="sxs-lookup"><span data-stu-id="2a5f1-493">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="2a5f1-494">![Publikování aplikace do Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "publikování aplikace do Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="2a5f1-494">![Application published to Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="2a5f1-495">*Aplikace publikovaná do Azure*</span><span class="sxs-lookup"><span data-stu-id="2a5f1-495">*Application published to Azure*</span></span>
