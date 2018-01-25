---
uid: web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
title: "Aktualizace a odstranění existující binární Data (VB) | Microsoft Docs"
author: rick-anderson
description: "V dřívějších kurzech jsme viděli, jak prvek GridView můžete snadno upravovat a odstraňovat textová data. V tomto kurzu vidíte, jak způsobují prvek GridView..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 3a052ced-9cf5-47b8-a400-934f0b687c26
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 8baf187d484424aeaee57f8c57ac391a0ae9e946
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="updating-and-deleting-existing-binary-data-vb"></a>Aktualizace a odstranění existující binární Data (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_57_VB.exe) nebo [stáhnout PDF](updating-and-deleting-existing-binary-data-vb/_static/datatutorial57vb1.pdf)

> V dřívějších kurzech jsme viděli, jak prvek GridView můžete snadno upravovat a odstraňovat textová data. V tomto kurzu vidíte, jak prvek GridView také umožňuje upravovat a odstraňovat binárních dat, zda tato binární data v databázi neukládá v systému souborů.


## <a name="introduction"></a>Úvod

V posledních tří kurzy jsme jste přidali s bit funkce pro práci s binární data. Spuštění přidáním `BrochurePath` sloupec, který se `Categories` tabulky a příslušným způsobem aktualizuje architekturu. Jsme přidali i Data Access Layer a vrstvu obchodní logiky metod pro práci s existující tabulku s kategorií `Picture` sloupec, který obsahuje binární obsah s souboru bitové kopie. Vyvinuli jsme web pages představovat binární data v GridView odkaz ke stažení pro – příručka, kategorie s obrázek ukazuje `<img>` elementu a přidali DetailsView, aby uživatelé mohli přidat novou kategorii a nahrajte data – příručka a obrázek.

K implementaci už jen zbývá možnost upravit a odstranit existující kategorie, které jsme budete provést v tomto kurzu pomocí GridView s integrovanou úprav a odstraňování funkce. Při úpravě kategorii, bude uživatel moct volitelně nahrát nový obrázek nebo mají kategorii nadále používat existující. Pro – příručka že můžete buď zvolit existující – příručka, nahrát nový – příručka nebo udávají, že kategorii už má – příručka s ním spojená. Umožňují s začít!

## <a name="step-1-updating-the-data-access-layer"></a>Krok 1: Aktualizace Data Access Layer

DAL má automaticky generovaný `Insert`, `Update`, a `Delete` metody, ale tyto metody byly vygenerovány na základě `CategoriesTableAdapter` s hlavním dotazu, který neobsahuje `Picture` sloupce. Proto `Insert` a `Update` metody tak nebudou obsahovat parametry pro zadání binární data pro obrázek kategorie s. Jako jsme provedli [předchozí kurzu](including-a-file-upload-option-when-adding-a-new-record-vb.md), potřebujeme k vytvoření nové metody TableAdapter pro aktualizaci `Categories` tabulky při zadávání binární data.

Otevřete typové datové sady a z návrháře, klikněte pravým tlačítkem na `CategoriesTableAdapter` s záhlaví a Průvodce konfigurací dotazu TableAdapter vyberte Přidat dotazu z místní nabídky k launche. Tento průvodce se spustí tím, že požádá nám, jak by měla dotazu TableAdapter přistupovat k databázi. Zvolte příkazy SQL použít a klikněte na tlačítko Další. Dalším krokem vyzve k zadání typu dotazu má být vygenerován. Od jsme re vytváření dotazu na přidejte nový záznam na `Categories` tabulky, vyberte aktualizaci a klikněte na tlačítko Další.


[![Vyberte možnosti aktualizace](updating-and-deleting-existing-binary-data-vb/_static/image2.png)](updating-and-deleting-existing-binary-data-vb/_static/image1.png)

**Obrázek 1**: Vyberte možnost aktualizace ([Kliknutím zobrazit obrázek v plné velikosti](updating-and-deleting-existing-binary-data-vb/_static/image3.png))


Teď je potřeba zadat `UPDATE` příkaz jazyka SQL. Průvodce automaticky navrhne `UPDATE` příkaz odpovídající hlavní dotazu TableAdapter s (ten, který aktualizuje `CategoryName`, `Description`, a `BrochurePath` hodnoty). Změňte příkaz tak, aby `Picture` sloupec se dodává spolu s `@Picture` parametr, například takto:


[!code-sql[Main](updating-and-deleting-existing-binary-data-vb/samples/sample1.sql)]

Poslední obrazovka průvodce požádá nám název nová metoda TableAdapter. Zadejte `UpdateWithPicture` a klikněte na tlačítko Dokončit.


[![Název nové UpdateWithPicture TableAdapter – metoda](updating-and-deleting-existing-binary-data-vb/_static/image5.png)](updating-and-deleting-existing-binary-data-vb/_static/image4.png)

**Obrázek 2**: název nová metoda TableAdapter `UpdateWithPicture` ([Kliknutím zobrazit obrázek v plné velikosti](updating-and-deleting-existing-binary-data-vb/_static/image6.png))


## <a name="step-2-adding-the-business-logic-layer-methods"></a>Krok 2: Přidání metody vrstvu obchodní logiky

Kromě aktualizace DAL, musíme aktualizovat BLL zahrnout metody pro aktualizace a odstranění kategorii. Toto jsou metody, které bude volána z prezentační vrstvy.

Pro odstranění kategorii, můžeme použít `CategoriesTableAdapter` s automaticky generovaný `Delete` metoda. Přidejte následující metodu do `CategoriesBLL` třídy:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample2.vb)]

