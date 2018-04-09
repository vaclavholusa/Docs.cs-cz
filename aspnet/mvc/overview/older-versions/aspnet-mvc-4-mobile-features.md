---
uid: mvc/overview/older-versions/aspnet-mvc-4-mobile-features
title: Funkce mobilní architektury ASP.NET MVC 4 | Microsoft Docs
author: Rick-Anderson
description: Je nyní ve verzi MVC 5 tohoto kurzu s ukázky kódu v nasazení technologie ASP.NET MVC 5 mobilní webové aplikace na weby Azure.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 27dc4fc8-1b51-43b0-933f-fc1b52476523
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/aspnet-mvc-4-mobile-features
msc.type: authoredcontent
ms.openlocfilehash: 5f38fcdd8e71ce12f7899214b6b2133e21f9910c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-mvc-4-mobile-features"></a>Funkce mobilní architektury ASP.NET MVC 4
====================
podle [Rick Anderson](https://github.com/Rick-Anderson)

> Je nyní verze MVC 5 s ukázky kódu v tomto kurzu [nasazení ASP.NET MVC 5 mobilní webové aplikace na weby Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/).


V tomto kurzu naučit základní informace o tom, jak pracovat s mobilní funkce v aplikaci ASP.NET MVC 4 Web. V tomto kurzu můžete použít [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) nebo Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer nebo VWD&quot;). Pokud již máte, můžete professional verze sady Visual Studio.

Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže.

- [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) (doporučeno) nebo Visual Studio Web Developer Express SP1. Visual Studio 2012 obsahuje ASP.NET MVC 4. Pokud používáte Visual Web Developer 2010, je nutné nainstalovat [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392).

Budete také potřebovat emulátoru prohlížeč pro mobilní zařízení. Bude fungovat některé z následujících:

