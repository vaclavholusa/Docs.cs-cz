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
<a name="integrated-windows-authentication"></a>Ověření integrované Windows
====================
podle [Mike Wasson](https://github.com/MikeWasson)

Integrované ověřování Windows umožňuje uživatelům přihlásit se pomocí svých přihlašovacích údajů Windows, pomocí protokolu Kerberos nebo NTLM. Klient odešle přihlašovací údaje v autorizační hlavičce. Ověřování Windows je nejvhodnější pro prostředí intranetu. Další informace najdete v tématu [ověřování Windows](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).

| Výhody | Nevýhody |
| --- | --- |
| -Integrovaná do služby IIS. -Neodesílá přihlašovacích údajů uživatele v požadavku. – Pokud je klientský počítač patří do domény (například intranet aplikace), není nutné zadávat přihlašovací údaje uživatele. | -Není doporučena pro internetové aplikace. -Vyžaduje podporu protokolu Kerberos nebo NTLM v klientovi. -Klient musí být v doméně služby Active Directory. |

> [!NOTE]
> Pokud je vaše aplikace hostovaná v Azure a budete mít místní doméně služby Active Directory, vezměte v úvahu federaci s Azure Active Directory vaše místní službě AD. Tímto způsobem uživatelé přihlásit pomocí svých přihlašovacích údajů místního, ale ve službě Azure AD provádí ověřování. Další informace najdete v tématu [ověřování Azure](../../../visual-studio/overview/2012/windows-azure-authentication.md).


Chcete-li vytvořit aplikaci, která se používá ověření integrované Windows, vyberte šablonu "Intranetové aplikace" v Průvodci vytvořením projektu MVC 4. Tuto šablonu projektu umístí následující nastavení v souboru Web.config:

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

Na straně klienta funguje ověření integrované Windows pomocí libovolného prohlížeče, který podporuje [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) schéma ověřování, která zahrnuje většinu hlavních prohlížečů. Pro klientské aplikace .NET **HttpClient** třída podporuje ověřování Windows:

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

Ověřování Windows je ohrožen útoky proti padělání (CSRF) žádosti více webů. Zobrazit [prevence útoků proti padělání (CSRF) podvržení žádosti](preventing-cross-site-request-forgery-csrf-attacks.md).
