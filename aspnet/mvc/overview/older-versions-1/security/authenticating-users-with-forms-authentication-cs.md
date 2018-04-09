---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
title: Ověřování uživatelů s formulářové ověřování (C#) | Microsoft Docs
author: microsoft
description: Naučte se používat atribut [autorizovat] heslo k ochraně konkrétní stránky v aplikaci MVC. Zjistíte, jak používat správu webu příliš...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 239fd3ca-5630-4b8d-bc4b-2f906b1d3504
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: e1def84bbf48847339e89b239b026d053640b935
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="authenticating-users-with-forms-authentication-c"></a><span data-ttu-id="6e232-104">Ověřování uživatelů pomocí ověřování pomocí formulářů (C#)</span><span class="sxs-lookup"><span data-stu-id="6e232-104">Authenticating Users with Forms Authentication (C#)</span></span>
====================
<span data-ttu-id="6e232-105">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="6e232-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="6e232-106">Naučte se používat atribut [autorizovat] heslo k ochraně konkrétní stránky v aplikaci MVC.</span><span class="sxs-lookup"><span data-stu-id="6e232-106">Learn how to use the [Authorize] attribute to password protect particular pages in your MVC application.</span></span> <span data-ttu-id="6e232-107">Zjistíte, jak používat nástroj pro správu webu k vytváření a správě uživatelů a rolí.</span><span class="sxs-lookup"><span data-stu-id="6e232-107">You learn how to use the Web Site Administration Tool to create and manage users and roles.</span></span> <span data-ttu-id="6e232-108">Také zjistíte, jak nakonfigurovat, se uloží informace o účtu a role uživatele.</span><span class="sxs-lookup"><span data-stu-id="6e232-108">You also learn how to configure where user account and role information is stored.</span></span>


<span data-ttu-id="6e232-109">Cílem tohoto kurzu je vysvětlují, jak můžete použít formuláře ověřování heslo k ochraně zobrazení v aplikacích ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="6e232-109">The goal of this tutorial is to explain how you can use Forms authentication to password protect the views in your ASP.NET MVC applications.</span></span> <span data-ttu-id="6e232-110">Zjistíte, jak používat nástroj pro správu webového serveru k vytvoření uživatelů a rolí.</span><span class="sxs-lookup"><span data-stu-id="6e232-110">You learn how to use the Web Site Administration Tool to create users and roles.</span></span> <span data-ttu-id="6e232-111">Také zjistíte, jak zabránit neoprávněným uživatelům v vyvolání akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="6e232-111">You also learn how to prevent unauthorized users from invoking controller actions.</span></span> <span data-ttu-id="6e232-112">Nakonec zjistíte, jak nakonfigurovat, kde jsou uložená uživatelská jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="6e232-112">Finally, you learn how to configure where user names and passwords are stored.</span></span>

#### <a name="using-the-web-site-administration-tool"></a><span data-ttu-id="6e232-113">Pomocí nástroje Správa webu</span><span class="sxs-lookup"><span data-stu-id="6e232-113">Using the Web Site Administration Tool</span></span>

<span data-ttu-id="6e232-114">Před provedeme cokoliv jiného, jsme měli začít vytváření některých uživatelů a rolí.</span><span class="sxs-lookup"><span data-stu-id="6e232-114">Before we do anything else, we should start by creating some users and roles.</span></span> <span data-ttu-id="6e232-115">Nejjednodušší způsob, jak vytvořit nové uživatele a role je využít nástroj pro správu Visual Studio 2008 webu.</span><span class="sxs-lookup"><span data-stu-id="6e232-115">The easiest way to create new users and roles is to take advantage of the Visual Studio 2008 Web Site Administration Tool.</span></span> <span data-ttu-id="6e232-116">Tento nástroj můžete spustit výběrem možnosti nabídky **projektu, konfigurace ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="6e232-116">You can launch this tool by selecting the menu option **Project, ASP.NET Configuration**.</span></span> <span data-ttu-id="6e232-117">Alternativně můžete spustit nástroj Správa webu kliknutím na ikonu (poněkud strach) v provede stiskne na světě, který se zobrazí v horní části okna Průzkumníka řešení (viz obrázek 1).</span><span class="sxs-lookup"><span data-stu-id="6e232-117">Alternatively, you can launch the Web Site Administration Tool by clicking the (somewhat scary) icon of the hammer hitting the world that appears at the top of the Solution Explorer window (see Figure 1).</span></span>

<span data-ttu-id="6e232-118">**Obrázek 1 – spustit nástroj Správa webu**</span><span class="sxs-lookup"><span data-stu-id="6e232-118">**Figure 1 – Launching the Web Site Administration Tool**</span></span>

![clip_image002](authenticating-users-with-forms-authentication-cs/_static/image1.jpg)

<span data-ttu-id="6e232-120">V rámci nástroj pro správu webu při vytváření nových uživatelů a rolí na kartě Zabezpečení dialogu. Klikněte **vytvořit uživateli** odkaz pro vytvoření nového uživatele s názvem Stephen (viz obrázek 2).</span><span class="sxs-lookup"><span data-stu-id="6e232-120">Within the Web Site Administration Tool, you create new users and roles by selecting the Security tab. Click the **Create user** link to create a new user named Stephen (see Figure 2).</span></span> <span data-ttu-id="6e232-121">Zadejte uživatele Stephen heslem, které chcete (například *tajný klíč*).</span><span class="sxs-lookup"><span data-stu-id="6e232-121">Provide the Stephen user with any password that you want (for example, *secret*).</span></span>

<span data-ttu-id="6e232-122">**Obrázek 2 – Vytvoření nového uživatele**</span><span class="sxs-lookup"><span data-stu-id="6e232-122">**Figure 2 – Creating a new user**</span></span>

![clip_image004](authenticating-users-with-forms-authentication-cs/_static/image2.jpg)

<span data-ttu-id="6e232-124">Při vytváření nové role první povolení role a definování jednu nebo více rolí.</span><span class="sxs-lookup"><span data-stu-id="6e232-124">You create new roles by first enabling roles and defining one or more roles.</span></span> <span data-ttu-id="6e232-125">Povolit role kliknutím **povolit role** odkaz.</span><span class="sxs-lookup"><span data-stu-id="6e232-125">Enable roles by clicking the **Enable roles** link.</span></span> <span data-ttu-id="6e232-126">Dále vytvořte roli s názvem *správci* kliknutím **vytvořit ani spravovat role** odkaz (viz obrázek 3).</span><span class="sxs-lookup"><span data-stu-id="6e232-126">Next, create a role named *Administrators* by clicking the **Create or Manage roles** link (see Figure 3).</span></span>

<span data-ttu-id="6e232-127">**Obrázek 3: vytvoření nové role**</span><span class="sxs-lookup"><span data-stu-id="6e232-127">**Figure 3 – Creating a new role**</span></span>

![clip_image006](authenticating-users-with-forms-authentication-cs/_static/image3.jpg)

<span data-ttu-id="6e232-129">Nakonec vytvořte nového uživatele s názvem Jan a přidružte Jan role správců tak, že kliknete na odkaz vytvořit uživatele výběr správci při vytváření Jan (viz obrázek 4).</span><span class="sxs-lookup"><span data-stu-id="6e232-129">Finally, create a new user named Sally and associate Sally with the Administrators role by clicking the Create User link and selecting Administrators when creating Sally (see Figure 4).</span></span>

<span data-ttu-id="6e232-130">**Obrázek 4 – přidání uživatele k roli**</span><span class="sxs-lookup"><span data-stu-id="6e232-130">**Figure 4 – Adding a user to a role**</span></span>

![clip_image008](authenticating-users-with-forms-authentication-cs/_static/image4.jpg)

<span data-ttu-id="6e232-132">Když všechny je uvedená a provádí, měli byste mít dva nové uživatele s názvem Stephen a Jan.</span><span class="sxs-lookup"><span data-stu-id="6e232-132">When all is said and done, you should have two new users named Stephen and Sally.</span></span> <span data-ttu-id="6e232-133">Musí být také novou roli s názvem Správci.</span><span class="sxs-lookup"><span data-stu-id="6e232-133">You should also have a new role named Administrators.</span></span> <span data-ttu-id="6e232-134">Jan je členem role Správci nástroje a Stephen není.</span><span class="sxs-lookup"><span data-stu-id="6e232-134">Sally is a member of the Administrators role and Stephen is not.</span></span>

#### <a name="requiring-authorization"></a><span data-ttu-id="6e232-135">Vyžadující ověření</span><span class="sxs-lookup"><span data-stu-id="6e232-135">Requiring Authorization</span></span>

<span data-ttu-id="6e232-136">Může vyžadovat uživatel ověřený, než uživatel vyvolá akce kontroleru přidáním atribut [autorizovat] na akci.</span><span class="sxs-lookup"><span data-stu-id="6e232-136">You can require a user to be authenticated before the user invokes a controller action by adding the [Authorize] attribute to the action.</span></span> <span data-ttu-id="6e232-137">Atribut [autorizovat] můžete použít pro jednotlivé řadiče akce nebo můžete použít tento atribut třídy celý kontroler.</span><span class="sxs-lookup"><span data-stu-id="6e232-137">You can apply the [Authorize] attribute to an individual controller action or you can apply this attribute to an entire controller class.</span></span>

<span data-ttu-id="6e232-138">Například řadič v výpis 1 zpřístupní akci s názvem CompanySecrets().</span><span class="sxs-lookup"><span data-stu-id="6e232-138">For example, the controller in Listing 1 exposes an action named CompanySecrets().</span></span> <span data-ttu-id="6e232-139">Protože tato akce je upraven pomocí atributu [Authorize], nelze vyvolat tuto akci, pokud je uživatel ověřen.</span><span class="sxs-lookup"><span data-stu-id="6e232-139">Because this action is decorated with the [Authorize] attribute, this action cannot be invoked unless a user is authenticated.</span></span>

<span data-ttu-id="6e232-140">**Výpis 1 – Controllers\HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="6e232-140">**Listing 1 – Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample1.cs)]

