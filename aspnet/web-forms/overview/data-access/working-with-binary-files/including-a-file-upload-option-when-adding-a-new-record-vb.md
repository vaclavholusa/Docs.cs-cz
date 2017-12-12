---
uid: web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-vb
title: "Včetně soubor nahrát možnost, při přidávání nového záznamu (VB) | Microsoft Docs"
author: rick-anderson
description: "Tento kurz ukazuje, jak vytvořit webové rozhraní, která umožňuje uživatelům zadejte textová data i nahrát binární soubory. Pro ilustraci t dostupné možnosti..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 5776281d-4637-4d1e-a65b-2621d2cade44
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-vb
msc.type: authoredcontent
ms.openlocfilehash: 4f49c201c71ca8f98d7e15b29f1df9a6bcd1b12e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="including-a-file-upload-option-when-adding-a-new-record-vb"></a>Při přidávání nového záznamu (VB) včetně řešením nahrávání souborů
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_56_VB.exe) nebo [stáhnout PDF](including-a-file-upload-option-when-adding-a-new-record-vb/_static/datatutorial56vb1.pdf)

> Tento kurz ukazuje, jak vytvořit webové rozhraní, která umožňuje uživatelům zadejte textová data i nahrát binární soubory. Pro ilustraci možnosti k dispozici pro uložení binární data, jeden soubor se uloží v databázi při dalších je uložen v systému souborů.


## <a name="introduction"></a>Úvod

V předchozích dvou kurzy jsme prozkoumali techniky pro ukládání binární data, která souvisí s aplikací s datovým modelem podívat, jak pomocí ovládacího prvku odesílání souborů při odpovědích odeslat soubory z klienta na webový server, a viděli, jak tato binární data k dispozici v datech W ovládací prvek EB. Jsme jste ještě na postup přidružení odeslaná data datového modelu, ale v souvislosti se.

V tomto kurzu vytvoříme na webové stránce Přidat novou kategorii. Kromě textová pole s názvem kategorie a popis bude nutné zahrnout dvou odesílání souborů při odpovědích ovládacích prvků pro nový obrázek kategorie s a jeden pro – příručka tuto stránku. Nahraný obrázek se uloží přímo v novém záznamu s `Picture` sloupce, zatímco – příručka se uloží do `~/Brochures` složku s cestou k souboru uložit v novém záznamu s `BrochurePath` sloupce.

Před vytvořením této nové webové stránce, budeme potřebovat aktualizovat architekturu. `CategoriesTableAdapter` Nenačte s hlavním dotazu `Picture` sloupce. V důsledku toho automaticky generovaný `Insert` metoda má vstupy pro jenom `CategoryName`, `Description`, a `BrochurePath` pole. Proto je potřeba vytvořit další způsob v TableAdapter, který zobrazí výzvu pro všechny čtyři `Categories` pole. `CategoriesBLL` Třídy v vrstvu obchodní logiky bude taky potřeba aktualizovat.

## <a name="step-1-adding-aninsertwithpicturemethod-to-thecategoriestableadapter"></a>Krok 1: Přidání`InsertWithPicture`metodu`CategoriesTableAdapter`

Pokud jsme vytvořili `CategoriesTableAdapter` zpět v [vytváření Data Access Layer](../introduction/creating-a-data-access-layer-vb.md) kurzu, jsme nakonfigurovali automaticky generovat `INSERT`, `UPDATE`, a `DELETE` příkazy založená na hlavní dotazu. Kromě toho jsme pokyn TableAdapter využívat DB přímý přístup, který vytvoří metody `Insert`, `Update`, a `Delete`. Tyto metody spouštění automaticky generovaný `INSERT`, `UPDATE`, a `DELETE` příkazy a v důsledku toho přijímat na základě sloupců vrácených dotazem hlavní vstupní parametry. V [nahrávání souborů](uploading-files-vb.md) kurzu jsme rozšířen `CategoriesTableAdapter` s hlavní dotaz, který bude použit `BrochurePath` sloupce.

