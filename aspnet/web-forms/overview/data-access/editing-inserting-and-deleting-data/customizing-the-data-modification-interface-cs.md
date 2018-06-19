---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs
title: Přizpůsobení rozhraní úpravu dat (C#) | Microsoft Docs
author: rick-anderson
description: V tomto kurzu budeme zabývat postup přizpůsobení rozhraní upravitelné GridView, nahrazením standardního textového pole a zaškrtávací políčko ovládací prvky s ternativní...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 22e99600-8d18-4a94-a20e-a3a62bb63798
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: f25b265c50870d59721a94c01d78f589a5d5f1c3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887626"
---
<a name="customizing-the-data-modification-interface-c"></a>Přizpůsobení rozhraní úpravu dat (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_20_CS.exe) nebo [stáhnout PDF](customizing-the-data-modification-interface-cs/_static/datatutorial20cs1.pdf)

> V tomto kurzu budeme zabývat postup přizpůsobení rozhraní upravitelné GridView, nahrazením standardního textového pole a zaškrtávací políčko řídí s alternativní vstupní ovládací prvky webového.


## <a name="introduction"></a>Úvod

BoundFields a CheckBoxFields používané ovládací prvky GridView a DetailsView zjednodušit proces upravovat data z důvodu jejich schopnost vykreslení jen pro čtení, upravovat a Vložitelný rozhraní. Tato rozhraní, které lze vykreslit bez nutnosti přidání další deklarativní kódu nebo kódu. Rozhraní BoundField a pro vlastnost CheckBoxField však nemají možnosti přizpůsobení často je potřeba v reálných scénářů. Chcete-li přizpůsobit rozhraní upravovat nebo Vložitelný v GridView nebo DetailsView musíme použijte TemplateField.

V [předchozí kurzu](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) jsme viděli, jak přizpůsobit rozhraní pro úpravu dat: můžete přidat ovládací prvky webového ověřování. V tomto kurzu budete podíváme na tom, jak přizpůsobit ovládacích prvků kolekce skutečná data, nahraďte BoundField a standardní textové pole na vlastnost CheckBoxField a ovládacích prvků CheckBox s alternativní vstupní ovládací prvky webového. Konkrétně jsme budete vytvářet upravitelné GridView, která umožňuje produktu název, kategorie, dodavatele a – starší formáty stav aktualizovat. Při úpravě konkrétního řádku, kategorie a dodavatele pole se vykreslují jako DropDownLists, obsahující sadu dostupných kategorií a všichni dodavatelé lze vybírat. Kromě toho jsme budete nahraďte třídě CheckBoxField výchozí políčko RadioButtonList ovládací prvek, který nabízí dvě možnosti: "Aktivní" a "Zastaví".


[![Úpravy rozhraní prvku GridView zahrnuje DropDownLists a přepínací tlačítka](customizing-the-data-modification-interface-cs/_static/image2.png)](customizing-the-data-modification-interface-cs/_static/image1.png)

**Obrázek 1**: úpravy DropDownLists zahrnuje rozhraní a přepínací tlačítka GridView ([Kliknutím zobrazit obrázek v plné velikosti](customizing-the-data-modification-interface-cs/_static/image3.png))


## <a name="step-1-creating-the-appropriateupdateproductoverload"></a>Krok 1: Vytvoření odpovídající`UpdateProduct`přetížení

V tomto kurzu vytvoříme upravitelné GridView, která umožňuje úpravy název produktu, kategorie, dodavatele a zastaví stav. Proto potřebujeme `UpdateProduct` přetížení, které přijímá pět vstupní parametry tyto hodnoty čtyři produktu a `ProductID`. Stejně jako v nástroji naše předchozí přetížení jeden budou:

1. Načtení informací o produktu z databáze pro zadaný `ProductID`,
2. Aktualizace `ProductName`, `CategoryID`, `SupplierID`, a `Discontinued` pole, a
3. Poslat žádost o aktualizaci DAL prostřednictvím TableAdapter `Update()` metoda.

Jako stručný výtah pro toto konkrétní přetížení I jste vynechání zkontrolujte obchodní pravidlo, zda zajišťuje, že produkt je označena jako zastaví není pouze produktů nabízených jeho dodavatele. Chování, můžete ho přidat, pokud dáváte přednost, nebo v ideálním případě refactor se logika pro samostatné metodě.

Následující kód ukazuje nový `UpdateProduct` přetížení v `ProductsBLL` třídy:


[!code-csharp[Main](customizing-the-data-modification-interface-cs/samples/sample1.cs)]

## <a name="step-2-crafting-the-editable-gridview"></a>Krok 2: Vytvoření upravitelné GridView

Pomocí `UpdateProduct` přetížení přidali, je vše připraveno k vytvoření naše upravitelné GridView. Otevřete `CustomizedUI.aspx` stránku `EditInsertDelete` složky a přidat ovládací prvek GridView do návrháře. Dál vytvořte novou ObjectDataSource z prvku GridView inteligentních značek. Konfigurace ObjectDataSource k načtení informací o produktu prostřednictvím `ProductBLL` třídy `GetProducts()` metoda a k aktualizaci dat pomocí produktu `UpdateProduct` přetížení jsme právě vytvořili. Z karty INSERT a DELETE vyberte z rozevíracího seznamu (žádný).


[![Konfigurace ObjectDataSource použít přetížení UpdateProduct právě vytvořili](customizing-the-data-modification-interface-cs/_static/image5.png)](customizing-the-data-modification-interface-cs/_static/image4.png)

**Obrázek 2**: Konfigurace ObjectDataSource pro použití `UpdateProduct` přetížení právě vytvořili ([Kliknutím zobrazit obrázek v plné velikosti](customizing-the-data-modification-interface-cs/_static/image6.png))


Jak jsme viděli v rámci kurzy změny dat, přiřadí deklarativní syntaxe ObjectDataSource vytvořené sadě Visual Studio `OldValuesParameterFormatString` vlastnost `original_{0}`. To samozřejmě, nebude fungovat se naše vrstvu obchodní logiky vzhledem k tomu, že naše metody neočekávané původní `ProductID` hodnota, která má být předán. Proto, jak jsme krok v předchozí kurzy, za chvíli toto přiřazení vlastnosti odebrání deklarativní syntaxi, nebo místo toho nastavte hodnotu této vlastnosti na `{0}`.

Po této změně ObjectDataSource deklarativní by měl vypadat následovně:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample2.aspx)]

