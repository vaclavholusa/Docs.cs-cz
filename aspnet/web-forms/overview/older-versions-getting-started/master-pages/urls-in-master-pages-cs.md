---
uid: web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-cs
title: Adresy URL stránek předloh (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Řeší, jak můžete přerušit adresy URL na hlavní stránce kvůli soubor předlohové stránky se v jiném adresáři relativní než stránku obsahu. Prohledá probíhá přenesení změn...
ms.author: aspnetcontent
ms.date: 06/10/2008
ms.assetid: 48b58a18-5ea4-468c-b326-f35331b3e1e9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 827c471074fbfeb049613f5cc5ffb82937284f00
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821557"
---
<a name="urls-in-master-pages-c"></a>Adresy URL stránek předloh (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_04_CS.zip) nebo [stahovat PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_04_CS.pdf)

> Řeší, jak můžete přerušit adresy URL na hlavní stránce kvůli soubor předlohové stránky se v jiném adresáři relativní než stránku obsahu. Prohledá probíhá přenesení změn adres URL prostřednictvím ~ v deklarativní syntaxi a použití ResolveUrl a ResolveClientUrl prostřednictvím kódu programu. (Se také podívat na


## <a name="introduction"></a>Úvod

Ve všech příkladech jsme viděli, jaký že se stránky předlohy a obsahu byly umístěny ve stejné složce (do kořenové složky webu). Ale neexistuje žádný důvod, proč se stránky předlohy a obsahu musí být ve stejné složce. Určitě můžete vytvářet stránky obsahu v podsložkách. Podobně můžete vytvořit `~/MasterPages/` složku, kam umístíte hlavní stránky.

Jedním potenciálním problémem s umístěním stránky předlohy a obsahu v různých složkách zahrnuje nefunkční adresy URL. Pokud hlavní stránka obsahuje relativní adresy URL v hypertextových odkazů, obrázků nebo jiných prvků, bude odkaz na neplatný pro stránky obsahu, které se nacházejí v jiné složce. V tomto kurzu jsme zkontrolujte příčiny tohoto problému, jakož i řešení.

## <a name="the-problem-with-relative-urls"></a>Problém s relativní adresy URL

Na adresu URL na webovou stránku se říká, že *relativní adresa URL* při umístění prostředku odkazovala na relativní k umístění webové stránky ve struktuře složek na webu. Libovolnou adresu URL, který nezačíná řetězcem počáteční lomítko (`/`) nebo protokol (například `http://`) je relativní vzhledem k tomu, že je přeložen v prohlížeči na základě umístění webové stránky, který obsahuje adresu URL.

Například, má náš web `~/Images/` složky se souborem jedné image `PoweredByASPNET.gif`. Soubor předlohové stránky daného `Site.master` má `<img>` prvek `footerContent` oblasti následujícím kódem:


[!code-html[Main](urls-in-master-pages-cs/samples/sample1.html)]

`src` Hodnotu v atributu `<img>` prvek je relativní adresa URL, protože nezačíná řetězcem `/` nebo `http://`. Stručně řečeno `src` hodnota atributu sděluje prohlížeči, aby podívejte se `Images` podsložku pro soubor s názvem `PoweredByASPNET.gif`.

Při návštěvě stránky obsahu, výše uvedené značky se pošle přímo v prohlížeči. Za chvíli navštivte `About.aspx` a zobrazit zdrojový kód HTML, který je odesláno prohlížeči. Zjistíte, že přesně stejné značky na stránce předlohy byl odeslán do prohlížeče.


[!code-html[Main](urls-in-master-pages-cs/samples/sample2.html)]

Pokud stránka obsahu je v kořenové složce (jako je `About.aspx`) všechno funguje podle očekávání, protože je `Images` podsložka relativně ke kořenové složce. Ale co rozdělit Pokud stránka obsahu je v jiné složce než stránky předlohy. Pro znázornění, vytvořte podsložku s názvem `Admin`. V dalším kroku přidat stránku obsahu s názvem `Default.aspx` k `Admin` složky, nezapomeňte vytvořit vazbu na novou stránku `Site.master` stránky předlohy.

> [!NOTE]
> V [ *zadáním názvu, metaznaček a ostatní hlaviček HTML na stránce předlohy* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) kurzu jsme vytvořili vlastní stránku základní třídu s názvem `BasePage` , který automaticky nastavit nadpis obsahu stránky (pokud ho nebyla přiřazena explicitně). Nezapomeňte mají třídy modelu code-behind nově vytvořený stránky odvozen od `BasePage` tak, aby ji můžete využít tuto funkci.


Po vytvoření tohoto obsahu stránky, by měl vypadat podobně jako na obrázku 1 Průzkumníku řešení.


![Nová složka a stránky ASP.NET se přidaly do projektu](urls-in-master-pages-cs/_static/image1.png)

**Obrázek 01**: novou složku a stránky ASP.NET se přidaly do projektu


Dále, aktualizujte `Web.sitemap` soubor obsahuje nové `<siteMapNode>` zadání v této lekci. Následující kód XML ukazuje kompletní `Web.sitemap` kód, který teď zahrnuje přidání třetí `<siteMapNode>` elementu.


[!code-xml[Main](urls-in-master-pages-cs/samples/sample3.xml)]

Nově vytvořený `Default.aspx` stránka by měla obsahovat čtyři ovládací prvky obsahu odpovídající čtyři prvků ContentPlaceHolder v `Site.master`. Přidejte nějaký text pro ovládací prvek obsahu odkazující `MainContent` ContentPlaceHolder a pak na stránce prostřednictvím prohlížeče. Jak je vidět na obrázku 2, nejde najít v prohlížeči `PoweredByASPNET.gif` soubor bitové kopie. K čemu tady?

`~/Admin/Default.aspx` Stránky obsahu se odešle stejný kód HTML `footerContent` oblasti jako byla `About.aspx` stránky:


[!code-html[Main](urls-in-master-pages-cs/samples/sample4.html)]

Protože `<img>` elementu `src` atribut je relativní adresa URL, v prohlížeči se pokusí vyhledat `Images` složce relativní k umístění složky webové stránky. Jinými slovy, v prohlížeči hledá soubor bitové kopie `Admin/Images/PoweredByASPNET.gif`.


[![Soubor bitové kopie PoweredByASPNET.gif nebyl nalezen.](urls-in-master-pages-cs/_static/image3.png)](urls-in-master-pages-cs/_static/image2.png)

**Obrázek 02**: `PoweredByASPNET.gif` Image soubor nebyl nalezen ([kliknutím ji zobrazíte obrázek v plné velikosti](urls-in-master-pages-cs/_static/image4.png))


### <a name="replacing-relative-urls-with-absolute-urls"></a>Nahraďte absolutní adresy URL relativní adresy URL

Je opakem relativní adresu URL *absolutní adresa URL*, což je jedna začínající lomítkem (`/`) nebo protokol, jako `http://`. Protože absolutní adresa URL určuje umístění prostředku ze známých pevnému bodu, je stejné absolutní adresa URL platná v jakékoli webové stránky, bez ohledu na umístění webové stránky ve struktuře složek na webu.

Chcete-li napravit porušení obrázku je znázorněno na obrázku 2, musíme aktualizovat `<img>` elementu `src` atribut tak, aby používal místo relativní absolutní adresu URL. Chcete-li určit správnou absolutní adresu URL, navštíví některý z webové stránky na webu a zkontrolujte do adresního řádku. Jak ukazuje do adresního řádku na obrázku 2 je plně kvalifikovanou cestu k webové aplikaci `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_CS/`. Proto jsme aktualizovat `<img>` elementu `src` atribut některou z následujících dvou absolutní adresy URL:

- `/ASPNET_MasterPages_Tutorial_04_CS/Images/PoweredByASPNET.gif`
- `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_CS/Images/PoweredByASPNET.gif`

Za chvíli se aktualizovat `<img>` elementu `src` atributu na absolutní adresu URL pomocí jedné z formuláře uvedené nahoře a přejděte `~/Admin/Default.aspx` stránky prostřednictvím prohlížeče. Tentokrát se správně najít a zobrazit v prohlížeči `PoweredByASPNET.gif` soubor bitové kopie (viz obrázek 3).


[![Obrázek PoweredByASPNET.gif se nyní zobrazí](urls-in-master-pages-cs/_static/image6.png)](urls-in-master-pages-cs/_static/image5.png)

**Obrázek 03**: `PoweredByASPNET.gif` bitová kopie je nyní zobrazen ([kliknutím ji zobrazíte obrázek v plné velikosti](urls-in-master-pages-cs/_static/image7.png))


Při pevném psaní kódu v absolutní adresa URL funguje pevně se spojuje kódu HTML k serveru na webu a umístění složky, které mohou změnit. Použití absolutní adresu URL ve formátu `http://localhost:3908/...` je křehká protože předchozí číslo portu `localhost` automaticky vybraný při každém spuštění sady Visual Studio integrované technologie ASP.NET vývojového webového serveru. Podobně platí `http://localhost` část je platný pouze při testování místně. Jakmile se kód nasazuje na produkční server, základ adresy URL změní na něco jiného, jako je třeba `http://www.yourserver.com`. Absolutní adresa URL ve formě `/ASPNET_MasterPages_Tutorial_04_CS/...` také vykazuje stejné brittleness, protože často cesta k této aplikaci liší mezi vývojovou a provozní servery.

Dobrou zprávou je, že technologie ASP.NET nabízí metody pro generování platnou relativní adresu URL za běhu.

## <a name="usingandresolveclienturl"></a>Pomocí`~`a`ResolveClientUrl`

Spíše než intenzivně kódu absolutní adresu URL, ASP.NET stránky vývojářům umožňuje použít tilda (`~`) k označení kořenovém adresáři webové aplikace. Například dříve v tomto kurzu, můžu použít zápis `~/Admin/Default.aspx` v text, který má odkazovat `Default.aspx` stránku `Admin` složky. `~` Znamená, že `Admin` složka je podsložkou kořenový adresář webové aplikace.

`Control` Třídy [ `ResolveClientUrl` metoda](https://msdn.microsoft.com/library/system.web.ui.control.resolveclienturl.aspx) přebírá adresu URL a změní na relativní adresu URL vhodné pro webové stránky, na kterém se ovládací prvek nachází. Například volání `ResolveClientUrl("~/Images/PoweredByASPNET.gif")` z `About.aspx` vrátí `Images/PoweredByASPNET.gif`. Volání z `~/Admin/Default.aspx`, ale vrátí `../Images/PoweredByASPNET.gif`.

> [!NOTE]
> Protože všechny serverové ovládací prvky technologie ASP.NET jsou odvozeny z `Control` třídu, všechny ovládací prvky serveru mají přístup k `ResolveClientUrl` metody. Dokonce i pomocí `Page` třída odvozena z `Control` třídy, což znamená, že můžete použít tuto metodu přímo ze stránky technologie ASP.NET použití modelu code-behind tříd.


### <a name="usingin-the-declarative-markup"></a>Pomocí`~`v deklarativním označení

Vlastnosti související s adresou URL zahrnují několik ovládacích prvků technologie ASP.NET: má ovládací prvek hypertextového odkazu `NavigateUrl` vlastnost; Image má ovládací prvek `ImageUrl` vlastnost; a tak dále. Při vykreslování, předejte tyto ovládací prvky jejich hodnoty vlastností související s adresou URL, které `ResolveClientUrl`. V důsledku toho pokud tyto vlastnosti obsahovat `~` k označení kořenovém adresáři webové aplikace, adresa URL bude upraveno pro platnou relativní adresou URL.

Mějte na paměti, který převádí pouze serverových ovládacích prvků ASP.NET `~` v jejich vlastnosti související s adresou URL. Pokud `~` se zobrazí v statický kód HTML, jako například `<img src="~/Images/PoweredByASPNET.gif" />`, odešle modul ASP.NET `~` spolu se zbývajícími obsah HTML v prohlížeči. Prohlížeč předpokládá, že `~` je součástí adresy URL. Například, pokud prohlížeč obdrží značky `<img src="~/Images/PoweredByASPNET.gif" />` předpokládá, že je podsložku s názvem `~` podsložku `Images` , která obsahuje soubor bitové kopie `PoweredByASPNET.gif`.

K vyřešení značky obrázku v `Site.master`, nahraďte existující `<img>` element s ovládacím prvkem obrázku v prostředí ASP.NET. Nastavení ovládacího prvku obrázek webové `ID` k `PoweredByImage`, jeho `ImageUrl` vlastnost `~/Images/PoweredByASPNET.gif`a jeho `AlternateText` vlastnost "Používá technologii ASP.NET!"


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample5.aspx)]

Po provedení této změny na stránce předlohy, opakování `~/Admin/Default.aspx` stránku znovu nezobrazovat. Tentokrát `PoweredByASPNET.gif` soubor obrázku se zobrazí na stránce (viz obrázek 3). Když je ovládací prvek webu Image vygenerování používá `ResolveClientUrl` metoda vyřešit jeho `ImageUrl` hodnotu vlastnosti. V `~/Admin/Default.aspx` `ImageUrl` se převede na příslušnou relativní adresu URL jako následující fragment zobrazí zdroj HTML:


[!code-html[Main](urls-in-master-pages-cs/samples/sample6.html)]

> [!NOTE]
> Kromě se používají ve vlastnosti ovládacích prvků webové adresy URL, `~` lze také při volání `Response.Redirect` a `Server.MapPath` metody, mimo jiné. Navíc `ResolveClientUrl` metody mohou být vyvolány přímo z technologie ASP.NET nebo deklarativním označení stránky předlohy, v případě potřeby; viz [Fritzovi průsvitek](https://www.pluralsight.com/blogs/fritz/)na blogu [použití `ResolveClientUrl` ve značkách](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx).


## <a name="fixing-the-master-pages-remaining-relative-urls"></a>Oprava zbývající relativní adresy URL stránky předlohy

Kromě `<img>` prvek `footerContent` , že jsme odstranili, hlavní stránky obsahuje jeden další relativní adresu URL, která vyžaduje pozornost. `topContent` Zahrnuje odkaz "Hlavní stránky kurzy," která odkazuje na oblast `Default.aspx`.


[!code-html[Main](urls-in-master-pages-cs/samples/sample7.html)]

Protože tato adresa URL je relativní, uživateli se odešle `Default.aspx` stránku ve složce stránky obsahu se navštívit. Mít tento odkaz, vždy přejděte na `Default.aspx` v kořenové složce budeme muset nahradit `<a>` tak, že můžeme použít ovládací prvek elementu s webovým hypertextový odkaz `~` zápis.

Odeberte `<a>` značka elementu a místo něj přidat ovládací prvek hypertextového odkazu. Nastavte na hypertextový odkaz `ID` k `lnkHome`, jeho `NavigateUrl` vlastnost `~/Default.aspx`a jeho `Text` vlastnost "Hlavní stránky kurzy."


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample8.aspx)]

