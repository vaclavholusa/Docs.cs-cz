---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-vb
title: Ověřování uživatelů pomocí ověřování (VB) formulářů | Dokumentace Microsoftu
author: microsoft
description: Zjistěte, jak použít atribut [Authorize] heslo chránit konkrétní stránky v aplikaci MVC. Zjistíte, jak používat správu webu příliš...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 4341f5b1-6fe5-44c5-8b8a-18fa84f80177
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: af91ae24cae505125dc237adfaa11b0ea4d60922
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752723"
---
<a name="authenticating-users-with-forms-authentication-vb"></a><span data-ttu-id="0a619-104">Ověřování uživatelů pomocí ověřování pomocí formulářů (VB)</span><span class="sxs-lookup"><span data-stu-id="0a619-104">Authenticating Users with Forms Authentication (VB)</span></span>
====================
<span data-ttu-id="0a619-105">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="0a619-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="0a619-106">Zjistěte, jak použít atribut [Authorize] heslo chránit konkrétní stránky v aplikaci MVC.</span><span class="sxs-lookup"><span data-stu-id="0a619-106">Learn how to use the [Authorize] attribute to password protect particular pages in your MVC application.</span></span> <span data-ttu-id="0a619-107">Zjistíte, jak používat nástroj Správa webu k vytváření a správě uživatelů a rolí.</span><span class="sxs-lookup"><span data-stu-id="0a619-107">You learn how to use the Web Site Administration Tool to create and manage users and roles.</span></span> <span data-ttu-id="0a619-108">Také se dozvíte, jak nakonfigurovat ukládat informace o účtu a role uživatele.</span><span class="sxs-lookup"><span data-stu-id="0a619-108">You also learn how to configure where user account and role information is stored.</span></span>


<span data-ttu-id="0a619-109">Cílem tohoto kurzu je vysvětlují, jak můžete pomocí formulářů ověřování hesla chránit zobrazení v aplikacích ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="0a619-109">The goal of this tutorial is to explain how you can use Forms authentication to password protect the views in your ASP.NET MVC applications.</span></span> <span data-ttu-id="0a619-110">Zjistíte, jak pomocí nástroje pro správu webu vytvoření uživatelů a rolí.</span><span class="sxs-lookup"><span data-stu-id="0a619-110">You learn how to use the Web Site Administration Tool to create users and roles.</span></span> <span data-ttu-id="0a619-111">Také se dozvíte, jak zabránit neoprávněným uživatelům vyvolání akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="0a619-111">You also learn how to prevent unauthorized users from invoking controller actions.</span></span> <span data-ttu-id="0a619-112">Nakonec se dozvíte, jak nakonfigurovat, kde jsou uložená uživatelská jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="0a619-112">Finally, you learn how to configure where user names and passwords are stored.</span></span>

#### <a name="using-the-web-site-administration-tool"></a><span data-ttu-id="0a619-113">Pomocí nástroje pro správu webu</span><span class="sxs-lookup"><span data-stu-id="0a619-113">Using the Web Site Administration Tool</span></span>

<span data-ttu-id="0a619-114">Před tím, než cokoli jiného, jsme měli spustit tak, že vytvoříte několik uživatelů a rolí.</span><span class="sxs-lookup"><span data-stu-id="0a619-114">Before we do anything else, we should start by creating some users and roles.</span></span> <span data-ttu-id="0a619-115">Nejjednodušší způsob, jak vytvořit nové uživatele a role je využít Visual Studio 2008 webový server nástroje pro správu.</span><span class="sxs-lookup"><span data-stu-id="0a619-115">The easiest way to create new users and roles is to take advantage of the Visual Studio 2008 Web Site Administration Tool.</span></span> <span data-ttu-id="0a619-116">Tento nástroj můžete spustit tak, že vyberete možnost nabídky **projektu, konfigurace ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="0a619-116">You can launch this tool by selecting the menu option **Project, ASP.NET Configuration**.</span></span> <span data-ttu-id="0a619-117">Alternativně můžete spustit nástroj Správa webu kliknutím (poněkud scary) ikonu kladivo narazili na světě, který se zobrazí v horní části okna Průzkumníka řešení (viz obrázek 1).</span><span class="sxs-lookup"><span data-stu-id="0a619-117">Alternatively, you can launch the Web Site Administration Tool by clicking the (somewhat scary) icon of the hammer hitting the world that appears at the top of the Solution Explorer window (see Figure 1).</span></span>

