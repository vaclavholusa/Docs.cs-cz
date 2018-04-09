---
title: Běžné chyby referenční dokumentace pro Azure App Service a IIS s ASP.NET Core
author: guardrex
description: Rozlišení běžných chyb při hostování aplikace ASP.NET Core v Azure aplikace služby a služby IIS.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: fb833ef8797ea7851cbaf53bb5681df248d07a49
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a>Běžné chyby referenční dokumentace pro Azure App Service a IIS s ASP.NET Core

Podle [Luke Latham](https://github.com/guardrex)

Následující není úplný seznam chyb. Pokud dojde k chybě, zde nejsou uvedeny [otevřete nový problém](https://github.com/aspnet/Docs/issues/new) s podrobné pokyny k tomu chybu reprodukovat.

Shromážděte následující informace:

* Chování prohlížeče
* Položky protokolu událostí aplikace
* Položky protokolu stdout modulu jádro ASP.NET

Porovnávání informací o následující běžné chyby. Pokud je nalezena shoda, postupujte podle pomoc při řešení potíží.

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a>Instalační program nemohl získat VC ++ Redistributable

* **Instalační program výjimka:** 0x80072efd nebo 0x80072f76 - Nespecifikovaná chyba

* **Instalační program protokolu výjimka&#8224;:** Chyba 0x80072efd nebo 0x80072f76: spuštění EXE balíčku se nezdařilo

  &#8224;V protokolu je umístěn v C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.

Řešení potíží:

* Pokud systém nemá přístup k Internetu při instalaci serveru pro hostování sady, tato výjimka nastane, když se instalační program nemůže získat *Microsoft Visual C++ 2015 Redistributable*. Získání Instalační program z [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840). Pokud instalační program selže, nemusí server dostat na .NET Core runtime potřebná pro Hosting nasazení závislé na framework (chyba). Pokud hostování disketové jednotky, zkontrolujte, zda je modul runtime nainstalovány v programech &amp; funkce. V případě potřeby získat za běhu instalačního programu z [.NET všechny soubory ke stažení](https://www.microsoft.com/net/download/all). Po instalaci modulu runtime, restartování systému nebo restartujte službu IIS tak, že spustíte **net stop byl /y** následuje **net start w3svc** z příkazového řádku.

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>Upgrade operačního systému odebrat modul 32-bit ASP.NET Core

* **Protokolu aplikace:** knihovnu DLL modulu **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** se nepodařilo načíst. Data je chyba.

Řešení potíží:

* Soubory s non-OS **C:\Windows\SysWOW64\inetsrv** directory nezachovají se během operační systém upgradu. Pokud je nainstalován modul ASP.NET Core starších než upgrade operačního systému a potom všechny fondu aplikací je spuštěn v režimu 32-bit po upgradu operačního systému, zjistil se tento problém. Po upgradu operačního systému opravte modul ASP.NET Core. V tématu [instalaci sady .NET jádra Windows serveru, který hostuje](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle). Vyberte **Repair** spuštění Instalační služby.

## <a name="platform-conflicts-with-rid"></a>Platformy je v konfliktu s identifikátorů RID

* **Prohlížeč:** chyb HTTP 502.5 - selhání procesu

* **Protokolu aplikace:** aplikace se MACHINE/WEBROOT/APPHOST / {sestavení} se s kořenem fyzické ' C:\{cesta}\' Nepodařilo se spustit proces s příkazového řádku ' "C:\\umístění {cesta} {sestavení}. { exe | dll} "", kód chyby = ' 0x80004005: ff.

* **ASP.NET Core modulu protokolu:** neošetřená výjimka: System.BadImageFormatException: Nepodařilo se načíst soubor nebo sestavení "{sestavení} .dll". Byl proveden pokus o načtení program v nesprávném formátu.

Řešení potíží:

* Potvrďte, že aplikace běží místně na Kestrel. Selhání procesu může být výsledkem problému v rámci aplikace. Další informace najdete v tématu [Poradce při potížích s](xref:host-and-deploy/iis/troubleshoot).

* Potvrďte, že `<PlatformTarget>` v *.csproj* není v konfliktu s identifikátor RID. Například nezadávejte `<PlatformTarget>` z `x86` a publikovat s RID `win10-x64`, buď pomocí *dotnet publikování - c - r verzi win10-x64* nebo nastavením `<RuntimeIdentifiers>` v *.csproj*  k `win10-x64`. Projekt publikuje bez upozornění nebo chyby, ale selže s výše zaznamenané výjimky v systému.

* Pokud tuto výjimku dojde k nasazení aplikace Azure při upgradu aplikace a nasazení novější sestavení, odstraňte ručně všechny soubory z předchozí nasazení. Přetrvávání odstraněných nekompatibilní sestavení může mít za následek `System.BadImageFormatException` výjimka při nasazení aplikace upgradovaný.

## <a name="uri-endpoint-wrong-or-stopped-website"></a>Identifikátor URI koncového bodu nesprávné nebo se zastavil webu

* **Prohlížeč:** ERR_CONNECTION_REFUSED

* **Protokolu aplikace:** žádná položka

* **ASP.NET Core modulu protokolu:** vytvoření souboru protokolu

Řešení potíží:

* Potvrďte, že se používá koncový bod správný identifikátor URI pro aplikaci. Zkontrolujte vazby.

* Potvrďte, že na web služby IIS není v *Zastaveno* stavu.

## <a name="corewebengine-or-w3svc-server-features-disabled"></a>Funkce serveru CoreWebEngine nebo W3SVC zakázána

* **Výjimka operačního systému:** CoreWebEngine služby IIS 7.0 a W3SVC funkce musí být nainstalována na použít modul ASP.NET Core.

Řešení potíží:

* Potvrďte, že jsou povoleny správné role a funkce. V tématu [konfigurace služby IIS](xref:host-and-deploy/iis/index#iis-configuration).

## <a name="incorrect-website-physical-path-or-app-missing"></a>Fyzická cesta nesprávný web nebo aplikaci chybí

* **Prohlížeč:** 403 Zakázáno – přístup byl odepřen. **--nebo--** 403.14 zakázáno – webový server je nakonfigurován tak, aby nezobrazovat obsah tohoto adresáře.

* **Protokolu aplikace:** žádná položka

* **ASP.NET Core modulu protokolu:** vytvoření souboru protokolu

Řešení potíží:

* Zkontrolujte na web služby IIS **základní nastavení** a složce fyzické aplikace. Potvrďte, že je aplikace ve složce na webu služby IIS **fyzická cesta**.

## <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a>Nesprávný role, modul není nainstalován nebo nesprávná oprávnění

* **Prohlížeč:** 500.19 vnitřní chyba serveru: K požadované stránce nelze přistupovat, protože související konfigurační data pro stránku nejsou platná.

* **Protokolu aplikace:** žádná položka

* **ASP.NET Core modulu protokolu:** vytvoření souboru protokolu

Řešení potíží:

* Zkontrolujte, zda je povoleno správné roli. V tématu [konfigurace služby IIS](xref:host-and-deploy/iis/index#iis-configuration).

* Zkontrolujte **programy &amp; funkce** a ověřte, že **Microsoft ASP.NET Core modulu** byl nainstalován. Pokud **Microsoft ASP.NET Core modulu** není uvedena v seznamu nainstalovaných programů, nainstalujte modul. V tématu [instalaci sady .NET jádra Windows serveru, který hostuje](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).

* Ujistěte se, že **fond aplikací** > **Model procesu** > **Identity** je nastaven na **ApplicationPoolIdentity** nebo vlastní identity má správná oprávnění pro přístup do složky pro nasazení aplikace.

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>Nesprávný processPath chybějící proměnné PATH, hostování sady není nainstalován, není restartování systému nebo služby IIS, VC ++ Redistributable není nainstalovaný nebo dotnet.exe narušení přístupu

* **Prohlížeč:** chyb HTTP 502.5 - selhání procesu

* **Protokolu aplikace:** aplikace ' MACHINE/WEBROOT/APPHOST / {sestavení} se s kořenem fyzické ' C:\\{umístění cesta}\' Nepodařilo se spustit proces s příkazového řádku. ".\{ sestavení} .exe"', kód chyby = ' 0x80070002: 0.

* **ASP.NET Core modulu protokolu:** vytvořit soubor protokolu, ale prázdný

Řešení potíží:

* Potvrďte, že aplikace běží místně na Kestrel. Selhání procesu může být výsledkem problému v rámci aplikace. Další informace najdete v tématu [Poradce při potížích s](xref:host-and-deploy/iis/troubleshoot).

* Zkontrolujte *processPath* atributu u `<aspNetCore>` element v *web.config* potvrďte, že je *dotnet* pro nasazení závislé na framework (disketové jednotky) nebo *. \{sestavení} .exe* samostatná nasazení (SCD).

* Pro disketové jednotky *dotnet.exe* nemusí být přístupné přes nastavení cesty. Potvrďte, že * C:\Program Files\dotnet\* existuje v systémové CESTĚ nastavení.

* Pro disketové jednotky *dotnet.exe* nemusí být dostupná pro identitu uživatele fondu aplikací. Zkontrolujte, jestli uživatel identita fondu aplikací má přístup k *C:\Program Files\dotnet* adresáře. Potvrďte, že neexistují žádná pravidla odepření nakonfigurované pro identitu fondu aplikací uživatele na *C:\Program Files\dotnet* a adresáře aplikace.

* Disketové jednotky nasazení a nainstalovat .NET Core bez restartování služby IIS. Restartujte server, nebo restartujte službu IIS tak, že spustíte **net stop byl /y** následuje **net start w3svc** z příkazového řádku.

* Disketové jednotky může mít nasazenou bez instalace na .NET Core runtime v hostitelském systému. Pokud na .NET Core runtime nebyla nainstalována, spusťte **hostování v rozhraní .NET Core systému Windows Server instalační program sady** v systému. V tématu [instalaci sady .NET jádra Windows serveru, který hostuje](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle). Pokud došlo k pokusu o instalaci na .NET Core runtime v systému bez připojení k Internetu, získat modul runtime z [.NET všechny soubory ke stažení](https://www.microsoft.com/net/download/all) a spusťte instalační program hostování sady nainstalovat modul ASP.NET Core. Dokončete instalaci restartováním systému nebo restartování služby IIS tak, že spustíte **net stop byl /y** následuje **net start w3svc** z příkazového řádku.

* Byla nasazena disketové jednotky a *Microsoft Visual C++ 2015 Redistributable (x64)* není nainstalovaná v systému. Získání Instalační program z [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).

## <a name="incorrect-arguments-of-aspnetcore-element"></a>Nesprávný argumenty \<aspNetCore\> – element

* **Prohlížeč:** chyb HTTP 502.5 - selhání procesu

* **Protokolu aplikace:** aplikace se MACHINE/WEBROOT/APPHOST / {sestavení} se s kořenem fyzické ' C:\\{umístění cesta}\' Nepodařilo se spustit proces s příkazového řádku ' "dotnet".\{ .dll sestavení}', kód chyby = ' 0x80004005: 80008081.

* **ASP.NET Core modulu protokolu:** aplikace provést neexistuje: "cesta\{sestavení} .dll.

Řešení potíží:

* Potvrďte, že aplikace běží místně na Kestrel. Selhání procesu může být výsledkem problému v rámci aplikace. Další informace najdete v tématu [Poradce při potížích s](xref:host-and-deploy/iis/troubleshoot).

* Zkontrolujte *argumenty* atributu u `<aspNetCore>` element v *web.config* potvrďte, že je buď (a) *.\{ sestavení} .dll* pro nasazení závislé na framework (disketové jednotky); nebo (b) není k dispozici, prázdný řetězec (*argumenty = ""*), nebo seznam argumentů aplikace (*argumenty = "arg1 arg2,..."*) samostatná nasazení (SCD).

## <a name="missing-net-framework-version"></a>Chybějící verze rozhraní .NET Framework

* **Prohlížeč:** 502.3 Chybná brána - došlo k chybě připojení při pokusu o pro směrování požadavku.

* **Protokolu aplikace:** kód chyby = aplikace ' MACHINE/WEBROOT/APPHOST / {sestavení} se s kořenem fyzické ' C:\\{umístění cesta}\' Nepodařilo se spustit proces s příkazového řádku ' "dotnet".\{ .dll sestavení}', kód chyby = ' 0x80004005: 80008081.

* **ASP.NET Core modulu protokolu:** chybí výjimka metoda, soubor nebo sestavení. Metoda, soubor nebo sestavení. výjimkou je metoda rozhraní .NET Framework, soubor nebo sestavení.

Řešení potíží:

* Nainstalujte verzi rozhraní .NET Framework chybí ze systému.

* Pro nasazení závislé na framework (disketové jednotky) zkontrolujte, že správné runtime nainstalovaná v systému. Pokud projekt je upgradovat z verze 1.1 na 2.0, které jsou nasazené v systému hostitele a výsledky výjimku, ujistěte se, že rozhraní framework 2.0 je v hostitelském systému.

## <a name="stopped-application-pool"></a>Zastavený fond aplikací

* **Prohlížeč:** 503 Služba nedostupná

* **Protokolu aplikace:** žádná položka

* **ASP.NET Core modulu protokolu:** vytvoření souboru protokolu

Poradce při potížích

* Potvrďte, že fond aplikací není v *Zastaveno* stavu.

## <a name="iis-integration-middleware-not-implemented"></a>Middleware integrační služby IIS není implementováno

* **Prohlížeč:** chyb HTTP 502.5 - selhání procesu

* **Protokolu aplikace:** aplikace se MACHINE/WEBROOT/APPHOST / {sestavení} se s kořenem fyzické ' C:\\{umístění cesta}\' vytvořené proces pomocí příkazového řádku ' "C:\\{umístění cesta}\{sestavení}. { exe | dll} ", ale buď došlo k chybě nebo nebyla odezvy nebo není naslouchat na zadaný port '{PORT}', kód chyby = '0x800705b4.

* **ASP.NET Core modulu protokolu:** vytvořit soubor protokolu a zobrazuje normálního provozu.

Poradce při potížích

* Potvrďte, že aplikace běží místně na Kestrel. Selhání procesu může být výsledkem problému v rámci aplikace. Další informace najdete v tématu [Poradce při potížích s](xref:host-and-deploy/iis/troubleshoot).

* Potvrďte, že buď:
  * Middleware integrační služby IIS je referencedby volání `UseIISIntegration` metodu aplikace `WebHostBuilder` (ASP.NET Core 1.x)
  * Aplikace používá `CreateDefaultBuilder` – metoda (ASP.NET Core 2.x).
  
  V tématu [hostování v ASP.NET Core](xref:fundamentals/hosting) podrobnosti.

## <a name="sub-application-includes-a-handlers-section"></a>Obsahuje dílčí aplikace \<obslužné rutiny\> části

* **Prohlížeč:** Chyba protokolu HTTP 500.19 – vnitřní chyba serveru

* **Protokolu aplikace:** žádná položka

* **ASP.NET Core modulu protokolu:** soubor protokolu vytvořen a zobrazuje normálního provozu pro kořenovou aplikaci. Soubor protokolu nebyl vytvořen pro aplikace sub.

Poradce při potížích

* Potvrďte, že dílčí aplikace *web.config* soubor neobsahuje `<handlers>` části.

## <a name="stdout-log-path-incorrect"></a>Cesta k protokolu STDOUT nesprávný

* **Prohlížeč:** aplikace reaguje normálně.

* **Protokolu aplikace:** upozornění: Nepodařilo se vytvořit stdoutLogFile \\? \C:\_apps\app_folder\bin\Release\netcoreapp2.0\win10-x64\publish\logs\path_doesnt_exist\stdout_8748_201831835937.log, kód chyby = - 2147024893.

* **ASP.NET Core modulu protokolu:** vytvoření souboru protokolu

Poradce při potížích

* `stdoutLogFile` Cesta zadaná v `<aspNetCore>` element *web.config* neexistuje. Další informace najdete v tématu [vytvoření a přesměrování protokolu](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) tématu referenční konfigurace modulu jádra ASP.NET.

## <a name="application-configuration-general-issue"></a>Obecné problém konfigurace aplikace

* **Prohlížeč:** chyb HTTP 502.5 - selhání procesu

* **Protokolu aplikace:** aplikace se MACHINE/WEBROOT/APPHOST / {sestavení} se s kořenem fyzické ' C:\\{umístění cesta}\' vytvořené proces pomocí příkazového řádku ' "C:\\{umístění cesta}\{sestavení}. { exe | dll} ", ale buď došlo k chybě nebo nebyla odezvy nebo není naslouchat na zadaný port '{PORT}', kód chyby = '0x800705b4.

* **ASP.NET Core modulu protokolu:** vytvořit soubor protokolu, ale prázdný

Poradce při potížích

* Tato obecná výjimka označuje, že proces se nepodařilo spustit, pravděpodobně z důvodu chyby konfigurace aplikace. Odkazy na [adresářovou strukturu](xref:host-and-deploy/directory-structure), potvrďte, že aplikace nasazená soubory a složky, které jsou vhodné, a zda jsou k dispozici konfigurační soubory aplikace a obsahovat správná nastavení pro aplikaci a prostředí. Další informace najdete v tématu [Poradce při potížích s](xref:host-and-deploy/iis/troubleshoot).
