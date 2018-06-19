---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: Trasování v rozhraní ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Ukazuje, jak povolit trasování v rozhraní ASP.NET Web API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7392ae5d9bc4c3aab45a9373099a0ee18e873a4f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
ms.locfileid: "28044205"
---
<a name="tracing-in-aspnet-web-api-2"></a><span data-ttu-id="db2b8-103">Trasování v rozhraní ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="db2b8-103">Tracing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="db2b8-104">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="db2b8-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="db2b8-105">Když se pokoušíte ladění webové aplikace, neexistuje žádný nenahrazuje sadu dobrou protokolů trasování.</span><span class="sxs-lookup"><span data-stu-id="db2b8-105">When you are trying to debug a web-based application, there is no substitute for a good set of trace logs.</span></span> <span data-ttu-id="db2b8-106">Tento kurz ukazuje, jak povolit trasování v rozhraní ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="db2b8-106">This tutorial shows how to enable tracing in ASP.NET Web API.</span></span> <span data-ttu-id="db2b8-107">Tato funkce slouží ke sledování, co dělají rozhraní Web API před a po vyvolá řadiči.</span><span class="sxs-lookup"><span data-stu-id="db2b8-107">You can use this feature to trace what the Web API framework does before and after it invokes your controller.</span></span> <span data-ttu-id="db2b8-108">Můžete ji použít i ke sledování vlastní kód.</span><span class="sxs-lookup"><span data-stu-id="db2b8-108">You can also use it to trace your own code.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="db2b8-109">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="db2b8-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="db2b8-110">[Visual Studio 2017](https://www.visualstudio.com/downloads/) (funguje taky s Visual Studiem 2015)</span><span class="sxs-lookup"><span data-stu-id="db2b8-110">[Visual Studio 2017](https://www.visualstudio.com/downloads/) (also works with Visual Studio 2015)</span></span>
> - <span data-ttu-id="db2b8-111">Webové rozhraní API 2</span><span class="sxs-lookup"><span data-stu-id="db2b8-111">Web API 2</span></span>
> - [<span data-ttu-id="db2b8-112">Microsoft.AspNet.WebApi.Tracing</span><span class="sxs-lookup"><span data-stu-id="db2b8-112">Microsoft.AspNet.WebApi.Tracing</span></span>](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)


## <a name="enable-systemdiagnostics-tracing-in-web-api"></a><span data-ttu-id="db2b8-113">Povolit System.Diagnostics trasování v rozhraní Web API</span><span class="sxs-lookup"><span data-stu-id="db2b8-113">Enable System.Diagnostics Tracing in Web API</span></span>

<span data-ttu-id="db2b8-114">Nejdříve vytvoříme nový projekt webové aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="db2b8-114">First, we'll create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="db2b8-115">V sadě Visual Studio z **soubor** nabídce vyberte možnost **nový**, pak **projektu**.</span><span class="sxs-lookup"><span data-stu-id="db2b8-115">In Visual Studio, from the **File** menu, select **New**, then **Project**.</span></span> <span data-ttu-id="db2b8-116">V části **šablony**, **webové**, vyberte **webové aplikace ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="db2b8-116">Under **Templates**, **Web**, select **ASP.NET Web Application**.</span></span>

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="db2b8-117">Výběr šablony projektu webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="db2b8-117">Choose the Web API project template.</span></span>

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

<span data-ttu-id="db2b8-118">Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**, pak **konzoly Správa balíčků**.</span><span class="sxs-lookup"><span data-stu-id="db2b8-118">From the **Tools** menu, select **Library Package Manager**, then **Package Manage Console**.</span></span>

<span data-ttu-id="db2b8-119">V okně konzoly Správce balíčků zadejte následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="db2b8-119">In the Package Manager Console window, type the following commands.</span></span>

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

<span data-ttu-id="db2b8-120">První příkaz nainstaluje nejnovější balíček trasování webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="db2b8-120">The first command installs the latest Web API tracing package.</span></span> <span data-ttu-id="db2b8-121">Aktualizuje také balíčky webového rozhraní API core.</span><span class="sxs-lookup"><span data-stu-id="db2b8-121">It also updates the core Web API packages.</span></span> <span data-ttu-id="db2b8-122">V druhém příkazu se WebApi.WebHost balíček aktualizuje na nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="db2b8-122">The second command updates the WebApi.WebHost package to the latest version.</span></span>

> [!NOTE]
> <span data-ttu-id="db2b8-123">Pokud chcete zacílit na konkrétní verzi rozhraní Web API, použijte příznak-verze při instalaci balíčku trasování.</span><span class="sxs-lookup"><span data-stu-id="db2b8-123">If you want to target a specific version of Web API, use the -Version flag when you install the tracing package.</span></span>


<span data-ttu-id="db2b8-124">Otevřete soubor WebApiConfig.cs v aplikaci\_spouštěcí složka.</span><span class="sxs-lookup"><span data-stu-id="db2b8-124">Open the file WebApiConfig.cs in the App\_Start folder.</span></span> <span data-ttu-id="db2b8-125">Přidejte následující kód, který **zaregistrovat** metoda.</span><span class="sxs-lookup"><span data-stu-id="db2b8-125">Add the following code to the **Register** method.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

<span data-ttu-id="db2b8-126">Tento kód přidá [zapisovač SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) třída do kanálu webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="db2b8-126">This code adds the [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) class to the Web API pipeline.</span></span> <span data-ttu-id="db2b8-127">**Zapisovač SystemDiagnosticsTraceWriter** zapíše trasování do třídy [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).</span><span class="sxs-lookup"><span data-stu-id="db2b8-127">The **SystemDiagnosticsTraceWriter** class writes traces to [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).</span></span>

<span data-ttu-id="db2b8-128">Pokud chcete zobrazit trasování, spusťte aplikaci v ladicím programu.</span><span class="sxs-lookup"><span data-stu-id="db2b8-128">To see the traces, run the application in the debugger.</span></span> <span data-ttu-id="db2b8-129">V prohlížeči přejděte na `/api/values`.</span><span class="sxs-lookup"><span data-stu-id="db2b8-129">In the browser, navigate to `/api/values`.</span></span>

![](tracing-in-aspnet-web-api/_static/image5.png)

<span data-ttu-id="db2b8-130">Příkazy trasování se zapisují do okna výstupu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="db2b8-130">The trace statements are written to the Output window in Visual Studio.</span></span> <span data-ttu-id="db2b8-131">(Z **zobrazení** nabídce vyberte možnost **výstup**).</span><span class="sxs-lookup"><span data-stu-id="db2b8-131">(From the **View** menu, select **Output**).</span></span>

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

<span data-ttu-id="db2b8-132">Protože **zapisovač SystemDiagnosticsTraceWriter** zapíše trasování do **System.Diagnostics.Trace**, můžete zaregistrovat další trasování – moduly naslouchání; například zapsat trasování do souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="db2b8-132">Because **SystemDiagnosticsTraceWriter** writes traces to **System.Diagnostics.Trace**, you can register additional trace listeners; for example, to write traces to a log file.</span></span> <span data-ttu-id="db2b8-133">Další informace o trasování zapisovače najdete v tématu [trasování – moduly naslouchání](https://msdn.microsoft.com/library/4y5y10s7.aspx) tématu na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="db2b8-133">For more information about trace writers, see the [Trace Listeners](https://msdn.microsoft.com/library/4y5y10s7.aspx) topic on MSDN.</span></span>

### <a name="configuring-systemdiagnosticstracewriter"></a><span data-ttu-id="db2b8-134">Konfigurace zapisovač SystemDiagnosticsTraceWriter</span><span class="sxs-lookup"><span data-stu-id="db2b8-134">Configuring SystemDiagnosticsTraceWriter</span></span>

<span data-ttu-id="db2b8-135">Následující kód ukazuje, jak konfigurovat zápis trasování.</span><span class="sxs-lookup"><span data-stu-id="db2b8-135">The following code shows how to configure the trace writer.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="db2b8-136">Existují dvě nastavení, které můžete řídit:</span><span class="sxs-lookup"><span data-stu-id="db2b8-136">There are two settings that you can control:</span></span>

- <span data-ttu-id="db2b8-137">IsVerbose: Pokud je hodnota false, obsahuje každý trasování minimální informace.</span><span class="sxs-lookup"><span data-stu-id="db2b8-137">IsVerbose: If false, each trace contains minimal information.</span></span> <span data-ttu-id="db2b8-138">V případě hodnoty true trasování obsahují další informace.</span><span class="sxs-lookup"><span data-stu-id="db2b8-138">If true, traces include more information.</span></span>
- <span data-ttu-id="db2b8-139">MinimumLevel: Nastaví minimální úroveň trasování.</span><span class="sxs-lookup"><span data-stu-id="db2b8-139">MinimumLevel: Sets the minimum trace level.</span></span> <span data-ttu-id="db2b8-140">Úrovně trasování v pořadí, jsou ladění, informace, varování, chyba a závažná chyba.</span><span class="sxs-lookup"><span data-stu-id="db2b8-140">Trace levels, in order, are Debug, Info, Warn, Error, and Fatal.</span></span>

## <a name="adding-traces-to-your-web-api-application"></a><span data-ttu-id="db2b8-141">Přidání trasování do vaší aplikace webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="db2b8-141">Adding Traces to Your Web API Application</span></span>

<span data-ttu-id="db2b8-142">Přidání zapisovač trasování umožňuje okamžitý přístup k trasování vytvořený kanál rozhraní Web API.</span><span class="sxs-lookup"><span data-stu-id="db2b8-142">Adding a trace writer gives you immediate access to the traces created by the Web API pipeline.</span></span> <span data-ttu-id="db2b8-143">Můžete taky zapisovač trasování pro trasování vlastní kód:</span><span class="sxs-lookup"><span data-stu-id="db2b8-143">You can also use the trace writer to trace your own code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="db2b8-144">Chcete-li získat zapisovač trasování, volejte **HttpConfiguration.Services.GetTraceWriter**.</span><span class="sxs-lookup"><span data-stu-id="db2b8-144">To get the trace writer, call **HttpConfiguration.Services.GetTraceWriter**.</span></span> <span data-ttu-id="db2b8-145">Z řadiče, tato metoda je přístupná prostřednictvím **ApiController.Configuration** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="db2b8-145">From a controller, this method is accessible through the **ApiController.Configuration** property.</span></span>

<span data-ttu-id="db2b8-146">Pokud chcete zapsat trasování, můžete zavolat **ITraceWriter.Trace** metoda přímo, ale [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) třída definuje některé rozšiřující metody, které jsou přehlednější.</span><span class="sxs-lookup"><span data-stu-id="db2b8-146">To write a trace, you can call the **ITraceWriter.Trace** method directly, but the [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) class defines some extension methods that are more friendly.</span></span> <span data-ttu-id="db2b8-147">Například **informace** výše uvedená metoda vytvoří trasování s úroveň trasování **informace**.</span><span class="sxs-lookup"><span data-stu-id="db2b8-147">For example, the **Info** method shown above creates a trace with trace level **Info**.</span></span>

## <a name="web-api-tracing-infrastructure"></a><span data-ttu-id="db2b8-148">Infrastruktura trasování webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="db2b8-148">Web API Tracing Infrastructure</span></span>

<span data-ttu-id="db2b8-149">Tato část popisuje, jak psát vlastní trasování zapisovač pro webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="db2b8-149">This section describes how to write a custom trace writer for Web API.</span></span>

<span data-ttu-id="db2b8-150">Balíček Microsoft.AspNet.WebApi.Tracing je postavená na infrastruktuře obecnější trasování v rozhraní Web API.</span><span class="sxs-lookup"><span data-stu-id="db2b8-150">The Microsoft.AspNet.WebApi.Tracing package is built on top of a more general tracing infrastructure in Web API.</span></span> <span data-ttu-id="db2b8-151">Místo použití Microsoft.AspNet.WebApi.Tracing, můžete také zařadit některé další trasování nebo protokolování knihovny, například [NLog](http://nlog-project.org/) nebo [log4net](http://logging.apache.org/log4net/).</span><span class="sxs-lookup"><span data-stu-id="db2b8-151">Instead of using Microsoft.AspNet.WebApi.Tracing, you can also plug in some other tracing/loggin library, such as [NLog](http://nlog-project.org/) or [log4net](http://logging.apache.org/log4net/).</span></span>

<span data-ttu-id="db2b8-152">Chcete-li shromažďovat trasování, implementujte **ITraceWriter** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="db2b8-152">To collect traces, implement the **ITraceWriter** interface.</span></span> <span data-ttu-id="db2b8-153">Zde je jednoduchý příklad:</span><span class="sxs-lookup"><span data-stu-id="db2b8-153">Here is a simple example:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="db2b8-154">**ITraceWriter.Trace** metoda vytvoří trasování.</span><span class="sxs-lookup"><span data-stu-id="db2b8-154">The **ITraceWriter.Trace** method creates a trace.</span></span> <span data-ttu-id="db2b8-155">Volající určuje úroveň kategorie a trasování.</span><span class="sxs-lookup"><span data-stu-id="db2b8-155">The caller specifies a category and trace level.</span></span> <span data-ttu-id="db2b8-156">Kategorie může být libovolný řetězec definovaný uživatelem.</span><span class="sxs-lookup"><span data-stu-id="db2b8-156">The category can be any user-defined string.</span></span> <span data-ttu-id="db2b8-157">Vaši implementaci **trasování** udělat následující:</span><span class="sxs-lookup"><span data-stu-id="db2b8-157">Your implementation of **Trace** should do the following:</span></span>

1. <span data-ttu-id="db2b8-158">Vytvořte novou **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="db2b8-158">Create a new **TraceRecord**.</span></span> <span data-ttu-id="db2b8-159">Inicializujte požadavku, kategorii a úroveň trasování, jak je vidět.</span><span class="sxs-lookup"><span data-stu-id="db2b8-159">Initialize it with the request, category, and trace level, as shown.</span></span> <span data-ttu-id="db2b8-160">Tyto hodnoty jsou poskytovány volající.</span><span class="sxs-lookup"><span data-stu-id="db2b8-160">These values are provided by the caller.</span></span>
2. <span data-ttu-id="db2b8-161">Vyvolání *traceAction* delegovat.</span><span class="sxs-lookup"><span data-stu-id="db2b8-161">Invoke the *traceAction* delegate.</span></span> <span data-ttu-id="db2b8-162">Uvnitř tento delegát volající by měl vyplnit zbytek **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="db2b8-162">Inside this delegate, the caller is expected to fill in the rest of the **TraceRecord**.</span></span>
3. <span data-ttu-id="db2b8-163">Zápis **TraceRecord**, všechny technikou protokolování, který chcete.</span><span class="sxs-lookup"><span data-stu-id="db2b8-163">Write the **TraceRecord**, using any logging technique that you like.</span></span> <span data-ttu-id="db2b8-164">Příkladu jednoduché volání **System.Diagnostics.Trace**.</span><span class="sxs-lookup"><span data-stu-id="db2b8-164">The example shown here simply calls into **System.Diagnostics.Trace**.</span></span>

## <a name="setting-the-trace-writer"></a><span data-ttu-id="db2b8-165">Nastavení zapisovač trasování</span><span class="sxs-lookup"><span data-stu-id="db2b8-165">Setting the Trace Writer</span></span>

<span data-ttu-id="db2b8-166">Chcete-li povolit trasování, je nutné nakonfigurovat webového rozhraní API používat vaše **ITraceWriter** implementace.</span><span class="sxs-lookup"><span data-stu-id="db2b8-166">To enable tracing, you must configure Web API to use your **ITraceWriter** implementation.</span></span> <span data-ttu-id="db2b8-167">To provedete pomocí **HttpConfiguration** objektu, jak je znázorněno v následujícím kódu:</span><span class="sxs-lookup"><span data-stu-id="db2b8-167">You do this through the **HttpConfiguration** object, as shown in the following code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="db2b8-168">Zapisovač trasování pouze jeden můžou být aktivní.</span><span class="sxs-lookup"><span data-stu-id="db2b8-168">Only one trace writer can be active.</span></span> <span data-ttu-id="db2b8-169">Ve výchozím nastavení, nastaví webového rozhraní API &quot;no-op&quot; závislostí, který se nic nestane.</span><span class="sxs-lookup"><span data-stu-id="db2b8-169">By default, Web API sets a &quot;no-op&quot; tracer that does nothing.</span></span> <span data-ttu-id="db2b8-170">( &quot;No-op&quot; trasovací existuje, takže není nutné zkontrolovat, zda je zapisovač trasování trasování kódu **null** před zápisem trasování.)</span><span class="sxs-lookup"><span data-stu-id="db2b8-170">(The &quot;no-op&quot; tracer exists so that tracing code does not have to check whether the trace writer is **null** before writing a trace.)</span></span>

## <a name="how-web-api-tracing-works"></a><span data-ttu-id="db2b8-171">Jak webové rozhraní API funguje pro trasování</span><span class="sxs-lookup"><span data-stu-id="db2b8-171">How Web API Tracing Works</span></span>

<span data-ttu-id="db2b8-172">Trasování v webového rozhraní API používá používá v webového rozhraní API *průčelí za* vzor: Pokud je povoleno sledování, webového rozhraní API zabalí různých částí kanál požadavku s třídami, které provádějí trasovacího volání.</span><span class="sxs-lookup"><span data-stu-id="db2b8-172">Tracing in Web API uses a in Web API uses a *facade* pattern: When tracing is enabled, Web API wraps various parts of the request pipeline with classes that perform trace calls.</span></span>

<span data-ttu-id="db2b8-173">Například při výběru kontroleru, kanál používá **IHttpControllerSelector** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="db2b8-173">For example, when selecting a controller, the pipeline uses the **IHttpControllerSelector** interface.</span></span> <span data-ttu-id="db2b8-174">Pomocí trasování povoleno, pipleline vloží třídu, která implementuje **IHttpControllerSelector** ale volání skutečné implementace:</span><span class="sxs-lookup"><span data-stu-id="db2b8-174">With tracing enabled, the pipleline inserts a class that implements **IHttpControllerSelector** but calls through to the real implementation:</span></span>

![Trasování rozhraní Web API využívá vzor průčelí za.](tracing-in-aspnet-web-api/_static/image8.png)

<span data-ttu-id="db2b8-176">Výhody tohoto návrhu patří:</span><span class="sxs-lookup"><span data-stu-id="db2b8-176">The benefits of this design include:</span></span>

- <span data-ttu-id="db2b8-177">Pokud nepřidáte zapisovač trasování, trasování součásti nejsou vytvořena instance a mít žádný vliv na výkon.</span><span class="sxs-lookup"><span data-stu-id="db2b8-177">If you do not add a trace writer, the tracing components are not instantiated and have no performance impact.</span></span>
- <span data-ttu-id="db2b8-178">Pokud například nahradíte výchozích služeb **IHttpControllerSelector** s vlastními vlastní implementaci nemá vliv trasování, protože objekt obálky se provádí trasování.</span><span class="sxs-lookup"><span data-stu-id="db2b8-178">If you replace default services such as **IHttpControllerSelector** with your own custom implementation, tracing is not affected, because tracing is done by the wrapper object.</span></span>

<span data-ttu-id="db2b8-179">Můžete také nahradit celý framework trasování webového rozhraní API vlastní vlastní framework nahrazením výchozí **ITraceManager** služby:</span><span class="sxs-lookup"><span data-stu-id="db2b8-179">You can also replace the entire Web API trace framework with your own custom framework, by replacing the default **ITraceManager** service:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="db2b8-180">Implementace **ITraceManager.Initialize** k chybě při inicializaci systému trasování.</span><span class="sxs-lookup"><span data-stu-id="db2b8-180">Implement **ITraceManager.Initialize** to initialize your tracing system.</span></span> <span data-ttu-id="db2b8-181">Uvědomte si, že to nahrazuje *celý* framework trasování, včetně všech objektů kód trasování, která je integrována do webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="db2b8-181">Be aware that this replaces the *entire* trace framework, including all of the tracing code that is built into Web API.</span></span>
