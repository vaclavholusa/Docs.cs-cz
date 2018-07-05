---
uid: web-forms/overview/moving-to-aspnet-20/master-pages
title: Stránky předlohy | Dokumentace Microsoftu
author: microsoft
description: Jednou z klíčových komponent, které mají úspěšné webu je konzistentní vzhled a chování. V technologii ASP.NET 1.x, vývojáři použít uživatelské ovládací prvky k replikaci běžné elem. stránky...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 9c0cce4d-efd9-4c14-b0e8-a1a140abb3f4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/master-pages
msc.type: authoredcontent
ms.openlocfilehash: b31627fec45f153f5832afa6e317f2dd2b296d02
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382618"
---
<a name="master-pages"></a>Stránky předlohy
====================
podle [Microsoft](https://github.com/microsoft)

> Jednou z klíčových komponent, které mají úspěšné webu je konzistentní vzhled a chování. V technologii ASP.NET 1.x, vývojáři použít uživatelské ovládací prvky k replikaci společné prvky stránky do webové aplikace. Jistě, který je možná řešení, pomocí uživatelské ovládací prvky mají určité nevýhody. Například změna pozice ovládacího prvku uživatel vyžaduje změnu více stránek na webu. Uživatelské ovládací prvky se nevykreslují také v návrhovém zobrazení po vloženého na stránce.


Jednou z klíčových komponent, které mají úspěšné webu je konzistentní vzhled a chování. V technologii ASP.NET 1.x, vývojáři použít uživatelské ovládací prvky k replikaci společné prvky stránky do webové aplikace. Jistě, který je možná řešení, pomocí uživatelské ovládací prvky mají určité nevýhody. Například změna pozice ovládacího prvku uživatel vyžaduje změnu více stránek na webu. Uživatelské ovládací prvky se nevykreslují také v návrhovém zobrazení po vloženého na stránce.

Technologie ASP.NET 2.0 přináší hlavní stránky, které umožňují udržování konzistentní vzhled a chování a při brzy zobrazí, hlavní stránky představují výrazným vylepšením přes metody uživatelského ovládacího prvku.

## <a name="why-master-pages"></a>Stránky předlohy proč?

Asi vás zajímá proč byly potřeba stránky předlohy v technologii ASP.NET 2.0. Po všech webu vývojáři už používají uživatelské ovládací prvky v technologii ASP.NET 1.x sdílet oblasti obsahu mezi stránkami. Existuje ve skutečnosti několik důvodů, proč jsou méně než optimální řešení pro vytvoření rozložení platného pro běžné uživatelské ovládací prvky.

Uživatelské ovládací prvky nejsou ve skutečnosti definovat rozložení stránky. Místo toho že definují rozložení a funkce pro části stránky. Rozdíl mezi těmito dvěma je důležité, protože to ztěžuje možnosti správy řešení řízení uživatele mnohem obtížnější. Například pokud chcete změnit umístění uživatelský ovládací prvek na stránce, musíte upravit Skutečná stránka, na kterém se zobrazí uživatelského ovládacího prvku. Pokud máte pouze několik stránek, ale u velkých webů, bude rychle připomínající správy lokality přesně nastavit thats!

Jiné nevýhodou uživatelské ovládací prvky pro definování obecné rozložení je integrován v architektuře ASP.NET sám. Pokud žádný veřejný člen uživatelský ovládací prvek se změní, vyžaduje, abyste znovu kompilovat všechny stránky, které používají uživatelského ovládacího prvku. Pak ASP.NET se pak přistupuje stránky při prvním opakovanou kompilaci JIT. To, znovu vytvoří – a škálovatelnou architekturu a problém Správa lokality pro větší weby.

Oba tyto problémy (a mnoho dalších) se přehledně řešit stránky předlohy v technologii ASP.NET 2.0.

## <a name="how-master-pages-work"></a>Jak fungují stránky předlohy

Je obdobou do šablony pro jiné stránky na stránku předlohy. Prvky stránky, které by se měla sdílet mezi další stránky (tj. nabídky, ohraničení atd.) jsou přidány k hlavní stránce. Při přidání nové stránky do lokality, můžete je přiřadit se stránkou předlohy. Na stránce, který je spojen se stránkou předlohy se nazývá **stránku obsahu**. Ve výchozím nastavení převezme obsahu stránky vzhledu ze stránky předlohy. Při vytváření stránky předlohy, ale můžete definovat části stránky, která stránku obsahu můžete nahradit vlastní obsah. Tyto části jsou definovány pomocí nového ovládacího prvku zavedené v ASP.NET 2.0. **ContentPlaceHolder** ovládacího prvku.

Hlavní stránka může obsahovat libovolný počet ovládacích prvků ContentPlaceHolder (nebo vůbec žádné.) Na stránce obsahu, zobrazí se obsah z ovládacích prvků ContentPlaceHolder uvnitř **obsah** ovládací prvky, jiné nový ovládací prvek v technologii ASP.NET 2.0. Ve výchozím nastavení jsou prázdné stránky obsahu, který ovládací prvky obsahu, tak, že poskytnete vlastní obsah. Pokud chcete použít obsah z hlavní stránky uvnitř ovládacích prvků obsahu, můžete udělat tak, jak uvidíte později v tomto modulu. Ovládací prvek obsahu se mapuje na ovládací prvek ContentPlaceHolder prostřednictvím atribut ContentPlaceHolderID ovládacího prvku obsahu. Ovládací prvek obsahu níže mapy kódu na ovládací prvek ContentPlaceHolder volá mainBody na hlavní stránce.

[!code-aspx[Main](master-pages/samples/sample1.aspx)]

> [!NOTE]
> Často se poslechněte si lidé popisují hlavní stránky jako základní třída pro jiné stránky. Thats ve skutečnosti není pravda. Vztah mezi hlavní stránky a stránky obsahu není jedním dědičnosti.


**Obrázek 1** ukazuje na stránku předlohy a související stránky obsahu, jak se objeví v sadě Visual Studio 2005. Zobrazí se ovládací prvek ContentPlaceHolder na stránce předlohy a odpovídající ovládací prvek na stránce obsahu obsahu. Všimněte si, že obsah stránky předlohy, která je mimo ContentPlaceHolder je zobrazena šedě na stránce obsahu, ale. Pouze obsah uvnitř ContentPlaceHolder může být nahrazen stránky obsahu. Veškerý obsah, který přichází z hlavní stránka je neměnný.


![Stránky předlohy a stránky jeho přidružené obsahu](master-pages/_static/image1.jpg)

**Obrázek 1**: hlavní stránky a jeho přidružené obsahu stránky


## <a name="creating-a-master-page"></a>Vytvoření stránky předlohy

Pokud chcete vytvořit novou stránku předlohy:

1. Otevřít Visual Studio 2005 a vytvořit nový web.
2. Klikněte na soubor, nový, souboru.
3. Zvolte soubor předlohové z tohoto dialogového okna Přidat novou položku, jak je znázorněno v **obrázek 2**.
4. Klikněte na tlačítko Přidat.


![Vytváří se nová stránka předlohy](master-pages/_static/image2.jpg)

**Obrázek 2**: vytvoření nové stránky předlohy


Všimněte si, že přípona souboru pro hlavní stránku <em>.master</em>. Toto je jeden ze způsobů, jak se liší od běžné stránky na stránku předlohy. Hlavní rozdíl je, že náhrada @Page direktiv, obsahuje stránky předlohy @Master – direktiva. Přepněte do zobrazení zdroje pro hlavní stránky, které jste právě vytvořili a projděte si kód.

Novou stránku předlohy, bude mít jeden ovládací prvek ContentPlaceHolder ve výchozím nastavení. Ve většině případů je vhodnější pro nejprve vytvořit společné prvky stránky a pak vkládání ovládacích prvků ContentPlaceHolder kde je žádoucí vlastní obsah. V takových případech budou vývojáři chtějí ovládací prvek ContentPlaceHolder výchozí odstranit a vložit nové, jako je vytvořena na stránce. Ovládací prvky ContentPlaceHolder nejsou umožňující změnu velikosti navzdory tomu, že se zobrazují úchyty pro změnu velikosti. Velikosti ovládacího prvku ContentPlaceHolder automaticky na základě obsahu, který obsahuje s jednou výjimkou; Pokud například buňky tabulky ovládací prvek ContentPlaceHolder v blokovém elementu, bude velikost podle velikosti prvku.

## <a name="lab-1-working-with-master-pages"></a>Praktické cvičení 1 práce pomocí stránek předlohy

V tomto testovacím prostředí vytvoříte novou stránku předlohy a definovat tři prvky ContentPlaceHolder. Pak vytvoříte novou stránku obsahu a nahraďte obsah z nejméně jeden z ovládacích prvků ContentPlaceHolder.

1. Vytvoření hlavní stránky a vložit ovládací prvky ContentPlaceHolder. 

    1. Vytvořte novou stránku předlohy, jak je popsáno výše.
    2. Odstraňte ovládací prvek ContentPlaceHolder výchozí.
    3. Vyberte ovládací prvek ContentPlaceHolder kliknutím na vystínovanou horního ohraničení ovládacího prvku a odstraňte ji stisknutím klávesy DELETE na klávesnici.
    4. Vložit novou tabulku s využitím *záhlaví a na straně* šablony, jak je znázorněno na obrázku 3. Změňte šířku a výšku na 90 % tak, aby celá tabulka je zobrazen v návrháři.


![](master-pages/_static/image3.jpg)

**Obrázek 3**


1. Umístěte kurzor do jednotlivých buněk v tabulce a nastavit *valign* vlastnost *horní*.
2. Z panelu nástrojů vložte ovládací prvek ContentPlaceHolder v horní buňka v tabulce (buňku záhlaví.)
3. Když vložíte tento ovládací prvek ContentPlaceHolder, si všimnete, výška řádku bude trvat skoro celou stránku, jak je znázorněno na obrázku 4. Není možné, které zajímá, který v tomto okamžiku.


![Prázdné místo je do jedné buňky jako ContentPlaceHolder](master-pages/_static/image1.gif)

**Obrázek 4**: je prázdné místo v buňce stejné jako ContentPlaceHolder


1. Umístěte ovládací prvek ContentPlaceHolder dvě buňky. Jakmile se další ovládací prvky ContentPlaceHolder jste vložili, velikost buněk tabulky by měla být dle očekávání. Na stránce by měla vypadat stránka zobrazená v **obrázek 5**.


![Hlavní u všech ovládacích prvků ContentPlaceHolder. Všimněte si, co by měl být je výška buňky pro buňku záhlaví nyní](master-pages/_static/image2.gif)

**Obrázek 5**: Master The u všech ovládacích prvků ContentPlaceHolder. Všimněte si, co by měl být je výška buňky pro buňku záhlaví nyní


1. Do každé tři ovládací prvky ContentPlaceHolder zadejte nějaký text podle vašeho výběru.
2. Uložte jako exercise1.master stránky předlohy.
3. Vytvořit nový webový formulář a přidružte jej k hlavní stránce exercise1.master.
4. Vyberte soubor, nový, souboru v sadě Visual Studio 2005.
5. Vyberte **webový formulář** v dialogovém okně Přidat novou položku.
6. Ujistěte se, že je zaškrtnuté políčko vyberte stránku předlohy, jak je znázorněno na obrázku 6.


![Přidání nové stránky obsahu](master-pages/_static/image3.gif)

**Obrázek 6**: Přidání nové stránky obsahu


1. Klikněte na tlačítko Přidat.
2. Vyberte exercise1.master vyberte v dialogovém okně stránky předlohy jak je znázorněno na obrázku 7.
3. Klikněte na tlačítko OK pro přidání nové stránky obsahu.

Nová stránka obsahu se zobrazí v sadě Visual Studio s jeden ovládací prvek obsahu pro každý ovládací prvek ContentPlaceHolder na stránce předlohy. Ve výchozím nastavení ovládací prvky obsahu jsou prázdné, kde můžete přidat vlastní obsah. Pokud youd, jako jsou pro ně použít obsah z ovládacího prvku ContentPlaceHolder na stránce předlohy, jednoduše klikněte na symbol inteligentní značky (malé černé šipky v pravém horním rohu ovládacího prvku) a zvolte *výchozí obsah předlohy* z inteligentních značek, jak je znázorněno v **obrázek 8**. Pokud tak učiníte, položka nabídky se změní na *vytvořit vlastní obsah*. Kliknutím na od tohoto okamžiku odebere obsah z hlavní stránky umožňují definovat vlastní obsah pro konkrétní ovládací prvek obsahu.


![Nastavení ovládacího prvku obsahu na výchozí obsah stránky předlohy](master-pages/_static/image4.gif)

**Obrázek 7**: nastavení ovládacího prvku obsahu na výchozí obsah stránky předlohy


## <a name="connecting-master-page-and-content-pages"></a>Propojení stránky předlohy a obsahu stránek

Přidružení mezi stránky předlohy a stránky obsahu lze nakonfigurovat v jedné ze čtyř různých způsobů:

- <strong>MasterPageFile</strong> atribut @Page – direktiva
- Nastavení **Page.MasterPageFile** vlastností v kódu.
- **&lt;Stránky&gt;** element v konfiguračním souboru aplikace (web.config v kořenové složce aplikace)
- **&lt;Stránky&gt;** element v podsložkách konfigurační soubor (soubor web.config v podsložce)

## <a name="masterpagefile-attribute"></a>Atribut MasterPageFile

Atribut MasterPageFile usnadňuje použití stránky předlohy na konkrétní stránce technologie ASP.NET. Je také metoda používá k aplikování stránky předlohy při vrácení se změnami **vybrat hlavní stránku** zaškrtávací políčko, jako jste to udělali v cvičení 1.

## <a name="setting-pagemasterpagefile-in-code"></a>Nastavení Page.MasterPageFile v kódu

Tím, že nastavíte vlastnost MasterPageFile v kódu, můžete použít konkrétní stránky předlohy k vašemu obsahu v době běhu. To je užitečné v případech, kdy budete muset použít konkrétní hlavní stránku na základě role uživatele nebo jiných kritérií. V metodě PreInit musí nastavit vlastnost MasterPageFile. Po provedení metody PreInit je nastavena, bude vyvolána výjimka InvalidOperationException. Na stránce, na kterém je nastavena tato vlastnost musí mít také obsahu ovládacího prvku jako ovládací prvek nejvyšší úrovně pro stránku. HttpException jinak bude vyvolána, pokud je nastavena vlastnost MasterPageFile.

## <a name="using-the-ltpagesgt-element"></a>Použití &lt;stránky&gt; – Element

Nastavením atributu masterPageFile v můžete nakonfigurovat na stránku předlohy pro stránky &lt;stránky&gt; element v souboru web.config. Při použití této metody, mějte na paměti, že toto nastavení lze přepsat soubory web.config nižší ve struktuře aplikace. Všechny atributy MasterPageFile nastavit @Page – direktiva se toto nastavení také přepsat. Použití &lt;stránky&gt; element umožňuje snadno vytvořit <em>hlavní</em> stránku předlohy, která se dá přepsat v případě potřeby v konkrétní složky nebo soubory.

## <a name="properties-in-master-pages"></a>Vlastnosti stránek předloh

Hlavní stránce můžete zveřejnit vlastnosti pouhou tyto vlastnosti veřejné v rámci stránky předlohy. Například následující kód definuje vlastnost s názvem SomeProperty:

[!code-csharp[Main](master-pages/samples/sample2.cs)]

Pro přístup k vlastnosti SomeProperty ze stránky obsahu, budete muset použít hlavní vlastnosti takto:

[!code-csharp[Main](master-pages/samples/sample3.cs)]

## <a name="nesting-master-pages"></a>Stránky předlohy vnoření

Hlavní stránky jsou ideální řešení pro zajištění běžné vzhled a chování napříč velké webové aplikace. To však není nic neobvyklého, když mají některé části sdílenou složku velké lokality společné rozhraní, zatímco jiné části sdílet jiné rozhraní. Chcete-li vyřešit tuto potřebu, jsou více stránek předloh ideální řešení. Ale že stále nezabývá skutečnost, že velké aplikace může mít některé součásti (jako je například nabídka, například), které jsou sdíleny mezi všechny stránky a další součásti, které jsou sdíleny pouze některé části webu. Pro daný typ situace vyplnit vložené hlavní stránky krásně potřeba. Jak už víte, normální stránky předlohy se skládá z stránky předlohy a stránky obsahu. V situaci, vnořené stránce předlohy existují dvě hlavní stránky; hlavní nadřazené a podřízené master. Podřízená stránka předlohy je také stránku obsahu a jeho hlavní je nadřazená stránka předlohy.

Zde je kód pro typické hlavní stránky:

[!code-aspx[Main](master-pages/samples/sample4.aspx)]

V rámci vnořené hlavní scénáře to může být hlavní nadřazené. Jiné stránky předlohy by tato stránka slouží jako jeho hlavní stránky a tento kód bude vypadat takto:

[!code-aspx[Main](master-pages/samples/sample5.aspx)]

Všimněte si, že v tomto scénáři, hlavní podřízené je také pro hlavní uzel nadřazené stránky obsahu. Všechny podřízené hlavního obsahu se zobrazí uvnitř obsahu ovládacího prvku, který získá z nadřazeného objektu prvek ContentPlaceHolder jeho obsah.

> [!NOTE]
> Podpora návrháře není k dispozici pro vložené hlavní stránky. Při vývoji pomocí vnořené hlavní servery, je potřeba použít zobrazení zdroje.


Toto video ukazuje návod k používání vložené hlavní stránky.


![](master-pages/_static/image1.png)


[Otevřít Video na celou obrazovku](master-pages/_static/nested1.wmv)


![Výběr stránky předlohy](master-pages/_static/image4.jpg)

**Obrázek 8**: Výběr stránky předlohy
