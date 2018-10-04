---
uid: mvc/overview/performance/bundling-and-minification
title: Sdružování a Minifikace | Dokumentace Microsoftu
author: Rick-Anderson
description: Sdružování a minifikace jsou dva postupy můžete použít v technologii ASP.NET 4.5 na zvýšení zatížení doba požadavku. Sdružování a minifikace zlepšuje reducin doba načítání...
ms.author: riande
ms.date: 08/23/2012
ms.assetid: 5894dc13-5d45-4dad-8096-136499120f1d
msc.legacyurl: /mvc/overview/performance/bundling-and-minification
msc.type: authoredcontent
ms.openlocfilehash: 9b627a66007aec09a404147698e2bef06c7e7794
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577714"
---
<a name="bundling-and-minification"></a>Sdružování a Minifikace
====================
Podle [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Sdružování a minifikace jsou dva postupy můžete použít v technologii ASP.NET 4.5 na zvýšení zatížení doba požadavku. Sdružování a minifikace zlepšuje dobu načítání snížením počtu požadavků na serveru a snížením velikosti požadovaných prostředků (například šablon stylů CSS a JavaScript.)


Většina aktuální hlavní prohlížeče omezit počet [současných připojení](http://www.browserscope.org/?category=network) za každý název hostitele na šest. To znamená, že při zpracování šest požadavků, další požadavky na prostředky v hostiteli se zařadí do fronty v prohlížeči. Na následujícím obrázku zobrazuje karty sítě nástroje pro vývojáře IE F12 časování pro prostředky vyžadované o zobrazení a ukázkové aplikace.

![B/M](bundling-and-minification/_static/image1.png)

Šedé pruhy ukazují čas, kdy žádosti zařazené do fronty v prohlížeči čekání na limitu šest připojení. Žlutý pruh je požadavek doba do prvního bajtu, tedy čas potřebný k odeslání požadavku a příjem první odpovědi ze serveru. Modré pruhy ukazují čas potřebný k přijetí dat odpovědi ze serveru. Poklikáním na prostředek k získání podrobných informací o časování. Například následující obrázek ukazuje podrobnosti časování pro načtení */Scripts/MyScripts/JavaScript6.js* souboru.

![](bundling-and-minification/_static/image2.png)

Předchozí obrázek ukazuje **Start** události, které poskytuje čas požadavku byl zařazen do fronty z důvodu prohlížeči omezit počet souběžných připojení. V takovém případě požadavku byl zařazen do fronty pro 46 milisekund čekání na další požadavek dokončit.

## <a name="bundling"></a>Sdružování

Sdružování je nová funkce v technologii ASP.NET 4.5, který umožňuje snadno kombinovat nebo sady více souborů do jediného souboru. Můžete vytvořit šablony stylů CSS, JavaScript a další svazky. Menší počet souborů znamená, že menší počet požadavků HTTP a která vylepší zatížení první stránky.

Následující obrázek ukazuje zobrazení časování o zobrazení uvedeno dříve, ale tentokrát s sdružování a minifikace povolena.

![](bundling-and-minification/_static/image3.png)

## <a name="minification"></a>Připravenost k minifikaci

Připravenost k minifikaci provede celou řadu různých kód optimalizace pro skripty a css, jako je například odebráním nepotřebných prázdné znaky a komentářů a zkrácení názvy proměnných jeden znak. Vezměte v úvahu následující funkce jazyka JavaScript.

[!code-javascript[Main](bundling-and-minification/samples/sample1.js)]

Po připravenost k minifikaci je omezit funkce takto:

[!code-javascript[Main](bundling-and-minification/samples/sample2.js)]

Kromě odebrání komentáře a zbytečné mezery, tyto parametry a názvy proměnných byly přejmenovány (zkrátila) následujícím způsobem:

| **Původní** | **Přejmenovat** |
| --- | --- |
| imageTagAndImageID | n |
| imageContext | t |
| imageElement | Mohu |

## <a name="impact-of-bundling-and-minification"></a>Dopad sdružování a Minifikace

V následující tabulce jsou uvedeny několik důležitých rozdílů mezi jednotlivě výpis všech prostředků a pomocí sdružování a minifikace (B/M) v ukázkové aplikaci.

|  | **Pomocí B/M** | **Bez B/M** | **Změna** |
| --- | --- | --- | --- |
| **Požadavky na soubory** | 9 | 34 | 256% |
| **Odeslání KB** | 3.26 | 11.92 | 266% |
| **Přijata KB** | 388.51 | 530 | 36% |
| **Čas načtení** | 510 MS | 780 MS | 53% |

Odeslané bajty došlo k výraznému snížení s sdružování prohlížeče jsou poměrně podrobné s hlavičky protokolu HTTP, které se vztahují na požadavky. Přijaté bajty snížení není velké protože největší soubory (*skripty\\jquery-ui-1.8.11.min.js* a *skripty\\jquery 1.7.1.min.js*) jsou již minifikovaný . Poznámka: Časování na ukázkový program používá [Fiddler](http://www.fiddler2.com/fiddler2/) nástroj pro simulaci pomalou síť. (Z Fiddler **pravidla** nabídce vyberte možnost **výkonu** pak **simulovat rychlostí**.)

## <a name="debugging-bundled-and-minified-javascript"></a>Ladění spojeny a Minifikovaný jazyka JavaScript

Snadno ladit JavaScript ve vývojovém prostředí (kde [prvek compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) v *Web.config* souboru má nastavenou `debug="true"` ) vzhledem k tomu, že nejsou spojeny soubory jazyka JavaScript nebo minifikovaný. Můžete také ladit sestavení pro vydání, pokud soubory jazyka JavaScript jsou spojeny a minifikovaný. Ladění pomocí vývojářských nástrojů F12 aplikace Internet Explorer, funkce JavaScriptu zahrnuté minifikovaný sady prostředků používá následující postup:

1. Vyberte **skript** kartu a potom vyberte **spustit ladění** tlačítko.
2. Vyberte sadu obsahující funkce JavaScriptu, který chcete ladit pomocí tlačítka pro prostředky.  
    ![](bundling-and-minification/_static/image4.png)
3. Formátování minifikovaný jazyka JavaScript tak, že vyberete **tlačítko konfigurace** ![](bundling-and-minification/_static/image5.png)a pak vyberete **formát JavaScript**.
4. V **skript vyhledávání** vstupní pole, vyberte název funkce, který chcete ladit. Na následujícím obrázku **AddAltToImg** jste zadali v **skript vyhledávání** vstupního pole.  
    ![](bundling-and-minification/_static/image6.png)

Další informace o ladění pomocí nástroje pro vývojáře F12, najdete v článku na webu MSDN [pomocí vývojářských nástrojů F12 k ladění chyby JavaScriptu](https://msdn.microsoft.com/library/ie/gg699336(v=vs.85).aspx).

## <a name="controlling-bundling-and-minification"></a>Řídící sdružování a Minifikace

Sdružování a minifikace povolit nebo zakázat tak, že nastavíte hodnotu atributu ladění v [prvek compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) v *Web.config* souboru. V následujícím souboru XML `debug` je nastavena na hodnotu true, tak sdružování a minifikace je zakázaná.

[!code-xml[Main](bundling-and-minification/samples/sample3.xml?highlight=2)]

Chcete-li povolit sdružování a minifikace, nastavte `debug` hodnotu "false". Můžete přepsat *Web.config* nastavení `EnableOptimizations` vlastnost `BundleTable` třídy. Následující kód umožňuje sdružování a minifikace a přepíše jakékoli nastavení v *Web.config* souboru.

[!code-csharp[Main](bundling-and-minification/samples/sample4.cs?highlight=7)]

> [!NOTE]
> Není-li `EnableOptimizations` je `true` nebo atribut v ladění [prvek compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) v *Web.config* soubor je nastavený na `false`, soubory nebudou spojeny ani minifikovaný. Kromě toho se nepoužije .min verzi souborů, úplné ladicí verze bude vybrána. `EnableOptimizations` přepíše atribut v ladění [prvek compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) v *Web.config* souboru


## <a name="using-bundling-and-minification-with-aspnet-web-forms-and-web-pages"></a>Pomocí sdružování a Minifikace s webovými formuláři ASP.NET a webových stránek

- Pro webové stránky, naleznete v příspěvku blogu [přidání optimalizaci webů pro webové stránky webu](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- Webové formuláře, naleznete v příspěvku blogu [přidání sdružování a Minifikace do webových formulářů](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).

## <a name="using-bundling-and-minification-with-aspnet-mvc"></a>Pomocí sdružování a Minifikace s architekturou ASP.NET MVC

V této části vytvoříme ASP.NET MVC projekt tak, aby zkontrolovat sdružování a minifikace. Nejprve vytvořte nový projekt ASP.NET MVC internet s názvem **MvcBM** beze změny některý z výchozí hodnoty.

Otevřít *aplikace\\\_Start\\BundleConfig.cs* souboru a zkoumat `RegisterBundles` metodu, která se používá k vytváření, registrace a konfigurace sady. Následující kód ukazuje část `RegisterBundles` metody.

[!code-csharp[Main](bundling-and-minification/samples/sample5.cs)]

Předchozí kód vytvoří novou sadu JavaScript s názvem *~/bundles/jquery* , který obsahuje všechny příslušné (, které je ladění nebo ale ne minifikovaný. *vsdoc*) soubory *skripty* složku, která odpovídá řetězci zástupný znak "~/Scripts/jquery-{version} .js". Pro technologii ASP.NET MVC 4, to znamená, že s konfiguraci ladění, soubor *jquery 1.7.1.js* se přidají do sady. V konfiguraci vydané verze *jquery 1.7.1.min.js* budou přidány. Vytváření prostředků framework následuje několik běžné konvence jako například:

- Výběr souboru ".min" pro uvolnění při *FileX.min.js* a *FileX.js* existovat.
- Výběr verze bez ".min" pro ladění.
- Ignoruje se "-vsdoc" soubory (například *jquery. 1.7.1 vsdoc.js*), které jsou používány pouze technologie IntelliSense.

`{version}` Zástupný znak porovnání uvedené výše se používá pro automatické vytvoření svazku jQuery s příslušnou verzi jQuery ve vaší *skripty* složky. V tomto příkladu pomocí zástupný znak poskytuje následující výhody:

- Umožňuje aktualizovat na novější verzi jQuery beze změny předchozí kód sady nebo jQuery odkazů v zobrazení stránek pomocí balíčku NuGet.
- Automaticky vybere na plnou verzi pro konfiguraci ladění a verze ".min" pro verzi sestavení.

## <a name="using-a-cdn"></a>Používání sítě CDN

 Následující kód nahradí sady místní jQuery sada jQuery CDN.

[!code-csharp[Main](bundling-and-minification/samples/sample6.cs)]

Zatímco ve verzi režimu a ladicí verzi jQuery se načtou místně v režimu ladění, bude ve výše uvedeném kódu, požadovaných jQuery z CDN. Při používání sítě CDN, měli byste mít záložní mechanismus v případě selhání CDN požadavku. Následující kód fragment od konce souboru, který ukazuje soubor rozložení přidána do žádosti o jQuery by měla selhání CDN.

[!code-cshtml[Main](bundling-and-minification/samples/sample7.cshtml?highlight=5-13)]

## <a name="creating-a-bundle"></a>Vytvoření svazku

[Sady](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) třídy `Include` metoda přijímá pole řetězců, kde je každý řetězec virtuální cesty k prostředku. Následující kód z `RegisterBundles` metodu *aplikace\\\_Start\\BundleConfig.cs* soubor ukazuje, jak více souborů jsou přidány k sadě:

[!code-csharp[Main](bundling-and-minification/samples/sample8.cs)]

[Sady](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) třídy `IncludeDirectory` zadaná metoda přidat všechny soubory v adresáři (a volitelně všech podadresářích), které odpovídají vzoru hledání. [Sady](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) třídy `IncludeDirectory` rozhraní API je uveden níže:

[!code-csharp[Main](bundling-and-minification/samples/sample9.cs)]

Sady jsou odkazovány v zobrazení pomocí metody vykreslení (`Styles.Render` pro šablony stylů CSS a `Scripts.Render` pro jazyk JavaScript). Následující kód z *zobrazení\\Shared\\\_Layout.cshtml* soubor ukazuje, jak odkazovat na výchozí zobrazení projektu ASP.NET internet sady šablon stylů CSS a JavaScriptu.

[!code-cshtml[Main](bundling-and-minification/samples/sample10.cshtml?highlight=5-6,11)]

Všimněte si, že metody vykreslení přijímá pole řetězců, takže můžete přidat víc sad na jednom řádku kódu. Obecně můžete použít metody vykreslení, které vytvářejí nezbytné HTML tak, aby odkazovaly asset. Můžete použít `Url` metoda ke generování adresy URL k prostředku bez potřeby tak, aby odkazovaly asset značky. Předpokládejme, že jste chtěli použít nové HTML5 [asynchronní](http://www.whatwg.org/specs/web-apps/current-work/#attr-script-async) atribut. Následující kód ukazuje, jak odkazovat pomocí modernizr `Url` metody.

[!code-cshtml[Main](bundling-and-minification/samples/sample11.cshtml?highlight=11)]

## <a name="using-the--wildcard-character-to-select-files"></a>Použití "\*" zástupný znak a vyberte soubory

Virtuální cesta zadaná v `Include` metoda a hledání vzorku v `IncludeDirectory` metoda může přijmout jednu "\*" zástupný znak jako předponu nebo příponu je třeba v posledním segmentu cesty. Hledaný řetězec se nerozlišují malá a velká písmena. `IncludeDirectory` Metoda má povolenou možnost hledání podadresářů.

Vezměte v úvahu projekt s následující soubory jazyka JavaScript:

- *Skripty\\běžné\\AddAltToImg.js*
- *Skripty\\běžné\\ToggleDiv.js*
- *Skripty\\běžné\\ToggleImg.js*
- *Skripty\\běžné\\Sub1\\ToggleLinks.js*

![dir imag](bundling-and-minification/_static/image7.png)

Soubory přidané do sady pomocí zástupného znaku, jak je znázorněno v následující tabulce:

| **Volání** | **Soubory přidané nebo výjimce** |
| --- | --- |
| Zahrnout ("~/Scripts/Common/\*.js") | *AddAltToImg.js*, *ToggleDiv.js*, *ToggleImg.js* |
| Zahrnout ("~/Scripts/Common/T\*.js") | Neplatný vzor došlo k výjimce. Zástupný znak je povolena pouze u předpony nebo přípony. |
| Zahrnout ("~/Scripts/Common/\*og.\*") | Neplatný vzor došlo k výjimce. Je povolený jenom jeden zástupný znak. |
| Zahrnout ("~/Scripts/Common/T\*") | *ToggleDiv.js*, *ToggleImg.js* |
| Zahrnout ("~/Scripts/Common/\*") | Neplatný vzor došlo k výjimce. Čistě zástupný segment není platný. |
| IncludeDirectory ("~/Scripts/Common", "T\*") | *ToggleDiv.js*, *ToggleImg.js* |
| IncludeDirectory ("~/Scripts/Common", "T\*", true) | *ToggleDiv.js*, *ToggleImg.js*, *ToggleLinks.js* |

Každý soubor explicitně přidáte k sadě je obecně upřednostňované přes zástupnými znaky načítání souborů z následujících důvodů:

- Přidávání skriptů pomocí zástupných znaků výchozí hodnoty na načítání v abecedním pořadí, což je obvykle nechcete. Soubory šablon stylů CSS a JavaScript je často potřeba přidat v určitém pořadí (neabecední). Toto riziko lze snížit tak, že přidáte vlastní [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) implementaci, ale explicitně přidat každého souboru je menší náchylné k chybám. Například můžete přidat nové prostředky do složky v budoucnu může být potřeba upravit vaše [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) implementace.
- Zobrazit konkrétní soubory přidané do adresáře pomocí zástupný znak načítání mohou být součástí všech zobrazení odkazující na tuto sadu. Je-li zobrazit konkrétní skript se přidá k sadě, se může zobrazit chyba jazyka JavaScript na jiné zobrazení, které odkazují na sadu.
- Soubory CSS, které importují ostatní soubory za následek importované soubory načíst dvakrát. Například následující kód vytvoří sada prostředků s největším počtem souborů CSS motiv uživatelského rozhraní jQuery načíst dvakrát. 

    [!code-csharp[Main](bundling-and-minification/samples/sample12.cs)]

  Selektor zástupný znak "\*.css" přináší do každého souboru CSS ve složce, včetně *obsahu\\motivy\\základní\\jquery.ui.all.css* souboru. *Jquery.ui.all.css* soubor importuje další soubory šablon stylů CSS.

## <a name="bundle-caching"></a>Vytvoření balíčku ukládání do mezipaměti

Sady nastavit hlavičku protokolu HTTP vyprší platnost jeden rok od při vytvoření sady. Když přejdete na stránku předchozího prohlíženého ukazuje Fiddleru IE neprovede Podmíněný požadavek pro sadu, to znamená, nejsou žádné požadavky metody GET protokolu HTTP z Internet Exploreru pro sady a žádné odpovědi HTTP 304 ze serveru. Můžete vynutit IE provést podmíněný požadavek pro každou sadu pomocí klávesy F5 (což v odpovědi HTTP 304 pro každou sadu). Úplná aktualizace můžete vynutit pomocí ^ F5 (výsledkem odpověď HTTP 200 pro každé sady prostředků.)

Na následujícím obrázku **ukládání do mezipaměti** kartu podokna Fiddleru odpovědi:

![ukládání do mezipaměti image fiddleru](bundling-and-minification/_static/image8.png)

Žádost   
`http://localhost/MvcBM_time/bundles/AllMyScripts?v=r0sLDicvP58AIXN_mc3QdyVvVj5euZNzdsa2N1PKvb81`  
 je pro sadu **AllMyScripts** a obsahuje pár řetězec dotazu **v = r0sLDicvP58AIXN\\\_mc3QdyVvVj5euZNzdsa2N1PKvb81**. Řetězec dotazu **v** má hodnotu token, který je jedinečný identifikátor sloužící k ukládání do mezipaměti. Tak dlouho, dokud nedojde ke změně sady, bude vyžadovat aplikace ASP.NET **AllMyScripts** sady prostředků pomocí tohoto tokenu. Pokud změn v souboru v sadě, optimalizační rozhraní technologie ASP.NET se vygenerovat nový token, zajištění, že požadavky na prohlížeč pro sadu získá nejnovější sady.

Je-li spustit nástroje pro vývojáře IE9 F12 a přejděte na stránku dříve načtená, IE nesprávně ukazuje podmíněné požadavků GET na každá sada a server vrací HTTP 304. Si můžete přečíst, proč IE9 má potíže s zjištění, zda Podmíněný požadavek byl proveden v příspěvku na blogu [použití sítě CDN a Expires ke zlepšení výkonu webu](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

## <a name="less-coffeescript-scss-sass-bundling"></a>CoffeeScript, SCSS, Sass, méně sdružování.

Sdružování a minifikace framework poskytuje mechanismus pro zpracování zprostředkující jazyků, jako [SCSS](http://sass-lang.com/), [Sass](http://sass-lang.com/), [méně](http://www.dotlesscss.org/) nebo [Coffeescript ](http://coffeescript.org/)a použití transformací, jako je připravenost k minifikaci výslednou sadu. Chcete-li například přidat [.less](http://www.dotlesscss.org/) soubory do projektu MVC 4:

1. Vytvořte složku pro méně obsah. V následujícím příkladu *obsahu\\MyLess* složky.
2. Přidat [.less](http://www.dotlesscss.org/) balíček NuGet **bez tečky** do projektu.  
    ![Instalace bez tečky NuGet](bundling-and-minification/_static/image9.png)
3. Přidat třídu, která implementuje [IBundleTransform](https://msdn.microsoft.com/library/system.web.optimization.ibundletransform(VS.110).aspx) rozhraní. Pro transformace .less přidejte následující kód do vašeho projektu.

    [!code-csharp[Main](bundling-and-minification/samples/sample13.cs)]
4. Vytvořit sadu s méně soubory `LessTransform` a [CssMinify](https://msdn.microsoft.com/library/system.web.optimization.cssminify(VS.110).aspx) transformace. Přidejte následující kód, který `RegisterBundles` metoda ve *aplikace\\_spustit\\BundleConfig.cs* souboru.

    [!code-csharp[Main](bundling-and-minification/samples/sample14.cs)]
5. Přidejte následující kód všech zobrazení, která odkazuje na menší sadu.

    [!code-cshtml[Main](bundling-and-minification/samples/sample15.cshtml)]

## <a name="bundle-considerations"></a>Důležité informace o sadě

Dobré konvence při vytváření sady se zahrnou "svázat" jako předponu v název sady prostředků. To zabrání možný výskyt [směrování konflikt](https://forums.asp.net/post/5012037.aspx).

Jakmile upgradujete jeden soubor v sadě, se vygeneruje nový token pro parametr řetězce dotazu sady a úplné sady musí být staženy automaticky při příštím klient požádá o stránku obsahující sadu. V tradičních kódu, kde je každý prostředek uvedeni jednotlivě byste stáhli pouze změněný soubor. Prostředky, které se často mění, nemusí být vhodnými kandidáty pro sdružování.

Sdružování a minifikace především zvýšení první čas načítání stránky požadavku. Jakmile požádal webovou stránku v prohlížeči ukládá do mezipaměti prostředky (jazyka JavaScript, CSS a obrázků) tak, aby sdružování a minifikace neposkytne žádné zvýšení výkonu při požadavku na stejnou stránku nebo stránky na stejném webu, požadování stejné prostředky. Pokud nenastavíte vyprší platnost záhlaví správně na vaše prostředky a nepoužíváte sdružování a minifikace, heuristiky aktuálnosti prohlížeče označí prostředky zastaralé po několika dnech a prohlížeč bude vyžadovat ověření žádosti pro každý prostředek. V takovém případě sdružování a minifikace poskytnout zvýšení výkonu od prvního požadavku na stránce. Další podrobnosti najdete v blogu [použití sítě CDN a Expires ke zlepšení výkonu webu](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

Omezení prohlížeče šest souběžných připojení za každý název hostitele lze zmírnit použitím [CDN](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx). Protože CDN bude mít jiný název hostitele než hostingové lokalitu, nebude asset požadavky z CDN započítávat šest limitu souběžných připojení do hostitelského prostředí. Sítě CDN můžete také zadat běžné ukládání do mezipaměti balíčku a hraničními zařízeními výhody ukládání do mezipaměti.

Sady by měly být rozdělené podle stránky, které potřebujete. Například výchozí šablony ASP.NET MVC pro aplikaci internet vytvoří svazek sad ověřování jQuery odděleně od jQuery. Protože výchozí zobrazení vytvořené žádný vstup a neprovede odeslání hodnoty, jejich nezahrnují sady ověřování.

`System.Web.Optimization` Obor názvů je implementována v *System.Web.Optimization.dll*. Využívá knihovna WebGrease (*WebGrease.dll*) pro možnosti připravenost k minifikaci, který pak používá *Antlr3.Runtime.dll*.

*Používám Twitteru rychlé příspěvky a sdílet odkazy. Tento popisovač Twitteru je*: [@RickAndMSFT](http://twitter.com/RickAndMSFT)

## <a name="additional-resources"></a>Další zdroje

- Video:[sdružování a optimalizace](https://channel9.msdn.com/Events/aspConf/aspConf/Bundling-and-Optimizing) podle [Howard Dierking](https://twitter.com/#!/howard_dierking)
- [Přidání optimalizaci webů do webových stránek webu](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- [Přidání sdružování a Minifikace do webových formulářů](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).
- [Výkon důsledky sdružování a Minifikace na procházení webu](https://blogs.msdn.com/b/henrikn/archive/2012/06/17/performance-implications-of-bundling-and-minification-on-http.aspx) podle [společnost Nielsen Petr F](http://en.wikipedia.org/wiki/Henrik_Frystyk_Nielsen) [@frystyk](https://twitter.com/frystyk)
- [Použití sítě CDN a zajistí vypršení platnosti ke zlepšení výkonu webu](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx) od Ricka Andersona [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)
- [Minimalizovat požadavku (doby odezvy)](https://developers.google.com/speed/docs/best-practices/rtt)

## <a name="contributors"></a>Contributors

- Ani Haovi společnosti
- [Howard Dierking](https://twitter.com/#!/howard_dierking)
- Diana LaRose
