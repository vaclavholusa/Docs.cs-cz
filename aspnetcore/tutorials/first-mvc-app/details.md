---
title: "Zkoumání podrobnosti a odstraňte metody"
author: rick-anderson
description: "Metoda kontroleru podrobnosti a zobrazení v základní aplikaci ASP.NET MVC jádra."
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: d70d9d06299e8ee1eaa64da2bd65eb15ffa16553
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="examining-the-details-and-delete-methods"></a>Zkoumání podrobnosti a odstraňte metody

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Otevřete řadičem film a prověřit `Details` metoda:

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

Modul generování uživatelského rozhraní MVC, který vytvořili této metodě akce přidá komentář zobrazující požadavek HTTP, která volá metodu. V takovém případě je požadavek GET s tři segmenty adres URL, `Movies` řadiče, `Details` metoda a `id` hodnotu. Tyto segmenty jsou definovány v odvolání *Startup.cs*.

[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

EF usnadňuje vyhledání dat pomocí `SingleOrDefaultAsync` metoda. Důležitou funkci zabezpečení integrovaná metoda je, že kód ověří, že metodu search nalezl film, než se pokusí provádět žádné kroky s ním. Například se hacker může představovat chyby na webu tak, že změníte adresu URL vytvořené odkazy z `http://localhost:xxxx/Movies/Details/1` například `http://localhost:xxxx/Movies/Details/12345` (nebo jinou hodnotu, která nepředstavuje skutečný film). Pokud nebylo vyhledat null film, aplikace by vyvolat výjimku.

Zkontrolujte `Delete` a `DeleteConfirmed` metody.

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]

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

Děkujeme, že dokončení tento úvod do architektury ASP.NET MVC jádra. Děkujeme za jakékoli komentáře, která zůstanou. [Začínáme s MVC a EF základní](xref:data/ef-mvc/intro) je vynikající postupujte podle kroků až tento kurz.

>[!div class="step-by-step"]
[Předchozí](validation.md)
