---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-cs
title: Protokolování podrobností o chybách pomocí knihovny ELMAH (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Chyba protokolování moduly a obslužné rutiny (ELMAH) nabízí jiný přístup k protokolování chyb za běhu v produkčním prostředí. ELMAH je bezplatná open source chybě...
ms.author: aspnetcontent
ms.date: 06/09/2009
ms.assetid: 11f6fe44-64ef-4a38-a3b4-35c7bb992352
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-cs
msc.type: authoredcontent
ms.openlocfilehash: 2432b22bd5dec1668fdb134eaeb92e372062ddda
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834121"
---
<a name="logging-error-details-with-elmah-c"></a>Protokolování podrobností o chybách pomocí knihovny ELMAH (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_14_CS.zip) nebo [stahovat PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial14_ELMAH_cs.pdf)

> Chyba protokolování moduly a obslužné rutiny (ELMAH) nabízí jiný přístup k protokolování chyb za běhu v produkčním prostředí. ELMAH je bezplatný open source Chyba knihovny protokolování, který obsahuje funkce, jako je filtrování chyb a možnost, chcete-li zobrazit v protokolu chyb z webové stránky, jako kanál RSS, nebo ho můžete stáhnout jako soubor CSV. Tento kurz vás provede stažením a konfigurace ELMAH.


## <a name="introduction"></a>Úvod

[Předchozím kurzu](logging-error-details-with-asp-net-health-monitoring-cs.md) prozkoumat ASP. Monitorování systému, která nabízí výstupního pole knihovny pro nahrávání širokou škálu webové události stavu vaší sítě. Mnoho vývojářů pomocí stav monitorování, protokolování a e-mailu podrobnosti o neošetřených výjimek. Existuje však několik problémové body s tímto systémem. Nejprve je nedostatek jakýkoli druh uživatelského rozhraní pro zobrazení informací o protokolovaných událostech. Pokud chcete prohlédnout souhrnné informace o 10 poslední chyby, nebo zobrazit podrobnosti o chybě, která se stalo minulý týden, musíte buď modifikovat prostřednictvím databáze, procházet složky Doručená pošta e-mailu nebo vytvořit webovou stránku, která se zobrazují informace z `aspnet_WebEvent_Events` tabulky.

Další z bolavých míst se soustředí kolem složitost stavu monitorování. Protože monitorování stavu slouží k zaznamenání množství různých událostí, a protože existují širokou škálu možností pro pokyny, jak a kdy se protokolují události, správné konfiguraci systému monitorování stavu může být obtížné úkol. Nakonec jsou problémy s kompatibilitou. Vzhledem k tomu sledování stavu se nejprve přidány ve verzi 2.0 rozhraní .NET Framework, není k dispozici pro starší webové aplikace vytvořené pomocí ASP.NET verzi 1.x. Kromě toho `SqlWebEventProvider` třídy, které jsme použili v předchozím kurzu a podrobnosti o chybě protokoly na databázi, pracuje pouze s databází systému Microsoft SQL Server. Je potřeba vytvořit třídu zprostředkovatele vlastního protokolu, budete chtít protokolování chyb nebo alternativní datové úložiště, například soubor XML nebo Oracle database.

Alternativa k monitorování systému stavu je chyba protokolování moduly a obslužné rutiny (ELMAH), protokolování chyb zdarma, open source systém vytvořil [Atif Aziz](http://www.raboof.com/). Nejdůležitější rozdíl mezi těmito dvěma systémy je schopnost ELAMH pro zobrazení seznamu chyb a podrobnosti o konkrétní chybě z webové stránky a jako informačního kanálu RSS. ELMAH je snazší než monitorování stavu, protože pouze protokoluje chyby konfigurace. Kromě toho ELMAH zahrnuje podporu pro ASP.NET 1.x aplikací ASP.NET 2.0 a technologii ASP.NET 3.5 a dodává s širokou škálu zprostředkovatelů zdroj protokolu.

Tento kurz vás provede jednotlivými kroky při přidávání ELMAH do aplikace ASP.NET. Pusťme se do práce!

> [!NOTE]
> Stav monitorování systému a ELMAH mají své vlastní sady výhody a nevýhody. Neváhejte se zkuste obou systémů a rozhodnout, jaké jeden nejlépe vyhovuje vašim potřebám.


## <a name="adding-elmah-to-an-aspnet-web-application"></a>Přidání knihovny ELMAH do webové aplikace ASP.NET

Integrace ELMAH do nové nebo existující aplikace v ASP.NET je jednoduché a přímočaré proces, který trvá méně než pět minut. Řečeno v kostce zahrnuje čtyři jednoduché kroky:

1. Stáhněte si ELMAH a přidejte `Elmah.dll` sestavení do vaší webové aplikace
2. Registrace na ELMAH z modulů HTTP a obslužné rutiny v `Web.config`,
3. Zadejte možnosti konfigurace pro ELMAH, a
4. V případě potřeby vytvořte zdrojové infrastruktuře protokolu chyb.

Projděme si každý z těchto čtyř kroků, postupně po jednom.

### <a name="step-1-downloading-the-elmah-project-files-and-addingelmahdllto-your-web-application"></a>Krok 1: Stažení ELMAH soubory projektu a přidání`Elmah.dll`do vaší webové aplikace

ELMAH 1.0 BETA 3 (sestavení 10617), nejnovější verze v době psaní, je součástí ke stažení v tomto kurzu. Případně můžete navštívit [ELMAH webu](https://code.google.com/p/elmah/) získat nejnovější verzi nebo stáhněte si zdrojový kód. Extrahovat ELMAH stažení do složky na vaší ploše a vyhledejte soubor sestavení knihovny ELMAH (`Elmah.dll`).

> [!NOTE]
> `Elmah.dll` Soubor se nachází v souboru pro stažení `Bin` složky, která obsahuje podsložky pro různé verze rozhraní .NET Framework a sestavení pro vydání a ladění. Pro verzi rozhraní framework vhodné používejte sestavení pro vydání. Například, pokud vytváříte webovou aplikaci ASP.NET 3.5, zkopírujte `Elmah.dll` soubor `Bin\net-3.5\Release` složky.


V dalším kroku spuštění sady Visual Studio a přidejte sestavení do projektu kliknutím pravým tlačítkem na název webu v Průzkumníku řešení a zvolíte možnost Přidat odkaz v místní nabídce. Tím se zobrazí dialogové okno Přidat odkaz. Přejděte na kartu Procházet a zvolte `Elmah.dll` souboru. Tato akce přidá `Elmah.dll` souborů do webové aplikace `Bin` složky.

> [!NOTE]
> Typ projektu webových aplikací (WAP) se nezobrazují `Bin` složku v Průzkumníku řešení. Místo toho uvádí seznam těchto položek ve složce odkazy.


`Elmah.dll` Sestavení zahrnuje třídy, které využívá systém ELMAH. Tyto třídy se dělí do tři kategorií:

- **Z modulů HTTP** – modul HTTP je třída, která definuje obslužné rutiny událostí pro `HttpApplication` události, například `Error` událostí. ELMAH zahrnuje více modulů HTTP, ta největší podstatný tři se: 

    - `ErrorLogModule` -zaznamená do protokolu zdroje neošetřených výjimek.
    - `ErrorMailModule` -odešle podrobnosti k neošetřené výjimce v e-mailovou zprávu.
    - `ErrorFilterModule` -vztahuje filtrů určených pro vývojáře k určení, jaké výjimky jsou protokolovány a co těch, které jsou ignorovány.
- **Obslužné rutiny HTTP** – obslužné rutiny HTTP je třída, která je zodpovědná za generování značky pro konkrétní typ požadavku. ELMAH obsahuje obslužné rutiny HTTP, která vykreslit podrobnosti o chybě jako webovou stránku, jako kanál RSS nebo jako soubor s oddělovači (CSV).
- **Zdroje chyb protokolu** – okamžité ELMAH můžete protokolovat chyby do paměti k databázi serveru Microsoft SQL Server pro databázi aplikace Microsoft Access k databázi Oracle do souboru XML, databázi SQLite, nebo do databáze Vista DB. Jako je stav monitorování systému bylo vytvořeno ELMAH vaší architektury prostřednictvím podle modelu poskytovatele, což znamená, že můžete vytvořit a bezproblémově integrovat vlastní zprostředkovatele zdroje vlastního protokolu v případě potřeby.

### <a name="step-2-registering-elmahs-http-module-and-handler"></a>Krok 2: Registrace modulu HTTP služby a obslužné rutiny ELMAH.

Zatímco `Elmah.dll` soubor obsahuje moduly protokolu HTTP a obslužná rutina potřebné k protokolování neošetřených výjimek automaticky a chcete-li zobrazit podrobnosti o chybě z webové stránky, je nutné je explicitně zaregistrovat v konfiguraci webové aplikace. `ErrorLogModule` Modulu HTTP, po registraci se přihlásí k odběru `HttpApplication`společnosti `Error` událostí. Vždy, když se tato událost je aktivována `ErrorLogModule` zaznamená do zadané zdrojové podrobnosti o výjimce. Uvidíme, jak definovat poskytovatel správy zdrojových protokolu v další části "Konfigurace ELMAH." `ErrorLogPageFactory` Pro vytváření obslužných rutin HTTP je zodpovědný za generování kódu při prohlížení v protokolu chyb z webové stránky.

Syntaxe specifické pro registraci modulů HTTP a obslužné rutiny, závisí na webový server, který je dostupné webu. Pro vývojový Server ASP.NET a společnosti Microsoft IIS 6.0 a starší verze modulů HTTP a obslužné rutiny jsou registrovány v `<httpModules>` a `<httpHandlers>` oddíly, které se zobrazí v rámci `<system.web>` elementu. Pokud používáte IIS 7.0, musí být zaregistrovaný ve `<system.webServer>` elementu `<modules>` a `<handlers>` oddíly. Naštěstí můžete definovat HTTP moduly a obslužné rutiny v *obě* umístí bez ohledu na webový server používá. Tato možnost je většina přenosných jako umožňuje stejnou konfiguraci pro použití ve vývojovém a produkčním prostředí bez ohledu na webový server používá.

Začněte tím, že registrace `ErrorLogModule` modulu HTTP služby a `ErrorLogPageFactory` obslužná rutina HTTP v `<httpModules>` a `<httpHandlers>` tématu `<system.web>`. Pokud vaše konfigurace už definuje tyto dva prvky pak jednoduše zahrnout `<add>` – element pro modul HTTP služby a obslužné rutiny na ELMAH.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample1.xml)]

V dalším kroku registrovat modul HTTP služby a obslužné rutiny v ELMAH společnosti `<system.webServer>` elementu. Stejně jako dříve Pokud tento prvek již není k dispozici v konfiguraci pak ho přidáte.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample2.xml)]