- [Emulátor Windows 7 Phone](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). (Toto je emulátor, který se používá ve většině snímky obrazovky v tomto kurzu).
- Změňte řetězec uživatelského agenta emulovat zařízení typu iPhone. V tématu [to](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) položku blogu.
- [Emulátoru mobilního Opera](http://www.opera.com/developer/tools/mobile/)
- [Apple Safari](http://www.apple.com/safari/download/) s uživatelský agent nastavena na zařízení iPhone. Pokyny, jak nastavit uživatelský agent v prohlížeči Safari na "iPhone", najdete v části [jak umožníte Safari předstírají, že je aplikace Internet Explorer](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) na blogu David Alison.

Projekty Visual Studio se zdrojovým kódem C# jsou k dispozici v tomto tématu:

- [Stažení Starter projektu](https://go.microsoft.com/fwlink/?linkid=228307&amp;clcid=0x409)
- [Dokončení stažení projektu](https://go.microsoft.com/fwlink/?linkid=228306&amp;clcid=0x409)

### <a name="what-youll-build"></a>Co budete sestavení

V tomto kurzu přidáte mobilní funkce pro jednoduchou aplikaci seznamu konference, která je součástí [starter projektu](https://go.microsoft.com/fwlink/?LinkId=228307). Následující snímek obrazovky ukazuje na stránku značky hotová aplikace, jak je vidět [emulátor Windows Phone 7](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). V tématu [klávesnice mapování pro Windows Phone emulátoru](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx) zjednodušit vstup z klávesnice.

[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)

Můžete použít Internet Explorer verze 9 nebo 10, k vývoji mobilních aplikací pomocí prohlížeče FireFox nebo Chrome [řetězec uživatelského agenta](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/). Následující obrázek znázorňuje dokončené kurz pomocí Internet Exploreru emulace zařízení typu iPhone. Můžete použít nástroje pro vývojáře F-12 Internet Explorer a [nástroj Fiddler](http://www.fiddler2.com/fiddler2/) pomoci při ladění aplikace.

![](aspnet-mvc-4-mobile-features/_static/image3.png)

### <a name="skills-youll-learn"></a>Dovedností, které se dozvíte

Zde je, co se dozvíte:

- Jak používat šablony ASP.NET MVC 4 HTML5 `viewport` atribut a adaptivního vykreslování ke zlepšení zobrazit na mobilních zařízeních.
- Postup vytvoření mobilní konkrétní zobrazení.
- Postup vytvoření přepínači zobrazení této umožňuje uživatelům přepínání mezi mobilní zobrazení a zobrazení plochy aplikace.

### <a name="getting-started"></a>Začínáme

Stažení aplikace konferenční výpis pro projekt starter pomocí následujícího odkazu: [Stáhnout](https://go.microsoft.com/fwlink/?LinkId=228307). V Průzkumníku Windows, klikněte pravým tlačítkem myši *MvcMobile.zip* souboru a zvolte **vlastnosti**. V **MvcMobile.zip vlastnosti** dialogovém okně vyberte **Odblokovat** tlačítko. (Odblokování brání upozornění zabezpečení, která nastane, když se pokusíte použít *.zip* souboru, který jste stáhli z webu.)

![p1_unBlock](aspnet-mvc-4-mobile-features/_static/image4.png)

Klikněte pravým tlačítkem myši *MvcMobile.zip* soubor a vyberte **Extrahovat vše** soubor rozbalit. V sadě Visual Studio, otevřete *MvcMobile.sln* souboru.

Stisknutím kombinace kláves CTRL + F5 a spusťte aplikaci, které se zobrazí v prohlížeči klientů. Spusťte emulátor váš prohlížeč pro mobilní zařízení, zkopírujte adresu URL pro aplikaci konferenční do emulátoru a klikněte **Procházet podle značky** odkaz. Pokud používáte emulátor Windows Phone, klikněte na panelu Adresa URL a stisknutím klávesy pozastavení a získat tak přístup klávesnice. Obrázek níže znázorňuje *AllTags* zobrazení (z výběru **Procházet podle značky**).

[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)

Zobrazení je velmi čitelná na mobilním zařízení. Vyberte odkaz ASP.NET.

[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)

Zobrazení značek ASP.NET je velmi nahuštěných. Například **datum** sloupec je velmi obtížné číst. Později v tomto kurzu vytvoříte z verze *AllTags* zobrazení, který je speciálně pro mobilní prohlížeče a který bude zobrazení čitelné.

Poznámka: Aktuálně chyby existuje v mobilních modul ukládání do mezipaměti. Aplikacích v produkčním prostředí, je nutné nainstalovat [pevné DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) balíček nuget. V tématu [ASP.NET MVC 4 Mobile ukládání do mezipaměti chyb pevné](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) podrobnosti o opravu.

## <a name="css-media-queries"></a>Dotazy na média šablon stylů CSS

[Dotazy na média šablon stylů CSS](http://www.w3.org/TR/css3-mediaqueries/) , jsou rozšíření do šablon stylů CSS pro typy médií. Umožňují vám umožňují vytvořit pravidla, které potlačí výchozí pravidla šablon stylů CSS pro konkrétní prohlížeče (Uživatelští agenti). Běžné pravidlo CSS, která je cílena mobilní prohlížeče je definování obrazovky maximální velikost. *Content\Site.css* soubor, který se vytvoří při vytvoření nového projektu ASP.NET MVC 4 Internet obsahuje následující dotaz média:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample1.css)]

Pokud 850 pixelů, maximálně okně prohlížeče se bude používat pravidla šablon stylů CSS v tomto bloku média. Dotazy na média šablon stylů CSS takto můžete zajistit lepší zobrazení obsah HTML na malé prohlížeče (např. mobilní prohlížeče) než výchozí pravidla šablon stylů CSS, která jsou navrženy pro širší zobrazí plochy prohlížečů.

## <a name="the-viewport-meta-tag"></a>Značka Meta zobrazení

Většina mobilních prohlížečů definovat šířku okna virtuální prohlížeče ( *zobrazení*), je mnohem větší než skutečná šířka mobilního zařízení. To umožňuje mobilní prohlížeče přizpůsobit celou webovou stránku uvnitř virtuální zobrazení. Uživatele můžete pak přiblížit zajímavé obsahu. Ale pokud nastavíte Šířka zobrazení na šířku skutečné zařízení, bez měřítka je nutná, protože obsah vejde do mobilní prohlížeče.

Zobrazení `<meta>` značky v architektuře ASP.NET MVC 4 rozložení souboru Nastaví zobrazení na šířku zařízení. Následující řádek ukazuje zobrazení `<meta>` značky v souboru rozložení ASP.NET MVC 4.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample2.html)]

## <a name="examining-the-effect-of-css-media-queries-and-the-viewport-meta-tag"></a>Zkoumání účinku dotazy na média šablon stylů CSS a metaznačku zobrazení

Otevřete *Views\Shared\\_Layout.cshtml* souboru v editoru a Odkomentujte zobrazení `<meta>` značky. Následující kód ukazuje komentované řádku.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample3.cshtml)]

Otevřete *MvcMobile\Content\Site.css* souboru v editoru a změňte maximální šířka v media dotaz na nula pixelů. To znemožní pravidla šablon stylů CSS používá v mobilních prohlížečích. Následující řádek ukazuje upravené media dotaz:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample4.css)]

Uložte změny a přejděte na konferenci aplikaci v emulátoru prohlížeč pro mobilní zařízení. Malá velikost textu na následujícím obrázku je výsledek odebrání zobrazení `<meta>` značky. S žádné zobrazení `<meta>` značky, v prohlížeči je zmenšení zobrazení na šířku výchozí zobrazení (850 pixelů nebo širší u většiny mobilních prohlížečů.)

[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)

Vrátit zpět změny – zrušte komentář u zobrazení `<meta>` značky v souboru rozložení a obnovit media dotaz 850 pixelů *Site.css* souboru. Změny uložte a aktualizujte prohlížeč pro mobilní zařízení k ověření, že byla obnovena zobrazení mobilní zařízení.

Zobrazení `<meta>` značky a šablon stylů CSS media dotaz nejsou specifické pro ASP.NET MVC 4, a můžete využít výhod těchto funkcí v jakékoli webové aplikace. Ale jsou teď integrované ve soubory, které jsou generovány, pokud vytvoříte nový projekt ASP.NET MVC 4.

Další informace o zobrazení `<meta>` značky, najdete v části [indikaci dvě okna zobrazení – druhá část](http://www.quirksmode.org/mobile/viewports2.html).

V další části se zobrazí, jak poskytnout konkrétní zobrazení mobilní prohlížeče.

## <a name="overriding-views-layouts-and-partial-views"></a>Přepsání, zobrazení, částečná zobrazení a rozložení

Významné nová funkce v rozhraní ASP.NET MVC 4 je jednoduchý mechanismus, který umožňuje přepsat všechna zobrazení (včetně rozložení a částečné zobrazení) pro mobilní prohlížeče obecně pro jednotlivé mobilního prohlížeče nebo pro jakékoli konkrétní prohlížeč. Abyste si mohli zobrazit konkrétní mobilní, můžete zkopírovat soubor zobrazení a přidat *. Mobilní* k názvu souboru. Chcete-li například vytvořit mobilních *Index* zobrazit, Kopírovat *Views\Home\Index.cshtml* k *Views\Home\Index.Mobile.cshtml*.

V této části vytvoříte soubor specifické mobilní rozložení.

Chcete-li začít, zkopírujte *Views\Shared\\_Layout.cshtml* k *Views\Shared\\_Layout.Mobile.cshtml*. Otevřete  *\_Layout.Mobile.cshtml* a změňte název od **MVC4 konference** k **konference (mobilní)**.

V každé `Html.ActionLink` volání, odeberte "Procházet podle" v každé propojení *ActionLink*. Následující kód ukazuje části dokončené textu mobilní rozložení souboru.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample5.cshtml)]

Kopírování *Views\Home\AllTags.cshtml* do souboru *Views\Home\AllTags.Mobile.cshtml*. Otevřete nový soubor a změňte `<h2>` element z "Značky" na "značky (M)":

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample6.html)]

