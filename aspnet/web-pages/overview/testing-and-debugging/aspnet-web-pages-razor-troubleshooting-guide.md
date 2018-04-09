---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: ASP.NET – webové stránky průvodce řešením potíží (Razor) | Microsoft Docs
author: tfitzmac
description: Tento článek popisuje problémy, které by mohly mít při práci s webových stránek ASP.NET (Razor) a některé navrhovanými řešeními. Verze softwaru ASP.NET Web Pag...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: ec51169ccea0016712de3fdb28a16a174150a8bd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-pages-razor-troubleshooting-guide"></a><span data-ttu-id="97ae7-104">ASP.NET – webové stránky průvodce řešením potíží (Razor)</span><span class="sxs-lookup"><span data-stu-id="97ae7-104">ASP.NET Web Pages (Razor) Troubleshooting Guide</span></span>
====================
<span data-ttu-id="97ae7-105">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="97ae7-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="97ae7-106">Tento článek popisuje problémy, které by mohly mít při práci s webových stránek ASP.NET (Razor) a některé navrhovanými řešeními.</span><span class="sxs-lookup"><span data-stu-id="97ae7-106">This article describes issues that you might have when working with ASP.NET Web Pages (Razor) and some suggested solutions.</span></span>
> 
> ## <a name="software-versions"></a><span data-ttu-id="97ae7-107">Verze softwaru</span><span class="sxs-lookup"><span data-stu-id="97ae7-107">Software versions</span></span>
> 
> 
> - <span data-ttu-id="97ae7-108">Rozhraní ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="97ae7-108">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="97ae7-109">V tomto kurzu taky spolupracuje se službou ASP.NET Web Pages 2 a ASP.NET Web Pages 1.0.</span><span class="sxs-lookup"><span data-stu-id="97ae7-109">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="97ae7-110">Toto téma obsahuje následující oddíly:</span><span class="sxs-lookup"><span data-stu-id="97ae7-110">This topic contains the following sections:</span></span>