<span data-ttu-id="6e232-141">Pokud volání akce CompanySecrets() zadáním adresy URL/Home nebo CompanySecrets na panelu Adresa prohlížeče a nejste ověřeného uživatele, pak budete přesměrováni na přihlášení zobrazení automaticky (viz obrázek 5).</span><span class="sxs-lookup"><span data-stu-id="6e232-141">If you invoke the CompanySecrets() action by entering the URL /Home/CompanySecrets in the address bar of your browser, and you are not an authenticated user, then you will be redirected to the Login view automatically (see Figure 5).</span></span>

<span data-ttu-id="6e232-142">**Obrázek 5 – zobrazení přihlášení**</span><span class="sxs-lookup"><span data-stu-id="6e232-142">**Figure 5 – The Login view**</span></span>

![clip_image010](authenticating-users-with-forms-authentication-cs/_static/image5.jpg)

<span data-ttu-id="6e232-144">Zobrazení přihlášení můžete použít k zadání uživatelského jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="6e232-144">You can use the Login view to enter your user name and password.</span></span> <span data-ttu-id="6e232-145">Pokud si nejste registrovaní uživatelé pak můžete kliknout na **zaregistrovat** odkazu přejděte na rejstříku zobrazení (viz obrázek 6).</span><span class="sxs-lookup"><span data-stu-id="6e232-145">If you are not a registered user then you can click the **register** link to navigate to the Register view (see Figure 6).</span></span> <span data-ttu-id="6e232-146">Registrace zobrazení můžete vytvořit nový uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="6e232-146">You can use the Register view to create a new user account.</span></span>

