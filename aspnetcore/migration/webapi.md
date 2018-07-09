---
title: Migrace z rozhraní ASP.NET Web API pro ASP.NET Core
author: ardalis
description: Zjistěte, jak migrovat implementaci webového rozhraní API z webové rozhraní API ASP.NET do ASP.NET Core MVC.
ms.author: riande
ms.date: 05/10/2018
uid: migration/webapi
ms.openlocfilehash: 8dd969c8644525606227989ca87e41fbfae5aed1
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894189"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>Migrace z rozhraní ASP.NET Web API pro ASP.NET Core

Podle [Steve Smith](https://ardalis.com/) a [Scott Addie](https://scottaddie.com)

Webová rozhraní API jsou služeb HTTP, které jsou poskytovány širokému spektru klientů, včetně prohlížečů a mobilních zařízení. ASP.NET Core MVC zahrnuje podporu pro vytváření webových rozhraní API poskytuje jednoduchý a konzistentní způsob vytváření webových aplikací. V tomto článku vám ukážeme kroky potřebné k migraci implementaci webového rozhraní API z webové rozhraní API ASP.NET do ASP.NET Core MVC.

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="review-aspnet-web-api-project"></a>Přezkoumání projektu ASP.NET Web API

Tento článek používá ukázkový projekt *ProductsApp*, které byly vytvořeny v článku [Začínáme s ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) jako výchozí bod. V příslušném projektu je jednoduchý projekt webového rozhraní API ASP.NET nakonfigurovat následujícím způsobem.

V *Global.asax.cs*, je provedeno volání `WebApiConfig.Register`:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

`WebApiConfig` je definován v *App_Start*, a má jenom jednu statickou `Register` metody:

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]

Tato třída nakonfiguruje [směrováním atributů](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), i když se ve skutečnosti používá v projektu. Nakonfiguruje taky směrovací tabulky, který používá rozhraní ASP.NET Web API. V takovém případě bude rozhraní ASP.NET Web API očekávané adresy URL ve formátu */api/ {controller} / {id}*, s *{id}* je nepovinná.

*ProductsApp* projekt obsahuje pouze jeden jednoduchý kontroleru, která dědí z `ApiController` a poskytuje dvě metody:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

Nakonec modelu *produktu*, používá *ProductsApp*, je jednoduchou třídu:

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

Když teď máme Jednoduchý projekt, ze kterého se má spustit, jsme ukazují, jak migrovat tento projekt webového rozhraní API pro ASP.NET Core MVC.

## <a name="create-the-destination-project"></a>Vytvořte cílový projekt

Pomocí sady Visual Studio, vytvořte novou, prázdnou řešení s názvem *WebAPIMigration*. Přidat existující *ProductsApp* projekt na ni a pak přidejte nový projekt webové aplikace ASP.NET Core do řešení. Název nového projektu *ProductsCore*.

![Otevře dialogové okno Nový projekt webové šablony](webapi/_static/add-web-project.png)

Potom vyberte šablonu projektu webového rozhraní API. Budeme migrovat *ProductsApp* obsah do tohoto nového projektu.

![Dialogové okno nové webové aplikace pomocí šablony projektu webového rozhraní API vybrali v seznamu šablon ASP.NET Core](webapi/_static/aspnet-5-webapi.png)

Odstranit `Project_Readme.html` z projektu nový soubor. Řešení by teď měl vypadat takto:

