---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: "Pomocí čítače výkonu SignalR webové role Azure | Microsoft Docs"
author: guardrex
description: "Jak nainstalovat a používat čítače výkonu SignalR webové role Azure."
keywords: "Čítač ASP.NET,signalr,Performance, webové role azure"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2017
ms.topic: article
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 2f6c6feb030fc17f95e7862c39029569f3d8c5dc
ms.sourcegitcommit: d8aa1d314891e981460b5e5c912afb730adbb3ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/05/2018
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a><span data-ttu-id="10813-104">Pomocí čítače výkonu SignalR webové role Azure</span><span class="sxs-lookup"><span data-stu-id="10813-104">Using SignalR performance counters in an Azure Web Role</span></span>

<span data-ttu-id="10813-105">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="10813-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="10813-106">Čítače výkonu SignalR se používají ke sledování výkonu vaší aplikace webové role Azure.</span><span class="sxs-lookup"><span data-stu-id="10813-106">SignalR performance counters are used to monitor your app's performance in an Azure Web Role.</span></span> <span data-ttu-id="10813-107">Čítače jsou zachyceny Microsoft Azure Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="10813-107">The counters are captured by Microsoft Azure Diagnostics.</span></span> <span data-ttu-id="10813-108">Čítače výkonu SignalR nainstalujete v Azure s *signalr.exe*, stejný nástroj, který slouží pro samostatnou nebo místní aplikace.</span><span class="sxs-lookup"><span data-stu-id="10813-108">You install SignalR performance counters on Azure with *signalr.exe*, the same tool used for standalone or on-premises apps.</span></span> <span data-ttu-id="10813-109">Vzhledem k tomu, že jsou přechodné Azure role, můžete aplikaci nakonfigurovat tak, instalace a registrace čítače výkonu SignalR při spuštění.</span><span class="sxs-lookup"><span data-stu-id="10813-109">Since Azure roles are transient, you configure an app to install and register SignalR performance counters upon startup.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="10813-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="10813-110">Prerequisites</span></span>

