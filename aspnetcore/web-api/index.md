---
title: Vytvoření webového rozhraní API pomocí ASP.NET Core
author: scottaddie
description: Další informace o funkcích, které jsou k dispozici pro vytváření webových rozhraní API v ASP.NET Core a kdy je vhodné používat jednotlivé funkce.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/15/2018
uid: web-api/index
ms.openlocfilehash: d410f28ff7fda3bf33f73c06b3e626dfd4ee7dd8
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/22/2018
ms.locfileid: "41822137"
---
# <a name="build-web-apis-with-aspnet-core"></a>Vytvoření webového rozhraní API pomocí ASP.NET Core

Podle [Scott Addie](https://github.com/scottaddie)

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))

Tento dokument popisuje, jak vytvořit webové rozhraní API v ASP.NET Core a je nejvhodnější používat jednotlivé funkce.

## <a name="derive-class-from-controllerbase"></a>Odvodit třídu z ControllerBase

Dědí <xref:Microsoft.AspNetCore.Mvc.ControllerBase> třídy v kontroleru, který má sloužit jako webové rozhraní API. Příklad:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

`ControllerBase` Třídě poskytuje přístup k několika vlastností a metod. V předchozím kódu mezi příklady patří <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> a <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>. Tyto metody jsou volány v rámci metod akce vrátit kód HTTP 400 a 201 stavové kódy, v uvedeném pořadí. <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> Vlastnost také poskytuje `ControllerBase`, přistupuje ke zpracování žádosti o ověření modelu.

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a>Třída s atributem ApiControllerAttribute opatřit poznámkami

ASP.NET Core 2.1 přináší [[objektu ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) atribut k označení webového rozhraní API třídy kontroleru. Příklad:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

Kompatibilita verze 2.1 nebo novější, nastavené přes <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, je potřeba použít tento atribut. Například zvýrazněný kód do *Startup.ConfigureServices* nastaví příznak 2.1 kompatibility:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

Další informace naleznete v tématu <xref:mvc/compatibility-version>.

`[ApiController]` Atribut běžně doplňuje `ControllerBase` povolit chování specifické pro REST pro řadiče. `ControllerBase` poskytuje přístup k metodám například <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> a <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.

Další možností je vytvořit třídu vlastní základní kontroler opatřen poznámkou `[ApiController]` atribut:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

Následující části popisují užitečných funkcí, které jsou přidány pomocí atributu.

### <a name="automatic-http-400-responses"></a>Automatické odpovědi HTTP 400

Chyby ověření automaticky aktivuje odpověď HTTP 400. Následující kód bude nutná u akcí:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

Výchozí chování je zakázaná. Pokud <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> je nastavena na `true`. Přidejte následující kód do *Startup.ConfigureServices* po `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a>Odvození parametr zdroje vazby

Zdrojový atribut vazby definuje umístění, ve kterém není nalezena hodnota parametru akce. Existují následující atributy zdroje vazby:

|Atribut|Zdroje připojení |
|---------|---------|
|**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**     | Text žádosti |
|**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**     | Data formuláře v textu požadavku |
|**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)** | Hlavička požadavku |
|**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**   | Požádat o parametru řetězce dotazu |
|**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**   | Data trasy z aktuální žádosti |
|**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)** | Žádost o služby, vložený jako parametru akce |

> [!WARNING]
> Nepoužívejte `[FromRoute]` při může obsahovat hodnoty `%2f` (to znamená `/`). `%2f` nebude znaků bez řídících k `/`. Použití `[FromQuery]` Pokud hodnota může obsahovat `%2f`.

Bez `[ApiController]` atribut, vytvoření vazby zdroje nejsou explicitně definovány atributy. V následujícím příkladu `[FromQuery]` atribut označuje, že `discontinuedOnly` je zadána hodnota parametru v řetězci dotazu v adrese URL požadavku:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

Odvozená pravidla se použijí pro zdroje dat výchozí parametry akce. Tato pravidla konfigurace zdroje připojení, v opačném případě budete pravděpodobně pro ruční použití na parametry akce. Atributy zdroje vazby chovají následovně:

* **[FromBody]**  odvodit pro komplexní typ parametrů. Výjimkou z tohoto pravidla je libovolný integrované, komplexní typ zvláštní význam, jako například <xref:Microsoft.AspNetCore.Http.IFormCollection> a <xref:System.Threading.CancellationToken>. Zdrojový kód odvození vazby ignoruje tyto speciální typy. `[FromBody]` není odvodit pro jednoduché typy, jako například `string` nebo `int`. Proto `[FromBody]` atribut by měl použít pro jednoduché typy, které tuto funkci potřebujete. Pokud má akci explicitně zadán více než jeden parametr (prostřednictvím `[FromBody]`) nebo odvozený jako vázaný z textu požadavku, je vyvolána výjimka. Například následující akce podpisy způsobit výjimku:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* **[FromForm]**  odvodit pro parametry akce typu <xref:Microsoft.AspNetCore.Http.IFormFile> a <xref:Microsoft.AspNetCore.Http.IFormFileCollection>. Není odvodit pro jednoduché nebo uživatelem definované typy.
* **[FromRoute]**  odvodit pro název parametru žádné akce odpovídající parametr v šabloně trasy. Když víc tras odpovídá parametru akce, je považován za libovolnou hodnotu trasy `[FromRoute]`.
* **[FromQuery]**  odvodit pro všechny ostatní parametry akce.

Výchozí pravidla pro odvození jsou zakázané. Pokud <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> je nastavena na `true`. Přidejte následující kód do *Startup.ConfigureServices* po `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a>Odvození multipart/formulář data požadavku

Když parametr akce je opatřen poznámkou [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) atribut, `multipart/form-data` žádosti je odvozený typ obsahu.

Výchozí chování je zakázaná. Pokud <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> je nastavena na `true`. Přidejte následující kód do *Startup.ConfigureServices* po `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a>Atribut směrování požadavků

Směrování atributů se změní na požadavek. Příklad:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

Akce jsou nedostupné přes [trasy konvenční](xref:mvc/controllers/routing#conventional-routing) definované v <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> nebo <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> v *Startup.Configure*.

::: moniker-end

## <a name="additional-resources"></a>Další zdroje

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
