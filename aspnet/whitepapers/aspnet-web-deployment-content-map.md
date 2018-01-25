---
uid: whitepapers/aspnet-web-deployment-content-map
title: "Nasazení webu ASP.NET - doporučené prostředky | Microsoft Docs"
author: rick-anderson
description: "Toto téma obsahuje odkazy na dokumentaci (publikovat) technologie ASP.NET web zdroje informací o tom, jak nasadit aplikace do služby IIS pomocí Visual Studio 2010, Visual Web De..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2014
ms.topic: article
ms.assetid: 58b583cd-c4ab-47a3-8527-8c92c298c91f
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-web-deployment-content-map
msc.type: content
ms.openlocfilehash: 78ff183394b5ff92f789b50551d01d28f9bff93b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-deployment---recommended-resources"></a>Nasazení webu ASP.NET - doporučené prostředky
====================
> Toto téma obsahuje odkazy na dokumentaci (publikovat) technologie ASP.NET web zdroje informací o tom, jak nasadit aplikace do služby IIS pomocí sady Visual Studio 2010, Visual Web Developer 2010 a novější verze.
> 
> Pokud znáte skvělé blogu, [stackoverflow](http://stackoverflow.com) vlákno nebo jiných odkaz, který by být užitečné, [pošlete nám e-mail](mailto:aspnetue@microsoft.com?subject=Deployment Content Map) s odkazem.
> 
> > [!NOTE] 
> > 
> > Mnoho z těchto informací popisují nasazení funkce, které jsou k dispozici pouze v případě, že instalujete nejnovější verzi [aktualizace Visual Studio Web publikování](https://go.microsoft.com/fwlink/?LinkID=208120). Některé funkce jsou k dispozici pouze v sadě Visual Studio 2012 nebo Visual Studio 2013.


Toto téma obsahuje následující oddíly:

- [Seznámení s možnostmi nasazení pro webové projekty](#understanding)
- [Hledání hostování zprostředkovatele pro aplikace ASP.NET](#findinghosting)
- [Nasazení webové aplikace z Visual Studia](#fromvs)
- [Nasazení webové aplikace pomocí vytvoření a instalace balíčku pro nasazení webu](#package)
- [Nasazení webové aplikace pomocí procesu průběžnou integraci (CI)](#ci)
- [Chcete-li změnit nastavení v cílovém souboru Web.config nebo app.config souboru během nasazení použití transformace Web.config](#transforms)
- [Chcete-li změnit nastavení v cílové webové aplikace v průběhu nasazení pomocí nástroje nasazení webu parametrů](#webdeployparms)
- [Zajistit, že aplikace je offline během nasazení](#appoffline)
- [Nasazení do databáze nebo změny k databázi jako součást nasazení webových aplikací](#databasewithweb)
- [Nasazení databáze odděleně od nasazení webových aplikací](#databaseseparate)
- [Nasazení webové aplikace, která používá aplikace ASP.NET službami, jako je členství a profilace](#aspnetmembership)
- [Předkompilace pro nasazení](#precompiling)
- [Nasazení webové aplikace služby intranetu](#intranet)
- [Automatizaci nejčastějších úloh nasazení, které nejsou automatizované mimo pole](#automating)
- [Konfigurace webové servery, abyste vývojáři mohli nasadit webové aplikace na jejich pomocí nástroje nasazení webu](#configuringservers)
- [Konfigurace serverů pro poskytovatele hostingu](#hostingprovider)
- [Řešení potíží nasazení](#troubleshooting)
- [Získání nápovědy s otázkou konkrétní nasazení](#gettinghelp)
- [Další zdroje informací](#additional)


<a id="understanding"></a>


## <a name="understanding-deployment-options-for-web-projects"></a>Seznámení s možnostmi nasazení pro webové projekty

- [Přehled nasazení pro Visual Studio a ASP.NET Web](https://msdn.microsoft.com/library/dd394698.aspx) (MSDN).
- [Postup nasazení webu systému Windows Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Vysvětluje možnosti a odkazy na prostředky pro nasazení webové projekty pro weby systému Windows Azure, včetně [nastavené průběžné doručování](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) (automatizované z [ovládací prvek zdroje](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)) a také pomocí sady Visual Studio.
- [Visual Studio 2012 Web publikování vylepšení](../visual-studio/overview/2012/visual-studio-2012-web-publishing-improvements.md) (Video Scotta Hanselmana, kde).
- [Přehled Post pro nasazení webu v VS 2010](http://vishaljoshi.blogspot.com/2009/09/overview-post-for-web-deployment-in-vs.html) (Vishal Joshi blog). Starší příspěvku blogu, ale některé prostředky, Visual Studio 2010 propojí se získat informace, které jsou stále relevantní pro sadu Visual Studio 2012.


<a id="findinghosting"></a>


## <a name="finding-hosting-providers-for-an-aspnet-application"></a>Hledání hostování zprostředkovatele pro aplikace ASP.NET

- [Hostování prostředí ASP.NET](https://asp.net/hosting)


<a id="fromvs"></a>


## <a name="deploying-a-web-application-from-visual-studio"></a>Nasazení webové aplikace z Visual Studia

- [Postup nasazení webu systému Windows Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Popisuje možnosti a poskytuje odkazy na zdroje informací pro webové projekty nasazení pro weby systému Windows Azure. Obsahuje části o nasazení ze sady Visual Studio.
- [Nasazení webu ASP.NET pomocí sady Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 12 část kurzu řady, ukazuje, jak nasadit webové aplikace s databází serveru SQL Server. Pro databázi nasazení používá poskytovatele dbDacFx i migrace Entity Framework Code First. Zahrnuje taky informace o [transformace souboru Web.config](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md), [nasazení jednotlivých souborů](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md#specificfiles), [příkazového řádku nasazení](../web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment.md), a [postup Přizpůsobení webu Visual Studio kanálu publikování úpravou .pubxml souborů](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md). Platí pro všechny webové projekty ASP.NET, včetně webových formulářů, MVC a webového rozhraní API).
- [Postupy: nasazení webového projektu pomocí publikování jedním kliknutím v sadě Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx) (referenční informace o průvodci Visual Studio Web Publish.)
- [Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). Toto je dřívější verzi **nasazení webu ASP.NET pomocí sady Visual Studio** uvedené na začátku této části. Užitečné hlavně teď informace o tom, jak nasadit systém SQL Server Compact databáze a způsob migrace ze systému SQL Server Compact na plnou verzi systému SQL Server.
- [Rozhraní .NET vícevrstvé aplikace pomocí úložiště tabulky, fronty a objekty BLOB](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (Microsoft Azure site). kurz řady část 5, ukazuje, jak vytvořit projekt MVC a nasadíte ho do cloudové služby Windows Azure.


<a id="package"></a>
## <a name="deploying-a-web-application-by-creating-and-installing-a-web-deployment-package"></a>Nasazení webové aplikace pomocí vytvoření a instalace balíčku pro nasazení webu

- [Postupy: vytvoření balíčku pro nasazení webu v sadě Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx) (MSDN).
- [Postupy: instalace balíčku pro nasazení pomocí souboru deploy.cmd vytvořili pomocí sady Visual Studio](https://msdn.microsoft.com/library/ff356104.aspx) (MSDN).
- [Pomocí balíčku pro nasazení webu k nasazení do služby IIS na pole vývojářů a na třetí strany hostitele](http://sedodream.com/2011/11/08/UsingAWebDeployPackageToDeployToIISOnTheDevBoxAndToAThirdPartyHost.aspx) (blog Sayed Hashimi). Jak používat Správce služby IIS k instalaci balíčku pro nasazení ve službě IIS v místním počítači a v hostování společnosti, který podporuje správce služby IIS pro vzdálenou správu.
- [Vytváření webové nasazení balíčku ze sady Visual Studio 2010](https://www.iis.net/learn/publish/using-web-deploy/building-a-web-deploy-package-from-visual-studio-2010) (IIS.NET webový server). Obsahuje pokyny pro vytvoření balíčku příkazového řádku a instalaci.
- [Balíček po publikování odkudkoli](http://sedodream.com/2012/03/14/PackageWebUpdatedAndVideoBelow.aspx) (blog Sayed Hashimi). Představuje balíčku NuGet, který automatizuje proces transformace souboru Web.config pro cílové prostředí s více, abyste mohli nasadit jeden balíček pro více serverů. Viz také [PackageWeb video](https://www.youtube.com/watch?v=-LvUJFI8CzM) podle Sayed Hashimi.

Další informace najdete v části v následující části.


<a id="ci"></a>


## <a name="deploying-a-web-application-using-a-continuous-integration-ci-process"></a>Nasazení webové aplikace pomocí procesu průběžnou integraci (CI)

- [Průběžnou integraci a nastavené průběžné doručování (vytváření reálných cloudových aplikací pomocí služby Windows Azure).](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) Elektronická kniha kapitoly, který představuje průběžnou integraci a nastavené průběžné doručování.
- [Postup nasazení webu systému Windows Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Vysvětluje možnosti a odkazy na prostředky pro nasazení webové projekty pro weby systému Windows Azure. Obsahuje části o automatizaci nasazení od správy zdrojového kódu.
- [Nasazení webové aplikace v podnikové scénáře](../web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). kurz řady 40 část, ukazuje, jak automatizovat nasazení v procesu CI pomocí sady Visual Studio 2010 a Team Foundation Server 2010.
- [Uvnitř stroje Microsoft Build Engine: pomocí nástroje MSBuild a Team Foundation Build Sayed Hashimi a William Bartholomew](http://msbuildbook.com). To je knihy, není webové prostředky, ale je základní Průvodce pro naučit, jak nakonfigurovat MSBuild pro scénáře nepřetržité integrace.
- [Rozšíření MSBuild Pack](https://github.com/mikefourie/MSBuildExtensionPack). Zahrnuje úlohy nasazení.
- [Team Foundation Build přizpůsobení průvodce](https://aka.ms/vsarsolutions). Dokumentace pomocí ALM Rangers na nastavení služby Team Foundation Server popisuje nasazení webu a zahrnuje kurzy a videa.
- [Transformuje SlowCheetah XML ze serveru CI](http://sedodream.com/2011/12/12/SlowCheetahXMLTransformsFromACIServer.aspx) (blog Sayed Hashimi). Vysvětluje, jak používat SlowCheetah A Visual Studio add-in pro převod app.config a další soubory XML.

Viz také [zajistit, že aplikace je offline během nasazení](aspnet-web-deployment-content-map.md#appoffline) dál v této stránce.


<a id="transforms"></a>


## <a name="using-webconfig-transformations-to-change-settings-in-the-destination-webconfig-file-or-appconfig-file-during-deployment"></a>Chcete-li změnit nastavení v cílovém souboru Web.config nebo app.config souboru během nasazení použití transformace Web.config

- [Soubor Web.config transformace](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md).
- [Syntaxe transformace Web.config pro nasazení webového projektu pomocí sady Visual Studio](https://msdn.microsoft.com/library/dd465326.aspx) (MSDN).
- [Webové nástroje 2012.2 - transformace web.config](https://www.youtube.com/watch?v=HdPK8mxpKEI) (video na YouTube podle Sayed Hashimi). Ukazuje, jak nastavit a náhled transformace Web.config.
- [Jakým způsobem vypnout transformace Web.config?](https://msdn.microsoft.com/library/ee942158.aspx#disable_web_config_transformation) (MSDN).
- [Kdy použít Web Deploy parametry místo transformace Web.config?](https://msdn.microsoft.com/library/ee942158.aspx#web_deploy_parameters) (MSDN).
- [XDT (transformace dokumentů XML) vydala codeplex.com](https://blogs.msdn.com/b/webdev/archive/2013/04/23/xdt-xml-document-transform-released-on-codeplex-com.aspx) (blog vývoj webových rozhraní .NET a nástroje). Oznámí dostupnost zdrojového kódu pro transformační modul soubor Web.config a uvádí některé nástroje, které ho používají.
- [Weby Azure Windows: Jak řetězců aplikace a připojovacích řetězců](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (blog Microsoft Azure). Transformuje alternativu k souboru Web.config, pokud je prostředí cílové weby systému Windows Azure a chcete k transformaci `appSettings` nebo `connectionStrings`.


<a id="webdeployparms"></a>


## <a name="using-web-deploy-parameters-to-change-settings-in-the-destination-web-application-during-deployment"></a>Chcete-li změnit nastavení v cílové webové aplikace v průběhu nasazení pomocí nástroje nasazení webu parametrů

- [Postupy: použití Web Deploy parametry v balíčku pro nasazení webu](https://msdn.microsoft.com/library/ff398068.aspx) (MSDN).
- [MSDeploy: Jak se bude aktualizovat nastavení aplikace na publikování založená na profilu publikování](http://sedodream.com/2013/03/02/MSDeployHowToUpdateAppSettingsOnPublishBasedOnThePublishProfile.aspx) (blog Sayed Hashimi). Ukazuje, jak integrovat nasazení webu parametry do sady Visual Studio publikační profily.
- [Web nasazení Parametrizace](https://www.iis.net/learn/publish/using-web-deploy/web-deploy-parameterization) (IIS.NET webový server).
- [Web nasazení Parametrizace v akci](http://vishaljoshi.blogspot.com/2010/07/web-deploy-parameterization-in-action.html) (Vishal Joshi blog).
- [Webové nasazení Parametrizace vs. Transformace Web.config](http://vishaljoshi.blogspot.com/2010/06/parameterization-vs-webconfig.html) (Vishal Joshi blog).
- [Weby Azure Windows: Jak řetězců aplikace a připojovacích řetězců](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (blog Microsoft Azure). Alternativu k webové nasazení parametrů, pokud je prostředí cílové weby systému Windows Azure a chcete Parametrizace `appSettings` nebo `connectionStrings`.


<a id="appoffline"></a>


## <a name="making-sure-an-application-is-off-line-during-deployment"></a>Zajistit, že aplikace je offline během nasazení

- [Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení aktualizace kódu](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Najdete v části **trvat aplikace do režimu offline během nasazení.**
- [Převedení aplikace do režimu Offline před publikováním](https://www.iis.net/learn/publish/deploying-application-packages/taking-an-application-offline-before-publishing) (IIS.net lokalita). Popisuje funkce integrovaná do nasazení webu 3.0 umožňuje automatizovat zpracování aplikace\_offline.htm souboru. Tato funkce nefunguje s vlastní aplikaci\_offline.htm soubory.
- [Jak vytvořit webovou aplikaci do offline režimu během publikování](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx) (blog Sayed Hashimi). Jak automatizovat proces pomocí vlastní aplikace\_offline.htm souboru.
- [Webové publikování aktualizací pro aplikaci do režimu offline a usechecksum](https://blogs.msdn.com/b/webdev/archive/2013/10/30/web-publishing-updates-for-app-offline-and-usechecksum.aspx) (blog vývoj webů společnosti Microsoft). Další možností pro automatizaci použití aplikace\_offline.htm souboru.
- [Web nasazení RTW 3.5](https://blogs.iis.net/msdeploy/archive/2013/07/09/webdeploy-3-5-rtw.aspx) (IIS.net lokalita). Nová funkce v 3.5 nasadit Web pro vlastní aplikaci\_offline.htm soubory.


<a id="databasewithweb"></a>


## <a name="deploying-a-database-or-changes-to-a-database-as-part-of-web-application-deployment"></a>Nasazení do databáze nebo změny k databázi jako součást nasazení webových aplikací

- [Konfigurace nasazení databáze v sadě Visual Studio](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment) (MSDN). Přehled možností pro nasazení databáze s webového projektu.
- [Nasazení webu ASP.NET pomocí sady Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 12 část kurzu řady, zobrazuje nasazení databáze pomocí poskytovatele dbDacFx a migrace Entity Framework Code First.
- [Postupy: nasazení webového projektu pomocí jedním kliknutím publikovat v sadě Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx) (MSDN).
- [Nasazení aplikace zabezpečené ASP.NET MVC 5 s členství, OAuth a databáze SQL na webu systému Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Dlouhé kurz, který vytvoří a nasadí aplikaci, která používá jeden Server SQL databáze pro data aplikací a členství v aplikaci.
- [Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). 12 část kurzu řady, ukazuje, jak nasadit systém SQL Server Compact databáze a způsob migrace ze systému SQL Server Compact na plnou verzi systému SQL Server.

Viz také nasazení webové aplikace pomocí vytvoření a instalaci balíčku pro nasazení webu a nasazení webové aplikace pomocí procesu průběžnou integraci (CI) dříve na této stránce.


<a id="databaseseparate"></a>


## <a name="deploying-a-database-separately-from-web-application-deployment"></a>Nasazení databáze odděleně od nasazení webových aplikací

- [Nástroje SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686(v=vs.103).aspx) (MSDN).
- [Včetně dat v projektu databáze serveru SQL](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx) (blogu týmu nástroje SQL Server Data Tools). Postup nasazení schématu i dat při nasazení databáze.
- [Postup nasazení databáze do systému Windows Azure](https://docs.microsoft.com/azure/sql-database/sql-database-cloud-migrate) (Microsoft Azure site)
- [Migrace databáze do systému Windows Azure SQL Database (dříve SQL Azure)](https://msdn.microsoft.com/library/windowsazure/ee730904.aspx) (MSDN).
- [Migrace databáze do Azure SQL pomocí rozšíření SSDT](https://blogs.msdn.com/b/ssdt/archive/2012/04/19/migrating-a-database-to-sql-azure-using-ssdt.aspx) (blogu týmu nástroje SQL Server Data Tools).
- [Migrace na střed Data aplikací do systému Windows Azure](https://msdn.microsoft.com/library/jj156154.aspx) (MSDN).
- [Migrace databáze systému SQL Server do systému Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).


<a id="aspnetmembership"></a>


## <a name="deploying-a-web-application-that-uses-aspnet-application-services-such-as-membership-and-profiling"></a>Nasazení webové aplikace, která používá aplikace ASP.NET službami, jako je členství a profilace

- [Nasazení aplikace zabezpečené ASP.NET MVC 5 s členství, OAuth a databáze SQL na webu systému Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Dlouhé kurz, který vytvoří a nasadí aplikaci, která používá jeden Server SQL databáze pro data aplikací a členství v aplikaci.
- [ASP.NET Identity](https://asp.net/identity/). Prostředky pro identitu ASP.NET.
- [Nasazení webu ASP.NET pomocí sady Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 12 část kurzu řady, ukazuje, jak nasadit databáze členství technologie ASP.NET.
- [Konfigurace webu, který používá aplikačních služeb](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs.md). Pro web projekty, ale je také důležité pro projekty webových aplikací.
- [Uživatelé a role na webu produkční](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs.md). Pro web projekty, ale je také důležité pro projekty webových aplikací.


<a id="precompiling"></a>


## <a name="precompiling-for-deployment"></a>Předkompilace pro nasazení

- [ASP.NET – webové aplikace projektu předkompilace přehled](https://msdn.microsoft.com/library/aa983464.aspx) (MSDN).
- [Balení/publikování karta Web, vlastnosti projektu](https://msdn.microsoft.com/library/dd410108.aspx) (MSDN).
- [Rozšířené předkompilovat dialogové okno nastavení](https://msdn.microsoft.com/library/hh475319.aspx) (MSDN).


<a id="intranet"></a>


## <a name="deploying-an-intranet-web-application"></a>Nasazení webové aplikace služby intranetu

- [Použijte možnost organizační ověřování místní (služby AD FS) pomocí technologie ASP.NET v sadě Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/) (Blog podle Vittorio Bertocci.).
- [Postup vytvoření intranetový server pomocí technologie ASP.NET MVC](https://msdn.microsoft.com/library/gg703322(VS.98).aspx) (MSDN). Starší writen návod pro Visual Studio 2010, nemusí odpovídat většími změnami v intranetu šablony projektů zavedená v sadě Visual Studio 2013.


<a id="automating"></a>


## <a name="automating-common-deployment-tasks-that-are-not-automated-out-of-the-box"></a>Automatizaci nejčastějších úloh nasazení, které nejsou automatizované mimo pole

- [Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení dalších souborů](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md).
- [Nastavení oprávnění ke složkám na publikování webu](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) (blog Sayed Hashimi).
- [Postup rozšíření souboru cíle nastavení registru pro balíček webového projektu](https://blogs.msdn.com/webdevtools/archive/2010/02/09/how-to-extend-target-file-to-include-registry-settings-for-web-project-package.aspx) (blog nástrojů Web Development Tools).
- [Rozšíření XML (soubor Web.config) transformaci](http://sedodream.com/2010/09/09/ExtendingXMLWebconfigConfigTransformation.aspx) (blog Sayed Hashimi). Ukazuje, jak vytvořit vlastní XDT transformací.
- [Webové nástroje pro nasazení (MSDeploy) vlastní zprostředkovatele trvat 1](http://sedodream.com/2010/03/11/WebDeploymentToolMSDeployCustomProviderTake1.aspx) (blog Sayed Hashimi). Ukazuje, jak vytvořit vlastního poskytovatele nasazení webu.
- [Balíček a nasazení komponent COM](https://blogs.msdn.com/webdevtools/archive/2010/03/03/how-to-package-and-deploy-com-component.aspx) (blog nástrojů Web Development Tools).
- [Postup sestavení .NET balíček](https://blogs.msdn.com/webdevtools/archive/2010/02/19/how-to-package-com-component.aspx) (blog nástrojů Web Development Tools). Postup nasazení sestavení do mezipaměti GAC.
- [Skript všechno - inicializovat si virtuální počítač Windows Azure pro váš webový Server služby IIS, nástroje nasazení webu a další vlastní položky](http://www.tugberkugurlu.com/archive/script-out-everything-initialize-your-windows-azure-vm-for-your-web-server-with-iis-web-deploy-and-other-stuff) (blog Tugberk Ugurlu).


<a id="configuringservers"></a>


## <a name="configuring-web-servers-so-that-developers-can-deploy-web-applications-to-them-using-web-deploy"></a>Konfigurace webové servery, abyste vývojáři mohli nasadit webové aplikace na jejich pomocí nástroje nasazení webu

- [Instalace a konfigurace nasazení webu pro správce a bez oprávnění správce nasazení](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy) (IIS.net lokalita).


<a id="hostingprovider"></a>


## <a name="configuring-servers-for-a-hosting-provider"></a>Konfigurace serverů pro poskytovatele hostingu

- [Průvodce nasazením hostování technologii Microsoft ASP.NET 4](https://go.microsoft.com/fwlink/?LinkId=191365) (Microsoft Download Center).
- [Generování souboru profilu XML](https://www.iis.net/learn/web-hosting/joining-the-web-hosting-gallery/generate-a-profile-xml-file) (IIS.net lokalita).


<a id="troubleshooting"></a>


## <a name="troubleshooting-deployment-problems"></a>Řešení potíží nasazení

- [Řešení potíží s weby systému Windows Azure v sadě Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) (Microsoft Azure site).
- [Nasazení webu ASP.NET pomocí sady Visual Studio: řešení potíží s](../web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting.md).
- [Odstraňování běžných problémů s webové nasazení](https://www.iis.net/learn/publish/troubleshooting-web-deploy/troubleshooting-common-problems-with-web-deploy).
- [Kódy chyb nasazení webové](https://www.iis.net/learn/publish/troubleshooting-web-deploy/web-deploy-error-codes) (IIS.net lokalita).
- [Nejčastější dotazy k nasazení pro Visual Studio a ASP.NET Web](https://msdn.microsoft.com/library/ee942158.aspx) (MSDN).
- [Základní rozdíly mezi službou IIS a ASP.NET Development Server](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md).
- [Běžné konfigurace rozdíly mezi vývojovým týmem a produkční](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs.md).
- [Hostování aplikace ASP.NET na úrovni Medium Trust](http://www.4guysfromrolla.com/articles/100307-1.aspx) (4 nepřetržitého z Rolla lokality).


<a id="gettinghelp"></a>


## <a name="getting-help-with-a-specific-deployment-question"></a>Získání nápovědy s otázkou konkrétní nasazení

- [Fórum pro nasazení a konfigurace ASP.NET](https://forums.asp.net/26.aspx/1?Configuration and Deployment).
- [StackOverflow.com](http://www.StackOverflow.com).


<a id="additional"></a>


## <a name="additional-resources"></a>Další prostředky

Tato část obsahuje odkazy na další zdroje, které jsou užitečné pro dozvědět více o tom, jak používat nástroje Visual Studio a IIS nasazení.

V těchto blozích často obsahují informace o nasazení webu Visual Studio:

- [Webové nástroje pro vývoj v blogu Microsoft](https://blogs.msdn.com/b/webdevtools/).
- [Blog SAYED Hashimi](http://www.sedodream.com/).

Následující zdroje poskytují dokumentaci o službě Web Deploy, rozhraní IIS, které Visual Studio používá k provedení úlohy nasazení projektu webové aplikace. Můžete klást otázky o službě Web Deploy v [nástroj pro nasazení webu fórum](https://go.microsoft.com/fwlink/?LinkId=149411) na webu IIS.net.

- [Úvod do webového nasazení](https://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy).
- [Instalace a konfigurace webové nasazení](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy).
- [Skripty prostředí PowerShell pro automatizaci webové nasadit nastavení](https://www.iis.net/learn/publish/using-web-deploy/powershell-scripts-for-automating-web-deploy-setup).
- [Webové nástroje pro nasazení](https://go.microsoft.com/fwlink/?LinkId=151481). Tabulce nejvyšší úrovně uzlu obsah pro nasazení webu dokumentace na webu TechNet. Obsahuje užitečné referenční informace, ale většina TechNet stránky nebyly aktualizovány let.
- [Namespace Microsoft.Web.Deployment](https://go.microsoft.com/fwlink/?LinkId=148630). Dokumentace rozhraní API, zatím není aktualizovaná od verze 1.0.
- [Blog týmu pro nasazení webové aplikace Microsoft](https://blogs.iis.net/msdeploy/default.aspx).
- [IIS.net webu publikovat karta](https://www.iis.net/learn/publish).
