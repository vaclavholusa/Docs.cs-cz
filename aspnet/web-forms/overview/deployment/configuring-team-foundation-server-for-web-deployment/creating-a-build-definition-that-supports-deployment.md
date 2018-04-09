---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
title: Vytvoření definice sestavení, který podporuje nasazení | Microsoft Docs
author: jrjlee
description: Pokud chcete provádět jakékoliv sestavení v Team Foundation Server (TFS) 2010, musíte vytvořit definici sestavení v týmových projektech. Toto téma des...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: fe47a018-f6d0-4979-80e7-5b1fa75a5865
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
msc.type: authoredcontent
ms.openlocfilehash: c5ea0bd9f01bb57b96abd349741f304c0093d887
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-build-definition-that-supports-deployment"></a>Vytvoření definice sestavení, který podporuje nasazení
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Pokud chcete provádět jakékoliv sestavení v Team Foundation Server (TFS) 2010, musíte vytvořit definici sestavení v týmových projektech. Toto téma popisuje, jak vytvořit novou definici sestavení v sadě TFS a postup řízení nasazení webu jako součást procesu sestavení v sestavení Team.


Toto téma je součástí ze série kurzů na základě kolem podnikové požadavky nasazení fiktivní společnost s názvem Fabrikam, Inc. Tento kurz řady používá ukázkové řešení&#x2014; [řešení obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s úrovní realistické složitější, včetně aplikace ASP.NET MVC 3, komunikaci Windows Služba Foundation (WCF) a projekt databáze.

