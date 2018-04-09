---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
title: Vyloučení souborů a složek z nasazení | Microsoft Docs
author: jrjlee
description: Toto téma popisuje, jak můžete vyloučit soubory a složky z balíčku pro nasazení webu při sestavování a balíček projekt webové aplikace.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f4cc2d40-6a78-429b-b06f-07d000d4caad
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
msc.type: authoredcontent
ms.openlocfilehash: c435448bf057bbef9127d66ffda24a07729f2322
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="excluding-files-and-folders-from-deployment"></a>Vyloučení souborů a složek z nasazení
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje, jak můžete vyloučit soubory a složky z balíčku pro nasazení webu při sestavování a balíček projekt webové aplikace.


Toto téma je součástí ze série kurzů na základě kolem podnikové požadavky nasazení fiktivní společnost s názvem Fabrikam, Inc. Tento kurz řady používá ukázkové řešení&#x2014; [řešení obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s úrovní realistické složitější, včetně aplikace ASP.NET MVC 3, komunikaci Windows Služba Foundation (WCF) a projekt databáze.

Metoda nasazení jádrem tyto kurzy je založena na popsaný přístup souboru projektu rozdělení [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve které je řízené procesu sestavení dva soubory projektu&#x2014;jeden obsahující sestavení pokyny, které platí pro každé cílové prostředí a jeden, který obsahuje nastavení pro konkrétní prostředí sestavení a nasazení. V okamžiku sestavení souboru projektu konkrétní prostředí sloučeny do souboru projektu bez ohledu na prostředí a vytvořit úplnou sadu pokynů sestavení.

## <a name="overview"></a>Přehled

Při sestavování projektu webové aplikace v sadě Visual Studio 2010 webových publikování kanálu (WPP) umožňuje rozšířit tento proces sestavení balení kompilované webové aplikace do balíčku nasadit web. Potom můžete použít nástroj nasazení webu Internetové informační služby (IIS) (Web Deploy) Chcete-li nasadit tento webový balíček na vzdálený webový server služby IIS nebo importovat balíček web ručně pomocí Správce služby IIS. Tento proces balení je vysvětleno v [budova a projekty webových aplikací balení](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).

Jak tedy můžete řídit, co získá součástí webového balíčku? Nastavení projektu v sadě Visual Studio prostřednictvím základního souboru projektu, zadejte dostatečná ovládací prvek pro mnoho scénářů. Ale v některých případech můžete chtít přizpůsobit obsah do webového balíčku do určité cílové prostředí. Například můžete chtít zahrnují složku pro soubory protokolu, pokud nasazení aplikace v testovacím prostředí, ale při nasazení aplikace na pracovním nebo produkčním prostředí vyloučit složky. Toto téma vám ukáže, jak to udělat.

## <a name="what-gets-included-by-default"></a>Co získá zahrnuté ve výchozím nastavení?

Pokud nakonfigurujete vlastnosti projektu webové aplikace v sadě Visual Studio **položky k nasazení** seznam na **balení/publikování webu** stránky umožňuje určit, co chcete zahrnout do vašeho nasazení webu balíček. Ve výchozím nastavení, to je nastavena na **pouze soubory potřebné ke spuštění této aplikace**.

![](excluding-files-and-folders-from-deployment/_static/image1.png)

Pokud vyberete **pouze soubory potřebné ke spuštění této aplikace**, jako se pokusí zjistit, které se má přidat do webového balíčku. Sem patří:

- Všechna sestavení výstupy pro projekt.
- Všechny soubory označené pomocí akce sestavení **obsahu**.

> [!NOTE]
> Logiky, která určuje, souborů, které chcete zahrnout je obsažené v tomto souboru:   
> *%PROGRAMFILES%\MSBuild\Microsoft\VisualStudio\v10.0\Web\ Microsoft.Web.Publishing.OnlyFilesToRunTheApp.targets*


## <a name="excluding-specific-files-and-folders"></a>Vyloučení určitých souborů a složek

V některých případech je výhodné, jemně odstupňovanou kontrolu nad niž se nasadí soubory a složky. Pokud víte, které soubory, které chcete vyloučit dříve času a vyloučení platí pro všechny cílové prostředí, můžete jednoduše nastavit **akce sestavení** souborů se **žádné**.

**Chcete-li vyloučit konkrétní soubory z nasazení**

1. V **Průzkumníku řešení** oken, klikněte pravým tlačítkem na soubor a pak klikněte na tlačítko **vlastnosti**.
2. V **vlastnosti** okno v **akce sestavení** řádek, vyberte **žádné**.

Tento přístup je však není vždy pohodlný. Například můžete chtít soubory, které se liší a složky, které jsou součástí podle cílové prostředí a mimo aplikaci Visual Studio. Například v ukázkové řešení obraťte se na správce, podívejte se na obsah ContactManager.Mvc projektu:

![](excluding-files-and-folders-from-deployment/_static/image2.png)

- Interní složka obsahuje některé skripty SQL, které vývojář použije k vytvoření, vyřaďte a naplnit místních databází pro účely vývoje. Nic v této složce musí být nasazené na pracovním nebo produkčním prostředí.
- Složka skripty obsahuje několik souborů JavaScript. Mnoho tyto soubory jsou zahrnuty výhradně pro podporu ladění nebo poskytovat technologii IntelliSense v sadě Visual Studio. Některé z těchto souborů nesmí být nasazeno do pracovním nebo produkčním prostředí. Můžete však nasadit do testovacího prostředí vývojáře usnadňuje řešení potíží.

I když může upravit soubory projektu vyloučit konkrétní soubory a složky, je jednodušší způsob. Zahrnuje jako mechanismus pro vyloučit soubory a složky podle budovy seznamy položka s názvem **ExcludeFromPackageFolders** a **ExcludeFromPackageFiles**. Tento mechanismus můžete rozšířit přidáním svoje vlastní položky do těchto seznamů. K tomu potřebujete k dokončení těchto kroků:

1. Vytvořte soubor vlastní projektu s názvem *[název projektu].wpp.targets* ve stejné složce jako soubor projektu.

    > [!NOTE]
    > *. Wpp.targets* souboru patřit do stejné složky jako soubor projektu webové aplikace&#x2014;například *ContactManager.Mvc.csproj*&#x2014;spíše než ve stejné složce jako vlastní soubory projektu, které můžete použít k řízení procesu sestavení a nasazení.
2. V *. wpp.targets* soubor, přidejte **ItemGroup** element.
3. V **ItemGroup** elementu, přidejte **ExcludeFromPackageFolders** a **ExcludeFromPackageFiles** položky vyloučit konkrétní soubory a složky podle potřeby.

Toto je základní strukturu tohoto *. wpp.targets* souboru:


[!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample1.xml)]