Ve výchozím nastavení, služby IIS 7 si bude stěžovat na Pokud HTTP moduly a obslužné rutiny jsou registrovány v `<system.web>` oddílu. `validateIntegratedModeConfiguration` Atribut `<validation>` element dává pokyn IIS 7 můžete potlačit tyto chybové zprávy.

Všimněte si, že syntaxe pro registraci `ErrorLogPageFactory` obsahuje obslužné rutiny HTTP `path` atribut, který je nastaven na `elmah.axd`. Tento atribut informuje webovou aplikaci, že pokud dorazí požadavek na stránku s názvem `elmah.axd` pak by měl zpracovat požadavek `ErrorLogPageFactory` obslužná rutina HTTP. Uvidíme, `ErrorLogPageFactory` obslužná rutina HTTP v akci později v tomto kurzu.

### <a name="step-3-configuring-elmah"></a>Krok 3: Konfigurace ELMAH

ELMAH vypadá jeho možnosti konfigurace na webu `Web.config` soubor s názvem vlastního konfiguračního oddílu `<elmah>`. Chcete-li použít vlastní části v `Web.config` jej nejprve musí být definován v `<configSections>` elementu. Otevřít `Web.config` a přidejte následující kód k `<configSections>`:

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample3.xml)]

> [!NOTE]
> Pokud konfigurujete ELMAH pro aplikaci ASP.NET 1.x odeberte `requirePermission="false"` atribut z `<section>` prvků uvedených výše.


