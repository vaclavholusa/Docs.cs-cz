---
uid: web-forms/what-is-web-forms
title: Novinky webových formulářů | Dokumentace Microsoftu
author: rick-anderson
description: ''
ms.author: riande
ms.date: 02/21/2014
ms.assetid: 5fa1daf9-1161-4cfa-bd4c-658f48b2c229
msc.legacyurl: /web-forms/what-is-web-forms
msc.type: content
ms.openlocfilehash: ccb0e6096b0281e5ef8af63bfd84b172e7f209b9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755103"
---
<a name="what-is-web-forms"></a>Novinky webových formulářů
====================
Webové formuláře ASP.NET je součástí architektury webové aplikace ASP.NET a je součástí [sady Visual Studio](https://www.asp.net/downloads). Představuje jednu ze čtyř programovacích modelů, které lze použít k vytvoření webové aplikace ASP.NET, ostatní jsou jednostránkové aplikace ASP.NET, ASP.NET MVC a ASP.NET Web Pages.

Webové formuláře jsou stránky, které vaši uživatelé požadavku ve svém prohlížeči. Tyto stránky může být napsán pomocí kombinace HTML, klientského skriptu, serverových ovládacích prvků a serverovým kódem. Pokud uživatelé požadují stránku, je kompilován a spuštěn na serveru v rámci rozhraní a pak rozhraní framework generuje kód HTML, který může mít za následek prohlížeče. Na stránce technologie ASP.NET webové formuláře uvádí informace pro uživatele v libovolném prohlížeči nebo klientské zařízení.

Pomocí sady Visual Studio, můžete vytvořit webových formulářů ASP.NET. Visual Studio integrované vývojové prostředí (IDE) umožňuje přetažení serverových ovládacích prvků pro rozložení stránky webových formulářů. Pak můžete snadno nastavit vlastnosti, metody a události pro ovládací prvky na stránce nebo stránce přímo. Tyto vlastnosti, metody a události se používají k definování chování webové stránky, vzhled a chování a tak dále. Vytvoření serveru kódu pro zpracování logiky stránky, můžete použít jazyk .NET, jako je Visual Basic nebo C#.

> [!NOTE] 
> 
> Dokumentace k ASP.NET a sady Visual Studio zahrnuje několik verzí. Témata, která demonstrují funkce z předchozích verzí může být užitečná pro aktuální úlohy a scénáře použití nejnovější verze.


**Webové formuláře ASP.NET jsou:** 

- Založené na technologii Microsoft ASP.NET, ve kterém kód, který běží na serveru dynamicky generuje webové stránky výstup do prohlížeče nebo klienta zařízení.
- Kompatibilní s libovolného prohlížeče nebo mobilního zařízení. Webová stránka ASP.NET automaticky vykreslí správné kompatibilní s prohlížečem kód HTML pro funkce, jako je styly, rozložení a tak dále.
- Kompatibilní s libovolný jazyk podporuje .NET common language runtime, jako je například Microsoft Visual Basicu a Microsoft Visual C#.
- Postaveno na rozhraní Microsoft .NET Framework. To poskytuje všechny výhody architektury, včetně spravovaném prostředí, bezpečnost typů a dědičnosti.
- Je flexibilní, protože přidáte vytvořené uživatelem a k nim ovládací prvky třetích stran.

**Nabídka webových formulářů ASP.NET:** 

- Oddělení kódu HTML a jiný kód uživatelského rozhraní od logiky aplikace.
- Bohatá sada serverové ovládací prvky pro běžné úlohy, včetně přístupu k datům.
- Výkonné datové vazby s podporou skvělé nástroje.
- Podpora skriptování na straně klienta, který se spustí v prohlížeči.
- Podpora pro různé další možnosti, jako je směrování, zabezpečení, výkon, mezinárodní prostředí, testování, ladění, zpracování chyb a správu stavu.

## <a name="aspnet-web-forms-helps-you-overcome-challenges"></a>Webové formuláře ASP.NET pomáhá překonávat výzvy

Programování webové aplikace představuje výzev, které obvykle nevznikají při programování tradičních aplikací na základě klienta. Mezi výzvy patří:

- **Implementace také bohaté rozhraní příkazů uživatelské Web** – může být obtížné a tedious navrhovat a implementovat uživatele pomocí základních funkcí HTML, zejména v případě, že na stránce má složitá rozložení, velký objem dynamický obsah, rozhraní a plně funkční interaktivní uživatelské objekty.
- **Oddělení klientů a serverů** -ve webové aplikaci, klient (prohlížeče) a serveru jsou různé programy často spuštěné v různých počítačích (a dokonce i v různých operačních systémech). V důsledku toho oběma polovinami aplikace sdílejí velmi málo informací; může komunikovat se ale obvykle exchange pouze menších dávkách simple informace.
- **Bezstavové provádění** - li webový server přijme požadavek na stránku, ho najde na stránce, procesy, odešle do prohlížeče a poté odstraní všechny informace o stránce. Pokud uživatel požaduje znovu stejná stránka, server zopakuje celé sekvenci stránku od začátku. Jinými slovy, je na serveru nedostatek paměti stránky, které se má zpracovat – stránky jsou bezstavové. Proto pokud aplikace potřebuje udržovat informace o stránce, jejich bezstavové povaze stát problémem.
- **Možnosti neznámého klienta** – v mnoha případech jsou dostupné pro mnoho uživatelů s různými prohlížeči webových aplikací. Prohlížečů obsahuje různé možnosti, což znesnadňuje vytvořit aplikaci, která se spustí stejně dobře pro všechny z nich.
- **Komplikace u přístupu k datům** – čtení a zápis ke zdroji dat v tradiční webové aplikace může být složitý a náročný.
- **Komplikace se škálovatelností** – v řadě případů nezdaří webové aplikace s existující metody pro splnění cíle škálovatelnosti vzhledem k absenci kompatibilitu mezi různými součástmi aplikace. To je často společný bod selhání pro aplikace v rámci cyklu vysokého růstu.

Splnění těchto výzev pro webové aplikace může vyžadovat značné množství času a úsilí. Webové formuláře ASP.NET a ASP.NET framework řeší tyto problémy následujícími způsoby:

- **Výsledkem je intuitivní a konzistentní objektový model** – objektový model, který umožňuje představit si formuláře jako celek, ne jako samostatné části klienta a serveru představuje rámec stránky ASP.NET. V tomto modelu můžete naprogramovat stránky v mnohem intuitivnější způsobem než tradiční webových aplikací, včetně možnosti nastavit vlastnosti pro elementy stránek a reagovat na události. Kromě toho jsou serverové ovládací prvky ASP.NET abstrakce fyzického obsah stránky HTML a přímé interakce mezi prohlížečem a serverem. Obecně platí můžete použít ovládací prvky serveru způsob, jak může pracovat s ovládacími prvky v aplikaci klienta a není potřeba uvažovat o tom, jak vytvořit HTML k prezentaci a zpracovat ovládací prvky a jejich obsah.
- **Programovací model založený na událostech** – přeneste webových formulářů ASP.NET do webové aplikace známý model zápisu obslužných rutin událostí pro události, ke kterým dochází na klienta nebo serveru. Rámec stránky ASP.NET abstrahuje tak, že základní mechanismus zachytávání událost na straně klienta, přenášení na server a voláním příslušné metody je všechny automatické a neviditelná, do které tento model. Výsledkem je jasné, snadno písemné kód strukturu, která podporuje vývoj založený na událostech.
- **Správa stavů intuitivní** – rámec stránky ASP.NET automaticky zpracovává Správa stavu stránky a ovládacích prvků a poskytuje explicitní způsoby, jak udržovat stav informace specifické pro aplikaci. To se provádí bez hodně využívají prostředky serveru a může být implementováno s nebo bez odeslání souborů cookie v prohlížeči.
- **Aplikace prohlížeče nezávislé** -rámec stránky ASP.NET umožňuje vytvářet veškerou logiku aplikace na serveru, tím eliminuje nutnost explicitně kód pro rozdíly v prohlížečích. Nicméně stále umožňuje využít výhod funkce specifické pro prohlížeč napsáním kódu na straně klienta k poskytování lepšího výkonu a pohodlnější a pestřejší prostředí klienta.
- **Rozhraní .NET framework podpora modulu CLR** -rámec stránky ASP.NET je založená na rozhraní .NET Framework, tak na celé rozhraní je k dispozici pro všechny aplikace ASP.NET. Aplikace lze zapsat v libovolném jazyce, který je kompatibilní s, který je s modulem runtime. Kromě toho přístup k datům zjednodušeno pomocí infrastruktury přístupu dat poskytované rozhraní .NET Framework, ADO.NET.
- **Výkon serveru škálovatelné rozhraní .NET framework** -rámec stránky ASP.NET umožňuje škálovat webovou aplikaci z jednoho počítače s jedním procesorem do webové farmy s více počítači čistě a bez nutnosti složité změn aplikace logika.

## <a name="features-of-aspnet-web-forms"></a>Funkce rozhraní ASP.NET Web Forms

- **Serverové ovládací prvky**– ovládací prvky ASP.NET webového serveru jsou objekty na webových stránkách ASP.NET spuštěné, když je zobrazení stránky vyžadováno a vykreslení značek v prohlížeči. Mnoho ovládacích prvků webového serveru jsou podobné známé elementy HTML, jako jsou tlačítka a textová pole. Další ovládací prvky zahrnují složité chování, jako je kalendář a ovládací prvky, které slouží k připojení ke zdrojům dat a zobrazovat data.
- **Stránky předlohy**-stránek předloh ASP.NET umožňují vytvářet konzistentního rozložení pro stránky v aplikaci. Jedna stránka předlohy definuje vzhled a chování a standardní chování, které chcete použít pro všechny stránky (nebo skupinu stránek) ve vaší aplikaci. Potom můžete vytvořit jednotlivé stránky obsahu, které obsahují obsah, který chcete zobrazit. Pokud uživatelé požadují obsah stránky, sloučit s hlavní stránkou vytvořit výstup, který kombinuje rozložení stránky předlohy s obsahem ze stránky obsahu.
- **Práce s daty**– poskytuje řadu možností uložení, načtení a zobrazení dat ASP.NET. V aplikaci webových formulářů ASP.NET které ovládací prvky vázané na data použít k automatizaci prezentaci nebo vstupní data webové stránky prvky uživatelského rozhraní, jako jsou tabulky a textová pole a rozevírací seznamy.
- **Členství**– ASP.NET Identity ukládá přihlašovací údaje uživatelů v databázi vytvořené aplikací. Pokud vaši uživatelé přihlásí, aplikace ověřuje přihlašovacích údajů tak, že databáze pro čtení. Váš projekt *účet* složka obsahuje soubory, které implementují jednotlivé součásti členství: registrace, přihlášení, změna hesla a autorizaci přístupu. Kromě toho webových formulářů ASP.NET podporuje protokol OAuth a OpenID. Tato vylepšení ověřování umožňuje uživatelům přihlášení na webu pomocí existujících přihlašovacích údajů, z těchto účtů jako Facebook, Twitter, Windows Live a Googlu. Ve výchozím nastavení šablona vytvoří databázi členství pomocí výchozí název databáze v instanci systému SQL Server Express LocalDB, vývoj pro databázový server, který je součástí sady Visual Studio Express 2013 for Web.
- **Skript klienta a klientská rozhraní**-na serveru funkce technologie ASP.NET můžete vylepšit včetně funkcí klientského skriptu na stránkách webový formulář ASP.NET. Můžete použít skript klienta můžete uživatelům poskytnout bohatší a rychleji reagující uživatelské rozhraní. Klientský skript můžete také provádět asynchronní volání na webový server je spuštěn na stránku v prohlížeči.
- **Směrování**– směrování adres URL umožňuje nakonfigurovat aplikaci tak, aby přijímal žádosti adresy URL, které nelze namapovat na fyzické soubory. Adresa URL požadavku je jednoduše adresu URL, které uživatel zadá do svého prohlížeče najděte požadovanou stránku na webu. Používáte směrování k definování adresy URL, která jsou sémanticky srozumitelné pro uživatele a, které mohou pomoci s optimalizací vyhledávače (SEO).
- **Stav správy**-webových formulářů technologie ASP.NET obsahuje několik možností, které vám pomohou chránit data na základě stránky a jednotlivé celou aplikaci.
- **Zabezpečení**-důležitou součástí vývoje aplikace bezpečnější je pochopit na ohrožení. Microsoft vyvinul způsob, jak zařadit hrozby: falšování identity, úmyslné poškozování, popírání odpovědnosti, zpřístupnění informací, odepření služby, zvýšení úrovně oprávnění (krok). Ve webových formulářů ASP.NET můžete přidat bodů rozšiřitelnosti a možnosti konfigurace, které vám umožní přizpůsobit různé chování zabezpečení ve webových formulářů ASP.NET.
- **Výkon**-výkonu může být klíčovým faktorem úspěšného webového serveru nebo projektu. Webové formuláře ASP.NET můžete upravit výkonu související stránky a zpracování ovládacího prvku serveru, správa stavů, přístup k datům, konfigurace aplikace a načítání a efektivní kódování.
- **Internacionalizace**– webové formuláře ASP.NET umožňuje vytvářet webové stránky, které můžete získat obsah a další data podle nastavení upřednostňovaného jazyka v prohlížeči nebo na základě uživatele explicitní volby jazyka. Obsah a další data se označuje jako prostředky a taková data mohou být uloženy v souborech prostředků nebo jiných zdrojů. Na stránce technologie ASP.NET webové formuláře nakonfigurujete ovládací prvky k získání hodnot jejich vlastností z prostředků. Výrazy prostředků v době běhu, jsou nahrazené prostředky z odpovídající lokalizované soubory prostředků.
- **Ladění a zpracování chyb**– technologie ASP.NET obsahuje funkce, které vám pomohou diagnostikovat problémy, které mohou nastat ve vaší aplikaci webových formulářů. Ladění a zpracování chyb jsou také podporovány v rámci technologie ASP.NET webové formuláře tak, aby vaše aplikace kompilace a spuštění efektivně.
- **Nasazení a hostování**– Visual Studio, ASP.NET, Azure a služby IIS poskytují nástroje, které vám pomůžou se proces nasazení a hostování aplikace webových formulářů.

## <a name="deciding-when-to-create-a-web-forms-application"></a>Rozhodování o tom, kdy vytvořit aplikaci Web Forms

Je třeba důkladně zvážit, zda pro webovou aplikaci implementovat pomocí ASP.NET Web Forms modelu nebo jiný model, jako je například rozhraní ASP.NET MVC. Architektura MVC nenahrazuje model webových formulářů; můžete použít buď rozhraní pro webové aplikace. Než se rozhodnete použít model webových formulářů nebo architektura MVC pro konkrétní web, zvažte výhody obou těchto přístupů.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Výhody webové aplikace webových formulářů

Architektura využívající webové formuláře nabízí následující výhody:

- Podporuje model událostí, který zachovává stav prostřednictvím protokolu HTTP, což je výhodné pro vývoj webových aplikací podnikové aplikace. Aplikace využívající webové formuláře poskytuje desítky událostí, které jsou podporovány stovkami ovládacích prvků serveru.
- Využívá vzor Page Controller, který přidává funkce jednotlivým stránkám. Další informace najdete v tématu [Page Controller](https://go.microsoft.com/fwlink/?LinkId=106359 "Page Controller") na webové stránce MSDN.
- Využívá stav zobrazení ani formuláře na serveru, jimž lze usnadnit správu informací o stavu.
- Je vhodný pro malé týmy webových vývojářů a návrhářů, kteří chtějí využít nabídky velkého množství komponent vhodných pro rychlý vývoj aplikací.
- Obecně je méně složitý pro vývoj aplikací, protože komponenty ( **stránky** třídy ovládacích prvků a tak dále) jsou pevně integrovány a obvykle vyžadují méně kódu než MVC model.

### <a name="advantages-of-an-mvc-based-web-application"></a>Výhody využívající architekturu MVC webové aplikace

Architektura ASP.NET MVC nabízí následující výhody:

- To usnadňuje správa složitých aplikací vydělením aplikaci na model, zobrazení a kontroler.
- Nevyužívá stav zobrazení ani formuláře umístěné na serveru. Díky tomu architektura MVC ideální pro vývojáře, kteří chtějí mít plnou kontrolu nad chováním aplikace.
- Využívá vzor Front Controller, který zpracovává požadavky webové aplikace prostřednictvím jediného kontroleru. To umožňuje návrh aplikace, který podporuje infrastrukturu s rozsáhlým směrováním. Další informace najdete v tématu [Front Controller](https://go.microsoft.com/fwlink/?LinkId=106357 "Front Controller") na webové stránce MSDN.
- Poskytuje lepší podporu pro vývoj řízený testováním (TDD).
- To funguje dobře pro webové aplikace, které jsou podporovány velkými týmy vývojářů a webových návrhářů, kteří potřebují značnou míru kontroly nad chováním aplikací.
