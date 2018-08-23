---
title: Testovací kontroler logiku v ASP.NET Core
author: ardalis
description: Zjistěte, jak otestovat logice kontroleru v ASP.NET Core s Moq a xUnit.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/controllers/testing
ms.openlocfilehash: d0b2a25d00187c088671be147844aa892f824c6e
ms.sourcegitcommit: 64c2ca86fff445944b155635918126165ee0f8aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2018
ms.locfileid: "41756745"
---
# <a name="test-controller-logic-in-aspnet-core"></a>Testovací kontroler logiku v ASP.NET Core

Podle [Steve Smith](https://ardalis.com/)

Kontrolery jsou klíčovou součást všech aplikací ASP.NET Core MVC. V důsledku toho byste měli mít jistotu, které se chovají se tak, jak má pro vaši aplikaci. Automatizované testy vám můžou poskytnout tento spolehlivosti a dokáže detekovat chyby dřív, než dorazí produkčního prostředí. Je důležité předejde zbytečné povinnosti v rámci vašich řadičů a zajistit vaše testy se zaměřují jen na kontroleru odpovědnosti.

Logice kontroleru by měl být minimální nelze zaměřit se na obchodní logiku nebo infrastrukturu otázky (například přístup k datům). Testovací kontroler logiku, není rozhraní. Test jak kontroler *chová* založené na vstupech platný nebo neplatný. Testovací kontroler odpovědi na základě výsledku obchodní operace, které provádí.

Odpovědnosti typické kontroleru:

* Ověřte `ModelState.IsValid`.
* Vrátí reakce na chybu, pokud `ModelState` je neplatný.
* Obchodní entity načtete z trvalého úložiště.
* Provedení akce na obchodní entitě.
* Uložte do trvalého obchodní entitě.
* Vrátí odpovídající `IActionResult`.

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="unit-tests-of-controller-logic"></a>Testy jednotek logice kontroleru

[Testy jednotek](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) zahrnovat testování částí aplikace v izolaci od jeho infrastrukturu a závislosti. Při testování logice kontroleru, jenom obsah jedné akce testování částí, není chování z jejich závislých nebo samotného rozhraní. Jako jednotku můžete testovat vaše akce kontroleru, ujistěte se, že se že zaměříte jenom na jeho chování. Řadič testu jednotek se vyhnete věci, jako je [filtry](xref:mvc/controllers/filters), [směrování](xref:fundamentals/routing), nebo [vazby modelu](xref:mvc/models/model-binding). Díky zaměření na testování pouze jednou z věcí, testování jednotek je obecně k zápisu snadné a rychlé spuštění. Kvalitně napsané sady testů jednotek můžete často spustit bez spojená tak velká režie. Však není částí detekovat problémy v interakci mezi součástmi, což je účelem [integrační testy](xref:test/integration-tests).

Pokud píšete vlastní filtry a tras, měli byste jednotky testování v izolaci, nikoli jako součást testy na určitý kontroler akce.

> [!TIP]
> [Vytváření a spouštění testování částí pomocí sady Visual Studio](/visualstudio/test/unit-test-your-code).

Abychom si předvedli testování částí, projděte si následující kontroler. Zobrazí seznam debaty relací a umožňuje nové debaty relace má být vytvořen s příspěvek:

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?highlight=12,16,21,42,43)]

Následující kontroler [explicitní závislosti Princip](http://deviq.com/explicit-dependencies-principle/), očekává se vkládání závislostí poskytnout instance `IBrainstormSessionRepository`. Díky tomu je poměrně snadné testovat pomocí mock objektu rozhraní, jako je třeba [Moq](https://www.nuget.org/packages/Moq/). `HTTP GET Index` Metoda nemá žádný nebo větvení a smyček pouze volání metod. Abyste to mohli otestovat `Index` metoda, potřebujeme si ověřit, která `ViewResult` se vrátí, s `ViewModel` z úložiště `List` – metoda.

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=17-18&range=1-33,76-95)]

`HomeController` `HTTP POST Index` – Metoda (popsaný výš) by měl ověřit:

* Metoda akce vrací chybnou žádost `ViewResult` s příslušnými daty při `ModelState.IsValid` je `false`.