Výše uvedené syntaxi zaregistruje vlastní `<elmah>` oddíl a všechny dílčí oddíly: `<security>`, `<errorLog>`, `<errorMail>`, a `<errorFilter>`.

V dalším kroku přidejte `<elmah>` části `Web.config`. V této části by se měla objevit na stejné úrovni jako `<system.web>` elementu. Uvnitř `<elmah>` přidat oddíl `<security>` a `<errorLog>` oddíly takto:

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample4.xml)]

`<security>` Oddílu `allowRemoteAccess` atribut označuje, zda je povolen vzdálený přístup. Pokud tato hodnota nastavena na hodnotu 0, pak Chyba protokolu webové stránky lze zobrazit pouze místně. Pokud tento atribut je nastaven na hodnotu 1 je povoleno Chyba protokolu webové stránky pro vzdálené a místní uživatelé. Teď můžeme zakázat webová stránka Chyba protokolu pro vzdálené návštěvníků. Jsme vám umožňují vzdálený přístup vyšší Jakmile budeme mít příležitost k zajištění zabezpečení tohoto postupu.

`<errorLog>` Oddíl definuje zdroj protokolu chyb, které určují, kde se zaznamenávají podrobnosti o chybě; je podobný `<providers>` oddíl ve stavu systému sledování. Určuje výše uvedené syntaxi `SqlErrorLog` třídu jako zdroj protokolu chyb, která protokoluje chyby do databáze Microsoft SQL Server určeném `connectionStringName` hodnotu atributu.

