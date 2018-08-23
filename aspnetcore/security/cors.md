---
title: Povolení žádostí napříč zdroji (CORS) v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak CORS jako standard pro povolení nebo odmítnutí žádostí nepůvodního v aplikaci ASP.NET Core.
ms.author: riande
ms.date: 08/17/2018
uid: security/cors
ms.openlocfilehash: 0dbb7933c76bb0d1d0cab519ea08c6c8f0ebedfd
ms.sourcegitcommit: 64c2ca86fff445944b155635918126165ee0f8aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2018
ms.locfileid: "41756746"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>Povolení žádostí napříč zdroji (CORS) v ASP.NET Core

Podle [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), a [Petr Dykstra](https://github.com/tdykstra)

Zabezpečení prohlížečů brání zasílání požadavků AJAX na jinou doménu na webové stránce. Toto omezení je volána *zásada stejného zdroje*a brání škodlivým webům ve čtení citlivých dat z jiné lokality. Ale v některých případech můžete chtít nechat jiných lokalit, zkontrolujte požadavky cross-origin do webového rozhraní API.

[Mezi sdílení zdrojů původu](http://www.w3.org/TR/cors/) (CORS) je standard W3C, která umožňuje server zmírnit zásadu stejného zdroje. Pomocí CORS, server explicitně můžou některé požadavky cross-origin zatímco jiné odmítnout. CORS je bezpečnější a flexibilnější, než starší techniky, jako [JSONP](https://wikipedia.org/wiki/JSONP). Toto téma ukazuje, jak povolit CORS v aplikaci ASP.NET Core.

## <a name="what-is-same-origin"></a>Co je "stejné zdroj"?

Dvě adresy URL mají stejný původ, pokud mají stejné schémata, hostitele a porty. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Tyto dvě adresy URL mají stejného původu:

* `http://example.com/foo.html`

* `http://example.com/bar.html`

Tyto adresy URL mají různé zdroje než ta předchozí dvě:

* `http://example.net` -Jinou doménu

* `http://www.example.com/foo.html` – Různé subdomény

* `https://example.com/foo.html` -Jiné schéma

* `http://example.com:9000/foo.html` -Jiný port

> [!NOTE]
> Aplikace Internet Explorer nezahrne port při porovnání zdrojů.

## <a name="enable-cors"></a>Povolení CORS

::: moniker range="<= aspnetcore-1.1"

Chcete-li nastavení CORS pro vaši aplikaci přidejte `Microsoft.AspNetCore.Cors` do svého projektu balíček.

::: moniker-end

Volání [AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) v `Startup.ConfigureServices`:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a>Povolení CORS s middlewaru

Postup pro povolení CORS, přidat CORS middleware do kanálu pomocí požadavku `UseCors` – metoda rozšíření. CORS middleware musí předcházet všechny koncové body definované ve vaší aplikaci ve které chcete podporovat požadavky cross-origin (například před voláním `UseMvc`).

Zásady nepůvodního lze zadat při přidání pomocí middleware CORS [CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) třídy. Existují dva způsoby, jak to provést. První je volání `UseCors` pomocí výrazu lambda:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

**Poznámka:** adresa URL musí být zadán bez koncové lomítko (`/`). Pokud adresa URL se ukončí s `/`, vrátí porovnání `false` a nevrátí se žádné záhlaví.

Používá výraz lambda `CorsPolicyBuilder` objektu. Seznam najdete [možnosti konfigurace](#cors-policy-options) dále v tomto tématu. V tomto příkladu, zásada umožňuje požadavky cross-origin z `http://example.com` a žádné jiné zdroje.

CorsPolicyBuilder má rozhraní API fluent, takže můžete vytvořit řetězové volání metody:

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

Druhý postup je definovat jeden nebo více s názvem zásady CORS, a potom vyberte zásady podle názvu v době běhu.

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

V tomto příkladu přidá zásada CORS, s názvem "AllowSpecificOrigin". Vyberte zásadu, předejte název, který má `UseCors`.

## <a name="enabling-cors-in-mvc"></a>Povolení CORS v aplikaci MVC

Chcete-li použít konkrétní CORS na akce na kontroler nebo globálně pro všechny řadiče můžete také použít MVC. Při použití MVC k povolení CORS se používají stejné CORS služby, ale není middlewarem CORS.

### <a name="per-action"></a>Za akci

K určení přidat zásadu CORS pro určité akce `[EnableCors]` atribut na akci. Zadejte název zásady.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a>Na kontroler

Chcete-li určit zásady CORS pro konkrétní ovladač přidat `[EnableCors]` atribut třídy kontroleru. Zadejte název zásady.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a>Globálně

Můžete povolit CORS globálně pro všechny řadiče tak, že přidáte `CorsAuthorizationFilterFactory` filtr do kolekce globálních filtrů:

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

Pořadí priority je: akci, kontroler, globální. Zásady na úrovni akce mají přednost zásady na úrovni kontroleru a zásad na úrovni kontroleru přednost před globální zásady.

### <a name="disable-cors"></a>Zákazu sdílení CORS

Chcete-li zakázat CORS pro kontroler nebo akce, použijte `[DisableCors]` atribut.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a>Možnosti zásad CORS

Tato část popisuje různé možnosti, které můžete nastavit zásadu CORS.

* [Nastavte povolené zdroje](#set-the-allowed-origins)

* [Nastavte povolené metody HTTP](#set-the-allowed-http-methods)

* [Nastavit hlavičku povolené žádosti](#set-the-allowed-request-headers)

* [Nastavit hlavičky vystavené odpovědi](#set-the-exposed-response-headers)

* [Přihlašovací údaje v požadavky cross-origin](#credentials-in-cross-origin-requests)

* [Nastavit čas vypršení platnosti předběžné](#set-the-preflight-expiration-time)

Pro některé možnosti, může být užitečné přečíst [funguje jak CORS](#how-cors-works) první.

### <a name="set-the-allowed-origins"></a>Nastavte povolené zdroje

Chcete-li povolit jeden nebo více konkrétních zdrojů:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

Chcete-li povolit všechny původy:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

Pečlivě zvažte předtím, než žádosti z původu. To znamená, že doslova všechny weby mohl provádět volání AJAX do vašeho rozhraní API.

### <a name="set-the-allowed-http-methods"></a>Nastavte povolené metody HTTP

Chcete-li povolit všechny metody HTTP:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

Tímto je ovlivněn přípravné požadavků a hlavičky Access-ovládacího prvku-Allow-Methods.

### <a name="set-the-allowed-request-headers"></a>Nastavit hlavičku povolené žádosti

Předběžný požadavek CORS může obsahovat hlavičku Access-Control-Request-Headers, výpis hlavičky protokolu HTTP, nastavte aplikací (takzvaný ", vytvářet hlavičky žádosti").

Na seznamu povolených IP adres konkrétní záhlaví:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

Chcete-li povolit všechny vytvářet hlavičky žádosti:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

Prohlížeče nejsou zcela konzistentní v tom, jak nastavují Access-Control-Request-Headers. Pokud nastavíte záhlaví na něco jiného než "*", měli byste zahrnout alespoň "přijímat", "content-type" a "zdroj" a navíc jakékoli vlastní hlavičky, které chcete podporovat.

### <a name="set-the-exposed-response-headers"></a>Nastavit hlavičky vystavené odpovědi

Ve výchozím prohlížeči nezveřejňuje všechny hlavičky odpovědí do aplikace. (Viz [ http://www.w3.org/TR/cors/#simple-response-header ](http://www.w3.org/TR/cors/#simple-response-header).) Hlavičky odpovědi, které jsou k dispozici ve výchozím nastavení jsou:

* Cache-Control

* Jazyk obsahu

* Typ obsahu

* Vypršení platnosti

* Datum poslední změny

* Direktiva pragma

Specifikace CORS volá tyto *hlavičky odpovědi jednoduché*. Pro zpřístupnění dalších hlaviček pro aplikaci:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a>Přihlašovací údaje v požadavky cross-origin

Přihlašovací údaje vyžadují speciální zacházení v požadavku CORS. Ve výchozím nastavení nebude prohlížeč odeslat žádné přihlašovací údaje se žádostí nepůvodního zdroje. Přihlašovací údaje zahrnují soubory cookie, jakož i schémat ověřování protokolu HTTP. Odesílá pověření s žádostí nepůvodního zdroje, musíte nastavit klienta XMLHttpRequest.withCredentials na hodnotu true.

Přímo pomocí rozhraní XMLHttpRequest:

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

Na serveru navíc musíte povolit přihlašovací údaje. Povolit přihlašovací údaje nepůvodního zdroje:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

Odpověď HTTP teď bude obsahovat hlavičku přístup – ovládací prvek-Allow-Credentials, která sděluje prohlížeči, že server umožňuje přihlašovací údaje pro žádosti nepůvodního zdroje.

Pokud prohlížeč odesílá pověření, ale odpověď neobsahuje platnou hlavičku přístup – ovládací prvek-Allow-Credentials, prohlížeči nebude vystavení odpovědi do aplikace a požadavek AJAX selže.

Buďte opatrní při povolování pověření nepůvodního zdroje. Web v jiné doméně odeslat do aplikace jménem uživatele bez vědomí uživatele pověření přihlášeného uživatele. Specifikace CORS také uvádí nastavení počátky k `"*"` (všechny původy) je neplatná-li `Access-Control-Allow-Credentials` záhlaví je k dispozici.

### <a name="set-the-preflight-expiration-time"></a>Nastavit čas vypršení platnosti předběžné

Hlavička Access – ovládací prvek-Max-Age Určuje, jak dlouho může do mezipaměti odpovědi pro předběžný požadavek. Nastavení této hlavičky:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a>Jak funguje CORS

Tato část popisuje, co se stane, že v požadavku CORS na úrovni zprávy HTTP. Je důležité pochopit, jak CORS funguje tak, aby zásady CORS můžete nakonfigurovat správně a ladit, když dojde k neočekávané chování.

Specifikace CORS zavádí několik nové hlavičky protokolu HTTP, které umožňují požadavky cross-origin. Pokud je prohlížeč podporuje CORS, nastaví tyto hlavičky automaticky pro požadavky cross-origin. Vlastní kód jazyka JavaScript není vyžadován k povolení sdílení CORS.

Tady je příklad žádosti nepůvodního zdroje. `Origin` Záhlaví obsahuje domény, lokality, která odeslala žádost:

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

Pokud server umožňuje, aby žádosti, nastaví hlavičky Access-Control-Allow-Origin v odpovědi. Hodnotu této hlavičky odpovídá hlavička počátečního z požadavku nebo hodnota zástupný znak "*", což znamená, že jakýkoli původ je povoleno:

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

Pokud odpověď nebude obsahovat hlavičky Access-Control-Allow-Origin, požadavek AJAX selže. Konkrétně v prohlížeči zakazuje požadavku. I v případě, že server vrátí úspěšné odpovědi, prohlížeč nevyužívá odpovědi k dispozici pro klientské aplikace.

### <a name="preflight-requests"></a>Předběžných požadavků

U některých požadavků CORS prohlížeč odešle požadavek další, nazývá "předběžný požadavek", předtím, než odešle skutečnou žádost pro prostředek. Prohlížeči můžete přeskočit předběžný požadavek, pokud jsou splněny následující podmínky:

* Metoda žádosti je GET, HEAD nebo POST, a

* Aplikace není nastavena záhlaví požadavku než přijmout, Accept-Language, jazyka obsahu Content-Type nebo poslední-Event-ID, a

* Hlavička Content-Type (Pokud nastavit) je jedním z následujících akcí:

  * Application/x--www-form-urlencoded

  * multipart/formuláře data

  * text/plain

Pravidlo o hlavičky požadavku se vztahuje na hlavičky, které aplikace nastaví voláním setRequestHeader na objekt XMLHttpRequest. (Specifikaci CORS volá tyto "Autor hlavičky žádosti".) Toto pravidlo neplatí pro hlavičky, které můžete nastavit v prohlížeči, jako je například uživatelského agenta, hostitele nebo Content-Length.

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

Přípravné požadavek používá metodu HTTP OPTIONS. Obsahuje dva speciálními záhlavími:

* Access-Control-Request-Method: Metoda HTTP, který se použije pro aktuálního požadavku.

* Access-Control-Request-Headers: Seznam hlaviček požadavků, které aplikace nastavena na skutečnou žádost. (Znovu to nezahrnuje hlavičky, které nastaví prohlížeče.)

Tady je příklad odpovědi, za předpokladu, že server umožňuje žádosti:

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

Odpověď obsahuje hlavičku přístup – ovládací prvek-Allow-Methods, který obsahuje seznam povolených metod a volitelně hlavičky Access-Control-povolit-Headers, která zobrazuje povolené hlavičky. Pokud je předběžný požadavek úspěšné, prohlížeč odesílá skutečnou žádost, jak je popsáno výše.
