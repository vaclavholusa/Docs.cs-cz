---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Hostování na vlastním rozhraní ASP.NET Web API 2 pomocí OWIN | Microsoft Docs
author: rick-anderson
description: Tento kurz ukazuje, jak k hostování webové rozhraní API ASP.NET v konzolové aplikaci, pomocí k hostování na vlastním rozhraní Web API OWIN. Spustit nástroj webové rozhraní pro rozhraní .NET (OWIN) d...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2013
ms.topic: article
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: fda0db8155c3303907331a690af35f619b589154
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566395"
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a>Hostování na vlastním rozhraní ASP.NET Web API 2 pomocí OWIN
====================
podle [Kanchan Mehrotra](https://twitter.com/kanchanmeh)

> Tento kurz ukazuje, jak k hostování webové rozhraní API ASP.NET v konzolové aplikaci, pomocí k hostování na vlastním rozhraní Web API OWIN.
> 
> [Otevřete Web Interface pro .NET](http://owin.org) (OWIN) definuje abstrakci mezi .NET webové servery a webové aplikace. OWIN oddělí webové aplikace ze serveru, takže je ideální pro samoobslužné webové aplikace v vlastní proces mimo službu IIS hostování OWIN.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (funguje taky s Visual Studio 2012)
> - Webové rozhraní API 2


> [!NOTE]
> Úplný zdrojový kód najdete v tomto kurzu v [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).


## <a name="create-a-console-application"></a>Vytvoření konzolové aplikace

Na **soubor** nabídky, klikněte na tlačítko **nový**, pak klikněte na tlačítko **projektu**. Z **nainstalovaných šablonách**, v části Visual C#, klikněte na **Windows** a pak klikněte na **konzolové aplikace**. Název projektu "OwinSelfhostSample" a klikněte na tlačítko **OK**.

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a>Přidání webového rozhraní API a balíčky OWIN

Z **nástroje** nabídky, klikněte na tlačítko **Správce balíčků knihoven**, pak klikněte na tlačítko **Konzola správce balíčků**. V okně konzoly Správce balíčků zadejte následující příkaz:

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

Nainstaluje balíček selfhost WebAPI OWIN a všechny požadované balíčky OWIN.

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a>Konfigurace webového rozhraní API pro hostování na vlastním serveru

V Průzkumníku řešení klikněte pravým tlačítkem na projekt a vyberte **přidat** / **třída** pro přidání nové třídy. Název třídy `Startup`.

![](use-owin-to-self-host-web-api/_static/image5.png)

Všechny často používaný kód v tomto souboru nahraďte následujícím textem:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a>Přidat řadič webové rozhraní API

V dalším kroku přidejte třídu kontroleru webového rozhraní API. V Průzkumníku řešení klikněte pravým tlačítkem na projekt a vyberte **přidat** / **třída** pro přidání nové třídy. Název třídy `ValuesController`.

Všechny často používaný kód v tomto souboru nahraďte následujícím textem:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a>Spuštění hostitele OWIN a podání žádosti o pomocí položky HttpClient

Nahraďte všechny často používaný kód v souboru Program.cs následujícím textem:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a>Spuštění aplikace

Ke spuštění aplikace, stisknutím klávesy F5 v sadě Visual Studio. Výstup by měl vypadat takto:

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a>Další prostředky

[Přehled Katana projektu](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[Hostitel rozhraní ASP.NET Web API roli pracovního procesu systému Azure](host-aspnet-web-api-in-an-azure-worker-role.md)
