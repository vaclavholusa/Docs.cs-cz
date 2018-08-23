---
uid: web-api/overview/security/basic-authentication
title: Základní ověřování v rozhraní ASP.NET Web API | Dokumentace Microsoftu
author: MikeWasson
description: Popisuje použití základního ověřování v rozhraní ASP.NET Web API.
ms.author: riande
ms.date: 10/02/2014
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 7afafb6b7851f0d955d1f4292318f64d2a068a45
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756084"
---
<a name="basic-authentication-in-aspnet-web-api"></a><span data-ttu-id="cfaaf-103">Základní ověřování v rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="cfaaf-103">Basic Authentication in ASP.NET Web API</span></span>
====================
<span data-ttu-id="cfaaf-104">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="cfaaf-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="cfaaf-105">Základní ověřování je definována v [RFC 2617, ověřování pomocí protokolu HTTP: Basic a ověřování algoritmem Digest přístup](http://www.ietf.org/rfc/rfc2617.txt).</span><span class="sxs-lookup"><span data-stu-id="cfaaf-105">Basic authentication is defined in [RFC 2617, HTTP Authentication: Basic and Digest Access Authentication](http://www.ietf.org/rfc/rfc2617.txt).</span></span>

<span data-ttu-id="cfaaf-106">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="cfaaf-106">Disadvantages</span></span>

- <span data-ttu-id="cfaaf-107">Přihlašovací údaje uživatele jsou odeslány v požadavku.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-107">User credentials are sent in the request.</span></span>
- <span data-ttu-id="cfaaf-108">Přihlašovací údaje jsou odeslány jako prostý text.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-108">Credentials are sent as plaintext.</span></span>
- <span data-ttu-id="cfaaf-109">Přihlašovací údaje jsou odeslány při každé žádosti.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-109">Credentials are sent with every request.</span></span>
- <span data-ttu-id="cfaaf-110">Žádný způsob, jak se odhlásit, s výjimkou ukončením relace prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-110">No way to log out, except by ending the browser session.</span></span>
- <span data-ttu-id="cfaaf-111">Snadno napadnutelný mezi weby (CSRF); proti padělání požadavků vyžaduje Antimalware CSRF opatření.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-111">Vulnerable to cross-site request forgery (CSRF); requires anti-CSRF measures.</span></span>

<span data-ttu-id="cfaaf-112">Výhody</span><span class="sxs-lookup"><span data-stu-id="cfaaf-112">Advantages</span></span>

- <span data-ttu-id="cfaaf-113">Internet standard.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-113">Internet standard.</span></span>
- <span data-ttu-id="cfaaf-114">Podporuje všechny hlavní prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-114">Supported by all major browsers.</span></span>
- <span data-ttu-id="cfaaf-115">Poměrně jednoduchá protokol.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-115">Relatively simple protocol.</span></span>

<span data-ttu-id="cfaaf-116">Základní ověřování pracuje následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="cfaaf-116">Basic authentication works as follows:</span></span>

1. <span data-ttu-id="cfaaf-117">Pokud požadavek vyžaduje ověření, server vrátí 401 (Neautorizováno).</span><span class="sxs-lookup"><span data-stu-id="cfaaf-117">If a request requires authentication, the server returns 401 (Unauthorized).</span></span> <span data-ttu-id="cfaaf-118">Odpověď obsahuje hlavičku WWW-Authenticate, oznamující, že server podporuje základní ověřování.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-118">The response includes a WWW-Authenticate header, indicating the server supports Basic authentication.</span></span>
2. <span data-ttu-id="cfaaf-119">Klient odešle požadavek na jiný, pomocí přihlašovacích údajů klienta v hlavičce autorizace.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-119">The client sends another request, with the client credentials in the Authorization header.</span></span> <span data-ttu-id="cfaaf-120">Přihlašovací údaje jsou formátovány jako řetězec "název: heslo", s kódováním base64.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-120">The credentials are formatted as the string "name:password", base64-encoded.</span></span> <span data-ttu-id="cfaaf-121">Přihlašovací údaje nejsou šifrovány.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-121">The credentials are not encrypted.</span></span>

