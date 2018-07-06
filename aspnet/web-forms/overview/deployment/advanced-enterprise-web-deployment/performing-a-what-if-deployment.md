---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: Co provádění, pokud nasazení | Dokumentace Microsoftu
author: jrjlee
description: Toto téma popisuje, jak provést "Co když" (nebo simulovaných) pomocí nástroje nasazení webu Internetové informační služby (IIS) (nasazení webu) a V nasazení...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: f54a4d91ea0f735d3cd5b99c0dda5d83cbb5e5d4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830646"
---
<a name="performing-a-what-if-deployment"></a><span data-ttu-id="6de94-103">Provádění nasazení "What If"</span><span class="sxs-lookup"><span data-stu-id="6de94-103">Performing a "What If" Deployment</span></span>
====================
<span data-ttu-id="6de94-104">podle [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="6de94-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="6de94-105">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="6de94-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="6de94-106">Toto téma popisuje, jak provést "what if" (nebo simulovaných) pomocí nástroje nasazení webu Internetové informační služby (IIS) (nasazení webu) a VSDBCMD nasazení.</span><span class="sxs-lookup"><span data-stu-id="6de94-106">This topic describes how to perform "what if" (or simulated) deployments using the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) and VSDBCMD.</span></span> <span data-ttu-id="6de94-107">To umožňuje zjistit dopady logiky nasazení na konkrétní cílové prostředí před při skutečném nasazení vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="6de94-107">This lets you determine the effects of your deployment logic on a particular target environment before you actually deploy your application.</span></span>


<span data-ttu-id="6de94-108">Toto téma je součástí série kurzů podle požadavků na nasazení enterprise fiktivní společnosti s názvem společnosti Fabrikam, Inc. V této sérii kurzů používá ukázkové řešení&#x2014; [řešení Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s realistické úroveň složitosti, včetně aplikace ASP.NET MVC 3, komunikace Windows Služba Foundation (WCF) a databázový projekt.</span><span class="sxs-lookup"><span data-stu-id="6de94-108">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="6de94-109">Metody nasazení v srdci těchto kurzů je založen na rozdělení přístupu soubor projektu je popsáno v [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve kterém je řízena procesem sestavení a nasazení dva soubory projektu&#x2014;jeden obsahuje pokyny pro sestavení, které platí pro všechny cílové prostředí a jeden obsahuje nastavení pro konkrétní prostředí sestavení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="6de94-109">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build and deployment process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="6de94-110">V okamžiku sestavení souboru projektu specifických pro prostředí se sloučí do souboru projektu bez ohledu na prostředí a vytvoří kompletní sadu pokynů sestavení.</span><span class="sxs-lookup"><span data-stu-id="6de94-110">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="performing-a-what-if-deployment-for-web-packages"></a><span data-ttu-id="6de94-111">Probíhá nasazení "What If" webových balíčků</span><span class="sxs-lookup"><span data-stu-id="6de94-111">Performing a "What If" Deployment for Web Packages</span></span>

<span data-ttu-id="6de94-112">Nástroj nasazení webu obsahuje funkce, které umožňuje provádět nasazení v "what if" (nebo zkušební verze) režimu.</span><span class="sxs-lookup"><span data-stu-id="6de94-112">Web Deploy includes functionality that lets you perform deployments in "what if" (or trial) mode.</span></span> <span data-ttu-id="6de94-113">Při nasazování artefaktů v režimu "what if" Webdeploy vygeneruje soubor protokolu, jako kdyby jste provedli nasazení, ale nic se nezmění ve skutečnosti na cílovém serveru.</span><span class="sxs-lookup"><span data-stu-id="6de94-113">When you deploy artifacts in "what if" mode, Web Deploy generates a log file as if you had performed the deployment, but it doesn't actually change anything on the destination server.</span></span> <span data-ttu-id="6de94-114">Kontrola souboru protokolu vám můžou pomoct pochopit, jaký vliv budou mít vaše nasazení na cílovém serveru, zejména:</span><span class="sxs-lookup"><span data-stu-id="6de94-114">Reviewing the log file can help you to understand what impact your deployment will have on the destination server, in particular:</span></span>

