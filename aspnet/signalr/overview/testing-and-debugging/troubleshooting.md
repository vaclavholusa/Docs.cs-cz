---
uid: signalr/overview/testing-and-debugging/troubleshooting
title: "Řešení potíží s SignalR | Microsoft Docs"
author: pfletcher
description: "Tento článek popisuje běžné problémy s vývojem aplikací SignalR."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 4b559e6c-4fb0-4a04-9812-45cf08ae5779
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: 2394ee81f4592417a034e47db6eefd3e4b91a9af
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="signalr-troubleshooting"></a>Řešení potíží s SignalR
====================
podle [Patrik Fletcher](https://github.com/pfletcher)

> Tento dokument popisuje řešení běžných problémů s SignalR.
> 
> ## <a name="software-versions-used-in-this-topic"></a>Verze softwaru použitým v tomto tématu
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR verze 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Předchozí verze tohoto tématu
> 
> Informace o předchozích verzích SignalR najdete v tématu [starší verze funkce SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Dotazy a připomínky
> 
> Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky. Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com/).


Tento dokument obsahuje následující části.

- [Volání metody mezi klientem a serverem bez upozornění selže](#connection)
- [Konfigurace služby IIS websocket pro příkaz ping/pong ke zjištění neaktivní klienta](#pong)
- [Jiné potíže s připojením](#other)
- [Chyby kompilace a na straně serveru](#server)
- [Problémy v sadě Visual Studio](#vs)
- [Internetová informační služba problémy](#iis)
- [Problémy Microsoft Azure](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a>Volání metody mezi klientem a serverem bez upozornění selže

Tato část popisuje možné příčiny pro volání metody mezi klientem a serverem smysluplné chybové zprávy v případě selhání. V aplikaci SignalR server nemá žádné informace o metody, které klient implementuje; Když server volá metodu klienta, metoda název a parametru data se odesílají do klienta a tuto metodu je spustit pouze v případě, že existuje ve formátu, který zadaný server. Pokud je nalezena žádná odpovídající metoda na klientovi, nedojde k žádné akci, a žádná chybová zpráva se vyvolá na serveru.

K hlubšímu prošetření aplikace volá metody klienta, můžete zapnout protokolování před voláním metody start na rozbočovači zobrazíte jaké volání pocházejí ze serveru. Povolení protokolování v aplikaci JavaScript, najdete v části [jak povolit protokolování na straně klienta (verze jazyka JavaScript klienta)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging). Povolení protokolování v aplikaci klienta rozhraní .NET, najdete v části [jak povolit protokolování na straně klienta (klient .NET verze)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a>Metoda překlepu, metoda nesprávný podpis nebo název nesprávný rozbočovače

Pokud název nebo podpis metody volané přesně neodpovídá odpovídající metody na straně klienta, volání selže. Ověřte, že název metody, které jsou volány server odpovídá názvu metody na straně klienta. Také SignalR vytvoří proxy server rozbočovače metodami ve formátu camelCase, jako je vhodné v jazyce JavaScript, takže volána metoda `SendMessage` na serveru by volat `sendMessage` proxy serveru klienta. Pokud použijete `HubName` atribut ve vašem kódu na straně serveru, zkontrolujte, zda použít název odpovídá názvu použít k vytvoření rozbočovače na straně klienta. Pokud použijete `HubName` atribut, ověřte, zda je název rozbočovače v klientovi JavaScript ve formátu camelCase-, jako je například chatHub místo ChatHub.

### <a name="duplicate-method-name-on-client"></a>Duplicitní název metody na klientovi

Zkontrolujte duplicitní metodu nemají na klientovi, který se liší pouze v případě. Pokud klientské aplikace má metodu s názvem `sendMessage`, ověřte, že není k dispozici také metodu s názvem `SendMessage` také.

### <a name="missing-json-parser-on-the-client"></a>Chybějící analyzátor JSON na straně klienta

SignalR vyžaduje analyzátor JSON pro serializaci volání mezi serverem a klientem. Pokud váš klient nemá předdefinované analyzátor JSON (například aplikace Internet Explorer 7), musíte použít jeden ve vaší aplikaci. Můžete si stáhnout analyzátor JSON [zde](http://nuget.org/packages/json2).

### <a name="mixing-hub-and-persistentconnection-syntax"></a>Kombinací rozbočovače a připojení PersistentConnection syntaxe

SignalR používá dva modely komunikace: rozbočovače a PersistentConnections. Syntaxe volání tyto modely dva komunikace se liší v kódu klienta. Pokud jste přidali rozbočovač v serverovém kódu, ověřte, že veškerý kód klienta se používá syntaxe správné rozbočovače.

**Kód JavaScript klienta, který vytvoří připojení PersistentConnection v klientovi JavaScript**

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

**Kód JavaScript klienta, který vytvoří proxy server rozbočovače v klientovi Javascript**

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

**C# serveru kód, který mapuje trasu připojení PersistentConnection**

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

**C# serveru kód, který mapuje trasu k rozbočovači, nebo více centra, pokud máte několik aplikací**

[!code-css[Main](troubleshooting/samples/sample4.css)]

### <a name="connection-started-before-subscriptions-are-added"></a>Připojení před předplatná se přidají spustit

Pokud připojení rozbočovače na spuštění před metody, které lze volat ze serveru se přidají k proxy serveru, nebude přijímat zprávy. Následující kód v JavaScriptu nespustí rozbočovače správně:

**Nesprávný kód klienta JavaScript, který nebude povolit příjem zpráv rozbočovače**

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

Místo toho přidejte odběry metoda před voláním metody Start:

**Kód JavaScript klienta, který správně přidá odběry k rozbočovači**

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a>Chybí název metody na proxy server rozbočovače

Ověřte, že metoda definované na serveru je přihlášen k na straně klienta. I když server definuje metodu, musí pořád přidané k proxy serveru klienta. Metody lze přidat k proxy serveru klienta následujícími způsoby (Všimněte si, že metoda je přidán do `client` člen rozbočovače, rozbočovače není přímo):

**Kód jazyka JavaScript klienta, který přidá metody proxy server rozbočovače**

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a>Centrum nebo není deklarován jako veřejné metody rozbočovače

Aby byl viditelný na straně klienta, musí být deklarován implementace rozbočovače a metody jako `public`.

### <a name="accessing-hub-from-a-different-application"></a>Přístup k centru z jiné aplikace

Rozbočovače SignalR je přístupné pouze prostřednictvím aplikace, které implementují klienti SignalR. SignalR nemůže spolupracovat s další komunikace knihovny (například SOAP nebo WCF webových služeb.) Pokud je k dispozici pro svou cílovou platformu žádné klienta SignalR, nemají přímý přístup serveru koncového bodu.

### <a name="manually-serializing-data"></a>Ručně serializaci dat

SignalR automaticky použije JSON k serializaci metodu parametry zde není nutné provést sami.

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a>Vzdálené metody rozbočovače nebyl proveden v klientovi ve funkci ondisconnected rozbočovače

Toto chování je záměrné. Když `OnDisconnected` je volána, rozbočovače již zadány `Disconnected` stav, který neumožňuje další metody rozbočovače k volání.

**C# serveru kód, který správně spustí kód v události ondisconnected rozbočovače**

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="ondisconnect-not-firing-at-consistent-times"></a>OnDisconnect se nespustí, v konzistentní dobu

Toto chování je záměrné. Když se uživatel pokusí o opustit stránku s aktivního připojení SignalR, ke klientovi SignalR budou best effort pokusu serveru oznámit, že připojení klienta bude zastaven. Pokud je ke klientovi SignalR best effort pokus selže připojit k serveru, serveru bude dispose připojení po konfigurovat `DisconnectTimeout` později, po kterých `OnDisconnected` událostí bude platit. Pokud je ke klientovi SignalR best effort pokus proběhne úspěšně, `OnDisconnected` událostí bude platit okamžitě.

Informace o nastavení `DisconnectTimeout` nastavení, najdete v části [zpracování události životnost připojení: DisconnectTimeout](../guide-to-the-api/handling-connection-lifetime-events.md#disconnecttimeout).

### <a name="connection-limit-reached"></a>Bylo dosaženo limitu připojení

Pokud používáte plnou verzi služby IIS na klientský operační systém jako Windows 7, omezení 10 připojení není nastaveno. Při použití klientského operačního systému, používejte IIS Express, místo aby se zabránilo toto omezení.

### <a name="cross-domain-connection-not-set-up-properly"></a>Není nastaveno správně připojení mezi doménami

Pokud připojení mezi doménami není správně nastaven (připojení, pro kterou SignalR adresa URL není ve stejné doméně jako hostování stránka), připojení se pravděpodobně nezdaří bez chybovou zprávu. Informace o tom, jak povolit komunikaci mezi doménami, najdete v článku [postup připojení mezi doménami](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a>Připojení pomocí protokolu NTLM (Active Directory) nefunguje v klientovi rozhraní .NET

Připojení v aplikaci klienta rozhraní .NET, který používá zabezpečení domény může selhat, pokud připojení není správně nakonfigurována. Pokud chcete používat funkce SignalR v prostředí domény, nastavte vlastnost požadavků připojení takto:

**C# klienta kód, který implementuje přihlašovací údaje pro připojení**

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="pong"></a>

## <a name="configuring-iis-websockets-to-pingpong-to-detect-a-dead-client"></a>Konfigurace služby IIS websocket pro příkaz ping/pong ke zjištění neaktivní klienta

Servery SignalR neznáte, pokud je klient mrtvých nebo Ne, a spoléhají na oznámení z základní websocket chyb připojení, který je zpětné volání při zavření. Jedno řešení tohoto problému je konfigurace služby IIS websocket udělat ping/pong pro vás. Tím se zajistí, že připojení se zavře, pokud to naruší neočekávaně. Další informace najdete v části [tento příspěvek stackoverflow](http://stackoverflow.com/questions/19502755/websocket-clients-state-not-changing-on-network-loss).

<a id="other"></a>

## <a name="other-connection-issues"></a>Jiné potíže s připojením

Tato část popisuje příčiny a řešení pro konkrétní příznaky nebo chybové zprávy, které se vyskytnou při připojení.

### <a name="start-must-be-called-before-data-can-be-sent-error"></a>"Spuštění musí být voláno před odesláním dat" Chyba

Tato chyba obvykle nastává, pokud kód odkazuje SignalR objekty před zahájením připojení. Wireup pro obslužné rutiny a podobně, budou volání metody definované na serveru musí přidat po dokončení připojení. Všimněte si, že volání `Start` je asynchronní, takže kód po volání může provést dříve, než se dokončí. Nejlepší způsob, jak přidat obslužné rutiny po spuštění připojení úplně je umístí je do funkce zpětného volání, která je jako parametr předaný metodě počáteční:

**Kód jazyka JavaScript klienta, který správně přidá obslužné rutiny událostí, které odkazují na objekty SignalR**

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

Tato chyba se zobrazí také v případě připojení zastaví při SignalR objekty jsou stále se na ně odkazovat.

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a>"Trvale přesunut 301" nebo "dočasně přesunout 302" Chyba

Tato chyba může být zobrazena, pokud projekt obsahuje složku s názvem SignalR, který koliduje s proxy serverem, automaticky vytvoří. Chcete-li se vyhnout této chybě, nepoužívejte složku s názvem `SignalR` v aplikaci, nebo vypněte vytváření automatické proxy vypnout. V tématu [The generované Proxy a jakým způsobem vám](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) další podrobnosti.

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a>"403 zakázán" došlo k chybě v rozhraní .NET nebo Silverlight klienta

K této chybě může dojít v prostředí napříč doménami, kde není správně povolit komunikaci mezi doménami. Informace o tom, jak povolit komunikaci mezi doménami, najdete v článku [postup připojení mezi doménami](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain). Vytvoření připojení mezi doménami v klienta Silverlight naleznete v části [napříč doménami připojení z klientů Silverlight](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).

### <a name="404-not-found-error"></a>Chyba "404 nebyl nalezen."

Existuje několik příčin tohoto problému. Ověřte všechny z následujících akcí:

- **Odkaz adresu proxy server rozbočovače není správně naformátovaný:** tato chyba obvykle nastává Pokud odkaz na adresu proxy serveru generovaného centra není správně naformátovaná. Ověřte, že je odkaz na adresu rozbočovače provedeny správně. V tématu [jak odkazovat dynamicky generovaném proxy](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) podrobnosti.
- **Přidání tras do aplikace před přidáním trasu rozbočovače:** Pokud vaše aplikace používá další postupy, ověřte, zda je první trasy přidat volání `MapSignalR`.
- **Pomocí služby IIS 7 nebo 7.5 bez aktualizace pro adresy URL bez přípony:** pomocí služby IIS 7 nebo 7.5 vyžaduje před spuštěním akce aktualizace pro adresy URL bez přípony, aby server může poskytnout přístup k centru definice v `/signalr/hubs`. Aktualizace lze nalézt [zde](https://support.microsoft.com/kb/980368).
- **Služba IIS mezipaměti zastaralá nebo poškození:** Pokud chcete ověřit, že obsah mezipaměti nejsou zastaralý, zadejte následující příkaz v okně prostředí PowerShell a vymažte mezipaměť:

    [!code-powershell[Main](troubleshooting/samples/sample11.ps1)]

### <a name="500-internal-server-error"></a>"Error 500 interního serveru.

Toto je velmi obecná chyba, která může mít celou řadu příčin. Informace o chybě by se měla objevit v protokolu událostí serveru nebo naleznete prostřednictvím ladění serveru. Když zapnete podrobné chyby na serveru může získat podrobnější informace o chybě. Další informace najdete v tématu [způsob zpracování chyb ve třídě rozbočovače](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).

Tato chyba také běžně dochází, pokud brána firewall nebo proxy serveru není správně nakonfigurována, způsobuje hlavičky žádosti třeba přepsat. Řešením je zajistit, že port 80 je povolena v bráně firewall nebo proxy serveru.

### <a name="unexpected-response-code-500"></a>"Neočekávanou odpověď kód: 500"

Této chybě může dojít, pokud verzi rozhraní .NET framework použitou v aplikaci neodpovídá verze zadaná v souboru Web.Config. Řešení je ověřit, že rozhraní .NET 4.5 se používá v nastavení aplikace a v souboru Web.Config.

### <a name="typeerror-lthubtypegt-is-undefined-error"></a>"TypeError: &lt;hubType&gt; není definován" Chyba

K této chybě dojde, pokud volání `MapSignalR` není provedené správně. V tématu [způsob registrace SignalR Middleware a konfigurovat možnosti SignalR](../guide-to-the-api/hubs-api-guide-server.md#route) Další informace.

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a>JsonSerializationException byl neošetřená pomocí uživatelského kódu

Ověřte, že parametry, které poslat vaše metody nezahrnují-Serializovatelné typy (například obslužných rutin souborů nebo připojení k databázi). Pokud budete muset použít členy na straně serveru objekt, který nechcete, aby k odeslání do klienta (buď pro zabezpečení nebo z důvodů serializace), použijte `JSONIgnore` atribut.

### <a name="protocol-error-unknown-transport-error"></a>"Chyba protokolu: Neznámý přenos" Chyba

Této chybě může dojít, pokud klient nepodporuje přenosy, které používá SignalR. V tématu [přenosy a případech Přejít](../getting-started/introduction-to-signalr.md#transports) informace, na kterém prohlížečů lze použít s SignalR.

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a>"Vytváření proxy JavaScript Hub bylo zakázáno."

K této chybě dojde, pokud `DisableJavaScriptProxies` je nastaven při také včetně odkazu na dynamicky vygenerovaný proxy server na `signalr/hubs`. Další informace o vytvoření proxy serveru ručně, najdete v části [vygenerovaný proxy server a jakým způsobem vám](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a>"ID připojení je v nesprávném formátu" nebo "identity uživatele se nemůže změnit během aktivního připojení SignalR" Chyba

Tato chyba může být zobrazena, pokud se používá ověřování a klient se zaznamená před ukončením připojení. Řešení je k zastavení před protokolování klienta připojení SignalR.

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a>"Nezachycená Chyba: SignalR: jQuery nebyl nalezen. Zkontrolujte, že jQuery odkazuje před soubor SignalR.js"Chyba

Klient SignalR JavaScript vyžaduje jQuery ke spuštění. Ověřte, zda je správný, zda cesty použité platná a že je odkaz na jQuery před odkaz na SignalR vaši informaci, abyste jQuery.

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a>"Nezachycená TypeError: Nelze načíst vlastnost '&lt;vlastnost&gt;' undefined" Chyba

Tato chyba se výsledkem nemá jQuery nebo proxy server rozbočovače odkazovaný správně. Ověřte, zda je správný, zda cesta používaná platná a že je odkaz na jQuery před odkaz na proxy server rozbočovače vaši informaci, abyste jQuery a proxy server rozbočovače. Výchozí odkaz na proxy server rozbočovače by měl vypadat asi takto:

**Kód na straně klienta HTML, která správně odkazuje na proxy server rozbočovače**

[!code-html[Main](troubleshooting/samples/sample12.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a>Chyba "RuntimeBinderException byl neošetřená pomocí uživatelského kódu."

Této chybě může dojít při přetížení nesprávné `Hub.On` se používá. Pokud metoda má návratovou hodnotu, návratový typ musí být zadány jako parametr obecného typu:

**Metody definované v klientovi (bez vygenerovaný proxy server)**

[!code-html[Main](troubleshooting/samples/sample13.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a>ID připojení je nekonzistentní nebo připojení dělí mezi načtení stránky

Toto chování je záměrné. Vzhledem k tomu, že objekt rozbočovače hostovaný v objektu page, rozbočovače zničena při aktualizaci stránky. Aplikace s více stránkami musí udržovat přidružení mezi uživateli a ID připojení tak, aby byla konzistentní mezi načtení stránky. ID připojení mohou být uloženy na serveru buď `ConcurrentDictionary` objekt nebo databáze.

### <a name="value-cannot-be-null-error"></a>Chyba "Hodnota nemůže být null."

Metody na straně serveru s volitelné parametry nejsou aktuálně podporovány; Pokud je volitelný parametr vynechán, metoda selže. Další informace najdete v tématu [volitelné parametry](https://github.com/SignalR/SignalR/issues/324).

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a>"Firefox nemůže navázat připojení k serveru na &lt;adresu&gt;" došlo k chybě v Firebug

Tato chybová zpráva se zobrazí v Firebug Pokud selže vyjednávání protokolu WebSocket přenosu a místo toho používá jiný přenos. Toto chování je záměrné.

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a>Chyba "Vzdálený certifikát není platný podle procedury ověřování" v aplikaci klienta rozhraní .NET

Pokud server vyžaduje vlastní klientské certifikáty, potom můžete přidat certifikátu x 509 na připojení před požadavku. Přidání certifikátu pro připojení s využitím `Connection.AddClientCertificate`.

### <a name="connection-drops-after-authentication-times-out"></a>Zrušení připojení vyčkat před po vypršení časového limitu ověřování

Toto chování je záměrné. Pověření ověřování nejde změnit, když je připojení aktivní; Aktualizujte přihlašovací údaje, musí být připojení zastavena a restartována.

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a>Onconnected rozbočovače volala dvakrát, při použití technologie jQuery Mobile

jQuery Mobile je `initializePage` funkce vynutí skripty v každé stránce znovu spouštění, proto vytváření druhé připojení. Řešení tohoto problému patří:

- Zahrnout odkaz na architekturu jQuery Mobile před souboru JavaScript.
- Zakažte `initializePage` funkce nastavením `$.mobile.autoInitializePage = false`.
- Počkejte, než pro stránku k dokončení inicializace před spuštěním připojení.

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a>Zprávy jsou odložené v aplikace Silverlight pomocí události odeslané serverem

Zprávy jsou zpoždění při používání serveru odeslání události na Silverlight. Chcete-li vynutit dlouhé dotazování místo toho použít, použijte následující postupy při spouštění připojení:

[!code-css[Main](troubleshooting/samples/sample14.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a>Použití "Oprávnění byl odepřen" navždy rámce protokolu

Jedná se o známý problém popsaný [zde](https://github.com/SignalR/SignalR/issues/1963). Tento příznak může zobrazit pomocí nejnovější knihovny JQuery; Přejít na starší verzi aplikace JQuery 1.8.2 je alternativní řešení.

### <a name="invalidoperationexception-not-a-valid-web-socket-request"></a>"InvalidOperationException: není platný webové žádosti soketu.

Této chybě může dojít, pokud se používá protokol WebSocket, ale síťového proxy serveru je úprava hlavičky žádosti. Řešení je nakonfigurovat proxy server umožňuje WebSocket na portu 80.

### <a name="exception-ltmethod-namegt-method-could-not-be-resolved-when-client-calls-method-on-server"></a>"Výjimka: &lt;název metody&gt; metoda nebylo možné přeložit" při klienta volá metodu na serveru

Tato chyba může být následek použití datových typů, které nelze zjistit v datové části JSON, jako je například pole. Alternativní řešení je použití datový typ, který je zjistitelný ve formátu JSON, jako je například IList. Další informace najdete v tématu [klient .NET nelze provést volání metod rozbočovače s parametry pole](https://github.com/SignalR/SignalR/issues/2672).

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a>Chyby kompilace a na straně serveru

 Následující oddíl obsahuje možná řešení kompilátoru a chyby za běhu na straně serveru. 

### <a name="reference-to-hub-instance-is-null"></a>Odkaz na instanci rozbočovače má hodnotu null

Vzhledem k tomu, že je pro každé připojení vytvořená hub instance, nelze vytvořit instanci rozbočovače ve vašem kódu sami. Volání metody v rozbočovači z mimo rozbočovače sám sebe, najdete v tématu [jak volat metody klienta a spravovat skupiny z mimo třídy rozbočovače](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) pro získání odkazu na kontext rozbočovače.

### <a name="httpcontextcurrentsession-is-null"></a>HTTPContext.Current.Session má hodnotu null.

Toto chování je záměrné. SignalR nepodporuje stavu relace ASP.NET, vzhledem k tomu, že povolení stavu relace by rozdělit duplexní zasílání zpráv.

### <a name="no-suitable-method-to-override"></a>Žádná vhodná metoda. k přepsání

Tato chyba nastane, pokud používáte kód z starší dokumentaci a blogů. Ověřte, že nejsou odkazující na názvy metod, které byly změněny nebo zastaralé (jako je `OnConnectedAsync`).

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a>HostContextExtensions.WebSocketServerUrl má hodnotu null.

Toto chování je záměrné. Tento člen je zastaralá a by se neměla používat.

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a>Chyba "trasu s názvem 'signalr.hubs' je již v kolekci trasy"

Tato chyba se zobrazí v případě `MapSignalR` volá dvakrát vaší aplikace. Některé aplikace volání příklad `MapSignalR` přímo ve třídě spuštění; ostatní použije volání v obálkovou třídu. Ujistěte se, že vaše aplikace není proveďte obojí.

### <a name="websocket-is-not-used"></a>Protokol WebSocket se nepoužívá.

Pokud jste ověřili, že serverem a klienty splňují požadavky protokolu WebSocket (uvedené v [podporované platformy](../getting-started/supported-platforms.md) dokumentu), budete muset povolit protokolu WebSocket na serveru. Tohoto postupu lze nalézt [zde](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support).

### <a name="connection-is-undefined"></a>$.connection není definován

Tato chyba znamená, že buď skripty na stránce správně nebyly načteny, nebo proxy server rozbočovače není dostupný nebo je nesprávně přistupuje. Ověřte, zda skript odkazy na stránku odpovídají skripty načtené v projektu, a že /signalr/hubs můžete získat přístup k v prohlížeči na serveru běží.

### <a name="one-or-more-types-required-to-compile-a-dynamic-expression-cannot-be-found"></a>Jeden nebo více typů, které jsou potřebné ke kompilaci dynamické výraz nebyl nalezen.

Tato chyba znamená, že `Microsoft.CSharp` chybí knihovna. Přidejte ho **sestavení -&gt;Framework** kartě.

### <a name="caller-state-cannot-be-accessed-from-clientscaller-in-visual-basic-or-in-a-strongly-typed-hub-conversion-from-type-taskof-object-to-type-string-is-not-valid-error"></a>Stav volajícího nelze získat přístup z Clients.Caller v jazyce Visual Basic nebo v rozbočovači silného typu; Chyba "Převod z typu 'úloh (objekt z), na typ 'Řetězec' není platný"

Chcete-li získat přístup k stav volajícího v jazyce Visual Basic nebo v rozbočovači silného typu, použijte `Clients.CallerState` vlastnost (dostupné ve verzi SignalR 2.1) místo `Clients.Caller`.

<a id="vs"></a>

## <a name="visual-studio-issues"></a>Problémy v sadě Visual Studio

Tato část popisuje problémy v sadě Visual Studio.

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a>Skript dokumenty uzel neobjeví v Průzkumníku řešení

Některé z našich kurzů vás nasměrovat k uzlu "Dokumentů skriptu" v Průzkumníku řešení při ladění. Tento uzel je produkovaný ladicího programu JavaScript a zobrazí se pouze při ladění klienty prohlížeče v Internet Exploreru; uzel se nezobrazí, pokud se používají Chrome nebo Firefox. Ladicí program JavaScript se nespustí, i pokud jiný klient ladicí program běží, jako je například ladicí program Silverlight.

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a>SignalR nefunguje v sadě Visual Studio 2008 nebo dřívější

Toto chování je záměrné. SignalR vyžaduje rozhraní .NET Framework 4 nebo novější; To vyžaduje, aby aplikací SignalR vývoje v sadě Visual Studio 2010 nebo novější. Komponenta serveru SignalR vyžaduje rozhraní .NET Framework 4.5.

<a id="iis"></a>

## <a name="iis-issues"></a>Problémy služby IIS

Tato část obsahuje problémy se službou IIS.

### <a name="signalr-works-on-visual-studio-development-server-but-not-in-iis"></a>SignalR funguje na vývojový server sady Visual Studio, ale není ve službě IIS

SignalR je podporována v IIS 7.0 a 7.5, ale podpora pro adresy URL bez přípony, musí být přidaný. Chcete-li přidat podporu pro adresy URL bez přípony, přečtěte si téma [https://support.microsoft.com/kb/980368](https://support.microsoft.com/kb/980368)

SignalR vyžaduje ASP.NET, aby mohlo být nainstalován na serveru (technologie ASP.NET není nainstalována ve službě IIS ve výchozím nastavení). Instalace technologie ASP.NET, najdete v části [ASP.NET stáhne](https://www.asp.net/downloads).

<a id="azure"></a>

## <a name="microsoft-azure-issues"></a>Problémy Microsoft Azure

Tato část obsahuje problémy s Microsoft Azure.

### <a name="fileloadexception-when-hosting-signalr-in-an-azure-worker-role"></a>FileLoadException při hostování SignalR roli pracovního procesu Azure

Hostování SignalR roli pracovního procesu Azure může mít za následek výjimku "nelze načíst soubor nebo sestavení ' Microsoft.Owin, verze = 2.0.0.0". Jedná se o známý problém s NuGet; V projektech Role pracovního procesu Azure nejsou automaticky přidá přesměrování vazby. Chcete-li odstranit tento problém, můžete přidat přesměrování vazby ručně. Přidejte následující řádky, které se `app.config` v souboru projektu Role pracovního procesu.

[!code-xml[Main](troubleshooting/samples/sample15.xml)]

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a>Prostřednictvím Azure propojovacího rozhraní po Změna tématu názvy nejsou přijaty zprávy.

V tématech, která používá Azure propojovacího rozhraní jsou zachována interně; nejsou určeny jako uživatelsky konfigurovatelného.
