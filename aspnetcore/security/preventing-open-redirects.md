---
title: "Prevence útoků přesměrování otevřít v aplikaci ASP.NET Core | Microsoft Docs"
author: ardalis
description: "Ukazuje, jak zabránit otevřete přesměrování útoky na aplikace ASP.NET Core"
keywords: "Útok ASP.NET Core, zabezpečení, otevřete přesměrování"
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: article
ms.assetid: 4604e563-e91a-4ecd-b7ed-00b3f1eee2b5
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/preventing-open-redirects
ms.openlocfilehash: 4083845a77eb19d9ba9beb389a92ceb5c14edbde
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="preventing-open-redirect-attacks-in-an-aspnet-core-app"></a><span data-ttu-id="d2759-104">Prevence útoků přesměrování otevřít v aplikaci ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d2759-104">Preventing Open Redirect Attacks in an ASP.NET Core app</span></span>

<span data-ttu-id="d2759-105">Webové aplikace, který přesměruje na adresu URL, která je zadána prostřednictvím požadavku například řetězci dotazu nebo formuláře dat může potenciálně manipulováno k přesměrování uživatelů na externí, škodlivý URL.</span><span class="sxs-lookup"><span data-stu-id="d2759-105">A web app that redirects to a URL that is specified via the request such as the querystring or form data can potentially be tampered with to redirect users to an external, malicious URL.</span></span> <span data-ttu-id="d2759-106">Tato manipulaci se nazývá útok otevřete přesměrování.</span><span class="sxs-lookup"><span data-stu-id="d2759-106">This tampering is called an open redirection attack.</span></span>

<span data-ttu-id="d2759-107">Vždy, když vaše aplikace logiky přesměruje na zadané adrese URL, je nutné ověřit, že adresu URL pro přesměrování nikdo neoprávněně nemanipuloval.</span><span class="sxs-lookup"><span data-stu-id="d2759-107">Whenever your application logic redirects to a specified URL, you must verify that the redirection URL hasn't been tampered with.</span></span> <span data-ttu-id="d2759-108">ASP.NET Core obsahuje integrovanou funkci k ochraně aplikace před útoky otevřete přesměrování (označované také jako otevřený přesměrování).</span><span class="sxs-lookup"><span data-stu-id="d2759-108">ASP.NET Core has built-in functionality to help protect apps from open redirect (also known as open redirection) attacks.</span></span>

## <a name="what-is-an-open-redirect-attack"></a><span data-ttu-id="d2759-109">Co je útok otevřete přesměrování?</span><span class="sxs-lookup"><span data-stu-id="d2759-109">What is an open redirect attack?</span></span>

<span data-ttu-id="d2759-110">Při přístupu k prostředkům, které vyžadují ověřování webových aplikací často přesměrovat uživatele na přihlašovací stránku.</span><span class="sxs-lookup"><span data-stu-id="d2759-110">Web applications frequently redirect users to a login page when they access resources that require authentication.</span></span> <span data-ttu-id="d2759-111">Zahrnuje typlically přesměrování `returnUrl` parametr řetězce dotazu tak, aby uživatel se může vracet na původně požadovanou URL adresu po jejich úspěšně přihlášeni.</span><span class="sxs-lookup"><span data-stu-id="d2759-111">The redirection typlically includes a `returnUrl` querystring parameter so that the user can be returned to the originally requested URL after they have successfully logged in.</span></span> <span data-ttu-id="d2759-112">Jakmile se uživatel ověřuje, bude přesměrován na adresu URL, který měl původně požadovali.</span><span class="sxs-lookup"><span data-stu-id="d2759-112">After the user authenticates, they are redirected to the URL they had originally requested.</span></span>

<span data-ttu-id="d2759-113">Vzhledem k tomu, že cílová adresa URL je zadána v řetězci dotazu požadavku, uživatel se zlými úmysly může manipulovat s řetězci dotazu.</span><span class="sxs-lookup"><span data-stu-id="d2759-113">Because the destination URL is specified in the querystring of the request, a malicious user could tamper with the querystring.</span></span> <span data-ttu-id="d2759-114">Zmanipulovanou řetězce dotazu by se mohl lokality tak, aby přesměruje uživatele na stránku externí, škodlivý.</span><span class="sxs-lookup"><span data-stu-id="d2759-114">A tampered querystring could allow the site to redirect the user to an external, malicious site.</span></span> <span data-ttu-id="d2759-115">Tento postup se nazývá útok otevřete přesměrování (nebo přesměrování).</span><span class="sxs-lookup"><span data-stu-id="d2759-115">This technique is called an open redirect (or redirection) attack.</span></span>