<span data-ttu-id="6e232-147">**Obrázek 6 – registrace zobrazení**</span><span class="sxs-lookup"><span data-stu-id="6e232-147">**Figure 6 – The Register view**</span></span>

![clip_image012](authenticating-users-with-forms-authentication-cs/_static/image6.jpg)

<span data-ttu-id="6e232-149">Po úspěšném přihlášení, uvidíte CompanySecrets zobrazení (viz obrázek 7).</span><span class="sxs-lookup"><span data-stu-id="6e232-149">After you successfully log in, you can see the CompanySecrets view (see Figure 7).</span></span> <span data-ttu-id="6e232-150">Ve výchozím nastavení budou nadále přihlášení, dokud nezavřete okno prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="6e232-150">By default, you will continue to be logged in until you close your browser window.</span></span>

<span data-ttu-id="6e232-151">**Obrázek 7 – CompanySecrets zobrazení**</span><span class="sxs-lookup"><span data-stu-id="6e232-151">**Figure 7 – The CompanySecrets view**</span></span>

![clip_image014](authenticating-users-with-forms-authentication-cs/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a><span data-ttu-id="6e232-153">Autorizace uživatelské jméno nebo roli uživatele</span><span class="sxs-lookup"><span data-stu-id="6e232-153">Authorizing by User Name or User Role</span></span>

<span data-ttu-id="6e232-154">Atribut [autorizovat] můžete použít k omezení přístupu k akci kontroleru na konkrétní sadu uživatelů nebo konkrétní sadu rolí uživatele.</span><span class="sxs-lookup"><span data-stu-id="6e232-154">You can use the [Authorize] attribute to restrict access to a controller action to a particular set of users or a particular set of user roles.</span></span> <span data-ttu-id="6e232-155">Například upravené domovské řadiče v výpis 2 obsahuje dvě nové akce s názvem StephenSecrets() a AdministratorSecrets().</span><span class="sxs-lookup"><span data-stu-id="6e232-155">For example, the modified Home controller in Listing 2 contains two new actions named StephenSecrets() and AdministratorSecrets().</span></span>

<span data-ttu-id="6e232-156">**Výpis 2 – Controllers\HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="6e232-156">**Listing 2 – Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample2.cs)]

