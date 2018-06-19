---
title: Migrace z rozhraní ASP.NET Web API na jádro ASP.NET
author: ardalis
description: Zjistěte, jak migrovat implementace webového rozhraní API z rozhraní ASP.NET Web API ASP.NET MVC jádra.
manager: wpickett
ms.author: riande
ms.date: 05/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/webapi
ms.openlocfilehash: 8d842877e49e317323d453e71ebb3302245f388d
ms.sourcegitcommit: 3d071fabaf90e32906df97b08a8d00e602db25c0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/10/2018
ms.locfileid: "34009069"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>Migrace z rozhraní ASP.NET Web API na jádro ASP.NET

Podle [Steve Smith](https://ardalis.com/) a [Scott Addie](https://scottaddie.com)

Webová rozhraní API jsou služeb HTTP, které využity širokou škálou klientů včetně prohlížečů a mobilních zařízení. Jádro ASP.NET MVC zahrnuje podporu pro vytváření webových rozhraní API poskytuje jednoduchý a konzistentní způsob vytváření webových aplikací. V tomto článku jsme ukazují na kroky potřebné k migraci implementace webového rozhraní API z webového rozhraní API ASP.NET na ASP.NET MVC jádra.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="review-aspnet-web-api-project"></a>Zkontrolujte ASP.NET Web API projektu

Tento článek používá ukázkový projekt *ProductsApp*, které byly vytvořeny v článku [Začínáme s ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) jako výchozí bod. V tomto projektu jednoduché projektu ASP.NET Web API nakonfigurovat následujícím způsobem.

V *Global.asax.cs*, Přišla žádost o `WebApiConfig.Register`:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

`WebApiConfig` je definována v *App_Start*, a má jenom jeden statický `Register` metoda:

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]


Tato třída nakonfiguruje [směrováním atributů](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), i když je ve skutečnosti není používán v projektu. Nakonfiguruje taky do směrovací tabulky, který je používán rozhraní ASP.NET Web API. V takovém případě bude rozhraní ASP.NET Web API očekávat adres URL pro odpovídat formátu */api/ {controller} / {id}*, s *{id}* je nepovinná.

*ProductsApp* projekt obsahuje pouze jednu jednoduché řadiči, který dědí z `ApiController` a zveřejňuje dvě metody:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

Nakonec model *produktu*používané nástrojem *ProductsApp*, je jednoduchá:

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

Teď, když máme Jednoduchý projekt, ze kterého má začít, budeme ukazují, jak migrovat tento projekt webového rozhraní API ASP.NET MVC jádra.

## <a name="create-the-destination-project"></a>Vytvoření projektu cílové

Pomocí sady Visual Studio, vytvořte nový, prázdný řešení a pojmenujte ji *WebAPIMigration*. Přidat existující *ProductsApp* projektu do ní a pak přidejte nový projekt webové aplikace ASP.NET Core do řešení. Název nového projektu *ProductsCore*.

![Otevřete dialogové okno Nový projekt webové šablon](webapi/_static/add-web-project.png)

V dalším kroku vyberte šablona projektu webového rozhraní API. Jsme bude migrovat *ProductsApp* obsah do nového projektu.

![Dialogové okno nové webové aplikace pomocí šablony projektu webového rozhraní API v seznamu šablon ASP.NET Core](webapi/_static/aspnet-5-webapi.png)

Odstranit `Project_Readme.html` soubor z nového projektu. Řešení by měl nyní vypadat takto:

![Otevřít v Průzkumníkovi řešení zobrazující soubory a složky z projektů ProductsApp a ProductsCore řešení aplikace](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a>Migruje konfiguraci

Už používá ASP.NET Core *Global.asax*, *web.config*, nebo *App_Start* složky. Místo toho jsou všechny spuštění úlohy provádějí *Startup.cs* v kořenovém adresáři projektu (viz [spuštění aplikace](../fundamentals/startup.md)). V aplikaci ASP.NET MVC jádra, na základě atributů směrování je nyní zahrnutá ve výchozím nastavení při `UseMvc()` nazývá; a toto je doporučený postup pro konfiguraci tras webového rozhraní API (a je jak starter projekt webového rozhraní API zpracovává směrování).

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

Za předpokladu, že chcete použít atribut směrování ve vašem projektu do budoucna, je potřeba žádná další konfigurace. Jednoduše použít atributy podle potřeby řadiče a akce, jak se provádí v ukázce `ValuesController` třídu, která je součástí starter projekt webového rozhraní API:

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

Všimněte si přítomnost *[controller]* na řádku 8. Na základě atributů směrování nyní podporuje určité tokeny, například *[controller]* a *[akce]*. Tyto tokeny se nahradí za běhu název kontroler nebo akce, v uvedeném pořadí, do které byl použit atribut. Tato možnost slouží k snížit počet magic řetězců v projektu, a zajišťuje, že trasy se zachová synchronizované s jejich odpovídající kontrolerů a akcí při použití automatické přejmenování refaktoring.

Pokud chcete migrovat kontroler API produktů, jsme musíte nejdříve zkopírovat *ProductsController* do nového projektu. Pak jednoduše uvést atribut trasy na řadiči:

```csharp
[Route("api/[controller]")]
```

Také je nutné přidat `[HttpGet]` atribut tyto dvě metody, protože obě by měla být volána prostřednictvím protokolu HTTP Get. Zahrnout do atribut pro se očekává parametr typu "id" `GetProduct()`:

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

Směrování je nyní nakonfigurován správně; ale jsme ho nemůže ještě otestovat. Další změny, je nutné provést před *ProductsController* bude zkompilovat.

## <a name="migrate-models-and-controllers"></a>Migrace modely a řadiče

Poslední krok v procesu migrace pro tento jednoduchý projekt webového rozhraní API je zkopírovat přes do řadičů a žádné modely používají. V takovém případě jednoduše zkopírovat *Controllers/ProductsController.cs* z původního do nového projektu. Potom zkopírujte celou složku modely z původního projektu do nového. Upravit obory názvů tak, aby odpovídala nové název projektu (*ProductsCore*).  V tomto okamžiku můžete sestavit aplikace, a zjistíte, určitý počet chyb kompilace. Obecně musí spadat do následujících kategorií:

* *Objektu ApiController* neexistuje

* *System.Web.Http* obor názvů neexistuje.

* *IHttpActionResult* neexistuje

Naštěstí tyto lze velmi snadno opravte:

* Změna *objektu ApiController* k *řadič* (budete muset přidat *pomocí Microsoft.AspNetCore.Mvc*)

* Odstraní všechny pomocí příkazu odkazující na *System.Web.Http*

* Změňte všechny metoda vrací *IHttpActionResult* vrátit *IActionResult*

Jakmile se tyto změny byly učiněna a nepoužívané pomocí příkazů odebrat, migrovaných *ProductsController* třída vypadá takto:

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

Teď by měla být moci spustit migrované projekt a přejděte do */api/produkty*; a měli byste vidět úplný seznam produktů 3. Přejděte do */api/products/1* a měli byste vidět první produktu.

## <a name="microsoftaspnetcoremvcwebapicompatshim"></a>Microsoft.AspNetCore.Mvc.WebApiCompatShim

Je užitečné nástroje při migraci rozhraní ASP.NET Web API projektů ASP.NET Core [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) knihovny. Shim kompatibility rozšiřuje základní technologie ASP.NET povolíte počet různých pravidel webovém rozhraní API 2 má být použit. Ukázka, která je součástí dříve v tomto dokumentu je dostatečně základní, že shim kompatibility není nezbytné. U větších projektů pomocí shimu kompatibility může být užitečná pro dočasně přemostění rozhraní API mezera mezi ASP.NET Core a ASP.NET Web API 2.

Shim kompatibility webového rozhraní API je určen pro použití jako dočasné opatření ke zjednodušení migrace velké projekty webového rozhraní API pro ASP.NET Core. V čase je třeba aktualizovat projekty využít ASP.NET Core sítích, aniž byste museli spoléhat na shim kompatibility. 

Kompatibilita funkce obsažené v Microsoft.AspNetCore.Mvc.WebApiCompatShim patří:

* Přidá `ApiController` zadejte tak, aby základních typů řadičů není třeba aktualizovat.
* Umožňuje webové rozhraní API-style vazby modelu. Jádro ASP.NET MVC model vazby funkcí podobně MVC 5, ve výchozím nastavení. Změny shim kompatibility model vazby více podobný konvence vazby modelu webovém rozhraní API 2. Komplexní typy jsou automaticky svázané z textu požadavku.
* Rozšiřuje vazby modelu tak, aby akce kontroleru může trvat parametrů typu `HttpRequestMessage`.
* Přidá formátování zpráv umožňuje akce vracet výsledky typu `HttpResponseMessage`.
* Přidá další odpovědi metody, které webové rozhraní API 2 akce může mít používají k obsluze odpovědí:
    * Objekt HttpResponseMessage generátory:
        * `CreateResponse<T>`
        * `CreateErrorResponse`
    * Výsledek metody akce:
        * `BadResuestErrorMessageResult`
        * `ExceptionResult`
        * `InternalServerErrorResult`
        * `InvalidModelStateResult`
        * `NegotiatedContentResult`
        * `ResponseMessageResult`
* Přidá instanci `IContentNegotiator` do kontejneru DI aplikace a související vyjednávání typů z obsahu díky [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) k dispozici. To zahrnuje typy jako `DefaultContentNegotiator`, `MediaTypeFormatter`atd.

Pokud chcete používat shim kompatibility, budete muset:

* Odkaz [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) balíček NuGet.
* Registrace služby shim kompatibility s kontejnerem aplikace DI voláním `services.AddWebApiConventions()` do aplikace `Startup.ConfigureServices` metoda.
* Definování tras specifické pro webové rozhraní API pomocí `MapWebApiRoute` na `IRouteBuilder` do aplikace `IApplicationBuilder.UseMvc` volání.

## <a name="summary"></a>Souhrn

Migrace Jednoduchý projekt webového rozhraní API ASP.NET na ASP.NET MVC základní je docela jasné, thanks integrovanou podporu pro webové rozhraní API v aplikaci ASP.NET MVC jádra. Hlavní součásti, potřebné pro každé projektu ASP.NET Web API pro migraci jsou tras, kontrolerů a modely, společně s typy používané kontrolery a akce aktualizace.
