---
uid: whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
title: 'Postupy: Přidání mobilních stránek do svých formulářů technologie ASP.NET nebo aplikace MVC | Microsoft Docs'
author: rick-anderson
description: Tento způsob k popisuje různé způsoby, jak sloužit stránky optimalizované pro mobilní zařízení z vaší webové formuláře ASP.NET nebo aplikace MVC a navrhne architektury a...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2011
ms.topic: article
ms.assetid: 3124f28e-cc32-418a-afe3-519fa56f4c36
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
msc.type: content
ms.openlocfilehash: a8358b91ca424f4f3e576057ab43d850081dda60
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "30898980"
---
<a name="how-to-add-mobile-pages-to-your-aspnet-web-forms--mvc-application"></a>Postupy: Přidání mobilních stránek do svých formulářů technologie ASP.NET nebo aplikace MVC
====================
> **Platí pro**
> 
> - Verze webových formulářů ASP.NET 4.0
> - ASP.NET MVC verze 3.0
> 
> **Shrnutí**
> 
> Tento způsob k popisuje různé způsoby, jak sloužit stránky optimalizované pro mobilní zařízení z vaší webové formuláře ASP.NET nebo aplikace MVC a navrhuje architektury a návrhu, které byste měli zvážit při cílení na širokou škálu zařízení. Tento dokument také vysvětluje, proč jsou nyní zastaralé ovládací prvky Mobile ASP.NET z technologie ASP.NET 2.0 až 3.5 a popisuje některé moderní alternativy.


## <a name="contents"></a>Obsah

- Přehled
- Možnosti architektury
- Zjišťování prohlížeče a zařízení
- Jak poskytnout stránek specifických pro mobilní aplikace webových formulářů ASP.NET
- Jak poskytnout stránek specifických pro mobilní aplikace ASP.NET MVC
- Další zdroje

