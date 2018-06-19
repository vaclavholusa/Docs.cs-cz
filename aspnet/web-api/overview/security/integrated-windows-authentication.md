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
<a name="integrated-windows-authentication"></a>Integrované ověřování systému Windows
====================
podle [Wasson Jan](https://github.com/MikeWasson)

Integrované ověřování systému Windows umožňuje uživatelům přihlásit pomocí svých pověření systému Windows pomocí protokolu Kerberos nebo NTLM. Klient odešle přihlašovací údaje v hlavičce autorizace. Ověřování systému Windows je nejvhodnější pro prostředí intranetu. Další informace najdete v tématu [ověřování systému Windows](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).

| Výhody | Nevýhody |
| --- | --- |
| -Integrované ve službě IIS. -Neodešle přihlašovací údaje uživatele v požadavku. – Pokud je klientský počítač patří do domény (například intranet aplikace), není potřeba zadat přihlašovací údaje uživatele. | -Není doporučena pro internetové aplikace. -Vyžaduje podporu protokolu Kerberos nebo NTLM v klientovi. -Klienta musí být v doméně služby Active Directory. |

> [!NOTE]
> Pokud je vaše aplikace hostována v Azure a budete mít místní doméně Active Directory, vezměte v úvahu federaci vaše místní AD pomocí služby Azure Active Directory. Tímto způsobem uživatelé přihlásit pomocí svých přihlašovacích údajů na místní, ale ověřování se provádí pomocí služby Azure AD. Další informace najdete v tématu [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).


Pokud chcete vytvořit aplikaci, která používá integrované ověřování systému Windows, vyberte šablonu "Intranetu aplikace" v Průvodci projekt MVC 4. Tato šablona projektu vloží následující nastavení v souboru Web.config:

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

Na straně klienta, integrované ověřování systému Windows funguje s žádný prohlížeč, který podporuje [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) schéma ověřování, který obsahuje většinu hlavní prohlížeče. Pro klientské aplikace .NET **HttpClient** třída podporuje ověřování systému Windows:

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

Ověřování systému Windows je zranitelný vůči webů útoky padělání (proti útokům CSRF) požadavku. V tématu [prevence útoků (proti útokům CSRF) padělání požadavku posílaného mezi weby](preventing-cross-site-request-forgery-csrf-attacks.md).
