---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: Vytvoření týmového projektu v sadě TFS | Microsoft Docs
author: jrjlee
description: Toto téma popisuje postup vytvoření nového týmového projektu v Team Foundation Server (TFS) 2010.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: 96e0ee5fd0b74e7b22b8e346aa8462f7558a3ddc
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960691"
---
<a name="creating-a-team-project-in-tfs"></a>Vytvoření týmového projektu v sadě TFS
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje postup vytvoření nového týmového projektu v Team Foundation Server (TFS) 2010.


Toto téma je součástí ze série kurzů na základě kolem podnikové požadavky nasazení fiktivní společnost s názvem Fabrikam, Inc. Tento kurz řady používá ukázkové řešení&#x2014; [řešení obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s úrovní realistické složitější, včetně aplikace ASP.NET MVC 3, komunikaci Windows Služba Foundation (WCF) a projekt databáze.

## <a name="task-overview"></a>Přehled úloh

Zřídit a použití nového týmového projektu v sadě TFS, budete potřebovat k dokončení těchto kroků:

- Udělení oprávnění pro uživatele, který se vytvoří nový týmový projekt.
- Vytvoření týmového projektu.
- Udělení oprávnění pro členy týmu, kteří budou pracovat na projekt.
- Zkontrolujte, že v některých obsahu.

Toto téma vám ukáže, jak k provedení těchto postupů, a určí uživatele a role úlohy, které by mohly být zodpovědná za každý postup. Mějte na paměti, že v závislosti na struktuře vaší organizace, každý z těchto úloh může být jiné osoba.

Úlohy a postupy v tomto tématu předpokládá, že jste nainstalován a nakonfigurován sady TFS a že jste vytvořili kolekci týmových projektů jako součást procesu konfigurace. Další informace o těchto domněnek a pro obecné informace na scénáři, najdete v části [konfigurace serveru TFS sestavení pro nasazení webu](configuring-a-tfs-build-server-for-web-deployment.md).

## <a name="grant-permissions-to-the-team-project-creator"></a>Udělení oprávnění pro tvůrce týmového projektu

Chcete-li vytvořit nového týmového projektu, potřebujete tato oprávnění:

- Musíte mít **vytvořit nové projekty** oprávnění v aplikační vrstvě TFS. Toto oprávnění přidáním uživatelům udělíte obvykle **správci kolekcí projektů** skupiny sady TFS. **Team Foundation správci** toto oprávnění zahrnuje globální skupinu.
- Musíte mít oprávnění k vytvoření nové týmové weby v rámci kolekce webů služby SharePoint, která odpovídá kolekci týmového projektu sady TFS. Toto oprávnění přidáním uživatele do skupiny služby SharePoint s udělíte obvykle **úplné řízení** kolekce webů oprávnění na webu služby SharePoint.
- Pokud používáte funkce SQL Server Reporting Services, musíte být členem skupiny **Team Foundation Content Manager** role ve službě Reporting Services.

### <a name="who-performs-these-procedures"></a>Kdo provádí tyto postupy?

Obvykle osoba nebo skupina, který spravuje nasazení TFS také provede tyto postupy.

Protože to je vysoce privilegované sadu oprávnění, nové týmové projekty obvykle vytvářejí malou podmnožinu uživatelů s zodpovědnost za správu nasazení sady TFS. Vývojáři obvykle neposkytne oprávnění potřebná k vytvoření nového týmového projektu.

### <a name="grant-permissions-in-tfs"></a>Udělení oprávnění v sadě TFS

Pokud chcete povolit uživateli vytvoření nového týmového projektu, první úloh vysoké úrovně je přidání uživateli **správci kolekcí projektů** pro kolekci týmových projektů.

**Přidání uživatele do skupiny Administrators kolekci projektu**

1. Na serveru TFS na **spustit** nabídky, přejděte na příkaz **všechny programy**, klikněte na tlačítko **Microsoft Team Foundation Server 2010**a potom klikněte na **Team Foundation Konzole pro správu**.
2. Ve stromovém zobrazení navigace, rozbalte položku **aplikační vrstvě**a potom klikněte na **kolekcemi týmových projektů**.

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. V **kolekcemi týmových projektů** podokně, vyberte tým projektu kolekce, kterou chcete spravovat.

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. Na **Obecné** , klikněte na **členství ve skupině**.

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. V **globální skupiny** dialogové okno, vyberte **správci kolekcí projektů** skupiny a pak klikněte na tlačítko **vlastnosti**.
6. V **vlastnosti Team Foundation Server skupiny** dialogové okno, vyberte **Windows uživatele nebo skupinu**a potom klikněte na **přidat**.

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. V **vybrat uživatele, počítače nebo skupiny** dialogové okno, zadejte uživatelské jméno uživatele, kterou chcete k vytvoření nového týmového projektu, klikněte na **Kontrola názvů**a potom klikněte na **OK** .

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. V **vlastnosti Team Foundation Server skupiny** dialogové okno, klikněte na tlačítko **OK**.
9. V **globální skupiny** dialogové okno, klikněte na tlačítko **Zavřít**.

