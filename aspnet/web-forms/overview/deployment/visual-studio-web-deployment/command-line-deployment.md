---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení příkazového řádku | Dokumentace Microsoftu'
author: tdykstra
description: V této sérii kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, podle usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: 9b2832dd23f68f9283983b52a85e8a9d5f85fd2f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756729"
---
<a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a>Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení příkazového řádku
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout počáteční projekt](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> V této sérii kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, s použitím sady Visual Studio 2012 nebo Visual Studio 2010. Informace o této sérii, naleznete v tématu [z prvního kurzu této série](introduction.md).


## <a name="overview"></a>Přehled

V tomto kurzu se dozvíte, jak vyvolat webu aplikace Visual Studio publikovat profilace z příkazového řádku. To je užitečné pro scénáře, ve které chcete [automatizovat proces nasazení](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) místo způsobem ho ručně v sadě Visual Studio, obvykle pomocí [zdroje systém správy verzí kódu](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).

## <a name="make-a-change-to-deploy"></a>Proveďte změnu nasazení

Na stránce o aktuálně zobrazuje kód šablony.

![Stránka s kód šablony](command-line-deployment/_static/image1.png)

Nahradíte, který s kódem, který se zobrazí souhrn registrace studentů.

Otevřít *About.aspx* stránce, odstraňte všechny značky uvnitř `MainContent` `Content` elementu a vložte následující kód na příslušné místo:

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

Spusťte projekt a vyberte **o** stránky.

![O stránku](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a>Nasazení do testu pomocí příkazového řádku

Nebude nasazovat další změnou databáze, tak nasazení databáze pro databázi aspnet ContosoUniversity zakázat dbDacFx. Otevřít **Publikovat Web** průvodce a v každé ze tří publikační profily, zrušte **aktualizace databáze** zaškrtávací políčko na **nastavení** kartu.

V systému Windows 8 úvodní stránky, vyhledejte **Developer Command Prompt for VS2012**.

Klikněte pravým tlačítkem na ikonu pro **Developer Command Prompt for VS2012** a klikněte na tlačítko **spustit jako správce**.

Zadejte následující příkaz na příkazovém řádku, nahraďte cestou k souboru vašeho řešení cestu k souboru řešení:

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

Nástroj MSBuild sestaví řešení a nasadí do testovacího prostředí.

![Výstup příkazového řádku](command-line-deployment/_static/image3.png)

Otevřete prohlížeč a přejděte na `http://localhost/ContosoUniversity`, klikněte **o** stránku a ověřte, že nasazení bylo úspěšné.

Pokud jste ještě nevytvořili žádné studenty v testu, zobrazí se prázdná stránka v části **Student tělo statistiky** záhlaví. Přejděte na **studenty** klikněte na **přidat studenty**a přidat některé studenty a pak se vraťte k **o** stránky zobrazit statistiku studentů.

![O stránku v testovacím prostředí](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a>Možnosti příkazového řádku klíče

Příkaz, který jste zadali předávaným do MSBuild cesta k souboru řešení a dvě vlastnosti:

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a>Nasazení řešení a nasazením jednotlivých projektů

Určení souboru řešení způsobí, že všechny projekty v řešení, která má být sestaven. Pokud máte více webových projektů v řešení, platí toto chování nástroje MSBuild:

- Vlastnosti, které zadáte v příkazovém řádku jsou předány do jakéhokoliv projektu. Proto musí mít každý webový projekt profil publikování se název, který zadáte. Pokud zadáte `/p:PublishProfile=Test`, každý webový projekt musí mít profilu publikování s názvem *Test*.
- Jeden projekt může publikovat úspěšně při jiný nedaří i sestavit. Další informace najdete v tématu vlákno na stackoverflow [MSBuild nezdaří a zobrazí se dva balíčky](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).

Pokud zadáte jednotlivé projektu namísto řešení, je nutné přidat parametr, který určuje verzi sady Visual Studio. Pokud používáte sadu Visual Studio 2012 příkazového řádku by podobně jako v následujícím příkladu:

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

Číslo verze pro Visual Studio 2010 je 10.0. Další informace najdete v tématu [kompatibilita projektu sady Visual Studio a VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) na blogu Sayed Hashimi.

### <a name="specifying-the-publish-profile"></a>Určení profilu publikování

Profil publikování můžete zadat název nebo úplnou cestu k *.pubxml* souboru, jak je znázorněno v následujícím příkladu:

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a>Publikování webu metod podporovaných pro publikování příkazového řádku

Publikovat tři metody jsou podporovány pro publikování příkazového řádku:

- `MSDeploy` -Publikujte pomocí nasazení webu.
- `Package` -Publikujte vytvořením balíčku pro nasazení webu. Je potřeba nainstalovat balíček samostatně z příkazu MSBuild, který ji vytvoří.
- `FileSystem` -Publikujte pomocí kopírování souborů do zadané složky.

### <a name="specifying-the-build-configuration-and-platform"></a>Určení konfigurace sestavení a platformy

Konfigurace sestavení a platformy musí být nastavena v sadě Visual Studio nebo na příkazovém řádku. Profily publikování obsahovat vlastnosti, které jsou pojmenovány `LastUsedBuildConfiguration` a `LastUsedPlatform`, ale aby bylo možné zjistit, jak je sestaven projekt nelze nastavit tyto vlastnosti. Další informace najdete v tématu [MSBuild: jak nastavit vlastnost konfigurace](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) na blogu Sayed Hashimi.

## <a name="deploy-to-staging"></a>Nasazení do přípravného prostředí

Pokud chcete nasadit do Azure, musíte přidat heslo do příkazového řádku. Pokud jste si uložili heslo v profilu publikování ve Visual Studio, byla uložena v zašifrované podobě v váš *. pubxml.user* souboru. Tento soubor není přistupuje MSBuild, když provedete nasazení příkazového řádku, takže musíte předat heslo v parametrech příkazového řádku.

1. Zkopírujte heslo, které potřebujete, na základě *.publishsettings* soubor, který jste stáhli dříve pro pracovní webové stránky. Heslo je hodnota `userPWD` atribut pro nasazení webu `publishProfile` elementu.

    ![Heslo webového nasazení](command-line-deployment/_static/image5.png)
2. V systému Windows 8 úvodní stránky, vyhledejte **Developer Command Prompt for VS2012**a klikněte na ikonu otevřít příkazový řádek. (Nemusíte ho jako správce otevřete tentokrát vzhledem k tomu, že nejsou nasazení do služby IIS v místním počítači.)
3. Zadejte následující příkaz na příkazovém řádku cestu k souboru řešení nahradíte cestu k souboru řešení a heslo s heslem:

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    Všimněte si, že tento příkazový řádek obsahuje další parametr: `/p:AllowUntrustedCertificate=true`. Jako v tomto kurzu se zápisem, `AllowUntrustedCertificate` při publikování do Azure z příkazového řádku, musí být nastavena vlastnost. Po vydání opravy pro tuto chybu, nemusíte tento parametr.
4. Otevřete prohlížeč a přejděte na adresu URL webu pracovní a potom klikněte na tlačítko **o** stránku a ověřte, že nasazení bylo úspěšné.

    Jak už jste viděli dříve pro testovací prostředí, bude pravděpodobně nutné vytvořit některé studenty, aby se zobrazovat Statistika na **o** stránky.

## <a name="deploy-to-production"></a>Nasazení do produkčního prostředí

Je podobný procesu pro pracovní proces nasazení do produkčního prostředí.

1. Zkopírujte heslo, které potřebujete, na základě *.publishsettings* soubor, který jste stáhli dříve pro produkční webový server.
2. Otevřít **Developer Command Prompt for VS2012**.
3. Zadejte následující příkaz na příkazovém řádku cestu k souboru řešení nahradíte cestu k souboru řešení a heslo s heslem:

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    Pro skutečné produkční lokality, pokud došlo také ke změně databáze by obvykle zkopírujete *aplikace\_offline.htm* soubor lokality před nasazením a odstranit po úspěšném nasazení.
4. Otevřete prohlížeč a přejděte na adresu URL webu pracovní a potom klikněte na tlačítko **o** stránku a ověřte, že nasazení bylo úspěšné.

## <a name="summary"></a>Souhrn

Jste teď nasadili aktualizace aplikace pomocí příkazového řádku.

![O stránku v testovacím prostředí](command-line-deployment/_static/image6.png)

V dalším kurzu, zobrazí se příklad toho, jak rozšířit webu kanálu publikování. V příkladu obsahuje pokyny k nasazení souborů, které nejsou součástí projektu.

> [!div class="step-by-step"]
> [Předchozí](deploying-a-database-update.md)
> [další](deploying-extra-files.md)
