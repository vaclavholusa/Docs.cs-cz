---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: "Publikování MVC databáze první lokalitu v Azure | Microsoft Docs"
author: tfitzmac
description: "Pomocí generování uživatelského rozhraní ASP.NET, MVC a Entity Framework, můžete vytvořit webovou aplikaci, která poskytuje rozhraní k existující databázi. Tento kurz seri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/22/2014
ms.topic: article
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: eadc0f2b08df29f80fe53d03cf88cd3cdcecfb12
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="publish-mvc-database-first-site-to-azure"></a><span data-ttu-id="b482d-104">Publikování MVC databáze první lokalitu v Azure</span><span class="sxs-lookup"><span data-stu-id="b482d-104">Publish MVC Database First site to Azure</span></span>
====================
<span data-ttu-id="b482d-105">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b482d-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b482d-106">Pomocí generování uživatelského rozhraní ASP.NET, MVC a Entity Framework, můžete vytvořit webovou aplikaci, která poskytuje rozhraní k existující databázi.</span><span class="sxs-lookup"><span data-stu-id="b482d-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="b482d-107">Tato řada kurzu se dozvíte, jak automaticky vygenerovat kód, který umožňuje uživatelům zobrazit, upravit, vytvořte a odstraňovat data, která se nachází v tabulce databáze.</span><span class="sxs-lookup"><span data-stu-id="b482d-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="b482d-108">Generovaný kód odpovídá sloupců v tabulce databáze.</span><span class="sxs-lookup"><span data-stu-id="b482d-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="b482d-109">Tato část řady se zaměřuje na publikování webové aplikace a databáze do Azure.</span><span class="sxs-lookup"><span data-stu-id="b482d-109">This part of the series focuses on publishing the web app and database to Azure.</span></span> <span data-ttu-id="b482d-110">Další informace v tomto tématu se dozvíte o publikování webové aplikace a databáze, ale ve skutečnosti provádět kroky, které je třeba spustit na začátku tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="b482d-110">You can read this topic to learn about publishing a web app and database, but to actually perform the steps you must start at the beginning of the tutorial.</span></span> <span data-ttu-id="b482d-111">V tématu [Začínáme](setting-up-database.md).</span><span class="sxs-lookup"><span data-stu-id="b482d-111">See [Getting Started](setting-up-database.md).</span></span>


## <a name="deploy-your-web-app-on-azure"></a><span data-ttu-id="b482d-112">Nasazení webové aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="b482d-112">Deploy your web app on Azure</span></span>

<span data-ttu-id="b482d-113">Účet Azure k dokončení tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="b482d-113">You need an Azure account to complete this tutorial:</span></span>

- <span data-ttu-id="b482d-114">Můžete [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) -získáte kredity, můžete použít k vyzkoušení placených služeb Azure a i po jejich použití až můžete účet ponechat a používat bezplatné služby Azure.</span><span class="sxs-lookup"><span data-stu-id="b482d-114">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="b482d-115">Můžete [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) -vaše předplatné MSDN vám dává kredity každý měsíc, které můžete použít pro placené služby Azure.</span><span class="sxs-lookup"><span data-stu-id="b482d-115">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

<span data-ttu-id="b482d-116">K publikování webové aplikace, klikněte pravým tlačítkem na projekt a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="b482d-116">To publish your web app, right-click the project and select **Publish**.</span></span>

![publikování webu](publish-to-azure/_static/image1.png)

<span data-ttu-id="b482d-118">Vyberte weby Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="b482d-118">Select Microsoft Azure Websites.</span></span>

![Vyberte Azure](publish-to-azure/_static/image2.png)

<span data-ttu-id="b482d-120">Pokud nejste přihlášení do Azure, zadejte přihlašovací údaje účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="b482d-120">If you are not signed in to Azure, provide your Azure account credentials.</span></span> <span data-ttu-id="b482d-121">Pak vyberte nový vytvořte novou webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b482d-121">Then, select New to create a new web app.</span></span>

![nové lokality](publish-to-azure/_static/image3.png)

