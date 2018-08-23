---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs
title: Přizpůsobení rozhraní pro úpravu dat (C#) | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu podíváme na tom, jak přizpůsobit rozhraní upravovat prvku GridView, tak, že nahradíte standardního textového pole a ovládací prvky zaškrtávacího políčka s ternativní...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 22e99600-8d18-4a94-a20e-a3a62bb63798
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: f7004192edd636f4660f3184c3e725a6bfda865c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754530"
---
<a name="customizing-the-data-modification-interface-c"></a>Přizpůsobení rozhraní pro úpravu dat (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_20_CS.exe) nebo [stahovat PDF](customizing-the-data-modification-interface-cs/_static/datatutorial20cs1.pdf)

> V tomto kurzu podíváme na tom, jak přizpůsobit rozhraní upravovat prvku GridView, tak, že nahradíte standardního textového pole a s alternativní vstupní ovládací prvky webové ovládací prvky CheckBox.


## <a name="introduction"></a>Úvod

BoundFields a CheckBoxFields používané ovládací prvky GridView a DetailsView zjednodušit proces úpravy dat z důvodu jejich možnost vykreslovat jen pro čtení, upravovat a Vložitelný rozhraní. Tato rozhraní lze vykreslit bez přidání jakékoli další deklarativní nebo kódu. Vlastnost BoundField a na třídě CheckBoxField rozhraní však nemají přizpůsobitelnost často je potřeba v reálné situace. Aby bylo možné přizpůsobit rozhraní upravovatelného nebo Vložitelný v prvku GridView nebo musíme místo toho použít na pole TemplateField prvku DetailsView.

V [předchozím kurzu](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) jsme viděli, jak přizpůsobit rozhraní pro úpravy dat tak, že přidáte ovládací prvky webového ověřování. V tomto kurzu podíváme na tom, jak přizpůsobit skutečné kolekce webových ovládacích prvcích dat, nahradí Vlastnost BoundField a standardního textového pole na třídě CheckBoxField a ovládacích prvků CheckBox s alternativní vstupní ovládací prvky webové. Zejména vytvoříme upravitelné prvku GridView, která umožňuje produktu název, kategorie, dodavatele a ukončená stav aktualizace. Při úpravách konkrétního řádku, kategorie a dodavatele pole se zobrazí takto DropDownLists, obsahující sadu dostupných kategorií a všichni dodavatelé lze vybírat. Kromě toho jsme budete nahraďte výchozí třídě CheckBoxField zaškrtávací políčko RadioButtonList ovládací prvek, který nabízí dvě možnosti: "Aktivní" a "Pozastaveno".


[![Rozhraní pro úpravy prvku GridView zahrnuje DropDownLists a přepínači (RadioButtons)](customizing-the-data-modification-interface-cs/_static/image2.png)](customizing-the-data-modification-interface-cs/_static/image1.png)

**Obrázek 1**: úpravy DropDownLists zahrnuje rozhraní a přepínačů prvku GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](customizing-the-data-modification-interface-cs/_static/image3.png))


## <a name="step-1-creating-the-appropriateupdateproductoverload"></a>Krok 1: Vytvoření odpovídající`UpdateProduct`přetížení

V tomto kurzu vytvoříme upravitelné prvku GridView, která umožňuje úpravy název produktu, kategorie, dodavatele a vyřazuje stav. Proto potřebujeme `UpdateProduct` přetížení přijímající pět vstupní parametry tyto čtyři produktu hodnoty plus `ProductID`. V naše předchozí přetížení, bude tento jeden, jako jsou:

1. Získávání informací o produktech z databáze pro zadaný rozbočovač `ProductID`,
2. Aktualizace `ProductName`, `CategoryID`, `SupplierID`, a `Discontinued` polí, a
3. Odeslat žádost o aktualizaci vrstvy Dal prostřednictvím objektu TableAdapter `Update()` metody.

