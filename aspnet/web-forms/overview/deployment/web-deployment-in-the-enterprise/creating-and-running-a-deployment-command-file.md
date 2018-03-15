---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: "Vytváření a spouštění nasazení příkaz Soubor | Microsoft Docs"
author: jrjlee
description: "Toto téma popisuje, jak vytvořit soubor příkazů, které vám umožní spustit nasazení, které využívá soubory projektu Microsoft Build Engine (MSBuild) jako jeden krok, znovu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: bc31bf55b29661816e0ca9a50b51b0abc3eb2c98
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
<a name="creating-and-running-a-deployment-command-file"></a>Vytváření a spouštění soubor příkazů nasazení
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje, jak vytvořit soubor příkazů, které vám umožní spuštění pomocí souborů projektu Microsoft Build Engine (MSBuild) jako jeden krok, opakovatelných procesu nasazení.


Toto téma je součástí ze série kurzů na základě kolem podnikové požadavky nasazení fiktivní společnost s názvem Fabrikam, Inc. Tento kurz series používá ukázkové řešení & #x 2014; [obraťte se na správce](the-contact-manager-solution.md) řešení & #x 2014; představující webovou aplikaci s úrovní realistické složitější, včetně aplikace ASP.NET MVC 3 systému Windows Komunikační služby Foundation (WCF) a projekt databáze.

Metoda nasazení jádrem tyto kurzy je založena na popsaný přístup souboru projektu rozdělení [Principy procesu sestavení](understanding-the-build-process.md), ve které je řízené procesu sestavení dva projektu soubory & #x 2014; jeden obsahující sestavení pokyny, které platí pro každé cílové prostředí a jeden, který obsahuje nastavení pro konkrétní prostředí sestavení a nasazení. V okamžiku sestavení souboru projektu konkrétní prostředí sloučeny do souboru projektu bez ohledu na prostředí a vytvořit úplnou sadu pokynů sestavení.

## <a name="process-overview"></a>Přehled procesu

V tomto tématu se dozvíte, jak vytvořit a spustit soubor příkazů, který používá tyto soubory projektu pro provedení opakované nasazení do cílového prostředí. V podstatě příkazového řádku jednoduše tak, aby obsahovala příkaz MSBuild který:

- Informuje MSBuild provést v prostředí bez ohledu na *Publish.proj* souboru.
- Informuje *Publish.proj* souboru, soubor, který obsahuje nastavení projektu konkrétní prostředí a kde najít ho.

## <a name="create-an-msbuild-command"></a>Vytvořit příkaz nástroje MSBuild

Jak je popsáno v [Principy procesu sestavení](understanding-the-build-process.md), soubor projektu konkrétní prostředí & #x 2014; například *Env Dev.proj*& #x 2014; slouží k importu do bez ohledu na prostředí *Publish.proj* souboru v čase vytvoření buildu. Společně poskytují tyto dva soubory kompletní sada pokynů, které informují MSBuild, jak vytvořit a nasadit řešení.

*Publish.proj* soubor používá **importovat** elementu, který chcete importovat soubor projektu konkrétní prostředí.


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


Jako takový když použijete MSBuild.exe sestavení a nasazení řešení. Obraťte se na správce, budete muset:

- Spustit MSBuild.exe na *Publish.proj* souboru.
- Zadejte umístění souboru projektu konkrétní prostředí zadáním příkazového řádku parametr s názvem **TargetEnvPropsFile**.

K tomuto účelu MSBuild příkazu by měl vypadat přibližně takto:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


Tady je jednoduchý kroku a přesunu do opakovatelné a krokování nasazení. Všechny, které musíte udělat je přidání příkazu MSBuild do souboru CMD. V řešení obraťte se na správce, publikovat složky obsahuje soubor s názvem *publikovat Dev.cmd* který přesně to provádí.


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> **/Fl** přepínač dá pokyn MSBuild vytvořit soubor protokolu s názvem *msbuild.log* v pracovním adresáři, ve kterém byl vyvolán MSBuild.exe.


Nasazení nebo znovu nasadit řešení obraťte se na správce, stačí provést spuštění *publikovat Dev.cmd* souboru. Při spuštění souboru, bude MSBuild:

- Vytvořte všechny projekty v řešení.
- Generovat nasadit webových balíčků pro projekty webových aplikací.
- Generovat .dbschema a .deploymanifest souborů pro databázové projekty.
- Balíčky webového nasazení na webový server.
- K databázovému serveru nasazení databáze.

## <a name="run-the-deployment"></a>Spustit nasazení

Když vytvoříte soubor příkazů pro cílové prostředí byste měli mít k dokončení celého nasazení jednoduše spuštěním souboru.

**K nasazení řešení. Obraťte se na správce vašeho testovacího prostředí**

1. Na pracovní stanici developer, otevřete Průzkumníka Windows a pak přejděte do umístění *publikovat Dev.cmd* souboru.
2. Poklikejte na soubor ji spustit.
3. Pokud **otevření souboru – upozornění zabezpečení** se zobrazí dialogové okno, klikněte na tlačítko **spustit**.
4. Pokud je nastaven nastavení konfigurace a testovací servery správně, okna příkazového řádku se zobrazí **sestavení bylo úspěšné** zprávu po dokončení zpracování souborů projektu nástroje MSBuild.

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. Pokud je při prvním nasazení řešení do prostředí tohoto nástroje, budete muset přidat účet počítače serveru webového testu k **db\_datawriter** a **db\_DataReader –**rolí na **ContactManager** databáze. Tento postup je popsaný v [konfigurace databáze serveru pro webové nasazení publikování](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

    > [!NOTE]
    > Potřebujete tato oprávnění přiřadit, když vytvoříte databázi. Ve výchozím nastavení procesu sestavení nebude znovu vytvořit databázi na každou nasazení & #x 2014; místo toho porovná existující databázi a schéma nejnovější a proveďte pouze požadované změny. V důsledku toho by měla pouze potřebujete namapovat tyto databázové role při prvním nasazení řešení.
6. Otevřete Internet Explorer a přejděte na adresu URL aplikace, obraťte se na správce (například `http://testweb1:85/ContactManager/`).
7. Ověřte, že aplikace funguje podle očekávání a vy budete moci přidat kontakty.

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a>Závěr

Vytvoření souboru příkaz, který obsahuje vaše pokyny MSBuild nabízí rychlý a snadný způsob vytváření a nasazování řešení vícenásobného projektu v určitém cílovém prostředí. Pokud potřebujete opakovaně nasadit řešení v prostředí s více cílové, můžete vytvořit více příkazové soubory. Do každého souboru příkaz příkaz MSBuild budete stavět stejný soubor universal projektu, ale specifikujete soubor jiný projekt konkrétní prostředí. Soubor příkazů publikování do vývojář nebo testovacím prostředí, může například obsahovat tento příkaz nástroje MSBuild:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


Tento příkaz nástroje MSBuild může obsahovat soubor příkazů pro publikování do pracovní prostředí:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> Pokyny k přizpůsobení souborů projektu specifický pro vaše vlastní prostředí serveru najdete v tématu [nakonfigurovat vlastnosti nasazení pro cílové prostředí](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


Přepsání vlastností nebo nastavení různé další přepínače v příkazu MSBuild můžete také upravit procesu sestavení pro každé prostředí. Další informace najdete v tématu [příkazového řádku MSBuild – Reference](https://msdn.microsoft.com/library/ms164311.aspx).

>[!div class="step-by-step"]
[Předchozí](deploying-database-projects.md)
[další](manually-installing-web-packages.md)
