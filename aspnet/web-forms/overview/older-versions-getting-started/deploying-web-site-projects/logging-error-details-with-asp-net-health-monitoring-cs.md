---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs
title: Podrobnosti o chybě protokolování s ASP.NET stavu monitorování (C#) | Microsoft Docs
author: rick-anderson
description: Systém monitorování stavu společnosti Microsoft poskytuje snadný a přizpůsobit způsob do protokolu různé události web, včetně neošetřených výjimek. V tomto kurzu provede p...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: b1abb452-642a-4ff3-8504-37b85590ff79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs
msc.type: authoredcontent
ms.openlocfilehash: 370f19b36628a9811a31e263e468453897cb7d92
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="logging-error-details-with-aspnet-health-monitoring-c"></a>Podrobnosti o chybě protokolování s ASP.NET stavu monitorování (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_13_CS.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial13_HealthMonitoring_cs.pdf)

> Systém monitorování stavu společnosti Microsoft poskytuje snadný a přizpůsobit způsob do protokolu různé události web, včetně neošetřených výjimek. Tento kurz vás provede nastavením stavu systému pro monitorování k protokolování neošetřených výjimek k databázi a vývojáři prostřednictvím e-mailovou zprávu.


## <a name="introduction"></a>Úvod

Protokolování je užitečným nástrojem pro sledování stavu nasazení aplikace a pro všechny problémy, které mohou nastat diagnostice. Je obzvláště důležité k protokolování chyb, ke kterým došlo v době nasazení aplikace, takže lze napravit. `Error` Událost se vyvolá, vždy, když dojde k neošetřené výjimce v aplikaci ASP.NET; [předchozí kurzu](processing-unhandled-exceptions-cs.md) vám ukázal, jak chcete upozornit vývojář chybu a vytvořením obslužné rutiny události pro protokolujejípodrobnosti.`Error` událostí. Vytváření však `Error` obslužné rutiny události do protokolu podrobnosti k chybě a upozornění vývojář je zbytečné, jak tento úkol provést ASP. NET na *stavu systému pro monitorování*.

Stav systému pro monitorování bylo zavedeno v technologii ASP.NET 2.0 a slouží k monitorování stavu nasazené aplikace ASP.NET pomocí protokolování událostí, ke kterým došlo během doby platnosti žádosti nebo aplikace. Události zapsané podle stavu systému pro monitorování jsou označovány jako *události sledování stavu* nebo *webové události*a zahrnují:

- Události životního cyklu aplikace, například pokud se aplikace spustí nebo zastaví
- Události zabezpečení, včetně neúspěšných pokusů o přihlášení a neúspěšné požadavky autorizace adresy URL
- Chyby aplikace, včetně neošetřené výjimky, zobrazení stavu analýza výjimky, žádost o ověření výjimky a chyby při kompilaci mezi jiné typy chyb.

Když stavu monitorování událost se vyvolá, můžete je zaznamenána do libovolný počet zadaný *protokolu zdroje*. Stav systému pro monitorování se dodává s protokolu zdrojů, které protokolovat webové události do databáze Microsoft SQL Server, do protokolu událostí systému Windows, nebo prostřednictvím e-mailovou zprávu, mimo jiné. Můžete také vytvořit vlastní zdroje protokolu.

Události protokoluje stav monitorování systému, společně s protokolu zdroje, používá, jsou definovány v `Web.config`. Zadání několika řádků kódu konfigurace můžete použít k protokolování neošetřených výjimek všechny k databázi a upozornit vás výjimky prostřednictvím e-mailu na sledování stavu.

## <a name="exploring-the-health-monitoring-systems-configuration"></a>Prohlížení stavu monitorování konfigurace systému

Stav monitorování chování systému je definované informace o konfiguraci, který je umístěný ve [ `<healthMonitoring>` element](https://msdn.microsoft.com/library/2fwh2ss9.aspx) v `Web.config`. Tento konfigurační oddíl definuje mimo jiné, tři důležité následující informace:

1. Sledování události stavu, je-li vyvolána, by se mělo protokolovat,
2. Protokol zdrojů, a
3. Jak každý stav monitorování událostí, které jsou definované v (1) je namapována na zdroji protokolu definované v (2).

Tyto informace se specifikuje prostřednictvím tři podřízené položky konfigurace – elementy: [ `<eventMappings>` ](https://msdn.microsoft.com/library/yc5yk01w.aspx), [ `<providers>` ](https://msdn.microsoft.com/library/zaa41kz1.aspx), a [ `<rules>` ](https://msdn.microsoft.com/library/fe5wyxa0.aspx), v uvedeném pořadí.

Výchozí stav monitorování informace o konfiguraci systému najdete v `Web.config` souboru v `%WINDIR%\Microsoft.NET\Framework\version\CONFIG` složky. Tyto informace o konfiguraci výchozí s některé značek odebrat jako stručný výtah je zobrazena níže:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample1.xml)]

Stav události týkající se monitorování jsou definovány v `<eventMappings>` element, který poskytuje lidské popisný název pro třídu události sledování stavu. Do výše uvedeného kódu `<eventMappings>` element přiřadí lidské popisný název "Všechny chyby" události typu sledování stavu `WebBaseErrorEvent` a název "selhání audity" stav monitorování událostí typu `WebFailureAuditEvent`.

`<providers>` Element definuje zdrojů protokolu, bude mít lidské popisný název a zadání žádné informace o konfiguraci určitého zdroj protokolu. První `<add>` element definuje zprostředkovatele "EventLogProvider", které protokoly sledování událostí pomocí zadaného stavu `EventLogWebEventProvider` třídy. `EventLogWebEventProvider` Třída protokoluje události do protokolu událostí systému Windows. Druhý `<add>` element definuje "SqlWebEventProvider" poskytovatele, který protokoluje události do databáze Microsoft SQL Server prostřednictvím `SqlWebEventProvider` třídy. Konfigurace "SqlWebEventProvider" Určuje připojovací řetězec databáze (`connectionStringName`) mezi další možnosti konfigurace.

`<rules>` Element mapy událostí uvedený v `<eventMappings>` element přihlásit zdroje `<providers>` elementu. Ve výchozím nastavení webové aplikace ASP.NET protokolu všechny neošetřené výjimky a auditování chyby do protokolu událostí systému Windows.

## <a name="logging-events-to-a-database"></a>Protokolování událostí do databáze

Výchozí konfigurace systému pro sledování stavu můžete přizpůsobit na základě webové aplikace ve webové aplikaci přidáním `<healthMonitoring>` oddílu aplikace `Web.config` souboru. Můžete zahrnout další prvky v `<eventMappings>`, `<providers>`, a `<rules>` části pomocí `<add>` elementu. Odebrání konfigurace použít výchozí nastavení `<remove>` element, nebo použijte `<clear />` odebrat všechny výchozí hodnoty z jednoho z těchto částí. Nakonfigurujme kniha recenze webové aplikace do databáze Microsoft SQL Server pomocí protokolu všechny neošetřené výjimky `SqlWebEventProvider` třídy.

`SqlWebEventProvider` Třída je součástí stavu systému pro monitorování a protokoly událostí k zadané databázi systému SQL Server sledování stavu. `SqlWebEventProvider` Třída očekává, že zadaná databáze obsahuje uložené procedury s názvem `aspnet_WebEvent_LogEvent`. Tato uložená procedura je předán podrobnosti události a za úkol ukládání podrobnosti události. Dobrá zpráva je, že není potřeba vytvořit to uložené procedury ani tabulku pro ukládání podrobností události. Můžete přidat tyto objekty k databázi pomocí `aspnet_regsql.exe` nástroj.

> [!NOTE]
> `aspnet_regsql.exe` Nástroj se zabývá zpět [ *konfigurace na web, používá aplikace služby* kurzu](configuring-a-website-that-uses-application-services-cs.md) když jsme doplnili podporu pro ASP. NET na aplikační služby. V důsledku toho již obsahuje databázi recenze adresáře webu `aspnet_WebEvent_LogEvent` uložené procedury, která ukládá informace o události do tabulky s názvem `aspnet_WebEvent_Events`.


Jakmile máte nezbytné uložené procedury a tabulky, které jsou přidány k vaší databázi, zbývá dáte pokyn, aby k protokolování neošetřených výjimek všechny do databáze sledování stavu. Dosáhnout přidáním následující kód do vašeho webu `Web.config` souboru:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample2.xml)]