* `Add` Volání metody v daném úložišti a `RedirectToActionResult` je vrácen s správné argumenty při `ModelState.IsValid` má hodnotu true.

Neplatný stav modelu můžete otestovat přidáním chyb s použitím `AddModelError` jak je znázorněno v následujícím prvního testu.

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=8,15-16,37-39&range=35-75)]

První test potvrdí při `ModelState` není platný, stejné `ViewResult` se vrátí jako `GET` požadavku. Všimněte si, že test nebude se pokoušet a zajistěte tak předání modelu je neplatný. Který nebude fungovat i přesto protože neběží vazby modelu (i když [test integrace](xref:test/integration-tests) byste použili vazby cvičení modelu). V takovém případě není právě testováno vazby modelu. Tyto testy jednotek jenom testujete, čemu kód v metodě akce.

Druhý test ověří, že `ModelState` platná, nový `BrainstormSession` přidá (prostřednictvím úložiště), a vrátí metodu `RedirectToActionResult` s očekávané vlastnosti. Imitaci volání, které nejsou volány jsou obvykle ignoruje, ale volání `Verifiable` na konci instalace volání díky tomu se dá ověřit v testu. To provedete pomocí volání `mockRepo.Verify`, který se test nezdaří, pokud nebyla volána metoda očekávané.

> [!NOTE]
> Knihovna Moq používané v tomto příkladu usnadňuje kombinovat mocks ověřitelný nebo "přísné", s neověřitelného mocks (také nazývané "dojde ke ztrátě" mocks nebo zástupné procedury). Další informace o [přizpůsobení chování model s využitím Moq](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior).

Jiné řadiče v aplikaci zobrazí informace týkající se konkrétní debaty. Obsahuje nějaké logiky se neplatné id hodnoty:

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?highlight=19,20,21,22,25,26,27,28)]

Akce kontroleru má tři případy otestovat, jeden pro každou `return` – příkaz:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?highlight=27,28,29,46,47,64,65,66,67,68)]

Aplikace zpřístupňuje funkce jako webové rozhraní API (seznam nápadů přidružené k relaci debaty a metoda pro přidání nové nápady týkající se k relaci):

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?highlight=21,22,27,30,31,32,33,34,35,36,41,42,46,52,65)]

`ForSession` Metoda vrátí seznam hodnot `IdeaDTO` typy. Vyhněte se vracení vaše obchodní entity domény přímo prostřednictvím volání rozhraní API, protože často zahrnují více dat, než klient rozhraní API vyžaduje, a se zbytečně spárovat model interní domény vaší aplikace pomocí rozhraní API můžete zveřejnit externě. Mapování mezi domény subjekty a typy, budete přesměrováni zpět při přenosu můžete provést ručně (pomocí LINQ `Select` jak je znázorněno zde) nebo pomocí knihovny jako [AutoMapper](https://github.com/AutoMapper/AutoMapper).

Pro testování částí `Create` a `ForSession` metody rozhraní API:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?highlight=18,23,29,33,38-39,43,50,58-59,68-70,76-78&range=1-83,121-135)]

Jak bylo uvedeno dříve, k otestování chování metody při `ModelState` je neplatný, přidejte chybu modelu do řadiče testu v rámci. Nepokoušejte testovací ověření nebo model vazby modelu v testech jednotky – testování chování metody akce, když čelí konkrétní `ModelState` hodnotu.

Druhý test závisí na úložiště, který vrací hodnotu null, takže mock úložiště je nakonfigurovaný k vrácení hodnoty null. Není nutné vytvořit testovací databáze (v paměti nebo jinak) a vytvořit dotaz, který vrátí tento výsledek - ji pak lze provést v jediném příkazu, jak je znázorněno.

Poslední test ověří, zda úložiště `Update` metoda je volána. Jako jsme to udělali dříve, model se nazývá s `Verifiable` a potom imitaci v úložišti `Verify` metoda je volána k potvrzení ověřitelné metody se spustil. Není jednotka testu odpovědnost zajistit, aby `Update` metoda uložit data; to jde udělat pomocí o test integrace.

## <a name="additional-resources"></a>Další zdroje

* <xref:test/integration-tests>
