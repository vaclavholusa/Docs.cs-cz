---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: Nasazení konkrétního sestavení | Dokumentace Microsoftu
author: jrjlee
description: Toto téma popisuje postup nasazení webových balíčků a databázové skripty z konkrétního předchozí sestavení na nové umístění, jako je pracovní nebo produkční enviro...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: 6d55497dbc13133aa9c8b8eaecca0f6915fd9ed0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388221"
---
<a name="deploying-a-specific-build"></a>Nasazení konkrétního sestavení
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje postup nasazení webových balíčků a databázové skripty z konkrétního předchozí sestavení na nové umístění, jako jsou testovací nebo produkční prostředí.


Toto téma je součástí série kurzů podle požadavků na nasazení enterprise fiktivní společnosti s názvem společnosti Fabrikam, Inc. V této sérii kurzů používá ukázkové řešení&#x2014; [řešení Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s realistické úroveň složitosti, včetně aplikace ASP.NET MVC 3, komunikace Windows Služba Foundation (WCF) a databázový projekt.

Metody nasazení v srdci těchto kurzů je založen na rozdělení přístupu soubor projektu je popsáno v [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve kterém je řízena procesem sestavení a nasazení dva soubory projektu&#x2014;jeden obsahuje pokyny pro sestavení, které platí pro všechny cílové prostředí a jeden obsahuje nastavení pro konkrétní prostředí sestavení a nasazení. V okamžiku sestavení souboru projektu specifických pro prostředí se sloučí do souboru projektu bez ohledu na prostředí a vytvoří kompletní sadu pokynů sestavení.

## <a name="task-overview"></a>Přehled úloh

Až doteď témata v této sérii kurzů zaměřené na tom, jak sestavit, balení a nasazení webových aplikací a databází v rámci jednoho kroku nebo automatizovat proces. Ale v některé běžné scénáře, třeba vyberte prostředky, které nasadíte ze seznamu sestavení v odkládací složce. Jinými slovy nejnovějšího buildu nemusí být sestavení, které chcete nasadit.

Vezměte v úvahu scénář kontinuální integrace (CI) je popsáno v předchozím tématu [vytvoření sestavení definice, že podporuje nasazení](creating-a-build-definition-that-supports-deployment.md). Vytvořili jste definici sestavení v Team Foundation Server (TFS) 2010. Pokaždé, když vývojář kontroluje kód do TFS, Team Build se vytváření kódu, vytváření balíčků pro webové a databázové skripty jako součást procesu sestavení, spuštění všech testů jednotek a nasadit prostředky do testovacího prostředí. V závislosti na zásady uchovávání informací, které jste nakonfigurovali při vytváření definice sestavení si zachovají TFS určitého počtu předchozích sestavení.

![](deploying-a-specific-build/_static/image1.png)

Nyní předpokládejme, že jste provedli ověření a ověřovací testování s některou z těchto sestavení v testovacím prostředí a jste připraveni nasadit aplikaci do přípravného prostředí. Do té doby vývojáři mohou se změnami nový kód. Nechcete, aby znovu sestavte řešení a nasazení do přípravného prostředí a nechcete, aby k nasazení na nejnovější verzi do přípravného prostředí. Místo toho kterou chcete nasadit konkrétní sestavení, které jste ověření a ověření na testovacích serverech.

K tomu budete muset zjistit Microsoft Build Engine (MSBuild), kde najdete balíčky webové a databázové skripty, které generují konkrétního sestavení.

## <a name="overriding-the-outputroot-property"></a>Přepisování vlastnosti OutputRoot

V [ukázkové řešení](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), *Publish.proj* souboru deklaruje vlastnost s názvem **OutputRoot**. Jak název napovídá, to je kořenová složka, která obsahuje všechno, co se generuje procesu sestavení. V *Publish.proj* souboru, můžete zobrazit, který **OutputRoot** vlastnost odkazuje na kořenový adresář pro všechny prostředky pro nasazení.

> [!NOTE]
> **OutputRoot** je běžně používaný vlastnost název. Soubory Visual C# a Visual Basic projektu tuto vlastnost k uložení umístění kořenového adresáře pro všechna výstupní sestavení také deklarovat.


[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]


Pokud chcete, aby váš soubor projektu pro nasazení webových balíčků a databázové skripty z jiného umístění&#x2014;výstupy předchozích sestavení TFS, jako jsou&#x2014;jednoduše je potřeba přepsat **OutputRoot** vlastnost. Do složky relevantní sestavení byste měli nastavit hodnotu vlastnosti na serveru Team Build. Pokud jste používali nástroj MSBuild z příkazového řádku, můžete zadat hodnotu pro **OutputRoot** jako argument příkazového řádku:


[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]


V praxi, ale také chcete přeskočit **sestavení** cílové&#x2014;neexistuje bod v sestavení vašeho řešení, pokud nemáte v úmyslu použít výstupy sestavení. Může to provedete tak, že určíte cíle, které chcete spustit z příkazového řádku:


[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]


Ale ve většině případů budete chtít vytvořit svoji logiku nasazení do definice sestavení TFS. To umožňuje uživatelům s **zařazovat sestavení do fronty** oprávnění k aktivaci nasazení z jakékoli instalaci sady Visual Studio s připojením k serveru TFS.

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a>Vytvoření definice sestavení pro nasazení konkrétní sestavení

Následující postup popisuje, jak vytvořit definici sestavení, která umožňuje uživatelům aktivační události nasazení do přípravného prostředí pomocí jediného příkazu.

V takovém případě nechcete, aby definice sestavení nic sestavit&#x2014;je vhodné ji ke spuštění v souboru projektu vlastní logiku nasazení. *Publish.proj* soubor obsahuje podmíněnou logiku, který přeskočí **sestavení** cílit, pokud soubor běží ve službě Team Build. Dělá to tak, že vyhodnocení vestavěných **BuildingInTeamBuild** vlastnost, která se automaticky nastaví na **true** při spuštění souboru projektu v Team Build. V důsledku toho můžete přeskočit procesu sestavení a spusťte soubor projektu a nasazení existujícího sestavení.

**Chcete-li vytvořit definici sestavení k ruční aktivaci nasazení**

1. V sadě Visual Studio 2010 v **Team Exploreru** okna, rozbalte uzel týmového projektu, klikněte pravým tlačítkem na **sestavení**a potom klikněte na tlačítko **nová definice sestavení**.

    ![](deploying-a-specific-build/_static/image2.png)
2. Na **Obecné** kartu, zadejte název definice sestavení (například **DeployToStaging**) a volitelně také popis.
3. Na **aktivační událost** kartu, vyberte možnost **manuálně – vrácení se změnami nespouštějí nového sestavení**.
4. Na **výchozí hodnoty sestavení** kartě **Kopírovat výstup sestavení na následující odkládací složky** zadejte cestu (Universal Naming Convention) odkládací složky (například  **\\TFSBUILD\Drops**).

    ![](deploying-a-specific-build/_static/image3.png)
5. Na **procesu** kartě **soubor procesu sestavení** rozevíracího seznamu, ponechejte tuto položku **DefaultTemplate.xaml** vybrané. Toto je jedna z výchozích šablon procesů sestavení, které se přidají do všech nových týmových projektů.
6. V **parametry procesu sestavení** tabulku, klikněte na tlačítko v **položky k sestavení** řádku a potom klikněte na tlačítko **tlačítko se třemi tečkami** tlačítko.

    ![](deploying-a-specific-build/_static/image4.png)
7. V **položky k sestavení** dialogové okno, klikněte na tlačítko **přidat**.
8. V **položky typu** rozevíracího seznamu vyberte **soubory projektu MSBuild**.
9. Přejděte do umístění souboru vlastních projektů, pomocí kterého řízení procesu nasazení, vyberte ho a pak klikněte na tlačítko **OK**.

    ![](deploying-a-specific-build/_static/image5.png)
10. V **položky k sestavení** dialogové okno, klikněte na tlačítko **OK**.
11. V **parametry procesu sestavení** tabulky, rozbalte **Upřesnit** oddílu.
12. V **argumenty nástroje MSBuild** řádek, zadejte umístění vašeho souboru projektu pro konkrétní prostředí a přidáte zástupný symbol pro umístění složky sestavení:

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > Je potřeba přepsat **OutputRoot** hodnotu pokaždé, když se zařadit sestavení do fronty. Je to popsané v dalším postupu.
13. Klikněte na tlačítko **Uložit**.

Po aktivaci sestavení, je potřeba aktualizovat **OutputRoot** vlastnost tak, aby odkazoval na sestavení, které chcete nasadit.

**K nasazení konkrétního sestavení z definice sestavení**

1. V **Team Exploreru** okna, klikněte pravým tlačítkem na definici sestavení a pak klikněte na tlačítko **zařadit nové sestavení**.

    ![](deploying-a-specific-build/_static/image7.png)
2. V **zařadit sestavení do fronty** dialogovém okně **parametry** kartu, rozbalte **Upřesnit** oddílu.
3. V **argumenty nástroje MSBuild** řádek, nahraďte hodnotu **OutputRoot** vlastnost umístění složky sestavení. Příklad:

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > Nezapomeňte zahrnout adresy koncové lomítko na konci cesty ke složce sestavení.
4. Klikněte na tlačítko **fronty**.

Při vložení sestavení do fronty, soubor projektu bude nasazení databázové skripty a webových balíčků ze složky pro přetažení sestavení, kterou jste zadali v **OutputRoot** vlastnost.

## <a name="conclusion"></a>Závěr

Toto téma popisuje, jak chcete publikovat nasazení prostředky, jako jsou webové balíčky a skripty databáze, z konkrétní předchozí sestavení pomocí modelu nasazení souboru projektu rozdělení. Vysvětlení postupu přepsání **OutputRoot** definice sestavení pro vlastnost a tom, jak začlenit logiky nasazení do TFS.

## <a name="further-reading"></a>Další čtení

Další informace o vytvoření definice sestavení, naleznete v tématu [vytvořit a Basic Build Definition](https://msdn.microsoft.com/library/ms181716.aspx) a [definovat svůj proces sestavení](https://msdn.microsoft.com/library/ms181715.aspx). Další pokyny k řazení sestavení do fronty, naleznete v tématu [zařadit sestavení do fronty](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Předchozí](creating-a-build-definition-that-supports-deployment.md)
> [další](configuring-permissions-for-team-build-deployment.md)
