---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: Pomocí čítače výkonu SignalR webové role Azure | Microsoft Docs
author: guardrex
description: Jak nainstalovat a používat čítače výkonu SignalR webové role Azure.
keywords: Čítač ASP.NET,signalr,Performance, webové role azure
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2017
ms.topic: article
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 2f6c6feb030fc17f95e7862c39029569f3d8c5dc
ms.sourcegitcommit: d8aa1d314891e981460b5e5c912afb730adbb3ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/05/2018
ms.locfileid: "28988013"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a>Pomocí čítače výkonu SignalR webové role Azure

Podle [Luke Latham](https://github.com/guardrex)

Čítače výkonu SignalR se používají ke sledování výkonu vaší aplikace webové role Azure. Čítače jsou zachyceny Microsoft Azure Diagnostics. Čítače výkonu SignalR nainstalujete v Azure s *signalr.exe*, stejný nástroj, který slouží pro samostatnou nebo místní aplikace. Vzhledem k tomu, že jsou přechodné Azure role, můžete aplikaci nakonfigurovat tak, instalace a registrace čítače výkonu SignalR při spuštění.

## <a name="prerequisites"></a>Požadavky

* [Visual Studio 2015](https://www.visualstudio.com/vs/visual-studio-express/)
* [Microsoft Azure SDK pro Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **Poznámka: Po instalaci sady SDK restartovat svůj počítač.**
* Předplatné Microsoft Azure: Chcete-li si zaregistrovat Bezplatný zkušební účet Azure, přečtěte si téma [bezplatná zkušební verze Azure](https://azure.microsoft.com/free/).

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a>Vytvoření aplikace Azure webovou roli, která zpřístupňuje čítače výkonu SignalR

1. Open Visual Studio 2015.

2. V sadě Visual Studio 2015, vyberte **soubor** > **nový** > **projektu**.

3. V **šablony** podokně **nový projekt** v části **Visual C#** uzlu, vyberte **cloudu** uzel a vyberte možnost **Azure Cloud Service** šablony. Název aplikace **SignalRPerfCounters** a vyberte **OK**.

   ![Nové cloudové aplikace](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)
    
4. V **nová Cloudová služba Microsoft Azure** dialogovém okně, vyberte **webovou roli ASP.NET** a vyberte > tlačítko do projektu přidejte roli. Vyberte **OK**.

   ![Přidat webová Role ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)
    
5. V **nové webové aplikace ASP.NET - WebRole1** dialogovém okně, vyberte **MVC** šablony a potom vyberte **OK**.

   ![Přidání MVC a webových rozhraní API](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)
    
6. V **Průzkumníku řešení**, otevřete *diagnostics.wadcfgx* souboru pod **WebRole1**.

   ![Diagnostics.wadcfgx Průzkumník řešení](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)
    
7. Nahraďte obsah souboru s následující konfigurací a soubor uložte:

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]
    
8. Otevřete **Konzola správce balíčků** z **nástroje** > **Správce balíčků NuGet**. Zadejte následující příkazy pro instalaci nejnovější verze SignalR a balíček nástroje SignalR:

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]
    
9. Nakonfiguruje aplikaci, aby při spuštění nebo recykluje nainstalovat čítače výkonu SignalR do role instance. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **WebRole1** projektu a vyberte **přidat** > **novou složku**. Název nové složky *spuštění*.

   ![Přidání spouštěcí složka](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)
    
10. Kopírování *signalr.exe* souboru (přidán s **Microsoft.AspNet.SignalR.Utils** balíčku) z \<složky projektu > / SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\< verze > / tools k *spuštění* složky, které jste vytvořili v předchozím kroku.

11. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *spuštění* složky a vyberte **přidat** > **existující položka**. V dialogovém okně se zobrazí, vyberte *signalr.exe* a vyberte **přidat**.

    ![Přidání signalr.exe do projektu](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)
    
12. Klikněte pravým tlačítkem na *spuštění* složky, které jste vytvořili. Vyberte **přidat** > **novou položku**. Vyberte **Obecné** uzlu, vyberte **textový soubor**a pojmenujte novou položku *SignalRPerfCounterInstall.cmd*. Tento příkaz soubor nainstaluje do webovou roli čítače výkonu SignalR.

    ![Vytvoření SignalR výkonu čítač instalace dávkového souboru](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)
     
13. Když Visual Studio vytvoří *SignalRPerfCounterInstall.cmd* souboru, se automaticky otevře v hlavním okně. Obsah souboru nahraďte následující skript, pak uložte a zavřete soubor. Tento skript spustí *signalr.exe*, k instanci role přidává čítače výkonu SignalR.

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]
    
14. Vyberte *signalr.exe* souboru v **Průzkumníku řešení**. V souboru **vlastnosti**, nastavte **kopírovat do výstupního adresáře** k **kopie vždy**.

    ![Nastavit kopírovat do výstupního adresáře pro kopírování vždy](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)
    
15. Opakujte předchozí krok pro *SignalRPerfCounterInstall.cmd* souboru.

    
16. Klikněte pravým tlačítkem na *SignalRPerfCounterInstall.cmd* soubor a vyberte **otevřít v**. V dialogovém okně se zobrazí, vyberte **binární Editor** a vyberte **OK**.

    ![Otevřete s binární Editor](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)
    
17. V editoru binární vyberte všechny úvodní bajty v souboru a odstraňte je. Soubor uložte a zavřete.

    ![Odstranit úvodní bajty](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)
    
18. Otevřete *ServiceDefinition.csdef* a přidat úloha spuštění, které provádí *SignalrPerfCounterInstall.cmd* souborů při spuštění služby:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]
    
