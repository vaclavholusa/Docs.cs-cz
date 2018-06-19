---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: Provádění a jaké Pokud nasazení | Microsoft Docs
author: jrjlee
description: Toto téma popisuje, co když' provádění (nebo simulated) pomocí nástroj nasazení webu Internetové informační služby (IIS) (Web Deploy) a V nasazení...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: c1a13f38c8e629bcd615190b00104109e25fb289
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879982"
---
<a name="performing-a-what-if-deployment"></a><span data-ttu-id="bb10c-103">Provádění nasazení "Co když"</span><span class="sxs-lookup"><span data-stu-id="bb10c-103">Performing a "What If" Deployment</span></span>
====================
<span data-ttu-id="bb10c-104">podle [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="bb10c-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="bb10c-105">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="bb10c-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="bb10c-106">Toto téma popisuje, jak provést "Co když" (nebo simulated) pomocí nástroj nasazení webu Internetové informační služby (IIS) (Web Deploy) a VSDBCMD nasazení.</span><span class="sxs-lookup"><span data-stu-id="bb10c-106">This topic describes how to perform "what if" (or simulated) deployments using the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) and VSDBCMD.</span></span> <span data-ttu-id="bb10c-107">Díky tomu můžete určit důsledky logika nasazení v prostředí s konkrétní cílový před svou aplikaci nasazujete ve skutečnosti.</span><span class="sxs-lookup"><span data-stu-id="bb10c-107">This lets you determine the effects of your deployment logic on a particular target environment before you actually deploy your application.</span></span>


<span data-ttu-id="bb10c-108">Toto téma je součástí ze série kurzů na základě kolem podnikové požadavky nasazení fiktivní společnost s názvem Fabrikam, Inc. Tento kurz řady používá ukázkové řešení&#x2014; [řešení obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s úrovní realistické složitější, včetně aplikace ASP.NET MVC 3, komunikaci Windows Služba Foundation (WCF) a projekt databáze.</span><span class="sxs-lookup"><span data-stu-id="bb10c-108">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="bb10c-109">Metoda nasazení jádrem tyto kurzy je založena na popsaný přístup souboru projektu rozdělení [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve kterém je řízena procesem sestavení a nasazení dva soubory projektu&#x2014;jeden obsahující sestavení pokyny, které platí pro každé cílové prostředí a jeden, který obsahuje nastavení pro konkrétní prostředí sestavení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="bb10c-109">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build and deployment process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="bb10c-110">V okamžiku sestavení souboru projektu konkrétní prostředí sloučeny do souboru projektu bez ohledu na prostředí a vytvořit úplnou sadu pokynů sestavení.</span><span class="sxs-lookup"><span data-stu-id="bb10c-110">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="performing-a-what-if-deployment-for-web-packages"></a><span data-ttu-id="bb10c-111">Provádění pro balíčky webového nasazení "Co když"</span><span class="sxs-lookup"><span data-stu-id="bb10c-111">Performing a "What If" Deployment for Web Packages</span></span>

<span data-ttu-id="bb10c-112">Nasazení webu obsahuje funkce, které umožňuje provádění nasazení v "Co když" (nebo zkušební verze) režimu.</span><span class="sxs-lookup"><span data-stu-id="bb10c-112">Web Deploy includes functionality that lets you perform deployments in "what if" (or trial) mode.</span></span> <span data-ttu-id="bb10c-113">Když nasadíte artefakty v režimu "Co když", Web Deploy vygeneruje soubor protokolu, jako kdyby měl provést nasazení, ale se nic nezmění. ve skutečnosti na cílovém serveru.</span><span class="sxs-lookup"><span data-stu-id="bb10c-113">When you deploy artifacts in "what if" mode, Web Deploy generates a log file as if you had performed the deployment, but it doesn't actually change anything on the destination server.</span></span> <span data-ttu-id="bb10c-114">Kontrola souboru protokolu vám můžou pomoct pochopit, jaký dopad bude mít vaše nasazení na cílovém serveru, zejména:</span><span class="sxs-lookup"><span data-stu-id="bb10c-114">Reviewing the log file can help you to understand what impact your deployment will have on the destination server, in particular:</span></span>

