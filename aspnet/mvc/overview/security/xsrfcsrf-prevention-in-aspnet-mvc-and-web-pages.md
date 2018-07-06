---
uid: mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
title: Prevence XSRF/CSRF v ASP.NET MVC a na webových stránkách | Dokumentace Microsoftu
author: Rick-Anderson
description: Proti padělání požadavků mezi weby (označované také jako XSRF nebo CSRF) je útok na hostované webové aplikace, kterým se škodlivý web mohou mít vliv Interakti...
ms.author: aspnetcontent
ms.date: 03/14/2013
ms.assetid: aadc5fa4-8215-4fc7-afd5-bcd2ef879728
msc.legacyurl: /mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
msc.type: authoredcontent
ms.openlocfilehash: be0e8ebe521e9952d7525b581f9b91af6edca1da
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820253"
---
<a name="xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages"></a>Prevence XSRF/CSRF v ASP.NET MVC a na webových stránkách
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> Padělání (označované také jako XSRF nebo CSRF) je útok na hostované webové aplikace, které škodlivý web mohou mít vliv na interakci mezi prohlížeče klienta a webový server důvěřuje prohlížeče podvržení žádosti. Tyto útoky jsou možné, protože webových prohlížečů pošle ověřovacích tokenů automaticky při každé žádosti na webovou stránku. Canonical příkladu je soubor cookie ověřování, jako je například ASP. Lístek ověřování pomocí formulářů pro síť. Webové servery, které používají každý použitý mechanizmus trvalé ověřování (jako je například ověřování Windows, Basic a tak dále) ale mohou být cíleny těmto útokům.
> 
> Útok XSRF se liší od útoku phishing. Útoky typu phishing vyžadovat interakci napadeným. V rámci útoku phishing škodlivý web bude napodobovat cílové webové stránky a do poskytuje citlivé informace pro útočníka je oklamat napadeným. V XSRF útoku je často bez zásahu z napadeným. Útočník se místo toho spoléhá na prohlížeč automaticky odesílá všechny relevantní soubory cookie na cílový webový server.
> 
> Další informace najdete v tématu [otevřít projekt webové aplikace zabezpečení](https://www.owasp.org/index.php/Main_Page)(OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)).


## <a name="anatomy-of-an-attack"></a>Anatomie útoku

Chcete-li provede XSRF útoku, zvažte uživatel, který chce provést některé online bankovnictví transakce. Tento uživatel navštíví nejprve WoodgroveBank.com a protokolů, v tomto okamžiku bude obsahovat hlavičku odpovědi svůj soubor cookie pro ověřování:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample1.cmd)]

Protože ověřovacího souboru cookie je soubor cookie relace, se bude automaticky vymazán v prohlížeči při ukončení procesu prohlížeče. Do té doby, ale prohlížeče budou automaticky zahrnovat soubor cookie s každou žádostí do WoodgroveBank.com. Uživatel se nyní chce přenos 1 000 USD na jiný účet, takže Jana vyplňování formuláře na webu bankovnictví a prohlížeče je tento požadavek na server:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample2.cmd)]

Protože tato operace má vedlejší účinek (iniciuje peněžní transakce), rozhodl bankovnictví lokality vyžadovat, aby se zahájit operaci HTTP POST. Server načte ověřovací token z požadavku, vyhledá číslo účtu aktuálního uživatele, ověří, zda existuje dostatek prostředků a poté zahájí transakci do cílového účtu.

Toho naučila online bankovnictví kompletní, a uživatel přejde od lokality bankovnictví a počty návštěv jiných umístění na webu. Jeden z těchto webů – fabrikam.com – zahrnuje následující značky na stránce vložené v rámci &lt;iframe&gt;:

[!code-html[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample3.html)]

Jaké poté způsobí, že prohlížeč k této žádosti:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample4.cmd)]

