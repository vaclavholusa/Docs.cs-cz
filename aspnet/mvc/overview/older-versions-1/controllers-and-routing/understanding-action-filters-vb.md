---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
title: Principy filtrů akcí (VB) | Dokumentace Microsoftu
author: microsoft
description: Cílem tohoto kurzu je vysvětlit filtrů akce. Filtr akce je atribut, který můžete použít na akce kontroleru--nebo celý kontroler...
ms.author: aspnetcontent
ms.date: 10/16/2008
ms.assetid: e83812f2-c53e-4a43-a7c1-d64c59ecf694
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
msc.type: authoredcontent
ms.openlocfilehash: 07e02330747b3b8f8bb318d30c7a984aa15baeef
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811179"
---
<a name="understanding-action-filters-vb"></a>Principy filtrů akcí (VB)
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_VB.pdf)

> Cílem tohoto kurzu je vysvětlit filtrů akce. Filtr akce je atribut, který můžete použít na akce kontroleru--nebo celý kontroler –, který mění způsob, jakým provedením akce.


## <a name="understanding-action-filters"></a>Principy filtrů akcí

Cílem tohoto kurzu je vysvětlit filtrů akce. Filtr akce je atribut, který můžete použít na akce kontroleru--nebo celý kontroler –, který mění způsob, jakým provedením akce. Architektura ASP.NET MVC zahrnuje několik filtrů akce:

- OutputCache – tento filtr akce ukládá do mezipaměti výstupu akce kontroleru pro určenou dobu.
- HandleError – tento filtr akce zpracovává chyby, na které se vyvolá, když se spustí akce kontroleru.
- Povolit – tento filtr akce umožňuje omezit přístup na konkrétní uživatele nebo roli.

Můžete také vytvořit vlastní filtry vlastní akce. Můžete například chtít vytvořit filtr vlastních akcí kvůli implementaci vlastního ověřovacího systému. Nebo můžete chtít vytvořit filtr akce, která upravuje zobrazení dat vrácených akce kontroleru.

V tomto kurzu se dozvíte, jak vytvořit filtr akce od základů. Vytvoříme protokolu filtru akce, která protokoluje různé fáze zpracování akce v okně Výstup Visual Studia.

### <a name="using-an-action-filter"></a>Pomocí filtru akce

Filtr akce je atribut. Většina filtrů akce můžete použít k akci individuální řadiče nebo celý kontroler.

Například kontroler dat v informacích 1 zpřístupňuje akci s názvem `Index()` , který vrací aktuální čas. Tato akce je doplněn `OutputCache` filtru akce. Tento filtr způsobí, že hodnota vrácená akce, která má být uložené v mezipaměti 10 sekund.

