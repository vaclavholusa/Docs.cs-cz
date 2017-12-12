---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: "Škálování SignalR službou Azure Service Bus (SignalR 1.x) | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 0dd245b597ebd4b58b60a53276d7808b6e2377e7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a><span data-ttu-id="3ed0d-102">Škálování SignalR službou Azure Service Bus (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="3ed0d-102">SignalR Scaleout with Azure Service Bus (SignalR 1.x)</span></span>
====================
<span data-ttu-id="3ed0d-103">podle [Karel Wasson](https://github.com/MikeWasson), [Patrik Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="3ed0d-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="3ed0d-104">V tomto kurzu nasadíte aplikaci SignalR webové roli systému Windows Azure, pomocí Service Bus propojovacího rozhraní k distribuci zpráv každá instance role.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-104">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="3ed0d-105">Předpoklady:</span><span class="sxs-lookup"><span data-stu-id="3ed0d-105">Prerequisites:</span></span>

- <span data-ttu-id="3ed0d-106">Účet služby Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-106">A Windows Azure account.</span></span>
- <span data-ttu-id="3ed0d-107">[Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="3ed0d-107">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="3ed0d-108">Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-108">Visual Studio 2012.</span></span>

<span data-ttu-id="3ed0d-109">Propojovacího rozhraní service bus je také kompatibilní s [sběrnice služby pro Windows Server](https://msdn.microsoft.com/en-us/library/windowsazure/dn282144.aspx), verze 1.1.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-109">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/en-us/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="3ed0d-110">Však není kompatibilní s verzí 1.0 sběrnice služby pro Windows Server.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-110">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="3ed0d-111">Ceny</span><span class="sxs-lookup"><span data-stu-id="3ed0d-111">Pricing</span></span>

<span data-ttu-id="3ed0d-112">Service Bus propojovacího rozhraní témata týkající se používá k odesílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-112">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="3ed0d-113">Cenová nejnovější informace najdete v tématu [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="3ed0d-113">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="3ed0d-114">V době psaní tohoto textu můžete odeslat 1 000 000 zpráv za měsíc pro menší než 1 USD.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-114">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="3ed0d-115">Propojovacího rozhraní odešle zprávu sběrnice služby pro každé vyvolání metody rozbočovače SignalR.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-115">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="3ed0d-116">Existují také některé řízení zpráv pro připojení, odpojení, spojující a výstupu skupiny a tak dále.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-116">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="3ed0d-117">Ve většině aplikací se bude většinu přenosu zpráv volání metod rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-117">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="3ed0d-118">Přehled</span><span class="sxs-lookup"><span data-stu-id="3ed0d-118">Overview</span></span>

<span data-ttu-id="3ed0d-119">Předtím, než se nám získat podrobný kurz, zde je rychlý přehled toho, co jste se dělat.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-119">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="3ed0d-120">Použití portálu Windows Azure k vytvoření nového oboru názvů Service Bus.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-120">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="3ed0d-121">Přidejte tyto balíčky NuGet do vaší aplikace:</span><span class="sxs-lookup"><span data-stu-id="3ed0d-121">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="3ed0d-122">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="3ed0d-122">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="3ed0d-123">Microsoft.AspNet.SignalR.ServiceBus</span><span class="sxs-lookup"><span data-stu-id="3ed0d-123">Microsoft.AspNet.SignalR.ServiceBus</span></span>](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. <span data-ttu-id="3ed0d-124">Vytvoření aplikace SignalR.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-124">Create a SignalR application.</span></span>
4. <span data-ttu-id="3ed0d-125">Přidejte následující kód do souboru Global.asax konfigurace propojovacího rozhraní:</span><span class="sxs-lookup"><span data-stu-id="3ed0d-125">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="3ed0d-126">Pro každou aplikaci vyberte jinou hodnotu pro "YourAppName".</span><span class="sxs-lookup"><span data-stu-id="3ed0d-126">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="3ed0d-127">Nepoužívejte stejnou hodnotu napříč různými aplikacemi.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-127">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="3ed0d-128">Vytvoření služby Azure</span><span class="sxs-lookup"><span data-stu-id="3ed0d-128">Create the Azure Services</span></span>

<span data-ttu-id="3ed0d-129">Vytvoření cloudové služby, jak je popsáno v [postup vytvoření a nasazení cloudové služby](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="3ed0d-129">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="3ed0d-130">Postupujte podle kroků v části "postup: vytvoření cloudové služby, pomocí funkce rychle vytvořit".</span><span class="sxs-lookup"><span data-stu-id="3ed0d-130">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="3ed0d-131">V tomto kurzu není potřeba nahrát certifikát.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-131">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="3ed0d-132">Vytvořit nový obor názvů Service Bus, jak je popsáno v [jak použití Service Bus témata nebo odběry](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="3ed0d-132">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="3ed0d-133">Postupujte podle kroků v části "Vytvoření služby Namespace".</span><span class="sxs-lookup"><span data-stu-id="3ed0d-133">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="3ed0d-134">Ujistěte se, že vyberte stejnou oblast pro cloudové služby a oboru názvů Service Bus.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-134">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="3ed0d-135">Vytvoření projektu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3ed0d-135">Create the Visual Studio Project</span></span>

<span data-ttu-id="3ed0d-136">Spuštění sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-136">Start Visual Studio.</span></span> <span data-ttu-id="3ed0d-137">Z **soubor** nabídky, klikněte na tlačítko **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-137">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="3ed0d-138">V **nový projekt** dialogové okno, rozbalte seznam **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-138">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="3ed0d-139">V části **nainstalovaných šablonách**, vyberte **cloudu** a pak vyberte **Windows Azure Cloud Service**.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-139">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="3ed0d-140">Ponechte výchozí rozhraní .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-140">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="3ed0d-141">Název aplikace ChatService a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-141">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="3ed0d-142">V **nová Cloudová služba Windows Azure** dialogovém okně, vyberte webovou roli ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-142">In the **New Windows Azure Cloud Service** dialog, select ASP.NET MVC 4 Web Role.</span></span> <span data-ttu-id="3ed0d-143">Klikněte na tlačítko se šipkou vpravo (**&gt;**) přidejte roli pro vaše řešení.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-143">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="3ed0d-144">Najeďte myší novou roli, tak na ikonu tužky viditelné.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-144">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="3ed0d-145">Kliknutím na tuto ikonu přejmenujte roli.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-145">Click this icon to rename the role.</span></span> <span data-ttu-id="3ed0d-146">Název role "SignalRChat" a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-146">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="3ed0d-147">V **nový ASP.NET MVC 4 projekt** průvodci vyberte **Internetové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-147">In the **New ASP.NET MVC 4 Project** wizard, select **Internet Application**.</span></span> <span data-ttu-id="3ed0d-148">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-148">Click **OK**.</span></span> <span data-ttu-id="3ed0d-149">Průvodce projektem vytvoří dva projekty:</span><span class="sxs-lookup"><span data-stu-id="3ed0d-149">The project wizard creates two projects:</span></span>

- <span data-ttu-id="3ed0d-150">ChatService: Tento projekt je aplikace systému Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-150">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="3ed0d-151">Definuje Azure role a další možnosti konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-151">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="3ed0d-152">SignalRChat: Tento projekt je projektu ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-152">SignalRChat: This project is your ASP.NET MVC 4 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="3ed0d-153">Vytvoření aplikace SignalR Chat</span><span class="sxs-lookup"><span data-stu-id="3ed0d-153">Create the SignalR Chat Application</span></span>

<span data-ttu-id="3ed0d-154">Vytvoření chatovací aplikace, postupujte podle kroků v tomto kurzu [Začínáme s SignalR a MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="3ed0d-154">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="3ed0d-155">Instalace potřebných knihoven pomocí NuGet.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-155">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="3ed0d-156">Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**, pak vyberte **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-156">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="3ed0d-157">V **Konzola správce balíčků** okno, zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="3ed0d-157">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="3ed0d-158">Použití `-ProjectName` možnost instalovat balíčky do projektu ASP.NET MVC, nikoli projekt systému Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-158">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="3ed0d-159">Konfigurace propojovacího rozhraní</span><span class="sxs-lookup"><span data-stu-id="3ed0d-159">Configure the Backplane</span></span>

<span data-ttu-id="3ed0d-160">V souboru Global.asax aplikace přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="3ed0d-160">In your application's Global.asax file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="3ed0d-161">Teď je potřeba získat připojovací řetězec service bus.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-161">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="3ed0d-162">Na portálu Azure vyberte obor názvů sběrnice služby, kterou jste vytvořili a klikněte na ikonu přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-162">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="3ed0d-163">Zkopírujte připojovací řetězec do schránky a vložte ji do *connectionString* proměnné.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-163">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="3ed0d-164">Nasazení do Azure</span><span class="sxs-lookup"><span data-stu-id="3ed0d-164">Deploy to Azure</span></span>

<span data-ttu-id="3ed0d-165">V Průzkumníku řešení rozbalte **role** složky uvnitř ChatService projektu.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-165">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

<span data-ttu-id="3ed0d-166">Klikněte pravým tlačítkem na roli SignalRChat a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-166">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="3ed0d-167">Vyberte **konfigurace** kartě. V části **instance** vyberte 2.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-167">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="3ed0d-168">Můžete také nastavit velikost virtuálního počítače na **navíc malé**.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-168">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="3ed0d-169">Uložte změny.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-169">Save the changes.</span></span>

<span data-ttu-id="3ed0d-170">V Průzkumníku řešení klikněte pravým tlačítkem na projekt ChatService.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-170">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="3ed0d-171">Vyberte **publikování**.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-171">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="3ed0d-172">Pokud je vaše první čas publikování do služby Windows Azure, je nutné stáhnout přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-172">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="3ed0d-173">V **publikovat** průvodce, klikněte na tlačítko "Přihlásit se k stáhnout přihlašovací údaje".</span><span class="sxs-lookup"><span data-stu-id="3ed0d-173">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="3ed0d-174">To vás vyzve k přihlásit k portálu Windows Azure a stáhněte soubor nastavení publikování.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-174">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="3ed0d-175">Klikněte na tlačítko **Import** a vyberte soubor nastavení publikování, který jste si stáhli.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-175">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="3ed0d-176">Klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-176">Click **Next**.</span></span> <span data-ttu-id="3ed0d-177">V **nastavení publikování** dialogové okno, v části **Cloudová služba**, vyberte cloudovou službu, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-177">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="3ed0d-178">Klikněte na tlačítko **publikování**.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-178">Click **Publish**.</span></span> <span data-ttu-id="3ed0d-179">Ho může trvat několik minut a nasaďte aplikaci spustit virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-179">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="3ed0d-180">Při spuštění aplikace chat instance rolí komunikovat přes Azure Service Bus pomocí tématu Service Bus.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-180">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="3ed0d-181">Téma je fronta zpráv, který umožňuje více odběrateli.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-181">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="3ed0d-182">Propojovacího rozhraní automaticky vytvoří téma a odběrů.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-182">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="3ed0d-183">Odběry a aktivitu zprávu zobrazíte otevřete portál Azure, vyberte obor názvů Service Bus a klikněte na "Témata".</span><span class="sxs-lookup"><span data-stu-id="3ed0d-183">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="3ed0d-184">Ujistěte se, trvat několik minut pro aktivitu zprávu objeví v řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-184">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="3ed0d-185">SignalR spravuje životnost tématu.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-185">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="3ed0d-186">Tak dlouho, dokud vaše aplikace je nasazená, nemusíte zkuste ručně odstraňte témata nebo změňte nastavení v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="3ed0d-186">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>
