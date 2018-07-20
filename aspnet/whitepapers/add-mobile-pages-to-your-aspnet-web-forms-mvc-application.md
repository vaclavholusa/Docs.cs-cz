---
uid: whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
title: 'Postupy: Přidání mobilních stránek do webových formulářů nebo aplikace MVC | Dokumentace Microsoftu'
author: rick-anderson
description: Tento postup popisuje různé způsoby, jak poskytovat stránky optimalizované pro mobilní zařízení z webových formulářů ASP.NET / aplikace MVC a navrhne architektury a...
ms.author: aspnetcontent
ms.date: 01/20/2011
ms.assetid: 3124f28e-cc32-418a-afe3-519fa56f4c36
msc.legacyurl: /whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
msc.type: content
ms.openlocfilehash: 59b81184852a7fe0ad2dcad9718b572a8c756918
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823352"
---
<a name="how-to-add-mobile-pages-to-your-aspnet-web-forms--mvc-application"></a>Postupy: Přidání mobilních stránek do webových formulářů nebo aplikace MVC
====================
> **Platí pro**
> 
> - Verze webových formulářů technologie ASP.NET 4.0
> - ASP.NET MVC verze 3.0
> 
> **Shrnutí**
> 
> Tento postup popisuje různé způsoby, jak poskytovat stránky optimalizované pro mobilní zařízení z webových formulářů ASP.NET / aplikace MVC a navrhne architektury a návrhu problémy, které je třeba zvážit při cílení na široké škále zařízení. Tento dokument popisuje taky, proč jsou nyní zastaralé technologie ASP.NET Mobile ovládací prvky technologie ASP.NET 2.0 3.5 a tento článek popisuje některé moderní alternativy.


## <a name="contents"></a>Obsah

- Přehled
- Možnosti architektury
- Zjišťování prohlížeče a zařízení
- Jak aplikace webových formulářů ASP.NET sebou může nést stránek specifických pro mobilní zařízení
- Jak aplikace ASP.NET MVC sebou může nést stránek specifických pro mobilní zařízení
- Další zdroje

