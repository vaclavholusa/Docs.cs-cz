---
uid: mvc/overview/older-versions/aspnet-mvc-4-mobile-features
title: Funkce mobilní architektury ASP.NET MVC 4 | Dokumentace Microsoftu
author: Rick-Anderson
description: Teď existuje verze MVC 5 tohoto kurzu s ukázkami kódu při nasazení webové aplikace ASP.NET MVC 5 Mobile na webech Azure.
ms.author: riande
ms.date: 08/15/2012
ms.assetid: 27dc4fc8-1b51-43b0-933f-fc1b52476523
msc.legacyurl: /mvc/overview/older-versions/aspnet-mvc-4-mobile-features
msc.type: authoredcontent
ms.openlocfilehash: 8b82b8b9b1ee6646072931da889c643afb34d474
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578156"
---
<a name="aspnet-mvc-4-mobile-features"></a>Funkce mobilní architektury ASP.NET MVC 4
====================
Podle [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Je teď verze MVC 5 s ukázkami kódu v tomto kurzu [nasazení webové aplikace ASP.NET MVC 5 Mobile na webech Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/).


V tomto kurzu se seznámíte se základy práce s mobilními funkcemi webové aplikace ASP.NET MVC 4. Pro účely tohoto kurzu můžete použít [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) nebo Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer nebo VWD&quot;). Pokud už máte, můžete profesionální verzi sady Visual Studio.

Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže.

- [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) (doporučeno) nebo Visual Studio Web Developer Express SP1. Visual Studio 2012 obsahuje ASP.NET MVC 4. Pokud používáte aplikaci Visual Web Developer 2010, je nutné nainstalovat [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392).

Budete také potřebovat emulátoru mobilního prohlížeče. Bude fungovat některé z následujících akcí:

