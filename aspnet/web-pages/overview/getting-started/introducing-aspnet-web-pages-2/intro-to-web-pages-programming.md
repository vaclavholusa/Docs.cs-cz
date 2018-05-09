---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
title: Představení technologie ASP.NET Web Pages – programování základy | Microsoft Docs
author: tfitzmac
description: 'V tomto kurzu získáte přehled o programu na webových stránkách ASP.NET se syntaxí Razor. Co se dozvíte: základní syntaxe, Razor, který používáte pro pr...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/17/2015
ms.topic: article
ms.assetid: 7526ed45-a97d-4e8a-8301-01324ef0eff9
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
msc.type: authoredcontent
ms.openlocfilehash: 60115dd06a27bf856427953de29e993194afb991
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2018
---
<a name="introducing-aspnet-web-pages---programming-basics"></a>Představení technologie ASP.NET Web Pages – základy programování
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> V tomto kurzu získáte přehled o programu na webových stránkách ASP.NET se syntaxí Razor.
> 
> Získáte informace:
> 
> - Základní syntaxe "Razor", který používáte pro programování na webových stránkách ASP.NET.
> - Některé základní C#, což je programovací jazyk, který budete používat.
> - Některé základní koncepty programování pro webové stránky.
> - Postup instalace balíčků (součásti, které obsahují předem kódu) pro použití s vaší lokality.
> - Jak používat *Pomocníci* k provádění běžných úloh programování.
>   
> 
> Funkce nebo technologie popsané:
> 
> - A Správce balíčků NuGet.
> - `Gravatar` Pomocné rutiny.


V tomto kurzu je primárně cvičení v Úvod k programování syntaxe, které budete používat pro ASP.NET Web Pages. Získáte informace o *syntaxe Razor* a kód, který je napsán v C# programovací jazyk. Z této syntaxe širokého jste získali v předchozí kurzu; v tomto kurzu vysvětlíme další syntaxe.

Promise jsme, že tento kurz zahrnuje většinu programování se zobrazí jeden kurzu, a že je pouze kurz, který je *pouze* o programování. Ve zbývající kurzy v této sadě ve skutečnosti vytvoříte stránky, které provádějí zajímavé věcí.

Budete také další informace o *Pomocníci*. Pomocné rutiny je komponenta – zabalené up úsek kódu – které můžete přidat na stránku. Pomocné rutiny provede práci za vás, který jinak může být zdlouhavé nebo komplexní udělat ručně.

## <a name="creating-a-page-to-play-with-razor"></a>Vytvoření stránky můžete experimentovat s Razor

V této části budete přehrávání trochu s jádrem Razor, kde získáte představu o základní syntaxe.

Spusťte službu WebMatrix, pokud je již spuštěn. Budete používat web, který jste vytvořili v předchozí kurzu ([získávání spuštěna s webové stránky](https://go.microsoft.com/fwlink/?LinkId=251578)). Chcete-li ho znovu otevřít, klikněte na tlačítko **osobní weby** a zvolte **WebPageMovies**:

![Služba WebMatrix úvodní obrazovka zobrazující možnosti otevřete lokality a osobní weby zvýrazněná](intro-to-web-pages-programming/_static/image1.png)

Vyberte **soubory** pracovního prostoru.

Na pásu karet klikněte na tlačítko **nový** vytvoření stránky. Vyberte **CSHTML** a zadejte název nové stránky *TestRazor.cshtml*.

Click **OK**.

Zkopírujte následující do souboru úplného nahrazení, co již existuje.

> [!NOTE]
> Při kopírování kód nebo kód z příkladů do stránky odsazení a zarovnání nemusí být stejné jako v tomto kurzu. Odsazení a zarovnání nemá vliv, jak kód spustí, když.


[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample1.cshtml)]

## <a name="examining-the-example-page"></a>Zkoumání na příkladu stránky

Většina zobrazené je obyčejnou HTML. V horní části je však tento blok kódu:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample2.cshtml)]

Všimněte si následujících akcí o tento blok kódu:

