---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
title: Principy možnosti ladění technologie ASP.NET AJAX | Dokumentace Microsoftu
author: scottcate
description: Umožňuje ladit kód je konkrétní dovednosti, které každý vývojář by měl mít v jejich arsenál bez ohledu na technologii, kterou používá. Přestože celá řada vývojářů...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 7f9380c6-19f7-4c82-a019-916ec6dffc9c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
msc.type: authoredcontent
ms.openlocfilehash: 533eb8d2faf735915fa5cf5044db09d0ab636938
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390969"
---
<a name="understanding-aspnet-ajax-debugging-capabilities"></a>Principy možnosti ladění technologie ASP.NET AJAX
====================
podle [– Scott Cate](https://github.com/scottcate)

[Stáhnout PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial06_Debugging_MS_Ajax_Applications_cs.pdf)

> Umožňuje ladit kód je konkrétní dovednosti, které každý vývojář by měl mít v jejich arsenál bez ohledu na technologii, kterou používá. Celá řada vývojářů jsou zvyklí ladění aplikací ASP.NET, které používají Kód VB.NET nebo C# pomocí sady Visual Studio .NET nebo Web Developer Express, některé nejste vědomi, že je také velmi užitečné pro ladění kódu na straně klienta, třeba JavaScript. Stejný typ techniky použít k ladění aplikací .NET můžete použít také k aplikacím s povoleným AJAX a přesněji řečeno aplikací technologie ASP.NET AJAX.


## <a name="debugging-aspnet-ajax-applications"></a>Ladění aplikací ASP.NET AJAX

DaN Wahlin

Umožňuje ladit kód je konkrétní dovednosti, které každý vývojář by měl mít v jejich arsenál bez ohledu na technologii, kterou používá. Je samozřejmé, že principy, které jsou k dispozici různé možnosti ladění můžete uložit obrovské množství času na projekt a možná dokonce i pár obrovskému. Celá řada vývojářů jsou zvyklí ladění aplikací ASP.NET, které používají Kód VB.NET nebo C# pomocí sady Visual Studio .NET nebo Web Developer Express, některé nejste vědomi, že je také velmi užitečné pro ladění kódu na straně klienta, třeba JavaScript. Stejný typ techniky použít k ladění aplikací .NET můžete použít také k aplikacím s povoleným AJAX a přesněji řečeno aplikací technologie ASP.NET AJAX.

V tomto článku uvidíte, jak Visual Studio 2008 a několik dalších nástrojů slouží k ladění aplikací ASP.NET AJAX k rychlému vyhledání chyb a další problémy. Toto pojednání bude zahrnovat informace o povolení aplikace Internet Explorer 6 nebo vyšší pro ladění, pomocí sady Visual Studio 2008 a Průzkumník skriptů, které krokovat kód také pomocí jiných nástrojů zdarma, jako je vývoj pro webové pomocné rutiny. Budete se také dozvíte, jak ladit aplikace technologie ASP.NET AJAX v aplikaci Firefox pomocí rozšíření s názvem Firebug který umožní procházet kód JavaScriptu přímo v prohlížeči bez dalších nástrojů. Nakonec se seznámíte s tříd v knihovně technologie ASP.NET AJAX, který vám pomůže s různými ladění úkoly, jako je například příkazy kódu kontrolní výraz a trasování.

Před pokusem o ladění stránek v prohlížeči Internet Explorer nejsou pomocí několika jednoduchých kroků, které budete muset provést, aby je pro ladění. Pojďme se podívat na některé základní nastavení požadavky, které je potřeba provést, abyste mohli začít.

## <a name="configuring-internet-explorer-for-debugging"></a>Konfigurace aplikace Internet Explorer pro ladění

Většina lidí nejsou nepotřebujete vidět problémy JavaScriptu došlo k na webovou stránku zobrazit pomocí aplikace Internet Explorer. Ve skutečnosti se průměrný uživatel nemusí ani vědět, co dělat, když se zobrazila chybová zpráva. Možnosti ladění se v důsledku toho vypnuto ve výchozím nastavení v prohlížeči. Je však velmi jednoduché zapnete ladění a začít využívat při vývoji nových aplikací AJAX.

Pokud chcete povolit funkce ladění, přejděte na možnosti Internetu nástrojů v nabídce aplikace Internet Explorer a vyberte kartu Upřesnit. V rámci oddílu procházení zajistěte, aby Nekontrolovaná následující položky:

- Zakázat ladění skriptů (aplikace Internet Explorer)
- Zakázat ladění skriptů (ostatní)

I když není požadováno, pokud se snažíte ladit aplikaci pravděpodobně budete chtít použít všechny chyby jazyka JavaScript na stránce budou okamžitě viditelné a zřejmý. Můžete vynutit všechny chyby zobrazený v okně se zprávou, zaškrtnutím políčka "Zobrazovat oznámení o každé chybě skriptu". Když je skvělou možností, jak zapnout, když vyvíjíte aplikaci, to může být nepříjemné, pokud jste právě perusing ostatních webů vzhledem k tomu, že pravděpodobnost vzniku chyby jazyka JavaScript jsou tom docela dobře.

Obrázek 1 ukazuje, jaké aplikace Internet Explorer Upřesnit dialogového okna by měl vypadat po byl správně nakonfigurován pro ladění.


[![Konfigurace aplikace Internet Explorer pro ladění.](understanding-asp-net-ajax-debugging-capabilities/_static/image2.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image1.png)

**Obrázek 1**: Konfigurace aplikace Internet Explorer pro ladění.  ([Kliknutím ji zobrazíte obrázek v plné velikosti](understanding-asp-net-ajax-debugging-capabilities/_static/image3.png))


Jakmile ladění je zapnutý, zobrazí se vám nové položky nabídky se zobrazí v nabídce zobrazení s názvem Script Debugger. Má dvě možnosti, které jsou k dispozici včetně Open a zalomení na další příkaz. Pokud je vybrána Open budete vyzváni k ladění na stránce v sadě Visual Studio 2008 (Všimněte si, že Visual Web Developer Express můžete také použít pro ladění). Pokud je nainstalováno Visual Studio .NET můžete použít tuto instanci nebo vytvořte novou instanci. Pokud je vybrána zalomení na další příkaz budete vyzváni k ladění stránku při spuštění kódu jazyka JavaScript. Pokud kód jazyka JavaScript provádí v události při načtení stránky můžete aktualizovat na stránce Aktivovat ladicí relaci. Pokud kód jazyka JavaScript se spustí po kliknutí na tlačítko ladicí program se spustí ihned po kliknutí na tlačítko.

> *> [!NOTE] Pokud používáte v systému Windows Vista s přístup k řízení Uživatelských účtů povolená a máte Visual Studio 2008 nastavena pro spuštění jako správce, Visual Studio se nepodaří připojit k procesu, když se zobrazí výzva k připojení. Chcete-li tento problém obejít, nejprve spusťte sadu Visual Studio a použít tuto instanci k ladění.*


I když další část popisuje, jak ladit stránky technologie ASP.NET AJAX přímo z v rámci sady Visual Studio 2008, pomocí Internet Exploreru Script Debugger možnost je užitečná při stránka je již otevřen a chcete podrobněji seznámit.

## <a name="debugging-with-visual-studio-2008"></a>Ladění pomocí Visual Studio 2008

Visual Studio 2008 poskytuje funkce pro ladění, který vývojářům po celém světě využívají každý den k ladění aplikací .NET. Integrované ladicího programu můžete krokovat kód, zobrazení dat objektů, sledovat určité proměnné sledování zásobníku volání a spoustu dalších věcí. Kromě ladění VB.NET nebo C# kód, ladicí program je také užitečné pro ladění aplikací ASP.NET AJAX a vám umožní procházet kód JavaScriptu řádek po řádku. Podrobnosti, které následují fokus na techniky, které slouží k ladění skriptů na straně klienta místo poskytování discourse na celkový proces ladění aplikací pomocí sady Visual Studio 2008.

Proces ladění na stránce v sadě Visual Studio 2008 může být spuštěný v několika různými způsoby. Nejprve můžete použít Internet Explorer Script Debugger možností uvedených v předchozí části. Tento postup funguje dobře, když se už načtení stránky v prohlížeči a chcete spustit ladění. Alternativně můžete klikněte pravým tlačítkem na stránku .aspx v Průzkumníku řešení a z nabídky vyberte nastavit jako úvodní stránku. Pokud jste zvyklí ladění stránek ASP.NET pak pravděpodobně udělali to. Po stisknutí klávesy F5 můžete ladit stránky. Ale zatímco obecně můžete nastavit zarážku kamkoli chcete v VB.NET nebo C# kód, který není vždy případu s použitím jazyka JavaScript jak zjistíte dále.

*Vložený oproti externích skriptů*

Ladicí program sady Visual Studio 2008 zpracuje JavaScript embedded liší od externí soubory jazyka JavaScript na stránce. Pomocí externích skriptů soubory můžete otevřít soubor a nastavte zarážku na kterýkoli řádek, který zvolíte. Zarážky lze nastavit kliknutím v oblasti šedé na hlavním panelu na levé straně okna editoru kódu. Když je vložený JavaScript přímo na stránku pomocí `<script>` není značky, nastavením zarážky v oblasti šedé na hlavním panelu klikněte na možnost. Pokusí se nastavit zarážku na řádek vloženého skriptu bude výsledkem je výstraha s oznámením "Toto není platné umístění zarážky".

Tento problém můžete obejít přesunutím kód do souboru JS externí a odkazuje pomocí atributu src &lt;skript&gt; značky:


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample1.html)]