**Výpis 1 – `Controllers\DataController.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample1.vb)]

Pokud opakovaně vyvoláte `Index()` akce zadáním adresy URL/Data/Index do adresního řádku prohlížeče a při aktualizaci tlačítko více než jednou, zobrazí se stejnou dobu 10 sekund. Výstup `Index()` akce se uloží do mezipaměti po dobu 10 sekund (viz obrázek 1).


[![Čas v mezipaměti](understanding-action-filters-vb/_static/image2.png)](understanding-action-filters-vb/_static/image1.png)

**Obrázek 01**: v mezipaměti Doba ([kliknutím ji zobrazíte obrázek v plné velikosti](understanding-action-filters-vb/_static/image3.png))


V jedné akce filtru – výpis 1 `OutputCache` filtr akce – platí pro `Index()` metoda. Pokud potřebujete, můžete provést několik filtrů akce u stejné akce. Například můžete chtít použít i `OutputCache` a `HandleError` filtrů Akce na stejnou akci.

V informacích 1 `OutputCache` filtr akce `Index()` akce. Můžete také použít tento atribut `DataController` třídu. V takovém případě výsledek vrácený z jakékoli akce kontroleru vystavené by do mezipaměti 10 sekund.

### <a name="the-different-types-of-filters"></a>Různé typy filtrů

Architektura ASP.NET MVC podporuje čtyři typy filtrů:

1. Filtry autorizace – implementuje `IAuthorizationFilter` atribut.
2. Filtry akcí – implementuje `IActionFilter` atribut.
3. Výsledek filtry – implementuje `IResultFilter` atribut.
4. Filtry výjimek – implementuje `IExceptionFilter` atribut.

Filtry jsou spuštěny v uvedeném pořadí. Například filtry autorizace se vždy spouští se před filtrů Akce a filtry výjimek jsou provedeny vždy po každý jiný typ filtru.

Filtry autorizace se používají k implementaci ověřování a autorizaci pro akce kontroleru. Například filtr Authorize je příkladem filtr autorizace.

Filtry akcí obsahují logiku, která se spustí před a po spuštění akce kontroleru. Filtr akce, můžete použít například upravit data zobrazení, která vrací akce kontroleru.

Filtry výsledků obsahují logiku, která se spustí před a po zobrazení výsledků spuštění. Například můžete chtít změnit zobrazení výsledků klikněte pravým tlačítkem myši předtím, než je zobrazení vykresleno do prohlížeče.

Filtry výjimek jsou poslední typ filtru pro spuštění. Zpracování chyb vyvolaných akce kontroleru nebo výsledky akce kontroleru můžete použít filtr výjimek. Filtry výjimek můžete použít také k protokolování chyb.

Každý jiný typ filtru je proveden v určitém pořadí. Pokud chcete řídit pořadí provedení filtrů stejného typu můžete nastavit vlastnost pořadí filtru.

Základní třída pro všechny filtry akce je `System.Web.Mvc.FilterAttribute` třídy. Pokud chcete implementovat konkrétní typ filtru, bude nutné vytvořit třídu, která dědí ze základní třídy filtr a implementuje jednu nebo více IAuthorizationFilter, IActionFilter, IResultFilter nebo ExceptionFilter rozhraní.

### <a name="the-base-actionfilterattribute-class"></a>Základní třídy ActionFilterAttribute

Aby bylo snazší pro vás k implementaci filtr vlastních akcí, rozhraní ASP.NET MVC zahrnuje základní `ActionFilterAttribute` třídy. Tato třída implementuje oba `IActionFilter` a `IResultFilter` rozhraní a dědí z `Filter` třídy.

Terminologie tady není zcela v souladu. Technicky je o třídu odvozenou od třídy ActionFilterAttribute filtru akce a výsledku filtru. V tom smyslu přijít o provedené, ale filtr akcí aplikace word se používá k odkazování na libovolný typ filtru v rozhraní ASP.NET MVC.

Základní třídy ActionFilterAttribute má následující metody, které můžete přepsat:

- OnActionExecuting – tato metoda je volána před provedením akce kontroleru.
- OnActionExecuted – tato metoda je volána po provedení akce kontroleru.
- OnResultExecuting – tato metoda je volána před provedením výsledku akce kontroleru.
- OnResultExecuted – tato metoda je volána po provedení výsledku akce kontroleru.

V další části uvidíme, jak můžete implementovat, každá z těchto různých metod.

### <a name="creating-a-log-action-filter"></a>Vytvoření filtru protokolu akcí

Aby bylo možné ukazují, jak se dají vytvářet filtr vlastních akcí, vytvoříme vlastní akce filtr, který se zaznamená v jednotlivých fázích zpracování akce kontroleru v okně Výstup Visual Studia. Naše `LogActionFilter` je obsažen v informacích 2.

**Výpis 2 – `ActionFilters\LogActionFilter.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample2.vb)]

V informacích 2 `OnActionExecuting()`, `OnActionExecuted()`, `OnResultExecuting()`, a `OnResultExecuted()` volání metody `Log()` metody. Název metody a aktuální data trasy, která je předána `Log()` metody. `Log()` Metoda zapíše zprávu do okna výstup Visual Studia (viz obrázek 2).


[![Zápis v okně Výstup Visual Studia](understanding-action-filters-vb/_static/image5.png)](understanding-action-filters-vb/_static/image4.png)

**Obrázek 02**: zápis do okna výstup Visual Studia ([kliknutím ji zobrazíte obrázek v plné velikosti](understanding-action-filters-vb/_static/image6.png))


Kontroler Home v informacích 3 znázorňuje, jak je možné použít filtr protokolu akcí do třídy celý kontroler. Vždy, když některou z akcí, které jsou vystavené kontroler Home jsou vyvolány – buď `Index()` metoda nebo `About()` metoda – fáze zpracování akce se Zaprotokolují v okně Výstup Visual Studia.

**Výpis 3 – `Controllers\HomeController.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample3.vb)]

### <a name="summary"></a>Souhrn

V tomto kurzu jste se seznámili s ASP.NET MVC filtrů akce. Jste se dozvěděli o čtyři typy filtrů: filtry autorizace, filtry akce, filtry výsledků a filtry výjimek. Také jste se naučili o základní `ActionFilterAttribute` třídy.

Nakonec jste zjistili, jak implementovat jednoduchého filtru akce. Vytvořili jsme filtr protokolu akce, která protokoluje v jednotlivých fázích zpracování akce kontroleru v okně Výstup Visual Studia.

> [!div class="step-by-step"]
> [Předchozí](asp-net-mvc-routing-overview-vb.md)
> [další](improving-performance-with-output-caching-vb.md)
