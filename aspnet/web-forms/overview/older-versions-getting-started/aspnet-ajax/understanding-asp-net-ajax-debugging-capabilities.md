---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
title: Principy prvku ASP.NET AJAX ladění možnosti | Microsoft Docs
author: scottcate
description: Možnost ladění kódu je odborností, která by měla mít každý vývojář v jejich arsenál bez ohledu na to, které používají technologii. Když se celá řada vývojářů...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 7f9380c6-19f7-4c82-a019-916ec6dffc9c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
msc.type: authoredcontent
ms.openlocfilehash: f082e2206f5e691579670e42634f30b57e3b3593
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890928"
---
<a name="understanding-aspnet-ajax-debugging-capabilities"></a>Seznámení s možností ladění prvku ASP.NET AJAX
====================
podle [Scott Dikovat](https://github.com/scottcate)

[Stáhnout PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial06_Debugging_MS_Ajax_Applications_cs.pdf)

> Možnost ladění kódu je odborností, která by měla mít každý vývojář v jejich arsenál bez ohledu na to, které používají technologii. Celá řada vývojářů jsou uzpůsobené pro použití sady Visual Studio .NET nebo Web Developer Express k ladění aplikací ASP.NET, které používají Kód VB.NET nebo C#, nejsou některé uvědomit, že je také velmi užitečné při ladění kódu na straně klienta jako je JavaScript. Stejný typ techniky používá k ladění aplikací .NET lze použít také k jazyka AJAX a přesněji řečeno aplikace ASP.NET AJAX.


## <a name="debugging-aspnet-ajax-applications"></a>Ladění aplikací ASP.NET AJAX

Dana Wahlin

Možnost ladění kódu je odborností, která by měla mít každý vývojář v jejich arsenál bez ohledu na to, které používají technologii. Je samozřejmé, seznámení s možnostmi ladění, které jsou k dispozici na můžete uložit obrovské množství času na projekt a možná i několik hlavu. Celá řada vývojářů jsou uzpůsobené pro použití sady Visual Studio .NET nebo Web Developer Express k ladění aplikací ASP.NET, které používají Kód VB.NET nebo C#, nejsou některé uvědomit, že je také velmi užitečné při ladění kódu na straně klienta jako je JavaScript. Stejný typ techniky používá k ladění aplikací .NET lze použít také k jazyka AJAX a přesněji řečeno aplikace ASP.NET AJAX.

V tomto článku uvidíte, jak Visual Studio 2008 a několik dalších nástrojů slouží k ladění aplikací ASP.NET AJAX rychle vyhledat chyby a ostatní problémy. Toto pojednání bude zahrnovat informace o povolení aplikace Internet Explorer 6 nebo vyšší pro ladění, pomocí sady Visual Studio 2008 a Průzkumník skriptu krok prostřednictvím kódu a také pomocí jiných volné nástrojů, například vývoj webové pomocné rutiny. Budete také zjistěte, jak k ladění aplikací ASP.NET AJAX v Firefox pomocí rozšíření s názvem Firebug které lze v průběhu kódu jazyka JavaScript přímo v prohlížeči bez dalších nástrojů. Nakonec se seznámíte na třídy v knihovně ASP.NET AJAX, který vám pomůže s různými ladění úkoly, jako je například trasování a příkazy assertion kódu.

Před pokusem o ladění stránky v prohlížeči Internet Explorer je několik základní kroky, které budete muset provést, abyste ji povolit pro ladění. Podívejme se na některé základní nastavení požadavků, které je třeba provést, abyste mohli začít.

## <a name="configuring-internet-explorer-for-debugging"></a>Konfigurace aplikace Internet Explorer pro ladění

Většina lidí nejsou nepotřebujete vidět JavaScript potíží vzniklých na webu zobrazí v aplikaci Internet Explorer. Ve skutečnosti průměrná uživatele i vědět, co dělat, když viděli totiž chybovou zprávu. V důsledku toho ladění možnosti jsou ve výchozím nastavení vypnuté v prohlížeči. Je však velmi jednoduché zapnout ladění a umístí jej používat při vývoji nových aplikací AJAX.

K povolení funkcí ladění, přejděte na možnosti Internetu nástrojů v nabídce aplikace Internet Explorer a vyberte kartě Upřesnit. V části funkce procházení zkontrolujte, zda jsou zaškrtnuté políčko následující položky:

- Zakázat ladění skriptů (aplikace Internet Explorer)
- Zakázat ladění skriptů (ostatní)

I když není požadováno, pokud se pokoušíte ladění aplikace výhodné žádné chyby jazyka JavaScript na stránce budou okamžitě viditelné a zřejmé. Můžete vynutit všechny chyby zobrazit s okno se zprávou zaškrtnutím políčka "Zobrazovat oznámení o chybě každý skript". Přestože se skvělou možnost zapnout, když vyvíjíte aplikaci, můžete rychle stane obtěžující, pokud jste právě perusing ostatních webů vzhledem k tomu, že jsou poměrně vhodné šanci zjištění chyby jazyka JavaScript.

Obrázek 1 ukazuje jaké v Internet Exploreru, dialogové okno Upřesnit by měla vypadat, když je správně nakonfigurovaná pro ladění.


[![Konfigurace aplikace Internet Explorer pro ladění.](understanding-asp-net-ajax-debugging-capabilities/_static/image2.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image1.png)

**Obrázek 1**: Konfigurace aplikace Internet Explorer pro ladění.  ([Kliknutím zobrazit obrázek v plné velikosti](understanding-asp-net-ajax-debugging-capabilities/_static/image3.png))


Jakmile ladění je zapnutý, zobrazí se nová položka nabídky se zobrazí v nabídce zobrazení s názvem Script Debugger. Má dvě možnosti, které jsou k dispozici včetně otevřete a zalomení v další příkaz. Pokud je vybrána otevřete budete vyzváni k ladění stránka v sadě Visual Studio 2008 (Všimněte si, že Visual Web Developer Express lze také použít pro ladění). Pokud Visual Studio .NET je aktuálně spuštěna, můžete použít tuto instanci nebo vytvořte novou instanci. Pokud je vybrána konec na další příkaz budete vyzváni k ladění stránku při spuštění kódu JavaScript. Pokud se provede kódu jazyka JavaScript v události při načtení stránky můžete obnovit stránku k aktivaci na relaci ladění. Pokud kód jazyka JavaScript se spouští po stisknutí tlačítka ladicí program se spustí hned po kliknutí na tlačítko.

> *> [!NOTE] Pokud používáte v systému Windows Vista s přístup řízení Uživatelských účtů povoleno a máte Visual Studio 2008 spuštěn jako správce, Visual Studio se nepodaří připojit k procesu, když se zobrazí výzva k připojení. Chcete-li tento problém obejít, první spuštění sady Visual Studio a použít tuto instanci k ladění.*


I když v další části ukazují, jak ladit stránku ASP.NET AJAX přímo z v sadě Visual Studio 2008, pomocí možnosti Internet Explorer Script Debugger je užitečné, když je již otevřeno stránky a chcete podrobněji zkontrolovat.

## <a name="debugging-with-visual-studio-2008"></a>Ladění pomocí sady Visual Studio 2008

Visual Studio 2008 poskytuje funkce ladění, která vývojářům po celém světě závisí na každý den k ladění aplikací .NET. Integrované ladicí program umožňuje krok prostřednictvím kódu, zobrazení, data objektu, sledovat určité proměnné, sledování zásobníku volání a mnoho dalšího. Ladicí program kromě ladění kódu VB.NET nebo C#, je také užitečné při ladění aplikací ASP.NET AJAX a vám umožňuje procházet řádky kódu JavaScript. Informace, které jsou podle fokus na techniky, které slouží k ladění skriptů na straně klienta souborů a nikoli poskytuje discourse na celý proces ladění aplikací pomocí sady Visual Studio 2008.

Proces ladění stránka v sadě Visual Studio 2008 lze spustit v několika různými způsoby. První můžete použít možnost Internet Explorer Script Debugger uvedených v předchozí části. Tento postup funguje dobře, pokud už je načtený na stránce v prohlížeči a chcete spustit ladění ho. Alternativně můžete na stránky ASPX v Průzkumníku řešení klikněte pravým tlačítkem a vyberte možnost nastavit jako úvodní stránku v nabídce. Pokud jste zvyklí ladění stránek ASP.NET pak kroky pravděpodobně dokončíte před. Po stisknutí klávesy F5 můžete ladit stránky. Ale když obecně můžete nastavit zarážky kdekoli chcete v VB.NET nebo C# kódu, který není vždy tomu u JavaScript jak zjistíte dále.

*Vložené Versus externích skriptů*

Ladicí program Visual Studio 2008 zpracovává vložený na stránce liší od externích souborů JavaScript JavaScript. S externího skriptu soubory můžete otevřít soubor a zarážku na kterýkoli řádek, který zvolíte. Klikněte v oblasti šedé panelu na levé straně okna editoru kódu lze nastavit zarážky. Když je přímo na stránku pomocí vložených JavaScript `<script>` značky, klikněte v oblasti šedé panelu nastavení boru přerušení není možnost. Pokusí se nastavit zarážky řádek vložený skript povede upozornění s oznámením "Tento není platné umístění pro zarážku".

Řešení tohoto problému můžete získat přesunutím kód do souboru externí .js a odkazování na pomocí atribut src &lt;skriptu&gt; značky:


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample1.html)]