<span data-ttu-id="cfaaf-122">Základní ověřování je prováděno v kontextu "sféru."</span><span class="sxs-lookup"><span data-stu-id="cfaaf-122">Basic authentication is performed within the context of a "realm."</span></span> <span data-ttu-id="cfaaf-123">Na serveru obsahuje název sféry hlavičky WWW-Authenticate.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-123">The server includes the name of the realm in the WWW-Authenticate header.</span></span> <span data-ttu-id="cfaaf-124">Přihlašovací údaje uživatele jsou platné v rámci této sféru.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-124">The user's credentials are valid within that realm.</span></span> <span data-ttu-id="cfaaf-125">Přesné oboru sféru je definována na serveru.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-125">The exact scope of a realm is defined by the server.</span></span> <span data-ttu-id="cfaaf-126">Například můžete třeba definovat několik sféry v pořadí oddílu prostředků.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-126">For example, you might define several realms in order to partition resources.</span></span>

![](basic-authentication/_static/image1.png)

<span data-ttu-id="cfaaf-127">Vzhledem k tomu, že přihlašovací údaje se odesílají v nešifrované, základní ověřování je pouze zabezpečené přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-127">Because the credentials are sent unencrypted, Basic authentication is only secure over HTTPS.</span></span> <span data-ttu-id="cfaaf-128">Zobrazit [práce s protokolem SSL ve webovém rozhraní API](working-with-ssl-in-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="cfaaf-128">See [Working with SSL in Web API](working-with-ssl-in-web-api.md).</span></span>

<span data-ttu-id="cfaaf-129">Základní ověřování je také zranitelný vůči útokům CSRF.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-129">Basic authentication is also vulnerable to CSRF attacks.</span></span> <span data-ttu-id="cfaaf-130">Poté, co uživatel zadá přihlašovací údaje, prohlížeč automaticky odesílá je na následné žádosti do stejné domény po dobu trvání relace.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-130">After the user enters credentials, the browser automatically sends them on subsequent requests to the same domain, for the duration of the session.</span></span> <span data-ttu-id="cfaaf-131">To zahrnuje odesílání požadavků AJAX.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-131">This includes AJAX requests.</span></span> <span data-ttu-id="cfaaf-132">Zobrazit [prevence útoků proti padělání (CSRF) podvržení žádosti](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="cfaaf-132">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>

## <a name="basic-authentication-with-iis"></a><span data-ttu-id="cfaaf-133">Základní ověřování se službou IIS</span><span class="sxs-lookup"><span data-stu-id="cfaaf-133">Basic Authentication with IIS</span></span>

<span data-ttu-id="cfaaf-134">Služba IIS podporuje základní ověřování, ale je výstrahou: ověření uživatele vůči přihlašovacích údajů Windows.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-134">IIS supports Basic authentication, but there is a caveat: The user is authenticated against their Windows credentials.</span></span> <span data-ttu-id="cfaaf-135">To znamená, že uživatel musí mít účet v doméně serveru.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-135">That means the user must have an account on the server's domain.</span></span> <span data-ttu-id="cfaaf-136">Pro webový server veřejnou obvykle chcete k ověřování na základě poskytovatele členství ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-136">For a public-facing web site, you typically want to authenticate against an ASP.NET membership provider.</span></span>

<span data-ttu-id="cfaaf-137">Pokud chcete povolit základní ověřování pomocí služby IIS, nastavte "Windows" v souboru Web.config vašeho projektu ASP.NET režim ověřování:</span><span class="sxs-lookup"><span data-stu-id="cfaaf-137">To enable Basic authentication using IIS, set the authentication mode to "Windows" in the Web.config of your ASP.NET project:</span></span>

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

<span data-ttu-id="cfaaf-138">V tomto režimu služba IIS používá přihlašovací údaje Windows k ověření.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-138">In this mode, IIS uses Windows credentials to authenticate.</span></span> <span data-ttu-id="cfaaf-139">Kromě toho musíte povolit základní ověřování ve službě IIS.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-139">In addition, you must enable Basic authentication in IIS.</span></span> <span data-ttu-id="cfaaf-140">Ve Správci služby IIS, přejděte na zobrazení funkcí, vyberte metodu ověřování a povolit základní ověřování.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-140">In IIS Manager, go to Features View, select Authentication, and enable Basic authentication.</span></span>