Jako stručný výtah pro toto konkrétní přetížení můžu jste vynechány kontroly obchodní pravidla, která zajistí, že produkt je označen jako ukončena není jenom produkt nabízí jeho dodavatele. Teď můžete ho přidat, pokud dáváte přednost, nebo, v ideálním případě Refaktorujte si logiku pro samostatné metodě.

Následující kód ukazuje novou `UpdateProduct` přetížení v `ProductsBLL` třídy:


[!code-csharp[Main](customizing-the-data-modification-interface-cs/samples/sample1.cs)]

## <a name="step-2-crafting-the-editable-gridview"></a>Krok 2: Vytvoření upravitelné GridView

S `UpdateProduct` přetížení přidali jsme připraveni vytvořit naše upravitelné ovládacího prvku GridView. Otevřít `CustomizedUI.aspx` stránku `EditInsertDelete` složky a přidejte ovládací prvek GridView do návrháře. Dále vytvořte nový prvek ObjectDataSource z inteligentních značek v prvku GridView. Konfigurace ObjectDataSource k získávání informací o produktech přes `ProductBLL` třídy `GetProducts()` metoda a aktualizovat data pomocí produktu `UpdateProduct` přetížení, které jsme právě vytvořili. Z karty INSERT a DELETE vyberte z rozevíracích seznamů (žádné).


[![Konfigurace ObjectDataSource použít přetížení UpdateProduct právě vytvořili](customizing-the-data-modification-interface-cs/_static/image5.png)](customizing-the-data-modification-interface-cs/_static/image4.png)

**Obrázek 2**: Konfigurace ObjectDataSource k použití `UpdateProduct` přetížení právě vytvořili ([kliknutím ji zobrazíte obrázek v plné velikosti](customizing-the-data-modification-interface-cs/_static/image6.png))


Jak jsme viděli v rámci kurzů změny dat, přiřadí deklarativní syntaxe pro prvek ObjectDataSource, vytvořených pomocí Visual Studia `OldValuesParameterFormatString` vlastnost `original_{0}`. To, samozřejmě, nebude fungovat s naší vrstvy obchodní logiky od našich metody Neočekáváme, že původní `ProductID` hodnota, která má být předán v. Proto se jako v předchozích kurzech, věnujte chvíli odstranit toto přiřazení vlastnosti z deklarativní syntaxe, nebo místo toho nastavte hodnotu této vlastnosti na `{0}`.

Po této změně ObjectDataSource deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample2.aspx)]

Všimněte si, že `OldValuesParameterFormatString` byla odebrána vlastnost a zda je `Parameter` v `UpdateParameters` kolekce pro každý vstupní parametry očekává naše `UpdateProduct` přetížení.

I když prvku ObjectDataSource je nakonfigurovat aktualizovat pouze podmnožinu hodnoty produktu, prvku GridView aktuálně ukazuje *všechny* polí produktů. Za chvíli upravit prvku GridView tak, aby:

- Zahrnuje pouze `ProductName`, `SupplierName`, `CategoryName` BoundFields a `Discontinued` třídě CheckBoxField
- `CategoryName` a `SupplierName` polí vyskytovat před (nalevo) `Discontinued` třídě CheckBoxField
- `CategoryName` a `SupplierName` BoundFields' `HeaderText` vlastnost je nastavena na "Kategorie" a "Poskytovatel"
- Je povolená podpora úpravy (zaškrtnutím políčka Povolit úpravy v prvku GridView inteligentní značky)

Po provedení těchto změn bude návrháře vypadat podobně jako na obrázku 3 s prvku GridView deklarativní syntaxe uvedená níže.


[![Odebrat nepotřebné pole z prvku GridView.](customizing-the-data-modification-interface-cs/_static/image8.png)](customizing-the-data-modification-interface-cs/_static/image7.png)

**Obrázek 3**: odeberte nepotřebné pole z prvku GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](customizing-the-data-modification-interface-cs/_static/image9.png))


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample3.aspx)]

