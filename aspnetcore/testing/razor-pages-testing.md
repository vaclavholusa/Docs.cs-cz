---
title: "Jednotka stránky Razor testování a integrace v ASP.NET Core"
author: guardrex
description: "Naučte se vytvářet testy částí a integrace pro stránky Razor aplikace."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 11/27/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: testing/razor-pages-testing
ms.openlocfilehash: 1ecdf010f7c283a0a08b224d570a5bc5cdf536df
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/03/2018
---
# <a name="razor-pages-unit-and-integration-testing-in-aspnet-core"></a>Jednotka stránky Razor testování a integrace v ASP.NET Core

Podle [Luke Latham](https://github.com/guardrex)

Jádro ASP.NET podporuje jednotky a integrace testování aplikací pro stránky Razor. Testování vrstvou (DAL), stránka modely a integrované stránka součásti pomáhá zajistit:

* Části stránky Razor aplikace fungovat nezávisle a společně jako jednotku během vytváření aplikace.
* Třídy a metody mají omezenou obory zodpovědnosti.
* Další dokumentaci existuje na chování aplikace.
* Regresí, které jsou chyby způsobené aktualizace kód, nebyly nalezeny během automatického vytváření a nasazení.

Toto téma předpokládá, že máte základní znalosti o aplikace stránky Razor, testování částí a integrace testování. Pokud jste obeznámeni s stránky Razor aplikace nebo testování koncepty, najdete v následujících tématech:

* [Úvod do stránky Razor](xref:mvc/razor-pages/index)
* [Začínáme se stránkami Razor](xref:tutorials/razor-pages/razor-pages-start)
* [Testování C# v .NET Core pomocí testovacích dotnet a xUnit částí](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Testování integrace](xref:testing/integration-testing)

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/razor-pages-testing/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample))

Ukázkový projekt se skládá ze dvou aplikací:

| Aplikace         | Složky projektu                        | Popis |
| ----------- | ------------------------------------- | ----------- |
| Zprávy aplikace | *src/RazorPagesTestingSample*         | Umožňuje uživateli přidat, odstraňte jednu, odstranit všechny a analyzovat zprávy. |
| Testování aplikace    | *tests/RazorPagesTestingSample.Tests* | Použít k testování aplikace zprávu.<ul><li>Testování částí: vrstva přístupu k datům (DAL), modelu stránky indexu</li><li>Integrace testů: indexovou stránku</li></ul> |