Co když přesun kód do externího souboru není možnost nebo vyžaduje další práci, než je vhodné? Při nelze nastavit zarážky pomocí editoru, můžete přidat příkaz ladicí program přímo do kódu, kde chcete spustit ladění. Můžete taky třídu Sys.Debug k dispozici v knihovně prvku ASP.NET AJAX Chcete-li vynutit, chcete-li spustit ladění. Dozvíte další informace o třídě Sys.Debug později v tomto článku.

Příklad použití `debugger` – klíčové slovo se zobrazí v výpis 1. Tento příklad vynutí ladicí program na přerušení správné, než Přišla žádost o aktualizaci funkce.

**Výpis 1. Chcete-li vynutit pro přerušení ladicího programu sady Visual Studio .NET pomocí klíčového slova ladicí program.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample2.js)]

Jakmile je dosáhl příkaz ladicí program vyzve k ladění stránky pomocí sady Visual Studio .NET a můžete začít krokování kódu. Při provádění jste to vy může dojít k potížím s přístupem k prvku ASP.NET AJAX knihovny skriptu soubory používané na stránce proto budeme věnovat pomocí sady Visual Studio. Průzkumník skriptu NET společnosti.

## <a name="using-visual-studio-net-windows-to-debug"></a>Pomocí sady Visual Studio .NET systému Windows k ladění