Ukázky kódu ke stažení demonstraci tento dokument techniky pro webové formuláře ASP.NET a MVC naleznete v části [Mobile Apps & servery s technologií ASP.NET](https://docs.microsoft.com/aspnet/mobile/overview).

## <a name="overview"></a>Přehled

Mobilní zařízení – smartphony, funkce telefonů a tabletů – dál zvýší popularita jako prostředek pro přístup k webu. Pro mnoho vývojářům webů a zaměřené na konkrétní webový firmy to znamená, že je stále důležité zajistit skvělé možnosti procházení pro návštěvníky pomocí těchto zařízení.

### <a name="how-earlier-versions-of-aspnet-supported-mobile-browsers"></a>Jak starší verze ASP.NET podporované mobilních prohlížečů

Verze technologie ASP.NET 2.0 až 3.5 zahrnuté *ovládací prvky ASP.NET Mobile*: sadu ovládacích prvků serveru pro mobilní zařízení v *System.Web.Mobile.dll* sestavení a  *System.Web.UI.MobileControls* oboru názvů. Sestavení je stále součástí ASP.NET 4, ale je zastaralý. Vývojáři by měli migrovat do více moderní přístupy, jsou popsané v tomto dokumentu.

Důvod, proč ovládací prvky ASP.NET Mobile bylo označeno jako zastaralé je orientován jejich návrh kolem mobilních telefonů, které byly běžné kolem 2005 a starší. Ovládací prvky slouží především k vykreslení kódu WML nebo cHTML (namísto regulární HTML) pro službu WAP prohlížeče z této letopočtu. Ale WAP, WML a cHTML již nejsou relevantní pro většinu projektů, protože HTML se nyní staly všudypřítomný značek jazyk prohlížeče mobilních a desktopových agentem.

### <a name="the-challenges-of-supporting-mobile-devices-today"></a>Na výzvy dnes podporu mobilních zařízení

Přestože mobilní prohlížeče teď podporují téměř všech HTML, budete stále mít mnoho problémů při s cílem vytvořit skvělé mobilní procházení prostředí:

- ***Velikost obrazovky*** – mobilní zařízení se výrazně liší v podobě, a jejich obrazovky jsou často mnohem menší než monitorování klientů. Potřebujete tedy návrh úplně jiné stránky rozložení pro ně.
- ***Vstupní metody*** – některá zařízení mají klávesnice, některé mají styluses, ostatní používat dotykové ovládání. Musíte vzít v úvahu několik navigační mechanismy a data metody zadávání znaků.
- ***Dodržování standardů*** – mnoho mobilních prohlížečů nepodporují nejnovějších standardů HTML, CSS a JavaScript.
- ***Šířka pásma*** – výkon sítě mobilních dat se liší podle velkým a jsou některé koncovým uživatelům o tarifech, které se účtují podle megabajtech.

Neexistuje univerzálně řešení; vaše aplikace bude muset vypadají a chovají se odlišně podle zařízení, které jsou k ní přistupují. V závislosti na tom, jakou úroveň mobilní podpory chcete může to být větší výzvu pro vývojářům webů než někdy byl desktop "prohlížeče válek".

Vývojáři často původně blíží Podpora mobilních prohlížečů poprvé myslíte, že se že jedná pouze důležité možná podporovat nejnovější a nejvíce sofistikované smartphony (například Windows Phone 7, iPhone nebo Android), protože vývojáři často osobní vlastní, například zařízení. Ale levnější telefony jsou stále velmi oblíbenou a jejich vlastníky, je použít k procházení webu – hlavně v zemí, kde se snadněji získat než širokopásmové připojení mobilních telefonů. Vaši firmu muset rozhodnout, jaký rozsah zařízení, které podporují v úvahu svých pravděpodobně zákazníků. Pokud vytváříte online – Příručka pro zabezpečené ověřování hesla možnost stavu, můžete provést obchodní rozhodnutí pouze pro cíl pokročilé smartphony, že pokud vytváříte rezervace systém lístků pro filmu, pravděpodobně muset účet pro návštěvníky s méně výkonná funkce telefony.

## <a name="architectural-options"></a>Možnosti architektury

Před jsme se k určité technické informace o webových formulářů ASP.NET nebo MVC, vezměte na vědomí, že vývojáři webů obecně mají tři hlavní možnosti Podpora mobilních prohlížečů:

1. ***Nedělat nic –*** můžete jednoduše vytvořit standardní, orientovaný na ploše webovou aplikaci a spoléhají na mobilní prohlížeče pro vykreslení přijatelně. 

    - **Výhody**: je nejlevnější možnost implementovat a udržovat – žádné dodatečné fungovat.
    - **Nevýhodou**: poskytuje nejhorší činnost koncového uživatele: 

        - Nejnovější smartphony může vykreslení kódu HTML stejně dobře jako prohlížeč pro stolní počítač, ale budou uživatelé stále přinucení přiblížení a posuňte se vodorovně a svisle využívat obsah na malou obrazovku. Toto je daleko od optimální.
        - Starší zařízení a funkce telefony může selhat k vykreslení k spokojenosti vašeho kódu.
        - Dokonce i v nejnovější tablet zařízení (jehož obrazovky může mít stejnou velikost jako obrazovky přenosného počítače) platí pravidla různých interakce. Na základě touch vstup funguje nejlépe s větší tlačítka a odkazy šíření další od sebe a neexistuje žádný způsob, jak najeďte ukazatelem myši rozevírací nabídce.
2. ***Vyřešit problém na straně klienta* –** s pečlivě použití šablon stylů CSS a [postupném rozšiřování](http://en.wikipedia.org/wiki/Progressive_enhancement) můžete vytvořit značky, stylů a skripty, které se aktivují prohlížeče běží přizpůsobit. Například s [dotazy na média šablon stylů CSS 3](http://www.w3.org/TR/2010/CR-css3-mediaqueries-20100727/), můžete vytvořit rozložení více sloupců, které do jednoho sloupce rozložení se změní na zařízení, jehož obrazovky jsou užší než zvolenou prahovou hodnotu. 

    - **Výhody**: optimalizuje vykreslování pro určité zařízení používá, a to i pro neznámé budoucí zařízení podle ať obrazovky a vstupní vlastnosti mají
    - **Výhody**: umožňuje snadné sdílení všech typů zařízení – minimální duplicitních kód nebo úsilí logiku na straně serveru
    - **Nevýhodou**: mobilní zařízení se tak liší od klientů zařízení může Opravdu chcete svoje mobilní stránky být zcela liší od vaší ploše stránky zobrazující různé informace. Takovéto změny může být neefektivní nebo možné dosáhnout robustní prostřednictvím šablon stylů CSS samostatně, zejména vzhledem k tomu, jak nekonzistentně starší zařízení interpretace pravidla šablon stylů CSS. To platí hlavně šablon stylů CSS 3 atributů.
    - **Nevýhodou**: poskytuje žádná podpora pro různých logiku na straně serveru a pracovní postupy pro různá zařízení. Zjednodušená nákupní košík najdete v článku věnovaném pracovního postupu pro mobilní uživatele nelze, například implementovat prostřednictvím šablon stylů CSS samostatně.
    - **Nevýhodou**: využití neefektivní šířky pásma. Server je pravděpodobně nutné přenášet značek a stylů, která se týkají všech možných zařízení a to i v případě, že cílové zařízení se použije pouze podmnožinu těchto informací.
3. ***Vyřešit problém na serveru* –** Pokud váš server zná, jaké zařízení přistupuje k jeho – nebo minimálně charakteristiky těchto zařízení, například jeho velikost obrazovky a IME, a jestli je mobilní zařízení – může být spuštěn jiný logiku a výstup různý kód HTML. 

    - **Výhody:** maximální flexibilitu. Neexistuje žádné omezení kolik může lišit logika straně serveru pro mobilní telefony nebo optimalizaci váš kód pro konkrétní zařízení, požadované rozložení.
    - **Výhody:** využití efektivní šířky pásma. Potřebujete jenom přenášet značek a informací o stylu, které cílové zařízení bude používat.
    - **Nevýhodou:** někdy Vynutí opakování úsilí nebo kód (například, že můžete vytvořit podobné, ale poněkud liší kopie stránky webových formulářů nebo zobrazení MVC). Kde to možné, bude zohlednit na běžné logiku do základní vrstvy nebo služby, ale přesto některé části kód uživatelského rozhraní nebo značek pravděpodobně duplicitní a pak uchovávat paralelně.
    - **Nevýhodou:** zařízení zjištění není triviální. To vyžaduje seznamu nebo v databázi typů známé zařízení a jejich vlastnosti (které nemusí být vždy zcela aktuální) a není zaručena tak, aby přesně odpovídaly každého příchozího požadavku. Tento dokument popisuje některé možnosti a jejich nástrahy později.

K dosažení nejlepších výsledků, najít Většina vývojářů, že které potřebují k kombinovat možnosti (2) a (3). Drobné rozdíly stylových nejlepší sloučeny na straně klienta pomocí šablon stylů CSS nebo i JavaScript, zatímco hlavní rozdíly v datech a pracovního postupu a značek jsou nejvíce efektivně implementovat v kódu na straně serveru.

### <a name="this-paper-focuses-on-server-side-techniques"></a>Tento dokument se zaměřuje na straně serveru techniky

Vzhledem k tomu, že jsou oba především serverovou technologie webových formulářů ASP.NET a MVC, tento dokument white paper se soustředí na straně serveru technik, které umožňují vytvořit různých značek a logiku pro mobilní prohlížeče. Samozřejmě to můžete také kombinovat s libovolnou techniku klienta (například šablon stylů CSS 3 dotazy na média, vylepšení progresivního JavaScript), ale který je více řádu návrh webu než vývoji ASP.NET.

## <a name="browser-and-device-detection"></a>Zjišťování prohlížeče a zařízení

Klíče předpokladem pro všechny serverové techniky pro podporu mobilních zařízení je vědět, jaké zařízení se pomocí vaší návštěvníka. Ve skutečnosti ještě lepší než zároveň budete vědět, je znalost číslo výrobce a model zařízení *charakteristiky* zařízení. Vlastnosti mohou zahrnovat:

- Je mobilní zařízení?
- Vstupní metoda (myši nebo klávesnice, dotykového ovládání, klávesnici, pákový,...)
- Velikost obrazovky (fyzicky a v pixelech)
- Podporované formáty média a dat
- Atd.

Je lepší rozhodnutí na základě charakteristik než číslo modelu, protože pak budete lépe vybavena pro budoucí zařízení.

### <a name="using-aspnets-built-in-browser-detection-support"></a>Použití prostředí ASP. Podpora detekce prohlížeče integrované na NET

Webové formuláře ASP.NET a MVC vývojáři mohou okamžitě zjistit důležité charakteristiky hostujícími prohlížeče zkontrolováním vlastnosti *Request.Browser* objektu. Například v tématu

- Request.Browser.IsMobileDevice
- Request.Browser.MobileDeviceManufacturer, Request.Browser.MobileDeviceModel
- Request.Browser.ScreenPixelsWidth
- Request.Browser.SupportsXmlHttp
- ...a mnoho dalších

Na pozadí, platformu ASP.NET odpovídá příchozí *User-Agent* hlavičky protokolu HTTP (uživatelský Agent) proti regulárních výrazů v sadě souborů XML definice prohlížeče. Ve výchozím nastavení platforma obsahuje definice pro mnoho běžných mobilních zařízení a ostatní uživatelé, který si přejete rozpoznat můžete přidat vlastní soubory definicí prohlížeče. Další podrobnosti najdete v tématu stránce MSDN [ovládací prvky webového serveru technologie ASP.NET a možnosti prohlížeče](https://msdn.microsoft.com/library/x3k2ssx2.aspx).

### <a name="using-the-wurfl-device-database-via-51degreesmobi-foundation"></a>Použití databáze zařízení WURFL prostřednictvím 51Degrees.mobi Foundation

Při ASP. Když nemusí být dost NET pro prohlížeče integrované detekce podporu jsou dostatečné pro mnoho aplikací jsou dvě hlavní případy:

- ***Chcete rozpoznat nejnovější zařízení***(bez ruční vytvoření souborů definic prohlížeče pro ně). Všimněte si, že rozhraní .NET 4 prohlížeče definiční soubory nejsou dostatečně nová, aby Windows Phone 7, telefony Android, prohlížeči Opera Mobile prohlížeče nebo Ipady Apple.
- ***Potřebujete podrobnější informace o možnostech zařízení***. Budete muset vědět o vstupní metoda zařízení (například touch vs klávesnici) nebo jaký zvuk formáty prohlížeč podporuje. Tyto informace není k dispozici v standardní prohlížeče definičních souborech.

[ *Universal souboru prostředků bezdrátové* projektu (WURFL)](http://wurfl.sourceforge.net/) udržuje mnohem víc aktuální a podrobné informace o mobilních zařízeních nepoužívá ještě dnes.

Skvělé zprávy pro vývojáře .NET je tento ASP. Funkce zjišťování prohlížeče na NET je rozšiřitelný, takže je možné jej lze vyřešit tyto problémy můžete vylepšit. Například můžete přidat open source [ *51Degrees.mobi Foundation* ](https://github.com/51Degrees/dotNET-Device-Detection) knihovny do projektu. Je ASP.NET IHttpModule nebo prohlížeče možnosti zprostředkovatele (lze použít u aplikací webových formulářů a MVC), který přímo čte WURFL data a zachytí do ASP. NET pro prohlížeče integrované detekce mechanismus. Po instalaci modulu, *Request.Browser* bude najednou obsahovat mnohem přesnější a podrobné informace: bude správně rozpoznat množství zařízení výše a jejich možnosti (včetně seznamu Další funkce jako je například IME). Další podrobnosti naleznete v dokumentaci daného projektu.

## <a name="how-web-forms-applications-can-present-mobile-specific-pages"></a>Jak poskytnout stránek specifických pro mobilní aplikace webových formulářů

Ve výchozím nastavení zde je, jak úplně nová aplikace webových formulářů zobrazí na běžné mobilních zařízeních:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image1.png)

Je zřejmé ani rozložení vypadá velmi mobilní zařízení – tato stránka byla navržena pro velký, orientovaný na šířku monitorování, ne pro malou obrazovku orientované na výšku. Proto co lze provádět o něm?

Jak je popsáno výše v tomto dokumentu, přizpůsobit svoje stránky pro mobilní zařízení mnoha způsoby. Některé techniky jsou na serveru, ostatní spustit na straně klienta.

### <a name="creating-a-mobile-specific-master-page"></a>Vytvoření mobilní konkrétní stránky předlohy

V závislosti na požadavcích, bude pravděpodobně možné používat stejné webové formuláře pro všechny návštěvníky, ale, abyste měli dva samostatné stránky předlohy: jednu pro stolní návštěvníky, druhý pro mobilní návštěvníky. To vám dává možnost změny šablony stylů CSS nebo vaši nejvyšší úrovně značka jazyka HTML, aby vyhovovala mobilní zařízení, bez nutnosti duplikovat veškeré logiky stránky.

Toto je snadné provést. Můžete třeba, přidejte obslužnou rutinu PreInit například následující webového formuláře:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample1.cs)]

Teď vytvořte hlavní stránku s názvem Mobile.Master ve složce nejvyšší úrovně vaší aplikace a použije se při zjištění mobilních zařízení. Vaše stránky předlohy můžete odkazovat šablonu stylů CSS specifické pro mobilní, v případě potřeby. Plochy návštěvníky se pořád zobrazí výchozí stránku předlohy, není ta mobilní.

### <a name="creating-independent-mobile-specific-web-forms"></a>Vytváření nezávislé specifické mobilní webové formuláře

Flexibilní můžete přejít mnohem další než jenom s samostatné stránky předlohy pro různé typy zařízení. Můžete implementovat dvě *zcela samostatné sady stránky webových formulářů* – jeden nastavit u stolních počítačů, jiné nastavení pro mobilní telefony. Tato metoda funguje nejlépe, pokud chcete nabízet velmi různé informace nebo pracovní postupy pro mobilní návštěvníky. Zbývající část tohoto oddílu popisuje tento postup podrobně.

Za předpokladu, že již máte aplikaci webové formuláře navržené pro stolních počítačů, je vytvoření podsložce s názvem "Mobilní" v rámci projektu a sestavení svoje mobilní stránky nejjednodušší způsob, jak pokračovat. Dílčí celý web, s vlastní stránky předlohy, šablony stylů a stránky, můžete vytvořit pomocí stejné techniky, které byste použili pro žádnou jinou aplikaci webových formulářů. K vytvoření mobilní ekvivalentní pro nepotřebujete nutně *každých* stránka ve vaší lokalitě plochy; můžete zvolit, jaké podmnožinu funkcí dává smysl pro mobilní návštěvníky.

Mobilních stránek můžete sdílet běžné statické prostředky (například obrázky, JavaScript nebo šablon stylů CSS soubory) s svoje regulární stránky, pokud chcete. Vzhledem k tomu, že bude vaší složky "Mobilní" *není* označit jako samostatné aplikace při hostovaný ve službě IIS (je stejně jednoduché podsložky stránky webových formulářů), ji budou také sdílet stejné konfiguraci, data relace a další infrastrukturou jako vaše plochy stránky.

> [!NOTE]
> Vzhledem k tomu, že tento postup obvykle zahrnuje některé duplikace kódu (mobilních stránek jsou pravděpodobně sdílet některé podobnosti s plochy stránek), je důležité faktor se všechny běžné obchodní logiky nebo data přístupový kód do sdílené základní vrstvy nebo služby. V ostatních případech budete double úsilí při vytváření a údržba vaší aplikace.


#### <a name="redirecting-mobile-visitors-to-your-mobile-pages"></a>Přesměrování mobilní návštěvníkům mobilních stránek

Často je vhodné pro přesměrování mobilní návštěvníkům mobilních stránek pouze na *první* požadavek v jejich procházení relace (a ne na každého požadavku v jejich relace), protože:

- Potom můžete snadno povolit mobilní návštěvníky pro přístup k vaší ploše stránky, pokud si přejí – stačí vložit odkaz na hlavní stránce, která přejde na "Desktop verze". Návštěvník nebude přesměrováni zpět na stránku mobilní, protože je již na první požadavky v jejich relace.
- Tím eliminujete odesílání riziko zasahovala do činnosti žádostí pro dynamické zdroje sdílené mezi desktop a mobile části serveru (např. Pokud máte běžné webový formulář, který desktop a mobile části serveru zobrazit v elementu IFRAME nebo určité obslužné rutiny Ajax)

K tomuto účelu můžete umístit logika přesměrování v **relace\_spustit** metoda. Například přidejte následující metodu do souboru Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample2.cs)]

#### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Konfigurace ověřování pomocí formulářů dodržovat mobilních stránek

Všimněte si, že ověřování pomocí formulářů určitých předpokladů o kde ji můžete během a po dokončení procesu ověřování přesměrování návštěvníky:

- Když uživatel potřebuje k ověření, ověřování pomocí formulářů je bude přesměrování na stránku desktopovém přihlášení, bez ohledu na to, jestli jsou desktop či mobile uživatele (protože má jenom koncept *jeden* přihlašovací adresa URL). Za předpokladu, že chcete jinak styl mobilní přihlašovací stránku, musíte k vylepšení plochy přihlašovací stránku tak, aby ho mobilní uživatele přesměruje na samostatné mobilní přihlašovací stránku. Například přidejte následující kód do vaší **plochy** přihlašovací stránky kódu: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample3.cs)]
- Poté, co uživatel úspěšně přihlásí, ověřování pomocí formulářů bude ve výchozím nastavení přesměrování je na ploše domovskou stránku (protože má jenom koncept *jeden* výchozí stránky). Je potřeba zvýšit mobilní přihlašovací stránku tak, aby je přesměrován na mobilní domovskou stránku po úspěšném přihlášení. Například přidejte následující kód do vaší **mobilní** přihlašovací stránky kódu: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample4.cs)]
  
  Tento kód předpokládá, že stránka má ovládací prvek serveru přihlášení názvem LoginUser, jako výchozí šablona projektu.