Všimněte si, že `OldValuesParameterFormatString` vlastnost byla odebrána a zda je `Parameter` v `UpdateParameters` kolekce pro každý vstupní parametry očekávanou naše `UpdateProduct` přetížení.

Když je ObjectDataSource nakonfigurovány na aktualizaci pouze podmnožinu hodnoty produktu, GridView aktuálně ukazuje *všechny* polí produktu. Za chvíli GridView upravit tak, aby:

- Obsahují jenom `ProductName`, `SupplierName`, `CategoryName` BoundFields a `Discontinued` Vlastnost CheckBoxField
- `CategoryName` a `SupplierName` pole zařazena před (nalevo od) `Discontinued` Vlastnost CheckBoxField
- `CategoryName` a `SupplierName` BoundFields' `HeaderText` vlastnost je nastavena na "Kategorie" a "Dodavatel",
- Je povolena podpora úpravy (zaškrtnutím políčka Povolit úpravy v prvku GridView inteligentních značek)

Po provedení těchto změn bude návrháře vypadat podobně jako na obrázku 3 s prvku GridView deklarativní syntaxe uvedená níže.


[![Odebrání nepotřebných polí GridView](customizing-the-data-modification-interface-cs/_static/image8.png)](customizing-the-data-modification-interface-cs/_static/image7.png)

**Obrázek 3**: odebrání nepotřebných polí GridView ([Kliknutím zobrazit obrázek v plné velikosti](customizing-the-data-modification-interface-cs/_static/image9.png))


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample3.aspx)]

