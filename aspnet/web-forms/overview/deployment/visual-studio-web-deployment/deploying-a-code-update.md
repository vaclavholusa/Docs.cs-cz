---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení aktualizace kódu | Microsoft Docs'
author: tdykstra
description: Tato řada kurzu se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo do hostujícího zprostředkovatele třetí strany podle usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: dd02b5c627fbfbb0034030f4c21207d24f6aabce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a><span data-ttu-id="5c2de-103">Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení aktualizace kódu</span><span class="sxs-lookup"><span data-stu-id="5c2de-103">ASP.NET Web Deployment using Visual Studio: Deploying a Code Update</span></span>
====================
<span data-ttu-id="5c2de-104">Podle [tní Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="5c2de-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="5c2de-105">Stáhněte si úvodní projekt</span><span class="sxs-lookup"><span data-stu-id="5c2de-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="5c2de-106">Tato řada kurzu se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, pomocí sady Visual Studio 2012 nebo Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="5c2de-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="5c2de-107">Informace o řadě najdete v tématu [z prvního kurzu řady](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5c2de-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="5c2de-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="5c2de-108">Overview</span></span>

<span data-ttu-id="5c2de-109">Po počátečním nasazení pokračuje v práci údržbu a vývoj vašeho webu a před dlouho bude chtít nasadit aktualizace.</span><span class="sxs-lookup"><span data-stu-id="5c2de-109">After the initial deployment, your work of maintaining and developing your web site continues, and before long you will want to deploy an update.</span></span> <span data-ttu-id="5c2de-110">Tento kurz vás provede procesem nasazení aktualizace do kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="5c2de-110">This tutorial takes you through the process of deploying an update to your application code.</span></span> <span data-ttu-id="5c2de-111">Aktualizace, která můžete implementovat a nasazení v tomto kurzu nezahrnuje změnu databáze; Zobrazí se, co se liší o nasazení změn databáze v dalším kurzu.</span><span class="sxs-lookup"><span data-stu-id="5c2de-111">The update that you implement and deploy in this tutorial does not involve a database change; you'll see what's different about deploying a database change in the next tutorial.</span></span>

<span data-ttu-id="5c2de-112">Upozornění: Pokud se zobrazí chybové hlášení, nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [řešení potíží s stránky](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="5c2de-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="make-a-code-change"></a><span data-ttu-id="5c2de-113">Ujistěte se, změňte kód</span><span class="sxs-lookup"><span data-stu-id="5c2de-113">Make a code change</span></span>

<span data-ttu-id="5c2de-114">Jako jednoduchý příklad aktualizace do vaší aplikace, přidáte do **vyučující** stránky Seznam kurzů výukové podle vybrané lektorem.</span><span class="sxs-lookup"><span data-stu-id="5c2de-114">As a simple example of an update to your application, you'll add to the **Instructors** page a list of courses taught by the selected instructor.</span></span>

<span data-ttu-id="5c2de-115">Pokud spustíte **vyučující** stránky, můžete si všimnout, že existují **vyberte** odkazy v mřížce, ale přitom nedělají nic jakoukoli jinou hodnotu než zkontrolujte šedá řádek pozadí zapnout.</span><span class="sxs-lookup"><span data-stu-id="5c2de-115">If you run the **Instructors** page, you'll notice that there are **Select** links in the grid, but they don't do anything other than make the row background turn gray.</span></span>

![Vyučující stránky s výběrem](deploying-a-code-update/_static/image1.png)

<span data-ttu-id="5c2de-117">Nyní přidáte kód, který běží, když **vyberte** po kliknutí na odkaz a zobrazí seznam kurzů výukové podle vybrané lektorem.</span><span class="sxs-lookup"><span data-stu-id="5c2de-117">Now you'll add code that runs when the **Select** link is clicked and displays a list of courses taught by the selected instructor .</span></span>

1. <span data-ttu-id="5c2de-118">V *Instructors.aspx*, přidejte následující kód bezprostředně po **ErrorMessageLabel** `Label` ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="5c2de-118">In *Instructors.aspx*, add the following markup immediately after the **ErrorMessageLabel** `Label` control:</span></span>

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. <span data-ttu-id="5c2de-119">Spuštění stránky a vyberte lektorem.</span><span class="sxs-lookup"><span data-stu-id="5c2de-119">Run the page and select an instructor.</span></span> <span data-ttu-id="5c2de-120">Zobrazí seznam kurzů výukové podle této lektorem.</span><span class="sxs-lookup"><span data-stu-id="5c2de-120">You see a list of courses taught by that instructor.</span></span>

    ![Stránka vyučující s kurzy výukové](deploying-a-code-update/_static/image2.png)
3. <span data-ttu-id="5c2de-122">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="5c2de-122">Close the browser.</span></span>

## <a name="deploy-the-code-update-to-the-test-environment"></a><span data-ttu-id="5c2de-123">Kód aktualizace nasadit do testovacího prostředí</span><span class="sxs-lookup"><span data-stu-id="5c2de-123">Deploy the code update to the test environment</span></span>

<span data-ttu-id="5c2de-124">Předtím, než vaše profilů publikování můžete použít k nasazení na testovací, přípravné nebo produkční prostředí, budete muset změnit možnosti publikování databáze.</span><span class="sxs-lookup"><span data-stu-id="5c2de-124">Before you can use your publish profiles to deploy to test, staging, and production, you need to change database publishing options.</span></span> <span data-ttu-id="5c2de-125">Už nemusí spouštět skripty nasazení grant a dat pro databázi členství.</span><span class="sxs-lookup"><span data-stu-id="5c2de-125">You no longer need to run the grant and data deployment scripts for the membership database.</span></span>

1. <span data-ttu-id="5c2de-126">Otevřete **Publikovat Web** Průvodce pravým tlačítkem na projekt ContosoUniversity a kliknutím na **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="5c2de-126">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="5c2de-127">Klikněte na tlačítko **Test** profilu v **profil** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="5c2de-127">Click the **Test** profile in the **Profile** drop-down list.</span></span>
3. <span data-ttu-id="5c2de-128">Klikněte **nastavení** kartě.</span><span class="sxs-lookup"><span data-stu-id="5c2de-128">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="5c2de-129">V části **objekt DefaultConnection** v **databáze** část, zrušte **aktualizace databáze** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="5c2de-129">Under **DefaultConnection** in the **Databases** section, clear the **Update database** check box.</span></span>
5. <span data-ttu-id="5c2de-130">Klikněte na tlačítko **profil** a pak klikněte **pracovní** profilu v **profil** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="5c2de-130">Click the **Profile** tab, and then click the **Staging** profile in the **Profile** drop-down list.</span></span>
6. <span data-ttu-id="5c2de-131">Když se zobrazí výzva Pokud chcete uložit změny provedené **Test** profilu, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="5c2de-131">When you are asked if you want to save the changes made to the **Test** profile, click **Yes**.</span></span>
7. <span data-ttu-id="5c2de-132">Proveďte požadovanou změnu stejné v pracovní profil.</span><span class="sxs-lookup"><span data-stu-id="5c2de-132">Make the same change in the Staging profile.</span></span>
8. <span data-ttu-id="5c2de-133">Opakujte tento proces zpřístupnění stejné změny v produkčním profilu.</span><span class="sxs-lookup"><span data-stu-id="5c2de-133">Repeat the process to make the same change in the Production profile.</span></span>
9. <span data-ttu-id="5c2de-134">Zavřít **Publikovat Web** průvodce.</span><span class="sxs-lookup"><span data-stu-id="5c2de-134">Close the **Publish Web** wizard.</span></span>

<span data-ttu-id="5c2de-135">Nasazení do testovacího prostředí se nyní znovu publikovat jednoduché spuštění jedním kliknutím.</span><span class="sxs-lookup"><span data-stu-id="5c2de-135">Deploying to the test environment is now a simple matter of running one-click publish again.</span></span> <span data-ttu-id="5c2de-136">Chcete-li tento proces rychlejší, můžete použít **publikování webu jedním kliknutím** panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="5c2de-136">To make this process quicker, you can use the **Web One Click Publish** toolbar.</span></span>

1. <span data-ttu-id="5c2de-137">V **zobrazení** nabídce zvolte **panely nástrojů** a pak vyberte **publikování webu jedním kliknutím**.</span><span class="sxs-lookup"><span data-stu-id="5c2de-137">In the **View** menu, choose **Toolbars** and then select **Web One Click Publish**.</span></span>

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. <span data-ttu-id="5c2de-139">V **Průzkumníku**, vyberte ContosoUniversity projekt.</span><span class="sxs-lookup"><span data-stu-id="5c2de-139">In **Solution Explorer**, select the ContosoUniversity project.</span></span>
3. <span data-ttu-id="5c2de-140">**publikování webu jedním kliknutím** nástrojů, vyberte **Test** profil publikování a pak klikněte na **Publikovat Web** (ikona s dvojice šipek, které ukazují doleva a doprava).</span><span class="sxs-lookup"><span data-stu-id="5c2de-140">the **Web One Click Publish** toolbar, choose the **Test** publish profile and then click **Publish Web** (the icon with arrows pointing left and right).</span></span>

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. <span data-ttu-id="5c2de-142">Visual Studio nasadí aktualizovanou aplikaci a prohlížeč se automaticky otevře na domovskou stránku.</span><span class="sxs-lookup"><span data-stu-id="5c2de-142">Visual Studio deploys the updated application, and the browser automatically opens to the home page.</span></span>
5. <span data-ttu-id="5c2de-143">Spuštění stránky vyučující a vyberte lektorem k ověření, že aktualizace byla úspěšně nasazena.</span><span class="sxs-lookup"><span data-stu-id="5c2de-143">Run the Instructors page and select an instructor to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="5c2de-144">Za normálních okolností byste také provést testování regrese (tedy testovací zbytek lokality a ujistěte se, že nové změny nebyla rozdělit žádné existující funkce).</span><span class="sxs-lookup"><span data-stu-id="5c2de-144">You would normally also do regression testing (that is, test the rest of the site to make sure that the new change didn't break any existing functionality).</span></span> <span data-ttu-id="5c2de-145">Ale pro účely tohoto kurzu budete tento krok přeskočit a přejít k aktualizace nasadit do pracovní a provozní.</span><span class="sxs-lookup"><span data-stu-id="5c2de-145">But for this tutorial you'll skip that step and proceed to deploy the update to staging and production.</span></span>

<span data-ttu-id="5c2de-146">Při opětovném, Web Deploy automaticky určuje, které soubory se změnili a změnit pouze kopie souborů na server.</span><span class="sxs-lookup"><span data-stu-id="5c2de-146">When you redeploy, Web Deploy automatically determines which files have changed and only copies changed files to the server.</span></span> <span data-ttu-id="5c2de-147">Ve výchozím nastavení nasazení webu pomocí poslední změny dat na soubory určuje ty, které se změnily.</span><span class="sxs-lookup"><span data-stu-id="5c2de-147">By default, Web Deploy uses last-changed dates on files to determine which ones have changed.</span></span> <span data-ttu-id="5c2de-148">Některé systémy správy zdrojového souboru data změnit i když neměnit obsah souboru.</span><span class="sxs-lookup"><span data-stu-id="5c2de-148">Some source control systems change file dates even when you don't change the file contents.</span></span> <span data-ttu-id="5c2de-149">V takovém případě můžete chtít nakonfigurovat nasazení webu použít kontrolní součty souborů k určení, které soubory se změnily.</span><span class="sxs-lookup"><span data-stu-id="5c2de-149">In that case, you might want to configure Web Deploy to use file checksums to determine which files have changed.</span></span> <span data-ttu-id="5c2de-150">Další informace najdete v tématu [Proč všechny soubory získat znovu nasazena i když nebyla je změnit?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) v části Nejčastější dotazy pro nasazení technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5c2de-150">For more information, see [Why do all of my files get redeployed although I didn't change them?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) in the ASP.NET Deployment FAQ.</span></span>

## <a name="take-the-application-offline-during-deployment"></a><span data-ttu-id="5c2de-151">Trvat aplikace do režimu offline během nasazení</span><span class="sxs-lookup"><span data-stu-id="5c2de-151">Take the application offline during deployment</span></span>

<span data-ttu-id="5c2de-152">Změna, kterou nasazujete nyní je jednoduchá změna na jednu stránku.</span><span class="sxs-lookup"><span data-stu-id="5c2de-152">The change you're deploying now is a simple change to a single page.</span></span> <span data-ttu-id="5c2de-153">Ale někdy nasadit větší změny, nebo nasazení změn kódu a databáze a lokality mohou chovat správně, pokud uživatel požádá o na stránce před dokončení nasazení.</span><span class="sxs-lookup"><span data-stu-id="5c2de-153">But sometimes you deploy larger changes, or you deploy both code and database changes, and the site might behave incorrectly if a user requests a page before deployment is finished.</span></span> <span data-ttu-id="5c2de-154">Chcete-li zabránit uživatelům v přístupu na web, když probíhá nasazení, můžete použít *aplikace\_offline.htm* souboru.</span><span class="sxs-lookup"><span data-stu-id="5c2de-154">To prevent users from accessing the site while deployment is in progress, you can use an *app\_offline.htm* file.</span></span> <span data-ttu-id="5c2de-155">Když vložíte soubor s názvem *aplikace\_offline.htm* kořenové složky vaší aplikace, služba IIS automaticky zobrazí tento soubor namísto spuštění vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="5c2de-155">When you put a file named *app\_offline.htm* in the root folder of your application, IIS automatically displays that file instead of running your application.</span></span> <span data-ttu-id="5c2de-156">Tak, aby se zabránilo přístup při nasazení, můžete zadat *aplikace\_offline.htm* v kořenové složce, spustí proces nasazení a potom odeberte *aplikace\_offline.htm* po úspěšné nasazení.</span><span class="sxs-lookup"><span data-stu-id="5c2de-156">So to prevent access during deployment, you put *app\_offline.htm* in the root folder, run the deployment process, and then remove *app\_offline.htm* after successful deployment.</span></span>

<span data-ttu-id="5c2de-157">Můžete nakonfigurovat nasazení webu se automaticky umístí výchozí *aplikace\_offline.htm* souboru na serveru, když se spustí, nasazení a jeho odebrání po dokončení.</span><span class="sxs-lookup"><span data-stu-id="5c2de-157">You can configure Web Deploy to automatically put a default *app\_offline.htm* file on the server when it starts deploying and remove it when it finishes.</span></span> <span data-ttu-id="5c2de-158">Chcete, aby všechny je nutné provést je publikování souboru profilu (.pubxml) přidejte následující element XML:</span><span class="sxs-lookup"><span data-stu-id="5c2de-158">To do that all you have to do is add the following XML element to your publish profile (.pubxml) file:</span></span>

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

<span data-ttu-id="5c2de-159">V tomto kurzu uvidíte, jak vytvořit a používat vlastní *aplikace\_offline.htm* souboru.</span><span class="sxs-lookup"><span data-stu-id="5c2de-159">For this tutorial you'll see how to create and use a custom *app\_offline.htm* file.</span></span>

<span data-ttu-id="5c2de-160">Pomocí *aplikace\_offline.htm* v pracovní lokalitě není povinné, protože nemáte uživatele, kteří používají pracovní lokality.</span><span class="sxs-lookup"><span data-stu-id="5c2de-160">Using *app\_offline.htm* in the staging site isn't required, because you don't have users accessing the staging site.</span></span> <span data-ttu-id="5c2de-161">Ale je dobrým zvykem pomocí pracovní proveďte kompletní testování způsobem, ke kterému plánujete nasadit v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5c2de-161">But it's a good practice to use staging to test everything the way you plan to deploy in production.</span></span>

### <a name="create-appofflinehtm"></a><span data-ttu-id="5c2de-162">Vytvoření aplikace\_offline.htm</span><span class="sxs-lookup"><span data-stu-id="5c2de-162">Create app\_offline.htm</span></span>

1. <span data-ttu-id="5c2de-163">V **Průzkumníku řešení**, klikněte pravým tlačítkem na řešení a klikněte na **přidat**a potom klikněte na **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="5c2de-163">In **Solution Explorer**, right-click the solution and click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="5c2de-164">Vytvoření **stránku HTML** s názvem *aplikace\_offline.htm* (odstranit konečné "l" v *.html* rozšíření, která sada Visual Studio vytvoří ve výchozím nastavení).</span><span class="sxs-lookup"><span data-stu-id="5c2de-164">Create an **HTML Page** named *app\_offline.htm* (delete the final "l" in the *.html* extension that Visual Studio creates by default).</span></span>
3. <span data-ttu-id="5c2de-165">Nahraďte kód šablony následující kód:</span><span class="sxs-lookup"><span data-stu-id="5c2de-165">Replace the template markup with the following markup:</span></span>

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. <span data-ttu-id="5c2de-166">Soubor uložte a zavřete.</span><span class="sxs-lookup"><span data-stu-id="5c2de-166">Save and close the file.</span></span>

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a><span data-ttu-id="5c2de-167">Kopie aplikace\_offline.htm ke kořenové složce webové stránky</span><span class="sxs-lookup"><span data-stu-id="5c2de-167">Copy app\_offline.htm to the root folder of the web site</span></span>

<span data-ttu-id="5c2de-168">Jakýkoli nástroj FTP můžete kopírovat soubory na web.</span><span class="sxs-lookup"><span data-stu-id="5c2de-168">You can use any FTP tool to copy files to the web site.</span></span> <span data-ttu-id="5c2de-169">[FileZilla](http://filezilla-project.org/) je nástroj oblíbených FTP a zobrazí se v snímky obrazovky.</span><span class="sxs-lookup"><span data-stu-id="5c2de-169">[FileZilla](http://filezilla-project.org/) is a popular FTP tool and is shown in the screen shots.</span></span>

<span data-ttu-id="5c2de-170">Pomocí nástroje FTP, budete potřebovat tři věci: adresy URL FTP, uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="5c2de-170">To use an FTP tool, you need three things: the FTP URL, the user name, and the password.</span></span>

<span data-ttu-id="5c2de-171">Adresa URL se zobrazí na stránce řídicího panelu webu v Azure Management Portal a uživatelské jméno a heslo pro FTP najdete v *.publishsettings* soubor, který jste předtím stáhli.</span><span class="sxs-lookup"><span data-stu-id="5c2de-171">The URL is shown on the web site's dashboard page in the Azure Management Portal, and the user name and password for FTP can be found in the *.publishsettings* file that you downloaded earlier.</span></span> <span data-ttu-id="5c2de-172">Následující kroky ukazují, jak získat tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5c2de-172">The following steps show how to get these values.</span></span>

1. <span data-ttu-id="5c2de-173">V portálu pro správu Azure, klikněte na **weby** a pak klikněte na pracovní webu.</span><span class="sxs-lookup"><span data-stu-id="5c2de-173">In the Azure Management Portal, click **Web Sites** tab and then click the staging web site.</span></span>
2. <span data-ttu-id="5c2de-174">Na **řídicí panel** stránky, posuňte se dolů najít název hostitele FTP v **rychlý přehled** části.</span><span class="sxs-lookup"><span data-stu-id="5c2de-174">On the **Dashboard** page, scroll down to find the FTP host name in the **Quick Glance** section.</span></span>

    ![Název hostitele FTP](deploying-a-code-update/_static/image5.png)
3. <span data-ttu-id="5c2de-176">Otevřete pracovní *.publishsettings* souboru v programu Poznámkový blok nebo jiném textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="5c2de-176">Open the staging *.publishsettings* file in Notepad or another text editor.</span></span>
4. <span data-ttu-id="5c2de-177">Najít `publishProfile` element profilu FTP.</span><span class="sxs-lookup"><span data-stu-id="5c2de-177">Find the `publishProfile` element for the FTP profile.</span></span>
5. <span data-ttu-id="5c2de-178">Kopírování `userName` a `userPWD` hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5c2de-178">Copy the `userName` and `userPWD` values.</span></span>

    ![Přihlašovací údaje serveru FTP](deploying-a-code-update/_static/image6.png)
6. <span data-ttu-id="5c2de-180">Otevřete nástroj FTP a přihlaste se k adresy URL FTP.</span><span class="sxs-lookup"><span data-stu-id="5c2de-180">Open your FTP tool and log on to the FTP URL.</span></span>
7. <span data-ttu-id="5c2de-181">Kopírování *aplikace\_offline.htm* ze složky řešení */web/wwwroot* složky na pracovní webu.</span><span class="sxs-lookup"><span data-stu-id="5c2de-181">Copy *app\_offline.htm* from the solution folder to the */site/wwwroot* folder in the staging site.</span></span>

    ![Zkopírujte app_offline](deploying-a-code-update/_static/image7.png)
8. <span data-ttu-id="5c2de-183">Přejděte na adresu URL pracovní lokality.</span><span class="sxs-lookup"><span data-stu-id="5c2de-183">Browse to your staging site's URL.</span></span> <span data-ttu-id="5c2de-184">Uvidíte, že *aplikace\_offline.htm* stránky se nyní zobrazí namísto domovské stránky.</span><span class="sxs-lookup"><span data-stu-id="5c2de-184">You see that the *app\_offline.htm* page is now displayed instead of your home page.</span></span>

    ![App_offline.htm v okně prohlížeče](deploying-a-code-update/_static/image8.png)

<span data-ttu-id="5c2de-186">Nyní jste připraveni k nasazení do přípravy.</span><span class="sxs-lookup"><span data-stu-id="5c2de-186">You are now ready to deploy to staging.</span></span>

## <a name="deploy-the-code-update-to-staging-and-production"></a><span data-ttu-id="5c2de-187">Kód aktualizace nasadit do přípravné nebo produkční prostředí</span><span class="sxs-lookup"><span data-stu-id="5c2de-187">Deploy the code update to staging and production</span></span>

1. <span data-ttu-id="5c2de-188">V **publikování webu jedním kliknutím** nástrojů, vyberte **pracovní** profil publikování a pak klikněte na **Publikovat Web**.</span><span class="sxs-lookup"><span data-stu-id="5c2de-188">In the **Web One Click Publish** toolbar, choose the **Staging** publish profile and then click **Publish Web**.</span></span>

    <span data-ttu-id="5c2de-189">Visual Studio nasadí aktualizovanou aplikaci a otevře prohlížeč na domovskou stránku.</span><span class="sxs-lookup"><span data-stu-id="5c2de-189">Visual Studio deploys the updated application and opens the browser to the site's home page.</span></span> <span data-ttu-id="5c2de-190">*Aplikace\_offline.htm* souboru se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="5c2de-190">The *app\_offline.htm* file is displayed.</span></span> <span data-ttu-id="5c2de-191">Než můžete otestovat a ověřit úspěšné nasazení, je třeba odstranit *aplikace\_offline.htm* souboru.</span><span class="sxs-lookup"><span data-stu-id="5c2de-191">Before you can test to verify successful deployment, you must remove the *app\_offline.htm* file.</span></span>
2. <span data-ttu-id="5c2de-192">Vraťte se na vaše nástroje FTP a odstranit **aplikace\_offline.htm** z pracovní lokality.</span><span class="sxs-lookup"><span data-stu-id="5c2de-192">Return to your FTP tool, and delete **app\_offline.htm** from the staging site.</span></span>
3. <span data-ttu-id="5c2de-193">V prohlížeči otevřít stránku vyučující v pracovní lokality a vyberte lektorem k ověření, že aktualizace byla úspěšně nasazena.</span><span class="sxs-lookup"><span data-stu-id="5c2de-193">In the browser, open the Instructors page in the staging site, and select an instructor to verify that the update was successfully deployed.</span></span>
4. <span data-ttu-id="5c2de-194">Postupujte stejným způsobem pro produkční prostředí, jako jste to udělali pracovní.</span><span class="sxs-lookup"><span data-stu-id="5c2de-194">Follow the same procedure for production as you did for staging.</span></span>

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a><span data-ttu-id="5c2de-195">Kontrola změn a nasazení konkrétní soubory</span><span class="sxs-lookup"><span data-stu-id="5c2de-195">Reviewing Changes and Deploying Specific Files</span></span>

<span data-ttu-id="5c2de-196">Visual Studio 2012 také poskytuje schopnost nasadit jednotlivé soubory.</span><span class="sxs-lookup"><span data-stu-id="5c2de-196">Visual Studio 2012 also gives you the ability to deploy individual files.</span></span> <span data-ttu-id="5c2de-197">Pro vybraný soubor můžete zobrazit rozdíly mezi místní verze a nasazené verzi, nasaďte soubor na cílovém prostředí nebo zkopírujte soubor do místní projekt z cílového prostředí.</span><span class="sxs-lookup"><span data-stu-id="5c2de-197">For a selected file you can view differences between the local version and the deployed version, deploy the file to the destination environment, or copy the file from the destination environment to the local project.</span></span> <span data-ttu-id="5c2de-198">V této části kurzu najdete v části používání těchto funkcí.</span><span class="sxs-lookup"><span data-stu-id="5c2de-198">In this section of the tutorial you see how to use these features.</span></span>

### <a name="make-a-change-to-deploy"></a><span data-ttu-id="5c2de-199">Změňte k nasazení</span><span class="sxs-lookup"><span data-stu-id="5c2de-199">Make a change to deploy</span></span>

1. <span data-ttu-id="5c2de-200">Otevřete *Content/Site.css*a najít bloku `body` značky.</span><span class="sxs-lookup"><span data-stu-id="5c2de-200">Open *Content/Site.css*, and find the block for the `body` tag.</span></span>
2. <span data-ttu-id="5c2de-201">Změňte hodnotu `background-color` z `#fff` k `darkblue`.</span><span class="sxs-lookup"><span data-stu-id="5c2de-201">Change the value for `background-color` from `#fff` to `darkblue`.</span></span>

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a><span data-ttu-id="5c2de-202">Zobrazení změn v okně Náhled publikování</span><span class="sxs-lookup"><span data-stu-id="5c2de-202">View the change in the Publish Preview window</span></span>

<span data-ttu-id="5c2de-203">Při použití **Publikovat Web** Průvodce publikování projektu, uvidíte, jaké změny budou publikována dvojitým kliknutím na soubor v **Preview** okno.</span><span class="sxs-lookup"><span data-stu-id="5c2de-203">When you use the **Publish Web** wizard to publish the project, you can see what changes are going to be published by double-clicking the file in the **Preview** window.</span></span>

1. <span data-ttu-id="5c2de-204">Klikněte pravým tlačítkem na projekt ContosoUniversity a klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="5c2de-204">Right-click the ContosoUniversity project and click **Publish**.</span></span>
2. <span data-ttu-id="5c2de-205">Z **profil** rozevíracího seznamu, vyberte **testovací** profilu publikování.</span><span class="sxs-lookup"><span data-stu-id="5c2de-205">From the **Profile** drop-down list, select the **Test** publish profile.</span></span>
3. <span data-ttu-id="5c2de-206">Klikněte na tlačítko **Preview**a potom klikněte na **spustit Náhled**.</span><span class="sxs-lookup"><span data-stu-id="5c2de-206">Click **Preview**, and then click **Start Preview**.</span></span>
4. <span data-ttu-id="5c2de-207">V **Preview** podokně dvakrát klikněte na **Site.css**.</span><span class="sxs-lookup"><span data-stu-id="5c2de-207">In the **Preview** pane, double-click **Site.css**.</span></span>

    ![Klikněte dvakrát na Site.css](deploying-a-code-update/_static/image9.png)

    <span data-ttu-id="5c2de-209">**Zobrazení náhledu změn** dialogové okno se zobrazí ukázka změny, které se nasadí.</span><span class="sxs-lookup"><span data-stu-id="5c2de-209">The **Preview changes** dialog shows a preview of the changes that will be deployed.</span></span>

    ![Zobrazení náhledu změn na Site.css](deploying-a-code-update/_static/image10.png)

    <span data-ttu-id="5c2de-211">Pokud dvakrát kliknete *Web.config* souboru **zobrazení náhledu změn** dialogové okno zobrazuje účinku buildu transformace konfigurace a publikování profil transformace.</span><span class="sxs-lookup"><span data-stu-id="5c2de-211">If you double-click the *Web.config* file, the **Preview changes** dialog shows the effect of your build configuration transformations and publish profile transformations.</span></span> <span data-ttu-id="5c2de-212">V tomto okamžiku jste to neudělali všechno, co by *Web.config* soubor na serveru, pokud chcete změnit, takže byste měli vidět žádné změny.</span><span class="sxs-lookup"><span data-stu-id="5c2de-212">At this point you have not done anything that would cause the *Web.config* file on the server to change, so you expect to see no changes.</span></span> <span data-ttu-id="5c2de-213">Ale **zobrazení náhledu změn** v okně se nesprávně zobrazí dvě změny.</span><span class="sxs-lookup"><span data-stu-id="5c2de-213">However, the **Preview changes** window incorrectly shows two changes.</span></span> <span data-ttu-id="5c2de-214">Zdá se, se odeberou dva elementy XML.</span><span class="sxs-lookup"><span data-stu-id="5c2de-214">It looks like two XML elements will be removed.</span></span> <span data-ttu-id="5c2de-215">Tyto prvky jsou přidávány proces publikování, když vyberete **spustit migrace Code First při spuštění aplikace** pro třídu kontextu Code First.</span><span class="sxs-lookup"><span data-stu-id="5c2de-215">These elements are added by the publish process when you select **Execute Code First Migrations on application start** for a Code First context class.</span></span> <span data-ttu-id="5c2de-216">Porovnání se provádí před proces publikování přidá tyto prvky, takže to vypadá, budou se odstraněny i když se neodeberou.</span><span class="sxs-lookup"><span data-stu-id="5c2de-216">The comparison is done before the publish process adds those elements, so it looks like they are being removed although they will not be removed.</span></span> <span data-ttu-id="5c2de-217">Tato chyba bude opraven v budoucí verzi.</span><span class="sxs-lookup"><span data-stu-id="5c2de-217">This error will be corrected in a future release.</span></span>
5. <span data-ttu-id="5c2de-218">Klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="5c2de-218">Click **Close**.</span></span>
6. <span data-ttu-id="5c2de-219">Klikněte na tlačítko **publikování**.</span><span class="sxs-lookup"><span data-stu-id="5c2de-219">Click **Publish**.</span></span>
7. <span data-ttu-id="5c2de-220">Když se otevře prohlížeč na domovské stránce webu testovací, stiskněte CTRL + F5 a způsobit pevný aktualizaci, aby se projevily změny šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="5c2de-220">When the browser opens to the home page of the Test site, press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Vliv změny šablon stylů CSS](deploying-a-code-update/_static/image11.png)
8. <span data-ttu-id="5c2de-222">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="5c2de-222">Close the browser.</span></span>

### <a name="publish-specific-files-from-solution-explorer"></a><span data-ttu-id="5c2de-223">Publikování konkrétní soubory v Průzkumníku řešení</span><span class="sxs-lookup"><span data-stu-id="5c2de-223">Publish specific files from Solution Explorer</span></span>

<span data-ttu-id="5c2de-224">Předpokládejme, že nemáte jako modré pozadí a chcete obnovit původní barvy.</span><span class="sxs-lookup"><span data-stu-id="5c2de-224">Suppose you don't like the blue background and want to revert to the original color.</span></span> <span data-ttu-id="5c2de-225">V této části budete obnovit původní nastavení tím, že publikujete konkrétní soubor přímo z **Průzkumníku řešení**.</span><span class="sxs-lookup"><span data-stu-id="5c2de-225">In this section you'll restore the original settings by publishing a specific file directly from **Solution Explorer**.</span></span>

1. <span data-ttu-id="5c2de-226">Otevřete *Content/Site.css* a obnovení `background-color` nastavení `#fff`.</span><span class="sxs-lookup"><span data-stu-id="5c2de-226">Open *Content/Site.css* and restore the `background-color` setting to `#fff`.</span></span>
2. <span data-ttu-id="5c2de-227">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *Content/Site.css* souboru.</span><span class="sxs-lookup"><span data-stu-id="5c2de-227">In **Solution Explorer**, right-click the *Content/Site.css* file.</span></span>

    <span data-ttu-id="5c2de-228">V místní nabídce zobrazuje že tři možnosti publikovat.</span><span class="sxs-lookup"><span data-stu-id="5c2de-228">The context menu shows three publish options.</span></span>

    ![Publikování možnosti v Průzkumníku řešení](deploying-a-code-update/_static/image12.png)
3. <span data-ttu-id="5c2de-230">Klikněte na tlačítko **Preview změny Site.css**.</span><span class="sxs-lookup"><span data-stu-id="5c2de-230">Click **Preview changes to Site.css**.</span></span>

    <span data-ttu-id="5c2de-231">Otevře se okno zobrazíte rozdíly mezi místního souboru a verze se v cílovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="5c2de-231">A window opens to show the differences between the local file and the version of it in the destination environment.</span></span>

    ![Diff-Content/Site.css](deploying-a-code-update/_static/image13.png)
4. <span data-ttu-id="5c2de-233">V **Průzkumníku řešení**, klikněte pravým tlačítkem na **Site.css** znovu a klikněte na tlačítko **publikování Site.css**.</span><span class="sxs-lookup"><span data-stu-id="5c2de-233">In **Solution Explorer**, right-click **Site.css** again and click **Publish Site.css**.</span></span>

    <span data-ttu-id="5c2de-234">**Aktivity publikování webového** ukazuje, že soubor byl publikován.</span><span class="sxs-lookup"><span data-stu-id="5c2de-234">The **Web Publish Activity** window shows that the file has been published.</span></span>

    ![Okno aktivity publikovat web](deploying-a-code-update/_static/image14.png)
5. <span data-ttu-id="5c2de-236">Otevřete prohlížeč na `http://localhost/contosouniversity` adresu URL a potom stiskněte klávesu CTRL + F5 a způsobit, že pevně aktualizovat Chcete-li zobrazit účinku CSS změnit.</span><span class="sxs-lookup"><span data-stu-id="5c2de-236">Open a browser to the `http://localhost/contosouniversity` URL, and then press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Domovská stránka s normální šablon stylů CSS](deploying-a-code-update/_static/image15.png)
6. <span data-ttu-id="5c2de-238">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="5c2de-238">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="5c2de-239">Souhrn</span><span class="sxs-lookup"><span data-stu-id="5c2de-239">Summary</span></span>

<span data-ttu-id="5c2de-240">Nyní jste viděli několik způsobů, jak nasadit aktualizace aplikace, která nezahrnuje změnu databáze a už víte, jak zobrazení náhledu změn k ověření toho, co se aktualizovalo očekávat.</span><span class="sxs-lookup"><span data-stu-id="5c2de-240">You've now seen several ways to deploy an application update that does not involve a database change, and you've seen how to preview the changes to verify that what will be updated is what you expect.</span></span> <span data-ttu-id="5c2de-241">Stránka vyučující má nyní **výukové kurzy** části.</span><span class="sxs-lookup"><span data-stu-id="5c2de-241">The Instructors page now has a **Courses Taught** section.</span></span>

![Stránka vyučující s kurzy výukové](deploying-a-code-update/_static/image16.png)

<span data-ttu-id="5c2de-243">V dalším kurzu se dozvíte, jak nasadit změnu databáze: přidáte datum narození pole do databáze a na stránku vyučující.</span><span class="sxs-lookup"><span data-stu-id="5c2de-243">The next tutorial shows you how to deploy a database change: you'll add a birthdate field to the database and to the Instructors page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5c2de-244">[Předchozí](deploying-to-production.md)
> [další](deploying-a-database-update.md)</span><span class="sxs-lookup"><span data-stu-id="5c2de-244">[Previous](deploying-to-production.md)
[Next](deploying-a-database-update.md)</span></span>
