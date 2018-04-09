---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-cs
title: Podrobnosti o chybě protokolování s ELMAH (C#) | Microsoft Docs
author: rick-anderson
description: Chyba protokolování moduly a obslužné rutiny (ELMAH) nabízí další způsob, jak protokolování chyby za běhu v produkčním prostředí. ELMAH je bezplatný chyba...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 11f6fe44-64ef-4a38-a3b4-35c7bb992352
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-cs
msc.type: authoredcontent
ms.openlocfilehash: cd91c745624f09d01a326a445bea2bb756576688
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="logging-error-details-with-elmah-c"></a>Podrobnosti o chybě protokolování s ELMAH (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_14_CS.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial14_ELMAH_cs.pdf)

> Chyba protokolování moduly a obslužné rutiny (ELMAH) nabízí další způsob, jak protokolování chyby za běhu v produkčním prostředí. ELMAH je bezplatný Chyba protokolování knihovny a obsahuje funkce, jako je filtrování chyb a možnost chcete zobrazit v protokolu chyb z webové stránky, jako informačního kanálu RSS, nebo ho stáhnout jako soubor s položkami oddělenými čárkou. Tento kurz vás provede stahování a konfigurace ELMAH.


## <a name="introduction"></a>Úvod

[Předchozí kurzu](logging-error-details-with-asp-net-health-monitoring-cs.md) zkontrolován ASP. NET na stav systému, který nabízí out of knihovně pole pro záznam širokou škálu webové události pro monitorování. Celá řada vývojářů pomocí protokolu a e-mailových podrobnosti o neošetřených výjimek sledování stavu. Existuje však několik problémové body s tímto systémem. Především je nedostatek žádné řazení uživatelské rozhraní pro zobrazení informací o protokolu událostí. Pokud chcete zobrazit souhrn 10 poslední chyby, nebo zobrazení podrobností o chybu, která se stalo poslední týden, musíte buď vkládat informace prostřednictvím databáze, kontrola prostřednictvím e-mailu doručené pošty nebo vytvořit webovou stránku, která se zobrazují informace z `aspnet_WebEvent_Events` tabulky.

Jiný bod problémové soustředí kolem složitost stavu monitorování. Protože sledování stavu je možné použít k zaznamenání nadbytku různé událostí a protože nejsou k dispozici různé možnosti pro instruující, jak a kdy se protokolují události, správně konfigurace sledování systému stavu může být obtížné úloh. Nakonec jsou problémy s kompatibilitou. Sledování stavu byl přidán první rozhraní .NET Framework verze 2.0, není k dispozici pro starší webové aplikace vyvíjené v technologii ASP.NET verze 1.x. Kromě toho `SqlWebEventProvider` třídy, která jsme použili v předchozí kurzu na podrobnosti o chybě protokoly na databázi, pracuje pouze s databází systému Microsoft SQL Server. Musíte pro vytvoření třídy zprostředkovatele vlastního protokolu, bude nutné protokolovat chyby k úložišti alternativní dat, například soubor XML nebo databáze Oracle.

Alternativu ke stavu systému pro monitorování je chyba protokolování moduly a obslužné rutiny (ELMAH), volné, open-source Chyba protokolování systému vytvořených [Atif Aziz](http://www.raboof.com/). Nejdůležitější rozdíl mezi těmito dvěma systémy je ELAMH na možnost zobrazit seznam chyb a podrobnosti o konkrétní chybě z webové stránky a jako informačního kanálu RSS. ELMAH je jednodušší než sledování stavu, protože jenom protokoluje chyby konfigurace. Kromě toho ELMAH zahrnuje podporu pro ASP.NET 1.x aplikací ASP.NET 2.0 a technologii ASP.NET 3.5 a se dodává s řadou zprostředkovatele protokolu zdrojů.

Tento kurz vás provede jednotlivými kroky při přidávání ELMAH do aplikace ASP.NET. Můžeme začít!

> [!NOTE]
> Stav monitorování systému a ELMAH mají své vlastní sady výhody a nevýhody. I doporučujeme, abyste zkuste obou systémů a rozhodnout, jaké jeden nejlépe vyhovuje vašim potřebám.


## <a name="adding-elmah-to-an-aspnet-web-application"></a>Přidání ELMAH do webové aplikace ASP.NET

Integrace ELMAH do nové nebo existující aplikace ASP.NET je proces snadno a přehledné, který trvá v části pět minut. Stručně řečeno zahrnuje čtyři jednoduchých kroků:

1. Stáhnout ELMAH a přidat `Elmah.dll` sestavení do vaší webové aplikace
2. Registrovat ELMAH na vytváření modulů HTTP a obslužné rutiny v `Web.config`,
3. Zadejte možnosti konfigurace pro ELMAH, a
4. Chyba protokolu zdrojové infrastruktuře, vytvořte v případě potřeby.

Projděme každý z těchto čtyř kroků, po jednom.

### <a name="step-1-downloading-the-elmah-project-files-and-addingelmahdllto-your-web-application"></a>Krok 1: Stahování souborů projektu ELMAH a přidání`Elmah.dll`k vaší webové aplikaci

ELMAH 1.0 BETA 3 (sestavení 10617), poslední verze v době psaní, je součástí ke stažení, které jsou k dispozici v tomto kurzu. Případně můžete navštívit [ELMAH webu](https://code.google.com/p/elmah/) získat nejnovější verzi nebo ke stažení ve zdrojovém kódu. Extrahování ELMAH stahování do složky na pracovní ploše a vyhledejte soubor sestavení ELMAH (`Elmah.dll`).

> [!NOTE]
> `Elmah.dll` Soubor je umístěný ve ke stažení `Bin` složky, která obsahuje podsložky pro různé verze rozhraní .NET Framework a pro verzi a ladění sestavení. Použijte sestavení pro vydání pro příslušnou verzi. Například pokud vytváříte webovou aplikaci ASP.NET 3.5, zkopírujte `Elmah.dll` souboru z `Bin\net-3.5\Release` složky.


Dále otevřete Visual Studio a sestavení do projektu přidejte kliknutím pravým tlačítkem na název webu v Průzkumníku řešení a výběrem možnosti Přidat odkaz v místní nabídce. Otevře dialogové okno Přidat odkaz. Přejděte na kartu Procházet a vyberte `Elmah.dll` souboru. Tato akce přidá `Elmah.dll` souboru k webové aplikaci `Bin` složky.

> [!NOTE]
> Typ projektu webové aplikace (WAP) nejsou zobrazeny `Bin` složky v Průzkumníku řešení. Místo toho uvádí tyto položky ve složce odkazy.


`Elmah.dll` Sestavení obsahuje třídy používané systémem ELMAH. Tyto třídy spadají do tří kategorií:

- **Vytváření modulů HTTP v** -modulu HTTP je třída, která definuje obslužné rutiny události pro `HttpApplication` události, například `Error` událostí. ELMAH obsahuje více modulů HTTP, tři nejvíce podstatný ty, které se: 

    - `ErrorLogModule` -Zdroj protokolu v protokolech neošetřených výjimek.
    - `ErrorMailModule` -odešle podrobnosti k neošetřené výjimce v e-mailovou zprávu.
    - `ErrorFilterModule` -vztahuje zadaný vývojáře filtrů určit, jaké výjimky jsou protokolovány a co ty, které jsou ignorovány.
- **Obslužné rutiny HTTP** – obslužné rutiny HTTP je třída, která je zodpovědná za generování kódu pro konkrétní typ požadavku. ELMAH zahrnuje obslužné rutiny HTTP, která vykreslit podrobnosti o chybě jako webovou stránku, jako informačního kanálu RSS nebo jako soubor s oddělovači (CSV).
- **Chyba protokolu zdroje** – předinstalované ELMAH můžete protokolovat chyby do paměti, do databáze Microsoft SQL Server, k databázi Microsoft Access k databázi Oracle do souboru XML, databáze SQLite, nebo do databáze Vista DB. Architektura ELMAH na jako je stav monitorování systému, bylo vytvořeno pomocí zprostředkovatele modelu, což znamená, že můžete vytvořit a v případě potřeby se hladce integrují zdroj vlastní vlastní protokol zprostředkovatele.

### <a name="step-2-registering-elmahs-http-module-and-handler"></a>Krok 2: Registrace modulu HTTP a obslužné rutiny na ELMAH

Když `Elmah.dll` soubor obsahuje modulů HTTP a obslužné rutiny potřebné k protokolování neošetřených výjimek automaticky a zobrazit podrobnosti o chybě z webové stránky, je nutné je explicitně zaregistrovat v konfiguraci webové aplikace. `ErrorLogModule` Přihlásí k odběru modulu HTTP, po registraci `HttpApplication`na `Error` událostí. Vždy, když se tato událost je aktivována `ErrorLogModule` protokoly podrobnosti výjimky a zdroj zadaného protokolu. Ukážeme, jak definovat poskytovatele správy zdrojového protokolu v další části "Konfigurace ELMAH." `ErrorLogPageFactory` Pro vytváření obslužných rutin HTTP je zodpovědný za generování kód při zobrazení v protokolu chyb z webové stránky.

Specifickou syntaxi pro registraci modulů HTTP a obslužné rutiny závisí na webový server, který je pohánějící webu. Pro vývojový Server ASP.NET a společnosti Microsoft IIS verze 6.0 a starší, jsou v zaregistrované modulů HTTP a obslužné rutiny `<httpModules>` a `<httpHandlers>` oddíly, které se zobrazí v rámci `<system.web>` elementu. Pokud používáte službu IIS 7.0, pak budou muset být zaregistrovaná v `<system.webServer>` elementu `<modules>` a `<handlers>` oddíly. Naštěstí můžete definovat modulů HTTP a obslužné rutiny v *i* umístí bez ohledu na webový server používá. Tato možnost je většina přenosných jeden jako umožňuje, aby se stejnou konfigurací, který se má použít v vývoj a produkční prostředích bez ohledu na webový server používá.

Začněte tím, že registrace `ErrorLogModule` modulu HTTP a `ErrorLogPageFactory` obslužná rutina HTTP v `<httpModules>` a `<httpHandlers>` kapitoly `<system.web>`. Pokud vaše konfigurace již definuje tyto dva prvky pak jednoduše zahrnout `<add>` element pro modul HTTP a obslužné rutiny na ELMAH.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample1.xml)]

V dalším kroku registrovat modul HTTP a obslužné rutiny v ELMAH na `<system.webServer>` elementu. Jako dříve Pokud tento prvek se již nenachází ve vaší konfiguraci přidejte ji.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample2.xml)]

