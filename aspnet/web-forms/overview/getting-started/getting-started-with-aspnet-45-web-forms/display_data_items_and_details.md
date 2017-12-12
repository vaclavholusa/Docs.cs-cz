---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: "Zobrazení dat položky a podrobnosti | Microsoft Docs"
author: Erikre
description: "Tento kurz řady naučit se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a Microsoft Visual Studio Express 2013 pro jsme..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 809d7a9c21a3ddf5dfd07d079eb8fe0d1d81712d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="display-data-items-and-details"></a>Zobrazení dat položky a podrobnosti
====================
Podle [Erik Reitan](https://github.com/Erikre)

[Stáhnout adresář Wingtip Toys ukázkového projektu (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) nebo [stáhnout elektronická kniha (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Tento kurz řady naučit se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a Microsoft Visual Studio Express 2013 pro Web. Visual Studio 2013 [projekt pomocí zdrojového kódu C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) dispozici je pro tento kurz řady.


Tento kurz popisuje, jak zobrazit datové položky a podrobnosti o položkách dat pomocí webových formulářů ASP.NET a Entity Framework Code First. V tomto kurzu vychází předchozí kurz "A uživatelského rozhraní navigace" a je součástí řady kurz Wingtip hračka úložiště. Po dokončení tohoto kurzu, budete moci zobrazit produkty na *ProductsList.aspx* stránky a podrobnosti o jednotlivých produktů na *ProductDetails.aspx* stránky.

## <a name="what-youll-learn"></a>Získáte informace:

- Postup přidání ovládacího prvku data k zobrazení produkty z databáze.
- Postup připojení ovládací prvek dat na vybraná data.
- Postup přidání ovládacího prvku dat. Chcete-li zobrazit podrobnosti o produktu z databáze.
- Jak načíst hodnotu z řetězce dotazu a tuto hodnotu použít k omezení dat, které se načítají z databáze.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Tyto jsou funkce, zavedená v tomto kurzu:

- Vazby modelu
- Zprostředkovatele hodnot

## <a name="adding-a-data-control-to-display-products"></a>Přidání ovládacího prvku Data k zobrazení produktů

Při vytváření vazby dat k ovládacímu prvku serveru, několika různými způsoby, které můžete použít. Nejběžnější možnosti zahrnují přidání ovládacího prvku zdroje dat, přidání kódu ručně nebo pomocí vazby modelu.

### <a name="using-a-data-source-control-to-bind-data"></a>K vytvoření vazby dat pomocí ovládacího prvku zdroje dat

Přidání ovládacího prvku zdroje dat můžete propojit prvek zdroje dat k ovládacímu prvku, který zobrazí data. Tento přístup umožňuje vám umožní deklarativně připojit se přímo do zdroje dat, nikoli pomocí programový přístup ovládacích prvků na straně serveru.

### <a name="coding-by-hand-to-bind-data"></a>Kódování ručně vázat Data

Přidání kódu ručně zahrnuje čtení hodnoty, kontrola hodnotu null, pokusu o převod na příslušný typ, kontrola, zda byl převod úspěšný a nakonec pomocí hodnoty v dotazu. Tento postup by použít, pokud je potřeba zachovat plnou kontrolu nad logika přístup k datům.

### <a name="using-model-binding-to-bind-data"></a>Pomocí vazby vázat Data modelu

Pomocí vazby modelu umožňuje svázání výsledky pomocí mnohem méně kódu a budete moci znovu použít funkci v celé vaší aplikaci. Pro zjednodušení práce s logiku zaměřené na kód přístup k datům a zároveň zachovat výhody bohaté, vazby dat framework cílem je vazby modelu.

## <a name="displaying-products"></a>Zobrazení produktů

V tomto kurzu použijete k vytvoření vazby dat vazby modelu. Pokud chcete konfigurovat ovládací prvek dat pomocí vazby modelu vyberte data, nastavte ovládacího prvku `SelectMethod` vlastnost na název metody v kódu stránky. Ovládací prvek dat volá metodu v příslušnou dobu v životním cyklu stránky a automaticky vytvoří vazbu vrácená data. Není nutné explicitně volat `DataBind` metoda.

Pomocí následujícího postupu, budete upravovat kód v *ProductList.aspx* stránky tak, aby stránce můžete zobrazit produkty.

1. V **Průzkumníku řešení**, otevřete *ProductList.aspx* stránky.
2. Nahraďte kód existující následující kód:   

    [!code-aspx[Main](display_data_items_and_details/samples/sample1.aspx)]

Tento kód používá **ListView** ovládací prvek s názvem "productList" zobrazíte produkty.

[!code-aspx[Main](display_data_items_and_details/samples/sample2.aspx)]

**ListView** ovládací prvek zobrazí data ve formátu, který definujete pomocí šablony a stylů. Je vhodné pro data v jakékoli opakující se podobě. To **ListView** příklad jednoduše zobrazuje data z databáze, ale můžete povolit uživatelům upravit, insert a odstranění dat a řazení a datům stránky, aniž byste museli kód.

Nastavením `ItemType` vlastnost v **ListView** řídit, vazby dat výraz `Item` je k dispozici a bude ovládacího prvku silného typu. Jak je uvedeno v předchozí kurzu, můžete vybrat Podrobnosti objektu položku pomocí IntelliSense, například určení `ProductName`:

![Zobrazení dat položky a podrobnosti - IntelliSense](display_data_items_and_details/_static/image1.png)

Kromě toho zadejte pomocí vazby modelu `SelectMethod` hodnotu. Tato hodnota (`GetProducts`) bude odpovídat metoda, která přidáte do kódu zobrazíte produkty v dalším kroku.

### <a name="adding-code-to-display-products"></a>Přidání kódu k zobrazení produktů

V tomto kroku přidáte kód k vyplnění **ListView** ovládacího prvku pomocí produktu data z databáze. Kód bude podporovat zobrazující produkty jednotlivé kategorie, jakož i zobrazuje všechny produkty.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na *ProductList.aspx* a pak klikněte na **kód zobrazení**.
2. Nahraďte stávající kód v *ProductList.aspx.cs* soubor s následujícím kódem:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

Tento kód ukazuje `GetProducts` metoda, která odkazuje `ItemType` vlastnost **ListView** řídit ve *ProductList.aspx* stránky. Omezit výsledky do určité kategorie v databázi, nastaví kód `categoryId` hodnotu z hodnotu řetězce dotazu, který je předán *ProductList.aspx* stránce, pokud *ProductList.aspx* stránka je přešli. `QueryStringAttribute` Třídy v `System.Web.ModelBinding` obor názvů se používá k načtení hodnoty id proměnné řetězce dotazu. Tím se nastaví vazby modelů a pokuste se vytvořit vazbu hodnotu z řetězce dotazu do `categoryId` parametr za běhu.

Když platný kategorie předána jako řetězec dotazu na stránku, výsledky dotazu jsou omezeny na tyto produkty v databázi, které odpovídají `categoryId` hodnotu. Například pokud adresu URL *ProductsList.aspx* stránka je následující:

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

Na stránce se zobrazuje pouze produkty, kde `category` rovná `1`.

Pokud žádný řetězec dotazu je zahrnuto při přechodu *ProductList.aspx* stránce se zobrazí všechny produkty.

Zdroje hodnot pro tyto metody jsou označovány jako *hodnotu zprostředkovatelé* (například *řetězce dotazu*), a atributy parametru, které indikují hodnota poskytovatele, kterého chcete použít se označují jako hodnota Zprostředkovatel atributy (například "`id`"). Technologie ASP.NET obsahuje zprostředkovatele hodnot a odpovídající atributy pro všechny z typických zdrojů uživatelský vstup ve formulářové webové aplikaci, například řetězec dotazu, soubory cookie, hodnot formuláře, ovládací prvky, stav zobrazení, stav relace a vlastnosti profilu. Je také možné zapsat zprostředkovatele vlastních hodnot.

### <a name="running-the-application"></a>Spuštění aplikace

Spusťte aplikaci teď chcete zobrazit, jak můžete zobrazit všechny produkty nebo jenom sadu produktů omezené podle kategorie.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *Default.aspx* a vyberte **zobrazit v prohlížeči**.  
 Prohlížeč a zobrazit *Default.aspx* stránky.
2. Vyberte **aut** v navigační nabídce kategorie produktu.  
 *ProductList.aspx* se zobrazí stránka zobrazuje jenom produkty, které jsou zahrnuty v kategorii "Aut". Později v tomto kurzu se zobrazí podrobnosti o produktu.  

    ![Zobrazení dat položky a podrobnosti - automobilů](display_data_items_and_details/_static/image2.png)
3. Vyberte **produkty** v navigační nabídce v horní části.  
 Znovu *ProductList.aspx* se zobrazí stránka, ale tentokrát zobrazuje celý seznam produktů.   

    ![Zobrazení dat položky a podrobnosti - produktů](display_data_items_and_details/_static/image3.png)
4. Zavřete prohlížeč a vraťte se k sadě Visual Studio.

### <a name="adding-a-data-control-to-display-product-details"></a>Přidání ovládacího prvku dat. Chcete-li zobrazit podrobnosti o produktu

Dále budete upravovat kód v *ProductDetails.aspx* stránku, kterou jste přidali v předchozím kurzu tak, aby stránce můžete zobrazit informace o jednotlivých produktů.

1. V **Průzkumníku řešení**, otevřete *ProductDetails.aspx* stránky.
2. Nahraďte kód existující následující kód:   

    [!code-aspx[Main](display_data_items_and_details/samples/sample5.aspx)]

Tento kód používá **FormView** ovládací prvek zobrazí podrobnosti o jednotlivých produktů. Tento kód používá metody, třeba ty, které se používají k zobrazení dat v *ProductList.aspx* stránky. **FormView** řízení slouží k zobrazení jeden záznam ze zdroje dat najednou. Při použití **FormView** ovládací prvek, je vytvořit šablony k zobrazení a úpravě hodnoty vázané na data. Šablony obsahují ovládací prvky, formátování a vazby výrazy definující vzhled a funkčnost formuláře.

Výše uvedený kód připojit k databázi, je nutné přidat další kód pro *ProductDetails.aspx* kódu.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na *ProductDetails.aspx* a pak klikněte na **kód zobrazení**.  
 *ProductDetails.aspx.cs* souboru se zobrazí.
2. Existujícího kódu nahraďte následujícím kódem:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

Tento kód zkontroluje "`productID`" hodnota řetězce dotazu. Pokud je nalezen hodnotu platný řetězec dotazu, zobrazí se odpovídající produktu. Pokud nenajde žádný řetězec dotazu, nebo hodnotu řetězce dotazu není platný, žádný produkt se zobrazí na *ProductDetails.aspx* stránky.

### <a name="running-the-application"></a>Spuštění aplikace

Teď můžete spustit aplikace, abyste viděli zobrazí jednotlivé produkty na základě id produktu.

1. Stiskněte klávesu **F5** při v sadě Visual Studio a spusťte aplikaci.  
 Prohlížeč a zobrazit *Default.aspx* stránky.
2. Vyberte "Lodě" v navigační nabídce kategorie.  
 *ProductList.aspx* zobrazí se stránka.
3. Vyberte produkt "Dokumentu člun" ze seznamu produktu.  
 *ProductDetails.aspx* zobrazí se stránka.   

    ![Zobrazení dat položky a podrobnosti - produktů](display_data_items_and_details/_static/image4.png)
4. Zavřete prohlížeč.

## <a name="summary"></a>Souhrn

V tomto kurzu řady mají přidáte značek a kódu, pokud chcete zobrazit seznam produktů a zobrazit podrobnosti o produktu. Během tohoto procesu jste se naučili o ovládacích prvcích silného typu dat, vazby modelu a zprostředkovatele hodnot. V dalším kurzu přidáte nákupní košík na adresář Wingtip Toys ukázkovou aplikaci.

## <a name="additional-resources"></a>Další prostředky

[Načítání a zobrazování dat pomocí vazby modelu a webové formuláře](../../presenting-and-managing-data/model-binding/retrieving-data.md)

>[!div class="step-by-step"]
[Předchozí](ui_and_navigation.md)
[další](shopping-cart.md)