<span data-ttu-id="6e232-157">Pouze uživatel s uživatelským jménem Stephen vyvolat StephenSecrets() akce.</span><span class="sxs-lookup"><span data-stu-id="6e232-157">Only a user with the user name Stephen can invoke the StephenSecrets() action.</span></span> <span data-ttu-id="6e232-158">Všichni ostatní uživatelé přesměrováni na přihlášení zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6e232-158">All other users get redirected to the Login view.</span></span> <span data-ttu-id="6e232-159">Vlastnost uživatele akceptuje čárkami oddělený seznam názvy uživatelských účtů.</span><span class="sxs-lookup"><span data-stu-id="6e232-159">The Users property accepts a comma separated list of user account names.</span></span>

<span data-ttu-id="6e232-160">Jenom uživatelé v roli správce můžete vyvolat AdministratorSecrets() akce.</span><span class="sxs-lookup"><span data-stu-id="6e232-160">Only users in the Administrators role can invoke the AdministratorSecrets() action.</span></span> <span data-ttu-id="6e232-161">Protože Jan je členem skupiny Administrators, Jana vyvolání akce AdministratorSecrets().</span><span class="sxs-lookup"><span data-stu-id="6e232-161">For example, because Sally is a member of the Administrators group, she can invoke the AdministratorSecrets() action.</span></span> <span data-ttu-id="6e232-162">Všichni ostatní uživatelé přesměrováni na přihlášení zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6e232-162">All other users get redirected to the Login view.</span></span> <span data-ttu-id="6e232-163">Vlastnost role akceptuje čárkami oddělený seznam názvů rolí.</span><span class="sxs-lookup"><span data-stu-id="6e232-163">The Roles property accepts a comma separated list of role names.</span></span>

#### <a name="configuring-authentication"></a><span data-ttu-id="6e232-164">Konfigurace ověřování</span><span class="sxs-lookup"><span data-stu-id="6e232-164">Configuring Authentication</span></span>

<span data-ttu-id="6e232-165">V tomto okamžiku možná se Ptáte se se uloží informace o uživatelském účtu a role.</span><span class="sxs-lookup"><span data-stu-id="6e232-165">At this point, you might be wondering where the user account and role information is being stored.</span></span> <span data-ttu-id="6e232-166">Ve výchozím nastavení je informace uložené v databázi SQL Express (RANU) s názvem ASPNETDB.mdf umístěný v aplikaci aplikace MVC\_složku Data.</span><span class="sxs-lookup"><span data-stu-id="6e232-166">By default, the information is stored in a (RANU) SQL Express database named ASPNETDB.mdf located in your MVC application's App\_Data folder.</span></span> <span data-ttu-id="6e232-167">Tato databáze je generován rozhraní ASP.NET automaticky při spuštění pomocí členství.</span><span class="sxs-lookup"><span data-stu-id="6e232-167">This database is generated by the ASP.NET framework automatically when you start using membership.</span></span>

