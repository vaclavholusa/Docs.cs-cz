---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: "Úvod do rozhraní ASP.NET Web programování pomocí syntaxe Razor (Visual Basic) | Microsoft Docs"
author: tfitzmac
description: "Tento dodatek poskytuje přehled programování s webovými stránkami ASP.NET v jazyce Visual Basic pomocí syntaxe Razor."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: 8f5d223a5944d8adb9fe65c89e87829d18d1c7ee
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a>Úvod do rozhraní ASP.NET Web programování pomocí syntaxe Razor (Visual Basic)
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento článek poskytuje přehled programování pomocí webových stránek ASP.NET pomocí syntaxe Razor a Visual Basic. ASP.NET je technologie společnosti Microsoft pro spouštění dynamické webové stránky na webové servery.
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


Většina příkladů použití technologie ASP.NET Web Pages se syntaxí Razor pomocí jazyka C#. Ale syntaxi Razor podporuje také jazyka Visual Basic. Chcete-li program webovou stránku ASP.NET v jazyce Visual Basic, vytvoříte webovou stránku pomocí *.vbhtml* příponu názvu souboru a poté přidejte kód jazyka Visual Basic. Tento článek poskytuje přehled o práci s jazykem Visual Basic a syntaxe k vytvoření webových stránek ASP.NET ve službě.

> [!NOTE]
> Výchozí šablony webové stránky pro nástroj Microsoft WebMatrix (**Pekárna**, **Fotogalerie**, a **Starter Site**atd) jsou k dispozici v C# a Visual Basic verze. Visual Basic šablony podle můžete nainstalovat jako balíčky NuGet. Šablony webu jsou nainstalovány v kořenové složce webu do složky s názvem *Microsoft Templates*.


## <a name="the-top-8-programming-tips"></a>Horní 8 Tipy pro programování

Tato část obsahuje několik tipy, které je nezbytně nutné znát jako počáteční psaní kódu serveru ASP.NET pomocí syntaxe Razor.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Přidejte kód na stránku pomocí @ – znak

`@` Začne znak vložené výrazy, jedním příkazem bloky a bloky vícepříkazové:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

Výsledek zobrazí v prohlížeči:

![Razor-Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> **HTML Encoding**
> 
> Při zobrazení obsahu stránce pomocí `@` znak, jako v předchozích příkladech ASP.NET umístí kódování HTML výstup. Tím se nahradí vyhrazené znaky HTML (například `<` a `>` a `&`) s kódy, které umožňují znaků, který má být zobrazen jako znaků na webové stránce nebude interpretován jako značky HTML nebo entity. Bez kódování HTML výstup z vašeho kódu serveru se nemusí zobrazit správně a mohla vystavit stránky na bezpečnostní rizika.
> 
> Pokud vaším cílem je výstup jazyka HTML, který vykreslí značky jako značky (například `<p></p>` pro odstavec nebo `<em></em>` pro zvýraznění textu), najdete v části [kombinování Text, značek a kódu v bloky kódu](#BM_CombiningTextMarkupAndCode) dále v tomto článku.
> 
> Další informace o kódování HTML v [práci s formuláři HTML v lokalitách webové stránky ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202892).


### <a name="2-you-enclose-code-blocks-with-codeend-code"></a>2. Je-li uzavřít bloky kódu s kódem... End kódu

Blok kódu obsahuje jeden nebo více příkazů kódu a je spolu s klíčovými slovy `Code` a `End Code`. Umístěte otevření `Code` – klíčové slovo ihned po `@` znak &#8212; mezi nimi nemůže být prázdné.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

Výsledek zobrazí v prohlížeči:

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a>3. Uvnitř bloku vám stát, že každý příkaz kódu s konec řádku

V bloku kódu jazyka Visual Basic každý příkaz končí konec řádku. (Dále v článku uvidíte způsob, jak zabalit příkaz dlouho kódu do více řádků v případě potřeby.)

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a>4. Proměnné se používají k ukládání hodnot

Hodnoty v lze ukládat *proměnná*, včetně řetězce, čísla a kalendářní data, atd. Vytvoření nové proměnné pomocí `Dim` – klíčové slovo. Hodnoty proměnných můžete vložit přímo do stránky pomocí `@`.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

Výsledek zobrazí v prohlížeči:

![Razor-Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. Uzavřete řetězcového literálu hodnoty v uvozovkách

A *řetězec* je posloupnost znaků, které jsou považovány za text. Pokud chcete zadat řetězec, vložte jej do dvojitých uvozovek:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

Pro vložení dvojitých uvozovek nahoře v rámci řetězcovou hodnotu, vložte dva znaky uvozovky. Pokud chcete znak dvojitých uvozovek jednou zobrazí ve výstupu stránky, zadejte je jako `""` v rámci uvozovkách řetězec a pokud chcete zobrazit dvakrát, zadejte je jako `""""` v rámci řetězec v uvozovkách.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

Výsledek zobrazí v prohlížeči:

![Razor Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a>6. Kód jazyka Visual Basic není malá a velká písmena

Jazyk Visual Basic není velká a malá písmena. Klíčová slova programování (jako je `Dim`, `If`, a `True`) a názvy proměnných (jako je `myString`, nebo `subTotal`) může být napsán v každém případě.

Následující řádky kódu přiřadit hodnotu proměnné `lastname` pomocí jedno malé písmeno název a hodnotu proměnné na stránku pomocí velká název ve výstupu.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

Výsledek zobrazí v prohlížeči:

![vb-syntax-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a>7. Velká část kódování zahrnuje pracovat s objekty

Představuje objekt věcí, kterou můžete programu s &#8212; na stránce textové pole, soubor, bitovou kopii, webové žádosti, e-mailovou zprávu, záznam zákazníka (řádku databáze), atd. Objekty mají vlastnosti, které popisují jejich charakteristikami &#8212; má objekt textové pole `Text` vlastnost, má objekt žádosti `Url` vlastnost, má e-mailovou zprávu `From` vlastnost a objekt zákazník má `FirstName` vlastnost. Objekty mají metody, které jsou také &quot;příkazy&quot; mohou provádět. Mezi příklady patří objektu soubor `Save` metoda, objekt image `Rotate` metoda a objekt e-mailu `Send` metoda.

Často budete pracovat `Request` objekt, který nabízí informace jako hodnoty formuláře polí na stránce (textová pole, atd.), jaký typ prohlížeče požadavek, adresu URL stránky, identita uživatele, atd. Tento příklad ukazuje, jak pro přístup k vlastnostem `Request` objekt a jak volat `MapPath` metodu `Request` objekt, který vám dává absolutní cesta ke stránce na serveru:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

Výsledek zobrazí v prohlížeči:

![Razor-Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. Můžete napsat kód, který uskutečňuje rozhodnutí

Klíčovou funkcí dynamické webové stránky je, že můžete určit co dělat na základě podmínky. Nejběžnější způsob k tomu je s `If` – příkaz (a volitelné `Else` příkaz).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

Příkaz `If IsPost` je zjednodušený způsob zápis `If IsPost = True`. Spolu s `If` příkazy, existuje mnoho různých způsobů, jak testovacích podmínkách, opakování bloky kódu, a tak dále, které jsou popsány dále v tomto článku.

Výsledek zobrazí v prohlížeči (po kliknutí na **odeslání**):

![Razor Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> **HTTP GET a POST metody a vlastnosti IsPost**
> 
> Protokol použitý pro webové stránky (HTTP) podporuje velmi omezený počet metod (&quot;příkazy&quot;) používané provádět požadavky na server. Dva nejběžnější ty, které jsou GET, které slouží k načtení stránky, a POST, který se používá k odeslání stránky. Obecně platí při prvním požadavku na stránce požadavku na stránku pomocí GET. Pokud uživatel doplní do formuláře a poté klikne na tlačítko **odeslání**, prohlížeč provede požadavek POST na server.
> 
> Ve webové programování je často užitečné vědět, jestli stránky je vyžadován jako GET nebo POST, abyste věděli, jak pro zpracování stránky. Na webových stránkách ASP.NET, můžete použít `IsPost` vlastnosti chcete zobrazit, zda je požadavek GET nebo POST. Pokud je požadavek POST, `IsPost` vlastnost vrátí hodnotu true, a můžete provést třeba číst hodnoty textová pole ve formuláři. Uvidíte mnoho příklady ukazují, jak zpracovat stránce odlišně v závislosti na hodnotě `IsPost`.


## <a name="a-simple-code-example"></a>Jednoduchým příkladem kódu

Tento postup ukazuje, jak vytvořit stránku, která ukazuje základní programovací techniky. V příkladu vytvoříte stránky, který umožňuje uživatelům zadejte dvě čísla, potom se přidají a zobrazuje výsledek.

1. V editoru, vytvořte nový soubor s názvem *AddNumbers.vbhtml*.
2. Zkopírujte následující kód a kód na stránku nahraďte nic již na stránce.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    Zde jsou některé kroky, abyste Všimněte si:

    - `@` Znak spustí první blok kódu na stránce, a předchází `totalMessage` proměnná vložených v dolní.
    - Blok v horní části stránky je uzavřen v `Code...End Code`.
    - Proměnné `total`, `num1`, `num2`, a `totalMessage` uložit několik čísla a řetězec.
    - Řetězcový literál hodnota přiřazená `totalMessage` proměnná je v uvozovkách.
    - Protože kód jazyka Visual Basic není velká a malá písmena, když `totalMessage` proměnná se používá v dolní části stránky, jeho název pouze musí odpovídat pravopis deklarace proměnné v horní části stránky. Jedno, malá a velká písmena.
    - Výraz `num1.AsInt()`  +  `num2.AsInt()` ukazuje, jak pracovat s objekty a metody. `AsInt` Metodu na každou proměnnou převede řetězec zadaný uživatelem na celé číslo (integer), který lze přidat.
    - `<form>` Zahrnuje značky `method="post"` atribut. To určuje, že když uživatel klikne **přidat**, stránky odešle na server pomocí metody POST protokolu HTTP. Při odeslání stránky, kód `If IsPost` vyhodnocena jako true a podmínku kód běží, zobrazení výsledkem přidání čísla.
3. Uložit stránky a spusťte ji v prohlížeči. (Ujistěte se, že je vybraný stránky v **soubory** pracovního prostoru, než ji spustit.) Zadejte dvě celá čísla a poté klikněte **přidat** tlačítko.

    ![Razor-Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a>Jazyk Visual Basic a syntaxe

Dříve jste viděli základních příkladů jak vytvořit webovou stránku ASP.NET a přidání serverový kód pro kód HTML. Zde se dozvíte základní informace o použití jazyka Visual Basic pro psaní kódu serveru ASP.NET pomocí syntaxe Razor &#8212; To znamená, programovací jazyk pravidla.

Pokud máte zkušenosti s programováním (obzvláště pokud jste použili C, C++, C#, Visual Basic nebo JavaScript), většinu si přečíst zde bude seznámit. Pravděpodobně budete muset Seznamte se jenom s jak služba WebMatrix kód je přidán do značek v *.vbhtml* soubory.

### <a id="BM_CombiningTextMarkupAndCode"></a>Kombinování text, značek a kódu v bloky kódu

V blocích kód serveru budete často chtít výstup text a značku na stránku. Pokud blok serverového kódu obsahuje text, který není kódu a který místo toho má být vykreslen jako je, musí být schopen rozlišit tento text z kódu ASP.NET. Chcete-li to provést několika způsoby.

- Uzavřete text v elementu HTML bloku jako `<p></p>` nebo `<em></em>`:

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    Prvek HTML mohou zahrnovat text, další prvky HTML a výrazy kódu serveru. Když ASP.NET uvidí otevírání značky HTML (například `<p>`), vykreslí všechno elementu a jeho obsahu, jako je prohlížeče (a překládá výrazy kódu serveru).

- Použití `@:` operátor nebo `<text>` elementu. `@:` Výstupy jeden řádek obsahující prostý text nebo neodpovídající značky HTML; obsahu `<text>` element uzavře více řádků na výstup. Tyto možnosti jsou užitečné, když nechcete, aby k vykreslení elementu HTML v rámci výstupu.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    Následující příklad opakuje předchozí příklad, ale používá jednu dvojici `<text>` značek k uzavřete text k vykreslení.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    V následujícím příkladu `<text>` a `</text>` značky uzavřete tři řádky, které mají všechny některé uncontained text a neodpovídající značky HTML (`<br />`), spolu s kódu serveru a odpovídající značky HTML. Znovu, může také předcházet každý řádek jednotlivě s `@:` operátor; buď způsob, jak funguje.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > Pokud výstupní text jak je znázorněno v této části &#8212; pomocí prvku HTML `@:` operátor, nebo `<text>` element &#8212; Technologie ASP.NET není kódování HTML výstup. (Jak je uvedeno výše, ASP.NET kódování výstup výrazy kódu serveru a bloky kódu serveru, které jsou označeny `@`, s výjimkou ve zvláštních případech uvedených v této části.)

### <a name="whitespace"></a>Whitespace

Mezery v příkazu (a mimo řetězcový literál) není ovlivňují příkaz:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a>Rozdělení dlouhých příkazů do více řádků

Příkaz dlouho kódu můžete rozdělit na více řádků pomocí znak podtržítka `_` (v jazyce Visual Basic tzv. *znak pro pokračování*) po každém řádku kódu. Pokud chcete rozdělit příkaz na další řádek, na konci řádku přidejte mezeru a poté znak pro pokračování. Příkaz pokračujte na další řádek. Může obtékat příkazy na počet řádků, je potřeba pro zlepšení čitelnosti. Následující příkazy jsou stejné:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

Však nelze obalit řádku uprostřed řetězcový literál. Předchozí postup nebude fungovat v následujícím příkladu:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

Kombinování dlouhý řetězec, který zabalí do více řádků jako výše uvedený kód, museli byste používat *operátor řetězení* (`&`), který se zobrazí později v tomto článku.

### <a name="code-comments"></a>Komentáře kódu

Komentáře umožňují nechte poznámky pro sebe nebo ostatní. Komentáře syntaxe Razor mají předponu `@*` a končit `*@`.

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

V rámci bloků kódu komentáře syntaxe Razor můžete použít, nebo můžete použít obyčejnou jazyka Visual Basic komentář znaku, který je v jednoduchých uvozovkách (`'`) předponu na každém řádku.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a>Proměnné

Proměnné je objekt s názvem, který použijete k uložení dat. Zadáte název proměnné nic, ale název musí začínat znakem abecedy a nesmí obsahovat prázdné znaky nebo vyhrazené znaky. V jazyce Visual Basic jak jste viděli dříve, malá a velká písmena v názvu proměnné jedno.

### <a name="variables-and-data-types"></a>Proměnné a datové typy

Proměnná může mít specifické datového typu, který určuje, jaký typ dat je uložené v proměnné. Můžete mít proměnné řetězce, které ukládají řetězcové hodnoty (například &quot;Hello, world&quot;), celé číslo proměnné, které ukládají celé číslo hodnoty (například 3 nebo 79) a proměnné datum, které ukládání hodnot data v různých formátech (např. 4/12/2012 nebo březnem 2009 ). A existuje mnoho dalších typů dat, které můžete použít.

Nemáte však zadejte typ pro proměnnou. Ve většině případů ASP.NET můžete zjistit typ závislosti na tom, jak se používá data v proměnné. (Někdy nutné zadat typ, uvidíte příklady, pokud je to pravda.)

Chcete-li deklarovat proměnnou bez zadání typu, použijte `Dim` plus název proměnné (například `Dim myVar`). Chcete-li proměnnou deklarovat s typem, použijte `Dim` plus název proměnné, za nímž následuje `As` a potom název typu (například `Dim myVar As String`).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

Následující příklad ukazuje některé vložené výrazy, které používají proměnné na webové stránce.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

Výsledek zobrazí v prohlížeči:

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Převod a testování datové typy

I když ASP.NET lze obvykle zjistit datový typ automaticky, někdy ji nelze. Proto můžete chtít Pomozte ASP.NET tím, že provádění explicitní převod. I když nemáte pro převod typů, někdy je užitečné zjistěte, jaký typ dat je možné, že pracujete s.

Nejběžnější případem je, že budete muset převést řetězec na jiný typ, například na celé číslo nebo datum. Následující příklad ukazuje obvyklý případ, kdy je nutné převést řetězec na číslo.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

Uživatelský vstup se dodává jako pravidlo, pro vás jako řetězce. I když jste zobrazí výzva, aby uživatel zadal číslo a i v případě číslici, že jste zadali při odeslání vstup uživatele a můžete číst v kódu, se data ve formátu řetězce. Proto je nutné převést řetězec na číslo. V příkladu Pokud se pokusíte provést aritmetické na hodnoty bez převodu, k následující chybě výsledky, protože ASP.NET nelze přidat dva řetězce:

`Cannot implicitly convert type 'string' to 'int'.`

K převodu hodnoty na celá čísla, zavoláte `AsInt` metoda. Pokud převod úspěšný, můžete pak přidat čísla.

Následující tabulka uvádí některé běžné převod a testování metody pro proměnné.

| **– Metoda** | **Popis** | **Příklad** |
| --- | --- | --- |
| `AsInt(), IsInt()` | Převede řetězec, který představuje celé číslo (například &quot;593&quot;) na celé číslo. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)] |
| `AsBool(), IsBool()` | Převede řetězec jako &quot;true&quot; nebo &quot;false&quot; typu Boolean. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)] |
| `AsFloat(), IsFloat()` | Převede řetězec, který má hodnotu decimal jako &quot;1.3&quot; nebo &quot;7.439&quot; na číslo s plovoucí desetinnou čárkou. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)] |
| `AsDecimal(), IsDecimal()` | Převede řetězec, který má hodnotu decimal jako &quot;1.3&quot; nebo &quot;7.439&quot; na desetinné číslo. (V technologii ASP.NET, je přesnější než číslo s plovoucí desetinnou čárkou desetinné číslo.) | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)] |
| `AsDateTime(), IsDateTime()` | Převede řetězec představující hodnotu data a času ASP.NET `DateTime` typu. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)] |
| `ToString()` | Převede ostatních typů dat na řetězec. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)] |

