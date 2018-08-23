---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-vb
title: Protokolování podrobností o chybách pomocí monitorování (VB) stavu v ASP.NET | Dokumentace Microsoftu
author: rick-anderson
description: Systém monitorování stavu od Microsoftu poskytuje snadný a přizpůsobit způsob do protokolu různé webové události, včetně neošetřených výjimek. Tento kurz vás p...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: 09a6c74e-936a-4c04-8547-5bb313a4e4a3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-vb
msc.type: authoredcontent
ms.openlocfilehash: f9aeeb0b3d21707324d239efad4deffcf0f7fead
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752239"
---
<a name="logging-error-details-with-aspnet-health-monitoring-vb"></a>Protokolování podrobností o chybách pomocí monitorování (VB) stavu v ASP.NET
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_13_VB.zip) nebo [stahovat PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial13_HealthMonitoring_vb.pdf)

> Systém monitorování stavu od Microsoftu poskytuje snadný a přizpůsobit způsob do protokolu různé webové události, včetně neošetřených výjimek. Tento kurz vás provede nastavení stavu monitorování systému k protokolování neošetřených výjimek k databázi a upozornit vývojáře prostřednictvím e-mailovou zprávu.


## <a name="introduction"></a>Úvod

Protokolování je užitečný nástroj pro monitorování stavu nasazení aplikace a diagnostikování potíží, které mohou nastat. Je obzvláště důležité k protokolování chyb, ke kterým dochází v nasazené aplikaci tak, aby lze napravit. `Error` Událost se vyvolá vždy, když dojde k neošetřené výjimce v aplikaci ASP.NET; [předchozím kurzu](processing-unhandled-exceptions-vb.md) jsme si ukázali, jak informovat vývojáře o chybě a protokolovat její podrobnosti tak, že vytvoříte obslužná rutina události `Error` událostí. Nicméně vytváření `Error` obslužnou rutinu události do protokolu podrobnosti chyby a upozornit vývojáře je zbytečné, protože tato úloha může provádět ASP. NET společnosti *systém monitorování stavu*.

Stav monitorování systému byla zavedena v technologii ASP.NET 2.0 a je navržená tak, aby monitorovala nasazené aplikace v ASP.NET pomocí protokolování událostí, ke kterým dochází během životního cyklu požadavku nebo aplikace. Události zapsané podle stavu monitorování systému jsou označovány jako *událostech monitorování stavu* nebo *webové události*a zahrnují:

- Události životního cyklu aplikace, například když aplikace, spuštění nebo zastavení
- Události zabezpečení, včetně neúspěšných pokusů o přihlášení a neúspěšné požadavky autorizace adresy URL
- Chyby aplikace, včetně neošetřené výjimky, zobrazení stavu, analýza výjimek, žádost o ověření výjimky a chyby při kompilaci, mimo jiné typy chyb.

Když je vyvolána událost monitorování stavu můžete protokolovat do libovolného počtu zadané *protokolu zdroje*. Stav monitorování systému se dodává s zdroje protokolů, kteří se přihlašují k databázi serveru Microsoft SQL Server, do protokolu událostí Windows, nebo prostřednictvím e-mailovou zprávu, mimo jiné webové události. Můžete také vytvořit vlastní zdroje protokolů.

Události stavu systému sledování přihlásí, spolu s zdroje protokolů použít, jsou definovány v `Web.config`. Po zadání několika řádků kódu konfigurace můžete použít k přihlášení k databázi všechny neošetřené výjimky a upozornění na výjimky e-mailem monitorování stavu.

## <a name="exploring-the-health-monitoring-systems-configuration"></a>Zkoumání monitorování systému konfigurace stavu

Stav monitorování chování systému je definován konfigurační informace, které se nachází v [ `<healthMonitoring>` element](https://msdn.microsoft.com/library/2fwh2ss9.aspx) v `Web.config`. Tento konfigurační oddíl definuje, mimo jiné následující tři důležité údaje informace:

1. Monitorování událostí stavu, když aktivovaná, by se mělo protokolovat,
2. Zdroje protokolů a
3. Jak každý stav monitorování události definované v (1) je namapována na zdroje protokolů definované v (2).

Tyto informace se specifikuje prostřednictvím tři děti konfigurační prvky: [ `<eventMappings>` ](https://msdn.microsoft.com/library/yc5yk01w.aspx), [ `<providers>` ](https://msdn.microsoft.com/library/zaa41kz1.aspx), a [ `<rules>` ](https://msdn.microsoft.com/library/fe5wyxa0.aspx)v uvedeném pořadí.

Výchozí stav monitorování informace o konfiguraci systému najdete v `Web.config` ve `%WINDIR%\Microsoft.NET\Framework\version\CONFIG` složky. Tyto informace o konfiguraci výchozí některé značkami odebrána jako stručný výtah je zobrazena níže:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample1.xml)]