Co když přesunutí kódu do externího souboru není nebo vyžaduje více práce než stojí? Zatímco nelze nastavit zarážku pomocí editoru, můžete přidat příkaz ladicího programu přímo do kódu, ve kterém chcete spustit ladění. Můžete také třídu Sys.Debug k dispozici v knihovně technologie ASP.NET AJAX k vynucení, chcete-li spustit ladění. Zobrazí další informace o třídě Sys.Debug dále v tomto článku.

Příklad použití `debugger` – klíčové slovo je zobrazena ve výpisu 1. V tomto příkladu vynutí ladicí program na přerušení správné, než je provedeno volání funkce aktualizace.

**Výpis 1. Pomocí klíčového slova ladicího programu k vynucení přerušení ladicího programu sady Visual Studio .NET.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample2.js)]

Po spuštění příkazu ladicího programu se výzva k ladění na stránce pomocí sady Visual Studio .NET a můžete začít kód krokovat. Při provádění jste to vy, může dojít k potížím s přístupem k soubory skriptu knihovna ASP.NET AJAX na stránce můžeme použít podívejte se na pomocí sady Visual Studio. Průzkumník skriptů vaší sítě.

## <a name="using-visual-studio-net-windows-to-debug"></a>Chcete-li ladit pomocí sady Visual Studio .NET Windows

