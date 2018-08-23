---
uid: web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-cs
title: Zobrazení binárních dat ve webových dat ovládací prvky (C#) | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu jsme podívejte se na možnosti prezentovat binární data na webové stránce, včetně zobrazení souboru bitové kopie a poskytování odkaz 'Ke stažení' f...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: 5cbeb9f8-5f92-4ba8-87ae-0b4d460ae6d4
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 50d7f8eceb4772c628f7f6ef71f110de03dd9348
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755970"
---
<a name="displaying-binary-data-in-the-data-web-controls-c"></a>Zobrazení binárních dat ve webových ovládacích prvcích dat (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_55_CS.exe) nebo [stahovat PDF](displaying-binary-data-in-the-data-web-controls-cs/_static/datatutorial55cs1.pdf)

> V tomto kurzu jsme podívejte se na možnosti prezentovat binární data na webové stránce, včetně zobrazení souboru bitové kopie a poskytování odkaz 'Ke stažení' soubor PDF.


## <a name="introduction"></a>Úvod

V předchozím kurzu jsme prozkoumali dvě techniky pro přidružení aplikaci s základní datový model binární data a použít ovládací prvek FileUpload k nahrání souborů z prohlížeče do systému souborů webového serveru s. Jsme ve ještě se dozvíte, jak přidružit nahrané binárních dat s datovým modelem. To znamená, že po souboru je nahraný a uloží do systému souborů, cesta k souboru musí být uložen v záznamu v příslušné databázi. Pokud data ukládají přímo v databázi, potom nahraný binárních dat nemusí být uloží do systému souborů, ale musí být vloženy do databáze.

Předtím, než se podíváme na data přidružení datový model, ale umožní s nejdřív se podívejte na tom, jak koncovým uživatelům poskytnout binární data. Nabízí ten samý textových dat je dostatečně jednoduchá, ale jak by měla předávat binární data? Závisí, samozřejmě, typ binární data. Pro Image jsme pravděpodobně chtít zobrazit obrázek; pro soubory PDF dokumentů aplikace Microsoft Word, soubory ZIP a jiné typy binárních dat, poskytuje odkaz ke stažení je pravděpodobně vhodnější.

V tomto kurzu se podíváme na to, jak data můžete prezentovat tak binární společně s jeho přidružené textových dat pomocí data webové ovládací prvky jako ovládacími prvky GridView a prvku DetailsView. V dalším kurzu jsme vám zapnout pozornost na přidružení nahraného souboru databáze.

## <a name="step-1-providingbrochurepathvalues"></a>Krok 1: Poskytnutí`BrochurePath`hodnoty

`Picture` Sloupec `Categories` tabulka již obsahuje binární data pro různé kategorie Image. Konkrétně `Picture` sloupec pro každý záznam obsahuje binární obsah, nízké kvality se 16 barev rastrový obrázek. Každá kategorie image je 172 pixelů široký a 120 pixelů na výšku a využívá přibližně 11 KB. Jaké s více, binární obsah `Picture` sloupec obsahuje 78 bajtů [OLE](http://en.wikipedia.org/wiki/Object_Linking_and_Embedding) hlavičku, která musí být odebrána před zobrazením na obrázku. Tyto informace hlavičky je k dispozici, protože databáze Northwind má jeho kořenových adresářů v aplikaci Microsoft Access. V přístup binární data se ukládají pomocí datového typu objektu OLE, který se přiřadí k této hlavičce. Prozatím se podíváme postupy odebrání hlaviček z těchto imagí nízké kvalitě, aby bylo možné zobrazit obrázek. V budoucích kurzu vytvoříme rozhraní pro aktualizace kategorie s `Picture` sloupce a nahradit tyto rastrové obrázky, které používají hlavičky OLE s ekvivalentní obrázky ve formátu JPG bez zbytečných záhlaví OLE.

V předchozím kurzu jsme viděli, jak pomocí ovládacího prvku FileUpload. Můžete proto pokračujte a přidat si brožuru o soubory do systému souborů webového serveru s. Tak učiníte, ale neaktualizuje `BrochurePath` sloupec `Categories` tabulky. V dalším kurzu uvidíme, jak to provést, ale teď potřebujeme ručně zadat hodnoty pro tento sloupec.

V tomto kurzu s ke stažení najdete sedm souborů PDF brožura v `~/Brochures` složky, jeden pro každou z kategorií s výjimkou ryby. Můžu záměrně vynechán, přidání brožuru ryby si ukážeme, jak zvládnout scénáře, ve kterém mají všechny záznamy přidružené binární data. Chcete-li aktualizovat `Categories` tabulky s těmito hodnotami, klikněte pravým tlačítkem na `Categories` uzlu z Průzkumníka serveru a zvolte možnost zobrazit Data tabulky. Zadejte virtuální cesty k souborům – Příručka pro každou kategorii, která má brožuru, jak ukazuje obrázek 1. Protože neexistuje žádný – Příručka pro kategorii ryby, nechte své `BrochurePath` hodnota sloupce s jako `NULL`.


[![Ručně zadejte hodnoty pro sloupec BrochurePath tabulky s kategorií](displaying-binary-data-in-the-data-web-controls-cs/_static/image1.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image1.png)

**Obrázek 1**: ručně zadejte hodnoty pro `Categories` tabulky s `BrochurePath` sloupec ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-binary-data-in-the-data-web-controls-cs/_static/image2.png))


## <a name="step-2-providing-a-download-link-for-the-brochures-in-a-gridview"></a>Krok 2: Poskytnutí odkaz ke stažení pro brožury v GridView

S `BrochurePath` zadané hodnoty `Categories` tabulku, můžeme znovu připravený k vytvoření prvku GridView, který obsahuje seznam jednotlivých kategorií spolu s odkazem ke stažení si brožuru o kategorie s. V kroku 4 rozšíříme tohoto ovládacího prvku GridView a také zobrazte obrázek kategorie s.

Začněte tím, že přetažením z panelu nástrojů na Návrhář GridView `DisplayOrDownloadData.aspx` stránku `BinaryData` složky. Nastavit prvek GridView s `ID` k `Categories` a prostřednictvím inteligentních značek GridView s tlačítko pro vytvoření vazby ke zdroji dat nový. Konkrétně svázat ObjectDataSource s názvem `CategoriesDataSource` načítající data s využitím `CategoriesBLL` objektu s `GetCategories()` metody.


[![Vytvoření nového prvku ObjectDataSource s názvem CategoriesDataSource](displaying-binary-data-in-the-data-web-controls-cs/_static/image2.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image3.png)

**Obrázek 2**: vytvoření nového prvku ObjectDataSource s názvem `CategoriesDataSource` ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-binary-data-in-the-data-web-controls-cs/_static/image4.png))