V tomto okamžiku je kompletní chování jen pro čtení v prvku GridView. Při prohlížení dat, každý produkt se vykreslí jako řádku v prvku GridView zobrazující název produktu, kategorie, dodavatel a vyřazuje stav.


[![Rozhraní prvku GridView jen pro čtení je dokončen.](customizing-the-data-modification-interface-cs/_static/image11.png)](customizing-the-data-modification-interface-cs/_static/image10.png)

**Obrázek 4**: rozhraní jen pro čtení prvku GridView bylo dokončeno ([kliknutím ji zobrazíte obrázek v plné velikosti](customizing-the-data-modification-interface-cs/_static/image12.png))


> [!NOTE]
> Jak je popsáno v [přehled o vložení, aktualizace a odstranění dat kurzu](an-overview-of-inserting-updating-and-deleting-data-cs.md), je životně důležitá, že GridView s zobrazení stav povoleno (výchozí chování). Pokud nastavíte GridView s `EnableViewState` vlastnost `false`, spustíte riskujete souběžných uživatelů zaznamenává neúmyslnému odstranění nebo úpravy. V tématu [upozornění: problém souběžnosti s ASP.NET 2.0 prvků GridViews/DetailsView/FormViews tohoto podpora úpravy nebo odstranění a jejichž stav zobrazení je zakázaná](http://scottonwriting.net/sowblog/posts/10054.aspx) Další informace.


## <a name="step-3-using-a-dropdownlist-for-the-category-and-supplier-editing-interfaces"></a>Krok 3: Použití DropDownList kategorie a úpravy rozhraní dodavatele

Vzpomeňte si, že `ProductsRow` objekt obsahuje `CategoryID`, `CategoryName`, `SupplierID`, a `SupplierName` vlastnosti, které obsahují skutečné hodnoty ID cizího klíče v `Products` databáze tabulky a odpovídající `Name` hodnoty v `Categories` a `Suppliers` tabulky. `ProductRow`Společnosti `CategoryID` a `SupplierID` lze obě číst z a zapisovat do while `CategoryName` a `SupplierName` vlastnosti jsou označeny jen pro čtení.

Kvůli stavu jen pro čtení `CategoryName` a `SupplierName` vlastnosti, měli odpovídající BoundFields jejich `ReadOnly` nastavenou na `true`, brání tyto hodnoty z právě upravována při úpravě řádku. Když jsme můžete nastavit `ReadOnly` vlastnost `false`, vykreslení `CategoryName` a `SupplierName` BoundFields jako textová pole při úpravách, tento postup způsobí výjimku když se uživatel pokusí k aktualizaci produktu, protože neexistuje žádný `UpdateProduct` přetížení, které přijímá `CategoryName` a `SupplierName` vstupy. Ve skutečnosti nechceme přetížit těchto dvou důvodů:

- `Products` Tabulka neobsahuje `SupplierName` nebo `CategoryName` pole, ale `SupplierID` a `CategoryID`. Proto chceme, aby naše metody mají být předány tyto konkrétní hodnoty ID, ne hodnoty vyhledávacími tabulkami.
- By uživatel musel zadejte název dodavatele nebo kategorie je menší než ideální, jak to vyžaduje, aby uživatel znát dostupné kategorie a dodavatelů a jejich správnou slova.

Pole dodavatele a kategorie by měl zobrazit kategorie a dodavatelů názvy v režimu jen pro čtení (stejně jako teď) a rozevírací seznam příslušných políček, když se upraví. Pomocí rozevíracího seznamu, koncový uživatel může rychle zjistit, jaké kategorie a Dodavatelé jsou k dispozici na výběr mezi, a informace snadno provádět jejich výběr.

Kvůli tomuto chování My potřebujeme převést `SupplierName` a `CategoryName` BoundFields do vlastností TemplateField jehož `ItemTemplate` vysílá `SupplierName` a `CategoryName` hodnoty a jehož `EditItemTemplate` používá ovládací prvek DropDownList do seznamu Dostupné kategorie a dodavatelů.

## <a name="adding-thecategoriesandsuppliersdropdownlists"></a>Přidávání`Categories`a`Suppliers`DropDownLists

Začněte tím, že převod `SupplierName` a `CategoryName` BoundFields do vlastností TemplateField podle: Kliknutím na odkaz Upravit sloupce v prvku GridView inteligentních značek; výběrem ze seznamu v levém dolním; Vlastnost BoundField a kliknutím na "převést toto pole do Odkaz na pole TemplateField". Proces převodu vytvoří TemplateField v rámci `ItemTemplate` a `EditItemTemplate`, jak je znázorněno níže uvedené deklarativní syntaxe:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample4.aspx)]