## <a name="operators"></a>Operátory

Operátor je – klíčové slovo nebo znak, který sděluje ASP.NET, jaký druh příkaz k provedení ve výrazu. Podporuje mnoho operátory jazyka Visual Basic, ale potřebujete rozpoznat mají-li začít s vývojem webových stránek ASP.NET. Následující tabulka shrnuje nejběžnější operátory.

| **Operátor** | **Popis** | **Příklady** |
| --- | --- | --- |
| `+ - * /` | Matematické operátory použít ve výrazech číselná. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)] |
| `=` | Přiřazení a rovnosti. V závislosti na kontextu buď hodnota na pravé straně příkazu přiřadí objekt na levé straně nebo kontroluje hodnoty rovnosti. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)] |
| `<>` | Nerovnosti. Vrátí `True` Pokud hodnoty nejsou shodné. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)] |
| `< > <= >=` | Menší než větší, menší nebo rovna a větší než nebo rovno. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)] |
| `&` | Zřetězení, který se používá pro připojení řetězce. | [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)] |
| `+= -=` | Přírůstek a snížení operátory, které sčítání a odečítání 1 (v uvedeném pořadí) z proměnné. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)] |
| `.` | Tečkou. Používá k rozlišení objekty a jejich vlastnosti a metody. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)] |
| `()` | Závorky. Použít na výrazy skupiny předat parametry metody a členy přístup pole a kolekcí. | [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)] |
| `Not` | Není. Obrátí hodnotu true na false a naopak. Obvykle se používá jako sdružená způsob, jak otestovat `False` (který je pro není `True`). | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)] |
| `AndAlso OrElse` | Logické a a, nebo které se používají k propojení podmínky společně. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)] |

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

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Odkazování na virtuální kořenový adresář: ~ operátor a metoda Href