<span data-ttu-id="0a619-118">**Obrázek 1 – spouštění nástroje pro správu webu**</span><span class="sxs-lookup"><span data-stu-id="0a619-118">**Figure 1 – Launching the Web Site Administration Tool**</span></span>

![clip_image002 [4]](authenticating-users-with-forms-authentication-vb/_static/image1.jpg)

<span data-ttu-id="0a619-120">V rámci nástroje pro správu webu můžete vytvořit nové uživatele a role tak, že vyberete kartu zabezpečení. Klikněte na tlačítko **vytvořit uživatele** odkaz pro vytvoření nového uživatele s názvem Stephen (viz obrázek 2).</span><span class="sxs-lookup"><span data-stu-id="0a619-120">Within the Web Site Administration Tool, you create new users and roles by selecting the Security tab. Click the **Create user** link to create a new user named Stephen (see Figure 2).</span></span> <span data-ttu-id="0a619-121">Stephen uživateli poskytnout žádné heslo, které chcete (například *tajný klíč*).</span><span class="sxs-lookup"><span data-stu-id="0a619-121">Provide the Stephen user with any password that you want (for example, *secret*).</span></span>

<span data-ttu-id="0a619-122">**Obrázek 2 – Vytvoření nového uživatele**</span><span class="sxs-lookup"><span data-stu-id="0a619-122">**Figure 2 – Creating a new user**</span></span>

![clip_image004 [4]](authenticating-users-with-forms-authentication-vb/_static/image2.jpg)

<span data-ttu-id="0a619-124">Vytvoření nové role první povolení role a definováním jedné či několika rolí.</span><span class="sxs-lookup"><span data-stu-id="0a619-124">You create new roles by first enabling roles and defining one or more roles.</span></span> <span data-ttu-id="0a619-125">Povolit role kliknutím **povolit role** odkaz.</span><span class="sxs-lookup"><span data-stu-id="0a619-125">Enable roles by clicking the **Enable roles** link.</span></span> <span data-ttu-id="0a619-126">Dále vytvořte roli s názvem *správci* kliknutím **vytvořit nebo spravovat role** propojení (viz obrázek 3).</span><span class="sxs-lookup"><span data-stu-id="0a619-126">Next, create a role named *Administrators* by clicking the **Create or Manage roles** link (see Figure 3).</span></span>

<span data-ttu-id="0a619-127">**Obrázek 3: vytvoření nové role**</span><span class="sxs-lookup"><span data-stu-id="0a619-127">**Figure 3 – Creating a new role**</span></span>

![clip_image006 [4]](authenticating-users-with-forms-authentication-vb/_static/image3.jpg)

<span data-ttu-id="0a619-129">Nakonec vytvořte nového uživatele s názvem Sally a přidružte Sally s rolí správce v kliknutím na vytvořit odkaz a následným výběrem správci při vytváření Sally (viz obrázek 4).</span><span class="sxs-lookup"><span data-stu-id="0a619-129">Finally, create a new user named Sally and associate Sally with the Administrators role by clicking the Create User link and selecting Administrators when creating Sally (see Figure 4).</span></span>

<span data-ttu-id="0a619-130">**Obrázek 4 – přidání uživatele do role**</span><span class="sxs-lookup"><span data-stu-id="0a619-130">**Figure 4 – Adding a user to a role**</span></span>

![clip_image008[4]](authenticating-users-with-forms-authentication-vb/_static/image4.jpg)

<span data-ttu-id="0a619-132">Když všechny říká, že se provádí, byste měli mít dva nové uživatele s názvem Stephen a Sally.</span><span class="sxs-lookup"><span data-stu-id="0a619-132">When all is said and done, you should have two new users named Stephen and Sally.</span></span> <span data-ttu-id="0a619-133">Měli byste také novou roli s názvem Správci.</span><span class="sxs-lookup"><span data-stu-id="0a619-133">You should also have a new role named Administrators.</span></span> <span data-ttu-id="0a619-134">Jan je členem role Správci nástroje a Stephen není.</span><span class="sxs-lookup"><span data-stu-id="0a619-134">Sally is a member of the Administrators role and Stephen is not.</span></span>

#### <a name="requiring-authorization"></a><span data-ttu-id="0a619-135">Vyžadování ověření</span><span class="sxs-lookup"><span data-stu-id="0a619-135">Requiring Authorization</span></span>

