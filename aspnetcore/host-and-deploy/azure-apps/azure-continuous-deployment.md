---
title: Průběžné nasazování do Azure pomocí sady Visual Studio a Git s ASP.NET Core
author: rick-anderson
description: Naučte se vytvářet webové aplikace ASP.NET Core pomocí sady Visual Studio a nasadíte ho do Azure App Service pro průběžné nasazování pomocí Git.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: 4de1893e8c1f7f2f4d9af7278a110067ea777c61
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30897888"
---
# <a name="continuous-deployment-to-azure-with-visual-studio-and-git-with-aspnet-core"></a>Průběžné nasazování do Azure pomocí sady Visual Studio a Git s ASP.NET Core

Podle [Erik Reitan](https://github.com/Erikre)

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

Tento kurz ukazuje, jak webová aplikace ASP.NET Core pomocí sady Visual Studio vytvořte a nasaďte ho ze sady Visual Studio do služby Azure App Service pomocí průběžné nasazování.

Viz také [služby VSTS použít při vytváření a publikování do webové aplikace Azure s průběžné nasazování](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), který ukazuje, jak nakonfigurovat nastavené průběžné doručování (CD) pracovního postupu pro [Azure App Service](/azure/app-service/app-service-web-overview) pomocí Visual Studio Team Služby. Průběžné doručování Azure v Team Services zjednodušuje nastavení robustní nasazení kanálu publikování aktualizací pro aplikace, které jsou hostované v Azure App Service. Kanál lze nakonfigurovat z portálu Azure k vytvoření, spuštění testů, nasazení na přípravný slot a pak nasadit do produkčního prostředí.

> [!NOTE]
> K dokončení tohoto kurzu, je nutné účet Microsoft Azure. Získat účet, [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) nebo [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Požadavky

V tomto kurzu se předpokládá, že je nainstalován následující software:

* [Visual Studio](https://www.visualstudio.com)
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* [Git](https://git-scm.com/downloads) pro Windows

## <a name="create-an-aspnet-core-web-app"></a>Vytvoření webové aplikace ASP.NET Core

1. Spuštění sady Visual Studio.

1. Z **soubor** nabídce vyberte možnost **nový** > **projektu**.

1. Vyberte **webové aplikace ASP.NET Core** šablona projektu. Zobrazí se pod **nainstalovaná** > **šablony** > **Visual C#** > **.NET Core**. Název projektu `SampleWebAppDemo`. Vyberte **vytvoření nového úložiště Git** možnost a klikněte na tlačítko **OK**.

   ![Dialogové okno Nový projekt](azure-continuous-deployment/_static/01-new-project.png)

1. V **nový projekt ASP.NET Core** dialogovém okně, vyberte ASP.NET Core **prázdný** šablony, pak klikněte na tlačítko **OK**.

   ![Dialogové okno Nový projekt ASP.NET](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> Nejnovější verzi .NET Core je 2.0.

### <a name="running-the-web-app-locally"></a>Místní spuštění webové aplikace

1. Po dokončení vytváření aplikace Visual Studio spusťte aplikaci tak, že vyberete **ladění** > **spustit ladění**. Jako alternativu, stiskněte klávesu **F5**.

   To může trvat času k chybě při inicializaci sady Visual Studio a nové aplikace. Po jeho dokončení se zobrazí v prohlížeči spuštěné aplikaci.

   ![Zobrazení okna prohlížeče spuštění aplikace, která zobrazuje "Hello, World!"](azure-continuous-deployment/_static/04-browser-runapp.png)

1. Po zkontrolování funkční webovou aplikaci, zavřete prohlížeč a vyberte ikonu "Zastavte ladění" na panelu nástrojů sady Visual Studio k zastavení aplikace.

## <a name="create-a-web-app-in-the-azure-portal"></a>Vytvořit webovou aplikaci na portálu Azure

Následující postup vytvoření webové aplikace na portálu Azure:

1. Přihlaste se k [portál Azure](https://portal.azure.com).

1. Vyberte **nový** v levém horním rohu rozhraní portálu.

1. Vyberte **Web + mobilní** > **webová aplikace**.

   ![Portál Microsoft Azure: Tlačítko Nový: Web + mobilní v Marketplace: tlačítko webové aplikace v rámci vybrané aplikace](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. V **webové aplikace** okno, zadejte jedinečnou hodnotu **název služby App Service**.

   ![Okně webové aplikace](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > **Název služby App Service** název musí být jedinečný. Na portálu vynucuje toto pravidlo, pokud je zadaný název. Pokud poskytnete jinou hodnotu, nahraďte tuto hodnotu pro každý výskyt **SampleWebAppDemo** v tomto kurzu.

   Také v **webové aplikace** okně Vybrat existující **umístění plán služby App** nebo vytvořte novou. Pokud vytváříte nový plán, vyberte cenovou úroveň, umístění a další možnosti. Další informace o plánech služby App Service naleznete v tématu [podrobný přehled plánů služby Azure App Service](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).

1. Vyberte **vytvořit**. Azure bude zřídit a spustí webovou aplikaci.

   ![Portálu Azure: Okno Essentials ukázkové webové aplikaci ukázku 01](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a>Povolení publikování Git pro nové webové aplikace

Git je systém správy distribuovaných verzí, který slouží k nasazení webové aplikace služby Azure App Service. Kódu webové aplikace je uložen v místní úložiště Git a kód je nasadit do Azure nuceným doručením do vzdáleného úložiště.

1. Přihlaste se [portál Azure](https://portal.azure.com).

1. Vyberte **App Services** zobrazení seznamu služeb aplikace přidružené k předplatnému Azure.

1. Vyberte webovou aplikaci vytvořili v předchozí části tohoto kurzu.

1. V **nasazení** vyberte **možnosti nasazení** > **zvolit zdroj** > **místní úložiště Git**.

   ![Okno nastavení: okno zdroj nasazení: Zvolte zdroj okno](azure-continuous-deployment/_static/deployment-options.png)

1. Vyberte **OK**.

1. Pokud přihlašovací údaje nasazení pro publikování webové aplikace nebo jiná aplikace služby App Service nebyly dříve nastaveny, nastavte je nyní:

   * Vyberte **nastavení** > **přihlašovací údaje nasazení**. **Nastavit přihlašovací údaje nasazení** zobrazí se okno.
   * Vytvořte uživatelské jméno a heslo. Uložte heslo pro pozdější použití při nastavování Git.
   * Vyberte **Uložit**.

1. V **webové aplikace** vyberte **nastavení** > **vlastnosti**. Adresu URL vzdáleného úložiště Git k nasazení se zobrazí v části **adresy URL pro GIT**.

1. Kopírování **adresy URL pro GIT** hodnoty pro pozdější použití v tomto kurzu.

   ![Portálu Azure: okno vlastností aplikace](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a>Publikování webové aplikace do služby Azure App Service

V této části vytvořte místní úložiště Git pomocí sady Visual Studio a posílejte nabízená oznámení z tohoto úložiště do Azure k nasazení webové aplikace. Kroky zahrnují následující:

* Přidejte nastavení vzdáleného úložiště pomocí adresy URL pro GIT hodnoty, aby místní úložiště můžete nasadit do Azure.
* Potvrďte změny projektu.
* Doručte změny projektu z místního úložiště do vzdáleného úložiště v Azure.

1. V **Průzkumníku řešení** klikněte pravým tlačítkem na **řešení 'SampleWebAppDemo'** a vyberte **potvrdit**. **Team Explorer** se zobrazí.

   ![Karta Team Explorer se připojit](azure-continuous-deployment/_static/10-team-explorer.png)

1. V **Team Explorer**, vyberte **domácí** (Ikona Domovská stránka) > **nastavení** > **nastavení úložiště**.

1. V **dálkové ovladače** části **nastavení úložiště**, vyberte **přidat**. **Přidat vzdálené** se zobrazí dialogové okno.

1. Nastavte **název** vzdáleného k **Azure SampleApp**.

1. Nastavit hodnotu pro **načíst** k **adresy URL pro Git** , zkopírovat z Azure dříve v tomto kurzu. Všimněte si, že toto je adresa URL, který končí **.git**.

   ![Vzdálené dialogové okno Upravit](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > Jako alternativu, zadejte vzdálené úložiště, ze kterého **příkazové okno** otevřením **příkazové okno**, změna k adresáři projektu a zadáte příkaz. Příklad:
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. Vyberte **domácí** (Ikona Domovská stránka) > **nastavení** > **globální nastavení**. Zkontrolujte, jestli jsou nastavené jméno a e-mailovou adresu. Vyberte **aktualizace** v případě potřeby.

1. Vyberte **Domů** > **změny** se vrátíte do **změny** zobrazení.

1. Zadejte zprávu o potvrzení, jako například **počáteční Push č. 1** a vyberte **potvrzení**. Tato akce vytvoří *potvrzení* místně.

   ![Karta Team Explorer se připojit](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > Jako alternativu, potvrzení změny z **příkazové okno** otevřením **příkazové okno**, změna k adresáři projektu a zadáte příkazy gitu. Příklad:
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. Vyberte **Domů** > **synchronizace** > **akce** > **otevřete příkazový řádek**. Do příkazového řádku se otevře adresáři projektu.

1. Do příkazového řádku zadejte následující příkaz:

   `git push -u Azure-SampleApp master`

1. Zadejte Azure **přihlašovací údaje nasazení** heslo v Azure vytvořili.

   Tento příkaz spustí proces vkládání souborů místní projektu do Azure. Výstup z výše uvedeném příkazu končí zprávu, že nasazení bylo úspěšné.

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > Pokud spolupráce na projekt se požaduje, zvažte, když zavedete [Githubu](https://github.com) před odesláním do Azure.
 
### <a name="verify-the-active-deployment"></a>Ověření aktivní nasazení

Ověřte, jestli přenos webové aplikace z místního prostředí do Azure se úspěšně.

V [portálu Azure](https://portal.azure.com), vyberte webovou aplikaci. Vyberte **nasazení** > **možnosti nasazení**.

![Portálu Azure: Okno nastavení: nasazení okna zobrazující úspěšné nasazení](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a>Spusťte aplikaci v Azure

Teď, když webové aplikace je nasazená do Azure, spusťte aplikaci.

Můžete to provést dvěma způsoby:

* Na portálu Azure vyhledejte okně webové aplikace pro webovou aplikaci. Vyberte **Procházet** Chcete-li zobrazit aplikaci ve výchozím prohlížeči.
* Otevřete prohlížeč a zadejte adresu URL pro webovou aplikaci. Příklad: `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-the-web-app-and-republish"></a>Aktualizovat webovou aplikaci a znovu publikovat

Po provedení změn pomocí místního kódu, znovu publikujte:

1. V **Průzkumníku řešení** sady Visual Studio, otevřete *Startup.cs* souboru.

1. V `Configure` metoda, upravte `Response.WriteAsync` metoda tak to vypadat takto:

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. Uložit změny do *Startup.cs*.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **řešení 'SampleWebAppDemo'** a vyberte **potvrdit**. **Team Explorer** se zobrazí.

1. Zadejte zprávu o potvrzení, jako například `Update #2`.

1. Stiskněte **potvrzení** tlačítko potvrzení změn projektu.

1. Vyberte **Domů** > **synchronizace** > **akce** > **Push**.

> [!NOTE]
> Jako alternativu, odešlete změny z **příkazové okno** otevřením **příkazové okno**, změna k adresáři projektu a zadáním příkazu git. Příklad:
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a>Zobrazení aktualizované webové aplikace v Azure

Zobrazení aktualizované webové aplikace tak, že vyberete **Procházet** v okně webové aplikace na portálu Azure nebo otevřením prohlížeče a zadávat adresu URL pro webovou aplikaci. Příklad: `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a>Další zdroje

* [Použití služby VSTS vytvářet a publikovat do webové aplikace Azure s průběžné nasazování.](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic)
* [Kudu projektu](https://github.com/projectkudu/kudu/wiki)
