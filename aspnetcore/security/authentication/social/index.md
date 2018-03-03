---
title: "Facebook, Google a externího poskytovatele ověřování v ASP.NET Core"
author: rick-anderson
description: "Tento kurz ukazuje, jak sestavit ASP.NET Core 2.x aplikaci pomocí externí zprostředkovatelé ověřování OAuth 2.0."
manager: wpickett
ms.author: riande
ms.date: 11/01/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/social/index
ms.openlocfilehash: 76433f814d6850a449434c29eb0bd27570ce193a
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a>Facebook, Google a externího poskytovatele ověřování v ASP.NET Core

<a name="security-authentication-social-logins"></a>

Podle [Valeriy Novytskyy](https://github.com/01binary) a [Rick Anderson](https://twitter.com/RickAndMSFT)

Tento kurz ukazuje, jak sestavit ASP.NET Core 2.x aplikaci, která umožňuje uživatelům přihlásit se pomocí pověření z externí zprostředkovatelé ověřování OAuth 2.0.

[Facebook](facebook-logins.md), [Twitter](twitter-logins.md), [Google](google-logins.md), a [Microsoft](microsoft-logins.md) poskytovatelé jsou popsané v následujících částech. Jiní poskytovatelé jsou k dispozici v balíčky jiných výrobců, jako [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) a [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).

![Ikony sociálních médií pro Facebook, Twitter, Google plus a Windows](index/_static/social.png)

Povolení uživatelům přihlásit se pomocí přihlašovacích údajů existující je vhodné pro uživatele a posune řadu složitosti správy proces přihlášení do jiného výrobce. Příklady, jak sociálních přihlášení můžete jednotka provozu a k zákaznickým převody, najdete v části případové studie podle [Facebook](https://www.facebook.com/unsupportedbrowser) a [Twitter](https://dev.twitter.com/resources/case-studies).

Poznámka: Balíčky uvedené v této abstraktní značnou část složitosti tok ověřování OAuth, ale pochopení podrobnosti může být nutný při řešení potíží. Mnoho prostředky jsou k dispozici. například v tématu [Úvod do OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) nebo [Principy OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/). Některé problémy lze vyřešit prohlížením [ASP.NET Core zdrojového kódu pro balíčky zprostředkovatele](https://github.com/aspnet/Security/tree/dev/src).

## <a name="create-a-new-aspnet-core-project"></a>Vytvořte nový projekt ASP.NET Core

* V aplikaci Visual Studio 2017, vytvořte nový projekt na stránce Start nebo prostřednictvím **soubor > Nový > projekt**.

* Vyberte **webové aplikace ASP.NET Core** šablony, které jsou k dispozici v **Visual C# > .NET Core** kategorie:

![Dialogové okno Nový projekt](index/_static/new-project.png)

* Klepněte na **webové aplikace** a ověřte **ověřování** je nastaven na **jednotlivé uživatelské účty**:

![Dialogové okno nové webové aplikace](index/_static/select-project.png)

Poznámka: V tomto kurzu platí pro verze sady SDK technologie ASP.NET Core 2.0, které lze vybrat v horní části průvodce.

## <a name="apply-migrations"></a>Použijte migrace

* Spusťte aplikaci a vyberte **přihlásit** odkaz.
* Vyberte **zaregistrujte se jako nový uživatel** odkaz.
* Zadejte e-mailu a heslo pro nový účet a potom vyberte **zaregistrovat**.
* Postupujte podle pokynů pro použití migrace.

## <a name="require-ssl"></a>Požadovat protokol SSL

OAuth 2.0 vyžaduje použití protokolu SSL pro ověřování prostřednictvím protokolu HTTPS.

Poznámka: Projekty vytvořené pomocí **webové aplikace** nebo **webového rozhraní API** šablony projektů pro ASP.NET Core 2.x automaticky konfigurují pro povolit protokol SSL a spustit s adresou URL https, pokud **jednotlivých Uživatelské účty** na jste vybrali možnost **dialogové okno Změna ověřování** v Průvodci projektu jak je uvedeno výše.

* Vyžadovat šifrování SSL na svém webu pomocí následujících kroků v [vynucování SSL v aplikaci ASP.NET Core](xref:security/enforcing-ssl) tématu.

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a>Používat SecretManager k uložení tokeny přiřadila zprostředkovatele přihlášení

Přihlášení prostřednictvím sociální sítě poskytovatelů přiřadit **Id aplikace** a **tajný klíč aplikace** tokeny během procesu registrace (přesné pojmenování závisí zprostředkovatele).

Tyto hodnoty jsou efektivně *uživatelské jméno* a *heslo* vaše aplikace používá pro přístup k jejich rozhraní API a tvoří "všechna tajemství" které může být propojený konfiguraci vaší aplikace pomocí **Tajný klíč správce** místo je uložen v konfiguračních souborech přímo nebo přímo v kódu je.

Postupujte podle kroků v [bezpečného úložiště tajné klíče aplikace během vývoje v ASP.NET Core](xref:security/app-secrets) téma tak, aby můžete ukládat tokeny přiřadí každé níže zprostředkovatele přihlášení.

## <a name="setup-login-providers-required-by-your-application"></a>Instalační program zprostředkovatele přihlášení, které jsou požadované aplikací

Ke konfiguraci aplikace pomocí příslušného zprostředkovatele použijte v následujících tématech:

* [Facebook](facebook-logins.md) pokyny
* [Twitter](twitter-logins.md) pokyny
* [Google](google-logins.md) pokyny
* [Microsoft](microsoft-logins.md) pokyny
* [Ostatní poskytovatele](other-logins.md) pokyny

## <a name="optionally-set-password"></a>Volitelně můžete nastavit heslo

Při registraci prostřednictvím poskytovatele externí přihlášení nemáte heslo zaregistrována aplikace. To nebude můžete vytvářet a zapamatování hesla pro lokalitu, ale také udržuje je závislá na na externího zprostředkovatele přihlášení. Pokud není k dispozici na externího zprostředkovatele přihlášení, nebudete moct přihlásit k webu.

Vytvořte heslo a přihlaste se pomocí e-mailu nastavený během v procesu přihlašování s externí zprostředkovatele:

* Klepněte na **Hello <email alias>**  v pravém horním rohu přejděte na odkaz **spravovat** zobrazení.

![Zobrazení Správa webové aplikace](index/_static/pass1a.png)

* Klepněte na **vytvořit**

![Nastavit stránku heslo](index/_static/pass2a.png)

* Nastavte platné heslo a tím můžete se přihlásit pomocí e-mailu.

## <a name="next-steps"></a>Další kroky

* Tento článek zavedená externího ověřování a vysvětlení nezbytné součásti potřebné k přidání externích přihlášení do aplikace ASP.NET Core.

* Referenční stránky specifický pro zprostředkovatele konfigurace přihlášení pro zprostředkovatele požadované aplikace.
