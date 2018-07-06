---
uid: web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
title: Aktualizace a odstranění stávajících binárních dat (VB) | Dokumentace Microsoftu
author: rick-anderson
description: V předchozích kurzech jsme viděli, jak ovládací prvek GridView umožňují snadno upravovat a odstraňovat textová data. V tomto kurzu uvidíme, jak ovládací prvek GridView také, aby se...
ms.author: aspnetcontent
ms.date: 03/27/2007
ms.assetid: 3a052ced-9cf5-47b8-a400-934f0b687c26
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
msc.type: authoredcontent
ms.openlocfilehash: dd5aea4bfc1a38dc3364cdf2657d3dca2b82022c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830452"
---
<a name="updating-and-deleting-existing-binary-data-vb"></a>Aktualizace a odstranění stávajících binárních dat (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_57_VB.exe) nebo [stahovat PDF](updating-and-deleting-existing-binary-data-vb/_static/datatutorial57vb1.pdf)

> V předchozích kurzech jsme viděli, jak ovládací prvek GridView umožňují snadno upravovat a odstraňovat textová data. V tomto kurzu uvidíme, jak ovládací prvek GridView také umožňuje upravovat a odstraňovat binárních dat, ať už je uloženo v databázi nebo uložené v systému souborů binární data.


## <a name="introduction"></a>Úvod

Za posledních tří kurzů jsme přidali hodně funkce pro práci s binární data. Začali jsme tak, že přidáte `BrochurePath` sloupec, který se `Categories` tabulky a architektury podle nich aktualizuje. Přidali jsme také vrstva přístupu k datům a vrstvu obchodní logiky metody pro práci s existující tabulku s kategorií `Picture` sloupec, který obsahuje binární obsah s souboru bitové kopie. Vytváříme webové stránky prezentovat binární data v GridView odkaz ke stažení pro brožura, s kategorií s obrázku je znázorněno v `<img>` elementu a přidali DetailsView umožňuje uživatelům přidávat nové kategorie a nahrajte data brožura a obrázek.

Všechno, zůstane k implementaci je schopnost upravovat a odstraňovat existující kategorie, které nám budete provádět v tomto kurzu pomocí ovládacího prvku GridView s integrovanou úpravy a odstranění funkce. Při úpravách kategorii, uživatel bude moct volitelně nahrajte nový obrázek nebo mají kategorii dál používat existující. Pro brožura se můžete rozhodnout buď použijte existující si brožuru o nahrát nový brožura a znamenat, že kategorie již má brožuru s ním spojená. Začínáme s let!

## <a name="step-1-updating-the-data-access-layer"></a>Krok 1: Aktualizuje se vrstva přístupu k datům

DAL obsahuje automaticky generovaný `Insert`, `Update`, a `Delete` metody, ale tyto metody byly generovány na základě `CategoriesTableAdapter` s hlavní dotaz, který se nenachází `Picture` sloupce. Proto `Insert` a `Update` metody neobsahují parametry pro zadání binární data pro obrázek kategorie s. Jak jsme to udělali [předchozím kurzu](including-a-file-upload-option-when-adding-a-new-record-vb.md), potřebujeme k vytvoření nové metody aktualizace objektu TableAdapter `Categories` tabulky při zadávání binární data.

Otevřete datovou sadu typu a z návrháře, klikněte pravým tlačítkem na `CategoriesTableAdapter` s záhlaví a chcete přidat dotaz v místní nabídce launche Průvodce konfigurací dotazu TableAdapter. Tento průvodce spustí požádá nám, jak se má TableAdapter dotazovat by měl přístup k databázi. Zvolte možnost použít SQL příkazy a klikněte na tlačítko Další. Dalším krokem vyzve k zadání typu dotazu vygenerování. Protože jsme k vytvoření dotazu na přidání nového záznamu `Categories` tabulku, vyberte aktualizaci a klikněte na tlačítko Další.


[![Vyberte možnost aktualizace](updating-and-deleting-existing-binary-data-vb/_static/image2.png)](updating-and-deleting-existing-binary-data-vb/_static/image1.png)

**Obrázek 1**: Vyberte možnost aktualizace ([kliknutím ji zobrazíte obrázek v plné velikosti](updating-and-deleting-existing-binary-data-vb/_static/image3.png))


Nyní potřebujeme k určení `UPDATE` příkaz jazyka SQL. Průvodce automaticky navrhne `UPDATE` odpovídající v hlavním dotazu objektu TableAdapter s – příkaz (ten, který se aktualizuje `CategoryName`, `Description`, a `BrochurePath` hodnoty). Změňte příkaz tak, aby `Picture` sloupec je součástí podél `@Picture` parametr, takto:


[!code-sql[Main](updating-and-deleting-existing-binary-data-vb/samples/sample1.sql)]

Poslední obrazovka průvodce výzva pojmenujte novou metodu objektu TableAdapter. Zadejte `UpdateWithPicture` a klikněte na tlačítko Dokončit.


[![Název nové UpdateWithPicture TableAdapter – metoda](updating-and-deleting-existing-binary-data-vb/_static/image5.png)](updating-and-deleting-existing-binary-data-vb/_static/image4.png)

**Obrázek 2**: název nové metody třídy TableAdapter `UpdateWithPicture` ([kliknutím ji zobrazíte obrázek v plné velikosti](updating-and-deleting-existing-binary-data-vb/_static/image6.png))


## <a name="step-2-adding-the-business-logic-layer-methods"></a>Krok 2: Přidání metody vrstvy obchodní logiky

Kromě aktualizací DAL, musíme aktualizace BLL zahrnout metod pro aktualizaci a odstraňování kategorie. Toto jsou metody, které se vyvolá od prezentační vrstvy.