V tomto kurzu umožňují s vytvořit dvě metody pro aktualizaci kategorie – jednu, která očekává data binární obrázku a vyvolá `UpdateWithPicture` metoda, kterou jsme právě přidali `CategoriesTableAdapter` a druhý, který přijímá jenom na `CategoryName`, `Description`a `BrochurePath`hodnoty a používá `CategoriesTableAdapter` třídy s automaticky generovaný `Update` příkaz. Důvody pomocí dvou metod je, že za určitých okolností se uživatel chtít aktualizovat obrázku s kategorie společně s jeho další pole, ve kterých bude mít případ uživatel nahrát nový obrázek. Binární data nahraný obrázek s použitím pak v `UPDATE` příkaz. V jiných případech může uživatel zájmu pouze v aktualizaci, Řekněme, název a popis. Avšak v tom případě `UPDATE` příkaz očekává binární data pro `Picture` také sloupec a potom jsme d musí definovat také tyto informace. To by znamenalo další cesty k databázi a navrácení data obrázku pro upravovaný záznam. Proto, že má být dva `UPDATE` metody. Určuje vrstvu obchodní logiky používané založené na tom, jestli se data obrázku poskytuje při aktualizaci kategorie.

K provedení této přidat dvě metody, které `CategoriesBLL` třídy, i s názvem `UpdateCategory`. První z nich by měl přijmout tři `String` s, `Byte` pole a `Integer` jako vstupní parametry; druhý, právě tři `String` s a `Integer`. `String` Jsou vstupní parametry pro název kategorie s, popis a cesta k souboru – Příručka `Byte` pole je pro binární obsah na obrázku kategorie s a `Integer` identifikuje `CategoryID` záznamu aktualizovat. Všimněte si, že první přetížení vyvolá druhý pokud předané `Byte` pole je `Nothing`:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample3.vb)]

## <a name="step-3-copying-over-the-insert-and-view-functionality"></a>Krok 3: Překopírování vložení a zobrazení funkce

V [předchozí kurzu](including-a-file-upload-option-when-adding-a-new-record-vb.md) jsme vytvořili stránku s názvem `UploadInDetailsView.aspx` , uvedené všechny kategorie v GridView a poskytuje DetailsView pro přidání nové kategorie do systému. V tomto kurzu rozšiřujeme GridView úpravy a odstraňování podpory. Místo pracujete z `UploadInDetailsView.aspx`, umožňují s místo umístit tento kurz s změny v `UpdatingAndDeleting.aspx` stránky ze stejné složky, `~/BinaryData`. Zkopírujte a vložte kód a kód z `UploadInDetailsView.aspx` k `UpdatingAndDeleting.aspx`.

Začněte otevřením `UploadInDetailsView.aspx` stránky. Zkopírujte všechny deklarativní syntaxi v rámci `<asp:Content>` elementu, jak je znázorněno na obrázku 3. Dále otevřete `UpdatingAndDeleting.aspx` a vložte tento kód v rámci jeho `<asp:Content>` elementu. Podobně, zkopírovat kód z `UploadInDetailsView.aspx` třídu s kódem v pozadí na stránce `UpdatingAndDeleting.aspx`.


[![Zkopírujte deklarativní z UploadInDetailsView.aspx](updating-and-deleting-existing-binary-data-vb/_static/image8.png)](updating-and-deleting-existing-binary-data-vb/_static/image7.png)

**Obrázek 3**: kopírování deklarativní z `UploadInDetailsView.aspx` ([Kliknutím zobrazit obrázek v plné velikosti](updating-and-deleting-existing-binary-data-vb/_static/image9.png))


Po zkopírování přes deklarativní značek a kódu, navštivte `UpdatingAndDeleting.aspx`. Měli byste vidět stejný výstup a mít stejné prostředí pro uživatele stejně jako u `UploadInDetailsView.aspx` stránky z předchozí kurzu.

## <a name="step-4-adding-deleting-support-to-the-objectdatasource-and-gridview"></a>Krok 4: Přidávání, odstraňování podporu, aby ObjectDataSource a GridView

Jak již bylo zmíněno zpět v [přehled o vložení, aktualizace a odstranění dat](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) kurzu GridView nabízí integrované funkce odstraňování a tyto možnosti se dají povolit v značek zaškrtávací políčko, pokud základní mřížky s zdroj dat podporuje odstraňování. Aktuálně ObjectDataSource GridView je vázána (`CategoriesDataSource`) nepodporuje odstraňování.

Chcete-li to opravit, klikněte na možnost konfigurace zdroje dat z ObjectDataSource s inteligentním spusťte průvodce. První obrazovka ukazuje, že je ObjectDataSource nakonfigurováno pro práci s `CategoriesBLL` třídy. Stiskněte tlačítko Další. V současné době pouze ObjectDataSource s `InsertMethod` a `SelectMethod` jsou zadané vlastnosti. Ale průvodce automaticky vyplněna rozevírací seznamy na kartách aktualizace a odstranění se `UpdateCategory` a `DeleteCategory` metody, v uvedeném pořadí. Důvodem je, že v `CategoriesBLL` třídy jsme označených těchto metod, pomocí `DataObjectMethodAttribute` jako výchozí metody pro aktualizace a odstranění.

Teď, nastavte aktualizace karta s rozevíracím seznamu (None), ale nechte Odstranění karta s rozevíracím seznamu nastavte na `DeleteCategory`. Se vrátí do tohoto průvodce v kroku 6 pro přidání podpory aktualizací.