Po spuštění relace ladění a začnete procházení kódu pomocí klávesy F11 výchozí, můžete narazit je znázorněno v chybovém dialogovém okně naleznete v tématu na obrázku 2, pokud jsou všechny soubory skriptu na stránce použít otevřený a dostupný pro ladění.


[![Dialogové okno s chybou zobrazuje, když je pro ladění k dispozici žádný zdrojový kód.](understanding-asp-net-ajax-debugging-capabilities/_static/image5.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image4.png)

**Obrázek 2**: chybové dialogové okno zobrazuje, když je pro ladění k dispozici žádný zdrojový kód.  ([Kliknutím ji zobrazíte obrázek v plné velikosti](understanding-asp-net-ajax-debugging-capabilities/_static/image6.png))


Toto dialogové okno se zobrazí, protože Visual Studio .NET není se, jak získat ke zdrojovému kódu některých skriptů odkazuje na stránku. Když to může být poměrně frustrující zpočátku je jednoduchý oprava. Po spuštění relace ladění a na zarážku, přejděte do okna ladit skript Průzkumníka Windows v nabídce sady Visual Studio 2008 nebo pomocí klávesové zkratky Ctrl + Alt + N.

> *> [!NOTE] Není-li v nabídce Průzkumníka skriptu uvedené, přejděte na nástroje* *vlastní* *příkazů v nabídce sady Visual Studio .NET. Vyhledejte položku ladění v části kategorie a klikněte na něj zobrazíte všechny položky nabídky k dispozici. V seznamu příkazů, posuňte se dolů Průzkumník skriptů a přetáhněte ji na ladění* *v nabídce Windows již bylo zmíněno dříve. To zpřístupní položka nabídky Průzkumník skriptů pokaždé, když spustíte Visual Studio .NET.*


Průzkumník skriptu lze použít k zobrazení všech skripty používané na stránce a otevřít v editoru kódu. Jakmile se otevře Průzkumník skriptů, dvakrát klikněte na stránku .aspx, která se právě ladí a otevře se v okně editoru kódu. Proveďte stejnou akci pro všechny ostatní skripty uvedené v podokně skriptu. Jakmile se všechny skripty jsou otevřeny v okně kódu můžete stisknutím klávesy F11 (a použití jiných ladění klávesové zkratky) pro jednotlivé kroky v kódu. Obrázek 3 ukazuje příklad Průzkumníka skriptu. Vypíše aktuální soubor laděného (Demo.aspx) a také dva vlastní skripty a dva skripty dynamicky vloženy do stránky technologie ASP.NET AJAX ScriptManager.


[![Průzkumník skriptů zajistí jednoduchý přístup k skripty používané na stránce.](understanding-asp-net-ajax-debugging-capabilities/_static/image8.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image7.png)

**Obrázek 3**. Průzkumník skriptů zajistí jednoduchý přístup k skripty používané na stránce.  ([Kliknutím ji zobrazíte obrázek v plné velikosti](understanding-asp-net-ajax-debugging-capabilities/_static/image9.png))


Několik dalších windows také umožňuje poskytují užitečné informace, jak procházet kód na stránce. Například chcete-li zobrazit hodnoty v různých proměnných použitých ve stránce, příkazovém k vyhodnocení, jestli konkrétní proměnné nebo podmínky a zobrazte výstup můžete v okně místních hodnot. V okně výstupu můžete použít také k zobrazení příkazů trasování zapsat pomocí funkce Sys.Debug.trace (což se budeme dále v tomto článku) nebo aplikaci Internet Explorer Debug.writeln – funkce.

Jak můžete krokovat kód pomocí ladicího programu můžete myší proměnné v kódu k zobrazení hodnoty, které jsou přiřazeny. Ale ladicímu prostředku skriptů příležitostně nezobrazí nic jako myší dané proměnné jazyka JavaScript. Pokud chcete zobrazit hodnotu, zvýrazněte příkaz nebo proměnnou, kterou se snažíte zobrazit v okně editoru kódu a potom myší nad ním. Přestože tento postup nefunguje v každé situaci, v mnoha případech budete moci zobrazit hodnota nemusíte hledat v různých ladicí okno, jako je třeba v okně místních hodnot.

Výukové video demonstrace některé funkce popsané Tady můžete zobrazit [ http://www.xmlforasp.net ](http://www.xmlforasp.net).

## <a name="debugging-with-web-development-helper"></a>Ladění pomocí webového vývoje pomocné rutiny

I když jsou velmi podporuje nástroje pro ladění sady Visual Studio 2008 (a Visual Web Developer Express 2008), existují další možnosti, které můžete použít také, které jsou nižšími nároky. Jednou z nejnovější nástroje potřebné k vydané je pomocná vývoj pro Web. Nikhil Kothari společnosti Microsoft, (jeden z klíčů architekty technologie ASP.NET AJAX v Microsoftu) napsal tento vynikající nástroj, který můžete provádět mnoho různých úloh z jednoduchého ladění při zobrazení zprávy požadavku a odpovědi protokolu HTTP. Vývoj webové pomocné rutiny si můžete stáhnout tady [ http://projects.nikhilk.net/Projects/WebDevHelper.aspx ](http://projects.nikhilk.net/Projects/WebDevHelper.aspx).

Pomocné rutiny vývoj pro web je možné přímo v aplikaci Internet Explorer, takže je pohodlné používat ho. Spuštění tak, že vyberete pomocné rutiny vývoje webových nástrojů v nabídce aplikace Internet Explorer. Otevře se nástroj v dolní části prohlížeče, což je skvělé, protože není nutné opustit prohlížeč, aby provést několik úloh, jako je například protokolování zpráv požadavků a odpovědí HTTP. Obrázek 4 ukazuje, jak pomocné rutiny vývoj pro Web vypadá v praxi.


[![Vývoj webové pomocné rutiny](understanding-asp-net-ajax-debugging-capabilities/_static/image11.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image10.png)

**Obrázek 4**: vývoj webové pomocné rutiny ([kliknutím ji zobrazíte obrázek v plné velikosti](understanding-asp-net-ajax-debugging-capabilities/_static/image12.png))


Vývoj webové pomocné rutiny není nástroj budete používat ke krokování kódu řádek po řádku jako s Visual Studio 2008. Ale můžete použít k zobrazení výstupu trasování, snadno vyhodnotit proměnné ve skriptu nebo prozkoumat data jsou v objektu JSON. Je také velmi užitečné při prohlížení data, která je předána do a z stránka technologie ASP.NET AJAX a serveru.

Jakmile pomocné rutiny vývoj pro Web je otevřen v aplikaci Internet Explorer, musí být ladění skriptů povoleno tak, že vyberete skript povolení ladění skriptu z nabídky pomocné rutiny vývoj pro Web jak je znázorněno na obrázku 4 výše. Díky tomu nástroj pro zachycení chyb, ke kterým dochází při spuštění stránky. Umožňuje také snadný přístup k trasování zpráv, které jsou výstupem na stránce. Trasovací informace zobrazit nebo spustit příkazy skriptu pro testování různých funkcí v rámci stránky, vyberte skript zobrazit skript konzoly z nabídky pomocné rutiny vývoj pro Web. To poskytuje přístup k příkazové okno a jednoduché podokna.

*Zobrazení zpráv trasování a Data objektu JSON*

Příkazové podokno umožňuje spouštět příkazy skriptu nebo dokonce načtení nebo uložení skripty, které se používají k testování různých funkcí na stránce. V příkazovém okně zobrazí zprávy trasování a ladění zapsal stránky zobrazení. Výpis 2 ukazuje, jak zapsat zprávu trasování pomocí aplikace Internet Explorer Debug.writeln – funkce.

**Výpis 2. Zápis zprávy trasování na straně klienta pomocí třídu ladění.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample3.js)]

Pokud vlastnost LastName obsahuje hodnotu Doe, pomocné rutiny vývoj pro Web se zobrazí zpráva "jméno: Doe" v příkazovém okně konzole skriptu (za předpokladu, že je povoleno ladění). Vývoj webové pomocné rutiny také přidá objekt nejvyšší úrovně debugService na stránky, které je možné zapsat informace trasování nebo zobrazení obsahu objektů JSON. Výpis 3 ukazuje příklad použití funkce trasování debugService třídy.

**Výpis 3. Pomocí třídy pomocné rutiny webového vývoje debugService k zápisu zprávy trasování.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample4.js)]

Užitečnou funkci třídy debugService je, že bude fungovat i v případě, že ladění není povoleno v aplikaci Internet Explorer, což usnadňuje vždy přístup k datům trasování při spouštění pomocníka vývoj pro Web. Při ladění na stránce se nepoužívá nástroj, příkazů trasování bude ignorovat, protože volání window.debugService vrátí hodnotu false.

Třída debugService také umožňuje data JSON objektu lze zobrazit pomocí okna pomocné vývoj pro Web inspector. Výpis 4 vytvoří jednoduchý objekt JSON obsahující data osob. Po vytvoření objektu je volána k debugService zkontrolovat třídy funkce umožní vizuálně zkontroloval objekt JSON.

**Výpis 4. Chcete-li zobrazit data objektu JSON pomocí funkce debugService.inspect.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample5.js)]