- Znak @ informuje ASP.NET že následuje kódu Razor, není ve formátu HTML. ASP.NET bude považovat všechno po @ – znak jako kód, dokud se spouští se znovu do některé kódu HTML. (V tomto případě to je &lt;! DOCTYPE&gt; elementu.
- Složené závorky ({a}) uzavřete blok kódu Razor, pokud kód obsahuje více než jeden řádek. Složené závorky řekněte ASP.NET, kde kód pro tento blok spuštění a ukončení.
- / / Komentář označte znaků – tedy součástí kód, který nebude provést.
- Každý příkaz musí končit středníkem (;). (Není komentář, ale.)
- Můžete uložit hodnoty v *proměnné*, který vytvoříte (*deklarovat*) s var. – klíčové slovo Při vytváření proměnné, můžete mu název, který může obsahovat písmena, číslice a podtržítka (\_). Názvy proměnných nesmí začínat číslicí a nelze použít název programování – klíčové slovo (například var).
- Řetězce znaků (např. "ASP.NET" a "Webové stránky") je uzavřít do uvozovek. (Musí být dvojitých uvozovek.) Čísla nejsou v uvozovkách.
- Prázdné znaky mimo uvozovky není důležité. Konce řádků většinou nezáleží; výjimkou je, že řetězec v uvozovkách nelze rozdělit na řádcích. Odsazení a zarovnání nejsou důležité.

Něco, co není zřejmé z tohoto příkladu je, že veškerý kód je malá a velká písmena. To znamená, že je proměnné TheSum proměnnou jiný než proměnné, které může mít název theSum nebo thesum. Podobně var je klíčové slovo, ale Var není.

### <a name="objects-and-properties-and-methods"></a>Objekty a vlastnosti a metody

Pak je výraz DateTime.Now. Jednoduše řečeno, data a času je *objekt*. Je-li objekt věcí, kterou můžete programu s – na stránce textové pole, soubor, bitovou kopii, webové žádosti, e-mailovou zprávu, záznam zákazníka, atd. Objekty mají jednu nebo více *vlastnosti* popisují jejich vlastnosti. Objekt textové pole má vlastnost Text (mimo jiné), objekt žádosti má vlastnost adresa Url (a dalších), e-mailovou zprávu má vlastnost From a na vlastnost, a tak dále. Objekty mají také *metody* , které jsou "akce" mohou provádět. Můžete budete pracovat s objekty mnoho.

Jak je vidět z příkladu, data a času je objekt, který vám umožní program data a časy. Obsahuje vlastnost s názvem nyní, vrátí aktuální datum a čas.

### <a name="using-code-to-render-markup-in-the-page"></a>Pomocí kódu k vykreslení kódu na stránce

V těle stránky Všimněte si následujících akcí:

[!code-html[Main](intro-to-web-pages-programming/samples/sample3.html)]

Znovu znak @ informuje technologie ASP.NET tento co následuje je kód, není ve formátu HTML. Kód můžete přidat následuje výraz kódu a ASP.NET v tomto bodě vykreslí hodnotu tohoto práva výraz. V příkladu @a vykreslí ať je hodnota proměnné s názvem a, @product vykreslí vše, co je v proměnné s názvem produktu a tak dále.

Nejste omezeno na proměnné, ale. Zde několik instancí znak @ předchází výrazu:

- @(\*b) vykreslí produktu ať jsou v proměnné a a b. ( \* Operátor znamená násobení.)
- @(technologie + "" + produktu) vykreslí po zřetězení je a přidání mezery v rozmezí hodnot v proměnných technologie a produktu. Operátor (+) pro zřetězení řetězců je stejný jako operátor pro přidání čísla. ASP.NET lze obvykle zjistit, zda pracujete s čísla nebo řetězce a správné funkci s + – operátor.
- @Request.Url vykreslí vlastnost adresa Url objektu požadavku. Objekt žádosti obsahuje informace o aktuálním požadavku z prohlížeče a samozřejmě vlastnost adresa Url obsahuje adresu URL aktuální žádosti.

V příkladu je navržen tak, aby ukazují, že můžete pracovat různými způsoby. Můžete provádět výpočty v bloku kódu v horní části, put výsledků do proměnné a následnému vykreslení proměnné v kódu. Nebo můžete provést výpočtů v práva výraz ve značkách. Přístupů, které použijete, závisí na jaké úlohy a do určité míry, na vlastní předvolby.

### <a name="seeing-the-code-in-action"></a>Zobrazení kódu v akci

Klikněte pravým tlačítkem na název souboru a potom zvolte **spustit v prohlížeči**. Zobrazí stránku v prohlížeči se všechny hodnoty a výrazy přeložit na stránce.

![Spouštění v prohlížeči stránky 'TestRazor.](intro-to-web-pages-programming/_static/image2.png)

Podívejte se na zdroj v prohlížeči.

!["Test Razor" zdroje stránku v prohlížeči](intro-to-web-pages-programming/_static/image3.png)

Podle očekávání z prostředí v předchozí kurzu žádná kódu Razor není na stránce. Zobrazí se pouze skutečné zobrazovaných hodnot. Při spuštění stránky ve skutečnosti děláte požadavek na webový server, který je součástí služby WebMatrix. Po přijetí žádosti ASP.NET vyřeší všechny hodnoty a výrazy a vykreslí jejich hodnoty na stránku. Pak odešle stránku v prohlížeči.

> [!TIP] 
> 
> **Syntaxe Razor a C#**
> 
> Až nyní jsme uvedli jste, že pracujete se syntaxí Razor. Je to pravda, ale není kompletní scénáře. Skutečné programovací jazyk, který používáte, se nazývá *C#*. C# byl vytvořen společností Microsoft prostřednictvím deset před a se stala jedna primární programovacích jazyků pro vytváření aplikací pro Windows. Všechna pravidla, které jste se seznámili s o název proměnné a postup vytvoření příkazy a tak dále jsou ve skutečnosti všechna pravidla jazyka C#.
> 
> Syntaxe Razor odkazuje přesněji řečeno na malou sadu konvence pro jak vložení tohoto kódu do stránky. Například konvence používání označit kódu na stránce a @ {} pro vložení bloku kódu je aspekt Razor stránky. Pomocné rutiny jsou také považovány za součást syntaxe Razor. Na více místech než jenom v aplikaci ASP.NET Web Pages se používá syntaxi Razor. (Například, použije se v zobrazení ASP.NET MVC také.)
> 
> Jsme zmínili, to vzhledem k tomu, že pokud hledáte informace o programovací rozhraní ASP.NET Web Pages, můžete využít mnoho odkazy na Razor. Však velké množství tyto odkazy se nevztahují na jste to a může být matoucí. A ve skutečnosti mnoho vaše otázky programovací skutečně budou o práci s C# nebo práci s technologií ASP.NET. Proto se podíváte speciálně pro informace o syntaxi Razor, nemusí najít odpovědi, které potřebujete.


## <a name="adding-some-conditional-logic"></a>Přidání některé podmíněnou logiku

Jedním z skvělé funkce o použití kódu na stránce je, že můžete změnit, co se stane závislosti na různých podmínkách. V této části kurzu budete přehrát s některé způsoby, jak změnit, co se zobrazí na stránce.

V příkladu bude jednoduchý a poněkud contrived tak, aby nám umožní soustředit se na podmíněnou logiku. Stránka, kterou vytvoříte, bude postupujte takto:

- Zobrazte různé text na stránce, v závislosti na tom, zda je první stránka se zobrazí, nebo zda jste klikli na tlačítko pro odeslání stránky. Bude první podmíněného test.
- Zobrazí se zpráva, pouze v případě, že určitou hodnotu, je předaná řetězec dotazu adresy URL (http://...?show=true). Který bude druhý podmíněného test.

Ve službě WebMatrix, vytvořte stránku a pojmenujte ji *TestRazorPart2.cshtml*. (Na pásu karet klikněte na tlačítko **nový**, zvolte **CSHTML**, název souboru a pak klikněte na tlačítko **OK**.)

Obsah této stránky nahraďte následujícím textem:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample4.cshtml)]

Blok kódu v horní části inicializuje proměnné s názvem zpráva s textem. V těle stránky, obsah proměnné zpráva se zobrazí uvnitř &lt;p&gt; elementu. Kód obsahuje také &lt;vstupní&gt; elementu, který chcete vytvořit **odeslání** tlačítko.

Spusťte stránku, abyste viděli, jak to funguje teď. Teď, je v podstatě statické stránky, i v případě, že kliknete **odeslání** tlačítko.

Vraťte se zpátky do prostředí WebMatrix. Uvnitř bloku kódu, přidejte následující zvýrazněný kód *po* řádek, který inicializuje zpráva:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample5.cshtml?highlight=4-6)]

