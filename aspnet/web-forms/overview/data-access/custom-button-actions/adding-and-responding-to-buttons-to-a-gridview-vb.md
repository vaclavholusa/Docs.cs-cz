---
uid: web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
title: "Přidání a reagovat na tlačítka na GridView (VB) | Microsoft Docs"
author: rick-anderson
description: "V tomto kurzu budeme zabývat postup přidání vlastních tlačítek do šablony i na pole GridView nebo DetailsView ovládacího prvku. Konkrétně jsme budete bui..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 06c6bbd2-4bdc-435b-87a3-df2c868f4baa
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
msc.type: authoredcontent
ms.openlocfilehash: 8a642a9a8e25d64028df0b5d8741da3008700652
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="adding-and-responding-to-buttons-to-a-gridview-vb"></a>Přidání a reagovat na tlačítka na GridView (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_28_VB.exe) nebo [stáhnout PDF](adding-and-responding-to-buttons-to-a-gridview-vb/_static/datatutorial28vb1.pdf)

> V tomto kurzu budeme zabývat postup přidání vlastních tlačítek do šablony i na pole GridView nebo DetailsView ovládacího prvku. Konkrétně jsme budete sestavení rozhraní, které má FormView, která umožňuje uživatelům procházet dodavatelů.


## <a name="introduction"></a>Úvod

Při přístup jen pro čtení k datům sestavy, zahrnují mnoho scénáře vytváření sestav je s není neobvyklé pro sestav, které zahrnují možnost provádět akce na základě dat, které zobrazuje. Obvykle to podílejí přidání ovládacího prvku tlačítko, LinkButton nebo ImageButton Web s každý záznam zobrazí v sestavě, která po kliknutí na způsobí, že zpětné volání a vyvolá některé kódu na straně serveru. Úpravy a odstraňování dat na základě záznamu podle je nejběžnější příklad. Ve skutečnosti, jak jsme viděli počínaje [Přehled vložení, aktualizace a odstranění dat](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) kurzu, úpravy a odstranění je běžné, že rutina GridView DetailsView a FormView ovládacích prvků může podporovat takové funkce bez nutnost napsat jediný řádek kódu.

Kromě toho upravovat a odstraňovat tlačítka, rutina GridView, DetailsView a FormView ovládací prvky můžete také zahrnovat tlačítka, LinkButtons nebo ImageButtons, po kliknutí na provádět některé vlastní logiky na straně serveru. V tomto kurzu budeme zabývat postup přidání vlastních tlačítek do šablony i na pole GridView nebo DetailsView ovládacího prvku. Konkrétně jsme budete sestavení rozhraní, které má FormView, která umožňuje uživatelům procházet dodavatelů. Pro danou jiného dodavatele FormView zobrazí informace o dodavateli společně s ovládacího prvku tlačítko Web, který kliknutí na označí všechny jejich přidružené produkty jako zastaví. Kromě toho GridView uvádí tyto produkty poskytnuté dodavatelem vybrané, se každý řádek obsahující zvýšit ceny a slevách cena tlačítka, která, pokud klikli, zvýšit nebo snížit produktu s `UnitPrice` 10 % (viz obrázek 1).