Přejděte na stránku značky pomocí prohlížeč pro stolní počítač a pomocí emulátoru prohlížeč pro mobilní zařízení. Emulátor mobilní prohlížeče ukazuje dva provedené změny.

[![p2m_layoutTags.mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)

Naproti tomu nebylo změněno zobrazení plochy.

[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)

## <a name="browser-specific-views"></a>Zobrazení specifické pro prohlížeč

Kromě zobrazení specifická pro mobilní a desktop můžete vytvořit zobrazení pro jednotlivé prohlížeče. Můžete například vytvořit zobrazení, které jsou speciálně určené pro iPhone prohlížeče. V této části vytvoříte rozložení pro prohlížeč iPhone a na zařízení iPhone verzi *AllTags* zobrazení.

Otevřete *Global.asax* souboru a přidejte následující kód, který `Application_Start` metoda.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample7.cs)]

Tento kód definuje nový režim zobrazení s názvem "iPhone", který bude porovnání jednotlivých příchozích požadavků. Pokud příchozí požadavek odpovídá podmínku, kterou jste definovali (Pokud uživatelský agent obsahuje řetězec "iPhone"), rozhraní ASP.NET MVC bude hledat zobrazení, jejíž název obsahuje příponu "iPhone".

V kódu, klikněte pravým tlačítkem na `DefaultDisplayMode`, zvolte **vyřešit**a potom vyberte `using System.Web.WebPages;`. Tento postup přidá odkaz na `System.Web.WebPages` názvů, který je tam, kde `DisplayModes` a `DefaultDisplayMode` typy jsou definované.