### <a name="the-if---block"></a>Zda blok {}

Co jste právě přidali byl Pokud podmínka. V kódu pokud podmínka má struktura takto:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample6.cs)]

Podmínky pro testování je v závorkách. Musí být hodnota nebo výraz, který vrátí hodnotu true nebo false. Pokud je podmínka pravdivá, ASP.NET spustí příkaz nebo příkazy, které jsou uvnitř složené závorky. (Těch, které jsou *pak* součástí *Pokud potom* logiku.) Pokud je podmínka vyhodnocena jako false, je přeskočen blok kódu.

Tady je několik příkladů podmínky můžete otestovat v Pokud příkaz:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample7.cs)]

Proměnné před hodnoty nebo výrazy můžete otestovat pomocí <em>logický operátor</em> nebo <em>relační operátor</em>: rovná se (==) větší než (&gt;), menší než (&lt;), větší než nebo rovno (&gt;=) a menší než nebo rovno (&lt;=). ! = Znamená operátor není rovno – například pokud (! = 0) znamená <em>Pokud</em> <em>a</em><em>není rovno 0</em>.

> [!NOTE]
> Ujistěte se, že jste si všimli, že relační operátor pro hodnotu (==) není stejný jako =. = Operátor slouží pouze k přiřadit hodnoty (var = 2). Pokud kombinujete tyto operátory, budete buď dojde k chybě nebo některé neobvyklé výsledků.


