---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
title: "Představení technologie ASP.NET Web Pages – aktualizace dat databáze | Microsoft Docs"
author: tfitzmac
description: "V tomto kurzu se dozvíte, jak aktualizovat položku (změnit) existující databáze při použití technologie ASP.NET Web Pages (Razor). Předpokládá, že jste dokončili řady tý..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: ac86ec9c-6b69-485b-b9e0-8b9127b13e6b
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
msc.type: authoredcontent
ms.openlocfilehash: 6fc8631c323dbb82f4f33b78128ba8de8bc9bfdd
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/09/2018
---
<a name="introducing-aspnet-web-pages---updating-database-data"></a>Představení technologie ASP.NET Web Pages – aktualizace dat databáze
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> V tomto kurzu se dozvíte, jak aktualizovat položku (změnit) existující databáze při použití technologie ASP.NET Web Pages (Razor). Předpokládá, že jste dokončili řady prostřednictvím [zadávání dat pomocí pomocí formulářů pomocí rozhraní ASP.NET Web Pages](entering-data.md).
> 
> Získáte informace:
> 
> - Jak vybrat jednotlivé záznamy v `WebGrid` pomocné rutiny.
> - Jak si jeden záznam z databáze.
> - Postup předběžné načtení formuláře s hodnotami ze záznamů databáze.
> - Postup aktualizace stávajícího záznamu v databázi.
> - Tom, jak ukládat informace na stránce bez jeho zobrazení.
> - Jak používat k ukládání informací skryté pole.
>   
> 
> Funkce nebo technologie popsané:
> 
> - `WebGrid` Pomocné rutiny.
> - SQL `Update` příkaz.
> - `Database.Execute` Metoda.
> - Skryté pole (`<input type="hidden">`).


## <a name="what-youll-build"></a>Co budete sestavení

V tomto kurzu předchozí jste zjistili, jak přidat záznam do databáze. Zde dozvíte jak zobrazit záznam pro úpravy. V *filmy* stránky, budete aktualizovat `WebGrid` pomocné rutiny, které se zobrazí **upravit** odkaz vedle jednotlivých film:

![WebGrid zobrazení, včetně odkazu na "Upravit" pro každý film](updating-data/_static/image1.png)

Když kliknete **upravit** odkaz, tím přejdete na jinou stránku, kde informace film je již ve formuláři:

![Upravit film stránky zobrazující film bude upravena](updating-data/_static/image2.png)

Můžete změnit žádnou hodnotu. Při odesílání změn kódu na stránce aktualizuje databázi a přejde zpět na film výpis.

Tato část procesu funguje skoro stejně jako *AddMovie.cshtml* stránky, které jste vytvořili v předchozí kurz, takže většinu tohoto kurzu bude známé.

Způsob, jak upravit jednotlivé film může implementovat několika způsoby. Přístup zobrazí jste vybrali, protože je snadno implementovat a pochopitelnější.

## <a name="adding-an-edit-link-to-the-movie-listing"></a>Přidávání do seznamu film odkaz pro úpravy

Pokud chcete začít, budete aktualizovat *filmy* stránky tak, aby každý film výpis také obsahuje **upravit** odkaz.

Otevřete *Movies.cshtml* souboru.

V těle stránky změnit `WebGrid` značek přidáním sloupce. Tady je upravených značek:

[!code-html[Main](updating-data/samples/sample1.html?highlight=6)]

Nový sloupec, je tato:

[!code-html[Main](updating-data/samples/sample2.html)]

Bod v tomto sloupci se zobrazí odkaz (`<a>` element) jejíž text říká "Upravit". Máme po je vytvoření odkaz, který vypadá takto při spuštění stránky s `id` hodnotu pro každý film jiné:

[!code-css[Main](updating-data/samples/sample3.css)]

Tento odkaz bude vyvolat stránku s názvem *EditMovie*, a předá řetězec dotazu `?id=7` na této stránce.