- <span data-ttu-id="bb10c-115">Co se se přidají.</span><span class="sxs-lookup"><span data-stu-id="bb10c-115">What will get added.</span></span>
- <span data-ttu-id="bb10c-116">Co se aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="bb10c-116">What will get updated.</span></span>
- <span data-ttu-id="bb10c-117">Co bude odstraněn.</span><span class="sxs-lookup"><span data-stu-id="bb10c-117">What will get deleted.</span></span>

<span data-ttu-id="bb10c-118">Protože je nasazení "Co když" nic nezmění. ve skutečnosti na cílovém serveru se nemůže vždy provádět předpovědi, zda bude úspěšné nasazení.</span><span class="sxs-lookup"><span data-stu-id="bb10c-118">Because a "what if" deployment doesn't actually change anything on the destination server, what it can't always do is predict whether a deployment will succeed.</span></span>

<span data-ttu-id="bb10c-119">Jak je popsáno v [nasazení webových balíčků](../web-deployment-in-the-enterprise/deploying-web-packages.md), bude možné nasadit balíčky webového pomocí nástroje nasazení webu dvěma způsoby&#x2014;pomocí nástroje příkazového řádku MSDeploy.exe přímo nebo spuštěním *. deploy.cmd* souboru aby generuje procesu sestavení.</span><span class="sxs-lookup"><span data-stu-id="bb10c-119">As described in [Deploying Web Packages](../web-deployment-in-the-enterprise/deploying-web-packages.md), you can deploy web packages using Web Deploy in two ways&#x2014;by using the MSDeploy.exe command-line utility directly or by running the *.deploy.cmd* file that the build process generates.</span></span>

<span data-ttu-id="bb10c-120">Pokud používáte MSDeploy.exe přímo, můžete spustit nasazení "Co když" tak, že přidáte **– whatif** příznak do příkazu.</span><span class="sxs-lookup"><span data-stu-id="bb10c-120">If you're using MSDeploy.exe directly, you can run a "what if" deployment by adding the **–whatif** flag to your command.</span></span> <span data-ttu-id="bb10c-121">Například k vyhodnocení, co by mohlo dojít, pokud jste nasadili balíček ContactManager.Mvc.zip pracovní prostředí, příkaz MSDeploy by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="bb10c-121">For example, to evaluate what would happen if you deployed the ContactManager.Mvc.zip package to a staging environment, the MSDeploy command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]


<span data-ttu-id="bb10c-122">Až budete spokojeni s výsledky nasazení "Co když", můžete odebrat **– whatif** příznak pro spuštění nasazení za provozu.</span><span class="sxs-lookup"><span data-stu-id="bb10c-122">When you're satisfied with the results of your "what if" deployment, you can remove the **–whatif** flag to run a live deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="bb10c-123">Další informace o parametrech příkazového řádku pro MSDeploy.exe najdete v tématu [nastavení operace nasazení webu](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="bb10c-123">For more information on command-line options for MSDeploy.exe, see [Web Deploy Operation Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span></span>


<span data-ttu-id="bb10c-124">Pokud používáte *. deploy.cmd* souboru, můžete spustit nasazení "Co když" zahrnutím **/t** příznak příznak (zkušební režim) místo **/y** příznak ("Ano" nebo režim aktualizace) v váš příkaz.</span><span class="sxs-lookup"><span data-stu-id="bb10c-124">If you're using the *.deploy.cmd* file, you can run a "what if" deployment by including the **/t** flag (trial mode) flag instead of the **/y** flag ("yes," or update mode) in your command.</span></span> <span data-ttu-id="bb10c-125">Například k vyhodnocení, co by mohlo dojít, pokud jste nasadili balíček ContactManager.Mvc.zip spuštěním *. deploy.cmd* souboru příkazu by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="bb10c-125">For example, to evaluate what would happen if you deployed the ContactManager.Mvc.zip package by running the *.deploy.cmd* file, your command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]