Všimněte si, že každá položka obsahuje element metadata položky s názvem **FromTarget**. Toto je volitelná hodnota, které nemají vliv na proces vytváření; jednoduše slouží k označení, proč se vynechá konkrétní soubory nebo složky pokud někdo zkontroluje protokolů o sestavení.

## <a name="excluding-files-and-folders-from-a-web-package"></a>Vyloučení souborů a složek z balíčku webu

Následující postup ukazuje, jak přidat *. wpp.targets* souborů projektu webové aplikace a jak pomocí souboru vyloučit konkrétní soubory a složky z webového balíčku při sestavování projektu.

**Vyloučit soubory a složky z balíčku pro nasazení webu**

1. Otevřete řešení v sadě Visual Studio 2010.
2. V **Průzkumníku řešení** okna, klikněte pravým tlačítkem na uzel projektu vaší webové aplikace (například **ContactManager.Mvc**), přejděte na příkaz **přidat**a potom klikněte na **Novou položku**.
3. V **přidat novou položku** dialogové okno, vyberte **souboru XML** šablony.
4. V **název** zadejte *[název projektu] ***.wpp.targets** (například **ContactManager.Mvc.wpp.targets**) a pak klikněte na tlačítko **přidat**.

    ![](excluding-files-and-folders-from-deployment/_static/image3.png)

    > [!NOTE]
    > Pokud přidáte novou položku do kořenového uzlu projektu, soubor se vytvoří ve stejné složce jako soubor projektu. Můžete to ověřit tak, že v Průzkumníku Windows otevřete složku.
5. V souboru, přidejte **projektu** elementu a **ItemGroup** element:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample2.xml)]
6. Pokud chcete vyloučení složek z webového balíčku, přidejte **ExcludeFromPackageFolders** elementu, který chcete **ItemGroup** element:

   1. V **zahrnout** atribut, zadejte středníky oddělený seznam složek, které chcete vyloučit.
   2. V **FromTarget** element metadat, zadejte smysluplný hodnotu indikující, proč jsou vyloučeny složky, jako je třeba název *. wpp.targets* souboru.

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample3.xml)]
7. Pokud chcete vyloučit soubory z webového balíčku, přidejte **ExcludeFromPackageFiles** elementu, který chcete **ItemGroup** element:

   1. V **zahrnout** atribut, zadejte středníky oddělený seznam souborů, které chcete vyloučit.
   2. V **FromTarget** element metadat, zadejte smysluplný hodnotu indikující, proč jsou vyloučeny soubory, jako je třeba název *. wpp.targets* souboru.

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample4.xml)]
8. *[Název projektu].wpp.targets* soubor by měl nyní vypadat takto:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample5.xml)]
9. Uložte a zavřete *[název projektu].wpp.targets* souboru.

Příštího sestavení a balíček projektu webové aplikace, jako automaticky zjistí *. wpp.targets* souboru. Všechny soubory a složky, které jste zadali nebude součástí webového balíčku.

## <a name="conclusion"></a>Závěr

Toto téma popisuje postup při sestavování webový balíček, tak, že vytvoříte vlastní vyloučit konkrétní soubory a složky *. wpp.targets* soubor ve stejné složce jako soubor projektu webové aplikace.

## <a name="further-reading"></a>Další čtení

Další informace o používání vlastních souborů projektu Microsoft Build Engine (MSBuild) k řízení procesu nasazení najdete v tématu [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) a [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Další informace o balení a nasazení procesu naleznete v tématu [budova a projekty webových aplikací balení](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [parametry konfigurace pro nasazení webového balíčku](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), a [ Nasazení webových balíčků](../web-deployment-in-the-enterprise/deploying-web-packages.md).

> [!div class="step-by-step"]
> [Předchozí](deploying-membership-databases-to-enterprise-environments.md)
> [další](taking-web-applications-offline-with-web-deploy.md)
