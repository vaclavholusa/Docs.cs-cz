---
title: "Prevence útoků přesměrování otevřít v aplikaci ASP.NET Core | Microsoft Docs"
author: ardalis
description: "Ukazuje, jak zabránit otevřete přesměrování útoky na aplikace ASP.NET Core"
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/preventing-open-redirects
ms.openlocfilehash: 6ecf2440ac7073bdad098f6fe48f6c788ba7795a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="preventing-open-redirect-attacks-in-an-aspnet-core-app"></a>Prevence útoků přesměrování otevřít v aplikaci ASP.NET Core

Webové aplikace, který přesměruje na adresu URL, která je zadána prostřednictvím požadavku například řetězci dotazu nebo formuláře dat může potenciálně manipulováno k přesměrování uživatelů na externí, škodlivý URL. Tato manipulaci se nazývá útok otevřete přesměrování.

Vždy, když vaše aplikace logiky přesměruje na zadané adrese URL, je nutné ověřit, že adresu URL pro přesměrování nikdo neoprávněně nemanipuloval. ASP.NET Core obsahuje integrovanou funkci k ochraně aplikace před útoky otevřete přesměrování (označované také jako otevřený přesměrování).

## <a name="what-is-an-open-redirect-attack"></a>Co je útok otevřete přesměrování?

Při přístupu k prostředkům, které vyžadují ověřování webových aplikací často přesměrovat uživatele na přihlašovací stránku. Zahrnuje typlically přesměrování `returnUrl` parametr řetězce dotazu tak, aby uživatel se může vracet na původně požadovanou URL adresu po jejich úspěšně přihlášeni. Poté, co se uživatel ověřuje, že přesměrováni na měl původně požadovanou adresu URL.

Vzhledem k tomu, že cílová adresa URL je zadána v řetězci dotazu požadavku, uživatel se zlými úmysly může manipulovat s řetězci dotazu. Zmanipulovanou řetězce dotazu by se mohl lokality tak, aby přesměruje uživatele na stránku externí, škodlivý. Tento postup se nazývá útok otevřete přesměrování (nebo přesměrování).

### <a name="an-example-attack"></a>Příklad útoku

Uživatel se zlými úmysly může vyvíjet útok díky uživatel se zlými úmysly přístup k pověření uživatele nebo citlivé informace ve vaší aplikaci. Zahájit útoku, jejich přimět kliknutím na odkaz na přihlašovací stránku vašeho webu, s uživateli `returnUrl` hodnotu querystring adresu URL. Například [NerdDinner.com](http://nerddinner.com) ukázkovou aplikaci (napsané pro rozhraní ASP.NET MVC) zahrnuje takové přihlašovací stránky zde: ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``. Útoku pak postupujte podle následujících kroků:

1. Uživatel klikne na odkaz ``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn`` (Všimněte si, druhý adresa URL je nerddi**n**če, není nerddi**nn**če).
2. Uživatel se přihlásí úspěšně.
3. Uživatel je přesměrován (lokalitou) na ``http://nerddiner.com/Account/LogOn`` (škodlivé weby, které vypadá skutečné lokality).
4. Uživatel znovu přihlásí (poskytnutí škodlivý lokality své přihlašovací údaje) a je přesměrován zpět na web skutečné.

Uživatel se pravděpodobně domníváte jejich první pokus o přihlášení se nezdařilo a jejich druhý byla úspěšná. S největší pravděpodobností zůstanou nebere v úvahu ohrožený přihlašovacích údajů.

![Proces útoku otevřete přesměrování](preventing-open-redirects/_static/open-redirection-attack-process.png)

Některé servery kromě přihlašovací stránky, zadejte přesměrování stránky nebo koncové body. Představte si aplikace se zobrazí stránka s otevřete přesměrování, ``/Home/Redirect``. Útočník by mohl vytvořit, například pomocí odkazu v e-mailu, který přejde na ``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``. Běžný uživatel na adrese URL a zjistit, že začíná název vaší lokality. Důvěřující, který, bude kliknutím na odkaz. Otevřete přesměrování by uživatel pak pošlete do lokality phishing, která vypadá totožná s tímto počítačem, a uživatel by pravděpodobně přihlášení, které budou věřit je váš web.

## <a name="protecting-against-open-redirect-attacks"></a>Ochrana proti útokům na otevřete přesměrování

Při vývoji webových aplikací, považovat za nedůvěryhodným všechna data zadaný uživatelem. Pokud aplikace obsahuje funkce, který přesměruje uživatele na základě obsahu adresy URL, zajistěte, aby takové přesměrování jsou pouze místně v rámci vaší aplikace (nebo známé adresy URL, není libovolnou URL, která může být zadána v řetězci dotazu).

### <a name="localredirect"></a>LocalRedirect

Použití ``LocalRedirect`` Pomocná metoda od základní `Controller` třídy:

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

``LocalRedirect``bude vyvolána výjimka, pokud je zadaná adresa URL není místní. Jinak se chová podobně jako ``Redirect`` metoda.

### <a name="islocalurl"></a>IsLocalUrl

Použití [IsLocalUrl](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iurlhelper#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) metoda před přesměrování otestovat adresy URL:

Následující příklad ukazuje, jak zkontrolovat, zda je adresa URL místní před přesměrování.

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

`IsLocalUrl` Metoda chrání uživatelé z nechtěně přesměrování na škodlivé weby. Přihlaste se na podrobnosti o adresu URL, který byl poskytnut, pokud nejsou místní adresa URL je zadáno v situaci, kde je očekávána místní adresa URL. Protokolování přesměrování adresy URL může pomoci při diagnostice přesměrování útoky.