Při odstraňování kategorie, můžeme použít `CategoriesTableAdapter` s automaticky generovanou `Delete` metody. Přidejte následující metodu do `CategoriesBLL` třídy:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample2.vb)]

Pro účely tohoto kurzu vám umožňují s vytvořit dvě metody pro aktualizaci kategorie – ten, který očekává, že obrázek binární data a vyvolá `UpdateWithPicture` metoda jsme právě přidali `CategoriesTableAdapter` a další vlastnost, která přijímá jenom `CategoryName`, `Description`a `BrochurePath`hodnoty a používá `CategoriesTableAdapter` třídy s automaticky generovanou `Update` příkazu. Odůvodnění pomocí dvou metod je, že v některých případech se uživateli chtít aktualizovat obrázku s kategorie spolu s jeho další pole, ve kterých se proces uživatel bude mít nahrát nový obrázek. Binární data nahraných obrázků s je pak možné v `UPDATE` příkazu. V jiných případech může uživatel by pouze v aktualizaci, Řekněme, že se název a popis. Avšak v tom případě `UPDATE` příkaz očekává, že binární data `Picture` také sloupec a pak jsme d muset zadat také tyto informace. Bude to vyžadovat další cesty k databázi vrátit data obrázku pro záznam, který právě upravujete. Proto se chceme dvě `UPDATE` metody. Vrstvy obchodní logiky určí, které z nich použít založené na tom, jestli obrázek nejsou k dispozici data při aktualizaci kategorie.

Usnadňuje to přidejte dvě metody, které `CategoriesBLL` třídy, oba s názvem `UpdateCategory`. První z nich by měla přijímat tři `String` s, `Byte` pole a `Integer` jako vstupní parametry; sekundy, pouze ve třech `String` s a `Integer`. `String` Vstupní parametry jsou pro kategorii s názvem, popis a si brožuru o cestu k souboru, `Byte` pole je pro binární obsah obrázek kategorie s a `Integer` identifikuje `CategoryID` z záznam, který chcete aktualizovat. Všimněte si, že první přetížení vyvolá druhou if předaným `Byte` pole je `Nothing`:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample3.vb)]

## <a name="step-3-copying-over-the-insert-and-view-functionality"></a>Krok 3: Kopírování přes Insert a zobrazení funkce

V [předchozím kurzu](including-a-file-upload-option-when-adding-a-new-record-vb.md) jsme vytvořili stránku s názvem `UploadInDetailsView.aspx` , který uvedené všechny kategorie GridView a poskytuje DetailsView do systému přidávat nové kategorie. V tomto kurzu jsme se rozšíří GridView zahrnout úpravy a odstranění podpory. Místo dále pracovat z `UploadInDetailsView.aspx`, umožňují s místo toho umístit tento kurz s změnami `UpdatingAndDeleting.aspx` stránky ze stejné složky, `~/BinaryData`. Zkopírujte a vložte deklarativní a kód od `UploadInDetailsView.aspx` k `UpdatingAndDeleting.aspx`.

Začněte otevřením `UploadInDetailsView.aspx` stránky. Zkopírujte všechny deklarativní syntaxe v rámci `<asp:Content>` elementu, jak je znázorněno na obrázku 3. Dále otevřete `UpdatingAndDeleting.aspx` a vložte tento kód v rámci jeho `<asp:Content>` elementu. Podobně, zkopírovat kód z `UploadInDetailsView.aspx` stránce třídy s použití modelu code-behind `UpdatingAndDeleting.aspx`.


[![Kopírovat deklarativní UploadInDetailsView.aspx](updating-and-deleting-existing-binary-data-vb/_static/image8.png)](updating-and-deleting-existing-binary-data-vb/_static/image7.png)

**Obrázek 3**: zkopírujte z deklarativní `UploadInDetailsView.aspx` ([kliknutím ji zobrazíte obrázek v plné velikosti](updating-and-deleting-existing-binary-data-vb/_static/image9.png))


Po zkopírování přes deklarativní značek a kódu, navštivte `UpdatingAndDeleting.aspx`. Měli byste vidět stejný výstup a mají stejné prostředí pro uživatele stejně jako u `UploadInDetailsView.aspx` stránky z předchozí kurz o službě.

## <a name="step-4-adding-deleting-support-to-the-objectdatasource-and-gridview"></a>Krok 4: Přidání, odstranění podporu ObjectDataSource a GridView

Jak jsme probírali v [přehled o vložení, aktualizace a odstranění dat](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) kurz, poskytuje integrované funkce pro odstranění prvku GridView a tyto možnosti lze povolit v značek zaškrtávací políčko, pokud základní mřížky s zdroj dat podporuje odstranění. Aktuálně ObjectDataSource prvku GridView je vázána na (`CategoriesDataSource`) nepodporuje odstraňování.

Chcete-li to napravit, klikněte na možnost konfigurace zdroje dat z prvku ObjectDataSource s inteligentním spusťte průvodce. Na první obrazovce se zobrazí, že je prvku ObjectDataSource nakonfigurováno pro práci s `CategoriesBLL` třídy. Další spuštění. V současné době pouze prvku ObjectDataSource s `InsertMethod` a `SelectMethod` nejsou zadány vlastnosti. Ale v Průvodci vyplní automaticky rozevírací seznamy na kartách UPDATE a DELETE se `UpdateCategory` a `DeleteCategory` metody, v uvedeném pořadí. Důvodem je, že v `CategoriesBLL` třídy jsme označené jako tyto metody používat `DataObjectMethodAttribute` jako výchozí metody pro aktualizace a odstranění.

