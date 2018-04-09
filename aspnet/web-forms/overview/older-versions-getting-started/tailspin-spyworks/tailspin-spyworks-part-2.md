---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Část 2: Data Access Layer | Microsoft Docs'
author: JoeStagner
description: Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 2 popisuje přidání vrstva přístupu k datům.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9f734b04a0f4cec3c33bc5b42ef283ea64cdb463
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="part-2-data-access-layer"></a>Část 2: Data Access Layer
====================
podle [Stagner Jan](https://github.com/JoeStagner)

> Tailspin Spyworks ukazuje, jak neobvykle jednoduché je vytvoření efektivní a škálovatelné aplikace pro platformu .NET. Zobrazuje vypnout používání skvělé nové funkce v rozhraní ASP.NET 4 k sestavení online úložiště, včetně nákupy, najdete v článku věnovaném a správu.
> 
> Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 2 popisuje přidání vrstva přístupu k datům.


## <a id="_Toc260221668"></a>  Přidání Data Access Layer

Naše aplikace pro elektronické obchodování bude záviset na dvě databáze.

Informace o odběrateli použijeme standardní databáze členství technologie ASP.NET. Pro naše nákupní košík a produkt katalog jsme budete implementovat databázi SQL Express následujícím způsobem.

![](tailspin-spyworks-part-2/_static/image1.jpg)

Obsahující vytvořené databáze (Commerce.mdf) v aplikaci aplikace\_složky dat je možné přejít k vytvoření naše Data Access Layer pomocí rozhraní .NET Framework Entity.

Vytvoříme složku s názvem "Data\_přístup" a jejich klikněte pravým tlačítkem na této složky a vyberte možnost "Přidat novou položku".

"Nainstalovaných šablonách" položku a pak vyberte položku "ADO.NET Entity Data Model" Zadejte EDM\_Commerce.edmx jako název a klikněte na tlačítko "Přidat".

![](tailspin-spyworks-part-2/_static/image2.jpg)

Zvolte "Generování z databáze".

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

Uložit a sestavit.

Nyní jsme připraveni přidat naše první funkce – Kategorie nabídky produktů.

> [!div class="step-by-step"]
> [Předchozí](tailspin-spyworks-part-1.md)
> [další](tailspin-spyworks-part-3.md)