[![Rutina GridView i FormView obsahovat tlačítek, provedení vlastní akce](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image2.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image1.png)

**Obrázek 1**: obě FormView a GridView obsahovat tlačítka, provedení vlastní akce ([Kliknutím zobrazit obrázek v plné velikosti](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image3.png))


## <a name="step-1-adding-the-button-tutorial-web-pages"></a>Krok 1: Přidání tlačítka kurz webové stránky

Předtím, než se podíváme na tom, jak přidat vlastní tlačítka, umožní s nejprve pozorně vytváření stránek ASP.NET v našem webu projekt, který budeme potřebovat pro účely tohoto kurzu. Nejprve přidejte novou složku s názvem `CustomButtons`. Dál přidejte následující dvě stránky ASP.NET do této složky, a zkontrolujte, zda přidružit každou stránku s `Site.master` hlavní stránky:

- `Default.aspx`
- `CustomButtons.aspx`


![Přidání stránky ASP.NET pro vlastní kurzy související tlačítka](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image4.png)

**Obrázek 2**: Přidání stránky ASP.NET pro vlastní kurzy související tlačítka


V jiných složkách, jako `Default.aspx` v `CustomButtons` složky zobrazí seznam kurzů k v jeho části. Odvolat, který `SectionLevelTutorialListing.ascx` tuto funkci zajišťuje uživatelský ovládací prvek. Proto přidat tento uživatelský ovládací prvek pro `Default.aspx` přetažením z Průzkumníka řešení na stránku s zobrazení návrhu.


[![Přidání SectionLevelTutorialListing.ascx uživatelského ovládacího prvku do Default.aspx](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image6.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image5.png)

**Obrázek 3**: Přidat `SectionLevelTutorialListing.ascx` uživatelského ovládacího prvku na `Default.aspx` ([Kliknutím zobrazit obrázek v plné velikosti](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image7.png))


Nakonec přidejte stránky jako položky na `Web.sitemap` souboru. Konkrétně, přidejte následující kód po stránkování a řazení `<siteMapNode>`:


[!code-xml[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample1.xml)]

Po aktualizaci `Web.sitemap`, pozorně Zobrazit kurzy web prostřednictvím prohlížeče. V nabídce na levé straně teď obsahuje položky pro úpravy, vkládání a odstraňování kurzy.


![Mapy webu nyní obsahuje položku pro tento kurz vlastních tlačítek](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image8.png)

**Obrázek 4**: mapy webu nyní obsahuje položku pro tento kurz vlastních tlačítek


## <a name="step-2-adding-a-formview-that-lists-the-suppliers"></a>Krok 2: Přidání FormView, který obsahuje seznam dodavatelů

Umožní s Začínáme v tomto kurzu přidáním FormView, který obsahuje seznam dodavatelů. Popsané v úvodu tohoto FormView umožní uživateli procházení dodavatelů, zobrazuje tyto produkty, které poskytuje dodavatel v GridView. Kromě toho tato FormView bude obsahovat tlačítko, při kliknutí na, označí všechny produkty dodavatele s jako zastaveny. Před jsme si týkají přidávání tlačítko Vlastní k třídě FormView, umožní s nejprve jenom vytvářet FormView tak, aby obsahovalo informace dodavatele.

Začněte otevřením `CustomButtons.aspx` stránku `CustomButtons` složky. Přidat FormView na stránku přetažením z panelu nástrojů na Designer a sadu jeho `ID` vlastnost `Suppliers`. Ze inteligentní značky s FormView opt k vytvoření nové ObjectDataSource s názvem `SuppliersDataSource`.


[![Vytvořit nový ObjectDataSource s názvem SuppliersDataSource](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image10.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image9.png)

**Obrázek 5**: vytvoření nové ObjectDataSource s názvem `SuppliersDataSource` ([Kliknutím zobrazit obrázek v plné velikosti](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image11.png))


Konfigurace této nové ObjectDataSource tak, že odešle dotaz z `SuppliersBLL` třídu s `GetSuppliers()` – metoda (viz obrázek 6). Vzhledem k tomu, že tento FormView neposkytuje rozhraní pro aktualizaci dodavatele informace, vyberte možnost (žádná) z rozevíracího seznamu na kartě aktualizace.


[![Nakonfigurujte zdroj dat pro použití třídy SuppliersBLL s GetSuppliers() – metoda](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image13.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image12.png)

**Obrázek 6**: Konfigurace zdroje dat pro použití `SuppliersBLL` třídu s `GetSuppliers()` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image14.png))


Po dokončení konfigurace ObjectDataSource, Visual Studio vytvoří `InsertItemTemplate`, `EditItemTemplate`, a `ItemTemplate` pro FormView. Odeberte `InsertItemTemplate` a `EditItemTemplate` a upravovat `ItemTemplate` tak, aby zobrazil jenom dodavatele s společnosti jméno a telefonní číslo. Nakonec zapnutí podpory stránkování pro FormView zaškrtnutím políčka Povolit stránkování z jeho inteligentní značky (nebo nastavením jeho `AllowPaging` vlastnost `True`). Po provedení těchto změn značky s deklarativní stránky s by měl vypadat takto:


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample2.aspx)]

Obrázek 7 znázorňuje stránce CustomButtons.aspx při zobrazení prostřednictvím prohlížeče.