Ve výchozím nastavení, IIS 7 complains, pokud jsou v registrované moduly HTTP a obslužné rutiny `<system.web>` oddílu. `validateIntegratedModeConfiguration` Atribut `<validation>` element dá pokyn IIS 7 k potlačení takové chybové zprávy.

Všimněte si, že syntaxe pro registraci `ErrorLogPageFactory` zahrnuje obslužná rutina HTTP `path` atribut, který je nastaven na `elmah.axd`. Tento atribut informuje webové aplikace, že pokud dorazí požadavek pro stránku s názvem `elmah.axd` pak by měl zpracovat požadavek `ErrorLogPageFactory` obslužnou rutinu HTTP. Ukážeme `ErrorLogPageFactory` obslužná rutina HTTP v akci později v tomto kurzu.

### <a name="step-3-configuring-elmah"></a>Krok 3: Konfigurace ELMAH

ELMAH hledá jeho možnosti konfigurace na webu `Web.config` souborů v části vlastní konfigurace s názvem `<elmah>`. Chcete-li použít vlastní oddíl v `Web.config` ji nejprve musí být definován v `<configSections>` elementu. Otevřete `Web.config` souboru a přidejte následující kód do `<configSections>`:

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample3.xml)]

> [!NOTE]
> Pokud konfigurujete ELMAH pro aplikaci ASP.NET 1.x, odeberte `requirePermission="false"` atribut z `<section>` prvků uvedených výše.


