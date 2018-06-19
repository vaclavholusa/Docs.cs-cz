---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: Implementace mapování scénáře pomocí rozhraní AJAX | Microsoft Docs
author: microsoft
description: Krok 11 ukazuje, jak integrovat podporu mapování AJAX do naší aplikaci NerdDinner uživatelům, kteří jsou vytváření, úpravy nebo zobrazení večeří zobrazíte l...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 4b3f1e46886c4c1f054e43768b0a44695d71bf09
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872712"
---
<a name="use-ajax-to-implement-mapping-scenarios"></a>Implementace mapování scénáře pomocí rozhraní AJAX
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Toto je krok 11 bezplatný [kurz aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , nevystavíte slabé stránky zabezpečení – prostřednictvím postup sestavení malá, ale dokončení, webové aplikace pomocí ASP.NET MVC 1.
> 
> Krok 11 ukazuje, jak integrovat do naší aplikaci NerdDinner uživatelům, kteří jsou vytváření, úpravy nebo zobrazení večeří zobrazíte umístění večeře graficky podporu mapování AJAX.
> 
> Pokud používáte ASP.NET MVC 3, doporučujeme provedením [získávání spuštěna s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Hudba úložiště](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.


## <a name="nerddinner-step-11-integrating-an-ajax-map"></a>NerdDinner krok 11: Integrace mapování AJAX

Nyní můžete vytočit naše aplikace trochu vizuálně skvělé integrací podporu mapování AJAX. Tato akce povolí vytváření, úpravy nebo zobrazení večeří zobrazíte umístění večeře graficky uživatelům.

### <a name="creating-a-map-partial-view"></a>Vytváření částečné zobrazení mapy

Budeme používat funkce, mapování na několika místech v naší aplikaci. Naše kód suchého zapouzdříme běžné funkce mapy v rámci jedné částečné šablonu, která jsme můžete znovu použít na více akce kontroleru a zobrazení. Jsme budete název toto částečné zobrazení "map.ascx" a vytvořte ji v adresáři \Views\Dinners.

Můžeme vytvořit map.ascx částečné pravým tlačítkem myši na adresář \Views\Dinners a zvolením Add -&gt;příkazu v nabídce zobrazení. Jsme budete název zobrazení "Map.ascx", zkontrolujte jako částečné zobrazení a znamenat, že přidáme předáváme třídu modelu silného typu "večeři":

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

Když kliknete na tlačítko "Přidat" bude naše částečné šablona vytvořena. Potom jsme budete aktualizovat soubor Map.ascx mít následující obsah:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

První &lt;skriptu&gt; referenční body do knihovny Microsoft Virtual Earth 6.2 mapování. Druhý &lt;skriptu&gt; referenční body do map.js souboru, který krátce vytvoříme, který bude zapouzdření běžné logiku mapování Javascript. &lt;Div id = "theMap"&gt; element je kontejner HTML, který aplikace Virtual Earth bude používat k hostování mapy.

Potom máme jako embedded &lt;skriptu&gt; blok, který obsahuje dvě funkce specifické pro toto zobrazení jazyka JavaScript. První funkce jQuery používá k navázání funkci, která provede, když se stránka je připraven ke spuštění skriptu na straně klienta. Zavolá LoadMap() podpůrná funkce, budeme definovat v souboru skriptu naše Map.js načíst mapový ovládací prvek aplikace virtual earth. Funkce second je obslužnou rutinu události zpětného volání, která přidá kód pin pro mapu, která identifikuje umístění.

Všimněte si, jak se používá straně serveru &lt;% = %&gt; bloku v bloku skriptu na straně klienta pro vložení zeměpisnou šířku a délku večeři chceme mapovat do jazyka JavaScript. To je užitečné pro výstup dynamické hodnoty, které můžete použít skript na straně klienta (bez nutnosti samostatné AJAX volání zpět na server načíst hodnoty – což umožňuje rychlejší). &lt;% = %&gt; Bloky se spustí, když je na serveru – vykreslování zobrazení a proto výstupu HTML se právě skončili hodnotami JavaScript embedded (například: var šířky = 47.64312;).

### <a name="creating-a-mapjs-utility-library"></a>Vytvoření knihovny nástroj Map.js

Nyní vytvoříme Map.js soubor, který používáme zapouzdření funkce JavaScript pro naše mapy (a implementovat LoadMap a LoadPin metody výše). Jsme to udělat kliknutím pravým tlačítkem na \Scripts adresáře v rámci naší projekt a potom vyberte "Přidat -&gt;novou položku" příkazu v nabídce, vyberte položku JScript a pojmenujte ji "Map.js".

Níže je kód JavaScript přidáme Map.js soubor, který bude v interakci se aplikace Virtual Earth zobrazíte naše mapy a pro naše večeří do ní přidejte umístění PIN:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a>Integrace mapy s vytvořit a upravit formulářů

Podpora mapy jsme teď budete integrovat s naše vytvořit a upravit existující scénáře. Dobrá zpráva je, že to je velmi snadné úkolů a nevyžaduje nám chcete změnit některé z našich kódu Kontroleru. Protože naše zobrazení vytvořit a upravit sdílejí společné "DinnerForm" částečné zobrazení k implementaci formuláře večeři uživatelského rozhraní, jsme přidejte mapy na jednom místě a mít oba naše vytvořit a upravit scénáře ho použít.

Všechny potřebujeme úkolů je částečné zobrazení \Views\Dinners\DinnerForm.ascx otevřít a aktualizovat tak, aby obsahovala naší nové částečné mapování. Níže je bude vypadat aktualizované DinnerForm po přidání mapy (Poznámka: prvků formuláře HTML, které byly vynechány z fragmentu kódu zobrazeném níže jako stručný výtah):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

Částečné výše DinnerForm vezme objekt typu "DinnerFormViewModel" jako typ modelu, (protože je nutné objekt večeři, jak SelectList k naplnění rozevírací seznam zemí,). Naše mapy částečné právě potřebuje objekt typu "Večeři" jako typ modelu, a proto když jsme vykreslení mapy částečné jsme předali právě večeři dílčí vlastnosti DinnerFormViewModel k němu:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

Funkce JavaScript, která jsme přidali do jQuery částečné používá k připojení "rozostření" události do textového pole "Address" HTML. Pravděpodobně jste slyšeli "fokusu" události, které aktivují, když uživatel klikne nebo karet do textové pole. Naopak je "rozostření" událost, která aktivuje se v případě, že uživatel ukončí textové pole. Výše uvedené obslužné rutiny události vymaže textbox hodnoty zeměpisné šířky a délky, pokud to se stane a potom ukazuje zeměpisný nové umístění adresy na našich mapě. Zpětné volání obslužné rutiny události, které jsme definovali v rámci tohoto souboru map.js aktualizují zeměpisné šířky a délky textová pole ve formuláři pomocí hodnot vrácených na základě adresy, které jsme zadali aplikace virtual earth.

A teď když jsme spuštění aplikace znovu a klikněte na kartu "Hostitele večeři" ukážeme výchozí mapování zobrazí společně s naše standardní elementy form večeři:

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

Když jsme zadejte adresu a potom na kartě rychle, mapy dynamicky aktualizuje pro zobrazení umístění a naše obslužné rutiny události naplní textových polí zeměpisnou šířku a délku hodnotami, umístění:

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

Pokud jsme uložit nové večeři a potom ho znovu otevřete pro úpravy, jsme najdete, že Mapa umístění se zobrazí při načtení stránky:

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

Pokaždé, když se změní pole adresy, bude aktualizovat souřadnice zeměpisnou šířku a délku a mapy.

Teď, když mapa zobrazuje umístění večeři, jsme můžete také změnit pole formuláře zeměpisnou šířku a délku nebudou viditelné textových polí místo jako skrytá elementy (protože mapy je automaticky aktualizuje pokaždé, když se zadá adresu). Úkolů to jsme přepínat pomocí pomocné rutiny Html.TextBox() HTML k použití Html.Hidden() pomocnou metodu:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

A teď naše formuláře jsou o něco více uživatelsky přívětivý a zamezení zobrazení nezpracovaná zeměpisnou šířku a délku (při současném stále je ukládání s každou večeři v databázi):

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a>Integrace s podrobné zobrazení mapy

Teď, když máme mapy integrovat naše scénáře vytvořit a upravit můžeme také integrovat s náš scénář podrobnosti. Všechny potřebujeme úkolů je volání &lt;% Html.RenderPartial("map"); %&gt; v zobrazení podrobností.

Dole je zdrojový kód pro dokončení zobrazení podrobností (s integrace mapy), která bude vypadat takto:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

A teď když uživatel přejde na /Dinners/podrobnosti / [id] adresu URL se zobrazí podrobnosti o večeři, umístění večeři na mapě (dokončení s kódem pin nabízené který po při přechodu myší přes zobrazí název večeře a adresu jeho), a mít propojení AJAX pro zasílání zpráv rysy fo r ho:

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a>Implementace hledání umístění v databázi a úložiště

K dokončení vypnout naše implementace AJAX, přidejte umožňuje mapování na domovskou stránku aplikace, která umožňuje uživatelům graficky vyhledejte večeří téměř je.

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

Začneme budete implementovat podporu v rámci naší vrstvy úložiště databáze a datový k efektivnímu provádění vyhledávání na základě umístění protokolu radius pro večeří. Můžeme použít nové [geoprostorové funkce SQL 2008](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx) implementovat, nebo můžete také můžeme použít přístup funkce SQL, který Gary Dryden popsané v článku zde: [ http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx ](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx) a Rob Conery rozsáhlý blok o pomocí technologie LINQ to SQL tady: [http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)

K implementaci Tato technika, jsme bude otevřete "Průzkumníka serveru" v sadě Visual Studio, vyberte databázi, NerdDinner a klikněte pravým tlačítkem na uzel dílčí "funkce" v něm a můžete vytvořit nový "skalární funkce založené na":

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

Budete jsme potom vložte následující funkce DistanceBetween:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

Poté vytvoříme novou funkci vracející tabulku v systému SQL Server, zavoláme vám "NearestDinners":

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

Tato funkce tabulky "NearestDinners" používá pomocné funkce DistanceBetween vrátit všechny večeří v rámci 100 miles zeměpisnou a zeměpisnou délku, jsme ji zadat:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

Pro volání této funkce, nejprve otevřeme až LINQ to SQL Návrháře poklikáním na soubor NerdDinner.dbml v našem \Models directory:

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

Funkce NearestDinners a DistanceBetween jsme pak budete přetažením na LINQ do návrháře SQL, které způsobí, že je možné přidat jako metody na našem LINQ ke třídě SQL NerdDinnerDataContext:

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

Zveřejňujeme metodu dotazu "FindByLocation" můžete pak na našem DinnerRepository třídu, která používá funkci NearestDinner vrátit nadcházející večeří, které jsou v rámci 100 miles v zadaném umístění:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a>Implementace metody akce vyhledávání na základě JSON AJAX

Nyní jsme budete implementovat metodu akce kontroleru, který využívá nové úložiště metody FindByLocation() vrácení zpět seznamu večeři dat, která slouží k naplnění mapy. Budeme mít této metodě akce vrácení zpět večeři dat ve formátu JSON (JavaScript Object Notation) tak, aby ji snadno práce s použitím jazyka JavaScript na straně klienta.

Chcete-li tuto funkci implementovat, vytvoříme novou třídu "SearchController" pravým tlačítkem myši na adresář \Controllers a zvolením Add -&gt;příkazu nabídky řadiče. Potom jsme budete implementovat metodu akce "SearchByLocation" v rámci nové třídy SearchController jako níže:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

Metody akce SearchController SearchByLocation interně volá metodu FindByLocation na DinnerRespository získat seznam blízkým večeří. Místo načíst objekty večeři přímo do klienta, ale místo toho vrátí JsonDinner objekty. Třída JsonDinner zpřístupňuje podmnožinu večeři vlastnosti (například: bezpečnostních důvodů se nepodporuje zveřejnit názvy uživatelů, kteří mají na které odpověděl večeře). Zahrnuje také o RSVPCount vlastnost, která neexistuje v večeři – a který je počítáno dynamicky určovat počet zasílání zpráv rysy objekty přidružené k určité večeři.

Potom používáme pomocnou metodu Json() na základní třídy Kontroleru vrátit pořadí večeří formátu JSON na základě přenosu. JSON je standardního textového formátu sloužící k zastoupení jednoduché datové struktury. Dole je příklad jak formátu JSON seznam dva objekty JsonDinner vypadá, když vrácená z našich metodu akce:

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a>Volání metody na základě JSON AJAX pomocí jQuery

Nyní jsme připraveni na aktualizaci domovské stránce aplikace NerdDinner lze pomocí metody akce SearchController SearchByLocation. Úkolů toho jsme budete otevřete šablonu zobrazení /Views/Home/Index.aspx a aktualizovat ji tak, aby měl textové pole, tlačítko vyhledat, naše mapy a &lt;div&gt; element s názvem dinnerList:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

Dvě funkce JavaScript jsme poté můžete přidat na stránku:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

První funkce JavaScript, která načte mapy, při prvním načtení stránky. Druhý vodičům funkce JavaScript nahoru JavaScript klikněte na obslužnou rutinu události na tlačítko Hledat. Při stisknutí tlačítka volá funkci FindDinnersGivenLocation() JavaScript, která přidáme do naše Map.js souborů:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

Tato funkce FindDinnersGivenLocation() volá mapy. Find() na Virtual Earth ovládacího prvku na střed na zadané umístění. Pokud službu aplikace virtual earth mapy vrátí, mapy. Find() metoda volá metodu zpětného volání callbackUpdateMapDinners, jsme předán jako konečný argument.

Metoda callbackUpdateMapDinners() je, kde se provádí skutečná práce. Na jQuery $.post() Pomocná metoda používá k provedení volání AJAX na metodu akce SearchByLocation() naše SearchController – předání zeměpisnou šířku a délku nově zarovnaný mapy. Definuje, vložené funkce, která bude volána po dokončení pomocnou metodu $.post() a vráceny výsledky formátu JSON večeři z SearchByLocation() metoda akce bude předán pomocí proměnné s názvem "večeří". Potom zajišťuje foreach přes každý vrácený večeři a přidat nový kód pin na mapě používá večeři zeměpisnou šířku a zeměpisnou délku a další vlastnosti. Položka večeři také přidá do seznamu HTML večeří napravo od mapy. Ji pak vodičům up hover událostí pushpins a seznamu HTML, aby se zobrazí podrobnosti o večeře při nastavení ukazatele myši je:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

A teď když jsme aplikaci spustit a přejděte na domovskou stránku, kterou jsme vám bude nabídnuta mapu. Když jsme zadejte název města mapy nadcházející večeří téměř se zobrazí:

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

Ukazatele myši večeři zobrazíte její podrobnosti.

Kliknutím na nadpis večeři do bubliny nebo na pravé straně v seznamu HTML pro přechod nám večeři – které jsme můžete poté volitelně RSVP pro:

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a>Další krok

Implementovali jsme nyní všechny funkce aplikace naše NerdDinner aplikace. Pojďme nyní pohled na tom, jak můžeme provést aktivaci automatizované jednotky testování ho.

> [!div class="step-by-step"]
> [Předchozí](use-ajax-to-deliver-dynamic-updates.md)
> [další](enable-automated-unit-testing.md)
