---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení příkazového řádku | Microsoft Docs'
author: tdykstra
description: Tato řada kurzu se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo do hostujícího zprostředkovatele třetí strany podle usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: acc4a0e7f4744a3759b90e0f1b159da68b7c7362
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890525"
---
<a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a><span data-ttu-id="e0c6b-103">Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="e0c6b-103">ASP.NET Web Deployment using Visual Studio: Command Line Deployment</span></span>
====================
<span data-ttu-id="e0c6b-104">Podle [tní Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="e0c6b-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="e0c6b-105">Stáhněte si úvodní projekt</span><span class="sxs-lookup"><span data-stu-id="e0c6b-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="e0c6b-106">Tato řada kurzu se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, pomocí sady Visual Studio 2012 nebo Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="e0c6b-107">Informace o řadě najdete v tématu [z prvního kurzu řady](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e0c6b-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="e0c6b-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="e0c6b-108">Overview</span></span>

<span data-ttu-id="e0c6b-109">V tomto kurzu se dozvíte, jak má být vyvolán webu Visual Studio publikovat profilace z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-109">This tutorial shows you how to invoke the Visual Studio web publish pipeline from the command line.</span></span> <span data-ttu-id="e0c6b-110">To je užitečné pro scénáře, ve které chcete [automatizovat proces nasazení](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) místo to ji ručně v sadě Visual Studio, obvykle pomocí [zdroje systém správy verzí kódu](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span><span class="sxs-lookup"><span data-stu-id="e0c6b-110">This is useful for scenarios where you want to [automate the deployment process](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) instead of doing it manually in Visual Studio, typically by using a [source code version control system](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span></span>

## <a name="make-a-change-to-deploy"></a><span data-ttu-id="e0c6b-111">Změňte k nasazení</span><span class="sxs-lookup"><span data-stu-id="e0c6b-111">Make a change to deploy</span></span>

<span data-ttu-id="e0c6b-112">Na stránce o aktuálně zobrazuje kód šablony.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-112">Currently the About page displays the template code.</span></span>

![O stránce s kód šablony](command-line-deployment/_static/image1.png)

<span data-ttu-id="e0c6b-114">Budete, nahraďte s kódem, který zobrazuje souhrn student registrace.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-114">You'll replace that with code that displays a summary of student enrollment.</span></span>

<span data-ttu-id="e0c6b-115">Otevřete *About.aspx* stránky, odstranit všechny značky uvnitř `MainContent` `Content` elementu a vložte následující kód na příslušné místo:</span><span class="sxs-lookup"><span data-stu-id="e0c6b-115">Open the *About.aspx* page, delete all of the markup inside the `MainContent` `Content` element, and insert the following markup in its place:</span></span>

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

<span data-ttu-id="e0c6b-116">Spusťte projekt a vyberte **o** stránky.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-116">Run the project and select the **About** page.</span></span>

![O stránku](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a><span data-ttu-id="e0c6b-118">Nasazení do testu pomocí příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="e0c6b-118">Deploy to Test by using the command line</span></span>

<span data-ttu-id="e0c6b-119">Nebude nasazování další změnou databáze, tak zakázat dbDacFx databáze nasazení pro databázi aspnet ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-119">You won't be deploying another database change, so disable dbDacFx database deployment for the aspnet-ContosoUniversity database.</span></span> <span data-ttu-id="e0c6b-120">Otevřete **Publikovat Web** průvodce a v každém ze tří publikační profily, zrušte **aktualizace databáze** políčko **nastavení** kartě.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-120">Open the **Publish Web** wizard, and in each of the three publish profiles, clear the **Update Database** check box on the **Settings** tab.</span></span>

<span data-ttu-id="e0c6b-121">Na stránce Start systému Windows 8 vyhledejte **příkazový řádek vývojáře pro VS2012**.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-121">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**.</span></span>

<span data-ttu-id="e0c6b-122">Klikněte pravým tlačítkem myši na ikonu **příkazový řádek vývojáře pro VS2012** a klikněte na tlačítko **spustit jako správce**.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-122">Right-click the icon for **Developer Command Prompt for VS2012** and click **Run as administrator**.</span></span>

<span data-ttu-id="e0c6b-123">Zadejte následující příkaz na příkazovém řádku, nahraďte cestu k souboru řešení cesta k souboru řešení:</span><span class="sxs-lookup"><span data-stu-id="e0c6b-123">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file:</span></span>

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

<span data-ttu-id="e0c6b-124">MSBuild sestavení řešení a nasadí ho na testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-124">MSBuild builds the solution and deploys it to the test environment.</span></span>

![Výstup příkazového řádku](command-line-deployment/_static/image3.png)

<span data-ttu-id="e0c6b-126">Otevřete prohlížeč a přejděte na `http://localhost/ContosoUniversity`, klikněte **o** a ověřte, že nasazení bylo úspěšné.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-126">Open a browser and go to `http://localhost/ContosoUniversity`, then click the **About** page to verify that the deployment was successful.</span></span>

<span data-ttu-id="e0c6b-127">Pokud jste nevytvořili žádné studenty v testu, zobrazí se prázdná stránka v části **Student textu statistiky** záhlaví.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-127">If you haven't created any students in test, you'll see an empty page under the **Student Body Statistics** heading.</span></span> <span data-ttu-id="e0c6b-128">Přejděte na **studenty** klikněte na tlačítko **přidat Student**a přidejte některé studenty a pak se vraťte do **o** stránky zobrazíte student statistiky.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-128">Go to the **Students** page, click **Add Student**, and add some students, and then return to the **About** page to see student statistics.</span></span>

![O stránku v testovacím prostředí](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a><span data-ttu-id="e0c6b-130">Možnosti klíče příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="e0c6b-130">Key command line options</span></span>

<span data-ttu-id="e0c6b-131">Příkaz, který jste zadali cestu k souboru řešení a dvě vlastnosti předaný MSBuild:</span><span class="sxs-lookup"><span data-stu-id="e0c6b-131">The command that you entered passed the solution file path and two properties to MSBuild:</span></span>

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a><span data-ttu-id="e0c6b-132">Nasazení řešení oproti nasazení jednotlivých projektů</span><span class="sxs-lookup"><span data-stu-id="e0c6b-132">Deploying the solution versus deploying individual projects</span></span>

<span data-ttu-id="e0c6b-133">Určení soubor řešení způsobí, že všechny projekty v řešení, která má být sestaven.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-133">Specifying the solution file causes all projects in the solution to be built.</span></span> <span data-ttu-id="e0c6b-134">Pokud máte více webové projekty v řešení, platí následující chování MSBuild:</span><span class="sxs-lookup"><span data-stu-id="e0c6b-134">If you have multiple web projects in the solution, the following MSBuild behavior applies:</span></span>

- <span data-ttu-id="e0c6b-135">Vlastnosti, které zadáte v příkazovém řádku se předávají na všechny projekty.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-135">The properties that you specify on the command line are passed to every project.</span></span> <span data-ttu-id="e0c6b-136">Každý webový projekt proto musí mít profil publikování se název, který zadáte.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-136">Therefore, each web project must have a publish profile with the name that you specify.</span></span> <span data-ttu-id="e0c6b-137">Pokud zadáte `/p:PublishProfile=Test`, každý webový projekt musí mít profilu publikování s názvem *Test*.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-137">If you specify `/p:PublishProfile=Test`, each web project must have a publish profile named *Test*.</span></span>
- <span data-ttu-id="e0c6b-138">Pokud jiný není i sestavení může úspěšně publikování jeden projektu.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-138">You might successfully publish one project when another one doesn't even build.</span></span> <span data-ttu-id="e0c6b-139">Další informace najdete v tématu vlákno stackoverflow [MSBuild nezdaří a zobrazí se dva balíčky](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span><span class="sxs-lookup"><span data-stu-id="e0c6b-139">For more information, see the stackoverflow thread [MSBuild fails with two packages](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span></span>

<span data-ttu-id="e0c6b-140">Pokud zadáte jednotlivých projektů místo řešení, je nutné přidat parametr, který určuje verzi sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-140">If you specify an individual project instead of a solution, you have to add a parameter that specifies the Visual Studio version.</span></span> <span data-ttu-id="e0c6b-141">Pokud používáte Visual Studio 2012 příkazového řádku by podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="e0c6b-141">If you are using Visual Studio 2012 the command line would be similar to the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

<span data-ttu-id="e0c6b-142">Číslo verze pro sadu Visual Studio 2010 je 10.0.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-142">The version number for Visual Studio 2010 is 10.0.</span></span> <span data-ttu-id="e0c6b-143">Další informace najdete v tématu [kompatibility projektu sady Visual Studio a VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) na blogu Sayed Hashimi.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-143">For more information, see [Visual Studio project compatability and VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) on Sayed Hashimi's blog.</span></span>

### <a name="specifying-the-publish-profile"></a><span data-ttu-id="e0c6b-144">Určení profilu publikování</span><span class="sxs-lookup"><span data-stu-id="e0c6b-144">Specifying the publish profile</span></span>

<span data-ttu-id="e0c6b-145">Můžete zadat profil publikování, podle názvu nebo podle úplnou cestu k *.pubxml* souboru, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="e0c6b-145">You can specify the publish profile by name or by the full path to the *.pubxml* file, as shown in the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a><span data-ttu-id="e0c6b-146">Metody pro příkazového řádku publikování podporuje publikování webu</span><span class="sxs-lookup"><span data-stu-id="e0c6b-146">Web publish methods supported for command-line publishing</span></span>

<span data-ttu-id="e0c6b-147">Tři publikování metody jsou podporovány pro publikování příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="e0c6b-147">Three publish methods are supported for command line publishing:</span></span>

- <span data-ttu-id="e0c6b-148">`MSDeploy` -Publikujte pomocí nástroje nasazení webu.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-148">`MSDeploy` - Publish by using Web Deploy.</span></span>
- <span data-ttu-id="e0c6b-149">`Package` -Publikujte vytvořením balíčku pro nasazení webu.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-149">`Package` - Publish by creating a Web Deploy Package.</span></span> <span data-ttu-id="e0c6b-150">Je nutné nainstalovat samostatně z nástroje MSBuild příkaz, který ji vytvoří.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-150">You have to install the package separately from the MSBuild command that creates it.</span></span>
- <span data-ttu-id="e0c6b-151">`FileSystem` -Publikujte pomocí kopírování souborů do zadané složky.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-151">`FileSystem` - Publish by copying files to a specified folder.</span></span>

### <a name="specifying-the-build-configuration-and-platform"></a><span data-ttu-id="e0c6b-152">Konfigurace sestavení a platformy</span><span class="sxs-lookup"><span data-stu-id="e0c6b-152">Specifying the build configuration and platform</span></span>

<span data-ttu-id="e0c6b-153">Konfigurace sestavení a platformy musí být nastavena v sadě Visual Studio nebo na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-153">The build configuration and platform must be set in Visual Studio or on the command line.</span></span> <span data-ttu-id="e0c6b-154">Profily publikování zahrnují vlastnosti, které jsou s názvem `LastUsedBuildConfiguration` a `LastUsedPlatform`, ale aby bylo možné zjistit, jak sestavení projektu nelze nastavit tyto vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-154">The publish profiles include properties that are named `LastUsedBuildConfiguration` and `LastUsedPlatform`, but you can't set these properties in order to determine how the project is built.</span></span> <span data-ttu-id="e0c6b-155">Další informace najdete v tématu [MSBuild: jak nastavit vlastnost konfigurace](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) na blogu Sayed Hashimi.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-155">For more information, see [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) on Sayed Hashimi's blog.</span></span>

## <a name="deploy-to-staging"></a><span data-ttu-id="e0c6b-156">Nasazení do přípravy</span><span class="sxs-lookup"><span data-stu-id="e0c6b-156">Deploy to staging</span></span>

<span data-ttu-id="e0c6b-157">Pokud chcete nasadit do Azure, musíte přidat heslo na příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-157">To deploy to Azure, you must add the password to the command line.</span></span> <span data-ttu-id="e0c6b-158">Pokud jste uložili heslo v profilu publikování v sadě Visual Studio, byla uložená v šifrovaném formátu v vaše *. pubxml.user* souboru.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-158">If you saved the password in the publish profile in Visual Studio, it was stored in encrypted form in the your *.pubxml.user* file.</span></span> <span data-ttu-id="e0c6b-159">Tento soubor není přístup MSBuild při nasazení příkazového řádku, takže budete muset předat heslo v parametru příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-159">That file is not accessed by MSBuild when you do a command line deployment, so you have to pass in the password in a command line parameter.</span></span>

1. <span data-ttu-id="e0c6b-160">Kopírovat heslo, které je nutné z *.publishsettings* souboru, který jste stáhli dříve pro pracovní webový server.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-160">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the staging web site.</span></span> <span data-ttu-id="e0c6b-161">Heslo je hodnota `userPWD` atribut pro nasazení webu `publishProfile` elementu.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-161">The password is the value of the `userPWD` attribute for the Web Deploy `publishProfile` element.</span></span>

    ![Heslo nasadit web](command-line-deployment/_static/image5.png)
2. <span data-ttu-id="e0c6b-163">Na stránce Start systému Windows 8 vyhledejte **příkazový řádek vývojáře pro VS2012**a klikněte na ikonu a otevřete okno příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-163">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**, and click the icon to open the command prompt.</span></span> <span data-ttu-id="e0c6b-164">(Nemáte a otevře se jako správce tentokrát vzhledem k tomu, že nejsou nasazení pro službu IIS v místním počítači.)</span><span class="sxs-lookup"><span data-stu-id="e0c6b-164">(You don't have to open it as administrator this time because you aren't deploying to IIS on the local computer.)</span></span>
3. <span data-ttu-id="e0c6b-165">Zadejte následující příkaz na příkazovém řádku, nahraďte cestu k souboru řešení cestu k souboru řešení a heslo pomocí hesla:</span><span class="sxs-lookup"><span data-stu-id="e0c6b-165">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    <span data-ttu-id="e0c6b-166">Všimněte si, že tento příkazový řádek obsahuje další parametr: `/p:AllowUntrustedCertificate=true`.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-166">Notice that this command line includes an extra parameter: `/p:AllowUntrustedCertificate=true`.</span></span> <span data-ttu-id="e0c6b-167">Jak probíhá zápis tohoto kurzu, `AllowUntrustedCertificate` při publikování v Azure z příkazového řádku, musí být nastavena vlastnost.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-167">As this tutorial is being written, the `AllowUntrustedCertificate` property must be set when you publish to Azure from the command line.</span></span> <span data-ttu-id="e0c6b-168">Až bude vydaná oprava této chyby, nebude nutné tento parametr.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-168">When the fix for this bug is released, you won't need that parameter.</span></span>
4. <span data-ttu-id="e0c6b-169">Otevřete prohlížeč a přejděte na adresu URL vašeho webu pracovní a klikněte **o** a ověřte, že nasazení bylo úspěšné.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-169">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

    <span data-ttu-id="e0c6b-170">Jak už jste viděli dříve pro testovací prostředí, možná budete muset vytvořit některé studenty zobrazíte statistiky na **o** stránky.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-170">As you saw earlier for the test environment, you might have to create some students to see statistics on the **About** page.</span></span>

## <a name="deploy-to-production"></a><span data-ttu-id="e0c6b-171">Nasazení do produkčního prostředí</span><span class="sxs-lookup"><span data-stu-id="e0c6b-171">Deploy to production</span></span>

<span data-ttu-id="e0c6b-172">Je podobný procesu pro pracovní proces nasazení do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-172">The process for deploying to production is similar to the process for staging.</span></span>

1. <span data-ttu-id="e0c6b-173">Kopírovat heslo, které je nutné z *.publishsettings* souboru, který jste stáhli dříve pro produkční webový server.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-173">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the production web site.</span></span>
2. <span data-ttu-id="e0c6b-174">Otevřete **příkazový řádek vývojáře pro VS2012**.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-174">Open **Developer Command Prompt for VS2012**.</span></span>
3. <span data-ttu-id="e0c6b-175">Zadejte následující příkaz na příkazovém řádku, nahraďte cestu k souboru řešení cestu k souboru řešení a heslo pomocí hesla:</span><span class="sxs-lookup"><span data-stu-id="e0c6b-175">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    <span data-ttu-id="e0c6b-176">Pro skutečné produkční lokality, pokud taky došlo ke změně databáze by obvykle zkopírujete *aplikace\_offline.htm* souborů do lokality před nasazením a odstranit po úspěšné nasazení.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-176">For a real production site, if there was also a database change, you would typically copy the *app\_offline.htm* file to the site before deployment and delete it after successful deployment.</span></span>
4. <span data-ttu-id="e0c6b-177">Otevřete prohlížeč a přejděte na adresu URL vašeho webu pracovní a klikněte **o** a ověřte, že nasazení bylo úspěšné.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-177">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

## <a name="summary"></a><span data-ttu-id="e0c6b-178">Souhrn</span><span class="sxs-lookup"><span data-stu-id="e0c6b-178">Summary</span></span>

<span data-ttu-id="e0c6b-179">Nyní jste nasadili aktualizaci aplikace pomocí příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-179">You have now deployed an application update by using the command line.</span></span>

![O stránku v testovacím prostředí](command-line-deployment/_static/image6.png)

<span data-ttu-id="e0c6b-181">V dalším kurzu se zobrazí příklad toho, jak rozšířit webu kanálu publikování.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-181">In the next tutorial, you will see an example of how to extend the web publish pipeline.</span></span> <span data-ttu-id="e0c6b-182">V příkladu vám ukáže, jak nasadit soubory, které nejsou součástí projektu.</span><span class="sxs-lookup"><span data-stu-id="e0c6b-182">The example will show you how to deploy files that are not included in the project.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e0c6b-183">[Předchozí](deploying-a-database-update.md)
> [další](deploying-extra-files.md)</span><span class="sxs-lookup"><span data-stu-id="e0c6b-183">[Previous](deploying-a-database-update.md)
[Next](deploying-extra-files.md)</span></span>