Na stránce nebo prostřednictvím podokna volání funkce GetPerson() způsobí v dialogovém okně Inspektor objektů povolí, jak je znázorněno na obrázku 5. Vlastnosti v rámci objektu můžete změnit dynamicky zvýrazněním, změna hodnoty uvedené v textovém poli hodnotu a potom kliknutím na odkaz aktualizace. Použití inspektoru objektu je přehledné zobrazení dat objektu JSON a experimentovat s použitím různých hodnot vlastností.

*Chyby ladění*

Kromě povolení trasování data a objekty JSON, který se má zobrazit, můžete také Web Development pomocné pomoci při ladění chyby na stránce. Pokud dojde k chybě, zobrazí výzva k pokračování na další řádek kódu nebo ladění skriptu (viz obrázek 6). Chyba skriptu dialogového okna okno zobrazí se kompletní volání zásobníku a čísla řádků mohli snadno identifikovat, kde jsou problémy ve skriptu pro.


[![V okně Inspektor objektů pomocí zobrazení objektu JSON.](understanding-asp-net-ajax-debugging-capabilities/_static/image14.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image13.png)

**Obrázek 5**: použití okně Inspektor objektů k zobrazení objekt JSON.  ([Kliknutím ji zobrazíte obrázek v plné velikosti](understanding-asp-net-ajax-debugging-capabilities/_static/image15.png))


Výběr možnosti ladění umožňuje spouštění příkazů skriptu přímo v okně okamžité pomocné rutiny webového vývoje k zobrazení hodnot proměnných, vypsat objekty JSON a navíc více. Pokud je znovu provést stejnou akci, která způsobila chybu a Visual Studio 2008 je k dispozici na počítači, se výzva ke spuštění relace ladění můžete krokovat kód řádek po řádku, jak je popsáno v předchozí části.


[![Webového vývoje pomocné dialogové okno chyby skriptu](understanding-asp-net-ajax-debugging-capabilities/_static/image17.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image16.png)

**Obrázek 6**: dialogové okno chyby skriptu pomocné rutiny webového vývoje ([kliknutím ji zobrazíte obrázek v plné velikosti](understanding-asp-net-ajax-debugging-capabilities/_static/image18.png))


*Kontrola požadavku a odpovědi na zprávy*

Při ladění stránek ASP.NET AJAX je často užitečné prohlédnout si žádostí a odpovědí zprávy odeslané mezi stránkou a serveru. Zobrazení obsahu v rámci zpráv umožňuje zobrazit, pokud je správná data předávána a také velikost zpráv. Vývoj webové pomocné rutiny poskytuje vynikající funkce protokolu HTTP zpráv protokolovacího nástroje, se kterou snadno zobrazit data jako nezpracovaný text nebo v přehlednějším tvaru.

Zobrazení zpráv požadavků a odpovědí technologie ASP.NET AJAX, musí být tak, že vyberete HTTP povolit protokolování HTTP z nabídky pomocné rutiny vývoj pro Web povoleno protokolování HTTP. Po povolení všech zpráv odeslaných z aktuální stránky lze zobrazit v prohlížeči protokolu HTTP, který se dá dostat tak, že vyberete HTTP zobrazit protokoly HTTP.

I když zobrazení nezpracovaný text odeslaný v každé zprávě požadavku nebo odpovědi je určitě užitečné (a možnost v pomocné vývoj pro Web), je často snazší zobrazit data zprávy ve formátu více grafických. Jakmile bylo povoleno protokolování HTTP a zprávy byly zaprotokolovány, data zprávy lze zobrazit dvojitým kliknutím na tuto zprávu najdete v protokolu HTTP log vieweru. To vám umožní zobrazit všechny hlavičky přidružené k zprávu, stejně jako skutečný zpráva obsahu. Obrázek 7 znázorňuje příklad zprávy s požadavkem a zprávy s odpovědí zobrazit v okně nástroje HTTP Log Viewer.


[![Chcete-li zobrazit data zprávy požadavku a odpovědi pomocí Prohlížeč protokolu HTTP.](understanding-asp-net-ajax-debugging-capabilities/_static/image20.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image19.png)

**Obrázek 7**: použití Prohlížeč protokolu HTTP k zobrazení dat zprávy požadavků a odpovědí.  ([Kliknutím ji zobrazíte obrázek v plné velikosti](understanding-asp-net-ajax-debugging-capabilities/_static/image21.png))


Prohlížeč protokolu HTTP automaticky analyzuje objekty JSON a zobrazí ho pomocí stromového zobrazení tak rychle a snadno zobrazit data vlastnosti objektu. Při použití ovládacího prvku UpdatePanel na stránce technologie ASP.NET AJAX, prohlížeč každá část zprávy na jednotlivé části dělí, jak je znázorněno na obrázku 8. To je skvělé funkce, která usnadňuje mnohem viděl a pochopil, co je ve zprávě porovnání s zobrazení nezpracovaná data zpráv.


[![Prvek UpdatePanel zprávu odpovědi zobrazit pomocí prohlížeče protokolu HTTP.](understanding-asp-net-ajax-debugging-capabilities/_static/image23.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image22.png)

**Obrázek 8**: UpdatePanel zprávy s odpovědí zobrazit pomocí prohlížeče protokolu HTTP.  ([Kliknutím ji zobrazíte obrázek v plné velikosti](understanding-asp-net-ajax-debugging-capabilities/_static/image24.png))


Existuje několik dalších nástrojů, které slouží k zobrazení zpráv požadavků a odpovědí kromě webového vývoje pomocné rutiny. Další dobrá možností je Fiddler, která je k dispozici zdarma na [ http://www.fiddlertool.com ](http://www.fiddlertool.com). I když aplikaci Fiddler nebude zde popsané, je také vhodné při je potřeba důkladně zkontrolovat záhlaví zpráv a data.

## <a name="debugging-with-firefox-and-firebug"></a>Ladění pomocí Firefoxu a Firebug

Aplikace Internet Explorer je stále nejpoužívanější prohlížeče, ostatní prohlížeče, jako je například Firefox staly poměrně Oblíbené a jsou používány více. Díky tomu budete chtít zobrazit a ladit vaše stránky technologie ASP.NET AJAX v aplikaci Firefox také aplikace Internet Explorer k zajištění, že vaše aplikace fungovat správně. I když Firefox nelze spojit přímo do sady Visual Studio 2008 pro ladění, má příponu volá Firebug, který slouží k ladění stránek. FireBug si můžete zdarma stáhnout tak, že přejdete do [ http://www.getfirebug.com ](http://www.getfirebug.com).

FireBug poskytuje plně vybavené prostředí ladění, který slouží k procházení kódu řádek po řádku, přístup k všechny skripty používané v rámci stránky, zobrazit strukturu modelu DOM, zobrazení stylů CSS a dokonce i sledování událostí, ke kterým dochází na stránce. Po instalaci Firebug je možný výběrem otevřete Firebug Firebug nástroje z nabídky Firefox. Jako pomocné rutiny vývoj pro Web slouží Firebug přímo v prohlížeči, i když můžete použít také jako samostatné aplikace.

Po spuštění Firebug zarážky můžete nastavit na kterýkoli řádek v souboru jazyka JavaScript, zda skript je součástí na stránce, nebo ne. Nastavit zarážku, nejdřív načtěte příslušnou stránku, kterou chcete ladit v aplikaci Firefox. Po načtení stránky vyberte skript, který chcete ladit z rozevíracího seznamu pro Firebug skripty. Zobrazí se všechny skripty používané stránky. Kliknutím na Firebug šedé na hlavním panelu oblast na řádku kde zarážka by měly patřit musí jako v sadě Visual Studio 2008 byla nastavena zarážka.

Po nastavení zarážky v Firebug lze provést akce potřebné ke spuštění skriptu, který je potřeba ladit, jako je například kliknutí na tlačítko nebo aktualizovat prohlížeč, aby aktivovat události při načtení. Spuštění se automaticky zastaví na řádek obsahující zarážku. Obrázek 9 ukazuje příklad, který se spustil zarážku v Firebug.


[![Zarážky v Firebug zpracování.](understanding-asp-net-ajax-debugging-capabilities/_static/image26.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image25.png)

**Obrázek 9**: zarážky v Firebug zpracování.  ([Kliknutím ji zobrazíte obrázek v plné velikosti](understanding-asp-net-ajax-debugging-capabilities/_static/image27.png))


Po dosažení zarážky můžete krokovat s vnořením, Krokovat přes nebo kroku mimo kód pomocí tlačítek se šipkami. Jak krokovat kód skriptu proměnné se zobrazí v pravé části ladicí program umožňuje zobrazit hodnoty a její procházení do objektů. FireBug také obsahuje zásobník volání rozevírací seznam postup spuštění skriptu, které vedly k aktuální řádek, který se právě ladí.

FireBug také zahrnuje okna konzoly, která slouží k testování různých skript příkazy, proměnné vyhodnoceny a zobrazit výstup trasování. Kliknutím na kartu konzoly v horní části okna Firebug přístupu. Na stránce, který se právě ladí můžete také "kontrolovány" zobrazíte kliknutím na kartě zkontrolujte, jestli se jeho strukturu modelu DOM a obsah. Při budou zvýrazněny myši přes různé prvky modelu DOM zobrazují v okně Inspektor odpovídající části stránky vám usnadní najdete v článku použití elementu na stránce. Hodnoty atributů, které jsou přidružené k daného elementu je možné změnit "live" a experimentovat s použití různých šířkách, styly atd. pro element. To je skvělé funkce, která vám ušetří nemusíte neustále přepínání v editor zdrojového kódu a prohlížeče Firefox, chcete-li zobrazit jak jednoduché změny projeví na stránce.

Obrázek 10 ukazuje příklad použití inspektoru modelu DOM k vyhledání textové pole s názvem txtCountry na stránce. Inspektor Firebug lze také zobrazit používané stránky, jakož i události, ke kterým dochází například sledování pohybu myši, kliknutí na tlačítko plus další styly CSS.


[![Použití modelu DOM inspectoru Firebug společnosti.](understanding-asp-net-ajax-debugging-capabilities/_static/image29.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image28.png)

**Obrázek 10**: použití Firebug modelu DOM inspector.  ([Kliknutím ji zobrazíte obrázek v plné velikosti](understanding-asp-net-ajax-debugging-capabilities/_static/image30.png))


FireBug poskytuje odlehčené způsob, jak rychle ladit stránku přímo v aplikaci Firefox, jakož i skvělým nástrojem pro kontrolu různé prvky v rámci stránky.

## <a name="debugging-support-in-aspnet-ajax"></a>Podpora ladění v rozhraní ASP.NET AJAX

Knihovna ASP.NET AJAX obsahuje mnoho různých tříd, které slouží ke zjednodušení procesu přidávání funkcí AJAX do webové stránky. Tyto třídy můžete použít k vyhledání elementů v rámci stránky a s nimi manipulovat, přidání nových ovládacích prvků, volání webové služby a i zpracování událostí. Knihovna ASP.NET AJAX obsahuje také třídy, které můžete použít k vylepšení procesů ladění stránek. V této části budete zavedené třídy Sys.Debug a zobrazit, jak můžete použít v aplikacích.

*Pomocí třídy Sys.Debug*

Sys.Debug třídy (třídy jazyka JavaScript v oboru názvů Sys) je možné provádět několik různých funkcí včetně zápisu výstupu trasování provádění kódu kontrolní výrazy a vynucení selhání tak, aby ji bylo možné ladit kódu. Používá se často v souborech ladění technologie ASP.NET AJAX library (ve výchozím nastavení nainstalované na C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0) k provedení podmíněné testy) volá se, kontrolní výrazy), které zajistí parametry jsou předány správné funkce, že objekty obsahují očekávaná data a psát příkazy trasování.

Třída Sys.Debug zveřejňuje několik různých funkcí, které lze použít ke zpracování trasování, kontrolní výrazy kódu nebo selhání, jak je znázorněno v tabulce 1.

**Tabulka 1. Sys.Debug funkcí třídy.**

| **Název funkce** | **Popis** |
| --- | --- |
| vyhodnocení (podmínka, zprávy, displayCaller) | Vyhodnotí, že parametr podmínky hodnotu true. Pokud se testovaná podmínka hodnotu false, okno se zprávou se použije k zobrazení hodnoty parametru zprávy. Pokud má parametr displayCaller hodnotu true, metoda zobrazí také informace o volajícím. |
| clearTrace() | Odstraní příkazy výstup z operace trasování. |
| Fail(Message) | Způsobí, že program k zastavení spuštěného procesu a do ladicího programu. Parametr message umožňuje zadat příslušný důvod selhání. |
| trace(Message) | Zapíše parametr zprávu do výstupu trasování. |
| traceDump (objektu, název) | Vrácení dat objektu v čitelném formátu. Název parametru slouží k zadejte jmenovku pro trasování s výpisem paměti. Ve výchozím nastavení se zapíšou všech podřízených objektů v rámci objektu jsou zálohované. |

Trasování na straně klienta je možné stejným způsobem jako funkce trasování, která je k dispozici v technologii ASP.NET. To umožňuje různé zprávy snadno vidět bez přerušení tok z aplikace. Výpis 5 ukazuje příklad použití funkce Sys.Debug.trace k zápisu do protokolu trasování. Tato funkce přebírá jednoduše zprávu, která by měla být zapsán jako parametr.

**Seznam 5. Pomocí funkce Sys.Debug.trace.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample6.js)]

Pokud při spuštění uvedeno v informacích 5 kódu nezobrazí žádný výstup trasování na stránce. Jediný způsob, jak vidět, jak to má používat k dispozici ve Visual Studio .NET, vývoj pro webové pomocné rutiny nebo Firebug okno konzoly. Pokud chcete zobrazit výstup trasování na stránce pak bude potřeba přidat značku TextArea a přiřaďte mu id TraceConsole, jak je ukázáno dále:


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample7.html)]

Všechny příkazy Sys.Debug.trace na stránce se zapíšou do TraceConsole TextArea.

V případech, ve kterém chcete zobrazit data obsažená v objektu JSON můžete použít funkci traceDump Sys.Debug třídy. Tato funkce přebírá dva parametry, včetně objektu, který by měl být zálohované do konzoly pro trasovacího a název, který slouží k identifikaci objektu ve výstupu trasování. Výpis 6 ukazuje příklad použití funkce traceDump.

**Výpis 6. Pomocí funkce Sys.Debug.traceDump.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample8.js)]

