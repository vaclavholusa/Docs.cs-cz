---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: Přidávání obsahu do správy zdrojového kódu | Dokumentace Microsoftu
author: jrjlee
description: Toto téma vysvětluje, jak přidat obsah do správy zdrojového kódu v Team Foundation Server (TFS) 2010. Popisuje postup přidání řešení a projektů do týmu proje...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: 2119705a75d0717d05d4a7db69b3f5d38b1cdd45
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754788"
---
<a name="adding-content-to-source-control"></a>Přidávání obsahu do správy zdrojového kódu
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma vysvětluje, jak přidat obsah do správy zdrojového kódu v Team Foundation Server (TFS) 2010. Popisuje postup přidání řešení a projektů do týmového projektu v TFS, a vysvětluje, jak přidat externí závislosti, jako jsou rozhraní nebo sestavení do správy zdrojového kódu.


Toto téma je součástí série kurzů podle požadavků na nasazení enterprise fiktivní společnosti s názvem společnosti Fabrikam, Inc. V této sérii kurzů používá ukázkové řešení&#x2014; [řešení Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s realistické úroveň složitosti, včetně aplikace ASP.NET MVC 3, komunikace Windows Služba Foundation (WCF) a databázový projekt.

## <a name="task-overview"></a>Přehled úloh

Ve většině případů by měl být každý člen týmu vývojářů možnost přidávat obsah do správy zdrojového kódu. Chcete-li přidat řešení do správy zdrojového kódu v sadě TFS, budete potřebovat k dokončení těchto kroků:

- Připojení k týmovému projektu.
- Struktura složek týmového projektu na serveru namapujte na strukturu složek v místním počítači.
- Přidáte do správy zdrojového kódu řešení a jeho obsah.
- Přidání externích závislostí do správy zdrojového kódu.

Toto téma se ukazují, jak provést tyto postupy.

Úlohy a názorné postupy v tomto tématu se předpokládá, že jste již vytvořili nového týmového projektu sady TFS spravovat obsah. Další informace o vytvoření nového týmového projektu naleznete v tématu [vytvořením týmového projektu v TFS](creating-a-team-project-in-tfs.md).

### <a name="who-performs-these-procedures"></a>Kdo provádí tyto postupy?

Ve většině případů by měl být každý člen týmu vývojářů moct přidávat a upravovat obsah v rámci konkrétní týmové projekty.

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a>Připojte k týmovému projektu a vytvořte mapování složky

Předtím, než přidáte veškerý obsah do správy zdrojových kódů, musíte připojit k týmovému projektu a vytvořte mapování mezi strukturu složek na serveru a systému souborů na místním počítači.

**Připojení k týmovému projektu a přiřadit místní cesta**

1. Na vaši vývojářskou pracovní stanici otevřete Visual Studio 2010.
2. V sadě Visual Studio na **týmu** nabídky, klikněte na tlačítko **připojit k Team Foundation Server**.

    > [!NOTE]
    > Pokud jste už nakonfigurovali připojení k serveru TFS, můžete vynechat kroky 3 až 6.
3. V **připojení k týmovému projektu** dialogové okno, klikněte na tlačítko **servery**.
4. V **přidat nebo odebrat Team Foundation Server** dialogové okno, klikněte na tlačítko **přidat**.
5. V **přidat Team Foundation Server** dialogové okno, zadejte podrobnosti o instanci TFS a potom klikněte na tlačítko **OK**.

    ![](adding-content-to-source-control/_static/image1.png)
6. V **přidat nebo odebrat Team Foundation Server** dialogové okno, klikněte na tlačítko **Zavřít**.
7. V **připojit k týmovému projektu** dialogové okno, vyberte instanci TFS, kterou chcete připojit, vyberte tým projektu kolekce, vyberte týmový projekt, který chcete přidat do a pak klikněte na tlačítko **připojit**.

    ![](adding-content-to-source-control/_static/image2.png)
8. V **Team Exploreru** okna, rozbalte týmový projekt a potom dvakrát klikněte na **správy zdrojových kódů**.

    ![](adding-content-to-source-control/_static/image3.png)
9. Na **Průzkumníka správy zdrojového kódu** klikněte na tlačítko **nenamapované**.

    ![](adding-content-to-source-control/_static/image4.png)
10. V **mapy** v dialogu **místní složky** pole, přejděte do (nebo vytvoření) do místní složky vystupovat jako kořenová složka pro týmový projekt a potom klikněte na tlačítko **mapy**.

    ![](adding-content-to-source-control/_static/image5.png)
11. Po zobrazení výzvy ke stažení zdrojových souborů, klikněte na tlačítko **Ano**.

    ![](adding-content-to-source-control/_static/image6.png)

V tomto okamžiku jste namapovali serverové složky pro týmový projekt do místní složky na vaši vývojářskou pracovní stanici. Jakýkoli existující obsah jsme také stáhnout z týmového projektu do struktury vaší místní složky. Teď můžete začít přidávat svůj obsah do správy zdrojového kódu.

## <a name="add-projects-and-solutions-to-source-control"></a>Přidat projekty a řešení do správy zdrojového kódu

Chcete-li přidat projekty a řešení do správy zdrojových kódů, musíte nejprve přesuňte je pro mapovanou složku pro týmový projekt na svém místním počítači. Potom můžete zkontrolovat v obsahu k synchronizaci dodatky k serveru.

**K přidání projektů do správy zdrojového kódu**

1. Na vaši vývojářskou pracovní stanici přesuňte své projekty a řešení na příslušné místo v rámci struktury mapovaná složka pro týmový projekt.

    > [!NOTE]
    > Mnoho organizací bude mít upřednostňovaný způsob jak projekty a řešení by měly být uspořádány ve správě zdrojového kódu. Informace o tom, jak struktura složek najdete v tématu [How To: struktura Your složky správy zdrojového kódu v sadě Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).
