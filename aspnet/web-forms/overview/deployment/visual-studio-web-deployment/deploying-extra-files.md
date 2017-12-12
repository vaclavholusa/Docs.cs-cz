---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: "Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení dalších souborů | Microsoft Docs"
author: tdykstra
description: "Tato řada kurzu se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo do hostujícího zprostředkovatele třetí strany podle usin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: a34607b25f6cf502f5fbf2fe51bf1937f470159e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a><span data-ttu-id="4b58f-103">Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení dalších souborů</span><span class="sxs-lookup"><span data-stu-id="4b58f-103">ASP.NET Web Deployment using Visual Studio: Deploying Extra Files</span></span>
====================
<span data-ttu-id="4b58f-104">podle [tní Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="4b58f-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="4b58f-105">Stáhněte si úvodní projekt</span><span class="sxs-lookup"><span data-stu-id="4b58f-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="4b58f-106">Tato řada kurzu se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, pomocí sady Visual Studio 2012 nebo Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="4b58f-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="4b58f-107">Informace o řadě najdete v tématu [z prvního kurzu řady](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="4b58f-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="4b58f-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="4b58f-108">Overview</span></span>

<span data-ttu-id="4b58f-109">Tento kurz ukazuje, jak rozšířit publikování webů sady Visual Studio kanálu, který má provést dodatečnou úlohu během nasazení.</span><span class="sxs-lookup"><span data-stu-id="4b58f-109">This tutorial shows how to extend the Visual Studio web publish pipeline to do an additional task during deployment.</span></span> <span data-ttu-id="4b58f-110">Tato úloha je zkopírovat další soubory, které nejsou ve složce projektu k cílovému webu.</span><span class="sxs-lookup"><span data-stu-id="4b58f-110">The task is to copy extra files that are not in the project folder to the destination web site.</span></span>

<span data-ttu-id="4b58f-111">Pro účely tohoto kurzu budete zkopírovat jeden další soubor: *robots.txt*.</span><span class="sxs-lookup"><span data-stu-id="4b58f-111">For this tutorial you'll copy one extra file: *robots.txt*.</span></span> <span data-ttu-id="4b58f-112">Chcete nasadit tento soubor do přípravy, ale ne do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="4b58f-112">You want to deploy this file to staging but not to production.</span></span> <span data-ttu-id="4b58f-113">V [nasazení do produkčního prostředí](deploying-to-production.md) kurz, přidání tohoto souboru do projektu a nakonfigurování provozní publikování profilu, které chcete vyloučit.</span><span class="sxs-lookup"><span data-stu-id="4b58f-113">In [the Deploying to Production](deploying-to-production.md) tutorial, you added this file to the project and configured the Production publish profile to exclude it.</span></span> <span data-ttu-id="4b58f-114">V tomto kurzu se zobrazí alternativní metoda pro zpracování této situaci, ten, který bude užitečná pro všechny soubory, které chcete nasadit, ale nechcete, aby pro zahrnutí do projektu.</span><span class="sxs-lookup"><span data-stu-id="4b58f-114">In this tutorial you'll see an alternative method to handle this situation, one that will be useful for any files that you want to deploy but don't want to include in the project.</span></span>

## <a name="move-the-robotstxt-file"></a><span data-ttu-id="4b58f-115">Přesuňte soubor robots.txt</span><span class="sxs-lookup"><span data-stu-id="4b58f-115">Move the robots.txt file</span></span>

<span data-ttu-id="4b58f-116">Příprava na jinou metodu zpracování *robots.txt*, v této části kurzu přesunout soubor do složky, který není zahrnutý v projektu a odstraníte *robots.txt* z testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="4b58f-116">To prepare for a different method of handling *robots.txt*, in this section of the tutorial you move the file to a folder that is not included in the project, and you delete *robots.txt* from the staging environment.</span></span> <span data-ttu-id="4b58f-117">Je nezbytné k odstranění souboru z pracovní, aby mohli ověřit, zda vaše nové metody nasazení soubor v tomto prostředí pracuje správně.</span><span class="sxs-lookup"><span data-stu-id="4b58f-117">It is necessary to delete the file from staging so that you can verify that your new method of deploying the file to that environment is working correctly.</span></span>

1. <span data-ttu-id="4b58f-118">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *robots.txt* souboru a klikněte na tlačítko **vyloučit z projektu**.</span><span class="sxs-lookup"><span data-stu-id="4b58f-118">In **Solution Explorer**, right-click the *robots.txt* file and click **Exclude From Project**.</span></span>
2. <span data-ttu-id="4b58f-119">Pomocí Průzkumníka souborů Windows, vytvořte novou složku ve složce řešení a pojmenujte ji *ExtraFiles*.</span><span class="sxs-lookup"><span data-stu-id="4b58f-119">Using Windows File Explorer, create a new folder in the solution folder and name it *ExtraFiles*.</span></span>
3. <span data-ttu-id="4b58f-120">Přesunout *robots.txt* souboru z *ContosoUniversity* složky projektu na *ExtraFiles* složky.</span><span class="sxs-lookup"><span data-stu-id="4b58f-120">Move the *robots.txt* file from the *ContosoUniversity* project folder to the *ExtraFiles* folder.</span></span>

    ![ExtraFiles složky](deploying-extra-files/_static/image1.png)
4. <span data-ttu-id="4b58f-122">Pomocí nástroje vaší FTP odstranit *robots.txt* soubor z pracovních webu.</span><span class="sxs-lookup"><span data-stu-id="4b58f-122">Using your FTP tool, delete the *robots.txt* file from the staging web site.</span></span>

    <span data-ttu-id="4b58f-123">Alternativně můžete vybrat **odebrat další soubory v cílovém umístění** pod **možností publikování souboru** na **nastavení** kartě pracovní profil publikování, a znovu publikujte do přípravy.</span><span class="sxs-lookup"><span data-stu-id="4b58f-123">As an alternative, you can select **Remove additional files at destination** under **File Publish Options** on the **Settings** tab of the Staging publish profile, and republish to staging.</span></span>

## <a name="update-the-publish-profile-file"></a><span data-ttu-id="4b58f-124">Aktualizace souboru profilu publikování</span><span class="sxs-lookup"><span data-stu-id="4b58f-124">Update the publish profile file</span></span>

<span data-ttu-id="4b58f-125">Potřebujete jenom *robots.txt* v pracovním, tak je pracovní pouze profil publikování, je potřeba aktualizovat, aby bylo možné ho nasadit.</span><span class="sxs-lookup"><span data-stu-id="4b58f-125">You only need *robots.txt* in staging, so the only publish profile you need to update in order to deploy it is Staging.</span></span>

1. <span data-ttu-id="4b58f-126">V sadě Visual Studio otevřete *Staging.pubxml*.</span><span class="sxs-lookup"><span data-stu-id="4b58f-126">In Visual Studio, open *Staging.pubxml*.</span></span>
2. <span data-ttu-id="4b58f-127">Na konci souboru, před uzavírací `</Project>` značky, přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="4b58f-127">At the end of the file, before the closing `</Project>` tag, add the following markup:</span></span>

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    <span data-ttu-id="4b58f-128">Tento kód vytvoří novou *cíl* , bude shromažďovat další soubory k nasazení.</span><span class="sxs-lookup"><span data-stu-id="4b58f-128">This code creates a new *target* that will collect additional files to be deployed.</span></span> <span data-ttu-id="4b58f-129">Cíl se skládá z jednoho nebo více úloh, které budou spuštěny MSBuild na základě podmínek, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="4b58f-129">A target is composed of one or more tasks that MSBuild will execute based on conditions you specify.</span></span>

    <span data-ttu-id="4b58f-130">`Include` Atribut určuje, že složka, ve kterém chcete najít soubory je *ExtraFiles*, který je umístěn na stejné úrovni jako složce projektu.</span><span class="sxs-lookup"><span data-stu-id="4b58f-130">The `Include` attribute specifies that the folder in which to find the files is *ExtraFiles*, located at the same level as the project folder.</span></span> <span data-ttu-id="4b58f-131">MSBuild bude shromažďovat všechny soubory z této složky a rekurzivně ze všech podsložek (dvojitý znak hvězdičky určuje rekurzivní podsložek).</span><span class="sxs-lookup"><span data-stu-id="4b58f-131">MSBuild will collect all files from that folder and recursively from any subfolders (the double asterisk specifies recursive subfolders).</span></span> <span data-ttu-id="4b58f-132">S tímto kódem by mohlo více souborů a souborů v podsložkách uvnitř *ExtraFiles* složky a všech bude nasazeno.</span><span class="sxs-lookup"><span data-stu-id="4b58f-132">With this code you could put multiple files, and files in subfolders inside the *ExtraFiles* folder, and all will be deployed.</span></span>

    <span data-ttu-id="4b58f-133">`DestinationRelativePath` Element určuje, že složek a souborů, by měl být zkopírovány do kořenové složky webu cílové ve stejné struktuře souborů a složek, protože se nenachází v *ExtraFiles* složky.</span><span class="sxs-lookup"><span data-stu-id="4b58f-133">The `DestinationRelativePath` element specifies that the folders and files should be copied to the root folder of the destination web site, in the same file and folder structure as they are found in the *ExtraFiles* folder.</span></span> <span data-ttu-id="4b58f-134">Pokud chcete zkopírovat *ExtraFiles* samotné, složce `DestinationRelativePath` hodnotu *ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*.</span><span class="sxs-lookup"><span data-stu-id="4b58f-134">If you wanted to copy the *ExtraFiles* folder itself, the `DestinationRelativePath` value would be *ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*.</span></span>
3. <span data-ttu-id="4b58f-135">Na konci souboru, před uzavírací `</Project>` značky, přidejte následující kód, který určuje, kdy k provedení nového cíle.</span><span class="sxs-lookup"><span data-stu-id="4b58f-135">At the end of the file, before the closing `</Project>` tag, add the following markup that specifies when to execute the new target.</span></span>

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    <span data-ttu-id="4b58f-136">Tento kód způsobí, že nové `CustomCollectFiles` cíle provést vždy, když se spustí cíl, který zkopíruje soubory do cílové složky.</span><span class="sxs-lookup"><span data-stu-id="4b58f-136">This code causes the new `CustomCollectFiles` target to be executed whenever the target that copies files to the destination folder is executed.</span></span> <span data-ttu-id="4b58f-137">Je samostatný cíle pro publikování a vytvoření balíčku nasazení a v případě, že se rozhodnete nasadit místo publikování pomocí balíčku pro nasazení, je nová cílová vložit v obou cíle.</span><span class="sxs-lookup"><span data-stu-id="4b58f-137">There is a separate target for publish versus deployment package creation, and the new target is injected in both targets in case you decide to deploy by using a deployment package instead of publishing.</span></span>

    <span data-ttu-id="4b58f-138">*.Pubxml* soubor bude vypadat jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="4b58f-138">The *.pubxml* file now looks like the following example:</span></span>

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. <span data-ttu-id="4b58f-139">Uložte a zavřete *Staging.pubxml* souboru.</span><span class="sxs-lookup"><span data-stu-id="4b58f-139">Save and close the *Staging.pubxml* file.</span></span>

## <a name="publish-to-staging"></a><span data-ttu-id="4b58f-140">Publikování do přípravy</span><span class="sxs-lookup"><span data-stu-id="4b58f-140">Publish to staging</span></span>

<span data-ttu-id="4b58f-141">Použití jedním kliknutím publikovat nebo příkazového řádku, publikování aplikace pomocí pracovní profil.</span><span class="sxs-lookup"><span data-stu-id="4b58f-141">Using one-click publish or the command line, publish the application by using the Staging profile.</span></span>

<span data-ttu-id="4b58f-142">Pokud používáte jedním kliknutím publikovat, můžete ověřit v **Preview** okno, *robots.txt* budou zkopírovány.</span><span class="sxs-lookup"><span data-stu-id="4b58f-142">If you use one-click publish, you can verify in the **Preview** window that *robots.txt* will be copied.</span></span> <span data-ttu-id="4b58f-143">Jinak používat vaše nástroje FTP a ověřte, zda *robots.txt* soubor nachází v kořenové složce webové stránky po nasazení.</span><span class="sxs-lookup"><span data-stu-id="4b58f-143">Otherwise, use your FTP tool to verify that the *robots.txt* file is in the root folder of the web site after deployment.</span></span>

## <a name="summary"></a><span data-ttu-id="4b58f-144">Souhrn</span><span class="sxs-lookup"><span data-stu-id="4b58f-144">Summary</span></span>

<span data-ttu-id="4b58f-145">Dokončení této série kurzů k nasazení do hostujícího zprostředkovatele třetí strany webovou aplikaci ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4b58f-145">This completes this series of tutorials on deploying an ASP.NET web application to a third-party hosting provider.</span></span> <span data-ttu-id="4b58f-146">Další informace o všech témat zahrnutých v těchto kurzech najdete v tématu [mapa obsahu nasazení ASP.NET](https://go.microsoft.com/fwlink/p/?LinkId=282413).</span><span class="sxs-lookup"><span data-stu-id="4b58f-146">For more information about any of the topics covered in these tutorials, see the [ASP.NET Deployment Content Map](https://go.microsoft.com/fwlink/p/?LinkId=282413).</span></span>

## <a name="more-information"></a><span data-ttu-id="4b58f-147">Další informace</span><span class="sxs-lookup"><span data-stu-id="4b58f-147">More information</span></span>

<span data-ttu-id="4b58f-148">Pokud umíte pracovat se soubory nástroje MSBuild, můžete automatizovat celou řadu dalších úloh nasazení podle psaní kódu v *.pubxml* soubory (pro úlohy specifické pro profil) nebo projektu *. wpp.targets* souboru (pro úkoly, které platí pro všechny profily).</span><span class="sxs-lookup"><span data-stu-id="4b58f-148">If you know how to work with MSBuild files, you can automate many other deployment tasks by writing code in *.pubxml* files (for profile-specific tasks) or the project *.wpp.targets* file (for tasks that apply to all profiles).</span></span> <span data-ttu-id="4b58f-149">Další informace o *.pubxml* a *. wpp.targets* soubory, najdete v části [postup: Upravit nastavení nasazení v souborech profil publikování (.pubxml) a. wpp.targets souboru v aplikaci Visual Studio Web Projekty](https://msdn.microsoft.com/en-us/library/ff398069).</span><span class="sxs-lookup"><span data-stu-id="4b58f-149">For more information about *.pubxml* and *.wpp.targets* files, see [How to: Edit Deployment Settings in Publish Profile (.pubxml) Files and the .wpp.targets File in Visual Studio Web Projects](https://msdn.microsoft.com/en-us/library/ff398069).</span></span> <span data-ttu-id="4b58f-150">Základní informace o MSBuild kód, najdete v části **The anatomie soubor projektu** v [Enterprise nasazení řady: Principy souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span><span class="sxs-lookup"><span data-stu-id="4b58f-150">For a basic introduction to MSBuild code, see **The Anatomy of a Project File** in [Enterprise Deployment Series: Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span></span> <span data-ttu-id="4b58f-151">Naučte se pracovat se soubory nástroje MSBuild úkoly pro vaše vlastní scénáře, najdete v článku tato kniha: [uvnitř Microsoft Build Engine: pomocí nástroje MSBuild a Team Foundation Build](http://msbuildbook.com) Sayed Ibraham Hashimi a William Bartholomew.</span><span class="sxs-lookup"><span data-stu-id="4b58f-151">To learn how to work with MSBuild files to perform tasks for your own scenarios, see this book: [Inside the Microsoft Build Engine: Using MSBuild and Team Foundation Build](http://msbuildbook.com) by Sayed Ibraham Hashimi and William Bartholomew.</span></span>

## <a name="acknowledgements"></a><span data-ttu-id="4b58f-152">Potvrzení</span><span class="sxs-lookup"><span data-stu-id="4b58f-152">Acknowledgements</span></span>

<span data-ttu-id="4b58f-153">Děkujeme, že následující osob, které významně přispěli k obsahu tento kurz řady chci:</span><span class="sxs-lookup"><span data-stu-id="4b58f-153">I would like to thank the following people who made significant contributions to the content of this tutorial series:</span></span>

- [<span data-ttu-id="4b58f-154">Alberto Poblacion, MVP &amp; MCT, Španělsko</span><span class="sxs-lookup"><span data-stu-id="4b58f-154">Alberto Poblacion, MVP &amp; MCT, Spain</span></span>](https://mvp.microsoft.com/en-us/mvp/Alberto%20Poblacion%20Bolano-36772)
- <span data-ttu-id="4b58f-155">Jarod Ferguson, vývoj platformy Data MVP, Spojené státy</span><span class="sxs-lookup"><span data-stu-id="4b58f-155">Jarod Ferguson, Data Platform Development MVP, United States</span></span>
- <span data-ttu-id="4b58f-156">Nevlídné Mittal, Microsoft</span><span class="sxs-lookup"><span data-stu-id="4b58f-156">Harsh Mittal, Microsoft</span></span>
- <span data-ttu-id="4b58f-157">[Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))</span><span class="sxs-lookup"><span data-stu-id="4b58f-157">[Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [@jongalloway](http://twitter.com/jongalloway))</span></span>
- [<span data-ttu-id="4b58f-158">Kristina Olson Microsoft</span><span class="sxs-lookup"><span data-stu-id="4b58f-158">Kristina Olson, Microsoft</span></span>](https://blogs.iis.net/krolson/default.aspx)
- [<span data-ttu-id="4b58f-159">Jan Pope, Microsoft</span><span class="sxs-lookup"><span data-stu-id="4b58f-159">Mike Pope, Microsoft</span></span>](http://www.mikepope.com/blog/DisplayBlog.aspx)
- <span data-ttu-id="4b58f-160">Mohit Srivastava, Microsoft</span><span class="sxs-lookup"><span data-stu-id="4b58f-160">Mohit Srivastava, Microsoft</span></span>
- [<span data-ttu-id="4b58f-161">Raffaele Rialdi, Itálie</span><span class="sxs-lookup"><span data-stu-id="4b58f-161">Raffaele Rialdi, Italy</span></span>](http://www.iamraf.net/)
- [<span data-ttu-id="4b58f-162">Rick Anderson Microsoft</span><span class="sxs-lookup"><span data-stu-id="4b58f-162">Rick Anderson, Microsoft</span></span>](https://blogs.msdn.com/b/rickandy/)
- <span data-ttu-id="4b58f-163">[SAYED Hashimi společnost Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))</span><span class="sxs-lookup"><span data-stu-id="4b58f-163">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))</span></span>
- <span data-ttu-id="4b58f-164">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))</span><span class="sxs-lookup"><span data-stu-id="4b58f-164">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))</span></span>
- <span data-ttu-id="4b58f-165">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))</span><span class="sxs-lookup"><span data-stu-id="4b58f-165">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))</span></span>
- [<span data-ttu-id="4b58f-166">Srđan Božović, Srbsko</span><span class="sxs-lookup"><span data-stu-id="4b58f-166">Srđan Božović, Serbia</span></span>](http://msforge.net/blogs/zmajcek/)
- <span data-ttu-id="4b58f-167">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))</span><span class="sxs-lookup"><span data-stu-id="4b58f-167">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="4b58f-168">[Předchozí](command-line-deployment.md)
[další](troubleshooting.md)</span><span class="sxs-lookup"><span data-stu-id="4b58f-168">[Previous](command-line-deployment.md)
[Next](troubleshooting.md)</span></span>