K ověření, zda je hodnota true, úplná syntaxe je if(IsDone == true). Ale můžete použít také if(IsDone) zástupce. Pokud není žádná relační operátor, ASP.NET předpokládá, že testujete pro hodnotu true.

Na! operátor samostatně znamená logický operátor NOT. Například podmínka v případě (! IsPost) znamená *Pokud IsPost není true*.

Podmínky můžete kombinovat pomocí logické a (&amp; &amp; operátor) nebo logické OR (|| – operátor). Například pokud poslední podmínky v předchozí příklady znamená *Pokud FileProcessingIsDone není nastaven na hodnotu true a displayMessage nastaven na hodnotu false*.

### <a name="the-else-block"></a>Else bloku

Jednou z poslední věcí o tom, zda bloky: Pokud bloku může následovat else bloku. Blok else je užitečné je budete muset provést jiný kód, pokud je podmínka vyhodnocena jako false. Zde je jednoduchý příklad:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample8.cs)]

Některé příklady uvidíte v dalších kurzech této série, kde je užitečné pomocí jiného bloku.

### <a name="testing-whether-the-request-is-a-submit-post"></a>Zjištění, zda se jedná o žádost odešlete (post)

Existuje více, ale umožňuje získat zpátky v příkladu, který má podmínku if(IsPost) {...}. IsPost je ve skutečnosti vlastnost aktuální stránky. Při prvním požadavku na stránku IsPost vrací hodnotu false. Ale pokud klikněte na tlačítko nebo jinak stránku odeslat – to znamená, post – IsPost vrací hodnotu true. Proto IsPost můžete určit, zda, že pracujete s odeslání formuláře. (Z hlediska příkazy HTTP, pokud je požadavek operaci GET IsPost vrací hodnotu false. Pokud je požadavek operaci POST, IsPost vrací hodnotu true.) V novější kurzu budete pracovat s vstupní formuláře, kde bude tento test zvlášť užitečná.

Spuštění stránky. Vzhledem k tomu, že je to první, kdy se požadované stránce, se zobrazí "Toto je prvním jste si vyžádali stránce". Tento řetězec je hodnota, která inicializovat proměnnou zprávy. Je if(IsPost) test, ale která vrátí hodnotu false v tuto chvíli tak kód uvnitř zda blokovat nefunguje.

