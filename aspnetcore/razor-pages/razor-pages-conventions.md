---
title: Konvence směrování a aplikačních stránky Razor v ASP.NET Core
author: guardrex
description: Zjistěte, jak směrování a aplikační konvence zprostředkovatele modelu můžete ovládací prvek stránky směrování, zjišťování a zpracování.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 04/12/2018
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: 5a5d580b4260767e411571ccacc19d6e8fe12559
ms.sourcegitcommit: 028ad28c546de706ace98066c76774de33e4ad20
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/08/2018
ms.locfileid: "39655373"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a>Konvence směrování a aplikačních stránky Razor v ASP.NET Core

Podle [Luke Latham](https://github.com/guardrex)

Další informace o použití stránky [směrování a aplikační model poskytovatele konvence](xref:mvc/controllers/application-model#conventions) řídit směrování stránky, zjišťování a zpracování v aplikacích pro stránky Razor.

Pokud potřebujete nakonfigurovat vlastní stránku trasy pro jednotlivé stránky, konfigurace směrování pro ně [AddPageRoute konvence](#configure-a-page-route) je popsáno dále v tomto tématu.

Zadejte trasy stránku, přidat segmenty směrování nebo parametry trasu, můžete na stránce `@page` směrnice. Další informace najdete v tématu [trasy vlastní](xref:razor-pages/index#custom-routes).

Existují vyhrazených slov, která nejde použít jako segmenty směrování nebo názvy parametrů. Další informace najdete v tématu [směrování: vyhrazené názvy směrování](xref:fundamentals/routing#reserved-routing-names).

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample))

::: moniker range="= aspnetcore-2.0"

| Scénář | Ukázce... |
| -------- | --------------------------- |
| [Vytváření modelu](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li></ul> | Přidáte šablonu trasy a záhlaví stránek vaší aplikace. |
| [Stránka trasy akce konvence](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Přidáte šablonu trasy na stránky ve složce a jednu stránku. |
| [Konvence akce modelu stránky](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (třídy filtru, výraz lambda nebo objekt pro vytváření filtru)</li></ul> | Přidat záhlaví stránky ve složce, přidat hlavičku do jediné stránce a nakonfigurovat [filtr factory](xref:mvc/controllers/filters#ifilterfactory) přidat záhlaví stránek vaší aplikace. |
| [Výchozího zprostředkovatele modelu stránky aplikace](#replace-the-default-page-app-model-provider) | Nahraďte výchozího zprostředkovatele modelu stránku Chcete-li změnit konvence pro názvy obslužných rutin. |

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

| Scénář | Ukázce... |
| -------- | --------------------------- |
| [Vytváření modelu](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li><li>IPageHandlerModelConvention</li></ul> | Přidáte šablonu trasy a záhlaví stránek vaší aplikace. |
| [Stránka trasy akce konvence](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Přidáte šablonu trasy na stránky ve složce a jednu stránku. |
| [Konvence akce modelu stránky](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (třídy filtru, výraz lambda nebo objekt pro vytváření filtru)</li></ul> | Přidat záhlaví stránky ve složce, přidat hlavičku do jediné stránce a nakonfigurovat [filtr factory](xref:mvc/controllers/filters#ifilterfactory) přidat záhlaví stránek vaší aplikace. |
| [Výchozího zprostředkovatele modelu stránky aplikace](#replace-the-default-page-app-model-provider) | Nahraďte výchozího zprostředkovatele modelu stránku Chcete-li změnit konvence pro názvy obslužných rutin. |

::: moniker-end

Vytváření stránek Razor přidávají a konfigurovat pomocí [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) metodu rozšíření k [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc) na kolekci služby v `Startup` třídy. Následující příklady konvence jsou vysvětleny dále v tomto tématu:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
            {
                options.Conventions.Add( ... );
                options.Conventions.AddFolderRouteModelConvention("/OtherPages", model => { ... });
                options.Conventions.AddPageRouteModelConvention("/About", model => { ... });
                options.Conventions.AddPageRoute("/Contact", "TheContactPage/{text?}");
                options.Conventions.AddFolderApplicationModelConvention("/OtherPages", model => { ... });
                options.Conventions.AddPageApplicationModelConvention("/About", model => { ... });
                options.Conventions.ConfigureFilter(model => { ... });
                options.Conventions.ConfigureFilter( ... );
            });
}
```

## <a name="model-conventions"></a>Vytváření modelu

Přidat delegáta pro [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) přidat [modelu konvencí](xref:mvc/controllers/application-model#conventions) , která platí pro stránky Razor.

**Přidání vytváření modelu trasu pro všechny stránky**

Použití [konvence](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) vytvořit a přidat [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) ke kolekci [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) instancí, které se použijí během model trasy stránky konstrukce.

Ukázková aplikace přidá `{globalTemplate?}` šablona trasy pro všechny stránky v aplikaci:

[!code-csharp[](razor-pages-conventions/sample/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

> [!NOTE]
> `Order` Vlastnost `AttributeRouteModel` je nastavena na `-1`. Tím se zajistí, že tato šablona je přiřazena prioritu pro první pozice hodnot dat trasy, když je zadaná hodnota jednu trasu a také by to mají přednost před automaticky vygenerované stránky Razor trasy. Například ukázka přidá `{aboutTemplate?}` šablonu trasy později v tomto tématu. `{aboutTemplate?}` Šablony je uveden `Order` z `1`. Když se na stránce o požaduje za `/About/RouteDataValue`, "RouteDataValue" je načten do `RouteData.Values["globalTemplate"]` (`Order = -1`) a není `RouteData.Values["aboutTemplate"]` (`Order = 1`) z důvodu nastavení `Order` vlastnost.

Možnosti stránky Razor, jako je například přidávání [konvence](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions), se přidají, když MVC se přidá do kolekce služby `Startup.ConfigureServices`. Příklad najdete v tématu [ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/sample/).

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet1)]

Žádosti o stránku ukázky na `localhost:5000/About/GlobalRouteValue` a zkontrolujte výsledek:

![Na stránce o žádá s segment GlobalRouteValue trasy. Na vykreslené stránce se zobrazí, že hodnota data trasy je zachycena v metodě OnGet stránky.](razor-pages-conventions/_static/about-page-global-template.png)

**Přidejte aplikaci modelu konvence pro všechny stránky**

Použití [konvence](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) vytvořit a přidat [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) ke kolekci [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) instancí, které se použijí během model stránky v aplikaci konstrukce.

Abychom si předvedli to a jiné konvence později v tomto tématu, obsahuje ukázkovou aplikaci `AddHeaderAttribute` třídy. Konstruktor třídy přijímá `name` řetězec a `values` pole řetězců. Tyto hodnoty jsou použity v jeho `OnResultExecuting` metoda nastavit hlavičku odpovědi. Úplné třídy je zobrazena ve [stránce modelu akce konvence](#page-model-action-conventions) dále v tomto tématu.

Tato ukázková aplikace používá `AddHeaderAttribute` třídy přidat záhlaví `GlobalHeader`, na všechny stránky v aplikaci:

[!code-csharp[](razor-pages-conventions/sample/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet2)]

Žádosti o stránku ukázky na `localhost:5000/About` a zkontrolujte záhlaví, abyste viděli výsledek:

![Hlavičky odpovědi na stránce o ukazují, že byly přidány GlobalHeader.](razor-pages-conventions/_static/about-page-global-header.png)

::: moniker range=">= aspnetcore-2.1"
**Přidat konvence model obslužné rutiny pro všechny stránky**

Použití [konvence](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) vytvořit a přidat [IPageHandlerModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipagehandlermodelconvention) ke kolekci [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) instancí, které se použijí během model obslužné rutiny stránky konstrukce.

```csharp
public class GlobalPageHandlerModelConvention 
    : IPageHandlerModelConvention
{
    public void Apply(PageHandlerModel model)
    {
        ...
    }
}
```

`Startup.ConfigureServices`:

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add(new GlobalPageHandlerModelConvention());
        });
```
::: moniker-end

## <a name="page-route-action-conventions"></a>Stránka trasy akce konvence

Výchozího zprostředkovatele modelu trasy, která je odvozena z [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider) vyvolá smluv, které poskytují body rozšiřitelnosti pro konfiguraci tras stránky.

**Složka trasy modelu konvence**

Použití [AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) vytvořit a přidat [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) , která vyvolá akci na [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) pro všechny stránky v části Zadaná složka.

Tato ukázková aplikace používá `AddFolderRouteModelConvention` přidat `{otherPagesTemplate?}` šablonu trasy na stránky *OtherPages* složky:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet3)]

> [!NOTE]
> `Order` Vlastnost `AttributeRouteModel` je nastavena na `1`. To zajistí, že šablona pro `{globalTemplate?}` (sada výše v tomto tématu) je prioritu pro první data trasy hodnotu pozice, pokud je zadaná hodnota jednu trasu. Pokud na stránce Page1 je požadováno v `/OtherPages/Page1/RouteDataValue`, "RouteDataValue" je načten do `RouteData.Values["globalTemplate"]` (`Order = -1`) a ne `RouteData.Values["otherPagesTemplate"]` (`Order = 1`) z důvodu nastavení `Order` vlastnost.

Požadavek ukázky Page1 stránku na `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` a zkontrolujte výsledek:

![Page1 ve složce OtherPages žádá segmentu směrování GlobalRouteValue a OtherPagesRouteValue. Na vykreslené stránce ukazuje, že v metodě OnGet stránky jsou zachyceny hodnot dat trasy.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

**Stránka trasy modelu konvence**

Použití [AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) vytvořit a přidat [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) , která vyvolá akci na [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) stránky se zadaným Jméno.

Tato ukázková aplikace používá `AddPageRouteModelConvention` přidat `{aboutTemplate?}` šablonu trasy ke stránce About:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet4)]

> [!NOTE]
> `Order` Vlastnost `AttributeRouteModel` je nastavena na `1`. To zajistí, že šablona pro `{globalTemplate?}` (sada výše v tomto tématu) je prioritu pro první data trasy hodnotu pozice, pokud je zadaná hodnota jednu trasu. Pokud je na stránku o `/About/RouteDataValue`, "RouteDataValue" je načten do `RouteData.Values["globalTemplate"]` (`Order = -1`) a ne `RouteData.Values["aboutTemplate"]` (`Order = 1`) z důvodu nastavení `Order` vlastnost.

Žádosti o stránku ukázky na `localhost:5000/About/GlobalRouteValue/AboutRouteValue` a zkontrolujte výsledek:

![Stránka je požadováno se segmenty směrování pro GlobalRouteValue a AboutRouteValue. Na vykreslené stránce ukazuje, že v metodě OnGet stránky jsou zachyceny hodnot dat trasy.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a>Konfigurace stránky trasy

Použití [AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) trasy na stránku konfigurace v cestě zadané stránky. Vygenerovaný odkazy na stránce použít zadanou trasu. `AddPageRoute` používá `AddPageRouteModelConvention` k vytvoření trasy.

Ukázková aplikace vytvoří trasu k `/TheContactPage` pro *Contact.cshtml*:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet5)]

Na stránce kontaktu lze dosáhnout také za `/Contact` prostřednictvím jeho výchozí trasy.

Vlastní trasy ukázkovou aplikaci na stránku kontaktujte umožňuje volitelně `text` segment směrování (`{text?}`). Na stránce také zahrnuje tato volitelná segment v jeho `@page` direktiv v případě, že na stránce návštěvníka má přístup k jeho `/Contact` trasy:

[!code-cshtml[](razor-pages-conventions/sample/Pages/Contact.cshtml?highlight=1)]

Všimněte si, že adresa URL se vygeneruje pro **kontakt** odkaz na vykreslené stránce odráží aktualizovaný trasy:

![Ukázkový odkaz na aplikaci obraťte se na navigačním panelu](razor-pages-conventions/_static/contact-link.png)

![Kontrola odkazu kontakt v zobrazený HTML označuje, že je odkaz nastavený na "/ TheContactPage.](razor-pages-conventions/_static/contact-link-source.png)

Navštivte stránku nástroje kontakt na buď jeho běžný trasu `/Contact`, nebo vlastní trasy `/TheContactPage`. Pokud zadáte další `text` trasy segmentu, na stránce se zobrazí segment kódovaný jazykem HTML, že zadáte:

![Příklad prohlížeče Edge poskytnutí segment trasy volitelné 'text' 'TextValue"v adrese URL. Na vykreslené stránce zobrazuje hodnota 'text' segmentu.](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>Konvence akce modelu stránky

Výchozího zprostředkovatele modelu stránky, která implementuje [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider) vyvolá smluv, které poskytují body rozšiřitelnosti pro konfiguraci stránky modely. Tato konvence jsou užitečné při vytváření a úprava stránky zjišťování a scénáře zpracování.

Příklady v této části, se ukázková aplikace používá `AddHeaderAttribute` třídy, která je [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute), aplikovaná hlavičky odpovědi:

[!code-csharp[](razor-pages-conventions/sample/Filters/AddHeader.cs?name=snippet1)]

Pomocí konvencí vzorek ukazuje, jak použít atribut na všechny stránky ve složce a jednu stránku.

**Složky aplikace modelu konvence**

Použití [AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) vytvořit a přidat [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) , která vyvolá akci na [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) instance pro všechny stránky v zadané složce.

Ukázka demonstruje použití `AddFolderApplicationModelConvention` přidáním záhlaví `OtherPagesHeader`, na stránky uvnitř *OtherPages* složku aplikace:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet6)]

Požadavek ukázky Page1 stránku na `localhost:5000/OtherPages/Page1` a zkontrolujte záhlaví, abyste viděli výsledek:

![Hlavičky odpovědi stránky OtherPages/Page1 ukazují, že byla přidána OtherPagesHeader.](razor-pages-conventions/_static/page1-otherpages-header.png)

**Stránka aplikace modelu konvence**

Použití [AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention) vytvořit a přidat [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) , která vyvolá akci na [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) stránky se zadaným názvem.

Ukázka demonstruje použití `AddPageApplicationModelConvention` přidáním záhlaví `AboutHeader`, ke stránce About:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet7)]

Žádosti o stránku ukázky na `localhost:5000/About` a zkontrolujte záhlaví, abyste viděli výsledek:

![Hlavičky odpovědi na stránce o ukazují, že byly přidány AboutHeader.](razor-pages-conventions/_static/about-page-about-header.png)

**Konfigurace filtru**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) nakonfiguruje zadaný filtr použít. Můžete implementovat filtr třídy, ale ukázková aplikace ukazuje, jak implementovat filtr ve výrazu lambda, který je implementován jako objekt factory, který vrátí filtr pozadí:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet8)]

Model stránky aplikace se používá ke kontrole relativní cestu pro segmenty, které vedou na stránku strany Page2 *OtherPages* složky. Pokud bude podmínka splněna, se přidá hlavičku. Pokud ne, `EmptyFilter` platí.

`EmptyFilter` je [filtr akce](xref:mvc/controllers/filters#action-filters). Protože filtrů Akce ignorovány pomocí Razor Pages `EmptyFilter` operace tak, jak má, pokud cesta neobsahuje `OtherPages/Page2`.

Požadavek ukázky strany Page2 stránku na `localhost:5000/OtherPages/Page2` a zkontrolujte záhlaví, abyste viděli výsledek:

![OtherPagesPage2Header se přidá do odpovědi pro strany Page2.](razor-pages-conventions/_static/page2-filter-header.png)

**Konfigurace továrny filtru**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) nastaví zadaný objekt factory použít [filtry](xref:mvc/controllers/filters) na všechny stránky Razor.

Ukázkovou aplikaci poskytuje příklad použití [filtr factory](xref:mvc/controllers/filters#ifilterfactory) přidáním záhlaví `FilterFactoryHeader`, se dvě hodnoty a stránek vaší aplikace:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*:

[!code-csharp[](razor-pages-conventions/sample/Factories/AddHeaderWithFactory.cs?name=snippet1)]

Žádosti o stránku ukázky na `localhost:5000/About` a zkontrolujte záhlaví, abyste viděli výsledek:

![Hlavičky odpovědi na stránce o ukazují, že byly přidány dva FilterFactoryHeader záhlaví.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="replace-the-default-page-app-model-provider"></a>Nahraďte stránky výchozího zprostředkovatele modelu aplikace

Používá Razor Pages `IPageApplicationModelProvider` rozhraní pro vytváření [DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider). Může dědit z výchozího zprostředkovatele modelu k poskytování logiky implementace obslužné rutiny zjišťování a zpracování. Výchozí implementace ([zdroj odkazu](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) vytváří konvence pro *nepojmenované* a *s názvem* rutiny pojmenování, který je popsán níže.

**Výchozí nepojmenované metody obslužné rutiny**

Metody obslužné rutiny pro příkazy HTTP ("nepojmenované" obslužnou rutinu metody) postupujte podle úmluvy: `On<HTTP verb>[Async]` (připojování `Async` je volitelné, ale doporučený pro asynchronní metody).

| Metoda nepojmenované obslužné rutiny     | Operace                      |
| -------------------------- | ------------------------------ |
| `OnGet`/`OnGetAsync`       | Inicializace stavu stránky.     |
| `OnPost`/`OnPostAsync`     | Zpracování požadavků POST.          |
| `OnDelete`/`OnDeleteAsync` | Zpracování požadavků DELETE&#8224;. |
| `OnPut`/`OnPutAsync`       | Zpracování požadavků PUT&#8224;.    |
| `OnPatch`/`OnPatchAsync`   | Zpracování žádosti PATCH&#8224;.  |

&#8224;Používá se pro uskutečňování volání rozhraní API na stránku.

**Výchozí název metody obslužné rutiny**

Obslužná rutina metody poskytované objektem developer ("s názvem" metody obslužné rutiny) podle podobných konvence. Název obslužné rutiny se zobrazí po příkazu HTTP, nebo mezi příkaz protokolu HTTP a `Async`: `On<HTTP verb><handler name>[Async]` (připojování `Async` je volitelné, ale doporučený pro asynchronní metody). Metody, které zpracovávají zprávy může například trvat pojmenování je znázorněno v následující tabulce.

| Příklad s názvem metody obslužné rutiny             | Příklad operace        |
| ---------------------------------------- | ------------------------ |
| `OnGetMessage`/`OnGetMessageAsync`       | Získáte zprávu.        |
| `OnPostMessage`/`OnPostMessageAsync`     | ODESLÁNÍ zprávy.          |
| `OnDeleteMessage`/`OnDeleteMessageAsync` | ODSTRANĚNÍ zprávy&#8224;. |
| `OnPutMessage`/`OnPutMessageAsync`       | ZAŘAZENÍ zprávy&#8224;.    |
| `OnPatchMessage`/`OnPatchMessageAsync`   | Oprava zprávu&#8224;.  |

&#8224;Používá se pro uskutečňování volání rozhraní API na stránku.

**Přizpůsobení názvů metoda obslužné rutiny**

Předpokládejme, že budete chtít změnit způsob, jakým jsou pojmenovány pojmenované a nepojmenované obslužné rutiny. Alternativní schéma pojmenování je vyhnout se spouštění názvy metod s "On" a použít první segment Wordu k určení, příkaz HTTP. Můžete provést další změny, jako je například převod příkazy pro odstranění, PUT a PATCH příspěvek. Takové schéma obsahuje názvy metod, které jsou uvedeny v následující tabulce.

| Metoda obslužné rutiny                       | Operace                      |
| ------------------------------------ | ------------------------------ |
| `Get`                                | Inicializace stavu stránky.     |
| `Post`/`PostAsync`                   | Zpracování požadavků POST.          |
| `Delete`/`DeleteAsync`               | Zpracování požadavků DELETE&#8224;. |
| `Put`/`PutAsync`                     | Zpracování požadavků PUT&#8224;.    |
| `Patch`/`PatchAsync`                 | Zpracování žádosti PATCH&#8224;.  |
| `GetMessage`                         | Získáte zprávu.              |
| `PostMessage`/`PostMessageAsync`     | ODESLÁNÍ zprávy.                |
| `DeleteMessage`/`DeleteMessageAsync` | ODESLÁNÍ zprávy na odstranit.      |
| `PutMessage`/`PutMessageAsync`       | POŠLE zprávu vložit.         |
| `PatchMessage`/`PatchMessageAsync`   | ODESLÁNÍ zprávy o opravu.       |

&#8224;Používá se pro uskutečňování volání rozhraní API na stránku.

Chcete-li vytvořit toto schéma, dědit z `DefaultPageApplicationModelProvider` třídy a přepsat [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel) metoda poskytnout vlastní logiku pro vyřešení [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) názvy obslužných rutin. Ukázková aplikace ukazuje, jak to provést jeho `CustomPageApplicationModelProvider` třídy:

[!code-csharp[](razor-pages-conventions/sample/CustomPageApplicationModelProvider.cs?name=snippet1&highlight=1-2,45-46,64-68,78-85,87,92,106)]

Mezi nejdůležitější funkce třídy patří:

* Třída dědí z `DefaultPageApplicationModelProvider`.
* `TryParseHandlerMethod` Zpracuje obslužná rutina k určení příkaz protokolu HTTP (`httpMethod`) a název pojmenované obslužné rutiny (`handlerName`) při vytváření `PageHandlerModel`.
  * `Async` Přípony se ignoruje, pokud jsou k dispozici.
  * Malých a velkých písmen slouží k analýze příkaz protokolu HTTP z názvu metody.
  * Pokud název metody (bez `Async`) je stejný jako název příkaz HTTP, neexistuje žádná obslužná rutina s názvem. `handlerName` Je nastavena na `null`, a název metody rozlišuje `Get`, `Post`, `Delete`, `Put`, nebo `Patch`.
  * Když název metody (bez `Async`) je delší než název operace HTTP je pojmenovaný obslužné rutiny. `handlerName` Je nastavena na `<method name (less 'Async', if present)>`. Například "GetMessage" a "GetMessageAsync" Zobrazit název obslužné rutiny z "GetMessage".
  * Příkazy DELETE, PUT a PATCH HTTP jsou převedeny na hodnotu POST.

Zaregistrujte `CustomPageApplicationModelProvider` v `Startup` třídy:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet10)]

Model stránky v *Index.cshtml.cs* ukazuje, jak se změní zásady vytváření názvů metoda běžné obslužné rutiny pro stránky v aplikaci. Odeberou se běžné "On" předponu názvů používat se stránkami Razor. Metoda, která inicializuje stavu stránky je teď jmenuje `Get`. Zobrazí se tato konvence při otevření modelu žádné stránky pro všechny stránky v celé aplikaci.

Každá z metod začínat příkaz HTTP, který popisuje jeho zpracování. Tyto dvě metody, které začínají `Delete` by obvykle považovány za příkazy DELETE HTTP, ale logika `TryParseHandlerMethod` explicitně nastaví příkaz na hodnotu POST pro obě obslužné rutiny.

Všimněte si, že `Async` je volitelný mezi `DeleteAllMessages` a `DeleteMessageAsync`. Jsou i asynchronní metody, ale můžete použít `Async` přípony nebo Ne, doporučujeme, abyste udělali. `DeleteAllMessages` Zde je použit pro demonstrační účely, ale doporučujeme vám, že zadáte název metody `DeleteAllMessagesAsync`. To nemá vliv na zpracování ukázková implementace, ale pomocí `Async` Příponové operátory volání mimo skutečnost, že se jedná o asynchronní metodu.

[!code-csharp[](razor-pages-conventions/sample/Pages/Index.cshtml.cs?name=snippet1&highlight=1,6,16,29)]

Poznamenejte si názvy obslužných rutin, které jsou součástí *Index.cshtml* odpovídat `DeleteAllMessages` a `DeleteMessageAsync` metody obslužné rutiny:

[!code-cshtml[](razor-pages-conventions/sample/Pages/Index.cshtml?range=29-60&highlight=7-8,24-25)]

`Async` v názvu metody obslužné rutiny `DeleteMessageAsync` je faktoroval: out `TryParseHandlerMethod` shody požadavek POST metody obslužné rutiny. `asp-page-handler` Název `DeleteMessage` je nalezena shoda s k metodě obslužné rutiny `DeleteMessageAsync`.

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>Filtry MVC a filtr stránky (IPageFilter)

MVC [filtrů Akce](xref:mvc/controllers/filters#action-filters) ignorují podle stránky Razor, protože stránky Razor pomocí metody obslužné rutiny. Jiné typy filtrů MVC jsou k dispozici pro použití: [autorizace](xref:mvc/controllers/filters#authorization-filters), [výjimka](xref:mvc/controllers/filters#exception-filters), [prostředků](xref:mvc/controllers/filters#resource-filters), a [výsledek](xref:mvc/controllers/filters#result-filters). Další informace najdete v tématu [filtry](xref:mvc/controllers/filters) tématu.

Filtr stránky ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) je filtr, který platí pro stránky Razor. Další informace najdete v tématu [metody filtrování pro Razor Pages](xref:razor-pages/filter).

## <a name="additional-resources"></a>Další zdroje

* [Konvence autorizace stránek Razor](xref:security/authorization/razor-pages-authorization)