Ukázky ke stažení kódu demonstrace techniky dokument white paper pro webové formuláře ASP.NET a MVC najdete v tématu [Mobile Apps & servery s technologií ASP.NET](https://docs.microsoft.com/aspnet/mobile/overview).

## <a name="overview"></a>Přehled

Mobilní zařízení – smartphony, funkce telefonů a tabletů – i nadále poroste popularita jako způsob pro přístup k webu. Pro mnoho webovým vývojářům a objektově orientovaný firmám to znamená, že je stále potřeba zajistit skvělé možnosti procházení pro návštěvníky pomocí těchto zařízení.

### <a name="how-earlier-versions-of-aspnet-supported-mobile-browsers"></a>Jak starší verze technologie ASP.NET podporovány mobilních prohlížečů

Technologie ASP.NET verze 2.0 na 3.5 zahrnuté *mobilní ovládací prvky ASP.NET*: sady serverových ovládacích prvků pro mobilní zařízení v *System.Web.Mobile.dll* sestavení a  *System.Web.UI.MobileControls* oboru názvů. Sestavení je zahrnutá v technologii ASP.NET 4, ale je zastaralé. Vývojářům doporučujeme migrovat na Modernější přístupy, například těch popsaných v tomto dokumentu.

Důvod, proč mobilní ovládací prvky technologie ASP.NET jsou označené jako zastaralé je, že jejich návrhu je orientovaný kolem mobilních telefonů, které bylo běžné kolem 2005 a starší. Ovládací prvky slouží především k vykreslení WML nebo cHTML kódu (místo regulární HTML) pro prohlížeče WAP tohoto období. Ale WAP, WML a cHTML již nejsou relevantní u většiny projektů, protože HTML se teď stal všudypřítomná značkovací jazyk pro mobilních a desktopových prohlížeče nabídne.

### <a name="the-challenges-of-supporting-mobile-devices-today"></a>Obtíže spojené s podporu mobilních zařízení ještě dnes

Přestože mobilní prohlížeče teď podporují téměř univerzálně HTML, budete stále mít řadu výzev při cílem vytvářet skvělé mobilní prostředí procházení:

- ***Velikost obrazovky*** – mobilní zařízení se výrazně liší ve formuláři a jejich obrazovky jsou často mnohem menší, než monitorování klientů. Ano budete muset navrhnout zcela jinou stránku rozložení pro ně.
- ***Vstupní metody*** – některá zařízení mít klávesnice, některé mají styluses, ostatní používat dotykové ovládání. Musíte vzít v úvahu několik navigace mechanismy a data vstupní metody.
- ***Dodržování standardů*** – mnoho mobilních prohlížečů nepodporují nejnovějších standardů HTML, šablon stylů CSS a JavaScript.
- ***Šířka pásma*** – mobilní datové síti se mění výkon zvýšením nečekaně a některé koncovým uživatelům se na sazby, které se účtují podle megabajtech.

Neexistuje žádné poskytovat řešení univerzální velikosti; vaše aplikace bude mít vypadat a chovat jinak v závislosti na zařízení přístup. V závislosti na tom, jaké úroveň podpory mobilních chcete může to být větší výzvu pro webové vývojáře než desktop "prohlížeče wars" bylo dříve.

Vývojáři často zpočátku blíží Podpora mobilních prohlížečů pro první myslíte, že je pouze potřeba podporovat nejnovější a nejvyspělejším smartphony (například Windows Phone 7, iPhone nebo Android), možná, protože vývojáři často sami vlastní zařízení. Ale levnější telefony jsou stále velmi oblíbenou a jejich vlastníky použít k procházení webu – zejména v zemích, kde jsou mobilní telefony jednodušší než širokopásmové připojení. Vaše podnikání muset rozhodnout, jaké škálu zařízení, které podporují zvýšením jeho pravděpodobně zákazníků. Pokud vytváříte online – Příručka pro spa stavu luxusní, můžete provést obchodní rozhodnutí jenom na cílové pokročilé smartphony, že pokud vytváříte rezervace systém lístků filmové, budete pravděpodobně muset počítat návštěvníky s méně výkonných funkcí telefony.

## <a name="architectural-options"></a>Možnosti architektury

Předtím, než se dostaneme k určité technické podrobnosti technologie ASP.NET webové formuláře nebo MVC, Všimněte si, že vývojáři webů obecně tři hlavní možnosti Podpora mobilních prohlížečů:

1. ***Neprovádět žádnou akci –*** můžete jednoduše vytvořit standardní, orientovaných na ploše webovou aplikaci a Spolehněte se na mobilní prohlížeče k vykreslení přijatelně. 

    - **Výhody**: je nejlevnější možnost implementovat a udržovat – bez dalších fungovat.
    - **Nevýhodou**: poskytuje nejhorší činnost koncového uživatele: 

        - Nejnovější smartphony může mít za následek kódu HTML stejně, jako prohlížeč pro stolní počítač, ale uživatelé se vynutí stále přiblížení a posuňte se vodorovně a svisle využívání obsahu na malé obrazovce. Toto je daleko od optimální.
        - Vykreslit váš kód k spokojenosti starší zařízení a funkce telefony nemusí podařit.
        - Dokonce i na nejnovějších tablet zařízení (jehož obrazovky může být velikosti obrazovky přenosných počítačů) platí pravidla různé interakce. Vstup s dotykovým ovládáním funguje nejlépe s větší tlačítka a propojí šíření další od sebe a neexistuje žádný způsob, jak kurzorem myši nad rozevírací nabídku.
2. ***Vyřešit problém na straně klienta* –** opatrní použití šablon stylů CSS a [postupném rozšiřování](http://en.wikipedia.org/wiki/Progressive_enhancement) můžete vytvářet značky, stylů a skripty, které přizpůsobit jakýkoli prohlížeč běží. Třeba index Mei [dotazy na média šablon stylů CSS 3](http://www.w3.org/TR/2010/CR-css3-mediaqueries-20100727/), můžete vytvořit více sloupci rozložení, které se změní rozložení jeden sloupec na zařízení, jehož obrazovky jsou užší než zvolená prahová hodnota. 

    - **Výhody**: optimalizuje pro určité zařízení používá, a to i pro neznámý budoucí zařízení podle libovolné obrazovce a vstupní vlastnosti mají vykreslování
    - **Výhody**: umožňuje snadno sdílet logiku na straně serveru ve všech typech zařízení – minimální duplicity kódu nebo úsilí
    - **Nevýhodou**: mobilní zařízení se tak liší od desktopové zařízení, může Opravdu chcete vašich mobilních stránek být zcela liší od plochy stránky, zobrazuje různé informace. Takové změny může být neefektivní nebo nemožné robustně dosáhnout prostřednictvím šablon stylů CSS samostatně, zejména vzhledem k tomu, jak nekonzistentně starší zařízení interpretace pravidel šablon stylů CSS. To platí zejména atributů šablony stylů CSS 3.
    - **Nevýhodou**: poskytuje bez podpory pro různé logiku na straně serveru a pracovní postupy pro různá zařízení. Zjednodušené nákupního košíku checkout pracovního postupu pro mobilní uživatele nelze, například implementovat pomocí šablon stylů CSS samostatně.
    - **Nevýhodou**: využití šířky pásma neefektivní. Server je pravděpodobně nutné přenášet značky a stylů, které platí pro všechna možná zařízení, i v případě, že cílové zařízení se použije pouze podmnožinu těchto informací.
3. ***Vyřešit problém na serveru* –** Pokud váš server ví, co zařízení přistupuje – nebo nejméně charakteristiky těchto zařízení, například jeho velikosti obrazovky a vstupní metodu, a zda je na mobilním zařízení – lze spustit různé logiky a výstup různý kód HTML. 

    - **Výhodou:** maximální flexibilitu. Neexistuje žádné omezení, kolik můžete měnit svoji logiku na straně serveru pro mobilní telefony nebo optimalizovat vaše značky pro konkrétní zařízení, požadované rozložení.
    - **Výhodou:** využití šířky pásma efektivní. Stačí přenášet značky a informací o stylu, na který budete používat cílové zařízení.
    - **Nevýhodou:** někdy Vynutí opakování úsilí nebo kód (například provedení je podobný, ale mírně odlišné kopie stránky webových formulářů nebo zobrazení MVC). Pokud to možné, budou faktor si běžné logiky do nadřízené vrstvy nebo služby, ale stále, některé části uživatelského rozhraní kód nebo značky pravděpodobně nutné být duplicitní a pak udržuje paralelně.
    - **Nevýhodou:** zjišťování zařízení není nic snadného. Vyžaduje seznamu nebo v databázi známé zařízení typů a jejich vlastnosti (které nemusí být vždy zcela aktuální) a není zaručeno, že tak, aby přesně odpovídaly všechny příchozí požadavky. Tento dokument popisuje některé možnosti a jejich nástrahy později.

Pokud chcete získat nejlepší výsledky, Většina vývojářů najít že potřebují kombinovat možnosti (2) a (3). Drobné rozdíly stylistické nejlépe sloučeny na straně klienta pomocí šablon stylů CSS nebo dokonce JavaScript, že hlavní rozdíly v datech, pracovní postup nebo značky jsou nejvíce efektivně implementovat v kódu na straně serveru.

### <a name="this-paper-focuses-on-server-side-techniques"></a>Tento dokument se zaměřuje na metody na straně serveru

Protože webových formulářů ASP.NET a MVC jsou obě technologie primárně na serveru, tento dokument white paper se soustředí na straně serveru techniky, které umožňují vytvářet různých značek a logiku pro mobilní prohlížeče. Samozřejmě to můžete také kombinovat s libovolnou techniku na straně klienta (například šablon stylů CSS 3 dotazy na média, postupné vylepšení JavaScript), ale, což je více webových stránek než vývoj v technologii ASP.NET.

## <a name="browser-and-device-detection"></a>Zjišťování prohlížeče a zařízení

Klíče předpokladem pro všechny metody na straně serveru pro podporu mobilních zařízení je vědět, jaké zařízení používá návštěvník. Ve skutečnosti ještě lepší než znalost toho, výrobce a model číslo tohoto zařízení je vědět, *charakteristiky* zařízení. Vlastnosti mohou zahrnovat:

- Je mobilní zařízení?
- Vstupní metody (myši a klávesnice, dotykové ovládání, klávesnici, joystick,...)
- Velikost obrazovky (fyzicky a v pixelech)
- Podporované formáty média a data
- Atd.

Je lepší rozhodování na základě charakteristik než číslo modelu, protože pak bude lépe vybavena pro budoucí zařízení.

### <a name="using-aspnets-built-in-browser-detection-support"></a>Použití prostředí ASP. NET pro prohlížeče integrované detekce podpory

Webové formuláře ASP.NET a MVC vývojáře můžete ihned zjišťovat důležité charakteristiky hostujícími prohlížeče zkontrolováním vlastnosti *Request.Browser* objektu. Příklad naleznete v tématu

- Request.Browser.IsMobileDevice
- Request.Browser.MobileDeviceManufacturer, Request.Browser.MobileDeviceModel
- Request.Browser.ScreenPixelsWidth
- Request.Browser.SupportsXmlHttp
- ...a mnoho dalších

Na pozadí, platformu ASP.NET odpovídá příchozí *User-Agent* hlavičky protokolu HTTP (UA) proti regulárních výrazů v sadě souborů definice XML prohlížeče. Ve výchozím nastavení platforma obsahuje definice pro mnoho běžných mobilních zařízení a pro ostatní uživatele, který chcete rozpoznat můžete přidat vlastní soubory definice prohlížeče. Další podrobnosti najdete na stránce MSDN [serverových ovládacích prvků technologie ASP.NET a schopností prohlížeče](https://msdn.microsoft.com/library/x3k2ssx2.aspx).

### <a name="using-the-wurfl-device-database-via-51degreesmobi-foundation"></a>Pomocí databáze WURFL zařízení prostřednictvím 51Degrees.mobi Foundation

Zatímco ASP. NET pro prohlížeče integrované detekce podpora bude dostačující pro mnoho aplikací, existují dva hlavní případy ale nemusí být dostatečně:

- ***Rozpoznáte nejnovější zařízení***(bez ruční vytvoření prohlížeče definiční soubory pro ně). Všimněte si, že rozhraní .NET 4 prohlížeče definiční soubory nejsou dostatečně nová, aby rozpoznat Windows Phone 7, telefony s Androidem, prohlížeči Opera Mobile nebo zařízení Apple iPad.
- ***Potřebujete podrobnější informace o možnostech zařízení***. Budete muset vědět o zařízení s IME (například touch vs klávesnici), nebo jaké zvukové formáty prohlížeč podporuje. Tyto informace není k dispozici ve standardní soubory definice prohlížeče.

[ *Univerzální soubor prostředků bezdrátové* (WURFL) projektu](http://wurfl.sourceforge.net/) uchovává mnohem více aktuální a podrobné informace o mobilních zařízení používaných ještě dnes.

Dobré zprávy pro vývojáře na platformě .NET je prostředí ASP. NET pro funkce rozpoznávání prohlížeče je rozšiřitelný, takže je možné zvýšit na tyto problémy překonat. Například můžete přidat open source [ *51Degrees.mobi Foundation* ](https://github.com/51Degrees/dotNET-Device-Detection) knihovny do projektu. To je IHttpModule technologie ASP.NET nebo poskytovateli schopností prohlížeče (použít u aplikací, webových formulářů a MVC), přímo čte WURFL data a připojí ji do ASP. Mechanismus detekce NET pro prohlížeče integrované. Po instalaci modulu *Request.Browser* náhle bude obsahovat mnohem více přesné a podrobné informace: ho správně rozpozná množství zařízení už jsme zmínili a jejich možnosti (včetně seznamu Další funkce jako metoda vstupu). Další podrobnosti naleznete v dokumentaci v projektu.

## <a name="how-web-forms-applications-can-present-mobile-specific-pages"></a>Jak aplikace webových formulářů sebou může nést stránek specifických pro mobilní zařízení

Ve výchozím nastavení zde je, jak zcela nové aplikace webových formulářů zobrazí v běžné mobilních zařízení:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image1.png)

Je zřejmé ani rozložení vypadá velmi mobilní zařízení – tato stránka je navržená pro velké a objektově orientovaný na šířku monitorování není pro malé obrazovce orientovaný na výšku. Tak co můžete udělat?

Jak je popsáno dříve v tomto dokumentu, existuje mnoho způsobů, jak přizpůsobit stránky pro mobilní zařízení. Některé postupy jsou založené na server, další spuštění na straně klienta.

### <a name="creating-a-mobile-specific-master-page"></a>Vytvoření stránky předlohy specifické pro mobilní zařízení

V závislosti na požadavcích, je možné používat stejné webové formuláře pro všechny návštěvníky, ale mají dvě samostatné stránky předlohy: jeden pro klasické pracovní plochy návštěvníci, druhý pro mobilní návštěvníků. Získáte flexibilitu změna šablony stylů CSS nebo tak, aby odpovídala mobilní zařízení, bez toho, že duplicitní jakékoli logiky stránku nejvyšší úrovně kódu HTML.

To je jednoduché. Například můžete přidat obslužnou rutinu PreInit například následující webové formuláře:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample1.cs)]

Teď vytvořte hlavní stránku s názvem Mobile.Master ve složce nejvyšší úrovně vaší aplikace a použije se při zjištění mobilních zařízení. Mobilní stránky předlohy odkazovat šablony stylů CSS specifické pro mobilní zařízení v případě potřeby. Klasické pracovní plochy návštěvníků i nadále uvidí výchozí stránku předlohy, není mobilní ten.

### <a name="creating-independent-mobile-specific-web-forms"></a>Vytváření nezávislé webové formuláře specifické pro mobilní zařízení

Pro maximální flexibilitu můžete přejít mnohem víc než jen samostatné stránky předlohy pro různé typy zařízení. Můžete implementovat dvě *zcela oddělit sad stránek webových formulářů* – jeden nastavení u počítačových prohlížečů, jiné sady pro mobilní telefony. Tato metoda funguje nejlépe, pokud chcete předložit mobilní návštěvníků velmi rozdílné informace nebo pracovních postupů. Zbytek tohoto oddílu popisuje tento postup podrobněji.

Za předpokladu, že již máte aplikaci webových formulářů navržené pro stolních počítačů, vytvořte podsložku nazvanou "Mobilní" v rámci vašeho projektu a sestavení vašich mobilních stránek je nejjednodušší způsob, jak pokračovat. Dílčí celý web, s vlastní stránky předlohy, šablony stylů a stránky, můžete vytvořit pomocí stejné techniky, které byste použili pro jakékoli jiné aplikace webových formulářů. K vytvoření mobilních ekvivalent pro nepotřebujete nutně *každý* stránce na webu klasické pracovní plochy, můžete zvolit, jaké podmnožinu funkcí dává smysl pro mobilní návštěvníků.

Mobilních stránek můžete sdílet společné statické prostředky (jako jsou obrázky, JavaScript nebo šablon stylů CSS soubory) s vaší regulární stránky, pokud chcete. Protože se do složky "Mobilní" *není* označit jako samostatnou aplikační když jsou hostované v IIS (je stejně jednoduché podsložky stránky webových formulářů), se budou také sdílet stejné konfiguraci, data relace a další infrastrukturu jako vaše klasické pracovní plochy stránky.

> [!NOTE]
> Protože tento přístup obvykle zahrnuje některé duplicity kódu (mobilních stránek můžou sdílet určité podobnosti s klasické pracovní plochy stránky), je důležité, abyste Multi-Factor si všechny běžné obchodní logiku a data přístupový kód do sdílené nadřízené vrstvy nebo služby. V opačném případě budete double snaha o vytvoření a údržba vaší aplikace.


#### <a name="redirecting-mobile-visitors-to-your-mobile-pages"></a>Přesměrování mobilní návštěvníkům mobilních stránek

Často je vhodné přesměrování pouze na mobilní návštěvníkům mobilních stránek *první* žádosti v jejich relace procházení (a ne u každého požadavku v jejich relace), protože:

- Pak můžete snadno povolit mobilní uživatelé pro přístup k ploše stránky, pokud si přejí – stačí vložit odkaz na hlavní stránce, která přejde na "Desktopová verze". Návštěvník nebude přesměrovat zpět na stránku mobilní, protože už nejsou první požadavek v jejich relace.
- Tím eliminujete riziko zasahovala do žádosti o žádné dynamické prostředky sdílené mezi částmi desktopových a mobilních vašeho webu (např. Pokud jste si běžné webový formulář desktopová i mobilní částí webu zobrazení v prvku IFRAME nebo určité obslužné rutiny jazyka Ajax)

K tomuto účelu můžete umístit logiky přesměrování v **relace\_Start** metody. Například přidejte následující metodu do souboru Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample2.cs)]

#### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Konfigurace ověřování pomocí formulářů se respektoval mobilních stránek

Všimněte si, že ověřování pomocí formulářů určitých předpokladů o kde ho můžete během a po dokončení procesu ověřování přesměrovat návštěvníků:

- Když uživatel potřebuje k ověření, ověřování pomocí formulářů je bude přesměrovávat na stránku desktopovém přihlášení, bez ohledu na to, zda jsou desktopové nebo mobilní uživatele (protože má jenom koncept *jeden* přihlašovací adresa URL). Za předpokladu, že chcete jinak stylu mobilní přihlašovací stránku, musíte k vylepšení plochy přihlašovací stránku tak, aby ji mobilní uživatele přesměruje na samostatné mobilní přihlašovací stránku. Například přidejte následující kód, který vaše **desktop** přihlašovací stránky kódu: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample3.cs)]
- Jakmile uživatel úspěšně přihlásí, ověřování pomocí formulářů se ve výchozím nastavení přesměruje je to klasické pracovní plochy domovské stránky (protože má jenom koncept *jeden* výchozí stránky). Je potřeba zvýšit mobilní přihlašovací stránku tak, aby ho přesměruje do mobile domovskou stránku po úspěšném přihlášení. Například přidejte následující kód, který vaše **mobilní** přihlašovací stránky kódu: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample4.cs)]
  
  Tento kód předpokládá, že vaše stránka má ovládací prvek Login server volá LoginUser, jako výchozí šablony projektu.

