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
# <a name="enabling-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="06dc9-103">Povolení generování kód QR pro aplikace v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="06dc9-103">Enabling QR Code generation for authenticator apps in ASP.NET Core</span></span>

<span data-ttu-id="06dc9-104">Poznámka: Toto téma se vztahuje na ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="06dc9-104">Note: This topic applies to ASP.NET Core 2.x</span></span>

<span data-ttu-id="06dc9-105">ASP.NET Core se dodává s podporou pro ověřovací aplikací pro jednotlivé ověřování.</span><span class="sxs-lookup"><span data-stu-id="06dc9-105">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="06dc9-106">Dva faktor ověřování (2FA), použití založené na čase jednorázové heslo algoritmus (TOTP), jsou tyto aplikace doporučenému přístupu pro 2FA odvětví.</span><span class="sxs-lookup"><span data-stu-id="06dc9-106">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="06dc9-107">2FA pomocí TOTP je upřednostňovaný k 2FA serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="06dc9-107">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="06dc9-108">Ověřovací aplikaci poskytuje 6 až 8 číslice kódu, které uživatelé musí zadat po potvrzení uživatelského jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="06dc9-108">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="06dc9-109">Ověřovací aplikaci je obvykle nainstalován na Smartphone.</span><span class="sxs-lookup"><span data-stu-id="06dc9-109">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="06dc9-110">Šablony webové aplikace ASP.NET Core podporovat ověřovací data, ale nemáte poskytují podporu pro generování QRCode.</span><span class="sxs-lookup"><span data-stu-id="06dc9-110">The ASP.NET Core web app templates support authenticators, but don't provide support for QRCode generation.</span></span> <span data-ttu-id="06dc9-111">Generátory QRCode usnadňují nastavení 2FA.</span><span class="sxs-lookup"><span data-stu-id="06dc9-111">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="06dc9-112">Tento dokument vás provede přidáním [kód QR](https://wikipedia.org/wiki/QR_code) generování na stránku konfigurace 2FA.</span><span class="sxs-lookup"><span data-stu-id="06dc9-112">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="06dc9-113">Přidání kódy QR na stránku konfigurace 2FA</span><span class="sxs-lookup"><span data-stu-id="06dc9-113">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="06dc9-114">Tyto pokyny používají *qrcode.js* z https://davidshimjs.github.io/qrcodejs/ úložiště.</span><span class="sxs-lookup"><span data-stu-id="06dc9-114">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="06dc9-115">Stažení [knihovna javascript qrcode.js](https://davidshimjs.github.io/qrcodejs/) k `wwwroot\lib` složku ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="06dc9-115">Download the [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

* <span data-ttu-id="06dc9-116">V *Pages\Account\Manage\EnableAuthenticator.cshtml* (stránky Razor) nebo *Views\Manage\EnableAuthenticator.cshtml* (MVC), vyhledejte `Scripts` na konci souboru:</span><span class="sxs-lookup"><span data-stu-id="06dc9-116">In *Pages\Account\Manage\EnableAuthenticator.cshtml* (Razor Pages) or *Views\Manage\EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="06dc9-117">Aktualizace `Scripts` části se přidat odkaz na `qrcodejs` knihovny, které jste přidali a volání generovat kód QR.</span><span class="sxs-lookup"><span data-stu-id="06dc9-117">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="06dc9-118">By měla vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="06dc9-118">It should look as follows:</span></span>

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

* <span data-ttu-id="06dc9-119">Odstraňte odstavce, který odkazy na tyto pokyny.</span><span class="sxs-lookup"><span data-stu-id="06dc9-119">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="06dc9-120">Spuštění aplikace a zkontrolujte, zda jste naskenujte kód QR a ověřit kód, který prokáže ověřovacích.</span><span class="sxs-lookup"><span data-stu-id="06dc9-120">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="06dc9-121">Změňte název webu v kód QR</span><span class="sxs-lookup"><span data-stu-id="06dc9-121">Change the site name in the QR Code</span></span>

<span data-ttu-id="06dc9-122">Název webu v kód QR je převzat ze název projektu, který si zvolíte při počátečním vytvoření projektu.</span><span class="sxs-lookup"><span data-stu-id="06dc9-122">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="06dc9-123">Můžete ji změnit tak, že vyhledá `GenerateQrCodeUri(string email, string unformattedKey)` metoda v *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* souboru (stránky Razor) nebo *Controllers\ManageController.cs* (MVC) souboru.</span><span class="sxs-lookup"><span data-stu-id="06dc9-123">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* (Razor Pages) file or the *Controllers\ManageController.cs* (MVC) file.</span></span> 

<span data-ttu-id="06dc9-124">Ve výchozím kódu ze šablony vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="06dc9-124">The default code from the template looks as follows:</span></span>

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

<span data-ttu-id="06dc9-125">Druhý parametr ve volání `string.Format` je název lokality, provedených od název řešení.</span><span class="sxs-lookup"><span data-stu-id="06dc9-125">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="06dc9-126">Lze změnit na jakoukoli hodnotu, ale musí být vždycky kódovaná adresou URL.</span><span class="sxs-lookup"><span data-stu-id="06dc9-126">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="06dc9-127">Pomocí různých kód QR knihovny</span><span class="sxs-lookup"><span data-stu-id="06dc9-127">Using a different QR Code library</span></span>

<span data-ttu-id="06dc9-128">Kód QR knihovny můžete nahradit své upřednostňované knihovny.</span><span class="sxs-lookup"><span data-stu-id="06dc9-128">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="06dc9-129">Obsahuje kód HTML `qrCode` obsahuje element, do kterého můžete umístit kód QR podle jakýmkoli své knihovny.</span><span class="sxs-lookup"><span data-stu-id="06dc9-129">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="06dc9-130">Je k dispozici v správně formátovaného adresa URL pro kód QR:</span><span class="sxs-lookup"><span data-stu-id="06dc9-130">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="06dc9-131">`AuthenticatorUri`Vlastnost modelu.</span><span class="sxs-lookup"><span data-stu-id="06dc9-131">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="06dc9-132">`data-url`Vlastnost `qrCodeData` elementu.</span><span class="sxs-lookup"><span data-stu-id="06dc9-132">`data-url` property in the `qrCodeData` element.</span></span> 

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="06dc9-133">TOTP klientských a serverových čas zkosení</span><span class="sxs-lookup"><span data-stu-id="06dc9-133">TOTP client and server time skew</span></span>

<span data-ttu-id="06dc9-134">Ověření TOTP závisí na serveru a ověřovací zařízení má přesnému času.</span><span class="sxs-lookup"><span data-stu-id="06dc9-134">TOTP authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="06dc9-135">Tokeny pouze trvat 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="06dc9-135">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="06dc9-136">Pokud se nedaří přihlášení 2FA TOTP, zkontrolujte, jestli čas serveru přesné a pokud možno synchronizovány s služby přesné NTP.</span><span class="sxs-lookup"><span data-stu-id="06dc9-136">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>
