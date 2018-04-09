---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
title: Nákupní košík | Microsoft Docs
author: Erikre
description: Tento kurz řady naučit se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a Microsoft Visual Studio Express 2013 pro jsme...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 6898c601-6c31-432f-8388-e6843f8a17cb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
msc.type: authoredcontent
ms.openlocfilehash: a8e96da7737cdf649575711a464c4f7726cb6ded
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="shopping-cart"></a>Nákupní košík
====================
Podle [Erik Reitan](https://github.com/Erikre)

[Stáhnout adresář Wingtip Toys ukázkového projektu (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) nebo [stáhnout elektronická kniha (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Tento kurz řady naučit se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a Microsoft Visual Studio Express 2013 pro Web. Visual Studio 2013 [projekt pomocí zdrojového kódu C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) dispozici je pro tento kurz řady.


Tento kurz popisuje obchodní logiky potřebné k přidání nákupní košík na adresář Wingtip Toys ukázkovou aplikaci webových formulářů ASP.NET. V tomto kurzu vychází předchozí kurzu "Zobrazení dat položky a podrobnosti" a je součástí řady kurz Wingtip hračka úložiště. Po dokončení tohoto kurzu, nebudou uživatelé ukázkové aplikace k přidání, odebrání a změna produkty v jejich nákupní košík.

## <a name="what-youll-learn"></a>Získáte informace:

1. Jak vytvořit nákupní košík pro webovou aplikaci.
2. Postup povolení uživatelům k přidávání položek do nákupního košíku.
3. Postup přidání [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview(v=vs.110).aspx#introduction) řízení zobrazit podrobnosti o nákupní košík.
4. Postup výpočtu a zobrazení celkové pořadí.
5. Postup odstranění a aktualizace položek v nákupní košík.
6. Jak se zahrnuje nákupní košík čítače.

## <a name="code-features-in-this-tutorial"></a>Funkce kód v tomto kurzu:

1. Rozhraní Entity Framework Code First
2. Datových poznámek
3. Ovládací prvky datových silného typu
4. Vazby modelu

## <a name="creating-a-shopping-cart"></a>Vytváření nákupní košík

Dříve v této série kurzu jste přidali stránky a kód pro zobrazení dat produktu z databáze. V tomto kurzu vytvoříte nákupní košík ke správě produktů, že jsou uživatelé zájem o nákupu. Uživatelé můžou Procházet a přidání položek do nákupního košíku, i když nejsou registrované nebo přihlášení. Ke správě přístupu nákupní košík, přiřadíte uživatelům jedinečný `ID` pomocí globálně jedinečný identifikátor (GUID), když uživatel získá přístup k nákupního košíku poprvé. Budete uložit tento `ID` pomocí stavu relace ASP.NET.

> [!NOTE] 
> 
> Stavu relace ASP.NET je vhodné místo k uložení informace specifické pro uživatele, který vyprší po uživatel odejde webu. Při zneužití stav relace může mít vliv na výkon na větší lokalit, light použití relace stavu funguje dobře pro demonstrační účely. Ukázkový projekt adresář Wingtip Toys ukazuje, jak se používání stavu relace bez externího poskytovatele, kde je stav relace uložené v procesu na webovém serveru hostícím webu. Pro větší weby, které poskytují více instancí aplikace nebo lokalit, které používají více instancí aplikace na různé servery, zvažte použití **služba systému Windows Azure mezipaměti**. Tato služba mezipaměti poskytuje Distribuovaná služba ukládání do mezipaměti, která je externí k webu a řeší problém použití stavu relace v procesu. Další informace najdete v tématu [postup používání stavu relace ASP.NET s weby systému Windows Azure](https://docs.microsoft.com/azure/redis-cache/cache-aspnet-session-state-provider).


### <a name="add-cartitem-as-a-model-class"></a>Přidat CartItem jako třídu modelu

Dříve v této série kurz definované schéma pro data kategorie a produktu vytvořením `Category` a `Product` třídy v *modely* složky. Nyní přidejte novou třídu definovat schéma pro nákupní košík. Později v tomto kurzu budete přidávat třídy pro zpracování přístupu k datům `CartItem` tabulky. Tato třída bude poskytovat obchodní logiky k přidání, odebrání a aktualizovat položky v nákupní košík.

1. Klikněte pravým tlačítkem myši *modely* složky a vyberte **přidat**  - &gt; **novou položku**. 

    ![Nákupní košík – nová položka](shopping-cart/_static/image1.png)
2. **Přidat novou položku** se zobrazí dialogové okno. Vyberte **kód**a potom vyberte **třída**. 

    ![Nákupní košík - přidat novou položku – dialogové okno](shopping-cart/_static/image2.png)
3. Název tato nová třída *CartItem.cs*.
4. Klikněte na tlačítko **přidat**.  
   Nový soubor třídy se zobrazí v editoru.
5. Ve výchozím kódu nahraďte následujícím kódem:   

    [!code-csharp[Main](shopping-cart/samples/sample1.cs)]

`CartItem` Třída obsahuje schéma, které definují jednotlivé produkty uživatel přidá do nákupního košíku. Tato třída je podobná jiné třídy schématu, které jste vytvořili dříve v této série kurzu. Podle konvence Entity Framework Code First očekává, primární klíč pro `CartItem` tabulka bude buď `CartItemId` nebo `ID`. Ale kód přepisuje výchozí chování pomocí datové poznámky `[Key]` atribut. `Key` Atribut ItemId vlastnost určuje, že `ItemID` vlastnost je primární klíč.

`CartId` Určuje vlastnost `ID` uživatele, který je přidružená k položce k nákupu. Přidáte kód k vytvoření tohoto uživatele `ID` když uživatel přistupuje k nákupní košík. To `ID` také se uloží jako na proměnnou relace ASP.NET.

### <a name="update-the-product-context"></a>Aktualizace produktu kontextu

Kromě přidání `CartItem` třída, budete muset aktualizovat třídy kontextu databáze, která spravuje tříd entit, který poskytuje přístup k datům do databáze. K tomu budete přidávat nově vytvořený `CartItem` třída na modelu `ProductContext` třídy.

1. V **Průzkumníku řešení**, najít a otevřít *ProductContext.cs* v soubor *modely* složky.
2. Přidejte zvýrazněný kód, který *ProductContext.cs* následujícím způsobem:  

    [!code-csharp[Main](shopping-cart/samples/sample2.cs?highlight=14)]

Jak je uvedeno nahoře v kurzu této série, kód v *ProductContext.cs* přidá soubor `System.Data.Entity` obor názvů tak, aby měli přístup k základním funkcím Entity Framework. Tato funkce zahrnuje schopnost dotazovat, vložit, aktualizovat a odstranit data ve spolupráci s objektů se silným typem. `ProductContext` Třída přidává přístup k nově přidaný `CartItem` třída modelu.

### <a name="managing-the-shopping-cart-business-logic"></a>Správa nákupní košík obchodní logiky

V dalším kroku vytvoříte `ShoppingCart` – třída v nové *logiku* složky. `ShoppingCart` Třída zpracovává přístupu k datům `CartItem` tabulky. Třída bude také obsahovat obchodní logiku pro přidání, odebrání a aktualizovat položky v nákupní košík.

Nákupní košík logiky, která přidáte bude obsahovat funkci ke správě následující akce:

1. Přidávání položek do nákupního košíku
2. Odebírání položek z nákupní košík
3. Získání ID nákupní košík
4. Načítání položek ze nákupní košík
5. Součtem množství všechny nákupní košík položky
6. Aktualizace dat nákupní košík

Na stránce nákupní košík (*ShoppingCart.aspx*) a třídu nákupního košíku se bude používat pro přístup k datům nákupní košík společně. Nákupní košík stránce se zobrazí všechny položky, které uživatel přidá do nákupního košíku. Kromě nákupního košíku stránky a třídy, vytvoříte stránky (*AddToCart.aspx*) přidejte produkty nákupní košík. Přidejte také kód pro *ProductList.aspx* stránky a *ProductDetails.aspx* stránky, která bude obsahovat odkaz na *AddToCart.aspx* stránky, takže uživatel může přidávat produkty do nákupního košíku.

Následující diagram znázorňuje základní proces, který nastane, když uživatel přidá do nákupního košíku produkt.

![Nákupní košík – přidání do nákupní košík](shopping-cart/_static/image3.png)

Když uživatel klikne **přidat do košíku** odkaz na buď *ProductList.aspx* stránky nebo *ProductDetails.aspx* stránky, aplikace bude přejděte do *AddToCart.aspx* stránky a potom automaticky na *ShoppingCart.aspx* stránky. *AddToCart.aspx* stránky voláním metody ve třídě ShoppingCart přidá vyberte produkt do nákupního košíku. *ShoppingCart.aspx* stránky se zobrazí produkty, které byly přidány do nákupního košíku.

#### <a name="creating-the-shopping-cart-class"></a>Vytvoření třídy nákupní košík

`ShoppingCart` Třída přidána do samostatné složky v aplikaci tak, že bude jasně odlišit modelu (složku modely), na stránkách (do kořenové složky) a logiku (logiku složky).

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **Northwind**projektu a vyberte **přidat**-&gt;**novou složku**. Název nové složky *logiku*.
2. Klikněte pravým tlačítkem myši *logiku* složku a potom vyberte **přidat**  - &gt; **novou položku**.
3. Přidat nový soubor třídy s názvem *ShoppingCartActions.cs*.
4. Ve výchozím kódu nahraďte následujícím kódem:   

    [!code-csharp[Main](shopping-cart/samples/sample3.cs)]

`AddToCart` Metoda umožňuje jednotlivé produkty, které mají být zahrnuty do nákupního košíku na základě produktu `ID`. Produkt je přidat do košíku nebo pokud košíku již obsahuje položku pro daný produkt, se zvyšuje množství.

`GetCartId` Metoda vrátí košíku `ID` pro uživatele. Košíku `ID` slouží ke sledování položky, které má uživatel v jejich nákupní košík. Pokud uživatel nemá stávající košíku `ID`, nové košíku `ID` je pro ně byly vytvořeny. Pokud je uživatel přihlášený jako registrovaný uživatel košíku `ID` nastavena na své uživatelské jméno. Ale pokud uživatel není přihlášený v košíku `ID` je nastavená na hodnotu jedinečný (GUID). Identifikátor GUID zajišťuje, že pouze jeden košíku se vytvoří pro každého uživatele, založené na relaci.

`GetCartItems` Metoda vrátí seznam hodnot nákupního košíku položky pro uživatele. Později v tomto kurzu, uvidíte, že vazby modelu se používá k zobrazení košíku položek do nákupního košíku pomocí `GetCartItems` metoda.

### <a name="creating-the-add-to-cart-functionality"></a>Vytváření funkci přidat do košíku

Jak už bylo zmíněno dříve, vytvoříte zpracování stránky s názvem *AddToCart.aspx* který se použije pro přidání nové produkty do nákupního košíku uživatele. Tato stránka vám zavoláme `AddToCart` metoda v `ShoppingCart` třídy, kterou jste právě vytvořili. *AddToCart.aspx* stránky se očekávat, který produkt `ID` je do ní předán. Tento produkt `ID` se použije při volání metody `AddToCart` metoda v `ShoppingCart` třídy.

> [!NOTE] 
> 
> Chystáte se změnit modelu code-behind (*AddToCart.aspx.cs*) pro tuto stránku není stránka uživatelského rozhraní (*AddToCart.aspx*).


#### <a name="to-create-the-add-to-cart-functionality"></a>Chcete-li vytvořit přidat do košíku funkce:

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **Northwind**projektu, klikněte na tlačítko **přidat**  - &gt; **novou položku**.  
   **Přidat novou položku** se zobrazí dialogové okno.
2. Přidat standardní novou stránku (webového formuláře) pro aplikaci s názvem *AddToCart.aspx*. 

    ![Nákupní košík – přidání webového formuláře](shopping-cart/_static/image4.png)
3. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *AddToCart.aspx* a pak klikněte na tlačítko **kód zobrazení**. *AddToCart.aspx.cs* otevření souboru kódu v editoru.
4. Nahraďte stávající kód v *AddToCart.aspx.cs* kódu následujícím kódem:   

    [!code-csharp[Main](shopping-cart/samples/sample4.cs)]

Když *AddToCart.aspx* načtení stránky produktu `ID` se načítají z řetězce dotazu. V dalším kroku instance třídy nákupního košíku se vytvoří a používá k volání `AddToCart` metoda, kterou jste přidali dříve v tomto kurzu. `AddToCart` Metody, které jsou součástí *ShoppingCartActions.cs* souboru, obsahuje logiku pro přidání vybrané produktu do nákupního košíku nebo zvýšit množství produktu vybrané produktu. Pokud produkt nebyl přidán do nákupního košíku, je produkt přidán do `CartItem` tabulky databáze. Pokud uživatel přidá další položku stejného produktu produktu již byla přidána do nákupního košíku, objemu produktu se zvýší v `CartItem` tabulky. Nakonec stránce přesměruje zpět *ShoppingCart.aspx* stránky, která přidáte v dalším kroku, kde uživateli se zobrazí aktualizovaný seznam položky v košíku.

Jak už jsme zmínili, uživatel `ID` slouží k identifikaci produkty, které jsou spojeny s konkrétním uživatelem. To `ID` se přidá řádek v `CartItem` tabulky pokaždé, když uživatel přidá do nákupního košíku produkt.

### <a name="creating-the-shopping-cart-ui"></a>Vytváření nákupní košík uživatelského rozhraní

*ShoppingCart.aspx* stránky se zobrazí produkty, které uživatel přidal do jejich nákupní košík. Také zajistí možnost přidat, odebrat a aktualizaci položky v nákupní košík.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **Northwind**, klikněte na tlačítko **přidat**  - &gt; **novou položku**.  
   **Přidat novou položku** se zobrazí dialogové okno.
2. Přidat novou stránku (webového formuláře) zahrnující hlavní stránky tak, že vyberete **webového formuláře pomocí stránky předlohy**. Zadejte název nové stránky *ShoppingCart.aspx*.
3. Vyberte **Site.Master** připojit stránky předlohy pro nově vytvořený *.aspx* stránky.
4. V *ShoppingCart.aspx* stránky, nahraďte existující kód následující kód:   

    [!code-aspx[Main](shopping-cart/samples/sample5.aspx)]

*ShoppingCart.aspx* stránka obsahuje **GridView** ovládací prvek s názvem `CartList`. Tento ovládací prvek pro vazbu nákupní košík data z databáze, která se používá vazby modelu **GridView** ovládacího prvku. Když nastavíte `ItemType` vlastnost **GridView** řídit, vazby dat výraz `Item` je k dispozici v kódu ovládacího prvku a řízení se stane silného typu. Jak je uvedeno výše v této série kurz, můžete vybrat podrobnosti `Item` pomocí IntelliSense. Pokud chcete konfigurovat ovládací prvek dat pomocí vazby modelu vyberte data, můžete nastavit `SelectMethod` vlastností ovládacího prvku. Ve výše uvedené značky, nastavíte `SelectMethod` lze pomocí této metody GetShoppingCartItems, který vrátí seznam hodnot `CartItem` objekty. **GridView** ovládací prvek pro datové volá metodu v příslušnou dobu v životním cyklu stránky a automaticky vytvoří vazbu vrácená data. `GetShoppingCartItems` Metoda musí být přidán.

#### <a name="retrieving-the-shopping-cart-items"></a>Načítání položek nákupní košík

Dále přidáte kód, který *ShoppingCart.aspx.cs* kódu k načtení a naplnit rozhraní nákupního košíku.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *ShoppingCart.aspx* a pak klikněte na tlačítko **kód zobrazení**. *ShoppingCart.aspx.cs* otevření souboru kódu v editoru.
2. Nahraďte stávající kód s následujícími službami:  

    [!code-csharp[Main](shopping-cart/samples/sample6.cs)]

Jak je uvedeno nahoře, `GridView` data řízení volání `GetShoppingCartItems` metoda v příslušnou dobu v stránky životní cyklus a automaticky váže vrácená data. `GetShoppingCartItems` Metoda vytvoří instanci `ShoppingCartActions` objektu. Potom kód používá tuto instanci k vrácení položky v košíku voláním `GetCartItems` metoda.

### <a name="adding-products-to-the-shopping-cart"></a>Přidání produktů do nákupní košík

Když buď *ProductList.aspx* nebo *ProductDetails.aspx* zobrazí se stránka, uživatel bude moct přidat produktu nákupního košíku pomocí odkazu. Po kliknutí na odkaz, aplikace přejde na stránku zpracování s názvem *AddToCart.aspx*. *AddToCart.aspx* stránky zavolá `AddToCart` metoda v `ShoppingCart` třídu, která jste přidali dříve v tomto kurzu.

Nyní přidáte **přidat do košíku** odkaz na obě *ProductList.aspx* stránky a *ProductDetails.aspx* stránky. Tento odkaz bude obsahovat produktu `ID` , je načtena z databáze.

1. V **Průzkumníku řešení**, najít a otevřít stránku s názvem *ProductList.aspx*.
2. Přidání značek zvýrazněných v žlutý k *ProductList.aspx* stránky tak, aby se celá stránka vypadat takto:  

    [!code-aspx[Main](shopping-cart/samples/sample7.aspx?highlight=50-54)]

### <a name="testing-the-shopping-cart"></a>Testování nákupní košík

Spuštění aplikace, abyste viděli, jak přidat produkty do nákupního košíku.

1. Stiskněte klávesu **F5** ke spuštění aplikace.  
 Po projekt znovu vytvoří databázi, bude v prohlížeči otevřít a zobrazit *Default.aspx* stránky.
2. Vyberte **aut** v navigační nabídce kategorie.  
 *ProductList.aspx* se zobrazí stránka zobrazuje jenom produkty, které jsou zahrnuty v kategorii "Aut". 

    ![Nákupní košík - automobilů](shopping-cart/_static/image5.png)
3. Klikněte **přidat do košíku** odkaz vedle první produktu uvedené (převést car).   
 *ShoppingCart.aspx* se zobrazí stránka zobrazující výběr v nákupního košíku. 

    ![Nákupní košík - košíku](shopping-cart/_static/image6.png)
4. Zobrazit další produkty výběrem **roviny** v navigační nabídce kategorie.
5. Klikněte **přidat do košíku** odkaz vedle první produktu uvedené.  
 *ShoppingCart.aspx* se zobrazí stránka s další položky.
6. Zavřete prohlížeč.

### <a name="calculating-and-displaying-the-order-total"></a>Výpočet a zobrazení celkové pořadí

Kromě přidání produktů do nákupního košíku, přidáte `GetTotal` metodu `ShoppingCart` třídy a zobrazit velikost celkové pořadí na stránce nákupní košík.

1. V **Průzkumníku řešení**, otevřete *ShoppingCartActions.cs* v soubor *logiku* složky.
2. Přidejte následující `GetTotal` metoda zvýrazněných v žlutý k `ShoppingCart` třídy, tak, aby třída vypadat takto:   

    [!code-csharp[Main](shopping-cart/samples/sample8.cs?highlight=85-97)]

Nejdřív `GetTotal` metoda získá ID nákupní košík pro daného uživatele. Potom metoda získá košíku celkový vynásobením cena produktu množství produktu pro každý produkt uvedený v košíku.

> [!NOTE] 
> 
> Výše uvedený kód používá typ s možnou hodnotou Null "`int?`". Typy s možnou hodnotou Null může představovat všechny hodnoty základní typ a také jako hodnotu null. Další informace najdete v tématu [pomocí typy s možnou hodnotou Null](https://msdn.microsoft.com/library/2cf62fcy(v=vs.110).aspx).


### <a name="modify-the-shopping-cart-display"></a>Upravit zobrazení nákupní košík

Dále budete upravovat kód pro *ShoppingCart.aspx* stránky k volání `GetTotal` metoda a zobrazení, které na celkový počet *ShoppingCart.aspx* stránka při načtení stránky.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *ShoppingCart.aspx* a vyberte **kód zobrazení**.
2. V *ShoppingCart.aspx.cs* souboru, aktualizovat `Page_Load` obslužná rutina přidáním následujícího kódu zvýrazněných v žlutý:   

    [!code-csharp[Main](shopping-cart/samples/sample9.cs?highlight=16-31)]

Když *ShoppingCart.aspx* stránka načte, načte nákupní košík objekt a potom načte nákupní košík celkem voláním `GetTotal` metodu `ShoppingCart` – třída. Pokud je prázdný nákupní košík, se zobrazí za tímto účelem zprávu.

### <a name="testing-the-shopping-cart-total"></a>Testování nákupní košík celkem

Spusťte aplikaci teď chcete zobrazit, jak nelze pouze přidat produkt do nákupního košíku, ale zobrazí celkový počet nákupní košík.

1. Stiskněte klávesu **F5** ke spuštění aplikace.  
 Prohlížeč a zobrazit *Default.aspx* stránky.
2. Vyberte **aut** v navigační nabídce kategorie.
3. Klikněte **přidat do košíku** odkaz vedle první produktu.   
 *ShoppingCart.aspx* se zobrazí stránka s celkem pořadí. 

    ![Nákupní košík - celkový počet košíku](shopping-cart/_static/image7.png)
4. Některé jiné produkty (například roviny) přidáte do košíku.
5. *ShoppingCart.aspx* se zobrazí stránka s aktualizovaný součet pro všechny produkty, které jste přidali. 

    ![Nákupní košík - více produktů](shopping-cart/_static/image8.png)
6. Zastavte spuštěné aplikaci ukončením okna prohlížeče.

### <a name="adding-update-and-checkout-buttons-to-the-shopping-cart"></a>Přidání tlačítka najdete v článku věnovaném a aktualizace do nákupní košík

Povolit uživatelům změnit nákupní košík, přidáte **aktualizace** tlačítko a **najdete v článku věnovaném** tlačítko na stránku nákupní košík. **Najdete v článku věnovaném** tlačítko nepoužívá až později z této série kurzu.

1. V **Průzkumníku řešení**, otevřete *ShoppingCart.aspx* stránky v kořenu projektu webové aplikace.
2. Chcete-li přidat **aktualizace** tlačítko a **najdete v článku věnovaném** tlačítko k *ShoppingCart.aspx* přidejte značku zvýrazněných v žlutý na existující značky, jak je znázorněno v Následující kód:   

    [!code-aspx[Main](shopping-cart/samples/sample10.aspx?highlight=36-45)]

Když uživatel klikne **aktualizace** tlačítko `UpdateBtn_Click` bude volána obslužná rutina události. Této obslužné rutiny události bude volat kód, který přidáte v dalším kroku.

Dále můžete aktualizovat kód součástí *ShoppingCart.aspx.cs* souboru můžete procházet košíku položky a volání `RemoveItem` a `UpdateItem` metody.

1. V **Průzkumníku řešení**, otevřete *ShoppingCart.aspx.cs* soubor v kořenu projektu webové aplikace.
2. Přidejte následující kód části zvýrazněných v žlutý k *ShoppingCart.aspx.cs* souboru:   

    [!code-csharp[Main](shopping-cart/samples/sample11.cs?highlight=9-11,33,44-89)]

Když uživatel klikne **aktualizace** tlačítko *ShoppingCart.aspx* stránky, je volána metoda UpdateCartItems. Metoda UpdateCartItems získá aktualizovanými hodnotami pro každou položku v nákupní košík. Pak zavolá metodu UpdateCartItems `UpdateShoppingCartDatabase` – metoda (Přidat a vysvětlené v dalším kroku) můžete přidat nebo odebrat položky z nákupního košíku. Jakmile databáze je aktualizovaná tak, aby odrážela aktualizace nákupní košík **GridView** řízení se aktualizuje na stránce nákupní košík voláním `DataBind` metodu pro **rutina GridView**. Velikost celkové pořadí na stránce nákupní košík je také aktualizovat tak, aby odrážela aktualizovaný seznam položek.

### <a name="updating-and-removing-shopping-cart-items"></a>Aktualizace a odstranění nákupního košíku položky

Na *ShoppingCart.aspx* stránce se zobrazí ovládací prvky byly přidány pro aktualizace množství položky a odebrání položky. Nyní přidáte kód, který bude tyto ovládací prvky fungovat.

1. V **Průzkumníku řešení**, otevřete *ShoppingCartActions.cs* v soubor *logiku* složky.
2. Přidejte následující kód zvýrazněných v žlutý k *ShoppingCartActions.cs* soubor třídy:   

    [!code-csharp[Main](shopping-cart/samples/sample12.cs?highlight=99-213)]

`UpdateShoppingCartDatabase` Metody volat z `UpdateCartItems` metodu *ShoppingCart.aspx.cs* stránky, obsahuje logiku pro aktualizace nebo odebrání položek z nákupního košíku. `UpdateShoppingCartDatabase` Metoda prochází všechny řádky v seznamu nákupní košík. Pokud položku nákupního košíku byla označena k odebrání nebo množství je nižší než jednou, `RemoveItem` metoda je volána. Jinak se kontroluje nákupní košík položky na aktualizace, když `UpdateItem` metoda je volána. Po položce nákupní košík byl odebrán nebo aktualizovat, ukládají se změny databáze.

`ShoppingCartUpdates` Struktura se používá k ukládání všech nákupní košík položky. `UpdateShoppingCartDatabase` Používá metoda `ShoppingCartUpdates` struktura k určení, pokud některou z položek muset aktualizovat nebo odebrat.

V dalším kurzu budete používat `EmptyCart` metoda zrušte nákupního košíku po zakoupení produkty. Ale prozatím se bude používat `GetCount` metoda, kterou jste právě přidali *ShoppingCartActions.cs* soubor k určení, kolik položek jsou v nákupní košík.

### <a name="adding-a-shopping-cart-counter"></a>Přidání nákupní košík čítače

Povolit uživatelům zobrazit celkový počet položek v nákupní košík, přidáte čítače k *Site.Master* stránky. Tento čítač se slouží také jako odkaz na nákupní košík.

1. V **Průzkumníku řešení**, otevřete *Site.Master* stránky.
2. Upravte kód tak, že přidáte odkaz čítač nákupní košík, jak je znázorněno v žlutý do části navigace, zobrazí se následující:  

    [!code-html[Main](shopping-cart/samples/sample13.html?highlight=6)]
3. Potom aktualizujte kódu z *Site.Master.cs* souboru tak, že přidáte kód zvýrazněných v žlutý následujícím způsobem:  

    [!code-csharp[Main](shopping-cart/samples/sample14.cs?highlight=11,77-84)]

Předtím, než ve formátu HTML, vykreslení stránky `Page_PreRender` událost se vyvolá. V `Page_PreRender` obslužnou rutinu, celkový počet nákupní košík je dáno volání `GetCount` metoda. Vrácená hodnota se přidá na `cartCount` rozpětí součástí z kódu *Site.Master* stránky. `<span>` Značky umožňuje vnitřní prvky, které mají být vykreslen správně. Při zobrazení stránky pro všechny lokality, se zobrazí celkový počet nákupní košík. Uživatele můžete také kliknout na nákupní košík celkem zobrazíte nákupní košík.

## <a name="testing-the-completed-shopping-cart"></a>Testování dokončené nákupní košík

Můžete spustit nyní aplikaci zobrazíte přidání, odstranění a aktualizace položek v nákupní košík. Celkový počet nákupního košíku se projeví celkové náklady na všechny položky v nákupní košík.

1. Stiskněte klávesu **F5** ke spuštění aplikace.  
 Otevře se prohlížeč a ukazuje *Default.aspx* stránky.
2. Vyberte **aut** v navigační nabídce kategorie.
3. Klikněte **přidat do košíku** odkaz vedle první produktu.   
 *ShoppingCart.aspx* se zobrazí stránka s celkem pořadí.
4. Vyberte **roviny** v navigační nabídce kategorie.
5. Klikněte **přidat do košíku** odkaz vedle první produktu.
6. Nastavit množství první položky v nákupní košík na 3 a vyberte **odebrat položky** políčko druhý položky.<a id="a"></a>
7. Klikněte **aktualizovat** tlačítko Aktualizovat stránku nákupní košík a zobrazí nový celkový pořadí. 

    ![Nákupní košík - košíku aktualizace](shopping-cart/_static/image9.png)

## <a name="summary"></a>Souhrn

V tomto kurzu jste vytvořili nákupní košík pro ukázkovou aplikaci Wingtip Toys webových formulářů. Při tomto kurzu jste už použili Entity Framework Code First, datových poznámek, ovládací prvky silného typu dat a vazby modelu.

Nákupní košík podporuje přidávání, odstraňování a aktualizaci položky, které uživatel vybral zakoupit. Kromě implementace funkci nákupní košík, jste se naučili nákupní košík položky v zobrazení **GridView** řízení a vypočítat celkové pořadí.

## <a name="addition-information"></a>Další informace

[Přehled stavu relace ASP.NET](https://msdn.microsoft.com/library/ms178581.aspx)

> [!div class="step-by-step"]
> [Předchozí](display_data_items_and_details.md)
> [další](checkout-and-payment-with-paypal.md)