Po spuštění relace ladění a začnete s návodem kódu pomocí výchozí F11 klíč, se můžete setkat dialogového okna chyby uvedené v viz obrázek 2, pokud na stránce použít všechny soubory skriptů jsou otevřené a dostupné pro ladění.


[![Dialogové okno chyby zobrazí, když žádný zdrojový kód je k dispozici pro ladění.](understanding-asp-net-ajax-debugging-capabilities/_static/image5.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image4.png)

**Obrázek 2**: chybové dialogové okno zobrazí, když žádný zdrojový kód je k dispozici pro ladění.  ([Kliknutím zobrazit obrázek v plné velikosti](understanding-asp-net-ajax-debugging-capabilities/_static/image6.png))


Toto dialogové okno se zobrazí, protože sady Visual Studio .NET není se, jak získat ke zdrojovému kódu některých skriptů odkazuje na stránku. Když to může být poměrně frustrující zpočátku je jednoduchý oprava. Po spuštění na relaci ladění a stiskněte tlačítko zarážku, přejít do okna ladění skriptu Průzkumníka Windows v nabídce sady Visual Studio 2008, nebo použijte klávesovou zkratku Ctrl + Alt + N.

> *> [!NOTE] Pokud se nezobrazí v nabídce Průzkumník skriptu uvedené, přejděte k nástroje* *přizpůsobit* *příkazů v nabídce sady Visual Studio .NET. V části kategorie vyhledejte položku ladění a klikněte na něj zobrazíte všechny položky nabídky k dispozici. V seznamu příkazů, posuňte se dolů a Průzkumník skriptu a přetáhněte ji na ladění* *nabídky systému Windows v uvedených výše. Díky tomuto budou položku nabídky Průzkumník skriptu k dispozici při každém spuštění sady Visual Studio .NET.*


Průzkumník skriptu lze použít k zobrazení všech skriptů, které se použije na stránce a je otevře v editoru kódu. Jakmile otevřete Průzkumník skriptu, dvakrát klikněte na právě laděn a otevře se v okně editoru kódu stránky .aspx. Proveďte stejné akce pro všechny ostatní skripty zobrazí v Průzkumníku skriptu. Jakmile všechny skripty jsou v okně Kód můžete otevřít stisknutím klávesy F11 (a použití jiných ladění zkratky) krokovat kód. Obrázek 3 ukazuje příklad Průzkumníka skriptu. Vypíše aktuální soubor laděné (Demo.aspx) a také dva vlastní skripty a dva skripty dynamicky vloženy do stránce ScriptManager AJAX technologie ASP.NET.


[![Průzkumník skriptu poskytuje snadný přístup k skripty použité na stránce.](understanding-asp-net-ajax-debugging-capabilities/_static/image8.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image7.png)

**Obrázek 3**. Průzkumník skriptu poskytuje snadný přístup k skripty použité na stránce.  ([Kliknutím zobrazit obrázek v plné velikosti](understanding-asp-net-ajax-debugging-capabilities/_static/image9.png))


Některé další systému windows slouží také k poskytují užitečné informace v průběhu kódu na stránce. Můžete například místní hodnoty – okno zobrazit hodnoty různých proměnných, které jsou použity na stránce aktuálního okna k vyhodnocení, jestli konkrétní proměnné nebo podmínky a zobrazte výstup. Ve výstupním okně můžete také zobrazit příkazy trasování, které jsou zapsány pomocí funkce Sys.Debug.trace (který se budeme dále v tomto článku) nebo v aplikaci Internet Explorer Debug.writeln – funkce.

V průběhu kódu pomocí ladicího programu můžete myší proměnných v kódu zobrazíte hodnotu, která jsou přiřazena. Však nástroj script debugger příležitostně nezobrazí se nic jako myší dané proměnné jazyka JavaScript. A zobrazit tak hodnotu, zvýrazněte příkaz nebo proměnnou, kterou zkoušíte zobrazit v okně editoru kódu a pak myši nad ním. I když tento postup nefunguje v každé situaci, kolikrát bude moci zobrazit tak hodnotu bez nutnosti Hledat v různých ladění okna například místní hodnoty – okno.

Video návod demonstraci některé funkce popsané v tomto poli lze zobrazit v [ http://www.xmlforasp.net ](http://www.xmlforasp.net).

## <a name="debugging-with-web-development-helper"></a>Ladění pomocí Pomocníka pro vývoj pro Web

I když Visual Studio 2008 (a Visual Web Developer Express 2008) jsou velmi podporující nástroje pro ladění, existují další možnosti, které lze použít také které jsou více šedé. Jedním z nejnovější nástroje potřebné k uvolnění je pomocná vývoj pro Web. Nikhil Kothari společnosti Microsoft, (jeden z klíčů architekty prvku ASP.NET AJAX v Microsoftu) napsali tohoto vynikající nástroje, které můžete provést celou řadu různých úloh z jednoduchého ladění při zobrazení zprávy požadavku a odpovědi protokolu HTTP. Vývoj webové pomocné rutiny si můžete stáhnout tady [ http://projects.nikhilk.net/Projects/WebDevHelper.aspx ](http://projects.nikhilk.net/Projects/WebDevHelper.aspx).

Vývoj webové pomocné rutiny lze přímo v aplikaci Internet Explorer, takže je vhodné používat. Je spuštěno výběrem pomocné rutiny vývoj webových nástrojů v nabídce aplikace Internet Explorer. Otevře se nástroj v dolní části Prohlížeč, který je dobrý, protože nemáte nechte prohlížeče provést několik úloh, jako je například protokolování zpráv žádostí a odpovědí HTTP. Obrázek 4 ukazuje, jak webové vývoj pomocná vypadá v akci.


[![Vývoj webové pomocné rutiny](understanding-asp-net-ajax-debugging-capabilities/_static/image11.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image10.png)

**Obrázek 4**: vývoj webové pomocné rutiny ([Kliknutím zobrazit obrázek v plné velikosti](understanding-asp-net-ajax-debugging-capabilities/_static/image12.png))


Vývoj webové pomocné rutiny není nástroj použijete krok prostřednictvím kódu řádek po řádku jako s Visual Studio 2008. Však může sloužit k zobrazení výstupu trasování, snadno vyhodnocení proměnné ve skriptu nebo zkoumat data jsou v objektu JSON. Je také užitečné pro zobrazení dat, který je předán do a z stránku ASP.NET AJAX a serveru.

Jakmile pomocná vývoj webové otevřít v aplikaci Internet Explorer, skriptu musí být povoleno ladění tak, že vyberete skript povolit ladění skriptů v nabídce pomocníka vývoj webů jak je znázorněno na obrázku 4 dříve. To umožňuje nástroj pro zachycení chyby spuštění stránky. Umožňuje také snadný přístup k trasování zpráv, které jsou výstupem na stránce. Zobrazit informace o trasování nebo provést příkazy skriptu pro testování různých funkcí v rámci stránky, vyberte možnost skriptu zobrazit skript konzoly v nabídce vývoj webové pomocné rutiny. To poskytuje přístup k okno příkazového řádku a jednoduchý hodnot proměnných.

*Zobrazení zpráv trasování a Data objektu JSON*

Příkazové podokno lze spustit příkazy skriptu nebo dokonce načíst nebo uložit skripty, které se používají pro testování různých funkcí na stránce. Příkazové okno zobrazí trasování nebo ladění zprávy, které jsou zapsané podle stránky zobrazení. Výpis 2 ukazuje, jak k zápisu trasování zprávy pomocí Internet Exploreru Debug.writeln – funkce.

**Výpis 2. Zápis na straně klienta trasování zprávy pomocí třídy ladění.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample3.js)]

Pokud vlastnost LastName obsahuje hodnotu Doe, pomocná vývoj webových se zobrazí zpráva "jméno osoby: Doe" v příkazovém okně konzole skriptu (za předpokladu, že je povoleno ladění). Vývoj webové pomocné rutiny také přidá objekt nejvyšší úrovně debugService do stránek, které lze zapsat informace trasování nebo zobrazit obsah objektů JSON. Výpis 3 ukazuje příklad použití třídy debugService trasování funkce.

**Výpis 3. Pomocí webové vývoj pomocná debugService třída zapsat zprávu trasování.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample4.js)]

Dobrý funkce třídy debugService je, že bude fungovat i v případě, že v aplikaci Internet Explorer a usnadňuje tak vždy přístup k datům trasování, když je spuštěna pomocná vývoj pro Web není povoleno ladění. Když nástroj není používán k ladění na stránce, příkazy trasování se budou ignorovat vzhledem k tomu, že volání window.debugService vrátí hodnotu false.

Třída debugService také umožňuje data JSON objekt lze zobrazit pomocí okně pomocníka vývoj webové kontroly. Výpis 4 vytvoří jednoduchý objekt JSON obsahující data osoby. Po vytvoření objektu došlo k volání na debugService zkontrolovat třídy a funkce umožňující objekt JSON, který má být prověřovány vizuálně.

**Výpis 4. Chcete-li zobrazit data JSON objektu pomocí funkce debugService.inspect.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample5.js)]