V tomto okamžiku je dokončena chování prvku GridView jen pro čtení. Při zobrazení dat, každý produkt je vykreslen jako řádek GridView, zobrazuje název produktu, kategorie, dodavatele a zastaví stavu.


[![Rozhraní prvku GridView jen pro čtení je dokončeno](customizing-the-data-modification-interface-cs/_static/image11.png)](customizing-the-data-modification-interface-cs/_static/image10.png)

**Obrázek 4**: rozhraní GridView jen pro čtení je dokončeno ([Kliknutím zobrazit obrázek v plné velikosti](customizing-the-data-modification-interface-cs/_static/image12.png))


> [!NOTE]
> Jak je popsáno v [kurzu přehled o vložení, aktualizace a odstranění dat](an-overview-of-inserting-updating-and-deleting-data-cs.md), je životně důležitá, že GridView stav zobrazení s být povolené (výchozí nastavení). Pokud jste nastavili GridView s `EnableViewState` vlastnost `false`, riskujete, že náhodně odstraněním nebo úpravou zaznamenává počet uživatelů. V tématu [upozornění: souběžnosti problém s ASP.NET 2.0 GridViews nebo DetailsView/FormViews tento podpora úpravy nebo odstranění a jejichž stav zobrazení je zakázána](http://scottonwriting.net/sowblog/posts/10054.aspx) Další informace.


## <a name="step-3-using-a-dropdownlist-for-the-category-and-supplier-editing-interfaces"></a>Krok 3: Použití rozevírací seznam pro danou kategorii a dodavatele úpravy rozhraní

Odvolat, který `ProductsRow` objekt obsahuje `CategoryID`, `CategoryName`, `SupplierID`, a `SupplierName` vlastnosti, které poskytují skutečnými hodnotami ID cizího klíče v `Products` databáze tabulky a odpovídající `Name` hodnoty v `Categories` a `Suppliers` tabulky. `ProductRow`Na `CategoryID` a `SupplierID` můžete i číst a zapisovat do při `CategoryName` a `SupplierName` vlastnosti jsou označeny jen pro čtení.

Z důvodu stavu jen pro čtení `CategoryName` a `SupplierName` vlastnosti, má odpovídající BoundFields jejich `ReadOnly` vlastnost nastavena na hodnotu `true`, brání upravována, když upravíte řádek tyto hodnoty. Když jsme můžete nastavit `ReadOnly` vlastnost `false`, vykreslení `CategoryName` a `SupplierName` BoundFields jako textová pole během úprav, tento postup bude mít za následek výjimku když se uživatel pokusí aktualizace produktu, protože žádné `UpdateProduct` přetížení při `CategoryName` a `SupplierName` vstupy. Ve skutečnosti jsme nechtějí vytvářet takové přetížení dvou důvodů:

- `Products` Tabulka neobsahuje `SupplierName` nebo `CategoryName` pole, ale `SupplierID` a `CategoryID`. Proto chceme, aby naše metoda předávané tyto konkrétní hodnoty ID není vyhledávací tabulky hodnoty.
- Vyžadování uživatele k zadání názvu dodavatele nebo kategorie je menší než ideální, protože se vyžaduje, aby uživatel vědět, k dispozici kategorií a Dodavatelé a jejich správné slova.

Pole dodavatele a kategorie by měla zobrazovat kategorii a dodavatelů názvy v režimu jen pro čtení (jako je tomu nyní) a rozevírací seznam příslušné možnosti Pokud upravována. Pomocí rozevíracího seznamu, koncový uživatel může rychle zjistit, jaké kategorií a všichni dodavatelé si můžete vybrat mezi a více snadno provádět jejich výběr.

Zajistit toto chování je potřeba převést `SupplierName` a `CategoryName` BoundFields do TemplateFields jejichž `ItemTemplate` vysílá `SupplierName` a `CategoryName` hodnoty a jehož `EditItemTemplate` používá ovládací prvek rozevírací seznam do seznamu kategorie k dispozici a všichni dodavatelé.

## <a name="adding-thecategoriesandsuppliersdropdownlists"></a>Přidávání`Categories`a`Suppliers`DropDownLists

Začněte tím, že převod `SupplierName` a `CategoryName` BoundFields do TemplateFields podle: Kliknutím na odkaz Upravit sloupce z prvku GridView inteligentních značek; výběrem ze seznamu v levém dolním; BoundField a kliknutím na "převést do tohoto pole Odkaz TemplateField". Proces převodu vytvoří TemplateField s oběma `ItemTemplate` a `EditItemTemplate`, jak je znázorněno níže uvedené deklarativní syntaxe:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample4.aspx)]