Obrázek 11 zobrazuje výstup z volání funkce Sys.Debug.traceDump. Všimněte si, že kromě zápisu dat objektu osoba, také zapíše data adresu sub objektu.

Kromě trasování, Sys.Debug třídy lze také provést kontrolní výrazy kódu. Kontrolní výrazy se používají k otestování, zda jsou splněny konkrétní podmínky, když aplikace běží. Ladicí verze knihovny skripty AJAX technologie ASP.NET obsahují několik Assert – příkazy k testování různých podmínek.

Výpis 7 ukazuje příklad použití funkce Sys.Debug.assert k otestování podmínky. Kód testuje, zda adresa objektu má hodnotu null, před aktualizací osoba.


[![Výstup Sys.Debug.traceDump funkce.](understanding-asp-net-ajax-debugging-capabilities/_static/image32.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image31.png)

**Obrázek 11**: výstup Sys.Debug.traceDump funkce.  ([Kliknutím ji zobrazíte obrázek v plné velikosti](understanding-asp-net-ajax-debugging-capabilities/_static/image33.png))


**Výpis 7. Pomocí funkce Debug.Assert –.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample9.js)]

Tři parametry jsou předány včetně podmínku, která má vyhodnotit, zpráva se zobrazí, pokud výraz vrátí hodnotu false a zda mají být zobrazeny informace o volajícím. V případech, kdy kontrolní výraz selže bude zprávy zobrazit, a také informace o subjektu volajícím Pokud dřív platilo třetí parametr. Obrázek 12 znázorňuje příklad dialogové okno selhání, který se zobrazí, pokud výraz je vidět na výpisu 7 se nezdaří.

