---
uid: web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-vb
title: Zobrazení binární Data v dat webové ovládací prvky (VB) | Microsoft Docs
author: rick-anderson
description: V tomto kurzu podíváme na možnosti pro binární data k dispozici na webové stránce, včetně zobrazení souboru bitové kopie a poskytování odkaz 'Stažení' f...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 9201656a-e1c2-4020-824b-18fb632d2925
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 006a4d014b610f3079d7f25e9420f687447a1b26
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30886924"
---
<a name="displaying-binary-data-in-the-data-web-controls-vb"></a>Zobrazení binární Data v ovládacích prvcích webové dat (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_55_VB.exe) nebo [stáhnout PDF](displaying-binary-data-in-the-data-web-controls-vb/_static/datatutorial55vb1.pdf)

> V tomto kurzu podíváme na možnosti pro binární data k dispozici na webové stránce, včetně zobrazení souboru bitové kopie a poskytování odkaz 'Stažení' pro soubor PDF.


## <a name="introduction"></a>Úvod

V předchozím kurzu jsme prozkoumali dvě techniky pro binární data možné přidružit aplikaci s základní datový model a použít ovládacího prvku odesílání souborů při odpovědích k ukládání souborů z prohlížeče do systému souborů webového serveru s. Jsme jste ještě chcete zjistit, jak přidružit nahrané binární data v datovém modelu. To znamená po soubor má byl nahrán a uložit do systému souborů, cesta k souboru musí být uložen v databázi záznamu. Pokud se data ukládají přímo do databáze, pak nahrané binární data nemusí být uloženy do systému souborů, ale musí být vloženy do databáze.

Předtím, než se podíváme na to přidružením data v datovém modelu, ale umožní s první pohled na to, jak zajistit binární data pro koncového uživatele. Prezentace textová data je dostatečně jednoduchá, ale jak by měla předkládané binární data? To závisí, samozřejmě na typ binární data. Pro Image chceme pravděpodobně umožňuje zobrazit obrázek; pro soubory PDF je pravděpodobně vhodnější Microsoft Word dokumenty, soubory ZIP a dalších typů binárních dat, poskytuje odkaz ke stažení.

V tomto kurzu se podíváme na to, jak k dispozici binární data souběžně s jeho přidružené textová data pomocí dat webové ovládací prvky jako GridView a DetailsView. V dalším kurzu jsme budete zapněte naše pozornost přidružení nahrávaný soubor s databází.

## <a name="step-1-providingbrochurepathvalues"></a>Krok 1: Poskytnutí`BrochurePath`hodnoty

