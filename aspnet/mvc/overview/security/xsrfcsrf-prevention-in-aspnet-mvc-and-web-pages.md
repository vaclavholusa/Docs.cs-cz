---
uid: mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
title: "Prevence XSRF/proti útokům CSRF v architektuře ASP.NET MVC a webových stránek | Microsoft Docs"
author: Rick-Anderson
description: "Padělání požadavku posílaného mezi weby (také označované jako XSRF nebo proti útokům CSRF) je útok na hostované webové aplikace, při kterém se škodlivý web mohou mít vliv Interakti..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2013
ms.topic: article
ms.assetid: aadc5fa4-8215-4fc7-afd5-bcd2ef879728
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
msc.type: authoredcontent
ms.openlocfilehash: 6cf30daa7ed966b11405cec715c5bc803b567249
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages"></a>Prevence XSRF/proti útokům CSRF v architektuře ASP.NET MVC a webových stránek
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> Padělání (také označované jako XSRF nebo proti útokům CSRF) je útok na hostované webové aplikace, které můžete vytvořit web ovlivnit interakce mezi klientského prohlížeče a webu důvěryhodný prohlížeč tohoto webů požadavek. Tyto útoky jsou možné, protože webových prohlížečů bude odesílat tokeny ověřování automaticky s každou žádost na web. V kanonickém tvaru příkladu je soubor cookie ověřování, jako je například ASP. Lístek ověřování pomocí formulářů pro Asp.net. Tyto útoky však může být cílem weby, které používají všechny trvalé ověřovací mechanismus (například ověřování systému Windows, Basic a tak dále).
> 
> Útok XSRF se liší od útoky phishing. Útoky phishing vyžadovat interakci z napadeného. V útoku phishing škodlivý web bude napodobovat cílového webu a napadené je oklamat do poskytování útočník citlivé informace. Při útoku XSRF není často žádná interakce potřebné z napadeného. Místo toho je útočník spoléhat na prohlížeči všechny relevantní soubory cookie automaticky odesílání do cílového webu.
> 
> Další informace najdete v tématu [otevřete projekt webové aplikace zabezpečení](https://www.owasp.org/index.php/Main_Page)(OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)).


## <a name="anatomy-of-an-attack"></a>Anatomie útoku

Chcete-li provede XSRF útok, zvažte uživatel, který chce provést některé online bankovnictví transakce. Tento uživatel navštíví nejprve WoodgroveBank.com a protokoly, za bod, který bude obsahovat hlavičku odpovědi svůj soubor cookie pro ověřování:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample1.cmd)]

Ověřovacího souboru cookie je soubor cookie relace, a proto jej bude automaticky vymazán prohlížeč při ukončení procesu prohlížeče. Do té doby, ale prohlížeč bude obsahovat automaticky souborů cookie u každé žádosti o WoodgroveBank.com. Uživatel se teď chce přenosu 1 000 USD na jiný účet, tak, aby Jana vyplňování formuláře na webu bankovnictví a prohlížeč provede tento požadavek na server:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample2.cmd)]

Protože tato operace má vedlejším účinkem (inicializuje peněžní transakci), se rozhodli webu bankovní vyžadují HTTP POST, aby bylo možné spustit tuto operaci. Server přečte ověřovací token ze žádosti, vyhledá číslo účtu aktuálního uživatele, ověří, že existují dostatečné prostředky a poté zahájí transakce do cílového účtu.

Jí online bankovnictví dokončení, uživatel přejde mimo lokalitu bankovnictví a navštíví jiných umístění na webu. Jednu z těchto lokalit – fabrikam.com – zahrnuje následující kód na stránce vložených v rámci &lt;iframe&gt;:

[!code-html[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample3.html)]

Které způsobí, že prohlížeč pro vytvoření této žádosti:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample4.cmd)]