* [<span data-ttu-id="10813-111">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="10813-111">Visual Studio 2015</span></span>](https://www.visualstudio.com/vs/visual-studio-express/)
* <span data-ttu-id="10813-112">[Microsoft Azure SDK pro Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **Poznámka: Po instalaci sady SDK restartovat svůj počítač.**</span><span class="sxs-lookup"><span data-stu-id="10813-112">[Microsoft Azure SDK for Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **Note: Restart your machine after installing the SDK.**</span></span>
* <span data-ttu-id="10813-113">Předplatné Microsoft Azure: Chcete-li si zaregistrovat Bezplatný zkušební účet Azure, přečtěte si téma [bezplatná zkušební verze Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="10813-113">Microsoft Azure subscription: To sign up for a free Azure trial account, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a><span data-ttu-id="10813-114">Vytvoření aplikace Azure webovou roli, která zpřístupňuje čítače výkonu SignalR</span><span class="sxs-lookup"><span data-stu-id="10813-114">Creating an Azure Web Role application that exposes SignalR performance counters</span></span>

1. <span data-ttu-id="10813-115">Open Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="10813-115">Open Visual Studio 2015.</span></span>

2. <span data-ttu-id="10813-116">V sadě Visual Studio 2015, vyberte **soubor** > **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="10813-116">In Visual Studio 2015, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="10813-117">V **šablony** podokně **nový projekt** v části **Visual C#** uzlu, vyberte **cloudu** uzel a vyberte možnost **Azure Cloud Service** šablony.</span><span class="sxs-lookup"><span data-stu-id="10813-117">In the **Templates** pane of the **New Project** window under the **Visual C#** node, select the **Cloud** node and select the **Azure Cloud Service** template.</span></span> <span data-ttu-id="10813-118">Název aplikace **SignalRPerfCounters** a vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="10813-118">Name the app **SignalRPerfCounters** and select **OK**.</span></span>

   ![Nové cloudové aplikace](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)
    
4. <span data-ttu-id="10813-120">V **nová Cloudová služba Microsoft Azure** dialogovém okně, vyberte **webovou roli ASP.NET** a vyberte > tlačítko do projektu přidejte roli.</span><span class="sxs-lookup"><span data-stu-id="10813-120">In the **New Microsoft Azure Cloud Service** dialog, select **ASP.NET Web Role** and select the > button to add the role to the project.</span></span> <span data-ttu-id="10813-121">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="10813-121">Select **OK**.</span></span>

   ![Přidat webová Role ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)
    
5. <span data-ttu-id="10813-123">V **nové webové aplikace ASP.NET - WebRole1** dialogovém okně, vyberte **MVC** šablony a potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="10813-123">In the **New ASP.NET Web Application - WebRole1** dialog, select the **MVC** template, and then select **OK**.</span></span>

   ![Přidání MVC a webových rozhraní API](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)
    
6. <span data-ttu-id="10813-125">V **Průzkumníku řešení**, otevřete *diagnostics.wadcfgx* souboru pod **WebRole1**.</span><span class="sxs-lookup"><span data-stu-id="10813-125">In **Solution Explorer**, open the *diagnostics.wadcfgx* file under **WebRole1**.</span></span>

   ![Diagnostics.wadcfgx Průzkumník řešení](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)
    
7. <span data-ttu-id="10813-127">Nahraďte obsah souboru s následující konfigurací a soubor uložte:</span><span class="sxs-lookup"><span data-stu-id="10813-127">Replace the contents of the file with the following configuration and save the file:</span></span>

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]
    
8. <span data-ttu-id="10813-128">Otevřete **Konzola správce balíčků** z **nástroje** > **Správce balíčků NuGet**.</span><span class="sxs-lookup"><span data-stu-id="10813-128">Open the **Package Manager Console** from **Tools** > **NuGet Package Manager**.</span></span> <span data-ttu-id="10813-129">Zadejte následující příkazy pro instalaci nejnovější verze SignalR a balíček nástroje SignalR:</span><span class="sxs-lookup"><span data-stu-id="10813-129">Enter the following commands to install the latest version of SignalR and the SignalR utilities package:</span></span>

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]
    
9. <span data-ttu-id="10813-130">Nakonfiguruje aplikaci, aby při spuštění nebo recykluje nainstalovat čítače výkonu SignalR do role instance.</span><span class="sxs-lookup"><span data-stu-id="10813-130">Configure the app to install the SignalR performance counters into the role instance when it starts up or recycles.</span></span> <span data-ttu-id="10813-131">V **Průzkumníku řešení**, klikněte pravým tlačítkem na **WebRole1** projektu a vyberte **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="10813-131">In **Solution Explorer**, right-click on the **WebRole1** project and select **Add** > **New Folder**.</span></span> <span data-ttu-id="10813-132">Název nové složky *spuštění*.</span><span class="sxs-lookup"><span data-stu-id="10813-132">Name the new folder *Startup*.</span></span>

   ![Přidání spouštěcí složka](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)
    
10. <span data-ttu-id="10813-134">Kopírování *signalr.exe* souboru (přidán s **Microsoft.AspNet.SignalR.Utils** balíčku) z \<složky projektu > / SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\< verze > / tools k *spuštění* složky, které jste vytvořili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="10813-134">Copy the *signalr.exe* file (added with the **Microsoft.AspNet.SignalR.Utils** package) from \<project folder>/SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\<version>/tools to the *Startup* folder you created in the previous step.</span></span>

11. <span data-ttu-id="10813-135">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *spuštění* složky a vyberte **přidat** > **existující položka**.</span><span class="sxs-lookup"><span data-stu-id="10813-135">In **Solution Explorer**, right-click the *Startup* folder and select **Add** > **Existing Item**.</span></span> <span data-ttu-id="10813-136">V dialogovém okně se zobrazí, vyberte *signalr.exe* a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="10813-136">In the dialog that appears, select *signalr.exe* and select **Add**.</span></span>

    ![Přidání signalr.exe do projektu](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)
    
12. <span data-ttu-id="10813-138">Klikněte pravým tlačítkem na *spuštění* složky, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="10813-138">Right-click on the *Startup* folder you created.</span></span> <span data-ttu-id="10813-139">Vyberte **přidat** > **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="10813-139">Select **Add** > **New Item**.</span></span> <span data-ttu-id="10813-140">Vyberte **Obecné** uzlu, vyberte **textový soubor**a pojmenujte novou položku *SignalRPerfCounterInstall.cmd*.</span><span class="sxs-lookup"><span data-stu-id="10813-140">Select the **General** node, select **Text File**, and name the new item *SignalRPerfCounterInstall.cmd*.</span></span> <span data-ttu-id="10813-141">Tento příkaz soubor nainstaluje do webovou roli čítače výkonu SignalR.</span><span class="sxs-lookup"><span data-stu-id="10813-141">This command file will install the SignalR performance counters into the web role.</span></span>

    ![Vytvoření SignalR výkonu čítač instalace dávkového souboru](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)
     
13. <span data-ttu-id="10813-143">Když Visual Studio vytvoří *SignalRPerfCounterInstall.cmd* souboru, se automaticky otevře v hlavním okně.</span><span class="sxs-lookup"><span data-stu-id="10813-143">When Visual Studio creates the *SignalRPerfCounterInstall.cmd* file, it will automatically open in the main window.</span></span> <span data-ttu-id="10813-144">Obsah souboru nahraďte následující skript, pak uložte a zavřete soubor.</span><span class="sxs-lookup"><span data-stu-id="10813-144">Replace the contents of the file with the following script, then save and close the file.</span></span> <span data-ttu-id="10813-145">Tento skript spustí *signalr.exe*, k instanci role přidává čítače výkonu SignalR.</span><span class="sxs-lookup"><span data-stu-id="10813-145">This script executes *signalr.exe*, which adds the SignalR performance counters to the role instance.</span></span>

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]
    
14. <span data-ttu-id="10813-146">Vyberte *signalr.exe* souboru v **Průzkumníku řešení**.</span><span class="sxs-lookup"><span data-stu-id="10813-146">Select the *signalr.exe* file in **Solution Explorer**.</span></span> <span data-ttu-id="10813-147">V souboru **vlastnosti**, nastavte **kopírovat do výstupního adresáře** k **kopie vždy**.</span><span class="sxs-lookup"><span data-stu-id="10813-147">In the file's **Properties**, set **Copy to Output Directory** to **Copy Always**.</span></span>

    ![Nastavit kopírovat do výstupního adresáře pro kopírování vždy](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)
    
15. <span data-ttu-id="10813-149">Opakujte předchozí krok pro *SignalRPerfCounterInstall.cmd* souboru.</span><span class="sxs-lookup"><span data-stu-id="10813-149">Repeat the previous step for the *SignalRPerfCounterInstall.cmd* file.</span></span>

    
16. <span data-ttu-id="10813-150">Klikněte pravým tlačítkem na *SignalRPerfCounterInstall.cmd* soubor a vyberte **otevřít v**.</span><span class="sxs-lookup"><span data-stu-id="10813-150">Right-click on the *SignalRPerfCounterInstall.cmd* file and select **Open With**.</span></span> <span data-ttu-id="10813-151">V dialogovém okně se zobrazí, vyberte **binární Editor** a vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="10813-151">In the dialog that appears, select **Binary Editor** and select **OK**.</span></span>

    ![Otevřete s binární Editor](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)
    
17. <span data-ttu-id="10813-153">V editoru binární vyberte všechny úvodní bajty v souboru a odstraňte je.</span><span class="sxs-lookup"><span data-stu-id="10813-153">In the binary editor, select any leading bytes in the file and delete them.</span></span> <span data-ttu-id="10813-154">Soubor uložte a zavřete.</span><span class="sxs-lookup"><span data-stu-id="10813-154">Save and close the file.</span></span>

    ![Odstranit úvodní bajty](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)
    
18. <span data-ttu-id="10813-156">Otevřete *ServiceDefinition.csdef* a přidat úloha spuštění, které provádí *SignalrPerfCounterInstall.cmd* souborů při spuštění služby:</span><span class="sxs-lookup"><span data-stu-id="10813-156">Open *ServiceDefinition.csdef* and add a startup task that executes the *SignalrPerfCounterInstall.cmd* file when the service starts up:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]
    