Teď nastavte aktualizace kartu s rozevíracím seznamu na (žádný), ale ponechejte odstranit kartu s rozevíracím seznamu nastavte na `DeleteCategory`. Se vrátíme k průvodci v kroku 6 přidává aktualizace.


[![Konfigurace ObjectDataSource DeleteCategory metody](updating-and-deleting-existing-binary-data-vb/_static/image11.png)](updating-and-deleting-existing-binary-data-vb/_static/image10.png)

**Obrázek 4**: Konfigurace ObjectDataSource k použití `DeleteCategory` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](updating-and-deleting-existing-binary-data-vb/_static/image12.png))


> [!NOTE]
> Po dokončení průvodce bude Visual Studio požádat, pokud chcete aktualizovat pole a klíče, který se znova vygeneruje dat webové ovládací prvky pole. Zvolte možnost Ne, protože kliknutím na tlačítko Ano, přepíše všechna pole vlastní nastavení provedené může mít.


Prvku ObjectDataSource teď bude zahrnovat hodnotu pro jeho `DeleteMethod` vlastnost a také `DeleteParameter`. Připomínáme, že při použití průvodce k určují metody, Visual Studio nastaví ObjectDataSource s `OldValuesParameterFormatString` vlastnost `original_{0}`, což způsobuje problémy s aktualizací a odstranit volání metod. Proto buď úplně vymažte tuto vlastnost nebo ho resetovat na výchozí hodnotu `{0}`. Pokud je potřeba aktualizovat paměti pro tuto vlastnost prvek ObjectDataSource, najdete v článku [přehled o vložení, aktualizace a odstranění dat](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) kurzu.

Po dokončení průvodce v odhalování a opravování `OldValuesParameterFormatString`, ObjectDataSource s deklarativní by měla vypadat podobně jako následující:


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample4.aspx)]

Po dokončení konfigurace ObjectDataSource, přidejte do prvku GridView. zaškrtnutím políčka Povolit odstranění z ovládacího prvku GridView s inteligentním odstranění funkce. Tím se přidá do prvku GridView CommandField jehož `ShowDeleteButton` je nastavena na `True`.


[![Povolení podpory pro odstraňování v prvku GridView.](updating-and-deleting-existing-binary-data-vb/_static/image14.png)](updating-and-deleting-existing-binary-data-vb/_static/image13.png)

**Obrázek 5**: Povolení podpory pro odstraňování v prvku GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](updating-and-deleting-existing-binary-data-vb/_static/image15.png))


Využijte k otestování funkce odstraňování. Je cizího klíče mezi `Products` tabulky s `CategoryID` a `Categories` tabulky s `CategoryID`, takže výjimky porušení omezení cizího klíče se zobrazí, když se pokusíte odstranit některé z prvních osm kategorií. K otestování této funkce si přidáte novou kategorii, poskytování si brožuru o i obrázek. Moje kategorie testu je znázorněno na obrázku 6, obsahuje testovací si brožuru o soubor s názvem `Test.pdf` a obrázek testu. Obrázek 7 znázorňuje prvku GridView, po přidání kategorie testu.


[![Přidejte kategorie testu s brožura a bitové kopie](updating-and-deleting-existing-binary-data-vb/_static/image17.png)](updating-and-deleting-existing-binary-data-vb/_static/image16.png)

**Obrázek 6**: Přidejte kategorie testu s brožura a Image ([kliknutím ji zobrazíte obrázek v plné velikosti](updating-and-deleting-existing-binary-data-vb/_static/image18.png))


[![Po vložení kategorie testu, se zobrazí v prvku GridView.](updating-and-deleting-existing-binary-data-vb/_static/image20.png)](updating-and-deleting-existing-binary-data-vb/_static/image19.png)

**Obrázek 7**: Po vložení kategorie testu, se zobrazí v prvku GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](updating-and-deleting-existing-binary-data-vb/_static/image21.png))


V sadě Visual Studio aktualizujte Průzkumníka řešení. Teď byste měli vidět nový soubor v `~/Brochures` složce `Test.pdf` (viz obrázek 8).

Dále klikněte na odkaz pro odstranění řádku kategorie testu, způsobí stránky a odeslat zpět a `CategoriesBLL` třída s `DeleteCategory` metoda vyvolána. Tím se vyvolá s vrstvou DAL `Delete` metody způsobí odpovídající `DELETE` příkazu k odeslání do databáze. Data pak znovu připojeno k prvku GridView a značky jsou odeslány zpět do klienta se kategorie testu už existuje.

Při odstranění pracovního postupu se úspěšně odebral kategorie testu záznam na základě `Categories` tabulky, neodebral jeho si brožuru o souboru ze systému souborů webového serveru s. Aktualizovat Průzkumníka řešení a uvidíte, že `Test.pdf` stále nachází `~/Brochures` složky.


![Soubor Test.pdf nebyl odstraněn ze systému souborů webového serveru s](updating-and-deleting-existing-binary-data-vb/_static/image1.gif)

**Obrázek 8**: `Test.pdf` soubor nebyl odstraněn ze systému souborů webového serveru s


## <a name="step-5-removing-the-deleted-category-s-brochure-file"></a>Krok 5: Odstraňuje se soubor odstranil kategorii s si brožuru o

Jednou z nevýhody ukládání binárních dat, které jsou externí vzhledem k databázi je, že dodatečné kroky je třeba brát k vyčištění těchto souborů při odstranění záznamu přidružená databáze. GridView a ObjectDataSource poskytují události, které budou spuštěny před a po provedení příkazu delete. Ve skutečnosti potřebujeme vytvořit obslužné rutiny událostí pro události před instrumentací a po akci. Před `Categories` odstranění záznamu potřebujeme k určení jeho cesta k souboru s PDF, ale můžeme zadávat t chcete odstranit soubor PDF před odstraněním kategorii v případě je některé výjimky a kategorie se neodstraní.