### <a name="working-with-output-caching"></a>Práce s ukládání výstupu do mezipaměti

Pokud při použití ukládání výstupu do mezipaměti, mějte na paměti, že ve výchozím nastavení je možné pro klasické pracovní plochy uživatele k navštívení určité adresy URL (což způsobí jeho výstupu do mezipaměti), následované mobilní uživatele, který pak obdrží výstup z mezipaměti klasické pracovní plochy. Toto upozornění platí, ať už právě různé stránky předlohy podle typu zařízení, nebo implementace zcela samostatné webové formuláře podle typu zařízení.

Abyste zabránili problémům, můžete dát pokyn, ASP.NET, aby položka mezipaměti podle Určuje, zda je návštěvníka použitím mobilního zařízení se liší. Přidáte parametr VaryByCustom na vaší stránce OutputCache deklaraci takto:

[!code-aspx[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample5.aspx)]

Dále definujte *isMobileDevice* jako vlastní mezipaměti přepsat parametru tak, že přidáte následující metodu do souboru Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample6.cs)]

Tím se zajistí, že mobilní návštěvníků na stránce nepřijímají výstup dříve vložit do mezipaměti klientů návštěvníka.

### <a name="a-working-example"></a>Funkční příklad

Pokud chcete zobrazit tyto techniky v akci, stáhněte si [dokument white paper, ukázky kódu](https://docs.microsoft.com/aspnet/mobile/overview). Ukázková aplikace webových formulářů mobilní uživatele automaticky přesměruje na sadu stránek specifických pro mobilní zařízení v podsložce s názvem mobilní zařízení. Značky a stylu tyto stránky je lépe optimalizovaná pro mobilní prohlížeče, jak je vidět na následujících snímcích obrazovky:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image2.png)

Další informace o optimalizaci značky a šablony stylů CSS pro mobilní prohlížeče najdete v části "Určení stylu mobilních stránek pro mobilní prohlížeče" dále v tomto dokumentu.

## <a name="how-aspnet-mvc-applications-can-present-mobile-specific-pages"></a>Jak aplikace ASP.NET MVC sebou může nést stránek specifických pro mobilní zařízení

Protože vzor Model-View-Controller odděluje logiku aplikace (kontrolery) od logiky prezentace (v zobrazení), můžete z kteréhokoli z následujících dvou přístupů zpracování mobilní podporu v kódu na straně serveru:

1. ***Použití stejného kontrolerů a zobrazení pro stolní počítače a mobilní prohlížeče, ale vykreslit zobrazení s různá rozložení Razor v závislosti na typu zařízení *.** Tato volba funguje nejlépe, je-li zobrazit stejná data na všech zařízeních, ale jednoduše chcete zadat jiné šablony stylů CSS nebo změnit několik elementů HTML nejvyšší úrovně pro mobilní telefony.
2. ***Použít stejnou řadiče pro stolní počítače a mobilní prohlížeče, ale vykreslení různá zobrazení v závislosti na typu zařízení***. Tato volba funguje nejlépe, pokud jste zobrazení přibližně stejná data a poskytují stejné pracovních postupů pro koncové uživatele, ale chcete vykreslit velice různý kód HTML tak, aby odpovídala zařízení používá.
3. ***Vytvoření samostatné oblasti pro stolní počítače a mobilní prohlížeče, implementace nezávislé kontrolerů a zobrazení pro každou *.** Tato volba funguje nejlépe, pokud jste zobrazení velmi různých obrazovek, obsahující různé informace a úvodní uživatele provede různé pracovní postupy, které jsou optimalizované pro jejich typ zařízení. Může to znamenat některé opakování kódu, ale můžete minimalizovat, ve které budou zohledňovat si běžné logiky do podkladové vrstvy nebo služby.

Pokud budete chtít využít **první** možnost a pouze rozložení Razor se liší podle typu zařízení, je velmi snadné. Upravit vaše \_ViewStart.cshtml souboru následujícím způsobem:

[!code-cshtml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample7.cshtml)]