### <a name="working-with-output-caching"></a>Práce s ukládání výstupu do mezipaměti

Pokud používáte ukládání výstupu do mezipaměti, mějte na paměti, že ve výchozím nastavení je možné pro plochy uživatele, aby navštívil některé adresy URL (což její výstup do mezipaměti), následuje mobilní uživatele, který pak obdrží plochy výstup z mezipaměti. Toto upozornění se použije, zda je právě různých stránky předlohy podle typu zařízení, nebo zda implementace zcela samostatné webové formuláře podle typu zařízení.

Se tomuto problému vyhnout, můžete určit, aby ASP.NET k odlišení položky mezipaměti na základě jestli návštěvníka používá mobilních zařízení. Přidejte parametr VaryByCustom deklarace OutputCache vaší stránky následujícím způsobem:

[!code-aspx[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample5.aspx)]

V dalším kroku definovat *isMobileDevice* jako vlastní mezipaměti přepsat parametr přidáním následující metodu do souboru Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample6.cs)]

Tím bude zajištěno, že mobilní návštěvníky na stránku neobdrželi dříve umístí do mezipaměti klientů návštěvníkem výstup.

### <a name="a-working-example"></a>Příklad práce

Pokud chcete zobrazit tyto postupy v akci, stáhněte si [ukázky kódu tento dokument](https://docs.microsoft.com/aspnet/mobile/overview). Ukázková aplikace webových formulářů automaticky přesměruje mobilní uživatele na sadu stránek specifických pro mobilní v podsložce s názvem Mobile. Značky a stylů tyto stránek je lépe optimalizovaná pro mobilní prohlížeče, jak je vidět na následujících snímcích obrazovky:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image2.png)

Další tipy k optimalizaci značek a šablon stylů CSS pro mobilní prohlížeče najdete v části "Stylů mobilních stránek pro mobilní prohlížeče" dál v tomto dokumentu.

## <a name="how-aspnet-mvc-applications-can-present-mobile-specific-pages"></a>Jak poskytnout stránek specifických pro mobilní aplikace ASP.NET MVC

Vzhledem k tomu, že vzoru Model-View-Controller oddělí aplikace logiky (v řadiče) z logiky prezentace (v zobrazení), můžete z jakéhokoli z následujících dvou přístupů zpracování mobilní podpory v kódu na straně serveru:

1. ***Použít stejnou řadiče a zobrazení pro stolní počítače a mobilní prohlížeče, ale vykreslit zobrazení s jinou Razor rozložení v závislosti na typu zařízení *.** Tato možnost funguje nejlíp, pokud jste zobrazení identické dat na všech zařízeních, ale jednoduše chcete zadat jinou šablony stylů CSS nebo změnit několik nejvyšší úrovně elementů HTML pro mobilní telefony.
2. ***Použít stejnou řadiče pro stolní počítače a mobilní prohlížeče, ale vykreslení různá zobrazení v závislosti na typu zařízení***. Tato možnost funguje nejlíp, pokud jste zobrazení zhruba stejná data a poskytuje stejné pracovních postupů pro koncové uživatele, ale chcete vykreslit velmi různý kód HTML tak, aby vyhovovala zařízení používá.
3. ***Vytvoření samostatné oblasti pro stolní počítače a mobilní prohlížeče, implementace nezávislé kontrolery a zobrazení pro každý *.** Tato možnost funguje nejlíp, pokud jste zobrazení obrazovky příliš neliší, obsahující různé informace a úvodní uživatele prostřednictvím různých pracovních optimalizované pro jejich typu. Může to znamenat některé opakování kódu, ale můžete minimalizovat, pomocí řešení se běžné logiku do podkladové vrstvy nebo služby.

Pokud budete chtít využít **první** možnost a pouze Razor rozložení se liší podle typu zařízení, je velmi snadné. Právě upravit vaše \_ViewStart.cshtml souboru následujícím způsobem:

[!code-cshtml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample7.cshtml)]

