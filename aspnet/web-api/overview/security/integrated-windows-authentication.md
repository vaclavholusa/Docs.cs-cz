---
uid: web-api/overview/security/integrated-windows-authentication
title: Integrované ověřování Windows | Dokumentace Microsoftu
author: MikeWasson
description: Popisuje použití integrovaného ověřování Windows v rozhraní ASP.NET Web API.
ms.author: riande
ms.date: 12/18/2012
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: 13dead421abf7ded73cbb2e5f87e54b1a869b5d4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754900"
---
<a name="integrated-windows-authentication"></a><span data-ttu-id="a6464-103">Ověření integrované Windows</span><span class="sxs-lookup"><span data-stu-id="a6464-103">Integrated Windows Authentication</span></span>
====================
<span data-ttu-id="a6464-104">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a6464-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="a6464-105">Integrované ověřování Windows umožňuje uživatelům přihlásit se pomocí svých přihlašovacích údajů Windows, pomocí protokolu Kerberos nebo NTLM.</span><span class="sxs-lookup"><span data-stu-id="a6464-105">Integrated Windows authentication enables users to log in with their Windows credentials, using Kerberos or NTLM.</span></span> <span data-ttu-id="a6464-106">Klient odešle přihlašovací údaje v autorizační hlavičce.</span><span class="sxs-lookup"><span data-stu-id="a6464-106">The client sends credentials in the Authorization header.</span></span> <span data-ttu-id="a6464-107">Ověřování Windows je nejvhodnější pro prostředí intranetu.</span><span class="sxs-lookup"><span data-stu-id="a6464-107">Windows authentication is best suited for an intranet environment.</span></span> <span data-ttu-id="a6464-108">Další informace najdete v tématu [ověřování Windows](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span><span class="sxs-lookup"><span data-stu-id="a6464-108">For more information, see [Windows Authentication](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span></span>

| <span data-ttu-id="a6464-109">Výhody</span><span class="sxs-lookup"><span data-stu-id="a6464-109">Advantages</span></span> | <span data-ttu-id="a6464-110">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="a6464-110">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="a6464-111">-Integrovaná do služby IIS.</span><span class="sxs-lookup"><span data-stu-id="a6464-111">- Built into IIS.</span></span> <span data-ttu-id="a6464-112">-Neodesílá přihlašovacích údajů uživatele v požadavku.</span><span class="sxs-lookup"><span data-stu-id="a6464-112">- Does not send the user credentials in the request.</span></span> <span data-ttu-id="a6464-113">– Pokud je klientský počítač patří do domény (například intranet aplikace), není nutné zadávat přihlašovací údaje uživatele.</span><span class="sxs-lookup"><span data-stu-id="a6464-113">- If the client computer belongs to the domain (for example, intranet application), the user does not need to enter credentials.</span></span> | <span data-ttu-id="a6464-114">-Není doporučena pro internetové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a6464-114">- Not recommended for Internet applications.</span></span> <span data-ttu-id="a6464-115">-Vyžaduje podporu protokolu Kerberos nebo NTLM v klientovi.</span><span class="sxs-lookup"><span data-stu-id="a6464-115">- Requires Kerberos or NTLM support in the client.</span></span> <span data-ttu-id="a6464-116">-Klient musí být v doméně služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="a6464-116">- Client must be in the Active Directory domain.</span></span> |

> [!NOTE]
> <span data-ttu-id="a6464-117">Pokud je vaše aplikace hostovaná v Azure a budete mít místní doméně služby Active Directory, vezměte v úvahu federaci s Azure Active Directory vaše místní službě AD.</span><span class="sxs-lookup"><span data-stu-id="a6464-117">If your application is hosted on Azure and you have an on-premise Active Directory domain, consider federating your on-premise AD with Azure Active Directory.</span></span> <span data-ttu-id="a6464-118">Tímto způsobem uživatelé přihlásit pomocí svých přihlašovacích údajů místního, ale ve službě Azure AD provádí ověřování.</span><span class="sxs-lookup"><span data-stu-id="a6464-118">That way, users can log in with their on-premise credentials, but the authentication is performed by Azure AD.</span></span> <span data-ttu-id="a6464-119">Další informace najdete v tématu [ověřování Azure](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="a6464-119">For more information, see [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span></span>


<span data-ttu-id="a6464-120">Chcete-li vytvořit aplikaci, která se používá ověření integrované Windows, vyberte šablonu "Intranetové aplikace" v Průvodci vytvořením projektu MVC 4.</span><span class="sxs-lookup"><span data-stu-id="a6464-120">To create an application that uses Integrated Windows authentication, select the "Intranet Application" template in the MVC 4 project wizard.</span></span> <span data-ttu-id="a6464-121">Tuto šablonu projektu umístí následující nastavení v souboru Web.config:</span><span class="sxs-lookup"><span data-stu-id="a6464-121">This project template puts the following setting in the Web.config file:</span></span>

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

<span data-ttu-id="a6464-122">Na straně klienta funguje ověření integrované Windows pomocí libovolného prohlížeče, který podporuje [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) schéma ověřování, která zahrnuje většinu hlavních prohlížečů.</span><span class="sxs-lookup"><span data-stu-id="a6464-122">On the client side, Integrated Windows authentication works with any browser that supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) authentication scheme, which includes most major browsers.</span></span> <span data-ttu-id="a6464-123">Pro klientské aplikace .NET **HttpClient** třída podporuje ověřování Windows:</span><span class="sxs-lookup"><span data-stu-id="a6464-123">For .NET client applications, the **HttpClient** class supports Windows authentication:</span></span>

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

<span data-ttu-id="a6464-124">Ověřování Windows je ohrožen útoky proti padělání (CSRF) žádosti více webů.</span><span class="sxs-lookup"><span data-stu-id="a6464-124">Windows authentication is vulnerable to cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="a6464-125">Zobrazit [prevence útoků proti padělání (CSRF) podvržení žádosti](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="a6464-125">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>
