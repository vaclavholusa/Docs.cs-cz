---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
title: Principy ověřování pomocí technologie ASP.NET AJAX a profil aplikačních služeb | Dokumentace Microsoftu
author: scottcate
description: Ověřovací služby umožňuje uživatelům zadat přihlašovací údaje, aby se zobrazí soubor cookie ověřování a se službou brány, aby umožňovala vlastní uživatelské...
ms.author: riande
ms.date: 03/14/2008
ms.assetid: 6ab4efb6-aab6-45ac-ad2c-bdec5848ef9e
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
msc.type: authoredcontent
ms.openlocfilehash: d722130e625a9f867923280fce0ef35f19bfeb9d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753124"
---
<a name="understanding-aspnet-ajax-authentication-and-profile-application-services"></a>Principy ověřování pomocí technologie ASP.NET AJAX a služby profilu aplikace
====================
podle [– Scott Cate](https://github.com/scottcate)

[Stáhnout PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial03_MSAjax_ASP.NET_Services_cs.pdf)

> Služba ověřování umožňuje uživatelům zadat přihlašovací údaje, aby se zobrazí soubor cookie ověřování a se službou brány, aby umožňovala vlastní uživatelské profily poskytuje pomocí technologie ASP.NET. Používání služby ověřování ASP.NET AJAX je kompatibilní s standardní ověřování pomocí formulářů ASP.NET, takže aplikace právě používá ověřování pomocí formulářů (například pomocí přihlášení na ovládací prvek) nebude nebudou fungovat, upgradujte k ověřovací službě AJAX.


## <a name="introduction"></a>Úvod

Jako součást rozhraní .NET Framework 3.5 poskytuje Microsoft s proměnlivou velikostí prostředí upgradem; Nejenže je k dispozici nové vývojové prostředí, ale nové funkce Language-Integrated Query (LINQ) a další vylepšení jazyka zobrazování poruch se. Kromě toho některé známé funkce další sady nástrojů, zejména rozšíření ASP.NET AJAX, se nebudou zahrnuty jako první třídy členy knihovny tříd rozhraní .NET Framework Base. Tato rozšíření povolit spoustu nových funkcí vzhled plně funkčního klienta, včetně částečného zobrazení stránek, bez nutnosti úplná aktualizace stránky, možnost přístupu k webovým službám prostřednictvím klientského skriptu (včetně ASP.NET, rozhraní API pro profilaci) a rozsáhlé rozhraní API na straně klienta navržené pro zrcadlení mnoho systémů ovládací prvek v sadě serverový ovládací prvek technologie ASP.NET.

Tento dokument White Paper zkoumá implementaci a použití profilace ASP.NET a ověřování založené na formulářích služby jako jsou vystavené AJAX rozšíření společnosti Microsoft ASP.NET AJAX ExtensionsThe usnadňují ověřování pomocí formulářů neuvěřitelně podpory, jako je (stejně jako Profilace služeb) je přístupný prostřednictvím skriptu proxy serveru webové služby. Rozšíření AJAX také podporují vlastní ověřování prostřednictvím AuthenticationServiceManager třídy.

Tento dokument White Paper vychází z verze beta verzi 2 sady Visual Studio 2008 a rozhraní .NET Framework 3.5. Tento dokument White Paper také předpokládá, že můžete pracovat s Visual Studio 2008 Beta 2, nikoli Visual Web Developer Express a poskytne návody podle uživatelského rozhraní sady Visual Studio. Šablony projektů, které jsou k dispozici v aplikaci Visual Web Developer Express může využívat několik ukázek kódu.

## <a name="profiles-and-authentication"></a>*Profily a ověřování*

Profily společnosti Microsoft ASP.NET a ověřovacích služeb jsou k dispozici v systému ověřování formulářů ASP.NET a jsou standardní součástí technologie ASP.NET. Rozšíření ASP.NET AJAX poskytují přístup skript do těchto služeb prostřednictvím skriptu proxy, přes jednoduchá model v rámci oboru názvů Sys.Services klientské knihovny AJAX.

Služba ověřování umožňuje uživatelům zadat přihlašovací údaje, aby se zobrazí soubor cookie ověřování a se službou brány, aby umožňovala vlastní uživatelské profily poskytuje pomocí technologie ASP.NET. Používání služby ověřování ASP.NET AJAX je kompatibilní s standardní ověřování pomocí formulářů ASP.NET, takže aplikace právě používá ověřování pomocí formulářů (například pomocí přihlášení na ovládací prvek) nebude nebudou fungovat, upgradujte k ověřovací službě AJAX.

Služba profilu umožňuje automatické integrace a ukládání dat uživatelů na základě členství jako poskytovaný službou ověřování. Uložená data je uvedené v souboru web.config a různých profilování poskytovatelů služeb postupovat při správě dat. Stejně jako u ověřovací službu, je kompatibilní se standardní profilovou službou ASP.NET AJAX profilovou službou, aby stránky aktuálně využívá funkce profilu ASP.NET služby by neměl nebudou fungovat, díky podpoře jazyka AJAX.

Začleňte do aplikace ověřování pomocí technologie ASP.NET a vlastních služeb profilace je mimo rozsah tohoto dokumentu. Další informace o tématu naleznete v knihovně MSDN odkazovat článku Správa uživatelů pomocí členství v [ https://msdn.microsoft.com/library/tw292whz.aspx ](https://msdn.microsoft.com/library/tw292whz.aspx). Technologie ASP.NET obsahuje také nástroj, který automaticky nastavit členství s SQL serverem, který je výchozím zprostředkovatelem služby ověřování pro členství technologie ASP.NET. Další informace najdete v článku nástroj pro registraci serveru SQL technologie ASP.NET (Aspnet\_regsql.exe) na [ https://msdn.microsoft.com/library/ms229862(vs.80).aspx ](https://msdn.microsoft.com/library/ms229862(vs.80).aspx).

## <a name="using-the-aspnet-ajax-authentication-service"></a>*Pomocí služby ověřování ASP.NET AJAX*

ASP.NET AJAX ověřovací služby musí být povoleno v souboru web.config:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample1.xml)]

