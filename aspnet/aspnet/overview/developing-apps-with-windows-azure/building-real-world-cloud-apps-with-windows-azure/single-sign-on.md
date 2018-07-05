---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: Jednotné přihlašování (vytváření skutečných cloudových aplikací s Azure) | Dokumentace Microsoftu
author: MikeWasson
description: Vytváření reálného světa cloudových aplikací s Azure e kniha je založená na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které se dají mu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 213b8fe091bcac7f55fd62ab305c77fcbc5a77ad
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374377"
---
<a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a><span data-ttu-id="71840-104">Jednotné přihlašování (vytváření skutečných cloudových aplikací s Azure)</span><span class="sxs-lookup"><span data-stu-id="71840-104">Single Sign-On (Building Real-World Cloud Apps with Azure)</span></span>
====================
<span data-ttu-id="71840-105">podle [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Petr Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="71840-105">by [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="71840-106">[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronickou knihu](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="71840-106">[Download Fix It Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) or [Download E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span></span>

> <span data-ttu-id="71840-107">**Vytváření reálného světa cloudových aplikací s Azure** e knihy je založena na prezentaci vypracovanou organizací cccppf Scott Guthrie.</span><span class="sxs-lookup"><span data-stu-id="71840-107">The **Building Real World Cloud Apps with Azure** e-book is based on a presentation developed by Scott Guthrie.</span></span> <span data-ttu-id="71840-108">Vysvětluje 13 vzory a postupy, které vám pomůžou být úspěšný vývoj webových aplikací v cloudu.</span><span class="sxs-lookup"><span data-stu-id="71840-108">It explains 13 patterns and practices that can help you be successful developing web apps for the cloud.</span></span> <span data-ttu-id="71840-109">Informace o e kniha najdete v tématu [první kapitoly](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="71840-109">For information about the e-book, see [the first chapter](introduction.md).</span></span>


<span data-ttu-id="71840-110">Existuje mnoho problémů se zabezpečením zamyslet, když vytváříte cloudové aplikace, ale pro tuto sérii zaměříme na jenom jednom: jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="71840-110">There are many security issues to think about when you're developing a cloud app, but for this series we'll focus on just one: single sign-on.</span></span> <span data-ttu-id="71840-111">Lidé otázku se často ptají, je to: "jsem jsem primárně vytváření aplikací pro zaměstnance společnosti; jak hostování těchto aplikací v cloudu a stále povolit je, aby používaly stejný model zabezpečení, který svých zaměstnanců znají a používat v místním prostředí, když běží aplikace, které jsou hostované za firewallem?"</span><span class="sxs-lookup"><span data-stu-id="71840-111">A question people often ask is this: "I'm primarily building apps for the employees of my company; how do I host these apps in the cloud and still enable them to use the same security model that my employees know and use in the on-premises environment when they're running apps that are hosted inside the firewall?"</span></span> <span data-ttu-id="71840-112">Jeden ze způsobů, jak můžeme povolit tento scénář se nazývá Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="71840-112">One of the ways we enable this scenario is called Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="71840-113">Azure AD umožňuje zpřístupnit enterprise-obchodní (LOB) aplikacím přes Internet a umožňuje zpřístupnit tyto aplikace a obchodními partnery.</span><span class="sxs-lookup"><span data-stu-id="71840-113">Azure AD enables you to make enterprise line-of-business (LOB) apps available over the Internet, and it enables you to make these apps available to business partners as well.</span></span>

## <a name="introduction-to-azure-ad"></a><span data-ttu-id="71840-114">Úvod do služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="71840-114">Introduction to Azure AD</span></span>

<span data-ttu-id="71840-115">[Azure AD](https://docs.microsoft.com/azure/active-directory/) poskytuje [služby Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) v cloudu.</span><span class="sxs-lookup"><span data-stu-id="71840-115">[Azure AD](https://docs.microsoft.com/azure/active-directory/) provides [Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) in the cloud.</span></span> <span data-ttu-id="71840-116">Klíčové funkce patří:</span><span class="sxs-lookup"><span data-stu-id="71840-116">Key features include the following:</span></span>

- <span data-ttu-id="71840-117">Integruje místní služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="71840-117">It integrates with on-premises Active Directory.</span></span>
- <span data-ttu-id="71840-118">Umožňuje jednotné přihlašování s vašimi aplikacemi.</span><span class="sxs-lookup"><span data-stu-id="71840-118">It enables single sign-on with your apps.</span></span>
- <span data-ttu-id="71840-119">Podporuje otevřených standardů, jako [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Fed](http://en.wikipedia.org/wiki/WS-Federation), a [OAuth 2.0](http://oauth.net/2/).</span><span class="sxs-lookup"><span data-stu-id="71840-119">It supports open standards such as [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Fed](http://en.wikipedia.org/wiki/WS-Federation), and [OAuth 2.0](http://oauth.net/2/).</span></span>
- <span data-ttu-id="71840-120">Podporuje organizace [rozhraní REST API služby Graph](https://msdn.microsoft.com/library/hh974476.aspx).</span><span class="sxs-lookup"><span data-stu-id="71840-120">It supports Enterprise [Graph REST API](https://msdn.microsoft.com/library/hh974476.aspx).</span></span>

<span data-ttu-id="71840-121">Předpokládejme, že máte v místním prostředí systému Windows Server Active Directory, který používáte, abyste mohli zaměstnancům zajistit Přihlaste se k intranetu aplikace:</span><span class="sxs-lookup"><span data-stu-id="71840-121">Suppose you have an on-premises Windows Server Active Directory environment that you use to enable employees to sign on to Intranet apps:</span></span>

![](single-sign-on/_static/image1.png)

<span data-ttu-id="71840-122">Co Azure AD díky tomu můžete je vytvořit adresář v cloudu.</span><span class="sxs-lookup"><span data-stu-id="71840-122">What Azure AD enables you to do is create a directory in the cloud.</span></span> <span data-ttu-id="71840-123">Je to bezplatná funkce a umožňují snadné nastavení.</span><span class="sxs-lookup"><span data-stu-id="71840-123">It's a free feature and easy to set up.</span></span>

<span data-ttu-id="71840-124">Může být zcela nezávislý na vaše místní službu Active Directory. můžete vložit kdokoli má v sobě a jejich ověření v aplikacích Internet.</span><span class="sxs-lookup"><span data-stu-id="71840-124">It can be entirely independent from your on-premises Active Directory; you can put anyone you want in it and authenticate them in Internet apps.</span></span>

![Windows Azure Active Directory](single-sign-on/_static/image2.png)

<span data-ttu-id="71840-126">Nebo ji integrovat s místní AD.</span><span class="sxs-lookup"><span data-stu-id="71840-126">Or you can integrate it with your on-premises AD.</span></span>

![AD a WAAD integrace](single-sign-on/_static/image3.png)

<span data-ttu-id="71840-128">Nyní všichni zaměstnanci, kteří můžou ověřovat místní objekt moci ověřovat také přes Internet – aniž by bylo nutné otevírat brány firewall nebo nasazovat nové servery ve vašem datovém centru.</span><span class="sxs-lookup"><span data-stu-id="71840-128">Now all the employees who can authenticate on-premises can also authenticate over the Internet – without you having to open up a firewall or deploy any new servers in your data center.</span></span> <span data-ttu-id="71840-129">Můžete dál využívat všechny stávajícího prostředí Active Directory, které znáte a používáte poskytnout vaše interní aplikace jednotného přihlašování na funkce.</span><span class="sxs-lookup"><span data-stu-id="71840-129">You can continue to leverage all the existing Active Directory environment that you know and use today to give your internal apps single-sign on capability.</span></span>

<span data-ttu-id="71840-130">Po provedení tohoto připojení mezi službami AD a Azure AD, můžete také povolit webových aplikací a mobilních zařízení k ověření vaši zaměstnanci v cloudu a můžete povolit aplikacím třetích stran, jako je Office 365 a SalesForce.com, Google apps, tak, aby přijímal vaší přihlašovací údaje zaměstnanců.</span><span class="sxs-lookup"><span data-stu-id="71840-130">Once you've made this connection between AD and Azure AD, you can also enable your web apps and your mobile devices to authenticate your employees in the cloud, and you can enable third-party apps, such as Office 365, SalesForce.com, or Google apps, to accept your employees' credentials.</span></span> <span data-ttu-id="71840-131">Pokud používáte Office 365, máte již nastavíte s Azure AD vzhledem k tomu Office 365 používá Azure AD pro ověřování a autorizaci.</span><span class="sxs-lookup"><span data-stu-id="71840-131">If you're using Office 365, you're already set up with Azure AD because Office 365 uses Azure AD for authentication and authorization.</span></span>

![3. stran aplikace](single-sign-on/_static/image4.png)

<span data-ttu-id="71840-133">Výhodou tohoto přístupu je, že vaše organizace přidá nebo odstraní uživatele, nebo uživatel změní heslo, použijte stejný postup, který už dnes používáte v místním prostředí.</span><span class="sxs-lookup"><span data-stu-id="71840-133">The beauty of this approach is that any time your organization adds or deletes a user, or a user changes a password, you use the same process that you use today in your on-premises environment.</span></span> <span data-ttu-id="71840-134">Všechny z místní AD změny automaticky rozšíří na cloudovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="71840-134">All of your on-premises AD changes are automatically propagated to the cloud environment.</span></span>

<span data-ttu-id="71840-135">Pokud vaše společnost používá nebo přesunutí do Office 365, dobrou zprávou je, že bude nutné nastavit automaticky protože Office 365 používá Azure AD pro ověřování Azure AD.</span><span class="sxs-lookup"><span data-stu-id="71840-135">If your company is using or moving to Office 365, the good news is that you'll have Azure AD set up automatically because Office 365 uses Azure AD for authentication.</span></span> <span data-ttu-id="71840-136">Abyste mohli snadno použít ve své vlastní aplikace stejným ověřování, které používá Office 365.</span><span class="sxs-lookup"><span data-stu-id="71840-136">So you can easily use in your own apps the same authentication that Office 365 uses.</span></span>

## <a name="set-up-an-azure-ad-tenant"></a><span data-ttu-id="71840-137">Nastavení tenanta služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="71840-137">Set up an Azure AD tenant</span></span>

<span data-ttu-id="71840-138">adresář Azure AD se volá s Azure AD [tenanta](https://technet.microsoft.com/library/jj573650.aspx), a nastavení klienta je poměrně snadné.</span><span class="sxs-lookup"><span data-stu-id="71840-138">an Azure AD directory is called an Azure AD [tenant](https://technet.microsoft.com/library/jj573650.aspx), and setting up a tenant is pretty easy.</span></span> <span data-ttu-id="71840-139">Vám ukážeme, jak se provádí v portálu pro správu Azure k objasnění konceptů, ale samozřejmě jako dalších funkcí portálu můžete provést také ho pomocí skriptu nebo rozhraní API pro správu.</span><span class="sxs-lookup"><span data-stu-id="71840-139">We'll show you how it's done in the Azure Management Portal in order to illustrate the concepts, but of course like the other portal functions you can also do it by using a script or management API.</span></span>

<span data-ttu-id="71840-140">V portálu pro správu klikněte na kartu služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="71840-140">In the management portal click the Active Directory tab.</span></span>

![WAAD portálu](single-sign-on/_static/image5.png)

<span data-ttu-id="71840-142">Máte jednoho tenanta Azure AD automaticky ke svému účtu Azure a můžete kliknout **přidat** tlačítko v dolní části stránky a vytvořte další adresáře souborů.</span><span class="sxs-lookup"><span data-stu-id="71840-142">You automatically have one Azure AD tenant for your Azure account, and you can click the **Add** button at the bottom of the page to create additional directories.</span></span> <span data-ttu-id="71840-143">Jednu pro testovací prostředí a druhý pro produkční prostředí, může být například vhodné.</span><span class="sxs-lookup"><span data-stu-id="71840-143">You might want one for a test environment and one for production, for example.</span></span> <span data-ttu-id="71840-144">Pečlivě zvážit, co zadáte název nového adresáře.</span><span class="sxs-lookup"><span data-stu-id="71840-144">Think carefully about what you name a new directory.</span></span> <span data-ttu-id="71840-145">Pokud pak použijete název znovu jednoho z uživatelů, použijte název vašeho adresáře, který může být matoucí.</span><span class="sxs-lookup"><span data-stu-id="71840-145">If you use your name for the directory and then you use your name again for one of the users, that can be confusing.</span></span>

![Přidat adresář](single-sign-on/_static/image6.png)

<span data-ttu-id="71840-147">Portál obsahuje plnou podporu pro vytváření, odstraňování a správu uživatelů v rámci tohoto prostředí.</span><span class="sxs-lookup"><span data-stu-id="71840-147">The portal has full support for creating, deleting, and managing users within this environment.</span></span> <span data-ttu-id="71840-148">Například, chcete-li přidat uživatele přejít na **uživatelé** kartě a klikněte na tlačítko **přidat uživatele** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="71840-148">For example, to add a user go to the **Users** tab and click the **Add User** button.</span></span>

![Tlačítko Přidat uživatele](single-sign-on/_static/image7.png)

![Přidat dialog uživatele](single-sign-on/_static/image8.png)

<span data-ttu-id="71840-151">Můžete vytvořit nového uživatele, který se nachází pouze v tomto adresáři, nebo můžete zaregistrovat Account Microsoft jako uživatel v tomto adresáři, nebo register nebo uživatele z jiného adresáře služby Azure AD jako uživatel v tomto adresáři.</span><span class="sxs-lookup"><span data-stu-id="71840-151">You can create a new user who exists only in this directory, or you can register a Microsoft Account as a user in this directory, or register or a user from another Azure AD directory as a user in this directory.</span></span> <span data-ttu-id="71840-152">(V adresáři skutečné, výchozí domény by ContosoTest.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="71840-152">(In a real directory, the default domain would be ContosoTest.onmicrosoft.com.</span></span> <span data-ttu-id="71840-153">Můžete také použít doménu podle vlastního výběru, třeba contoso.com.)</span><span class="sxs-lookup"><span data-stu-id="71840-153">You can also use a domain of your own choosing, like contoso.com.)</span></span>

![Uživatelské typy](single-sign-on/_static/image9.png)

![Přidat dialog uživatele](single-sign-on/_static/image10.png)

<span data-ttu-id="71840-156">Přiřadit uživatele k roli.</span><span class="sxs-lookup"><span data-stu-id="71840-156">You can assign the user to a role.</span></span>

![Profil uživatele](single-sign-on/_static/image11.png)

<span data-ttu-id="71840-158">A účet se vytvoří s dočasným heslem.</span><span class="sxs-lookup"><span data-stu-id="71840-158">And the account is created with a temporary password.</span></span>

![Dočasné heslo](single-sign-on/_static/image12.png)

<span data-ttu-id="71840-160">Uživatele vytvoříte tímto způsobem můžete okamžitě Přihlaste se k vaší webové aplikace s využitím tento adresář v cloudu.</span><span class="sxs-lookup"><span data-stu-id="71840-160">The users you create this way can immediately log in to your web apps using this cloud directory.</span></span>

<span data-ttu-id="71840-161">Co se skvěle hodí pro podnikové jednotné přihlašování, ale je **integrace adresáře** kartu:</span><span class="sxs-lookup"><span data-stu-id="71840-161">What's great for enterprise single sign-on, though, is the **Directory Integration** tab:</span></span>

![Karta integrace adresáře](single-sign-on/_static/image13.png)

<span data-ttu-id="71840-163">Pokud povolíte integrace adresáře a [stáhněte si nástroj](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), můžete synchronizovat tento adresář v cloudu s vaší stávající místní služby Active Directory, který již používáte ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="71840-163">If you enable directory integration, and [download a tool](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), you can sync this cloud directory with your existing on-premises Active Directory that you're already using inside your organization.</span></span> <span data-ttu-id="71840-164">Potom všechny uživatele uložené v adresáři se zobrazí tento adresář v cloudu.</span><span class="sxs-lookup"><span data-stu-id="71840-164">Then all of the users stored in your directory will show up in this cloud directory.</span></span> <span data-ttu-id="71840-165">Cloudové aplikace může ověřit nyní všechny zaměstnance pomocí existujících přihlašovacích údajů služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="71840-165">Your cloud apps can now authenticate all of your employees using their existing Active Directory credentials.</span></span> <span data-ttu-id="71840-166">A všechno, co to je bezplatné – nástroj pro synchronizaci i samotné služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="71840-166">And all this is free – both the sync tool and Azure AD itself.</span></span>

<span data-ttu-id="71840-167">Tento nástroj je průvodce, který se snadno používá, jak je vidět z těchto obrazovek.</span><span class="sxs-lookup"><span data-stu-id="71840-167">The tool is a wizard that is easy to use, as you can see from these screen shots.</span></span> <span data-ttu-id="71840-168">Ty nejsou úplné pokyny uvedené jenom jako příklad zobrazuje základní proces.</span><span class="sxs-lookup"><span data-stu-id="71840-168">These are not complete instructions, just an example showing you the basic process.</span></span> <span data-ttu-id="71840-169">Podrobnější postupy-k-it, najdete na odkazech v [prostředky](#resources) části na konci kapitoly.</span><span class="sxs-lookup"><span data-stu-id="71840-169">For more detailed how-to-do-it information, see the links in the [Resources](#resources) section at the end of the chapter.</span></span>

![Průvodce konfigurací nástroje synchronizace WAAD](single-sign-on/_static/image14.png)

<span data-ttu-id="71840-171">Klikněte na tlačítko **Další**a pak zadejte svoje přihlašovací údaje Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="71840-171">Click **Next**, and then enter your Azure Active Directory credentials.</span></span>

![Průvodce konfigurací nástroje synchronizace WAAD](single-sign-on/_static/image15.png)

<span data-ttu-id="71840-173">Klikněte na tlačítko **Další**a poté zadejte místní přihlašovací údaje služby AD.</span><span class="sxs-lookup"><span data-stu-id="71840-173">Click **Next**, and then enter your on-premises AD credentials.</span></span>

![Průvodce konfigurací nástroje synchronizace WAAD](single-sign-on/_static/image16.png)

<span data-ttu-id="71840-175">Klikněte na tlačítko **Další**a pak poznáte, zda chcete ukládat hodnoty hash hesla služby AD v cloudu.</span><span class="sxs-lookup"><span data-stu-id="71840-175">Click **Next**, and then indicate if you want to store a hash of your AD passwords in the cloud.</span></span>

![Průvodce konfigurací nástroje synchronizace WAAD](single-sign-on/_static/image17.png)

<span data-ttu-id="71840-177">Hodnota hash hesla, které lze uložit do cloudu tvoří otisk; skutečná hesla se nikdy neukládají v Azure AD.</span><span class="sxs-lookup"><span data-stu-id="71840-177">The password hash that you can store in the cloud is a one-way hash; actual passwords are never stored in Azure AD.</span></span> <span data-ttu-id="71840-178">Pokud se rozhodnete pro ukládání hodnoty hash v cloudu, budete muset použít [Active Directory Federation Services](https://technet.microsoft.com/library/hh831502.aspx) (služby AD FS).</span><span class="sxs-lookup"><span data-stu-id="71840-178">If you decide against storing hashes in the cloud, you'll have to use [Active Directory Federation Services](https://technet.microsoft.com/library/hh831502.aspx) (ADFS).</span></span> <span data-ttu-id="71840-179">Existují také [dalších faktorů, které je třeba zvážit při výběru, jestli se mají použít služby AD FS](https://technet.microsoft.com/library/jj573653.aspx).</span><span class="sxs-lookup"><span data-stu-id="71840-179">There are also [other factors to consider when choosing whether or not to use ADFS](https://technet.microsoft.com/library/jj573653.aspx).</span></span> <span data-ttu-id="71840-180">Možnost služby AD FS vyžaduje několik dalších kroků konfigurace.</span><span class="sxs-lookup"><span data-stu-id="71840-180">The ADFS option requires a few additional configuration steps.</span></span>

<span data-ttu-id="71840-181">Pokud se rozhodnete ukládat hodnoty hash v cloudu, budete mít a spustí nástroj pro synchronizaci adresářů, po kliknutí na **Další**.</span><span class="sxs-lookup"><span data-stu-id="71840-181">If you choose to store hashes in the cloud, you're done, and the tool starts synchronizing directories when you click **Next**.</span></span>

![Průvodce konfigurací nástroje synchronizace WAAD](single-sign-on/_static/image18.png)

<span data-ttu-id="71840-183">A za pár minut budete mít.</span><span class="sxs-lookup"><span data-stu-id="71840-183">And in a few minutes you're done.</span></span>

![Průvodce konfigurací nástroje synchronizace WAAD](single-sign-on/_static/image19.png)

<span data-ttu-id="71840-185">Stačí ji spustit na jednom řadiči domény v organizaci na Windows serveru 2003 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="71840-185">You only have to run this on one domain controller in the organization, on Windows 2003 or higher.</span></span> <span data-ttu-id="71840-186">A není potřeba restartovat.</span><span class="sxs-lookup"><span data-stu-id="71840-186">And no need to reboot.</span></span> <span data-ttu-id="71840-187">Když to uděláte, všichni uživatelé jsou v cloudu a můžete provést jednotné přihlašování z jakéhokoli webových nebo mobilních aplikací pomocí SAML a OAuth, WS-Fed.</span><span class="sxs-lookup"><span data-stu-id="71840-187">When you're done, all of your users are in the cloud and you can do single sign-on from any web or mobile application, using SAML, OAuth, or WS-Fed.</span></span>

<span data-ttu-id="71840-188">Někdy jsme získat požádáni o zabezpečené jde – Microsoft používá ho pro své vlastní citlivá firemní data?</span><span class="sxs-lookup"><span data-stu-id="71840-188">Sometimes we get asked about how secure this is – does Microsoft use it for their own sensitive business data?</span></span> <span data-ttu-id="71840-189">A odpověď zní: Ano, co děláme.</span><span class="sxs-lookup"><span data-stu-id="71840-189">And the answer is yes we do.</span></span> <span data-ttu-id="71840-190">Například, pokud přejdete na interní web Microsoft SharePoint na [ https://microsoft.sharepoint.com/ ](https://microsoft.sharepoint.com/), výzva k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="71840-190">For example, if you go to the internal Microsoft SharePoint site at [https://microsoft.sharepoint.com/](https://microsoft.sharepoint.com/), you get prompted to log in.</span></span>

![Přihlášení k Office 365](single-sign-on/_static/image20.png)

<span data-ttu-id="71840-192">Microsoft má povolenou služby AD FS, takže když zadáte ID Microsoftu, získat přesměruje na přihlašovací stránku služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="71840-192">Microsoft has enabled ADFS, so when you enter a Microsoft ID, you get redirected to an ADFS log-in page.</span></span>

![Přihlášení služby AD FS](single-sign-on/_static/image21.png)

<span data-ttu-id="71840-194">A jakmile zadáte přihlašovací údaje uložené v interní účet Microsoft AD, máte přístup k této interní aplikace.</span><span class="sxs-lookup"><span data-stu-id="71840-194">And once you enter credentials stored in an internal Microsoft AD account, you have access to this internal application.</span></span>

![MS Sharepointového webu](single-sign-on/_static/image22.png)

<span data-ttu-id="71840-196">Používáme přihlášení serveru služby AD hlavně, protože již bylo předtím, než jsou dostupné služby Azure AD, ale proces přihlášení prochází adresář Azure AD v cloudu nastavenou službu AD FS.</span><span class="sxs-lookup"><span data-stu-id="71840-196">We're using an AD sign-in server mainly because we already had ADFS set up before Azure AD became available, but the log-in process is going through an Azure AD directory in the cloud.</span></span> <span data-ttu-id="71840-197">Jsme naše důležité dokumenty, správy zdrojového kódu, souborů správy výkonu, prodejní sestavy a informace, v cloudu a pomocí tohoto přesně stejné řešení je zabezpečit.</span><span class="sxs-lookup"><span data-stu-id="71840-197">We put our important documents, source control, performance management files, sales reports, and more, in the cloud and are using this exact same solution to secure them.</span></span>

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a><span data-ttu-id="71840-198">Vytvoření aplikace ASP.NET využívající Azure AD pro jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="71840-198">Create an ASP.NET app that uses Azure AD for single sign-on</span></span>

<span data-ttu-id="71840-199">Visual Studio umožňuje skutečně snadno vytvořit aplikaci, která používá Azure AD pro jednotné přihlašování, jak je vidět z několika snímky obrazovky.</span><span class="sxs-lookup"><span data-stu-id="71840-199">Visual Studio makes it really easy to create an app that uses Azure AD for single sign-on, as you can see from a few screen shots.</span></span>

<span data-ttu-id="71840-200">Když vytvoříte novou aplikaci ASP.NET, MVC nebo webového formuláře, je výchozí metodu ověřování ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="71840-200">When you create a new ASP.NET application, either MVC or Web Forms, the default authentication method is ASP.NET Identity.</span></span> <span data-ttu-id="71840-201">Chcete-li změnit, která do služby Azure AD, kliknete **změnit ověřování** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="71840-201">To change that to Azure AD, you click a **Change Authentication** button.</span></span>

![Změnit ověřování](single-sign-on/_static/image23.png)

<span data-ttu-id="71840-203">Vyberte účty organizace, zadejte název domény a pak vyberte Single Sign On.</span><span class="sxs-lookup"><span data-stu-id="71840-203">Select Organizational Accounts, enter your domain name, and then select Single Sign On.</span></span>

![Konfigurace dialog ověřování.](single-sign-on/_static/image24.png)

<span data-ttu-id="71840-205">Můžete také poskytnout aplikaci pro čtení nebo pro čtení a zápis oprávnění pro adresář data.</span><span class="sxs-lookup"><span data-stu-id="71840-205">You can also give the app read or read/write permission for directory data.</span></span> <span data-ttu-id="71840-206">Pokud to uděláte, můžete použít [REST API služby Azure Graph](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) k vyhledání uživatelů telefonní číslo, zjistěte, pokud jsou v kanceláři, po poslední přihlášení na atd.</span><span class="sxs-lookup"><span data-stu-id="71840-206">If you do that, it can use the [Azure Graph REST API](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) to look up users' phone number, find out if they're in the office, when they last logged on, etc.</span></span>

<span data-ttu-id="71840-207">To je všechno, co musíte udělat – Visual Studio výzvu pro přihlašovací údaje pro správce pro vašeho tenanta Azure AD a pak nastaví váš projekt a tenanta Azure AD pro novou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="71840-207">That's all you have to do - Visual Studio asks for the credentials for an administrator for your Azure AD tenant, and then it configures both your project and your Azure AD tenant for the new application.</span></span>

<span data-ttu-id="71840-208">Při spuštění projektu se zobrazí přihlašovací stránku a můžete se přihlásit pomocí přihlašovacích údajů uživatele v adresáři služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="71840-208">When you run the project, you'll see a sign-in page, and you can sign in with credentials of a user in your Azure AD directory.</span></span>

![Přihlašovací účet organizace](single-sign-on/_static/image25.png)

![Přihlášen](single-sign-on/_static/image26.png)

<span data-ttu-id="71840-211">Když nasadíte aplikaci do Azure, je vybrat vše, co musíte udělat **povolit ověřování organizace** zaškrtněte políčko, a ještě jednou všechny konfigurace za vás postará o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="71840-211">When you deploy the app to Azure, all you have to do is select an **Enable Organizational Authentication** check box, and once again Visual Studio takes care of all the configuration for you.</span></span>

![Publikování webu](single-sign-on/_static/image27.png)

<span data-ttu-id="71840-213">Tyto snímky obrazovky pocházejí z úplný podrobný kurz, který ukazuje, jak vytvořit aplikaci, která používá ověřování Azure AD: [vývoj aplikací ASP.NET s použitím Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="71840-213">These screen shots come from a complete step-by-step tutorial that shows how to build an app that uses Azure AD authentication: [Developing ASP.NET Apps with Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).</span></span>

## <a name="summary"></a><span data-ttu-id="71840-214">Souhrn</span><span class="sxs-lookup"><span data-stu-id="71840-214">Summary</span></span>

<span data-ttu-id="71840-215">V této kapitole jste viděli, že Azure Active Directory, Visual Studio a technologie ASP.NET, umožňují snadno nastavit jednotné přihlašování v aplikacích Internet pro uživatelé ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="71840-215">In this chapter you saw that Azure Active Directory, Visual Studio, and ASP.NET, make it easy to set up single sign-on in Internet applications for your organization's users.</span></span> <span data-ttu-id="71840-216">Vaši uživatelé můžou přihlásit v aplikacích Internet pomocí stejných přihlašovacích údajů, které používají k přihlášení pomocí služby Active Directory v interní síti.</span><span class="sxs-lookup"><span data-stu-id="71840-216">Your users can sign on in Internet apps using the same credentials they use to sign on using Active Directory in your internal network.</span></span>

<span data-ttu-id="71840-217">[Další kapitolu](data-storage-options.md) zkoumá možnosti úložiště dat, která je k dispozici pro cloudové aplikace.</span><span class="sxs-lookup"><span data-stu-id="71840-217">The [next chapter](data-storage-options.md) looks at the data storage options available for a cloud app.</span></span>

<a id="resources"></a>
## <a name="resources"></a><span data-ttu-id="71840-218">Prostředky</span><span class="sxs-lookup"><span data-stu-id="71840-218">Resources</span></span>

<span data-ttu-id="71840-219">Další informace naleznete v následujících materiálech:</span><span class="sxs-lookup"><span data-stu-id="71840-219">For more information, see the following resources:</span></span>

- <span data-ttu-id="71840-220">[Dokumentace ke službě Azure Active Directory](https://docs.microsoft.com/azure/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="71840-220">[Azure Active Directory Documentation](https://docs.microsoft.com/azure/active-directory/).</span></span> <span data-ttu-id="71840-221">Stránka portálu dokumentace ke službě Azure AD na webu windowsazure.com.</span><span class="sxs-lookup"><span data-stu-id="71840-221">Portal page for Azure AD documentation on the windowsazure.com site.</span></span> <span data-ttu-id="71840-222">Podrobné pokyny krok za krokem, najdete v článku **vývoj** oddílu.</span><span class="sxs-lookup"><span data-stu-id="71840-222">For step by step tutorials, see the **Develop** section.</span></span>
- <span data-ttu-id="71840-223">[Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/).</span><span class="sxs-lookup"><span data-stu-id="71840-223">[Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/).</span></span> <span data-ttu-id="71840-224">Stránka portálu dokumentaci ke službě Multi-Factor authentication v Azure.</span><span class="sxs-lookup"><span data-stu-id="71840-224">Portal page for documentation about multi-factor authentication in Azure.</span></span>
- <span data-ttu-id="71840-225">[Možnosti ověřování účtu organizace](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions).</span><span class="sxs-lookup"><span data-stu-id="71840-225">[Organizational account authentication options](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions).</span></span> <span data-ttu-id="71840-226">Vysvětlení možností ověřování Azure AD v dialogovém okně Nový projekt sady Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="71840-226">Explanation of the Azure AD authentication options in the Visual Studio 2013 new-project dialog.</span></span>
- <span data-ttu-id="71840-227">[Microsoft Patterns and Practices - federované Identity vzor](https://msdn.microsoft.com/library/dn589790.aspx).</span><span class="sxs-lookup"><span data-stu-id="71840-227">[Microsoft Patterns and Practices - Federated Identity Pattern](https://msdn.microsoft.com/library/dn589790.aspx).</span></span>
- <span data-ttu-id="71840-228">[Postupy: Instalace nástroje Azure Active Directory Sync](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).</span><span class="sxs-lookup"><span data-stu-id="71840-228">[HowTo: Install the Azure Active Directory Sync Tool](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).</span></span>
- <span data-ttu-id="71840-229">[Active Directory Federation Services 2.0 mapa obsahu](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx).</span><span class="sxs-lookup"><span data-stu-id="71840-229">[Active Directory Federation Services 2.0 Content Map](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx).</span></span> <span data-ttu-id="71840-230">Obsahuje odkazy na dokumentaci ke službě AD FS 2.0.</span><span class="sxs-lookup"><span data-stu-id="71840-230">Links to documentation about ADFS 2.0.</span></span>
- <span data-ttu-id="71840-231">[Ověřování založené na rolích a na základě seznamu ACL v aplikaci Windows Azure AD](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1).</span><span class="sxs-lookup"><span data-stu-id="71840-231">[Role-Based and ACL-Based Authorization in a Windows Azure AD Application](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1).</span></span> <span data-ttu-id="71840-232">Ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="71840-232">Sample application.</span></span>
- <span data-ttu-id="71840-233">[Blog o Azure Active Directory Graph API](https://blogs.msdn.com/b/aadgraphteam/).</span><span class="sxs-lookup"><span data-stu-id="71840-233">[Azure Active Directory Graph API blog](https://blogs.msdn.com/b/aadgraphteam/).</span></span>
- <span data-ttu-id="71840-234">[Řízení přístupu v modelu BYOD a integrace adresáře v hybridní infrastrukturu identit](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=).</span><span class="sxs-lookup"><span data-stu-id="71840-234">[Access Control in BYOD and Directory Integration in a Hybrid Identity Infrastructure](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=).</span></span> <span data-ttu-id="71840-235">Odborný Ed 2014 relace videa podle Gayana Bagdasaryan.</span><span class="sxs-lookup"><span data-stu-id="71840-235">Tech Ed 2014 session video by Gayana Bagdasaryan.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="71840-236">[Předchozí](web-development-best-practices.md)
> [další](data-storage-options.md)</span><span class="sxs-lookup"><span data-stu-id="71840-236">[Previous](web-development-best-practices.md)
[Next](data-storage-options.md)</span></span>
