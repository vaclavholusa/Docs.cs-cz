---
title: "Jádro ASP.NET na Nano Server"
author: shirhatti
description: "Zjistěte, jak chcete využít stávající aplikace ASP.NET Core a nasaďte ho do instance Nano Server služby IIS."
manager: wpickett
ms.author: riande
ms.date: 11/04/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/nano-server
ms.openlocfilehash: 4fc5f6874f86130da9f66d13778516d984ff8b46
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="aspnet-core-with-iis-on-nano-server"></a>Jádro ASP.NET se službou IIS na Nano Server

Podle [Sourabh Shirhatti](https://twitter.com/sshirhatti)

V tomto kurzu budete trvat stávající aplikace ASP.NET Core a nasaďte ji do instance Nano Server služby IIS.

## <a name="introduction"></a>Úvod

Nano Server je možnost instalace v systému Windows Server 2016, nabízí lepší zabezpečení, malými nároky na paměť a lepší obsluhy než jádra serveru nebo celý Server. Přečtěte si oficiální [Nano Server dokumentace](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server) další podrobnosti a odkazy na stažení zkušební verze 180 dní. 

Existují tři způsoby snadno můžete vyzkoušet Nano Server. Pokud jste se přihlaste pomocí svého účtu MS:

1. Můžete stáhnout soubor systému Windows Server 2016 ISO a sestavení Nano Server bitové kopie.

2. Stáhněte si Nano Server virtuálního pevného disku.

3. Vytvoření virtuálního počítače v Azure pomocí bitové kopie Nano Server v Azure Gallery. Bezplatnou zkušební verzi Azure je dostupné.

V tomto kurzu použijeme 2. možnost předdefinovaných VHD Nano Server ze systému Windows Server 2016.

Než budete pokračovat v tomto kurzu, budete potřebovat [publikovaná výstup](xref:host-and-deploy/directory-structure) stávající aplikace ASP.NET Core. Ujistěte se, vaše aplikace je sestavena pro běh **64-bit** procesu.

## <a name="setting-up-the-nano-server-instance"></a>Nastavení instance Nano Server

[Vytvoření nového virtuálního počítače pomocí technologie Hyper-V](https://technet.microsoft.com/library/hh846766.aspx) na vývojovém počítači pomocí dříve stažené virtuální pevný disk. Tento počítač bude vyžadovat, abyste nastavili heslo správce před přihlášením. Na konzole virtuálního počítače stiskněte klávesu F11 k nastavení hesla před prvním přihlášení. Také je třeba zkontrolovat nového virtuálního počítače IP adresu buď Moje kontrola DHCP server vyřešeny IP zadaný při zřízení virtuálního počítače nebo v Nano Server konzoly pro zotavení nastavení sítě.

> [!NOTE]
> Předpokládejme, že nový virtuální počítač spustí pomocí místní adresy V4 IP 192.168.1.10.

Teď už brzo budete moct spravovat pomocí vzdálenou komunikaci prostředí PowerShell, který je jediným způsobem, jak plně spravovat Nano Server.

## <a name="connecting-to-your-nano-server-instance-using-powershell-remoting"></a>Připojení k vaší instanci Nano Server pomocí vzdálenou komunikaci prostředí PowerShell

Otevřete okno prostředí PowerShell se zvýšenými oprávněními přidat vaše vzdálené instance systému Nano Server k vaší `TrustedHosts` seznamu.

```PowerShell
$nanoServerIpAddress = "192.168.1.10"
Set-Item WSMan:\localhost\Client\TrustedHosts "$nanoServerIpAddress" -Concatenate -Force
```

> [!NOTE]
> Nahraďte proměnnou `$nanoServerIpAddress` se správnou adresou IP.

Po přidání vaší instance Nano Server k vaší `TrustedHosts`, můžete připojit se pomocí vzdálenou komunikaci prostředí PowerShell.

```PowerShell
$nanoServerSession = New-PSSession -ComputerName $nanoServerIpAddress -Credential ~\Administrator
Enter-PSSession $nanoServerSession
```

Úspěšné připojení výsledků v řádku s formátu vyhledávání jako:`[192.168.1.10]: PS C:\Users\Administrator\Documents>`

## <a name="creating-a-file-share"></a>Vytvoření sdílené složky

Vytvoření sdílené složky na serveru Nano tak, aby k publikované aplikaci lze zkopírovat na ni. Spusťte následující příkazy v relaci vzdálené plochy:

```PowerShell
New-Item C:\PublishedApps\AspNetCoreSampleForNano -type directory
netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=yes
net share AspNetCoreSampleForNano=c:\PublishedApps\AspNetCoreSampleForNano /GRANT:EVERYONE`,FULL
```

Po spuštění výše uvedených příkazů, byste měli mít přístup k této sdílené složce, navštivte stránky `\\192.168.1.10\AspNetCoreSampleForNano` v Průzkumníku Windows hostitelský počítač.

## <a name="open-port-in-the-firewall"></a>Otevřete port v bráně firewall

Spusťte následující příkazy v této relaci vzdálené otevření portu v bráně firewall chcete, aby služba IIS naslouchat komunikaci TCP na portu TCP nebo 8000.

```PowerShell
New-NetFirewallRule -Name "AspNet5 IIS" -DisplayName "Allow HTTP on TCP/8000" -Protocol TCP -LocalPort 8000 -Action Allow -Enabled True
```

## <a name="installing-iis"></a>Instalace služby IIS

Přidat `NanoServerPackage` poskytovatele z Galerie prostředí PowerShell. Jakmile zprostředkovatele je nainstalován a importovat, můžete nainstalovat balíčky systému Windows.

Spusťte následující příkazy v relaci prostředí PowerShell, který jste vytvořili:

```PowerShell
Install-PackageProvider NanoServerPackage
Import-PackageProvider NanoServerPackage
Install-NanoServerPackage -Name Microsoft-NanoServer-Storage-Package
Install-NanoServerPackage -Name Microsoft-NanoServer-IIS-Package
```

Chcete-li rychle ověřit, pokud služba IIS je nastavena správně, můžete navštívit adresu URL `http://192.168.1.10/` a měla by se zobrazit úvodní stránku. Pokud je nainstalována služba IIS, volat web `Default Web Site` naslouchá na portu 80 se vytvoří ve výchozím nastavení.

## <a name="installing-the-aspnet-core-module-ancm"></a>Instalace modulu ASP.NET Core (ANCM)

Základní modul ASP.NET je službu IIS 7.5 + modul, který je zodpovědný za správu proces naslouchacího procesu ASP.NET Core HTTP a na požadavky na proxy pro procesy, které spravuje. V tuto chvíli je ruční proces instalace technologie ASP.NET základní modul pro službu IIS. Budete muset nainstalovat [.NET jádra Windows serveru, který hostuje sady](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe) na běžný (ne Nano) počítače. Po instalaci sady regulární počítače, musíte zkopírujte následující soubory do sdílené složky, kterou jsme vytvořili předtím.

Na server regular (ne Nano) se službou IIS spusťte následující příkazy pro kopírování:

```PowerShell
Copy-Item -Path  C:\windows\system32\inetsrv\aspnetcore.dll -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
Copy-Item -Path  C:\windows\system32\inetsrv\config\schema\aspnetcore_schema.xml -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
```

Nahraďte `C:\windows\system32\inetsrv` s `C:\Program Files\IIS Express` na počítače s Windows 10.

Na straně Nano musíte zkopírujte následující soubory ze sdílené složky, kterou jsme vytvořili výše na platné umístění. Ano spusťte následující příkazy pro kopírování:

```PowerShell
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore.dll -Destination C:\windows\system32\inetsrv\
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore_schema.xml -Destination C:\windows\system32\inetsrv\config\schema\
```

Spusťte následující skript v relaci vzdálené plochy:

```PowerShell
# Backup existing applicationHost.config
Copy-Item -Path C:\Windows\System32\inetsrv\config\applicationHost.config -Destination  C:\Windows\System32\inetsrv\config\applicationHost_BeforeInstallingANCM.config

Import-Module IISAdministration

# Initialize variables
$aspNetCoreHandlerFilePath="C:\windows\system32\inetsrv\aspnetcore.dll"
Reset-IISServerManager -confirm:$false
$sm = Get-IISServerManager

# Add AppSettings section 
$sm.GetApplicationHostConfiguration().RootSectionGroup.Sections.Add("appSettings")

# Set Allow for handlers section
$appHostconfig = $sm.GetApplicationHostConfiguration()
$section = $appHostconfig.GetSection("system.webServer/handlers")
$section.OverrideMode="Allow"

# Add aspNetCore section to system.webServer
$sectionaspNetCore = $appHostConfig.RootSectionGroup.SectionGroups["system.webServer"].Sections.Add("aspNetCore")
$sectionaspNetCore.OverrideModeDefault = "Allow"
$sm.CommitChanges()

# Configure globalModule
Reset-IISServerManager -confirm:$false
$globalModules = Get-IISConfigSection "system.webServer/globalModules" | Get-IISConfigCollection
New-IISConfigCollectionElement $globalModules -ConfigAttribute @{"name"="AspNetCoreModule";"image"=$aspNetCoreHandlerFilePath}

# Configure module
$modules = Get-IISConfigSection "system.webServer/modules" | Get-IISConfigCollection
New-IISConfigCollectionElement $modules -ConfigAttribute @{"name"="AspNetCoreModule"}
```

> [!NOTE]
> Odstraňte soubory *aspnetcore.dll* a *aspnetcore_schema.xml* ze sdílené složky po předchozím kroku.

## <a name="installing-net-core-framework"></a>Instalace rozhraní .NET Framework Core

Pokud vaše aplikace je publikován jako [nasazení závislé na framework (disketové jednotky)](/dotnet/core/deploying/#framework-dependent-deployments-fdd), .NET Core musí být nainstalován na serveru. Použití [skript prostředí PowerShell dotnet install.ps1](https://dot.net/v1/dotnet-install.ps1) v relaci vzdálené prostředí PowerShell na Nano Server nainstalovat .NET Core. Verze rozhraní příkazového řádku s předat `-Version` přepínače:

```console
dotnet-install.ps1 -Version 2.0.0
```

## <a name="publishing-the-application"></a>Publikování aplikace

Zkopírujte výstup publikované aplikace do kořenové sdílené složky.

Budete muset provést změny vašeho *web.config* tak, aby odkazoval na které jste extrahovali *dotnet.exe*. Alternativně můžete přidat *dotnet.exe* pro vaši CESTU.

Příklad, jak *web.config* může vypadat, pokud *dotnet.exe* je **není** na CESTĚ:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="C:\dotnet\dotnet.exe" arguments=".\AspNetCoreSampleForNano.dll" stdoutLogEnabled="false" stdoutLogFile=".\logs\aspnetcore-stdout" forwardWindowsAuthToken="true" />
  </system.webServer>
</configuration>
```

Spusťte následující příkazy v této relaci vzdálené k vytvoření nového webu ve službě IIS pro publikovanou aplikaci na jiný port než výchozí web. Také budete muset otevřít tento port pro přístup k webu. Tento skript používá `DefaultAppPool` pro jednoduchost. Další požadavky na spuštění pod fond aplikací, najdete v části [fondy aplikací](xref:host-and-deploy/iis/index#application-pools).

```PowerShell
Import-module IISAdministration
New-IISSite -Name "AspNetCore" -PhysicalPath c:\PublishedApps\AspNetCoreSampleForNano -BindingInformation "*:8000:"
```

## <a name="known-issue-running-net-core-cli-on-nano-server-and-workaround"></a>Známé problémy s .NET Core rozhraní příkazového řádku v Nano Server a alternativní řešení
```PowerShell
New-NetFirewallRule -Name "AspNetCore Port 81 IIS" -DisplayName "Allow HTTP on TCP/81" -Protocol TCP -LocalPort 81 -Action Allow -Enabled True
```

## <a name="running-the-application"></a>Spuštění aplikace

Publikovaná webová aplikace je dostupný v prohlížeči na `http://192.168.1.10:8000`. Pokud jste si nastavili protokolování, jak je popsáno v [vytvoření a přesměrování protokolu](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection), můžete zobrazit protokoly na *C:\PublishedApps\AspNetCoreSampleForNano\logs*.
