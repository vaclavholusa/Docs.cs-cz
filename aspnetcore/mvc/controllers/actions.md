---
title: Obslužná rutina požadavky řadiče v aplikaci ASP.NET MVC jádra
author: ardalis
description: ''
manager: wpickett
ms.author: riande
ms.date: 07/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/actions
ms.openlocfilehash: c2f37bc7999b4c4ccc985d25d2ef009954d8f3f0
ms.sourcegitcommit: 7d02ca5f5ddc2ca3eb0258fdd6996fbf538c129a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/03/2018
---
# <a name="handle-requests-with-controllers-in-aspnet-core-mvc"></a>Obslužná rutina požadavky řadiče v aplikaci ASP.NET MVC jádra

Podle [Steve Smith](https://ardalis.com/) a [Scott Addie](https://github.com/scottaddie)

Řadiče, akce a výsledky akce jsou základní součástí jak vývojářům vytvářet aplikace pomocí ASP.NET MVC jádra.

## <a name="what-is-a-controller"></a>Co je řadič?

Řadič se používá k definování a seskupení sady akcí. Akce (nebo *metoda akce*) je metoda na řadič, která zpracovává požadavky. Řadiče logicky seskupit podobné akce. Tato agregace akce umožňuje společné sady pravidel, například směrování, ukládání do mezipaměti a autorizace, které se má použít společně. Požadavky jsou namapované na akce prostřednictvím [směrování](xref:mvc/controllers/routing).

Podle konvence řadiče třídy:
* Jsou umístěné v kořenové úrovni projektu *řadiče* složky
* Dědit z `Microsoft.AspNetCore.Mvc.Controller`

Řadič je instantiable třída, ve kterém je aspoň jeden z následujících podmínek true:
* Název třídy je na konci "Controller"
* Třídy dědí z třídy, jejíž název je na konci "Controller"
* Třída je upraven pomocí `[Controller]` atribut

Třída kontroleru nesmí mít přiřazený `[NonController]` atribut.

Postupujte podle řadiče [explicitní závislosti Princip](http://deviq.com/explicit-dependencies-principle/). Existuje několik přístupů k implementaci této zásady. Pokud více akce kontroleru vyžadují stejnou službu, zvažte použití [konstruktor vkládání](xref:mvc/controllers/dependency-injection#constructor-injection) požádat o těchto závislostí. Pokud služba je nutná metodou jenom jednu akci, zvažte použití [akce vkládání](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) požádat o závislost.

V rámci **M**odelu -**V**obrazit -**C**ontroller vzor řadič je odpovědná za počáteční zpracování žádosti a vytvoření instance modelu. Obecně platí obchodní rozhodnutí, která je třeba provést v rámci modelu.

Řadičem trvá výsledek zpracování modelu (pokud existuje) a vrátí správné zobrazení a jeho přidružené zobrazení dat nebo výsledek volání rozhraní API. Další informace na [přehled ASP.NET Core MVC](xref:mvc/overview) a [Začínáme s ASP.NET MVC jádra a sady Visual Studio](xref:tutorials/first-mvc-app/start-mvc).

Je řadič *uživatelského rozhraní úrovni* abstrakce. Jsou jeho zodpovědnosti zajistit data požadavku jsou platné a vybrat, které zobrazení (nebo výsledek rozhraní API) má být vrácen. V dobře započítané aplikace neobsahuje přímo data přístupu nebo obchodní logiku. Místo toho řadičem deleguje zpracování těchto odpovědnosti Services.

## <a name="defining-actions"></a>Definování akcí

Veřejné metody na řadič, s výjimkou označených pomocí `[NonAction]` atribut, akce. Parametry akce je vázána na data žádosti a se ověřují pomocí [model vazby](xref:mvc/models/model-binding). Pro vše, co je model vázán dojde k ověření modelu. `ModelState.IsValid` Hodnota vlastnosti označuje, zda vazby modelu a ověření bylo úspěšné.

Metody akce by měl obsahovat logiku pro mapování požadavek na obchodní problém. Problémů obchodního charakteru by měl být obvykle reprezentován jako služby, které řadičem přistupuje prostřednictvím [vkládání závislostí](xref:mvc/controllers/dependency-injection). Akce se mapují pak výsledek akce obchodní do stavu aplikace.

Akce vrátí nic, ale často vrátit instanci třídy `IActionResult` (nebo `Task<IActionResult>` pro asynchronní metody), která vyvolává odpověď. Metoda akce odpovídá pro výběr *jaký druh odpovědi*. Výsledek akce *nemá neodpovídá*.

### <a name="controller-helper-methods"></a>Metody Helper kontroleru

Řadiče obvykle dědí [řadič](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller), i když to není nutné. Odvozování z `Controller` poskytuje přístup k tří kategorií pomocné metody:

#### <a name="1-methods-resulting-in-an-empty-response-body"></a>1. Metody, což vede prázdný odpovědi

Ne `Content-Type` hlavičku HTTP odpovědi je zahrnuté, protože text odpovědi chybí obsah k popisu.

Existují dva typy výsledků v rámci této kategorie: přesměrování a stavový kód HTTP.

* **Kód stavu HTTP**

    Tento typ vrátí stavový kód HTTP. Několik pomocné metody tohoto typu jsou `BadRequest`, `NotFound`, a `Ok`. Například `return BadRequest();` vytváří 400 stavový kód při spuštění. Když metody, jako `BadRequest`, `NotFound`, a `Ok` jsou přetížený, už kvalifikují jako respondéry stavový kód protokolu HTTP, protože probíhající vyjednávání obsahu.

* **Redirect**

    Tento typ vrátí přesměrování na akci nebo cílové (pomocí `Redirect`, `LocalRedirect`, `RedirectToAction`, nebo `RedirectToRoute`). Například `return RedirectToAction("Complete", new {id = 123});` přesměruje na `Complete`, předá anonymní objekt.

    Typ výsledku přesměrování se liší od typu stavový kód HTTP především v přidání `Location` hlavičku HTTP odpovědi.

#### <a name="2-methods-resulting-in-a-non-empty-response-body-with-a-predefined-content-type"></a>2. Metody, které jsou výsledkem neprázdný odpovědi s předdefinované typ obsahu.

Většina pomocné metody této kategorie patří `ContentType` vlastnosti, což umožňuje, abyste nastavili `Content-Type` hlavičku odpovědi k popisu text odpovědi.

Existují dva typy výsledků v rámci této kategorie: [zobrazení](xref:mvc/views/overview) a [formátu odpovědi](xref:web-api/advanced/formatting).

* **View**

    Tento typ vrací zobrazení, která používá model k vykreslení HTML. Například `return View(customer);` předá zobrazení pro datové vazby modelu.

* **Formátovaný odpovědi**

    Tento typ vrátí JSON nebo podobné formát exchange dat reprezentovat objekt konkrétním způsobem. Například `return Json(customer);` serializuje zadaný objekt do formátu JSON.
    
    Zahrnout další běžné metody tohoto typu `File`, `PhysicalFile`, a `VirtualFile`. Například `return PhysicalFile(customerFilePath, "text/xml");` vrátí popsaného souboru XML `Content-Type` hodnotu hlavičky odpovědi "text/xml".

#### <a name="3-methods-resulting-in-a-non-empty-response-body-formatted-in-a-content-type-negotiated-with-the-client"></a>3. Metody, které jsou výsledkem text odpovědi neprázdné hodnoty ve formátu v vyjedná se klient pro typ obsahu.

Tato kategorie se nazývá lépe **vyjednávání obsahu**. [Vyjednávání obsahu](xref:web-api/advanced/formatting#content-negotiation) platí vždy, když akce vrátí [ObjectResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.objectresult) typu nebo něco jiného než [IActionResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iactionresult) implementace. Akce, která vrátí jinou hodnotu než`IActionResult` implementace (například `object`) také vrátí hodnotu formátu odpovědi.

Některé metody helper tohoto typu zahrnují `BadRequest`, `CreatedAtRoute`, a `Ok`. Příklady těchto metod `return BadRequest(modelState);`, `return CreatedAtRoute("routename", values, newobject);`, a `return Ok(value);`, v uvedeném pořadí. Všimněte si, že `BadRequest` a `Ok` provedení vyjednávání obsahu pouze v případě, že je předaná hodnota; bez předávány hodnotu, místo toho slouží jako typy výsledků stavový kód HTTP. `CreatedAtRoute` Metody na druhé straně vždy provede vyjednávání obsahu od jeho přetížení všechny vyžadují předat hodnotu.

### <a name="cross-cutting-concerns"></a>Mezi vyjímání otázky

Aplikace obvykle sdílet části svých pracovních postupů. Mezi příklady patří aplikaci, která vyžaduje ověřování pro přístup k nákupní košík nebo aplikaci, která ukládá data na některé stránky do mezipaměti. Pokud chcete provést logiku před nebo po metody akce, použijte *filtru*. Pomocí [filtry](xref:mvc/controllers/filters) na otázky mezi vyjímání, můžete snížit duplikace, což jim umožní postupujte podle [nemáte opakujte sami (suchého) Princip](http://deviq.com/don-t-repeat-yourself/).

Většina filtrovat atributy, jako například `[Authorize]`, lze použít na úrovni kontroler nebo akce v závislosti na požadované úrovni členitosti.

Zpracování chyb a ukládání odpovědí do mezipaměti, jsou často mezi vyjímání otázky:
   * [Zpracování chyb](xref:mvc/controllers/filters#exception-filters)
   * [Ukládání odpovědí do mezipaměti](xref:performance/caching/response)

Mnoho mezi vyjímání otázky můžete ke zpracování pomocí filtrů nebo vlastní [middleware](xref:fundamentals/middleware/index).