<span data-ttu-id="bb10c-126">Až budete spokojeni s výsledky nasazení "zkušební režim", můžete nahradit **/t** příznak s **/y** příznak pro spuštění nasazení za provozu:</span><span class="sxs-lookup"><span data-stu-id="bb10c-126">When you're satisfied with the results of your "trial mode" deployment, you can replace the **/t** flag with a **/y** flag to run a live deployment:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="bb10c-127">Další informace o parametrech příkazového řádku pro *. deploy.cmd* soubory, najdete v části [postupy: instalace nasazení balíčku pomocí souboru deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx).</span><span class="sxs-lookup"><span data-stu-id="bb10c-127">For more information on command-line options for *.deploy.cmd* files, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span> <span data-ttu-id="bb10c-128">Pokud spustíte *. deploy.cmd* souboru bez zadání žádné příznaky, příkazovém řádku se zobrazí seznam dostupných příznaků.</span><span class="sxs-lookup"><span data-stu-id="bb10c-128">If you run the *.deploy.cmd* file without specifying any flags, the command prompt will display a list of available flags.</span></span>


## <a name="performing-a-what-if-deployment-for-databases"></a><span data-ttu-id="bb10c-129">Provádění nasazení "Co když" pro databáze</span><span class="sxs-lookup"><span data-stu-id="bb10c-129">Performing a "What If" Deployment for Databases</span></span>

<span data-ttu-id="bb10c-130">V této části se předpokládá, že používáte nástroj VSDBCMD k provedení nasazení přírůstkové, na základě schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="bb10c-130">This section assumes that you're using the VSDBCMD utility to perform incremental, schema-based database deployment.</span></span> <span data-ttu-id="bb10c-131">Tento postup je popsán v podrobněji [nasazení databázové projekty](../web-deployment-in-the-enterprise/deploying-database-projects.md).</span><span class="sxs-lookup"><span data-stu-id="bb10c-131">This approach is described in more detail in [Deploying Database Projects](../web-deployment-in-the-enterprise/deploying-database-projects.md).</span></span> <span data-ttu-id="bb10c-132">Doporučujeme vám, že je seznámit s tímto tématem před použitím konceptů popsaných v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="bb10c-132">We recommend that you familiarize yourself with this topic before you apply the concepts described here.</span></span>

<span data-ttu-id="bb10c-133">Při použití VSDBCMD v **nasadit** režimu, můžete použít **/dd** (nebo **/DeployToDatabase**) příznak pro ovládací prvek, ať už VSDBCMD ve skutečnosti nasadí databázi nebo právě generuje skript nasazení.</span><span class="sxs-lookup"><span data-stu-id="bb10c-133">When you use VSDBCMD in **Deploy** mode, you can use the **/dd** (or **/DeployToDatabase**) flag to control whether VSDBCMD actually deploys the database or just generates a deployment script.</span></span> <span data-ttu-id="bb10c-134">Pokud nasazujete soubor .dbschema, jedná se o chování:</span><span class="sxs-lookup"><span data-stu-id="bb10c-134">If you're deploying a .dbschema file, this is the behavior:</span></span>

- <span data-ttu-id="bb10c-135">Pokud zadáte **/dd+** nebo **/dd**, VSDBCMD vygenerovat skript nasazení a nasazení databáze.</span><span class="sxs-lookup"><span data-stu-id="bb10c-135">If you specify **/dd+** or **/dd**, VSDBCMD will generate a deployment script and deploy the database.</span></span>
- <span data-ttu-id="bb10c-136">Pokud zadáte **/dd-** nebo vynechejte přepínač, VSDBCMD vydá skript nasazení.</span><span class="sxs-lookup"><span data-stu-id="bb10c-136">If you specify **/dd-** or omit the switch, VSDBCMD will generate a deployment script only.</span></span>

> [!NOTE]
> <span data-ttu-id="bb10c-137">Pokud nasazujete souboru .deploymanifest místo soubor .dbschema chování **/dd** přepínač je mnohem složitější.</span><span class="sxs-lookup"><span data-stu-id="bb10c-137">If you're deploying a .deploymanifest file rather than a .dbschema file, the behavior of the **/dd** switch is a lot more complicated.</span></span> <span data-ttu-id="bb10c-138">V podstatě VSDBCMD bude ignorovat hodnotu **/dd** přepínače, pokud soubor .deploymanifest obsahuje **DeployToDatabase** element s hodnotou **True**.</span><span class="sxs-lookup"><span data-stu-id="bb10c-138">Essentially, VSDBCMD will ignore the value of the **/dd** switch if the .deploymanifest file includes a **DeployToDatabase** element with a value of **True**.</span></span> <span data-ttu-id="bb10c-139">[Nasazení databázové projekty](../web-deployment-in-the-enterprise/deploying-database-projects.md) popisuje toto chování v plném znění.</span><span class="sxs-lookup"><span data-stu-id="bb10c-139">[Deploying Database Projects](../web-deployment-in-the-enterprise/deploying-database-projects.md) describes this behavior in full.</span></span>