GridView s [ `RowDeleting` události](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx) je aktivována před příkazem k odstranění prvku ObjectDataSource s byla vyvolána, při jeho [ `RowDeleted` události](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleted.aspx) aktivuje po. Vytváření obslužných rutin událostí pro tyto dvě události pomocí následujícího kódu:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample5.vb)]

V `RowDeleting` obslužné rutiny události `CategoryID` řádku, který odstraňuje je převzatý z ovládacího prvku GridView s `DataKeys` kolekce, která je přístupná v tomto popisovači událostí prostřednictvím `e.Keys` kolekce. Dále `CategoriesBLL` třída s `GetCategoryByCategoryID(categoryID)` je vyvolána k vrácení informací o odstranění záznamu. Pokud vráceného `CategoriesDataRow` objekt má non -`NULL``BrochurePath` hodnotu, není uložen v proměnné stránky `deletedCategorysPdfPath` tak, že soubor je možné odstranit v `RowDeleted` obslužné rutiny události.

> [!NOTE]
> Místo získávání `BrochurePath` podrobnosti pro `Categories` zaznamenat odstraňuje v `RowDeleting` obslužná rutina události, může také přidali jsme `BrochurePath` do ovládacího prvku GridView s `DataKeyNames` vlastnost a k nim přistupovat hodnota záznamu s až `e.Keys` kolekce. Mohlo by mírně dojít ke zvětšení GridView s zobrazení stavu, ale by snížit množství kódu, které jsou potřebné a uložit výlet do databáze.


Po zavolání s základní příkazem k odstranění ObjectDataSource GridView s `RowDeleted` dojde k aktivaci obslužné rutiny události. Pokud nebyly žádné výjimky v odstranění dat a je hodnota pro `deletedCategorysPdfPath`, pak soubor PDF se odstraní ze systému souborů. Všimněte si, že není potřeba tento další kód pro vyčištění kategorie s binární data přidružená k jeho obrázek. Tento s protože přímo v databázi jsou uložena data obrázku, takže odstranění `Categories` řádků se odstraní také tato data obrázku kategorie s.

Po přidání obslužné rutiny událostí dvě, znovu spusťte tento testovací případ. Při odstraňování kategorie, jeho přidružený soubor PDF se také odstraní.

Aktualizace stávajících binárních dat záznamů s přidružené poskytuje některé zajímavé běžné problémy. Zbývající část tohoto kurzu obsahuje podrobné přidávají aktualizace brožura a obrázek. Krok 6 zkoumá techniky pro aktualizaci si brožuru o informace během kroku 7 vyhledá aktualizace na obrázku.

## <a name="step-6-updating-a-category-s-brochure"></a>Krok 6: Aktualizace brožuru s kategorií

Jak je popsáno v kurzu přehled o vložení, aktualizace a odstranění dat prvku GridView nabízí integrovanou podporu úpravy na úrovni řádků, který může být implementována značek zaškrtávací políčko, pokud je její podkladový zdroj dat správně nakonfigurovaný. V současné době `CategoriesDataSource` ObjectDataSource ještě není nakonfigurovaná zahrnout aktualizace podpory, takže vám umožňují s přidat, že v.

Klikněte na odkaz Konfigurovat zdroj dat z prvku ObjectDataSource s průvodce a pokračujte v druhém kroku. Z důvodu `DataObjectMethodAttribute` používané `CategoriesBLL`, rozevírací seznam aktualizace mělo být automaticky vyplněno pomocí `UpdateCategory` přetížení, které přijímá čtyři vstupní parametry (pro všechny sloupce, ale `Picture`). Změňte tak, aby používala přetížení s pěti parametry.


[![Konfigurace ObjectDataSource UpdateCategory metody, která zahrnuje parametr pro obrázek](updating-and-deleting-existing-binary-data-vb/_static/image23.png)](updating-and-deleting-existing-binary-data-vb/_static/image22.png)

**Obrázek 9**: Konfigurace ObjectDataSource k použití `UpdateCategory` metoda, která zahrnuje parametr `Picture` ([kliknutím ji zobrazíte obrázek v plné velikosti](updating-and-deleting-existing-binary-data-vb/_static/image24.png))


Prvku ObjectDataSource teď bude zahrnovat hodnotu pro jeho `UpdateMethod` vlastnosti také odpovídající `UpdateParameter` s. Jak je uvedeno v kroku 4, Visual Studio nastaví ObjectDataSource s `OldValuesParameterFormatString` vlastnost `original_{0}` při použití konfigurace zdroje dat průvodce. To způsobí problémy s aktualizací a odstranit volání metod. Proto buď úplně vymažte tuto vlastnost nebo ho resetovat na výchozí hodnotu `{0}`.

Po dokončení průvodce v odhalování a opravování `OldValuesParameterFormatString`, ObjectDataSource s deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample6.aspx)]

Chcete-li v prvku GridView s integrované funkce úprav, zaškrtněte možnost Povolit úpravy z ovládacího prvku GridView s inteligentním. Tím nastavíte CommandField s `ShowEditButton` vlastnost `True`výsledkem přidání tlačítko pro úpravy (a aktualizace a zrušit tlačítka pro řádek, který právě upravujete).


[![Konfigurace ovládacího prvku GridView pro podporu úpravy](updating-and-deleting-existing-binary-data-vb/_static/image26.png)](updating-and-deleting-existing-binary-data-vb/_static/image25.png)