Nyní můžete vytvořit konkrétní mobilní rozložení volá \_LayoutMobile.cshtml se strukturou stránky a šablony stylů CSS pravidla optimalizovány pro mobilní zařízení.

Pokud chcete převést **druhý** možnosti vykreslování něco úplně jiného zobrazení v závislosti na typu zařízení návštěvníka, přečtěte si téma [blogový příspěvek Scotta Hanselmana](http://www.hanselman.com/blog/ABetterASPNETMVCMobileDeviceCapabilitiesViewEngine.aspx).

Zbytek tohoto dokumentu se zaměřuje na **třetí** možnost – vytvoření samostatných řadičů *a* zobrazení pro mobilní zařízení – proto můžete řídit přesně se nabízí jaké podmnožinu funkcí pro mobilní návštěvníků.

### <a name="setting-up-a-mobile-area-within-your-aspnet-mvc-application"></a>Nastavení na mobilní oblast v rámci vaší aplikace ASP.NET MVC

Můžete přidat oblast s názvem "Mobilní" do stávající aplikace ASP.NET MVC obvyklým způsobem: klikněte pravým tlačítkem na název vašeho projektu v Průzkumníku řešení a zvolte Přidat a oblasti. Poté můžete přidat kontrolerů a zobrazení stejně jako ostatní oblasti v rámci aplikace ASP.NET MVC. Například přidáte mobilní oblast nový kontroler volá HomeController tak, aby fungoval jako domovskou stránku pro mobilní návštěvníků.

### <a name="ensuring-the-url-mobile-reaches-the-mobile-homepage"></a>Zajištění URL /Mobile dosáhne mobilní domovské stránky

Pokud chcete adresy URL /Mobile kontaktovat, akce indexu na HomeController uvnitř mobilních areálu, je potřeba dělat dva malé změny konfigurace směrování. Nejprve aktualizujte vaše třída MobileAreaRegistration tak, aby byla HomeController výchozí řadiče ve vaší oblasti mobilních jak je znázorněno v následujícím kódu:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample8.cs)]