Vzhledem k tomu, vlastnost BoundField byla označena jako jen pro čtení, jak `ItemTemplate` a `EditItemTemplate` obsahovat popisek webové ovládací prvek, jehož `Text` vlastnost je vázána na příslušné datové pole (`CategoryName`, v syntaxi výše). Potřebujeme upravit `EditItemTemplate`, nahraďte ovládací prvek webového popisek s ovládacím prvkem DropDownList.

Jak jsme viděli v předchozích kurzech, šablona se dá upravit pomocí návrháře nebo přímo z deklarativní syntaxe. Upravovat pomocí návrháře, klikněte na odkaz Upravit šablony z prvku GridView inteligentních značek a pracovat s polem kategorie `EditItemTemplate`. Odebrání popisku webového ovládacího prvku a nahraďte ji metodou nastavením vlastnosti ID DropDownList na ovládací prvek DropDownList `Categories`.


[![Odeberte TexBox a přidejte EditItemTemplate DropDownList](customizing-the-data-modification-interface-cs/_static/image14.png)](customizing-the-data-modification-interface-cs/_static/image13.png)

**Obrázek 5**: Odeberte TexBox a přidejte DropDownList k `EditItemTemplate` ([kliknutím ji zobrazíte obrázek v plné velikosti](customizing-the-data-modification-interface-cs/_static/image15.png))


Dále je potřeba vyplnit DropDownList s dostupné kategorie. Klikněte na odkaz zvolit zdroj dat z inteligentních značek DropDownList a rozhodnout vytvořit nového prvku ObjectDataSource s názvem `CategoriesDataSource`.


[![Vytvoření nového ovládacího prvku ObjectDataSource s názvem CategoriesDataSource](customizing-the-data-modification-interface-cs/_static/image17.png)](customizing-the-data-modification-interface-cs/_static/image16.png)

**Obrázek 6**: Vytvořte nový ovládací prvek ObjectDataSource název `CategoriesDataSource` ([kliknutím ji zobrazíte obrázek v plné velikosti](customizing-the-data-modification-interface-cs/_static/image18.png))


Pokud chcete, aby tento prvek ObjectDataSource, vrátí všechny kategorie, vytvořte mu vazbu k `CategoriesBLL` třídy `GetCategories()` metody.


[![Svázat ObjectDataSource CategoriesBLL GetCategories() – metoda](customizing-the-data-modification-interface-cs/_static/image20.png)](customizing-the-data-modification-interface-cs/_static/image19.png)

**Obrázek 7**: ObjectDataSource k vytvoření vazby `CategoriesBLL`společnosti `GetCategories()` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](customizing-the-data-modification-interface-cs/_static/image21.png))


A konečně, nakonfigurujte nastavení DropDownList tak, aby `CategoryName` pole se zobrazí v každé DropDownList `ListItem` s `CategoryID` pole, které slouží jako hodnotu.


[![Zobrazí pole CategoryName a jako hodnota použita ID kategorie](customizing-the-data-modification-interface-cs/_static/image23.png)](customizing-the-data-modification-interface-cs/_static/image22.png)

**Obrázek 8**: mají `CategoryName` zobrazí pole a `CategoryID` použít jako hodnotu ([kliknutím ji zobrazíte obrázek v plné velikosti](customizing-the-data-modification-interface-cs/_static/image24.png))