Stav monitorování události jsou definovány v `<eventMappings>` element, který poskytuje lidských – popisný název pro třídu událostech monitorování stavu. V kódu výše `<eventMappings>` element přiřadí událostí typu monitorování stavu lidských – popisný název "Všechny chyby" `WebBaseErrorEvent` a název "selhání auditů" stavy sledování událostí typu `WebFailureAuditEvent`.

`<providers>` Element definuje zdroje protokolů, jim lidských – popisný název a zadání jakékoli informace konfigurace pro konkrétní zdroj protokolu. První `<add>` element definuje "EventLogProvider" poskytovatele, které zanáší monitorování událostí s použitím zadaného stavu `EventLogWebEventProvider` třídy. `EventLogWebEventProvider` Třídy protokoluje události do protokolu událostí Windows. Druhá `<add>` element definuje "SqlWebEventProvider" poskytovatele, který protokoluje události do databáze Microsoft SQL Server prostřednictvím `SqlWebEventProvider` třídy. Konfigurace "SqlWebEventProvider" Určuje připojovací řetězec databáze (`connectionStringName`) mezi další možnosti konfigurace.

`<rules>` Prvek mapuje události podle `<eventMappings>` element zdroje přihlásit `<providers>` elementu. Ve výchozím nastavení webové aplikace ASP.NET protokolovat všechny neošetřené výjimky a auditovat chyby do protokolu událostí Windows.

## <a name="logging-events-to-a-database"></a>Protokolování událostí do databáze

Stav monitorování výchozí konfigurace systému je přizpůsobit na základě webové aplikace ve webové aplikaci tak, že přidáte `<healthMonitoring>` části aplikace `Web.config` souboru. Můžete zahrnout další elementy v `<eventMappings>`, `<providers>`, a `<rules>` oddíly s použitím `<add>` elementu. Odebrání konfigurace použít výchozí nastavení `<remove>` element, nebo použijte `<clear />` odebrat všechny výchozí hodnoty z jednoho z těchto oddílů. Nakonfigurujme recenzí webové aplikace do databáze serveru Microsoft SQL Server pomocí protokolu všechny neošetřené výjimky `SqlWebEventProvider` třídy.

`SqlWebEventProvider` Třídy je součástí stavu systému sledování a protokolů událostí k zadané databázi systému SQL Server monitorování stavu. `SqlWebEventProvider` Třídy očekává, že zadaná databáze obsahuje uloženou proceduru s názvem `aspnet_WebEvent_LogEvent`. Tuto uloženou proceduru se předá podrobnosti o události a úkol s ukládáním podrobnosti o události. Dobrou zprávou je, že není nutné k vytvoření tohoto uložená procedura ani tabulku pro ukládání podrobnosti o události. Tyto objekty lze přidat do vaší databáze pomocí `aspnet_regsql.exe` nástroj.

> [!NOTE]
> `aspnet_regsql.exe` Nástroj byl popsán v [ *konfigurace na webu, že používá aplikační služby* kurzu](configuring-a-website-that-uses-application-services-vb.md) když jsme přidali podporu pro ASP. SÍŤ pro aplikační služby. V důsledku toho již obsahuje databázi na webu knihy recenze `aspnet_WebEvent_LogEvent` uloženou proceduru, která ukládá informace o události do tabulky s názvem `aspnet_WebEvent_Events`.


Jakmile budete mít potřebné uložené procedury a tabulky přidá k vaší databázi, už jen zbývá dáte pokyn, aby k protokolování neošetřených výjimek všechny do databáze sledování stavu. To provést tak, že přidáte následující kód na váš web `Web.config` souboru:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample2.xml)]

Monitorování konfigurace značek výše používá stavu `<clear />` prvků vymazat sledování informací o konfiguraci z předem definovaných stavu `<eventMappings>`, `<providers>`, a `<rules>` oddíly. Pak přidá jednu položku na každý z těchto oddílů.

- `<eventMappings>` Prvek definuje jeden stav monitorování události zájmu s názvem "Všechny chyby", která je vyvolána pokaždé, když dojde k neošetřené výjimce.
- `<providers>` Prvek definuje jeden protokol zdroj s názvem "SqlWebEventProvider", který používá `SqlWebEventProvider` třídy. `connectionStringName` Atribut je nastavená na "ReviewsConnectionString", což je název naší připojovací řetězec definovaný v `<connectionStrings>` části.
- Nakonec &lt;pravidla&gt; element označuje, že když událost "Všechny chyby" ukáže, že se mají být protokolovány pomocí zprostředkovatele "SqlWebEventProvider".

Tyto informace o konfiguraci nastaví stav monitorování systému do protokolu všechny neošetřené výjimky databáze recenzí.