Nyní můžete vytvořit konkrétní mobilní rozložení názvem \_LayoutMobile.cshtml se strukturou stránky a šablon stylů CSS pravidla optimalizované pro mobilní zařízení.

Pokud budete chtít využít **druhý** možnost vykreslení uvidíte úplně jiné zobrazení v závislosti na typu zařízení návštěvníka, najdete v části [Scott Hanselman příspěvku na blogu](http://www.hanselman.com/blog/ABetterASPNETMVCMobileDeviceCapabilitiesViewEngine.aspx).

Zbytek tohoto dokumentu se zaměřuje na **třetí** možnost – vytvoření samostatné řadičů *a* zobrazení pro mobilní zařízení – tak můžete řídit přesně se nabízí jaké podmnožinu funkce pro mobilní návštěvníky.

### <a name="setting-up-a-mobile-area-within-your-aspnet-mvc-application"></a>Nastavení mobilních oblasti v rámci vaší aplikace ASP.NET MVC

Můžete přidat oblast s názvem "Mobilní" do existující aplikace ASP.NET MVC běžným způsobem: klikněte pravým tlačítkem na název projektu v Průzkumníku řešení, a potom vyberte ve přidat oblast. Poté můžete přidat kontrolery a zobrazení jako u jiných oblasti v aplikaci ASP.NET MVC. Například přidáte do vaší mobilní oblasti nový řadič názvem HomeController tak, aby fungoval jako domovskou stránku pro mobilní návštěvníky.

### <a name="ensuring-the-url-mobile-reaches-the-mobile-homepage"></a>Zajištění URL /Mobile dosáhne mobilní domovské stránky

Pokud chcete /Mobile adresu URL k dosažení akce indexu na HomeController uvnitř vaší mobilní oblasti, musíte provést dvě malé změny konfigurace služby směrování. Nejdřív aktualizujte třídě MobileAreaRegistration tak, aby HomeController je výchozí v oblasti systému mobilní, jak je znázorněno v následujícím kódu:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample8.cs)]

