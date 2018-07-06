---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Hostovat rozhraní ASP.NET Web API 2 v roli pracovního procesu Azure | Dokumentace Microsoftu
author: MikeWasson
description: Tento kurz ukazuje, jak hostovat rozhraní ASP.NET Web API v roli pracovního procesu Azure používá OWIN k samoobslužnému hostování webového rozhraní API rozhraní framework. Otevřít Web Interface pro .NET (OWIN) de...
ms.author: aspnetcontent
ms.date: 04/02/2014
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: c53256b8a72a377f51b9fbac7944657cb6d4c6e4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803847"
---
<a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a>Hostovat rozhraní ASP.NET Web API 2 v roli pracovního procesu Azure
====================
podle [Mike Wasson](https://github.com/MikeWasson)

> Tento kurz ukazuje, jak hostovat rozhraní ASP.NET Web API v roli pracovního procesu Azure používá OWIN k samoobslužnému hostování webového rozhraní API rozhraní framework.
> 
> [Otevřete Web Interface pro .NET](http://owin.org/) (OWIN) definuje abstrakce mezi .NET webové servery a webové aplikace. Webové aplikace ze serveru, který je ideální pro webové aplikace ve vašem vlastním procesu mimo službu IIS s vlastním hostováním OWIN OWIN odděluje obě části – například v roli pracovního procesu systému Azure.
> 
> V tomto kurzu budete používat Microsoft.Owin.Host.HttpListener balíček, který poskytuje HTTP server, který použije k samoobslužnému hostování aplikací OWIN.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Webové rozhraní API 2
> - [Sada Azure SDK for .NET 2.3](https://azure.microsoft.com/downloads/)


## <a name="create-a-microsoft-azure-project"></a>Vytvořte projekt Microsoft Azure

Spusťte Visual Studio s oprávněními správce. Oprávnění správce jsou potřebné k ladění aplikace v místním prostředí, pomocí emulátoru služby výpočty v Azure.

Na **souboru** nabídky, klikněte na tlačítko **nový**, pak klikněte na tlačítko **projektu**. Z **nainstalované šablony**, v části Visual C#, klikněte na tlačítko **cloudu** a potom klikněte na tlačítko **Windows Azure Cloud Service**. Pojmenujte projekt "AzureApp" a klikněte na tlačítko **OK**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

V **nová Cloudová služba Windows Azure** dialogového okna, dvakrát klikněte na panel **Role pracovního procesu**. Ponechte výchozí název ("WorkerRole1"). Tento krok přidává role pracovního procesu k řešení. Klikněte na tlačítko **OK**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

Řešení sady Visual Studio, který je vytvořen obsahuje dva projekty:

- &quot;AzureApp&quot; definuje role a konfigurace pro aplikace Azure.
- &quot;WorkerRole1&quot; obsahuje kód pro roli pracovního procesu.

Obecně platí aplikace Azure může obsahovat více rolí, i když tento kurz používá jednu roli.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a>Přidání webového rozhraní API a balíčky OWIN

Z **nástroje** nabídky, klikněte na tlačítko **Správce balíčků knihoven**, pak klikněte na tlačítko **Konzola správce balíčků**.

V okně konzoly Správce balíčků zadejte následující příkaz:

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Přidat koncový bod HTTP

V Průzkumníku řešení rozbalte projekt AzureApp. Rozbalte uzel role, klikněte pravým tlačítkem na WorkerRole1 a vyberte **vlastnosti**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

Klikněte na tlačítko **koncové body**a potom klikněte na tlačítko **přidat koncový bod**.

V **protokol** rozevíracího seznamu vyberte možnost "http". V **veřejný Port** a **privátní Port**, zadejte 80. Tato čísla portů se mohou lišit. Veřejný port je, co klienti používat při odesílání požadavku do role.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a>Konfigurace webového rozhraní API pro hostování na vlastním serveru

V Průzkumníku řešení klikněte pravým tlačítkem myši klikněte na WorkerRole1 projekt a vyberte **přidat** / **třídy** přidání nové třídy. Název třídy `Startup`.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

Nahraďte všechny často používaný kód v tomto souboru následujícím kódem:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a>Přidat kontroler rozhraní Web API

Dále přidejte třídu kontroleru webového rozhraní API. Klikněte pravým tlačítkem na projekt WorkerRole1 a vyberte **přidat** / **třídy**. Název třídy TestController. Nahraďte všechny často používaný kód v tomto souboru následujícím kódem:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

Pro zjednodušení tohoto kontroleru pouze definuje dvě metody GET, které vracejí prostý text.

## <a name="start-the-owin-host"></a>Spuštění hostitele OWIN

Otevřete soubor WorkerRole.cs. Tato třída definuje kód, který se spustí při spuštění a zastavení role pracovního procesu.

Přidejte následující příkaz using:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

Přidat **IDisposable** člena `WorkerRole` třídy:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

V `OnStart` metodu, přidejte následující kód pro spuštění hostitele:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

**WebApp.Start** metoda spustí hostitele OWIN. Název `Startup` třídy je parametr typu metody. Podle konvence, bude volat hostitele `Configure` metody této třídy.

Přepsat `OnStop` a neprovede  *\_aplikace* instance:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

Tady je kompletní kód WorkerRole.cs:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

Sestavte řešení a stisknutím klávesy F5 spusťte aplikaci místně v emulátoru Azure Compute. V závislosti na nastavení brány firewall potřebujete povolit emulátor přes bránu firewall.

> [!NOTE]
> Pokud dojde k výjimce následujícím postupem, najdete [tento příspěvek na blogu](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) alternativní řešení. "Nelze načíst soubor nebo sestavení" Microsoft.Owin, verze = 2.0.2.0, jazyková verze = neutral, PublicKeyToken = 31bf3856ad364e35' nebo některou z jeho závislostí. Definice manifestu sestavení neodpovídá odkazu na sestavení. (Výjimka z HRESULT: 0x80131040) "


Emulátor služby výpočty místní IP adresa přiřadí ke koncovému bodu. IP adresu můžete najít zobrazením Uživatelském prostředí emulátoru výpočtů. Klikněte pravým tlačítkem na hlavním panelu oznamovací oblasti ikonu emulátoru a vyberte **zobrazit Uživatelském prostředí emulátoru výpočtů**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

Zjistit IP adresu v rámci nasazení služeb, nasazení [id], podrobnosti o službě. Otevřete webový prohlížeč a přejdete na http://<em>adresu</em>/testovací/1, kde <em>adresu</em> je IP adresa přidělí emulátor služby výpočty; například `http://127.0.0.1:80/test/1`. Měli byste vidět odpovědi z kontroleru webového rozhraní API:

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a>Nasazení do Azure

Pro tento krok musí mít účet Azure. Pokud již nemáte, můžete během několika minut vytvořit Bezplatný zkušební účet. Podrobnosti najdete v tématu [bezplatná zkušební verze Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

V Průzkumníku řešení klikněte pravým tlačítkem na projekt AzureApp. Vyberte **publikovat**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

Pokud nejste přihlášení k účtu Azure, klikněte na tlačítko **Sign In**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

Když jste přihlášeni, zvolte předplatné a klikněte na **Další**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

Zadejte název pro cloudovou službu a zvolte oblast. Klikněte na tlačítko **vytvořit**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

Klikněte na tlačítko **publikovat**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

Okno Protokol aktivit Azure zobrazuje průběh nasazení. Když je aplikace nasazena, přejdete k http://appname.cloudapp.net/test/1.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a>Další prostředky

- [Přehled projektu Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [Katana projektu na Githubu](https://github.com/aspnet/AspNetKatana)