- <span data-ttu-id="6de94-115">Co se přidají.</span><span class="sxs-lookup"><span data-stu-id="6de94-115">What will get added.</span></span>
- <span data-ttu-id="6de94-116">Co bude aktualizován.</span><span class="sxs-lookup"><span data-stu-id="6de94-116">What will get updated.</span></span>
- <span data-ttu-id="6de94-117">Co bude odstraněn.</span><span class="sxs-lookup"><span data-stu-id="6de94-117">What will get deleted.</span></span>

<span data-ttu-id="6de94-118">Protože nasazení "what if" ve skutečnosti nic nezmění na cílovém serveru, se nemůže vždy proveďte je Předvídejte, jestli nasazení proběhne úspěšně.</span><span class="sxs-lookup"><span data-stu-id="6de94-118">Because a "what if" deployment doesn't actually change anything on the destination server, what it can't always do is predict whether a deployment will succeed.</span></span>

<span data-ttu-id="6de94-119">Jak je popsáno v [nasazení webových balíčků](../web-deployment-in-the-enterprise/deploying-web-packages.md), můžete nasadit pomocí nasazení webu dvěma způsoby webových balíčků&#x2014;pomocí nástroje pro příkazový řádek MSDeploy.exe přímo nebo spuštěním *. deploy.cmd* souboru který generuje procesu sestavení.</span><span class="sxs-lookup"><span data-stu-id="6de94-119">As described in [Deploying Web Packages](../web-deployment-in-the-enterprise/deploying-web-packages.md), you can deploy web packages using Web Deploy in two ways&#x2014;by using the MSDeploy.exe command-line utility directly or by running the *.deploy.cmd* file that the build process generates.</span></span>

<span data-ttu-id="6de94-120">Pokud používáte MSDeploy.exe přímo, můžete spustit nasazení "what if" tak, že přidáte **– whatif** příznak do svých rukou.</span><span class="sxs-lookup"><span data-stu-id="6de94-120">If you're using MSDeploy.exe directly, you can run a "what if" deployment by adding the **–whatif** flag to your command.</span></span> <span data-ttu-id="6de94-121">Například k vyhodnocení, co by mohlo dojít, pokud jste nasadili ContactManager.Mvc.zip balíček do přípravného prostředí, příkaz MSDeploy by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="6de94-121">For example, to evaluate what would happen if you deployed the ContactManager.Mvc.zip package to a staging environment, the MSDeploy command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]


<span data-ttu-id="6de94-122">Jakmile budete spokojeni s výsledky nasazení "what if", můžete odebrat **– whatif** příznak pro spuštění nasazení za provozu.</span><span class="sxs-lookup"><span data-stu-id="6de94-122">When you're satisfied with the results of your "what if" deployment, you can remove the **–whatif** flag to run a live deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="6de94-123">Další informace o možnostech příkazového řádku pro MSDeploy.exe najdete v tématu [nastavení operace nasazení webu](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="6de94-123">For more information on command-line options for MSDeploy.exe, see [Web Deploy Operation Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span></span>


<span data-ttu-id="6de94-124">Pokud používáte *. deploy.cmd* soubor, můžete spustit nasazení "what if" zahrnutím **/t** příznak příznak (zkušební režim) místo **/y** příznak ("Ano", nebo režim aktualizace) v příkaz.</span><span class="sxs-lookup"><span data-stu-id="6de94-124">If you're using the *.deploy.cmd* file, you can run a "what if" deployment by including the **/t** flag (trial mode) flag instead of the **/y** flag ("yes," or update mode) in your command.</span></span> <span data-ttu-id="6de94-125">Například chcete vyhodnotit, co by mohlo dojít, pokud jste nasadili balíček ContactManager.Mvc.zip spuštěním *. deploy.cmd* souboru příkazu by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="6de94-125">For example, to evaluate what would happen if you deployed the ContactManager.Mvc.zip package by running the *.deploy.cmd* file, your command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]


