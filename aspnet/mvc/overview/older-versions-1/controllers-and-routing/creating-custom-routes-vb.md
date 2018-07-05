---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
title: Vytváření vlastních tras (VB) | Dokumentace Microsoftu
author: microsoft
description: Zjistěte, jak přidat vlastní trasy pro aplikace ASP.NET MVC. V tomto kurzu se dozvíte, jak změnit výchozí směrovací tabulka v souboru Global.asax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 6ac5758b-6199-42af-adcb-21954b864951
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
msc.type: authoredcontent
ms.openlocfilehash: b2bca002434274e334509d21f6857e094d391198
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362015"
---
<a name="creating-custom-routes-vb"></a>Vytváření vlastních tras (VB)
====================
podle [Microsoft](https://github.com/microsoft)

> Zjistěte, jak přidat vlastní trasy pro aplikace ASP.NET MVC. V tomto kurzu se dozvíte, jak změnit výchozí směrovací tabulka v souboru Global.asax.


V tomto kurzu se dozvíte, jak přidat vlastní trasy pro aplikace ASP.NET MVC. Zjistíte, jak změnit výchozí směrovací tabulka v souboru Global.asax s vlastní trasy.

V aplikacích ASP.NET MVC bude výchozí směrovací tabulka fungovat správně. Však můžete zjistit, že používáte specializovaný směrování potřebám. V takovém případě můžete vytvořit vlastní trasy.

Představte si například, že vytváříte aplikaci blogu. Můžete chtít zpracovat příchozí požadavky, které vypadají takto:

/ Archivu/12 – 25-2009

Když uživatel zadá tento požadavek, chcete vrátit blogu, který odpovídá na datum 12/25/2009. Aby bylo možné zpracovávat tento typ požadavku, je potřeba vytvořit vlastní trasy.

Výpis 1 soubor Global.asax obsahuje novou vlastní trasu s názvem blogu, které zpracovává požadavky, které mají tvar /Archive/*datum položky*.

**Výpis 1 - Global.asax (s vlastní směrování)**

[!code-vb[Main](creating-custom-routes-vb/samples/sample1.vb)]

Je důležité pořadí tras, které přidáte do směrovací tabulky. Před existující výchozí trasa se přidá naši novou vlastní trasu blogu. Pokud je obrácený pořadí, pak výchozí trasu vždy bude zavolána místo vlastní trasy.

Vlastní Blog trasa odpovídá všechny požadavky, které začíná/archivu /. Ano splňuje všechna následující adresy URL:

- / Archivu/12 – 25-2009

- / Archivu/10-6-2004

- / Archivu/apple

Vlastní trasy příchozí požadavek se mapuje na kontroler s názvem Archiv a vyvolá akci Entry(). Při volání metody Entry() datum položky se předá jako parametr s názvem entryDate.

Blog vlastní trasy můžete použít u kontroleru v zobrazení 2.

**Výpis 2 - ArchiveController.vb**

[!code-vb[Main](creating-custom-routes-vb/samples/sample2.vb)]

Všimněte si, že metoda Entry() ve výpisu 2 přijímá parametr typu DateTime. Architektura MVC je dostatečně inteligentní, aby automaticky převést datum položky z adresy URL na hodnotu data a času. Je-li parametr vstupní data z adresy URL nelze převést na typ DateTime, je vyvolána k chybě (viz obrázek 1).

**Obrázek 1 – Chyba z převodu parametru**


[![Dialogové okno Nový projekt](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)

**Obrázek 01**: Chyba z převodu parametru ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-custom-routes-vb/_static/image2.png))


## <a name="summary"></a>Souhrn

Cílem tohoto kurzu bylo ukazují, jak můžete vytvořit vlastní trasy. Jste zjistili, jak přidat vlastní trasy do směrovací tabulky v souboru Global.asax, která představuje dostupné zápisy z blogu. Jsme probírali jak mapovat požadavky na zápisy z blogu řadič ArchiveController a s názvem Entry() akce kontroleru.

> [!div class="step-by-step"]
> [Předchozí](asp-net-mvc-controller-overview-vb.md)
> [další](creating-a-route-constraint-vb.md)
