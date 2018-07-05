---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Zobrazení datových položek a podrobností | Dokumentace Microsoftu
author: Erikre
description: V této sérii kurzů se seznámíte se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a službu Microsoft Visual Studio Express 2013 pro jsme...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 08efafc1ae076bcf481c5c7d8aea2bbaa704af17
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379735"
---
<a name="display-data-items-and-details"></a>Zobrazení datových položek a podrobností
====================
podle [Erik Reitan](https://github.com/Erikre)

[Stáhněte si ukázkový projekt Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) nebo [stáhnout elektronickou knihu (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> V této sérii kurzů se seznámíte se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a službu Microsoft Visual Studio Express 2013 for Web. Visual Studio 2013 [projektu se zdrojovým kódem jazyka C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) je k dispozici v této sérii kurzů.


Tento kurz popisuje způsob zobrazení datových položek a podrobnosti o položkách data pomocí webových formulářů ASP.NET a Entity Framework Code First. V tomto kurzu vychází z předchozí kurz o službě "Uživatelské rozhraní a navigace" a je součástí série kurzů Wingtip Slonovi Store. Po dokončení tohoto kurzu, budete moct zobrazit produkty na *ProductsList.aspx* stránky a podrobnosti o jednotlivých produktů na *ProductDetails.aspx* stránky.

## <a name="what-youll-learn"></a>Co se dozvíte:

- Postup přidání ovládacího prvku dat pro zobrazení produktů z databáze.
- Postup připojení ovládacího prvku na vybraná data.
- Postup přidání ovládacího prvku dat. Chcete-li zobrazit podrobnosti o produktu z databáze.
- Jak načíst hodnotu z řetězce dotazu a tuto hodnotu použít k omezení dat načtených z databáze.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Toto jsou vlastnosti představené v tomto kurzu:

- Vazby modelu
- Zprostředkovatele hodnot

## <a name="adding-a-data-control-to-display-products"></a>Přidání ovládacího prvku dat pro zobrazení produktů

Při vytváření vazby dat k ovládacímu prvku serveru, několika různými způsoby, kterými můžete. Nejběžnější možnosti zahrnují přidání ovládací prvek zdroje dat, přidání kódu ručně nebo pomocí vazby modelu.

### <a name="using-a-data-source-control-to-bind-data"></a>Vytvoření vazby dat pomocí ovládacího prvku zdroje dat

Přidání ovládacího prvku zdroje dat umožňuje propojit ovládací prvek zdroje dat k ovládacímu prvku, který se zobrazí data. Tento přístup umožňuje vám umožní deklarativně připojit se přímo do zdroje dat, nikoli pomocí programový přístup serverové ovládací prvky.

### <a name="coding-by-hand-to-bind-data"></a>Ručně kódování až po vytvoření vazby dat

Přidání kódu ručně zahrnuje čtení hodnoty, kontrola hodnot null, pokusu o převod na příslušný typ, kontroluje, zda byl převod úspěšný a nakonec pomocí hodnoty v dotazu. Tento postup byste použili, když budete chtít zachovat plnou kontrolu nad logiky přístupu k datům.

### <a name="using-model-binding-to-bind-data"></a>Pomocí modelu vazba k vytvoření vazby dat

Pomocí vazby modelu umožňuje vytvořit vazbu výsledky pomocí mnohem méně kódu a poskytuje možnosti opakovaně používat funkce v rámci aplikace. Vazby modelu cílem je zjednodušit práci s logikou zaměřený na kód – přístup k datům při zachování výhod framework bohatě vybaveným a datové vazby.

## <a name="displaying-products"></a>Zobrazení produkty

V tomto kurzu použijete k vytvoření vazby dat vazby modelu. Chcete-li nakonfigurovat ovládací prvek data pomocí vazby modelu vyberte data, nastavte ovládacího prvku `SelectMethod` nastavte název metody v kódu stránky. Ovládací prvek dat volá metodu v příslušnou dobu v životním cyklu stránky a automaticky sváže s vrácenými daty. Není nutné explicitně volat `DataBind` metody.

Pomocí následujícího postupu upravíte značky *ProductList.aspx* stránce tak, aby na stránce můžete zobrazit produkty.

1. V **Průzkumníka řešení**, otevřete *ProductList.aspx* stránky.
2. Nahraďte stávající kód následujícím kódem:   

    [!code-aspx[Main](display_data_items_and_details/samples/sample1.aspx)]

Tento kód používá **ListView** ovládací prvek s názvem "productList" pro zobrazení produktů.

[!code-aspx[Main](display_data_items_and_details/samples/sample2.aspx)]

**ListView** ovládací prvek zobrazuje data ve formátu, který definujete pomocí šablony a styly. Je vhodné pro data v libovolné opakující se struktura. To **ListView** příklad jednoduše zobrazí data z databáze, ale můžete povolit uživatelům upravit, vložení a odstranění dat a seřadit a datům stránky, aniž byste museli kód.

Nastavením `ItemType` vlastnost **ListView** řídit vazbový výraz `Item` je k dispozici a ovládací prvek stane silného typu. Jak je uvedeno v předchozím kurzu, můžete vybrat Podrobnosti objektu položky díky technologii IntelliSense, jako je například určení `ProductName`:

![Zobrazení dat položek a podrobnosti o – technologie IntelliSense](display_data_items_and_details/_static/image1.png)

Kromě toho používáte vazby modelu k určení `SelectMethod` hodnotu. Tato hodnota (`GetProducts`) bude odpovídat metoda, která přidáte do kódu pro zobrazení produktů v dalším kroku.

### <a name="adding-code-to-display-products"></a>Přidání kódu pro zobrazení produktů

V tomto kroku přidáte kód k vyplnění **ListView** ovládacího prvku pomocí produktu data z databáze. Kód bude podporovat zobrazující produktů podle jednotlivých kategorií, jakož i zobrazující všechny produkty.

1. V **Průzkumníka řešení**, klikněte pravým tlačítkem na *ProductList.aspx* a potom klikněte na tlačítko **zobrazit kód**.
2. Nahraďte existující kód ve třídě *ProductList.aspx.cs* souboru následujícím kódem:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

Tento kód ukazuje `GetProducts` metodu, která odkazuje `ItemType` vlastnost **ListView** v ovládacím prvku *ProductList.aspx* stránky. Omezit rozsah výsledků do určité kategorie v databázi, kód nastaví `categoryId` hodnotu z hodnotu řetězce dotazu, který je předán *ProductList.aspx* stránce, pokud *ProductList.aspx* stránka je Přejde. `QueryStringAttribute` Třídy v `System.Web.ModelBinding` obor názvů slouží k načtení hodnoty id proměnné řetězce dotazu. Toto dá pokyn vazby modelu pro pokus o vytvoření vazby hodnotu z řetězce dotazu do `categoryId` parametr v době běhu.

Když platnou kategorii je předán jako řetězec dotazu na stránce výsledky dotazu jsou omezené na tyto produkty v databázi, které odpovídají `categoryId` hodnotu. Například pokud adresa URL *ProductsList.aspx* stránka je následující:

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

Na stránce se zobrazí pouze produkty, kde `category` rovná `1`.

Pokud žádný řetězec dotazu je dostupná při přechodu na *ProductList.aspx* stránce se zobrazí všechny produkty.

Zdroje hodnoty pro tyto metody jsou označovány jako *hodnota poskytovatelé* (například *řetězce dotazu*), a parametr atributy, které označují které zprostředkovatele hodnot pro použití se označují jako hodnota Zprostředkovatel atributů (jako například "`id`"). Technologie ASP.NET obsahuje zprostředkovatele hodnot a odpovídající atributy pro všemi typické zdroji uživatelský vstup v aplikaci webových formulářů, jako je například řetězec dotazu, soubory cookie, hodnot formuláře, ovládací prvky, zobrazit stav, stav relace a vlastnosti profilu. Můžete taky psát vlastní hodnotu poskytovatelů.

### <a name="running-the-application"></a>Spuštění aplikace

Spusťte aplikaci teď zobrazíte, jak můžete zobrazit všechny produkty nebo jenom sadu produktů podle kategorie omezené.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *Default.aspx* stránku a vybrat **zobrazit v prohlížeči**.  
 V prohlížeči se otevře a zobrazí *Default.aspx* stránky.
2. Vyberte **auta** z navigační nabídky kategorie produktu.  
 *ProductList.aspx* se zobrazí stránka zobrazující pouze produkty, které jsou zahrnuté v kategorii cars (auta). Později v tomto kurzu se zobrazí podrobnosti o produktu.  

    ![Zobrazení dat položek a podrobnosti - automobilů](display_data_items_and_details/_static/image2.png)
3. Vyberte **produkty** z navigační nabídce v horní části.  
 Znovu *ProductList.aspx* se zobrazí stránka, ale tentokrát zobrazuje úplný seznam produktů.   

    ![Zobrazení dat položek a podrobnosti - produkty](display_data_items_and_details/_static/image3.png)
4. Zavřete prohlížeč a vraťte se do sady Visual Studio.

### <a name="adding-a-data-control-to-display-product-details"></a>Přidání ovládacího prvku dat. Chcete-li zobrazit podrobnosti o produktu

V dalším kroku upravíte značky *ProductDetails.aspx* stránka, která jste přidali v předchozím kurzu tak, aby na stránce můžete zobrazit informace o jednotlivých produktů.

1. V **Průzkumníka řešení**, otevřete *ProductDetails.aspx* stránky.
2. Nahraďte stávající kód následujícím kódem:   

    [!code-aspx[Main](display_data_items_and_details/samples/sample5.aspx)]

Tento kód používá **FormView** ovládací prvek pro zobrazení podrobností o jednotlivých produktů. Tento kód použije metody podobné těm, které se používají k zobrazení dat v *ProductList.aspx* stránky. **FormView** ovládacího prvku se používá k zobrazení jeden záznam v čase ze zdroje dat. Při použití **FormView** ovládacího prvku, je vytvořit šablony můžete zobrazit a upravit hodnoty vázané na data. Šablony obsahují ovládací prvky, vazba výrazy a formátování, které definují vzhled a funkce formuláře.

Pro připojení k databázi výše uvedené značky, je nutné přidat dalšího kódu, který *ProductDetails.aspx* kódu.

1. V **Průzkumníka řešení**, klikněte pravým tlačítkem na *ProductDetails.aspx* a potom klikněte na tlačítko **zobrazit kód**.  
   *ProductDetails.aspx.cs* souboru se zobrazí.
2. Nahraďte stávající kód následujícím kódem:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

Tento kód kontroluje "`productID`" hodnotu řetězce dotazu. Pokud není nalezena hodnota platný řetězec dotazu, zobrazí se odpovídající produkt. Pokud se nenajde žádný řetězec dotazu, nebo hodnotu řetězce dotazu není platný, žádný produkt, který se zobrazí na *ProductDetails.aspx* stránky.

### <a name="running-the-application"></a>Spuštění aplikace

Nyní můžete spustit aplikaci, abyste viděli zobrazí jednotlivé produkty na základě id produktu.

1. Stisknutím klávesy **F5** během činnosti v sadě Visual Studio ke spuštění aplikace.  
 V prohlížeči se otevře a zobrazí *Default.aspx* stránky.
2. Z kategorie navigační nabídce vyberte "Lodě".  
 *ProductList.aspx* zobrazí se stránka.
3. Vyberte produkt "Papíru loď" ze seznamu produktů.  
 *ProductDetails.aspx* zobrazí se stránka.   

    ![Zobrazení dat položek a podrobnosti - produkty](display_data_items_and_details/_static/image4.png)
4. Zavřete prohlížeč.

## <a name="summary"></a>Souhrn

V tomto kurzu této série mají přidání značek a kódu k zobrazení seznamu produktů a zobrazit podrobnosti o produktu. Během tohoto procesu jste se dozvěděli o ovládací prvky dat silného typu, vazby modelu a zprostředkovatele hodnot. V dalším kurzu přidáte do nákupního košíku ukázkové aplikace Wingtip Toys.

## <a name="additional-resources"></a>Další prostředky

[Načtení a zobrazení dat ovládacím prvkem vazby modelu a webové formuláře](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> [Předchozí](ui_and_navigation.md)
> [další](shopping-cart.md)