Stav monitorování konfigurace značek výše používá `<clear />` elementy vymazat sledování informace o konfiguraci z předem definovaných stavu `<eventMappings>`, `<providers>`, a `<rules>` oddíly. Pak přidá jeden záznam pro každý z těchto částí.

- `<eventMappings>` Element definuje jeden stav monitorování události týkající se s názvem "Všechny chyby", která je vyvolána vždy, když dojde k neošetřené výjimce.
- `<providers>` Element definuje jeden protokol zdroj s názvem "SqlWebEventProvider", který používá `SqlWebEventProvider` třídy. `connectionStringName` Atribut byla nastavena na "ReviewsConnectionString", což je název připojení k naší řetězce definované v `<connectionStrings>` oddílu.
- Nakonec &lt;pravidla&gt; element označuje, že když událost "Všechny chyby" ukáže, že se mají být protokolovány pomocí zprostředkovatele "SqlWebEventProvider".

Tyto informace o konfiguraci dá pokyn, stav systému k protokolování neošetřených výjimek všechny k databázi recenze adresáře pro monitorování.

> [!NOTE]
> `WebBaseErrorEvent` Událost je aktivována pouze pro chyby serveru; není vyvolána pro chyby protokolu HTTP, třeba požadavek na zdroj technologie ASP.NET, který nebyl nalezen. Tím se liší od chování `HttpApplication` třídy `Error` událost, která je vyvolána pro server i chyby protokolu HTTP.