V *.cshtml* nebo *.vbhtml* souboru, můžete odkazovat pomocí cesty virtuální kořenový adresář `~` operátor. To je velmi užitečný, protože můžete přesouváte stránky v lokalitě a všechny odkazy, které obsahují k ostatním stránkám nebudou přerušená. Je taky užitečný v případě, že přesunete někdy webu do jiného umístění. Následuje několik příkladů:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

Pokud je web `http://myserver/myapp`, zde je, jak bude ASP.NET považovat tyto cesty při spuštění stránky:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(Ve skutečnosti neuvidíte tyto cesty jako hodnoty proměnné, ale technologie ASP.NET bude považovat cesty, jako kdyby se, jak byly.)

Můžete použít `~` operátor v serverovém kódu (jak je uvedeno výše) a ve značkách takto:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

V kódu, můžete použít `~` operátor vytvoření cesty k prostředkům, jako jsou soubory obrázků, jiné webové stránky a souborů CSS. Při spuštění stránky ASP.NET vypadá prostřednictvím stránky (kód a kód) a všechny přeloží `~` odkazy na příslušné cestě.

## <a name="conditional-logic-and-loops"></a>Podmíněnou logiku a smyčky

Kód serveru technologie ASP.NET umožňuje provádět úlohy na základě ujednání a zápisu kód opakující se příkazy konkrétní počet tedy kód, který běží smyčku).