Vzhledem k tomu, že BoundField byla označena jako jen pro čtení, jak `ItemTemplate` a `EditItemTemplate` obsahovat webové popisek řídit, jehož `Text` vlastnost je vázána na příslušné datové pole (`CategoryName`, v syntaxi výše). Je potřeba upravit `EditItemTemplate`, nahraďte ovládacího prvku popisek webové ovládací prvek rozevírací seznam.

Jak jsme viděli v předchozí kurzy, můžete šablonu upravit pomocí návrháře nebo přímo z deklarativní syntaxi. Pokud ho Pokud chcete upravit pomocí návrháře, klikněte na odkaz Upravit šablony z prvku GridView inteligentních značek a rozhodli se pracovat s pole kategorie `EditItemTemplate`. Odebrání ovládacího prvku popisek webové a nahraďte ji metodou nastavení vlastnosti rozevírací seznam ID na ovládací prvek rozevírací seznam `Categories`.


[![Odeberte TexBox a přidejte do EditItemTemplate rozevírací seznam](customizing-the-data-modification-interface-cs/_static/image14.png)](customizing-the-data-modification-interface-cs/_static/image13.png)

**Obrázek 5**: Odeberte TexBox a přidejte rozevírací seznam k `EditItemTemplate` ([Kliknutím zobrazit obrázek v plné velikosti](customizing-the-data-modification-interface-cs/_static/image15.png))


Potřebujeme další naplnit rozevírací seznam s kategorie k dispozici. Klikněte na odkaz zvolit zdroj dat z inteligentních značek rozevírací seznam a zapojit k vytvoření nové ObjectDataSource s názvem `CategoriesDataSource`.


[![Vytvoření nového ovládacího prvku ObjectDataSource s názvem CategoriesDataSource](customizing-the-data-modification-interface-cs/_static/image17.png)](customizing-the-data-modification-interface-cs/_static/image16.png)

**Obrázek 6**: Vytvořte nový název ovládacího prvku ObjectDataSource `CategoriesDataSource` ([Kliknutím zobrazit obrázek v plné velikosti](customizing-the-data-modification-interface-cs/_static/image18.png))


Chcete-li tento ObjectDataSource vrátit všechny kategorie, vytvořte mu vazbu k `CategoriesBLL` třídy `GetCategories()` metoda.


[![Vytvořit vazbu ObjectDataSource CategoriesBLL GetCategories() – metoda](customizing-the-data-modification-interface-cs/_static/image20.png)](customizing-the-data-modification-interface-cs/_static/image19.png)

**Obrázek 7**: ObjectDataSource k vytvoření vazby `CategoriesBLL`na `GetCategories()` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](customizing-the-data-modification-interface-cs/_static/image21.png))


Nakonec, nakonfigurujte nastavení rozevírací seznam tak, aby `CategoryName` pole se zobrazí v každé rozevírací seznam `ListItem` s `CategoryID` pole použité jako hodnotu.


[![Zobrazí pole CategoryName a použít CategoryID jako hodnota](customizing-the-data-modification-interface-cs/_static/image23.png)](customizing-the-data-modification-interface-cs/_static/image22.png)