[![Konfigurace ObjectDataSource lze pomocí této metody DeleteCategory](updating-and-deleting-existing-binary-data-vb/_static/image11.png)](updating-and-deleting-existing-binary-data-vb/_static/image10.png)

**Obrázek 4**: Konfigurace ObjectDataSource pro použití `DeleteCategory` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](updating-and-deleting-existing-binary-data-vb/_static/image12.png))


> [!NOTE]
> Po dokončení průvodce, Visual Studio požádat, pokud chcete aktualizovat pole a klíče, který se znova vygeneruje data webové ovládací prvky pole. Ne, vybrat, protože kliknutím na tlačítko Ano, přepíše se tam pole vlastního nastavení provedené.


ObjectDataSource bude teď obsahovat hodnotu pro jeho `DeleteMethod` vlastnost a také `DeleteParameter`. Odvolat, že při použití průvodce k určení metody, nastaví Visual Studio ObjectDataSource s `OldValuesParameterFormatString` vlastnost `original_{0}`, což způsobuje problémy s aktualizace a odstranění volání metod. Proto buď úplně vymažte tuto vlastnost nebo ho resetovat na výchozí hodnoty `{0}`. Pokud bude nutné aktualizovat vaše paměti na tuto vlastnost ObjectDataSource, přečtěte si téma [přehled o vložení, aktualizace a odstranění dat](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) kurzu.

Po dokončení průvodce a opravit `OldValuesParameterFormatString`, ObjectDataSource s deklarativní by měl vypadat takto:


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample4.aspx)]

Po dokončení konfigurace ObjectDataSource, přidejte do GridView zaškrtnutím políčka Povolit odstranění z GridView s inteligentním odstraňování možnosti. Tím se přidá CommandField GridView jejichž `ShowDeleteButton` je nastavena na `True`.


[![Povolit podporu pro odstranění v prvku GridView.](updating-and-deleting-existing-binary-data-vb/_static/image14.png)](updating-and-deleting-existing-binary-data-vb/_static/image13.png)

**Obrázek 5**: Povolení podpory pro odstranění v GridView ([Kliknutím zobrazit obrázek v plné velikosti](updating-and-deleting-existing-binary-data-vb/_static/image15.png))


Chcete-li otestovat funkci odstranění chvíli trvat. Je cizího klíče mezi `Products` tabulky s `CategoryID` a `Categories` tabulky s `CategoryID`, takže budete mít k výjimce porušení omezení pro cizí klíč pokoušíte se odstranit některé z prvních 8 kategorií. Tuto funkci se přidáte novou kategorii, poskytuje – příručka i obrázek. Kategorie Mé test, vidíte na obrázku 6, zahrnuje – příručka testovací soubor s názvem `Test.pdf` a zkušební obrázek. Obrázek 7 znázorňuje GridView po přidání testovací kategorie.


[![Přidat kategorii Test s – příručka a bitové kopie](updating-and-deleting-existing-binary-data-vb/_static/image17.png)](updating-and-deleting-existing-binary-data-vb/_static/image16.png)

**Obrázek 6**: Přidat kategorii Test s – příručka a bitové kopie ([Kliknutím zobrazit obrázek v plné velikosti](updating-and-deleting-existing-binary-data-vb/_static/image18.png))


[![Po vložení kategorie testů, se zobrazí v prvku GridView.](updating-and-deleting-existing-binary-data-vb/_static/image20.png)](updating-and-deleting-existing-binary-data-vb/_static/image19.png)

**Obrázek 7**: Po vložení kategorie testů, se zobrazí v GridView ([Kliknutím zobrazit obrázek v plné velikosti](updating-and-deleting-existing-binary-data-vb/_static/image21.png))


V sadě Visual Studio aktualizujte Průzkumníku řešení. Teď byste měli vidět nový soubor v `~/Brochures` složky, `Test.pdf` (viz obrázek 8).

Potom kliknutím na odkaz odstranit v řádku testovací kategorie způsobuje stránce odeslání a `CategoriesBLL` třídu s `DeleteCategory` metoda má provést. To bude vyvolán DAL s `Delete` metoda, způsobuje odpovídající `DELETE` příkaz k odeslání do databáze. Data se pak odrážejí na GridView a kód budou odeslána zpět do klienta se už testovací kategorie.

Při odstranění pracovního postupu byla úspěšně odebrána záznam kategorie testů z `Categories` tabulce neodstranil jeho – příručka souboru ze systému souborů webového serveru s. Aktualizovat Průzkumníka řešení a zobrazí se `Test.pdf` je stále uložený ve `~/Brochures` složky.


![Test.pdf soubor odstraněn, není ze systému souborů webového serveru s](updating-and-deleting-existing-binary-data-vb/_static/image1.gif)

**Obrázek 8**: `Test.pdf` soubor nebyl odstraněn ze systému souborů webového serveru s


## <a name="step-5-removing-the-deleted-category-s-brochure-file"></a>Krok 5: Odebrání souboru odstraněného kategorie s – příručka

Jedním z downsides ukládání binární data externí do databáze je, že je třeba brát kroků navíc odstranit tyto soubory při odstranění záznamu přidružené databáze. Rutina GridView a ObjectDataSource poskytují události, které budou spuštěny před a po provedení příkaz delete. Ve skutečnosti potřebujeme vytváření obslužných rutin událostí pro události před a po akce. Před `Categories` odstranění záznamu je potřeba určit její cesta k souboru s PDF, ale nemůžeme nejsou zobrazeny t chcete odstranit PDF před odstraněním kategorii v případě, že je některé výjimky a kategorie se neodstraní.

