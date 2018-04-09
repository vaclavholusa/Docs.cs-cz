---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
title: Princip ověřování pomocí technologie ASP.NET AJAX a profil aplikačních služeb | Microsoft Docs
author: scottcate
description: Služba ověřování umožňuje uživatelům zadat přihlašovací údaje, aby bylo možné přijímat souboru cookie pro ověřování a se službou Brána, aby umožňovala vlastní uživatelské...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2008
ms.topic: article
ms.assetid: 6ab4efb6-aab6-45ac-ad2c-bdec5848ef9e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
msc.type: authoredcontent
ms.openlocfilehash: 0bf6538d0c4ae9488e6ac29ccba6d4b243cf070e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="understanding-aspnet-ajax-authentication-and-profile-application-services"></a>Princip ověřování pomocí technologie ASP.NET AJAX a profil aplikačních služeb
====================
podle [Scott Dikovat](https://github.com/scottcate)

[Stáhnout PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial03_MSAjax_ASP.NET_Services_cs.pdf)

> Ověřovací služby umožňuje uživatelům zadat přihlašovací údaje, aby bylo možné přijímat souboru cookie pro ověřování a se službou Brána, aby umožňovala vlastní uživatelské profily poskytuje pomocí technologie ASP.NET. Pomocí prvku ASP.NET AJAX ověřovací služby je kompatibilní s standardní ověřování pomocí formulářů ASP.NET, takže aplikace, které aktuálně používají ověřování pomocí formulářů (například přihlášení kontrolu) nebude ho rozdělit upgradem k ověřovací službě AJAX.


## <a name="introduction"></a>Úvod

V rámci rozhraní .NET Framework 3.5 poskytuje Microsoft upgrade značné množství prostředí; jenom je k dispozici nové vývojové prostředí, ale jsou chystaný nové funkce Language-Integrated Query (LINQ) a další rozšíření pro jazyk. Kromě toho některé známé funkce jiných modulové, zejména rozšíření ASP.NET AJAX, se nebudou zahrnuty jako první třídy členy knihovně tříd rozhraní .NET Framework Base. Tato rozšíření umožňují mnoha nových funkcí plně funkčního klienta, včetně částečným vykreslením stránky bez nutnosti celou stránku obnovení, přístup prostřednictvím klientského skriptu (včetně ASP.NET, rozhraní API pro profilaci) a rozhraní API rozsáhlé klientské webové služby určená pro řadu schémata řízení vidět v sadě serverový ovládací prvek ASP.NET zrcadlení.

Tento dokument White Paper vypadá na implementaci a použití profilace ASP.NET a ověřování pomocí formulářů jako vystavené AJAX rozšíření Microsoft ASP.NET AJAX ExtensionsThe provádějí služby ověřování pomocí formulářů pro podporu, jak se velmi snadno (a také Profilace služeb) jsou dostupné prostřednictvím skript proxy webové služby. Rozšíření AJAX také podporují vlastní ověřování prostřednictvím třídy AuthenticationServiceManager.

Tento dokument White Paper vychází z verze beta verzi 2 sady Visual Studio 2008 a rozhraní .NET Framework 3.5. Tento dokument White Paper také předpokládá, že budou pracovat se Visual Studio 2008 Beta 2, není Visual Web Developer Express a poskytne návody podle uživatelského rozhraní sady Visual Studio. Šablony projektů, které jsou k dispozici v aplikaci Visual Web Developer Express můžou využívat některé ukázky kódu.

## <a name="profiles-and-authentication"></a>*Profily a ověřování*

Microsoft ASP.NET profily a ověřovacích služeb jsou poskytované systémem ověřování ASP.NET pomocí formulářů a jsou standardní součástí technologie ASP.NET. Rozšíření ASP.NET AJAX poskytují skriptu přístup k těmto službám prostřednictvím proxy skriptu prostřednictvím přímočará modelu pod oborem názvů Sys.Services Klientská knihovna pro AJAX.

Ověřovací služby umožňuje uživatelům zadat přihlašovací údaje, aby bylo možné přijímat souboru cookie pro ověřování a se službou Brána, aby umožňovala vlastní uživatelské profily poskytuje pomocí technologie ASP.NET. Pomocí prvku ASP.NET AJAX ověřovací služby je kompatibilní s standardní ověřování pomocí formulářů ASP.NET, takže aplikace, které aktuálně používají ověřování pomocí formulářů (například přihlášení kontrolu) nebude ho rozdělit upgradem k ověřovací službě AJAX.

Profil služby umožňuje automatické integrace a úložiště dat uživatele na základě členství jako poskytovaný službou ověřování. Uložená data je uvedené v souboru web.config a různé profilování poskytovatelé služeb zpracovat data správy. Stejně jako u ověřovací služby, je kompatibilní s standardní profilovou službou ASP.NET AJAX profilovou službou, tak, aby stránky aktuálně zařadit funkce profilu ASP.NET služby by neměl přerušený zahrnutím podporu AJAX.

Ověřování pomocí technologie ASP.NET a služby profilování, sami zapojení do aplikace je mimo rozsah tohoto dokumentu. Další informace v tématu naleznete v knihovně MSDN odkazovat článku Správa uživatelů pomocí členství v [ https://msdn.microsoft.com/library/tw292whz.aspx ](https://msdn.microsoft.com/library/tw292whz.aspx). ASP.NET obsahuje také nástroj automaticky nastavit členství se systémem SQL Server, který je výchozím zprostředkovatelem služeb ověřování pro členství technologie ASP.NET. Další informace najdete v článku ASP.NET nástroj pro registraci serveru SQL Server (Aspnet\_regsql.exe) v [ https://msdn.microsoft.com/library/ms229862(vs.80).aspx ](https://msdn.microsoft.com/library/ms229862(vs.80).aspx).

## <a name="using-the-aspnet-ajax-authentication-service"></a>*Pomocí služby ověřování ASP.NET AJAX*

Služba ověřování pomocí technologie ASP.NET AJAX, musí být povoleno v souboru web.config:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample1.xml)]