To znamená, že mobilní Domovská stránka nyní bude umístěn na /Mobile, nikoli/Mobile Domů, protože "Home" je teď výchozí název kontroleru implicitně mobilních stránek.

Dále si všimněte, že tak, že přidáte druhý HomeController do vaší aplikace (například mobilní tu, nejen u desktopového existující jeden), jste budete zrušili domovské stránce regulární klasické pracovní plochy. Dojde k selhání s chybou "*bylo nalezeno několik typů, které se shodují s řadičem s názvem"Home"*". Chcete-li tento problém vyřešit, aktualizujte nejvyšší úrovně konfigurace směrování (v Global.asax.cs) k určení, že by klasické pracovní plochy HomeController po nejednoznačnosti mají přednost:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample9.cs)]

Nyní chyba půjdou vypustila při optimalizaci a URL http://<em>yoursite</em>/ dosáhne klasické pracovní plochy domovskou stránku a http://<em>yoursite</em>/mobile/ dosáhne mobilní domovské stránky.

### <a name="redirecting-mobile-visitors-to-your-mobile-area"></a>Přesměrování mobilní návštěvníkům mobilní oblast

Existuje mnoho různých rozšíření bodů v architektuře ASP.NET MVC, tak, že existuje mnoho způsobů je to možné vkládat logiky přesměrování. Jednou z úhledné možností je vytvořit atribut filtru, [RedirectMobileDevicesToMobileArea], který provádí přesměrování, pokud jsou splněny následující podmínky:

1. Je první požadavek v relaci uživatele (například Session.IsNewSession rovná se true)
2. Požadavek pochází z mobilního prohlížeče (například Request.Browser.IsMobileDevice rovná se true)
3. Uživatel není již požadování prostředku v oblasti mobilních (například *cesta* část adresy URL nezačíná /Mobile)

Ukázky ke stažení, které jsou zahrnuté v tomto dokumentu white paper obsahuje implementaci tuto logiku. Je implementován jako filtr autorizace, odvozené od třídy AuthorizeAttribute, což znamená, že je správně fungovaly i v případě použití ukládání výstupu do mezipaměti (jinak, pokud první přístupy klasické pracovní plochy návštěvníka určité adresy URL, klasické pracovní plochy výstupu mohou být uložené v mezipaměti a pak dodávat do následné mobilní návštěvníci).

Protože je filtr, můžete zvolit uplatňovat na konkrétní řadiče a akce, například

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample10.cs)]

… nebo můžete ho použít pro všechny kontrolery a akce jako MVC 3 *globálních filtrů* v souboru Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample11.cs)]