Rutina GridView s [ `RowDeleting` událostí](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx) aktivuje před příkaz delete s ObjectDataSource byla vyvolána, při jeho [ `RowDeleted` událostí](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleted.aspx) aktivuje se po. Vytváření obslužných rutin událostí pro tyto dvě události pomocí následujícího kódu:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample5.vb)]

V `RowDeleting` obslužné rutiny události, `CategoryID` řádku odstraňuje bude převzat z GridView s `DataKeys` kolekce, který je přístupný v této obslužné rutiny události prostřednictvím `e.Keys` kolekce. Dále `CategoriesBLL` třídu s `GetCategoryByCategoryID(categoryID)` je vyvolána k vrácení informací o odstranění záznamu. Pokud vrácený `CategoriesDataRow` objekt má jinou hodnotu než`NULL``BrochurePath` hodnotu uložen v proměnné stránky `deletedCategorysPdfPath` tak, aby soubor můžete odstranit v `RowDeleted` obslužné rutiny události.

> [!NOTE]
> Místo načítání `BrochurePath` podrobnosti o `Categories` záznam odstraňuje v `RowDeleting` obslužné rutiny události, jsme může případně přidali `BrochurePath` k GridView s `DataKeyNames` vlastnost, ke kterým přistupují hodnota záznamu s prostřednictvím `e.Keys` kolekce. To by mírně zvyšte velikost stavu GridView s zobrazení, ale by snížení objemu kód potřebný a uložit cestě k databázi.


Po ObjectDataSource metoda byla volána s základní příkaz delete, rutina GridView s `RowDeleted` aktivuje obslužná rutina události. Pokud nebyly žádné výjimky v odstranění dat a je hodnota pro `deletedCategorysPdfPath`, PDF je odstraněn ze systému souborů. Všimněte si, že tento další kód není potřeba vyčistěte kategorie s binární data související s jeho obrázek. Tento s data obrázku je ukládána přímo v databázi, takže odstranění `Categories` řádek dojde také k odstranění této kategorie s obrázek data.

Po přidání obslužné rutiny dvou událostí, spusťte tuto testovací případ znovu. Při odstraňování kategorii, je taky odstranit její přidružené PDF.

Aktualizace stávající záznam s přidružené binární data poskytuje některé běžné problémy, zajímavé. Zbývající část tohoto kurzu obsahuje podrobné přidání možností aktualizace – příručka a obrázek. Krok 6 prozkoumá techniky pro aktualizaci informace – příručka během kroku 7 zjistí aktualizace na obrázku.

## <a name="step-6-updating-a-category-s-brochure"></a>Krok 6: Aktualizace informace o s kategorie

Jak je popsáno v tomto kurzu přehled o vložení, aktualizace a odstranění dat, GridView má integrovanou podporu úpravy úrovni řádků, který může být implementována značek zaškrtávací políčko, pokud je jeho základní zdroj dat správně nakonfigurovaný. V současné době `CategoriesDataSource` ObjectDataSource ještě není nakonfigurovaná zahrnout aktualizace podpory, takže umožňují s přidat v.

Klikněte na odkaz Konfigurace zdroje dat z ObjectDataSource s průvodce a pokračujte ve druhém kroku. Z důvodu `DataObjectMethodAttribute` použít v `CategoriesBLL`, rozevírací seznam aktualizace by měla být vyplní automaticky `UpdateCategory` přetížení, které přijímá čtyři vstupní parametry (pro všechny sloupce, ale `Picture`). Změňte tak, aby ho využívá přetížení s pěti parametry.


[![Konfigurace ObjectDataSource lze pomocí této metody UpdateCategory, která obsahuje parametr pro obrázek](updating-and-deleting-existing-binary-data-vb/_static/image23.png)](updating-and-deleting-existing-binary-data-vb/_static/image22.png)

**Obrázek 9**: Konfigurace ObjectDataSource pro použití `UpdateCategory` metoda, která obsahuje parametr pro `Picture` ([Kliknutím zobrazit obrázek v plné velikosti](updating-and-deleting-existing-binary-data-vb/_static/image24.png))


ObjectDataSource bude teď obsahovat hodnotu pro jeho `UpdateMethod` vlastnost, jakož i odpovídající `UpdateParameter` s. Jak jsme uvedli v kroku 4, Visual Studio nastaví ObjectDataSource s `OldValuesParameterFormatString` vlastnost `original_{0}` při použití Průvodce konfigurace zdroje dat. Tím způsobit problémy s aktualizace a odstranění volání metod. Proto buď úplně vymažte tuto vlastnost nebo ho resetovat na výchozí hodnoty `{0}`.

Po dokončení průvodce a opravit `OldValuesParameterFormatString`, ObjectDataSource s deklarativní by měl vypadat třeba takto:


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample6.aspx)]

Chcete-li na GridView s integrované úpravy funkce, zaškrtněte možnost Povolit úpravy z GridView s inteligentním. Tato hodnota bude nastavena CommandField s `ShowEditButton` vlastnost `True`, což kromě tlačítko Upravit (a aktualizace a zrušit tlačítka pro řádek upravovaný).


[![Rutina GridView k úpravám podporu konfigurace](updating-and-deleting-existing-binary-data-vb/_static/image26.png)](updating-and-deleting-existing-binary-data-vb/_static/image25.png)

**Obrázek 10**: Konfigurace GridView k úpravám podporu ([Kliknutím zobrazit obrázek v plné velikosti](updating-and-deleting-existing-binary-data-vb/_static/image27.png))


