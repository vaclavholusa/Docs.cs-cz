---
title: Zobrazení součásti v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak zobrazit součásti jsou používány v ASP.NET Core a jejich přidání do aplikace.
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-components
ms.openlocfilehash: cdf44fc15ac64497b2589e8b7b289beb94c5b2c4
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/10/2018
ms.locfileid: "33962681"
---
# <a name="view-components-in-aspnet-core"></a>Zobrazení součásti v ASP.NET Core

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="view-components"></a>Zobrazení součásti

Zobrazení součásti jsou podobná částečné zobrazení, ale jsou mnohem silnější. Součásti zobrazení nemáte použít modelovou vazbu a pouze závisí na data zadaná při volání do ní. Tento článek byl napsané v ASP.NET MVC jádra, ale součásti zobrazení také pracovat se stránky Razor.

Součást zobrazení:

* Vykreslí bloku dat, nikoli celý odpovědi.
* Zahrnuje stejné oddělení z otázky a výhody testovatelnosti nalezen mezi řadiče a zobrazení.
* Může mít parametry a obchodní logiku.
* Obvykle volat z ke stránce rozložení.

Zobrazení součásti jsou určeny kdekoli, že máte opakovaně použitelné vykreslování logiky, která je příliš složitý pro částečné zobrazení, jako například:

* Dynamické navigační nabídky
* Značka cloudu (kde dotazuje databázi)
* Panel přihlášení
* Nákupní košík
* Nedávno publikovaných článcích
* Obsah bočním panelu na typické blogu
* Přihlášení panel, který by být vykreslen na každé stránce a zobrazovat odkazy na odhlášení nebo přihlášení, v závislosti na protokolu ve stavu uživatele

Součást zobrazení se skládá ze dvou částí: třídy (obvykle odvozené od [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)) a výsledek vrátí (obvykle zobrazení). Jako řadiče, může být součást zobrazení objektů POCO, ale Většina vývojářů chtít využívat výhod metody a vlastnosti, které jsou k dispozici odvozené z `ViewComponent`.

## <a name="creating-a-view-component"></a>Vytvoření zobrazení komponenty

Tato část obsahuje základní požadavky pro vytvoření zobrazení komponenty. Dále v tomto článku jsme budete Zkontrolujte každý krok podrobně a vytvoření zobrazení komponenty.

### <a name="the-view-component-class"></a>Třídy součástí zobrazení

Třída součásti zobrazení lze vytvořit pomocí některé z následujících:

* Odvozování z *ViewComponent*
* Architekturu třídu s `[ViewComponent]` atribut nebo odvozování od třídy, se `[ViewComponent]` atribut
* Vytvoření třídy, kde název končí příponou *ViewComponent*

Jako řadiče zobrazení součásti musí být veřejné, -nested a neabstraktní třídy. Název součásti zobrazení je název třídy s příponou "ViewComponent" odebrat. Ho je také možné explicitně zadat pomocí `ViewComponentAttribute.Name` vlastnost.

Třídy zobrazení komponenty:

* Plně podporuje konstruktor [vkládání závislostí](../../fundamentals/dependency-injection.md)

* Neberou v rámci v životním cyklu řadiče, což znamená, nemůžete použít [filtry](../controllers/filters.md) v komponentě zobrazení

### <a name="view-component-methods"></a>Zobrazení metody součásti

Součást zobrazení definuje svou logikou v `InvokeAsync` metodu, která vrátí `IViewComponentResult`. Parametry pocházejí přímo z volání součásti zobrazení, nikoli z vazby modelu. Součást zobrazení nikdy přímo zpracuje požadavek. Obvykle komponentu zobrazení inicializuje modelu a předává je pro zobrazení pomocí volání `View` metoda. Souhrnně zobrazení součást metody:

* Definování `InvokeAsync` metodu, která vrátí `IViewComponentResult`
* Obvykle inicializuje modelu a předává je pro zobrazení pomocí volání `ViewComponent` `View` – metoda
* Parametry pocházejí z volání metoda HTTP není, neexistuje žádná vazba modelu
* Jsou přímo jako koncový bod HTTP není dostupná, budou se volat z kódu (obvykle v zobrazení). Součást zobrazení nikdy zpracovává žádost
* Jsou přetížené na podpis a nikoli na všechny podrobnosti, z aktuální žádosti HTTP

### <a name="view-search-path"></a>Zobrazení – cesta hledání

Modul runtime prohledá zobrazení do následující cesty:

   * Views/\<controller_name>/Components/\<view_component_name>/\<view_name>
   * Views/Shared/Components/\<view_component_name>/\<view_name>

