---
title: "Syntaxe Razor stránky trasy a aplikace konvence funkce ASP.NET Core"
author: guardrex
description: "Zjistit, jak trasy a aplikací modelu zprostředkovatele konvence funkce vám pomohou ovládací prvek stránky směrování, zjišťování a zpracování."
manager: wpickett
ms.author: riande
ms.date: 10/23/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/razor-pages/razor-pages-convention-features
ms.openlocfilehash: bf1c895fc972310d5541d0098226d58b8183e320
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="razor-pages-route-and-app-convention-features-in-aspnet-core"></a>Syntaxe Razor stránky trasy a aplikace konvence funkce ASP.NET Core

Podle [Luke Latham](https://github.com/guardrex)

Naučte se používat stránky trasy aplikace model zprostředkovatele konvence funkcí a k řízení stránky směrování, zjišťování a zpracování v aplikacích pro stránky Razor. Pokud potřebujete nakonfigurovat vlastní stránku trasy pro jednotlivé stránky, konfigurace směrování na stránky s [AddPageRoute konvence](#configure-a-page-route) popsané dál v tomto tématu.

Použití [ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-convention-features/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample)) prozkoumat funkce popsané v tomto tématu.

| Funkce | Ukázka ukazuje... |
| -------- | --------------------------- |
| [Konvence modelu trasy a aplikací](#add-route-and-app-model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li></ul> | Přidání šablonu trasy a záhlaví stránkám aplikace. |
| [Stránka trasy akce konvence](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Přidání šablonu trasy na stránky ve složce a na jednu stránku. |
| [Stránka modelu akce konvence](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (filtru třídu, výrazu lambda nebo filtr factory)</li></ul> | Přidání hlavičky na stránky ve složce, přidání hlavičky na jednu stránku a konfigurací [rodiny filtru](xref:mvc/controllers/filters#ifilterfactory) přidat hlavičku stránkám aplikace. |
| [Výchozí stránka aplikace model zprostředkovatele](#replace-the-default-page-app-model-provider) | Nahrazení výchozího zprostředkovatele modelu stránky Chcete-li změnit konvence pro pojmenování obslužné rutiny. |

## <a name="add-route-and-app-model-conventions"></a>Přidat trasy a aplikaci konvence modelu

Přidat delegáta pro [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) přidat trasy a aplikaci konvence modelu, které platí pro stránky Razor.

**Přidat na všechny stránky modelu konvencí směrování**

Použití [konvence](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) vytvořit a přidat [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) do kolekce [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) instancí, které se používají při modelu trasy a stránky konstrukce.

Ukázková aplikace přidá `{globalTemplate?}` šablonu trasy na všechny stránky v aplikaci:

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

> [!NOTE]
> `Order` Vlastnost `AttributeRouteModel` je nastaven na `0` (nula). Tím se zajistí, že tato šablona je daná prioritu pro první pozici hodnoty data trasy, pokud je zadána hodnota jedné směrovací. Příklad: Ukázka přidá `{aboutTemplate?}` šablonu trasy později v tomto tématu. `{aboutTemplate?}` Šablony je uveden `Order` z `1`. Když je stránka o požádali v `/About/RouteDataValue`, "RouteDataValue" je načten do `RouteData.Values["globalTemplate"]` (`Order = 0`) a ne `RouteData.Values["aboutTemplate"]` (`Order = 1`) z důvodu nastavení `Order` vlastnost.

*Startup.cs*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet1)]

Požadavek vzorku o stránku v `localhost:5000/About/GlobalRouteValue` a zkontrolovat výsledek:

![O stránku žádá s segment směrování GlobalRouteValue. Na vykreslené stránce ukazuje, že je hodnota dat trasy zachyceny v metodě OnGet stránky.](razor-pages-convention-features/_static/about-page-global-template.png)

**Přidat modelu konvencí aplikace na všechny stránky**

Použití [konvence](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) vytvořit a přidat [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) do kolekce [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) instancí, které se používají při trasy a stránky vytváření modelu.

K předvedení tato a další konvence později v tomto tématu, obsahuje ukázkovou aplikaci `AddHeaderAttribute` třídy. Přijímá konstruktoru třídy `name` řetězec a `values` pole řetězců. Tyto hodnoty jsou použity v jeho `OnResultExecuting` metodu a nastavit tak hlavičky odpovědi. Úplné třídy se zobrazí v [stránky modelu akce konvence](#page-model-action-conventions) dále v tomto tématu.

Použití ukázkové aplikace `AddHeaderAttribute` třída přidat hlavičku, `GlobalHeader`, na všechny stránky v aplikaci:

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet2)]

Požadavek vzorku o stránku v `localhost:5000/About` a zkontrolovat hlavičky zobrazíte výsledek:

![Hlavičky odpovědi stránky o ukazují, že byl přidán GlobalHeader.](razor-pages-convention-features/_static/about-page-global-header.png)

## <a name="page-route-action-conventions"></a>Stránka trasy akce konvence

Výchozího zprostředkovatele modelu trasy, která je odvozena z [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider) vyvolá konvence, které poskytují body rozšiřitelnosti pro konfiguraci tras stránky.

**Složka trasy modelu konvencí**

Použití [AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) vytvořit a přidat [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) , vyvolá akce na [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) pro všechny stránky v rámci Zadaná složka.

Použití ukázkové aplikace `AddFolderRouteModelConvention` přidat `{otherPagesTemplate?}` šablona trasy pro stránky v *OtherPages* složky:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet3)]

> [!NOTE]
> `Order` Vlastnost `AttributeRouteModel` je nastaven na `1`. To zajistí, že šablona pro `{globalTemplate?}` (set dříve v tomto tématu) pro data trasy, která první hodnota pozice, pokud je zadána hodnota jedné směrovací přidělen s prioritou. Pokud v požadavku na stránku Page1 `/OtherPages/Page1/RouteDataValue`, "RouteDataValue" je načten do `RouteData.Values["globalTemplate"]` (`Order = 0`) a ne `RouteData.Values["otherPagesTemplate"]` (`Order = 1`) z důvodu nastavení `Order` vlastnost.

Požadavek ukázkové Page1 stránku v `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` a zkontrolovat výsledek:

![Stránka1 ve složce OtherPages se požaduje segmentu směrování GlobalRouteValue a OtherPagesRouteValue. Na vykreslené stránce ukazuje, že hodnoty dat trasy zachyceny v metodě OnGet stránky.](razor-pages-convention-features/_static/otherpages-page1-global-and-otherpages-templates.png)

**Stránka trasy modelu konvencí**

Použití [AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) vytvořit a přidat [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) , vyvolá akce na [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) pro stránku se zadaným Jméno.

Použití ukázkové aplikace `AddPageRouteModelConvention` přidat `{aboutTemplate?}` šablonu trasy na stránku o:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet4)]