Metoda nasazení jádrem tyto kurzy je založena na popsaný přístup souboru projektu rozdělení [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve kterém je řízena procesem sestavení a nasazení dva soubory projektu&#x2014;jeden obsahující sestavení pokyny, které platí pro každé cílové prostředí a jeden, který obsahuje nastavení pro konkrétní prostředí sestavení a nasazení. V okamžiku sestavení souboru projektu konkrétní prostředí sloučeny do souboru projektu bez ohledu na prostředí a vytvořit úplnou sadu pokynů sestavení.

## <a name="task-overview"></a>Přehled úloh

Definice sestavení je mechanismus, který řídí, jak a kdy dojde k sestavení pro týmové projekty v sadě TFS. Určuje, každý definici sestavení:

- Věcí, kterou chcete vytvořit, jako jsou soubory řešení sady Visual Studio nebo vlastní soubory projektu Microsoft Build Engine (MSBuild).
- Kritéria, která určení, kdy by neměl zabrat sestavení umístit jako ruční, aktivační události průběžnou integraci (CI), nebo ověřované vrácení se změnami.
- Umístění, na který Team Build posílat výstupy sestavení, včetně nasazení artefakty jako webových balíčků a databázové skripty.
- Množství času, který by měl být zachován každé sestavení.
- Různé další parametry procesu sestavení.

> [!NOTE]
> Další informace o definice sestavení, najdete v části [definovat proces vaše sestavení](https://msdn.microsoft.com/library/ms181715.aspx).


Toto téma vám ukáže, jak vytvořit definici sestavení, která používá CI, takže sestavení se aktivuje, když vývojář kontroluje nového obsahu. Pokud sestavení úspěšné, spustí službu sestavení vlastní projektu soubor, který chcete nasadit řešení do testovacího prostředí.

Když spustíte sestavení, musí dojít tyto akce:

- Sestavení Team nejdřív měli sestavte řešení. V rámci tohoto procesu bude Team Build vyvolání webové publikování kanálu (WPP) ke generování balíčky nasazení webu pro každou z webové aplikace projekty v řešení. Sestavení Team také spustí všechny testy jednotek, které jsou přidružené k řešení.
- V případě selhání sestavení řešení Team Build provést žádné další akce. Selhání při testu jednotky zacházeno jako selhání sestavení.
- Pokud sestavení řešení úspěšné, Team Build spuštěním souboru vlastní projektu, který řídí nasazení řešení. V rámci tohoto procesu se Team Build vyvolá nástroj nasazení webu Internetové informační služby (IIS) (Web Deploy) k instalaci zabalené webových aplikací na cílové webové servery a vyvolá nástroj VSDBCMD.exe ke spuštění vytvoření databáze skripty na cílových serverech databáze.

To je znázorněn proces:

![](creating-a-build-definition-that-supports-deployment/_static/image1.png)

[Obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) ukázkové řešení obsahuje vlastní soubor projektu nástroje MSBuild *Publish.proj*, který můžete spustit z nástroje MSBuild nebo Team Build. Jak je popsáno v [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md), tento projektový soubor definuje logiku, která nasadí webových balíčků a databáze do cílové prostředí. Soubor obsahuje logiku, která vynechá proces balení a sestavení, pokud je spuštěn v sestavení Team ponechat právě úlohy nasazení spustit. Toto je vzhledem k tomu, že při automatizaci nasazení tímto způsobem, budete obvykle chcete zajistit úspěšně sestavení řešení a předává všechny testy jednotek před zahájením procesu nasazení.

V další části vysvětluje, jak implementovat tento proces vytvořením nové definice buildu.

> [!NOTE]
> Tento postup&#x2014;v které jedné automatizované procesu sestavení, testuje a nasadí řešení&#x2014;může být nejvhodnější pro nasazení na testovací prostředí. Pro pracovní a provozní prostředí budete mnohem víc pravděpodobně chcete nasadit obsah z předchozího sestavení, který jste již ověřit a ověřit v testovacím prostředí. Tento postup je popsaný v dalším tématu [nasazení konkrétní sestavení](deploying-a-specific-build.md).


### <a name="who-performs-this-procedure"></a>Kdo provádí tento postup?

Obvykle provádí správce sady TFS tento postup. V některých případech mohou vedoucím týmu vývojáře převzít odpovědnost za kolekce týmového projektu v sadě TFS. Chcete-li vytvořit nové definice buildu, musíte být členem skupiny **správci sestavení projektu kolekce** pro kolekce týmového projektu, který obsahuje vaše řešení.

## <a name="create-a-build-definition-for-ci-and-deployment"></a>Vytvořit definici sestavení pro položky konfigurace a nasazení

Následující postup popisuje, jak vytvořit definici sestavení, která aktivuje položek konfigurace. Pokud sestavení úspěšné, je řešení nasazeno v vlastní soubor projektu nástroje MSBuild logiku.

**K vytvoření definice sestavení pro položky konfigurace a nasazení**

1. V sadě Visual Studio 2010 v **Team Explorer** okno, rozbalte uzel vaší týmového projektu, klikněte pravým tlačítkem na **sestavení**a potom klikněte na **novou definici sestavení**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image2.png)
2. Na **Obecné** kartě, pojmenujte definici sestavení (například **DeployToTest**) a volitelný popis.
3. Na **aktivační událost** , vyberte kritéria, na které chcete aktivovat nové sestavení. Například pokud chcete nasadit do testovacího prostředí pokaždé, když vývojář změnami nový kód a sestavte řešení, vyberte **průběžnou integraci**.
4. Na **sestavení výchozí** ve **kopie sestavení výstup do následující složky, vyřaďte** pole, zadejte cestu Universal Naming Convention (UNC) vaší složky (například  **\\TFSBUILD\Drops**).

    ![](creating-a-build-definition-that-supports-deployment/_static/image3.png)

    > [!NOTE]
    > Toto umístění rozevírací ukládá několik sestavení, v závislosti na tom, které konfigurujete zásady uchovávání informací. Když chcete publikovat nasazení artefaktů z konkrétní sestavení a pracovním nebo produkčním prostředí, je to, kde je můžete nalézt.
5. Na **proces** ve **souboru procesu sestavení** rozevíracího seznamu, ponechejte **DefaultTemplate.xaml** vybrané. Toto je jedna z výchozích šablon procesu sestavení, které se přidají do všechny nové týmových projektů.
6. V **parametry procesu sestavení** tabulky, klikněte na tlačítko v **položky k sestavení** řádek a klikněte **třemi tečkami** tlačítko.

    ![](creating-a-build-definition-that-supports-deployment/_static/image4.png)
7. V **položky k sestavení** dialogové okno, klikněte na tlačítko **přidat**.
8. Přejděte do umístění souboru řešení a pak klikněte na **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image5.png)
9. V **položky k sestavení** dialogové okno, klikněte na tlačítko **přidat**.
10. V **položky typu** rozevíracího seznamu vyberte **soubory projektu nástroje MSBuild**.
11. Přejděte do umístění souboru vlastní projektu, se kterým řízení procesu nasazení, vyberte soubor a pak klikněte na tlačítko **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image6.png)
12. **Položky k sestavení** dialogové okno byste nyní měli vidět dvě položky. Click **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image7.png)
13. Na **proces** ve **parametry procesu sestavení** tabulky, rozbalte **Upřesnit** části.
14. V **argumenty MSBuild** řádek, přidejte žádných argumentů příkazového řádku nástroje MSBuild, *buď* položky k sestavení vyžaduje. Scénář řešení, obraťte se na správce tyto argumenty jsou povinné:

    [!code-console[Main](creating-a-build-definition-that-supports-deployment/samples/sample1.cmd)]

    ![](creating-a-build-definition-that-supports-deployment/_static/image8.png)
