---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: "Konfigurace parametrů pro nasazení webového balíčku | Microsoft Docs"
author: jrjlee
description: "Toto téma popisuje, jak nastavit hodnoty parametrů, jako jsou názvy webových aplikací Internetové informační služby (IIS), připojovací řetězce a koncové body služby..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: dd1ae266740ea4728c0624b1833a98ac262e0e5a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="configuring-parameters-for-web-package-deployment"></a>Konfigurace parametrů pro nasazení webového balíčku
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje, jak nastavit hodnoty parametrů, jako jsou názvy webových aplikací Internetové informační služby (IIS), připojovací řetězce a koncové body služby, při nasazení webového balíčku na vzdálený webový server služby IIS.


Při vytváření projektu webové aplikace, sestavení a proces balení generuje tři soubory klíčů:

- A *[název projektu] .zip* souboru. Toto je balíčku pro nasazení webu pro váš projekt webové aplikace. Tento balíček obsahuje všechny sestavení, soubory, skripty databáze a prostředků potřebných k opětovnému vytvoření webové aplikace na vzdálený webový server IIS.
- A *[název projektu].deploy.cmd* souboru. Tato položka obsahuje sadu parametrizované příkazy nasazení webu (MSDeploy.exe), které publikování vašeho balíčku pro nasazení webu na vzdálený webový server služby IIS.
- A *[název projektu]. SetParameters.xml* souboru. To poskytuje sadu hodnot parametrů k příkazu MSDeploy.exe. Můžete aktualizovat hodnoty v tomto souboru a předejte ji do nasazení webu jako parametr příkazového řádku při nasazení webového balíčku.

> [!NOTE]
> Další informace o sestavení a proces balení, najdete v části [budova a projekty webových aplikací balení](building-and-packaging-web-application-projects.md).


*SetParameters.xml* souboru se dynamicky vygeneruje ze souboru projektu webové aplikace a všechny konfigurační soubory v projektu. Při sestavení a balíček projektu webové publikování kanálu (WPP) automaticky zjistí spoustu proměnné, které mohou změnit mezi nasazení prostředí, třeba cílové webové aplikace IIS a všechny databázové připojovací řetězce. Tyto hodnoty jsou automaticky parametrizovaných v balíčku pro nasazení webu a přidat do *SetParameters.xml* souboru. Například, pokud přidáte připojovacího řetězce, který *web.config* souboru v projektu webové aplikace procesu sestavení tuto změnu zjistí a přidá položku do *SetParameters.xml* souboru odpovídajícím způsobem.

V mnoha případech bude tato automatické Parametrizace dostatečná. Ale pokud budou uživatelé potřebovat k odlišení další nastavení mezi prostředími nasazení, jako je nastavení aplikace nebo adresy URL koncového bodu služby, musíte říct WPP Parametrizace tyto hodnoty v balíčku pro nasazení a přidat odpovídající položky do *SetParameters.xml* souboru. Následující části vysvětlují, jak na to.

### <a name="automatic-parameterization"></a>Automatické Parametrizace

Při sestavování a balíček webovou aplikaci, bude jako automaticky Parametrizace tyto věci:

- Cíl služby IIS webové název a cesta k aplikaci.
- Jakékoli připojení, řetězce ve vaší *web.config* souboru.
- Připojovací řetězce pro všechny databáze, které přidáte do **balení/publikování kódu SQL** karty na stránkách vlastností projektu.

Například pokud byste chtěli sestavení a balíčku [obraťte se na správce](the-contact-manager-solution.md) ukázkové řešení bez zásahu do procesu Parametrizace žádným způsobem, jako by vygeneroval toto *ContactManager.Mvc.SetParameters.xml* souboru:


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]


V tomto případě:

- **Název webové aplikace IIS** parametr je cesta služby IIS, ve které chcete nasadit webovou aplikaci. Výchozí hodnota je založena **balení/publikování webu** stránky na stránkách vlastností projektu.
- **ApplicationServices-Web.config připojovací řetězec** parametr se vygeneroval ze **connectionStrings nebo přidat** element v *web.config* souboru. Reprezentuje připojovací řetězec, který by aplikace měla použít kontaktovat databáze členství. Hodnota, zadejte zde bude nahrazena do nasazené *web.config* souboru. Výchozí hodnota je převzat ze před nasazením *web.config* souboru.

