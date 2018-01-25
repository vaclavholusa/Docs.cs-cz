---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: "Jednotné přihlašování (vytváření reálných cloudových aplikací s Azure) | Microsoft Docs"
author: MikeWasson
description: "Cloudové aplikace skutečné World sestavení s Azure elektronická kniha je založena na prezentace vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které můžete mu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: b3640c94a8ae9ede330c0fe6a392acb5843cb65c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a><span data-ttu-id="1dec0-104">Jednotné přihlašování (vytváření reálných cloudových aplikací s Azure)</span><span class="sxs-lookup"><span data-stu-id="1dec0-104">Single Sign-On (Building Real-World Cloud Apps with Azure)</span></span>
====================
<span data-ttu-id="1dec0-105">podle [Karel Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [tní Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="1dec0-105">by [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="1dec0-106">[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronická kniha](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="1dec0-106">[Download Fix It Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) or [Download E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span></span>

> <span data-ttu-id="1dec0-107">**Vytváření reálného světa cloudových aplikací s Azure** elektronická kniha je založena na prezentaci vyvinuté Scott Guthrie.</span><span class="sxs-lookup"><span data-stu-id="1dec0-107">The **Building Real World Cloud Apps with Azure** e-book is based on a presentation developed by Scott Guthrie.</span></span> <span data-ttu-id="1dec0-108">Vysvětluje 13 vzory a postupy, které vám mohou pomoci být úspěšná, vývoj webových aplikací pro cloud.</span><span class="sxs-lookup"><span data-stu-id="1dec0-108">It explains 13 patterns and practices that can help you be successful developing web apps for the cloud.</span></span> <span data-ttu-id="1dec0-109">Informace o elektronická kniha najdete v tématu [první kapitoly](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1dec0-109">For information about the e-book, see [the first chapter](introduction.md).</span></span>


<span data-ttu-id="1dec0-110">Existuje mnoho problémy se zabezpečením myslet, když vyvíjíte cloudové aplikace, ale pro tuto řadu budete zaměříme na jenom jednom: jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1dec0-110">There are many security issues to think about when you're developing a cloud app, but for this series we'll focus on just one: single sign-on.</span></span> <span data-ttu-id="1dec0-111">Lidé otázku klást, je toto: "I se primárně vytváření aplikací pro zaměstnance Moje společnost; jak hostování těchto aplikací v cloudu a stále povolit, aby uživatelé používali stejný model zabezpečení, Moje zaměstnanci znáte a používat v místním prostředí, pokud jejich používáte systém aplikací, které jsou hostovány v bráně firewall?"</span><span class="sxs-lookup"><span data-stu-id="1dec0-111">A question people often ask is this: "I'm primarily building apps for the employees of my company; how do I host these apps in the cloud and still enable them to use the same security model that my employees know and use in the on-premises environment when they're running apps that are hosted inside the firewall?"</span></span> <span data-ttu-id="1dec0-112">Jeden ze způsobů, jak jsme povolit tento scénář se nazývá Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1dec0-112">One of the ways we enable this scenario is called Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="1dec0-113">Azure AD umožňuje zpřístupnit enterprise-obchodní (LOB) aplikacím přes Internet a umožňuje vám umožní provádět tyto aplikace k dispozici také obchodními partnery.</span><span class="sxs-lookup"><span data-stu-id="1dec0-113">Azure AD enables you to make enterprise line-of-business (LOB) apps available over the Internet, and it enables you to make these apps available to business partners as well.</span></span>

## <a name="introduction-to-azure-ad"></a><span data-ttu-id="1dec0-114">Úvod do služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="1dec0-114">Introduction to Azure AD</span></span>

<span data-ttu-id="1dec0-115">[Azure AD](https://docs.microsoft.com/azure/active-directory/) poskytuje [služby Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) v cloudu.</span><span class="sxs-lookup"><span data-stu-id="1dec0-115">[Azure AD](https://docs.microsoft.com/azure/active-directory/) provides [Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) in the cloud.</span></span> <span data-ttu-id="1dec0-116">Klíčové funkce patří:</span><span class="sxs-lookup"><span data-stu-id="1dec0-116">Key features include the following:</span></span>

- <span data-ttu-id="1dec0-117">Se integruje s místní služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1dec0-117">It integrates with on-premises Active Directory.</span></span>
- <span data-ttu-id="1dec0-118">Umožňuje jednotné přihlašování s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="1dec0-118">It enables single sign-on with your apps.</span></span>
- <span data-ttu-id="1dec0-119">Podporuje otevřený standardy, jako [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Fed](http://en.wikipedia.org/wiki/WS-Federation), a [OAuth 2.0](http://oauth.net/2/).</span><span class="sxs-lookup"><span data-stu-id="1dec0-119">It supports open standards such as [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Fed](http://en.wikipedia.org/wiki/WS-Federation), and [OAuth 2.0](http://oauth.net/2/).</span></span>
- <span data-ttu-id="1dec0-120">Podporuje Enterprise [grafu REST API](https://msdn.microsoft.com/library/hh974476.aspx).</span><span class="sxs-lookup"><span data-stu-id="1dec0-120">It supports Enterprise [Graph REST API](https://msdn.microsoft.com/library/hh974476.aspx).</span></span>

<span data-ttu-id="1dec0-121">Předpokládejme, že máte místní prostředí Windows Server Active Directory, které používáte k povolení zaměstnanci k přihlášení do intranetu aplikací:</span><span class="sxs-lookup"><span data-stu-id="1dec0-121">Suppose you have an on-premises Windows Server Active Directory environment that you use to enable employees to sign on to Intranet apps:</span></span>

![](single-sign-on/_static/image1.png)

<span data-ttu-id="1dec0-122">Co Azure AD můžete to udělat, je vytvoření adresáře v cloudu.</span><span class="sxs-lookup"><span data-stu-id="1dec0-122">What Azure AD enables you to do is create a directory in the cloud.</span></span> <span data-ttu-id="1dec0-123">Je bezplatné funkce a snadno nastavit.</span><span class="sxs-lookup"><span data-stu-id="1dec0-123">It's a free feature and easy to set up.</span></span>

<span data-ttu-id="1dec0-124">Může být zcela nezávislé z vaší místní služby Active Directory; můžete vložit každý, kdo má v něm a jejich ověření v aplikacích Internet.</span><span class="sxs-lookup"><span data-stu-id="1dec0-124">It can be entirely independent from your on-premises Active Directory; you can put anyone you want in it and authenticate them in Internet apps.</span></span>

![Windows Azure Active Directory](single-sign-on/_static/image2.png)

<span data-ttu-id="1dec0-126">Nebo můžete službu integrovat s místní AD.</span><span class="sxs-lookup"><span data-stu-id="1dec0-126">Or you can integrate it with your on-premises AD.</span></span>

![AD a službou WAAD integrace](single-sign-on/_static/image3.png)

<span data-ttu-id="1dec0-128">Nyní všechny zaměstnance, kteří můžou ověřovat na místě, můžete také ověřovat přes Internet – bez nutnosti otevřít brány firewall nebo nasadit všechny nové servery ve vašem datovém centru.</span><span class="sxs-lookup"><span data-stu-id="1dec0-128">Now all the employees who can authenticate on-premises can also authenticate over the Internet – without you having to open up a firewall or deploy any new servers in your data center.</span></span> <span data-ttu-id="1dec0-129">Můžete nadále využívat všechny existující služby Active Directory prostředí, které znáte a používají. jednotlivá poskytnout vaše interní aplikace jednotné přihlašování na funkce.</span><span class="sxs-lookup"><span data-stu-id="1dec0-129">You can continue to leverage all the existing Active Directory environment that you know and use today to give your internal apps single-sign on capability.</span></span>

<span data-ttu-id="1dec0-130">Jakmile jste udělali toto připojení mezi AD a Azure AD, můžete také povolit vašich webových aplikací a mobilních zařízení k ověření zaměstnancům v cloudu a můžete povolit aplikacím třetích stran, jako je například aplikace Office 365 a SalesForce.com, Google, tak, aby přijímal vaší přihlašovací údaje zaměstnanců.</span><span class="sxs-lookup"><span data-stu-id="1dec0-130">Once you've made this connection between AD and Azure AD, you can also enable your web apps and your mobile devices to authenticate your employees in the cloud, and you can enable third-party apps, such as Office 365, SalesForce.com, or Google apps, to accept your employees' credentials.</span></span> <span data-ttu-id="1dec0-131">Pokud používáte Office 365, již nastavení s Azure AD protože Office 365 používá Azure AD pro ověřování a autorizaci.</span><span class="sxs-lookup"><span data-stu-id="1dec0-131">If you're using Office 365, you're already set up with Azure AD because Office 365 uses Azure AD for authentication and authorization.</span></span>

![3. stran aplikace](single-sign-on/_static/image4.png)

<span data-ttu-id="1dec0-133">Výhodou tohoto přístupu je, že kdykoli organizaci přidá nebo odstraní uživatele, nebo uživatel změní heslo, použijte stejný proces, který používáte dnes v místním prostředí.</span><span class="sxs-lookup"><span data-stu-id="1dec0-133">The beauty of this approach is that any time your organization adds or deletes a user, or a user changes a password, you use the same process that you use today in your on-premises environment.</span></span> <span data-ttu-id="1dec0-134">Všechny z místní AD změny rozšířeny automaticky cloudového prostředí.</span><span class="sxs-lookup"><span data-stu-id="1dec0-134">All of your on-premises AD changes are automatically propagated to the cloud environment.</span></span>

<span data-ttu-id="1dec0-135">Pokud vaše společnost používá nebo přesun do služeb Office 365 je dobrá zpráva, že budete mít Azure AD nastavit automaticky protože Office 365 používá Azure AD pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="1dec0-135">If your company is using or moving to Office 365, the good news is that you'll have Azure AD set up automatically because Office 365 uses Azure AD for authentication.</span></span> <span data-ttu-id="1dec0-136">Abyste mohli snadno používat ve svých vlastních aplikacích stejné ověřování, který používá Office 365.</span><span class="sxs-lookup"><span data-stu-id="1dec0-136">So you can easily use in your own apps the same authentication that Office 365 uses.</span></span>

## <a name="set-up-an-azure-ad-tenant"></a><span data-ttu-id="1dec0-137">Nastavení klienta služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="1dec0-137">Set up an Azure AD tenant</span></span>

<span data-ttu-id="1dec0-138">adresář služby Azure AD se nazývá na Azure AD [klienta](https://technet.microsoft.com/library/jj573650.aspx), a nastavení klienta je velmi snadné.</span><span class="sxs-lookup"><span data-stu-id="1dec0-138">an Azure AD directory is called an Azure AD [tenant](https://technet.microsoft.com/library/jj573650.aspx), and setting up a tenant is pretty easy.</span></span> <span data-ttu-id="1dec0-139">Ukážeme vám jak se provádí v portálu pro správu Azure k objasnění konceptů, ale samozřejmě jako dalších funkcí portálu můžete provést také ho pomocí skriptu nebo rozhraní API pro správu.</span><span class="sxs-lookup"><span data-stu-id="1dec0-139">We'll show you how it's done in the Azure Management Portal in order to illustrate the concepts, but of course like the other portal functions you can also do it by using a script or management API.</span></span>

<span data-ttu-id="1dec0-140">V portálu pro správu klikněte na kartu služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1dec0-140">In the management portal click the Active Directory tab.</span></span>

![Službou WAAD portálu](single-sign-on/_static/image5.png)

<span data-ttu-id="1dec0-142">Máte jednoho klienta Azure AD automaticky k účtu Azure, a můžete kliknout na **přidat** tlačítko v dolní části stránky a vytvářet další adresáře.</span><span class="sxs-lookup"><span data-stu-id="1dec0-142">You automatically have one Azure AD tenant for your Azure account, and you can click the **Add** button at the bottom of the page to create additional directories.</span></span> <span data-ttu-id="1dec0-143">Měli byste jeden pro testovací prostředí a jeden pro produkční prostředí, například.</span><span class="sxs-lookup"><span data-stu-id="1dec0-143">You might want one for a test environment and one for production, for example.</span></span> <span data-ttu-id="1dec0-144">Vezměte v úvahu pečlivě co zadáte název nového adresáře.</span><span class="sxs-lookup"><span data-stu-id="1dec0-144">Think carefully about what you name a new directory.</span></span> <span data-ttu-id="1dec0-145">Pokud použijete název adresáře a pak použijete název znovu pro uživatele, který může být matoucí.</span><span class="sxs-lookup"><span data-stu-id="1dec0-145">If you use your name for the directory and then you use your name again for one of the users, that can be confusing.</span></span>

![Přidejte adresář](single-sign-on/_static/image6.png)

<span data-ttu-id="1dec0-147">Portál obsahuje plnou podporu pro vytváření, odstraňování a Správa uživatelů v tomto prostředí.</span><span class="sxs-lookup"><span data-stu-id="1dec0-147">The portal has full support for creating, deleting, and managing users within this environment.</span></span> <span data-ttu-id="1dec0-148">Například pokud chcete přidat uživatele přejděte na **uživatelé** a klikněte **přidat uživatele** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1dec0-148">For example, to add a user go to the **Users** tab and click the **Add User** button.</span></span>

![Tlačítko Přidat uživatele](single-sign-on/_static/image7.png)

![Přidat uživatele – dialogové okno](single-sign-on/_static/image8.png)

<span data-ttu-id="1dec0-151">Můžete vytvořit nového uživatele, který se nachází pouze v tomto adresáři nebo Account Microsoft můžete zaregistrovat jako uživatel v tomto adresáři nebo zaregistrovat nebo uživatele z jiného adresáře služby Azure AD jako uživatel v tomto adresáři.</span><span class="sxs-lookup"><span data-stu-id="1dec0-151">You can create a new user who exists only in this directory, or you can register a Microsoft Account as a user in this directory, or register or a user from another Azure AD directory as a user in this directory.</span></span> <span data-ttu-id="1dec0-152">(V skutečné adresáři, jako výchozí doménu bude ContosoTest.onmicrosoft.com. Můžete také použít doménu podle vlastní volby, například contoso.com.)</span><span class="sxs-lookup"><span data-stu-id="1dec0-152">(In a real directory, the default domain would be ContosoTest.onmicrosoft.com. You can also use a domain of your own choosing, like contoso.com.)</span></span>

![Typy uživatelů](single-sign-on/_static/image9.png)

![Přidat uživatele – dialogové okno](single-sign-on/_static/image10.png)

<span data-ttu-id="1dec0-155">Můžete přiřadit uživatele k roli.</span><span class="sxs-lookup"><span data-stu-id="1dec0-155">You can assign the user to a role.</span></span>

![Profil uživatele](single-sign-on/_static/image11.png)

<span data-ttu-id="1dec0-157">A účet je vytvořen s dočasným heslem.</span><span class="sxs-lookup"><span data-stu-id="1dec0-157">And the account is created with a temporary password.</span></span>

![Dočasné heslo](single-sign-on/_static/image12.png)

<span data-ttu-id="1dec0-159">Uživatelé, vytvoříte tímto způsobem okamžitě přihlásit vašich webových aplikací pomocí tohoto cloudu.</span><span class="sxs-lookup"><span data-stu-id="1dec0-159">The users you create this way can immediately log in to your web apps using this cloud directory.</span></span>

<span data-ttu-id="1dec0-160">Co je výborně hodí pro podnikové jednotné přihlašování, ale je **integrace adresáře** karty:</span><span class="sxs-lookup"><span data-stu-id="1dec0-160">What's great for enterprise single sign-on, though, is the **Directory Integration** tab:</span></span>

![Karta integrace adresáře](single-sign-on/_static/image13.png)

<span data-ttu-id="1dec0-162">Pokud povolíte integraci adresáře a [stáhněte si nástroj](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), budete moct synchronizovat tento adresář cloudu s vaší stávající místní služby Active Directory, kterou používáte už ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="1dec0-162">If you enable directory integration, and [download a tool](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), you can sync this cloud directory with your existing on-premises Active Directory that you're already using inside your organization.</span></span> <span data-ttu-id="1dec0-163">Potom všechny uživatele uložené v adresáři se zobrazí v tomto adresáři cloudu.</span><span class="sxs-lookup"><span data-stu-id="1dec0-163">Then all of the users stored in your directory will show up in this cloud directory.</span></span> <span data-ttu-id="1dec0-164">Cloudové aplikace, můžete nyní ověřovat všechny zaměstnance pomocí přihlašovacích existující služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1dec0-164">Your cloud apps can now authenticate all of your employees using their existing Active Directory credentials.</span></span> <span data-ttu-id="1dec0-165">A všechny, to je volné – nástroj pro synchronizaci i Azure AD, sám sebe.</span><span class="sxs-lookup"><span data-stu-id="1dec0-165">And all this is free – both the sync tool and Azure AD itself.</span></span>

<span data-ttu-id="1dec0-166">Tento nástroj je průvodce, který se snadno používá, jak je vidět na tyto snímky obrazovky.</span><span class="sxs-lookup"><span data-stu-id="1dec0-166">The tool is a wizard that is easy to use, as you can see from these screen shots.</span></span> <span data-ttu-id="1dec0-167">Tyto nejsou úplné pokyny, jenom jako příklad ukazuje základní proces.</span><span class="sxs-lookup"><span data-stu-id="1dec0-167">These are not complete instructions, just an example showing you the basic process.</span></span> <span data-ttu-id="1dec0-168">Podrobné informace how-k-proveďte it, najdete v článku odkazy v [prostředky](#resources) části na konci kapitoly.</span><span class="sxs-lookup"><span data-stu-id="1dec0-168">For more detailed how-to-do-it information, see the links in the [Resources](#resources) section at the end of the chapter.</span></span>

![Průvodce konfigurací nástroje synchronizace službou WAAD](single-sign-on/_static/image14.png)

<span data-ttu-id="1dec0-170">Klikněte na tlačítko **Další**a pak zadejte svoje přihlašovací údaje služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1dec0-170">Click **Next**, and then enter your Azure Active Directory credentials.</span></span>

![Průvodce konfigurací nástroje synchronizace službou WAAD](single-sign-on/_static/image15.png)

<span data-ttu-id="1dec0-172">Klikněte na tlačítko **Další**a pak zadejte místní přihlašovací údaje služby AD.</span><span class="sxs-lookup"><span data-stu-id="1dec0-172">Click **Next**, and then enter your on-premises AD credentials.</span></span>

![Průvodce konfigurací nástroje synchronizace službou WAAD](single-sign-on/_static/image16.png)

<span data-ttu-id="1dec0-174">Klikněte na tlačítko **Další**a pak vyberte, pokud chcete ukládání klíče hash hesla služby AD v cloudu.</span><span class="sxs-lookup"><span data-stu-id="1dec0-174">Click **Next**, and then indicate if you want to store a hash of your AD passwords in the cloud.</span></span>

![Průvodce konfigurací nástroje synchronizace službou WAAD](single-sign-on/_static/image17.png)

<span data-ttu-id="1dec0-176">Hodnota hash hesla, který chcete uložit v cloudu je jednosměrná hodnota hash; skutečných hesel nikdy ukládají ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1dec0-176">The password hash that you can store in the cloud is a one-way hash; actual passwords are never stored in Azure AD.</span></span> <span data-ttu-id="1dec0-177">Pokud se rozhodnete pro ukládání hodnot hash v cloudu, budete muset použít [Active Directory Federation Services](https://technet.microsoft.com/library/hh831502.aspx) (ADFS).</span><span class="sxs-lookup"><span data-stu-id="1dec0-177">If you decide against storing hashes in the cloud, you'll have to use [Active Directory Federation Services](https://technet.microsoft.com/library/hh831502.aspx) (ADFS).</span></span> <span data-ttu-id="1dec0-178">Existují také [dalších faktorů, které je třeba zvážit při výběru, zda se má používat služba AD FS](https://technet.microsoft.com/library/jj573653.aspx).</span><span class="sxs-lookup"><span data-stu-id="1dec0-178">There are also [other factors to consider when choosing whether or not to use ADFS](https://technet.microsoft.com/library/jj573653.aspx).</span></span> <span data-ttu-id="1dec0-179">Možnosti služby AD FS vyžaduje několik další konfigurační kroky.</span><span class="sxs-lookup"><span data-stu-id="1dec0-179">The ADFS option requires a few additional configuration steps.</span></span>

<span data-ttu-id="1dec0-180">Pokud se rozhodnete ukládat hodnoty hash v cloudu, s tím budete hotovi a spustí nástroj pro synchronizaci adresářů, po kliknutí na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="1dec0-180">If you choose to store hashes in the cloud, you're done, and the tool starts synchronizing directories when you click **Next**.</span></span>

![Průvodce konfigurací nástroje synchronizace službou WAAD](single-sign-on/_static/image18.png)

<span data-ttu-id="1dec0-182">A za pár minut jste hotovi.</span><span class="sxs-lookup"><span data-stu-id="1dec0-182">And in a few minutes you're done.</span></span>

![Průvodce konfigurací nástroje synchronizace službou WAAD](single-sign-on/_static/image19.png)

<span data-ttu-id="1dec0-184">Potřebujete ji spustit na jednom řadiči domény v organizaci, v systému Windows 2003 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="1dec0-184">You only have to run this on one domain controller in the organization, on Windows 2003 or higher.</span></span> <span data-ttu-id="1dec0-185">A není nutné restartovat.</span><span class="sxs-lookup"><span data-stu-id="1dec0-185">And no need to reboot.</span></span> <span data-ttu-id="1dec0-186">Když jste hotovi, všichni uživatelé jsou v cloudu a můžete provést jednotné přihlašování v jakémkoli web nebo mobilní aplikace, pomocí SAML, WS-Fed nebo OAuth.</span><span class="sxs-lookup"><span data-stu-id="1dec0-186">When you're done, all of your users are in the cloud and you can do single sign-on from any web or mobile application, using SAML, OAuth, or WS-Fed.</span></span>

<span data-ttu-id="1dec0-187">Někdy jsme získat vyzváni, o tom, jak zabezpečené jde – Microsoft používá ho pro své vlastní citlivých obchodních dat?</span><span class="sxs-lookup"><span data-stu-id="1dec0-187">Sometimes we get asked about how secure this is – does Microsoft use it for their own sensitive business data?</span></span> <span data-ttu-id="1dec0-188">A odpovíte kladně, které jsme provést.</span><span class="sxs-lookup"><span data-stu-id="1dec0-188">And the answer is yes we do.</span></span> <span data-ttu-id="1dec0-189">Například, pokud přejdete na interní web Microsoft SharePoint na [https://microsoft.sharepoint.com/](https://microsoft.sharepoint.com/), získat zobrazí výzva k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="1dec0-189">For example, if you go to the internal Microsoft SharePoint site at [https://microsoft.sharepoint.com/](https://microsoft.sharepoint.com/), you get prompted to log in.</span></span>

![Přihlášení k Office 365](single-sign-on/_static/image20.png)

<span data-ttu-id="1dec0-191">Microsoft má povolenou službou AD FS, takže když zadáte ID společnosti Microsoft, budete přesměrováni na přihlašovací stránku služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="1dec0-191">Microsoft has enabled ADFS, so when you enter a Microsoft ID, you get redirected to an ADFS log-in page.</span></span>

![Přihlášení služby AD FS](single-sign-on/_static/image21.png)

<span data-ttu-id="1dec0-193">A až zadáte přihlašovací údaje uložené v interní účet Microsoft AD, budete mít přístup k této interní aplikace.</span><span class="sxs-lookup"><span data-stu-id="1dec0-193">And once you enter credentials stored in an internal Microsoft AD account, you have access to this internal application.</span></span>

![Server MS SharePoint](single-sign-on/_static/image22.png)

<span data-ttu-id="1dec0-195">Používáme přihlášení serveru služby AD, především, protože jsme už měli ADFS nastavit před Azure AD jsou k dispozici, ale proces přihlášení prochází přes adresář služby Azure AD v cloudu.</span><span class="sxs-lookup"><span data-stu-id="1dec0-195">We're using an AD sign-in server mainly because we already had ADFS set up before Azure AD became available, but the log-in process is going through an Azure AD directory in the cloud.</span></span> <span data-ttu-id="1dec0-196">Nemůžeme vložit naše důležité dokumenty, Správa zdrojového kódu, soubory správy výkonu, prodejní sestavy a další v cloudu a používají k zabezpečení je toto přesně stejnou řešení.</span><span class="sxs-lookup"><span data-stu-id="1dec0-196">We put our important documents, source control, performance management files, sales reports, and more, in the cloud and are using this exact same solution to secure them.</span></span>

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a><span data-ttu-id="1dec0-197">Vytvořit aplikaci ASP.NET, která používá Azure AD pro jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="1dec0-197">Create an ASP.NET app that uses Azure AD for single sign-on</span></span>

<span data-ttu-id="1dec0-198">Visual Studio skutečně usnadňuje vytvořit aplikaci, která používá Azure AD pro jednotné přihlašování, jak je vidět z několika snímky obrazovky.</span><span class="sxs-lookup"><span data-stu-id="1dec0-198">Visual Studio makes it really easy to create an app that uses Azure AD for single sign-on, as you can see from a few screen shots.</span></span>

<span data-ttu-id="1dec0-199">Když vytvoříte novou aplikaci ASP.NET, MVC ani webové formuláře, je výchozí metodou ověřování ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="1dec0-199">When you create a new ASP.NET application, either MVC or Web Forms, the default authentication method is ASP.NET Identity.</span></span> <span data-ttu-id="1dec0-200">Pokud chcete změnit, do Azure AD, kliknutí na tlačítko **změnit ověřování** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1dec0-200">To change that to Azure AD, you click a **Change Authentication** button.</span></span>

![Změna ověřování](single-sign-on/_static/image23.png)

<span data-ttu-id="1dec0-202">Vyberte účty organizace, zadejte název domény a pak vybrat jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1dec0-202">Select Organizational Accounts, enter your domain name, and then select Single Sign On.</span></span>

![Konfigurace ověřování dialogové okno](single-sign-on/_static/image24.png)

<span data-ttu-id="1dec0-204">Můžete také číst aplikace nebo pro čtení a zápis oprávnění pro adresář data.</span><span class="sxs-lookup"><span data-stu-id="1dec0-204">You can also give the app read or read/write permission for directory data.</span></span> <span data-ttu-id="1dec0-205">Pokud tak učiníte, můžete použít [REST API služby Azure Graph](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) k vyhledání uživatelů telefonní číslo, zjistěte, pokud jsou v kanceláři, při posledním přihlášení na atd.</span><span class="sxs-lookup"><span data-stu-id="1dec0-205">If you do that, it can use the [Azure Graph REST API](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) to look up users' phone number, find out if they're in the office, when they last logged on, etc.</span></span>

<span data-ttu-id="1dec0-206">Který je všechno, co musíte udělat – Visual Studio vyzve k zadání pověření pro správce pro vašeho tenanta Azure AD, a poté konfiguruje váš projekt a klientovi Azure AD pro novou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1dec0-206">That's all you have to do - Visual Studio asks for the credentials for an administrator for your Azure AD tenant, and then it configures both your project and your Azure AD tenant for the new application.</span></span>

<span data-ttu-id="1dec0-207">Při spuštění projektu, se zobrazí na přihlašovací stránku a můžete se přihlásit pomocí přihlašovacích údajů uživatele v adresáři služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1dec0-207">When you run the project, you'll see a sign-in page, and you can sign in with credentials of a user in your Azure AD directory.</span></span>

![Přihlášení účtu organizace](single-sign-on/_static/image25.png)

![Přihlášení](single-sign-on/_static/image26.png)

<span data-ttu-id="1dec0-210">Když nasadíte aplikaci do Azure, stačí je vyberte **povolit ověřování organizace** zaškrtněte políčko a ještě jednou Visual Studio postará všechny konfigurace pro vás.</span><span class="sxs-lookup"><span data-stu-id="1dec0-210">When you deploy the app to Azure, all you have to do is select an **Enable Organizational Authentication** check box, and once again Visual Studio takes care of all the configuration for you.</span></span>

![Průvodci Publikovat Web](single-sign-on/_static/image27.png)

<span data-ttu-id="1dec0-212">Tyto snímky obrazovky pocházet z úplný podrobný návod, jak sestavit aplikaci, která používá ověřování Azure AD: [vývoj aplikace ASP.NET se službou Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="1dec0-212">These screen shots come from a complete step-by-step tutorial that shows how to build an app that uses Azure AD authentication: [Developing ASP.NET Apps with Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).</span></span>

## <a name="summary"></a><span data-ttu-id="1dec0-213">Souhrn</span><span class="sxs-lookup"><span data-stu-id="1dec0-213">Summary</span></span>

<span data-ttu-id="1dec0-214">V této kapitole jste viděli, že Azure Active Directory, Visual Studio a technologii ASP.NET, můžete snadno nastavit jednotné přihlašování v internetových aplikací pro uživatele ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="1dec0-214">In this chapter you saw that Azure Active Directory, Visual Studio, and ASP.NET, make it easy to set up single sign-on in Internet applications for your organization's users.</span></span> <span data-ttu-id="1dec0-215">Vaši uživatelé přihlásit Internetu aplikací pomocí stejných přihlašovacích údajů, které používají k přihlášení pomocí služby Active Directory v interní síti.</span><span class="sxs-lookup"><span data-stu-id="1dec0-215">Your users can sign on in Internet apps using the same credentials they use to sign on using Active Directory in your internal network.</span></span>

<span data-ttu-id="1dec0-216">[Další kapitoly](data-storage-options.md) zjistí možnosti úložiště dat, která je k dispozici pro cloudové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1dec0-216">The [next chapter](data-storage-options.md) looks at the data storage options available for a cloud app.</span></span>

<a id="resources"></a>
## <a name="resources"></a><span data-ttu-id="1dec0-217">Prostředky</span><span class="sxs-lookup"><span data-stu-id="1dec0-217">Resources</span></span>

<span data-ttu-id="1dec0-218">Další informace naleznete v následujících materiálech:</span><span class="sxs-lookup"><span data-stu-id="1dec0-218">For more information, see the following resources:</span></span>

- <span data-ttu-id="1dec0-219">[Dokumentaci ke službě Azure Active Directory](https://docs.microsoft.com/azure/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="1dec0-219">[Azure Active Directory Documentation](https://docs.microsoft.com/azure/active-directory/).</span></span> <span data-ttu-id="1dec0-220">Stránky portálu pro Azure AD dokumentaci na webu windowsazure.com.</span><span class="sxs-lookup"><span data-stu-id="1dec0-220">Portal page for Azure AD documentation on the windowsazure.com site.</span></span> <span data-ttu-id="1dec0-221">Podrobné pokyny krok za krokem, najdete v článku **vývoj** části.</span><span class="sxs-lookup"><span data-stu-id="1dec0-221">For step by step tutorials, see the **Develop** section.</span></span>
- <span data-ttu-id="1dec0-222">[Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/).</span><span class="sxs-lookup"><span data-stu-id="1dec0-222">[Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/).</span></span> <span data-ttu-id="1dec0-223">Stránky portálu pro dokumentaci o službě Multi-Factor authentication v Azure.</span><span class="sxs-lookup"><span data-stu-id="1dec0-223">Portal page for documentation about multi-factor authentication in Azure.</span></span>
- <span data-ttu-id="1dec0-224">[Možnosti ověřování účtu organizace](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions).</span><span class="sxs-lookup"><span data-stu-id="1dec0-224">[Organizational account authentication options](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions).</span></span> <span data-ttu-id="1dec0-225">Vysvětlení možností ověřování Azure AD v dialogu Nový projekt Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="1dec0-225">Explanation of the Azure AD authentication options in the Visual Studio 2013 new-project dialog.</span></span>
- <span data-ttu-id="1dec0-226">[Microsoft Patterns and Practices - federované Identity vzor](https://msdn.microsoft.com/library/dn589790.aspx).</span><span class="sxs-lookup"><span data-stu-id="1dec0-226">[Microsoft Patterns and Practices - Federated Identity Pattern](https://msdn.microsoft.com/library/dn589790.aspx).</span></span>
- <span data-ttu-id="1dec0-227">[Postupy: Instalace nástroje Azure Active Directory Sync](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).</span><span class="sxs-lookup"><span data-stu-id="1dec0-227">[HowTo: Install the Azure Active Directory Sync Tool](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).</span></span>
- <span data-ttu-id="1dec0-228">[Active Directory Federation Services 2.0 mapa obsahu](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx).</span><span class="sxs-lookup"><span data-stu-id="1dec0-228">[Active Directory Federation Services 2.0 Content Map](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx).</span></span> <span data-ttu-id="1dec0-229">Odkazy na dokumentaci o služby AD FS 2.0.</span><span class="sxs-lookup"><span data-stu-id="1dec0-229">Links to documentation about ADFS 2.0.</span></span>
- <span data-ttu-id="1dec0-230">[Ověřování na základě rolí a na základě seznamu ACL v aplikaci Windows Azure AD](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1).</span><span class="sxs-lookup"><span data-stu-id="1dec0-230">[Role-Based and ACL-Based Authorization in a Windows Azure AD Application](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1).</span></span> <span data-ttu-id="1dec0-231">Ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1dec0-231">Sample application.</span></span>
- <span data-ttu-id="1dec0-232">[Blog Azure Active Directory Graph API](https://blogs.msdn.com/b/aadgraphteam/).</span><span class="sxs-lookup"><span data-stu-id="1dec0-232">[Azure Active Directory Graph API blog](https://blogs.msdn.com/b/aadgraphteam/).</span></span>
- <span data-ttu-id="1dec0-233">[Řízení přístupu v modelu BYOD a integrace adresáře v hybridní infrastrukturu identit](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=).</span><span class="sxs-lookup"><span data-stu-id="1dec0-233">[Access Control in BYOD and Directory Integration in a Hybrid Identity Infrastructure](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=).</span></span> <span data-ttu-id="1dec0-234">Technická Ed 2014 relace video Gayana Bagdasaryan.</span><span class="sxs-lookup"><span data-stu-id="1dec0-234">Tech Ed 2014 session video by Gayana Bagdasaryan.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="1dec0-235">[Předchozí](web-development-best-practices.md)
[další](data-storage-options.md)</span><span class="sxs-lookup"><span data-stu-id="1dec0-235">[Previous](web-development-best-practices.md)
[Next](data-storage-options.md)</span></span>
