---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: Trasování v rozhraní ASP.NET Web API 2 | Dokumentace Microsoftu
author: MikeWasson
description: Ukazuje, jak povolit trasování v rozhraní ASP.NET Web API.
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: e0d525e497cf41a79820417a9c832fa6b5cd7f8a
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912836"
---
<a name="tracing-in-aspnet-web-api-2"></a><span data-ttu-id="6fb2d-103">Trasování v rozhraní ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="6fb2d-103">Tracing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="6fb2d-104">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6fb2d-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="6fb2d-105">Pokud se pokoušíte ladit webové aplikace, není žádná náhrada správnou sadu protokoly trasování.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-105">When you are trying to debug a web-based application, there is no substitute for a good set of trace logs.</span></span> <span data-ttu-id="6fb2d-106">Tento kurz ukazuje, jak povolit trasování v rozhraní ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-106">This tutorial shows how to enable tracing in ASP.NET Web API.</span></span> <span data-ttu-id="6fb2d-107">Tuto funkci můžete použít ke sledování, co dělá rozhraní webového rozhraní API před a po vyvolá kontrolér.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-107">You can use this feature to trace what the Web API framework does before and after it invokes your controller.</span></span> <span data-ttu-id="6fb2d-108">Také ho můžete použít ke sledování vlastního kódu.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-108">You can also use it to trace your own code.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6fb2d-109">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="6fb2d-109">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="6fb2d-110">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) (funguje taky s Visual Studiem 2015)</span><span class="sxs-lookup"><span data-stu-id="6fb2d-110">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) (also works with Visual Studio 2015)</span></span>
> - <span data-ttu-id="6fb2d-111">Webové rozhraní API 2</span><span class="sxs-lookup"><span data-stu-id="6fb2d-111">Web API 2</span></span>
> - [<span data-ttu-id="6fb2d-112">Microsoft.AspNet.WebApi.Tracing</span><span class="sxs-lookup"><span data-stu-id="6fb2d-112">Microsoft.AspNet.WebApi.Tracing</span></span>](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)

## <a name="enable-systemdiagnostics-tracing-in-web-api"></a><span data-ttu-id="6fb2d-113">Povolit System.Diagnostics trasování ve webovém rozhraní API</span><span class="sxs-lookup"><span data-stu-id="6fb2d-113">Enable System.Diagnostics Tracing in Web API</span></span>

<span data-ttu-id="6fb2d-114">Nejprve vytvoříme nový projekt webové aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-114">First, we'll create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="6fb2d-115">V sadě Visual Studio z **souboru** nabídce vyberte možnost **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-115">In Visual Studio, from the **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="6fb2d-116">V části **šablony**, **webové**vyberte **webová aplikace ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-116">Under **Templates**, **Web**, select **ASP.NET Web Application**.</span></span>

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="6fb2d-117">Vyberte šablonu projektu webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-117">Choose the Web API project template.</span></span>

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

<span data-ttu-id="6fb2d-118">Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet**, pak **konzoly Správa balíčků**.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-118">From the **Tools** menu, select **NuGet Package Manager**, then **Package Manage Console**.</span></span>

<span data-ttu-id="6fb2d-119">V okně konzoly Správce balíčků zadejte následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-119">In the Package Manager Console window, type the following commands.</span></span>

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

<span data-ttu-id="6fb2d-120">První příkaz nainstaluje nejnovější balíček trasování webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-120">The first command installs the latest Web API tracing package.</span></span> <span data-ttu-id="6fb2d-121">Rovněž aktualizuje balíčky webového rozhraní API core.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-121">It also updates the core Web API packages.</span></span> <span data-ttu-id="6fb2d-122">Druhý příkaz je WebApi.WebHost balíček aktualizován na nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-122">The second command updates the WebApi.WebHost package to the latest version.</span></span>

> [!NOTE]
> <span data-ttu-id="6fb2d-123">Pokud chcete cílit na konkrétní verzi rozhraní Web API, použijte příznak-verze při instalaci balíčku trasování.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-123">If you want to target a specific version of Web API, use the -Version flag when you install the tracing package.</span></span>

<span data-ttu-id="6fb2d-124">Otevřete soubor WebApiConfig.cs v aplikaci\_spouštěcí složka.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-124">Open the file WebApiConfig.cs in the App\_Start folder.</span></span> <span data-ttu-id="6fb2d-125">Přidejte následující kód, který **zaregistrovat** metody.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-125">Add the following code to the **Register** method.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

