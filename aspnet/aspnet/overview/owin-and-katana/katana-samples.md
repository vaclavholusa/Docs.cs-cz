---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Použití sady Katana | Dokumentace Microsoftu
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 01/17/2014
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: a26cdf144d02f0b429994df9d7863163807ac7b5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827269"
---
<a name="katana-samples"></a>Použití sady Katana
====================
podle [Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Použití sady Katana

**ASP.NET směruje ukázka** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)  
V některých aplikacích budete chtít připojení komponenty OWIN v tabulce směrování Asp.Net vedle komponenty bez OWIN. Tento příklad ukazuje, jak použít rozšiřující metody RouteCollection MapOwinPath a MapOwinRoute poskytované Microsoft.Owin.Host.SystemWeb.

**Větvení ukázkové kanály** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)  
Není potřeba mít lineární kanály zpracování žádostí OWIN, je možné větvit ke zpracování požadavků různými způsoby. Tento příklad ukazuje, jak vytvořit větvení profilace na základě cesty k požadavkům nebo jiné žádosti o data, jako jsou hlavičky. Tyto součásti jsou k dispozici v balíčku nuget Microsoft.Owin.Mapping.

**Ukázka vlastních serverových** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs)   
Ukazuje, jak použít vlastní server OWIN při vlastním hostování OWIN.

**Vložit ukázková** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)  
Některé servery OWIN lze spustit ve vašem vlastním procesu (&quot;v místním prostředí&quot;). Tato ukázka předvádí, jak můžete začít pomocí nástroje poskytované subsystémem balíček nuget Microsoft.Owin.Hosting aplikace OWIN.

**Ukázka Hello World** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)  
OWIN je HTTP server abstrakce rozhraní API, která umožňuje přenositelnost aplikací napříč různými servery. Tato ukázka předvádí, jak psát aplikace Hello World pomocí několika **jednoduché obálky** kolem nezpracovaná abstrakce OWIN a spusťte ho na webovém serveru, jako je ASP.NET.

**Ukázka programu Hello World nezpracovaná OWIN** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)  
Tato ukázka předvádí, jak psát aplikace Hello World pomocí **nezpracovaná** abstrakce OWIN a spusťte ho na webovém serveru, jako je Asp.Net.

**Ukázka SignalR** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)  
Ukazuje, jak hostování na vlastním serveru SignalR používá OWIN a Katana. Další informace o samoobslužné hostování funkci SignalR naleznete v tématu [kurz: SignalR v místním](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

**Statické soubory ukázka** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs)   
Ukazuje, jak pro podporu požadavků HTTP pro statické soubory, které používá OWIN a Katana.

**Webové rozhraní API** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt)   
Tento příklad ukazuje, jak k hostování OWIN ve službě IIS a webového rozhraní API přidat do kanálu OWIN.

**Webových soketů ukázka** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs)   
Ukazuje, jak pomocí podporují webové sokety OWIN [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) třídy.