Testy můžete spustit pomocí funkce integrované testování IDE, jako třeba [Visual Studio](https://www.visualstudio.com/vs/). Pokud používáte [Visual Studio Code](https://code.visualstudio.com/) nebo příkazového řádku, spusťte následující příkaz na příkazovém řádku v *tests/RazorPagesTestingSample.Tests* složky:

```console
dotnet test
```

## <a name="message-app-organization"></a>Zprávy aplikace organizace

Zprávy aplikace je jednoduchý systém stránky Razor zpráv s následujícími charakteristikami:

* Index stránky aplikace (*Pages/Index.cshtml* a *Pages/Index.cshtml.cs*) poskytuje uživatelské rozhraní a stránky model metody k řízení přidání, odstranění a analýzu zprávy (průměrné slov za zprávy) .
* Zpráva je popsán `Message` – třída (*Data/Message.cs*) s dvě vlastnosti: `Id` (klíč) a `Text` (zprávy). `Text` Vlastnost je nutné a omezena na 200 znaků.
* Zprávy jsou uloženy pomocí [databáze v paměti rozhraní Entity Framework](/ef/core/providers/in-memory/)&#8224;.
* Aplikace obsahuje vrstva přístupu k datům (DAL) v kontextu své databáze třídy `AppDbContext` (*Data/AppDbContext.cs*). DAL metody jsou označeny `virtual`, což umožňuje mocking metody pro použití v testech.
* Pokud se databáze nachází prázdný při spuštění aplikace, se inicializuje tři zprávy úložiště zpráv. Tyto *nasadí zprávy* se také používají při testování.

&#8224; V tématu EF [testování pomocí InMemory](/ef/core/miscellaneous/testing/in-memory), vysvětluje, jak používat databázi v paměti pro testování pomocí Mstestu. Toto téma používá [xUnit](https://xunit.github.io/) testování framework. Testování koncepce a testovací implementace mezi různé testování architektury jsou podobné, ale nejsou identické.

I když se aplikace nepoužívá [použitému vzoru](http://martinfowler.com/eaaCatalog/repository.html) a není příklad efektivní [pracovní jednotky (UoW) vzor](https://martinfowler.com/eaaCatalog/unitOfWork.html), stránky Razor podporuje tyto vzory vývoj. Další informace najdete v tématu [navrhování vrstvu trvalosti infrastruktury](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), [implementace úložiště a jednotky pracovních vzorů v aplikaci ASP.NET MVC](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application), a [testování Řadič logiku](/aspnet/core/mvc/controllers/testing) (ukázka implementuje vzor úložiště).

## <a name="test-app-organization"></a>Testování aplikace organizace

Testování aplikace je konzolovou aplikaci uvnitř *tests/RazorPagesTestingSample.Tests* složky:

| Složky aplikace testu    | Popis |
| ------------------ | ----------- |
| *IntegrationTests* | <ul><li>*IndexPageTest.cs* obsahuje integrace testy pro indexovou stránku.</li><li>*TestFixture.cs* vytvoří testovacího hostitele k testování aplikace zprávu.</li></ul> |
| *UnitTests*        | <ul><li>*DataAccessLayerTest.cs* obsahuje testování částí pro DAL.</li><li>*IndexPageTest.cs* obsahuje testování částí modelu Index stránky.</li></ul> |
| *Nástroje*        | *Utilities.cs* obsahuje:<ul><li>`TestingDbContextOptions`Metoda použitá k vytvoření nové databáze kontextu možnosti pro každou testování částí DAL tak, aby se obnovení databáze do stavu standardních hodnot pro každý test.</li><li>`GetRequestContentAsync`Metoda použitá k přípravě `HttpClient` a obsahu pro požadavky, které se odesílají do aplikace zprávu během testování integrace.</li></ul>

Testovací prostředí je [xUnit](https://xunit.github.io/). Objekt mocking framework [Moq](https://github.com/moq/moq4). Integrace se zkoušky podle [ASP.NET Core testovacího hostitele](xref:testing/integration-testing#the-test-host).

## <a name="unit-testing-the-data-access-layer-dal"></a>Vrstva přístupu k datům (DAL) testování částí

Zpráva k němu má aplikace DAL s čtyři metody, které jsou součástí `AppDbContext` – třída (*src/RazorPagesTestingSample/Data/AppDbContext.cs*). Každá z metod má jedno nebo dvě testování částí v testování aplikace.

| DAL – metoda               | Funkce                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | Získá `List<Message>` z databáze, seřazené podle `Text` vlastnost. |
| `AddMessageAsync`        | Přidá `Message` do databáze.                                          |
| `DeleteAllMessagesAsync` | Odstraní všechna `Message` záznamy z databáze.                           |
| `DeleteMessageAsync`     | Odstraní jeden `Message` z databáze pomocí `Id`.                      |

Testy jednotek DAL vyžadují [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) při vytváření nového `AppDbContext` pro každý test. Jeden ze způsobů vytvoření `DbContextOptions` pro každý test je použití [DbContextOptionsBuilder](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptionsbuilder):

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

Problém s tímto přístupem je, že každý test obdrží databáze ve stavu, předchozím testu nacházela. To může být problém při pokusu o zápis testy atomic jednotek, které nekoliduje s navzájem. Chcete-li vynutit `AppDbContext` Pokud chcete použít pro každý test nový kontext databáze, zadejte `DbContextOptions` instanci, která je založena na nového poskytovatele služby. Testování aplikace ukazuje, jak to provést pomocí jeho `Utilities` třídy metoda `TestingDbContextOptions` (*tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs*):

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs?name=snippet1)]

Pomocí `DbContextOptions` v jednotce DAL testy umožňuje každý test se spouští atomicky s instanci vytvoří nová databáze:

```csharp
using (var db = new AppDbContext(Utilities.TestingDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

Každá metoda testu v `DataAccessLayerTest` – třída (*UnitTests/DataAccessLayerTest.cs*) pracuje podle vzoru uspořádat Act Assert:

1. Uspořádat: Databáze je nakonfigurovaná pro test nebo je definován očekávaný výsledek.
1. AKT: Test se spustí.
1. Assert –: Kontrolní výrazy jsou vytvářeny k určení, pokud je výsledek testu úspěšné.

Například `DeleteMessageAsync` metoda odpovídá za odebrání do jedné zprávy identifikovaný jeho `Id` (*src/RazorPagesTestingSample/Data/AppDbContext.cs*):

[!code-csharp[Main](razor-pages-testing/sample/src/RazorPagesTestingSample/Data/AppDbContext.cs?name=snippet4)]

Existují dva testy pro tuto metodu. Jeden test kontroluje, že metoda po zprávy se nachází v databázi odstraní zprávu. Ostatní metody testy, které databázi nezmění, pokud zpráva `Id` pro odstranění neexistuje. `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` Metoda jsou uvedeny níže:

[!code-csharp[Main](razor-pages-testing/sample_snapshot/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Nejprve metoda provádí uspořádat krok, kde probíhá příprava pro krok akce. Synchronizace replik indexů zprávy jsou získány a uchovávat v `seedMessages`. Synchronizace replik indexů zprávy se uloží do databáze. Zpráva s `Id` z `1` je nastaven pro odstranění. Když `DeleteMessageAsync` spuštění metody, očekávané zprávy by měly mít všechny zprávy s výjimkou toho s `Id` z `1`. `expectedMessages` Proměnná představuje tento očekávaný výsledek.

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Metoda funguje: `DeleteMessageAsync` metoda spuštěna předávání v `recId` z `1`:

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

Nakonec získá metodu `Messages` z kontextu a porovná ho do `expectedMessages` potvrzující, zda jsou si rovny dva:

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

Chcete-li porovnat, dva `List<Message>` jsou stejné:

* Zprávy jsou seřazené podle `Id`.
* Zpráva páry jsou porovnávány na `Text` vlastnost.

Podobné metody test, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` zkontroluje výsledek pokusu o odstranění zprávu, která neexistuje. V takovém případě musí být roven skutečné zprávy po očekávané zprávy v databázi `DeleteMessageAsync` metoda spuštěna. Měla by existovat žádná změna databáze obsahu:

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-testing-the-page-model-methods"></a>Model metody stránky testování částí

Další sadu testů jednotek je zodpovědná za testování metody modelu stránky. V aplikaci zprávy jsou součástí modely stránky indexu `IndexModel` třídy v *src/RazorPagesTestingSample/Pages/Index.cshtml.cs*.

| Metoda modelu stránky | Funkce |
| ----------------- | -------- | 
| `OnGetAsync` | Získá zprávy z DAL pomocí uživatelského rozhraní `GetMessagesAsync` metoda. |
| `OnPostAddMessageAsync` | Pokud `ModelState` je platný, vyvolá `AddMessageAsync` přidat zprávu do databáze. | 
| `OnPostDeleteAllMessagesAsync` | Volání `DeleteAllMessagesAsync` odstranit všechny zprávy v databázi. |
| `OnPostDeleteMessageAsync` | Provede `DeleteMessageAsync` odstranění zprávy s `Id` zadaný. |
| `OnPostAnalyzeMessagesAsync` | Pokud jeden nebo více zpráv v databázi, vypočítá průměrné počty slov za zprávy. |

Model metody stránky se zkoušejí sedm testy `IndexPageTest` – třída (*tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs*). Testy pomocí známých vzor uspořádat Assert akce. Tyto testy zaměřit se na:

* Určení, pokud metody, postupujte podle kroků správné chování při `ModelState` je neplatný.
* Potvrzení metody vytvoření správné `IActionResult`.
* Kontroluje, že jsou správně provedeny přiřazení hodnoty vlastnosti.

Tato skupina testů často model metody vrstvy DAL k vytvoření očekávaná data pro Act krok, kde je stránka modelu metoda spuštěna. Například `GetMessagesAsync` metodu `AppDbContext` je mocked k vytváření výstupu. Když metoda modelu stránky provede tuto metodu, model vrátí výsledek. Data nepřejde do stavu z databáze. Tím se vytvoří testovací předvídatelný a spolehlivé podmínky pro použití DAL v testech modelu stránky.

`OnGetAsync_PopulatesThePageModel_WithAListOfMessages` Testování ukazuje jak `GetMessagesAsync` metoda je mocked modelu stránky:

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet1&highlight=3-4)]