[![Konfigurace ObjectDataSource pomocí třídy CategoriesBLL](displaying-binary-data-in-the-data-web-controls-cs/_static/image3.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image5.png)

**Obrázek 3**: Konfigurace ObjectDataSource k použití `CategoriesBLL` třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-binary-data-in-the-data-web-controls-cs/_static/image6.png))


[![Načíst seznam kategorií pomocí GetCategories() – metoda](displaying-binary-data-in-the-data-web-controls-cs/_static/image4.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image7.png)

**Obrázek 4**: načíst seznam kategorií pomocí `GetCategories()` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-binary-data-in-the-data-web-controls-cs/_static/image8.png))


Po dokončení Průvodce nakonfigurovat zdroj dat, sada Visual Studio automaticky přidá vlastnost BoundField k `Categories` GridView pro `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, a `BrochurePath` `DataColumn` s. Pokračujte a odebrat `NumberOfProducts` Vlastnost BoundField od `GetCategories()` metody s dotazu nejsou tyto informace načíst. Odstranit také `CategoryID` Vlastnost BoundField a přejmenovat `CategoryName` a `BrochurePath` BoundFields `HeaderText` vlastnosti do kategorií a – příručka, v uvedeném pořadí. Po provedení těchto změn vašeho ovládacího prvku GridView a prvku ObjectDataSource s deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample1.aspx)]

Zobrazení této stránky prostřednictvím prohlížeče (viz obrázek 5). Každý osm kategorií je uvedený. Sedm kategorií s `BrochurePath` hodnoty mají `BrochurePath` hodnoty zobrazené v příslušných Vlastnost BoundField. Ryby, který má `NULL` hodnotu pro jeho `BrochurePath`, zobrazí na prázdnou buňku.


[![Je uvedená každá kategorie s název, popis a hodnotu BrochurePath](displaying-binary-data-in-the-data-web-controls-cs/_static/image5.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image9.png)

**Obrázek 5**: s každou kategorii název, popis, a `BrochurePath` hodnota uvedená ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-binary-data-in-the-data-web-controls-cs/_static/image10.png))


Místo zobrazování textu `BrochurePath` sloupce, chceme vytvořit odkaz na brožura. Chcete-li to provést, odeberte `BrochurePath` Vlastnost BoundField a nahraďte ji metodou HyperLinkField. Nastavte nový s HyperLinkField `HeaderText` vlastnost brožuru, na jeho `Text` vlastnost si brožuru o zobrazení a jeho `DataNavigateUrlFields` vlastnost `BrochurePath`.


![Přidat HyperLinkField pro BrochurePath](displaying-binary-data-in-the-data-web-controls-cs/_static/image6.gif)

**Obrázek 6**: Přidat HyperLinkField pro `BrochurePath`


Sloupec odkazů se přidá do prvku GridView, jak je vidět na obrázku 7. Kliknutím na odkaz si brožuru o zobrazení se zobrazí přímo v prohlížeči PDF nebo vyzvat uživatele ke stažení souboru, v závislosti na tom, jestli je nainstalovaná čtečka PDF a prohlížeč s nastavení.


[![Brožura s kategorie lze zobrazit kliknutím na odkaz si brožuru o zobrazení](displaying-binary-data-in-the-data-web-controls-cs/_static/image7.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image11.png)

**Obrázek 7**: kategorie s si brožuru o lze zobrazit kliknutím na odkaz zobrazit si brožuru o ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-binary-data-in-the-data-web-controls-cs/_static/image12.png))


[![Zobrazí se kategorie s si brožuru o PDF](displaying-binary-data-in-the-data-web-controls-cs/_static/image8.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image13.png)

**Obrázek 8**: The kategorie s se zobrazí si brožuru o PDF ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-binary-data-in-the-data-web-controls-cs/_static/image14.png))


## <a name="hiding-the-view-brochure-text-for-categories-without-a-brochure"></a>Skrývání textu si brožuru o zobrazení kategorií bez brožuru

Jak je vidět na obrázku 7, `BrochurePath` HyperLinkField zobrazí jeho `Text` hodnota vlastnosti (zobrazení brožura) pro všechny záznamy, bez ohledu na to, jestli se tam s non -`NULL` hodnotu pro `BrochurePath`. Samozřejmě pokud `BrochurePath` je `NULL`, potom na odkaz se zobrazí jako text, stejně jako v případě ryby kategorie (vrátit zpět k obrázek 7). Místo zobrazování textu si brožuru o zobrazení, může být dobré si tyto kategorie bez `BrochurePath` hodnotu zobrazit některé alternativní text, jako je k dispozici si brožuru o č.

Aby bylo možné poskytovat toto chování, musíme použít na pole TemplateField, jejíž obsah je generován prostřednictvím volání metody stránky, který vysílá odpovídající výstup na základě `BrochurePath` hodnotu. Nejprve Prozkoumali jsme toto formátování techniku zpátky [použití vlastností TemplateField v ovládacím prvku GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) kurzu.

Proměnit HyperLinkField TemplateField tak, že vyberete `BrochurePath` HyperLinkField a potom kliknete na převést toto pole na pole TemplateField na odkaz v dialogovém okně Upravit sloupce.


![Převést HyperLinkField TemplateField](displaying-binary-data-in-the-data-web-controls-cs/_static/image9.gif)

**Obrázek 9**: HyperLinkField převést na pole TemplateField


Tím se vytvoří TemplateField s `ItemTemplate` obsahující hypertextový odkaz webové ovládací prvek, jehož `NavigateUrl` vlastnost je vázána na `BrochurePath` hodnotu. Nahraďte tento kód pomocí volání metody `GenerateBrochureLink`a předejte hodnotu `BrochurePath`:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample2.aspx)]

Dále vytvořte `protected` stránce metoda v ASP.NET s použití modelu code-behind třídu s názvem `GenerateBrochureLink` , která vrací `string` a přijímá `object` jako vstupní parametr.


[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample3.cs)]

Tato metoda určuje, zda předaným `object` hodnota je databáze `NULL` a pokud ano, vrátí se zpráva oznamující, že chybí kategorie brožuru. Jinak, pokud je `BrochurePath` hodnotu, se zobrazí v hypertextový odkaz s. Všimněte si, že pokud `BrochurePath` hodnotu prezentovat je předaných do [ `ResolveUrl(url)` metoda](https://msdn.microsoft.com/library/system.web.ui.control.resolveurl.aspx). Tato metoda překládá předaný *url*a nahraďte `~` znak s příslušnou virtuální cestou. Například, pokud aplikace je kořenovým adresářem v `/Tutorial55`, `ResolveUrl("~/Brochures/Meats.pdf")` vrátí `/Tutorial55/Brochures/Meat.pdf`.

Obrázek 10 ukazuje na stránku, až tyto změny se použily. Všimněte si, že ryby kategorie s `BrochurePath` pole teď zobrazuje text bez – příručka k dispozici.


[![Text bez si brožuru o dostupná se zobrazí pro tyto kategorie bez si brožuru o](displaying-binary-data-in-the-data-web-controls-cs/_static/image10.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image15.png)

**Obrázek 10**: Text bez si brožuru o dostupná se zobrazí pro tyto kategorie bez brožura ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-binary-data-in-the-data-web-controls-cs/_static/image16.png))


## <a name="step-3-adding-a-web-page-to-display-a-category-s-picture"></a>Krok 3: Přidání webové stránky k zobrazení obrázku kategorie s

Když uživatel navštíví stránku ASP.NET, obdrží tento uživatel ASP.NET stránky s HTML. Přijatý kód HTML je jenom text a neobsahuje žádné binární data. Žádná další binární data, jako jsou obrázky, zvukové soubory, aplikace Macromedia Flash, vložený Windows Media Player videa a tak dále, existují jako samostatné prostředky na webovém serveru. Obsahuje odkazy na tyto soubory HTML, ale nezahrnuje skutečný obsah souborů.

Například ve formátu HTML `<img>` prvek slouží jako odkaz obrázek s `src` atribut odkazující na soubor obrázku takto:


[!code-html[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample4.html)]

Když prohlížeč obdrží HTML, zadá další žádost na webový server načíst binární obsah souboru obrázku, která se pak zobrazí v prohlížeči. Totéž platí pro všechny binární data. V kroku 2 nebyl brožura odešlou do prohlížeče jako součást značky HTML stránky s. Místo toho zobrazený HTML k dispozici hypertextové odkazy, po kliknutí na způsobila prohlížeč, aby přímo požadovat dokumentu PDF.

Pokud chcete zobrazit nebo povolit uživatelům stahovat binární data, která se nachází v databázi, potřebujeme vytvořit samostatnou webovou stránku, která vrací data. Pro naši aplikaci tam s pouze jeden binární datové pole uložen přímo v databázi s kategorie obrázek. Proto potřebujeme stránku, která při volání vrátí obrazová data pro určitou kategorii.

Přidejte novou stránku ASP.NET `BinaryData` složku s názvem `DisplayCategoryPicture.aspx`. Pokud tak učiníte, nechte na hlavní stránce vyberte zaškrtávací políčko nezaškrtnuté. Očekává, že tuto stránku `CategoryID` hodnoty v řetězci dotazu a vrátí binárních dat této kategorie s `Picture` sloupce. Vzhledem k tomu, že tato stránka vrátí binárních dat a nic jiného, není nutné žádné značky v oddílu HTML. Proto klikněte na kartě Zdroj v levém dolním rohu a odebrat všechny značky stránky s s výjimkou `<%@ Page %>` směrnice. To znamená `DisplayCategoryPicture.aspx` s deklarativní by měl obsahovat jeden řádek:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample5.aspx)]

Pokud se zobrazí `MasterPageFile` atribut `<%@ Page %>` směrnice, odeberte ji.

Ve třídě použití modelu code-behind stránky s přidejte následující kód, který `Page_Load` obslužné rutiny události:


[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample6.cs)]

Tento kód spustí, přečtěte si téma v `CategoryID` hodnotu řetězce dotazu do proměnné s názvem `categoryID`. V dalším kroku se obrázek data načítají prostřednictvím volání `CategoriesBLL` třída s `GetCategoryWithBinaryDataByCategoryID(categoryID)` metody. Tato data je vrácen do klienta pomocí `Response.BinaryWrite(data)` metody, ale předtím, než tento postup se nazývá, `Picture` záhlaví sloupce hodnoty s OLE se musí odebrat. To lze provést tak, že vytvoříte `byte` pole s názvem `strippedImageData` , který bude obsahovat přesně 78 znaky menší než co je v `Picture` sloupce. [ `Array.Copy` Metoda](https://msdn.microsoft.com/library/z50k9bft.aspx) se použije ke zkopírování dat z `category.Picture` začíná na pozici 78 přes se `strippedImageData`.

`Response.ContentType` Určuje vlastnost [typ MIME](http://en.wikipedia.org/wiki/MIME) obsahu se vrací tak, aby prohlížeč ví, jak ji vykreslit. Protože `Categories` tabulky s `Picture` rastrový obrázek je sloupec, slouží rastrového obrázku nastaven typ MIME tady (image/bmp). Vynecháte-li typ MIME, většina prohlížečů se stále zobrazí obrázek správně vzhledem k tomu, že odvození typu na základě obsahu binární data bitové kopie souboru s. Ale je vhodné zahrnout MIME s zadejte, pokud je to možné. Najdete v článku [webu Internet Assigned Numbers Authority](http://www.iana.org/) pro úplný seznam všech [typů MIME médií](http://www.iana.org/assignments/media-types/).

Pomocí této stránky vytvořené, lze zobrazit obrázek určité kategorie s návštěvou `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Obrázku 11 můžete vidět nápoje obrázek kategorie s, který si můžete prohlížet `DisplayCategoryPicture.aspx?CategoryID=1`.