Ověřovací službu vyžaduje ověřování pomocí formulářů ASP.NET aby byla povolena a vyžaduje soubory cookie, aby byla povolená na klientském prohlížeči (skriptu nelze povolit bez souborů cookie relace od relací bez souborů cookie vyžadují parametrů adresy URL).

Jakmile služba AJAX ověřování je povolena a konfigurována, klientského skriptu můžete okamžitě využít Sys.Services.AuthenticationService objektu. Především klientského skriptu chtít využívat výhod `login` metoda a `isLoggedIn` vlastnost. Existuje několik vlastností k poskytnutí výchozí hodnoty pro metodu přihlášení, která může přijmout velký počet parametrů.

*Sys.Services.AuthenticationService členy*

*způsob přihlášení:*

Metoda login() zahájí požadavek na ověření přihlašovacích údajů uživatele. Tato metoda je asynchronní a nedochází k blokování spuštění.

*Parametry:*

| **Název parametru** | **Význam** |
| --- | --- |
| uživatelské jméno | Požadováno. Uživatelské jméno k ověření. |
| Heslo | Volitelný (výchozí hodnota je null). Heslo uživatele. |
| isPersistent | Nepovinné (výchozí hodnota je false). Soubor cookie pro ověřování uživatele určuje, zda byste neměli zachovat napříč relacemi. Pokud má hodnotu false, bude se uživatel odhlásit při zavření prohlížeče nebo vypršení platnosti relace. |
| redirectUrl | Volitelný (výchozí hodnota je null). Adresa URL pro přesměrování prohlížeče po úspěšném ověření. Pokud tento parametr hodnotu null nebo prázdný řetězec, dojde k žádné přesměrování. |
| customInfo | Volitelný (výchozí hodnota je null). Tento parametr se aktuálně nepoužívá a je vyhrazen pro budoucí použití. |
| loginCompletedCallback | Volitelný (výchozí hodnota je null). Funkce, která má být volána při přihlášení bylo úspěšně dokončeno. -Li zadána, tento parametr přepisuje vlastnost defaultLoginCompleted. |
| failedCallback | Volitelný (výchozí hodnota je null). Funkce, která má být volána při přihlášení se nezdařilo. Je-li zadána, přepíše tento parametr vlastnosti defaultFailedCallback. |
| userContext | Volitelný (výchozí hodnota je null). Vlastní uživatelská data kontextu, který by měly být předány funkcím zpětného volání. |