[![FormView uvádí NázevSpolečnosti a pole Phone z aktuálně vybraného dodavatele](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image16.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image15.png)

**Obrázek 7**: uvádí FormView `CompanyName` a `Phone` pole z aktuálně vybrané dodavatele ([Kliknutím zobrazit obrázek v plné velikosti](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image17.png))


## <a name="step-3-adding-a-gridview-that-lists-the-selected-supplier-s-products"></a>Krok 3: Přidání GridView, obsahující seznam produktů s vybrané dodavatele

Tlačítko ukončí všechny produkty jsme mohli přidat do šablony s FormView, umožní s nejprve přidat GridView pod FormView, které jsou uvedeny produkty, které poskytuje dodavatel vybrané. GridView dosáhnout, přidat na stránku, nastavte jeho `ID` vlastnost `SuppliersProducts`, a přidejte nový ObjectDataSource s názvem `SuppliersProductsDataSource`.


[![Vytvořit nový ObjectDataSource s názvem SuppliersProductsDataSource](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image19.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image18.png)

**Obrázek 8**: vytvoření nové ObjectDataSource s názvem `SuppliersProductsDataSource` ([Kliknutím zobrazit obrázek v plné velikosti](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image20.png))


Konfigurace této ObjectDataSource použití třídy ProductsBLL s `GetProductsBySupplierID(supplierID)` – metoda (viz obrázek 9). Když tato rutina GridView vám umožní produktu s cenu být upravena, won t používat integrované úpravy nebo odstranění funkce z GridView. Proto jsme lze nastavit rozevíracím seznamu (None) pro ObjectDataSource s karty UPDATE, INSERT a DELETE.


[![Nakonfigurujte zdroj dat pro použití třídy ProductsBLL s GetProductsBySupplierID(supplierID) – metoda](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image22.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image21.png)

**Obrázek 9**: Konfigurace zdroje dat pro použití `ProductsBLL` třídu s `GetProductsBySupplierID(supplierID)` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image23.png))


Vzhledem k tomu `GetProductsBySupplierID(supplierID)` metoda přijímá vstupní parametr, Průvodce ObjectDataSource nám výzvy pro zdroj této hodnoty parametru. Předávat `SupplierID` z FormView hodnoty, nastavte parametr zdrojového rozevíracího seznamu na ovládací prvek a rozevírací seznam ControlID na `Suppliers` (ID FormView vytvořili v kroku 2).


[![Označuje pole supplierID parametr musí pocházet z ovládacího prvku FormView dodavatelů](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image25.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image24.png)

**Obrázek 10**: označuje, že  *`supplierID`*  parametr musí pocházet z `Suppliers` FormView řízení ([Kliknutím zobrazit obrázek v plné velikosti](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image26.png))


Po dokončení Průvodce ObjectDataSource, bude GridView obsahovat BoundField nebo vlastnost CheckBoxField pro každý produkt s datová pole. Umožňují s to trim dolů k zobrazení jenom na `ProductName` a `UnitPrice` BoundFields spolu s `Discontinued` Vlastnost CheckBoxField; kromě toho mohli formátu s `UnitPrice` BoundField tak, aby jeho text je formátován jako měny. Vaše GridView a `SuppliersProductsDataSource` ObjectDataSource s deklarativní by měl vypadat podobně jako následující kód:


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample3.aspx)]

V tomto okamžiku zobrazí našem kurzu hlavní/podrobnosti sestavy, uživatel vybrala dodavatele z FormView v horní části a produkty od dodavatele prostřednictvím GridView v dolní části. Obrázek 11 zobrazuje snímek obrazovky části této stránky, když vyberete dodavatele Tokio Traders z FormView.


