---
title: Řešení potíží pro ASP.NET Core
author: Rick-Anderson
description: Rady pro pochopení a řešení potíží s upozornění a chyby s projekty ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 04/05/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: testing/troubleshoot
ms.openlocfilehash: a75dc666621600e1e2fe36c29acbe7484bae9229
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/18/2018
---
# <a name="troubleshoot-aspnet-core-projects"></a>Řešení potíží s projekty ASP.NET Core

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Následující odkazy obsahují pokyny k odstraňování problémů:

* [Řešení potíží s ASP.NET Core ve službě Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)
* [Řešení potíží s ASP.NET Core ve službě IIS](xref:host-and-deploy/iis/troubleshoot)
* [Běžné chyby referenční dokumentace pro Azure App Service a IIS s ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [YouTube: Diagnostika problémů s v aplikacích ASP.NET Core](https://www.youtube.com/watch?v=RYI0DHoIVaA)

<a name="sdk"></a>
## <a name="net-core-sdk-warnings"></a>Upozornění na .NET core SDK

### <a name="both-the-32-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>Obě 32 až 64 bit verze rozhraní .NET Core SDK jsou nainstalovány.
V **nový projekt** dialogové okno pro ASP.NET Core, může se zobrazit následující varování zobrazí v horní části: 

    Both 32 and 64 bit versions of the .NET Core SDK are installed. Only templates from the 64 bit version(s) installed at C:\Program Files\dotnet\sdk\" will be displayed.

![Snímek obrazovky dialogového okna OneASP.NET zobrazující upozornění](troubleshoot/_static/both32and64bit.png)

Toto upozornění se zobrazí, když (x86) 32bitovou i 64bitovou (x 64) verze [.NET Core SDK](https://www.microsoft.com/net/download/all) jsou nainstalovány. Běžné obě verze lze nainstalovat z těchto důvodů:

* Jste původně stáhnout instalační program rozhraní .NET Core SDK s použitím 32bitový počítač, ale pak zkopírovali ho napříč a nainstalován na 64bitový počítač. 
* 32bitová verze rozhraní .NET Core SDK byla nainstalována jiná aplikace.
* Nesprávné verze byla stáhli a nainstalovali.

Odinstalujte 32bitová verze rozhraní .NET Core SDK aby toto upozornění. Odinstalovat z **ovládací panely** > **programy a funkce** > **odinstalovat nebo změnit program**. Pokud budete rozumět tomu, proč dochází upozornění a jeho dopad, můžete upozornění ignorovat.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>.NET Core SDK je nainstalován na více místech
V **nový projekt** dialogové okno pro ASP.NET Core, mohou se zobrazit následující upozornění se zobrazí v horní části: 

 .NET Core SDK je nainstalována na více místech. Pouze šablony z SDK(s) nainstalovaným v "C:\Program Files\dotnet\sdk\' se zobrazí.

![Snímek obrazovky dialogového okna OneASP.NET zobrazující upozornění](troubleshoot/_static/multiplelocations.png)

Tato zpráva se zobrazuje, protože máte alespoň jedna instalace rozhraní .NET Core SDK v adresáři mimo * C:\Program Files\dotnet\sdk\*. Obvykle k tomu dojde, když .NET Core SDK nasazený na počítači použití, kopírování a vkládání namísto Instalační služby MSI.

Odinstalujte 32bitová verze rozhraní .NET Core SDK aby toto upozornění. Odinstalovat z **ovládací panely** > **programy a funkce** > **odinstalovat nebo změnit program**. Pokud budete rozumět tomu, proč dochází upozornění a jeho dopad, můžete upozornění ignorovat.