- [Emulátor Windows 7 Phone](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). (Toto je emulátor, který se používá ve většině snímky obrazovky v tomto kurzu).
- Změňte identifikační řetězec pro emulaci Iphonu. Zobrazit [to](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) blogu.
- [Emulátor Opera Mobile](http://www.opera.com/developer/tools/mobile/)
- [Apple Safari](http://www.apple.com/safari/download/) s uživatelského agenta, nastavte na Iphonu. Pokyny o tom, jak nastavit uživatelský agent v Safari na "iPhone" najdete v tématu [jak chcete, aby Safari, které předstírají, že je aplikace Internet Explorer](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) na blog Davida Alison.

Projekty aplikace Visual Studio se zdrojovým kódem jazyka C# jsou k dispozici v tomto tématu:

- [Stažení projektu Starter](https://go.microsoft.com/fwlink/?linkid=228307&amp;clcid=0x409)
- [Dokončení stahování projektu](https://go.microsoft.com/fwlink/?linkid=228306&amp;clcid=0x409)

### <a name="what-youll-build"></a>Co budete vytvářet

Pro účely tohoto kurzu přidáte mobilními funkcemi jednoduchou aplikaci výpis konference, která je součástí [počáteční projekt](https://go.microsoft.com/fwlink/?LinkId=228307). Následující snímek obrazovky ukazuje stránku značek dokončené aplikace, jak je vidět [emulátor telefonu Windows 7](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). Zobrazit [klávesnice mapování pro Windows Phone Emulator](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx) ke zjednodušení vstup z klávesnice.

[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)

Můžete použít Internet Explorer verze 9 nebo 10, FireFox nebo Chrome pro vývoj mobilních aplikací tak, že nastavíte [identifikační řetězec prohlížeče](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/). Následující obrázek ukazuje dokončený kurz pomocí Internet Exploreru emulace Iphonu. Můžete použít nástroje pro vývojáře F-12 aplikace Internet Explorer a [nástroj Fiddler](http://www.fiddler2.com/fiddler2/) pro ladění vaší aplikace.

![](aspnet-mvc-4-mobile-features/_static/image3.png)

### <a name="skills-youll-learn"></a>Dovednosti, které se dozvíte

Zde je, co se dozvíte:

- Jak používat HTML5 šablony ASP.NET MVC 4 `viewport` atribut a adaptivní vykreslování ke zlepšení zobrazení na mobilních zařízeních.
- Postup vytvoření zobrazení specifické pro mobilní zařízení.
- Postup vytvoření zobrazení přepínání této umožňuje uživatelům přepínat mezi mobilní zobrazení a zobrazení plochy aplikace.

### <a name="getting-started"></a>Začínáme

Stáhnout aplikaci pro výpis konference pro počáteční projekt pomocí následujícího odkazu: [Stáhnout](https://go.microsoft.com/fwlink/?LinkId=228307). V Průzkumníku Windows, klikněte pravým tlačítkem myši *MvcMobile.zip* soubor a zvolte **vlastnosti**. V **MvcMobile.zip vlastnosti** dialogového okna zvolte **Odblokovat** tlačítko. (Odblokování brání upozornění zabezpečení, ke které dojde při pokusu o použití *ZIP* soubor, který jste stáhli z webu.)

![p1_unBlock](aspnet-mvc-4-mobile-features/_static/image4.png)

Klikněte pravým tlačítkem myši *MvcMobile.zip* a vyberte možnost **Extrahovat vše** dekomprimovat soubor. V sadě Visual Studio, otevřete *MvcMobile.sln* souboru.

Stiskněte kombinaci kláves CTRL + F5 ke spuštění aplikace, které se zobrazí v prohlížeči klientů. Spustit emulátor vašeho mobilního prohlížeče, zkopírujte adresu URL pro aplikaci konference do emulátoru a potom klikněte na tlačítko **Procházet podle klíčových slov** odkaz. Pokud používáte emulátor Windows Phone, klikněte na tlačítko na panelu Adresa URL a stiskněte klávesu pozastavení získat přístup z klávesnice. Obrázek níže ukazuje *AllTags* zobrazení (vybrala **Procházet podle klíčových slov**).

[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)

Zobrazení je velmi čitelné na mobilním zařízení. Vyberte odkaz ASP.NET.

[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)

Zobrazení značek technologie ASP.NET je velmi nevypadala. Například **datum** sloupec je velmi obtížné číst. V pozdější části kurzu vytvoříte verzi *AllTags* zobrazení, který je speciálně pro mobilní prohlížeče a, který bude čitelnost zobrazení.

Poznámka: Aktuálně chybu existuje mobilní modul pro ukládání do mezipaměti. Pro produkční aplikace, je nutné nainstalovat [pevné DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) balíčku nuget. Zobrazit [ASP.NET MVC 4 Mobile ukládání do mezipaměti chyby opravené](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) podrobnosti o opravu.

## <a name="css-media-queries"></a>Dotazy na média šablon stylů CSS

[Dotazy na média šablon stylů CSS](http://www.w3.org/TR/css3-mediaqueries/) jsou rozšíření šablon stylů CSS pro typy médií. Umožňují vytvořit pravidla, které přepíší výchozí pravidla šablon stylů CSS pro konkrétní prohlížeče (Uživatelští agenti). Běžné pravidlo pro šablony stylů CSS, který cílí na mobilních prohlížečích je definování maximální velikosti obrazovky. *Content\Site.css* soubor, který se vytvoří při vytvoření nového projektu ASP.NET MVC 4 Internet obsahuje následující dotaz na média:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample1.css)]

Pokud okno prohlížeče je 850 pixelů široký nebo méně, použije pravidla šablon stylů CSS v tomto bloku média. Dotazy na média šablon stylů CSS tímto způsobem můžete použít k poskytnutí lepší zobrazení obsahu HTML na malé prohlížeče (například mobilní prohlížeče) než výchozí pravidla šablon stylů CSS, které jsou určeny pro širší zobrazí stolních počítačů.

## <a name="the-viewport-meta-tag"></a>Značka Meta zobrazení

Většina mobilních prohlížečů definovat šířku okna virtuálního prohlížeče ( *zobrazení*), který je mnohem větší než skutečná šířku mobilního zařízení. To umožňuje mobilní prohlížeče podle celé webové stránky uvnitř virtuální zobrazení. Uživatelé pak přiblížením zajímavého obsahu. Ale pokud nastavíte Šířka zobrazení na šířku skutečné zařízení, bez měřítka je nutná, protože obsah vejde v mobilním prohlížeči.

Zobrazení `<meta>` značky v souboru rozložení ASP.NET MVC 4 Nastaví zobrazení na šířku zařízení. Následující řádek obsahuje zobrazení `<meta>` značky v souboru rozložení ASP.NET MVC 4.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample2.html)]

## <a name="examining-the-effect-of-css-media-queries-and-the-viewport-meta-tag"></a>Zkoumání šablon stylů CSS Media dotazů a zobrazení metaznačku efekt

Otevřít *Views\Shared\\_Layout.cshtml* souboru v editoru a Odkomentujte zobrazení `<meta>` značky. Následující kód ukazuje komentovaný řádek.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample3.cshtml)]

Otevřít *MvcMobile\Content\Site.css* souboru v editoru a změňte maximální šířka v dotazu médií na nula pixelů. To zabrání pravidel šablon stylů CSS z používání mobilních prohlížečů. Následující řádek obsahuje upravený multimediální dotaz:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample4.css)]

Uložte změny a přejděte do místa konání konference aplikace spustila v emulátoru mobilního prohlížeče. Velmi malé text na následujícím obrázku je výsledek odebrání zobrazení `<meta>` značky. Se žádné zobrazení `<meta>` značky, prohlížeč je oddálení výchozí zobrazení šířku (850 pixelů nebo větší u většiny mobilních prohlížečů.)

[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)

Zrušit změny – zrušte komentář u zobrazení `<meta>` označení na soubor rozložení a obnovte multimediální dotaz 850 pixelů *Site.css* souboru. Uložte změny a aktualizovat mobilní prohlížeče k ověření, že se obnovila mobilní zobrazení.

Zobrazení `<meta>` značky a šablony stylů CSS media dotaz nejsou specifická pro technologii ASP.NET MVC 4 a můžete využít tyto funkce v jakékoli webové aplikaci. Ale jsou teď součástí soubory, které jsou generovány, pokud vytvoříte nový projekt ASP.NET MVC 4.

Další informace o zobrazení `<meta>` značku, přečtěte si téma [činnosti z dvě okna – část 2](http://www.quirksmode.org/mobile/viewports2.html).

V další části uvidíte, jak poskytnout konkrétní zobrazení mobilní prohlížeče.

## <a name="overriding-views-layouts-and-partial-views"></a>Přepsání, rozložení a částečné zobrazení

Důležité nové funkce v architektuře ASP.NET MVC 4 je jednoduchý mechanismus, který umožňuje přepsat všechna zobrazení (včetně rozložení a částečných zobrazení) pro mobilní prohlížeče, obecně pro jednotlivé mobilního prohlížeče nebo pro všechny určitého webového prohlížeče. Abyste si mohli zobrazit konkrétní mobilní zařízení, můžete zkopírovat soubor zobrazení a přidání *. Mobilní* k názvu souboru. Chcete-li například vytvořit mobilní *Index* zobrazit, zkopírovat *Views\Home\Index.cshtml* k *Views\Home\Index.Mobile.cshtml*.

V této části vytvoříte soubor rozložení specifické pro mobilní zařízení.

Pokud chcete začít, zkopírujte *Views\Shared\\_Layout.cshtml* k *Views\Shared\\_Layout.Mobile.cshtml*. Otevřít  *\_Layout.Mobile.cshtml* a změnit název z **MVC4 konference** k **Conference (mobilní)**.

V každém `Html.ActionLink` volání, odeberte "Procházet podle" v každé propojení *ActionLink*. Následující kód ukazuje dokončené těla souboru mobilní rozložení.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample5.cshtml)]

Kopírovat *Views\Home\AllTags.cshtml* do souboru *Views\Home\AllTags.Mobile.cshtml*. Otevřete nový soubor a změňte `<h2>` element z "Tags" k "značky (M)":

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample6.html)]

