---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
title: Vyloučení souborů a složek z nasazení | Dokumentace Microsoftu
author: jrjlee
description: Toto téma popisuje, jak můžete vyloučit soubory a složky z balíčku pro nasazení webu při sestavení a zabalení webové aplikace.
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: f4cc2d40-6a78-429b-b06f-07d000d4caad
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
msc.type: authoredcontent
ms.openlocfilehash: 33eecfea86842a93eac705e7823f1eba9519670e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816675"
---
<a name="excluding-files-and-folders-from-deployment"></a>Vyloučení souborů a složek z nasazení
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje, jak můžete vyloučit soubory a složky z balíčku pro nasazení webu při sestavení a zabalení webové aplikace.


Toto téma je součástí série kurzů podle požadavků na nasazení enterprise fiktivní společnosti s názvem společnosti Fabrikam, Inc. V této sérii kurzů používá ukázkové řešení&#x2014; [řešení Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s realistické úroveň složitosti, včetně aplikace ASP.NET MVC 3, komunikace Windows Služba Foundation (WCF) a databázový projekt.

Metody nasazení v srdci těchto kurzů je založen na rozdělení přístupu soubor projektu je popsáno v [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve které je řízena procesem sestavení dva soubory projektu&#x2014;jeden obsahující pokyny, které platí pro všechny cílové prostředí a jeden obsahuje nastavení pro konkrétní prostředí sestavení a nasazení pro sestavení. V okamžiku sestavení souboru projektu specifických pro prostředí se sloučí do souboru projektu bez ohledu na prostředí a vytvoří kompletní sadu pokynů sestavení.

## <a name="overview"></a>Přehled

Při sestavování projektu webové aplikace v sadě Visual Studio 2010 webových publikování kanálu (WPP) umožňuje rozšířit tento proces sestavení balení kompilované webové aplikace do nasazení webového balíčku. Můžete použít nástroj pro nasazení Internetové informační služby (IIS) webu (nasazení webu) nasadit tento webový balíček do vzdáleného webového serveru služby IIS nebo importovat balíček web ručně pomocí Správce služby IIS. Tento proces vytváření balíčku je podrobně [sestavení a balení projektů webových aplikací](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).

Jak tedy můžete řídit, co získá součástí webového balíčku? Nastavení projektu v sadě Visual Studio prostřednictvím základního souboru projektu, poskytuje dostatečnou kontrolu pro mnoho scénářů. Nicméně v některých případech můžete chtít přizpůsobit obsah váš webový balíček do určité cílové prostředí. Například můžete chtít zahrnout složku pro soubory protokolů, při nasazení aplikace do testovacího prostředí, ale vyloučit složku, když nasadíte aplikaci do testovací nebo produkční prostředí. Toto téma se ukazují, jak to udělat.

## <a name="what-gets-included-by-default"></a>Co získá zahrnuté ve výchozím nastavení?

Když nakonfigurujete vlastnosti projektu webové aplikace v sadě Visual Studio **položky, které chcete nasadit** seznamu **balení/publikování webu** stránky umožňuje určit, co chcete zahrnout do nasazení webových balíček. Ve výchozím nastavení, je nastavené na **pouze soubory potřebné ke spuštění této aplikace**.

![](excluding-files-and-folders-from-deployment/_static/image1.png)

Pokud zvolíte **pouze soubory potřebné ke spuštění této aplikace**, WPP se pokusí zjistit, které soubory přidaly do webového balíčku. Sem patří:

- Vypíše všechna sestavení pro projekt.
- Všechny soubory označené pomocí akce sestavení **obsahu**.

> [!NOTE]
> Logika, která určuje soubory, které chcete zahrnout je obsažen v tomto souboru:   
> *%ProgramFiles%\MSBuild\Microsoft\VisualStudio\v10.0\Web\ Microsoft.Web.Publishing.OnlyFilesToRunTheApp.targets*


## <a name="excluding-specific-files-and-folders"></a>Vyloučení určitých souborů a složek

V některých případech je vhodné jemněji odstupňovanou kontrolu nad tím, které jsou nasazené soubory a složky. Pokud víte, že čas soubory, které chcete vyloučit předem a vyloučení platí pro všechny cílové prostředí, stačí nastavit **akce sestavení** každého souboru **žádný**.

**Vyloučit konkrétní soubory z nasazení**

1. V **Průzkumníka řešení** okna, klikněte pravým tlačítkem na soubor a potom klikněte na tlačítko **vlastnosti**.
2. V **vlastnosti** okno v **akce sestavení** řádek, vyberte **žádný**.

Ale tento přístup není vždy vhodné. Například můžete chtít soubory, které se liší a složky, které jsou součástí podle cílového prostředí a mimo aplikaci Visual Studio. Například v ukázkovém řešení Správce kontaktů, podívejte se na obsah ContactManager.Mvc projektu:

![](excluding-files-and-folders-from-deployment/_static/image2.png)

- Interní složka obsahuje některé skripty SQL, které vývojář používá k vytvoření, vyřaďte a naplnit místních databází pro účely vývoje. Nic v této složce musí být nasazené pro testovací nebo produkční prostředí.
- Složka skripty obsahuje několik souborů JavaScriptu. Velké množství tyto soubory jsou zahrnuty výhradně na podporu ladění nebo na poskytovat technologii IntelliSense v sadě Visual Studio. Některé z těchto souborů by se neměly nasazovat do pracovní nebo produkční prostředí. Ale můžete je nasadit do testovacího prostředí pro vývojáře pro usnadnění odstraňování potíží.

I když může pracovat s souborů vyloučit konkrétní soubory a složky projektu, je snadnější. Obsahuje mechanismus pro vyloučení souborů a složek pomocí seznamů položek s názvem WPP **ExcludeFromPackageFolders** a **ExcludeFromPackageFiles**. Tento mechanismus můžete rozšířit přidáním vlastní položky do seznamů. K tomu, které potřebujete k dokončení těchto kroků:

1. Vytvoření vlastního projektu soubor s názvem *[název projektu].wpp.targets* ve stejné složce jako soubor projektu.

    > [!NOTE]
    > *. Wpp.targets* souboru musí být ve stejné složce jako soubor projektu webové aplikace&#x2014;například *ContactManager.Mvc.csproj*&#x2014;, nikoli ve stejné složce jako vlastní soubory projektu, které můžete použít k řízení procesu sestavení a nasazení.
2. V *. wpp.targets* přidejte **ItemGroup** elementu.
3. V **ItemGroup** prvku, přidejte **ExcludeFromPackageFolders** a **ExcludeFromPackageFiles** položky, které chcete vyloučit určité soubory a složky podle potřeby.

Toto je základní struktura *. wpp.targets* souboru:


[!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample1.xml)]