[![Kategorie nápoje s, se zobrazí obrázek](displaying-binary-data-in-the-data-web-controls-cs/_static/image11.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image17.png)

**Obrázek 11**: The kategorie nápoje s se zobrazí obrázek ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-binary-data-in-the-data-web-controls-cs/_static/image18.png))


Pokud při návštěvě `DisplayCategoryPicture.aspx?CategoryID=categoryID`, obdržíte výjimku, která čte nelze přetypovat objekt typu "hodnotu System.DBNull' na typ System.Byte [], existují dvě věci, které mohou být příčinou to. Nejprve je potřeba `Categories` tabulky s `Picture` sloupec nepovoluje `NULL` hodnoty. `DisplayCategoryPicture.aspx` Stránky, ale předpokládá se non -`NULL` hodnoty, které jsou k dispozici. `Picture` Vlastnost `CategoriesDataTable` nelze přistupovat přímo, pokud má `NULL` hodnotu. Pokud chcete povolit `NULL` hodnoty `Picture` sloupce, d chcete zahrnout následující podmínky:


[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample7.cs)]

Výše uvedený kód předpokládá tom, že s některé image soubor s názvem `NoPictureAvailable.gif` v `Images` složku, která se má zobrazit pro tyto kategorie bez obrázku.

Tato výjimka je může také tehdy, když `CategoriesTableAdapter` s `GetCategoryWithBinaryDataByCategoryID` metody s `SELECT` příkaz má vrátit zpět do hlavního dotazu s seznamu sloupců, které může dojít, pokud používáte SQL příkazy ad-hoc a jste již znovu spusťte Průvodce pro TableAdapter s Hlavní dotaz. Zaškrtněte, pokud chcete zajistit, aby `GetCategoryWithBinaryDataByCategoryID` metody s `SELECT` příkaz stále zahrnuje i `Picture` sloupce.

