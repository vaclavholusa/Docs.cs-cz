---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
title: Převedení webových aplikací Offline s webovým nasazení | Dokumentace Microsoftu
author: jrjlee
description: Toto téma popisuje, jak provést webové aplikace v režimu offline po dobu trvání automatického nasazení pomocí Web pro internetové informační služby (IIS)...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 3e9f6e7d-8967-4586-94d5-d3a122f12529
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
msc.type: authoredcontent
ms.openlocfilehash: 9b18a48914a74a7fea85278d0e601f6f84376b23
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753660"
---
<a name="taking-web-applications-offline-with-web-deploy"></a>Převedení webových aplikací Offline pomocí webové nasazení
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje, jak provést webové aplikace v režimu offline po dobu trvání automatického nasazení pomocí Internetové informační služby (IIS) nástroj pro nasazení webu (nasazení webu). Uživatelé, kteří přejděte do webové aplikace se přesměrují do *aplikace\_offline.htm* souborů až do dokončení nasazení.


Toto téma je součástí série kurzů podle požadavků na nasazení enterprise fiktivní společnosti s názvem společnosti Fabrikam, Inc. V této sérii kurzů používá ukázkové řešení&#x2014; [řešení Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s realistické úroveň složitosti, včetně aplikace ASP.NET MVC 3, komunikace Windows Služba Foundation (WCF) a databázový projekt.

Metody nasazení v srdci těchto kurzů je založen na rozdělení přístupu soubor projektu je popsáno v [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve které je řízena procesem sestavení dva soubory projektu&#x2014;jeden obsahující pokyny, které platí pro všechny cílové prostředí a jeden obsahuje nastavení pro konkrétní prostředí sestavení a nasazení pro sestavení. V okamžiku sestavení souboru projektu specifických pro prostředí se sloučí do souboru projektu bez ohledu na prostředí a vytvoří kompletní sadu pokynů sestavení.

## <a name="task-overview"></a>Přehled úloh

V možných scénářů budete chtít webovou aplikaci převést do režimu offline, když provedete změny související součásti, jako jsou databáze nebo webové služby. Obvykle ve službě IIS a ASP.NET, můžete dosáhnout tak, že soubor s názvem *aplikace\_offline.htm* v kořenové složce webu či webové aplikace služby IIS. *Aplikace\_offline.htm* soubor je standardní soubor HTML a bude obvykle obsahovat jednoduché zprávu vyzývající uživatele, že lokalita je dočasně nedostupný kvůli údržbě. Zatímco *aplikace\_offline.htm* soubor v kořenové složce webu existuje, služba IIS automaticky přesměrovat všechny žádosti do souboru. Po dokončení provádění aktualizací, odeberete *aplikace\_offline.htm* Souborová služba a Web obnoví obsluhovat požadavky jako obvykle.

Když použijete nasazení webu k provedení jednoho kroku nebo automatizovaných nasazení do cílového prostředí, můžete chtít začlenit přidávání a odebírání *aplikace\_offline.htm* souboru do procesu nasazení. Chcete-li to provést, budete potřebovat k dokončení těchto úloh:

- V souboru projektu Microsoft Build Engine (MSBuild), který používáte k řízení procesu nasazení, vytvořte cíl nástroje MSBuild, který kopíruje *aplikace\_offline.htm* souboru na cílovém serveru před všechny úlohy nasazení začnete.
- Přidat jiný cíl nástroje MSBuild, který odebere *aplikace\_offline.htm* souboru z cílového serveru, až se dokončí všechny úlohy nasazení.
- Vytvořte v projektu webové aplikace *. wpp.targets* soubor, který zajistí, že *aplikace\_offline.htm* přidá soubor do balíčku pro nasazení při nasazení webu je vyvolána.

Toto téma se ukazují, jak provést tyto postupy. Úlohy a názorné postupy v tomto tématu se předpokládá, že jste již vytvořili řešení, které obsahuje alespoň jeden projekt webové aplikace a použít vlastní soubor k řízení procesu nasazení, jak je popsáno v [nasazení webu v Enterprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Alternativně můžete použít [Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) ukázkové řešení postupovat podle příkladů v tomto tématu.

## <a name="adding-an-appoffline-file-to-a-web-application-project"></a>Přidávání aplikací\_Offline souboru do projektu webové aplikace

Je první úkol, které potřebujete k dokončení přidání *aplikace\_offline* soubor do projektu webové aplikace:

- Zabránit souboru zasahovala do procesu vývoje (nechcete, aby vaše aplikace bude trvale offline), měli byste zavolat to něco jiného než *aplikace\_offline.htm*. Můžete například pojmenovat soubor *aplikace\_offline template.htm*.
- Chcete-li zabránit nasazuje jako soubor-je, měli byste nastavit akci sestavení na **žádný**.

**Přidání aplikace\_offline souboru do projektu webové aplikace**

1. Otevřete řešení v sadě Visual Studio 2010.
2. V **Průzkumníka řešení** okna, klikněte pravým tlačítkem na projekt webové aplikace, přejděte na **přidat**a potom klikněte na tlačítko **nová položka**.
3. V **přidat novou položku** dialogu **stránku HTML**.
4. V **název** zadejte **aplikace\_offline template.htm**a potom klikněte na tlačítko **přidat**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image1.png)
5. Přidat některé jednoduchého kódu HTML k informování uživatelů o tom, že aplikace je k dispozici a pak soubor uložte. Neobsahují žádné značky na straně serveru (například všechny značky, které mají předponu "asp:"). 

    ![](taking-web-applications-offline-with-web-deploy/_static/image2.png)
6. V **Průzkumníka řešení** okna, klikněte pravým tlačítkem na nový soubor a potom klikněte na tlačítko **vlastnosti**.
7. V **vlastnosti** okno v **akce sestavení** řádek, vyberte **žádný**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image3.png)

