---
title: "Testování řadiče logiku v ASP.NET Core"
author: ardalis
description: "Zjistěte, jak otestovat řadiče logiku v ASP.NET Core s Moq a xUnit."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/testing
ms.openlocfilehash: f27e7ec43cd17e249dd646a7dfbce5df69d59664
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="testing-controller-logic-in-aspnet-core"></a>Testování řadiče logiku v ASP.NET Core

Podle [Steve Smith](https://ardalis.com/)

Řadiče v aplikacích ASP.NET MVC musí být malé a zaměřují se na otázky uživatelského rozhraní. Velké řadiče, které pracují s obavy bez uživatelského rozhraní je složité testování a údržbu.

[Zobrazení nebo stažení ukázky z webu GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample)

## <a name="testing-controllers"></a>Testování řadiče

Řadiče jsou centrální součástí všech aplikací ASP.NET MVC jádra. Jako takový měli byste mít přitom jistotu, že chovají se jako určený pro vaši aplikaci. Automatizované testy vám může poskytnout tento spolehlivosti a chyby může zjistit, než dosáhnou produkční. Je důležité předejde nepotřebné odpovědnosti v rámci své řadiče a zkontrolovat vaše testy se zaměřují jen na odpovědnosti řadiče.

Řadič logiku by měl být minimální a nesmí být zaměřené na obchodní logiku nebo infrastrukturu obavy (například přístup k datům). Otestujte řadič logiku, není rozhraní. Test jak řadičem *chová* podle vstupy platný nebo neplatný. Otestujte řadič odpovědí na základě výsledku operace firmy, které provádí.

Typické řadiče zodpovědnosti:

* Ověřte `ModelState.IsValid`.
* Vrátí odpověď chyby, pokud `ModelState` je neplatný.
* Načtení entity obchodní z trvalost.
* Provedení akce v obchodní entity.
* Uložte obchodní entity do trvalost.
* Vrátí odpovídající `IActionResult`.

## <a name="unit-testing"></a>Testování částí

[Testování částí](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) zahrnuje testování součástí aplikace izolovaně od jeho infrastruktury a závislosti. Při testování řadiče logiku, obsah jenom jednu akci testování částí, není chování jeho závislé součásti nebo rozhraní sám sebe. Jako jednotku můžete otestovat vaše akce kontroleru, ujistěte se, že byste se zaměřit jenom na své chování. Testování částí řadiče zabraňuje třeba [filtry](filters.md), [směrování](../../fundamentals/routing.md), nebo [model vazby](../models/model-binding.md). Se zaměříte na testování právě jednou z věcí, testy jednotek jsou obecně jednoduché k zápisu a rychlé spuštění. Kvalitně sadu testů jednotek se může spouštět často bez mnoho zásahů. Testy jednotek však není rozpoznat problémy v interakci mezi součástmi, což je účelem [testování integrace](xref:mvc/controllers/testing#integration-testing).

Pokud píšete vlastní filtry, tras atd., měli byste testování částí je, ale nikoli jako součást testy na určitý kontroler akce. Musí být testovány v izolaci.

> [!TIP]
> [Vytvoření a spuštění testů jednotek pomocí sady Visual Studio](https://docs.microsoft.com/visualstudio/test/unit-test-your-code).

K předvedení testování částí, zkontrolujte následující řadiče. Zobrazí seznam Debata relací a umožňuje nové Debata relací se vytvoří s příspěvku na:

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?highlight=12,16,21,42,43)]

Kontroleru je následující [explicitní závislosti Princip](http://deviq.com/explicit-dependencies-principle/), byla očekávána vkládání závislostí poskytnout instanci `IBrainstormSessionRepository`. Díky tomu je poměrně snadno testovat pomocí mock objektu rozhraní, jako je třeba [Moq](https://www.nuget.org/packages/Moq/). `HTTP GET Index` Metoda má žádné opakování nebo větvení a pouze volání jednu metodu. Abyste to mohli otestovat `Index` metoda, potřebujeme ověřit, jestli `ViewResult` se vrátí, s `ViewModel` z v úložišti `List` metoda.

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=17-18&range=1-33,76-95)]

`HomeController` `HTTP POST Index` By měl ověřit – metoda (viz výše):

* Metoda akce vrací chybný požadavek `ViewResult` s příslušná data při `ModelState.IsValid` je`false`

* `Add` Je volána metoda na úložiště a `RedirectToActionResult` je vrácen s správné argumenty při `ModelState.IsValid` hodnotu true.