Když `OnGetAsync` metoda se spustí v kroku akce, volá model stránky `GetMessagesAsync` metoda.

Jednotkové testování krok Application Compatibility Toolkit (*tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs*):

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet2)]

`IndexPage`model stránky `OnGetAsync` – metoda (*src/RazorPagesTestingSample/Pages/Index.cshtml.cs*):

[!code-csharp[Main](razor-pages-testing/sample/src/RazorPagesTestingSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

`GetMessagesAsync` Metoda v DAL nevrací výsledek pro toto volání metody. Mocked verzi metoda vrací výsledek.

V `Assert` krok, skutečný zprávy (`actualMessages`) jsou přiřazeny z `Messages` vlastnost modelu stránky. Kontrola typu také provádí, když jsou přiřazeny zprávy. Zprávy očekávaných a aktuálních jsou porovnávány na základě jejich `Text` vlastnosti. Vyhodnotí test, který dva `List<Message>` instance obsahují stejné zprávy.

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet3)]

Jiné testy v této skupině vytvořit stránku objekty modelu, které zahrnují `DefaultHttpContext`, `ModelStateDictionary`, `ActionContext` k navázání `PageContext`, `ViewDataDictionary`a `PageContext`. Tyto jsou užitečné při provádění testů. Například zpráva aplikace vytváří `ModelState` chyba s `AddModelError` zkontroluje, jestli platná `PageResult` je vrácena, pokud `OnPostAddMessageAsync` se spustí:

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="integration-testing-the-app"></a>Integrace testování aplikace