Informace o stavu systému v akci pro monitorování, přejděte na webovou stránku a generování běhová chyba navštivte stránky `Genre.aspx?ID=foo`. Zobrazí se příslušná chybová stránka – výjimka podrobnosti žlutý obrazovka smrti (Pokud návštěvou místně) nebo vlastní chybovou stránku (při návštěvě webu v produkčním prostředí). Stav systému pro monitorování na pozadí protokolovány informace o chybě databáze. By měl být jeden záznam v `aspnet_WebEvent_Events` tabulky (viz **obrázek 1**); tento záznam obsahuje informace o této chybě modulu runtime, který právě došlo k chybě.

[![](logging-error-details-with-asp-net-health-monitoring-cs/_static/image2.png)](logging-error-details-with-asp-net-health-monitoring-cs/_static/image1.png)

**Obrázek 1**: Podrobnosti o chybě byly protokolovány `aspnet_WebEvent_Events` tabulky  
([Kliknutím zobrazit obrázek v plné velikosti](logging-error-details-with-asp-net-health-monitoring-cs/_static/image3.png))

### <a name="displaying-the-error-log-in-a-web-page"></a>Zobrazení v protokolu chyb na webové stránce

Stav systému pro monitorování s aktuální konfigurací webu, zaznamená všechny neošetřené výjimky k databázi. Sledování stavu však neposkytuje žádný mechanismus zobrazte protokol chyb prostřednictvím na webové stránce. Je však sestavení stránku ASP.NET, která zobrazuje tyto informace z databáze. (Jako ukážeme na okamžik, se můžete rozhodnout pro mít podrobnosti o chybě vám odeslán e-mailovou zprávu.)

Pokud vytvoříte taková stránka, zkontrolujte, zda že je provést kroky k Povolit jenom autorizovaným uživatelům, chcete-li zobrazit podrobnosti o chybě. Pokud váš web již aktivuje uživatelské účty se autorizačních pravidel adres URL můžete použít k omezení přístupu na stránku na určité uživatele nebo role. Další informace o tom, jak udělit nebo omezení přístupu k webovým stránkám na základě přihlášeného uživatele, najdete v části Moje [kurzy zabezpečení webu](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

> [!NOTE]
> Jsou zde popsány následné kurzu alternativní Chyba protokolování a oznámení systému s názvem ELMAH. ELMAH zahrnuje integrovanou mechanismus pro zobrazení v protokolu chyb z obou webové stránky a jako informačního kanálu RSS.


## <a name="logging-events-to-email"></a>Protokolování událostí k e-mailu

Stav systému pro monitorování obsahuje poskytovatele zdroj protokolu, který "" zaprotokoluje událost e-mailové zprávě. Zdroj protokolu obsahuje stejné informace, která je zaznamenána do databáze v textu e-mailové zprávy. Tento zdroj protokolu můžete informovala, že vývojář stavu monitorování výskytu daných událostí.

Umožňuje aktualizovat recenze kniha konfiguraci příslušného webu tak, aby obdržíme e-mailu vždy, když k výjimce dochází. K tomu je potřeba provést tři úkoly:

1. Konfigurace webové aplikace ASP.NET k odesílání e-mailu. Toho dosahuje tak, že určíte, jak se odesílají e-mailové zprávy prostřednictvím `<system.net>` konfigurační prvek. Další informace o odesílání e-mailové zprávy v aplikaci ASP.NET najdete v části [odesílání e-mailu v ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx) a [nejčastější dotazy týkající se System.Net.Mail](http://systemnetmail.com/).
2. Zaregistrujte poskytovatele správy zdrojového protokolu e-mailu v `<providers>` elementu a
3. Přidání položky do `<rules>` element, který mapuje událostí "Všechny chyby" poskytovatele správy zdrojového protokolu přidali v kroku (2).

Sledování systému stavu zahrnuje dvě třídy zprostředkovatele zdroj e-mailu protokolu: `SimpleMailWebEventProvider` a `TemplatedMailWebEventProvider`. [ `SimpleMailWebEventProvider` Třída](https://msdn.microsoft.com/library/system.web.management.simplemailwebeventprovider.aspx) odešle zprávu e-mailu ve formátu prostého textu, která zahrnuje události, podrobnosti a poskytuje málo přizpůsobení obsahu e-mailu. S [ `TemplatedMailWebEventProvider` třída](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) zadáte stránku ASP.NET, jejichž vykreslované značky se používá jako text e-mailové zprávy. [ `TemplatedMailWebEventProvider` Třída](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) vám dává mnohem větší kontrolu nad obsah a formát e-mailové zprávy, ale vyžaduje trochu další předem práci, jako je nutné vytvořit stránku ASP.NET, který generuje tělo e-mailové zprávy. Tento kurz se zaměřuje na pomocí `SimpleMailWebEventProvider` třídy.

Aktualizovat stav monitorování systému `<providers>` element v `Web.config` souboru protokolu zdroj pro `SimpleMailWebEventProvider` třídy:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample3.xml)]

Výše uvedený kód používá `SimpleMailWebEventProvider` třída jako poskytovatele správy zdrojového protokolu a přiřadí ji popisný název "EmailWebEventProvider". Kromě toho `<add>` atribut obsahuje další možnosti konfigurace, jako je například Komu a z adres e-mailové zprávy.

Se zdrojem e-mailu protokolu definované zbývá dáte pokyn, aby stav systému pro použití tohoto zdroje k "protokolování" neošetřené výjimky pro monitorování. To je možné udělat přidáním nové pravidlo v `<rules>` části:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample4.xml)]