Neplatný stav modelu může být testována přidáním chyb s použitím `AddModelError` jak je znázorněno níže prvního testu.

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=8,15-16,37-39&range=35-75)]

První test potvrdí, kdy `ModelState` není platný, stejné `ViewResult` se vrátí jako pro `GET` požadavku. Všimněte si, že test není pokusí předat ve model neplatný. Že nebude fungovat přesto vzhledem k tomu, že není spuštěna vazby modelu (i když [integrace testovací](xref:mvc/controllers/testing#integration-testing) využije vazby modelu cvičení). V takovém případě není testuje vazby modelu. Tyto testy jednotek jenom testujete, jaké jsou kód v metodě akce.

Druhý test ověřuje, že když `ModelState` je platný, nový `BrainstormSession` se přidá (prostřednictvím úložiště), a vrátí metodu `RedirectToActionResult` s očekávanou vlastností. Mocked volání, které nejsou názvem jsou obvykle ignorováno, ale volání `Verifiable` na konci instalace volání umožňuje ověřit v testu. To provedete pomocí volání `mockRepo.Verify`, který selže test, pokud nebyla volána metoda očekávané.

> [!NOTE]
> Knihovnou Moq používanou v této ukázce usnadňuje kombinovat ověřitelný nebo "striktní", mocks s nejsou ověřitelné mocks (také nazývané "přijít" mocks nebo zástupných procedur). Další informace o [přizpůsobení chování model s Moq](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior).

Jiný řadič v aplikaci zobrazí informace týkající se konkrétní Debata relace. Obsahuje některé logiku pro řeší neplatné id hodnoty:

[!code-csharp[Main](./testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?highlight=19,20,21,22,25,26,27,28)]

Akce kontroleru má tři případech chcete otestovat, jeden pro každou `return` příkaz:

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?highlight=27,28,29,46,47,64,65,66,67,68)]

Aplikace zpřístupňuje funkci, jako webové rozhraní API (seznam nápady přidružené Debata relace a metody pro přidání nových nápadů do relace):

<a name="ideas-controller"></a>

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?highlight=21,22,27,30,31,32,33,34,35,36,41,42,46,52,65)]

`ForSession` Metoda vrátí seznam hodnot `IdeaDTO` typy. Vyhněte se vrací entity domény vaší firmy přímo prostřednictvím volání rozhraní API, protože často obsahují více dat, než klient rozhraní API vyžaduje, a jejich zbytečně spojte modelu interní domény vaší aplikace s rozhraním API vystavit externě. Mapování mezi domény entity a typy, vrátíte se prostřednictvím sítě můžete provést ručně (pomocí LINQ `Select` jak je vidět tady) nebo pomocí knihovny jako [AutoMapper](https://github.com/AutoMapper/AutoMapper)

Jednotka testů pro `Create` a `ForSession` metody rozhraní API:

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?highlight=18,23,29,33,38-39,43,50,58-59,68-70,76-78&range=1-83,121-135)]

Jak už jsme si říkali, k testování chování metodu při `ModelState` je neplatný, přidejte k řadiči chybu modelu v rámci testu. Nepokoušejte se otestovat ověření nebo model vazby modelu v testů jednotek - právě testovací metody akce chování-li čelit konkrétní `ModelState` hodnotu.

Druhý test závisí na úložiště vrací hodnotu null, takže imitované úložiště je nakonfigurovaný k vrácení hodnoty null. Není nutné k vytvoření testovací databáze (v paměti nebo jinak) a sestavte dotaz, který vrátí tento výsledek – je možné ji provést v jediném příkazu, jak je vidět.

Poslední test ověřuje, že v úložišti `Update` metoda je volána. Jako jsme to udělali dřív, model je volán s `Verifiable` a pak mocked v úložišti `Verify` metoda je volána potvrďte ověřitelný metoda byla spuštěna. Není odpovědnost test jednotky zajistit, aby `Update` metoda uložit data; to můžete udělat pomocí test integrace.

## <a name="integration-testing"></a>Testování integrace

[Testování integrace](../../testing/integration-testing.md) slouží k zajištění samostatné moduly v práci aplikace správně společně. Obecně platí nic, co můžete otestovat s testů jednotek, můžete také otestovat s test integrace, ale naopak není pravda. Ale integrace testů jsou obvykle mnohem nižší než testování částí. Proto je nejvhodnější pro testování ať můžete pomocí jednotkových testů a použít integrace testy pro scénáře, které zahrnují více spolupracovníci.