### <a name="testing-conditions"></a>Testování podmínky

K otestování jednoduché podmínku můžete použít `If...Then` příkaz, který vrací `True` nebo `False` podle testu zadáte:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

`If` – Klíčové slovo spustí blok. Dodržuje skutečný test (podmínka) `If` – klíčové slovo a vrátí hodnotu true nebo false. `If` Příkaz končí `Then`. Příkazy, které se spustí, pokud je testu hodnota true, jsou uvedeny v `If` a `End If`. `If` Může zahrnovat příkaz `Else` blok, který určuje příkazy ke spouštění, pokud je podmínka vyhodnocena jako false:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

Pokud `If` příkaz spustí blok kódu, nemusíte používat normální `Code...End Code` příkazy k zahrnutí bloků. Můžete přidat pouze `@` blok, a bude fungovat. Tento postup funguje s `If` a také další jazyka Visual Basic programování klíčová slova, která následuje za bloky kódu, včetně `For`, `For Each`, `Do While`atd.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

Můžete přidat více podmínek použití jednoho nebo více `ElseIf` bloky:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

V tomto příkladu, pokud je první podmínka vyhodnocena jako v `If` blok není nastavena hodnota true, `ElseIf` se kontroluje podmínku. Při splnění této podmínky je, příkazy v `ElseIf` provedení bloku. Pokud žádná z podmínek jsou splněny, příkazy v `Else` provedení bloku. Můžete přidat libovolný počet `ElseIf` blokuje a pak zavřete s `Else` blokovat jako &quot;všem ostatním&quot; podmínku.

