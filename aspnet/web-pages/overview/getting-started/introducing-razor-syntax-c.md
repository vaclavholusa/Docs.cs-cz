---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: Úvod do rozhraní ASP.NET Web programování pomocí syntaxe Razor (C#) | Microsoft Docs
author: tfitzmac
description: Tato kapitola obsahuje přehled programování pomocí webových stránek ASP.NET pomocí syntaxe Razor. ASP.NET je technologie společnosti Microsoft pro spuštění dynamického webového pa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: 430033c06df74cc3661c40ca7f7bd9244cd257c9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a>Úvod do rozhraní ASP.NET Web programování pomocí syntaxe Razor (C#)
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento článek poskytuje přehled programování pomocí webových stránek ASP.NET pomocí syntaxe Razor. ASP.NET je technologie společnosti Microsoft pro spouštění dynamické webové stránky na webové servery. Toto se článek zaměřuje na pomocí programovacího jazyka C#.
> 
> **Co se dozvíte**:
> 
> - Horní 8 Programovací tipy pro Začínáme s programováním pomocí syntaxe Razor rozhraní ASP.NET Web Pages.
> - Základní koncepty programování, které budete potřebovat.
> - Jaké kódu serveru ASP.NET a syntaxe Razor je všechno o.
>   
> 
> ## <a name="software-versions"></a>Verze softwaru
> 
> 
> - Rozhraní ASP.NET Web Pages (Razor) 3
>   
> 
> V tomto kurzu taky spolupracuje se službou ASP.NET Web Pages 2.


## <a name="the-top-8-programming-tips"></a>Horní 8 Tipy pro programování

Tato část obsahuje několik tipy, které je nezbytně nutné znát jako počáteční psaní kódu serveru ASP.NET pomocí syntaxe Razor.

> [!NOTE]
> Syntaxe Razor je založena na programovací jazyk C# a který je jazyk, který se používá nejčastěji s ASP.NET Web Pages. Syntaxe Razor však také podporuje jazyka Visual Basic a vše, co vidíte, že můžete provést také v jazyce Visual Basic. Podrobnosti najdete v tématu příloze [jazyka Visual Basic a syntaxe](https://go.microsoft.com/fwlink/?LinkId=202908).


Další informace o většinu těchto programátorských technik, naleznete dále v článku.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Přidejte kód na stránku pomocí @ – znak

`@` Začne znak vložené výrazy, jediný příkaz bloky a bloky vícepříkazové:

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

Toto je, jak tyto příkazy vypadat při spuštění stránky v prohlížeči:

![Razor-Img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> **HTML Encoding**
> 
> Při zobrazení obsahu stránce pomocí `@` znak, jako v předchozích příkladech ASP.NET umístí kódování HTML výstup. Tím se nahradí vyhrazené znaky HTML (například `<` a `>` a `&`) s kódy, které umožňují znaků, který má být zobrazen jako znaků na webové stránce nebude interpretován jako značky HTML nebo entity. Bez kódování HTML výstup z vašeho kódu serveru se nemusí zobrazit správně a mohla vystavit stránky na bezpečnostní rizika.
> 
> Pokud vaším cílem je výstup jazyka HTML, který vykreslí značky jako značky (například `<p></p>` pro odstavec nebo `<em></em>` pro zvýraznění textu), najdete v části [kombinování Text, značek a kódu v bloky kódu](#BM_CombiningTextMarkupAndCode) dále v tomto článku.
> 
> Další informace o kódování HTML v [práci s formuláři](https://go.microsoft.com/fwlink/?LinkId=202892).


### <a name="2-you-enclose-code-blocks-in-braces"></a>2. Bloky kódu je uzavřít do složených závorek

A *blok kódu* obsahuje jeden nebo více příkazů kódu a je uzavřené do složených závorek.

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

Výsledek zobrazí v prohlížeči:

![Razor-Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a>3. Uvnitř bloku vám stát, že každý příkaz kódu středníkem

Každý příkaz kompletní kód uvnitř bloku kódu musí končit středníkem. Vložené výrazy nejsou končit středníkem.

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a>4. Proměnné se používají k ukládání hodnot

Hodnoty v lze ukládat *proměnná*, včetně řetězce, čísla a kalendářní data, atd. Vytvoření nové proměnné pomocí `var` – klíčové slovo. Hodnoty proměnných můžete vložit přímo do stránky pomocí `@`.

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

Výsledek zobrazí v prohlížeči:

![Razor-Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. Uzavřete řetězcového literálu hodnoty v uvozovkách

A *řetězec* je posloupnost znaků, které jsou považovány za text. Pokud chcete zadat řetězec, vložte jej do dvojitých uvozovek:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

Pokud řetězec, který chcete zobrazit obsahuje znak zpětného lomítka ( `\` ) nebo dvojité uvozovky ( `"` ), použijte *typu verbatim řetězcový literál* , je s předponou `@` operátor. (V jazyku C# \ znak má zvláštní význam, pokud nechcete použít typu verbatim řetězcový literál.)

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

Pro vložení dvojitých uvozovek nahoře, použijte literálu typu verbatim řetězec a opakujte uvozovek:

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

Zde je výsledkem použití obě tyto příklady na stránce:

![Razor Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> Všimněte si, že `@` znak se používá k označení typu verbatim textové literály v jazyce C# a označit kódu na stránkách ASP.NET.


### <a name="6-code-is-case-sensitive"></a>6. Kód je malá a velká písmena

V jazyce C#, klíčová slova (jako je `var`, `true`, a `if`) a názvy proměnných jsou velká a malá písmena. Následující řádky kódu vytvoření dvou různých proměnných, `lastName` a `LastName.`

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

Pokud je deklarovat proměnnou jako `var lastName = "Smith";` a pokud se pokusíte odkazovat na tuto proměnnou na stránce jako `@LastName`, dochází k chybě, protože `LastName` nebude rozpoznána.

> [!NOTE]
> V jazyce Visual Basic, klíčová slova a proměnné jsou *není* velká a malá písmena.


### <a name="7-much-of-your-coding-involves-objects"></a>7. Velká část kódování zahrnuje objekty

*Objekt* představuje věcí, kterou můžete programu s &#8212; stránky textové pole, soubor, bitovou kopii, webové žádosti, e-mailovou zprávu, záznam zákazníka (řádku databáze), atd. Objekty mají vlastnosti, které popisují jejich vlastnosti a že můžou číst nebo změnit &#8212; má objekt textové pole `Text` vlastnost (mimo jiné), má objekt žádosti `Url` vlastnost, má e-mailovou zprávu `From` vlastnost a má objekt zákazníka `FirstName` vlastnost. Objekty mají metody, které jsou také &quot;příkazy&quot; mohou provádět. Mezi příklady patří objektu soubor `Save` metoda, objekt image `Rotate` metoda a objekt e-mailu `Send` metoda.

Často budete pracovat `Request` objektu, který nabízí informace jako hodnoty textová pole (polí formuláře) na stránce, jaký typ prohlížeče požadavek, adresu URL stránky, identita uživatele, atd. Následující příklad ukazuje, jak pro přístup k vlastnostem `Request` objekt a jak volat `MapPath` metodu `Request` objekt, který vám dává absolutní cesta ke stránce na serveru:

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

Výsledek zobrazí v prohlížeči:

![Razor-Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. Můžete napsat kód, který uskutečňuje rozhodnutí

Klíčovou funkcí dynamické webové stránky je, že můžete určit co dělat na základě podmínky. Nejběžnější způsob k tomu je s `if` – příkaz (a volitelné `else` příkaz).

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

Příkaz `if(IsPost)` je zjednodušený způsob zápis `if(IsPost == true)`. Spolu s `if` příkazy, existuje mnoho různých způsobů, jak testovacích podmínkách, opakování bloky kódu, a tak dále, které jsou popsány dále v tomto článku.

Výsledek zobrazí v prohlížeči (po kliknutí na **odeslání**):

![Razor Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a>HTTP GET a POST metody a vlastnosti IsPost
> 
> Protokol použitý pro webové stránky (HTTP) podporuje velmi omezený počet metody (operace), které se používají k provádění požadavků na server. Dva nejběžnější ty, které jsou GET, které slouží k načtení stránky, a POST, který se používá k odeslání stránky. Obecně platí při prvním požadavku na stránce požadavku na stránku pomocí GET. Pokud uživatel doplní do formuláře a potom kliknutím na tlačítko pro odeslání, prohlížeč provede požadavek POST na server.
> 
> Ve webové programování je často užitečné vědět, jestli stránky je vyžadován jako GET nebo POST, abyste věděli, jak pro zpracování stránky. Na webových stránkách ASP.NET, můžete použít `IsPost` vlastnosti chcete zobrazit, zda je požadavek GET nebo POST. Pokud je požadavek POST, `IsPost` vlastnost vrátí hodnotu true, a můžete provést třeba číst hodnoty textová pole ve formuláři. Uvidíte mnoho příklady ukazují, jak zpracovat stránce odlišně v závislosti na hodnotě `IsPost`.


## <a name="a-simple-code-example"></a>Jednoduchým příkladem kódu

Tento postup ukazuje, jak vytvořit stránku, která ukazuje základní programovací techniky. V příkladu vytvoříte stránky, který umožňuje uživatelům zadejte dvě čísla, potom se přidají a zobrazuje výsledek.

1. V editoru, vytvořte nový soubor s názvem *AddNumbers.cshtml*.
2. Zkopírujte následující kód a kód na stránku nahraďte nic již na stránce.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    Zde jsou některé kroky, abyste Všimněte si:

    - `@` Znak spustí první blok kódu na stránce, a předchází `totalMessage` proměnné, která se vloží v dolní části stránky.
    - Blok v horní části stránky je uzavřené do složených závorek.
    - Všechny řádky v bloku v horní části končit středníkem.
    - Proměnné `total`, `num1`, `num2`, a `totalMessage` uložit několik čísla a řetězec.
    - Řetězcový literál hodnota přiřazená `totalMessage` proměnná je v uvozovkách.
    - Protože kód je malá a velká písmena, když `totalMessage` proměnná se používá v dolní části stránky, jeho název musí přesně shodovat proměnnou v horní části.
    - Výraz `num1.AsInt() + num2.AsInt()` ukazuje, jak pracovat s objekty a metody. `AsInt` Metodu na každou proměnnou převede řetězec zadaný uživatelem na číslo (integer) tak, aby aritmetické operace můžete provádět na něm.
    - `<form>` Zahrnuje značky `method="post"` atribut. To určuje, že když uživatel klikne **přidat**, stránky odešle na server pomocí metody POST protokolu HTTP. Při odeslání stránky `if(IsPost)` testovací vyhodnocena jako true a podmínku kód běží, zobrazení výsledkem přidání čísla.
3. Uložit stránky a spusťte ji v prohlížeči. (Ujistěte se, že je vybraný stránky v **soubory** pracovního prostoru, než ji spustit.) Zadejte dvě celá čísla a poté klikněte **přidat** tlačítko. 

    ![Razor-Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a>Základní koncepty programování

Tento článek vám poskytne přehled webové programování technologie ASP.NET. Není vyčerpávající zkoumání se právě stručný přehled prostřednictvím koncepty programování, který budete používat nejčastěji. I tak pokrývá téměř všechny objekty, které budete muset Začínáme s ASP.NET Web Pages.

Ale první, málo technické pozadí.

### <a name="the-razor-syntax-server-code-and-aspnet"></a>Syntaxe Razor, kódu serveru a technologie ASP.NET

Syntaxe Razor je jednoduchý programovací syntaxi pro vkládání serverový kód na webové stránce. Na webové stránce, která používá syntaxi Razor, existují dva typy obsahu: obsah a server kódu klienta. Obsah klienta je vy jste zvyklí ve webových stránkách: Značka jazyka HTML (prvky), styl informace, jako je šablon stylů CSS, možná některé klientského skriptu, jako je JavaScript a prostý text.

Syntaxe Razor můžete přidat serverový kód k tomuto obsahu klienta. Pokud je na stránce kód serveru, na serveru běží nejprve tento kód tak, než odešle stránku v prohlížeči uvedené. Spuštěním na serveru, kód můžete provádět úlohy, které mohou být mnohem složitější provedete použitím klienta obsahu samostatně, jako je přístup na serveru databáze. Co je nejdůležitější kódu serveru můžete dynamicky vytvořit obsah klienta &#8212; může generovat kód HTML nebo jiného obsahu za chodu a potom ji odešlete do prohlížeče spolu s statické jazyka HTML, který stránky může obsahovat. Z hlediska prohlížeče je obsah klienta, který je generovaný kód serveru nejsou jiné než žádný jiný obsah klienta. Jak už jste viděli, kód serveru, které je nutné je poměrně jednoduché.

Webové stránky ASP.NET, jež obsahují syntaxi Razor mají příponu speciální (*.cshtml* nebo *.vbhtml*). Server rozpozná tato rozšíření, spustí kód, který je označen se syntaxí Razor a poté odešle stránce v prohlížeči.

### <a name="where-does-aspnet-fit-in"></a>Kde se uplatní ASP.NET?

Syntaxe Razor je založena na technologii Microsoft názvem ASP.NET, která zase je založena na rozhraní Microsoft .NET Framework. Rozhraní.NET Framework je velký, komplexní programovací architektura od společnosti Microsoft pro vývoj prakticky jakéhokoli typu aplikace počítače. ASP.NET je součástí rozhraní .NET Framework, která je určená speciálně pro vytváření webových aplikací. Vývojáři mají ASP.NET použít k vytvoření řadu největší a nejvyšší provoz webů na světě. (Vždy, když se zobrazí příponu názvu souboru *.aspx* jako část adresy URL v lokalitě, budete vědět, zda webu, byla zapsána pomocí technologie ASP.NET.)

Syntaxe Razor vám dává všechny ASP.NET, ale pomocí zjednodušenou syntaxi, která je jednodušší zjistit, zda jste začátečník a který zpřístupňuje můžete zvýšit produktivitu Pokud jste odborník. I když tato syntaxe je snadno použitelný, jeho rodiny relaci ASP.NET a rozhraní .NET Framework znamená, že vaše weby jsou složitější, nutné power větší rozhraní, které jsou k dispozici.

![Razor-Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> **Třídy a instance**
> 
> Kód serveru ASP.NET používá objekty, které jsou naopak postavené na nápad tříd. Třída je definice nebo šablony objektu. Například může aplikace obsahovat `Customer` třídu, která definuje vlastnosti a metody, které potřebuje libovolného objektu zákazníka.
> 
> Když aplikace potřebuje pracovat s informacemi o skutečnou zákazníků, vytvoří instanci (nebo *vytvoří*) objekt zákazníka. Každého zákazníka se samostatnou instanci `Customer` třídy. Všechny instance podporuje stejné vlastnosti a metody, ale hodnoty vlastností pro každou instanci se obvykle liší, protože každý objekt zákazníka je jedinečný. V objektu jednoho zákazníka `LastName` vlastnost může být "Smith"; v jiném objektu zákazníka `LastName` vlastnost může být "Petr."
> 
> Podobně je všechny jednotlivé webové stránky ve vaší lokalitě `Page` objekt, který představuje instanci `Page` – třída. Tlačítko na stránce je `Button` objekt, který představuje instanci `Button` třídy a tak dále. Každá instance má svou vlastní vlastnosti, ale všechny jsou založené na zadaných v definici třídy objektu.


## <a name="basic-syntax"></a>Základní syntaxe

Dříve jste viděli základních příkladů jak vytvořit stránku ASP.NET Web Pages a přidání serverový kód pro kód HTML. Zde se dozvíte základní informace o psaní kódu serveru ASP.NET pomocí syntaxe Razor &#8212; tedy programovací jazyk pravidla.

Pokud máte zkušenosti s programováním (obzvláště pokud jste použili C, C++, C#, Visual Basic nebo JavaScript), většinu si přečíst zde bude seznámit. Pravděpodobně budete muset Seznamte se jenom s jak kód serveru je přidán do značek v *.cshtml* soubory.

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a>Kombinování Text, značek a kódu v bloky kódu

V serveru bloky kódu často chcete výstup textu nebo značek (nebo obě) na stránku. Pokud blok serverového kódu obsahuje text, který není kódu a který místo toho má být vykreslen jako je, musí být schopen rozlišit tento text z kódu ASP.NET. Chcete-li to provést několika způsoby.

- Uzavřete text v elementu HTML jako `<p></p>` nebo `<em></em>`:   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    Prvek HTML mohou zahrnovat text, další prvky HTML a výrazy kódu serveru. Když ASP.NET uvidí otevírání značky HTML (například `<p>`), vykreslí vše, včetně elementu a jeho obsah, jako je do prohlížeče, řešení výrazy kód serveru, protože probíhá.
- Použití `@:` operátor nebo `<text>` elementu. `@:` Výstupy jeden řádek obsahující prostý text nebo neodpovídající značky HTML; obsahu `<text>` element uzavře více řádků na výstup. Tyto možnosti jsou užitečné, když nechcete, aby k vykreslení elementu HTML v rámci výstupu.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    Pokud chcete výstup více řádků textu nebo neodpovídající značky HTML, je před každý řádek s `@:`, nebo můžete uzavřete na řádku `<text>` elementu. Podobně jako `@:` operátor,`<text>` značky jsou použitý technologií ASP.NET k identifikaci textového obsahu a nikdy se zobrazují v výstup stránky.

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    V prvním příkladu opakuje předchozí příklad, ale používá jednu dvojici `<text>` značek k uzavřete text k vykreslení. V druhém příkladu `<text>` a `</text>` značky uzavřete tři řádky, které mají všechny některé uncontained text a neodpovídající značky HTML (`<br />`), spolu s kódu serveru a odpovídající značky HTML. Znovu, může také předcházet každý řádek jednotlivě s `@:` operátor; buď způsob, jak funguje.

    > [!NOTE]
    > Pokud výstup text jak je znázorněno v této části &#8212; pomocí prvku HTML `@:` operátor, nebo `<text>` element &#8212; ASP.NET nepodporuje kódování HTML výstup. (Jak je uvedeno výše, ASP.NET kódování výstup výrazy kódu serveru a bloky kódu serveru, které jsou označeny `@`, s výjimkou ve zvláštních případech uvedených v této části.)

### <a name="whitespace"></a>Whitespace

Mezery v příkazu (a mimo řetězcový literál) není ovlivňují příkaz:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

Konec řádku v příkazu nemá žádný vliv na příkaz a může obtékat příkazy čitelnější. Následující příkazy jsou stejné:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

Však nelze obalit řádku uprostřed řetězcový literál. Předchozí postup nebude fungovat v následujícím příkladu:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

Kombinování dlouhý řetězec, který zabalí do více řádků jako výše uvedený kód, existují dvě možnosti. Můžete použít operátor řetězení (`+`), který se zobrazí později v tomto článku. Můžete také `@` znak k vytvoření typu verbatim řetězec literálu, protože jste viděli dříve v tomto článku. Literály typu verbatim řetězec lze rozdělit na řádcích:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a>Kód (a značky) komentáře

Komentáře umožňují nechte poznámky pro sebe nebo ostatní. Také umožňují vám zakázat (*komentář*) oddílu kód nebo značky, které nechcete spustit ale chcete zachovat v stránku prozatím.

Neexistuje jiný komentářů syntaxe Razor kódu a kódu HTML. Stejně jako u všech kódu Razor, komentáře syntaxe Razor jsou zpracovány (a pak odebral) na serveru, než se stránka odešle do prohlížeče. Proto komentovaný syntaxi Razor umožňuje umístit komentáře v kódu (nebo i do kódu), které se zobrazí, pokud chcete soubor upravit, ale není zobrazen uživatelům, i ve zdroji stránky.

Pro komentáře syntaxe Razor rozhraní ASP.NET, spusťte komentář s `@*` a ukončete její `*@`. Komentář může být na jeden nebo více řádků:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

Tady je komentář v rámci bloku kódu:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

Zde je stejný blok kódu, s řádkem kódu komentované tak, aby se nespustí:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

Uvnitř bloku kódu jako alternativu k použití komentáře syntaxe Razor, můžete použít komentovaný syntaxe programovací jazyk, který používáte, například C#:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

V jazyce C#, jeden řádek komentáře předchází `//` znaky a komentáře Víceřádkový začínat `/*` a končit `*/`. (Stejně jako u komentáře syntaxe Razor, se nevykreslí komentáře jazyka C# do prohlížeče.)

Pro značky jak pravděpodobně víte, můžete vytvořit komentář HTML:

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

Komentáře HTML začínat `<!--` znaků a konec s `-->`. Komentáře HTML můžete obklopit pouze text, ale také všechny jazyka HTML, který budete chtít zachovat na stránce, ale nechcete, aby k vykreslení. Tento komentář HTML skryje celý obsah značky a text, který obsahují:

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

Na rozdíl od komentáře syntaxe Razor, HTML komentáře *jsou* vykresluje na stránku a uživatel je uvidí zobrazením zdroji stránky.

Syntaxe Razor má omezení na vnořené bloky jazyka C#. Další informace najdete v článku [s názvem C# proměnné a vnořené bloky generovat přerušený kód](http://aspnetwebstack.codeplex.com/workitem/1914)

## <a name="variables"></a>Proměnné

Proměnné je objekt s názvem, který použijete k uložení dat. Zadáte název proměnné nic, ale název musí začínat znakem abecedy a nesmí obsahovat prázdné znaky nebo vyhrazené znaky.

### <a name="variables-and-data-types"></a>Datové typy a proměnné

Proměnná může mít specifické datového typu, který určuje, jaký typ dat je uložené v proměnné. Můžete mít proměnné řetězce, které ukládají řetězcové hodnoty (například &quot;Hello, world&quot;), celé číslo proměnné, které ukládají celé číslo hodnoty (například 3 nebo 79) a proměnné datum, které ukládání hodnot data v různých formátech (např. 4/12/2012 nebo březnem 2009 ). A existuje mnoho dalších typů dat, které můžete použít.

Ale obecně nemusíte zadejte typ pro proměnnou. Ve většině případů, ASP.NET můžete zjistit typ závislosti na tom, jak se používá data v proměnné. (Někdy nutné zadat typ, uvidíte příklady, pokud je to pravda.)

Deklarace proměnné pomocí `var` – klíčové slovo (Pokud nechcete zadat typ) nebo pomocí název typu:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

Následující příklad ukazuje některé typické použití proměnných na webové stránce:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

Pokud kombinujete v předchozích příkladech na stránce, zobrazí tento zobrazený v prohlížeči:

![Razor-Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Převod a testování datové typy

I když ASP.NET lze obvykle zjistit datový typ automaticky, někdy ji nelze. Proto můžete chtít Pomozte ASP.NET tím, že provádění explicitní převod. I když nemáte pro převod typů, někdy je užitečné zjistěte, jaký typ dat je možné, že pracujete s.

Nejběžnější případem je, že budete muset převést řetězec na jiný typ, například na celé číslo nebo datum. Následující příklad ukazuje obvyklý případ, kdy je nutné převést řetězec na číslo.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

Uživatelský vstup se dodává jako pravidlo, pro vás jako řetězce. I když jste výzva k uživatelům zadejte číslo, a i v případě číslici, že jste zadali při odeslání vstup uživatele a můžete číst v kódu, se data ve formátu řetězce. Proto je nutné převést řetězec na číslo. V příkladu Pokud se pokusíte provést aritmetické na hodnoty bez převodu, k následující chybě výsledky, protože ASP.NET nelze přidat dva řetězce:

*Typ 'řetězec' pro 'int, nelze implicitně převést.*

K převodu hodnoty na celá čísla, zavoláte `AsInt` metoda. Pokud převod úspěšný, můžete pak přidat čísla.

Následující tabulka uvádí některé běžné převod a testování metody pro proměnné.


|   <strong>– Metoda</strong>    |                                                                              <strong>Popis</strong>                                                                              |                         <strong>Příklad</strong>                         |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------|
|      `AsInt(), IsInt()`      |                                                      Převede řetězec, který představuje celé číslo (např. "593") na celé číslo.                                                      |   [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)]   |
|     `AsBool(), IsBool()`     |                                                    Převede řetězec jako &quot;true&quot; nebo &quot;false&quot; typu Boolean.                                                     |   [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)]   |
|    `AsFloat(), IsFloat()`    |                                    Převede řetězec, který má hodnotu decimal jako &quot;1.3&quot; nebo &quot;7.439&quot; na číslo s plovoucí desetinnou čárkou.                                    |   [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)]   |
|  `AsDecimal(), IsDecimal()`  | Převede řetězec, který má hodnotu decimal jako &quot;1.3&quot; nebo &quot;7.439&quot; na desetinné číslo. (V technologii ASP.NET, je přesnější než číslo s plovoucí desetinnou čárkou desetinné číslo.) |   [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)]   |
| `AsDateTime(), IsDateTime()` |                                                Převede řetězec představující hodnotu data a času ASP.NET `DateTime` typu.                                                 |   [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)]   |
|         `ToString()`         |                                                                       Převede ostatních typů dat na řetězec.                                                                        | [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)] |

## <a name="operators"></a>Operátory

Operátor je – klíčové slovo nebo znak, který sděluje ASP.NET, jaký druh příkaz k provedení ve výrazu. Jazyk C# (a syntaxe Razor, který je založený na něm) podporuje mnoho operátory, ale potřebujete rozpoznat pár začít pracovat. Následující tabulka shrnuje nejběžnější operátory.


|   <strong>Operátor</strong>    |                                                                     <strong>Popis</strong>                                                                     |                        <strong>Příklady</strong>                         |
|--------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------|
|        `+` `-` `*` `/`         |                                                            Matematické operátory použít ve výrazech číselná.                                                             |    [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)]    |
|              `=`               |                                    Přiřazení. Hodnota na pravé straně příkazu přiřadí objekt na levé straně.                                    |   [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)]   |
|              `==`              |                      Rovnosti. Vrátí `true` Pokud jsou hodnoty stejné. (Všimněte si rozdíl mezi `=` operátor a `==` operátor.)                      |   [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)]   |
|              `!=`              |                                                       Nerovnosti. Vrátí `true` Pokud hodnoty nejsou shodné.                                                        |   [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)]   |
|          `< > <= >=`           |                                               Menší – než, více – než menší než nebo rovno a větší než nebo rovno.                                                |   [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)]   |
|              `+`               | Zřetězení, který se používá pro připojení řetězce. ASP.NET zná rozdíl mezi tento operátor a operátor sčítání na základě typu dat, z výrazu. |   [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]   |
|           `+=``-=`            |                                   Přírůstek a snížení operátory, které sčítání a odečítání 1 (v uvedeném pořadí) z proměnné.                                    |   [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)]   |
|              `.`               |                                                  Tečkou. Používá k rozlišení objekty a jejich vlastnosti a metody.                                                  |   [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)]   |
|              `()`              |                                              Závorky. Použít na výrazy skupiny a předat parametry metody.                                               | [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)] |
|              `[]`              |                                                    Závorky. Použít pro přístup k hodnot v polích nebo kolekce.                                                     |   [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)]   |
|              `!`               |               Není. Obrátí `true` hodnotu `false` a naopak. Obvykle se používá jako sdružená způsob, jak otestovat `false` (který je pro není `true`).               |   [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]   |
| `&&`<code>&#124;&#124;</code> |                                                   Logické a a, nebo které se používají k propojení podmínky společně.                                                    |   [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]   |

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a>Práce s souboru a cesty ke složce zadat v kódu

