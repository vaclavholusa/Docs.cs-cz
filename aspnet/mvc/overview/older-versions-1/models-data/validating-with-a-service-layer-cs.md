---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
title: Ověřování vrstvou služby (C#) | Dokumentace Microsoftu
author: StephenWalther
description: Další informace o přesunutí logiky ověřování mimo vaše akce kontroleru a do samostatné služby vrstvy. V tomto kurzu, Stephen Walther vysvětluje, jak budete...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 4eabc535-b8a1-43f5-bb99-cfeb86db0fca
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 8f533fca3357d060f072e2e28d05071d636cd9e6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362301"
---
<a name="validating-with-a-service-layer-c"></a>Ověřování vrstvou služby (C#)
====================
podle [Stephen Walther](https://github.com/StephenWalther)

> Další informace o přesunutí logiky ověřování mimo vaše akce kontroleru a do samostatné služby vrstvy. V tomto kurzu Stephen Walther vysvětluje, jak díky čemuž můžete udržovat sharp oddělené oblasti zájmu izolováním vrstvě služby mezi kontroleru.


Cílem tohoto kurzu je k popisu jednu z metod ověřování v aplikaci ASP.NET MVC. V tomto kurzu se dozvíte, jak k přesunutí logiky ověřování mimo řadičů a do samostatné služby vrstvy.

## <a name="separating-concerns"></a>Oddělení otázky

Když sestavíte aplikaci ASP.NET MVC, by neměla logiky databáze umístíte text do vaší akce kontroleru. Kombinování logiky databáze a kontroler je aplikace obtížné udržovat v čase. Doporučujeme umísťovat celý logiky databáze ve vrstvě oddělené úložiště.

Výpis 1 například obsahuje jednoduché úložiště s názvem ProductRepository. Úložiště produkt obsahuje všechny kód přístupu k datům pro aplikaci. Seznam zahrnuje také rozhraní IProductRepository, který implementuje úložiště produktu.

**Výpis 1 – Models\ProductRepository.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample1.cs)]

Řadič v informacích 2 používá vrstvy úložiště v jeho Index() a Create() akce. Všimněte si, že tento kontroler neobsahuje žádnou logiku databáze. Vytvoření vrstvy úložiště umožňuje udržovat jasně oddělit oblasti zájmu. Kontrolery jsou zodpovědné za logiku řízení toku aplikace a úložiště je zodpovědný za logikou přístupu k datům.

**Výpis 2 - Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample2.cs)]

## <a name="creating-a-service-layer"></a>Vytvoření vrstvy služby

Tedy aplikace tok řízení logika patří kontroler a logiky přístupu k datům patří v úložišti. V takovém případě místo, kam dáte logiky ověřování? Jednou z možností je umístit logiky ověřování v *vrstva služby*.

Vrstva služby představuje další vrstvu v aplikaci ASP.NET MVC, která zprostředkovává komunikaci mezi řadičem a vrstvy úložiště. Vrstva služby obsahuje logiku. Konkrétně obsahuje logiku ověřování.

Například vrstva služby produkt ve verzi 3 výpis obsahuje metodu CreateProduct(). Metoda CreateProduct() volá ValidateProduct() metodu pro ověření nového produktu před předáním produktu do úložiště produktu.

**Výpis 3 - Models\ProductService.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample3.cs)]

Kontroler produktu se aktualizoval v informacích 4 nahrazujícím vrstvu služby vrstvě úložiště. Vrstva kontroleru hovoří se vrstva služby. Přednášky vrstvu služby vrstvě úložiště. Každá vrstva má samostatné odpovědnost.

**Část 4 – Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample4.cs)]

Všimněte si, že služby se vytvoří v konstruktoru kontroler produktu. Po vytvoření služby se předá slovníku stavů modelu ve službě. Služba produktu pomocí modelu stavu projít ověřením chybové zprávy zpět do kontroleru.

## <a name="decoupling-the-service-layer"></a>Oddělení vrstva služby

Můžeme se nepodařilo izolovat kontroleru a vrstvy služby v jednom ohledu. Kontroler a vrstvy služby komunikují prostřednictvím ze stavů modelu. Jinými slovy vrstva služby obsahuje závislost na konkrétní funkce architektury ASP.NET MVC.

Chcete izolovat vrstva služby z našich řadič vrstvy co největší míře. Teoreticky jsme měl vrstvy služby pomocí libovolného typu aplikace a ne jenom aplikace ASP.NET MVC. Například v budoucnu může chceme vytvořit front-endu pro naši aplikaci WPF. Jsme měli hledal způsob, jak odebrat závislost na rozhraní ASP.NET MVC stavu modelu z našich vrstvy služby.

Výpis 5 vrstva služby byla aktualizována tak, aby již nepoužíval ze stavů modelu. Místo toho používá jakoukoli třídu, která implementuje rozhraní IValidationDictionary.

**Výpis 5 - Models\ProductService.cs (oddělené)**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample5.cs)]

IValidationDictionary rozhraní je definováno v informacích 6. Toto jednoduché rozhraní má jedné metody a vlastnosti jediné.

**Výpis 6 - Models\IValidationDictionary.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample6.cs)]

Výpis 7, s názvem třídy ModelStateWrapper implementuje rozhraní IValidationDictionary. Můžete vytvořit instanci třídy ModelStateWrapper předáním slovníku stavů modelu do konstruktoru.

**Výpis 7 - Models\ModelStateWrapper.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample7.cs)]

Nakonec aktualizované řadič v informacích 8 používá ModelStateWrapper při vytvoření vrstvy služby 've svém konstruktoru.

**Výpis 8 - Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample8.cs)]

Použití IValidationDictionary rozhraní a třídy ModelStateWrapper nám umožňuje zcela izolovat naše vrstva služby z našich vrstvy kontroleru. Vrstva služby již není závislý na stavu modelu. Můžete předat jakoukoli třídu, která implementuje rozhraní IValidationDictionary pro vrstvu služby. Aplikace WPF může například implementovat rozhraní IValidationDictionary s třída jednoduchých kolekcí.

## <a name="summary"></a>Souhrn

Cílem tohoto kurzu byla fattica jedním z přístupů k provedení ověření v aplikaci ASP.NET MVC. V tomto kurzu jste zjistili, jak chcete přesunout všechny logiky ověřování mimo řadičů a do samostatné služby vrstvy. Také jste zjistili, jak izolovat vrstvě služby mezi řadiče vytvořením ModelStateWrapper třídy.

> [!div class="step-by-step"]
> [Předchozí](validating-with-the-idataerrorinfo-interface-cs.md)
> [další](validation-with-the-data-annotation-validators-cs.md)
