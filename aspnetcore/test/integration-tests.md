---
title: Integrace testů v ASP.NET Core
author: guardrex
description: Zjistěte, jak integrace testy ujistit, že součásti aplikace správně fungovat na úrovni infrastruktury, včetně databáze, systém souborů a sítě.
ms.author: riande
ms.custom: mvc
ms.date: 05/30/2018
uid: test/integration-tests
ms.openlocfilehash: 1895b06f1af9a9eb66c14aa5c7834497fc95d583
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277693"
---
# <a name="integration-tests-in-aspnet-core"></a>Integrace testů v ASP.NET Core

Podle [Luke Latham](https://github.com/guardrex) a [Steve Smith](https://ardalis.com/)

Integrace testy ujistit, že součásti aplikace správně fungovat na úrovni obsahující podpůrnou infrastrukturu aplikace, jako je například databáze, systém souborů a sítě. Jádro ASP.NET podporuje integraci testů částí unit test framework pomocí webového hostitele test a serveru test v paměti.

Toto téma předpokládá základní znalosti o testování částí. Je-li obeznámeni s testovací koncepty, najdete v článku [testování částí v .NET Core a .NET Standard](/dotnet/core/testing/) téma a jeho odkazovaný obsah.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))

Ukázková aplikace je aplikace stránky Razor a předpokládá základní znalosti o stránky Razor. Je-li obeznámeni s stránky Razor, najdete v následujících tématech:

* [Úvod do stránky Razor](xref:razor-pages/index)
* [Začínáme se stránkami Razor](xref:tutorials/razor-pages/razor-pages-start)
* [Testy jednotek stránek Razor](xref:test/razor-pages-tests)

## <a name="introduction-to-integration-tests"></a>Úvod do integrace testů

Integrace testy vyhodnocení součásti aplikace na úrovní širší než [testování částí](/dotnet/core/testing/). Testování částí se používá k testování izolované softwarové komponenty, jako je například metody jednotlivé třídy. Integrace testy potvrďte, že dvě nebo více součástí aplikace vzájemně spolupracují a poskytnout očekávaný výsledek, případně také všechny komponenty potřebné plně zpracovat požadavek.

Tyto testy širší se používají pro testování aplikace infrastruktury a celý framework, často včetně následující součásti:

* Databáze
* Systém souborů
* Síťová zařízení
* Kanál požadavků a odpovědí

Testování částí použijte vyrobeny tak, součásti, známé jako *fakes* nebo *model objektů*, místo součásti infrastruktury.

Testy jednotek, na rozdíl od integrace testů:

* Použijte skutečný součásti, které aplikace se používá v produkčním prostředí.
* Vyžadovat další kód a zpracování dat.
* Trvat delší dobu.

Proto Omezte použití integrace testy nejdůležitější scénářů infrastruktury. Pokud chování lze otestovat pomocí testování částí nebo test integrace, zvolte testování částí.

> [!TIP]
> Nemáte zápisu integrace testů pro všechny možné Permutace dat a souboru přístup s databází a systémy souborů. Bez ohledu na to, kolik umístí napříč aplikace komunikovat s databází a systémy souborů, cílených sadu pro čtení, zápisu, aktualizace a odstranění integrace testy obvykle podporují adekvátní testování databáze a součásti systému souborů. Použití jednotek testů pro běžné testy metoda logiky, které interakci s těchto součástí. Při testování částí používání infrastruktury fakes/mocks následek rychlejší spuštění testu.

> [!NOTE]
> Diskuze integrace testů, se často nazývá otestované projektu *systému testovaného*, nebo "SUT" pro krátké.

## <a name="aspnet-core-integration-tests"></a>ASP.NET Core integrace testů

Integrace testů v ASP.NET Core vyžaduje následující:

* Projekt testu se používá k obsahovat a spusťte testy. K testovacímu projektu obsahuje odkaz na otestované projekt ASP.NET Core, názvem *systému testovaného* (SUT). _"SUT" v tomto tématu slouží k odkazování na otestované aplikace._
* K testovacímu projektu vytvoří testovací web pro hostitele, který SUT a zpracovávat požadavky a odpovědi SUT pomocí testovacího serveru klienta.
* Test runner se používá k provedení testů a sestava výsledky testů.

Integrace testy podle posloupnost událostí, které zahrnují obvykle *uspořádat*, *Act*, a *Assert* kroky testů:

1. Je nakonfigurován SUT webového hostitele.
1. Testovací server klienta se vytvoří pro odesílání žádostí o aplikaci.
1. *Uspořádat* spouští test krok: testování aplikace připraví žádost.
1. *Act* spouští test krok: klient odešle žádost a obdrží odpověď.
1. *Assert* spouští test krok: *skutečné* odpověď je ověřená jako *předat* nebo *nezdaří* na základě *očekávání*  odpovědi.
1. Proces pokračuje, dokud se spustit všechny testy.
1. Výsledky testů jsou hlášeny.

Obvykle je testovacího hostitele webové nastavují různým způsobem, než hostitel normální webové aplikace pro test spustí. Například jiné databázi nebo jiné aplikace. nastavení se dají používat pro testy.

Součásti infrastruktury, například testovací webového hostitele a test v paměti serveru ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), poskytuje nebo spravovaná [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) balíčku. Použití tohoto balíčku zjednodušuje vytváření testů a provádění.

`Microsoft.AspNetCore.Mvc.Testing` Balíček zpracovává následující úlohy:

* Zkopíruje soubor závislosti (*\*.deps*) z SUT do testovacího projektu *bin* složky.
* Nastaví kořenu obsahu na kořenové projektu SUT tak, aby statické soubory stránky nebo zobrazení jsou a k dispozici po provedení testů.
* Poskytuje [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) třída pro zjednodušení zavádění SUT s `TestServer`.

[Testování částí](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) dokumentace popisuje, jak nastavit projektu a testovacímu spuštění testu, společně s podrobné pokyny o tom, jak spustit testy a doporučení, jak pro název testy a otestovat třídy.

> [!NOTE]
> Když vytváříte projekt testu pro aplikaci, oddělte testy jednotek z testů integrace do různých projektů. To pomáhá zajistit, že testování součásti infrastruktury nejsou ať už náhodně součástí testování částí. Oddělení testů jednotek a integrace také umožňuje řídit, přes které sada testů se spouští.

Není prakticky žádný rozdíl mezi konfigurace pro testy stránky Razor aplikací a aplikací MVC. Jediný rozdíl spočívá v tom, jak jsou pojmenované testy. V aplikaci pro stránky Razor, mají obvykle název testy koncových bodů stránky po třídy modelu stránky (například `IndexPageTests` k testování součást integrace pro indexovou stránku). V aplikaci MVC testy jsou obvykle uspořádané podle třídy controller a s názvem po řadiče testují (například `HomeControllerTests` k testování součást integrace pro domovskou řadič).

## <a name="test-app-prerequisites"></a>Testování požadavky aplikací

Musí být k testovacímu projektu:

* Mít odkaz na balíček pro [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/).
* Použití sady SDK webové v souboru projektu (`<Project Sdk="Microsoft.NET.Sdk.Web">`).

Tyto prerequesities si můžete prohlédnout ve [ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/). Zkontrolujte *tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj* souboru.

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>Základní testy s výchozím WebApplicationFactory

[WebApplicationFactory&lt;TEntryPoint&gt; ](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) se používá k vytvoření [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) pro testy integrace. `TEntryPoint` vstupní bod třídu SUT, je obvykle `Startup` třídy.

Implementace třídy test *přípojka – třída* rozhraní (`IClassFixture`) k označení třída obsahuje testy a zajištění instance sdílených objektů v testů ve třídě.

### <a name="basic-test-of-app-endpoints"></a>Základní test koncových bodů aplikace