I když může být stále užitečné, se v testech integrace zřídka používají mock objektů. V jednotce testování jsou mock objektů efektivní způsob, jak řídit chování spolupracovníci mimo jednotky během testování pro testovací účely. V test integrace skutečné spolupracovníci používají a ověřte, zda že celý subsystému společně správně funguje.

### <a name="application-state"></a>Stav aplikace

Jeden důležitý aspekt při provádění testování integrace spočívá v tom, jak nastavit stav vaší aplikace. Testy muset spustit nezávisle na sobě, a proto by se měl spustit všechny testy s aplikací v známého stavu. Pokud vaše aplikace není použít databázi nebo mít žádné trvalost, nemusí to být problém. Většinu aplikací reálného však zachovat jejich stavu pro nějaký druh úložiště dat, všechny změny provedené jeden test by mohlo mít vliv jiného testu, pokud se vynuluje úložiště dat. Pomocí integrovaných `TestServer`, je velmi jednoduchá do hostitele aplikací ASP.NET Core v testech integrace, ale který nutně neuděluje přístup k datům se bude používat. Pokud používáte skutečné databáze, jeden z přístupů je tak, aby měl aplikaci připojit k databázi test, testy můžete přístup a ujistěte se, je před každým testem resetovat do známého stavu.

V této ukázkové aplikaci používám podporu InMemoryDatabase Entity Framework Core, takže jen z nelze připojit k němu Moje testovacího projektu. Místo toho I vystavit `InitializeDatabase` metoda z aplikace `Startup` třídy, které můžu volat při spuštění aplikace, pokud se nachází v `Development` prostředí. Moje integrace testy automaticky těžit z tohoto tak dlouho, dokud se nastavit prostředí `Development`. Nemám starat o obnovení databáze, protože InMemoryDatabase se vynuluje pokaždé, když aplikace restartuje.

`Startup` Třídy:

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Startup.cs?highlight=19,20,34,35,43,52)]

Zobrazí se `GetTestSession` metoda často používají v testech integrace níže.

### <a name="accessing-views"></a>Přístup k zobrazení

Každá třída testovací integrace nakonfiguruje `TestServer` který se spustí aplikace ASP.NET Core. Ve výchozím nastavení `TestServer` hostuje webové aplikace ve složce, kde běží – v takovém případě složce projektu testu. Proto když zkusíte testovací akce kontroleru, které vracejí `ViewResult`, může se tato chyba:

```
The view 'Index' wasn't found. The following locations were searched:
(list of locations)
```

Chcete-li opravit tento problém, nakonfigurujte kořenového serveru obsahu, tak, aby mohl vyhledat umístění zobrazení pro projekt testuje. To se provádí volání `UseContentRoot` v `TestFixture` třída, vidíte níže:

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/TestFixture.cs?highlight=30,33)]

`TestFixture` Třída je zodpovědná za konfiguraci a vytváření `TestServer`, nastavuje se `HttpClient` ke komunikaci s `TestServer`. Každý integraci testy používá `Client` vlastnost pro připojení k serveru test a podání žádosti o.

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/HomeControllerTests.cs?highlight=20,26,29,30,31,35,38,39,40,41,44,47,48)]

V první testu výše `responseString` obsahuje skutečnou vykreslení HTML ze zobrazení, které mohly být zkontrolovány a ověřit tak obsahuje očekávané výsledky.

Druhý test vytvoří operací POST formuláře s názvem jedinečný relace a odešle ji do aplikace a pak ověřuje, že se vrátí očekávané přesměrování.

### <a name="api-methods"></a>Metody rozhraní API

Pokud vaše aplikace zpřístupní webové rozhraní API, jeho vhodné mít automatizovaných testů potvrďte, že provést podle očekávání. Integrované `TestServer` usnadňuje testování webových rozhraní API. Pokud vaše rozhraní API metody používají vazby modelu, byste měli vždy zkontrolovat `ModelState.IsValid`, a integrace testů jsou na správném místě potvrďte, že vaše ověření modelu funguje správně.

Následující sadu testů cíl `Create` metoda v [IdeasController](xref:mvc/controllers/testing#ideas-controller) třídy uvedené výše:

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/ApiIdeasControllerTests.cs)]

Na rozdíl od integrace testy akcí, které vrátí HTML zobrazení webového rozhraní API metody, které vracejí výsledky obvykle lze deserializovat jako objektů se silným typem, jak ukazuje poslední test výše. V takovém případě, test deserializuje výsledek, který má `BrainstormSession` instance a potvrdí, že na nápad správně přidaná do jeho kolekce návrhy.

V tomto článku najdete další příklady integrace testů [ukázkový projekt](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample).
