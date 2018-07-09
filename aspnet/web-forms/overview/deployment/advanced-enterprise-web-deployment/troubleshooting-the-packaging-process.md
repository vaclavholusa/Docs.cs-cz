---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: Řešení potíží s procesem vytváření balíčku | Dokumentace Microsoftu
author: jrjlee
description: Toto téma popisuje, jak můžete pomocí vlastnosti EnablePackageProcessLoggingAndAssert v M shromáždit podrobné informace o proces vytváření balíčku...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: e262968ffb1f847e393e13b4e82d4f1a6029028e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802912"
---
<a name="troubleshooting-the-packaging-process"></a>Řešení potíží s procesem vytváření balíčku
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje, jak můžete shromažďovat podrobné informace o procesu vytváření balíčků pomocí **EnablePackageProcessLoggingAndAssert** vlastnost v Microsoft Build Engine (MSBuild).
> 
> Při nastavení **EnablePackageProcessLoggingAndAssert** vlastnost **true**, bude nástroj MSBuild:
> 
> - Přidejte další informace o procesu balení do protokolů o sestavení.
> - Protokolování chyb za určitých podmínek, například pokud duplicitních souborů se nenachází v seznamu balení.
> - Vytvořte adresář protokolů v *ProjectName*\_balíček složky a použít ho k zaznamenání informací o souborech, které jste balení.
> 
> Pokud proces vytváření balíčku se nedaří nebo balíčky pro nasazení vaší webové neobsahují soubory, které jste očekávali, můžete použít tyto informace k řešení potíží s procesem a pinpoint kde neuvádíme nesprávné.
> 
> > [!NOTE]
> > **EnablePackageProcessLoggingAndAssert** vlastnost funguje jenom v případě sestavení projektu pomocí **ladění** konfigurace. Vlastnost je ignorována v jiné konfigurace.


Toto téma je součástí série kurzů podle požadavků na nasazení enterprise fiktivní společnosti s názvem společnosti Fabrikam, Inc. V této sérii kurzů používá ukázkové řešení&#x2014; [řešení Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s realistické úroveň složitosti, včetně aplikace ASP.NET MVC 3, komunikace Windows Služba Foundation (WCF) a databázový projekt.

Metody nasazení v srdci těchto kurzů je založen na rozdělení přístupu soubor projektu je popsáno v [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve které je řízena procesem sestavení dva soubory projektu&#x2014;jeden obsahující pokyny, které platí pro všechny cílové prostředí a jeden obsahuje nastavení pro konkrétní prostředí sestavení a nasazení pro sestavení. V okamžiku sestavení souboru projektu specifických pro prostředí se sloučí do souboru projektu bez ohledu na prostředí a vytvoří kompletní sadu pokynů sestavení.

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a>Principy vlastnost EnablePackageProcessLoggingAndAssert

[Sestavení a balení projektů webových aplikací](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) popsané, jak webové publikování kanálu (WPP) poskytuje sadu cílů MSBuild, které rozšiřují funkce nástroje MSBuild a umožňuje integraci s webovým rozhraním Internetové informační služby (IIS) Nástroj pro nasazení (Web Deploy). Když vytvoříte balíček projektu webové aplikace, volání WPP cíle.

Spoustu těchto cílů WPP zahrnují podmíněnou logiku, která ukládá do protokolu Další informace při **EnablePackageProcessLoggingAndAssert** je nastavena na **true**. Například, když se podíváte **balíčku** cíl, můžete vidět, že vytvoří adresář služby dalších protokolů a zapíše seznam souborů do textového souboru, pokud **EnablePackageProcessLoggingAndAssert** rovná **true**.


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]


> [!NOTE]
> WPP cíle, které jsou definovány v *Microsoft.Web.Publishing.targets* souboru ve složce % PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web. Můžete otevřít tento soubor a zkontrolujte cílů v sadě Visual Studio 2010 nebo editoru XML. Zajistíme, abyste neupravovali obsah souboru.


## <a name="enabling-the-additional-logging"></a>Povolení dodatečné protokolování

Můžete zadat hodnotu **EnablePackageProcessLoggingAndAssert** vlastnost různými způsoby v závislosti na tom, jak svůj projekt sestavit.

Pokud se sestavení projektu z příkazového řádku můžete zadat hodnotu **EnablePackageProcessLoggingAndAssert** vlastnost jako argument příkazového řádku:


[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]