19. <span data-ttu-id="10813-157">Otevřete `Views/Shared/_Layout.cshtml` a odeberte skript sady jQuery od konce souboru.</span><span class="sxs-lookup"><span data-stu-id="10813-157">Open `Views/Shared/_Layout.cshtml` and remove the jQuery bundle script from the end of the file.</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]
    
20. <span data-ttu-id="10813-158">Přidat JavaScript klienta, který volá nepřetržitě `increment` metoda na serveru.</span><span class="sxs-lookup"><span data-stu-id="10813-158">Add a JavaScript client that continuously calls the `increment` method on the server.</span></span> <span data-ttu-id="10813-159">Otevřete `Views/Home/Index.cshtml` a nahraďte jeho obsah následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="10813-159">Open `Views/Home/Index.cshtml` and replace the contents with the following code:</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]
    
21. <span data-ttu-id="10813-160">Vytvořte novou složku v **WebRole1** projektu s názvem *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="10813-160">Create a new folder in the **WebRole1** project named *Hubs*.</span></span> <span data-ttu-id="10813-161">Klikněte pravým tlačítkem myši *Hubs* složky v **Průzkumníku řešení**, vyberte **webové** > **SignalR**a vyberte  **Třídy rozbočovače SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="10813-161">Right-click the *Hubs* folder in **Solution Explorer**, select **Web** > **SignalR**, and select **SignalR Hub Class (v2)**.</span></span> <span data-ttu-id="10813-162">Název nového centra *MyHub.cs* a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="10813-162">Name the new hub *MyHub.cs* and select **Add**.</span></span>

    ![Přidání třídy rozbočovače SignalR do složky Hubs v dialogovém okně Přidat novou položku](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. <span data-ttu-id="10813-164">*MyHub.cs* se automaticky otevře v hlavním okně.</span><span class="sxs-lookup"><span data-stu-id="10813-164">*MyHub.cs* will automatically open in the main window.</span></span> <span data-ttu-id="10813-165">Nahraďte obsah následujícím kódem, pak uložte a zavřete soubor:</span><span class="sxs-lookup"><span data-stu-id="10813-165">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]
    