Chcete-li otestovat velký počet podmínky, použijte `Select Case` bloku:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

Hodnota k testování je v závorkách (v příkladu proměnnou den v týdnu). Používá každé jednotlivé testy `Case` příkaz, který uvádí hodnotu. Pokud hodnota `Case` příkaz odpovídá hodnotě testovací kód v tom, že `Case` bloku se spustí.

Výsledek posledních dvou podmíněné bloky zobrazený v prohlížeči:

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a>Ve smyčce kódu

Potřebujete často opakovaně spouštět stejné příkazy. To provedete opakování ve smyčce. Například můžete často spustit stejné příkazy pro každou položku v kolekci dat. Pokud znáte přesně kolikrát chcete cykly, můžete použít `For` smyčky. Tento druh smyčky je zvláště užitečná pro počítání nebo počítání:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

Smyčky začíná `For` – klíčové slovo, za nímž následuje tři prvky:

- Ihned po `For` příkaz deklarace proměnné čítače (nemusíte používat `Dim`) a potom rozsahu, jako v `i = 10 to 20`. To znamená proměnnou `i` bude spustit počítání na 10 a bude pokračovat, dokud nebude dosaženo 20 (včetně).
- Mezi `For` a `Next` příkazy je obsah bloku. To může obsahovat jeden nebo více kód příkazů, které provést s každou smyčku.
- `Next i` Příkaz ukončení smyčky. To zvýší čítače a spustí další iteraci smyčky.

Řádek kódu mezi `For` a `Next` řádky obsahuje kód, který spouští pro každou iteraci smyčky. Kód vytvoří nový odstavec (`<p>` element) každou čas a přidá řádek do výstupu zobrazení hodnotu i (Čítač). Při spuštění této stránce v příkladu se vytváří 11 řádky zobrazení výstupu s textem do každého řádku, která určuje počet položek.

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

Pokud pracujete s kolekce nebo pole, často používají `For Each` smyčky. Kolekce je skupina podobné objekty a `For Each` cykly umožňuje provádět úlohy na každou položku v kolekci. Tento typ smyčky je vhodné pro kolekce, protože na rozdíl od `For` smyčky, nemusíte zvýší čítače nebo nastavit limit. Místo toho `For Each` smyčky kód jednoduše pokračuje prostřednictvím kolekce, dokud se nedokončí jeho.

Vrátí položky v tomto příkladu `Request.ServerVariables` kolekce (která obsahuje informace o vašem webovém serveru). Použije `For Each` smyčky a zobrazí se název každé položky tak, že vytvoříte novou `<li>` element v seznamu s odrážkami HTML.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

`For Each` – Klíčové slovo následuje proměnné, která představuje jednu položku v kolekci (v příkladu `myItem`), za nímž následují `In` – klíčové slovo, za nímž následuje kolekce, kterou chcete projít. V textu `For Each` smyčky, dostanete s aktuální položkou použitím proměnné, kterou jste předtím deklarován.

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

Chcete-li vytvořit více pro obecné účely smyčku, použijte `Do While` příkaz:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

