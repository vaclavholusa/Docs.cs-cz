---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Soubor příkazů pro vytvoření a spuštění nasazení | Dokumentace Microsoftu
author: jrjlee
description: Toto téma popisuje, jak vytvořit soubor příkazů, který vám umožní spustit nasazení, které využívá Microsoft Build Engine (MSBuild) souborů projektu jako jeden krok, re...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: 6b2a75e0740f648a3a6e4f39c00bd30609325728
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830299"
---
<a name="creating-and-running-a-deployment-command-file"></a>Vytvoření a spuštění souboru příkazů k nasazení
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje, jak vytvořit soubor příkazů, který vám umožní spustit nasazení, které využívá Microsoft Build Engine (MSBuild) souborů projektu jako krokování a opakovatelného procesu.


Toto téma je součástí série kurzů podle požadavků na nasazení enterprise fiktivní společnosti s názvem společnosti Fabrikam, Inc. V této sérii kurzů používá ukázkové řešení&#x2014; [Správce kontaktů](the-contact-manager-solution.md) řešení&#x2014;představující webovou aplikaci s realistické úroveň složitosti, včetně aplikace ASP.NET MVC 3, komunikace Windows Služba Foundation (WCF) a databázový projekt.

Metody nasazení v srdci těchto kurzů je založen na rozdělení přístupu soubor projektu je popsáno v [Principy procesu sestavení](understanding-the-build-process.md), ve které je řízena procesem sestavení dva soubory projektu&#x2014;jeden obsahující pokyny, které platí pro všechny cílové prostředí a jeden obsahuje nastavení pro konkrétní prostředí sestavení a nasazení pro sestavení. V okamžiku sestavení souboru projektu specifických pro prostředí se sloučí do souboru projektu bez ohledu na prostředí a vytvoří kompletní sadu pokynů sestavení.

## <a name="process-overview"></a>Přehled procesu

V tomto tématu se dozvíte, jak vytvořit a spustit příkaz soubor, který používá tyto soubory projektu nasazení opakovatelným na vaše cílové prostředí. V podstatě souboru příkazů jednoduše musí obsahovat příkaz MSBuild, který:

- Dává pokyn ke spuštění prostředí bez ohledu na MSBuild *Publish.proj* souboru.
- Říká *Publish.proj* souboru, soubor, který obsahuje nastavení projektu pro konkrétní prostředí a kde ho najít.

## <a name="create-an-msbuild-command"></a>Vytvoření příkazu MSBuild

Jak je popsáno v [Principy procesu sestavení](understanding-the-build-process.md), soubor projektu specifických pro prostředí&#x2014;například *Env Dev.proj*&#x2014;slouží k importu do prostředí nezávislá *Publish.proj* soubor v okamžiku sestavení. Tyto dva soubory společně poskytují kompletní sadu pokynů, které informují MSBuild, jak sestavit a nasadit řešení.

*Publish.proj* soubor používá **importovat** – element pro import souboru projektu pro konkrétní prostředí.


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


V důsledku toho použijete-li vytvářet a nasazovat řešení Správce kontaktů MSBuild.exe, musíte:

- Spouštět MSBuild.exe *Publish.proj* souboru.
- Zadejte umístění souboru projektu pro konkrétní prostředí zadáním příkazového řádku parametr s názvem **TargetEnvPropsFile**.

K tomuto účelu nástroj MSBuild příkazu by měl vypadat takto:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


Tady je jednoduchý kroku a přesunu do nasazení opakovatelným a krokování. Všechno, co musíte udělat, je přidání příkazu MSBuild do souboru .cmd. V řešení Správce kontaktů publikovat složka obsahuje soubor s názvem *publikovat Dev.cmd* , který přesně to dělá.


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> **/Fl** přepínač nastaví nástroj MSBuild vytvoří soubor protokolu s názvem *msbuild.log* v pracovním adresáři, ve kterém se vyvolala MSBuild.exe.


Nasazení nebo znovu nasadit řešení Správce kontaktů, stačí provést spuštění *publikovat Dev.cmd* souboru. Když spustíte tento soubor, bude nástroj MSBuild:

- Sestavte všechny projekty v řešení.
- Generovat nasaditelný webových balíčků pro projekty webových aplikací.
- Generovat .dbschema a .deploymanifest souborů pro databázové projekty.
- Nasazení webových balíčků na webový server.
- Nasazení databáze na serveru databáze.

## <a name="run-the-deployment"></a>Spustit nasazení

Když vytvoříte soubor příkazů pro cílové prostředí byste měli dokončit celé nasazení jednoduše spuštěním souboru.

**K nasazení řešení Správce kontaktů na vašem testovacím prostředí**

1. Na vaši vývojářskou pracovní stanici, otevřete Průzkumníka Windows a pak přejděte do umístění *publikovat Dev.cmd* souboru.
2. Poklikejte na soubor a spustíme ji.
3. Pokud **otevření souboru – upozornění zabezpečení** dialogové okno se zobrazí, klikněte na tlačítko **spustit**.
4. Pokud nastavení konfigurace a testovací servery jsou nastaveny správně, okna příkazového řádku se zobrazí **úspěšnými Buildy** zprávu po dokončení zpracování souborů projektu MSBuild.

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. Pokud to je poprvé, kdy jste nasadili řešení do tohoto prostředí, budete muset přidat účet počítače serveru webového testu k **db\_datawriter nesmí být** a **db\_datareader**role na **ContactManager** databáze. Tento postup je popsaný v [konfigurace databázového serveru pro publikování nasazení na webu](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

    > [!NOTE]
    > Potřebujete pouze přiřadit tato oprávnění, když vytvoříte databázi. Ve výchozím nastavení, proces sestavení nebude znovu vytvořit databázi na každé nasazení&#x2014;místo toho se porovnat existující databázi a nejnovější schéma a provádět pouze změny, které vyžaduje. V důsledku toho by měla pouze potřeba namapovat tyto databázové role při prvním nasazení řešení.
6. Spusťte aplikaci Internet Explorer a přejděte na adresu URL aplikace Správce kontaktů (například `http://testweb1:85/ContactManager/`).
7. Ověřte, že aplikace funguje podle očekávání a budete moct přidat kontakty.

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a>Závěr

Vytváření souboru příkazů obsahující pokyny k MSBuild poskytuje rychlý a snadný způsob sestavování a nasazování řešení vícenásobného projektu pro konkrétní cílové prostředí. Pokud je potřeba opakované nasazení svého řešení do cílového prostředí s více, můžete vytvořit více souborů příkazů. V každém souboru příkaz příkaz MSBuild se sestaví stejný soubor projektu univerzální, ale určí soubor jiného projektu pro konkrétní prostředí. Soubor příkazů publikovat vývojáři nebo testovací prostředí může obsahovat například tento příkaz MSBuild:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


Soubor příkazů k publikování do přípravného prostředí může obsahovat tento příkaz MSBuild:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> Pokyny k přizpůsobení souborů projektu specifických pro prostředí pro prostředí serveru, naleznete v tématu [nakonfigurovat vlastnosti nasazení pro cílové prostředí](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


Můžete také přizpůsobit proces sestavení pro jednotlivá prostředí přepsání vlastností nebo nastavením různé jinými přepínači ve svých rukou nástroje MSBuild. Další informace najdete v tématu [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).

> [!div class="step-by-step"]
> [Předchozí](deploying-database-projects.md)
> [další](manually-installing-web-packages.md)