Výše uvedené syntaxe zaregistruje vlastní `<elmah>` části a jeho pododdíly: `<security>`, `<errorLog>`, `<errorMail>`, a `<errorFilter>`.

Dál přidejte `<elmah>` části k `Web.config`. V této části se zobrazí na stejné úrovni jako `<system.web>` elementu. Uvnitř `<elmah>` přidejte část `<security>` a `<errorLog>` části takto:

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample4.xml)]

`<security>` Oddílu `allowRemoteAccess` atribut uvádí, zda je povolen vzdálený přístup. Pokud tato hodnota nastavena na hodnotu 0, pak Chyba protokolu webové stránky lze zobrazit pouze místně. Pokud tento atribut je nastaven na hodnotu 1 je povoleno Chyba protokolu webové stránky pro místní i vzdálené návštěvníky. Nyní umožňuje zakázat Chyba protokolu webové stránky pro vzdálené návštěvníky. Vzdálený přístup jsme budete povolit později po máme příležitost popisují zabezpečení se to udělat.

`<errorLog>` Oddíl definuje zdroj protokolu chyb, které určují, kde se zaznamenávají podrobnosti o chybě; je podobná `<providers>` oddíl ve stavu systému pro monitorování. Určuje výše uvedené syntaxe `SqlErrorLog` třída jako zdroj protokolu chyb, které protokoly chyb pro Microsoft SQL serveru databáze určené parametrem `connectionStringName` hodnota atributu.