Je to! V tomto okamžiku všech adres URL v naší hlavní stránky jsou závislosti správně při zobrazení obsahu stránka bez ohledu na to, jaké složky stránky předlohy a obsahu stránky jsou umístěné v.

### <a name="automatic-url-resolution-in-theheadsection"></a>URL adresy automatických řešení v`<head>`oddílu

V [ *vytváření webu rozložení pomocí stránek předlohy* ](creating-a-site-wide-layout-using-master-pages-cs.md) kurzu jsme přidali `<link>` k `Styles.css` soubor `<head>` oblasti:


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample9.aspx)]

Zatímco `<link>` elementu `href` atribut je relativní, je automaticky převedena na správnou cestu v době běhu. Jak jsme probírali v [ *zadáním názvu, metaznaček a ostatní hlaviček HTML na stránce předlohy* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) kurzu `<head>` oblasti je ve skutečnosti ovládací prvek na straně serveru, který umožňuje změnit obsah vnitřní ovládacích prvků při vykreslení.

Chcete-li to ověřit, opakování `~/Admin/Default.aspx` stránky a zobrazit zdrojový kód HTML, který je odesláno prohlížeči. Jak ukazuje následující fragment `<link>` elementu `href` atribut byl automaticky změněn na odpovídající relativní adresu URL, `../Styles.css`.


