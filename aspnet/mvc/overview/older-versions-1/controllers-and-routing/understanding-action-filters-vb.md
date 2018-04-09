---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
title: Seznámení s filtry akcí (VB) | Microsoft Docs
author: microsoft
description: Cílem tohoto kurzu je vysvětlit, filtrů akce. Filtr akce je atribut, který můžete použít pro akce kontroleru--nebo celý řadiče...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: e83812f2-c53e-4a43-a7c1-d64c59ecf694
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
msc.type: authoredcontent
ms.openlocfilehash: 2796b67cba6a2ddaee7a006a170dfb7e5ff89888
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="understanding-action-filters-vb"></a>Seznámení s filtry akcí (VB)
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_VB.pdf)

> Cílem tohoto kurzu je vysvětlit, filtrů akce. Filtr akce je atribut, který můžete použít pro akce kontroleru--nebo celý řadiče – které mění způsob, ve kterém se spustí akce.


## <a name="understanding-action-filters"></a>Seznámení s filtry akcí

Cílem tohoto kurzu je vysvětlit, filtrů akce. Filtr akce je atribut, který můžete použít pro akce kontroleru--nebo celý řadiče – které mění způsob, ve kterém se spustí akce. Rozhraní ASP.NET MVC zahrnuje několik filtrů akce:

- OutputCache – tento filtr akce ukládá do mezipaměti výstup akce kontroleru pro zadanou dobu.
- HandleError – tento filtr akce zpracovává chyby vyvolá, když provádí akce kontroleru.
- Autorizovat – tento filtr akce umožňuje omezení přístupu ke konkrétní uživatel nebo role.

Také můžete vytvořit vlastní akce filtry. Můžete například vytvořit vlastní akce filtru kvůli implementaci vlastního ověřování systému. Nebo můžete chtít vytvořit filtr akce, která upraví zobrazení dat vrácených akce kontroleru.

V tomto kurzu zjistěte, jak vytvořit filtr akce od základů. Vytvoříme filtr akce protokol, který protokoluje různé fáze zpracování akce do okna výstupu Visual Studio.

### <a name="using-an-action-filter"></a>Pomocí filtru akce

Filtr akce je atribut. Většina filtrů akce můžete použít pro akce jednotlivých řadiče nebo celý kontroler.

Například řadič Data v výpis 1 zpřístupní akci s názvem `Index()` , který vrací aktuální čas. Tato akce je upraven pomocí `OutputCache` filtru akce. Tento filtr způsobí, že hodnoty vrácené akce ukládat do mezipaměti 10 sekund.