> [!NOTE]
> ELMAH se dodává s další chybové zprostředkovatelů protokolu, které lze použít k protokolování chyb do souboru XML, databázi aplikace Microsoft Access, databáze Oracle a jiných úložišť dat. Odkazovat na ukázku `Web.config` soubor, který je součástí stahování ELMAH informace o tom, jak tyto alternativní chyba zprostředkovatele protokolu použít.


### <a name="step-4-creating-the-error-log-source-infrastructure"></a>Krok 4: Vytvoření zdrojové infrastruktuře protokolu chyb

Na ELMAH `SqlErrorLog` zprostředkovatele v protokolech podrobnosti o chybě zadaná databáze serveru Microsoft SQL Server. `SqlErrorLog` Zprostředkovatele očekává tato databáze obsahuje tabulku s názvem `ELMAH_Error` a tři uložené procedury: `ELMAH_GetErrorsXml`, `ELMAH_GetErrorXml`, a `ELMAH_LogError`. Stahování ELMAH obsahuje soubor s názvem `SQLServer.sql` v `db` složku, která obsahuje T-SQL pro vytvoření této tabulky a tyto uložené procedury. Budete potřebovat ke spuštění těchto příkazů na vaše databáze pro použití `SqlErrorLog` zprostředkovatele.

**Obrázky 1** a **2** zobrazení Průzkumníka databáze v sadě Visual Studio po databázové objekty, které vyžaduje `SqlErrorLog` byly přidány zprostředkovatele.

[![](logging-error-details-with-elmah-cs/_static/image2.png)](logging-error-details-with-elmah-cs/_static/image1.png)

**Obrázek 1**: `SqlErrorLog` zprostředkovatele chyby do protokolu `ELMAH_Error` tabulky

[![](logging-error-details-with-elmah-cs/_static/image4.png)](logging-error-details-with-elmah-cs/_static/image3.png)

**Obrázek 2**: `SqlErrorLog` poskytovatele používá tři uložené procedury

## <a name="elmah-in-action"></a>ELMAH v akci