Na stránce nebo prostřednictvím příkazové podokno při volání funkce GetPerson() bude v dialogovém okně objektu Inspector zobrazující jak je znázorněno na obrázku 5. Vlastnosti v rámci objektu můžete změnit dynamicky podle zvýraznění, změna hodnoty v textovém poli hodnota a pak kliknete na odkaz aktualizace. Pomocí objektu Inspector mohou přehledné zobrazení dat objektu JSON a experimentovat s použití různé hodnoty pro vlastnosti.

*Chyby ladění*

Kromě toho, že data trasování a objekty JSON, který se má zobrazit, můžete vývoj webů Pomocník také pomáhá při ladění chyby na stránce. Pokud dojde k chybě, zobrazí se výzva k pokračovat na další řádek kódu nebo laďte skript (viz obrázek 6). Zobrazuje pro okno dialogové okno se chyba skriptu, dokončení volání zásobník a čísla řádků, abyste mohli snadno identifikovat, kde jsou problémy v rámci skriptu.


[![Používání okna objekt Inspector zobrazíte objekt JSON.](understanding-asp-net-ajax-debugging-capabilities/_static/image14.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image13.png)

**Obrázek 5**: použití okna objekt Inspector zobrazíte objekt JSON.  ([Kliknutím zobrazit obrázek v plné velikosti](understanding-asp-net-ajax-debugging-capabilities/_static/image15.png))


Výběrem možnosti ladění, můžete spustit přímo v příkazové podokno pomocníka vývoj webové k zobrazení hodnot proměnných, vypsat objekty JSON, plus další příkazy skriptu. Pokud stejnou akci, která způsobila chybu je prováděno také a Visual Studio 2008 je dostupný na počítači, zobrazí se výzva k spustit relaci ladění, takže můžete krokovat kód řádek po řádku, jak je popsáno v předchozí části.


[![Webové pomocné vývoj dialogové okno chyby skriptu](understanding-asp-net-ajax-debugging-capabilities/_static/image17.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image16.png)

**Obrázek 6**: Pomocná vývoj webové dialogové okno chyby skriptu ([Kliknutím zobrazit obrázek v plné velikosti](understanding-asp-net-ajax-debugging-capabilities/_static/image18.png))


*Probíhá kontrola požadavku a odpovědi na zprávy*

Při ladění prvku ASP.NET AJAX stránky je často užitečné zobrazit zprávy požadavku a odpovědi posílaná mezi stránky a serveru. Zobrazení obsahu v rámci zpráv umožňuje zobrazit, pokud je správná data předávána a také velikost zprávy. Vývoj webové pomocné rutiny nabízí vynikající funkci protokolovacího nástroje zpráva HTTP, které usnadňuje zobrazit data jako nezpracovaný text nebo v přehlednějším tvaru.

Chcete-li zobrazit zprávy požadavku a odpovědi prvku ASP.NET AJAX, musí být povolena protokoly HTTP tak, že vyberete HTTP povolit protokolování HTTP z nabídky vývoj webové pomocné rutiny. Po povolení všech zpráv odeslaných z aktuální stránky lze zobrazit v prohlížeči protokolu HTTP, který je přístupný výběrem HTTP zobrazit protokoly HTTP.

Sice jistě užitečné zobrazení nezpracovaná text odeslaný v každou zprávu žádosti a odpovědi (a možnost v vývoj webové pomocné rutiny), je často usnadňují zobrazit data zpráv ve více grafické formátu. Jakmile bylo povoleno protokolování HTTP a byly zaprotokolovány zpráv, zpráv data lze zobrazit dvojitým kliknutím na zprávu v prohlížeči protokolu HTTP. To vám umožní zobrazit všechny hlavičky přidružené zprávu, jakož i skutečné zpráva obsahu. Obrázek 7 znázorňuje příklad zprávu žádosti a odpovědi zprávy zobrazit v okně nástroje HTTP Log Viewer.


[![Použití prohlížeče protokolu HTTP pro zobrazení dat zprávu žádosti a odpovědi.](understanding-asp-net-ajax-debugging-capabilities/_static/image20.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image19.png)

**Obrázek 7**: použití prohlížeče protokolu HTTP pro zobrazení dat zprávu žádosti a odpovědi.  ([Kliknutím zobrazit obrázek v plné velikosti](understanding-asp-net-ajax-debugging-capabilities/_static/image21.png))


Prohlížeč protokolu HTTP automaticky analyzuje objekty JSON a zobrazí je pomocí stromové zobrazení, což snadno a rychle zobrazit data vlastností objektu. Při UpdatePanel se používá v stránku ASP.NET AJAX, prohlížeč dělí na jednotlivé části zprávy na jednotlivé části, jak je znázorněno na obrázku 8. Toto je skvělé funkce, která chcete zobrazit a co je ve zprávě ve srovnání se zobrazení nezpracovaná data zpráv mnohem jednodušší.


[![UpdatePanel zpráva odpovědi zobrazit pomocí prohlížeče protokolu HTTP.](understanding-asp-net-ajax-debugging-capabilities/_static/image23.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image22.png)

**Obrázek 8**: zpráva odpovědi UpdatePanel zobrazit pomocí prohlížeče protokolu HTTP.  ([Kliknutím zobrazit obrázek v plné velikosti](understanding-asp-net-ajax-debugging-capabilities/_static/image24.png))


Existuje několik dalších nástrojů, které lze použít k zobrazení zpráv žádostí a odpovědí kromě vývoj webové pomocné rutiny. Dobrý další možnost spočívá v aplikaci Fiddler, která je k dispozici zdarma v [ http://www.fiddlertool.com ](http://www.fiddlertool.com). I když aplikaci Fiddler nebude tady popisovaných, je také vhodný když potřebujete důkladně zkontrolovat hlavičky zpráv a data.

## <a name="debugging-with-firefox-and-firebug"></a>Ladění pomocí prohlížeče Firefox a Firebug

Při aplikaci Internet Explorer je stále nejčastěji používané prohlížeče, jiné prohlížeče, jako je například Firefox mít stane velmi oblíbenou a jsou používány více. Výsledkem je budete chtít zobrazit a ladění vaší stránky ASP.NET AJAX v Firefox a také aplikace Internet Explorer zajistit, že vaše aplikace fungovat správně. Přestože Firefox nelze zapojení přímo do sady Visual Studio 2008 pro ladění, má rozšíření názvem Firebug, který slouží k ladění stránek. FireBug lze stáhnout zdarma přechodem na [ http://www.getfirebug.com ](http://www.getfirebug.com).

FireBug nabízí plnohodnotné prostředí ladění kterého lze jednotlivé řádky kódu kroky, přístup všechny skripty použité v rámci stránky, zobrazit DOM struktury, zobrazení stylů CSS a i sledování událostí, ke kterým došlo na stránce. Po instalaci Firebug přístupná výběrem otevřete Firebug Firebug nástrojů v nabídce Firefox. Jako webové vývoj pomocné rutiny slouží Firebug přímo v prohlížeči, i když můžete použít také jako samostatné aplikace.

Po Firebug běží, zarážky můžete nastavit na kterýkoli řádek v souboru jazyka JavaScript, zda skript je vložen do stránky, nebo ne. K nastavení boru přerušení, nejdřív načtěte na příslušnou stránku, kterou chcete ladit v Firefox. Po načtení stránky, vyberte skript pro ladění z rozevíracího seznamu Firebug pro skripty. Všechny skripty používané na stránce se zobrazí. Kliknutím v Firebug na šedé oznamovací oblasti na ose kde by měli zarážce přejít musí stejně, jako by se v sadě Visual Studio 2008 je nastavit zarážky.

Po zarážku byla nastavena v Firebug můžete provést akce potřebné ke spuštění skriptu, který potřebuje chcete ladit například kliknutí na tlačítko nebo aktualizaci prohlížeče pro aktivaci události při načtení. Spuštění se automaticky zastaví na řádek obsahující bod přerušení. Obrázek 9 ukazuje příklad zarážek, která byla spuštěna v Firebug.


[![Zpracování zarážky v Firebug.](understanding-asp-net-ajax-debugging-capabilities/_static/image26.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image25.png)

**Obrázek 9**: zpracování zarážky v Firebug.  ([Kliknutím zobrazit obrázek v plné velikosti](understanding-asp-net-ajax-debugging-capabilities/_static/image27.png))


Po zarážku je přístupů můžete krok do, krok přes nebo vystoupení ze kódu pomocí tlačítek se šipkami. Krocích kód skriptu proměnné jsou zobrazeny v pravé části ladicí program, který vám umožní prohlédnout hodnoty a procházení do objektů. FireBug zahrnuje také rozevírací seznam zásobníkem volání. Chcete-li zobrazit tento skript provádění kroků, které vedly k aktuálnímu řádku laděné.

FireBug také zahrnuje okna konzoly, která slouží k testování různých skriptu příkazy, proměnné, vyhodnotí a zobrazit výstup trasování. Kliknutím na kartu konzoly v horní části okna Firebug je přístupný. Stránka laděné můžete také být "prozkoumá" a zobrazit jeho DOM strukturu a obsah, klikněte na kartě kontroly. Jak jste se zvýrazní myši přes různé prvky DOM zobrazený v okně inspector příslušné části stránky a usnadňuje tak zjistit, kde se používá element na stránce. Hodnoty přidružené daného elementu lze změnit "live" a experimentovat s použitím různých šířek, styly, atd. pro element. To je dobrý funkce, která ušetří práci s neustále přepínat mezi editoru zdrojového kódu a prohlížeče Firefox, chcete-li zobrazit změny vliv jak jednoduché na stránce.

Obrázek 10 ukazuje příklad použití DOM inspector najít textové pole s názvem txtCountry na stránce. Nástroj inspector Firebug lze také zobrazit používány na stránce, jakož i událostí, ke kterým dochází například sledování pohyb myši, kliknutí na tlačítko plus další stylů CSS.


[![Pomocí na Firebug DOM inspector.](understanding-asp-net-ajax-debugging-capabilities/_static/image29.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image28.png)

**Obrázek 10**: pomocí Firebug DOM inspector.  ([Kliknutím zobrazit obrázek v plné velikosti](understanding-asp-net-ajax-debugging-capabilities/_static/image30.png))


FireBug poskytuje šedé – způsob, jak rychle ladění stránky přímo v Firefox, jakož i vynikající nástroj pro zkontrolujete různé prvky v rámci dané stránky.

## <a name="debugging-support-in-aspnet-ajax"></a>Podpora ladění v rozhraní ASP.NET AJAX

Knihovna prvku ASP.NET AJAX obsahuje mnoho různých tříd, které umožňuje zjednodušit proces přidávání možnosti AJAX do webové stránky. Vyhledejte elementů v rámci stránky a s nimi manipulovat, přidejte nové ovládací prvky, volají webové služby, a to i zpracování událostí můžete použít tyto třídy. Knihovna prvku ASP.NET AJAX také obsahuje třídy, které lze použít k vylepšení proces ladění stránek. V této části budete pro třídu Sys.Debug zavést a najdete v části Jak lze použít v aplikacích.

*Pomocí třídy Sys.Debug*

Třída Sys.Debug (třídu JavaScript nachází v oboru názvů Sys) slouží k provádění několik různých funkcí, včetně zápis výstup trasování provádění kódu kontrolní výrazy a vynucení selhání tak, aby ho můžete ladit kódu. Použije se hojně v knihovně prvku ASP.NET AJAX soubory ladění (ve výchozím nastavení nainstalována v C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0) k provedení (podmíněného testů kontrolní výrazy názvem), ujistěte se, k funkcím, že objekty obsahovat očekávaným objemem dat a k zápisu příkazů trasování jsou správně předány parametry.

Třída Sys.Debug zpřístupňuje několik různé funkce, které lze použít pro zpracování trasování, kontrolní výrazy kódu nebo selhání, jak je znázorněno v tabulce 1.

**Tabulka 1. Funkce tříd Sys.Debug.**

| **Název funkce** | **Popis** |
| --- | --- |
| Assert (podmínky, zprávu, displayCaller) | Vyhodnotí, že parametr podmínky platí. Pokud je podmínka testuje vyhodnocena jako false, okno se zprávou se použije k zobrazení zpráv hodnotu parametru. Pokud má parametr displayCaller hodnotu true, metoda zobrazí také informace o volajícím. |
| clearTrace() | Vymaže příkazy výstup trasování operací. |
| Fail(Message) | Způsobí, že program, který chcete zastavit provádění a rozdělit na ladicího programu. Parametr zpráva slouží k poskytování příčinu selhání. |
| trace(Message) | Zapíše zpráva parametr do výstupu. |
| traceDump (objekt, název) | Výstupy dat objektu v čitelném formátu. Parametr name slouží k poskytování popisku pro výpis trasování. Dílčí objekty v rámci objektu se zálohované se zapíšou ve výchozím nastavení. |

Trasování klienta můžete použít stejným způsobem jako funkce trasování, která je k dispozici v technologii ASP.NET. To umožňuje používat různé zprávy a pokuste se snadno vidět bez přerušení toku aplikace. Výpis 5 ukazuje příklad použití funkce Sys.Debug.trace k zápisu do protokolu trasování. Tato funkce přebírá jednoduše zprávu, která by měly být zapsány jako parametr.

**Výpis 5. Pomocí funkce Sys.Debug.trace.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample6.js)]

Pokud spustíte kód ukazuje výpis 5 neuvidíte žádné výstup trasování na stránce. Jediný způsob, jak ji zobrazit se má používat k dispozici v sadě Visual Studio .NET, pomocná vývoj webových nebo Firebug okna konzoly. Pokud chcete zobrazit výstup trasování na stránce pak bude nutné přidat značku TextArea a dejte mu id TextArea s názvem, jak ukazuje následující:


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample7.html)]

Všechny příkazy Sys.Debug.trace na stránce budou zapisovat do TextArea TextArea s názvem.

V případech, kde chcete zobrazit data obsažená v objektu JSON, můžete použít třídu Sys.Debug traceDump funkce. Tato funkce přebírá dva parametry, včetně objekt, který by měl být zálohované do konzoly trasování a název, který slouží k identifikaci objektů ve výstupu trasování. Výpis 6 ukazuje příklad použití funkce traceDump.

**Výpis 6. Pomocí funkce Sys.Debug.traceDump.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample8.js)]

Obrázek 11 se zobrazuje výstup z volání funkce Sys.Debug.traceDump. Všimněte si, že kromě zápisu dat objekt osoba, také zapíše data adresu sub objektu.

Kromě trasování, třída Sys.Debug lze také provést kontrolní výrazy kódu. Kontrolní výrazy se používají pro testování, zda jsou splněny konkrétní podmínky, když aplikace běží. Ladicí verze skriptů pro knihovny ASP.NET AJAX obsahují několik assert – příkazy pro testování různých podmínek.

Výpis 7 znázorňuje příklad použití funkce Sys.Debug.assert k testování podmínku. Kód testuje, zda objekt adresa má hodnotu null před aktualizací objekt osoba.


[![Výstup Sys.Debug.traceDump funkce.](understanding-asp-net-ajax-debugging-capabilities/_static/image32.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image31.png)

**Obrázek 11**: výstup Sys.Debug.traceDump funkce.  ([Kliknutím zobrazit obrázek v plné velikosti](understanding-asp-net-ajax-debugging-capabilities/_static/image33.png))


**Výpis 7. Pomocí funkce Debug.Assert –.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample9.js)]

Tři parametry jsou předávány včetně podmínky pro vyhodnocení, zprávu, která se zobrazí, pokud kontrolní výraz vrátí hodnotu false a zda má být zobrazena informace o volajícím. V případech, kdy kontrolní výrazy nejsou zpráva se zobrazí a také informace o subjektu volajícím Pokud třetí parametr měl true. Obrázek 12 znázorňuje příklad dialogového okna chyby, který se zobrazí, pokud se nezdaří assertion vidět na výpisu 7.

Poslední funkce tak, aby pokrývalo je Sys.Debug.fail. Pokud chcete vynutit kód selhání na konkrétní řádku ve skriptu můžete přidat volání Sys.Debug.fail spíše než příkaz ladicí program, který je obvykle používaných v aplikacích jazyka JavaScript. Funkce Sys.Debug.fail přijme parametr jednoho řetězce, který představuje příčinu selhání, jak ukazuje následující:


[!code-css[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample10.css)]


[![Chybová zpráva o Sys.Debug.assert.](understanding-asp-net-ajax-debugging-capabilities/_static/image35.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image34.png)

**Obrázek 12**: zpráva o neúspěšném zpracování A Sys.Debug.assert.  ([Kliknutím zobrazit obrázek v plné velikosti](understanding-asp-net-ajax-debugging-capabilities/_static/image36.png))


Pokud příkaz Sys.Debug.fail je došlo při spouští skript, hodnota parametru zpráva se zobrazí v konzole pro ladění aplikace, jako je například Visual Studio 2008 a zobrazí se výzva k ladění aplikace. Jeden případ, kdy to může být velmi užitečné je, když nelze nastavit zarážky s Visual Studio 2008 vloženého skriptu, ale byste chtěli kód na konkrétní řádku zastaví, si můžete prohlédnout hodnotu proměnné.

*Principy vlastnosti ScriptManager ScriptMode ovládacího prvku*

Zahrnuje knihovny ASP.NET AJAX pro ladění a vydání verze skriptu, které jsou nainstalovány na C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 ve výchozím nastavení. Ladění skriptů jsou vhodně formátovaná, snadno čitelný a mít několik volání Sys.Debug.assert na různých místech je při vydání skripty mít prázdné vynechají a používat třídu Sys.Debug pouze minimalizovat jejich celkovou velikost.

Ovládací prvek ScriptManager přidat na stránky ASP.NET AJAX přečte atribut ladění kompilace element v souboru web.config. Chcete-li určit, které verze knihovny skriptů k načtení. Můžete však řídí, zda ladění a vydání skriptů jsou načíst (skripty knihovny nebo vlastní skripty) tak, že změníte vlastnost ScriptMode. ScriptMode přijme ScriptMode výčtu, jejíž členové zahrnují automaticky, ladění, verzi a Inherit.

Výchozí hodnota ScriptMode na hodnotu Auto, což znamená, že prvek ScriptManager zkontroluje atribut ladění v souboru web.config. Při ladění je hodnota false bude prvek ScriptManager načíst verzi skriptů pro knihovny ASP.NET AJAX. Pokud ladění hodnotu true budou načteny ladicí verze skriptů. Změna vlastnosti ScriptMode verzi nebo ladění vynutí ScriptManager načíst odpovídajících skriptů bez ohledu na to, jaká hodnota ladění atribut má v souboru web.config. Výpis 8 ukazuje příklad použití ovládacího prvku ScriptManager načíst ladění skriptů z knihovny ASP.NET AJAX.

**Výpis 8. Načítání ladění skriptů pomocí prvek ScriptManager**.


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample11.aspx)]

