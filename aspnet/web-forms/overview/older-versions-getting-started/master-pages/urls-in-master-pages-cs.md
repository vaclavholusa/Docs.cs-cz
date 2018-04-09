---
uid: web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-cs
title: Adresy URL v hlavní stránky (C#) | Microsoft Docs
author: rick-anderson
description: Řeší, jak můžete rozdělit adresy URL na hlavní stránce kvůli soubor předlohové stránky se v jiném adresáři relativní než stránky obsahu. Zjistí rebasing...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2008
ms.topic: article
ms.assetid: 48b58a18-5ea4-468c-b326-f35331b3e1e9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 494338f1a0705c8d7e15bc693ae1ec6362b26d64
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="urls-in-master-pages-c"></a>Adresy URL v hlavní stránky (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_04_CS.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_04_CS.pdf)

> Řeší, jak můžete rozdělit adresy URL na hlavní stránce kvůli soubor předlohové stránky se v jiném adresáři relativní než stránky obsahu. Zjistí rebasing adresy URL prostřednictvím ~ v deklarativní syntaxi a použití ResolveUrl a ResolveClientUrl prostřednictvím kódu programu. (Taky podívejte se na


## <a name="introduction"></a>Úvod

Ve všech příkladech jsme viděli doposud, že se stránky předlohy a obsahu byly umístěny ve stejné složce (do kořenové složky webu). Ale neexistuje žádný důvod, proč se stránky předlohy a obsahu musí být ve stejné složce. Určitě můžete vytvářet stránky obsahu v podsložkách. Podobně můžete vytvořit `~/MasterPages/` složku, kam umístit hlavní stránky vašeho webu.

Jeden potenciální problém s umístěním stránky v různých složkách zahrnuje porušený adresy URL. Pokud stránka předlohy obsahuje relativní adresy URL v hypertextové odkazy, Image nebo jiných prvků, budou mít neplatná pro stránky obsahu, které se nacházejí v jiné složce odkaz. V tomto kurzu jsme zkontrolujte zdroji tento problém, stejně jako alternativní řešení.

## <a name="the-problem-with-relative-urls"></a>Problém s relativní adresy URL

Adresa URL webové stránky se říká, že *relativní adresu URL* Pokud je umístění prostředku odkazuje na relativní k umístění webové stránky v struktura složek tohoto webu. Libovolnou URL, která nezačíná úvodní lomítkem (`/`) nebo protokol (například `http://`) je relativní vzhledem k tomu, že se vyřeší v prohlížeči na základě umístění webové stránky, který obsahuje adresu URL.

Například náš web má `~/Images/` složky se souborem bitové kopie jedné `PoweredByASPNET.gif`. Soubor předlohové stránky `Site.master` má `<img>` element v `footerContent` oblasti s následující kód:


[!code-html[Main](urls-in-master-pages-cs/samples/sample1.html)]

`src` Hodnotu v atributu `<img>` element je relativní adresa URL, protože se nespustí, s `/` nebo `http://`. Stručně řečeno `src` hodnota atributu sděluje prohlížeči, aby oblast hledání `Images` podsložky pro soubor s názvem `PoweredByASPNET.gif`.

Při návštěvě stránky obsahu, je odeslána výše uvedený kód přímo do prohlížeče. Za chvíli navštivte `About.aspx` a zobrazit zdrojový kód HTML, který je odesláno prohlížeči. Zjistíte, že přesně stejnou značek na hlavní stránce byl odeslán do prohlížeče.


[!code-html[Main](urls-in-master-pages-cs/samples/sample2.html)]

Pokud je obsahu stránce v kořenové složce (jako je `About.aspx`) všechno funguje podle očekávání, protože je `Images` podsložka relativně ke kořenové složce. Však věcí rozdělení Pokud stránky obsahu do jiné složky než stránky předlohy. Pro znázornění je vytvořit podsložku s názvem `Admin`. Dál přidejte obsahu stránce s názvem `Default.aspx` k `Admin` složku, a zkontrolujte, zda novou stránku vázat `Site.master` stránky předlohy.

> [!NOTE]
> V [ *zadáte název, značky Meta a ostatní hlavičky HTML na hlavní stránce* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) kurzu jsme vytvořili vlastní základní stránky třídy s názvem `BasePage` , automaticky nastaví název obsahu stránce (pokud ho nebyl přiřazen explicitně). Nezapomeňte mít odvozena od třídy kódu nově vytvořený stránky `BasePage` tak, aby ji můžete využít tuto funkci.


Po vytvoření této obsahu stránce, vaše Průzkumníku řešení by měla vypadat podobně jako na obrázku 1.


![Novou složku a stránku ASP.NET byly přidány do projektu](urls-in-master-pages-cs/_static/image1.png)

**Obrázek 01**: novou složku a stránku ASP.NET byly přidány do projektu


Potom aktualizujte `Web.sitemap` souboru novou `<siteMapNode>` záznam pro tento účel. Následující kód XML zobrazuje kompletní `Web.sitemap` značek, které nyní zahrnuje přidání třetí `<siteMapNode>` elementu.


[!code-xml[Main](urls-in-master-pages-cs/samples/sample3.xml)]

Nově vytvořený `Default.aspx` stránka by měla mít čtyři ovládací prvky obsahu odpovídající čtyři ContentPlaceHolders v `Site.master`. Přidejte nějaký text k odkazování na ovládací prvek obsahu `MainContent` ContentPlaceHolder a potom navštivte stránku prostřednictvím prohlížeče. Jak je vidět na obrázku 2, nelze najít v prohlížeči `PoweredByASPNET.gif` soubor bitové kopie. Co se děje tady?

`~/Admin/Default.aspx` Stránky obsahu jsou odeslány stejné HTML `footerContent` oblasti jako byla `About.aspx` stránky:


[!code-html[Main](urls-in-master-pages-cs/samples/sample4.html)]

Protože `<img>` elementu `src` atribut je relativní adresa URL, v prohlížeči se pokusí vyhledat `Images` složky relativní k umístění složky webové stránky. Jinými slovy, v prohlížeči hledá soubor bitové kopie `Admin/Images/PoweredByASPNET.gif`.


[![Soubor bitové kopie PoweredByASPNET.gif nebyl nalezen.](urls-in-master-pages-cs/_static/image3.png)](urls-in-master-pages-cs/_static/image2.png)

**Obrázek 02**: `PoweredByASPNET.gif` Image soubor nebyl nalezen ([Kliknutím zobrazit obrázek v plné velikosti](urls-in-master-pages-cs/_static/image4.png))


### <a name="replacing-relative-urls-with-absolute-urls"></a>Nahraďte absolutní adresy URL relativní adresy URL

Je opakem relativní adresa URL *absolutní adresa URL*, což je ten, který začíná lomítkem (`/`) nebo protokolu, jako `http://`. Protože absolutní adresu URL určuje umístění prostředku ze známé pevné bodu, stejné absolutní adresa URL je platná v jakékoli webové stránky, bez ohledu na umístění webové stránky v struktura složek webu.

Chcete-li opravit poškozený obrázek znázorňuje obrázek 2, je potřeba aktualizovat `<img>` elementu `src` atributů tak, aby používala místo relativní jedna absolutní adresu URL. Pokud chcete určit správnou absolutní adresu URL, navštíví některý z webových stránek ve vašem webu a prozkoumat panelu Adresa. Jak je vidět na panelu Adresa na obrázku 2, je plně kvalifikovanou cestu k webové aplikaci `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_CS/`. Proto budeme aktualizovat `<img>` elementu `src` atribut na jednu z následujících dvou absolutní adresy URL:

- `/ASPNET_MasterPages_Tutorial_04_CS/Images/PoweredByASPNET.gif`
- `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_CS/Images/PoweredByASPNET.gif`

Za chvíli aktualizovat `<img>` elementu `src` atribut absolutní adresu URL pomocí jedné z formuláře uvedené výše a potom navštivte `~/Admin/Default.aspx` stránku prostřednictvím prohlížeče. Tentokrát prohlížeče správně vyhledat a zobrazit `PoweredByASPNET.gif` soubor bitové kopie (viz obrázek 3).


[![Bitovou kopii PoweredByASPNET.gif se nyní zobrazí](urls-in-master-pages-cs/_static/image6.png)](urls-in-master-pages-cs/_static/image5.png)

**Obrázek 03**: `PoweredByASPNET.gif` bitová kopie je nyní zobrazí ([Kliknutím zobrazit obrázek v plné velikosti](urls-in-master-pages-cs/_static/image7.png))


Při pevném kódování v absolutní adresa URL funguje, pevně se spojuje kódu HTML k webu serveru a umístění složky, které může změnit. Pomocí absolutní adresu URL ve formátu `http://localhost:3908/...` je křehká protože předchozí číslo portu `localhost` vybere se automaticky při každém spuštění sady Visual Studio integrovaného ASP.NET vývoj webového serveru. Podobně `http://localhost` část je platný pouze při testování místně. Jakmile kód je nasazena na provozním serveru, se základní adresa URL změní na něco jiného, jako je třeba `http://www.yourserver.com`. Absolutní adresa URL ve formátu `/ASPNET_MasterPages_Tutorial_04_CS/...` také vykazuje stejný brittleness, protože často tuto cestu aplikace se liší mezi vývoj a produkční servery.

Dobrá zpráva je, že technologie ASP.NET nabízí metody pro generování platnou relativní adresu URL za běhu.

## <a name="usingandresolveclienturl"></a>Pomocí`~`a`ResolveClientUrl`

Spíše než pevného code absolutní adresu URL, ASP.NET umožňuje vývojářům stránky používat tilda (`~`) k označení kořenové webové aplikace. V tomto kurzu lze použít, například notace `~/Admin/Default.aspx` v textu, který bude odkazovat na `Default.aspx` stránky v `Admin` složky. `~` Znamená, že `Admin` složka je podsložkou kořenový adresář webové aplikace.

`Control` Třídy [ `ResolveClientUrl` metoda](https://msdn.microsoft.com/library/system.web.ui.control.resolveclienturl.aspx) přebírá adresu URL a upraví relativní adresy URL vhodné pro webovou stránku, na kterém se ovládací prvek nachází. Například volání `ResolveClientUrl("~/Images/PoweredByASPNET.gif")` z `About.aspx` vrátí `Images/PoweredByASPNET.gif`. Volání z `~/Admin/Default.aspx`, ale vrátí `../Images/PoweredByASPNET.gif`.

> [!NOTE]
> Vzhledem k tomu, že jsou odvozeny od všech serverových ovládacích prvků ASP.NET `Control` třída, všechny ovládací prvky serveru mají přístup k `ResolveClientUrl` metoda. I v `Page` třída odvozená z `Control` třídy, což znamená, že můžete použít tuto metodu přímo z třídy kódu stránky ASP.NET.


### <a name="usingin-the-declarative-markup"></a>Pomocí`~`v deklarativní

Vlastnosti související s adresou URL zahrnují několik ovládacích prvků technologie ASP.NET: má ovládacího prvku hypertextový odkaz `NavigateUrl` vlastnost; bitovou kopii se má ovládací prvek `ImageUrl` vlastnost; a tak dále. Při vykreslování, tyto ovládací prvky předat jejich hodnoty související s adresou URL vlastností pro `ResolveClientUrl`. V důsledku toho pokud tyto vlastnosti obsahovat `~` k označení kořenové webové aplikace, se změní adresu URL platná relativní adresu URL.

Mějte na paměti, který transformuje pouze serverových ovládacích prvků ASP.NET `~` v jejich vlastnosti související s adresou URL. Pokud `~` se zobrazí v statické značka jazyka HTML, jako například `<img src="~/Images/PoweredByASPNET.gif" />`, odešle modul ASP.NET `~` do prohlížeče spolu s ostatními obsah HTML. V prohlížeči předpokládá, že `~` je součástí adresy URL. Například, pokud prohlížeč obdrží kód `<img src="~/Images/PoweredByASPNET.gif" />` předpokládá, že je pojmenovaná podsložka `~` s podsložku `Images` obsahující soubor bitové kopie `PoweredByASPNET.gif`.

Opravit značku bitové kopie v `Site.master`, nahradit existující `<img>` elementu pomocí prvku ASP.NET bitové kopie. Nastavení ovládacího prvku obrázek webové `ID` k `PoweredByImage`, jeho `ImageUrl` vlastnost `~/Images/PoweredByASPNET.gif`a jeho `AlternateText` vlastnost "Používá technologii ASP.NET!"


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample5.aspx)]

Po provedení této změny na stránku předlohy, pokroku `~/Admin/Default.aspx` stránku znovu. Tato doba `PoweredByASPNET.gif` soubor bitové kopie se zobrazí na stránce (viz obrázek 3). Když je ovládací prvek webu bitové kopie vygenerování používá `ResolveClientUrl` metoda vyřešit jeho `ImageUrl` hodnotu vlastnosti. V `~/Admin/Default.aspx` `ImageUrl` je převeden na příslušné relativní adresa URL, jako následující fragment kódu ukazuje zdroje HTML:


[!code-html[Main](urls-in-master-pages-cs/samples/sample6.html)]

> [!NOTE]
> Kromě použitá v vlastností ovládacího prvku webovou adresu URL, `~` lze také použít při volání metody `Response.Redirect` a `Server.MapPath` metody, mimo jiné. Navíc `ResolveClientUrl` metoda může být volána přímo z technologie ASP.NET nebo deklarativní stránky předlohy, v případě potřeby; viz [Fritz průsvitek](https://www.pluralsight.com/blogs/fritz/)na položce blogu [pomocí `ResolveClientUrl` ve značkách](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx).


## <a name="fixing-the-master-pages-remaining-relative-urls"></a>Oprava zbývající relativní adresy URL stránky předlohy

Kromě `<img>` element v `footerContent` právě vyřešili, že stránka předlohy obsahuje jeden více relativní adresu URL, která vyžaduje naše pozornost. `topContent` Oblast zahrnuje odkaz "Hlavní stránky kurzy,", který odkazuje na `Default.aspx`.


[!code-html[Main](urls-in-master-pages-cs/samples/sample7.html)]

Protože tato adresa URL je relativní, odešle uživateli `Default.aspx` stránku ve složce stránky obsahu, že se jedná o. Tak, aby měl tento odkaz, vždy přejděte na `Default.aspx` v kořenové složce je potřeba nahradit `<a>` element s HyperLink Web řídit tak, aby můžeme použít `~` zápis.

Odeberte `<a>` element kódu a přidání ovládacího prvku hypertextový odkaz na jeho místo. Nastavte na hypertextový odkaz `ID` k `lnkHome`, jeho `NavigateUrl` vlastnost `~/Default.aspx`a jeho `Text` vlastnost "Hlavní stránky kurzy."


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample8.aspx)]

