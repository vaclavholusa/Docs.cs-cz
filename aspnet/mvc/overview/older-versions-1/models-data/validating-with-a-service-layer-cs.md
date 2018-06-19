---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
title: Ověřování pomocí služby vrstvu (C#) | Microsoft Docs
author: StephenWalther
description: Další informace o přesunutí logika ověřování z vaší akce kontroleru a do vrstvy samostatné služby. V tomto kurzu Stephen Walther vysvětluje, jak můžete...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 4eabc535-b8a1-43f5-bb99-cfeb86db0fca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 06042ac197cc54da767a94a44c57eb09bb3db9fa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869033"
---
<a name="validating-with-a-service-layer-c"></a>Ověřování pomocí služby vrstvu (C#)
====================
podle [Stephen Walther](https://github.com/StephenWalther)

> Další informace o přesunutí logika ověřování z vaší akce kontroleru a do vrstvy samostatné služby. V tomto kurzu Stephen Walther vysvětluje, jak je možné uchovávat sharp oddělené oblasti zájmu tak, že izoluje vrstvě služby z vašeho řadiče vrstvy.


Cílem tohoto kurzu je popisují jednu z metod ověřování v aplikaci ASP.NET MVC. V tomto kurzu zjistěte, jak přesunout logika ověřování z řadičů a do vrstvy samostatné služby.

## <a name="separating-concerns"></a>Oddělení otázky

Když vytvoříte aplikaci ASP.NET MVC, by neměla umístit logika databáze uvnitř vaší akce kontroleru. Kombinování logika databáze a řadič díky aplikaci obtížnější Údržba v průběhu času. Doporučuje se, umístěte všechny logika databáze ve vrstvě samostatné úložiště.

Například výpis 1 obsahuje jednoduché úložiště s názvem ProductRepository. Produkt úložiště obsahuje všechny data přístupový kód aplikace. Seznam také zahrnuje rozhraní IProductRepository, který implementuje úložiště produktu.

**Listing 1 -- Models\ProductRepository.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample1.cs)]

Řadič v výpis 2 používá vrstvy úložiště v jeho Index() a Create() akce. Všimněte si, že tento řadič neobsahuje žádné databáze logiku. Vytvoření vrstvy úložiště umožňuje udržovat čistou oddělené oblasti zájmu. Řadiče jsou zodpovědní za logiku aplikace toku řízení a úložiště je zodpovědná za data přístup logiku.

**Výpis 2 - Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample2.cs)]

## <a name="creating-a-service-layer"></a>Vytvoření vrstvy služby

Ano aplikace toku řízení logika patří řadič a data přístup logika patří v úložišti. V takovém případě, kde vložíte logika ověřování? Jednou z možností je logika ověřování v umístit *vrstvy služby*.

Vrstva služby je další vrstvy v aplikaci ASP.NET MVC, která zprostředkovává komunikaci mezi řadiče a vrstvy úložiště. Vrstva služby obsahuje obchodní logiku. Konkrétně obsahuje logiku ověření.

Například vrstvy služby produktu ve výpisu 3 má CreateProduct() metodu. CreateProduct() metoda volá metodu ValidateProduct() k ověření nového produktu před předáním produktu do úložiště produktu.

**Výpis 3 - Models\ProductService.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample3.cs)]

Kontroler produktu se aktualizovalo v výpis 4 používat vrstvu služby namísto vrstvy úložiště. Vrstva řadiče komunikuje se vrstvě služby. Vrstva služby komunikuje se vrstvy úložiště. Jednotlivé úrovně má samostatnou zodpovědnost.

**Výpis 4 - Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample4.cs)]

Všimněte si, vytvořený službu produktu v konstruktoru řadiče produktu. Při vytváření služby ze slovníku stavu modelu je předán do služby. Služba produktu používá předávání chybových zpráv ověření zpátky na řadič stavu modelu.

## <a name="decoupling-the-service-layer"></a>Oddělení vrstvě služby

Nemůžeme se nepodařilo izolovat řadiče a vrstvy služby v jednom ohledu. Řadiče a vrstvy služby komunikují prostřednictvím stavu modelu. Jinými slovy vrstvě služby má závislost na konkrétní funkce rozhraní ASP.NET MVC.

Chceme izolovat vrstvě služby z našich řadiče vrstvy co nejvíc. Teoreticky jsme byste měli mít vrstvy služby pomocí libovolného typu aplikace a ne jenom aplikaci ASP.NET MVC. Například v budoucnu může chceme sestavení front-end pro naši aplikaci WPF. Jsme by měl zjistit způsob, jak odebrat závislost na rozhraní ASP.NET MVC stav modelu z našich vrstvy služby.

Ve výpisu 5 vrstvě služby byla aktualizována tak, aby již nepoužíval stavu modelu. Místo toho použije všechny třídy, která implementuje rozhraní IValidationDictionary.

**Výpis 5 - Models\ProductService.cs (odpojené)**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample5.cs)]

Rozhraní IValidationDictionary je definována v výpis 6. Toto jednoduché rozhraní má jedné metody a vlastnosti jediné.

**Výpis 6 - Models\IValidationDictionary.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample6.cs)]

Třídy ve výpisu 7, s názvem třídy ModelStateWrapper implementuje rozhraní IValidationDictionary. Můžete vytvořit instanci třídy ModelStateWrapper předáním slovník stavu modelu konstruktoru.

**Výpis 7 – Models\ModelStateWrapper.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample7.cs)]

Nakonec aktualizované řadiče v výpis 8 používá ModelStateWrapper při vytváření vrstvě služby v jeho konstruktoru.

**Výpis 8 - Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample8.cs)]

Použití IValidationDictionary rozhraní a třídy ModelStateWrapper umožňuje nám zcela izolovat naše vrstvy služby z našich řadiče vrstvy. Už není závislá na stav modelu vrstvě služby. Můžete předat všechny třídy, která implementuje rozhraní IValidationDictionary k vrstvě služby. Například může aplikace WPF implementovat rozhraní IValidationDictionary s třída jednoduchých kolekcí.

## <a name="summary"></a>Souhrn

Cílem tohoto kurzu bylo popisují jeden z přístupů k provádění ověření v aplikaci ASP.NET MVC. V tomto kurzu jste zjistili, jak přesunout všechny logika ověřování z řadičů a do vrstvy samostatné služby. Také jste zjistili, jak izolovat vrstvě služby z vašeho řadiče vrstvy ModelStateWrapper třídu.

> [!div class="step-by-step"]
> [Předchozí](validating-with-the-idataerrorinfo-interface-cs.md)
> [další](validation-with-the-data-annotation-validators-cs.md)