> [!NOTE]
> `Order` Vlastnost `AttributeRouteModel` je nastaven na `1`. To zajistí, že šablona pro `{globalTemplate?}` (set dříve v tomto tématu) pro data trasy, která první hodnota pozice, pokud je zadána hodnota jedné směrovací přidělen s prioritou. Pokud v požadavku na stránku o `/About/RouteDataValue`, "RouteDataValue" je načten do `RouteData.Values["globalTemplate"]` (`Order = 0`) a ne `RouteData.Values["aboutTemplate"]` (`Order = 1`) z důvodu nastavení `Order` vlastnost.

Požadavek vzorku o stránku v `localhost:5000/About/GlobalRouteValue/AboutRouteValue` a zkontrolovat výsledek:

![O stránku se s segmentů směrování pro požaduje GlobalRouteValue a AboutRouteValue. Na vykreslené stránce ukazuje, že hodnoty dat trasy zachyceny v metodě OnGet stránky.](razor-pages-convention-features/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a>Konfigurace stránky trasy

Použití [AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) ke konfiguraci trasy na stránku v cestě zadané stránky. Vygenerovaný odkazy na stránce použít zadanou trasu. `AddPageRoute`používá `AddPageRouteModelConvention` k vytvoření trasy.

Ukázková aplikace vytvoří trasu k `/TheContactPage` pro *Contact.cshtml*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet5)]

