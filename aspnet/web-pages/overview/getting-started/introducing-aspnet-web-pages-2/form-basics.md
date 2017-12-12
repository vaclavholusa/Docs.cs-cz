---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
title: "Představení technologie ASP.NET Web Pages – základy formuláře HTML | Microsoft Docs"
author: tfitzmac
description: "Tento kurz ukazuje základy toho, jak vytvořit vstupní formulář a jak se zpracovat vstup uživatele při použití technologie ASP.NET Web Pages (Razor). A teď kterou jste..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 81ed82bf-b940-44f1-b94a-555d0cb7cc98
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
msc.type: authoredcontent
ms.openlocfilehash: 97e4a2a1794dbdccf80f0b44c1246c743fa23019
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---html-form-basics"></a>Představení technologie ASP.NET Web Pages – základy formuláře HTML
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento kurz ukazuje základy toho, jak vytvořit vstupní formulář a jak se zpracovat vstup uživatele při použití technologie ASP.NET Web Pages (Razor). A teď, když máte k dispozici databáze, budete používat svoje dovednosti formuláře umožníte uživatelům najít konkrétní filmy v databázi. Předpokládá, že jste dokončili řady prostřednictvím [Úvod k zobrazení dat pomocí rozhraní ASP.NET Web Pages](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data).
> 
> Získáte informace:
> 
> - Jak vytvořit formulář pomocí standardní elementů HTML.
> - Jak si uživatel vstupního ve formuláři.
> - Jak vytvořit dotaz SQL, selektivně získá data pomocí hledání termín, uživatel poskytuje.
> - Jak obsahovat pole na stránce "nezapomeňte" co uživatel zadal.
>   
> 
> Funkce nebo technologie popsané:
> 
> - `Request` Objektu.
> - SQL `Where` klauzule.


## <a name="what-youll-build"></a>Co budete sestavení

V předchozích kurzu vytvořili databázi, do ní přidat data a pak použít `WebGrid` pomocné rutiny pro zobrazení dat. V tomto kurzu přidáte vyhledávací pole, která umožňuje najít filmy konkrétní genre nebo jejichž název obsahuje ať slovo, které zadáte. (Například budete moci najít všechny filmy, jehož genre je "Action" nebo jejichž název obsahuje "Harry Sverdlove" nebo "Adventure.")

Po dokončení tohoto kurzu, budete mít stránky podobné následujícímu:

![Stránka filmy s Genre a název vyhledávání](form-basics/_static/image1.png)

Seznam součástí stránky je stejný jako poslední kurzu &mdash; mřížka. Rozdíl bude, že mřížky zobrazí pouze filmy, že jste hledali.

## <a name="about-html-forms"></a>O formuláře HTML

(Pokud máte zkušenosti s vytvářením formuláře HTML a s rozdíl mezi `GET` a `POST`, můžete tuto část přeskočit.)

Formulář má elementy vstupu uživatele &mdash; textová pole, tlačítka, přepínače, zaškrtněte políčka, rozevírací seznamy a tak dále. Uživatelé vyplnit tyto ovládací prvky nebo možnosti a pak kliknutím na tlačítko odeslání formuláře.

Tento příklad je zobrazená základní syntaxe HTML ve tvaru:

[!code-html[Main](form-basics/samples/sample1.html)]

Když tento kód spustí na stránce, vytvoří jednoduchý formulář, který vypadá jako na obrázku:

![Základní formuláře HTML jako vykreslené v prohlížeči](form-basics/_static/image2.png)

`<form>` Element uzavře elementů HTML k odeslání. (Snadno chybou, aby je přidat elementy na stránce, ale pak zapomněli jejich uvnitř umístění `<form>` elementu. V takovém případě nic je odeslat.) `method` Atribut sděluje prohlížeči, jak odeslat vstup uživatele. Nastavíte `post` při provádění aktualizace na serveru nebo na `get` Pokud jste právě načítání dat ze serveru.

<a id="GET,_POST,_and_HTTP_Verb_Safety"></a>