## <a name="deploying-and-deleting-an-appoffline-file"></a>Nasazení a odstraňuje se aplikace\_Offline souboru

Dalším krokem je úprava logiky nasazení odebrat po uplynutí a zkopírujte soubor do cílového serveru na začátku procesu nasazení.

> [!NOTE]
> V dalším postupu se předpokládá, že používáte vlastní soubor projektu MSBuild řídit proces nasazení, jak je popsáno v [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Pokud nasazujete přímo z Visual Studio, budete muset použít jiný přístup. SAYED Ibrahim Hashimi popisuje jeden takový přístup v [jak využít vaše aplikace v režimu Offline během publikování na webu](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx).


K nasazení *aplikace\_offline* soubor na cílový web služby IIS, je potřeba vyvolat pomocí MSDeploy.exe [Webdeploy **contentPath** poskytovatele](https://technet.microsoft.com/library/dd569034(WS.10).aspx). **ContentPath** zprostředkovatel podporuje fyzický adresář cesty a cesty k webu nebo aplikaci služby IIS, díky tomu je ideální volbou pro synchronizaci souborů mezi složky projektu sady Visual Studio a webové aplikace služby IIS. K nasazení souboru, MSDeploy příkazu by měl vypadat přibližně takto:


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample1.cmd)]


Odeberte soubor z cílové lokality na konci procesu nasazení, MSDeploy příkazu by měl vypadat přibližně takto:


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample2.cmd)]


K automatizaci těchto příkazů jako součást procesu sestavení a nasazení, musíte integrovat do vlastního souboru projektu MSBuild. Následující postup popisuje, jak to provést.

**K nasazení a odstranění aplikace\_offline souboru**