<span data-ttu-id="6e232-168">Chcete-li zobrazit databázi ASPNETDB.mdf v okně Průzkumníka řešení, musíte nejdřív vybrat možnost nabídky projekt, zobrazit všechny soubory.</span><span class="sxs-lookup"><span data-stu-id="6e232-168">In order to see the ASPNETDB.mdf database in the Solution Explorer window, you first need to select the menu option Project, Show All Files.</span></span>

<span data-ttu-id="6e232-169">Nastavit výchozí databázi SQL Express je v pořádku při vývoji aplikace.</span><span class="sxs-lookup"><span data-stu-id="6e232-169">Using the default SQL Express database is fine when developing an application.</span></span> <span data-ttu-id="6e232-170">Pravděpodobně však nebudou chcete použít výchozí ASPNETDB.mdf databáze pro produkční aplikace.</span><span class="sxs-lookup"><span data-stu-id="6e232-170">Most likely, however, you won't want to use the default ASPNETDB.mdf database for a production application.</span></span> <span data-ttu-id="6e232-171">V takovém případě můžete změnit, se uloží informace o uživatelském účtu pomocí následujících dvou kroků:</span><span class="sxs-lookup"><span data-stu-id="6e232-171">In that case, you can change where user account information is stored by completing the following two steps:</span></span>

1. <span data-ttu-id="6e232-172">Přidat objekty databáze aplikace služby k vaší databázi provozního – změnit připojovací řetězec aplikace tak, aby odkazoval na vaši produkční databázi</span><span class="sxs-lookup"><span data-stu-id="6e232-172">Add the Application Services database objects to your production database - Change your application connection string to point to your production database</span></span>

<span data-ttu-id="6e232-173">Prvním krokem je přidání všechny potřebné databázové objekty (tabulky a uložené procedury) pro vaši produkční databázi.</span><span class="sxs-lookup"><span data-stu-id="6e232-173">The first step is to add all of the necessary database objects (tables and stored procedures) to your production database.</span></span> <span data-ttu-id="6e232-174">Nejjednodušší způsob, jak přidat tyto objekty pro novou databázi je využít Průvodce instalací serveru SQL pro technologii ASP.NET (viz obrázek 8).</span><span class="sxs-lookup"><span data-stu-id="6e232-174">The easiest way to add these objects to a new database is to take advantage of the ASP.NET SQL Server Setup Wizard (see Figure 8).</span></span> <span data-ttu-id="6e232-175">Tento nástroj můžete spustit ze skupiny programu Microsoft Visual Studio 2008 otevřením Visual Studio 2008 příkazový řádek a spuštěním následujícího příkazu z příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="6e232-175">You can launch this tool by opening the Visual Studio 2008 Command Prompt from the Microsoft Visual Studio 2008 program group and executing the following command from the command prompt:</span></span>

<span data-ttu-id="6e232-176">aspnet\_regsql</span><span class="sxs-lookup"><span data-stu-id="6e232-176">aspnet\_regsql</span></span>

<span data-ttu-id="6e232-177">**Obrázek 8 – Průvodce instalací serveru SQL technologie ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="6e232-177">**Figure 8 – The ASP.NET SQL Server Setup Wizard**</span></span>

![clip_image016](authenticating-users-with-forms-authentication-cs/_static/image8.jpg)

<span data-ttu-id="6e232-179">Průvodce instalací serveru SQL technologie ASP.NET umožňuje vybrat databázi systému SQL Server v síti a nainstalujte všechny databázové objekty, které vyžadují služby aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6e232-179">The ASP.NET SQL Server Setup Wizard enables you to select a SQL Server database on your network and install all of the database objects required by the ASP.NET application services.</span></span> <span data-ttu-id="6e232-180">Databázový server není potřeba nacházet na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="6e232-180">The database server is not required to be located on your local machine.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="6e232-181">Pokud nechcete použít Průvodce instalací serveru SQL pro ASP.NET, můžete najít skripty SQL pro přidání aplikace služby databázové objekty v následující složce:</span><span class="sxs-lookup"><span data-stu-id="6e232-181">If you don't want to use the ASP.NET SQL Server Setup Wizard, then you can find SQL scripts for adding the application services database objects in the following folder:</span></span>
> 
> > <span data-ttu-id="6e232-182">C:\Windows\Microsoft.NET\Framework\v2.0.50727</span><span class="sxs-lookup"><span data-stu-id="6e232-182">C:\Windows\Microsoft.NET\Framework\v2.0.50727</span></span>


