---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Kurz: SignalR hostování na vlastním serveru | Microsoft Docs'
author: pfletcher
description: 'Tento kurz ukazuje, jak vytvořit vlastním hostováním SignalR 2: server a jak se připojit k němu pomocí jazyka JavaScript klienta. Použité v tomto kurzu V softwaru verze...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: d38e6fbc3407e4beca6942bbdefcaa8258ebc5ad
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036775"
---
<a name="tutorial-signalr-self-host"></a><span data-ttu-id="4a3b8-104">Kurz: SignalR hostování na vlastním serveru</span><span class="sxs-lookup"><span data-stu-id="4a3b8-104">Tutorial: SignalR Self-Host</span></span>
====================
<span data-ttu-id="4a3b8-105">podle [Patrik Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="4a3b8-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="4a3b8-106">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="4a3b8-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> <span data-ttu-id="4a3b8-107">Tento kurz ukazuje, jak vytvořit vlastním hostováním SignalR 2: server a jak se připojit k němu pomocí jazyka JavaScript klienta.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-107">This tutorial shows how to create a self-hosted SignalR 2 server, and how to connect to it with a JavaScript client.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="4a3b8-108">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="4a3b8-108">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="4a3b8-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="4a3b8-109">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="4a3b8-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="4a3b8-110">.NET 4.5</span></span>
> - <span data-ttu-id="4a3b8-111">SignalR verze 2</span><span class="sxs-lookup"><span data-stu-id="4a3b8-111">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="4a3b8-112">Tento kurz pomocí sady Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="4a3b8-112">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="4a3b8-113">Pokud chcete používat Visual Studio 2012 v tomto kurzu, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="4a3b8-113">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="4a3b8-114">Aktualizace vašeho [Správce balíčků](http://docs.nuget.org/docs/start-here/installing-nuget) na nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-114">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="4a3b8-115">Nainstalujte [webové platformy](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="4a3b8-115">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="4a3b8-116">Ve webové platformy, vyhledání a instalace **ASP.NET a webové nástroje 2013.1 pro sadu Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-116">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="4a3b8-117">Šablony sady Visual Studio pro třídy SignalR dojde k instalaci, jako **rozbočovače**.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-117">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="4a3b8-118">Některé šablony (jako například **třídy pro spuštění OWIN**) nebudou k dispozici; pro tyto třídy soubor použít místo.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-118">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="4a3b8-119">Dotazy a připomínky</span><span class="sxs-lookup"><span data-stu-id="4a3b8-119">Questions and comments</span></span>
> 
> <span data-ttu-id="4a3b8-120">Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-120">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="4a3b8-121">Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="4a3b8-121">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="4a3b8-122">Přehled</span><span class="sxs-lookup"><span data-stu-id="4a3b8-122">Overview</span></span>

<span data-ttu-id="4a3b8-123">SignalR server je obvykle hostované v aplikaci ASP.NET ve službě IIS, ale může být také vlastním hostováním (například v konzolové aplikace nebo služba systému Windows) pomocí knihovny hostování na vlastním serveru.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-123">A SignalR server is usually hosted in an ASP.NET application in IIS, but it can also be self-hosted (such as in a console application or Windows service) using the self-host library.</span></span> <span data-ttu-id="4a3b8-124">Tuto knihovnu, jako jsou všechny funkce SignalR 2, je založená na OWIN ([Open Web Interface pro .NET](http://owin.org)).</span><span class="sxs-lookup"><span data-stu-id="4a3b8-124">This library, like all of SignalR 2, is built on OWIN ([Open Web Interface for .NET](http://owin.org)).</span></span> <span data-ttu-id="4a3b8-125">OWIN definuje abstrakci mezi .NET webové servery a webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-125">OWIN defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="4a3b8-126">OWIN oddělí webové aplikace ze serveru, takže je ideální pro samoobslužné webové aplikace v vlastní proces mimo službu IIS hostování OWIN.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-126">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>

<span data-ttu-id="4a3b8-127">Pro hostování není ve službě IIS z těchto důvodů:</span><span class="sxs-lookup"><span data-stu-id="4a3b8-127">Reasons for not hosting in IIS include:</span></span>

- <span data-ttu-id="4a3b8-128">Prostředí, kde služby IIS není k dispozici nebo žádoucí, jako je například existující serverové farmy bez služby IIS.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-128">Environments where IIS is not available or desirable, such as an existing server farm without IIS.</span></span>
- <span data-ttu-id="4a3b8-129">Nároky na výkon služby IIS, je potřeba se vyhnout.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-129">The performance overhead of IIS needs to be avoided.</span></span>
- <span data-ttu-id="4a3b8-130">Funkce SignalR je přidat do aplikace exising, která běží v služby systému Windows, role pracovního procesu Azure nebo jiný proces.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-130">SignalR functionality is to be added to an exising application that runs in a Windows Service, Azure worker role, or other process.</span></span>

<span data-ttu-id="4a3b8-131">Pokud řešení je vyvíjen jako hostování na vlastním z důvodů výkonu, se doporučuje také testovací aplikace hostované ve službě IIS k určení výhody výkonu.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-131">If a solution is being developed as self-host for performance reasons, it's recommended to also test the application hosted in IIS to determine the performance benefit.</span></span>

<span data-ttu-id="4a3b8-132">Tento kurz obsahuje následující části:</span><span class="sxs-lookup"><span data-stu-id="4a3b8-132">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="4a3b8-133">Vytvoření serveru</span><span class="sxs-lookup"><span data-stu-id="4a3b8-133">Creating the server</span></span>](#server)
- [<span data-ttu-id="4a3b8-134">Přístup k serveru pomocí klienta JavaScript</span><span class="sxs-lookup"><span data-stu-id="4a3b8-134">Accessing the server with a JavaScript client</span></span>](#js)

<a id="server"></a>

## <a name="creating-the-server"></a><span data-ttu-id="4a3b8-135">Vytvoření serveru</span><span class="sxs-lookup"><span data-stu-id="4a3b8-135">Creating the server</span></span>

<span data-ttu-id="4a3b8-136">V tomto kurzu vytvoříte serveru, který je hostován v konzolové aplikaci, ale server je možné hostovat v žádné řazení procesu, jako je služba systému Windows nebo role pracovního procesu Azure.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-136">In this tutorial, you'll create a server that's hosted in a console application, but the server can be hosted in any sort of process, such as a Windows service or Azure worker role.</span></span> <span data-ttu-id="4a3b8-137">Ukázkový kód pro hostování serveru SignalR ve službě Windows, najdete v části [Self-Hosting SignalR ve službě Windows](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span><span class="sxs-lookup"><span data-stu-id="4a3b8-137">For sample code for hosting a SignalR server in a Windows Service, see [Self-Hosting SignalR in a Windows Service](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span></span>

1. <span data-ttu-id="4a3b8-138">Otevřete Visual Studio 2013 s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-138">Open Visual Studio 2013 with administrator privileges.</span></span> <span data-ttu-id="4a3b8-139">Vyberte **soubor**, **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-139">Select **File**, **New Project**.</span></span> <span data-ttu-id="4a3b8-140">Vyberte **Windows** pod **Visual C#** uzel v **šablony** panelu a vyberte **konzolové aplikace** šablony.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-140">Select **Windows** under the **Visual C#** node in the **Templates** pane, and select the **Console Application** template.</span></span> <span data-ttu-id="4a3b8-141">Název nového projektu "SignalRSelfHost" a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-141">Name the new project "SignalRSelfHost" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image1.png)
2. <span data-ttu-id="4a3b8-142">Otevření konzoly Správce balíčků knihoven výběrem **nástroje**, **Správce balíčků knihoven**, **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-142">Open the library package manager console by selecting **Tools**, **Library Package Manager**, **Package Manager Console**.</span></span>
3. <span data-ttu-id="4a3b8-143">V konzole správce balíčku zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4a3b8-143">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    <span data-ttu-id="4a3b8-144">Tento příkaz přidá do projektu knihovny SignalR 2 Self-Host.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-144">This command adds the SignalR 2 Self-Host libraries to the project.</span></span>
4. <span data-ttu-id="4a3b8-145">V konzole správce balíčku zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4a3b8-145">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    <span data-ttu-id="4a3b8-146">Tento příkaz přidá do projektu knihovny Microsoft.Owin.Cors.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-146">This command adds the Microsoft.Owin.Cors library to the project.</span></span> <span data-ttu-id="4a3b8-147">V této knihovně se použije pro podporu jiné domény, který je nutný pro aplikace, které jsou hostiteli SignalR a klient webové stránky v různých doménách.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-147">This library will be used for cross-domain support, which is required for applications that host SignalR and a web page client in different domains.</span></span> <span data-ttu-id="4a3b8-148">Vzhledem k tomu, že je budete hostování serveru SignalR a webového klienta na jiné porty, to znamená, že mezi doménami musí být povolen pro komunikaci mezi těmito součástmi.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-148">Since you'll be hosting the SignalR server and the web client on different ports, this means that cross-domain must be enabled for communication between these components.</span></span>
5. <span data-ttu-id="4a3b8-149">Nahraďte obsah souboru Program.cs následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-149">Replace the contents of Program.cs with the following code.</span></span>

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    <span data-ttu-id="4a3b8-150">Ve výše uvedeném kódu obsahuje tři třídy:</span><span class="sxs-lookup"><span data-stu-id="4a3b8-150">The above code includes three classes:</span></span>

    - <span data-ttu-id="4a3b8-151">**Program**, včetně **hlavní** metoda definování primární cestu provádění.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-151">**Program**, including the **Main** method defining the primary path of execution.</span></span> <span data-ttu-id="4a3b8-152">V této metodě, webovou aplikaci typu **spuštění** je spuštěna na zadané adrese URL (`http://localhost:8080`).</span><span class="sxs-lookup"><span data-stu-id="4a3b8-152">In this method, a web application of type **Startup** is started at the specified URL (`http://localhost:8080`).</span></span> <span data-ttu-id="4a3b8-153">Pro potřeby zabezpečení na koncový bod se dá implementovat protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-153">If security is required on the endpoint, SSL can be implemented.</span></span> <span data-ttu-id="4a3b8-154">V tématu [postup: Nakonfigurujte certifikát protokolu SSL Port](https://msdn.microsoft.com/library/ms733791.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-154">See [How to: Configure a Port with an SSL Certificate](https://msdn.microsoft.com/library/ms733791.aspx) for more information.</span></span>
    - <span data-ttu-id="4a3b8-155">**Spuštění**, třída obsahující konfiguraci serveru SignalR (pouze konfigurace tento kurz používá je volání `UseCors`) a volání k `MapSignalR`, která vytvoří trasy pro všechny objekty rozbočovače v projektu.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-155">**Startup**, the class containing the configuration for the SignalR server (the only configuration this tutorial uses is the call to `UseCors`), and the call to `MapSignalR`, which creates routes for any Hub objects in the project.</span></span>
    - <span data-ttu-id="4a3b8-156">**MyHub**, rozbočovače SignalR třídu, která aplikace bude poskytovat klientům.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-156">**MyHub**, the SignalR Hub class that the application will provide to clients.</span></span> <span data-ttu-id="4a3b8-157">Tato třída obsahuje jedinou metodu, **odeslat**, že klienti zavolá k vysílání zprávy do všech ostatních připojených klientů.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-157">This class has a single method, **Send**, that clients will call to broadcast a message to all other connected clients.</span></span>
6. <span data-ttu-id="4a3b8-158">Zkompilování a spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-158">Compile and run the application.</span></span> <span data-ttu-id="4a3b8-159">Zadaná adresa na serveru běží by měl zobrazit v okně konzoly.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-159">The address that the server is running should show in a console window.</span></span>

    ![](tutorial-signalr-self-host/_static/image2.png)
7. <span data-ttu-id="4a3b8-160">Pokud se nezdaří spuštění s výjimkou `System.Reflection.TargetInvocationException was unhandled`, budete muset restartovat Visual Studio s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-160">If execution fails with the exception `System.Reflection.TargetInvocationException was unhandled`, you will need to restart Visual Studio with administrator privileges.</span></span>
8. <span data-ttu-id="4a3b8-161">Zastavte aplikaci, než budete pokračovat k dalšímu oddílu.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-161">Stop the application before proceeding to the next section.</span></span>

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a><span data-ttu-id="4a3b8-162">Přístup k serveru pomocí klienta JavaScript</span><span class="sxs-lookup"><span data-stu-id="4a3b8-162">Accessing the server with a JavaScript client</span></span>

<span data-ttu-id="4a3b8-163">V této části budete používat stejného klienta JavaScript z [kurzu Začínáme](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="4a3b8-163">In this section, you'll use the same JavaScript client from the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="4a3b8-164">Vytočit pouze jeden úpravy klientovi, který je explicitně zadat adresu URL rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-164">We'll only make one modification to the client, which is to explicitly define the hub URL.</span></span> <span data-ttu-id="4a3b8-165">S vlastním hostováním aplikací server nemusí být nutně na stejnou adresu jako adresu URL připojení (z důvodu reverzní proxy a nástroje pro vyrovnávání zatížení), takže adresa URL musí být explicitně definován.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-165">With a self-hosted application, the server may not necessarily be at the same address as the connection URL (due to reverse proxies and load balancers), so the URL needs to be defined explicitly.</span></span>

1. <span data-ttu-id="4a3b8-166">V **Průzkumníku řešení**, klikněte pravým tlačítkem na řešení a vyberte **přidat**, **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-166">In **Solution Explorer**, right-click on the solution and select **Add**, **New Project**.</span></span> <span data-ttu-id="4a3b8-167">Vyberte **webové** uzel a vyberte možnost **webové aplikace ASP.NET** šablony.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-167">Select the **Web** node, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="4a3b8-168">Název projektu "JavascriptClient" a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-168">Name the project "JavascriptClient" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image3.png)
2. <span data-ttu-id="4a3b8-169">Vyberte **prázdný** šablony a ponechejte zbývající možnosti zrušit.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-169">Select the **Empty** template, and leave the remaining options unselected.</span></span> <span data-ttu-id="4a3b8-170">Vyberte **vytvoření projektu**.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-170">Select **Create Project**.</span></span>

    ![](tutorial-signalr-self-host/_static/image4.png)
3. <span data-ttu-id="4a3b8-171">V konzole správce balíčku vyberte projekt "JavascriptClient" v **výchozí projekt** rozevíracího seznamu a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4a3b8-171">In the package manager console, select the "JavascriptClient" project in the **Default project** drop-down, and execute the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    <span data-ttu-id="4a3b8-172">Tento příkaz nainstaluje SignalR a JQuery knihoven, které budete potřebovat v klientovi.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-172">This command installs the SignalR and JQuery libraries that you'll need in the client.</span></span>
4. <span data-ttu-id="4a3b8-173">Klikněte pravým tlačítkem na projekt a vyberte **přidat**, **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-173">Right-click on your project and select **Add**, **New Item**.</span></span> <span data-ttu-id="4a3b8-174">Vyberte **webové** uzel a vyberte stránku HTML.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-174">Select the **Web** node, and select HTML Page.</span></span> <span data-ttu-id="4a3b8-175">Název stránky **Default.html**.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-175">Name the page **Default.html**.</span></span>

    ![](tutorial-signalr-self-host/_static/image5.png)
5. <span data-ttu-id="4a3b8-176">Nahraďte obsah novou stránku HTML následující kód.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-176">Replace the contents of the new HTML page with the following code.</span></span> <span data-ttu-id="4a3b8-177">Ověřte, že zde reference skriptu odpovídají skripty ve složce skripty projektu.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-177">Verify that the script references here match the scripts in the Scripts folder of the project.</span></span>

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    <span data-ttu-id="4a3b8-178">Následující kód (zvýrazněných v výše ukázka kódu) je přidání, které jste udělali u klienta použít v kurzu Začínáme Stared (kromě kód upgradu na verzi beta verze 2 SignalR).</span><span class="sxs-lookup"><span data-stu-id="4a3b8-178">The following code (highlighted in the code sample above) is the addition that you've made to the client used in the Getting Stared tutorial (in addition to upgrading the code to SignalR version 2 beta).</span></span> <span data-ttu-id="4a3b8-179">Tento řádek kódu explicitně nastaví adresu URL základní připojení pro SignalR na serveru.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-179">This line of code explicitly sets the base connection URL for SignalR on the server.</span></span>

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. <span data-ttu-id="4a3b8-180">Klikněte pravým tlačítkem na řešení a vyberte **nastavit projekty po spuštění...** . Vyberte **více projektů po spuštění** přepínač a nastavte oba projekty **akce** k **spustit**.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-180">Right-click on the solution, and select **Set Startup Projects...**. Select the **Multiple startup projects** radio button, and set both projects' **Action** to **Start**.</span></span>

    ![](tutorial-signalr-self-host/_static/image6.png)
7. <span data-ttu-id="4a3b8-181">Klikněte pravým tlačítkem na "Default.html" a vyberte **nastavit jako úvodní stránku**.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-181">Right-click on "Default.html" and select **Set As Start Page**.</span></span>
8. <span data-ttu-id="4a3b8-182">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-182">Run the application.</span></span> <span data-ttu-id="4a3b8-183">Spustí se server a stránky.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-183">The server and page will launch.</span></span> <span data-ttu-id="4a3b8-184">Budete muset znovu načíst webovou stránku (nebo vyberte **pokračovat** v ladicím programu) Pokud se stránka načte, než je server spuštěn.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-184">You may need to reload the web page (or select **Continue** in the debugger) if the page loads before the server is started.</span></span>
9. <span data-ttu-id="4a3b8-185">V prohlížeči zadejte uživatelské jméno při zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-185">In the browser, provide a username when prompted.</span></span> <span data-ttu-id="4a3b8-186">Zkopírujte adresu URL stránky do jiné okně nebo kartě prohlížeče a zadejte jiné uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-186">Copy the page's URL into another browser tab or window and provide a different username.</span></span> <span data-ttu-id="4a3b8-187">Bude moct odesílat zprávy v podokně Prohlížeč jeden na druhý jako v kurzu Začínáme.</span><span class="sxs-lookup"><span data-stu-id="4a3b8-187">You will be able to send messages from one browser pane to the other, as in the Getting Started tutorial.</span></span>
