---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: "Detekce třídy pro spuštění OWIN | Microsoft Docs"
author: Praburaj
description: "Tento kurz ukazuje, jak nakonfigurovat, které třídy pro spuštění OWIN je načtena. Další informace o OWIN najdete v části Přehled Katana projektu. V tomto kurzu se..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: 618f8fa23630dcf9821a54415766dc015694e535
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="owin-startup-class-detection"></a><span data-ttu-id="17b0d-105">Detekce třídy pro spuštění OWIN</span><span class="sxs-lookup"><span data-stu-id="17b0d-105">OWIN Startup Class Detection</span></span>
====================
<span data-ttu-id="17b0d-106">podle [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="17b0d-106">by [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="17b0d-107">Tento kurz ukazuje, jak nakonfigurovat, které třídy pro spuštění OWIN je načtena.</span><span class="sxs-lookup"><span data-stu-id="17b0d-107">This tutorial shows how to configure which OWIN startup class is loaded.</span></span> <span data-ttu-id="17b0d-108">Další informace o OWIN najdete v tématu [Přehled projektu Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="17b0d-108">For more information on OWIN, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="17b0d-109">V tomto kurzu byla zapsána od Ricka Andersona ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan a Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).</span><span class="sxs-lookup"><span data-stu-id="17b0d-109">This tutorial was written by Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan, and Howard Dierking ( [@howard\_dierking](https://twitter.com/howard_dierking) ).</span></span>
> 
> ## <a name="prerequisites"></a><span data-ttu-id="17b0d-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="17b0d-110">Prerequisites</span></span>
> 
> [<span data-ttu-id="17b0d-111">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="17b0d-111">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)


## <a name="owin-startup-class-detection"></a><span data-ttu-id="17b0d-112">Detekce třídy pro spuštění OWIN</span><span class="sxs-lookup"><span data-stu-id="17b0d-112">OWIN Startup Class Detection</span></span>

 <span data-ttu-id="17b0d-113">Každá aplikace OWIN obsahuje třídu, spuštění, kde můžete určit komponenty pro kanálu aplikace.</span><span class="sxs-lookup"><span data-stu-id="17b0d-113">Every OWIN Application has a startup class where you specify components for the application pipeline.</span></span> <span data-ttu-id="17b0d-114">Existují různé způsoby připojíte třídě spuštění s modulem runtime, v závislosti na hostování modelu, které zvolíte (OwinHost, IIS a služby IIS Express).</span><span class="sxs-lookup"><span data-stu-id="17b0d-114">There are different ways you can connect your startup class with the runtime, depending on the hosting model you choose (OwinHost, IIS, and IIS-Express).</span></span> <span data-ttu-id="17b0d-115">Třída při spuštění uvedené v tomto kurzu lze použít v každé hostitelskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="17b0d-115">The startup class shown in this tutorial can be used in every hosting application.</span></span> <span data-ttu-id="17b0d-116">Třída při spuštění se připojit pomocí hostování modulu runtime, který se blíží mezi tyto:</span><span class="sxs-lookup"><span data-stu-id="17b0d-116">You connect the startup class with the hosting runtime using one of the these approaches:</span></span>  

1. <span data-ttu-id="17b0d-117">**Zásady vytváření názvů**: Katana hledá třídy s názvem `Startup` v oboru názvů odpovídající název sestavení nebo obor názvů globální.</span><span class="sxs-lookup"><span data-stu-id="17b0d-117">**Naming Convention**: Katana looks for a class named `Startup` in namespace matching the assembly name or the global namespace.</span></span>
2. <span data-ttu-id="17b0d-118">**Atribut OwinStartup**: Jedná se o postup Většina vývojářů bude trvat zadejte třída při spuštění.</span><span class="sxs-lookup"><span data-stu-id="17b0d-118">**OwinStartup Attribute**: This is the approach most developers will take to specify the startup class.</span></span> <span data-ttu-id="17b0d-119">Následující atribut nastaví třída při spuštění `TestStartup` třídy v `StartupDemo` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="17b0d-119">The following attribute will set the startup class to the `TestStartup` class in the `StartupDemo` namespace.</span></span> 

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

 <span data-ttu-id="17b0d-120">`OwinStartup` Atribut přepsání zásady vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="17b0d-120">The `OwinStartup` attribute overrides the naming convention.</span></span> <span data-ttu-id="17b0d-121">Také můžete zadat popisný název k tomuto atributu, ale pomocí popisný název vyžaduje, abyste také použít `appSetting` element v konfiguračním souboru.</span><span class="sxs-lookup"><span data-stu-id="17b0d-121">You can also specify a friendly name with this attribute, however, using a friendly name requires you to also use the `appSetting` element in the configuration file.</span></span>
3. <span data-ttu-id="17b0d-122">**Element appSetting v konfiguračním souboru**: `appSetting` element přepsání `OwinStartup` atribut a zásady vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="17b0d-122">**The appSetting element in the Configuration file**: The `appSetting` element overrides the `OwinStartup` attribute and naming convention.</span></span> <span data-ttu-id="17b0d-123">Může mít několik tříd spuštění (každý využívající `OwinStartup` atribut) a konfigurace, které třída při spuštění bude načten do konfiguračního souboru pomocí značek podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="17b0d-123">You can have multiple startup classes (each using an `OwinStartup` attribute) and configure which startup class will be loaded in a configuration file using markup similar to the following:</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

 <span data-ttu-id="17b0d-124">Následující klíč, který explicitně určuje třídy pro spuštění a sestavení lze také použít:</span><span class="sxs-lookup"><span data-stu-id="17b0d-124">The following key, which explicitly specifies the startup class and assembly can also be used:</span></span> 

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

 <span data-ttu-id="17b0d-125">Následující kód XML v konfiguračním souboru Určuje název třídy popisný spuštění `ProductionConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="17b0d-125">The following XML in the configuration file specifies a friendly startup class name of `ProductionConfiguration`.</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

 <span data-ttu-id="17b0d-126">Výše uvedený kód musí být použit s následující `OwinStartup` atribut, který určuje popisný název a způsobí, že `ProductionStartup2` třídy ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="17b0d-126">The above markup must be used with the following `OwinStartup` attribute which specifies a friendly name and causes the `ProductionStartup2` class to run.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. <span data-ttu-id="17b0d-127">Chcete-li zakázat zjišťování OWIN při spuštění přidejte `appSetting owin:AutomaticAppStartup` s hodnotou `"false"` v souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="17b0d-127">To disable OWIN startup discovery add the `appSetting owin:AutomaticAppStartup` with a value of `"false"` in the web.config file.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a><span data-ttu-id="17b0d-128">Vytvoření webové aplikace ASP.NET pomocí OWIN při spuštění</span><span class="sxs-lookup"><span data-stu-id="17b0d-128">Create an ASP.NET Web App using OWIN Startup</span></span>

1. <span data-ttu-id="17b0d-129">Vytvořit prázdnou webovou aplikaci Asp.Net a pojmenujte ji **StartupDemo**.</span><span class="sxs-lookup"><span data-stu-id="17b0d-129">Create an empty Asp.Net web application and name it **StartupDemo**.</span></span> <span data-ttu-id="17b0d-130">-Instalovat `Microsoft.Owin.Host.SystemWeb` pomocí Správce balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="17b0d-130">- Install `Microsoft.Owin.Host.SystemWeb` using the NuGet package manager.</span></span> <span data-ttu-id="17b0d-131">Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**a potom **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="17b0d-131">From the **Tools** menu, select **Library Package Manager**, and then **Package Manager Console**.</span></span> <span data-ttu-id="17b0d-132">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="17b0d-132">Enter the following command:</span></span>  

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
- <span data-ttu-id="17b0d-133">Přidání třídy pro spuštění OWIN.</span><span class="sxs-lookup"><span data-stu-id="17b0d-133">Add an OWIN startup class.</span></span> <span data-ttu-id="17b0d-134">V sadě Visual Studio 2013 klikněte pravým tlačítkem na projekt a vyberte **přidat třídu**. - v **přidat novou položku** dialogové okno zadejte *OWIN* do pole hledání a změňte název na Startup.cs, a pak klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="17b0d-134">In Visual Studio 2013 right click the project and select **Add Class**.- In the **Add New Item** dialog box, enter *OWIN* in the search field, and change the name to Startup.cs, and then click **Add**.</span></span>  
  
    ![](owin-startup-class-detection/_static/image1.png)   
  
 <span data-ttu-id="17b0d-135">Další čas, který chcete přidat *třídy pro spuštění Owin*, bude ve dostupné z **přidat** nabídky.</span><span class="sxs-lookup"><span data-stu-id="17b0d-135">The next time you want to add an *Owin Startup class*, it will be in available from the **Add** menu.</span></span>  
   
    ![](owin-startup-class-detection/_static/image2.png)  
  
 <span data-ttu-id="17b0d-136">Alternativně můžete klikněte pravým tlačítkem na projekt a vyberte **přidat**, pak vyberte **nová položka**a pak vyberte **třídy pro spuštění Owin**.</span><span class="sxs-lookup"><span data-stu-id="17b0d-136">Alternatively, you can right click the project and select **Add**, then select **New Item**, and then select the **Owin Startup class**.</span></span>  
  
    ![](owin-startup-class-detection/_static/image3.png)  
  
- <span data-ttu-id="17b0d-137">Nahraďte generovaného kódu v *Startup.cs* soubor s následující:</span><span class="sxs-lookup"><span data-stu-id="17b0d-137">Replace the generated code in the *Startup.cs* file with the following:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]
  
 <span data-ttu-id="17b0d-138">`app.Use` Výrazu lambda slouží k registraci komponenty zadaný middleware do kanálu OWIN.</span><span class="sxs-lookup"><span data-stu-id="17b0d-138">The `app.Use` lambda expression is used to register the specified middleware component to the OWIN pipeline.</span></span> <span data-ttu-id="17b0d-139">V takovém případě nastavujeme protokolování příchozích požadavků před reagovat na příchozí požadavek.</span><span class="sxs-lookup"><span data-stu-id="17b0d-139">In this case we are setting up logging of incoming requests before responding to the incoming request.</span></span> <span data-ttu-id="17b0d-140">`next` Parametr je delegáta ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [úloh](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) další komponentou v kanálu.</span><span class="sxs-lookup"><span data-stu-id="17b0d-140">The `next` parameter is the delegate ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) to the next component in the pipeline.</span></span> <span data-ttu-id="17b0d-141">`app.Run` Výrazu lambda zachytí až kanálu na příchozí požadavky a poskytuje mechanismus odpovědi.</span><span class="sxs-lookup"><span data-stu-id="17b0d-141">The `app.Run` lambda expression hooks up the pipeline to incoming requests and provides the response mechanism.</span></span>
     > [!NOTE]
     > <span data-ttu-id="17b0d-142">Ve výše uvedeném kódu, budeme mít komentované `OwinStartup` atribut a My se spoléhat na konvence spuštěných třída s názvem `Startup` .-stiskněte ***F5*** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="17b0d-142">In the code above we have commented out the `OwinStartup` attribute and we're relying on the convention of running the class named `Startup` .- Press ***F5*** to run the application.</span></span> <span data-ttu-id="17b0d-143">Dosáhl aktualizace několikrát.</span><span class="sxs-lookup"><span data-stu-id="17b0d-143">Hit refresh a few times.</span></span>  
  
    ![](owin-startup-class-detection/_static/image4.png)  
<span data-ttu-id="17b0d-144">Poznámka: Na číslo zobrazené v obrázcích v tomto kurzu nebudou odpovídat na číslo se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="17b0d-144">Note: The number shown in the images in this tutorial will not match the number you see.</span></span> <span data-ttu-id="17b0d-145">Milisekundu řetězec se používá k zobrazení novou odpověď, když obnovíte stránku.</span><span class="sxs-lookup"><span data-stu-id="17b0d-145">The millisecond string is used to show a new response when you refresh the page.</span></span>  
 <span data-ttu-id="17b0d-146">Zobrazí se informace trasování v **výstup** okno.</span><span class="sxs-lookup"><span data-stu-id="17b0d-146">You can see the trace information in the **Output** window.</span></span>  
  
    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a><span data-ttu-id="17b0d-147">Přidat další třídy spuštění</span><span class="sxs-lookup"><span data-stu-id="17b0d-147">Add More Startup Classes</span></span>

<span data-ttu-id="17b0d-148">V této části přidáme jiné třídy spuštění.</span><span class="sxs-lookup"><span data-stu-id="17b0d-148">In this section we'll add another Startup class.</span></span> <span data-ttu-id="17b0d-149">Můžete přidat více třídy pro spuštění OWIN do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="17b0d-149">You can add multiple OWIN startup class to your application.</span></span> <span data-ttu-id="17b0d-150">Můžete například vytvořit spuštění třídy pro vývoj, testování a produkci.</span><span class="sxs-lookup"><span data-stu-id="17b0d-150">For example, you might want to create startup classes for development, testing and production.</span></span>

1. <span data-ttu-id="17b0d-151">Vytvořte novou třídu OWIN při spuštění a pojmenujte ji `ProductionStartup`.</span><span class="sxs-lookup"><span data-stu-id="17b0d-151">Create a new OWIN Startup class and name it `ProductionStartup`.</span></span>
2. <span data-ttu-id="17b0d-152">Generovaného kódu nahraďte následujícím textem:</span><span class="sxs-lookup"><span data-stu-id="17b0d-152">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. <span data-ttu-id="17b0d-153">Stisknutím klávesy F5 řízení a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="17b0d-153">Press Control F5 to run the app.</span></span> <span data-ttu-id="17b0d-154">`OwinStartup` Určuje atribut třída při spuštění produkční běží.</span><span class="sxs-lookup"><span data-stu-id="17b0d-154">The `OwinStartup` attribute specifies the production startup class is run.</span></span>  
  
    ![](owin-startup-class-detection/_static/image6.png)
4. <span data-ttu-id="17b0d-155">Vytvořte další OWIN při spuštění třídu s názvem `TestStartup`.</span><span class="sxs-lookup"><span data-stu-id="17b0d-155">Create another OWIN Startup class and name it `TestStartup`.</span></span>
5. <span data-ttu-id="17b0d-156">Generovaného kódu nahraďte následujícím textem:</span><span class="sxs-lookup"><span data-stu-id="17b0d-156">Replace the generated code with the following:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

 <span data-ttu-id="17b0d-157">`OwinStartup` Určuje atribut přetížení výše `TestingConfiguration` jako *popisný* název třída při spuštění.</span><span class="sxs-lookup"><span data-stu-id="17b0d-157">The `OwinStartup` attribute overload above specifies `TestingConfiguration` as the *friendly* name of the Startup class.</span></span>
6. <span data-ttu-id="17b0d-158">Otevřete *web.config* souboru a přidejte spouštěcí klíč aplikace OWIN, který určuje popisný název třídy spuštění:</span><span class="sxs-lookup"><span data-stu-id="17b0d-158">Open the *web.config* file and add the OWIN App startup key which specifies the friendly name of the Startup class:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. <span data-ttu-id="17b0d-159">Stisknutím klávesy F5 řízení a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="17b0d-159">Press Control F5 to run the app.</span></span> <span data-ttu-id="17b0d-160">Element nastavení aplikace trvá předcházejících a testovací konfigurace běží.</span><span class="sxs-lookup"><span data-stu-id="17b0d-160">The app settings element takes precedent, and the test configuration is run.</span></span>  
  
    ![](owin-startup-class-detection/_static/image7.png)
8. <span data-ttu-id="17b0d-161">Odebrat *popisný* název z `OwinStartup` atribut `TestStartup` třídy.</span><span class="sxs-lookup"><span data-stu-id="17b0d-161">Remove the *friendly* name from the `OwinStartup` attribute in the `TestStartup` class.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. <span data-ttu-id="17b0d-162">Nahradit spouštěcí klíč aplikace OWIN v *web.config* soubor s následující:</span><span class="sxs-lookup"><span data-stu-id="17b0d-162">Replace the OWIN App startup key in the *web.config* file with the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. <span data-ttu-id="17b0d-163">Vrátit zpět `OwinStartup` atribut v každé třídě ve výchozím kódu atribut vytvořen sadou Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="17b0d-163">Revert the `OwinStartup` attribute in each class to the default attribute code generated by Visual Studio:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

 <span data-ttu-id="17b0d-164">Všechny tyto klíče aplikace OWIN při spuštění způsobí, že třída produkční ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="17b0d-164">Each of the OWIN App startup keys below will cause the production class to run.</span></span> 

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

 <span data-ttu-id="17b0d-165">Klíč poslední spuštění určuje metodu spuštění konfigurace.</span><span class="sxs-lookup"><span data-stu-id="17b0d-165">The last startup key specifies the startup configuration method.</span></span> <span data-ttu-id="17b0d-166">Následující spouštěcí klíč OWIN aplikace můžete změnit název třídy pro konfiguraci `MyConfiguration` .</span><span class="sxs-lookup"><span data-stu-id="17b0d-166">The following OWIN App startup key allows you to change the name of the configuration class to `MyConfiguration` .</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a><span data-ttu-id="17b0d-167">Pomocí Owinhost.exe</span><span class="sxs-lookup"><span data-stu-id="17b0d-167">Using Owinhost.exe</span></span>

1. <span data-ttu-id="17b0d-168">Nahraďte v souboru Web.config následující kód:</span><span class="sxs-lookup"><span data-stu-id="17b0d-168">Replace the Web.config file with the following markup:</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

 <span data-ttu-id="17b0d-169">Klíč poslední služby wins, tak v tomto případě `TestStartup` je zadán.</span><span class="sxs-lookup"><span data-stu-id="17b0d-169">The last key wins, so in this case `TestStartup` is specified.</span></span>
2. <span data-ttu-id="17b0d-170">Nainstalujte Owinhost z pomocí PMC:</span><span class="sxs-lookup"><span data-stu-id="17b0d-170">Install Owinhost from the PMC:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. <span data-ttu-id="17b0d-171">Přejděte do složky aplikace (složku, která obsahuje *Web.config* souboru) a na příkazový řádek a zadejte:</span><span class="sxs-lookup"><span data-stu-id="17b0d-171">Navigate to the application folder (the folder containing the *Web.config* file) and in a command prompt and type:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

 <span data-ttu-id="17b0d-172">Příkazové okno se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="17b0d-172">The command window will show:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. <span data-ttu-id="17b0d-173">Spustit prohlížeč s adresou URL `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="17b0d-173">Launch a browser with the URL `http://localhost:5000/`.</span></span>  
  
    ![](owin-startup-class-detection/_static/image8.png)  
  
 <span data-ttu-id="17b0d-174">OwinHost dodržení konvence spuštění uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="17b0d-174">OwinHost honored the startup conventions listed above.</span></span>
5. <span data-ttu-id="17b0d-175">V příkazovém okně stisknutím klávesy Enter ukončete OwinHost.</span><span class="sxs-lookup"><span data-stu-id="17b0d-175">In the command window, press Enter to exit OwinHost.</span></span>
6. <span data-ttu-id="17b0d-176">V `ProductionStartup` třídy, přidejte následující atribut OwinStartup, který určuje popisný název *ProductionConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="17b0d-176">In the `ProductionStartup` class, add the following OwinStartup attribute which specifies a friendly name of *ProductionConfiguration*.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. <span data-ttu-id="17b0d-177">Do příkazového řádku a zadejte:</span><span class="sxs-lookup"><span data-stu-id="17b0d-177">In the command prompt and type:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

 <span data-ttu-id="17b0d-178">Třída při spuštění produkční je načten.</span><span class="sxs-lookup"><span data-stu-id="17b0d-178">The Production startup class is loaded.</span></span>  
    ![](owin-startup-class-detection/_static/image9.png)  
 <span data-ttu-id="17b0d-179">Naše aplikace má více tříd, spuštění a v tomto příkladu jsme mají odložené které třída při spuštění načíst do modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="17b0d-179">Our application has multiple startup classes, and in this example we have deferred which startup class to load until runtime.</span></span>
8. <span data-ttu-id="17b0d-180">Testovací následující možnosti při spuštění modulu runtime:</span><span class="sxs-lookup"><span data-stu-id="17b0d-180">Test the following runtime startup options:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