Přejděte na stránku značky pomocí prohlížeč pro stolní počítač a pomocí emulátoru mobilního prohlížeče. Emulátoru mobilního prohlížeče ukazuje dvě změny, které jste provedli.

[![p2m_layoutTags.mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)

Naproti tomu nedošlo ke změně zobrazení plochy.

[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)

## <a name="browser-specific-views"></a>Zobrazení specifické pro prohlížeč

Kromě zobrazení specifické pro mobilní zařízení a desktop můžete vytvořit zobrazení pro jednotlivé prohlížeče. Můžete například vytvořit zobrazení, které jsou určené konkrétně pro iPhone prohlížeče. V této části vytvoříte rozložení pro iPhone prohlížeče a uváděna verze *AllTags* zobrazení.

Otevřít *Global.asax* a přidejte následující kód, který `Application_Start` metoda.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample7.cs)]

Tento kód definuje nový režim zobrazení s názvem "iPhone", který bude porovnání jednotlivých příchozích požadavků. Pokud příchozí žádost odpovídá podmínku, kterou jste definovali (tj. Pokud uživatelský agent obsahuje řetězec "iPhone"), bude technologie ASP.NET MVC hledat zobrazení, jejichž název obsahuje příponu "iPhone".

V kódu, klikněte pravým tlačítkem na `DefaultDisplayMode`, zvolte **vyřešit**a klikněte na tlačítko `using System.Web.WebPages;`. To přidá odkaz na `System.Web.WebPages` obor názvů, který je tam, kde `DisplayModes` a `DefaultDisplayMode` typy jsou definovány.

