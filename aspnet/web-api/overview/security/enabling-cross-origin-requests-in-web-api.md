---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: "Povolení žádostí napříč zdroji v rozhraní ASP.NET Web API 2 | Microsoft Docs"
author: MikeWasson
description: "Ukazuje, jak pro podporu sdílení prostředků různých původů (CORS) v rozhraní ASP.NET Web API."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/15/2014
ms.topic: article
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 453ad29ff4f10f9660f3aa8bab358519b4cfd48b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="enabling-cross-origin-requests-in-aspnet-web-api-2"></a>Povolení žádostí napříč zdroji v rozhraní ASP.NET Web API 2
====================
podle [Wasson Jan](https://github.com/MikeWasson)

> Zabezpečení prohlížeče brání provedení požadavky AJAX do jiné domény na webové stránce. Toto omezení se nazývá *zásady stejného původu*a zabrání škodlivé weby čtení citlivá data z jiné lokality. Ale v některých případech můžete chtít umožní jiných lokalitách volání webového rozhraní API.
> 
> [Mezi sdílení prostředků původu](http://www.w3.org/TR/cors/) (CORS) je standard W3C, který umožňuje serveru zmírnit zásady stejného původu. Pomocí CORS, server explicitně povolit některé žádostí napříč zdroji při odmítnutí ostatní. CORS, jako je bezpečnější a flexibilnější než dřívější techniky [JSONP](http://en.wikipedia.org/wiki/JSONP). Tento kurz ukazuje postup povolení CORS v aplikaci webového rozhraní API.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - [Visual Studio 2013 Update 2](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Webové rozhraní API 2.2


<a id="intro"></a>
## <a name="introduction"></a>Úvod

Tento kurz představuje podporu CORS v rozhraní ASP.NET Web API. Začneme vytvořením dva projekty ASP.NET – jeden názvem "Webová služba", který hostuje kontroleru webového rozhraní API, a další názvem "webový klient", která volá webové služby. Protože dvě možná využití jsou hostované v různých doménách, je požadavek AJAX z WebClient webovou žádost cross-origin.

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a>Co je "Stejné původ"?

Dvou adres URL mají stejný původ v případě, že mají stejné schémata, hostitele a porty. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Tyto dvě adresy URL mají stejný původ:

- `http://example.com/foo.html`
- `http://example.com/bar.html`

Tyto adresy URL mít různého původu než předchozí dva:

- `http://example.net`-Jiné domény
- `http://example.com:9000/foo.html`-Jiný port
- `https://example.com/foo.html`-Jiné schéma
- `http://www.example.com/foo.html`-Různých subdomény

> [!NOTE]
> Internet Explorer nebere v úvahu port při porovnávání zdroje.


<a id="create-webapi-project"></a>
## <a name="create-the-webservice-project"></a>Vytvoření projektu webové služby

> [!NOTE]
> V této části se předpokládá, že již víte, jak vytvořit projekty webového rozhraní API. Pokud ne, najdete v části [Začínáme s rozhraním ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).


Spuštění sady Visual Studio a vytvořte novou **webové aplikace ASP.NET** projektu. Vyberte **prázdný** šablona projektu. V části "Přidat složky a odkazy na základní" vyberte **webového rozhraní API** zaškrtávací políčko. Volitelně vyberte možnost "Hostitel v cloudu" k nasazení aplikace do Azure výrobci. Společnost Microsoft nabízí bezplatné webových hostitelských služeb pro až 10 webů ve [Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

[![](enabling-cross-origin-requests-in-web-api/_static/image3.png)](enabling-cross-origin-requests-in-web-api/_static/image2.png)

Přidání kontroleru webového rozhraní API s názvem `TestController` následujícím kódem:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

Můžete spustit aplikace místně nebo nasazení do Azure. (Snímky obrazovky v tomto kurzu, lze nasadit do Azure App Service Web Apps.) Chcete-li ověřit, zda je funkční webové rozhraní API, přejděte na `http://hostname/api/test/`, kde *hostname* je doména, kde jste nasadili aplikace. Měli byste vidět text odpovědi &quot;získat: testovací zpráva&quot;.

![](enabling-cross-origin-requests-in-web-api/_static/image4.png)

<a id="create-client"></a>
## <a name="create-the-webclient-project"></a>Vytvoření projektu WebClient

Vytvořte jiný projekt webové aplikace ASP.NET a vyberte **MVC** šablona projektu. Volitelně vyberte **změna ověřování** > **bez ověřování**. Nepotřebujete ověřování pro tento kurz.

[![](enabling-cross-origin-requests-in-web-api/_static/image6.png)](enabling-cross-origin-requests-in-web-api/_static/image5.png)

V Průzkumníku řešení otevřete soubor Views/Home/Index.cshtml. Nahraďte kód v tomto souboru s následujícími službami:

[!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

Pro *serviceUrl* proměnnou, použijte identifikátor URI aplikace webové služby. Nyní spusťte aplikaci WebClient místně nebo publikovat na jiný web.

Kliknutím na tlačítko "Zkuste ho" odešle požadavek AJAX na webovou aplikaci pomocí protokolu HTTP metody uvedené v rozevíracím (GET, POST nebo PUT). To umožňuje nám prozkoumat různých žádostí napříč zdroji. Teď, webovou aplikaci nepodporuje CORS, takže pokud kliknete na tlačítko, zobrazí se chyba.

![](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> Je-li sledovat provoz protokolu HTTP v nástroji jako [Fiddler](http://www.telerik.com/fiddler), uvidíte, že do prohlížeče odeslat požadavek GET a neproběhne úspěšně, ale volání AJAX vrátí chybu. Je důležité si uvědomit, že zásady stejného původu nebrání prohlížeče z *odesílání* žádosti. Místo toho ji brání aplikaci v zobrazuje *odpovědi*.


![](enabling-cross-origin-requests-in-web-api/_static/image8.png)

<a id="enable-cors"></a>
## <a name="enable-cors"></a>Povolení CORS

Nyní Pojďme povolení CORS v webovou aplikaci. Nejprve přidejte balíček CORS NuGet. V sadě Visual Studio z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**, pak vyberte **Konzola správce balíčků**. V okně konzoly Správce balíčků zadejte následující příkaz:

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

Tento příkaz nainstaluje nejnovější balíček a aktualizuje všechny závislosti, včetně základní knihovny webového rozhraní API. Uživatel-příznak verze cílit na konkrétní verzi. Vyžaduje balíček CORS webového rozhraní API 2.0 nebo novější.

Otevřete soubor aplikace\_Start/WebApiConfig.cs. Přidejte následující kód, který **WebApiConfig.Register** metoda.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

Dál přidejte **[EnableCors]** atribut `TestController` třídy:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

Pro *zdroje* parametr, použijte identifikátor URI, které jste nasadili webový klient aplikace. To umožňuje žádostí napříč zdroji z WebClient, při stále zakazuje všech ostatních požadavků napříč doménami. Později, I budete popisují parametry **[EnableCors]** podrobněji.

Nezahrnují lomítka na konci *zdroje* adresy URL.

Znovu nasaďte aktualizovanou webovou aplikaci. Nemusíte aktualizovat WebClient. Nyní požadavek AJAX WebClient uspěli. Metody GET a PUT, POST všechny povolené.

![](enabling-cross-origin-requests-in-web-api/_static/image9.png)

<a id="how-it-works"></a>
## <a name="how-cors-works"></a>Jak funguje CORS

Tato část popisuje, co se stane, že v žádosti o CORS na úrovni zpráv protokolu HTTP. Je důležité pochopit, jak funguje CORS, takže můžete nakonfigurovat **[EnableCors]** atribut správně a řešení potíží s Pokud věcí nefungují podle očekávání.

Specifikace CORS zavádí několik nových hlavičky HTTP, které povolení žádostí napříč zdroji. Pokud prohlížeč podporuje CORS, nastaví tato záhlaví automaticky pro požadavky cross-origin; nemusíte provádět žádné zvláštní v kódu jazyka JavaScript.

Tady je příklad žádost nepůvodního zdroje. Záhlaví "Původ" dává domény, lokality, která odeslala žádost.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

Pokud server umožňuje žádost, nastaví hlavičky Access-Control-Allow-Origin. Hodnotu této hlavičky buď odpovídá hlavičky počátku, nebo je hodnota zástupný znak "\*", což znamená, že jakýkoli původ je povolen.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

Pokud odpověď nezahrnuje hlavičky Access-Control-Allow-Origin, požadavek AJAX se nezdaří. V prohlížeči přímo, neumožňuje žádosti. I když server vrátí úspěšné odpovědi, prohlížeč není zpřístupnit odpověď do klientské aplikace.

**Předběžných požadavků**

U některých požadavků CORS prohlížeč odešle požadavek další, nazývá "předběžný požadavek", než odešle skutečné požadavek na prostředek.

V prohlížeči můžete přeskočit předběžný požadavek, pokud jsou splněny následující podmínky:

- Metoda požadavku je GET, HEAD nebo POST, *a*
- Aplikace není nastavena všechny hlavičky požadavku než přijmout, Accept-Language, jazyk obsahu Content-Type nebo poslední událost ID, *a*
- Hlavička Content-Type (Pokud nastavit) je jedním z následujících akcí: 

    - Application/x--www-form-urlencoded
    - multipart/formulář data
    - text/plain

Pravidlo o hlavičky požadavku se vztahuje na hlavičky, které aplikace nastaví voláním **setRequestHeader** na **XMLHttpRequest** objektu. (Specifikace CORS volá tyto "Autor hlavičky žádosti"). Pravidlo se nevztahuje na záhlaví *prohlížeče* můžete nastavit, jako je například uživatelský Agent, hostitele nebo Content-Length.

Tady je příklad předběžný požadavek:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

Žádost o předběžné letu používá metodu HTTP OPTIONS. Obsahuje dva speciální hlavičky:

- Access-Control-Request-Method: Metodu protokolu HTTP, který se použije pro skutečný požadavek.
- Access-Control-Request-Headers: Seznam hlaviček požadavku který *aplikace* nastavit na skutečnou žádost. (Znovu, nezahrnuje hlavičky, které nastaví prohlížeče.)

Zde je odpověď příklad, za předpokladu, že server umožňuje žádost:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

Odpověď obsahuje hlavičku přístup – ovládací prvek-Allow-Methods, který obsahuje seznam povolených metod a volitelně hlavičky Access-Control-povolit-Headers, kde jsou uvedeny povolené hlavičky. Pokud předběžný požadavek úspěšné, prohlížeč odesílá skutečné požadavku, jak je popsáno výše.

<a id="scope"></a>
## <a name="scope-rules-for-enablecors"></a>Pravidla rozsahu pro [EnableCors]

CORS můžete povolit na každou akci, na jeden kontroler nebo globálně pro všechny řadiče webového rozhraní API ve vaší aplikaci.

**Na každou akci**

Chcete-li povolit CORS pro jednu akci, nastavte **[EnableCors]** atribut na metodu akce. Následující příklad umožňuje použití CORS pro `GetItem` pouze metoda.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

**Na jeden kontroler**

Pokud nastavíte **[EnableCors]** na třídy kontroleru se vztahuje na všechny akce v kontroleru. Chcete-li zakázat CORS pro akce, přidejte **[DisableCors]** atribut akce. Následující příklad umožňuje použití CORS pro každou metodu s výjimkou `PutItem`.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

**Globálně**

K povolení sdílení CORS pro všechny řadiče webového rozhraní API v aplikaci, předat **EnableCorsAttribute** instance k **EnableCors** metoda:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

Pokud atribut nastavíte na více než jednoho oboru, je pořadí podle priority:

1. Akce
2. Řadiče
3. Globální

<a id="allowed-origins"></a>
## <a name="set-the-allowed-origins"></a>Nastavit povolené zdroje

*Zdroje* parametr **[EnableCors]** atribut určuje, jaké zdroje jsou povoleny pro přístup k prostředku. Hodnota je čárkami oddělený seznam Povolené zdroje.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

Můžete také použít zástupný znak hodnotu "\*" Povolit požadavky od žádné zdroje.

Pečlivě zvažte, před povolením požadavky od jakýkoli původ. Znamená to, že názvy všech webu provádět volání AJAX do webového rozhraní API.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

<a id="allowed-methods"></a>
## <a name="set-the-allowed-http-methods"></a>Nastavit metody povolené HTTP

*Metody* parametr **[EnableCors]** atribut určuje, jaké metody HTTP jsou povoleny pro přístup k prostředku. Pokud chcete povolit všechny metody, použijte hodnotu zástupný znak "\*". Následující příklad umožňuje pouze požadavky GET a POST.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

<a id="allowed-request-headers"></a>
## <a name="set-the-allowed-request-headers"></a>Nastavit hlavičky povolených požadavků

Dříve I popisuje jak předběžný požadavek může obsahovat hlavičku Access-Control-Request-Headers výpis hlavičky protokolu HTTP, které jsou nastaveny aplikací (takzvaný "vytvářet hlavičky žádosti"). *Hlavičky* parametr **[EnableCors]** atribut určuje, které autor hlavičky žádosti jsou povoleny. Chcete-li povolit všechny hlavičky, nastavte *hlavičky* na "\*". Chcete-li konkrétní hlavičky seznamu povolených IP adres, nastavte *hlavičky* na seznam povolených hlaviček oddělených čárkami:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

Však nejsou prohlížeče zcela konzistentní v tom, jak nastavují Access-Control-Request-Headers. Například Chrome aktuálně obsahuje "Původ"; Při FireFox nezahrnuje hlavičky standardních, jako je například "Přijmout", i když je aplikace nastaví ve skriptu.

Pokud nastavíte *hlavičky* k ničemu jiný než "\*", by měla obsahovat alespoň "přijmout", "content-type" a "Původ" a jakékoli vlastní hlavičky, které chcete podporovat.

<a id="allowed-response-headers"></a>
## <a name="set-the-allowed-response-headers"></a>Nastavit hlavičky povolené odpovědi

Ve výchozím prohlížeči nevystavuje všechny hlavičky odpovědi do aplikace. Hlavičky odpovědi, které jsou k dispozici ve výchozím nastavení jsou:

- Cache-Control
- Obsah – jazyk
- Typ obsahu
- Vypršení platnosti
- Poslední úpravy
- Direktiva pragma

Specifikace CORS volá tyto [hlavičky odpovědi jednoduché](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header). Chcete-li zpřístupnit další hlavičky pro aplikace, nastavte *exposedHeaders* parametr **[EnableCors]**.

V následujícím příkladu, řadičem na `Get` metoda nastaví vlastní hlavičku s názvem 'X-vlastní hlavičku'. Ve výchozím nastavení nebude prohlížeč vystavit této hlavičky v požadavku nepůvodního zdroje. Pokud chcete zpřístupnit záhlaví, zahrnout 'X vlastní hlavičky, *exposedHeaders*.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

<a id="credentials"></a>
## <a name="passing-credentials-in-cross-origin-requests"></a>Předávání pověření v žádostí napříč zdroji

Přihlašovací údaje vyžadují zvláštní zpracování v požadavek CORS. Ve výchozím prohlížeči neodešle všechny přihlašovací údaje s žádostí napříč zdroji. Přihlašovací údaje zahrnují soubory cookie, jakož i schémat ověřování protokolu HTTP. K odeslání pověření s žádost nepůvodního zdroje, musíte nastavit klienta **XMLHttpRequest.withCredentials** na hodnotu true.

Pomocí **XMLHttpRequest** přímo:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

V jQuery:

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

Kromě toho serveru musí umožňovat přihlašovací údaje. Povolit cross-origin přihlašovací údaje v rozhraní Web API, nastavte **SupportsCredentials** vlastnost na hodnotu true na **[EnableCors]** atribut:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

Pokud je tato vlastnost hodnotu true, odpověď HTTP bude obsahovat hlavičku přístup – ovládací prvek-Allow-Credentials. Tuto hlavičku sděluje prohlížeči, že server umožňuje přihlašovací údaje pro žádost o nepůvodního zdroje.

Pokud prohlížeč odesílá přihlašovací údaje, ale odpověď neobsahuje platný přístup – ovládací prvek-Allow-Credentials záhlaví, prohlížeče nebude vystavit odpověď na žádost a vyvolá chybu požadavek AJAX.

Buďte velmi opatrní o nastavení **SupportsCredentials** na hodnotu true, protože znamená webu v jiné doméně může odesílat pověření přihlášeného uživatele na váš Web API jménem uživatele, aniž by uživatel znal. Specifikace CORS také stavy tohoto nastavení *zdroje* k &quot; \* &quot; je neplatný Pokud **SupportsCredentials** hodnotu true.

<a id="cors-policy-providers"></a>
## <a name="custom-cors-policy-providers"></a>Zprostředkovatelé zásady vlastní CORS

**[EnableCors]** atribut implementuje **ICorsPolicyProvider** rozhraní. Vlastní implementace můžete zadat třídu odvozenou od **atribut** a implementuje **ICorsProlicyProvider**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

Teď můžete použít atribut žádné umístit, že by měla být umístěna **[EnableCors]**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

Například vlastní zprostředkovatel zásad CORS by mohl číst nastavení z konfiguračního souboru.

Jako alternativu k použití atributy, můžete zaregistrovat **ICorsPolicyProviderFactory** objekt, který vytvoří **ICorsPolicyProvider** objekty.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

Chcete-li nastavit **ICorsPolicyProviderFactory**, volání **SetCorsPolicyProviderFactory** metoda rozšíření při spuštění, následujícím způsobem:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

<a id="browser-support"></a>
## <a name="browser-support"></a>Podpora prohlížeče

Balíček CORS webového rozhraní API je technologie straně serveru. Také musí podporovat CORS prohlížeče uživatele. Naštěstí aktuálních verzích všechny hlavní prohlížeče zahrnují [podpora CORS](http://caniuse.com/cors).

Internet Explorer 8 a Internet Explorer 9 je částečná podpora CORS, pomocí starší verze objektu XDomainRequest místo XMLHttpRequest. Další informace najdete v tématu [XDomainRequest - omezení, omezení a řešení](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).