[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)

Alternativně můžete právě ručně přidejte následující řádek na `using` část souboru.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample8.cs)]

Úplný obsahu *Global.asax* souboru je uveden níže.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample9.cs)]

Uložte změny. Kopírování *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* do souboru *MvcMobile\Views\Shared\\_Layout.iPhone.cshtml*. Otevřete nový soubor a potom změňte `h1` nadpis z `Conference (Mobile)` k `Conference (iPhone)`.

Kopírování *MvcMobile\Views\Home\AllTags.Mobile.cshtml* do souboru *MvcMobile\Views\Home\AllTags.iPhone.cshtml*. Nový soubor, změňte `<h2>` element z "značky (M)" pro "Značky (iPhone)".

Spusťte aplikaci. Spustit prohlížeč pro mobilní zařízení emulátoru, ujistěte se jeho uživatelský agent je nastavena na "iPhone" a přejděte do *AllTags* zobrazení. Následující snímek obrazovky ukazuje *AllTags* v vykreslit zobrazení [Safari](http://www.apple.com/safari/download/) prohlížeče. Prohlížeč Safari pro systém Windows si můžete stáhnout [zde](https://support.apple.com/kb/DL1531).

[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)

V této části jsme viděli postup vytvoření mobilní rozložení a zobrazení a postup vytvoření zobrazení pro konkrétní zařízení, jako je iPhone a rozložení. V další části se zobrazí, jak využít jQuery Mobile pro další poutavé mobilní zobrazení.

## <a name="using-jquery-mobile"></a>Pomocí technologie jQuery Mobile

[JQuery Mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html) knihovna obsahuje rozhraní framework uživatele, který funguje na všechny hlavní mobilní prohlížeče. platí pro architekturu jQuery Mobile *postupném rozšiřování* do mobilních prohlížečů, které podporují šablon stylů CSS a JavaScript. Progresivní vylepšení umožňuje všechny prohlížeče zobrazíte základní obsah na webové stránce, zatímco výkonnější prohlížeče a zařízení, které mají širší zobrazení. JavaScript a CSS soubory, které jsou součástí jQuery Mobile stylů mnoho prvků podle mobilní prohlížeče bez jakýchkoli změn kódu.

V této části nainstalujete *jQuery.Mobile.MVC* balíček NuGet, který se nainstaluje jQuery Mobile a widget přepínači zobrazení.

Chcete-li začít, odstraňte *sdílené\\_Layout.Mobile.cshtml* a *sdílené\\_Layout.iPhone.cshtml* soubory, které jste vytvořili dříve.

Přejmenování *Views\Home\AllTags.Mobile.cshtml* a *Views\Home\AllTags.iPhone.cshtml* souborů do *Views\Home\AllTags.iPhone.cshtml.hide* a  *Views\Home\AllTags.Mobile.cshtml.hide*. Protože soubory již nemáte *.cshtml* rozšíření, nebudou použity modulem runtime ASP.NET MVC pro vykreslení *AllTags* zobrazení.

Nainstalujte *jQuery.Mobile.MVC* balíček NuGet tímto způsobem:

1. Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**a potom vyberte **Konzola správce balíčků**.

    [![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)
2. V **Konzola správce balíčků**, zadejte `Install-Package jQuery.Mobile.MVC -version 1.0.0`

Následující obrázek ukazuje soubory přidávání a měnění do projektu MvcMobile jQuery.Mobile.MVC balíček NuGet. Soubory, které jsou přidány [přidat] připojí po název souboru. Obrázek nezobrazuje GIF, soubory PNG přidat do *Content\images* složky.

![](aspnet-mvc-4-mobile-features/_static/image21.png)

Balíček NuGet jQuery.Mobile.MVC nainstaluje následující:

- *Aplikace\_Start\BundleMobileConfig.cs* soubor, který je nutný k odkazování jQuery JavaScript a CSS souborů přidán. Musíte podle pokynů níže a odkazovat na sadu mobilní definované v tomto souboru.
- soubory jQuery Mobile šablon stylů CSS.
- A `ViewSwitcher` řadiče pomůcky (*Controllers\ViewSwitcherController.cs*).
- soubory jQuery Mobile JavaScript.
- JQuery Mobile stylem rozložení souboru (*Views\Shared\\_Layout.Mobile.cshtml*).
- Částečné zobrazení přepínači zobrazení *(MvcMobile\Views\Shared\\_ViewSwitcher.cshtml*), který poskytuje odkaz v horní části každé stránce přepnutí z zobrazení plochy na mobilní zobrazení a naopak.
- Několik<em>.png</em> a <em>.gif</em> soubory v obrázku <em>Content\images</em> složky.

Otevřete *Global.asax* souboru a přidejte následující kód jako poslední řádek `Application_Start` metoda.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample10.cs)]

