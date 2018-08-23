---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: Použití čítačů výkonu SignalR webové role Azure | Dokumentace Microsoftu
author: guardrex
description: Postup instalace a použití čítačů výkonu SignalR webové role Azure.
keywords: Čítač ASP.NET,signalr,Performance, webové role azure
ms.author: riande
ms.date: 02/11/2017
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: acc9836535466a801f1f46ec18d05937e2e42af2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757125"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a>Použití čítačů výkonu SignalR webové role Azure

Podle [Luke Latham](https://github.com/guardrex)

Čítače výkonu SignalR slouží k monitorování výkonu vaší aplikace do webové Role Azure. Čítače jsou zachyceny na základě Microsoft Azure Diagnostics. Instalace čítačů výkonu SignalR v Azure s *signalr.exe*, nástroje, který používá pro samostatnou službu nebo místní aplikace. Protože role služby Azure jsou přechodné, konfigurace aplikace pro instalaci a registraci čítačů výkonu SignalR při spuštění.

## <a name="prerequisites"></a>Požadavky

* [Visual Studio 2015](https://www.visualstudio.com/vs/visual-studio-express/)
* [Microsoft Azure SDK pro Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **Poznámka: restartujte počítač po instalaci sady SDK.**
* Předplatné Microsoft Azure: si zaregistrovat Bezplatný zkušební účet Azure, najdete v článku [bezplatná zkušební verze Azure](https://azure.microsoft.com/free/).

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a>Vytvoření webové Role Azure aplikace, která zveřejňuje čítačů výkonu SignalR

1. Otevřít Visual Studio 2015.

2. V sadě Visual Studio 2015, vyberte **souboru** > **nový** > **projektu**.

3. V **šablony** podokně **nový projekt** okně v části **Visual C#** uzlu, vyberte **cloudu** uzel a vyberte **Azure Cloud Service** šablony. Pojmenujte aplikaci **SignalRPerfCounters** a vyberte **OK**.

   ![Nové cloudové aplikace](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)
    
4. V **nová Cloudová služba Microsoft Azure** dialogového okna, vyberte **webovou roli ASP.NET** a vyberte > a přidejte do projektu role. Vyberte **OK**.

   ![Přidat webová Role ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)
    
5. V **nová webová aplikace ASP.NET - WebRole1** dialogového okna, vyberte **MVC** šablonu a pak vyberte **OK**.

   ![Přidat MVC a webového rozhraní API](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)
    
6. V **Průzkumníka řešení**, otevřete *diagnostics.wadcfgx* soubor **WebRole1**.

   ![Diagnostics.wadcfgx Průzkumníka řešení](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)
    
7. Nahraďte obsah souboru s následující konfigurací a soubor uložte:

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]
    
8. Otevřít **Konzola správce balíčků** z **nástroje** > **Správce balíčků NuGet**. Zadejte následující příkazy, abyste nainstalovali nejnovější verzi SignalR a balíček nástroje SignalR:

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]
    
9. Konfigurace aplikace k instalaci čítačů výkonu SignalR do role instance při spuštění nebo recykluje. V **Průzkumníka řešení**, klikněte pravým tlačítkem na **WebRole1** projektu a vyberte **přidat** > **novou složku**. Název nové složky *spuštění*.

   ![Přidat složka po spuštění](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)
    
10. Kopírovat *signalr.exe* souboru (přidá s **Microsoft.AspNet.SignalR.Utils** balíček) z \<složky projektu > / SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\< verze > / nástrojů k *spuštění* složku, kterou jste vytvořili v předchozím kroku.

11. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *spuštění* a pak zvolte položku **přidat** > **existující položku**. V zobrazeném dialogu vyberte *signalr.exe* a vyberte **přidat**.

    ![Přidat signalr.exe do projektu](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)
    
12. Klikněte pravým tlačítkem na *spuštění* složky, které jste vytvořili. Vyberte **přidat** > **nová položka**. Vyberte **Obecné** uzlu, vyberte **textový soubor**a pojmenujte novou položku *SignalRPerfCounterInstall.cmd*. Tento soubor příkaz nainstaluje čítačů výkonu SignalR do webové role.

    ![Vytvořit soubor batch instalace čítače výkonu SignalR](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)
     
13. Když Visual Studio vytvoří *SignalRPerfCounterInstall.cmd* soubor, automaticky otevře v hlavním okně. Nahraďte obsah souboru následující skript, pak uložte a zavřete soubor. Tento skript spustí *signalr.exe*, který přidá čítačů výkonu SignalR k instanci role.

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]
    
14. Vyberte *signalr.exe* ve **Průzkumníka řešení**. V souboru **vlastnosti**, nastavte **kopírovat do výstupního adresáře** k **vždy Kopírovat**.

    ![Nastavte kopírovat do výstupního adresáře vždy kopírovat](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)
    
15. Opakujte předchozí krok u *SignalRPerfCounterInstall.cmd* souboru.

    
16. Klikněte pravým tlačítkem na *SignalRPerfCounterInstall.cmd* a vyberte možnost **otevřít v**. V zobrazeném dialogu vyberte **binární Editor** a vyberte **OK**.

    ![Otevřít pomocí editoru binárního souboru](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)
    
17. V binárním editoru vyberte všechny úvodní bajty v souboru a odstraňte je. Soubor uložte a zavřete.

    ![Odstranit úvodní bajty](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)
    
18. Otevřít *ServiceDefinition.csdef* a přidání úlohy po spuštění, který se spustí *SignalrPerfCounterInstall.cmd* souboru při spuštění služby:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]
    
