---
uid: web-forms/overview/older-versions-security/membership/user-based-authorization-cs
title: Ověřování založené na uživatelích (C#) | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu se podíváme na omezení přístupu na stránky a omezení funkce na úrovni stránky prostřednictvím různých technik.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 3c815a9e-2296-4b9b-b945-776d54989daa
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/membership/user-based-authorization-cs
msc.type: authoredcontent
ms.openlocfilehash: 5ab8fb2782cf9d29c9912fa81158c50a9d8d8af5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396775"
---
<a name="user-based-authorization-c"></a>Ověřování založené na uživatelích (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_07_CS.zip) nebo [stahovat PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial07_UserAuth_cs.pdf)

> V tomto kurzu se podíváme na omezení přístupu na stránky a omezení funkce na úrovni stránky prostřednictvím různých technik.


## <a name="introduction"></a>Úvod

Většiny webových aplikací, které nabízejí uživatelské účty tak učinit v části omezit určité návštěvníků přístup k určité stránky v rámci stránky. V nejvíce online messageboard servery například všichni uživatelé – anonymní a ověřené - budou moct zobrazit messageboard příspěvky, ale jenom ověření uživatelé navštívit webovou stránku, aby se vytvoří nový příspěvek. A může být pro správu stránky, které jsou přístupné pro konkrétního uživatele (nebo konkrétní sadu uživatelů). Kromě toho funkce na úrovni stránky se může lišit na základě uživatele uživatelů. Při prohlížení seznam příspěvků, ověřeným uživatelům se zobrazí rozhraní pro hodnocení každého příspěvku, že toto rozhraní není k dispozici pro anonymní návštěvníků.

Technologie ASP.NET umožňuje snadno definovat pravidla ověřování na základě uživatele. S jen trochu jazyka `Web.config`, konkrétní webové stránky nebo celou adresáře můžete být uzamčen, tak, aby pouze jsou přístupné pro zadaný podmnožině uživatelů. Funkce na úrovni stránky je možné zapnout nebo vypnout podle aktuálně přihlášeného uživatele prostřednictvím kódu programu a deklarativní způsobem.

V tomto kurzu se podíváme na omezení přístupu na stránky a omezení funkce na úrovni stránky prostřednictvím různých technik. Pusťme se do práce!

## <a name="a-look-at-the-url-authorization-workflow"></a>Podívejte se na pracovní postup autorizace adresy URL

Jak je popsáno v [ *Přehled ověřování založené na formulářích* ](../introduction/an-overview-of-forms-authentication-cs.md) kurz, když modul runtime ASP.NET zpracovává žádost pro prostředek ASP.NET žádost umocnit číslo události během životního cyklu. *Z modulů HTTP* jsou spravované třídy, jejíž kód je prováděn v reakci na konkrétní událost v životního cyklu požadavku. Technologie ASP.NET se dodává s celou řadou z modulů HTTP, který provádění základních úloh na pozadí.

Jeden takový modul HTTP je [ `FormsAuthenticationModule` ](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx). Jak je popsáno v předchozích kurzech primárních funkcí `FormsAuthenticationModule` je k určení identity aktuálního požadavku. To se provádí kontrola lístek ověřování pomocí formulářů, které je umístěný v souboru cookie nebo vložené v rámci adresy URL. Tato identifikace dojde během [ `AuthenticateRequest` události](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Je jiný důležité modul HTTP [ `UrlAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx), která je vyvolána v reakci na [ `AuthorizeRequest` události](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx) (který se stane po `AuthenticateRequest` událostí). `UrlAuthorizationModule` Zkontroluje konfiguraci značek v `Web.config` slouží k určení, zda aktuální identita má oprávnění naleznete na stránce zadané. Tento proces se označuje jako *autorizace adres URL*.

Prozkoumáme syntaxe autorizačních pravidel adres URL v kroku 1, ale nejprve Pojďme podívat na co `UrlAuthorizationModule` nemá v závislosti na tom, zda je požadavek autorizován nebo ne. Pokud `UrlAuthorizationModule` zjistí, že je požadavek autorizován, a to nemá žádný účinek a požadavek pokračuje přes životního cyklu. Ale pokud se jedná *není* oprávnění, pak bude `UrlAuthorizationModule` zruší životní cyklus a předá instrukci `Response` objektu, který chcete vrátit [HTTP 401 neoprávněný](http://www.checkupdown.com/status/E401.html) stav. Při použití ověřování pomocí formulářů tento stav HTTP 401 je nikdy vrácen do klienta protože pokud `FormsAuthenticationModule` zjistí HTTP 401 je stav změní na [přesměrování HTTP 302](http://www.checkupdown.com/status/E302.html) na přihlašovací stránku.

Obrázek 1 ukazuje pracovní postup kanálu ASP.NET `FormsAuthenticationModule`a `UrlAuthorizationModule` při přijetí neoprávněného požadavku. Konkrétně se obrázek 1 ukazuje požadavek anonymní návštěvníkem pro `ProtectedPage.aspx`, což je stránka, která se odepře přístup pro anonymní uživatele. Protože je k anonymní, návštěvník `UrlAuthorizationModule` zruší požadavek a vrátí HTTP 401 Unauthorized status. `FormsAuthenticationModule` Stavu 401 převede po přesměrování 302 na přihlašovací stránku. Po ověření uživatele přes stránku pro přihlášení, že je přesměrován na `ProtectedPage.aspx`. Tentokrát `FormsAuthenticationModule` identifikuje uživatele na základě jeho lístku ověřování. Teď, když je ověřen návštěvníka, `UrlAuthorizationModule` povoluje přístup ke stránce.


[![Ověřování pomocí formulářů a pracovní postup autorizace adresy URL](user-based-authorization-cs/_static/image2.png)](user-based-authorization-cs/_static/image1.png)

**Obrázek 1**: ověřování pomocí formulářů a pracovní postup autorizace adresy URL ([kliknutím ji zobrazíte obrázek v plné velikosti](user-based-authorization-cs/_static/image3.png))


Obrázek 1 znázorňuje interakci, která nastane, pokud anonymní návštěvníka pokusí získat přístup k prostředku, který není k dispozici pro anonymní uživatele. V takovém případě anonymní návštěvníka přesměruje na přihlašovací stránku s stránku, kterou se uživatel pokusil navštivte zadané v poli řetězec dotazu. Po úspěšném přihlášení uživatele si budete automaticky přesměrováni zpět na prostředek, který uživatel byl zpočátku pokusu o zobrazení.

Po provedení neautorizované žádosti anonymního uživatele, tento pracovní postup je jednoduché a snadno pro návštěvníka pochopit, co se stalo a proč. Ale mějte na paměti, že `FormsAuthenticationModule` přesměruje *jakékoli* neoprávněný uživatel na přihlašovací stránku i v případě, že byla podána žádost od ověřeného uživatele. Pokud ověřený uživatel se pokusí najdete na stránce, u kterého uživatel nemá autority to může vést matoucí činnost koncového uživatele.

Představte si, že náš web měli jeho autorizačních pravidel adres URL nakonfigurovaná tak, aby stránka technologie ASP.NET `OnlyTito.aspx` bylo accessibly pouze Tito. Nyní, imagine Sam navštíví web, přihlášení a poté se pokusí navštivte `OnlyTito.aspx`. `UrlAuthorizationModule` Zastaví životního cyklu požadavku, který se vrátí HTTP 401 Unauthorized status, které `FormsAuthenticationModule` rozpozná a potom přesměrujte Sam na přihlašovací stránku. Vzhledem k tomu, že již byl přihlášen Sam, ale marcela může zajímat, proč si byl odeslán zpět na stránku pro přihlášení. Marcela může být z důvodu, že její přihlašovací údaje byly ztraceny nějakým způsobem, nebo že Jana zadané neplatné přihlašovací údaje. Pokud Sam znovu vstoupí svých přihlašovacích údajů z přihlašovací stránky Jana se přihlášení (znovu) a přesměrování na `OnlyTito.aspx`. `UrlAuthorizationModule` Zjistí, že Sam nemůžou navštívit tuto stránku, a Jana se vrátit na přihlašovací stránku.

Obrázek 2 znázorňuje tento pracovní postup matoucí.


[![Výchozí pracovní postup může mít za následek matoucí cyklu](user-based-authorization-cs/_static/image5.png)](user-based-authorization-cs/_static/image4.png)

**Obrázek 2**: The výchozí pracovní postup může mít za následek cyklus matoucí ([kliknutím ji zobrazíte obrázek v plné velikosti](user-based-authorization-cs/_static/image6.png))


Pracovní postup znázorněné na obrázku 2 můžete rychle befuddle i většinu počítače koumák návštěvníka. Podíváme se na způsoby, jak tomu zabránit, matoucí cyklus v kroku 2.

> [!NOTE]
> ASP.NET používá k určení, zda má aktuální uživatel přístup konkrétní webovou stránku dva mechanismy: Autorizace adres URL a souborů. Autorizace souboru je implementované [ `FileAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx), který určuje o požadované soubory seznamů řízení přístupu autorita. Autorizace souboru se nejčastěji používá s ověřováním Windows, protože seznamy ACL jsou oprávnění, které se vztahují na účty Windows. Při použití ověřování pomocí formulářů, všechny požadavky operačního systému a soubor úrovni systému jsou spouštěny příkazem stejný účet Windows, bez ohledu na následujícím webu uživatele. Protože v této sérii kurzů se zaměřuje na ověřování pomocí formulářů, společnost Microsoft nebude mluvit o autorizace souboru.


### <a name="the-scope-of-url-authorization"></a>Obor autorizace adres URL

`UrlAuthorizationModule` Je spravovaný kód, který je součástí modulu runtime ASP.NET. Starší než verze 7 od Microsoftu [Internetové informační služby (IIS)](https://www.iis.net/) webový server, se liší bariéru mezi kanálu protokolu HTTP služby IIS a modulu runtime ASP.NET kanálu. Stručně řečeno, IIS 6 a dřívějších verzí ASP. NET společnosti `UrlAuthorizationModule` provede jenom v případě požadavku se deleguje ze služby IIS na modul runtime ASP.NET. Ve výchozím nastavení, služba IIS zpracovává statický obsah, samotný – například stránky HTML a CSS, JavaScript a soubory obrázků – a pouze předá požadavky na modul runtime ASP.NET, pokud stránka s rozšířením `.aspx`, `.asmx`, nebo `.ashx` je požadováno.

Integrované služby IIS a ASP.NET IIS 7, ale umožňuje spouštění kanálů. S několika nastavení konfigurace můžete nastavit službu IIS 7, který má být vyvolán `UrlAuthorizationModule` pro *všechny* požadavky, což znamená, že pro soubory libovolného typu je možné definovat autorizačních pravidel adres URL. Kromě toho zahrnuje IIS 7 svůj vlastní stroj autorizace adresy URL. Další informace o integrace technologie ASP.NET a IIS 7 nativních funkcí autorizace adresy URL, najdete v části [autorizace adres URL pro službu IIS7 Principy](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization). Podrobnější rozbor integraci technologie ASP.NET a IIS 7, vyberte kopii Shahram Khosravi knihy *Professional IIS 7 a ASP.NET integrovaný programovací* (ISBN: 978 0470152539).

Řečeno v kostce ve verzích před IIS 7, adresa URL autorizační pravidla platí pouze pro prostředky, zpracovává modul runtime ASP.NET. Se službou IIS 7 je však možné používat funkce na nativní autorizace adresy URL služby IIS nebo integrovat ASP. NET společnosti `UrlAuthorizationModule` do kanálu HTTP služby IIS, tím rozšiřuje tuto funkci pro všechny požadavky.

> [!NOTE]
> Existují určité jednoduchý, ale důležité rozdíly v tom ASP. NET společnosti `UrlAuthorizationModule` a funkce autorizace adresy URL služby IIS 7 zpracování autorizačních pravidel. Funkce autorizace adresy URL služby IIS 7 nebo rozdíly mezi jak analyzuje autorizační pravidla ve srovnání s nezkoumá v tomto kurzu `UrlAuthorizationModule`. Další informace o těchto tématech naleznete v dokumentaci služby IIS 7 na webu MSDN nebo na [www.iis.net](https://www.iis.net/).


## <a name="step-1-defining-url-authorization-rules-inwebconfig"></a>Krok 1: Definování autorizačních pravidel adres URL v`Web.config`

`UrlAuthorizationModule` Určuje, jestli se má udělit nebo odepřít přístup k požadovanému prostředku pro konkrétní identity na základě pravidel autorizace URL definované v konfiguraci vaší aplikace. Autorizační pravidla jsou vypsány v [ `<authorization>` element](https://msdn.microsoft.com/library/8d82143t.aspx) ve formě `<allow>` a `<deny>` podřízené prvky. Každý `<allow>` a `<deny>` podřízený prvek můžete zadat:

- Určitého uživatele
- Seznam uživatelů oddělených čárkou
- Všichni anonymní uživatelé označeny otazníkem (?)
- Všichni uživatelé, označený hvězdičkou (\*)

Následující kód ukazuje, jak můžete uživatelům Tito a Scott povolit a zakázat všechny ostatní autorizačních pravidel adres URL:

[!code-xml[Main](user-based-authorization-cs/samples/sample1.xml)]

`<allow>` Element definuje, co uživatelé jsou povolené - Tito a Scott – zatímco `<deny>` element, který instruuje *všechny* uživatelé mají odepřený.

> [!NOTE]
> `<allow>` a `<deny>` prvky můžete také určit autorizační pravidla pro role. Prozkoumáme ověřování na základě role v budoucích kurzech.


Následující nastavení povolí přístup všem uživatelům kromě Sam (včetně anonymních návštěvníci):

[!code-xml[Main](user-based-authorization-cs/samples/sample2.xml)]

Umožňuje pouze ověřeným uživatelům, použijte následující konfiguraci, čímž se odmítne přístup všichni anonymní uživatelé:

[!code-xml[Main](user-based-authorization-cs/samples/sample3.xml)]

Autorizační pravidla jsou definována v rámci `<system.web>` prvek `Web.config` a platí pro všechny prostředky technologie ASP.NET ve webové aplikaci. Aplikace často, má jiné autorizačních pravidel pro různé oddíly. Například na webu elektronického obchodování, všechny návštěvníky může zobrazit produkty, podívat na hodnocení produktu, vyhledejte v katalogu atd. Pouze ověřeným uživatelům může dosáhnout však rezervace nebo na stránkách ke správě svých přesouvání historie. Kromě toho může být částí webu, které jsou přístupné pro výběr uživatelů, třeba o správce lokality.

Technologie ASP.NET umožňuje snadno definovat různé autorizačních pravidel pro různé soubory a složky v lokalitě. Autorizační pravidla zadaná v kořenové složce `Web.config` souboru se vztahují na všechny prostředky technologie ASP.NET v lokalitě. Však lze přepsat toto výchozí nastavení autorizace pro konkrétní složku tak, že přidáte `Web.config` s `<authorization>` oddílu.

Umožňuje aktualizovat našeho webu tak, aby pouze ověřeným uživatelům najdete na stránkách ASP.NET `Membership` složky. K tomu je potřeba přidat `Web.config` do souboru `Membership` složku a její nastavení autorizace anonymním uživatelům odepřít. Klikněte pravým tlačítkem myši `Membership` složku v Průzkumníku řešení zvolte v nabídce Přidat novou položku v místní nabídce a přidejte nový soubor webové konfigurace s názvem `Web.config`.


[![Přidat soubor Web.config ke složce členství](user-based-authorization-cs/_static/image8.png)](user-based-authorization-cs/_static/image7.png)

**Obrázek 3**: Přidejte `Web.config` do souboru `Membership` složky ([kliknutím ji zobrazíte obrázek v plné velikosti](user-based-authorization-cs/_static/image9.png))


Váš projekt v tomto okamžiku by měla obsahovat dva `Web.config` soubory: jeden v kořenovém adresáři a druhý v `Membership` složky.


[![Vaše aplikace by měla nyní obsahovat dva soubory Web.config](user-based-authorization-cs/_static/image11.png)](user-based-authorization-cs/_static/image10.png)

**Obrázek 4**: vaše aplikace by měla nyní obsahovat dvě `Web.config` soubory ([kliknutím ji zobrazíte obrázek v plné velikosti](user-based-authorization-cs/_static/image12.png))


Aktualizovat konfigurační soubor v `Membership` složku tak, že zakazují přístup pro anonymní uživatele.

[!code-xml[Main](user-based-authorization-cs/samples/sample4.xml)]

To je všechno je to!

K otestování této změny, přejděte na domovskou stránku v prohlížeči a ujistěte se, že jste se přihlásili navýšení kapacity. Protože výchozí chování aplikace technologie ASP.NET je umožnit všem návštěvníkům a vzhledem k tomu, že jsme nevytvořili žádné změny autorizace do kořenového adresáře `Web.config` souboru, jsme schopní navštivte soubory v kořenovém adresáři jako anonymní návštěvníka.

Kliknutím na odkaz vytváření uživatelských účtů v levém sloupci. Tím přejdete na `~/Membership/CreatingUserAccounts.aspx`. Protože `Web.config` ve `Membership` složky definuje autorizační pravidla za účelem zakázat anonymní přístup `UrlAuthorizationModule` zruší požadavek a vrátí HTTP 401 Unauthorized status. `FormsAuthenticationModule` Upraví to 302 stavu přesměrování, odeslání na přihlašovací stránku. Všimněte si, že na stránce jsme se pokouší o přístup (`CreatingUserAccounts.aspx`) je předána na stránku pro přihlášení přes `ReturnUrl` parametr řetězce dotazu.


[![Od adresy URL autorizační pravidla zakázat anonymní přístup jsme přesměrováni na stránku pro přihlášení](user-based-authorization-cs/_static/image14.png)](user-based-authorization-cs/_static/image13.png)

**Obrázek 5**: protože autorizace adres URL pravidla zakázat anonymní přístup, jsme přesměrováni na stránku pro přihlášení ([kliknutím ji zobrazíte obrázek v plné velikosti](user-based-authorization-cs/_static/image15.png))


Po úspěšném přihlášení, jsme přesměrováni `CreatingUserAccounts.aspx` stránky. Tentokrát `UrlAuthorizationModule` povoluje přístup na stránku, protože jsme už nejsou anonymní.

### <a name="applying-url-authorization-rules-to-a-specific-location"></a>Použití autorizačních pravidel adres URL do určitého umístění

Povolení nastavení definovaných v `<system.web>` část `Web.config` platí pro všechny prostředky technologie ASP.NET v tomto adresáři a jeho podadresářích (dokud přepsány jiným `Web.config` souboru). V některých případech se však může chceme, všech prostředků ASP.NET v daném adresáři mít konkrétní autorizace konfigurace s výjimkou jednoho nebo dvou konkrétní stránky. Toho lze dosáhnout tak, že přidáte `<location>` prvek `Web.config`, odkazující na soubor, jehož autorizační pravidla se liší a definování jeho jedinečné autorizační pravidla v něm.

Pro ilustraci použití `<location>` prvek, který chcete přepsat nastavení konfigurace pro určitý prostředek, můžeme přizpůsobit nastavení autorizace tak, aby navštívit pouze Tito `CreatingUserAccounts.aspx`. Chcete-li to provést, přidejte `<location>` elementu `Membership` složky `Web.config` souboru a aktualizujte svůj kód tak, aby vypadal jako následující:

[!code-xml[Main](user-based-authorization-cs/samples/sample5.xml)]

`<authorization>` Prvek `<system.web>` definuje výchozí adresa URL autorizační pravidla pro prostředky technologie ASP.NET v `Membership` složce a jejích podsložkách. `<location>` Element umožňuje přepsat tato pravidla pro určitý prostředek. Ve výše uvedené značky `<location>` elementu odkazy `CreatingUserAccounts.aspx` stránky a určí, jeho autorizační pravidla, například za účelem povolení Tito, ale všichni ostatní odepřít.

K otestování této změny autorizace, začněte návštěvou webu jako anonymní uživatel. Pokud při pokusu o návštěvu všechny stránky v `Membership` složky, například `UserBasedAuthorization.aspx`, `UrlAuthorizationModule` odmítne žádost a budete přesměrováni na přihlašovací stránku. Po přihlášení jako například Scott, můžete navštívit libovolné stránky `Membership` složky *s výjimkou* pro `CreatingUserAccounts.aspx`. Pokus o navštivte `CreatingUserAccounts.aspx` přihlášeni jako kdokoli, ale Tito dojde pokusu o neoprávněný přístup budete přesměrování zpět na stránku pro přihlášení.

> [!NOTE]
> `<location>` Elementu musí nacházet mimo v konfiguraci `<system.web>` elementu. Musíte použít samostatné `<location>` – element pro každý prostředek, jehož nastavení ověřování, kterou chcete přepsat.


### <a name="a-look-at-how-theurlauthorizationmoduleuses-the-authorization-rules-to-grant-or-deny-access"></a>Podívejte se na tom, jak`UrlAuthorizationModule`používá autorizační pravidla udělit nebo odepřít přístup

`UrlAuthorizationModule` Určuje, jestli k autorizaci konkrétní identitu pro určitou adresu URL díky analýze autorizace adres URL pravidla postupně, počínaje první z nich a funguje směrem dolů. Jakmile se najde shoda, je uživatel povolen nebo odepřen přístup, v závislosti na if v byla nalezena shoda `<allow>` nebo `<deny>` elementu. <strong>Pokud není nalezena žádná shoda, uživateli je udělen přístup.</strong> V důsledku toho pokud chcete omezit přístup, je nutné použít `<deny>` prvek jako poslední prvek v konfiguraci autorizace adresy URL. <strong>Vynecháte-li</strong><strong>`<deny>`</strong><strong>elementu, všichni uživatelé budou mít přístup.</strong>

Abyste lépe pochopili proces využívaný sadou `UrlAuthorizationModule` určit autority, podívejte se na příklad autorizačních pravidel adres URL, které jsme se podívali na dříve v tomto kroku. První pravidlo je `<allow>` element, který umožňuje přístup ke Tito a Scott. Je druhá pravidla `<deny>` element, který se odepře přístup všem uživatelům. Pokud anonymní uživatel navštíví, `UrlAuthorizationModule` začne s dotazem, je anonymní Scott nebo Tito? Odpověď, samozřejmě, je Ne, tak pokračuje do druhé pravidlo. Je anonymní v sadě všichni? Protože odpověď je tady Ano, `<deny>` pravidlo je umístěn v platnosti a návštěvníka se přesměruje na přihlašovací stránku. Podobně, pokud je návštěva Jisun, `UrlAuthorizationModule` začne s dotazem, je Jisun Scott nebo Tito? Vzhledem k tomu, že uživatel není `UrlAuthorizationModule` pokračuje na druhou otázku Jisun je v sadě všichni? Marcela se tak, že příliš, je odepřen přístup. Nakonec, pokud Tito návštěvy, na první otázku kompenzaci `UrlAuthorizationModule` se kladná odpověď, aby Tito je udělen přístup.

Protože `UrlAuthorizationModule` povolení pravidla shora dolů, zastavování na žádná shoda, je důležité mít konkrétnější pravidla předcházet ta méně konkrétní procesy. To znamená Chcete-li definovat autorizační pravidla, která zakazuje Jisun a anonymní uživatele, ale všechny ostatní ověřeným uživatelům umožnit, by začít s nejspecifičtější pravidla – jeden ovlivňuje Jisun – a pak pokračujte s méně konkrétní pravidla - pravidla povolení všech ostatních ověřené uživatele, ale odepření všichni anonymní uživatelé. Následující adresa URL autorizační pravidla implementuje tyto zásady tak, že nejprve odepření Jisun a potom odepření anonymní uživatel. Každý ověřený uživatel než Jisun bude udělen přístup protože ani jedno z těchto `<deny>` příkazy budou odpovídat.

[!code-xml[Main](user-based-authorization-cs/samples/sample6.xml)]

## <a name="step-2-fixing-the-workflow-for-unauthorized-authenticated-users"></a>Krok 2: Stanovení pracovního postupu pro neoprávněné, ověřeného uživatele

Jak jsme probírali dříve v tomto kurzu v A podívejte se na část pracovní postup autorizace adresy URL, kdykoli ukáže neoprávněného požadavku, `UrlAuthorizationModule` zruší požadavek a vrátí HTTP 401 Unauthorized status. Tento stav 401 je upraven na `FormsAuthenticationModule` do 302 přesměrování stav, který se odešle uživatel na přihlašovací stránku. Tento pracovní postup probíhá na neoprávněného požadavku, i když je uživatel ověřený.

Vrací ověřeného uživatele na přihlašovací stránku se pravděpodobně je matoucí, protože jste už přihlášení do systému. Stačí nepatrné práce můžeme vylepšit tento pracovní postup přesměrováním ověřeným uživatelům, kteří žádají neoprávněné požadavky na stránku, která vysvětluje, že se pokusí otevřít stránku s omezeným přístupem.

Začněte tím, že vytvoříte novou stránku ASP.NET v kořenové složce webové aplikace s názvem `UnauthorizedAccess.aspx`; nezapomeňte přidružit tuto stránku `Site.master` stránky předlohy. Po vytvoření této stránky, odebrat ovládací prvek obsahu, který odkazuje `LoginContent` ContentPlaceHolder tak, aby výchozí obsahu stránky předlohy se zobrazí. Dále přidejte zprávu, která popisuje situaci, a to, že se uživatel pokusil o přístup k chráněnému prostředku. Po přidání zpráva, `UnauthorizedAccess.aspx` deklarativním označení stránky by měl vypadat nějak takto:

[!code-aspx[Main](user-based-authorization-cs/samples/sample7.aspx)]

Nyní potřebujeme ke změně pracovního postupu tak, že pokud neoprávněného požadavku se provádí pomocí ověřeného uživatele jsou odeslány do `UnauthorizedAccess.aspx` stránky místo na přihlašovací stránku. Logika, která přesměruje neoprávněné požadavky na přihlašovací stránku je schovaný v rámci privátní metoda `FormsAuthenticationModule` třídy, takže jsme nemůže upravit toto chování. Co můžeme udělat, ale je přidat vlastní logiku na přihlašovací stránku, který přesměruje uživatele na `UnauthorizedAccess.aspx`, v případě potřeby.

Když `FormsAuthenticationModule` neoprávněné návštěvníka přesměruje na přihlašovací stránku přidá požadovaná, neoprávněným adresa URL řetězec dotazu s názvem `ReturnUrl`. Například při pokusu o neoprávněný uživatel navštívit `OnlyTito.aspx`, `FormsAuthenticationModule` by přesměrovat je na `Login.aspx?ReturnUrl=OnlyTito.aspx`. Proto když se dosáhne přihlašovací stránku od ověřeného uživatele pomocí řetězce dotazu, který obsahuje `ReturnUrl` parametr a potom budeme vědět, že tento neověřený uživatel se pokusil pouze na stránce uživatel nemá oprávnění k zobrazení. V takovém případě se chceme přesměrování jí `UnauthorizedAccess.aspx`.

Chcete-li to provést, přidejte následující kód na přihlašovací stránku `Page_Load` obslužné rutiny události:

[!code-csharp[Main](user-based-authorization-cs/samples/sample8.cs)]

Výše uvedený kód přesměruje ověřený, neoprávněným uživatelům `UnauthorizedAccess.aspx` stránky. Pokud chcete zobrazit tuto logiku v akci, přejděte na web jako anonymní návštěvníka a klikněte na odkaz vytváření uživatelských účtů v levém sloupci. Tím přejdete na `~/Membership/CreatingUserAccounts.aspx` stránky, která v kroku 1 jsme nakonfigurovali pouze povolit přístup k Tito. Protože jsou zakázány anonymním uživatelům, `FormsAuthenticationModule` nám přesměruje zpátky na přihlašovací stránku.

V tuto chvíli jsme jsou anonymní, takže `Request.IsAuthenticated` vrátí `false` a My se přesměruje na `UnauthorizedAccess.aspx`. Místo toho se zobrazí přihlašovací stránku. Přihlaste se pod jiným než Tito, jako je například Bruce. Po zadání příslušných přihlašovacích údajů, přihlašovací stránce přesměruje nám zpět na `~/Membership/CreatingUserAccounts.aspx`. Ale vzhledem k tomu, že tato stránka je přístupná pouze pro Tito, jsme jsou neoprávněný přístup k jeho zobrazení a o tom bezodkladně informuje přesměrováni na přihlašovací stránku. Tentokrát ale `Request.IsAuthenticated` vrátí `true` (a `ReturnUrl` parametr querystring existuje), takže jsme se přesměrují na `UnauthorizedAccess.aspx` stránky.


[![Ověření, neoprávněným uživatelům se přesměrují na UnauthorizedAccess.aspx](user-based-authorization-cs/_static/image17.png)](user-based-authorization-cs/_static/image16.png)

**Obrázek 6**: ověření, neoprávněným uživatelům se přesměrují na `UnauthorizedAccess.aspx` ([kliknutím ji zobrazíte obrázek v plné velikosti](user-based-authorization-cs/_static/image18.png))


Tato vlastní pracovní postup představuje rozumné a jednoduché uživatelské prostředí podle krátký circuiting cyklu znázorněno na obrázku 2.

## <a name="step-3-limiting-functionality-based-on-the-currently-logged-in-user"></a>Krok 3: Omezení funkcí podle aktuálně přihlášeného uživatele

Autorizace adres URL umožňuje snadno určit hrubý autorizačních pravidel. Jak jsme viděli v kroku 1, s autorizace adres URL jsme můžete stručně stavu identit, které jsou povolené a které z nich byl odepřen v zobrazení konkrétní stránky nebo všech stránek ve složce. V některých případech však může Chceme všem uživatelům najdete na stránce, ale omezit na stránce funkce na základě uživatele na jeho.

Podívejte se na webu elektronického obchodování, která umožňuje ověřeným návštěvníkům Zkontrolujte své produkty. Anonymní uživatel navštíví stránku konkrétního produktu, uvidí jenom informace o produktu a nebude mít možnost ponechat kontrolu. Ověřený uživatel na stejné stránce by ale uvidí revize rozhraní. Pokud ověřený uživatel nebyl zatím Nezkontrolováno tohoto produktu, rozhraní by jim povolit odešlete recenzi; v opačném případě se zobrazí jejich jejich kontrola byla odeslána. Abyste mohli tento scénář krok dále, na stránce produktu může zobrazit další informace a nabízí rozšířené funkce pro uživatele, kteří pracují pro elektronické obchodování společnosti. Na stránce produktu může být například seznam inventáře v zásobách a zahrnují možnost Upravit ceny a popis, když uživatel zaměstnanec produktu.

Takové nich spočívá v jemné autorizační pravidla je možné implementovat buď deklarativně nebo prostřednictvím kódu programu (nebo pomocí kombinace obou). V další části uvidíme, jak implementovat nich spočívá v jemné autorizace pomocí ovládacího prvku LoginView. Pod se podíváme na programový techniky. Předtím, než abychom se mohli podívat na použití nich spočívá v jemné autorizační pravidla, ale nejprve musíme vytvořit stránku, jehož funkce závisí na uživatele na jeho.

Pojďme vytvořit stránku, která obsahuje soubory v určitém adresáři v rámci GridView. Spolu s výpisem název každého souboru, velikost a další informace, prvku GridView bude obsahovat dva sloupce LinkButtons: jednu s názvem zobrazení a jednu s názvem odstranit. Pokud dojde ke kliknutí na prvek LinkButton zobrazení, zobrazí se obsah na vybraný soubor; Pokud dojde ke kliknutí na prvek LinkButton odstranit, soubor se odstraní. Nejprve vytvoříme tuto stránku tak, aby jeho zobrazení a odstraněných funkcí je k dispozici všem uživatelům. V použití části ovládacího prvku LoginView a omezení funkce prostřednictvím kódu programu, uvidíme, jak povolit nebo zakázat tyto funkce na základě uživatele na stránce.

> [!NOTE]
> Stránky ASP.NET, které se chystáte vytvořit používá k zobrazení seznamu souborů ovládacího prvku GridView. Od tohoto kurzu, kterou řada se zaměřuje na ověřování pomocí formulářů, autorizace, uživatelských účtů a rolí nechci strávit příliš mnoho času diskuze o vnitřní fungování ovládacího prvku GridView. Tento kurz poskytuje konkrétní podrobné pokyny pro nastavení této stránky, ne delve podrobnosti, proč byly provedeny některé možnosti, nebo konkrétní vlastnosti vliv mají na vykresleného výstupu. Důkladné přezkoumání ovládacího prvku GridView, poraďte Moje *[pracovat s daty v ASP.NET 2.0](../../data-access/index.md)* série kurzů.


Začněte otevřením `UserBasedAuthorization.aspx` soubor `Membership` složky a přidání ovládacího prvku GridView na stránku s názvem `FilesGrid`. Z prvku GridView inteligentní značky klikněte na odkaz Upravit sloupce spustit dialogové okno pole. Z tohoto místa, zrušte zaškrtnutí políčka automaticky generovat pole v levém dolním rohu. Dále přidejte tlačítko pro výběr, tlačítko pro odstranění a dva BoundFields z levého horního rohu (tlačítka pro výběr a odstranění najdete v části Typ CommandField). Vyberte tlačítko nastavit `SelectText` vlastnosti k zobrazení a první vlastnost BoundField `HeaderText` a `DataField` vlastnosti Name. Nastavit druhý Vlastnost BoundField `HeaderText` vlastnost na velikost v bajtech, jeho `DataField` vlastnost Length, jeho `DataFormatString` vlastnost {0:N0} a jeho `HtmlEncode` vlastnost na hodnotu False.

Po dokončení konfigurace sloupce prvku GridView, kliknutím na OK zavřete dialogové okno pole. V okně Vlastnosti nastavte prvku GridView `DataKeyNames` vlastnost `FullName`. V tuto chvíli deklarativním označení prvku GridView, by měl vypadat nějak takto:

[!code-aspx[Main](user-based-authorization-cs/samples/sample9.aspx)]

Pomocí vytvoření značky prvku GridView jsme připraveni napsat kód, který načte soubory v určitém adresáři a svázat ho s prvku GridView. Přidejte následující kód na stránku `Page_Load` obslužné rutiny události:

[!code-csharp[Main](user-based-authorization-cs/samples/sample10.cs)]

Výše uvedený kód používá [ `DirectoryInfo` třídy](https://msdn.microsoft.com/library/system.io.directoryinfo.aspx) získat seznam souborů v kořenové složce vaší aplikace. [ `GetFiles()` Metoda](https://msdn.microsoft.com/library/system.io.directoryinfo.getfiles.aspx) vrátí všechny soubory v adresáři jako pole [ `FileInfo` objekty](https://msdn.microsoft.com/library/system.io.fileinfo.aspx), který je pak vázán na prvku GridView. `FileInfo` Objekt má celé řady různých doprovodných vlastností, jako například `Name`, `Length`, a `IsReadOnly`, mimo jiné. Jak je vidět z jeho deklarativním označení prvku GridView zobrazí pouze `Name` a `Length` vlastnosti.

> [!NOTE]
> `DirectoryInfo` a `FileInfo` třídy se nacházejí v [ `System.IO` obor názvů](https://msdn.microsoft.com/library/system.io.aspx). Proto bude buď nutnosti začínat tyto názvy tříd s jejich názvy oborů názvů nebo obor názvů importovat do souboru třídy (prostřednictvím `using System.IO`).


Za chvíli tuto stránku prostřednictvím prohlížeče. Zobrazí seznam souborů, které se nacházejí v kořenovém adresáři aplikace. Kliknutím na Zobrazit nebo odstranit LinkButtons způsobí zpětné volání, ale nic se dojít, protože jsme dosud k vytvoření obslužné rutiny událostí nezbytné.


[![GridView Vypíše soubory v kořenovém adresáři webové aplikace](user-based-authorization-cs/_static/image20.png)](user-based-authorization-cs/_static/image19.png)

**Obrázek 7**: prvku GridView Vypíše soubory v kořenovém adresáři webové aplikace ([kliknutím ji zobrazíte obrázek v plné velikosti](user-based-authorization-cs/_static/image21.png))


Potřebujeme způsob zobrazení obsahu na vybraný soubor. Vraťte se do sady Visual Studio a přidejte textové pole s názvem `FileContents` nad prvku GridView. Nastavte jeho `TextMode` vlastnost `MultiLine` a jeho `Columns` a `Rows` vlastností 95 % a 10, v uvedeném pořadí.

[!code-aspx[Main](user-based-authorization-cs/samples/sample11.aspx)]

Dále vytvořte obslužnou rutinu události pro prvku GridView [ `SelectedIndexChanged` události](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindexchanged.aspx) a přidejte následující kód:

[!code-csharp[Main](user-based-authorization-cs/samples/sample12.cs)]

Tento kód používá prvku GridView `SelectedValue` a určí název úplný soubor vybraný soubor. Interně jsou `DataKeys` kolekce se odkazuje, aby bylo možné získat `SelectedValue`, takže je nutné nastavit prvku GridView `DataKeyNames` vlastnost na název, jak je popsáno dříve v tomto kroku. [ `File` Třídy](https://msdn.microsoft.com/library/system.io.file.aspx) slouží k načtení obsahu vybraný soubor do řetězce, který je poté přiřazen `FileContents` textového `Text` vlastnost, a tím zobrazení obsahu vybraný soubor na stránce.


[![Soubor vybraný obsah se zobrazí v textovém poli](user-based-authorization-cs/_static/image23.png)](user-based-authorization-cs/_static/image22.png)

**Obrázek 8**: vybraný soubor na obsah se zobrazí v textovém poli ([kliknutím ji zobrazíte obrázek v plné velikosti](user-based-authorization-cs/_static/image24.png))


> [!NOTE]
> Je-li zobrazit obsah souboru, který obsahuje kód HTML a pak se pokusíte zobrazit nebo odstranit soubor, dostanete `HttpRequestValidationException` chyby. K tomu dochází, protože na zpětné obsah v textovém poli odesílají zpět do webového serveru. Ve výchozím nastavení, vyvolá ASP.NET `HttpRequestValidationException` chyba pokaždé, když se zjistí potenciálně nebezpečný obsah v zpětného volání, jako je značka jazyka HTML. Chcete-li zakázat výskytu této chyby, vypněte ověření žádosti pro stránku tak, že přidáte `ValidateRequest="false"` k `@Page` směrnice. Další informace o výhodách ověření žádosti jako i jaká opatření byste měli podniknout při zakázání, najdete v článku [ověření požadavku – prevence útoků skript](https://asp.net/learn/whitepapers/request-validation/).


Nakonec přidejte obslužnou rutinu události s následujícím kódem pro prvku GridView [ `RowDeleting` události](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx):

[!code-csharp[Main](user-based-authorization-cs/samples/sample13.cs)]

Kód jednoduše zobrazí celý název souboru k odstranění ve `FileContents` TextBox *bez* skutečného odstranění souboru.


[![Kliknutím na tlačítko Odstranit neodstraní ve skutečnosti soubor](user-based-authorization-cs/_static/image26.png)](user-based-authorization-cs/_static/image25.png)

**Obrázek 9**: Kliknutím odstranit tlačítko ve skutečnosti neodstraní na soubor ([kliknutím ji zobrazíte obrázek v plné velikosti](user-based-authorization-cs/_static/image27.png))


V kroku 1 jsme nakonfigurovali zakázat anonymní uživatelé ze zobrazení stránek v autorizačních pravidel adres URL `Membership` složky. Aby bylo možné lépe vykazovat nich spočívá v jemné ověřování, můžeme povolit anonymní uživatelé přejdete `UserBasedAuthorization.aspx` stránky, ale s omezenou funkčností. Otevřete tuto stránku Pokud chcete mít přístup všichni uživatelé, přidejte následující `<location>` elementu `Web.config` soubor `Membership` složky:

[!code-xml[Main](user-based-authorization-cs/samples/sample14.xml)]

Po přidání tohoto `<location>` elementu, otestujte nový autorizačních pravidel adres URL protokolování mimo lokalitu. Jako anonymní uživatel by měl povolena navštívit `UserBasedAuthorization.aspx` stránky.

V současné době můžete navštívit každý ověřený nebo anonymní uživatel `UserBasedAuthorization.aspx` stránce a zobrazovat a odstraňovat soubory. Tak, aby pouze ověřeným uživatelům může zobrazit obsah souboru a soubor lze odstranit pouze Tito ho vytvoříme. Tato pravidla autorizace nich spočívá v jemné lze použít deklarativně, prostřednictvím kódu programu nebo kombinací obou metod. S použitím deklarativní přístup omezit, kdo může zobrazit obsah souboru použijeme programový přístup k omezení toho, kdo může odstranit soubor.

### <a name="using-the-loginview-control"></a>Použití ovládacího prvku LoginView

Jak jsme viděli v posledních kurzy, ovládacího prvku LoginView je užitečné pro zobrazení různých rozhraní pro uživatele ověřeného a anonymní a nabízí snadný způsob, jak skrýt funkce, které nejsou dostupné pro anonymní uživatele. Protože anonymní uživatelé nemohou zobrazit nebo odstranit soubory, potřebujeme jenom zobrazit `FileContents` textového pole při návštěvě stránky ověřeného uživatele. K dosažení tohoto cíle, přidejte ovládací prvek zobrazení přihlášení na stránku, pojmenujte ho `LoginViewForFileContentsTextBox`a přesunout `FileContents` textového deklarativní do ovládacího prvku LoginView `LoggedInTemplate`.

[!code-aspx[Main](user-based-authorization-cs/samples/sample15.aspx)]

Webové ovládací prvky v prvku LoginView šablony už nejsou přímo dostupné z třídy modelu code-behind. Například `FilesGrid` prvku GridView `SelectedIndexChanged` a `RowDeleting` obslužné rutiny událostí aktuálně odkazují `FileContents` ovládací prvek TextBox s kódem, jako jsou:

[!code-csharp[Main](user-based-authorization-cs/samples/sample16.cs)]

Tento kód je však již nebude platný. Přechodem `FileContents` textového pole do `LoggedInTemplate` textového pole nelze přistupovat přímo. Místo toho, musíme použít `FindControl("controlId")` metoda prostřednictvím kódu programu odkazovat na ovládací prvek. Aktualizace `FilesGrid` obslužných rutin událostí k odkazování textové pole takto:

[!code-csharp[Main](user-based-authorization-cs/samples/sample17.cs)]

Po přesunutí textového pole LoginView `LoggedInTemplate` a aktualizaci na stránku kódu do textového pole pomocí odkazu `FindControl("controlId")` vzorku naleznete na stránce jako anonymní uživatel. Obrázek 10 ukazuje, `FileContents` textové pole nezobrazí. Na prvek LinkButton zobrazení je však stále zobrazen.


[![Ovládacího prvku LoginView pouze vykresluje textové pole FileContents ověřených uživatelů](user-based-authorization-cs/_static/image29.png)](user-based-authorization-cs/_static/image28.png)

**Obrázek 10**: The LoginView pouze vykresluje `FileContents` textové pole pro ověřené uživatele ([kliknutím ji zobrazíte obrázek v plné velikosti](user-based-authorization-cs/_static/image30.png))


Jeden způsob, jak skrýt tlačítko pro anonymní uživatele je převést na pole TemplateField pole ovládacího prvku GridView. Tím se vygeneruje šablonu, která obsahuje deklarativní LinkButton zobrazení. My pak přidání ovládacího prvku LoginView pole TemplateField a umístěte odkazem (LinkButton) v rámci prvku LoginView `LoggedInTemplate`, a tím skrytí tlačítka zobrazit z anonymního návštěvníků. K tomu, klikněte na odkaz Upravit sloupce v prvku GridView inteligentních značek ke spuštění dialogové okno pole. V dalším kroku vyberte tlačítko pro výběr v seznamu v levém dolním rohu a klikněte na převést toto pole TemplateField propojení. Tím upraví deklarativní pole od:

[!code-aspx[Main](user-based-authorization-cs/samples/sample18.aspx)]

 Do: 

[!code-aspx[Main](user-based-authorization-cs/samples/sample19.aspx)]

V tuto chvíli můžeme přidat zobrazení přihlášení pole TemplateField. Následující kód zobrazí zobrazení odkazem (LinkButton) pouze pro ověřené uživatele.

[!code-aspx[Main](user-based-authorization-cs/samples/sample20.aspx)]

Jak ukazuje obrázek 11, není konečný výsledek, že poměrně jako zobrazení sloupce se stále zobrazí i v případě, že jsou skryté LinkButtons zobrazení ve sloupci. Podíváme se na tom, jak skrýt celý sloupec ovládacího prvku GridView (a ne jenom na prvek LinkButton) v další části.


[![Ovládacího prvku LoginView skryje LinkButtons zobrazení pro anonymní uživatelé](user-based-authorization-cs/_static/image32.png)](user-based-authorization-cs/_static/image31.png)

**Obrázek 11**: ovládacího prvku LoginView skryje LinkButtons zobrazení pro anonymní uživatelé ([kliknutím ji zobrazíte obrázek v plné velikosti](user-based-authorization-cs/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>Programová omezení funkcí

V některých případech deklarativní techniky nedostačují k omezení funkcí na stránku. Dostupnost funkcí některé stránky může být například závislé na kritériích nad rámec, jestli je uživatel na stránce ověřený nebo anonymní. V takových případech můžete zobrazení nebo skrytí programové způsoby různé prvky uživatelského rozhraní.

Aby bylo možné omezit funkčnost prostřednictvím kódu programu, musíme provést dvě úlohy:

1. Určení, zda má uživatel na stránce přístup je funkce, a
2. Programová změna uživatelské rozhraní založené na tom, jestli má uživatel přístup k funkci dotyčný.

Abychom si předvedli aplikace tyto dvě úlohy, jenom povolíme Tito odstranit soubory z prvku GridView. Naše první úkol, je zjistit, jestli je na stránce Tito. Po zjištění, které potřebujeme pro skrytí (nebo zobrazení) odstranit sloupec prvku GridView. Sloupce prvku GridView jsou přístupné prostřednictvím jeho `Columns` vlastnost; sloupec je vykreslen pouze, pokud jeho `Visible` je nastavena na `true` (výchozí).

Přidejte následující kód, který `Page_Load` obslužná rutina události před vazbě dat na prvku GridView:

[!code-csharp[Main](user-based-authorization-cs/samples/sample21.cs)]

Jak jsme probírali v [ *Přehled ověřování založené na formulářích* ](../introduction/an-overview-of-forms-authentication-cs.md) kurzu `User.Identity.Name` vrátí název identity. To odpovídá zadané v ovládacím prvku přihlašovací uživatelské jméno. Pokud se Tito navštívit stránky, druhý sloupec prvku GridView `Visible` je nastavena na `true`; v opačném případě je nastavený na `false`. Net výsledkem je, že pokud někdo jiný než Tito navštíví stránku, jiný uživatel ověřený nebo anonymní uživatel, odstranit sloupec není vykreslen (viz obrázek 12); Pokud však Tito navštíví stránku, odstranit sloupec je k dispozici (viz obrázek 13).


[![Odstranit sloupec není vykresleno při navštívené podle někdo jiný než Tito (například Bruce)](user-based-authorization-cs/_static/image35.png)](user-based-authorization-cs/_static/image34.png)

**Obrázek 12**: odstranit sloupec není vykresleno při navštívené podle někdo jiný než Tito (například Bruce) ([kliknutím ji zobrazíte obrázek v plné velikosti](user-based-authorization-cs/_static/image36.png))


[![Odstranit sloupec je vykreslen pro Tito](user-based-authorization-cs/_static/image38.png)](user-based-authorization-cs/_static/image37.png)

**Obrázek 13**: odstranit sloupec je vykreslen pro Tito ([kliknutím ji zobrazíte obrázek v plné velikosti](user-based-authorization-cs/_static/image39.png))


## <a name="step-4-applying-authorization-rules-to-classes-and-methods"></a>Krok 4: Použití autorizačních pravidel pro třídy a metody

V kroku 3 jsme zakázán anonymní uživatelé ze zobrazení obsahu souboru zpřístupníme a všichni uživatelé, ale Tito odstraňování souborů. Toho dosáhlo tím, že skryjete prvky přidružené uživatelského rozhraní pro neoprávněné návštěvníků prostřednictvím deklarativní a programová techniky. Náš jednoduchý příklad správně skrytí prvky uživatelského rozhraní bylo jednoznačné, ale co složitější lokality tam, kde mohou existovat mnoho různých způsobů, jak provádět stejné funkce? V omezení, které tuto funkci neoprávněným uživatelům, co se stane, pokud jsme zapomněli pro skrytí nebo zakázat všechny prvky odpovídající uživatelské rozhraní?

Snadný způsob, jak zajistit, že konkrétní funkce nelze přistupovat jakýkoli neoprávněný uživatel je k vyplnění této třídy nebo metody pomocí [ `PrincipalPermission` atribut](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Pokud modul .NET runtime používá třídu nebo spustí jednu z jeho metod, zkontroluje, ujistěte se, že aktuální kontext zabezpečení má oprávnění k použití třídy nebo proveďte metodu. `PrincipalPermission` Atribut poskytuje mechanismus, pomocí kterého definujeme tato pravidla.

Přiblížíme pomocí `PrincipalPermission` atribut v prvku GridView `SelectedIndexChanged` a `RowDeleting` obslužných rutin událostí k zakázat spouštění anonymní uživatelé a uživatelé než Tito, v uvedeném pořadí. Všechno, co musíme udělat, je přidání odpovídajícího atributu imitovaná každá definice funkce:

[!code-csharp[Main](user-based-authorization-cs/samples/sample22.cs)]

Atribut pro `SelectedIndexChanged` určují obslužné rutiny událostí, které jenom ověření uživatelé můžete spustit obslužnou rutinu události, kde jako atribut `RowDeleting` tak provedení Tito omezuje obslužné rutiny události.

Pokud nějakým způsobem, jiným uživatelem, než Tito pokusy o spuštění `RowDeleting` obslužná rutina události nebo jiné ověřený uživatel pokusí spustit `SelectedIndexChanged` obslužná rutina události, vyvolá se modul .NET runtime `SecurityException`.


[![Pokud je kontext zabezpečení nemá oprávnění k provádění metody, je vyvolána SecurityException –](user-based-authorization-cs/_static/image41.png)](user-based-authorization-cs/_static/image40.png)

**Obrázek 14**: Pokud kontext zabezpečení nemá oprávnění k provádění metody, `SecurityException` je vyvolána výjimka ([kliknutím ji zobrazíte obrázek v plné velikosti](user-based-authorization-cs/_static/image42.png))


> [!NOTE]
> Pokud chcete povolit více kontexty zabezpečení pro přístup k tříd nebo metod, uspořádání třídy nebo metody pomocí `PrincipalPermission` atribut pro každý kontext zabezpečení. To znamená aby Tito a Bruce spustit `RowDeleting` obslužná rutina události, přidejte *dvě* `PrincipalPermission` atributy:


[!code-csharp[Main](user-based-authorization-cs/samples/sample23.cs)]

Kromě stránek ASP.NET mají mnoho aplikací také architekturu, která zahrnuje různé vrstvy, jako je například obchodní logiky a vrstvy přístupu k datům. Tyto vrstvy jsou obvykle implementovány jako knihovny tříd a nabízejí třídy a metody pro provádění obchodní logiku a data související funkce. `PrincipalPermission` Atribut je užitečné pro použití autorizačních pravidel pro tyto vrstvy.

Další informace o používání `PrincipalPermission` atribut pro definování pravidel autorizace na třídy a metody, přečtěte si [Scott Guthrie](https://weblogs.asp.net/scottgu/)na blogu [přidáním autorizačních pravidel a obchodních dat pomocí vrstvy `PrincipalPermissionAttributes` ](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Souhrn

V tomto kurzu jsme se podívali na tom, jak aplikovat autorizační pravidla založená na uživatele. Začali jsme s podívat, ASP. Na .NET framework autorizace adresy URL. V každé žádosti, modul ASP.NET `UrlAuthorizationModule` zkontroluje autorizačních pravidel adres URL definované v konfiguraci vaší aplikace k určení, zda je oprávněn identitu pro přístup k požadovanému prostředku. Stručně řečeno autorizace adres URL umožňuje snadno určit autorizační pravidla pro konkrétní stránky, nebo pro všechny stránky v určitém adresáři.

Rozhraní autorizace adresy URL použije pravidla autorizace na základě stránku po stránce. S autorizace adres URL je žádost o identitu oprávnění pro přístup k určitému prostředku nebo ne. Mnoho scénářů, ale volání pro další nich spočívá v jemné autorizačních pravidel. Místo definovat, kdo smí získat přístup na stránku, možná potřebujeme umožňuje všem uživatelům přístup ke stránce, ale zobrazení jiných dat nebo nabízejí různé funkce v závislosti na uživatele na stránce. Autorizaci na úrovni stránky obvykle zahrnuje, aby bylo možné zabránit neoprávněným uživatelům v přístupu k zakázané funkce skrytí prvky konkrétní uživatelského rozhraní. Kromě toho je možné použít atributy k omezení přístupu k třídy a spuštění z jeho metod pro určité uživatele.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Přidání obchodní a datové vrstvy pomocí autorizačních pravidel `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Povolení technologie ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Změny mezi IIS6 a zabezpečení služby IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Konfigurace určité soubory a podadresáře](https://msdn.microsoft.com/library/6hbkh9s7.aspx)
- [Omezení funkcí pro úpravu dat podle uživatele](../../data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs.md)
- [Šablony rychlý start ovládací prvek zobrazení přihlášení](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/loginview.aspx)
- [Principy autorizace adres URL služby IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)
- [`UrlAuthorizationModule` Technická dokumentace](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)
- [Práce s daty v technologii ASP.NET 2.0](../../data-access/index.md)

### <a name="about-the-author"></a>O autorovi

Scott Meisnerová, Autor více ASP/ASP.NET knih a zakladatelem 4GuysFromRolla.com, má pracovali Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy  *[Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott může být dostupný na adrese [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) nebo prostřednictvím na svém blogu [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Předchozí](validating-user-credentials-against-the-membership-user-store-cs.md)
> [další](storing-additional-user-information-cs.md)