To znamená mobilní domovské stránce teď budou umístěné na /Mobile, nikoli/Mobile/Domů, protože "Domů" je teď výchozí název řadiče implicitně pro mobilních stránek.

Dále si všimněte, že přidáním druhý HomeController do vaší aplikace (tj, mobilní ten, kromě existující ploše jeden) jste budete zrušili regulární plochy domovské stránky. Se nezdaří s chybou "*více typů nebyly nalezeny odpovídající řadičem s názvem"Home"*". To vyřešit, aktualizujte konfiguraci nejvyšší úrovně směrování (v Global.asax.cs) k určení, že by vaše plochy HomeController po nejednoznačnosti mají přednost:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample9.cs)]

Teď chyba přejde tokeny a adresu URL http://<em>yoursite</em>/ dosáhnou plochy domovské stránky a http://<em>yoursite</em>/mobile/ dosáhnou mobilní domovské stránky.

### <a name="redirecting-mobile-visitors-to-your-mobile-area"></a>Přesměrování mobilní návštěvníkům vaší mobilní oblasti

Existuje mnoho různých rozšíření bodů v architektuře ASP.NET MVC, proto nejsou možné způsoby vložit logiku přesměrování. Úhledné jednou z možností je vytvořit atribut filtru, [RedirectMobileDevicesToMobileArea], který provádí přesměrování, pokud jsou splněny následující podmínky:

1. Jde o první požadavek v relaci uživatele (tj, Session.IsNewSession hodnotu PRAVDA)
2. Požadavek pochází z prohlížeč pro mobilní zařízení (tj, Request.Browser.IsMobileDevice hodnotu PRAVDA)
3. Uživatel není už žádají o prostředku v oblasti mobilních (tj, *cesta* část adresy URL nezačíná /Mobile)

Ke stažení ukázkové součástí tento dokument white paper zahrnuje implementace této logiky. Je implementovaný jako filtr autorizace, který je odvozen od třídy AuthorizeAttribute, což znamená, že je správně fungovaly i v případě, že používáte ukládání výstupu do mezipaměti (jinak, pokud první přístupů plochy návštěvníka některé adresy URL, plochy výstupu mohou být uložené v mezipaměti a pak dodávat do následné mobilní návštěvníky).

Protože je filtr, můžete zvolit buď jej použít na konkrétní kontrolerů a akcí, například

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample10.cs)]

… nebo můžete použít jej pro všechny kontrolery a akce jako MVC 3 *globálních filtrů* v souboru Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample11.cs)]