*Návratová hodnota:*

Tato funkce neobsahuje návratovou hodnotu. Počet chování jsou však zahrnuty po dokončení volání této funkce:

- Aktuální stránka bude buď aktualizovat nebo změnit, pokud `redirectUrl` parametr měl hodnotu null ani prázdný řetězec.
- Nicméně, pokud parametr byl null nebo prázdný řetězec, `loginCompletedCallback` parametr, nebo `defaultLoginCompletedCallback` názvem vlastnosti.
- Pokud selže volání webové služby, `failedCallback` parametr `defaultFailedCallback` názvem vlastnosti.

*Metoda odhlášení:*

Metoda logout() odstraní soubor cookie přihlašovací údaje a odhlásí aktuálního uživatele z webové aplikace.

*Parametry:*

| **Název parametru** | **Význam** |
| --- | --- |
| redirectUrl | Volitelný (výchozí hodnota je null). Adresa URL pro přesměrování prohlížeče po úspěšném ověření. Pokud tento parametr hodnotu null nebo prázdný řetězec, dojde k žádné přesměrování. |
| logoutCompletedCallback | Volitelný (výchozí hodnota je null). Funkce, která má být volána při odhlášení bylo úspěšně dokončeno. -Li zadána, tento parametr přepisuje vlastnost defaultLogoutCompleted. |
| failedCallback | Volitelný (výchozí hodnota je null). Funkce, která má být volána při přihlášení se nezdařilo. Je-li zadána, přepíše tento parametr vlastnosti defaultFailedCallback. |
| userContext | Volitelný (výchozí hodnota je null). Vlastní uživatelská data kontextu, který by měly být předány funkcím zpětného volání. |

*Návratová hodnota:*

Tato funkce neobsahuje návratovou hodnotu. Počet chování jsou však zahrnuty po dokončení volání této funkce:

- Aktuální stránka bude buď aktualizovat nebo změnit, pokud `redirectUrl` parametr měl hodnotu null ani prázdný řetězec.
- Nicméně, pokud parametr byl null nebo prázdný řetězec, `logoutCompletedCallback` parametr, nebo `defaultLogoutCompletedCallback` názvem vlastnosti.
- Pokud selže volání webové služby, `failedCallback` parametr `defaultFailedCallback` názvem vlastnosti.

*Vlastnosti defaultFailedCallback (get, set):*

Tato vlastnost určuje funkci, která by měla být volána, pokud dojde k selhání ke komunikaci s webovou službou. Obdrželi delegáta (nebo odkazů na funkce).

Referenční dokumentace funkcí této vlastnosti by měl mít následující podpis:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample2.js)]

*Parametry:*

| **Název parametru** | **Význam** |
| --- | --- |
| Chyba | Určuje informace o této chybě. |
| userContext | Určuje informace o kontextu uživatele, pokud byla volána funkce přihlášení nebo odhlášení. |
| methodName | Název volání metody. |

*Vlastnost defaultLoginCompletedCallback (get, set):*

Tato vlastnost určuje funkci, která by měla být volána po dokončení přihlášení volání webové služby. Obdrželi delegáta (nebo odkazů na funkce).

Referenční dokumentace funkcí této vlastnosti by měl mít následující podpis:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample3.js)]

*Parametry:*

| **Název parametru** | **Význam** |
| --- | --- |
| validCredentials | Určuje, zda uživatel zadal pověření. `true` Pokud uživatel úspěšně přihlášen; v opačném případě `false`. |
| userContext | Určuje informace o kontextu uživatele, pokud byla volána funkce login. |
| methodName | Název volání metody. |

*vlastnost defaultLogoutCompletedCallback (get, set):*

Tato vlastnost určuje funkci, která by měla být volána po dokončení odhlášení volání webové služby. Obdrželi delegáta (nebo odkazů na funkce).

Referenční dokumentace funkcí této vlastnosti by měl mít následující podpis:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample4.js)]

*Parametry:*

| **Název parametru** | **Význam** |
| --- | --- |
| výsledek | Tento parametr bude vždy `null`; je vyhrazená pro budoucí použití. |
| userContext | Určuje informace o kontextu uživatele, pokud byla volána funkce login. |
| methodName | Název volání metody. |

