---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
title: ASP.NET MVC – Přehled směrování (VB) | Dokumentace Microsoftu
author: StephenWalther
description: V tomto kurzu Stephen Walther ukazuje, jak rozhraní ASP.NET MVC mapuje požadavky prohlížeče na akce kontroleru.
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 4bc8d19a-80f1-44b4-adbf-95ed22d691ca
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: c20626b24e43031fc4103365396fc26b6a6daf93
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755710"
---
<a name="aspnet-mvc-routing-overview-vb"></a>ASP.NET MVC – Přehled směrování (VB)
====================
podle [Stephen Walther](https://github.com/StephenWalther)

> V tomto kurzu Stephen Walther ukazuje, jak rozhraní ASP.NET MVC mapuje požadavky prohlížeče na akce kontroleru.


V tomto kurzu jste se seznámili s důležitou funkcí každou aplikaci ASP.NET MVC s názvem *směrování ASP.NET*. Modul Směrování ASP.NET je zodpovědná za mapování příchozích požadavků prohlížeče na konkrétní akce kontroleru MVC. Na konci tohoto kurzu se seznámíte s jak standardní směrovací tabulka mapuje požadavky na akce kontroleru.

## <a name="using-the-default-route-table"></a>Pomocí výchozí směrovací tabulka

Při vytváření nové aplikace ASP.NET MVC, aplikace je již nakonfigurována pro použití směrování ASP.NET. Směrování ASP.NET je nastavená na dvou místech.

Nejprve směrování ASP.NET je povolena v souboru konfigurace webové aplikace (soubor Web.config). Existují čtyři oddíly v konfiguračním souboru, které souvisí se směrováním: části system.web.httpModules, části system.web.httpHandlers, části system.webserver.modules a system.webserver.handlers oddílu. Dejte pozor, abyste odstranit tyto oddíly, protože bez těchto oddílů směrování se už nebude fungovat.

Za druhé a důležitější je se vytvoří směrovací tabulku v souboru Global.asax. Soubor Global.asax je zvláštní soubor, který obsahuje obslužné rutiny událostí pro události životního cyklu aplikace ASP.NET. Směrovací tabulka se vytvoří během události spuštění aplikace.

Soubor výpisu 1 obsahuje výchozí soubor Global.asax pro aplikace ASP.NET MVC.

**Listing 1 - Global.asax.vb**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample1.vb)]

Při prvním spuštění aplikace MVC, aplikace\_volání metody Start(). Tato metoda pak volá metodu RegisterRoutes(). Metoda RegisterRoutes() vytvoří směrovací tabulku.

Výchozí směrovací tabulka obsahuje jednu trasu (výchozí). Výchozí trasu první segment adresy URL odpovídá názvu kontroleru, druhý segment adresy URL pro akce kontroleru a třetí parametr s názvem segmentu **id**.

Představte si, zadejte následující adresu URL do adresního řádku webového prohlížeče:

/ Home/Index/3

Výchozí trasu mapuje tuto adresu URL na následující parametry:

- Kontroler = Home

- Akce = Index

- id = 3

Pokud budete požadovat adresy URL/Home/Index/3, následující kód se spustí:

HomeController.Index(3)

Výchozí trasa obsahuje výchozí hodnoty pro všemi třemi parametry. Pokud nezadáte kontroleru, pak řadič parametr výchozí hodnotu k hodnotě **Domů**. Pokud nezadáte akci, parametr akce výchozí hodnotou je hodnota **Index**. Nakonec, pokud nezadáte id, id parametr výchozí hodnotu prázdný řetězec.

Podívejme se na několik příkladů způsobu výchozí trasu mapování adres URL na akce kontroleru. Představte si, zadejte následující adresu URL do adresního řádku prohlížeče:

Domů

Z důvodu parametr výchozí trasy zadáte tuto adresu URL způsobí, že metoda Index() třídy HomeController v výpis 2, která se má volat.

**Výpis 2 - HomeController.vb**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample2.vb)]

Výpis 2 HomeController třídy obsahuje metodu s názvem Index(), která přijímá jeden parametr s názvem ID. Adresa URL/Home způsobí, že metoda Index() volat s hodnotou Nothing jako hodnotu parametru Id.

Vzhledem ke způsobu, že rozhraní MVC volá akce kontroleru adresy URL/Home také odpovídající metodu Index() HomeController třídy v informacích 3.

**Výpis 3 - HomeController.vb (Index akce s žádný parametr)**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample3.vb)]

Metoda Index() ve výpisu 3 nepřijímá žádné parametry. Adresa URL/Home způsobí, že tato metoda Index() volat. Adresa URL/Home/Index/3 také vyvolá tuto metodu (Id se ignoruje).

Adresa URL/Home také odpovídající metodu Index() HomeController třídy v informacích 4.

**Část 4 – HomeController.vb (Index akce pomocí parametru s možnou hodnotou Null)**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample4.vb)]

V zobrazení 4 Index() metoda má jeden parametr celé číslo. Protože parametr je parametr s možnou hodnotou Null (může mít hodnotu Nothing), Index() lze volat bez vyvolání k chybě.

Volání metody Index() výpis 5 pomocí adresy URL/Home nakonec dojde k výjimce od parametru Id *není* parametru s možnou hodnotou Null. Při pokusu o volání metody Index() zobrazí chyba zobrazí na obrázku 1.

**Výpis 5 - HomeController.vb (Index akce s parametrem Id)**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample5.vb)]


[![Vyvolání akce kontroleru, který očekává, že hodnota parametru](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)

**Obrázek 01**: vyvolání akce kontroleru, který očekává, že hodnota parametru ([kliknutím ji zobrazíte obrázek v plné velikosti](asp-net-mvc-routing-overview-vb/_static/image2.png))


Adresy URL/Home/Index/3, na druhé straně funguje jenom s akce kontroleru indexu v informacích 5. Žádost o /Home/Index/3 způsobí, že metoda Index() nelze volat s parametrem Id, který má hodnotu 3.

## <a name="summary"></a>Souhrn

Cílem tohoto kurzu bylo poskytne stručný úvod do směrování ASP.NET. Jsme se zaměřili na výchozí směrovací tabulka, která získáte pomocí nové aplikace ASP.NET MVC. Jste se naučili, jak mapuje výchozí trasu adresy URL na akce kontroleru.

> [!div class="step-by-step"]
> [Předchozí](creating-an-action-cs.md)
> [další](understanding-action-filters-vb.md)