Následující kód ukazuje kompletní *Global.asax* souboru.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample11.cs?highlight=26)]

> [!NOTE]
> Pokud používáte Internet Explorer 9 a nevidíte `BundleMobileConfig` řádek výše v žlutý zvýraznění, klikněte na tlačítko [kompatibilního zobrazení tlačítka](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![obrázek kompatibilního zobrazení tlačítka (vypnuto)] (http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg " Obrázek tlačítka kompatibilního zobrazení (vypnuto)") v aplikaci Internet Explorer, chcete-li změnit z přehledu ikonu ![obrázek kompatibilního zobrazení tlačítka (vypnuto)](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "obrázek kompatibilního zobrazení tlačítka (vypnuto) ") na plnou barvou ![obrázek kompatibilního zobrazení tlačítka (na)](http://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "obrázek kompatibilního zobrazení tlačítka (na)"). Případně můžete zobrazit v tomto kurzu v FireFox nebo Chrome.


Otevřete *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* souboru a přidejte následující kód přímo po `Html.Partial` volání:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample12.cshtml)]

Kompletní *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* souboru jsou uvedeny níže:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample13.cshtml)]

Sestavení aplikace a v váš prohlížeč pro mobilní zařízení emulátoru přejděte do *AllTags* zobrazení. Zobrazí následující:

[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)

> [!NOTE]
> Můžete ladit mobilní konkrétní kód podle [nastavení řetězec uživatelského agenta](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) pro aplikaci Internet Explorer nebo Chrome iPhone a potom pomocí nástrojů pro vývojáře F-12. Pokud se váš prohlížeč pro mobilní zařízení nezobrazí **Domů**, **mluvčího**, **značka**, a **datum** odkazy jako tlačítka, odkazy na architekturu jQuery Mobile skripty a souborů CSS jsou pravděpodobně není správná.


Kromě změn styl, uvidíte **zobrazení mobilním zobrazením** a odkaz, který umožňuje přepnout na zobrazení plochy z mobilního zobrazení. Vyberte **zobrazení plochy** se zobrazí odkaz a zobrazení plochy.

[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)

Zobrazení plochy neposkytuje způsob, jak přímo přejděte zpět na mobilní zobrazení. Nyní, budete opravte. Otevřete *Views\Shared\\_Layout.cshtml* souboru. Právě v části stránky `body` elementu, přidejte následující kód, který vykreslí widgetu přepínači zobrazení:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample14.cshtml)]

Obnovit *AllTags* zobrazení v prohlížeč pro mobilní zařízení. Teď můžete Navigovat mezi zobrazeními desktop a mobile.

[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)

> [!NOTE]
> Ladění Poznámka: můžete přidat následující kód do konce Views\Shared\\_ViewSwitcher.cshtml pomoci při ladění zobrazení pomocí prohlížeče řetězec uživatelského agenta nastavena na mobilní zařízení.
> 
> [!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample15.cs)]
> 
>  a přidání záhlaví následující *Views\Shared\\_Layout.cshtml* souboru.  
> 
> [!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample16.html)]


Vyhledejte *AllTags* stránku v prohlížeči počítače. Pomůcka přepínači zobrazení není zobrazen ve prohlížeč pro stolní počítač, protože se přidá jenom do mobilních rozložení stránky. Později v tomto kurzu se zobrazí, jak můžete přidat pomůcky přepínači zobrazení k zobrazení plochy.