Útočník využívá skutečnost, že uživatel může stále mít platný ověřovací token pro cílové webové stránky a používají malý fragment kódu jazyka Javascript a způsobit prohlížeč automaticky provede HTTP POST do cílové lokality. Pokud je stále platný ověřovací token, bankovnictví lokality zahájí přenos 250 USD na účet útočníkem.

### <a name="ineffective-mitigations"></a>Neúčinná způsoby zmírnění rizik

To je zajímavé uvědomit si, že ve výše popsaném scénáři, byla skutečnost, že WoodgroveBank.com se přistupuje přes SSL a měl soubor cookie ověřování jenom pro protokol SSL dostatečná k zamezit útoku. Útočník je zadat [schéma identifikátoru URI](http://en.wikipedia.org/wiki/URI_scheme) (https) v její &lt;formuláře&gt; elementu a prohlížeč bude nadále odesílat nevypršela soubory cookie k cílovému webu, pokud jsou tyto soubory cookie konzistentní s identifikátorem URI: schéma zamýšleným cílem.

Jeden může tvrdí, že uživatel by neměl navštivte nedůvěryhodné weby, jako navštívit jenom důvěryhodných serverů je pomáhá zůstat online bezpečné. Existuje několik informací k tomuto, ale bohužel toto doporučení není vždy reálné. Například uživatel "vztahy důvěryhodnosti" místní web ConsolidatedMessenger. ConsolidatedMessenger.com a vloží do najdete příslušné místo lokalitě, ale tento web má chybu XSS, což umožňuje útočníkovi vložení stejného fragmentu kódu, který byl spuštěn na fabrikam.com.

Můžete ověřit, že příchozí žádosti [hlavička odkazující strany](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) odkazují na vaši doménu. Tento kód přestane nechtěně požadavků z domény třetí strany. Ale někteří lidé zakázat hlavička odkazující strany jejich prohlížeče z důvodů ochrany osobních údajů a útočníci někdy může zfalšovat této hlavičky, pokud napadeným má určitý nezabezpečené software nainstalován. Ověření [hlavička odkazující strany](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) není považováno za zabezpečený přístup k prevence XSRF útoků.

## <a name="web-stack-runtime-xsrf-mitigations"></a>Web zásobník modulu Runtime XSRF způsoby zmírnění rizik

Modul Runtime ASP.NET Web Stack používá hodnotu variant [token vzor synchronizátor](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet#General_Recommendation:_Synchronizer_Token_Pattern) se chránit před útoky XSRF. Obecná forma synchronizátor token vzor je, že dva tokeny anti-XSRF odesílají na server s každou HTTP POST (kromě ověřovací token): jeden token souboru cookie a druhý jako hodnotu formuláře. Token hodnoty generované modulem ASP.NET runtime nejsou deterministické nebo předvídatelný ze strany útočníka. Odeslání tokeny serveru povolí požadavku pokračovat pouze v případě, že oba tokeny předat kontrolu porovnání.

Žádost o ověřování XSRF *tokenu relace* se ukládá jako souboru cookie protokolu HTTP a v současné době obsahuje následující informace v její datové části:

- Token zabezpečení skládající se z náhodných identifikátor 128 bitů.   
 Následující obrázek ukazuje token pro ověření relace XSRF žádost zobrazit pomocí vývojářských nástrojů Internet Explorer F12: (Poznámka: Toto je aktuální implementace a podléhá, dokonce i pravděpodobně, chcete-li změnit.)

![](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/_static/image1.png)

*Token pole* jako ukládána `<input type="hidden" />` a obsahuje následující informace v její datové části:

- Uživatelské jméno přihlášeného uživatele (je-li ověřit).
- Žádná další data poskytované [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx).

Datové části anti-XSRF tokenů jsou zašifrované a podepsaná, abyste při používání nástrojů pro zjištění tokeny nelze zobrazit uživatelské jméno. Pokud webová aplikace cílí na technologii ASP.NET 4.0, šifrovací služby jsou poskytovány [MachineKey.Encode](https://msdn.microsoft.com/library/system.web.security.machinekey.encode.aspx) rutiny. Když webové aplikace je cílen na verzi ASP.NET 4.5 nebo vyšší, kryptografických služeb jsou poskytovány [MachineKey.Protect](https://msdn.microsoft.com/library/system.web.security.machinekey.protect(v=vs.110)) rutina, která nabízí lepší výkon, rozšíření a zabezpečení. Zobrazit že následující blogové příspěvky, podrobné informace:

- [Kryptografických vylepšení v technologii ASP.NET 4.5, pt. 1](https://blogs.msdn.com/b/webdev/archive/2012/10/22/cryptographic-improvements-in-asp-net-4-5-pt-1.aspx)
- [Kryptografických vylepšení v technologii ASP.NET 4.5, pt. 2](https://blogs.msdn.com/b/webdev/archive/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2.aspx)
- [Kryptografických vylepšení v technologii ASP.NET 4.5, pt. 3](https://blogs.msdn.com/b/webdev/archive/2012/10/24/cryptographic-improvements-in-asp-net-4-5-pt-3.aspx)

## <a name="generating-the-tokens"></a>Generování tokenů

Chcete-li generovat tokeny anti-XSRF, zavolejte [ @Html.AntiForgeryToken ](https://msdn.microsoft.com/library/dd470175.aspx) metoda ze zobrazení MVC nebo @AntiForgery.GetHtml() ze stránky Razor. Modul runtime bude potom proveďte následující kroky:

1. Pokud aktuální požadavek HTTP již obsahuje token anti-XSRF relace (anti-XSRF souboru cookie \_ \_RequestVerificationToken), je z něj extrahovat token zabezpečení. Pokud požadavek HTTP neobsahuje token anti-XSRF relace nebo extrakce tokenu zabezpečení nezdaří, vygeneruje se nový token anti-XSRF náhodné.
2. Token anti-XSRF pole je generována pomocí tokenu zabezpečení z kroku (1) a identity aktuální přihlášeného uživatele. (Další informace o určení identity uživatele, najdete v článku **[scénáře s podporou speciální](#_Scenarios_with_special)** níže v části.) Kromě toho pokud [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/jj158328(v=vs.111).aspx) je nakonfigurován, modul runtime zavolá jeho [GetAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.getadditionaldata(v=vs.111).aspx) metoda a zahrnout token pole vráceného řetězce. (Viz **[konfigurace a rozšíření](#_Configuration_and_extensibility)** části Další informace.)
3. Pokud nový token anti-XSRF byl vytvořen v kroku (1), token novou relaci obsahujícího se vytvoří a přidá do výstupního kolekce souborů cookie protokolu HTTP. Token pole z kroku (2) se bude zabalena do `<input type="hidden" />` elementu, tento kód HTML to návratová hodnota `Html.AntiForgeryToken()` nebo `AntiForgery.GetHtml()`.

## <a name="validating-the-tokens"></a>Ověřování tokenů

Pro ověření příchozích tokenů anti-XSRF, vývojář zahrnuje [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(VS.108).aspx) atribut na své akce MVC nebo řadiči nebo volání she `@AntiForgery.Validate()` z její stránky Razor. Modul runtime provede následující kroky:

1. Příchozí token relace a token pole se čtou a token anti-XSRF extrahovat z každého. Anti-XSRF tokeny musí být identické za krok (2) v rutině generace.
2. Pokud je aktuální uživatel ověřen, své uživatelské jméno se porovná se ukládají v tokenu pole uživatelské jméno. Uživatelská jména se musí shodovat.
3. Pokud [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) je nakonfigurovaný, modul runtime zavolá jeho *ValidateAdditionalData* metody. Metoda musí vracet logickou hodnotu *true*.

Pokud ověření proběhne úspěšně, žádost může pokračovat. Pokud se ověření nezdaří, rozhraní vyvolá *HttpAntiForgeryException*.

## <a name="failure-conditions"></a>Podmínky při selhání

Od verze modulu Runtime zásobníku webového ASP.NET v2, všechny *HttpAntiForgeryException* , která je vyvolána během ověřování bude obsahovat podrobné informace o chybě. Jsou aktuálně definovaných selhání podmínky:

- Token relace nebo tokenu formuláře se nenachází v požadavku.
- Token relace nebo tokenu formuláře nečitelný. Nejpravděpodobnější příčinou je do farmy s Neshoda verzí modulu Runtime zásobníku Web ASP.NET Windows nebo farmy kde &lt;machineKey&gt; element v souboru Web.config se liší mezi počítači. Můžete například nástroj Fiddler k manipulaci s buď token anti-XSRF vynutit této výjimky.
- Byli vyměněni tokenu relace a token pole.
- Token relace a token pole obsahují tokeny neodpovídající zabezpečení.
- Vložená do tokenu pole uživatelské jméno neodpovídá uživatelské jméno aktuálního přihlášeného uživatele.
- *[IAntiForgeryAdditionalDataProvider.ValidateAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.validateadditionaldata(v=vs.111).aspx)* Metoda vrátila *false*.

Anti-XSRF zařízení mohou také provádět další kontrolu během ověření nebo generování tokenů a vyvolané výjimky může způsobit selhání během těchto kontrol. Naleznete v tématu [technologie WIF a služby ACS / založené na deklaracích ověřování](#_WIF_ACS) a **[konfigurace a rozšíření](#_Configuration_and_extensibility)** oddíly pro další informace.

<a id="_Scenarios_with_special"></a>

## <a name="scenarios-with-special-support"></a>Scénáře s podporou speciální

### <a name="anonymous-authentication"></a>Anonymní ověření

Systém anti-XSRF obsahuje speciální podporu pro anonymní uživatele, kde "anonymní" je definován jako uživatel, kde *IIdentity.IsAuthenticated* vrátí vlastnost *false*. Scénáře zahrnují poskytující ochranu XSRF přihlašovací stránky (před ověření uživatele) a vlastní ověřovací schémata tam, kde se aplikace používá mechanismus jiné než *IIdentity* k identifikaci uživatelů.

Pro podporu těchto scénářů, si možná Vzpomínáte, že relace a pole tokeny jsou spojena token zabezpečení, který je neprůhledný identifikátor náhodně generovaných 128 bitů. Tento token zabezpečení slouží ke sledování jednotlivých uživatelské relace, jak které přejde na webu, tak efektivně slouží účel anonymní identifikátor. Prázdný řetězec se používá namísto uživatelského jména pro generování a ověřování rutiny popsané výše.

<a id="_WIF_ACS"></a>

### <a name="wif--acs--claims-based-authentication"></a>Technologie WIF a služby ACS / založené na deklaracích ověřování

Za normálních okolností *IIdentity* třídy, které jsou součástí rozhraní .NET Framework mají vlastnost, která *IIdentity.Name* je dostačující k jedinečné identifikaci konkrétní uživatele v rámci konkrétní aplikace. Například *FormsIdentity.Name* vrátí uživatelské jméno uložené v databázi členství (které je jedinečné pro všechny aplikace v závislosti na této databázi), *WindowsIdentity.Name* vrátí kvalifikované domény identity uživatele a tak dále. Tyto systémy ověřování nejen; jsou také *identifikovat* uživatelů pro aplikaci.

Ověřování na základě deklarací identity, na druhé straně, nemusí nutně vyžadovat určení konkrétního uživatele. Místo toho *ClaimsPrincipal* a *ClaimsIdentity* typy jsou přidružené sady *deklarace identity* instance, kde jednotlivé deklarace může být "je 18 + let" nebo " je správce"cokoli. Protože uživatel nutně nezjistila, modul runtime nelze použít *ClaimsIdentity.Name* vlastnost jako jedinečný identifikátor pro tento konkrétní uživatel. Tým má vidět příklady z reálného světa kde *ClaimsIdentity.Name* vrátí *null*, vrátí názvu zařízení (Zobrazit) nebo v opačném případě vrátí řetězec, který není vhodné pro použití jako jedinečný identifikátor pro daného uživatele.

Počet nasazení, které používají ověřování nezaloženého na deklaracích pomocí [Azure Access Control Service](https://msdn.microsoft.com/library/windowsazure/gg429786.aspx) (ACS) zejména. ACS umožňuje vývojářům konfigurovat jednotlivé *zprostředkovatelé identity* (například ADFS, Account Microsoft poskytovatele OpenID, jako je Yahoo!, atd.), a vraťte se zprostředkovatelé identity *název identifikátory*. Tyto identifikátory název může obsahovat identifikovatelné osobní údaje (PII) jako e-mailovou adresu, nebo může být anonymizované jako privátní osobní identifikátor (identifikátoru PPID). Bez ohledu na to, řazené kolekce členů (zprostředkovatele identity, identifikátor názvu) dostatečně slouží jako token příslušné sledování pro konkrétní uživatele a uživatel je procházení webu, takže modul Runtime ASP.NET Web Stack můžete použít řazené kolekce členů namísto uživatelského jména, při generování a ověřování tokenů anti-XSRF pole. Konkrétní identifikátory URI pro zprostředkovatele identity a název identifikátoru jsou:

- `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`
- `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`

(Toto [stránky dokumentu ACS](https://msdn.microsoft.com/library/windowsazure/gg185971.aspx) pro další informace.)

Při generování nebo ověření tokenu, modul Runtime ASP.NET Web zásobníku za běhu pokusí vazby na typy:

- `Microsoft.IdentityModel.Claims.IClaimsIdentity, Microsoft.IdentityModel, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35` (Pro sady SDK technologie WIF.)
- `System.Security.Claims.ClaimsIdentity` (Pro .NET 4.5).

Pokud existují tyto typy a aktuální uživatel *IIIIdentity* implementuje nebo podtřídy jednu z těchto typů, anti-XSRF zařízení bude používat (zprostředkovatele identity, identifikátor názvu) řazené kolekce členů namísto uživatelského jména, při generování a ověřování tokenů. Pokud neexistuje žádný takový řazené kolekce členů, požadavek selže s chybou vývojářům popisující, jak nakonfigurovat systém anti-XSRF a zjistěte, používá mechanismus konkrétní ověřování nezaloženého na deklaracích. Zobrazit **[konfigurace a rozšíření](#_Configuration_and_extensibility)** části Další informace.

### <a name="oauth--openid-authentication"></a>OAuth / OpenID ověřování

Nakonec anti-XSRF zařízení má zvláštní podporu pro aplikace, které používají ověřování OAuth nebo OpenID. Tato podpora je založená na heuristiky: Pokud se aktuální *IIdentity.Name* začíná http:// nebo https://, pak se provede porovnání uživatelského jména používá pořadové porovnávání spíše než výchozí porovnávací metody OrdinalIgnoreCase.

<a id="_Configuration_and_extensibility"></a>

## <a name="configuration-and-extensibility"></a>Konfigurace a rozšíření

Vývojáři v některých případech může být vhodné přesnější kontrolu nad generování anti-XSRF a chování ověřování. Například možná není žádoucí, MVC a na webových stránkách Pomocníci výchozí chování automaticky přidat do odpovědi soubory cookie protokolu HTTP a vývojáře chtít zachovat tokeny jinde. Existují dvě rozhraní API, která vám pomůže s tím:

`AntiForgery.GetTokens(string oldCookieToken, out string newCookieToken, out string formToken);`  
`AntiForgery.Validate(string cookieToken, string formToken);`

*GetTokens* metoda přijímá jako vstup existující XSRF žádost o ověření tokenu relace (která může mít hodnotu null) a vytvoří jako výstup nový token relace ověřování XSRF požadavku a token pole. Tokeny jsou jednoduše neprůhledné řetězce s žádná dekorace; *formToken* hodnota nesmí být pro instanci zabalené v &lt;vstupní&gt; značky. *NewCookieToken* hodnota může být null; Pokud k tomu dojde, pak bude *oldCookieToken* hodnota je stále platné a nový soubor cookie odpovědi musí být nastavena. Volající *GetTokens* zodpovídá za všechny potřebné odpovědi soubory cookie uložením nebo generování všechny potřebné značky *GetTokens* metoda sama odpověď jako vedlejší účinek nijak nezmění. *Ověřit* metoda přijímá příchozí relace a pole tokeny a spustí logiku ověřování výše uvedené nad nimi.

### <a name="antiforgeryconfig"></a>AntiForgeryConfig

Vývojáři mohou konfigurovat systém anti-XSRF z aplikace\_Start. Konfigurace je prostřednictvím kódu programu. Vlastnosti statické *AntiForgeryConfig* typu jsou popsané níže. Většinu uživatelů použití deklarací identity bude chtít nastavit vlastnost UniqueClaimTypeIdentifier.

| **Vlastnost** | **Popis** |
| --- | --- |
| **AdditionalDataProvider** | [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) , který poskytuje dodatečná data během generování tokenů a používá další data během ověřování tokenů. Výchozí hodnota je *null*. Další informace najdete v tématu [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) oddílu. |
| **CookieName** | Řetězec, který obsahuje název souboru cookie HTTP, který se používá k ukládání tokenu anti-XSRF relace. Pokud tato hodnota není nastavená, název budou automaticky generovány na základě aplikace nasazené virtuální cesty. Výchozí hodnota je *null*. |
| **Vlastnost RequireSsl** | Logická hodnota, která určuje, zda anti-XSRF tokeny jsou vyžadovány k odeslání přes kanál zabezpečené protokolem SSL. Pokud je tato hodnota *true*, všechny soubory cookie automaticky generovaných bude mít nastaven příznak "zabezpečení" a anti-XSRF rozhraní API vyvolá výjimku, pokud volat v rámci žádosti, která se neodešlou přes SSL. Výchozí hodnota je *false*. |
| **SuppressIdentityHeuristicChecks** | Logická hodnota, která určuje, zda by měl systém anti-XSRF deaktivovat podporu založené na deklaracích identity. Pokud je tato hodnota *true*, systém předpokládá, že *IIdentity.Name* je vhodné pro použití jako jedinečný uživatelský identifikátor a zvláštní případy se nebude pokoušet *IClaimsIdentity*nebo *ClClaimsIdentity* jak je popsáno v [technologie WIF a služby ACS / založené na deklaracích ověřování](#_WIF_ACS) oddílu. Výchozí hodnota je `false`. |
| **UniqueClaimTypeIdentifier** | Řetězec, který určuje, jaké deklarace identity zadejte je vhodné pro použití jako jedinečný uživatelský identifikátor. Pokud je tato hodnota sady a aktuální *IIdentity* je založené na deklaracích, systém se pokusí extrahovat typu deklarace identity určená *UniqueClaimTypeIdentifier*, a odpovídající hodnota, která se použije. místo při generování tokenu pole uživatelské jméno. Pokud není nalezen typ deklarace identity, systém nesplní žádost. Výchozí hodnota je *null*, což znamená, že systém má použít (zprostředkovatele identity, identifikátor názvu) řazené kolekce členů, jak je uvedeno výše namísto uživatelského jména uživatele. |

<a id="_IAntiForgeryAdditionalDataProvider"></a>

### <a name="iantiforgeryadditionaldataprovider"></a>IAntiForgeryAdditionalDataProvider

*[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)* typ umožňuje vývojářům rozšířit chování systému anti-XSRF verzemi další data v jednotlivých tokenu. *GetAdditionalData* metoda je volána pokaždé, když se vygeneruje token pole, a návratová hodnota je vložený v rámci vygenerovaný token. Implementátora můžou se zobrazovat časové razítko, hodnotu nonce nebo libovolné jiné hodnoty, které si uživatel přeje z této metody.

Podobně *ValidateAdditionalData* metoda je volána pokaždé, když se ověří token pole a řetězec "Další data", který byl vložen v tokenu je předán metodě. Rutiny ověřování může implementovat vypršení časového limitu (kontrolou aktuální čas čase, která byla uložená, při vytváření tokenu), hodnotu nonce kontrola rutiny nebo jiné požadovaného logiku.

## <a name="design-decisions-and-security-considerations"></a>Rozhodnutí o návrhu a důležité informace o zabezpečení

Token zabezpečení, který odkazuje relaci a pole tokeny je technicky pouze nezbytné při pokusu o ochranu anonymní / neověřené uživatele před útoky XSRF. Při ověření uživatele ověřovací token, samotný (pravděpodobně odeslání ve formě souboru cookie) by se použil jako jedna polovina synchronizátora token pár. Ale existují platné scénáře ochrany přihlašovací stránky přístupů od neověřených uživatelů a logiku anti-XSRF provedenými jednodušší vždy generování a ověřování tokenu zabezpečení, dokonce i pro ověřeného uživatele. Také poskytuje některé další ochranu v případě, že token pole je někdy dojde k ohrožení bezpečnosti útočník jako nastavení nebo opakovaně uhodnout, že token relace bude jiný mezní útočníkovi překonat.

Vývojáři by měl buďte opatrní při více aplikací, které jsou hostované v jedné doméně. Například i když *example1.cloudapp.net* a *example2.cloudapp.net* jsou různých hostitelích, je vztah implicitní vztah důvěryhodnosti mezi všemi hostiteli v rámci  *\*. cloudapp.net* domény. Tento vztah důvěryhodnosti implicitní [umožňuje potenciálně nedůvěryhodní hostitelé mít vliv na soubory cookie uživatele toho druhého](http://stackoverflow.com/questions/9636857/how-can-asp-net-or-asp-net-mvc-be-protected-from-related-domain-cookie-attacks) (stejného zdroje zásad, kterými se řídí odesílání požadavků AJAX nemusí platit pro soubory cookie protokolu HTTP). Modul Runtime ASP.NET Web zásobník obsahuje některá omezení rizik v tom, že uživatelské jméno se vloží do tokenu pole, tak i v případě, že škodlivý subdoménu je možné přepsat tokenu relace se nebudete moct vygenerovat token pro daného uživatele platné pole. Ale když jsou hostované v takovém prostředí rutiny integrované anti-XSRF stále nelze braňte se proti zneužití relace nebo přihlášení XSRF.

Rutiny anti-XSRF aktuálně není obrana proti nim [útoků typu clickjacking](https://www.owasp.org/index.php/Clickjacking). Aplikace, které chcete chránit proti útoků typu clickjacking může snadno to provedete odesláním X-Frame-Options: SAMEORIGIN záhlaví s každou odpověď. Tato hlavička podporuje všechny poslední prohlížeče. Další informace najdete v tématu [blogu IE](https://blogs.msdn.com/b/ieinternals/archive/2010/03/30/combating-clickjacking-with-x-frame-options.aspx), [SDL blogu](https://blogs.msdn.com/b/sdl/archive/2009/02/05/clickjacking-defense-in-ie8.aspx), a [OWASP](https://www.owasp.org/index.php/Clickjacking). Modul Runtime ASP.NET Web zásobník může v některé budoucí verzi. Zkontrolujte MVC a pomocné rutiny anti-XSRF webové stránky automaticky nastavit toto záhlaví tak, aby aplikace jsou automaticky chráněné proti útoku.

Webovým vývojářům nadále Ujistěte se, že není ohrožená útoky XSS jejich lokality. Útoky XSS jsou velmi výkonné a úspěšné před zneužitím také by narušil modul Runtime ASP.NET Web Stack obranu před útoky XSRF.

## <a name="acknowledgment"></a>Potvrzení

[@LeviBroderick](https://twitter.com/LeviBroderick), který napsal velkou část kódu ASP.NET zabezpečení hromadné tyto informace.