<span data-ttu-id="b482d-123">Vytvořte jedinečný název pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b482d-123">Create a unique name for your web app.</span></span> <span data-ttu-id="b482d-124">Budete vědět, že že název je jedinečný. Pokud se zobrazí zelená značka zaškrtnutí vpravo od pole název.</span><span class="sxs-lookup"><span data-stu-id="b482d-124">You will know the name is unique if you see a green check mark to the right of the name field.</span></span> <span data-ttu-id="b482d-125">Vyberte oblast pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b482d-125">Select a region for your web app.</span></span> <span data-ttu-id="b482d-126">Vyberte **vytvořit nový server** pro databázi a zadejte uživatelské jméno a heslo pro tento nový server databáze.</span><span class="sxs-lookup"><span data-stu-id="b482d-126">Select **Create new server** for the database, and provide a user name and password for this new database server.</span></span> <span data-ttu-id="b482d-127">Po dokončení klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b482d-127">When finished, click **Create**.</span></span>

![Vytvoření webu](publish-to-azure/_static/image4.png)

<span data-ttu-id="b482d-129">Vaše připojení hodnoty jsou nyní všechny nastavené.</span><span class="sxs-lookup"><span data-stu-id="b482d-129">Your connection values are now all set.</span></span> <span data-ttu-id="b482d-130">Můžete ponechat tyto hodnoty beze změny.</span><span class="sxs-lookup"><span data-stu-id="b482d-130">You can leave these values unchanged.</span></span>

![Připojení](publish-to-azure/_static/image5.png)

<span data-ttu-id="b482d-132">Klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="b482d-132">Click **Next**.</span></span>

<span data-ttu-id="b482d-133">Pro nastavení, Všimněte si, že dvě připojení databáze zadávají - ApplicationDBContext a ContosoUniversityDataEntities.</span><span class="sxs-lookup"><span data-stu-id="b482d-133">For Settings, notice that two database connections are specified - ApplicationDBContext and ContosoUniversityDataEntities.</span></span> <span data-ttu-id="b482d-134">ApplicationDBContext je připojení pro tabulky účtu uživatele.</span><span class="sxs-lookup"><span data-stu-id="b482d-134">ApplicationDBContext is the connection for user account tables.</span></span> <span data-ttu-id="b482d-135">Tyto hodnoty jenom zobrazit připojovací řetězce pro databáze.</span><span class="sxs-lookup"><span data-stu-id="b482d-135">These values only show the connection strings for the databases.</span></span> <span data-ttu-id="b482d-136">Neznamená to, že tyto databáze bude publikována při publikování webu.</span><span class="sxs-lookup"><span data-stu-id="b482d-136">It does not mean that these databases will be published when you publish your site.</span></span> <span data-ttu-id="b482d-137">Po dokončení publikování webové aplikace, budete publikovat projekt databáze.</span><span class="sxs-lookup"><span data-stu-id="b482d-137">You will publish your database project after you have finished publishing the web app.</span></span>

![](publish-to-azure/_static/image6.png)

<span data-ttu-id="b482d-138">Na znak výpustky (...) vedle připojení k databázi zobrazuje podrobnosti o připojovacím řetězci.</span><span class="sxs-lookup"><span data-stu-id="b482d-138">The ellipsis (...) next to the database connection shows you the details of the connection string.</span></span> <span data-ttu-id="b482d-139">Klikněte na tlačítko se třemi tečkami vedle ContosoUniversityDataEntities.</span><span class="sxs-lookup"><span data-stu-id="b482d-139">Click the ellipsis next to ContosoUniversityDataEntities.</span></span>

![](publish-to-azure/_static/image7.png)

<span data-ttu-id="b482d-140">Poznamenejte si název databázového serveru a databáze.</span><span class="sxs-lookup"><span data-stu-id="b482d-140">Note the name of the database server and the database.</span></span> <span data-ttu-id="b482d-141">Název serveru je náhodně vygenerované.</span><span class="sxs-lookup"><span data-stu-id="b482d-141">The server name is randomly generated.</span></span> <span data-ttu-id="b482d-142">Název databáze je jednoduchý název vaší lokality s  **\_db** připojí.</span><span class="sxs-lookup"><span data-stu-id="b482d-142">The database name is simple the name of your site with **\_db** appended.</span></span> <span data-ttu-id="b482d-143">Oba názvy bude potřebovat později při publikování databáze.</span><span class="sxs-lookup"><span data-stu-id="b482d-143">You will need both names later when you publish your database.</span></span>