19. Otevřít `Views/Shared/_Layout.cshtml` a odebrat skript sady jQuery od konce souboru.

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]
    
20. Přidejte javascriptový klient, který nepřetržitě volá `increment` metoda na serveru. Otevřít `Views/Home/Index.cshtml` a nahraďte jeho obsah následujícím kódem:

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]
    
21. Vytvořte novou složku v **WebRole1** projekt s názvem *rozbočovače*. Klikněte pravým tlačítkem na *rozbočovače* složky **Průzkumníku řešení**vyberte **webové** > **SignalR**a vyberte  **Třída rozbočovače SignalR (v2)**. Název nového centra *MyHub.cs* a vyberte **přidat**.

    ![Přidání třídy rozbočovače SignalR do rozbočovače složky v dialogovém okně Přidat novou položku](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. *MyHub.cs* se automaticky otevře v hlavním okně. Nahraďte obsah následujícím kódem, pak uložte a zavřete soubor:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]
    
23. *[Crank.exe](signalr-connection-density-testing-with-crank.md)*  je testování nástroj, opatřeného SignalR codebase hustoty připojení. Protože Crank vyžaduje trvalé připojení, přidáte jej do vaší lokality pro použití při testování. Přidat novou složku **WebRole1** projekt s názvem *PersistentConnections*. Klikněte pravým tlačítkem na tuto složku a vyberte **přidat** > **třídy**. Pojmenujte nový soubor třídy *MyPersistentConnections.cs* a vyberte **přidat**.

24. Visual Studio otevře *MyPersistentConnections.cs* soubor v hlavním okně. Nahraďte obsah následujícím kódem, pak uložte a zavřete soubor:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]
    
25. Použití `Startup` třídu, objekty SignalR spustit při spuštění OWIN. Otevřete nebo vytvořte *Startup.cs* a nahraďte jeho obsah následujícím kódem:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]
    
    Ve výše uvedeném kódu `OwinStartup` atribut určí tuto třídu pro spuštění OWIN. `Configuration` Metoda začíná SignalR.
    
26. Otestujte aplikaci stisknutím klávesy v Microsoft Azure Emulator **F5**.

    > [!NOTE]
    > Pokud narazíte **FileLoadException –** na **MapSignalR**, změnit přesměrování vazby v *web.config* takto:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]
    
27. Počkejte přibližně jednu minutu. Otevřete panel nástrojů Průzkumník cloudu v sadě Visual Studio (**zobrazení** > **Průzkumníka cloudu**) a rozbalte cestu `(Local)/Storage Accounts/(Development)/Tables`. Dvakrát klikněte na panel **WADPerformanceCountersTable**. Měli byste vidět čítače SignalR v datech tabulky. Pokud se nezobrazí v tabulce, budete muset znovu zadat svoje přihlašovací údaje služby Azure Storage. Budete muset vybrat **aktualizovat** tlačítko naleznete v tabulce v **Průzkumníka cloudu** nebo vyberte **aktualizovat** tlačítka v okně Otevřít tabulku k zobrazení dat v tabulce.

    ![Výběr tabulky WAD čítače výkonu v Průzkumníku cloudu sady Visual Studio](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Zobrazuje čítače shromažďují v tabulce čítače výkonu využitím WAD](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)
    
28. Chcete-li testovat aplikace v cloudu, aktualizujte **ServiceConfiguration.Cloud.cscfg** souboru a nastavit `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` na platný připojovací řetězec účtu úložiště Azure.

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. Nasaďte aplikaci do vašeho předplatného Azure. Podrobnosti o tom, jak nasadit aplikaci do Azure najdete v tématu [jak vytvořit a nasadit Cloudovou službu](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).

30. Počkejte několik minut. V **Průzkumníka cloudu**, vyhledejte účet úložiště, který je nakonfigurován výše a najít `WADPerformanceCountersTable` tabulky v ní. Měli byste vidět čítače SignalR v datech tabulky. Pokud se nezobrazí v tabulce, budete muset znovu zadat svoje přihlašovací údaje služby Azure Storage. Budete muset vybrat **aktualizovat** tlačítko naleznete v tabulce v **Průzkumníka cloudu** nebo vyberte **aktualizovat** tlačítka v okně Otevřít tabulku k zobrazení dat v tabulce.

Speciální k [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) pro původní obsah použitý v tomto kurzu.
