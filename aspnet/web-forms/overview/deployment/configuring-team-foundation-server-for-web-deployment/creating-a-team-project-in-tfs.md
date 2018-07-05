---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: Vytvoření týmového projektu v TFS | Dokumentace Microsoftu
author: jrjlee
description: Toto téma popisuje postup vytvoření nového týmového projektu v Team Foundation Server (TFS) 2010.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: 2c3b30cac408f47d7d15ae7456f0744219506c85
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397558"
---
<a name="creating-a-team-project-in-tfs"></a>Vytvoření týmového projektu v sadě TFS
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje postup vytvoření nového týmového projektu v Team Foundation Server (TFS) 2010.


Toto téma je součástí série kurzů podle požadavků na nasazení enterprise fiktivní společnosti s názvem společnosti Fabrikam, Inc. V této sérii kurzů používá ukázkové řešení&#x2014; [řešení Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s realistické úroveň složitosti, včetně aplikace ASP.NET MVC 3, komunikace Windows Služba Foundation (WCF) a databázový projekt.

## <a name="task-overview"></a>Přehled úloh

Zřízení a použití nového týmového projektu v TFS, budete potřebovat k dokončení těchto kroků:

- Udělení oprávnění pro uživatele, který se vytvoří nový týmový projekt.
- Vytvořte týmový projekt.
- Udělení oprávnění členům týmu, kteří budou pracovat na projektu.
- Vrátit se změnami nějaký obsah.

V tomto tématu ukazují, jak provést tyto postupy, a určí uživatele a úlohy, které by mohly být zodpovědný za každý postup. Mějte na paměti, že v závislosti na struktuře vaší organizace, každá z těchto úloh může být odpovědnost na jinou osobu.

Úlohy a názorné postupy v tomto tématu se předpokládá, že je nainstalujete a nakonfigurujete TFS, a že jste vytvořili kolekci týmového projektu jako součást procesu konfigurace. Další informace o těchto domněnek a obecné informace na scénáři, najdete v části [nakonfigurovat Server sestavení TFS pro nasazení webu](configuring-a-tfs-build-server-for-web-deployment.md).

## <a name="grant-permissions-to-the-team-project-creator"></a>Udělení oprávnění pro autora týmového projektu

Chcete-li vytvořit nový týmový projekt, je třeba tato oprávnění:

- Musíte mít **vytvořit nové projekty** oprávnění na aplikační vrstvu TFS. Obvykle tak, že přidáte uživatelům udělit toto oprávnění **Project Collection Administrators** skupinu TFS. **Správci serveru Team Foundation** toto oprávnění zahrnuje globální skupina.
- Musíte mít oprávnění k vytvoření nových lokalit týmu v rámci kolekce webů služby SharePoint, která odpovídá kolekci týmových projektů TFS. Obvykle udělit tato oprávnění tak, že přidáte uživatele do skupiny SharePoint s **úplné řízení** kolekce webů oprávnění služby SharePoint.
- Pokud používáte funkce SQL Server Reporting Services, musíte být členem skupiny **správce obsahu Team Foundation** role ve službě Reporting Services.

### <a name="who-performs-these-procedures"></a>Kdo provádí tyto postupy?

Obvykle osoba nebo skupina, která spravuje nasazení TFS také provádí tyto postupy.

Protože se jedná s vysokou úrovní oprávnění sadu oprávnění, nové týmové projekty jsou obvykle vytvořené malou podmnožinu uživatelů s zodpovědnost za správu nasazení TFS. Vývojáři obvykle neposkytne oprávnění požadovaná k vytvoření nových týmových projektů.

### <a name="grant-permissions-in-tfs"></a>Udělení oprávnění v TFS

Pokud chcete povolit tak uživateli vytvářet nové týmové projekty, prvního úkolu vysoké úrovně je přidání uživatele do **Project Collection Administrators** pro kolekci týmového projektu.

**K přidání uživatele do skupiny Správci kolekcí projektů**

1. Na serveru TFS na **Start** nabídky, přejděte k **všechny programy**, klikněte na tlačítko **Microsoft Team Foundation Server 2010**a potom klikněte na tlačítko **Team Foundation Konzola pro správu**.
2. Ve stromovém zobrazení navigace, rozbalte **aplikační vrstva**a potom klikněte na tlačítko **kolekce týmových projektů**.

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. V **kolekce týmových projektů** podokně, vyberte je vhodné spravovat kolekce týmových projektů.

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. Na **Obecné** klikněte na tlačítko **členství ve skupině**.

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. V **globální skupiny** dialogové okno, vyberte **Project Collection Administrators** skupiny a potom klikněte na tlačítko **vlastnosti**.
6. V **vlastnosti skupiny serveru Team Foundation** dialogu **Windows uživatele nebo skupinu**a potom klikněte na tlačítko **přidat**.

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. V **vybrat uživatele, počítače nebo skupiny** dialogové okno, zadejte uživatelské jméno uživatele, který chcete vytvořit nové týmové projekty, klikněte na **Kontrola názvů**a potom klikněte na tlačítko **OK** .

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. V **vlastnosti skupiny serveru Team Foundation** dialogové okno, klikněte na tlačítko **OK**.
9. V **globální skupiny** dialogové okno, klikněte na tlačítko **Zavřít**.