Navštívit stránku prostřednictvím prohlížeče a klikněte na jeden řádek s tlačítka Upravit. `CategoryName` a `Description` BoundFields se vykresluje jako textových polí. `BrochurePath` Chybí TemplateField `EditItemTemplate`, takže nadále zobrazit jeho `ItemTemplate` odkaz – příručka. `Picture` ImageField vykreslí jako textové pole, jehož `Text` vlastnost je přiřazena hodnota ImageField s `DataImageUrlField` hodnotu v tomto případě `CategoryID`.


[![GridView chybí úpravy rozhraní pro BrochurePath](updating-and-deleting-existing-binary-data-vb/_static/image29.png)](updating-and-deleting-existing-binary-data-vb/_static/image28.png)

**Obrázek 11**: GridView chybí rozhraní pro úpravy `BrochurePath` ([Kliknutím zobrazit obrázek v plné velikosti](updating-and-deleting-existing-binary-data-vb/_static/image30.png))


## <a name="customizing-thebrochurepaths-editing-interface"></a>Přizpůsobení`BrochurePath`s úpravy rozhraní

Je potřeba vytvořit rozhraní pro úpravy `BrochurePath` TemplateField, ten, který umožňuje uživatelům buď:

- Nechte – příručka kategorie s jako-se,
- Aktualizace – příručka kategorie s tím, že nahrajete nové – příručka, nebo
- Odeberte – příručka kategorie s úplně (v případě, že kategorie už má přidružené – příručka).

Také je potřeba aktualizovat `Picture` ImageField s úpravy rozhraní, ale nemůžeme budete získat k tomuto v kroku 7.

Z s GridView inteligentních značek, klikněte na odkaz Upravit šablony a vyberte `BrochurePath` TemplateField s `EditItemTemplate` z rozevíracího seznamu. Přidání ovládacího prvku RadioButtonList na tuto šablonu, nastavení jeho `ID` vlastnost `BrochureOptions` a jeho `AutoPostBack` vlastnost `True`. V okně vlastností klikněte na symbol tří teček v `Items` vlastnosti, která se otevře `ListItem` Editor kolekce. Přidejte následující tři možnosti s `Value` s 1, 2 a 3, v uvedeném pořadí:

- Použít aktuální – příručka
- Odebrat aktuální – příručka
- Nahrát nový – příručka

Nastavit první `ListItem` s `Selected` vlastnost `True`.


![Přidejte tři položky ListItems do RadioButtonList](updating-and-deleting-existing-binary-data-vb/_static/image2.gif)

**Obrázek 12**: Přidejte tři `ListItem` chcete RadioButtonList s


Pod RadioButtonList, přidání ovládacího prvku odesílání souborů při odpovědích s názvem `BrochureUpload`. Nastavte její `Visible` vlastnost `False`.


[![Přidat do EditItemTemplate RadioButtonList a odesílání souborů při odpovědích ovládací prvek](updating-and-deleting-existing-binary-data-vb/_static/image32.png)](updating-and-deleting-existing-binary-data-vb/_static/image31.png)

**Obrázek 13**: Přidání RadioButtonList a odesílání souborů při odpovědích řízení `EditItemTemplate` ([Kliknutím zobrazit obrázek v plné velikosti](updating-and-deleting-existing-binary-data-vb/_static/image33.png))


Tato RadioButtonList poskytuje tři možnosti pro uživatele. Cílem je, že ovládacího prvku odesílání souborů při odpovědích se zobrazí jenom v případě, že je vybrána poslední možnost, odeslání nové – příručka. K tomu, vytvoření obslužné rutiny události pro RadioButtonList s `SelectedIndexChanged` událostí a přidejte následující kód:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample7.vb)]

Vzhledem k tomu, že ovládací prvky RadioButtonList a odesílání souborů při odpovědích jsou v rámci šablon, máme zápisu bit kódu k programovému přístupu ke tyto ovládací prvky. `SelectedIndexChanged` Obslužné rutiny události je předán odkaz RadioButtonList v `sender` vstupní parametr. Ovládací prvek odesílání souborů při odpovědích získáte, je potřeba získat RadioButtonList s nadřazeného ovládacího prvku a použijte `FindControl("controlID")` metoda odtud. Jakmile jsme odkaz na RadioButtonList i odesílání souborů při odpovědích ovládací prvky, odesílání souborů při odpovědích řízení s `Visible` je nastavena na `True` pouze v případě RadioButtonList s `SelectedValue` rovná 3, která je `Value` pro nové – příručka nahrávání `ListItem`.

S tímto kódem na místě za chvíli k otestování úpravy rozhraní. Klikněte na tlačítko Upravit pro řádek. Na začátku je nutné vybrat možnosti použít aktuální – příručka. Změna vybraného indexu způsobí, že zpětné volání. Pokud tato třetí možnost je vybrána, se zobrazí ovládací prvek odesílání souborů při odpovědích, v opačném případě je skrytá. Obrázek 14 zobrazuje rozhraní pro úpravy, když nejdřív po kliknutí na tlačítko Upravit; Obrázek 15 vidíte rozhraní po je vybrána možnost – příručka nové odeslání.


[![Standardně použití aktuální – příručka, který je vybraná možnost](updating-and-deleting-existing-binary-data-vb/_static/image35.png)](updating-and-deleting-existing-binary-data-vb/_static/image34.png)

**Obrázek 14**: nejdřív použití aktuální – příručka je vybrána možnost ([Kliknutím zobrazit obrázek v plné velikosti](updating-and-deleting-existing-binary-data-vb/_static/image36.png))