Syntaxe pro nový sloupec, může vypadat poněkud složité, je to ale jenom protože umístí společně několik elementy. Každý jednotlivý prvek je jednoduchá. Pokud vám soustředit se jenom na právě na `<a>` elementu, zobrazí tento kód:

[!code-html[Main](updating-data/samples/sample4.html)]

Některé informace o tom, jak funguje mřížky: v mřížce zobrazené řádky, jeden pro každý záznam v databázi, a zobrazuje sloupce pro každé pole v záznamu v databázi. Když je vytvářen každý řádek mřížky, `item` objekt obsahuje záznam databáze (položek) pro tento řádek. Toto uspořádání poskytuje způsob, jak můžete získat na data pro tento řádek v kódu. To je, co vidíte zde: výraz `item.ID` dochází hodnota ID položky pro aktuální databázi. Můžete získat žádné z hodnot v databázi (název, genre nebo rok) stejným způsobem jako pomocí `item.Title`, `item.Genre`, nebo `item.Year`.

Výraz `"~/EditMovie?id=@item.ID` kombinuje součástí pevně cílová adresa URL (`~/EditMovie?id=`) s číslem ID této dynamicky odvozené. (Jste viděli `~` operátor v předchozí kurzu; je operátor ASP.NET, který představuje aktuální kořenový adresář webu.)

Výsledkem je, že tato součást kód ve sloupci jednoduše vytvoří něco jako následující kód v době běhu:

[!code-xml[Main](updating-data/samples/sample5.xml)]

Samozřejmě se skutečnou hodnotou `id` se bude lišit pro každý řádek.

## <a name="creating-a-custom-display-for-a-grid-column"></a>Vytváření vlastních zobrazení sloupce mřížky

Nyní zpět na sloupci mřížky. Tři sloupce původně bylo v mřížce zobrazí pouze data hodnoty (název, genre a roku). Toto zobrazení určeného předávání název sloupce databáze &mdash; například `grid.Column("Title")`.

Tento nový **upravit** odkaz sloupce se liší. Místo zadání název sloupce, které předávání `format` parametr. Tento parametr umožňuje definovat značek, `WebGrid` pomocná učiní spolu s `item` hodnotu pro zobrazení data ve sloupci jako tučné a zelenou nebo ve formátu, které chcete. Například pokud byste chtěli nadpis zobrazuje tučným písmem, můžete vytvořit sloupec následujícím způsobem:

[!code-html[Main](updating-data/samples/sample6.html)]

(Různé `@` znaky, které se zobrazí v `format` vlastnost označit přechod mezi značek a kódu hodnotu.)

Jakmile víte o `format` vlastnost, je jednodušší zjistit, jak nové **upravit** připravili odkaz sloupce:

[!code-html[Main](updating-data/samples/sample7.html)]

Sloupec obsahuje *pouze* z kódu, který vykreslí odkaz, plus některé informace (ID), je extrahována ze záznamů databáze pro řádek.

> [!TIP] 
> 
> **Poziční parametry pro metodu a pojmenované parametry**
> 
> Tolikrát, kolikrát jste volal metodu a do ní předán parametry, jednoduše uvedené v seznamu hodnot parametrů oddělených čárkami. Tady je několik příkladů:
> 
> `db.Execute(insertCommand, title, genre, year)`
> 
> `Validation.RequireField("title", "You must enter a title")`
> 
> Jsme nepomohly problém zmínili, když znáte tento kód, ale v každém případě se předávání do metod v určitém pořadí parametrů &mdash; konkrétně, pořadí, ve kterém jsou definovány parametry v dané metody. Pro `db.Execute` a `Validation.RequireFields`, pokud se promíchají pořadí hodnot předáte, by získat chybová zpráva při spuštění stránky nebo alespoň některé neobvyklé výsledky. Je zřejmé budete muset vědět předat parametry v pořadí. (Ve službě WebMatrix, IntelliSense vám může pomoci další obrázek na název, typ a pořadí parametrů.)
> 
> Jako alternativu k předávání hodnot v pořadí, můžete použít *pojmenované parametry*. (Předávání parametrů v pořadí se označuje jako pomocí *poziční parametry*.) Pojmenované parametry je explicitně zahrnout název parametru při předávání jeho hodnotu. Jste použili pojmenované parametry již několikrát v těchto kurzech. Příklad:
> 
> [!code-csharp[Main](updating-data/samples/sample8.cs)]
> 
> and
> 
> [!code-css[Main](updating-data/samples/sample9.css)]
> 
> Pojmenované parametry jsou užitečné několik situací, zejména v případě, že metoda přebírá mnoho parametrů. Jedna je, když chcete předat pouze jeden nebo dva parametry, ale hodnoty, které chcete předat nejsou mezi první pozice v seznamu parametrů. Jiné situaci je, pokud chcete zvýšit vašeho kódu předáním parametry v pořadí, ve kterém je pro vás nejvhodnější.
> 
> Samozřejmě používat pojmenované parametry, budete muset znát názvy parametrů. Služba WebMatrix IntelliSense můžete *zobrazit* názvy, ale nemůžete vyplnit aktuálně je pro vás.


