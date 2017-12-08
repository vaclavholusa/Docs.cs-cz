---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
title: "Přehled směrování rozhraní ASP.NET MVC (VB) | Microsoft Docs"
author: StephenWalther
description: "V tomto kurzu Stephen Walther ukazuje, jak rozhraní ASP.NET MVC mapuje požadavky prohlížeče akce kontroleru."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 4bc8d19a-80f1-44b4-adbf-95ed22d691ca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: 1e4c74e61b1a0d5f5020154756e34dd2fa507034
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-routing-overview-vb"></a>Přehled směrování rozhraní ASP.NET MVC (VB)
====================
podle [Stephen Walther](https://github.com/StephenWalther)

> V tomto kurzu Stephen Walther ukazuje, jak rozhraní ASP.NET MVC mapuje požadavky prohlížeče akce kontroleru.


V tomto kurzu jsou zavedené k důležitou součást každých aplikace ASP.NET MVC s názvem *směrování ASP.NET*. Modul Směrování ASP.NET je zodpovědná za mapování příchozích požadavků prohlížeče na konkrétní akce kontroleru MVC. Na konci tohoto kurzu bude pochopit, jak standardní směrovací tabulka mapuje požadavky akce kontroleru.

## <a name="using-the-default-route-table"></a>Pomocí výchozí směrovací tabulka

Když vytvoříte novou aplikaci ASP.NET MVC, aplikace již byla konfigurována pro použití směrování ASP.NET. Směrování ASP.NET je nastavená na dvou místech.

Nejprve směrování ASP.NET je povolený v souboru konfigurace webové aplikace (soubor Web.config). Existují čtyři části v konfiguračním souboru, které jsou relevantní pro směrování: části system.web.httpModules, v části system.web.httpHandlers, v části system.webserver.modules a v části system.webserver.handlers. Dejte pozor, abyste tyto části odstranit, protože bez těchto částí směrování přestane fungovat.

Druhý a důležitější je v souboru Global.asax aplikace vytvoří směrovací tabulku. Soubor Global.asax je speciální soubor, který obsahuje obslužné rutiny události pro události životního cyklu aplikace ASP.NET. Směrovací tabulka je vytvořen během události spustit aplikace.

Soubor v výpis 1 obsahuje výchozí soubor Global.asax pro aplikaci ASP.NET MVC.

**Výpis 1 - Global.asax.vb**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample1.vb)]

Při prvním spuštění aplikace MVC, aplikace\_Start() metoda je volána. Tuto metodu, volá metodu RegisterRoutes(). Metoda RegisterRoutes() vytvoří směrovací tabulka.

Výchozí směrovací tabulka obsahuje jednu trasu (s názvem výchozí). Výchozí trasu mapy první segment adresy URL názvu kontroleru, druhý segment adresy URL k akci kontroleru a třetí segment, který má parametr s názvem **id**.

Představte si, zadejte následující adresu URL do adresního řádku webového prohlížeče:

/ Home/Index nebo 3

Výchozí trasu mapuje tuto adresu URL následující parametry:

- Řadič = Domů

- Akce = indexu

- ID = 3

Pokud budete požadovat adresy URL/Home nebo Index nebo 3, se spustí následující kód:

HomeController.Index(3)

Výchozí trasu zahrnuje výchozí hodnoty pro všechny tři parametry. Pokud nezadáte řadič, pak parametr řadiče výchozí hodnotou je hodnota **Domů**. Pokud nezadáte akce, parametr akce výchozí hodnotou je hodnota **Index**. Nakonec Pokud nezadáte id, parametr id výchozí prázdný řetězec.

Podívejme se na několik příkladů způsobu výchozí trasu mapování adresy URL pro akce kontroleru. Představte si, zadejte následující adresu URL do adresního řádku prohlížeče:

Domů

Z důvodu výchozí hodnoty výchozí trasy parametr zadáte tuto adresu URL způsobí, že metoda Index() HomeController třídy v výpis 2, která se má volat.

**Výpis 2 - HomeController.vb**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample2.vb)]

Výpis 2 třída HomeController zahrnuje metodu s názvem Index(), který přijímá jeden parametr s názvem ID. Adresa URL/Home způsobí, že metoda Index() volat s hodnotou nic jako hodnotu parametru Id.

Kvůli způsobu, že rozhraní MVC volá akce kontroleru adresy URL/Home také odpovídající metodu Index() třídy HomeController ve výpisu 3.

**Výpis 3 - HomeController.vb (indexu akce s žádný parametr)**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample3.vb)]

Metoda Index() ve výpisu 3 nepřijímá žádné parametry. Adresa URL/Home způsobí, že tato metoda Index(), která se má volat. Adresa URL/Home/indexu/3 také vyvolá tuto metodu (Id bude ignorován).

Adresa URL/Home také odpovídající metodu Index() třídy HomeController ve výpisu 4.

**Výpis 4 - HomeController.vb (indexu akce s parametrem s možnou hodnotou Null)**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample4.vb)]

Výpis 4 metodu Index() má jeden parametr celé číslo. Protože parametr je parametr s možnou hodnotou Null (může mít hodnotu Nothing), je možné volat Index() bez vyvolání k chybě.

Nakonec volání metody Index() v výpis 5 s adresy URL/Home dojde k výjimce od parametr Id *není* parametr hodnotu Null. Pokud se pokusíte k vyvolání metody Index() zobrazí chyba zobrazené na obrázku 1.

**Výpis 5 - HomeController.vb (indexu akce s parametrem Id)**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample5.vb)]


[![Vyvolání akce kontroleru, která očekává hodnotu parametru](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)

**Obrázek 01**: vyvolání akce kontroleru, která očekává hodnotu parametru ([Kliknutím zobrazit obrázek v plné velikosti](asp-net-mvc-routing-overview-vb/_static/image2.png))


Adresa URL/Home nebo Index nebo 3, na druhé straně funguje správně, pomocí akce kontroleru indexu v výpis 5. Žádost o /Home/Index/3 způsobí, že metoda Index() volat s parametrem Id, který má hodnotu 3.

## <a name="summary"></a>Souhrn

Cílem tohoto kurzu bylo poskytnout stručný úvod do směrování ASP.NET. Jsme se zaměřili na výchozí směrovací tabulka, kterou můžete získat pomocí nové aplikace ASP.NET MVC. Jste se dozvěděli, jak výchozí trasu mapuje adresy URL akce kontroleru.

>[!div class="step-by-step"]
[Předchozí](creating-an-action-cs.md)
[další](understanding-action-filters-vb.md)