> [!NOTE]
> ELMAH se dodává s poskytovateli další chybové protokolu, které slouží k protokolování chyb do souboru XML, databáze Microsoft Access, Oracle database a dalšími datovými úložišti. Odkazovat na ukázku `Web.config` soubor, který je součástí knihovny ELMAH stažení informace o tom, jak používat tyto alternativní chyba zprostředkovatele protokolu.


### <a name="step-4-creating-the-error-log-source-infrastructure"></a>Krok 4: Vytvoření zdrojové infrastruktuře protokolu chyba

Společnosti ELMAH `SqlErrorLog` poskytovatele protokoluje podrobné informace o chybě k zadané databázi serveru Microsoft SQL Server. `SqlErrorLog` Poskytovatele očekává, že tato databáze obsahuje tabulku s názvem `ELMAH_Error` a tři uložených procedur komponentami TableAdapter: `ELMAH_GetErrorsXml`, `ELMAH_GetErrorXml`, a `ELMAH_LogError`. Stažení ELMAH obsahuje soubor s názvem `SQLServer.sql` v `db` složku, která obsahuje T-SQL pro vytvoření této tabulky a tyto uložené procedury. Budete potřebovat ke spuštění těchto příkazů na databázi použít `SqlErrorLog` zprostředkovatele.

**Obrázek 1** a **2** zobrazení Průzkumníka databáze v sadě Visual Studio po databázové objekty vyžadované `SqlErrorLog` byly přidány zprostředkovatele.

[![](logging-error-details-with-elmah-cs/_static/image2.png)](logging-error-details-with-elmah-cs/_static/image1.png)

**Obrázek 1**: `SqlErrorLog` poskytovatele protokoluje chyby do `ELMAH_Error` tabulky

[![](logging-error-details-with-elmah-cs/_static/image4.png)](logging-error-details-with-elmah-cs/_static/image3.png)

**Obrázek 2**: `SqlErrorLog` poskytovatel použije tři uložených procedur

## <a name="elmah-in-action"></a>ELMAH v akci