[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)

## <a name="improving-the-speakers-list"></a>Vylepšení seznamu mluvčí

Mobilní prohlížeče, vyberte **Řečníci** odkaz. Protože není žádná mobilní zobrazení (*AllSpeakers.Mobile.cshtml*), zobrazit výchozí mluvčí (*AllSpeakers.cshtml*) je vykreslen pomocí mobilních rozložení zobrazení ( *\_ Layout.Mobile.cshtml*).

[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)

Výchozí zobrazení (jiných než mobilních) z vykreslování uvnitř mobilní rozložení můžete zakázat globálně nastavením `RequireConsistentDisplayMode` k `true` v *zobrazení\\soubor _ViewStart.cshtml* souboru, například takto:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample17.cshtml)]

Když `RequireConsistentDisplayMode` je nastaven na `true`, mobilní rozložení (<em>\_Layout.Mobile.cshtml</em>) se používá pouze pro mobilní zobrazení. (To znamená, je soubor zobrazení formuláře <em>** ViewName</em><em>. Mobile.cshtml</em>.) Můžete chtít nastavit `RequireConsistentDisplayMode` k `true` Pokud vaše mobilní rozložení nebude fungovat dobře u jiných než mobilních zobrazení. Na snímku obrazovky níže znázorňuje jak <em>Řečníci</em> stránka vykresluje při `RequireConsistentDisplayMode` je nastaven na `true`.

[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)

Konzistentní režim zobrazení v zobrazení můžete zakázat nastavením `RequireConsistentDisplayMode` k `false` v souboru zobrazení. Následující kód v *Views\Home\AllSpeakers.cshtml* souboru nastaví `RequireConsistentDisplayMode` k `false`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample18.cshtml)]

## <a name="creating-a-mobile-speakers-view"></a>Vytvoření mobilní mluvčí zobrazení

Protože jste právě viděli, *mluvčí* zobrazení je čitelná, ale odkazy jsou malé a obtížně se klepnout na mobilní zařízení. V této části vytvoříte konkrétního mobile *Řečníci* zobrazení jako moderní mobilní aplikace – zobrazí velký, snadno klepněte odkazuje a obsahuje vyhledávací pole a rychle tak najít mluvčí.

Kopírování *AllSpeakers.cshtml* k *AllSpeakers.Mobile.cshtml*. Otevřete *AllSpeakers.Mobile.cshtml* souborů a odebrat `<h2>` element záhlaví.

V `<ul>` značky, přidejte `data-role` atribut a jeho hodnotu nastavte `listview`. Stejně jako jiné [ `data-*` atributy](http://html5doctor.com/html5-custom-data-attributes/), `data-role="listview"` umožňuje snadnější rozsáhlého seznamu položek klepnout na. Toto je dokončené značek, které bude vypadat takto:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample19.cshtml)]

Aktualizujte prohlížeč pro mobilní zařízení. Aktualizovaná zobrazení vypadá takto:

[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)

I když se zlepšila mobilní zobrazení, je obtížné přejděte dlouhý seznam mluvčí. Chcete-li tento problém odstranit, `<ul>` značky, přidejte `data-filter` atribut a nastavte ji na `true`. Kód uvedený níže ukazuje `ul` značek.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample20.html)]

Následující obrázek ukazuje do vyhledávacího pole filtru v horní části stránky, která je výsledkem `data-filter` atribut.

[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)

Každý písmeno, při psaní do vyhledávacího pole, jQuery Mobile filtry pro zobrazený seznam, jak je znázorněno na obrázku níže.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)

## <a name="improving-the-tags-list"></a>Vylepšení seznam klíčových slov

Jako výchozí *Řečníci* zobrazení, *značky* zobrazení je čitelná, ale jsou odkazy na malé a obtížně se klepnout na mobilní zařízení. V této části budete opravit *značky* zobrazit vyřešeny, stejně jako *Řečníci* zobrazení.

Odeberte &quot;skrýt&quot; přípona k *Views\Home\AllTags.Mobile.cshtml.hide* souboru tak, aby název *Views\Home\AllTags.Mobile.cshtml*. Otevřete soubor přejmenovat a odstraňte `<h2>` elementu.

Přidat `data-role` a `data-filter` atributů k `<ul>` značka, jak je vidět tady:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample21.html)]

Následující obrázek zobrazuje stránku značky filtrování písmeno `J`.