[![Vybrané dodavatele s produkty se zobrazují v prvku GridView.](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image28.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image27.png)

**Obrázek 11**: vybrané dodavatele s produkty jsou zobrazeny v GridView ([Kliknutím zobrazit obrázek v plné velikosti](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image29.png))


## <a name="step-4-creating-dal-and-bll-methods-to-discontinue-all-products-for-a-supplier"></a>Krok 4: Vytvoření DAL a metody BLL ukončí všechny produkty pro dodavatele

Před jsme přidání tlačítka do FormView, po kliknutí na ze všech produktů s jiného dodavatele, musíme nejprve přidejte metodu DAL i BLL, který provádí tuto akci. Konkrétně tato metoda bude mít název `DiscontinueAllProductsForSupplier(supplierID)`. Při kliknutí na s FormView tlačítko jsme budete volat tuto metodu v vrstvy obchodní logiky, předávání ve vybrané dodavatele s `SupplierID`; BLL pak zavolá odpovídající Data Access Layer metody, která bude vydávat `UPDATE` na – příkaz databáze, která ze zadaného dodavatele s produkty.

Jak je uvedeno v v našem předchozí kurzy, použijeme přístupu zdola nahoru, počínaje vytvoření metodu DAL, pak metodu BLL a nakonec implementace funkce na stránce technologie ASP.NET. Otevřete `Northwind.xsd` typové datové sady v `App_Code/DAL` složky a přidat nové metody pro `ProductsTableAdapter` (klikněte pravým tlačítkem na `ProductsTableAdapter` a zvolte Přidat dotazu). Tím zobrazíte Průvodce konfigurací dotazu TableAdapter, nám provede proces přidávání nové metody. Spustit tak, že zadáte, tato metoda naše DAL pomocí příkazu SQL ad hoc.


[![Create – metoda DAL příkazu SQL Ad-Hoc](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image31.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image30.png)

**Obrázek 12**: vytvoření DAL metoda pomocí příkazu SQL Ad-Hoc ([Kliknutím zobrazit obrázek v plné velikosti](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image32.png))


Dále požádáni o nám, jaký typ dotaz k vytvoření. Vzhledem k tomu, `DiscontinueAllProductsForSupplier(supplierID)` metoda bude nutné provést aktualizaci `Products` databázové tabulky, nastavení `Discontinued` pole 1 pro všechny produkty, které poskytuje zadaný  *`supplierID`* , musíme vytvořit dotaz, který aktualizuje data.


[![Vyberte typ dotazu aktualizace](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image34.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image33.png)

**Obrázek 13**: Vyberte typ dotazu aktualizace ([Kliknutím zobrazit obrázek v plné velikosti](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image35.png))


Na další obrazovce Průvodce poskytuje TableAdapter s existující `UPDATE` příkaz, který aktualizuje každé pole definovaná v `Products` DataTable. Text dotazu nahraďte následující příkaz:


[!code-sql[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample4.sql)]

Po zadání tohoto dotazu a kliknutím na tlačítko Další, poslední obrazovka průvodce požádá o nový název metody s pomocí `DiscontinueAllProductsForSupplier`. Dokončete průvodce kliknutím na tlačítko Dokončit. Při vrácení k návrháře DataSet byste měli vidět nové metody v `ProductsTableAdapter` s názvem `DiscontinueAllProductsForSupplier(@SupplierID)`.


[![Název nové DiscontinueAllProductsForSupplier DAL – metoda](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image37.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image36.png)

**Obrázek 14**: název nová metoda DAL `DiscontinueAllProductsForSupplier` ([Kliknutím zobrazit obrázek v plné velikosti](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image38.png))


Pomocí `DiscontinueAllProductsForSupplier(supplierID)` metoda vytvořené v Data Access Layer, naše dalším krokem je vytvoření `DiscontinueAllProductsForSupplier(supplierID)` metoda v vrstvu obchodní logiky. Chcete-li dosáhnout, otevřete `ProductsBLL` třídy souboru a přidejte následující:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample5.vb)]

Tato metoda jednoduše volá dolů na `DiscontinueAllProductsForSupplier(supplierID)` metoda v vrstvy DAL předávání podél poskytnutého  *`supplierID`*  hodnota parametru. Kdyby všechny obchodních pravidel, která je povoleno pouze dodavatele s produkty přerušeny za určitých okolností by měl být tato pravidla zde prováděn BLL.

> [!NOTE]
> Na rozdíl od `UpdateProduct` přetížení v `ProductsBLL` třídy, `DiscontinueAllProductsForSupplier(supplierID)` podpis metody nezahrnuje `DataObjectMethodAttribute` atribut (`<System.ComponentModel.DataObjectMethodAttribute(System.ComponentModel.DataObjectMethodType.Update, Boolean)>`). To vylučuje `DiscontinueAllProductsForSupplier(supplierID)` z rozevíracího seznamu ObjectDataSource s s Průvodce konfigurace zdroje dat na kartě aktualizace. I sunout vynechána tento atribut, protože jsme budete volání `DiscontinueAllProductsForSupplier(supplierID)` metoda přímo z obslužné rutiny události v naší stránce s ASP.NET.


## <a name="step-5-adding-a-discontinue-all-products-button-to-the-formview"></a>Krok 5: Přidání odebrání ze všech produktů tlačítko FormView

Pomocí `DiscontinueAllProductsForSupplier(supplierID)` dokončit metodu v BLL a DAL, v posledním kroku pro přidání možnost ukončí všechny produkty pro vybrané dodavatele, je přidání ovládacího prvku tlačítko pro FormView s `ItemTemplate`. Umožňují s přidat tlačítko níže telefonní číslo dodavatele s text tlačítka, ukončí všechny produkty a `ID` hodnota vlastnosti `DiscontinueAllProductsForSupplier`. Tento ovládací prvek tlačítko webu prostřednictvím návrháře můžete přidat kliknutím na odkaz Upravit šablony v FormView s inteligentním (viz obrázek 15), nebo přímo pomocí deklarativní syntaxe.


[![Přidání odebrání ze všech produktů webové ovládacího prvku tlačítka FormView s ItemTemplate](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image40.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image39.png)

**Obrázek 15**: Přidání ukončí všechny produkty tlačítko ovládacího prvku do FormView s `ItemTemplate` ([Kliknutím zobrazit obrázek v plné velikosti](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image41.png))


Když po kliknutí na tlačítko tak, že uživatel navštívíte, vyplývá stránce zpětné volání a FormView s [ `ItemCommand` událostí](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.itemcommand.aspx) aktivuje. Spuštění vlastního kódu v reakci na toto tlačítko se klikli, můžeme vytvořit obslužnou rutinu události pro tuto událost. Pochopení, ale který `ItemCommand` událost se aktivuje vždy, když *žádné* ve třídě FormView kliknutí na tlačítko, LinkButton nebo ImageButton webové ovládací prvek. To znamená, že když se uživatel přesune z jedné stránky na druhou v FormView, `ItemCommand` aktivuje událost; samé, když uživatel klikne na tlačítko Nový, upravit, nebo odstranit v FormView, který podporuje vkládání, aktualizaci nebo odstranění.

Vzhledem k tomu `ItemCommand` aktivuje se bez ohledu na to, jaké po kliknutí na tlačítko, v případě, že obslužná rutina budeme potřebovat způsob, jak určit, pokud bylo stisknuto tlačítko ukončí všechny produkty, nebo pokud se jednalo o některé další tlačítko. K tomu, jsme můžete nastavit ovládací prvek tlačítko webového s `CommandName` vlastnost některé identifikační hodnotu. Pokud po kliknutí na tlačítko, to `CommandName` do byla předána hodnota `ItemCommand` obslužné rutiny události, povolení ke zjištění, zda byla tlačítko ukončí všechny produkty kliknutí na tlačítko. Nastavit ukončí všechny produkty tlačítko s `CommandName` vlastnost DiscontinueProducts.

Nakonec umožní s pomůže zajistit, že opravdu chce uživatel ukončí vybrané dodavatele s produkty dialogového okna potvrdit na straně klienta. Jak jsme viděli v [přidání Client-Side potvrzení při odstranění](../editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb.md) kurz, můžete to provést pomocí verze jazyka JavaScript. Konkrétně nastavte vlastnost tlačítko webové ovládací prvek s OnClientClick na`return confirm('This will mark _all_ of this supplier\'s products as discontinued. Are you certain you want to do this?');`

Po provedení těchto změn, deklarativní syntaxi s FormView by měl vypadat následovně:


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample6.aspx)]

Dále vytvořte obslužnou rutinu události pro FormView s `ItemCommand` událostí. V této obslužné rutiny události je třeba nejprve určit, zda bylo stisknuto tlačítko ukončí všechny produkty. Pokud tedy chcete vytvořit instanci `ProductsBLL` třídy a vyvolání jeho `DiscontinueAllProductsForSupplier(supplierID)` předávání v případě metody `SupplierID` z vybrané FormView:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample7.vb)]