*Vlastnost isLoggedIn (get):*

Tato vlastnost získá aktuální stav ověření daného uživatele. během požadavku na stránku nastavení v objektu ScriptManager.

Tato vlastnost vrátí `true` Pokud uživatel je aktuálně přihlášený v; jinak vrátí hodnotu, vrátí `false`.

*Vlastnost Path (get, set):*

Tato vlastnost určuje programově umístění ověřování webové služby. Je možné přepsat výchozí zprostředkovatel ověřování, jakož i jeden nastavit deklarací v cestě vlastnosti ovládacího prvku ScriptManager AuthenticationService podřízený uzel (Další informace najdete v vlastního zprostředkovatele ověřování služby téma níže).

Všimněte si, že umístění výchozí ověřovací službě nezmění. ASP.NET AJAX však umožňuje určit umístění webovou službu, která poskytuje rozhraní třídy jako proxy služby ověřování ASP.NET AJAX.

Všimněte si také, že by neměla tato vlastnost nastavená na hodnotu, která přesměruje požadavek skriptu z aktuální lokality. Vzhledem k tomu, že aktuální aplikace nebude přijímat přihlašovací údaje pro ověřování, je zbytečné. také AJAX podkladových technologie neúčtuje žádosti více webů a může vygenerovat výjimku zabezpečení do prohlížeče klienta.

Tato vlastnost je `String` objekt představující cestu k webové službě ověřování.

*Vlastnost časového limitu (get, set):*

Tato vlastnost určuje dobu čekání pro ověřovací službu před za předpokladu, že žádost o přihlášení se nezdařilo. Pokud vyprší časový limit při čekání na dokončení volání, zavolá se zpětné volání požadavek se nezdařil a volání se nikdy nedokončí.

Tato vlastnost je `Number` objekt představující počet milisekund čekání na výsledky z ověřovací služby.

*Ukázka kódu: Protokolování ve službě ověřování*

Následující kód je příklad stránky technologie ASP.NET pomocí volání metody přihlášení a odhlášení třídy AuthenticationService jednoduchý skript.

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample5.aspx)]

## <a name="accessing-aspnet-profiling-data-via-ajax"></a>Přístup k datům prostřednictvím jazyka AJAX profilování technologie ASP.NET

Profilování služby technologie ASP.NET je přístupný také prostřednictvím rozšíření ASP.NET AJAX. Od služby profilace ASP.NET poskytuje propracované a detailní API, kterým se mají ukládat a načítat data uživatele, může se jednat o nástroj vynikající produktivity.

Služba profilu musí být povoleno v souboru web.config; není ve výchozím nastavení. Uděláte to tak, aby `profileService` podřízený element má povolenou = true zadané v souboru web.config a zda jste zadali vlastnosti, které může číst nebo zapsat takto:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample6.xml)]

Služba profilu musí být taky nakonfigurovaný. Přestože konfigurace služby profilace je mimo rozsah tento dokument White Paper, je vhodné si uvědomit, že skupiny definované v nastavení profilu konfigurace budou dostupné jako dílčí vlastnosti názvu skupiny. Například v následující části profilu zadali:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample7.xml)]

Klientský skript bude mít přístup k názvu, Address.Line1, Address.Line2, Address.City, Address.State, Address.Zip a BackgroundColor jako vlastnosti pole vlastnosti třídy ProfileService.

Po nakonfigurování služby profilace AJAX bude okamžitě k dispozici na stránkách; načíst jednou před použitím však bude mít.

*Sys.Services.ProfileService členy*

*Vlastnosti pole:*

Pole vlastností uvádí všechna data nakonfigurovaný profil jako podřízené vlastnosti, které lze odkazovat pomocí názvu operátoru tečka konvence. Vlastnosti, které jsou podřízené skupiny vlastností se označují jako GroupName.PropertyName. V profilu příklad konfigurace uvedené výše Chcete-li získat informace o stavu uživatele, můžete použít následující identifikátor:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample8.cs)]

*Metoda load:*

Načte všechny vlastnosti nebo vybraný seznam ze serveru.

*Parametry:*