Služba ověřování vyžaduje ověřování pomocí formulářů ASP.NET být povolena a vyžaduje povolení na klientském prohlížeči (skriptu nelze povolit bez souborů cookie relace vzhledem k tomu, že relací bez souborů cookie vyžadují parametry adresy URL) souborů cookie.

Jakmile ověřovací služby AJAX je povolené a nakonfigurované, můžete klientský skript okamžitě využít výhod Sys.Services.AuthenticationService objekt. Především se stává, bude klientský skript chcete využít výhod `login` metoda a `isLoggedIn` vlastnost. Existuje několik vlastností zajistit výchozí hodnoty pro metodu přihlášení, která může přijmout velký počet parametrů.

*Sys.Services.AuthenticationService členy*

*Metoda přihlášení:*

Metoda login() zahájí požadavek na ověření pověření uživatele. Tato metoda je asynchronní a nebrání provádění.

*Parametry:*

| **Název parametru** | **Význam** |
| --- | --- |
| userName | Požadováno. Uživatelské jméno k ověření. |
| Heslo | Volitelné (výchozí nastavení na hodnotu null). Heslo uživatele. |
| isPersistent | Volitelné (výchozí hodnota je false). Jestli má soubor cookie pro ověřování uživatele zachová napříč relacemi. Pokud je hodnota false, bude se uživatel odhlaste při zavření prohlížeče nebo platnosti relace. |
| redirectUrl | Volitelné (výchozí nastavení na hodnotu null). Adresa URL pro přesměrování prohlížeče po úspěšném ověření. Pokud tento parametr je null nebo prázdný řetězec, dojde k žádné přesměrování. |
| customInfo | Volitelné (výchozí nastavení na hodnotu null). Tento parametr se aktuálně nepoužívá a je rezervovaná pro budoucí použití. |
| loginCompletedCallback | Volitelné (výchozí nastavení na hodnotu null). Funkce se má volat při přihlášení bylo úspěšně dokončeno. -Li zadána, přepíše tento parametr vlastností defaultLoginCompleted. |
| failedCallback | Volitelné (výchozí nastavení na hodnotu null). Funkce se má volat při přihlášení se nezdařilo. -Li zadána, přepíše tento parametr vlastnosti defaultFailedCallback. |
| userContext | Volitelné (výchozí nastavení na hodnotu null). Data kontextu vlastní uživatele, která by měla být předána funkce zpětného volání. |

*Návratovou hodnotu:*

