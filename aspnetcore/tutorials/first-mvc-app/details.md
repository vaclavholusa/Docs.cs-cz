---
title: Zkontrolujte podrobnosti a odstranit metody aplikace ASP.NET Core
author: rick-anderson
description: Další informace o metodě kontroleru podrobnosti a zobrazit v základní aplikaci ASP.NET Core MVC.
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: ce5b2af148ddba9bc718345c0b8074da8724308d
ms.sourcegitcommit: 32f5ee0690604d451f61e9a5c28881c9fcf85738
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/29/2018
ms.locfileid: "47454801"
---
# <a name="examine-the-details-and-delete-methods-of-an-aspnet-core-app"></a>Zkontrolujte podrobnosti a odstranit metody aplikace ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Otevřete řadič video a zkontrolovat `Details` metody:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_details)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

::: moniker-end

Modul generování uživatelského rozhraní MVC, který vytvořili této metodě akce přidá komentář zobrazující požadavek HTTP, který vyvolá metodu. V tomto případě je požadavek GET s tři segmenty adres URL, `Movies` kontroleru, `Details` metoda a `id` hodnotu. Odvolání tyto segmenty jsou definovány v *Startup.cs*.

[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

EF usnadňuje hledání pro data s využitím `SingleOrDefaultAsync` metody. Důležitou funkci zabezpečení integrované do metody je, že kód ověří, že metoda hledání našla filmu předtím, než se pokusí provádět s ním. Například se hacker by mohla zanést chyby do lokality tak, že změníte adresu URL vytvořené odkazy z `http://localhost:xxxx/Movies/Details/1` na něco jako `http://localhost:xxxx/Movies/Details/12345` (nebo jinou hodnotu, která nepředstavuje skutečný film). Pokud jste nezaškrtli null filmu, aplikace by vyvolat výjimku.

Zkontrolujte `Delete` a `DeleteConfirmed` metody.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_delete)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]

::: moniker-end

Všimněte si, `HTTP GET Delete` metody nedojde k odstranění zadaného film, vrátí zobrazení videa ve kterém můžete odeslat (HttpPost) odstranění. Operace delete v reakci na příkaz GET požádat o (nebo k tomuto účelu, provádění operace úpravy, vytváření operace nebo jiné operace, která se mění data) otevře bezpečnostní riziko.

`[HttpPost]` Pojmenovanou metodu, která odstraní data `DeleteConfirmed` poskytnout metodu HTTP POST jedinečný podpis nebo název. Níže jsou zobrazena podpisy dvou metod:

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]


Modul CLR (CLR) vyžaduje přetížené metody mají jedinečný podpis (stejný název metody, ale jiný seznam parametrů). Tady však dva `Delete` metody – jeden u metody GET - a jeden pro metodu POST, že mají stejný podpis parametru. (I potřebují tak, aby přijímal celá čísla jako parametr.)

Existují dva přístupy k tomuto problému, jeden je umožnit metody různými názvy. Je to, co mechanismu generování uživatelského rozhraní nebyl v předchozím příkladu. Ale zavádí malé potíže: ASP.NET mapuje segmentů adresy URL na metody akce podle názvu, a Pokud přejmenujete metodu, obvykle směrování nebude moci najít tuto metodu. Řešení se zobrazí v příkladu, který je přidat `ActionName("Delete")` atribut `DeleteConfirmed` metoda. Tento atribut provádí mapování pro směrování systém tak, aby se najít adresu URL, která zahrnuje /Delete/ pro požadavek POST `DeleteConfirmed` metody.

Další běžné alternativní pro metody, které mají stejné názvy a podpisy se uměle změna podpisu metody POST, která zahrnují speciální (nepoužívané) parametru. To je, co jsme to udělali v předchozí odeslání když jsme přidali `notUsed` parametru. Může to samé udělá zde pro `[HttpPost] Delete` metody:

```csharp
// POST: Movies/Delete/6
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a>Publikování do Azure

Informace o nasazení do Azure, viz [kurz: vytvoření aplikace ASP.NET se službou SQL Database v Azure](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase). Pokyny jsou určené pro aplikace ASP.NET, není aplikace v ASP.NET Core, ale postup je stejný.

> [!div class="step-by-step"]
> [Předchozí](validation.md)