Klikněte **odeslání** tlačítko. Požadavek na stránku je znovu. Jako dříve, proměnné zpráva nastavena na "Toto je prvním...". Ale tentokrát test if(IsPost) vrátí hodnotu true, takže kód uvnitř zda blokovat spuštění. Kód změní hodnotu proměnné zpráva na jinou hodnotu, kterou bude vykreslen do kódu.

Pokud teď přidat podmínku do kódu. Níže &lt;p&gt; elementu, který obsahuje **odeslání** tlačítko, přidejte následující kód:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample9.cshtml)]

Přidáváte kód uvnitř značky, takže je musíte spustit s @. Pak je Pokud test podobné tomu jste přidali dříve v bloku kódu. Ve složených závorkách, ale přidáváte obyčejnou HTML – alespoň, je běžné dokud získá k @DateTime.Now. Je to jiné trocha kódu Razor, proto znovu budete muset přidat úrovních před ním.

Bod tady je, že můžete přidat, pokud podmínky v obou kód blokovat v horní části a ve značkách. Pokud použijete, pokud kód může být podmínka v těle stránky, řádky uvnitř bloku. V takovém případě a jako hodnotu true, kdykoliv kombinovat značek a kódu, budete muset použít, aby bylo jasné s technologií ASP.NET kde je kód.

Spuštění stránky a klikněte na tlačítko **odeslání**. Tentokrát nejen zobrazí jiná zpráva při odeslání ("teď jste odeslané..."), ale zobrazit novou zprávu, která zobrazí datum a čas.

!["Test Razor 2" stránku v prohlížeči s časovým razítkem zobrazující po odeslání](intro-to-web-pages-programming/_static/image4.png)

### <a name="testing-the-value-of-a-query-string"></a>Testování hodnota řetězce dotazu

Jeden další test. Tentokrát přidáte pokud blok, který porovnává hodnotu s názvem show, která mohla být předána v řetězci dotazu. (Jako jsou to: `http://localhost:43097/TestRazorPart2.cshtml?show=true`) změníte stránky tak, aby zpráva vám jste se zobrazení ("Toto je prvním...", atd.) se zobrazí jenom v případě zobrazit hodnotu true.

V dolní (ale uvnitř) přidejte následující blok kódu v horní části stránky:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample10.cs)]

Kompletní kód bloku teď vypadá jako v následujícím příkladu. (Nezapomeňte, že zkopírujete kód na stránku, odsazení může vypadat jinak. Ale který nemá žádný vliv na to, jak se spustí kód nástroje.)

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample11.cshtml)]

Nový kód v bloku inicializuje proměnné s názvem showMessage na hodnotu false. Pokud tomu pak testy a vyhledejte hodnotu v řetězci dotazu. Při prvním požadavku na stránku, má adresu URL podobné následujícímu:

`http://localhost:43097/TestRazorPart2.cshtml`

Kód určuje, zda adresa URL obsahuje proměnné s názvem show v řetězci dotazu, jako je tato verze adresy URL:

`http://localhost:43097/TestRazorPart2.cshtml`? Zobrazit = true

Sama zjistí QueryString vlastnost objektu žádosti. Pokud řetězec dotazu obsahuje položku nazvanou zobrazit, a pokud je daná položka nastavená na hodnotu true a zda bloku běží a nastavuje proměnnou showMessage na hodnotu true.

Jak můžete vidět je tady, možností. Jako název informacemi o tom, řetězec dotazu je řetězec. Ale můžete jenom otestovat pro hodnotu true a false Pokud je hodnota, které testujete logickou hodnotu (true nebo false). Hodnota proměnné zobrazit mohli otestovat v řetězci dotazu, musíte ho převést na logickou hodnotu. To je, jaké metody AsBool – přebírá řetězec jako vstup a převede jej na logickou hodnotu. Je zřejmé zda je řetězec "PRAVDA", AsBool metoda převede tuto hodnotu na hodnotu true. Pokud je hodnota řetězce cokoliv jiného, AsBool vrací hodnotu false.