[!code-html[Main](urls-in-master-pages-cs/samples/sample10.html)]

## <a name="summary"></a>Souhrn

Hlavní stránky velmi často obsahují odkazy, obrázky a další externí prostředky, které musí být zadán prostřednictvím adresy URL. Stránky předlohy a obsahu stránek nemusí existovat ve stejné složce, proto je důležité zdržet relativní adresy URL. I když je možné použít pevně zakódované absolutní adresy URL, to tedy úzce páry v odstupu absolutní adresu URL na webovou aplikaci. Pokud se absolutní adresa URL změní – jak často se při přesunutí nebo nasazení webové aplikace – budete mít Nezapomeňte přejít zpět a aktualizovat absolutní adresy URL.

Ideální přístupem je použití tilda (`~`) k označení kořenový adresář aplikace. Ovládací prvky technologie ASP.NET, obsahující vlastnosti související s adresou URL, které mapují `~` do kořenového adresáře aplikace za běhu. Interně, použijte ovládací prvky webového `Control` třídy `ResolveClientUrl` metoda ke generování platnou relativní adresou URL. Tato metoda je veřejná a dostupná z každé serverový ovládací prvek (včetně `Page` třídy), takže můžete prostřednictvím kódu programu z vašeho použití modelu code-behind třídy, v případě potřeby.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Stránky předlohy v ASP.NET](http://www.odetocode.com/Articles/419.aspx)
- [Adresa URL probíhá přenesení změn do hlavní stránky](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx#urls)
- [Pomocí `ResolveClientUrl` v kódu](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)

### <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor více ASP/ASP.NET knih a Zakladatel 4GuysFromRolla.com pracuje s Microsoft webových technologií od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami technologie ASP.NET 3.5 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott může být dostupný na adrese [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím na svém blogu [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Speciální k

Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Předchozí](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)
> [další](control-id-naming-in-content-pages-cs.md)