Cesty k souborům a složkám budete často pracovat v kódu. Tady je příklad struktury fyzická složka pro web, jak se může objevit ve svém vývojovém počítači:

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

Zde jsou některé důležité podrobnosti o adresy URL a cesty:

- Adresa URL začíná, buď název domény (`http://www.example.com`) nebo název serveru (`http://localhost`, `http://mycomputer`).
- Adresa URL odpovídá na fyzickou cestu na hostitelském počítači. Například `http://myserver` může odpovídat do složky *C:\websites\mywebsite* na serveru.
- Virtuální cesta je sdružená představují cesty v kódu bez nutnosti zadávat úplnou cestu. Obsahuje části adresy URL, která následuje název domény nebo serveru. Pokud používáte virtuální cesty, můžete přesunout kódu do jiné domény nebo serveru bez nutnosti aktualizovat cesty.

Tady je příklad, které vám pomohou pochopit rozdíly:

| Úplnou adresu URL | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| název serveru | *mycompanyserver* |
| Virtuální cesta | */humanresources/CompanyPolicy.htm* |
| Fyzická cesta | *C:\mywebsites\humanresources\CompanyPolicy.htm* |

Je virtuální kořenový adresář, /, stejně jako kořenové jednotce C:, jednotka je \. (Virtuální složky cesty vždy použít lomítka.) Virtuální cesta ke složce nemusí mít stejný název jako fyzická složka; může být alias. (Na provozních serverech virtuální cestu zřídka odpovídá přesnou fyzickou cestu.)