[![Výběr odeslání nové – příručka zobrazí možnosti řízení odesílání souborů při odpovědích](updating-and-deleting-existing-binary-data-vb/_static/image38.png)](updating-and-deleting-existing-binary-data-vb/_static/image37.png)

**Obrázek 15**: výběr odeslání nové – příručka zobrazí možnosti řízení odesílání souborů při odpovědích ([Kliknutím zobrazit obrázek v plné velikosti](updating-and-deleting-existing-binary-data-vb/_static/image39.png))


## <a name="saving-the-brochure-file-and-updating-thebrochurepathcolumn"></a>Ukládání informace o souboru a aktualizace`BrochurePath`sloupce

Při kliknutí na tlačítko Aktualizovat GridView s jeho `RowUpdating` je aktivována událost. ObjectDataSource příkaz aktualizace s je volána a potom GridView s `RowUpdated` je aktivována událost. Jako s odstraněním pracovní postup, musíme vytváření obslužných rutin událostí pro obě tyto události. V `RowUpdating` obslužné rutiny události, musíme určit, jaká opatření se mají provést na základě `SelectedValue` z `BrochureOptions` RadioButtonList:

- Pokud `SelectedValue` je 1, chceme dál používat stejné `BrochurePath` nastavení. Proto je potřeba nastavit ObjectDataSource s `brochurePath` parametr ke stávající `BrochurePath` hodnota záznamu aktualizované. ObjectDataSource s `brochurePath` parametr se dá nastavit pomocí `e.NewValues["brochurePath"] = value`.
- Pokud `SelectedValue` je 2, pak chceme nastavit záznam s `BrochurePath` hodnotu `NULL`. To můžete udělat nastavením ObjectDataSource s `brochurePath` parametru `Nothing`, výsledkem databázi `NULL` použitá v `UPDATE` příkaz. Pokud je existující – příručka soubor, který má být odebrán, musíme odstraňte existující soubor. Ale chceme jenom to udělat, pokud se aktualizace dokončí bez vyvolání k výjimce.
- Pokud `SelectedValue` je 3, chceme, ujistěte se, že uživatel odeslal soubor PDF a uložte ho do systému souborů a aktualizovat záznam s `BrochurePath` hodnota sloupce. Navíc pokud je existující – příručka soubor, který se nahrazuje, je potřeba odstranit předchozí soubor. Ale chceme jenom to udělat, pokud se aktualizace dokončí bez vyvolání k výjimce.

Kroky potřebné při dokončení RadioButtonList s `SelectedValue` je 3 jsou téměř stejné jako ty používané DetailsView s `ItemInserting` obslužné rutiny události. Tuto obslužnou rutinu události se spustí při přidání nového záznamu kategorie z ovládacího prvku DetailsView jsme přidali v [předchozí kurzu](including-a-file-upload-option-when-adding-a-new-record-vb.md). Proto behooves nám Refaktorovat tato funkce se do samostatné metody. Konkrétně po přesunutí na běžné funkce do dvou metod:

- `ProcessBrochureUpload(FileUpload, out bool)`přijme jako vstup instanci ovládacího prvku odesílání souborů při odpovědích a výstup logickou hodnotu, která určuje, zda by měl odstranit nebo upravit operace pokračovat, nebo pokud by měl být zrušena z důvodu chyby ověření. Tato metoda vrací cestu k souboru uložené nebo `null` Pokud žádný soubor byl uložen.
- `DeleteRememberedBrochurePath`Odstraní soubor určený touto cestu, do proměnné stránky `deletedCategorysPdfPath` Pokud `deletedCategorysPdfPath` není `null`.

Následuje kód pro tyto dvě metody. Všimněte si podobnosti mezi `ProcessBrochureUpload` a DetailsView s `ItemInserting` obslužné rutiny události z předchozích kurzu. V tomto kurzu I jste aktualizovali DetailsView obslužné rutiny s událostí na tyto nové metody. Stáhněte si kód spojený s úpravy obslužné rutiny událostí s DetailsView najdete v tomto kurzu.


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample8.vb)]

Rutina GridView s `RowUpdating` a `RowUpdated` obslužných rutin událostí pomocí `ProcessBrochureUpload` a `DeleteRememberedBrochurePath` metod, jak ukazuje následující kód:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample9.vb)]

Poznámka: Jak `RowUpdating` obslužné rutiny události používá řadu podmíněné příkazy provádět příslušné akce na základě `BrochureOptions` RadioButtonList s `SelectedValue` hodnotu vlastnosti.

S tímto kódem na místě můžete upravit kategorii a mějte ho pomocí jeho aktuální – příručka, použijte žádné – příručka nebo nahrát nový. Pokračujte a vyzkoušejte ji. Nastavte zarážky v `RowUpdating` a `RowUpdated` obslužné rutiny událostí získáte přehled o pracovní postup.

## <a name="step-7-uploading-a-new-picture"></a>Krok 7: Odesílání nový obrázek

`Picture` Úpravy rozhraní vykreslí jako naplněný hodnotu z textové pole s ImageField jeho `DataImageUrlField` vlastnost. Během úprav pracovního postupu, GridView předá parametr ObjectDataSource s názvem parametru hodnota ImageField s `DataImageUrlField` vlastnost a parametr s hodnotou hodnota zadaná do textového pole v úpravy rozhraní. Toto chování je vhodné, když bitovou kopii je uloženo jako soubor v systému souborů a `DataImageUrlField` obsahuje úplnou adresu URL obrázku. Úpravy rozhraní s takových okolností zobrazí adresa URL obrázku s do textového pole, které uživatel může změnit a uložit zpět do databáze. Udělena, tato výchozí rozhraní nemá t povolit uživateli odeslat novou bitovou kopii, ale nechat, je změňte adresu URL obrázku z aktuální hodnotu do jiné. V tomto kurzu ale ImageField s nastaven jako výchozí rozhraní není postačovat protože `Picture` binární data ukládají přímo v databázi a `DataImageUrlField` vlastnost blokování jenom na `CategoryID`.