[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)

Můžete také pouze ručně přidejte následující řádek, který `using` část souboru.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample8.cs)]

Úplný obsah *Global.asax* souboru je uveden níže.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample9.cs)]

Uložte změny. Kopírovat *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* do souboru *MvcMobile\Views\Shared\\_Layout.iPhone.cshtml*. Otevřete nový soubor a potom změňte `h1` nadpis z `Conference (Mobile)` k `Conference (iPhone)`.

Kopírovat *MvcMobile\Views\Home\AllTags.Mobile.cshtml* do souboru *MvcMobile\Views\Home\AllTags.iPhone.cshtml*. V novém souboru změnit `<h2>` element z "značky (M)" k "Tags (iPhone)".

Spusťte aplikaci. Spustit emulátor mobilní prohlížeče, ujistěte se, že jeho uživatelský agent je nastavena na "iPhone" a přejděte *AllTags* zobrazení. Následující snímek obrazovky ukazuje *AllTags* zobrazení vykresleno v [Safari](http://www.apple.com/safari/download/) prohlížeče. Safari pro Windows si můžete stáhnout [tady](https://support.apple.com/kb/DL1531).

[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)

V této části jsme jste viděli, jak vytvořit mobilní rozložení a zobrazení a postup vytvoření zobrazení pro konkrétní zařízení, jako je například iPhone a rozložení. V další části uvidíte, jak využít jQuery Mobile pro další zajímavé mobilní zobrazení.

## <a name="using-jquery-mobile"></a>Pomocí jQuery Mobile

[JQuery Mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html) knihovny poskytuje architekturu uživatelské rozhraní, která funguje na všech hlavních mobilních prohlížečů. jQuery Mobile platí *postupném rozšiřování* na mobilních prohlížečích, které podporují šablon stylů CSS a JavaScriptu. Postupném rozšiřování umožňuje všechny prohlížeče zobrazíte základní obsah na webové stránce, přičemž výkonnější prohlížečích a zařízeních mít bohatší zobrazení. Soubory jazyka JavaScript a CSS, které jsou součástí jQuery Mobile stylu mnoho prvků podle mobilní prohlížeče bez jakýchkoli změn kódu.

V této části nainstalujete *jQuery.Mobile.MVC* balíček NuGet, který nainstaluje jQuery Mobile a widget přepínači zobrazení.

Pokud chcete začít, odstraňte *sdílené\\_Layout.Mobile.cshtml* a *sdílené\\_Layout.iPhone.cshtml* soubory, které jste vytvořili dříve.

Přejmenovat *Views\Home\AllTags.Mobile.cshtml* a *Views\Home\AllTags.iPhone.cshtml* soubory *Views\Home\AllTags.iPhone.cshtml.hide* a  *Views\Home\AllTags.Mobile.cshtml.hide*. Vzhledem k tomu, že už nebude mít soubory *.cshtml* rozšíření, nebudou použity modulem runtime ASP.NET MVC pro vykreslení *AllTags* zobrazení.

Nainstalujte *jQuery.Mobile.MVC* balíček NuGet díky tomu:

1. Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**a pak vyberte **Konzola správce balíčků**.

    [![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)
2. V **Konzola správce balíčků**, zadejte `Install-Package jQuery.Mobile.MVC -version 1.0.0`

Následující obrázek ukazuje soubory přidat a změnit do projektu MvcMobile jQuery.Mobile.MVC balíček NuGet. Soubory, které se přidají [přidat] připojí po název souboru. Obrázek GIF nezobrazuje a soubory PNG přidány do *Content\images* složky.

![](aspnet-mvc-4-mobile-features/_static/image21.png)

Balíček NuGet jQuery.Mobile.MVC nainstaluje následující:

- *Aplikace\_Start\BundleMobileConfig.cs* soubor, který je nutný k odkazování jQuery jazyka JavaScript a CSS soubory přidané. Musí podle pokynů níže a odkazovat na mobilní sadu definovanou v tomto souboru.
- jQuery Mobile CSS soubory.
- A `ViewSwitcher` widget kontroler (*Controllers\ViewSwitcherController.cs*).
- jQuery Mobile Javascriptové soubory.
- JQuery Mobile styl rozložení souboru (*Views\Shared\\_Layout.Mobile.cshtml*).
- Přepínači zobrazení částečného zobrazení *(MvcMobile\Views\Shared\\_ViewSwitcher.cshtml*), který poskytuje odkaz v horní části každé stránky přepněte ze zobrazení plochy na mobilní zobrazení a naopak.
- Několik<em>.png</em> a <em>.gif</em> soubory v obrázků <em>Content\images</em> složky.

Otevřít *Global.asax* a přidejte následující kód jako poslední řádek `Application_Start` metody.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample10.cs)]