Po provedení těchto změn deklarativní `EditItemTemplate` v `CategoryName` TemplateField bude obsahovat DropDownList a prvku ObjectDataSource:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample5.aspx)]

> [!NOTE]
> Rozevírací seznam v `EditItemTemplate` musí mít svůj stav zobrazení povolený. Brzy přidáme Syntaxe datové vazby deklarativní syntaxe DropDownList a datové vazby příkazy jako `Eval()` a `Bind()` může být použit pouze v ovládacích prvcích, jehož stav zobrazení je povolen.


Opakujte tyto kroky pro přidání DropDownList s názvem `Suppliers` k `SupplierName` společnosti TemplateField `EditItemTemplate`. To bude zahrnovat DropDownList k přidání `EditItemTemplate` a vytváření jiný prvek ObjectDataSource. `Suppliers` DropDownList prvku ObjectDataSource, ale by měl být nakonfigurovaný k vyvolání `SuppliersBLL` třídy `GetSuppliers()` metody. Kromě toho konfigurace `Suppliers` DropDownList zobrazíte `CompanyName` pole a použít `SupplierID` pole jako hodnotu pro jeho `ListItem` s.

Po přidání DropDownLists do dvou `EditItemTemplate` s, načtení stránky v prohlížeči a klikněte na tlačítko Upravit Chef Anton Cajun Seasoning produktu. Jak je vidět na obrázku 9, kategorie a dodavatele sloupce produktu jsou vykresleny jako obsahující dostupné kategorie a dodavatelé zvolit z rozevíracích seznamů. Mějte však na paměti, která *první* v obou rozevíracích seznamech jsou vybrané položky ve výchozím nastavení (kategorie Nápoje) a exotické kapaliny jako dodavatele, i když Chef Anton Cajun Seasoning přísady poskytnutých Cajun Orleans nový Delights.


[![Ve výchozím nastavení se vybere první položka v rozevíracích seznamech](customizing-the-data-modification-interface-cs/_static/image26.png)](customizing-the-data-modification-interface-cs/_static/image25.png)

**Obrázek 9**: první položky v rozevírací seznam obsahuje ve výchozím nastavení zaškrtnuto ([kliknutím ji zobrazíte obrázek v plné velikosti](customizing-the-data-modification-interface-cs/_static/image27.png))


Kromě toho pokud kliknutím na tlačítko Aktualizovat, zjistíte, která produktu `CategoryID` a `SupplierID` hodnoty jsou nastaveny na `NULL`. Obě tyto nežádoucí chování se nezdařila, protože DropDownLists v `EditItemTemplate` s nejsou vázány na všechna pole data z podkladových dat produktu.

## <a name="binding-the-dropdownlists-to-thecategoryidandsupplieriddata-fields"></a>DropDownLists k vytvoření vazby`CategoryID`a`SupplierID`datových polí

Abyste měli kategorie a dodavatele upravených produktu rozevírací seznamy nastavte na odpovídající hodnoty a tyto hodnoty odeslány zpět BLL `UpdateProduct` metoda při kliknutím na tlačítko Aktualizovat, potřebujeme vytvořit vazbu DropDownLists `SelectedValue` vlastnosti, které chcete `CategoryID` a `SupplierID` datových polí pomocí dvousměrnou datovou vazbou. K tomu se `Categories` DropDownList, můžete přidat `SelectedValue='<%# Bind("CategoryID") %>'` přímo do deklarativní syntaxe.

Alternativně můžete nastavit DropDownList databindings úpravy šablony prostřednictvím návrháře a kliknutím na odkaz upravit vlastnosti DataBindings z inteligentních značek DropDownList. Dále, která označuje, že `SelectedValue` vlastnost by měla být vázána na `CategoryID` pole pomocí dvousměrnou datovou vazbou (viz obrázek 10). Postupujte stejně buď jako deklarativní nebo návrháře k vytvoření vazby `SupplierID` datové pole `Suppliers` DropDownList.