JAKO také parameterizes tyto vlastnosti v balíčku pro nasazení, které generuje. Při instalaci balíčku pro nasazení, můžete zadat hodnoty pro tyto vlastnosti. Pokud budete instalovat balíček ručně pomocí Správce služby IIS, jak je popsáno v [ručně instalaci webových balíčků](manually-installing-web-packages.md), Průvodce instalací vás vyzve k zadání hodnot pro všechny parametry. Při instalaci balíčku vzdáleně pomocí *. deploy.cmd* souboru, jak je popsáno v [nasazení webových balíčků](deploying-web-packages.md), Web Deploy bude vypadat k tomuto *SetParameters.xml* do souboru Zadejte hodnoty parametrů. Můžete upravit hodnoty *SetParameters.xml* soubor ručně nebo jako součást automatizovaného procesu sestavení a nasazení, můžete upravit soubor. Tento proces je popsán podrobněji dále v tomto tématu.

### <a name="custom-parameterization"></a>Vlastní Parametrizace

Ve složitějších scénářích nasazení budete často chtít Parametrizace další vlastnosti před nasazením projektu. Obecně řečeno by měl Parametrizace všechny vlastnosti a nastavení, která se bude lišit podle cílové prostředí. Mohou mezi ně patří:

- Koncové body v služby *web.config* souboru.
- Nastavení aplikace v nástroji *web.config* souboru.
- Další deklarativní vlastnosti, které chcete vyzvat uživatele k zadání.

Nejjednodušší způsob, jak Parametrizace tyto vlastnosti je přidat *parameters.xml* souboru ke kořenové složce projektu webové aplikace. Například v řešení obraťte se na správce, tento projekt ContactManager.Mvc zahrnuje *parameters.xml* soubor v kořenové složce.

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

Pokud jste tento soubor otevřít, se zobrazí, že obsahuje jeden **parametr** položku. Položka používá pro vyhledání a Parametrizace adresy URL koncového bodu služby ContactService Windows Communication Foundation (WCF) v příkazu jazyka XML Path Language (XPath) *web.config* souboru.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]


Kromě Parametrizace adresu URL koncového bodu v balíčku pro nasazení, jako také přidá odpovídající záznam na *SetParameters.xml* soubor, který získá generované společně se balíček pro nasazení.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]


Pokud ručně nainstalovat balíček pro nasazení, Správce služby IIS zobrazí výzvu pro adresa koncového bodu služby spolu s vlastností, které byly automaticky parametry. Při instalaci balíčku pro nasazení tak, že spustíte *. deploy.cmd* souboru, můžete upravit *SetParameters.xml* souboru k zadání hodnoty pro adresa koncového bodu služby společně s hodnoty pro vlastnosti, které byly automaticky parametry.

Úplné podrobnosti o tom, jak vytvořit *parameters.xml* souborů najdete v tématu [postupy: použití parametrů ke konfiguraci nastavení při balíček pro nasazení je nainstalován](https://msdn.microsoft.com/en-us/library/ff398068.aspx). Proces s názvem **chcete používat parametry nasazení pro nastavení souboru Web.config** poskytuje podrobné pokyny.

## <a name="modifying-the-setparametersxml-file"></a>Úprava souboru SetParameters.xml

Pokud plánujete nasadit balíček webových aplikací ručně & #x 2014; buď spuštěním *. deploy.cmd* souboru nebo spuštěním MSDeploy.exe z příkazového řádku & #x 2014; není co vám ruční úpravy *SetParameters.xml* soubor před nasazení. Ale pokud pracujete v rámci řešení podnikovém měřítku, můžete nasadit balíček webových aplikací jako součást větší, automatizované procesu sestavení a nasazení. V tomto scénáři budete potřebovat Microsoft Build Engine (MSBuild) Chcete-li upravit *SetParameters.xml* souboru pro vás. Můžete to provést pomocí MSBuild **xmlpoke –** úloh.

[Obraťte se na správce ukázkové řešení](the-contact-manager-solution.md) znázorňuje tento proces. Příklady kódu, které byly upraveny a zobrazit pouze podrobnosti, které jsou relevantní pro tento příklad.

> [!NOTE]
> Komplexnější přehled model souboru projektu v ukázkové řešení a úvod do vlastních projektů soubory v obecné, najdete v tématu [vysvětlení souboru projektu](understanding-the-project-file.md) a [Principy procesu sestavení](understanding-the-build-process.md).


Nejprve hodnoty parametrů, které vás zajímají jsou definovány jako vlastnosti v souboru projektu konkrétní prostředí (například *Env Dev.proj*).


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]