Konečná funkce pro pokrytí je Sys.Debug.fail. Pokud chcete vynutit selhání na konkrétní řádek ve skriptu kódu můžete přidat volání Sys.Debug.fail spíše než příkaz ladicího programu, obvykle používaných v aplikacích jazyka JavaScript. Funkce Sys.Debug.fail přijímá jako parametr jeden řetězec, který představuje důvod selhání, jak je ukázáno dále:


[!code-css[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample10.css)]


[![Zpráva o selhání Sys.Debug.assert.](understanding-asp-net-ajax-debugging-capabilities/_static/image35.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image34.png)

**Obrázek 12**: zpráva o selhání A Sys.Debug.assert.  ([Kliknutím ji zobrazíte obrázek v plné velikosti](understanding-asp-net-ajax-debugging-capabilities/_static/image36.png))


Při výpisu Sys.Debug.fail dochází při provádění skriptu, hodnota parametru zpráva se zobrazí v konzole ladění aplikace, jako je Visual Studio 2008 a zobrazí se výzva k ladění aplikace. Jeden případ, kdy to může být užitečná při nelze nastavit zarážku s Visual Studio 2008 na vložený skript, ale chtěli kódu pro ukončení na konkrétní řádek, takže si můžete prohlédnout hodnoty proměnných.

*Principy vlastnosti ovládacího prvku ScriptManager ScriptMode*

Knihovna ASP.NET AJAX obsahuje ladění a vydání verze skriptu, které jsou nainstalovány na C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 ve výchozím nastavení. Ladění skriptů se přehledně naformátovaná, snadno čitelný a mít několik volání Sys.Debug.assert rozmístěno jinde v rámci jejich při vydání skripty vynechají prázdné znaky a pomocí třídy Sys.Debug opatrně minimalizovat jejich celkovou velikost.

Ovládací prvek ScriptManager přidat do stránky technologie ASP.NET AJAX načte atribut compilation element debug v souboru web.config, chcete-li určit, které verze knihovny skriptů pro načtení. Můžete však určit, zda ladit nebo vydaná verze skriptů jsou načtené (knihovna skripty nebo vlastní skripty) tak, že změníte vlastnost ScriptMode. ScriptMode přijímá výčet ScriptMode, jejíž členové zahrnují automatické, ladění, vydání a dědičnosti.

Výchozí hodnota ScriptMode na hodnotu Auto, což znamená, že bude kontrolovat ScriptManager atribut debug v souboru web.config. Při ladění má hodnotu false ScriptManager načte verzi technologie ASP.NET AJAX knihovny skriptů. Při ladění má hodnotu true ladicí verzi tyto skripty načtou. Změna vlastnosti ScriptMode vydání nebo ladění způsobí, že ovládacímu prvku ScriptManager načítat příslušné skripty bez ohledu na to, jakou hodnotu má atribut debug v souboru web.config. Výpis 8 ukazuje příklad použití ovládacího prvku ScriptManager načítat skripty ladění z knihovny ASP.NET AJAX.

**Výpis 8. Načítají se ladění skriptů pomocí ScriptManager**.


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample11.aspx)]

