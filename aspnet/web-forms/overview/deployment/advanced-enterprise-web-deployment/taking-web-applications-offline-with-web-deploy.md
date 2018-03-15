---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
title: "Nasazení pořízení webových aplikací do offline režimu s Web | Microsoft Docs"
author: jrjlee
description: "Toto téma popisuje, jak provést webové aplikace do režimu offline po dobu trvání automatického nasazení pomocí nasazení webové služby Internetové informační služby (IIS)..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 3e9f6e7d-8967-4586-94d5-d3a122f12529
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
msc.type: authoredcontent
ms.openlocfilehash: 1c262ec7b834107524a18c6552b171f731452c91
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
<a name="taking-web-applications-offline-with-web-deploy"></a>Pořízení webových aplikací do offline režimu s Web nasazení
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje, jak vytvořit webovou aplikaci do režimu offline po dobu trvání automatického nasazení pomocí Internetové informační služby (IIS) nástroj pro nasazení webu, které je (Web Deploy). Uživatelé, kteří přejděte do webové aplikace se přesměrují do *aplikace\_offline.htm* souborů až do dokončení nasazení.


Toto téma je součástí ze série kurzů na základě kolem podnikové požadavky nasazení fiktivní společnost s názvem Fabrikam, Inc. Tento kurz series používá ukázkové řešení & #x 2014; [řešení obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; představující webovou aplikaci s úrovní realistické složitější, včetně aplikace ASP.NET MVC 3 systému Windows Komunikační služby Foundation (WCF) a projekt databáze.

Metoda nasazení jádrem tyto kurzy je založena na popsaný přístup souboru projektu rozdělení [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve které je řízené procesu sestavení dva projektu soubory & #x 2014; jeden obsahující sestavení pokyny, které platí pro každé cílové prostředí a jeden, který obsahuje nastavení pro konkrétní prostředí sestavení a nasazení. V okamžiku sestavení souboru projektu konkrétní prostředí sloučeny do souboru projektu bez ohledu na prostředí a vytvořit úplnou sadu pokynů sestavení.

## <a name="task-overview"></a>Přehled úloh

V celá řada možných scénářů budete chtít převést webové aplikace do režimu offline, když provedete změny související součásti, jako jsou databáze nebo webové služby. Obvykle ve službě IIS a ASP.NET, můžete dosáhnout umístit soubor s názvem *aplikace\_offline.htm* v kořenové složce web nebo webovou aplikaci služby IIS. *Aplikace\_offline.htm* soubor je standardní soubor HTML a bude obvykle obsahovat jednoduché zprávu vyzývající uživatele, že lokalita je dočasně nedostupná z důvodu údržby. Když *aplikace\_offline.htm* soubor existuje v kořenové složce webové stránky, služba IIS automaticky přesměruje všechny žádosti do souboru. Pokud po provedení aktualizace, odstraňte *aplikace\_offline.htm* souborové služby a webu obnoví obsluhovat požadavky jako obvykle.

Pokud použijete nasazení webu k provedení automatizované nebo krokování nasazení pro cílové prostředí, můžete začlenit přidávání a odebírání *aplikace\_offline.htm* souboru do procesu nasazení. K tomu budete potřebovat k dokončení těchto úloh vysoké úrovně:

- V souboru projektu Microsoft Build Engine (MSBuild), které umožňují řídit proces nasazení, vytvořit cíl MSBuild, která zkopíruje *aplikace\_offline.htm* souboru na cílovém serveru před všechny úlohy nasazení Začněte.
- Přidejte jiný MSBuild cíl, který odebere *aplikace\_offline.htm* souboru z cílového serveru po dokončení všech úloh nasazení.
- V projektu webové aplikace, vytvořte *. wpp.targets* soubor, který zajistí, že *aplikace\_offline.htm* souboru se přidá do balíčku nasazení při nasazení webu je volána.

Toto téma vám ukáže, jak k provedení těchto postupů. Úlohy a postupy v tomto tématu se předpokládá, že jste již vytvořili řešení, které obsahuje alespoň jeden projekt webové aplikace a použít soubor vlastní projektu k řízení procesu nasazení, jak je popsáno v [nasazení webu v Enterprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Alternativně můžete použít [obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) ukázkové řešení použít postup popsaný v příkladech v tomto tématu.

## <a name="adding-an-appoffline-file-to-a-web-application-project"></a>Přidání aplikace\_Offline souborů pro projekt webové aplikace

První úlohou, které potřebujete k dokončení je přidat *aplikace\_offline* souboru na projekt webové aplikace:

- Aby se zabránilo soubor zasahování procesu vývoje (nechcete, aby vaše aplikace trvale offline), byste měli zavolat ho něco jiné než *aplikace\_offline.htm*. Například může název souboru *aplikace\_offline template.htm*.
- Abyste zabránili nasazený jako soubor-je, měli byste nastavit akce sestavení na **žádné**.

**Chcete-li přidat aplikaci\_offline souborů pro projekt webové aplikace**

1. Otevřete řešení v sadě Visual Studio 2010.
2. V **Průzkumníku řešení** okna, klikněte pravým tlačítkem na projekt vaší webové aplikace, přejděte na **přidat**a potom klikněte na **novou položku**.
3. V **přidat novou položku** dialogové okno, vyberte **stránku HTML**.
4. V **název** zadejte **aplikace\_offline template.htm**a potom klikněte na **přidat**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image1.png)
5. Přidejte některé jednoduché HTML a informovat uživatele o tom, že aplikace je k dispozici a pak soubor uložte. Nezahrnují všechny serverové značky (například všechny značky, které mají předponu "asp:"). 

    ![](taking-web-applications-offline-with-web-deploy/_static/image2.png)
6. V **Průzkumníku řešení** oken, klikněte pravým tlačítkem na nový soubor a pak klikněte na tlačítko **vlastnosti**.
7. V **vlastnosti** okno v **akce sestavení** řádek, vyberte **žádné**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image3.png)