<span data-ttu-id="bb10c-140">Například pro generování skriptu pro nasazení **ContactManager** databáze bez ve skutečnosti nasazení databáze, VSDBCMD příkazu by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="bb10c-140">For example, to generate a deployment script for the **ContactManager** database without actually deploying the database, your VSDBCMD command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]


<span data-ttu-id="bb10c-141">VSDBCMD je nástroj pro nasazení rozdílové databáze a jako takový se dynamicky vygeneruje skript nasazení tak, aby obsahovala všechny příkazy SQL potřebnými k aktualizaci aktuální databázi, pokud existuje, zadaný schématu.</span><span class="sxs-lookup"><span data-stu-id="bb10c-141">VSDBCMD is a differential database deployment tool, and as such the deployment script is dynamically generated to contain all the SQL commands necessary to update the current database, if one exists, to the specified schema.</span></span> <span data-ttu-id="bb10c-142">Kontrola skript nasazení je užitečný způsob, jak určit, jaký vliv má vaše nasazení bude mít na aktuální databázi a data, která obsahuje.</span><span class="sxs-lookup"><span data-stu-id="bb10c-142">Reviewing the deployment script is a useful way to determine what impact your deployment will have on the current database and the data it contains.</span></span> <span data-ttu-id="bb10c-143">Například můžete chtít zjistit:</span><span class="sxs-lookup"><span data-stu-id="bb10c-143">For example, you might want to determine:</span></span>

- <span data-ttu-id="bb10c-144">Určuje, zda bude odebrána veškeré stávající tabulky a jestli, dojde ke ztrátě dat.</span><span class="sxs-lookup"><span data-stu-id="bb10c-144">Whether any existing tables will be removed, and whether that will result in data loss.</span></span>
- <span data-ttu-id="bb10c-145">Jestli pořadí operací s sebou nese riziko ztráty dat, například pokud jste rozdělení nebo sloučení tabulky.</span><span class="sxs-lookup"><span data-stu-id="bb10c-145">Whether the order of operations carries a risk of data loss, for example, if you're splitting or merging tables.</span></span>

<span data-ttu-id="bb10c-146">Pokud budete spokojeni s skript nasazení, můžete opakovat VSDBCMD s **/dd+** příznak provést změny.</span><span class="sxs-lookup"><span data-stu-id="bb10c-146">If you're happy with the deployment script, you can repeat the VSDBCMD with a **/dd+** flag to make the changes.</span></span> <span data-ttu-id="bb10c-147">Alternativně můžete upravit skript nasazení k plnění požadavků a pak ji ručně spustit na serveru databáze.</span><span class="sxs-lookup"><span data-stu-id="bb10c-147">Alternatively, you can edit the deployment script to meet your requirements and then execute it manually on the database server.</span></span>

## <a name="integrating-what-if-functionality-into-custom-project-files"></a><span data-ttu-id="bb10c-148">Integrace funkce "Co když" do vlastních projektů souborů</span><span class="sxs-lookup"><span data-stu-id="bb10c-148">Integrating "What If" Functionality into Custom Project Files</span></span>