**Obrázek 10**: Konfigurace GridView pro podporu úpravy ([kliknutím ji zobrazíte obrázek v plné velikosti](updating-and-deleting-existing-binary-data-vb/_static/image27.png))


Na stránce prostřednictvím prohlížeče a klikněte na jeden řádek s tlačítka Upravit. `CategoryName` a `Description` BoundFields jsou vykresleny jako textová pole. `BrochurePath` TemplateField nemá `EditItemTemplate`, takže ho pořád zobrazovat jeho `ItemTemplate` odkaz brožura. `Picture` ImageField vykreslí jako textové pole, jehož `Text` vlastnost je přiřazena hodnota ImageField s `DataImageUrlField` hodnoty v tomto případě `CategoryID`.


[![GridView nemá pro BrochurePath editační rozhraní](updating-and-deleting-existing-binary-data-vb/_static/image29.png)](updating-and-deleting-existing-binary-data-vb/_static/image28.png)

**Obrázek 11**: chybí rozhraní pro úpravy prvku GridView `BrochurePath` ([kliknutím ji zobrazíte obrázek v plné velikosti](updating-and-deleting-existing-binary-data-vb/_static/image30.png))


## <a name="customizing-thebrochurepaths-editing-interface"></a>Přizpůsobení`BrochurePath`rozhraní pro úpravy s

Potřebujeme vytvořit rozhraní pro úpravy `BrochurePath` TemplateField, ten, který umožňuje uživateli buď:

- Nechte si brožuru o kategorie s jako-se,
- Aktualizujte si brožuru o kategorie s nahráním nové si brožuru o nebo
- Odeberte brožura kategorie s úplně (v případě, že kategorie již nemá přidružený brožura).

Musíme také aktualizovat `Picture` ImageField s úpravy rozhraní, používá se ale podíváte na to v kroku 7.

V prvku GridView s inteligentních značek kliknutím na odkaz Upravit šablony a vyberte `BrochurePath` TemplateField s `EditItemTemplate` z rozevíracího seznamu. Přidání ovládacího prvku RadioButtonList na tuto šablonu, nastavení jeho `ID` vlastnost `BrochureOptions` a jeho `AutoPostBack` vlastnost `True`. V okně Vlastnosti klikněte na symbol tří teček v `Items` vlastnost, která se otevře `ListItem` Editor kolekce. Přidejte následující tři možnosti `Value` s 1, 2 a 3, v uvedeném pořadí:

- Aktuální informace o použití
- Odebrat aktuální si brožuru o
- Nahrát nový si brožuru o

Nastavte první `ListItem` s `Selected` vlastnost `True`.


![Přidejte tři položky ListItems RadioButtonList](updating-and-deleting-existing-binary-data-vb/_static/image2.gif)

**Obrázek 12**: Přidejte tři `ListItem` s RadioButtonList


Pod RadioButtonList, přidejte ovládací prvek FileUpload s názvem `BrochureUpload`. Nastavte jeho `Visible` vlastnost `False`.


[![Přidat EditItemTemplate RadioButtonList a FileUpload ovládacího prvku](updating-and-deleting-existing-binary-data-vb/_static/image32.png)](updating-and-deleting-existing-binary-data-vb/_static/image31.png)

**Obrázek 13**: Přidat RadioButtonList a odesílání souborů při odpovědích řízení `EditItemTemplate` ([kliknutím ji zobrazíte obrázek v plné velikosti](updating-and-deleting-existing-binary-data-vb/_static/image33.png))


Tato RadioButtonList poskytuje tři možnosti pro uživatele. Cílem je, že pouze v případě, že je vybrána poslední možnost, nové si brožuru o nahrání, se zobrazí ovládací prvek FileUpload. K tomu vytvořit obslužná rutina události RadioButtonList s `SelectedIndexChanged` událostí a přidejte následující kód:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample7.vb)]

Protože RadioButtonList a odesílání souborů při odpovědích ovládací prvky jsou v rámci šablony, musíme psát hodně kódu, které jim programově zpřístupňují tyto ovládací prvky. `SelectedIndexChanged` Obslužná rutina události je předána odkazem RadioButtonList v `sender` vstupního parametru. Ovládací prvek FileUpload získáte musíme získat RadioButtonList s nadřazený ovládací prvek a použijte `FindControl("controlID")` metoda z něj. Jakmile budeme mít odkaz na RadioButtonList i FileUpload ovládací prvky, odesílání souborů při odpovědích ovládací prvek s `Visible` je nastavena na `True` pouze tehdy, pokud RadioButtonList s `SelectedValue` rovná 3, který je `Value` pro si brožuru o nové nahrávání `ListItem`.

S tímto kódem na místě využijte k otestování rozhraní úprav. Klikněte na tlačítko Upravit pro řádek. Zpočátku je třeba vybrat možnosti použít aktuální si brožuru o. Změna vybraného indexu vyvolá zpětné volání. Pokud třetí možnost je vybrána, se zobrazí ovládací prvek FileUpload, v opačném případě je skrytá. Obrázek 14 zobrazuje rozhraní pro úpravy, když nejdřív po kliknutí na tlačítko Upravit; Obrázek 15 ukazuje rozhraní po výběru nové možnosti si brožuru o nahrání.


[![Na začátku použít aktuální brožura, který je vybraná možnost](updating-and-deleting-existing-binary-data-vb/_static/image35.png)](updating-and-deleting-existing-binary-data-vb/_static/image34.png)

**Obrázek 14**: původně, použijte aktuální brožura je vybrána možnost ([kliknutím ji zobrazíte obrázek v plné velikosti](updating-and-deleting-existing-binary-data-vb/_static/image36.png))