Výchozí název zobrazení pro součást zobrazení je *výchozí*, což znamená, že váš soubor zobrazení se obvykle nazývá *Default.cshtml*. Můžete zadat název jiné zobrazení, při vytváření komponenty výsledný objekt zobrazení, nebo při volání metody `View` metoda.

Doporučujeme název souboru zobrazení *Default.cshtml* a použít *zobrazení/sdílené nebo součástí nebo\<view_component_name > /\<view_name >* cesta. `PriorityList` Používá zobrazení součástí používanou v této ukázce *Views/Shared/Components/PriorityList/Default.cshtml* pro zobrazení součásti zobrazení.

## <a name="invoking-a-view-component"></a>Vyvolání komponentu zobrazení

Chcete-li použít komponentu zobrazení, volejte následující uvnitř zobrazení:

```cshtml
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

Parametry se předá `InvokeAsync` metoda. `PriorityList` Zobrazení součásti vyvinuté v následujícím článku se volá z *Views/Todo/Index.cshtml* zobrazení souboru. V následujícím příkladu `InvokeAsync` metoda je volána s dva parametry:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a>Vyvolání komponentu zobrazení jako značka pomocné rutiny

Pro technologii ASP.NET Core 1.1 a vyšší, můžete vyvolat součást zobrazení jako [značky pomocná](xref:mvc/views/tag-helpers/intro):

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

Jsou použita Pascal třídy a metody parametry pro značku Pomocníci přeložit na jejich [nižší případ kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101). Používá značky pomocníka, který má vyvolat součást zobrazení `<vc></vc>` elementu. Součást zobrazení je určena následujícím způsobem:

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

Poznámka: Pokud chcete používat komponenty zobrazení jako značka pomocné rutiny, je nutné zaregistrovat sestavení obsahující komponenty zobrazení pomocí `@addTagHelper` – direktiva. Například pokud příslušné součásti zobrazení v sestavení nazvané "MyWebApp", přidejte následující direktiva k `_ViewImports.cshtml` souboru:

```cshtml
@addTagHelper *, MyWebApp
```

Součást zobrazení můžete zaregistrovat jako značka pomocné rutiny do souboru, která odkazuje na komponentu zobrazení. V tématu [Správa oboru pomocná značky](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) Další informace o postupu při registraci značky pomocné rutiny.

`InvokeAsync` Metodu použitou v tomto kurzu:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

V kódu pomocné rutiny značky:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

V ukázce výše `PriorityList` zobrazení součást se změní na `priority-list`. Parametry pro zobrazení součásti jsou předána jako atributy malá písmena kebab.

### <a name="invoking-a-view-component-directly-from-a-controller"></a>Vyvolání komponentu zobrazení přímo z řadiče

Zobrazení součásti jsou obvykle vyvolány ze zobrazení, ale je přímo z metody kontroleru můžete vyvolat. Při zobrazení součásti nemusíte definovat koncové body, jako jsou řadiče, můžete snadno implementovat akce kontroleru, který vrátí obsah `ViewComponentResult`.

V tomto příkladu je přímo z řadiče volá komponentu zobrazení:

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a>Návod: Vytvoření jednoduché zobrazení komponenty

[Stáhněte si](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), vytvoření a testování počáteční kód. Je jednoduchý projekt se `Todo` řadič, který zobrazí seznam *Todo* položky.

![Seznam ToDos](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a>Přidání třídy ViewComponent

Vytvoření *ViewComponents* složky a přidejte následující `PriorityListViewComponent` třídy:

[!code-csharp[](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

Poznámky k kód:

* Třídy součásti zobrazení mohou být obsaženy v **žádné** složky v projektu.
* Protože třída název PriorityList**ViewComponent** končí příponou **ViewComponent**, modul runtime použije řetězec "PriorityList" při odkazování na komponenty třídy ze zobrazení. I objasníme, který podrobněji později.
* `[ViewComponent]` Atributu můžete změnit název slouží k odkazování komponentu zobrazení. Například můžeme může jste s názvem třídy `XYZ` a použít `ViewComponent` atribut:

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* `[ViewComponent]` Výše uvedený atribut informuje výběr zobrazení komponent pro použití názvu `PriorityList` při vyhledávání pro zobrazení související s komponentou a použití řetězce "PriorityList" při odkazování na komponenty třídy ze zobrazení. I objasníme, který podrobněji později.
* Používá komponentu [vkládání závislostí](../../fundamentals/dependency-injection.md) chcete zpřístupnit data kontextu.
* `InvokeAsync` zpřístupňuje metodu, která lze volat z zobrazení ale může trvat libovolný počet argumentů.
* `InvokeAsync` Metoda vrací sadu `ToDo` položky, které odpovídají `isDone` a `maxPriority` parametry.

### <a name="create-the-view-component-razor-view"></a>Vytvoření zobrazení syntaxe Razor součást zobrazení

* Vytvořte *zobrazení/sdílené nebo součásti* složky. Tato složka **musí** nazván *součásti*.

* Vytvořte *zobrazení/sdílené nebo součástí nebo PriorityList* složky. Tento název složky musí odpovídat názvu třídy součástí zobrazení, nebo název třídy minus přípona (Pokud jsme postupovali podle konvence a použít *ViewComponent* přípony v názvu třídy). Pokud jste použili `ViewComponent` atribut název třídy potřebovat tak, aby odpovídaly označení atribut.

* Vytvoření *Views/Shared/Components/PriorityList/Default.cshtml* Razor zobrazení: [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]
    
   Zobrazení syntaxe Razor přebírá seznam `TodoItem` a zobrazí je. Pokud komponentu zobrazení `InvokeAsync` metoda neprojde název zobrazení (jako naše ukázka), *výchozí* slouží pro název zobrazení pomocí konvencí. Později v tomto kurzu I budete ukazují, jak předat název zobrazení. Pokud chcete přepsat výchozí stylu k určitému kontroleru, přidat zobrazení do složky specifické řadiče zobrazení (například *Views/Todo/Components/PriorityList/Default.cshtml)*.
    
    Pokud komponentu zobrazení je specifický pro řadič, můžete ho přidat do složky pro konkrétní řadič (*Views/Todo/Components/PriorityList/Default.cshtml*).

* Přidat `div` obsahující volání součást Seznam s prioritou k dolnímu okraji *Views/Todo/index.cshtml* souboru:

    [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

Kód `@await Component.InvokeAsync` ukazuje syntaxi pro volání součásti zobrazení. První argument je název součást, kterou chceme volání nebo volání. Následující parametry jsou předány součásti. `InvokeAsync` může trvat libovolný počet argumentů.

Testování aplikací. Následující obrázek znázorňuje seznamu úkolů a položky s prioritou:

![seznam a prioritu položek todo](view-components/_static/pi.png)

Součást zobrazení můžete také volat přímo z řadiče:

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![Priorita položky z IndexVC akce](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a>Zadejte název a zobrazení

Komponentu komplexní zobrazení může být nutné zadat jiné než výchozí zobrazení za určitých podmínek. Následující kód ukazuje, jak určit "PVC" zobrazení `InvokeAsync` metoda. Aktualizace `InvokeAsync` metoda v `PriorityListViewComponent` třídy.

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

Kopírování *Views/Shared/Components/PriorityList/Default.cshtml* soubor k zobrazení s názvem *Views/Shared/Components/PriorityList/PVC.cshtml*. Přidáte záhlaví se indikovat, že zobrazení PVC se používá.

[!code-cshtml[](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

Aktualizace *Views/TodoList/Index.cshtml*:

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

Spusťte aplikaci a ověření PVC zobrazení.

![Součást zobrazení s prioritou](view-components/_static/pvc.png)

Pokud není PVC zobrazení vykresleno, ověřte, zda že jsou volání komponentu zobrazení s prioritou 4 nebo vyšší.

### <a name="examine-the-view-path"></a>Zkontrolujte cestu zobrazení

* Změňte parametr priority na tři nebo méně, nevrátí zobrazení s prioritou.
* Dočasně přejmenujte *Views/Todo/Components/PriorityList/Default.cshtml* k *1Default.cshtml*.
* Testování aplikací, získáte následující chybě:

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' wasn't found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* Kopírování *Views/Todo/Components/PriorityList/1Default.cshtml* k *Views/Shared/Components/PriorityList/Default.cshtml*.
* Přidat některé značky, aby *sdílené* Todo zobrazení součásti zobrazení udávajících zobrazení je z *sdílené* složky.
* Testovací **sdílené** součásti zobrazení.

![Výstup úkolů s sdílené součásti zobrazení](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a>Zamezení magic řetězce

Pokud chcete zkompilovat čas zabezpečení, můžete název komponenty pevně zobrazení nahradit název třídy. Vytvořte komponentu zobrazení bez přípony "ViewComponent":

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

Přidat `using` příkaz, který má vaše Razor zobrazení souboru a použít `nameof` operátor:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,35-)]

## <a name="additional-resources"></a>Další zdroje

* [Injektáž závislostí do zobrazení](xref:mvc/views/dependency-injection)
