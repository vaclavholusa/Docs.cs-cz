---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: "Začínáme s OWIN a Katana | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/27/2013
ms.topic: article
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 8922aada723da9b149ec111902fcd883c8241dfb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-owin-and-katana"></a><span data-ttu-id="b8ecf-102">Začínáme s OWIN a Katana</span><span class="sxs-lookup"><span data-stu-id="b8ecf-102">Getting Started with OWIN and Katana</span></span>
====================
<span data-ttu-id="b8ecf-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b8ecf-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="b8ecf-104">[Otevřete Web Interface pro .NET (OWIN)](http://owin.org/) definuje abstrakci mezi .NET webové servery a webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-104">[Open Web Interface for .NET (OWIN)](http://owin.org/) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="b8ecf-105">Protože webový server z aplikace, OWIN usnadňuje vytvoření middleware pro vývoj webů .NET.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-105">By decoupling the web server from the application, OWIN makes it easier to create middleware for .NET web development.</span></span> <span data-ttu-id="b8ecf-106">Navíc OWIN usnadňuje port webové aplikace k jiným hostitelům &#8212; například vlastní hostování služby systému Windows nebo jiný proces.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-106">Also, OWIN makes it easier to port web applications to other hosts&#8212;for example, self-hosting in a Windows service or other process.</span></span>

<span data-ttu-id="b8ecf-107">OWIN je specifikace vlastněných komunity, není implementace.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-107">OWIN is a community-owned specification, not an implementation.</span></span> <span data-ttu-id="b8ecf-108">Projekt Katana je sada open-source OWIN součásti vyvinuté společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-108">The Katana project is a set of open-source OWIN components developed by Microsoft.</span></span> <span data-ttu-id="b8ecf-109">Obecné přehled OWIN a Katana, najdete v tématu [Přehled projektu Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="b8ecf-109">For a general overview of both OWIN and Katana, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="b8ecf-110">V tomto článku I se pustíme přímo do kódu začít pracovat.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-110">In this article, I will jump right into code to get started.</span></span>

<span data-ttu-id="b8ecf-111">Tento kurz používá [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), ale můžete také použít Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-111">This tutorial uses [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), but you can also use Visual Studio 2012.</span></span> <span data-ttu-id="b8ecf-112">Pár kroků se liší v sadě Visual Studio 2012, který beru na vědomí následující.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-112">A few of the steps are different in Visual Studio 2012, which I note below.</span></span>

## <a name="host-owin-in-iis"></a><span data-ttu-id="b8ecf-113">Hostitele OWIN ve službě IIS</span><span class="sxs-lookup"><span data-stu-id="b8ecf-113">Host OWIN in IIS</span></span>

<span data-ttu-id="b8ecf-114">V této části Pořádáme OWIN ve službě IIS.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-114">In this section, we'll host OWIN in IIS.</span></span> <span data-ttu-id="b8ecf-115">Tato možnost vám poskytuje flexibilitu a composability kanálu OWIN společně s sadu vyspělé funkce služby IIS.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-115">This option gives you the flexibility and composability of an OWIN pipeline together with the mature feature set of IIS.</span></span> <span data-ttu-id="b8ecf-116">Použití této možnosti se aplikace OWIN spouští kanál požadavku.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-116">Using this option, the OWIN application runs in the ASP.NET request pipeline.</span></span>

<span data-ttu-id="b8ecf-117">Nejdřív vytvořte nový projekt webové aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-117">First, create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="b8ecf-118">(V sadě Visual Studio 2012, použijte typ projektu prázdný webové aplikace ASP.NET.)</span><span class="sxs-lookup"><span data-stu-id="b8ecf-118">(In Visual Studio 2012, use the ASP.NET Empty Web Application project type.)</span></span>

![](getting-started-with-owin-and-katana/_static/image1.png)

<span data-ttu-id="b8ecf-119">V **nový projekt ASP.NET** dialogovém okně, vyberte **prázdný** šablony.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-119">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span>

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a><span data-ttu-id="b8ecf-120">Přidání balíčků NuGet</span><span class="sxs-lookup"><span data-stu-id="b8ecf-120">Add NuGet Packages</span></span>

<span data-ttu-id="b8ecf-121">Dál přidejte požadované balíčky NuGet.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-121">Next, add the required NuGet packages.</span></span> <span data-ttu-id="b8ecf-122">Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**, pak vyberte **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-122">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="b8ecf-123">V okně konzoly Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b8ecf-123">In the Package Manager Console window, type the following command:</span></span>

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a><span data-ttu-id="b8ecf-124">Přidání třídy spuštění</span><span class="sxs-lookup"><span data-stu-id="b8ecf-124">Add a Startup Class</span></span>

<span data-ttu-id="b8ecf-125">Dál přidejte třídy pro spuštění OWIN.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-125">Next, add an OWIN startup class.</span></span> <span data-ttu-id="b8ecf-126">V Průzkumníku řešení klikněte pravým tlačítkem na projekt a vyberte **přidat**, pak vyberte **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-126">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span> <span data-ttu-id="b8ecf-127">V **přidat novou položku** dialogovém okně, vyberte **třídy pro spuštění Owin**.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-127">In the **Add New Item** dialog, select **Owin Startup class**.</span></span> <span data-ttu-id="b8ecf-128">Další informace o konfiguraci třída při spuštění najdete v tématu [OWIN při spuštění třída detekce](owin-startup-class-detection.md).</span><span class="sxs-lookup"><span data-stu-id="b8ecf-128">For more info on configuring the startup class, see [OWIN Startup Class Detection](owin-startup-class-detection.md).</span></span>

![](getting-started-with-owin-and-katana/_static/image4.png)

<span data-ttu-id="b8ecf-129">Přidejte následující kód, který `Startup1.Configuration` metoda:</span><span class="sxs-lookup"><span data-stu-id="b8ecf-129">Add the following code to the `Startup1.Configuration` method:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

<span data-ttu-id="b8ecf-130">Tento kód přidá do kanálu OWIN, implementovaný jako funkci, která přijímá jednoduché druhem middleware **Microsoft.Owin.IOwinContext** instance.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-130">This code adds a simple piece of middleware to the OWIN pipeline, implemented as a function that receives a **Microsoft.Owin.IOwinContext** instance.</span></span> <span data-ttu-id="b8ecf-131">Když server obdrží požadavek HTTP, vyvolá kanálu OWIN middleware.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-131">When the server receives an HTTP request, the OWIN pipeline invokes the middleware.</span></span> <span data-ttu-id="b8ecf-132">Middleware nastaví typ obsahu pro odpověď a zapíše text odpovědi.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-132">The middleware sets the content type for the response and writes the response body.</span></span>

> [!NOTE]
> <span data-ttu-id="b8ecf-133">Šablona třídy OWIN při spuštění je k dispozici v sadě Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-133">The OWIN Startup class template is available in Visual Studio 2013.</span></span> <span data-ttu-id="b8ecf-134">Pokud používáte Visual Studio 2012, stačí přidat nový prázdný třídy s názvem `Startup1`a vložte následující kód:</span><span class="sxs-lookup"><span data-stu-id="b8ecf-134">If you are using Visual Studio 2012, just add a new empty class named `Startup1`, and paste in the following code:</span></span>


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a><span data-ttu-id="b8ecf-135">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="b8ecf-135">Run the Application</span></span>

<span data-ttu-id="b8ecf-136">Stisknutím klávesy F5 spusťte ladění.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-136">Press F5 to begin debugging.</span></span> <span data-ttu-id="b8ecf-137">Visual Studio se otevře okno prohlížeče a `http://localhost:*port*/`.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-137">Visual Studio will open a browser window to `http://localhost:*port*/`.</span></span> <span data-ttu-id="b8ecf-138">Stránce by měl vypadat asi takto:</span><span class="sxs-lookup"><span data-stu-id="b8ecf-138">The page should look like the following:</span></span>

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a><span data-ttu-id="b8ecf-139">Vlastní hostování OWIN v konzolové aplikaci</span><span class="sxs-lookup"><span data-stu-id="b8ecf-139">Self-Host OWIN in a Console Application</span></span>

<span data-ttu-id="b8ecf-140">Je snadné převést hostování IIS pro vlastní hostování v vlastní proces, který tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-140">It's easy to convert this application from IIS hosting to self-hosting in a custom process.</span></span> <span data-ttu-id="b8ecf-141">S hostování IIS IIS funguje jako HTTP server a jako proces tohoto hostitele k serveru.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-141">With IIS hosting, IIS acts as both the HTTP server and as the process that host the sever.</span></span> <span data-ttu-id="b8ecf-142">S vlastní hostování, vaše aplikace vytvoří proces a použije **HttpListener** třída jako HTTP server.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-142">With self-hosting, your application creates the process and uses the **HttpListener** class as the HTTP server.</span></span>

<span data-ttu-id="b8ecf-143">V sadě Visual Studio vytvořte novou konzolovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-143">In Visual Studio, create a new console application.</span></span> <span data-ttu-id="b8ecf-144">V okně konzoly Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b8ecf-144">In the Package Manager Console window, type the following command:</span></span>

`Install-Package Microsoft.Owin.SelfHost -Pre`

<span data-ttu-id="b8ecf-145">Přidat `Startup1` třídy z část 1 v tomto kurzu do projektu.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-145">Add a `Startup1` class from part 1 of this tutorial to the project.</span></span> <span data-ttu-id="b8ecf-146">Nemusíte upravovat tuto třídu.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-146">You don't need to modify this class.</span></span>

<span data-ttu-id="b8ecf-147">Implementovat aplikace `Main` metoda následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-147">Implement the application's `Main` method as follows.</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

<span data-ttu-id="b8ecf-148">Když spustíte aplikaci konzoly, server se spustí naslouchání `http://localhost:9000`.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-148">When you run the console application, the server starts listening to `http://localhost:9000`.</span></span> <span data-ttu-id="b8ecf-149">Pokud přejdete na tuto adresu ve webovém prohlížeči, zobrazí se stránka "Hello, world".</span><span class="sxs-lookup"><span data-stu-id="b8ecf-149">If you navigate to this address in a web browser, you will see the "Hello world" page.</span></span>

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a><span data-ttu-id="b8ecf-150">Přidat OWIN Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-150">Add OWIN Diagnostics</span></span>

<span data-ttu-id="b8ecf-151">Balíček Microsoft.Owin.Diagnostics obsahuje middleware, který zachycuje neošetřené výjimky a zobrazí stránku HTML s podrobnosti o chybě.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-151">The Microsoft.Owin.Diagnostics package contains middleware that catches unhandled exceptions and displays an HTML page with error details.</span></span> <span data-ttu-id="b8ecf-152">Tato stránka funguje jako mnohem chybová stránka technologie ASP.NET, která se někdy označuje jako "[žlutý obrazovka smrti](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span><span class="sxs-lookup"><span data-stu-id="b8ecf-152">This page functions much like the ASP.NET error page that is sometimes called the "[yellow screen of death](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span></span> <span data-ttu-id="b8ecf-153">Podobně jako YSOD chybová stránka Katana je užitečné při vývoji, ale je dobrým zvykem zakázat v produkčním režimu.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-153">Like the YSOD, the Katana error page is useful during development, but it's a good practice to disable it in production mode.</span></span>

<span data-ttu-id="b8ecf-154">K instalaci balíčku diagnostiky ve vašem projektu, zadejte následující příkaz v okně konzoly Správce balíčků:</span><span class="sxs-lookup"><span data-stu-id="b8ecf-154">To install the Diagnostics package in your project, type the following command in the Package Manager Console window:</span></span>

`install-package Microsoft.Owin.Diagnostics –Pre`

<span data-ttu-id="b8ecf-155">Změnit kód ve vaší `Startup1.Configuration` metoda následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="b8ecf-155">Change the code in your `Startup1.Configuration` method as follows:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

<span data-ttu-id="b8ecf-156">Teď použijte CTRL + F5 a spusťte aplikaci bez ladění, tak, aby Visual Studio nebude rozdělit na výjimku.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-156">Now use CTRL+F5 to run the application without debugging, so that Visual Studio will not break on the exception.</span></span> <span data-ttu-id="b8ecf-157">Aplikace se chová stejně jako předtím, dokud přejděte na `http://localhost/fail`, na který bod aplikace vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-157">The application behaves the same as before, until you navigate to `http://localhost/fail`, at which point the application throws the exception.</span></span> <span data-ttu-id="b8ecf-158">Middleware chybové stránky se zachycení výjimky a zobrazit stránku HTML s informace o této chybě.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-158">The error page middleware will catch the exception and display an HTML page with information about the error.</span></span> <span data-ttu-id="b8ecf-159">Kliknutím na karty zobrazíte zásobníku, řetězec dotazu, soubory cookie, hlavička požadavku a proměnných prostředí OWIN.</span><span class="sxs-lookup"><span data-stu-id="b8ecf-159">You can click the tabs to see the stack, query string, cookies, request header, and OWIN environment variables.</span></span>

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a><span data-ttu-id="b8ecf-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b8ecf-160">Next Steps</span></span>

- [<span data-ttu-id="b8ecf-161">Detekce třídy pro spuštění OWIN</span><span class="sxs-lookup"><span data-stu-id="b8ecf-161">OWIN Startup Class Detection</span></span>](owin-startup-class-detection.md)
- [<span data-ttu-id="b8ecf-162">Použít k hostování na vlastním rozhraní ASP.NET Web API OWIN</span><span class="sxs-lookup"><span data-stu-id="b8ecf-162">Use OWIN to Self-Host ASP.NET Web API</span></span>](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [<span data-ttu-id="b8ecf-163">Hostování na vlastním SignalR pomocí OWIN</span><span class="sxs-lookup"><span data-stu-id="b8ecf-163">Use OWIN to Self-Host SignalR</span></span>](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