[![Výběr nahrání nového brožura zobrazí možnost FileUpload ovládacího prvku](updating-and-deleting-existing-binary-data-vb/_static/image38.png)](updating-and-deleting-existing-binary-data-vb/_static/image37.png)

**Obrázek 15**: výběr nové brožura nahrávání zobrazí možnost řízení FileUpload ([kliknutím ji zobrazíte obrázek v plné velikosti](updating-and-deleting-existing-binary-data-vb/_static/image39.png))


## <a name="saving-the-brochure-file-and-updating-thebrochurepathcolumn"></a>Brožura uložení souboru a aktualizace`BrochurePath`sloupec

Po kliknutí na tlačítko Aktualizovat prvek GridView s jeho `RowUpdating` dojde k aktivaci události. Prvek ObjectDataSource vyvolání příkazu update s a pak GridView s `RowUpdated` dojde k aktivaci události. Stejně jako s odstraňování pracovního postupu, potřebujeme vytvořit obslužné rutiny událostí pro obě tyto události. V `RowUpdating` obslužná rutina události, musíme určit, jaká akce má být podle `SelectedValue` z `BrochureOptions` RadioButtonList:

- Pokud `SelectedValue` 1, chcete dál používat stejné `BrochurePath` nastavení. Proto musíme nastavit ObjectDataSource s `brochurePath` parametr ke stávající `BrochurePath` hodnotu záznamu aktualizuje. Prvek ObjectDataSource s `brochurePath` parametr lze nastavit pomocí `e.NewValues["brochurePath"] = value`.
- Pokud `SelectedValue` je 2, pak chceme nastavit záznam s `BrochurePath` hodnota, která se `NULL`. To lze provést nastavením prvku ObjectDataSource s `brochurePath` parametr `Nothing`, povede k databázi `NULL` používán `UPDATE` příkazu. Pokud je existující si brožuru o soubor, který se odebírá, musíme odstranit existující soubor. Nicméně pouze chcete to provést, pokud aktualizace se dokončí bez vyvolání výjimky.
- Pokud `SelectedValue` je 3, a my chceme zajistit, že má uživatel nahrál soubor PDF a uložte ho do systému souborů a aktualizovat record s `BrochurePath` hodnota ve sloupci. Kromě toho pokud existující si brožuru o soubor, který se nahrazuje, musíme odstranit předchozí soubor. Nicméně pouze chcete to provést, pokud aktualizace se dokončí bez vyvolání výjimky.

Kroky potřebné k dokončení při RadioButtonList s `SelectedValue` je 3 jsou prakticky stejné jako používané DetailsView s `ItemInserting` obslužné rutiny události. Tato obslužná rutina události se spustí při přidání nového záznamu kategorie z ovládacího prvku DetailsView jsme přidali [předchozí kurz o službě](including-a-file-upload-option-when-adding-a-new-record-vb.md). Proto behooves nám Refaktorovat této funkce si do samostatných metod. Konkrétně přestěhoval jsem si běžné funkce do dvou metod:

- `ProcessBrochureUpload(FileUpload, out bool)` přijímá jako vstup instanci FileUpload ovládacího prvku a výstup logická hodnota, která určuje, zda by mělo pokračovat operace odstranění nebo úpravy nebo pokud by měla být zrušena kvůli chybě ověřování. Tato metoda vrátí cestu k souboru uloženého nebo `null` Pokud byl uložen žádný soubor.
- `DeleteRememberedBrochurePath` Odstraní soubor určený podle cesty do proměnné stránky `deletedCategorysPdfPath` Pokud `deletedCategorysPdfPath` není `null`.

Následující kód pro tyto dvě metody. Všimněte si, podobnosti `ProcessBrochureUpload` a DetailsView s `ItemInserting` obslužné rutiny události z předchozí kurz o službě. V tomto kurzu jste aktualizoval(a) jsem prvek DetailsView s obslužných rutin událostí k použít tyto nové metody. Stáhněte si kód spojený s tímto kurzem, chcete-li zobrazit změny obslužné rutiny událostí s prvku DetailsView.


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample8.vb)]

GridView s `RowUpdating` a `RowUpdated` obslužné rutiny události použít `ProcessBrochureUpload` a `DeleteRememberedBrochurePath` metod, jak ukazuje následující kód:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample9.vb)]

Poznámka: Jak `RowUpdating` obslužná rutina události používá řadu podmíněné příkazy k provedení příslušné akce na základě `BrochureOptions` RadioButtonList s `SelectedValue` hodnotu vlastnosti.

S tímto kódem na místě můžete upravit kategorie a jeho použít své aktuální brožura, použít žádné si brožuru o nebo odešlete nový. Neváhejte a vyzkoušejte si to. Nastavte zarážky v `RowUpdating` a `RowUpdated` obslužné rutiny událostí, získali povědomí pracovního postupu.

## <a name="step-7-uploading-a-new-picture"></a>Krok 7: Nahrává se nový obrázek

`Picture` ImageField s úpravy rozhraní vykresluje jako textové pole s vyplněny hodnotou z jeho `DataImageUrlField` vlastnost. Během úprav pracovního postupu, prvku GridView předá parametr s názvem parametru ObjectDataSource hodnota třídy ImageField s `DataImageUrlField` vlastností a parametrů s hodnotu hodnoty zadané do textového pole v rozhraní pro úpravy. Toto chování je vhodný, pokud je na obrázku uložen jako soubor v systému souborů a `DataImageUrlField` obsahuje úplnou adresu URL obrázku. Úpravy rozhraní se k těmto okolnostem zobrazí adresa URL obrázku s do textového pole, které uživatel může změnit a uložit zpět do databáze. Udělena, tato výchozí rozhraní není t povolit uživatele s nahráním nové image, ale je mohly změnit adresu URL obrázku z aktuální hodnoty. Pro účely tohoto kurzu však třídy ImageField s nastaven jako výchozí rozhraní není stačit protože `Picture` binární data ukládají přímo v databázi a `DataImageUrlField` vlastnost obsahuje jenom `CategoryID`.