23. <span data-ttu-id="10813-166">*[Crank.exe](signalr-connection-density-testing-with-crank.md)*  je připojení hustotu testování nástroj, který poskytuje s SignalR codebase.</span><span class="sxs-lookup"><span data-stu-id="10813-166">*[Crank.exe](signalr-connection-density-testing-with-crank.md)* is a connection density testing tool provided with the SignalR codebase.</span></span> <span data-ttu-id="10813-167">Vzhledem k tomu, že Crank vyžaduje trvalé připojení, můžete přidat na váš web pro použití při testování.</span><span class="sxs-lookup"><span data-stu-id="10813-167">Since Crank requires a persistent connection, you add one to your site for use when testing.</span></span> <span data-ttu-id="10813-168">Přidat novou složku do **WebRole1** projekt s názvem *PersistentConnections*.</span><span class="sxs-lookup"><span data-stu-id="10813-168">Add a new folder to the **WebRole1** project called *PersistentConnections*.</span></span> <span data-ttu-id="10813-169">Klikněte pravým tlačítkem na tuto složku a vyberte **přidat** > **třída**.</span><span class="sxs-lookup"><span data-stu-id="10813-169">Right-click this folder and select **Add** > **Class**.</span></span> <span data-ttu-id="10813-170">Název nového souboru třídy *MyPersistentConnections.cs* a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="10813-170">Name the new class file *MyPersistentConnections.cs* and select **Add**.</span></span>

24. <span data-ttu-id="10813-171">Otevře se Visual Studio *MyPersistentConnections.cs* souboru v hlavním okně.</span><span class="sxs-lookup"><span data-stu-id="10813-171">Visual Studio will open the *MyPersistentConnections.cs* file in the main window.</span></span> <span data-ttu-id="10813-172">Nahraďte obsah následujícím kódem, pak uložte a zavřete soubor:</span><span class="sxs-lookup"><span data-stu-id="10813-172">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]
    
25. <span data-ttu-id="10813-173">Pomocí `Startup` třídu, objekty SignalR spustit při spuštění OWIN.</span><span class="sxs-lookup"><span data-stu-id="10813-173">Using the `Startup` class, the SignalR objects start when OWIN starts up.</span></span> <span data-ttu-id="10813-174">Otevřít nebo vytvořit *Startup.cs* a nahraďte jeho obsah následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="10813-174">Open or create *Startup.cs* and replace the contents with the following code:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]
    
    <span data-ttu-id="10813-175">Ve výše, kódu `OwinStartup` atribut určí této třídy lze spustit OWIN.</span><span class="sxs-lookup"><span data-stu-id="10813-175">In the code above, the `OwinStartup` attribute marks this class to start OWIN.</span></span> <span data-ttu-id="10813-176">`Configuration` Metoda spustí SignalR.</span><span class="sxs-lookup"><span data-stu-id="10813-176">The `Configuration` method starts SignalR.</span></span>
    