Všimněte si, že `SupplierID` aktuální vybrané dodavatele ve třídě FormView lze přistupovat pomocí FormView s [ `SelectedValue` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.selectedvalue.aspx). `SelectedValue` Vlastnost vrací hodnotu pro záznam se zobrazuje ve třídě FormView klíče první data. FormView s [ `DataKeyNames` vlastnost](https://msdn.microsoft.com/system.web.ui.webcontrols.formview.datakeynames.aspx), což naznačuje data pole, ze kterých data hodnoty klíče načítána, byla automaticky nastavena na `SupplierID` Visual Studio při vazbě ObjectDataSource na FormView zpět v kroku 2.

Pomocí `ItemCommand` vytvoření obslužné rutiny události za chvíli k otestování stránky. Přejděte do Cooperativa de Quesos, Las Cabras' dodavatele (ho s páté dodavatele v FormView pro mě nejlepší). Tato dodavatele poskytuje dva produkty, Queso Cabrales a Queso Manchego La Pastora, obě tyto položky jsou *není* zastaveny.

Představte si, že Cooperativa de Quesos, Las Cabras' byla mimo obchodní, a proto jeho produktů jsou přerušeny. Klikněte na tlačítko ukončí všechny produkty tlačítko. Zobrazí se dialogové okno potvrzení na straně klienta (viz obrázek 16).


[![Cooperativa de Quesos Las Cabras zdroje dva Active produkty](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image43.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image42.png)

**Obrázek 16**: Cooperativa de Quesos Las Cabras zdroje dva Active produkty ([Kliknutím zobrazit obrázek v plné velikosti](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image44.png))


Pokud kliknete na tlačítko OK v dialogovém okně potvrďte na straně klienta, bude pokračovat odeslání formuláře, v kterém příčinou zpětné volání FormView s `ItemCommand` událostí bude platit. Obslužné rutiny události jsme vytvořili pak provede, volání `DiscontinueAllProductsForSupplier(supplierID)` metoda a zrušený Queso Cabrales i Queso Manchego La Pastora produkty.

Pokud je zakázána stav zobrazení s GridView, GridView je právě odrážejí do základní úložiště dat v každé zpětné volání a proto bude okamžitě aktualizovat tak, aby odrážela, aby tyto dva produkty jsou nyní zastaveny (viz obrázek 17). Pokud však nejsou zakázané stav zobrazení v prvku GridView, musíte ručně rebind data, která mají GridView po provedení této změny. K tomu, jednoduše proveďte volání GridView s `DataBind()` metoda ihned po volání `DiscontinueAllProductsForSupplier(supplierID)` metoda.


[![Po kliknutí na tlačítko ukončí všechny produkty, jsou tyto produkty dodavatele s odpovídajícím způsobem aktualizovat](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image46.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image45.png)

**Obrázek 17**: Po kliknutí na tlačítko ukončí všechny produkty, jsou tyto produkty dodavatele s odpovídajícím způsobem aktualizovat ([Kliknutím zobrazit obrázek v plné velikosti](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image47.png))


## <a name="step-6-creating-an-updateproduct-overload-in-the-business-logic-layer-for-adjusting-a-product-s-price"></a>Krok 6: Vytvoření přetížení UpdateProduct v vrstvu obchodní logiky pro úpravu cenu s produktu

Jako ukončí všechny produkty tlačítkem v FormView, aby bylo možné přidat tlačítka pro zvýšení a snížení ceny pro produkt v GridView musíme nejprve přidejte příslušné metody Data Access Layer a vrstvu obchodní logiky. Vzhledem k tomu, že jsme už máte metodu, která aktualizuje jeden produkt řádek v DAL, můžeme poskytnout tyto funkce tak, že vytvoříte novou přetížení pro `UpdateProduct` metoda v BLL.

Naše minulosti `UpdateProduct` přetížení pořídili v některé kombinaci produktu polí jako skalární vstupní hodnoty a pak jste aktualizovali pouze pole pro daný produkt. Pro toto přetížení jsme budete mírně lišit od tento standard a místo toho předat v produktu s `ProductID` a procento, o které chcete upravit `UnitPrice` (oproti předávání v novém upravena `UnitPrice` sám sebe). Tento přístup zjednoduší kód potřebujeme se zapisovat do třídy kódu stránky ASP.NET, protože jsme nejsou zobrazeny t třeba zabývat určení aktuální produkt s `UnitPrice`.

`UpdateProduct` Přetížení pro tento kurz je zobrazena níže:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample8.vb)]

Toto přetížení načte informace o zadaný produkt prostřednictvím DAL s `GetProductByProductID(productID)` metoda. Následně kontroluje, zda produktu s `UnitPrice` je přiřazen databázi `NULL` hodnotu. Pokud se jedná, ceny, je ponechán beze změny. Pokud je však jinou hodnotu než`NULL` `UnitPrice` hodnotu, metoda aktualizace produktu s `UnitPrice` podle zadaného procenta (`unitPriceAdjustmentPercent`).

## <a name="step-7-adding-the-increase-and-decrease-buttons-to-the-gridview"></a>Krok 7: Přidání tlačítka snížení a zvýšení do GridView

Rutina GridView (a DetailsView) i vytvořené v kolekci polí. Kromě BoundFields, CheckBoxFields a TemplateFields technologie ASP.NET obsahuje ButtonField, který jak již název napovídá, vykreslí jako sloupec s tlačítko, LinkButton nebo ImageButton pro každý řádek. Podobně jako FormView, kliknutím na tlačítko *žádné* tlačítko GridView stránkování tlačítka, upravit nebo odstranit tlačítka, řazení tlačítka a tak dále způsobí, že zpětné volání a vyvolá rutina GridView s [ `RowCommand` událostí](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowcommand.aspx).

Má ButtonField `CommandName` vlastnost, která přiřadí zadaná hodnota pro každý z jeho tlačítka `CommandName` vlastnosti. S FormView, jako `CommandName` hodnota používá `RowCommand` obslužné rutiny události k určení, které tlačítko bylo kliknuto.

Umožňují s přidejte dva nové ButtonFields do GridView, jeden s text tlačítka cena + 10 % a další s textem cena -10 %. Přidat tyto ButtonFields, klikněte na odkaz Upravit sloupce z GridView s inteligentním, vyberte typ ButtonField pole ze seznamu v levém horním a klikněte na tlačítko Přidat.


![Přidejte dva ButtonFields do GridView](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image48.png)

**Obrázek 18**: Přidejte dva ButtonFields do GridView


Přesuňte dva ButtonFields, aby se zobrazily jako první dvě pole GridView. Dále nastavte `Text` vlastnosti těchto dvou ButtonFields k cena + 10 % a ceny -10 % a `CommandName` vlastností IncreasePrice a DecreasePrice, v uvedeném pořadí. Ve výchozím nastavení ButtonField vykreslí jeho sloupec tlačítek jako LinkButtons. To se dá změnit, ale prostřednictvím ButtonField s [ `ButtonType` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.buttonfieldbase.buttontype.aspx). Umožní s mají tyto dva ButtonFields se vykresluje jako regulární tlačítek; proto nastavit `ButtonType` vlastnost `Button`. Obrázek 19 zobrazí pole dialogové po tyto změny byly provedeny; rutina GridView s deklarativní následující, která je.


![Konfigurovat ButtonFields Text, CommandName a ButtonType vlastnosti](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image49.png)

**Obrázek 19**: Konfigurace ButtonFields `Text`, `CommandName`, a `ButtonType` vlastnosti


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample9.aspx)]