<span data-ttu-id="b482d-144">Klikněte na tlačítko **OK** zavřete okno databáze připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="b482d-144">Click **OK** to close the database connection string window.</span></span>

<span data-ttu-id="b482d-145">V okně Publikovat Web, klikněte na **Další** zobrazíte ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="b482d-145">In the Publish Web window, click **Next** to see the preview.</span></span>

![](publish-to-azure/_static/image8.png)

<span data-ttu-id="b482d-146">Klikněte na tlačítko Spustit Náhled zobrazíte seznam souborů k publikování.</span><span class="sxs-lookup"><span data-stu-id="b482d-146">You can click Start Preview to see a list of the files to publish.</span></span> <span data-ttu-id="b482d-147">Vzhledem k tomu, že je to první, kdy jste publikovali této lokality, je v seznamu všechny relevantní soubory v projektu.</span><span class="sxs-lookup"><span data-stu-id="b482d-147">Since this is the first time you have published this site, the list is every relevant file in the project.</span></span>

<span data-ttu-id="b482d-148">Klikněte na tlačítko **publikování**.</span><span class="sxs-lookup"><span data-stu-id="b482d-148">Click **Publish**.</span></span>

<span data-ttu-id="b482d-149">V podokně výstupu se zobrazí výsledek publikace.</span><span class="sxs-lookup"><span data-stu-id="b482d-149">The Output pane will display the result of your publication.</span></span>

![](publish-to-azure/_static/image9.png)

<span data-ttu-id="b482d-150">Po publikaci web je immedialely spuštění ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="b482d-150">After publication, the site is immedialely launched in a web browser.</span></span> <span data-ttu-id="b482d-151">Po nasazení vašeho webu a můžete zaregistrovat nového uživatele na web; ale tabulek v projektu ContosoUniversityData zatím nebyly publikovány.</span><span class="sxs-lookup"><span data-stu-id="b482d-151">Your site has been deployed and you can register a new user to the site; however, your tables in the ContosoUniversityData project have not yet been published.</span></span> <span data-ttu-id="b482d-152">Pokud kliknete na seznam studenty odkaz dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="b482d-152">If you click on the List of students link you will receive an error.</span></span>

## <a name="publish-database-to-sql-azure"></a><span data-ttu-id="b482d-153">Publikování databáze SQL Azure</span><span class="sxs-lookup"><span data-stu-id="b482d-153">Publish database to SQL Azure</span></span>

<span data-ttu-id="b482d-154">Před publikováním vaší databáze, musí se ujistěte, že váš místní počítač může připojit k serveru databáze.</span><span class="sxs-lookup"><span data-stu-id="b482d-154">Before publishing your database, you must make sure your local computer can connect to the database server.</span></span> <span data-ttu-id="b482d-155">Brány firewall na serveru databáze omezuje, které počítače může připojit k databázi.</span><span class="sxs-lookup"><span data-stu-id="b482d-155">The firewall for your database server restricts which machines can connect to the database.</span></span> <span data-ttu-id="b482d-156">Je nutné přidat IP adresu počítače na povolené IP adresy pro bránu firewall.</span><span class="sxs-lookup"><span data-stu-id="b482d-156">You need to add the IP address of your computer to the allowed IP addresses for the firewall.</span></span>

<span data-ttu-id="b482d-157">Přihlášení k účtu Azure prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b482d-157">Login to your Azure account through the Azure portal.</span></span>

<span data-ttu-id="b482d-158">Vyberte nové databáze a vyberte **spravovat**.</span><span class="sxs-lookup"><span data-stu-id="b482d-158">Select your new database and select **Manage**.</span></span>

![Správa](publish-to-azure/_static/image10.png)

<span data-ttu-id="b482d-160">Je nutné nakonfigurovat databázový server umožňuje připojení z vašeho počítače.</span><span class="sxs-lookup"><span data-stu-id="b482d-160">You must configure your database server to allow connections from your computer.</span></span> <span data-ttu-id="b482d-161">Když vyberete spravovat, jste vyzváni k přidejte aktuální IP adresu, jak umožnit databázového serveru.</span><span class="sxs-lookup"><span data-stu-id="b482d-161">When you select Manage, you are asked to add the current IP address as permitted to the database server.</span></span> <span data-ttu-id="b482d-162">Vyberte možnost Ano.</span><span class="sxs-lookup"><span data-stu-id="b482d-162">Select Yes.</span></span>

