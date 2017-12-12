---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: "Přidávání obsahu do správy zdrojového kódu | Microsoft Docs"
author: jrjlee
description: "Toto téma vysvětluje, jak přidávat obsah do správy zdrojového kódu v Team Foundation Server (TFS) 2010. Popisuje postup přidání řešení a projekty do týmu proje..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: a6a90a03674cfe7565da0ed56148186ee9525707
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="adding-content-to-source-control"></a>Přidávání obsahu do správy zdrojového kódu
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma vysvětluje, jak přidávat obsah do správy zdrojového kódu v Team Foundation Server (TFS) 2010. Popisuje postup přidání řešení a projekty do týmového projektu v sadě TFS, a vysvětluje postup přidání externí závislosti, jako je rozhraní nebo sestavení do správy zdrojového kódu.


Toto téma je součástí ze série kurzů na základě kolem podnikové požadavky nasazení fiktivní společnost s názvem Fabrikam, Inc. Tento kurz series používá ukázkové řešení & #x 2014; [řešení obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; představující webovou aplikaci s úrovní realistické složitější, včetně aplikace ASP.NET MVC 3 systému Windows Komunikační služby Foundation (WCF) a projekt databáze.

## <a name="task-overview"></a>Přehled úloh

Ve většině případů by měl být každý člen týmu pro vývojáře přidávat obsah do správy zdrojového kódu. Pokud chcete přidat řešení ke správě zdrojového kódu v sadě TFS, budete potřebovat k dokončení těchto kroků:

- Připojení k týmovému projektu.
- Struktura složek týmového projektu na serveru mapují strukturu složek v místním počítači.
- Přidáte řešení a její obsah do správy zdrojového kódu.
- Přidáte žádné externí závislosti do správy zdrojového kódu.

Toto téma vám ukáže, jak k provedení těchto postupů.

Postupy v tomto tématu a úlohy Předpokládejme, že jste již vytvořili nového týmového projektu sady TFS ke správě vašeho obsahu. Další informace o vytvoření nového týmového projektu, najdete v části [vytvoření týmového projektu v sadě TFS](creating-a-team-project-in-tfs.md).

### <a name="who-performs-these-procedures"></a>Kdo provádí tyto postupy?

Ve většině případů by měl být každý člen týmu pro vývojáře možnost přidávat a upravovat obsah v rámci konkrétní týmové projekty.

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a>Připojení k týmovému projektu a vytvořte složku mapování

Než přidáte žádný obsah k řízení zdrojů, potřebujete připojení k týmovému projektu a vytvořte mapování mezi struktura složek na serveru a systému souborů na místním počítači.

**K připojení k týmovému projektu a mapování místní cestu**

1. Na pracovní stanici developer otevřete Visual Studio 2010.
2. V sadě Visual Studio na **Team** nabídky, klikněte na tlačítko **připojit k serveru Team Foundation Server**.

    > [!NOTE]
    > Pokud jste již nakonfigurovali připojení k serveru TFS, můžete vynechat kroky 3 až 6.
3. V **připojení k týmovému projektu** dialogové okno, klikněte na tlačítko **servery**.
4. V **přidat nebo odebrat Team Foundation Server** dialogové okno, klikněte na tlačítko **přidat**.
5. V **přidat Team Foundation Server** dialogové okno, zadejte podrobnosti o instanci sady TFS a pak klikněte na tlačítko **OK**.

    ![](adding-content-to-source-control/_static/image1.png)
6. V **přidat nebo odebrat Team Foundation Server** dialogové okno, klikněte na tlačítko **Zavřít**.
7. V **připojení k týmovému projektu** dialogové okno, vyberte instanci sady TFS, chcete připojit, vyberte tým projektu vyberte týmový projekt, který chcete přidat do, kolekce a pak klikněte na tlačítko **Connect**.

    ![](adding-content-to-source-control/_static/image2.png)
8. V **Team Explorer** okně rozbalte vašemu týmovému projektu a potom dvakrát klikněte na **správy zdrojového kódu**.

    ![](adding-content-to-source-control/_static/image3.png)
9. Na **Průzkumník správy zdrojového kódu** , klikněte na **není mapováno**.

    ![](adding-content-to-source-control/_static/image4.png)
10. V **mapy** dialogu **místní složky** pole, přejděte do (nebo ji vytvořte) do místní složky fungovat jako kořenová složka pro týmový projekt, a pak klikněte na **mapy**.

    ![](adding-content-to-source-control/_static/image5.png)
11. Když se zobrazí výzva ke stažení zdrojových souborů, klikněte na tlačítko **Ano**.

    ![](adding-content-to-source-control/_static/image6.png)

V tomto okamžiku jste namapovali serverové složky pro týmový projekt do místní složky na pracovní stanici developer. Existující obsah si také stáhnout z týmový projekt na strukturu místní složky. Nyní můžete spustit přidat vlastní obsah do správy zdrojového kódu.

## <a name="add-projects-and-solutions-to-source-control"></a>Přidat projekty a řešení do správy zdrojového kódu

Pokud chcete přidat do správy zdrojového kódu projekty a řešení, musíte nejprve přesunout do mapované složky pro týmový projekt na místním počítači. Potom můžete zkontrolovat obsah dodatky synchronizovat se serverem.

**K přidání projektů do správy zdrojového kódu**

