---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
title: Sestavení a balení projektů webových aplikací | Dokumentace Microsoftu
author: jrjlee
description: Pokud chcete nasadit projekt webové aplikace do prostředí vzdáleného serveru, je první úkol se projekt sestavil a generovat ob nasazení webové...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 94e92f80-a7e3-4d18-9375-ff8be5d666ac
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
msc.type: authoredcontent
ms.openlocfilehash: ff8312d16dff2a9eec9ae909bca5e72d52f17094
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382605"
---
<a name="building-and-packaging-web-application-projects"></a>Sestavení a balení projektů webových aplikací
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Pokud chcete nasadit projekt webové aplikace do prostředí vzdáleného serveru, je první úkol se projekt sestavil a vygenerování balíčku pro nasazení webu. Toto téma popisuje, jak funguje proces sestavení pro projekty webových aplikací. Konkrétně se vysvětluje:
> 
> - Jak webových publikování kanálu (WPP) rozšiřuje proces sestavení zahrnout funkce nasazení.
> - Jak nástroj pro nasazení Internetové informační služby (IIS) webu (nasazení webu) se změní na vaší webové aplikace do balíčku pro nasazení.
> - Sestavení a zabalení zpracovat jak funguje a jaké soubory jsou vytvořeny.


V sadě Visual Studio 2010 podporuje WPP procesu sestavení a nasazení pro projekty webových aplikací. WPP poskytuje sadu Microsoft Build Engine (MSBuild) cíle, které rozšiřují funkce nástroje MSBuild a povolte ji integrovat s nasazením webu. V sadě Visual Studio můžete zobrazit tyto rozšířené funkce na stránkách vlastností projektu webové aplikace. **Balení/publikování webu** stránky, společně s **balení/publikování kódu SQL** stránce umožňuje nakonfigurovat jak projektu webové aplikace je zabalená pro účely nasazení po dokončení procesu sestavení.

![](building-and-packaging-web-application-projects/_static/image1.png)

## <a name="how-does-the-wpp-work"></a>Jak funguje WPP?

Pokud jste se podívejte na soubor projektu pro jazyk C#-projekt na základě webové aplikace, uvidíte, že importuje dva soubory .targets.


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample1.xml)]


První **Import** příkaz je společné pro všechny projekty Visual C#. Tento soubor *Microsoft.CSharp.targets*, obsahuje cíle a úlohy, které jsou specifické pro jazyk Visual C#. Například kompilátor jazyka C# (**Csc**) je úkol vyvolán tady. *Microsoft.CSharp.targets* souboru zase importy *cílů Microsoft.Common.targets* souboru. Definuje cíle, které jsou společné pro všechny projekty, jako je třeba **sestavení**, **znovu sestavit**, **spustit**, **kompilaci**, a **vyčistit** . Druhá **Import** příkaz je specifické pro projekty webových aplikací. *Microsoft.WebApplication.targets* zase soubor importy *Microsoft.Web.Publishing.targets* souboru. *Microsoft.Web.Publishing.targets* soubor v podstatě *je* WPP. Určuje cíle, jako je třeba **balíčku** a **MSDeployPublish**, který vyvolat Webdeploy dokončit různé úkoly nasazení.

Chcete-li pochopit, jak se používají tyto další cíle v ukázkovém řešení Správce kontaktů, otevřete *Publish.proj* soubor a podívejte se na **BuildProjects** cíl.


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample2.xml)]


Používá tento cíl **MSBuild** úkolů k sestavení různé projekty. Všimněte si, že **DeployOnBuild** a **DeployTarget** vlastnosti:

- **DeployOnBuild = true** vlastnost v podstatě znamená "Chci provést další cíl při sestavení bylo úspěšně dokončeno."
- **DeployTarget** vlastnost určuje název cíle, které chcete provést, když **DeployOnBuild** rovná vlastnost **true**. V takovém případě určujete, že chcete, aby MSBuild ke spuštění **balíčku** cíl po sestavení projektu.

**Balíčku** cíl je definována v *Microsoft.Web.Publishing.targets* souboru. Tento cíl v podstatě trvá výstupu sestavení projektu webové aplikace a převede ji na balíčku pro nasazení webu, který lze publikovat na webový server služby IIS.

