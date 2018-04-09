---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: Řešení potíží s proces balení | Microsoft Docs
author: jrjlee
description: Toto téma popisuje, jak můžete shromáždit podrobné informace o vytváření balíčku pomocí vlastnosti EnablePackageProcessLoggingAndAssert v M...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 816ab77c44b52c6449a139475f2ef8546bd38071
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="troubleshooting-the-packaging-process"></a>Řešení potíží s proces balení
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje, jak můžete shromáždit podrobné informace o vytváření balíčku pomocí **EnablePackageProcessLoggingAndAssert** vlastnost v Microsoft Build Engine (MSBuild).
> 
> Když nastavíte **EnablePackageProcessLoggingAndAssert** vlastnost **true**, bude MSBuild:
> 
> - Přidejte další informace o vytváření balíčku do protokolů o sestavení.
> - Protokolování chyb za určitých podmínek, například pokud se v seznamu balení nacházejí duplicitní soubory.
> - Vytvořte adresář protokolu v *ProjectName*\_balíček složky a použít ho k zaznamenání informací o souborech jste balení.
> 
> Pokud se nedaří proces balení nebo balíčky pro nasazení vaší webové neobsahují soubory, které očekáváte, můžete použít tyto informace k řešení potíží s proces a pinpoint kam věcí nesprávný.
> 
> > [!NOTE]
> > **EnablePackageProcessLoggingAndAssert** vlastnost funguje jenom v případě, že vytvoříte projekt pomocí **ladění** konfigurace. Vlastnost je ignorován v ostatní konfigurace.


Toto téma je součástí ze série kurzů na základě kolem podnikové požadavky nasazení fiktivní společnost s názvem Fabrikam, Inc. Tento kurz řady používá ukázkové řešení&#x2014; [řešení obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s úrovní realistické složitější, včetně aplikace ASP.NET MVC 3, komunikaci Windows Služba Foundation (WCF) a projekt databáze.

Metoda nasazení jádrem tyto kurzy je založena na popsaný přístup souboru projektu rozdělení [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve které je řízené procesu sestavení dva soubory projektu&#x2014;jeden obsahující sestavení pokyny, které platí pro každé cílové prostředí a jeden, který obsahuje nastavení pro konkrétní prostředí sestavení a nasazení. V okamžiku sestavení souboru projektu konkrétní prostředí sloučeny do souboru projektu bez ohledu na prostředí a vytvořit úplnou sadu pokynů sestavení.

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a>Principy vlastnost EnablePackageProcessLoggingAndAssert

[Vytváření a projekty webových aplikací balení](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) popisuje, jak webové publikování kanálu (WPP) poskytuje sadu cíle MSBuild, které rozšiřují funkce nástroje MSBuild jej a povolte integraci s Internetové informační služby (IIS) webu Nástroj pro nasazení (Web Deploy). Když vytvoříte balíček projekt webové aplikace, se volání WPP cíle.

Velké množství těchto cílů WPP zahrnují podmíněnou logiku, která ukládá do protokolu Další informace při **EnablePackageProcessLoggingAndAssert** je nastavena na **true**. Například, pokud byste si projít **balíček** cíl, můžete zjistit, že vytvoří adresář dodatečných protokolů a zapisuje do textového souboru seznam souborů, pokud **EnablePackageProcessLoggingAndAssert** rovná **true**.


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]


> [!NOTE]
> Cíle WPP jsou definovány v *Microsoft.Web.Publishing.targets* soubor ve složce % PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web. Můžete otevřít tento soubor a zkontrolujte cíle v sadě Visual Studio 2010 nebo editoru XML. Je třeba dbát na změnit obsah souboru.


## <a name="enabling-the-additional-logging"></a>Povolení další protokolování

Můžete zadat hodnotu **EnablePackageProcessLoggingAndAssert** vlastnost různými způsoby v závislosti na tom, jak sestavit projekt.

Pokud vytvoříte projekt z příkazového řádku, můžete zadat hodnotu **EnablePackageProcessLoggingAndAssert** vlastnost jako argument příkazového řádku:


[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]


Pokud používáte vlastní projektu soubor vytvářet projekty, můžete zahrnout **EnablePackageProcessLoggingAndAssert** hodnotu **vlastnosti** atribut **MSBuild**úloh:


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]


