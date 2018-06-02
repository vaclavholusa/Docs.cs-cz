---
title: Co je nového v technologii ASP.NET Core 2.1
author: isaac2004
description: Další informace o nových funkcích v ASP.NET Core 2.1.
manager: wpickett
monikerRange: = aspnetcore-2.1
ms.author: riande
ms.custom: mvc
ms.date: 05/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: aspnetcore-2.1
ms.openlocfilehash: 97c9f3d82264715358591a656dc4c1c5975b6113
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/02/2018
ms.locfileid: "34728944"
---
# <a name="whats-new-in-aspnet-core-21"></a>Co je nového v technologii ASP.NET Core 2.1

V tomto článku klade důraz nejvýznamnějších změn v ASP.NET Core 2.1, s odkazy na příslušné dokumentaci.

## <a name="signalr"></a>SignalR

SignalR jsou přepsána pro ASP.NET Core 2.1. Jádro ASP.NET SignalR zahrnuje množství vylepšení:

* Zjednodušený model Škálováním na více systémů.
* Nový klient JavaScript se žádné jQuery závislostí.
* Nový kompaktní binární protokol podle MessagePack.
* Podpora pro vlastní protokoly.
* Nový model datových proudů odpovědi.
* Podpora pro klienty založené na úplné objekty WebSockets.

Další informace najdete v tématu [ASP.NET Core SignalR](xref:signalr/index).

## <a name="razor-class-libraries"></a>Knihovny tříd Razor

ASP.NET Core 2.1 usnadňuje sestavení a zahrnout Razor na základě uživatelského rozhraní v knihovně a sdílet mezi více projektů. Nové Razor SDK umožňuje vytvářet soubory Razor do projektu knihovny tříd, které můžete zabalené do balíčku NuGet. Zobrazení a stránky v knihovnách se automaticky zjistí a aplikace se dá přepsat. Díky integraci Razor kompilace do sestavení:

* Doba spuštění aplikace je výrazně rychlejší.
* Rychlé aktualizace zobrazení syntaxe Razor a stránky v době běhu jsou stále k dispozici jako součást pracovního postupu iterativní vývoj.

Další informace najdete v tématu [vytvářet opakovaně použitelné uživatelské rozhraní pomocí projektu knihovny tříd Razor](xref:mvc/razor-pages/ui-class).

## <a name="identity-ui-library--scaffolding"></a>Knihovna uživatelského rozhraní identity & generování uživatelského rozhraní

Poskytuje ASP.NET Core 2.1 [ASP.NET Core Identity](xref:security/authentication/identity) jako [knihovny tříd Razor](xref:mvc/razor-pages/ui-class). Aplikace, které zahrnují Identity můžete použít nové Identity scaffolder selektivně přidat zdrojový kód obsažené v Identity Razor třídu knihovny (RCL). Můžete chtít generovat zdrojový kód, abyste mohli upravit kód a změnit chování. Například může vyzvat scaffolder ke generování kódu použít v registraci. Generovaného kódu mají přednost před stejný kód v Identity RCL.

Aplikace, které provádějí **není** zahrnují ověřování můžete použít scaffolder Identity k přidání balíčku RCL Identity. Máte možnost výběru Identity kódu má být vygenerován.

Další informace najdete v tématu [Identity vygenerované uživatelské rozhraní v projektů ASP.NET Core](xref:security/authentication/scaffold-identity).

## <a name="https"></a>HTTPS

S vyšší zaměřit na zabezpečení a ochrana osobních údajů je důležité povolit HTTPS pro webové aplikace. Vynucení HTTPS se stává stále striktní na webu. Servery, které nepoužívají HTTPS, jsou považovány za nezabezpečené. K vynucení, že webových funkcí produktu musí používat z kontextu zabezpečení jsou spouštění prohlížeče (chromu, Mozilla). [GDPR](xref:security/gdpr) vyžaduje použití protokolu HTTPS k ochrany osobních údajů uživatele. Při použití protokolu HTTPS v produkčním prostředí je rozhodující, pomocí protokolu HTTPS v vývoj pomáhají zabránit problémům v nasazení (například nezabezpečené odkazy). ASP.NET Core 2.1 zahrnuje množství vylepšení, které usnadňují používání protokolu HTTPS v vývoj a konfigurace protokolu HTTPS v produkčním prostředí. Další informace najdete v tématu [vynutit HTTPS](xref:security/enforcing-ssl).