Tato smyčka začíná `Do While` – klíčové slovo, za nímž následuje podmínku, za nímž následuje bloku opakování. Obvykle zvýšit smyčky (přidejte) nebo snížení (odečíst od) proměnnou nebo objekt použitý pro počítání. V příkladu `+=` operátor přidá 1 na hodnotu proměnné pokaždé, když běží smyčky. (Chcete-li snížení proměnné ve smyčce počty dolů, použijte operátor snížení `-=`.)

## <a name="objects-and-collections"></a>Objekty a kolekce

Téměř všechno ve webové stránky ASP.NET je objekt, včetně webové stránky. Tato část popisuje některé důležité objekty, se kterými budete pracovat často v kódu.

### <a name="page-objects"></a>Objekty stránky

Objekt nejzákladnější technologie ASP.NET je stránka. Můžete přistoupit k vlastnostem objektu page přímo bez žádné opravňující objektu. Následující kód získá cesta k souboru stránky, pomocí `Request` objekt stránky:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

Můžete použít vlastnosti `Page` objekt, který chcete získat velké množství informací, jako například:

- `Request`. Jak už jsme viděli, to je kolekce informací o aktuálním požadavku, včetně, jaký typ prohlížeče požadavek, adresu URL stránky, identita uživatele, atd.
- `Response`. Toto je kolekce informací o odpovědi (stránky), která se odešle do prohlížeče při kódu serveru bylo dokončeno. Například můžete tuto vlastnost při zápisu informací do odpovědi.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a>Kolekce objektů (pole a slovník)

Kolekce je pro skupinu objektů stejného typu, například v kolekci `Customer` objekty z databáze. ASP.NET obsahuje mnoho předdefinovaných kolekcí, jako je třeba `Request.Files` kolekce.

Často budete pracovat dat v kolekcích. Jsou dvě běžné typy kolekcí *pole* a *slovník*. Pole je užitečné, když chcete ukládat kolekce podobné položky, ale nechcete vytvořit samostatnou proměnnou pro každou položku:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

S poli, je třeba deklarovat určité datového typu, `String`, `Integer`, nebo `DateTime`. K označení, proměnná může obsahovat pole, je závorkách přidat k názvu proměnné v deklaraci (například `Dim myVar() As String`). Dostanete položky v poli pomocí jejich polohu (index) nebo pomocí `For Each` příkaz. Pole indexy jsou od nuly &#8212; To znamená první položka je v umístění 0, druhý položka je na pozici 1 a tak dále.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

Můžete určit počet položek v matici získáním jeho `Length` vlastnost. Získat pozice konkrétní položku v poli (tedy prohledávání pole), použijte `Array.IndexOf` metoda. Můžete také provést třeba zpětného obsah pole ( `Array.Reverse` metoda) nebo řazení obsahu ( `Array.Sort` metoda).

Výstupní kód pole řetězec, který je zobrazený v prohlížeči:

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

Slovník je kolekce dvojic klíč/hodnota, kde zadáte klíč (nebo název) nastavit nebo načíst s odpovídající hodnotou:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

Chcete-li vytvořit slovník, je použít `New` – klíčové slovo indikující, že vytváříte novou `Dictionary` objektu. Slovník můžete přiřadit k proměnné pomocí `Dim` – klíčové slovo. Určují typy položek ve slovníku pomocí závorek ( `( )` ). Na konci tohoto prohlášení je nutné přidat jinou dvojici závorek, protože to je ve skutečnosti metoda, která vytvoří do nového slovníku.

Chcete-li přidat položky do slovníku, můžete zavolat `Add` metoda slovník proměnné (`myScores` v tomto případě) a pak zadejte klíč a hodnotu. Alternativně můžete závorkách označuje klíč a provádět jednoduché přiřazení, jako v následujícím příkladu:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

Získá hodnotu ze slovníku, zadejte klíč v závorkách:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a>Volání metody s parametry

Jak už jste viděli dříve v tomto článku, objekty, které můžete naprogramovat s mají metody. Například `Database` objekt může mít `Database.Connect` metoda. Mnoho způsobů také mít jeden nebo více parametrů. A *parametr* je hodnota, kterou předáte metodě povolení metody a dokončení úkolu. Například, podívejte se na deklaraci pro `Request.MapPath` metodu, která přijímá tři parametry:

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