![Přidat ip adresu](publish-to-azure/_static/image11.png)

<span data-ttu-id="b482d-164">Je pravděpodobné, že IP adresu, kterou jste přidali v předchozím kroku není jedinou IP adresu, kterou je nutné nakonfigurovat pro připojení.</span><span class="sxs-lookup"><span data-stu-id="b482d-164">There is a chance that the IP address you added in the previous step is not the only IP address you need to configure for connections.</span></span> <span data-ttu-id="b482d-165">Pokuste se přihlásit k databázi a zda připojení správně nastaven.</span><span class="sxs-lookup"><span data-stu-id="b482d-165">You can attempt to login to the database to see if the connections have been properly set up.</span></span> <span data-ttu-id="b482d-166">Zadejte uživatele a heslo, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="b482d-166">Provide the user and password you created earlier.</span></span>

![přihlášení](publish-to-azure/_static/image12.png)

<span data-ttu-id="b482d-168">Pokud se zobrazí chybovou zprávu, budete muset přidat další IP adresu.</span><span class="sxs-lookup"><span data-stu-id="b482d-168">If you receive an error message, you need to add another IP address.</span></span> <span data-ttu-id="b482d-169">Klikněte na tlačítko chybovou zprávu zobrazíte další podrobnosti o chybě.</span><span class="sxs-lookup"><span data-stu-id="b482d-169">Click the error message to see more details about error.</span></span> <span data-ttu-id="b482d-170">V podrobnostech uvidíte IP adresu, která je nutné přidat.</span><span class="sxs-lookup"><span data-stu-id="b482d-170">In the details you will see the IP address that you need to add.</span></span> <span data-ttu-id="b482d-171">Všimněte si tuto IP adresu.</span><span class="sxs-lookup"><span data-stu-id="b482d-171">Note this IP address.</span></span>

![není povolené.](publish-to-azure/_static/image13.png)

<span data-ttu-id="b482d-173">Zavřete toto okno přihlášení a vrátit na portál Azure.</span><span class="sxs-lookup"><span data-stu-id="b482d-173">Close this login window, and return to the Azure portal.</span></span>

<span data-ttu-id="b482d-174">Přejděte na řídicí panel pro vaši databázi.</span><span class="sxs-lookup"><span data-stu-id="b482d-174">Navigate to the Dashboard for your database.</span></span> <span data-ttu-id="b482d-175">Klikněte na tlačítko **Spravovat povolené IP adresy**.</span><span class="sxs-lookup"><span data-stu-id="b482d-175">Click **Manage allowed IP addresses**.</span></span>

![Správa ip adres](publish-to-azure/_static/image14.png)

<span data-ttu-id="b482d-177">Nyní je nutné přidat IP adresu z chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="b482d-177">You must now add the IP address from the error message.</span></span> <span data-ttu-id="b482d-178">Buď změnit rozsah povolených IP adres zahrnout jeden z chybová zpráva, nebo přidejte tuto IP adresu jako samostatný záznam.</span><span class="sxs-lookup"><span data-stu-id="b482d-178">Either change the range of allowed IP addresses to include the one from the error message, or add that IP address as a separate entry.</span></span>

![Přidání nové adresy](publish-to-azure/_static/image15.png)

<span data-ttu-id="b482d-180">Uložte změny do povolené IP adresy.</span><span class="sxs-lookup"><span data-stu-id="b482d-180">Save the change to allowed IP addresses.</span></span>

<span data-ttu-id="b482d-181">Kliknutím na spravovat a zkuste se znovu přihlásit k databázi.</span><span class="sxs-lookup"><span data-stu-id="b482d-181">Click Manage, and try logging in again to the database.</span></span> <span data-ttu-id="b482d-182">Potřebujete Počkejte několik minut, než povolené IP adresy pro bránu firewall správně nakonfigurováno.</span><span class="sxs-lookup"><span data-stu-id="b482d-182">You may need to wait a few minutes before the allowed IP addresses are correctly configured for the firewall.</span></span> <span data-ttu-id="b482d-183">Když se můžete úspěšně přihlásit databáze, jste dokončili nastavení připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="b482d-183">When you can successfully log in the database, you have finished setting up your connection to the database.</span></span>

