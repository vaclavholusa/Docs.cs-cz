---
uid: web-api/overview/security/integrated-windows-authentication
title: Integrované ověřování systému Windows | Microsoft Docs
author: MikeWasson
description: Popisuje použití integrovaného ověřování systému Windows v rozhraní ASP.NET Web API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/18/2012
ms.topic: article
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: bf5f55d98d61cdfdd246a847f41a6f1c65f00bfc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566752"
---
<a name="integrated-windows-authentication"></a><span data-ttu-id="6ac87-103">Integrované ověřování systému Windows</span><span class="sxs-lookup"><span data-stu-id="6ac87-103">Integrated Windows Authentication</span></span>
====================
<span data-ttu-id="6ac87-104">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6ac87-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="6ac87-105">Integrované ověřování systému Windows umožňuje uživatelům přihlásit pomocí svých pověření systému Windows pomocí protokolu Kerberos nebo NTLM.</span><span class="sxs-lookup"><span data-stu-id="6ac87-105">Integrated Windows authentication enables users to log in with their Windows credentials, using Kerberos or NTLM.</span></span> <span data-ttu-id="6ac87-106">Klient odešle přihlašovací údaje v hlavičce autorizace.</span><span class="sxs-lookup"><span data-stu-id="6ac87-106">The client sends credentials in the Authorization header.</span></span> <span data-ttu-id="6ac87-107">Ověřování systému Windows je nejvhodnější pro prostředí intranetu.</span><span class="sxs-lookup"><span data-stu-id="6ac87-107">Windows authentication is best suited for an intranet environment.</span></span> <span data-ttu-id="6ac87-108">Další informace najdete v tématu [ověřování systému Windows](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span><span class="sxs-lookup"><span data-stu-id="6ac87-108">For more information, see [Windows Authentication](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span></span>

| <span data-ttu-id="6ac87-109">Výhody</span><span class="sxs-lookup"><span data-stu-id="6ac87-109">Advantages</span></span> | <span data-ttu-id="6ac87-110">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="6ac87-110">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="6ac87-111">-Integrované ve službě IIS.</span><span class="sxs-lookup"><span data-stu-id="6ac87-111">- Built into IIS.</span></span> <span data-ttu-id="6ac87-112">-Neodešle přihlašovací údaje uživatele v požadavku.</span><span class="sxs-lookup"><span data-stu-id="6ac87-112">- Does not send the user credentials in the request.</span></span> <span data-ttu-id="6ac87-113">– Pokud je klientský počítač patří do domény (například intranet aplikace), není potřeba zadat přihlašovací údaje uživatele.</span><span class="sxs-lookup"><span data-stu-id="6ac87-113">- If the client computer belongs to the domain (for example, intranet application), the user does not need to enter credentials.</span></span> | <span data-ttu-id="6ac87-114">-Není doporučena pro internetové aplikace.</span><span class="sxs-lookup"><span data-stu-id="6ac87-114">- Not recommended for Internet applications.</span></span> <span data-ttu-id="6ac87-115">-Vyžaduje podporu protokolu Kerberos nebo NTLM v klientovi.</span><span class="sxs-lookup"><span data-stu-id="6ac87-115">- Requires Kerberos or NTLM support in the client.</span></span> <span data-ttu-id="6ac87-116">-Klienta musí být v doméně služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6ac87-116">- Client must be in the Active Directory domain.</span></span> |

> [!NOTE]
> <span data-ttu-id="6ac87-117">Pokud je vaše aplikace hostována v Azure a budete mít místní doméně Active Directory, vezměte v úvahu federaci vaše místní AD pomocí služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6ac87-117">If your application is hosted on Azure and you have an on-premise Active Directory domain, consider federating your on-premise AD with Azure Active Directory.</span></span> <span data-ttu-id="6ac87-118">Tímto způsobem uživatelé přihlásit pomocí svých přihlašovacích údajů na místní, ale ověřování se provádí pomocí služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6ac87-118">That way, users can log in with their on-premise credentials, but the authentication is performed by Azure AD.</span></span> <span data-ttu-id="6ac87-119">Další informace najdete v tématu [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="6ac87-119">For more information, see [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span></span>


<span data-ttu-id="6ac87-120">Pokud chcete vytvořit aplikaci, která používá integrované ověřování systému Windows, vyberte šablonu "Intranetu aplikace" v Průvodci projekt MVC 4.</span><span class="sxs-lookup"><span data-stu-id="6ac87-120">To create an application that uses Integrated Windows authentication, select the "Intranet Application" template in the MVC 4 project wizard.</span></span> <span data-ttu-id="6ac87-121">Tato šablona projektu vloží následující nastavení v souboru Web.config:</span><span class="sxs-lookup"><span data-stu-id="6ac87-121">This project template puts the following setting in the Web.config file:</span></span>

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

<span data-ttu-id="6ac87-122">Na straně klienta, integrované ověřování systému Windows funguje s žádný prohlížeč, který podporuje [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) schéma ověřování, který obsahuje většinu hlavní prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="6ac87-122">On the client side, Integrated Windows authentication works with any browser that supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) authentication scheme, which includes most major browsers.</span></span> <span data-ttu-id="6ac87-123">Pro klientské aplikace .NET **HttpClient** třída podporuje ověřování systému Windows:</span><span class="sxs-lookup"><span data-stu-id="6ac87-123">For .NET client applications, the **HttpClient** class supports Windows authentication:</span></span>

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

<span data-ttu-id="6ac87-124">Ověřování systému Windows je zranitelný vůči webů útoky padělání (proti útokům CSRF) požadavku.</span><span class="sxs-lookup"><span data-stu-id="6ac87-124">Windows authentication is vulnerable to cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="6ac87-125">V tématu [prevence útoků (proti útokům CSRF) padělání požadavku posílaného mezi weby](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="6ac87-125">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>