> [!NOTE]
> Chcete-li zobrazit soubor projektu (například <em>ContactManager.Mvc.csproj</em>) v sadě Visual Studio 2010, musíte nejprve uvolněte projekt z řešení. V <strong>Průzkumníka řešení</strong> okna, klikněte pravým tlačítkem na uzel projektu a pak klikněte na tlačítko <strong>uvolnit projekt</strong>. Znovu klikněte pravým tlačítkem na uzel projektu a pak klikněte na tlačítko <strong>upravit</strong><em>[soubor projektu]</em>). Soubor projektu se otevře v nezpracované podobě XML. Nezapomeňte znovu načíst projekt, až budete hotovi.  
> Další informace o MSBuild cíle, úkoly, a <strong>Import</strong> příkazy, naleznete v tématu [vysvětlení souboru projektu](understanding-the-project-file.md). Podrobnější Úvod do souborů projektu a WPP najdete v tématu [uvnitř the Microsoft Build Engine: použití nástroje MSBuild a Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi a William Bartholomew, ISBN: 978-0-7356-4524-0.


## <a name="what-is-a-web-deployment-package"></a>Co je balíček nasazení webu?

Při sestavení a nasazení webové aplikace pomocí sady Visual Studio 2010 nebo přímo pomocí nástroje MSBuild, konečný výsledek je obvykle *balíčku pro nasazení webu*. Balíčku pro nasazení webu je soubor ZIP. Obsahuje všechno, co tuto službu IIS a nasazení webu potřebovat k znovu vytvořit webové aplikace, včetně:

- Kompilovaný výstup webové aplikace, včetně obsahu, soubory prostředků, konfigurační soubory, JavaScript a CSS prostředků šablon stylů CSS stylu a tak dále.
- Reference na sestavení pro projekt webové aplikace a pro všechny projekty v rámci vašeho řešení.
- Skripty SQL pro generování všechny databáze, které nasazujete s vaší webovou aplikací.

Po vygenerování balíčku pro nasazení webu je možné ji publikovat na webový server služby IIS různými způsoby. Například můžete nasadit ho vzdáleně cílí na vzdáleného agenta pro nasazení webu služby nebo obslužné rutiny webu nasadit na cílový webový server nebo správce služby IIS můžete použít k ručnímu importu balíček na cílový webový server. Další informace o těchto přístupů k nasazení najdete v tématu [výběr právo přístupu k nasazení webu](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md).

## <a name="how-does-the-build-process-work"></a>Jak funguje proces sestavení?

To ukazuje, co se stane, když sestavení a zabalení webové aplikace:

![](building-and-packaging-web-application-projects/_static/image2.png)

Při sestavování projektu webové aplikace, proces sestavení generuje soubor s názvem *[název projektu]. SourceManifest.xml*. Soubor projektu a výstup sestavení to *. SourceManifest.xml* soubor říká Webdeploy co je potřeba zahrnout do balíčku pro nasazení webu. Pomocí těchto vstupů, Web Deploy generuje balíčku pro nasazení webu s názvem *[název projektu] ZIP*.

Spolu s balíčku pro nasazení webu proces sestavení generuje dva soubory, které vám mohou pomoci při použití balíčku:

- *. Deploy.cmd* soubor obsahuje sadu nasazení webu (MSDeploy.exe) příkazy s parametry, které publikují balíčku pro nasazení webu na vzdáleného webového serveru služby IIS. Spuštění *. deploy.cmd* soubor s příslušnými parametry, obvykle poskytuje rychlejší a snazší alternativní k ruční vytváření MSDeploy.exe příkazy sami.
- *SetParameters.xml* soubor obsahuje sadu hodnot parametrů příkazu MSDeploy.exe. Tyto hodnoty patří vlastnosti, jako je název webové aplikace služby IIS, do které chcete nasadit balíček hodnoty žádné koncové body služby a připojovací řetězce definované v *web.config* soubor a všechny vlastnosti nasazení hodnoty definované na stránkách vlastností projektu.

*SetParameters.xml* soubor je klíčem ke správě procesu nasazení. Tento soubor je vygenerovaný dynamicky podle obsahu projektu webové aplikace. Například, pokud chcete přidat připojovací řetězec pro váš *web.config* souboru, proces sestavení bude automaticky rozpoznávat připojovací řetězec, proto parametrizovat nasazení a vytvořit položku v  *SetParameters.xml* souboru, aby bylo možné upravit připojovací řetězec jako součást procesu nasazení. Dalším tématu s názvem [konfigurace parametrů nasazení webového balíčku](configuring-parameters-for-web-package-deployment.md), vysvětluje roli tohoto souboru podrobněji a popisuje různé způsoby, ve kterém můžete upravit ho během sestavení a nasazení.

> [!NOTE]
> V sadě Visual Studio 2010 WPP nepodporuje předkompilaci stránek ve webové aplikaci před balení. Další verze sady Visual Studio a WPP bude zahrnovat možnost předkompilování webové aplikace jako možnost balení.


## <a name="conclusion"></a>Závěr

Toto téma poskytuje přehled o postupu sestavení a balení projektů webových aplikací v sadě Visual Studio 2010. Je popsáno, jak WPP umožňuje volat Web Deploy příkazy MSBuild, a to je vysvětleno o sestavení a balení procesu.

Po vytvoření balíčku pro nasazení webu, bude dalším krokem je její nasazení. Další informace najdete v části [konfigurace parametrů nasazení webového balíčku](configuring-parameters-for-web-package-deployment.md) a [nasazení webových balíčků](deploying-web-packages.md).

## <a name="further-reading"></a>Další čtení

Další témata v tomto kurzu [konfigurace parametrů nasazení webového balíčku](configuring-parameters-for-web-package-deployment.md) a [nasazení webových balíčků](deploying-web-packages.md), obsahují pokyny, jak používat webový balíček, který jste vytvořili. Posledním kurzu této série [pokročilé nasazení podnikového webu](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md), poskytuje pokyny o tom, jak přizpůsobit a řešení potíží s procesem vytváření balíčku.

Podrobnější Úvod do souborů projektu a WPP najdete v tématu [uvnitř the Microsoft Build Engine: použití nástroje MSBuild a Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi a William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Předchozí](understanding-the-build-process.md)
> [další](configuring-parameters-for-web-package-deployment.md)