Vzhledem k tomu, `CategoriesTableAdapter` s hlavní dotaz neodkazuje `Picture` sloupce, jsme můžete přidat nový záznam ani aktualizovat stávající záznam s hodnotou `Picture` sloupce. Aby bylo možné zachytit tyto informace, budeme buď vytvoření nové metody v TableAdapter, který se používá konkrétně k vložení záznamu s binární data nebo jsme, můžete upravit automaticky generovaný `INSERT` příkaz. Problém s přizpůsobení automaticky generovaného `INSERT` příkaz je, že jsme riskujete naše přizpůsobení přepsány průvodce. Představte si například, že jsme přizpůsobit `INSERT` k začlenění použití `Picture` sloupce. Tím by aktualizaci TableAdapter s `Insert` tak, aby zahrnoval další vstupní parametr pro binární data obrázek s kategorií s. Metoda jsme poté lze vytvořit v vrstvu obchodní logiky a použít tuto metodu DAL volat tuto metodu BLL prostřednictvím prezentační vrstvy a všechno, co by nádherně fungovat. To znamená až po příštím jsme nakonfigurovali TableAdapter pomocí Průvodce nastavením TableAdapter konfigurace. Dokončení průvodce, naše přizpůsobení `INSERT` příkaz budou přepsána, `Insert` metoda by obnovit jeho původní formuláře a kódu by nebude možné je kompilovat!

> [!NOTE]
> Tento obtěžování hlukem je jiný problém, když pomocí uložené procedury místo ad-hoc příkazů SQL. Budoucí kurzu bude prozkoumat pomocí uložené procedury místo příkazů SQL ad-hoc v datové vrstvě přístup.


Aby se zabránilo této potenciální umožní místo vytvoření nové metody pro TableAdapter s bolestmi hlavy, nikoli přizpůsobení automaticky generovaného příkazy SQL. Tuto metodu s názvem `InsertWithPicture`, bude přijímat hodnoty `CategoryName`, `Description`, `BrochurePath`, a `Picture` sloupce a spusťte `INSERT` příkaz, který ukládá všechny čtyři hodnoty v novém záznamu.

Otevřete typové datové sady a z návrháře, klikněte pravým tlačítkem na `CategoriesTableAdapter` záhlaví s a v místní nabídce vyberte Přidat dotazu. Spustí se Průvodce konfigurací dotazu TableAdapter, které začne tím, že požádá nám, jak by měla dotazu TableAdapter přistupovat k databázi. Zvolte příkazy SQL použít a klikněte na tlačítko Další. Dalším krokem vyzve k zadání typu dotazu má být vygenerován. Od jsme re vytváření dotazu na přidejte nový záznam na `Categories` tabulky, vyberte vložení a klikněte na tlačítko Další.


[![Možnost vložení](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image1.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image1.png)

**Obrázek 1**: Vyberte možnost Vložit ([Kliknutím zobrazit obrázek v plné velikosti](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image2.png))


Teď je potřeba zadat `INSERT` příkaz jazyka SQL. Průvodce automaticky navrhne `INSERT` příkaz odpovídající hlavní dotazu TableAdapter s. V takovém případě ji s `INSERT` příkaz, který se vloží `CategoryName`, `Description`, a `BrochurePath` hodnoty. Aktualizovat příkaz tak, aby `Picture` sloupec se dodává spolu s `@Picture` parametr, například takto:


[!code-sql[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample1.sql)]

Poslední obrazovka průvodce požádá nám název nová metoda TableAdapter. Zadejte `InsertWithPicture` a klikněte na tlačítko Dokončit.


[![Název nové InsertWithPicture TableAdapter – metoda](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image2.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image3.png)

**Obrázek 2**: název nová metoda TableAdapter `InsertWithPicture` ([Kliknutím zobrazit obrázek v plné velikosti](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image4.png))


## <a name="step-2-updating-the-business-logic-layer"></a>Krok 2: Aktualizace vrstvu obchodní logiky

Vzhledem k tomu, že by měl prezentační vrstvy pouze rozhraní s vrstvu obchodní logiky než obcházení ho přejít přímo na Data Access Layer, potřebujeme vytvořit BLL metoda, která volá metodu DAL jsme právě vytvořili (`InsertWithPicture`). V tomto kurzu Vytvoření metody v `CategoriesBLL` třídu s názvem `InsertWithPicture` , přijímá jako vstupní tři `String` s a `Byte` pole. `String` Jsou vstupní parametry pro název kategorie s, popis a cesta k souboru – příručka při `Byte` pole je pro binární obsah kategorie s obrázku. Jak ukazuje následující kód, tato metoda BLL vyvolá metodu odpovídající DAL:


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample2.vb)]