![](basic-authentication/_static/image2.png)

<span data-ttu-id="cfaaf-141">V projektu webového rozhraní API, přidejte `[Authorize]` atribut pro všechny akce kontroleru, které vyžadují ověřování.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-141">In your Web API project, add the `[Authorize]` attribute for any controller actions that need authentication.</span></span>

<span data-ttu-id="cfaaf-142">Klient se ověří tak, že nastavíte hlavičku autorizace v požadavku.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-142">A client authenticates itself by setting the Authorization header in the request.</span></span> <span data-ttu-id="cfaaf-143">Klienty prohlížeče proveďte tento krok automaticky.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-143">Browser clients perform this step automatically.</span></span> <span data-ttu-id="cfaaf-144">Klienti nonbrowser muset nastavit hlavičku.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-144">Nonbrowser clients will need to set the header.</span></span>

## <a name="basic-authentication-with-custom-membership"></a><span data-ttu-id="cfaaf-145">Základní ověřování s vlastní členství</span><span class="sxs-lookup"><span data-stu-id="cfaaf-145">Basic Authentication with Custom Membership</span></span>

<span data-ttu-id="cfaaf-146">Jak už bylo zmíněno, základní ověření integrované do služby IIS používá přihlašovací údaje Windows.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-146">As mentioned, the Basic Authentication built into IIS uses Windows credentials.</span></span> <span data-ttu-id="cfaaf-147">To znamená, že je potřeba vytvořit účty pro uživatele na hostitelském serveru.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-147">That means you need to create accounts for your users on the hosting server.</span></span> <span data-ttu-id="cfaaf-148">Ale pro internetovou aplikaci, uživatelské účty jsou obvykle uložena v externí databázi.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-148">But for an internet application, user accounts are typically stored in an external database.</span></span>

<span data-ttu-id="cfaaf-149">Následující kód způsob, jakým modul HTTP, který provádí základní ověřování.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-149">The following code how an HTTP module that performs Basic Authentication.</span></span> <span data-ttu-id="cfaaf-150">Můžete snadno připojit poskytovatele členství ASP.NET tak, že nahradíte `CheckPassword` metodu, která je fiktivní metoda v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-150">You can easily plug in an ASP.NET membership provider by replacing the `CheckPassword` method, which is a dummy method in this example.</span></span>

<span data-ttu-id="cfaaf-151">Ve webovém rozhraní API 2, měli byste zvážit zápis [filtr ověřování](authentication-filters.md) nebo [OWIN middleware](../../../aspnet/overview/owin-and-katana/index.md), místo modulu HTTP.</span><span class="sxs-lookup"><span data-stu-id="cfaaf-151">In Web API 2, you should consider writing an [authentication filter](authentication-filters.md) or [OWIN middleware](../../../aspnet/overview/owin-and-katana/index.md), instead of an HTTP module.</span></span>

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

<span data-ttu-id="cfaaf-152">Pokud chcete povolit modul HTTP, přidejte následující v souboru web.config **system.webServer** části:</span><span class="sxs-lookup"><span data-stu-id="cfaaf-152">To enable the HTTP module, add the following to your web.config file in the **system.webServer** section:</span></span>

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

<span data-ttu-id="cfaaf-153">Nahraďte název sestavení (nezahrnuje příponu "dll") "YourAssemblyName".</span><span class="sxs-lookup"><span data-stu-id="cfaaf-153">Replace "YourAssemblyName" with the name of the assembly (not including the "dll" extension).</span></span>

<span data-ttu-id="cfaaf-154">Měli byste zakázat další metody ověřování, jako jsou tyto formuláře nebo Windows</span><span class="sxs-lookup"><span data-stu-id="cfaaf-154">You should disable other authentication schemes, such as Forms or Windows auth.</span></span>
