---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Rozpoznání spouštěcí třídy OWIN | Dokumentace Microsoftu
author: Praburaj
description: Tento kurz ukazuje, jak konfigurovat načteny které třídy pro spuštění OWIN. Další informace o OWIN naleznete v tématu Přehled projektu Katana. V tomto kurzu se...
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: 591a04f429284ae73896807a6c2837ad498e8ae1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755187"
---
<a name="owin-startup-class-detection"></a><span data-ttu-id="7450a-105">Rozpoznání spouštěcí třídy OWIN</span><span class="sxs-lookup"><span data-stu-id="7450a-105">OWIN Startup Class Detection</span></span>
====================
<span data-ttu-id="7450a-106">podle [Praburaj manažer](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="7450a-106">by [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="7450a-107">Tento kurz ukazuje, jak konfigurovat načteny které třídy pro spuštění OWIN.</span><span class="sxs-lookup"><span data-stu-id="7450a-107">This tutorial shows how to configure which OWIN startup class is loaded.</span></span> <span data-ttu-id="7450a-108">Další informace o OWIN, naleznete v tématu [Přehled projektu Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="7450a-108">For more information on OWIN, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="7450a-109">V tomto kurzu byla zapsána od Ricka Andersona ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Manažer Praburaj a Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).</span><span class="sxs-lookup"><span data-stu-id="7450a-109">This tutorial was written by Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan, and Howard Dierking ( [@howard\_dierking](https://twitter.com/howard_dierking) ).</span></span>
> 
> ## <a name="prerequisites"></a><span data-ttu-id="7450a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7450a-110">Prerequisites</span></span>
> 
> [<span data-ttu-id="7450a-111">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="7450a-111">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)


## <a name="owin-startup-class-detection"></a><span data-ttu-id="7450a-112">Rozpoznání spouštěcí třídy OWIN</span><span class="sxs-lookup"><span data-stu-id="7450a-112">OWIN Startup Class Detection</span></span>

 <span data-ttu-id="7450a-113">Každá aplikace OWIN obsahuje třídu spuštění zadávat komponenty pro kanál aplikací.</span><span class="sxs-lookup"><span data-stu-id="7450a-113">Every OWIN Application has a startup class where you specify components for the application pipeline.</span></span> <span data-ttu-id="7450a-114">Existují různé způsoby připojení třídu pro spuštění v modulu runtime, v závislosti na model hostingu zvolíte (OwinHost, služby IIS a služby IIS Express).</span><span class="sxs-lookup"><span data-stu-id="7450a-114">There are different ways you can connect your startup class with the runtime, depending on the hosting model you choose (OwinHost, IIS, and IIS-Express).</span></span> <span data-ttu-id="7450a-115">Třída při spuštění uvedeno v tomto kurzu je možné v každé hostitelské aplikace.</span><span class="sxs-lookup"><span data-stu-id="7450a-115">The startup class shown in this tutorial can be used in every hosting application.</span></span> <span data-ttu-id="7450a-116">Třída při spuštění napojení hostování modulu runtime pomocí jedné z těchto přístupů:</span><span class="sxs-lookup"><span data-stu-id="7450a-116">You connect the startup class with the hosting runtime using one of the these approaches:</span></span>  

1. <span data-ttu-id="7450a-117">**Zásady vytváření názvů**: Katana hledá třídu s názvem `Startup` v oboru názvů odpovídající název sestavení nebo v globálním oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="7450a-117">**Naming Convention**: Katana looks for a class named `Startup` in namespace matching the assembly name or the global namespace.</span></span>
2. <span data-ttu-id="7450a-118">**Atribut OwinStartup**: Toto je přístup k určení třída při spuštění bude trvat Většina vývojářů.</span><span class="sxs-lookup"><span data-stu-id="7450a-118">**OwinStartup Attribute**: This is the approach most developers will take to specify the startup class.</span></span> <span data-ttu-id="7450a-119">Tento atribut nastaví na třídu pro spuštění `TestStartup` třídy v `StartupDemo` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="7450a-119">The following attribute will set the startup class to the `TestStartup` class in the `StartupDemo` namespace.</span></span> 

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   <span data-ttu-id="7450a-120">`OwinStartup` Atribut přepisuje zásady vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="7450a-120">The `OwinStartup` attribute overrides the naming convention.</span></span> <span data-ttu-id="7450a-121">Můžete také zadat popisný název k tomuto atributu, ale použít popisný název vyžaduje také použití `appSetting` element v konfiguračním souboru.</span><span class="sxs-lookup"><span data-stu-id="7450a-121">You can also specify a friendly name with this attribute, however, using a friendly name requires you to also use the `appSetting` element in the configuration file.</span></span>
3. <span data-ttu-id="7450a-122">**Element nastavení aplikace v konfiguračním souboru**: `appSetting` přepíše element `OwinStartup` atribut a zásady vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="7450a-122">**The appSetting element in the Configuration file**: The `appSetting` element overrides the `OwinStartup` attribute and naming convention.</span></span> <span data-ttu-id="7450a-123">Můžete mít více tříd spuštění (každý pomocí `OwinStartup` atribut) a konfigurace, které třída při spuštění bude načten v konfiguračním souboru pomocí značek podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="7450a-123">You can have multiple startup classes (each using an `OwinStartup` attribute) and configure which startup class will be loaded in a configuration file using markup similar to the following:</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   <span data-ttu-id="7450a-124">Můžete také použít následující klíč, který explicitně určuje třídu pro spuštění a sestavení:</span><span class="sxs-lookup"><span data-stu-id="7450a-124">The following key, which explicitly specifies the startup class and assembly can also be used:</span></span> 

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   <span data-ttu-id="7450a-125">Následující kód XML v konfiguračním souboru určí název třídy popisný spuštění `ProductionConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="7450a-125">The following XML in the configuration file specifies a friendly startup class name of `ProductionConfiguration`.</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   <span data-ttu-id="7450a-126">Výše uvedené značky musí být použit s následující `OwinStartup` atribut, který určuje popisný název a způsobí, že `ProductionStartup2` má třída spustit.</span><span class="sxs-lookup"><span data-stu-id="7450a-126">The above markup must be used with the following `OwinStartup` attribute which specifies a friendly name and causes the `ProductionStartup2` class to run.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. <span data-ttu-id="7450a-127">Chcete-li zakázat zjišťování pro spuštění OWIN přidat `appSetting owin:AutomaticAppStartup` s hodnotou `"false"` v souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="7450a-127">To disable OWIN startup discovery add the `appSetting owin:AutomaticAppStartup` with a value of `"false"` in the web.config file.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a><span data-ttu-id="7450a-128">Vytvoření webové aplikace ASP.NET pomocí OWIN Startup</span><span class="sxs-lookup"><span data-stu-id="7450a-128">Create an ASP.NET Web App using OWIN Startup</span></span>

1. <span data-ttu-id="7450a-129">Vytvoří prázdná webová aplikace Asp.Net a pojmenujte ho **StartupDemo**.</span><span class="sxs-lookup"><span data-stu-id="7450a-129">Create an empty Asp.Net web application and name it **StartupDemo**.</span></span> <span data-ttu-id="7450a-130">– Instalace `Microsoft.Owin.Host.SystemWeb` pomocí Správce balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="7450a-130">- Install `Microsoft.Owin.Host.SystemWeb` using the NuGet package manager.</span></span> <span data-ttu-id="7450a-131">Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**a potom **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="7450a-131">From the **Tools** menu, select **Library Package Manager**, and then **Package Manager Console**.</span></span> <span data-ttu-id="7450a-132">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7450a-132">Enter the following command:</span></span>  

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. <span data-ttu-id="7450a-133">Přidání třídy pro spuštění OWIN.</span><span class="sxs-lookup"><span data-stu-id="7450a-133">Add an OWIN startup class.</span></span> <span data-ttu-id="7450a-134">V sadě Visual Studio 2013 klikněte pravým tlačítkem projekt a vyberte **přidat třídu**. - v **přidat novou položku** dialogového okna zadejte *OWIN* do vyhledávacího pole a změnit název, který má Startup.cs, a pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="7450a-134">In Visual Studio 2013 right click the project and select **Add Class**.- In the **Add New Item** dialog box, enter *OWIN* in the search field, and change the name to Startup.cs, and then click **Add**.</span></span>  
  
     ![](owin-startup-class-detection/_static/image1.png)   
  
   <span data-ttu-id="7450a-135">Další čas, které chcete přidat *třída Owin Startup*, bude uvedená v k dispozici **přidat** nabídky.</span><span class="sxs-lookup"><span data-stu-id="7450a-135">The next time you want to add an *Owin Startup class*, it will be in available from the **Add** menu.</span></span>  
   
     ![](owin-startup-class-detection/_static/image2.png)  
  
   <span data-ttu-id="7450a-136">Alternativně klikněte pravým tlačítkem na projekt a vyberte **přidat**a pak vyberte **nová položka**a pak vyberte **třída Owin Startup**.</span><span class="sxs-lookup"><span data-stu-id="7450a-136">Alternatively, you can right click the project and select **Add**, then select **New Item**, and then select the **Owin Startup class**.</span></span>  
  
     ![](owin-startup-class-detection/_static/image3.png)  
  
- <span data-ttu-id="7450a-137">Nahraďte generovaný kód *Startup.cs* souboru následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="7450a-137">Replace the generated code in the *Startup.cs* file with the following:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]
  
  <span data-ttu-id="7450a-138">`app.Use` Výraz lambda se použije k registraci zadané middleware součást do kanálu OWIN.</span><span class="sxs-lookup"><span data-stu-id="7450a-138">The `app.Use` lambda expression is used to register the specified middleware component to the OWIN pipeline.</span></span> <span data-ttu-id="7450a-139">V tomto případě nastavíme protokolování příchozích požadavků před reagovat na příchozí požadavek.</span><span class="sxs-lookup"><span data-stu-id="7450a-139">In this case we are setting up logging of incoming requests before responding to the incoming request.</span></span> <span data-ttu-id="7450a-140">`next` Parametrem je delegát ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [úloh](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) k další komponenta v kanálu.</span><span class="sxs-lookup"><span data-stu-id="7450a-140">The `next` parameter is the delegate ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) to the next component in the pipeline.</span></span> <span data-ttu-id="7450a-141">`app.Run` Lambda výraz zachytí vytvořit kanál na příchozí požadavky a poskytuje mechanismus odpovědi.</span><span class="sxs-lookup"><span data-stu-id="7450a-141">The `app.Run` lambda expression hooks up the pipeline to incoming requests and provides the response mechanism.</span></span>
     > [!NOTE]
     > <span data-ttu-id="7450a-142">Ve výše uvedeném kódu, budeme mít komentář `OwinStartup` atribut a My se spoléhat na konvenci spuštěných třídu s názvem `Startup` .-stiskněte ***F5*** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="7450a-142">In the code above we have commented out the `OwinStartup` attribute and we're relying on the convention of running the class named `Startup` .- Press ***F5*** to run the application.</span></span> <span data-ttu-id="7450a-143">Stiskněte několikrát tlačítko Aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="7450a-143">Hit refresh a few times.</span></span>  
  
    ![](owin-startup-class-detection/_static/image4.png)  
  <span data-ttu-id="7450a-144">Poznámka: Na číslo zobrazené na obrázcích v tomto kurzu nebudou odpovídat na číslo uvedené.</span><span class="sxs-lookup"><span data-stu-id="7450a-144">Note: The number shown in the images in this tutorial will not match the number you see.</span></span> <span data-ttu-id="7450a-145">Milisekundy řetězec se používá k zobrazit nová odpověď, když obnovíte stránku.</span><span class="sxs-lookup"><span data-stu-id="7450a-145">The millisecond string is used to show a new response when you refresh the page.</span></span>  
  <span data-ttu-id="7450a-146">Zobrazí se informace o trasování v **výstup** okna.</span><span class="sxs-lookup"><span data-stu-id="7450a-146">You can see the trace information in the **Output** window.</span></span>  
  
    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a><span data-ttu-id="7450a-147">Přidat další spuštění třídy</span><span class="sxs-lookup"><span data-stu-id="7450a-147">Add More Startup Classes</span></span>

<span data-ttu-id="7450a-148">V této části přidáme další spuštění třídu.</span><span class="sxs-lookup"><span data-stu-id="7450a-148">In this section we'll add another Startup class.</span></span> <span data-ttu-id="7450a-149">Do vaší aplikace můžete přidat více třídy pro spuštění OWIN.</span><span class="sxs-lookup"><span data-stu-id="7450a-149">You can add multiple OWIN startup class to your application.</span></span> <span data-ttu-id="7450a-150">Například můžete chtít vytvořit spouštěcí třídy pro vývoj, testování a produkci.</span><span class="sxs-lookup"><span data-stu-id="7450a-150">For example, you might want to create startup classes for development, testing and production.</span></span>

1. <span data-ttu-id="7450a-151">Vytvořte novou třídu spuštění OWIN s názvem `ProductionStartup`.</span><span class="sxs-lookup"><span data-stu-id="7450a-151">Create a new OWIN Startup class and name it `ProductionStartup`.</span></span>
2. <span data-ttu-id="7450a-152">Generovaného kódu nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="7450a-152">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. <span data-ttu-id="7450a-153">Stisknutím klávesy F5 spusťte aplikaci ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="7450a-153">Press Control F5 to run the app.</span></span> <span data-ttu-id="7450a-154">`OwinStartup` Atribut určuje třídu pro spuštění produkčního prostředí běží.</span><span class="sxs-lookup"><span data-stu-id="7450a-154">The `OwinStartup` attribute specifies the production startup class is run.</span></span>  
  
    ![](owin-startup-class-detection/_static/image6.png)
4. <span data-ttu-id="7450a-155">Vytvořte jinou třídu pro spuštění OWIN s názvem `TestStartup`.</span><span class="sxs-lookup"><span data-stu-id="7450a-155">Create another OWIN Startup class and name it `TestStartup`.</span></span>
5. <span data-ttu-id="7450a-156">Generovaného kódu nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="7450a-156">Replace the generated code with the following:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   <span data-ttu-id="7450a-157">`OwinStartup` Určuje atribut přetížení výše `TestingConfiguration` jako *popisný* název třída při spuštění.</span><span class="sxs-lookup"><span data-stu-id="7450a-157">The `OwinStartup` attribute overload above specifies `TestingConfiguration` as the *friendly* name of the Startup class.</span></span>
6. <span data-ttu-id="7450a-158">Otevřít *web.config* a přidejte spouštěcí klíč aplikace OWIN, který určuje popisný název třídy spuštění:</span><span class="sxs-lookup"><span data-stu-id="7450a-158">Open the *web.config* file and add the OWIN App startup key which specifies the friendly name of the Startup class:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. <span data-ttu-id="7450a-159">Stisknutím klávesy F5 spusťte aplikaci ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="7450a-159">Press Control F5 to run the app.</span></span> <span data-ttu-id="7450a-160">Element nastavení aplikace trvá předcházejících a testovací konfigurace spuštění.</span><span class="sxs-lookup"><span data-stu-id="7450a-160">The app settings element takes precedent, and the test configuration is run.</span></span>  
  
    ![](owin-startup-class-detection/_static/image7.png)
8. <span data-ttu-id="7450a-161">Odeberte *popisný* název z `OwinStartup` atribut `TestStartup` třídy.</span><span class="sxs-lookup"><span data-stu-id="7450a-161">Remove the *friendly* name from the `OwinStartup` attribute in the `TestStartup` class.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. <span data-ttu-id="7450a-162">Nahradit spouštěcí klíč aplikace OWIN v *web.config* souboru následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="7450a-162">Replace the OWIN App startup key in the *web.config* file with the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. <span data-ttu-id="7450a-163">Vrátit zpět `OwinStartup` atribut v každé třídě atribut výchozího kódu generovaný sady Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="7450a-163">Revert the `OwinStartup` attribute in each class to the default attribute code generated by Visual Studio:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    <span data-ttu-id="7450a-164">Všechny klíče aplikace OWIN při spuštění níže způsobí, že třída produkčního prostředí pro spuštění.</span><span class="sxs-lookup"><span data-stu-id="7450a-164">Each of the OWIN App startup keys below will cause the production class to run.</span></span> 

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    <span data-ttu-id="7450a-165">Poslední spouštěcí klíč určuje metodu spouštěný konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7450a-165">The last startup key specifies the startup configuration method.</span></span> <span data-ttu-id="7450a-166">Následující spouštěcí klíč aplikace OWIN umožňuje změnit název třídy konfigurace má `MyConfiguration` .</span><span class="sxs-lookup"><span data-stu-id="7450a-166">The following OWIN App startup key allows you to change the name of the configuration class to `MyConfiguration` .</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a><span data-ttu-id="7450a-167">Pomocí Owinhost.exe</span><span class="sxs-lookup"><span data-stu-id="7450a-167">Using Owinhost.exe</span></span>

1. <span data-ttu-id="7450a-168">Nahradíte soubor Web.config následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="7450a-168">Replace the Web.config file with the following markup:</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   <span data-ttu-id="7450a-169">Klíč poslední služby wins, tak v tomto případě `TestStartup` je zadán.</span><span class="sxs-lookup"><span data-stu-id="7450a-169">The last key wins, so in this case `TestStartup` is specified.</span></span>
2. <span data-ttu-id="7450a-170">Nainstalujte z konzole PMC Owinhost:</span><span class="sxs-lookup"><span data-stu-id="7450a-170">Install Owinhost from the PMC:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. <span data-ttu-id="7450a-171">Přejděte do složky aplikace (složku obsahující *Web.config* soubor) a příkazový řádek a zadejte:</span><span class="sxs-lookup"><span data-stu-id="7450a-171">Navigate to the application folder (the folder containing the *Web.config* file) and in a command prompt and type:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   <span data-ttu-id="7450a-172">Příkazové okno se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="7450a-172">The command window will show:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. <span data-ttu-id="7450a-173">Spustit prohlížeč s adresou URL `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="7450a-173">Launch a browser with the URL `http://localhost:5000/`.</span></span>  
  
    ![](owin-startup-class-detection/_static/image8.png)  
  
   <span data-ttu-id="7450a-174">OwinHost zachované při spuštění konvence uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="7450a-174">OwinHost honored the startup conventions listed above.</span></span>
5. <span data-ttu-id="7450a-175">V příkazovém okně stisknutím klávesy Enter ukončete OwinHost.</span><span class="sxs-lookup"><span data-stu-id="7450a-175">In the command window, press Enter to exit OwinHost.</span></span>
6. <span data-ttu-id="7450a-176">V `ProductionStartup` třídy, přidejte následující atribut OwinStartup, který určuje popisný název *ProductionConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="7450a-176">In the `ProductionStartup` class, add the following OwinStartup attribute which specifies a friendly name of *ProductionConfiguration*.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. <span data-ttu-id="7450a-177">Do příkazového řádku a zadejte:</span><span class="sxs-lookup"><span data-stu-id="7450a-177">In the command prompt and type:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   <span data-ttu-id="7450a-178">Třída při spuštění produkčního prostředí je načtena.</span><span class="sxs-lookup"><span data-stu-id="7450a-178">The Production startup class is loaded.</span></span>  
    ![](owin-startup-class-detection/_static/image9.png)  
   <span data-ttu-id="7450a-179">Naše aplikace má více spuštění třídy a v tomto příkladu jsme změnily která třída při spuštění načíst do modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="7450a-179">Our application has multiple startup classes, and in this example we have deferred which startup class to load until runtime.</span></span>
8. <span data-ttu-id="7450a-180">Následující možnosti modulu runtime při spuštění testů:</span><span class="sxs-lookup"><span data-stu-id="7450a-180">Test the following runtime startup options:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
