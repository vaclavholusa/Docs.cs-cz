---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Hostitel rozhraní ASP.NET Web API 2 roli pracovního procesu Azure | Microsoft Docs
author: MikeWasson
description: Tento kurz ukazuje, jak k hostování role pracovního procesu Azure, rozhraní ASP.NET Web API pomocí k hostování na vlastním rozhraní Web API OWIN. Otevřít Web Interface pro .NET (OWIN) de...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/02/2014
ms.topic: article
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 7ba1dc850e2f9d9c88e6ddf263a796e1867a98be
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873648"
---
<a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a>Hostitel rozhraní ASP.NET Web API 2 roli pracovního procesu systému Azure
====================
podle [Wasson Jan](https://github.com/MikeWasson)

> Tento kurz ukazuje, jak k hostování role pracovního procesu Azure, rozhraní ASP.NET Web API pomocí k hostování na vlastním rozhraní Web API OWIN.
> 
> [Otevřete Web Interface pro .NET](http://owin.org/) (OWIN) definuje abstrakci mezi .NET webové servery a webové aplikace. OWIN oddělí webové aplikace ze serveru, takže je ideální pro hostování samoobslužné webové aplikace v vlastní proces mimo službu IIS OWIN – například uvnitř o roli pracovního procesu systému Azure.
> 
> V tomto kurzu budete používat Microsoft.Owin.Host.HttpListener balíčku, který poskytuje HTTP server, použije k hostování na vlastním aplikací OWIN.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Webové rozhraní API 2
> - [Azure SDK pro .NET 2.3](https://azure.microsoft.com/downloads/)


## <a name="create-a-microsoft-azure-project"></a>Vytvoření projektu Microsoft Azure

Spuštění sady Visual Studio s oprávněními správce. K ladění aplikace místně, pomocí emulátor výpočtů v Azure jsou potřeba oprávnění správce.

Na **soubor** nabídky, klikněte na tlačítko **nový**, pak klikněte na tlačítko **projektu**. Z **nainstalovaných šablonách**, v části Visual C#, klikněte na **cloudu** a pak klikněte na **Windows Azure Cloud Service**. Název projektu "AzureApp" a klikněte na tlačítko **OK**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

V **nová Cloudová služba Windows Azure** dialogové okno, dvakrát klikněte na **Role pracovního procesu**. Ponechte výchozí název ("WorkerRole1"). Tento krok přidává roli pracovního procesu k řešení. Click **OK**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

Řešení nástroje Visual Studio, který se vytvoří obsahuje dva projekty:

- &quot;AzureApp&quot; definuje role a konfigurace pro aplikaci Azure.
- &quot;WorkerRole1&quot; obsahuje kód pro roli pracovního procesu.

Obecně platí aplikaci Azure může obsahovat víc rolí, i když tento kurz používá jednu roli.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a>Přidání webového rozhraní API a balíčky OWIN

Z **nástroje** nabídky, klikněte na tlačítko **Správce balíčků knihoven**, pak klikněte na tlačítko **Konzola správce balíčků**.

V okně konzoly Správce balíčků zadejte následující příkaz:

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Přidání koncového bodu protokolu HTTP

V Průzkumníku řešení rozbalte AzureApp projektu. Rozbalte uzel role, klikněte pravým tlačítkem na WorkerRole1 a vyberte **vlastnosti**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

Klikněte na tlačítko **koncové body**a potom klikněte na **přidat koncový bod**.

V **protokol** rozevíracího seznamu vyberte možnost "http". V **veřejný Port** a **privátní Port**, zadejte 80. Tato čísla portů se může lišit. Veřejný port je co klienti používat při odesílání požadavku do role.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a>Konfigurace webového rozhraní API pro hostování na vlastním serveru

V Průzkumníku řešení klikněte pravým tlačítkem na projekt WorkerRole1 a vyberte **přidat** / **třída** pro přidání nové třídy. Název třídy `Startup`.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

Všechny často používaný kód v tomto souboru nahraďte následujícím textem:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a>Přidat řadič webové rozhraní API

V dalším kroku přidejte třídu kontroleru webového rozhraní API. Klikněte pravým tlačítkem na projekt WorkerRole1 a vyberte **přidat** / **třída**. Název třídy TestController. Všechny často používaný kód v tomto souboru nahraďte následujícím textem:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

Pro jednoduchost definuje tento řadič jen dvě metody GET, které vracejí prostý text.

## <a name="start-the-owin-host"></a>Spuštění hostitele OWIN

Otevřete soubor WorkerRole.cs. Tato třída definuje kód, který je spuštěn při spuštění a zastavení role pracovního procesu.

Přidejte následující příkaz using:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

Přidat **IDisposable** člena `WorkerRole` třídy:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

V `OnStart` metoda, přidejte následující kód pro spuštění hostitele:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

**WebApp.Start** metoda spustí hostitele OWIN. Název `Startup` třída je typ parametru metody. Podle konvence, bude volat hostitele `Configure` metoda této třídy.

Přepsání `OnStop` k odstranění  *\_aplikace* instance:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

Tady je kompletní kód WorkerRole.cs:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

Sestavte řešení a stisknutím klávesy F5 místně spustíte aplikaci v emulátoru výpočetní Azure. V závislosti na nastavení brány firewall může musíte povolit emulátoru prostřednictvím brány firewall.

> [!NOTE]
> Pokud dojde k výjimce takto, najdete v tématu [tomto příspěvku na blogu](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) alternativní řešení. "Nelze načíst soubor nebo sestavení ' Microsoft.Owin, verze = 2.0.2.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35, nebo jeden z jeho závislých. Definice sestavení manifestu neodpovídá odkaz na sestavení. (Výjimka z HRESULT: 0x80131040) "


Emulátoru služby výpočty v přiřadí místní IP adresu ke koncovému bodu. Zobrazením uživatelské prostředí emulátoru výpočtů můžete najít IP adresu. Klikněte pravým tlačítkem myši na ikonu emulátoru na hlavním panelu oznamovací oblasti a vyberte **zobrazit uživatelské prostředí emulátoru výpočtů**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

Najít IP adresu v rámci nasazení služby, nasazení [id] podrobnosti ze služby. Otevřete webový prohlížeč a přejděte na http://<em>adresu</em>/testovací/1, kde <em>adresu</em> je IP adresa přiřadila emulátoru služby výpočty v; například `http://127.0.0.1:80/test/1`. Měli byste vidět odpovědi z kontroleru webového rozhraní API:

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a>Nasazení do Azure

V tomto kroku musí mít účet Azure. Pokud jste již nemáte, můžete si během několika minut vytvořit Bezplatný zkušební účet. Podrobnosti najdete v tématu [bezplatná zkušební verze Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

V Průzkumníku řešení klikněte pravým tlačítkem na projekt AzureApp. Vyberte **publikování**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

Pokud nejste přihlášení k účtu Azure, klikněte na tlačítko **přihlásit**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

Když jste přihlášeni, vyberte předplatné a klikněte na **Další**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

Zadejte název pro cloudové služby a vyberte oblast. Klikněte na tlačítko **vytvořit**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

Klikněte na tlačítko **publikování**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

Okno Protokol činnosti Azure zobrazuje průběh nasazení. Při nasazení aplikace, přejděte do http://appname.cloudapp.net/test/1.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a>Další prostředky

- [Přehled projektu Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [Katana projektu na Githubu](https://github.com/aspnet/AspNetKatana)