Pomocí těchto ButtonFields vytvořen, posledním krokem je vytvoření obslužné rutiny události pro GridView s `RowCommand` událostí. Této obslužné rutiny události, pokud je aktivována, protože buď za cenu + 10 % nebo % tlačítek cena -10 byly klikli, musí určit `ProductID` pro řádek, jehož tlačítko označeného a pak vyvolejte `ProductsBLL` třídu s `UpdateProduct` metody předávání do příslušné `UnitPrice` procento úpravu spolu s `ProductID`. Tyto úlohy provedou následující kód:

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample10.vb)]

Aby bylo možné zjistit `ProductID` pro řádek jejichž cena + 10 % nebo % tlačítko cena -10 označeného, musíme poraďte se GridView s `DataKeys` kolekce. Tato kolekce obsahuje hodnoty pole určená v `DataKeyNames` vlastnost pro každý řádek GridView. Protože rutina GridView s `DataKeyNames` vlastnost byla nastavena na ProductID Visual Studio, při vytváření vazby ObjectDataSource k GridView, `DataKeys(rowIndex).Value` poskytuje `ProductID` pro zadaný *rowIndex*.

ButtonField automaticky předá *rowIndex* řádku, jehož tlačítko označeného prostřednictvím `e.CommandArgument` parametr. Proto k určení `ProductID` pro řádek jejichž cena + 10 % nebo % tlačítko cena -10 označeného, používáme: `Convert.ToInt32(SuppliersProducts.DataKeys(Convert.ToInt32(e.CommandArgument)).Value)`.

