---
title: "Povolení žádostí napříč zdroji (CORS)"
author: rick-anderson
description: "Toto téma představuje CORS jako standard pro povolení nebo odmítnutí žádostí napříč zdroji v aplikaci ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 05/17/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/cors
ms.openlocfilehash: 9f53ce11f1659aa3416fe4fbb94183c64ab0dab5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="enabling-cross-origin-requests-cors"></a>Povolení žádostí napříč zdroji (CORS)

Podle [Karel Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), a [tní Dykstra](https://github.com/tdykstra)

Zabezpečení prohlížeče brání provedení požadavky AJAX do jiné domény na webové stránce. Toto omezení se nazývá *zásady stejného původu*a zabrání škodlivé weby čtení citlivá data z jiné lokality. Ale v některých případech můžete chtít nechat ostatních lokalit, zkontrolujte žádostí napříč zdroji do webového rozhraní API.

[Mezi sdílení prostředků původu](http://www.w3.org/TR/cors/) (CORS) je standard W3C, který umožňuje serveru zmírnit zásady stejného původu. Pomocí CORS, server explicitně povolit některé žádostí napříč zdroji při odmítnutí ostatní. CORS, jako je bezpečnější a flexibilnější než dřívější techniky [JSONP](https://wikipedia.org/wiki/JSONP). Toto téma ukazuje postup povolení CORS v aplikaci ASP.NET Core.

## <a name="what-is-same-origin"></a>Co je "stejné původ"?

Dvou adres URL mají stejný původ v případě, že mají stejné schémata, hostitele a porty. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Tyto dvě adresy URL mají stejný původ:

* `http://example.com/foo.html`

* `http://example.com/bar.html`

Tyto adresy URL mít různého původu než předchozí dva:

* `http://example.net`-Jiné domény

* `http://www.example.com/foo.html`-Různých subdomény

* `https://example.com/foo.html`-Jiné schéma

* `http://example.com:9000/foo.html`-Jiný port

> [!NOTE]
> Internet Explorer nepovažuje port při porovnávání zdroje.

## <a name="setting-up-cors"></a>Nastavení CORS

K nastavení CORS pro vaši aplikaci přidat `Microsoft.AspNetCore.Cors` balíčku do projektu.

Přidání služeb CORS v souboru Startup.cs:

[!code-csharp[Main](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a>Povolení CORS s middlewaru.

Chcete-li přidat CORS pro celou aplikaci CORS middleware do kanálu vaší žádosti o pomocí `UseCors` metoda rozšíření. Všimněte si, že CORS middleware musí předcházet žádné koncové body definované ve vaší aplikaci, která chcete podporovat žádostí napříč zdroji (např.) Před voláním jakékoli `UseMvc`).

Při přidávání pomocí middleware CORS můžete zadat zásady cross-origin `CorsPolicyBuilder` třídy. Chcete-li to provést dvěma způsoby. První je volání UseCors s lambda:

[!code-csharp[Main](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

**Poznámka:** bez koncové lomítko musíte zadat adresu URL (`/`). Pokud adresu URL ukončí s `/`, vrátí porovnání `false` , které budou vráceny žádné záhlaví.

Argument lambda trvá `CorsPolicyBuilder` objektu. Seznam najdete [možnosti konfigurace](#cors-policy-options) dál v tomto tématu. V tomto příkladu zásada umožňuje žádostí napříč zdroji z `http://example.com` a žádné jiné zdroje.

Všimněte si, že CorsPolicyBuilder obsahuje rozhraní fluent API, takže můžete řetězu volání metod:

[!code-csharp[Main](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

Druhý postup je definovat jeden nebo více s názvem zásady CORS, a potom vyberte zásady podle názvu v době běhu.

[!code-csharp[Main](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

Tento příklad přidá zásadu CORS s názvem "AllowSpecificOrigin". Vyberte zásadu, předat název, který má `UseCors`.

## <a name="enabling-cors-in-mvc"></a>Povolení CORS v MVC

Případně můžete MVC použít konkrétní CORS na každou akci, na jeden kontroler nebo globálně pro všechny řadiče. Při použití MVC k povolení sdílení CORS se používají stejné CORS služby, ale není CORS middleware.

### <a name="per-action"></a>Na každou akci

Zadejte přidat zásadu CORS pro určité akci `[EnableCors]` atribut akce. Zadejte název zásady.

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a>Na jeden kontroler

Zadejte zásady CORS pro konkrétní řadič přidat `[EnableCors]` atributu do třídy kontroleru. Zadejte název zásady.

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a>Globálně

Můžete povolit CORS globálně pro všechny řadiče přidáním `CorsAuthorizationFilterFactory` filtr do kolekce globálních filtrů:

[!code-csharp[Main](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

Pořadí priorit je: akci, kontroler, globální. Zásady na úrovni akce mají přednost před zásady na úrovni kontroleru a zásady na úrovni kontroleru mají přednost před globální zásady.

### <a name="disable-cors"></a>Zakázat CORS

Chcete-li zakázat CORS pro kontroler nebo akce, použijte `[DisableCors]` atribut.

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a>Možnosti zásad CORS

Tato část popisuje různé možnosti, které můžete nastavit v zásadu CORS.

* [Nastavit povolené zdroje](#set-the-allowed-origins)

* [Nastavení povolených metod HTTP](#set-the-allowed-http-methods)

* [Nastavit hlavičky povolených požadavků](#set-the-allowed-request-headers)

* [Nastavit hlavičky zveřejněné odpovědi](#set-the-exposed-response-headers)

* [Přihlašovací údaje v žádostí napříč zdroji](#credentials-in-cross-origin-requests)

* [Nastavte hodnotu doby předběžných vypršení platnosti](#set-the-preflight-expiration-time)

Pro některé možnosti může být užitečné číst [funguje jak CORS](#how-cors-works) první.

### <a name="set-the-allowed-origins"></a>Nastavit povolené zdroje

Chcete-li povolit jeden nebo více konkrétních zdroje:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=19-23)]

Chcete-li povolit všechny původy:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs??range=27-31)]

Pečlivě zvažte, před povolením požadavky od jakýkoli původ. Znamená to, že názvy všech webu provádět volání AJAX k vašemu rozhraní API.

### <a name="set-the-allowed-http-methods"></a>Nastavení povolených metod HTTP

Chcete-li povolit všechny metody HTTP:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=44-49)]

To ovlivní letu předběžné požadavky a záhlaví přístupu – ovládací prvek-Allow-Methods.

### <a name="set-the-allowed-request-headers"></a>Nastavit hlavičky povolených požadavků

Předběžný požadavek CORS může obsahovat hlavičku Access-Control-Request-Headers výpis hlavičky protokolu HTTP, které jsou nastaveny aplikací (takzvaný "vytvářet hlavičky žádosti").

Pro konkrétní hlavičky seznamu povolených IP adres:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=53-58)]

Umožňuje vytvářet všechny hlavičky žádosti:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=62-67)]

Nejsou prohlížeče zcela v souladu v tom, jak nastavují Access-Control-Request-Headers. Pokud nastavíte hlavičky k ničemu jiné než "*", by měla obsahovat alespoň "přijmout", "content-type" a "Původ" a jakékoli vlastní hlavičky, které chcete podporovat.

### <a name="set-the-exposed-response-headers"></a>Nastavit hlavičky zveřejněné odpovědi

Ve výchozím prohlížeči nezveřejňuje všechny hlavičky odpovědi do aplikace. (Viz [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) Hlavičky odpovědi, které jsou k dispozici ve výchozím nastavení jsou:

* Cache-Control

* Obsah – jazyk

* Typ obsahu

* Vypršení platnosti

* Poslední úpravy

* Direktiva pragma

Specifikace CORS volá tyto *hlavičky odpovědi jednoduché*. Pro zpřístupnění jiných hlavičky pro aplikaci:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a>Přihlašovací údaje v žádostí napříč zdroji

Přihlašovací údaje vyžadují zvláštní zpracování v požadavek CORS. Ve výchozím prohlížeči neodešle všechny přihlašovací údaje s žádostí napříč zdroji. Přihlašovací údaje zahrnují soubory cookie, jakož i schémat ověřování protokolu HTTP. K odeslání pověření s žádostí napříč zdroji, musí klient nastavit XMLHttpRequest.withCredentials na hodnotu true.

Pomocí rozhraní XMLHttpRequest přímo:

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

V jQuery:

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

Kromě toho serveru musí umožňovat přihlašovací údaje. Chcete-li povolit cross-origin přihlašovací údaje:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=80-85)]

Odpověď HTTP bude teď obsahovat přístup – ovládací prvek-Allow-Credentials záhlaví, která sděluje prohlížeči, že server umožňuje přihlašovací údaje pro žádost o nepůvodního zdroje.

Pokud prohlížeč odesílá přihlašovací údaje, ale odpověď neobsahuje platný přístup – ovládací prvek-Allow-Credentials záhlaví, prohlížeče nebude vystavit odpověď na žádost a vyvolá chybu požadavek AJAX.

Pečlivě velmi o povolení cross-origin přihlašovací údaje, protože znamená to, že web v jiné doméně může odesílat pověření přihlášeného uživatele do vaší aplikace jménem uživatele, aniž by uživatel znal. Sdílení CORS spec také stavy tohoto nastavení počátky k "*" (všechny původy) je neplatný, pokud je k dispozici přístup – ovládací prvek-Allow-Credentials záhlaví.

### <a name="set-the-preflight-expiration-time"></a>Nastavte hodnotu doby předběžných vypršení platnosti

Záhlaví přístupu – ovládací prvek-Max-Age Určuje, jak dlouho může do mezipaměti odpovědi na předběžný požadavek. Chcete-li nastavit tuto hlavičku:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a>Jak funguje CORS

Tato část popisuje, co se stane, že v žádosti o CORS na úrovni zpráv protokolu HTTP. Je důležité pochopit, jak CORS funguje, takže můžete nakonfigurovat zásady CORS správně a vyřešit případné věcí nefungují podle očekávání.

Specifikace CORS zavádí několik nových hlavičky HTTP, které povolení žádostí napříč zdroji. Pokud prohlížeč podporuje CORS, nastaví tato záhlaví automaticky pro požadavky cross-origin; nemusíte provádět žádné zvláštní v kódu jazyka JavaScript.

Tady je příklad žádost nepůvodního zdroje. Záhlaví "Původ" dává domény, lokality, která odeslala žádost:

```
GET http://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: http://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: http://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

Pokud server umožňuje žádost, nastaví hlavičky Access-Control-Allow-Origin v odpovědi. Hodnotu této hlavičky buď odpovídá počátek hlavička ze žádosti, nebo je hodnota zástupný znak "*", což znamená, že je povoleno jakýkoli původ:

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

Pokud odpověď neobsahuje hlavičky Access-Control-Allow-Origin, požadavek AJAX se nezdaří. V prohlížeči přímo, neumožňuje žádosti. I když server vrátí úspěšné odpovědi, prohlížeč nepodporuje zpřístupnit odpověď do klientské aplikace.

### <a name="preflight-requests"></a>Předběžných požadavků

U některých požadavků CORS prohlížeč odešle požadavek další, nazývá "předběžný požadavek", než odešle skutečné požadavek na prostředek. V prohlížeči můžete přeskočit předběžný požadavek, pokud jsou splněny následující podmínky:

* Metoda požadavku je GET, HEAD nebo POST, a

* Aplikace není nastavena všechny hlavičky požadavku než přijmout, Accept-Language, jazyk obsahu Content-Type nebo poslední událost ID, a

* Hlavička Content-Type (Pokud nastavit) je jedním z následujících akcí:

  * Application/x--www-form-urlencoded

  * multipart/form-data

  * text/plain

Pravidlo o hlavičky požadavku platí pro hlavičky, které aplikace nastaví voláním setRequestHeader XMLHttpRequest objektu. (Specifikace CORS volá tyto "Autor hlavičky žádosti"). Pravidlo se nevztahuje na hlavičky, které můžete nastavit v prohlížeči, třeba uživatelského agenta, hostitele nebo Content-Length.

Tady je příklad předběžný požadavek:

```
OPTIONS http://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: http://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

Žádost o předběžné letu používá metodu HTTP OPTIONS. Obsahuje dva speciální hlavičky:

* Access-Control-Request-Method: Metodu protokolu HTTP, který se použije pro skutečný požadavek.

* Access-Control-Request-Headers: Seznam hlaviček požadavků, které aplikace nastavena na skutečnou žádost. (Znovu to neobsahuje hlavičky, které nastaví prohlížeče.)

Zde je odpověď příklad, za předpokladu, že server umožňuje žádost:

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

Odpověď obsahuje hlavičku přístup – ovládací prvek-Allow-Methods, který obsahuje seznam povolených metod a volitelně hlavičky Access-Control-povolit-Headers, kde jsou uvedeny povolené hlavičky. Pokud předběžný požadavek úspěšné, prohlížeč odesílá skutečné požadavku, jak je popsáno výše.
