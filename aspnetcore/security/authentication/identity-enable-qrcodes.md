---
title: "Povolení generování kód QR pro aplikace v ASP.NET Core"
author: rick-anderson
description: "Povolení generování kód QR pro aplikace v ASP.NET Core"
manager: wpickett
ms.author: riande
ms.date: 09/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: cf941314d54aa4a7bd1724805dc62c763ca71dfb
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/03/2018
---
# <a name="enabling-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a>Povolení generování kód QR pro aplikace v ASP.NET Core

Poznámka: Toto téma se vztahuje na ASP.NET Core 2.x

ASP.NET Core se dodává s podporou pro ověřovací aplikací pro jednotlivé ověřování. Dva faktor ověřování (2FA), použití založené na čase jednorázové heslo algoritmus (TOTP), jsou tyto aplikace doporučenému přístupu pro 2FA odvětví. 2FA pomocí TOTP je upřednostňovaný k 2FA serveru SMS. Ověřovací aplikaci poskytuje 6 až 8 číslice kódu, které uživatelé musí zadat po potvrzení uživatelského jména a hesla. Ověřovací aplikaci je obvykle nainstalován na Smartphone.

Šablony webové aplikace ASP.NET Core podporovat ověřovací data, ale nemáte poskytují podporu pro generování QRCode. Generátory QRCode usnadňují nastavení 2FA. Tento dokument vás provede přidáním [kód QR](https://wikipedia.org/wiki/QR_code) generování na stránku konfigurace 2FA.

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a>Přidání kódy QR na stránku konfigurace 2FA

Tyto pokyny používají *qrcode.js* z https://davidshimjs.github.io/qrcodejs/ úložiště.

* Stažení [knihovna javascript qrcode.js](https://davidshimjs.github.io/qrcodejs/) k `wwwroot\lib` složku ve vašem projektu.

* V *Pages\Account\Manage\EnableAuthenticator.cshtml* (stránky Razor) nebo *Views\Manage\EnableAuthenticator.cshtml* (MVC), vyhledejte `Scripts` na konci souboru:

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* Aktualizace `Scripts` části se přidat odkaz na `qrcodejs` knihovny, které jste přidali a volání generovat kód QR. By měla vypadat takto:

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")

    <script type="text/javascript" src="~/lib/qrcode.js"></script>
    <script type="text/javascript">
        new QRCode(document.getElementById("qrCode"),
            {
                text: "@Html.Raw(Model.AuthenticatorUri)",
                width: 150,
                height: 150
            });
    </script>
}
```

* Odstraňte odstavce, který odkazy na tyto pokyny.

Spuštění aplikace a zkontrolujte, zda jste naskenujte kód QR a ověřit kód, který prokáže ověřovacích.

## <a name="change-the-site-name-in-the-qr-code"></a>Změňte název webu v kód QR

Název webu v kód QR je převzat ze název projektu, který si zvolíte při počátečním vytvoření projektu. Můžete ji změnit tak, že vyhledá `GenerateQrCodeUri(string email, string unformattedKey)` metoda v *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* souboru (stránky Razor) nebo *Controllers\ManageController.cs* (MVC) souboru. 

Ve výchozím kódu ze šablony vypadá takto:

```c#
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenicatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

Druhý parametr ve volání `string.Format` je název lokality, provedených od název řešení. Lze změnit na jakoukoli hodnotu, ale musí být vždycky kódovaná adresou URL.

## <a name="using-a-different-qr-code-library"></a>Pomocí různých kód QR knihovny

Kód QR knihovny můžete nahradit své upřednostňované knihovny. Obsahuje kód HTML `qrCode` obsahuje element, do kterého můžete umístit kód QR podle jakýmkoli své knihovny.

Je k dispozici v správně formátovaného adresa URL pro kód QR:

* `AuthenticatorUri`Vlastnost modelu.
* `data-url`Vlastnost `qrCodeData` elementu. 

## <a name="totp-client-and-server-time-skew"></a>TOTP klientských a serverových čas zkosení

Ověření TOTP závisí na serveru a ověřovací zařízení má přesnému času. Tokeny pouze trvat 30 sekund. Pokud se nedaří přihlášení 2FA TOTP, zkontrolujte, jestli čas serveru přesné a pokud možno synchronizovány s služby přesné NTP.