Obraťte se na stránce také k dispozici na adrese `/Contact` přes jeho výchozí trasu.

Vlastní trasy ukázkové aplikace, obraťte se na stránku umožňuje volitelný `text` segment směrování (`{text?}`). Stránka taky obsahuje této volitelné segmentu v jeho `@page` direktivy v případě, že návštěvník přistupuje na stránce na jeho `/Contact` trasy:

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Contact.cshtml?highlight=1)]

Všimněte si, že adresa URL vygenerované **kontaktujte** odkaz na vykreslené stránce odráží aktualizované trasy:

![Obraťte se na odkaz na aplikaci na navigačním panelu ukázkové](razor-pages-convention-features/_static/contact-link.png)

![Kontroly obraťte se na odkaz v kódu HTML vykreslené označuje, že odkaz href je nastavená na "/ TheContactPage.](razor-pages-convention-features/_static/contact-link-source.png)

Navštivte stránku obraťte se na buď jeho obyčejnou trasu `/Contact`, nebo vlastní trasy `/TheContactPage`. Pokud zadáte další `text` segmentu trasy, stránce se zobrazuje segmentu kódu HTML zadat:

![Příklad prohlížeče Edge poskytnutí segment směrování volitelné 'text' z 'TextValue' v adrese URL. Na vykreslené stránce zobrazuje hodnota segmentu 'text'.](razor-pages-convention-features/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>Stránka modelu akce konvence

Výchozí zprostředkovatel modelu stránky, který implementuje [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider) vyvolá konvence, které poskytují body rozšiřitelnosti pro konfiguraci stránky modelů. Tyto konvence jsou užitečné při vytváření a úpravy stránky zjišťování a zpracování funkce.

Příklady v této části najdete ukázkové aplikace používá `AddHeaderAttribute` třída, která je [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute), které se týkají hlavičky odpovědi:

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/AddHeader.cs?name=snippet1)]

Ukázku pomocí konvencí, ukazuje, jak do atribut se vztahují na všechny stránky ve složce a na jednu stránku.

**Složky aplikace modelu konvencí**

Použití [AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) vytvořit a přidat [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) , vyvolá akce na [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) instance pro všechny stránky v zadané složce.

Ukázka demonstruje použití `AddFolderApplicationModelConvention` přidáním záhlaví, `OtherPagesHeader`, stránkám uvnitř *OtherPages* složky aplikace:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet6)]

Požadavek ukázkové Page1 stránku v `localhost:5000/OtherPages/Page1` a zkontrolovat hlavičky zobrazíte výsledek:

![Hlavičky odpovědi OtherPages/Page1 stránky ukazují, že byl přidán OtherPagesHeader.](razor-pages-convention-features/_static/page1-otherpages-header.png)

**Stránka aplikace modelu konvencí**

Použití [AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention) vytvořit a přidat [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) , vyvolá akce na [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) pro stránku. s názvem speciifed.

Ukázka demonstruje použití `AddPageApplicationModelConvention` přidáním záhlaví, `AboutHeader`, o stránku:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet7)]

Požadavek vzorku o stránku v `localhost:5000/About` a zkontrolovat hlavičky zobrazíte výsledek:

![Hlavičky odpovědi stránky o ukazují, že byl přidán AboutHeader.](razor-pages-convention-features/_static/about-page-about-header.png)

**Konfigurovat filtr**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) nakonfiguruje zadaný filtr použít. Můžete implementovat třídu filtru, ale ukázková aplikace ukazuje, jak implementovat filtr ve výrazu lambda, které je implementované servisní jako objekt factory, který vrátí filtru:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet8)]