Útočník zneužívá skutečnost, že uživatel může mít stále platný ověřovací token pro cílový webový server, a uživatel používá malé fragment kódu jazyka Javascript v prohlížeči, aby HTTP POST do cílové lokality automaticky. Pokud ověřovací token je stále platné, bude webu bankovní zahájení přenosu 250 do účet útočníkem.

### <a name="ineffective-mitigations"></a>Neúčinná způsoby zmírnění rizik

Je zajímavé Všimněte si, že uvedený scénář, bylo skutečnost, že WoodgroveBank.com se přistupuje přes protokol SSL a měl soubor cookie ověřování pouze SSL dostatek zabránit útokům. Je možné zadat útočník [schéma identifikátoru URI](http://en.wikipedia.org/wiki/URI_scheme) (https) v její &lt;formuláře&gt; elementu a prohlížeč bude nadále odesílat neprošlé soubory cookie k cílovému webu, tak dlouho, dokud tyto soubory cookie jsou konzistentní s identifikátorem URI schéma zamýšleného cílového.

Jeden může uvádějí, které uživatel by neměl jednoduše naleznete umístěné na serverech, jako návštěvou pouze důvěryhodných serverů je pomáhá zůstat online bezpečné. Je některé založená na to, ale bohužel tuto žádost není vždy reálné. Možná uživatele "důvěřuje" místní zprávy lokality ConsolidatedMessenger. ConsolidatedMessenger.com a vloží do návštěvu, která místo lokality, ale této lokality musí XSS ohrožení zabezpečení, která umožňuje útočníkovi vložení stejné fragment kódu, který byl spuštěn na fabrikam.com.

Můžete ověřit, že mají příchozí požadavky [odkazující server záhlaví](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) odkazující na doménu. To se zastaví požadavky bezděčně odeslané z domény třetí strany. Ale někteří uživatelé zakažte záhlaví odkazující server svého prohlížeče z důvodů ochrany osobních údajů a útočník může někdy zfalšovat této hlavičky, pokud napadené má určitá nezabezpečené nainstalovaný software. Ověření [odkazující server záhlaví](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) nepovažuje zabezpečený přístup k ochrana před útoky XSRF.

## <a name="web-stack-runtime-xsrf-mitigations"></a>Způsoby zmírnění rizik webové XSRF Runtime zásobníku

Modul Runtime ASP.NET Web zásobníku používá jeho variantě [tokenu vzor synchronizátor](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet#General_Recommendation:_Synchronizer_Token_Pattern) se chránit před útoky XSRF. Obecná forma vzoru synchronizátor tokenu je, že dva anti-XSRF tokeny jsou odeslána na server s každou HTTP POST (kromě ověřovací token): jeden token souboru cookie a druhý jako hodnotu formuláře. Hodnoty tokenu generované modulem runtime ASP.NET nejsou deterministickou nebo předvídatelný uživatelem se zlými úmysly. Tokeny jsou odeslány, serveru umožňuje v případě požadavku pokračujte pouze v případě, že oba tokeny předat kontrolu porovnání.

Požadavek ověřování XSRF *token relace* se ukládají jako souboru cookie HTTP a aktuálně obsahuje následující informace v jeho datové části:

- Token zabezpečení skládající se z náhodné identifikátor 128-bit.   
 Následující obrázek ukazuje XSRF žádosti o ověření relace token zobrazí pomocí nástrojů pro vývojáře Internet Explorer F12: (Poznámka: Toto je aktuální implementace a je předmět, i pravděpodobně, chcete-li změnit.)

![](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/_static/image1.png)

*Pole token* se ukládají jako `<input type="hidden" />` a obsahuje následující informace v jeho datové části:

- Uživatelské jméno přihlášeného uživatele (Pokud ověřený).
- Žádná další data poskytované [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx).

Datových částí tokeny anti-XSRF se šifrovaný a podepsaný, tak při použití nástroje pro zjištění tokeny nelze zobrazit uživatelské jméno. Pokud je webová aplikace je cílem technologii ASP.NET 4.0, šifrovací služby jsou poskytovány [MachineKey.Encode](https://msdn.microsoft.com/library/system.web.security.machinekey.encode.aspx) rutiny. Když webové aplikace je cílení na technologii ASP.NET 4.5 nebo vyšší, kryptografické služby jsou poskytovány [MachineKey.Protect](https://msdn.microsoft.com/library/system.web.security.machinekey.protect(v=vs.110)) rutiny, který nabízí lepší výkon, rozšíření a zabezpečení. Zjistit že následující příspěvky další podrobnosti:

- [Kryptografických vylepšení v technologii ASP.NET 4.5, pt. 1](https://blogs.msdn.com/b/webdev/archive/2012/10/22/cryptographic-improvements-in-asp-net-4-5-pt-1.aspx)
- [Kryptografických vylepšení v technologii ASP.NET 4.5, pt. 2](https://blogs.msdn.com/b/webdev/archive/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2.aspx)
- [Kryptografických vylepšení v technologii ASP.NET 4.5, pt. 3](https://blogs.msdn.com/b/webdev/archive/2012/10/24/cryptographic-improvements-in-asp-net-4-5-pt-3.aspx)

## <a name="generating-the-tokens"></a>Generování tokenů

Ke generování tokenů anti-XSRF, volání [ @Html.AntiForgeryToken ](https://msdn.microsoft.com/library/dd470175.aspx) metoda ze zobrazení MVC nebo @AntiForgery.GetHtml() ze stránky Razor. Modul runtime pak provede následující kroky:

1. Pokud aktuální žádost HTTP již obsahuje token anti-XSRF relace (soubor cookie anti-XSRF \_ \_RequestVerificationToken), se z něj extrahuje token zabezpečení. Pokud požadavek HTTP neobsahuje token anti-XSRF relace nebo pokud extrakce tokenu zabezpečení se nezdařila, vygeneruje se nový token anti-XSRF náhodné.
2. Token anti-XSRF pole je generována pomocí tokenu zabezpečení z kroku (1) a totožnosti aktuálně přihlášeného uživatele. (Další informace o určení identity uživatele, najdete v článku  **[scénáře s podporou speciální](#_Scenarios_with_special)**  části.) Kromě toho pokud [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/jj158328(v=vs.111).aspx) je nakonfigurována, bude volat modulu runtime jeho [GetAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.getadditionaldata(v=vs.111).aspx) metoda a zahrnout do pole token vrácený řetězec. (Viz  **[konfigurace a rozšíření](#_Configuration_and_extensibility)**  části Další informace.)
3. Pokud nový token anti-XSRF se vygeneroval v kroku (1), token novou relaci pro ji obsahují se vytvoří a bude přidán ke kolekci souborů cookie odchozí HTTP. Pole tokenu z kroku (2) bude uzavřen do `<input type="hidden" />` elementu a tento kód HTML se vrátí hodnotu, která `Html.AntiForgeryToken()` nebo `AntiForgery.GetHtml()`.

## <a name="validating-the-tokens"></a>Ověřování tokenů

K ověření příchozí tokeny anti-XSRF, zahrnuje Vývojář [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(VS.108).aspx) atribut na jeho akce MVC nebo řadiče nebo volání she `@AntiForgery.Validate()` z jeho stránky Razor. Modul runtime provede následující kroky:

1. Příchozí token relace a pole token se čtou a token anti-XSRF extrahovat z každé. Anti-XSRF tokeny musí být identické za krok (2) v rutině generace.
2. Pokud je aktuální uživatel ověřen, svoje uživatelské jméno se porovná s uživatelským jménem, uložené v tokenu pole. Uživatelských jménech se musí shodovat.
3. Pokud [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) je nakonfigurován, volání modulu runtime jeho *ValidateAdditionalData* metoda. Metoda musí vrátit logickou hodnotu *true*.

V případě úspěšného ověření požadavku je povoleno pokračovat. Pokud ověření selže, vyvolá výjimku rozhraní *HttpAntiForgeryException*.

## <a name="failure-conditions"></a>Podmínky při selhání

Od verze ASP.NET Web zásobníku Runtime v2, všechny *HttpAntiForgeryException* , je vyvolána při ověření bude obsahovat podrobné informace o chybě. Aktuálně definovaných selhání podmínky jsou:

- Token relace nebo tokenu formuláře se nenachází v požadavku.
- Token relace nebo tokenu formuláře nečitelná. Nejpravděpodobnější příčinou je farmou neodpovídající verzemi ASP.NET Web zásobníku Runtime nebo farmy kde &lt;machineKey&gt; element v souboru Web.config se liší mezi počítači. Můžete například nástroj Fiddler k manipulaci s buď token anti-XSRF vynutit výjimku.
- Token relace a pole token byly vzájemně zaměněny.
- Token relace a pole token obsahovat tokeny neodpovídající zabezpečení.
- Vložená do tokenu pole uživatelské jméno neodpovídá uživatelské jméno aktuálního přihlášeného uživatele.
- *[IAntiForgeryAdditionalDataProvider.ValidateAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.validateadditionaldata(v=vs.111).aspx)*  metoda vrátí *false*.

Anti-XSRF zařízení může také provést další kontrolu během ověření nebo generování tokenů a selhání během těchto kontrol může vést k výjimkám hlášeny. Najdete v článku [WIF / ACS / založené na deklaracích identity ověřování](#_WIF_ACS) a  **[konfigurace a rozšíření](#_Configuration_and_extensibility)**  částech Další informace.

<a id="_Scenarios_with_special"></a>

## <a name="scenarios-with-special-support"></a>Scénáře s speciální podpory

### <a name="anonymous-authentication"></a>Anonymní ověření

Systém anti-XSRF obsahuje speciální podporu pro anonymní uživatele, kde "anonymní" je definován jako uživatel, kde *IIdentity.IsAuthenticated* vlastnost vrátí *false*. Scénáře patří ochranu XSRF na přihlašovací stránku (dříve, než je uživatel ověřený) a schémat vlastní ověřování, kde aplikace používá mechanismus jiné než *identita* k identifikaci uživatelů.

Pro tyto scénáře podporovat odvolat, aby tokeny relace a pole jsou spojena tokeny zabezpečení, které je plné krytí identifikátor náhodně generované 128-bit. Tento token zabezpečení slouží ke sledování relace pro jednotlivé uživatele v Jana přejde lokality, takže ho efektivně slouží účel anonymní identifikátor. Prázdný řetězec se používá namísto uživatelské jméno pro generování a ověřování rutiny, které jsou popsané výše.

<a id="_WIF_ACS"></a>

### <a name="wif--acs--claims-based-authentication"></a>WIF / ACS / založené na deklaracích identity ověřování

Za normálních okolností *identita* založená na rozhraní .NET Framework třídy mají vlastnost který *IIdentity.Name* je dostačující k jednoznačné identifikaci určitého uživatele v rámci konkrétní aplikace. Například *FormsIdentity.Name* vrátí uživatelské jméno uložené v databázi členství (které je jedinečné pro všechny aplikace, v závislosti na tom, že databáze), *WindowsIdentity.Name* vrátí kvalifikovaný domény identity uživatele a tak dále. Tyto systémy poskytují nejenom ověřování; budou také *identifikovat* uživatelům aplikace.

Ověřování na základě deklarace identity, na druhé straně, nemusí nutně vyžadovat identifikace určitého uživatele. Místo toho *ClaimsPrincipal* a *ClaimsIdentity* typů jsou spojeny s sadu *deklarace identity* instance, kde jednotlivé deklaracích může být "je 18 + let" nebo " je správce"na jakoukoli jinou. Vzhledem k tomu, že uživatel nutně nezjistila, nemůžete použít modul runtime *ClaimsIdentity.Name* vlastnost jako jedinečný identifikátor pro tento konkrétní uživatel. Tým služby zaznamenal reálného příklady kde *ClaimsIdentity.Name* vrátí *null*, vrátí (zobrazení) popisný název, nebo v opačném případě vrátí řetězec, který není vhodné používat jako jedinečný identifikátor pro uživatele.

Mnoho nasazení, které používají ověřování založené na deklaracích pomocí [služby Řízení přístupu Azure](https://msdn.microsoft.com/library/windowsazure/gg429786.aspx) (ACS) na konkrétní. ACS umožňuje vývojáři ke konfiguraci jednotlivých *zprostředkovatelů identity* (například ADFS, poskytovatele Microsoft Account OpenID zprostředkovatelé třeba Yahoo! atd.), a vrátí zprostředkovatelů identity *název identifikátory*. Tyto identifikátory název může obsahovat identifikovatelné osobní informace (PII) jako e-mailovou adresu, nebo může být anonymní jako identifikátoru PPID (Private Personal). Bez ohledu na to, řazené kolekce členů (zprostředkovatele identity, identifikátor jména) dostatečně slouží jako token příslušné sledování pro určitého uživatele při Jana je procházení webu, tak modulem Runtime ASP.NET webových protokolů můžete použít řazenou kolekci členů místo uživatelské jméno při generování a ověřování tokenů anti-XSRF pole. Konkrétní identifikátory URI pro zprostředkovatele identity a identifikátor jména jsou:

- `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`
- `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`

(Toto [stránky dokumentace služby ACS](https://msdn.microsoft.com/library/windowsazure/gg185971.aspx) Další informace.)

Při generování nebo ověření tokenu, bude modulem Runtime ASP.NET Web zásobníku v době běhu zkuste vazby pro typy:

- `Microsoft.IdentityModel.Claims.IClaimsIdentity, Microsoft.IdentityModel, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35`(Pro sadu SDK WIF.)
- `System.Security.Claims.ClaimsIdentity`(Pro .NET 4.5).

Pokud tyto typy existují a pokud aktuální uživatel *IIIIdentity* implementuje nebo podtřídy jednu z těchto typů, anti-XSRF zařízení bude používat (zprostředkovatele identity, identifikátor jména) řazené kolekce členů namísto uživatelského jména, při generování a ověřování tokenů. Pokud je k dispozici žádný takový řazené kolekce členů, požadavek se nezdaří s chybou popisující pro vývojáře, jak nakonfigurovat systém anti-XSRF a zjistěte, konkrétní založené na deklaracích ověřovací mechanismus používá. Najdete v článku  **[konfigurace a rozšíření](#_Configuration_and_extensibility)**  části Další informace.

### <a name="oauth--openid-authentication"></a>OAuth / OpenID ověřování

Nakonec anti-XSRF zařízení má zvláštní podporu pro aplikace, které používají ověřování OAuth nebo OpenID. Tato podpora se na základě heuristiky: Pokud aktuální *IIdentity.Name* začíná http:// nebo https://, pak bude provedeno porovnání uživatelské jméno pomocí pořadí porovnávače spíše než porovnávače OrdinalIgnoreCase výchozí.

<a id="_Configuration_and_extensibility"></a>

## <a name="configuration-and-extensibility"></a>Konfigurace a rozšíření

Vývojáři v některých případech může být vhodné přesnější kontrolou nad anti-XSRF generování a ověřování chování. Například možná není žádoucí, MVC a webové stránky pomocné výchozí chování automaticky přidat do odpovědi soubory cookie protokolu HTTP a vývojáři mohou chtít zachovat tokeny jinde. Existují dvě rozhraní API, abyste toto:

`AntiForgery.GetTokens(string oldCookieToken, out string newCookieToken, out string formToken);`  
`AntiForgery.Validate(string cookieToken, string formToken);`

*GetTokens* metoda přijímá jako vstup existující XSRF žádosti o ověření relace tokenu (který může mít hodnotu null) a vytváří jako výstup pole token a nové relace tokenu XSRF žádosti o ověření. Tokeny jsou jednoduše neprůhledného řetězce s žádné decoration; *formToken* hodnota nebude pro instanci měl být uzavřen do &lt;vstupní&gt; značky. *NewCookieToken* , může mít hodnotu null; Pokud k tomu dojde, pak se *oldCookieToken* hodnota je stále platný a nový soubor cookie odpovědi musí být nastavena. Volající *GetTokens* zodpovídá za uchování všechny potřebné odpovědi soubory cookie nebo generování všechny potřebné značky *GetTokens* metoda sama nijak nezmění jako vedlejším účinkem odpověď. *Ověřením* metoda přijímá příchozí relace a pole tokeny a spouští logiku zmíněnými ověření nad nimi.

### <a name="antiforgeryconfig"></a>AntiForgeryConfig

Vývojář může konfigurovat systém anti-XSRF z aplikace\_spustit. Konfigurace je programový. Vlastnosti statických *AntiForgeryConfig* typu jsou popsané níže. Většina uživatelů použití deklarací identity budou chtít nastavit vlastnost UniqueClaimTypeIdentifier.

| **Vlastnost** | **Popis** |
| --- | --- |
| **AdditionalDataProvider** | [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) , poskytuje dodatečná data během generování tokenů a odebírá další data během ověření tokenu. Výchozí hodnota je *null*. Další informace najdete v tématu [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) části. |
| **CookieName** | Řetězec, který poskytuje název souboru cookie HTTP, který se používá k ukládání tokenu anti-XSRF relace. Pokud tato hodnota není nastavená, název budou automaticky generovány podle nasazené virtuální cestu aplikace. Výchozí hodnota je *null*. |
| **RequireSsl** | Logická hodnota, která určuje, jestli tokeny anti-XSRF se vyžadují k odeslání přes kanál zabezpečené protokolem SSL. Pokud je tato hodnota *true*, všechny soubory cookie automaticky generované bude mít nastaven příznak "zabezpečení" a rozhraní API anti-XSRF vyvolá výjimku, pokud je volána v rámci žádosti, které není odeslána prostřednictvím protokolu SSL. Výchozí hodnota je *false*. |
| **SuppressIdentityHeuristicChecks** | Logická hodnota, která určuje, zda by měl systém anti-XSRF deaktivovat podporuje založené na deklaracích identity. Pokud je tato hodnota *true*, systém bude předpokládat, že *IIdentity.Name* je vhodné pro použití jako identifikátor jedinečná uživatelská a nebude pokoušet zvláštní případ *IClaimsIdentity*nebo *ClClaimsIdentity* jak je popsáno v [WIF / ACS / založené na deklaracích identity ověřování](#_WIF_ACS) části. Výchozí hodnota je `false`. |
| **UniqueClaimTypeIdentifier** | Řetězec, který určuje, které deklarace identity typu je vhodná pro použití jako identifikátor jedinečný jednotlivé uživatele. Pokud tato hodnota je sada a aktuální *identita* je založené na deklaracích, systém se pokusí o extrahování deklarace identity typu určeného *UniqueClaimTypeIdentifier*, a budou použity s odpovídající hodnotou místo při generování token pole uživatelské jméno. Pokud není nalezen typ deklarace identity, systém nesplní žádost. Výchozí hodnota je *null*, což naznačuje, že systém má použít (zprostředkovatele identity, identifikátor jména) podle předchozích pokynů namísto uživatelského jména uživatele řazené kolekce členů. |

<a id="_IAntiForgeryAdditionalDataProvider"></a>

### <a name="iantiforgeryadditionaldataprovider"></a>IAntiForgeryAdditionalDataProvider

*[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)*  typ mohou vývojáři rozšířit chování systému anti-XSRF odezvy další data v každý token. *GetAdditionalData* metoda je volána pokaždé, když se vygeneruje token pole a vložené návratovou hodnotu v rámci vygenerovaný token. Implementátor může vrátit časové razítko, hodnotu nonce nebo jakoukoli jinou hodnotu, které Jana, které z této metody.

Podobně *ValidateAdditionalData* metoda je volána pokaždé, když se ověří token pole a je "Další data" řetězec, který byl vložen v rámci token předaný metodě. Rutiny ověřování může implementovat vypršení časového limitu (kontrolou aktuální čas na dobu, která byla uložená, když byl vytvořen token), hodnotu nonce zaškrtnutím rutiny nebo jakékoliv potřeby logiku.

## <a name="design-decisions-and-security-considerations"></a>Rozhodnutí o návrhu a aspekty zabezpečení

Token zabezpečení, který odkazuje tokeny relace a pole je technicky nutné pouze při pokusu o chránit anonymní / neověřené uživatele před útoky XSRF. Pokud je uživatel ověřený, ověřovací token samotné (pravděpodobně z důvodu odeslaná ve formátu souboru cookie) by se použil jako jednu polovinu synchronizátora token pár. Ale existují scénáře platné pro ochranu přihlašovací stránky zasažení neověřené uživatele a logiku anti-XSRF byl učiněn jednodušší vždy generování a ověřování tokenu zabezpečení, i pro ověřené uživatele. Také poskytuje některé další ochrany v případě, že token pole někdy dojde k narušení uživatelem se zlými úmysly, jako nastavení nebo uhodnutí token relace by být jiné mezní útočníkovi překonat.

Vývojáři měli při více aplikací, které jsou hostované v jedné doméně buďte opatrní. Například i když *example1.cloudapp.net* a *example2.cloudapp.net* jsou různých hostitelích, je vztah implicitní vztah důvěryhodnosti mezi všechny hostitele pod  *\*. cloudapp.net* domény. Tento vztah důvěryhodnosti implicitní [umožňuje potenciálně nedůvěryhodní hostitelé ovlivnit navzájem své soubory cookie](http://stackoverflow.com/questions/9636857/how-can-asp-net-or-asp-net-mvc-be-protected-from-related-domain-cookie-attacks) (stejného původu zásady, které řídí požadavky AJAX nemusí platit pro soubory cookie HTTP). Modul Runtime ASP.NET Web zásobníku poskytuje některé zmírnění v tom, že uživatelské jméno se vloží do pole tokenu, tak i v případě, že škodlivý subdoména je možné přepsat token relace nelze vygenerovat token pro daného uživatele platné pole. Ale pokud je hostováno v takovém prostředí rutiny předdefinované anti-XSRF stále nelze bránit proti zneužití relace nebo přihlášení XSRF.

Rutiny anti-XSRF aktuálně není bránit proti [útoků typu clickjacking](https://www.owasp.org/index.php/Clickjacking). Aplikace, které chcete chránit proti útoků typu clickjacking mohou snadno to provedete odesláním X-Frame-Options: Hlavička SAMEORIGIN s každá odpověď. Tuto hlavičku podporuje všechny poslední prohlížeče. Další informace najdete v tématu [IE blog](https://blogs.msdn.com/b/ieinternals/archive/2010/03/30/combating-clickjacking-with-x-frame-options.aspx), [SDL blog](https://blogs.msdn.com/b/sdl/archive/2009/02/05/clickjacking-defense-in-ie8.aspx), a [OWASP](https://www.owasp.org/index.php/Clickjacking). Modulem Runtime ASP.NET Web zásobníku může v některých zkontrolujte budoucí verze MVC a webových stránek anti-XSRF Pomocníci automaticky nastavit tuto hlavičku tak, aby aplikace automaticky chráněny před tento útok.

Vývojáři webů by měly být nadále Ujistěte se, že není ohrožena útoky XSS své lokality. Útoky XSS jsou velmi mocné a úspěšné zneužití by také zalomit Runtime ASP.NET Web zásobníku obrany před útoky XSRF.

## <a name="acknowledgment"></a>Potvrzení

[@LeviBroderick](https://twitter.com/LeviBroderick), který velkou část ASP.NET bezpečnostní kód napsali hromadným tyto informace.