> [!NOTE]
> Pokaždé, když `DisplayCategoryPicture.aspx` je navštívili, databázi přistupuje a vrátí data obrázku s zadané kategorie. Pokud kategorie s obrázek nemá t změněn uživatel má naposledy zobrazené ji, ale je to plýtvání úsilí. Naštěstí HTTP umožňuje *podmíněné získá*. Pomocí podmíněného GET, odešle klientovi provádějícímu žádost HTTP společně [ `If-Modified-Since` hlavičky protokolu HTTP](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) , který obsahuje datum a čas klienta posledního načtení tohoto prostředku z webového serveru. Pokud obsah se nezměnil, protože tento parametr zadán datum, webový server může odpovědět [nedojde ke změně stavový kód (304)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) a forgo odesílá zpět požadovaný prostředek s obsahem. Stručně řečeno tento postup přišla webový server nebudou muset odeslat zpět obsah pro prostředek, pokud ho nebyl změněn od klienta posledního použití.


K implementaci tohoto chování však vyžaduje, abyste přidali `PictureLastModified` sloupec, který se `Categories` tabulky k zachycení, kdy `Picture` sloupce došlo k poslední aktualizaci a také kód pro kontrolu `If-Modified-Since` záhlaví. Další informace o `If-Modified-Since` záhlaví a podmíněné pracovní postup GET, najdete v části [podmíněné GET protokolu HTTP pro hackery RSS](http://fishbowl.pastiche.org/2002/10/21/http_conditional_get_for_rss_hackers) a [A hlouběji podívejte se na provádění požadavků HTTP na stránce ASP.NET](http://aspnet.4guysfromrolla.com/articles/122204-1.aspx).

## <a name="step-4-displaying-the-category-pictures-in-a-gridview"></a>Krok 4: Zobrazení obrázků kategorie v GridView

Když teď máme webové stránky k zobrazení určité kategorie s obrázku, můžeme pomocí Zobrazit [ovládací prvek Obrázek webu](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/image.aspx) nebo HTML `<img>` element odkazující na `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Obrázky, jehož adresa URL se určuje podle dat z databáze lze zobrazit v prvku GridView nebo pomocí ImageField prvku DetailsView. Třídy ImageField obsahuje `DataImageUrlField` a `DataImageUrlFormatString` vlastnosti, které fungují jako HyperLinkField s `DataNavigateUrlFields` a `DataNavigateUrlFormatString` vlastnosti.

Umožní s rozšířit `Categories` GridView v `DisplayOrDownloadData.aspx` přidáním ImageField zobrazíte všechny kategorie s obrázky. Jednoduše přidejte třídy ImageField a nastavte jeho `DataImageUrlField` a `DataImageUrlFormatString` vlastností `CategoryID` a `DisplayCategoryPicture.aspx?CategoryID={0}`v uvedeném pořadí. Tím se vytvoří, který vykreslí sloupce GridView `<img>` elementu jehož `src` atribut odkazy `DisplayCategoryPicture.aspx?CategoryID={0}`, kde {0} nahradí řádky GridView s `CategoryID` hodnotu.


![Přidat ImageField do prvku GridView.](displaying-binary-data-in-the-data-web-controls-cs/_static/image12.gif)

**Obrázek 12**: přidejte ImageField do prvku GridView.


Po přidání třídy ImageField, vaše GridView s deklarativní syntaxe by měl vypadat jako soothe následující:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample8.aspx)]

Za chvíli zobrazení této stránky prostřednictvím prohlížeče. Všimněte si, jak každý záznam nyní obsahuje obrázek pro kategorii.


[![Zobrazí se kategorie s obrázek pro každý řádek](displaying-binary-data-in-the-data-web-controls-cs/_static/image13.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image19.png)

**Obrázek 13**: The kategorie s se zobrazí obrázek pro každý řádek ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-binary-data-in-the-data-web-controls-cs/_static/image20.png))


## <a name="summary"></a>Souhrn

V tomto kurzu jsme se zaměřili na o představení binární data. Jak se data zobrazují závisí na typu dat. Brožura souborů PDF, jsme nabízeli uživatel brožuru zobrazení odkaz, který po kliknutí na trvalo uživatele přímo do souboru PDF. Obrázek s kategorií jsme poprvé vytvořena stránka k načtení a vrátit binární data z databáze a pak použít tuto stránku zobrazení obrázku s každou kategorii v GridView.

Nyní, který jsme ve podívali se na tom, jak zobrazit binárních dat, můžeme znovu připravený k prozkoumání jak provést vložení, aktualizace a odstranění v databázi s binárními daty. V dalším kurzu podíváme na tom, jak přidružit nahraného souboru jeho odpovídající záznam v databázi. V tomto kurzu potom uvidíme, jak aktualizovat stávajících binárních dat, jakož i jak odstranit binárních dat, pokud její přidružený záznam se odebere.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Teresy Murphy a Dave Gardner. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](uploading-files-cs.md)
> [další](including-a-file-upload-option-when-adding-a-new-record-cs.md)