V tuto chvíli jsme přidali ELMAH do webové aplikace zaregistrované `ErrorLogModule` modulu HTTP služby a `ErrorLogPageFactory` obslužnou rutinu HTTP, možností ELMAH vaší konfigurace v `Web.config`a přidá potřebné databázových objektů pro `SqlErrorLog` Zprostředkovatel protokolu chyb. Nyní jsme připraveni v akci najdete v článku ELMAH! Navštivte web knihy recenze a přejděte na stránku, která vygeneruje chybu modulu runtime, jako například `Genre.aspx?ID=foo`, nebo neexistující stránku, jako například `NoSuchPage.aspx`. Co se zobrazí při návštěvě těchto stránek závisí `<customErrors>` konfigurace a zda navštívený místně nebo vzdáleně. (Vrátit zpět k [ *zobrazení vlastní chybové stránky* kurzu](displaying-a-custom-error-page-cs.md) pro aktualizační program v tomto tématu.)

ELMAH nemá vliv, jaký obsah se zobrazí uživateli, když dojde k neošetřené výjimce; zaznamená pouze jeho podrobnosti. Tento protokol chyb je přístupný z webové stránky `elmah.axd` z kořenového adresáře vašeho webu, jako například `http://localhost/BookReviews/elmah.axd`. (Tento soubor neexistuje fyzicky ve vašem projektu, ale v případě, že žádost je k dispozici ve pro `elmah.axd` ji odešle modul runtime `ErrorLogPageFactory` obslužnou rutinu HTTP, který generuje kód odesílaných zpět do prohlížeče.)

> [!NOTE]
> Můžete také použít `elmah.axd` stránky dáte pokyn, aby ELMAH generovat chybu testu. Navštívit `elmah.axd/test` (jako v `http://localhost/BookReviews/elmah.axd/test`) způsobí, že ELMAH výjimku typu `Elmah.TestException`, který má chybová zpráva: "Toto je test výjimky, která můžete bezpečně ignorovat."


**Obrázek 3** zobrazuje v protokolu chyb při návštěvě `elmah.axd` z vývojového prostředí.

[![](logging-error-details-with-elmah-cs/_static/image6.png)](logging-error-details-with-elmah-cs/_static/image5.png)

**Obrázek 3**: `Elmah.axd` zobrazí v protokolu chyb z webové stránky  
([Kliknutím ji zobrazíte obrázek v plné velikosti](logging-error-details-with-elmah-cs/_static/image7.png))

Chyba přihlášení **obrázek 3** obsahuje šest položky chyby. Každá položka obsahuje stavový kód HTTP (404 nebo 500 pro tyto chyby), typ, popis, jméno přihlášeného uživatele, když došlo k chybě a datum a čas. Kliknutím na odkaz Podrobnosti se zobrazí stránka, která obsahuje stejnou chybovou zprávu zobrazenou v chybě podrobnosti žlutý obrazovky z smrti (naleznete v tématu **obrázek 4**) společně s hodnotami serverových proměnných v době chyby (viz  **Obrázek 5**). Můžete také zobrazit nezpracovaném kódu XML ve kterém jsou uloženy podrobnosti o chybě, který obsahuje další informace, jako jsou hodnoty v hlavičce HTTP POST.

[![](logging-error-details-with-elmah-cs/_static/image9.png)](logging-error-details-with-elmah-cs/_static/image8.png)

**Obrázek 4**: Zobrazit podrobnosti o chybě YSOD  
([Kliknutím ji zobrazíte obrázek v plné velikosti](logging-error-details-with-elmah-cs/_static/image10.png))

[![](logging-error-details-with-elmah-cs/_static/image12.png)](logging-error-details-with-elmah-cs/_static/image11.png)

**Obrázek 5**: zkoumat hodnoty proměnné kolekce serveru v době chyby  
([Kliknutím ji zobrazíte obrázek v plné velikosti](logging-error-details-with-elmah-cs/_static/image13.png))

Nasazení do produkčního webu ELMAH zahrnuje:

- Kopírování `Elmah.dll` do souboru `Bin` složky v produkčním prostředí
- Kopírování nastavení konfigurace specifické pro ELMAH `Web.config` soubor používat v produkčním prostředí a
- Chyba protokolu zdrojové infrastruktuře přidává do provozní databáze.

Prozkoumali jsme techniky pro kopírování souborů z vývojového do produkčního prostředí v předchozích kurzech. Možná je nejjednodušší způsob, jak získat zdrojové infrastruktuře protokolu chyb na provozní databáze použít SQL Server Management Studio k připojení k produkční databázi a následné provádění `SqlServer.sql` souboru skriptu, který vytvoří potřebné tabulky a uložené postupy.

### <a name="viewing-the-error-details-page-on-production"></a>Zobrazení podrobností chybové stránky v produkčním prostředí

Po nasazení webu do produkčního prostředí, přejděte na produkčního webovou stránku a generovat neošetřenou výjimku. Jako vývojové prostředí ELMAH nemá žádný vliv na stránce chyba zobrazí, když dojde k neošetřené výjimce; Místo toho pouze zaznamená chybu. Při pokusu o návštěvu chybovou stránku protokolu (`elmah.axd`) z produkčního prostředí vám bude se vám stránka zakázáno v **obrázek 6**.

[![](logging-error-details-with-elmah-cs/_static/image15.png)](logging-error-details-with-elmah-cs/_static/image14.png)

**Obrázek 6**: ve výchozím nastavení, nelze zobrazit vzdálené návštěvníků webové stránky chyba protokolu  
([Kliknutím ji zobrazíte obrázek v plné velikosti](logging-error-details-with-elmah-cs/_static/image16.png))

Vzpomeňte si, že v konfiguraci ELMAH `<security>` části jsme nastavili `allowRemoteAccess` atribut na hodnotu 0, která vzdáleným uživatelům zabrání v prohlížení v protokolu chyb. Je třeba zakázat anonymní uživatelé zobrazit v protokolu chyb jako podrobnosti o chybě může odhalit slabá místa zabezpečení nebo jiné citlivé informace. Pokud se rozhodnete tento atribut nastavte na hodnotu 1 a povolte vzdálený přístup v protokolu chyb, je potřeba uzamknutí `elmah.axd` cestu tak, který pouze oprávnění uživatelé k němu máte přístup. Toho lze dosáhnout tak, že přidáte `<location>` elementu `Web.config` souboru.

Následující Konfigurace povoluje jenom uživatelé v roli správce pro přístup k webové stránce chyba protokolu:

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample5.xml)]