## <a name="deploying-and-deleting-an-appoffline-file"></a>Nasazení a odstraňování aplikace\_Offline souborů

Dalším krokem je upravit logika nasazení zkopírujte soubor na cílový server na začátku procesu nasazení a odeberte jej na konci.

> [!NOTE]
> V dalším postupu se předpokládá, že používáte vlastní soubor projektu nástroje MSBuild k řízení procesu vaše nasazení, jak je popsáno v [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Pokud nasazujete přímé ze sady Visual Studio, budete muset použít jiný přístup. SAYED Ibrahim Hashimi popisuje jeden takový přístup v [postup trvat vaše webové aplikace do režimu Offline během publikování](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx).


K nasazení *aplikace\_offline* soubor do cílového webu služby IIS, je nutné vyvolat MSDeploy.exe pomocí [nasazení webu **contentPath** zprostředkovatele](https://technet.microsoft.com/library/dd569034(WS.10).aspx). **ContentPath** poskytovatel podporuje fyzický adresář cesty a cesty k webu nebo aplikace služby IIS, takže je ideálním řešením pro synchronizaci souborů mezi složku projekt sady Visual Studio a webové aplikace služby IIS. K nasazení souboru, MSDeploy příkazu by měl vypadat přibližně takto:


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample1.cmd)]


Odeberte soubor z cílové lokality na konci procesu nasazení, MSDeploy příkazu by měl vypadat přibližně takto:


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample2.cmd)]


Chcete-li automatizovat tyto příkazy v rámci procesu sestavení a nasazení, je integrovat do vlastní soubor projektu nástroje MSBuild. Následující postup popisuje, jak na to.

**K nasazení a odstranit aplikaci\_offline souborů**