### <a name="an-example-attack"></a><span data-ttu-id="d2759-116">Příklad útoku</span><span class="sxs-lookup"><span data-stu-id="d2759-116">An example attack</span></span>

<span data-ttu-id="d2759-117">Uživatel se zlými úmysly může vyvíjet útok díky uživatel se zlými úmysly přístup k pověření uživatele nebo citlivé informace ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d2759-117">A malicious user could develop an attack intended to allow the malicious user access to a user's credentials or sensitive information on your app.</span></span> <span data-ttu-id="d2759-118">Zahájit útoku, jejich přimět kliknutím na odkaz na přihlašovací stránku vašeho webu, s uživateli `returnUrl` hodnotu querystring adresu URL.</span><span class="sxs-lookup"><span data-stu-id="d2759-118">To begin the attack, they convince the user to click a link to your site's login page, with a `returnUrl` querystring value added to the URL.</span></span> <span data-ttu-id="d2759-119">Například [NerdDinner.com](http://nerddinner.com) ukázkovou aplikaci (napsané pro rozhraní ASP.NET MVC) zahrnuje takové přihlašovací stránky zde: ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``.</span><span class="sxs-lookup"><span data-stu-id="d2759-119">For example, the [NerdDinner.com](http://nerddinner.com) sample application (written for ASP.NET MVC) includes such a login page here: ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``.</span></span> <span data-ttu-id="d2759-120">Útoku pak postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="d2759-120">The attack then follows these steps:</span></span>

1. <span data-ttu-id="d2759-121">Uživatel klikne na odkaz ``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn`` (Všimněte si, druhý adresa URL je nerddi**n**če, není nerddi**nn**če).</span><span class="sxs-lookup"><span data-stu-id="d2759-121">User clicks a link to ``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn`` (note, second URL is nerddi**n**er, not nerddi**nn**er).</span></span>
2. <span data-ttu-id="d2759-122">Uživatel se přihlásí úspěšně.</span><span class="sxs-lookup"><span data-stu-id="d2759-122">The user logs in successfully.</span></span>
3. <span data-ttu-id="d2759-123">Uživatel je přesměrován (lokalitou) na ``http://nerddiner.com/Account/LogOn`` (škodlivé weby, které vypadá skutečné lokality).</span><span class="sxs-lookup"><span data-stu-id="d2759-123">The user is redirected (by the site) to ``http://nerddiner.com/Account/LogOn`` (malicious site that looks like real site).</span></span>
4. <span data-ttu-id="d2759-124">Uživatel znovu přihlásí (poskytnutí škodlivý lokality své přihlašovací údaje) a je přesměrován zpět na web skutečné.</span><span class="sxs-lookup"><span data-stu-id="d2759-124">The user logs in again (giving malicious site their credentials) and is redirected back to the real site.</span></span>

<span data-ttu-id="d2759-125">Uživatel se pravděpodobně domníváte jejich první pokus o přihlášení se nezdařilo a jejich druhý byla úspěšná.</span><span class="sxs-lookup"><span data-stu-id="d2759-125">The user will likely believe their first attempt to log in failed, and their second one was successful.</span></span> <span data-ttu-id="d2759-126">Pravděpodobně budete zůstanou nebere v úvahu ohrožený přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="d2759-126">They'll most likely remain unaware their credentials have been compromised.</span></span>

![Proces útoku otevřete přesměrování](preventing-open-redirects/_static/open-redirection-attack-process.png)

<span data-ttu-id="d2759-128">Některé servery kromě přihlašovací stránky, zadejte přesměrování stránky nebo koncové body.</span><span class="sxs-lookup"><span data-stu-id="d2759-128">In addition to login pages, some sites provide redirect pages or endpoints.</span></span> <span data-ttu-id="d2759-129">Představte si aplikace se zobrazí stránka s otevřete přesměrování, ``/Home/Redirect``.</span><span class="sxs-lookup"><span data-stu-id="d2759-129">Imagine your app has a page with an open redirect, ``/Home/Redirect``.</span></span> <span data-ttu-id="d2759-130">Útočník by mohl vytvořit, například pomocí odkazu v e-mailu, který přejde na ``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``.</span><span class="sxs-lookup"><span data-stu-id="d2759-130">An attacker could create, for example, a link in an email that goes to ``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``.</span></span> <span data-ttu-id="d2759-131">Běžný uživatel na adrese URL a zjistit, že začíná název vaší lokality.</span><span class="sxs-lookup"><span data-stu-id="d2759-131">A typical user will look at the URL and see it begins with your site name.</span></span> <span data-ttu-id="d2759-132">Důvěřující, který, bude kliknutím na odkaz.</span><span class="sxs-lookup"><span data-stu-id="d2759-132">Trusting that, they will click the link.</span></span> <span data-ttu-id="d2759-133">Otevřete přesměrování by uživatel pak pošlete do lokality phishing, která vypadá totožná s tímto počítačem, a uživatel by pravděpodobně přihlášení, které budou věřit je váš web.</span><span class="sxs-lookup"><span data-stu-id="d2759-133">The open redirect would then send the user to the phishing site, which looks identical to yours, and the user would likely login to what they believe is your site.</span></span>

