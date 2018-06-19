---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
title: Přehled směrování rozhraní ASP.NET MVC (C#) | Microsoft Docs
author: StephenWalther
description: V tomto kurzu Stephen Walther ukazuje, jak rozhraní ASP.NET MVC mapuje požadavky prohlížeče akce kontroleru.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 5b39d2d5-4bf9-4d04-94c7-81b84dfeeb31
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: fa565d2ef253539844f5224df00bdcdc047bb3f9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868591"
---
<a name="aspnet-mvc-routing-overview-c"></a>Přehled směrování rozhraní ASP.NET MVC (C#)
====================
podle [Stephen Walther](https://github.com/StephenWalther)

> V tomto kurzu Stephen Walther ukazuje, jak rozhraní ASP.NET MVC mapuje požadavky prohlížeče akce kontroleru.


V tomto kurzu jsou zavedené k důležitou součást každých aplikace ASP.NET MVC s názvem *směrování ASP.NET*. Modul Směrování ASP.NET je zodpovědná za mapování příchozích požadavků prohlížeče na konkrétní akce kontroleru MVC. Na konci tohoto kurzu bude pochopit, jak standardní směrovací tabulka mapuje požadavky akce kontroleru.

## <a name="using-the-default-route-table"></a>Pomocí výchozí směrovací tabulka

Když vytvoříte novou aplikaci ASP.NET MVC, aplikace již byla konfigurována pro použití směrování ASP.NET. Směrování ASP.NET je nastavená na dvou místech.

Nejprve směrování ASP.NET je povolený v souboru konfigurace webové aplikace (soubor Web.config). Existují čtyři části v konfiguračním souboru, které jsou relevantní pro směrování: části system.web.httpModules, v části system.web.httpHandlers, v části system.webserver.modules a v části system.webserver.handlers. Dejte pozor, abyste tyto části odstranit, protože bez těchto částí směrování přestane fungovat.

Druhý a důležitější je v souboru Global.asax aplikace vytvoří směrovací tabulku. Soubor Global.asax je speciální soubor, který obsahuje obslužné rutiny události pro události životního cyklu aplikace ASP.NET. Směrovací tabulka je vytvořen během události spustit aplikace.

Soubor v výpis 1 obsahuje výchozí soubor Global.asax pro aplikaci ASP.NET MVC.

**Listing 1 - Global.asax.cs**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample1.cs)]

Při prvním spuštění aplikace MVC, aplikace\_Start() metoda je volána. Tuto metodu, volá metodu RegisterRoutes(). Metoda RegisterRoutes() vytvoří směrovací tabulka.

Výchozí směrovací tabulka obsahuje jednu trasu (s názvem výchozí). Výchozí trasu mapy první segment adresy URL názvu kontroleru, druhý segment adresy URL k akci kontroleru a třetí segment, který má parametr s názvem **id**.

Představte si, zadejte následující adresu URL do adresního řádku webového prohlížeče:

/ Home/Index nebo 3

Výchozí trasu mapuje tuto adresu URL následující parametry:

- Řadič = Domů

- Akce = indexu

- id = 3

Pokud budete požadovat adresy URL/Home nebo Index nebo 3, se spustí následující kód:

HomeController.Index(3)

Výchozí trasu zahrnuje výchozí hodnoty pro všechny tři parametry. Pokud nezadáte řadič, pak parametr řadiče výchozí hodnotou je hodnota **Domů**. Pokud nezadáte akce, parametr akce výchozí hodnotou je hodnota **Index**. Nakonec Pokud nezadáte id, parametr id výchozí prázdný řetězec.

Podívejme se na několik příkladů způsobu výchozí trasu mapování adresy URL pro akce kontroleru. Představte si, zadejte následující adresu URL do adresního řádku prohlížeče:

Domů

Z důvodu výchozí hodnoty výchozí trasy parametr zadáte tuto adresu URL způsobí, že metoda Index() HomeController třídy v výpis 2, která se má volat.

**Výpis 2 - HomeController.cs**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample2.cs)]

Výpis 2 třída HomeController zahrnuje metodu s názvem Index(), který přijímá jeden parametr s názvem ID. Adresa URL/Home způsobí, že metoda Index() být volána s prázdný řetězec jako hodnotu parametru Id.

Kvůli způsobu, že rozhraní MVC volá akce kontroleru adresy URL/Home také odpovídající metodu Index() třídy HomeController ve výpisu 3.

**Výpis 3 - HomeController.cs (indexu akce s žádný parametr)**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample3.cs)]

Metoda Index() ve výpisu 3 nepřijímá žádné parametry. Adresa URL/Home způsobí, že tato metoda Index(), která se má volat. Adresa URL/Home/indexu/3 také vyvolá tuto metodu (Id bude ignorován).

Adresa URL/Home také odpovídající metodu Index() třídy HomeController ve výpisu 4.

**Výpis 4 - HomeController.cs (indexu akce s parametrem s možnou hodnotou Null)**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample4.cs)]

Výpis 4 metodu Index() má jeden parametr celé číslo. Protože parametr je parametr s možnou hodnotou Null (může mít hodnotu Null), je možné volat Index() bez vyvolání k chybě.

Nakonec volání metody Index() v výpis 5 s adresy URL/Home dojde k výjimce od parametr Id *není* parametr hodnotu Null. Pokud se pokusíte k vyvolání metody Index() zobrazí chyba zobrazené na obrázku 1.

**Výpis 5 - HomeController.cs (indexu akce s parametrem Id)**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample5.cs)]


[![Vyvolání akce kontroleru, která očekává hodnotu parametru](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)

**Obrázek 01**: vyvolání akce kontroleru, která očekává hodnotu parametru ([Kliknutím zobrazit obrázek v plné velikosti](asp-net-mvc-routing-overview-cs/_static/image2.png))


Adresa URL/Home nebo Index nebo 3, na druhé straně funguje správně, pomocí akce kontroleru indexu v výpis 5. Žádost o /Home/Index/3 způsobí, že metoda Index() volat s parametrem Id, který má hodnotu 3.

## <a name="summary"></a>Souhrn

Cílem tohoto kurzu bylo poskytnout stručný úvod do směrování ASP.NET. Jsme se zaměřili na výchozí směrovací tabulka, kterou můžete získat pomocí nové aplikace ASP.NET MVC. Jste se dozvěděli, jak výchozí trasu mapuje adresy URL akce kontroleru.

> [!div class="step-by-step"]
> [Next](understanding-action-filters-cs.md)
