---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Vytvoření aplikace OData v4 klienta (C#) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: 51a3c7b9c5b6525d6d82b9a45910f58b71268b7f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036697"
---
<a name="create-an-odata-v4-client-app-c"></a><span data-ttu-id="b0fb7-102">Vytvoření aplikace OData v4 klienta (C#)</span><span class="sxs-lookup"><span data-stu-id="b0fb7-102">Create an OData v4 Client App (C#)</span></span>
====================
<span data-ttu-id="b0fb7-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b0fb7-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="b0fb7-104">V předchozích kurzu vytvoříte základní služba OData, která podporuje operace CRUD.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-104">In the previous tutorial, you created a basic OData service that supports CRUD operations.</span></span> <span data-ttu-id="b0fb7-105">Nyní vytvoříme klienta pro službu.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-105">Now let's create a client for the service.</span></span>

<span data-ttu-id="b0fb7-106">Spusťte novou instanci sady Visual Studio a vytvořte nový projekt konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-106">Start a new instance of Visual Studio and create a new console application project.</span></span> <span data-ttu-id="b0fb7-107">V **nový projekt** dialogovém okně, vyberte **nainstalovaná** &gt; **šablony** &gt; **Visual C#** &gt; **Windows Desktop**a vyberte **konzolové aplikace** šablony.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-107">In the **New Project** dialog, select **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Windows Desktop**, and select the **Console Application** template.</span></span> <span data-ttu-id="b0fb7-108">Název projektu &quot;ProductsApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-108">Name the project &quot;ProductsApp&quot;.</span></span>

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="b0fb7-109">Konzolové aplikace můžete také přidat do stejného řešení sady Visual Studio, který obsahuje službu OData.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-109">You can also add the console app to the same Visual Studio solution that contains the OData service.</span></span>


## <a name="install-the-odata-client-code-generator"></a><span data-ttu-id="b0fb7-110">Nainstalujte klienta OData generátor kódu</span><span class="sxs-lookup"><span data-stu-id="b0fb7-110">Install the OData Client Code Generator</span></span>

<span data-ttu-id="b0fb7-111">Z **nástroje** nabídce vyberte možnost **rozšíření a aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-111">From the **Tools** menu, select **Extensions and Updates**.</span></span> <span data-ttu-id="b0fb7-112">Vyberte **Online** &gt; **Galerie sady Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-112">Select **Online** &gt; **Visual Studio Gallery**.</span></span> <span data-ttu-id="b0fb7-113">Do vyhledávacího pole Hledat &quot;generátor kódu klienta OData&quot;.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-113">In the search box, search for &quot;OData Client Code Generator&quot;.</span></span> <span data-ttu-id="b0fb7-114">Klikněte na tlačítko **Stáhnout** k instalaci VSIX.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-114">Click **Download** to install the VSIX.</span></span> <span data-ttu-id="b0fb7-115">Může se zobrazit výzva k restartování sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-115">You might be prompted to restart Visual Studio.</span></span>

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a><span data-ttu-id="b0fb7-116">Spusťte službu OData místně</span><span class="sxs-lookup"><span data-stu-id="b0fb7-116">Run the OData Service Locally</span></span>

<span data-ttu-id="b0fb7-117">Spusťte projekt ProductService ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-117">Run the ProductService project from Visual Studio.</span></span> <span data-ttu-id="b0fb7-118">Ve výchozím nastavení Visual Studio spustí prohlížeč na kořenový adresář aplikace.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-118">By default, Visual Studio launches a browser to the application root.</span></span> <span data-ttu-id="b0fb7-119">Poznámka: identifikátor URI; je nutné v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-119">Note the URI; you will need this in the next step.</span></span> <span data-ttu-id="b0fb7-120">Nechte aplikaci spuštěnou.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-120">Leave the application running.</span></span>

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="b0fb7-121">Když vložíte obou projektů ve stejném řešení, nezapomeňte spustit projekt ProductService bez ladění.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-121">If you put both projects in the same solution, make sure to run the ProductService project without debugging.</span></span> <span data-ttu-id="b0fb7-122">V dalším kroku musíte udržovat služby běží, když upravíte projekt konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-122">In the next step, you will need to keep the service running while you modify the console application project.</span></span>


## <a name="generate-the-service-proxy"></a><span data-ttu-id="b0fb7-123">Generovat Proxy služby</span><span class="sxs-lookup"><span data-stu-id="b0fb7-123">Generate the Service Proxy</span></span>

<span data-ttu-id="b0fb7-124">Proxy server služby je třída rozhraní .NET, která definuje metody pro přístup ke službě OData.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-124">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="b0fb7-125">Proxy server přeloží volání metod na požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-125">The proxy translates method calls into HTTP requests.</span></span> <span data-ttu-id="b0fb7-126">Vytvoří třídu proxy spuštěním [šablony T4](https://msdn.microsoft.com/library/bb126445.aspx).</span><span class="sxs-lookup"><span data-stu-id="b0fb7-126">You will create the proxy class by running a [T4 template](https://msdn.microsoft.com/library/bb126445.aspx).</span></span>

<span data-ttu-id="b0fb7-127">Klikněte pravým tlačítkem na projekt.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-127">Right-click the project.</span></span> <span data-ttu-id="b0fb7-128">Vyberte **přidat** &gt; **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-128">Select **Add** &gt; **New Item**.</span></span>

![](create-an-odata-v4-client-app/_static/image5.png)

<span data-ttu-id="b0fb7-129">V **přidat novou položku** dialogovém okně, vyberte **Visual C# položky** &gt; **kód** &gt; **klient OData**.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-129">In the **Add New Item** dialog, select **Visual C# Items** &gt; **Code** &gt; **OData Client**.</span></span> <span data-ttu-id="b0fb7-130">Název šablony &quot;ProductClient.tt&quot;.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-130">Name the template &quot;ProductClient.tt&quot;.</span></span> <span data-ttu-id="b0fb7-131">Klikněte na tlačítko **přidat** a klikněte na tlačítko prostřednictvím upozornění zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-131">Click **Add** and click through the security warning.</span></span>

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

<span data-ttu-id="b0fb7-132">V tomto okamžiku budete dojde k chybě, která můžete ignorovat.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-132">At this point, you'll get an error, which you can ignore.</span></span> <span data-ttu-id="b0fb7-133">Visual Studio automaticky spustí šablony, ale šablona potřebuje některá nastavení konfigurace první.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-133">Visual Studio automatically runs the template, but the template needs some configuration settings first.</span></span>

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

<span data-ttu-id="b0fb7-134">Otevřete soubor ProductClient.odata.config. V `Parameter` elementu, vložte identifikátor URI z projektu ProductService (předchozí krok).</span><span class="sxs-lookup"><span data-stu-id="b0fb7-134">Open the file ProductClient.odata.config. In the `Parameter` element, paste in the URI from the ProductService project (previous step).</span></span> <span data-ttu-id="b0fb7-135">Příklad:</span><span class="sxs-lookup"><span data-stu-id="b0fb7-135">For example:</span></span>

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

<span data-ttu-id="b0fb7-136">Znovu spusťte šablonu.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-136">Run the template again.</span></span> <span data-ttu-id="b0fb7-137">V Průzkumníku řešení klikněte pravým tlačítkem na soubor ProductClient.tt a vyberte **spustit nástroj pro vlastní**.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-137">In Solution Explorer, right click the ProductClient.tt file and select **Run Custom Tool**.</span></span>

<span data-ttu-id="b0fb7-138">Šablona vytvoří kód soubor s názvem ProductClient.cs, která definuje proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-138">The template creates a code file named ProductClient.cs that defines the proxy.</span></span> <span data-ttu-id="b0fb7-139">Když budete vyvíjet aplikace, pokud změníte koncový bod OData, spusťte šablonu znovu k aktualizaci proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-139">As you develop your app, if you change the OData endpoint, run the template again to update the proxy.</span></span>

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a><span data-ttu-id="b0fb7-140">Použít proxy server služby pro volání služby OData</span><span class="sxs-lookup"><span data-stu-id="b0fb7-140">Use the Service Proxy to Call the OData Service</span></span>

<span data-ttu-id="b0fb7-141">Otevřete soubor Program.cs a nahradit následující často používaný kód.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-141">Open the file Program.cs and replace the boilerplate code with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

<span data-ttu-id="b0fb7-142">Nahraďte hodnotu *serviceUri* s URI služby z dříve.</span><span class="sxs-lookup"><span data-stu-id="b0fb7-142">Replace the value of *serviceUri* with the service URI from earlier.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

<span data-ttu-id="b0fb7-143">Při spuštění aplikace by měla výstup následující:</span><span class="sxs-lookup"><span data-stu-id="b0fb7-143">When you run the app, it should output the following:</span></span>

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
