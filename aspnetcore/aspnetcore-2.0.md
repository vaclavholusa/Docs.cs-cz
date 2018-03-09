---
title: "Co je nového v technologii ASP.NET 2.0 jádra"
author: rick-anderson
description: "Co je nového v technologii ASP.NET 2.0 jádra"
manager: wpickett
ms.author: riande
ms.date: 07/10/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: aspnetcore-2.0
ms.openlocfilehash: 81d24088d53a7e37d17bb7c57892c98efb06ca6f
ms.sourcegitcommit: 53ee14b9c8200f44705d8997c3619fa874192d45
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/08/2018
---
# <a name="whats-new-in-aspnet-core-20"></a>Co je nového v technologii ASP.NET 2.0 jádra

V tomto článku klade důraz nejvýznamnějších změn v aplikaci ASP.NET 2.0 jádra, s odkazy na příslušné dokumentaci.

## <a name="razor-pages"></a>Stránky Razor

Stránky Razor je nová funkce rozhraní ASP.NET MVC jádra, která vytváří kódování zaměřené na stránce scénáře jednodušší a zvýšit produktivitu.

Další informace najdete v tématu Úvod a kurzu:

* [Úvod do stránky Razor](xref:mvc/razor-pages/index)
* [Začínáme se stránkami Razor](xref:tutorials/razor-pages/razor-pages-start)

## <a name="aspnet-core-metapackage"></a>Metapackage ASP.NET Core

Nové metapackage ASP.NET Core zahrnuje všechny balíčky provedené a nepodporuje týmy ASP.NET Core a Entity Framework Core spolu s jejich interní a 3. stran závislosti. Již nepotřebujete vyberte jednotlivé ASP.NET Core funkce balíčkem. Všechny funkce jsou součástí [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) balíčku. Tento balíček použít výchozí šablony.

Další informace najdete v tématu [Microsoft.AspNetCore.All metapackage pro technologii ASP.NET 2.0 základní](xref:fundamentals/metapackage).

## <a name="runtime-store"></a>Modul runtime úložiště

Aplikace, které používají `Microsoft.AspNetCore.All` metapackage automaticky využít výhod nové úložiště .NET Core Runtime. Úložiště obsahuje všechny prostředky runtime potřebné ke spuštění aplikace ASP.NET 2.0 jádra. Při použití `Microsoft.AspNetCore.All` metapackage žádné prostředky z odkazované balíčky ASP.NET Core NuGet nasazených s aplikací, protože se již nacházejí v cílovém systému. Prostředky v úložišti Runtime jsou také předkompilovaných ke zlepšení času spuštění aplikace.

