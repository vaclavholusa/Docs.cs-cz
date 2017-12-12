---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
title: "Vytváření a balení projekty webových aplikací | Microsoft Docs"
author: jrjlee
description: "Pokud chcete nasadit projekt webové aplikace v prostředí vzdáleného serveru, je první úlohou se projekt sestavil a generovat XPSpolečnost nasazení webové..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 94e92f80-a7e3-4d18-9375-ff8be5d666ac
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
msc.type: authoredcontent
ms.openlocfilehash: c05f725c9e6b493a6af8f5b5d20dbc9ff73a1ef8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="building-and-packaging-web-application-projects"></a>Vytváření a balení projekty webových aplikací
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Pokud chcete nasadit projekt webové aplikace v prostředí vzdáleného serveru, je první úlohou se projekt sestavil a generovat balíčku pro nasazení webu. Toto téma popisuje, jak funguje procesu sestavení pro projekty webových aplikací. Konkrétně vysvětluje:
> 
> - Jak webových publikování kanálu (WPP) rozšiřuje procesu sestavení, aby obsahovaly nasazení funkce.
> - Jak nástroj nasazení webu Internetové informační služby (IIS) (Web Deploy) změní na webové aplikace do balíčku nasazení.
> - Sestavení a balení zpracovat jak funguje a jaké soubory jsou vytvořeny.


V sadě Visual Studio 2010 je podporováno jako procesem sestavení a nasazení pro projekty webových aplikací. JAKO poskytuje sada Microsoft Build Engine (MSBuild) cíle, které rozšiřují funkce nástroje MSBuild jej a povolte integraci s nasazení webu. V sadě Visual Studio můžete zobrazit tato rozšířená funkce na stránkách vlastností projektu webové aplikace. **Balení/publikování webu** stránky, společně s **balení/publikování kódu SQL** stránce umožňuje nakonfigurovat jak projektu webové aplikace zabalené pro nasazení po dokončení procesu sestavení.

![](building-and-packaging-web-application-projects/_static/image1.png)

## <a name="how-does-the-wpp-work"></a>Jak funguje jako?

Pokud jste si prohlédněte soubor projektu pro jazyk C#-projektu na základě webové aplikace, uvidíte, že se importuje dva soubory .targets.


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample1.xml)]


První **Import** příkaz jsou společná pro všechny projekty Visual C#. Tento soubor *Microsoft.CSharp.targets*, obsahuje cílů a úloh, které jsou specifické pro Visual C#. Například kompilátor C# (**Csc**) úkol vyvolán sem. *Microsoft.CSharp.targets* souboru zase importy *Microsoft.Common.targets* souboru. Definuje cílů, které jsou společné pro všechny projekty, jako je třeba **sestavení**, **znovu sestavit**, **spustit**, **zkompilovat**, a **vyčištění** . Druhý **Import** příkaz je specifická pro projekty webových aplikací. *Microsoft.WebApplication.targets* souboru zase importy *Microsoft.Web.Publishing.targets* souboru. *Microsoft.Web.Publishing.targets* souboru v podstatě *je* jako. Definuje cíle, jako je třeba **balíček** a **MSDeployPublish**, který vyvolání Web Deploy dokončit různé úkoly nasazení.

Abyste pochopili, jak se používají tyto další cíle v ukázkové řešení obraťte se na správce, otevřete *Publish.proj* souborů a podívejte se na **BuildProjects** cíl.


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample2.xml)]


Tento cíl se používá **MSBuild** úloh k vytvoření různých projektů. Upozornění **DeployOnBuild** a **DeployTarget** vlastnosti:

- **DeployOnBuild = true** vlastnost v podstatě znamená "Chcete provést další cíl po úspěšném dokončení sestavení."
- **DeployTarget** vlastnost identifikuje název cíle, který chcete provést, když **DeployOnBuild** vlastnost rovná **true**. V takovém případě určujete, že chcete MSBuild provést **balíček** cíl po sestavení projektu.

**Balíček** cíl je definován v *Microsoft.Web.Publishing.targets* souboru. Tento cíl v podstatě trvá výstup sestavení projektu webové aplikace a převede ji balíčku pro nasazení webu, která může být publikována na webovém serveru IIS.