| **Název parametru** | **Význam** |
| --- | --- |
| propertyNames | Volitelný (výchozí hodnota je null). Vlastnosti, které mají být načteny ze serveru. |
| loadCompletedCallback | Volitelný (výchozí hodnota je null). Funkce, která má být volána po dokončení načítání. |
| failedCallback | Volitelný (výchozí hodnota je null). Funkci zavolat, když dojde k chybě. |
| userContext | Volitelný (výchozí hodnota je null). Informace o kontextu má být předán funkci zpětného volání. |

Funkce zatížení nemá návratovou hodnotu. Pokud volání bylo úspěšně dokončeno, bude volat buď `loadCompletedCallback` parametr nebo `defaultLoadCompletedCallback` vlastnost. Pokud volání se nezdařilo, nebo vypršel její časový limit, buď `failedCallback` parametr nebo `defaultFailedCallback` vlastností bude volána.

Pokud `propertyNames` parametr nezadáte, všechny vlastnosti nakonfigurovaná pro čtení se načítají ze serveru.

*Save – metoda:*

Metody save() uloží zadanou vlastnost seznamu (nebo všechny vlastnosti) do profilu technologie ASP.NET.

*Parametry:*

| **Název parametru** | **Význam** |
| --- | --- |
| propertyNames | Volitelný (výchozí hodnota je null). Vlastnosti, které mají být uloženy na serveru. |
| saveCompletedCallback | Volitelný (výchozí hodnota je null). Funkce, která má být volána při ukládání bylo dokončeno. |
| failedCallback | Volitelný (výchozí hodnota je null). Funkci zavolat, když dojde k chybě. |
| userContext | Volitelný (výchozí hodnota je null). Informace o kontextu má být předán funkci zpětného volání. |

Uložení funkce nemá návratovou hodnotu. Pokud volání dokončí úspěšně, bude volat buď `saveCompletedCallback` parametr nebo `defaultSaveCompletedCallback` vlastnost. Pokud volání se nezdařilo, nebo vypršel její časový limit, buď `failedCallback` nebo `defaultFailedCallback` vlastností bude volána.

Pokud `propertyNames` parametr má hodnotu null, všechny vlastnosti profilu se odešlou na server a server se rozhodnout, vlastnosti, které mohou být uloženy a které nemohou.

*Vlastnosti defaultFailedCallback (get, set):*

Tato vlastnost určuje funkci, která by měla být volána, pokud dojde k selhání ke komunikaci s webovou službou. Obdrželi delegáta (nebo odkazů na funkce).

Referenční dokumentace funkcí této vlastnosti by měl mít následující podpis:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample9.js)]

*Parametry:*

| **Název parametru** | **Význam** |
| --- | --- |
| Chyba | Určuje informace o této chybě. |
| userContext | Určuje informace o kontextu uživatele zadané při zatížení nebo uložit funkce byla volána. |
| methodName | Název volání metody. |

*Vlastnost defaultSaveCompleted (get, set):*

Tato vlastnost určuje funkci, která by měla být volána při dokončení uložení data profilu uživatele. Obdrželi delegáta (nebo odkazů na funkce).

Referenční dokumentace funkcí této vlastnosti by měl mít následující podpis:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample10.js)]

*Parametry:*

| **Název parametru** | **Význam** |
| --- | --- |
| numPropsSaved | Určuje počet vlastností, které byly uloženy. |
| userContext | Určuje informace o kontextu uživatele zadané při zatížení nebo uložit funkce byla volána. |
| methodName | Název volání metody. |

*Vlastnost defaultLoadCompleted (get, set):*

Tato vlastnost určuje funkci, která by měla být volána po dokončení načítání dat profilu uživatele. Obdrželi delegáta (nebo odkazů na funkce).

Referenční dokumentace funkcí této vlastnosti by měl mít následující podpis:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample11.js)]

*Parametry:*

| **Název parametru** | **Význam** |
| --- | --- |
| numPropsLoaded | Určuje počet vlastností načíst. |
| userContext | Určuje informace o kontextu uživatele zadané při zatížení nebo uložit funkce byla volána. |
| methodName | Název volání metody. |

*Vlastnost Path (get, set):*

Tato vlastnost určuje programově umístění profilu webové služby. Je možné přepsat výchozí zprostředkovatel profilu služby, stejně jako jeden nastavit deklarací v cestě vlastnosti ovládacího prvku ScriptManager ProfileService podřízený uzel.