V tuto chvíli jsme přidali ELMAH k webové aplikaci, zaregistrovaný `ErrorLogModule` modulu HTTP a `ErrorLogPageFactory` obslužnou rutinu HTTP, zadaný pro ELMAH možnosti konfigurace v `Web.config`a potřebné databázové objekty pro přidání `SqlErrorLog` Zprostředkovatel protokolu chyb. Nyní připraveni najdete v části ELMAH v akci! Navštivte web knihy recenze a najdete na stránce, který generuje chyba za běhu, jako například `Genre.aspx?ID=foo`, nebo neexistující stránky, jako například `NoSuchPage.aspx`. Co se zobrazí při návštěvě tyto stránek závisí na `<customErrors>` konfigurace a právě navštíveného místně nebo vzdáleně. (Odkazovat zpět [ *zobrazení vlastní chybovou stránku* kurzu](displaying-a-custom-error-page-cs.md) pro aktualizační program v tomto tématu.)

ELMAH nemá vliv, který obsah se zobrazí uživateli, když dojde k neošetřené výjimce; zaznamená pouze její podrobnosti. Tato chyba protokolu je přístupný z webové stránky `elmah.axd` z kořene webu, jako například `http://localhost/BookReviews/elmah.axd`. (Tento soubor neexistuje fyzicky ve vašem projektu, ale když přijde žádost pro `elmah.axd` modulu runtime odešle zprávu, aby `ErrorLogPageFactory` obslužná rutina HTTP, která generuje kód odeslána zpět do prohlížeče.)

> [!NOTE]
> Můžete také `elmah.axd` stránky dáte pokyn, aby ELMAH ke generování Chyba testu. Návštěvou `elmah.axd/test` (jako v `http://localhost/BookReviews/elmah.axd/test`) způsobí, že má být vyvolána výjimka typu ELMAH `Elmah.TestException`, na kterém je chybová zpráva: "Toto je test výjimka, která můžete bezpečně ignorovat."


**Obrázek 3** zobrazuje v protokolu chyb při návštěvě `elmah.axd` z vývojového prostředí.

[![](logging-error-details-with-elmah-cs/_static/image6.png)](logging-error-details-with-elmah-cs/_static/image5.png)

**Obrázek 3**: `Elmah.axd` zobrazí v protokolu chyb z webové stránky  
([Kliknutím zobrazit obrázek v plné velikosti](logging-error-details-with-elmah-cs/_static/image7.png))

V protokolu chyb **obrázek 3** obsahuje šest chyba položky. Každá položka obsahuje stavový kód HTTP (404 nebo 500 pro tyto chyby), typ, popis, jméno přihlášeného uživatele při chybě a datum a čas. Kliknutím na odkaz Podrobnosti zobrazí stránku, která zahrnuje ke stejné chybové zprávě ukazuje chybu podrobnosti žlutý obrazovka smrti (najdete v části **obrázek 4**) společně s hodnoty proměnných serveru v době chyby (najdete v části  **Obrázek 5**). Můžete také zobrazit nezpracované XML ve kterém jsou uloženy podrobnosti o chybě, který obsahuje další informace, jako je například hodnoty v hlavičce HTTP POST.

[![](logging-error-details-with-elmah-cs/_static/image9.png)](logging-error-details-with-elmah-cs/_static/image8.png)

**Obrázek 4**: Zobrazit podrobnosti o chybě YSOD  
([Kliknutím zobrazit obrázek v plné velikosti](logging-error-details-with-elmah-cs/_static/image10.png))

[![](logging-error-details-with-elmah-cs/_static/image12.png)](logging-error-details-with-elmah-cs/_static/image11.png)

**Obrázek 5**: prozkoumat hodnoty kolekce proměnných serveru v době chyby  
([Kliknutím zobrazit obrázek v plné velikosti](logging-error-details-with-elmah-cs/_static/image13.png))

Nasazení do provozního webu ELMAH zahrnuje:

- Kopírování `Elmah.dll` do souboru `Bin` složky v produkčním prostředí
- Kopírování specifických ELMAH nastavení konfigurace, aby `Web.config` soubor použitý v produkčním prostředí a
- Chyba protokolu zdrojové infrastruktuře přidání do provozní databáze.