<span data-ttu-id="6de94-126">Jakmile budete spokojeni s výsledky nasazení "zkušební režim", můžete nahradit **/t** s příznakem **/y** příznak pro spuštění nasazení za provozu:</span><span class="sxs-lookup"><span data-stu-id="6de94-126">When you're satisfied with the results of your "trial mode" deployment, you can replace the **/t** flag with a **/y** flag to run a live deployment:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="6de94-127">Další informace o možnostech příkazového řádku pro *. deploy.cmd* soubory, naleznete v tématu [postupy: instalace nasazení balíčku pomocí souboru deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx).</span><span class="sxs-lookup"><span data-stu-id="6de94-127">For more information on command-line options for *.deploy.cmd* files, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span> <span data-ttu-id="6de94-128">Pokud spustíte *. deploy.cmd* souboru bez zadání jakékoli příznaky příkazového řádku se zobrazí seznam dostupných příznaků.</span><span class="sxs-lookup"><span data-stu-id="6de94-128">If you run the *.deploy.cmd* file without specifying any flags, the command prompt will display a list of available flags.</span></span>


## <a name="performing-a-what-if-deployment-for-databases"></a><span data-ttu-id="6de94-129">Probíhá nasazení "What If" pro databáze</span><span class="sxs-lookup"><span data-stu-id="6de94-129">Performing a "What If" Deployment for Databases</span></span>

<span data-ttu-id="6de94-130">V této části se předpokládá, že používáte nástroj VSDBCMD provádět nasazení přírůstkové, založené na schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="6de94-130">This section assumes that you're using the VSDBCMD utility to perform incremental, schema-based database deployment.</span></span> <span data-ttu-id="6de94-131">Tento přístup je popsána podrobněji [nasazení databázové projekty](../web-deployment-in-the-enterprise/deploying-database-projects.md).</span><span class="sxs-lookup"><span data-stu-id="6de94-131">This approach is described in more detail in [Deploying Database Projects](../web-deployment-in-the-enterprise/deploying-database-projects.md).</span></span> <span data-ttu-id="6de94-132">Doporučujeme, aby měli seznámit s tímto tématem před použitím koncepty popsané tady.</span><span class="sxs-lookup"><span data-stu-id="6de94-132">We recommend that you familiarize yourself with this topic before you apply the concepts described here.</span></span>

<span data-ttu-id="6de94-133">Při použití VSDBCMD v **nasadit** režimu, můžete použít **/dd** (nebo **/DeployToDatabase**) příznak, který ovládací prvek, zda VSDBCMD provede samotné nasazení databáze nebo právě generuje skript nasazení.</span><span class="sxs-lookup"><span data-stu-id="6de94-133">When you use VSDBCMD in **Deploy** mode, you can use the **/dd** (or **/DeployToDatabase**) flag to control whether VSDBCMD actually deploys the database or just generates a deployment script.</span></span> <span data-ttu-id="6de94-134">Pokud nasazujete .dbschema souboru, toto chování je:</span><span class="sxs-lookup"><span data-stu-id="6de94-134">If you're deploying a .dbschema file, this is the behavior:</span></span>

- <span data-ttu-id="6de94-135">Pokud zadáte **/dd+** nebo **/dd**, VSDBCMD vygenerovat skript nasazení a nasazení databáze.</span><span class="sxs-lookup"><span data-stu-id="6de94-135">If you specify **/dd+** or **/dd**, VSDBCMD will generate a deployment script and deploy the database.</span></span>
- <span data-ttu-id="6de94-136">Pokud zadáte **/dd-** nebo vynechejte přepínač, VSDBCMD vygeneruje jenom skript nasazení.</span><span class="sxs-lookup"><span data-stu-id="6de94-136">If you specify **/dd-** or omit the switch, VSDBCMD will generate a deployment script only.</span></span>

> [!NOTE]
> <span data-ttu-id="6de94-137">Pokud nasazujete .deploymanifest souboru spíše než soubor .dbschema, chování **/dd** přepínač je mnohem složitější.</span><span class="sxs-lookup"><span data-stu-id="6de94-137">If you're deploying a .deploymanifest file rather than a .dbschema file, the behavior of the **/dd** switch is a lot more complicated.</span></span> <span data-ttu-id="6de94-138">V podstatě VSDBCMD bude ignorovat hodnoty **/dd** Pokud .deploymanifest soubor obsahuje, přejděte **DeployToDatabase** element s hodnotou **True**.</span><span class="sxs-lookup"><span data-stu-id="6de94-138">Essentially, VSDBCMD will ignore the value of the **/dd** switch if the .deploymanifest file includes a **DeployToDatabase** element with a value of **True**.</span></span> <span data-ttu-id="6de94-139">[Nasazení projektu databáze](../web-deployment-in-the-enterprise/deploying-database-projects.md) popisuje toto chování v plném rozsahu.</span><span class="sxs-lookup"><span data-stu-id="6de94-139">[Deploying Database Projects](../web-deployment-in-the-enterprise/deploying-database-projects.md) describes this behavior in full.</span></span>