> [!TIP] 
> 
> **GET, POST a akce HTTP zabezpečení**
> 
> Protokolu HTTP, protokol, který prohlížeče a servery používají k výměně informací o, je pozoruhodně jednoduchý v jeho základní operace. V prohlížečích používá jenom pár operace provádět požadavky na servery. Při psaní kódu pro web, je vhodné se seznámit s tyto příkazy a jak je používat prohlížečem a serverem. Na nejčastěji používané příkazy a daleko jsou tyto:
> 
> - `GET`. Prohlížeč používá tento příkaz se něco načíst ze serveru. Například když zadáte adresu URL do prohlížeče, prohlížeč provede `GET` operaci žádosti požadovanou stránku. Pokud stránka obsahuje grafiky, prohlížeč provede další `GET` operations získat bitové kopie. Pokud `GET` operace obsahuje předávat informace na server, se předá informace jako část adresy URL v řetězci dotazu.
> - `POST`. Odešle do prohlížeče `POST` požadavek, aby bylo možné odeslat data přidat nebo změnit na serveru. Například `POST` příkaz slouží k vytváření záznamů v databázi nebo změnit stávající. Ve většině případů, kdy vyplňte formulář a klikněte na tlačítko pro odeslání, Prohlížeč provádí `POST` operaci. V `POST` operace, data předávány na server je v těle stránky.
> 
> Je zásadní rozdíl mezi tyto příkazy, které `GET` operace nemá změnit nic na serveru – a umístí jej něco abstraktnějšího způsobem, `GET` operaci nedojde ke změně stavu na serveru. Můžete provádět `GET` operace na stejné prostředky jako tolikrát, kolikrát se vám líbí a tyto prostředky se nezmění. (A `GET` operaci často říká, že je jako "bezpečnou", nebo použít technický termín, je *idempotent*.) Naproti tomu samozřejmě `POST` žádost o něco na serveru změní pokaždé, když provádíte operaci.
> 
> Pomůže dva příklady ilustrují tento rozdíl. Při hledání pomocí modul jako Bing nebo Google, abyste vyplnili formulář, který se skládá z jednoho textového pole a klikněte na tlačítko Hledat. Provede prohlížeče `GET` operace s hodnotou zadanou do pole předanou v rámci adresy URL. Použití `GET` operace pro tento typ formuláře je vhodná, protože operace vyhledávání nezmění žádné prostředky na serveru, stačí načte informace.
> 
> Nyní v úvahu proces řazení něco online. Zadejte podrobnosti pořadí a pak klikněte na tlačítko pro odeslání. Tato operace bude `POST` požadavek, protože operace bude mít za následek změny na serveru, jako je například nový záznam pořadí, změnu informací o vašem účtu a případně mnoho dalších změn. Na rozdíl od `GET` operaci, nelze opakovat vaší `POST` žádost – Pokud jste to udělali, pokaždé, když byly opětovně odeslány žádost, by vytvořit novou objednávku na serveru. (V takových případech weby často upozornění, není k více než jednou klikněte na tlačítko pro odeslání, nebo bude zakázat tlačítko pro odeslání, takže je nemusíte formulář znovu odešlete omylem.)
> 
> V průběhu tohoto kurzu budete používat i `GET` operace a `POST` operace pro práci s formuláře HTML. Vysvětlíme v každém případu důvod, proč je operace, které můžete použít příslušné.
> 
> (Další informace o příkazy HTTP, najdete v článku [metoda definice](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) článku na webu W3C.)


Většina elementy vstupu uživatele jsou HTML `<input>` elementy. Zobrazují se jako `<input type="type" name="name">,` kde *typ* určuje druh uživatele vstupního ovládacího prvku chcete. Tyto prvky jsou běžné těm, které jsou:

- Textové pole:`<input type="text">`
- Zaškrtněte políčko:`<input type="check">`
- Přepínač:`<input type="radio">`
- Tlačítko:`<input type="button">`
- Tlačítko odešlete:`<input type="submit">`

