---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: Použití jazyka AJAX k implementaci scénářů mapování | Dokumentace Microsoftu
author: microsoft
description: Krok 11 ukazuje, jak integrovat podporu mapování AJAX do naší aplikace NerdDinner povolení uživatelé, kteří jsou vytvoření, úpravách nebo prohlížení večeří zobrazíte l...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: f7de23ca46e6dc00fe8075e28068a8b3f95d02cd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753043"
---
<a name="use-ajax-to-implement-mapping-scenarios"></a>Použití jazyka AJAX k implementaci scénářů mapování
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Toto je krok 11 bezplatného [kurz vývoje aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , který procházení procházení po tom, jak sestavit malý, ale bylo možné provést, webové aplikace pomocí ASP.NET MVC 1.
> 
> Krok 11 ukazuje, jak integrovat podporu mapování AJAX do naší aplikace NerdDinner povolení uživatelé, kteří jsou vytvoření, úpravách nebo prohlížení večeří zobrazíte umístění společnosti dinner graficky.
> 
> Pokud používáte ASP.NET MVC 3, doporučujeme je provést [získávání začít s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.


## <a name="nerddinner-step-11-integrating-an-ajax-map"></a>NerdDinner krok 11: Integrace mapu AJAX

Teď uděláme naši aplikaci trochu vizuálně vzrušující integrací podporu mapování AJAX. Tato možnost umožní uživatelům, kteří jsou vytvoření, úpravách nebo prohlížení večeří zobrazíte umístění společnosti dinner graficky.

### <a name="creating-a-map-partial-view"></a>Vytváření částečné zobrazení mapy

Budeme používat funkce mapování na několika místech v rámci naší aplikace. Náš kód suchého zapouzdříme běžné funkce mapy v rámci jednoho, který můžeme znovu použít pro různé akce kontroleru a zobrazení částečné šablony. Vytvoříme pojmenujte toto částečné zobrazení "map.ascx" a vytvořit v rámci \Views\Dinners adresáře.

Můžeme vytvořit map.ascx částečné pravým tlačítkem myši na adresáři \Views\Dinners a zvolením přidat -&gt;zobrazit příkaz nabídky. Použijeme název zobrazení "Map.ascx", zkontrolujte jako částečné zobrazení a označuje, že budeme předáváme třídy silný "Dinner" modelu:

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

Když kliknete na tlačítko "Přidat" náš částečné šablony vytvoří. Potom aktualizujeme Map.ascx soubor s následujícím obsahem:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

První &lt;skript&gt; referenční body ke knihovně Microsoft Virtual Earth 6.2 mapování. Druhá &lt;skript&gt; referenční body do map.js souboru, který bude brzy vytvoříme, který bude zapouzdření náš společný mapování logiky Javascript. &lt;Div id = "theMap"&gt; element je kontejner ve formátu HTML, která k hostování na mapě používá aplikace Virtual Earth.

Potom máme jako vložený &lt;skript&gt; blok, který obsahuje dvě funkce jazyka JavaScript specifické pro toto zobrazení. První funkce jQuery používá k navázání funkci, která se provede, když je na stránce Připraveno ke spuštění skriptu na straně klienta. Volá funkci LoadMap() pomocné rutiny, které budeme definovat v rámci naší Map.js soubor skriptu načíst mapový ovládací prvek aplikace virtual earth. Druhá funkce je obslužnou rutinu události zpětného volání, která přidá PIN kódu do mapy, která identifikuje umístění.

Všimněte si, jak se používá na straně serveru &lt;% = %&gt; blok v rámci bloku skriptu na straně klienta pro zeměpisnou šířku a délku Dinner chceme mapování do jazyka JavaScript pro vložení. Toto je užitečný způsob, jak výstup dynamických hodnot, které se dají ve skriptu na straně klienta (bez nutnosti samostatné AJAX zpětné volání na serveru k načtení hodnoty – díky tomu je rychlejší). &lt;% = %&gt; Bloky se spustí, když je zobrazení vykreslování na serveru – a tedy výstupu HTML se právě skončit s vložený JavaScript hodnoty (například: zeměpisná šířka var = 47.64312;).

### <a name="creating-a-mapjs-utility-library"></a>Vytvoření knihovny Map.js nástroje

Pojďme teď vytvořit Map.js soubor, který můžeme použít k zapouzdření funkce jazyka JavaScript pro mapa (a implementovat LoadMap a LoadPin metod uvedených výše). Můžeme to udělat kliknutím pravým tlačítkem na adresář \Scripts v našem projektu a klikněte na tlačítko "Add -&gt;novou položku" příkazu nabídky vyberte položku JScript a pojmenujte ho "Map.js".

Níže je kód jazyka JavaScript, přidáme Map.js soubor, který bude komunikovat s Virtual Earth a zobrazte Mapa k němu přidat umístění špendlíky pro naše večeří:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a>Integrace mapy pomocí vytváření a úpravy formulářů

Podpora mapování jsme teď bude integrovat s naší stávající scénáře vytvořit a upravit. Dobrou zprávou je, že to je poměrně snadné úkolů a nevyžaduje, aby nám chcete-li změnit některý z našich kódu Kontroleru. Naše vytvořit a upravit zobrazení sdílejí společné "DinnerForm" částečné zobrazení k implementaci dinner formulář, uživatelského rozhraní, jsme přidat mapování na jednom místě a mít oba vytvořit a upravit scénáři použití.

Všechny potřebujeme úkolů je otevřete \Views\Dinners\DinnerForm.ascx částečné zobrazení a aktualizujte tak, aby obsahovala naše nové částečné mapování. Níže je aktualizovaný DinnerForm bude vypadat po přidání mapy (Poznámka: jsou vynechány prvků formuláře HTML z fragmentu kódu níže pro zkrácení):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

Částečné nad DinnerForm přijímá objekt typu "DinnerFormViewModel" jako typ modelu (protože je nutné, jak objekt Dinner, tak i SelectList k naplnění dropdownlist země). Mapa částečné právě musí objekt typu "Dinner" jako typ modelu a tak když jsme vykreslovat na mapě částečné jsme prochází pouze Dinner dílčí vlastnost DinnerFormViewModel do ní:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

Funkce JavaScript, která jsme přidali do částečné použití jQuery připojit "rozostření" události do textového pole "Address" HTML. Pravděpodobně jste slyšeli událostí "fokusu", které se aktivují, když uživatel klikne nebo karty do textové pole. Naopak je "rozostření" událost, která je vyvoláno, když uživatel ukončí textové pole. Výše uvedené obslužná rutina události vymaže textového pole hodnoty zeměpisné šířky a délky při to se stane a pak zobrazí na nové místo adres na mapa. Aktualizujte obslužnou rutinu události zpětného volání, které jsme definovali v souboru map.js budou textová pole zeměpisné šířky a délky na našem formuláři pomocí hodnot vrácených na základě adresy, které jsme zadali aplikace virtual earth.

A teď když jsme naši aplikaci znovu spustit a klikněte na kartu "Hostitele Dinner" uvidíme výchozí mapování, zobrazí spolu s naší standardní prvky formuláře Dinner:

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

Když jsme zadejte adresu a pak na kartě okamžitě, mapa způsobí dynamickou aktualizaci pro zobrazení umístění a naše obslužná rutina události vyplní zeměpisné šířky a délky textová pole s hodnotami umístění:

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

Pokud se nám uložit nový web dinner a potom ho znovu otevřete pro úpravy, můžeme najdete, když se stránka načte, zobrazí se Mapa umístění:

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

Pokaždé, když se do pole adresy změnilo, aktualizujeme, mapy a souřadnice zeměpisné šířky a délky.

Teď, když mapa zobrazuje umístění Dinner, jsme také změnit pole formuláře zeměpisné šířky a délky tomu nebudou viditelné textových polí místo toho bude skrytý elementy (protože na mapě se automaticky aktualizuje pokaždé, když se zadá adresu). Úkol to nám budete přepnou od používání pomocné rutiny Html.TextBox() HTML pomocí Html.Hidden() pomocnou metodu:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

A teď naše formuláře jsou mírně přívětivější a zabránit zobrazování nezpracovaná zeměpisnou šířkou/délkou (při stále ukládání s každou večeře v databázi):

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a>Integrace mapy s zobrazení podrobností

Když teď máme na mapě integrovat s našimi scénáři vytvořit a upravit můžeme také provést integraci s scénáři podrobnosti. Všechny potřebujeme úkolů je volání &lt;% Html.RenderPartial("map"); %&gt; v zobrazení Details.

Níže je, jak vypadá byl zdrojový kód úplné podrobnosti o zobrazení (díky integraci s mapou):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

A teď když uživatel přejde na /Dinners/podrobnosti / [id] adresa URL zobrazí podrobnosti o dinner umístění dinner na mapě (s nabízenou – kód pin, že myš zobrazí název společnosti dinner a adresu ji), a mají propojení jazyka AJAX k RSVP fo r je:

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a>Implementace hledání umístění v databázi a úložiště

Na dokončení vypnutí naše implementace jazyka AJAX, přidáme na domovskou stránku aplikace, která umožňuje uživatelům graficky vyhledejte večeří téměř je mapy.

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

Začneme budete implementací podpory v rámci naší vrstvy úložiště databáze a dat můžete efektivně vyhledávat založená na poloze pomocí protokolu radius pro večeří. Mohli bychom použít nové [geoprostorové funkce SQL 2008](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx) k implementaci, nebo také můžeme použít přístup funkce SQL, který Gary Dryden popsané v článku: [ http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx ](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx) a Rob Conery blozích máte o tady pomocí LINQ to SQL: [http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)

K implementaci tuto techniku, jsme se otevřít "Průzkumníka serveru" v sadě Visual Studio, vyberte databázi NerdDinner a klikněte pravým tlačítkem na uzel dílčí "funkce" je pod ním a můžete vytvořit novou "vracející skalární funkci":

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

Vložte jsme budete ve funkci DistanceBetween následující:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

Poté vytvoříme novou funkci vracející tabulku v systému SQL Server, zavoláme vám "NearestDinners":

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

Tuto funkci tabulky "NearestDinners" používá funkci pomocné rutiny DistanceBetween vrátit všechny večeří v rámci 100 mil zeměpisná šířka a zeměpisná délka, můžeme ji zadat:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

Volání této funkce, nejprve otevřeme nahoru LINQ to SQL Návrhář poklikáním na soubor NerdDinner.dbml v rámci naší \Models adresáře:

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

Potom přetáhneme funkce NearestDinners a DistanceBetween na LINQ do SQL návrháře, což způsobí přidají jako metody na naše LINQ na třídy SQL NerdDinnerDataContext:

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

Metody dotazu "FindByLocation" jsme pak můžete zveřejnit na naše DinnerRepository třídu, která používá funkci NearestDinner vrátit nadcházející večeří, které jsou v rámci 100 mil zadané umístění:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a>Implementace metody akce hledání na základě JSON AJAX

Nyní jsme budete implementovat metodu akce kontroleru, který využívá novou metodu úložiště FindByLocation() vrátit zpět seznam Dinner data, která slouží k naplnění mapy. Budeme mít této metodě akce vrátit zpět web Dinner data ve formátu JSON (JavaScript Object Notation) tak, aby jej lze snadno ovládat pomocí jazyka JavaScript na straně klienta.

K provedení vytvoříme novou třídu "SearchController" pravým tlačítkem myši na adresáři \Controllers a zvolením přidat -&gt;příkazu nabídky Kontroleru. Potom jsme budete implementovat metodu akce "SearchByLocation" v rámci nové třídy SearchController jako níže:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

Metody akce SearchController SearchByLocation interně volá metodu FindByLocation na DinnerRespository zobrazíte seznam blízké večeří. Místo vrátí objekty Dinner přímo do klienta, ale místo toho vrátí JsonDinner objekty. Třída JsonDinner zveřejňuje podmnožinu vlastností Dinner (například: z bezpečnostních důvodů ho nebude tyto názvy uživatelů, kteří mají RSVP'd večeře). Zahrnuje také vlastnost RSVPCount, který neexistuje v Dinner – a které je počítáno dynamicky určovat počet RSVP objekty přidružené k určité dinner.

Potom používáme Json() pomocnou metodu v základní třídě Controller k vrácení sekvence večeří pomocí založenými na JSON přenosový formát. JSON je standardní textový formát pro uvádění jednoduché datové struktury. Níže je příklad vypadá seznam dvou JsonDinner objektů ve formátu JSON, pokud vrácená metodě akce:

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a>Volání metody založené na JSON AJAX pomocí jQuery

Nyní jsme připraveni aktualizujte domovskou stránku aplikace NerdDinner SearchController SearchByLocation akce metody. Úkol, vytvoříme otevřete /Views/Home/Index.aspx zobrazit šablonu a aktualizujte ji, aby mají textové pole, tlačítko hledání, mapa a &lt;div&gt; element s názvem dinnerList:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

Pak můžeme přidat dvě funkce jazyka JavaScript na stránku:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

První funkce JavaScript, která načte mapy, při prvním načtení stránky. Druhý kabely funkce JavaScript nahoru v JavaScriptu klikněte na obslužnou rutinu události na tlačítko Hledat. Při stisknutí tlačítka volá funkci FindDinnersGivenLocation() JavaScript, které přidáme k naší Map.js souboru:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

Tato funkce FindDinnersGivenLocation() volá mapy. Find() na ovládacím prvku Virtual Earth na střed na zadané umístění. Když service pro mapy aplikace virtual earth návratu na mapě. Find() metoda volá metodu zpětného volání callbackUpdateMapDinners, kterou jsme předán jako poslední argument.

Metoda callbackUpdateMapDinners() je, kde se provádí skutečnou práci. Používá pro jQuery $.post() Pomocná metoda provádět volání AJAX na metodu akce SearchByLocation() naše SearchController – předáním zeměpisnou šířku a délku nově zaměřena na mapě. Definuje vložené funkce, která bude volána po dokončení $.post() Pomocná metoda, a vrátí výsledky ve formátu JSON dinner z SearchByLocation() metody akce bude předán pomocí proměnnou s názvem "večeří". Potom zajišťuje přes každý vrácený dinner foreach a používá večeři šířky a délky a dalších vlastností, chcete-li přidat nový PIN kód na mapě. Také přidá dinner položku do seznamu HTML večeří napravo od mapy. To pak vodičům stanice up události při najetí myší připínáčky a seznamu HTML tak, aby se zobrazí podrobnosti o společnosti dinner, když uživatel najede myší je:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

A teď když jsme spusťte aplikaci a na domovské stránce –, který jsme zobrazí se vám s mapou. Když jsme zadejte název města se zobrazí na mapě nadcházející večeří máte telefon po ruce:

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

Najede myší večeři zobrazí podrobnosti o něm.

Kliknutím na název společnosti Dinner v bublinovém nebo na pravé straně v seznamu HTML bude Navigovat nám na večeři – které můžeme můžete poté volitelně potvrďte svou účast na:

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a>Dalším krokem

Implementovali jsme teď všechny funkce aplikace naší aplikace NerdDinner. Pojďme teď podívat, jak jsme povolili automatizované jednotky testování ji.

> [!div class="step-by-step"]
> [Předchozí](use-ajax-to-deliver-dynamic-updates.md)
> [další](enable-automated-unit-testing.md)