Testování následující třídy, `BasicTests`, používá `WebApplicationFactory` bootstrap SUT a zadejte [HttpClient](/dotnet/api/system.net.http.httpclient) testovací metodu, `Get_EndpointsReturnSuccessAndCorrectContentType`. Metoda ověří, zda je úspěšné stavový kód odpovědi (stavové kódy v rozsahu 200 299) a `Content-Type` záhlaví je `text/html; charset=utf-8` pro několik stránky aplikace.

[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) vytvoří instanci `HttpClient` , automaticky postupuje podle přesměrování a zpracovává soubory cookie.

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

### <a name="test-a-secure-endpoint"></a>Testování zabezpečený koncový bod

Jiného testu v `BasicTests` třída zkontroluje, že koncový bod zabezpečené přesměruje neověřené uživatele na přihlašovací stránku aplikace.

V SUT `/SecurePage` stránka používá [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) konvenci, kterou chcete použít [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) na stránku. Další informace najdete v tématu [stránky Razor autorizace konvence](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

V `Get_SecurePageRequiresAnAuthenticatedUser` otestovat, [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) je nastaven tak, aby zakázala přesměrování nastavením [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) k `false`:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

Protože je zakázáno klienta podle přesměrování, můžete provést následující kontroly:

* Stavový kód vrácený SUT lze zkontrolovat proti očekávané [HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode) výsledek, není kód konečného stavu po přesměrování na přihlašovací stránku, kterou by [HttpStatusCode.OK](/dotnet/api/system.net.httpstatuscode).
* `Location` Hodnota hlavičky v hlavičky odpovědi se kontroluje k potvrzení, že je spuštěna s `http://localhost/Identity/Account/Login`, není posledním přihlášení stránky odpovědi, kde `Location` by být záhlaví.

Další informace o `WebApplicationFactoryClientOptions`, najdete v článku [možnosti klienta](#client-options) části.

## <a name="customize-webapplicationfactory"></a>Přizpůsobení WebApplicationFactory

Konfigurace hostitele webové lze vytvořit nezávisle na třídy testovací dědění ze `WebApplicationFactory` vytvořit jeden nebo více vlastních objektů Factory:

1. Dědit z `WebApplicationFactory` a přepsání [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost). [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) umožňuje konfiguraci kolekce služby s [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   Synchronizace replik indexů v databázi [ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) provádí `InitializeDbForTests` metoda. Metoda je popsána v [integrace testy ukázka: testování aplikace organizace](#test-app-organization) části.

2. Použít vlastní `CustomWebApplicationFactory` v testovací třídy. Následující příklad používá objekt factory v `IndexPageTests` třídy:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   Ukázková aplikace klient je nakonfigurován k zabránit `HttpClient` z následující přesměrování. Jak je popsáno v [testování koncový bod zabezpečené](#test-a-secure-endpoint) části, to umožňuje testy zkontrolujte výsledek první odpověď aplikace. První odpověď je přesměrování v mnoha tyto testy se `Location` záhlaví.

3. Typické testu používá `HttpClient` a pomocné metody pro zpracování požadavku a odpovědi:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

Každá žádost POST o SUT musí splňovat antiforgery zkontrolujte, zda je automaticky provedené aplikace [antiforgery systému ochrany dat](xref:security/data-protection/introduction). Chcete-li uspořádat pro požadavek POST test, musí být testování aplikace:

1. Vytvořte žádost pro stránku.
1. Analyzovat antiforgery soubor cookie a tokenu žádosti o ověření z odpovědi.
1. Zkontrolujte požadavek POST s antiforgery ověřování souborů cookie a žádosti o token na místě.

`SendAsync` Pomocné metody rozšíření (*Helpers/HttpClientExtensions.cs*) a `GetDocumentAsync` metodu helper (*Helpers/HtmlHelpers.cs*) v [ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/) použít [AngleSharp](https://anglesharp.github.io/) analyzátor pro zpracování antiforgery kontroly pomocí následujících metod:

* `GetDocumentAsync` &ndash; Přijme [objekt HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) a vrátí `IHtmlDocument`. `GetDocumentAsync` používá objekt factory, který připraví *virtuální odpovědi* založena na původní `HttpResponseMessage`. Další informace najdete v tématu [AngleSharp dokumentaci](https://github.com/AngleSharp/AngleSharp#documentation).
* `SendAsync` rozšiřující metody pro `HttpClient` tvoří [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) a volání [SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) mají být odeslány požadavky SUT. Přetížení pro `SendAsync` přijmout formuláře HTML (`IHtmlFormElement`) a následující:
  - Odeslání tlačítko formuláře (`IHtmlElement`)
  - Kolekce hodnot formuláře (`IEnumerable<KeyValuePair<string, string>>`)
  - Tlačítko Odeslat (`IHtmlElement`) a hodnot z formulářů (`IEnumerable<KeyValuePair<string, string>>`)

> [!NOTE]
> [AngleSharp](https://anglesharp.github.io/) je analýza knihovně použité pro účely ukázky v tomto tématu a ukázková aplikace třetích stran. AngleSharp není podporován nebo požadováno pro integraci testování aplikací ASP.NET Core. Další analyzátory lze použít, například [Html flexibility Pack (HAP)](http://html-agility-pack.net/). Další možností je napsat kód pro zpracování tokenu žádosti o ověření a antiforgery cookie antiforgery systému přímo.

## <a name="customize-the-client-with-withwebhostbuilder"></a>Vlastní nastavení klienta se WithWebHostBuilder

Pokud je potřeba v rámci zkušební metoda další konfiguraci [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) vytvoří novou `WebApplicationFactory` s [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) dále uzpůsobeny konfigurace.

`Post_DeleteMessageHandler_ReturnsRedirectToRoot` Testování metodu [ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) demonstruje použití `WithWebHostBuilder`. Tento test provádí pomocí aktivován odeslání formuláře v SUT odstranit záznam v databázi.

Protože jiné testování v `IndexPageTests` třída provádí operaci, která odstraňuje všechny záznamy v databázi a může spustit před `Post_DeleteMessageHandler_ReturnsRedirectToRoot` metoda, databáze se nasadí do této metody testovací zajistit, že je k dispozici pro SUT odstranit záznam. Výběr `deleteBtn1` tlačítko `messages` formuláře SUT se simuluje v požadavku SUT:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>Možnosti klienta

Následující tabulka uvádí výchozí [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) dostupné při vytváření `HttpClient` instance.

| Možnost | Popis | Výchozí |
| ------ | ----------- | ------- |
| [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | Získá nebo nastaví, jestli `HttpClient` instancí automaticky postupujte přesměrování odpovědi. | `true` |
| [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | Získá nebo nastaví základní adresu `HttpClient` instance. | `http://localhost` |
| [HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | Získá nebo nastaví zda `HttpClient` instance by měla řídit tyto soubory cookie. | `true` |
| [MaxAutomaticRedirections](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | Získá nebo nastaví maximální počet přesměrování odpovědí, který `HttpClient` byste měli postupovat podle instance. | 7 |

Vytvořte `WebApplicationFactoryClientOptions` třídy a předejte jej [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) – metoda (výchozí hodnoty jsou zobrazeny v příkladu kódu):

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>Jak infrastruktury testovací odvodí obsahu kořenová cesta aplikace

`WebApplicationFactory` Konstruktor odvodí, že obsah kořenové cestě aplikace tak, že [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) na sestavení obsahujícího testy integrace s klíčem rovna `TEntryPoint` sestavení `System.Reflection.Assembly.FullName`. V případě, že nebyl nalezen atribut se správným klíčem, `WebApplicationFactory` spadne zpět na vyhledat soubor řešení (*\*.sln*) a připojí `TEntryPoint` název sestavení do adresáře řešení. Kořenový adresář aplikace (obsahu kořenovou cestu) se používá ke zjišťování zobrazení a soubory obsahu.

Ve většině případů není nutné explicitně nastavit obsahu kořenová aplikace jako logice vyhledávání obvykle vyhledá správné obsahu kořenové za běhu. Ve speciální scénářích, kde nebyl nalezen kořenu obsahu pomocí integrovaný vyhledávací algoritmus, obsahu, že kořenový mohou být zadané explicitně nebo pomocí vlastní logiky aplikace. Pokud chcete nastavit kořenový obsah aplikace v těchto scénářích, volání `UseSolutionRelativeContentRoot` metoda rozšíření z [Microsoft.AspNetCore.TestHost](https://www.nuget.org/packages/Microsoft.AspNetCore.TestHost) balíčku. Zadejte relativní cestu na řešení a volitelné řešení vzor názvu nebo glob souborů (výchozí = `*.sln`).

Volání [UseSolutionRelativeContentRoot](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.usesolutionrelativecontentroot) pomocí rozšíření metoda *jeden* z následujících postupů:

* Při konfiguraci testovací tříd pomocí `WebApplicationFactory`, zadejte vlastní konfigurace s [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder):

   ```csharp
   public IndexPageTests(
       WebApplicationFactory<RazorPagesProject.Startup> factory)
   {
       var _factory = factory.WithWebHostBuilder(builder =>
       {
           builder.UseSolutionRelativeContentRoot("<SOLUTION-RELATIVE-PATH>");

           ...
       });
   }
   ```

* Při konfiguraci testovací třídy s vlastní `WebApplicationFactory`, dědí `WebApplicationFactory` a přepsání [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost):

   ```csharp
   public class CustomWebApplicationFactory<TStartup>
       : WebApplicationFactory<RazorPagesProject.Startup>
   {
       protected override void ConfigureWebHost(IWebHostBuilder builder)
       {
           builder.ConfigureServices(services =>
           {
               builder.UseSolutionRelativeContentRoot("<SOLUTION-RELATIVE-PATH>");

               ...
           });
       }
   }
   ```

## <a name="disable-shadow-copying"></a>Zakázat stínové kopírování sestavení

Stínové kopírování sestavení způsobí, že testy provést do jiné složky než do výstupní složky. Pro testy fungovalo správně musí se zakázat stínové kopírování sestavení. [Ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) používá xUnit a zakáže stínové kopírování sestavení pro xUnit zahrnutím *xunit.runner.json* soubor s nastavením správnou konfiguraci. Další informace najdete v tématu [konfigurace xUnit.net s JSON](https://xunit.github.io/docs/configuring-with-json.html).

Přidat *xunit.runner.json* soubor pro kořenový testovacího projektu s následujícím obsahem:

```json
{
  "shadowCopy": false
}
```

## <a name="integration-tests-sample"></a>Ukázka integrace testů

[Ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) se skládá ze dvou aplikací:

| Aplikace | Složky projektu | Popis |
| --- | -------------- | ----------- |
| Zpráva aplikace (SUT) | *src/RazorPagesProject* | Umožňuje uživateli přidat, odstraňte jednu, odstranit všechny a analyzovat zprávy. |
| Testování aplikace | *tests/RazorPagesProject.Tests* | Slouží jako test integrace SUT. |

Testy můžete spustit pomocí funkce integrované testovací IDE, jako třeba [Visual Studio](https://www.visualstudio.com/vs/). Pokud používáte [Visual Studio Code](https://code.visualstudio.com/) nebo příkazového řádku, spusťte následující příkaz na příkazovém řádku v *tests/RazorPagesProject.Tests* složky:

```console
dotnet test
```

### <a name="message-app-sut-organization"></a>Zpráva aplikace (SUT) organizace

SUT je systém stránky Razor zpráv s následujícími charakteristikami:

* Index stránky aplikace (*Pages/Index.cshtml* a *Pages/Index.cshtml.cs*) poskytuje uživatelské rozhraní a stránky model metody k řízení přidání, odstranění a analýzu zprávy (průměrné slov za zprávy) .
* Zpráva je popsán `Message` – třída (*Data/Message.cs*) s dvě vlastnosti: `Id` (klíč) a `Text` (zprávy). `Text` Vlastnost je nutné a omezena na 200 znaků.
* Zprávy jsou uloženy pomocí [databáze v paměti rozhraní Entity Framework](/ef/core/providers/in-memory/)&#8224;.
* Aplikace obsahuje vrstva přístupu k datům (DAL) v kontextu své databáze třídy `AppDbContext` (*Data/AppDbContext.cs*).
* Pokud se databáze nachází prázdný při spuštění aplikace, se inicializuje tři zprávy úložiště zpráv.
* Zahrnuje aplikace `/SecurePage` , můžete přistupovat pouze ověřeného uživatele.

&#8224;V tématu EF [testu s InMemory](/ef/core/miscellaneous/testing/in-memory), vysvětluje, jak používat databázi v paměti pro testování pomocí Mstestu. Toto téma používá [xUnit](https://xunit.github.io/) test framework. Test koncepty a testovací implementace napříč jiného testovacího architektury jsou podobné, ale nejsou identické.

I když se aplikace nepoužívá [použitému vzoru](http://martinfowler.com/eaaCatalog/repository.html) a není příklad efektivní [pracovní jednotky (UoW) vzor](https://martinfowler.com/eaaCatalog/unitOfWork.html), stránky Razor podporuje tyto vzory vývoj. Další informace najdete v tématu [navrhování vrstvu trvalosti infrastruktury](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), [implementace úložiště a jednotky pracovních vzorů v aplikaci ASP.NET MVC](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application), a [testovací kontroler Logika](/aspnet/core/mvc/controllers/testing) (ukázka implementuje vzor úložiště).

### <a name="test-app-organization"></a>Testování aplikace organizace

Testování aplikace je konzolovou aplikaci uvnitř *tests/RazorPagesProject.Tests* složky.

| Složky aplikace testu | Popis |
| --------------- | ----------- |
| *BasicTests* | *BasicTests.cs* obsahuje testovací metod směrování, zabezpečení stránku neověřeného uživatele a získání profil uživatele Githubu a kontroly přihlášení profilu uživatele. |
| *IntegrationTests* | *IndexPageTests.cs* obsahuje integrace testy pro indexovou stránku pomocí vlastní `WebApplicationFactory` třídy. |
| *Pomocníci/nástroje* | <ul><li>*Utilities.cs* obsahuje `InitializeDbForTests` metodu použitou k naplnit databázi daty testu.</li><li>*HtmlHelpers.cs* poskytuje metodu pro návrat AngleSharp `IHtmlDocument` pro použití metody testu.</li><li>*HttpClientExtensions.cs* poskytují přetížení pro `SendAsync` mají být odeslány požadavky SUT.</li></ul> |

Testovací prostředí je [xUnit](https://xunit.github.io/). Integrace se zkoušky podle [Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost), což zahrnuje [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver). Protože [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) balíček slouží ke konfiguraci testovací server hostitele a testování `TestHost` a `TestServer` balíčky nevyžadují odkazů přímé balíčku v souboru projektu testování aplikace nebo Konfigurace vývojáře v testování aplikace.

**Synchronizace replik indexů databáze pro testování**

Integrace testy obvykle vyžadují na malou datovou sadu v databázi před spuštění testu. Například odstranění otestovat volání pro odstranění záznamů databáze, takže databáze musí mít alespoň jeden záznam pro žádost o odstranění úspěšné.

Ukázková aplikace doplňuje pro databázi s tří zpráv *Utilities.cs* , testů můžete použít při jejich spuštění:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

## <a name="additional-resources"></a>Další zdroje

* [Testy jednotek](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Testy jednotek stránek Razor](xref:test/razor-pages-tests)
* [Middleware](xref:fundamentals/middleware/index)
* [Testovací kontrolery](xref:mvc/controllers/testing)