Další informace najdete v tématu [Runtime úložiště](https://docs.microsoft.com/dotnet/core/deploying/runtime-store)

## <a name="net-standard-20"></a>Rozhraní .NET standard 2.0

Balíčky technologii ASP.NET 2.0 základní cíle standardní rozhraní .NET 2.0. Balíčky mohou odkazovat další rozhraní .NET 2.0 standardní knihovny a jejich spuštěním na .NET Standard 2.0 kompatibilní implementace rozhraní .NET, včetně .NET Core 2.0 a .NET Framework 4.6.1. 

`Microsoft.AspNetCore.All` Metapackage cílem .NET Core 2.0, protože je určen pro použití s úložištěm modulu Runtime .NET Core 2.0.

## <a name="configuration-update"></a>Aktualizace konfigurace

`IConfiguration` Instance je přidat do kontejneru služby ve výchozím nastavení v technologii ASP.NET 2.0 jádra. `IConfiguration` v služby kontejneru usnadňuje aplikace k načtení hodnoty konfigurace z kontejneru.

Informace o stavu plánované dokumentaci najdete v tématu [potíže Githubu](https://github.com/aspnet/Docs/issues/3387).

## <a name="logging-update"></a>Protokolování aktualizace

V aplikaci ASP.NET 2.0 jádra je ve výchozím nastavení protokolování součástí systému (DI) vkládání závislostí. Můžete přidat zprostředkovatele a nakonfigurovat filtrování v *Program.cs* místo v souboru *Startup.cs* souboru. A ve výchozím nastavení `ILoggerFactory` podporuje filtrování způsobem, který vám umožní používat jeden flexibilní přístup pro filtrování mezi zprostředkovatele i filtrování specifické pro zprostředkovatele.

Další informace najdete v tématu [Úvod k protokolování](xref:fundamentals/logging/index).

## <a name="authentication-update"></a>Aktualizace ověřování

Nový model ověřování usnadňuje nakonfigurovat ověřování pro aplikaci pomocí DI.

Nové šablony jsou dostupné pro konfiguraci ověřování pro webové aplikace a webové rozhraní API pomocí [Azure AD B2C] (https://azure.microsoft.com/services/active-directory-b2c/).

Informace o stavu plánované dokumentaci najdete v tématu [potíže Githubu](https://github.com/aspnet/Docs/issues/3054).

## <a name="identity-update"></a>Aktualizace identity

Provedli jsme je snazší sestavovat zabezpečené webové rozhraní API pomocí Identity v technologii ASP.NET 2.0 jádra. Můžete získat přístupové tokeny pro přístup k pomocí webového rozhraní API [Microsoft ověřování knihovny (MSAL)](https://www.nuget.org/packages/Microsoft.Identity.Client).

Další informace o ověřování změny v 2.0 naleznete v následujících zdrojích informací:

* [Potvrzení účtu a obnovení hesla v ASP.NET Core](xref:security/authentication/accconfirm)
* [Povolení generování kód QR pro aplikace v ASP.NET Core](xref:security/authentication/identity-enable-qrcodes)
* [Migrace ověřování a identita na jádro ASP.NET 2.0](xref:migration/1x-to-2x/identity-2x)

## <a name="spa-templates"></a>SPA šablony

Jeden šablony projektů stránky aplikace (SPA) pro úhlová, Aurelia, Knockout.js, React.js a React.js s Redux jsou k dispozici. Úhlová šablony je aktualizovaná tak, aby úhlová 4. Úhlová a reagují šablony jsou dostupné ve výchozím nastavení; informace o tom, jak získat další šablony najdete v tématu [vytvoření nového projektu SPA](xref:client-side/spa-services#creating-a-new-project). Informace o tom, jak sestavit SPA v ASP.NET Core najdete v tématu [pomocí JavaScriptServices pro vytváření jednostránkové aplikace](xref:client-side/spa-services).

## <a name="kestrel-improvements"></a>Vylepšení kestrel

Webový server Kestrel obsahuje nové funkce, které lépe vyhovovalo jako internetový server. Počet možnosti konfigurace serveru omezení přidán v `KestrelServerOptions` třída je nové `Limits` vlastnost. Přidejte následující omezení:

- Maximální počet klientských připojení
- Velikost textu maximální požadavku
- Minimální požadavek textu přenosová rychlost

Další informace najdete v tématu [Kestrel webového serveru implementace v ASP.NET Core](xref:fundamentals/servers/kestrel).

## <a name="weblistener-renamed-to-httpsys"></a>WebListener přejmenovat ovladače HTTP.sys

Balíčky `Microsoft.AspNetCore.Server.WebListener` a `Microsoft.Net.Http.Server` byly sloučeny do nového balíčku `Microsoft.AspNetCore.Server.HttpSys`. Obory názvů byly aktualizovány tak, aby odpovídaly.

Další informace najdete v tématu [HTTP.sys webového serveru implementace v ASP.NET Core](xref:fundamentals/servers/httpsys).

## <a name="enhanced-http-header-support"></a>Rozšířená podpora hlavičky protokolu HTTP

Při použití MVC přenést `FileStreamResult` nebo `FileContentResult`, máte nyní možnost nastavit `ETag` nebo `LastModified` datum obsah přenosu. Můžete nastavit tyto hodnoty na vrácený obsah s kódem podobný následujícímu:

```csharp
var data = Encoding.UTF8.GetBytes("This is a sample text from a binary array");
var entityTag = new EntityTagHeaderValue("\"MyCalculatedEtagValue\"");
return File(data, "text/plain", "downloadName.txt", lastModified: DateTime.UtcNow.AddSeconds(-5), entityTag: entityTag);
```

Soubor vrátil návštěvnících bude označených pomocí příslušné hlavičky protokolu HTTP pro `ETag` a `LastModified` hodnoty.

Pokud aplikaci návštěvníka vyžádá obsah s hlavičku požadavku rozsahu, ASP.NET, rozpozná a zpracování této hlavičky. Pokud je požadovaný obsah mohou být zajišťovány částečně, ASP.NET správně přeskočit a vrátit pouze požadovanou sadu bajtů.  Nepotřebujete k zápisu do vaší metody přizpůsobit nebo zpracování této funkce; všechny speciální obslužné rutiny prováděno automaticky za vás.

## <a name="hosting-startup-and-application-insights"></a>Hostování spuštění a Application Insights

Teď hostitelská prostředí můžete vložit balíčku Další závislosti a spuštění kódu při spuštění aplikace, bez nutnosti explicitně trvat závislost nebo volat všechny metody aplikace. Tato funkce slouží k povolení určitých prostředích "light-up" funkcí, které jsou jedinečné pro prostředí bez aplikace nepotřebuje vědět předem. 

V aplikaci ASP.NET 2.0 jádra, tato funkce slouží automaticky povolí se Diagnostika Application Insights při ladění v sadě Visual Studio a (po výslovném souhlasu) při spuštění v Azure App Services. V důsledku toho šablony projektů už přidejte Application Insights balíčky a kódu ve výchozím nastavení.

Informace o stavu plánované dokumentaci najdete v tématu [potíže Githubu](https://github.com/aspnet/Docs/issues/3389).

## <a name="automatic-use-of-anti-forgery-tokens"></a>Automatické použití tokeny proti zfalšování

ASP.NET Core vždy pomohlo kódování HTML obsah ve výchozím nastavení, ale s novou verzí pořízení krok navíc pomohou zabránit útokům (XSRF) padělání požadavku posílaného mezi weby. ASP.NET Core bude nyní emitování tokeny proti zfalšování ve výchozím nastavení a ověření je na akce POST formuláře a stránkách bez další konfigurace.

Další informace najdete v tématu [zabránit webů požadavku padělání (XSRF/proti útokům CSRF) před útoky](xref:security/anti-request-forgery).

## <a name="automatic-precompilation"></a>Automatická předkompilace

Předběžné kompilace zobrazení syntaxe Razor je standardně během publikování, snižuje velikost výstup publikování a aplikace čas spuštění.

Další informace najdete v tématu [kompilace zobrazení syntaxe Razor a předkompilaci v ASP.NET Core](xref:mvc/views/view-compilation).

## <a name="razor-support-for-c-71"></a>Podpora syntaxe Razor pro C# 7.1

Pro práci s novou kompilátoru Roslyn byla aktualizována zobrazovací modul Razor. Který zahrnuje podporu pro funkce C# 7.1 jako výchozí výrazy, odvodit názvy řazené kolekce členů a porovnávání s obecnými typy. Použití jazyka C# 7.1 v projektu, přidejte následující vlastnost v souboru projektu a potom ho znovu načtěte řešení:

```xml
<LangVersion>latest</LangVersion>
```

Informace o stavu funkcí jazyka C# 7.1 najdete v tématu [úložiště Roslyn GitHub](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md).

## <a name="other-documentation-updates-for-20"></a>Další aktualizace dokumentace pro 2.0

* [Visual Studio publikační profily pro nasazení aplikace ASP.NET Core](xref:host-and-deploy/visual-studio-publish-profiles)
* [Správa klíčů](xref:security/data-protection/implementation/key-management)
* [Konfigurace ověřování Facebook](xref:security/authentication/facebook-logins)
* [Konfigurace ověřování služby Twitter.](xref:security/authentication/twitter-logins)
* [Konfigurace ověřování Google](xref:security/authentication/google-logins)
* [Konfigurace ověřování Account Microsoft](xref:security/authentication/microsoft-logins)

## <a name="migration-guidance"></a>Pokyny pro migraci

Pokyny k migraci aplikace ASP.NET Core 1.x na technologii ASP.NET 2.0 jádra najdete v následujících zdrojích informací:

* [Migrace z ASP.NET Core 1.x na jádro ASP.NET 2.0](xref:migration/1x-to-2x/index)
* [Migrace ověřování a identita na jádro ASP.NET 2.0](xref:migration/1x-to-2x/identity-2x)

## <a name="additional-information"></a>Další informace

Úplný seznam změn, najdete v článku [poznámky k verzi ASP.NET Core 2.0](https://github.com/aspnet/Home/releases/tag/2.0.0).

Pro připojení k vývojový tým ASP.NET Core průběh a plány, naladit [ASP.NET komunity Standup](https://live.asp.net/).