<span data-ttu-id="6de94-140">Například chcete vygenerovat skript pro nasazení **ContactManager** databáze bez skutečného nasazení databáze VSDBCMD příkazu by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="6de94-140">For example, to generate a deployment script for the **ContactManager** database without actually deploying the database, your VSDBCMD command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]


<span data-ttu-id="6de94-141">VSDBCMD je nástroj pro nasazení rozdílovými a jako takový nasazovací skript generuje dynamicky tak, aby obsahovala všechny příkazy SQL potřebnými k aktualizaci aktuální databázi, pokud existuje, zadané schéma.</span><span class="sxs-lookup"><span data-stu-id="6de94-141">VSDBCMD is a differential database deployment tool, and as such the deployment script is dynamically generated to contain all the SQL commands necessary to update the current database, if one exists, to the specified schema.</span></span> <span data-ttu-id="6de94-142">Kontrola skriptu nasazení je užitečný způsob, jak určit, jaký vliv má vaše nasazení bude mít v aktuální databázi a data, která ho obsahuje.</span><span class="sxs-lookup"><span data-stu-id="6de94-142">Reviewing the deployment script is a useful way to determine what impact your deployment will have on the current database and the data it contains.</span></span> <span data-ttu-id="6de94-143">Můžete například chtít určit:</span><span class="sxs-lookup"><span data-stu-id="6de94-143">For example, you might want to determine:</span></span>

- <span data-ttu-id="6de94-144">Určuje, zda se odeberou veškeré stávající tabulky a určuje, zda povede ke ztrátě dat.</span><span class="sxs-lookup"><span data-stu-id="6de94-144">Whether any existing tables will be removed, and whether that will result in data loss.</span></span>
- <span data-ttu-id="6de94-145">Určuje, zda pořadí operací s sebou nese riziko ztráty dat, například pokud rozdělení nebo sloučení tabulky.</span><span class="sxs-lookup"><span data-stu-id="6de94-145">Whether the order of operations carries a risk of data loss, for example, if you're splitting or merging tables.</span></span>

<span data-ttu-id="6de94-146">Pokud jste spokojení s skript nasazení, můžete opakovat VSDBCMD s **/dd+** příznak provést změny.</span><span class="sxs-lookup"><span data-stu-id="6de94-146">If you're happy with the deployment script, you can repeat the VSDBCMD with a **/dd+** flag to make the changes.</span></span> <span data-ttu-id="6de94-147">Případně můžete upravit skript nasazení podle svých požadavků a pak ji spustit ručně na databázovém serveru.</span><span class="sxs-lookup"><span data-stu-id="6de94-147">Alternatively, you can edit the deployment script to meet your requirements and then execute it manually on the database server.</span></span>

## <a name="integrating-what-if-functionality-into-custom-project-files"></a><span data-ttu-id="6de94-148">Integrace "What If" funkce do vlastní projektových souborů</span><span class="sxs-lookup"><span data-stu-id="6de94-148">Integrating "What If" Functionality into Custom Project Files</span></span>

