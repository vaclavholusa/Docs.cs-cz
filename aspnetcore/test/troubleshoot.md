---
title: Řešení potíží s projekty ASP.NET Core
author: Rick-Anderson
description: Pochopení a odstraňování potíží upozornění a chyby s projekty ASP.NET Core.
ms.author: riande
ms.date: 04/05/2018
uid: test/troubleshoot
ms.openlocfilehash: b4d90f541a4cda2d41b49101b7ea39af87a4dcb4
ms.sourcegitcommit: a09820f91e71a7d98b7347bf93210abb9e995e22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889009"
---
# <a name="troubleshoot-aspnet-core-projects"></a>Řešení potíží s projekty ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Následující odkazy obsahují pokyny k odstraňování problémů:

* [Řešení potíží s ASP.NET Core ve službě Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)
* [Řešení potíží s ASP.NET Core ve službě IIS](xref:host-and-deploy/iis/troubleshoot)
* [Referenční informace o běžných chybách pro Azure App Service a IIS s ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Konference Norwegian (Praha, 2018): Diagnostika problémů v aplikacích ASP.NET Core](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [Blog o ASP.NET: Řešení potíží s ASP.NET Core problémy s výkonem](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a>Upozornění na .NET core SDK

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>Jsou nainstalovány 32bitové a 64bitové verze sady .NET Core SDK

V **nový projekt** dialogové okno pro ASP.NET Core, může zobrazit následující upozornění:

> 32bitová i 64bitová verze sady .NET Core SDK jsou nainstalovány. Pouze šablony z 64bitové verze nainstalované na "C:\\Program Files\\dotnet\\sdk\\" se zobrazí.

![Snímek obrazovky dialogového okna OneASP.NET zprávou upozornění](troubleshoot/_static/both32and64bit.png)

Toto upozornění se zobrazí, když (x86) 32bitové i 64bitovou (x 64) verze [.NET Core SDK](https://www.microsoft.com/net/download/all) jsou nainstalovány. Běžné důvody, které mohou být nainstalovány obě verze zahrnují:

* Původně stáhnout instalační program sady SDK .NET Core s použitím 32bitový počítač, ale potom zkopírování napříč a nainstalována na 64bitovém počítači.
* 32bitová verze .NET Core SDK byla nainstalována jiná aplikace.
* Chybná verze se stáhnul a nainstaloval.

Odinstalujte 32-bit .NET Core SDK, aby se zabránilo toto upozornění. Odinstalovat z **ovládací panely** > **programy a funkce** > **odinstalovat nebo změnit program**. Pokud budete rozumět tomu, proč dojde k upozornění a jeho dopady, můžete upozornění ignorovat.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>.NET Core SDK je nainstalována v několika umístěních

V **nový projekt** dialogové okno pro ASP.NET Core, může zobrazit následující upozornění:

> .NET Core SDK je nainstalována na více místech. Pouze šablony ze sad SDK nainstalovaných v ' C:\\Program Files\\dotnet\\sdk\\"se zobrazí.

![Snímek obrazovky dialogového okna OneASP.NET zprávou upozornění](troubleshoot/_static/multiplelocations.png)

Pokud máte alespoň jedna instalace sady .NET Core SDK do adresáře mimo se zobrazí tato zpráva *C:\\Program Files\\dotnet\\sdk\\*. K tomu obvykle dochází při .NET Core SDK je nasazený na počítači místo kopírovat/vložit instalační službu MSI.

Odinstalujte 32-bit .NET Core SDK, aby se zabránilo toto upozornění. Odinstalovat z **ovládací panely** > **programy a funkce** > **odinstalovat nebo změnit program**. Pokud budete rozumět tomu, proč dojde k upozornění a jeho dopady, můžete upozornění ignorovat.

### <a name="no-net-core-sdks-were-detected"></a>Nezjistily se žádné sady SDK .NET Core

V **nový projekt** dialogové okno pro ASP.NET Core, může zobrazit následující upozornění:

> Nezjistily se žádné sady .NET Core SDK, ujistěte se, že jsou součástí proměnné prostředí 'PATH'.

![Snímek obrazovky dialogového okna OneASP.NET zprávou upozornění](troubleshoot/_static/NoNetCore.png)

Toto upozornění se zobrazí, když je proměnná prostředí `PATH` neodkazuje na žádné .NET Core SDK na počítači. Chcete-li tento problém vyřešit:

* Nainstalujte nebo ověřte, že je nainstalovaná sada .NET Core SDK.
* Ověřte, že `PATH` proměnnou prostředí odkazuje na umístění, ve kterém je nainstalována sada SDK. Instalační program obvykle nastavuje `PATH`.