Ke stažení ukázkové také ukazuje, jak můžete vytvořit podtříd tento atribut přesměrovat na konkrétní umístění v rámci vaší mobilní oblasti. To znamená, například že můžete:

- Registraci globálního filtru jak je uvedené výše, odešle mobilní návštěvníky na mobilní domovskou stránku ve výchozím nastavení.
- Také použijte speciální filtr [RedirectMobileDevicesToMobileProductPage] "zobrazení produkt" akci, která přebírá mobilní návštěvníkům mobilní verzi produktu stránka měl požadovali.
- Jiné speciální podtřídy filtru se rovněž vztahují na konkrétní akce, přesměrování mobilní návštěvníky na stránku ekvivalentní mobilní

### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Konfigurace ověřování pomocí formulářů dodržovat mobilních stránek

Pokud používáte ověřování pomocí formulářů, upozorňujeme ale, že když uživatel potřebuje k přihlášení, se automaticky přesměruje uživatele na jednu konkrétní "přihlášení" adresu URL, který ve výchozím nastavení je **/Account/přihlášení**. To znamená, že mobilní uživatelé mohou přesměrováni na akci plochy "přihlášení".

K tomuto problému nedošlo, přidejte logiku plochy akci "přihlášení" tak, aby znovu přesměrování mobilní uživatele na mobilní "přihlášení" akce. Pokud používáte výchozí šablony aplikace ASP.NET MVC, aktualizujte akce AccountController na přihlášení následujícím způsobem:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample12.cs)]

… a potom implementovat vhodný účet specifické mobile "přihlášení" akce na řadiči názvem AccountController ve vaší mobilní oblasti.

### <a name="working-with-output-caching"></a>Práce s ukládání výstupu do mezipaměti

Pokud používáte filtr [OutputCache], je nutné vynutit položky mezipaměti na mění podle typu zařízení. Například se zapisuje:

[!code-javascript[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample13.js)]

Pak přidejte následující metodu do souboru Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample14.cs)]

Tím bude zajištěno, že mobilní návštěvníky na stránku neobdrželi dříve umístí do mezipaměti klientů návštěvníkem výstup.

### <a name="a-working-example"></a>Příklad práce

Pokud chcete zobrazit tyto postupy v akci, stáhněte si [tento dokument kódu přidružené ukázky](https://docs.microsoft.com/aspnet/mobile/overview). Ukázka obsahuje aplikaci ASP.NET MVC 3 (Release Candidate) rozšířené k podpoře mobilních zařízení pomocí metody popsané výše.

## <a name="further-guidance-and-suggestions"></a>Další pokyny a doporučení

Následující informace platí jak pro webové formuláře a MVC vývojáři, kteří používají techniky zahrnuté v tomto dokumentu.

### <a name="enhancing-your-redirection-logic-using-51degreesmobi-foundation"></a>Rozšíření přesměrování logiky pomocí 51Degrees.mobi Foundation

Přesměrování logiku v tomto dokumentu, může být perfektně pro vaši aplikaci, ale nebude fungovat, pokud je nutné zakázat relace, nebo v mobilních prohlížečích, které odmítnout soubory cookie (tyto nemůže mít relace), protože nebude vědět, zda daný požadavek je první z nich z tohoto návštěvníka.

Již jste zjistili, jak otevřít zdroj 51Degrees.mobi Foundation může zlepšit přesnost ASP. Zjišťování na NET prohlížeče. Je také integrovanou schopnost přesměrování mobilní návštěvníky na konkrétní umístění nakonfigurované v souboru Web.config. Je možné pracovat bez v závislosti na relací ASP.NET (a tím i soubory cookie) uložením dočasné protokolu hodnoty hash návštěvníků hlavičky protokolu HTTP a IP adres tak bude vědět, jestli je každý požadavek první z nich z daného vistor.

Přidat do sekce fiftyOne v souboru web.config následující element bude přesměrovávat první požadavek zjištěné mobilních zařízení na stránce ~ / Mobile/Default.aspx. Budou všechny požadavky na stránky ve složce mobilní *není* přesměrovat, bez ohledu na typ zařízení. Pokud mobilní zařízení bylo nečinné po dobu 20 minut nebo další zařízení bude zapomenete a následné žádosti budou považovány za nové pro účely přesměrování.

[!code-xml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample15.xml)]

