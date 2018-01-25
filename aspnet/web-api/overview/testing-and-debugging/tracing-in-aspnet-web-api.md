---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: "Trasování v rozhraní ASP.NET Web API 2 | Microsoft Docs"
author: MikeWasson
description: "Ukazuje, jak povolit trasování v rozhraní ASP.NET Web API."
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
---
<a name="tracing-in-aspnet-web-api-2"></a>Trasování v rozhraní ASP.NET Web API 2
====================
podle [Wasson Jan](https://github.com/MikeWasson)

> Když se pokoušíte ladění webové aplikace, neexistuje žádný nenahrazuje sadu dobrou protokolů trasování. Tento kurz ukazuje, jak povolit trasování v rozhraní ASP.NET Web API. Tato funkce slouží ke sledování, co dělají rozhraní Web API před a po vyvolá řadiči. Můžete ji použít i ke sledování vlastní kód.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - [Visual Studio 2017](https://www.visualstudio.com/downloads/) (funguje taky s Visual Studiem 2015)
> - Webové rozhraní API 2
> - [Microsoft.AspNet.WebApi.Tracing](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)


## <a name="enable-systemdiagnostics-tracing-in-web-api"></a>Povolit System.Diagnostics trasování v rozhraní Web API

Nejdříve vytvoříme nový projekt webové aplikace ASP.NET. V sadě Visual Studio z **soubor** nabídce vyberte možnost **nový**, pak **projektu**. V části **šablony**, **webové**, vyberte **webové aplikace ASP.NET**.

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

Výběr šablony projektu webového rozhraní API.

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**, pak **konzoly Správa balíčků**.

V okně konzoly Správce balíčků zadejte následující příkazy.

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

První příkaz nainstaluje nejnovější balíček trasování webového rozhraní API. Aktualizuje také balíčky webového rozhraní API core. V druhém příkazu se WebApi.WebHost balíček aktualizuje na nejnovější verzi.

> [!NOTE]
> Pokud chcete zacílit na konkrétní verzi rozhraní Web API, použijte příznak-verze při instalaci balíčku trasování.


Otevřete soubor WebApiConfig.cs v aplikaci\_spouštěcí složka. Přidejte následující kód, který **zaregistrovat** metoda.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

Tento kód přidá [zapisovač SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) třída do kanálu webové rozhraní API. **Zapisovač SystemDiagnosticsTraceWriter** zapíše trasování do třídy [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).

Pokud chcete zobrazit trasování, spusťte aplikaci v ladicím programu. V prohlížeči přejděte na `/api/values`.

![](tracing-in-aspnet-web-api/_static/image5.png)

Příkazy trasování se zapisují do okna výstupu v sadě Visual Studio. (Z **zobrazení** nabídce vyberte možnost **výstup**).

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

Protože **zapisovač SystemDiagnosticsTraceWriter** zapíše trasování do **System.Diagnostics.Trace**, můžete zaregistrovat další trasování – moduly naslouchání; například zapsat trasování do souboru protokolu. Další informace o trasování zapisovače najdete v tématu [trasování – moduly naslouchání](https://msdn.microsoft.com/library/4y5y10s7.aspx) tématu na webu MSDN.

### <a name="configuring-systemdiagnosticstracewriter"></a>Konfigurace zapisovač SystemDiagnosticsTraceWriter

Následující kód ukazuje, jak konfigurovat zápis trasování.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

Existují dvě nastavení, které můžete řídit:

- IsVerbose: Pokud je hodnota false, obsahuje každý trasování minimální informace. V případě hodnoty true trasování obsahují další informace.
- MinimumLevel: Nastaví minimální úroveň trasování. Úrovně trasování v pořadí, jsou ladění, informace, varování, chyba a závažná chyba.

## <a name="adding-traces-to-your-web-api-application"></a>Přidání trasování do vaší aplikace webového rozhraní API

Přidání zapisovač trasování umožňuje okamžitý přístup k trasování vytvořený kanál rozhraní Web API. Můžete taky zapisovač trasování pro trasování vlastní kód:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

Chcete-li získat zapisovač trasování, volejte **HttpConfiguration.Services.GetTraceWriter**. Z řadiče, tato metoda je přístupná prostřednictvím **ApiController.Configuration** vlastnost.

Pokud chcete zapsat trasování, můžete zavolat **ITraceWriter.Trace** metoda přímo, ale [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) třída definuje některé rozšiřující metody, které jsou přehlednější. Například **informace** výše uvedená metoda vytvoří trasování s úroveň trasování **informace**.

## <a name="web-api-tracing-infrastructure"></a>Infrastruktura trasování webového rozhraní API

Tato část popisuje, jak psát vlastní trasování zapisovač pro webového rozhraní API.

Balíček Microsoft.AspNet.WebApi.Tracing je postavená na infrastruktuře obecnější trasování v rozhraní Web API. Místo použití Microsoft.AspNet.WebApi.Tracing, můžete také zařadit některé další trasování nebo protokolování knihovny, například [NLog](http://nlog-project.org/) nebo [log4net](http://logging.apache.org/log4net/).

Chcete-li shromažďovat trasování, implementujte **ITraceWriter** rozhraní. Zde je jednoduchý příklad:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

**ITraceWriter.Trace** metoda vytvoří trasování. Volající určuje úroveň kategorie a trasování. Kategorie může být libovolný řetězec definovaný uživatelem. Vaši implementaci **trasování** udělat následující:

1. Vytvořte novou **TraceRecord**. Inicializujte požadavku, kategorii a úroveň trasování, jak je vidět. Tyto hodnoty jsou poskytovány volající.
2. Vyvolání *traceAction* delegovat. Uvnitř tento delegát volající by měl vyplnit zbytek **TraceRecord**.
3. Zápis **TraceRecord**, všechny technikou protokolování, který chcete. Příkladu jednoduché volání **System.Diagnostics.Trace**.

## <a name="setting-the-trace-writer"></a>Nastavení zapisovač trasování

Chcete-li povolit trasování, je nutné nakonfigurovat webového rozhraní API používat vaše **ITraceWriter** implementace. To provedete pomocí **HttpConfiguration** objektu, jak je znázorněno v následujícím kódu:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

Zapisovač trasování pouze jeden můžou být aktivní. Ve výchozím nastavení, nastaví webového rozhraní API &quot;no-op&quot; závislostí, který se nic nestane. ( &quot;No-op&quot; trasovací existuje, takže není nutné zkontrolovat, zda je zapisovač trasování trasování kódu **null** před zápisem trasování.)

## <a name="how-web-api-tracing-works"></a>Jak webové rozhraní API funguje pro trasování

Trasování v webového rozhraní API používá používá v webového rozhraní API *průčelí za* vzor: Pokud je povoleno sledování, webového rozhraní API zabalí různých částí kanál požadavku s třídami, které provádějí trasovacího volání.

Například při výběru kontroleru, kanál používá **IHttpControllerSelector** rozhraní. Pomocí trasování povoleno, pipleline vloží třídu, která implementuje **IHttpControllerSelector** ale volání skutečné implementace:

![Trasování rozhraní Web API využívá vzor průčelí za.](tracing-in-aspnet-web-api/_static/image8.png)

Výhody tohoto návrhu patří:

- Pokud nepřidáte zapisovač trasování, trasování součásti nejsou vytvořena instance a mít žádný vliv na výkon.
- Pokud například nahradíte výchozích služeb **IHttpControllerSelector** s vlastními vlastní implementaci nemá vliv trasování, protože objekt obálky se provádí trasování.

Můžete také nahradit celý framework trasování webového rozhraní API vlastní vlastní framework nahrazením výchozí **ITraceManager** služby:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

Implementace **ITraceManager.Initialize** k chybě při inicializaci systému trasování. Uvědomte si, že to nahrazuje *celý* framework trasování, včetně všech objektů kód trasování, která je integrována do webového rozhraní API.
