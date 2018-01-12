---
title: "Jádro ASP.NET hostitele v systému Windows pomocí služby IIS"
author: guardrex
description: "Zjistěte, jak hostovat aplikace ASP.NET Core na Windows serveru Internetové informační služby (IIS)."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/iis/index
ms.openlocfilehash: 01cedb4e3abb35670d2908fe8cb4367c3fd58b33
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/11/2018
---
# <a name="host-aspnet-core-on-windows-with-iis"></a>Jádro ASP.NET hostitele v systému Windows pomocí služby IIS

Podle [Luke Latham](https://github.com/guardrex) a [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="supported-operating-systems"></a>Podporované operační systémy

Podporovány jsou následující operační systémy:

* Windows 7 a novější
* Windows Server 2008 R2 a novějších &#8224;

&#8224; Koncepčně, konfigurace služby IIS, které jsou popsané v tomto dokumentu platí také pro hostování aplikací ASP.NET Core na Nano Server IIS, ale odkazovat na [ASP.NET Core pomocí služby IIS na serveru Nano](xref:tutorials/nano-server) konkrétní pokyny.

[Ovladač HTTP.sys serveru](xref:fundamentals/servers/httpsys) (dříve se označovaly jako [WebListener](xref:fundamentals/servers/weblistener)) nebude fungovat v konfiguraci reverzní proxy server se službou IIS. Použití [Kestrel server](xref:fundamentals/servers/kestrel).

## <a name="iis-configuration"></a>Konfigurace služby IIS

Povolit **webového serveru (IIS)** role a vytvořit služby rolí.

### <a name="windows-desktop-operating-systems"></a>Operační systémy Windows

Přejděte na **ovládací panely** > **programy** > **programy a funkce** > **zapnout Windows funkce na nebo vypnout** (levé straně obrazovky). Otevřete skupinu, pro **Internetová informační služba** a **nástroje webové správy**. Zaškrtněte políčko pro **konzoly pro správu služby IIS**. Zaškrtněte políčko pro **webové služby**. Přijměte výchozí funkce pro **webové služby** nebo přizpůsobit funkce služby IIS.

![Konzola pro správu služby IIS a webové služby jsou vybrány v funkce systému Windows.](index/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a>Operační systémy Windows Server

Serverové operační systémy, používat **přidat role a funkce** Průvodce prostřednictvím **spravovat** nabídky nebo na odkaz v **správce serveru**. Na **role serveru** kroku, zaškrtněte políčko pro **webového serveru (IIS)**.

![Role webového serveru IIS je vybrali v kroku rolí vyberte server.](index/_static/server-roles-ws2016.png)

Na **služby rolí** krok, vyberte požadovaných služeb role služby IIS nebo přijměte výchozí nastavení role služby poskytované.

![Výchozí služby rolí jsou vybrané v kroku vyberte role služeb.](index/_static/role-services-ws2016.png)

Pokračujte **potvrzení** krok instalace role Webový server a služby. Restartování serveru nebo IIS není povinné po instalaci role webového serveru (IIS).

## <a name="install-the-net-core-windows-server-hosting-bundle"></a>Instalaci sady hostování v rozhraní .NET Core systému Windows Server

1. Nainstalujte [.NET jádra Windows serveru, který hostuje sady](https://aka.ms/dotnetcore-2-windowshosting) v hostitelském systému. Sada nainstaluje rozhraní .NET Core Runtime, základní knihovny .NET a [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module). Modul vytvoří reverzní proxy server mezi službou IIS a Kestrel server. Pokud systém nemá připojení k Internetu, získejte a nainstalujte [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) před instalací sady hostování v rozhraní .NET Core systému Windows Server.

2. Restartování systému nebo spuštění **net stop byl /y** následuje **net start w3svc** z příkazového řádku a pokračovat tam ke změně systému CESTU.

> [!NOTE]
> Informace o sdílené konfiguraci IIS najdete v tématu [ASP.NET Core modul s sdílenou konfiguraci IIS](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>Nainstaluje Web Deploy při publikování pomocí sady Visual Studio

Při nasazování aplikací na servery s [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), nainstalujte nejnovější verzi nástroje nasazení webu na serveru. Instalovat nasazení webu, použijte [instalačního programu webové platformy (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) nebo získat instalační program přímo z [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717). Upřednostňuje se má používat službu WebPI. WebPI nabízí samostatná instalace a konfigurace pro poskytovatele hostingových služeb.

## <a name="application-configuration"></a>Konfigurace aplikace

### <a name="enabling-the-iisintegration-components"></a>Povolení IISIntegration součástí

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Typické *Program.cs* volání [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) zahájíte nastavení hostitele. `CreateDefaultBuilder`nakonfiguruje [Kestrel](xref:fundamentals/servers/kestrel) jako integrace webového serveru a umožňuje služby IIS tak, že nakonfigurujete základní cesta a port pro [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module):

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Zahrnout závislost na [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) balíček v závislosti aplikaci. Integrace služby IIS middleware začlenit do aplikace tak, že přidáte *UseIISIntegration* metody rozšíření pro *WebHostBuilder*:

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

Obě `UseKestrel` a `UseIISIntegration` jsou povinné. Volání kódu `UseIISIntegration` nemá vliv přenositelnost kódu. Pokud aplikace není spuštěna za služby IIS (například spuštění aplikace přímo na Kestrel), `UseIISIntegration` ops ne.

---

Další informace o hostování najdete v tématu [hostování v ASP.NET Core](xref:fundamentals/hosting).

### <a name="iis-options"></a>Možnosti služby IIS

Chcete-li nakonfigurovat možnosti služby IIS, obsahovat konfigurace služby pro `IISOptions` v `ConfigureServices`:

```csharp
services.Configure<IISOptions>(options => 
{
    ...
});
```

| Možnost                         | Výchozí | Nastavení |
| ------------------------------ | ------- | ------- |
| `AutomaticAuthentication`      | `true`  | Pokud `true`, nastaví middleware ověřování `HttpContext.User` a reaguje na obecné problémy. Pokud `false`, middleware ověřování se poskytuje pouze identity (`HttpContext.User`) a reaguje na výzvy explicitně žádost `AuthenticationScheme`. Ověřování systému Windows musí být povolené ve službě IIS pro `AutomaticAuthentication` funkce. |
| `AuthenticationDisplayName`    | `null`  | Nastaví zobrazovaný název zobrazovat uživatelům na přihlašovací stránky. |
| `ForwardClientCertificate`     | `true`  | Pokud `true` a `MS-ASPNETCORE-CLIENTCERT` nachází hlavička požadavku `HttpContext.Connection.ClientCertificate` se naplní. |

### <a name="webconfig"></a>soubor Web.config

*Web.config* souboru primárním účelem je konfigurace [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module). Volitelně ji mohou zadat další nastavení konfigurace služby IIS. Vytváření, transformace a publikování *web.config* zpracováván pomocí .NET SDK webového jádra (`Microsoft.NET.Sdk.Web`). Sada SDK je nastavena v horní části souboru projektu `<Project Sdk="Microsoft.NET.Sdk.Web">`. Zabránit transformace sady SDK *web.config* soubor, přidejte  **\<IsTransformWebConfigDisabled >** vlastnost k souboru projektu s nastavením `true`:

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Pokud *web.config* soubor je v projektu, převede se správnou *processPath* a *argumenty* nakonfigurovat [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module) a přesunut do [publikovaná výstup](xref:host-and-deploy/directory-structure). Transformace není změnit nastavení konfigurace služby IIS v souboru.

### <a name="webconfig-location"></a>umístění souboru Web.config

Aplikace .NET core hostují přes reverzní proxy server mezi službou IIS a Kestrel server. Chcete-li vytvořit reverzní proxy server, *web.config* souboru musí být přítomné v obsahu kořenovou cestu (obvykle aplikace základní cesta) nasazené aplikace, které je fyzická cesta web zadat do služby IIS. *Web.config* soubor je vyžadován v kořenovém adresáři aplikace, aby se daly publikovat víc aplikací pomocí nástroje nasazení webu.

Citlivé soubory existují na fyzickou cestu aplikace, včetně podsložek, jako například  *\<assembly_name >. runtimeconfig.json*,  *\<assembly_name > .xml* (XML Dokumentační komentáře), a  *\<assembly_name >. deps.json*. Když *web.config* souboru je k dispozici a konfiguruje lokalitu, služba IIS zabraňuje zpracování těchto citlivé soubory. **Proto je důležité, který *web.config* soubor není ať už náhodně, přejmenovat nebo odebrat z nasazení.**

## <a name="create-the-iis-website"></a>Vytvoření webu služby IIS

1. V cílovém systému služby IIS, vytvořte složku tak, aby obsahovala aplikace publikované složek a souborů, které jsou popsány v [adresářovou strukturu](xref:host-and-deploy/directory-structure).

2. Ve složce, vytvoření *protokoly* složku pro uložení protokoly stdout, pokud je povoleno protokolování stdout. Pokud je aplikace nasazena pomocí *protokoly* složku v datové části, tento krok přeskočit. Pokyny, jak provádět MSBuild vytvořit *protokoly* složky, najdete v článku [adresářovou strukturu](xref:host-and-deploy/directory-structure) tématu.

3. V **Správce služby IIS**, vytvoření nového webu. Zadejte **název lokality** a nastavte **fyzická cesta** do složky pro nasazení aplikace. Zadejte **vazby** konfigurace a vytvořit web.

4. Nastavte fond aplikací **bez spravovaného kódu**. ASP.NET Core spouští v samostatném procesu a spravuje modulu runtime.

5. Otevřete **přidat web** okno.

   !["Přidat web, vyberte z kontextové nabídky weby.](index/_static/add-website-context-menu-ws2016.png)

6. Konfigurace webu.

   ![Zadejte název lokality, fyzickou cestu a název hostitele v kroku přidat web.](index/_static/add-website-ws2016.png)

7. V **fondy aplikací** panelech, otevřete **upravit fond aplikací** okno kliknutím pravým tlačítkem myši na fond aplikací webu a výběrem **základní nastavení...**  z místní nabídky.

   ![Vyberte základní nastavení v kontextové nabídce fondu aplikací.](index/_static/apppools-basic-settings-ws2016.png)

8. Nastavte **verze modulu .NET CLR** k **bez spravovaného kódu**.

   ![Nastavit bez spravovaného kódu pro verze .NET CLR.](index/_static/edit-apppool-ws2016.png)
     
    Poznámka: Nastavení **verze modulu .NET CLR** k **bez spravovaného kódu** je volitelný. ASP.NET Core není spoléhají na načítání plochy CLR.

9. Potvrďte, že identita procesu modelu má příslušná oprávnění.

   Pokud výchozí identitu fondu aplikací (**Model procesu** > **Identity**) se změní z **ApplicationPoolIdentity** na jinou identitu, ověřte, zda novou identitu má požadovaná oprávnění k přístupu aplikace složka, databáze a další požadované prostředky.
   
## <a name="deploy-the-app"></a>Nasazení aplikace

Nasazení aplikace na složku vytvořit v cílovém systému služby IIS. [Nasazení webové](/iis/publish/using-web-deploy/introduction-to-web-deploy) je doporučené mechanismus pro nasazení.

Potvrďte, že není spuštěn publikované aplikace pro nasazení. Soubory *publikování* složky jsou zamčené, když aplikace běží. Uzamčené soubory nelze přepsat. Chcete-li uvolnit uzamčené soubory v nasazení, zastavte fondu aplikací:

* Ručně v Správce služby IIS na serveru.
* Pomocí nástroje nasazení webu a odkazování na `Microsoft.NET.Sdk.Web` v souboru projektu. *App_offline.htm* soubor je umístěn v kořenovém adresáři webové aplikace. Když se soubor nachází, modul ASP.NET Core řádně ukončí aplikaci a slouží *app_offline.htm* souboru během nasazení. Další informace najdete v tématu [odkazu na modul jádro ASP.NET konfigurace](xref:host-and-deploy/aspnet-core-module#appofflinehtm).
* Pomocí prostředí PowerShell zastavte a restartujte fond aplikací (vyžaduje rozhraní PowerShell 5 nebo novější):
  ```PowerShell
  $webAppPoolName = 'APP_POOL_NAME'

  # Stop the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
      Stop-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Stopped
  }

  # Provide script commands here to deploy the app

  # Restart the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
      Start-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Started
  }
  ```

### <a name="web-deploy-with-visual-studio"></a>Nasazení webu pomocí sady Visual Studio

Najdete v článku [profilů publikování sady Visual Studio pro nasazování aplikací ASP.NET Core](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) tématu se dozvíte, jak vytvořit profil publikování pro použití pomocí nástroje nasazení webu. Pokud poskytovatel hostingu poskytuje profilu publikování nebo podpora pro vytváření jeden, stáhněte si svůj profil a importujte jej pomocí sady Visual Studio **publikovat** dialogové okno.

![Dialogové okno stránky publikování](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Nasazení webu mimo Visual Studio

[Nasazení webové](/iis/publish/using-web-deploy/introduction-to-web-deploy) lze také použít mimo Visual Studio z příkazového řádku. Další informace najdete v tématu [nástroj pro nasazení webu](/iis/publish/using-web-deploy/use-the-web-deployment-tool).

### <a name="alternatives-to-web-deploy"></a>Alternativy k webového nasazení

Přesunout aplikaci k hostování systému, třeba Xcopy, Robocopy nebo prostředí PowerShell můžete použijte některou z několika metod. Visual Studio uživatelé mohou využít [publikování ukázky](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).

## <a name="browse-the-website"></a>Přejděte na web

![V prohlížeči Microsoft Edge načetl úvodní stránce služby IIS.](index/_static/browsewebsite.png)

## <a name="data-protection"></a>Ochrana dat

Ochrana dat je využíván několika middlewares ASP.NET, včetně těch, které používané v ověřování. I v případě, že rozhraní Data Protection API nejsou volat z kódu uživatele, musí se nakonfigurovat ochranu dat pomocí skriptu pro nasazení nebo v uživatelském kódu pro vytvoření trvalého úložiště klíčů. Pokud není nakonfigurovaná ochrana dat, jsou klíče uložené v paměti a zahozené během restartování aplikace.

Pokud se Správce klíčů je uložené v paměti po restartování aplikace:

* Všechny forms ověřovací tokeny jsou zneplatněny. 
* Uživatelé musí znovu přihlásit na jejich další požadavek. 
* Všechna data chráněné pomocí Správce klíčů lze už dešifrovat.

Ke konfiguraci ochrany dat v rámci služby IIS, použijte **jeden** z následujících postupů:

### <a name="create-a-data-protection-registry-hive"></a>Vytvoření podregistr registru ochrany dat

Klíče ochrany dat používá aplikace ASP.NET jsou uloženy v registru podregistrů externí aplikací. Chcete-li zachovat klíče pro danou aplikaci, vytvořte podregistru pro fond aplikací.

Pro samostatné instalace služby IIS – webová farma [skript prostředí PowerShell AutoGenKeys.ps1 zřízení ochrany dat](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) lze použít pro každý fond aplikací používat s aplikací ASP.NET Core. Tento skript vytvoří speciální klíč v registru HKLM, který je ACLed pouze pro účet pracovního procesu. Klíče jsou zašifrovaná přinejmenším pomocí rozhraní DPAPI klíčem celého systému.

Ve scénářích webové farmy může být aplikace nakonfigurován pro použijte cestu UNC k ukládání klíčů ochrany jeho data. Ve výchozím nastavení nejsou klíče ochrany dat šifrována. Zkontrolujte, zda jsou omezená na účet systému Windows, které aplikace běží jako soubor oprávnění pro tyto sdílené složky. Kromě toho, X509 certifikát můžete použít k ochraně klíče v klidovém stavu. Vezměte v úvahu mechanismus, aby uživatelé mohli nahrát certifikátů: místní certifikáty do důvěryhodného certifikátu uživatele, ukládání a ujistěte se, jsou k dispozici na všech počítačích, kde je spuštěna aplikace uživatele. V tématu [konfiguraci ochrany dat](xref:security/data-protection/configuration/overview) podrobnosti.

### <a name="configure-the-iis-application-pool-to-load-the-user-profile"></a>Nakonfigurujte fond aplikací služby IIS načíst profil uživatele

Toto nastavení je ve **Model procesu** oddílu pod **Upřesnit nastavení** pro fond aplikací. Nastavit načíst profil uživatele na `True`. To ukládá klíče v adresáři profilu uživatele a chrání je pomocí rozhraní DPAPI klíčem specifické pro uživatelský účet použitý pro fond aplikací.

### <a name="use-the-file-system-as-a-key-ring-store"></a>Pomocí systému souborů jako úložiště prstenec klíč

Upravit kód aplikace, který [pomocí systému souborů jako úložiště prstenec klíč](xref:security/data-protection/configuration/overview). Použití na X509 certifikát k ochraně Správce klíčů a ujistěte se, certifikát je důvěryhodný certifikát. Pokud je certifikát podepsaný sám sebou, umístěte certifikát v úložišti Důvěryhodné kořenové.

Při použití IIS ve webové farmě:

* Použijte sdílené složky, ke kterému mají přístup všechny počítače.
* Nasazení X509 certifikát pro každý počítač. Konfigurace [ochrany dat v kódu](xref:security/data-protection/configuration/overview).

### <a name="set-a-machine-wide-policy-for-data-protection"></a>Nastavit zásady celého systému ochrany dat

Systém ochrany dat má omezenou podporu pro nastavení výchozí [celého zásad](xref:security/data-protection/configuration/machine-wide-policy) pro všechny aplikace, které využívají rozhraní Data Protection API. Najdete v článku [ochrany dat](xref:security/data-protection/index) dokumentace pro další podrobnosti.

## <a name="configuration-of-sub-applications"></a>Konfiguraci dílčí aplikací

Dílčí aplikace přidají pod kořenové aplikace by neměla zahrnovat modul ASP.NET Core jako obslužná rutina. Pokud je modul přidán jako obslužná rutina v dílčí aplikaci *web.config* souboru, 500.19 (vnitřní chyba serveru) odkazující na vadný konfigurační soubor je oznámených při pokusu o vyhledání aplikace sub. Následující příklad ukazuje obsah publikovaný *web.config* soubor pro dílčí ASP.NET Core-aplikace:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Při hostování ASP.NET Core dílčí aplikaci pod aplikace ASP.NET Core, v aplikaci dílčí explicitně odebrat zděděné obslužná rutina *web.config* souboru:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore"/>
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Další informace o konfiguraci technologie ASP.NET základní modul, najdete v článku [Úvod k modulu jádra ASP.NET](xref:fundamentals/servers/aspnet-core-module) tématu a [odkazu na modul jádro ASP.NET konfigurace](xref:host-and-deploy/aspnet-core-module).

## <a name="configuration-of-iis-with-webconfig"></a>Konfigurace služby IIS pomocí souboru web.config

Konfigurace služby IIS je ovlivněno  **\<system.webServer >** části *web.config* pro tyto funkce služby IIS, která se týkají konfigurace reverzní proxy server. Pokud je služba IIS konfigurována na úrovni systému použití dynamické komprese, toto nastavení je možné zakázat aplikace pomocí  **\<urlCompression >** element v dané aplikaci *web.config* souboru. Další informace najdete v tématu [odkaz Konfigurace pro \<system.webServer >](https://docs.microsoft.com/iis/configuration/system.webServer/), [odkazu na modul Konfigurace ASP.NET Core](xref:host-and-deploy/aspnet-core-module) a [pomocí moduly služby IIS s prostředím ASP. Základní NET](xref:host-and-deploy/iis/modules). Pokud je potřeba nastavit proměnné prostředí pro jednotlivé aplikace spuštěných ve fondech izolované aplikace (podporuje se ve službě IIS 10.0 +), najdete *příkazu AppCmd.exe* části [proměnné prostředí \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) téma v IIS referenční dokumentace.

## <a name="configuration-sections-of-webconfig"></a>Konfigurační oddíly souboru Web.config

Části Configruation aplikace rozhraní ASP.NET v *web.config* nejsou používány nástrojem aplikace ASP.NET Core pro konfiguraci:

* **\<System.Web >**
* **\<appSettings >**
* **\<connectionStrings >**
* **\<umístění >**

Aplikace ASP.NET Core jsou konfigurováni pomocí jiných poskytovatelů konfigurace. Další informace najdete v tématu [konfigurace](xref:fundamentals/configuration/index).

## <a name="application-pools"></a>Fondy aplikací

Při hostování více webů na jednom systému, izolace aplikace od sebe navzájem spuštěním každou aplikaci ve vlastním fondu aplikací. Služby IIS **přidat web** dialogovém okně výchozí hodnoty pro tuto konfiguraci. Když **název lokality** je zadaný text se automaticky přenese na **fond aplikací** textové pole. Nový fond aplikací se vytvoří při přidání webu pomocí názvu serveru.

## <a name="application-pool-identity"></a>Identita fondu aplikací

Účet identity fondu aplikací umožňuje aplikaci, běh jedinečný účet, aniž by museli vytvářet a spravovat domény nebo místní účty. Ve službě IIS 8.0 + procesů Worker Správce služby IIS (WAS) vytvoří virtuální účet s názvem nového fondu aplikací a aplikace spouští pracovní procesy fondu pod tímto účtem ve výchozím nastavení. V konzole pro správu služby IIS v části **Upřesnit nastavení** pro fond aplikací, zkontrolujte, zda **Identity** je nastavený na použití **ApplicationPoolIdentity**:

![Dialogové okno Upřesnit nastavení fondu aplikací](index/_static/apppool-identity.png)

Proces správy služby IIS vytvoří zabezpečeného identifikátoru se název fondu aplikací v zabezpečení systému Windows. Prostředky lze zabezpečit pomocí tuto identitu; Tato identita však není skutečné uživatelský účet a nezobrazí v konzole pro správu uživatelů systému Windows.

Pokud pracovní proces služby IIS vyžaduje přístup k aplikaci se zvýšeným oprávněním, upravte seznam řízení přístupu (ACL) pro adresář obsahující aplikaci:

1. Otevřete Průzkumníka Windows a přejděte do adresáře.

2. Klikněte pravým tlačítkem na adresář a vyberte **vlastnosti**.

3. V části **zabezpečení** vyberte **upravit** tlačítko a potom **přidat** tlačítko.

4. Vyberte **umístění** tlačítko a musí být vybrána systému.

5. Zadejte **IIS AppPool\DefaultAppPool** v **zadejte názvy objektů k výběru** textové pole.

   ![Vyberte uživatele nebo skupiny dialogové okno pro složce aplikace](index/_static/select-users-or-groups-1.png)

6. Vyberte **Kontrola názvů** tlačítko. Vyberte **OK**.

   ![Vyberte uživatele nebo skupiny dialogové okno pro složce aplikace](index/_static/select-users-or-groups-2.png)

To můžete provést také pomocí příkazového řádku pomocí **ICACLS** nástroje:

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="additional-resources"></a>Další zdroje

* [Řešení potíží s ASP.NET Core ve službě IIS](xref:host-and-deploy/iis/troubleshoot)
* [Běžné chyby referenční dokumentace pro Azure App Service a IIS s ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Úvod do modulu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Odkaz na konfiguraci základní modul ASP.NET](xref:host-and-deploy/aspnet-core-module)
* [Moduly služby IIS pomocí ASP.NET Core](xref:host-and-deploy/iis/modules)
* [Úvod do ASP.NET Core](../index.md)
* [Lokality oficiální Microsoft IIS](https://www.iis.net/)
* [Microsoft TechNet Library: Windows Server](https://docs.microsoft.com/windows-server/windows-server-versions)
