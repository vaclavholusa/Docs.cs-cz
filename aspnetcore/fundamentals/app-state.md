---
title: Stav relace a aplikace v ASP.NET Core
author: rick-anderson
description: Zjistit přístupy k zachování stavu relace a aplikace mezi požadavky.
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2018
uid: fundamentals/app-state
ms.openlocfilehash: 9c63d9313acb055e6c692a7fef3d28e94cb37093
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272880"
---
# <a name="session-and-app-state-in-aspnet-core"></a>Stav relace a aplikace v ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), [Diana LaRose](https://github.com/DianaLaRose), a [Luke Latham](https://github.com/guardrex)

HTTP je bezstavový. Bez nutnosti převádět další kroky, požadavky HTTP jsou nezávislé zprávy, které nezachovají hodnoty uživatele nebo stav aplikace. Tento článek popisuje několik přístupů, které chcete zachovat stav aplikací a dat uživatele mezi požadavky.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="state-management"></a>Stav správy

Stav může být uložen několik přístupů. Každý postup je popsán dále v tomto tématu.

| Přístup k ukládání | Mechanismus úložiště |
| ---------------- | ----------------- |
| [Soubory cookie](#cookies) | Soubory cookie protokolu HTTP (mohou zahrnovat data uložená pomocí kódu aplikace na straně serveru) |
| [Stav relace](#session-state) | Soubory cookie protokolu HTTP a kód aplikace na straně serveru |
| [TempData](#tempdata) | Soubory cookie protokolu HTTP nebo stav relace |
| [Řetězce dotazů](#query-strings) | Řetězce dotazu HTTP |
| [Skrytá pole](#hidden-fields) | Pole formuláře HTTP |
| [HttpContext.Items](#httpcontextitems) | Kód aplikace na straně serveru |
| [Mezipaměti](#cache) | Kód aplikace na straně serveru |
| [Injektáž závislostí](#dependency-injection) | Kód aplikace na straně serveru |

## <a name="cookies"></a>Soubory cookie

Soubory cookie ukládat data napříč požadavky. Soubory cookie jsou odesílány s každou žádost, jejich velikost měli omezit na minimum. V ideálním případě by měly jenom identifikátor uložené v souboru cookie s daty uloženými aplikací. Většina prohlížečů omezit velikost souboru cookie na 4096 bajtů. Pouze omezený počet souborů cookie jsou k dispozici pro každou doménu.

Protože soubory cookie se vztahují manipulaci, musí být ověřený aplikací. Soubory cookie může odstranit uživatele a vyprší na klientských počítačích. Soubory cookie jsou však obecně nejvíce trvanlivá formu trvalosti dat na straně klienta.

Soubory cookie se často používají pro přizpůsobení, kde je obsah přizpůsobit pro známé uživatele. Uživatel je pouze identifikovat a ve většině případů není ověřen. Soubor cookie může ukládat jméno uživatele, název účtu nebo jedinečné ID uživatele (například identifikátor GUID). Pak můžete soubor cookie pro přístup k individuální nastavení uživatele, jako je například jejich barvu pozadí upřednostňované webu.

Mělo pamatovat [Evropské unie obecné Data Protection předpisy (GDPR)](https://ec.europa.eu/info/law/law-topic/data-protection) při vydávání soubory cookie a týkajících se ochrany osobních údajů týká. Další informace najdete v tématu [podporu obecné Data Protection nařízení (GDPR) v ASP.NET Core](xref:security/gdpr).

## <a name="session-state"></a>Stav relace

Stav relace se o scénář ASP.NET Core pro ukládání dat uživatele, když uživatel prohlíží webovou aplikaci. Stav relace používá úložiště spravuje pomocí aplikace pro uložení dat napříč požadavky klientů. Data relace je zálohovaný mezipaměti a považována za dočasných dat&mdash;webu by měly být nadále fungovat bez data relace.

> [!NOTE]
> Relace není podporována v [SignalR](xref:signalr/index) aplikace protože [rozbočovače SignalR](xref:signalr/hubs) může spustit nezávisle na kontextu HTTP. Například to může dojít, když typ long požadavku dotazování je otevřena podle rozbočovač za dobu životnosti kontext žádosti HTTP.

ASP.NET Core Udržovat stav relace tím, že poskytuje soubor cookie klientovi, který obsahuje ID relace, které se odesílají do aplikace s každou žádostí. Aplikace používá ID relace se načíst data relace.

Stav relace projevuje následující chování:

* Vzhledem k tomu, že soubor cookie relace prohlížeč, relace nejsou sdílená mezi prohlížeče.
* Soubory cookie relací jsou odstraněny při ukončení relace prohlížeče.
* Pokud je soubor cookie pro relaci s ukončenou platností, je vytvořit novou relaci, který používá stejný soubor cookie relace.
* Prázdný relace nejsou uchována&mdash;relace musí mít alespoň jednu hodnotu nastavit do ní k uchování relace napříč požadavky. Když relace není zachována, nové ID relace se generuje pro každý nový požadavek.
* Aplikace uchovává relace po omezenou dobu od poslední žádosti. Aplikace načte časový limit relace nebo výchozí hodnotu 20 minut. Stav relace je ideální pro ukládání uživatelských dat, která je specifická pro konkrétní relace, ale kde data nevyžaduje trvalé úložiště napříč relacemi.
* Data relace se odstraní buď když [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) je volána implementace nebo vypršení platnosti relace.
* Neexistuje žádný výchozí mechanismus k informování kód aplikace bylo ukončeno prohlížeče klienta nebo když je soubor cookie relace odstranit nebo platnost na straně klienta.

> [!WARNING]
> Citlivá data neukládejte do stavu relace. Uživatel nemusí zavřete prohlížeč a zrušte souboru cookie relace. Některé prohlížeče soubory cookie relace platná udržovat napříč okna prohlížeče. Relaci nemusí být omezen na jednoho uživatele&mdash;další uživatel může pokračovat ve procházet aplikace pomocí stejného souboru cookie relace.

Poskytovatel mezipaměti v paměti ukládá data relace v paměti serveru, na kterém se nachází aplikace. Ve scénáři farmy serveru:

* Použití *trvalé relace* ke svázání každou relaci do instance konkrétní aplikaci na individuálním serveru. [Aplikační služba Azure](https://azure.microsoft.com/services/app-service/) používá [aplikace směrování žádostí na](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) vynutit trvalé relace ve výchozím nastavení. Trvalé relace však může mít vliv na škálovatelnost a zkomplikovat aktualizace webové aplikace. Lepší možných přístupů je použít Redis nebo SQL Server distribuované mezipaměti, která nevyžaduje trvalé relace. Další informace najdete v tématu [pracovat s distribuované mezipaměti](xref:performance/caching/distributed).
* Soubor cookie relace se šifruje pomocí [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector). Ochrana dat musí být správně nakonfigurované ke čtení souborů cookie relací na každém počítači. Další informace najdete v tématu [ochrany dat v ASP.NET Core](xref:security/data-protection/index) a [klíče poskytovatelů úložiště](xref:security/data-protection/implementation/key-storage-providers).

### <a name="configure-session-state"></a>Nakonfigurovat stav relace

::: moniker range=">= aspnetcore-2.0"

[Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) balíček, který je součástí [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), poskytuje middleware pro správu stavu relace. Povolit middleware relace `Startup` musí obsahovat:

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) balíček poskytuje middleware pro správu stavu relace. Povolit middleware relace `Startup` musí obsahovat:

::: moniker-end

* Některé z [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) mezipaměti paměti. `IDistributedCache` Implementace slouží jako úložiště zálohování pro relaci. Další informace najdete v tématu [pracovat s distribuované mezipaměti](xref:performance/caching/distributed).
* Volání [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) v `ConfigureServices`.
* Volání [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) v `Configure`.

Následující kód ukazuje, jak nastavit Zprostředkovatel relací v paměti s výchozí implementace v paměti `IDistributedCache`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13-18,39)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5,7-12,19)]

::: moniker-end

Je důležité pořadí middleware. V předchozím příkladu `InvalidOperationException` došlo k výjimce při `UseSession` je volána poté, co `UseMvc`. Další informace najdete v tématu [řazení Middleware](xref:fundamentals/middleware/index#ordering).

[HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) je k dispozici po dokončení konfigurace stavu relace.

`HttpContext.Session` Nelze získat přístup před `UseSession` byla volána.

Nelze vytvořit novou relaci pomocí souboru cookie relace, po aplikaci byl zahájen zápis do datového proudu odpovědi. Výjimka je zaznamená do protokolu webového serveru a nezobrazuje se v prohlížeči.

### <a name="load-session-state-asynchronously"></a>Asynchronně načíst stav relace

Výchozí zprostředkovatel relace v ASP.NET Core načte relace záznamů z základní [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) asynchronně pouze v případě zálohování úložiště [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) explicitně volána metoda před [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue), [nastavit](/dotnet/api/microsoft.aspnetcore.http.isession.set), nebo [odebrat](/dotnet/api/microsoft.aspnetcore.http.isession.remove) metody. Pokud `LoadAsync` není jako první, základní záznam relace je načtena synchronně, což může způsobit snížení výkonu ve velkém měřítku.

Pokud chcete, aby aplikace vynutit tento vzor, zabalení [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) a [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) implementace s verzemi, které způsobí výjimku, pokud `LoadAsync` metoda není volána před provedením `TryGetValue`, `Set`, nebo `Remove`. Zaregistrujte zabalené verze v kontejneru služby.

### <a name="session-options"></a>Možnosti relace

Chcete-li přepsat výchozí hodnoty relace, použijte [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions).

::: moniker range=">= aspnetcore-2.0"

| Možnost | Popis |
| ------ | ----------- |
| [Soubor cookie](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookie) | Určuje nastavení používaná k vytvoření souboru cookie. [Název](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) výchozí [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`). [Cesta](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) výchozí [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`). [SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) výchozí [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`). [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) výchozí `true`. [IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) výchozí `false`. |
| [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | `IdleTimeout` Ukazuje, jak dlouho relace může být nečinnosti, než jsou opuštění její obsah. Každý přístup relace obnoví časový limit. Všimněte si, že to platí jenom pro obsah relace, není soubor cookie. Výchozí hodnota je 20 minut. |
| [IOTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.iotimeout) | Maximální množství času povoleny relace načíst z úložiště nebo provést zpět do úložiště. Všimněte si, že toto může platit pouze pro asynchronní operace. Tento časový limit můžete zakázat pomocí [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan). Výchozí hodnota je 1 minuta. |

Relace používá ke sledování a identifikaci požadavků z jednoho prohlížeče do souboru cookie. Ve výchozím nastavení, je tento soubor cookie s názvem `.AspNetCore.Session`, a používá cestu `/`. Protože výchozí soubor cookie není zadejte doménu, neprovede klientský skript k dispozici na stránce (protože [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) výchozí `true`).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| Možnost | Popis |
| ------ | ----------- |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiedomain) | Určuje doménu použitou k vytvoření souboru cookie. `CookieDomain` ve výchozím nastavení není nastaven. |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiehttponly) | Určuje, pokud v prohlížeči by měl povolit souboru cookie, ke kterým přistupují kódu JavaScript na straně klienta. Výchozí hodnota je `true`, což znamená, že soubor cookie je pouze předáván požadavkům HTTP a nebude zpřístupněn pro skript na stránce. |
| [CookieName](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiename) | Určuje název souboru cookie použitý k zachování ID relace. Výchozí hodnota je [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`). |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiepath) | Určuje cestu použitou k vytvoření souboru cookie. Použije se výchozí hodnota [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`). |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiesecure) | Určuje, pokud je soubor cookie měl být přenášen pouze na požadavky HTTPS. Výchozí hodnota je [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`). |
| [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | `IdleTimeout` Ukazuje, jak dlouho relace může být nečinnosti, než jsou opuštění její obsah. Každý přístup relace obnoví časový limit. Všimněte si, že to platí jenom pro obsah relace, není soubor cookie. Výchozí hodnota je 20 minut. |

Relace používá ke sledování a identifikaci požadavků z jednoho prohlížeče do souboru cookie. Ve výchozím nastavení, je tento soubor cookie s názvem `.AspNet.Session`, a používá cestu `/`.

::: moniker-end

Chcete-li přepsat výchozí hodnoty souboru cookie relace, použijte `SessionOptions`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/2.x/SessionSample/Startup.cs?name=snippet1&highlight=13-18)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5-9)]

::: moniker-end

Aplikace používá [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) vlastnosti k určení, jak dlouho může být relace nečinnosti před jeho obsah do mezipaměti serveru jsou opuštění. Tato vlastnost je nezávislá na vypršení platnosti souboru cookie. Každý požadavek, který prochází [relace Middleware](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware) obnoví časový limit.

Stav relace je *bez uzamčení*. Pokud dva požadavky současně pokusí změnit obsah relace, poslední žádosti přednost před první. `Session` je implementovaný jako *souvislý relace*, což znamená, že se veškerý obsah ukládat společně. Když dva požadavky požadovat upravte hodnoty různé relace, poslední žádosti může přepsat relace změny provedené při první.

### <a name="set-and-get-session-values"></a>Nastavování a získávání hodnoty relace

Stav relace je k němu přistupovat z stránky Razor [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) třídu nebo MVC [řadič](/dotnet/api/microsoft.aspnetcore.mvc.controller) třídy s [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session). Tato vlastnost je [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) implementace.

::: moniker range=">= aspnetcore-2.0"

`ISession` Implementace poskytuje několik metod rozšíření pro sadu a načíst hodnoty celé číslo a řetězec. Rozšiřující metody jsou v [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) obor názvů (Přidat `using Microsoft.AspNetCore.Http;` příkaz k získání přístupu k rozšiřující metody) při [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) balíček je odkazován objektem projektu. Oba balíčky jsou součástí [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

`ISession` Implementace poskytuje několik metod rozšíření pro sadu a načíst hodnoty celé číslo a řetězec. Rozšiřující metody jsou v [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) obor názvů (Přidat `using Microsoft.AspNetCore.Http;` příkaz k získání přístupu k rozšiřující metody) při [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) balíček je odkazován objektem projektu.

::: moniker-end

`ISession` rozšiřující metody:

* [Get (ISession, řetězec)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.get)
* [GetInt32(ISession, String)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getint32)
* [GetString – (ISession, řetězec)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getstring)
* [Setint32 – (ISession, řetězec, Int32)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setint32)
* [SetString – (ISession, řetězec, řetězec)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setstring)

Následující příklad načte hodnotu relace `IndexModel.SessionKeyName` klíč (`_Name` v ukázkové aplikace) na stránce pro stránky Razor:

```csharp
@page
@using Microsoft.AspNetCore.Http
@model IndexModel

...

Name: @HttpContext.Session.GetString(IndexModel.SessionKeyName)
```

Následující příklad ukazuje, jak nastavit a získat celé číslo a řetězec:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=18-19,22-23)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet1&highlight=10-11,18-19)]

::: moniker-end

Všechna data relace musí být serializované povolit scénáři distribuované mezipaměti, i když se používá mezipaměť v paměti. Jsou k dispozici minimální řetězec a číslo serializátorů (Viz metody a metodami rozšíření [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)). Komplexní typy musí být serializované uživatelem pomocí jiný mechanismus, jako je například JSON.

Přidejte následující metody rozšíření nastavit a získat serializovatelné objekty:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

Následující příklad ukazuje, jak nastavit a získat serializovatelný objekt pomocí metod rozšíření:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet2&highlight=4,12)]