26. <span data-ttu-id="10813-177">Otestovat aplikaci v emulátoru Microsoft Azure stisknutím **F5**.</span><span class="sxs-lookup"><span data-stu-id="10813-177">Test your application in the Microsoft Azure Emulator by pressing **F5**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="10813-178">Pokud narazíte **FileLoadException** v **MapSignalR**, změnit přesměrování vazby v *web.config* pro následující:</span><span class="sxs-lookup"><span data-stu-id="10813-178">If you encounter a **FileLoadException** at **MapSignalR**, change the binding redirects in *web.config* to the following:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]
    
27. <span data-ttu-id="10813-179">Počkejte přibližně jednu minutu.</span><span class="sxs-lookup"><span data-stu-id="10813-179">Wait about one minute.</span></span> <span data-ttu-id="10813-180">Otevřete okno Průzkumník cloudu nástroje v sadě Visual Studio (**zobrazení** > **Průzkumník cloudu**) a rozbalte cestu `(Local)/Storage Accounts/(Development)/Tables`.</span><span class="sxs-lookup"><span data-stu-id="10813-180">Open the Cloud Explorer tool window in Visual Studio (**View** > **Cloud Explorer**) and expand the path `(Local)/Storage Accounts/(Development)/Tables`.</span></span> <span data-ttu-id="10813-181">Klikněte dvakrát na **WADPerformanceCountersTable**.</span><span class="sxs-lookup"><span data-stu-id="10813-181">Double-click **WADPerformanceCountersTable**.</span></span> <span data-ttu-id="10813-182">Měli byste vidět čítače SignalR v datech tabulky.</span><span class="sxs-lookup"><span data-stu-id="10813-182">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="10813-183">Pokud nevidíte v tabulce, musíte znovu zadat přihlašovací údaje Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="10813-183">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="10813-184">Je nutné vybrat **aktualizovat** tlačítko najdete v tabulce v **Průzkumník cloudu** nebo vyberte **aktualizovat** tlačítka v okně Otevřít tabulku se zobrazí data v tabulce.</span><span class="sxs-lookup"><span data-stu-id="10813-184">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

    ![Výběr v tabulce čítače výkonu WAD v sadě Visual Studio Průzkumníku cloudu](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Zobrazuje čítače shromážděné v tabulce WAD čítače výkonu](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)
    
28. <span data-ttu-id="10813-187">Testování vaší aplikace v cloudu aktualizujte **ServiceConfiguration.Cloud.cscfg** souboru a nastavit `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` na platný připojovací řetězec účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="10813-187">To test your application in the cloud, update the **ServiceConfiguration.Cloud.cscfg** file and set the `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` to a valid Azure Storage account connection string.</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. <span data-ttu-id="10813-188">Nasaďte aplikaci do vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="10813-188">Deploy the application to your Azure subscription.</span></span> <span data-ttu-id="10813-189">Podrobnosti o tom, jak nasadit aplikaci do Azure najdete v tématu [postup vytvoření a nasazení cloudové služby](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="10813-189">For details on how to deploy an application to Azure, see [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span>

30. <span data-ttu-id="10813-190">Počkejte několik minut.</span><span class="sxs-lookup"><span data-stu-id="10813-190">Wait a few minutes.</span></span> <span data-ttu-id="10813-191">V **Průzkumník cloudu**, vyhledejte účet úložiště, který jste nakonfigurovali výše a najít `WADPerformanceCountersTable` tabulky v ní.</span><span class="sxs-lookup"><span data-stu-id="10813-191">In **Cloud Explorer**, locate the storage account you configured above and find the `WADPerformanceCountersTable` table in it.</span></span> <span data-ttu-id="10813-192">Měli byste vidět čítače SignalR v datech tabulky.</span><span class="sxs-lookup"><span data-stu-id="10813-192">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="10813-193">Pokud nevidíte v tabulce, musíte znovu zadat přihlašovací údaje Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="10813-193">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="10813-194">Je nutné vybrat **aktualizovat** tlačítko najdete v tabulce v **Průzkumník cloudu** nebo vyberte **aktualizovat** tlačítka v okně Otevřít tabulku se zobrazí data v tabulce.</span><span class="sxs-lookup"><span data-stu-id="10813-194">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

<span data-ttu-id="10813-195">Zvláštní poděkování [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) pro původní obsah použitý v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="10813-195">Special thanks to [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) for the original content used in this tutorial.</span></span>
