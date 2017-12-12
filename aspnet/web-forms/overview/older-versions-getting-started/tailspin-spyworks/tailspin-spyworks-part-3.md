---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: "Část 3: Rozložení a kategorie nabídky | Microsoft Docs"
author: JoeStagner
description: "Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 3 zahrnuje přidání rozložení a kategorie nabídky."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 57c0342efb67b94a0d8c8b06dc13a727e7184db8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="part-3-layout-and-category-menu"></a>Část 3: Rozložení a kategorie nabídky
====================
podle [Stagner Jan](https://github.com/JoeStagner)

> Tailspin Spyworks ukazuje, jak neobvykle jednoduché je vytvoření efektivní a škálovatelné aplikace pro platformu .NET. Zobrazuje vypnout používání skvělé nové funkce v rozhraní ASP.NET 4 k sestavení online úložiště, včetně nákupy, najdete v článku věnovaném a správu.
> 
> Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 3 zahrnuje přidání rozložení a kategorie nabídky.


## <a id="_Toc260221669"></a>Přidání některé rozložení a kategorie nabídky

V našem stránka přidáme div pro sloupec levé straně, který bude obsahovat našich produktů kategorie nabídky.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

Všimněte si, že požadované zarovnání a další formátování bude poskytována třídy CSS, který jsme přidali do našich soubor Style.css.

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

V nabídce kategorie produktu dynamicky vytvoří za běhu prostřednictvím dotazů na databázi a obchodu Spojených států pro existující kategorie produktů a vytváření položek nabídky a odpovídající odkazy.

K tomu budeme používat dvě ASP. Ovládací prvky na NET výkonné data. Prvek "Zdroje dat Entity" a "ListView" řízení.

![](tailspin-spyworks-part-3/_static/image1.jpg)

Umožňuje přepnout do "Zobrazení návrhu" a použijte pomocníky ke konfiguraci naše ovládací prvky.

![](tailspin-spyworks-part-3/_static/image2.jpg)

Umožňuje nastavit vlastnost EntityDataSource ID na EDS\_kategorie\_nabídky a klikněte na zdroj dat"konfigurace".

![](tailspin-spyworks-part-3/_static/image3.jpg)

Vyberte CommerceEntities připojení, která byla vytvořena pro nás, když jsme vytvořili Model Entity zdroje dat pro naše obchodu Spojených států v databázi a klikněte na tlačítko "Další".

![](tailspin-spyworks-part-3/_static/image4.jpg)

Vyberte název sady entit "Kategorie" a ponechejte zbývající možnosti jako výchozí. Klikněte na tlačítko "Dokončit".

Teď umožňuje nastavit vlastnosti ID instance ovládacího prvku ListView, která nemůžeme kladena na naší stránce ListView\_ProductsMenu a aktivujte její pomocné rutiny.

![](tailspin-spyworks-part-3/_static/image5.jpg)

Když jsme může používat možnosti řízení k formátování položky dat zobrazení a formátování, naše vytvoření nabídky bude pouze vyžadovat jednoduchých značek tak jsme zadá kód v zobrazení zdroje.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

Poznámka: příkaz "Eval": &lt;% Eval("CategoryName") % #&gt;

Syntaxe ASP.NET &lt;% # %&gt; je sdružená konvence, který se dá pokyn modulu runtime spustit ať je obsažena v a výstup výsledků "v řádku".

Příkaz Eval("CategoryName") dá pokyn, že aktuální položku v vázané kolekce datových položek, načtěte hodnotě názvů položka modelu Entity "CatagoryName". Toto je stručný syntaxe pro velmi výkonné funkce.

Umožňuje spustit nyní aplikaci.

![](tailspin-spyworks-part-3/_static/image6.jpg)

Všimněte si, že našich produktů kategorie nabídky se nyní zobrazí a když jsme více než jedné z položek nabídky kategorie vidíme body odkaz položky nabídky na stránku, musíme ještě implementovat najeďte pojmenovaný ProductsList.aspx a že Vyvinuli jsme argument řetězce dynamické dotazu, obsahuje  id kategorie.

>[!div class="step-by-step"]
[Předchozí](tailspin-spyworks-part-2.md)
[další](tailspin-spyworks-part-4.md)
