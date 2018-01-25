---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: "Škálování SignalR službou Azure Service Bus | Microsoft Docs"
author: MikeWasson
description: "Verze softwaru v této verzi Visual Studio 2013 .NET 4.5 SignalR tématu používat 2 předchozí verzích tohoto tématu pro SignalR 1.x verzi tohoto tématu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 7cb68d578fee8d6ee036f8fb096ba45e0c8ef3d6
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="signalr-scaleout-with-azure-service-bus"></a>Škálování SignalR službou Azure Service Bus
====================
podle [Karel Wasson](https://github.com/MikeWasson), [Patrik Fletcher](https://github.com/pfletcher)

V tomto kurzu nasadíte aplikaci SignalR webové roli systému Windows Azure, pomocí Service Bus propojovacího rozhraní k distribuci zpráv každá instance role. (Můžete použít také propojovacího rozhraní služby Service Bus s [webové aplikace v Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

Předpoklady:

- Účet služby Windows Azure.
- [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).
- Visual Studio 2012 nebo 2013.

Propojovacího rozhraní service bus je také kompatibilní s [sběrnice služby pro Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), verze 1.1. Však není kompatibilní s verzí 1.0 sběrnice služby pro Windows Server.

## <a name="pricing"></a>Ceny

Service Bus propojovacího rozhraní témata týkající se používá k odesílání zpráv. Cenová nejnovější informace najdete v tématu [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/). V době psaní tohoto textu můžete odeslat 1 000 000 zpráv za měsíc pro menší než 1 USD. Propojovacího rozhraní odešle zprávu sběrnice služby pro každé vyvolání metody rozbočovače SignalR. Existují také některé řízení zpráv pro připojení, odpojení, spojující a výstupu skupiny a tak dále. Ve většině aplikací se bude většinu přenosu zpráv volání metod rozbočovače.

## <a name="overview"></a>Přehled

Předtím, než se nám získat podrobný kurz, zde je rychlý přehled toho, co jste se dělat.

1. Použití portálu Windows Azure k vytvoření nového oboru názvů Service Bus.
2. Přidejte tyto balíčky NuGet do vaší aplikace: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.ServiceBus](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. Vytvoření aplikace SignalR.
4. Přidejte následující kód do souboru Startup.cs konfigurace propojovacího rozhraní: 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

Tento kód konfiguruje propojovacího rozhraní s výchozími hodnotami u [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) a [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx). Informace o změně těchto hodnot najdete v tématu [výkonu SignalR: metriky škálování](signalr-performance.md#scaleout_metrics).

Pro každou aplikaci vyberte jinou hodnotu pro "YourAppName". Nepoužívejte stejnou hodnotu napříč různými aplikacemi.

## <a name="create-the-azure-services"></a>Vytvoření služby Azure

Vytvoření cloudové služby, jak je popsáno v [postup vytvoření a nasazení cloudové služby](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy). Postupujte podle kroků v části "postup: vytvoření cloudové služby, pomocí funkce rychle vytvořit". V tomto kurzu není potřeba nahrát certifikát.

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

Vytvořit nový obor názvů Service Bus, jak je popsáno v [jak použití Service Bus témata nebo odběry](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions). Postupujte podle kroků v části "Vytvoření služby Namespace".

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> Ujistěte se, že vyberte stejnou oblast pro cloudové služby a oboru názvů Service Bus.


## <a name="create-the-visual-studio-project"></a>Vytvoření projektu Visual Studio

Spuštění sady Visual Studio. Z **soubor** nabídky, klikněte na tlačítko **nový projekt**.

V **nový projekt** dialogové okno, rozbalte seznam **Visual C#**. V části **nainstalovaných šablonách**, vyberte **cloudu** a pak vyberte **Windows Azure Cloud Service**. Ponechte výchozí rozhraní .NET Framework 4.5. Název aplikace ChatService a klikněte na tlačítko **OK**.

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

V **nová Cloudová služba Windows Azure** dialogovém okně, vyberte webovou roli ASP.NET. Klikněte na tlačítko se šipkou vpravo (**&gt;**) přidejte roli pro vaše řešení.

Najeďte myší novou roli, tak na ikonu tužky viditelné. Kliknutím na tuto ikonu přejmenujte roli. Název role "SignalRChat" a klikněte na tlačítko **OK**.

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

V **nový projekt ASP.NET** dialogovém okně, vyberte **MVC**a klikněte na tlačítko OK.

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

Průvodce projektem vytvoří dva projekty:

- ChatService: Tento projekt je aplikace systému Windows Azure. Definuje Azure role a další možnosti konfigurace.
- SignalRChat: Tento projekt je projektu ASP.NET MVC 5.

## <a name="create-the-signalr-chat-application"></a>Vytvoření aplikace SignalR Chat

Vytvoření chatovací aplikace, postupujte podle kroků v tomto kurzu [Začínáme s SignalR a MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

Instalace potřebných knihoven pomocí NuGet. Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**, pak vyberte **Konzola správce balíčků**. V **Konzola správce balíčků** okno, zadejte následující příkazy:

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

Použití `-ProjectName` možnost instalovat balíčky do projektu ASP.NET MVC, nikoli projekt systému Windows Azure.

## <a name="configure-the-backplane"></a>Konfigurace propojovacího rozhraní

V souboru Startup.cs vaší aplikace přidejte následující kód:

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

Teď je potřeba získat připojovací řetězec service bus. Na portálu Azure vyberte obor názvů sběrnice služby, kterou jste vytvořili a klikněte na ikonu přístupový klíč.

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

Zkopírujte připojovací řetězec do schránky a vložte ji do *connectionString* proměnné.

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>Nasazení do Azure

V Průzkumníku řešení rozbalte **role** složky uvnitř ChatService projektu.

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

Klikněte pravým tlačítkem na roli SignalRChat a vyberte **vlastnosti**. Vyberte **konfigurace** kartě. V části **instance** vyberte 2. Můžete také nastavit velikost virtuálního počítače na **navíc malé**.

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

Uložte změny.

V Průzkumníku řešení klikněte pravým tlačítkem na projekt ChatService. Vyberte **publikování**.

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

Pokud je vaše první čas publikování do služby Windows Azure, je nutné stáhnout přihlašovací údaje. V **publikovat** průvodce, klikněte na tlačítko "Přihlásit se k stáhnout přihlašovací údaje". To vás vyzve k přihlásit k portálu Windows Azure a stáhněte soubor nastavení publikování.

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

Klikněte na tlačítko **Import** a vyberte soubor nastavení publikování, který jste si stáhli.

Klikněte na tlačítko **Další**. V **nastavení publikování** dialogové okno, v části **Cloudová služba**, vyberte cloudovou službu, kterou jste vytvořili dříve.

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

Klikněte na tlačítko **publikování**. Ho může trvat několik minut a nasaďte aplikaci spustit virtuální počítače.

Při spuštění aplikace chat instance rolí komunikovat přes Azure Service Bus pomocí tématu Service Bus. Téma je fronta zpráv, který umožňuje více odběrateli.

Propojovacího rozhraní automaticky vytvoří téma a odběrů. Odběry a aktivitu zprávu zobrazíte otevřete portál Azure, vyberte obor názvů Service Bus a klikněte na "Témata".

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

Ujistěte se, trvat několik minut pro aktivitu zprávu objeví v řídicím panelu.

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

SignalR spravuje životnost tématu. Tak dlouho, dokud vaše aplikace je nasazená, nemusíte zkuste ručně odstraňte témata nebo změňte nastavení v tomto tématu.

## <a name="troubleshooting"></a>Poradce při potížích

**System.InvalidOperationException "Pouze podporované IsolationLevel je 'IsolationLevel.Serializable'."**

Této chybě může dojít, pokud je úroveň transakce operace nastavená na jinou hodnotu než `Serializable`. Ověřte, že se žádné operace probíhají s další úrovní transakcí.
