---
title: "Nahraďte machineKey technologie ASP.NET"
author: rick-anderson
description: "Zjistit, jak nahradit machineKey technologie ASP.NET umožňuje použití ochrany systému nová a bezpečnější data."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/compatibility/replacing-machinekey
ms.openlocfilehash: 8113780dd7b8b3010cf77d14abb45730a2f28959
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="replace-machinekey-in-aspnet"></a>Nahraďte machineKey technologie ASP.NET

<a name="compatibility-replacing-machinekey"></a>

Implementace `<machineKey>` element technologie ASP.NET [je replaceable](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/). To umožňuje většinu volání kryptografické rutiny ASP.NET prostřednictvím ochranný mechanismus nahrazení data, včetně nového systému ochrany dat směrování.

## <a name="package-installation"></a>Instalace balíčku

> [!NOTE]
> Nový systém ochrany dat pouze lze nainstalovat do stávající aplikaci ASP.NET cílení na rozhraní .NET 4.5.1 nebo vyšší. Instalace se nezdaří, pokud aplikace cílí .NET 4.5 nebo nižší.

Instalace nového systému ochrany dat do existujícího projektu ASP.NET 4.5.1+, nainstalujte balíček Microsoft.AspNetCore.DataProtection.SystemWeb. To vytvoří instanci systému ochrany dat pomocí [výchozí konfigurace](xref:security/data-protection/configuration/default-settings) nastavení.

Při instalaci balíčku se vloží řádek do *Web.config* použít pro technologii ASP.NET, která oznamuje [nejvíce kryptografické operace](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), včetně ověřování pomocí formulářů, zobrazení stavu a volání MachineKey.Protect. Řádek, který je vložen přečte následujícím způsobem.

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> Můžete zjistit, pokud je nový systém ochrany dat active zkontrolováním pole jako `__VIEWSTATE`, který by měl začínat obráceným "CfDJ8" jako v příkladu níže. "CfDJ8" je base64 reprezentace záhlaví magic "09 F0 C9 F0", který identifikuje chráněného systémem ochrany dat datové části.

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk..." />
```

## <a name="package-configuration"></a>Konfigurace balíčku

Vytvoření instance systému ochrany dat s výchozí konfigurací nastavení nula. Ale vzhledem k tomu, že ve výchozím nastavení jsou trvalé klíče do místního systému souborů, to nebude fungovat pro aplikace, které jsou nasazené ve farmě. To vyřešit, můžete zadat konfiguraci vytvořením typu které podtřídy DataProtectionStartup a přepíše jeho ConfigureServices metoda.

Dole je příklad typ spuštění ochrany vlastních dat, který nakonfigurovaný, kde jsou klíče nastavené jako trvalé i jak jste zašifrovaná přinejmenším. Potlačí výchozí zásady izolace aplikací také tím, že poskytuje svůj vlastní název aplikace.

```csharp
using System;
using System.IO;
using Microsoft.AspNetCore.DataProtection;
using Microsoft.AspNetCore.DataProtection.SystemWeb;
using Microsoft.Extensions.DependencyInjection;

namespace DataProtectionDemo
{
    public class MyDataProtectionStartup : DataProtectionStartup
    {
        public override void ConfigureServices(IServiceCollection services)
        {
            services.AddDataProtection()
                .SetApplicationName("my-app")
                .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\myapp-keys\"))
                .ProtectKeysWithCertificate("thumbprint");
        }
    }
}
```

>[!TIP]
> Můžete také použít `<machineKey applicationName="my-app" ... />` místo explicitní volání SetApplicationName. Jedná se o usnadnění práce mechanismus, aby se zabránilo vynucení vývojáře pro vytvoření typu odvozeného DataProtectionStartup, pokud se všechny chtěli nakonfigurovat nastavení název aplikace.

Pokud chcete povolit tento vlastní konfigurace, přejděte zpět do souboru Web.config a vyhledejte `<appSettings>` element, který nainstalujte balíček přidat do konfiguračního souboru. Ho bude vypadat jako následující kód:

```xml
<appSettings>
  <!--
  If you want to customize the behavior of the ASP.NET Core Data Protection stack, set the
  "aspnet:dataProtectionStartupType" switch below to be the fully-qualified name of a
  type which subclasses Microsoft.AspNetCore.DataProtection.SystemWeb.DataProtectionStartup.
  -->
  <add key="aspnet:dataProtectionStartupType" value="" />
</appSettings>
```

Zadejte prázdnou hodnotu s názvem kvalifikovaný sestavení DataProtectionStartup odvozeného typu, který jste právě vytvořili. Pokud název aplikace je DataProtectionDemo, to bude vypadat níže.

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

Nově nakonfigurovaný data ochrany systému je nyní připravena k použití uvnitř aplikace.