Různé verze vlastních skriptů (ladění nebo vydání) můžete také načíst pomocí prvku ScriptManager skripty vlastnosti spolu s komponentou ScriptReference, jak je vidět na výpisu 9.

**Výpis 9. Načítání vlastních skriptů používajících ScriptManager.**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample12.aspx)]

> [!NOTE]
> Pokud načítáte vlastní skripty pomocí komponenty ScriptReference musíte upozornit ScriptManager po dokončení načítání skriptu přidejte následující kód v dolní části skriptu:


[!code-csharp[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample13.cs)]

Kód je vidět na výpisu 9 říká vyhledejte ladicí verzi souboru, který uživatel tak bude automaticky hledat Person.debug.js místo Person.js ovládacímu prvku ScriptManager. Pokud se soubor Person.debug.js nenajde, že bude vyvolána chyba.

V případech, kdy chcete, aby ladění nebo vlastní skript, který se má načíst verzi na základě hodnoty vlastnosti ScriptMode nastavena ovládacímu prvku ScriptManager můžete nastavit vlastnosti ovládacího prvku ScriptReference ScriptMode do dědičnosti. To způsobí, že správná verze vlastní skript, který se má načíst stupněm MODERATE prvku ScriptManager ScriptMode vlastnost jak je uvedeno v informacích 10. Protože ScriptMode vlastnost ovládacího prvku ScriptManager je nastavena na ladění, skript Person.debug.js načíst a použít na stránce.