Můžete také `<textarea>` elementu, který chcete vytvořit víceřádkové textové pole a `<select>` elementu, který chcete vytvořit rozevíracím seznamu nebo posuvný seznamu. (Další informace o HTML formuláři elementy najdete v části [formuláře HTML a vstup](http://www.w3schools.com/html/html_forms.asp) na webu W3Schools.)

`name` Atribut je velmi důležité, protože název je, jak získáte hodnota elementu později, jak se krátce zobrazí.

Zajímavé část je, vývojář stránky provést se vstupem uživatele. Není k dispozici žádné předdefinované chování spojené s těmito elementy. Místo toho budete muset získat hodnoty, které má uživatel zadaný nebo vybrané a dělat něco s nimi. To je, získáte informace v tomto kurzu.

> [!TIP] 
> 
> **HTML5 a vstupních formulářů**
> 
> Možná jste zjistili, HTML je v přechodném stavu a na nejnovější verzi (HTML5) zahrnuje podporu pro více intuitivní způsoby pro uživatele k zadání informací. Například v jazyce HTML5, (stránka vývojáře) zjistíte stránky chcete, aby uživatel zadal datum. V prohlížeči můžete pak automaticky zobrazí kalendář místo nutnosti uživatelům ručně zadejte datum. Však HTML5 je nová a zatím nepodporuje v všechny prohlížeče.
> 
> ASP.NET – webové stránky podporuje HTML5 vstup, pokud nemá prohlížeče uživatele. Pro představu o nové atributy `<input>` element v jazyce HTML5, najdete v části [HTML &lt;vstupní&gt; atribut typu](http://www.w3schools.com/html/html_form_input_types.asp) na webu W3Schools.


## <a name="creating-the-form"></a>Vytvoření formuláře

Ve službě WebMatrix v **soubory** pracovního prostoru, otevřete *Movies.cshtml* stránky.

Po zavření `</h1>` značky a před otevření `<div>` značky `grid.GetHtml` volání, přidejte následující kód:

[!code-html[Main](form-basics/samples/sample2.html)]

Tento kód vytvoří formulář obsahující textové pole s názvem `searchGenre` a tlačítko pro odeslání. Text pole a tlačítko jsou uzavřené v odeslání `<form>` element jejichž `method` je nastavena na hodnotu `get`. (Nezapomeňte, že pokud nemáte put textového pole a tlačítko uvnitř odeslat `<form>` elementu, nic se po kliknutí na tlačítko Odeslat.) Můžete použít `GET` příkaz zde vzhledem k tomu, že vytváříte formulář, nebyly provedeny žádné změny na serveru – je právě výsledkem vyhledávání. (V tomto kurzu předchozí jste použili `post` metoda, která je, jak odeslat změny na server. Uvidíte, v dalším kurzu znovu.)

Spuštění stránky. I když nebyly definovány žádné chování pro daný formulář, najdete v článku bude vypadat takto:

![Stránka filmy s vyhledávacího pole pro Genre](form-basics/_static/image3.png)

Zadejte hodnotu do textového pole, jako je "Komedie." Pak klikněte na tlačítko **vyhledávání Genre**.

Poznamenejte si adresu URL stránky. Vzhledem k tomu, že nastavíte `<form>` elementu `method` atribut `get`, je zadaná hodnota je teď součástí řetězce dotazu v adrese URL, například takto:

`http://localhost:45661/Movies.cshtml?searchGenre=Comedy`

## <a name="reading-form-values"></a>Čtení hodnot formuláře

Tato stránka již obsahuje některé kód, který získá data databáze a zobrazí výsledky v mřížce. Nyní máte přidat nějaký kód, který čte hodnota textového pole, takže je možné spustit dotaz SQL, který obsahuje hledaný termín.

Protože nastavena metoda formuláře na `get`, si můžete přečíst hodnotu, která byla zadán do textového pole pomocí kódu takto:

`var searchTerm = Request.QueryString["searchGenre"];`

`Request.QueryString` Objekt ( `QueryString` vlastnost `Request` objektu) obsahuje hodnoty elementů, které byly odeslány jako součást `GET` operaci. `Request.QueryString` Vlastnost obsahuje *kolekce* (list) hodnoty, které jsou odeslány ve formuláři. Chcete-li získat všechny jednotlivé hodnoty, zadáte název elementu, který chcete. Proto musíte mít `name` atributu u `<input>` – element (`searchTerm`) vytvářející textového pole. (Další informace o tom `Request` objektu, najdete v článku [bočním panelu](#BKMK_TheRequestObject) později.)

Je dostatečně jednoduchá načíst hodnotu textového pole. Ale pokud uživatel nebyl zadejte nic vůbec do textového pole, ale kliknutí na **vyhledávání** i přesto, můžete ignorovat kliknutím, vzhledem k tomu, že žádná data pro vyhledávání.

Následující kód je příklad, který ukazuje, jak implementovat tyto podmínky. (Přidat tento kód ještě nemáte, můžete to udělat za chvíli.)

[!code-csharp[Main](form-basics/samples/sample3.cs)]

Test rozpis tímto způsobem:

- Získá hodnotu `Request.QueryString["searchGenre"]`, konkrétně hodnotu, která byla zadán do `<input>` element s názvem `searchGenre`.
- Zjistěte, jestli je prázdný pomocí `IsEmpty` metoda. Tato metoda je standardní způsob jak určit, zda něco (například element form) obsahuje hodnotu. Ale skutečně, které jsou pro vás pouze v případě, že má *není* prázdný, proto...
- Přidat `!` operátor před `IsEmpty` otestovat. ( `!` Operátor znamená logická NEGACE).

Zjednodušeně celý `if` podmínku překládá do následující: *Pokud formuláře searchGenre element není prázdný, pak...*

Tento blok nastaví fázi pro vytvoření dotazu, který používá hledaný termín. Můžete to udělat v další části.

<a id="BKMK_TheRequestObject"></a>

> [!TIP] 
> 
> **Objekt požadavku**
> 
> `Request` Objekt obsahuje všechny informace, které prohlížeč odesílá do vaší aplikace po požadovaný nebo odeslání stránky. Tento objekt obsahuje všechny informace, které uživatel nezadá jako textové pole hodnoty nebo soubor k odeslání. Zahrnuje také nejrůznějším Další informace, jako jsou soubory cookie, hodnot v řetězci dotazu adresy URL (pokud existuje), cesta k souboru stránky, která je spuštěná, typu prohlížeče, které uživatel používá, seznam jazyků, které jsou nastaveny v prohlížeči a mnohem víc.
> 
> `Request` Objekt *kolekce* (list) hodnot. Jednotlivé hodnoty z kolekce získáte zadáním názvu:
> 
> `var someValue = Request["name"];`
> 
> `Request` Objekt ve skutečnosti poskytuje několik podmnožin. Příklad:
> 
> - `Request.Form`poskytuje hodnoty z elementů uvnitř odeslané `<form>` element, je-li žádost `POST` požadavku.
> - `Request.QueryString`vám právě hodnot v řetězci dotazu adresu URL. (V adrese URL jako `http://mysite/myapp/page?searchGenre=action&page=2`, `?searchGenre=action&page=2` část adresy URL je řetězec dotazu.)
> - `Request.Cookies`kolekce umožňuje přístup k soubory cookie, které prohlížeč odeslal.
> 
> Chcete-li získat hodnotu, která víte, je v odeslané podobě, můžete použít `Request["name"]`. Alternativně můžete použít více konkrétních verzí `Request.Form["name"]` (pro `POST` požadavky) nebo `Request.QueryString["name"]` (pro `GET` požadavky). Samozřejmě *název* je název položky získat.
> 
> Název položky, které chcete získat musí být jedinečný v rámci kolekce, kterou používáte. Proto `Request` objekt poskytuje, jako jsou podmnožiny `Request.Form` a `Request.QueryString`. Předpokládejme, že stránka obsahuje element form s názvem `userName` a *také* obsahuje soubor cookie s názvem `userName`. Pokud se zobrazí `Request["userName"]`, je nejednoznačné, jestli chcete hodnot formulářů nebo soubor cookie. Ale pokud získáte `Request.Form["userName"]` nebo `Request.Cookie["userName"]`, jste právě explicitní o hodnotu, která potřebujete.
> 
> Je dobrým zvykem konkrétní a použít podmnožinu `Request` , co vás zajímá, jako je `Request.Form` nebo `Request.QueryString`. Jednoduché stránek, které vytváříte v tomto kurzu je pravděpodobně nemá je skutečně žádný rozdíl. Ale při vytváření složitějších stránky, pomocí explicitní verze `Request.Form` nebo `Request.QueryString` vám může pomoct vyhnout problémům, které mohou nastat, když se tato stránka obsahuje formuláře (nebo více formulářů), soubory cookie, hodnoty řetězců dotazu a tak dále.


## <a name="creating-a-query-by-using-a-search-term"></a>Vytvoření dotazu pomocí hledaný termín

Teď, když víte, jak získat hledaný termín, který uživatel zadal, můžete vytvořit dotaz, který se používá. Mějte na paměti, že všechny položky film z databáze nástroje získáte používáte dotaz SQL, který vypadá jako tento příkaz:

`SELECT * FROM Movies`

Potřebujete jenom určité filmy, budete muset použít dotaz, který zahrnuje `Where` klauzule. Tuto klauzuli umožňuje nastavit podmínky, na kterém jsou řádky vrácené dotazem. Tady je příklad:

`SELECT * FROM Movies WHERE Genre = 'Action'`

Základní formát je `WHERE column = value`. Můžete použít různé operátory kromě právě `=`, například `>` (větší než), `<` (méně než), `<>` (nerovná se), `<=` (je menší než nebo rovno), atd., v závislosti na tom, co hledáte.

V případě, že přemýšlíte, příkazy SQL nejsou velká a malá písmena &mdash; `SELECT` je stejný jako `Select` (nebo i `select`). Ale osoby často velká počáteční klíčová slova v příkazu jazyka SQL, jako je třeba `SELECT` a `WHERE`, aby bylo snazší ke čtení.

### <a name="passing-the-search-term-as-a-parameter"></a>Předání jako parametr hledaný termín

Hledání pro konkrétní genre je dostatečně snadno (`WHERE Genre = 'Action'`), ale chcete mít možnost vyhledat všechny genre, který uživatel zadá. K tomu, je vytvořit jako dotaz SQL, který obsahuje zástupný symbol pro hodnotu k vyhledání. Ho bude vypadat jako tento příkaz:

`SELECT * FROM Movies WHERE Genre = @0`

Je zástupný symbol `@` následovaný nula. Jak můžou snadno uhodnout, dotaz může obsahovat více zástupných symbolů a by se jmenovala `@0`, `@1`, `@2`atd.

Nastavit dotaz a ve skutečnosti je předat hodnotu, je použít kód takto:

[!code-sql[Main](form-basics/samples/sample4.sql)]

Tento kód je podobná co již bylo provedeno k zobrazení dat v mřížce. Pouze rozdíly jsou:

- Dotaz obsahuje zástupný znak (`WHERE Genre = @0"`).
- Dotaz je uvést do proměnné (`selectCommand`); před přímo na předán dotaz `db.Query` metoda.
- Při volání `db.Query` metoda, předáte dotaz a hodnota, která má použít pro zástupný symbol. (Pokud dotaz měli více zástupných symbolů, by je předat všechny jako samostatné hodnoty metodě.)

Pokud jste v kostce řečeno tyto prvky, získáte následující kód:

[!code-csharp[Main](form-basics/samples/sample5.cs)]

> [!NOTE] 
> 
> **Důležité!** Pomocí zástupné symboly (například `@0`) mají být předány hodnoty příkaz SQL je *velmi důležité* pro zabezpečení. Způsob, jakým jste ji tady vidíte, se zástupnými symboly pro různá data, je jediným způsobem, jak je potřeba vytvořit příkazy SQL.
> 
> Nikdy vytvoření příkazu SQL umístěním společně (spojováním) literálu text a hodnoty, které můžete získat z uživatele. Zřetězení uživatelský vstup do příkazu SQL otevře váš web, *útok prostřednictvím injektáže SQL* kde uživatel se zlými úmysly odešle hodnoty na stránku, které zabezpečení vaší databáze. (Další informace v článku [Injektáž SQL](https://msdn.microsoft.com/en-us/library/ms161953.aspx) webu MSDN.)


## <a name="updating-the-movies-page-with-search-code"></a>Aktualizací stránky filmy vyhledávání kódu

Teď můžete aktualizovat kód *Movies.cshtml* souboru. Pokud chcete začít, nahraďte kód v bloku kódu v horní části stránky s tímto kódem:

[!code-csharp[Main](form-basics/samples/sample6.cs)]

Rozdíl je, že jste vložte dotaz do `selectCommand` proměnné, které budete předat `db.Query` později. Vložení příkaz jazyka SQL do proměnné umožňuje změnit příkazu, které se to udělat provést hledání.

Rovněž jsme odstranili tyto dva řádky, které jste budete vrátit zpět k později:

[!code-csharp[Main](form-basics/samples/sample7.cs)]

Nechcete spusťte dotaz ještě (to znamená, volání `db.Query`) a nechcete, aby k chybě při inicializaci `WebGrid` pomocné rutiny, ale buď. Poté, co jste započítáno se má spuštěný, které příkaz jazyka SQL, můžete to udělat všechny tyto věci.

Po tento rewritten blok můžete přidat nové logiku pro zpracování vyhledávání. Dokončený kód bude vypadat následovně. Aktualizujte kód na stránku tak, aby odpovídala tento příklad:

[!code-cshtml[Main](form-basics/samples/sample8.cshtml)]

Stránka teď funguje takto. Při každém spuštění stránky kód otevře databáze a `selectCommand` proměnná je nastavená na příkazu SQL, který získá všechny záznamy z `Movies` tabulky. Kód také inicializuje `searchTerm` proměnné.

Ale pokud aktuální požadavek obsahuje hodnotu pro `searchGenre` element, nastaví kód `selectCommand` na jiný dotaz –, konkrétně k, které obsahuje `Where` klauzule k vyhledání genre. Nastaví taky `searchTerm` ať byl předán pro do vyhledávacího pole (který může být nic).

Bez ohledu na to, které SQL příkazu je v `selectCommand`, kód pak zavolá `db.Query` spusťte dotaz, předání ať je v `searchTerm`. Pokud není nic `searchTerm`, nezáleží, protože v takovém případě není žádný parametr předat hodnotu pro `selectCommand` přesto.

Nakonec kód inicializuje `WebGrid` pomocná pomocí výsledky dotazu, stejně jako předtím.

Můžete uvidíte, že umístěním příkaz jazyka SQL a hledaný termín do proměnné, kterou jste přidali flexibilitu do kód. Jak uvidíte později v tomto kurzu, můžete použít tento základní architekturu a pokračujte v přidávání logiku pro různé typy vyhledávání.

## <a name="testing-the-search-by-genre-feature"></a>Testování funkce vyhledávání Genre

Ve službě WebMatrix, spusťte *Movies.cshtml* stránky. Zobrazí se stránka s textového pole pro genre.

Zadejte genre, který jste zadali pro jednu ze svých záznamech testu a pak klikněte na **vyhledávání**. Tentokrát uvidíte seznam právě filmy, které odpovídají této genre:

![Stránka filmy výpis po hledání genre Comedies](form-basics/_static/image4.png)

Zadejte jiný genre a hledat znovu. Zkuste zadat genre prostřednictvím všechna všechna písmena velká nebo malá písmena, takže uvidíte, že vyhledávání není velká a malá písmena.

## <a name="remembering-what-the-user-entered"></a>"Nezapomeňte" co uživatel zadal

Možná jste si všimli, poté, co jste zadali genre a kliknutí na **vyhledávání Genre**, jste viděli v seznamu, pro nějž. Ale textového pole pro vyhledávání byla prázdná &mdash; jinými slovy, nebyla pamatovat, co jste nezadali.

Je důležité pochopit, proč k tomuto chování dochází. Při odesílání stránky prohlížeč odešle požadavek na webový server. Když ASP.NET získá požadavek, vytvoří instanci zcela nové stránky, spuštěním kódu v ní a pak znovu vykreslí stránku v prohlížeči. Platit ale stránky neví, že jste právě pracovali s předchozí verzí sám sebe. Všechny že bude vědět, že je to byla přijata žádost, která měla některé data formuláře v ní.

Pokaždé, když požadavek stránku &mdash; zda poprvé nebo odesláním ji &mdash; vám nová stránka. Webový server má nedostatek paměti u vaší poslední žádosti. Ani nepodporuje technologii ASP.NET a ani se v prohlížeči. Pouze připojení mezi tyto samostatné instance stránky je žádná data, která přenášet mezi nimi. Pokud odešlete stránku, například novou instanci stránky mohou získat data formuláře, který vám byl zaslán starší instance. (Další způsob předávání dat mezi stránkami je používat soubory cookie.)

Formální způsob, jak popisuje tato situace je to, že webové stránky jsou *bezstavové*. Webové servery a vlastních stránek a elementy na stránce neudržují žádné informace o stavu předchozí stránky. Web byl navržen tímto způsobem, protože zachování stavu pro jednotlivé požadavky by rychle vyčerpat prostředky webových serverů, které často zpracování tisíc, možná i stovky tisíc požadavků za sekundu.

Tak, aby se důvod, proč byla textového pole prázdná. Po odeslání stránky ASP.NET vytvoří novou instanci třídy stránky a spustili prostřednictvím kódu a kódu. V tomto kód, který vás vyzval technologii ASP.NET pro vložení hodnoty do textového pole nic došlo. Proto ASP.NET nereaguje, a bez hodnoty v něm byl vykreslen textového pole.

Je ve skutečnosti snadný způsob, jak získat chcete tento problém vyřešit. Genre, kterou jste zadali do textového pole *je* k dispozici v kódu &mdash; je v `Request.QueryString["searchGenre"]`.

Aktualizujte kód pro textové pole tak, aby `value` atribut získá svou hodnotu z `searchTerm`, jako jsou v tomto příkladu:

[!code-html[Main](form-basics/samples/sample9.html?highlight=1)]

Na této stránce můžete mít také nastavit `value` atribut `searchTerm` proměnné, protože tuto proměnnou také obsahuje genre jste zadali. Ale pomocí `Request` objekt, který chcete nastavit `value` atributu, jak je vidět tady je standardní způsob k provedení této úlohy. (Za předpokladu, že chcete i tomu &mdash; v některých případech můžete chtít vykreslení stránky *bez* hodnoty v polích. Všechny závisí na co se děje s vaší aplikací.)

> [!NOTE]
> "Nepamatujete" hodnota textové pole, které se používá pro hesla. Je bezpečnostní riziko umožnit uživatelům vyplnit pole pro heslo pomocí kódu.


Znovu spusťte stránky, zadejte genre a klikněte na **vyhledávání Genre**. Tentokrát nejen zobrazí výsledky hledání, ale textového pole pamatuje, co jste naposledy zadali:

![Stránky zobrazující, že textového pole má 'uloží' předchozí položce](form-basics/_static/image5.png)

## <a name="searching-for-any-word-in-the-title"></a>Hledání libovolného slova v názvu

Nyní můžete vyhledat všechny genre, ale můžete také vyhledat název. Je obtížné získat název zcela správně při hledání, takže místo toho že můžete vyhledat slovo, které se zobrazí kdekoli uvnitř název. To lze provést v systému SQL, můžete použít `LIKE` operátor a syntaxe, podobně jako následující:

`SELECT * FROM Movies WHERE Title LIKE '%adventure%'`

Tento příkaz načte všechny filmy, jejichž názvy obsahovat "adventure". Při použití `LIKE` operátor, zahrnují zástupného znaku `%` jako součást hledaný termín. Hledání `LIKE 'adventure%'` znamená "od 'adventure'". (Technicky znamená "Řetězec, který následuje nic adventure.") Podobně hledaný termín `LIKE '%adventure'` znamená "jakýkoli následuje řetězec 'adventure'", což je jiný způsob, jak můžete "ukončuje s 'adventure'".

Hledaný termín `LIKE '%adventure%'` proto znamená "s"adventure' kdekoli v názvu." (Technicky "nic v názvu, za nímž následuje 'adventure', za nímž následuje nic.")

Uvnitř `<form>` elementu, přidejte následující značky vpravo pod ukončovací `</div>` značky pro hledání genre (těsně před uzavírací `</form>` element):

[!code-html[Main](form-basics/samples/sample10.html)]

Kód pro zpracování tohoto hledání je podobný kódu pro hledání genre, s tím rozdílem, že máte ke kompilaci `LIKE` vyhledávání. Uvnitř bloku kódu v horní části stránky, přidejte tuto `if` blokovat jenom po `if` blok pro hledání genre:

[!code-csharp[Main](form-basics/samples/sample11.cs)]

Tento kód používá stejné logiky, která jste viděli dříve, s tím rozdílem, že používá hledání `LIKE` operátor a PUT kód "`%`" před a po hledaný termín.

Všimněte si, jak se snadno přidat další hledání na stránku. Všechny, které jste museli provést byla:

- Vytvoření `if` blok, který testován, zda měl relevantní vyhledávacího pole zadejte hodnotu.
- Nastavte `selectCommand` proměnnou nový příkaz SQL.
- Nastavte `searchTerm` proměnné na hodnotu k předání do dotazu.

Tady je blok dokončení kódu, který obsahuje nové logiku pro hledání názvu:

[!code-cshtml[Main](form-basics/samples/sample12.cshtml)]

Zde je souhrn jaké tento kód:

- Proměnné `searchTerm` a `selectCommand` jsou inicializovány v horní části. Chcete-li nastavit tyto proměnné na příslušné hledaný termín (pokud existuje) a příslušný příkaz SQL založený na uživatele nemá na stránce. Výchozí nastavení hledání je jednoduchý případ všechny filmy získat z databáze.
- V testech pro `searchGenre` a `searchTitle`, nastaví kód `searchTerm` na hodnotu, kterou chcete vyhledat. Tyto bloky kódu nastavit také `selectCommand` příslušný příkaz SQL pro toto hledání.
- `db.Query` Metoda je volána pouze jednou, pomocí ať příkaz SQL je v `selectedCommand` a ať hodnota `searchTerm`. Pokud neexistuje žádný hledaný termín (žádné genre a žádné slovo title), hodnota `searchTerm` je prázdný řetězec. Ale, který není důležité, protože v takovém případě dotaz nevyžaduje parametr.

## <a name="testing-the-title-search-feature"></a>Testování funkce hledání názvu

Nyní můžete otestovat stránku dokončené vyhledávání. Spustit *Movies.cshtml*.

Zadejte genre a klikněte na **vyhledávání Genre**. V mřížce zobrazené filmy genre, jako je před.

Zadejte název word a klikněte na **vyhledávání název**. V mřížce zobrazené filmy, které mají v názvu aplikace word.

![Stránka filmy výpis po hledání pro "Na" v názvu](form-basics/_static/image6.png)

Ponechat prázdné obou polí a buď tlačítko. V mřížce zobrazené všechny filmy.

## <a name="combining-the-queries"></a>Kombinování dotazy

Můžete si všimnout, že se hledání, které můžete provádět nevylučují. Název a genre nelze hledat ve stejnou dobu, i když obě pole hledání obsahují hodnoty. Například nelze vyhledat všechny akce filmy, jejíž název obsahuje "Adventure". (Jako stránky je zakódovaný nyní, pokud zadejte hodnoty pro genre a název, získá název vyhledávání prioritu.) Pokud chcete vytvořit hledání, které kombinuje podmínky, je nutné vytvořit dotaz SQL, který má následující syntaxi:

`SELECT * FROM Movies WHERE Genre = @0 AND Title LIKE @1`

A budete muset spustit dotaz pomocí příkazu následujícím způsobem (přibližně hovořícího):

`var selectedData = db.Query(selectCommand, searchGenre, searchTitle);`

Vytváření logiku, která umožní velký počet permutací kritéria vyhledávání můžete zapojení trochu, jak můžete vidět. Proto jsme už nebudou sem.

## <a name="coming-up-next"></a>Objevuje další

V dalším kurzu vytvoříte stránky, která používá formuláře tak, aby uživatelé přidat filmy do databáze.

## <a name="complete-listing-for-movie-page-updated-with-search"></a>Úplný seznam pro stránku Movie (aktualizováno vyhledávání)

[!code-cshtml[Main](form-basics/samples/sample13.cshtml)]

## <a name="additional-resources"></a>Další prostředky

- [Úvod do rozhraní ASP.NET Web programování pomocí syntaxe Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Klauzule WHERE SQL](http://www.w3schools.com/sql/sql_where.asp) na webu W3Schools
- [Metoda definice](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) článku na webu W3C

>[!div class="step-by-step"]
[Předchozí](displaying-data.md)
[další](entering-data.md)
