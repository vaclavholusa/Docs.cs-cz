---
uid: web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-vb
title: Včetně soubor nahrát možnost, při přidání nového záznamu (VB) | Dokumentace Microsoftu
author: rick-anderson
description: Tento kurz ukazuje, jak vytvořit webové rozhraní, který umožňuje uživateli zadat text data i nahrát binární soubory. Pro ilustraci t dostupné možnosti...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: 5776281d-4637-4d1e-a65b-2621d2cade44
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-vb
msc.type: authoredcontent
ms.openlocfilehash: 22ca0d85fac598b2f845be4bd5c18fdcbd3bc3a8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753114"
---
<a name="including-a-file-upload-option-when-adding-a-new-record-vb"></a>Zahrnutí možnosti nahrání souboru při přidání nového záznamu (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_56_VB.exe) nebo [stahovat PDF](including-a-file-upload-option-when-adding-a-new-record-vb/_static/datatutorial56vb1.pdf)

> Tento kurz ukazuje, jak vytvořit webové rozhraní, který umožňuje uživateli zadat text data i nahrát binární soubory. Pro ilustraci, možnosti, které jsou k dispozici pro ukládání binárních dat, jeden soubor uloží v databázi, zatímco druhá je uložen v systému souborů.


## <a name="introduction"></a>Úvod

V předchozích kurzech dvě Prozkoumali jsme techniky pro ukládání binárních dat, který je spojen s aplikací s datovým modelem podívali se na tom, jak pomocí ovládacího prvku FileUpload odeslání souborů od klienta na webový server, a viděli, jak k dispozici tato binární data v datech W EB ovládacího prvku. Doporučujeme uložit ještě do mluvit o tom, jak přidružit k nahraných dat v datovém modelu, ale.

V tomto kurzu vytvoříme webovou stránku, abyste mohli přidat novou kategorii. Kromě textová pole kategorie s název a popis Tato stránka bude nutné zahrnout dvou ovládacích prvků FileUpload pro novou kategorii s obrázek a jeden pro brožura. Nahraného obrázku se uloží přímo do nového záznamu s `Picture` sloupce, zatímco brožura se uloží do `~/Brochures` složku s cestou k souboru uloženého v novém záznamu s `BrochurePath` sloupce.

Před vytvořením této nové webové stránky, potřebujeme aktualizovat architektury. `CategoriesTableAdapter` Nenačte s hlavním dotazem `Picture` sloupce. V důsledku toho automaticky generovanou `Insert` metoda má jenom vstupy pro `CategoryName`, `Description`, a `BrochurePath` pole. Proto potřebujeme vytvořit další metodu v TableAdapter, který zobrazí výzvu pro všechny čtyři `Categories` pole. `CategoriesBLL` Třídy v vrstvy obchodní logiky bude také potřeba aktualizovat.

## <a name="step-1-adding-aninsertwithpicturemethod-to-thecategoriestableadapter"></a>Krok 1: Přidání`InsertWithPicture`metodu`CategoriesTableAdapter`

Když jsme vytvořili `CategoriesTableAdapter` zpátky [vytvoření vrstvy přístupu k datům](../introduction/creating-a-data-access-layer-vb.md) kurzu jsme se nakonfigurovaly, budou automaticky generovány `INSERT`, `UPDATE`, a `DELETE` příkazy na základě hlavního dotazu. Kromě toho jsme vydal pokyn pro nástroje TableAdapter využívat DB přímý přístup, kde vytvořeny metody `Insert`, `Update`, a `Delete`. Tyto metody spustit automaticky generovanou `INSERT`, `UPDATE`, a `DELETE` příkazy a v důsledku toho přijmout vstupní parametry, které jsou založeny na sloupcích vráceného hlavním dotazem. V [nahrávání souborů](uploading-files-vb.md) kurz můžeme rozšířit `CategoriesTableAdapter` s hlavní dotaz, který bude použit `BrochurePath` sloupce.