**Obrázek 8**: mít `CategoryName` zobrazí pole a `CategoryID` použít jako hodnotu ([Kliknutím zobrazit obrázek v plné velikosti](customizing-the-data-modification-interface-cs/_static/image24.png))


Po provedení těchto změn deklarativní `EditItemTemplate` v `CategoryName` TemplateField bude obsahovat rozevírací seznam a ObjectDataSource:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample5.aspx)]

> [!NOTE]
> Rozevírací seznam v `EditItemTemplate` musí mít svůj stav zobrazení, které jsou povolené. Brzy přidáme Syntaxe datové vazby k deklarativní syntaxe rozevírací seznam a příkazy vazby dat, jako `Eval()` a `Bind()` může vyskytovat pouze v ovládacích prvcích, jejichž stav zobrazení je povolen.


Opakujte tyto kroky pro přidání rozevírací seznam s názvem `Suppliers` k `SupplierName` na TemplateField `EditItemTemplate`. To bude zahrnovat rozevírací seznam pro přidání `EditItemTemplate` a vytváření jiný ObjectDataSource. `Suppliers` ObjectDataSource na rozevírací seznam, ale měla by se nakonfigurovat vyvolání `SuppliersBLL` třídy `GetSuppliers()` metoda. Navíc nakonfigurovat `Suppliers` rozevírací seznam pro zobrazení `CompanyName` pole a použít `SupplierID` pole jako hodnota jeho `ListItem` s.

Po přidání DropDownLists do dvou `EditItemTemplate` s, načtěte stránku v prohlížeči a klikněte na tlačítko Upravit Chef Anton Cajun Seasoning produktu. Jak je vidět na obrázku 9, kategorie a dodavatele sloupce produktu jsou vykresleny jako obsahující kategorie k dispozici a všichni dodavatelé zvolit z rozevírací seznamy. Všimněte si však, že *první* položek v obou rozevírací seznamy jsou vybrané ve výchozím nastavení (nápoje pro kategorii) a exotické kapaliny jako dodavatele, i když Chef Anton Cajun Seasoning přísady poskytl nový Orléans Cajun Delights.


[![Ve výchozím nastavení se vybere první položka v rozevírací seznamy](customizing-the-data-modification-interface-cs/_static/image26.png)](customizing-the-data-modification-interface-cs/_static/image25.png)

**Obrázek 9**: ve výchozím nastavení se vybere první položka v rozevírací seznamy ([Kliknutím zobrazit obrázek v plné velikosti](customizing-the-data-modification-interface-cs/_static/image27.png))


Kromě toho pokud kliknutím na tlačítko Aktualizovat budete zjistíte, že produktu `CategoryID` a `SupplierID` hodnoty jsou nastaveny na `NULL`. Obě tyto nežádoucí chování se nezdařila, protože DropDownLists v `EditItemTemplate` s nejsou vázány na všechna pole data ze zadaných dat produktu.

## <a name="binding-the-dropdownlists-to-thecategoryidandsupplieriddata-fields"></a>DropDownLists k vytvoření vazby`CategoryID`a`SupplierID`datových polí

Aby bylo možné používat kategorie a dodavatele upravená produktu rozevírací seznamy nastavit na odpovídající hodnoty a mít tyto hodnoty odeslány zpět BLL `UpdateProduct` metoda po kliknutí na tlačítko Aktualizovat, je potřeba vytvořit vazbu DropDownLists `SelectedValue` vlastnosti, které chcete `CategoryID` a `SupplierID` pomocí obousměrné vazby dat datová pole. K tomu se `Categories` rozevírací seznam, můžete přidat `SelectedValue='<%# Bind("CategoryID") %>'` přímo na deklarativní syntaxi.

Alternativně můžete nastavit rozevírací seznam datových vazeb úpravě šablony pomocí Designer a kliknutím na odkaz Upravit datové vazby ze inteligentní značky rozevírací seznam. V dalším kroku označuje, že `SelectedValue` vlastnost by měla být vázána na `CategoryID` pole pomocí obousměrné vazby dat (viz obrázek 10). Opakujte buď deklarativní návrháře procesu nebo k vytvoření vazby `SupplierID` data pole na `Suppliers` rozevírací seznam.


