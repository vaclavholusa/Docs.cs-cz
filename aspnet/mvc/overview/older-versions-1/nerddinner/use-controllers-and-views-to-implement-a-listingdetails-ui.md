---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: Použití Kontrolerů a zobrazení k implementaci uživatelského rozhraní seznamu a podrobností | Dokumentace Microsoftu
author: microsoft
description: Krok 4 ukazuje, jak přidat řadič aplikace, která využívá výhod náš model uživatelům poskytnout data seznamu a podrobností navigaci...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: 4fe065e29950a076de07d73205a97399f82f07d6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377873"
---
<a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a>Použití Kontrolerů a zobrazení k implementaci uživatelského rozhraní seznamu a podrobností
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Toto je krok 4 bezplatného [kurz vývoje aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , který procházení procházení po tom, jak sestavit malý, ale bylo možné provést, webové aplikace pomocí ASP.NET MVC 1.
> 
> Krok 4 ukazuje, jak přidat do aplikace, která využívá výhod náš model uživatelům poskytnout data seznamu a podrobností navigaci večeří na našem webu NerdDinner Kontroleru.
> 
> Pokud používáte ASP.NET MVC 3, doporučujeme je provést [získávání začít s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.


## <a name="nerddinner-step-4-controllers-and-views"></a>NerdDinner krok 4: Kontrolerů a zobrazení

S tradičními webovými rozhraními (klasické rozhraní ASP, PHP, webových formulářů ASP.NET atd.) se obvykle mapují příchozí adresy URL na soubory na disku. Příklad: požadavek na adresu URL, jako je "/ Products.aspx" nebo "/ Products.php" může být zpracována "Products.aspx" nebo "Products.php" soubor.

Webové rozhraní MVC mapování adres URL do kódu serveru mírně odlišným způsobem. Místo souborů mapování příchozích adres URL, že místo toho mapování adres URL na metody třídy. Tyto třídy se nazývají "Řadiče" a zodpovídají za zpracování příchozích požadavků HTTP, zpracování uživatelského vstupu, načítání a ukládání dat a určení odpověď k odeslání zpátky do klienta (zobrazení HTML, stáhněte si soubor, přesměrovat na jinou Adresa URL atd.).

Teď, když jsme vytvořili základní model pro naši aplikaci NerdDinner, naším dalším krokem bude přidání Kontroleru aplikace, která využívá výhod ji uživatelům poskytnout data seznamu a podrobností navigaci večeří na našem webu.

### <a name="adding-a-dinnerscontroller-controller"></a>Přidání Kontroleru DinnersController

Použijeme začněte tím, že pravým tlačítkem na složku "Řadiče" v rámci naší webový projekt a pak vyberte **Add -&gt;řadič** příkazu nabídky (Tento příkaz můžete spustit také tak, že zadáte Ctrl-M, Ctrl-C):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

Tím se otevře dialogové okno "Přidat kontroler":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

Jsme budete pojmenujte nový kontroler "DinnersController" a klikněte na tlačítko "Přidat". Visual Studio se potom přidejte soubor DinnersController.cs v našich \Controllers adresáři:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

Se rovněž otevře novou třídu DinnersController v editoru kódu.

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a>Přidat do třídy DinnersController Index() a Details() metody akce

Chceme, aby návštěvníkům používajícím naší aplikace, projděte si seznam nadcházejících večeří a zajistí, aby kliknutím na libovolný web Dinner v seznamu zobrazíte podrobnosti o tom. Provedeme následující adresy URL z naší aplikace pro publikování:

| **ADRESA URL** | **Účel** |
| --- | --- |
| */Dinners/* | Zobrazení HTML seznam nadcházejících večeří |
| */Dinners/podrobnosti / [id]* | Zobrazit podrobnosti o konkrétní společnosti dinner indikován parametr "id" vloženým do adresy URL, – které bude odpovídat DinnerID večeře v databázi. Příklad: /Dinners/Details/2 zobrazí stránku HTML s podrobnostmi o Dinner, jehož hodnota DinnerID je 2. |

Počáteční implementace z těchto adres URL budeme publikovat do přidáním dvě veřejné "metody akce" pro naše DinnersController třídy níže:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

Provozujeme budete aplikace NerdDinner a můžete je vyvolat prohlížeče. Zadávání *"/ večeří /"* způsobí, že adresa URL naše *Index()* metodu spustit a odešle zpět odpověď na následující:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

Zadávání *"/ večeří/podrobnosti/2"* způsobí, že adresa URL naše *Details()* metodu spustit a jejich odeslání zpět odpověď na následující:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

Možná se ptáte – jak ASP.NET MVC věděli k vytvoření naší DinnersController třídy a volat tyto metody? Si uvědomit, že můžeme využít rychlé podívat, jak směrování funguje.

### <a name="understanding-aspnet-mvc-routing"></a>Principy ASP.NET MVC směrovací

ASP.NET MVC zahrnuje výkonné adresy URL směrování modul, který poskytuje značnou flexibilitu při řízení způsobu mapování adres URL na třídy kontroleru. To umožňuje zcela přizpůsobit, jak ASP.NET MVC zvolí které kontroleru třídy za účelem vytvoření, která metoda k vyvolání na něm také nakonfigurovat různými způsoby, že proměnné lze automaticky získá analýzou z adresy URL řetězec dotazu a předá metodě jako parametr argumenty. Poskytuje flexibilitu při zcela optimalizovat web pro SEO (optimalizace pro vyhledávací weby) a také publikovat všechny Struktura adresy URL, kterou chceme, aby z aplikace.

Ve výchozím nastavení obsahuje nové projekty ASP.NET MVC předkonfigurované sadu pravidly směrování adres URL je už zaregistrovaný. Umožňuje nám to snadno začít používat aplikaci bez nutnosti explicitně nic konfigurovat. Výchozí směrování pravidla registrace najdete v rámci třídy "Aplikace" z našich projektů – které můžeme lze otevřít dvojitým kliknutím na soubor "Global.asax" v kořenovém adresáři projektu:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

Výchozí pravidla směrování ASP.NET MVC jsou registrovány v rámci metody "RegisterRoutes" této třídy:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

"Trasy. MapRoute() "volání metody výše zaregistruje výchozí pravidlo směrování, která se mapuje příchozí adresy URL pomocí formátu adresy URL třídy kontroleru:" / {controller} / {action} / {id} "–"controller"je název třídy kontroleru pro vytvoření instance,"action"je název veřejná metoda k vyvolání na ně a "id" je volitelný parametr vkládán adresu URL, kterou lze předat jako argument metody. Třetí parametr předaný voláním metody "MapRoute()" je sada výchozích hodnot v případě, že se nenachází v adrese URL určený pro kontroler nebo akce/id hodnoty (kontroler = "Domů" Action = "Index", Id = "").

Níže je tabulka, která ukazuje, jak různé adresy URL se mapují pomocí výchozí "<em>/ {řadiče} / {action} / {id}"</em>trasy pravidlo:

| **ADRESA URL** | **Třída kontroleru** | **Metody akce** | **Parametry předané** |
| --- | --- | --- | --- |
| */ Večeří/podrobnosti/2* | DinnersController | Details(ID) | id=2 |
| */ Večeří/Edit/5* | DinnersController | Edit(ID) | id=5 |
| */ Večeří/vytvoření* | DinnersController | Create() | Není k dispozici |
| */ Večeří* | DinnersController | Index() | Není k dispozici |
| *Domů* | HomeController | Index() | Není k dispozici |
| */* | HomeController | Index() | Není k dispozici |

Poslední tři řádky zobrazit výchozí hodnoty (kontroler = Home, akce = Index, Id = "") se používají. Protože metoda "Index" je zaregistrovaný jako výchozí název akce, pokud není zadaná, "/ večeří" a "/ Home" příčina adresy URL Index() metoda akce má být volána na jejich třídy Kontroleru. Protože kontroler "Home" je zaregistrovaný jako výchozí kontroler neuvede, adresa URL "/" způsobí, že HomeController, který se má vytvořit a metody akce Index() na něj má být volána.

Pokud se vám tato pravidla směrování výchozí adresy URL, dobrou zprávou je, že jsou snadno změnit – stačí upravovat v rámci výše uvedené RegisterRoutes metody. Pro naši aplikaci NerdDinner ale nebudeme změnit pravidla směrování výchozí adresy URL – místo toho právě použijeme jako-je.

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a>Pomocí DinnerRepository z našich DinnersController

Pojďme nyní nahraďte naše aktuální implementace DinnersController Index() a Details() metody akce pomocí implementace, které používají náš model.

Použijeme třídu DinnerRepository jsme vytvořili dříve k implementaci chování. Vytvoříme začít tak, že přidáte "pomocí" příkaz, který odkazuje na obor názvů "NerdDinner.Models" a potom deklarovat instanci naše DinnerRepository jako pole na naše DinnerController třídy.

Dále v této kapitole vytvoříme přinášejí koncept "Injektáž závislostí" a zobrazit další způsob pro naše řadiče k získání odkazu na DinnerRepository, umožňující lepší jednotky testování – ale právo teď vytvoříme pouze instance naší DinnerRepository vložené jako níže.

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

Nyní jsme připraveni ke generování odpovědi HTML pomocí našich objekty modelu načtená data.

### <a name="using-views-with-our-controller"></a>Pomocí zobrazení Kontroleru

I když je možné napsat kód v rámci naší metody akce k sestavení HTML a následné použití *metody Response.Write()* Pomocná metoda ji odeslat zpět klientovi, že přístup nepraktický poměrně rychle. Mnoho lepším řešením je pro nás provádět jenom aplikace a data logika uvnitř naše DinnersController metody akce a pak předejte data potřebná k vykreslení odpovědi HTML na samostatné "Zobrazit" šablonu, která je zodpovědná za výstupu HTML reprezentace jeho. Jak uvidíme ve chvíli, šablona "Zobrazit" je textový soubor, který obvykle obsahuje kombinaci kódu HTML a vykreslování vložený kód.

Oddělení naše logice kontroleru z našich vykreslování zobrazení přináší několik velké výhody. Zejména pomáhá vynucovat jasně "oddělené oblasti zájmu" mezi kódu aplikace a kód pro formátování/vykreslování uživatelského rozhraní. Díky tomu je mnohem jednodušší pro testování částí aplikace logiky v izolaci z logiku pro vykreslení uživatelského rozhraní. To usnadňuje později upravit šablony vykreslení uživatelské rozhraní bez nutnosti provádět změny kódu aplikace. A to může usnadnit práci vývojářům a návrhářům spolupráce na projektech.

Aktualizujeme Naše třída DinnersController k označení, že má být šablona zobrazení má být zaslán zpět při reakci na uživatelském rozhraní HTML změnou signatury metody naše dvě akce s návratovým typem "void". místo toho mít typ vrácené hodnoty "ActionResult". Můžete pak volat *View()* pomocnou metodu v základní třídě Kontroleru vrátit zpět objekt "ViewResult" podobná níže uvedenému příkladu:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

Podpis metody *View()* Pomocná metoda používáme nad vypadá níže:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

První parametr *View()* Pomocná metoda je název souboru šablony zobrazení chceme použít k vykreslení odpovědi HTML. Druhý parametr je objekt modelu, který obsahuje data, která šablona zobrazení potřebuje k vykreslení odpovědi HTML.

V metodě akce Index() voláme *View()* pomocnou metodu a označením, že má být k vykreslení HTML seznam večeří pomocí zobrazení šablony "Index". Zobrazit šablonu jsme je úspěšných sekvenci objektů Dinner vytvořit seznam od:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

V metodě akce Details() jsme pokoušejí o načtení objektu Dinner pomocí zadané v rámci URL id. Pokud se najde platný Dinner říkáme *View()* Pomocná metoda označující chceme použít šablonu "Podrobnosti" zobrazení k vykreslení objektu načtený Dinner. Pokud o to požádá neplatný dinner jsme vykreslení užitečné chybovou zprávu, která označuje, že večeře neexistuje pomocí zobrazení šablony "Serveru" (a přetížené verze *View()* Pomocná metoda, která trvá to jenom název šablony ):

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

Teď můžeme implementovat zobrazit šablony "Serveru", "Details" a "Index".

### <a name="implementing-the-notfound-view-template"></a>Implementace "Serveru" Zobrazit šablonu

Začneme budete implementací zobrazit šablonu – které zobrazí se popisná chybová zpráva, nebyl nalezen požadovaný dinner "Serveru".

Vytvoříme novou šablonu zobrazení umístěním kurzoru náš text v rámci metody akce kontroleru a potom klikněte pravým tlačítkem myši a zvolte příkaz "Přidat zobrazení" (jsme můžete také spustit tento příkaz zadáním kombinace kláves Ctrl-M, Ctrl-V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

Tím se otevře dialog "Přidat zobrazení" jako níže. Ve výchozím nastavení dialogové okno se vyplní předem název zobrazení, vytvořit tak, aby odpovídaly názvu metody akce kurzor byla ve chvíli dialogového okna se spustila (v tomto případě "Details"). Protože chceme první implementaci "Serveru" šablony, vytvoříme tento název zobrazení přepsání a nastavit tak, aby se místo toho "Serveru":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

Když kliknete na tlačítko "Přidat", Visual Studio vytvořit novou šablonu zobrazení "NotFound.aspx" nám v adresáři "\Views\Dinners" (který také se vytvoří, pokud již neexistuje adresář):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

Otevře se také si naše nové šablony "NotFound.aspx" zobrazení v editoru kódu:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

Zobrazení šablony ve výchozím nastavení mají dvě "oblasti obsahu" kde můžeme přidat obsah a kód. První umožňuje přizpůsobit "název" HTML stránka zaslal zpět. Druhý umožňuje přizpůsobit "hlavní obsah" HTML stránka zaslal zpět.

K implementaci naše "Serveru" Zobrazit šablonu přidáme některé základní obsah:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

My potom ho můžou vyzkoušet v prohlížeči. K tomu můžeme požadavek *"/ večeří/podrobnosti/9999"* adresy URL. To bude odkazovat na večeři, která aktuálně neexistuje v databázi a způsobí, že naše DinnersController.Details() metody akce k vykreslení zobrazení šablony naše "Serveru":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

Jednou z věcí, které uvidíte ve snímku obrazovky výše je, že upravíme šablonu základním zobrazení zdědil spoustu kód HTML, který obklopuje hlavní obsah na obrazovce. Je to proto, že upravíme šablonu zobrazení používá "stránka předlohy" šablonu, která umožňuje nám to konzistentního rozložení můžete použít ve všech zobrazeních v lokalitě. Podíváme se, jak fungují více v pozdější části tohoto kurzu stránky předlohy.

### <a name="implementing-the-details-view-template"></a>Implementace "Details" Zobrazit šablonu

Teď můžeme implementovat "Details" Zobrazit šablonu – které se generují kód HTML pro jeden model Dinner.

Jsme budete k tomu umístění kurzoru náš text v rámci podrobnosti o metodě akce a potom klikněte pravým tlačítkem myši a zvolte příkaz "Přidat zobrazení" (nebo stiskněte klávesy Ctrl-M, Ctrl-V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

Tím se otevře dialogové okno "Přidat zobrazení". Výchozí název zobrazení ("o") budeme uchovávat. Také jsme budete v dialogovém okně vyberte zaškrtávací políčko "Vytvořit zobrazení se silnými typy" a vyberte (pomocí rozevíracího seznamu pole se seznamem) název typu modelu, který jsme jsou předány z Kontroleru zobrazení. Pro toto zobrazení jsme prochází Dinner objektu (plně kvalifikovaný název pro tento typ je: "NerdDinner.Models.Dinner"):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

Na rozdíl od předchozích šablony, ve kterém jsme se rozhodli vytvořit "Prázdné zobrazení", tentokrát, která jsme se vyberou automaticky "generování uživatelského rozhraní" zobrazení pomocí šablony "Podrobnosti". Můžeme to lze naznačit to změnou na "Zobrazit obsah" rozevírací seznam v dialogovém okně výše.

"Generování uživatelského rozhraní" vygeneruje počáteční implementace naše podrobnosti zobrazení šablony založené na večeři objekt, který jsme do něj prochází. To poskytuje snadný způsob, abychom mohli rychle začít používat naše implementace zobrazení šablony.

Když kliknete na tlačítko "Přidat", Visual Studio vytvoří nový soubor šablony zobrazení "Details.aspx" pro nás v rámci naší "\Views\Dinners" adresář:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

Otevře se také si naše nové šablony "Details.aspx" zobrazení v editoru kódu. Bude obsahovat implementace počáteční vygenerované uživatelské rozhraní zobrazení podrobností na základě modelu večeři. Modul generování uživatelského rozhraní používá reflexe .NET se podívat na veřejné vlastnosti, které jsou zveřejněné na třídy je předán a přidá na základě pro každý typ, který najde odpovídající obsahu:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

Můžeme požádat o *"/ večeří/podrobnosti/1"* adresy URL vypadá implementovaný "Podrobnosti" vygenerované uživatelské rozhraní v prohlížeči. Pomocí této adresy URL se zobrazí jeden večeří, kterou jsme ručně přidali do databáze při jsme prvním vytvoření:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

To nám získá rychlým seznámením a spuštěním a poskytuje nám počáteční implementace naše Details.aspx zobrazení. Pak můžeme přejít a upravit ji přizpůsobit uživatelské rozhraní pro naše spokojenost.

Když podrobněji podíváme na šabloně Details.aspx, jsme zjistíte, že ji obsahuje statický kód HTML a také vložený kód pro vykreslování. &lt;%%&gt; útržky kódu spuštění kódu při vykreslení zobrazení šablony a &lt;% = %&gt; útržky kódu spouštění kódu vytvořeného v nich obsažené a potom vykreslit výsledek do výstupního datového proudu šablony.

Nám můžete napsat kód v rámci naší zobrazení přistupující objekt modelu "Dinner", který byl předán z kontroleru pomocí vlastnosti "Model" silného typu. Visual Studio poskytuje úplné funkce intellisense kód při přístupu k této vlastnosti "Vzor" v editoru:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

Tak, aby zdroj pro naše finální šablona zobrazení podrobností vypadá níže vytvoříme několik vylepšení:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

Při přístupu k *"/ večeří/podrobnosti/1"* bude adresa URL znovu ji nyní vykreslení podobná níže uvedenému příkladu:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a>Implementace "Index" Zobrazit šablonu

Teď můžeme implementovat "Index" Zobrazit šablonu – který vygeneruje seznam nadcházejících večeří. Úkol to vytvoříme umístění kurzoru náš text v rámci metody akce indexu a potom klikněte pravým tlačítkem myši klikněte na tlačítko a zvolte příkaz "Přidat zobrazení" (nebo stiskněte klávesy Ctrl-M, Ctrl-V).

V dialogovém okně "Přidat zobrazení" vytvoříme zachovat zobrazení šablony s názvem "Index" a vyberte zaškrtávací políčko "Vytvořit zobrazení se silnými typy". Tentokrát vybereme možnost automaticky vygenerovat šablonu zobrazení "seznamu List", a vyberte "NerdDinner.Models.Dinner" jako typ modelu předána do zobrazení (které vzhledem k tomu, že jsme uvedli vytváříme "Seznam" způsobí, že vygenerované uživatelské rozhraní předpokládat, že jsme dialogové okno Přidat zobrazení předání pouze sekvenci objektů Dinner z Kontroleru zobrazení):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

Když kliknete na tlačítko "Přidat", Visual Studio vytvoří nový soubor šablony zobrazení "Index.aspx" pro nás v rámci naší "\Views\Dinners" adresáře. To bude "generování uživatelského rozhraní" počáteční implementace v rámci, která poskytuje výpisu tabulky HTML večeří jsme předána do zobrazení.

Pokud provozujeme aplikace a přístup *"/ večeří /"* adresy URL se bude vykreslovat náš seznam večeří takto:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

Výše uvedené tabulce řešení získáváme mřížky rozložení částí produktu naše data Dinner – které není úplně chceme pro naše spotřebitelům Dinner výpis. Budeme aktualizovat Index.aspx zobrazit šablonu a upravte je tak seznam méně sloupců dat a použít &lt;ul&gt; – element pro vykreslení namísto tabulky pomocí níže uvedeného kódu:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

Jak jsme ve smyčce každý večeře v náš Model se používá – klíčové slovo "var" v rámci výše uvedeného příkazu foreach. Tyto zkušenosti s C# 3.0 možná myslíte, že použití "var" znamená, že večeře objektu s pozdní vazbou. Místo toho znamená, že kompilátor používá odvození typu proti vlastnost silného typu "Vzor" (která je typu "IEnumerable&lt;Dinner&gt;") a kompilace proměnnou místní "dinner" jako typ web Dinner – což znamená, že jsme získali úplný technologie IntelliSense a kompilace kontrolují v rámci bloků kódu:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

Když jsme stiskněte tlačítko Aktualizovat */Dinners* adresu URL do prohlížeče naše aktualizované zobrazení teď vypadá podobně jako následující:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

To je o něco lépe – ale ještě není úplně existuje. Naše posledním krokem je povolit koncovým uživatelům, aby kliknutím na jednotlivé večeří v seznamu a zobrazte podrobnosti o nich. To jsme budete implementovat podle vykreslení elementů HTML hypertextový odkaz, odkaz na metodu akce podrobnosti v našich DinnersController.

Vygenerujeme můžete tyto hypertextové odkazy v rámci naší Index zobrazení v jednom ze dvou způsobů. První je ruční vytvoření HTML &lt;&gt; prvky, jako jsou níže, kde zahrnujeme &lt;%%&gt; blokuje v rámci &lt;&gt; prvek HTML:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

Alternativním přístupem můžete je využít integrované Pomocná metoda "Html.ActionLink()" v rámci technologie ASP.NET MVC, která podporuje programové vytvoření HTML &lt;&gt; element, který odkazuje na jinou metodu akce v Kontroler:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

První parametr metody helper Html.ActionLink() je text odkazu pro zobrazení (v tomto případě název ze společnosti dinner), druhý parametr je název akce Kontroleru, chceme, aby ke generování odkazu na (v tomto případě metodu podrobnosti) a třetí parametr je Sada parametry se mají odeslat na akci (implementován jako anonymní typ s názvem/hodnotami vlastností). V tomto případě zadáváme parametru "id" dinner jsme chcete propojit a vzhledem k tomu, že výchozí směrování adres URL v architektuře ASP.NET MVC pravidlo "{Controller} / {Action} / {id}" Pomocná metoda Html.ActionLink() vygeneruje následující výstup:

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

Pro naše zobrazení Index.aspx použijeme Html.ActionLink() Pomocná metoda přístup a mít každý večeře v seznamu odkazu na adresu URL odpovídající podrobnosti:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

Kliknu na nyní Když jsme */Dinners* adresa URL vypadá naše společnost dinner seznam níže:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

Když kliknete na některý z večeří v seznamu přejdeme budete zobrazíte podrobnosti o něm:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a>Podle konvence pojmenování a strukturu adresářů \Views

Ve výchozím nastavení aplikace ASP.NET MVC pomocí adresáře podle úmluvy strukturu pojmenování při překladu zobrazit šablony. To umožňuje vývojářům vyhnout se tak nutnosti plnému cesta k umístění při odkazování na zobrazení z v rámci třídy Kontroleru. Ve výchozím nastavení bude vypadat ASP.NET MVC pro zobrazení souboru šablony v rámci * \Views\[ControllerName]\* adresáře pod aplikace.

Například jsme pracovali na třídě DinnersController – který explicitně odkazuje tři zobrazení šablony: "Index", "Details" a "Serveru". ASP.NET MVC ve výchozím nastavení vyhledá tato zobrazení v rámci *\Views\Dinners* adresáře pod naše kořenový adresář aplikace:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

Všimněte si, že nad jak Zde jsou nyní tři třídy kontroleru v rámci projektu (DinnersController, HomeController a AccountController – poslední dva byly přidány ve výchozím nastavení, když jsme vytvořili projekt), a existují tři dílčí adresáře (jeden pro každý kontroler) v rámci \Views adresáře.

Zobrazení na něj odkazovat z řadiče domovské a účty automaticky vyřeší jejich zobrazení šablony z příslušné *\Views\Home* a *\Views\Account* adresáře. *\Views\Shared* podadresář poskytuje způsob, jak ukládat zobrazit šablony, které jsou znovu použít v rámci několika řadičů v rámci aplikace. Když ASP.NET MVC se pokusí přeložit zobrazit šablonu, nejprve zkontroluje v rámci *\Views\[kontroler]* konkrétní adresář, a pokud nemůže najít zobrazit šablonu existuje bude vypadat v rámci *\Views\ Sdílené* adresáře.

Pokud jde o názvy šablon jednotlivých zobrazení, je doporučené pokyny k zobrazení šablony mají stejný název jako metody akce, který způsobil, že k vykreslení. Například výše naše "Index" je metodu akce pomocí "Index" zobrazení k vykreslení zobrazení výsledků a metoda akce "Details" je pomocí "Details" zobrazení k vykreslení jeho výsledky. To usnadňuje se krátce zobrazit šablonu, která souvisí s každou akci.

Vývojáři, není potřeba explicitně zadat název šablony zobrazení při zobrazení šablony má stejný název jako metoda akce volaná na řadiči. Jsme můžete místo toho stačí pouze předat objekt modelu "View()" pomocné metody (bez zadání názvu zobrazení) a ASP.NET MVC automaticky odvodí, že chceme použít *\Views\[ControllerName]\[ActionName]* zobrazit šablonu na disku pro vykreslení.

To umožňuje nám to trochu vyčistit našeho kódu kontroleru a předchází vzniku duplicitní název dvakrát v našem kódu:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

Ve výše uvedeném kódu je, že všechny, které je potřeba k implementaci nice seznamu a podrobností Dinner prostředí pro web.

#### <a name="next-step"></a>Dalším krokem

Teď máme dobré Dinner integrované prostředí pro procházení.

Pojďme teď povolit podporu editaci formulář dat CRUD (vytváření, čtení, Update, Delete).

> [!div class="step-by-step"]
> [Předchozí](build-a-model-with-business-rule-validations.md)
> [další](provide-crud-create-read-update-delete-data-form-entry-support.md)
