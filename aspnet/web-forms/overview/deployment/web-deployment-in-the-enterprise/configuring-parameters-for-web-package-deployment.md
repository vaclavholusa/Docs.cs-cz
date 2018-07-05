---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: Konfigurace parametrů nasazení webového balíčku | Dokumentace Microsoftu
author: jrjlee
description: Toto téma popisuje, jak nastavit hodnoty parametrů, jako jsou názvy webových aplikací Internetové informační služby (IIS), připojovacích řetězců a koncové body služby...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: e6db7a8351e01bbbc14eb2b993248ee7d5a84f7e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386546"
---
<a name="configuring-parameters-for-web-package-deployment"></a>Konfigurace parametrů nasazení webového balíčku
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje, jak nastavit hodnoty parametrů, jako jsou názvy webových aplikací Internetové informační služby (IIS), připojovacích řetězců a koncové body služby, při nasazení webového balíčku do vzdáleného webového serveru služby IIS.


Při sestavení projektu webové aplikace, sestavení a procesem vytváření balíčku generuje tři soubory klíčů:

- A *[název projektu] ZIP* souboru. Toto je balíčku pro nasazení webu pro váš projekt webové aplikace. Tento balíček obsahuje všechny sestavení, soubory, skripty databáze a prostředků potřebných pro opětovné vytvoření webové aplikace na vzdáleném serveru webové služby IIS.
- A *[název projektu].deploy.cmd* souboru. Tato položka obsahuje sadu nasazení webu (MSDeploy.exe) příkazy s parametry, které publikují balíčku pro nasazení webu na vzdáleného webového serveru služby IIS.
- A *[název projektu]. SetParameters.xml* souboru. To poskytuje sadu hodnot parametrů příkazu MSDeploy.exe. Můžete aktualizovat hodnoty v tomto souboru a předat ji pro nástroj nasazení webu jako parametr příkazového řádku při nasazení webového balíčku.

> [!NOTE]
> Další informace o sestavení a procesem vytváření balíčku naleznete v tématu [sestavení a balení projektů webových aplikací](building-and-packaging-web-application-projects.md).


*SetParameters.xml* souboru generuje dynamicky ze souboru projektu vaší webové aplikace a všechny konfigurační soubory v rámci svého projektu. Při sestavení a zabalení webové publikování kanálu (WPP) projektu budou automaticky zjišťovat velké množství proměnné, které se budou pravděpodobně měnit mezi prostředími nasazení, jako cíl služby IIS webová aplikace a připojovacích řetězců databáze. Tyto hodnoty jsou automaticky v balíčku pro nasazení webu s parametry a přidat do *SetParameters.xml* souboru. Například, pokud chcete přidat připojovací řetězec pro *web.config* soubor v projektu webové aplikace, proces sestavení tuto změnu zjistí a přidá záznam, tím *SetParameters.xml* souboru odpovídajícím způsobem.

V mnoha případech budou tato automatická Parametrizace dostatečné. Ale pokud vaši uživatelé potřebují k odlišení další nastavení mezi prostředími nasazení, jako je nastavení aplikace nebo adresy URL koncového bodu služby, budete muset říct WPP parametrizovat tyto hodnoty v balíčku pro nasazení a přidejte odpovídající položky *SetParameters.xml* souboru. Následující části popisují, jak to provést.

### <a name="automatic-parameterization"></a>Automatické Parametrizace

Při sestavení a zabalení webové aplikace, bude WPP automaticky parametrizovat Tyhle věci:

- Cíl služby IIS webová název a cesta k aplikaci.
- Žádné připojovací řetězce ve vaší *web.config* souboru.
- Připojovací řetězce pro všechny databáze, které přidáte **balení/publikování kódu SQL** karty na stránkách vlastností projektu.

Například, pokud chcete sestavit a zabalit [Správce kontaktů](the-contact-manager-solution.md) toto ukázkové řešení bez zásahu do procesu Parametrizace žádným způsobem, WPP vygenerují *ContactManager.Mvc.SetParameters.xml* souboru:


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]


V tomto případě:

- **Název webové aplikace IIS** parametrem je cesta služby IIS, ve které chcete nasadit webovou aplikaci. Výchozí hodnota je založena **balení/publikování webu** stránky na stránkách vlastností projektu.
- **ApplicationServices Web.config připojovací řetězec** parametru byla vygenerována z **connectionStrings nebo přidat** element v *web.config* souboru. Představuje připojovací řetězec, který by aplikace měla použít pro kontaktování databáze členství. Hodnota, které zadáte tady, se nahradí nasazených *web.config* souboru. Výchozí hodnota je převzata z před nasazením *web.config* souboru.

