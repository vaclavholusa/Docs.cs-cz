---
title: "Průběžné nasazování do Azure pomocí sady Visual Studio a Git"
author: rick-anderson
description: "Naučte se vytvářet webové aplikace ASP.NET Core pomocí sady Visual Studio a nasadíte ho do služby Azure App Service pro průběžné nasazování pomocí Git."
keywords: "Jádro ASP.NET"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 2707c7a8-2350-4304-9856-fda58e5c0a16
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/azure-continuous-deployment
ms.openlocfilehash: a9efad38b1c75bd3a186b4ec85861357ecf744b9
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/12/2017
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a>Průběžné nasazování do Azure pro ASP.NET Core, sadou Visual Studio a Git

Podle [Erik Reitan](https://github.com/Erikre)

Tento kurz ukazuje, jak chcete vytvořit webové aplikace ASP.NET Core pomocí sady Visual Studio a nasaďte ho ze sady Visual Studio do služby Azure App Service pomocí průběžné nasazování. 

Viz také [služby VSTS použít při vytváření a publikování do webové aplikace Azure s průběžné nasazování](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic), který ukazuje, jak nakonfigurovat nastavené průběžné doručování (CD) pracovního postupu pro [Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) pomocí Visual Studio Team Služby. Průběžné doručování Azure v Team Services zjednodušuje nastavení robustní nasazení kanálu publikování aktualizací pro vaši aplikaci do služby Azure App Service. Kanál lze nakonfigurovat z portálu Azure k vytvoření, spuštění testů, nasazení na přípravný slot a pak nasadit do produkčního prostředí.

> [!NOTE]
> K dokončení tohoto kurzu potřebujete účet Microsoft Azure. Pokud účet nemáte, můžete [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) nebo [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Požadavky

V tomto kurzu se předpokládá, že jste již nainstalovali následující:

* [Visual Studio](https://www.visualstudio.com)

* [ASP.NET Core](https://download.microsoft.com/download/F/6/E/F6ECBBCC-B02F-424E-8E03-D47E9FA631B7/DotNetCore.1.0.1-VS2015Tools.Preview2.0.3.exe) (runtime a nástrojů)

* [Git](https://git-scm.com/downloads) pro Windows

## <a name="create-an-aspnet-core-web-app"></a>Vytvoření webové aplikace ASP.NET Core

1. Spuštění sady Visual Studio.

2. Z **soubor** nabídce vyberte možnost **nový** > **projektu**.

3. Vyberte **webové aplikace ASP.NET** šablona projektu. Zobrazí se pod **nainstalovaná** > **šablony** > **Visual C#** > **webové**. Název projektu `SampleWebAppDemo`. Vyberte **vytvoření nového úložiště Git** možnost a klikněte na tlačítko **OK**.

   ![Dialogové okno Nový projekt](azure-continuous-deployment/_static/01-new-project.png)

4. V **nový projekt ASP.NET** dialogovém okně, vyberte ASP.NET Core **prázdný** šablony, pak klikněte na tlačítko **OK**.

   ![Dialogové okno Nový projekt ASP.NET](azure-continuous-deployment/_static/02-web-site-template.png)


### <a name="running-the-web-app-locally"></a>Místní spuštění webové aplikace

1. Po dokončení vytváření aplikace Visual Studio spusťte aplikaci tak, že vyberete **ladění** -> **spustit ladění**. Jako alternativu, můžete stisknout **F5**.

   To může trvat času k chybě při inicializaci sady Visual Studio a nové aplikace. Po dokončení se zobrazí v prohlížeči spuštěné aplikaci.

   ![Zobrazení okna prohlížeče spuštění aplikace, která zobrazuje "Hello, World!"](azure-continuous-deployment/_static/04-browser-runapp.png)

2. Po zkontrolování funkční webovou aplikaci, zavřete prohlížeč a klikněte na ikonu "Zastavte ladění" na panelu nástrojů sady Visual Studio k zastavení aplikace.

## <a name="create-a-web-app-in-the-azure-portal"></a>Vytvořit webovou aplikaci na portálu Azure

Následující postup vás provede vytvořením webové aplikace na portálu Azure.

1. Přihlaste se k [portálu Azure](https://portal.azure.com)
2. Klepněte na **nový** v horní pravé portálu
3. Klepněte na **Web + mobilní** > **webové aplikace**

    ![Portál Microsoft Azure: Tlačítko Nový: Web + mobilní v Marketplace: tlačítko webové aplikace v rámci vybrané aplikace](azure-continuous-deployment/_static/05-azure-newwebapp.png)

4.  V **webové aplikace** okno, zadejte jedinečnou hodnotu **název služby App Service**.

    ![Okně webové aplikace](azure-continuous-deployment/_static/06-azure-newappblade.png)

    >[!NOTE]
    >**Název služby App Service** název musí být jedinečný. Při pokusu o zadání názvu, vynutí portálu toto pravidlo. Po zadání jinou hodnotu, budete muset nahraďte tuto hodnotu pro každý výskyt **SampleWebAppDemo** , které vidíte v tomto kurzu.

    &nbsp;
    
    Také v **webové aplikace** okně Vybrat existující **umístění plán služby App** nebo vytvořte novou. Pokud vytvoříte nový plán, vyberte cenovou úroveň, umístění a další možnosti. Další informace o plánech služby App Service [podrobný přehled plánů služby Azure App Service](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).

5.  Klikněte na tlačítko **vytvořit**. Azure bude zřídit a spuštění webové aplikace.

    ![Portálu Azure: Okno Essentials ukázkové webové aplikaci ukázku 01](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a>Povolení publikování Git pro nové webové aplikace

Git je systém správy distribuovaných verzí, který můžete použít k nasazení webové aplikace služby Azure App Service. Budete ukládat kód napsaný pro webovou aplikaci v místní úložiště Git a kód budete nasazovat do Azure nuceným doručením do vzdáleného úložiště.

1. Přihlaste se [portálu Azure](https://portal.azure.com), pokud jste již přihlášeni.

2. Klikněte na tlačítko **Procházet**, umístěné v dolní části navigačního podokna.

3. Klikněte na tlačítko **webové aplikace** zobrazíte seznam webové aplikace přidružené k předplatnému Azure.

4. Vyberte webovou aplikaci, kterou jste vytvořili v předchozí části tohoto kurzu.

5. Pokud **nastavení** okno není zobrazený, vyberte **nastavení** v **webové aplikace** okno.

6. V **nastavení** vyberte **zdroj nasazení** > **zvolit zdroj** > **místní úložiště Git**.

   ![Okno nastavení: okno zdroj nasazení: Zvolte zdroj okno](azure-continuous-deployment/_static/08-azure-localrepository.png)

7. Click **OK**.

8. Pokud jste dříve vytvořili přihlašovací údaje nasazení pro publikování webové aplikace nebo jiná aplikace služby App Service, nastavte je nyní:

   * Klikněte na tlačítko **nastavení** > **přihlašovací údaje nasazení**. **Nastavit přihlašovací údaje nasazení** zobrazí se okno.

   * Vytvořte uživatelské jméno a heslo.  Toto heslo budete potřebovat později při nastavování Git.

   * Klikněte na tlačítko **Uložit**.

9. V **webové aplikace** okně klikněte na tlačítko **nastavení** > **vlastnosti**. Vzdálené úložiště Git, které budete nasazovat na adresu URL se zobrazí v části **adresy URL pro GIT**.

10. Kopírování **adresy URL pro GIT** hodnoty pro pozdější použití v tomto kurzu.

   ![Portálu Azure: okno vlastností aplikace](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-your-web-app-to-azure-app-service"></a>Publikování webové aplikace do služby Azure App Service

V této části vytvoříte místní úložiště Git pomocí sady Visual Studio a posílejte nabízená oznámení z tohoto úložiště do Azure k nasazení vaší webové aplikace. Kroky zahrnují následující:

   * Přidejte nastavení vzdáleného úložiště pomocí adresy URL pro GIT hodnota, abyste mohli nasadit místního úložiště do Azure.

   * Potvrdíte změny projektu.

   * Odešlete své změny projektu z místního úložiště do vzdáleného úložiště v Azure.

&nbsp;
   
1.  V **Průzkumníku řešení** klikněte pravým tlačítkem na **řešení 'SampleWebAppDemo'** a vyberte **potvrdit**. **Team Explorer** se zobrazí.

    ![Karta Team Explorer se připojit](azure-continuous-deployment/_static/10-team-explorer.png)

2.  V **Team Explorer**, vyberte **domácí** (Ikona Domovská stránka) > **nastavení** > **nastavení úložiště**.

3.  V **dálkové ovladače** části **nastavení úložiště** vyberte **přidat**. **Přidat vzdálené** zobrazí se dialogové okno.

4.  Nastavte **název** vzdáleného k **Azure SampleApp**.

5.  Nastavit hodnotu pro **načíst** k **adresy URL pro Git** který jste zkopírovali z Azure dříve v tomto kurzu. Všimněte si, že toto je adresa URL, který končí **.git**.

    ![Vzdálené dialogové okno Upravit](azure-continuous-deployment/_static/11-add-remote.png)

    >[!NOTE]
    >Jako alternativu, můžete zadat vzdálené úložiště, ze kterého **příkazové okno** otevřením **příkazové okno**, změna do vašeho adresáře projektu a zadáte příkaz. Například:`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

6.  Vyberte **domácí** (Ikona Domovská stránka) > **nastavení** > **globální nastavení**. Ujistěte se, že máte své jméno a e-mailovou adresu, nastavte. Také můžete potřebovat k výběru **aktualizace**.

7.  Vyberte **Domů** > **změny** se vrátíte do **změny** zobrazení.

8.  Zadejte zprávu o potvrzení, jako například **počáteční Push č. 1** a klikněte na tlačítko **potvrzení**. Tato akce vytvoří *potvrzení* místně. Potom budete muset *synchronizace* s Azure.

    ![Karta Team Explorer se připojit](azure-continuous-deployment/_static/12-initial-commit.png)

    >[!NOTE]
    >Jako alternativu můžete potvrdit změny z **příkazové okno** otevřením **příkazové okno**, změna do vašeho adresáře projektu a zadáte příkazy gitu. Příklad:
    >
    >`git add .`
    >
    >`git commit -am "Initial Push #1"`

9.  Vyberte **Domů** > **synchronizace** > **akce** > **otevřete příkazový řádek**. Do příkazového řádku se otevře do vašeho adresáře projektu.

10.  Do příkazového řádku zadejte následující příkaz:

    `git push -u Azure-SampleApp master`

11.  Zadejte vaše Azure **přihlašovací údaje nasazení** heslo, které jste vytvořili dříve v Azure.

    >[!NOTE]
    >Heslo se nezobrazí, jak ho zadáte.

    Tento příkaz spustí proces nabízení soubory místního projektu do Azure. Výstup z výše uvedeném příkazu končí zprávu, že nasazení bylo úspěšné.
        
    ```
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
    * [new branch]      master -> master
    Branch master set up to track remote branch master from Azure-SampleApp.
    ```
    > [!NOTE]
    > Pokud budete potřebovat spolupracovat na projektu, měli byste zvážit nabízet [Githubu](https://github.com) v rozmezí vkládání do Azure.
 
### <a name="verify-the-active-deployment"></a>Ověření aktivní nasazení

Můžete ověřit, že jste úspěšně přenesla webové aplikace z vaší místní prostředí do Azure. Uvidíte uvedené úspěšné nasazení.

1. V [portálu Azure](https://portal.azure.com), vyberte webovou aplikaci. Pak vyberte **nastavení** > **průběžné nasazování**.

   ![Portálu Azure: Okno nastavení: nasazení okna zobrazující úspěšné nasazení](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a>Spusťte aplikaci v Azure

Teď, když jste nasadili vaší webové aplikace do Azure, můžete spustit aplikace.

Tento krok můžete provést dvěma způsoby:

* Na portálu Azure, vyhledejte okně webové aplikace pro vaši webovou aplikaci a klikněte na tlačítko **Procházet** Chcete-li zobrazit aplikaci ve výchozím prohlížeči.

* Otevřete prohlížeč a zadejte adresu URL pro webovou aplikaci. Příklad:

  `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-your-web-app-and-republish"></a>Aktualizovat webovou aplikaci a znovu publikovat

Po provedení změn do místní kódu můžete znovu publikovat.

1.  V **Průzkumníku řešení** sady Visual Studio, otevřete *Startup.cs* souboru.

2.  V `Configure` metoda, upravte `Response.WriteAsync` metoda tak to vypadat takto:

    ```aspx-cs
    await context.Response.WriteAsync("Hello World! Deploy to Azure.");
    ```
3.  Uložit změny do *Startup.cs*.

4.  V **Průzkumníku řešení**, klikněte pravým tlačítkem na **řešení 'SampleWebAppDemo'** a vyberte **potvrdit**. **Team Explorer** se zobrazí.

5.  Zadejte zprávu o potvrzení, jako například:

    ```none
    Update #2
    ```

6.  Stiskněte **potvrzení** tlačítko potvrzení změn projektu.

7.  Vyberte **Domů** > **synchronizace** > **akce** > **Push**.

>[!NOTE]
>Jako alternativu můžete odešlete své změny z **příkazové okno** otevřením **příkazové okno**, změna do vašeho adresáře projektu a zadáním příkazu git. Příklad:
>
>`git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a>Zobrazení aktualizované webové aplikace v Azure

Zobrazení aktualizované webové aplikace tak, že vyberete **Procházet** v okně webové aplikace na portálu Azure nebo otevřením prohlížeče a zadávat adresu URL pro webové aplikace. Příklad:

   `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a>Další prostředky

* [Publikování a nasazení](index.md)

* [Kudu projektu](https://github.com/projectkudu/kudu/wiki)