15. V tomto příkladu:

    1. **DeployOnBuild = true** a **DeployTarget = balíček** argumenty jsou povinné, při sestavování řešení obraťte se na správce. Tím se nastaví MSBuild k vytvoření webové balíčky pro nasazení po sestavení každý projekt webové aplikace, jak je popsáno v [budova a projekty webových aplikací balení](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).
    2. **TargetEnvPropsFile** se vyžaduje při sestavování *Publish.proj* souboru. Tato vlastnost určuje umístění konkrétní prostředí konfiguračního souboru, jak je popsáno v [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md).
16. Na **zásady uchovávání informací** nakonfigurujte, kolik sestavení jednotlivých typů, které chcete uchovávat podle potřeby.
17. Klikněte na tlačítko **Uložit**.

## <a name="queue-a-build"></a>Zařazení sestavení do fronty

V tomto okamžiku jste vytvořili alespoň jednu definici nové sestavení. Proces sestavení, který jste definovali se teď spustí podle aktivačních událostí, který jste zadali v definici sestavení.

Pokud jste nakonfigurovali vaší definice sestavení používat položek konfigurace, můžete testovat svou definici sestavení dvěma způsoby:

- Zkontrolujte v některé obsah do týmového projektu pro aktivaci automatické sestavení.
- Fronty sestavení ručně.

**Do fronty sestavení ručně**

1. V **Team Explorer** okno, klikněte pravým tlačítkem na definici sestavení a pak klikněte na tlačítko **fronty nové sestavení**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image9.png)
2. V **fronty sestavení** dialogové okno, zkontrolujte vlastnosti sestavení a pak klikněte na tlačítko **fronty**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image10.png)

Chcete-li zkontrolovat průběh a výsledek sestavení&#x2014;bez ohledu na to, zda byla aktivována ručně nebo automaticky&#x2014;dvakrát klikněte na definici sestavení v **Team Explorer** okno. Tím se otevře **Průzkumník sestavení** kartě.

![](creating-a-build-definition-that-supports-deployment/_static/image11.png)

Zde můžete řešit sestavení se nezdařilo. Pokud dvakrát kliknete na jednotlivé sestavení, můžete zobrazit souhrnné informace a klikněte na tlačítko prostřednictvím podrobné souborů protokolu.

![](creating-a-build-definition-that-supports-deployment/_static/image12.png)

Tyto informace slouží k řešení neúspěšných sestavení a vyřešte všechny problémy před dalším pokusem jiné sestavení.

> [!NOTE]
> Sestavení, které jsou spouštěny logiku nasazení budou pravděpodobně fungovat, dokud nebude server sestavení jste povolili všechny oprávněních v cílovém prostředí. Další informace najdete v tématu [konfigurace oprávnění pro nasazení sestavení Team](configuring-permissions-for-team-build-deployment.md).


## <a name="monitor-the-build-process"></a>Monitorování procesu sestavení

TFS poskytuje širokou škálou funkcí, které vám pomohou při sledování procesu sestavení. Například můžete TFS e-mailem nebo zobrazení výstrah v oznamovací oblasti hlavního panelu systému po dokončení sestavení. Další informace najdete v tématu [spuštění a monitorování vytvoří](https://msdn.microsoft.com/library/ms181721.aspx).

## <a name="conclusion"></a>Závěr

Toto téma popisuje postup vytvoření definice sestavení v sadě TFS. Definici sestavení je nakonfigurovaný pro nepřetržitou Integraci, takže procesu sestavení běží vždy, když vývojář zkontroluje v obsahu k týmovému projektu. Definici sestavení provede vlastní soubor projektu nástroje MSBuild nasazení webových balíčků a databázové skripty prostředí cílového serveru.

Aby automatického nasazení na úspěšné jako součást procesu sestavení budete muset udělit příslušná oprávnění k účtu služby sestavení na cílové webové servery a cílový server databáze. Poslední téma v tomto kurzu [konfigurace oprávnění pro nasazení sestavení Team](configuring-permissions-for-team-build-deployment.md), popisuje, jak identifikovat a nakonfigurovat oprávnění vyžadovaná pro automatické nasazení ze serveru Team Build.

## <a name="further-reading"></a>Další čtení

Další informace o vytváření definic sestavení, najdete v části [vytvoření základní definice sestavení](https://msdn.microsoft.com/library/ms181716.aspx) a [definovat proces vaše sestavení](https://msdn.microsoft.com/library/ms181715.aspx). Další pokyny k sestavení služby Řízení front, najdete v části [fronty sestavení](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Předchozí](configuring-a-tfs-build-server-for-web-deployment.md)
> [další](deploying-a-specific-build.md)
