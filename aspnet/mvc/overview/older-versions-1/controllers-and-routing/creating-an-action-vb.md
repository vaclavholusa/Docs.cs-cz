---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: "Vytváření akce (VB) | Microsoft Docs"
author: microsoft
description: "Zjistěte, jak přidat novou akci k řadiči ASP.NET MVC. Informace o požadavcích pro metodu jako akce."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: 3d1d355599c17df597f9c08d9d7f129fffc1a2e4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="creating-an-action-vb"></a>Vytváření akce (VB)
====================
podle [Microsoft](https://github.com/microsoft)

> Zjistěte, jak přidat novou akci k řadiči ASP.NET MVC. Informace o požadavcích pro metodu jako akce.


Cílem tohoto kurzu je vysvětlují, jak můžete vytvořit nové akce kontroleru. Můžete další informace o požadavcích metody akce. Také zjistíte, jak zabránit metodu zasahování přímo jako akce.

## <a name="adding-an-action-to-a-controller"></a>Přidání akce do řadiče

Přidat novou akci k řadiči přidáním nové metody pro kontroler. Například řadič v výpis 1 obsahuje akci s názvem Index() a akce s názvem SayHello(). Obě metody jsou zveřejněné jako akce.

**Výpis 1 - Controllers\HomeController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

Chcete-li být vystavený universe jako akce, metody musí splňovat určité požadavky:

- Metoda musí být veřejné.
- Metodu nelze statickou metodu.
- Metodu nelze metody rozšíření.
- Metodu nelze konstruktor, metody getter nebo setter.
- Metoda nemůže mít otevřete obecné typy.
- Metoda není metoda základní třídy controller.
- Nemůže obsahovat metodu **ref** nebo **out** parametry.

Všimněte si, že neexistují žádná omezení návratový typ akce kontroleru. Akce kontroleru vrátit řetězec, DateTime, instance třídy náhodných nebo void. Rozhraní ASP.NET MVC se převést žádný návratový typ, který není výsledek akce do řetězce a vykreslit řetězec do prohlížeče.

Když přidáte libovolné metody, která není porušují tyto požadavky na řadič, je metoda zpřístupněná jako akce kontroleru. Dávejte pozor, zde. Akce kontroleru můžete vyvolat každý, kdo připojený k Internetu. Například nevytvářejte DeleteMyWebsite() akce kontroleru.

## <a name="preventing-a-public-method-from-being-invoked"></a>Volaná brání veřejná metoda

Pokud potřebujete vytvořit veřejnou metodu do třídy kontroleru a nechcete, aby ke zveřejnění metodu jako akce kontroleru, pak můžete zabránit metodu volanou pomocí &lt;NonAction&gt; atribut. Například řadič v výpis 2 obsahuje veřejnou metodu s názvem CompanySecrets(), který je upraven pomocí &lt;NonAction&gt; atribut.

**Výpis 2 - Controllers\WorkController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

Pokud se pokusíte vyvolání akce kontroleru CompanySecrets() zadáním /Work/CompanySecrets do adresního řádku prohlížeče budete získání chybové zprávy na obrázku 1.


[![Volání metody NonAction](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)

**Obrázek 01**: volání metody NonAction ([Kliknutím zobrazit obrázek v plné velikosti](creating-an-action-vb/_static/image2.png))

>[!div class="step-by-step"]
[Předchozí](creating-a-controller-vb.md)
[další](aspnet-mvc-controllers-overview-cs.md)