Všimněte si, že každá položka obsahuje prvek položky metadat s názvem **FromTarget**. Toto je volitelná hodnota, která nemá vliv na procesu sestavení. jednoduše slouží k označení, proč byly vynechány konkrétní soubory nebo složky, pokud někdo kontroluje protokoly sestavení.

## <a name="excluding-files-and-folders-from-a-web-package"></a>Vyloučení souborů a složek z webového balíčku

Následující postup ukazuje, jak přidat *. wpp.targets* soubor do projektu webové aplikace a jak vyloučit konkrétní soubory a složky z webového balíčku při vytváření projektu pomocí souboru.

**Vyloučit soubory a složky z balíčku pro nasazení webu**

1. Otevřete řešení v sadě Visual Studio 2010.
2. V **Průzkumníka řešení** okna, klikněte pravým tlačítkem na uzel projektu webové aplikace (například **ContactManager.Mvc**), přejděte na **přidat**a potom klikněte na tlačítko **Nová položka**.
3. V **přidat novou položku** dialogové okno, vyberte **soubor XML** šablony.
4. V **název** zadejte *[název projektu]* **.wpp.targets** (například **ContactManager.Mvc.wpp.targets**) a potom klikněte na tlačítko **přidat**.

    ![](excluding-files-and-folders-from-deployment/_static/image3.png)

    > [!NOTE]
    > Pokud chcete přidat novou položku do kořenového uzlu projektu, soubor se vytvoří ve stejné složce jako soubor projektu. Můžete to ověřit tak, že otevřete složku v Průzkumníku Windows.
5. V souboru, přidejte **projektu** elementu a **ItemGroup** element:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample2.xml)]
6. Pokud chcete vyloučit složky z webového balíčku, přidejte **ExcludeFromPackageFolders** elementu **ItemGroup** element:

   1. V **zahrnout** atribut, zadejte středníkem oddělený seznam složek, které chcete vyloučit.
   2. V **FromTarget** prvek metadat znamenat smysluplnou hodnotu označující, proč jsou vyloučeny složek, jako je název *. wpp.targets* souboru.

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample3.xml)]
7. Pokud chcete vyloučit soubory z webového balíčku, přidejte **ExcludeFromPackageFiles** elementu **ItemGroup** element:

   1. V **zahrnout** atribut, zadejte středníkem oddělený seznam souborů, které chcete vyloučit.
   2. V **FromTarget** prvek metadat znamenat smysluplnou hodnotu označující, proč jsou vyloučeny soubory, jako je název *. wpp.targets* souboru.

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample4.xml)]
8. *[Název projektu].wpp.targets* soubor by měl nyní vypadat takto:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample5.xml)]
9. Uložte a zavřete *[název projektu].wpp.targets* souboru.

Při příštím build a balíček projektu webové aplikace, WPP automaticky zjistí, *. wpp.targets* souboru. Všechny soubory a složky, které jste zadali nesmí být součástí webového balíčku.

## <a name="conclusion"></a>Závěr

Toto téma popisuje, jak vyloučit určité soubory a složky, když vytvoříte tak, že vytvoříte vlastní webový balíček, *. wpp.targets* souboru ve stejné složce jako soubor projektu webové aplikace.

## <a name="further-reading"></a>Další čtení

Další informace o používání vlastních souborů projektu Microsoft Build Engine (MSBuild) k řízení procesu nasazení najdete v tématu [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) a [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Další informace o vytváření balíčků a proces nasazení, najdete v části [sestavení a balení projektů webových aplikací](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [konfigurace parametrů nasazení webového balíčku](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), a [ Nasazení webových balíčků](../web-deployment-in-the-enterprise/deploying-web-packages.md).

> [!div class="step-by-step"]
> [Předchozí](deploying-membership-databases-to-enterprise-environments.md)
> [další](taking-web-applications-offline-with-web-deploy.md)