Následující kód ukazuje kompletní *Global.asax* souboru.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample11.cs?highlight=26)]

> [!NOTE]
> Pokud používáte Internet Explorer 9 a nevidíte `BundleMobileConfig` řádek výše v zvýraznit žlutý, klikněte na tlačítko [kompatibilního zobrazení tlačítka](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![obrázek kompatibilní zobrazení tlačítka (vypnuto)](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg " Obrázek tlačítka kompatibilního zobrazení (vypnuto)") v Internet Exploreru, chcete-li změnit z přehledu ikonu ![obrázek kompatibilní zobrazení tlačítka (vypnuto)](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "obrázek kompatibilní zobrazení tlačítka (vypnuté) ") plnou barvu ![obrázku (na) tlačítko pro kompatibilní zobrazení](http://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "obrázku (na) tlačítko pro kompatibilní zobrazení"). Případně můžete zobrazit v tomto kurzu v aplikaci FireFox nebo Chrome.


Otevřít *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* a přidejte následující kód přímo po `Html.Partial` volání:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample12.cshtml)]

Kompletní *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* souboru je uveden níže:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample13.cshtml)]

Sestavení aplikace a v emulátoru vašeho mobilního prohlížeče přejděte *AllTags* zobrazení. Zobrazí se následující:

[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)

> [!NOTE]
> Můžete ladit mobilní konkrétním kódu pomocí [nastavení identifikační řetězec prohlížeče](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) pro aplikace Internet Explorer nebo Chrome Iphonů a pak pomocí nástroje pro vývojáře F-12. Pokud se nezobrazí v mobilním prohlížeči **Domů**, **mluvčího**, **značky**, a **datum** odkazy jako tlačítka, odkazy na jQuery Mobile skriptů a souborů CSS jsou pravděpodobně není správné.


Kromě změn stylů se zobrazí **mobilní zobrazení** a odkaz, který umožňuje přepnout do zobrazení plochy z mobilní zobrazení. Zvolte **zobrazení plochy** propojení a zobrazení plochy se zobrazí.

[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)

Zobrazení plochy neposkytuje způsob, jak přímo přejít zpět na mobilní zobrazení. Opravíte to nyní. Otevřít *Views\Shared\\_Layout.cshtml* souboru. Přímo pod stránku `body` prvku, přidejte následující kód, který vykreslí widgetu přepínači zobrazení:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample14.cshtml)]

Aktualizovat *AllTags* zobrazení v mobilním prohlížeči. Můžete nyní procházet mezi stolní počítače a mobilní zobrazení.

[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)

> [!NOTE]
> Ladění Poznámka: následující kód můžete přidat na konec objektu Views\Shared\\_ViewSwitcher.cshtml pro ladění zobrazení pomocí prohlížeče identifikační řetězec prohlížeče nastavena na mobilním zařízení.
> 
> [!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample15.cs)]
> 
>  a přidejte následující záhlaví *Views\Shared\\_Layout.cshtml* souboru.  
> 
> [!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample16.html)]


Přejděte *AllTags* stránky v desktopovém prohlížeči. Ve widgetu přepínači zobrazení není v desktopovém prohlížeči zobrazen, protože se přidá jenom do mobilní rozložení stránky. Později v tomto kurzu uvidíte, jak můžete přidat widget přepínači zobrazení do zobrazení plochy.