> [!NOTE]
> `WebBaseErrorEvent` Událost je aktivována pouze pro chyby serveru; není vyvolána pro chyby protokolu HTTP, třeba požadavek na zdroj technologie ASP.NET, který se nenachází. Tím se liší od chování `HttpApplication` třídy `Error` událost, která se vyvolá pro server a chyby protokolu HTTP.


Pokud chcete zobrazit stav monitorování v akci, přejděte na webovou stránku a generovat Chyba za běhu návštěvou `Genre.aspx?ID=foo`. Měli byste vidět příslušná chybová stránka – výjimka podrobnosti žlutý obrazovky z smrti (při návštěvě místně) nebo vlastní chybové stránky (při návštěvě webu v produkčním prostředí). Systém monitorování stavu na pozadí protokolovány informace o chybě databáze. Měla by existovat jeden záznam v `aspnet_WebEvent_Events` tabulky (naleznete v tématu **obrázek 1**); tento záznam obsahuje informace o této chybě modulu runtime, že právě došlo k chybě.

[![](logging-error-details-with-asp-net-health-monitoring-vb/_static/image2.png)](logging-error-details-with-asp-net-health-monitoring-vb/_static/image1.png)

**Obrázek 1**: Podrobnosti o chybě byly protokolovány `aspnet_WebEvent_Events` tabulky  
([Kliknutím ji zobrazíte obrázek v plné velikosti](logging-error-details-with-asp-net-health-monitoring-vb/_static/image3.png))

### <a name="displaying-the-error-log-in-a-web-page"></a>Zobrazení v protokolu chyb na webové stránce

Aktuální konfigurací webu stav monitorování systému zaznamená všechny neošetřené výjimky do databáze. Monitorování stavu však neposkytuje žádný mechanismus zobrazte protokol chyb prostřednictvím webové stránky. Může ale vytvořit stránky ASP.NET, která zobrazuje tyto informace z databáze. (Jak uvidíme okamžik, můžete se rozhodnout obsahovat podrobnosti o chybě odesílat e-mailové zprávy.)

Pokud vytvoříte taková ještě stránka, zkontrolujte, zda že je provést postup, který umožňuje jenom Autorizovaní uživatelé, chcete-li zobrazit podrobnosti o chybě. Pokud váš web již využívá uživatelské účty autorizačních pravidel adres URL můžete použít k omezení přístupu ke stránce na určité uživatele nebo role. Další informace o tom, jak udělit nebo omezit přístup k webovým stránkám na základě přihlášeného uživatele, najdete v mé [kurzy o zabezpečení webu](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

> [!NOTE]
> Následujícím kurzu zkoumá alternativní Chyba protokolování a oznámení systému s názvem ELMAH. ELMAH zahrnuje předdefinovaný mechanismus, chcete-li zobrazit v protokolu chyb z obou webové stránky a jako informačního kanálu RSS.


## <a name="logging-events-to-email"></a>Protokolování událostí k e-mailu

Stav monitorování systému zahrnuje poskytovatele zdroj protokolu, který "zaprotokoluje" událost do e-mailovou zprávu. Zdroj protokolu obsahuje stejné informace, která je zaznamenána do databáze v textu e-mailové zprávy. Upozornit vývojáře stavu monitorování výskytu daných událostí můžete použít tento zdroj protokolu.

Aktualizaci Pojďme recenzí konfiguraci příslušného webu tak, aby jsme dostávat e-mailu pokaždé, když výjimka nastane. K tomu potřeba provést tři úkoly:

1. Konfigurace webové aplikace ASP.NET k odeslání e-mailu. To lze provést tak, že určíte, jak se odesílají e-mailové zprávy prostřednictvím `<system.net>` konfiguračního prvku. Další informace o odesílání e-mailové zprávy v aplikaci ASP.NET najdete [odesílání e-mailu v ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx) a [nejčastější dotazy týkající se System.Net.Mail](http://systemnetmail.com/).
2. Zaregistrovat poskytovatele e-mailu protokolu zdroje v `<providers>` elementu a
3. Přidání položky do `<rules>` element, který se mapuje na přidali v kroku (2) poskytovatel správy zdrojových protokol událostí "Všechny chyby".

Stav monitorování systému obsahuje dvě e-mailu protokolu zdrojový poskytovatel třídy: `SimpleMailWebEventProvider` a `TemplatedMailWebEventProvider`. [ `SimpleMailWebEventProvider` Třídy](https://msdn.microsoft.com/library/system.web.management.simplemailwebeventprovider.aspx) odešle zprávu emaily s prostým textem, který obsahuje události podrobnosti a poskytuje trochu přizpůsobit e-mailu. S [ `TemplatedMailWebEventProvider` třídy](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) zadáte stránky technologie ASP.NET, jehož vykreslované značky se používá jako text e-mailové zprávy. [ `TemplatedMailWebEventProvider` Třídy](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) nabízí mnohem větší kontrolu nad obsah a formát e-mailové zprávy, ale vyžaduje trochu další předem fungují podle musíte vytvořit stránku ASP.NET, která generuje text e-mailové zprávy. Tento kurz se zaměřuje na použití `SimpleMailWebEventProvider` třídy.

Aktualizovat stav monitorování systému `<providers>` prvek v `Web.config` souboru zdroj protokolu zahrnout `SimpleMailWebEventProvider` třídy:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample3.xml)]

Pomocí výše uvedeného kódu `SimpleMailWebEventProvider` třídu jako poskytovatel správy zdrojových protokolu a přiřadí ji popisný název "EmailWebEventProvider". Kromě toho `<add>` atribut obsahuje další možnosti konfigurace, jako je například hodnota do a z adres e-mailové zprávy.

S definován zdroj e-mailu protokolu už jen zbývá dáte pokyn, aby stav monitorování systému budou používat tento zdroj "přihlásit" neošetřené výjimky. To lze provést tak, že přidáte nové pravidlo v `<rules>` části:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample4.xml)]

