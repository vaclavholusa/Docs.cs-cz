---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení aktualizace kódu | Dokumentace Microsoftu'
author: tdykstra
description: V této sérii kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, podle usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: 3649f0250e830b76c42d832e51038c2c52ed4561
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368211"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a><span data-ttu-id="f584a-103">Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení aktualizace kódu</span><span class="sxs-lookup"><span data-stu-id="f584a-103">ASP.NET Web Deployment using Visual Studio: Deploying a Code Update</span></span>
====================
<span data-ttu-id="f584a-104">podle [Petr Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="f584a-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="f584a-105">Stáhnout počáteční projekt</span><span class="sxs-lookup"><span data-stu-id="f584a-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="f584a-106">V této sérii kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, s použitím sady Visual Studio 2012 nebo Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="f584a-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="f584a-107">Informace o této sérii, naleznete v tématu [z prvního kurzu této série](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f584a-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="f584a-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="f584a-108">Overview</span></span>

<span data-ttu-id="f584a-109">Po počátečním nasazení pokračuje v práci údržbu a vývoj webu, a před dlouho budete chtít nasazení aktualizace.</span><span class="sxs-lookup"><span data-stu-id="f584a-109">After the initial deployment, your work of maintaining and developing your web site continues, and before long you will want to deploy an update.</span></span> <span data-ttu-id="f584a-110">Tento kurz vás provede procesem nasazení aktualizace kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="f584a-110">This tutorial takes you through the process of deploying an update to your application code.</span></span> <span data-ttu-id="f584a-111">Aktualizaci, implementaci a nasazení v tomto kurzu nezahrnuje Změna databáze; uvidíte, co se liší o nasazení změn databází v dalším kurzu.</span><span class="sxs-lookup"><span data-stu-id="f584a-111">The update that you implement and deploy in this tutorial does not involve a database change; you'll see what's different about deploying a database change in the next tutorial.</span></span>

<span data-ttu-id="f584a-112">Připomenutí: Pokud se zobrazí chybová zpráva nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [stránka o řešení problémů](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="f584a-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="make-a-code-change"></a><span data-ttu-id="f584a-113">Změně kódu</span><span class="sxs-lookup"><span data-stu-id="f584a-113">Make a code change</span></span>

<span data-ttu-id="f584a-114">Jako jednoduchý příklad příkazu update pro vaši aplikaci, přidáte k **Instruktoři** stránce Seznam kurzy vedené vybrané instruktorem.</span><span class="sxs-lookup"><span data-stu-id="f584a-114">As a simple example of an update to your application, you'll add to the **Instructors** page a list of courses taught by the selected instructor.</span></span>

<span data-ttu-id="f584a-115">Při spuštění **Instruktoři** stránky, můžete si všimnout, že jsou **vyberte** odkazy v mřížce, ale jejich nic nedělají než zkontrolujte šedá zapnout pozadí řádku.</span><span class="sxs-lookup"><span data-stu-id="f584a-115">If you run the **Instructors** page, you'll notice that there are **Select** links in the grid, but they don't do anything other than make the row background turn gray.</span></span>

![Instruktoři stránky s výběrem](deploying-a-code-update/_static/image1.png)

<span data-ttu-id="f584a-117">Teď přidejte kód, který spouští, když **vyberte** dojde ke kliknutí na odkaz a zobrazí seznam kurzy vedené vybrané instruktorem.</span><span class="sxs-lookup"><span data-stu-id="f584a-117">Now you'll add code that runs when the **Select** link is clicked and displays a list of courses taught by the selected instructor .</span></span>

1. <span data-ttu-id="f584a-118">V *Instructors.aspx*, přidejte následující kód bezprostředně po **ErrorMessageLabel** `Label` ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="f584a-118">In *Instructors.aspx*, add the following markup immediately after the **ErrorMessageLabel** `Label` control:</span></span>

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. <span data-ttu-id="f584a-119">Spustit na stránku a vybrat instruktorem.</span><span class="sxs-lookup"><span data-stu-id="f584a-119">Run the page and select an instructor.</span></span> <span data-ttu-id="f584a-120">Zobrazí seznam kurzy vedené této instruktorem.</span><span class="sxs-lookup"><span data-stu-id="f584a-120">You see a list of courses taught by that instructor.</span></span>

    ![Stránka Instruktoři s výukové kurzy](deploying-a-code-update/_static/image2.png)
3. <span data-ttu-id="f584a-122">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="f584a-122">Close the browser.</span></span>

## <a name="deploy-the-code-update-to-the-test-environment"></a><span data-ttu-id="f584a-123">Nasazení aktualizace kódu do testovacího prostředí</span><span class="sxs-lookup"><span data-stu-id="f584a-123">Deploy the code update to the test environment</span></span>

<span data-ttu-id="f584a-124">Než použijete profil publikování pro nasazení do testu, přípravném nebo produkčním prostředí budete muset změnit možnosti publikování databáze.</span><span class="sxs-lookup"><span data-stu-id="f584a-124">Before you can use your publish profiles to deploy to test, staging, and production, you need to change database publishing options.</span></span> <span data-ttu-id="f584a-125">Už nemusíte spouštět skripty nasazení udělení a dat pro databázi členství.</span><span class="sxs-lookup"><span data-stu-id="f584a-125">You no longer need to run the grant and data deployment scripts for the membership database.</span></span>

1. <span data-ttu-id="f584a-126">Otevřít **Publikovat Web** Průvodce pravým tlačítkem myši projekt ContosoUniversity a kliknutím na **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="f584a-126">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="f584a-127">Klikněte na tlačítko **testovací** profil v **profilu** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="f584a-127">Click the **Test** profile in the **Profile** drop-down list.</span></span>
3. <span data-ttu-id="f584a-128">Klikněte na tlačítko **nastavení** kartu.</span><span class="sxs-lookup"><span data-stu-id="f584a-128">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="f584a-129">V části **objekt DefaultConnection** v **databází** části, zrušte **aktualizace databáze** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="f584a-129">Under **DefaultConnection** in the **Databases** section, clear the **Update database** check box.</span></span>
5. <span data-ttu-id="f584a-130">Klikněte na tlačítko **profilu** kartu a potom klikněte na tlačítko **pracovní** profil v **profilu** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="f584a-130">Click the **Profile** tab, and then click the **Staging** profile in the **Profile** drop-down list.</span></span>
6. <span data-ttu-id="f584a-131">Pokud se výzva, pokud chcete uložit změny provedené **testovací** profilu, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="f584a-131">When you are asked if you want to save the changes made to the **Test** profile, click **Yes**.</span></span>
7. <span data-ttu-id="f584a-132">Stejnou změnu proveďte v pracovní profil.</span><span class="sxs-lookup"><span data-stu-id="f584a-132">Make the same change in the Staging profile.</span></span>
8. <span data-ttu-id="f584a-133">Opakujte postup provést stejnou změnu v produkčním prostředí profilu.</span><span class="sxs-lookup"><span data-stu-id="f584a-133">Repeat the process to make the same change in the Production profile.</span></span>
9. <span data-ttu-id="f584a-134">Zavřít **publikování webu** průvodce.</span><span class="sxs-lookup"><span data-stu-id="f584a-134">Close the **Publish Web** wizard.</span></span>

<span data-ttu-id="f584a-135">Nasazení do testovacího prostředí je teď znovu publikovat jednoduché spuštění jedním kliknutím.</span><span class="sxs-lookup"><span data-stu-id="f584a-135">Deploying to the test environment is now a simple matter of running one-click publish again.</span></span> <span data-ttu-id="f584a-136">Chcete-li tento proces urychlit, můžete použít **publikování webu jedním kliknutím** nástrojů.</span><span class="sxs-lookup"><span data-stu-id="f584a-136">To make this process quicker, you can use the **Web One Click Publish** toolbar.</span></span>

1. <span data-ttu-id="f584a-137">V **zobrazení** nabídce zvolte **panely nástrojů** a pak vyberte **publikování webu jedním kliknutím**.</span><span class="sxs-lookup"><span data-stu-id="f584a-137">In the **View** menu, choose **Toolbars** and then select **Web One Click Publish**.</span></span>

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. <span data-ttu-id="f584a-139">V **Průzkumníka řešení**, vyberte projekt ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="f584a-139">In **Solution Explorer**, select the ContosoUniversity project.</span></span>
3. <span data-ttu-id="f584a-140">**publikování webu jedním kliknutím** nástrojů, zvolte **testovací** profil publikování a pak klikněte na tlačítko **Publikovat Web** (na ikonu šipky směřující vlevo a vpravo).</span><span class="sxs-lookup"><span data-stu-id="f584a-140">the **Web One Click Publish** toolbar, choose the **Test** publish profile and then click **Publish Web** (the icon with arrows pointing left and right).</span></span>

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. <span data-ttu-id="f584a-142">Visual Studio nasadí aktualizované aplikace a prohlížeč se automaticky otevře na domovskou stránku.</span><span class="sxs-lookup"><span data-stu-id="f584a-142">Visual Studio deploys the updated application, and the browser automatically opens to the home page.</span></span>
5. <span data-ttu-id="f584a-143">Spustit školitelů stránky a vyberte k ověření, že se aktualizace úspěšně nasadilo instruktorem.</span><span class="sxs-lookup"><span data-stu-id="f584a-143">Run the Instructors page and select an instructor to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="f584a-144">Běžným způsobem provést také regresního testování (to znamená, že test zbytku lokality, abyste měli jistotu, že tato nová změna neměli přerušit všechny existující funkce).</span><span class="sxs-lookup"><span data-stu-id="f584a-144">You would normally also do regression testing (that is, test the rest of the site to make sure that the new change didn't break any existing functionality).</span></span> <span data-ttu-id="f584a-145">Ale pro účely tohoto kurzu budete tento krok přeskočit a přejít k nasazení aktualizací do pracovního a produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="f584a-145">But for this tutorial you'll skip that step and proceed to deploy the update to staging and production.</span></span>

<span data-ttu-id="f584a-146">Při opětovném nasazování, Web Deploy automaticky určuje, které soubory se změnily a kopíruje jen změněné soubory na server.</span><span class="sxs-lookup"><span data-stu-id="f584a-146">When you redeploy, Web Deploy automatically determines which files have changed and only copies changed files to the server.</span></span> <span data-ttu-id="f584a-147">Ve výchozím nastavení nasazení webu používá data poslední změny na soubory k určení, které se nezměnila.</span><span class="sxs-lookup"><span data-stu-id="f584a-147">By default, Web Deploy uses last-changed dates on files to determine which ones have changed.</span></span> <span data-ttu-id="f584a-148">Některé systémy správy zdrojového kódu změňte soubor data i při Neměňte obsah souboru.</span><span class="sxs-lookup"><span data-stu-id="f584a-148">Some source control systems change file dates even when you don't change the file contents.</span></span> <span data-ttu-id="f584a-149">V takovém případě můžete chtít nakonfigurovat nasazení webu použití souboru kontrolní součty k určení, které soubory se změnily.</span><span class="sxs-lookup"><span data-stu-id="f584a-149">In that case, you might want to configure Web Deploy to use file checksums to determine which files have changed.</span></span> <span data-ttu-id="f584a-150">Další informace najdete v tématu [Proč všechny soubory získat znovu nasadil Ačkoli jsem je nezměnili?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) v nejčastější dotazy k nasazení technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f584a-150">For more information, see [Why do all of my files get redeployed although I didn't change them?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) in the ASP.NET Deployment FAQ.</span></span>

## <a name="take-the-application-offline-during-deployment"></a><span data-ttu-id="f584a-151">Nastavit aplikaci offline během nasazení</span><span class="sxs-lookup"><span data-stu-id="f584a-151">Take the application offline during deployment</span></span>

<span data-ttu-id="f584a-152">Změna, kterou nasazujete nyní je jednoduchou změnu na jednu stránku.</span><span class="sxs-lookup"><span data-stu-id="f584a-152">The change you're deploying now is a simple change to a single page.</span></span> <span data-ttu-id="f584a-153">Ale někdy nasadit větší změny, nebo nasadit změny kódu a databáze a lokality se může chovat chybně Pokud uživatel požádá o stránku předtím, než se nasazení dokončí.</span><span class="sxs-lookup"><span data-stu-id="f584a-153">But sometimes you deploy larger changes, or you deploy both code and database changes, and the site might behave incorrectly if a user requests a page before deployment is finished.</span></span> <span data-ttu-id="f584a-154">Chcete-li uživatelům zabránit v přístupu na web, zatímco probíhá nasazení, můžete použít *aplikace\_offline.htm* souboru.</span><span class="sxs-lookup"><span data-stu-id="f584a-154">To prevent users from accessing the site while deployment is in progress, you can use an *app\_offline.htm* file.</span></span> <span data-ttu-id="f584a-155">Zadaný soubor s názvem *aplikace\_offline.htm* v kořenové složce vaší aplikace, služba IIS automaticky zobrazí tento soubor namísto spuštění vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="f584a-155">When you put a file named *app\_offline.htm* in the root folder of your application, IIS automatically displays that file instead of running your application.</span></span> <span data-ttu-id="f584a-156">Tak, aby se zabránilo přístupu během nasazování, kam si ukládáte *aplikace\_offline.htm* v kořenové složce spustit proces nasazení a pak odeberte *aplikace\_offline.htm* po úspěšné nasazení.</span><span class="sxs-lookup"><span data-stu-id="f584a-156">So to prevent access during deployment, you put *app\_offline.htm* in the root folder, run the deployment process, and then remove *app\_offline.htm* after successful deployment.</span></span>

<span data-ttu-id="f584a-157">Můžete nakonfigurovat nasazení webu automatické přepnutí výchozí *aplikace\_offline.htm* souboru na serveru při spuštění nasazení a jeho odebrání po dokončení.</span><span class="sxs-lookup"><span data-stu-id="f584a-157">You can configure Web Deploy to automatically put a default *app\_offline.htm* file on the server when it starts deploying and remove it when it finishes.</span></span> <span data-ttu-id="f584a-158">Provedete to, že všechny budete muset provést je, přidejte následující element XML do souboru profilu (.pubxml) publikování:</span><span class="sxs-lookup"><span data-stu-id="f584a-158">To do that all you have to do is add the following XML element to your publish profile (.pubxml) file:</span></span>

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

<span data-ttu-id="f584a-159">Pro účely tohoto kurzu uvidíte, jak vytvořit a používat vlastní *aplikace\_offline.htm* souboru.</span><span class="sxs-lookup"><span data-stu-id="f584a-159">For this tutorial you'll see how to create and use a custom *app\_offline.htm* file.</span></span>

<span data-ttu-id="f584a-160">Pomocí *aplikace\_offline.htm* na pracovním webu není povinné, protože nemáte uživatele, kteří používají na pracovním webu.</span><span class="sxs-lookup"><span data-stu-id="f584a-160">Using *app\_offline.htm* in the staging site isn't required, because you don't have users accessing the staging site.</span></span> <span data-ttu-id="f584a-161">Ale je vhodné testovat vše, co tak, jak máte v plánu nasazení v produkčním prostředí pomocí přípravy.</span><span class="sxs-lookup"><span data-stu-id="f584a-161">But it's a good practice to use staging to test everything the way you plan to deploy in production.</span></span>

### <a name="create-appofflinehtm"></a><span data-ttu-id="f584a-162">Vytvoření aplikace\_offline.htm</span><span class="sxs-lookup"><span data-stu-id="f584a-162">Create app\_offline.htm</span></span>

1. <span data-ttu-id="f584a-163">V **Průzkumníka řešení**, klikněte pravým tlačítkem na řešení a klikněte na tlačítko **přidat**a potom klikněte na tlačítko **nová položka**.</span><span class="sxs-lookup"><span data-stu-id="f584a-163">In **Solution Explorer**, right-click the solution and click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="f584a-164">Vytvoření **stránku HTML** s názvem *aplikace\_offline.htm* (odstranit poslední "l" v *.html* rozšíření, které sada Visual Studio vytvoří ve výchozím nastavení).</span><span class="sxs-lookup"><span data-stu-id="f584a-164">Create an **HTML Page** named *app\_offline.htm* (delete the final "l" in the *.html* extension that Visual Studio creates by default).</span></span>
3. <span data-ttu-id="f584a-165">Nahraďte kód šablony následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="f584a-165">Replace the template markup with the following markup:</span></span>

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. <span data-ttu-id="f584a-166">Soubor uložte a zavřete.</span><span class="sxs-lookup"><span data-stu-id="f584a-166">Save and close the file.</span></span>

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a><span data-ttu-id="f584a-167">Kopírování aplikací\_offline.htm do kořenové složky webu</span><span class="sxs-lookup"><span data-stu-id="f584a-167">Copy app\_offline.htm to the root folder of the web site</span></span>

<span data-ttu-id="f584a-168">Můžete použít jakýkoli nástroj FTP pro kopírování souborů do webové stránky.</span><span class="sxs-lookup"><span data-stu-id="f584a-168">You can use any FTP tool to copy files to the web site.</span></span> <span data-ttu-id="f584a-169">[Filezilly](http://filezilla-project.org/) je oblíbený nástroj FTP a zobrazí se v snímky obrazovky.</span><span class="sxs-lookup"><span data-stu-id="f584a-169">[FileZilla](http://filezilla-project.org/) is a popular FTP tool and is shown in the screen shots.</span></span>

<span data-ttu-id="f584a-170">Pokud chcete použít nástroj FTP, potřebujete tři věci: adresa URL protokolu FTP, uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="f584a-170">To use an FTP tool, you need three things: the FTP URL, the user name, and the password.</span></span>

<span data-ttu-id="f584a-171">Adresa URL se zobrazí na stránce řídicího panelu na webu portálu pro správu Azure a uživatelské jméno a heslo pro FTP najdete v *.publishsettings* soubor, který jste předtím stáhli.</span><span class="sxs-lookup"><span data-stu-id="f584a-171">The URL is shown on the web site's dashboard page in the Azure Management Portal, and the user name and password for FTP can be found in the *.publishsettings* file that you downloaded earlier.</span></span> <span data-ttu-id="f584a-172">Následující kroky ukazují, jak získat tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f584a-172">The following steps show how to get these values.</span></span>

1. <span data-ttu-id="f584a-173">Na portálu Azure Management Portal, klikněte na tlačítko **weby** kartu a potom klikněte na pracovním webu.</span><span class="sxs-lookup"><span data-stu-id="f584a-173">In the Azure Management Portal, click **Web Sites** tab and then click the staging web site.</span></span>
2. <span data-ttu-id="f584a-174">Na **řídicí panel** stránky, posuňte se dolů najít název hostitele FTP v **rychlý přehled** oddílu.</span><span class="sxs-lookup"><span data-stu-id="f584a-174">On the **Dashboard** page, scroll down to find the FTP host name in the **Quick Glance** section.</span></span>

    ![Název hostitele FTP](deploying-a-code-update/_static/image5.png)
3. <span data-ttu-id="f584a-176">Otevřete v přípravném *.publishsettings* soubor v poznámkovém bloku nebo jiného textového editoru.</span><span class="sxs-lookup"><span data-stu-id="f584a-176">Open the staging *.publishsettings* file in Notepad or another text editor.</span></span>
4. <span data-ttu-id="f584a-177">Najít `publishProfile` – element pro profil FTP.</span><span class="sxs-lookup"><span data-stu-id="f584a-177">Find the `publishProfile` element for the FTP profile.</span></span>
5. <span data-ttu-id="f584a-178">Kopírovat `userName` a `userPWD` hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f584a-178">Copy the `userName` and `userPWD` values.</span></span>

    ![Přihlašovací údaje serveru FTP](deploying-a-code-update/_static/image6.png)
6. <span data-ttu-id="f584a-180">Otevřete nástroj FTP a přihlaste se k adrese URL protokolu FTP.</span><span class="sxs-lookup"><span data-stu-id="f584a-180">Open your FTP tool and log on to the FTP URL.</span></span>
7. <span data-ttu-id="f584a-181">Kopírování *aplikace\_offline.htm* složce řešení a */site/wwwroot* složky na pracovním webu.</span><span class="sxs-lookup"><span data-stu-id="f584a-181">Copy *app\_offline.htm* from the solution folder to the */site/wwwroot* folder in the staging site.</span></span>

    ![Zkopírujte app_offline](deploying-a-code-update/_static/image7.png)
8. <span data-ttu-id="f584a-183">Přejděte na adresu URL přípravného lokalitě.</span><span class="sxs-lookup"><span data-stu-id="f584a-183">Browse to your staging site's URL.</span></span> <span data-ttu-id="f584a-184">Uvidíte, že *aplikace\_offline.htm* stránky se nyní zobrazí místo domovské stránky.</span><span class="sxs-lookup"><span data-stu-id="f584a-184">You see that the *app\_offline.htm* page is now displayed instead of your home page.</span></span>

    ![App_offline.htm v okně prohlížeče](deploying-a-code-update/_static/image8.png)

<span data-ttu-id="f584a-186">Nyní jste připraveni k nasazení do přípravného prostředí.</span><span class="sxs-lookup"><span data-stu-id="f584a-186">You are now ready to deploy to staging.</span></span>

## <a name="deploy-the-code-update-to-staging-and-production"></a><span data-ttu-id="f584a-187">Nasazení aktualizace kódu do pracovního a produkčního prostředí</span><span class="sxs-lookup"><span data-stu-id="f584a-187">Deploy the code update to staging and production</span></span>

1. <span data-ttu-id="f584a-188">V **publikování webu jedním kliknutím** nástrojů, zvolte **pracovní** profil publikování a pak klikněte na tlačítko **Publikovat Web**.</span><span class="sxs-lookup"><span data-stu-id="f584a-188">In the **Web One Click Publish** toolbar, choose the **Staging** publish profile and then click **Publish Web**.</span></span>

    <span data-ttu-id="f584a-189">Visual Studio nasadí aktualizované aplikace a otevře prohlížeč na domovské stránce webu.</span><span class="sxs-lookup"><span data-stu-id="f584a-189">Visual Studio deploys the updated application and opens the browser to the site's home page.</span></span> <span data-ttu-id="f584a-190">*Aplikace\_offline.htm* soubor se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="f584a-190">The *app\_offline.htm* file is displayed.</span></span> <span data-ttu-id="f584a-191">Předtím, než můžete otestovat a ověřit úspěšné nasazení, je nutné odebrat *aplikace\_offline.htm* souboru.</span><span class="sxs-lookup"><span data-stu-id="f584a-191">Before you can test to verify successful deployment, you must remove the *app\_offline.htm* file.</span></span>
2. <span data-ttu-id="f584a-192">Vraťte se do vašeho nástroje pro FTP a odstranit **aplikace\_offline.htm** z přípravného lokalitě.</span><span class="sxs-lookup"><span data-stu-id="f584a-192">Return to your FTP tool, and delete **app\_offline.htm** from the staging site.</span></span>
3. <span data-ttu-id="f584a-193">V prohlížeči otevřete stránku Instruktoři na pracovním webu a vyberte instruktorem k ověření, že se aktualizace úspěšně nasadilo.</span><span class="sxs-lookup"><span data-stu-id="f584a-193">In the browser, open the Instructors page in the staging site, and select an instructor to verify that the update was successfully deployed.</span></span>
4. <span data-ttu-id="f584a-194">Postupujte stejným způsobem pro produkční prostředí, jako jste to udělali přípravy.</span><span class="sxs-lookup"><span data-stu-id="f584a-194">Follow the same procedure for production as you did for staging.</span></span>

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a><span data-ttu-id="f584a-195">Zkontrolujte změny a nasaďte konkrétní soubory</span><span class="sxs-lookup"><span data-stu-id="f584a-195">Reviewing Changes and Deploying Specific Files</span></span>

<span data-ttu-id="f584a-196">Visual Studio 2012 také poskytuje schopnost nasadit jednotlivé soubory.</span><span class="sxs-lookup"><span data-stu-id="f584a-196">Visual Studio 2012 also gives you the ability to deploy individual files.</span></span> <span data-ttu-id="f584a-197">Pro vybraný soubor můžete zobrazit rozdíly mezi místní verzi a verzi nasazené, nasaďte soubor do cílového prostředí nebo zkopírujte soubor do místní projekt v cílovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="f584a-197">For a selected file you can view differences between the local version and the deployed version, deploy the file to the destination environment, or copy the file from the destination environment to the local project.</span></span> <span data-ttu-id="f584a-198">V této části kurzu naleznete v tématu Jak používat tyto funkce.</span><span class="sxs-lookup"><span data-stu-id="f584a-198">In this section of the tutorial you see how to use these features.</span></span>

### <a name="make-a-change-to-deploy"></a><span data-ttu-id="f584a-199">Proveďte změnu nasazení</span><span class="sxs-lookup"><span data-stu-id="f584a-199">Make a change to deploy</span></span>

1. <span data-ttu-id="f584a-200">Otevřít *Content/Site.css*a najít bloku `body` značky.</span><span class="sxs-lookup"><span data-stu-id="f584a-200">Open *Content/Site.css*, and find the block for the `body` tag.</span></span>
2. <span data-ttu-id="f584a-201">Změňte hodnotu `background-color` z `#fff` k `darkblue`.</span><span class="sxs-lookup"><span data-stu-id="f584a-201">Change the value for `background-color` from `#fff` to `darkblue`.</span></span>

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a><span data-ttu-id="f584a-202">Zobrazení změn v okně Náhled publikování</span><span class="sxs-lookup"><span data-stu-id="f584a-202">View the change in the Publish Preview window</span></span>

<span data-ttu-id="f584a-203">Při použití **Publikovat Web** Průvodce Publikujte projekt, naleznete v tématu co se změny chystáte publikovat dvojitým kliknutím na soubor v **ve verzi Preview** okna.</span><span class="sxs-lookup"><span data-stu-id="f584a-203">When you use the **Publish Web** wizard to publish the project, you can see what changes are going to be published by double-clicking the file in the **Preview** window.</span></span>

1. <span data-ttu-id="f584a-204">Klikněte pravým tlačítkem na projekt ContosoUniversity a klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="f584a-204">Right-click the ContosoUniversity project and click **Publish**.</span></span>
2. <span data-ttu-id="f584a-205">Z **profilu** rozevíracího seznamu, vyberte **Test** profil publikování.</span><span class="sxs-lookup"><span data-stu-id="f584a-205">From the **Profile** drop-down list, select the **Test** publish profile.</span></span>
3. <span data-ttu-id="f584a-206">Klikněte na tlačítko **ve verzi Preview**a potom klikněte na tlačítko **spustit Náhled**.</span><span class="sxs-lookup"><span data-stu-id="f584a-206">Click **Preview**, and then click **Start Preview**.</span></span>
4. <span data-ttu-id="f584a-207">V **ve verzi Preview** podokně dvakrát klikněte na panel **Site.css**.</span><span class="sxs-lookup"><span data-stu-id="f584a-207">In the **Preview** pane, double-click **Site.css**.</span></span>

    ![Dvakrát klikněte na panel Site.css](deploying-a-code-update/_static/image9.png)

    <span data-ttu-id="f584a-209">**Náhled změn** dialogové okno zobrazí náhled změn, které se nasadí.</span><span class="sxs-lookup"><span data-stu-id="f584a-209">The **Preview changes** dialog shows a preview of the changes that will be deployed.</span></span>

    ![Změny ve verzi Preview Site.css](deploying-a-code-update/_static/image10.png)

    <span data-ttu-id="f584a-211">Pokud dvakrát kliknete *Web.config* soubor, **náhled změn** dialogové okno demonstruje účinek sestavení transformace konfigurace a publikování profilu transformace.</span><span class="sxs-lookup"><span data-stu-id="f584a-211">If you double-click the *Web.config* file, the **Preview changes** dialog shows the effect of your build configuration transformations and publish profile transformations.</span></span> <span data-ttu-id="f584a-212">V tomto okamžiku jste neudělali cokoli, co způsobilo *Web.config* soubor na serveru, chcete-li změnit, takže byste měli vidět žádné změny.</span><span class="sxs-lookup"><span data-stu-id="f584a-212">At this point you have not done anything that would cause the *Web.config* file on the server to change, so you expect to see no changes.</span></span> <span data-ttu-id="f584a-213">Ale **náhled změn** okno nesprávně ukazuje dvě změny.</span><span class="sxs-lookup"><span data-stu-id="f584a-213">However, the **Preview changes** window incorrectly shows two changes.</span></span> <span data-ttu-id="f584a-214">Zdá se, odeberou se dva prvky XML.</span><span class="sxs-lookup"><span data-stu-id="f584a-214">It looks like two XML elements will be removed.</span></span> <span data-ttu-id="f584a-215">Tyto prvky jsou přidány pomocí procesu publikování vyberete **spustit migrace Code First při spuštění aplikace** Code First třídy kontextu.</span><span class="sxs-lookup"><span data-stu-id="f584a-215">These elements are added by the publish process when you select **Execute Code First Migrations on application start** for a Code First context class.</span></span> <span data-ttu-id="f584a-216">Předtím, než se proces publikování přidá tyto prvky, takže to vypadá, že budou odebrány, i když se neodeberou, provádí se porovnání.</span><span class="sxs-lookup"><span data-stu-id="f584a-216">The comparison is done before the publish process adds those elements, so it looks like they are being removed although they will not be removed.</span></span> <span data-ttu-id="f584a-217">Tato chyba bude opraven v budoucí verzi.</span><span class="sxs-lookup"><span data-stu-id="f584a-217">This error will be corrected in a future release.</span></span>
5. <span data-ttu-id="f584a-218">Klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="f584a-218">Click **Close**.</span></span>
6. <span data-ttu-id="f584a-219">Klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="f584a-219">Click **Publish**.</span></span>
7. <span data-ttu-id="f584a-220">Když prohlížeč otevře domovskou stránku Test webu, stiskněte CTRL + F5 způsobit pevné aktualizace Chcete-li zobrazit účinky změn šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="f584a-220">When the browser opens to the home page of the Test site, press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Účinky změn šablon stylů CSS](deploying-a-code-update/_static/image11.png)
8. <span data-ttu-id="f584a-222">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="f584a-222">Close the browser.</span></span>

### <a name="publish-specific-files-from-solution-explorer"></a><span data-ttu-id="f584a-223">Publikovat konkrétní soubory z Průzkumníka řešení</span><span class="sxs-lookup"><span data-stu-id="f584a-223">Publish specific files from Solution Explorer</span></span>

<span data-ttu-id="f584a-224">Předpokládejme, že nemáte jako modré pozadí a chcete se vrátit na původní barvu.</span><span class="sxs-lookup"><span data-stu-id="f584a-224">Suppose you don't like the blue background and want to revert to the original color.</span></span> <span data-ttu-id="f584a-225">V této části budete obnovit původní nastavení a publikujte přímo z konkrétního souboru **Průzkumníka řešení**.</span><span class="sxs-lookup"><span data-stu-id="f584a-225">In this section you'll restore the original settings by publishing a specific file directly from **Solution Explorer**.</span></span>

1. <span data-ttu-id="f584a-226">Otevřít *Content/Site.css* a obnovení `background-color` nastavení `#fff`.</span><span class="sxs-lookup"><span data-stu-id="f584a-226">Open *Content/Site.css* and restore the `background-color` setting to `#fff`.</span></span>
2. <span data-ttu-id="f584a-227">V **Průzkumníka řešení**, klikněte pravým tlačítkem myši *Content/Site.css* souboru.</span><span class="sxs-lookup"><span data-stu-id="f584a-227">In **Solution Explorer**, right-click the *Content/Site.css* file.</span></span>

    <span data-ttu-id="f584a-228">V místní nabídce ukazuje že tři možnosti publikování.</span><span class="sxs-lookup"><span data-stu-id="f584a-228">The context menu shows three publish options.</span></span>

    ![Publikování možnosti z Průzkumníka řešení](deploying-a-code-update/_static/image12.png)
3. <span data-ttu-id="f584a-230">Klikněte na tlačítko **ve verzi Preview se změní na Site.css**.</span><span class="sxs-lookup"><span data-stu-id="f584a-230">Click **Preview changes to Site.css**.</span></span>

    <span data-ttu-id="f584a-231">Otevře se okno zobrazit rozdíly mezi místního souboru a verze se v cílovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="f584a-231">A window opens to show the differences between the local file and the version of it in the destination environment.</span></span>

    ![Diff-Content/Site.css](deploying-a-code-update/_static/image13.png)
4. <span data-ttu-id="f584a-233">V **Průzkumníka řešení**, klikněte pravým tlačítkem na **Site.css** znovu a klikněte na tlačítko **publikovat Site.css**.</span><span class="sxs-lookup"><span data-stu-id="f584a-233">In **Solution Explorer**, right-click **Site.css** again and click **Publish Site.css**.</span></span>

    <span data-ttu-id="f584a-234">**Aktivita publikování webu** okno zobrazuje, že soubor byl publikován.</span><span class="sxs-lookup"><span data-stu-id="f584a-234">The **Web Publish Activity** window shows that the file has been published.</span></span>

    ![Okno aktivity publikovat web](deploying-a-code-update/_static/image14.png)
5. <span data-ttu-id="f584a-236">Otevřete prohlížeč `http://localhost/contosouniversity` adresy URL a poté stiskněte klávesu CTRL + F5 a způsobit, že pevně aktualizaci, aby se projevily šablony CSS změnit.</span><span class="sxs-lookup"><span data-stu-id="f584a-236">Open a browser to the `http://localhost/contosouniversity` URL, and then press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Domovská stránka s normální šablony stylů CSS](deploying-a-code-update/_static/image15.png)
6. <span data-ttu-id="f584a-238">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="f584a-238">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="f584a-239">Souhrn</span><span class="sxs-lookup"><span data-stu-id="f584a-239">Summary</span></span>

<span data-ttu-id="f584a-240">Nyní jste viděli několik způsobů, jak nasadit aktualizace aplikace, který nezahrnuje Změna databáze a už víte, jak zobrazit náhled změn můžete ověřit, že toho, co se aktualizovalo čekáte.</span><span class="sxs-lookup"><span data-stu-id="f584a-240">You've now seen several ways to deploy an application update that does not involve a database change, and you've seen how to preview the changes to verify that what will be updated is what you expect.</span></span> <span data-ttu-id="f584a-241">Stránka Instruktoři má nyní **výukové kurzy** oddílu.</span><span class="sxs-lookup"><span data-stu-id="f584a-241">The Instructors page now has a **Courses Taught** section.</span></span>

![Stránka Instruktoři s výukové kurzy](deploying-a-code-update/_static/image16.png)

<span data-ttu-id="f584a-243">V dalším kurzu se dozvíte, jak nasadit databázi změnu: do databáze a na stránce Instruktoři přidáte pole Datum narození.</span><span class="sxs-lookup"><span data-stu-id="f584a-243">The next tutorial shows you how to deploy a database change: you'll add a birthdate field to the database and to the Instructors page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f584a-244">[Předchozí](deploying-to-production.md)
> [další](deploying-a-database-update.md)</span><span class="sxs-lookup"><span data-stu-id="f584a-244">[Previous](deploying-to-production.md)
[Next](deploying-a-database-update.md)</span></span>
