---
uid: web-forms/overview/older-versions-security/membership/user-based-authorization-cs
title: Ověření na základě uživatele (C#) | Microsoft Docs
author: rick-anderson
description: V tomto kurzu se podíváme na omezení přístupu na stránky a omezení funkce úrovně stránky prostřednictvím řady různých způsobů.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 3c815a9e-2296-4b9b-b945-776d54989daa
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/user-based-authorization-cs
msc.type: authoredcontent
ms.openlocfilehash: 9a0d476ffaf1f176c21b245520fa943f66e8c0d5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/10/2018
ms.locfileid: "30891916"
---
<a name="user-based-authorization-c"></a>Ověření na základě uživatele (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_07_CS.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial07_UserAuth_cs.pdf)

> V tomto kurzu se podíváme na omezení přístupu na stránky a omezení funkce úrovně stránky prostřednictvím řady různých způsobů.


## <a name="introduction"></a>Úvod

Většina webových aplikací, které nabízejí uživatelské účty tak učinit v rámci omezit určité návštěvníky přístup na určité stránky v rámci lokality. V weby messageboard nejvíce online například všechny uživatele – anonymní a ověřené - dokážou zobrazit příspěvcích messageboard, ale pouze ověření uživatelé navštívit webovou stránku pro vytvoření nového příspěvku. A může být stránky pro správu, které jsou přístupné pro konkrétního uživatele (nebo konkrétní sadu uživatelů). Kromě toho funkce úrovně stránky se může lišit na základě uživatele uživatelů. Při zobrazení seznamu příspěvky, ověřené uživatele zobrazí rozhraní pro hodnocení každého post, zatímco toto rozhraní není k dispozici pro anonymní návštěvníky.

Technologie ASP.NET umožňuje snadno definovat autorizační pravidla založená na uživatele. S jenom kousek značek v `Web.config`, konkrétní webové stránky nebo celou adresáře může být uzamčena, tak, aby pouze jsou přístupné pro zadaný podmnožině uživatelů. Funkce na úrovni stránky může být zapnout nebo vypnout, podle aktuálně přihlášeného uživatele prostřednictvím programová a deklarativní prostředky.

V tomto kurzu se podíváme na omezení přístupu na stránky a omezení funkce úrovně stránky prostřednictvím řady různých způsobů. Můžeme začít!

## <a name="a-look-at-the-url-authorization-workflow"></a>Podívejte se na pracovním postupu autorizace adresy URL

Jak je popsáno v [ *Přehled ověřování založené na formulářích* ](../introduction/an-overview-of-forms-authentication-cs.md) kurzu, když modulem runtime ASP.NET zpracovává žádost pro prostředek ASP.NET požadavku vyvolá určitý počet událostí během životního cyklu. *Vytváření modulů HTTP v* jsou spravované třídy, jejichž kód se spustí v reakci na určité události v průběhu životního cyklu požadavku. ASP.NET se dodává s počtem modulů HTTP, který provádění základních úloh na pozadí.

Jeden takový modul HTTP je [ `FormsAuthenticationModule` ](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx). Jak je popsáno v předchozí kurzy, primárních funkcí `FormsAuthenticationModule` je určit identitu aktuální žádosti. To se provádí kontroly lístek ověřování pomocí formulářů, který se buď nachází v souboru cookie, nebo vložené v rámci adresy URL. Toto identifikační probíhá během [ `AuthenticateRequest` událostí](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Bude další důležité modul HTTP [ `UrlAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx), která se vyvolá v reakci na [ `AuthorizeRequest` událostí](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx) (který se stane po `AuthenticateRequest` událostí). `UrlAuthorizationModule` Prozkoumá značek konfigurace v `Web.config` k určení, zda má aktuální identitu oprávněni naleznete na stránce zadaný. Tento proces se označuje jako *autorizace adres URL*.

Podíváme syntaxe autorizačních pravidel adres URL v kroku 1, ale nejdřív umožňuje podívejte se na co `UrlAuthorizationModule` nemá v závislosti na tom, zda je požadavek autorizován nebo ne. Pokud `UrlAuthorizationModule` zjistí, že je požadavek autorizován, pak se neprovede žádnou akci a požadavek pokračuje prostřednictvím životního cyklu. Ale je-li žádost *není* oprávnění, pak se `UrlAuthorizationModule` zruší životní cyklus a dává pokyn `Response` objekt, který chcete vrátit [HTTP 401 neoprávněný](http://www.checkupdown.com/status/E401.html) stav. Při použití ověřování pomocí formulářů tento stav HTTP 401 je nikdy vrácen do klienta vzhledem k tomu, pokud `FormsAuthenticationModule` zjistí HTTP 401 je stav změní jeho [přesměrování HTTP 302](http://www.checkupdown.com/status/E302.html) na přihlašovací stránku.

Obrázek 1 ukazuje pracovní postup kanálu ASP.NET `FormsAuthenticationModule`a `UrlAuthorizationModule` když dorazí neoprávněného požadavku. Konkrétně obrázek 1 zobrazuje žádost o anonymní návštěvníkem pro `ProtectedPage.aspx`, což je stránka, která odepře přístup pro anonymní uživatele. Vzhledem k tomu, že je anonymní, návštěvníka `UrlAuthorizationModule` zruší požadavek a vrátí protokolu HTTP 401 Unauthorized status. `FormsAuthenticationModule` Potom převede stavu 401 přesměrováním 302 na přihlašovací stránku. Po ověření uživatele prostřednictvím přihlašovací stránky, mohl se přesměruje na `ProtectedPage.aspx`. Tato doba `FormsAuthenticationModule` identifikuje uživatele na základě jeho lístku ověřování. Teď, když ověření návštěvníka `UrlAuthorizationModule` umožňuje přístup na stránku.


[![Ověřování pomocí formulářů a ověřování adresy URL pracovního postupu](user-based-authorization-cs/_static/image2.png)](user-based-authorization-cs/_static/image1.png)

**Obrázek 1**: ověřování pomocí formulářů a pracovní postup autorizace adresy URL ([Kliknutím zobrazit obrázek v plné velikosti](user-based-authorization-cs/_static/image3.png))


Obrázek 1 znázorňuje interakci, ke kterému dochází, když se pokusí získat přístup k prostředku, který není k dispozici anonymním uživatelům anonymní návštěvníka. V takovém případě se anonymní návštěvníka přesměruje na přihlašovací stránku s stránku, kterou uživatel se pokusil navštíví zadaný v řetězci dotazu. Po úspěšném přihlášení uživatele se automaticky přesměrováni zpět na prostředek, který se původně při pokusu o zobrazení.

Při neoprávněného požadavku anonymního uživatele, tento pracovní postup je jednoduchý a snadno pro návštěvníka pochopit, co se stalo a proč. Ale mějte na paměti, že `FormsAuthenticationModule` přesměruje *žádné* neoprávněného uživatele na přihlašovací stránku, i v případě požadavku ověřený uživatel. To může způsobit matoucí činnost koncového uživatele Pokud ověřený uživatel se pokusí najdete na stránce, pro které Jana chybí autoritu.

Představte si, že náš web měl jeho autorizačních pravidel adres URL nakonfigurována tak, že stránka ASP.NET `OnlyTito.aspx` bylo accessibly pouze Tito. Nyní, představte si Sam navštíví web, přihlášení a poté se pokusí navštívit `OnlyTito.aspx`. `UrlAuthorizationModule` Bude životního cyklu žádost o zastavení a vrátit HTTP 401 Unauthorized status, který `FormsAuthenticationModule` rozpozná a pak přesměruje na přihlašovací stránku Sam. Vzhledem k tomu, že již byl přihlášen Sam, ale uživatel může zajímat, proč se byl odeslán zpět na stránku přihlášení. Uživatel může důvodu, že jeho přihlašovací údaje byly ztraceny nějakým způsobem, nebo že uživatel zadá neplatné přihlašovací údaje. Pokud Sam reenters svoje přihlašovací údaje z přihlašovací stránky se bude přihlášený (znovu) a přesměrování na `OnlyTito.aspx`. `UrlAuthorizationModule` Zjistí, že Sam nelze najdete na této stránce a uživatel bude vrácen na přihlašovací stránku.

Obrázek 2 znázorňuje tento pracovní postup matoucí.


[![Výchozí pracovní postup může vést k cyklus matoucí](user-based-authorization-cs/_static/image5.png)](user-based-authorization-cs/_static/image4.png)

**Obrázek 2**: výchozí pracovní postup může vést k cyklus složitá ([Kliknutím zobrazit obrázek v plné velikosti](user-based-authorization-cs/_static/image6.png))


Pracovní postup vidíte na obrázku 2 můžete rychle befuddle i většině počítače koumák návštěvníka. Podíváme se na způsoby, jak tomu zabránit, složitá cyklu v kroku 2.

> [!NOTE]
> ASP.NET používá k určení, zda má aktuální uživatel přístup konkrétní webové stránky dvou mechanismů: ověřování adresy URL a souborů. Soubor autorizace je implementováno modulem [ `FileAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx), která určuje autoritu podle konzultace ohledně požadované soubory seznamy ACL. Soubor autorizace se nejčastěji používá pomocí ověřování systému Windows, protože seznamy ACL jsou oprávnění, které se vztahují na účty systému Windows. Pokud používáte ověřování pomocí formulářů, jsou všechny požadavky na úrovni systému operačního systému a soubor provedený stejný účet systému Windows, bez ohledu na uživatele, kteří navštěvují webu. Vzhledem k tomu, že tato řada kurz se zaměřuje na ověřování pomocí formulářů, jsme nebude možné hovoříte o souboru autorizace.


### <a name="the-scope-of-url-authorization"></a>Rozsah autorizace adres URL

`UrlAuthorizationModule` Je spravovaný kód, který je součástí modulu runtime ASP.NET. Starší než verze 7 společnosti Microsoft [Internetové informační služby (IIS)](https://www.iis.net/) webový server byl odlišné bariéry mezi kanál protokolu HTTP služby IIS a modulu runtime ASP.NET kanálu. V krátké, služby IIS 6 a starší, ASP. Na NET `UrlAuthorizationModule` pouze provede, když se žádost o deleguje ze služby IIS na modulem runtime ASP.NET. Ve výchozím nastavení, služba IIS zpracovává statický obsah, samotné – například stránky HTML a CSS, JavaScript a soubory obrázků - a pouze předá požadavky na modulem runtime ASP.NET, když se zobrazí stránka s příponou `.aspx`, `.asmx`, nebo `.ashx` se požaduje.

Integrované služby IIS a ASP.NET IIS 7, ale umožňuje kanály. S několik nastavení konfigurace se dá nastavit službu IIS 7 k vyvolání `UrlAuthorizationModule` pro *všechny* požadavky, což znamená, že lze definovat autorizačních pravidel adres URL pro souborů všech typů. Kromě toho IIS 7 zahrnuje modul autorizace vlastní adresu URL. Další informace o integraci technologie ASP.NET a IIS 7 nativních funkcí autorizace adresy URL, najdete v části [autorizace adres URL pro službu IIS7 Principy](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization). Pro více hlubší pohled na integraci technologie ASP.NET a IIS 7, nevybere kopii adresáře Shahram Khosravi *Professional IIS 7 a ASP.NET integrované programování* (ISBN: 978 0470152539).

Stručně řečeno v verze starší než IIS 7, autorizačních pravidel adres URL platí pouze pro prostředky, které jsou zpracovávány modulem ASP.NET runtime. Se službou IIS 7 je však možné používat funkce služby IIS nativní adresy URL autorizace nebo integrovat ASP. NET na `UrlAuthorizationModule` do kanálu HTTP služby IIS, čímž rozšíření tuto funkci pro všechny požadavky.

> [!NOTE]
> Existují určité rozdíly jemně ještě důležité v jak ASP. NET na `UrlAuthorizationModule` a funkce autorizace adresy URL služby IIS 7 je zpracování autorizačních pravidel. V tomto kurzu není zkontrolovat, jestli funkce autorizace adresy URL služby IIS 7 nebo rozdíly v tom, jak ho analyzuje autorizační pravidla ve srovnání s `UrlAuthorizationModule`. Další informace o těchto tématech naleznete v dokumentaci služby IIS 7 na webu MSDN nebo v [www.iis.net](https://www.iis.net/).


## <a name="step-1-defining-url-authorization-rules-inwebconfig"></a>Krok 1: Definování autorizačních pravidel adres URL v`Web.config`

`UrlAuthorizationModule` Určuje, jestli se mají udělit nebo odepřít přístup k požadovanému prostředku pro konkrétní identity podle autorizačních pravidel adres URL definované v konfiguraci aplikace. Autorizační pravidla jsou vypsány v [ `<authorization>` element](https://msdn.microsoft.com/library/8d82143t.aspx) ve formě `<allow>` a `<deny>` podřízené elementy. Každý `<allow>` a `<deny>` podřízený element můžete zadat:

- Určitého uživatele
- Čárkami oddělený seznam uživatelů
- Všichni anonymní uživatelé označený jako otazník (?)
- Všichni uživatelé, označené hvězdičkou (\*)

Následující kód ukazuje, jak povolit uživatelům Tito a Scott a všechny ostatní zakázat pomocí autorizačních pravidel adres URL:

[!code-xml[Main](user-based-authorization-cs/samples/sample1.xml)]

`<allow>` Element definuje, jaký mají uživatelé - Tito a Scott - při `<deny>` dá pokyn element, který *všechny* uživatelé mají odepřený.

> [!NOTE]
> `<allow>` a `<deny>` prvky můžete také určit autorizační pravidla pro role. Vyzkoušíme autorizace na základě rolí v budoucnu kurzu.


Toto nastavení uděluje přístup nikoho jiného, než Sam (včetně anonymních návštěvníky):

[!code-xml[Main](user-based-authorization-cs/samples/sample2.xml)]

Povolit pouze ověřené uživatele, použijte následující konfiguraci, které odepře přístup všem anonymním uživatelům:

[!code-xml[Main](user-based-authorization-cs/samples/sample3.xml)]

Autorizační pravidla jsou definovány v rámci `<system.web>` element v `Web.config` a platí pro všechny prostředky technologie ASP.NET ve webové aplikaci. Často má aplikace jiný autorizační pravidla pro různé oddíly. Například na místě, elektronické obchodování, všechny návštěvníky může důkladně prostudovat produkty, najdete v části recenzemi produktů, vyhledat v katalogu a tak dále. Pouze ověření uživatelé mohou však dosáhnout rezervaci nebo stránky, které chcete spravovat jeden pro přesouvání historie. Kromě toho může být částí webu, které jsou pouze přístupné vyberte uživatele, například správci webu.

Technologie ASP.NET umožňuje snadno definovat různé autorizační pravidla pro různé soubory a složky v této lokalitě. Autorizační pravidla, zadaný v kořenové složce `Web.config` souboru platí pro všechny prostředky technologie ASP.NET v této lokalitě. Však můžete přepsat toto výchozí nastavení autorizace pro konkrétní složky přidáním `Web.config` s `<authorization>` části.

Umožňuje aktualizovat náš web tak, aby pouze ověření uživatelé najdete na stránkách ASP.NET v `Membership` složky. K tomu je potřeba přidat `Web.config` do souboru `Membership` složku a její nastavení autorizace tak, aby odepřel anonymní uživatelé. Klikněte pravým tlačítkem myši `Membership` složky v Průzkumníku řešení, vyberte v nabídce Přidat novou položku v kontextové nabídce a přidejte nový soubor webové konfigurace s názvem `Web.config`.


[![Přidá soubor Web.config ke složce členství](user-based-authorization-cs/_static/image8.png)](user-based-authorization-cs/_static/image7.png)

**Obrázek 3**: přidání `Web.config` do souboru `Membership` složky ([Kliknutím zobrazit obrázek v plné velikosti](user-based-authorization-cs/_static/image9.png))


Projekt v tomto okamžiku by měl obsahovat dvě `Web.config` soubory: jeden v kořenovém adresáři a po jednom v `Membership` složky.


[![Aplikace by měl nyní obsahovat dvou souborech Web.config](user-based-authorization-cs/_static/image11.png)](user-based-authorization-cs/_static/image10.png)

**Obrázek 4**: vaše aplikace by měl nyní obsahovat dvě `Web.config` soubory ([Kliknutím zobrazit obrázek v plné velikosti](user-based-authorization-cs/_static/image12.png))


Aktualizovat konfigurační soubor v `Membership` složky, které zakazuje přístup k anonymním uživatelům.

[!code-xml[Main](user-based-authorization-cs/samples/sample4.xml)]

To je všechno je k němu!

K otestování této změny, přejděte na domovskou stránku v prohlížeči a ujistěte se, že jste přihlášeni. Vzhledem k tomu, že výchozí chování aplikace technologie ASP.NET je umožnit všechny návštěvníky a vzhledem k tomu, že jsme nepodali veškeré úpravy autorizace kořenový adresář `Web.config` souboru jsme se můžou navštívit soubory v kořenovém adresáři jako anonymní návštěvníka.

Klikněte na odkaz vytváření uživatelských účtů v levém sloupci najít. Bude to trvat, abyste `~/Membership/CreatingUserAccounts.aspx`. Vzhledem k tomu, `Web.config` v soubor `Membership` složky definuje autorizační pravidla se má anonymní přístup, `UrlAuthorizationModule` zruší požadavek a vrátí protokolu HTTP 401 Unauthorized status. `FormsAuthenticationModule` Upraví to 302 stavu přesměrování, nám pošlete na přihlašovací stránku. Všimněte si, že stránce jsme byly pokusu o přístup k (`CreatingUserAccounts.aspx`) je předána na stránku přihlášení prostřednictvím `ReturnUrl` parametr řetězce dotazu.


[![Od adresy URL autorizační pravidla zakázat anonymní přístup jsme přesměrováni na stránku přihlášení](user-based-authorization-cs/_static/image14.png)](user-based-authorization-cs/_static/image13.png)

**Obrázek 5**: vzhledem k tomu, že autorizace adres URL pravidla zakázat anonymní přístup, jsme přesměrováni na stránku pro přihlášení ([Kliknutím zobrazit obrázek v plné velikosti](user-based-authorization-cs/_static/image15.png))


Po úspěšném přihlášení jsme přesměrováni `CreatingUserAccounts.aspx` stránky. Tato doba `UrlAuthorizationModule` umožňuje přístup na stránku, protože jsme už jsou anonymní.

### <a name="applying-url-authorization-rules-to-a-specific-location"></a>Použití autorizačních pravidel adres URL do určitého umístění

Definované v nastavení autorizace `<system.web>` části `Web.config` platí pro všechny prostředky technologie ASP.NET v tomto adresáři a jejích podadresářů (dokud jinak nepotlačí jiným `Web.config` souboru). V některých případech ale může chceme všechny prostředky technologie ASP.NET v daném adresáři do mají autorizaci konkrétní konfiguraci s výjimkou jedno nebo dvě konkrétní stránky. Toho lze dosáhnout přidáním `<location>` element v `Web.config`, odkazující na soubor, jehož autorizační pravidla se liší a v něm definování jeho jedinečné autorizačních pravidel.

Pro ilustraci použití `<location>` elementu, který chcete přepsat nastavení konfigurace pro určitý prostředek, můžeme přizpůsobit nastavení autorizace tak, aby navštívit pouze Tito `CreatingUserAccounts.aspx`. K tomu, přidejte `<location>` elementu, který chcete `Membership` složky `Web.config` souboru a aktualizujte jeho kód tak, aby vypadal jako následující:

[!code-xml[Main](user-based-authorization-cs/samples/sample5.xml)]

`<authorization>` Element v `<system.web>` definuje výchozí autorizačních pravidel adres URL na prostředky ve `Membership` složce a jejích podsložkách. `<location>` Element umožňuje nám k přepsání těchto pravidel pro určitý prostředek. Ve výše uvedených značek `<location>` element odkazy `CreatingUserAccounts.aspx` stránky a určuje jeho autorizační pravidla, například tak, aby Tito, ale všichni ostatní odepřít.

K otestování této změny autorizace, spusťte navštivte web jako anonymní uživatel. Když zkusíte najdete na žádné stránce v `Membership` složky, například `UserBasedAuthorization.aspx`, `UrlAuthorizationModule` odmítne žádost a budete přesměrováni na stránku přihlášení. Po přihlášení jako Scott Řekněme, můžete navštívit libovolné stránce `Membership` složky *s výjimkou* pro `CreatingUserAccounts.aspx`. Probíhá pokus o navštivte `CreatingUserAccounts.aspx` přihlášení jako každý, kdo ale Tito způsobí pokus o neoprávněný přístup, můžete přesměrovat zpět na stránku přihlášení.

> [!NOTE]
> `<location>` Element musí nacházet mimo konfigurace `<system.web>` elementu. Je nutné použít samostatné `<location>` element pro každý prostředek, jehož nastavení autorizace, kterou chcete přepsat.


### <a name="a-look-at-how-theurlauthorizationmoduleuses-the-authorization-rules-to-grant-or-deny-access"></a>Podívejte se na jak`UrlAuthorizationModule`používá autorizační pravidla pro přístup udělit nebo odepřít

`UrlAuthorizationModule` Určuje, jestli k autorizaci konkrétní identity pro konkrétní adresu URL analýzou autorizace adres URL pravidla jeden po druhém, od první a práci směrem dolů. Jakmile je nalezena shoda, je uživatel povolen nebo odepřen přístup, podle toho, pokud v byla nalezena shoda `<allow>` nebo `<deny>` elementu. <strong>Pokud není nalezena žádná shoda, uživateli je udělen přístup.</strong> V důsledku toho, pokud chcete omezit přístup, je nutné použít `<deny>` element jako posledním prvkem v konfiguraci ověřování adresy URL. <strong>Pokud vynecháte</strong><strong>`<deny>`</strong><strong>elementu, všichni uživatelé bude udělen přístup.</strong>

Abyste lépe pochopili proces používané `UrlAuthorizationModule` k určení autority, podívejte se na příklad autorizačních pravidel adres URL, které jsme se podívali na dříve v tomto kroku. První pravidlo je `<allow>` element, který umožňuje přístup k Tito a Scott. Je druhý pravidla `<deny>` element, který odepře přístup všem uživatelům. Pokud anonymní uživatel navštíví, `UrlAuthorizationModule` začne tím, že požádá, je anonymní Scott nebo Tito? Odpověď, ne, je samozřejmě, takže pokračuje na druhé pravidlo. Je v sadě každý uživatel anonymní? Od odpověď tady je Ano, `<deny>` pravidlo je v platnosti put a návštěvníka se přesměruje na přihlašovací stránku. Podobně, pokud je návštěvou Jisun, `UrlAuthorizationModule` začne tím, že požádá, je Jisun Scott nebo Tito? Vzhledem k tomu, že uživatel není, je `UrlAuthorizationModule` pokračuje druhou otázku, je Jisun v sadě každý uživatel? Uživatel je, takže uživatel, příliš, byl odepřen přístup. Navíc pokud Tito navštíví, první otázku chráněná `UrlAuthorizationModule` je pozitivní odpovědi, takže Tito je udělen přístup.

Vzhledem k tomu `UrlAuthorizationModule` procesy autorizaci pravidla shora dolů zastavit v žádné shody, je důležité mít více konkrétní pravidla dřívější než těm, které jsou menší konkrétní. To znamená můžete definovat pravidla autorizace nezakazuje Jisun a anonymní uživatelé, ale všechny ostatní ověřeným uživatelům umožnit, můžete by začínat nejvíce konkrétní pravidlo – jeden ovlivňuje Jisun - a pak pokračujte méně specifická pravidla - pravidla povolení všech ostatních ověřené uživatele, ale odepření všichni anonymní uživatelé. Následující autorizačních pravidel adres URL implementuje tuto zásadu nejprve odepření Jisun, a pak odepření anonymní uživatel. Všechny ověřené uživatele než Jisun bude udělen přístup protože ani jedno z těchto `<deny>` bude shodovat s příkazy.

[!code-xml[Main](user-based-authorization-cs/samples/sample6.xml)]

## <a name="step-2-fixing-the-workflow-for-unauthorized-authenticated-users"></a>Krok 2: Opravě pracovního postupu pro neoprávněné, ověřeného uživatele

Jak již bylo zmíněno dříve v tomto kurzu v vypadat A v části Adresa URL autorizace pracovního postupu, kdykoli ukáže neoprávněného požadavku, `UrlAuthorizationModule` zruší požadavek a vrátí protokolu HTTP 401 Unauthorized status. Tento stav 401 je upraveném `FormsAuthenticationModule` do 302 přesměrování stav, který odešle uživatele na přihlašovací stránku. Tento pracovní postup probíhá na neoprávněného požadavku, i když je uživatel ověřený.

Vrácení ověřeného uživatele na přihlašovací stránku je pravděpodobně jim zaměnit, protože jsou již přihlášení do systému. S chvilku pracovní můžeme vylepšit tento pracovní postup přesměrováním ověřené uživatele, kteří žádají o neoprávněným na stránku, která vysvětluje, že se pokusí přistoupit ke stránce s omezeným přístupem.

Začněte vytvořením novou stránku ASP.NET v kořenové složce webové aplikace s názvem `UnauthorizedAccess.aspx`; nezapomeňte přidružit tuto stránku s `Site.master` stránky předlohy. Po vytvoření této stránce, odeberte obsahu ovládací prvek, který odkazuje `LoginContent` ContentPlaceHolder tak, aby stránky předlohy výchozí obsahu se zobrazí. Dál přidejte zprávu, která vysvětluje situaci, a to, že uživatel se pokusil získat přístup k chráněnému prostředku. Po přidání takové zprávy `UnauthorizedAccess.aspx` stránky deklarativní by měl vypadat podobně jako následující:

[!code-aspx[Main](user-based-authorization-cs/samples/sample7.aspx)]

Nyní potřebujeme ke změně pracovního postupu, takže pokud ověřený uživatel provádí neoprávněného požadavku se odešlou `UnauthorizedAccess.aspx` stránky místo přihlašovací stránku. Je v privátní metoda schovaný logiky, která přesměruje na přihlašovací stránku neoprávněné žádosti `FormsAuthenticationModule` třídy, takže jsme nelze přizpůsobit toto chování. Co můžeme udělat, ale je přidat vlastní logiky na přihlašovací stránku, který přesměruje uživatele na `UnauthorizedAccess.aspx`, v případě potřeby.

Když `FormsAuthenticationModule` neoprávněným návštěvníka přesměruje na přihlašovací stránku přidá požadovaný, neoprávněným URL do řetězce dotazu s názvem `ReturnUrl`. Například, pokud neoprávněný uživatel se pokusil navštivte `OnlyTito.aspx`, `FormsAuthenticationModule` by přesměruje je na `Login.aspx?ReturnUrl=OnlyTito.aspx`. Proto pokud ověřený uživatel s řetězec dotazu, který zahrnuje je dosaženo přihlašovací stránky `ReturnUrl` parametr a potom jsme vědět, že tento neověřený uživatel právě pokusil o najdete stránce uživatel nemá oprávnění k zobrazení. V takovém případě chceme přesměrování mu pomůže `UnauthorizedAccess.aspx`.

K tomu, přidejte následující kód na stránku přihlášení `Page_Load` obslužné rutiny události:

[!code-csharp[Main](user-based-authorization-cs/samples/sample8.cs)]

Ve výše uvedeném kódu přesměruje ověřený, neoprávněným uživatelům `UnauthorizedAccess.aspx` stránky. Pokud chcete zobrazit tuto logiku akce, přejděte na web jako anonymní návštěvníka a klikněte na odkaz vytváření uživatelských účtů v levém sloupci. Bude to trvat, abyste `~/Membership/CreatingUserAccounts.aspx` stránku, která v kroku 1, které jsme nakonfigurovali jenom povolit přístup k Tito. Vzhledem k tomu, že anonymní uživatelé jsou zakázané, `FormsAuthenticationModule` přesměruje nám zpět na stránku přihlášení.

V tomto okamžiku jsou jsme anonymní, takže `Request.IsAuthenticated` vrátí `false` a jsme nejsou přesměrován na `UnauthorizedAccess.aspx`. Místo toho se zobrazí přihlašovací stránku. Přihlaste se jako uživatel než Tito, jako je například Bruce. Po zadání příslušná pověření, přihlašovací stránce přesměruje nám zpět na `~/Membership/CreatingUserAccounts.aspx`. Vzhledem k tomu, že tato stránka je k dispozici pouze pro Tito, přesto však jsou Neautorizováno k jejímu zobrazení a rychle se vrátíte na stránku přihlášení. Tentokrát, ale `Request.IsAuthenticated` vrátí `true` (a `ReturnUrl` existuje parametr řetězce dotazu), takže jsme zprávy jsou přesměrovány do `UnauthorizedAccess.aspx` stránky.


[![Ověření neoprávnění uživatelé přesměrováni na UnauthorizedAccess.aspx](user-based-authorization-cs/_static/image17.png)](user-based-authorization-cs/_static/image16.png)

**Obrázek 6**: ověření neoprávnění uživatelé přesměrováni na `UnauthorizedAccess.aspx` ([Kliknutím zobrazit obrázek v plné velikosti](user-based-authorization-cs/_static/image18.png))


Tento vlastní pracovní postup uvede víc rozumný a přehledné činnost koncového uživatele ve circuiting krátký cyklus znázorněný na obrázku 2.

## <a name="step-3-limiting-functionality-based-on-the-currently-logged-in-user"></a>Krok 3: Omezení funkce na základě aktuálně přihlášeného uživatele

Autorizace adres URL umožňuje snadno určit hrubým autorizačních pravidel. Jak jsme viděli v kroku 1, s autorizace adres URL jsme můžete stručně stavu jaké identity jsou povoleny a které z nich je odepřeno zobrazení konkrétní stránky nebo všech stránek ve složce. V některých scénářích však může Chceme všem uživatelům najdete stránce, ale omezit tak jeho funkčnost stránky na základě uživatele návštěvou ho.

Podívejte se elektronické obchodování web, který umožňuje ověřeným návštěvníkům zkontrolujte jejich produkty. Anonymní uživatel navštíví stránku produktů, uvidí jenom informace o produktu a nebude mít možnost ponechat kontrolu. Ověřený uživatel na stejné stránce však uvidí prostudovali rozhraní. Pokud ověřený uživatel nemá ještě prohlédli tohoto produktu, rozhraní by mohly k odeslání kontrolu; v opačném případě ji by je zobrazovat jejich kontrola byla odeslána. Aby bylo tento scénář krok navíc stránky produktu může zobrazit další informace a nabízí rozšířené funkce pro uživatele, kteří pracují pro elektronické obchodování společnosti. Stránky produktu může například seznam inventáře v stock a zahrnují možnost Upravit ceny a popis při návštěvě zaměstnanec produktu.

Tato pravidla autorizace spočívá v jemné dá implementovat buď deklarativně nebo prostřednictvím kódu programu (nebo prostřednictvím některé kombinaci obou). V další části jsme se zobrazí jak implementovat spočívá v jemné autorizace prostřednictvím ovládacího prvku LoginView. Následující, se podíváme na programový techniky. Podíváme můžete na použití spočívá v jemné autorizační pravidla, ale nejprve musíme vytvořit stránku, jejichž funkce závisí na, že uživatel navštíví ho.

Umožňuje vytvořit stránku, která obsahuje soubory, které v konkrétní adresáře v rámci GridView. Společně s výpis název souboru, velikost a další informace, bude obsahovat GridView dva sloupce LinkButtons: jednu s názvem, zobrazení a jeden názvem odstranit. Po kliknutí na LinkButton zobrazení, zobrazí se obsah vybraného souboru; Po kliknutí na LinkButton odstranit soubor se odstraní. Nejdřív vytvoříme této stránky tak, aby jeho zobrazení a odstranění funkce je k dispozici všem uživatelům. V pomocí části LoginView řízení a omezení funkce prostřednictvím kódu programu, vidíme postup povolení nebo zakázání tyto funkce na základě uživatele, kteří navštěvují stránky.

> [!NOTE]
> Stránku ASP.NET, které jsme se chystáte vytvořit pomocí ovládacího prvku GridView zobrazí seznam souborů. Od tohoto kurzu, které řady se zaměřuje na ověřování pomocí formulářů, ověřování, uživatelské účty a rolí nechci se příliš mnoho času hovoříte o vnitřního chodu ovládacího prvku GridView. Při tomto kurzu obsahuje konkrétní podrobné pokyny pro nastavení této stránky, není pustíte do podrobnosti proč byly provedeny některé možnosti, nebo konkrétní vlastnosti vliv mají na vykresleném výstupu. Získat důkladné posouzení ovládacího prvku GridView, obraťte se na Moje *[práci s daty v technologii ASP.NET 2.0](../../data-access/index.md)* kurz řady.


Začněte otevřením `UserBasedAuthorization.aspx` v soubor `Membership` složku a přidání ovládacího prvku GridView na stránku s názvem `FilesGrid`. Z prvku GridView inteligentních značek klikněte na odkaz Upravit sloupce spustit dialogové okno pole. Z tohoto místa zrušte zaškrtnutí políčka automaticky generovat pole v levém dolním rohu. Dál přidejte tlačítka Vybrat tlačítko Odstranit a dvě BoundFields z levého horního rohu (tlačítka vyberte a odstranění naleznete v části Typ CommandField). Vyberte tlačítko nastavit `SelectText` vlastnost k zobrazení a první BoundField `HeaderText` a `DataField` vlastnosti název. Nastavit druhý BoundField `HeaderText` vlastnost, která má velikost v bajtech, jeho `DataField` vlastnost, která má délku jeho `DataFormatString` vlastnost {0: N0} a její `HtmlEncode` vlastnost na hodnotu False.

Po dokončení konfigurace prvku GridView sloupce, klikněte na tlačítko OK zavřete dialogové okno pole. V okně vlastnosti nastavit GridView `DataKeyNames` vlastnost `FullName`. V tomto okamžiku rutina GridView deklarativní by měl vypadat následovně:

[!code-aspx[Main](user-based-authorization-cs/samples/sample9.aspx)]

S prvku GridView značek vytvořen jsme připravení psát kód, který bude načítat soubory v jednom adresáři a navázat je GridView. Přidejte následující kód na stránku `Page_Load` obslužné rutiny události:

[!code-csharp[Main](user-based-authorization-cs/samples/sample10.cs)]

Výše uvedený kód používá [ `DirectoryInfo` třída](https://msdn.microsoft.com/library/system.io.directoryinfo.aspx) získat seznam souborů v kořenové složce aplikace. [ `GetFiles()` Metoda](https://msdn.microsoft.com/library/system.io.directoryinfo.getfiles.aspx) vrátí všechny soubory v adresáři jako pole [ `FileInfo` objekty](https://msdn.microsoft.com/library/system.io.fileinfo.aspx), který je pak vázána GridView. `FileInfo` Objekt má širokou vlastnosti, jako například `Name`, `Length`, a `IsReadOnly`, mimo jiné. Jak je vidět z jeho deklarativní GridView zobrazí jenom `Name` a `Length` vlastnosti.

> [!NOTE]
> `DirectoryInfo` a `FileInfo` třídy se nacházejí v [ `System.IO` obor názvů](https://msdn.microsoft.com/library/system.io.aspx). Proto bude buď potřeba adresa tyto názvy tříd s jejich názvy oborů názvů nebo obor názvů importovat soubor třídy (prostřednictvím `using System.IO`).


Za chvíli najdete na této stránce prostřednictvím prohlížeče. Zobrazí seznam souborů, které se nacházejí v kořenovém adresáři aplikace. Kliknutím na tlačítko Zobrazit nebo odstranit LinkButtons způsobí, že zpětné volání, ale žádná akce dojde, protože jsme ještě k vytváření obslužných rutin událostí nezbytné.


[![GridView obsahuje soubory, které v kořenovém adresáři webové aplikace](user-based-authorization-cs/_static/image20.png)](user-based-authorization-cs/_static/image19.png)

**Obrázek 7**: GridView obsahuje soubory, které v kořenovém adresáři webové aplikace ([Kliknutím zobrazit obrázek v plné velikosti](user-based-authorization-cs/_static/image21.png))


Potřebujeme prostředky k zobrazení obsahu vybraného souboru. Vraťte se na Visual Studio a přidat textové pole s názvem `FileContents` výše GridView. Nastavte její `TextMode` vlastnost `MultiLine` a jeho `Columns` a `Rows` vlastností 95 % a 10, v uvedeném pořadí.

[!code-aspx[Main](user-based-authorization-cs/samples/sample11.aspx)]

Dále vytvořte obslužnou rutinu události pro prvku GridView [ `SelectedIndexChanged` událostí](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindexchanged.aspx) a přidejte následující kód:

[!code-csharp[Main](user-based-authorization-cs/samples/sample12.cs)]

Tento kód používá prvku GridView `SelectedValue` vlastnosti k určení názvu celého souboru vybraného souboru. Interně `DataKeys` kolekce se odkazuje, aby bylo možné získat `SelectedValue`, takže je nutné nastavit GridView `DataKeyNames` vlastnost, která má název, jak je popsáno výše v tomto kroku. [ `File` Třída](https://msdn.microsoft.com/library/system.io.file.aspx) slouží k načtení obsah vybraného souboru na řetězec, který byl přiřazen k `FileContents` textové pole na `Text` vlastnost, a tím zobrazení obsahu vybraného souboru na stránce.


[![Do textového pole se zobrazí obsah souboru vybrané](user-based-authorization-cs/_static/image23.png)](user-based-authorization-cs/_static/image22.png)

**Obrázek 8**: vybraný soubor je obsah se zobrazí v textové pole ([Kliknutím zobrazit obrázek v plné velikosti](user-based-authorization-cs/_static/image24.png))


> [!NOTE]
> Pokud zobrazit obsah souboru, který obsahuje kód HTML a pak se pokusíte zobrazit nebo odstranit soubor, dojde `HttpRequestValidationException` chyby. K tomu dochází, protože na zpětné volání obsahu textového pole jsou odeslána zpět do webového serveru. Ve výchozím nastavení, ASP.NET vyvolá `HttpRequestValidationException` chyba vždy, když se zjistí potenciálně nebezpečný obsah zpětného volání, jako je například značka jazyka HTML. Zakázat výskytu této chyby, vypněte ověření žádosti pro stránku přidáním `ValidateRequest="false"` k `@Page` – direktiva. Další informace o výhodách ověření žádosti jako a taky jaká opatření byste měli vzít při zakázání, přečtěte si [ověření žádosti - prevence útoků skriptu](https://asp.net/learn/whitepapers/request-validation/).


Nakonec přidejte obslužné rutiny události s následujícím kódem pro prvku GridView [ `RowDeleting` událostí](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx):

[!code-csharp[Main](user-based-authorization-cs/samples/sample13.cs)]

Kód jednoduše zobrazí úplný název souboru pro odstranění v `FileContents` TextBox *bez* skutečného odstranění souboru.


[![Kliknutím na tlačítko Odstranit nedojde k odstranění ve skutečnosti souboru](user-based-authorization-cs/_static/image26.png)](user-based-authorization-cs/_static/image25.png)

**Obrázek 9**: Kliknutím odstranit tlačítko ve skutečnosti neodstraní na soubor ([Kliknutím zobrazit obrázek v plné velikosti](user-based-authorization-cs/_static/image27.png))


V kroku 1 jsme nakonfigurovali autorizačních pravidel adres URL do mají anonymní uživatelé zakázáno zobrazení stránky v `Membership` složky. Aby bylo možné lépe vykazovat spočívá v jemné ověřování, můžeme povolit anonymní uživatelé navštíví `UserBasedAuthorization.aspx` stránky, ale s omezenou funkčností. Otevřete tuto stránku přístup všichni uživatelé, přidejte následující `<location>` elementu, který chcete `Web.config` v soubor `Membership` složky:

[!code-xml[Main](user-based-authorization-cs/samples/sample14.xml)]

Po přidání tohoto `<location>` elementu test nové pravidel autorizace adres URL protokolování mimo lokalitu. Jako anonymní uživatel má povolené přejděte `UserBasedAuthorization.aspx` stránky.

V současné době navštívit každý uživatel ověřený nebo anonymní `UserBasedAuthorization.aspx` stránky a zobrazovat a odstraňovat soubory. Umožňuje nastavit ho, aby pouze ověřené uživatele můžete zobrazit obsah souboru a pouze Tito můžete odstranit soubor. Takové spočívá v jemné autorizační pravidla můžete použít deklarativně, prostřednictvím kódu programu nebo pomocí kombinace obou metod. Umožňuje použít deklarativní způsob pro omezení, kdo může zobrazit obsah souboru; použijeme programový přístup k omezení, kdo může odstranit soubor.

### <a name="using-the-loginview-control"></a>Použití ovládacího prvku LoginView

Jak jsme viděli v posledních kurzy, ovládacího prvku LoginView je užitečná pro zobrazení různých rozhraní pro ověřený a anonymní uživatele a nabízí snadný způsob, jak skrýt funkce, která není přístupná pro anonymní uživatele. Vzhledem k tomu, že počet anonymních uživatelů nelze zobrazit nebo odstranit soubory, potřebujeme pouze zobrazit `FileContents` TextBox při návštěvě stránce ověřeného uživatele. Jak toho docílit, přidání ovládacího prvku LoginView na stránku, pojmenujte ji `LoginViewForFileContentsTextBox`a přesuňte `FileContents` textové pole na deklarativní do ovládacího prvku LoginView `LoggedInTemplate`.

[!code-aspx[Main](user-based-authorization-cs/samples/sample15.aspx)]

Ovládací prvky webového v šablonách LoginView již nejsou přímo přístupné z třídy kódu. Například `FilesGrid` GridView `SelectedIndexChanged` a `RowDeleting` obslužné rutiny událostí aktuálně odkazují `FileContents` jako TextBox – ovládací prvek s kódem:

[!code-csharp[Main](user-based-authorization-cs/samples/sample16.cs)]

Tento kód je však již nebude platný. Přesunutím `FileContents` textové pole do `LoggedInTemplate` textové pole nemůže být k nim přistupuje přímo. Místo toho musíte použít jsme `FindControl("controlId")` metoda prostřednictvím kódu programu odkazovat na ovládací prvek. Aktualizace `FilesGrid` obslužné rutiny událostí tak, aby odkazovaly do textového pole takto:

[!code-csharp[Main](user-based-authorization-cs/samples/sample17.cs)]

Po přesunutí textového pole na LoginView `LoggedInTemplate` a aktualizace kódu stránky k textovému poli pomocí odkazu `FindControl("controlId")` vzor, najdete na stránce jako anonymní uživatel. Jak ukazuje obrázek 10, `FileContents` TextBox se nezobrazí. LinkButton zobrazení je však stále zobrazena.


[![Ovládací prvek LoginView pouze vykreslí FileContents textové pole pro ověřené uživatele](user-based-authorization-cs/_static/image29.png)](user-based-authorization-cs/_static/image28.png)

**Obrázek 10**: LoginView řízení pouze vykreslí `FileContents` textové pole pro ověřené uživatele ([Kliknutím zobrazit obrázek v plné velikosti](user-based-authorization-cs/_static/image30.png))


Jedním ze způsobů skrýt tlačítko zobrazení pro anonymní uživatele je převést na TemplateField GridView pole. Tím se vygeneruje šablonu, která obsahuje deklarativní pro LinkButton zobrazení. Můžeme pak přidání ovládacího prvku LoginView pole TemplateField a umístěte LinkButton v rámci LoginView `LoggedInTemplate`, a tím skrytí tlačítko zobrazení od návštěvníků anonymní. K tomu, klikněte na odkaz Upravit sloupce z prvku GridView inteligentních značek spustit dialogové okno pole. V dalším kroku vyberte tlačítka vybrat ze seznamu v levém dolním a potom klikněte převést toto pole na TemplateField odkaz. Díky tomu slouží k úpravě tohoto pole deklarativní z:

[!code-aspx[Main](user-based-authorization-cs/samples/sample18.aspx)]

 Do: 

[!code-aspx[Main](user-based-authorization-cs/samples/sample19.aspx)]

V tuto chvíli jsme můžete přidat LoginView TemplateField. Následující kód zobrazí LinkButton zobrazení pouze pro ověřené uživatele.

[!code-aspx[Main](user-based-authorization-cs/samples/sample20.aspx)]

Jak ukazuje obrázek 11, není konečný výsledek, že poměrně jako zobrazení je stále zobrazen sloupec Přestože LinkButtons zobrazení v rámci sloupce jsou skryté. Podíváme se na postup skrýt celý sloupec GridView (a ne jenom LinkButton) v další části.


[![Ovládací prvek LoginView skryje LinkButtons zobrazení pro anonymní návštěvníky](user-based-authorization-cs/_static/image32.png)](user-based-authorization-cs/_static/image31.png)

**Obrázek 11**: ovládací prvek LoginView skryje LinkButtons zobrazení pro anonymní návštěvníky ([Kliknutím zobrazit obrázek v plné velikosti](user-based-authorization-cs/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>Prostřednictvím kódu programu omezení funkce

V některých případech deklarativní techniky nedostačují k omezení funkce na stránku. Například dostupnost určité funkce pro stránky může být závislá na kritéria nad rámec, zda je uživatel na stránce anonymní nebo ověřený. V takových případech můžou různé prvky uživatelského rozhraní zobrazení nebo skrytí programové způsoby.

Chcete-li omezit funkčnost prostřednictvím kódu programu, je potřeba provést dvě úlohy:

1. Určení, zda má uživatel na stránce přístup je funkce, a
2. Programová změna uživatelského rozhraní na základě toho, jestli má uživatel přístup k funkci nejistá.

K předvedení aplikace tyto dvě úlohy, můžeme povolit pouze Tito k odstranění souborů z GridView. Naše první úkolů, pak je zjistit, zda je na stránce Tito. Po který byl vyhodnocen, potřebujeme (zobrazit nebo skrýt) prvku GridView odstranění sloupce. Rutina GridView na sloupce jsou přístupná prostřednictvím jeho `Columns` vlastnost; sloupec je vykreslen pouze, pokud jeho `Visible` je nastavena na `true` (výchozí).

Přidejte následující kód, který `Page_Load` obslužné rutiny události před vazbě dat na GridView:

[!code-csharp[Main](user-based-authorization-cs/samples/sample21.cs)]

Jak již bylo zmíněno [ *Přehled ověřování založené na formulářích* ](../introduction/an-overview-of-forms-authentication-cs.md) kurzu `User.Identity.Name` vrací název identity. To odpovídá zadané v ovládacím prvku přihlašovací uživatelské jméno. Pokud je Tito návštěvou stránky, druhý sloupec prvku GridView `Visible` je nastavena na `true`, jinak je nastaven na hodnotu `false`. Net výsledkem je, že když někdo jiný než Tito navštíví stránce jiné ověřeného uživatele nebo anonymního uživatele, odstranit sloupec není vykreslen (viz obrázek 12); ale pokud Tito navštíví stránky, odstranit sloupec nachází (viz obrázek 13).


[![Odstranit sloupec není vykresluje při navštívené podle někdo jiný než Tito (například Bruce)](user-based-authorization-cs/_static/image35.png)](user-based-authorization-cs/_static/image34.png)

**Obrázek 12**: odstranění sloupec není vykresluje při navštívené podle někdo jiný než Tito (například Bruce) ([Kliknutím zobrazit obrázek v plné velikosti](user-based-authorization-cs/_static/image36.png))


[![Odstranit sloupce je vykreslen Tito](user-based-authorization-cs/_static/image38.png)](user-based-authorization-cs/_static/image37.png)

**Obrázek 13**: odstranění sloupec je vykreslen Tito ([Kliknutím zobrazit obrázek v plné velikosti](user-based-authorization-cs/_static/image39.png))


## <a name="step-4-applying-authorization-rules-to-classes-and-methods"></a>Krok 4: Použití autorizačních pravidel pro třídy a metody

V kroku 3 jsme nepovolené anonymní uživatelé ze zobrazení obsahu souboru a nesmějí všechny uživatele, ale Tito odstraňování souborů. Toho dosáhlo tím skrytí prvky přidružené uživatelského rozhraní pro neoprávněné návštěvníky prostřednictvím deklarativní a programové techniky. Pro naše jednoduchý příklad správně skrytí prvky uživatelského rozhraní se jednoznačné, ale co o složitější lokality tam, kde může být mnoha různými způsoby provádět stejné funkce? V omezení, které tuto funkci neoprávněným uživatelům, co se stane, pokud jsme nezapomněli skrytí nebo zakažte všechny prvky odpovídající uživatelské rozhraní?

Snadný způsob, jak Ujistěte se, že konkrétní funkce nelze získat přístup, neoprávněný uživatel je pro uspořádání třídy nebo metoda s [ `PrincipalPermission` atribut](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Pokud modul runtime rozhraní .NET používá třídu nebo spustí jeden z jeho metody, zkontroluje, ujistěte se, zda aktuální kontext zabezpečení má oprávnění k použití třídy nebo spuštění metody. `PrincipalPermission` Atribut poskytuje mechanismus, pomocí kterého definujeme tato pravidla.

Pojďme demonstruje použití `PrincipalPermission` atribut v prvku GridView `SelectedIndexChanged` a `RowDeleting` obslužné rutiny událostí zakázat spouštění anonymní uživatelé a uživatelé než Tito, v uvedeném pořadí. Je budeme muset udělat přidat atribut příslušné na každý definice funkce:

[!code-csharp[Main](user-based-authorization-cs/samples/sample22.cs)]

Atribut pro `SelectedIndexChanged` určují obslužná rutina události, které pouze ověřené uživatele může spustit obslužné rutiny události, kde je jako atribut na `RowDeleting` obslužné rutiny události omezuje na Tito provedení.

Pokud nějakým způsobem, než Tito uživatel pokusí spustit `RowDeleting` obslužné rutiny události nebo neověřený uživatel pokusí spustit `SelectedIndexChanged` obslužné rutiny události, bude vyvolána modul runtime rozhraní .NET `SecurityException`.


[![Pokud kontext zabezpečení nemá oprávnění k provedení metody, je vyvolána výjimka zabezpečení](user-based-authorization-cs/_static/image41.png)](user-based-authorization-cs/_static/image40.png)

**Obrázek 14**: Pokud kontext zabezpečení nemá oprávnění k provedení metody, `SecurityException` je vyvolána ([Kliknutím zobrazit obrázek v plné velikosti](user-based-authorization-cs/_static/image42.png))


> [!NOTE]
> Povolit více kontexty zabezpečení pro přístup k třída nebo metoda, uspořádání třída nebo metoda s `PrincipalPermission` atribut pro každý kontext zabezpečení. To znamená aby Tito a Bruce provést `RowDeleting` obslužné rutiny události, přidat *dva* `PrincipalPermission` atributy:


[!code-csharp[Main](user-based-authorization-cs/samples/sample23.cs)]

Kromě stránek ASP.NET mnoho aplikací mít také architekturu, která zahrnuje různé vrstvy, jako je například obchodní logiku a Data přístup vrstvy. Tyto vrstvy jsou obvykle implementovány jako knihovny tříd a nabízet třídy a metody pro provádění obchodní logiku a data související funkce. `PrincipalPermission` Atribut je vhodná pro použití autorizačních pravidel pro tyto vrstvy.

Další informace o používání `PrincipalPermission` atribut definovat pravidla ověřování na třídy a metody, získáte na [Scott Guthrie](https://weblogs.asp.net/scottgu/)na položce blogu [přidáním autorizačních pravidel pro obchodní a datové vrstvy pomocí `PrincipalPermissionAttributes` ](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Souhrn

V tomto kurzu jsme se podívali na tom, jak používat autorizační pravidla založená na uživatele. Spuštění se podívejte se na ASP. Na NET framework autorizace adresy URL. V každé žádosti, modul ASP.NET `UrlAuthorizationModule` zkontroluje autorizačních pravidel adres URL definované v konfiguraci aplikace k určení, zda je identita oprávnění k přístupu k požadovanému zdroji. Stručně řečeno autorizace adres URL umožňuje snadno určit autorizační pravidla pro konkrétní stránky nebo pro všechny stránky v jednom adresáři.

Rozhraní autorizace adresy URL používá pravidla autorizace na základě po stránkách. S autorizace adres URL je identitě žádajícího oprávnění pro přístup k určitému prostředku nebo ne. Mnoho scénářů, ale volání pro další spočívá v jemné autorizačních pravidel. Místo definovat, kdo je umožnit přístup k stránky, může musíme umožníte všem uživatelům přistoupit ke stránce, ale jiné data zobrazují nebo nabízejí různé funkce v závislosti na uživatele na stránce. Autorizace úrovni stránky je většinou potřeba skrytí prvky rozhraní konkrétního uživatele Chcete-li zabránit neoprávněným uživatelům v přístupu k funkci zakázané. Kromě toho je možné použít atributy omezit přístup k třídy a provádění její metody pro určité uživatele.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Přidávání do obchodní a datové vrstvy pomocí autorizačních pravidel `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [ASP.NET Authorization](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Změny mezi služby IIS 6 a službu IIS7 zabezpečení](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Konfigurace konkrétní soubory a podadresáře](https://msdn.microsoft.com/library/6hbkh9s7.aspx)
- [Omezení funkce změny dat na základě uživatele](../../data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs.md)
- [Ovládací prvek LoginView – elementy QuickStart](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/loginview.aspx)
- [Principy IIS7 autorizace adres URL](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)
- [`UrlAuthorizationModule` Technická dokumentace](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)
- [Práce s daty v technologii ASP.NET 2.0](../../data-access/index.md)

### <a name="about-the-author"></a>O autorovi

Scott Meisnerová, vytvořit více knih ASP/ASP.NET a zakladatele 4GuysFromRolla.com, má byla od 1998 práce s technologií Microsoft Web. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k  *[Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott lze dosáhnout za [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) nebo prostřednictvím svého blogu v [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Předchozí](validating-user-credentials-against-the-membership-user-store-cs.md)
> [další](storing-additional-user-information-cs.md)