> [!NOTE]
> Ujistěte se, že před přidáním uložení typové datové sady `InsertWithPicture` metodu BLL. Vzhledem k tomu, `CategoriesTableAdapter` kód třídy není automaticky generovaný na základě datové sady zadali tento t nejprve uložení změn do typové datové sady `Adapter` vlastnost won t vědět o `InsertWithPicture` metoda.


## <a name="step-3-listing-the-existing-categories-and-their-binary-data"></a>Krok 3: Výpis existující kategorie a jejich binární Data

V tomto kurzu vytvoříme stránky, který umožňuje koncový uživatel přidat novou kategorii k systému, poskytuje obrázek a – příručka pro novou kategorii. V [předchozí kurzu](displaying-binary-data-in-the-data-web-controls-vb.md) jsme použili GridView s TemplateField a ImageField zobrazíte každý název kategorie s odkaz ke stažení jeho – příručka, popis a obrázek. Umožní s replikovat této funkce v tomto kurzu Vytvoření stránky, která uvádí všechny existující kategorie i umožňuje vytvořit nové.

Začněte otevřením `DisplayOrDownload.aspx` stránku z `BinaryData` složky. Přejděte do zobrazení zdroje a zkopírujte GridView a ObjectDataSource s deklarativní syntaxi, vložením v rámci `<asp:Content>` element v `UploadInDetailsView.aspx`. Navíc tento t nezapomněli zkopírujte přes `GenerateBrochureLink` metoda z kódu třídu `DisplayOrDownload.aspx` k `UploadInDetailsView.aspx`.


[![Zkopírujte a vložte deklarativní syntaxe z DisplayOrDownload.aspx UploadInDetailsView.aspx](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image3.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image5.png)

**Obrázek 3**: Zkopírujte a vložte deklarativní syntaxi z `DisplayOrDownload.aspx` k `UploadInDetailsView.aspx` ([Kliknutím zobrazit obrázek v plné velikosti](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image6.png))


Po zkopírování deklarativní syntaxi a `GenerateBrochureLink` metodu přes `UploadInDetailsView.aspx` si prohlédněte stránku prostřednictvím prohlížeče a ujistěte se, že všechno, co jste zkopírovali přes správně. Měli byste vidět GridView výpis osm kategorie, která obsahuje odkaz ke stažení – příručka, jakož i na obrázku kategorie s.


[![Teď byste měli vidět každou kategorii společně s jeho binární Data](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image4.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image7.png)

**Obrázek 4**: můžete by měl nyní najdete v části každou kategorii společně s její binární Data ([Kliknutím zobrazit obrázek v plné velikosti](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image8.png))


## <a name="step-4-configuring-thecategoriesdatasourceto-support-inserting"></a>Krok 4: Konfigurace`CategoriesDataSource`pro podporu vkládání

`CategoriesDataSource` ObjectDataSource používané `Categories` GridView aktuálně neposkytuje možnost vložit data. Kvůli podpoře vkládání prostřednictvím tohoto ovládacího prvku zdroje dat, je potřeba mapovat jeho `Insert` metoda na metodu v jeho základní objekt `CategoriesBLL`. Konkrétně jsme ji chcete mapovat k `CategoriesBLL` metoda jsme přidali zpět v kroku 2 `InsertWithPicture`.

Spusťte kliknutím na odkaz Konfigurace zdroje dat z ObjectDataSource s inteligentním. Na první obrazovce se zobrazí objekt zdroje dat je nakonfigurováno pro práci s `CategoriesBLL`. Nechte toto nastavení jako-je a klikněte na tlačítko Další zálohy na obrazovku definovat metody Data. Přesunout na kartu vložení a vyberte `InsertWithPicture` z rozevíracího seznamu. Kliknutím na tlačítko Dokončit ukončete průvodce.


[![Konfigurace ObjectDataSource lze pomocí této metody InsertWithPicture](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image5.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image9.png)

**Obrázek 5**: Konfigurace ObjectDataSource používat `InsertWithPicture` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image10.png))


> [!NOTE]
> Po dokončení průvodce, Visual Studio požádat, pokud chcete aktualizovat pole a klíče, který se znova vygeneruje data webové ovládací prvky pole. Ne, vybrat, protože kliknutím na tlačítko Ano, přepíše se tam pole vlastního nastavení provedené.


Po dokončení průvodce, ObjectDataSource nyní zahrnují hodnotu pro jeho `InsertMethod` vlastnost a také `InsertParameters` pro sloupce čtyři kategorie, jako následující kód ukazuje:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample3.aspx)]