Tato funkce nezahrnuje návratovou hodnotu. Zahrnují po dokončení volání této funkce se však počet chování:

- Aktuální stránka bude nutné aktualizovat nebo změnit, pokud `redirectUrl` parametr měl hodnotu null ani prázdný řetězec.
- Nicméně, pokud parametr hodnotu null nebo prázdný řetězec, `loginCompletedCallback` parametr, nebo `defaultLoginCompletedCallback` je vlastnost s názvem.
- Pokud volání webové služby selže, `failedCallback` parametr `defaultFailedCallback` vlastnost je volána.

*Metoda odhlášení:*

Metoda logout() odstraní soubor cookie s pověřením a neodhlásí aktuální uživatel z webové aplikace.

*Parametry:*

| **Název parametru** | **Význam** |
| --- | --- |
| redirectUrl | Volitelné (výchozí nastavení na hodnotu null). Adresa URL pro přesměrování prohlížeče po úspěšném ověření. Pokud tento parametr je null nebo prázdný řetězec, dojde k žádné přesměrování. |
| logoutCompletedCallback | Volitelné (výchozí nastavení na hodnotu null). Funkce se má volat při odhlášení bylo úspěšně dokončeno. -Li zadána, přepíše tento parametr vlastností defaultLogoutCompleted. |
| failedCallback | Volitelné (výchozí nastavení na hodnotu null). Funkce se má volat při přihlášení se nezdařilo. -Li zadána, přepíše tento parametr vlastnosti defaultFailedCallback. |
| userContext | Volitelné (výchozí nastavení na hodnotu null). Data kontextu vlastní uživatele, která by měla být předána funkce zpětného volání. |

*Návratovou hodnotu:*

Tato funkce nezahrnuje návratovou hodnotu. Zahrnují po dokončení volání této funkce se však počet chování:

- Aktuální stránka bude nutné aktualizovat nebo změnit, pokud `redirectUrl` parametr měl hodnotu null ani prázdný řetězec.
- Nicméně, pokud parametr hodnotu null nebo prázdný řetězec, `logoutCompletedCallback` parametr, nebo `defaultLogoutCompletedCallback` je vlastnost s názvem.
- Pokud volání webové služby selže, `failedCallback` parametr `defaultFailedCallback` vlastnost je volána.

*Vlastnosti defaultFailedCallback (get, sada):*

Tato vlastnost určuje funkce, která by měla být volána, pokud dojde k selhání ke komunikaci s webovou službou. Obdrželi delegáta (nebo odkaz na funkci).

Odkaz na funkce této vlastnosti musí mít následující podpis:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample2.js)]

*Parametry:*

| **Název parametru** | **Význam** |
| --- | --- |
| Chyba | Určuje informace o chybě. |
| userContext | Určuje informace o kontextu uživatele zadané při volání funkce přihlášení nebo odhlášení. |
| MethodName | Název volání metody. |

*Vlastnost defaultLoginCompletedCallback (get, sada):*

Tato vlastnost určuje funkce, která by měla být volána po dokončení volání webové služby přihlášení. Obdrželi delegáta (nebo odkaz na funkci).

Odkaz na funkce této vlastnosti musí mít následující podpis:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample3.js)]

*Parametry:*

| **Název parametru** | **Význam** |
| --- | --- |
| validCredentials | Určuje, zda uživatel zadat platné přihlašovací údaje. `true` Pokud uživatel úspěšně přihlášen; v opačném případě `false`. |
| userContext | Určuje informace o kontextu uživatele zadané při volání funkce přihlášení. |
| MethodName | Název volání metody. |

*vlastnost defaultLogoutCompletedCallback (get, sada):*

Tato vlastnost určuje funkce, která by měla být volána po dokončení volání webové služby odhlášení. Obdrželi delegáta (nebo odkaz na funkci).

Odkaz na funkce této vlastnosti musí mít následující podpis:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample4.js)]

*Parametry:*

| **Název parametru** | **Význam** |
| --- | --- |
| výsledek | Tento parametr bude vždy `null`; je vyhrazený pro budoucí použití. |
| userContext | Určuje informace o kontextu uživatele zadané při volání funkce přihlášení. |
| MethodName | Název volání metody. |

*Vlastnost isLoggedIn (get):*