### <a name="grant-permissions-in-sharepoint-services"></a>Udělení oprávnění ve službě SharePoint Services

Dále je třeba udělit oprávnění uživatele k vytvoření nové týmové weby v kolekci webů služby SharePoint, která odpovídá vaší kolekci týmových projektů TFS.

**Udělení oprávnění k úplnému řízení na kolekce webů služby SharePoint**

1. V Team Foundation Server Administration Console na **kolekce týmových projektů** vyberte kolekci týmových projektů, kterou chcete spravovat.
2. Na **webu služby SharePoint** kartu, poznamenejte si hodnotu **aktuální výchozí umístění webu** adresy URL.

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. Spusťte aplikaci Internet Explorer a přejděte na adresu URL, kterou jste si poznamenali v kroku 2.

    > [!NOTE]
    > Pokud nejste přihlášení k Windows jako uživatel, který vytvořil kolekci týmových projektů, budete potřebovat k přihlášení do služby SharePoint jako tento uživatel aby bylo možné pokračovat.
4. Na **Akce webu** nabídky, klikněte na tlačítko **nastavení webu**.

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. Na **nastavení webu** stránce v části **uživatelů a oprávnění**, klikněte na tlačítko **osoby a skupiny**.
6. V levém navigačním panelu klikněte na tlačítko **skupiny**.

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. Na **osoby a skupiny: všechny skupiny** klikněte na **nastavení skupin pro tuto lokalitu**.

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > Může se zobrazit <strong>HTTP 404 Nenalezeno</strong> chyba z důvodu double kódování chyby protokolu HTTP. Pokud k tomu dojde, nahraďte adresu URL s tímto:   
   > `[site_collection_URL]/_layouts/permsetup.aspx` Příklad:  
   > `http://tfs/sites/Fabrikam%20Web%20Projects/_layouts/permsetup.aspx` 
8. Na **nastavení skupin pro tuto lokalitu** stránce, přidejte uživatele, který bude vytvářet týmové projekty do **vlastníky** skupiny a potom klikněte na tlačítko **OK**.

    ![](creating-a-team-project-in-tfs/_static/image10.png)

Další informace o povolení uživatelům vytvářet nové týmové projekty v kolekci týmových projektů, naleznete v tématu [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).

## <a name="create-a-new-team-project-and-add-users"></a>Vytvoření nového týmového projektu a přidání uživatelů

Jakmile máte potřebná oprávnění, můžete použít **Team Exploreru** okna v sadě Visual Studio 2010 k vytvoření nového týmového projektu. Tento přístup poskytuje průvodce, který shromažďuje všechny požadované informace a provádí potřebné úkoly v TFS, SharePoint a SQL Server Reporting Services. Také budete muset udělit oprávnění pro nový týmový projekt na členy týmu pro vývojáře, aby mohli přidávat a upravovat obsah.

### <a name="who-performs-these-procedures"></a>Kdo provádí tyto postupy?

Tyto postupy provádí obvykle správcem serveru TFS nebo vedoucí týmu pro vývojáře.

### <a name="create-a-new-team-project"></a>Vytvořte nový týmový projekt

Následující postup popisuje, jak vytvořit nový týmový projekt v TFS 2010.

**Chcete-li vytvořit nový týmový projekt**

1. Na **Start** nabídky, přejděte k **všechny programy**, klikněte na tlačítko **sadu Microsoft Visual Studio 2010**, klikněte pravým tlačítkem na **sadu Microsoft Visual Studio 2010**, a pak klikněte na tlačítko **spustit jako správce**.

    > [!NOTE]
    > Při spuštění sady Visual Studio 2010 jako správce, Průvodce novým týmovým projektem se nezdaří v posledním kroku.
2. Pokud **řízení uživatelských účtů** dialogové okno se zobrazí, klikněte na tlačítko **Ano**.
3. V sadě Visual Studio na **týmu** nabídky, klikněte na tlačítko **připojit k Team Foundation Server**.

    > [!NOTE]
    > Pokud jste už nakonfigurovali připojení k serveru TFS, můžete vynechat kroky 4 až 7.