Ukázky ke stažení také ukazuje, jak můžete vytvořit podtříd tento atribut přesměrovat na konkrétní umístění v rámci mobilní oblast. To znamená, že například že můžete:

- Registraci globálního filtru jak je znázorněno výše, která odesílá mobilní návštěvníků mobilní domovskou stránku ve výchozím nastavení.
- Speciální filtr [RedirectMobileDevicesToMobileProductPage] platí také pro "zobrazení product" akci, která přebírá mobilní návštěvníků na mobilní verzi měl požadovaná stránka produktu.
- Další zvláštní podtřídy filtr platí také pro konkrétní akce, přesměrování mobilní návštěvníků na stejnou stránku mobilní

### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Konfigurace ověřování pomocí formulářů se respektoval mobilních stránek

Pokud používáte ověřování pomocí formulářů, nezapomeňte přitom, že když uživatel potřebuje přihlásit, se automaticky přesměruje uživatele na jednu konkrétní "přihlásit" adresu URL, která ve výchozím nastavení je **/účet/přihlášení**. To znamená, že mobilní uživatelé mohou přesměrováni na akci klasické pracovní plochy "přihlášení".

K tomuto problému vyhnout, přidáte logiku klasické pracovní plochy akci "přihlásit" tak, aby znovu přesměrování mobilními uživateli na mobilní "přihlásit" akce. Pokud používáte výchozí šablonu aplikace ASP.NET MVC, aktualizujte akce AccountController společnosti přihlášení následujícím způsobem:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample12.cs)]

… a na řadiči volá AccountController nejbližší mobilní implementací vhodný "přihlásit" akce specifické pro mobilní zařízení.

### <a name="working-with-output-caching"></a>Práce s ukládání výstupu do mezipaměti

Pokud používáte filtr [OutputCache], je nutné vynutit položky mezipaměti se liší podle typu zařízení. Například napište:

[!code-javascript[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample13.js)]

Potom přidejte následující metodu do souboru Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample14.cs)]

Tím se zajistí, že mobilní návštěvníků na stránce nepřijímají výstup dříve vložit do mezipaměti klientů návštěvníka.

### <a name="a-working-example"></a>Funkční příklad

Pokud chcete zobrazit tyto techniky v akci, stáhněte si [dokument white paper kód související ukázky](https://docs.microsoft.com/aspnet/mobile/overview). Ukázka zahrnuje aplikaci ASP.NET MVC 3 (verze Release Candidate) Vylepšený pro podporu mobilních zařízení pomocí metod popsaných výše.

## <a name="further-guidance-and-suggestions"></a>Další doprovodné materiály a návrhy

Následující informace platí i pro webové formuláře a MVC vývojáři, kteří jsou pomocí techniky popsané v tomto dokumentu.

### <a name="enhancing-your-redirection-logic-using-51degreesmobi-foundation"></a>Rozšíření přesměrování logiku pomocí 51Degrees.mobi Foundation

Může být dokonale dostatečná pro vaši aplikaci logiky přesměrování je znázorněno v tomto dokumentu, ale nebude fungovat, pokud je nutné zakázat relace, nebo mobilní prohlížeče, které zamítnout soubory cookie (tyto nemůže mít relace), protože nebude vědět, jestli daný požadavek je první z nich z tohoto návštěvníka.

Už jste zjistili, jak opensourcový 51Degrees.mobi Foundation zlepšit přesnost ASP. Zjišťování sítě pro prohlížeče. Také obsahuje integrovanou schopnost přesměrování mobilní návštěvníkům konkrétní umístění nakonfigurovaná v souboru Web.config. Je možné pracovat bez v závislosti na relace ASP.NET (a tím i soubory cookie) uložením dočasné log hash návštěvníků hlavičky protokolu HTTP a IP adres, tedy ví, zda každý požadavek je první z nich z daného vistor.

Přidat do části fiftyOne souboru web.config následující element bude přesměrovávat první požadavek z mobilního zařízení zjištěná na stránku ~ / Mobile/Default.aspx. Budou všechny žádosti na stránky ve složce mobilní *není* přesměrovat, bez ohledu na typ zařízení. Pokud mobilní zařízení bylo nečinné po dobu 20 minut nebo více zařízení se budou vymazány a následné žádosti, bude zacházeno jako nové značky pro účely přesměrování.

[!code-xml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample15.xml)]

