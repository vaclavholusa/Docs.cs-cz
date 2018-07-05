---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: '3. část: Nabídka rozložení a kategorie | Dokumentace Microsoftu'
author: JoeStagner
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. 3. část popisuje přidání rozložení a kategorie nabídky.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 72642610c203127b431e03214b2f1bc85a85b5fb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366644"
---
<a name="part-3-layout-and-category-menu"></a>3. část: Nabídka rozložení a kategorie
====================
podle [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks ukazuje, jak mimořádně jednoduché je vytvářet výkonné a škálovatelné aplikace pro platformu .NET. Zobrazuje vypnout použití skvělých nových funkcí v technologii ASP.NET 4 k sestavení nebo online úložiště, včetně nákupu, Pokladna a správu.
> 
> V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. 3. část popisuje přidání rozložení a kategorie nabídky.


## <a id="_Toc260221669"></a>  Přidání některých rozložení a kategorie nabídky

V našem stránku předlohy přidáme div sloupce na levé straně, který bude obsahovat naší nabídce kategorie produktu.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

Všimněte si, že požadované zarovnání a další formátování budou poskytovat třídu šablony stylů CSS, který jsme přidali na náš soubor Style.css.

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

V nabídce kategorie produktu dynamicky vytvoří za běhu pomocí dotazu Commerce databázi pro existující kategorie produktů a vytváření položek nabídky a odpovídající odkazy.

K tomu že použijeme dvě ASP. Výkonné datové ovládací prvky vaší sítě. Ovládací prvek "Entity Data Source" a "ListView" ovládací prvek.

![](tailspin-spyworks-part-3/_static/image1.jpg)

Umožňuje přepnout na "Návrhové zobrazení" a použijte pomocníky ke konfiguraci naše ovládací prvky.

![](tailspin-spyworks-part-3/_static/image2.jpg)

Pojďme nastavit vlastnost EntityDataSource ID EDS\_kategorie\_nabídky a klikněte na zdroj dat"konfigurace".

![](tailspin-spyworks-part-3/_static/image3.jpg)

Vyberte CommerceEntities připojení, který byl vytvořen pro nás, když jsme vytvořili Model Entity zdroje dat pro naše obchodní databáze a klikněte na tlačítko "Následující".

![](tailspin-spyworks-part-3/_static/image4.jpg)

Vyberte název sady entit "Kategorie" a ponechte zbývající možnosti jako výchozí. Klikněte na tlačítko "Dokončit".

Nyní Pojďme nastavit vlastnost ID instance ovládacího prvku ListView, který se nám na naší stránce ListView\_ProductsMenu a aktivovat její pomocné rutiny.

![](tailspin-spyworks-part-3/_static/image5.jpg)

I když možnosti ovládání bychom mohli použít k formátování položky zobrazení dat a formátování, vytváří nabídky budete potřebovat jenom jednoduché značky tak jsme přejde kód v zobrazení zdroje.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

Poznámka: příkaz "Eval": &lt;% # Eval("CategoryName") %&gt;

Syntaxe ASP.NET &lt;% # %&gt; je zkrácený tvar vlastností konvence, který dává pokyn modul runtime umožňující spouštění cokoli, co je součástí a vypíše výsledky "na řádku".

Příkaz Eval("CategoryName") dává pokyn, že pro aktuální položku v vázané kolekce položek dat, načíst změnu hodnoty názvy položek Entity Model "CatagoryName". Toto je stručné syntaxe pro velmi výkonné funkce.

Můžete spustit nyní aplikaci.

![](tailspin-spyworks-part-3/_static/image6.jpg)

Mějte na paměti, že naše nabídky kategorie produktu se teď zobrazuje a kdy jsme při najetí myší více než jednu z položek nabídky kategorie vidíme odkaz směřuje položky nabídky na stránku, musíme ještě implementovat pojmenované ProductsList.aspx a, jsme vyvinuli řetězcový argument dynamický dotaz, který obsahuje  id kategorie.

> [!div class="step-by-step"]
> [Předchozí](tailspin-spyworks-part-2.md)
> [další](tailspin-spyworks-part-4.md)