> [!TIP] 
> 
> **Datové typy a metody As()**
> 
> Jsme uvedli jste, pouze pokud při vytváření proměnné používané var. – klíčové slovo To není celý článek, ale. Chcete-li upravit hodnoty – k přidání čísla, nebo zřetězení řetězců, a porovnání kalendářních dat a testovat true nebo false – C# má pro práci s příslušnou interního vyjádření hodnoty. C# můžete *obvykle* zjistěte, co by měl být této reprezentace (to znamená, co *typ* data) podle jaké úlohy s hodnotami. Teď a potom ale ji nejde udělat. Pokud ne, je nutné zvýšit tak, že explicitně zadáte jak C# by měl představovat data. Metoda AsBool nemá, který – obsahuje informace pro C#, řetězcovou hodnotu "true" nebo "Nepravda" zacházeno jako logická hodnota. Podobné metody existovat k reprezentaci řetězce jako jiné typy taky jako AsInt (zpracovávat jako celé číslo), AsDateTime (zpracovávat jako datum a čas), AsFloat (zpracovávat jako číslo s plovoucí desetinnou čárkou) a tak dále. Pokud používáte jako metody (), pokud C# nelze představují hodnotu řetězce podle požadavku, uvidíte chybu.


V kódu stránky, odebrat nebo komentář tohoto elementu (v tomto poli zobrazuje komentáři out):

[!code-html[Main](intro-to-web-pages-programming/samples/sample12.html)]

Práva, kde můžete odebrat nebo označeno jako komentář text, přidejte následující:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample13.cshtml)]

Pokud je test zobrazeno, pokud je nastavena hodnota true, proměnnou showMessage vykreslení &lt;p&gt; element s hodnotou proměnné zpráva.

### <a name="summary-of-your-conditional-logic"></a>Souhrn podmíněnou logiku

V případě, že nejste zcela jisti co jste právě provedli, zde je souhrn.

- Proměnné zpráva je inicializováno výchozí řetězec ("Toto je prvním...").
- Pokud požadavek na stránku výsledek odeslání (post), je hodnota zprávy změněna na "teď jste odeslána..."
- Proměnná showMessage je inicializován na hodnotu false.
- Pokud obsahuje řetězec dotazu? zobrazit = true, showMessage proměnná je nastavená na hodnotu true.
- Do kódu, pokud je nastavena hodnota true, showMessage &lt;p&gt; prvek je vykreslovaný, která zobrazuje hodnotu zprávy. (Pokud showMessage hodnotu false, nic se vykreslí v tomto bodě ve značkách.)
- Do kódu, pokud je požadavek post &lt;p&gt; prvek je vykreslovaný, která zobrazuje datum a čas.

Spuštění stránky. Neexistuje žádná zpráva, protože showMessage je nastavena hodnota false, takže ve značkách if(showMessage) test vrátí hodnotu false.

Klikněte na tlačítko **odeslání**. Zobrazí datum a čas, ale stále žádná zpráva.

V prohlížeči přejděte do pole Adresa URL a přidejte následující na konec adresy URL:? Zobrazit = true a stiskněte klávesu Enter.

!["Test Razor 2" stránku v prohlížeči a zobrazí řetězec dotazu](intro-to-web-pages-programming/_static/image5.png)

Stránka se zobrazí znovu. (Protože jste změnili adresu URL, to je nový požadavek, není odeslat.) Klikněte na tlačítko **odeslání** znovu. Zpráva zobrazí znovu, jako je datum a čas.

!["Test Razor 2" stránka po odeslání, pokud je řetězec dotazu](intro-to-web-pages-programming/_static/image6.png)

V adrese URL, změňte? zobrazit = true na? Zobrazit = false a stiskněte klávesu Enter. Stránku odešlete znovu. Stránka je zpět na tom, jak jste spustili – žádná zpráva.

Jak již bylo uvedeno dříve, logiku tohoto příkladu je malým contrived. Ale pokud budete se objeví v mnoha stránek, a bude trvat jednu nebo více formulářů, které jste se seznámili s sem.

## <a name="installing-a-helper-displaying-a-gravatar-image"></a>Instalace pomocné rutiny (zobrazení bitovou kopii Gravatar)