1. V sadě Visual Studio 2010 otevřete soubor projektu MSBuild, který řídí proces nasazení. V [Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) ukázkové řešení, je to *Publish.proj* souboru.
2. V kořenovém adresáři **projektu** elementu, vytvořte nový **PropertyGroup** – element pro uložení proměnné pro *aplikace\_offline* nasazení:

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample3.xml)]
3. **SourceRoot** vlastnost je definována jinde v *Publish.proj* souboru. Určuje umístění kořenové složky pro zdrojový obsah vzhledem k aktuální cesta&#x2014;jinými slovy, relativní k umístění *Publish.proj* souboru.
4. **ContentPath** poskytovatele nebude přijímat relativní cesty k souboru, takže je potřeba získat absolutní cestu ke zdrojovému souboru, než bude možné nasadit. Můžete použít [converttoabsolutepath –](https://msdn.microsoft.com/library/bb882668.aspx) úlohy provedete to tak.
5. Přidat nový **cílové** element s názvem **GetAppOfflineAbsolutePath**. Uvnitř tohoto cíle, použijte **converttoabsolutepath –** úlohu k získání absolutní cesta k *aplikace\_offline šablony* souboru ve složce vašeho projektu.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample4.xml)]
6. Relativní cesta k přebírá tento cíl *aplikace\_offline šablony* souboru ve složce vašeho projektu a uloží jej do nové vlastnosti jako absolutní cestu k souboru. **BeforeTargets** Určuje atribut, který má tento cíl, předtím než **DeployAppOffline** cíl, který vytvoříte v dalším kroku.
7. Přidat nový cíl s názvem **DeployAppOffline**. Uvnitř tohoto cíle vyvolat příkaz MSDeploy.exe, která nasadí vaši *aplikace\_offline* soubor na cílový webový server.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample5.xml)]
8. V tomto příkladu **ContactManagerIisPath** vlastnost je definována jinde v souboru projektu. Toto je jednoduše IIS cestu k aplikaci, ve formuláři *[název webu služby IIS] / [název aplikace]*. Včetně podmínku v cílovém umožňuje uživatelům přepínat *aplikace\_offline* nasazení nebo vypnout změnou hodnoty vlastnosti nebo poskytnutí parametr příkazového řádku.
9. Přidat nový cíl s názvem **DeleteAppOffline**. Uvnitř tohoto cíle vyvolat příkaz MSDeploy.exe, která odebere vaše *aplikace\_offline* soubor z webového serveru cílový.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample6.xml)]
10. Posledním úkolem je vyvolat tyto nové cíle na odpovídající body při spuštění souboru projektu. To lze provést různými způsoby. Například v *Publish.proj* souboru **FullPublishDependsOn** vlastnost určuje seznam cílů, které musí být provedeny v pořadí, kdy **FullPublish** výchozí cíl je vyvolána.
11. Upravte soubor projektu MSBuild k vyvolání **DeployAppOffline** a **DeleteAppOffline** cíle ve vhodných míst v procesu publikování.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample7.xml)]

Když spustíte svůj vlastní soubor projektu MSBuild, *aplikace\_offline* se soubor nasadí na server hned po úspěšném sestavení. Potom se odstraní ze serveru po dokončení všech úloh nasazení.

## <a name="adding-an-appoffline-file-to-deployment-packages"></a>Přidávání aplikací\_Offline souboru do balíčků pro nasazení

V závislosti na tom, jak nakonfigurovat nasazení, všechny existující obsah v cílovém umístění služby IIS webová aplikace&#x2014;jako *aplikace\_offline.htm* souboru&#x2014;mohou být automaticky odstraněny při nasazování web balíček do cíle. Chcete-li zajistit *aplikace\_offline.htm* soubor zůstává na místě po dobu trvání nasazení, je nutné zahrnout soubor v rámci samotné balíčku pro nasazení webu také k nasazení souboru přímo na začátku proces nasazení.

- Pokud jste postupovali podle předchozích úlohách v tomto tématu, budete mít přidaný *aplikace\_offline.htm* soubor do projektu webové aplikace v rámci jiný název souboru (jsme použili *aplikace\_ offline template.htm*) a budete mít nastavte akci sestavení na **žádný**. Tyto změny jsou nutné zabránit souboru z zasahovala vývoj a ladění. V důsledku toho je třeba přizpůsobit proces balení a zkontrolujte, že *aplikace\_offline.htm* soubor je součástí balíčku pro nasazení webu.

Web pro publikování kanálu (WPP) používá seznam položek s názvem **FilesForPackagingFromProject** k vytvoření seznamu souborů, které mají být zahrnuty do balíčku pro nasazení webu. Obsah webových balíčků můžete přizpůsobit tak, že přidáte vlastní položky do tohoto seznamu. K tomu, které potřebujete k dokončení těchto kroků:

1. Vytvoření vlastního projektu soubor s názvem *[název projektu].wpp.targets* ve stejné složce jako soubor projektu.

    > [!NOTE]
    > *. Wpp.targets* souboru musí být ve stejné složce jako soubor projektu webové aplikace&#x2014;například *ContactManager.Mvc.csproj*&#x2014;, nikoli ve stejné složce jako vlastní soubory projektu, které můžete použít k řízení procesu sestavení a nasazení.