## <a name="protecting-against-open-redirect-attacks"></a><span data-ttu-id="d2759-134">Ochrana proti útokům na otevřete přesměrování</span><span class="sxs-lookup"><span data-stu-id="d2759-134">Protecting against open redirect attacks</span></span>

<span data-ttu-id="d2759-135">Při vývoji webových aplikací, považovat za nedůvěryhodným všechna data zadaný uživatelem.</span><span class="sxs-lookup"><span data-stu-id="d2759-135">When developing web applications, treat all user-provided data as untrustworthy.</span></span> <span data-ttu-id="d2759-136">Pokud aplikace obsahuje funkce, který přesměruje uživatele na základě obsahu adresy URL, zajistěte, aby takové přesměrování jsou pouze místně v rámci vaší aplikace (nebo známé adresy URL, není libovolnou URL, která může být zadána v řetězci dotazu).</span><span class="sxs-lookup"><span data-stu-id="d2759-136">If your application has functionality that redirects the user based on the contents of the URL,  ensure that such redirects are only done locally within your app (or to a known URL, not any URL that may be supplied in the querystring).</span></span>

### <a name="localredirect"></a><span data-ttu-id="d2759-137">LocalRedirect</span><span class="sxs-lookup"><span data-stu-id="d2759-137">LocalRedirect</span></span>

<span data-ttu-id="d2759-138">Použití ``LocalRedirect`` Pomocná metoda od základní `Controller` třídy:</span><span class="sxs-lookup"><span data-stu-id="d2759-138">Use the ``LocalRedirect`` helper method from the base `Controller` class:</span></span>

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

<span data-ttu-id="d2759-139">``LocalRedirect``bude vyvolána výjimka, pokud je zadaná adresa URL není místní.</span><span class="sxs-lookup"><span data-stu-id="d2759-139">``LocalRedirect`` will throw an exception if a non-local URL is specified.</span></span> <span data-ttu-id="d2759-140">Jinak se chová podobně jako ``Redirect`` metoda.</span><span class="sxs-lookup"><span data-stu-id="d2759-140">Otherwise, it behaves just like the ``Redirect`` method.</span></span>

### <a name="islocalurl"></a><span data-ttu-id="d2759-141">IsLocalUrl</span><span class="sxs-lookup"><span data-stu-id="d2759-141">IsLocalUrl</span></span>

<span data-ttu-id="d2759-142">Použití [IsLocalUrl](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iurlhelper#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) metoda před přesměrování otestovat adresy URL:</span><span class="sxs-lookup"><span data-stu-id="d2759-142">Use the [IsLocalUrl](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iurlhelper#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) method to test URLs before redirecting:</span></span>

<span data-ttu-id="d2759-143">Následující příklad ukazuje, jak zkontrolovat, zda je adresa URL místní před přesměrování.</span><span class="sxs-lookup"><span data-stu-id="d2759-143">The following example shows how to check whether a URL is local before redirecting.</span></span>

```csharp
private IActionResult RedirectToLocal(string returnUrl)
{
    if (Url.IsLocalUrl(returnUrl))
    {
        return Redirect(returnUrl);
    }
    else
    {
        return RedirectToAction(nameof(HomeController.Index), "Home");
    }
}
```

<span data-ttu-id="d2759-144">`IsLocalUrl` Metoda chrání uživatelé z nechtěně přesměrování na škodlivé weby.</span><span class="sxs-lookup"><span data-stu-id="d2759-144">The `IsLocalUrl` method protects users from being inadvertently redirected to a malicious site.</span></span> <span data-ttu-id="d2759-145">Přihlaste se na podrobnosti o adresu URL, který byl poskytnut, pokud nejsou místní adresa URL je zadáno v situaci, kde je očekávána místní adresa URL.</span><span class="sxs-lookup"><span data-stu-id="d2759-145">You can log the details of the URL that was provided when a non-local URL is supplied in a situation where you expected a local URL.</span></span> <span data-ttu-id="d2759-146">Protokolování přesměrování adresy URL může pomoci při diagnostice přesměrování útoky.</span><span class="sxs-lookup"><span data-stu-id="d2759-146">Logging redirect URLs may help in diagnosing redirection attacks.</span></span>
