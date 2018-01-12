---
title: "Práce s několika prostředí v ASP.NET Core"
author: ardalis
description: "Zjistěte, jak ASP.NET Core poskytuje podporu pro řízení chování aplikace ve více prostředích."
keywords: "ASP.NET Core, nastavení prostředí, ASPNETCORE_ENVIRONMENT"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b5bba985-be12-4464-9a01-df3599b2a6f1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: 9127c3d7180422c0e3dbd813340dd485bf360c81
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/11/2018
---
# <a name="working-with-multiple-environments"></a>Práce s několika prostředí

Podle [Steve Smith](https://ardalis.com/)

Základní technologie ASP.NET poskytuje podporu pro řízení chování aplikace ve více prostředích, jako je například vývoj, pracovní a provozní. Proměnné prostředí jsou slouží k určení běhové prostředí, umožňuje aplikaci nakonfigurovat pro prostředí.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="development-staging-production"></a>Vývoj, pracovní, výroby

ASP.NET Core odkazuje na dané proměnné prostředí, `ASPNETCORE_ENVIRONMENT` k popisu aplikace je aktuálně spuštěna v prostředí. Tuto proměnnou lze nastavit na jakoukoli hodnotu se vám líbí, ale tří hodnot používají konvence: `Development`, `Staging`, a `Production`. Zjistíte, tyto hodnoty použité v ukázkách a šablony, které jsou součástí ASP.NET Core.

Aktuální nastavení prostředí zjistit prostřednictvím kódu programu z v rámci vaší aplikace. Kromě toho můžete použít prostředí [značky pomocná](../mvc/views/tag-helpers/index.md) zahrnout určité části vaší [zobrazení](../mvc/views/index.md) na základě aktuální prostředí aplikace.

Poznámka: V systémech Windows a systému macOS prostředí zadaný název je malá a velká písmena. Jestli nastavte proměnnou na `Development` nebo `development` nebo `DEVELOPMENT` výsledky budou stejné. Je však Linux **velká a malá písmena** operačního systému ve výchozím nastavení. Proměnné prostředí, názvy souborů a nastavení vyžadují malá a velká písmena.

### <a name="development"></a>Vývoj

To by měl být prostředí používá při vývoji aplikace. Obvykle se používá k povolení funkcí, které chcete by být k dispozici při spuštění aplikace v provozním prostředí, například [vývojáře výjimka stránky](xref:fundamentals/error-handling#the-developer-exception-page).

Pokud používáte Visual Studio, prostředí se dá nakonfigurovat v projektu na ladicí profily. Ladění profily zadejte [server](xref:fundamentals/servers/index) nastavit pomocí při spuštění aplikace a proměnných prostředí. Projekt může mít více ladicí profily, které jinak nastavení proměnných prostředí. Správy těchto profilů pomocí **ladění** kartě projektu webové aplikace **vlastnosti** nabídky. S hodnotami nastavenými v okně Vlastnosti projektu zůstávají v *launchSettings.json* soubor a také můžete konfigurovat profily přímou úpravou tohoto souboru.

Profil pro službu IIS Express je znázorněno zde:

![Proměnné prostředí nastavení vlastností projektu](environments/_static/project-properties-debug.png)

Tady je `launchSettings.json` soubor, který zahrnuje profily pro `Development` a `Staging`:

[!code-json[Main](../fundamentals/environments/sample/src/Environments/Properties/launchSettings.json?highlight=15,22)]

Změny provedené v projektu profily pravděpodobně se projeví až po restartu webového serveru používá (konkrétně Kestrel musí být restartován zjistí změny provedené v jeho prostředí).

>[!WARNING]
> Proměnné prostředí uložené v *launchSettings.json* nejsou zabezpečené jakýmkoli způsobem a bude součástí úložiště zdrojového kódu pro svůj projekt, pokud použijete jeden. **Nikdy neukládají přihlašovací údaje nebo jiné tajné údaje v tomto souboru.** Pokud budete potřebovat místo, kam můžete ukládat taková data, použijte *tajný klíč správce* nástroje popsané v [bezpečného úložiště tajné klíče aplikace během vývoje](xref:security/app-secrets).

### <a name="staging"></a>Pracovní

Podle konvence `Staging` prostředí je předprodukční prostředí pro konečné testování před nasazením do produkčního prostředí. V ideálním případě by jeho fyzické charakteristiky by měl zrcadlení, výroby, tak, aby všechny problémy, které může způsobit v produkčním prostředí objevit jako první v testovacím prostředí, kde lze řešit bez dopadu na uživatele.

### <a name="production"></a>Produkční

`Production` Prostředí je prostředí, ve kterém je aplikace spuštěná, pokud je za provozu a používán koncovým uživatelům. Toto prostředí musí být nakonfigurovaný pro maximalizaci zabezpečení, výkonu a odolnosti aplikace. Některé běžné nastavení, která může mít provozním prostředí které by se liší od vývoj patří:

* Zapnutí ukládání do mezipaměti

* Ujistěte se, všechny prostředky na straně klienta jsou seskupeny, minifikovaný a potenciálně zpracovat z název CDN

* Vypnout diagnostické ErrorPages

* Zapnout popisný chybové stránky

* Povolit produkční protokolování a monitorování (například [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/))

To se rozhodně není určená být úplný seznam. Je vhodné se vyhnout rozptylu kontroly prostředí v mnoha částí aplikace. Místo toho doporučuje se provést tyto kontroly v rámci aplikace `Startup` třídy je to možné

## <a name="setting-the-environment"></a>Nastavení prostředí

Metoda pro nastavení prostředí závisí na operačním systému.

### <a name="windows"></a>Windows
Chcete-li nastavit `ASPNETCORE_ENVIRONMENT` pro aktuální relaci, pokud je aplikace spuštěná pomocí `dotnet run`, se používají následující příkazy

**Příkazový řádek**
```
set ASPNETCORE_ENVIRONMENT=Development
```
**Prostředí PowerShell**
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Tyto příkazy se projeví pouze pro aktuální okno. Při zavření okna nastavení ASPNETCORE_ENVIRONMENT obnoví výchozí nastavení nebo počítač hodnotu. Chcete-li nastavit hodnotu globálně při otevření Windows **ovládací panely** > **systému** > **Upřesnit nastavení systému** a přidat nebo upravit `ASPNETCORE_ENVIRONMENT` hodnotu.

![Rozšířené vlastnosti systému](environments/_static/systemsetting_environment.png)

![Proměnné prostředí základní ASPNET](environments/_static/windows_aspnetcore_environment.png) 

**soubor Web.config**

Najdete v článku *nastavení proměnných prostředí* části [odkazu na modul jádro ASP.NET konfigurace](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) tématu.

**Fondy aplikací služby IIS**

Pokud je nutné nastavit proměnné prostředí pro jednotlivé aplikace běžící v izolované fondy aplikací (podporuje se ve službě IIS 10.0 +), najdete *příkazu AppCmd.exe* části [proměnné prostředí \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) téma v IIS referenční dokumentace.

### <a name="macos"></a>systému macOS
Nastavení aktuální prostředí pro systému macOS lze provést v řádku při spuštění aplikace;

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
nebo pomocí `export` pro ni nastavit před spuštěním aplikace.

```bash
export ASPNETCORE_ENVIRONMENT=Development
``` 
Proměnné prostředí úrovni počítače jsou nastaveny v *.bashrc* nebo *.bash_profile* souboru. Upravte soubor pomocí libovolného textového editoru a přidejte následující příkaz.

```
export ASPNETCORE_ENVIRONMENT=Development
```  

### <a name="linux"></a>Linux
Pro distribucích systému Linux, použijte `export` příkaz na příkazovém řádku pro relace na základě nastavení proměnné a *bash_profile* soubor pro nastavení úrovně prostředí počítače.

## <a name="determining-the-environment-at-runtime"></a>Určení prostředí za běhu

`IHostingEnvironment` Služba poskytuje základní abstrakci pro práci s prostředími. Tato služba je zadaný pomocí technologie ASP.NET hostování vrstvy a může být vloženy do logika spuštění prostřednictvím [vkládání závislostí](dependency-injection.md). Šablona webové stránky ASP.NET Core v sadě Visual Studio používá tento přístup načíst konfigurační soubory specifické pro prostředí (pokud existuje) a chcete-li přizpůsobit nastavení pro zpracování chyb aplikace. V obou případech toto chování je dosaženo tím, že odkazuje na aktuálně zadané prostředí voláním `EnvironmentName` nebo `IsEnvironment` na instanci `IHostingEnvironment` předaný do odpovídající metodu.

> [!NOTE]
> Pokud je potřeba zkontrolovat, zda je aplikace spuštěna v konkrétním prostředí, použijte `env.IsEnvironment("environmentname")` vzhledem k tomu, že bude správně můžete ignorovat případ (místo kontrole, jestli se `env.EnvironmentName == "Development"` třeba).

Můžete například použít následující kód v způsob konfigurace nastavení zpracování chyb konkrétní prostředí:

[!code-csharp[Main](environments/sample/src/Environments/Startup.cs?range=19-30)]

Pokud aplikace běží v `Development` prostředí a potom Povolí podporu runtime nezbytných k používání funkce "BrowserLink" v sadě Visual Studio, vývoj specifické chybové stránky, (které obvykle by neměl být spuštění v produkčním prostředí) a chyba speciální databáze stránky (které poskytnout způsob, jak použít migrace a proto lze používat pouze v vývoj). Jinak Pokud není aplikace spuštěna v prostředí pro vývoj, standardní chyba zpracování stránky je nakonfigurovaná zobrazený v reakci na jakékoli neošetřené výjimky.

Musíte určit, které obsah odeslat do klienta v době běhu v závislosti na aktuálním prostředí. Například ve vývojovém prostředí je obecně sloužit-minimalizaci skriptů a stylů, které umožňuje snazší ladění. Produkční a testovací prostředí by měla sloužit minifikovaný verze a obecně z název CDN. Můžete provést pomocí prostředí [značky pomocná](../mvc/views/tag-helpers/intro.md). Pomocník značky prostředí pouze vykreslí obsah Pokud aktuální prostředí odpovídá jednomu z prostředí zadat pomocí `names` atribut.

[!code-html[Main](environments/sample/src/Environments/Views/Shared/_Layout.cshtml?range=13-22)]

Chcete-li začít s použitím značky Pomocníci najdete v tématu vaše aplikace [Úvod do pomocné rutiny značky](../mvc/views/tag-helpers/intro.md).

## <a name="startup-conventions"></a>Spuštění konvence

Jádro ASP.NET podporuje založené na konvenci přístup k nastavení konfigurace spuštění aplikace na základě aktuálního prostředí. Můžete také programově řídit chování aplikací na podle do prostředí, ve kterém je v, což umožňuje vytvářet a spravovat vlastní konvence.

Při spuštění aplikace ASP.NET Core, `Startup` třída se používá k bootstrap aplikace a načtěte jeho nastavení atd ([Další informace o spouštění ASP.NET](startup.md)). Však s názvem pokud existuje třída `Startup{EnvironmentName}` (například `StartupDevelopment`) a `ASPNETCORE_ENVIRONMENT` proměnnou prostředí shoduje s tímto názvem a pak `Startup` třída se používá místo. Proto může nakonfigurovat `Startup` pro vývoj, ale mít samostatnou `StartupProduction` který se použije při spuštění aplikace v produkčním prostředí. Nebo naopak.

> [!NOTE]
> Volání metody `WebHostBuilder.UseStartup<TStartup>()` přepsání konfigurační oddíly.

Kromě použití zcela samostatné `Startup` třídy založené na aktuálním prostředí, můžete také provádět úpravy konfigurace aplikace v rámci `Startup` třídy. `Configure()` a `ConfigureServices()` metody podporují konkrétní prostředí verze, podobně jako `Startup` třídy samostatně, formuláře `Configure{EnvironmentName}()` a `Configure{EnvironmentName}Services()`. Pokud definujete metodu `ConfigureDevelopment()` nezavolá se místo `Configure()` když prostředí je nastavená na vývoj. Podobně `ConfigureDevelopmentServices()` by být volána místo `ConfigureServices()` ve stejném prostředí.

## <a name="summary"></a>Souhrn

Základní technologie ASP.NET poskytuje několik funkcí a konvence, které umožňují vývojářům snadno řídit chování svých aplikací v různých prostředích. Při publikování aplikace z vývojového do pracovní do produkčního prostředí, proměnným prostředí nastaveným správně pro prostředí povolit pro optimalizaci aplikace pro ladění, testování nebo produkční použití, podle potřeby.

## <a name="additional-resources"></a>Další prostředky

* [Konfigurace](xref:fundamentals/configuration/index)

* [Úvod do pomocné rutiny značky](../mvc/views/tag-helpers/intro.md)
