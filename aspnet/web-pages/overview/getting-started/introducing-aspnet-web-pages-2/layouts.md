---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: Představení technologie ASP.NET Web Pages – vytváření konzistentního rozložení | Microsoft Docs
author: tfitzmac
description: V tomto kurzu se dozvíte, jak používat rozložení k vytvoření konzistentního vzhledu stránky na webu, který používá rozhraní ASP.NET Web Pages. Předpokládá, že jste dokončili...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: c2d5c4d8ed8a71979c16d484ab90d283a45de537
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a>Představení technologie ASP.NET Web Pages – vytváření konzistentního rozložení
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> V tomto kurzu se dozvíte, jak používat *rozložení* k vytvoření konzistentního vzhledu stránky na webu, který používá rozhraní ASP.NET Web Pages. Předpokládá, že jste dokončili řady prostřednictvím [odstranění dat z databáze na webových stránkách ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251584).
> 
> Získáte informace:
> 
> - Rozložení stránky je.
> - Jak kombinovat rozložení stránek s dynamickým obsahem.
> - Jak předat hodnoty ke stránce rozložení.


## <a name="about-layouts"></a>O rozložení

Stránky, které jste vytvořili, pokud mají byla dokončena, samostatné stránky. Všechny patří do stejné lokality, ale nemají žádné společné prvky nebo standardní vzhled.