- [<span data-ttu-id="97ae7-111">Problémy se spouštěním stránky</span><span class="sxs-lookup"><span data-stu-id="97ae7-111">Issues with Running Pages</span></span>](#Issues_Running_.cshtml_Pages)
- [<span data-ttu-id="97ae7-112">Problémy s kódu Razor</span><span class="sxs-lookup"><span data-stu-id="97ae7-112">Issues with Razor Code</span></span>](#IssuesWithRazorCode)
- [<span data-ttu-id="97ae7-113">Problémy s členství a zabezpečení</span><span class="sxs-lookup"><span data-stu-id="97ae7-113">Issues with Security and Membership</span></span>](#membership)
- [<span data-ttu-id="97ae7-114">Problémy s e-mailem</span><span class="sxs-lookup"><span data-stu-id="97ae7-114">Issues with Sending Email</span></span>](#email)
- [<span data-ttu-id="97ae7-115">Další zdroje informací</span><span class="sxs-lookup"><span data-stu-id="97ae7-115">Additional Resources</span></span>](#AdditionalResources)

<span data-ttu-id="97ae7-116">Obecné otázky, najdete v části [rozhraní ASP.NET Web Pages (Razor) – nejčastější dotazy](https://go.microsoft.com/fwlink/?LinkId=253000).</span><span class="sxs-lookup"><span data-stu-id="97ae7-116">For general questions, see [ASP.NET Web Pages (Razor) FAQ](https://go.microsoft.com/fwlink/?LinkId=253000).</span></span>

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a><span data-ttu-id="97ae7-117">Problémy se spouštěním stránky</span><span class="sxs-lookup"><span data-stu-id="97ae7-117">Issues with Running Pages</span></span>

<span data-ttu-id="97ae7-118">Můžete zabránit celou řadu problémů *.cshtml* a *.vbhtml* stránky z správně spuštěna.</span><span class="sxs-lookup"><span data-stu-id="97ae7-118">A variety of issues can prevent *.cshtml* and *.vbhtml* pages from running properly.</span></span> <span data-ttu-id="97ae7-119">V této části jsou uvedeny běžné chybové zprávy a pravděpodobně způsobí, že.</span><span class="sxs-lookup"><span data-stu-id="97ae7-119">This section lists common error messages and likely causes.</span></span>

### <a name="http-error-403---forbidden-access-is-denied"></a><span data-ttu-id="97ae7-120">Chyba protokolu HTTP 403 – Zakázáno: Přístup byl odepřen.</span><span class="sxs-lookup"><span data-stu-id="97ae7-120">HTTP Error 403 - Forbidden: Access is denied</span></span>

<span data-ttu-id="97ae7-121">*Nemáte oprávnění k zobrazení tohoto adresáře nebo stránky pomocí zadaných přihlašovacích údajů.*</span><span class="sxs-lookup"><span data-stu-id="97ae7-121">*You do not have permission to view this directory or page using the credentials that you supplied.*</span></span>

<span data-ttu-id="97ae7-122">Této chybě může dojít, pokud server není spuštěn správnou verzi rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="97ae7-122">This error can occur if the server is not running the correct version of the .NET Framework.</span></span> <span data-ttu-id="97ae7-123">Ujistěte se, že počítač, který je spuštěný server (místně nebo vzdáleně) má alespoň nainstalované rozhraní .NET Framework 4.</span><span class="sxs-lookup"><span data-stu-id="97ae7-123">Make sure that the computer that's running the server (locally or remotely) has at least the .NET Framework 4 installed.</span></span> <span data-ttu-id="97ae7-124">Také zajistěte, aby vlastní aplikace je nakonfigurovaná pro spuštění správnou verzi.</span><span class="sxs-lookup"><span data-stu-id="97ae7-124">Also make sure that the application itself is configured to run the right version.</span></span>

<span data-ttu-id="97ae7-125">Pokud tento problém se zobrazí místně při práci ve službě WebMatrix, klikněte na tlačítko **lokality** pracovního prostoru a potom ve stromovém zobrazení, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="97ae7-125">If you see this problem locally while working in WebMatrix, click the **Site** workspace, and then in the treeview click **Settings**.</span></span> <span data-ttu-id="97ae7-126">V **vyberte verzi rozhraní .NET Framework** vyberte **.NET 4 (integrovaný režim)**.</span><span class="sxs-lookup"><span data-stu-id="97ae7-126">In the **Select .NET Framework Version** list, select **.NET 4 (Integrated)**.</span></span> <span data-ttu-id="97ae7-127">Pokud tato verze je už nastavené, zkuste spustit jako správce služby WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="97ae7-127">If this version is already set, try running WebMatrix as an administrator.</span></span>

<span data-ttu-id="97ae7-128">Ověřte, zda kořenovém adresáři vašeho webu má alespoň jeden *.cshtml* souboru v ní.</span><span class="sxs-lookup"><span data-stu-id="97ae7-128">Make sure that the root of your website has at least one *.cshtml* file in it.</span></span>

<span data-ttu-id="97ae7-129">Pokud se zobrazí tato chyba, když webový server je na vzdáleném serveru, obraťte se na správce serveru.</span><span class="sxs-lookup"><span data-stu-id="97ae7-129">If you see this error when the web server is on a remote server, contact the server administrator.</span></span> <span data-ttu-id="97ae7-130">Ujistěte se, že server má rozhraní .NET Framework 4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="97ae7-130">Make sure that the server has the .NET Framework 4 or later installed.</span></span> <span data-ttu-id="97ae7-131">Také se ujistěte, že je aplikace spuštěna ve fondu aplikací, který je nakonfigurován pro použití této verzi rozhraní.NET Framework.</span><span class="sxs-lookup"><span data-stu-id="97ae7-131">Also make sure that the application is running in an application pool that's configured to use that version of the.NET Framework.</span></span>

<span data-ttu-id="97ae7-132">Pokud budete mít kontrolu nad serveru, ujistěte se, že běží správnou verzi rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="97ae7-132">If you have control over the server, make sure it's running the correct version of the .NET Framework.</span></span> <span data-ttu-id="97ae7-133">Může také zkuste opravit instalaci spuštěním `aspnet_regiis -iru` příkaz.</span><span class="sxs-lookup"><span data-stu-id="97ae7-133">You might also try repairing the installation by running the `aspnet_regiis -iru` command.</span></span> <span data-ttu-id="97ae7-134">(Například při instalaci služby IIS po instalaci rozhraní .NET Framework, služba IIS nebude správně konfigurován pro spuštění stránek ASP.NET.) Další informace najdete v tématu [ASP.NET IIS Registration Tool (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="97ae7-134">(For example, if you install IIS after you install the .NET Framework, IIS will not be correctly configured to run ASP.NET pages.) For more information, see [ASP.NET IIS Registration Tool (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).</span></span>

### <a name="http-error-40314---forbidden"></a><span data-ttu-id="97ae7-135">Chyba protokolu HTTP 403.14 – zakázáno</span><span class="sxs-lookup"><span data-stu-id="97ae7-135">HTTP Error 403.14 - Forbidden</span></span>

<span data-ttu-id="97ae7-136">*Webový server je nakonfigurován tak, aby nezobrazovat obsah tohoto adresáře.*</span><span class="sxs-lookup"><span data-stu-id="97ae7-136">*The Web server is configured to not list the contents of this directory.*</span></span>

<span data-ttu-id="97ae7-137">Této chybě může dojít v případě požadavku na prostředek, který je chráněn (jako *Web.config* soubor) nebo do složky, který je chráněný, který je (jako *aplikace\_Data* nebo *aplikace\_Kód*).</span><span class="sxs-lookup"><span data-stu-id="97ae7-137">This error can occur if you request a resource that's protected (like the *Web.config* file) or that's in a folder that's protected (like *App\_Data* or *App\_Code*).</span></span>

### <a name="http-error-40417---not-found"></a><span data-ttu-id="97ae7-138">Chyba protokolu HTTP 404.17 – Nenalezeno</span><span class="sxs-lookup"><span data-stu-id="97ae7-138">HTTP Error 404.17 - Not Found</span></span>

<span data-ttu-id="97ae7-139">*Požadovaný obsah pravděpodobně skript který nebude obslužnou rutinou statických souborů.*</span><span class="sxs-lookup"><span data-stu-id="97ae7-139">*The requested content appears to be script and will not be served by the static file handler.*</span></span>

<span data-ttu-id="97ae7-140">Této chybě může dojít, pokud server není správně nakonfigurován pro používání rozhraní .NET Framework 4 nebo novější a proto nemůže rozpoznat kód v `@{ }` bloky.</span><span class="sxs-lookup"><span data-stu-id="97ae7-140">This error can occur if the server is not configured correctly to use the .NET Framework 4 or later, and therefore does not recognize the code in `@{ }` blocks.</span></span> <span data-ttu-id="97ae7-141">Naleznete v popisu výše pro *HTTP chyby 403 – Zakázáno: přístup byl odepřen*.</span><span class="sxs-lookup"><span data-stu-id="97ae7-141">See the description earlier for *HTTP Error 403 - Forbidden: Access is denied*.</span></span>

### <a name="http-error-4047---not-found"></a><span data-ttu-id="97ae7-142">Chyba protokolu HTTP 404.7 - nebyl nalezen</span><span class="sxs-lookup"><span data-stu-id="97ae7-142">HTTP Error 404.7 - Not Found</span></span>

<span data-ttu-id="97ae7-143">*Modul filtrování požadavků je nakonfigurován tak, aby odepřel příponu souboru*</span><span class="sxs-lookup"><span data-stu-id="97ae7-143">*The request filtering module is configured to deny the file extension*</span></span>

<span data-ttu-id="97ae7-144">Této chybě může dojít, pokud *.cshtml* nebo *.vbhtml* rozšíření byla explicitně zablokovaná na serveru.</span><span class="sxs-lookup"><span data-stu-id="97ae7-144">This error can occur if *.cshtml* or *.vbhtml* extensions have been explicitly blocked on the server.</span></span> <span data-ttu-id="97ae7-145">Příznakem tohoto problému je svou práci adresy URL, když není uvedena rozšíření, ale adresy URL, které zahrnují *.cshtml* nebo *.vbhtml* nefungují.</span><span class="sxs-lookup"><span data-stu-id="97ae7-145">A symptom of this problem is that URLs work when they do not include the extension, but URLs that include *.cshtml* or *.vbhtml* do not work.</span></span> <span data-ttu-id="97ae7-146">Možných řešení je znovu povolit rozšíření na webu *Web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="97ae7-146">A possible solution is to re-enable the extensions in the site's *Web.config* file.</span></span> <span data-ttu-id="97ae7-147">Následující příklad ukazuje, jak povolit *.cshtml* rozšíření.</span><span class="sxs-lookup"><span data-stu-id="97ae7-147">The following example shows how to enable the *.cshtml* extension.</span></span>

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a><span data-ttu-id="97ae7-148">Chyba protokolu HTTP 404.8 - nebyla nalezena</span><span class="sxs-lookup"><span data-stu-id="97ae7-148">HTTP Error 404.8 - Not Found</span></span>

<span data-ttu-id="97ae7-149">*Modul filtrování požadavků je nakonfigurován tak, aby odepřel cestu v adrese URL, která obsahuje oddíl hiddensegment.*</span><span class="sxs-lookup"><span data-stu-id="97ae7-149">*The request filtering module is configured to deny a path in the URL that contains a hiddenSegment section.*</span></span>

<span data-ttu-id="97ae7-150">Této chybě může dojít v případě požadavku na prostředek, který je chráněn (jako *Web.config* soubor) nebo do složky, který je chráněný, který je (jako *aplikace\_Data* nebo *aplikace\_Kód*).</span><span class="sxs-lookup"><span data-stu-id="97ae7-150">This error can occur if you request a resource that's protected (like the *Web.config* file) or that's in a folder that's protected (like *App\_Data* or *App\_Code*).</span></span>

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a><span data-ttu-id="97ae7-151">Tento typ stránky není obsluhovat (Chyba serveru v aplikaci '/')</span><span class="sxs-lookup"><span data-stu-id="97ae7-151">This type of page is not served (Server Error in '/' Application)</span></span>

<span data-ttu-id="97ae7-152">Naleznete v popisu výše pro 404.17 chyby protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="97ae7-152">See the description earlier for HTTP Error 404.17.</span></span>

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a><span data-ttu-id="97ae7-153">Problémy s kódu Razor</span><span class="sxs-lookup"><span data-stu-id="97ae7-153">Issues with Razor code</span></span>

### <a name="the-name-class-does-not-exist-in-the-current-context"></a><span data-ttu-id="97ae7-154">Název '*třída*' neexistuje v aktuálním kontextu</span><span class="sxs-lookup"><span data-stu-id="97ae7-154">The name '*class*' does not exist in the current context</span></span>

<span data-ttu-id="97ae7-155">Často, který je důvod se zobrazí tato chyba `class` odkazy pomocné rutiny, ale pomocné rutiny není nainstalována.</span><span class="sxs-lookup"><span data-stu-id="97ae7-155">Often, a reason you see this error is that `class` references a helper, but the helper is not installed.</span></span> <span data-ttu-id="97ae7-156">Například pokud se pokusíte použít pomocné rutiny, ale pokud nemáte nainstalovaný balíček z NuGet, zobrazí se tato chyba.</span><span class="sxs-lookup"><span data-stu-id="97ae7-156">For example, if you try to use a helper, but if you haven't installed the package from NuGet, you'll see this error.</span></span> <span data-ttu-id="97ae7-157">Pomocí Galerie ve službě WebMatrix můžete najít a nainstalovat pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="97ae7-157">Use the Gallery in WebMatrix to find and install the helper.</span></span>

<span data-ttu-id="97ae7-158">Pokud je nainstalován pomocné rutiny, ale stránce stále nerozpoznal ho, pokuste se přidat přidat `using` příkaz ke kódu.</span><span class="sxs-lookup"><span data-stu-id="97ae7-158">If the helper is installed, but the page still doesn't recognize it, try adding add a `using` statement to the code.</span></span> <span data-ttu-id="97ae7-159">V `using` prohlášení, odkaz na obor názvů, který obsahuje pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="97ae7-159">In the `using` statement, reference the namespace that includes the helper.</span></span> <span data-ttu-id="97ae7-160">Například základní pomocné rutiny, které jsou v balíčku ASP.NET webové pomocné objekty jsou v `System.Web.Helpers` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="97ae7-160">For example, the basic helpers that are in the ASP.NET Web Helpers package are in the `System.Web.Helpers` namespace.</span></span> <span data-ttu-id="97ae7-161">V horní části stránky, kde chcete použít pomocné rutiny přidejte následující řádek:</span><span class="sxs-lookup"><span data-stu-id="97ae7-161">At the top of the page where you want to use the helper, add this line:</span></span>

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a><span data-ttu-id="97ae7-162">Problémy s členství a zabezpečení</span><span class="sxs-lookup"><span data-stu-id="97ae7-162">Issues with Security and Membership</span></span>

<span data-ttu-id="97ae7-163">Pokud používáte systém integrované zabezpečení (členství) na webových stránkách ASP.NET (Razor), mohou se vyskytnout následující problémy.</span><span class="sxs-lookup"><span data-stu-id="97ae7-163">If you are using the built-in security (membership) system in ASP.NET Web Pages (Razor), you might encounter the following issues.</span></span>

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a><span data-ttu-id="97ae7-164">Mohli volat tuto metodu, musí být vlastnost "Membership.Provider" instance "ExtendedMembershipProvider"</span><span class="sxs-lookup"><span data-stu-id="97ae7-164">To call this method, the "Membership.Provider" property must be an instance of "ExtendedMembershipProvider"</span></span>

<span data-ttu-id="97ae7-165">Tuto chybu můžete určit, že žádné `AspNetSqlMembershipProvider` třída je nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="97ae7-165">This error can indicate that no `AspNetSqlMembershipProvider` class is configured.</span></span> <span data-ttu-id="97ae7-166">(Příznakem je, že lokality funguje bez problémů místně, ale vrátí tuto chybu při publikování serveru poskytovatele hostingu.) Jeden tohoto problému je explicitně povolit jednoduché členství vložením následujícího textu na web *Web.config* souboru:</span><span class="sxs-lookup"><span data-stu-id="97ae7-166">(A symptom is that the site works fine locally but throws this error when you publish it to a hosting provider's server.) One fix for this problem is to explicitly enable simple membership by adding the following to the site's *Web.config* file:</span></span>

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a><span data-ttu-id="97ae7-167">Problémy s e-mailem</span><span class="sxs-lookup"><span data-stu-id="97ae7-167">Issues with Sending Email</span></span>

<span data-ttu-id="97ae7-168">Problémy s e-mailem, může být náročné k ladění.</span><span class="sxs-lookup"><span data-stu-id="97ae7-168">Problems with sending email can be challenging to debug.</span></span> <span data-ttu-id="97ae7-169">Počáteční problém může být, že se nelze připojit k serveru SMTP.</span><span class="sxs-lookup"><span data-stu-id="97ae7-169">An initial problem can be that you can't connect to the SMTP server.</span></span> <span data-ttu-id="97ae7-170">Pokud je připojení úspěšné, ASP.NET předá zprávu na SMTP server.</span><span class="sxs-lookup"><span data-stu-id="97ae7-170">If the connection is successful, ASP.NET hands the message off to the SMTP server.</span></span> <span data-ttu-id="97ae7-171">Však může být problémy se zprávou sám sebe, který zabrání odeslání serveru SMTP.</span><span class="sxs-lookup"><span data-stu-id="97ae7-171">However, there can be problems with the message itself that prevents the SMTP server from sending it.</span></span>

<span data-ttu-id="97ae7-172">Pokud vaše aplikace není úspěšně odeslat e-mailu, zkuste následující postup:</span><span class="sxs-lookup"><span data-stu-id="97ae7-172">If your application does not successfully send email, try the following:</span></span>

- <span data-ttu-id="97ae7-173">Název serveru SMTP je často něco podobného jako `smtp.provider.com` nebo `smtp.provider.net`.</span><span class="sxs-lookup"><span data-stu-id="97ae7-173">The SMTP server name is often something like `smtp.provider.com` or `smtp.provider.net`.</span></span> <span data-ttu-id="97ae7-174">Pokud však publikování webu k poskytovateli hostingu, název serveru SMTP v daném okamžiku může být `localhost`.</span><span class="sxs-lookup"><span data-stu-id="97ae7-174">However, if you publish your site to a hosting provider, the SMTP server name at that point might be `localhost`.</span></span> <span data-ttu-id="97ae7-175">K této situaci dochází, protože když jste publikovali a vaše lokalita běží na serveru poskytovatele, SMTP server může být místní z hlediska vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="97ae7-175">This situation occurs because after you've published and your site is running on the provider's server, the SMTP server might be local from the perspective of your application.</span></span> <span data-ttu-id="97ae7-176">Tuto změnu v hodnotě názvů serverů může znamenat, že budete muset změnit název serveru SMTP jako součást procesu publikování.</span><span class="sxs-lookup"><span data-stu-id="97ae7-176">This change in server names might mean that you have to change the SMTP server name as part of your publishing process.</span></span>
- <span data-ttu-id="97ae7-177">Číslo portu je obvykle 25.</span><span class="sxs-lookup"><span data-stu-id="97ae7-177">The port number is usually 25.</span></span> <span data-ttu-id="97ae7-178">Ale někteří poskytovatelé vyžadovat použití portu 587 nebo některé port.</span><span class="sxs-lookup"><span data-stu-id="97ae7-178">However, some providers require you to use port 587 or some other port.</span></span> <span data-ttu-id="97ae7-179">Obraťte se na vlastníka serveru SMTP, jaké číslo portu se předpokládá, že použít.</span><span class="sxs-lookup"><span data-stu-id="97ae7-179">Check with the owner of the SMTP server what port number they expect you to use.</span></span>
- <span data-ttu-id="97ae7-180">Ujistěte se, že používáte správné přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="97ae7-180">Make sure that you use the right credentials.</span></span> <span data-ttu-id="97ae7-181">Pokud váš web jste publikovali do poskytovatele hostitelských služeb, použijte přihlašovací údaje, které zprostředkovatel má konkrétně označil jsou e-mailu.</span><span class="sxs-lookup"><span data-stu-id="97ae7-181">If you've published your site to a hosting provider, use the credentials that the provider has specifically indicated are for email.</span></span> <span data-ttu-id="97ae7-182">Tyto přihlašovací údaje se může lišit od přihlašovacích údajů, které můžete použít k publikování.</span><span class="sxs-lookup"><span data-stu-id="97ae7-182">These credentials might be different from the credentials you use to publish.</span></span>
- <span data-ttu-id="97ae7-183">Někdy vůbec nepotřebujete přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="97ae7-183">Sometimes you don't need credentials at all.</span></span> <span data-ttu-id="97ae7-184">Pokud odesíláte e-mailu pomocí osobní poskytovatel internetových služeb, může poskytovatel e-mailu již znáte svoje přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="97ae7-184">If you're sending email by using your personal ISP, your email provider might already know your credentials.</span></span> <span data-ttu-id="97ae7-185">Po publikování, možná budete muset použít jiné pověření než při testování v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="97ae7-185">After you publish, you might need to use different credentials than when you test on your local computer.</span></span>
- <span data-ttu-id="97ae7-186">Pokud váš poskytovatel e-mailu používá šifrování, nastavte `WebMail.EnableSsl` k `true`.</span><span class="sxs-lookup"><span data-stu-id="97ae7-186">If your email provider uses encryption, set `WebMail.EnableSsl` to `true`.</span></span>

<span data-ttu-id="97ae7-187">Pokud dojde k chybě při odesílání e-mailu, může se zobrazit zpráva standardní chyba ASP.NET, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="97ae7-187">If there is an error sending email, you might see a standard ASP.NET error message, which looks like this:</span></span>

![ASP.NET chybovou zprávu, když dojde k potížím s e-mailu](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

<span data-ttu-id="97ae7-189">Můžete také ladění potíží při odesílání e-mailu pomocí `try-catch` bloku, jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="97ae7-189">You can also debug problems with sending email by using a `try-catch` block, as in the following example.</span></span> <span data-ttu-id="97ae7-190">Při použití `try-catch` bloku ASP.NET nezobrazuje jeho standardní cíl chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="97ae7-190">When you use a `try-catch` block, ASP.NET does not display its standard error messages.</span></span> <span data-ttu-id="97ae7-191">Místo toho můžete zaznamenat chybu v `catch` část bloku.</span><span class="sxs-lookup"><span data-stu-id="97ae7-191">Instead, you can capture the error in the `catch` portion of the block.</span></span>

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

<span data-ttu-id="97ae7-192">Nahraďte příslušnými hodnotami pro `your-SMTP-server-name`a tak dále.</span><span class="sxs-lookup"><span data-stu-id="97ae7-192">Substitute the appropriate values for `your-SMTP-server-name`, and so on.</span></span> <span data-ttu-id="97ae7-193">Některé chybové zprávy, které mohou být zobrazeny tímto způsobem zahrnují následující:</span><span class="sxs-lookup"><span data-stu-id="97ae7-193">Some of the error messages you might see this way include the following:</span></span>

- <span data-ttu-id="97ae7-194">*Chyba při odesílání e-mailu.*</span><span class="sxs-lookup"><span data-stu-id="97ae7-194">*Failure sending mail.*</span></span>

    <span data-ttu-id="97ae7-195">-nebo-</span><span class="sxs-lookup"><span data-stu-id="97ae7-195">-or-</span></span>

    <span data-ttu-id="97ae7-196">*Pokus o připojení se nezdařila, protože připojená strana nereagovala správně po určitou dobu nebo navázané připojení se nezdařila, protože připojení hostitele se nepodařilo reagovat*</span><span class="sxs-lookup"><span data-stu-id="97ae7-196">*A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond*</span></span>

    <span data-ttu-id="97ae7-197">Tato chyba obvykle znamená, že aplikace se nemůže připojit k serveru SMTP.</span><span class="sxs-lookup"><span data-stu-id="97ae7-197">This error usually means that the application could not connect to the SMTP server.</span></span> <span data-ttu-id="97ae7-198">Zkontrolujte název serveru a číslo portu.</span><span class="sxs-lookup"><span data-stu-id="97ae7-198">Check the server name and port number.</span></span>
- <span data-ttu-id="97ae7-199"><em>Poštovní schránky k dispozici. Odpověď serveru: 5.1.0 &lt; someuser@invaliddomain &gt; odesílatele odmítl: Neplatný odesílatele domény</em></span><span class="sxs-lookup"><span data-stu-id="97ae7-199"><em>Mailbox unavailable. The server response was: 5.1.0 &lt;someuser@invaliddomain&gt; sender rejected : invalid sender domain</em></span></span>

    <span data-ttu-id="97ae7-200">Tuto zprávu můžete určit, že `From` adresa není správná nebo chybí.</span><span class="sxs-lookup"><span data-stu-id="97ae7-200">This message can indicate that the `From` address is not correct or is missing.</span></span>
- <span data-ttu-id="97ae7-201">*Zadaný řetězec není ve formátu požadované pro e-mailovou adresu.*</span><span class="sxs-lookup"><span data-stu-id="97ae7-201">*The specified string is not in the form required for an email address.*</span></span>

    <span data-ttu-id="97ae7-202">Tato chyba může znamenat, hodnota `To` nebo `From` vlastnosti nejsou rozpoznány jako e-mailové adresy.</span><span class="sxs-lookup"><span data-stu-id="97ae7-202">This error might indicate that the value of the `To` or `From` properties are not recognized as email addresses.</span></span> <span data-ttu-id="97ae7-203">(ASP.NET nelze zkontrolovat, že e-mailová adresa je platná, pouze to je ve správném formátu, jako je třeba *name@domain.com*.)</span><span class="sxs-lookup"><span data-stu-id="97ae7-203">(ASP.NET cannot check that the email address is valid, only that it's in the correct format, like *name@domain.com*.)</span></span>

> [!NOTE]
> <span data-ttu-id="97ae7-204">Odeberte kód, který se zobrazí chyba (`@errorMessage`) před publikováním stránky k lokalitě za provozu.</span><span class="sxs-lookup"><span data-stu-id="97ae7-204">Remove the markup that displays the error (`@errorMessage`) before you publish the page to a live site.</span></span> <span data-ttu-id="97ae7-205">Není vhodné tak, aby uživatelé zobrazit chybové zprávy, které jste získali ze serveru.</span><span class="sxs-lookup"><span data-stu-id="97ae7-205">It's not a good idea to let users see error messages that you get from a server.</span></span>


<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="97ae7-206">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="97ae7-206">Additional Resources</span></span>

[<span data-ttu-id="97ae7-207">Webové stránky ASP.NET (Razor) – časté otázky</span><span class="sxs-lookup"><span data-stu-id="97ae7-207">ASP.NET Web Pages (Razor) FAQ</span></span>](https://go.microsoft.com/fwlink/?LinkId=253000)

<span data-ttu-id="97ae7-208">[Služba WebMatrix a webové stránky ASP.NET](https://forums.asp.net/1224.aspx/1?WebMatrix) fórum na webu technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="97ae7-208">[WebMatrix and ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) forum on the ASP.NET website</span></span>
