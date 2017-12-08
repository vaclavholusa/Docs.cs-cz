---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: "Pomocí funkce SignalR webové aplikace v Azure App Service | Microsoft Docs"
author: pfletcher
description: "Tento dokument popisuje, jak nakonfigurovat aplikaci SignalR, která běží na Microsoft Azure. V tomto kurzu použili verze softwaru, Visual Studio 2013 nebo Vis...."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/01/2015
ms.topic: article
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 414701159b4e1fa3da9597503b14281a1e9991de
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a>Pomocí funkce SignalR webové aplikace v Azure App Service
====================
podle [Patrik Fletcher](https://github.com/pfletcher)

> Tento dokument popisuje, jak nakonfigurovat aplikaci SignalR, která běží na Microsoft Azure.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) nebo Visual Studio 2012
> - .NET 4.5
> - SignalR verze 2
> - 2.3 Azure SDK pro Visual Studio 2013 nebo 2012
>   
> 
> 
> ## <a name="questions-and-comments"></a>Dotazy a připomínky
> 
> Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky. Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), nebo [fóra Microsoft Azure](https://social.msdn.microsoft.com/Forums/windowsazure/en-US/home?category=windowsazureplatform).


## <a name="table-of-contents"></a>Obsah

- [Úvod](#introduction)
- [Nasazení funkce SignalR webové aplikace do Azure App Service](#deploying)
- [Povolení technologie WebSockets v Azure App Service](#websocket)
- [Pomocí propojovacího rozhraní mezipaměť Redis systému Azure](#backplane)
- [Další kroky](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a>Úvod

Funkce SignalR technologie ASP.NET umožňuje uvést novou úroveň interaktivity mezi servery a webové nebo klientů .NET. Když jsou hostované v Azure, SignalR můžou aplikace využít vysoce dostupných, škálovatelných a původce prostředí, který běží v cloudu poskytuje.

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a>Nasazení funkce SignalR webové aplikace do Azure App Service

SignalR nepřidá žádné konkrétní komplikace k nasazení aplikace do Azure a nasazení na místní server. Aplikace, která používá funkci SignalR může být hostovaný v Azure bez jakékoli změny v konfiguraci a další nastavení (přestože objekty WebSockets podporu najdete na webu [povolení technologie WebSockets v Azure App Service](#websocket) níže.) V tomto kurzu budete nasazovat aplikace vytvořené v [kurzu Začínáme](../getting-started/tutorial-getting-started-with-signalr.md) do Azure.

**Požadavky**

- Visual Studio 2013. Pokud nemáte Visual Studio, Visual Studio 2013 Express pro Web je součástí instalace sady Azure SDK.
- [2.3 Azure SDK pro Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) nebo [2.3 Azure SDK pro sadu Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).
- K dokončení tohoto kurzu, potřebujete předplatné Azure. Můžete [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/), nebo [si zaregistrovat zkušební předplatné](https://azure.microsoft.com/en-us/pricing/free-trial/).

### <a name="deploying-a-signalr-web-app-to-azure"></a>Nasazení funkce SignalR webové aplikace do Azure

1. Dokončení [kurzu Začínáme](../getting-started/tutorial-getting-started-with-signalr.md), nebo stáhnout dokončení projektu ze [galerie kódů](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
2. V sadě Visual Studio, vyberte **sestavení**, **publikování SignalR Chat**.
3. V dialogovém okně "Publikovat Web" Vyberte "webů systému Windows Azure".

    ![Vyberte weby Azure](using-signalr-with-azure-web-sites/_static/image1.png)
4. Pokud nejste přihlášení ke svému účtu Microsoft, klikněte na tlačítko **přihlásit...**  v dialogovém okně "Vyberte existující webový server" a přihlaste se.

    ![Vyberte existující webovou stránku](using-signalr-with-azure-web-sites/_static/image2.png)    ![Přihlaste se k Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. V dialogovém okně "Vyberte existující webový server", klikněte na tlačítko **nový**.

    ![Nový web](using-signalr-with-azure-web-sites/_static/image4.png)
6. V dialogovém okně "Při vytváření webu v systému Windows Azure" Zadejte jedinečným názvem aplikace. Vyberte oblast, která je nejblíže k vám v rozevírací nabídce oblast. Klikněte na tlačítko **vytvořit**.

    ![Vytvořit web v Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. V dialogovém okně "Publikovat Web", klikněte na tlačítko **publikovat**.

    ![publikování webu](using-signalr-with-azure-web-sites/_static/image6.png)
8. Po dokončení publikování aplikace SignalR chatovací aplikace hostované v Azure App Service Web Apps se otevře v prohlížeči.

    ![Otevření v prohlížeči lokality](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a>Povolení technologie WebSockets ve službě Azure App Service Web Apps

Technologie WebSockets musí být explicitně povolená ve vaší webové aplikaci mají být použity v SignalR aplikace; jinak, bude třeba použít jiné protokoly (viz [přenosy a případech Přejít](../getting-started/introduction-to-signalr.md#transports) podrobnosti).

Chcete-li použít objekty WebSockets v Azure App Service Web Apps, povolte v části Konfigurace webové aplikace. K tomu, otevřete webovou aplikaci v [portálu pro správu Azure](https://manage.windowsazure.com/)a vyberte položku konfigurace.

![Karta Konfigurace](using-signalr-with-azure-web-sites/_static/image8.png)

V horní části stránky konfigurace Ujistěte se, .NET 4.5 slouží pro vaši webovou aplikaci.

![Nastavení rozhraní .NET framework verze 4.5](using-signalr-with-azure-web-sites/_static/image9.png)

Na stránce konfigurace v **Websocket** nastavení, vyberte **na**.

![Technologie WebSockets nastavení: na](using-signalr-with-azure-web-sites/_static/image10.png)

V dolní části stránky konfigurace, vyberte **Uložit** uložte provedené změny.

![Uložit nastavení](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a>Pomocí propojovacího rozhraní mezipaměť Redis systému Azure

Pokud používáte více instancí pro vaši webovou aplikaci a uživatele těchto instancí muset vzájemné interakce (tak, aby, například chat zpráv vytvořených za jednu instanci dosáhnout uživatelů připojených k další instance), [Azure Redis Cache Propojovací rozhraní](../performance/scaleout-with-redis.md) musí být implementován v aplikaci.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Další kroky

Další informace o webových aplikací ve službě Azure App Service naleznete v tématu [přehled Web Apps](https://azure.microsoft.com/en-us/documentation/articles/app-service-web-overview/).