Různé verze svých vlastních skriptů (ladění nebo verze) můžete také načíst pomocí vlastnosti skripty prvek ScriptManager společně s komponentu ScriptReference, jak je vidět na výpisu 9.

**Výpis 9. Načítání vlastních skriptů používajících prvek ScriptManager.**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample12.aspx)]

> [!NOTE]
> Pokud načítáte vlastní skripty pomocí součásti ScriptReference musíte prvek ScriptManager upozornit, když skript dokončil načítání přidáním následující kód v dolní části skript:


[!code-csharp[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample13.cs)]

Kód ukazuje výpis 9 informuje ScriptManager hledání ladicí verze skriptu osoba, bude automaticky hledat Person.debug.js místo Person.js. Pokud soubor Person.debug.js nebyla nalezena že chyba, bude vyvolána.

V případech, kdy chcete ladicího nebo prodejní verze nástroje vlastní skript má být načten na základě hodnoty vlastnosti ScriptMode nastavit na ovládací prvek ScriptManager můžete nastavit vlastnost ScriptMode ScriptReference ovládacího prvku na zděděné. To způsobí, že správnou verzi vlastní skript má být načten podle prvek ScriptManager ScriptMode vlastnost jak je vidět na výpisu 10. Protože vlastnost ScriptMode ovládací prvek ScriptManager je nastavena na ladění, skript Person.debug.js se načíst a použije na stránce.