<span data-ttu-id="6fb2d-126">Tento kód přidá [zapisovač SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) třídy do kanálu webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-126">This code adds the [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) class to the Web API pipeline.</span></span> <span data-ttu-id="6fb2d-127">**Zapisovač SystemDiagnosticsTraceWriter** zapíše trasování do třídy [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).</span><span class="sxs-lookup"><span data-stu-id="6fb2d-127">The **SystemDiagnosticsTraceWriter** class writes traces to [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).</span></span>

<span data-ttu-id="6fb2d-128">Chcete-li zobrazit trasování, spusťte aplikaci v ladicím programu.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-128">To see the traces, run the application in the debugger.</span></span> <span data-ttu-id="6fb2d-129">V prohlížeči přejděte na `/api/values`.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-129">In the browser, navigate to `/api/values`.</span></span>

![](tracing-in-aspnet-web-api/_static/image5.png)

<span data-ttu-id="6fb2d-130">Příkazy trasování jsou zapsány do okna výstup v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-130">The trace statements are written to the Output window in Visual Studio.</span></span> <span data-ttu-id="6fb2d-131">(Z **zobrazení** nabídce vyberte možnost **výstup**).</span><span class="sxs-lookup"><span data-stu-id="6fb2d-131">(From the **View** menu, select **Output**).</span></span>

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

