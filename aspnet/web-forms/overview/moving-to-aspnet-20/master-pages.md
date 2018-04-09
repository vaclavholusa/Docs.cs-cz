---
uid: web-forms/overview/moving-to-aspnet-20/master-pages
title: Stránky předlohy | Microsoft Docs
author: microsoft
description: Mezi klíčové součásti, které úspěšná webu je konzistentní vzhled a chování. V technologii ASP.NET 1.x, vývojáři použít uživatelské ovládací prvky replikace běžné elem. stránky...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 9c0cce4d-efd9-4c14-b0e8-a1a140abb3f4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/master-pages
msc.type: authoredcontent
ms.openlocfilehash: f45dd9704f665244d2a48ec000326f6e98984e4f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="master-pages"></a>Stránky předlohy
====================
podle [Microsoft](https://github.com/microsoft)

> Mezi klíčové součásti, které úspěšná webu je konzistentní vzhled a chování. V technologii ASP.NET 1.x, vývojáři použít uživatelských ovládacích prvků k replikaci společné prvky stránky do webové aplikace. Přesto, že je určitě použitelnou řešení, pomocí uživatelské ovládací prvky mít některé nevýhody. Například ke změně pozice uživatelského ovládacího prvku vyžaduje změnu více stránek v lokalitě. Uživatelské ovládací prvky se nevykreslí také v zobrazení návrhu po vkládání na stránce.


Mezi klíčové součásti, které úspěšná webu je konzistentní vzhled a chování. V technologii ASP.NET 1.x, vývojáři použít uživatelských ovládacích prvků k replikaci společné prvky stránky do webové aplikace. Přesto, že je určitě použitelnou řešení, pomocí uživatelské ovládací prvky mít některé nevýhody. Například ke změně pozice uživatelského ovládacího prvku vyžaduje změnu více stránek v lokalitě. Uživatelské ovládací prvky se nevykreslí také v zobrazení návrhu po vkládání na stránce.

Technologie ASP.NET 2.0 přináší hlavní stránky jako způsob zachování konzistentní vzhled a chování a jako vám brzy zobrazí, hlavní stránky představují výrazným vylepšením přes metodu řízení uživatele.

## <a name="why-master-pages"></a>Proč hlavní stránky?

Asi vás zajímá, proč nebylo potřeba hlavní stránky v aplikaci ASP.NET 2.0. Po všech webu vývojáři už používáte uživatelských ovládacích prvků technologie ASP.NET 1.x pro sdílení obsahu oblasti mezi stránkami. Tady je ve skutečnosti několik důvodů, proč uživatelské ovládací prvky jsou méně než optimální řešení pro vytváření běžné rozložení.

Uživatelské ovládací prvky nejsou ve skutečnosti definovat rozložení stránky. Místo toho že definují rozložení a funkce pro část stránky. Mezi tyto dva rozdíl je důležité, protože možnosti správy řešení řízení uživatele je mnohem obtížnější. Například pokud chcete změnit umístění uživatelského ovládacího prvku na stránku, je nutné upravit Skutečná stránka, na kterém se zobrazí uživatelského ovládacího prvku. Thats přesně, pokud máte pouze několik stránek, ale u velkých webů, bude rychle při důvodů správy lokality!

Jiné nevýhodou uživatelské ovládací prvky pro definování běžné rozložení se zobrazuje v architektuře ASP.NET sám sebe. Pokud dojde ke změně žádný veřejný člen uživatelského ovládacího prvku, se vyžaduje, abyste znovu zkompiluje všechny stránky, které používají uživatelského ovládacího prvku. Pak ASP.NET se pak znovu JIT svoje stránky při prvním přístup. To, znovu vytvoří-škálovatelná architektura a problém Správa lokality pro větší lokalit.

Oba tyto problémy (a mnoho dalších) jsou vhodně řešit hlavní stránky v aplikaci ASP.NET 2.0.

## <a name="how-master-pages-work"></a>Jak fungují stránky předlohy

Stránky předlohy je obdobou šablonu pro jiné stránky. Prvky stránky, které by měl být sdílen dalších stránek (tj. nabídky, ohraničení atd.) se přidají k hlavní stránce. Po přidání nové stránky na web, můžete je přidružit se stránkou předlohy. Je volána stránku, která je přidružena k hlavní stránce **obsahu stránce**. Ve výchozím nastavení stránky obsahu bude vzhled ze stránky předlohy. Ale když vytvoříte stránky předlohy, můžete definovat části stránky, která stránky obsahu můžete nahradit vlastní obsah. Tyto části jsou definovány pomocí ovládacího prvku nové byla zavedená v technologii ASP.NET 2.0; **ContentPlaceHolder** ovládacího prvku.

Na hlavní stránce může obsahovat libovolný počet ovládacích prvků ContentPlaceHolder (nebo none na všechny.) Na stránce obsahu, zobrazí se obsah z obsahuje rozložení uvnitř **obsah** ovládací prvky, jiné nový ovládací prvek v technologii ASP.NET 2.0. Ve výchozím nastavení jsou obsahu stránky, které řídí obsah tak, aby můžete zadat vlastní obsah prázdné. Pokud chcete používat obsah ze stránky předlohy uvnitř ovládací prvky obsahu, můžete to udělat tak, jak uvidíte později v tomto modulu. Ovládací prvek obsahu je namapována na ovládací prvek ContentPlaceHolder prostřednictvím atribut ContentPlaceHolderID obsahu ovládacího prvku. Kód pod mapy obsahu řízení do ovládacího prvku ContentPlaceHolder nazývá mainBody na hlavní stránce.

[!code-aspx[Main](master-pages/samples/sample1.aspx)]

> [!NOTE]
> Budou často slyšet osoby, které popisují hlavní stránky jako základní třída pro jiné stránky. Thats ve skutečnosti není pravda. Vztah mezi hlavní stránky a stránky obsahu není jedním z dědičnosti.


**Obrázek 1** zobrazuje stránku předlohy a stránku obsahu přidružené, jak jsou v sadě Visual Studio 2005. Zobrazí ovládací prvek ContentPlaceHolder stránky předlohy a odpovídající obsah ovládacího prvku obsahu stránce. Všimněte si, že je hlavní stránky obsahu, který je mimo ContentPlaceHolder viditelné, ale šedě na stránce obsahu. Pouze obsah, uvnitř ContentPlaceHolder může být nahrazen stránky obsahu. Veškerý obsah, který přichází z stránky předlohy se nedá změnit.


![Hlavní stránky a jeho přidružené obsahu stránky](master-pages/_static/image1.jpg)

**Obrázek 1**: hlavní stránky a jeho přidružené obsahu stránky


## <a name="creating-a-master-page"></a>Vytvoření stránky předlohy

Pokud chcete vytvořit novou stránku předlohy:

1. Otevřete Visual Studio 2005 a vytvořit nový web.
2. Klikněte na soubor, nový, soubor.
3. Vyberte soubor předlohové z dialogového okna Přidat novou položku, jak je znázorněno v **obrázek 2**.
4. Klikněte na tlačítko Přidat.


![Vytvoření nové stránky předlohy](master-pages/_static/image2.jpg)

**Obrázek 2**: vytvoření nové stránky předlohy


Všimněte si, že přípona souboru pro hlavní stránky je <em>.master</em>. Toto je jedním ze způsobů, který na hlavní stránce se liší od obyčejnou stránky. Základní rozdíl je, že v lieu z @Page direktivy, obsahuje stránky předlohy @Master – direktiva. Přepněte do zobrazení zdroje pro hlavní stránky, které jste právě vytvořili a zkontrolujte kód.

Novou stránku předlohy bude mít jeden prvek ContentPlaceHolder ve výchozím nastavení. Ve většině případů je vhodnější nejprve vytvořte společné prvky stránky a pak vložení ovládacích prvků ContentPlaceHolder kde je žádoucí vlastní obsah. V takových případech bude vývojáři chcete odstranit ovládací prvek ContentPlaceHolder výchozí a vkládání nových jako vyvinutý stránky. Ovládací prvky ContentPlaceHolder nejsou s možností změny velikosti přes skutečnost, že se zobrazí úchyty. Velikosti ovládacího prvku ContentPlaceHolder automaticky podle obsahu, který obsahuje s jednou výjimkou; Pokud jste například buňky tabulky ovládacího prvku ContentPlaceHolder v blokovém elementu, bude velikost na základě velikosti elementu.

## <a name="lab-1-working-with-master-pages"></a>Laboratoř 1 práce stránky předlohy

V tomto testovacím prostředí vytvoříte novou stránku předlohy a definovat tři ContentPlaceHolder ovládací prvky. Pak vytvoříte nové stránky obsahu a nahradí obsah alespoň jednu z obsahuje rozložení.

1. Vytvoření stránky předlohy a vložení ContentPlaceHolder ovládacích prvků. 

    1. Vytvořte novou stránku předlohy, jak je popsáno výše.
    2. Odstraňte ovládací prvek ContentPlaceHolder výchozí.
    3. Vyberte ovládací prvek ContentPlaceHolder kliknutím šedou barvou horního okraje ovládacího prvku a pak ji odstraňte zasažení DEL klíč na klávesnici.
    4. Vložit novou tabulku pomocí *záhlaví a straně* šablony, jak je znázorněno na obrázku 3. Změňte šířku a výšku na 90 % a celá tabulka je viditelný v návrháři.


![](master-pages/_static/image3.jpg)

**Obrázek 3**


1. Umístěte kurzor do jednotlivých buněk tabulky a nastavte *valign* vlastnost *horní*.
2. Z panelu nástrojů vložení ovládacího prvku ContentPlaceHolder na horní buňku v tabulce (záhlaví buňky.)
3. Po vložení tohoto ovládacího prvku ContentPlaceHolder si všimnete, že výšku řádku bude trvat téměř celý stránku, jak je znázorněno na obrázku 4. Dont se starat o který v tomto okamžiku.


![Prázdné místo je do jedné buňky jako ContentPlaceHolder](master-pages/_static/image1.gif)

**Obrázek 4**: prázdné místo je do jedné buňky jako ContentPlaceHolder


1. Umístěte ovládací prvek ContentPlaceHolder dvě buňky. Po vložení jiných ContentPlaceHolder ovládacích prvků, měl by být velikost buněk tabulky byste očekávali. Stránka by měl nyní vypadat stránka zobrazená v **obrázek 5**.


![Hlavní s všechny ovládací prvky ContentPlaceHolder. Všimněte si, že je výška buňky pro datovou buňku teď by měla být](master-pages/_static/image2.gif)

**Obrázek 5**: Master The s všechny ovládací prvky ContentPlaceHolder. Všimněte si, že je výška buňky pro datovou buňku teď by měla být


1. Zadejte text zvoleného do každé tři ovládacích prvků ContentPlaceHolder.
2. Uložte jako exercise1.master stránky předlohy.
3. Vytvořit nový webový formulář a přidružte ji k hlavní stránce exercise1.master.
4. Vyberte soubor, nový, souborů v sadě Visual Studio 2005.
5. Vyberte **webového formuláře** v dialogovém okně Přidat novou položku.
6. Ujistěte se, že je zaškrtnuté políčko vyberte stránku předlohy, jak je znázorněno na obrázku 6.


![Přidání nové stránky obsahu](master-pages/_static/image3.gif)

**Obrázek 6**: Přidání nové stránky obsahu


1. Klikněte na tlačítko Přidat.
2. Vyberte exercise1.master vyberte v dialogovém okně stránky předlohy jak je znázorněno na obrázku 7.
3. Klikněte na OK přidáte nové stránky obsahu.

Nová stránka obsahu se zobrazí v sadě Visual Studio obsahuje jeden prvek obsahu pro každý ovládací prvek ContentPlaceHolder na hlavní stránce. Ve výchozím nastavení jsou ovládací prvky obsahu tak, aby můžete přidat vlastní obsah prázdné. Pokud se pro ně k používání obsahu z ovládacího prvku ContentPlaceHolder na hlavní stránce jako youd, jednoduše klikněte na symbol inteligentní značky (malé černou šipku v pravém horním rohu ovládacího prvku) a zvolte *výchozí obsah hlavních serverů* ze inteligentní značky, jak je uvedené v **obrázek 8**. Pokud tak učiníte, položky nabídky se změní na *vytvořit vlastní obsah*. Kliknutím na v tomto bodě odebere obsah z hlavní stránky umožňuje definovat vlastní obsah pro konkrétní ovládací prvek obsahu.


![Nastavení ovládacího prvku obsahu jako výchozí obsah stránky předlohy](master-pages/_static/image4.gif)

**Obrázek 7**: nastavení ovládacího prvku obsahu na výchozí obsah stránky předlohy


## <a name="connecting-master-page-and-content-pages"></a>Připojení stránky předlohy a stránky obsahu

Přidružení mezi stránky předlohy a stránky obsahu mohou být konfigurovány v jednom z znázorněné různé způsoby:

- <strong>MasterPageFile</strong> atribut @Page – direktiva
- Nastavení **Page.MasterPageFile** vlastností v kódu.
- **&lt;Stránky&gt;** element v konfiguračním souboru aplikace (web.config v kořenové složce aplikace)
- **&lt;Stránky&gt;** element v konfiguračním souboru podsložky (soubor web.config v podsložce)

## <a name="masterpagefile-attribute"></a>Atribut MasterPageFile

Atribut MasterPageFile snadno použít stránku předlohy na konkrétní stránku ASP.NET. Je také metodu použitou k použití stránky předlohy, pokud zaškrtnete **vyberte stránku předlohy** políčko jako jste postupovali při cvičení 1.

## <a name="setting-pagemasterpagefile-in-code"></a>Nastavení Page.MasterPageFile v kódu

Nastavením vlastnost MasterPageFile v kódu můžete použít konkrétní stránky předlohy k vašemu obsahu za běhu. To je užitečné v případech, kdy budete muset použít na konkrétní stránku předlohy, které jsou na základě role uživatele nebo jiných kritérií. V metodě PreInit musí nastavit vlastnost MasterPageFile. Pokud je nastavené po metodě PreInit, bude vyvolána výjimkou InvalidOperationException. Stránka, na kterém je tato vlastnost nastavena musí také mít obsah ovládacího prvku jako ovládací prvek nejvyšší úrovně pro stránku. HttpException jinak bude vyvolána, pokud je vlastnost MasterPageFile nastavena.

## <a name="using-the-ltpagesgt-element"></a>Pomocí &lt;stránky&gt; – Element

Na hlavní stránce můžete nakonfigurovat pro stránky nastavením atributu masterPageFile v &lt;stránky&gt; element v souboru web.config. Při použití této metody, mějte na paměti, soubory web.config nižší ve struktuře aplikace můžete toto nastavení přepsat. Všechny atributy MasterPageFile, nastavte v @Page – direktiva bude toto nastavení také přepsat. Pomocí &lt;stránky&gt; element můžete snadno vytvořit <em>hlavní</em> stránky předlohy, který může být přepsána nastaveními v případě potřeby v konkrétní složek nebo souborů.

## <a name="properties-in-master-pages"></a>Vlastnosti stránky předlohy

Na hlavní stránce můžete vystavení vlastností pouhou tyto vlastnosti veřejné v rámci stránky předlohy. Například následující kód definuje vlastnost s názvem SomeProperty:

[!code-csharp[Main](master-pages/samples/sample2.cs)]

Pro přístup k vlastnosti SomeProperty ze stránky obsahu, budete muset použít hlavní vlastnost takto:

[!code-csharp[Main](master-pages/samples/sample3.cs)]

## <a name="nesting-master-pages"></a>Stránky předlohy vnoření

Hlavní stránky jsou ideální řešení pro zajištění běžné vzhled a chování napříč velkou webovou aplikaci. Však není do mají některé části sdílenou složku velké lokality společné rozhraní, zatímco ostatní části sdílet jiné rozhraní. Chcete-li vyřešit tuto potřebu, více hlavní stránky jsou ideální řešení. Ale že stále není adres fakt, že velké aplikace může mít některé součásti (například nabídky, např.), které jsou sdíleny mezi všechny stránky a další součásti, které jsou sdíleny pouze určité části webu. Pro tento typ situaci vyplní vnořené hlavní stránky vhodně potřeba. Už víte, se skládá ze stránky předlohy a stránky obsahu normální stránky předlohy. V situaci, vnořené hlavní stránky existují dvě hlavní stránky; hlavní nadřazené a podřízené hlavní. Podřízená stránka předlohy je také stránky obsahu a jeho hlavní server je nadřazená stránka předlohy.

Tady je kód pro typické hlavní stránky:

[!code-aspx[Main](master-pages/samples/sample4.aspx)]

Ve scénáři vnořené hlavní bude hlavní nadřazené. Jiné stránky předlohy by tato stránka slouží jako jeho hlavní stránky a tento kód bude vypadat takto:

[!code-aspx[Main](master-pages/samples/sample5.aspx)]

Všimněte si, že se v tomto scénáři, hlavní podřízené je také obsahu stránce pro hlavní nadřazený server. Všechny podřízené hlavního obsahu se zobrazí uvnitř ovládacího prvku obsahu, který získá jeho obsah z ovládacího prvku ContentPlaceHolder nadřazeného objektu.

> [!NOTE]
> Podpora návrháře není dostupné pro vnořené hlavní stránky. Pokud vyvíjíte pomocí vnořené hlavních serverů, budou muset použít zobrazení zdroje.


Toto video ukazuje návod k používání vnořené hlavní stránky.


![](master-pages/_static/image1.png)


[Open Full-Screen Video](master-pages/_static/nested1.wmv)


![Vyberte stránku předlohy](master-pages/_static/image4.jpg)

**Obrázek 8**: Výběr stránky předlohy