WPP parametrizuje také tyto vlastnosti v balíčku pro nasazení, který generuje. Při instalaci balíčku pro nasazení, můžete zadat hodnoty pro tyto vlastnosti. Pokud nainstalujete balíček ručně pomocí Správce služby IIS, jak je popsáno v [ruční instalace webových balíčků](manually-installing-web-packages.md), Průvodce instalací vás vyzve k zadání hodnot všech parametrů. Při instalaci balíčku vzdáleně pomocí *. deploy.cmd* sdílené, jak je popsáno v [nasazení webových balíčků](deploying-web-packages.md), nasazení webu bude vypadat na tuto *SetParameters.xml* do souboru Zadejte hodnoty parametrů. Můžete upravit hodnoty *SetParameters.xml* soubor ručně nebo ho můžete přizpůsobit jako součást automatizovaného procesu sestavení a nasazení. Tento proces je popsán dále v tomto tématu podrobněji.

### <a name="custom-parameterization"></a>Vlastní Parametrizace

Ve složitějších scénářích nasazení bude často potřeba parametrizovat další vlastnosti, před nasazením do projektu. Obecně řečeno by měl parametrizovat všechny vlastnosti a nastavení, která se liší mezi cílové prostředí. Můžou mezi ně patří:

- Koncové body v služby *web.config* souboru.
- Nastavení aplikace v *web.config* souboru.
- Všechny deklarativní vlastnosti, které chcete vyzvat uživatele k zadání.

Nejjednodušší způsob, jak parametrizovat těchto vlastností je přidat *parameters.xml* soubor do kořenové složky projektu webové aplikace. Například v řešení Správce kontaktů obsahuje projekt ContactManager.Mvc *parameters.xml* soubor v kořenové složce.

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

Pokud tento soubor otevřít, uvidíte, že obsahuje jediný **parametr** položka. Položka pomocí dotazu jazyka XML cesta (XPath) vyhledá a Parametrizace adresy URL koncového bodu služby ContactService Windows Communication Foundation (WCF) v *web.config* souboru.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]


Kromě Parametrizace adresu URL koncového bodu v balíčku pro nasazení, WPP přidá také odpovídající záznam do *SetParameters.xml* soubor, který získá vygenerována, společně s balíčku pro nasazení.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]


Pokud ručně nainstalovat balíček pro nasazení, Správce služby IIS zobrazí výzvu pro adresu koncového bodu služby vedle vlastnosti, které byly parametrizované automaticky. Pokud nainstalujete spuštěním balíčku pro nasazení *. deploy.cmd* soubor, můžete upravit *SetParameters.xml* souboru zadejte hodnotu pro adresu koncového bodu služby spolu s hodnotami pro vlastnosti, které byly automaticky parametrizovány.

Úplné podrobnosti o tom, jak vytvořit *parameters.xml* souborů naleznete v tématu [postupy: použití parametrů ke konfiguraci nastavení když balíček pro nasazení je nainstalován](https://msdn.microsoft.com/library/ff398068.aspx). Postup s názvem **používat parametry nasazení pro nastavení souboru Web.config** obsahuje podrobné pokyny.

## <a name="modifying-the-setparametersxml-file"></a>Úprava souboru SetParameters.xml

Pokud plánujete nasadit balíček webových aplikací ručně&#x2014;buď spuštěním *. deploy.cmd* souboru nebo spuštěním MSDeploy.exe z příkazového řádku&#x2014;není nutné nic vám ruční úpravy  *SetParameters.xml* souboru předcházející nasazení. Nicméně pokud pracujete v řešení podnikové úrovni, budete muset nasadit balíček webových aplikací jako součást větší, automatizovaného procesu sestavení a nasazení. V tomto scénáři budete potřebovat Microsoft Build Engine (MSBuild) Chcete-li změnit *SetParameters.xml* souboru za vás. Můžete to provést pomocí MSBuild **xmlpoke –** úloh.

[Ukázkové řešení Správce kontaktů](the-contact-manager-solution.md) tento proces. Chcete-li zobrazit podrobnosti, které jsou relevantní pro tento příklad se upravily příklady kódu, které následují.

> [!NOTE]
> Širší přehled modelu soubor projektu v ukázkovém řešení a úvod do projektu vlastní soubory v obecné, najdete v tématu [vysvětlení souboru projektu](understanding-the-project-file.md) a [Principy procesu sestavení](understanding-the-build-process.md).


Nejprve hodnoty parametrů, které vás zajímají jsou definovány jako vlastnosti v souboru projektu specifických pro prostředí (například *Env Dev.proj*).


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]


