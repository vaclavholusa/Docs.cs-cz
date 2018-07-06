---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
title: Vytvoření definice sestavení, která podporuje nasazení | Dokumentace Microsoftu
author: jrjlee
description: Pokud chcete provádět jakékoliv sestavení v Team Foundation Server (TFS) 2010, musíte vytvořit definici sestavení v rámci týmového projektu. Toto téma des...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: fe47a018-f6d0-4979-80e7-5b1fa75a5865
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
msc.type: authoredcontent
ms.openlocfilehash: 18f88cff032bd0694ef98f0b19849f0edf1681ee
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827385"
---
<a name="creating-a-build-definition-that-supports-deployment"></a>Vytvoření definice sestavení, která podporuje nasazení
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Pokud chcete provádět jakékoliv sestavení v Team Foundation Server (TFS) 2010, musíte vytvořit definici sestavení v rámci týmového projektu. Toto téma popisuje postup vytvoření nové definice sestavení v TFS a řízení nasazení webu jako součást procesu sestavení v Team Build.


Toto téma je součástí série kurzů podle požadavků na nasazení enterprise fiktivní společnosti s názvem společnosti Fabrikam, Inc. V této sérii kurzů používá ukázkové řešení&#x2014; [řešení Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s realistické úroveň složitosti, včetně aplikace ASP.NET MVC 3, komunikace Windows Služba Foundation (WCF) a databázový projekt.