Chcete-li lépe pochopit, co se stane v našem kurzu, když uživatel upravuje řádek s ImageField, zvažte následující příklad: uživatel upravuje řádek s `CategoryID` 10, která způsobila `Picture` ImageField k vykreslení jako textové pole s hodnotou 10. Představte si, že uživatel změní hodnotu v tomuto textovému poli na 50 a kliknutím na tlačítko Aktualizovat. Probíhá zpětné volání a GridView původně vytvoří parametr s názvem `CategoryID` s hodnotou 50. Ale předtím, než GridView odešle tento parametr (a `CategoryName` a `Description` parametry), se přidá do hodnoty z `DataKeys` kolekce. Proto přepíše `CategoryID` parametr s aktuální řádek s základní `CategoryID` hodnotu 10. Stručně řečeno, s ImageField úpravy rozhraní nemá žádný vliv na úpravy pracovního postupu v tomto kurzu protože názvy ImageField s `DataImageUrlField` vlastnost a mřížky s `DataKey` hodnota jsou v stejné.

Když ImageField usnadňuje zobrazte obrázek na základě dat databáze, jsme nejsou zobrazeny chcete t zadejte do textového pole v rozhraní pro úpravy. Místo toho chceme nabídnout odesílání souborů při odpovědích ovládací prvek, který koncového uživatele můžete použít ke změně obrázku s kategorie. Na rozdíl od `BrochurePath` hodnoty pro tyto kurzy jsme jste se rozhodli nutné, aby každá kategorie musí obrázku. Proto jsme nejsou zobrazeny t nutné, aby mohl uživatel znamenat, že žádné přidružené obrázku uživatel může buď nahrávání nový obrázek nebo nechte aktuální obrázek jako-je.

Chcete-li přizpůsobit rozhraní ImageField s úpravy, musíme převádět je do TemplateField. Rutina GridView s inteligentní značky klikněte na odkaz Upravit sloupce, vyberte ImageField a klikněte na převést toto pole na TemplateField odkaz.


![Převést ImageField TemplateField](updating-and-deleting-existing-binary-data-vb/_static/image3.gif)

**Obrázek 16**: převést ImageField TemplateField


Převádění ImageField do TemplateField tímto způsobem generuje TemplateField s dvě šablony. Jak ukazuje následující deklarativní syntaxe, `ItemTemplate` obsahuje Image webového řídit, jehož `ImageUrl` vlastnost je přiřazena pomocí syntaxe vazby dat podle ImageField s `DataImageUrlField` a `DataImageUrlFormatString` vlastnosti. `EditItemTemplate` Obsahuje textové pole jehož `Text` vlastnost je vázána na hodnotu zadanou pomocí `DataImageUrlField` vlastnost.


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample10.aspx)]

Je potřeba aktualizovat `EditItemTemplate` používat odesílání souborů při odpovědích řízení. Odkaz z GridView s inteligentním kliknutím na Upravit šablony a potom vyberte `Picture` TemplateField s `EditItemTemplate` z rozevíracího seznamu. V šabloně, měli byste vidět odebrat toto textové pole. V dalším kroku přetáhněte ovládací prvek odesílání souborů při odpovědích z panelu nástrojů do šablony, nastavení jeho `ID` k `PictureUpload`. Také přidáte text, který chcete změnit obrázek, kategorie s, zadejte nový obrázek. Zachovat na obrázku kategorie s stejné, pole ponechte prázdné šablony také.


[![Přidání ovládacího prvku odesílání souborů při odpovědích na EditItemTemplate](updating-and-deleting-existing-binary-data-vb/_static/image41.png)](updating-and-deleting-existing-binary-data-vb/_static/image40.png)

**Obrázek 17**: Přidání ovládacího prvku odesílání souborů při odpovědích `EditItemTemplate` ([Kliknutím zobrazit obrázek v plné velikosti](updating-and-deleting-existing-binary-data-vb/_static/image42.png))


Po přizpůsobení rozhraní úpravy, zobrazte průběh v prohlížeči. Při zobrazení řádek v režimu jen pro čtení, bitovou kopii s kategorie, je zobrazena jako před, ale kliknutím na tlačítko Upravit vykreslí sloupci obrázek jako text s ovládacím prvkem odesílání souborů při odpovědích.


[![Úpravy rozhraní obsahuje ovládacího prvku odesílání souborů při odpovědích](updating-and-deleting-existing-binary-data-vb/_static/image44.png)](updating-and-deleting-existing-binary-data-vb/_static/image43.png)

**Obrázek 18**: rozhraní úpravy obsahuje ovládací prvek odesílání souborů při odpovědích ([Kliknutím zobrazit obrázek v plné velikosti](updating-and-deleting-existing-binary-data-vb/_static/image45.png))