<span data-ttu-id="bb10c-149">Ve složitějších scénářích nasazení, budete chtít použít vlastní soubor projektu Microsoft Build Engine (MSBuild) k zapouzdření logika sestavení a nasazení, jak je popsáno v [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span><span class="sxs-lookup"><span data-stu-id="bb10c-149">In more complex deployment scenarios, you'll want to use a custom Microsoft Build Engine (MSBuild) project file to encapsulate your build and deployment logic, as described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span></span> <span data-ttu-id="bb10c-150">Například v [obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) ukázkové řešení, *Publish.proj* souboru:</span><span class="sxs-lookup"><span data-stu-id="bb10c-150">For example, in the [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) sample solution, the *Publish.proj* file:</span></span>

- <span data-ttu-id="bb10c-151">Sestavení řešení.</span><span class="sxs-lookup"><span data-stu-id="bb10c-151">Builds the solution.</span></span>
- <span data-ttu-id="bb10c-152">Zabalení a nasazení aplikace ContactManager.Mvc používá nasazení webu.</span><span class="sxs-lookup"><span data-stu-id="bb10c-152">Uses Web Deploy to package and deploy the ContactManager.Mvc application.</span></span>
- <span data-ttu-id="bb10c-153">Zabalení a nasazení aplikace ContactManager.Service používá nasazení webu.</span><span class="sxs-lookup"><span data-stu-id="bb10c-153">Uses Web Deploy to package and deploy the ContactManager.Service application.</span></span>
- <span data-ttu-id="bb10c-154">Nasadí **ContactManager** databáze.</span><span class="sxs-lookup"><span data-stu-id="bb10c-154">Deploys the **ContactManager** database.</span></span>

<span data-ttu-id="bb10c-155">Při integraci nasazení více webových balíčků nebo databáze do procesu krokování tímto způsobem, můžete také možnost provedení celé nasazení v režimu "Co když".</span><span class="sxs-lookup"><span data-stu-id="bb10c-155">When you integrate the deployment of multiple web packages and/or databases into a single-step process in this way, you may also want the option of performing the entire deployment in a "what if" mode.</span></span>

<span data-ttu-id="bb10c-156">*Publish.proj* souboru ukazuje, jak můžete to provést.</span><span class="sxs-lookup"><span data-stu-id="bb10c-156">The *Publish.proj* file demonstrates how you can do this.</span></span> <span data-ttu-id="bb10c-157">Nejprve musíte vytvořit vlastnost pro uložení hodnotu "Co když":</span><span class="sxs-lookup"><span data-stu-id="bb10c-157">First, you need to create a property to store the "what if" value:</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]


<span data-ttu-id="bb10c-158">V takovém případě jste vytvořili vlastnost s názvem **WhatIf** s výchozí hodnotou **false**.</span><span class="sxs-lookup"><span data-stu-id="bb10c-158">In this case, you've created a property named **WhatIf** with a default value of **false**.</span></span> <span data-ttu-id="bb10c-159">Uživatelé mohou tuto hodnotu přepsat nastavením vlastnosti **true** parametr příkazového řádku, jako je zobrazí za chvíli.</span><span class="sxs-lookup"><span data-stu-id="bb10c-159">Users can override this value by setting the property to **true** in a command-line parameter, as you'll see shortly.</span></span>

<span data-ttu-id="bb10c-160">Další fáze je o parametrizaci nasazení webu a VSDBCMD příkazů tak, aby odrážela příznaků **WhatIf** hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="bb10c-160">The next stage is to parameterize any Web Deploy and VSDBCMD commands so that the flags reflect the **WhatIf** property value.</span></span> <span data-ttu-id="bb10c-161">Například Další cíl (převzaty z *Publish.proj* souborů a zjednodušená) běží *. deploy.cmd* souboru k nasazení webového balíčku.</span><span class="sxs-lookup"><span data-stu-id="bb10c-161">For example, the next target (taken from the *Publish.proj* file and simplified) runs the *.deploy.cmd* file to deploy a web package.</span></span> <span data-ttu-id="bb10c-162">Ve výchozím nastavení, příkaz obsahuje **/Y** přepínače ("Ano" nebo režim aktualizace).</span><span class="sxs-lookup"><span data-stu-id="bb10c-162">By default, the command includes a **/Y** switch ("yes," or update mode).</span></span> <span data-ttu-id="bb10c-163">Pokud **WhatIf** je nastaven na **true**, to je nahrazena **/T** přepínače (zkušební verze nebo režim "Co když").</span><span class="sxs-lookup"><span data-stu-id="bb10c-163">If **WhatIf** is set to **true**, this is replaced by a **/T** switch (trial, or "what if" mode).</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]


