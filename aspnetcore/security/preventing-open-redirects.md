---
title: "Prevence útoků přesměrování otevřít v aplikaci ASP.NET Core"
author: ardalis
description: "Ukazuje, jak zabránit otevřete přesměrování útoky na aplikace ASP.NET Core"
manager: wpickett
ms.author: riande
ms.date: 07/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/preventing-open-redirects
ms.openlocfilehash: d6cd65a2516c4d5e41428f0c1f2dbbe913ac2123
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="preventing-open-redirect-attacks-in-an-aspnet-core-app"></a><span data-ttu-id="56de6-103">Prevence útoků přesměrování otevřít v aplikaci ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56de6-103">Preventing Open Redirect Attacks in an ASP.NET Core app</span></span>

<span data-ttu-id="56de6-104">Webové aplikace, který přesměruje na adresu URL, která je zadána prostřednictvím požadavku například řetězci dotazu nebo formuláře dat může potenciálně manipulováno k přesměrování uživatelů na externí, škodlivý URL.</span><span class="sxs-lookup"><span data-stu-id="56de6-104">A web app that redirects to a URL that's specified via the request such as the querystring or form data can potentially be tampered with to redirect users to an external, malicious URL.</span></span> <span data-ttu-id="56de6-105">Tato manipulaci se nazývá útok otevřete přesměrování.</span><span class="sxs-lookup"><span data-stu-id="56de6-105">This tampering is called an open redirection attack.</span></span>

<span data-ttu-id="56de6-106">Vždy, když vaše aplikace logiky přesměruje na zadané adrese URL, je nutné ověřit, že adresu URL pro přesměrování nikdo neoprávněně nemanipuloval.</span><span class="sxs-lookup"><span data-stu-id="56de6-106">Whenever your application logic redirects to a specified URL, you must verify that the redirection URL hasn't been tampered with.</span></span> <span data-ttu-id="56de6-107">ASP.NET Core obsahuje integrovanou funkci k ochraně aplikace před útoky otevřete přesměrování (označované také jako otevřený přesměrování).</span><span class="sxs-lookup"><span data-stu-id="56de6-107">ASP.NET Core has built-in functionality to help protect apps from open redirect (also known as open redirection) attacks.</span></span>

## <a name="what-is-an-open-redirect-attack"></a><span data-ttu-id="56de6-108">Co je útok otevřete přesměrování?</span><span class="sxs-lookup"><span data-stu-id="56de6-108">What is an open redirect attack?</span></span>

<span data-ttu-id="56de6-109">Při přístupu k prostředkům, které vyžadují ověřování webových aplikací často přesměrovat uživatele na přihlašovací stránku.</span><span class="sxs-lookup"><span data-stu-id="56de6-109">Web applications frequently redirect users to a login page when they access resources that require authentication.</span></span> <span data-ttu-id="56de6-110">Zahrnuje typlically přesměrování `returnUrl` parametr řetězce dotazu tak, aby uživatel se může vracet na původně požadovanou URL adresu po jejich úspěšně přihlášeni.</span><span class="sxs-lookup"><span data-stu-id="56de6-110">The redirection typlically includes a `returnUrl` querystring parameter so that the user can be returned to the originally requested URL after they have successfully logged in.</span></span> <span data-ttu-id="56de6-111">Poté, co se uživatel ověřuje, že přesměrováni na měl původně požadovanou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="56de6-111">After the user authenticates, they're redirected to the URL they had originally requested.</span></span>

<span data-ttu-id="56de6-112">Vzhledem k tomu, že cílová adresa URL je zadána v řetězci dotazu požadavku, uživatel se zlými úmysly může manipulovat s řetězci dotazu.</span><span class="sxs-lookup"><span data-stu-id="56de6-112">Because the destination URL is specified in the querystring of the request, a malicious user could tamper with the querystring.</span></span> <span data-ttu-id="56de6-113">Zmanipulovanou řetězce dotazu by se mohl lokality tak, aby přesměruje uživatele na stránku externí, škodlivý.</span><span class="sxs-lookup"><span data-stu-id="56de6-113">A tampered querystring could allow the site to redirect the user to an external, malicious site.</span></span> <span data-ttu-id="56de6-114">Tento postup se nazývá útok otevřete přesměrování (nebo přesměrování).</span><span class="sxs-lookup"><span data-stu-id="56de6-114">This technique is called an open redirect (or redirection) attack.</span></span>

### <a name="an-example-attack"></a><span data-ttu-id="56de6-115">Příklad útoku</span><span class="sxs-lookup"><span data-stu-id="56de6-115">An example attack</span></span>