> [!NOTE]
> Pokyny k přizpůsobení souborů projektu specifický pro vaše vlastní prostředí serveru najdete v tématu [nakonfigurovat vlastnosti nasazení pro cílové prostředí](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


Dále *Publish.proj* tyto vlastnosti import souboru. Protože každý *SetParameters.xml* soubor je přidružen *. deploy.cmd* souboru a jsme nakonec chcete soubor projektu pro každé vyvolání *. deploy.cmd* souboru projektu Vytvoří soubor MSBuild *položky* pro každou *. deploy.cmd* souborů a definuje vlastnosti týkající se jako *metadata položky*.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]


V tomto případě:

- **ParametersXml** hodnota metadat Určuje umístění *SetParameters.xml* souboru.
- **IisWebAppName** hodnota je cesta služby IIS, do které chcete nasadit webovou aplikaci.
- **MembershipDBConnectionString** hodnota je připojovací řetězec pro databázi členství a **MembershipDBConnectionName** hodnota je **název** atribut odpovídajícího parametru ve *SetParameters.xml* souboru.
- **ServiceEndpointValue** hodnota je adresa koncového bodu služby WCF na cílovém serveru a **ServiceEndpointParamName** hodnota je atribut názvu odpovídající parametr v *SetParameters.xml* souboru.

Nakonec v *Publish.proj* souboru **PublishWebPackages** cíle používá **xmlpoke –** úloh k úpravě těchto hodnot v *SetParameters.xml* souboru.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]


Můžete si všimnout, aby se každý **xmlpoke –** úloh určuje čtyři hodnoty atributů:

- **XmlInputPath** atribut udává úlohu, kde najít soubor, který chcete upravit.
- **Dotazu** atribut je dotaz XPath, který identifikuje uzel XML, kterou chcete změnit.
- **Hodnotu** atribut je nová hodnota chcete vložit do vybrané uzel XML.
- **Podmínku** atribut je kritéria, na kterých by měl úlohu spustit nebo nepoběží. V těchto případech podmínku zajistí, že nemáte pokusíte vložit hodnotu null ani prázdnou hodnotu do *SetParameters.xml* souboru.

## <a name="conclusion"></a>Závěr

Toto téma popisuje role *SetParameters.xml* souboru a vysvětlení, jak je vytvořena při sestavování projektu webové aplikace. Vysvětlení najdete, jak můžete přidáním parametrizovat další nastavení *parameters.xml* souboru do projektu. Také popisuje, jak můžete upravit *SetParameters.xml* soubor jako součást procesu sestavení větší, automatizované, pomocí **xmlpoke –** úloh v souborech projektu.

Dalším tématu [nasazení webových balíčků](deploying-web-packages.md), popisuje, jak nasadit webový balíček buď spuštěním *. deploy.cmd* souboru nebo pomocí MSDeploy.exe příkazy přímo. V obou případech můžete zadat vaše *SetParameters.xml* souboru jako parametr nasazení.

## <a name="further-reading"></a>Další čtení

Informace o tom, jak vytvořit webových balíčků najdete v tématu [budova a projekty webových aplikací balení](building-and-packaging-web-application-projects.md). Pokyny o tom, jak ve skutečnosti nasazení webového balíčku najdete v tématu [nasazení webových balíčků](deploying-web-packages.md). Podrobný návod k vytvoření *parameters.xml* souborů najdete v tématu [postupy: použití parametrů ke konfiguraci nastavení při balíček pro nasazení je nainstalován](https://msdn.microsoft.com/en-us/library/ff398068.aspx).

Další obecné informace o Parametrizace v nasazení webu, najdete v části [Parametrizace webového nasazení v akci](https://go.microsoft.com/?linkid=9805119) (příspěvek na blogu).

>[!div class="step-by-step"]
[Předchozí](building-and-packaging-web-application-projects.md)
[další](deploying-web-packages.md)