<span data-ttu-id="0a619-136">Vyžadujete k ověření, než uživatel vyvolá akci kontroleru tak, že přidáte atribut [Authorize] na akci uživatele.</span><span class="sxs-lookup"><span data-stu-id="0a619-136">You can require a user to be authenticated before the user invokes a controller action by adding the [Authorize] attribute to the action.</span></span> <span data-ttu-id="0a619-137">Atribut [Authorize] můžete použít pro jednotlivé řadiče akce nebo můžete použít tento atribut do třídy celý kontroler.</span><span class="sxs-lookup"><span data-stu-id="0a619-137">You can apply the [Authorize] attribute to an individual controller action or you can apply this attribute to an entire controller class.</span></span>

<span data-ttu-id="0a619-138">Například kontroler v informacích 1 zpřístupňuje akci s názvem CompanySecrets().</span><span class="sxs-lookup"><span data-stu-id="0a619-138">For example, the controller in Listing 1 exposes an action named CompanySecrets().</span></span> <span data-ttu-id="0a619-139">Protože tato akce je upravený pomocí atributu [Authorize], tuto akci nelze vyvolat, pokud je ověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="0a619-139">Because this action is decorated with the [Authorize] attribute, this action cannot be invoked unless a user is authenticated.</span></span>

<span data-ttu-id="0a619-140">**Výpis 1 – Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="0a619-140">**Listing 1 – Controllers\HomeController.vb**</span></span>

[!code-vb[Main](authenticating-users-with-forms-authentication-vb/samples/sample1.vb)]

<span data-ttu-id="0a619-141">Vyvolat akci CompanySecrets() zadáním adresy URL/Home/CompanySecrets do adresního řádku prohlížeče a nejste ověřený uživatel, pak budete přesměrováni na zobrazení přihlášení automaticky (viz obrázek 5).</span><span class="sxs-lookup"><span data-stu-id="0a619-141">If you invoke the CompanySecrets() action by entering the URL /Home/CompanySecrets in the address bar of your browser, and you are not an authenticated user, then you will be redirected to the Login view automatically (see Figure 5).</span></span>

<span data-ttu-id="0a619-142">**Obrázek 5 – zobrazení přihlášení**</span><span class="sxs-lookup"><span data-stu-id="0a619-142">**Figure 5 – The Login view**</span></span>

![clip_image010 [4]](authenticating-users-with-forms-authentication-vb/_static/image5.jpg)

<span data-ttu-id="0a619-144">Pohled pro přihlášení můžete použít k zadání uživatelského jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="0a619-144">You can use the Login view to enter your user name and password.</span></span> <span data-ttu-id="0a619-145">Pokud si nejste registrovaní uživatelé pak můžete kliknout **zaregistrovat** odkaz přejděte do registru na zobrazení (viz obrázek 6).</span><span class="sxs-lookup"><span data-stu-id="0a619-145">If you are not a registered user then you can click the **register** link to navigate to the Register view (see Figure 6).</span></span> <span data-ttu-id="0a619-146">Zobrazení registru můžete použít k vytvoření nového uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="0a619-146">You can use the Register view to create a new user account.</span></span>

<span data-ttu-id="0a619-147">**Obrázek 6 – zobrazení registr**</span><span class="sxs-lookup"><span data-stu-id="0a619-147">**Figure 6 – The Register view**</span></span>

![clip_image012](authenticating-users-with-forms-authentication-vb/_static/image6.jpg)

<span data-ttu-id="0a619-149">Po úspěšném přihlášení, zobrazí se CompanySecrets zobrazení (viz obrázek 7).</span><span class="sxs-lookup"><span data-stu-id="0a619-149">After you successfully log in, you can see the CompanySecrets view (see Figure 7).</span></span> <span data-ttu-id="0a619-150">Ve výchozím nastavení budou nadále přihlášeni, dokud nezavřete okno prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="0a619-150">By default, you will continue to be logged in until you close your browser window.</span></span>

<span data-ttu-id="0a619-151">**Obrázek 7 – CompanySecrets zobrazení**</span><span class="sxs-lookup"><span data-stu-id="0a619-151">**Figure 7 – The CompanySecrets view**</span></span>