<span data-ttu-id="b482d-184">Toto okno správy můžete nechat otevřené, protože zkontroluje výsledek nasazení databáze za chvíli.</span><span class="sxs-lookup"><span data-stu-id="b482d-184">You can leave this management window open because you will check the result of your database deployment shortly.</span></span>

<span data-ttu-id="b482d-185">Vraťte se k projektu databáze.</span><span class="sxs-lookup"><span data-stu-id="b482d-185">Return to your database project.</span></span> <span data-ttu-id="b482d-186">Klikněte pravým tlačítkem na projekt a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="b482d-186">Right-click the project and select **Publish**.</span></span>

![publikování databáze](publish-to-azure/_static/image16.png)

<span data-ttu-id="b482d-188">V okně publikování databáze vyberte **upravit**.</span><span class="sxs-lookup"><span data-stu-id="b482d-188">In the Publish Database window, select **Edit**.</span></span>

![Upravit](publish-to-azure/_static/image17.png)

<span data-ttu-id="b482d-190">Zadejte název databázového serveru a pověření pro ověření serveru.</span><span class="sxs-lookup"><span data-stu-id="b482d-190">Provide the name of the database server and your authentication credentials for the server.</span></span> <span data-ttu-id="b482d-191">Po zadání přihlašovacích údajů, vyberte databázi, kterou jste vytvořili z seznam dostupných databází.</span><span class="sxs-lookup"><span data-stu-id="b482d-191">After providing the credentials, select the database you created from the list of available databases.</span></span> <span data-ttu-id="b482d-192">Ve výchozím nastavení Visual Studio nastaví název pole databáze na název projektu, který nemusí být stejný jako databázi, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="b482d-192">By default, Visual Studio sets the name of the database field to the name of your project which might not be the same as the database you created.</span></span>

![](publish-to-azure/_static/image18.png)

<span data-ttu-id="b482d-193">Klikněte na tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="b482d-193">Click OK.</span></span>

<span data-ttu-id="b482d-194">Budete pravděpodobně chtít uložit tento profil, takže aktualizace v budoucnu můžete publikovat bez nutnosti opětovného zadání všechny informace o připojení.</span><span class="sxs-lookup"><span data-stu-id="b482d-194">You will probably want to save this profile so you can publish updates in the future without re-entering all of the connection information.</span></span> <span data-ttu-id="b482d-195">Vyberte **vytvořit profil**.</span><span class="sxs-lookup"><span data-stu-id="b482d-195">Select **Create Profile**.</span></span>

![Uložit profil](publish-to-azure/_static/image19.png)

<span data-ttu-id="b482d-197">Vytvoří soubor v projektu s názvem **ContosoUniversityData.publish.xml**.</span><span class="sxs-lookup"><span data-stu-id="b482d-197">It will create a file in your project named **ContosoUniversityData.publish.xml**.</span></span> <span data-ttu-id="b482d-198">Při příštím publikování databáze v Azure, jednoduše načíst tento profil.</span><span class="sxs-lookup"><span data-stu-id="b482d-198">The next time you want to publish the database to Azure, simply load that profile.</span></span>

<span data-ttu-id="b482d-199">Nyní, klikněte na tlačítko **publikovat** k vytvoření databáze v Azure.</span><span class="sxs-lookup"><span data-stu-id="b482d-199">Now, click **Publish** to create the database on Azure.</span></span>

![Publikování](publish-to-azure/_static/image20.png)

<span data-ttu-id="b482d-201">Po spuštění nějakou dobu, se zobrazí publikování výsledků.</span><span class="sxs-lookup"><span data-stu-id="b482d-201">After running for a while, the publishing results are displayed.</span></span>

![výsledky](publish-to-azure/_static/image21.png)

