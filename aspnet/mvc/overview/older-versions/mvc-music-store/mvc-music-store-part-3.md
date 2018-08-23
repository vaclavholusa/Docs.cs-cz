---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: '3. část: Zobrazení a modely ViewModels | Dokumentace Microsoftu'
author: jongalloway
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store. 3. část popisuje zobrazení a modely ViewModel.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 828ff18abcc5932f82be71a45ebde589eeb051fa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752404"
---
<a name="part-3-views-and-viewmodels"></a>3. část: Zobrazení a modely ViewModels
====================
podle [Jon Galloway](https://github.com/jongalloway)

> MVC Music Store jde o kurz, který se seznámíte, podrobné postupy pro vývoj pro web pomocí ASP.NET MVC a sady Visual Studio.  
>   
> Music Store MVC je jednoduché ukázku implementace úložiště prodává hudebních alb online, který implementuje správu základního webu, přihlášení uživatele a nákupního košíku funkce.  
>   
> V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store. 3. část popisuje zobrazení a modely ViewModel.


Zatím jsme jste právě byla vracení řetězců z akce kontroleru. To je dobrý způsob, jak získat představu o fungování řadiče, ale není jak byste k sestavení aplikace skutečný webu. Budeme má lepší způsob, jak generují kód HTML zpět do prohlížečů navštěvující náš web – jeden kde soubory šablony můžete použít k více snadno přizpůsobit obsah HTML odeslání zpět. To je přesně co dělat zobrazení.

## <a name="adding-a-view-template"></a>Přidání zobrazení šablony

Použít šablonu zobrazení, jsme budete změnit metodu HomeController Index vrátit třídu ActionResult a jej vrátit View(), jako je následující:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

Výše uvedené změny označuje, že místo vrátil řetězec, chceme místo toho použít k získání výsledku zpět "Zobrazit".

Teď přidáme vhodnou šablonu zobrazení na našem projektu. Provedete to tak jsme budete umístit textový kurzor v rámci metody akce indexu, pak klikněte pravým tlačítkem a vyberte "Přidat zobrazení". Tím se otevře dialogové okno Přidat zobrazení:

![](mvc-music-store-part-3/_static/image1.jpg)![](mvc-music-store-part-3/_static/image1.png)

Toto dialogové okno "Přidat zobrazení" umožňuje nám to rychle a snadno generovat soubory šablon zobrazení. Ve výchozím nastavení "Přidat zobrazení" dialogové okno předem vyplní název zobrazení šablony vytvořit tak, aby odpovídalo metodě akce, která bude používat. Protože jsme použili v rámci metody akce Index() naše HomeController místní nabídce "Přidat zobrazení", "Přidat zobrazení" dialog výše má "Index" jako název zobrazení předem vyplní ve výchozím nastavení. Není třeba změnit možnosti v tomto dialogovém okně, proto klikněte na tlačítko Přidat.

Když kliknete na tlačítko Přidat, vytvoří Visual Web Developer nové Index.cshtml zobrazit šablonu pro nás v adresáři \Views\Home složku vytvoříte, pokud ještě neexistuje.

![](mvc-music-store-part-3/_static/image2.png)

Název a složku umístění souboru "Index.cshtml" je důležité a dodržuje zásady vytváření názvů výchozí rozhraní ASP.NET MVC. Název adresáře, \Views\Home, odpovídá kontroler – který se nazývá HomeController. Název šablony zobrazení, Index, odpovídá metodu akce kontroleru, který se zobrazuje zobrazení.

ASP.NET MVC umožňuje vyhnout se tak nutnosti explicitně zadat název nebo umístění šablony zobrazení, pokud použijeme tyto zásady vytváření názvů pro vrátit zobrazení. Zobrazí ve výchozím nastavení se pak \Views\Home\Index.cshtml zobrazit šablonu při psaní kódu jako níže v rámci naší HomeController:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

Visual Web Developer a otevře šablonu zobrazení "Index.cshtml" poté, co jsme kliknutí na tlačítko "Přidat" v dialogovém okně "Přidat zobrazení". Obsah Index.cshtml jsou uvedeny níže.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

Toto zobrazení je pomocí syntaxe Razor, což je stručnější než webových formulářů zobrazovací modul používá v webové formuláře ASP.NET a předchozích verzích rozhraní ASP.NET MVC. Webové formuláře zobrazovací modul je stále k dispozici v architektuře ASP.NET MVC 3, ale celá řada vývojářů považuje zobrazovací modul Razor velice dobře zapadá vývoj pro ASP.NET MVC.

První tři řádky nastavit nadpis stránky pomocí ViewBag.Title. Vytvoříme podívat na fungování tohoto podrobněji brzy, ale nejprve můžeme aktualizace textu v nadpisu textu a zobrazení stránky. Aktualizace &lt;h2&gt; značky říct "Toto je Home Page", jak je znázorněno níže.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

Spuštění aplikace ukazuje, že náš nový text zobrazený na domovské stránce.

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a>Použití rozložení společné prvky webu

Většina webů mají obsah, který je sdílen mezi mnoho stránek: navigace zápatí, loga, odkazy na šablony stylů, atd. Zobrazovací modul Razor umožňuje jednoduše spravovat pomocí stránku s názvem \_Layout.cshtml automaticky vytvořené pro nás ve složce/zobrazení/Shared.

![](mvc-music-store-part-3/_static/image4.png)

Dvakrát klikněte na tuto složku k zobrazení obsahu, které jsou uvedeny níže.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

Obsah z našich jednotlivá zobrazení se zobrazí podle @RenderBodypříkazu () a společný obsah, který chceme nacházet mimo, který lze přidat do \_Layout.cshtml značek. Chceme vám naše MVC Music Store mít společné hlavičky s odkazy na naše oblast domovské stránky a Store na všech stránkách v lokalitě, tak, které přidáme do šablony přímo nad tím @RenderBody– příkaz ().

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a>Aktualizuje se šablona stylů

Šablonu prázdného projektu obsahuje velmi zjednodušený soubor šablon stylů CSS, který obsahuje pouze styly slouží k zobrazení ověřovacích zpráv. Naše návrháře poskytuje některé další šablony stylů CSS a bitové kopie k definování vzhledu a chování našich webu, tak těch v teď přidáme.

Aktualizovaný soubor CSS a image jsou součástí obsahu adresáře MvcMusicStore Assets.zip, která je k dispozici v [MVC. Music Store](https://github.com/evilDave/MVC-Music-Store). Budete výběru obou z nich v Průzkumníku Windows a umístit je do složky obsahu naše řešení v aplikaci Visual Web Developer, jak je znázorněno níže:

![](mvc-music-store-part-3/_static/image5.png)

Budete vyzváni k potvrzení, pokud chcete přepsat existující soubor Site.css. Klikněte na tlačítko Ano.

![](mvc-music-store-part-3/_static/image6.png)

Obsah složky vaší aplikace se teď budou zobrazovat následujícím způsobem:

![](mvc-music-store-part-3/_static/image7.png)

Teď naklonujeme aplikaci spustit a prohlédnout naše změny na domovské stránce.

![](mvc-music-store-part-3/_static/image8.png)

- Pojďme se podívat, co se změnilo: metody akce indexu The HomeController najít a zobrazit šablonu \Views\Home\Index.cshtmlView i v případě, že náš kód volat "návratový View()", protože naše zobrazit šablonu a potom standardní zásady vytváření názvů.
- Na domovské stránce se zobrazuje jednoduché zobrazení uvítací zprávy, která je definována v rámci šablony \Views\Home\Index.cshtml zobrazení.
- Domovská stránka používá naše \_Layout.cshtml šablony, a proto se uvítací zpráva je obsažena v rozložení standardní webu ve formátu HTML.

## <a name="using-a-model-to-pass-information-to-our-view"></a>Použití modelu k předávání informací do našich zobrazení

Zobrazit šablonu, která se zobrazí pouze pevně zakódované HTML není vytvořím velmi zajímavé webovou stránku. K vytvoření dynamické webové stránky, bude místo toho chcete předávání informací do našich zobrazit šablony z našich akce kontroleru.

V modelu Model-View-Controller termín Model odkazuje na objekty, které představují data v aplikaci. Často objekty modelu odpovídají tabulek v databázi, ale nemusí to tak být.

Metody akce kontroleru, které vracejí třídu ActionResult, můžete předat objekt modelu do zobrazení. To umožňuje Kontroleru čistě zabalili všechny informace potřebné k vygenerování odpovědí a potom předal tyto informace zobrazit šablonu pro generování odpovídající odpověď ve formátu HTML. Toto je nejjednodušší k seznámení s tím, že zobrazíte v akci, takže můžeme začít.

Nejprve vytvoříme některé třídy modelu k reprezentaci žánry a alb v našem úložišti. Začněme vytvořením žánr třídy. Klikněte pravým tlačítkem na složku "Modely" v rámci projektu, zvolte možnost "Přidat třídu" a název souboru "Genre.cs".

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

Pak přidejte řetězec veřejnou vlastnost Name do třídy, který byl vytvořen:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

*Poznámka: V případě vás zajímá {get; nastavit;} provádí zápis používání automaticky implementovaných vlastností funkce C#. Tento produkt nám nabízí výhody vlastnost bez nutnosti nám chcete-li deklarovat pole zálohování.*

Potom postupujte podle stejného postupu vytvořte třídu alba (s názvem Album.cs), který má název a vlastnosti žánr:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

Teď můžeme upravit StoreController použití zobrazení, které budou zobrazovat dynamické informace z našeho modelu. Pokud - pro demonstrační účely teď - jsme naše alb podle ID požadavku, zobrazíme tyto informace jako v zobrazení níže.

![](mvc-music-store-part-3/_static/image10.png)

Začneme tak, že změníte akce Store Details tak, aby zobrazovala informace pro jeden album. Přidejte "pomocí" příkaz do horní části **StoreControllers** třídy, aby obsahoval obor názvů MvcMusicStore.Models, takže jsme nemusíte zadávat MvcMusicStore.Models.Album pokaždé, když chceme použít třídu alba. Část "použití" této třídy by teď měl vypadat jako dole.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

V dalším kroku aktualizujeme akce kontroleru podrobnosti tak, aby vracel třídu ActionResult, spíše než řetězce, jako jsme to udělali s metodou HomeController Index.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

Nyní jsme můžete upravit logiku pro objekt alba vrátit do zobrazení. Později v tomto kurzu jsme se se načítání dat z databáze – ale právě teď budeme používat "fiktivní data" abyste mohli začít.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

*Poznámka: Pokud nejste obeznámeni s jazykem C#, které mohou předpokládat, že pomocí var znamená, že je naše proměnná alba s pozdní vazbou. Který není správný – kompilátor jazyka C# používá odvození typu na základě toho, co jsme se přiřadí proměnné k určení, že je tento alba typu alba a kompilaci alba místní proměnné jako typ alba jsme získali kompilace kontrolu a sady Visual Studio code – editor podpora.*

Pojďme teď vytvořit zobrazit šablonu, která používá naše alba generovat odpověď jazyka HTML. Než to potřebujeme k sestavení projektu tak, aby se dialogové okno Přidat zobrazení ví o náš nově vytvořené třídy alba. Můžete sestavte projekt výběrem Debug⇨Build MvcMusicStore položky nabídky (pro větší užitečnost můžete klávesovou zkratku Ctrl-Shift-B a sestavte projekt).

![](mvc-music-store-part-3/_static/image11.png)

Teď, když jsme vytvořili naši podpůrných tříd, jsme připraveni k sestavení upravíme šablonu zobrazení. Klikněte pravým tlačítkem v rámci metody podrobnosti a v místní nabídce vyberte "Přidat zobrazení...".

![](mvc-music-store-part-3/_static/image12.png)

Budeme vytvořte novou šablonu zobrazení jako jsme to udělali dříve s HomeController. Protože jsme vytváření z StoreController je ve výchozím nastavení se vygeneruje soubor \Views\Store\Index.cshtml.

Na rozdíl od dříve, budeme zaškrtněte políčko "Vytvořit silného typu" zobrazení. Pak budeme vyberte naše "Album" třídu v rámci rozevírací downlist "Data třída zobrazení". To způsobí, že dialog "Přidat zobrazení" pro vytvoření zobrazit šablonu, která očekává, alba objektu se předají do něj chcete použít.

![](mvc-music-store-part-3/_static/image13.png)

Když kliknete na tlačítko "Přidat" náš \Views\Store\Details.cshtml zobrazit šablonu se vytvoří, obsahující následující kód.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

Všimněte si, že první řádek, který označuje, že toto zobrazení je silně typováno do našich alba třídy. Zobrazovací modul Razor plně chápe, že ho byl předán objekt alb, abychom mohli snadno přistupovat k vlastnosti modelu a dokonce i využívat technologie IntelliSense v editoru Visual Web Developer.

Aktualizace &lt;h2&gt; označit tak, aby zobrazila alba název vlastnosti tak, že upravíte tento řádek se zobrazí takto.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

Všimněte si, že technologie IntelliSense se aktivuje, když zadáte období po @Model – klíčové slovo ukazující vlastnosti a metody, které podporuje alba třídy.

Pojďme teď znovu spustit našem projektu a přejděte na adresu URL Store/podrobnosti/5. Uvidíme podrobnosti Album jako níže.

![](mvc-music-store-part-3/_static/image14.png)

Teď uděláme podobné aktualizace pro Store vyhledejte metodu akce. Aktualizovat tuto metodu tak, aby vracel třídu ActionResult a upravit logiku metody tak, aby ho vytvoří nový objekt žánr a vrátí ji do zobrazení.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

Klikněte pravým tlačítkem v metodě Procházet a vyberte v místní nabídce "Přidat zobrazení..." Přidat zobrazení silného typu přidejte do třídy rozšířením podle tematických silného typu.

![](mvc-music-store-part-3/_static/image15.png)

Aktualizace &lt;h2&gt; element v zobrazení kódu (v /Views/Store/Browse.cshtml) k zobrazení informací žánr.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

Nyní Pojďme naši projekt spusťte znovu a přejděte do/Store/Procházet? Rozšířením podle tematických = Disco adresy URL. Uvidíme procházet stránky zobrazují jako níže.

![](mvc-music-store-part-3/_static/image16.png)

Nakonec vytvoříme o něco složitější aktualizace **Store Index** metody akce a zobrazení seznamu všech žánry v našem úložišti. Můžeme to udělat pomocí seznamu žánry jako náš objekt modelu, nikoli jen jeden žánr.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

Klikněte pravým tlačítkem na metodu akce Store Index a vyberte Přidat zobrazení, vyberte žánr jako třída modelu a klepněte na tlačítko Přidat.

![](mvc-music-store-part-3/_static/image17.png)

Nejprve Změníme @model deklarace můžete určit, že zobrazení se očekává několik žánr objekty místo jen jeden. Změňte první řádek /Store/Index.cshtml takto:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

Znamená to, který bude pracovat objekt modelu, který může obsahovat několik objektů žánr zobrazovací modul Razor. Používáme použití rozhraní IEnumerable&lt;žánr&gt; namísto seznamu&lt;žánr&gt; protože obecnější, abychom mohli změnit později náš model typ na libovolný typ objektu, který podporuje rozhraní IEnumerable.

Dále jsme vám projít žánr objekty v modelu, jak je znázorněno v následujícím kódu dokončené zobrazení.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

Všimněte si, že máme plnou podporu technologie IntelliSense jsme zadávání tento kód tak, že když zadáme "@Model." vidíme všechny metody a vlastnosti nepodporuje použití typu žánr rozhraní IEnumerable.

![](mvc-music-store-part-3/_static/image18.png)

V rámci naší smyčky "foreach" Visual Web Developer ví, že každá položka je typu Genre, takže uvidíme technologie IntelliSense pro každý žánr typ.

![](mvc-music-store-part-3/_static/image19.png)

V dalším kroku funkci generování uživatelského rozhraní prozkoumat žánr objekt a určit, že každý bude mít název vlastnosti, takže prochází a zapíše je. Generuje také upravit, podrobnosti a odstranit odkazy na jednotlivé položky. Provedeme výhod, které později v našich Správce úložiště, ale prozatím jsme chtěli místo toho máte jednoduchý seznam.

Po spuštění aplikace a přejděte do/Store, vidíme, že se zobrazí počet a seznam žánrů.

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a>Přidání propojení mezi stránkami

Naší adresy URL/Store, který obsahuje seznam aktuálně žánry Vypíše názvy žánr jednoduše jako prostý text. Umožňuje změnit tak, aby místo prostého textu máme místo toho odkaz názvy žánr na příslušnou adresu URL Store/procházet tak, že kliknete na Hudba žánr jako "ROZ" přejdete na/Store/procházet? žánr = Roz adresy URL. Může aktualizujeme naše \Views\Store\Index.cshtml zobrazit šablonu pro výstup těchto odkazů pomocí kódu jako níže **(nezadávejte to – teď ještě chvíli Zůstaneme ke zlepšení v něm)**:

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

To funguje, ale může způsobit potíže při později, protože závisí na pevně zakódované řetězce. Například jsme chtěli přejmenovat Kontroleru, jsme by nemusíte hledat našeho kódu hledají odkazy, které se musí aktualizovat.

Alternativním přístupem, které používáme můžete je využít metody pomocné rutiny HTML. ASP.NET MVC zahrnuje metody pomocné rutiny HTML, které jsou k dispozici z našeho kódu šablony zobrazení provádět řadu běžných úloh stejně jako to. Pomocná metoda Html.ActionLink() je zvlášť užitečné a usnadňuje sestavení HTML &lt;&gt; odkazy a se postará o obtěžující podrobnosti jako zajistit cesty adresy URL jsou správně kódování URL.

Html.ActionLink() má několik různých přetížení, aby umožňoval zadání co nejvíce informací, jako je třeba pro odkazy. V nejjednodušším případě budete zadáte text odkazu a metoda akce se má přejít při kliknutí na hypertextový odkaz na straně klienta. Můžete například odkaz na "/ Store /" Index() metodu na stránce s podrobnostmi Store se text odkazu "Přejít na the Store Index" pomocí následujícího volání:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

*Poznámka: V tomto případě neměli potřebujeme zadat název kontroleru, protože jsme právě při připojování ke jinou akci v rámci stejné kontroleru, který je vykreslování aktuálního zobrazení.*

Naše odkazy na stránky procházet muset předat parametr, takže použijeme jiné přetížení metody Html.ActionLink, který přijímá tři parametry:

- 1. Text odkazu, který se zobrazí název žánru
- 2. Název akce kontroleru (Procházet)
- 3. Hodnoty parametru trasy, zadáte název (žánr) a hodnota (žánr name)

Uvedení, že všechno dohromady, tady je budeme jak psát tyto odkazy na zobrazení indexu Store:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

Nyní když jsme naše projekt spusťte znovu a přístup k adrese URL /Store/ uvidíme seznam žánrů. Každý žánr je hypertextového odkazu – při klepnutí na potrvá nám na naše/Store/procházet? žánr =*[žánr]* adresy URL.

![](mvc-music-store-part-3/_static/image3.jpg)

Kód HTML pro daný seznam žánr vypadá takto:

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> [Předchozí](mvc-music-store-part-2.md)
> [další](mvc-music-store-part-4.md)