> [!NOTE]
> Přidání role správce a tři uživatele v systému – Scott, Jisun a Alice - v [ *konfigurace na webu, že používá aplikační služby* kurzu](configuring-a-website-that-uses-application-services-cs.md). Scott uživatelů a Jisun jsou členové s rolí správce. Další informace o ověřování a autorizace, najdete v mé [kurzy o zabezpečení webu](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


V protokolu chyb v produkčním prostředí můžete nyní zobrazit vzdálení uživatelé; Vraťte se do **obrázky 3**, **4**, a **5** snímky obrazovky chybové stránky webových protokolů. Ale pokud anonymní nebo bez oprávnění správce. uživatel se pokusí zobrazit chybovou stránku protokolu je automaticky přesměrován na stránku pro přihlášení (`Login.aspx`), jako **obrázek 7** ukazuje.

[![](logging-error-details-with-elmah-cs/_static/image18.png)](logging-error-details-with-elmah-cs/_static/image17.png)

**Obrázek 7**: neoprávnění uživatelé jsou automaticky přesměrováni na stránku pro přihlášení  
([Kliknutím ji zobrazíte obrázek v plné velikosti](logging-error-details-with-elmah-cs/_static/image19.png))

### <a name="programmatically-logging-errors"></a>Protokolování chyb prostřednictvím kódu programu

Společnosti ELMAH `ErrorLogModule` modulu HTTP služby automaticky zaznamenává neošetřené výjimky do zdroje zadaného protokolu. Alternativně můžete zaznamenat chybu bez nutnosti, aby se vyvolala nezpracovanou výjimku pomocí `ErrorSignal` třídy a jeho `Raise` metoda. `Raise` Metodě je předána `Exception` objektu a přihlásí jej jako by měl byla vyvolána tuto výjimku a bylo dosaženo modul runtime ASP.NET bez zpracování. Rozdíl je ale, že žádost pokračuje v provádění po obvykle `Raise` byla volána metoda, že k této výjimce dojde, neošetřené výjimky přerušení běžného provedení žádosti a způsobí, že modul runtime ASP.NET zobrazíte nakonfigurované chybová stránka.

`ErrorSignal` Třída je užitečná v situacích, kdy se některé akce, který může selhat, ale není katastrofickými celé operace právě probíhá jeho selhání. Například web může obsahovat formulář, který přijímá vstup uživatele, uloží je v databázi a pak pošle uživateli e-mailu informuje o tom, že informace zpracovala. Co se stane při splnění informace byla úspěšně uložena do databáze, ale dojde k chybě při odesílání e-mailové zprávě? Jednou z možností je vytvořit výjimku a uživatele poslat na chybovou stránku. Však to může být matoucí uživatele do myšlení, který nebyl uložen, informace, které zadal. Další možností je protokolovat chyby související s e-mailu, ale nezmění uživatelské prostředí žádným způsobem. Tady `ErrorSignal` třída je užitečná.

[!code-csharp[Main](logging-error-details-with-elmah-cs/samples/sample6.cs)]

## <a name="error-notification-via-email"></a>Oznámení o chybě e-mailem

Spolu s protokolování chyb do databáze ELMAH lze také nastavit pro zadaného příjemce e-mailem poslat podrobnosti o chybě. Tato funkce je poskytována `ErrorMailModule` modulu HTTP služby; proto je nutné zaregistrovat tento modul HTTP v `Web.config` aby bylo možné odesílat podrobnosti o chybě e-mailem.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample7.xml)]

V dalším kroku zadejte informace o chybě e-mailu v `<elmah>` elementu `<errorMail>` části označující, odesílatel a příjemce, předmět, v e-mailu a určuje, zda e-mail je odeslán asynchronně.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample8.xml)]

Pomocí výše uvedených nastavení na místě, vždy, když chyba za běhu vyvolá ELMAH odešle e-mailu support@example.com se podrobnosti o chybě. E-mailu společnosti ELMAH chyba obsahuje stejné informace zobrazené v chybě podrobnosti webové stránky, konkrétně chybové zprávy, trasování zásobníku a proměnných serveru (vrátit zpět k **4 obrázky** a **5**). Chyba e-mailu také zahrnuje obsah výjimky podrobnosti žlutý obrazovka smrti jako přílohu (`YSOD.html`).

**Obrázek 8** ukazuje na ELMAH chyba e-mail vytvořené návštěvou `Genre.aspx?ID=foo`. Zatímco **obrázek 8** zobrazuje pouze chybová zpráva nebo trasování zásobníku, serverových proměnných jsou uvedeny dále v textu v e-mailu.

[![](logging-error-details-with-elmah-cs/_static/image21.png)](logging-error-details-with-elmah-cs/_static/image20.png)

**Obrázek 8**: můžete nakonfigurovat ELMAH odesílat podrobnosti o chybě e-mailem  
([Kliknutím ji zobrazíte obrázek v plné velikosti](logging-error-details-with-elmah-cs/_static/image22.png))

## <a name="only-logging-errors-of-interest"></a>Jenom protokolování chyb, které vás zajímají

Ve výchozím nastavení protokoly ELMAH podrobnosti o každé neošetřená výjimka, včetně 404 a další chyby protokolu HTTP. Můžete dát pokyn ELMAH ignorovat tyto nebo jiných druhů chyb pomocí filtrování chyb. Logiku filtrování se provádí pomocí knihovny ELMAH společnosti `ErrorFilterModule` modulu HTTP, které budete muset zaregistrovat v `Web.config` Chcete-li použít logiku filtrování. Pravidla pro filtrování jsou určené v `<errorFilter>` oddílu.