[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)

## <a name="improving-the-speakers-list"></a>Vylepšení seznamu přednášejících

V mobilním prohlížeči, vyberte **přednášející** odkaz. Protože neexistuje žádná mobilní zobrazení (*AllSpeakers.Mobile.cshtml*), zobrazit přednášející výchozí (*AllSpeakers.cshtml*) je vykreslen pomocí mobilní rozložení zobrazení ( *\_ Layout.Mobile.cshtml*).

[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)

Výchozí zobrazení (nemobilních zařízení) z vykreslování uvnitř mobilní rozložení můžete globálně zakázat nastavením `RequireConsistentDisplayMode` k `true` v *zobrazení\\soubor _ViewStart.cshtml* souboru následujícím způsobem:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample17.cshtml)]

Když `RequireConsistentDisplayMode` je nastavena na `true`, mobilní rozložení (<em>\_Layout.Mobile.cshtml</em>) se používá pouze pro mobilní zobrazení. (To znamená, je soubor zobrazení formuláře <em>** ViewName</em><em>. Mobile.cshtml</em>.) Můžete chtít nastavit `RequireConsistentDisplayMode` k `true` Pokud mobilní rozložení nebude fungovat s zobrazení nemobilních zařízení. Na snímku obrazovky níže ukazuje jak <em>přednášející</em> při vykreslení stránky `RequireConsistentDisplayMode` je nastavena na `true`.

[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)

Konzistentní režim zobrazení v zobrazení lze zakázat nastavením `RequireConsistentDisplayMode` k `false` v zobrazení souboru. Následující kód v *Views\Home\AllSpeakers.cshtml* souboru sady `RequireConsistentDisplayMode` k `false`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample18.cshtml)]

## <a name="creating-a-mobile-speakers-view"></a>Vytvoření zobrazení mobilní přednášející

Protože jste před chvílí, *přednášející* zobrazení je čitelný, ale odkazy jsou malé a obtížně se klepnutím na mobilním zařízení. V této části vytvoříte mobile konkrétní *přednášející* zobrazení, která vypadá moderní mobilní aplikace – zobrazuje velké, -tap odkazy a obsahuje vyhledávací pole a rychle najít mluvčí.

Kopírování *AllSpeakers.cshtml* k *AllSpeakers.Mobile.cshtml*. Otevřít *AllSpeakers.Mobile.cshtml* souboru a odebrat `<h2>` záhlaví elementu.

V `<ul>` značky, přidejte `data-role` atribut a nastavte jej na hodnotu `listview`. Stejně jako jiné [ `data-*` atributy](http://html5doctor.com/html5-custom-data-attributes/), `data-role="listview"` umožňuje snadněji položky seznamu velkých klepnout na. To vypadá dokončené značky:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample19.cshtml)]

Aktualizace mobilních prohlížečů. Zobrazení aktualizované vypadá takto:

[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)

I když se mobilní zobrazení se zlepšila, je obtížné přejděte dlouhý seznam přednášejících. Chcete-li tento problém odstranit, `<ul>` značky, přidejte `data-filter` atribut a nastavte ho na `true`. Níže uvedený kód ukazuje `ul` značek.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample20.html)]

Následující obrázek ukazuje filtrování pole Hledat v horní části stránky, která je výsledkem `data-filter` atribut.

[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)

Každé písmeno, při psaní do vyhledávacího pole, jQuery Mobile zobrazený seznam filtrů, jak je znázorněno na následujícím obrázku.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)

## <a name="improving-the-tags-list"></a>Zlepšení seznam značek

Výchozí nastavení, jako jsou *přednášející* zobrazení *značky* zobrazení je čitelný, ale tyto odkazy jsou malé a obtížně klepněte na mobilním zařízení. V této části opravíte *značky* zobrazit stejným způsobem, který jste opravili *přednášející* zobrazení.

Odeberte &quot;skrýt&quot; přípona, která má *Views\Home\AllTags.Mobile.cshtml.hide* souboru je název *Views\Home\AllTags.Mobile.cshtml*. Otevřete soubor přejmenován a odeberte `<h2>` elementu.

Přidat `data-role` a `data-filter` atributů `<ul>` označit, jak je znázorněno zde:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample21.html)]

Následující obrázek ukazuje stránku značek filtrování písmeno `J`.

