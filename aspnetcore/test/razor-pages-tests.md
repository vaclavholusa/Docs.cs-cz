---
title: Testování částí stránky Razor v ASP.NET Core
author: guardrex
description: Naučte se vytvářet testy částí pro stránky Razor aplikace.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: test/razor-pages-tests
ms.openlocfilehash: df74d8e44b2dff00e76139edba47fd8a30ce33ef
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252302"
---
# <a name="razor-pages-unit-tests-in-aspnet-core"></a>Testování částí stránky Razor v ASP.NET Core

Podle [Luke Latham](https://github.com/guardrex)

Jádro ASP.NET podporuje testování částí aplikací pro stránky Razor. Testy dat přístup layer (DAL) a zajistíte modely stránky:

* Části stránky Razor aplikace fungovat nezávisle a společně jako jednotku během vytváření aplikace.
* Třídy a metody mají omezenou obory zodpovědnosti.
* Další dokumentaci existuje na chování aplikace.
* Regresí, které jsou chyby způsobené aktualizace kód, nebyly nalezeny během automatického vytváření a nasazení.

Toto téma předpokládá, že máte základní znalosti a testování částí aplikací stránky Razor. Pokud jste obeznámeni s stránky Razor aplikace nebo testovací koncepty, najdete v následujících tématech:

* [Úvod do stránky Razor](xref:mvc/razor-pages/index)
* [Začínáme se stránkami Razor](xref:tutorials/razor-pages/razor-pages-start)
* [Testování C# v .NET Core pomocí testovacích dotnet a xUnit částí](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))

Ukázkový projekt se skládá ze dvou aplikací:

| Aplikace         | Složky projektu                        | Popis |
| ----------- | ------------------------------------- | ----------- |
| Zprávy aplikace | *src/RazorPagesTestSample*            | Umožňuje uživateli přidat, odstraňte jednu, odstranit všechny a analyzovat zprávy. |
| Testování aplikace    | *tests/RazorPagesTestSample.Tests*    | Použít k testování částí aplikace zprávu: layer (DAL) a Index stránky model přístup k datům. |

Testy můžete spustit pomocí funkce integrované testovací IDE, jako třeba [Visual Studio](https://www.visualstudio.com/vs/). Pokud používáte [Visual Studio Code](https://code.visualstudio.com/) nebo příkazového řádku, spusťte následující příkaz na příkazovém řádku v *tests/RazorPagesTestSample.Tests* složky:

```console
dotnet test
```

## <a name="message-app-organization"></a>Zprávy aplikace organizace

Zprávy aplikace je jednoduchý systém stránky Razor zpráv s následujícími charakteristikami:

* Index stránky aplikace (*Pages/Index.cshtml* a *Pages/Index.cshtml.cs*) poskytuje uživatelské rozhraní a stránky model metody k řízení přidání, odstranění a analýzu zprávy (průměrné slov za zprávy) .
* Zpráva je popsán `Message` – třída (*Data/Message.cs*) s dvě vlastnosti: `Id` (klíč) a `Text` (zprávy). `Text` Vlastnost je nutné a omezena na 200 znaků.
* Zprávy jsou uloženy pomocí [databáze v paměti rozhraní Entity Framework](/ef/core/providers/in-memory/)&#8224;.
* Aplikace obsahuje vrstva přístupu k datům (DAL) v kontextu své databáze třídy `AppDbContext` (*Data/AppDbContext.cs*). DAL metody jsou označeny `virtual`, což umožňuje mocking metody pro použití v testech.
* Pokud se databáze nachází prázdný při spuštění aplikace, se inicializuje tři zprávy úložiště zpráv. Tyto *nasadí zprávy* se také používají v testech.

&#8224;V tématu EF [testu s InMemory](/ef/core/miscellaneous/testing/in-memory), vysvětluje, jak používat databázi v paměti pro testování pomocí Mstestu. Toto téma používá [xUnit](https://xunit.github.io/) test framework. Test koncepty a testovací implementace napříč jiného testovacího architektury jsou podobné, ale nejsou identické.

I když se aplikace nepoužívá [použitému vzoru](http://martinfowler.com/eaaCatalog/repository.html) a není příklad efektivní [pracovní jednotky (UoW) vzor](https://martinfowler.com/eaaCatalog/unitOfWork.html), stránky Razor podporuje tyto vzory vývoj. Další informace najdete v tématu [navrhování vrstvu trvalosti infrastruktury](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), [implementace úložiště a jednotky pracovních vzorů v aplikaci ASP.NET MVC](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application), a [testovací kontroler Logika](/aspnet/core/mvc/controllers/testing) (ukázka implementuje vzor úložiště).

## <a name="test-app-organization"></a>Testování aplikace organizace

Testování aplikace je konzolovou aplikaci uvnitř *tests/RazorPagesTestSample.Tests* složky.

| Složky aplikace testu | Popis |
| --------------- | ----------- |
| *UnitTests*     | <ul><li>*DataAccessLayerTest.cs* obsahuje testování částí pro DAL.</li><li>*IndexPageTests.cs* obsahuje testování částí modelu Index stránky.</li></ul> |
| *Nástroje*     | Obsahuje `TestingDbContextOptions` metodu použitou k vytvoření nové databáze kontextu možnosti pro každou testování částí DAL tak, aby se obnovení databáze do stavu standardních hodnot pro každý test. |

Testovací prostředí je [xUnit](https://xunit.github.io/). Objekt mocking framework [Moq](https://github.com/moq/moq4).

## <a name="unit-tests-of-the-data-access-layer-dal"></a>Testování částí dat přístup layer (DAL)

Zpráva k němu má aplikace DAL s čtyři metody, které jsou součástí `AppDbContext` – třída (*src/RazorPagesTestSample/Data/AppDbContext.cs*). Každá z metod má jedno nebo dvě testování částí v testování aplikace.

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

Problém s tímto přístupem je, že každý test obdrží databáze ve stavu, předchozím testu nacházela. To může být problém při pokusu o zápis testy atomic jednotek, které nekoliduje s navzájem. Chcete-li vynutit `AppDbContext` Pokud chcete použít pro každý test nový kontext databáze, zadejte `DbContextOptions` instanci, která je založena na nového poskytovatele služby. Testování aplikace ukazuje, jak to provést pomocí jeho `Utilities` třídy metoda `TestingDbContextOptions` (*tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

Pomocí `DbContextOptions` v jednotce DAL testy umožňuje spouštět atomicky s novou databází instance každého testu:

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

Například `DeleteMessageAsync` metoda odpovídá za odebrání do jedné zprávy identifikovaný jeho `Id` (*src/RazorPagesTestSample/Data/AppDbContext.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

Existují dva testy pro tuto metodu. Jeden test kontroluje, že metoda po zprávy se nachází v databázi odstraní zprávu. Ostatní metody testy, které databázi nezmění, pokud zpráva `Id` pro odstranění neexistuje. `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` Metoda jsou uvedeny níže:

[!code-csharp[](razor-pages-tests/samples_snapshot/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Nejprve metoda provádí uspořádat krok, kde probíhá příprava pro krok akce. Synchronizace replik indexů zprávy jsou získány a uchovávat v `seedMessages`. Synchronizace replik indexů zprávy se uloží do databáze. Zpráva s `Id` z `1` je nastaven pro odstranění. Když `DeleteMessageAsync` spuštění metody, očekávané zprávy by měly mít všechny zprávy s výjimkou toho s `Id` z `1`. `expectedMessages` Proměnná představuje tento očekávaný výsledek.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Metoda funguje: `DeleteMessageAsync` metoda spuštěna předávání v `recId` z `1`:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

Nakonec získá metodu `Messages` z kontextu a porovná ho do `expectedMessages` potvrzující, zda jsou si rovny dva:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

Chcete-li porovnat, dva `List<Message>` jsou stejné:

* Zprávy jsou seřazené podle `Id`.
* Zpráva páry jsou porovnávány na `Text` vlastnost.

Podobné metody test, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` zkontroluje výsledek pokusu o odstranění zprávu, která neexistuje. V takovém případě musí být roven skutečné zprávy po očekávané zprávy v databázi `DeleteMessageAsync` metoda spuštěna. Měla by existovat žádná změna databáze obsahu:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a>Testování částí modelu metod stránky

Další sadu testů jednotek je zodpovědná za testy metody modelu stránky. V aplikaci zprávy jsou součástí modely stránky indexu `IndexModel` třídy v *src/RazorPagesTestSample/Pages/Index.cshtml.cs*.

| Metoda modelu stránky | Funkce |
| ----------------- | -------- |
| `OnGetAsync` | Získá zprávy z DAL pomocí uživatelského rozhraní `GetMessagesAsync` metoda. |
| `OnPostAddMessageAsync` | Pokud `ModelState` je platný, vyvolá `AddMessageAsync` přidat zprávu do databáze. |
| `OnPostDeleteAllMessagesAsync` | Volání `DeleteAllMessagesAsync` odstranit všechny zprávy v databázi. |
| `OnPostDeleteMessageAsync` | Provede `DeleteMessageAsync` odstranění zprávy s `Id` zadaný. |
| `OnPostAnalyzeMessagesAsync` | Pokud jeden nebo více zpráv v databázi, vypočítá průměrné počty slov za zprávy. |

Model metody stránky se zkoušejí sedm testy `IndexPageTests` – třída (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*). Testy pomocí známých vzor uspořádat Assert akce. Tyto testy zaměřit se na:

* Určení, pokud metody, postupujte podle kroků správné chování při `ModelState` je neplatný.
* Potvrzení metody vytvoření správné `IActionResult`.
* Kontroluje, že jsou správně provedeny přiřazení hodnoty vlastnosti.

Tato skupina testů často model metody vrstvy DAL k vytvoření očekávaná data pro Act krok, kde je stránka modelu metoda spuštěna. Například `GetMessagesAsync` metodu `AppDbContext` je mocked k vytváření výstupu. Když metoda modelu stránky provede tuto metodu, model vrátí výsledek. Data nepřejde do stavu z databáze. Tím se vytvoří testovací předvídatelný a spolehlivé podmínky pro použití DAL v testech modelu stránky.

`OnGetAsync_PopulatesThePageModel_WithAListOfMessages` Testování ukazuje jak `GetMessagesAsync` metoda je mocked modelu stránky:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

Když `OnGetAsync` metoda se spustí v kroku akce, volá model stránky `GetMessagesAsync` metoda.

Jednotkové testování krok Application Compatibility Toolkit (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

`IndexPage` model stránky `OnGetAsync` – metoda (*src/RazorPagesTestSample/Pages/Index.cshtml.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

`GetMessagesAsync` Metoda v DAL nevrací výsledek pro toto volání metody. Mocked verzi metoda vrací výsledek.

V `Assert` krok, skutečný zprávy (`actualMessages`) jsou přiřazeny z `Messages` vlastnost modelu stránky. Kontrola typu také provádí, když jsou přiřazeny zprávy. Zprávy očekávaných a aktuálních jsou porovnávány na základě jejich `Text` vlastnosti. Vyhodnotí test, který dva `List<Message>` instance obsahují stejné zprávy.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

Jiné testy v této skupině vytvořit stránku objekty modelu, které zahrnují `DefaultHttpContext`, `ModelStateDictionary`, `ActionContext` k navázání `PageContext`, `ViewDataDictionary`a `PageContext`. Tyto jsou užitečné při provádění testů. Například zpráva aplikace vytváří `ModelState` chyba s `AddModelError` zkontroluje, jestli platná `PageResult` je vrácena, pokud `OnPostAddMessageAsync` se spustí:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a>Další zdroje

* [Testování C# v .NET Core pomocí testovacích dotnet a xUnit částí](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Testovací kontrolery](xref:mvc/controllers/testing)
* [Testování částí kódu](/visualstudio/test/unit-test-your-code) (Visual Studio)
* [Integrační testy](xref:test/integration-tests)
* [xUnit.net](https://xunit.github.io/)
* [Začínáme s xUnit.net (.NET Core/ASP.NET Core)](https://xunit.github.io/docs/getting-started-dotnet-core)
* [Moq](https://github.com/moq/moq4)
* [Moq rychlý start](https://github.com/Moq/moq4/wiki/Quickstart)