[![Vytvořit vazbu CategoryID vlastnost SelectedValue rozevírací seznam pomocí obousměrné vazby dat.](customizing-the-data-modification-interface-cs/_static/image29.png)](customizing-the-data-modification-interface-cs/_static/image28.png)

**Obrázek 10**: vytvoření vazby `CategoryID` k rozevírací seznam `SelectedValue` vlastnost pomocí obousměrné vazby dat ([Kliknutím zobrazit obrázek v plné velikosti](customizing-the-data-modification-interface-cs/_static/image30.png))


Jakmile se aplikovaly vazby `SelectedValue` vlastnosti dva DropDownLists, upravená produktové kategorie a dodavatele sloupce použije výchozí hodnoty pro aktuální produkt. Po kliknutí na tlačítko Aktualizovat, `CategoryID` a `SupplierID` hodnoty položky vybrané rozevíracím seznamu se předá `UpdateProduct` metoda. Obrázek 11 kurz ukazuje, po přidání datové vazby příkazy; Všimněte si, jak jsou položky vybrané rozevíracího seznamu pro Chef Anton Cajun Seasoning správně přísady a New Orléans Cajun Delights.


[![Ve výchozím nastavení jsou vybrané aktuální kategorii produktu upravit a hodnoty dodavatele](customizing-the-data-modification-interface-cs/_static/image32.png)](customizing-the-data-modification-interface-cs/_static/image31.png)

**Obrázek 11**: ve výchozím nastavení jsou vybrány upravit produkt aktuální kategorii a hodnoty dodavatele ([Kliknutím zobrazit obrázek v plné velikosti](customizing-the-data-modification-interface-cs/_static/image33.png))


## <a name="handlingnullvalues"></a>Zpracování`NULL`hodnoty

`CategoryID` a `SupplierID` sloupců v `Products` tabulka se dá `NULL`, ještě DropDownLists v `EditItemTemplate` s neobsahují položky seznamu představují `NULL` hodnotu. To má dva důsledky:

- Uživatel nemůže používat naše rozhraní změnit kategorii nebo dodavatele produktu z jinou hodnotu než`NULL` hodnotu `NULL` jeden
- Pokud má produkt `NULL` `CategoryID` nebo `SupplierID`, kliknutím na tlačítko Upravit bude mít za následek výjimku. Důvodem je, že `NULL` hodnoty vrácené `CategoryID` (nebo `SupplierID`) v `Bind()` příkaz nejsou namapované na hodnotu v rozevírací seznam (rozevírací seznam vyvolá výjimku při jeho `SelectedValue` je nastavena na hodnotu *není* v jeho kolekce položky seznamu).

Za účelem podpory `NULL` `CategoryID` a `SupplierID` hodnoty, je potřeba přidat další `ListItem` pro každý rozevírací seznam představují `NULL` hodnotu. V [a podrobností filtrování s rozevírací seznam](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) kurz, jsme viděli, jak přidat další `ListItem` k Vycentrovat rozevírací seznam, který podílejí nastavení rozevírací seznam `AppendDataBoundItems` vlastnost `true` a ručně přidejte další `ListItem`. V tomto kurzu předchozí však jsme přidali `ListItem` s `Value` z `-1`. Logika datové vazby v technologii ASP.NET, ale automaticky převede prázdný řetězec tak, aby `NULL` hodnota a naopak. Proto v tomto kurzu chceme `ListItem`na `Value` být prázdný řetězec.

Spuštění nastavením obou DropDownLists `AppendDataBoundItems` vlastnost `true`. Dál přidejte `NULL` `ListItem` přidáním následující `<asp:ListItem>` element pro každou rozevírací seznam tak, aby deklarativní vypadá jako:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample6.aspx)]