Jsme jste prozkoumali techniky pro kopírování souborů z vývojového do produkčního prostředí v předchozí kurzy. Možná je nejjednodušší způsob, jak získat zdrojové infrastruktuře protokolu chyb na provozní databáze použít SQL Server Management Studio k připojení k provozní databázi a potom spusťte `SqlServer.sql` souboru skriptu, který vytvoří potřebné tabulky a uložené postupy.

### <a name="viewing-the-error-details-page-on-production"></a>Prohlížení stránky, podrobnosti o chybě u produkčních

Po nasazení webu do produkčního prostředí, navštivte web produkční a generovat k neošetřené výjimce. Jako vývojové prostředí ELMAH nemá žádný vliv na chybová stránka zobrazí, když dojde k neošetřené výjimce; Místo toho jenom zaznamená chybu. Pokud se pokusíte navštívit stránku Chyba protokolu (`elmah.axd`) z provozní prostředí, se zobrazí s zakázáno stránka zobrazená v **obrázek 6**.

[![](logging-error-details-with-elmah-cs/_static/image15.png)](logging-error-details-with-elmah-cs/_static/image14.png)

**Obrázek 6**: ve výchozím nastavení, vzdálené návštěvníky nelze zobrazit chyba protokolu webové stránky  
([Kliknutím zobrazit obrázek v plné velikosti](logging-error-details-with-elmah-cs/_static/image16.png))

Odvolat, v konfiguraci ELMAH `<security>` části nastaví `allowRemoteAccess` atributu na hodnotu 0, která zabraňuje zobrazování v protokolu chyb vzdálené uživatele. Je důležité zakázat anonymní návštěvníky ze zobrazení v protokolu chyb jako podrobnosti o chybě může odhalit ohrožení zabezpečení nebo jiných citlivých informací. Pokud se rozhodnete tento atribut nastavený na hodnotu 1 a povolte tak vzdálený přístup v protokolu chyb, pak je potřeba zámku `elmah.axd` cestu, která návštěvníky pouze oprávnění k němu přístup. Toho lze dosáhnout přidáním `<location>` elementu, který chcete `Web.config` souboru.

Následující konfigurace umožňuje jenom uživatelé v roli správce pro přístup k webové stránce chyba protokolu:

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample5.xml)]