Metody nasazení v srdci těchto kurzů je založen na rozdělení přístupu soubor projektu je popsáno v [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve kterém je řízena procesem sestavení a nasazení dva soubory projektu&#x2014;jeden obsahuje pokyny pro sestavení, které platí pro všechny cílové prostředí a jeden obsahuje nastavení pro konkrétní prostředí sestavení a nasazení. V okamžiku sestavení souboru projektu specifických pro prostředí se sloučí do souboru projektu bez ohledu na prostředí a vytvoří kompletní sadu pokynů sestavení.

## <a name="task-overview"></a>Přehled úloh

Definice sestavení je mechanismus, který řídí, jak a kdy dojde k sestavení pro týmové projekty v sadě TFS. Každá definice sestavení určuje:

- Věci, které mají být sestaveny jako soubory řešení sady Visual Studio nebo vlastních souborů projektu Microsoft Build Engine (MSBuild).
- Kritéria určující, když by neměl zabrat sestavení umístit, jako je ruční triggery, kontinuální integrace (CI), nebo hlídané vrácení se změnami se.
- Umístění, ke kterému má Team Build odeslat výstupy sestavení, včetně nasazení artefakty stejně jako webových balíčků a databázové skripty.
- Množství času, který by měl být zachován každého sestavení.
- Různé další parametry procesu sestavení.

> [!NOTE]
> Další informace o definicích sestavení naleznete v tématu [definovat svůj proces sestavení](https://msdn.microsoft.com/library/ms181715.aspx).


Toto téma se ukazují, jak vytvořit definici sestavení, která používá CI, tak, aby sestavení se aktivuje, když vývojář vrátí nový obsah. Pokud byl sestaven úspěšně, sestavovací služba spustí soubor vlastní projekt k nasazení řešení do testovacího prostředí.

Po aktivaci sestavení, třeba dojít tyto akce:

- Nejprve Team Build má sestavit řešení. V rámci tohoto procesu se vyvolá Team Build Web publikování kanálu (WPP) ke generování balíčky nasazení webu pro jednotlivé projekty webových aplikací v řešení. Sestavení týmu také spustí všechny testy jednotek, které jsou přidružené k řešení.
- Pokud se nezdaří sestavení řešení, sestavení týmu by neměl zabrat žádná další akce. Selhání testu jednotky by měl být považován za selhání sestavení.
- Pokud bude úspěšné sestavení řešení, by měl Team Build spusťte soubor vlastní projekt, který řídí nasazení řešení. Jako součást tohoto procesu Team Build se vyvolá Internetové informační služby (IIS) nástroj pro nasazení webu (nasazení webu) k instalaci zabalené webové aplikace na cílové webové servery a vyvolá nástroj VSDBCMD.exe ke spuštění vytvoření databáze skripty na cílových serverech databáze.

To je znázorněn proces:

![](creating-a-build-definition-that-supports-deployment/_static/image1.png)

[Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) ukázkové řešení obsahuje vlastní soubor projektu MSBuild *Publish.proj*, který můžete spustit z nástroje MSBuild nebo Team Build. Jak je popsáno v [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md), tento soubor projektu definuje logiku, která nasadí webových balíčků a databáze do cílové prostředí. Soubor obsahuje logiku, která vynechává sestavení a procesem vytváření balíčku, pokud běží ve službě Team Build, byste museli opustit jenom nasazení úlohy pro spuštění. Toto je vzhledem k tomu, že při automatizaci nasazení tímto způsobem je obvykle vhodné zajistit, že řešení je sestaveno úspěšně a předává všechny testy jednotek, před zahájením procesu nasazení.

Další části je vysvětlen postup implementace tohoto procesu tak, že vytvoříte novou definici sestavení.

> [!NOTE]
> Tento postup&#x2014;ve kterém automatizované jeden proces sestavení, testy a nasadí řešení&#x2014;by mohla být nejvhodnější pro nasazení pro testovací prostředí. Pro testovací a produkční prostředí budete pravděpodobně mnohem více chcete nasadit obsah z předchozího buildu, který jste již ověření a ověření v testovacím prostředí. Tento postup je popsaný v dalším tématu [nasazení konkrétního sestavení](deploying-a-specific-build.md).


### <a name="who-performs-this-procedure"></a>Kdo provádí tento postup?

Obvykle provádí správce serveru TFS tento postup. V některých případech může vedoucí týmu pro vývojáře nést odpovědnost za kolekci týmových projektů v TFS. Chcete-li vytvořit novou definici sestavení, musíte být členem skupiny **správci sestavení kolekcí projektů** pro kolekci týmového projektu, který obsahuje vaše řešení.

## <a name="create-a-build-definition-for-ci-and-deployment"></a>Vytvořte definici sestavení CI a nasazení

Následující postup popisuje, jak vytvořit definici sestavení, která aktivuje CI. Pokud byl sestaven úspěšně, je řešení nasadit s použitím logiku vlastního souboru projektu MSBuild.

**Chcete-li vytvořit definici sestavení CI a nasazení**

1. V sadě Visual Studio 2010 v **Team Exploreru** okna, rozbalte uzel týmového projektu, klikněte pravým tlačítkem na **sestavení**a potom klikněte na tlačítko **nová definice sestavení**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image2.png)
2. Na **Obecné** kartu, zadejte název definice sestavení (například **DeployToTest**) a volitelně také popis.
3. Na **aktivační událost** kartu, vyberte kritéria, na kterých chcete aktivovat nový build. Pokud například chcete řešení sestavit a nasadit do testovacího prostředí, pokaždé, když vývojář vrátí nový kód, vyberte **kontinuální integrace**.
4. Na **výchozí hodnoty sestavení** kartě **Kopírovat výstup sestavení na následující odkládací složky** zadejte cestu (Universal Naming Convention) odkládací složky (například  **\\TFSBUILD\Drops**).

    ![](creating-a-build-definition-that-supports-deployment/_static/image3.png)

    > [!NOTE]
    > Toto umístění přetažení ukládá několik sestavení, v závislosti na vámi nakonfigurovaných zásad uchovávání informací. Když chcete publikovat artefakty nasazení z konkrétního sestavení pro testovací nebo produkční prostředí, je to, kde najdete ho.
5. Na **procesu** kartě **soubor procesu sestavení** rozevíracího seznamu, ponechejte tuto položku **DefaultTemplate.xaml** vybrané. Toto je jedna z výchozích šablon procesů sestavení, které se přidají do všech nových týmových projektů.
6. V **parametry procesu sestavení** tabulku, klikněte na tlačítko v **položky k sestavení** řádku a potom klikněte na tlačítko **tlačítko se třemi tečkami** tlačítko.

    ![](creating-a-build-definition-that-supports-deployment/_static/image4.png)
7. V **položky k sestavení** dialogové okno, klikněte na tlačítko **přidat**.
8. Přejděte do umístění souboru řešení a potom klikněte na tlačítko **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image5.png)
9. V **položky k sestavení** dialogové okno, klikněte na tlačítko **přidat**.
10. V **položky typu** rozevíracího seznamu vyberte **soubory projektu MSBuild**.
11. Přejděte do umístění souboru vlastních projektů, pomocí kterého řízení procesu nasazení, vyberte ho a pak klikněte na tlačítko **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image6.png)
12. **Položky k sestavení** dialogové okno by teď zobrazují dvě položky. Klikněte na tlačítko **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image7.png)
13. Na **procesu** kartě **parametry procesu sestavení** tabulky, rozbalte **Upřesnit** oddílu.
14. V **argumenty nástroje MSBuild** řádek, přidejte jakékoli argumenty příkazového řádku MSBuild, který *buď* položek k sestavení vyžaduje. Scénář řešení Správce kontaktů tyto argumenty jsou povinné:

    [!code-console[Main](creating-a-build-definition-that-supports-deployment/samples/sample1.cmd)]

    ![](creating-a-build-definition-that-supports-deployment/_static/image8.png)
