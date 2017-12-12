---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: "Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení příkazového řádku | Microsoft Docs"
author: tdykstra
description: "Tato řada kurzu se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo do hostujícího zprostředkovatele třetí strany podle usin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: 8446b3fc05e3ef4a5a30c753c989252fd7f1a56f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a>Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení příkazového řádku
====================
podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si úvodní projekt](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Tato řada kurzu se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, pomocí sady Visual Studio 2012 nebo Visual Studio 2010. Informace o řadě najdete v tématu [z prvního kurzu řady](introduction.md).


## <a name="overview"></a>Přehled

V tomto kurzu se dozvíte, jak má být vyvolán webu Visual Studio publikovat profilace z příkazového řádku. To je užitečné pro scénáře, ve které chcete [automatizovat proces nasazení](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) místo to ji ručně v sadě Visual Studio, obvykle pomocí [zdroje systém správy verzí kódu](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).

## <a name="make-a-change-to-deploy"></a>Změňte k nasazení

Na stránce o aktuálně zobrazuje kód šablony.

![O stránce s kód šablony](command-line-deployment/_static/image1.png)

Budete, nahraďte s kódem, který zobrazuje souhrn student registrace.

Otevřete *About.aspx* stránky, odstranit všechny značky uvnitř `MainContent` `Content` elementu a vložte následující kód na příslušné místo:

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

Spusťte projekt a vyberte **o** stránky.

![O stránku](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a>Nasazení do testu pomocí příkazového řádku

Nebude nasazování další změnou databáze, tak zakázat dbDacFx databáze nasazení pro databázi aspnet ContosoUniversity. Otevřete **Publikovat Web** průvodce a v každém ze tří publikační profily, zrušte **aktualizace databáze** políčko **nastavení** kartě.

Na stránce Start systému Windows 8 vyhledejte **příkazový řádek vývojáře pro VS2012**.

Klikněte pravým tlačítkem myši na ikonu **příkazový řádek vývojáře pro VS2012** a klikněte na tlačítko **spustit jako správce**.

Zadejte následující příkaz na příkazovém řádku, nahraďte cestu k souboru řešení cesta k souboru řešení:

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

MSBuild sestavení řešení a nasadí ho na testovací prostředí.

![Výstup příkazového řádku](command-line-deployment/_static/image3.png)

Otevřete prohlížeč a přejděte na `http://localhost/ContosoUniversity`, klikněte **o** a ověřte, že nasazení bylo úspěšné.

Pokud jste nevytvořili žádné studenty v testu, zobrazí se prázdná stránka v části **Student textu statistiky** záhlaví. Přejděte na **studenty** klikněte na tlačítko **přidat Student**a přidejte některé studenty a pak se vraťte do **o** stránky zobrazíte student statistiky.

![O stránku v testovacím prostředí](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a>Možnosti klíče příkazového řádku

Příkaz, který jste zadali cestu k souboru řešení a dvě vlastnosti předaný MSBuild:

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a>Nasazení řešení oproti nasazení jednotlivých projektů

Určení soubor řešení způsobí, že všechny projekty v řešení, která má být sestaven. Pokud máte více webové projekty v řešení, platí následující chování MSBuild:

- Vlastnosti, které zadáte v příkazovém řádku se předávají na všechny projekty. Každý webový projekt proto musí mít profil publikování se název, který zadáte. Pokud zadáte `/p:PublishProfile=Test`, každý webový projekt musí mít profilu publikování s názvem *Test*.
- Pokud jiný není i sestavení může úspěšně publikování jeden projektu. Další informace najdete v tématu vlákno stackoverflow [MSBuild nezdaří a zobrazí se dva balíčky](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).

Pokud zadáte jednotlivých projektů místo řešení, je nutné přidat parametr, který určuje verzi sady Visual Studio. Pokud používáte Visual Studio 2012 příkazového řádku by podobně jako v následujícím příkladu:

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

Číslo verze pro sadu Visual Studio 2010 je 10.0. Další informace najdete v tématu [kompatibility projektu sady Visual Studio a VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) na blogu Sayed Hashimi.

### <a name="specifying-the-publish-profile"></a>Určení profilu publikování

Můžete zadat profil publikování, podle názvu nebo podle úplnou cestu k *.pubxml* souboru, jak je znázorněno v následujícím příkladu:

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a>Metody pro příkazového řádku publikování podporuje publikování webu

Tři publikování metody jsou podporovány pro publikování příkazového řádku:

- `MSDeploy`-Publikujte pomocí nástroje nasazení webu.
- `Package`-Publikujte vytvořením balíčku pro nasazení webu. Je nutné nainstalovat samostatně z nástroje MSBuild příkaz, který ji vytvoří.
- `FileSystem`-Publikujte pomocí kopírování souborů do zadané složky.

### <a name="specifying-the-build-configuration-and-platform"></a>Konfigurace sestavení a platformy

Konfigurace sestavení a platformy musí být nastavena v sadě Visual Studio nebo na příkazovém řádku. Profily publikování zahrnují vlastnosti, které jsou s názvem `LastUsedBuildConfiguration` a `LastUsedPlatform`, ale aby bylo možné zjistit, jak sestavení projektu nelze nastavit tyto vlastnosti. Další informace najdete v tématu [MSBuild: jak nastavit vlastnost konfigurace](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) na blogu Sayed Hashimi.

## <a name="deploy-to-staging"></a>Nasazení do přípravy

Pokud chcete nasadit do Azure, musíte přidat heslo na příkazový řádek. Pokud jste uložili heslo v profilu publikování v sadě Visual Studio, byla uložená v šifrovaném formátu v vaše *. pubxml.user* souboru. Tento soubor není přístup MSBuild při nasazení příkazového řádku, takže budete muset předat heslo v parametru příkazového řádku.

1. Kopírovat heslo, které je nutné z *.publishsettings* souboru, který jste stáhli dříve pro pracovní webový server. Heslo je hodnota `userPWD` atribut pro nasazení webu `publishProfile` elementu.

    ![Heslo nasadit web](command-line-deployment/_static/image5.png)
2. Na stránce Start systému Windows 8 vyhledejte **příkazový řádek vývojáře pro VS2012**a klikněte na ikonu a otevřete okno příkazového řádku. (Nemáte a otevře se jako správce tentokrát vzhledem k tomu, že nejsou nasazení pro službu IIS v místním počítači.)
3. Zadejte následující příkaz na příkazovém řádku, nahraďte cestu k souboru řešení cestu k souboru řešení a heslo pomocí hesla:

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    Všimněte si, že tento příkazový řádek obsahuje další parametr: `/p:AllowUntrustedCertificate=true`. Jak probíhá zápis tohoto kurzu, `AllowUntrustedCertificate` při publikování v Azure z příkazového řádku, musí být nastavena vlastnost. Až bude vydaná oprava této chyby, nebude nutné tento parametr.
4. Otevřete prohlížeč a přejděte na adresu URL vašeho webu pracovní a klikněte **o** a ověřte, že nasazení bylo úspěšné.

    Jak už jste viděli dříve pro testovací prostředí, možná budete muset vytvořit některé studenty zobrazíte statistiky na **o** stránky.

## <a name="deploy-to-production"></a>Nasazení do produkčního prostředí

Je podobný procesu pro pracovní proces nasazení do produkčního prostředí.

1. Kopírovat heslo, které je nutné z *.publishsettings* souboru, který jste stáhli dříve pro produkční webový server.
2. Otevřete **příkazový řádek vývojáře pro VS2012**.
3. Zadejte následující příkaz na příkazovém řádku, nahraďte cestu k souboru řešení cestu k souboru řešení a heslo pomocí hesla:

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    Pro skutečné produkční lokality, pokud taky došlo ke změně databáze by obvykle zkopírujete *aplikace\_offline.htm* souborů do lokality před nasazením a odstranit po úspěšné nasazení.
4. Otevřete prohlížeč a přejděte na adresu URL vašeho webu pracovní a klikněte **o** a ověřte, že nasazení bylo úspěšné.

## <a name="summary"></a>Souhrn

Nyní jste nasadili aktualizaci aplikace pomocí příkazového řádku.

![O stránku v testovacím prostředí](command-line-deployment/_static/image6.png)

V dalším kurzu se zobrazí příklad toho, jak rozšířit webu kanálu publikování. V příkladu vám ukáže, jak nasadit soubory, které nejsou součástí projektu.

>[!div class="step-by-step"]
[Předchozí](deploying-a-database-update.md)
[další](deploying-extra-files.md)