> [!NOTE]
> Role správce a tři uživatelé v systému - Scott, Jisun a Alice - byly přidány v [ *konfigurace na web, používá aplikace služby* kurzu](configuring-a-website-that-uses-application-services-cs.md). Scott uživatelů a Jisun jsou členy role správce. Další informace o ověřování a autorizaci, najdete v části Moje [kurzy zabezpečení webu](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


V protokolu chyb v provozním prostředí můžete nyní zobrazit vzdálení uživatelé; odkazovat zpět **obrázky 3**, **4**, a **5** snímky obrazovky Chyba protokolu webové stránky. Ale pokud anonymní nebo bez oprávnění správce. uživatel se pokusí zobrazit chybovou stránku protokolu je automaticky přesměrován na přihlašovací stránku (`Login.aspx`), jako **na obrázku 7** zobrazuje.

[![](logging-error-details-with-elmah-cs/_static/image18.png)](logging-error-details-with-elmah-cs/_static/image17.png)

**Obrázek 7**: neoprávnění uživatelé jsou automaticky přesměrováni na stránku pro přihlášení  
([Kliknutím zobrazit obrázek v plné velikosti](logging-error-details-with-elmah-cs/_static/image19.png))

### <a name="programmatically-logging-errors"></a>Prostřednictvím kódu programu protokolování chyb

Na ELMAH `ErrorLogModule` modulu HTTP automaticky protokoluje neošetřených výjimek ke zdroji zadaného protokolu. Alternativně může přihlásit k chybě bez nutnosti zvýšení k neošetřené výjimce pomocí `ErrorSignal` třídy a jeho `Raise` metoda. `Raise` Metodě se předává `Exception` objektu a do protokolu zaznamená, jako by měl nebyly vytvořeny této výjimky a dosahoval modulem runtime ASP.NET bez zpracování. Rozdíl, je ale, že žádost pokračuje v provádění obvykle po `Raise` byla volána metoda, zatímco výjimce dojde, neošetřené výjimky přerušení normální spuštění žádosti a způsobí, že modulem runtime ASP.NET zobrazíte nakonfigurované chybové stránky.

`ErrorSignal` Třída je užitečné v situacích, kde je některé akce, která může selhat, ale jeho selhání není závažné celkové operace provádí. Například web může obsahovat formulář, který přijímá vstup uživatele, uloží je v databázi a pak odešle uživateli e-mailu informací, že se informace byla zpracována. Které dojde, pokud informace se ukládají do databáze úspěšně, ale dojde k chybě při odesílání e-mailové zprávě? Jednou z možností je vyvolána výjimka a uživateli odeslat chybovou stránku. Ale to může zmást uživatele do přemýšlení, který informace, které zadal nebyla uložena. Jiná možnost by mohla být protokolu chyby související s e-mailu, ale není činnost koncového uživatele nijak změnit. To je, kdy `ErrorSignal` třída je užitečná.

[!code-csharp[Main](logging-error-details-with-elmah-cs/samples/sample6.cs)]

## <a name="error-notification-via-email"></a>Oznámení o chybě e-mailem

Společně s protokolování chyb do databáze může být ELMAH nakonfigurovat taky k zadané příjemce e-mailem poslat podrobnosti o chybě. Tato funkce poskytuje `ErrorMailModule` modulu HTTP; proto je nutné zaregistrovat tento modul HTTP v `Web.config` aby bylo možné odesílat podrobnosti o chybě e-mailem.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample7.xml)]

Potom zadejte informace o chybě e-mailu v `<elmah>` elementu `<errorMail>` části znamenající odesílatele a příjemce, předmět, k e-mailu a zda e-mail je odeslán asynchronně.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample8.xml)]

Výše uvedené nastavení na místě a vždy, když chyba za běhu dochází ELMAH odešle e-mail s support@example.com s podrobnosti o chybě. E-mailu chyba na ELMAH zahrnuje stejné informace zobrazené v chybě podrobnosti o webové stránky, a to chybovou zprávu, trasování zásobníku a proměnných serveru (odkazuje zpět na **obrázky 4** a **5**). E-mailu, chyba také zahrnuje obsah výjimky podrobnosti žlutý obrazovka smrti jako přílohu (`YSOD.html`).

**Obrázek 8** zobrazuje e-mailu chyba na ELMAH generované navštívíte `Genre.aspx?ID=foo`. Při **obrázek 8** zobrazuje pouze chyby zásobníku a zpráva trasování, proměnné serveru jsou zahrnuty další dolů v obsahu e-mailu.

[![](logging-error-details-with-elmah-cs/_static/image21.png)](logging-error-details-with-elmah-cs/_static/image20.png)

**Obrázek 8**: můžete nakonfigurovat ELMAH Odeslat podrobnosti o chybě e-mailem  
([Kliknutím zobrazit obrázek v plné velikosti](logging-error-details-with-elmah-cs/_static/image22.png))

## <a name="only-logging-errors-of-interest"></a>Jenom protokolování chyb, které vás zajímají

Ve výchozím nastavení zaznamená ELMAH podrobnosti o každé neošetřená výjimka, včetně 404 a dalších chyb HTTP. Můžete určit, aby ELMAH ignorovat tyto nebo jiné typy chyb pomocí filtrování chyb. Filtrování logika je realizována vytvořením na ELMAH `ErrorFilterModule` modulu HTTP, které budete muset zaregistrovat v `Web.config` Chcete-li použít filtrování logiku. Pravidla pro filtrování jsou určené v `<errorFilter>` oddílu.