Většina serverů mají konzistentní vzhled a rozložení. Například můžete přejít [http://www.microsoft.com/web/gallery/install.aspx?appid=IISExpress](https://www.microsoft.com/web/) lokality a vyhledejte, uvidíte, že všechny stránky splňovat celkové rozložení a vizuální motiv:

![Http://www.microsoft.com/web/gallery/install.aspx?appid=IISExpress stránka zobrazující rozložení záhlaví, navigační oblast, oblast obsahu a zápatí](layouts/_static/image1.png)

*Neefektivní* způsob, jak vytvořit toto rozložení by k definování záhlaví, navigačním panelu a zápatí samostatně na jednotlivých stránkách. Můžete by být duplikování stejný kód pokaždé, když. Pokud chcete něco změnit (například aktualizace zápatí), budete muset změnit každou stránku samostatně.

Kde je *rozložení stránky* mají. Na webových stránkách ASP.NET, můžete definovat rozložení stránky, který poskytuje celkový kontejner pro stránky na váš web. Ke stránce rozložení může například obsahovat záhlaví, navigační oblasti a zápatí stránky. Ke stránce rozložení obsahuje zástupný symbol kde hlavní obsah stránky.

Potom můžete definovat jednotlivé stránky obsahu, které obsahují kód a kód pouze pro danou stránku. Stránky obsahu nemusí být kompletní stránky HTML; Nemáte i tak, aby měl `<body>` elementu. Mají také řádek kódu, který určuje ASP.NET, jaké rozložení stránky, které chcete zobrazit obsah. Tady je obrázek, který zhruba ukazuje, jak funguje tuto relaci:

![Koncepční diagram, který popisuje dvě stránky obsahu a ke stránce rozložení, do kterého se vešly](layouts/_static/image2.png)

Tato interakce je snadno zjistit, když se zobrazí v akci. V tomto kurzu změníte svoje filmy stránky použít rozložení.

## <a name="adding-a-layout-page"></a>Přidání stránky rozložení

Budete začněte vytvořením ke stránce rozložení, který definuje rozložení typické stránky v záhlaví, zápatí a oblast pro hlavní obsah. V lokalitě WebPagesMovies přidat CSHTML stránku s názvem  *\_Layout.cshtml*.

Vedoucí znak podtržítko ( `_` ) znak je důležité. Pokud na stránce název začíná podtržítkem, nebudou ASP.NET přímo odesílat této stránce v prohlížeči. Touto konvencí umožňuje definovat stránky, které jsou požadovány pro svůj web, ale, že uživatelé neměli mít možnost požádat přímo.

Nahraďte obsah na stránce s následujícími službami:

[!code-html[Main](layouts/samples/sample1.html)]

Jak je vidět, tento kód je právě HTML, který používá `<div>` elementy k definování třech částech v stránce plus jedna více `<div>` element pro uložení tři oddíly. Zápatí obsahuje bit kódu Razor: `@DateTime.Now.Year`, který vykreslí do aktuálního roku v tomto umístění na stránce.

Všimněte si, že je odkaz na šablonu stylů s názvem *Movies.css*. Šablony stylů je, kde budou určené podrobnosti o fyzické rozložení elementů. Vytvoříte, za chvíli.

Pouze neobvyklou funkce v tomto  *\_Layout.cshtml* stránka je `@Render.Body()` řádku. To je zástupný symbol, kde bude přejděte obsah, když toto rozložení je sloučen s jinou stránku.

## <a name="adding-a-css-file"></a>Přidání souboru .css

Upřednostňovaný způsob, jak definovat skutečné uspořádání (vzhled) elementy na stránce je použití pravidla kaskádových stylů (CSS). Takže vytvoříte *.css* soubor, který má pravidla pro nové rozložení.

Ve službě WebMatrix vyberte kořenový vašeho webu. Potom v **soubory** karty na pásu karet klikněte na šipku v části **nový** tlačítko a pak klikněte na **novou složku**.

![Možnost "novou složku, ve sloupci Nová na pásu karet.](layouts/_static/image3.png)

Název nové složky *styly*.

![Pojmenování nové složky, styly.](layouts/_static/image4.png)

Uvnitř nové *styly* složky, vytvořte soubor s názvem *Movies.css*.

![Vytvoření nového souboru Movies.css](layouts/_static/image5.png)

Nahraďte obsah nové *.css* soubor s následující:

[!code-css[Main](layouts/samples/sample2.css)]

Říkáme nebude mnohem o tato pravidla šablon stylů CSS, s výjimkou a Všimněte si dvě věci. Jeden je, že kromě nastavení písma a velikosti, pravidla používat absolutní umístění pro vytvoření umístění záhlaví, zápatí a oblast hlavního obsahu. Pokud jste nové umístění v jazyce CSS, si můžete přečíst [umístění šablon stylů CSS](http://www.w3schools.com/css/css_positioning.asp) kurz na webu W3Schools.

Dalším krokem, si je, že v dolní části, jsme jste zkopírovali pravidel stylu, které byly původně jednotlivě v definované *Movies.cshtml* souboru. Tato pravidla, které byly používány v [Úvod do zobrazení dat ve pomocí rozhraní ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) kurzu, aby `WebGrid` pomocné rutiny vykreslení kódu, který rozděluje do tabulky přidat. (Pokud se chystáte použít *.css* soubor definice styl, můžete také ukládat pravidla stylu pro celý web v ní.)

## <a name="updating-the-movies-file-to-use-the-layout"></a>Aktualizuje se tento soubor filmy rozložení

Nyní můžete aktualizovat existující soubory ve vaší lokalitě použít nové rozložení. Otevřete *Movies.cshtml* souboru. V horní části přidejte jako první řádek kódu, následující:

[!code-csharp[Main](layouts/samples/sample3.cs)]

Stránky teď začíná takto:

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

Tento jeden řádek kódu instruuje ASP.NET který po *filmy* spuštění stránky, by měly být sloučeny s  *\_Layout.cshtml* souboru.

Vzhledem k tomu, *Movies.cshtml* souboru teď používá ke stránce rozložení, můžete odebrat kód z *Movies.cshtml* stránky, která má postaráno podle  *\_Layout.cshtml*souboru. Vyjměte `<!DOCTYPE>`, `<html>`, a `<body>` otvírání a zavírání značky. Vyjměte celý `<head>` elementu a jeho obsah, který obsahuje pravidla stylu mřížky, protože nyní vám tato pravidla *.css* souboru. Když budete na to, změňte existující `<h1>` elementu, který chcete `<h2>` element; budete mít `<h1>` element na stránce rozložení již. Změna `<h2>` text na "Seznamu filmy".

Za normálních okolností by je nutné provést těchto nastavení neovlivní obsahu stránce. Při spuštění webu s ke stránce rozložení, vytváření obsahu stránky bez těchto prvků na začátku. V takovém případě ale převádíte na samostatné stránce na takový, který používá rozložení, takže je bit čištění.

Jakmile budete hotovi, *Movies.cshtml* stránka bude vypadat takto:

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a>Testování rozložení

Nyní uvidíte, jak vypadá rozložení. Ve službě WebMatrix, klikněte pravým tlačítkem myši *Movies.cshtml* a vyberte **spustit v prohlížeči**. Pokud prohlížeč zobrazí stránku, vypadá na této stránce:

![Stránka filmy vykreslen pomocí rozložení](layouts/_static/image6.png)

ASP.NET se sloučil obsahu stránce Movies.cshtml do  *\_Layout.cshtml* stránky správné where `RenderBody` je metoda. A samozřejmě  *\_Layout.cshtml* stránka odkazy *.css* souboru, která definuje vzhled stránky.

## <a name="updating-the-addmovie-page-to-use-the-layout"></a>Aktualizace stránky AddMovie používat rozložení

Skutečné výhodou rozložení je, že je můžete použít pro všechny stránky ve vaší lokalitě. Otevřete *AddMovie.cshtml* stránky.

Může nezapomeňte, že *AddMovie.cshtml* stránky původně měli některá pravidla šablon stylů CSS v něm k definování vzhledu chybových zpráv ověření. Vzhledem k tomu, že máte *.css* souboru pro váš webový server, můžete přesunout tyto pravidla, která *.css* souboru. Odeberte je ze *AddMovie.cshtml* souboru a přidat je do dolní části *Movies.css* souboru. Přesouváte následující pravidla:

[!code-css[Main](layouts/samples/sample6.css)]

Nyní provést stejné druhům změny v *AddMovie.cshtml* nástroji *Movies.cshtml* – přidat `Layout="~/_Layout.cshtml;` a odebrání jazyka HTML, který je nyní nadbytečné. Změna `<h1>` element `<h2>`. Když jste hotovi, stránka bude vypadat v tomto příkladu:

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

Spuštění stránky. Teď to vypadá tomto obrázku:

!["Přidat filmy, stránku vykreslen pomocí rozložení](layouts/_static/image7.png)

Chcete provést podobné změny stránek v lokalitě – *EditMovie.cshtml* a *DeleteMovie.cshtml*. Ale předtím, než to uděláte, můžete nastavit další změnou rozložení, která usnadňuje trochu flexibilnější.

## <a name="passing-title-information-to-the-layout-page"></a>Informace o předávání titulu na stránku rozložení

 *\_Layout.cshtml* stránce, kterou jste vytvořili `<title>` element, který je nastaven na "Tento server film". Většina prohlížečů zobrazí jako text na kartě obsah tento element:

![Na stránce &lt;název&gt; element zobrazí na záložce prohlížeče](layouts/_static/image8.png)

Tyto informace název je obecná. Předpokládejme, že chcete názvu text, který má být konkrétnější na aktuální stránce. (Text názvu se používá také vyhledávači určit, co je stránka o.) Můžete předat informace ze stránky obsahu jako *Movies.cshtml* nebo *AddMovie.cshtml* na stránku rozložení a pak použijte tyto informace k přizpůsobení stránky rozložení vykreslí.

Otevřete *Movies.cshtml* stránku znovu. V kódu v horní části přidejte následující řádek:

[!code-csharp[Main](layouts/samples/sample8.cs)]

`Page` Objekt je k dispozici na všech *.cshtml* stránky a je pro tento účel, a to sdílení informací mezi stránky a jeho rozložení.

Otevřete<em>\_Layout.cshtml</em> stránky. Změna `<title>` element tak, aby vypadá jako tento kód:

[!code-html[Main](layouts/samples/sample9.html)]

Tento kód vykreslí, bez ohledu `Page.Title` vlastnost přímo v tomto umístění na stránce.

Spustit *Movies.cshtml* stránky. Tentokrát záložce prohlížeče ukazuje předán jako hodnotu `Page.Title`:

![Na záložce prohlížeče zobrazující název vytváří dynamicky](layouts/_static/image9.png)

Pokud chcete, zobrazte zdroj stránku v prohlížeči. Uvidíte, že `<title>` prvek je vykreslen jako `<title>List Movies</title>`.

> [!TIP] 
> 
> **Objekt stránky**
> 
> Užitečné funkce `Page` je, že se jedná o dynamický objekt – `Title` vlastnost není pevnou velikostí, nebo vyhrazený název. Můžete použít *žádné* název hodnotu `Page` objektu. Například může snadno předané nadpis pomocí vlastnost s názvem `Page.CurrentName` nebo `Page.MyPage`. Pouze omezení je, že název má podle běžných pravidel pro jaké vlastnosti může mít název. (Například název nesmí obsahovat mezery.)
> 
> Můžete předat libovolný počet hodnot pomocí `Page` objektu. Pokud jste chtěli předat film informace ke stránce rozložení, můžete předat hodnoty pomocí něčeho jako jazyk `Page.MovieTitle` a `Page.Genre` a `Page.MovieYear`. (Nebo jiné názvy, které jste již dříve vynalezli uložit informace o.) Jediným požadavkem, což je pravděpodobně zřejmé – je, že budete muset použít stejné názvy v stránky obsahu a ke stránce rozložení.
> 
> Informace o předávání pomocí `Page` objekt není omezen na právě text, který se zobrazí na stránce rozložení. Můžete předat hodnotu na stránku rozložení a poté kódu na stránce rozložení použít hodnotu rozhodnout, jestli se má zobrazit oddíl stránky, co *.css* souboru použít a tak dále. Hodnoty, kterou předáte `Page` objekt jsou jako žádné jiné hodnoty, které použijete v kódu. Je právě zda hodnoty pocházejí ze stránky obsahu a jsou předávány ke stránce rozložení.


Otevřete *AddMovie.cshtml* stránky a přidá řádek do horní části kódu, který poskytuje název *AddMovie.cshtml* stránky:

[!code-csharp[Main](layouts/samples/sample10.cs)]

Spustit *AddMovie.cshtml* stránky. Zobrazí nový název:

![Na záložce prohlížeče zobrazující název "přidat filmy vytváří dynamicky](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a>Aktualizace zbývající stránky, které chcete použít pro rozložení

Nyní můžete dokončit zbývající stránky ve vaší lokalitě, aby používají nové rozložení. Otevřete *EditMovie.cshtml* a *DeleteMovie.cshtml* v zapnout a provést stejné změny v každé.

Přidejte řádek kódu, který odkazuje na stránku rozložení:

[!code-csharp[Main](layouts/samples/sample11.cs)]

Přidejte řádek nastavit název stránky:

[!code-csharp[Main](layouts/samples/sample12.cs)]

nebo:

[!code-csharp[Main](layouts/samples/sample13.cs)]

Odeberte všechny nadbytečné značka jazyka HTML – v zásadě platí, nechte jenom službu bits, které jsou uvnitř `<body>` – element (plus blok kódu v horní části).

Změna `<h1>` element `<h2>` elementu.

Po provedení těchto změn, otestujte a ujistěte se, že je zobrazení správně a zda je správný název.

## <a name="parting-thoughts-about-layout-pages"></a>Závěrečný myšlenky o rozložení stránky

V tomto kurzu jste vytvořili  *\_Layout.cshtml* stránky a použít `RenderBody` metoda sloučit obsahu z jiné stránky. To je základní vzor pro používání rozložení ve webových stránkách.

Rozložení stránky mají další funkce, které nebylo nabídneme sem. Můžete například vnořit rozložení stránky – jednu stránku rozložení můžete zase odkazují jiné. Vnořené rozložení může být užitečné, pokud pracujete s témata lokality, které vyžadují různé rozložení. Můžete také použít další metody (například `RenderSection`) k nastavení s názvem části na stránce rozložení.

Kombinace stránkami rozložení a *.css* je výkonný soubory. Jak uvidíte v dalším kurzu řad, ve službě WebMatrix můžete vytvořit na základě lokality *šablony*, což dává web, který obsahuje předkompilované funkce v ní. Šablony využít rozložení stránky a šablon stylů CSS chcete vytvářet weby, které vypadají skvěle a které mají funkce, například nabídky. Zde je snímek domovské stránky z lokality na základě šablony, zobrazuje funkce, které používají rozložení stránky a šablon stylů CSS:

![Rozložení vytvořené šablony webu služby WebMatrix zobrazující záhlaví, navigační oblast, oblasti obsahu, nepovinný oddíl a přihlášení odkazy](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a>Úplný seznam pro stránku Movie (aktualizovat, a použít ke stránce rozložení)

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a>Stránka dokončení výpis pro přidání stránky Movie (aktualizovat rozložení)

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a>Stránka dokončení seznamu pro odstranění film stránku (aktualizovat rozložení)

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a>Stránka dokončení výpis pro stránku film upravit (aktualizovat rozložení)

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a>Objevuje další

V dalším kurzu dozvíte, jak publikovat vaší lokality k Internetu, takže ji vidí všichni.

## <a name="additional-resources"></a>Další prostředky

- [Vytváření konzistentní vypadat](https://go.microsoft.com/fwlink/?LinkID=202891) – článek, který obsahuje některé další podrobnosti o práci s rozložení. Také popisuje, jak předat hodnotu ke stránce rozložení, který zobrazí nebo skryje část obsahu.
- [Vnořené rozložení stránky s jádrem Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) – Karel Brind blogy příklad toho, jak lze vnořit rozložení stránky. (Včetně stažení stránek.)

> [!div class="step-by-step"]
> [Předchozí](deleting-data.md)
> [další](publishing.md)
