---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: '5. část: Obchodní logiku | Dokumentace Microsoftu'
author: JoeStagner
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. 5. dílu přidá spustí nějakou obchodní logiku.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: e18acb66dbdb3bd3e0dfa21193f617dad82afc74
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754894"
---
<a name="part-5-business-logic"></a>5. část: Obchodní logiky
====================
podle [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks ukazuje, jak mimořádně jednoduché je vytvářet výkonné a škálovatelné aplikace pro platformu .NET. Zobrazuje vypnout použití skvělých nových funkcí v technologii ASP.NET 4 k sestavení nebo online úložiště, včetně nákupu, Pokladna a správu.
> 
> V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. 5. dílu přidá spustí nějakou obchodní logiku.


## <a id="_Toc260221671"></a>  Přidání spustí nějakou obchodní logiku

Chceme, aby naše prostředí pro nákup bude k dispozici pokaždé, když uživatel navštíví naše webové stránky. Uživatelé budou moci procházet a přidávat položky do nákupního košíku, i v případě, že nejsou zaregistrované nebo přihlášení. Jakmile jsou připravené k rezervaci budou mít možnost ověřovat a pokud nejsou ještě členové budou moct vytvořit účet.

To znamená, že budeme muset implementovat logiku do nákupního košíku převést anonymní stavu do stavu "Registrovaný uživatel".

Pojďme vytvořit adresář s názvem "Třídy" a klikněte pravým tlačítkem na složku a vytvořte nový soubor "Třída" MyShoppingCart.cs

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

Jak už jsme zmínili jsme rozšíření třídy, která implementuje stránce MyShoppingCart.aspx a Uděláme to pomocí. Konstrukce výkonné "částečné třídy" vaší sítě.

Vygenerovaný volání pro naše soubor MyShoppingCart.aspx.cf vypadá takto.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

Všimněte si použití klíčového slova "částečné".

Soubor třídy, který jsme právě vygenerovali vypadá takto.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

Můžeme tak, že přidáte partial – klíčové slovo k tomuto souboru a sloučí naše implementace.

Náš nový soubor třídy nyní vypadá takto.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

První metoda, která přidáme k naší třídy je metoda "AddItem". Toto je metoda, která bude volána nakonec po kliknutí na "Přidat do obrázky" odkazy na stránkách seznam produktů a podrobnosti o produktu.

Připojte pomocí příkazů v horní části stránky.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

A přidejte tuto metodu do třídy MyShoppingCart.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

Chcete-li zobrazit, pokud položka se již v košíku se používá technologii LINQ to Entities. Pokud ano, aktualizujeme množství objednávky položku, jinak se nám vytvořit nový záznam pro vybranou položku

Aby bylo možné tuto metodu volat Implementujeme AddToCart.aspx stránky, který nejen třídy tuto metodu, ale zobrazí aktuální nákupního košíku = po přidání položky.

Klikněte pravým tlačítkem na název řešení v Průzkumníku řešení a přidejte a novou stránku s názvem AddToCart.aspx, protože jsme udělali dříve.

Když jsme může pomocí této stránky lze zobrazit dočasné výsledky, jako jsou problémy s nízkou uložených atd., v naší implementaci, na stránce nejsou ve skutečnosti vykreslit, ale spíše zavolá logiky "Add" a přesměrování.

K tomu přidáme následující kód na stránku\_událost Load.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

Mějte na paměti, že jsme načítají produktu pro přidání do nákupního košíku z parametru řetězce dotazu a volání metody AddItem naší třídy.

Pokud nedojde k chybě nedojde k řízení je předáno SHoppingCart.aspx stránky, které jsme plně implementuje další. Pokud má být chybu jsme vyvolat výjimku.

Právě jsme dosud nebyla implementována globální obslužná rutina chyb, tato výjimka přejde neošetřené v naší aplikaci, ale jsme se to za chvíli napravit.

Všimněte si také použití příkazu Debug.Fail() (k dispozici prostřednictvím `using System.Diagnostics;)`

Je aplikace běží v ladicím programu, tato metoda zobrazí podrobné dialogové okno s informacemi o stavu aplikace spolu s chybovou zprávu, které určíme.

Při spouštění v produkčním prostředí Debug.Fail() příkaz je ignorován.

Všimněte si v kódu nad volání metody v našich nákupního košíku – názvy tříd "GetShoppingCartId".

Přidejte kód pro implementaci metody následujícím způsobem.

Všimněte si, že jsme taky přidali aktualizace a ověření tlačítka a popisek, kde můžeme Zobrazit košík "celkem".

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

Jsme teď můžete přidat položky na naše řešení nákupního košíku, ale jsme neimplementovali logiku pro zobrazení košíku po produktu se přidala.

Tak na stránce MyShoppingCart.aspx přidáme ovládací prvek EntityDataSource a ovládací prvek GridVire následujícím způsobem.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

Volání nahoru na formulář v návrháři, mohli dvakrát klikněte na tlačítko Aktualizovat košík a generování obslužné rutiny události kliknutí, který je zadaný v deklaraci v kódu.

Podrobnosti nám budete implementovat později, ale to se nám sestavte a spusťte naši aplikaci bez chyb.

Při spuštění aplikace a přidání položky do nákupního košíku se zobrazí toto.

![](tailspin-spyworks-part-5/_static/image2.jpg)

Všimněte si, že jsme odchylují od zobrazení mřížky "Výchozí" implementací tři vlastních sloupců.

První je upravit "Vázané" pole pro množství:

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

Další je "počítané" sloupec, který zobrazuje celkový počet řádků položky (náklady položky krát množství povolujeme):

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

A konečně budeme mít vlastní sloupec, který obsahuje ovládací prvek CheckBox, který bude uživatel používat k označení, že by položka odebrána z nákupního grafu.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

Jak je vidět, pořadí celkový řádek je prázdný Pojďme přidat nějaké logiky k výpočtu celkového pořadí.

Do naší třídy MyShoppingCart jsme nejprve budete implementovat metodu "GetTotal".

V souboru MyShoppingCart.cs přidejte následující kód.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

Pak na stránce\_můžete zavoláme vám metodě GetTotal zatížení obslužné rutiny události. Ve stejnou dobu přidáme test jestli nákupní košík je prázdný a pokud je odpovídajícím způsobem upravit zobrazení.

Teď pokud nákupní košík je prázdný získáme toto:

![](tailspin-spyworks-part-5/_static/image4.jpg)

A pokud ne, uvidíme náš celkem.

![](tailspin-spyworks-part-5/_static/image5.jpg)

Nicméně tato stránka ještě nebyla dokončena.

Budeme potřebovat další logiku k přepočítání nákupního košíku, tak, že odeberete položky označené k odstranění a tak, že určíte nové hodnoty pro množství, protože některé mohou se změnily v mřížce uživatelem.

Umožňuje přidat metodu "RemoveItem" do našich nákupního košíku třídy v MyShoppingCart.cs pro zpracování případu, když uživatel označí položku pro odebrání.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

Nyní Řekněme ad metodu ke zpracování situaci, když uživatel změní jednoduše kvality povolujeme v prvku GridView.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

Pomocí základních funkcí odebrat a aktualizace na místě můžeme implementovat do logiky, která ve skutečnosti aktualizace nákupního košíku v databázi. (V MyShoppingCart.cs)

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

Všimněte si, že tato metoda očekává dva parametry. Jeden je nákupní košík Id a druhý je pole objektů typu definované uživatelem.

Aby se minimalizovalo závislost naše logiku specifika uživatelské rozhraní, jsme definovali strukturu dat, která můžeme použít k předání nákupního košíku položky do našeho kódu bez metodě vyžadující přímý přístup k ovládacím prvku GridView.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

V našem MyShoppingCart.aspx.cs souboru můžete použít tuto strukturu v našich obslužná rutina události klikněte na tlačítko aktualizace následujícím způsobem. Všimněte si, že kromě aktualizací košíku aktualizujeme celý košíku.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

Mějte na paměti se zajímají hlavně o tento řádek kódu:

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

GetValues() je speciální pomocná funkce, které jsme se implementovat v MyShoppingCart.aspx.cs následujícím způsobem.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

To poskytuje čistý způsob, jak přistupovat k hodnotám vázaných prvků v našich prvku GridView. Vzhledem k tomu, že naše ovládací prvek zaškrtávací políčko "Odebrat položku" není vázán jsme budete k němu přístup prostřednictvím metody FindControl().

V této fázi ve vývoji projektu již brzy implementovat proces platby u pokladny.

Před tím můžeme pomocí sady Visual Studio vygenerovala databáze členství a přidejte uživatele do úložiště členství.

> [!div class="step-by-step"]
> [Předchozí](tailspin-spyworks-part-4.md)
> [další](tailspin-spyworks-part-6.md)