`<rules>` Část nyní obsahuje dvě pravidla. První z nich, s názvem "Všechny chyby k e-mailu", odešle všechny neošetřené výjimky do zdroje "EmailWebEventProvider" protokolu. Toto pravidlo má účinek Odeslat podrobnosti o chybách na webu na zadané adrese. Pravidlo "Všechny chyby do databáze" protokoly podrobnosti o chybě pro databázi této lokality. V důsledku toho pokaždé, když dojde k neošetřené výjimce v lokalitě její podrobnosti jsou obě přihlášení k databázi a odeslány na zadanou e-mailovou adresu.

**Obrázek 2** zobrazuje vygenerovaná e-mailu `SimpleMailWebEventProvider` třídy při návštěvě `Genre.aspx?ID=foo`.

[![](logging-error-details-with-asp-net-health-monitoring-vb/_static/image5.png)](logging-error-details-with-asp-net-health-monitoring-vb/_static/image4.png)

**Obrázek 2**: The podrobnosti o chybě se odesílají v e-mailovou zprávu  
([Kliknutím ji zobrazíte obrázek v plné velikosti](logging-error-details-with-asp-net-health-monitoring-vb/_static/image6.png))

## <a name="summary"></a>Souhrn

Systém monitorování stavu technologie ASP.NET je navržena k umožnění můžou správci sledovat stav nasazených webových aplikací. Stav monitorování události jsou vyvolány při unfold určité akce, například když se aplikace zastaví, když úspěšně přihlášení uživatele webu, nebo když dojde k neošetřené výjimce. Tyto události je možné protokolovat libovolný počet zdroje protokolů. Tento kurz vám ukázal, jak protokolovat podrobnosti o neošetřené výjimky k databázi a prostřednictvím e-mailovou zprávu.

V tomto kurzu se zaměřují na použití k protokolování neošetřených výjimek, ale mějte na paměti, že monitorování stavu slouží k měření celkový stav nasazené aplikace v ASP.NET a zahrnuje celou řadu událostí monitorování stavu a protokolu zdrojů není monitorování stavu prozkoumali jste ji sem. Navíc můžete vytvořit vlastní sledování událostí a protokolu zdroje stavu potřeby by měl nastat. Pokud se chcete dozvědět více o monitorování stavu je dobrý první krok k pročtěte [Erik Reitan](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)společnosti [nejčastější dotazy k monitorování stavu](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx). Pod najdete [postupy: použití sledování stavu v ASP.NET 2.0](https://msdn.microsoft.com/library/ms998306.aspx).

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Přehled monitorování stavu v ASP.NET](https://msdn.microsoft.com/library/bb398933.aspx)
- [Konfigurace a přizpůsobení stav monitorování systému technologie ASP.NET](http://dotnetslackers.com/articles/aspnet/ConfiguringAndCustomizingTheHealthMonitoringSystemOfASPNET.aspx)
- [Časté otázky – sledování stavu v ASP.NET 2.0](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)
- [Postupy: Odesílání e-mailové oznámení o sledování stavu](https://msdn.microsoft.com/library/ms227553.aspx)
- [Postupy: Použití sledování stavu v ASP.NET](https://msdn.microsoft.com/library/ms998306.aspx)
- [Sledování stavu v ASP.NET](http://aspnet.4guysfromrolla.com/articles/031407-1.aspx)

> [!div class="step-by-step"]
> [Předchozí](processing-unhandled-exceptions-vb.md)
> [další](logging-error-details-with-elmah-vb.md)