Je to! V tomto okamžiku všechny adresy URL v naší hlavní stránce správně na základě při stránky předlohy a stránky obsahu pro vykreslení stránky obsahu bez ohledu na to, jaké složky jsou umístěny v.

### <a name="automatic-url-resolution-in-theheadsection"></a>URL adresy automatických řešení v`<head>`části

V [ *vytváření celém webu rozložení pomocí stránky předlohy* ](creating-a-site-wide-layout-using-master-pages-cs.md) kurzu jsme přidali `<link>` k `Styles.css` v soubor `<head>` oblast:


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample9.aspx)]

Když `<link>` elementu `href` atribut je relativní, je automaticky převeden na správnou cestu za běhu. Jsme popsané v [ *zadáte název, značky Meta a ostatní hlavičky HTML na hlavní stránce* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) kurzu `<head>` oblast je ve skutečnosti serverové řízení, které umožní, aby upravit obsah jeho vnitřní ovládacích prvků je vykreslen.

Chcete-li to ověřit pokroku `~/Admin/Default.aspx` stránky a zobrazit zdrojový kód HTML, který je odesláno prohlížeči. Jak ukazuje následující fragment, `<link>` elementu `href` atribut automaticky změnilo na odpovídající relativní adresu URL, `../Styles.css`.