### <a name="grant-permissions-in-sharepoint-services"></a>Udělení oprávnění ve službě SharePoint Services

Dále musíte poskytnout oprávnění uživatele k vytvoření nových lokalit týmu v kolekce webů služby SharePoint, která odpovídá vaší kolekce týmového projektu sady TFS.

**Udělit oprávnění k úplnému řízení na kolekce webů služby SharePoint**

1. V konzole správy pro Team Foundation Server na **kolekcemi týmových projektů** vyberte kolekce týmových projektů, které chcete spravovat.
2. Na **web služby SharePoint** kartě, poznamenejte si hodnotu **aktuální výchozí umístění lokality** adresy URL.

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. Otevřete Internet Explorer a přejděte na adresu URL, které jste si poznamenali v kroku 2.

    > [!NOTE]
    > Pokud nejste přihlášení k systému Windows jako uživatel, který vytvořil kolekce týmového projektu, budete muset přihlásit do služby SharePoint jako tento uživatel aby bylo možné pokračovat.
4. Na **Akce webu** nabídky, klikněte na tlačítko **nastavení lokality**.

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. Na **nastavení lokality** v části **uživatele a oprávnění**, klikněte na tlačítko **lidé a skupiny**.
6. V levém navigačním panelu klikněte na tlačítko **skupiny**.

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. Na **lidé a skupiny: všechny skupiny** klikněte na tlačítko **nastavit skupiny pro tento web**.

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > Může se zobrazit <strong>HTTP 404 nebyl nalezen</strong> chyba z důvodu dvojité kódování chyb HTTP. Pokud k tomu dojde, nahraďte adresu URL s tímto:   
   > `[site_collection_URL]/_layouts/permsetup.aspx` Například:  
   > `http://tfs/sites/Fabrikam%20Web%20Projects/_layouts/permsetup.aspx` 
8. Na **nastavit skupiny pro tento web** přidejte uživatele, který vytvoří týmové projekty k **vlastníky** skupiny a pak klikněte na tlačítko **OK**.

    ![](creating-a-team-project-in-tfs/_static/image10.png)

