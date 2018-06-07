---
title: Jádro ASP.NET hostitele v systému Windows pomocí služby IIS
author: guardrex
description: Zjistěte, jak hostovat aplikace ASP.NET Core na Windows serveru Internetové informační služby (IIS).
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/index
ms.openlocfilehash: 0cb9bc7d8bf415e5a0125c3798f2430c9e861c98
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729649"
---
# <a name="host-aspnet-core-on-windows-with-iis"></a>Jádro ASP.NET hostitele v systému Windows pomocí služby IIS

Podle [Luke Latham](https://github.com/guardrex) a [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="supported-operating-systems"></a>Podporované operační systémy

Podporovány jsou následující operační systémy:

* Windows 7 nebo novější
* Windows Server 2008 R2 nebo novější

[Ovladač HTTP.sys serveru](xref:fundamentals/servers/httpsys) (dříve se označovaly jako [WebListener](xref:fundamentals/servers/weblistener)) nefunguje v konfiguraci reverzní proxy server se službou IIS. Použití [Kestrel server](xref:fundamentals/servers/kestrel).

## <a name="application-configuration"></a>Konfigurace aplikace

### <a name="enable-the-iisintegration-components"></a>Povolit IISIntegration součásti

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Typické *Program.cs* volání [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) zahájíte nastavení hostitele. `CreateDefaultBuilder` nakonfiguruje [Kestrel](xref:fundamentals/servers/kestrel) jako integrace webového serveru a umožňuje služby IIS tak, že nakonfigurujete základní cesta a port pro [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module):

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

Základní modul ASP.NET generuje dynamický port přiřadit pro proces back-end. `CreateDefaultBuilder` volání [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) metoda, která převezme dynamický port a nakonfiguruje Kestrel tak, aby naslouchala na `http://localhost:{dynamicPort}/`. Přepíše ostatní konfigurace adresy URL, například volání `UseUrls` nebo [API naslouchat na Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration). Proto volání `UseUrls` nebo na Kestrel `Listen` při použití modulu nejsou požadované rozhraní API. Pokud `UseUrls` nebo `Listen` nazývá Kestrel sleduje na port zadaný při spuštění aplikace bez služby IIS.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Zahrnout závislost na [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) balíček v závislosti aplikaci. Pomocí integrace služby IIS middleware přidáním [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) rozšíření metodu [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder):

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

Obě [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) a [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) jsou povinné. Volání kódu `UseIISIntegration` nemá vliv přenositelnost kódu. Pokud aplikace není spuštěna za služby IIS (například spuštění aplikace přímo na Kestrel), `UseIISIntegration` nepracuje.

Základní modul ASP.NET generuje dynamický port přiřadit pro proces back-end. `UseIISIntegration` Metoda převezme dynamický port a nakonfiguruje Kestrel tak, aby naslouchala na `http://locahost:{dynamicPort}/`. Přepíše ostatní konfigurace adresy URL, například volání `UseUrls`. Proto volání `UseUrls` není povinné, pokud používáte modul. Pokud `UseUrls` nazývá Kestrel sleduje na port zadaný při spuštění aplikace bez služby IIS.

Pokud `UseUrls` je volána v aplikaci ASP.NET Core 1.0, volání **před** volání `UseIISIntegration` tak, aby modul Konfigurovat port není přepsán. Toto pořadí volání není povinné technologii ASP.NET Core 1.1, protože modul nastavení přepsání `UseUrls`.

---

Další informace o hostování najdete v tématu [hostitele v ASP.NET Core](xref:fundamentals/host/index).

### <a name="iis-options"></a>Možnosti služby IIS

Chcete-li nakonfigurovat možnosti služby IIS, obsahovat konfigurace služby pro [IISOptions](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) v [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices). V následujícím příkladu předávání klientské certifikáty k aplikaci pro naplnění `HttpContext.Connection.ClientCertificate` je zakázaná:

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| Možnost                         | Výchozí | Nastavení |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | Pokud `true`, nastaví Middleware integrační služby IIS `HttpContext.User` ověřována [ověřování systému Windows](xref:security/authentication/windowsauth). Pokud `false`, middleware poskytuje pouze identity `HttpContext.User` a reaguje na výzvy explicitně žádost `AuthenticationScheme`. Ověřování systému Windows musí být povolené ve službě IIS pro `AutomaticAuthentication` funkce. Další informace najdete v tématu [ověřování systému Windows](xref:security/authentication/windowsauth) tématu. |
| `AuthenticationDisplayName`    | `null`  | Nastaví zobrazovaný název zobrazovat uživatelům na přihlašovací stránky. |
| `ForwardClientCertificate`     | `true`  | Pokud `true` a `MS-ASPNETCORE-CLIENTCERT` nachází hlavička požadavku `HttpContext.Connection.ClientCertificate` se naplní. |

### <a name="proxy-server-and-load-balancer-scenarios"></a>Proxy server a scénáře pro vyrovnávání zatížení

Middlewaru integrační služby IIS, který konfiguruje předávaných Middleware hlavičky a základní modul ASP.NET se tak, aby předával schématu (HTTP či HTTPS) a vzdálené IP adrese, kde tato žádost pochází. Další konfigurace může být potřeba pro aplikace, které jsou hostovány za další proxy servery a nástroje pro vyrovnávání zatížení. Další informace najdete v tématu [konfigurace ASP.NET Core k práci s proxy servery a nástroje pro vyrovnávání zatížení](xref:host-and-deploy/proxy-load-balancer).

### <a name="webconfig-file"></a>soubor Web.config

*Web.config* nakonfiguruje souboru [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module). Vytváření, transformace a publikování *web.config* souboru se zpracovává souborem cíl MSBuild (`_TransformWebConfig`) při publikování projektu. Tento cíl je součástí sady SDK webové cíle (`Microsoft.NET.Sdk.Web`). Sada SDK je nastavena v horní části souboru projektu:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

Pokud *web.config* souboru se nacházejí v projektu, se vytvoří soubor s správný *processPath* a *argumenty* nakonfigurovat [ASP.NET Core Modul](xref:fundamentals/servers/aspnet-core-module) a přesunut do [publikovaná výstup](xref:host-and-deploy/directory-structure).

Pokud *web.config* souboru se nacházejí v projektu, převede soubor s správný *processPath* a *argumenty* konfigurace modulu jádra ASP.NET a přesunut do publikované výstup. Transformace není změnit nastavení konfigurace služby IIS v souboru.

*Web.config* soubor může poskytovat další nastavení konfigurace služby IIS, která řídí aktivní moduly služby IIS. Informace o moduly služby IIS, které podporují zpracování požadavků s aplikacemi ASP.NET Core, najdete v článku [moduly služby IIS](xref:host-and-deploy/iis/modules) tématu.

Abyste zabránili transformace sady SDK webové *web.config* soubor, použijte  **\<IsTransformWebConfigDisabled >** vlastnost v souboru projektu:

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Při zakazování sady SDK webové z transformaci souboru *processPath* a *argumenty* musí ručně nastavit sám vývojář. Další informace najdete v tématu [odkazu na modul jádro ASP.NET konfigurace](xref:host-and-deploy/aspnet-core-module).

### <a name="webconfig-file-location"></a>umístění souboru Web.config

Chcete-li vytvořit reverzní proxy server mezi službou IIS a serveru Kestrel *web.config* souboru musí být přítomné v obsahu kořenovou cestu (obvykle aplikace základní cesta) nasazené aplikace. Jde o stejné umístění jako fyzická cesta k webu poskytované služby IIS. *Web.config* soubor je vyžadován v kořenovém adresáři aplikace, aby se daly publikovat víc aplikací pomocí nástroje nasazení webu.

Citlivé soubory existují na fyzickou cestu aplikace, například  *\<sestavení >. runtimeconfig.json*,  *\<sestavení > .xml* (komentáře dokumentace XML) a  *\<sestavení >. deps.json*. Když *web.config* je soubor k dispozici a a lokality spustí běžným způsobem, služba IIS nebude poskytovat tyto důvěrné soubory, pokud jste požadovali. Pokud *web.config* soubor nebyl nalezen, nesprávně s názvem, nebo nebylo možné konfigurace lokality k normálnímu spuštění, služba IIS může poskytovat citlivé soubory veřejně.

***Web.config* soubor musí být přítomen v nasazení za všech okolností, správně s názvem a možnost konfigurace lokality pro normální spuštění nahoru. Nikdy odebrat *web.config* soubor z produkčního nasazení.**

## <a name="iis-configuration"></a>Konfigurace služby IIS

**Operační systémy Windows Server**

Povolit **webového serveru (IIS)** role serveru a vytvořit služby rolí.

1. Použití **přidat role a funkce** průvodce z **spravovat** nabídky nebo na odkaz v **správce serveru**. Na **role serveru** kroku, zaškrtněte políčko pro **webového serveru (IIS)**.

   ![Role webového serveru IIS je vybrali v kroku rolí vyberte server.](index/_static/server-roles-ws2016.png)

1. Po **funkce** kroku **služby rolí** krok načte pro webový Server (IIS). Vyberte požadovaných služeb role služby IIS nebo přijmout že výchozí role služeb zadaný.

   ![Výchozí služby rolí jsou vybrané v kroku vyberte role služeb.](index/_static/role-services-ws2016.png)

   **Ověřování systému Windows (volitelné)**  
   Chcete-li povolit ověřování systému Windows, rozbalte následující uzly: **Webový Server** > **zabezpečení**. Vyberte **ověřování systému Windows** funkce. Další informace najdete v tématu [ověřování systému Windows \<windowsAuthentication >](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) a [ověřování systému Windows nakonfigurovat](xref:security/authentication/windowsauth).

   **Technologie WebSockets (volitelné)**  
   Technologie WebSockets je podporována s ASP.NET Core 1.1 nebo novější. Chcete-li povolit Websocket, rozbalte následující uzly: **Webový Server** > **vývoj aplikací**. Vyberte **protokol WebSocket** funkce. Další informace najdete v tématu [Websocket](xref:fundamentals/websockets).

1. Pokračujte **potvrzení** krok instalace role Webový server a služby. Restartování serveru nebo IIS není povinné po instalaci **webového serveru (IIS)** role.

**Operační systémy Windows**

Povolit **konzoly pro správu služby IIS** a **webové služby**.

1. Přejděte na **ovládací panely** > **programy** > **programy a funkce** > **zapnout Windows funkce na nebo vypnout** (levé straně obrazovky).

1. Otevřete **Internetová informační služba** uzlu. Otevřete **nástroje webové správy** uzlu.

1. Zaškrtněte políčko pro **konzoly pro správu služby IIS**.

1. Zaškrtněte políčko pro **webové služby**.

1. Přijměte výchozí funkce pro **webové služby** nebo přizpůsobit funkce služby IIS.

   **Ověřování systému Windows (volitelné)**  
   Chcete-li povolit ověřování systému Windows, rozbalte následující uzly: **webové služby** > **zabezpečení**. Vyberte **ověřování systému Windows** funkce. Další informace najdete v tématu [ověřování systému Windows \<windowsAuthentication >](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) a [ověřování systému Windows nakonfigurovat](xref:security/authentication/windowsauth).

   **Technologie WebSockets (volitelné)**  
   Technologie WebSockets je podporována s ASP.NET Core 1.1 nebo novější. Chcete-li povolit Websocket, rozbalte následující uzly: **webové služby** > **funkce pro vývoj aplikací**. Vyberte **protokol WebSocket** funkce. Další informace najdete v tématu [Websocket](xref:fundamentals/websockets).

1. Pokud instalace služby IIS vyžaduje restartování, restartování systému.

![Konzola pro správu služby IIS a webové služby jsou vybrány v funkce systému Windows.](index/_static/windows-features-win10.png)

---

## <a name="install-the-net-core-hosting-bundle"></a>Nainstalujte rozhraní .NET základní hostování sady

1. Nainstalujte *sady hostování rozhraní .NET Core* v hostitelském systému. Sada nainstaluje rozhraní .NET Core Runtime, základní knihovny .NET a [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module). Modul vytvoří reverzní proxy server mezi službou IIS a Kestrel server. Pokud systém nemá připojení k Internetu, získejte a nainstalujte [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) před instalací sady hostování rozhraní .NET Core.

   1. Přejděte na [.NET všechny soubory ke stažení stránky](https://www.microsoft.com/net/download/all).
   1. V **Runtime** sloupci tabulky, vyberte ze seznamu na nejnovější .NET Core runtime bez preview (**stáhne X.Y Runtime (vX.Y.Z)**). Obsahuje nejnovější modul runtime **aktuální** popisek. Pokud máte v úmyslu pracovat s software verze preview, brání v jeho text odkazu modulu runtime slovem "náhled" nebo "rc" (Release Candidate).
   1. Na .NET Core runtime stránce v části stažení **Windows**, vyberte **hostování instalační program sady** odkaz ke stažení *sady hostování rozhraní .NET Core* Instalační služby.
   1. Spusťte instalační program na serveru.

   **Důležité!** Pokud je sada hostování nainstalovaná před službou IIS, je nutné opravit instalaci sady. Spusťte instalační program sady hostování znovu po instalaci služby IIS.
   
   Instalační program zabránit instalaci x86 balíčků na x64 OS, spusťte instalační program z příkazového řádku s přepínačem správce `OPT_NO_X86=1`.

1. Restartování systému nebo spuštění **net stop byl /y** následuje **net start w3svc** z příkazového řádku. Restartování služby IIS převezme ke změně systému cesta provedené Instalační služby.

> [!NOTE]
> Informace o sdílené konfiguraci IIS najdete v tématu [ASP.NET Core modul s sdílenou konfiguraci IIS](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>Nainstaluje Web Deploy při publikování pomocí sady Visual Studio

Při nasazování aplikací na servery s [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), nainstalujte nejnovější verzi nástroje nasazení webu na serveru. Instalovat nasazení webu, použijte [instalačního programu webové platformy (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) nebo získat instalační program přímo z [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717). Upřednostňuje se má používat službu WebPI. WebPI nabízí samostatná instalace a konfigurace pro poskytovatele hostingových služeb.

## <a name="create-the-iis-site"></a>Vytvoření webu služby IIS

1. V hostitelském systému vytvořte složku tak, aby obsahovala soubory a složky publikované aplikace. Rozložení nasazení aplikace je podrobněji popsaná [adresářovou strukturu](xref:host-and-deploy/directory-structure) tématu.

1. V rámci do nové složky, vytvořte *protokoly* složku pro uložení protokoly stdout ASP.NET základní modul, pokud je povoleno protokolování stdout. Pokud je aplikace nasazena pomocí *protokoly* složku v datové části, tento krok přeskočit. Pokyny k povolení nástroje MSBuild k vytvoření *protokoly* najdete v části složky automaticky, když místně, sestavení projektu [adresářovou strukturu](xref:host-and-deploy/directory-structure) tématu.

   > [!IMPORTANT]
   > Protokol stdout lze použijte pouze k řešení chyb při spuštění aplikace. Nikdy nepoužívejte stdout protokolování pro běžné aplikace protokolování. Neexistuje žádné omezení velikosti souboru protokolu nebo počet soubory protokolů vytvořené. Fond aplikací, musí mít oprávnění k zápisu do umístění, kde se zapisují protokoly. Musí existovat všechny složky na cestu k umístění protokolu. Další informace o protokolu stdout najdete v tématu [vytvoření a přesměrování protokolu](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection). Informace o protokolování v aplikaci ASP.NET Core, najdete v článku [protokolování](xref:fundamentals/logging/index) tématu.

1. V **Správce služby IIS**, otevřete uzel serveru v **připojení** panelu. Klikněte pravým tlačítkem myši **lokality** složky. Vyberte **přidat web** v kontextové nabídce.

1. Zadejte **název lokality** a nastavte **fyzická cesta** do složky pro nasazení aplikace. Zadejte **vazby** konfigurace a vytvořit web výběrem **OK**:

   ![Zadejte název lokality, fyzickou cestu a název hostitele v kroku přidat web.](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > Vazby nejvyšší úrovně zástupný znak (`http://*:80/` a `http://+:80`) by měl **není** použít. Vazby nejvyšší úrovně zástupný znak můžete otevřít vaší aplikaci k ohrožení zabezpečení. To platí pro silné a slabé zástupné znaky. Použijte explicitní hostitele názvy místo zástupných znaků. Vazba subdomény zástupný znak (například `*.mysub.com`) nemá toto bezpečnostní riziko, pokud řízení celého nadřazené domény (Naproti tomu `*.com`, což je snadno napadnutelný). V tématu [rfc7230 části-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) Další informace.

1. V uzlu serveru, vyberte **fondy aplikací**.

1. Klikněte pravým tlačítkem na fond aplikací webu a vyberte **základní nastavení** v kontextové nabídce.

1. V **upravit fond aplikací** nastavte **verze modulu .NET CLR** k **bez spravovaného kódu**:

   ![Nastavit bez spravovaného kódu pro verze modulu .NET CLR.](index/_static/edit-apppool-ws2016.png)

    ASP.NET Core spouští v samostatném procesu a spravuje modulu runtime. ASP.NET Core není spoléhají na načítání plochy CLR. Nastavení **verze modulu .NET CLR** k **bez spravovaného kódu** je volitelný.

1. Potvrďte, že identita procesu modelu má příslušná oprávnění.

   Pokud výchozí identitu fondu aplikací (**Model procesu** > **Identity**) se změní z **ApplicationPoolIdentity** na jinou identitu, ověřte, zda novou identitu má požadovaná oprávnění k přístupu aplikace složka, databáze a další požadované prostředky. Například fond aplikací vyžaduje oprávnění ke čtení a zápisu do složky, kde aplikace čte a zapisuje soubory.

**Konfigurace ověřování systému Windows (volitelné)**  
Další informace najdete v tématu [ověřování systému Windows nakonfigurovat](xref:security/authentication/windowsauth).

## <a name="deploy-the-app"></a>Nasazení aplikace

Nasazení aplikace na složku vytvořenou v hostitelském systému. [Nasazení webové](/iis/publish/using-web-deploy/introduction-to-web-deploy) je doporučené mechanismus pro nasazení.

### <a name="web-deploy-with-visual-studio"></a>Nasazení webu pomocí sady Visual Studio

Najdete v článku [profilů publikování sady Visual Studio pro nasazování aplikací ASP.NET Core](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) tématu se dozvíte, jak vytvořit profil publikování pro použití pomocí nástroje nasazení webu. Pokud poskytovatel hostingu poskytuje profilu publikování nebo podporu pro vytváření jeden, stáhněte si svůj profil a importujte jej pomocí sady Visual Studio **publikovat** dialogové okno.

![Dialogové okno stránky publikování](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Nasazení webu mimo Visual Studio

[Nasazení webové](/iis/publish/using-web-deploy/introduction-to-web-deploy) lze také použít mimo Visual Studio z příkazového řádku. Další informace najdete v tématu [nástroj pro nasazení webu](/iis/publish/using-web-deploy/use-the-web-deployment-tool).

### <a name="alternatives-to-web-deploy"></a>Alternativy k webového nasazení

Přesunout aplikaci k hostování systému, jako je ruční kopírování, Xcopy, Robocopy nebo prostředí PowerShell můžete použijte některou z několika metod.

Další informace o nasazení základní technologie ASP.NET do služby IIS najdete v tématu [materiály pro nasazení pro správce služby IIS](#deployment-resources-for-iis-administrators) části.

## <a name="browse-the-website"></a>Přejděte na web

![V prohlížeči Microsoft Edge načetl úvodní stránce služby IIS.](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a>Soubory uzamčení nasazení

Soubory ve složce nasazení jsou zamčené, když aplikace běží. Během nasazení nejde přepsat uzamčení souborů. Chcete-li uvolnit uzamčení soubory v nasazení, zastavit fond aplikace pomocí **jeden** z následujících postupů:

* Použití nástroje nasazení webu a odkazu `Microsoft.NET.Sdk.Web` v souboru projektu. *App_offline.htm* soubor je umístěn v kořenovém adresáři webové aplikace. Když se soubor nachází, modul ASP.NET Core řádně ukončí aplikaci a slouží *app_offline.htm* souboru během nasazení. Další informace najdete v tématu [odkazu na modul jádro ASP.NET konfigurace](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).
* Ručně zastavte fond aplikací ve Správci služby IIS na serveru.
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

## <a name="data-protection"></a>Ochrana dat

[Ochranu dat ASP.NET Core zásobníku](xref:security/data-protection/index) používá několik ASP.NET Core [middlewares](xref:fundamentals/middleware/index), včetně middleware používané v ověřování. I když nejsou rozhraní Data Protection API volat pomocí uživatelského kódu, ochrany dat by měl být nakonfigurovaný pomocí skriptu pro nasazení nebo v uživatelském kódu k vytvoření trvalé kryptografických [úložiště klíčů](xref:security/data-protection/implementation/key-management). Pokud není nakonfigurovaná ochrana dat, jsou klíče uložené v paměti a zahozené během restartování aplikace.

Pokud prstenec klíče jsou uloženy v paměti po restartování aplikace:

* Všechny tokeny na základě souboru cookie ověřování jsou zneplatněny. 
* Uživatelé musí znovu přihlásit na jejich další požadavek. 
* Všechna data chráněné pomocí prstenec klíč lze už dešifrovat. To může zahrnovat [tokeny proti útokům CSRF](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) a [soubory cookie ASP.NET Core MVC TempData](xref:fundamentals/app-state#tempdata).

Ke konfiguraci ochrany dat v rámci služby IIS k uchování prstenec klíč, použijte **jeden** z následujících postupů:

* **Vytvoření klíče registru ochranu dat**

  Klíče ochrany dat používá aplikace ASP.NET Core jsou uloženy v registru externí aplikací. Chcete-li zachovat klíče pro danou aplikaci, vytvořte klíče registru pro fond aplikací.

  Pro samostatné instalace služby IIS – webová farma [skript prostředí PowerShell AutoGenKeys.ps1 zřízení ochrany dat](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) lze použít pro každý fond aplikací používat s aplikací ASP.NET Core. Tento skript vytvoří klíč registru v registru HKLM, které je přístupné pouze pro účet pracovního procesu fondu aplikací aplikace. Klíče jsou zašifrovaná přinejmenším pomocí rozhraní DPAPI klíčem celého systému.

  Ve scénářích webové farmy lze nastavit pomocí cesty UNC ukládat její prstenec klíč ochrany dat aplikace. Ve výchozím nastavení nejsou klíče ochrany dat šifrována. Zkontrolujte, zda jsou omezená na účet systému Windows, které aplikace běží v části oprávnění pro sdílené síťové složce. X509 certifikát můžete použít k ochraně klíče v klidovém stavu. Vezměte v úvahu mechanismus, aby uživatelé mohli nahrát certifikátů: místní certifikáty do důvěryhodného certifikátu uživatele, ukládání a ujistěte se, jsou k dispozici na všech počítačích, kde je spuštěna aplikace uživatele. V tématu [Konfigurace ochrany dat ASP.NET Core](xref:security/data-protection/configuration/overview) podrobnosti.

* **Nakonfigurujte fond aplikací služby IIS načíst profil uživatele**

  Toto nastavení je ve **Model procesu** oddílu pod **Upřesnit nastavení** pro fond aplikací. Nastavit načíst profil uživatele na `True`. To ukládá klíče v adresáři profilu uživatele a chrání je pomocí rozhraní DPAPI klíčem specifické pro uživatelský účet, který používá fond aplikací.

* **Pomocí systému souborů jako úložiště prstenec klíč**

  Upravit kód aplikace, který [pomocí systému souborů jako úložiště prstenec klíč](xref:security/data-protection/configuration/overview). Použití na X509 certifikát k ochraně klíče prstenec a ujistěte se, certifikát je důvěryhodný certifikát. Pokud je certifikát podepsaný svým držitelem, umístěte certifikát v úložišti Důvěryhodné kořenové.

  Při použití IIS ve webové farmě:

  * Použijte sdílené složky, ke kterému mají přístup všechny počítače.
  * Nasazení X509 certifikát pro každý počítač. Konfigurace [ochrany dat v kódu](xref:security/data-protection/configuration/overview).

* **Nastavit zásady celého systému ochrany dat**

  Systém ochrany dat má omezenou podporu pro nastavení výchozí [celého zásad](xref:security/data-protection/configuration/machine-wide-policy) pro všechny aplikace, které využívají rozhraní Data Protection API. Najdete v článku [dokumentace k data protection](xref:security/data-protection/index) podrobnosti.

## <a name="sub-application-configuration"></a>Dílčí aplikace konfigurace

Dílčí aplikace přidají pod kořenové aplikace by neměla zahrnovat modul ASP.NET Core jako obslužná rutina. Pokud je modul přidán jako obslužná rutina v dílčí aplikaci *web.config* souboru, *500.19 vnitřní chyba serveru* odkazující na vadný konfigurační soubor je oznámených při pokusu o vyhledání aplikace sub.

Následující příklad ukazuje publikovaný *web.config* soubor pro dílčí ASP.NET Core-aplikace:

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
      <remove name="aspNetCore" />
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

Konfigurace služby IIS je ovlivněno  **\<system.webServer >** části *web.config* pro tyto funkce služby IIS, která se týkají konfigurace reverzní proxy server. Pokud je služba IIS konfigurována na úrovni serveru použití dynamické komprese  **\<urlCompression >** element v dané aplikaci *web.config* souboru ji můžete vypnout.

Další informace najdete v tématu [odkaz Konfigurace pro \<system.webServer >](/iis/configuration/system.webServer/), [odkazu na modul Konfigurace ASP.NET Core](xref:host-and-deploy/aspnet-core-module), a [moduly služby IIS s technologií ASP.NET Základní](xref:host-and-deploy/iis/modules). Nastavení proměnných prostředí pro jednotlivé aplikace spuštěných ve fondech izolované aplikace (podporuje pro IIS 10.0 nebo novější), najdete v článku *příkazu AppCmd.exe* části [proměnné prostředí \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) téma v IIS referenční dokumentace.

## <a name="configuration-sections-of-webconfig"></a>Konfigurační oddíly souboru Web.config

Konfigurační oddíly aplikací ASP.NET 4.x v *web.config* nejsou používány nástrojem aplikace ASP.NET Core pro konfiguraci:

* **\<system.web>**
* **\<appSettings>**
* **\<connectionStrings >**
* **\<umístění >**

Aplikace ASP.NET Core jsou konfigurováni pomocí jiných poskytovatelů konfigurace. Další informace najdete v tématu [konfigurace](xref:fundamentals/configuration/index).

## <a name="application-pools"></a>Fondy aplikací

Při hostování více webů na serveru, izolace aplikace od sebe navzájem spuštěním každou aplikaci ve vlastním fondu aplikací. Služby IIS **přidat web** dialogovém okně výchozí hodnoty pro tuto konfiguraci. Když **název lokality** je zadaný text se automaticky přenese na **fond aplikací** textové pole. Nový fond aplikací se vytvoří při přidání webu pomocí názvu serveru.

## <a name="application-pool-identity"></a>Identita fondu aplikací

Účet identity fondu aplikací umožňuje aplikaci, běh jedinečný účet, aniž by museli vytvářet a spravovat domény nebo místní účty. V IIS 8.0 nebo novějším procesů Worker Správce služby IIS (WAS) vytvoří virtuální účet s názvem nového fondu aplikací a aplikace spouští pracovní procesy fondu pod tímto účtem ve výchozím nastavení. V konzole pro správu služby IIS v části **Upřesnit nastavení** pro fond aplikací, zkontrolujte, zda **Identity** je nastavený na použití **ApplicationPoolIdentity**:

![Dialogové okno Upřesnit nastavení fondu aplikací](index/_static/apppool-identity.png)

Proces správy služby IIS vytvoří zabezpečeného identifikátoru se název fondu aplikací v zabezpečení systému Windows. Prostředky se dají zabezpečit tuto identitu. Tato identita však není skutečné uživatelský účet a nezobrazí v konzole pro správu uživatelů systému Windows.

Pokud pracovní proces služby IIS vyžaduje přístup k aplikaci se zvýšeným oprávněním, upravte seznam řízení přístupu (ACL) pro adresář obsahující aplikaci:

1. Otevřete Průzkumníka Windows a přejděte do adresáře.

1. Klikněte pravým tlačítkem na adresář a vyberte **vlastnosti**.

1. V části **zabezpečení** vyberte **upravit** tlačítko a potom **přidat** tlačítko.

1. Vyberte **umístění** tlačítko a musí být vybrána systému.

1. Zadejte **IIS AppPool\\< app_pool_name >** v **zadejte názvy objektů k výběru** oblasti. Vyberte **Kontrola názvů** tlačítko. Pro *DefaultAppPool* Zkontrolujte názvy pomocí **IIS AppPool\DefaultAppPool**. Když **Kontrola názvů** výběru tlačítka hodnotu **DefaultAppPool** uvedené v oblasti názvy objektů. Není možné zadat název fondu aplikací přímo do oblasti názvy objektů. Použití **IIS AppPool\\< app_pool_name >** formátu při vyhledávání pro název objektu.

   ![Vyberte uživatele nebo skupiny dialogové okno pro složku aplikace: název fondu aplikací "DefaultAppPool" je připojen na "IIS AppPool\" v oblasti objekt názvy před výběrem"Zkontrolujte název."](index/_static/select-users-or-groups-1.png)

1. Vyberte **OK**.

   ![Vyberte uživatele nebo skupiny dialogové okno pro složku aplikace: Po výběru "Kontrola názvů", název objektu "DefaultAppPool" se zobrazí v objektu názvy oblasti.](index/_static/select-users-or-groups-2.png)

1. Čtení &amp; provést ve výchozím nastavení by měl být udělena oprávnění. Zadejte další oprávnění, podle potřeby.

Na příkazovém řádku pomocí můžete také udělit přístup **ICACLS** nástroj. Pomocí *DefaultAppPool* jako příklad slouží následující příkaz:

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

Další informace najdete v tématu [icacls](/windows-server/administration/windows-commands/icacls) tématu.

## <a name="deployment-resources-for-iis-administrators"></a>Materiály pro nasazení pro správce služby IIS

Další informace o službě IIS podrobný v dokumentaci služby IIS.  
[Dokumentace ke službě IIS](/iis)

Další informace o modelech nasazení aplikace .NET Core.  
[Nasazení aplikace .NET core](/dotnet/core/deploying/)

Zjistěte, jak modul základní technologie ASP.NET umožňuje Kestrel webového serveru použít jako reverzní proxy server služby IIS nebo IIS Express.  
[Modul ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)

Postup konfigurace modulu jádra ASP.NET pro hostování aplikací ASP.NET Core.  
[Referenční dokumentace k modulu ASP.NET Core](xref:host-and-deploy/aspnet-core-module)

Další informace o struktura adresářů publikované aplikace ASP.NET Core.  
[Adresářová struktura](xref:host-and-deploy/directory-structure)

Zjistit aktivní i neaktivní moduly služby IIS pro aplikace ASP.NET Core a jak spravovat moduly služby IIS.  
[Moduly služby IIS](xref:host-and-deploy/iis/troubleshoot)

Zjistěte, jak diagnostikovat problémy s IIS nasazení aplikací ASP.NET Core.  
[Řešení potíží](xref:host-and-deploy/iis/troubleshoot)

Rozlišení běžných chyb při hostování aplikace ASP.NET Core ve službě IIS.  
[Referenční informace k běžným chybám u služeb Azure App Service a IIS](xref:host-and-deploy/azure-iis-errors-reference)

## <a name="additional-resources"></a>Další zdroje

* [Úvod do ASP.NET Core](xref:index)
* [Lokality oficiální Microsoft IIS](https://www.iis.net/)
* [Technické knihovně obsahu systému Windows Server](/windows-server/windows-server)