Odvolat, aby ObjectDataSource je nakonfigurován k volání `CategoriesBLL` třídu s `UpdateCategory` metodu, která přijímá jako vstup pro obrázek jako binární data `Byte` pole. Pokud toto pole je `Nothing`, ale alternativní `UpdateCategory` přetížení je volána, problémy, které `UPDATE` příkaz jazyka SQL, který nelze změnit `Picture` sloupce, a tím ponechat kategorie s aktuální obrázek Internet nedotčené. Proto v GridView s `RowUpdating` obslužné rutiny události musíme programově odkazovat `PictureUpload` odesílání souborů při odpovědích řídit a určit, pokud byl nahrán do souboru. Pokud jeden nebyl odeslán, a poté proveďte jsme *není* chcete zadat hodnotu `picture` parametr. Na druhé straně, pokud byla v souboru `PictureUpload` odesílání souborů při odpovědích řízení, chceme zajistěte, aby byl soubor JPG. Pokud je, pak vám můžeme poslat binární obsah do ObjectDataSource prostřednictvím `picture` parametr.

Jako kód používá v kroku 6, většinu kódu potřeby zde již existuje v DetailsView s `ItemInserting` obslužné rutiny události. Proto I jste rozdělili běžné funkce do nové metody, `ValidPictureUpload`a aktualizovat `ItemInserting` obslužné rutiny události chcete použít tuto metodu.

Přidejte následující kód do začátku GridView s `RowUpdating` obslužné rutiny události. Je důležité, aby tento kód dřívější než kód, který uloží soubor – příručka vzhledem k tomu, že jsme nejsou zobrazeny t s Chcete uložit – příručka k systému souborů webového serveru s, pokud je soubor neplatný obrázek.


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample11.vb)]

`ValidPictureUpload(FileUpload)` Metoda přebírá v ovládacím prvku odesílání souborů při odpovědích jako svůj jediný vstupní parametr a kontroluje nahrávaný soubor s příponou pro zajištění nahrávaný soubor formát JPG; je volána pouze pokud je soubor s obrázkem. Pokud žádný soubor odešle, pak parametr obrázek není nastavený a proto používá jeho výchozí hodnotu `Nothing`. Pokud byl odeslán obrázku a `ValidPictureUpload` vrátí `True`, `picture` parametr je přiřazen binární data nahraná obrázku; Pokud metoda vrátí `False`, pracovní postup aktualizace byla zrušena a obslužné rutiny události byl ukončen.

`ValidPictureUpload(FileUpload)` Metoda kód, který byl rozdělili z DetailsView s `ItemInserting` obslužná rutina události, řídí:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample12.vb)]

## <a name="step-8-replacing-the-original-categories-pictures-with-jpgs"></a>Krok 8: Nahrazení původní obrázky kategorií formátu JPG využívá

Odvolat, že původní obrázky osm kategorie jsou soubory rastrový obrázek zabalené v hlavičce OLE. Teď, když jsme přidali schopnost upravit stávající záznam s obrázek, nahraďte formátu JPG využívá tyto bitmap pomocí za chvíli. Pokud chcete nadále používat aktuální kategorii obrázky, můžete je převést do formátu JPG využívá provedením následujících kroků:

1. Rastrové obrázky uložte na pevném disku. Přejděte `UpdatingAndDeleting.aspx` stránce v prohlížeči a pro každou kategorii prvních 8, klikněte pravým tlačítkem na bitovou kopii a vyberte obrázek uložit.
2. Otevřete bitovou kopii v editoru obrázků, vašeho výběru. Paint, můžete použít například.
3. Uložte bitovou mapu jako obrázek na JPG.
4. Aktualizace kategorie s obrázku přes rozhraní úprav, pomocí souboru JPG.

Po úpravě kategorii a odesílání JPG bitovou kopii, bitovou kopii nebude vykreslení v prohlížeči protože `DisplayCategoryPicture.aspx` stránky je odstraňování první 78 bajtů z obrázky nejprve osm kategorie. Opravte to odebráním kód, který provádí odstraňování záhlaví OLE. Po této, `DisplayCategoryPicture.aspx``Page_Load` obslužné rutiny události musí mít právě následující kód:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample13.vb)]

> [!NOTE]
> `UpdatingAndDeleting.aspx` Stránky s vkládání a úpravy rozhraní použít trochu další práci. `CategoryName` a `Description` BoundFields v DetailsView a GridView by měly být převedeny TemplateFields. Vzhledem k tomu `CategoryName` neumožňuje `NULL` hodnoty, RequiredFieldValidator by měly být přidány. A `Description` TextBox pravděpodobně by měly být převedeny do textového pole. více řádků. Můžete nechat tyto dokončujeme úpravy jako cvičení.


## <a name="summary"></a>Souhrn

V tomto kurzu dokončení naše pohled na práci s binární data. V tomto kurzu a předchozí tři jsme viděli jak binární data mohou být uloženy v systému souborů nebo přímo v databázi. Uživatel poskytuje binární data do systému výběrem soubor z jejich pevného disku a odesílání na webový server, kde může být uložený v systému souborů nebo vložit do databáze. Technologie ASP.NET 2.0 obsahuje odesílání souborů při odpovědích ovládací prvek, který umožňuje poskytování takového rozhraní stejně snadná jako přetažení. Ale, jak je uvedeno v [nahrávání souborů](uploading-files-vb.md) kurzu ovládacího prvku odesílání souborů při odpovědích je pouze pro nahrávání souborů poměrně malý, ideálně nepřesahující megabajtech vhodné. Můžeme také prozkoumali postup přidružení odeslaná data základní datový model, jakož i jak upravit a odstranit binární data z existující záznamy.

Jsou zde popsány naše další sadu kurzy různé postupy ukládání do mezipaměti. Ukládání do mezipaměti poskytuje prostředky ke zlepšení aplikace s celkový výkon převzetím výsledků náročná operace a jejich ukládání do umístění, které mají rychlejší přístup.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl Teresy Murphy. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](including-a-file-upload-option-when-adding-a-new-record-vb.md)