2. Otevřete řešení v sadě Visual Studio 2010.
3. V **Průzkumníka řešení** okna, klikněte pravým tlačítkem na řešení a potom klikněte na tlačítko **přidat řešení do správy zdrojových kódů**.

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > V některých případech může v závislosti na tom, jak vaše organizace ráda struktura obsahu na serveru TFS budete muset přidat projekty do správy zdrojových kódů jednotlivě a poskytuje jemněji odstupňovanou kontrolu nad uspořádání zdrojového kódu.
4. Ověřte, že **Průzkumníka správy zdrojového kódu** karta zobrazuje obsah, který jste přidali v rámci struktury složky serveru pro týmový projekt.

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > **Průzkumníka správy zdrojového kódu** karta zobrazuje váš obsah s zobrazovat žádné další výzvy vzhledem k tomu, že jste přidali řešení pro mapovanou složku na místním systému souborů. Pokud vaše řešení v nenamapovaného umístění, by vyzváni k zadání umístění složek na serveru TFS i vašeho místního systému souborů.
5. Na **Průzkumníka správy zdrojového kódu** kartě **složky** podokně klikněte pravým tlačítkem na týmový projekt (například **ContactManager**) a potom klikněte na tlačítko **vrátit se změnami Čekající změny**.
6. V **vrátit se změnami – zdrojové soubory** dialogové okno, zadejte komentář a potom klikněte na tlačítko **vrátit se změnami**.

    ![](adding-content-to-source-control/_static/image9.png)

V tomto okamžiku jste přidali řešení do správy zdrojového kódu v sadě TFS.

## <a name="add-external-dependencies-to-source-control"></a>Přidat externí závislosti do správy zdrojového kódu

Když přidáte projekt nebo řešení do správy zdrojových kódů, všechny soubory a složky v projektu nebo řešení se také přidají. Ale v mnoha případech, projekty a řešení využívají také externí závislosti, jako je místní sestavení, aby správně fungoval. Budete muset přidat tyto prostředky do správy zdrojového kódu Team Build a ostatní členové týmu pro vývojáře, aby váš kód sestavení proběhne úspěšně.

Například strukturu složek pro ukázkové řešení Správce kontaktů obsahuje složku s názvem balíčky. Tato položka obsahuje sestavení a různé Podpůrné prostředky pro ADO.NET Entity Framework 4.1. Složku packages není součástí řešení Správce kontaktů, ale řešení nebude úspěšně sestavit bez něj. Povolit týmového sestavení k sestavení řešení, budete muset přidat složku balíčků do správy zdrojového kódu.

> [!NOTE]
> Zahrnutí složky packages je typický pro co se stane, když přidáte Entity Framework nebo podobné prostředky do řešení pomocí rozšíření NuGet pro Visual Studio 2010.


**K přidání obsahu mimo projekt do správy zdrojového kódu**

1. Zajistěte, aby byly položky, které chcete přidat (například složce balíčků) v příslušné umístění v rámci mapované složky na vašem místním systému souborů.
2. V sadě Visual Studio 2010 v **Team Exploreru** okna, rozbalte týmový projekt a potom dvakrát klikněte na **správy zdrojových kódů**.

    ![](adding-content-to-source-control/_static/image10.png)
3. Na **Průzkumníka správy zdrojového kódu** kartě **složky** podokně, vyberte složku, která obsahuje položky nebo položky, které chcete přidat.
4. Klikněte na tlačítko **přidat položky do složky** tlačítko.

    ![](adding-content-to-source-control/_static/image11.png)
5. V **přidat do správy zdrojových kódů** dialogového okna, vyberte složku nebo položky, které chcete přidat, a potom klikněte na **Další**.

    ![](adding-content-to-source-control/_static/image12.png)
6. Na **vyloučené položky** kartu, vyberte všechny požadované položky, které jsou automaticky vyloučené (například sestavení) a potom klikněte na tlačítko **zahrnout položky**.

    ![](adding-content-to-source-control/_static/image13.png)
7. Na **položky, které chcete přidat** kartu, ověřte, zda jsou uvedeny všechny soubory, které chcete zahrnout a potom klikněte na **Dokončit**.

    ![](adding-content-to-source-control/_static/image14.png)
8. V **Průzkumníka správy zdrojového kódu** okna, klikněte na tlačítko **vrátit se změnami** tlačítko.

    ![](adding-content-to-source-control/_static/image15.png)
9. V **vrátit se změnami – zdrojové soubory** dialogové okno, zadejte komentář a potom klikněte na tlačítko **vrátit se změnami**.

V tomto okamžiku jste přidali vnější závislosti pro vaše řešení do správy zdrojového kódu.

## <a name="conclusion"></a>Závěr

Toto téma popisuje, jak se připojit k týmovému projektu, zmapování struktury složky a přidat obsah do správy zdrojového kódu. Další informace o tom, jak pracovat s položkami pod správou zdrojových kódů, najdete v části [pomocí správy verzí](https://msdn.microsoft.com/library/ms181368.aspx).

Dalším tématu s názvem [konfigurace TFS Build Server pro nasazení webu](configuring-a-tfs-build-server-for-web-deployment.md), popisuje postup přípravy serveru TFS Team Build pro sestavení a nasazení vašeho řešení.

## <a name="further-reading"></a>Další čtení

Další informace o práci se správou zdrojového kódu v sadě TFS, naleznete v tématu [pomocí správy verzí](https://msdn.microsoft.com/library/ms181368.aspx).

> [!div class="step-by-step"]
> [Předchozí](creating-a-team-project-in-tfs.md)
> [další](configuring-a-tfs-build-server-for-web-deployment.md)
