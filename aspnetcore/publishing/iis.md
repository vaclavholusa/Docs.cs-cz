---
title: "Jádro ASP.NET hostitele v systému Windows pomocí služby IIS"
author: guardrex
description: "Konfigurace Windows serveru Internetové informační služby (IIS) a nasazení aplikací ASP.NET Core."
keywords: "Nasazení základní technologie ASP.NET, Internetové informační služby, služby iis, windows server, hostování bundle,asp.net základní modul, webové"
ms.author: riande
manager: wpickett
ms.date: 03/13/2017
ms.topic: article
ms.assetid: a4449ad3-5bad-410c-afa7-dc32d832b552
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/iis
ms.openlocfilehash: 75fc1edec9050a4690a39d37307f2f95f5d534a5
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/28/2017
---
# <a name="host-aspnet-core-on-windows-with-iis"></a>Jádro ASP.NET hostitele v systému Windows pomocí služby IIS

Podle [Luke Latham](https://github.com/guardrex) a [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="supported-operating-systems"></a>Podporované operační systémy

Podporovány jsou následující operační systémy:

* Windows 7 a novější
* Windows Server 2008 R2 a novějších &#8224;

&#8224; Koncepčně, konfigurace služby IIS, které jsou popsané v tomto dokumentu platí také pro hostování aplikací ASP.NET Core na Nano Server IIS, ale odkazovat na [ASP.NET Core pomocí služby IIS na serveru Nano](xref:tutorials/nano-server) konkrétní pokyny.

[Ovladač HTTP.sys serveru](xref:fundamentals/servers/httpsys) (dříve se označovaly jako [WebListener](xref:fundamentals/servers/weblistener)) nebude fungovat v konfiguraci reverznímu proxy serveru se službou IIS. Je nutné použít [Kestrel server](xref:fundamentals/servers/kestrel).

## <a name="iis-configuration"></a>Konfigurace služby IIS

Povolit **webového serveru (IIS)** role a vytvořit služby rolí.

### <a name="windows-desktop-operating-systems"></a>Operační systémy Windows

Přejděte na **ovládací panely** > **programy** > **programy a funkce** > **zapnout Windows funkce na nebo vypnout** (levé straně obrazovky). Otevřete skupinu, pro **Internetová informační služba** a **nástroje webové správy**. Zaškrtněte políčko pro **konzoly pro správu služby IIS**. Zaškrtněte políčko pro **webové služby**. Přijměte výchozí funkce pro **webové služby** nebo přizpůsobit funkce služby IIS tak, aby vyhovovala vašim potřebám.

![Konzola pro správu služby IIS a webové služby jsou vybrány v funkce systému Windows.](iis/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a>Operační systémy Windows Server

Serverové operační systémy, používat **přidat role a funkce** Průvodce prostřednictvím **spravovat** nabídky nebo na odkaz v **správce serveru**. Na **role serveru** kroku, zaškrtněte políčko pro **webového serveru (IIS)**.

![Role webového serveru IIS je vybrali v kroku rolí vyberte server.](iis/_static/server-roles-ws2016.png)

Na **služby rolí** kroku, vyberte služby rolí služby IIS požadavky nebo přijměte výchozí nastavení role služeb zadaný.

![Výchozí služby rolí jsou vybrané v kroku vyberte role služeb.](iis/_static/role-services-ws2016.png)

Pokračujte **potvrzení** krok instalace role Webový server a služby. Restartování serveru nebo IIS není povinné po instalaci role webového serveru (IIS).

## <a name="install-the-net-core-windows-server-hosting-bundle"></a>Instalaci sady hostování v rozhraní .NET Core systému Windows Server

1. Nainstalujte [.NET jádra Windows serveru, který hostuje sady](https://aka.ms/dotnetcore.2.0.0-windowshosting) v hostitelském systému. Sada nainstaluje rozhraní .NET Core Runtime, základní knihovny .NET a [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module). Modul vytvoří proxy zpětného mezi službou IIS a Kestrel server. Pokud systém nemá připojení k Internetu, získejte a nainstalujte [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) před instalací sady hostování v rozhraní .NET Core systému Windows Server.

2. Restartování systému nebo spuštění **net stop byl /y** následuje **net start w3svc** z příkazového řádku a pokračovat tam ke změně systému CESTU.

> [!NOTE]
> Pokud používáte sdílenou konfiguraci IIS, najdete v části [ASP.NET Core modul s sdílenou konfiguraci IIS](xref:hosting/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>Nainstaluje Web Deploy při publikování pomocí sady Visual Studio

Pokud chcete provést nasazení aplikací s [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) v [Visual Studio](https://www.visualstudio.com/vs/), nainstalujte nejnovější verzi nástroje nasazení webu v hostitelském systému. Chcete-li nainstalovat nasazení webu, můžete použít [instalačního programu webové platformy (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) nebo získat instalační program přímo z [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717). Upřednostňuje se má používat službu WebPI. WebPI nabízí samostatná instalace a konfigurace pro poskytovatele hostingových služeb.

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

Zahrnout závislost na [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) balíček ve vaší aplikaci závislosti. Integrace služby IIS middleware začlenit do aplikace tak, že přidáte *UseIISIntegration* metody rozšíření pro *WebHostBuilder*:

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

Obě `UseKestrel` a `UseIISIntegration` jsou povinné. Volání kódu *UseIISIntegration* nemá vliv přenositelnost kódu. Pokud aplikace není spuštěna za služby IIS (například spuštění aplikace přímo na Kestrel), `UseIISIntegration` ops ne.

---

Další informace o hostování najdete v tématu [hostování v ASP.NET Core](xref:fundamentals/hosting).

### <a name="iis-options"></a>Možnosti služby IIS

Ke konfiguraci *IISIntegration* služby možnosti, patří konfigurace služby pro *IISOptions* v *ConfigureServices*:

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

*Web.config* souboru konfiguruje modul jádro ASP.NET a poskytuje další konfigurace služby IIS. Vytváření, transformace a publikování *web.config* se zpracovává souborem `Microsoft.NET.Sdk.Web`, která je součástí při nastavení vašeho projektu SDK v horní části projektu (*.csproj*) souboru `<Project Sdk="Microsoft.NET.Sdk.Web">`. Zabránit transformace nástroje MSBuild cíle vaší *web.config* soubor, přidejte  **\<IsTransformWebConfigDisabled >** vlastnost se v souboru projektu s nastavením `true`:

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Pokud nemáte *web.config* v projektu při publikování s *dotnet publikování* nebo pomocí sady Visual Studio publikovat, soubor se vytvoří pro vás v [publikovaná výstup](xref:hosting/directory-structure). Pokud máte *web.config* soubor ve vašem projektu transformována s správný *processPath* a *argumenty* nakonfigurovat [ASP.NET Core modulu ](xref:fundamentals/servers/aspnet-core-module) a přesunut do publikované výstup. Transformace není touch nastavení konfigurace služby IIS, které jste zadali v souboru.

## <a name="create-the-iis-website"></a>Vytvoření webu služby IIS

1. V cílovém systému služby IIS, vytvořte složku tak, aby obsahovala aplikace publikované složek a souborů, které jsou popsány v [adresářovou strukturu](xref:hosting/directory-structure).

2. Ve složce vytvoříte, vytvoření *protokoly* složku pro uložení protokoly stdout (Pokud budete chtít povolit protokolování pro řešení problémů spuštění). Pokud máte v úmyslu nasadit aplikaci s *protokoly* složku v datové části, může tento krok přeskočit. Došlo [otevřete problém automaticky vytvořit složku](https://github.com/aspnet/AspNetCoreModule/issues/30). Pokud chcete nástroje MSBuild vytvořit *protokolu* složky, přidejte následující `Target` se v souboru projektu:

   ```xml
   <Target Name="CreateLogsFolder" AfterTargets="AfterPublish">
     <MakeDir Directories="$(PublishDir)logs" Condition="!Exists('$(PublishDir)logs')" />
     <MakeDir Directories="$(PublishUrl)logs" Condition="!Exists('$(PublishUrl)logs')" />
   </Target>
   ```

3. V **Správce služby IIS**, vytvoření nového webu. Zadejte **název lokality** a nastavte **fyzická cesta** do složky pro nasazení aplikace, kterou jste vytvořili. Zadejte **vazby** konfigurace a vytvořit web.

4. Nastavte fond aplikací **bez spravovaného kódu**. ASP.NET Core spouští v samostatném procesu a spravuje modulu runtime.

5. Otevřete **přidat web** okno.

   ![Klikněte na tlačítko Přidat web v kontextové nabídce lokalit.](iis/_static/add-website-context-menu-ws2016.png)

6. Konfigurace webu.

   ![Zadejte název lokality, fyzickou cestu a název hostitele v kroku přidat web.](iis/_static/add-website-ws2016.png)

7. V **fondy aplikací** panelech, otevřete **upravit fond aplikací** okno kliknutím pravým tlačítkem myši na fond aplikací webu a výběrem **základní nastavení...**  z místní nabídky.

   ![Vyberte základní nastavení v kontextové nabídce fondu aplikací.](iis/_static/apppools-basic-settings-ws2016.png)

8. Nastavte **verze modulu .NET CLR** k **bez spravovaného kódu**.

   ![Nastavit bez spravovaného kódu pro verze .NET CLR.](iis/_static/edit-apppool-ws2016.png)
     
    Poznámka: Nastavení **verze modulu .NET CLR** k **bez spravovaného kódu** je volitelný. ASP.NET Core není spoléhají na načítání plochy CLR.

9. Potvrďte, že identita procesu modelu má příslušná oprávnění.

   Pokud změníte výchozí identitu fondu aplikací (**Model procesu** > **Identity**) z **ApplicationPoolIdentity** na jinou identitu, ověřte, zda novou identitu má požadovaná oprávnění k přístupu aplikace složka, databáze a další požadované prostředky.
   
## <a name="deploy-the-application"></a>Nasazení aplikace
Nasaďte aplikaci do složky, kterou jste vytvořili v cílovém systému služby IIS. [Nasazení webové](/iis/publish/using-web-deploy/introduction-to-web-deploy) je doporučené mechanismus pro nasazení. Alternativy k nasazení webu jsou uvedeny níže.

Potvrďte, že není spuštěn publikované aplikace pro nasazení. Soubory *publikování* složky jsou zamčené, když aplikace běží. Nasazení nemůže proběhnout, protože uzamčen, že soubory nelze kopírovat.

### <a name="web-deploy-with-visual-studio"></a>Nasazení webu pomocí sady Visual Studio
V tématu [vytvoření profilů publikování pro Visual Studio a nástroje MSBuild, jak nasadit aplikace ASP.NET Core](xref:publishing/web-publishing-vs#publish-profiles) tématu se dozvíte, jak vytvořit profil publikování pro použití pomocí nástroje nasazení webu. Pokud poskytovatel hostitelských poskytuje profilu publikování nebo podpora pro vytváření jeden, stáhněte si svůj profil a importujte jej pomocí sady Visual Studio **publikovat** dialogové okno.

![Dialogové okno stránky publikování](iis/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Nasazení webu mimo Visual Studio
Můžete také použít [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) mimo sadu Visual Studio pomocí příkazového řádku. Další informace najdete v tématu [nástroj pro nasazení webu](/iis/publish/using-web-deploy/use-the-web-deployment-tool).

### <a name="alternatives-to-web-deploy"></a>Alternativy k webového nasazení
Pokud jste si jej nepřejete použít, Web Deploy nebo nejsou pomocí sady Visual Studio, může používat kterýkoli z několika metod přesunout aplikaci k hostování systému, třeba Xcopy, Robocopy nebo prostředí PowerShell. Visual Studio uživatelé mohou využít [publikování ukázky](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).

## <a name="browse-the-website"></a>Přejděte na web
![V prohlížeči Microsoft Edge načetl úvodní stránce služby IIS.](iis/_static/browsewebsite.png)
   
> [!WARNING]
> Aplikace .NET core hostují přes proxy zpětného mezi službou IIS a Kestrel server. Chcete-li vytvořit zpětného proxy *web.config* souboru musí být přítomné v obsahu kořenové cestě (obvykle aplikace základní cesta) nasazené aplikace, která je fyzická cesta web zadat do služby IIS. Citlivé soubory existují na fyzickou cestu aplikace, včetně podsložek, jako například *my_application.runtimeconfig.json*, *my_application.xml* (komentáře dokumentace XML), a *my_ Application.deps.JSON*. *Web.config* soubor je vyžadován pro vytvoření reverzní proxy server na Kestrel, což zabrání obsluhující těchto a dalších citlivých souborů služby IIS. **Proto je důležité, který *web.config* soubor není ať už náhodně, přejmenovat nebo odebrat z nasazení.**

## <a name="data-protection"></a>Ochrana dat

Aplikace ASP.NET Core ukládá Správce klíčů v paměti za následujících podmínek:

* Web je hostován za služby IIS.
* Zásobník ochrany dat nebyl nakonfigurován k uložení Správce klíčů v trvalém úložišti.

Pokud se Správce klíčů je uložené v paměti po restartování aplikace:

* Všechny forms ověřovací tokeny jsou zneplatněny. 
* Uživatelé jsou požadovány k přihlášení na jejich další požadavek znovu. 
* Data, která můžete chránit pomocí Správce klíčů již nebudou chráněny.

> [!WARNING]
> Ochrana dat je využíván několika middlewares ASP.NET, včetně těch, které používané v ověřování. I v případě, že nemůžete volat rozhraní API ochrany dat z vlastního kódu, měli byste nakonfigurovat ochranu dat pomocí skriptu pro nasazení nebo v kódu. Pokud nenakonfigurujete ochranu dat, ve výchozím nastavení klíče jsou uložené v paměti a zahozené během restartování aplikace. Restartování zruší se platnost souborů cookie zapsaných správcem middleware ověřování souborů cookie a uživatelé musí přihlásit znovu.

Ke konfiguraci ochrany dat v rámci služby IIS, musíte použít jeden z následujících postupů:

* Spuštění [skript prostředí powershell](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) k vytvoření položky vhodný registru (například `.\Provision-AutoGenKeys.ps1 DefaultAppPool`). To ukládá klíče v registru, chráněné pomocí rozhraní DPAPI klíč celého systému.
* Nakonfigurujte fond aplikací služby IIS načíst profil uživatele. Toto nastavení je ve **Model procesu** oddílu pod **Upřesnit nastavení** pro fond aplikací. Nastavit **načíst profil uživatele** k `True`. To ukládá klíče v adresáři profilu uživatele a chrání je pomocí rozhraní DPAPI klíčem specifické pro uživatelský účet použitý pro fond aplikací.
* Upravit kód aplikace a [pomocí systému souborů jako úložiště prstenec klíč](xref:security/data-protection/configuration/overview). Použití na X509 certifikát k ochraně Správce klíčů a zda je certifikát důvěryhodný. Pokud je certifikát podepsaný sám sebou, je nutné umístit do úložiště důvěryhodných kořenových.

Při použití IIS ve webové farmě:

* Použijte sdílené složky, ke kterému mají přístup všechny počítače.
* Nasazení X509 certifikát pro každý počítač. Konfigurace [ochrany dat v kódu](https://docs.microsoft.com/aspnet/core/security/data-protection/configuration/overview).

### <a name="1-create-a-data-protection-registry-hive"></a>1. Vytvoření podregistr registru ochrany dat

Používané aplikacemi ASP.NET klíče ochrany dat jsou uloženy v registru podregistrů externí k aplikacím. Udržení klíče pro danou aplikaci, musíte vytvořit podregistru pro fond aplikací aplikace.

Pro samostatné instalace služby IIS, můžete použít [skript prostředí PowerShell AutoGenKeys.ps1 zřízení ochrany dat](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) pro každý fond aplikací používat s aplikací ASP.NET Core. Tento skript vytvoří speciální klíč v registru HKLM, který je ACLed pouze pro účet pracovního procesu. Klíče jsou zašifrovaná přinejmenším pomocí rozhraní DPAPI.

Ve scénářích webové farmy může být aplikace nakonfigurován pro použijte cestu UNC k ukládání klíčů ochrany jeho data. Ve výchozím nastavení nejsou klíče ochrany dat šifrována. Měli byste zajistit, že jsou omezeny na účet systému Windows, které aplikace běží jako soubor oprávnění pro tyto sdílené složky. Kromě toho můžete k ochraně klíče přinejmenším pomocí X509 certifikátu. Pravděpodobně budete chtít zvážit mechanismus, aby uživatelé mohli nahrát certifikátů: místní certifikáty do důvěryhodného certifikátu uživatele, ukládání a ujistěte se, jsou k dispozici na všech počítačích, kde je spuštěna aplikace uživatele. V tématu [konfiguraci ochrany dat](xref:security/data-protection/configuration/overview#data-protection-configuring) podrobnosti.

### <a name="2-configure-the-iis-application-pool-to-load-the-user-profile"></a>2. Nakonfigurujte fond aplikací služby IIS načíst profil uživatele

Toto nastavení je ve **Model procesu** oddílu pod **Upřesnit nastavení** pro fond aplikací. Nastavit načíst profil uživatele na `True`. To ukládá klíče v adresáři profilu uživatele a chrání je pomocí rozhraní DPAPI klíčem specifické pro uživatelský účet použitý pro fond aplikací.

### <a name="3-machine-wide-policy-for-data-protection"></a>3. Celého systému zásady ochrany dat

Systém ochrany dat má omezenou podporu pro nastavení výchozí [celého zásad](xref:security/data-protection/configuration/machine-wide-policy#data-protection-configuration-machinewidepolicy) pro všechny aplikace, které využívají rozhraní Data Protection API. Najdete v článku [ochrany dat](xref:security/data-protection/index) dokumentace pro další podrobnosti.

## <a name="configuration-of-sub-applications"></a>Konfiguraci dílčí aplikací

Sub – aplikace, které přidají pod kořenová aplikace by neměla zahrnovat modul ASP.NET Core jako obslužná rutina. Když přidáte modul jako obslužná rutina dílčí-aplikace *web.config* souboru, obdržíte 500.19 (vnitřní chyba serveru), při pokusu o procházet aplikace dílčí odkazující na vadný konfiguračním souboru. Následující příklad ukazuje obsah publikovaný *web.config* soubor pro dílčí ASP.NET Core-aplikace:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Pokud máte v úmyslu hostitele ASP.NET Core dílčí aplikaci pod aplikace ASP.NET Core, je třeba explicitně odebrat zděděné obslužné rutiny v aplikaci dílčí *web.config* souboru:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore"/>
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Další informace o konfiguraci modulu ASP.NET Core s *web.config* souborů najdete v tématu [Úvod k modulu jádra ASP.NET](xref:fundamentals/servers/aspnet-core-module) tématu a [konfigurace modulu jádro ASP.NET referenční dokumentace](xref:hosting/aspnet-core-module).

## <a name="configuration-of-iis-with-webconfig"></a>Konfigurace služby IIS pomocí souboru web.config

Konfigurace služby IIS je stále vliv `<system.webServer>` části *web.config* pro tyto funkce služby IIS, která se týkají konfigurace reverzní proxy server. Například můžete mít IIS konfigurována na úrovni systému použití dynamické komprese, ale je možné vypnout tohoto nastavení pro aplikace pomocí `<urlCompression>` element v dané aplikaci *web.config* souboru. Další informace najdete v tématu [odkaz Konfigurace pro `<system.webServer>` ](https://docs.microsoft.com/iis/configuration/system.webServer/), [odkazu na modul Konfigurace ASP.NET Core](xref:hosting/aspnet-core-module) a [pomocí modulů IIS s ASP.NET Core](xref:hosting/iis-modules). Pokud je nutné nastavit proměnné prostředí pro jednotlivé aplikace běžící v izolované fondy aplikací (podporuje se ve službě IIS 10.0 +), najdete *příkazu AppCmd.exe* části [proměnné prostředí \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) téma v IIS referenční dokumentace.

## <a name="configuration-sections-of-webconfig"></a>Konfigurační oddíly souboru Web.config

Na rozdíl od aplikací rozhraní .NET Framework, které jsou nakonfigurovány s `<system.web>`, `<appSettings>`, `<connectionStrings>`, a `<location>` elementů v *web.config*, aplikace ASP.NET Core jsou nakonfigurovány pomocí jiné konfigurace zprostředkovatelé. Další informace najdete v tématu [konfigurace](xref:fundamentals/configuration).

## <a name="application-pools"></a>Fondy aplikací

Při hostování více webů na jednom systému, by měl izolace aplikace od sebe navzájem spuštěním každou aplikaci ve vlastním fondu aplikací. Služby IIS **přidat web** dialogovém okně výchozí nastavení je toto chování. Když zadáte **název lokality**, textu je automaticky převeden do **fond aplikací** textové pole. Nový fond aplikací je vytvořený pomocí názvu serveru, když přidáte na web.

## <a name="application-pool-identity"></a>Identita fondu aplikací

Účet identity fondu aplikací umožňuje spustit aplikaci v části jedinečný účet bez nutnosti vytvářet a spravovat domény nebo místní účty. Ve službě IIS 8.0 + procesů Worker Správce služby IIS (WAS) vytvoří virtuální účet s názvem nového fondu aplikací a aplikace spouští pracovní procesy fondu pod tímto účtem ve výchozím nastavení. V konzole pro správu služby IIS v části **Upřesnit nastavení** pro fond aplikací, zkontrolujte, zda **Identity** je nastavený na použití **ApplicationPoolIdentity** jak je znázorněno na obrázku níže.

![Dialogové okno Upřesnit nastavení fondu aplikací](iis/_static/apppool-identity.png)

Proces správy služby IIS vytvoří zabezpečeného identifikátoru se název fondu aplikací v zabezpečení systému Windows. Prostředky lze zabezpečit pomocí tuto identitu; Tato identita však není skutečné uživatelský účet a nezobrazí v konzole pro správu uživatelů systému Windows.

Pokud potřebujete udělit IIS pracovní proces se zvýšenými oprávněními přístup do vaší aplikace, musíte upravit seznam řízení přístupu (ACL) pro adresář obsahující aplikaci.

1. Otevřete Průzkumníka Windows a přejděte do adresáře.

2. Klikněte pravým tlačítkem na adresář a klikněte na tlačítko **vlastnosti**.

3. V části **zabezpečení** , klikněte na **upravit** tlačítko a potom **přidat** tlačítko.

4. Klikněte **umístění** tlačítko a zkontrolujte, zda jste vybrali systému.

5. Zadejte **IIS AppPool\DefaultAppPool** v **zadejte názvy objektů k výběru** textové pole.

  ![Vyberte uživatele nebo skupiny dialogové okno pro složce aplikace](iis/_static/select-users-or-groups-1.png)

6. Klikněte **Kontrola názvů** tlačítko a pak klikněte na **OK**.

  ![Vyberte uživatele nebo skupiny dialogové okno pro složce aplikace](iis/_static/select-users-or-groups-2.png)

Můžete to provést taky pomocí příkazového řádku pomocí **ICACLS** nástroje:

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="troubleshooting-tips"></a>Tipy k řešení potíží

K diagnostikování problémů s nasazením služby IIS:

* Studie výstup prohlížeče.
* Zkontrolovat, zda systém **aplikace** protokolu prostřednictvím **Prohlížeč událostí**.
* Povolit `stdout` protokolování. **ASP.NET Core modulu** protokolu nachází na v zadané cestě *stdoutLogFile* atribut `<aspNetCore>` element v *web.config*. Všechny složky v cestě uvedené v hodnotě atributu musí existovat v nasazení. Musíte taky nastavit *stdoutLogEnabled = "true"*. Aplikace, které používají `Microsoft.NET.Sdk.Web` SDK k vytvoření *web.config* souboru výchozí *stdoutLogEnabled* nastavení *false*, takže musíte ručně zadat *web.config* souboru nebo upravte soubor. Chcete-li povolit `stdout` protokolování.

Několik běžných chyb, dokud modul nezobrazí v prohlížeči, protokolu aplikace a ASP.NET Core modulu protokolu *startupTimeLimit* (výchozí: 120 sekundách) a *startupRetryCount* (výchozí: 2) byly splněny. Proto počkejte úplné šest minut před deducing které modulu se nepodařilo spustit proces pro aplikaci.

A spusťte aplikaci přímo na Kestrel je jeden rychlý způsob, jak zjistit, zda aplikace funguje správně. Pokud aplikace byl publikován jako závislé na framework nasazení (disketové jednotky), spouštění `dotnet my_application.dll` ve složce nasazení, což je fyzická cesta k aplikaci služby IIS. Pokud byla aplikace publikována jako samostatná nasazení (SCD), spusťte aplikaci je spustitelný soubor přímo z příkazového řádku, `my_application.exe`, ve složce nasazení. Pokud Kestrel naslouchá na výchozí port 5000, byste měli moct přejít aplikaci ve `http://localhost:5000/`. Pokud aplikace odpovídá za normálních okolností na adrese Kestrel koncový bod, problém se týká víc pravděpodobně ke konfiguraci služby IIS – ASP.NET Core modulu-Kestrel a méně pravděpodobně v rámci aplikace.

Jeden způsob jak určit, jestli služby IIS reverzní proxy server a Kestrel server správně funguje je provést žádost o jednoduché statických souborů pro šablony stylů, skript nebo bitovou kopii z aplikace statické soubory v *wwwroot* pomocí [statických souborů Middleware](xref:fundamentals/static-files). Pokud aplikace může obsluhovat statické soubory, ale dochází k selhání zobrazení MVC a dalších koncových bodů, je ale problém menší pravděpodobně související ke konfiguraci služby IIS – ASP.NET Core modulu-Kestrel a více pravděpodobně v rámci aplikace (například směrování MVC nebo 500 – Vnitřní chyba serveru).

Pokud Kestrel spustí běžným způsobem za služby IIS, ale aplikace se nespustí v systému po úspěšném spuštění místně, můžete dočasně přidat proměnnou prostředí pro *web.config* nastavit `ASPNETCORE_ENVIRONMENT` k `Development`. Tak dlouho, dokud není přepsat prostředí v aplikaci při spuštění, to umožňuje [vývojáře výjimka stránky](xref:fundamentals/error-handling) zobrazí při spuštění aplikace v systému. Nastavení proměnné prostředí pro `ASPNETCORE_ENVIRONMENT` tímto způsobem doporučujeme pouze pro testování nebo pracovní systémy, které nejsou vystaveny k Internetu. Ujistěte se, odeberete z proměnné prostředí *web.config* souboru po dokončení. Informace o nastavení proměnné prostředí prostřednictvím *web.config* reverzní proxy server, najdete v části [environmentVariables podřízený element aspNetCore](xref:hosting/aspnet-core-module#setting-environment-variables).

Ve většině případů povolení protokolování aplikací pomáhá při řešení potíží s aplikací nebo reverzní proxy server. V tématu [protokolování](xref:fundamentals/logging) Další informace.

Naše poslední tip pro odstraňování potíží se vztahují k aplikacím, které se nepodařilo spustit po upgradu buď .NET Core SDK na vývoj pro počítače nebo balíček verze v aplikaci. V některých případech je možné osamocené balíčky ukončit aplikaci při provádění hlavních aktualizací. Většinu tyto problémy můžete vyřešit odstraněním `bin` a `obj` balíček složek v projektu, vymazání mezipaměti na `%UserProfile%\.nuget\packages\` a `%LocalAppData%\Nuget\v3-cache`, obnovení projektu a potvrzení, že bylo předchozí nasazení v systému úplně odstranit před novým nasazením aplikace.

> [!TIP]
> Vhodný způsob k vymazání mezipamětí balíčku je použít k získání *NuGet.exe* nástroje z [NuGet.org](https://www.nuget.org/), přidejte ho k vašemu systému CESTU a spouštět `nuget locals all -clear` z příkazového řádku. Můžete také provést `dotnet nuget locals all --clear` příkazu z příkazového řádku bez získání *NuGet.exe*.

## <a name="common-errors"></a>Běžné chyby

Následující není úplný seznam chyb. Podrobná chybová zpráva se vyskytne chyba zde nejsou uvedeny, nechejte prosím komentáře níže v části.

### <a name="installer-unable-to-obtain-vc-redistributable"></a>Instalační program nemohl získat VC ++ Redistributable

* **Instalační program výjimka:** 0x80072efd nebo 0x80072f76 - Nespecifikovaná chyba

* **Instalační program protokolu výjimka &#8224;:** Chyba 0x80072efd nebo 0x80072f76: spuštění EXE balíčku se nezdařilo

  &#8224; V protokolu je umístěn v C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.

Řešení potíží:

* Pokud systém nemá přístup k Internetu při instalaci serveru pro hostování sady, tato výjimka nastane, když se instalační program nemůže získat *Microsoft Visual C++ 2015 Redistributable*. Můžete získat z instalačního programu [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840). Pokud instalační program selže, nemusíte dostávat na .NET Core runtime potřebná pro Hosting nasazení závislé na framework (chyba). Pokud máte v plánu k hostování disketové jednotky, zkontrolujte, zda je modul runtime nainstalovány v programech &amp; funkce. Můžete získat za běhu instalačního programu z [.NET stáhne](https://www.microsoft.com/net/download/core). Po instalaci modulu runtime, restartování systému nebo restartujte službu IIS tak, že spustíte **net stop byl /y** následuje **net start w3svc** z příkazového řádku.

### <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>Upgrade operačního systému odebrat modul 32-bit ASP.NET Core

* **Protokolu aplikace:** knihovnu DLL modulu **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** se nepodařilo načíst. Data je chyba.

Řešení potíží:

* Soubory s non-OS **C:\Windows\SysWOW64\inetsrv** directory nezachovají se během operační systém upgradu. Pokud modul ASP.NET Core nainstalovaný před upgrade operačního systému a potom zkuste spustit všechny fondu aplikací v režimu 32-bit po upgradu operačního systému, narazíte na potíže. Po upgradu operačního systému opravte modul ASP.NET Core. V tématu [instalaci sady .NET jádra Windows serveru, který hostuje](#install-the-net-core-windows-server-hosting-bundle). Vyberte **Repair** při spuštění Instalační služby.

### <a name="platform-conflicts-with-rid"></a>Platformy je v konfliktu s identifikátorů RID

* **Prohlížeč:** chyb HTTP 502.5 - selhání procesu

* **Protokolu aplikace:** aplikaci ' Počítač/WEBROOT/APPHOST/MY_APPLICATION' s kořenem fyzické "C:\{cesta}\' Nepodařilo se spustit proces s commandline '" C:\\\my_application {umístění cesta}. { exe | dll} "", kód chyby = ' 0x80004005: ff.

* **ASP.NET Core modulu protokolu:** neošetřená výjimka: System.BadImageFormatException: Nepodařilo se načíst soubor nebo sestavení 'my_application.dll'. Byl proveden pokus o načtení program v nesprávném formátu.

Řešení potíží:

* Potvrďte, že je aplikace spuštěná místně na Kestrel. Selhání procesu může být výsledkem problému v aplikaci. Další informace najdete v tématu [tipy pro odstraňování potíží](#troubleshooting-tips).

* Potvrďte, zda nebyla nastavena `<PlatformTarget>` ve vaší *.csproj* , je v konfliktu s identifikátor RID. Například nezadávejte `<PlatformTarget>` z `x86` a publikovat s identifikátorů RID z `win10-x64`, buď pomocí *dotnet publikování - c - r verzi win10-x64* nebo nastavením `<RuntimeIdentifiers>` ve vaší *.csproj*  k `win10-x64`. Projekt publikuje bez upozornění nebo chyby, ale selže s výše zaznamenané výjimky v systému.

* Pokud tuto výjimku dojde k nasazení aplikace Azure při upgradu aplikace a nasazení novější sestavení, odstraňte ručně všechny soubory z předchozí nasazení. Přetrvávání odstraněných nekompatibilní sestavení může mít za následek `System.BadImageFormatException` výjimka při nasazení aplikace upgradovaný.

### <a name="uri-endpoint-wrong-or-stopped-website"></a>Identifikátor URI koncového bodu nesprávné nebo se zastavil webu

* **Prohlížeč:** ERR_CONNECTION_REFUSED

* **Protokolu aplikace:** žádná položka

* **ASP.NET Core modulu protokolu:** vytvoření souboru protokolu

Řešení potíží:

* Potvrďte, že používáte správný identifikátor URI koncového bodu pro aplikaci. Zkontrolujte vaše vazby.

* Potvrďte, že na web služby IIS není v *Zastaveno* stavu.

### <a name="corewebengine-or-w3svc-server-features-disabled"></a>Funkce serveru CoreWebEngine nebo W3SVC zakázána

* **Výjimka operačního systému:** CoreWebEngine služby IIS 7.0 a W3SVC funkce musí být nainstalována na použít modul ASP.NET Core.

Řešení potíží:

* Potvrďte, že je povoleno správné role a funkce. V tématu [konfigurace služby IIS](#iis-configuration).

### <a name="incorrect-website-physical-path-or-application-missing"></a>Fyzická cesta nesprávný webu nebo aplikace chybí

* **Prohlížeč:** 403 Zakázáno – přístup byl odepřen. **--nebo--** 403.14 zakázáno – webový server je nakonfigurován tak, aby nezobrazovat obsah tohoto adresáře.

* **Protokolu aplikace:** žádná položka

* **ASP.NET Core modulu protokolu:** vytvoření souboru protokolu

Řešení potíží:

* Zkontrolujte na web služby IIS **základní nastavení** a složce fyzické aplikace. Potvrďte, že aplikace je ve složce na webu služby IIS **fyzická cesta**.

### <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a>Nesprávný role, modul není nainstalován nebo nesprávná oprávnění

* **Prohlížeč:** 500.19 vnitřní chyba serveru: K požadované stránce nelze přistupovat, protože související konfigurační data pro stránku nejsou platná.

* **Protokolu aplikace:** žádná položka

* **ASP.NET Core modulu protokolu:** vytvoření souboru protokolu

Řešení potíží:

* Potvrďte, že jste povolili správné roli. V tématu [konfigurace služby IIS](#iis-configuration).

* Zkontrolujte **programy &amp; funkce** a ověřte, že **Microsoft ASP.NET Core modulu** byl nainstalován. Pokud **Microsoft ASP.NET Core modulu** není uvedena v seznamu nainstalovaných programů, nainstalujte modul. V tématu [instalaci sady .NET jádra Windows serveru, který hostuje](#install-the-net-core-windows-server-hosting-bundle).

* Ujistěte se, že **fond aplikací** > **Model procesu** > **Identity** je nastaven na **ApplicationPoolIdentity** nebo vlastní identitu má správná oprávnění pro přístup do složky pro nasazení aplikace.

### <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>Nesprávný processPath chybějící proměnné PATH, hostování sady není nainstalován, není restartování systému nebo služby IIS, VC ++ Redistributable není nainstalovaný nebo dotnet.exe narušení přístupu

* **Prohlížeč:** chyb HTTP 502.5 - selhání procesu

* **Protokolu aplikace:** aplikaci ' Počítač/WEBROOT/APPHOST/MY_APPLICATION' s kořenem fyzické "C:\\{umístění cesta}\' Nepodařilo se spustit proces s commandline"".\my_application.exe" ' kód chyby = ' 0x80070002: 0.

* **ASP.NET Core modulu protokolu:** vytvořit soubor protokolu, ale prázdný

Řešení potíží:

* Potvrďte, že je aplikace spuštěná místně na Kestrel. Selhání procesu může být výsledkem problému v aplikaci. Další informace najdete v tématu [tipy pro odstraňování potíží](#troubleshooting-tips).

* Zkontrolujte *processPath* atributu u `<aspNetCore>` element v *web.config* potvrďte, že je *dotnet* pro nasazení závislé na framework (disketové jednotky) nebo *.\my_application.exe* samostatná nasazení (SCD).

* Pro disketové jednotky *dotnet.exe* nemusí být přístupné přes nastavení cesty. Potvrďte, že * C:\Program Files\dotnet\* existuje v systémové CESTĚ nastavení.

* Pro disketové jednotky *dotnet.exe* nemusí být dostupná pro identitu uživatele fondu aplikací. Zkontrolujte, jestli uživatel identita fondu aplikací má přístup k *C:\Program Files\dotnet* adresáře. Potvrďte, že neexistují žádná pravidla odepření nakonfigurované pro identitu fondu aplikací uživatele na *C:\Program Files\dotnet* a adresáře aplikace.

* Můžete mít nasazené disketové jednotky a nainstalovat .NET Core bez restartování služby IIS. Restartujte server, nebo restartujte službu IIS tak, že spustíte **net stop byl /y** následuje **net start w3svc** z příkazového řádku.

* Může mít nasazenou disketové jednotky bez instalace na .NET Core runtime v hostitelském systému. Pokud se pokus o nasazení disketové jednotky a nenainstalovali na .NET Core runtime, spusťte **hostování v rozhraní .NET Core systému Windows Server instalační program sady** v systému. V tématu [instalaci sady .NET jádra Windows serveru, který hostuje](#install-the-net-core-windows-server-hosting-bundle). Pokud se pokoušíte nainstalovat na .NET Core runtime v systému bez připojení k Internetu, získat modul runtime z [.NET stáhne](https://www.microsoft.com/net/download/core) a spusťte instalační program hostování sady nainstalovat modul ASP.NET Core. Dokončete instalaci restartováním systému nebo restartování služby IIS tak, že spustíte **net stop byl /y** následuje **net start w3svc** z příkazového řádku.

* Můžete mít nasazené disketové jednotky a bez restartování systému služby IIS nainstalovat .NET Core. Restartování systému nebo restartujte službu IIS tak, že spustíte **net stop byl /y** následuje **net start w3svc** z příkazového řádku.

* Jste nasadili disketové jednotky a *Microsoft Visual C++ 2015 Redistributable (x64)* není nainstalovaná v systému. Můžete získat z instalačního programu [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).

### <a name="incorrect-arguments-of-aspnetcore-element"></a>Nesprávný argumenty \<aspNetCore\> – element

* **Prohlížeč:** chyb HTTP 502.5 - selhání procesu

* **Protokolu aplikace:** aplikaci ' Počítač/WEBROOT/APPHOST/MY_APPLICATION' s kořenem fyzické "C:\\{umístění cesta}\' Nepodařilo se spustit proces s commandline"dotnet".\my_application.dll, kód chyby = ' 0x80004005: 80008081.

* **ASP.NET Core modulu protokolu:** aplikace provést neexistuje: "PATH\my_application.dll.

Řešení potíží:

* Potvrďte, že je aplikace spuštěná místně na Kestrel. Selhání procesu může být výsledkem problému v aplikaci. Další informace najdete v tématu [tipy pro odstraňování potíží](#troubleshooting-tips).

* Zkontrolujte *argumenty* atributu u `<aspNetCore>` element v *web.config* potvrďte, že je buď (a) *.\my_application.dll* pro závislém framework nasazení (disketové jednotky); nebo (b) není k dispozici, prázdný řetězec (*argumenty = ""*), nebo seznam argumentů vaší aplikace (*argumenty = "arg1 arg2,..."*) pro samostatný nasazení (SCD).

### <a name="missing-net-framework-version"></a>Chybějící verze rozhraní .NET Framework

* **Prohlížeč:** 502.3 Chybná brána - došlo k chybě připojení při pokusu o pro směrování požadavku.

* **Protokolu aplikace:** kód chyby = ' MACHINE/WEBROOT/APPHOST/MY_APPLICATION' aplikace s kořenem fyzické "C:\\{umístění cesta}\' Nepodařilo se spustit proces s commandline"dotnet".\my_application.dll, kód chyby = ' 0x80004005: 80008081.

* **ASP.NET Core modulu protokolu:** chybí výjimka metoda, soubor nebo sestavení. Metoda, soubor nebo sestavení. výjimkou je metoda rozhraní .NET Framework, soubor nebo sestavení.

Řešení potíží:

* Nainstalujte verzi rozhraní .NET Framework chybí ze systému.

* Pro nasazení závislé na framework (disketové jednotky) potvrďte, že správné runtime v systému nainstalována. Pokud upgradujete z verze 1.1 projektu na 2.0, nasazení do hostujícího systému a přijímat výjimku, ujistěte se, že nainstalujte rozhraní framework 2.0 v hostitelském systému.

### <a name="stopped-application-pool"></a>Zastavený fond aplikací

* **Prohlížeč:** 503 Služba nedostupná

* **Protokolu aplikace:** žádná položka

* **ASP.NET Core modulu protokolu:** vytvoření souboru protokolu

Poradce při potížích

* Potvrďte, že není ve fondu aplikací *Zastaveno* stavu.

### <a name="iis-integration-middleware-not-implemented"></a>Middleware integrační služby IIS není implementováno

* **Prohlížeč:** chyb HTTP 502.5 - selhání procesu

* **Protokolu aplikace:** aplikaci ' Počítač/WEBROOT/APPHOST/MY_APPLICATION' s kořenem fyzické "C:\\{umístění cesta}\' vytvořené proces pomocí příkazového řádku se" C:\\\my_application {umístění cesta}. { exe | dll} ", ale buď došlo k chybě nebo nebyla odezvy nebo není naslouchat na zadaný port '{PORT}', kód chyby = '0x800705b4.

* **ASP.NET Core modulu protokolu:** vytvořit soubor protokolu a zobrazuje normálního provozu.

Poradce při potížích

* Potvrďte, že aplikace běží místně na Kestrel. Selhání procesu může být výsledkem problému v rámci aplikace. Další informace najdete v tématu [tipy pro odstraňování potíží](#troubleshooting-tips).

* Potvrďte, že jste správně odkazuje middleware integrační služby IIS voláním *. UseIISIntegration()* metodu aplikace *WebHostBuilder()* (ASP.NET Core 1.x) nebo používat `CreateDefaultBuilder` – metoda (ASP.NET Core 2.x). V tématu [hostování v ASP.NET Core](xref:fundamentals/hosting) podrobnosti.

### <a name="sub-application-includes-a-handlers-section"></a>Obsahuje dílčí aplikace \<obslužné rutiny\> části

* **Prohlížeč:** Chyba protokolu HTTP 500.19 – vnitřní chyba serveru

* **Protokolu aplikace:** žádná položka

* **ASP.NET Core modulu protokolu:** vytvořit soubor protokolu a zobrazuje normálního provozu pro kořenové aplikace. Soubor protokolu nebyl vytvořen pro dílčí aplikace.

Poradce při potížích

* Potvrďte, že dílčí aplikace *web.config* soubor neobsahuje `<handlers>` části.

### <a name="application-configuration-general-issue"></a>Obecné problém konfigurace aplikace

* **Prohlížeč:** chyb HTTP 502.5 - selhání procesu

* **Protokolu aplikace:** aplikaci ' Počítač/WEBROOT/APPHOST/MY_APPLICATION' s kořenem fyzické "C:\\{umístění cesta}\' vytvořené proces pomocí příkazového řádku se" C:\\\my_application {umístění cesta}. { exe | dll} ", ale buď došlo k chybě nebo nebyla odezvy nebo není naslouchat na zadaný port '{PORT}', kód chyby = '0x800705b4.

* **ASP.NET Core modulu protokolu:** vytvořit soubor protokolu, ale prázdný

Poradce při potížích

* Tato obecná výjimka označuje, že proces se nepodařilo spustit, pravděpodobně z důvodu chyby konfigurace aplikace. Odkazy na [adresářovou strukturu](xref:hosting/directory-structure), potvrďte, že vaše aplikace nasazená soubory a složky, které jsou vhodné, a zda jsou přítomné soubory konfigurace vaší aplikace a obsahovat správná nastavení pro aplikace a prostředí. Další informace najdete v tématu [tipy pro odstraňování potíží](#troubleshooting-tips).

## <a name="resources"></a>Prostředky

* [Úvod do modulu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Odkaz na konfiguraci základní modul ASP.NET](xref:hosting/aspnet-core-module)
* [Moduly služby IIS pomocí ASP.NET Core](xref:hosting/iis-modules)
* [Úvod do ASP.NET Core](../index.md)
* [Lokality oficiální Microsoft IIS](https://www.iis.net/)
* [Microsoft TechNet Library: Windows Server](https://docs.microsoft.com/windows-server/windows-server-versions)