1. V sadě Visual Studio 2010 otevřete soubor projektu nástroje MSBuild, kterými se řídí váš proces nasazení. V [obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) ukázkové řešení, jde *Publish.proj* souboru.
2. V kořenovém **projektu** elementu, vytvořte novou **PropertyGroup** element k uložení proměnné pro *aplikace\_offline* nasazení:

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample3.xml)]
3. **SourceRoot** vlastnost je definována jinde v *Publish.proj* souboru. Označuje umístění kořenová složka pro zdrojový obsah vzhledem k aktuální cestě & #x 2014; jinými slovy, relativní k umístění *Publish.proj* souboru.
4. **ContentPath** zprostředkovatele nebude přijímat relativní cesty k souboru, takže potřebujete získat absolutní cestu k souboru zdroje, než bude možné nasadit. Můžete použít [converttoabsolutepath –](https://msdn.microsoft.com/library/bb882668.aspx) úloh k tomu.
5. Přidejte nový **cíl** element s názvem **GetAppOfflineAbsolutePath**. Uvnitř tohoto cíle, použijte **converttoabsolutepath –** na absolutní cestu k získání úlohy *aplikace\_offline šablony* soubor ve složce projektu.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample4.xml)]
6. Tento cíl má relativní cestu k *aplikace\_offline šablony* soubor ve složce projektu a uloží ji do nové vlastnosti jako absolutní cestu k souboru. **BeforeTargets** atribut určuje, že tento cíl ke spuštění před **DeployAppOffline** cíl, který vytvoříte v dalším kroku.
7. Přidat nový cíl s názvem **DeployAppOffline**. Uvnitř tohoto cíle vyvolat příkaz MSDeploy.exe, který nasadí vaši *aplikace\_offline* soubor na cílový webový server.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample5.xml)]
8. V tomto příkladu **ContactManagerIisPath** vlastnost jinde definovaná v souboru projektu. Toto je jednoduše cesta k aplikaci IIS, ve tvaru *[název webu služby IIS] / [název aplikace]*. Včetně podmínku cílové umožňuje uživatelům přepnout *aplikace\_offline* nasazení zapnout nebo vypnout změnou hodnoty vlastnosti nebo poskytování parametr příkazového řádku.
9. Přidat nový cíl s názvem **DeleteAppOffline**. Uvnitř tohoto cíle vyvolat příkaz MSDeploy.exe, který odstraní vaše *aplikace\_offline* souboru z cílového webového serveru.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample6.xml)]
10. Poslední, je k vyvolání těchto nového cíle na příslušné body při spuštění souboru projektu. Můžete provést různými způsoby. Například v *Publish.proj* souboru **FullPublishDependsOn** vlastnost určuje seznam cílů, které je třeba spustit v pořadí, kdy **FullPublish** výchozí cíl je volána.
11. Upravte soubor projektu nástroje MSBuild má být vyvolán **DeployAppOffline** a **DeleteAppOffline** cíle v příslušné body v procesu publikování.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample7.xml)]

Když spustíte vaše vlastní soubor projektu nástroje MSBuild *aplikace\_offline* soubor nasazen na server hned po úspěšném sestavení. Ji potom se odstraní ze serveru po dokončení všech úloh nasazení.

## <a name="adding-an-appoffline-file-to-deployment-packages"></a>Přidání aplikace\_Offline souborů pro balíčky pro nasazení

V závislosti na tom, jak nakonfigurujete vaše nasazení, všechny existující obsah na webové aplikace IIS cílové & #x 2014; jako *aplikace\_offline.htm* #x 2014; & souborů může být automaticky odstraněny při nasazení webu balíček do cílového umístění. Abyste ověřili, že *aplikace\_offline.htm* soubor zůstává na místě po dobu trvání nasazení, musíte zahrnout soubor v rámci samotného balíčku pro nasazení webu také k nasazení souboru přímo na začátku proces nasazení.

- Pokud jste postupovali podle předchozích úlohách v tomto tématu, budete přidáte *aplikace\_offline.htm* soubor projektu webové aplikace v rámci jiný název souboru (jsme použili *aplikace\_ offline template.htm*) a akce sestavení budete mít nastavený na **žádné**. Tyto změny jsou nezbytné, aby se zabránilo souborů zasahovala do činnosti vývoj a ladění. V důsledku toho je nutné přizpůsobit proces balení zajistit, aby *aplikace\_offline.htm* soubor je součástí balíčku pro nasazení webu.

Webové publikování kanálu (WPP) používá seznamu položek s názvem **FilesForPackagingFromProject** vytvořit seznam souborů, které mají být zahrnuty v balíčku pro nasazení webu. Obsah webových balíčků můžete přizpůsobit přidáním svoje vlastní položky do tohoto seznamu. K tomu potřebujete k dokončení těchto kroků:

1. Vytvořte soubor vlastní projektu s názvem *[název projektu].wpp.targets* ve stejné složce jako soubor projektu.

    > [!NOTE]
    > *. Wpp.targets* souboru patřit do stejné složky jako soubor projektu webové aplikace & #x 2014; například *ContactManager.Mvc.csproj*& #x 2014; místo ve stejné složce jako kterákoli soubory projektu vlastní slouží k řízení sestavení a proces nasazení.
