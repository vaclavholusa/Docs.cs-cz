---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: Trasování v rozhraní ASP.NET Web API 2 | Dokumentace Microsoftu
author: MikeWasson
description: Ukazuje, jak povolit trasování v rozhraní ASP.NET Web API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f9c33e62a1d04bc7851be13560a2bd39e997448f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366861"
---
<a name="tracing-in-aspnet-web-api-2"></a>Trasování v rozhraní ASP.NET Web API 2
====================
podle [Mike Wasson](https://github.com/MikeWasson)

> Pokud se pokoušíte ladit webové aplikace, není žádná náhrada správnou sadu protokoly trasování. Tento kurz ukazuje, jak povolit trasování v rozhraní ASP.NET Web API. Tuto funkci můžete použít ke sledování, co dělá rozhraní webového rozhraní API před a po vyvolá kontrolér. Také ho můžete použít ke sledování vlastního kódu.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - [Visual Studio 2017](https://www.visualstudio.com/downloads/) (funguje taky s Visual Studiem 2015)
> - Webové rozhraní API 2
> - [Microsoft.AspNet.WebApi.Tracing](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)


## <a name="enable-systemdiagnostics-tracing-in-web-api"></a>Povolit System.Diagnostics trasování ve webovém rozhraní API

Nejprve vytvoříme nový projekt webové aplikace ASP.NET. V sadě Visual Studio z **souboru** nabídce vyberte možnost **nový**, pak **projektu**. V části **šablony**, **webové**vyberte **webová aplikace ASP.NET**.

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

Vyberte šablonu projektu webového rozhraní API.

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**, pak **konzoly Správa balíčků**.

V okně konzoly Správce balíčků zadejte následující příkazy.

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

První příkaz nainstaluje nejnovější balíček trasování webového rozhraní API. Rovněž aktualizuje balíčky webového rozhraní API core. Druhý příkaz je WebApi.WebHost balíček aktualizován na nejnovější verzi.

> [!NOTE]
> Pokud chcete cílit na konkrétní verzi rozhraní Web API, použijte příznak-verze při instalaci balíčku trasování.


Otevřete soubor WebApiConfig.cs v aplikaci\_spouštěcí složka. Přidejte následující kód, který **zaregistrovat** metody.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

Tento kód přidá [zapisovač SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) třídy do kanálu webové rozhraní API. **Zapisovač SystemDiagnosticsTraceWriter** zapíše trasování do třídy [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).

Chcete-li zobrazit trasování, spusťte aplikaci v ladicím programu. V prohlížeči přejděte na `/api/values`.

![](tracing-in-aspnet-web-api/_static/image5.png)

Příkazy trasování jsou zapsány do okna výstup v sadě Visual Studio. (Z **zobrazení** nabídce vyberte možnost **výstup**).

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

Protože **zapisovač SystemDiagnosticsTraceWriter** zapíše trasování do **System.Diagnostics.Trace**, zaregistrujete naslouchací procesy další trasování; například zapsat trasování do souboru protokolu. Další informace o trasování zapisovače, najdete v článku [naslouchacích procesů trasování](https://msdn.microsoft.com/library/4y5y10s7.aspx) tématu na webu MSDN.

### <a name="configuring-systemdiagnosticstracewriter"></a>Konfigurace zapisovač SystemDiagnosticsTraceWriter

Následující kód ukazuje, jak konfigurovat zápis trasování.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

Existují dvě nastavení, které můžete řídit:

- IsVerbose: Pokud má hodnotu false, obsahuje každý trasování minimální informace. Při hodnotě true se trasování obsahují další informace.
- MinimumLevel: Nastaví minimální úroveň trasování. Jsou úrovně trasování, v pořadí, ladění, informace, varování, chyba a závažná chyba.

## <a name="adding-traces-to-your-web-api-application"></a>Přidání trasování aplikace webového rozhraní API

Přidání zapisovač trasování umožňuje okamžitý přístup k trasování vytvořený kanál rozhraní Web API. Můžete také zapisovač trasování pro trasování váš vlastní kód:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

Chcete-li získat zapisovač trasování, zavolejte **HttpConfiguration.Services.GetTraceWriter**. Z řadiče, tato metoda je přístupná prostřednictvím **ApiController.Configuration** vlastnost.

Zapsat trasování do třídy, můžete volat **ITraceWriter.Trace** metoda přímo, ale [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) třída definuje některé metody rozšíření, které jsou popisnějšího. Například **informace** výše uvedená metoda vytvoří trasování s úrovní trasování **informace**.

## <a name="web-api-tracing-infrastructure"></a>Sledování infrastruktury webového rozhraní API

Tato část popisuje, jak psát vlastní trasování zapisovače pro webové rozhraní API.

Další obecné infrastruktury trasování v rozhraní Web API je nástavbou Microsoft.AspNet.WebApi.Tracing balíčku. Namísto použití Microsoft.AspNet.WebApi.Tracing, můžete také zařadit některé jiné trasování nebo odfiltrovat z knihovny, například [NLog](http://nlog-project.org/) nebo [log4net](http://logging.apache.org/log4net/).

Chcete-li shromažďovat trasování, implementovat **ITraceWriter** rozhraní. Tady je jednoduchý příklad:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

**ITraceWriter.Trace** metoda vytvoří trasování. Volající určuje úroveň kategorie a trasování. Kategorie může být libovolný řetězec definovaný uživatelem. Implementace **trasování** udělat toto:

1. Vytvořte nový **TraceRecord**. Jak je znázorněno, inicializujte ho s požadavkem, kategorií a úroveň trasování. Tyto hodnoty jsou k dispozici volajícím.
2. Vyvolat *traceAction* delegovat. Uvnitř tohoto delegáta, volající by měl k vyplnění zbývající části **TraceRecord**.
3. Zápis **TraceRecord**, pomocí jakékoli protokolování technika, který vás zajímá. V příkladu je vidět tady jednoduše volá **System.Diagnostics.Trace**.

## <a name="setting-the-trace-writer"></a>Nastavení zapisovač trasování

Pokud chcete povolit trasování, musíte nakonfigurovat webové rozhraní API pro použití vašeho **ITraceWriter** implementace. Až to uděláte **HttpConfiguration** objektu, jak je znázorněno v následujícím kódu:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

Může být aktivní pouze jeden trasování zapisovače. Ve výchozím nastavení, nastaví webového rozhraní API &quot;no-op&quot; závislostí, který nemá žádný účinek. ( &quot;No-op&quot; závislostí existuje, takže není potřeba zkontrolovat, zda je zapisovač trasování trasování kódu **null** před zápisem trasování.)

## <a name="how-web-api-tracing-works"></a>Jak webové rozhraní API trasování funguje

Trasování v použití webového rozhraní API používá v webové rozhraní API *průčelí* vzor: když je povoleno trasování, webové rozhraní API zabalí různé části požadavku kanálu pomocí třídy, které provádějí sledování volání.

Například při výběru kontroleru, kanál používá **IHttpControllerSelector** rozhraní. S povoleným trasováním, vloží pipleline třídu, která implementuje **IHttpControllerSelector** , ale volání skutečné implementaci:

![Trasování serveru webové rozhraní API používá průčelí modelu.](tracing-in-aspnet-web-api/_static/image8.png)

Tento návrh mezi výhody patří:

- Pokud nepřidáte zapisovač trasování, komponenty trasování není vytvořena instance a mít žádný vliv na výkon.
- Pokud například nahradíte výchozí služby **IHttpControllerSelector** s vlastním vlastních implementací, není ovlivněno trasování, protože trasování se provádí Obálkový objekt.

Můžete také nahradit celé trasování rozhraní Web API s vlastním vlastní rozhraní .NET framework, tak, že nahradíte výchozí **ITraceManager** služby:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

Implementace **ITraceManager.Initialize** inicializovat trasování systému. Mějte na paměti, že to nahrazuje *celý* trasování rozhraní framework, včetně všech trasování kódu, která je integrována do webového rozhraní API.