Protože `CategoriesTableAdapter` s hlavní dotaz neodkazuje `Picture` sloupce, můžeme přidat nový záznam ani aktualizovat stávající záznam s hodnotou pro `Picture` sloupec. Aby bylo možné zachytit tyto informace, jsme můžete vytvořit novou metodu v TableAdapter, který se používá konkrétně k vložení záznamu s binárními daty nebo nám můžete přizpůsobit automaticky generovanou `INSERT` příkazu. Problém s přizpůsobení automaticky generovaného `INSERT` příkaz je, že jsme rizika s naší vlastní nastavení přepsat pomocí průvodce. Představte si například, že můžeme přizpůsobit `INSERT` příkaz patří použití `Picture` sloupce. To by aktualizace objektu TableAdapter s `Insert` tak, aby zahrnoval další vstupní parametr pro binární data obrázku s kategorií s. My pak vytvořit metodu v vrstvy obchodní logiky a použít tuto metodu DAL vyvolat tuto metodu BLL přes prezentační vrstvu a všechno, co by fungovalo nádherně. To znamená až do příštího jsme nakonfigurovali TableAdapter pomocí Průvodce konfigurací TableAdapter. Co nejdříve po dokončení průvodce, naše vlastní nastavení `INSERT` příkaz přepsána, `Insert` metoda vrátí na jeho původní formulář, a našeho kódu by již kompilován!

> [!NOTE]
> Tato obtěžování hlukem je bez problému, při použití uložené procedury místo SQL příkazy ad-hoc. Budoucí kurzu bude prozkoumat pomocí uložené procedury namísto SQL příkazy ad-hoc v vrstvy přístupu k datům.


Chcete-li se vyhnout této potenciálnímu starostí, nikoli úprava příkazů SQL automaticky generované umožní s místo vytvoření nové metody pro TableAdapter. Tuto metodu s názvem `InsertWithPicture`, bude přijímat hodnoty `CategoryName`, `Description`, `BrochurePath`, a `Picture` sloupců a spusťte `INSERT` příkaz, který uchovává všechny čtyři hodnoty v novém záznamu.

Otevřete datovou sadu typu a z návrháře, klikněte pravým tlačítkem na `CategoriesTableAdapter` s záhlaví a v místní nabídce zvolte možnost přidat dotaz. Otevře se Průvodce konfigurací dotazu TableAdapter, který začíná nám požádá, jak se má TableAdapter dotazovat by měl přístup k databázi. Zvolte možnost použít SQL příkazy a klikněte na tlačítko Další. Dalším krokem vyzve k zadání typu dotazu vygenerování. Protože jsme k vytvoření dotazu na přidání nového záznamu `Categories` tabulky, zvolte vložení a klikněte na tlačítko Další.


[![Vyberte možnost Vložit](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image1.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image1.png)

**Obrázek 1**: Vyberte možnost Vložit ([kliknutím ji zobrazíte obrázek v plné velikosti](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image2.png))


Nyní potřebujeme k určení `INSERT` příkaz jazyka SQL. Průvodce automaticky navrhne `INSERT` příkaz odpovídající s hlavním dotazu objektu TableAdapter. V takovém případě ji s `INSERT` příkaz, který se vloží `CategoryName`, `Description`, a `BrochurePath` hodnoty. Aktualizujte příkaz tak, aby `Picture` sloupec je zahrnut spolu s `@Picture` parametr, takto:


[!code-sql[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample1.sql)]

Poslední obrazovka průvodce výzva pojmenujte novou metodu objektu TableAdapter. Zadejte `InsertWithPicture` a klikněte na tlačítko Dokončit.


[![Název nové InsertWithPicture TableAdapter – metoda](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image2.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image3.png)

**Obrázek 2**: název nové metody třídy TableAdapter `InsertWithPicture` ([kliknutím ji zobrazíte obrázek v plné velikosti](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image4.png))


## <a name="step-2-updating-the-business-logic-layer"></a>Krok 2: Aktualizace vrstvy obchodní logiky

Protože prezentační vrstva by měla pouze rozhraní s vrstvy obchodní logiky, spíše než bude vynecháno, přejdete přímo na vrstvy přístupu k datům, je nutné vytvořit BLL metodu, která volá metodu DAL jsme právě vytvořili (`InsertWithPicture`). Pro účely tohoto kurzu vytvořte metodu v `CategoriesBLL` třídu s názvem `InsertWithPicture` , který přijímá jako vstupní tři `String` s a `Byte` pole. `String` Vstupní parametry jsou pro kategorii s názvem, popis a si brožuru o cestu k souboru, zatímco `Byte` pole je pro binární obsah obrázek kategorie s. Jak ukazuje následující kód, tato metoda BLL vyvolá metodu odpovídající vrstvy DAL:


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample2.vb)]