<span data-ttu-id="6de94-149">Ve složitějších scénářích nasazení, budete chtít použít vlastní soubor projektu Microsoft Build Engine (MSBuild) k zapouzdření svoji logiku sestavení a nasazení, jak je popsáno v [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span><span class="sxs-lookup"><span data-stu-id="6de94-149">In more complex deployment scenarios, you'll want to use a custom Microsoft Build Engine (MSBuild) project file to encapsulate your build and deployment logic, as described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span></span> <span data-ttu-id="6de94-150">Například v [Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) ukázkové řešení, *Publish.proj* souboru:</span><span class="sxs-lookup"><span data-stu-id="6de94-150">For example, in the [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) sample solution, the *Publish.proj* file:</span></span>

- <span data-ttu-id="6de94-151">Sestavení řešení.</span><span class="sxs-lookup"><span data-stu-id="6de94-151">Builds the solution.</span></span>
- <span data-ttu-id="6de94-152">Balení a nasazení aplikace ContactManager.Mvc pomocí nasazení webu.</span><span class="sxs-lookup"><span data-stu-id="6de94-152">Uses Web Deploy to package and deploy the ContactManager.Mvc application.</span></span>
- <span data-ttu-id="6de94-153">Balení a nasazení aplikace ContactManager.Service pomocí nasazení webu.</span><span class="sxs-lookup"><span data-stu-id="6de94-153">Uses Web Deploy to package and deploy the ContactManager.Service application.</span></span>
- <span data-ttu-id="6de94-154">Nasadí **ContactManager** databáze.</span><span class="sxs-lookup"><span data-stu-id="6de94-154">Deploys the **ContactManager** database.</span></span>

<span data-ttu-id="6de94-155">Při integraci nasazení více webových balíčků a/nebo databází do procesu krokování tímto způsobem můžete také možnost provedení celé nasazení v režimu "what if".</span><span class="sxs-lookup"><span data-stu-id="6de94-155">When you integrate the deployment of multiple web packages and/or databases into a single-step process in this way, you may also want the option of performing the entire deployment in a "what if" mode.</span></span>

<span data-ttu-id="6de94-156">*Publish.proj* soubor ukazuje, jak to udělat.</span><span class="sxs-lookup"><span data-stu-id="6de94-156">The *Publish.proj* file demonstrates how you can do this.</span></span> <span data-ttu-id="6de94-157">Nejprve je potřeba vytvořit vlastnost pro uložení hodnoty "what if":</span><span class="sxs-lookup"><span data-stu-id="6de94-157">First, you need to create a property to store the "what if" value:</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]


<span data-ttu-id="6de94-158">V tomto případě vytvoříte vlastnost s názvem **WhatIf** s výchozí hodnotou **false**.</span><span class="sxs-lookup"><span data-stu-id="6de94-158">In this case, you've created a property named **WhatIf** with a default value of **false**.</span></span> <span data-ttu-id="6de94-159">Uživatelé mohou přepsat tuto hodnotu tak, že nastavíte vlastnost **true** v parametr příkazového řádku, jako je ukážeme za chvíli.</span><span class="sxs-lookup"><span data-stu-id="6de94-159">Users can override this value by setting the property to **true** in a command-line parameter, as you'll see shortly.</span></span>

<span data-ttu-id="6de94-160">Další fází je parametrizovat Web Deploy a VSDBCMD příkazy, aby odrážely příznaků **WhatIf** hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="6de94-160">The next stage is to parameterize any Web Deploy and VSDBCMD commands so that the flags reflect the **WhatIf** property value.</span></span> <span data-ttu-id="6de94-161">Například další cíle (na základě *Publish.proj* souboru a zjednodušená čínština) běží *. deploy.cmd* uvést pro nasazení webového balíčku.</span><span class="sxs-lookup"><span data-stu-id="6de94-161">For example, the next target (taken from the *Publish.proj* file and simplified) runs the *.deploy.cmd* file to deploy a web package.</span></span> <span data-ttu-id="6de94-162">Ve výchozím nastavení, příkaz zahrnuje **/Y** přepínače ("Ano" nebo režim aktualizace).</span><span class="sxs-lookup"><span data-stu-id="6de94-162">By default, the command includes a **/Y** switch ("yes," or update mode).</span></span> <span data-ttu-id="6de94-163">Pokud **WhatIf** je nastavena na **true**, to je nahrazena **/T** přepínače (zkušební verze nebo režim "what if").</span><span class="sxs-lookup"><span data-stu-id="6de94-163">If **WhatIf** is set to **true**, this is replaced by a **/T** switch (trial, or "what if" mode).</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]