I jste se rozhodli použít "(None)" jako textové hodnoty pro tento `ListItem`, ale můžete ji také být prázdný řetězec, pokud chcete změnit.

> [!NOTE]
> Jak jsme viděli v *a podrobností filtrování s rozevírací seznam* kurzu `ListItem` s mohou být přidány do rozevírací seznam prostřednictvím návrháře kliknutím na rozevírací seznam `Items` vlastnost v okně vlastností (který se zobrazí `ListItem` Editor kolekce). Nicméně je nutné přidat `NULL` `ListItem` pro účely tohoto kurzu využijte deklarativní syntaxi. Pokud použijete `ListItem` Editor kolekce, bude vynechejte generovaného deklarativní syntaxi `Value` nastavení zcela při přiřazené prázdné řetězce, vytvoří deklarativní jako: `<asp:ListItem>(None)</asp:ListItem>`. Když to vypadat neškodné, chybějící hodnoty způsobí, že rozevírací seznam používat `Text` hodnotu vlastnosti na příslušné místo. To znamená, že pokud to `NULL` `ListItem` je vybrána, hodnota "(None)" se pokusí vytvořit přiřazení `CategoryID`, který bude mít za následek výjimku. Explicitně nastavením `Value=""`, `NULL` se přiřadí hodnota `CategoryID` při `NULL` `ListItem` je vybrána.


Opakujte tyto kroky pro rozevírací seznam dodavatelů.

Pomocí této další `ListItem`, můžete nyní přiřadit úpravy rozhraní `NULL` hodnoty pro určitý produkt `CategoryID` a `SupplierID` polí, jak ukazuje obrázek 12.


[![Zvolte (žádný) přiřadit hodnotu NULL pro o produktu, kategorie nebo dodavatele](customizing-the-data-modification-interface-cs/_static/image35.png)](customizing-the-data-modification-interface-cs/_static/image34.png)

**Obrázek 12**: Zvolte (žádný) k přiřazení `NULL` hodnota o produktu, kategorie nebo dodavatele ([Kliknutím zobrazit obrázek v plné velikosti](customizing-the-data-modification-interface-cs/_static/image36.png))


## <a name="step-4-using-radiobuttons-for-the-discontinued-status"></a>Krok 4: Použití přepínací tlačítka – starší formáty stavu

Aktuálně produkty `Discontinued` datové pole, je vyjádřit pomocí třídy CheckBoxField, která vykreslí zakázané zaškrtávací políčko pro řádky jen pro čtení a povoleno zaškrtávací políčko pro řádek Upravovaný. Při této uživatelské rozhraní je často vhodný, jsme ji v případě potřeby pomocí TemplateField přizpůsobit. V tomto kurzu Změníme Vlastnost CheckBoxField do TemplateField používající ovládací prvek RadioButtonList dvě možnosti "Aktivní" a "Zastaví" ze kterého může uživatel zadat produktu `Discontinued` hodnotu.

Začněte tím, že převod `Discontinued` Vlastnost CheckBoxField do TemplateField, která vytvoří TemplateField s `ItemTemplate` a `EditItemTemplate`. Obě šablony zahrnují zaškrtávací políčko s jeho `Checked` vlastnost vázána na `Discontinued` pole dat, je jediným rozdílem mezi dvěma, který se `ItemTemplate`na zaškrtávací políčko je `Enabled` je nastavena na `false`.

Nahraďte políčka v obou `ItemTemplate` a `EditItemTemplate` s ovládacím prvkem RadioButtonList nastavení obou RadioButtonLists `ID` vlastnosti, které chcete `DiscontinuedChoice`. V dalším kroku určujících, zda RadioButtonLists by každý obsahují dva přepínače, jeden s popiskem "aktivní" s hodnotou "False" a jednu s názvem bez přípony "Zastaví" s hodnotou "True". K tomu můžete buď zadat `<asp:ListItem>` elementů v přímo pomocí deklarativní syntaxe nebo pomocí `ListItem` Editor kolekce z návrháře. Obrázek 13 ukazuje `ListItem` bylo zadáno Editor kolekce po dvou přepínač – tlačítko Možnosti.


