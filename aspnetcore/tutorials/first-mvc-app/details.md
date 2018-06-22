---
title: Zkontrolujte podrobnosti a odstraňte metody aplikace ASP.NET Core
author: rick-anderson
description: Další informace o metodě řadiče podrobnosti a zobrazit v základní aplikaci ASP.NET MVC jádra.
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: f7f9a369e3e612542140fcf1091b21037e530a91
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278645"
---
# <a name="examine-the-details-and-delete-methods-of-an-aspnet-core-app"></a>Zkontrolujte podrobnosti a odstraňte metody aplikace ASP.NET Core

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Otevřete řadičem film a prověřit `Details` metoda:

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_details)]

::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

::: moniker-end

Modul generování uživatelského rozhraní MVC, který vytvořili této metodě akce přidá komentář zobrazující požadavek HTTP, která volá metodu. V takovém případě je požadavek GET s tři segmenty adres URL, `Movies` řadiče, `Details` metoda a `id` hodnotu. Tyto segmenty jsou definovány v odvolání *Startup.cs*.

[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

EF usnadňuje vyhledání dat pomocí `SingleOrDefaultAsync` metoda. Důležitou funkci zabezpečení integrovaná metoda je, že kód ověří, že metodu search nalezl film, než se pokusí provádět žádné kroky s ním. Například se hacker může představovat chyby na webu tak, že změníte adresu URL vytvořené odkazy z `http://localhost:xxxx/Movies/Details/1` například `http://localhost:xxxx/Movies/Details/12345` (nebo jinou hodnotu, která nepředstavuje skutečný film). Pokud nebylo vyhledat null film, aplikace by vyvolat výjimku.

Zkontrolujte `Delete` a `DeleteConfirmed` metody.

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_delete)]

::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]

::: moniker-end

Všimněte si, že `HTTP GET Delete` metoda nedojde k odstranění určený film, vrátí zobrazení filmu kde můžete odeslat (HttpPost) k odstranění. Provádění operace odstranění v reakci na příkaz GET požadavku (nebo k tomuto účelu, provádění operace upravit, vytvořit operaci, nebo jiná operace, která se mění data) otevře bezpečnostní riziko.

`[HttpPost]` Metodu, která odstraňuje data jmenuje `DeleteConfirmed` umožnit metodu HTTP POST jedinečný podpis nebo název. Níže jsou uvedeny podpisy dvou metod:

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]


Modul CLR (CLR) vyžaduje přetížené metody jedinečný podpis (stejný název metody, ale jiné seznam parametrů). Zde však dvě `Delete` metody – jeden pro GET--a jeden pro metodu POST, že mají stejný parametr podpis. (I potřebují tak, aby přijímal jeden celé číslo jako parametr.)

Existují dva přístupy k tomuto problému, jedna je umožnit metody odlišné názvy. To je mechanismus generování uživatelského rozhraní, které jste provedli v předchozím příkladu. Ale vzniká malé problému: ASP.NET mapuje segmentů adresy URL na metody akce podle názvu, a Pokud přejmenujete metodu, normálně směrování nebude schopna nalézt dané metody. Je řešení, najdete v příkladu, který je přidání `ActionName("Delete")` atribut `DeleteConfirmed` metoda. Tento atribut provádí mapování pro směrování systému, aby se najít adresu URL, která zahrnuje /Delete/ pro požadavek POST `DeleteConfirmed` metoda.

Další běžné pracovní kolem pro metody, které mají stejné názvy a podpisy je uměle změnit podpis metodu POST zahrnout další (nepoužívané) parametr. To je, co jsme to udělali v předchozím post při jsme přidali `notUsed` parametr. Můžete to udělat samé sem pro `[HttpPost] Delete` metoda:

```csharp
// POST: Movies/Delete/6
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a>Publikování v Azure

V tématu [publikování webové aplikace ASP.NET Core Azure App Service pomocí sady Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) pokyny, jak publikovat tuto aplikaci do Azure pomocí sady Visual Studio.  Aplikace lze publikovat také z [příkazového řádku](xref:tutorials/publish-to-azure-webapp-using-cli).

> [!div class="step-by-step"]
> [Předchozí](validation.md)