Při práci se soubory a složky v kódu, někdy budete muset odkazovat na fyzickou cestu a někdy virtuální cesty, v závislosti na tom, jaké objekty, kterými pracujete. Technologie ASP.NET poskytuje tyto nástroje pro práci s cest k souborům a složkám v kódu: `Server.MapPath` metody a `~` operátor a `Href` metoda.

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a>Převod virtuálních a fyzických cest: Metoda Server.MapPath

`Server.MapPath` Metoda převede virtuální cestu (například */default.cshtml*) na absolutní fyzická cesta (jako *C:\WebSites\MyWebSiteFolder\default.cshtml*). Tuto metodu použijte vždy, když potřebujete úplnou fyzickou cestu. Typickým příkladem je při čtení nebo zápis textový soubor nebo soubor bitové kopie na webovém serveru.

Obvykle neznáte absolutní fyzická cesta vašeho webu na serveru pro hostování lokality, takže této metody můžete převést cestu si jisti – virtuální cestu – odpovídající cestu na serveru pro vás. Předat virtuální cestu k souboru nebo složce metodu a vrátí fyzickou cestu:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Odkazování na virtuální kořenový adresář: ~ operátor a metoda Href

V *.cshtml* nebo *.vbhtml* souboru, můžete odkazovat pomocí cesty virtuální kořenový adresář `~` operátor. To je velmi užitečný, protože můžete přesouváte stránky v lokalitě a všechny odkazy, které obsahují k ostatním stránkám nebudou přerušená. Je taky užitečný v případě, že přesunete někdy webu do jiného umístění. Následuje několik příkladů:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