1. Na pracovní stanici developer přesuňte do příslušného umístění v rámci strukturu mapované složky pro týmový projekt projekty a řešení.

    > [!NOTE]
    > Mnoho organizací bude mít žádoucí pro projekty a řešení uspořádání ve správě zdrojového kódu. Informace o tom, jak struktura složek, najdete v části [postupy: struktura si zdroj ovládacího prvku složek na serveru Team Foundation Server](https://msdn.microsoft.com/en-us/library/bb668992.aspx).
2. Otevřete řešení v sadě Visual Studio 2010.
3. V **Průzkumníku řešení** oken, klikněte pravým tlačítkem na řešení a pak klikněte na tlačítko **přidat řešení správy zdrojového kódu**.

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > V některých případech, v závislosti na tom, jak vaše organizace Neradi struktura obsahu v sadě TFS musíte k přidání projektů do správy zdrojového jednotlivě a poskytují jemně odstupňovanou kontrolu nad uspořádání vašeho zdrojového kódu.
4. Ověřte, zda **Průzkumník správy zdrojového kódu** karta zobrazuje obsah, který jste přidali v rámci struktury složek serveru pro týmový projekt.

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > **Průzkumník správy zdrojového kódu** karta zobrazuje svůj obsah pomocí žádné další výzvy, protože jste přidali řešení do mapované složky v místním systému souborů. Pokud vaše řešení v nenamapovaný umístění, by vyzváni k zadání umístění složek v sadě TFS a do místního systému souborů.
5. Na **Průzkumník správy zdrojového kódu** ve **složky** podokně klikněte pravým tlačítkem na týmový projekt (například **ContactManager**) a pak klikněte na tlačítko **vrátit se změnami Změny čekající na zpracování**.
6. V **změnami – zdrojové soubory** dialogové okno, zadejte komentář a potom klikněte na **změnami**.

    ![](adding-content-to-source-control/_static/image9.png)

V tomto okamžiku jste přidali řešení ke správě zdrojového kódu v sadě TFS.

## <a name="add-external-dependencies-to-source-control"></a>Přidat externí závislosti do správy zdrojového kódu

Když přidáte do správy zdrojového kódu projekt nebo řešení, všechny soubory a složky v projekt nebo řešení bude také přidán. Ale v mnoha případech, projekty a řešení také spoléhají na externí závislosti, stejně jako místní sestavení, aby správně fungoval. Je nutné přidat, že tyto prostředky k řízení zdrojů umožníte Team Build i ostatní členové týmu pro vývojáře sestavení kódu úspěšně.

Struktura složek pro ukázkové řešení obraťte se na správce například obsahuje složku s názvem balíčky. Tato položka obsahuje sestavení a různé doprovodné materiály pro ADO.NET Entity Framework 4.1. Balíčky složka není součástí řešení obraťte se na správce, ale nebude řešení úspěšně sestavení bez. Povolit Team Build sestavte řešení, musíte přidat složku balíčky do správy zdrojového kódu.

> [!NOTE]
> Zahrnutí do složky balíčků je typický pro co se stane, když přidáte Entity Framework nebo podobné prostředky, vaše řešení pomocí rozšíření NuGet pro Visual Studio 2010.


**Přidání obsahu jiný projekt do správy zdrojového kódu**

1. Zajistěte, aby položky, které chcete přidat (například složce balíčků) do příslušného umístění v rámci mapované složky v místním systému souborů.
2. V sadě Visual Studio 2010 v **Team Explorer** okně rozbalte vašemu týmovému projektu a potom dvakrát klikněte na **správy zdrojového kódu**.

    ![](adding-content-to-source-control/_static/image10.png)
3. Na **Průzkumník správy zdrojového kódu** ve **složky** podokně, vyberte složku, která obsahuje položky nebo položky, které chcete přidat.
4. Klikněte **přidat položky do složky** tlačítko.

    ![](adding-content-to-source-control/_static/image11.png)
5. V **přidat do správy zdrojového kódu** dialogové okno, vyberte složku nebo položky, které chcete přidat, a pak klikněte na **Další**.

    ![](adding-content-to-source-control/_static/image12.png)
6. Na **vyloučit položky** , vyberte všechny požadované položky, které byly automaticky vyloučeny (například sestavení) a potom klikněte na **zahrnout položky**.

    ![](adding-content-to-source-control/_static/image13.png)
7. Na **položky k přidání** kartě, ověřte, zda jsou uvedeny všechny soubory, které chcete zahrnout a pak klikněte na tlačítko **Dokončit**.

    ![](adding-content-to-source-control/_static/image14.png)
8. V **Průzkumník správy zdrojového kódu** okně klikněte na tlačítko **změnami** tlačítko.

    ![](adding-content-to-source-control/_static/image15.png)
9. V **změnami – zdrojové soubory** dialogové okno, zadejte komentář a potom klikněte na **změnami**.

V tomto okamžiku jste přidali vnější závislosti pro vaše řešení do správy zdrojového kódu.

## <a name="conclusion"></a>Závěr

Toto téma popisuje postup připojení k týmovému projektu, mapy strukturu složek a přidání obsahu do správy zdrojového kódu. Další informace o tom, jak pracovat s položkami ve správě zdrojového kódu najdete v tématu [pomocí verzí](https://msdn.microsoft.com/en-us/library/ms181368.aspx).

Dalším tématu [konfigurace serveru TFS sestavení pro nasazení webu](configuring-a-tfs-build-server-for-web-deployment.md), popisuje postup přípravy serveru TFS Team Build k vytváření a nasazování řešení.

## <a name="further-reading"></a>Další čtení

Další informace o práci se správa zdrojového kódu v sadě TFS naleznete v tématu [pomocí verzí](https://msdn.microsoft.com/en-us/library/ms181368.aspx).

>[!div class="step-by-step"]
[Předchozí](creating-a-team-project-in-tfs.md)
[další](configuring-a-tfs-build-server-for-web-deployment.md)