<span data-ttu-id="56de6-116">Uživatel se zlými úmysly může vyvíjet útok díky uživatel se zlými úmysly přístup k pověření uživatele nebo citlivé informace ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="56de6-116">A malicious user could develop an attack intended to allow the malicious user access to a user's credentials or sensitive information on your app.</span></span> <span data-ttu-id="56de6-117">Zahájit útoku, jejich přimět kliknutím na odkaz na přihlašovací stránku vašeho webu, s uživateli `returnUrl` hodnotu querystring adresu URL.</span><span class="sxs-lookup"><span data-stu-id="56de6-117">To begin the attack, they convince the user to click a link to your site's login page, with a `returnUrl` querystring value added to the URL.</span></span> <span data-ttu-id="56de6-118">Například [NerdDinner.com](http://nerddinner.com) ukázkovou aplikaci (napsané pro rozhraní ASP.NET MVC) zahrnuje takové přihlašovací stránky zde: ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``.</span><span class="sxs-lookup"><span data-stu-id="56de6-118">For example, the [NerdDinner.com](http://nerddinner.com) sample application (written for ASP.NET MVC) includes such a login page here: ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``.</span></span> <span data-ttu-id="56de6-119">Útoku pak postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="56de6-119">The attack then follows these steps:</span></span>

1. <span data-ttu-id="56de6-120">Uživatel klikne na odkaz ``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn`` (Všimněte si, druhý adresa URL je nerddi**n**če, není nerddi**nn**če).</span><span class="sxs-lookup"><span data-stu-id="56de6-120">User clicks a link to ``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn`` (note, second URL is nerddi**n**er, not nerddi**nn**er).</span></span>
2. <span data-ttu-id="56de6-121">Uživatel se přihlásí úspěšně.</span><span class="sxs-lookup"><span data-stu-id="56de6-121">The user logs in successfully.</span></span>
3. <span data-ttu-id="56de6-122">Uživatel je přesměrován (lokalitou) na ``http://nerddiner.com/Account/LogOn`` (škodlivé weby, které vypadá skutečné lokality).</span><span class="sxs-lookup"><span data-stu-id="56de6-122">The user is redirected (by the site) to ``http://nerddiner.com/Account/LogOn`` (malicious site that looks like real site).</span></span>
4. <span data-ttu-id="56de6-123">Uživatel znovu přihlásí (poskytnutí škodlivý lokality své přihlašovací údaje) a je přesměrován zpět na web skutečné.</span><span class="sxs-lookup"><span data-stu-id="56de6-123">The user logs in again (giving malicious site their credentials) and is redirected back to the real site.</span></span>

<span data-ttu-id="56de6-124">Uživatel se pravděpodobně domníváte jejich první pokus o přihlášení se nezdařilo a jejich druhý byla úspěšná.</span><span class="sxs-lookup"><span data-stu-id="56de6-124">The user will likely believe their first attempt to log in failed, and their second one was successful.</span></span> <span data-ttu-id="56de6-125">S největší pravděpodobností zůstanou nebere v úvahu ohrožený přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="56de6-125">They will most likely remain unaware their credentials have been compromised.</span></span>

![Proces útoku otevřete přesměrování](preventing-open-redirects/_static/open-redirection-attack-process.png)

<span data-ttu-id="56de6-127">Některé servery kromě přihlašovací stránky, zadejte přesměrování stránky nebo koncové body.</span><span class="sxs-lookup"><span data-stu-id="56de6-127">In addition to login pages, some sites provide redirect pages or endpoints.</span></span> <span data-ttu-id="56de6-128">Představte si aplikace se zobrazí stránka s otevřete přesměrování, ``/Home/Redirect``.</span><span class="sxs-lookup"><span data-stu-id="56de6-128">Imagine your app has a page with an open redirect, ``/Home/Redirect``.</span></span> <span data-ttu-id="56de6-129">Útočník by mohl vytvořit, například pomocí odkazu v e-mailu, který přejde na ``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``.</span><span class="sxs-lookup"><span data-stu-id="56de6-129">An attacker could create, for example, a link in an email that goes to ``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``.</span></span> <span data-ttu-id="56de6-130">Běžný uživatel na adrese URL a zjistit, že začíná název vaší lokality.</span><span class="sxs-lookup"><span data-stu-id="56de6-130">A typical user will look at the URL and see it begins with your site name.</span></span> <span data-ttu-id="56de6-131">Důvěřující, který, bude kliknutím na odkaz.</span><span class="sxs-lookup"><span data-stu-id="56de6-131">Trusting that, they will click the link.</span></span> <span data-ttu-id="56de6-132">Otevřete přesměrování by uživatel pak pošlete do lokality phishing, která vypadá totožná s tímto počítačem, a uživatel by pravděpodobně přihlášení, které budou věřit je váš web.</span><span class="sxs-lookup"><span data-stu-id="56de6-132">The open redirect would then send the user to the phishing site, which looks identical to yours, and the user would likely login to what they believe is your site.</span></span>

## <a name="protecting-against-open-redirect-attacks"></a><span data-ttu-id="56de6-133">Ochrana proti útokům na otevřete přesměrování</span><span class="sxs-lookup"><span data-stu-id="56de6-133">Protecting against open redirect attacks</span></span>

<span data-ttu-id="56de6-134">Při vývoji webových aplikací, považovat za nedůvěryhodným všechna data zadaný uživatelem.</span><span class="sxs-lookup"><span data-stu-id="56de6-134">When developing web applications, treat all user-provided data as untrustworthy.</span></span> <span data-ttu-id="56de6-135">Pokud aplikace obsahuje funkce, který přesměruje uživatele na základě obsahu adresy URL, zajistěte, aby takové přesměrování jsou pouze místně v rámci vaší aplikace (nebo známé adresy URL, není libovolnou URL, která může být zadána v řetězci dotazu).</span><span class="sxs-lookup"><span data-stu-id="56de6-135">If your application has functionality that redirects the user based on the contents of the URL,  ensure that such redirects are only done locally within your app (or to a known URL, not any URL that may be supplied in the querystring).</span></span>

### <a name="localredirect"></a><span data-ttu-id="56de6-136">LocalRedirect</span><span class="sxs-lookup"><span data-stu-id="56de6-136">LocalRedirect</span></span>

<span data-ttu-id="56de6-137">Použití ``LocalRedirect`` Pomocná metoda od základní `Controller` třídy:</span><span class="sxs-lookup"><span data-stu-id="56de6-137">Use the ``LocalRedirect`` helper method from the base `Controller` class:</span></span>

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

<span data-ttu-id="56de6-138">``LocalRedirect``bude vyvolána výjimka, pokud je zadaná adresa URL není místní.</span><span class="sxs-lookup"><span data-stu-id="56de6-138">``LocalRedirect`` will throw an exception if a non-local URL is specified.</span></span> <span data-ttu-id="56de6-139">Jinak se chová podobně jako ``Redirect`` metoda.</span><span class="sxs-lookup"><span data-stu-id="56de6-139">Otherwise, it behaves just like the ``Redirect`` method.</span></span>

### <a name="islocalurl"></a><span data-ttu-id="56de6-140">IsLocalUrl</span><span class="sxs-lookup"><span data-stu-id="56de6-140">IsLocalUrl</span></span>

<span data-ttu-id="56de6-141">Použití [IsLocalUrl](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iurlhelper#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) metoda před přesměrování otestovat adresy URL:</span><span class="sxs-lookup"><span data-stu-id="56de6-141">Use the [IsLocalUrl](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iurlhelper#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) method to test URLs before redirecting:</span></span>

<span data-ttu-id="56de6-142">Následující příklad ukazuje, jak zkontrolovat, zda je adresa URL místní před přesměrování.</span><span class="sxs-lookup"><span data-stu-id="56de6-142">The following example shows how to check whether a URL is local before redirecting.</span></span>

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

<span data-ttu-id="56de6-143">`IsLocalUrl` Metoda chrání uživatelé z nechtěně přesměrování na škodlivé weby.</span><span class="sxs-lookup"><span data-stu-id="56de6-143">The `IsLocalUrl` method protects users from being inadvertently redirected to a malicious site.</span></span> <span data-ttu-id="56de6-144">Přihlaste se na podrobnosti o adresu URL, který byl poskytnut, pokud nejsou místní adresa URL je zadáno v situaci, kde je očekávána místní adresa URL.</span><span class="sxs-lookup"><span data-stu-id="56de6-144">You can log the details of the URL that was provided when a non-local URL is supplied in a situation where you expected a local URL.</span></span> <span data-ttu-id="56de6-145">Protokolování přesměrování adresy URL může pomoci při diagnostice přesměrování útoky.</span><span class="sxs-lookup"><span data-stu-id="56de6-145">Logging redirect URLs may help in diagnosing redirection attacks.</span></span>
