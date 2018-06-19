---
title: Oblasti v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak jsou oblasti o architektuře ASP.NET MVC funkci sloužící k organizování související funkce do skupiny jako samostatný obor názvů (pro směrování) a strukturu složek (pro zobrazení).
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/areas
ms.openlocfilehash: 61527eb350b5aba9cb37b1de5acdeae1287bf073
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
ms.locfileid: "30072718"
---
# <a name="areas-in-aspnet-core"></a>Oblasti v ASP.NET Core

Podle [Dhananjay Kumar](https://twitter.com/debug_mode) a [Rick Anderson](https://twitter.com/RickAndMSFT)

Oblasti se o funkci ASP.NET MVC sloužící k organizování související funkce do skupiny jako samostatný obor názvů (pro směrování) a strukturu složek (pro zobrazení). Použití oblastí vytvoří hierarchii pro účely směrování přidáním jiný parametr trasy, `area`do `controller` a `action`.

Oblasti poskytují způsob, jak oddílu velké ASP.NET Core MVC webové aplikace do menších funkční seskupení. Oblast je efektivně strukturu MVC uvnitř aplikace. V projektu MVC logické součásti jako Model, Kontroleru a zobrazení jsou uchovány v různých složkách a konvence pojmenování aplikace MVC používá k vytvoření vztahu mezi těmito součástmi. Pro velké aplikace může být výhodné oddílu aplikace na samostatné vysoké úrovni oblasti funkcí. Pro instanci elektronické obchodování aplikace s více organizačních jednotek, jako je například checkout, fakturace a vyhledávání atd. Každý z těchto jednotek mají své vlastní logickou součástí zobrazení, řadiče a modely. V tomto scénáři můžete v oblasti fyzicky oddílu komponenty obchodní ve stejném projektu.

Oblast může být definován jako menší funkční jednotky v projektu ASP.NET MVC jádra s vlastní sadou řadiče, zobrazení a modely.

Zvažte použití oblastí v MVC při projektu:

* Aplikace se provádí více vysoké úrovně funkčnosti komponent, které by měl být oddělený logicky

* Chcete oddílu projektu MVC tak, aby každý funkční oblast bylo možné pracovat nezávisle

Oblast funkce:

* ASP.NET MVC základní aplikaci může mít libovolný počet oblastí

* Každou oblast má svou vlastní řadiče, modely a zobrazení

* Umožňuje uspořádat do několika komponent vysoké úrovně, které bylo možné pracovat nezávisle rozsáhlých projektů MVC

* Podporuje více řadiče se stejným názvem - tak dlouho, dokud mají různé *oblastí*

Podívejme se na příklad znázorňující způsob vytváření a použít oblasti. Řekněme, že máte aplikaci ze storu, která má dva odlišné seskupení kontrolery a zobrazení: produktů a služeb. Složka obvykle struktury pro, že pomocí MVC oblasti vypadá níže:

* název projektu

  * Oblasti

    * Produkty

      * Kontrolery

        * HomeController.cs

        * ManageController.cs

      * zobrazení

        * Domů

          * Index.cshtml

        * Správa

          * Index.cshtml

    * Služby

      * Kontrolery

        * HomeController.cs

      * zobrazení

        * Domů

          * Index.cshtml

Když se pokusí MVC pro vykreslení zobrazení v oblasti, ve výchozím nastavení, pokusí se hledat v následujících umístěních:

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

Jedná se o výchozí umístění, které je možné změnit prostřednictvím `AreaViewLocationFormats` na `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.

Například v níže uvedeného kódu místo nutnosti název složky jako "Oblasti", byla změněna na "Kategorií".

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

Poznámka je, že strukturu *zobrazení* složka je jenom jeden, která je považována za důležité sem a obsah ostatních složek, jako jsou *řadiče* a *modely* nemá **není** vás. Například nemusí mít *řadiče* a *modely* složku v všechny. Toto funguje, protože obsah *řadiče* a *modely* je pouze kód, který získá zkompilovat do ve formátu .dll, kde je jako obsah *zobrazení* není dokud požadavek na který zobrazení byly provedeny.

Jakmile hierarchii složek, které jste definovali, budete muset MVC říct, že každý řadič je přiřazený k oblasti. Provedete to tak architekturu názvu řadiče s `[Area]` atribut.

```csharp
...
   namespace MyStore.Areas.Products.Controllers
   {
       [Area("Products")]
       public class HomeController : Controller
       {
           // GET: /Products/Home/Index
           public IActionResult Index()
           {
               return View();
           }

           // GET: /Products/Home/Create
           public IActionResult Create()
           {
               return View();
           }
       }
   }
   ```

Nastavte definici trasy, která funguje s vaší nově vytvořený oblasti. [Trasy, která má akce kontroleru](routing.md) článek obsahuje podrobnosti o tom, jak vytvořit definicí cesty, včetně použití konvenční trasy a trasy atributů. V tomto příkladu použijeme konvenční trasy. Chcete-li to provést, otevřete *Startup.cs* soubor a upravit přidáním `areaRoute` s názvem definice trasy níže.

```csharp
...
   app.UseMvc(routes =>
   {
     routes.MapRoute(
         name: "areaRoute",
         template: "{area:exists}/{controller=Home}/{action=Index}/{id?}");

     routes.MapRoute(
         name: "default",
         template: "{controller=Home}/{action=Index}/{id?}");
   });
   ```

Procházení k `http://<yourApp>/products`, `Index` metody akce `HomeController` v `Products` oblasti bude volána.

## <a name="link-generation"></a>Generování odkazů

* Generování odkazů z akce v rámci oblast na základě řadiče jiné akce v rámci stejného řadiče.

  Řekněme, že je cesta aktuální žádosti jako `/Products/Home/Create`

  HtmlHelper syntaxe: `@Html.ActionLink("Go to Product's Home Page", "Index")`

  TagHelper syntaxe: `<a asp-action="Index">Go to Product's Home Page</a>`

  Všimněte si, že jsme nemusí zadat hodnoty 'oblastí' a 'controller' tady jsou již k dispozici v kontextu aktuálního požadavku. Tyto druhy hodnot, se nazývají `ambient` hodnoty.

* Generování odkazů z akce v rámci oblast na základě řadiče další akci na jiném řadiči

  Řekněme, že je cesta aktuální žádosti jako `/Products/Home/Create`

  HtmlHelper syntaxe: `@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`

  TagHelper syntaxe: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`

  Všimněte si, tady je použita hodnota vedlejším oblasti, ale výše je výslovně zadána hodnota 'controller'.

* Generování odkazů z akce v rámci oblasti řadiče další akci na základě různých řadiče a jiné oblasti.

  Řekněme, že je cesta aktuální žádosti jako `/Products/Home/Create`

  HtmlHelper syntaxe: `@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`

  TagHelper syntaxe: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`

  Všimněte si, že tady jsou použity žádné vedlejším hodnoty.

* Generování odkazů z akce v rámci řadič oblast na základě jinou akci na jiném řadiči a **není** v oblasti.

  HtmlHelper syntaxe: `@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`

  TagHelper syntaxe: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`

  Vzhledem k tomu, že chcete generovat odkazy na jiné oblasti na základě akce kontroleru, jsme prázdný vedlejším hodnota "plochu" sem.

## <a name="publishing-areas"></a>Publikování oblastí

Všechny `*.cshtml` a `wwwroot/**` soubory jsou publikovány do výstupního při `<Project Sdk="Microsoft.NET.Sdk.Web">` je součástí *.csproj* souboru.