> [!NOTE]
> Ujistěte se, že jste uložili datové sadě zadán před přidáním `InsertWithPicture` metodu BLL. Protože `CategoriesTableAdapter` kód třídy je automaticky vytvářen založené na datové sadě zadán, pokud don t uložte provedené změny k datové sadě zadán `Adapter` vlastnost vyhráli t vědět o `InsertWithPicture` metoda.


## <a name="step-3-listing-the-existing-categories-and-their-binary-data"></a>Krok 3: Výpis existující kategorie a jejich binárních dat

V tomto kurzu vytvoříme stránku, která umožňuje koncovým uživatelům přidat novou kategorii k systému, poskytuje přehled a – příručka pro novou kategorii. V [předchozím kurzu](displaying-binary-data-in-the-data-web-controls-vb.md) jsme použili GridView s TemplateField a ImageField zobrazíte každé kategorie s názvem popis, obrázek a odkaz ke stažení jeho si brožuru o. Umožní s replikace, které tuto funkci pro účely tohoto kurzu, vytvoření stránky, který obsahuje všechny existující kategorie a umožňuje nové značky, který se má vytvořit.

Začněte otevřením `DisplayOrDownload.aspx` stránku ze `BinaryData` složky. Přejděte do zobrazení zdroje a zkopírujte ovládacími prvky GridView a prvku ObjectDataSource s deklarativní syntaxe, vložte ho v rámci `<asp:Content>` prvek `UploadInDetailsView.aspx`. Také, nezapomeňte zkopírovat don t `GenerateBrochureLink` metody třídy modelu code-behind `DisplayOrDownload.aspx` k `UploadInDetailsView.aspx`.


[![Zkopírujte a vložte z DisplayOrDownload.aspx UploadInDetailsView.aspx deklarativní syntaxe](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image3.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image5.png)

**Obrázek 3**: Zkopírujte a vložte deklarativní syntaxe z `DisplayOrDownload.aspx` k `UploadInDetailsView.aspx` ([kliknutím ji zobrazíte obrázek v plné velikosti](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image6.png))


Po zkopírování deklarativní syntaxe a `GenerateBrochureLink` metodu přes `UploadInDetailsView.aspx` stránku, prohlížení stránky prostřednictvím prohlížeče k zajištění, že všechno, co se zkopíruje správně. Měli byste vidět GridView výpis osm kategorií, která obsahuje odkaz ke stažení brožura, stejně jako kategorie s obrázek.


[![Teď byste měli vidět jednotlivých kategorií spolu s jeho binární Data](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image4.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image7.png)

**Obrázek 4**: můžete by měl nyní naleznete v tématu každý kategorie spolu s jeho binárních dat ([kliknutím ji zobrazíte obrázek v plné velikosti](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image8.png))


## <a name="step-4-configuring-thecategoriesdatasourceto-support-inserting"></a>Krok 4: Konfigurace`CategoriesDataSource`k vkládání podpory

`CategoriesDataSource` ObjectDataSource používá `Categories` GridView aktuálně neposkytuje možnost vložit data. Kvůli podpoře vkládání přes tento ovládací prvek zdroje dat, musíme jeho `Insert` metoda na metodu v základní objekt, `CategoriesBLL`. Konkrétně se chceme ji namapovat na `CategoriesBLL` jsme přidali zpět v kroku 2, metoda `InsertWithPicture`.

Začněte kliknutím na odkaz Konfigurovat zdroj dat z prvku ObjectDataSource s inteligentním. Na první obrazovce se zobrazí objekt zdroje dat je nakonfigurováno pro práci s `CategoriesBLL`. Toto políčko nechat jako-je a klikněte na tlačítko Další přejděte na obrazovku definovat metody dat. Přejít na kartu vložení a vybrat `InsertWithPicture` metodu z rozevíracího seznamu. Kliknutím na Dokončit dokončíte průvodce.


[![Konfigurace ObjectDataSource InsertWithPicture metody](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image5.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image9.png)

**Obrázek 5**: Konfigurace ObjectDataSource používat `InsertWithPicture` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image10.png))


> [!NOTE]
> Po dokončení průvodce bude Visual Studio požádat, pokud chcete aktualizovat pole a klíče, který se znova vygeneruje dat webové ovládací prvky pole. Zvolte možnost Ne, protože kliknutím na tlačítko Ano, přepíše všechna pole vlastní nastavení provedené může mít.


Po dokončení průvodce, prvku ObjectDataSource teď bude zahrnovat hodnotu pro jeho `InsertMethod` vlastnost stejně jako `InsertParameters` čtyři kategorie sloupců jako následující kód ukazuje:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample3.aspx)]

