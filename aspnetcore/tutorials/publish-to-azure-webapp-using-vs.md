---
title: "Publikování aplikace ASP.NET Core do Azure pomocí sady Visual Studio"
author: rick-anderson
description: "Zjistěte, jak publikovat aplikaci ASP.NET Core Azure App Service pomocí sady Visual Studio."
manager: wpickett
ms.author: riande
ms.date: 12/16/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: 0c2905575751c9880e02d8581642a1628bea5a49
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a>Publikování webové aplikace ASP.NET Core Azure App Service pomocí sady Visual Studio

Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), a [Rachel Appel](https://twitter.com/rachelappel)

[!INCLUDE[Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

V tématu [publikovat do Azure ze sady Visual Studio pro Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) při práci na macu.

## <a name="set-up"></a>Nastavení

* Otevřete [bezplatný účet Azure](https://aka.ms/K5y5yh) Pokud nemáte. 

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

## <a name="run-the-app"></a>Spuštění aplikace

* Stisknutím kombinace kláves CTRL + F5 a spusťte projekt.
* Testovací **o** a **kontaktujte** odkazy.

![Webové aplikace otevřete v Microsoft Edge na místním hostiteli](publish-to-azure-webapp-using-vs/_static/show.png)

### <a name="register-a-user"></a>Registrace uživatele

* Vyberte **zaregistrovat** a registraci nového uživatele. Můžete použít fiktivní e-mailovou adresu. Při odesílání, na stránce se zobrazí chybová zpráva:

    *"Vnitřní chyba serveru: Databázová operace se nezdařila při zpracování požadavku. Výjimky SQL: databázi nelze otevřít. Použití existující migrace pro kontext databáze aplikace může tento problém vyřešit."*
* Vyberte **použít migrace** a jakmile se aktualizace stránky, aktualizujte stránku.

![Vnitřní chyba serveru: Databázová operace se nezdařila při zpracování požadavku. Výjimky SQL: databázi nelze otevřít. Použití existující migrace pro kontext databáze aplikace může tento problém vyřešit.](publish-to-azure-webapp-using-vs/_static/mig.png)

Aplikace zobrazí e-mail použitý k registraci nového uživatele a **odhlášení** odkaz.

![Webové aplikace, otevřete v Microsoft Edge. Odkaz registrace je nahrazena textu Hello email@domain.com!](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a>Nasazení aplikace do Azure

Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **publikování...** .

![Kontextové nabídky otevřete se zvýrazněnou odkaz publikování](publish-to-azure-webapp-using-vs/_static/pub.png)

V **publikovat** dialogové okno:

* Vyberte **služby Microsoft Azure App Service**.
* Vyberte ikonu zařízení a pak vyberte **vytvořit profil**.
* Vyberte **vytvořit profil**.

![Dialogové okno publikování](publish-to-azure-webapp-using-vs/_static/maas1.png)

### <a name="create-azure-resources"></a>Vytváření prostředků Azure

**Vytvořit službu App Service** otevře se dialogové okno:

* Zadejte vaše předplatné.
* **Název aplikace**, **skupiny prostředků**, a **plán služby App Service** vstupní pole se vyplní. Můžete ponechat tyto názvy nebo změnit.

![Dialogovém okně App Service](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* Vyberte **služby** a vytvořit novou databázi.

* Vyberte zeleným  **+**  vytvořte novou databázi SQL.

![Novou databázi SQL.](publish-to-azure-webapp-using-vs/_static/sql.png)

* Vyberte **nové...**  na **nakonfigurovat databázi SQL** dialogovém okně můžete vytvořit novou databázi.

![Nové databáze SQL a serveru](publish-to-azure-webapp-using-vs/_static/conf.png)

**Nakonfigurujte systém SQL Server** otevře se dialogové okno.

* Zadejte uživatelské jméno správce a heslo a potom vyberte **OK**. Můžete ponechat výchozí **název serveru**. 

> [!NOTE]
> "admin" není povolen jako uživatelské jméno správce.

![Konfigurace systému SQL Server dialogové okno](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* Vyberte **OK**.

Visual Studio vrátí **vytvořit službu App Service** dialogové okno.

* Vyberte **vytvořit** na **vytvořit službu App Service** dialogové okno.

![Konfigurace dialogové okno databáze SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

Visual Studio vytvoří webovou aplikaci a SQL Server na platformě Azure. Tento krok může trvat několik minut. Informace o prostředky vytvořené v tématu [dalších prostředků](#additonal-resources).

Po dokončení nasazení, vyberte **nastavení**:

![Konfigurace systému SQL Server dialogové okno](publish-to-azure-webapp-using-vs/_static/set.png)

Na **nastavení** stránky **publikovat** dialogové okno:

  * Rozbalte položku **databáze** a zkontrolujte **použít tento připojovací řetězec za běhu**.
  * Rozbalte položku **Entity Framework migrace** a zkontrolujte **použít publikování této migrace na**.

* Vyberte **Uložit**. Visual Studio vrátí **publikovat** dialogové okno. 

![Dialogové okno publikování: panel nastavení](publish-to-azure-webapp-using-vs/_static/pubs.png)

Klikněte na tlačítko **publikování**. Visual Studio publishs vaší aplikace do Azure. Po dokončení nasazení aplikace se otevře v prohlížeči.

### <a name="test-your-app-in-azure"></a>Testování aplikace v Azure

* Testovací **o** a **kontaktujte** odkazy

* Registrace nového uživatele

![Webová aplikace otevřít v Microsoft Edge ve službě Azure App Service](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a>Aktualizace aplikace

* Upravit *Pages/About.cshtml* Razor stránky a změňte jeho obsah. Například můžete upravit odstavce. Tím vyjádříte "Hello ASP.NET Core!": [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]

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

* [Průběžné nasazování do Azure pomocí sady Visual Studio a Git](xref:host-and-deploy/azure-apps/azure-continuous-deployment)

## <a name="additonal-resources"></a>Dalších prostředků

* [Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-web-overview)
* [Skupiny prostředků Azure.](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups)
* [Databáze Azure SQL](https://docs.microsoft.com/azure/sql-database/)