Všimněte si, že se nezmění umístění výchozí službě profilů. ASP.NET AJAX však umožňuje určit umístění webovou službu, která poskytuje rozhraní třídy jako proxy služby ověřování ASP.NET AJAX.

Všimněte si také, že by neměla tato vlastnost nastavená na hodnotu, která přesměruje požadavek skriptu z aktuální lokality. AJAX podkladových technologie neúčtuje žádosti více webů a může vygenerovat výjimku zabezpečení do prohlížeče klienta.

Tato vlastnost je `String` objekt představující cestu k webové službě profilů.

*Vlastnost časového limitu (get, set):*

Tato vlastnost určuje dobu čekání pro službu profilů před za předpokladu, že načtení nebo uložení žádosti se nezdařilo. Pokud vyprší časový limit při čekání na dokončení volání, zavolá se zpětné volání požadavek se nezdařil a volání se nikdy nedokončí.

Tato vlastnost je `Number` objekt představující počet milisekund čekání na výsledky z profilu služby.

*Ukázka kódu: načítání dat profilu na načtení stránky*

Následující kód zkontroluje, jestli je uživatel ověřený a pokud ano, načte barvu pozadí upřednostňované uživatele jako stránky.

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample12.js)]

## <a name="using-a-custom-authentication-service-provider"></a>*Pomocí poskytovatele služeb vlastní ověřování*

Rozšíření ASP.NET AJAX umožňují vytvářet poskytovatele ověřování vlastní skript zveřejněním funkcí prostřednictvím vlastní webové služby. Pokud chcete použít, vaše webová služba musí vystavit dvě metody, `Login` a `Logout`; a tyto metody musí být zadaný pomocí stejné podpisy metod jako webovou službu ASP.NET AJAX ověřování výchozí.

Jakmile vytvoříte vlastní webovou službu, je potřeba zadat cestu k němu, buď pomocí deklarace na stránce programově v kódu nebo prostřednictvím klientského skriptu.

*Chcete-li nastavit cestu deklarativně:*

Pro nastavení cesty k deklarativně, patří podřízené AuthenticationService objektu ScriptManager na stránce ASP.NET:

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample13.aspx)]

*K nastavení cesty v kódu:*

Pro nastavení cesty k prostřednictvím kódu programu, zadejte cestu prostřednictvím instance Správce skriptu:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample14.cs)]

*Chcete-li nastavit cestu ve skriptu:*

Programové nastavení cesty ve skriptu, využívat `path` vlastnost AuthenticationService třídy:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample15.js)]

*Ukázková webová služba pro vlastní ověřování*

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample16.aspx)]

## <a name="summary"></a>Souhrn

Služby technologie ASP.NET – konkrétně služby profilace, členství a ověřování – jsou snadno přístupné pro jazyk JavaScript na klientském prohlížeči. To umožňuje vývojářům integrovat svůj kód na straně klienta pomocí mechanismu ověřování bez problémů, bez v závislosti na ovládací prvky, jako jsou komponenty UpdatePanel provedete rutinní. Data profilu se dá chránit z klienta, využívající webové nastavení konfigurace. nejsou k dispozici ve výchozím nastavení data a vývojáři musí přihlásit k vlastnosti profilu.

Kromě toho vytvořením implementací zjednodušené webové služby s podpisy metod ekvivalentní, mohou vývojáři vytvářet poskytovatelé vlastní skript pro tyto vnitřní služby ASP.NET. Podpora pro tyto techniky zjednodušuje vývoj aplikacemi rich client, poskytuje vývojářům s širokou škálou zajištění flexibility umožňující splnit určité požadavky.

## <a name="bio"></a>*Životopis*

Scott Cate má práce s Microsoft webových technologiích od roku 1997 a je prezident myKB.com ([www.myKB.com](http://www.myKB.com)) kde mu se specializuje na technologie ASP.NET psaní aplikací, zaměřuje na znalostní báze softwarová řešení založených na. Scott můžete kontaktovat prostřednictvím e-mailové adrese [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) nebo na svém blogu [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Předchozí](understanding-asp-net-ajax-updatepanel-triggers.md)
> [další](understanding-asp-net-ajax-localization.md)