<span data-ttu-id="6de94-164">Další cílový obdobně, používá nástroj VSDBCMD nasazení databáze.</span><span class="sxs-lookup"><span data-stu-id="6de94-164">Similarly, the next target uses the VSDBCMD utility to deploy a database.</span></span> <span data-ttu-id="6de94-165">Ve výchozím nastavení **/dd** přepínač není zahrnutý.</span><span class="sxs-lookup"><span data-stu-id="6de94-165">By default, a **/dd** switch is not included.</span></span> <span data-ttu-id="6de94-166">To znamená, že VSDBCMD bude generovat skript nasazení, ale nenasadí databáze&#x2014;jinými slovy, "what if" scénáři.</span><span class="sxs-lookup"><span data-stu-id="6de94-166">This means that VSDBCMD will generate a deployment script but will not deploy the database&#x2014;in other words, a "what if" scenario.</span></span> <span data-ttu-id="6de94-167">Pokud **WhatIf** vlastnost není nastavena na **true**, **/dd** přidán přepínač a VSDBCMD se nasazení databáze.</span><span class="sxs-lookup"><span data-stu-id="6de94-167">If the **WhatIf** property is not set to **true**, a **/dd** switch is added and VSDBCMD will deploy the database.</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]


<span data-ttu-id="6de94-168">Můžete použít stejný přístup k parametrizaci všechny relevantní příkazy v souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="6de94-168">You can use the same approach to parameterize all the relevant commands in your project file.</span></span> <span data-ttu-id="6de94-169">Pokud chcete spustit nasazení "what if", můžete pak stačí zadat **WhatIf** hodnota vlastnosti z příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="6de94-169">When you want to run a "what if" deployment, you can then simply provide a **WhatIf** property value from the command line:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]


<span data-ttu-id="6de94-170">Tímto způsobem můžete spustit nasazení "what if" pro všechny součásti projektu v jediném kroku.</span><span class="sxs-lookup"><span data-stu-id="6de94-170">In this way, you can run a "what if" deployment for all your project components in a single step.</span></span>

## <a name="conclusion"></a><span data-ttu-id="6de94-171">Závěr</span><span class="sxs-lookup"><span data-stu-id="6de94-171">Conclusion</span></span>

<span data-ttu-id="6de94-172">Toto téma popisuje, jak spustit "what if" nasazení pomocí nasazení webu, VSDBCMD a MSBuild.</span><span class="sxs-lookup"><span data-stu-id="6de94-172">This topic described how to run "what if" deployments using Web Deploy, VSDBCMD, and MSBuild.</span></span> <span data-ttu-id="6de94-173">Nasazení "what if" umožňuje vyhodnotit její dopad navrhované nasazení před provedením jakýchkoli změn ve skutečnosti na cílovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="6de94-173">A "what if" deployment lets you evaluate the impact of a proposed deployment before you actually make any changes to the destination environment.</span></span>

## <a name="further-reading"></a><span data-ttu-id="6de94-174">Další čtení</span><span class="sxs-lookup"><span data-stu-id="6de94-174">Further Reading</span></span>

<span data-ttu-id="6de94-175">Další informace o nasazení webu syntaxe příkazového řádku najdete v tématu [nastavení operace nasazení webu](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="6de94-175">For more information on Web Deploy command-line syntax, see [Web Deploy Operation Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span></span> <span data-ttu-id="6de94-176">Informace o možnostech příkazového řádku při použití *. deploy.cmd* souborů naleznete v tématu [postupy: instalace nasazení balíčku pomocí souboru deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx).</span><span class="sxs-lookup"><span data-stu-id="6de94-176">For guidance on command-line options when you use the *.deploy.cmd* file, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span> <span data-ttu-id="6de94-177">Informace o syntaxi příkazového řádku VSDBCMD najdete v tématu [Reference k příkazovému řádku pro VSDBCMD. Soubor EXE (nasazení a Import schématu)](https://msdn.microsoft.com/library/dd193283.aspx).</span><span class="sxs-lookup"><span data-stu-id="6de94-177">For guidance on VSDBCMD command-line syntax, see [Command-Line Reference for VSDBCMD.EXE (Deployment and Schema Import)](https://msdn.microsoft.com/library/dd193283.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6de94-178">[Předchozí](advanced-enterprise-web-deployment.md)
> [další](customizing-database-deployments-for-multiple-environments.md)</span><span class="sxs-lookup"><span data-stu-id="6de94-178">[Previous](advanced-enterprise-web-deployment.md)
[Next](customizing-database-deployments-for-multiple-environments.md)</span></span>
