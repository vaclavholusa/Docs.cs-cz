---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
title: Nákupní košík | Dokumentace Microsoftu
author: Erikre
description: V této sérii kurzů se seznámíte se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a službu Microsoft Visual Studio Express 2013 pro jsme...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 6898c601-6c31-432f-8388-e6843f8a17cb
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
msc.type: authoredcontent
ms.openlocfilehash: 22253b8708efd9ce505c9fbeb9cb2e942588b37d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375775"
---
<a name="shopping-cart"></a>Nákupní košík
====================
podle [Erik Reitan](https://github.com/Erikre)

[Stáhněte si ukázkový projekt Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) nebo [stáhnout elektronickou knihu (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> V této sérii kurzů se seznámíte se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a službu Microsoft Visual Studio Express 2013 for Web. Visual Studio 2013 [projektu se zdrojovým kódem jazyka C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) je k dispozici v této sérii kurzů.


Tento kurz popisuje obchodní logiku potřebnou k přidání do nákupního košíku do webových formulářů ASP.NET ukázkové aplikace Wingtip Toys. V tomto kurzu vychází z předchozí kurz o "Zobrazení dat a podrobnosti o položkách" a je součástí série kurzů Wingtip Slonovi Store. Po dokončení tohoto kurzu, budou uživatelé ukázkovou aplikaci přidat, odebrat nebo změnit produkty v jejich nákupního košíku.

## <a name="what-youll-learn"></a>Co se dozvíte:

1. Postup vytvoření nákupního košíku pro webovou aplikaci.
2. Jak povolit uživatelům přidávat položky do nákupního košíku.
3. Postup přidání [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview(v=vs.110).aspx#introduction) ovládací prvek pro zobrazení podrobností nákupního košíku.
4. Jak vypočítat a zobrazit ordinálního součtu.
5. Postup odstranění a aktualizovat položky v nákupním košíku.
6. Postup zahrnutí nákupního košíku čítače.

## <a name="code-features-in-this-tutorial"></a>Funkce kódu v tomto kurzu:

1. Entity Framework Code First
2. Datové poznámky
3. Ovládací prvky dat silného typu
4. Vazby modelu

## <a name="creating-a-shopping-cart"></a>Vytvoření nákupního košíku

Dříve v této řadě kurzů jste přidali stránky a kód, chcete-li zobrazit data produktů z databáze. V tomto kurzu vytvoříte nákupního košíku pro správu produktů, že jsou uživatelé zájem o nákup. Uživatelé budou moct Procházet a přidat položky do nákupního košíku, i když nejsou zaregistrované nebo přihlášení. Ke správě přístupu nákupního košíku, kterého přiřadíte uživatelům jedinečný `ID` pomocí globálně jedinečný identifikátor (GUID), když uživatel přistupuje k nákupního košíku poprvé. Toto úložiště `ID` používání stavu relace ASP.NET.

> [!NOTE] 
> 
> Stav relace technologie ASP.NET je vhodné místo pro ukládání informací specifických pro uživatele, jejíž platnost vyprší po opuštění webu. Zatímco zneužití stav relace může mít vliv na výkon na větších serverech, světle užívání relace stavu funguje dobře pro demonstrační účely. Ukázkový projekt na adresář Wingtip Toys ukazuje způsob použití stavu relace bez externího poskytovatele, kde stavu relace je uložené v procesu na webovém serveru hostujícím webu. Pro větší serverů, které poskytují více instancí aplikace nebo weby, na kterých běží více instancí aplikace na různých serverech, zvažte použití **služby Windows Azure Cache Service**. Tato služba mezipaměti umožňuje distribuované ukládání do mezipaměti služba, která je externí webový server a řeší problém používání stavu relace v procesu. Další informace najdete v tématu [postupy používání stavu relace ASP.NET pomocí modelu weby Windows Azure](https://docs.microsoft.com/azure/redis-cache/cache-aspnet-session-state-provider).


### <a name="add-cartitem-as-a-model-class"></a>Přidat CartItem jako třídu modelu

Dříve v této řadě kurzů definované schéma pro data kategorií a produktu tím, že vytvoříte `Category` a `Product` tříd v *modely* složky. Teď přidejte novou třídu definovat schéma pro nákupní košík. Později v tomto kurzu přidáte třídy ke zpracování přístupu k datům `CartItem` tabulky. Tato třída bude poskytovat obchodní logiky k přidání, odebrání a aktualizovat položky v nákupním košíku.

1. Klikněte pravým tlačítkem myši *modely* a pak zvolte položku **přidat**  - &gt; **nová položka**. 

    ![Nákupní košík – nová položka](shopping-cart/_static/image1.png)
2. **Přidat novou položku** se zobrazí dialogové okno. Vyberte **kód**a pak vyberte **třídy**. 

    ![Nákupní košík - přidat novou položku – dialogové okno](shopping-cart/_static/image2.png)
3. Název této nové třídy *CartItem.cs*.
4. Klikněte na tlačítko **přidat**.  
   Nový soubor třídy se zobrazí v editoru.
5. Nahraďte kód následujícím kódem:   

    [!code-csharp[Main](shopping-cart/samples/sample1.cs)]

`CartItem` Třída obsahuje schéma, které bude definovat každý produkt uživatel přidá do nákupního košíku. Tato třída je podobně jako jiné třídy schématu, kterou jste vytvořili dříve v této sérii kurzů. Podle konvence platformy Entity Framework Code First očekává, primární klíč pro `CartItem` tabulka může mít hodnotu `CartItemId` nebo `ID`. Kód však přepisuje výchozí chování s použitím anotace data `[Key]` atribut. `Key` Atribut ItemId vlastnost určuje, že `ItemID` vlastnost představuje primární klíč.

`CartId` Vlastnost určuje, `ID` uživatele, který je přidružený k položce k nákupu. Přidáte kód k vytvoření tohoto uživatele `ID` když uživatel přistupuje k nákupního košíku. To `ID` se uloží také jako proměnnou relace technologie ASP.NET.

### <a name="update-the-product-context"></a>Aktualizace produktu kontextu

Kromě přidání `CartItem` třídy, budete muset aktualizovat třídy kontextu databáze, která spravuje tříd entit, který poskytuje přístup k datům v databázi. Chcete-li to provést, přidejte nově vytvořený `CartItem` třída do modelu `ProductContext` třídy.

1. V **Průzkumníka řešení**, najít a otevřít *ProductContext.cs* soubor *modely* složky.
2. Přidejte zvýrazněný kód, který *ProductContext.cs* to následujícím způsobem:  

    [!code-csharp[Main](shopping-cart/samples/sample2.cs?highlight=14)]

Jak už bylo zmíněno dříve v této řadě kurzů, kód v *ProductContext.cs* přidá soubor `System.Data.Entity` obor názvů, abyste měli přístup ke všem funkcím základní rozhraní Entity Framework. Tato funkce zahrnuje možnost pro dotazování, vložení, aktualizace a odstranění dat při práci s objektů se silným typem. `ProductContext` Třídy přidá přístup do nově přidaný `CartItem` třída modelu.

### <a name="managing-the-shopping-cart-business-logic"></a>Správa nákupního košíku obchodní logiky

V dalším kroku vytvoříte `ShoppingCart` tříd v novém *logiky* složky. `ShoppingCart` Třída zpracovává při přístupu k datům `CartItem` tabulky. Třída bude také obsahovat obchodní logiky k přidání, odebrání a aktualizovat položky v nákupním košíku.

Nákupní košík logiku, která přidáte bude obsahovat funkce pro správu následující akce:

1. Přidávání položek do nákupního košíku
2. Odebrání položek z nákupního košíku
3. Získání ID nákupního košíku
4. Načítání položek z nákupního košíku
5. Součet množství všechny nákupního košíku položky
6. Aktualizace dat nákupního košíku

Na stránce nákupní košík (*ShoppingCart.aspx*) a třídu nákupního košíku se použije společně pro přístup k datům nákupního košíku. Na stránce nákupní košík se zobrazí všechny položky, které uživatel přidá do nákupního košíku. Kromě nákupního košíku stránky a třídy, vytvoříte stránku (*AddToCart.aspx*) přidejte produkty do nákupního košíku. Bude také přidat kód do *ProductList.aspx* stránky a *ProductDetails.aspx* stránka, která bude obsahovat odkaz na *AddToCart.aspx* stránky, takže uživatel může přidávat produkty do nákupního košíku.

Následující diagram znázorňuje základní proces, ke které dochází, když uživatel přidá produktu do nákupního košíku.

![Nákupní košík – přidání do nákupního košíku](shopping-cart/_static/image3.png)

Pokud uživatel klikne **přidat do košíku** odkazu na buď *ProductList.aspx* stránky nebo *ProductDetails.aspx* stránce aplikace přejde *AddToCart.aspx* stránky a pak automaticky do *ShoppingCart.aspx* stránky. *AddToCart.aspx* voláním metody ve třídě ShoppingCart stránku přidá vyberte produktu do nákupního košíku. *ShoppingCart.aspx* stránce se zobrazí produkty, které byly přidány do nákupního košíku.

#### <a name="creating-the-shopping-cart-class"></a>Vytvoření třídy nákupního košíku

`ShoppingCart` Třídy se přidají do samostatné složky v aplikaci tak, že bude jasně odlišit modelu (složka modelů), na stránkách (do kořenové složky) a logiku (logiku složky).

1. V **Průzkumníka řešení**, klikněte pravým tlačítkem myši **Northwind**projektu a vyberte **přidat**-&gt;**novou složku**. Název nové složky *logiky*.
2. Klikněte pravým tlačítkem myši *logiky* složku a pak vyberte **přidat**  - &gt; **nová položka**.
3. Přidejte nový soubor třídy s názvem *ShoppingCartActions.cs*.
4. Nahraďte kód následujícím kódem:   

    [!code-csharp[Main](shopping-cart/samples/sample3.cs)]

`AddToCart` Metoda umožňuje jednotlivé produkty, které mají být zahrnuty do nákupního košíku podle produktu `ID`. Produkt se přidá do košíku, nebo pokud košíku již obsahuje položku pro tento produkt, se zvýší množství.

`GetCartId` Metoda vrátí košíku `ID` pro daného uživatele. Košíku `ID` se používá ke sledování položky, které má uživatel v jejich nákupního košíku. Pokud uživatel nemá existující košíku `ID`, nové nákupní seznam `ID` je pro ně vytvořili. Pokud je uživatel přihlášený jako registrovaný uživatel košíku `ID` je nastavena na jejich uživatelskému jménu. Nicméně pokud uživatel není přihlášený v košíku `ID` je nastavena na jedinečnou hodnotu (GUID). Identifikátor GUID zajišťuje, že pouze jeden košíku se vytvoří pro každého uživatele, založené na relaci.

`GetCartItems` Metoda vrátí seznam hodnot položky v nákupním košíku pro daného uživatele. Později v tomto kurzu, zobrazí se, že vazba modelu se používá k zobrazení košíku položky do nákupního košíku pomocí `GetCartItems` metody.

### <a name="creating-the-add-to-cart-functionality"></a>Vytvoření funkce přidat do košíku

Jak už bylo zmíněno dříve, vytvoříte zpracování stránky s názvem *AddToCart.aspx* , který se použije k přidání do nákupního košíku uživatele nové produkty. Tato stránka bude volat `AddToCart` metodu `ShoppingCart` třídu, která jste právě vytvořili. *AddToCart.aspx* stránky bude očekávat, že produkt `ID` je předán. Tento produkt `ID` se použije při volání `AddToCart` metodu `ShoppingCart` třídy.

> [!NOTE] 
> 
> Bude změna modelu code-behind (*AddToCart.aspx.cs*) pro tuto stránku, není na stránce uživatelského rozhraní (*AddToCart.aspx*).


#### <a name="to-create-the-add-to-cart-functionality"></a>Chcete-li vytvořit přidat do košíku funkce:

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **Northwind**projektu, klikněte na tlačítko **přidat**  - &gt; **nová položka**.  
   **Přidat novou položku** se zobrazí dialogové okno.
2. Přidejte novou stránku standardní (webové formuláře) pro aplikaci s názvem *AddToCart.aspx*. 

    ![Nákupní košík – přidejte webový formulář](shopping-cart/_static/image4.png)
3. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *AddToCart.aspx* stránce a potom klikněte na tlačítko **zobrazit kód**. *AddToCart.aspx.cs* použití modelu code-behind soubor je otevřen v editoru.
4. Nahraďte existující kód ve třídě *AddToCart.aspx.cs* modelu code-behind následujícím kódem:   

    [!code-csharp[Main](shopping-cart/samples/sample4.cs)]

Když *AddToCart.aspx* načtení stránky, produkt `ID` je načten z řetězce dotazu. V dalším kroku je instance třídy nákupního košíku vytvořit a použít k volání `AddToCart` metodu, která jste přidali dříve v tomto kurzu. `AddToCart` Metoda součástí *ShoppingCartActions.cs* souboru, obsahuje logiku pro přidání vybraného produktu do nákupního košíku nebo zvýšení množství produktů vybranému produktu. Pokud produkt nebyl přidán do nákupního košíku, produktu je přidána do `CartItem` tabulky databáze. Pokud uživatel přidá další položka stejný produkt produktu se už přidala do nákupního košíku, se v zvýší množství produktu `CartItem` tabulky. Nakonec přesměruje na stránku zpět *ShoppingCart.aspx* stránka, která přidáte v dalším kroku, ve kterém se uživateli zobrazí aktualizovaný seznam položek v košíku.

Jak už jsme zmínili, uživatel `ID` slouží k identifikaci produkty, které jsou spojeny s konkrétním uživatelem. To `ID` se přidá na řádek v `CartItem` tabulky pokaždé, když uživatel přidá produktu do nákupního košíku.

### <a name="creating-the-shopping-cart-ui"></a>Vytvoření nákupního košíku uživatelského rozhraní

*ShoppingCart.aspx* stránce se zobrazí produkty, které uživatel přidal do svého nákupního košíku. Také bude poskytovat možnost přidávat, odebírat a aktualizovat položky v nákupním košíku.

1. V **Průzkumníka řešení**, klikněte pravým tlačítkem na **Northwind**, klikněte na tlačítko **přidat**  - &gt; **nová položka**.  
   **Přidat novou položku** se zobrazí dialogové okno.
2. Přidejte novou stránku (webové formuláře), která obsahuje stránku předlohy tak, že vyberete **webový formulář používající stránku předlohy**. Zadejte název nové stránky *ShoppingCart.aspx*.
3. Vyberte **Site.Master** připojení na nově vytvořený na hlavní stránce *.aspx* stránky.
4. V *ShoppingCart.aspx* stránka, nahraďte existující kód následujícím kódem:   

    [!code-aspx[Main](shopping-cart/samples/sample5.aspx)]

*ShoppingCart.aspx* stránka obsahuje **GridView** ovládací prvek s názvem `CartList`. Tento ovládací prvek používá vazbu modelu z databáze, kterou chcete vytvořit vazbu dat nákupního košíku **GridView** ovládacího prvku. Při nastavení `ItemType` vlastnost **GridView** řídit vazbový výraz `Item` je k dispozici v značky ovládacího prvku a ovládací prvek stane silného typu. Jak je uvedeno výše v této sérii kurzů, můžete vybrat podrobnosti `Item` pomocí technologie IntelliSense. Chcete-li nakonfigurovat ovládací prvek data pomocí vazby modelu vyberte data, nastavte `SelectMethod` vlastnost ovládacího prvku. Ve výše uvedené značky, můžete nastavit `SelectMethod` GetShoppingCartItems metody, které vrací seznam `CartItem` objekty. **GridView** ovládací prvek dat volá metodu v příslušnou dobu v životním cyklu stránky a automaticky sváže s vrácenými daty. `GetShoppingCartItems` – Metoda musí být přidána.

#### <a name="retrieving-the-shopping-cart-items"></a>Načítání položek nákupního košíku

V dalším kroku přidáte kód, který *ShoppingCart.aspx.cs* modelu code-behind načíst a naplňte jimi rozhraní nákupního košíku.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *ShoppingCart.aspx* stránce a potom klikněte na tlačítko **zobrazit kód**. *ShoppingCart.aspx.cs* použití modelu code-behind soubor je otevřen v editoru.
2. Nahraďte stávající kód následujícím kódem:  

    [!code-csharp[Main](shopping-cart/samples/sample6.cs)]

Jak je uvedeno výše, `GridView` dat řídit volání `GetShoppingCartItems` metoda v příslušnou dobu života stránky cyklu a automaticky vytvoří vazbu vrácená data. `GetShoppingCartItems` Metoda vytvoří instanci `ShoppingCartActions` objektu. Potom kód použije tuto instanci k vrácení položek v košíku voláním `GetCartItems` metody.

### <a name="adding-products-to-the-shopping-cart"></a>Přidání produktů do nákupního košíku

Když buď *ProductList.aspx* nebo *ProductDetails.aspx* se zobrazí stránka, uživatel bude moct přidat produktu do nákupního košíku pomocí odkazu. Po kliknutí na odkaz, aplikace přejde na stránku zpracování s názvem *AddToCart.aspx*. *AddToCart.aspx* stránky bude volat `AddToCart` metodu `ShoppingCart` třídu, která jste přidali dříve v tomto kurzu.

Nyní, přidáte **přidat do košíku** odkaz na obě *ProductList.aspx* stránky a *ProductDetails.aspx* stránky. Tento odkaz bude obsahovat produktu `ID` , který je načten z databáze.

1. V **Průzkumníka řešení**, najít a otevřít stránku s názvem *ProductList.aspx*.
2. Přidejte značky zvýrazněné žlutou barvou na *ProductList.aspx* stránce tak, aby celé stránky se zobrazí takto:  

    [!code-aspx[Main](shopping-cart/samples/sample7.aspx?highlight=50-54)]

### <a name="testing-the-shopping-cart"></a>Testování nákupního košíku

Spusťte aplikaci, abyste viděli, jak přidat produkty do nákupního košíku.

1. Stisknutím klávesy **F5** ke spuštění aplikace.  
 Po projekt znovu vytvoří databázi, v prohlížeči se otevře a zobrazí *Default.aspx* stránky.
2. Vyberte **auta** navigační nabídce kategorie.  
 *ProductList.aspx* se zobrazí stránka zobrazující pouze produkty, které jsou zahrnuté v kategorii cars (auta). 

    ![Nákupní košík - automobilů](shopping-cart/_static/image5.png)
3. Klikněte na tlačítko **přidat do košíku** uvedený odkaz vedle první produktu (převoditelné auta).   
 *ShoppingCart.aspx* se zobrazí stránka zobrazující výběr v nákupním košíku. 

    ![Nákupní košík - košíku](shopping-cart/_static/image6.png)
4. Zobrazit další produkty tak, že vyberete **rovin** navigační nabídce kategorie.
5. Klikněte na tlačítko **přidat do košíku** odkaz vedle první produktu uvedené.  
 *ShoppingCart.aspx* zobrazí se stránka s další položky.
6. Zavřete prohlížeč.

### <a name="calculating-and-displaying-the-order-total"></a>Výpočet a zobrazování ordinálního součtu

Kromě přidání do nákupního košíku produkty, které přidáte `GetTotal` metodu `ShoppingCart` třídy a zobrazí celková částka objednávky na stránce nákupní košík.

1. V **Průzkumníku řešení**, otevřete *ShoppingCartActions.cs* soubor *logiky* složky.
2. Přidejte následující `GetTotal` metoda zvýrazněné žlutou barvou na `ShoppingCart` třídy tak, aby třída se zobrazí takto:   

    [!code-csharp[Main](shopping-cart/samples/sample8.cs?highlight=85-97)]

Nejprve je potřeba `GetTotal` metoda získá ID nákupního košíku pro daného uživatele. Potom metoda získá košíku celkový vynásobením cena produktu množství produktu pro jednotlivé produkty uvedené v košíku.

> [!NOTE] 
> 
> Výše uvedený kód používá typ s možnou hodnotou Null "`int?`". Typy připouštějící hodnotu Null, může představovat všechny hodnoty z nadřazeného typu a také jako hodnotu null. Další informace najdete v tématu [typy připouštějící hodnotu Null pomocí](https://msdn.microsoft.com/library/2cf62fcy(v=vs.110).aspx).


### <a name="modify-the-shopping-cart-display"></a>Upravit zobrazení nákupního košíku

Dále upravíte kód *ShoppingCart.aspx* stránku pro volání `GetTotal` metody a zobrazení, které celkem *ShoppingCart.aspx* stránce při načtení stránky.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *ShoppingCart.aspx* stránku a vybrat **zobrazit kód**.
2. V *ShoppingCart.aspx.cs* soubor, aktualizovat `Page_Load` obslužná rutina přidáním následujícího kódu zvýrazněné žlutou barvou:   

    [!code-csharp[Main](shopping-cart/samples/sample9.cs?highlight=16-31)]

Když *ShoppingCart.aspx* stránka načte, načte objekt nákupního košíku a pak načte nákupního košíku celkem voláním `GetTotal` metodu `ShoppingCart` třídy. Pokud nákupní košík je prázdný, se zobrazí příslušná zpráva.

### <a name="testing-the-shopping-cart-total"></a>Testování nákupního košíku celkem

Spusťte aplikaci teď zobrazíte, jak nelze pouze přidání produktu do nákupního košíku, ale můžete zobrazit celkový počet nákupního košíku.

1. Stisknutím klávesy **F5** ke spuštění aplikace.  
 V prohlížeči se otevře a zobrazí *Default.aspx* stránky.
2. Vyberte **auta** navigační nabídce kategorie.
3. Klikněte na tlačítko **přidat do košíku** odkaz vedle první produktu.   
 *ShoppingCart.aspx* zobrazí se stránka s ordinálního součtu. 

    ![Nákupní košík - celkem košíku](shopping-cart/_static/image7.png)
4. Některé produkty (například roviny) přidáte do košíku.
5. *ShoppingCart.aspx* zobrazí se stránka s aktualizovanou celkový součet pro všechny produkty, které jste přidali. 

    ![Nákupní košík - více produktů](shopping-cart/_static/image8.png)
6. Zavření okna prohlížeče zastavte spuštěnou aplikaci.

### <a name="adding-update-and-checkout-buttons-to-the-shopping-cart"></a>Přidání tlačítka rezervovat a aktualizace do nákupního košíku

Povolit uživatelům změnit nákupního košíku, přidáte **aktualizace** tlačítko a **Checkout** tlačítko na stránku nákupní košík. **Checkout** tlačítko nepoužívá až později v této sérii kurzů.

1. V **Průzkumníka řešení**, otevřete *ShoppingCart.aspx* stránky v kořenovém adresáři projektu webové aplikace.
2. Přidat **aktualizace** tlačítko a **Checkout** tlačítko *ShoppingCart.aspx* stránce, přidání značek zvýrazněné žlutou barvou na existující značky, jak je znázorněno Následující kód:   

    [!code-aspx[Main](shopping-cart/samples/sample10.aspx?highlight=36-45)]

Pokud uživatel klikne **aktualizace** tlačítko, `UpdateBtn_Click` obslužná rutina události zavolá se. Tato obslužná rutina události zavolá kód, který přidáte v dalším kroku.

V dalším kroku můžete aktualizovat kód obsažený v *ShoppingCart.aspx.cs* souboru pro cyklický průchod položky nákupního košíku a volání `RemoveItem` a `UpdateItem` metody.

1. V **Průzkumníka řešení**, otevřete *ShoppingCart.aspx.cs* soubor v kořenové složce projektu webové aplikace.
2. Následující oddíly kódu zvýrazněné žlutou barvou, které chcete přidat *ShoppingCart.aspx.cs* souboru:   

    [!code-csharp[Main](shopping-cart/samples/sample11.cs?highlight=9-11,33,44-89)]

Pokud uživatel klikne **aktualizace** tlačítko *ShoppingCart.aspx* stránky, je volána metoda UpdateCartItems. Metoda UpdateCartItems získá aktualizovanými hodnotami pro každé položky v nákupním košíku. Potom volá metodu UpdateCartItems `UpdateShoppingCartDatabase` – metoda (přidání a je vysvětleno v dalším kroku) Chcete-li přidat nebo odebrat položky z nákupního košíku. Po databázi byl aktualizován tak, aby odrážely aktualizace do nákupního košíku **GridView** ovládací prvek se aktualizuje na stránce nákupní košík voláním `DataBind` metodu **GridView**. Celková částka objednávky na stránce nákupní košík je také aktualizovat tak, aby odrážely aktualizovaný seznam položek.

### <a name="updating-and-removing-shopping-cart-items"></a>Aktualizace a odebrání položky v nákupním košíku

Na *ShoppingCart.aspx* stránce se zobrazí ovládací prvky byly přidány pro aktualizaci množství položku nebo odebráním položky. Teď přidejte kód, který provede tyto ovládací prvky fungují.

1. V **Průzkumníku řešení**, otevřete *ShoppingCartActions.cs* soubor *logiky* složky.
2. Přidejte následující kód zvýrazněné žlutou barvou na *ShoppingCartActions.cs* soubor třídy:   

    [!code-csharp[Main](shopping-cart/samples/sample12.cs?highlight=99-213)]

`UpdateShoppingCartDatabase` Metoda volána z `UpdateCartItems` metodu *ShoppingCart.aspx.cs* stránce, obsahuje logiku pro aktualizaci nebo odebrání položek z nákupního košíku. `UpdateShoppingCartDatabase` Metoda Iteruje přes všechny řádky v rámci seznamu nákupního košíku. Pokud byl označen položku nákupního košíku odebrat, nebo je množství menší než jedna `RemoveItem` metoda je volána. V opačném případě položce nákupního košíku se kontroluje u aktualizuje, když `UpdateItem` metoda je volána. Po odebrání nákupního košíku položku nebo aktualizovat, databáze změny se uložily.

`ShoppingCartUpdates` Struktura se používá pro uložení všech nákupního košíku položek. `UpdateShoppingCartDatabase` Metoda používá `ShoppingCartUpdates` struktura určit, pokud některá z položek muset aktualizovat nebo odebrat.

V dalším kurzu, budete používat `EmptyCart` metoda zrušte nákupního košíku po zakoupení produkty. Ale prozatím budete používat `GetCount` metodu, která jste právě přidali *ShoppingCartActions.cs* soubor k určení, kolik položek jsou v nákupním košíku.

### <a name="adding-a-shopping-cart-counter"></a>Přidání nákupního košíku čítače

Chcete-li povolit uživatelům zobrazit celkový počet položek v nákupním košíku, přidáte čítače, který chcete *Site.Master* stránky. Tento čítač se chovat i jako odkaz do nákupního košíku.

1. V **Průzkumníka řešení**, otevřete *Site.Master* stránky.
2. Upravte kód tak, že přidáte odkaz čítač nákupního košíku, jak je znázorněno v žlutá do části navigace, zobrazí se takto:  

    [!code-html[Main](shopping-cart/samples/sample13.html?highlight=6)]
3. V dalším kroku aktualizace kódu ze *Site.Master.cs* soubor přidáním kódu zvýrazněné žlutou barvou následujícím způsobem:  

    [!code-csharp[Main](shopping-cart/samples/sample14.cs?highlight=11,77-84)]

Před vykreslením stránky ve formátu HTML, `Page_PreRender` událost se vyvolá. V `Page_PreRender` obslužnou rutinu, celkový počet nákupní košík je určit pomocí volání `GetCount` metody. Vrácená hodnota je přidána do `cartCount` rozpětí součástí z kódu *Site.Master* stránky. `<span>` Značky umožňuje vnitřní prvky, které mají být vykresleny správně. Když se zobrazí všechny stránky webu, celkem nákupního košíku se zobrazí. Uživatele můžete také kliknout na nákupního košíku celkové zobrazení nákupního košíku.

## <a name="testing-the-completed-shopping-cart"></a>Testování dokončené nákupního košíku

Můžete spustit nyní aplikaci chcete zobrazit, jak můžete přidat, odstranit a aktualizovat položky v nákupním košíku. Nákupní košík celkem bude odrážet celkové náklady na všechny položky v nákupním košíku.

1. Stisknutím klávesy **F5** ke spuštění aplikace.  
 V prohlížeči se otevře a zobrazí *Default.aspx* stránky.
2. Vyberte **auta** navigační nabídce kategorie.
3. Klikněte na tlačítko **přidat do košíku** odkaz vedle první produktu.   
 *ShoppingCart.aspx* zobrazí se stránka s ordinálního součtu.
4. Vyberte **rovin** navigační nabídce kategorie.
5. Klikněte na tlačítko **přidat do košíku** odkaz vedle první produktu.
6. Nastavte počet první položky v nákupním košíku na 3 a vyberte **odebrat položku** zaškrtávací políčko druhé položky.<a id="a"></a>
7. Klikněte na tlačítko **aktualizovat** tlačítka Aktualizovat na stránce nákupní košík a zobrazovat nové ordinálního součtu. 

    ![Nákupní košík – aktualizace nákupního košíku](shopping-cart/_static/image9.png)

## <a name="summary"></a>Souhrn

V tomto kurzu jste vytvořili pro ukázkovou aplikaci Wingtip Toys webových formulářů nákupního košíku. Během tohoto kurzu používáte Entity Framework Code First, anotacemi dat, ovládací prvky dat silného typu a vazby modelu.

Nákupní košík podporuje přidávání, odstraňování a aktualizace položek, které uživatel vybral pro nákup. Kromě provádění funkci nákupního košíku, jste se naučili, jak zobrazení nákupního košíku položek v **GridView** řídit a výpočet ordinálního součtu.

## <a name="addition-information"></a>Další informace

[Přehled stavu relace ASP.NET](https://msdn.microsoft.com/library/ms178581.aspx)

> [!div class="step-by-step"]
> [Předchozí](display_data_items_and_details.md)
> [další](checkout-and-payment-with-paypal.md)