Následující kód dá pokyn ELMAH do protokolu chyb 404.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample9.xml)]

> [!NOTE]
> Nezapomeňte, aby bylo možné používat, je nutné zaregistrovat filtrování chyb `ErrorFilterModule` modulu HTTP.


`<equal>` Element uvnitř `<test>` části se označuje jako kontrolní výrazy. Pokud kontrolní výraz vyhodnotí jako true, je filtrovaná chyby z protokolu na ELMAH. Nejsou k dispozici, včetně dalších kontrolních výrazů: `<greater>`, `<greater-or-equal>`, `<not-equal>`, `<lesser>`, `<lesser-or-equal>`a tak dále. Můžete také kombinovat kontrolní výrazy pomocí `<and>` a `<or>` logické operátory. Navíc můžete i zahrnují jednoduchý výraz jako kontrolní výrazy jazyka JavaScript, nebo můžete zapsat vlastní kontrolní výrazy v jazyce C# nebo Visual Basic.

Další informace o chybě pro ELMAH možností filtrování, najdete v části [filtrování chyb části](https://code.google.com/p/elmah/wiki/ErrorFiltering) v [ELMAH wiki](https://code.google.com/p/elmah/w/list).

## <a name="summary"></a>Souhrn

ELMAH poskytuje jednoduché, ale výkonný mechanismus pro protokolování chyb ve webové aplikaci ASP.NET. Jako systém sledování stavu společnosti Microsoft ELMAH chyby můžete připojit k databázi a podrobnosti o chybě můžete odeslat vývojář e-mailem. Na rozdíl od stavu systému pro monitorování, zahrnuje ELMAH z pole podporu pro širší škálu úložišť dat protokolu chyb, včetně: Microsoft SQL Server, Microsoft Access, Oracle, soubory XML a několik dalších. Kromě toho ELMAH nabízí integrovanou mechanismus pro zobrazení v protokolu chyb a podrobnosti o konkrétní chybě z webové stránky, `elmah.axd`. `elmah.axd` Stránky může také zpracovat informace o chybě jako informačního kanálu RSS, nebo jako soubor hodnot oddělených čárkami (CSV), který si můžete přečíst pomocí aplikace Microsoft Excel. Můžete také určit, aby ELMAH filtru chyby z protokolu pomocí deklarativní nebo programové kontrolní výrazy. A ELMAH lze použít s aplikací ASP.NET verze 1.x.

Všechny nasazené aplikace by měl mít některé mechanismus pro automaticky protokolování neošetřených výjimek a odesílání oznámení do vývojového týmu. Jestli toho dosahuje pomocí sledování stavu nebo ELMAH je sekundární. Jinými slovy nezáleží na skutečně mnohem ať už používáte sledování stavu nebo ELMAH; Posuďte obou systémů a potom vyberte ten, který nejlépe vyhovuje vašim potřebám. Co je zásadně důležité je, že některé mechanismus zavedou k protokolování neošetřených výjimek v provozním prostředí.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [ELMAH - moduly protokolování chyb a obslužné rutiny](http://dotnetslackers.com/articles/aspnet/ErrorLoggingModulesAndHandlers.aspx)
- [Stránka projektu ELMAH](https://code.google.com/p/elmah/) (zdrojového kódu, ukázky, wiki)
- [Zapojení ELMAH do webovou aplikaci na Catch neošetřených výjimek](http://screencastaday.com/ScreenCasts/43_Plugging_Elmah_into_Web_Application_to_Catch_Unhandled_Exceptions.aspx) (video)
- [Zabezpečení chybové protokolu stránky](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)
- [K vytvoření komponentů modulární ASP.NET pomocí modulů HTTP a obslužné rutiny](https://msdn.microsoft.com/library/aa479332.aspx)
- [Kurzy zabezpečení webu](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Předchozí](logging-error-details-with-asp-net-health-monitoring-cs.md)
> [další](precompiling-your-website-cs.md)