<span data-ttu-id="6fb2d-132">Protože **zapisovač SystemDiagnosticsTraceWriter** zapíše trasování do **System.Diagnostics.Trace**, zaregistrujete naslouchací procesy další trasování; například zapsat trasování do souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-132">Because **SystemDiagnosticsTraceWriter** writes traces to **System.Diagnostics.Trace**, you can register additional trace listeners; for example, to write traces to a log file.</span></span> <span data-ttu-id="6fb2d-133">Další informace o trasování zapisovače, najdete v článku [naslouchacích procesů trasování](https://msdn.microsoft.com/library/4y5y10s7.aspx) tématu na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-133">For more information about trace writers, see the [Trace Listeners](https://msdn.microsoft.com/library/4y5y10s7.aspx) topic on MSDN.</span></span>

### <a name="configuring-systemdiagnosticstracewriter"></a><span data-ttu-id="6fb2d-134">Konfigurace zapisovač SystemDiagnosticsTraceWriter</span><span class="sxs-lookup"><span data-stu-id="6fb2d-134">Configuring SystemDiagnosticsTraceWriter</span></span>

<span data-ttu-id="6fb2d-135">Následující kód ukazuje, jak konfigurovat zápis trasování.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-135">The following code shows how to configure the trace writer.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="6fb2d-136">Existují dvě nastavení, které můžete řídit:</span><span class="sxs-lookup"><span data-stu-id="6fb2d-136">There are two settings that you can control:</span></span>

- <span data-ttu-id="6fb2d-137">IsVerbose: Pokud má hodnotu false, obsahuje každý trasování minimální informace.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-137">IsVerbose: If false, each trace contains minimal information.</span></span> <span data-ttu-id="6fb2d-138">Při hodnotě true se trasování obsahují další informace.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-138">If true, traces include more information.</span></span>
- <span data-ttu-id="6fb2d-139">MinimumLevel: Nastaví minimální úroveň trasování.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-139">MinimumLevel: Sets the minimum trace level.</span></span> <span data-ttu-id="6fb2d-140">Jsou úrovně trasování, v pořadí, ladění, informace, varování, chyba a závažná chyba.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-140">Trace levels, in order, are Debug, Info, Warn, Error, and Fatal.</span></span>

## <a name="adding-traces-to-your-web-api-application"></a><span data-ttu-id="6fb2d-141">Přidání trasování aplikace webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="6fb2d-141">Adding Traces to Your Web API Application</span></span>

<span data-ttu-id="6fb2d-142">Přidání zapisovač trasování umožňuje okamžitý přístup k trasování vytvořený kanál rozhraní Web API.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-142">Adding a trace writer gives you immediate access to the traces created by the Web API pipeline.</span></span> <span data-ttu-id="6fb2d-143">Můžete také zapisovač trasování pro trasování váš vlastní kód:</span><span class="sxs-lookup"><span data-stu-id="6fb2d-143">You can also use the trace writer to trace your own code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="6fb2d-144">Chcete-li získat zapisovač trasování, zavolejte **HttpConfiguration.Services.GetTraceWriter**.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-144">To get the trace writer, call **HttpConfiguration.Services.GetTraceWriter**.</span></span> <span data-ttu-id="6fb2d-145">Z řadiče, tato metoda je přístupná prostřednictvím **ApiController.Configuration** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-145">From a controller, this method is accessible through the **ApiController.Configuration** property.</span></span>

<span data-ttu-id="6fb2d-146">Zapsat trasování do třídy, můžete volat **ITraceWriter.Trace** metoda přímo, ale [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) třída definuje některé metody rozšíření, které jsou popisnějšího.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-146">To write a trace, you can call the **ITraceWriter.Trace** method directly, but the [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) class defines some extension methods that are more friendly.</span></span> <span data-ttu-id="6fb2d-147">Například **informace** výše uvedená metoda vytvoří trasování s úrovní trasování **informace**.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-147">For example, the **Info** method shown above creates a trace with trace level **Info**.</span></span>

## <a name="web-api-tracing-infrastructure"></a><span data-ttu-id="6fb2d-148">Sledování infrastruktury webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="6fb2d-148">Web API Tracing Infrastructure</span></span>

<span data-ttu-id="6fb2d-149">Tato část popisuje, jak psát vlastní trasování zapisovače pro webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-149">This section describes how to write a custom trace writer for Web API.</span></span>

<span data-ttu-id="6fb2d-150">Další obecné infrastruktury trasování v rozhraní Web API je nástavbou Microsoft.AspNet.WebApi.Tracing balíčku.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-150">The Microsoft.AspNet.WebApi.Tracing package is built on top of a more general tracing infrastructure in Web API.</span></span> <span data-ttu-id="6fb2d-151">Namísto použití Microsoft.AspNet.WebApi.Tracing, můžete také zařadit některé jiné trasování nebo odfiltrovat z knihovny, například [NLog](http://nlog-project.org/) nebo [log4net](http://logging.apache.org/log4net/).</span><span class="sxs-lookup"><span data-stu-id="6fb2d-151">Instead of using Microsoft.AspNet.WebApi.Tracing, you can also plug in some other tracing/loggin library, such as [NLog](http://nlog-project.org/) or [log4net](http://logging.apache.org/log4net/).</span></span>

<span data-ttu-id="6fb2d-152">Chcete-li shromažďovat trasování, implementovat **ITraceWriter** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-152">To collect traces, implement the **ITraceWriter** interface.</span></span> <span data-ttu-id="6fb2d-153">Tady je jednoduchý příklad:</span><span class="sxs-lookup"><span data-stu-id="6fb2d-153">Here is a simple example:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="6fb2d-154">**ITraceWriter.Trace** metoda vytvoří trasování.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-154">The **ITraceWriter.Trace** method creates a trace.</span></span> <span data-ttu-id="6fb2d-155">Volající určuje úroveň kategorie a trasování.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-155">The caller specifies a category and trace level.</span></span> <span data-ttu-id="6fb2d-156">Kategorie může být libovolný řetězec definovaný uživatelem.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-156">The category can be any user-defined string.</span></span> <span data-ttu-id="6fb2d-157">Implementace **trasování** udělat toto:</span><span class="sxs-lookup"><span data-stu-id="6fb2d-157">Your implementation of **Trace** should do the following:</span></span>

1. <span data-ttu-id="6fb2d-158">Vytvořte nový **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-158">Create a new **TraceRecord**.</span></span> <span data-ttu-id="6fb2d-159">Jak je znázorněno, inicializujte ho s požadavkem, kategorií a úroveň trasování.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-159">Initialize it with the request, category, and trace level, as shown.</span></span> <span data-ttu-id="6fb2d-160">Tyto hodnoty jsou k dispozici volajícím.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-160">These values are provided by the caller.</span></span>
2. <span data-ttu-id="6fb2d-161">Vyvolat *traceAction* delegovat.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-161">Invoke the *traceAction* delegate.</span></span> <span data-ttu-id="6fb2d-162">Uvnitř tohoto delegáta, volající by měl k vyplnění zbývající části **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-162">Inside this delegate, the caller is expected to fill in the rest of the **TraceRecord**.</span></span>
3. <span data-ttu-id="6fb2d-163">Zápis **TraceRecord**, pomocí jakékoli protokolování technika, který vás zajímá.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-163">Write the **TraceRecord**, using any logging technique that you like.</span></span> <span data-ttu-id="6fb2d-164">V příkladu je vidět tady jednoduše volá **System.Diagnostics.Trace**.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-164">The example shown here simply calls into **System.Diagnostics.Trace**.</span></span>

## <a name="setting-the-trace-writer"></a><span data-ttu-id="6fb2d-165">Nastavení zapisovač trasování</span><span class="sxs-lookup"><span data-stu-id="6fb2d-165">Setting the Trace Writer</span></span>

<span data-ttu-id="6fb2d-166">Pokud chcete povolit trasování, musíte nakonfigurovat webové rozhraní API pro použití vašeho **ITraceWriter** implementace.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-166">To enable tracing, you must configure Web API to use your **ITraceWriter** implementation.</span></span> <span data-ttu-id="6fb2d-167">Až to uděláte **HttpConfiguration** objektu, jak je znázorněno v následujícím kódu:</span><span class="sxs-lookup"><span data-stu-id="6fb2d-167">You do this through the **HttpConfiguration** object, as shown in the following code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="6fb2d-168">Může být aktivní pouze jeden trasování zapisovače.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-168">Only one trace writer can be active.</span></span> <span data-ttu-id="6fb2d-169">Ve výchozím nastavení, nastaví webového rozhraní API &quot;no-op&quot; závislostí, který nemá žádný účinek.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-169">By default, Web API sets a &quot;no-op&quot; tracer that does nothing.</span></span> <span data-ttu-id="6fb2d-170">( &quot;No-op&quot; závislostí existuje, takže není potřeba zkontrolovat, zda je zapisovač trasování trasování kódu **null** před zápisem trasování.)</span><span class="sxs-lookup"><span data-stu-id="6fb2d-170">(The &quot;no-op&quot; tracer exists so that tracing code does not have to check whether the trace writer is **null** before writing a trace.)</span></span>

## <a name="how-web-api-tracing-works"></a><span data-ttu-id="6fb2d-171">Jak webové rozhraní API trasování funguje</span><span class="sxs-lookup"><span data-stu-id="6fb2d-171">How Web API Tracing Works</span></span>

<span data-ttu-id="6fb2d-172">Používá trasování v rozhraní Web API *průčelí* vzor: když je povoleno trasování, webové rozhraní API zabalí různé části požadavku kanálu pomocí třídy, které provádějí sledování volání.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-172">Tracing in Web API uses a *facade* pattern: When tracing is enabled, Web API wraps various parts of the request pipeline with classes that perform trace calls.</span></span>

<span data-ttu-id="6fb2d-173">Například při výběru kontroleru, kanál používá **IHttpControllerSelector** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-173">For example, when selecting a controller, the pipeline uses the **IHttpControllerSelector** interface.</span></span> <span data-ttu-id="6fb2d-174">S povoleným trasováním, vloží pipleline třídu, která implementuje **IHttpControllerSelector** , ale volání skutečné implementaci:</span><span class="sxs-lookup"><span data-stu-id="6fb2d-174">With tracing enabled, the pipleline inserts a class that implements **IHttpControllerSelector** but calls through to the real implementation:</span></span>

![Trasování serveru webové rozhraní API používá průčelí modelu.](tracing-in-aspnet-web-api/_static/image8.png)

<span data-ttu-id="6fb2d-176">Tento návrh mezi výhody patří:</span><span class="sxs-lookup"><span data-stu-id="6fb2d-176">The benefits of this design include:</span></span>

- <span data-ttu-id="6fb2d-177">Pokud nepřidáte zapisovač trasování, komponenty trasování není vytvořena instance a mít žádný vliv na výkon.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-177">If you do not add a trace writer, the tracing components are not instantiated and have no performance impact.</span></span>
- <span data-ttu-id="6fb2d-178">Pokud například nahradíte výchozí služby **IHttpControllerSelector** s vlastním vlastních implementací, není ovlivněno trasování, protože trasování se provádí Obálkový objekt.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-178">If you replace default services such as **IHttpControllerSelector** with your own custom implementation, tracing is not affected, because tracing is done by the wrapper object.</span></span>

<span data-ttu-id="6fb2d-179">Můžete také nahradit celé trasování rozhraní Web API s vlastním vlastní rozhraní .NET framework, tak, že nahradíte výchozí **ITraceManager** služby:</span><span class="sxs-lookup"><span data-stu-id="6fb2d-179">You can also replace the entire Web API trace framework with your own custom framework, by replacing the default **ITraceManager** service:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="6fb2d-180">Implementace **ITraceManager.Initialize** inicializovat trasování systému.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-180">Implement **ITraceManager.Initialize** to initialize your tracing system.</span></span> <span data-ttu-id="6fb2d-181">Mějte na paměti, že to nahrazuje *celý* trasování rozhraní framework, včetně všech trasování kódu, která je integrována do webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="6fb2d-181">Be aware that this replaces the *entire* trace framework, including all of the tracing code that is built into Web API.</span></span>