[![Vytvořit vazbu CategoryID vlastnost SelectedValue DropDownList pomocí dvousměrnou datovou vazbou](customizing-the-data-modification-interface-cs/_static/image29.png)](customizing-the-data-modification-interface-cs/_static/image28.png)

**Obrázek 10**: vytvoření vazby `CategoryID` k DropDownList `SelectedValue` datové vlastnosti pomocí obousměrné vazby ([kliknutím ji zobrazíte obrázek v plné velikosti](customizing-the-data-modification-interface-cs/_static/image30.png))


Jakmile se použily vazby `SelectedValue` vlastnosti dvě DropDownLists upravených produktové kategorie a dodavatele sloupce budou ve výchozím nastavení hodnoty aktuální produkt. Po kliknutí na tlačítko aktualizace, `CategoryID` a `SupplierID` hodnoty rozevíracího seznamu vybrané položky budou předána pracovnímu `UpdateProduct` metody. Obrázku 11 můžete vidět v kurzu po přidání datové vazby příkazů; Všimněte si, jak jsou položky vybrané rozevíracího seznamu pro Chef Anton Cajun Seasoning správně přísady a nový Orleans Cajun Delights.


[![Ve výchozím nastavení se vybere aktuální kategorii a dodavatele hodnoty upravit produkt](customizing-the-data-modification-interface-cs/_static/image32.png)](customizing-the-data-modification-interface-cs/_static/image31.png)

**Obrázek 11**: ve výchozím nastavení jsou vybrány The upravit aktuální kategorii a produktu dodavatele hodnoty ([kliknutím ji zobrazíte obrázek v plné velikosti](customizing-the-data-modification-interface-cs/_static/image33.png))


## <a name="handlingnullvalues"></a>Zpracování`NULL`hodnoty

`CategoryID` a `SupplierID` sloupců v `Products` tabulka se dá `NULL`, ale DropDownLists v `EditItemTemplate` s nezahrnují položku seznamu pro reprezentaci `NULL` hodnotu. To má dva důsledky:

- Uživatele nelze změnit kategorie produktu nebo na dodavatele z non - pomocí našeho rozhraní`NULL` hodnota, která se `NULL` jeden
- Pokud má produkt `NULL` `CategoryID` nebo `SupplierID`, kliknutím na tlačítko Upravit bude mít za následek výjimku. Důvodem je, že `NULL` hodnoty vrácené `CategoryID` (nebo `SupplierID`) v `Bind()` příkaz nemapuje na hodnotu v DropDownList (DropDownList vyvolá výjimku při jeho `SelectedValue` je nastavena na hodnotu *není* v jeho kolekci položek seznamu).

Za účelem podpory `NULL` `CategoryID` a `SupplierID` hodnoty, musíme přidejte další `ListItem` pro každý DropDownList k reprezentaci `NULL` hodnotu. V [filtrování záznamů Master/Detail s DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) kurzu jsme viděli, jak přidat další `ListItem` k vazeb DropDownList, které součástí nastavení DropDownList `AppendDataBoundItems` vlastnost `true` a Ruční přidání dalšího `ListItem`. V předchozím kurzu, ale jsme přidali `ListItem` s `Value` z `-1`. Logika datové vazby v technologii ASP.NET, ale se automaticky převede na prázdný řetězec `NULL` hodnotu a naopak. Proto pro účely tohoto kurzu chceme, aby `ListItem`společnosti `Value` být prázdný řetězec.

Začněte tím, že nastavení obě DropDownLists `AppendDataBoundItems` vlastnost `true`. V dalším kroku přidejte `NULL` `ListItem` přidáním následujícího kódu `<asp:ListItem>` element na každý DropDownList tak, aby deklarativní, jako jsou:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample6.aspx)]