![Otevřít v Průzkumníkovi řešení se soubory a složkami ProductsApp a ProductsCore projektů řešení aplikací](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a>Migrace konfigurace

ASP.NET Core se už používá *Global.asax*, *web.config*, nebo *App_Start* složek. Místo toho všechny úlohy po spuštění se provádějí *Startup.cs* v kořenovém adresáři projektu (viz [spuštění aplikace](../fundamentals/startup.md)). V ASP.NET Core MVC, směrování na základě atributů je nyní standardní součástí při `UseMvc()` nazývá; a toto je doporučený postup pro konfiguraci tras webového rozhraní API (a způsob, jakým zpracovává počáteční projekt webového rozhraní API směrování).

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

Za předpokladu, že chcete používat směrování atributů ve vašem projektu do budoucna, je potřeba žádná další konfigurace. Jednoduše použijte atributy podle potřeby do kontrolerů a akcí, jak je tomu v ukázce `ValuesController` třídu, která je součástí počáteční projekt webového rozhraní API:

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

Všimněte si přítomnost *[řadič]* na řádku 8. Směrování na základě atributu nyní podporuje určité tokeny, jako například *[řadič]* a *[action]*. Tyto tokeny se nahradí za běhu s názvem kontroler nebo akce, v uvedeném pořadí, ke které byl použit atribut. Tato možnost slouží k omezení počtu magic řetězce v projektu, a zajišťuje, že trasy se budou uchovávat synchronizované s jejich odpovídající kontrolery a akce při použití automatické přejmenování refaktoring.

K migraci kontroler API produkty, jsme nutné nejprve zkopírovat *ProductsController* do nového projektu. Pak jednoduše uvést atribut trasy na kontroleru:

```csharp
[Route("api/[controller]")]
```

Budete taky muset přidat `[HttpGet]` atribut dvě metody, protože oba by měla být volána prostřednictvím HTTP Get. Zahrnout očekává parametr "id" atribut `GetProduct()`:

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

V tomto okamžiku směrování je správně nakonfigurovaná; ale jsme ji nelze ještě otestovat. Je nutné provést další změny před *ProductsController* bude zkompilována.

## <a name="migrate-models-and-controllers"></a>Migrace modelů a Kontrolerů

Posledním krokem v procesu migrace pro tento jednoduchý projekt webového rozhraní API se mají zkopírovat do řadičů a všechny modely, které používají. V takovém případě stačí zkopírovat *Controllers/ProductsController.cs* z původního projektu do nové. Potom zkopírujte celou složku modely z původního projektu do nové. Upravit obory názvů tak, aby odpovídaly novým názvem projektu (*ProductsCore*).  V tomto okamžiku můžete vytvořit aplikaci a zjistíte, počet chyb kompilace. Obecně by měla spadat do následujících kategorií:

* *Objektu ApiController* neexistuje

* *System.Web.Http* obor názvů neexistuje.

* *IHttpActionResult* neexistuje

Naštěstí následují všechny velmi snadné, chcete-li opravit:

* Změna *objektu ApiController* k *řadič* (budete muset přidat *pomocí Microsoft.AspNetCore.Mvc*)

* Odstraní všechny pomocí příkazu odkazující na *System.Web.Http*

* Změnit všechny metody vracející *IHttpActionResult* se vraťte *IActionResult*

Jakmile tyto změny byly provedeny a nepoužívané pomocí příkazů odebrat, migrovaných *ProductsController* třídy vypadá takto:

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

Teď by měl být schopni spustit migrovaného projektu a přejděte do */api/produkty*; a zobrazí se úplný seznam produktů 3. Přejděte do */api/products/1* a měli byste vidět první produktu.

## <a name="aspnet-4x-web-api-2-compatibility-shim"></a>Překrytí kompatibility ASP.NET 4.x webovým rozhraním API 2

Je užitečné nástroje při migraci ASP.NET Web API projektů ASP.NET Core [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) knihovny. Překrytí kompatibility rozšiřuje ASP.NET Core umožňující celou řadou různých konvencí webovým rozhraním API 2, který se má použít. Ukázka přenáší dříve v tomto dokumentu je dostatečně základní, že překrytí kompatibility není nezbytné. U větších projektů pomocí shimu kompatibility může být užitečná pro dočasné překlenutí této propasti API mezi ASP.NET Core a ASP.NET Web API 2.

Překrytí kompatibility webového rozhraní API je určen se použije jako dočasné opatření pro usnadnění migrace velké projekty webového rozhraní API pro ASP.NET Core. V průběhu času se musí aktualizovat projekty ASP.NET Core využít aniž byste museli spoléhat na překrytí kompatibility.

Kompatibilita funkce obsažené v `Microsoft.AspNetCore.Mvc.WebApiCompatShim` patří:

* Přidá `ApiController` tak, aby základních typů řadičů není potřeba aktualizovat.
* Umožňuje vytvořit vazbu modelu webové rozhraní API – stylu. ASP.NET Core MVC model vazby funkce podobně jako na MVC 5, ve výchozím nastavení. Změny překrytí kompatibility vazby se víc podobá vytváření vazby modelu webovým rozhraním API 2 modelu. Komplexní typy jsou automaticky svázán z textu požadavku.
* Rozšiřuje vazby modelu, tak, aby akce kontroleru může mít parametry typu `HttpRequestMessage`.
* Přidá formátování zpráv umožňuje akce vrátí výsledky typu `HttpResponseMessage`.
* Přidá další odpovědi metody, které webové rozhraní API 2 akce může mít používají k předávání odpovědi:
  * Objekt HttpResponseMessage generátorů:
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * Výsledek metody akce:
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* Přidá instanci `IContentNegotiator` do kontejnerů DI aplikace a provede vyjednávání souvisejících typů z obsahu [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) k dispozici. Jedná se o typech, jako je `DefaultContentNegotiator`, `MediaTypeFormatter`atd.

Chcete-li použít překrytí kompatibility, budete muset:

* Nainstalujte [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) balíček NuGet.
* Zaregistrovat překrytí kompatibility služby kontejnerů DI aplikace po zavolání `services.AddMvc().AddWebApiConventions()` aplikace `Startup.ConfigureServices` metody.
* Definování tras specifické pro webové rozhraní API pomocí `MapWebApiRoute` na `IRouteBuilder` aplikace `IApplicationBuilder.UseMvc` volání.

## <a name="summary"></a>Souhrn

Migrace Jednoduchý projekt webového rozhraní API ASP.NET do ASP.NET Core MVC je celkem jasné, Děkujeme integrovanou podporu pro webové rozhraní API v ASP.NET Core MVC. Hlavní části, bude nutné každý projekt ASP.NET Web API pro migraci jsou tras, kontrolerů a modelů, spolu s aktualizace typů používaných modulem kontrolery a akce.