**Seznam 10. ScriptMode dědění z objektu ScriptManager pro vlastní skripty.**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample14.aspx)]

Pomocí vlastnosti ScriptMode odpovídajícím způsobem můžete snadněji ladit aplikace a zjednodušuje celý proces. Skripty verze knihovny ASP.NET AJAX se místo toho obtížně procházení a čtení, protože formátování kódu byla odebrána při ladění skriptů se formátují speciálně pro účely ladění.

## <a name="conclusion"></a>Závěr

Technologie ASP.NET AJAX od Microsoftu poskytuje solidní základ pro vytváření aplikací s podporou jazyka AJAX, která můžou zvýšit celkové prostředí koncového uživatele. Ale jako se jakékoli programovací technologie, chyby a další potíže s aplikacemi jistě, vyvstanou vám. Znalosti o různých možnostech ladění k dispozici můžete spoustu času a výsledek uložit do více stabilní produkt.

V tomto článku jste se seznámili s několik různých postupů pro ladění stránek ASP.NET AJAX, včetně aplikace Internet Explorer s Visual Studio 2008, pomocné rutiny Web Development a Firebug. Tyto nástroje můžete zjednodušit celkový proces ladění, protože můžete používat data proměnných, provede kódu řádek po řádku a zobrazit příkazy trasování. Kromě různých ladicí nástroje popsané také viděli použití třídy Sys.Debug knihovna ASP.NET AJAX v aplikaci a použití třídy správce skriptů pro načtení ladění a vydání verze skriptů.

## <a name="bio"></a>Životopis

DaN Wahlin (Microsoft Most Valuable Professional pro ASP.NET a webových služeb XML) je .NET development instruktorem a architektura konzultant na technické školení rozhraní ([www.interfacett.com)](http://www.interfacett.com). DaN založil XML pro vývojáře v prostředí ASP.NET Web ([www.XMLforASP.NET](http://www.XMLforASP.NET)), která je na mluvčího INETA kancelář a přednáší na konferencích několik. DaN spoluautorem DNA Windows Professional (Wrox), technologie ASP.NET: tipy, kurzy a kód (edice), technologie ASP.NET 1.1 Insider řešení, Professional ASP.NET 2.0 AJAX (Wrox), technologie ASP.NET 2.0 MVP změní a vytvořené XML pro vývojáře využívající technologii ASP.NET (edice). Když mu není psaní kódu, články nebo knihy, Dan infrastrukturální, zápis a Hudba záznam a přehrávání golf a Basketbalový se svou ženou a děti.

Scott Cate má práce s Microsoft webových technologiích od roku 1997 a je prezident myKB.com ([www.myKB.com](http://www.myKB.com)) kde mu se specializuje na technologie ASP.NET psaní aplikací, zaměřuje na znalostní báze softwarová řešení založených na. Scott můžete kontaktovat prostřednictvím e-mailové adrese [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) nebo na svém blogu [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Předchozí](understanding-asp-net-ajax-web-services.md)