Jsem se rozhodl používat "(None)" jako textová hodnota, která to `ListItem`, ale můžete ho také být prázdný řetězec, pokud chcete změnit.

> [!NOTE]
> Jak jsme viděli v *filtrování záznamů Master/Detail s DropDownList* kurzu `ListItem` s lze přidat do DropDownList prostřednictvím návrháře kliknutím na DropDownList `Items` vlastností v okně Vlastnosti (který Zobrazí `ListItem` Editor kolekce). Určitě chcete přidat `NULL` `ListItem` pro účely tohoto kurzu využijte deklarativní syntaxi. Pokud používáte `ListItem` Editor kolekce generované deklarativní syntaxe vynechá `Value` nastavení úplně při přiřazen prázdný řetězec, deklarativní, jako je vytváření: `<asp:ListItem>(None)</asp:ListItem>`. Když to může vypadat neškodné, chybějící hodnoty způsobí, že DropDownList používat `Text` hodnotu vlastnosti na příslušné místo. To znamená, že pokud to `NULL` `ListItem` je vybrán, hodnota "(None)" se pokusí vytvořit přiřazení `CategoryID`, což povede k výjimce. Explicitním nastavením `Value=""`, `NULL` hodnota přiřazena `CategoryID` při `NULL` `ListItem` zaškrtnuto.


Tento postup opakujte pro DropDownList dodavatelů.

S tímto Další `ListItem`, teď můžete přiřadit rozhraní úprav `NULL` hodnoty na produkt `CategoryID` a `SupplierID` pole, jak ukazuje obrázek 12.


[![Zvolte (žádný) Chcete-li přiřadit hodnotu NULL pro kategorii produktu nebo na dodavatele](customizing-the-data-modification-interface-cs/_static/image35.png)](customizing-the-data-modification-interface-cs/_static/image34.png)

**Obrázek 12**: Zvolte (žádný) k přiřazení `NULL` hodnotu o produktu, kategorie nebo dodavatele ([kliknutím ji zobrazíte obrázek v plné velikosti](customizing-the-data-modification-interface-cs/_static/image36.png))


## <a name="step-4-using-radiobuttons-for-the-discontinued-status"></a>Krok 4: Využití RadioButtons ukončená stavu

Aktuálně produktů `Discontinued` datové pole je vyjádřena pomocí třídy CheckBoxField, která vykresluje zakázané zaškrtávací políčko pro řádky jen pro čtení a povolené zaškrtávací políčko pro řádek, který právě upravujete. Když toto uživatelské rozhraní je často vhodné, jsme ji upravit v případě potřeby pomocí TemplateField. Pro účely tohoto kurzu Změníme třídě CheckBoxField na pole TemplateField, která ho používá dvě možnosti "Aktivní" a "Pozastaveno" RadioButtonList ze kterého může uživatel zadat produktu `Discontinued` hodnotu.

Začněte tím, že převod `Discontinued` třídě CheckBoxField na pole TemplateField, který vytvoří TemplateField s `ItemTemplate` a `EditItemTemplate`. Obě šablony zahrnují zaškrtávacího políčka s jeho `Checked` vlastnost vázána na `Discontinued` datové pole, jediným rozdílem mezi těmito dvěma, který se `ItemTemplate`na zaškrtávací políčko pro `Enabled` je nastavena na `false`.

Nahraďte zaškrtávacího políčka v obou `ItemTemplate` a `EditItemTemplate` s ovládacím prvkem RadioButtonList nastavení obě RadioButtonLists `ID` vlastností `DiscontinuedChoice`. V dalším kroku znamenat, že RadioButtonLists každý může obsahovat dva přepínače, jednu s popiskem "aktivní" s hodnotou "False" a jednu s názvem "Pozastaveno" s hodnotou "True". K tomu můžete zadat buď `<asp:ListItem>` elementů v přímo prostřednictvím deklarativní syntaxe nebo použití `ListItem` Editor kolekce z návrháře. Obrázek 13 ukazuje `ListItem` zadali Editor kolekce po dvou přepínač tlačítko Možnosti.