Další podrobnosti najdete v tématu [51degrees.mobi Foundation dokumentaci](https://github.com/51Degrees/dotNET-Device-Detection).

> [!NOTE]
> Můžete *můžete* Foundation 51Degrees.mobi použití funkce přesměrování na aplikace ASP.NET MVC, ale bude nutné definovat konfiguraci přesměrování z hlediska jednoduché adresy URL není z hlediska směrování parametry nebo vložením MVC filtry na akce. Důvodem je, že (v době psaní) nemůže rozpoznat 51Degrees.mobi Foundation filtry nebo směrování.


### <a name="disabling-transcoders-and-proxy-servers"></a>Zakázání Tricaster a Proxy servery

Mobilní síť operátory mají dva různé cíle v jejich přístupu k mobilní síti internet.

1. Zadejte jako mnohem souvisejícího obsahu nejvíce
2. Zajištění maximálního počtu zákazníků, kteří můžou sdílet šířku pásma sítě omezené přepínač

Protože většina webových stránek byly navrženy pro velké obrazovky velikost plochy a Rychlá oprava řádku širokopásmové připojení, použít mnoho operátorů *tricaster* nebo *proxy servery* , která dynamicky měnit webového obsahu. Se může změnit kód HTML a CSS tak, aby odpovídala menších obrazovkách (hlavně u "funkce telefony", které nemají výpočetní výkon pro zpracování složitých rozložení), a jejich znovu komprimovat imagí (výrazně snižuje jejich kvalitu) ke zlepšení rychlosti doručení stránky.

Ale pokud jste pořídili úsilí k vytvoření optimalizované pro mobilní verzi vaší lokality, pravděpodobně nebudete chtít, operátora mobilní sítě narušoval se žádné další. Přidáte následující řádek na stránce\_zatížení událost v jakékoli webový formulář ASP.NET:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample16.cs)]

Nebo pro kontroler ASP.NET MVC, můžete přidat následující přepsání metody tak, aby se vztahuje na všechny akce v kontroleru:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample17.cs)]

Výslednou zprávu HTTP informuje tricaster standardu W3C a proxy servery obsahu nemění. Samozřejmě není zaručeno, že operátory mobilní sítě bude respektovat tato zpráva.

### <a name="styling-mobile-pages-for-mobile-browsers"></a>Používání stylů pro mobilních stránek pro mobilní prohlížeče

Je nad rámec tohoto dokumentu k podrobnému popisu, skvělé jaké druhy práce značky HTML správně nebo které techniky návrh webové zajištění maximálního použitelnosti na konkrétní zařízení. To má nahoru vám najít dostatečně jednoduché rozložení, optimalizované pro mobile velikost obrazovky, bez použití nespolehlivé HTML nebo umístění triky šablony stylů CSS. Jedna z technik důležité zmínit, ale *zobrazení metaznačku*.

Některé moderní mobilní prohlížeče, na úsilí zobrazení webové stránky určené pro monitorování klientů, vykreslení stránky na virtuální plátno, také nazývané "zobrazení" (například virtuální zobrazení je 980 pixelů na šířku na Iphonu a 850 pixelů na šířku v prohlížeči Opera Mobile ve výchozím nastavení) a pak výsledek škálujte tak, aby na fyzických pixelech na obrazovce. Uživatel pak můžete přiblížit a posouvání zobrazení tohoto zobrazení. To má výhodu v tom, že umožňuje, aby prohlížeč zobrazení stránky v jeho zamýšlenou rozložení, ale je také obsahuje nevýhodou je, že vynutí přibližování a posouvání, která je vhodná pro uživatele. Pokud vytváříte pro mobilní zařízení, je lepší návrh pro úzké obrazovku tak, aby žádné zvětšování nebo posouvání ve vodorovném směru, je nezbytné.

Způsob, jak zjistit mobilní prohlížeče, jak široké by měl být zobrazení je nestandardní *zobrazení* značka meta. Například, pokud přidáte následující hlavní části na stránce

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample18.html)]

… potom podpora prohlížeče chytrém telefonu se rozložení stránky na plátně celý virtuální 480 pixelů. To znamená, že pokud vaše elementů HTML definovat jejich šířku v procentech, procenta interpretován s ohledem na tento šířka v pixelech 480, ne výchozí šířka zobrazení. Díky tomu je méně pravděpodobné, že máte přiblížení a posouvání vodorovně – výrazně zlepšuje mobilní rozhraní prohlížeče uživatele.

Pokud chcete, aby šířka zobrazení tak, aby odpovídaly fyzických pixelech zařízení, můžete zadat následující:

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample19.html)]

Pro tento postup správně fungoval, nesmí vynutíte explicitně prvků, které mají být delší než tento šířku (například pomocí *šířka* atribut nebo vlastnost CSS), jinak v prohlížeči bude muset použít větší zobrazení bez ohledu na to. Viz také: [další podrobnosti o značce nestandardní zobrazení](https://developer.apple.com/library/safari/#documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html).

Většina moderních smartphony podporují *duální orientace*: můžou uchovávat v režimu na výšku nebo na šířku. Proto je důležité, abyste vytvářet předpoklady o obrazovky šířka v pixelech. Dokonce i Nepředpokládejte, že je pevná šířka obrazovky, protože uživatel může znovu zorientovat svoje zařízení, pokud nejsou na stránce.

Starší zařízení Windows Mobile a Blackberry může také přijímat těchto značek meta v záhlaví stránky informovat obsahu byla optimalizována pro mobilní zařízení a proto by neměl být převedeny.

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample20.html)]

## <a name="additional-resources"></a>Další prostředky

Seznam mobilních zařízení emulátorů a simulátorů, můžete použít k testování mobilní webové aplikace v ASP.NET, naleznete na stránce [simulace oblíbených mobilních zařízení pro testování](../mobile/device-simulators.md).

## <a name="credits"></a>Kredity

- Primárního autora: Steven Sanderson
- Revidující / další obsah zapisovačů: James Rosewell, Mikael Söderström, Scott Hanselman a Scott Hunter
