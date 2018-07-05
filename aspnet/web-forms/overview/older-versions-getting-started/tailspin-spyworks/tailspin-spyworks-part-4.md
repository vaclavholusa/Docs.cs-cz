---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: '4. část: Seznam produktů | Dokumentace Microsoftu'
author: JoeStagner
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. 4. část obsahuje seznam produktů s GridView sml...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 929d88d747c25ca7c4f6f991421cb3aa9456aa45
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368806"
---
<a name="part-4-listing-products"></a>4. část: Seznam produktů
====================
podle [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks ukazuje, jak mimořádně jednoduché je vytvářet výkonné a škálovatelné aplikace pro platformu .NET. Zobrazuje vypnout použití skvělých nových funkcí v technologii ASP.NET 4 k sestavení nebo online úložiště, včetně nákupu, Pokladna a správu.
> 
> V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. 4. část obsahuje seznam produktů pomocí ovládacího prvku GridView.


## <a id="_Toc260221670"></a>  Seznam produktů pomocí ovládacího prvku GridView.

Začněme implementace naši stránku ProductsList.aspx "Klikněte pravým tlačítkem myši klikněte na" na naše řešení a výběrem možnosti "Add" a "Nová položka".

![](tailspin-spyworks-part-4/_static/image1.jpg)

Zvolte možnost "Webové formuláře pomocí – stránka předlohy" a zadejte název stránky ProductsList.aspx".

Klikněte na tlačítko "Přidat".

![](tailspin-spyworks-part-4/_static/image2.jpg)

Dále vyberte složku "Styly", které jsme umístili Site.Master stránky a vyberte ho z okna "Obsah složky".

![](tailspin-spyworks-part-4/_static/image3.jpg)

Klikněte na tlačítko "Ok" k vytvoření stránky.

Databáze je naplněný daty produktu, jak vidíte níže.

![](tailspin-spyworks-part-4/_static/image4.jpg)

Po vytvoření naši stránku znovu použijeme Entity Data Source přistupovat k těmto datům produktu, ale v tomto případě potřebujeme vyberte entity, produktu a My potřebujeme k omezení položky, které jsou vráceny pouze na ty pro vybranou kategorii.

K tomu budete tom EntityDataSource automaticky generovat klauzuli WHERE a zadáme budete WhereParameter.

Budete si možná Vzpomínáte, že když jsme vytvořili položky nabídky v našich "kategorie nabídky produktů" dynamicky sestavili jsme odkaz tak, že přidáte CatagoryID řetězec dotazu pro každý odkaz. Budete informováni Entity zdroje dat se odvodit parametr WHERE z parametru řetězce dotazu.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

V dalším kroku nakonfigurujeme ovládací prvek ListView pro zobrazení seznamu produktů. Chcete-li vytvořit optimální prostředí pro nákup jsme vám několik funkcí stručné komprimace zobrazují v našem ListVew jednotlivé produkty.

- Název produktu, bude obsahovat odkaz k zobrazení podrobností produktu.
- Ceny produktu se zobrazí.
- Zobrazí bitovou kopii produktu a dynamicky vybereme image z Image adresáře katalogu v naší aplikaci.
- Společnost Microsoft bude obsahovat odkaz okamžitě přidat konkrétního produktu do nákupního košíku.

Toto je zápis pro naše instance ovládacího prvku ListView.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

Dynamicky vytváříme několik odkazů pro každý zobrazený produkt.

Také před testujeme vlastní nová stránka potřebujeme vytvořit strukturu adresářů pro produkt obrázky katalogu následujícím způsobem.

![](tailspin-spyworks-part-4/_static/image1.png)

Jakmile naše Image produktu jsou přístupné můžeme otestovat naše stránky produktu seznamu.

![](tailspin-spyworks-part-4/_static/image5.jpg)

Na domovské stránce webu klikněte na jeden z odkazů seznamu kategorií.

![](tailspin-spyworks-part-4/_static/image6.jpg)

Teď musíme implementovat ProductDetials.apsx stránky a funkce AddToCart.

Použijte soubor -&gt;nový název stránky ProductDetails.aspx pomocí web stránku předlohy, jako jsme to udělali dříve.

Znovu použijeme ovládací prvek třídu EntityDataSource platí pro přístup k určitým produktem záznam v databázi a zobrazení dat produktu takto použijeme ovládacího prvku FormView technologie ASP.NET.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

Nedělejte si starosti, pokud formátování vypadá trochu... pro vás. Výše uvedené značky ponechá místa v zobrazení rozložení pro několik funkcí, které budete později na implementaci.

Nákupního košíku se reprezentují složitější logiku v naší aplikaci. Abyste mohli začít, použijte soubor -&gt;nová vytvořit stránku s názvem MyShoppingCart.aspx.

Všimněte si, že jsme nejsou volba názvu ShoppingCart.aspx.

Naše databáze obsahuje tabulku s názvem "ShoppingCart". Jsme generování modelu Entity Data Model třídu byla vytvořena pro každou tabulku v databázi. Proto modelu Entity Data Model vygeneruje třídu Entity s názvem "ShoppingCart". Model jsme může upravit tak, že jsme mohli použít tento název pro naše implementace nákupního košíku nebo ji rozšířit pro naše požadavky na, ale jsme se místo toho vyjádřit souhlas se službou jednoduše vyberte název, který bude nedošlo ke konfliktu.

Je také vhodné poznamenat, že budeme vytvářet jednoduché nákupního košíku a vkládání nákupního košíku logiky pomocí zobrazení nákupního košíku. Také jsme rozhodnout k implementaci naše řešení nákupního košíku ve zcela samostatném obchodní vrstvě.

> [!div class="step-by-step"]
> [Předchozí](tailspin-spyworks-part-3.md)
> [další](tailspin-spyworks-part-5.md)