## <a name="step-5-creating-the-inserting-interface"></a>Krok 5: Vytvoření rozhraní pro vložení

Jako první se věnují [přehled o vložení, aktualizace a odstranění dat](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md), ovládacím prvku DetailsView poskytuje integrované vkládání rozhraní, které můžete využít při práci s ovládací prvek zdroje dat, který podporuje vkládání. Umožní s přidání ovládacího prvku DetailsView na tuto stránku nad prvku GridView, která se vykreslí trvale jeho vkládání rozhraní, která uživatelům umožňuje rychle přidat novou kategorii. Při přidání nové kategorie v ovládacím prvku DetailsView, GridView pod ním automaticky aktualizovat a zobrazit novou kategorii.

Začněte tím, že přetažením z panelu nástrojů na Návrhář nad prvku GridView, nastavení DetailsView jeho `ID` vlastnost `NewCategory` a mazání navýšení kapacity `Height` a `Width` hodnot vlastností. Z inteligentních značek prvek DetailsView s vázat na existující `CategoriesDataSource` a potom zaškrtněte políčko Povolit vložení.


[![Svázat s ovládacím prvku DetailsView CategoriesDataSource a Povolit vložení](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image6.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image11.png)

**Obrázek 6**: ovládacím prvku DetailsView. k vytvoření vazby `CategoriesDataSource` a Povolit vložení ([kliknutím ji zobrazíte obrázek v plné velikosti](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image12.png))


Chcete-li trvale vykreslení ovládacím prvku DetailsView. v jeho vkládání rozhraní, nastavte jeho `DefaultMode` vlastnost `Insert`.

Všimněte si, že ovládacím prvku DetailsView má pět BoundFields `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, a `BrochurePath` i když `CategoryID` Vlastnost BoundField není v rozhraní vkládání vykreslit, protože jeho `InsertVisible` Vlastnost je nastavena na `False`. Tyto BoundFields existuje, protože jsou vrácené sloupce `GetCategories()` metodu, která je prvku ObjectDataSource vyvolá načíst jeho data. Pro vložení, ale můžeme zadávat t chcete uživatelům povolit, zadejte hodnotu pro `NumberOfProducts`. Kromě toho musíte zajistí, aby nahrát obrázek pro novou kategorii, stejně jako nahrát soubor PDF pro brožura.

Odeberte `NumberOfProducts` Vlastnost BoundField zcela DetailsView a pak aktualizujte `HeaderText` vlastnosti `CategoryName` a `BrochurePath` BoundFields kategorii a brožura, v uvedeném pořadí. V dalším kroku převést `BrochurePath` Vlastnost BoundField na pole TemplateField a přidat nový TemplateField obrázku, poskytuje tento nový TemplateField `HeaderText` hodnotu obrázku. Přesunout `Picture` TemplateField tak, že je mezi `BrochurePath` TemplateField a CommandField.


![Svázat s ovládacím prvku DetailsView CategoriesDataSource a Povolit vložení](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image7.gif)

**Obrázek 7**: ovládacím prvku DetailsView. k vytvoření vazby `CategoriesDataSource` a Povolit vložení


Pokud jste převedli `BrochurePath` obsahuje vlastnost BoundField na pole TemplateField prostřednictvím dialogového okna Upravit pole pole TemplateField `ItemTemplate`, `EditItemTemplate`, a `InsertItemTemplate`. Pouze `InsertItemTemplate` je potřeba, ale tak můžete bez obav odstranit tyto dvě šablony. V tomto okamžiku vaše prvek DetailsView s deklarativní syntaxe by měl vypadat nějak takto:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample4.aspx)]

## <a name="adding-fileupload-controls-for-the-brochure-and-picture-fields"></a>Přidání ovládacích prvků FileUpload brožura a pole obrázků

V současné době `BrochurePath` TemplateField s `InsertItemTemplate` obsahuje textové pole, zatímco `Picture` TemplateField neobsahuje žádné šablony. Potřebujeme k aktualizaci těchto dvou s TemplateField `InsertItemTemplate` s k využití řízení FileUpload.

Z prvku DetailsView s inteligentní značku, zvolte možnost Upravit šablony a pak vyberte `BrochurePath` TemplateField s `InsertItemTemplate` z rozevíracího seznamu. Odeberte textového pole a pak přetáhněte FileUpload ovládacího prvku z panelu nástrojů do šablony. Nastavení ovládacího prvku FileUpload s `ID` k `BrochureUpload`. Podobně, přidání ovládacího prvku FileUpload `Picture` TemplateField s `InsertItemTemplate`. Nastavit tento ovládací prvek FileUpload s `ID` k `PictureUpload`.


[![Přidejte ovládací prvek odesílání souborů při odpovědích InsertItemTemplate](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image8.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image13.png)

**Obrázek 8**: Přidání ovládacího prvku odesílání souborů při odpovědích `InsertItemTemplate` ([kliknutím ji zobrazíte obrázek v plné velikosti](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image14.png))


Po provedení tyto doplňky, budou dva TemplateField s deklarativní syntaxe:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample5.aspx)]

Když uživatel přidá novou kategorii, chceme se ujistit, že si brožuru o a obrázek mají správný typ souboru. Pro brožura musí uživatel zadat soubor PDF. Pro obrázek, potřebujeme, aby uživatel k nahrání souboru obrázku, ale proveďte umožňujeme *žádné* image souboru nebo jenom soubory obrázků určitého typu, jako jsou obrázky ve formátu GIF nebo formátu JPG využívá? Aby bylo možné povolit pro různé typy souborů, d musíme rozšířit `Categories` schéma sloupec, který explicitně zaznamenává typ souboru tak, aby tento typ je odeslat do klienta prostřednictvím `Response.ContentType` v `DisplayCategoryPicture.aspx`. Protože jsme zadávat t mají tyto sloupce, bylo by vhodné omezit uživatele na poskytují pouze typ souboru konkrétní image. `Categories` Tabulky s existující Image rastrové obrázky, ale formátu JPG využívá jsou vhodnější formátu pro obrázky obsluhovat prostřednictvím webu.

Pokud nějaký uživatel odešle typu souboru, musíme zrušit insert a zobrazit zprávu s upozorněním problém. Přidejte ovládací prvek webového popisek pod ovládacím prvku DetailsView. Nastavte jeho `ID` vlastnost `UploadWarning`, zrušte zaškrtnutí políčka si jeho `Text` , nastavit `CssClass` vlastnost na upozornění a `Visible` a `EnableViewState` vlastností `False`. `Warning` Třídu šablony stylů CSS je definována v `Styles.css` a generuje text ve velké, červenou, kurzíva, tučné písmo.

> [!NOTE]
> V ideálním případě by `CategoryName` a `Description` BoundFields by převedou na hodnoty vlastností TemplateField a jejich vkládání rozhraní přizpůsobit. `Description` Vkládání rozhraní, například by pravděpodobně aby lépe vyhovoval prostřednictvím ve víceřádkovém textovém poli. A protože `CategoryName` sloupec nepřijímá `NULL` hodnoty, RequiredFieldValidator by se měl přidat k zajištění uživatel zadá hodnotu pro název nové kategorie s. Tyto kroky jako cvičení zůstanou čtečku. Vraťte se do [přizpůsobení rozhraní pro úpravu dat](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) pro hlubší pohled na rozšiřování úpravy rozhraní data.


## <a name="step-6-saving-the-uploaded-brochure-to-the-web-server-s-file-system"></a>Krok 6: Ukládání nahraného – příručka k systému souborů webového serveru s

Když uživatel zadá hodnoty pro novou kategorii a klikne na tlačítko Vložit, dojde k zpětné volání a kterou se vkládání pracovního postupu. První, prvek DetailsView s [ `ItemInserting` události](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserting.aspx) aktivována. Další, ObjectDataSource s `Insert()` je metoda vyvolána, výsledek bude přidán do nového záznamu `Categories` tabulky. Po tomto prvku DetailsView s [ `ItemInserted` události](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserted.aspx) aktivována.

Před ObjectDataSource s `Insert()` vyvolání metody, jsme nutné nejdříve zkontrolovat, že příslušný soubor typy nahraný uživatelem a uložte si brožuru o PDF do systému souborů webového serveru s. Vytvořte obslužnou rutinu události pro prvek DetailsView s `ItemInserting` událostí a přidejte následující kód:


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample6.vb)]

Obslužná rutina události spustí odkazováním `BrochureUpload` FileUpload ovládacího prvku z prvku DetailsView s šablony. Potom Pokud odeslal brožuru rozšíření s nahraného souboru je zkontrolován. Pokud rozšíření není. PDF, pak upozornění se zobrazí, se zrušila, insert a ukončí provádění obslužné rutiny události.

> [!NOTE]
> Spoléhání se na rozšíření s nahraný soubor není sure-fire techniku pro zajištění, že je nahraný soubor dokumentu PDF. Uživatel může mít platný dokument PDF s příponou `.Brochure`, nebo může mít provést bez PDF dokument a je uveden `.pdf` rozšíření. Binární obsah souboru s by bylo potřeba vyšetřit programově více jednoznačně ověří typ souboru. Takové důkladné přístupy, ale jsou často přehnaně; kontroluje se rozšíření stačí pro většinu scénářů.


Jak je popsáno v [nahrávání souborů](uploading-files-vb.md) výukový program, musí věnovat pozornost při ukládání souborů do systému souborů, tak tento jeden uživatel s nahrávání nepřepisuje s jinou. Pro účely tohoto kurzu jsme se pokusí použít stejný název jako nahraný soubor. Pokud už existuje soubor v `~/Brochures` adresář se stejným název tohoto souboru, ale můžeme vám připojte číslo na konci dokud nebude nalezen jedinečný název. Například, pokud uživatel nahraje si brožuru o soubor s názvem `Meats.pdf`, ale již existuje soubor s názvem `Meats.pdf` v `~/Brochures` složky, Změníme názvu uloženého souboru `Meats-1.pdf`. Pokud, který existuje, Zkusíme `Meats-2.pdf`, a tak dále, dokud nebude nalezen jedinečný název souboru.

Následující kód používá [ `File.Exists(path)` metoda](https://msdn.microsoft.com/library/system.io.file.exists.aspx) k určení, zda soubor již existuje s názvem zadaného souboru. Pokud ano, bude pokračovat vyzkoušet nové názvy souborů pro brožura, dokud nebude nalezen žádný konflikt.


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample7.vb)]

Jakmile byl nalezen platný název souboru, soubor je potřeba uložit do systému souborů a prvku ObjectDataSource s `brochurePath``InsertParameter` hodnota musí být aktualizovány tak, aby tento název souboru je zapsána do databáze. Jak jsme viděli v *nahrávání souborů* kurzu se soubor můžete uložit pomocí ovládacího prvku FileUpload s `SaveAs(path)` metody. Chcete-li aktualizovat prvek ObjectDataSource s `brochurePath` parametrů, použijte `e.Values` kolekce.


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample8.vb)]

## <a name="step-7-saving-the-uploaded-picture-to-the-database"></a>Krok 7: Ukládání nahraného obrázku do databáze

K ukládání nahraného obrázku na novém `Categories` záznam, musíme přiřadit odeslaný binární obsah prvku ObjectDataSource s `picture` parametr v prvku DetailsView s `ItemInserting` událostí. Před provedením tohoto přiřazení, ale potřebujeme nejprve se ujistěte, že je nahraný obrázek JPG a ne některé další typ obrázku. Stejně jako v kroku 6 let s můžete zjistit jeho typ nahraných obrázků s příponou.

Zatímco `Categories` tabulky umožňuje `NULL` hodnoty `Picture` sloupec, všechny kategorie aktuálně obsahovat obrázek. Umožní s nutí uživatele zadat obrázek při přidávání nové kategorie prostřednictvím této stránky. Následující kód zkontroluje, ujistěte se, že obrázek je nahraný a má odpovídající rozšíření.


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample9.vb)]

Tento kód by měl být umístěn *před* kódu z kroku 6 tak, že pokud dojde k potížím s nahrání obrázku, obslužné rutiny události bude ukončen, než si brožuru o soubor se uloží do systému souborů.

Za předpokladu, že příslušný soubor se odeslal, přiřadíte odeslaný binární obsah s hodnota parametru obrázku s následující řádek kódu:


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample10.vb)]

## <a name="the-completeiteminsertingevent-handler"></a>Kompletní`ItemInserting`obslužné rutiny události

Pro úplnost je zde `ItemInserting` obslužné rutiny události v celém rozsahu:


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample11.vb)]

## <a name="step-8-fixing-thedisplaycategorypictureaspxpage"></a>Krok 8: Řešení`DisplayCategoryPicture.aspx`stránky

Umožňují s využít k otestování rozhraní vkládání a `ItemInserting` obslužná rutina události, který byl vytvořen za posledních několik kroků. Přejděte `UploadInDetailsView.aspx` stránce prostřednictvím prohlížeče a pokus o přidání kategorie, ale vynechejte obrázek nebo zadejte jiné JPG obrázek nebo brožuru – soubor PDF. Ve všech těchto případech se zobrazí chybová zpráva a pracovní postup vložit zrušena.


[![Upozornění se zobrazí, pokud je odeslána neplatný typ souboru](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image9.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image15.png)

**Obrázek 9**: zprávy A upozornění se zobrazí, pokud je odeslána neplatný typ souboru ([kliknutím ji zobrazíte obrázek v plné velikosti](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image16.png))


Jakmile ověříte, že stránka vyžaduje k nahrání obrázku a získaných oproti očekávaným t-formátu PDF nebo jiných JPG soubory přijmout, přidat novou kategorii s platnou obrázek JPG, když si brožuru o pole necháte prázdné. Po kliknutí na tlačítko pro vložení se bude odeslat zpět na stránku a přibude nový záznam `Categories` tabulku s nahraného obrázku s binární obsah uložen přímo v databázi. Prvku GridView se aktualizuje a zobrazí řádek pro nově přidaná kategorie, ale, jak ukazuje obrázek 10 nový obrázek s kategorie nevykreslí správně.


[![Novou kategorii s, který není zobrazen obrázek](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image10.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image17.png)

**Obrázek 10**: novou kategorii s není zobrazen obrázek ([kliknutím ji zobrazíte obrázek v plné velikosti](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image18.png))


Důvod se nezobrazí nový obrázek je vzhledem k tomu, `DisplayCategoryPicture.aspx` stránka, která vrací obrázek s zadané kategorie je nakonfigurován ke zpracování rastrové obrázky, které mají hlavičku OLE. Tato hlavička 78 bajtů je odebrána z `Picture` sloupec s binární obsah před jejich odesláním zpět do klienta. Ale soubor .jpg, který jsme nahráli nové kategorie neobsahuje toto záhlaví OLE; Proto budou odebrány z binární data bitové kopie s platný, nezbytné bajtů.

Protože jsou nyní obě rastrové obrázky s hlavičkami OLE a formátu JPG využívá v `Categories` tabulky, musíme aktualizovat `DisplayCategoryPicture.aspx` tak, aby hlavičku OLE odstranění původního osm kategorií a obchází odstranění pro novější záznamy kategorie. V následujícím kurzem prozkoumáme jak aktualizovat stávající záznam s image a aktualizujeme všechny obrázky staré kategorie tak, aby byly formátu JPG využívá. Prozatím se však použijte následující kód v `DisplayCategoryPicture.aspx` k odebrání hlaviček OLE pouze pro tyto původní osm kategorií:


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample12.vb)]

Díky této změně obrázek JPG je nyní správně vykreslen v prvku GridView.


[![Obrázky ve formátu JPG pro nové kategorie jsou vykresleny správně](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image11.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image19.png)

**Obrázek 11**: The obrázky ve formátu JPG pro nové kategorie jsou vykresleny správně ([kliknutím ji zobrazíte obrázek v plné velikosti](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image20.png))


## <a name="step-9-deleting-the-brochure-in-the-face-of-an-exception"></a>Krok 9: Odstranění si brožuru o i v případě výjimky

Jedním z problémů v systému souborů webového serveru s ukládání binárních dat je, že odpojení mezi datového modelu a jeho binární data. Proto se pokaždé, když se odstranění záznamu odpovídající binární data v systému souborů musí také odebrat. To můžete uplatnit při vkládání také. Vezměte v úvahu následující scénář: uživatel přidá novou kategorii, zadáním platné obrázek a si brožuru o. Po kliknutí na tlačítko Vložit zpětného odeslání dojde k a DetailsView s `ItemInserting` událost je aktivována, ukládání brožura do systému souborů webového serveru s. Další, ObjectDataSource s `Insert()` je vyvolána metoda, která volá `CategoriesBLL` třída s `InsertWithPicture` metoda, která volá `CategoriesTableAdapter` s `InsertWithPicture` metody.

Nyní, co se stane, pokud se databáze nachází v režimu offline nebo pokud dojde k chybě v `INSERT` příkaz jazyka SQL? Jasně se nezdaří vložit, aby žádný nový řádek kategorie se přidají do databáze. Ale ještě máme si brožuru o nahraný soubor na souborový systém webového serveru s!. Tento soubor je nutné odstranit i v případě výjimku při vkládání pracovního postupu.

Jak je popsáno výše v [zpracování knihoven BLL a výjimek úrovni DAL na stránce ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) kurz, když dojde k výjimce z v rámci hlubin architektura je pronikají do prostřednictvím různých vrstev. V prezentační vrstvě, můžeme určit, pokud došlo k výjimce z prvku DetailsView s `ItemInserted` událostí. Tato obslužná rutina události také poskytuje hodnoty prvku ObjectDataSource s `InsertParameters`. Proto vytvoříme obslužná rutina události `ItemInserted` událost, která zkontroluje, jestli došlo k výjimce a pokud ano, odstraní soubor určený parametrem ObjectDataSource s `brochurePath` parametr:


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample13.vb)]

## <a name="summary"></a>Souhrn

Existuje mnoho kroků, které se musí provádět negace webové rozhraní pro přidání záznamů, které zahrnují binární data. Pokud binární data ukládají přímo do databáze, je pravděpodobné, že je potřeba aktualizovat na architekturu, přidání specifické metody pro zpracování případu, kdy bude vložen binární data. Jakmile se aktualizovala na architekturu, dalším krokem vytvoření vkládání rozhraní, které můžete udělat pomocí prvku DetailsView, který byl přizpůsoben, aby zahrnují FileUpload ovládací prvek pro každé pole binární data. Nahraných dat potom můžete uložit do systému souborů webového serveru s nebo přiřazené parametr zdroje dat v prvku DetailsView s `ItemInserting` obslužné rutiny události.

Ukládání binárních dat do systému souborů vyžaduje více plánování než ukládání dat přímo do databáze. Schéma pojmenování, musí se zvolit předejdete tak přepsání jiného s jeden uživatel s nahrávání. Navíc pár kroků navíc třeba odstranit nahraný soubor, pokud selže vložit databáze.

Nyní je k dispozici možnost přidávat nové kategorie v systému s brožuru a obrázek, ale můžeme odebrat ještě se podívat na tom, jak aktualizovat existující kategorie s binární data nebo jak správně odebrat binární data pro Odstraněná kategorie. V dalším kurzu se podíváme těchto dvou témat.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Dave Gardner Teresy Murphy a Bernadette Leigh. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](displaying-binary-data-in-the-data-web-controls-vb.md)
> [další](updating-and-deleting-existing-binary-data-vb.md)