`<rules>` Část teď obsahuje dvě pravidla. První z nich, s názvem "Všechny chyby na e-mailu", odešle zdroj protokolu "EmailWebEventProvider" všechny neošetřené výjimky. Toto pravidlo má za následek odesílání podrobnosti o chybách na webu do zadané adresy. "Všechny chyby do databáze" pravidlo zaznamená podrobnosti o chybě do databáze lokality. V důsledku toho vždy, když dojde k neošetřené výjimce v lokalitě podrobnosti se obě přihlášení k databázi a odesílají do zadané e-mailovou adresu.

**Obrázek 2** ukazuje vygenerovaných e-mailu `SimpleMailWebEventProvider` třídy při návštěvě `Genre.aspx?ID=foo`.

[![](logging-error-details-with-asp-net-health-monitoring-cs/_static/image5.png)](logging-error-details-with-asp-net-health-monitoring-cs/_static/image4.png)

**Obrázek 2**: Podrobnosti o chybě jsou zasílány v e-mailové zprávy  
([Kliknutím zobrazit obrázek v plné velikosti](logging-error-details-with-asp-net-health-monitoring-cs/_static/image6.png))

## <a name="summary"></a>Souhrn

Systém monitorování stavu ASP.NET je navržena k umožnění správci sledovat stav nasazených webových aplikací. Události monitorování stavu se vyvolá, když unfold určité akce, například při zastavení aplikace, pokud uživatel úspěšně přihlásí webu, nebo když dojde k neošetřené výjimce. Tyto události může být protokolovány libovolný počet zdrojů protokolu. Tento kurz vám ukázal, jak k databázi a prostřednictvím e-mailovou zprávu protokolu podrobnosti o neošetřených výjimek.

Tento kurz se zaměřuje na použití k protokolování neošetřených výjimek, ale mějte na paměti, že monitorování stavu slouží k měření celkového stavu nasazené aplikace ASP.NET a obsahuje celou řadu událostí sledování stavu a protokolu zdroje není sledování stavu prozkoumali sem. Co je více, můžete vytvořit vlastní sledování událostí a protokolu zdrojů, stavu potřeby nastat. Pokud vás zajímá dozvědět další informace o sledování stavu, dobrý prvním krokem je pročtěte [Erik Reitan](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)na [sledování nejčastější dotazy týkající se stavu](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx). Následující, poraďte se [postupy: použití sledování stavu v technologii ASP.NET 2.0](https://msdn.microsoft.com/library/ms998306.aspx).

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Přehled monitorování stavu technologie ASP.NET](https://msdn.microsoft.com/library/bb398933.aspx)
- [Konfigurace a přizpůsobení stav monitorování systému technologie ASP.NET](http://dotnetslackers.com/articles/aspnet/ConfiguringAndCustomizingTheHealthMonitoringSystemOfASPNET.aspx)
- [Časté otázky – stavu monitorování v technologii ASP.NET 2.0](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)
- [Postupy: Odeslání e-mailové oznámení o sledování stavu](https://msdn.microsoft.com/library/ms227553.aspx)
- [Postupy: Použití sledování stavu technologie ASP.NET](https://msdn.microsoft.com/library/ms998306.aspx)
- [Stav monitorování technologie ASP.NET](http://aspnet.4guysfromrolla.com/articles/031407-1.aspx)

> [!div class="step-by-step"]
> [Předchozí](processing-unhandled-exceptions-cs.md)
> [další](logging-error-details-with-elmah-cs.md)