> [!NOTE]
> Chcete-li zobrazit soubor projektu (například *ContactManager.Mvc.csproj*) v sadě Visual Studio 2010, je nutné nejprve uvolnit projekt z vašeho řešení. V **Průzkumníku řešení** oken, klikněte pravým tlačítkem na uzel projektu a pak klikněte na tlačítko **uvolnit projekt**. Znovu klikněte pravým tlačítkem na uzel projektu a pak klikněte na **upravit***[soubor projektu]*). Soubor projektu se otevřou v jeho základním formátu XML. Nezapomeňte projekt znovu načíst, když jste hotovi.  
> Další informace o cíle MSBuild, úlohy, a **Import** příkazy, najdete v části [vysvětlení souboru projektu](understanding-the-project-file.md). Podrobnější Úvod do souborů projektu a jako, najdete v části [uvnitř Microsoft Build Engine: pomocí nástroje MSBuild a Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi a William Bartholomew, ISBN: 978-0-7356-4524-0.


## <a name="what-is-a-web-deployment-package"></a>Co je balíčku pro nasazení webu?

Vytvořit a nasadit projekt webové aplikace pomocí sady Visual Studio 2010 nebo pomocí nástroje MSBuild přímo, výsledek je obvykle *balíčku pro nasazení webu*. Balíčku pro nasazení webu je soubor ZIP. Obsahuje všechno, co této služby IIS a Web Deploy potřebují k znovu vytvořit webové aplikace, včetně:

- Výstup kompilované webové aplikace, včetně obsahu, soubory prostředků, konfigurační soubory, JavaScript a kaskádových stylů CSS prostředky styl a tak dále.
- Reference na sestavení pro váš projekt webové aplikace a pro všechny projekty v řešení.
- Skripty SQL pro generování všechny databáze, které nasazujete s webovou aplikací.

Po vygenerování balíčku pro nasazení webu, můžete ho publikovat na webový server služby IIS různými způsoby. Například můžete nasadit vzdáleně pomocí cílení vzdáleného agenta nasazení webové služby nebo obslužná rutina webových nasadit na cílový webový server nebo správce služby IIS lze ručně importovat balíček na cílový webový server. Další informace o těchto přístupy k nasazení najdete v tématu [výběr práva přístup k nasazení webu](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md).

## <a name="how-does-the-build-process-work"></a>Jak funguje procesu sestavení?

Ukazuje to, co se stane, když sestavení a balíček projekt webové aplikace:

![](building-and-packaging-web-application-projects/_static/image2.png)

Při sestavování projektu webové aplikace procesu sestavení generuje soubor s názvem *[název projektu]. SourceManifest.xml*. Soubor projektu a sestavení výstupu to *. SourceManifest.xml* soubor informuje Web Deploy vše potřebné pro zahrnutí do balíčku pro nasazení webu. Pomocí těchto vstupů, Web Deploy generuje balíčku pro nasazení webu s názvem *[název projektu] .zip*.

Vedle balíčku pro nasazení webu generuje procesu sestavení dva soubory, které vám můžou pomoct při použití balíčku:

- *. Deploy.cmd* soubor obsahuje sadu parametrizované příkazy nasazení webu (MSDeploy.exe), které publikování vašeho balíčku pro nasazení webu na vzdálený webový server služby IIS. Spuštění *. deploy.cmd* soubor s příslušnými parametry, obvykle poskytuje rychlejší a snadnější alternativní k ruční vytváření MSDeploy.exe příkazů sami.
- *SetParameters.xml* soubor poskytuje sadu hodnot parametrů, které příkaz MSDeploy.exe. Tyto hodnoty obsahovat vlastnosti, jako je třeba název webové aplikace IIS, do které chcete nasadit balíček hodnoty žádné koncové body služby a v definovány připojovací řetězce *web.config* soubor a všechny vlastnosti nasazení hodnoty definované na stránkách vlastností projektu.

*SetParameters.xml* souborů je klíčová pro správu procesu nasazení. Tento soubor je generována dynamicky podle obsahu projektu webové aplikace. Například, pokud přidat připojovací řetězec k vaší *web.config* souboru procesu sestavení bude automaticky zjistit připojovací řetězec, odpovídajícím způsobem Parametrizace nasazení a vytvořit položku v  *SetParameters.xml* souboru a umožní vám upravit připojovací řetězec jako součást procesu nasazení. Dalším tématu [parametry konfigurace pro nasazení webového balíčku](configuring-parameters-for-web-package-deployment.md), vysvětluje roli tento soubor podrobněji a popisuje různé způsoby, ve kterém můžete upravit ji během sestavení a nasazení.

> [!NOTE]
> V sadě Visual Studio 2010 jako nepodporuje předkompilace stránky ve webové aplikaci před balení. Další verze sady Visual Studio a jako bude obsahovat možnost předkompilovat webovou aplikaci jako možnost balení.


## <a name="conclusion"></a>Závěr

Toto téma poskytuje přehled o sestavení a proces balení pro projekty webových aplikací v sadě Visual Studio 2010. Je popsané, jak jako umožňuje vyvolání nástroje nasazení webu příkazy z nástroje MSBuild a je popsáno, jak sestavení a balení procesu.

Po vytvoření balíčku pro nasazení webu, dalším krokem je její nasazení. Další informace najdete v tématu [parametry konfigurace pro nasazení webového balíčku](configuring-parameters-for-web-package-deployment.md) a [nasazení webových balíčků](deploying-web-packages.md).

## <a name="further-reading"></a>Další čtení

Další témata v tomto kurzu [parametry konfigurace pro nasazení webového balíčku](configuring-parameters-for-web-package-deployment.md) a [nasazení webových balíčků](deploying-web-packages.md), poskytovat informace o tom, jak používat webový balíček, který jste vytvořili. Poslední kurzu této série [pokročilé nasazení webu Enterprise](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md), poskytuje pokyny o tom, jak přizpůsobit a řešení potíží během balení.

Podrobnější Úvod do souborů projektu a jako, najdete v části [uvnitř Microsoft Build Engine: pomocí nástroje MSBuild a Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi a William Bartholomew, ISBN: 978-0-7356-4524-0.

>[!div class="step-by-step"]
[Předchozí](understanding-the-build-process.md)
[další](configuring-parameters-for-web-package-deployment.md)