Integrace testy zaměřit na to, testování, které vzájemně spolupracují součásti aplikace. Integrace se zkoušky podle [ASP.NET Core testovacího hostitele](xref:testing/integration-testing#the-test-host). Úplné požadavků a odpovědí životního cyklu zpracování je testována. Tyto testy assert, že stránce produkuje správné stavový kód a `Location` záhlaví, pokud nastavení.

Integrační testování příklad z ukázky ověří výsledek požaduje indexovou stránku aplikace zprávu (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs?name=snippet1)]

Neexistuje žádný krok uspořádání. `GetAsync` Metoda je volána na `HttpClient` odeslat požadavek GET na koncový bod. Test vyhodnotí, že výsledkem je 200 – OK stavový kód.

Všechny požadavek POST do zprávy aplikace musí splňovat antiforgery zkontrolujte, zda je automaticky provedené aplikace [antiforgery systému ochrany dat](xref:security/data-protection/introduction). Chcete-li uspořádat pro požadavek POST test, musí být testování aplikace:

1. Vytvořte žádost pro stránku.
1. Analyzovat antiforgery soubor cookie a tokenu žádosti o ověření z odpovědi.
1. Zkontrolujte požadavek POST s antiforgery ověřování souborů cookie a žádosti o token na místě.

`Post_AddMessageHandler_ReturnsRedirectToRoot` Test metody:

* Připraví zprávu a `HttpClient`.
* Odešle požadavek POST do aplikace.
* Ověří, zda že je odpověď na přesměrování zpátky na indexovou stránku.

`Post_AddMessageHandler_ReturnsRedirectToRoot `metoda (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs?name=snippet2)]

`GetRequestContentAsync` Spravuje metoda nástroj Příprava klienta se antiforgery soubor cookie a tokenu žádosti o ověření. Všimněte si, jak metodu obdrží `IDictionary` který povoluje metodu volání testu předávat data pro požadavek na kódování společně s tokenem ověření požadavku (*tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs*):

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs?name=snippet2&highlight=1-2,8-9,29)]

Integrace testy můžete také předat aplikace k testování aplikace odpovědi chování chybná data. Zprávy aplikace omezuje zpráva délce až 200 znaků (*src/RazorPagesTestingSample/Data/Message.cs*):

[!code-csharp[Main](razor-pages-testing/sample/src/RazorPagesTestingSample/Data/Message.cs?name=snippet1&highlight=7)]

`Post_AddMessageHandler_ReturnsSuccess_WhenMessageTextTooLong` Testování `Message` explicitně předá v textu s 201 znaky "X". To vede `ModelState` chyby. V příspěvku nepřesměruje zpět na indexovou stránku. Vrátí hodnotu 200 OK s `null` `Location` záhlaví (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs?name=snippet3&highlight=7,16-17)]

## <a name="see-also"></a>Viz také

* [Testování C# v .NET Core pomocí testovacích dotnet a xUnit částí](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Testování integrace](xref:testing/integration-testing)
* [Testování kontrolerů](xref:mvc/controllers/testing)
* [Testování částí kódu](/visualstudio/test/unit-test-your-code) (Visual Studio)
* [xUnit.net](https://xunit.github.io/)
* [Začínáme s xUnit.net (.NET Core/ASP.NET Core)](https://xunit.github.io/docs/getting-started-dotnet-core)
* [Moq](https://github.com/moq/moq4)
* [Moq rychlý start](https://github.com/Moq/moq4/wiki/Quickstart)