Některé úlohy, které uživatelé často chtějí provést na webových stránkách vyžadovat velké množství kódu nebo vyžadovat další znalostní báze. Příklady: zobrazení grafu pro data; ukládání Facebook "Jako" na stránku; odesílání e-mailu z webu; oříznutí nebo změna velikosti obrázků; pomocí služby PayPal pro váš web. Chcete-li snadné provést tyto druhy věcí, rozhraní ASP.NET Web Pages vám umožní používat *Pomocníci*. Pomocné rutiny jsou komponenty, která po instalaci lokality a které umožňují typické úlohy provádět pomocí několika řádků kódu Razor.

ASP.NET – webové stránky má několik Pomocníci součástí. Mnoho pomocné rutiny jsou však k dispozici v balíčcích (doplňky), které jsou k dispozici pomocí Správce balíčků NuGet. NuGet umožňuje vybrat balíček pro instalaci a pak se stará o všechny podrobnosti instalace.

V této části kurzu nainstalujete pomocné rutiny, která umožňuje zobrazit obrázek Gravatar (dále jen "globálně rozpoznaný miniatury"). Dozvíte dvě věci. Jeden je jak najít a nainstalovat pomocné rutiny. Budete také zjistěte, jak pomocné rutiny usnadňuje dělejte, které byste jinak potřebovali Uděláte to pomocí velké množství kódu, budete muset napsat sami.

