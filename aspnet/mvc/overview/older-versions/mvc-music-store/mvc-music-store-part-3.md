---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Část 3: Zobrazení a ViewModels | Microsoft Docs'
author: jongalloway
description: Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště. Část 3 popisuje zobrazení a ViewModels.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 497c2898db2e03b0650982c3ad1e6b5ac5ca639d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="part-3-views-and-viewmodels"></a>Část 3: Zobrazení a ViewModels
====================
podle [Jon Galloway](https://github.com/jongalloway)

> Úložiště Hudba MVC je kurz aplikace, která představuje a vysvětluje krok za krokem, jak používat rozhraní ASP.NET MVC a Visual Studio pro vývoj webů.  
>   
> Úložiště Hudba MVC je implementace úložiště lightweight ukázkové, který prodává hudebních alb online a implementuje základní Správa serveru, přihlášení uživatele a nákupního košíku funkce.  
>   
> Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště. Část 3 popisuje zobrazení a ViewModels.


Pokud jsme jste právě byla vrácení řetězce z akce kontroleru. Který je dobrý způsob, jak získat představu o fungování řadiče, ale není by způsob vytvoření skutečné webové aplikace. Přidáme má lepší způsob, jak generují kód HTML do prohlížečů navštěvující náš web – jeden kde šablony soubory můžete použít k více snadno přizpůsobit obsah HTML, který zaslán zpět. Je přesně co dělat, zobrazení.

## <a name="adding-a-view-template"></a>Přidání zobrazení šablony

Použít šablonu zobrazení, jsme budete změnit metodu HomeController Index vrátit třídu ActionResult a s její pomocí vrátit View(), jako je nižší než:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

Výše uvedené změnu označuje, že místo vrátí řetězec, chceme místo toho použijte "Náhled" k získání výsledku zpět.

Teď přidáme odpovídající zobrazit šablonu do našich projektu. Uděláte to tak jsme budete umístěte kurzor text v rámci metody akce indexu, pak klikněte pravým tlačítkem a vyberte možnost "Přidat zobrazení". Tím se otevře dialogové okno Přidat zobrazení:

![](mvc-music-store-part-3/_static/image1.jpg)![](mvc-music-store-part-3/_static/image1.png)

Dialogové okno "Přidat zobrazení" umožňuje snadno a rychle generovat soubory šablon zobrazení. Ve výchozím nastavení "Přidat zobrazení" dialogové okno předem vyplní název zobrazení šablony vytvořit tak, aby odpovídala metody akce, která bude používat. Protože jsme použili v místní nabídce "Přidat zobrazení" v metodě akce Index() naše HomeController, dialogové okno "Přidat zobrazení" výše má "Index" jako název zobrazení předem vyplní ve výchozím nastavení. Není třeba změnit nastavení v tomto dialogovém okně, proto klikněte na tlačítko Přidat.

Když kliknete na tlačítko Přidat, Visual Web Developer vytvoří novou Index.cshtml zobrazit šablonu pro nám v adresáři \Views\Home složku vytvoříte, pokud ještě neexistuje.

![](mvc-music-store-part-3/_static/image2.png)

Název a umístění složky souboru "Index.cshtml" je důležité a dodržuje konvence pojmenování výchozí rozhraní ASP.NET MVC. Název adresáře, \Views\Home, odpovídá řadičem – který se nazývá HomeController. Název šablony zobrazení, Index, odpovídá metoda akce kontroleru, který bude zobrazit zobrazení.

ASP.NET MVC umožňuje vyhnout se nutnosti explicitně zadat název nebo umístění zobrazení šablony při používáme tyto zásady vytváření názvů lze vrátit zobrazení. Se budou ve výchozím nastavení vykreslovat zobrazit šablonu \Views\Home\Index.cshtml při psaní kódu jako níže v rámci naší HomeController:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

Visual Web Developer vytvořil a spustil zobrazit šablonu "Index.cshtml" po jsme kliknutí na tlačítko "Přidat" v dialogovém okně "Přidat zobrazení". Níže jsou uvedeny obsah Index.cshtml.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

Toto zobrazení je pomocí syntaxe Razor, který je přesnější, než bude zobrazovací modul webových formulářů používají ve webových formulářů ASP.NET a předchozích verzích rozhraní ASP.NET MVC. Modul zobrazení webových formulářů je stále k dispozici v architektuře ASP.NET MVC 3, ale celá řada vývojářů najít zobrazovací modul Razor skutečně dobře vyhovuje vývoj ASP.NET MVC.

První tři řádky nastavit název stránky pomocí ViewBag.Title. Jsme budete podívejte se jak to funguje podrobněji brzy, ale nejdřív umožňuje aktualizace textu v textu nadpisu a zobrazení stránky. Aktualizace &lt;h2&gt; značky k vyslovení "Toto je na domovskou stránku", jak je uvedeno níže.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

Spuštění aplikace zobrazí, že naše nového textu je zobrazen na domovské stránce.

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a>Společné prvky lokality pomocí rozložení

Většina webů mají obsah, který je sdílen mezi počet stránek: navigace, zápatí stránky, loga, odkazy na šablony stylů, atd. Zobrazovací modul Razor umožňuje snadno spravovat pomocí stránku s názvem \_Layout.cshtml, která automaticky byla vytvořena pro nás do sdílené složky nebo zobrazení /.

![](mvc-music-store-part-3/_static/image4.png)

Dvakrát klikněte na tuto složku pro zobrazení obsahu, který je uveden níže.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

Zobrazí se obsah z našich jednotlivých zobrazení ve @RenderBodypříkazu () a všechny společného obsahu, který má být nacházet mimo, lze přidat do \_Layout.cshtml značek. Chceme budete naše úložiště Hudba MVC mít běžné hlavička s odkazy na našich oblast domovské stránky a úložiště na všechny stránky v lokalitě, která přidáme do šablony přímo vyšší @RenderBody– příkaz ().

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a>Aktualizace šablony stylů

Šablona prázdný projekt obsahuje soubor velmi efektivní šablon stylů CSS, který obsahuje pouze styly slouží k zobrazení zpráv ověření. Naše Návrhář poskytl některé další šablon stylů CSS a bitové kopie k definování vzhledu a chování pro náš web, tak v nyní přidáme.

Aktualizovaný soubor CSS a image jsou součástí obsahu adresáře MvcMusicStore Assets.zip, která je k dispozici v [MVC. Hudba úložiště](https://github.com/evilDave/MVC-Music-Store). Jsme budete oba dva vyberte v Průzkumníku Windows a jejich umístění do složky obsahu naše řešení v aplikaci Visual Web Developer, jak je uvedeno níže:

![](mvc-music-store-part-3/_static/image5.png)

Budete vyzváni k potvrzení, pokud ji chcete přepsat existující soubor Site.css. Klikněte na tlačítko Ano.

![](mvc-music-store-part-3/_static/image6.png)

Složku obsahu aplikace bude nyní vypadat takto:

![](mvc-music-store-part-3/_static/image7.png)

Teď umožňuje spuštění aplikace a zjistit, jak naše změny zobrazovat na domovské stránce.

![](mvc-music-store-part-3/_static/image8.png)

- Pojďme si shrnout, co se změnilo: metody akce HomeController Index najít a zobrazí šabloně \Views\Home\Index.cshtmlView, i když naše kód volat "návratový View()", protože naše zobrazit šablonu a potom standardní zásady vytváření názvů.
- Domovská stránka zobrazuje jednoduché uvítací zprávy, která je definována v rámci šablony \Views\Home\Index.cshtml zobrazení.
- Domovská stránka používá naše \_Layout.cshtml šablony, a proto zobrazení uvítací zprávy je obsažena v rozložení standardní webu HTML.

## <a name="using-a-model-to-pass-information-to-our-view"></a>Použití modelu k předávání informací do našich zobrazení

Zobrazit šablonu, která se zobrazí pouze pevně zakódované HTML nepůjde zajistit velmi zajímavé webu. K vytvoření dynamického webového serveru, budete místo chceme předávání informací z našich akce kontroleru našich šablon zobrazení.

Ve vzoru Model-View-Controller pojem modelu odkazuje na objekty, které představují data v aplikaci. Často objekty modelu odpovídají tabulek v databázi, ale není nutné.

Metody akce kontroleru, které vrací třídu ActionResult můžete předat objekt modelu zobrazení. To umožňuje Kontroleru do této aplikace balíčku se všechny informace potřebné k generovat odpověď a pak předejte tyto informace do zobrazit šablonu použít pro vygenerování odpovídající odpověď protokolu HTML. Toto je nejjednodušší pochopit zobrazuje v akci, takže můžeme začít.

Nejdříve vytvoříme některé třídy modelu k reprezentaci žánry a alb v rámci naší úložiště. Začněme vytvořením třídy Genre. Klikněte pravým tlačítkem na složku "Modely" v rámci projektu, zvolte možnost "Přidat třídu" a "Genre.cs" název souboru.

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

Pak přidáte řetězec veřejný název vlastnosti pro třídu, která byla vytvořena:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

*Poznámka: V případě přemýšlíte {získat; nastavit;} zápis je zajistit použití funkce jazyka C# je automaticky implementované vlastnosti. To nám poskytuje výhody vlastnost bez nutnosti nám základní pole deklarovat.*

Potom postupujte podle stejných kroků pro vytvoření alb třídy (s názvem Album.cs), který má název a Genre vlastnost:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

Nyní jsme upravit StoreController použít zobrazení, které budou zobrazovat dynamické informace z našich modelu. Pokud - pro demonstrační účely teď - jsme pojmenovali naše alb podle ID požadavku, zobrazujeme tyto informace jako zobrazení níže.

![](mvc-music-store-part-3/_static/image10.png)

Začneme změnou akce podrobnosti úložiště tak, že se zobrazí informace o jednom alb. Přidejte příkaz, "pomocí" do horní části **StoreControllers** třída zahrnout MvcMusicStore.Models oboru názvů, takže jsme není nutné zadávat MvcMusicStore.Models.Album pokaždé, když chcete použít třídu alb. V části "direktiv Using" této třídy by se měla zobrazit jako níže.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

Dále budete aktualizujeme akce kontroleru podrobnosti tak, aby vracel třídu ActionResult, nikoli řetězec, jako jsme to udělali s metodou HomeController Index.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

Nyní jsme upravit logika pro vrátit objekt alb do zobrazení. Později v tomto kurzu jsme bude možné načítání dat z databáze – ale pro nyní budeme používat "fiktivní data" začít pracovat.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

*Poznámka: Pokud jste obeznámeni s C#, jste mohou předpokládat, že pomocí var znamená, že naše album proměnné se pozdní vazbou. Není správný – kompilátor jazyka C# používá odvození typu na základě co jsme se přiřazení k proměnné můžete určit, že alb je typu alba a kompilování proměnnou místní album jako typ alb, proto jsme kontrolu kompilaci a Visual Studio – editor kódu podpora.*

Nyní vytvoříme zobrazení šablony, která používá naše Album ke generování odpovědi HTML. Než to potřebujeme sestavte projekt tak, aby se dialogové okno Přidat zobrazení, že zná naše nově vytvořené třídy alb. Můžete vytvořit projekt tak, že vyberete Debug⇨Build MvcMusicStore položku nabídky (pokud to pro další platební můžete použít zástupce Ctrl-Shift-B a tím projekt sestavit).

![](mvc-music-store-part-3/_static/image11.png)

Nastavili jsme naše podpůrných tříd, jsme připraveni k sestavení naše zobrazit šablonu. Klikněte pravým tlačítkem do metody podrobnosti a vyberte možnost "Přidat zobrazení..." v místní nabídce.

![](mvc-music-store-part-3/_static/image12.png)

Přidáme vytvořte novou šablonu zobrazení jako jsme to udělali před s HomeController. Protože jsme vytváříte z StoreController je ve výchozím nastavení vygeneruje v souboru \Views\Store\Index.cshtml.

Na rozdíl od před, přidáme zaškrtnutím políčka "Vytvořit silného typu" zobrazení. Potom přidáme vyberte třídu naše "Album" v rámci rozevírací downlist "Zobrazení třída dat". To způsobí, že "Přidat zobrazení" dialogové okno vytvořit šablony zobrazení, která očekává, alba objektu se předá ho na použití.

![](mvc-music-store-part-3/_static/image13.png)

Když kliknete na tlačítko "Přidat" naše \Views\Store\Details.cshtml zobrazení bude vytvořit šablonu, obsahující následující kód.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

Všimněte si první řádek, který označuje, že je toto zobrazení silného typu naše Album třídy. Zobrazovací modul Razor plně chápe, že ho byl předán objekt alb, proto jsme snadný přístup k vlastnosti modelu a i využívat výhod IntelliSense v editoru Visual Web Developer.

Aktualizace &lt;h2&gt; značky proto zobrazí název vlastnosti alba změnou daného řádku a zobrazí následující.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

Všimněte si, že IntelliSense se aktivuje, když zadáte dobu, po @Model – klíčové slovo, zobrazuje vlastnosti a metody, které třída Album podporuje.

Pojďme nyní znovu spustit naše projektu a navštívíte adresu úložiště/podrobnosti/5. Jsme zobrazí podrobnosti o Album jako níže.

![](mvc-music-store-part-3/_static/image14.png)

Nyní vytočit podobné aktualizace pro metodu akce procházet úložiště. Aktualizujte metodu tak, aby vracel třídu ActionResult a změnit metoda logiku, takže se vytvoří nový objekt Genre a vrátí ji do zobrazení.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

Klikněte pravým tlačítkem v metodě Procházet a vyberte možnost "Přidat zobrazení..." v místní nabídce pak přidat zobrazení, které je silně typované třídy Genre přidat silného typu.

![](mvc-music-store-part-3/_static/image15.png)

Aktualizace &lt;h2&gt; element v zobrazení Chcete-li zobrazit informace o Genre kód (v /Views/Store/Browse.cshtml).

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

Nyní Pojďme znovu spustit naše projektu a vyhledejte/úložiště/Procházet? Genre = Disco adresy URL. Uvidíme procházet stránka zobrazená jako níže.

![](mvc-music-store-part-3/_static/image16.png)

Nakonec vytvoříme trochu složitější aktualizace **ukládání Index** metody akce a zobrazení seznamu všech žánry v našem úložišti. Jsme to udělat pomocí seznamu žánry jako naše objekt modelu, nikoli jen jeden Genre.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

Klikněte pravým tlačítkem na metodu akce Index úložiště a vyberte možnost Přidat zobrazení jako dříve, vyberte Genre jako třídu modelu a klikněte na tlačítko Přidat.

![](mvc-music-store-part-3/_static/image17.png)

Nejprve Změníme @model deklarace k označení, že zobrazení se očekává několik Genre objekty místo pouze jeden. Změna první řádek /Store/Index.cshtml tímto:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

Tato hodnota informuje zobrazovací modul Razor, který bude práce objekt modelu, který může obsahovat několik Genre objekty. Použití rozhraní IEnumerable používáme&lt;Genre&gt; místo seznam&lt;Genre&gt; vzhledem k tomu, že je více obecné, abychom mohli změnit později naše typu modelu k libovolnému typu objektu, který podporuje rozhraní IEnumerable.

V dalším kroku jsme vám projít Genre objekty v modelu, jak je znázorněno v následujícím kódu dokončené zobrazení.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

Všimněte si, že jsme mají úplnou podporu technologie IntelliSense jako jsme tento kód zadejte, že když jsme zadejte "@Model." vidíte všechny metody a vlastnosti podporuje použití rozhraní IEnumerable typu Genre.

![](mvc-music-store-part-3/_static/image18.png)

V našem smyčka "typu foreach" Visual Web Developer ví, že jednotlivé položky je typu Genre, tak vidíme IntelliSence pro každý typ Genre.

![](mvc-music-store-part-3/_static/image19.png)

Funkce generování uživatelského rozhraní v dalším kroku zkontrolován objekt Genre a zjistit, název vlastnosti, každý bude mít, aby prochází a zapíše. Generuje také upravit, podrobnosti a odstranit odkazy na jednotlivé položky. Provedeme výhod, dále v našem Správce úložiště, ale prozatím jsme chtěli místo toho máte jednoduchý seznam.

Když jsme aplikaci spustit a vyhledejte/Store, vidíme, že se zobrazí počet a seznam žánry.

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a>Přidávání odkazů mezi stránkami

Naše/Store URL, která uvádí žánry aktuálně seznam názvů Genre jednoduše jako prostý text. Umožňuje změnit tak, aby místo prostého textu máme místo odkaz Genre názvy na příslušnou adresu URL úložiště nebo procházení, tak, že kliknete na genre Hudba, jako jsou "Disco" bude přejděte/úložiště / procházet? genre = Disco adresy URL. Aktualizujeme může naše \Views\Store\Index.cshtml zobrazit šablonu výstupu tyto odkazy pomocí kódu jako níže **(to nezadávejte – vytvoříme ke zlepšení na něm)**:

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

To funguje, ale by mohlo vést k řešení problémů později vzhledem k tomu, že přitom spoléhá na pevně kódovaný řetězec. Například pokud jsme chtěli přejmenovat Kontroleru, by potřebujeme k hledání v našem kódu, hledá odkazy, které je nutné aktualizovat.

Alternativní způsob, které můžete používáme je využít metody pomocné rutiny HTML. ASP.NET MVC zahrnuje metody pomocné rutiny HTML, které jsou k dispozici z našich kód šablony zobrazení k provádění různých běžných úloh stejně jako tento. Pomocná metoda Html.ActionLink() je užitečné a usnadňuje sestavení HTML &lt;&gt; odkazy a má na starosti nepříjemných podrobnosti, třeba zajistit cest URL mohly správně kódovaná adresou URL.

Html.ActionLink() má několik různých přetížení umožňující zadání tolik informací, jako je třeba pro vaše odkazy. V nejjednodušším případě zadejte text odkazu a metoda akce se má přejít při kliknutí na hypertextový odkaz na straně klienta. Například můžeme můžete propojit "/ úložiště /" metoda Index() na stránce Podrobnosti úložiště s text odkazu "Přejděte na v úložišti Index" pomocí následující volání:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

*Poznámka: V tomto případě nebylo musíme zadat jeho název, protože jsme se právě propojení k jiné akce v rámci stejné kontroleru, který je vykreslování aktuální zobrazení.*

Naše odkazy na stránce procházení se bude muset předání parametru, ale proto použijeme jiné přetížení metody Html.ActionLink, která přijímá tři parametry:

- 1. Text odkazu, který se zobrazí název Genre
- 2. Název akce kontroleru (Procházet)
- 3. Hodnoty parametru trasy, zadání názvu (Genre) a hodnota (Genre název)

Uvedení, že všechny společně, zde je jak tyto odkazy budete psát do zobrazení Index úložiště:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

Nyní když jsme naše projekt znovu spustit a přístup k adrese URL /Store/ jsme se zobrazí seznam žánry. Každý genre je při kliknutí na hypertextový odkaz – bude trvat nám naše/úložiště/procházet? genre =*[genre]* adresy URL.

![](mvc-music-store-part-3/_static/image3.jpg)

Ve zdroji HTML seznamu genre vypadá takto:

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> [Předchozí](mvc-music-store-part-2.md)
> [další](mvc-music-store-part-4.md)