<span data-ttu-id="6e232-183">Po vytvoření nezbytné databázové objekty, budete muset upravit připojení k databázi používat aplikaci MVC.</span><span class="sxs-lookup"><span data-stu-id="6e232-183">After you create the necessary database objects, you need to modify the database connection used by your MVC application.</span></span> <span data-ttu-id="6e232-184">Upravte připojovací řetězec ApplicationServices v souboru webové konfigurace (web.config) tak, aby ukazoval na produkční databázi.</span><span class="sxs-lookup"><span data-stu-id="6e232-184">Modify the ApplicationServices connection string in your web configuration (web.config) file so that it points to the production database.</span></span> <span data-ttu-id="6e232-185">Například upravené připojení v výpis 3 odkazuje na databázi s názvem MyProductionDB (původní připojovací řetězec ApplicationServices byla změněna na komentář).</span><span class="sxs-lookup"><span data-stu-id="6e232-185">For example, the modified connection in Listing 3 points to a database named MyProductionDB (the original ApplicationServices connection string has been commented out).</span></span>

<span data-ttu-id="6e232-186">**Výpis 3 – soubor Web.config**</span><span class="sxs-lookup"><span data-stu-id="6e232-186">**Listing 3 – Web.config**</span></span>

[!code-xml[Main](authenticating-users-with-forms-authentication-cs/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a><span data-ttu-id="6e232-187">Konfigurace oprávnění databáze</span><span class="sxs-lookup"><span data-stu-id="6e232-187">Configuring Database Permissions</span></span>

<span data-ttu-id="6e232-188">Pokud používáte integrované zabezpečení pro připojení k databázi je potřeba přidat správné uživatelský účet systému Windows jako přihlašovací údaje k vaší databázi.</span><span class="sxs-lookup"><span data-stu-id="6e232-188">If you use Integrated Security to connect to your database then you will need to add the correct Windows user account as a login to your database.</span></span> <span data-ttu-id="6e232-189">Správný účet, závisí na tom, jestli používáte vývojový Server ASP.NET nebo služby IIS jako webový server.</span><span class="sxs-lookup"><span data-stu-id="6e232-189">The correct account depends on whether you are using the ASP.NET Development Server or Internet Information Services as your web server.</span></span> <span data-ttu-id="6e232-190">Správné uživatelský účet také závisí na operačním systému.</span><span class="sxs-lookup"><span data-stu-id="6e232-190">The correct user account also depends on your operating system.</span></span>

<span data-ttu-id="6e232-191">Pokud používáte je ASP.NET Development Server (výchozí webový server využívá sada Visual Studio) vaší aplikace spustí v kontextu uživatelského účtu systému Windows.</span><span class="sxs-lookup"><span data-stu-id="6e232-191">If you are using the ASP.NET Development Server (the default web server used by Visual Studio) then your application executes within the context of your Windows user account.</span></span> <span data-ttu-id="6e232-192">V takovém případě musíte přidat uživatelský účet systému Windows jako server přihlášení databáze.</span><span class="sxs-lookup"><span data-stu-id="6e232-192">In that case, you need to add your Windows user account as a database server login.</span></span>

<span data-ttu-id="6e232-193">Případně pokud používáte Internetová informační služba bude nutné přidání účtu ASPNET nebo účet NT AUTORITY nebo síťové služby jako server přihlášení databáze.</span><span class="sxs-lookup"><span data-stu-id="6e232-193">Alternatively, if you are using Internet Information Services then you need to add either the ASPNET account or the NT AUTHORITY/NETWORK SERVICE account as a database server login.</span></span> <span data-ttu-id="6e232-194">Pokud používáte systém Windows XP přidejte účtu ASPNET jako přihlašovací údaje k vaší databázi.</span><span class="sxs-lookup"><span data-stu-id="6e232-194">If you are using Windows XP then add the ASPNET account as a login to your database.</span></span> <span data-ttu-id="6e232-195">Pokud používáte novější operačního systému, například Windows Vista nebo Windows Server 2008, přidejte účet služby NT AUTORITY nebo SÍŤOVÝCH jako přihlášení k databázi.</span><span class="sxs-lookup"><span data-stu-id="6e232-195">If you are using a more recent operating system, such as Windows Vista or Windows Server 2008, then add the NT AUTHORITY/NETWORK SERVICE account as the database login.</span></span>

<span data-ttu-id="6e232-196">Můžete přidat nový uživatelský účet k vaší databázi pomocí Microsoft SQL Server Management Studio (viz obrázek 9).</span><span class="sxs-lookup"><span data-stu-id="6e232-196">You can add a new user account to your database by using Microsoft SQL Server Management Studio (see Figure 9).</span></span>

<span data-ttu-id="6e232-197">**Obrázek 9 – vytvoření nové přihlašovací údaje systému Microsoft SQL Server**</span><span class="sxs-lookup"><span data-stu-id="6e232-197">**Figure 9 – Creating a new Microsoft SQL Server login**</span></span>

![clip_image018](authenticating-users-with-forms-authentication-cs/_static/image9.jpg)

<span data-ttu-id="6e232-199">Jakmile vytvoříte požadované přihlášení, budete muset mapovat přihlášení uživatele databáze s správné databázové role.</span><span class="sxs-lookup"><span data-stu-id="6e232-199">After you create the required login, you need to map the login to a database user with the right database roles.</span></span> <span data-ttu-id="6e232-200">Dvakrát klikněte na přihlášení a vyberte kartu Mapování uživatele. Vyberte jednu nebo více rolí databáze aplikace služby.</span><span class="sxs-lookup"><span data-stu-id="6e232-200">Double-click the login and select the User Mapping tab. Select one or more application services database roles.</span></span> <span data-ttu-id="6e232-201">Například, aby bylo možné ověřovat uživatele, je nutné povolit aspnet\_členství\_BasicAccess databázové role.</span><span class="sxs-lookup"><span data-stu-id="6e232-201">For example, in order to authenticate users, you need to enable the aspnet\_Membership\_BasicAccess database role.</span></span> <span data-ttu-id="6e232-202">Chcete-li vytvořit nové uživatele, musíte povolit aspnet\_členství\_FullAccess databázové role (viz obrázek 10).</span><span class="sxs-lookup"><span data-stu-id="6e232-202">In order to create new users, you need to enable the aspnet\_Membership\_FullAccess database role (see Figure 10).</span></span>

<span data-ttu-id="6e232-203">**Obrázek 10: přidání role databáze aplikačních služeb**</span><span class="sxs-lookup"><span data-stu-id="6e232-203">**Figure 10 – Adding Application Services database roles**</span></span>

![clip_image020](authenticating-users-with-forms-authentication-cs/_static/image10.jpg)

#### <a name="summary"></a><span data-ttu-id="6e232-205">Souhrn</span><span class="sxs-lookup"><span data-stu-id="6e232-205">Summary</span></span>

<span data-ttu-id="6e232-206">V tomto kurzu jste zjistili, jak používat ověřování pomocí formulářů, při vytváření aplikace ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="6e232-206">In this tutorial, you learned how to use Forms authentication when building an ASP.NET MVC application.</span></span> <span data-ttu-id="6e232-207">Nejprve jste zjistili, jak vytvořit nové uživatele a role a využívají nástroj pro správu webu.</span><span class="sxs-lookup"><span data-stu-id="6e232-207">First, you learned how to create new users and roles by taking advantage of the Web Site Administration Tool.</span></span> <span data-ttu-id="6e232-208">V dalším kroku jste zjistili, jak pomocí atributu [autorizovat] zabránit neoprávněným uživatelům v vyvolání akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="6e232-208">Next, you learned how to use the [Authorize] attribute to prevent unauthorized users from invoking controller actions.</span></span> <span data-ttu-id="6e232-209">Nakonec jste zjistili, jak nakonfigurovat aplikace MVC uložit uživatele a informace o rolích do provozní databáze.</span><span class="sxs-lookup"><span data-stu-id="6e232-209">Finally, you learned how to configure your MVC application to store user and role information in a production database.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="6e232-210">Next</span><span class="sxs-lookup"><span data-stu-id="6e232-210">Next</span></span>](authenticating-users-with-windows-authentication-cs.md)