Můžete zaregistrovat vlastní Gravatar na webu Gravatar na [ http://www.gravatar.com/ ](http://www.gravatar.com/), ale to není nezbytně nutné, aby vytvořit účet Gravatar k provedení této části kurzu.

Ve službě WebMatrix, klikněte **NuGet** tlačítko.

![Dialogové okno Galerie NuGet ve službě WebMatrix](intro-to-web-pages-programming/_static/image7.png)

Tím se spustí Správce balíčků NuGet a zobrazí se dostupné balíčky. (Ne všechny balíčky jsou pomocné; některé přidání funkcí do prostředí WebMatrix samotné některé další šablony atd.) Může se zobrazit chybová zpráva o nekompatibility verzí. Tuto chybovou zprávu můžete ignorovat kliknutím **OK** a budete pokračovat v tomto kurzu.

![Dialogové okno Galerie NuGet ve službě WebMatrix](intro-to-web-pages-programming/_static/image8.png)

Do vyhledávacího pole zadejte "Pomocníci asp.net". NuGet ukazuje balíčky, které odpovídají hledaným výrazům.

![Galerie NuGet ve službě WebMatrix zobrazující balíčky](intro-to-web-pages-programming/_static/image9.png)

Knihovnu ASP.NET Web Helpers obsahuje kód pro zjednodušení mnoho běžných úkolů, včetně použití Gravatar bitových kopií. Vyberte **knihovnu ASP.NET Web Helpers** balíček a potom klikněte na **nainstalovat** spustíte instalační program. Vyberte **Ano** když se zobrazí dotaz, pokud chcete nainstalovat balíček a přijměte podmínky pro dokončení instalace.

Je to. NuGet se stáhne a nainstaluje vše, včetně žádné další součásti, které mohou být požadovány (*závislosti*).

Pokud z nějakého důvodu musíte odinstalovat pomocné rutiny, je velmi podobný procesu. Klikněte **NuGet** tlačítko, klikněte na tlačítko **nainstalovaná** kartě a vyberte balíček, který chcete odinstalovat.

## <a name="using-a-helper-in-a-page"></a>Na stránce pomocí pomocné rutiny

Teď použijete pomocné rutiny, který jste právě nainstalovali. Proces přidávání pomocné rutiny pro stránky je podobný pro většinu pomocné rutiny.

Ve službě WebMatrix, vytvořte stránku a pojmenujte ji *GravatarTest.cshml*. (Vytváření zvláštní stránky k testování pomocné rutiny, ale pomocné rutiny můžete použít na libovolné stránce ve vaší lokalitě.)

Uvnitř &lt;textu&gt; elementu, přidejte &lt;div&gt; elementu. Uvnitř &lt;div&gt; elementu, zadejte toto:

@Gravatar.

Znak @ je ve stejném jste dosud používali označit kódu Razor. **Gravatar** je pomocný objekt, který kterými pracujete.

Jakmile zadáte tečku (.), služba WebMatrix zobrazí seznam *metody* (funkce) zpřístupní Gravatar pomocné rutiny:

![Gravatar pomocná IntelliSense rozevírací seznam](intro-to-web-pages-programming/_static/image10.png)

Tato funkce se označuje jako *IntelliSense*. Kód vám pomáhá tím, že poskytuje kontext příslušné volby. IntelliSense pracuje s HTML, CSS, kódu ASP.NET, JavaScript a další jazyky, které jsou podporovány ve službě WebMatrix. Je jiné funkce, která usnadňuje vývoj webových stránek ve službě WebMatrix.

Stisknutím klávesy G na klávesnici a že IntelliSense vyhledá metodu GetHtml najdete v článku. Stisknutím klávesy Tab. IntelliSense vloží vybranou metodu (GetHtml) za vás. Otevřete závorku a Všimněte si, že je automaticky přidán kulaté závorky. Zadejte e-mailovou adresu v uvozovkách mezi dvěma závorky. Pokud máte účet Gravatar, bude vrácen profilový obrázek. Pokud nemáte účet Gravatar, je vrácen výchozí image. Když jste hotovi, řádek vypadá takto:

[!code-css[Main](intro-to-web-pages-programming/samples/sample14.css)]

Nyní zobrazte stránku v prohlížeči. Obrázek nebo výchozí image se zobrazí, v závislosti na tom, jestli máte účet Gravatar.

![Gravatar](intro-to-web-pages-programming/_static/image11.png) ![Výchozí image](intro-to-web-pages-programming/_static/image12.png)

Získat představu o co je to pomocné rutiny pro vás, zobrazte si zdroj stránku v prohlížeči. Společně s HTML, které jste měli v stránku zobrazí element bitovou kopii, která obsahuje identifikátor. Toto je kód, který pomocné rutiny vykreslovány na stránce na místě, kde jste měli @Gravatar.GetHtml. Pomocné rutiny trvalo informace, které poskytuje a vygeneruje kód, který komunikuje přímo se Gravatar, aby bylo možné získat zpět správné bitové kopie pro zadaný účet.

Metoda GetHtml také umožňuje přizpůsobit image tím, že poskytuje další parametry. Následující kód ukazuje, jak požádat o image má šířky a výšky 40 pixelů a používá zadané výchozí obrázek s názvem **wavatar** Pokud zadaný účet neexistuje.

[!code-javascript[Main](intro-to-web-pages-programming/samples/sample15.js)]

Tento kód vytvoří přibližně následující výsledek (výchozí image se náhodně liší).

![](intro-to-web-pages-programming/_static/image13.png)

## <a name="coming-up-next"></a>Objevuje další

Aby tento kurz krátké jsme museli soustředit na pouze několik základy. Samozřejmě je *mnoho* další Razor a C#. Dozvíte se další jako můžete projít tyto kurzy. Pokud byste chtěli dozvědět další informace o programování aspektů Razor a C# nyní, si můžete přečíst podrobnější Úvod zde: [Úvod do ASP.NET webové programování pomocí syntaxe Razor](https://go.microsoft.com/fwlink/?LinkID=202890).

V dalším kurzu se seznámíte s práci s databází. V tomto kurzu začnete budete vytvářet ukázkovou aplikaci, která umožňuje zobrazit seznam oblíbených filmů.

## <a name="complete-listing-for-testrazor-page"></a>Úplný seznam TestRazor stránky

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample16.cshtml)]

## <a name="complete-listing-for-testrazorpart2-page"></a>Úplný seznam TestRazorPart2 stránky

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample17.cshtml)]

## <a name="complete-listing-for-gravatartest-page"></a>Úplný seznam GravatarTest stránky

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample18.cshtml)]

## <a name="additional-resources"></a>Další prostředky

- [Úvod do rozhraní ASP.NET Web programování pomocí syntaxe Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Pomocník Twitter](../../ui-layouts-and-themes/twitter-helper.md)

> [!div class="step-by-step"]
> [Předchozí](getting-started.md)
> [další](displaying-data.md)