Pokud je web `http://myserver/myapp`, zde je, jak bude ASP.NET považovat tyto cesty při spuštění stránky:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(Ve skutečnosti neuvidíte tyto cesty jako hodnoty proměnné, ale technologie ASP.NET bude považovat cesty, jako kdyby se, jak byly.)

Můžete použít `~` operátor v serverovém kódu (jak je uvedeno výše) a ve značkách takto:

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

V kódu, můžete použít `~` operátor vytvoření cesty k prostředkům, jako jsou soubory obrázků, jiné webové stránky a souborů CSS. Při spuštění stránky ASP.NET vypadá prostřednictvím stránky (kód a kód) a všechny přeloží `~` odkazy na příslušné cestě.

## <a name="conditional-logic-and-loops"></a>Podmíněnou logiku a smyčky

Kód serveru technologie ASP.NET umožňuje provádět úlohy na základě podmínek a napsat kód, který provede příkazy konkrétní počet (to znamená, kód, který běží smyčku).

### <a name="testing-conditions"></a>Testování podmínky

K otestování jednoduché podmínku můžete použít `if` příkaz, který vrátí hodnotu PRAVDA nebo NEPRAVDA podle testu zadáte:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

`if` – Klíčové slovo spustí blok. Skutečné test (podmínka) je v závorkách a vrátí hodnotu true nebo false. Příkazy, které spustit, pokud platí test jsou uzavřené do složených závorek. `if` Může zahrnovat příkaz `else` blok, který určuje příkazy ke spouštění, pokud je podmínka vyhodnocena jako false:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