**Výpis 10. ScriptMode, která dědí z ScriptManager pro vlastní skripty.**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample14.aspx)]

Pomocí vlastnosti ScriptMode správně můžou snadněji ladit aplikace a zjednodušit celý proces. Jednotlivé kroky a číst od formátování kódu byl odebrán při ladění skriptů jsou formátovány speciálně pro účely ladění místo obtížně skripty verzi knihovny ASP.NET AJAX.

## <a name="conclusion"></a>Závěr

Technologie ASP.NET AJAX společnosti Microsoft poskytuje sice solidní základ pro vytváření aplikací s povolenými AJAX, která můžou zvýšit celkový dojem koncového uživatele. Však jako každá programování technologie, chyby a další aplikace problémy určitě vyvstanou. Zároveň budete vědět o různých možnostech ladění dostupných můžete uložit spoustu času a výsledek v stabilnější produktu.

V tomto článku jste si úvod do několika různých technik pro stránky ASP.NET AJAX, včetně aplikace Internet Explorer s Visual Studio 2008, vývoj webové pomocné rutiny a Firebug ladění. Tyto nástroje můžete zjednodušit proces celkové ladění, protože můžete používat proměnné datové, provede řádky kódu a zobrazení příkazů trasování. Kromě ladění různých nástrojů popsané také viděli použití třídy Sys.Debug knihovny ASP.NET AJAX v aplikaci a použití třídy ScriptManager načíst ladění a vydání verze skriptů.