Jako pomocí tlačítka ukončí všechny produkty, pokud je zakázána ke stavu zobrazení s GridView GridView je právě odrážejí do základní úložiště dat v každé zpětné volání a proto bude okamžitě aktualizovat tak, aby odrážely změny ceně, ke kterému dochází z kliknutím na buď tlačítek. Pokud však nejsou zakázané stav zobrazení v prvku GridView, musíte ručně rebind data, která mají GridView po provedení této změny. K tomu, jednoduše proveďte volání GridView s `DataBind()` metoda ihned po volání `UpdateProduct` metoda.

Obrázek 20 zobrazuje stránku při prohlížení produkty poskytované Grandma Jan věci odvál čas. Obrázek 21 zobrazuje výsledky po cena + 10 % bylo stisknuto tlačítko dvakrát pro Grandma's Boysenberry Spread a tlačítko cena -10 % jednou pro Northwoods Cranberry omáčka.


[![GridView zahrnuje cena + 10 % a ceny -10 % tlačítka](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image51.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image50.png)

**Obrázek 20**: cena zahrnuje GridView + 10 % a % tlačítka cena -10 ([Kliknutím zobrazit obrázek v plné velikosti](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image52.png))


[![Ceny pro produkt první a třetí byly aktualizovány prostřednictvím cena + 10 % a ceny -10 % tlačítka](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image54.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image53.png)

**Obrázek 21**: ceny pro první a třetí produktu byly aktualizovány prostřednictvím cena + 10 % a % tlačítka cena -10 ([Kliknutím zobrazit obrázek v plné velikosti](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image55.png))


> [!NOTE]
> Rutina GridView (a DetailsView) může mít tlačítka, LinkButtons nebo ImageButtons přidat do jejich TemplateFields. Protože tato tlačítka, po kliknutí na s BoundField, způsobí zpětné volání, vyvolání GridView s `RowCommand` událostí. Při přidání tlačítka v TemplateField, ale tlačítko s `CommandArgument` není automaticky nastavený na index řádku, jako je při použití ButtonFields. Pokud je potřeba určit index řádku tlačítka, které bylo kliknuto v rámci `RowCommand` obslužné rutiny události, musíte ručně nastavit tlačítko s `CommandArgument` vlastnost v jeho deklarativní syntaxi v rámci TemplateField, pomocí kódu jako:  
> `<asp:Button runat="server" ... CommandArgument='<%# CType(Container, GridViewRow).RowIndex %>' />`.


## <a name="summary"></a>Souhrn

Všechny prvky GridView, DetailsView a FormView může zahrnovat tlačítka, LinkButtons nebo ImageButtons. Tlačítka, po kliknutí na způsobit zpětné volání a zvýšit `ItemCommand` události v ovládacích prvcích FormView a DetailsView a `RowCommand` událost v GridView. Tyto ovládací prvky webového dat mít integrovanou funkci pro zpracování běžné akce související s příkaz, jako je odstranění nebo úprava záznamů. Ale můžeme také pomocí tlačítka, která při kliknutí na, odpoví provádění vlastní vlastní kód.

K tomu je potřeba vytvořit obslužnou rutinu události pro `ItemCommand` nebo `RowCommand` událostí. V této obslužné rutiny události jsme příchozí nejprve zkontrolujte `CommandName` hodnotu k určení, které tlačítko bylo kliknuto a proveďte příslušnou vlastní akci. V tomto kurzu jsme viděli postup používání tlačítka a ButtonFields odebrání ze všech produktů pro zadaný dodavatele nebo můžete zvýšit nebo snížit cenu konkrétní produkt 10 %.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Předchozí](adding-and-responding-to-buttons-to-a-gridview-cs.md)