Tato vlastnost získá aktuální stav ověření uživatele; během požadavku na stránku nastavení objektem ScriptManager.

Tato vlastnost vrací `true` . když je uživatel momentálně přihlášen, jinak hodnota, vrátí `false`.

*Path – vlastnost (get, sada):*

Tato vlastnost určuje prostřednictvím kódu programu umístění Webová služba ověření. Může sloužit k přepsání výchozího zprostředkovatele ověřování, jakož i jeden nastavit deklarativně ve vlastnosti cesta ovládací prvek ScriptManager AuthenticationService podřízený uzel (Další informace najdete v tématu Použití vlastního zprostředkovatele ověřování služby téma níže).

Všimněte si, že umístění služby ověřování výchozí nezmění. Technologie ASP.NET AJAX však umožňuje zadat umístění Webová služba, která poskytuje stejné třídy rozhraní jako proxy služby ověřování prvku ASP.NET AJAX.

Všimněte si také, že by neměl být tato vlastnost nastavená na hodnotu, která přesměruje požadavek skriptu z aktuální lokality. Vzhledem k tomu, že aktuální aplikace nebude přijímat ověřovací pověření, bylo by zbytečné. také AJAX základní technologie neúčtuje požadavků webů a může generovat výjimka zabezpečení v prohlížeči klienta.

Tato vlastnost je `String` objektu, který představuje cestu k webové službě ověřování.

*Vlastnost časového limitu (get, sada):*

Tato vlastnost určuje dobu čekání pro službu ověřování před za předpokladu, že žádost o přihlášení se nezdařilo. Pokud vyprší časový limit při čekání na dokončení volání, nezavolá se zpětné volání požadavek se nezdařil a se nedokončí volání.

Tato vlastnost je `Number` objekt reprezentující počet milisekund čekání na výsledky z ověřovací služby.

*Ukázka kódu: Protokolování ve službě ověřování*

Následující kód je příklad stránky ASP.NET pomocí jednoduchého skriptu volání metody přihlášení a odhlášení AuthenticationService třídy.

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample5.aspx)]

## <a name="accessing-aspnet-profiling-data-via-ajax"></a>Přístup k ASP.NET profilace dat přes AJAX

ASP.NET profilace služby jsou taky dostupné prostřednictvím rozšíření ASP.NET AJAX. Vzhledem k tomu, že služba profilace ASP.NET poskytuje bohaté a podrobné rozhraní API podle kterého k ukládání a načítání uživatelských dat, může to být nástroj na vynikající produktivitu.

Profil služby musí být povoleno v souboru web.config; není ve výchozím nastavení. Uděláte to tak, ujistěte se, že `profileService` podřízený element povolil = true zadaný v souboru web.config a zda jste zadali vlastnosti, které můžou číst nebo zapisovat následujícím způsobem:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample6.xml)]

Službu profilu musí být taky nakonfigurovaný. Přestože konfigurace profilování služby je mimo rozsah tohoto dokumentu, je vhodné poznamenat, že skupiny definované v nastavení profilu konfigurace bude mít k dispozici dílčí vlastnosti je název skupiny. Například v následující části profil zadat:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample7.xml)]

Bude mít přístup k názvu, Address.Line1, Address.Line2, Address.City, Address.State, Address.Zip a BackgroundColor jako vlastnosti pole vlastnosti třídy ProfileService klientského skriptu.

Po nakonfigurování služby profilace AJAX bude okamžitě k dispozici na stránkách; však bude mít načíst jednou před použitím.

*Sys.Services.ProfileService členy*

*pole s vlastnostmi:*

Pole vlastnosti zpřístupní všechna data nakonfigurovaný profil jako podřízené vlastnosti, které lze odkazovat pomocí konvencí tečkou-– operátor name. Vlastnosti, které jsou podřízené skupiny vlastností se označují jako GroupName.PropertyName. V profilu příklad konfigurace uvedené výše Chcete-li získat stav uživatele, můžete použít tento identifikátor:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample8.cs)]

*Metoda load:*

Načte všechny vlastnosti nebo vybraný seznam ze serveru.

*Parametry:*