Můžete přidat více podmínek použití `else if` bloku:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

V tomto příkladu, pokud je první podmínka vyhodnocena jako v oblasti, pokud blok není nastavena hodnota true, `else if` se kontroluje podmínku. Při splnění této podmínky je, příkazy v `else if` provedení bloku. Pokud žádná z podmínek jsou splněny, příkazy v `else` provedení bloku. Můžete přidat libovolný počet else if blokuje a pak zavřete s `else` blokovat jako &quot;všem ostatním&quot; podmínku.

Chcete-li otestovat velký počet podmínky, použijte `switch` bloku:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

Hodnota k testování je v závorkách (v příkladu `weekday` proměnné). Používá každé jednotlivé testy `case` příkaz, který končí dvojtečkou (:). Pokud hodnota `case` příkaz odpovídá hodnotě testu, spuštění kódu v tomto případu bloku. Zavřete každou case – příkaz s `break` příkaz. (Pokud zapomenete zahrnout rozdělení v každém `case` blok kódu z další `case` příkaz spustí také.) A `switch` bloku často má `default` příkaz jako poslední případ &quot;všem ostatním&quot; možnost, která spustí, pokud žádné z ostatních případech platí.

Výsledek posledních dvou podmíněné bloky zobrazený v prohlížeči:

![Razor-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a>Ve smyčce kódu

Potřebujete často opakovaně spouštět stejné příkazy. To provedete opakování ve smyčce. Například můžete často spustit stejné příkazy pro každou položku v kolekci dat. Pokud znáte přesně kolikrát chcete cykly, můžete použít `for` smyčky. Tento druh smyčky je zvláště užitečná pro počítání nebo počítání:

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

Smyčky začíná `for` – klíčové slovo, za nímž následuje tři příkazy v závorkách, každý ukončit středníkem.

- V závorkách, první příkaz (`var i=10;`) vytvoří čítače a inicializuje do 10. Nemáte název čítače `i` &#8212; můžete použít všechny proměnné. Když `for` spustí smyčky, hodnota čítače se zvýší automaticky.
- Druhý příkaz (`i < 21;`) nastaví podmínky pro jak daleko chcete spočítat. V takovém případě se má přejít na maximálně 20 (to znamená, pokračujte při čítač je menší než 21).
- Příkaz třetí (`i++` ) používá operátor přírůstku, která jednoduše Určuje, že čítač by měl mít 1 přidat k němu pokaždé, když běží smyčky.

Uvnitř závorek je kód, který se spustí pro každou iteraci smyčky. Kód vytvoří nový odstavec (`<p>` element) každou čas a přidá řádek do výstupu zobrazení hodnotu `i` (Čítač). Při spuštění této stránce v příkladu se vytváří 11 řádky zobrazení výstupu s textem do každého řádku, která určuje počet položek.

![Razor-Img11](introducing-razor-syntax-c/_static/image11.jpg)

Pokud pracujete s kolekce nebo pole, často používají `foreach` smyčky. Kolekce je skupina podobné objekty a `foreach` cykly umožňuje provádět úlohy na každou položku v kolekci. Tento typ smyčky je vhodné pro kolekce, protože na rozdíl od `for` smyčky, nemusíte zvýší čítače nebo nastavit limit. Místo toho `foreach` smyčky kód jednoduše pokračuje prostřednictvím kolekce, dokud se nedokončí jeho.

Například následující kód vrátí položky v `Request.ServerVariables` kolekce, která je objekt, který obsahuje informace o vašem webovém serveru. Použije `foreac` h smyčky, zobrazí se název každé položky tak, že vytvoříte novou `<li>` element v seznamu s odrážkami HTML.

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

`foreach` – Klíčové slovo následuje závorkách kde deklarace proměnné, která představuje jednu položku v kolekci (v příkladu `var item`), za nímž následují `in` – klíčové slovo, za nímž následuje kolekce, kterou chcete projít. V textu `foreach` smyčky, dostanete s aktuální položkou použitím proměnné, kterou jste předtím deklarován.

![Razor-Img12](introducing-razor-syntax-c/_static/image12.jpg)

Chcete-li vytvořit více pro obecné účely smyčku, použijte `while` příkaz:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

A `while` smyčky začíná `while` – klíčové slovo, následuje závorky, kde můžete určit, jak dlouho bude pokračovat smyčky (sem pro tak dlouho, dokud `countNum` je menší než 50), pak bloku opakování. Obvykle zvýšit smyčky (přidejte) nebo snížení (odečíst od) proměnnou nebo objekt použitý pro počítání. V příkladu `+=` operátor přidá 1 `countNum` pokaždé, když běží smyčky. (Chcete-li snížení proměnné ve smyčce počty dolů, použijte operátor snížení `-=`).

## <a name="objects-and-collections"></a>Objekty a kolekce

Téměř všechno ve webové stránky ASP.NET je objekt, včetně webové stránky. Tato část popisuje některé důležité objekty, se kterými budete pracovat často v kódu.

### <a name="page-objects"></a>Objekty stránky

Objekt nejzákladnější technologie ASP.NET je stránka. Můžete přistoupit k vlastnostem objektu page přímo bez žádné opravňující objektu. Následující kód získá cesta k souboru stránky, pomocí `Request` objekt stránky:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

Aby bylo jasné jste odkazování vlastnosti a metody pro aktuální objekt stránky, můžete volitelně použít klíčové slovo `this` k reprezentaci objektu stránky ve vašem kódu. Tady je předchozí příklad kódu s `this` přidány do představují stránky:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

Můžete použít vlastnosti `Page` objekt, který chcete získat velké množství informací, jako například:

- `Request`. Jak už jsme viděli, to je kolekce informací o aktuálním požadavku, včetně, jaký typ prohlížeče požadavek, adresu URL stránky, identita uživatele, atd.
- `Response`. Toto je kolekce informací o odpovědi (stránky), která se odešle do prohlížeče při kódu serveru bylo dokončeno. Například můžete tuto vlastnost při zápisu informací do odpovědi. 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a>Kolekce objektů (pole a slovník)

A *kolekce* je skupina objekty stejného typu, například v kolekci `Customer` objekty z databáze. ASP.NET obsahuje mnoho předdefinovaných kolekcí, jako je třeba `Request.Files` kolekce.

Často budete pracovat dat v kolekcích. Jsou dvě běžné typy kolekcí *pole* a *slovník*. Pole je užitečné, když chcete ukládat kolekce podobné položky, ale nechcete vytvořit samostatnou proměnnou pro každou položku:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

S poli, je třeba deklarovat určité datového typu, `string`, `int`, nebo `DateTime`. K označení, proměnná může obsahovat pole, přidat závorky deklarace (například `string[]` nebo `int[]`). Dostanete položky v poli pomocí jejich polohu (index) nebo pomocí `foreach` příkaz. Indexy pole jsou od nuly &#8212; tedy první položka je v umístění 0, druhý položka je na pozici 1 a tak dále.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

Můžete určit počet položek v matici získáním jeho `Length` vlastnost. Pozice konkrétní položku v poli (pro vyhledávání pole), použijte `Array.IndexOf` metoda. Můžete také provést třeba zpětného obsah pole ( `Array.Reverse` metoda) nebo řazení obsahu ( `Array.Sort` metoda).

Výstupní kód pole řetězec, který je zobrazený v prohlížeči:

![Razor-Img13](introducing-razor-syntax-c/_static/image13.jpg)

Slovník je kolekce dvojic klíč/hodnota, kde zadáte klíč (nebo název) nastavit nebo načíst s odpovídající hodnotou:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

Chcete-li vytvořit slovník, je použít `new` – klíčové slovo indikující, že vytváříte nový objekt slovníku. Slovník můžete přiřadit k proměnné pomocí `var` – klíčové slovo. Určují typy položek ve slovníku pomocí lomené závorky ( `< >` ). Na konci tohoto prohlášení je nutné přidat pár závorky, protože to je ve skutečnosti metoda, která vytvoří do nového slovníku.

Chcete-li přidat položky do slovníku, můžete zavolat `Add` metoda slovník proměnné (`myScores` v tomto případě) a pak zadejte klíč a hodnotu. Alternativně můžete hranaté závorky znamenat klíč a provádět jednoduché přiřazení, jako v následujícím příkladu:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

Získá hodnotu ze slovníku, zadejte klíč do hranatých závorek:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a>Volání metody s parametry

Při čtení dříve v tomto článku, může mít objekty, které můžete naprogramovat s metody. Například `Database` objekt může mít `Database.Connect` metoda. Mnoho způsobů také mít jeden nebo více parametrů. A *parametr* je hodnota, kterou předáte metodě povolení metody a dokončení úkolu. Například, podívejte se na deklaraci pro `Request.MapPath` metodu, která přijímá tři parametry:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

(Na řádku byl zalomen aby byl čitelnější. Mějte na paměti, které můžete vložit konce řádků téměř žádné umístit s výjimkou uvnitř řetězců, které jsou uzavřené v uvozovkách.)

Tato metoda vrátí fyzickou cestu na serveru, který odpovídá do zadané virtuální cesty. Jsou tři parametry pro metodu `virtualPath`, `baseVirtualDir`, a `allowCrossAppMapping`. (Všimněte si, že v deklaraci, jsou parametry uvedené s datovými typy dat, která budete přijmou.) Když zavoláte tuto metodu, je nutné zadat hodnoty pro všechny tři parametry.

Syntaxe Razor nabízí dvě možnosti pro předávání parametrů pro metodu: *poziční parametry* a *pojmenované parametry*. Pro volání metody pomocí poziční parametry, předáte parametry striktní pořadí, které je zadaný v deklaraci metoda. (By obvykle víte, toto pořadí načtením dokumentace pro metodu.) Je třeba postupovat podle pořadí, a přeskočíte nelze některý z parametrů &#8212; Pokud potřeby předáte prázdný řetězec (`""`) nebo `null` pro poziční nemáte hodnotu pro parametr.

Následující příklad předpokládá, že máte složku s názvem *skripty* na vašem webu. Volání kódu `Request.MapPath` metoda a předává hodnoty pro tři parametry ve správném pořadí. Pak zobrazí výsledná namapované cesta.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

Když metoda má mnoho parametrů, můžete ponechat kódu srozumitelnější pomocí pojmenované parametry. Chcete-li zavolat metodu pomocí pojmenované parametry, zadáte název parametru následovaný dvojtečkou (:) a hodnotu. Výhodou pojmenované parametry je, že můžete je předat v libovolném pořadí, které chcete. (Nevýhodou je, že volání metody není jako compact.)

V následujícím příkladu volání stejnou metodu, jak je uvedeno výše, ale používá pojmenované parametry k zadání hodnoty:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

Jak je vidět parametry se jí předávají v jiném pořadí. Pokud spustíte předchozí příklad a v tomto příkladu, budete však vrátit stejnou hodnotu.

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a>Zpracování chyb

### <a name="try-catch-statements"></a>Try-Catch – příkazy

Příkazy často budete mít ve svém kódu, který může selhat z důvodů mimo vlastního ovládacího prvku. Příklad:

- Pokud váš kód se pokusí vytvořit nebo získat přístup souboru, může dojít k nejrůznějším chyb. Požadovaný soubor pravděpodobně neexistuje, může být zablokován, kód nemusí mít oprávnění a tak dále.
- Podobně pokud kód pokusí k aktualizaci záznamů v databázi, může být problémy s oprávněními, může dojít ke ztrátě připojení k databázi, data uložit může být neplatný a tak dále.

Programovací podmínky, se nazývají těchto situacích *výjimky*. Pokud váš kód zjistí výjimku, generuje (vyvolává) chybovou zprávu to je v nejlépe obtěžování uživatelům:

![Razor-Img14](introducing-razor-syntax-c/_static/image14.jpg)

V situacích, kde kódu setkat výjimky a aby se zabránilo chybové zprávy tohoto typu, můžete použít `try/catch` příkazy. V `try` prohlášení, můžete spustit kód, který při kontrole. V jedné nebo více `catch` příkazy, můžete vyhledat konkrétní chyby (konkrétní typy výjimek), jež mohly nastat. Můžete zahrnout tolik `catch` příkazy, jako je třeba vyhledejte chyby, které se očekává.

> [!NOTE]
> Doporučujeme, abyste neměli používat `Response.Redirect` metoda v `try/catch` příkazy, protože ho může způsobit výjimku v stránku.


Následující příklad ukazuje stránky, který vytvoří textový soubor na první požadavek a potom zobrazí tlačítko, které umožňuje uživateli otevřít soubor. V příkladu úmyslně použije chybný název souboru, takže způsobí výjimku. Tento kód obsahuje `catch` příkazy pro dvě možné výjimky: `FileNotFoundException`, která nastane, pokud je špatná, název souboru a `DirectoryNotFoundException`, který v případě ASP.NET i nelze najít složku. (Chcete-li zobrazit, jak se spustí při všechno funguje správně můžete Odkomentujte příkaz v příkladu.)

Pokud váš kód nebyl zpracovat výjimku, zobrazí se chybová stránka jako předchozí snímek obrazovky. Ale `try/catch` části pomáhá zabránit uživateli v zobrazení tyto typy chyb.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a>Další prostředky

**Programování v jazyce Visual Basic**


[Dodatek: jazyk Visual Basic a syntaxe](https://go.microsoft.com/fwlink/?LinkId=202908)


**Referenční dokumentace**


[ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)

[Jazyk C#](https://msdn.microsoft.com/library/kx37x362.aspx)