[![Přidat](customizing-the-data-modification-interface-cs/_static/image38.png)](customizing-the-data-modification-interface-cs/_static/image37.png)

**Obrázek 13**: přidání "Aktivní" a "Nákup ukončen" možnosti do RadioButtonList ([Kliknutím zobrazit obrázek v plné velikosti](customizing-the-data-modification-interface-cs/_static/image39.png))


Od RadioButtonList v `ItemTemplate` by neměl být upravovat, nastavit jeho `Enabled` vlastnost `false`, výstupu `Enabled` vlastnost `true` (výchozí) pro RadioButtonList v `EditItemTemplate`. To bude přepínací tlačítka v řádku upravovat jen pro čtení, ale umožní uživateli změnu hodnoty RadioButton upravená řádek.

Musíme přiřadit ovládací prvky RadioButtonList `SelectedValue` vlastnosti tak, aby příslušný přepínač je vybráno podle produktu `Discontinued` datové pole. Stejně jako u DropDownLists zkontrolován dříve v tomto kurzu, mohou být tato syntaxe datové vazby přidány do deklarativní přímo nebo prostřednictvím na odkaz Upravit datové vazby v RadioButtonLists se inteligentní značky.

Po přidání dvě RadioButtonLists a konfiguraci, `Discontinued` na TemplateField deklarativní by měl vypadat jako:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample7.aspx)]

Tyto změny `Discontinued` transformaci sloupce ze seznamu zaškrtávací políčka na seznam přepínačů tlačítko páry (viz obrázek 14). Při úpravě produktu, je vybrán příslušný přepínač a stav – starší formáty produktu lze aktualizovat výběrem jiné přepínač a kliknutím na tlačítko Aktualizovat.


[![– Starší formáty políček byly nahrazeny páry tlačítko přepínačů](customizing-the-data-modification-interface-cs/_static/image41.png)](customizing-the-data-modification-interface-cs/_static/image40.png)

**Obrázek 14**: zastaví zaškrtávací políčka byly nahrazeny přepínač tlačítko páry ([Kliknutím zobrazit obrázek v plné velikosti](customizing-the-data-modification-interface-cs/_static/image42.png))


> [!NOTE]
> Vzhledem k tomu `Discontinued` sloupec v `Products` databáze nemůže mít `NULL` hodnoty, jsme nemusíte si dělat starosti o zachycení `NULL` informace v rozhraní. V případě, ale `Discontinued` sloupec může obsahovat `NULL` hodnoty by chceme přidat třetí přepínač do seznamu s jeho `Value` nastavit na prázdný řetězec (`Value=""`), jenom jako kategorie a dodavatele DropDownLists.


## <a name="summary"></a>Souhrn

Zatímco BoundField a vlastnost CheckBoxField automaticky generují jen pro čtení, úpravám a vkládání rozhraní, nemají možnost pro vlastní nastavení. Často, ale budeme muset přizpůsobit úprav nebo vkládání rozhraní, možná přidávání ovládacích prvků ověření (jak jsme viděli v předchozím kurzu) nebo přizpůsobením uživatelského rozhraní sběru dat (jak jsme viděli v tomto kurzu). Přizpůsobení rozhraní s TemplateField můžete sumarizovány v následujících krocích:

1. Přidat TemplateField nebo převést na existující BoundField nebo vlastnost CheckBoxField TemplateField
2. Posílení rozhraní podle potřeby
3. Navázat pole příslušná data na nově přidané ovládací prvky webového pomocí obousměrné vazby dat.

Kromě používání předdefinovaných ovládacích prvků technologie ASP.NET, můžete také upravit šablony TemplateField s ovládacími prvky vlastní kompilované serveru a uživatelské ovládací prvky.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Předchozí](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)
> [další](implementing-optimistic-concurrency-cs.md)
