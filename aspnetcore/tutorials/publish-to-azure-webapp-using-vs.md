---
title: "Publikování aplikace ASP.NET Core do Azure pomocí sady Visual Studio"
author: rick-anderson
description: 
keywords: "Jádro ASP.NET"
ms.author: riande
manager: wpickett
ms.date: 10/05/2017
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: df22852d2daddb2a3faef8404d0d250a6a1697a5
ms.sourcegitcommit: e987c950caae7af9c4ece8a82228caa364e0a5df
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/05/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a>Publikování webové aplikace ASP.NET Core Azure App Service pomocí sady Visual Studio

Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), a [Rachel Appel](https://twitter.com/rachelappel)

## <a name="set-up"></a>Nastavení

* Otevřete [bezplatný účet Azure](https://aka.ms/K5y5yh) Pokud nemáte jeden. 

## <a name="create-a-web-app"></a>Vytvoření webové aplikace

Ve Visual Studio – úvodní stránka, vyberte **soubor > Nový > projekt...**

![nabídka Soubor](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

Dokončení **nový projekt** dialogové okno:

* V levém podokně vyberte **.NET Core**.

* V prostředním podokně vyberte **webové aplikace ASP.NET Core**.

* Vyberte **OK**.

![Dialogové okno Nový projekt](publish-to-azure-webapp-using-vs/_static/new_prj.png)

V **nové webové aplikace ASP.NET Core** dialogové okno:

* Vyberte **webové aplikace**.

* Vyberte **změnit ověřování**.

![Dialogové okno Nový projekt](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

**Změna ověřování** otevře se dialogové okno. 

* Vyberte **jednotlivých uživatelských účtů**.

* Vyberte **OK** se vrátíte do **nové webové aplikace ASP.NET Core**, pak vyberte **OK** znovu.

![Dialogové okno Nový ASP.NET Web základní ověřování](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

Visual Studio vytvoří řešení.

## <a name="run-the-app-locally"></a>Místní spuštění aplikace

* Zvolte **ladění** pak **spustit bez ladění** a spusťte aplikaci místně.

* Klikněte na tlačítko **o** a **kontaktujte** odkazy na ověření webové aplikace funguje.

![Webové aplikace otevřete v Microsoft Edge na místním hostiteli](publish-to-azure-webapp-using-vs/_static/show.png)

* Vyberte **zaregistrovat** a registraci nového uživatele. Můžete použít fiktivní e-mailovou adresu. Při odesílání, na stránce se zobrazí chybová zpráva:

    *"Vnitřní chyba serveru: Databázová operace se nezdařila při zpracování požadavku. Výjimky SQL: databázi nelze otevřít. Použití existující migrace pro kontext databáze aplikace může tento problém vyřešit."*

* Vyberte **použít migrace** a jakmile se aktualizace stránky, aktualizujte stránku.

![Vnitřní chyba serveru: Databázová operace se nezdařila při zpracování požadavku. Výjimky SQL: databázi nelze otevřít. Použití existující migrace pro kontext databáze aplikace může tento problém vyřešit.](publish-to-azure-webapp-using-vs/_static/mig.png)

Aplikace zobrazí e-mail použitý k registraci nového uživatele a **odhlášení** odkaz.

![Webové aplikace, otevřete v Microsoft Edge. Odkaz registrace je nahrazena textu Hello email@domain.com!](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a>Nasazení aplikace do Azure

Zavřete webové stránky, vraťte se na Visual Studio a vyberte **Zastavte ladění** z **ladění** nabídky.

Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **publikování...** .

![Kontextové nabídky otevřete se zvýrazněnou odkaz publikování](publish-to-azure-webapp-using-vs/_static/pub.png)

V **publikovat** dialogovém okně, vyberte **Microsoft Azure App Service** a klikněte na tlačítko **publikovat**.

![Dialogové okno publikování](publish-to-azure-webapp-using-vs/_static/maas1.png)

* Název aplikace jedinečný název. 

* Vyberte předplatné.

* Vyberte **nové...**  pro prostředek skupiny a zadejte název pro novou skupinu prostředků.

* Vyberte **nové...**  pro plán služby app service a vyberte umístění okolo vás. Můžete ponechat název, který je generován ve výchozím nastavení.

![Dialogovém okně App Service](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* Vyberte **služby** a vytvořit novou databázi.

* Vyberte zeleným  **+**  vytvořte novou databázi SQL.

![Novou databázi SQL.](publish-to-azure-webapp-using-vs/_static/sql.png)

* Vyberte **nové...**  na **nakonfigurovat databázi SQL** dialogovém okně můžete vytvořit novou databázi.

![Nové databáze SQL a serveru](publish-to-azure-webapp-using-vs/_static/conf.png)

**Nakonfigurujte systém SQL Server** otevře se dialogové okno.

* Zadejte uživatelské jméno správce a heslo a potom vyberte **OK**. Nezapomeňte, uživatelské jméno a heslo, které vytvoříte v tomto kroku. Můžete ponechat výchozí **název serveru**. 

* Zadejte názvy databáze a připojovací řetězec.

> [!NOTE]
> "admin" není povolen jako uživatelské jméno správce.

![Konfigurace systému SQL Server dialogové okno](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* Vyberte **OK**.

Visual Studio vrátí **vytvořit službu App Service** dialogové okno.

* Vyberte **vytvořit** na **vytvořit službu App Service** dialogové okno.

![Konfigurace dialogové okno databáze SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* Klikněte **nastavení** na odkaz v **publikovat** dialogové okno.

![Dialogové okno publikování: připojení panely](publish-to-azure-webapp-using-vs/_static/pubc.png)

Na **nastavení** stránky **publikovat** dialogové okno:

  * Rozbalte položku **databáze** a zkontrolujte **použít tento připojovací řetězec za běhu**.

  * Rozbalte položku **Entity Framework migrace** a zkontrolujte **použít publikování této migrace na**.

* Vyberte **Uložit**. Visual Studio vrátí **publikovat** dialogové okno. 

![Dialogové okno publikování: panel nastavení](publish-to-azure-webapp-using-vs/_static/pubs.png)

Klikněte na tlačítko **publikování**. Visual Studio bude publikování aplikace do Azure a spouštět cloudové aplikace v prohlížeči.

### <a name="test-your-app-in-azure"></a>Testování aplikace v Azure

* Testovací **o** a **kontaktujte** odkazy

* Registrace nového uživatele

![Webová aplikace otevřít v Microsoft Edge ve službě Azure App Service](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a>Aktualizace aplikace

* Upravit *Pages/About.cshtml* Razor stránky a změňte jeho obsah. Například můžete upravit odstavce. Tím vyjádříte "Hello ASP.NET Core!":

    [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]

* Klikněte pravým tlačítkem na projekt a vyberte **publikování...**  znovu.

![Kontextové nabídky otevřete se zvýrazněnou odkaz publikování](publish-to-azure-webapp-using-vs/_static/pub.png)

* Po publikování aplikace, ověřte, zda jsou k dispozici v Azure provedené změny.

![Ověřte, že úkol je dokončen](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a>Vyčištění

Po dokončení testování aplikace, přejděte na [portál Azure](https://portal.azure.com/) a odstraňte aplikaci.

* Vyberte **skupiny prostředků**, pak vyberte skupinu prostředků, kterou jste vytvořili.

![Azure Portal: Skupiny prostředků v nabídce bočním panelu](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* V **skupiny prostředků** vyberte **odstranit**.

![Portálu Azure: Skupiny prostředků stránky](publish-to-azure-webapp-using-vs/_static/rgd.png)

* Zadejte název skupiny prostředků a vyberte **odstranit**. Vaše aplikace a všechny další prostředky, které jsou vytvořené v tomto kurzu se teď odstraní z Azure.

### <a name="next-steps"></a>Další kroky

* [Průběžné nasazování do Azure pomocí sady Visual Studio a Git](../publishing/azure-continuous-deployment.md)