`Picture` Sloupec v `Categories` tabulka již obsahuje binární data pro různé kategorie Image. Konkrétně `Picture` sloupec pro každý záznam obsahuje binární obsah, nízké kvality se 16 barev rastrového obrázku. Každé kategorie bitové kopie je 172 pixelů široké a 120 pixelů vysoký a odebírá zhruba 11 KB. Jaké s další, binární obsah `Picture` sloupec obsahuje 78 bajtů [OLE](http://en.wikipedia.org/wiki/Object_Linking_and_Embedding) hlavičky, která musí se odstraní a před zobrazením bitovou kopii. Tyto informace hlavičky nachází, protože databáze Northwind má jeho kořeny v aplikaci Microsoft Access. Binární data v přístup, jsou uložena pomocí datového typu OLE objekt, který se přiřadí k této hlavičce. Prozatím jsme zobrazí postup odebrání hlaviček z těchto bitových kopií nízké kvality, aby bylo možné zobrazit na obrázku. V budoucích kurzu budete využijeme rozhraní pro aktualizace kategorie s `Picture` sloupce a nahraďte tyto rastrové obrázky, které pomocí OLE hlavičky ekvivalentní obrázků JPG bez nepotřebné hlavičky OLE.

V předchozím kurzu jsme viděli, jak pomocí ovládacího prvku odesílání souborů při odpovědích. Proto můžete přejít k tématu a přidat soubory – příručka k systému souborů webového serveru s. To uděláte, ale neaktualizuje `BrochurePath` sloupec v `Categories` tabulky. V dalším kurzu ukážeme, jak to provést, ale nyní je nutné ručně zadat hodnoty pro tento sloupec.

V tomto kurzu s ke stažení najdete sedm – příručka soubory PDF v `~/Brochures` složky, jednu pro každou kategorii kromě ryby. I záměrně vynechán, přidávání ryby – Příručka pro ukazují, jak zpracovávat scénáře, kde není všechny záznamy přidruženy binární data. Chcete-li aktualizovat `Categories` tabulky s těmito hodnotami, klikněte pravým tlačítkem na `Categories` uzlu z Průzkumníka serveru a zvolte Zobrazit Data tabulky. Potom zadejte virtuální cesty k souborům – Příručka pro každou kategorii, která má – příručka, jak ukazuje obrázek 1. Vzhledem k tomu, že neexistuje žádná – Příručka pro kategorii ryby, nechte jeho `BrochurePath` hodnota sloupce s jako `NULL`.


[![Ručně zadejte hodnoty pro sloupec BrochurePath tabulky s kategorií](displaying-binary-data-in-the-data-web-controls-vb/_static/image1.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image1.png)

**Obrázek 1**: ručně zadejte hodnoty položek `Categories` tabulky s `BrochurePath` sloupce ([Kliknutím zobrazit obrázek v plné velikosti](displaying-binary-data-in-the-data-web-controls-vb/_static/image2.png))


## <a name="step-2-providing-a-download-link-for-the-brochures-in-a-gridview"></a>Krok 2: Poskytuje odkaz ke stažení pro brožury v GridView

S `BrochurePath` zadané hodnoty `Categories` tabulky, jsme re připravené k vytvoření GridView, který uvádí každou kategorii spolu s odkazem ke stažení – příručka kategorie s. V kroku 4 rozšiřujeme Tato rutina GridView také zobrazíte kategorie s bitové kopie.

Začněte tím, že přetáhnete GridView z panelu nástrojů na Návrhář `DisplayOrDownloadData.aspx` stránku `BinaryData` složky. Nastavit GridView s `ID` k `Categories` a pomocí inteligentních značek GridView s vyberte pro vytvoření vazby ke zdroji dat nové. Konkrétně navázat jej ObjectDataSource s názvem `CategoriesDataSource` , načte data pomocí `CategoriesBLL` objekt s `GetCategories()` metoda.


[![Vytvořit nový ObjectDataSource s názvem CategoriesDataSource](displaying-binary-data-in-the-data-web-controls-vb/_static/image2.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image3.png)

**Obrázek 2**: vytvoření nové ObjectDataSource s názvem `CategoriesDataSource` ([Kliknutím zobrazit obrázek v plné velikosti](displaying-binary-data-in-the-data-web-controls-vb/_static/image4.png))


[![Konfigurace ObjectDataSource použití třídy CategoriesBLL](displaying-binary-data-in-the-data-web-controls-vb/_static/image3.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image5.png)

**Obrázek 3**: Konfigurace ObjectDataSource pro použití `CategoriesBLL` – třída ([Kliknutím zobrazit obrázek v plné velikosti](displaying-binary-data-in-the-data-web-controls-vb/_static/image6.png))


[![Načtení seznamu kategorií metodou GetCategories()](displaying-binary-data-in-the-data-web-controls-vb/_static/image4.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image7.png)

**Obrázek 4**: načtení seznamu kategorií pomocí `GetCategories()` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](displaying-binary-data-in-the-data-web-controls-vb/_static/image8.png))


Po dokončení průvodce Konfigurace zdroje dat, Visual Studio automaticky přidá BoundField k `Categories` GridView pro `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, a `BrochurePath` `DataColumn` s. Pokračujte a odebrat `NumberOfProducts` BoundField od `GetCategories()` metoda s dotazu není načtení těchto informací. K odebrání `CategoryID` BoundField a přejmenujte `CategoryName` a `BrochurePath` BoundFields `HeaderText` vlastností kategorie a – příručka, v uvedeném pořadí. Po provedení těchto změn, vaše GridView a ObjectDataSource s deklarativní značek by měl vypadat následovně:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample1.aspx)]

Zobrazit tuto stránku prostřednictvím prohlížeče (viz obrázek 5). Každou kategorii osm je uveden. Sedm kategorií s `BrochurePath` hodnoty mají `BrochurePath` hodnoty zobrazené v příslušných BoundField. Ryby, který má `NULL` hodnotu pro jeho `BrochurePath`, zobrazí prázdné buňky.


[![Každá kategorie s název, popis a hodnotu BrochurePath je uveden.](displaying-binary-data-in-the-data-web-controls-vb/_static/image5.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image9.png)

**Obrázek 5**: každou kategorii s název, popis a `BrochurePath` je uvedena hodnota ([Kliknutím zobrazit obrázek v plné velikosti](displaying-binary-data-in-the-data-web-controls-vb/_static/image10.png))


Místo zobrazení textu `BrochurePath` sloupce, chceme vytvořit odkaz – příručka. Chcete-li dosáhnout, odeberte `BrochurePath` BoundField a nahraďte ji metodou HyperLinkField. Nastavit nový s HyperLinkField `HeaderText` vlastnost – příručka, jeho `Text` vlastnost – příručka zobrazení a jeho `DataNavigateUrlFields` vlastnost `BrochurePath`.


![Přidání HyperLinkField pro BrochurePath](displaying-binary-data-in-the-data-web-controls-vb/_static/image6.gif)

**Obrázek 6**: přidejte HyperLinkField pro `BrochurePath`


Sloupec odkazů se přidá do GridView, jak je vidět na obrázku 7. Kliknutím na odkaz zobrazit – příručka se přímo v prohlížeči zobrazit PDF nebo vyzvat uživatele k stažení souboru, v závislosti na tom, jestli je nainstalovaná čtečka PDF a prohlížeč s nastavení.


[![– Příručka kategorie s lze zobrazit kliknutím na odkaz zobrazit – příručka](displaying-binary-data-in-the-data-web-controls-vb/_static/image7.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image11.png)

**Obrázek 7**: kategorie A s – příručka lze zobrazit kliknutím na odkaz zobrazit – příručka ([Kliknutím zobrazit obrázek v plné velikosti](displaying-binary-data-in-the-data-web-controls-vb/_static/image12.png))


[![Kategorie s PDF – příručka se zobrazí.](displaying-binary-data-in-the-data-web-controls-vb/_static/image8.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image13.png)

**Obrázek 8**: kategorii s PDF – příručka se zobrazí ([Kliknutím zobrazit obrázek v plné velikosti](displaying-binary-data-in-the-data-web-controls-vb/_static/image14.png))


## <a name="hiding-the-view-brochure-text-for-categories-without-a-brochure"></a>Skrývání textu – příručka zobrazení kategorií bez – příručka

Jak je vidět na obrázku 7, `BrochurePath` HyperLinkField zobrazí jeho `Text` hodnota vlastnosti (zobrazení – příručka) pro všechny záznamy, bez ohledu na to, jestli se zde s jinou hodnotu než`NULL` hodnota `BrochurePath`. Samozřejmě pokud `BrochurePath` je `NULL`, potom na odkaz se zobrazí jako text, jako je tomu u ryby kategorie (odkazuje zpět na obrázku 7). Místo zobrazení textu – příručka zobrazení, může být dobrý tak, aby měl těchto kategorií bez `BrochurePath` hodnota zobrazí alternativní text, například není k dispozici – příručka.

Chcete-li provést toto chování, je potřeba použít TemplateField, jehož obsah je generovaný prostřednictvím volání metody stránky, který vysílá odpovídající výstup na základě `BrochurePath` hodnotu. Nám nejdřív prozkoumali toto formátování technika zpět v [pomocí TemplateFields v ovládacím prvku GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) kurzu.

Zapnout HyperLinkField do TemplateField výběrem `BrochurePath` HyperLinkField a kliknete na převést toto pole do TemplateField na odkaz v dialogovém okně Upravit sloupce.


![Převést HyperLinkField TemplateField](displaying-binary-data-in-the-data-web-controls-vb/_static/image9.gif)

**Obrázek 9**: převést HyperLinkField TemplateField


Tím se vytvoří TemplateField s `ItemTemplate` obsahující hypertextový odkaz webové řídit, jehož `NavigateUrl` vlastnost je vázána na `BrochurePath` hodnotu. Nahraďte tento kód volání do metody `GenerateBrochureLink`a předejte hodnotu `BrochurePath`:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample2.aspx)]

Dále vytvořte `Protected` metoda v prostředí ASP.NET stránky s názvem třídu s kódem v pozadí `GenerateBrochureLink` , který vrací `String` a přijímá `Object` jako vstupní parametr.


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample3.vb)]

Tato metoda určuje, zda předané `Object` hodnota je databáze `NULL` a pokud ano, vrátí zpráva označující, že chybí kategorii – příručka. Jinak, pokud je `BrochurePath` hodnotu, se zobrazí v hypertextový odkaz s. Všimněte si, že pokud `BrochurePath` hodnota je k dispozici s předaný do [ `ResolveUrl(url)` metoda](https://msdn.microsoft.com/library/system.web.ui.control.resolveurl.aspx). Tato metoda přeloží předané *adresa url*, a nahraďte `~` znak s příslušnou virtuální cestu. Například pokud aplikace se zobrazuje v `/Tutorial55`, `ResolveUrl("~/Brochures/Meats.pdf")` vrátí `/Tutorial55/Brochures/Meat.pdf`.

Obrázek 10 ukazuje na stránku po použití těchto změn. Všimněte si, že ryby kategorie s `BrochurePath` pole nyní zobrazí text není – příručka k dispozici.


[![Dostupné Text ne – příručka se zobrazí u těch kategorií bez – příručka](displaying-binary-data-in-the-data-web-controls-vb/_static/image10.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image15.png)

**Obrázek 10**: textu není – příručka k dispozici se zobrazí u těch kategorií bez – příručka ([Kliknutím zobrazit obrázek v plné velikosti](displaying-binary-data-in-the-data-web-controls-vb/_static/image16.png))


## <a name="step-3-adding-a-web-page-to-display-a-category-s-picture"></a>Krok 3: Přidání na webové stránce zobrazení kategorie s obrázku

Pokud uživatel navštíví stránku ASP.NET, obdrží ASP.NET stránky s HTML. Přijaté HTML je jenom text a neobsahuje žádné binární data. Žádné další binárních dat, například bitové kopie, zvukové soubory, aplikace Macromedia Flash, vložená videa program Windows Media Player a tak dále, existovat jako samostatné prostředky na webovém serveru. Obsahuje odkazy na tyto soubory HTML, ale nezahrnuje skutečný obsah souborů.

Například ve formátu HTML `<img>` element se používá k odkazování obrázek s `src` atribut odkazující na soubor obrázku takto:


[!code-html[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample4.html)]

Pokud prohlížeč obdrží tuto HTML, zadá další žádost na webový server k načtení binární obsah souboru bitové kopie, který se potom zobrazí v prohlížeči. Totéž platí pro všechny binární data. V kroku 2 – Příručka nebyla odeslána do prohlížeče jako součást kód HTML stránky s. Místo toho vykreslené HTML zadat hypertextové odkazy, při kliknutí na, způsobila prohlížeče přímo požadovat dokumentu PDF.

Pokud chcete zobrazit nebo povolit uživatelům stahovat binární data, která se nachází v databázi, musíme vytvořit samostatné webové stránky, která vrací data. Pro naši aplikaci pouze jedno pole binární data s ní uložena přímo v databázi s kategorie obrázek. Proto je třeba na stránce, pokud je volána, vrátí data bitové kopie pro určité kategorie.

Přidat novou stránku ASP.NET do `BinaryData` složku s názvem `DisplayCategoryPicture.aspx`. Když to uděláte, nechte políčko vyberte stránku předlohy nezaškrtnuté. Tato stránka očekává `CategoryID` hodnota v řetězci dotazu a vrací binární data této kategorie s `Picture` sloupce. Vzhledem k tomu, že tato stránka vrátí binární data a nic jiného, to není vyžadováno žádné značky v části HTML. Proto klikněte na kartě Zdroj v levém dolním a odeberte všechny stránky s značky s výjimkou `<%@ Page %>` – direktiva. To znamená `DisplayCategoryPicture.aspx` s deklarativní by měla obsahovat jeden řádek:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample5.aspx)]

Pokud se zobrazí `MasterPageFile` atribut `<%@ Page %>` – direktiva, odeberte ji.

Ve třídě, stránku s kódem v pozadí, přidejte následující kód, který `Page_Load` obslužné rutiny události:


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample6.vb)]

Tento kód začíná čtení v `CategoryID` hodnotu querystring do proměnné s názvem `categoryID`. V dalším kroku jsou načtena data obrázek prostřednictvím volání `CategoriesBLL` třídu s `GetCategoryWithBinaryDataByCategoryID(categoryID)` metoda. Tato data je vrácen do klienta pomocí `Response.BinaryWrite(data)` metoda, ale předtím, než je tomu se říká, `Picture` záhlaví OLE hodnotu s sloupce musí být odebrány. Toho dosahuje tak, že vytvoříte `Byte` pole s názvem `strippedImageData` , bude obsahovat přesně 78 znaky menší než co je v `Picture` sloupce. [ `Array.Copy` Metoda](https://msdn.microsoft.com/library/z50k9bft.aspx) se používá ke zkopírování dat z `category.Picture` začínající na pozici 78 přes k `strippedImageData`.

`Response.ContentType` Určuje vlastnost [typ MIME](http://en.wikipedia.org/wiki/MIME) obsahu nevrátila tak, aby prohlížeč umí vykreslit ho. Vzhledem k tomu `Categories` tabulky s `Picture` sloupec rastrový obrázek, zobrazí typ MIME rastrový obrázek zde (bitovou kopii nebo bmp). Pokud vynecháte typ MIME, většina prohlížečů bude stále umožňuje zobrazit obrázek správně vzhledem k tomu, že můžete odvození typu na základě obsahu binární data bitové kopie souboru s. Nicméně je doporučeno zahrnout MIME s zadejte, pokud je to možné. Najdete v článku [webu s Internet Assigned Numbers Authority](http://www.iana.org/) pro úplný seznam všech [typy MIME média](http://www.iana.org/assignments/media-types/).

Navštivte stránky mohou zobrazit tuto stránku vytvořit, obrázku určité kategorie s `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Obrázek 11 ukazuje na nápoje kategorie s obrázku, který lze zobrazit z `DisplayCategoryPicture.aspx?CategoryID=1`.


[![Kategorie nápoje s, se zobrazí obrázek](displaying-binary-data-in-the-data-web-controls-vb/_static/image11.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image17.png)

**Obrázek 11**: kategorii nápoje s zobrazí obrázek ([Kliknutím zobrazit obrázek v plné velikosti](displaying-binary-data-in-the-data-web-controls-vb/_static/image18.png))


Pokud při návštěvě `DisplayCategoryPicture.aspx?CategoryID=categoryID`, dojde k výjimce, který čte nelze vrátit objekt typu 'Hodnotu ' System.DBNull na typ 'System.Byte []', dva postupy, které může být příčinou to. Nejdřív `Categories` tabulky s `Picture` sloupec povolit `NULL` hodnoty. `DisplayCategoryPicture.aspx` Stránky, ale předpokládá existuje jinou hodnotu než`NULL` hodnota. `Picture` Vlastnost `CategoriesDataTable` nelze přistupovat přímo, pokud má `NULL` hodnotu. Pokud chcete povolit `NULL` hodnoty `Picture` sloupce d chcete zahrnout následující podmínka:


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample7.vb)]

Ve výše uvedeném kódu předpokládá tom, že s některé bitové kopie souboru s názvem `NoPictureAvailable.gif` v `Images` složky, kterou chcete zobrazit pro tyto kategorie bez obrázku.

Tato výjimka může také dojít, pokud `CategoriesTableAdapter` s `GetCategoryWithBinaryDataByCategoryID` metoda s `SELECT` příkaz vrátil zpět do hlavní dotazu s seznam sloupců, které může dojít, pokud používáte ad-hoc příkazů SQL a jste již znovu spusťte Průvodce nastavením TableAdapter s hlavní dotazu. Zkontrolujte, ujistěte se, že `GetCategoryWithBinaryDataByCategoryID` metoda s `SELECT` stále zahrnuje příkaz `Picture` sloupce.

> [!NOTE]
> Pokaždé, když `DisplayCategoryPicture.aspx` je navštívili, databázi přistupuje a data zadaná kategorie s obrázku se vrátí. Pokud se ještě t kategorie s obrázek změnila vzhledem k tomu, že se má zobrazit poslední uživatele, ale toto je nevyužité úsilí. Naštěstí protokolu HTTP umožňuje *podmíněného získá*. Pomocí podmíněného GET, pošle klient vytvoření požadavku HTTP společně [ `If-Modified-Since` hlavičky protokolu HTTP](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) datum a čas klienta poslední načíst tento prostředek z webového serveru, který poskytuje. Je-li obsah se nezměnila, protože toto nastavení zadané datum, webový server odpoví [nedojde ke změně stavový kód (304)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) a forgo zpět zasílání daného obsahu požadovaný prostředek s. Zbavuje stručně řečeno, tato technika kterou přináší nutnost webový server z má odeslat zpět obsahu pro prostředek, pokud se nezměnil od klienta posledního použití.


Implementovat toto chování však vyžaduje, abyste přidali `PictureLastModified` sloupec, který se `Categories` tabulky k zachycení, kdy `Picture` sloupec poslední aktualizace a zároveň i kód zkontrolujte `If-Modified-Since` záhlaví. Další informace o `If-Modified-Since` záhlaví a podmíněného pracovní postup GET, najdete v části [podmíněného GET protokolu HTTP pro hackery RSS](http://fishbowl.pastiche.org/2002/10/21/http_conditional_get_for_rss_hackers) a [A hlubší podívejte se na provádění požadavků HTTP na stránku ASP.NET](http://aspnet.4guysfromrolla.com/articles/122204-1.aspx).

## <a name="step-4-displaying-the-category-pictures-in-a-gridview"></a>Krok 4: Zobrazení obrázků kategorie v GridView

Teď, když máme na webové stránce zobrazení určité kategorie s obrázku, jsme můžete zobrazit pomocí [ovládací prvek webu Image](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/image.aspx) nebo HTML `<img>` element odkazující na `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Obrázky, jehož adresa URL je určen podle data databáze lze zobrazit v GridView nebo DetailsView pomocí ImageField. Obsahuje ImageField `DataImageUrlField` a `DataImageUrlFormatString` vlastnosti, které fungují jako HyperLinkField s `DataNavigateUrlFields` a `DataNavigateUrlFormatString` vlastnosti.

Umožní s posílení `Categories` GridView v `DisplayOrDownloadData.aspx` přidáním ImageField pro zobrazení každého kategorie s obrázku. Stačí přidat ImageField a nastavit jeho `DataImageUrlField` a `DataImageUrlFormatString` vlastnosti, které chcete `CategoryID` a `DisplayCategoryPicture.aspx?CategoryID={0}`, v uvedeném pořadí. Tím se vytvoří rutina GridView sloupec, který vykreslí `<img>` element jejichž `src` atribut odkazy `DisplayCategoryPicture.aspx?CategoryID={0}`, kde je {0} nahrazena GridView řádek s `CategoryID` hodnotu.


![Přidat ImageField do GridView](displaying-binary-data-in-the-data-web-controls-vb/_static/image12.gif)

**Obrázek 12**: přidejte ImageField do GridView


Po přidání ImageField, deklarativní syntaxi s GridView by měl vypadat jako soothe následující:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample8.aspx)]

Chcete-li zobrazit tuto stránku prostřednictvím prohlížeče chvíli trvat. Všimněte si, jak každý záznam nyní zahrnuje obrázku pro kategorii.


[![Kategorie s obrázku se zobrazí pro každý řádek](displaying-binary-data-in-the-data-web-controls-vb/_static/image13.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image19.png)

**Obrázek 13**: kategorii s zobrazí obrázek pro každý řádek ([Kliknutím zobrazit obrázek v plné velikosti](displaying-binary-data-in-the-data-web-controls-vb/_static/image20.png))


## <a name="summary"></a>Souhrn

V tomto kurzu jsme se zaměřili na tom, jak binární data k dispozici. Jak se data zobrazí závisí na typu dat. – Příručka soubory PDF, jsme nabízená uživatele – příručka zobrazení odkaz, který, po kliknutí na trvalo uživatele přímo do souboru PDF. Pro obrázek s kategorie jsme nejprve vytvořit stránku, kterou chcete načíst a vrátit binární data z databáze a pak použít této stránce zobrazení obrázku s každou kategorii v GridView.

Nyní který jsme sunout podívat, jak zobrazit binárních dat, můžeme znovu připravena k prozkoumání jak provést vložení, aktualizace a odstranění v databázi s binární data. V dalším kurzu podíváme přidružení nahrávaný soubor k jeho odpovídající záznam v databázi. V tomto kurzu potom uvidíme, jak aktualizovat existující binární data a také jak odstranit binární data, když dojde k odebrání jeho přidružený záznam.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři v tomto kurzu se Teresy Murphy a Dave Gardner. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](uploading-files-vb.md)
> [další](including-a-file-upload-option-when-adding-a-new-record-vb.md)