**Výpis 1 – `Controllers\DataController.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample1.vb)]

Pokud opakovaně vyvolání `Index()` akce zadáním adresy URL/Data/Index do adresního řádku prohlížeče a stiskne aktualizace tlačítko vícekrát, zobrazí se ve stejnou dobu 10 sekund. Výstup `Index()` akce se uloží do mezipaměti 10 sekund (viz obrázek 1).


[![Čas v mezipaměti](understanding-action-filters-vb/_static/image2.png)](understanding-action-filters-vb/_static/image1.png)

**Obrázek 01**: v mezipaměti Doba ([Kliknutím zobrazit obrázek v plné velikosti](understanding-action-filters-vb/_static/image3.png))


V jedné akce filtru – výpis 1 `OutputCache` filtru akce – u `Index()` metoda. Pokud potřebujete, můžete použít více filtrů Akce na stejné akce. Například můžete chtít použít i `OutputCache` a `HandleError` filtry akcí na stejnou akci.

Výpis 1 `OutputCache` akce filtr je použit `Index()` akce. Také může použít pro tento atribut `DataController` třídy sám sebe. V takovém případě výsledek vrácený žádnou akci vystavené řadičem by do mezipaměti 10 sekund.

### <a name="the-different-types-of-filters"></a>Různé typy filtrů

Rozhraní ASP.NET MVC podporuje čtyři typy filtrů:

1. Filtry autorizace – implementuje `IAuthorizationFilter` atribut.
2. Filtry akce – implementuje `IActionFilter` atribut.
3. Vést filtry – implementuje `IResultFilter` atribut.
4. Filtry výjimek – implementuje `IExceptionFilter` atribut.

Filtry jsou spouštěny v pořadí uvedené výše. Například filtry autorizace se vždy spuštěny před filtry akcí a filtry výjimek jsou vždy provést po každý jiný typ filtru.

Filtry autorizace se používají k implementace ověřování a autorizace pro akce kontroleru. Například filtr Authorize je příkladem filtr autorizace.

Filtrů akce obsahují logiku, která se provedla před a po provedení akce kontroleru. Filtr akce můžete použít například k úpravě zobrazení data, která vrátí akce kontroleru.

Filtry výsledků obsahují logiku, která se provedla před a po provedení výsledku zobrazení. Například můžete chtít změnit zobrazení výsledků pravým předtím, než je zobrazení vykresleno do prohlížeče.

Filtry výjimek jsou poslední typ spustit filtr. Zpracování chyb, vyvolá akce kontroleru nebo výsledky akce kontroleru můžete použít filtr výjimek. Můžete také použít filtry výjimek chyb do protokolu.

Každý jiný typ filtru je provést v určitém pořadí. Pokud chcete určit pořadí, ve kterém jsou prováděny filtry stejného typu, můžete nastavit vlastnost pořadí filtru.

Základní třída pro všechny filtry akce je `System.Web.Mvc.FilterAttribute` třídy. Pokud chcete implementovat konkrétní typ filtru, budete muset vytvořit třídu, která dědí vlastnosti ze základní třídy filtru a implementuje jeden nebo více IAuthorizationFilter, IActionFilter, IResultFilter nebo ExceptionFilter rozhraní.

### <a name="the-base-actionfilterattribute-class"></a>Základní třídy ActionFilterAttribute

Abyste snadněji implementovat vlastní akce filtru, rozhraní ASP.NET MVC zahrnuje na základní `ActionFilterAttribute` třídy. Tato třída implementuje i `IActionFilter` a `IResultFilter` rozhraní a dědí z `Filter` třídy.

Terminologie zde není zcela v souladu. Technicky je třídu, která dědí z třídy ActionFilterAttribute filtr akce a filtr výsledků. V tom smyslu přijít, však filtr akcí aplikace word se používá k odkazování na jakýkoli typ filtru v rozhraní ASP.NET MVC.

Základní třídy ActionFilterAttribute má následující metody, které můžete přepsat:

- OnActionExecuting – tato metoda je volána před provedením akce kontroleru.
- OnActionExecuted – tato metoda je volána po provedení akce kontroleru.
- OnResultExecuting – tato metoda je volána před provedením výsledku akce kontroleru.
- OnResultExecuted – tato metoda je volána po provedení výsledku akce kontroleru.

V další části uvidíme, jak můžete implementovat, každá z těchto různých metod.

### <a name="creating-a-log-action-filter"></a>Vytváření akce filtru protokolu

Chcete-li ilustrují, jak můžete vytvořit vlastní akce filtru, vytvoříme vlastní akce filtr, který protokoluje fází zpracování akce kontroleru do okna výstupu Visual Studio. Naše `LogActionFilter` je součástí výpis 2.

**Výpis 2 – `ActionFilters\LogActionFilter.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample2.vb)]

Výpis 2 `OnActionExecuting()`, `OnActionExecuted()`, `OnResultExecuting()`, a `OnResultExecuted()` všechny metody volání `Log()` metoda. Název metody a aktuální data trasy, která je předána `Log()` metoda. `Log()` Metoda zapíše zprávu do okna výstupu Visual Studio (viz obrázek 2).


[![Zápis do okna výstupu Visual Studio](understanding-action-filters-vb/_static/image5.png)](understanding-action-filters-vb/_static/image4.png)

**Obrázek 02**: zápis do okna výstupu Visual Studio ([Kliknutím zobrazit obrázek v plné velikosti](understanding-action-filters-vb/_static/image6.png))


Domovské řadiče v výpis 3 znázorňuje, jak můžete použít filtr akce protokolu celý ovladač třídy. Vždy, když jednu z akcí, které jsou vystavené domovské řadiče jsou vyvolány – buď `Index()` metoda nebo `About()` metoda – fáze zpracování akce se zaznamenávají do okna výstupu Visual Studio.

**Výpis 3 – `Controllers\HomeController.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample3.vb)]

### <a name="summary"></a>Souhrn

V tomto kurzu jste se seznámili s ASP.NET MVC filtrů akce. Seznámili jste se čtyři různé typy filtrů: filtry autorizace, filtry akce, filtry výsledků a filtry výjimek. Seznámili jste se také základní `ActionFilterAttribute` třídy.

Nakonec jste zjistili, jak implementovat jednoduchého filtru akce. Jsme vytvořili filtr akce protokol, který protokoluje fází zpracování akce kontroleru do okna výstupu Visual Studio.

> [!div class="step-by-step"]
> [Předchozí](asp-net-mvc-routing-overview-vb.md)
> [další](improving-performance-with-output-caching-vb.md)