[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)

## <a name="improving-the-dates-list"></a>Vylepšení seznamu kalendářních dat

Můžete zvýšit *data* zobrazit stejně, jako je vylepšená *Řečníci* a *značky* zobrazení, takže je jednodušší použít na mobilním zařízení.

Kopírování *Views\Home\AllDates.cshtml* do souboru *Views\Home\AllDates.Mobile.cshtml*. Otevřete nový soubor a odebrat `<h2>` elementu.

Přidat `data-role="listview"` k `<ul>` značky, například takto:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample22.html)]

Následující obrázek znázorňuje, co **datum** stránky vypadá jako s `data-role` atribut na místě.

[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png) nahraďte obsah *Views\Home\AllDates.Mobile.cshtml* soubor s následujícím kódem:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample23.cshtml)]

Tento kód skupiny všechny relace dnů. Vytvoří seznam dělicí pro každý nový den a vypíše všechny relace pro každý den v části na příčku. Zde je, jak vypadá spuštění tento kód:

[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)

## <a name="improving-the-sessionstable-view"></a>Vylepšení SessionsTable zobrazení

V této části vytvoříte zobrazení mobile konkrétní relací. Změny, které jsme provedli bude rozsáhlejší než v ostatních zobrazeních, které jsme vytvořili.

Mobilní prohlížeče, klepněte **mluvčího** tlačítko a potom zadejte `Sc` do vyhledávacího pole.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)

Klepněte **Scott Hanselman** odkaz.

[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)

Jak vidíte, zobrazení je obtížné číst na prohlížeč pro mobilní zařízení. Sloupce s datem je obtížné číst a značky sloupce je mimo zobrazení. Chcete-li odstranit tento problém, zkopírujte *Views\Home\SessionsTable.cshtml* k *Views\Home\SessionsTable.Mobile.cshtml*a pak obsah souboru nahraďte následujícím kódem:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample24.cshtml)]

Kód odebere místnosti značky sloupců a formátech název, mluvčího a datum ve svislém směru, takže tyto informace je čitelná na prohlížeč pro mobilní zařízení. Následující obrázek zobrazuje změny kódu.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)

## <a name="improving-the-sessionbycode-view"></a>Vylepšení SessionByCode zobrazení

Nakonec vytvoříte zobrazení konkrétní mobilní *SessionByCode* zobrazení. Mobilní prohlížeče, klepněte **mluvčího** tlačítko a potom zadejte `Sc` do vyhledávacího pole.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)

Klepněte **Scott Hanselman** odkaz. Scott Hanselman relace se zobrazí.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)

Vyberte **Přehled webové zásobníku láska MS** odkaz.

[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)

Zobrazení plochy výchozí je v pořádku, ale je možné zvýšit.

Kopírování *Views\Home\SessionByCode.cshtml* k *Views\Home\SessionByCode.Mobile.cshtml* a nahraďte obsah *Views\Home\SessionByCode.Mobile.cshtml*soubor s následující kód:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample25.cshtml)]

Nový kód používá `data-role` atribut ke zlepšení rozložení zobrazení.

Aktualizujte prohlížeč pro mobilní zařízení. Následující obrázek zobrazuje změny kódu, které jste právě vytvořili:

[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)

## <a name="wrapup-and-review"></a>Wrapup a zkontrolujte

Tento kurz obsahuje zavedla nových mobilních funkcí technologie ASP.NET MVC 4 Developer Preview. Mobilní funkce:

- Možnost přepsání částečné zobrazení, zobrazení a rozložení globálně i pro jednotlivé zobrazení.
- Kontrolu nad rozložení a částečné přepsání vynucení pomocí `RequireConsistentDisplayMode` vlastnost.
- Widget přepínači zobrazení pro mobilní zobrazení než lze také zobrazit v zobrazení plochy.
- Podpora pro podporu určité prohlížeče, jako je třeba iPhone prohlížeč.

## <a name="see-also"></a>Viz také

- [jQuery Mobile](http://jquerymobile.com) lokality.
- [jQuery Mobile – přehled](http://jquerymobile.com/demos/1.0b3/docs/about/intro.html)
- [Osvědčené postupy W3C doporučení mobilní webové aplikace](http://www.w3.org/TR/mwabp/)
- [W3C Candidate doporučení pro dotazy na média](http://www.w3.org/TR/css3-mediaqueries/)
