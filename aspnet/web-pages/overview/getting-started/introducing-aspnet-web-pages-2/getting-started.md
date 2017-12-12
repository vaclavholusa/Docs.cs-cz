---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
title: "Představení technologie ASP.NET Web Pages – Začínáme | Microsoft Docs"
author: tfitzmac
description: "Služba WebMatrix se už doporučuje jako integrované vývojové prostředí pro ASP.NET Web Pages. Pomocí sady Visual Studio nebo Visual Studio Code. Tyto pokyny..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: a36d3bdf-ef1b-47a4-b932-3a0cf4cad716
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 615ddc31d0d857e5bf9a7f372b7efcf67d185905
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---getting-started"></a><span data-ttu-id="6de56-105">Představení technologie ASP.NET Web Pages – Začínáme</span><span class="sxs-lookup"><span data-stu-id="6de56-105">Introducing ASP.NET Web Pages - Getting Started</span></span>
====================
<span data-ttu-id="6de56-106">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="6de56-106">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> > [!NOTE] 
> > 
> > <span data-ttu-id="6de56-107">Služba WebMatrix se už doporučuje jako integrované vývojové prostředí pro ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="6de56-107">WebMatrix is no longer recommended as an integrated development environment for ASP.NET Web Pages.</span></span> <span data-ttu-id="6de56-108">Použití [sady Visual Studio](../program-asp-net-web-pages-in-visual-studio.md) nebo [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="6de56-108">Use [Visual Studio](../program-asp-net-web-pages-in-visual-studio.md) or [Visual Studio Code](https://code.visualstudio.com/).</span></span>
> 
> 
> <span data-ttu-id="6de56-109">Tento pokyny a aplikace získáte přehled o rozhraní ASP.NET Web Pages (verze 2 nebo novější) a syntaxi Razor, jednoduché rozhraní pro vytváření dynamických webů.</span><span class="sxs-lookup"><span data-stu-id="6de56-109">This guidance and application gives you an overview of ASP.NET Web Pages (version 2 or later) and Razor syntax, a lightweight framework for creating dynamic websites.</span></span> <span data-ttu-id="6de56-110">Také zavádí WebMatrix, nástroj pro vytváření stránek a webů.</span><span class="sxs-lookup"><span data-stu-id="6de56-110">It also introduces WebMatrix, a tool for creating pages and sites.</span></span>
> 
> <span data-ttu-id="6de56-111">**Úroveň**: nové rozhraní ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="6de56-111">**Level**: New to ASP.NET Web Pages.</span></span>  
> <span data-ttu-id="6de56-112">**Předpokládá, že znalosti**: HTML, základní kaskádových stylů (CSS).</span><span class="sxs-lookup"><span data-stu-id="6de56-112">**Skills assumed**: HTML, basic cascading style sheets (CSS).</span></span>
> 
> <span data-ttu-id="6de56-113">Co se dozvíte z prvního kurzu sady:</span><span class="sxs-lookup"><span data-stu-id="6de56-113">What you'll learn in the first tutorial of the set:</span></span>
> 
> - <span data-ttu-id="6de56-114">Jaké technologii ASP.NET Web Pages a co je pro.</span><span class="sxs-lookup"><span data-stu-id="6de56-114">What ASP.NET Web Pages technology is and what it's for.</span></span>
> - <span data-ttu-id="6de56-115">Co je služba WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="6de56-115">What WebMatrix is.</span></span>
> - <span data-ttu-id="6de56-116">Jak nainstalovat vše.</span><span class="sxs-lookup"><span data-stu-id="6de56-116">How to install everything.</span></span>
> - <span data-ttu-id="6de56-117">Postup vytvoření webu pomocí služby WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="6de56-117">How to create a website by using WebMatrix.</span></span>
>   
> 
> <span data-ttu-id="6de56-118">Funkce nebo technologie popsané:</span><span class="sxs-lookup"><span data-stu-id="6de56-118">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="6de56-119">Webové platformy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="6de56-119">Microsoft Web Platform Installer.</span></span>
> - <span data-ttu-id="6de56-120">Služba WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="6de56-120">WebMatrix.</span></span>
> - <span data-ttu-id="6de56-121">*.cshtml* stránky</span><span class="sxs-lookup"><span data-stu-id="6de56-121">*.cshtml* pages</span></span>
>   
> 
> <span data-ttu-id="6de56-122">Jan Pope byl původně zapsán v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="6de56-122">Mike Pope originally wrote this tutorial.</span></span> <span data-ttu-id="6de56-123">Tní FitzMacken aktualizaci pro sadu Microsoft WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="6de56-123">Tom FitzMacken updated it for Microsoft WebMatrix 3.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6de56-124">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="6de56-124">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="6de56-125">Rozhraní ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="6de56-125">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="6de56-126">Služba WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="6de56-126">WebMatrix 3</span></span>


## <a name="what-should-you-know"></a><span data-ttu-id="6de56-127">Co byste vědět?</span><span class="sxs-lookup"><span data-stu-id="6de56-127">What Should You Know?</span></span>

<span data-ttu-id="6de56-128">Nemůžeme se za předpokladu, že jste obeznámeni s:</span><span class="sxs-lookup"><span data-stu-id="6de56-128">We're assuming that you're familiar with:</span></span>

- <span data-ttu-id="6de56-129">**HTML**.</span><span class="sxs-lookup"><span data-stu-id="6de56-129">**HTML**.</span></span> <span data-ttu-id="6de56-130">Bez důkladné znalosti je povinný.</span><span class="sxs-lookup"><span data-stu-id="6de56-130">No in-depth expertise is required.</span></span> <span data-ttu-id="6de56-131">Objasníme nebude HTML, ale nemáte používáme nic komplexní.</span><span class="sxs-lookup"><span data-stu-id="6de56-131">We won't explain HTML, but we also don't use anything complex.</span></span> <span data-ttu-id="6de56-132">Poskytujeme odkazy na kurzy HTML, kde myslíme si, že jsou užitečná.</span><span class="sxs-lookup"><span data-stu-id="6de56-132">We'll provide links to HTML tutorials where we think they're useful.</span></span>
- <span data-ttu-id="6de56-133">**Kaskádových stylů (CSS)**.</span><span class="sxs-lookup"><span data-stu-id="6de56-133">**Cascading style sheets (CSS)**.</span></span> <span data-ttu-id="6de56-134">Stejné jako s jazykem HTML.</span><span class="sxs-lookup"><span data-stu-id="6de56-134">Same as with HTML.</span></span>
- <span data-ttu-id="6de56-135">**Databáze Basic nápady**.</span><span class="sxs-lookup"><span data-stu-id="6de56-135">**Basic database ideas**.</span></span> <span data-ttu-id="6de56-136">Pokud jste do tabulky se používá pro data a třídění a filtrování dat, který je úroveň znalosti jsme se obecně za předpokladu, že pro tento kurz sadu.</span><span class="sxs-lookup"><span data-stu-id="6de56-136">If you've used a spreadsheet for data and sorted and filtered the data, that's the level of expertise we're generally assuming for this tutorial set.</span></span>

<span data-ttu-id="6de56-137">Jsme se také za předpokladu, že byste chtěli dozvědět základní programování.</span><span class="sxs-lookup"><span data-stu-id="6de56-137">We're also assuming that you're interested in learning basic programming.</span></span> <span data-ttu-id="6de56-138">ASP.NET – webové stránky použijte programovací jazyk C#.</span><span class="sxs-lookup"><span data-stu-id="6de56-138">ASP.NET Web Pages use a programming language called C#.</span></span> <span data-ttu-id="6de56-139">Nemusíte mít žádné pozadí v programování, právě zájmu v ní.</span><span class="sxs-lookup"><span data-stu-id="6de56-139">You don't have to have any background in programming, just an interest in it.</span></span> <span data-ttu-id="6de56-140">Pokud jste někdy napsali žádné JavaScript na webové stránce před, máte k dispozici dostatek pozadí.</span><span class="sxs-lookup"><span data-stu-id="6de56-140">If you've ever written any JavaScript in a web page before, you've got plenty of background.</span></span>

<span data-ttu-id="6de56-141">Všimněte si, že pokud jste obeznámeni s programováním, můžete zjistit, že v tomto kurzu zpočátku nastavený přesune pomalu při můžeme uvést programátory urychlit.</span><span class="sxs-lookup"><span data-stu-id="6de56-141">Note that if you are familiar with programming, you might find that this tutorial set initially moves slowly while we bring new programmers up to speed.</span></span> <span data-ttu-id="6de56-142">Jak se nám získat po první několik kurzy, ale bude menší základní programování vysvětlit, a věcí přesune na rychlejší klip.</span><span class="sxs-lookup"><span data-stu-id="6de56-142">As we get past the first few tutorials, though, there will be less basic programming to explain and things will move along at a faster clip.</span></span>

## <a name="what-do-you-need"></a><span data-ttu-id="6de56-143">Co je potřeba?</span><span class="sxs-lookup"><span data-stu-id="6de56-143">What Do You Need?</span></span>

<span data-ttu-id="6de56-144">Tady je co budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="6de56-144">Here's what you'll need:</span></span>

- <span data-ttu-id="6de56-145">Počítač se systémem Windows 8, Windows 7, Windows Server 2008 nebo Windows Server 2012.</span><span class="sxs-lookup"><span data-stu-id="6de56-145">A computer that is running Windows 8, Windows 7, Windows Server 2008, or Windows Server 2012.</span></span>
- <span data-ttu-id="6de56-146">Aktivní internetové připojení.</span><span class="sxs-lookup"><span data-stu-id="6de56-146">A live internet connection.</span></span>
- <span data-ttu-id="6de56-147">Oprávnění správce (požadováno pro instalaci).</span><span class="sxs-lookup"><span data-stu-id="6de56-147">Administrator privileges (required for the installation process).</span></span>

## <a name="what-is-aspnet-web-pages"></a><span data-ttu-id="6de56-148">Co je ASP.NET – webové stránky?</span><span class="sxs-lookup"><span data-stu-id="6de56-148">What Is ASP.NET Web Pages?</span></span>

<span data-ttu-id="6de56-149">ASP.NET – webové stránky je rozhraní, které můžete vytvářet dynamické webové stránky.</span><span class="sxs-lookup"><span data-stu-id="6de56-149">ASP.NET Web Pages is a framework that you can use to create dynamic web pages.</span></span> <span data-ttu-id="6de56-150">Jednoduché webové stránky HTML je statický; jeho obsah je určen podle pevné jazyka HTML, který je na stránce.</span><span class="sxs-lookup"><span data-stu-id="6de56-150">A simple HTML web page is static; its content is determined by the fixed HTML markup that's in the page.</span></span> <span data-ttu-id="6de56-151">Dynamických stránek jako ty, které vytvoříte pomocí webových stránek ASP.NET umožňují vytvářet obsah stránky za chodu pomocí kódu.</span><span class="sxs-lookup"><span data-stu-id="6de56-151">Dynamic pages like those you create with ASP.NET Web Pages let you create the page content on the fly, by using code.</span></span>

<span data-ttu-id="6de56-152">Dynamických stránek umožňují udělat nejrůznějším věcí.</span><span class="sxs-lookup"><span data-stu-id="6de56-152">Dynamic pages let you do all sorts of things.</span></span> <span data-ttu-id="6de56-153">Můžete požádat uživatele o vstup pomocí formuláře a potom změnit na stránce zobrazuje nebo jak vypadá.</span><span class="sxs-lookup"><span data-stu-id="6de56-153">You can ask a user for input by using a form and then change what the page displays or how it looks.</span></span> <span data-ttu-id="6de56-154">Může trvat informace od uživatele, uložit do databáze a seznam později.</span><span class="sxs-lookup"><span data-stu-id="6de56-154">You can take information from a user, save it in a database, and then list it later.</span></span> <span data-ttu-id="6de56-155">Odesílat e-maily z vaší lokality.</span><span class="sxs-lookup"><span data-stu-id="6de56-155">You can send email from your site.</span></span> <span data-ttu-id="6de56-156">Můžete pracovat s jinými službami na webu (například služba mapování) a vytvořit stránky, které se integrují informace z těchto zdrojů.</span><span class="sxs-lookup"><span data-stu-id="6de56-156">You can interact with other services on the web (for example, a mapping service) and produce pages that integrate information from those sources.</span></span>

## <a name="what-is-webmatrix"></a><span data-ttu-id="6de56-157">Co je služba WebMatrix?</span><span class="sxs-lookup"><span data-stu-id="6de56-157">What is WebMatrix?</span></span>

<span data-ttu-id="6de56-158">WebMatrix je nástroj, který se integruje do editoru webové stránky, nástroj databáze, webový server pro testování stránky a funkce pro publikování vašeho webu k Internetu.</span><span class="sxs-lookup"><span data-stu-id="6de56-158">WebMatrix is a tool that integrates a web page editor, a database utility, a web server for testing pages, and features for publishing your website to the Internet.</span></span> <span data-ttu-id="6de56-159">Služba WebMatrix je zdarma a je snadno nainstalovat a snadno se používá.</span><span class="sxs-lookup"><span data-stu-id="6de56-159">WebMatrix is free, and it's easy to install and easy to use.</span></span> <span data-ttu-id="6de56-160">(Také funguje jen prostý stránky HTML, a také pro jiné technologie, jako je PHP.)</span><span class="sxs-lookup"><span data-stu-id="6de56-160">(It also works for just plain HTML pages, as well as for other technologies like PHP.)</span></span>

<span data-ttu-id="6de56-161">Ve skutečnosti není *mít* použití služby WebMatrix pro práci s ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="6de56-161">You don't actually *have* to use WebMatrix to work with ASP.NET Web Pages.</span></span> <span data-ttu-id="6de56-162">Můžete vytvářet stránky pomocí textového editoru, například a testovat stránky pomocí webového serveru, máte přístup.</span><span class="sxs-lookup"><span data-stu-id="6de56-162">You can create pages by using a text editor, for example, and test pages by using a web server that you have access to.</span></span> <span data-ttu-id="6de56-163">Však služba WebMatrix usnadňuje všechny velmi, tak tyto kurzy budou pomocí služby WebMatrix v průběhu.</span><span class="sxs-lookup"><span data-stu-id="6de56-163">However, WebMatrix makes it all very easy, so these tutorials will use WebMatrix throughout.</span></span>

## <a name="about-these-tutorials"></a><span data-ttu-id="6de56-164">O tyto kurzy</span><span class="sxs-lookup"><span data-stu-id="6de56-164">About These Tutorials</span></span>

<span data-ttu-id="6de56-165">Tento kurz sada je Úvod do používání rozhraní ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="6de56-165">This tutorial set is an introduction to how to use ASP.NET Web Pages.</span></span> <span data-ttu-id="6de56-166">Celkový počet v této úvodní kurz sady jsou 9 kurzy.</span><span class="sxs-lookup"><span data-stu-id="6de56-166">There are 9 tutorials total in this introductory tutorial set.</span></span> <span data-ttu-id="6de56-167">To je součástí série kurz sad cyklus od začátečníků webových stránek ASP.NET pro vytváření skutečné, profesionální weby.</span><span class="sxs-lookup"><span data-stu-id="6de56-167">It's part of a series of tutorial sets that takes you from ASP.NET Web Pages novice to creating real, professional-looking websites.</span></span>

<span data-ttu-id="6de56-168">Tato první kurz nastavit koncentráty ukazuje základní informace o tom, jak pracovat s ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="6de56-168">This first tutorial set concentrates on showing you the basics of how to work with ASP.NET Web Pages.</span></span> <span data-ttu-id="6de56-169">Když jste hotovi, můžete pracovat s další kurz sad, které se vyzvedávat kde ukončení této a který další podrobněji prozkoumat webové stránky.</span><span class="sxs-lookup"><span data-stu-id="6de56-169">When you're done, you can work with additional tutorial sets that pick up where this one ends and that explore Web Pages in more depth.</span></span>

<span data-ttu-id="6de56-170">Jsme úmyslně přejděte snadno podrobné vysvětlení.</span><span class="sxs-lookup"><span data-stu-id="6de56-170">We deliberately go easy on the in-depth explanations.</span></span> <span data-ttu-id="6de56-171">A vždy, když jsme zobrazit něco, pro tento kurz sadu jsme vždy vyberte způsob, jakým Věříme, že se nejjednodušší pochopit.</span><span class="sxs-lookup"><span data-stu-id="6de56-171">And whenever we show something, for this tutorial set we always choose the way that we think is easiest to understand.</span></span> <span data-ttu-id="6de56-172">Novější kurz sady přejděte do větší hloubky a zobrazit je efektivnější a flexibilnější přístupy (také další fun).</span><span class="sxs-lookup"><span data-stu-id="6de56-172">Later tutorial sets go into more depth and show you more efficient or more flexible approaches (also more fun).</span></span> <span data-ttu-id="6de56-173">Ale tyto kurzy vyžadují, abyste nejdřív pochopit základy.</span><span class="sxs-lookup"><span data-stu-id="6de56-173">But those tutorials require you to understand the basics first.</span></span>

<span data-ttu-id="6de56-174">Kurz sadu, do které jste začali se vztahuje na tyto funkce z webových stránek ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="6de56-174">The tutorial set you've just started covers these features of ASP.NET Web Pages:</span></span>

- <span data-ttu-id="6de56-175">Úvod a získávání všechno, co je nainstalována.</span><span class="sxs-lookup"><span data-stu-id="6de56-175">Introduction and getting everything installed.</span></span> <span data-ttu-id="6de56-176">(Který je v tomto kurzu, které při čtení.)</span><span class="sxs-lookup"><span data-stu-id="6de56-176">(That's in the tutorial you're reading.)</span></span>
- <span data-ttu-id="6de56-177">Základní informace o programování technologie ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="6de56-177">The basics of ASP.NET Web Pages programming.</span></span>
- <span data-ttu-id="6de56-178">Vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="6de56-178">Creating a database.</span></span>
- <span data-ttu-id="6de56-179">Vytváření a zpracování formulář vstupu uživatele.</span><span class="sxs-lookup"><span data-stu-id="6de56-179">Creating and processing a user input form.</span></span>
- <span data-ttu-id="6de56-180">Přidání, aktualizace a odstranění dat v databázi.</span><span class="sxs-lookup"><span data-stu-id="6de56-180">Adding, updating, and deleting data in the database.</span></span>

## <a name="what-will-you-create"></a><span data-ttu-id="6de56-181">Co se vytvoří?</span><span class="sxs-lookup"><span data-stu-id="6de56-181">What Will You Create?</span></span>

<span data-ttu-id="6de56-182">Nastavit tento kurz a následné ty, které jsou základem web, kde můžete vytvořit seznam filmy, které se vám líbí.</span><span class="sxs-lookup"><span data-stu-id="6de56-182">This tutorial set and subsequent ones revolve around a website where you can list movies that you like.</span></span> <span data-ttu-id="6de56-183">Budete moct zadat filmy, upravovat a jejich seznam.</span><span class="sxs-lookup"><span data-stu-id="6de56-183">You'll be able to enter movies, edit them, and list them.</span></span> <span data-ttu-id="6de56-184">Tady je několik stránek, které vytvoříte v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="6de56-184">Here are a couple of the pages you'll create in this tutorial set.</span></span> <span data-ttu-id="6de56-185">První z nich uvádí film výpis stránky, která vytvoříte:</span><span class="sxs-lookup"><span data-stu-id="6de56-185">The first one shows the movie listing page that you'll create:</span></span>

![Dokončila filmová aplikace zobrazuje v seznamu filmu](getting-started/_static/image1.png)

<span data-ttu-id="6de56-187">A zde je stránka, která umožňuje přidat nové informace film na váš web:</span><span class="sxs-lookup"><span data-stu-id="6de56-187">And here's the page that lets you add new movie information to your site:</span></span>

![Dokončení filmová aplikace stránkou Přidat filmu](getting-started/_static/image2.png)

<span data-ttu-id="6de56-189">Další kurz sady sestavení v tomto nastavte a přidejte další funkce, jako jsou nahrávání obrázky, když necháte osoby přihlásit, odesílání e-mailu a integrací s sociálních médií.</span><span class="sxs-lookup"><span data-stu-id="6de56-189">Subsequent tutorial sets build on this set and add more functionality, like uploading pictures, letting people log in, sending email, and integrating with social media.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="6de56-190">Tato aplikace spuštěné v Azure</span><span class="sxs-lookup"><span data-stu-id="6de56-190">See this App Running on Azure</span></span>

<span data-ttu-id="6de56-191">Chcete zobrazit dokončení web spuštěný jako živou webovou aplikaci?</span><span class="sxs-lookup"><span data-stu-id="6de56-191">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="6de56-192">Úplnou verzi aplikace můžete nasadit ke svému účtu Azure jednoduše kliknutím na následující tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6de56-192">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebPagesMovies)

<span data-ttu-id="6de56-193">Potřebujete účet Azure k nasazení tohoto řešení do Azure.</span><span class="sxs-lookup"><span data-stu-id="6de56-193">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="6de56-194">Pokud již účet nemáte, máte následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="6de56-194">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="6de56-195">[Zdarma otevřít účet Azure](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604) -získáte kredity, můžete použít k vyzkoušení placených služeb Azure a i po jejich použití až můžete účet ponechat a používat bezplatné služby Azure.</span><span class="sxs-lookup"><span data-stu-id="6de56-195">[Open an Azure account for free](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="6de56-196">[Aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -vaše předplatné MSDN vám dává kredity každý měsíc, které můžete použít pro placené služby Azure.</span><span class="sxs-lookup"><span data-stu-id="6de56-196">[Activate MSDN subscriber benefits](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="installing-everything"></a><span data-ttu-id="6de56-197">Instalace vše</span><span class="sxs-lookup"><span data-stu-id="6de56-197">Installing Everything</span></span>

<span data-ttu-id="6de56-198">Všechno, co můžete nainstalovat pomocí instalačního programu webové platformy společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="6de56-198">You can install everything by using the Web Platform Installer from Microsoft.</span></span> <span data-ttu-id="6de56-199">V důsledku toho nainstalovat Instalační program a použít jej k instalaci všechno ostatní.</span><span class="sxs-lookup"><span data-stu-id="6de56-199">In effect, you install the installer, and then use it to install everything else.</span></span>

<span data-ttu-id="6de56-200">Pomocí webových stránek, je nutné mít minimálně Windows XP s aktualizací SP3 nainstalována nebo Windows Server 2008 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="6de56-200">To use Web Pages, you have to be have at least Windows XP with SP3 installed, or Windows Server 2008 or later.</span></span>

<span data-ttu-id="6de56-201">Na [stránka Web Pages](../../../index.md) webu ASP.NET, klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="6de56-201">On the [Web Pages page](../../../index.md) of the ASP.NET website, click **Install**.</span></span>

![Zobrazuje webu ASP.NET Web &quot;instalace služby WebMatrix&quot; tlačítko](getting-started/_static/image3.png)

<span data-ttu-id="6de56-203">Jste vyzváni k přijmout licenční podmínky a prohlášení o ochraně osobních údajů před instalací služby WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="6de56-203">You are asked to accept the license terms and privacy statement before installing WebMatrix.</span></span>

![Přijměte termín k zahájení instalace](getting-started/_static/image4.png)

<span data-ttu-id="6de56-205">Klikněte na tlačítko **spustit** spusťte instalaci.</span><span class="sxs-lookup"><span data-stu-id="6de56-205">Click **Run** to start the installation.</span></span> <span data-ttu-id="6de56-206">(Pokud chcete uložit instalační program, klikněte na tlačítko **Uložit** a znovu spusťte instalační program ze složky, kam jste jej uložili.)</span><span class="sxs-lookup"><span data-stu-id="6de56-206">(If you want to save the installer, click **Save** and then run the installer from the folder where you saved it.)</span></span>

![](getting-started/_static/image5.png)

<span data-ttu-id="6de56-207">Instalace webové platformy se zobrazí, je připravena k instalaci služby WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="6de56-207">The Web Platform Installer appears, ready to install WebMatrix.</span></span> <span data-ttu-id="6de56-208">Klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="6de56-208">Click **Install**.</span></span>

![](getting-started/_static/image6.png)

<span data-ttu-id="6de56-209">Proces instalace obrázky na co se má nainstalovat do počítače a zahájí proces.</span><span class="sxs-lookup"><span data-stu-id="6de56-209">The installation process figures out what it has to install on your computer and starts the process.</span></span> <span data-ttu-id="6de56-210">V závislosti na tom, co přesně musí být nainstalovaná proces může trvat od chvíli několik minut.</span><span class="sxs-lookup"><span data-stu-id="6de56-210">Depending on what exactly has to be installed, the process can take anywhere from a few moments to several minutes.</span></span> <span data-ttu-id="6de56-211">Vyberte **souhlasím** přijmout licenční podmínky.</span><span class="sxs-lookup"><span data-stu-id="6de56-211">Select **I Accept** to accept the license terms.</span></span>

## <a name="hello-webmatrix"></a><span data-ttu-id="6de56-212">Hello, služba WebMatrix</span><span class="sxs-lookup"><span data-stu-id="6de56-212">Hello, WebMatrix</span></span>

<span data-ttu-id="6de56-213">Až skončíte, proces instalace můžete spustit službu WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="6de56-213">When it's done, the installation process can launch WebMatrix automatically.</span></span> <span data-ttu-id="6de56-214">Pokud tomu tak není, v systému Windows, z **spustit** nabídky, spusťte **Microsoft WebMatrix**.</span><span class="sxs-lookup"><span data-stu-id="6de56-214">If it doesn't, in Windows, from the **Start** menu, launch **Microsoft WebMatrix**.</span></span>

<span data-ttu-id="6de56-215">Při prvním spuštění služby WebMatrix, budete mít možnost se přihlásit do služby Microsoft Azure pomocí účtu Microsoft.</span><span class="sxs-lookup"><span data-stu-id="6de56-215">When you launch WebMatrix for the first time, you are given a chance to sign in to Microsoft Azure with your Microsoft account.</span></span> <span data-ttu-id="6de56-216">Po přihlášení, obdržíte 10 bezplatných webových aplikací prostřednictvím Azure.</span><span class="sxs-lookup"><span data-stu-id="6de56-216">By signing in, you will receive 10 free web apps through Azure.</span></span> <span data-ttu-id="6de56-217">Tyto aplikace bezplatných webových poskytnout vhodný způsob k testování aplikace.</span><span class="sxs-lookup"><span data-stu-id="6de56-217">These free web apps provide a convenient way to test your apps.</span></span> <span data-ttu-id="6de56-218">Pokud ještě nemáte účet Azure, ale máte předplatné MSDN, můžete [aktivovat výhody pro předplatné MSDN](https://www.windowsazure.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="6de56-218">If you don't already have an Azure account, but you do have an MSDN subscription, you can [activate your MSDN subscription benefits](https://www.windowsazure.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="6de56-219">Jinak můžete vytvořit Bezplatný zkušební účet si během několika minut.</span><span class="sxs-lookup"><span data-stu-id="6de56-219">Otherwise, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="6de56-220">Podrobnosti najdete v tématu [bezplatná zkušební verze Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="6de56-220">For details, see [Azure Free Trial](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>

<span data-ttu-id="6de56-221">Nemáte nyní přihlásit a pokračujte v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="6de56-221">You do not have to sign in right now to continue with this tutorial.</span></span> <span data-ttu-id="6de56-222">Není-li není nyní, bude stále mít možnost se přihlásit později.</span><span class="sxs-lookup"><span data-stu-id="6de56-222">If you do not sign in now, you will still have the option to sign in later.</span></span> <span data-ttu-id="6de56-223">Poslední [tématu](publishing.md) v tomto kurzu řady vysvětluje postup nasazení webu do Azure, proto by potřeba Přihlaste se k dokončení tohoto tématu.</span><span class="sxs-lookup"><span data-stu-id="6de56-223">The last [topic](publishing.md) in this tutorial series covers how to deploy your website to Azure; therefore, you would need to sign in to complete that topic.</span></span>

<span data-ttu-id="6de56-224">V tomto okamžiku buď přihlásit pomocí účtu Microsoft nebo vyberte **teď ne** v pravém dolním rohu.</span><span class="sxs-lookup"><span data-stu-id="6de56-224">At this point, either sign in with your Microsoft account or select **Not Now** in the lower right corner.</span></span>

![Přihlásit se](getting-started/_static/image7.png)

<span data-ttu-id="6de56-226">Pokud chcete začít, můžete vytvořit prázdný web a přidat stránku.</span><span class="sxs-lookup"><span data-stu-id="6de56-226">To begin, you'll create a blank website and add a page.</span></span> <span data-ttu-id="6de56-227">Novější kurzu v této sadě budete přehrát s jedním z předdefinovaných webu šablony.</span><span class="sxs-lookup"><span data-stu-id="6de56-227">In a later tutorial in this set you'll play with one of the built-in website templates.</span></span>

<span data-ttu-id="6de56-228">V okně start klikněte na **nový**.</span><span class="sxs-lookup"><span data-stu-id="6de56-228">In the start window, click **New**.</span></span>

![Služba WebMatrix úvodní obrazovky](getting-started/_static/image8.png)

<span data-ttu-id="6de56-230">Šablony jsou předem souborů a stránek pro různé typy webů.</span><span class="sxs-lookup"><span data-stu-id="6de56-230">Templates are prebuilt files and pages for different types of websites.</span></span> <span data-ttu-id="6de56-231">Pokud chcete zobrazit všechny šablony, které jsou k dispozici ve výchozím nastavení, vyberte možnost galerie šablon.</span><span class="sxs-lookup"><span data-stu-id="6de56-231">To see all of the templates that are available by default, select the Template Gallery option.</span></span>

![Vyberte šablonu Galerie](getting-started/_static/image9.png)

<span data-ttu-id="6de56-233">V **rychlý Start** vyberte **prázdný web** z **ASP.NET** skupiny a název nové lokality "WebPagesMovies".</span><span class="sxs-lookup"><span data-stu-id="6de56-233">In the **Quick Start** window, select **Empty Site** from the **ASP.NET** group and name the new site "WebPagesMovies".</span></span>

![Služba WebMatrix rychlý Start okno s vybraná šablona prázdný web](getting-started/_static/image10.png)

<span data-ttu-id="6de56-235">Klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="6de56-235">Click **Next**.</span></span>

<span data-ttu-id="6de56-236">Pokud jste se přihlásili k účtu Microsoft, budete mít možnost vytvoření webu v Azure.</span><span class="sxs-lookup"><span data-stu-id="6de56-236">If you have signed in to your Microsoft account, you will be given the opportunity to create the site on Azure.</span></span> <span data-ttu-id="6de56-237">Na základě názvu vašeho webu, výchozí název **WebPagesMovies.azurewebsites.net** navržený; však vykřičník označuje, že tento název není k dispozici v systému Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="6de56-237">Based on the name of your site, the default name of **WebPagesMovies.azurewebsites.net** is suggested; however, the exclamation point indicates that this name is not available on Windows Azure.</span></span> <span data-ttu-id="6de56-238">Pro jednoduchost, vyberte **přeskočit** obejít nyní vytváření webu v Azure.</span><span class="sxs-lookup"><span data-stu-id="6de56-238">For simplicity, select **Skip** to bypass creating the web site on Azure right now.</span></span> <span data-ttu-id="6de56-239">Dále v této série publikujeme webu do Azure.</span><span class="sxs-lookup"><span data-stu-id="6de56-239">Later in this series, we will publish the site to Azure.</span></span>

![Vytvoření azure lokality](getting-started/_static/image11.png)

<span data-ttu-id="6de56-241">Služba WebMatrix vytvoří a otevře web:</span><span class="sxs-lookup"><span data-stu-id="6de56-241">WebMatrix creates and opens the site:</span></span>

![Otevřete nový web WebPagesMovies ve službě WebMatrix](getting-started/_static/image12.png)

<span data-ttu-id="6de56-243">V horní části je panel nástrojů Rychlý přístup a pásu karet.</span><span class="sxs-lookup"><span data-stu-id="6de56-243">At the top, there's a Quick Access Toolbar and a ribbon.</span></span> <span data-ttu-id="6de56-244">V dolní části vlevo, uvidíte modulu pro výběr pracovního prostoru kde přepínat mezi úlohy (**lokality**, **soubory**, **databáze**, **sestavy**).</span><span class="sxs-lookup"><span data-stu-id="6de56-244">At the bottom left, you see the workspace selector where you switch between tasks (**Site**, **Files**, **Databases**, **Reports**).</span></span> <span data-ttu-id="6de56-245">Na pravé straně se podokno obsahu pro editor a pro sestavy.</span><span class="sxs-lookup"><span data-stu-id="6de56-245">On the right is the content pane for the editor and for reports.</span></span> <span data-ttu-id="6de56-246">A v dolní části se zobrazí příležitostně oznamovací pruh pro zprávy.</span><span class="sxs-lookup"><span data-stu-id="6de56-246">And across the bottom you'll occasionally see a notification bar for messages.</span></span>

<span data-ttu-id="6de56-247">Další informace o službě WebMatrix a jeho funkce při procházení tyto kurzy dozvíte.</span><span class="sxs-lookup"><span data-stu-id="6de56-247">You'll learn more about WebMatrix and its features as you go through these tutorials.</span></span>

## <a name="creating-a-web-page"></a><span data-ttu-id="6de56-248">Vytvoření webové stránky</span><span class="sxs-lookup"><span data-stu-id="6de56-248">Creating a Web Page</span></span>

<span data-ttu-id="6de56-249">Abyste se seznámili s WebMatrix a webové stránky ASP.NET, vytvoříte jednoduchou stránku.</span><span class="sxs-lookup"><span data-stu-id="6de56-249">To become familiar with WebMatrix and ASP.NET Web Pages, you'll create a simple page.</span></span>

<span data-ttu-id="6de56-250">V modulu pro výběr pracovního prostoru, vyberte **soubory** pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="6de56-250">In the workspace selector, select the **Files** workspace.</span></span> <span data-ttu-id="6de56-251">Tento pracovní prostor umožňuje pracovat se soubory a složky.</span><span class="sxs-lookup"><span data-stu-id="6de56-251">This workspace lets you work with files and folders.</span></span> <span data-ttu-id="6de56-252">V levém podokně ukazuje soubor strukturu serveru.</span><span class="sxs-lookup"><span data-stu-id="6de56-252">The left pane shows the file structure of your site.</span></span> <span data-ttu-id="6de56-253">Změny pásu karet zobrazíte úloh souvisejících s souboru.</span><span class="sxs-lookup"><span data-stu-id="6de56-253">The ribbon changes to show file-related tasks.</span></span>

![Pracovní prostor souboru ve službě WebMatrix](getting-started/_static/image13.png)

<span data-ttu-id="6de56-255">Na pásu karet klikněte na šipku v části **nový** a pak klikněte na **nový soubor**.</span><span class="sxs-lookup"><span data-stu-id="6de56-255">In the ribbon, click the arrow under **New** and then click **New File**.</span></span>

![Pomocí &quot;nový&quot; příkaz na pásu karet k vytvoření nového souboru](getting-started/_static/image14.png)

<span data-ttu-id="6de56-257">Služba WebMatrix zobrazí seznam typů souborů.</span><span class="sxs-lookup"><span data-stu-id="6de56-257">WebMatrix displays a list of file types.</span></span> <span data-ttu-id="6de56-258">Vyberte **CSHTML**a v **název** zadejte "HelloWorld".</span><span class="sxs-lookup"><span data-stu-id="6de56-258">Select **CSHTML**, and in the **Name** box, type "HelloWorld".</span></span> <span data-ttu-id="6de56-259">Na stránce CSHTML představuje stránku ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="6de56-259">A CSHTML page is an ASP.NET Web Pages page.</span></span>

![Vytváří se nová stránka CSHTML s názvem HelloWorld.cshtml](getting-started/_static/image15.png)

<span data-ttu-id="6de56-261">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="6de56-261">Click **OK**.</span></span>

<span data-ttu-id="6de56-262">Služba WebMatrix stránky vytvoří a otevře v editoru.</span><span class="sxs-lookup"><span data-stu-id="6de56-262">WebMatrix creates the page and opens it in the editor.</span></span>

![Novou stránku Hello World v editoru WebMatrix](getting-started/_static/image16.png)

<span data-ttu-id="6de56-264">Jak vidíte, tato stránka obsahuje většinou obyčejnou kódování HTML, s výjimkou blok v horní části, které vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="6de56-264">As you can see, the page contains mostly ordinary HTML markup, except for a block at the top that looks like this:</span></span>

[!code-cshtml[Main](getting-started/samples/sample1.cshtml)]

<span data-ttu-id="6de56-265">To je pro přidání kódu, jak se krátce zobrazí.</span><span class="sxs-lookup"><span data-stu-id="6de56-265">That's for adding code, as you'll see shortly.</span></span>

<span data-ttu-id="6de56-266">Všimněte si, že různé části stránky &mdash; názvy elementů, atributy a text a bloku v horní části – jsou všechny v různých barev.</span><span class="sxs-lookup"><span data-stu-id="6de56-266">Notice that the different parts of the page &mdash; the element names, attributes, and text, plus the block at the top — are all in different colors.</span></span> <span data-ttu-id="6de56-267">To se označuje jako *zvýraznění syntaxe*, a umožňuje jednodušší, aby vše jasné.</span><span class="sxs-lookup"><span data-stu-id="6de56-267">This is called *syntax highlighting*, and it makes it easier to keep everything clear.</span></span> <span data-ttu-id="6de56-268">Je jedna z funkcí, které usnadňuje práci s webovými stránkami ve službě WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="6de56-268">It's one of the features that makes it easy to work with web pages in WebMatrix.</span></span>

<span data-ttu-id="6de56-269">Přidat obsah `<head>` a `<body>` prvky, jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="6de56-269">Add content for the `<head>` and `<body>` elements like in the following example.</span></span> <span data-ttu-id="6de56-270">(Pokud chcete, můžete právě zkopírujte následující blok a celý existující stránku nahraďte tento kód.)</span><span class="sxs-lookup"><span data-stu-id="6de56-270">(If you want, you can just copy the following block and replace the entire existing page with this code.)</span></span>

[!code-cshtml[Main](getting-started/samples/sample2.cshtml)]

<span data-ttu-id="6de56-271">V panelu nástrojů Rychlý přístup nebo v **soubor** nabídky, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="6de56-271">In the Quick Access Toolbar or in the **File** menu, click **Save**.</span></span>

![Tlačítko Uložit ve službě WebMatrix nástrojů Rychlý přístup](getting-started/_static/image17.png)

## <a name="testing-the-page"></a><span data-ttu-id="6de56-273">Testování stránky</span><span class="sxs-lookup"><span data-stu-id="6de56-273">Testing the Page</span></span>

<span data-ttu-id="6de56-274">V **soubory** pracovního prostoru, klikněte pravým tlačítkem myši *HelloWorld.cshtml* a pak klikněte na tlačítko **spustit v prohlížeči**.</span><span class="sxs-lookup"><span data-stu-id="6de56-274">In the **Files** workspace, right-click the *HelloWorld.cshtml* page and then click **Launch in browser**.</span></span>

![Spuštění stránky pomocí služby WebMatrix pásu karet tlačítko Spustit](getting-started/_static/image18.png)

<span data-ttu-id="6de56-276">Služba WebMatrix spustí předdefinované webový server (IIS Express), který můžete použít k testování stránek ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="6de56-276">WebMatrix starts a built-in web server (IIS Express) that you can use to test pages on your computer.</span></span> <span data-ttu-id="6de56-277">(Bez služby IIS Express ve službě WebMatrix, museli byste publikovat na webový server stránku někde předtím, než může otestovat ji.) Stránce se zobrazí ve výchozím prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="6de56-277">(Without IIS Express in WebMatrix, you'd have to publish your page to a web server somewhere before you could test it.) The page is displayed in your default browser.</span></span>

![&quot;Hello World&quot; stránky, které jsou spuštěné v prohlížeči](getting-started/_static/image19.png)

<span data-ttu-id="6de56-279">Všimněte si, že při testování stránky ve službě WebMatrix, adresu URL v prohlížeči je podobný `http://localhost:33651/HelloWorld.cshtml.` název *localhost* odkazuje na místním serveru, což znamená, že stránce obsloužených webový server, který je ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="6de56-279">Notice that when you test a page in WebMatrix, the URL in the browser is something like `http://localhost:33651/HelloWorld.cshtml.` The name *localhost* refers to a local server, meaning that the page is served by a web server that's on your own computer.</span></span> <span data-ttu-id="6de56-280">Jak jsme uvedli, služba WebMatrix obsahuje program webového serveru s názvem služby IIS Express, která se spouští při spuštění stránky.</span><span class="sxs-lookup"><span data-stu-id="6de56-280">As noted, WebMatrix includes a web server program named IIS Express that runs when you launch a page.</span></span>

<span data-ttu-id="6de56-281">Počet po *localhost* (například *localhost:33651*) odkazuje *číslo portu* ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="6de56-281">The number after *localhost* (for example, *localhost:33651*) refers to a *port number* on your computer.</span></span> <span data-ttu-id="6de56-282">Toto je počet "kanál" používající službu IIS Express pro tento konkrétní web.</span><span class="sxs-lookup"><span data-stu-id="6de56-282">This is the number of the "channel" that IIS Express uses for this particular website.</span></span> <span data-ttu-id="6de56-283">Číslo portu je vybrán náhodně z rozsahu 1024 až 65536, při vytváření webu a je jiný pro každý web, který vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="6de56-283">The port number is selected at random from the range 1024 through 65536 when you create your site, and it's different for every site that you create.</span></span> <span data-ttu-id="6de56-284">(Při testování vlastního webu, číslo portu skoro určitě bude jinou hodnotu než 33561.) Když použijete jiný port pro každý web, IIS Express můžete ponechat přímých který stránek je rozhovoru s.</span><span class="sxs-lookup"><span data-stu-id="6de56-284">(When you test your own site, the port number will almost certainly be a different number than 33561.) By using a different port for each website, IIS Express can keep straight which of your sites it's talking to.</span></span>

<span data-ttu-id="6de56-285">Novější při publikování webu veřejné webový server, se již nezobrazují *localhost* v adrese URL.</span><span class="sxs-lookup"><span data-stu-id="6de56-285">Later when you publish your site to a public web server, you no longer see *localhost* in the URL.</span></span> <span data-ttu-id="6de56-286">V tomto bodě se zobrazí více klasickou adresu URL jako `http://myhostingsite/mywebsite/HelloWorld.cshtml` nebo bez ohledu na stránce.</span><span class="sxs-lookup"><span data-stu-id="6de56-286">At that point, you'll see a more typical URL like `http://myhostingsite/mywebsite/HelloWorld.cshtml` or whatever the page is.</span></span> <span data-ttu-id="6de56-287">Dozvíte více o publikování lokality později z této série kurzu.</span><span class="sxs-lookup"><span data-stu-id="6de56-287">You'll learn more about publishing a site later in this tutorial series.</span></span>

## <a name="adding-some-server-side-code"></a><span data-ttu-id="6de56-288">Přidání některé kódu na straně serveru</span><span class="sxs-lookup"><span data-stu-id="6de56-288">Adding Some Server-Side Code</span></span>

<span data-ttu-id="6de56-289">Zavřete prohlížeč a přejděte zpět na stránku ve službě WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="6de56-289">Close the browser and go back to the page in WebMatrix.</span></span>

<span data-ttu-id="6de56-290">Přidá řádek do bloku kódu tak, aby vypadal jako následující kód:</span><span class="sxs-lookup"><span data-stu-id="6de56-290">Add a line to the code block so that it looks like the following code:</span></span>

[!code-cshtml[Main](getting-started/samples/sample3.cshtml)]

<span data-ttu-id="6de56-291">Toto je jenom trocha kódu Razor.</span><span class="sxs-lookup"><span data-stu-id="6de56-291">This is a little bit of Razor code.</span></span> <span data-ttu-id="6de56-292">Je nejspíš jasné, že získá aktuální datum a čas a uloží tuto hodnotu do *proměnná* s názvem `currentDateTime`.</span><span class="sxs-lookup"><span data-stu-id="6de56-292">It's probably clear that it gets the current date and time and puts that value into a *variable* named `currentDateTime`.</span></span> <span data-ttu-id="6de56-293">Další informace o syntaxi Razor v dalším kurzu.</span><span class="sxs-lookup"><span data-stu-id="6de56-293">You'll read more about Razor syntax in the next tutorial.</span></span>

<span data-ttu-id="6de56-294">V těle stránce po `<p>Hello World!</p>` elementu, přidejte následující:</span><span class="sxs-lookup"><span data-stu-id="6de56-294">In the body of the page, after the `<p>Hello World!</p>` element, add the following:</span></span>

[!code-html[Main](getting-started/samples/sample4.html)]

<span data-ttu-id="6de56-295">Tento kód získá hodnotu, která vložíte do `currentDateTime` proměnné v horní části a vloží je do kódu stránky.</span><span class="sxs-lookup"><span data-stu-id="6de56-295">This code gets the value that you put into the `currentDateTime` variable at the top and inserts it into the markup of the page.</span></span> <span data-ttu-id="6de56-296">`@` Znak označí kód webové stránky ASP.NET na stránce.</span><span class="sxs-lookup"><span data-stu-id="6de56-296">The `@` character marks the ASP.NET Web Pages code in the page.</span></span>

<span data-ttu-id="6de56-297">Spuštění stránky znovu (WebMatrix uloží změny, můžete před spuštěním stránce).</span><span class="sxs-lookup"><span data-stu-id="6de56-297">Run the page again (WebMatrix saves the changes for you before it runs the page).</span></span> <span data-ttu-id="6de56-298">Tentokrát uvidíte datum a čas na stránce.</span><span class="sxs-lookup"><span data-stu-id="6de56-298">This time you see the date and time in the page.</span></span>

![&quot;Hello World&quot; spuštěná v prohlížeči s dynamicky generovaném času zobrazení stránky](getting-started/_static/image20.png)

<span data-ttu-id="6de56-300">Chvíli počkejte a pak aktualizujte stránku v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="6de56-300">Wait a few moments and then refresh the page in the browser.</span></span> <span data-ttu-id="6de56-301">Datum a čas zobrazení se aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="6de56-301">The date and time display is updated.</span></span>

<span data-ttu-id="6de56-302">V prohlížeči podívejte se na stránce zdroj.</span><span class="sxs-lookup"><span data-stu-id="6de56-302">In the browser, look at the page source.</span></span> <span data-ttu-id="6de56-303">To vypadá následující kód:</span><span class="sxs-lookup"><span data-stu-id="6de56-303">It looks like the following markup:</span></span>

[!code-html[Main](getting-started/samples/sample5.html)]

<span data-ttu-id="6de56-304">Všimněte si, že `@{ }` bloku v horní části nejsou.</span><span class="sxs-lookup"><span data-stu-id="6de56-304">Notice that the `@{ }` block at the top isn't there.</span></span> <span data-ttu-id="6de56-305">Také Všimněte si, že datum a čas zobrazení zobrazuje skutečné řetězec znaků (`1/18/2012 2:49:50 PM` nebo jakoukoli), nikoli `@currentDateTime` stejně, jako jste měli *.cshtml* stránky.</span><span class="sxs-lookup"><span data-stu-id="6de56-305">Also notice that the date and time display shows an actual string of characters (`1/18/2012 2:49:50 PM` or whatever), not `@currentDateTime` like you had in the *.cshtml* page.</span></span> <span data-ttu-id="6de56-306">Co se stalo, zde je, že při spuštění stránky ASP.NET zpracovány všechny kód (velmi málo v tomto případě), byla označena `@`.</span><span class="sxs-lookup"><span data-stu-id="6de56-306">What happened here is that when you ran the page, ASP.NET processed all the code (very little in this case) that was marked with `@`.</span></span> <span data-ttu-id="6de56-307">Kód vytvoří výstup a tento výstup byl vložen do stránky.</span><span class="sxs-lookup"><span data-stu-id="6de56-307">The code produces output, and that output was inserted into the page.</span></span>

## <a name="this-is-what-aspnet-web-pages-are-about"></a><span data-ttu-id="6de56-308">Toto je ASP.NET – webové stránky jsou o</span><span class="sxs-lookup"><span data-stu-id="6de56-308">This Is What ASP.NET Web Pages Are About</span></span>

<span data-ttu-id="6de56-309">Při čtení, rozhraní ASP.NET Web Pages vytváří dynamického webového obsahu, co jste se seznámili s tady je na nápad.</span><span class="sxs-lookup"><span data-stu-id="6de56-309">When you read that ASP.NET Web Pages produces dynamic web content, what you've seen here is the idea.</span></span> <span data-ttu-id="6de56-310">Stránka, kterou jste právě vytvořili obsahuje stejné jazyka HTML, který jste se seznámili s před.</span><span class="sxs-lookup"><span data-stu-id="6de56-310">The page you just created contains the same HTML markup that you've seen before.</span></span> <span data-ttu-id="6de56-311">Může také obsahovat kód, který může provádět nejrůznějším úlohy.</span><span class="sxs-lookup"><span data-stu-id="6de56-311">It can also contain code that can perform all sorts of tasks.</span></span> <span data-ttu-id="6de56-312">V tomto příkladu ho nebyla jednoduchý úkol získat aktuální datum a čas.</span><span class="sxs-lookup"><span data-stu-id="6de56-312">In this example, it did the trivial task of getting the current date and time.</span></span> <span data-ttu-id="6de56-313">Jak už jste viděli, můžete intersperse kódu HTML k vytváření výstupu na stránce.</span><span class="sxs-lookup"><span data-stu-id="6de56-313">As you saw, you can intersperse code with the HTML to produce output in the page.</span></span> <span data-ttu-id="6de56-314">Když někdo požadavky *.cshtml* stránku v prohlížeči, ASP.NET zpracovává stránky, když je stále v rukou webového serveru.</span><span class="sxs-lookup"><span data-stu-id="6de56-314">When someone requests a *.cshtml* page in the browser, ASP.NET processes the page while it's still in the hands of the web server.</span></span> <span data-ttu-id="6de56-315">ASP.NET vloží výstup kódu (pokud existuje) na stránce ve formátu HTML.</span><span class="sxs-lookup"><span data-stu-id="6de56-315">ASP.NET inserts the output of the code (if any) into the page as HTML.</span></span> <span data-ttu-id="6de56-316">Po dokončení zpracování kódu, ASP.NET výsledná stránka odešle do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="6de56-316">When the code processing is done, ASP.NET sends the resulting page to the browser.</span></span> <span data-ttu-id="6de56-317">Všechny prohlížeče někdy získá jsou ve formátu HTML.</span><span class="sxs-lookup"><span data-stu-id="6de56-317">All the browser ever gets is HTML.</span></span> <span data-ttu-id="6de56-318">Zde je diagram:</span><span class="sxs-lookup"><span data-stu-id="6de56-318">Here's a diagram:</span></span>

![Koncepční toku jak ASP.NET dynamicky vygeneruje HTML](getting-started/_static/image21.png)

<span data-ttu-id="6de56-320">Cílem je jednoduchý, ale řadu zajímavé úkolů, které může provádět kód a existuje mnoho způsobů zajímavé, ve kterých můžete dynamicky přidat obsah HTML na stránku.</span><span class="sxs-lookup"><span data-stu-id="6de56-320">The idea is simple, but there are many interesting tasks that the code can perform, and there are many interesting ways in which you can dynamically add HTML content to the page.</span></span> <span data-ttu-id="6de56-321">A ASP.NET *.cshtml* stránky, jako jakoukoli stránku HTML, můžou taky patřit kód, který běží v (kód JavaScript a jQuery) přímo z prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="6de56-321">And ASP.NET *.cshtml* pages, like any HTML page, can also include code that runs in the browser itself (JavaScript and jQuery code).</span></span> <span data-ttu-id="6de56-322">Budete prozkoumejte všechny tyto věci v této sadě kurz a následné ty.</span><span class="sxs-lookup"><span data-stu-id="6de56-322">You'll explore all of these things in this tutorial set and in subsequent ones.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="6de56-323">Objevuje další</span><span class="sxs-lookup"><span data-stu-id="6de56-323">Coming Up Next</span></span>

<span data-ttu-id="6de56-324">V dalším kurzu této série prozkoumejte další programování technologie ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="6de56-324">In the next tutorial in this series, you explore ASP.NET Web Pages programming a little more.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6de56-325">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="6de56-325">Additional Resources</span></span>

<span data-ttu-id="6de56-326">[Vytvoření webu ASP.NET od začátku](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch).</span><span class="sxs-lookup"><span data-stu-id="6de56-326">[Create an ASP.NET website from scratch](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch).</span></span> <span data-ttu-id="6de56-327">Kurz, který je konkrétně se jedná o pomocí služby WebMatrix (ne webové stránky ASP.NET).</span><span class="sxs-lookup"><span data-stu-id="6de56-327">This is a tutorial that's specifically about using WebMatrix (not ASP.NET Web Pages).</span></span> <span data-ttu-id="6de56-328">Přejde do trochu další podrobnosti o některých dalších funkcí služby WebMatrix, který nebude nabídneme v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="6de56-328">It goes into a little more detail about some of the additional features of WebMatrix that we won't cover in this tutorial set.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="6de56-329">Další</span><span class="sxs-lookup"><span data-stu-id="6de56-329">Next</span></span>](intro-to-web-pages-programming.md)