## <a name="creating-the-edit-page"></a>Vytvoření stránky úpravy

Nyní můžete vytvořit *EditMovie* stránky. Když uživatelé kliknou na **upravit** odkazu, který se nakonec se na této stránce.

Vytvoření stránky s názvem *EditMovie.cshtml* a nahraďte, co je v souboru s následující kód:

[!code-cshtml[Main](updating-data/samples/sample10.cshtml)]

Tento kód a kód je podobná máte ve *AddMovie* stránky. Je malý rozdíl v text pro tlačítko pro odeslání. Stejně jako u *AddMovie* stránky, dojde `Html.ValidationSummary` volání, které se zobrazí chyby ověření, pokud jsou k dispozici. Tentokrát jsme se vynechala volání `Validation.Message`, protože se zobrazí chyby v souhrnu ověření. Jak jsme uvedli v předchozím kurzu, můžete použít v různých kombinacích souhrnu ověření a jednotlivé chybové zprávy.

Všimněte si znovu, že `method` atribut `<form>` element je nastaven na hodnotu `post`. Stejně jako u *AddMovie.cshtml* stránky, tato stránka umožňuje změny do databáze. Proto by měla provést tento formulář `POST` operaci. (Další informace o rozdílu mezi `GET` a `POST` operací, najdete v článku [GET, POST a zabezpečení protokolu HTTP příkaz](form-basics.md#GET,_POST,_and_HTTP_Verb_Safety) bočním panelu v tomto kurzu ve formulářích HTML.)

Jak už jste viděli v dřívější kurzu, `value` atributy do textových polí se se nastavují s kódu Razor, aby bylo možné přednačtení je. Tentokrát, když používáte proměnné jako `title` a `genre` pro tuto úlohu místo `Request.Form["title"]`:

`<input type="text" name="title" value="@title" />`

Jako dříve, tento kód bude přednačtení textové hodnoty pole s hodnotami film. Se zobrazí ve chvíli, proč je užitečné používat proměnné tentokrát místo použití `Request` objektu.

K dispozici je také `<input type="hidden">` element na této stránce. Tento element ukládá ID film bez viditelnosti na stránce. ID je původně předána na stránku pomocí pomocí hodnoty řetězce dotazu (`?id=7` nebo podobné v adrese URL). Hodnota ID vložíte do skryté pole, můžete zkontrolovat, zda je k dispozici při odeslání formuláře, i když už máte přístup k původní adresu URL, který stránka byla vyvolána s.

Na rozdíl od *AddMovie* stránky kód pro *EditMovie* stránky má dvě odlišné funkce. První funkce je, že pokud stránka se zobrazí při prvním (a *pouze* pak), kód získá ID film z řetězce dotazu. Kód pak používá ID, které ke čtení odpovídající film z databáze nástroje a zobrazení (přednačtení) ji do textových polí.

Funkce second je, že když uživatel klikne **odeslání změn** tlačítko kód má číst hodnoty do textových polí a jejich ověření. Kód má také k aktualizaci položky databáze s novými hodnotami. Tento postup je podobný jako při přidávání záznam, jako jste viděli v *AddMovie*.

## <a name="adding-code-to-read-a-single-movie"></a>Přidání kódu ke čtení jeden film

K provedení první funkci, přidejte do horní části stránky tento kód:

[!code-cshtml[Main](updating-data/samples/sample11.cshtml)]

Většina tohoto kódu je uvnitř bloku, která se spouští `if(!IsPost)`. `!` Operátor znamená "Ne", takže znamená výraz *Pokud tento požadavek není post odeslání*, což je nepřímým způsobem o tom, že *Pokud tento požadavek je při prvním spuštění této stránce*. Jak již bylo uvedeno dříve, tento kód by měl spustit *pouze* při prvním spuštění stránky. Pokud nebylo uzavřete kód v `if(!IsPost)`, by spustit při každém stránka je vyvolána, jestli poprvé nebo v reakci na tlačítko klikněte na tlačítko.

Všimněte si, že tento kód obsahuje `else` blokovat této doby. Jak jsme uvedli, když zavedli jsme `if` bloky, někdy chcete spustit alternativní kódu, pokud není podmínka testujete hodnotu true. Je to tento případ sem. Pokud podmínka předá (to znamená, pokud je třeba ID předána na stránku ok), přečíst řádek z databáze. Ale pokud podmínka neprojde, `else` bloku běží a kód nastaví chybovou zprávu.

## <a name="validating-a-value-passed-to-the-page"></a>Ověřování hodnotu předána na stránku

Kód používá `Request.QueryString["id"]` získat ID, která je předána na stránku. Kód zajišťuje, že hodnota byla zadána ve skutečnosti ID. Pokud byla předána žádná hodnota, nastaví kód chyby ověřování.

Tento kód ukazuje jiný způsob, jak ověřit informace. V tomto kurzu předchozí pracoval s `Validation` pomocné rutiny. Registrovaných pole k ověření a automaticky se ověřování ASP.NET a zobrazí chyby pomocí `Html.ValidationMessage` a `Html.ValidationSummary`. V takovém případě však můžete jste ověřování není skutečně uživatelského vstupu. Místo toho že ověřování hodnotu, která byla předána na stránku z jinde. `Validation` Pomocné rutiny, která není to pro vás.

Proto byste zkontrolovat hodnotu sami, vyzkoušejte s `if(!Request.QueryString["ID"].IsEmpty()`). Pokud dojde k potížím, můžete zobrazit chyba pomocí `Html.ValidationSummary`, stejně jako `Validation` pomocné rutiny. Chcete-li to udělat, zavolejte `Validation.AddFormError` a předejte ji zprávu zobrazíte. `Validation.AddFormError`je integrované metoda, která umožňuje definovat vlastní zprávy, které se pomocí ověřování systému, které jste již obeznámeni s svázání. (Později v tomto kurzu budeme mluvit o tom, aby tento proces ověření trochu robustnější.)

Po provedení se, že je ID videa, kód čte databázi, hledá pouze položku jedné databáze. (Pravděpodobně jste si všimli obecné vzor pro databázové operace: Otevřete databázi, definovat příkazu jazyka SQL a spusťte příkaz.) Tentokrát SQL `Select` příkaz obsahuje `WHERE ID = @0`. Protože ID je jedinečný, se může vracet jenom jeden záznam.

Dotaz se provádí pomocí `db.QuerySingle` (není `db.Query`, jako jste použili movie seznam), a vloží kód výsledku do `row` proměnné. Název `row` libovolný; zadáte název proměnné jakkoli chcete. Proměnné inicializovat v horní části pak hodnotu podrobnosti o filmu tak, aby tyto hodnoty lze zobrazit v textových polích.

## <a name="testing-the-edit-page-so-far"></a>Testování stránce pro úpravy (dosavadní)

Pokud chcete otestovat stránku, spusťte *filmy* nyní a klikněte na tlačítko **upravit** odkaz vedle žádné film. Zobrazí se *EditMovie* stránky s podrobnostmi předvyplněné film jste vybrali:

![Upravit film stránky zobrazující film bude upravena](updating-data/_static/image3.png)

Všimněte si, že adresa URL stránky obsahuje něco podobného jako `?id=10` (nebo jiné telefonní číslo). Dosavadní který otestujete **upravit** odkazů v *film* stránky práce, stránku je přečtení ID z řetězce dotazu, a že databázi dotazy s cílem získat jeden film záznam pracuje.

Můžete změnit informace film, ale nic se stane, když kliknete na tlačítko **odeslání změn**.

## <a name="adding-code-to-update-the-movie-with-the-users-changes"></a>Přidání kódu k aktualizaci film uživatele změn

V *EditMovie.cshtml* souboru, pokud chcete implementovat funkce second (ukládají se změny), přidejte následující kód pouze uvnitř složená závorka `@` bloku. (Pokud si nejste jistí, přesně umístění kód, můžete se podívat na [úplný výpis pro stránku upravit film kódu](#Complete_Page_Listing_for_EditMovie) , zobrazí se na konci tohoto kurzu.)

[!code-csharp[Main](updating-data/samples/sample12.cs)]

Znovu, tento kód a kód je podobný kódu v *AddMovie*. Kód je v `if(IsPost)` blokovat, protože tento kód spustí pouze v případě, že uživatel klikne **odeslání změn** tlačítko &mdash; , kdy (a pouze tehdy, když) formulář odeslal. V takovém případě nepoužíváte testu jako `if(IsPost && Validation.IsValid())`– to znamená, nejsou kombinace obou testů pomocí AND. Na této stránce můžete nejprve určit, jestli je odeslání formuláře (`if(IsPost)`) a pak zaregistrujte pole pro ověření. Potom můžete testovat výsledky ověření (`if(Validation.IsValid()`). Tok se poněkud liší od v *AddMovie.cshtml* stránky, ale účinek je stejný.

Získat hodnoty do textových polí pomocí `Request.Form["title"]` a podobný kód pro druhý `<input>` elementy. Všimněte si, že tuto chvíli kód získá film ID mimo skryté pole (`<input type="hidden">`). Pokud stránky spustili poprvé, kód tu ID z řetězce dotazu. Získat hodnotu z pole skrytá a ujistěte se, že vám ID videa, která byla původně zobrazena, v případě, že od té doby se nějakým způsobem změnit řetězec dotazu.

Opravdu důležité rozdíl mezi *AddMovie* kód a tento kód je, že v tomto kódu použít SQL `Update` příkaz místo `Insert Into` příkaz. Následující příklad ukazuje syntaxi SQL `Update` příkaz:

`UPDATE table SET col1="value", col2="value", col3="value" ... WHERE ID = value`

Můžete zadat všechny sloupce v libovolném pořadí a nemáte nutně aktualizovat každý sloupec během `Update` operaci. (ID samostatně, nelze aktualizovat, protože záznam, výsledkem by uložit jako nový záznam a který není povolen pro `Update` operace.)

> [!NOTE] 
> 
> **Důležité** `Where` klauzule s ID je velmi důležité, protože se jedná o tom, jak databázi zná databázi, kterou záznamů, které chcete aktualizovat. Pokud jste ji přerušili `Where` klauzuli by aktualizoval databázi *každých* záznamů v databázi. Ve většině případů, které by se havárie.


V kódu jsou hodnoty tak, aby aktualizace předán příkaz jazyka SQL s použitím zástupné symboly. Opakování, co jsme uvedli jste před: z bezpečnostních důvodů *pouze* použít zástupné symboly a předat hodnoty do příkazu SQL.

Po kód používá `db.Execute` ke spuštění `Update` prohlášení, je přesměrován zpět na stránku výpis, kde můžete sledovat změny.

> [!TIP] 
> 
> **Jiný SQL příkazy, různé metody**
> 
> Možná jste si všimli, používáte mírně různé metody pro spouštění různých příkazů SQL. Ke spuštění `Select` dotaz to potenciálně vrací více záznamů, můžete použít `Query` metoda. Ke spuštění `Select` dotaz, který víte, bude vrátit jen jednu položku databáze, můžete použít `QuerySingle` metoda. Ke spuštění příkazů, které usnadňují změny, ale které nevrací položky databáze, můžete použít `Execute` metoda.
> 
> Budete muset mít různé metody, protože každý z nich vrátí odlišné výsledky, protože jste viděli již rozdíl mezi `Query` a `QuerySingle`. ( `Execute` Ve skutečnosti vrátí metoda hodnotu také &mdash; konkrétně, počet řádky databáze, které byly ovlivněny příkaz &mdash; ale můžete jste byla ignorována, dosavadní.)
> 
> Samozřejmě `Query` metoda může vrátit pouze jeden řádek databáze. Ale technologie ASP.NET vždy zpracuje výsledky `Query` metoda jako kolekce. I v případě, že metoda vrátí pouze jeden řádek, budete muset tento jeden řádek extrahovat z kolekce. Proto v situacích, kde jste *vědět* získáte zpět pouze jeden řádek, je bit pohodlnější použití `QuerySingle`.
> 
> Existuje několik metod, které provádějí konkrétní typy databázových operací. Můžete najít v seznamu metod databáze v [Stručná referenční příručka aplikace ASP.NET Web Pages rozhraní API](../../api-reference/asp-net-web-pages-api-reference.md#Data).


## <a name="making-validation-for-the-id-more-robust"></a>Provedení ověření informace ID robustní

Při prvním spuštění stránky, můžete získat ID film z řetězce dotazu, takže můžete přejít, že film získat z databáze. Můžete provedeny jistotu, že ve skutečnosti došlo hodnotu na vyhledání, která jste udělali s využitím tento kód:

[!code-csharp[Main](updating-data/samples/sample13.cs)]

Použít tento kód a ujistěte se, pokud uživatel získá k *EditMovies* stránky bez první výběru video v *filmy* stránky, na stránce se zobrazí uživatelsky přívětivý chybová zpráva. (Jinak, uživatelé by viz chybu, která je pravděpodobně pouze matou.)

Toto ověření není však nepříliš robustní. Stránka může být vyvolána také s tyto chyby:

- ID není číslo. Například může vyvolat stránku s adresou URL jako `http://localhost:nnnnn/EditMovie?id=abc`.
- ID je číslo, ale odkazuje na film, který neexistuje (například `http://localhost:nnnnn/EditMovie?id=100934`).

Pokud jste zvědaví, pokud chcete zobrazit chyby, které jsou výsledkem tyto adresy URL, spusťte *filmy* stránky. Vyberte video upravit a poté změňte adresu URL *EditMovie* stránky na adresu URL, která obsahuje abecedním ID nebo ID film neexistující.

Proto co můžete dělat? První je zajistit, že ne jenom je ID předána na stránku, ale že ID je celé číslo. Změnit kód `!IsPost` test, aby vypadala jako v tomto příkladu:

[!code-csharp[Main](updating-data/samples/sample14.cs)]

Na druhou podmínku, která jste přidali `IsEmpty` test, propojený s `&&` (logické a):

[!code-csharp[Main](updating-data/samples/sample15.cs)]

Může pamatovat z [Úvod do programování webových stránek ASP.NET](../introducing-razor-syntax-c.md) kurz, který jako metody `AsBool` `AsInt` převést na jiný datový typ řetězec znaků. `IsInt` – Metoda (a jiné, jako jsou `IsBool` a `IsDateTime`) jsou podobné. Ale testují pouze zda je *můžete* převést řetězec bez ve skutečnosti převod. Proto zde jste se v podstatě oznámením *Pokud hodnotu řetězce dotazu lze převést na celé číslo...* .

Potenciální problém hledá film, který neexistuje. Kód slouží k získání film vypadá tento kód:

[!code-csharp[Main](updating-data/samples/sample16.cs)]

Pokud předáte `movieId` hodnotu `QuerySingle` metoda, která neodpovídá skutečné film, nic nevrátí a příkazy, které následují (například `title=row.Title`) vést k chybám.

Je opět snadno opravu. Pokud `db.QuerySingle` metoda vrátí žádné výsledky `row` proměnná bude mít hodnotu null. Proto můžete zkontrolovat, zda `row` proměnná má hodnotu null, před pokusem o získání hodnoty z něj. Následující kód přidá `if` bloku kolem příkazy, které získávají hodnoty z `row` objektu:

[!code-csharp[Main](updating-data/samples/sample17.cs)]

Stránku s tyto dva další ověřovací testy, stane odrážka ověření. Kompletní kód `!IsPost` větve bude vypadat jako tento ukázkový:

[!code-csharp[Main](updating-data/samples/sample18.cs)]

Jsme budete jednou Upozorňujeme, že tato úloha je vhodné využít pro `else` bloku. Pokud nemáte jsou testy úspěšné, `else` bloky nastavit chybové zprávy.

## <a name="adding-a-link-to-return-to-the-movies-page"></a>Přidání odkazu na návrat na stránku filmy

Konečné a užitečné podrobností je přidáte propojení zpět *filmy* stránky. Obyčejnou toku události, uživatelé spustí ve *filmy* a klikněte na tlačítko **upravit** odkaz. Která integruje do *EditMovie* stránky, kde můžete upravit film a klikněte na tlačítko. Po kód zpracovává změnu, je přesměrován zpět *filmy* stránky.

Ale:

- Uživatel může rozhodnout nic nezmění.
- Uživatel může být, že podmínky na tuto stránku bez první kliknutí na **upravit** na odkaz v *filmy* stránky.

V obou případech chcete mohli snadno vrátit do hlavní výpis. Je snadno opravu &mdash; přidejte následující kód bezprostředně za ukončovací `</form>` značky v kódu:

[!code-html[Main](updating-data/samples/sample19.html)]

Tyto značky používají stejnou syntaxi pro `<a>` element, který jste se seznámili s jinde. Adresa URL obsahuje `~` rozumí "root webu."

## <a name="testing-the-movie-update-process"></a>Testování procesu aktualizace filmu

Nyní můžete otestovat. Spustit *filmy* a klikněte na tlačítko **upravit** vedle film. Když *EditMovie* se zobrazí stránka provádět změny film a klikněte na tlačítko **odeslání změn**. Jakmile se zobrazí seznam film, ujistěte se, že se změny zobrazují.

Chcete-li mít jistotu, že ověřování pracuje, klikněte na tlačítko **upravit** pro jiné film. Když dojde k *EditMovie* zrušte zaškrtnutí políčka **Genre** pole (nebo **roku** pole, nebo obě) a zkuste odeslat změny. Se zobrazí chyba, jak jste zvyklí:

![Upravit film stránky zobrazující chyby ověřování](updating-data/_static/image4.png)

Klikněte na tlačítko **vrátit do seznamu film** odkaz na zrušte změny a vrátit se *filmy* stránky.

## <a name="coming-up-next"></a>Objevuje další

V dalším kurzu se zobrazí, jak odstranit záznam film.

## <a name="complete-listing-for-movie-page-updated-with-edit-links"></a>Úplný seznam pro stránku Movie (aktualizovat pomocí odkazů pro úpravy)

[!code-cshtml[Main](updating-data/samples/sample20.cshtml)]

<a id="Complete_Page_Listing_for_EditMovie"></a>
## <a name="complete-page-listing-for-edit-movie-page"></a>Dokončení výpis stránky pro stránku film úpravy

[!code-cshtml[Main](updating-data/samples/sample21.cshtml)]

## <a name="additional-resources"></a>Další prostředky

- [Úvod do rozhraní ASP.NET Web programování pomocí syntaxe Razor](../introducing-razor-syntax-c.md)
- [Prohlášení aktualizace SQL](http://www.w3schools.com/sql/sql_update.asp) na webu W3Schools

>[!div class="step-by-step"]
[Předchozí](entering-data.md)
[další](deleting-data.md)