## <a name="bio"></a>Bio

Dana Wahlin (Microsoft nejvíc hodí v situaci Professional pro technologii ASP.NET a webové služby XML) je rozhraní .NET development lektorem a architektura konzultantem na technické školení rozhraní ([www.interfacett.com)](http://www.interfacett.com). Dana založena XML pro web ASP.NET s vývojáři ([www.XMLforASP.NET](http://www.XMLforASP.NET)), je na INETA mluvčího kancelář a mluví na několik konferencí. Dana společně vytvořené Professional Windows DNA (Wrox), ASP.NET: tipy, kurzy a kód (Edice nakladatelství Sams), ASP.NET 1.1 vnitřních řešení, Professional ASP.NET 2.0 AJAX (Wrox), útoky zvenku MVP ASP.NET 2.0 a vytvořené XML pro vývojáře využívající technologii ASP.NET (Edice nakladatelství Sams). Při mu není psaní kódu, články nebo knihy, Dana požívá zápis a Hudba záznam a přehrávání golf a Basketbalový s jeho manželku a dětí.

Scott Dikovat pracuje s technologií Microsoft Web od 1997 a je ředitel myKB.com ([www.myKB.com](http://www.myKB.com)) kde mu se specializuje na psaní ASP.NET na základě aplikací, které jsou zaměřené na řešení softwaru znalostní báze Knowledge Base. Scott nelze kontaktovat prostřednictvím e-mailu v [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) nebo jeho blog na [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Předchozí](understanding-asp-net-ajax-web-services.md)