Chcete-li lépe pochopit, co se stane v našem kurzu, když uživatel upraví řádek s ImageField, zvažte následující příklad: uživatel upraví řádek s `CategoryID` 10, což způsobí `Picture` ImageField k vykreslení jako textové pole s hodnotou 10. Představte si, že uživatel změní hodnotu do tohoto textového pole na 50 a klikne na tlačítko Aktualizovat. Vyvolá zpětné volání a prvku GridView původně vytvoří parametr s názvem `CategoryID` s hodnotou 50. Ale předtím, než odešle tento parametr prvku GridView (a `CategoryName` a `Description` parametry), přidá hodnoty z `DataKeys` kolekce. Proto se přepíše `CategoryID` parametr s aktuální řádek s podkladovým `CategoryID` hodnota 10. Stručně řečeno, s ImageField úpravy rozhraní nemá žádný vliv na úpravu pracovního postupu pro účely tohoto kurzu protože názvy ImageField s `DataImageUrlField` vlastnost a mřížka s `DataKey` hodnota jsou ve stejné.

Zatímco ImageField usnadňuje zobrazení image založenou na datech z databáze, jsme zadávat t chcete poskytnout textové pole v rozhraní pro úpravy. Místo toho chcete nabízet FileUpload ovládací prvek, který koncový uživatel použít pro změnu obrázku s kategorií. Na rozdíl od `BrochurePath` hodnoty pro tyto kurzy jsme ve se rozhodli, že obrázek musí mít pro každou kategorii vyžadují. Proto jsme nejsou potřeba t, umožníte uživateli označit, že neexistuje žádný obrázek přidružený uživatel může buď nahrát nový obrázek nebo ponechte aktuální obrázek jako-je.

Přizpůsobení rozhraní třídy ImageField s úprav, musíme ho převést na pole TemplateField. Z inteligentních značek GridView s kliknutím na odkaz Upravit sloupce, vyberte třídy ImageField a klikněte na odkaz TemplateField převést toto pole.


![Převést ImageField TemplateField](updating-and-deleting-existing-binary-data-vb/_static/image3.gif)

**Obrázek 16**: ImageField převést na pole TemplateField


Převádění ImageField TemplateField tímto způsobem generuje TemplateField s dvě šablony. Jak ukazuje následující deklarativní syntaxe `ItemTemplate` webovému rozhraní Image obsahuje ovládací prvek, jehož `ImageUrl` je vlastnosti přiřazen pomocí syntaxe vázání dat podle třídy ImageField s `DataImageUrlField` a `DataImageUrlFormatString` vlastnosti. `EditItemTemplate` Obsahuje textové pole jejichž `Text` vlastnost je vázána na hodnotu zadanou proměnnou `DataImageUrlField` vlastnost.


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample10.aspx)]

Potřebujeme aktualizovat `EditItemTemplate` použití ovládacího prvku FileUpload. Z prvku GridView s inteligentním klikněte na Upravit šablony propojení a potom vyberte `Picture` TemplateField s `EditItemTemplate` z rozevíracího seznamu. V šabloně byste měli vidět textové pole, toto odeberte. V dalším kroku přetáhněte FileUpload ovládacího prvku z panelu nástrojů do šablony, nastavení jeho `ID` k `PictureUpload`. Také přidáte text pro změnu obrázku kategorie s zadejte nový obrázek. Pokud chcete zachovat obrázku kategorie s stejné, ponechte toto pole prázdné šablony, stejně.


[![Přidejte ovládací prvek odesílání souborů při odpovědích EditItemTemplate](updating-and-deleting-existing-binary-data-vb/_static/image41.png)](updating-and-deleting-existing-binary-data-vb/_static/image40.png)

**Obrázek 17**: Přidání ovládacího prvku odesílání souborů při odpovědích `EditItemTemplate` ([kliknutím ji zobrazíte obrázek v plné velikosti](updating-and-deleting-existing-binary-data-vb/_static/image42.png))


Po přizpůsobení rozhraní pro úpravy, můžete zobrazte průběh v prohlížeči. Při prohlížení řádek v režimu jen pro čtení, obrázek s kategorií se zobrazí jako před ale kliknutím na tlačítko Upravit jako text s ovládacím prvkem FileUpload vykreslí obrázek sloupce.


[![Úpravy rozhraní zahrnuje FileUpload ovládací prvek](updating-and-deleting-existing-binary-data-vb/_static/image44.png)](updating-and-deleting-existing-binary-data-vb/_static/image43.png)

**Obrázek 18**: úpravy rozhraní obsahuje ovládací prvek FileUpload ([kliknutím ji zobrazíte obrázek v plné velikosti](updating-and-deleting-existing-binary-data-vb/_static/image45.png))