Pokud používáte soubor vlastních projektů k sestavení projektů, můžete zahrnout **EnablePackageProcessLoggingAndAssert** hodnotu **vlastnosti** atribut **MSBuild**úloh:


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]


Pokud používáte definici sestavení Team Foundation Server (TFS) k sestavení projektů, můžete zadat hodnotu **EnablePackageProcessLoggingAndAssert** vlastnost **argumenty nástroje MSBuild** řádek:![](troubleshooting-the-packaging-process/_static/image1.png)

> [!NOTE]
> Další informace o vytváření a konfiguraci definic sestavení, naleznete v tématu [vytvoření sestavení definice, že podporuje nasazení](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).


Případně, pokud chcete, aby zahrnovaly balíček v každém sestavení, můžete upravit soubor projektu pro webový projekt aplikace nastavit **EnablePackageProcessLoggingAndAssert** vlastnost **true**. Měli byste přidat vlastnost na první **PropertyGroup** element v souboru .csproj nebo .vbproj.


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]


## <a name="reviewing-the-log-files"></a>Kontrola souborů protokolu

Při sestavení a zabalení webové aplikace s **EnablePackageProcessLoggingAndAssert** nastavena na **true**, MSBuild vytvoří další složku s názvem protokolu v *ProjectName* \_Složky balíčku. Složka protokolu obsahuje různé soubory:

![](troubleshooting-the-packaging-process/_static/image2.png)

Seznam souborů, které jste se bude lišit podle věci v projektu a procesu sestavení. Tyto soubory jsou obvykle používá k zaznamenání seznamu souborů, které shromažďuje WPP balení v různých fázích procesu:

- *PreExcludePipelineCollectFilesPhaseFileList.txt* soubor obsahuje seznam souborů, které shromažďuje MSBuild pro vytváření balíčků před odstraněním všechny soubory, které jsou určené pro vyloučení.
- *AfterExcludeFilesFilesList.txt* soubor obsahuje seznam upravený soubor po odebrání všechny soubory, které jsou určené pro vyloučení.

    > [!NOTE]
    > Další informace o vyloučení souborů a složek z procesem vytváření balíčku najdete v tématu [vyloučení souborů a složek z nasazení](excluding-files-and-folders-from-deployment.md).
- *AfterTransformWebConfig.txt* soubor seznamy pro balení po všech shromážděných souborů *Web.config* transformace byly provedeny. V tomto seznamu všech určených pro konfigurace *Web.config* transformační soubory, jako je třeba *Web.Debug.config* a *Web.Release.config*, nezahrnou se seznam souborů pro balení. Jediný transformuje *Web.config* je součástí jejich místo.
- *PostAutoParameterizationWebConfigConnectionStrings.txt* soubor obsahuje seznam souborů, které po připojovací řetězce v *Web.config* soubor mít parametrizované. To je proces, který umožňuje při nasazení balíčku nahradit připojovací řetězce správné nastavení pro cílové prostředí.
- *Prepackage.txt* soubor obsahuje dokončené před sestavením seznam souborů, které mají být součástí balíčku.

> [!NOTE]
> Názvy souborů dalších protokolů obvykle odpovídají WPP cíle. Můžete zkontrolovat tyto cíle prozkoumáním *Microsoft.Web.Publishing.targets* souboru ve složce % PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web.


Pokud obsah webového balíčku se, co jste očekávali, kontrola těchto souborů může být užitečný způsob, jak identifikovat na jaké bodu ve věci procesu došlo k chybě.

## <a name="conclusion"></a>Závěr

Toto téma popisuje, jak můžete použít **EnablePackageProcessLoggingAndAssert** vlastnost MSBuild k řešení potíží s procesem vytváření balíčku. Vysvětlení různých způsobů, ve kterém můžete zadat hodnotu vlastnosti, aby proces sestavení, a popisuje další informace, které zaznamenává, když nastavíte vlastnost na **true**.

## <a name="further-reading"></a>Další čtení

Další informace o používání vlastní soubory projektu MSBuild k řízení procesu nasazení najdete v tématu [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) a [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Další informace o WPP a jak spravuje proces vytváření balíčku naleznete v tématu [sestavení a balení projektů webových aplikací](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md). Informace o tom, jak vyloučit určité soubory a složky z balíčky nasazení webu najdete v tématu [vyloučení souborů a složek z nasazení](excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Předchozí](running-windows-powershell-scripts-from-msbuild-project-files.md)