![clip_image014](authenticating-users-with-forms-authentication-vb/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a><span data-ttu-id="0a619-153">Ověřování pomocí uživatelského jména nebo Role uživatele</span><span class="sxs-lookup"><span data-stu-id="0a619-153">Authorizing by User Name or User Role</span></span>

<span data-ttu-id="0a619-154">Atribut [Authorize] můžete použít k omezení přístupu k akci kontroleru na konkrétní sadu uživatelů nebo konkrétní sadu rolí uživatelů.</span><span class="sxs-lookup"><span data-stu-id="0a619-154">You can use the [Authorize] attribute to restrict access to a controller action to a particular set of users or a particular set of user roles.</span></span> <span data-ttu-id="0a619-155">Například upravené kontroler Home výpis 2 obsahuje dvě nové akce s názvem StephenSecrets() a AdministratorSecrets().</span><span class="sxs-lookup"><span data-stu-id="0a619-155">For example, the modified Home controller in Listing 2 contains two new actions named StephenSecrets() and AdministratorSecrets().</span></span>

<span data-ttu-id="0a619-156">**Výpis 2 – Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="0a619-156">**Listing 2 – Controllers\HomeController.vb**</span></span>

[!code-vb[Main](authenticating-users-with-forms-authentication-vb/samples/sample2.vb)]

<span data-ttu-id="0a619-157">Akci StephenSecrets() lze vyvolat pouze uživatele se uživatelské jméno, Stephen.</span><span class="sxs-lookup"><span data-stu-id="0a619-157">Only a user with the user name Stephen can invoke the StephenSecrets() action.</span></span> <span data-ttu-id="0a619-158">Všichni ostatní uživatelé získat přesměrováni na zobrazení přihlášení.</span><span class="sxs-lookup"><span data-stu-id="0a619-158">All other users get redirected to the Login view.</span></span> <span data-ttu-id="0a619-159">Vlastnost uživatele přijímá čárkami oddělený seznam názvy uživatelských účtů.</span><span class="sxs-lookup"><span data-stu-id="0a619-159">The Users property accepts a comma separated list of user account names.</span></span>

<span data-ttu-id="0a619-160">Akci AdministratorSecrets() lze vyvolat pouze uživatelé v roli správce.</span><span class="sxs-lookup"><span data-stu-id="0a619-160">Only users in the Administrators role can invoke the AdministratorSecrets() action.</span></span> <span data-ttu-id="0a619-161">Jan je členem skupiny Administrators, Jana vyvolání akce AdministratorSecrets().</span><span class="sxs-lookup"><span data-stu-id="0a619-161">For example, because Sally is a member of the Administrators group, she can invoke the AdministratorSecrets() action.</span></span> <span data-ttu-id="0a619-162">Všichni ostatní uživatelé získat přesměrováni na zobrazení přihlášení.</span><span class="sxs-lookup"><span data-stu-id="0a619-162">All other users get redirected to the Login view.</span></span> <span data-ttu-id="0a619-163">Vlastnosti role přijímá čárkami oddělený seznam názvů rolí.</span><span class="sxs-lookup"><span data-stu-id="0a619-163">The Roles property accepts a comma separated list of role names.</span></span>

#### <a name="configuring-authentication"></a><span data-ttu-id="0a619-164">Konfigurace ověřování</span><span class="sxs-lookup"><span data-stu-id="0a619-164">Configuring Authentication</span></span>

<span data-ttu-id="0a619-165">V tomto okamžiku bude vás možná zajímat kam ukládají informace o uživatelském účtu a role.</span><span class="sxs-lookup"><span data-stu-id="0a619-165">At this point, you might be wondering where the user account and role information is being stored.</span></span> <span data-ttu-id="0a619-166">Ve výchozím nastavení, informace jsou uloženy v databázi SQL Express (RANU) s názvem ASPNETDB.mdf umístěný v aplikaci MVC aplikace\_složku Data.</span><span class="sxs-lookup"><span data-stu-id="0a619-166">By default, the information is stored in a (RANU) SQL Express database named ASPNETDB.mdf located in your MVC application's App\_Data folder.</span></span> <span data-ttu-id="0a619-167">Tato databáze je rozhraním ASP.NET automaticky generovat při spuštění pomocí členství.</span><span class="sxs-lookup"><span data-stu-id="0a619-167">This database is generated by the ASP.NET framework automatically when you start using membership.</span></span>

<span data-ttu-id="0a619-168">Chcete-li zobrazit ASPNETDB.mdf databáze v okně Průzkumník řešení, musíte nejprve vyberte možnost nabídky projekt, zobrazit všechny soubory.</span><span class="sxs-lookup"><span data-stu-id="0a619-168">In order to see the ASPNETDB.mdf database in the Solution Explorer window, you first need to select the menu option Project, Show All Files.</span></span>

<span data-ttu-id="0a619-169">Pomocí výchozí databáze SQL Express je v pořádku při vývoji aplikace.</span><span class="sxs-lookup"><span data-stu-id="0a619-169">Using the default SQL Express database is fine when developing an application.</span></span> <span data-ttu-id="0a619-170">Pravděpodobně ale nebudete chtít používat výchozí ASPNETDB.mdf databáze pro produkční aplikace.</span><span class="sxs-lookup"><span data-stu-id="0a619-170">Most likely, however, you won't want to use the default ASPNETDB.mdf database for a production application.</span></span> <span data-ttu-id="0a619-171">V takovém případě můžete změnit informace o uživatelském účtu se mají ukládat provedením následujících dvou kroků:</span><span class="sxs-lookup"><span data-stu-id="0a619-171">In that case, you can change where user account information is stored by completing the following two steps:</span></span>

1. <span data-ttu-id="0a619-172">Přidat objekty databáze aplikace služby k produkční databázi – změňte připojovací řetězec vaší aplikace tak, aby odkazoval na produkční databáze</span><span class="sxs-lookup"><span data-stu-id="0a619-172">Add the Application Services database objects to your production database - Change your application connection string to point to your production database</span></span>

<span data-ttu-id="0a619-173">Prvním krokem je přidání všechny nezbytné databázových objektů (tabulkám a uloženým procedurám) k produkční databázi.</span><span class="sxs-lookup"><span data-stu-id="0a619-173">The first step is to add all of the necessary database objects (tables and stored procedures) to your production database.</span></span> <span data-ttu-id="0a619-174">Nejjednodušší způsob, jak přidat tyto objekty do nové databáze je využít Průvodce instalací serveru SQL pro ASP.NET (viz obrázek 8).</span><span class="sxs-lookup"><span data-stu-id="0a619-174">The easiest way to add these objects to a new database is to take advantage of the ASP.NET SQL Server Setup Wizard (see Figure 8).</span></span> <span data-ttu-id="0a619-175">Tento nástroj můžete spustit tak, že otevřete příkazový řádek sady Visual Studio 2008 ze skupiny programů Microsoft Visual Studio 2008 a spuštěním následujícího příkazu z příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="0a619-175">You can launch this tool by opening the Visual Studio 2008 Command Prompt from the Microsoft Visual Studio 2008 program group and executing the following command from the command prompt:</span></span>

<span data-ttu-id="0a619-176">aspnet\_regsql</span><span class="sxs-lookup"><span data-stu-id="0a619-176">aspnet\_regsql</span></span>

<span data-ttu-id="0a619-177">**Obrázek 8 – Průvodce instalací serveru SQL technologie ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="0a619-177">**Figure 8 – The ASP.NET SQL Server Setup Wizard**</span></span>

![clip_image016](authenticating-users-with-forms-authentication-vb/_static/image8.jpg)

<span data-ttu-id="0a619-179">Průvodce instalací serveru SQL pro ASP.NET umožňuje vybrat databázi SQL serveru ve vaší síti a nainstalujte všechny databázové objekty nezbytné v aplikačních služeb technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0a619-179">The ASP.NET SQL Server Setup Wizard enables you to select a SQL Server database on your network and install all of the database objects required by the ASP.NET application services.</span></span> <span data-ttu-id="0a619-180">Databázový server není musí být umístěné na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="0a619-180">The database server is not required to be located on your local machine.</span></span>

> [!NOTE]
> <span data-ttu-id="0a619-181">Pokud už nechcete používat Průvodce instalací serveru SQL pro ASP.NET, můžete najít skripty SQL pro přidání objektů databáze aplikace služby v následující složce:</span><span class="sxs-lookup"><span data-stu-id="0a619-181">If you don't want to use the ASP.NET SQL Server Setup Wizard, then you can find SQL scripts for adding the application services database objects in the following folder:</span></span>
> 
> 
> <span data-ttu-id="0a619-182">C:\Windows\Microsoft.NET\Framework\v2.0.50727</span><span class="sxs-lookup"><span data-stu-id="0a619-182">C:\Windows\Microsoft.NET\Framework\v2.0.50727</span></span>


<span data-ttu-id="0a619-183">Po vytvoření nezbytné databázové objekty, budete muset upravit připojení k databázi použít v aplikaci MVC.</span><span class="sxs-lookup"><span data-stu-id="0a619-183">After you create the necessary database objects, you need to modify the database connection used by your MVC application.</span></span> <span data-ttu-id="0a619-184">Upravte ApplicationServices připojovací řetězec v souboru webové konfigurace (web.config) tak, aby odkazovala na produkční databázi.</span><span class="sxs-lookup"><span data-stu-id="0a619-184">Modify the ApplicationServices connection string in your web configuration (web.config) file so that it points to the production database.</span></span> <span data-ttu-id="0a619-185">Například upravené připojení v zobrazení 3, odkazuje na databázi s názvem MyProductionDB (původní ApplicationServices připojovacího řetězce byla změněna na komentář).</span><span class="sxs-lookup"><span data-stu-id="0a619-185">For example, the modified connection in Listing 3 points to a database named MyProductionDB (the original ApplicationServices connection string has been commented out).</span></span>

<span data-ttu-id="0a619-186">**Výpis 3 – Web.config**</span><span class="sxs-lookup"><span data-stu-id="0a619-186">**Listing 3 – Web.config**</span></span>

[!code-xml[Main](authenticating-users-with-forms-authentication-vb/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a><span data-ttu-id="0a619-187">Konfigurace oprávnění databáze</span><span class="sxs-lookup"><span data-stu-id="0a619-187">Configuring Database Permissions</span></span>

<span data-ttu-id="0a619-188">Pokud používáte integrované zabezpečení k připojení k databázi je potřeba přidat správné uživatelský účet Windows jako přihlašovací údaje k vaší databázi.</span><span class="sxs-lookup"><span data-stu-id="0a619-188">If you use Integrated Security to connect to your database then you will need to add the correct Windows user account as a login to your database.</span></span> <span data-ttu-id="0a619-189">Správný účet závisí na tom, jestli používáte serveru ASP.NET Development Server nebo Internetová informační služba jako webový server.</span><span class="sxs-lookup"><span data-stu-id="0a619-189">The correct account depends on whether you are using the ASP.NET Development Server or Internet Information Services as your web server.</span></span> <span data-ttu-id="0a619-190">Správný účet také závisí na operační systém.</span><span class="sxs-lookup"><span data-stu-id="0a619-190">The correct user account also depends on your operating system.</span></span>

<span data-ttu-id="0a619-191">Pokud používáte serveru ASP.NET Development Server (výchozí webový server používá sada Visual Studio) vaše aplikace spouští v kontextu uživatelského účtu Windows.</span><span class="sxs-lookup"><span data-stu-id="0a619-191">If you are using the ASP.NET Development Server (the default web server used by Visual Studio) then your application executes within the context of your Windows user account.</span></span> <span data-ttu-id="0a619-192">V takovém případě musíte přidat uživatelský účet Windows jako přihlašovací server databáze.</span><span class="sxs-lookup"><span data-stu-id="0a619-192">In that case, you need to add your Windows user account as a database server login.</span></span>

<span data-ttu-id="0a619-193">Případně pokud používáte Internetová informační služba musíte pro přidání účtu ASPNET nebo účet NT AUTORITY/NETWORK SERVICE jako přihlašovací server databáze.</span><span class="sxs-lookup"><span data-stu-id="0a619-193">Alternatively, if you are using Internet Information Services then you need to add either the ASPNET account or the NT AUTHORITY/NETWORK SERVICE account as a database server login.</span></span> <span data-ttu-id="0a619-194">Pokud používáte Windows XP, přidejte účet ASPNET jako přihlašovací údaje k vaší databázi.</span><span class="sxs-lookup"><span data-stu-id="0a619-194">If you are using Windows XP then add the ASPNET account as a login to your database.</span></span> <span data-ttu-id="0a619-195">Pokud používáte novější operační systém, jako je například Windows Vista nebo Windows Server 2008 přidejte účet NT AUTORITY/NETWORK SERVICE jako přihlášení k databázi.</span><span class="sxs-lookup"><span data-stu-id="0a619-195">If you are using a more recent operating system, such as Windows Vista or Windows Server 2008, then add the NT AUTHORITY/NETWORK SERVICE account as the database login.</span></span>

<span data-ttu-id="0a619-196">Nový uživatelský účet můžete přidat k vaší databázi pomocí aplikace Microsoft SQL Server Management Studio (viz obrázek 9).</span><span class="sxs-lookup"><span data-stu-id="0a619-196">You can add a new user account to your database by using Microsoft SQL Server Management Studio (see Figure 9).</span></span>

<span data-ttu-id="0a619-197">**Obrázek 9: vytvoření nové přihlašovací údaje serveru Microsoft SQL Server**</span><span class="sxs-lookup"><span data-stu-id="0a619-197">**Figure 9 – Creating a new Microsoft SQL Server login**</span></span>

![clip_image018](authenticating-users-with-forms-authentication-vb/_static/image9.jpg)

<span data-ttu-id="0a619-199">Po vytvoření vyžaduje přihlášení, budete muset namapovat přihlášení uživatele databáze s rolemi správnou databázi.</span><span class="sxs-lookup"><span data-stu-id="0a619-199">After you create the required login, you need to map the login to a database user with the right database roles.</span></span> <span data-ttu-id="0a619-200">Klikněte dvakrát na přihlášení a vyberte kartu u mapování uživatele. Vyberte jednu nebo více rolí databáze aplikace služby.</span><span class="sxs-lookup"><span data-stu-id="0a619-200">Double-click the login and select the User Mapping tab. Select one or more application services database roles.</span></span> <span data-ttu-id="0a619-201">Například, aby bylo možné ověřovat uživatele, je nutné povolit aspnet\_členství\_BasicAccess databázové role.</span><span class="sxs-lookup"><span data-stu-id="0a619-201">For example, in order to authenticate users, you need to enable the aspnet\_Membership\_BasicAccess database role.</span></span> <span data-ttu-id="0a619-202">Chcete-li vytvořit nové uživatele, je potřeba povolit aspnet\_členství\_FullAccess databázová role (viz obrázek 10).</span><span class="sxs-lookup"><span data-stu-id="0a619-202">In order to create new users, you need to enable the aspnet\_Membership\_FullAccess database role (see Figure 10).</span></span>

<span data-ttu-id="0a619-203">**Obrázek 10: Přidání aplikačních služeb databázové role**</span><span class="sxs-lookup"><span data-stu-id="0a619-203">**Figure 10 – Adding Application Services database roles**</span></span>

![clip_image020](authenticating-users-with-forms-authentication-vb/_static/image10.jpg)

#### <a name="summary"></a><span data-ttu-id="0a619-205">Souhrn</span><span class="sxs-lookup"><span data-stu-id="0a619-205">Summary</span></span>

<span data-ttu-id="0a619-206">V tomto kurzu jste zjistili, jak používat ověřování pomocí formulářů, při sestavování aplikace ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="0a619-206">In this tutorial, you learned how to use Forms authentication when building an ASP.NET MVC application.</span></span> <span data-ttu-id="0a619-207">Nejdřív jste zjistili, jak k vytvoření nových uživatelů a rolí s využitím nástroje pro správu webu.</span><span class="sxs-lookup"><span data-stu-id="0a619-207">First, you learned how to create new users and roles by taking advantage of the Web Site Administration Tool.</span></span> <span data-ttu-id="0a619-208">Dále jste zjistili, jak používat atribut [Authorize] k zabránit neoprávněným uživatelům v vyvolání akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="0a619-208">Next, you learned how to use the [Authorize] attribute to prevent unauthorized users from invoking controller actions.</span></span> <span data-ttu-id="0a619-209">Nakonec jste zjistili, jak nakonfigurovat svoji aplikaci MVC k uložení uživatele a informace o rolích v provozní databázi.</span><span class="sxs-lookup"><span data-stu-id="0a619-209">Finally, you learned how to configure your MVC application to store user and role information in a production database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0a619-210">[Předchozí](preventing-javascript-injection-attacks-cs.md)
> [další](authenticating-users-with-windows-authentication-vb.md)</span><span class="sxs-lookup"><span data-stu-id="0a619-210">[Previous](preventing-javascript-injection-attacks-cs.md)
[Next](authenticating-users-with-windows-authentication-vb.md)</span></span>
