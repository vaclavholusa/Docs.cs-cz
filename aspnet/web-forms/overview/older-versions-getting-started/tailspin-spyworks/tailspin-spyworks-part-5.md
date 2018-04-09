---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'Část 5: Obchodní logiky | Microsoft Docs'
author: JoeStagner
description: Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 5 přidá některé obchodní logiku.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: e4342e634ef8c4bcf4e0085650a28f414ab23736
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="part-5-business-logic"></a>Část 5: Obchodní logiky
====================
podle [Stagner Jan](https://github.com/JoeStagner)

> Tailspin Spyworks ukazuje, jak neobvykle jednoduché je vytvoření efektivní a škálovatelné aplikace pro platformu .NET. Zobrazuje vypnout používání skvělé nové funkce v rozhraní ASP.NET 4 k sestavení online úložiště, včetně nákupy, najdete v článku věnovaném a správu.
> 
> Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 5 přidá některé obchodní logiku.


## <a id="_Toc260221671"></a>  Přidání některé obchodní logiky

Chceme našich zkušeností nákupní být k dispozici vždy, když někdo navštíví našeho webu. Návštěvníků bude možné procházet a přidání položek do nákupního košíku, i když nejsou registrované nebo přihlášení. Jakmile jsou připravené k rezervaci budou mít možnost ověřit a pokud nejsou ještě členy bude mít k vytvoření účtu.

To znamená, že budeme muset implementovat logiku převést nákupní košík ze anonymní stavu do stavu "Registrovaný uživatel".

Umožňuje vytvořit adresář s názvem "Třídy" pak klikněte pravým tlačítkem na složku a vytvořte nový soubor "Třída" s názvem MyShoppingCart.cs

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

Jak už jsme zmínili jsme rozšíření třídu, která implementuje stránce MyShoppingCart.aspx a Uděláme to pomocí. Vytvořit NET je výkonný "třídu".

Vygenerovaný volání pro naše MyShoppingCart.aspx.cf souborů vypadá takto.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

Všimněte si použití klíčového slova "částečné".

Třída soubor, který jsme právě vygeneroval vypadá takto.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

Sloučíme bude naše implementace přidáním partial – klíčové slovo také tohoto souboru.

Naše nový soubor třídy nyní vypadat třeba takto.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

První metodu, která přidáme do našich třídy je metoda "AddItem". Toto je metoda, která bude volána nakonec i když uživatel klikne na "Přidat do obrázky" odkazy na stránkách seznam produktů a podrobnosti o produktu.

Připojit pomocí následujících příkazů v horní části stránky.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

A přidejte tuto metodu do třídy MyShoppingCart.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

Pokud položka je již v košíku se používá technologie LINQ to Entities. Pokud ano, můžeme aktualizovat pořadí množství položky, jinak jsme vytvořit nový záznam pro vybranou položku.

Aby bylo možné tuto metodu volat bude implementaci stránku AddToCart.aspx, která je jenom třídy tuto metodu, ale pak se zobrazují aktuální nákupního košíku = po přidání položky.

Klikněte pravým tlačítkem na název řešení v Průzkumníku řešení a přidejte a novou stránku s názvem AddToCart.aspx, jak je uvedeno v dříve.

Při dočasné výsledky jako nízkou uložených problémy atd., se na naše implementace jsme může pomocí této stránky, stránky se ve skutečnosti není vykreslení, ale místo volání logice "Přidat" a přesměruje.

K tomu přidáme následující kód na stránku\_zatížení událostí.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

Všimněte si, načítají produktu pro přidání do nákupního košíku ze parametr řetězce dotazu a volání metody AddItem naše třídy.

Za předpokladu, že žádné chyby došlo k ovládací prvek předává na stránku SHoppingCart.aspx, který bude plně implementaci Další. Pokud má být chybu jsme vyvolat výjimku.

Aktuálně jsme nebyly dosud implementován popisovač globální chyb, by se dostala neošetřené v naší aplikaci výjimku, ale jsme se to za chvíli napravit.

Všimněte si také použití příkazu Debug.Fail() (dostupné prostřednictvím `using System.Diagnostics;)`

Je je aplikace spuštěna v rámci ladicího programu, tato metoda zobrazí podrobné dialogové okno s informacemi o stavu aplikace spolu s chybovou zprávu, která určíme.

Při spuštění v produkčním prostředí Debug.Fail() příkaz je ignorováno.

Všimněte si v kódu výše volání metody v našem nákupní košík – názvy tříd "GetShoppingCartId".

Přidáte kód pro implementaci metodu následujícím způsobem.

Všimněte si, že také přidali jsme aktualizace a najdete v článku věnovaném tlačítka a kde jsme můžete zobrazit košík "celkem" štítek.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

Jsme teď můžete přidat položky do naše řešení nákupního košíku ale jsme ještě implementována logiku zobrazíte košíku po přidání produktu.

Ano na stránce MyShoppingCart.aspx přidáme GridVire ovládací prvek EntityDataSource a následujícím způsobem.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

Volání do formuláře v návrháři, aby mohli dvakrát klikněte na tlačítko Aktualizovat košíku a generovat obslužnou rutinu události kliknutí, který je zadaný v deklaraci do kódu.

Podrobnosti budete implementaci později, ale tato funkce bude dejte nám sestavení a spuštění aplikace bez chyb.

Při spuštění aplikace a přidejte položku do nákupního košíku uvidíte to.

![](tailspin-spyworks-part-5/_static/image2.jpg)

Všimněte si, že jsme odchylují od zobrazení mřížky "Výchozí" implementací tři vlastní sloupce.

První je upravit, "Vázaná" pole pro množství:

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

Další je "počítané" sloupec, který zobrazuje celkový řádku položky (náklady položka časy množství, které má být pořadí):

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

Nakonec máme vlastní sloupec, který obsahuje ovládací prvek CheckBox, který bude uživatel používat k označení, že položka má být odebrána z nákupní grafu.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

Jak vidíte, přidejte pořadí celkový řádek je prázdný, můžeme některé logiku vypočítat celkové pořadí.

Pro naše třída MyShoppingCart jsme nejprve budete implementovat metodu "GetTotal".

V souboru MyShoppingCart.cs přidejte následující kód.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

Pak na stránce\_obslužné rutiny události zatížení může zavoláme vám naše GetTotal metoda. Ve stejnou dobu přidáme testu chcete zobrazit, pokud je prázdný nákupní košík a odpovídajícím způsobem upravit zobrazení, pokud se jedná.

Teď Pokud prázdný nákupní košík jsme se toto:

![](tailspin-spyworks-part-5/_static/image4.jpg)

A pokud ne, vidíme naše celkem.

![](tailspin-spyworks-part-5/_static/image5.jpg)

Však tato stránka se ještě nedokončilo.

Potřebujeme další logiku přepočítat nákupní košík odstraněním položky označené k odstranění a tak, že určíte nové hodnoty pro množství, protože některé může být změněna v mřížce uživatelem.

Umožňuje přidat metodu "RemoveItem –" naše nákupní košík třídy v MyShoppingCart.cs zpracování případě, pokud uživatel označí položku pro odebrání.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

Nyní Pojďme ad metodu ke zpracování za podmínek, když uživatel změní jednoduše kvality chcete objednat v GridView.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

Se základními funkcemi odebrat a aktualizace na místě můžeme implementovat logiku, která ve skutečnosti aktualizuje nákupní košík v databázi. (In MyShoppingCart.cs)

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

Budete Všimněte si, že tato metoda očekává dva parametry. Jedna je nákupní košík Id a druhá je pole objektů uživatelem definovaný typ.

Aby pro minimalizaci závislost na uživatelské rozhraní specifika logiku, jsme jste definovali datová struktura, která jsme slouží k předávání nákupní košík položky na našem kódu bez naše metoda, které vyžadují přímý přístup k ovládacím prvku GridView.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

V našem MyShoppingCart.aspx.cs souboru můžete použít tuto strukturu v našich obslužné rutiny události klikněte na tlačítko Aktualizovat následujícím způsobem. Všimněte si, že kromě aktualizace košíku budeme aktualizovat také celkové košíku.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

Mějte na paměti, zvláštní zájem tento řádek kódu:

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

GetValues() je speciální pomocné funkce, která bude v MyShoppingCart.aspx.cs implementaci následujícím způsobem.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

To umožňuje vyčištění pro přístup k hodnotám vázaných prvků v ovládacím prvku naše GridView. Vzhledem k tomu, že naše "Odebrat položka" CheckBox – ovládací prvek není vázán jsme budete k němu přístup prostřednictvím metody FindControl().

V této fázi v projektu na vývoj jsme jsou Příprava implementovat proces checkout.

Než tak umožňuje generovat databázi členství a přidejte uživatele do úložiště členství pomocí sady Visual Studio.

> [!div class="step-by-step"]
> [Předchozí](tailspin-spyworks-part-4.md)
> [další](tailspin-spyworks-part-6.md)