| **Název parametru** | **Význam** |
| --- | --- |
| propertyNames | Volitelné (výchozí nastavení na hodnotu null). Vlastnosti, které mají být načtena ze serveru. |
| loadCompletedCallback | Volitelné (výchozí nastavení na hodnotu null). Funkce se má volat při načítání byla dokončena. |
| failedCallback | Volitelné (výchozí nastavení na hodnotu null). Funkce zavolat, když dojde k chybě. |
| userContext | Volitelné (výchozí nastavení na hodnotu null). Informace o kontextu mají být předány funkce zpětného volání. |

Funkce zatížení nemá návratovou hodnotu. Pokud volání byla úspěšně dokončena, se bude volat buď `loadCompletedCallback` parametr nebo `defaultLoadCompletedCallback` vlastnost. Pokud toto volání se nezdařilo nebo vypršel časový limit, buď `failedCallback` parametr nebo `defaultFailedCallback` vlastností bude volána.

Pokud `propertyNames` není zadán parametr, všechny vlastnosti nakonfigurované pro čtení se načítají ze serveru.

*Save – metoda:*

Metoda save() uloží do profilu uživatele ASP.NET seznamu zadanou vlastnost (nebo všechny vlastnosti).

*Parametry:*

| **Název parametru** | **Význam** |
| --- | --- |
| propertyNames | Volitelné (výchozí nastavení na hodnotu null). Vlastnosti, které chcete uložit na server. |
| saveCompletedCallback | Volitelné (výchozí nastavení na hodnotu null). Funkce se má volat při ukládání byla dokončena. |
| failedCallback | Volitelné (výchozí nastavení na hodnotu null). Funkce zavolat, když dojde k chybě. |
| userContext | Volitelné (výchozí nastavení na hodnotu null). Informace o kontextu mají být předány funkce zpětného volání. |

Uložení funkce nemá návratovou hodnotu. Po úspěšném dokončení volání se zavolá buď `saveCompletedCallback` parametr nebo `defaultSaveCompletedCallback` vlastnost. Pokud toto volání se nezdařilo nebo vypršel časový limit, buď `failedCallback` nebo `defaultFailedCallback` vlastností bude volána.

Pokud `propertyNames` parametr má hodnotu null, odešle se všechny vlastnosti profilu k serveru a serveru rozhodne vlastnosti, které lze uložit a které nemohou.

*Vlastnosti defaultFailedCallback (get, sada):*

Tato vlastnost určuje funkce, která by měla být volána, pokud dojde k selhání ke komunikaci s webovou službou. Obdrželi delegáta (nebo odkaz na funkci).

Odkaz na funkce této vlastnosti musí mít následující podpis:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample9.js)]

*Parametry:*

| **Název parametru** | **Význam** |
| --- | --- |
| Chyba | Určuje informace o chybě. |
| userContext | Určuje informace o kontextu uživatele zadané při zatížení nebo uložte funkce byla volána. |
| MethodName | Název volání metody. |

*Vlastnost defaultSaveCompleted (get, sada):*

Tato vlastnost určuje funkce, která by měla být volána po dokončení ukládání dat profilu uživatele. Obdrželi delegáta (nebo odkaz na funkci).

Odkaz na funkce této vlastnosti musí mít následující podpis:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample10.js)]

*Parametry:*

| **Název parametru** | **Význam** |
| --- | --- |
| numPropsSaved | Určuje počet vlastností, které byly uloženy. |
| userContext | Určuje informace o kontextu uživatele zadané při zatížení nebo uložte funkce byla volána. |
| MethodName | Název volání metody. |

*Vlastnost defaultLoadCompleted (get, sada):*

Tato vlastnost určuje funkce, která by měla být volána po dokončení načítání dat profilu uživatele. Obdrželi delegáta (nebo odkaz na funkci).

Odkaz na funkce této vlastnosti musí mít následující podpis:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample11.js)]

*Parametry:*

| **Název parametru** | **Význam** |
| --- | --- |
| numPropsLoaded | Určuje počet vlastnosti načteny. |
| userContext | Určuje informace o kontextu uživatele zadané při zatížení nebo uložte funkce byla volána. |
| MethodName | Název volání metody. |

*Path – vlastnost (get, sada):*

Tato vlastnost určuje prostřednictvím kódu programu umístění profilu webové služby. Může sloužit k přepsání výchozího poskytovatele služby profilu, jakož i jeden nastavit deklarativně ve vlastnosti cesta ovládací prvek ScriptManager ProfileService podřízený uzel.