2. V *. wpp.targets* souboru, vytvořte nový MSBuild cíl, který provede *před* **CopyAllFilesToSingleFolderForPackage** cíl. Toto je jako cíl, který sestavuje seznam skutečností, které chcete zahrnout do balíčku.
3. V nové cílové vytvořit **ItemGroup** element.
4. V **ItemGroup** elementu, přidejte **FilesForPackagingFromProject** položku a zadejte *aplikace\_offline.htm* souboru.

*. Wpp.targets* soubor by měl vypadat takto:


[!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample8.xml)]


Toto jsou klíčové body Poznámka: v tomto příkladu:

- **BeforeTargets** atribut vloží tento cíl do WPP zadáním, který se má provést bezprostředně před **CopyAllFilesToSingleFolderForPackage** cíl.
- **FilesForPackagingFromProject** položky používá **DestinationRelativePath** hodnota metadat o přejmenování souboru z *aplikace\_offline template.htm* k *aplikace\_offline.htm* který je přidán do seznamu.

Následující postup ukazuje, jak to přidat *. wpp.targets* soubor pro projekt webové aplikace.

**Chcete-li přidat. wpp.targets soubor balíčku pro nasazení webu**

1. Otevřete řešení v sadě Visual Studio 2010.
2. V **Průzkumníku řešení** okna, klikněte pravým tlačítkem na uzel projektu vaší webové aplikace (například **ContactManager.Mvc**), přejděte na příkaz **přidat**a potom klikněte na **Novou položku**.
3. V **přidat novou položku** dialogové okno, vyberte **souboru XML** šablony.
4. V **název** zadejte *[název projektu] ***.wpp.targets** (například **ContactManager.Mvc.wpp.targets**) a pak klikněte na tlačítko **přidat**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image4.png)

    > [!NOTE]
    > Pokud přidáte novou položku do kořenového uzlu projektu, soubor se vytvoří ve stejné složce jako soubor projektu. Můžete to ověřit tak, že v Průzkumníku Windows otevřete složku.
5. Soubor přidejte značku MSBuild popsané.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample9.xml)]
6. Uložte a zavřete *[název projektu].wpp.targets* souboru.

Příštího sestavení a balíček projektu webové aplikace, jako automaticky zjistí *. wpp.targets* souboru. *Aplikace\_offline template.htm* souboru budou zahrnuty do výsledného balíčku pro nasazení webu jako *aplikace\_offline.htm*.

> [!NOTE]
> Pokud se nasazení nezdaří, *aplikace\_offline.htm* soubor zůstane na místě a aplikace bude i nadále offline. To je obvykle toto chování žádoucí. Aby vaše aplikace zpět do režimu online, můžete odstranit *aplikace\_offline.htm* soubor z webového serveru. Případně, pokud opravte všechny chyby a spusťte úspěšné nasazení se *aplikace\_offline.htm* soubor bude odebrán.


## <a name="conclusion"></a>Závěr

Toto téma popisuje postup vám umožní webové aplikace do režimu offline po dobu trvání nasazení, publikování *aplikace\_offline.htm* souboru na cílovém serveru na začátku procesu nasazení a odebere ji na end. Ho také popsané postupy zahrnují *aplikace\_offline.htm* souboru v balíčku pro nasazení webu.

## <a name="further-reading"></a>Další čtení

Další informace o balení a nasazení procesu naleznete v tématu [budova a projekty webových aplikací balení](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [parametry konfigurace pro nasazení webového balíčku](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), a [ Nasazení webových balíčků](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Pokud publikování webových aplikací přímo ze sady Visual Studio, místo použití přístupu pro soubor vlastní projektu nástroje MSBuild popsaných v těchto kurzech, budete muset použít mírně odlišný postup provést vaší aplikace do režimu offline během publikování proces. Další informace najdete v tématu [převzetí při publikování webové aplikace do režimu offline](https://go.microsoft.com/?linkid=9805135) (příspěvek na blogu).

>[!div class="step-by-step"]
[Předchozí](excluding-files-and-folders-from-deployment.md)
[další](running-windows-powershell-scripts-from-msbuild-project-files.md)