Další podrobnosti najdete v tématu [51degrees.mobi Foundation dokumentaci](https://github.com/51Degrees/dotNET-Device-Detection).

> [!NOTE]
> Můžete *můžete* Foundation 51Degrees.mobi použití funkce přesměrování na aplikace ASP.NET MVC, ale bude nutné definovat konfiguraci přesměrování z hlediska prostý adresy URL, není z hlediska směrování parametry nebo umístěním filtry MVC na akce. Důvodem je, že (v době psaní) 51Degrees.mobi Foundation nelze rozpoznat filtry nebo směrování.


### <a name="disabling-transcoders-and-proxy-servers"></a>Zakázání Transkodéry a Proxy servery

Operátory mobilní sítě mají dva široké cíle v jejich přístupu k mobilní síti internet.

1. Zadejte jako mnohem souvisejícího obsahu nejdříve
2. Co nejvíce zákazníci, kteří mohou sdílet přepínač omezenou šířku pásma sítě

Vzhledem k tomu, že většina webových stránek byly navrženy pro velké plochy proměnlivé velikosti obrazovky a rychlé pevné řádku širokopásmové připojení, použijte řada operátorů *transkodéry* nebo *proxy servery* který dynamicky upravit obsah webového serveru. Se může změnit kód HTML nebo šablon stylů CSS tak, aby vyhovovala menších obrazovkách (hlavně u "funkce telefonů" která nemají výpočetní výkon zpracování komplexní rozložení) a jejich znovu komprimovat obrázků (výrazně snižuje jejich kvality) k vylepšení rychlostmi doručení stránky.

Ale pokud jste prováděné úsilí nezbytné k vytvoření optimalizované mobilní verzi vaší lokality, pravděpodobně nebudete chtít, operátora mobilní sítě narušoval ho žádné další. Můžete přidat následující řádek na stránku\_zatížení událost v jakékoli webové formuláře ASP.NET:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample16.cs)]

Nebo pro kontroler ASP.NET MVC, můžete přidat následující přepsání metody tak, aby se vztahuje na všechny akce v kontroleru:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample17.cs)]

Výsledný zprávy HTTP informuje W3C kompatibilní transkodéry a proxy servery nemění obsah. Samozřejmě není zaručeno, že mobilní sítě operátory respektuje tuto zprávu.

### <a name="styling-mobile-pages-for-mobile-browsers"></a>Práce se styly mobilních stránek pro mobilní prohlížeče

Je nad rámec tohoto dokumentu příliš podrobně popisují, jaké druhy pracovní značek HTML správně nebo které techniky návrhu webové maximalizovat použitelnost na konkrétní zařízení. Ho má až vám najít dostatečně jednoduché rozložení, optimalizované pro obrazovky mobile proměnlivé velikosti, bez použití nespolehlivé HTML nebo umístění triky šablony stylů CSS. Jedním z důležitých postupů důležité zmínit, je však *zobrazení metaznačku*.

Některé moderní mobilní prohlížeče v úsilí zobrazení webové stránky určené výhradně pro monitorování klientů, vykreslení stránky na virtuální plátno, označované taky jako "zobrazení" (například virtuální zobrazení je 980 pixelů na zařízení iPhone a 850 pixelů v prohlížeči Opera Mobile ve výchozím nastavení) a potom snižovat výsledek pro uložení na fyzické pixelů na obrazovce. Uživatel může potom přiblížení a posouvání zobrazení tohoto zobrazení. To má výhodu, že umožňuje, aby prohlížeč zobrazení stránky v jeho určený rozložení, ale je také má nevýhodou, vynutí si přibližování a posouvání, což je nepohodlná pro uživatele. Pokud vytváříte Mobile, je lepší návrhu úzké obrazovky tak, aby žádné přiblížení a oddálení nebo vodorovného posouvání je nezbytné.

Určit prohlížeč pro mobilní zařízení, jak široké by měla být zobrazení je prostřednictvím nestandardní *zobrazení* značka meta. Například přidejte následující části HEAD vaší stránky

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample18.html)]

… potom podpora prohlížečů smartphone bude rozložení stránky na plátně celý virtuální 480 pixelů. To znamená, že pokud vaše elementů HTML definovat jejich šířku v procentech, procenta interpretovat s ohledem na tato šířka 480 pixelů, není šířka výchozí zobrazení. V důsledku toho je méně pravděpodobné, že máte přiblížení a posouvání vodorovně – podstatně zlepšení mobilní rozhraní prohlížeče uživatele.

Pokud chcete, aby šířka zobrazení tak, aby odpovídala fyzické pixelů zařízení, můžete zadat následující:

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample19.html)]

Tento postup fungovat správně, nesmí vynutíte explicitně elementy delší než tento šířka (například pomocí *šířka* atribut nebo vlastnost CSS), jinak se vynutí prohlížeči používat větší zobrazení bez ohledu na to. Viz také: [další podrobnosti o značce nestandardní zobrazení](https://developer.apple.com/library/safari/#documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html).

Většina moderních smartphony podporu *duální orientaci*: můžete možné uchovávat v režimu na výšku nebo na šířku. Ano je důležité, abyste Zkontrolujte předpoklady o obrazovky šířku v pixelech. I Nepředpokládejte, že šířku obrazovky odstraněna, protože uživatel může znovu orientaci jejich zařízení, době, kdy jsou na stránce.

Starší zařízení Windows Mobile a Blackberry může také přijímat těchto značek meta v záhlaví stránky k informování obsahu je optimalizovaná pro mobilní a proto by nemělo být transformaci.

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample20.html)]

## <a name="additional-resources"></a>Další prostředky

Seznam mobilních zařízení emulátorů a simulátorů, můžete použít k testování mobilní webové aplikace ASP.NET naleznete na stránce [simulovat oblíbených mobilních zařízení pro testování](../mobile/device-simulators.md).

## <a name="credits"></a>Kredity

- Primárního autora: Steven Sanderson
- Kontroloři / další obsahu zapisovače: James Rosewell, Mikael Söderström, Scott Hanselman, Scott Hunter