Následující kód nastaví ELMAH protokolu chyby 404.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample9.xml)]

> [!NOTE]
> Nezapomeňte, chcete-li použít filtrování chyb je potřeba se zaregistrovat `ErrorFilterModule` modulu HTTP služby.


`<equal>` Element v rámci `<test>` oddíl se označuje jako kontrolní výraz. Pokud se výraz vyhodnotí jako true, chyba filtruje z vaší ELMAH protokolu. Nejsou k dispozici, včetně další kontrolní výrazy: `<greater>`, `<greater-or-equal>`, `<not-equal>`, `<lesser>`, `<lesser-or-equal>`, a tak dále. Můžete také kombinovat pomocí kontrolních výrazů `<and>` a `<or>` logické operátory. A co víc můžete dokonce obsahovat jednoduchý výraz jazyka JavaScript jako kontrolní výraz nebo napsat vlastní kontrolní výrazy v jazyce C# nebo Visual Basic.

Další informace o chybě na ELMAH možnosti filtrování, najdete [filtrování chyb části](https://code.google.com/p/elmah/wiki/ErrorFiltering) v [ELMAH wiki](https://code.google.com/p/elmah/w/list).

## <a name="summary"></a>Souhrn

ELMAH poskytuje jednoduché, ale výkonný mechanismus pro protokolování chyb ve webové aplikaci ASP.NET. Stejně jako systém monitorování stavu od Microsoftu ELMAH chyby můžete připojit k databázi a podrobnosti o chybě poslat vývojář e-mailem. Na rozdíl od monitorování systému stavu ELMAH zahrnuje podporu pro používání nástroje většímu počtu úložišť dat protokolu chyb, včetně: Microsoft SQL Server, Microsoft Access, Oracle, soubory XML a několik dalších. Kromě toho nabízí ELMAH předdefinovaný mechanismus pro zobrazení v protokolu chyb a podrobnosti o konkrétní chybě z webové stránky, `elmah.axd`. `elmah.axd` Stránky může také vykreslit informace o chybě jako kanál RSS nebo jako soubor hodnot oddělených čárkami (CSV), který si můžete přečíst pomocí aplikace Microsoft Excel. Můžete také dát pokyn ELMAH filtrování chyb z protokolu pomocí deklarativní a programová kontrolní výrazy. A ELMAH jde použít s aplikací ASP.NET verzi 1.x.

Každé nasazené aplikace by měla mít určitý mechanismus pro automatické protokolování neošetřené výjimky a odesílání oznámení do vývojového týmu. Určuje, zda to lze provést pomocí monitorování stavu nebo ELMAH je sekundární. Jinými slovy nebude vadit, ve skutečnosti mnohem ať už používáte monitorování stavu nebo ELMAH; Vyhodnocení obou systémů a pak zvolte ten, který nejlépe vyhovuje vašim potřebám. Co je zásadně důležité je, že některé mechanismus být umístěny na místě k protokolování neošetřených výjimek v provozním prostředí.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [ELMAH – chyba protokolovací moduly a obslužné rutiny](http://dotnetslackers.com/articles/aspnet/ErrorLoggingModulesAndHandlers.aspx)
- [Stránka projektu ELMAH](https://code.google.com/p/elmah/) (zdrojového kódu, ukázek, wiki)
- [Zapojení ELMAH do webové aplikace chcete zachytávat neošetřené výjimky](http://screencastaday.com/ScreenCasts/43_Plugging_Elmah_into_Web_Application_to_Catch_Unhandled_Exceptions.aspx) (video)
- [Stránky protokolů chyb zabezpečení](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)
- [Použití modulů HTTP a obslužné rutiny k vytvoření komponentů modulární ASP.NET](https://msdn.microsoft.com/library/aa479332.aspx)
- [Kurzy zabezpečení webu](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Předchozí](logging-error-details-with-asp-net-health-monitoring-cs.md)
> [další](precompiling-your-website-cs.md)
