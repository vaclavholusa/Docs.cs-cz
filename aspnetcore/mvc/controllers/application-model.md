---
title: "Práce s modelem aplikace"
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/application-model
ms.openlocfilehash: 89a7af0ff95754f036b027aeafb8e25e49f397e2
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="working-with-the-application-model"></a>Práce s modelem aplikace

Podle [Steve Smith](https://ardalis.com/)

Definuje rozhraní ASP.NET MVC jádra *aplikačního modelu* představující součásti aplikace MVC. Můžete číst a manipulaci s Tento model změnit chování prvky MVC. Ve výchozím nastavení MVC dodržovat určité konvence určit, které třídy jsou považovány za řadiče, které metody na tyto třídy jsou akce a chování parametry a směrování. Toto chování, aby odpovídaly potřebám vaší aplikace pomocí vytvoření vlastní konvence a jejich použití globálně, nebo jako atributy můžete přizpůsobit.

## <a name="models-and-providers"></a>Modely a zprostředkovatelů

Model aplikace ASP.NET MVC základní zahrnují abstraktní rozhraní a třídy konkrétní implementace, které popisují aplikaci MVC. Tento model je výsledkem MVC zjišťování řadičů aplikace, akce, parametrů akcí, trasy a filtry podle výchozích konvencí. Ve spolupráci s modelem aplikace, můžete upravit podle různých pravidel výchozí chování MVC v aplikaci. Parametry, názvy, trasy a filtry se používají jako konfigurační data pro akce a kontrolery.

Model ASP.NET MVC základní aplikace má následující strukturu:

* ApplicationModel
    * Řadiče (ControllerModel)
        * Akce (ActionModel)
            * Parametry (ParameterModel)

Každou úroveň modelu má přístup k společného `Properties` umožňuje přístup a přepsat vyšší úrovně v hierarchii, která nastavuje hodnoty vlastností kolekce a nižších úrovních. Vlastnosti jsou nastavené jako trvalé k `ActionDescriptor.Properties` vytvoření akce. Pak pokud se žádost, všechny vlastnosti konvence přidat ani upravit lze přistupovat prostřednictvím `ActionContext.ActionDescriptor.Properties`. Pomocí vlastnosti je skvělým způsobem, jak konfigurovat filtry, vazače modelů, atd. pro jednotlivé akce.

> [!NOTE]
> `ActionDescriptor.Properties` Kolekce není vláken (pro zápisy) po dokončení spuštění aplikace. Konvence jsou nejlepší způsob, jak bezpečně přidat data do této kolekce.

### <a name="iapplicationmodelprovider"></a>IApplicationModelProvider

Jádro ASP.NET MVC načte model aplikace pomocí zprostředkovatele vzoru, definovaného [IApplicationModelProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider) rozhraní. Tato část popisuje některé z interní implementace podrobnosti o tom, tato funkce zprostředkovatele. To je rozšířená – většinu aplikací, které využívají model aplikace by to udělat ve spolupráci s konvence.

Implementace `IApplicationModelProvider` rozhraní "wrap" jiné, s každou implementaci volání `OnProvidersExecuting` ve vzestupném pořadí podle jeho `Order` vlastnost. `OnProvidersExecuted` Metoda je volána poté v obráceném pořadí. Rozhraní framework definuje několik poskytovatelů:

První (`Order=-1000`):

* [`DefaultApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.defaultapplicationmodelprovider)

Potom (`Order=-990`):

* [`AuthorizationApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.authorizationapplicationmodelprovider)
* [`CorsApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.cors.internal.corsapplicationmodelprovider)

> [!NOTE]
> Pořadí, ve které dva poskytovatelé se stejnou hodnotou pro `Order` se nazývají není definován a proto se spolehnout.

> [!NOTE]
> `IApplicationModelProvider` je rozšířené koncept pro nástroj framework autorům rozšíření. Obecně platí aplikace by měl použít konvence a rozhraní by měl používat zprostředkovatele. Klíče rozdíl je, že zprostředkovatelé vždy před konvence.

`DefaultApplicationModelProvider` Vytváří řadu výchozí chování používá ASP.NET MVC jádra. Jeho zodpovědnosti patří:

* Přidávání globálních filtrů pro daný kontext
* Přidání řadiče do kontextu
* Přidání metody veřejné kontroleru jako akce
* Přidání parametrů metody akce v kontextu
* Použití trasy a další atributy

Některé integrované chování jsou implementované `DefaultApplicationModelProvider`. Tento zprostředkovatel je zodpovědný za vytváření [ `ControllerModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel), která zase odkazuje [ `ActionModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ActionModel), [ `PropertyModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel), a [ `ParameterModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ParameterModel) instance. `DefaultApplicationModelProvider` Třída je podrobnosti implementace interní framework, která může a bude v budoucnu změnit. 

`AuthorizationApplicationModelProvider` Zodpovídá za použití chování přidružené `AuthorizeFilter` a `AllowAnonymousFilter` atributy. [Další informace o těchto atributů](xref:security/authorization/simple).

`CorsApplicationModelProvider` Implementuje chování přidružené `IEnableCorsAttribute` a `IDisableCorsAttribute`a `DisableCorsAuthorizationFilter`. [Další informace o CORS](xref:security/cors).

## <a name="conventions"></a>Konvence

Aplikační model definuje abstrakce konvence, které poskytují jednodušší způsob, jak přizpůsobit chování modelů než přepsání celý model nebo zprostředkovatele. Tato abstrakce jsou doporučeným způsobem, jak upravit chování vaší aplikace. Konvence poskytují způsob, jak napsat kód, který bude dynamicky použít vlastní nastavení. Při [filtry](xref:mvc/controllers/filters) úprava chování rozhraní framework pro zajištění, přizpůsobení umožňují řídit, jak společně drátové celou aplikaci.

K dispozici jsou následující konvence:

* [`IApplicationModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelconvention)
* [`IControllerModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.icontrollermodelconvention)
* [`IActionModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iactionmodelconvention)
* [`IParameterModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iparametermodelconvention)

Konvence použijí přidáním možnosti MVC nebo implementací `Attribute`s a jejich použití k řadiče, akcí nebo parametrů akcí (podobně jako [ `Filters` ](xref:mvc/controllers/filters)). Na rozdíl od filtrů jsou konvence spustit pouze při spouštění aplikace, nikoli jako součást každý požadavek.

### <a name="sample-modifying-the-applicationmodel"></a>Ukázka: Úprava ApplicationModel

Následující konvence se používá k přidání vlastnosti do aplikačního modelu. 

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/ApplicationDescription.cs)]

Konvence modelu aplikace se použijí jako možnosti, když MVC je přidaný do `ConfigureServices` v `Startup`.

[!code-csharp[](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=5)]

Vlastnosti jsou k dispozici `ActionDescriptor` vlastnosti kolekce v rámci akce kontroleru:

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/AppModelController.cs?name=AppModelController)]

### <a name="sample-modifying-the-controllermodel-description"></a>Ukázka: Úprava ControllerModel popis

Jako v předchozím příkladu řadiče modelu můžete také upravit, aby obsahovat vlastní vlastnosti. Tyto přepíše existující vlastnosti se stejným názvem zadané v modelu aplikace. Následující konvence atribut přidá popis na úrovni kontroleru:

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/ControllerDescriptionAttribute.cs)]

Touto konvencí se použije jako atribut na řadiči.

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=ControllerDescription&highlight=1)]

Vlastnost "Popis" přistupuje stejným způsobem jako v předchozích příkladech.

### <a name="sample-modifying-the-actionmodel-description"></a>Ukázka: Úprava ActionModel popis

Konvence samostatné atribut lze použít k jednotlivým akcím, přepsání nastavení na úrovni aplikace nebo řadič se už používá.

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/ActionDescriptionAttribute.cs)]

Použití tato akce v kontroleru předchozí příklad ukazuje, jak přepíše úrovni kontroleru konvence:

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=DescriptionAttributesController&highlight=9)]

### <a name="sample-modifying-the-parametermodel"></a>Ukázka: Úprava ParameterModel

Následující konvence lze použít pro parametry akce k úpravě jejich `BindingInfo`. Následující konvenci vyžaduje, aby parametr parametr trasy; Další potenciální vazby zdroje (např. hodnoty řetězce dotazu) se ignorují.

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/MustBeInRouteParameterModelConvention.cs)]

Atribut může použít jakékoli parametr akce:

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/ParameterModelController.cs?name=ParameterModelController&highlight=5)]

### <a name="sample-modifying-the-actionmodel-name"></a>Ukázka: Úprava názvu ActionModel

Upraví následující konvence `ActionModel` aktualizovat *název* akce, které je použito. Nový název je zadat jako parametr do atribut. Tento nový název se používá ve směrování, takže ovlivní trasy použité k dosažení této metodě akce.

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/CustomActionNameAttribute.cs)]

Tento atribut se používá pro metodu akce v `HomeController`:

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/HomeController.cs?name=ActionModelConvention&highlight=2)]

I když je název metody `SomeName`, atribut přepsání konvence MVC pomocí názvu metody a nahradí název akce s `MyCoolAction`. Proto trasy použité k dosažení této akce je `/Home/MyCoolAction`.

> [!NOTE]
> V tomto příkladu je v podstatě stejný jako pomocí integrovaných [název akce](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.actionnameattribute) atribut.

### <a name="sample-custom-routing-convention"></a>Ukázka: Vlastní směrování konvence

Můžete použít `IApplicationModelConvention` přizpůsobit funguje jak směrování. Například následující konvence bude začlenit řadiče obory názvů do jejich trasy, nahraďte `.` v oboru názvů s `/` v trasy:

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/NamespaceRoutingConvention.cs)]

Konvence je přidán jako možnost v spuštění.

[!code-csharp[](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=6)]

> [!TIP]
> Můžete přidat konvence pro vaše [middleware](xref:fundamentals/middleware/index) díky přístupu k `MvcOptions` pomocí `services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`

Tato ukázka platí tato konvence pro tras, které nepoužívají atribut směrování, pokud je řadič má "Namespace" v názvu. Následující řadiče ukazuje touto konvencí:

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/NamespaceRoutingController.cs?highlight=7-8)]

## <a name="application-model-usage-in-webapicompatshim"></a>Používání modelu aplikací v WebApiCompatShim

Jádro ASP.NET MVC používá jinou sadu konvence z technologie ASP.NET Web API 2. Pomocí vlastních názvů, můžete upravit aplikaci ASP.NET MVC základní chování, aby byla konzistentní se u aplikace webového rozhraní API. Microsoft se dodává [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/) speciálně pro tento účel.

> [!NOTE]
> Další informace o [migrace z rozhraní ASP.NET Web API](xref:migration/webapi).

Pokud chcete použít Shimu Web API kompatibility, budete muset do projektu přidejte balíček a pak přidejte se názvů do MVC voláním `AddWebApiConventions` v `Startup`:

```c#
services.AddMvc().AddWebApiConventions();
```

Konvence poskytované shimu se použije pouze na části aplikace, které předtím určité atributy použité k nim. Následující čtyři atributy se používají řídit, která řadiče by měl mít jejich konvence upraveném shim konvence:

* [UseWebApiActionConventionsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiactionconventionsattribute)
* [UseWebApiOverloadingAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapioverloadingattribute)
* [UseWebApiParameterConventionsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiparameterconventionsattribute)
* [UseWebApiRoutesAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiroutesattribute)

### <a name="action-conventions"></a>Konvence akce

`UseWebApiActionConventionsAttribute` Se používá k mapování metoda HTTP pro akce na základě jejich názvu (například `Get` by mapovat `HttpGet`). Platí jenom pro akce, které nepoužívají směrováním atributů.

### <a name="overloading"></a>Přetížení

`UseWebApiOverloadingAttribute` Se používá k aplikování `WebApiOverloadingApplicationModelConvention` konvence. Touto konvencí přidá `OverloadActionConstraint` do procesu výběr akce, což omezuje akce kandidáta na ty, pro které žádost, splňuje všechny-volitelné parametry.

### <a name="parameter-conventions"></a>Parametr konvence

`UseWebApiParameterConventionsAttribute` Se používá k aplikování `WebApiParameterConventionsApplicationModelConvention` akci convention. Touto konvencí Určuje, že jednoduché typy používat jako akce parametry jsou vázány z identifikátoru URI ve výchozím nastavení, při komplexní typy jsou svázány z textu požadavku.

### <a name="routes"></a>Trasy

`UseWebApiRoutesAttribute` Ovládací prvky jestli `WebApiApplicationModelConvention` řadiče konvence platí. Když je povolené, touto konvencí se používá k přidání podpory pro [oblasti](xref:mvc/controllers/areas) na trasu.

Kromě sadu konvence, balíček kompatibility obsahuje `System.Web.Http.ApiController` základní třídy, který nahrazuje zadanému pomocí webového rozhraní API. To umožňuje řadičů zapsán pro webového rozhraní API a dědění z jeho `ApiController` postup, jak byly navrženy, když jsou spuštěné na ASP.NET MVC jádra. Tato třída základní řadič opatřen se všemi `UseWebApi*` atributy uvedené výše. `ApiController` Zpřístupní vlastnosti, metod a typy výsledků, které jsou kompatibilní s výstrahám nacházejícím se v rozhraní Web API.

## <a name="using-apiexplorer-to-document-your-app"></a>Pomocí ApiExplorer do dokumentů aplikace

Zpřístupní aplikačního modelu [ `ApiExplorer` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel) vlastnost na každé úrovni, který slouží k procházení struktury aplikace. To může být slouží jako [generování stránky nápovědy pro vaše webové rozhraní API pomocí nástroje, například Swagger](https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger). `ApiExplorer` Zpřístupňuje vlastnost `IsVisible` vlastnost, která můžete nastavit k určení, které části modelu vaše aplikace by měly být vystaveny. Můžete nakonfigurovat toto nastavení používá konvence:

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/EnableApiExplorerApplicationConvention.cs)]

Pomocí tohoto přístupu (a další konvence v případě potřeby), můžete povolit nebo zakázat rozhraní API viditelnost na všechny úrovně v rámci vaší aplikace. 