Model aplikace stránky se používá k ověření pro segmenty, které vést ke stránce strany Page2 na relativní cestu *OtherPages* složky. Pokud podmínka úspěšně projde, se přidá hlavičku. Pokud ne, `EmptyFilter` platí.

`EmptyFilter`je [filtr akce](xref:mvc/controllers/filters#action-filters). Vzhledem k tomu, že filtrů akce jsou ignorovat stránky Razor `EmptyFilter` ne ops tak, jak má, pokud cesta neobsahuje `OtherPages/Page2`.

Požadavek ukázkové strany Page2 stránku v `localhost:5000/OtherPages/Page2` a zkontrolovat hlavičky zobrazíte výsledek:

![OtherPagesPage2Header se přidá do odpovědi pro strany Page2.](razor-pages-convention-features/_static/page2-filter-header.png)

**Konfigurace vytváření filtru**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) nakonfiguruje zadaný objekt factory pro použití [filtry](xref:mvc/controllers/filters) na všechny stránky Razor.

Ukázková aplikace obsahuje příklady použití [filtr vytváření](xref:mvc/controllers/filters#ifilterfactory) přidáním záhlaví, `FilterFactoryHeader`, se dvěma hodnotami stránkám aplikace:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*:

[!code-csharp[Main](razor-pages-convention-features/sample/Factories/AddHeaderWithFactory.cs?name=snippet1)]

Požadavek vzorku o stránku v `localhost:5000/About` a zkontrolovat hlavičky zobrazíte výsledek:

![Hlavičky odpovědi stránky o ukazují, že byly přidány dva FilterFactoryHeader hlavičky.](razor-pages-convention-features/_static/about-page-filter-factory-header.png)

## <a name="replace-the-default-page-app-model-provider"></a>Nahradí zprostředkovatele modelu výchozí stránka aplikace

Používá stránky Razor `IPageApplicationModelProvider` rozhraní pro vytváření [DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider). Může dědit vlastnosti z výchozího zprostředkovatele modelu k poskytování logika implementaci obslužné rutiny zjišťování a zpracování. Výchozí implementace ([odkaz na zdroj](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) konvence pro zakládá *nepojmenované* a *s názvem* obslužná rutina pojmenováním, která je popsána níže.

**Výchozí metody nepojmenované obslužné rutiny**

Metody obslužné rutiny pro příkazy HTTP ("nepojmenované" Obslužná rutina metody) postupujte podle konvence: `On<HTTP verb>[Async]` (připojování `Async` je volitelné, ale doporučené pro asynchronní metody).

| Nepojmenované obslužná rutina – metoda     | Operace                      |
| -------------------------- | ------------------------------ |
| `OnGet`/`OnGetAsync`       | Inicializujte stav stránky.     |
| `OnPost`/`OnPostAsync`     | Zpracování požadavků POST.          |
| `OnDelete`/`OnDeleteAsync` | Zpracování žádosti o odstranění &#8224;. |
| `OnPut`/`OnPutAsync`       | Zpracování žádosti PUT &#8224;.    |
| `OnPatch`/`OnPatchAsync`   | Zpracovává požadavky PATCH &#8224;.  |

&#8224; Použít pro volání rozhraní API na stránku.

**Výchozí název metody obslužné rutiny**

Obslužná rutina metody poskytované developer ("s názvem" metody obslužné rutiny) postupujte podle podobné konvence. Název obslužné rutiny se zobrazí po příkazu HTTP, nebo mezi příkaz protokolu HTTP a `Async`: `On<HTTP verb><handler name>[Async]` (připojování `Async` je volitelné, ale doporučené pro asynchronní metody). Například metody, které zpracování zpráv, může trvat pojmenování uvedené v následující tabulce.

| Příklad s názvem obslužná rutina – metoda             | Příklad operace        |
| ---------------------------------------- | ------------------------ |
| `OnGetMessage`/`OnGetMessageAsync`       | Získáte zprávu.        |
| `OnPostMessage`/`OnPostMessageAsync`     | Odeslat zprávu.          |
| `OnDeleteMessage`/`OnDeleteMessageAsync` | Odstraňte zprávu &#8224;. |
| `OnPutMessage`/`OnPutMessageAsync`       | UVEĎTE zprávu &#8224;.    |
| `OnPatchMessage`/`OnPatchMessageAsync`   | Oprava zprávu &#8224;.  |

&#8224; Použít pro volání rozhraní API na stránku.

**Přizpůsobení názvů obslužná rutina – metoda**

Předpokládejme, že chcete změnit způsob, jakým jsou pojmenované metody pojmenované a nepojmenované obslužná rutina. Alternativní schéma pojmenování je chcete zabránit spouštění názvy metoda s "Na" a použít první segment word k určení příkazu HTTP. Můžete provést další změny, jako třeba převádění pomocí příkazů pro odstranění, PUT a oprava na hodnotu POST. Tento systém poskytuje názvy metody uvedené v následující tabulce.

| Obslužná rutina – metoda                       | Operace                      |
| ------------------------------------ | ------------------------------ |
| `Get`                                | Inicializujte stav stránky.     |
| `Post`/`PostAsync`                   | Zpracování požadavků POST.          |
| `Delete`/`DeleteAsync`               | Zpracování žádosti o odstranění &#8224;. |
| `Put`/`PutAsync`                     | Zpracování žádosti PUT &#8224;.    |
| `Patch`/`PatchAsync`                 | Zpracovává požadavky PATCH &#8224;.  |
| `GetMessage`                         | Získáte zprávu.              |
| `PostMessage`/`PostMessageAsync`     | Odeslat zprávu.                |
| `DeleteMessage`/`DeleteMessageAsync` | Odeslat zprávu odstranit.      |
| `PutMessage`/`PutMessageAsync`       | Odeslat zprávu pro umístění.         |
| `PatchMessage`/`PatchMessageAsync`   | Odeslat zprávu na opravu.       |

&#8224; Použít pro volání rozhraní API na stránku.

K vytvoření tohoto systému, dědí `DefaultPageApplicationModelProvider` třídy a přepsat [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel) metoda zadat vlastní logiky pro vyřešení [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) názvy obslužné rutiny. Ukázková aplikace se dozvíte, jak je to v jeho `CustomPageApplicationModelProvider` třídy:

[!code-csharp[Main](razor-pages-convention-features/sample/CustomPageApplicationModelProvider.cs?name=snippet1&highlight=1-2,45-46,64-68,78-85,87,92,106)]

Mezi nejdůležitější funkce třídy patří:

* Třídy dědí z `DefaultPageApplicationModelProvider`.
* `TryParseHandlerMethod` Zpracuje obslužná rutina k určení akce HTTP (`httpMethod`) a název pojmenovaného obslužné rutiny (`handlerName`) při vytváření `PageHandlerModel`.
  * `Async` Operátory se ignoruje, pokud je k dispozici.
  * Malá a velká písmena se používá k analýze příkaz protokolu HTTP z název metody.
  * Pokud název metody (bez `Async`) je stejný jako název příkaz HTTP, není žádná pojmenované obslužná rutina. `handlerName` Je nastaven na `null`, a název metody `Get`, `Post`, `Delete`, `Put`, nebo `Patch`.
  * Pokud název metody (bez `Async`) je delší než název příkaz HTTP, je obslužnou rutinu s názvem. `handlerName` Je nastaven na `<method name (less 'Async', if present)>`. Například "GetMessage" a "GetMessageAsync" yield obslužná rutina název "GetMessage".
  * Operace DELETE a PUT, PATCH HTTP jsou převedeny na hodnotu POST.

Zaregistrovat `CustomPageApplicationModelProvider` v `Startup` třídy:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet10)]

