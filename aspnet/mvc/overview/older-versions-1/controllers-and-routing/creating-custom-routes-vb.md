---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
title: Vytváření vlastní trasy (VB) | Microsoft Docs
author: microsoft
description: Zjistěte, jak přidat vlastní trasy pro aplikace ASP.NET MVC. V tomto kurzu zjistěte, jak změnit výchozí směrovací tabulka v souboru Global.asax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 6ac5758b-6199-42af-adcb-21954b864951
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
msc.type: authoredcontent
ms.openlocfilehash: e827725ab7ce54c86ae9f4193d0a8a7ef4af8512
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="creating-custom-routes-vb"></a>Vytváření vlastní trasy (VB)
====================
podle [Microsoft](https://github.com/microsoft)

> Zjistěte, jak přidat vlastní trasy pro aplikace ASP.NET MVC. V tomto kurzu zjistěte, jak změnit výchozí směrovací tabulka v souboru Global.asax.


V tomto kurzu zjistěte, jak přidat vlastní trasy pro aplikace ASP.NET MVC. Zjistíte, jak změnit výchozí směrovací tabulka v souboru Global.asax s vlastní trasy.

V aplikacích ASP.NET MVC bude výchozí směrovací tabulka pracovat správně. Však může zjišťovat specializovaný směrování potřebám. V takovém případě můžete vytvořit vlastní trasy.

Představte si například, že vytváříte aplikaci blogu. Můžete chtít zpracovávat příchozí požadavky, které vypadají takto:

/ Archivu/12-25-2009

Když uživatel zadá tuto žádost, chcete vrátit položku blogu, která odpovídá datum 12/25/2009. Aby bylo možné zpracovávat tento typ požadavku, musíte vytvořit vlastní trasy.

Soubor Global.asax v výpis 1 obsahuje nové vlastní trasy, s názvem Blog, která zpracovává požadavky, které vypadat /Archive/*datum položky*.

**Výpis 1 - Global.asax (s vlastní směrování)**

[!code-vb[Main](creating-custom-routes-vb/samples/sample1.vb)]

Je důležité pořadí tras, které přidáte do tabulky směrování. Naše nová vlastní Blog trasa se přidá před stávající výchozí trasu. Pokud je změněn na opačný pořadí, pak výchozí trasu vždy bude zavolána místo vlastní trasy.

Vlastní Blog trasa odpovídá každá žádost, který začíná/archivu /. Proto že splňuje všechny následující adresy URL:

- / Archivu/12-25-2009

- / Archivu/10-6-2004

- / Archivu nebo apple

Vlastní trasy příchozí žádost se mapuje na řadič archivu a vyvolá akci Entry(). Při volání metody Entry() datum položky se předá jako parametr s názvem entryDate.

Můžete vytvořit vlastní trasy Blog pomocí řadiče v výpis 2.

**Výpis 2 - ArchiveController.vb**

[!code-vb[Main](creating-custom-routes-vb/samples/sample2.vb)]

Všimněte si, že metoda Entry() v výpis 2 přijímá parametr typu data a času. Architektura MVC je dostatečně inteligentní automaticky převést vstupní data z adresy URL na hodnotu data a času. Pokud parametr vstupní data z adresy URL nelze převést na typ DateTime, je vyvolána chyba (viz obrázek 1).

**Obrázek 1 – Chyba z převodu parametru**


[![Dialogové okno Nový projekt](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)

**Obrázek 01**: Chyba z převodu parametr ([Kliknutím zobrazit obrázek v plné velikosti](creating-custom-routes-vb/_static/image2.png))


## <a name="summary"></a>Souhrn

Cílem tohoto kurzu bylo ukazují, jak můžete vytvořit vlastní trasy. Jste zjistili, jak přidat vlastní trasy do směrovací tabulky v souboru Global.asax, která představuje položky blogu. Jsme probrali jak mapovat požadavky pro položky blogu řadič ArchiveController a s názvem Entry() akce kontroleru.

> [!div class="step-by-step"]
> [Předchozí](asp-net-mvc-controller-overview-vb.md)
> [další](creating-a-route-constraint-vb.md)
