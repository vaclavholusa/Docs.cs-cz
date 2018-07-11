---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Vytvoření klientské aplikace OData v4 (C#) | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/26/2014
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: 1e621988b43b4f012f113d7a52250879da56bb1c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818673"
---
<a name="create-an-odata-v4-client-app-c"></a><span data-ttu-id="14bf2-102">Vytvoření klientské aplikace OData v4 (C#)</span><span class="sxs-lookup"><span data-stu-id="14bf2-102">Create an OData v4 Client App (C#)</span></span>
====================
<span data-ttu-id="14bf2-103">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="14bf2-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="14bf2-104">V předchozím kurzu jste vytvořili základní služby OData, který podporuje operace CRUD.</span><span class="sxs-lookup"><span data-stu-id="14bf2-104">In the previous tutorial, you created a basic OData service that supports CRUD operations.</span></span> <span data-ttu-id="14bf2-105">Nyní Pojďme vytvořit klienta pro službu.</span><span class="sxs-lookup"><span data-stu-id="14bf2-105">Now let's create a client for the service.</span></span>

<span data-ttu-id="14bf2-106">Spustit novou instanci sady Visual Studio a vytvořte nový projekt konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="14bf2-106">Start a new instance of Visual Studio and create a new console application project.</span></span> <span data-ttu-id="14bf2-107">V **nový projekt** dialogového okna, vyberte **nainstalováno** &gt; **šablony** &gt; **Visual C#** &gt; **Windows Desktop**a vyberte **konzolovou aplikaci** šablony.</span><span class="sxs-lookup"><span data-stu-id="14bf2-107">In the **New Project** dialog, select **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Windows Desktop**, and select the **Console Application** template.</span></span> <span data-ttu-id="14bf2-108">Pojmenujte projekt &quot;ProductsApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="14bf2-108">Name the project &quot;ProductsApp&quot;.</span></span>

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="14bf2-109">Aplikace konzoly můžete také přidat do stejného řešení sady Visual Studio, která obsahuje službu OData.</span><span class="sxs-lookup"><span data-stu-id="14bf2-109">You can also add the console app to the same Visual Studio solution that contains the OData service.</span></span>


## <a name="install-the-odata-client-code-generator"></a><span data-ttu-id="14bf2-110">Instalace generátoru kódu klienta OData</span><span class="sxs-lookup"><span data-stu-id="14bf2-110">Install the OData Client Code Generator</span></span>

<span data-ttu-id="14bf2-111">Z **nástroje** nabídce vyberte možnost **rozšíření a aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="14bf2-111">From the **Tools** menu, select **Extensions and Updates**.</span></span> <span data-ttu-id="14bf2-112">Vyberte **Online** &gt; **Galerie sady Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="14bf2-112">Select **Online** &gt; **Visual Studio Gallery**.</span></span> <span data-ttu-id="14bf2-113">Do vyhledávacího pole vyhledejte &quot;generátor kódu klienta OData&quot;.</span><span class="sxs-lookup"><span data-stu-id="14bf2-113">In the search box, search for &quot;OData Client Code Generator&quot;.</span></span> <span data-ttu-id="14bf2-114">Klikněte na tlačítko **Stáhnout** nainstalovat VSIX.</span><span class="sxs-lookup"><span data-stu-id="14bf2-114">Click **Download** to install the VSIX.</span></span> <span data-ttu-id="14bf2-115">Vám může zobrazit výzva k restartování sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="14bf2-115">You might be prompted to restart Visual Studio.</span></span>

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a><span data-ttu-id="14bf2-116">Místní spuštění služby OData</span><span class="sxs-lookup"><span data-stu-id="14bf2-116">Run the OData Service Locally</span></span>

<span data-ttu-id="14bf2-117">Spusťte projekt ProductService ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="14bf2-117">Run the ProductService project from Visual Studio.</span></span> <span data-ttu-id="14bf2-118">Ve výchozím nastavení spustí aplikace Visual Studio prohlížeč na kořenový adresář aplikace.</span><span class="sxs-lookup"><span data-stu-id="14bf2-118">By default, Visual Studio launches a browser to the application root.</span></span> <span data-ttu-id="14bf2-119">Poznámka: identifikátor URI; to budete potřebovat v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="14bf2-119">Note the URI; you will need this in the next step.</span></span> <span data-ttu-id="14bf2-120">Nechte aplikaci spuštěnou.</span><span class="sxs-lookup"><span data-stu-id="14bf2-120">Leave the application running.</span></span>

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="14bf2-121">Když vložíte oba projekty ve stejném řešení, ujistěte se, že ke spuštění ProductService projektu bez ladění.</span><span class="sxs-lookup"><span data-stu-id="14bf2-121">If you put both projects in the same solution, make sure to run the ProductService project without debugging.</span></span> <span data-ttu-id="14bf2-122">V dalším kroku je potřeba udržet službu při úpravě projektu konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="14bf2-122">In the next step, you will need to keep the service running while you modify the console application project.</span></span>


## <a name="generate-the-service-proxy"></a><span data-ttu-id="14bf2-123">Generování Proxy služby</span><span class="sxs-lookup"><span data-stu-id="14bf2-123">Generate the Service Proxy</span></span>

<span data-ttu-id="14bf2-124">Proxy služby je třída rozhraní .NET, která definuje metody pro přístup ke službě OData.</span><span class="sxs-lookup"><span data-stu-id="14bf2-124">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="14bf2-125">Proxy server překládá volání metod na požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="14bf2-125">The proxy translates method calls into HTTP requests.</span></span> <span data-ttu-id="14bf2-126">Vytvoří třídu proxy spuštěním [šablona T4](https://msdn.microsoft.com/library/bb126445.aspx).</span><span class="sxs-lookup"><span data-stu-id="14bf2-126">You will create the proxy class by running a [T4 template](https://msdn.microsoft.com/library/bb126445.aspx).</span></span>

<span data-ttu-id="14bf2-127">Klikněte pravým tlačítkem na projekt.</span><span class="sxs-lookup"><span data-stu-id="14bf2-127">Right-click the project.</span></span> <span data-ttu-id="14bf2-128">Vyberte **přidat** &gt; **nová položka**.</span><span class="sxs-lookup"><span data-stu-id="14bf2-128">Select **Add** &gt; **New Item**.</span></span>

![](create-an-odata-v4-client-app/_static/image5.png)

<span data-ttu-id="14bf2-129">V **přidat novou položku** dialogového okna, vyberte **položky Visual C#** &gt; **kód** &gt; **klient OData**.</span><span class="sxs-lookup"><span data-stu-id="14bf2-129">In the **Add New Item** dialog, select **Visual C# Items** &gt; **Code** &gt; **OData Client**.</span></span> <span data-ttu-id="14bf2-130">Název šablony &quot;ProductClient.tt&quot;.</span><span class="sxs-lookup"><span data-stu-id="14bf2-130">Name the template &quot;ProductClient.tt&quot;.</span></span> <span data-ttu-id="14bf2-131">Klikněte na tlačítko **přidat** proklikat upozornění zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="14bf2-131">Click **Add** and click through the security warning.</span></span>

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

<span data-ttu-id="14bf2-132">V tomto okamžiku budete dojde k chybě, které můžete ignorovat.</span><span class="sxs-lookup"><span data-stu-id="14bf2-132">At this point, you'll get an error, which you can ignore.</span></span> <span data-ttu-id="14bf2-133">Visual Studio automaticky spustí šablony, ale šablona potřebuje některá nastavení konfigurace první.</span><span class="sxs-lookup"><span data-stu-id="14bf2-133">Visual Studio automatically runs the template, but the template needs some configuration settings first.</span></span>

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

<span data-ttu-id="14bf2-134">Otevřete soubor ProductClient.odata.config. V `Parameter` elementu, vložením identifikátoru URI z projektu ProductService (předchozího kroku).</span><span class="sxs-lookup"><span data-stu-id="14bf2-134">Open the file ProductClient.odata.config. In the `Parameter` element, paste in the URI from the ProductService project (previous step).</span></span> <span data-ttu-id="14bf2-135">Příklad:</span><span class="sxs-lookup"><span data-stu-id="14bf2-135">For example:</span></span>

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

<span data-ttu-id="14bf2-136">Spusťte šablonu znovu.</span><span class="sxs-lookup"><span data-stu-id="14bf2-136">Run the template again.</span></span> <span data-ttu-id="14bf2-137">V Průzkumníku řešení klikněte pravým tlačítkem myši klikněte na soubor ProductClient.tt a vyberte **spustit vlastní nástroj**.</span><span class="sxs-lookup"><span data-stu-id="14bf2-137">In Solution Explorer, right click the ProductClient.tt file and select **Run Custom Tool**.</span></span>

<span data-ttu-id="14bf2-138">Šablona vytvoří soubor kódu s názvem ProductClient.cs, který definuje proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="14bf2-138">The template creates a code file named ProductClient.cs that defines the proxy.</span></span> <span data-ttu-id="14bf2-139">Při vývoji vaší aplikace, pokud změníte koncový bod OData, spusťte šablonu znovu a aktualizujte server proxy.</span><span class="sxs-lookup"><span data-stu-id="14bf2-139">As you develop your app, if you change the OData endpoint, run the template again to update the proxy.</span></span>

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a><span data-ttu-id="14bf2-140">Použít proxy server služby k volání služby OData</span><span class="sxs-lookup"><span data-stu-id="14bf2-140">Use the Service Proxy to Call the OData Service</span></span>

<span data-ttu-id="14bf2-141">Otevřete soubor Program.cs a nahraďte často používaný kód následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="14bf2-141">Open the file Program.cs and replace the boilerplate code with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

<span data-ttu-id="14bf2-142">Nahraďte hodnotu *serviceuri:* s URI služby ze dříve.</span><span class="sxs-lookup"><span data-stu-id="14bf2-142">Replace the value of *serviceUri* with the service URI from earlier.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

<span data-ttu-id="14bf2-143">Při spuštění aplikace by měl výstup následující:</span><span class="sxs-lookup"><span data-stu-id="14bf2-143">When you run the app, it should output the following:</span></span>

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