[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)

## <a name="improving-the-dates-list"></a>Vylepšení seznamu kalendářních dat

Můžete zvýšit *data* zobrazit jako tak lepší *přednášející* a *značky* zobrazení tak, aby se snadněji používá na mobilním zařízení.

Kopírovat *Views\Home\AllDates.cshtml* do souboru *Views\Home\AllDates.Mobile.cshtml*. Otevřete nový soubor a odeberte `<h2>` elementu.

Přidat `data-role="listview"` k `<ul>` značku, například takto:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample22.html)]

Následující obrázek ukazuje, co **datum** stránka vypadá podobně jako s `data-role` atribut na místě.

[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png) nahraďte obsah *Views\Home\AllDates.Mobile.cshtml* souboru následujícím kódem:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample23.cshtml)]

Tento kód seskupí všechny relace podle dnů. Vytvoří seznam oddělovač pro každého nového dne a vypíše všechny relace pro každý den v rámci rozdělovač. Zde je, jak to funguje při spuštění tohoto kódu:

[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)

## <a name="improving-the-sessionstable-view"></a>Zlepšení SessionsTable zobrazení

V této části vytvoříte specifické pro mobilní zobrazení relací. Změny, které jsme provedli bude rozsáhlejší než v ostatních zobrazeních, který jsme vytvořili.

V mobilním prohlížeči klepněte **mluvčího** tlačítko a pak zadejte `Sc` do vyhledávacího pole.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)

Klepněte **Scott Hanselman** odkaz.

[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)

Jak je vidět zobrazení je obtížné číst v mobilním prohlížeči. Sloupec datum je obtížné číst a značky sloupce je mimo zobrazení. Chcete-li opravit, zkopírujte *Views\Home\SessionsTable.cshtml* k *Views\Home\SessionsTable.Mobile.cshtml*a poté nahraďte obsah souboru následujícím kódem:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample24.cshtml)]

Kód odebere místnosti značky sloupce a naformátuje název, mluvčího a datum svisle, tak, aby všechny tyto informace jsou číst v mobilním prohlížeči. Na následujícím obrázku odráží změny kódu.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)

## <a name="improving-the-sessionbycode-view"></a>Zlepšení SessionByCode zobrazení

Nakonec vytvoříte zobrazení specifické pro mobilní zařízení *SessionByCode* zobrazení. V mobilním prohlížeči klepněte **mluvčího** tlačítko a pak zadejte `Sc` do vyhledávacího pole.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)

Klepněte **Scott Hanselman** odkaz. Zobrazí se Scottem Hanselmanem relace.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)

Zvolte **přehledu Web Stack láskou MS** odkaz.

[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)

Zobrazení plochy výchozí je v pořádku, ale může to zlepšit.

Kopírovat *Views\Home\SessionByCode.cshtml* k *Views\Home\SessionByCode.Mobile.cshtml* a nahraďte obsah *Views\Home\SessionByCode.Mobile.cshtml*souboru následujícím kódem:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample25.cshtml)]

Nový kód používá `data-role` atribut ke zlepšení rozložení zobrazení.

Aktualizace mobilních prohlížečů. Na následujícím obrázku odráží změny kódu, které jste právě vytvořili:

[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)

## <a name="wrapup-and-review"></a>Wrapup a revize

V tomto kurzu má zavedeny nové mobilní funkce ASP.NET MVC 4 pro vývojáře ve verzi Preview. Mobilní funkce patří:

- Možnost přepsat rozložení a zobrazení, částečná zobrazení, jak globálně a pro jednotlivé zobrazení.
- Řídit rozložení a pomocí přepsání částečné vynucení `RequireConsistentDisplayMode` vlastnost.
- Widget přepínači zobrazení pro mobilní zařízení zobrazení než lze také zobrazit v zobrazení plochy.
- Podpora pro podporu určité prohlížeče, jako je iPhone prohlížeče.

## <a name="see-also"></a>Viz také

- [jQuery Mobile](http://jquerymobile.com) lokality.
- [jQuery Mobile přehled](http://jquerymobile.com/demos/1.0b3/docs/about/intro.html)
- [Osvědčené postupy W3C doporučení mobilní webové aplikace](http://www.w3.org/TR/mwabp/)
- [Doporučení W3C Release Candidate pro dotazy na média](http://www.w3.org/TR/css3-mediaqueries/)