4. V **připojení k týmovému projektu** dialogové okno, klikněte na tlačítko **servery**.
5. V **přidat nebo odebrat Team Foundation Server** dialogové okno, klikněte na tlačítko **přidat**.
6. V **přidat Team Foundation Server** dialogové okno, zadejte podrobnosti o instanci TFS a potom klikněte na tlačítko **OK**.

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. V **přidat nebo odebrat Team Foundation Server** dialogové okno, klikněte na tlačítko **Zavřít**.
8. V **připojit k týmovému projektu** dialogové okno, vyberte instanci TFS, kterou chcete připojit, vyberte tým projektu kolekce, kterou chcete přidat a potom klikněte na **připojit**.

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. V **Team Exploreru** okna, klikněte pravým tlačítkem na kolekci projektu týmu a potom klikněte na **novým týmovým projektem**.

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. V **novým týmovým projektem** dialogové okno, zadejte název a popis pro týmový projekt a potom klikněte na tlačítko **Další**.

    > [!NOTE]
    > Pokud váš týmový projekt obsahuje mezery, které mohou nastat některé problémy, když přejdete na použijte nástroj pro nasazení Internetové informační služby (IIS) webu (nasazení webu) nasadit balíčky z výstupní cesta. Mezery v cestě může ztížit mnohem více ke spuštění příkazů pro nasazení webu.

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. Na **vyberte šablonu procesu** vyberte šablonu procesu, kterou chcete použít ke správě procesu vývoje a potom klikněte na tlačítko **Další**.

    > [!NOTE]
    > Další informace o šablonách procesu TFS naleznete v tématu [nástroje a šablony procesů](https://msdn.microsoft.com/vstudio/aa718795).
12. Na **nastavení týmového webu** stránce, ponechejte výchozí nastavení pro beze změny a pak klikněte na tlačítko **Další**.
13. Toto nastavení se vytvoří nebo identifikuje týmový web Sharepointu, který je spojen s týmovým projektem TFS. Váš vývojový tým mohou používat tuto lokalitu ke správě dokumentaci, účastnit diskusní vlákna, vytvořit wiki stránek a provádění různých úloh, které nesouvisejí se kód. Další informace najdete v tématu [interakce mezi produkty SharePoint a serveru Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).
14. Na **zadejte nastavení správy zdrojových kódů** stránce, ponechejte výchozí nastavení pro beze změny a pak klikněte na tlačítko **Další**.
15. Toto nastavení určuje, nebo vytvoří umístění v hierarchii složek serveru TFS, který bude sloužit jako kořenová složka pro obsah.
16. Na **potvrzení nastavení týmového projektu** klikněte na **Dokončit**.
17. Pokud nový týmový projekt je úspěšně vytvořen, na **týmový projekt vytvořen** klikněte na **Zavřít**.

### <a name="add-users-to-a-team-project"></a>Přidání uživatelů do týmového projektu

Teď, když jste vytvořili nový týmový projekt, můžete udělit oprávnění uživatelům, aby se mohly spustit přidávání a spolupráci nad obsahem.

**Přidání uživatelů do týmového projektu**

1. V sadě Visual Studio 2010 v **Team Exploreru** okna, klikněte pravým tlačítkem na týmový projekt, přejděte na **nastavení týmového projektu**a potom klikněte na tlačítko **členství ve skupině**.

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. A povolit tak uživateli přidávat, upravovat a odebírat kód pod správou zdrojových kódů, je třeba přidat ji do **přispěvatelé** skupiny.
3. V **skupiny projektu** dialogové okno, vyberte **přispěvatelé** skupiny a potom klikněte na tlačítko **vlastnosti**.

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. V **vlastnosti skupiny serveru Team Foundation** dialogu **Windows uživatele nebo skupinu**a potom klikněte na tlačítko **přidat**.

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. V **vybrat uživatele, počítače nebo skupiny** dialogové okno, zadejte uživatelské jméno uživatele, které chcete přidat do týmového projektu, klikněte na **Kontrola názvů**a potom klikněte na tlačítko **OK**.

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. V **vlastnosti skupiny serveru Team Foundation** dialogové okno, klikněte na tlačítko **OK**.
7. V **skupiny projektu** dialogové okno, klikněte na tlačítko **Zavřít**.

## <a name="conclusion"></a>Závěr

V tomto okamžiku je připravený k použití nového týmového projektu, a tým vývojářů může začít přidávání obsahu a spolupráci na proces vývoje.

Dalším tématu s názvem [přidávání obsahu do správy zdrojových kódů](adding-content-to-source-control.md), popisuje, jak přidat obsah do správy zdrojového kódu.

## <a name="further-reading"></a>Další čtení

Širší pokyny k vytvoření týmových projektů v TFS najdete v tématu [vytvořit týmový projekt](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx). Další informace o povolení uživatelům vytvářet nové týmové projekty v kolekci týmových projektů, naleznete v tématu [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx). Další informace o přidávání uživatelů do týmových projektů, naleznete v tématu [Add Users to Team Projects](https://msdn.microsoft.com/library/bb558971.aspx).

> [!div class="step-by-step"]
> [Předchozí](configuring-team-foundation-server-for-web-deployment.md)
> [další](adding-content-to-source-control.md)