Model stránky v *Index.cshtml.cs* ukazuje, jak jsou změnit zásady vytváření názvů metoda obyčejnou obslužné rutiny pro stránky v aplikaci. Odeberou se běžném provozu "Na" předponu pojmenování použít s stránky Razor. Metoda, která inicializuje stav stránky teď jmenuje `Get`. Můžete zobrazit touto konvencí použít v celé aplikaci v případě, že otevřete kteréhokoli modelu stránky pro všechny stránky.

Každý z jiné metody spustit pomocí příkazu HTTP, který popisuje jeho zpracování. Tyto dvě metody, které začínají `Delete` by za normálních okolností považována za odstranit příkaz HTTP, ale logika `TryParseHandlerMethod` explicitně na hodnotu POST nastaví příkaz pro obě obslužné rutiny.

Všimněte si, že `Async` je volitelný mezi `DeleteAllMessages` a `DeleteMessageAsync`. Jsou oba asynchronní metody, ale můžete použít `Async` přípony nebo není; doporučujeme, abyste provedli. `DeleteAllMessages`Tady je použita pro demonstrační účely, ale doporučujeme pojmenovat tato metoda `DeleteAllMessagesAsync`. Neovlivní zpracování tohoto příkladu implementace, ale pomocí `Async` přípony volání na fakt, že se jedná o asynchronní metodu.

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/Index.cshtml.cs?name=snippet1&highlight=1,6,16,29)]