Tato metoda vrátí fyzickou cestu na serveru, který odpovídá do zadané virtuální cesty. Jsou tři parametry pro metodu `virtualPath`, `baseVirtualDir`, a `allowCrossAppMapping`. (Všimněte si, že v deklaraci, jsou parametry uvedené s datovými typy dat, která budete přijmou.) Když zavoláte tuto metodu, je nutné zadat hodnoty pro všechny tři parametry.

Pokud používáte Visual Basic se syntaxí Razor, máte dvě možnosti pro předávání parametrů pro metodu: *poziční parametry* nebo *pojmenované parametry*. Pro volání metody pomocí poziční parametry, předáte parametry striktní pořadí, které je zadaný v deklaraci metoda. (By obvykle víte, toto pořadí načtením dokumentace pro metodu.) Je třeba postupovat podle pořadí, a přeskočíte nelze některý z parametrů &#8212; Pokud potřeby předáte prázdný řetězec (`""`) nebo pro poziční parametr, který nemáte hodnotu null.

Následující příklad předpokládá, že máte složku s názvem *skripty* na vašem webu. Volání kódu `Request.MapPath` metoda a předává hodnoty pro tři parametry ve správném pořadí. Pak zobrazí výsledná namapované cesta.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

Pokud je mnoho parametrů pro metodu, můžete ponechat kódu čisticí a srozumitelnější pomocí pojmenované parametry. Chcete-li zavolat metodu pomocí pojmenované parametry, zadejte název parametru, za nímž následuje `:=` a pak zadejte hodnotu. Výhodou pojmenované parametry je, že budete moct přidat v libovolném pořadí, které chcete. (Nevýhodou je, že volání metody není jako compact.)

V následujícím příkladu volání stejnou metodu, jak je uvedeno výše, ale používá pojmenované parametry k zadání hodnoty:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

Jak je vidět parametry se jí předávají v jiném pořadí. Pokud spustíte předchozí příklad a v tomto příkladu, budete však vrátit stejnou hodnotu.

## <a name="handling-errors"></a>Zpracování chyb

### <a name="try-catch-statements"></a>Try-Catch – příkazy

Příkazy často budete mít ve svém kódu, který může selhat z důvodů mimo vlastního ovládacího prvku. Příklad:

- Pokud váš kód se pokusí otevřít, vytváření, čtení a zápis do souboru, může dojít k nejrůznějším chyb. Požadovaný soubor pravděpodobně neexistuje, může být zablokován, kód nemusí mít oprávnění a tak dále.
- Podobně pokud kód pokusí k aktualizaci záznamů v databázi, může být problémy s oprávněními, může dojít ke ztrátě připojení k databázi, data uložit může být neplatný a tak dále.

Programovací podmínky, se nazývají těchto situacích *výjimky*. Pokud váš kód zjistí výjimku, generuje (vyvolává) chybové zprávy na nejvyšší, který je obtěžování uživatelům.

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

V situacích, kde kódu setkat výjimky a aby se zabránilo chybové zprávy tohoto typu, můžete použít `Try/Catch` příkazy. V `Try` prohlášení, můžete spustit kód, který při kontrole. V jedné nebo více `Catch` příkazy, můžete vyhledat konkrétní chyby (konkrétní typy výjimek), jež mohly nastat. Můžete zahrnout tolik `Catch` příkazy, jako je třeba vyhledejte chyby, které se očekává.

> [!NOTE]
> Doporučujeme, abyste neměli používat `Response.Redirect` metoda v `Try/Catch` příkazy, protože ho může způsobit výjimku v stránku.


Následující příklad ukazuje stránky, který vytvoří textový soubor na první požadavek a potom zobrazí tlačítko, které umožňuje uživateli otevřít soubor. V příkladu úmyslně použije chybný název souboru, takže způsobí výjimku. Tento kód obsahuje `Catch` příkazy pro dvě možné výjimky: `FileNotFoundException`, která nastane, pokud je špatná, název souboru a `DirectoryNotFoundException`, který v případě ASP.NET i nelze najít složku. (Chcete-li zobrazit, jak se spustí při všechno funguje správně můžete Odkomentujte příkaz v příkladu.)

Pokud váš kód nebyl zpracovat výjimku, zobrazí se chybová stránka jako předchozí snímek obrazovky. Ale `Try/Catch` části pomáhá zabránit uživateli v zobrazení tyto typy chyb.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a>Další prostředky

### <a name="reference-documentation"></a>Referenční dokumentace

- [ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)
- [Jazyk Visual Basic](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