## <a name="step-5-creating-the-inserting-interface"></a>Krok 5: Vytvoření rozhraní vložení

Jako první součástí [přehled o vložení, aktualizace a odstranění dat](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md), ovládacím prvku DetailsView poskytuje integrované vkládání rozhraní, které můžete použít při práci s ovládací prvek zdroje dat, která podporuje vkládání. Umožní s přidání ovládacího prvku DetailsView na této stránce nahoře GridView, která bude trvale vykreslit jeho vkládání rozhraní umožňuje uživatelům rychle přidáte novou kategorii. Při přidávání novou kategorii v ovládacím prvku DetailsView, bude rutina GridView pod ním automaticky aktualizovat a zobrazit novou kategorii.

Spuštění přetažením DetailsView z panelu nástrojů na návrháře výše GridView, nastavení jeho `ID` vlastnost `NewCategory` a vymazání `Height` a `Width` hodnot vlastností. Z inteligentních značek DetailsView s navázat jej na existující `CategoriesDataSource` a pak zaškrtněte políčko Povolit vložení.


[![Vytvořit vazbu CategoriesDataSource DetailsView a Povolit vložení](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image6.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image11.png)

**Obrázek 6**: DetailsView k vytvoření vazby `CategoriesDataSource` a Povolit vložení ([Kliknutím zobrazit obrázek v plné velikosti](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image12.png))


Chcete-li trvale vykreslení DetailsView v jeho vkládání rozhraní, nastavte jeho `DefaultMode` vlastnost `Insert`.

Všimněte si, že DetailsView má pět BoundFields `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, a `BrochurePath` i když `CategoryID` BoundField není v rozhraní vkládání vykreslit, protože jeho `InsertVisible` je nastavena na `False`. Tyto BoundFields existuje, protože jsou sloupce vrácený `GetCategories()` metoda, která je ObjectDataSource vyvolá načíst data. Pro vkládání, ale nemůžeme nejsou zobrazeny t chcete, aby mohl uživatel, zadejte hodnotu pro `NumberOfProducts`. Kromě toho je potřeba povolit, aby nahrát obrázku pro nové kategorie a také nahrát PDF pro – příručka.

Odeberte `NumberOfProducts` BoundField DetailsView úplně a pak aktualizujte `HeaderText` vlastnosti `CategoryName` a `BrochurePath` BoundFields do kategorií a – příručka, v uvedeném pořadí. V dalším kroku převést `BrochurePath` BoundField do TemplateField a přidat nové TemplateField pro obrázek, poskytnete tento nový TemplateField `HeaderText` hodnotu obrázku. Přesunout `Picture` TemplateField, aby byla mezi `BrochurePath` TemplateField a CommandField.


![Vytvořit vazbu CategoriesDataSource DetailsView a Povolit vložení](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image7.gif)

**Obrázek 7**: DetailsView k vytvoření vazby `CategoriesDataSource` a Povolit vložení


Pokud jste převedli `BrochurePath` BoundField do TemplateField prostřednictvím dialogové okno Upravit pole TemplateField zahrnuje `ItemTemplate`, `EditItemTemplate`, a `InsertItemTemplate`. Pouze `InsertItemTemplate` je potřeba, ale, takže můžete bez obav odstranit dvě šablony. V tomto okamžiku deklarativní syntaxi DetailsView s by měl vypadat asi takto:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample4.aspx)]

## <a name="adding-fileupload-controls-for-the-brochure-and-picture-fields"></a>Přidání ovládacích prvků odesílání souborů při odpovědích – příručka a pole obrázků

V současné době `BrochurePath` TemplateField s `InsertItemTemplate` obsahuje textové pole, když `Picture` TemplateField neobsahuje žádné šablony. Je potřeba aktualizovat tyto dvě s TemplateField `InsertItemTemplate` s k využití řízení odesílání souborů při odpovědích.

Ze s DetailsView inteligentní značky, zvolte možnost Upravit šablony a potom vyberte `BrochurePath` TemplateField s `InsertItemTemplate` z rozevíracího seznamu. Odeberte textového pole a poté přetáhněte ovládací prvek odesílání souborů při odpovědích z panelu nástrojů do šablony. Nastavení ovládacího prvku odesílání souborů při odpovědích s `ID` k `BrochureUpload`. Podobně, přidání ovládacího prvku odesílání souborů při odpovědích `Picture` TemplateField s `InsertItemTemplate`. Nastavit tento ovládací prvek odesílání souborů při odpovědích s `ID` k `PictureUpload`.


[![Přidání ovládacího prvku odesílání souborů při odpovědích na InsertItemTemplate](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image8.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image13.png)

**Obrázek 8**: Přidání ovládacího prvku odesílání souborů při odpovědích `InsertItemTemplate` ([Kliknutím zobrazit obrázek v plné velikosti](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image14.png))


Po provedení těchto dodatky, bude mít dva TemplateField s deklarativní syntaxi:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample5.aspx)]

Pokud uživatel přidá novou kategorii, chceme zajistěte, aby – příručka a obrázek o správný typ souboru. Pro – příručka musí uživatel zadat PDF. Na obrázku, je nutné uživatelům nahrát soubor obrázku, ale provést povolujeme *žádné* bitové kopie souboru nebo jenom soubory obrázků určitého typu, například GIF nebo formátu JPG využívá? Aby bylo možné různých typech souborů, d musíme rozšířit `Categories` schéma tak, aby zahrnovaly sloupec, který zachycuje typ souboru, aby tento typ může odeslat klientovi prostřednictvím `Response.ContentType` v `DisplayCategoryPicture.aspx`. Vzhledem k tomu, že jsme nejsou zobrazeny t mít tento sloupce, je doporučeno omezit uživatele, kteří pouze poskytovat typu souboru konkrétní image. `Categories` Existujících bitových kopií tabulky s rastrové obrázky, ale formátu JPG využívá jsou vhodnější formát souboru pro obrázky obsluhovat prostřednictvím webu.

Pokud uživatel odešle typ nesprávný soubor, je potřeba zrušit úlohy insert a zobrazí zprávu s upozorněním, problém. Přidání ovládacího prvku popisek webové pod DetailsView. Nastavit jeho `ID` vlastnost `UploadWarning`, zrušte na jeho `Text` vlastnost, nastavte `CssClass` vlastnost na upozornění a `Visible` a `EnableViewState` vlastnosti, které chcete `False`. `Warning` Třída CSS, která je definována v `Styles.css` a vykreslí text ve velkých, red, kurzívou, tučné písmo.

> [!NOTE]
> V ideálním případě `CategoryName` a `Description` BoundFields bude převedena na TemplateFields a jejich vložení rozhraní přizpůsobit. `Description` Vkládání rozhraní, například by pravděpodobně být vhodnější prostřednictvím textové pole víceřádkový. A protože `CategoryName` sloupec nepřijímá `NULL` hodnoty, RequiredFieldValidator musí být přidaní do zkontrolujte uživatele poskytuje hodnotu pro název nové kategorie s. Tyto kroky jsou ponechána jako cvičení čtečky. Odkazovat zpět [přizpůsobení rozhraní pro úpravu dat](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) pro hlubší pohled na rozhraní pro úpravu dat rozšířit.


## <a name="step-6-saving-the-uploaded-brochure-to-the-web-server-s-file-system"></a>Krok 6: Ukládání nahrané – příručka k systému souborů webového serveru s

Když uživatel zadá hodnoty pro novou kategorii a kliknutím na tlačítko Vložit, proběhne zpětné volání a kterou se vkládání pracovního postupu. První, DetailsView s [ `ItemInserting` událostí](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.detailsview.iteminserting.aspx) aktivuje. Další, ObjectDataSource s `Insert()` metoda je volána, výsledkem nový záznam, který se přidává do `Categories` tabulky. Potom DetailsView s [ `ItemInserted` událostí](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.detailsview.iteminserted.aspx) aktivuje.

Před ObjectDataSource s `Insert()` metoda je volána, jsme musíte nejdříve se ujistěte, typy souborů odpovídající byly odeslaný uživatele a pak uložte – příručka PDF do systému souborů webového serveru s. Vytvoření obslužné rutiny události pro DetailsView s `ItemInserting` událostí a přidejte následující kód:


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample6.vb)]

Obslužné rutiny události spustí odkazem `BrochureUpload` řízení odesílání souborů při odpovědích ze šablon s DetailsView. Poté Pokud – příručka se nahrají, je zkontrolován s příponou nahrávaný soubor. Pokud není rozšíření. PDF, pak upozornění se zobrazí, se zruší úlohy insert a ukončí provádění obslužné rutiny události.

> [!NOTE]
> Spoléhat na s příponou nahrávaný soubor není sure-fire technika pro zajištění, že nahrávaný soubor je dokument PDF. Uživatel může mít platný dokument PDF s příponou `.Brochure`, nebo může mít prováděné dokumentu souborů PDF a zadané ho `.pdf` rozšíření. Binární obsah souboru s by bylo nutné prostřednictvím kódu programu prověřit, chcete-li více jednoznačně ověřte typ souboru. Takové důkladné přístupy, ale jsou často přehnaně; Kontrola rozšíření je dostatečné pro většinu scénářů.


Jak je popsáno v [nahrávání souborů](uploading-files-vb.md) kurzu, musí dát pozor, při ukládání souborů do systému souborů tak, že jeden uživatel s nahrávání nepřepisuje jiné s. V tomto kurzu jsme se pokusí použít stejný název jako nahrávaný soubor. Pokud již existuje soubor v `~/Brochures` directory tohoto souboru stejný název, ale nemůžeme budete připojit číslo na konci dokud nebude nalezen jedinečný název. Například, pokud uživatel odesílá informace o soubor s názvem `Meats.pdf`, ale již existuje soubor s názvem `Meats.pdf` v `~/Brochures` složky, Změníme názvu uloženého souboru `Meats-1.pdf`. Pokud existuje, pokusíme se `Meats-2.pdf`a tak dále, dokud nebude nalezen jedinečný název souboru.

Následující kód používá [ `File.Exists(path)` metoda](https://msdn.microsoft.com/en-us/library/system.io.file.exists.aspx) k určení, pokud soubor se zadaným názvem již existuje. Pokud ano, potrvá pokusit nové názvy souborů – příručka, dokud nebude nalezen žádný konflikt.


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample7.vb)]

Jakmile nebyl nalezen platný název souboru, soubor musí být uloženy do systému souborů a ObjectDataSource s `brochurePath``InsertParameter` hodnota musí být aktualizovány tak, aby tento název souboru je zapsána do databáze. Jak jsme viděli zpět v *nahrávání souborů* kurzu soubor můžete uložit pomocí ovládacího prvku odesílání souborů při odpovědích s `SaveAs(path)` metoda. Chcete-li aktualizovat ObjectDataSource s `brochurePath` parametr, použijte `e.Values` kolekce.


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample8.vb)]

## <a name="step-7-saving-the-uploaded-picture-to-the-database"></a>Krok 7: Ukládání nahraný obrázek do databáze

K uložení nahraný obrázek v novém `Categories` záznamů, je potřeba přiřadit nahrané binární obsah ObjectDataSource s `picture` parametr v DetailsView s `ItemInserting` událostí. Uděláme toto přiřazení, ale musíme nejprve se ujistěte, že nahraný obrázek je JPG a není nějaký jiný typ obrázku. Jako v kroku 6 umožní používat příponu souboru s nahraný obrázek zjistit typ s.

Při `Categories` tabulky umožňuje `NULL` hodnoty `Picture` sloupec, všechny kategorie aktuálně mít obrázku. Umožní s nutí uživatele zadat obrázku při přidávání nové kategorie prostřednictvím této stránky. Následující kód kontroluje odeslal obrázku a zda má příslušná rozšíření.


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample9.vb)]

Tento kód musí být umístěny *před* kód z kroku 6 tak, že pokud dojde k potížím s nahrávání obrázku, obslužné rutiny události bude ukončen před uložením souboru – příručka k systému souborů.

Za předpokladu, že příslušný soubor byl odeslán, přiřaďte nahrané binární obsah hodnotu obrázek parametru s následující řádek kódu:


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample10.vb)]

## <a name="the-completeiteminsertingevent-handler"></a>Kompletní`ItemInserting`obslužné rutiny události

Pro úplnost, zde je `ItemInserting` obslužné rutiny událostí v celé jeho šíři:


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample11.vb)]

## <a name="step-8-fixing-thedisplaycategorypictureaspxpage"></a>Krok 8: Opravě`DisplayCategoryPicture.aspx`stránky

Umožňují s za chvíli k otestování vkládání rozhraní a `ItemInserting` obslužné rutiny události, který byl vytvořen v posledních několika krocích. Přejděte `UploadInDetailsView.aspx` stránky prostřednictvím prohlížeče a pokus o přidání kategorie, ale vynechejte na obrázku, nebo zadat jiný JPG obrázku nebo – příručka jiného typu než PDF. V každém z těchto případů zobrazí se chybová zpráva a pracovní postup vložení zrušen.


[![Zpráva upozornění se zobrazí, pokud je načtený neplatný typ souboru](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image9.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image15.png)

**Obrázek 9**: zprávy A upozornění se zobrazí, pokud je načtený neplatný typ souboru ([Kliknutím zobrazit obrázek v plné velikosti](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image16.png))


Jakmile ověříte, že stránka vyžaduje obrázku k odeslání a získaných t přijmout soubory jiného typu než PDF nebo jiných JPG, přidat novou kategorii s platný obrázek JPG, když – příručka pole ponecháte prázdné. Po kliknutí na tlačítko Vložit, bude odeslat zpět na stránku a nový záznam se zařadí do `Categories` tabulku s binární obsah nahraný obrázek s uloženy přímo v databázi. GridView se aktualizuje a zobrazí řádek pro nově přidaného kategorie, ale, jak ukazuje obrázek 10, není nový obrázek s kategorie vykreslí správně.


[![Nové kategorie s, které se nezobrazí obrázek](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image10.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image17.png)

**Obrázek 10**: novou kategorii s obrázek nezobrazí ([Kliknutím zobrazit obrázek v plné velikosti](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image18.png))


Nový obrázek se nezobrazí. Důvodem je skutečnost, protože `DisplayCategoryPicture.aspx` stránky, která vrátí Zadaná kategorie s obrázku je nakonfigurovat pro zpracování rastrové obrázky, které mají hlavičku OLE. Tuto hlavičku 78 bajtů se odstraní z `Picture` sloupec s binární obsah před jejich odesláním zpět do klienta. Ale soubor JPG, který jsme právě nahráli nové kategorie nemá tuto hlavičku OLE; platný, nezbytné bajtů jsou proto odebírán z bitové kopie s binární data.

Vzhledem k tomu, že jsou teď i bitmapy s hlavičky OLE a formátu JPG využívá v `Categories` tabulky, musíme aktualizovat `DisplayCategoryPicture.aspx` tak, aby nepodporuje hlavičku OLE odstraňování pro původní osm kategorie a obchází odstraňování záznamů novější kategorie. V našem kurzu další podíváme, jak se bude aktualizovat stávající záznam s image a budete aktualizujeme všechny staré kategorie obrázky, aby byly formátu JPG využívá. Teď, když, použijte následující kód v `DisplayCategoryPicture.aspx` k odebrání hlaviček OLE pouze u těchto původní osm kategorií:


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample12.vb)]

Díky této změně JPG bitové kopie je nyní v správně vykreslen GridView.


[![JPG Image pro nové kategorie jsou správně vykreslen.](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image11.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image19.png)

**Obrázek 11**: obrázků JPG pro nové kategorie jsou správně vykreslen ([Kliknutím zobrazit obrázek v plné velikosti](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image20.png))


## <a name="step-9-deleting-the-brochure-in-the-face-of-an-exception"></a>Krok 9: Odstranění – příručka při krátkodobém výjimku

Jedním z problémů v systému souborů webového serveru s ukládání binárních dat je, že přináší odpojení mezi v datovém modelu a jeho binární data. Proto vždy, když dojde k odstranění záznamu, odpovídající binární data v systému souborů musí také odebrány. To lze uplatnit při vkládání také. Vezměte v úvahu následující scénář: uživatel přidá novou kategorii, zadání – příručka a platný obrázek. Po kliknutí na tlačítko Vložit, dojde k zpětné volání a DetailsView s `ItemInserting` událost je aktivována, ukládání – příručka k systému souborů webového serveru s. Další, ObjectDataSource s `Insert()` je volána metoda, která volá `CategoriesBLL` třídu s `InsertWithPicture` metodu, která volá `CategoriesTableAdapter` s `InsertWithPicture` metoda.

Nyní, co se stane, pokud se databáze nachází v režimu offline nebo pokud dojde k chybě v `INSERT` příkazu SQL? Jasně se nezdaří úlohy INSERT, aby žádné nové kategorie řádek bude přidán do databáze. Ale stále nahrané – příručka soubor uložený v systému souborů webového serveru s! Tento soubor je nutné odstranit při krátkodobém výjimku během vkládání pracovního postupu.

Jak je popsáno dříve v [zpracování BLL - a výjimky na úrovni DAL na stránku ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) kurzu, když je vyvolána výjimka z v rámci depths architektury probublá až prostřednictvím různých vrstev. V prezentační vrstvě, můžeme určit, pokud došlo k výjimce z DetailsView s `ItemInserted` událostí. Tuto obslužnou rutinu události také poskytuje hodnoty ObjectDataSource s `InsertParameters`. Proto, můžeme vytvořit obslužnou rutinu události pro `ItemInserted` událost, která zkontroluje, zda došlo k výjimce a pokud ano, odstraní soubor určený touto ObjectDataSource s `brochurePath` parametr:


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample13.vb)]

## <a name="summary"></a>Souhrn

Existuje několik kroků, které musíte provést, chcete-li poskytovat webové rozhraní pro přidávání záznamů, které zahrnují binární data. Pokud binární data ukládají přímo do databáze, je možné se, že budete muset aktualizovat architekturu, přidání konkrétní metody pro zpracování případu, kdy bude vložen, binární data. Jakmile architektura byla aktualizovaná, dalším krokem vytvoření vkládání rozhraní, což lze provést pomocí DetailsView, který byl přizpůsoben, aby patří odesílání souborů při odpovědích ovládací prvek pro každé pole binární data. Odeslaná data pak můžete uložit do systému souborů webového serveru s nebo přiřazené parametr zdroje dat v DetailsView s `ItemInserting` obslužné rutiny události.

Ukládání binární data do systému souborů vyžaduje další plánování než ukládání dat přímo do databáze. Schéma pojmenování musí být vybrána, aby se zabránilo jeden uživatel s nahrávání přepsal jiné s. Další kroky musí navíc přesměrováni na Odstranit nahrávaný soubor, pokud se nezdaří vložení databáze.

Nyní je k dispozici možnost přidávat nové kategorie s – příručka a obrázek, ale nemůžeme systému jste ještě a podívat se na tom, jak aktualizovat existující kategorie s binární data nebo jak správně odebrat binární data pro odstraněné kategorii. V dalším kurzu jsme budete prozkoumejte těchto dvou tématech.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři v tomto kurzu se Dave Gardner, Teresy Murphy a Bernadette Leigh. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](displaying-binary-data-in-the-data-web-controls-vb.md)
[další](updating-and-deleting-existing-binary-data-vb.md)