<span data-ttu-id="b482d-203">Nyní můžete přejít zpět na portálu pro správu pro vaši databázi.</span><span class="sxs-lookup"><span data-stu-id="b482d-203">Now, you can go back to the management portal for your database.</span></span> <span data-ttu-id="b482d-204">Aktualizujte zobrazení návrhu a Všimněte si, že byly nasazeny 3 tabulky s předem vyplněné data.</span><span class="sxs-lookup"><span data-stu-id="b482d-204">Refresh the design view, and notice the 3 tables with pre-filled data have been deployed.</span></span>

![nové tabulky](publish-to-azure/_static/image22.png)

<span data-ttu-id="b482d-206">Nyní jste připraveni k otestování webové aplikace, která je nasazena do Azure.</span><span class="sxs-lookup"><span data-stu-id="b482d-206">Now you are ready to test the web app that is deployed to Azure.</span></span> <span data-ttu-id="b482d-207">Přejděte na webovou aplikaci v Azure (například http://contosositeexample.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="b482d-207">Navigate to the web app on Azure (such as http://contosositeexample.azurewebsites.net/).</span></span> <span data-ttu-id="b482d-208">Klikněte na odkaz pro seznam studenty a měli byste vidět zobrazení index pro studenty.</span><span class="sxs-lookup"><span data-stu-id="b482d-208">Click the link for List of students and you should see the index view for students.</span></span>

![Zobrazení](publish-to-azure/_static/image23.png)

<span data-ttu-id="b482d-210">V některých případech databáze a připojení potřebují trochu čas musí být správně nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="b482d-210">Occasionally, the database and connection need a little time to be properly configured.</span></span> <span data-ttu-id="b482d-211">Pokud obdržíte chybu, počkejte několik minut a potom akci opakujte.</span><span class="sxs-lookup"><span data-stu-id="b482d-211">If you receive an error, wait a few minutes and then try again.</span></span>

## <a name="conclusion"></a><span data-ttu-id="b482d-212">Závěr</span><span class="sxs-lookup"><span data-stu-id="b482d-212">Conclusion</span></span>

<span data-ttu-id="b482d-213">Tato řada zadat jednoduchý příklad způsob generování kódu z existující databázi, která umožňuje uživatelům upravit, aktualizovat, vytvářet a odstraňovat data.</span><span class="sxs-lookup"><span data-stu-id="b482d-213">This series provided a simple example of how to generate code from an existing database that enables users to edit, update, create and delete data.</span></span> <span data-ttu-id="b482d-214">Použije pro vytvoření projektu ASP.NET MVC 5, rozhraní Entity Framework a generování uživatelského rozhraní ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b482d-214">It used ASP.NET MVC 5, Entity Framework and ASP.NET Scaffolding to create the project.</span></span>

<span data-ttu-id="b482d-215">Příklad úvodní Code First vývoj naleznete v části [Začínáme s ASP.NET MVC 5](../introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="b482d-215">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span>

<span data-ttu-id="b482d-216">Pokročilejší příklad najdete v tématu [vytváření datového modelu Entity Framework pro aplikace ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="b482d-216">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="b482d-217">Všimněte si, že rozhraní API DbContext, který používáte pro práci s daty v první databáze je stejný jako rozhraní API, které používáte pro práci s daty v Code First.</span><span class="sxs-lookup"><span data-stu-id="b482d-217">Note that the DbContext API that you use for working with data in Database First is the same as the API you use for working with data in Code First.</span></span> <span data-ttu-id="b482d-218">I v případě, že máte v úmyslu použít Database First, můžete naučit pro zpracování složitějších scénáře, jako je čtení a aktualizaci související data, zpracování konfliktů souběžnosti, a tak dále z Code First kurzu.</span><span class="sxs-lookup"><span data-stu-id="b482d-218">Even if you intend to use Database First, you can learn how to handle more complex scenarios such as reading and updating related data, handling concurrency conflicts, and so forth from a Code First tutorial.</span></span> <span data-ttu-id="b482d-219">Jediný rozdíl spočívá v tom, jak vytvořit databázi, třída kontextu a tříd entit.</span><span class="sxs-lookup"><span data-stu-id="b482d-219">The only difference is in how the database, context class, and entity classes are created.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="b482d-220">Předchozí</span><span class="sxs-lookup"><span data-stu-id="b482d-220">Previous</span></span>](enhancing-data-validation.md)