### <a name="on-by-default"></a>Na ve výchozím nastavení

Usnadňuje vývoj zabezpečeného webu HTTPS nyní ve výchozím nastavení zapnutá. Od verze 2.1, Kestrel naslouchá na `https://localhost:5001` při místní vývojový certifikát nachází. Vývojový certifikát se vytvoří:

* V rámci prostředí prvního spuštění .NET Core SDK, při použití sady SDK pro první.
* Ručně pomocí nové `dev-certs` nástroj.

Spustit `dotnet dev-certs https --trust` certifikátu důvěřovat.

### <a name="https-redirection-and-enforcement"></a>Přesměrování protokolu HTTPS a vynucení

Webové aplikace obvykle nutné naslouchání protokolu HTTP a HTTPS, ale pak přesměruje veškerý provoz protokolu HTTP na HTTPS. V 2.1 specializované middleware přesměrování protokolu HTTPS, který inteligentně přesměruje na základě přítomnosti konfigurace nebo je zavedený porty vázané serveru.

Použití protokolu HTTPS lze dále vynutit pomocí [HTTP striktní přenosu zabezpečení protokolu (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts). HSTS dá pokyn prohlížeče vždy přístup k webu přes HTTPS. ASP.NET Core 2.1 přidá HSTS middleware, který podporuje možnosti pro maximální stáří, subdomény, a HSTS přednačtení seznamu.

### <a name="configuration-for-production"></a>Konfigurace pro produkční prostředí

V produkčním prostředí musí být explicitně nakonfigurovaný protokol HTTPS. V 2.1 výchozí schéma konfigurace pro konfiguraci protokolu HTTPS pro Kestrel přidala. Aplikace může být nakonfigurován k používání:

* Víc koncových bodů včetně adres URL. Další informace najdete v tématu [Kestrel webového serveru implementace: konfigurace koncového bodu](xref:fundamentals/servers/kestrel#endpoint-configuration).
* Certifikát, který se má použít pro protokol HTTPS ze souboru na disku nebo z úložiště certifikátů.

## <a name="gdpr"></a>GDPR

Základní technologie ASP.NET poskytuje rozhraní API a šablony, které vám pomůžou splňovat některé [Evropa obecné Data Protection nařízení (GDPR)](https://www.eugdpr.org/) požadavky. Další informace najdete v tématu [GDPR podporovat v ASP.NET Core](xref:security/gdpr). A [ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ukazuje způsob použití a umožňuje testovat většinu GDPR rozšíření bodů a přidat do šablony ASP.NET Core 2.1 rozhraní API.

## <a name="integration-tests"></a>Integrace testů

Nový balíček uvádíme, že test zjednodušuje vytváření a spouštění. [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/) balíček zpracovává následující úlohy:

* Zkopíruje soubor závislosti (*\*.deps*) z otestované aplikace do testovacího projektu *bin* složky.
* Nastaví kořenu obsahu na kořenového projektu otestované aplikace, aby statické soubory a stránek/zobrazení se našly při provádění testů.
* Poskytuje [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) třída pro zjednodušení zavádění otestované aplikace s [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).

Tento test používá [xUnit](https://xunit.github.io/) zkontrolujte, že indexovou stránku načte se stavovým kódem úspěch a s správnou hlavičku Content-Type:

```csharp
public class BasicTests
    : IClassFixture<WebApplicationFactory<RazorPagesProject.Startup>>
{
    private readonly HttpClient _client;

    public BasicTests(WebApplicationFactory<RazorPagesProject.Startup> factory)
    {
        _client = factory.CreateClient();
    }

    [Fact]
    public async Task GetHomePage()
    {
        // Act
        var response = await _client.GetAsync("/");

        // Assert
        response.EnsureSuccessStatusCode(); // Status Code 200-299
        Assert.Equal("text/html; charset=utf-8",
            response.Content.Headers.ContentType.ToString());
    }
}
```

Další informace najdete v tématu [integrace testy](xref:test/integration-tests) tématu.

## <a name="apicontroller-actionresult"></a>[Objektu ApiController], ActionResult

ASP.NET Core 2.1 přidá nový programovací konvence, které usnadňují sestavení vyčištění a popisný webových rozhraní API. `ActionResult<T>` je k přidání nového typu umožňuje aplikacím vracet typ odpovědi nebo jiné akce výsledek (podobně jako IActionResult), při stále označující typ odpovědi. `[ApiController]` Atribut přidala také jako způsob, jak můžete vyjádřit výslovný souhlas konvence specifické pro webové rozhraní API a chování.

Další informace najdete v tématu [sestavení webového rozhraní API pomocí ASP.NET Core](xref:web-api/index).

## <a name="ihttpclientfactory"></a>IHttpClientFactory

ASP.NET Core 2.1 obsahuje novou `IHttpClientFactory` služba, která usnadňuje konfigurace a využívat instancí `HttpClient` v aplikacích. `HttpClient` již obsahuje koncept delegování obslužných rutin, které může být propojena pro odchozí požadavky HTTP. Objekt factory:

* Díky registrace instancí `HttpClient` za intuitivnější pojmenované klienta.
* Implementuje Polly obslužná rutina, která umožňuje Polly zásady, které se používají pro opakování, CircuitBreakers atd.

Další informace najdete v tématu [zahájit požadavků HTTP](xref:fundamentals/http-requests).

## <a name="kestrel-transport-configuration"></a>Konfigurace kestrel přenosu

Ve verzi ASP.NET Core 2.1 Kestrel na výchozí přenos je už podle Libuv ale na spravované sokety. Další informace najdete v tématu [Kestrel webového serveru implementace: Konfigurace přenosu](xref:fundamentals/servers/kestrel#transport-configuration).

## <a name="generic-host-builder"></a>Tvůrce obecné hostitele

Obecný Tvůrce hostitele (`HostBuilder`) byla zavedena. Tohoto Tvůrce lze použít pro aplikace, které nejsou zpracování požadavků HTTP (zasílání zpráv, úlohy na pozadí, atd.).

Další informace najdete v tématu [obecné hostitele .NET](xref:fundamentals/host/generic-host).

## <a name="updated-spa-templates"></a>Aktualizované šablony SPA

Šablony jedné stránky aplikace pro úhlová reagují, a reagují s Redux se aktualizují používat struktury standardní projektu a sestavení systémy pro každou platformu.

Úhlová šablona je založená na úhlová rozhraní příkazového řádku a reagují šablony jsou založeny na vytvořit aplikaci reagují.
Další informace najdete v tématu [šablony jedné stránky aplikace pomocí ASP.NET Core](xref:spa/index).

## <a name="razor-pages-search-for-razor-assets"></a>Stránky Razor vyhledejte prostředky Razor

V 2.1 vyhledejte stránky Razor Razor prostředky (například rozložení a částečné.) v následujících adresářích v uvedeném pořadí:

1. Aktuální stránky složce.
1. *Řetězec/Pages/sdílené /*
1. */Views/sdílené /*

## <a name="razor-pages-in-an-area"></a>Stránky Razor v oblasti.

Teď podporují řešení stránky Razor [oblasti](xref:mvc/controllers/areas). Pokud chcete zobrazit příklad oblastech, vytvořte novou webovou aplikaci stránky Razor s jednotlivých uživatelských účtů. Stránky Razor webové aplikace s jednotlivých uživatelských účtů zahrnuje */Areas/Identity/Pages*.

## <a name="migrate-from-20-to-21"></a>Migrace z 2.0 na 2.1

V tématu [migraci ze základní technologie ASP.NET 2.0 na 2.1](xref:migration/20_21).

## <a name="additional-information"></a>Další informace

Úplný seznam změn, najdete v článku [poznámky k verzi ASP.NET Core 2.1](https://github.com/aspnet/Home/releases/tag/2.1.0).
