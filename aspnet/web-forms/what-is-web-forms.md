---
uid: web-forms/what-is-web-forms
title: Co je Web Forms | Microsoft Docs
author: rick-anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: 5fa1daf9-1161-4cfa-bd4c-658f48b2c229
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/what-is-web-forms
msc.type: content
ms.openlocfilehash: dcaba60a8640ce83f73460a5c8ee457257b9397e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="what-is-web-forms"></a>Co je Web Forms
====================
Webové formuláře ASP.NET je součástí Architektura webových aplikací ASP.NET a je součástí [Visual Studio](https://www.asp.net/downloads). Představuje jednu ze čtyř programovací modely, které můžete použít k vytvoření webové aplikace ASP.NET, ostatní jsou ASP.NET MVC, rozhraní ASP.NET Web Pages a jednostránkové aplikace ASP.NET.

Webové formuláře jsou stránky, které uživatelé požadují pomocí svého prohlížeče. Tyto stránek je možné zapisovat pomocí kombinace HTML, klientského skriptu, ovládací prvky serveru a kódu serveru. Pokud uživatelé požadují stránky, je zkompilovat a provedený rozhraní na serveru a pak rozhraní generuje kód HTML, která může vykreslit v prohlížeči. Stránku webových formulářů ASP.NET zobrazí informace o uživateli v libovolném prohlížeče nebo klientského zařízení.

Pomocí sady Visual Studio, můžete vytvořit webových formulářů ASP.NET. Prostředí Visual Studio integrované vývoj prostředí (IDE) umožňuje přetažení ovládací prvky serveru k rozložení stránky webových formulářů. Pak můžete snadno nastavit vlastnosti, metod a události pro ovládací prvky na stránce nebo při stránky. Tyto vlastnosti, metod a události se používá k definování chování webové stránky, vzhled a chování a tak dále. Zápis serverový kód pro zpracování logiky pro stránku, můžete použít jazyky .NET jako Visual Basic a C#.

> [!NOTE] 
> 
> Dokumentace k technologii ASP.NET a Visual Studio rozdělena na několik verzí. Témata s přehledem funkcí z předchozích verzí může být užitečná pro aktuální úlohy a scénáře pomocí nejnovější verze.


**Webové formuláře ASP.NET jsou:** 

- Založené na technologii Microsoft ASP.NET, ve kterém kód, který běží na serveru dynamicky vygeneruje výstup webové stránky do prohlížeče nebo klienta zařízení.
- Kompatibilní s prohlížeči nebo mobilní zařízení. Webové stránky ASP.NET automaticky vykreslí správný HTML kompatibilní s prohlížečem funkcí, jako jsou styly, rozložení a tak dále.
- Kompatibilní s žádný jazyk podporuje modulu .NET CLR, jako je například Microsoft Visual Basic a Microsoft Visual C#.
- Založený na rozhraní Microsoft .NET Framework. To poskytuje všechny výhody rozhraní, včetně spravovaném prostředí, bezpečnost typů a dědičnosti.
- Je flexibilní, protože můžete přidat uživatele vytvořit a k nim ovládací prvky třetích stran.

**Nabídka webových formulářů ASP.NET:** 

- Oddělení HTML a jiný kód uživatelského rozhraní od logiku aplikace.
- Bohatá sada serverové ovládací prvky pro běžné úkoly, včetně přístupu k datům.
- Výkonné datové vazby, s podporou skvělý nástroj.
- Podpora pro skriptování na straně klienta, který se spustí v prohlížeči.
- Podpora pro celou řadu dalších funkcí, včetně směrování, zabezpečení, výkon, mezinárodní prostředí, testování, ladění, zpracování chyb a správy stavu.

## <a name="aspnet-web-forms-helps-you-overcome-challenges"></a>Webové formuláře ASP.NET umožňuje předcházet problémům, které

Programování webové aplikace představuje výzvy, které obvykle nevznikají při programování tradiční klientských aplikací. Mezi výzvy patří:

- **Implementace bohaté webové uživatelské rozhraní** – může být složité a zdlouhavé pro návrh a implementaci uživatel rozhraní pomocí základních funkcí HTML, zejména v případě, že stránka má komplexní rozložení, velké množství dynamický obsah a plně funkční interaktivní uživatelské objekty.
- **Oddělení klienta a serveru** -ve webové aplikaci klienta (prohlížeč) a serveru jsou různé aplikace často spuštěné v různých počítačích (a to i v různých operačních systémů). V důsledku toho dvě poloviny aplikace sdílejí velmi málo informací; budou komunikovat, ale obvykle exchange jenom malé bloky jednoduché informace.
- **Bezstavové provádění** – když webový server obdrží požadavek stránku ho najde stránky, procesy, odešle do prohlížeče a poté odstraní všechny informace o stránku. Pokud uživatel požaduje stejné stránce znovu, server zopakuje sekvenci celou stránku od začátku. Jinými slovy, má server nedostatek paměti stránek, které zpracoval – stránky jsou bezstavové. Proto pokud aplikace potřebuje ke správě informací o stránce, jeho bezstavové povaze se může stát problém.
- **Možnosti neznámého klienta** – v mnoha případech jsou webové aplikace dostupné pro mnoho uživatelů pomocí různých prohlížečích. Prohlížeče mají různé možnosti, takže je těžké vytvořit aplikaci, která se spustí stejnou měrou i na všech z nich.
- **Komplikace se přístup k datům** -čtení a zápis ke zdroji dat v tradiční webové aplikace může být složité a prostředky.
- **Komplikace se škálovatelností** – v mnoha případech webové aplikace s existujícími metodami selhat neplní cíle škálovatelnosti z důvodu nedostatku kompatibility mezi různými součástmi aplikace. Toto je často společný bod selhání pro aplikace v cyklu vysokého růstu.

Splnění těchto výzev pro webové aplikace může vyžadovat dlouhý čas a úsilí. Webové formuláře ASP.NET a rozhraní ASP.NET vyřešit tyto problémy následujícími způsoby:

- **Objekt intuitivní a konzistentní model** – architektura stránky ASP.NET uvede objektový model, který umožňuje představit si formuláře jako jednotku, ne jako samostatné součásti klienta a serveru. V tomto modelu můžete program stránce intuitivnější způsobem než v tradiční webových aplikací, včetně možnosti nastavit vlastnosti pro prvky stránky a reakce na události. Kromě toho jsou serverových ovládacích prvků ASP.NET abstrakci z fyzického obsahu stránce HTML a přímé interakce mezi prohlížečem a serverem. Obecně platí můžete použít ovládací prvky serveru způsob, jak můžete pracovat s ovládacími prvky v aplikaci klienta a není nutné myslíte o tom, jak vytvořit HTML k dispozici a zpracovat ovládací prvky a jejich obsah.
- **Událostmi řízené programování modelu** -uvedení webových formulářů ASP.NET do webové aplikace známý model zápisu obslužné rutiny události pro události, které nastaly na straně klienta nebo serveru. Stránky ASP.NET abstrahuje tento model tak, že je základní mechanismus zachycení událost na straně klienta, přenosem k serveru a volání odpovídající metodu automatický a neviditelná pro vás. Výsledkem je, vymazat, snadno napsané kód strukturu, která podporuje vývoj založeného na událostech.
- **Řízení stavu intuitivní** – architektura stránky ASP.NET zpracovává automaticky udržovat stav stránku a jeho ovládací prvky a poskytne vám explicitní způsobů, jak udržovat stav informace specifické pro aplikaci. To je prováděno bez velkou využít prostředky serveru a může být implementováno s nebo bez odeslání souborů cookie v prohlížeči.
- **Aplikace prohlížeče nezávislé** – architektura stránky ASP.NET umožňuje vytvářet veškerou logiku aplikace na serveru, takže není nutné explicitně kód rozdíly v prohlížečích. Však stále umožňuje využít výhod funkce specifické pro prohlížeč napsáním kódu na straně klienta zajistit lepší výkon a bohatší možnosti klienta.
- **Podpora modulu runtime jazyka společné rozhraní .NET framework** – architektura stránky ASP.NET je postavený na rozhraní .NET Framework, takže celý framework je k dispozici pro všechny aplikace ASP.NET. Aplikace lze zapsat v libovolném jazyce, který je kompatibilní, který je s modulem runtime. Kromě toho je zjednodušený přístup k datům, pomocí dat přístup infrastrukturu rozhraní .NET Framework, včetně ADO.NET.
- **Výkon serveru škálovatelné rozhraní .NET framework** – architektura stránky ASP.NET umožňuje škálovat webovou aplikaci z jednoho počítače s jedním procesorem na webové farmy s více počítači ještě jednou a bez složitá změny aplikace. logika.

## <a name="features-of-aspnet-web-forms"></a>Funkce webové formuláře ASP.NET

- **Ovládací prvky serveru**– ovládací prvky ASP.NET webového serveru jsou objekty na webových stránek ASP.NET, které jsou spuštěny při požadavku na stránku a že vykreslení značek do prohlížeče. Mnoho ovládací prvky webového serveru jsou podobné známé prvky HTML, jako jsou tlačítka a textová pole. Další ovládací prvky obsahují komplexní chování, například kalendář a ovládacích prvků, které můžete použít k připojení ke zdrojům dat a zobrazovat data.
- **Stránky předlohy**– hlavní stránky ASP.NET umožňují vytvářet konzistentního rozložení pro stránky v aplikaci. Jedna stránka předlohy definuje vzhled a chování a standardní chování, které chcete použít pro všechny stránky (nebo skupinu stránek) ve vaší aplikaci. Potom můžete vytvořit jednotlivé stránky obsahu, které obsahují obsah, který chcete zobrazit. Pokud uživatelé požadují obsahu stránky, sloučení se stránkou předlohy k vytvoření výstupu, který kombinuje rozložení stránky předlohy s obsahem ze stránky obsahu.
- **Práce s daty**-technologie ASP.NET poskytuje mnoho možností pro ukládání a načítání a zobrazení data. V aplikaci webových formulářů ASP.NET které ovládací prvky vázané na data použít k automatizaci prezentace nebo vstupní data v prvky uživatelského rozhraní webové stránky, jako jsou tabulky a textová pole a rozevírací seznamy.
- **Členství**-ASP.NET Identity ukládá přihlašovací údaje uživatelů v databázi vytvořené aplikací. Pokud vaši uživatelé přihlásí, aplikace ověří jejich přihlašovacích údajů načtením databáze. Váš projekt *účet* složka obsahuje soubory, které implementují různých částí členství: registrace, přihlášení, změna hesla a autorizaci přístupu. Kromě toho webových formulářů ASP.NET podporuje OAuth a OpenID. Tato vylepšení ověřování umožňují uživatelům přihlásit se do vaší lokality pomocí stávající přihlašovací údaje z těchto účtů jako Facebook, Twitter, Windows Live a Google. Ve výchozím nastavení vytvoří šablona dílčí databázi členství pomocí výchozí název databáze na instanci systému SQL Server Express LocalDB, vývoj databázový server, který se dodává s Visual Studio Express 2013 pro Web.
- **Klientský skript a klientské rozhraní**-na serveru funkce technologie ASP.NET můžete vylepšit zahrnutím funkce klientského skriptu v stránky webového formuláře ASP.NET. Klientského skriptu můžete použít k poskytnutí bohatší rychleji reagující uživatelské rozhraní pro uživatele. Můžete také klientský skript provádět asynchronní volání na webový server je spuštěn na stránce v prohlížeči.
- **Směrování**-směrování adres URL umožňuje nakonfigurovat aplikaci tak, aby přijímal žádosti adresy URL, které nelze namapovat na fyzické soubory. Adresu URL požadavku je jednoduše adresa URL, uživatel zadá do svého prohlížeče na stránce najít na webu. Používáte směrování k definování adresy URL, které jsou sémanticky smysl pro uživatele a které mohou pomoci s optimalizací vyhledávání (vyhledávací weby SEO).
- **Stav správy**-webových formulářů ASP.NET obsahuje několik možností, které vám pomohou zachovat data na základě stránky a základ celou aplikaci.
- **Zabezpečení**-důležitou součástí vývoj bezpečnější aplikace je pochopit na ohrožení. Společnost Microsoft vyvinula způsob kategorizace hrozeb: falšování identity, úmyslné poškozování, popírání odpovědnosti, zpřístupnění informací, odepření služby, zvýšení úrovně oprávnění (STRIDE). Ve webových formulářů ASP.NET můžete přidat body rozšiřitelnosti a možnosti konfigurace, které vám umožní přizpůsobit různé chování zabezpečení ve webových formulářů ASP.NET.
- **Výkon**-výkon může být ale klíčovým faktorem v úspěšného webu nebo v projektu. Webové formuláře ASP.NET umožňuje upravit výkonu související s stránky a ovládací prvek zpracování serverem, správy stavu, přístup k datům, konfigurace aplikace a načítání a efektivní kódování postupy.
- **Internacionalizace**– webových formulářů ASP.NET umožňuje vytvářet webové stránky, které můžete získat obsahu a další data na základě nastavení upřednostňovaný jazyk pro prohlížeč nebo podle explicitního výběru jazyka uživatele. Obsah a jiná data se označuje jako prostředky a tato data mohou být uložena ve zdrojových souborech nebo jiných zdrojů. Na stránce webových formulářů ASP.NET nakonfigurujete ovládací prvky získat jejich hodnoty vlastností z prostředků. V době běhu výrazy prostředků se nahrazují prostředky ze souboru odpovídající lokalizovaný prostředek.
- **Ladění a zpracování chyb**-ASP.NET obsahuje funkce, které vám pomohou diagnostikovat problémy, které by mohly nastat ve vaší aplikaci webových formulářů. Ladění a zpracování chyb a podporují v rámci webových formulářů ASP.NET, aby vaše aplikace zkompilování a spuštění efektivně.
- **Nasazení a hostitelský**-Visual Studio, ASP.NET, Azure a služby IIS poskytují nástroje, které vám pomohou s procesem nasazení a hostování aplikace webových formulářů.

## <a name="deciding-when-to-create-a-web-forms-application"></a>Rozhodování, kdy vytvořit aplikaci Web Forms

Je třeba důkladně zvážit zda pro webovou aplikaci implementovat pomocí rozhraní ASP.NET Web Forms modelu nebo jiný model, jako je například rozhraní ASP.NET MVC. Architektura MVC nenahrazuje model webových formulářů; Můžete buď framework pro webové aplikace. Předtím, než se rozhodnete použít model webových formulářů nebo rozhraní MVC pro konkrétní web, zvažte výhody obou těchto přístupů.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Výhody webové aplikace webových formulářů

Architektura využívající webové formuláře nabízí následující výhody:

- Podporuje model událostí, který zachovává stav prostřednictvím protokolu HTTP, což je výhodné pro vývoj webových aplikací – obchodní. Aplikace využívající webové formuláře poskytuje desítky událostí, které jsou podporovány stovkami ovládacích prvků serveru.
- Využívá vzor Page Controller, který přidává funkce jednotlivým stránkám. Další informace najdete v tématu [Page Controller](https://go.microsoft.com/fwlink/?LinkId=106359 "Page Controller") na webu MSDN.
- Využívá stav zobrazení ani formuláře umístěné na serveru, které může usnadnit správu informací o stavu.
- Dobře se hodí pro menší týmy webových vývojářů a návrhářů, kteří chtějí využít výhod velkého množství komponent, které jsou k dispozici pro rychlý vývoj aplikací.
- Je obecně méně složitých pro vývoj aplikací, protože komponenty ( **stránky** třídy, ovládací prvky a tak dále) jsou pevně integrovány a obvykle vyžadují méně kódu než MVC model.

### <a name="advantages-of-an-mvc-based-web-application"></a>Výhody založené na MVC webové aplikace

Rozhraní ASP.NET MVC nabízí následující výhody:

- Ho usnadňuje správu složitých vydělením aplikace do modelu, zobrazení a kontroler.
- Nepoužívá se stav zobrazení ani formuláře umístěné na serveru. Díky rozhraní MVC ideální pro vývojáře, kteří chtějí mít plnou kontrolu nad chováním aplikace.
- Využívá vzor Front Controller, který zpracovává požadavky webové aplikace prostřednictvím jediného kontroleru. To umožňuje návrh aplikací podporující infrastrukturu s rozsáhlým směrováním. Další informace najdete v tématu [Front Controller](https://go.microsoft.com/fwlink/?LinkId=106357 "Front Controller") na webu MSDN.
- Poskytuje lepší podporu pro testy řízený vývoj (TDD).
- Funguje dobře pro webové aplikace, které jsou podporovány velkými týmy vývojářů a webových návrhářů, kteří potřebují značnou míru kontroly nad chováním aplikací.
