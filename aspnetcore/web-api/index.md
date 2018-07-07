---
title: Vytvoření webového rozhraní API pomocí ASP.NET Core
author: scottaddie
description: Další informace o funkcích, které jsou k dispozici pro vytváření webových rozhraní API v ASP.NET Core a kdy je vhodné používat jednotlivé funkce.
ms.author: scaddie
ms.custom: mvc
ms.date: 07/06/2018
uid: web-api/index
ms.openlocfilehash: 84e4a51a8a8ab031752ef054cba834bd202a4927
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894212"
---
# <a name="build-web-apis-with-aspnet-core"></a>Vytvoření webového rozhraní API pomocí ASP.NET Core

Podle [Scott Addie](https://github.com/scottaddie)

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))

Tento dokument popisuje, jak vytvořit webové rozhraní API v ASP.NET Core a je nejvhodnější používat jednotlivé funkce.

## <a name="derive-class-from-controllerbase"></a>Odvodit třídu z ControllerBase

Dědí [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) třídy v kontroleru, který má sloužit jako webové rozhraní API. Příklad:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

`ControllerBase` Třídě poskytuje přístup k několika vlastností a metod. V předchozím kódu mezi příklady patří [chybného požadavku](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) a [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction). Tyto metody jsou volány v rámci metod akce vrátit kód HTTP 400 a 201 stavové kódy, v uvedeném pořadí. [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) vlastnost také poskytuje `ControllerBase`, přistupuje ke zpracování žádosti o ověření modelu.

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a>Třída s atributem ApiControllerAttribute opatřit poznámkami

ASP.NET Core 2.1 přináší [[objektu ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) atribut k označení webového rozhraní API třídy kontroleru. Příklad:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

Kompatibilita verze 2.1 nebo novější, nastavené přes [SetCompatibilityVersion](/dotnet/api/microsoft.extensions.dependencyinjection.mvccoremvcbuilderextensions.setcompatibilityversion), je potřeba použít tento atribut. Například zvýrazněný kód do *Startup.ConfigureServices* nastaví příznak 2.1 kompatibility:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

`[ApiController]` Atribut běžně doplňuje `ControllerBase` povolit chování specifické pro REST pro řadiče. `ControllerBase` poskytuje přístup k metodám například [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) a [souboru](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).

Další možností je vytvořit třídu vlastní základní kontroler opatřen poznámkou `[ApiController]` atribut:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

Následující části popisují užitečných funkcí, které jsou přidány pomocí atributu.

### <a name="automatic-http-400-responses"></a>Automatické odpovědi HTTP 400

Chyby ověření automaticky aktivuje odpověď HTTP 400. Následující kód bude nutná u akcí:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]

Výchozí chování je zakázaná. Pokud [SuppressModelStateInvalidFilter](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressmodelstateinvalidfilter) je nastavena na `true`. Přidejte následující kód do *Startup.ConfigureServices* po `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a>Odvození parametr zdroje vazby

Zdrojový atribut vazby definuje umístění, ve kterém není nalezena hodnota parametru akce. Existují následující atributy zdroje vazby:

|Atribut|Zdroje připojení |
|---------|---------|
|**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**     | Text žádosti |
|**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**     | Data formuláře v textu požadavku |
|**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)** | Hlavička požadavku |
|**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**   | Požádat o parametru řetězce dotazu |
|**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**   | Data trasy z aktuální žádosti |
|**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)** | Žádost o služby, vložený jako parametru akce |

> [!WARNING]
> Nepoužívejte `[FromRoute]` při může obsahovat hodnoty `%2f` (to znamená `/`). `%2f` nebude znaků bez řídících k `/`. Použití `[FromQuery]` Pokud hodnota může obsahovat `%2f`.

Bez `[ApiController]` atribut, vytvoření vazby zdroje nejsou explicitně definovány atributy. V následujícím příkladu `[FromQuery]` atribut označuje, že `discontinuedOnly` je zadána hodnota parametru v řetězci dotazu v adrese URL požadavku:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=2)]

Odvozená pravidla se použijí pro zdroje dat výchozí parametry akce. Tato pravidla konfigurace zdroje připojení, v opačném případě budete pravděpodobně pro ruční použití na parametry akce. Atributy zdroje vazby chovají následovně:

* **[FromBody]**  odvodit pro komplexní typ parametrů. Výjimkou z tohoto pravidla je libovolný integrované, komplexní typ zvláštní význam, jako například [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) a [CancellationToken](/dotnet/api/system.threading.cancellationtoken). Zdrojový kód odvození vazby ignoruje tyto speciální typy. Pokud má akci explicitně zadán více než jeden parametr (prostřednictvím `[FromBody]`) nebo odvozený jako vázaný z textu požadavku, je vyvolána výjimka. Například následující akce podpisy způsobit výjimku:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* **[FromForm]**  odvodit pro parametry akce typu [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) a [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection). Není odvodit pro jednoduché nebo uživatelem definované typy.
* **[FromRoute]**  odvodit pro název parametru žádné akce odpovídající parametr v šabloně trasy. Když víc tras odpovídá parametru akce, je považován za libovolnou hodnotu trasy `[FromRoute]`.
* **[FromQuery]**  odvodit pro všechny ostatní parametry akce.

Výchozí pravidla pro odvození jsou zakázané. Pokud [SuppressInferBindingSourcesForParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressinferbindingsourcesforparameters) je nastavena na `true`. Přidejte následující kód do *Startup.ConfigureServices* po `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a>Odvození multipart/formulář data požadavku

Když parametr akce je opatřen poznámkou [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) atribut, `multipart/form-data` žádosti je odvozený typ obsahu.

Výchozí chování je zakázaná. Pokud [SuppressConsumesConstraintForFormFileParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressconsumesconstraintforformfileparameters) je nastavena na `true`. Přidejte následující kód do *Startup.ConfigureServices* po `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a>Atribut směrování požadavků

Směrování atributů se změní na požadavek. Příklad:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

Akce jsou nedostupné přes [trasy konvenční](xref:mvc/controllers/routing#conventional-routing) definované v [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) nebo [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) v *Startup.Configure*.

::: moniker-end

## <a name="additional-resources"></a>Další zdroje

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
