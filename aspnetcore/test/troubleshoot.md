---
title: Řešení potíží s projekty ASP.NET Core
author: Rick-Anderson
description: Rady pro pochopení a řešení potíží s upozornění a chyby s projekty ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 04/05/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: test/troubleshoot
ms.openlocfilehash: 8ff8ddaf4a35a02db650ff429ffbbf4e76a53ecf
ms.sourcegitcommit: 4e3497bda0c3e5011ffba3717eb61a1d46c61c15
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/14/2018
ms.locfileid: "35613002"
---
# <a name="troubleshoot-aspnet-core-projects"></a>Řešení potíží s projekty ASP.NET Core

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Následující odkazy obsahují pokyny k odstraňování problémů:

* [Řešení potíží s ASP.NET Core ve službě Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)
* [Řešení potíží s ASP.NET Core ve službě IIS](xref:host-and-deploy/iis/troubleshoot)
* [Běžné chyby referenční dokumentace pro Azure App Service a IIS s ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [NDC konference (Londýn, 2018): Diagnostika problémů s v aplikacích ASP.NET Core](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [ASP.NET Blog: Řešení potíží s problémy s výkonem ASP.NET Core](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a>Upozornění na .NET core SDK

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>Instalaci 32bitové a 64bitové verze rozhraní .NET Core SDK

V **nový projekt** dialogové okno pro ASP.NET Core, může se zobrazit následující varování:

> 32 a 64 bitových verzích .NET Core SDK jsou nainstalovány. Pouze šablony z verze 64bitová verze nainstalované v ' C:\\Program Files\\dotnet\\sdk\\, se zobrazí.

![Snímek obrazovky dialogového okna OneASP.NET zobrazující upozornění](troubleshoot/_static/both32and64bit.png)

Toto upozornění se zobrazí, když (x86) 32bitovou i 64bitovou (x 64) verze [.NET Core SDK](https://www.microsoft.com/net/download/all) jsou nainstalovány. Obě verze může být nainstalována obvyklé příčiny patří:

* Jste původně stáhnout instalační program rozhraní .NET Core SDK s použitím 32bitový počítač, ale pak zkopírovali ho napříč a nainstalován na 64bitový počítač.
* 32bitová verze rozhraní .NET Core SDK byla nainstalována jiná aplikace.
* Nesprávné verze byla stáhli a nainstalovali.

Odinstalujte 32bitová verze rozhraní .NET Core SDK aby toto upozornění. Odinstalovat z **ovládací panely** > **programy a funkce** > **odinstalovat nebo změnit program**. Pokud budete rozumět tomu, proč dochází upozornění a jeho dopad, můžete upozornění ignorovat.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>.NET Core SDK je nainstalován na více místech

V **nový projekt** dialogové okno pro ASP.NET Core, může se zobrazit následující varování:

> .NET Core SDK je nainstalována na více místech. Pouze šablony z SDK(s) nainstalovaným v ' C:\\Program Files\\dotnet\\sdk\\, se zobrazí.

![Snímek obrazovky dialogového okna OneASP.NET zobrazující upozornění](troubleshoot/_static/multiplelocations.png)

Se zobrazí tato zpráva, když máte alespoň jedna instalace rozhraní .NET Core SDK v adresáři mimo *C:\\Program Files\\dotnet\\sdk\\*. Obvykle se to stane, když .NET Core SDK nasazený na počítači použití, kopírování a vkládání namísto Instalační služby MSI.

Odinstalujte 32bitová verze rozhraní .NET Core SDK aby toto upozornění. Odinstalovat z **ovládací panely** > **programy a funkce** > **odinstalovat nebo změnit program**. Pokud budete rozumět tomu, proč dochází upozornění a jeho dopad, můžete upozornění ignorovat.

### <a name="no-net-core-sdks-were-detected"></a>Nebyly zjištěny žádné .NET Core SDK

V **nový projekt** dialogové okno pro ASP.NET Core, může se zobrazit následující varování:

> Nebyly zjištěny žádné .NET Core SDK, ujistěte se, že jsou zahrnuty v proměnné prostředí 'PATH'.

![Snímek obrazovky dialogového okna OneASP.NET zobrazující upozornění](troubleshoot/_static/NoNetCore.png)

Toto upozornění se zobrazí, když proměnnou prostředí `PATH` neodkazuje na žádné rozhraní .NET Core SDK na počítači. Chcete-li tento problém vyřešit:

* Nainstalujte nebo ověřte, že je nainstalováno rozhraní .NET Core SDK.
* Ověřte `PATH` proměnnou prostředí odkazuje na umístění, je nainstalována sada SDK. Instalační program za normálních okolností nastaví `PATH`.

::: moniker range=">= aspnetcore-2.1"

### <a name="use-of-ihtmlhelperpartial-may-result-in-app-deadlocks"></a>Použití IHtmlHelper.Partial může vést k zablokování aplikace

V technologii ASP.NET Core 2.1 nebo novější, volání `Html.Partial` vede upozornění analyzátor kvůli možným zablokování. Upozornění je:

> Použití IHtmlHelper.Partial může vést k zablokování aplikace. Zvažte použití `<partial>` pomocná značka nebo `IHtmlHelper.PartialAsync`.

Volání `@Html.Partial` by měl být nahrazen `@await Html.PartialAsync` nebo částečné značky pomocná `<partial name="_Partial" />`.

::: moniker-end
