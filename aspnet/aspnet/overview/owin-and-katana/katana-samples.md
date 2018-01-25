---
uid: aspnet/overview/owin-and-katana/katana-samples
title: "Ukázky Katana | Microsoft Docs"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2014
ms.topic: article
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 815355c00c9c15cfefa5f98dc89da676743b0390
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="katana-samples"></a>Ukázky Katana
====================
podle [Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Ukázky Katana

**ASP.NET směruje ukázka** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)  
V některých aplikacích můžete spojit komponenty OWIN v tabulce směrování Asp.Net node souběžně s komponenty jiných OWIN. Tento příklad ukazuje způsob použití metod rozšíření RouteCollection MapOwinPath a MapOwinRoute poskytované Microsoft.Owin.Host.SystemWeb.

**Vytvoření větve kanálů ukázka** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)  
Zpracování žádosti OWIN kanály nemusí být lineární, že můžete vytvořit větve zpracovávat požadavky různými způsoby. Tento příklad ukazuje, jak vytvořit větvení kanál na základě cesty k požadavku nebo jiná data požadavku třeba hlavičky. Tyto součásti jsou k dispozici v Microsoft.Owin.Mapping balíček nuget.

**Ukázka vlastního serveru** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs)   
Ukazuje, jak použít vlastní server OWIN při vlastním hostování OWIN.

**Ukázka vložených** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)  
Některé servery OWIN lze spustit v rámci vlastní proces (&quot;hostovanou na vlastním&quot;). Tento příklad ukazuje postup spuštění OWIN aplikace pomocí nástroje poskytované subsystémem pro balíček Microsoft.Owin.Hosting nuget.

**Ukázka HelloWorld** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)  
OWIN je server HTTP abstrakce rozhraní API, která umožňuje přenosnost aplikací na různých serverech. Tento příklad ukazuje, jak psát aplikace Hello World pomocí některé **jednoduché obálky** kolem nezpracovaná abstrakce OWIN a spusťte ho na webovém serveru jako ASP.NET.

**Ukázka programu Hello World nezpracovaná OWIN** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)  
Tento příklad znázorňuje, jak psát aplikace Hello, World pomocí **nezpracovaná** abstrakce OWIN a potom ho spusťte na webovém serveru, jako je technologie Asp.Net.

**Ukázka SignalR** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)  
Ukazuje, jak hostování na vlastním SignalR pomocí OWIN nebo Katana. Další informace o samoobslužné hostování SignalR najdete v tématu [kurz: SignalR hostování na vlastním](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

**Statické soubory ukázka** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs)   
Ukazuje, jak pro podporu požadavků HTTP pro statické soubory pomocí OWIN nebo Katana.

**Webové rozhraní API** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt)   
Tento příklad ukazuje, jak pro hostování OWIN ve službě IIS a přidání webového rozhraní API do kanálu OWIN.

**Webový soket ukázka** | [zdrojový kód](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs)   
Ukazuje, jak pro podporu webové sokety v OWIN pomocí [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) třídy.