Další informace o povolení uživatelům vytvářet nové týmové projekty v rámci kolekce týmového projektu, najdete v části [nastavit oprávnění správce pro kolekce týmových projektů](https://msdn.microsoft.com/library/dd547204.aspx).

## <a name="create-a-new-team-project-and-add-users"></a>Vytvoření nového týmového projektu a přidání uživatelů

Jakmile máte potřebná oprávnění, můžete použít **Team Explorer** oken v sadě Visual Studio 2010 pro vytvoření nového týmového projektu. Tento přístup poskytuje průvodce, který shromažďuje všechny požadované informace a provede nezbytných úloh v sadě TFS, SharePoint a SQL Server Reporting Services. Musíte také udělit oprávnění pro nový týmový projekt na členy týmu pro vývojáře, aby se mohly přidávat a upravovat obsah.

### <a name="who-performs-these-procedures"></a>Kdo provádí tyto postupy?

Obvykle správce sady TFS nebo vedoucí týmu vývojáře provede tyto postupy.

### <a name="create-a-new-team-project"></a>Vytvoření nového týmového projektu

Následující postup popisuje vytvoření nového týmového projektu v sadě TFS 2010.

**K vytvoření nového týmového projektu**

1. Na **spustit** nabídky, přejděte na příkaz **všechny programy**, klikněte na tlačítko **Microsoft Visual Studio 2010**, klikněte pravým tlačítkem na **Microsoft Visual Studio 2010**, a pak klikněte na **spustit jako správce**.

    > [!NOTE]
    > Pokud sadu Visual Studio 2010 není spustit jako správce, Průvodce novým týmovým projektem na poslední krok selžou.
2. Pokud **řízení uživatelských účtů** se zobrazí dialogové okno, klikněte na tlačítko **Ano**.
3. V sadě Visual Studio na **Team** nabídky, klikněte na tlačítko **připojit k serveru Team Foundation Server**.

    > [!NOTE]
    > Pokud jste již nakonfigurovali připojení k serveru TFS, můžete vynechat kroky 4 až 7.
4. V **připojení k týmovému projektu** dialogové okno, klikněte na tlačítko **servery**.
5. V **přidat nebo odebrat Team Foundation Server** dialogové okno, klikněte na tlačítko **přidat**.
6. V **přidat Team Foundation Server** dialogové okno, zadejte podrobnosti o instanci sady TFS a pak klikněte na tlačítko **OK**.

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. V **přidat nebo odebrat Team Foundation Server** dialogové okno, klikněte na tlačítko **Zavřít**.
8. V **připojení k týmovému projektu** dialogové okno, vyberte kolekce, které chcete přidat a pak klikněte na projekt sady TFS instanci chcete připojit, vyberte tým **Connect**.

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. V **Team Explorer** okna, klikněte pravým tlačítkem do týmového projektu kolekce a potom klikněte na **nového týmového projektu**.

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. V **nového týmového projektu** dialogové okno, zadejte název a popis pro týmový projekt a pak klikněte na tlačítko **Další**.

    > [!NOTE]
    > Pokud váš týmový projekt obsahuje mezery, může mít některé potíže, když dojdete ke použít nástroj nasazení webu Internetové informační služby (IIS) (Web Deploy) nasazení balíčků z výstupní cesta. Mezery v cestě může být mnohem obtížnější ke spuštění příkazů nasazení webu.

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. Na **vyberte šablonu procesu** vyberte šablonu procesu, který chcete použít ke správě procesu vývoje a pak klikněte na tlačítko **Další**.

    > [!NOTE]
    > Další informace o šablony procesů pro TFS najdete v tématu [nástroje a šablony procesů](https://msdn.microsoft.com/vstudio/aa718795).
12. Na **nastavení lokality Team** stránky, ponechejte výchozí nastavení beze změny a pak klikněte na tlačítko **Další**.
13. Toto nastavení se vytvoří nebo identifikuje, team Web služby SharePoint, který je přidružen týmového projektu sady TFS. Váš vývojový tým mohou používat tuto lokalitu pro správu dokumentaci, účastnit diskuse, vytvářet wiki stránky a provádět různé úlohy, které nejsou v relaci ke kódu. Další informace najdete v tématu [interakce mezi produkty SharePoint a serveru Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).
14. Na **zadejte nastavení řízení zdroje** stránky, ponechejte výchozí nastavení beze změny a pak klikněte na tlačítko **Další**.
15. Toto nastavení určuje, nebo vytvoří umístění v hierarchii složek sady TFS, který bude fungovat jako kořenová složka pro obsah.
16. Na **potvrďte nastavení týmového projektu** klikněte na tlačítko **Dokončit**.
17. Pokud nový týmový projekt je úspěšně vytvořen, na **vytvoření týmového projektu** klikněte na tlačítko **Zavřít**.

### <a name="add-users-to-a-team-project"></a>Přidání uživatelů do týmového projektu

Teď, když jste vytvořili nový týmový projekt, můžete udělit oprávnění uživatelům, aby se mohly spustit přidávání a spolupráce na obsah.

**K přidání uživatele do týmového projektu**

1. V sadě Visual Studio 2010 v **Team Explorer** okna, klikněte pravým tlačítkem na týmový projekt, přejděte na **nastavení projektu Team**a potom klikněte na **členství ve skupině**.

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. Pokud chcete povolit uživatelům přidávat, upravovat a odebírat ve správě zdrojového kódu, je nutné přidat ji do **přispěvatelé** skupiny.
3. V **projektu skupiny** dialogové okno, vyberte **přispěvatelé** skupiny a pak klikněte na tlačítko **vlastnosti**.

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. V **vlastnosti Team Foundation Server skupiny** dialogové okno, vyberte **Windows uživatele nebo skupinu**a potom klikněte na **přidat**.

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. V **vybrat uživatele, počítače nebo skupiny** dialogové okno, zadejte uživatelské jméno uživatele, které chcete přidat do týmového projektu, klikněte na **Kontrola názvů**a potom klikněte na **OK**.

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. V **vlastnosti Team Foundation Server skupiny** dialogové okno, klikněte na tlačítko **OK**.
7. V **projektu skupiny** dialogové okno, klikněte na tlačítko **Zavřít**.

## <a name="conclusion"></a>Závěr

V tomto okamžiku je připravený k použití nového týmového projektu a váš tým vývojáře můžete spustit přidávání obsahu a spolupráce v procesu vývoje.

Dalším tématu [přidávání obsahu do správy zdrojového kódu](adding-content-to-source-control.md), popisuje postup přidání obsahu do správy zdrojového kódu.

## <a name="further-reading"></a>Další čtení

Širší pokyny týkající se vytváření týmových projektů sady TFS najdete v tématu [vytvoření týmového projektu](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx). Další informace o povolení uživatelům vytvářet nové týmové projekty v rámci kolekce týmového projektu, najdete v části [nastavit oprávnění správce pro kolekce týmových projektů](https://msdn.microsoft.com/library/dd547204.aspx). Další informace o přidávání uživatelů do týmových projektů najdete v tématu [přidání uživatelů do týmových projektů](https://msdn.microsoft.com/library/bb558971.aspx).

> [!div class="step-by-step"]
> [Předchozí](configuring-team-foundation-server-for-web-deployment.md)
> [další](adding-content-to-source-control.md)
