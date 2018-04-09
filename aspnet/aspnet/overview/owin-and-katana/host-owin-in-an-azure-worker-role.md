---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: Hostování OWIN roli pracovního procesu Azure | Microsoft Docs
author: MikeWasson
description: Tento kurz ukazuje, jak v roli pracovního procesu Microsoft Azure vlastní hostování OWIN. Open Web Interface pro .NET (OWIN) definuje abstrakci mezi .NET webový server...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/11/2014
ms.topic: article
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 13bccc4b2d6f1b22c94446deaf6795dab766275b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="host-owin-in-an-azure-worker-role"></a>Hostitele OWIN roli pracovního procesu systému Azure
====================
podle [Wasson Jan](https://github.com/MikeWasson)

> Tento kurz ukazuje, jak v roli pracovního procesu Microsoft Azure vlastní hostování OWIN.
> 
> [Otevřete Web Interface pro .NET](http://owin.org/) (OWIN) definuje abstrakci mezi .NET webové servery a webové aplikace. OWIN oddělí webové aplikace ze serveru, takže je ideální pro hostování samoobslužné webové aplikace v vlastní proces mimo službu IIS OWIN – například uvnitř o roli pracovního procesu systému Azure.
> 
> V tomto kurzu budete zjistěte, jak k hostování na vlastním aplikace OWIN uvnitř roli pracovního procesu Microsoft Azure. Další informace o rolích pracovního procesu naleznete v tématu [Azure provádění modely](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [Azure SDK pro .NET 2.3](https://azure.microsoft.com/downloads/)
> - [Microsoft.Owin.Selfhost 2.1.0](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a>Vytvoření projektu Microsoft Azure

Spuštění sady Visual Studio s oprávněními správce. K ladění aplikace místně, pomocí emulátor výpočtů v Azure jsou potřeba oprávnění správce.

Na **soubor** nabídky, klikněte na tlačítko **nový**, pak klikněte na tlačítko **projektu**. Z **nainstalovaných šablonách**, v části Visual C#, klikněte na **cloudu** a pak klikněte na **Windows Azure Cloud Service**. Název projektu "AzureApp" a klikněte na tlačítko **OK**.

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

V **nová Cloudová služba Windows Azure** dialogové okno, dvakrát klikněte na **Role pracovního procesu**. Ponechte výchozí název ("WorkerRole1"). Tento krok přidává roli pracovního procesu k řešení. Click **OK**.

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

Řešení nástroje Visual Studio, který se vytvoří obsahuje dva projekty:

- &quot;AzureApp&quot; definuje role a konfigurace pro aplikaci Azure.
- &quot;WorkerRole1&quot; obsahuje kód pro roli pracovního procesu.

Obecně platí aplikaci Azure může obsahovat víc rolí, i když tento kurz používá jednu roli.

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a>Přidání balíčků hostování na vlastním serveru OWIN

Z **nástroje** nabídky, klikněte na tlačítko **Správce balíčků knihoven**, pak klikněte na tlačítko **Konzola správce balíčků**.

V okně konzoly Správce balíčků zadejte následující příkaz:

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Přidání koncového bodu protokolu HTTP

V Průzkumníku řešení rozbalte AzureApp projektu. Rozbalte uzel role, klikněte pravým tlačítkem na WorkerRole1 a vyberte **vlastnosti**.

![](host-owin-in-an-azure-worker-role/_static/image6.png)

Klikněte na tlačítko **koncové body**a potom klikněte na **přidat koncový bod**.

V **protokol** rozevíracího seznamu vyberte možnost "http". V **veřejný Port** a **privátní Port**, zadejte 80. Tato čísla portů se může lišit. Veřejný port je co klienti používat při odesílání požadavku do role.

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a>Vytvoření třídy pro spuštění OWIN

V Průzkumníku řešení klikněte pravým tlačítkem na projekt WorkerRole1 a vyberte **přidat** / **třída** pro přidání nové třídy. Název třídy `Startup`.

Nahraďte všechny často používaný kód s následujícími službami:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

`UseWelcomePage` Metoda rozšíření přidá jednoduchá stránka HTML k vaší aplikaci, ověřte funkčnost webu.

## <a name="start-the-owin-host"></a>Spuštění hostitele OWIN

Otevřete soubor WorkerRole.cs. Tato třída definuje kód, který je spuštěn při spuštění a zastavení role pracovního procesu.

Přidejte následující příkaz using:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

Přidat **IDisposable** člena `WorkerRole` třídy:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

V `OnStart` metoda, přidejte následující kód pro spuštění hostitele:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

**WebApp.Start** metoda spustí hostitele OWIN. Název `Startup` třída je typ parametru metody. Podle konvence, bude volat hostitele `Configure` metoda této třídy.

Přepsání `OnStop` k odstranění  *\_aplikace* instance:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

Tady je kompletní kód WorkerRole.cs:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

Sestavte řešení a stisknutím klávesy F5 místně spustíte aplikaci v emulátoru výpočetní Azure. V závislosti na nastavení brány firewall může musíte povolit emulátoru prostřednictvím brány firewall.

Emulátoru služby výpočty v přiřadí místní IP adresu ke koncovému bodu. Zobrazením uživatelské prostředí emulátoru výpočtů můžete najít IP adresu. Klikněte pravým tlačítkem myši na ikonu emulátoru na hlavním panelu oznamovací oblasti a vyberte **zobrazit uživatelské prostředí emulátoru výpočtů**.

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

Najít IP adresu v rámci nasazení služby, nasazení [id] podrobnosti ze služby. Otevřete webový prohlížeč a přejděte na http://<em>adresu</em>, kde <em>adresu</em> je IP adresa přiřadila emulátoru služby výpočty v; například `http://127.0.0.1:80`. Měli byste vidět úvodní stránku OWIN:

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a>Nasazení do Azure

V tomto kroku musí mít účet Azure. Pokud jste již nemáte, můžete si během několika minut vytvořit Bezplatný zkušební účet. Podrobnosti najdete v tématu [bezplatná zkušební verze Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

V Průzkumníku řešení klikněte pravým tlačítkem na projekt AzureApp. Vyberte **publikování**.

![](host-owin-in-an-azure-worker-role/_static/image12.png)

Pokud nejste přihlášení k účtu Azure, klikněte na tlačítko **přihlásit**.

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

Když jste přihlášeni, vyberte předplatné a klikněte na **Další**.

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

Zadejte název pro cloudové služby a vyberte oblast. Klikněte na tlačítko **vytvořit**.

![](host-owin-in-an-azure-worker-role/_static/image17.png)

Klikněte na tlačítko **publikování**.

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

Okno Protokol činnosti Azure zobrazuje průběh nasazení. Při nasazení aplikace, přejděte do `http://appname.cloudapp.net/`, kde *appname* je název cloudové služby.

## <a name="additional-resources"></a>Další prostředky

- [Přehled projektu Katana](an-overview-of-project-katana.md)
- [Katana projektu na Githubu](https://github.com/aspnet/AspNetKatana/)
