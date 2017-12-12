---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: "Část 4: Výpis produkty | Microsoft Docs"
author: JoeStagner
description: "Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 4 obsahuje výpis produkty s sml GridView..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 420cdbcc002bcbfff619d399a7a374009999d754
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="part-4-listing-products"></a>Část 4: Seznam produktů
====================
podle [Stagner Jan](https://github.com/JoeStagner)

> Tailspin Spyworks ukazuje, jak neobvykle jednoduché je vytvoření efektivní a škálovatelné aplikace pro platformu .NET. Zobrazuje vypnout používání skvělé nové funkce v rozhraní ASP.NET 4 k sestavení online úložiště, včetně nákupy, najdete v článku věnovaném a správu.
> 
> Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 4 obsahuje výpis produkty pomocí ovládacího prvku GridView.


## <a id="_Toc260221670"></a>Výpis produkty pomocí ovládacího prvku GridView

Začněme implementace naší stránce s ProductsList.aspx kliknutím pravým tlačítkem"" na naše řešení a výběr "Přidat" a "Nová položka".

![](tailspin-spyworks-part-4/_static/image1.jpg)

Zvolte "Webové formuláře pomocí – stránka předlohy" a zadejte název stránky ProductsList.aspx".

Klikněte na tlačítko "Přidat".

![](tailspin-spyworks-part-4/_static/image2.jpg)

Dále vyberte "Styly" složku, kam jsme umístit stránku Site.Master a vyberte ho v okně "Obsah složky".

![](tailspin-spyworks-part-4/_static/image3.jpg)

Klikněte na tlačítko "Ok" k vytvoření stránky.

Databáze je naplněný daty produktu, jak vidíte níže.

![](tailspin-spyworks-part-4/_static/image4.jpg)

Po vytvoření naši stránku znovu použijeme k zdroji dat Entity přístup k těmto datům produktu, ale v této instanci, je potřeba vyberte entity produktu a je potřeba omezit položky, které se vrátí jenom ty pro vybranou kategorii.

K tomu můžeme EntityDataSource pro automatické generování informujeme klauzule WHERE a určíme budete WhereParameter.

Budete si Vzpomínáte, že pokud jsme vytvořili položek nabídky v našem "kategorie nabídky produktů" jsme dynamicky vytvořili odkaz tak, že přidáte do řetězce dotazu pro každý odkaz CatagoryID. Budete informováni Entity zdroje dat pro parametr kde odvozena z tohoto parametru řetězce dotazu.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

V dalším kroku nakonfigurujeme ovládacího prvku ListView zobrazíte seznam produktů. Chcete-li vytvořit nákupní optimálních jsme budete na každé jednotlivé produkt zobrazí v našem ListVew compact několik stručným funkcí.

- Název produktu, bude odkaz na zobrazení podrobností produktu.
- Zobrazí se ceny produktu.
- Zobrazí se obrázek produktu a jsme dynamicky vyberte bitovou kopii z adresáře, bitové kopie katalogu v naší aplikaci.
- Jsme bude obsahovat odkaz na okamžitě přidat konkrétní produkt nákupního košíku.

Zde je kód pro naše instance ovládacího prvku ListView.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

Dynamicky vytváříte několik odkazů pro jednotlivé zobrazené produkty.

Také jsme otestovat vlastní nová stránka musíme vytvořit strukturu adresáře pro produkt obrázky katalogu takto.

![](tailspin-spyworks-part-4/_static/image1.png)

Jakmile jsou přístupné obrázky našich produktů, abychom mohli otestovat naše stránky produktu seznamu.

![](tailspin-spyworks-part-4/_static/image5.jpg)

Z domovské stránky daného webu klikněte na jeden z odkazů seznamu kategorie.

![](tailspin-spyworks-part-4/_static/image6.jpg)

Teď musíme implementovat stránce ProductDetials.apsx a AddToCart funkce.

Pomocí souboru -&gt;nová vytvořit název stránky ProductDetails.aspx pomocí webu – stránka předlohy, jako jsme to udělali dřív.

Ovládací prvek EntityDataSource budeme znovu používat pro přístup k určitým produktem záznam v databázi a budeme používat ovládací prvek ASP.NET FormView zobrazíte data produktu.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

Nebojte se, pokud formátování, které vypadá trochu věc pro vás. Kód výše ponechá místa v zobrazení rozložení pro několik funkcí, které budete později na implementaci.

Nákupní košík bude reprezentovat složitější logiku v naší aplikaci. Chcete-li začít, použijte soubor -&gt;nová vytvořit stránku s názvem MyShoppingCart.aspx.

Všimněte si, že jsme nejsou volba názvu ShoppingCart.aspx.

Naše databáze obsahuje tabulku s názvem "ShoppingCart". Když jsme datového modelu Entity třídu byl vytvořen pro každou tabulku v databázi. Proto datového modelu Entity vygeneruje třídu Entity s názvem "ShoppingCart". Model jsme může upravit tak, aby jsme může použijte tento název pro naše nákupní košík implementace nebo rozšířit pro naše potřeby, ale jsme se místo toho opt jednoduše vyberte název, který bude nedošlo ke konfliktu.

Je také vhodné poznamenat, že budeme vytvářet jednoduché nákupní košík a vkládání logice nákupní košík s hodnotou display nákupní košík. Také jsme možné implementovat naše řešení nákupního košíku zcela oddělenou obchodní vrstvy.

>[!div class="step-by-step"]
[Předchozí](tailspin-spyworks-part-3.md)
[další](tailspin-spyworks-part-5.md)