Poznamenejte si názvy obslužná rutina součástí *Index.cshtml* odpovídat `DeleteAllMessages` a `DeleteMessageAsync` metody obslužné rutiny:

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Index.cshtml?range=29-60&highlight=7-8,24-25)]

`Async`v názvu metoda obslužná rutina `DeleteMessageAsync` je rozdělen odhlašování pomocí `TryParseHandlerMethod` pro párování obslužné rutiny požadavku POST metodě. `asp-page-handler` Název `DeleteMessage` odpovídá metodu obslužné rutiny `DeleteMessageAsync`.

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>Filtry MVC a filtr stránek (IPageFilter)

MVC [filtrů Akce](xref:mvc/controllers/filters#action-filters) ignorují pomocí stránky Razor, protože stránky Razor použít metody obslužné rutiny. Jiné typy filtrů MVC jsou k dispozici pro použití: [autorizace](xref:mvc/controllers/filters#authorization-filters), [výjimka](xref:mvc/controllers/filters#exception-filters), [prostředků](xref:mvc/controllers/filters#resource-filters), a [výsledek](xref:mvc/controllers/filters#result-filters). Další informace najdete v tématu [filtry](xref:mvc/controllers/filters) tématu.

Filtr stránek ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) je filtr, který se vztahuje na stránky Razor. Ho obklopuje provádění metody obslužná rutina stránky. Umožňuje zpracovat vlastní kód ve fázích provádění metoda obslužná rutina stránky. Tady je příklad z ukázkové aplikace:

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/ReplaceRouteValueFilterAttribute.cs?name=snippet1)]

Tento filtr vyhledává `globalTemplate` směrování hodnotu "TriggerValue" a záměna v "Zastaralá".

`ReplaceRouteValueFilter` Atribut lze použít přímo na `PageModel`:

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/OtherPages/Page3.cshtml.cs?range=10-12&highlight=1)]

Požadavek stránku Page3 z ukázkové aplikace s v `localhost:5000/OtherPages/Page3/TriggerValue`. Všimněte si, jak filtr nahradí hodnotu trasy:

![Žádost o OtherPages nebo Page3 s výsledků segment směrování TriggerValue ve filtru nahrazení hodnoty trasy zastaralá.](razor-pages-convention-features/_static/otherpages-page3-filter-replacement-value.png)

## <a name="see-also"></a>Viz také

* [Konvence autorizace stránky Razor](xref:security/authorization/razor-pages-authorization)