[!code-html[Main](urls-in-master-pages-cs/samples/sample10.html)]

## <a name="summary"></a>Souhrn

Hlavní stránky velmi často obsahují odkazy, Image a další externí prostředky, které je třeba zadat prostřednictvím adresy URL. Protože stránky předlohy a stránky obsahu nemusí existovat ve stejné složce, je důležité zdržet pomocí relativní adresy URL. I když je možné použít naprogramováno absolutní adresy URL, takže úzce provádění páry v odstupu absolutní adresa URL webové aplikace. Pokud se absolutní adresa URL změní - často stejně jako při přesunutí nebo nasazení webové aplikace - budete muset mějte na paměti, přejděte zpět a aktualizovat absolutní adresy URL.

Ideální možných přístupů je použít tilda (`~`) k označení kořenový adresář aplikace. Mapování ovládacích prvků technologie ASP.NET obsahující vlastnosti týkající se adresa URL `~` do kořenového adresáře aplikace za běhu. Interně, použijte ovládací prvky webového `Control` třídy `ResolveClientUrl` metoda ke generování platná relativní adresy URL. Tato metoda je veřejná a dostupný z každého serveru ovládacího prvku (včetně `Page` třída), abyste mohli používat je prostřednictvím kódu programu z vašeho kódu třídy, v případě potřeby.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Stránky předlohy technologie ASP.NET](http://www.odetocode.com/Articles/419.aspx)
- [Adresa URL Rebasing na hlavní stránce](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx#urls)
- [Pomocí `ResolveClientUrl` v kódu](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)

### <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor více knih ASP/ASP.NET a zakladatele 4GuysFromRolla.com, pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologie ASP.NET 3.5 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott lze dosáhnout za [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu v [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Zvláštní poděkování

Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Předchozí](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)
> [další](control-id-naming-in-content-pages-cs.md)