Připomínáme, že prvku ObjectDataSource je nakonfigurován k volání `CategoriesBLL` třída s `UpdateCategory` metody, která přijímá jako vstup pro obrázek jako binární data `Byte` pole. Pokud je toto pole `Nothing`, ale alternativní `UpdateCategory` přetížená metoda je volána, problémy, které `UPDATE` příkazu SQL, který neprovádí úpravy `Picture` sloupce, a tím byste museli opustit kategorie s aktuální obrázek Internet. Proto v prvku GridView s `RowUpdating` budeme potřebovat programově odkazovat obslužná rutina události `PictureUpload` FileUpload řídit a určit, pokud byl nahrán do souboru. Pokud jeden nebyl odeslán, pak provedeme *není* chcete zadat hodnotu `picture` parametru. Na druhou stranu, pokud soubor byl nahrán do `PictureUpload` FileUpload ovládacího prvku, chceme se ujistit, že se jedná o soubor .jpg. Pokud je pak pošleme její binární obsah na ObjectDataSource prostřednictvím `picture` parametru.

Jako s kód použitý v kroku 6 velkou část kódu potřeba zde již existuje v prvku DetailsView s `ItemInserting` obslužné rutiny události. Proto jsem ve Refaktorovat běžné funkce do nové metody, `ValidPictureUpload`a aktualizovat `ItemInserting` obslužná rutina události, aby používali tuto metodu.

Přidejte následující kód na začátek ovládacího prvku GridView s `RowUpdating` obslužné rutiny události. Je důležité, že tento kód předcházet kód, který uloží si brožuru o soubor, protože jsme zadávat t chcete uložit brožura do systému souborů webového serveru s, pokud se odešle soubor neplatný obrázek.


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample11.vb)]

`ValidPictureUpload(FileUpload)` Metoda přijímá FileUpload ovládací prvek jako svůj jediný vstupní parametr a zkontroluje nahraný soubor s příponou tak, aby byl odeslaný soubor JPG, je volána pouze pokud je soubor s obrázkem. Pokud žádný soubor je odeslán, pak parametr obrázku je Nenastaveno a je proto používá jeho výchozí hodnotu `Nothing`. Pokud byl obrázek nahrán a `ValidPictureUpload` vrátí `True`, `picture` parametr je přiřazený binární data z této odeslané image; Pokud metoda vrátí `False`, pracovního postupu aktualizace se zrušila a byla ukončena, obslužné rutiny události.

`ValidPictureUpload(FileUpload)` Metoda kód, který byl Refaktorovat z prvku DetailsView s `ItemInserting` následuje obslužné rutiny události:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample12.vb)]

## <a name="step-8-replacing-the-original-categories-pictures-with-jpgs"></a>Krok 8: Nahrazení původní obrázky kategorie formátu JPG využívá

Připomínáme, že původní obrázky osm kategorií jsou zabaleny v hlavičce OLE rastrové soubory. Teď, když jsme přidali možnost upravit stávající záznam s obrázek, věnujte chvíli nahraďte formátu JPG využívá tyto rastrových obrázků. Pokud chcete pokračovat v používání aktuální kategorii obrázky, můžete je převést na formátu JPG využívá provedením následujících kroků:

1. Bitmapové obrázky uložte na pevném disku. Přejděte `UpdatingAndDeleting.aspx` stránky v prohlížeči a pro každou z prvních osm kategorií, klikněte pravým tlačítkem na bitovou kopii a zvolit možnost uložení obrázku.
2. Otevřete obrázek v editoru obrázků podle výběru. Například můžete použít Microsoft Paint.
3. Uložte jako obrázek JPG rastrového obrázku.
4. Aktualizujte kategorie s obrázek prostřednictvím úprav rozhraní pomocí souboru JPG.

Po úpravě kategorii a nahrávání obrázků JPG, image nebude vykreslovat v prohlížeči vzhledem k tomu, `DisplayCategoryPicture.aspx` stránky je odstranění první 78 bajtů z obrázků prvních osm kategorií. Opravte tak, že odeberete kód, který provede odstranění záhlaví OLE. Za to, `DisplayCategoryPicture.aspx``Page_Load` obslužné rutiny události by měly mít pouze následující kód:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample13.vb)]

> [!NOTE]
> `UpdatingAndDeleting.aspx` Stránky s vkládání a úpravy rozhraní použít trochu více práce. `CategoryName` a `Description` BoundFields v prvku DetailsView a ovládacího prvku GridView, by měly být převedeny vlastností TemplateField. Protože `CategoryName` neumožňuje `NULL` hodnoty, by měly být přidány RequiredFieldValidator. A `Description` TextBox pravděpodobně by měly být převedeny ve víceřádkovém textovém poli. Opuštění tyto doladění jako cvičení za vás.


## <a name="summary"></a>Souhrn

V tomto kurzu dokončíte naše Přehled práce s binárními daty. V tomto kurzu a předchozí tři jsme viděli, jak binární data mohou být uloženy v systému souborů nebo přímo v databázi. Uživatel zadá binární data do systému, že vyberete soubor z jejich pevného disku a pak ho nahrát na webový server, kde ho můžete uložit v systému souborů nebo vložena do databáze. Technologie ASP.NET 2.0 obsahuje FileUpload ovládací prvek, který umožňuje poskytování takového rozhraní stejně snadné jako přetažení. Nicméně, jako jsou uvedené v [nahrávání souborů](uploading-files-vb.md) kurzu FileUpload ovládací prvek je pouze velmi vhodná pro poměrně malý soubor nahrávání, v ideálním případě nejvýše jeden. Jsme také prozkoumat, jak přidružit nahraných dat v základní datový model, jakož i jak upravit a odstranit binární data z existujících záznamů.

Naše sada další kurzy vám umožní prozkoumat různých technik vytváření ukládání do mezipaměti. Ukládání do mezipaměti poskytuje prostředky ke zlepšení aplikace s celkový výkon tak, že výsledky z nákladné operace ukládání do umístění, které je přístupná rychleji.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla Teresy Murphy. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](including-a-file-upload-option-when-adding-a-new-record-vb.md)
