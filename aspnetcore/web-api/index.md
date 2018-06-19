---
title: Sestavení webového rozhraní API pomocí ASP.NET Core
author: scottaddie
description: Informace o funkcích, které jsou k dispozici pro sestavení webového rozhraní API v ASP.NET Core a v případě, že je vhodné k použití jednotlivých funkcí.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: web-api/index
ms.openlocfilehash: 6afc02c1a966b62d0fcead0349c5f0803309dcbb
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/10/2018
ms.locfileid: "33972784"
---
# <a name="build-web-apis-with-aspnet-core"></a>Sestavení webového rozhraní API pomocí ASP.NET Core

Podle [Scott Addie](https://github.com/scottaddie)

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))

Tento dokument vysvětluje, jak sestavit webové rozhraní API v ASP.NET Core a v případě, že je nejvhodnější použít každou funkci.

## <a name="derive-class-from-controllerbase"></a>Odvození třídy z ControllerBase

Dědit z [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) třídy v kontroler, který se má sloužit jako webové rozhraní API. Příklad:

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end

`ControllerBase` Třída poskytuje přístup k mnoha vlastnosti a metody. V předchozím příkladu, některé tyto metody zahrnují [struktura BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) a [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction). Tyto metody jsou vyvolány v rámci metody akce vrátí HTTP 400 a 201 stavových kódů, v uvedeném pořadí. [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) vlastnost, také poskytované `ControllerBase`, přistupuje k provedení ověření modelu.

::: moniker range=">= aspnetcore-2.1"
## <a name="annotate-class-with-apicontrollerattribute"></a>Přidání poznámek ke třídě pomocí ApiControllerAttribute

Zavádí ASP.NET Core 2.1 [[objektu ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) atribut k označení webového rozhraní API třídy kontroleru. Příklad:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

Tento atribut je běžně kombinaci s `ControllerBase` k získání přístupu k užitečných metod a vlastností. `ControllerBase` poskytuje přístup k metody, jako [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) a [soubor](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).

Další možností je vytvořit třídu vlastní základní řadič opatřen poznámkou `[ApiController]` atribut:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

Následující části popisují užitečných funkcí, které jsou přidány v atributu.

### <a name="automatic-http-400-responses"></a>Automatické odpovědi HTTP 400

Chyby ověření automaticky aktivuje odpověď HTTP 400. Následující kód změní nepotřebné v vaše akce:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]

Toto výchozí chování je zakázáno s následující kód v *Startup.ConfigureServices*:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a>Odvození parametr zdroje vazby

Zdrojový atribut vazby definuje umístění, ve kterém je hodnota parametru akce nalezena. Následující atributy zdroje vazba neexistuje:

|Atribut|Zdroje vazby |
|---------|---------|
|**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**     | Text žádosti |
|**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**     | Data formuláře v textu požadavku |
|**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)** | Hlavička požadavku |
|**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**   | Žádosti o parametr řetězce dotazu |
|**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**   | Data trasy z aktuální žádosti |
|**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)** | Žádost o službu vložit jako parametru akce |

> [!NOTE]
> Proveďte **není** použít `[FromRoute]` při může obsahovat hodnoty `%2f` (který je `/`) protože `%2f` nebude znaků bez řídících k `/`. Použití `[FromQuery]` Pokud hodnota může obsahovat `%2f`.

Bez `[ApiController]` atribut, vytvoření vazby zdroje explicitně definovaných atributech. V následujícím příkladu `[FromQuery]` atribut znamená, že `discontinuedOnly` je zadána hodnota parametru v adrese URL žádosti řetězec dotazu:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=2)]

Odvozená pravidla jsou použita pro zdroje dat výchozí parametry akce. Tato pravidla nakonfigurovat vazby zdroje, které jinak budete pravděpodobně použít ručně k parametrům akce. Atributy zdroje vazba chovat následujícím způsobem:

* **[FromBody]**  je odvodit parametrů komplexního typu. Výjimku pro toto pravidlo je všech komplexní, předdefinovaných typů zvláštní význam, například [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) a [CancellationToken](/dotnet/api/system.threading.cancellationtoken). Zdrojový kód odvození vazby ignoruje těchto speciálních typů. Když akce má více než jeden parametr explicitně určena (prostřednictvím `[FromBody]`) nebo odvodit jako vázané z textu žádosti, je vyvolána výjimka. Například následující akce podpisy způsobit výjimku:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* **[FromForm]**  je odvodit parametrů akce typu [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) a [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection). Není ho odvodit pro všechny typy jednoduchý nebo vlastní.
* **[FromRoute]**  je odvodit pro libovolný název parametru akce odpovídající parametr v šabloně trasy. Při víc tras odpovídat parametru akce, považuje za žádnou hodnotu trasy `[FromRoute]`.
* **[FromQuery]**  je odvodit pro všechny ostatní parametry akce.

Odvozená pravidla výchozí jsou zakázány následující kód v *Startup.ConfigureServices*:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a>Odvození multipart/formulář data požadavku

Když je opatřen poznámkou parametr akce [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) atribut, `multipart/form-data` požadavku je odvodit typ obsahu.

Výchozí chování je zakázáno s následující kód v *Startup.ConfigureServices*:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a>Atribut směrování požadavek

Atribut směrování se změní na požadavek. Příklad:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

Akce jsou nedostupné prostřednictvím [konvenční trasy](xref:mvc/controllers/routing#conventional-routing) definované v [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) nebo [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) v *Startup.Configure*.
::: moniker-end

## <a name="additional-resources"></a>Další zdroje

* [Návratové typy akcí kontroleru](xref:web-api/action-return-types)
* [Vlastní formátovací moduly](xref:web-api/advanced/custom-formatters)
* [Formátování dat odpovědi](xref:web-api/advanced/formatting)
* [Stránky nápovědy používající Swagger](xref:tutorials/web-api-help-pages-using-swagger)
* [Směrování na akce kontroleru](xref:mvc/controllers/routing)
