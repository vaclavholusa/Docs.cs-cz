---
uid: web-api/overview/security/basic-authentication
title: "Základní ověřování v rozhraní ASP.NET Web API | Microsoft Docs"
author: MikeWasson
description: "Popisuje použití základního ověřování v rozhraní ASP.NET Web API."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/02/2014
ms.topic: article
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 4b8e6410668b2db289488bb4b6cd26d881e70f4c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="basic-authentication-in-aspnet-web-api"></a><span data-ttu-id="5675e-103">Základní ověřování v rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="5675e-103">Basic Authentication in ASP.NET Web API</span></span>
====================
<span data-ttu-id="5675e-104">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5675e-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="5675e-105">Základní ověřování je definována v [RFC 2617, ověřování pomocí protokolu HTTP: Basic a ověřování algoritmem Digest přístup](http://www.ietf.org/rfc/rfc2617.txt).</span><span class="sxs-lookup"><span data-stu-id="5675e-105">Basic authentication is defined in [RFC 2617, HTTP Authentication: Basic and Digest Access Authentication](http://www.ietf.org/rfc/rfc2617.txt).</span></span>

<span data-ttu-id="5675e-106">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="5675e-106">Disadvantages</span></span>

- <span data-ttu-id="5675e-107">Přihlašovací údaje uživatele jsou zasílány v požadavku.</span><span class="sxs-lookup"><span data-stu-id="5675e-107">User credentials are sent in the request.</span></span>
- <span data-ttu-id="5675e-108">Přihlašovací údaje se odesílají jako prostý text.</span><span class="sxs-lookup"><span data-stu-id="5675e-108">Credentials are sent as plaintext.</span></span>
- <span data-ttu-id="5675e-109">Přihlašovací údaje se odesílají s každou žádostí.</span><span class="sxs-lookup"><span data-stu-id="5675e-109">Credentials are sent with every request.</span></span>
- <span data-ttu-id="5675e-110">Žádný způsob, jak odhlášení, s výjimkou ukončení relace prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="5675e-110">No way to log out, except by ending the browser session.</span></span>
- <span data-ttu-id="5675e-111">Bude zranitelný vůči webů padělání žádosti (proti útokům CSRF); vyžaduje opatření proti proti útokům CSRF.</span><span class="sxs-lookup"><span data-stu-id="5675e-111">Vulnerable to cross-site request forgery (CSRF); requires anti-CSRF measures.</span></span>

<span data-ttu-id="5675e-112">Výhody</span><span class="sxs-lookup"><span data-stu-id="5675e-112">Advantages</span></span>

- <span data-ttu-id="5675e-113">Internetovým standardem.</span><span class="sxs-lookup"><span data-stu-id="5675e-113">Internet standard.</span></span>
- <span data-ttu-id="5675e-114">Podporuje všechny hlavní prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="5675e-114">Supported by all major browsers.</span></span>
- <span data-ttu-id="5675e-115">Relativně jednoduché protokol.</span><span class="sxs-lookup"><span data-stu-id="5675e-115">Relatively simple protocol.</span></span>

<span data-ttu-id="5675e-116">Základní ověřování pracuje takto:</span><span class="sxs-lookup"><span data-stu-id="5675e-116">Basic authentication works as follows:</span></span>

1. <span data-ttu-id="5675e-117">Pokud požadavek vyžaduje ověření, server vrátí 401 (Neautorizováno).</span><span class="sxs-lookup"><span data-stu-id="5675e-117">If a request requires authentication, the server returns 401 (Unauthorized).</span></span> <span data-ttu-id="5675e-118">Odpověď obsahuje hlavičku WWW-Authenticate, což značí, že server podporuje základní ověřování.</span><span class="sxs-lookup"><span data-stu-id="5675e-118">The response includes a WWW-Authenticate header, indicating the server supports Basic authentication.</span></span>
2. <span data-ttu-id="5675e-119">Klient odešle jinou žádost, pomocí přihlašovacích údajů klienta v hlavičce autorizace.</span><span class="sxs-lookup"><span data-stu-id="5675e-119">The client sends another request, with the client credentials in the Authorization header.</span></span> <span data-ttu-id="5675e-120">Přihlašovací údaje jsou formátovány jako řetězec "název: heslo", kódování base64.</span><span class="sxs-lookup"><span data-stu-id="5675e-120">The credentials are formatted as the string "name:password", base64-encoded.</span></span> <span data-ttu-id="5675e-121">Přihlašovací údaje nejsou šifrovány.</span><span class="sxs-lookup"><span data-stu-id="5675e-121">The credentials are not encrypted.</span></span>

<span data-ttu-id="5675e-122">Základní ověřování se provádí v kontextu "sféru."</span><span class="sxs-lookup"><span data-stu-id="5675e-122">Basic authentication is performed within the context of a "realm."</span></span> <span data-ttu-id="5675e-123">Server obsahuje název sféry v záhlaví WWW-Authenticate.</span><span class="sxs-lookup"><span data-stu-id="5675e-123">The server includes the name of the realm in the WWW-Authenticate header.</span></span> <span data-ttu-id="5675e-124">Přihlašovací údaje uživatele jsou platné v rámci této sféry.</span><span class="sxs-lookup"><span data-stu-id="5675e-124">The user's credentials are valid within that realm.</span></span> <span data-ttu-id="5675e-125">Server je definována přesný oboru sféru.</span><span class="sxs-lookup"><span data-stu-id="5675e-125">The exact scope of a realm is defined by the server.</span></span> <span data-ttu-id="5675e-126">Například můžete třeba definovat několik sfér, aby oddíl prostředky.</span><span class="sxs-lookup"><span data-stu-id="5675e-126">For example, you might define several realms in order to partition resources.</span></span>

![](basic-authentication/_static/image1.png)

<span data-ttu-id="5675e-127">Protože přihlašovací údaje se odesílají nezašifrované, základní ověřování je pouze zabezpečené pomocí protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="5675e-127">Because the credentials are sent unencrypted, Basic authentication is only secure over HTTPS.</span></span> <span data-ttu-id="5675e-128">V tématu [práce s protokolem SSL v rozhraní Web API](working-with-ssl-in-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="5675e-128">See [Working with SSL in Web API](working-with-ssl-in-web-api.md).</span></span>

<span data-ttu-id="5675e-129">Základní ověřování je také bude zranitelný vůči útoku proti útokům CSRF.</span><span class="sxs-lookup"><span data-stu-id="5675e-129">Basic authentication is also vulnerable to CSRF attacks.</span></span> <span data-ttu-id="5675e-130">Poté, co uživatel zadá přihlašovací údaje, prohlížeč automaticky odesílá je na následné žádosti do stejné domény, po dobu trvání relace.</span><span class="sxs-lookup"><span data-stu-id="5675e-130">After the user enters credentials, the browser automatically sends them on subsequent requests to the same domain, for the duration of the session.</span></span> <span data-ttu-id="5675e-131">To zahrnuje požadavky AJAX.</span><span class="sxs-lookup"><span data-stu-id="5675e-131">This includes AJAX requests.</span></span> <span data-ttu-id="5675e-132">V tématu [prevence útoků (proti útokům CSRF) padělání požadavku posílaného mezi weby](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="5675e-132">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>

## <a name="basic-authentication-with-iis"></a><span data-ttu-id="5675e-133">Základní ověřování pomocí služby IIS</span><span class="sxs-lookup"><span data-stu-id="5675e-133">Basic Authentication with IIS</span></span>

<span data-ttu-id="5675e-134">Služba IIS podporuje základní ověřování, ale je přímý přístup paměti: ověření uživatele před jejich přihlašovací údaje systému Windows.</span><span class="sxs-lookup"><span data-stu-id="5675e-134">IIS supports Basic authentication, but there is a caveat: The user is authenticated against their Windows credentials.</span></span> <span data-ttu-id="5675e-135">To znamená, že uživatel musí mít účet v doméně serveru.</span><span class="sxs-lookup"><span data-stu-id="5675e-135">That means the user must have an account on the server's domain.</span></span> <span data-ttu-id="5675e-136">Pro veřejné webový server budete chtít obvykle ověřují se zprostředkovatelem členství prostředí ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5675e-136">For a public-facing web site, you typically want to authenticate against an ASP.NET membership provider.</span></span>

<span data-ttu-id="5675e-137">Pokud chcete povolit základní ověřování pomocí služby IIS, nastavte režim ověřování "Systém Windows" v souboru Web.config vašeho projektu ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="5675e-137">To enable Basic authentication using IIS, set the authentication mode to "Windows" in the Web.config of your ASP.NET project:</span></span>

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

<span data-ttu-id="5675e-138">V tomto režimu služba IIS používá přihlašovací údaje Windows k ověření.</span><span class="sxs-lookup"><span data-stu-id="5675e-138">In this mode, IIS uses Windows credentials to authenticate.</span></span> <span data-ttu-id="5675e-139">Kromě toho je třeba povolit základní ověřování ve službě IIS.</span><span class="sxs-lookup"><span data-stu-id="5675e-139">In addition, you must enable Basic authentication in IIS.</span></span> <span data-ttu-id="5675e-140">Ve Správci služby IIS, přejděte do zobrazení funkce, vyberte možnost ověřování a povolit základní ověřování.</span><span class="sxs-lookup"><span data-stu-id="5675e-140">In IIS Manager, go to Features View, select Authentication, and enable Basic authentication.</span></span>

![](basic-authentication/_static/image2.png)

<span data-ttu-id="5675e-141">V projektu webového rozhraní API, přidejte `[Authorize]` atribut pro všechny akce kontroleru, které je třeba ověřování.</span><span class="sxs-lookup"><span data-stu-id="5675e-141">In your Web API project, add the `[Authorize]` attribute for any controller actions that need authentication.</span></span>

<span data-ttu-id="5675e-142">Klient se ověří nastavením autorizační hlavičky v požadavku.</span><span class="sxs-lookup"><span data-stu-id="5675e-142">A client authenticates itself by setting the Authorization header in the request.</span></span> <span data-ttu-id="5675e-143">Tento krok proveďte klienty prohlížeče automaticky.</span><span class="sxs-lookup"><span data-stu-id="5675e-143">Browser clients perform this step automatically.</span></span> <span data-ttu-id="5675e-144">Klienti nonbrowser muset nastavit hlavičku.</span><span class="sxs-lookup"><span data-stu-id="5675e-144">Nonbrowser clients will need to set the header.</span></span>

## <a name="basic-authentication-with-custom-membership"></a><span data-ttu-id="5675e-145">Základní ověřování s vlastní členství</span><span class="sxs-lookup"><span data-stu-id="5675e-145">Basic Authentication with Custom Membership</span></span>

<span data-ttu-id="5675e-146">Jak je uvedeno, základní ověřování, které jsou integrovány do služby IIS používá přihlašovací údaje systému Windows.</span><span class="sxs-lookup"><span data-stu-id="5675e-146">As mentioned, the Basic Authentication built into IIS uses Windows credentials.</span></span> <span data-ttu-id="5675e-147">To znamená, že je potřeba vytvořit uživatelské účty na hostitelském serveru.</span><span class="sxs-lookup"><span data-stu-id="5675e-147">That means you need to create accounts for your users on the hosting server.</span></span> <span data-ttu-id="5675e-148">Ale pro internetovou aplikaci, uživatelské účty jsou obvykle uložena v externí databáze.</span><span class="sxs-lookup"><span data-stu-id="5675e-148">But for an internet application, user accounts are typically stored in an external database.</span></span>

<span data-ttu-id="5675e-149">Následující kód, jak modul HTTP, která provádí základní ověřování.</span><span class="sxs-lookup"><span data-stu-id="5675e-149">The following code how an HTTP module that performs Basic Authentication.</span></span> <span data-ttu-id="5675e-150">Můžete snadno zařadit zprostředkovatelem členství prostředí ASP.NET tak, že nahradíte `CheckPassword` metodu, která je fiktivní metoda v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="5675e-150">You can easily plug in an ASP.NET membership provider by replacing the `CheckPassword` method, which is a dummy method in this example.</span></span>

<span data-ttu-id="5675e-151">Ve webovém rozhraní API 2, byste měli zvážit zápis [ověřování filtru](authentication-filters.md) nebo [OWIN middleware](../../../aspnet/overview/owin-and-katana/index.md), místo modulu HTTP.</span><span class="sxs-lookup"><span data-stu-id="5675e-151">In Web API 2, you should consider writing an [authentication filter](authentication-filters.md) or [OWIN middleware](../../../aspnet/overview/owin-and-katana/index.md), instead of an HTTP module.</span></span>

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

<span data-ttu-id="5675e-152">Chcete-li povolit modul HTTP, přidejte následující v souboru web.config **system.webServer** části:</span><span class="sxs-lookup"><span data-stu-id="5675e-152">To enable the HTTP module, add the following to your web.config file in the **system.webServer** section:</span></span>

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

<span data-ttu-id="5675e-153">"YourAssemblyName" nahraďte název sestavení (včetně není rozšíření "dll").</span><span class="sxs-lookup"><span data-stu-id="5675e-153">Replace "YourAssemblyName" with the name of the assembly (not including the "dll" extension).</span></span>

<span data-ttu-id="5675e-154">Měli byste zakázat další metody ověřování, jako jsou formuláře nebo systému Windows umožňuje</span><span class="sxs-lookup"><span data-stu-id="5675e-154">You should disable other authentication schemes, such as Forms or Windows auth.</span></span>