::: moniker-end

## <a name="tempdata"></a>TempData

ASP.NET Core zpřístupní [TempData vlastnost modelu stránky stránky Razor](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata) nebo [TempData kontroler MVC](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata). Tato vlastnost ukládá data, dokud je pro čtení. [Zachovat](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) a [prohlížet](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek) metody můžete použít k prozkoumání dat bez odstranění. TempData je obzvláště užitečné pro přesměrování, pokud dat je potřeba pro více než jeden požadavek. TempData je implementováno modulem TempData zprostředkovatelů pomocí souborů cookie nebo stav relace.

### <a name="tempdata-providers"></a>Zprostředkovatelé TempData

::: moniker range=">= aspnetcore-2.0"

V technologii ASP.NET Core 2.0 nebo novější TempData zprostředkovatele na základě souborů cookie se používá ve výchozím nastavení k uložení TempData v souborech cookie.

Cookie data se šifruje pomocí [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), kódovaného s [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), pak blokové. Vzhledem k tomu, že je soubor cookie blokové, velikost jednoho souboru cookie limit v ASP.NET Core 1.x netýká nalezen. Není komprimována cookie data, protože kompresi šifrovaná data může způsobit problémy se zabezpečením, jako [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) a [porušení](https://wikipedia.org/wiki/BREACH_(security_exploit)) útoky. Další informace o poskytovateli TempData na základě souborů cookie najdete v tématu [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

TempData poskytovatele stavu relace ASP.NET Core 1.0 a 1.1, je výchozím zprostředkovatelem.

::: moniker-end

### <a name="choose-a-tempdata-provider"></a>Vyberte poskytovatele TempData

Výběr zprostředkovatele TempData zahrnuje několik důležité informace, například:

1. Aplikace už používá stav relace? Pokud ano, použití TempData poskytovatele stavu relace má bez dalších nákladů na aplikaci (kromě zajištění dostatečného množství dat).
2. Aplikace používá TempData pouze pouze pro relativně malé množství dat (až 500 bajtů)? Pokud ano, zprostředkovatele TempData souboru cookie přidá malé náklady na každý požadavek, který představuje TempData. Pokud ne, může být výhodné, aby se zabránilo odezvy velké množství dat v každé žádosti o, dokud TempData obsazením TempData poskytovatele stavu relace.
3. Aplikace spouští v serverové farmě na několika serverech? Pokud ano, neexistuje žádná další konfigurace vyžaduje použití zprostředkovatele TempData souboru cookie mimo ochrany dat (v tématu [ochrany dat](xref:security/data-protection/index) a [klíče poskytovatelů úložiště](xref:security/data-protection/implementation/key-storage-providers)).

> [!NOTE]
> Většina webovými klienty (například webové prohlížeče) vynutit omezení pro maximální velikost každého souboru cookie, celkový počet souborů cookie nebo obojí. Při použití zprostředkovatele TempData souboru cookie, ověřte, že aplikace nesmí být větší než tato omezení. Vezměte v úvahu celková velikost data. Účet pro zvyšování velikost souboru cookie z důvodu šifrování a rozdělování.

### <a name="configure-the-tempdata-provider"></a>Konfigurace zprostředkovatele TempData

::: moniker range=">= aspnetcore-2.0"

Ve výchozím nastavení je povolen TempData zprostředkovatel na základě souboru cookie.

Pokud chcete povolit TempData zprostředkovatele na bázi relace, použijte [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider) metoda rozšíření:

[!code-csharp[](app-state/samples_snapshot_2/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13,32)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Následující `Startup` kód třídy nakonfiguruje TempData zprostředkovatele na bázi relace:

[!code-csharp[](app-state/samples_snapshot_2/1.x/SessionSample/Startup.cs?name=snippet1&highlight=4,9)]

::: moniker-end

Je důležité pořadí middleware. V předchozím příkladu `InvalidOperationException` došlo k výjimce při `UseSession` je volána poté, co `UseMvc`. Další informace najdete v tématu [řazení Middleware](xref:fundamentals/middleware/index#ordering).

> [!IMPORTANT]
> Pokud cílení na rozhraní .NET Framework a pomocí TempData zprostředkovatele na bázi relace, přidejte [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) balíčku do projektu.

## <a name="query-strings"></a>Řetězce dotazů

Omezené množství dat může být předán z jednoho požadavku na jiný přidáním do novou žádost o řetězce dotazu. To je užitečné pro zaznamenání stavu trvalé způsobem, který umožňuje propojení s embedded stavu sdílení e-mailu nebo sociálních sítí. Pro citlivá data, protože adresa URL řetězce dotazu veřejné, nikdy nepoužívejte řetězce dotazu.

Kromě neúmyslnému sdílení, včetně dat v řetězcích dotazů můžete vytvořit příležitosti pro [webů požadavku padělání (proti útokům CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) útoků, které můžete obelstít návštěvou škodlivé weby při ověření uživatele. Útočníci mohou pak ukrást uživatelská data z aplikace nebo trvat škodlivé akce jménem uživatele. Všechny zachovaných aplikace nebo stav relace musí zajistit ochranu proti útokům proti útokům CSRF. Další informace najdete v tématu [zabránit webů požadavku padělání (XSRF/proti útokům CSRF) před útoky](xref:security/anti-request-forgery).

## <a name="hidden-fields"></a>Skrytá pole

Data můžete uložit ve skrytého pole a odeslány zpět na další požadavek. To je běžné ve formulářích s více stránkami. Vzhledem k tomu, že klient může potenciálně manipulovat s daty, aplikace musí vždy znovu ověřit data uložená v skrytá pole.

## <a name="httpcontextitems"></a>HttpContext.Items

[HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) kolekce se používá k ukládání dat při zpracování jedné žádosti. Obsah kolekce jsou zahozeny po zpracování požadavku. `Items` Kolekci často používá k povolení součásti nebo middleware pro komunikaci, když budou fungovat v různých okamžicích v době žádost a mají přímý způsob, jak předat parametry.

V následujícím příkladu [middleware](xref:fundamentals/middleware/index) přidá `isVerified` k `Items` kolekce.

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

Později v kanálu, další middleware přístup k hodnotě `isVerified`:

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync($"Verified: {context.Items["isVerified"]}");
});
```

Pro middleware, který se používá pouze v jedné aplikaci `string` klíče jsou přijatelné. Middleware sdílené mezi instancemi aplikace používejte jedinečný objekt předejdete klíče kolizí. Následující příklad ukazuje, jak chcete použít klíč jedinečný objekt definovaný ve třídě middleware:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=4,13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=5,14)]

::: moniker-end

Jiný kód, můžete přístup s hodnotou uloženou v `HttpContext.Items` pomocí klíče vystavené middleware třídy:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet3)]

::: moniker-end

Tento přístup má také výhod odstraňuje použití klíče řetězců v kódu.

## <a name="cache"></a>Mezipaměti

Ukládání do mezipaměti je účinný způsob, jak ukládat a načítat data. Aplikace můžete řídit životnost položek v mezipaměti.

Data uložená v mezipaměti není spojen s konkrétní žádost, uživatele nebo relace. **Dávejte pozor, aby mezipaměti uživatelská data, která může načíst žádostmi o jiných uživatelů.**

Další informace najdete v tématu [odpovědí do mezipaměti](xref:performance/caching/index) tématu.

## <a name="dependency-injection"></a>Vkládání závislostí

Použití [vkládání závislostí](xref:fundamentals/dependency-injection) jak zpřístupnit data pro všechny uživatele:

1. Definujte službu, která obsahuje data. Například třídy s názvem `MyAppData` je definována:

    ```csharp
    public class MyAppData
    {
        // Declare properties and methods
    }
    ```

2. Přidání třídy pro službu `Startup.ConfigureServices`:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<MyAppData>();
    }
    ```

3. Využívat třídě dat služby:

    ::: moniker range=">= aspnetcore-2.0"

    ```csharp
    public class IndexModel : PageModel
    {
        public IndexModel(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

    ::: moniker range="< aspnetcore-2.0"

    ```csharp
    public class HomeController : Controller
    {
        public HomeController(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

## <a name="common-errors"></a>Běžné chyby

* "Nelze přeložit služby pro typ 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' při pokusu o aktivaci 'Microsoft.AspNetCore.Session.DistributedSessionStore'."

  To je obvykle způsobeno nedaří nakonfigurovat alespoň jednu `IDistributedCache` implementace. Další informace najdete v tématu [pracovat s distribuované mezipaměti](xref:performance/caching/distributed) a [mezipaměti v paměti](xref:performance/caching/memory).

* V události, která relace, které middleware nepodaří zachovat relace (například pokud záložní úložiště není k dispozici), middleware protokoluje výjimku a požadavek pokračuje normálním způsobem. To vede k nepředvídatelné chování.

  Například uživatel ukládá nákupní košík v relaci. Uživatel přidá položku do košíku ale potvrzení se nezdaří. Aplikace neví o selhání, proto ji sestavy pro uživatele, že položka byla přidána do jejich košíku, která není pravda.

  Je doporučeným přístupem ke kontrole chyb volání `await feature.Session.CommitAsync();` z kódu aplikace, pokud aplikace se provádí zápis do relace. `CommitAsync` vyvolá výjimku, pokud záložní úložiště není k dispozici. Pokud `CommitAsync` nezdaří, aplikace může zpracovat výjimku. `LoadAsync` vyvolá za stejných podmínek, kde je úložiště dat není k dispozici.