19. Otevřete `Views/Shared/_Layout.cshtml` a odeberte skript sady jQuery od konce souboru.

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]
    
20. Přidat JavaScript klienta, který volá nepřetržitě `increment` metoda na serveru. Otevřete `Views/Home/Index.cshtml` a nahraďte jeho obsah následujícím kódem:

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]
    
21. Vytvořte novou složku v **WebRole1** projektu s názvem *Hubs*. Klikněte pravým tlačítkem myši *Hubs* složky v **Průzkumníku řešení**, vyberte **webové** > **SignalR**a vyberte  **Třídy rozbočovače SignalR (v2)**. Název nového centra *MyHub.cs* a vyberte **přidat**.

    ![Přidání třídy rozbočovače SignalR do složky Hubs v dialogovém okně Přidat novou položku](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. *MyHub.cs* se automaticky otevře v hlavním okně. Nahraďte obsah následujícím kódem, pak uložte a zavřete soubor:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]
    
23. *[Crank.exe](signalr-connection-density-testing-with-crank.md)*  je připojení hustotu testování nástroj, který poskytuje s SignalR codebase. Vzhledem k tomu, že Crank vyžaduje trvalé připojení, můžete přidat na váš web pro použití při testování. Přidat novou složku do **WebRole1** projekt s názvem *PersistentConnections*. Klikněte pravým tlačítkem na tuto složku a vyberte **přidat** > **třída**. Název nového souboru třídy *MyPersistentConnections.cs* a vyberte **přidat**.

24. Otevře se Visual Studio *MyPersistentConnections.cs* souboru v hlavním okně. Nahraďte obsah následujícím kódem, pak uložte a zavřete soubor:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]
    
25. Pomocí `Startup` třídu, objekty SignalR spustit při spuštění OWIN. Otevřít nebo vytvořit *Startup.cs* a nahraďte jeho obsah následujícím kódem:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]
    
    Ve výše, kódu `OwinStartup` atribut určí této třídy lze spustit OWIN. `Configuration` Metoda spustí SignalR.
    
26. Otestovat aplikaci v emulátoru Microsoft Azure stisknutím **F5**.

    > [!NOTE]
    > Pokud narazíte **FileLoadException** v **MapSignalR**, změnit přesměrování vazby v *web.config* pro následující:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]
    
27. Počkejte přibližně jednu minutu. Otevřete okno Průzkumník cloudu nástroje v sadě Visual Studio (**zobrazení** > **Průzkumník cloudu**) a rozbalte cestu `(Local)/Storage Accounts/(Development)/Tables`. Klikněte dvakrát na **WADPerformanceCountersTable**. Měli byste vidět čítače SignalR v datech tabulky. Pokud nevidíte v tabulce, musíte znovu zadat přihlašovací údaje Azure Storage. Je nutné vybrat **aktualizovat** tlačítko najdete v tabulce v **Průzkumník cloudu** nebo vyberte **aktualizovat** tlačítka v okně Otevřít tabulku se zobrazí data v tabulce.

    ![Výběr v tabulce čítače výkonu WAD v sadě Visual Studio Průzkumníku cloudu](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Zobrazuje čítače shromážděné v tabulce WAD čítače výkonu](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)
    
28. Testování vaší aplikace v cloudu aktualizujte **ServiceConfiguration.Cloud.cscfg** souboru a nastavit `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` na platný připojovací řetězec účet úložiště Azure.

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. Nasaďte aplikaci do vašeho předplatného Azure. Podrobnosti o tom, jak nasadit aplikaci do Azure najdete v tématu [postup vytvoření a nasazení cloudové služby](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).

30. Počkejte několik minut. V **Průzkumník cloudu**, vyhledejte účet úložiště, který jste nakonfigurovali výše a najít `WADPerformanceCountersTable` tabulky v ní. Měli byste vidět čítače SignalR v datech tabulky. Pokud nevidíte v tabulce, musíte znovu zadat přihlašovací údaje Azure Storage. Je nutné vybrat **aktualizovat** tlačítko najdete v tabulce v **Průzkumník cloudu** nebo vyberte **aktualizovat** tlačítka v okně Otevřít tabulku se zobrazí data v tabulce.

Zvláštní poděkování [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) pro původní obsah použitý v tomto kurzu.