<span data-ttu-id="bb10c-164">Podobně Další cíl používá nástroj VSDBCMD nasazení databáze.</span><span class="sxs-lookup"><span data-stu-id="bb10c-164">Similarly, the next target uses the VSDBCMD utility to deploy a database.</span></span> <span data-ttu-id="bb10c-165">Ve výchozím nastavení **/dd** přepínač není zahrnutý.</span><span class="sxs-lookup"><span data-stu-id="bb10c-165">By default, a **/dd** switch is not included.</span></span> <span data-ttu-id="bb10c-166">To znamená, že VSDBCMD vygeneruje skript nasazení, ale nebude nasazení databáze&#x2014;jinými slovy, "Co když" scénáři.</span><span class="sxs-lookup"><span data-stu-id="bb10c-166">This means that VSDBCMD will generate a deployment script but will not deploy the database&#x2014;in other words, a "what if" scenario.</span></span> <span data-ttu-id="bb10c-167">Pokud **WhatIf** vlastnost není nastavena na **true**, **/dd** přidán přepínač a VSDBCMD se nasazení databáze.</span><span class="sxs-lookup"><span data-stu-id="bb10c-167">If the **WhatIf** property is not set to **true**, a **/dd** switch is added and VSDBCMD will deploy the database.</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]


<span data-ttu-id="bb10c-168">Můžete použít stejný způsob o parametrizaci všechny relevantní příkazy v souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="bb10c-168">You can use the same approach to parameterize all the relevant commands in your project file.</span></span> <span data-ttu-id="bb10c-169">Pokud chcete spustit nasazení "Co když", můžete pak stačí zadat **WhatIf** hodnotu vlastnosti z příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="bb10c-169">When you want to run a "what if" deployment, you can then simply provide a **WhatIf** property value from the command line:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]


<span data-ttu-id="bb10c-170">Tímto způsobem můžete spustit "Co když" nasazení všech součástí projektu v jediném kroku.</span><span class="sxs-lookup"><span data-stu-id="bb10c-170">In this way, you can run a "what if" deployment for all your project components in a single step.</span></span>

## <a name="conclusion"></a><span data-ttu-id="bb10c-171">Závěr</span><span class="sxs-lookup"><span data-stu-id="bb10c-171">Conclusion</span></span>

<span data-ttu-id="bb10c-172">Toto téma popisuje postup spouštět "Co když" nasazení pomocí nástroje nasazení webu, VSDBCMD a MSBuild.</span><span class="sxs-lookup"><span data-stu-id="bb10c-172">This topic described how to run "what if" deployments using Web Deploy, VSDBCMD, and MSBuild.</span></span> <span data-ttu-id="bb10c-173">Nasazení "Co když" umožňuje hodnotit dopad navrhované nasazení před provedením jakýchkoli změn ve skutečnosti na cílovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="bb10c-173">A "what if" deployment lets you evaluate the impact of a proposed deployment before you actually make any changes to the destination environment.</span></span>

## <a name="further-reading"></a><span data-ttu-id="bb10c-174">Další čtení</span><span class="sxs-lookup"><span data-stu-id="bb10c-174">Further Reading</span></span>

<span data-ttu-id="bb10c-175">Další informace o nasazení webu syntaxe příkazového řádku najdete v tématu [nastavení operace nasazení webu](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="bb10c-175">For more information on Web Deploy command-line syntax, see [Web Deploy Operation Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span></span> <span data-ttu-id="bb10c-176">Pokyny k možnosti příkazového řádku při použití *. deploy.cmd* souborů najdete v tématu [postup: instalace nasazení balíčku pomocí souboru deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx).</span><span class="sxs-lookup"><span data-stu-id="bb10c-176">For guidance on command-line options when you use the *.deploy.cmd* file, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span> <span data-ttu-id="bb10c-177">Pokyny v VSDBCMD syntaxe příkazového řádku najdete v tématu [Reference k příkazovému řádku pro VSDBCMD. EXE (nasazení a Import schématu)](https://msdn.microsoft.com/library/dd193283.aspx).</span><span class="sxs-lookup"><span data-stu-id="bb10c-177">For guidance on VSDBCMD command-line syntax, see [Command-Line Reference for VSDBCMD.EXE (Deployment and Schema Import)](https://msdn.microsoft.com/library/dd193283.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bb10c-178">[Předchozí](advanced-enterprise-web-deployment.md)
> [další](customizing-database-deployments-for-multiple-environments.md)</span><span class="sxs-lookup"><span data-stu-id="bb10c-178">[Previous](advanced-enterprise-web-deployment.md)
[Next](customizing-database-deployments-for-multiple-environments.md)</span></span>