> [!NOTE]
> Pokyny k přizpůsobení souborů projektu specifických pro prostředí pro prostředí serveru, naleznete v tématu [nakonfigurovat vlastnosti nasazení pro cílové prostředí](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


Dále *Publish.proj* import souboru tyto vlastnosti. Protože každý *SetParameters.xml* je přidružené k souboru *. deploy.cmd* souboru a My trváme na soubor projektu pro každé vyvolání *. deploy.cmd* souboru projektu Vytvoří soubor MSBuild *položky* pro každou *. deploy.cmd* souboru a definuje vlastnosti, které vás zajímají jako *metadata položky*.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]


V tomto případě:

- **ParametersXml** metadat hodnota označuje umístění *SetParameters.xml* souboru.
- **IisWebAppName** hodnota je cesta služby IIS, do které chcete nasadit webovou aplikaci.
- **MembershipDBConnectionString** hodnotu připojovacího řetězce pro databázi členství a **MembershipDBConnectionName** hodnotu **název** atribut odpovídajícího parametru ve *SetParameters.xml* souboru.
- **ServiceEndpointValue** hodnota je adresa koncového bodu pro služby WCF na cílovém serveru a **ServiceEndpointParamName** hodnotu atributu název příslušného parametru v *SetParameters.xml* souboru.

Nakonec v *Publish.proj* soubor, **PublishWebPackages** cílit používá **xmlpoke –** úloh k úpravě tyto hodnoty *SetParameters.xml* souboru.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]


Můžete si všimnout, že každá **xmlpoke –** úloha určuje čtyři hodnoty atributů:

- **XmlInputPath** atribut oznamuje úkolu, kde najít soubor, který chcete upravit.
- **Dotazu** atribut je dotaz XPath, který identifikuje uzel XML, který chcete změnit.
- **Hodnotu** atribut je nová hodnota chcete vložit do vybraného uzlu XML.
- **Podmínku** atribut je kritéria, na kterých by měl spustit nebo nepoběží úkolu. V těchto případech podmínka zajistí, že není pokusíte vložit hodnotu null nebo prázdný do *SetParameters.xml* souboru.

## <a name="conclusion"></a>Závěr

Toto téma popisuje role *SetParameters.xml* souboru a vysvětlení, jak se vygeneruje, když vytváříte projekt webové aplikace. Je vysvětleno, jak můžete parametrizovat další nastavení tak, že přidáte *parameters.xml* soubor do projektu. Také popisuje, jak můžete upravit *SetParameters.xml* souboru jako součást procesu sestavení větší, automatizované, s použitím **xmlpoke –** úloh v souborech projektu.

Dalším tématu s názvem [nasazení webových balíčků](deploying-web-packages.md), popisuje, jak nasadit webový balíček buď spuštěním *. deploy.cmd* souboru nebo pomocí MSDeploy.exe příkazy přímo. V obou případech můžete zadat vaše *SetParameters.xml* soubor jako parametr nasazení.

## <a name="further-reading"></a>Další čtení

Informace o tom, jak vytvořit webových balíčků naleznete v tématu [sestavení a balení projektů webových aplikací](building-and-packaging-web-application-projects.md). Informace o tom, jak ve skutečnosti nasazení webového balíčku najdete v tématu [nasazení webových balíčků](deploying-web-packages.md). Podrobný názorný postup pro vytvoření *parameters.xml* souborů naleznete v tématu [postupy: použití parametrů ke konfiguraci nastavení když balíček pro nasazení je nainstalován](https://msdn.microsoft.com/library/ff398068.aspx).

Další obecné informace o parametrizaci v nasazení webu, naleznete v tématu [webové nasazení Parametrizace v akci](https://go.microsoft.com/?linkid=9805119) (příspěvek na blogu).

> [!div class="step-by-step"]
> [Předchozí](building-and-packaging-web-application-projects.md)
> [další](deploying-web-packages.md)