Všimněte si, že umístění služby výchozí profil se nemění. Technologie ASP.NET AJAX však umožňuje zadat umístění Webová služba, která poskytuje stejné třídy rozhraní jako proxy služby ověřování prvku ASP.NET AJAX.

Všimněte si také, že by neměl být tato vlastnost nastavená na hodnotu, která přesměruje požadavek skriptu z aktuální lokality. Základní AJAX technologie neúčtuje požadavků webů a může generovat výjimka zabezpečení v prohlížeči klienta.

Tato vlastnost je `String` objektu, který představuje cestu k webové službě profilu.

*Vlastnost časového limitu (get, sada):*

Tato vlastnost určuje dobu čekání službu profilu před za předpokladu, že zatížení nebo uložení žádosti se nezdařilo. Pokud vyprší časový limit při čekání na dokončení volání, nezavolá se zpětné volání požadavek se nezdařil a se nedokončí volání.

Tato vlastnost je `Number` objekt reprezentující počet milisekund čekání na výsledky z profilu služby.

*Ukázka kódu: načítání dat profilu na načtení stránky*

Následující kód zjistí, zda je uživatel ověřen a pokud ano, načte barvu pozadí upřednostňované uživatele jako stránky.

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample12.js)]

## <a name="using-a-custom-authentication-service-provider"></a>*Pomocí zprostředkovatele služeb vlastního ověřování*

Rozšíření ASP.NET AJAX umožňují vytvořit vlastní skript poskytovatele služby ověřování díky zpřístupnění vaší funkce prostřednictvím vlastní webové služby. Aby bylo možné použít, musí vaše webové služby vystavit dvě metody, `Login` a `Logout`; a tyto metody musí být zadána se stejnou podpisy metoda jako webovou službu ASP.NET AJAX ověřování výchozí.

Jakmile vytvoříte vlastní webovou službu, musíte zadat cestu k jeho, buď deklarativně na stránku, prostřednictvím kódu programu v kódu, nebo prostřednictvím klientského skriptu.

*Nastavení cesty deklarativně:*

Nastavení cesty deklarativně, patří podřízené AuthenticationService ScriptManager objektu na stránku ASP.NET:

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample13.aspx)]

*Nastavení cesty v kódu:*

Nastavení cesty prostřednictvím kódu programu, zadejte cestu prostřednictvím instanci správce vašeho skriptu:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample14.cs)]

*Nastavení cesty ve skriptu:*

Nastavení cesty prostřednictvím kódu programu ve skriptu, využívají `path` vlastnost AuthenticationService třídy:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample15.js)]

*Ukázkové webové služby pro vlastní ověřování*

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample16.aspx)]

## <a name="summary"></a>Souhrn

ASP.NET – služby – konkrétně profilace, členství a ověřování služby – snadno vystaveni JavaScript na klientském prohlížeči. To umožňuje vývojářům jejich kódu na straně klienta s ověřovací mechanismus umožňují snadnou integraci, bez v závislosti na ovládací prvky, jako jsou komponenty UpdatePanel udělat lifting náročné. Data profilu můžete chránit z klienta i, s využitím nastavení webové konfigurace. nejsou k dispozici ve výchozím nastavení data a vývojáři musí přihlásit k vlastnosti profilu.

Kromě toho vytvořením implementací zjednodušené webové služby s podpisy ekvivalentní metoda vývojáři můžou vytvářet vlastní skript zprostředkovatele pro tyto vnitřní služby ASP.NET. Podpora pro tyto postupy usnadňuje vývoj aplikacemi rich client současně vývojářům nabízí širokou škálu flexibilitu podle konkrétních potřeb.

## <a name="bio"></a>*Bio*

Scott Dikovat pracuje s technologií Microsoft Web od 1997 a je ředitel myKB.com ([www.myKB.com](http://www.myKB.com)) kde mu se specializuje na psaní ASP.NET na základě aplikací, které jsou zaměřené na řešení softwaru znalostní báze Knowledge Base. Scott nelze kontaktovat prostřednictvím e-mailu v [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) nebo jeho blog na [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Předchozí](understanding-asp-net-ajax-updatepanel-triggers.md)
> [další](understanding-asp-net-ajax-localization.md)