15. V tomto příkladu:

    1. **DeployOnBuild = true** a **DeployTarget = balíček** argumenty jsou povinné, když vytvoříte řešení Správce kontaktů. Toto nastaví nástroj MSBuild pro vytváření webových balíčky pro nasazení po sestavení projektu každé webové aplikace, jak je popsáno v [sestavení a balení projektů webových aplikací](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).
    2. **TargetEnvPropsFile** argument je povinný při vytváření *Publish.proj* souboru. Tato vlastnost určuje umístění souboru konfigurace pro konkrétní prostředí, jak je popsáno v [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md).
16. Na **zásady uchovávání informací** kartu, nakonfigurujte, kolik sestavení každého typu, které chcete zachovat podle potřeby.
17. Klikněte na tlačítko **Uložit**.

## <a name="queue-a-build"></a>Zařazení sestavení do fronty

V tomto okamžiku jste vytvořili aspoň jednu novou definici sestavení. Proces sestavení, které jste definovali se teď spustí podle aktivačních událostí, které jste zadali v definici sestavení.

Pokud jste nakonfigurovali definici sestavení CI používat, můžete otestovat definici sestavení dvěma způsoby:

- Vrátit se změnami nějaký obsah do týmového projektu k aktivaci automatického sestavení.
- Zařadit sestavení do fronty ručně.

**Sestavení do fronty ručně**

1. V **Team Exploreru** okna, klikněte pravým tlačítkem na definici sestavení a pak klikněte na tlačítko **zařadit nové sestavení**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image9.png)
2. V **zařadit sestavení do fronty** dialogové okno, zkontrolujte vlastnosti sestavení a pak klikněte na tlačítko **fronty**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image10.png)

Chcete zkontrolovat průběh a výsledky sestavení&#x2014;bez ohledu na to, zda byla spuštěna ručně nebo automaticky&#x2014;dvakrát na definici sestavení v **Team Exploreru** okna. Tím se otevře **Průzkumník sestavení** kartu.

![](creating-a-build-definition-that-supports-deployment/_static/image11.png)

Zde můžete řešit selhání sestavení. Pokud dvakrát kliknete na jednotlivý build, můžete zobrazit souhrnné informace a proklikejte se k podrobné soubory protokolů.

![](creating-a-build-definition-that-supports-deployment/_static/image12.png)

Tyto informace slouží k řešení selhání sestavení a řešit jakékoli problémy, než se pokusíte jiné sestavení.

> [!NOTE]
> Sestavení, které jsou spouštěny logiky nasazení budou pravděpodobně fungovat, dokud jste udělili server sestavení všechna oprávnění nutná v cílovém prostředí. Další informace najdete v tématu [konfigurace oprávnění pro nasazení sestavení týmu](configuring-permissions-for-team-build-deployment.md).


## <a name="monitor-the-build-process"></a>Monitorování procesu sestavení

TFS poskytuje širokou škálu funkcí, které vám pomohou monitorovat proces sestavení. TFS můžete například odeslat e-mailu nebo zobrazení výstrah v oznamovací oblasti hlavního panelu po dokončení sestavení. Další informace najdete v tématu [spuštění a monitorování sestavení](https://msdn.microsoft.com/library/ms181721.aspx).

## <a name="conclusion"></a>Závěr

Toto téma popisuje, jak vytvořit definici sestavení v sadě TFS. Definice sestavení je nakonfigurovaný pro nepřetržitou Integraci, aby proces sestavení poběží pokaždé, když vývojář vrátí obsah do týmového projektu. Definici sestavení spustí vlastní soubor projektu MSBuild k nasazení webových balíčků a databázové skripty do cílového serveru prostředí.

V pořadí pro automatické nasazení proběhla úspěšně jako součást procesu sestavení budete muset udělit příslušná oprávnění k účtu sestavovací služby na cílové webové servery a databáze cílový server. V posledním tématu v tomto kurzu se [konfigurace oprávnění pro nasazení sestavení týmu](configuring-permissions-for-team-build-deployment.md), popisuje, jak identifikovat a nakonfigurovat oprávnění požadovaná pro automatické nasazení ze serveru Team Build.

## <a name="further-reading"></a>Další čtení

Další informace o vytvoření definice sestavení, naleznete v tématu [vytvořit a Basic Build Definition](https://msdn.microsoft.com/library/ms181716.aspx) a [definovat svůj proces sestavení](https://msdn.microsoft.com/library/ms181715.aspx). Další pokyny k řazení sestavení do fronty, naleznete v tématu [zařadit sestavení do fronty](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Předchozí](configuring-a-tfs-build-server-for-web-deployment.md)
> [další](deploying-a-specific-build.md)
