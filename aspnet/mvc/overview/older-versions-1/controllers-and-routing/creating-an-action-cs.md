---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: Vytvoření akce (C#) | Dokumentace Microsoftu
author: microsoft
description: Zjistěte, jak přidat novou akci kontroler ASP.NET MVC. Další informace o požadavcích pro metodu na akci.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: 243248ee30c6a2db7f102f7743d0393d4a6a9d24
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755186"
---
<a name="creating-an-action-c"></a>Vytvoření akce (C#)
====================
podle [Microsoft](https://github.com/microsoft)

> Zjistěte, jak přidat novou akci kontroler ASP.NET MVC. Další informace o požadavcích pro metodu na akci.


Cílem tohoto kurzu je vysvětlují, jak můžete vytvořit nové akce kontroleru. Informace o požadavky na metodu akce. Také se dozvíte, jak zabránit metodu vystaven jako akci.

## <a name="adding-an-action-to-a-controller"></a>Přidání akce Kontroleru

Přidejte novou akci k řadiči tak, že přidáte nové metody pro kontroler. Například řadič v informacích 1 obsahuje akce s názvem Index() a akce s názvem SayHello(). Obě metody jsou vystaveny jako akce.

**Výpis 1 - Controllers\HomeController.cs**

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

Aby bylo možné vystavit rozhraní universe jako akci, metoda musí splňovat určité požadavky:

- Metoda musí být veřejné.
- Metoda nemůže být statickou metodu.
- Metoda nemůže být metodou rozšíření.
- Metoda nemůže být konstruktor, metoda getter nebo setter.
- Metoda nemůže mít otevřených obecných typů.
- Metoda není metodu základní třídy kontroleru.
- Metoda nemůže obsahovat **ref** nebo **si** parametry.

Všimněte si, že neexistují žádná omezení u návratového typu akce kontroleru. Akce kontroleru vrátit řetězec, DateTime, instance třídy Random nebo void. Architektura ASP.NET MVC se převést návratový typ, který není výsledek akce do řetězce a vykreslit řetězec, který se v prohlížeči.

Když přidáte jakoukoli metodu, která nebudou porušovat tyto požadavky na řadič, metoda je vystavena jako akce kontroleru. Buďte opatrní tady. Akce kontroleru můžete vyvolat každý připojený k Internetu. Ne, například vytvořit DeleteMyWebsite() akce kontroleru.

## <a name="preventing-a-public-method-from-being-invoked"></a>Brání veřejnou metodu volanou

Pokud je potřeba vytvořit veřejnou metodu do třídy kontroleru a nechcete vystavit metodu akce kontroleru můžete zabránit metody vyvolání pomocí atributu [NonAction]. Například kontroler v informacích 2 obsahuje veřejnou metodu s názvem CompanySecrets(), který je upraven pomocí atributu [NonAction].

**Výpis 2 - Controllers\WorkController.cs**

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

Pokud při pokusu o vyvolání akce kontroleru CompanySecrets() zadáním /Work/CompanySecrets do adresního řádku prohlížeče zobrazí se zpráva chyby na obrázku 1.


[![Volání metody NonAction](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)

**Obrázek 01**: volání metody NonAction ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-an-action-cs/_static/image2.png))

> [!div class="step-by-step"]
> [Předchozí](creating-a-controller-cs.md)
> [další](asp-net-mvc-routing-overview-vb.md)