Používáte-li vytvářet projekty definice sestavení Team Foundation Server (TFS), můžete zadat hodnotu **EnablePackageProcessLoggingAndAssert** vlastnost **argumenty MSBuild** řádek:![](troubleshooting-the-packaging-process/_static/image1.png)

> [!NOTE]
> Další informace o vytváření a konfiguraci definice sestavení, najdete v části [vytváření sestavení definice, podporuje nasazení](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).


Případně, pokud chcete zahrnout do každé sestavení balíčku, můžete upravit soubor projektu pro webový projekt aplikace nastavit **EnablePackageProcessLoggingAndAssert** vlastnost **true**. Měli byste přidat vlastnost prvního **PropertyGroup** element v souboru .csproj nebo .vbproj.


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]


## <a name="reviewing-the-log-files"></a>Kontrola soubory protokolu

Při sestavení a balíček projekt webové aplikace s **EnablePackageProcessLoggingAndAssert** nastavena na **true**, MSBuild vytvoří další složku s názvem přihlášení *ProjectName* \_Složky balíčku. Složka protokolu obsahuje různé soubory:

![](troubleshooting-the-packaging-process/_static/image2.png)

Seznam souborů, které jste se bude lišit podle věcí v projektu a vaše procesu sestavení. Tyto soubory jsou obvykle používány k zaznamenání seznam souborů, jako je shromažďování pro balení v různých fázích procesu:

- *PreExcludePipelineCollectFilesPhaseFileList.txt* jsou uvedené soubory, které shromažďuje MSBuild pro balení předtím, než se odeberou všechny soubory, které jsou určené pro vyloučení souborů.
- *AfterExcludeFilesFilesList.txt* soubor obsahuje seznam upravené soubor po odebrání všechny soubory, které jsou určené k vyloučení.

    > [!NOTE]
    > Další informace o vyloučení souborů a složek z procesu balení, najdete v části [vyloučení souborů a složek z nasazení](excluding-files-and-folders-from-deployment.md).
- *AfterTransformWebConfig.txt* souborů obsahuje soubory, které shromažďují pro balení po všech *Web.config* prováděly transformací. V tomto seznamu žádné specifické konfigurace *Web.config* transformace souborů, jako je třeba *Web.Debug.config* a *Web.Release.config*, jsou vyloučeny ze seznamu souborů pro balení. Jediný transformovat *Web.config* je součástí místo nich.
- *PostAutoParameterizationWebConfigConnectionStrings.txt* soubor obsahuje seznam souborů, které po připojovací řetězce v *Web.config* souboru mají byla parametry. Toto je proces, který umožňuje připojovací řetězce nahraďte správné nastavení pro cílové prostředí při nasazení balíčku.
- *Prepackage.txt* soubor obsahuje dokončené před sestavením seznam souborů, které mají být zahrnuty v balíčku.

> [!NOTE]
> Názvy souborů protokolu další obvykle odpovídají WPP cíle. Tyto cíle můžete zkontrolovat tak, že prověří *Microsoft.Web.Publishing.targets* soubor ve složce % PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web.


Pokud obsah webového balíčku nejsou, co jste očekávali, kontrola těchto souborů může být užitečný způsob, jak identifikovat v jaké bodu v procesu věcí došlo k chybě.

## <a name="conclusion"></a>Závěr

Toto téma popisuje, jak můžete použít **EnablePackageProcessLoggingAndAssert** vlastnost v nástroji MSBuild k řešení potíží během balení. Je popsané různé způsoby, ve kterém můžete zadat hodnotu vlastnosti do procesu sestavení a jeho popsané Další informace, které se zaznamená při nastavte vlastnost na **true**.

## <a name="further-reading"></a>Další čtení

Další informace o používání vlastních souborů projektu nástroje MSBuild k řízení procesu nasazení najdete v tématu [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) a [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Další informace o jako a jak se spravuje proces balení, najdete v části [budova a projekty webových aplikací balení](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md). Pokyny o tom, jak vyloučit konkrétní soubory a složky balíčky webového nasazení najdete v tématu [vyloučení souborů a složek z nasazení](excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Předchozí](running-windows-powershell-scripts-from-msbuild-project-files.md)
