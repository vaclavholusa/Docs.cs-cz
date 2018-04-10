---
uid: mvc/overview/performance/bundling-and-minification
title: Sdružování a minimalizace | Microsoft Docs
author: Rick-Anderson
description: Sdružování a minimalizace jsou dvě techniky v technologii ASP.NET 4.5 můžete použít ke zlepšení čas načítání požadavku. Sdružování a minimalizace zlepšuje reducin čas načítání...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/23/2012
ms.topic: article
ms.assetid: 5894dc13-5d45-4dad-8096-136499120f1d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/bundling-and-minification
msc.type: authoredcontent
ms.openlocfilehash: 001ebf89cda66a50cddcd7e4944f27b9396d4450
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/10/2018
---
<a name="bundling-and-minification"></a>Sdružování a minimalizace
====================
podle [Rick Anderson](https://github.com/Rick-Anderson)

> Sdružování a minimalizace jsou dvě techniky v technologii ASP.NET 4.5 můžete použít ke zlepšení čas načítání požadavku. Sdružování a minimalizace zlepšuje čas načítání snižuje počet požadavků na server a zmenšení velikosti požadovaných prostředků (například šablon stylů CSS a JavaScript.)


Většina aktuální hlavní prohlížeče omezit počet [současných připojení](http://www.browserscope.org/?category=network) za každý název hostitele pro šest. To znamená, že šest požadavky během zpracování, další požadavky na prostředky na hostiteli se zařadí do fronty v prohlížeči. Na obrázku níže ukazuje karty sítě nástroje pro vývojáře IE F12 časování prostředků vyžaduje zobrazení o ukázkovou aplikaci.

![B/M](bundling-and-minification/_static/image1.png)

Šedé pruhy Zobrazí dobu, kterou požadavek ve frontě v prohlížeči čekání na limitu šesti připojení. Žlutý pruh je požadavek doba do prvního bajtu, který je čas potřebný k žádosti odesílat a přijímat první odpověď ze serveru. Modré pruhy zobrazit čas potřebný k přijetí data odpovědi ze serveru. Poklepáním na prostředek se získat informace o podrobné časování. Například následující obrázek ukazuje časové podrobnosti načítání */Scripts/MyScripts/JavaScript6.js* souboru.

![](bundling-and-minification/_static/image2.png)

Předchozí obrázek ukazuje **spustit** událostí, které poskytuje čas požadavku byl zařazen do fronty z důvodu prohlížeče omezit počet souběžných připojení. V takovém případě požadavek byl zařazen do fronty pro 46 počet milisekund čekání na další požadavek dokončit.

## <a name="bundling"></a>Sdružování

Sdružování je nová funkce v technologii ASP.NET 4.5, která usnadňuje kombinovat nebo sady více souborů do jediného souboru. Můžete vytvořit šablon stylů CSS, JavaScript a dalších sad. Méně souborů znamená, že méně požadavků HTTP a který může zvýšit zatížení první stránky.

Následující obrázek znázorňuje zobrazení časování o zobrazení zobrazí dříve, ale tentokrát s sdružování a minimalizace povolena.

![](bundling-and-minification/_static/image3.png)

## <a name="minification"></a>Minimalizace

Minimalizace provede celou řadu různých kód optimalizace skripty nebo šablon stylů css, jako je například odebráním nepotřebných mezer a komentáře a zkrátit názvy proměnných jeden znak. Vezměte v úvahu následující funkce jazyka JavaScript.

[!code-javascript[Main](bundling-and-minification/samples/sample1.js)]

Po minimalizaci je funkce snížen na následující:

[!code-javascript[Main](bundling-and-minification/samples/sample2.js)]

Kromě odebírání komentářů a nepotřebné prázdné znaky, tyto parametry a proměnné názvy byly přejmenovány (zkrátit) následujícím způsobem:

| **Původní** | **Přejmenovat** |
| --- | --- |
| imageTagAndImageID | n |
| imageContext | t |
| imageElement | Mohu |

## <a name="impact-of-bundling-and-minification"></a>Dopad sdružování a Minifikace

Následující tabulka uvádí několik důležitých rozdílů mezi výpis všechny prostředky samostatně a pomocí sdružování a minimalizace (B/M) v ukázka programu.

|  | **Pomocí B/M** | **Bez B/M** | **Change** |
| --- | --- | --- | --- |
| **Požadavky na soubory** | 9 | 34 | 256% |
| **KB Sent** | 3.26 | 11.92 | 266% |
| **KB Received** | 388.51 | 530 | 36% |
| **Čas načtení** | 510 MS | 780 MS | 53% |

Počet odeslaných bajtů došlo k výraznému snížení s sdružování jsou poměrně podrobné s hlavičky protokolu HTTP, které se vztahují na požadavky prohlížeče. Přijaté bajty snížení není tak velké, protože největší soubory (*Scripts\jquery-ui-1.8.11.min.js* a *Scripts\jquery-1.7.1.min.js*) jsou již minifikovaný. Poznámka: Časování na ukázkový program použitý [Fiddler](http://www.fiddler2.com/fiddler2/) nástroj k simulaci pomalé síti. (Z aplikaci Fiddler **pravidla** nabídce vyberte možnost **výkonu** pak **simulovat rychlostí**.)

## <a name="debugging-bundled-and-minified-javascript"></a>Ladění dodávat a Minifikovaný JavaScript

Usnadňuje ladění vaší JavaScriptu ve vývojovém prostředí (kde [kompilace Element](https://msdn.microsoft.com/library/s10awwz0.aspx) v *Web.config* soubor pro `debug="true"` ) vzhledem k tomu, že nejsou seskupeny soubory JavaScript nebo minifikovaný. Můžete také ladění sestavení pro vydání JavaScript souborů jsou seskupeny a minifikovaný. Používání nástrojů pro vývojáře IE F12, ladění funkce jazyka JavaScript, který je součástí sady minifikovaný pomocí následující postup:

1. Vyberte **skriptu** a pak vyberte **spustit ladění** tlačítko.
2. Vyberte sadu obsahující funkce JavaScript, který chcete ladit pomocí tlačítka prostředky.  
    ![](bundling-and-minification/_static/image4.png)
3. Formátování minifikovaný JavaScript výběrem **tlačítko konfigurace** ![](bundling-and-minification/_static/image5.png)a potom vyberete **formát JavaScript**.
4. V **skript vyhledávání** t vstupní pole, vyberte název funkce, kterou chcete ladit. Na následujícím obrázku **AddAltToImg** bylo zadáno v **skript vyhledávání** t vstupního pole.  
    ![](bundling-and-minification/_static/image6.png)

Další informace o ladění pomocí nástrojů pro vývojáře F12, najdete v článku na webu MSDN [používání nástrojů pro vývojáře F12 k ladění chyby jazyka JavaScript](https://msdn.microsoft.com/library/ie/gg699336(v=vs.85).aspx).

## <a name="controlling-bundling-and-minification"></a>Řízení sdružování a minimalizace

Sdružování a minimalizace povolit nebo zakázat nastavením hodnoty atributu ladění v [kompilace Element](https://msdn.microsoft.com/library/s10awwz0.aspx) v *Web.config* souboru. V následující soubor XML `debug` je nastaven na hodnotu true, tak sdružování a minimalizace je zakázána.

[!code-xml[Main](bundling-and-minification/samples/sample3.xml?highlight=2)]

Chcete-li povolit sdružování a minimalizace, nastavte `debug` hodnotu "Nepravda". Můžete přepsat *Web.config* nastavení s `EnableOptimizations` vlastnost na `BundleTable` třídy. Následující kód umožňuje sdružování a minimalizace a přepíše všechna nastavení v *Web.config* souboru.

[!code-csharp[Main](bundling-and-minification/samples/sample4.cs?highlight=7)]

> [!NOTE]
> Pokud `EnableOptimizations` je `true` nebo ladění atribut v [kompilace Element](https://msdn.microsoft.com/library/s10awwz0.aspx) v *Web.config* soubor pro `false`, soubory nebude dodávat nebo minifikovaný. Kromě toho se nepoužijí .min verze souborů, úplné ladicí verze bude vybrána. `EnableOptimizations` přepíše ladění atribut v [kompilace Element](https://msdn.microsoft.com/library/s10awwz0.aspx) v *Web.config* souboru


## <a name="using-bundling-and-minification-with-aspnet-web-forms-and-web-pages"></a>Pomocí sdružování a minimalizace s webovými formuláři ASP.NET a webové stránky

- Pro webové stránky, najdete v položkách blogu [přidání optimalizaci webů k lokalitě webové stránky](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- Webové formuláře, naleznete v příspěvku blogu [přidání sdružování a minimalizace pro webové formuláře](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).

## <a name="using-bundling-and-minification-with-aspnet-mvc"></a>Pomocí sdružování a minimalizace s architekturou ASP.NET MVC

V této části vytvoříme ASP.NET MVC projektu prozkoumat sdružování a minimalizace. Nejprve vytvořte nový projekt ASP.NET MVC internet s názvem **MvcBM** beze změny všechny výchozí hodnoty.

Otevřete *aplikace\_Start\BundleConfig.cs* soubor a zkontrolujte `RegisterBundles` metodu, která se používá k vytvoření, registrace a konfigurace sady. Následující kód ukazuje část `RegisterBundles` metoda.

[!code-csharp[Main](bundling-and-minification/samples/sample5.cs)]

Předchozí kód vytvoří novou sadu JavaScript s názvem *~/bundles/jquery* , který obsahuje všechny příslušné (který je ladění nebo ale není minifikovaný. *vsdoc*) soubory *skripty* složky, která odpovídají řetězci zástupný znak "~/Scripts/jquery-{version} .js". Pro technologii ASP.NET MVC 4, to znamená v konfiguraci ladění, soubor *jquery 1.7.1.js* budou přidány do sady. V rámci konfigurace verze *jquery 1.7.1.min.js* bude přidán. Rozhraní instalujícího dodržovat konvence prostředí několik běžných jako například:

- Soubor ".min" pro verzi vyberete, pokud existují "FileX.min.js" a "FileX.js".
- Vyberte jiný ".min" verze pro ladění.
- Ignoruje "-vsdoc" (například jquery-1.7.1-vsdoc.js), soubory, které se používají pouze technologii IntelliSense.

`{version}` Zástupný znak odpovídající uvedené výše se používá pro automatické vytvoření sady jQuery s příslušnou verzi jQuery ve vaší *skripty* složky. V tomto příkladu pomocí zástupný znak poskytuje následující výhody:

- Umožňuje aktualizovat na novější verzi jQuery beze změn v předchozí instalujícího kód nebo jQuery odkazy na stránkách zobrazení pomocí balíčku NuGet.
- Automaticky vybere na plnou verzi pro konfiguraci ladění a verzí ".min" pro verzi sestavení.

## <a name="using-a-cdn"></a>Pomocí název CDN

 Postupujte podle kódu nahradí sadu místní jQuery sady jQuery CDN.

[!code-csharp[Main](bundling-and-minification/samples/sample6.cs)]

Zatímco ve verzi režimu a ladění verzi jQuery budou načteny místně v režimu ladění, bude ve výše uvedeném kódu, vyžádána jQuery od CDN. Při použití CDN, měli byste mít nouzový mechanismus, v případě, že CDN požadavek selže. Následující kód fragmentovat od konce skript ukazuje souboru rozložení přidána do žádosti o jQuery by měla selhání CDN.

[!code-cshtml[Main](bundling-and-minification/samples/sample7.cshtml?highlight=5-13)]

## <a name="creating-a-bundle"></a>Vytvoření sady

[Sady](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) třída `Include` metoda přebírá pole řetězců, kde je každý řetězec virtuální cesty k prostředku. Následující kód z metody RegisterBundles v *aplikace\_Start\BundleConfig.cs* souboru ukazuje, jak více souborů, které jsou přidány do sady:

[!code-csharp[Main](bundling-and-minification/samples/sample8.cs)]

[Sady](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) třída `IncludeDirectory` metoda zajišťuje přidejte všechny soubory v adresáři (a volitelně všechny podadresáře), které se shodují vzoru hledání. [Sady](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) třída `IncludeDirectory` rozhraní API jsou uvedeny níže:

[!code-csharp[Main](bundling-and-minification/samples/sample9.cs)]

Sady odkazují v zobrazení pomocí metody vykreslení ( `Styles.Render` pro šablon stylů CSS a `Scripts.Render` pro jazyk JavaScript). Následující kód z *Views\Shared\\_Layout.cshtml* souboru ukazuje, jak výchozí zobrazení projektu ASP.NET internet odkazovat sady šablon stylů CSS a JavaScript.

[!code-cshtml[Main](bundling-and-minification/samples/sample10.cshtml?highlight=5-6,11)]

Všimněte si, vykreslení metody má pole řetězců, takže můžete přidat více sad na jeden řádek kódu. Obvykle můžete používat vykreslení metody, které pak vytvoří nezbytné HTML tak, aby odkazovaly asset. Můžete použít `Url` metoda ke generování adresy URL pro daný prostředek bez značek potřeby tak, aby odkazovaly asset. Předpokládejme, že jste chtěli použít novou HTML5 [asynchronní](http://www.whatwg.org/specs/web-apps/current-work/#attr-script-async) atribut. Následující kód ukazuje, jak odkazovat pomocí modernizr `Url` metoda.

[!code-cshtml[Main](bundling-and-minification/samples/sample11.cshtml?highlight=11)]

## <a name="using-the--wildcard-character-to-select-files"></a>Pomocí "\*" zástupný znak a vyberte soubory

Virtuální cestě uvedené v `Include` metoda a vyhledávání vzorů v `IncludeDirectory` metoda může přijmout jeden "\*" zástupný znak jako předponu nebo příponu v poslední segment cesty. Řetězec pro hledání nejsou rozlišována malá a velká písmena. `IncludeDirectory` Metoda má možnost hledání podadresářů.

Zvažte projekt s následující soubory JavaScript:

- *Scripts\Common\AddAltToImg.js*
- *Scripts\Common\ToggleDiv.js*
- *Scripts\Common\ToggleImg.js*
- *Scripts\Common\Sub1\ToggleLinks.js*

![dir imag](bundling-and-minification/_static/image7.png)

Následující tabulka uvádí soubory přidané do sady pomocí zástupného, jak je znázorněno:

| **Volání** | **Přidat soubory nebo byla vyvolána výjimka** |
| --- | --- |
| Include("~/Scripts/Common/\*.js") | *AddAltToImg.js, ToggleDiv.js, ToggleImg.js* |
| Include("~/Scripts/Common/T\*.js") | Neplatný vzor výjimka. Zástupný znak je povolena pouze u předponu nebo příponu. |
| Include("~/Scripts/Common/\*og.\*") | Neplatný vzor výjimka. Je povolen pouze jeden zástupný znak. |
| "Include("~/Scripts/Common/T\*") | *ToggleDiv.js, ToggleImg.js* |
| "Include("~/Scripts/Common/\*") | Neplatný vzor výjimka. Segment čistý zástupný znak není platný. |
| IncludeDirectory ("~/Scripts/Common", "T\*") | *ToggleDiv.js, ToggleImg.js* |
| IncludeDirectory ("~/Scripts/Common", "T\*", true) | *ToggleDiv.js, ToggleImg.js, ToggleLinks.js* |

Explicitně přidání každého souboru do sady je obecně upřednostňované přes zástupný znak načítání souborů z následujících důvodů:

- Probíhá přidávání skriptů pomocí zástupných znaků výchozí hodnoty k jejich načtení v abecedním pořadí, což je obvykle není co chcete použít. Soubory šablon stylů CSS a JavaScript je často nutné přidat v určitém pořadí (neabecední). Toto riziko zmírnit přidáním vlastní [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) implementace, ale explicitně přidání každý soubor je menší chyba náchylné k chybám. Například můžete přidat nové prostředky do složky v budoucnosti, které může vyžadovat, abyste upravili vaše [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) implementace.
- Zobrazení přidat do adresáře pomocí zástupný znak načítání konkrétní soubory můžou být součástí všech zobrazení odkazující na této sady. Je-li zobrazit konkrétní skript přidá do sady, může dojít k chybě jazyka JavaScript na jiné zobrazení, které odkazují na sadu.
- Soubory šablon stylů CSS, které importovat další soubory za následek importovaných souborů načíst dvakrát. Například následující kód vytvoří sadu s většina souborů šablon stylů CSS motiv uživatelského rozhraní jQuery načíst dvakrát. 

    [!code-csharp[Main](bundling-and-minification/samples/sample12.cs)]

  Selektor zástupný znak "\*.css" přináší do každého souboru CSS ve složce, včetně *Content\themes\base\jquery.ui.all.css* souboru. *Jquery.ui.all.css* soubor importuje soubory jiných šablon stylů CSS.

## <a name="bundle-caching"></a>Sady ukládání do mezipaměti

Sady nastavit hlavičku HTTP vyprší platnost jeden rok od vytvoření sady. Pokud přejdete na dříve zobrazenou stránku, ukazuje aplikaci Fiddler, které aplikace Internet Explorer neprovede podmíněného požadavku pro sadu, to znamená, že jsou žádné požadavky HTTP GET z prohlížeče IE pro sady a žádné HTTP 304 odpovědi ze serveru. Můžete vynutit IE vytvořte žádost na podmíněného pro každou sadu pomocí klávesy F5 (což vede k odpovědi HTTP 304 pro každého svazku). Úplnou aktualizaci můžete vynutit pomocí ^ F5 (výsledkem odpověď HTTP 200 pro každou sadu.)

Na následujícím obrázku **ukládání do mezipaměti** kartu podokna Fiddler odpovědi:

![ukládání do mezipaměti image Fiddler](bundling-and-minification/_static/image8.png)

Požadavek   
`http://localhost/MvcBM_time/bundles/AllMyScripts?v=r0sLDicvP58AIXN_mc3QdyVvVj5euZNzdsa2N1PKvb81`  
 je pro sadu **AllMyScripts** a obsahuje pár řetězec dotazu **v = r0sLDicvP58AIXN\_mc3QdyVvVj5euZNzdsa2N1PKvb81**. Řetězec dotazu **v** má hodnotu token to znamená jedinečný identifikátor, který se používá pro ukládání do mezipaměti. Tak dlouho, dokud se nezmění. sada, bude požadovat aplikace ASP.NET **AllMyScripts** sady pomocí tohoto tokenu. Pokud se všechny soubory v sadě změní, rozhraní ASP.NET optimalizace vygeneruje nový token, která zaručí, že požadavky na prohlížeč pro sadu získáte nejnovější sady.

Pokud spuštění nástroje pro vývojáře F12 Internet Explorer 9 a přejít na dříve načíst stránku, IE nesprávně ukazuje podmíněného požadavky GET na každého svazku a serverem vrácení HTTP 304. Můžete si přečíst proč Internet Explorer 9 došlo k problémům se určení toho, jestli žádost podmíněného byl proveden v položce blogu [sítím pomocí CDN a Expires ke zlepšení výkonu webu](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

## <a name="less-coffeescript-scss-sass-bundling"></a>CoffeeScript, SCSS, Sass méně, sdružování.

Sdružování a minimalizace framework poskytuje mechanismus pro zpracování zprostředkující jazyky, jako je například [SCSS](http://sass-lang.com/), [Sass](http://sass-lang.com/), [menší](http://www.dotlesscss.org/) nebo [Coffeescript ](http://coffeescript.org/)a použít transformací například minimalizace výsledné sady. Chcete-li například přidat [.less](http://www.dotlesscss.org/) soubory do projektu MVC 4:

1. Vytvořte složku pro menší obsah. Následující příklad používá *Content\MyLess* složky.
2. Přidat [.less](http://www.dotlesscss.org/) balíček NuGet **bez tečky** do projektu.  
    ![Instalace bez tečky NuGet](bundling-and-minification/_static/image9.png)
3. Přidat třídu, která implementuje [IBundleTransform](https://msdn.microsoft.com/library/system.web.optimization.ibundletransform(VS.110).aspx) rozhraní. Pro transformaci .less přidejte následující kód do projektu.

    [!code-csharp[Main](bundling-and-minification/samples/sample13.cs)]
4. Vytvoření sady menší soubory se `LessTransform` a [CssMinify](https://msdn.microsoft.com/library/system.web.optimization.cssminify(VS.110).aspx) transformace. Přidejte následující kód, který `RegisterBundles` metoda v *aplikace\_Start\BundleConfig.cs* souboru.

    [!code-csharp[Main](bundling-and-minification/samples/sample14.cs)]
5. Přidejte následující kód do všech zobrazení, která odkazuje na sadu menší.

    [!code-cshtml[Main](bundling-and-minification/samples/sample15.cshtml)]

## <a name="bundle-considerations"></a>Důležité informace o sadě

Dobrý konvence dodržujte při vytváření sady je zahrnout "sady" jako předpony v názvu sady. To zabrání možného [směrování konflikt](https://forums.asp.net/post/5012037.aspx).

Jakmile aktualizujete jeden soubor v sadě, se vygeneruje nový token pro parametr řetězce dotazu sady a úplné sady je nutné stáhnout při příštím klient požádá o stránku obsahující sadu. V tradiční značky, kde je každý prostředek uvedeny jednotlivě by pouze změněný soubor stáhnout. Prostředky, které se často mění nemusí být vhodnými kandidáty pro sdružování.

Sdružování a minimalizace především zlepšit první čas načítání stránky požadavku. Po požádal webovou stránku v prohlížeči ukládá do mezipaměti prostředky (JavaScript, CSS a obrázků) tak, aby sdružování a minimalizace nebude poskytovat žádné zvýšení výkonu při žádosti o stejné stránku nebo stránky na stejné lokality požaduje stejné prostředky. Pokud není nastavený vyprší platnost záhlaví správně na vaše prostředky a nepoužíváte sdružování a minimalizace, heuristiky aktuálnosti prohlížečů bude označit prostředky zastaralé po několik dní. prohlížeč bude vyžadovat ověření žádosti pro každý prostředek. V takovém případě sdružování a minimalizace poskytnout zvýšení výkonu po první požadavek na stránku. Podrobnosti najdete v tématu na blogu [sítím pomocí CDN a Expires ke zlepšení výkonu webu](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

Omezení prohlížeče šesti souběžných připojení na každý název hostitele můžete zmírnit použitím [CDN](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx). Protože CDN bude mít jiný název hostitele než hostitelském webu, nebude asset požadavky od CDN započítává šesti limit souběžných připojení do hostitelského prostředí. Běžné ukládání do mezipaměti balíček a hraniční ukládání do mezipaměti výhody, může také poskytnout název CDN.

Sady by měl stránkami, které je třeba rozdělit na oddíly. Například výchozí šablony ASP.NET MVC pro aplikaci internet vytvoří sady ověřování jQuery odděleně od jQuery. Protože výchozí zobrazení, vytvořili jste žádný vstup a neprovede odeslání hodnoty, neobsahují se sady ověřování.

`System.Web.Optimization` Obor názvů je implementována ve System.Web.Optimization.DLL. Využívá knihovně WebGrease (WebGrease.dll) pro minimalizaci možnosti, které dále používá Antlr3.Runtime.dll.

*Používám služby Twitter a ujistěte se, rychlé příspěvcích sdílet odkazy. Moje Twitter popisovač*: [@RickAndMSFT](http://twitter.com/RickAndMSFT)

## <a name="additional-resources"></a>Další prostředky

- Video:[sdružování a optimalizace](https://channel9.msdn.com/Events/aspConf/aspConf/Bundling-and-Optimizing) podle [Howard Dierking](https://twitter.com/#!/howard_dierking)
- [Přidání optimalizaci webů k lokalitě webové stránky](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- [Přidání sdružování a minimalizace pro webové formuláře](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).
- [Důsledky výkonu sdružování a minimalizace na procházení webu](https://blogs.msdn.com/b/henrikn/archive/2012/06/17/performance-implications-of-bundling-and-minification-on-http.aspx) podle [společnost Nielsen Petr F](http://en.wikipedia.org/wiki/Henrik_Frystyk_Nielsen) [@frystyk](https://twitter.com/frystyk)
- [Pomocí sítím CDN a jeho platnost vyprší ke zlepšení výkonu webu](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx) od Ricka Andersona [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)
- [Minimalizovat požadavku (časy odezvy)](https://developers.google.com/speed/docs/best-practices/rtt)

## <a name="contributors"></a>Contributors

- Ani Haovi společnosti
- [Howard Dierking](https://twitter.com/#!/howard_dierking)
- Diana LaRose