[![Přidat](customizing-the-data-modification-interface-cs/_static/image38.png)](customizing-the-data-modification-interface-cs/_static/image37.png)

**Obrázek 13**: Přidat "Aktivní" a "Vyřazeno" možnosti RadioButtonList ([kliknutím ji zobrazíte obrázek v plné velikosti](customizing-the-data-modification-interface-cs/_static/image39.png))


Od RadioButtonList v `ItemTemplate` by neměl být možné upravovat, nastavit jeho `Enabled` vlastnost `false`, odejít ze `Enabled` vlastnost `true` (výchozí) pro RadioButtonList v `EditItemTemplate`. To budou přepínačů v řádku číselně upravené jen pro čtení, ale umožní uživateli změnu hodnoty ovládacího prvku RadioButton upravených řádku.

Stále potřebujeme přiřadit ovládací prvky RadioButtonList `SelectedValue` vlastnosti tak, aby vybraný příslušný přepínač na základě produktu `Discontinued` datové pole. Jak ovládacích prvků DropDownList zkontrolován dříve v tomto kurzu, mohou být tato syntaxe vázání dat přidány přímo do deklarativní nebo prostřednictvím odkazu upravit datové vazby v RadioButtonLists' inteligentní značky.

Po přidání dvou RadioButtonLists a nakonfigurovaly, `Discontinued` deklarativní TemplateField společnosti by měl vypadat takto:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample7.aspx)]

S těmito změnami `Discontinued` transformaci sloupce ze seznamu zaškrtávacích políček do seznamu párů tlačítko přepínače (viz obrázek 14). Při úpravě produktu, se určí příslušný přepínač a produktu ukončená stav lze aktualizovat výběrem jiné tlačítko přepínače a kliknutím na tlačítko Aktualizovat.


[![Ukončená zaškrtávací políčka byly nahrazeny páry tlačítko přepínače](customizing-the-data-modification-interface-cs/_static/image41.png)](customizing-the-data-modification-interface-cs/_static/image40.png)

**Obrázek 14**: The ukončena zaškrtávací políčka byly nahrazeny páry tlačítko přepínače ([kliknutím ji zobrazíte obrázek v plné velikosti](customizing-the-data-modification-interface-cs/_static/image42.png))


> [!NOTE]
> Protože `Discontinued` sloupec v `Products` databáze nemůže mít `NULL` hodnoty, jsme nemusíte starat o zachytávání `NULL` informace v rozhraní. Pokud však `Discontinued` sloupec může obsahovat `NULL` hodnoty by chceme přidat třetí přepínač do seznamu s jeho `Value` nastavenou na prázdný řetězec (`Value=""`), stejně jako u kategorie a dodavatele DropDownLists.


## <a name="summary"></a>Souhrn

Vlastnost BoundField a třídě CheckBoxField automaticky generují jen pro čtení, úpravy a vložení rozhraní, nemají možnosti vlastního nastavení. Často, ale budeme muset přizpůsobit úpravy nebo vložení rozhraní, třeba přidání validačních ovládacích prvků (jak jsme viděli v předchozím kurzu) nebo přizpůsobením uživatelského rozhraní sběru dat (jak jsme viděli v tomto kurzu). Přizpůsobení rozhraní s TemplateField může být sečteno v následujících krocích:

1. Přidat TemplateField nebo převod existující vlastnost BoundField nebo třídě CheckBoxField na pole TemplateField
2. Podle potřeby rozšířit rozhraní
3. Vytvořit vazbu pole příslušná data u nově přidaných ovládacích prvků pomocí dvousměrnou datovou vazbou

Kromě použití integrovaných ovládacích prvků technologie ASP.NET, můžete také upravit šablony TemplateField s vlastní, kompilované serverové ovládací prvky a uživatelských ovládacích prvků.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Předchozí](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)
> [další](implementing-optimistic-concurrency-cs.md)