2. V *. wpp.targets* soubor, vytvořte nový cíl nástroje MSBuild, který se spustí *před* **CopyAllFilesToSingleFolderForPackage** cíl. Toto je cílová WPP, který vytváří seznam skutečností, které chcete zahrnout do balíčku.
3. Vytvořte v nový cíl **ItemGroup** element.
4. V **ItemGroup** element, přidejte **FilesForPackagingFromProject** položku a zadat *aplikace\_offline.htm* souboru.

*. Wpp.targets* soubor by měl vypadat takto:


[!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample8.xml)]


Toto jsou klíčové body Poznámka: v tomto příkladu:

- **BeforeTargets** atribut vloží tento cíl do WPP tak, že určíte, který by měl být spuštěn bezprostředně před **CopyAllFilesToSingleFolderForPackage** cíl.
- **FilesForPackagingFromProject** položky používá **DestinationRelativePath** hodnota metadat se přejmenovat soubor z *aplikace\_offline template.htm* k *aplikace\_offline.htm* je přidat do seznamu.

Následující postup ukazuje, jak přidat *. wpp.targets* soubor do projektu webové aplikace.

**Chcete-li přidat. wpp.targets soubor balíčku pro nasazení webu**

1. Otevřete řešení v sadě Visual Studio 2010.
2. V **Průzkumníka řešení** okna, klikněte pravým tlačítkem na uzel projektu webové aplikace (například **ContactManager.Mvc**), přejděte na **přidat**a potom klikněte na tlačítko **Nová položka**.
3. V **přidat novou položku** dialogové okno, vyberte **soubor XML** šablony.
4. V **název** zadejte *[název projektu]* **.wpp.targets** (například **ContactManager.Mvc.wpp.targets**) a potom klikněte na tlačítko **přidat**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image4.png)

    > [!NOTE]
    > Pokud chcete přidat novou položku do kořenového uzlu projektu, soubor se vytvoří ve stejné složce jako soubor projektu. Můžete to ověřit tak, že otevřete složku v Průzkumníku Windows.
5. V souboru přidejte značky MSBuild je popsáno výše.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample9.xml)]
6. Uložte a zavřete *[název projektu].wpp.targets* souboru.

Při příštím build a balíček projektu webové aplikace, WPP automaticky zjistí, *. wpp.targets* souboru. *Aplikace\_offline template.htm* soubor bude obsahovat výsledný balíčku pro nasazení webu jako *aplikace\_offline.htm*.

> [!NOTE]
> Pokud se nasazení nezdaří, *aplikace\_offline.htm* soubor zůstane na místě a vaše aplikace bude i nadále v režimu offline. Obvykle se jedná o požadované chování. Aby vaše aplikace zpět do online režimu, můžete odstranit *aplikace\_offline.htm* soubor z webového serveru. Případně, pokud opravíte jakékoli chyby a spuštění úspěšné nasazení *aplikace\_offline.htm* soubor bude odstraněn.


## <a name="conclusion"></a>Závěr

Toto téma popisuje postupy v této webové aplikace v režimu offline po dobu trvání nasazení, publikování *aplikace\_offline.htm* soubor na cílový server na začátku procesu nasazení a odebere ji na Konec. Také popisuje způsoby patří *aplikace\_offline.htm* souborů v balíčku pro nasazení webu.

## <a name="further-reading"></a>Další čtení

Další informace o vytváření balíčků a proces nasazení, najdete v části [sestavení a balení projektů webových aplikací](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [konfigurace parametrů nasazení webového balíčku](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), a [ Nasazení webových balíčků](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Pokud publikujete, aby vaše webové aplikace přímo ze sady Visual Studio, místo použití přístupu pro soubor vlastní projekt MSBuild je popsáno v těchto kurzech, budete muset použít trochu jiný přístup k převést aplikaci do offline režimu během publikování proces. Další informace najdete v tématu [o provedení offline vaší webové aplikace během publikování](https://go.microsoft.com/?linkid=9805135) (příspěvek na blogu).

> [!div class="step-by-step"]
> [Předchozí](excluding-files-and-folders-from-deployment.md)
> [další](running-windows-powershell-scripts-from-msbuild-project-files.md)
