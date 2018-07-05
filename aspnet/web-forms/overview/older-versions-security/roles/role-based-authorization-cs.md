---
uid: web-forms/overview/older-versions-security/roles/role-based-authorization-cs
title: Autorizace na základě role (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Tento kurz pracuje s podívat, jak rozhraní role přidruží role uživatele jeho kontext zabezpečení. Poté zkoumá, jak použít adresu URL na základě rolí...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: 4d9b63fa-c3d4-4e85-82b1-26ae3ba3ca1c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/roles/role-based-authorization-cs
msc.type: authoredcontent
ms.openlocfilehash: c8c22f140478deddc2e44f0933edfe0e499bb471
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397160"
---
<a name="role-based-authorization-c"></a>Autorizace na základě role (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.11.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial11_RoleAuth_cs.pdf)

> Tento kurz pracuje s podívat, jak rozhraní role přidruží role uživatele jeho kontext zabezpečení. Poté zkoumá, jak použít autorizačních pravidel adres URL na základě rolí. Následující, že se podíváme na pomocí deklarativní a programová prostředky pro změnu zobrazená data a funkce nabízené stránky ASP.NET.


## <a name="introduction"></a>Úvod

V <a id="_msoanchor_1"> </a> [ *autorizace na základě uživatele* ](../membership/user-based-authorization-cs.md) kurzu jsme viděli, jak pomocí autorizace adres URL můžete určit, co uživatelé můžou navštívit konkrétní sadu stránek. Stačí stačí nepatrné jazyka `Web.config`, jsme může dát pokyn umožňuje pouze ověřeným uživatelům najdete na stránce ASP.NET. Jsme může určovat, které byly pouze uživatelé Tito a Bob dovolené, nebo označuje, že byly převody povoleny všechny ověřené uživatele s výjimkou Sam.

Kromě autorizace adres URL zvažovali jsme i také deklarativní a programová techniky pro řízení zobrazených dat. a funkce nabízené na základě uživatele na stránku. Konkrétně jsme vytvořili stránku, která uvedena obsah do aktuálního adresáře. Kdokoli může návštěvě této stránky, ale pouze ověřeným uživatelům může zobrazit obsah souborů a pouze Tito může odstranit soubory.

Použití pravidel autorizace na základě uživatele uživatelů může postupně rozrůstat na připomínající účetnictví. Jednodušší údržbu přístupem je použití ověřování na základě rolí. Dobrou zprávou je, že nástroje naše k dispozici pro použití autorizačních pravidel fungovat stejně dobře s rolemi, jako je tomu u uživatelských účtů. Adresa URL autorizační pravidla můžete zadat role místo uživatelů. Ovládací prvek zobrazení přihlášení, který vykreslí jiný výstupní pro ověřený a anonymním uživatelům, lze nastavit k zobrazení různých obsahu na základě rolí přihlášeného uživatele. A rozhraní API rolí obsahuje metody pro zjištění role přihlášeného uživatele.

Tento kurz pracuje s podívat, jak rozhraní role přidruží role uživatele jeho kontext zabezpečení. Poté zkoumá, jak použít autorizačních pravidel adres URL na základě rolí. Následující, že se podíváme na pomocí deklarativní a programová prostředky pro změnu zobrazená data a funkce nabízené stránky ASP.NET. Pusťme se do práce!

## <a name="understanding-how-roles-are-associated-with-a-users-security-context"></a>Principy jak role jsou přidružené k kontext zabezpečení uživatele

Pokaždé, když se požadavek přejde do kanálu technologie ASP.NET je přidružený kontext zabezpečení, který obsahuje informace, které identifikují žadatel. Pokud používáte ověřování pomocí formulářů, ověřovací lístek slouží jako identity token. Jak jsme probírali v <a id="_msoanchor_2"> </a> [ *Přehled ověřování založené na formulářích* ](../introduction/an-overview-of-forms-authentication-cs.md) a <a id="_msoanchor_3"> </a> [ *formulářů Konfigurace ověřování a témata pokročilé* ](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) kurzy, `FormsAuthenticationModule` zodpovídá za určení identitu žadatele, což se provádí během [ `AuthenticateRequest` události](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Pokud se najde platný, vypršela platnost ověřovacího lístku `FormsAuthenticationModule` dekóduje zjistit identitu žadatele. Vytvoří novou `GenericPrincipal` objektu a přiřadí tuto hodnotu na `HttpContext.User` objektu. Účelem objektu zabezpečení, jako jsou `GenericPrincipal`, je identifikace ověřené uživatelské jméno a co uživatel patřit do role. Tento účel je evidentní skutečnost, že mají všechny instanční objekty `Identity` vlastnost a `IsInRole(roleName)` metoda. `FormsAuthenticationModule`, Ale není zájem zaznamenávání informací o rolích a `GenericPrincipal` vytvoří objekt neurčuje žádné role.

Pokud je povolené rozhraní role, [ `RoleManagerModule` ](https://msdn.microsoft.com/library/system.web.security.rolemanagermodule.aspx) modulu HTTP služby kroky po `FormsAuthenticationModule` a identifikuje ověřený uživatel rolí během [ `PostAuthenticateRequest` události](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), který Aktivuje se po `AuthenticateRequest` událostí. Pokud je žádost od ověřeného uživatele, `RoleManagerModule` přepíše `GenericPrincipal` objekt vytvořený pomocí `FormsAuthenticationModule` a nahradí jej s [ `RolePrincipal` objekt](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx). `RolePrincipal` Třídy používá rozhraní API rolí k určení, ke kterým rolím uživatel patří.

Obrázek 1 znázorňuje pracovním postupem kanálu ASP.NET při použití ověřování pomocí formulářů a rozhraní role. `FormsAuthenticationModule` Poprvé spustí, identifikuje uživatele prostřednictvím její ověřovací lístek a vytvoří novou `GenericPrincipal` objektu. Dále `RoleManagerModule` kroky a přepíše `GenericPrincipal` objekt s `RolePrincipal` objektu.

Pokud anonymní uživatel navštíví web, ani `FormsAuthenticationModule` ani `RoleManagerModule` vytvoří objekt zabezpečení.


[![Události kanálu ASP.NET pro ověřeného uživatele při použití ověřování pomocí formulářů a rozhraní role](role-based-authorization-cs/_static/image2.png)](role-based-authorization-cs/_static/image1.png)

**Obrázek 1**: The události kanálu ASP.NET pro k ověření uživatele při použití ověřování pomocí formulářů a rozhraní role ([kliknutím ji zobrazíte obrázek v plné velikosti](role-based-authorization-cs/_static/image3.png))


### <a name="caching-role-information-in-a-cookie"></a>Ukládání do mezipaměti informace o rolích do souboru cookie

`RolePrincipal` Objektu `IsInRole(roleName)` volání metody `Roles.GetRolesForUser` získat role pro uživatele, aby bylo možné zjistit, jestli je uživatel členem *roleName*. Při použití `SqlRoleProvider`, výsledkem dotazu do databáze úložiště rolí. Při použití autorizačních pravidel adres URL na základě rolí `RolePrincipal`společnosti `IsInRole` metoda se volá u každého požadavku na stránku, který je chráněný pomocí autorizačních pravidel adres URL na základě rolí. A nenechat vyhledat informace o rolích v databázi u každého požadavku, framework role obsahuje možnost pro ukládání do mezipaměti role uživatele v souboru cookie.

Pokud role framework je nakonfigurovaný pro ukládání do mezipaměti role uživatele do souboru cookie `RoleManagerModule` během profilace ASP.NET vytvoří soubor cookie [ `EndRequest` události](https://msdn.microsoft.com/library/system.web.httpapplication.endrequest.aspx). Tento soubor cookie se používá v následné žádosti v `PostAuthenticateRequest`, což je, když `RolePrincipal` je vytvořen objekt. Pokud má soubor cookie platný a nevypršela, data v souboru cookie, který je analyzovat a použitých k naplnění rolí uživatele, a tím ukládání `RolePrincipal` nebudou muset provést volání do `Roles` třídu k určení rolí uživatele. Obrázek 2 znázorňuje tento pracovní postup.


[![Informace o rolích uživatele mohou být uloženy do souboru cookie kvůli zvýšení výkonu](role-based-authorization-cs/_static/image5.png)](role-based-authorization-cs/_static/image4.png)

**Obrázek 2**: pole uživatelské Role informace mohou být uloženy do souboru cookie ke zlepšení výkonu ([kliknutím ji zobrazíte obrázek v plné velikosti](role-based-authorization-cs/_static/image6.png))


Ve výchozím nastavení je zakázána mechanismus roli mezipaměti souborů cookie. Povolit prostřednictvím `<roleManager>` konfigurace značek v `Web.config`. Jsme probírali pomocí [ `<roleManager>` element](https://msdn.microsoft.com/library/ms164660.aspx) určit zprostředkovatele rolí v <a id="_msoanchor_4"> </a> [ *vytváření a Správa rolí* ](creating-and-managing-roles-cs.md) kurzu Proto byste už měli mít tento element ve vaší aplikaci `Web.config` souboru. Nastavení mezipaměti souborů cookie role jsou zadané jako atributy `<roleManager>` element a jsou shrnutá v tabulce 1.

> [!NOTE]
> Konfigurace nastavení uvedená v tabulce 1 zadejte vlastnosti výsledného souboru cookie roli mezipaměti. Další informace o souborech cookie, jak fungují a jejich různé vlastnosti čtení [v tomto kurzu soubory cookie](http://www.quirksmode.org/js/cookies.html).


| <strong>Vlastnost</strong> |                                                                                                                                                                                                                                                                                                                                                         <strong>Popis</strong>                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   `cacheRolesInCookie`    |                                                                                                                                                                                                                                                                                                                              Logická hodnota, která určuje, jestli se používá ukládání do mezipaměti souborů cookie. Výchozí hodnota je `false`.                                                                                                                                                                                                                                                                                                                              |
|       `cookieName`        |                                                                                                                                                                                                                                                                                                                                     Název souboru cookie, roli mezipaměti. Výchozí hodnota je ". ASPXROLES".                                                                                                                                                                                                                                                                                                                                     |
|       `cookiePath`        |                                                                                                                                                                                                                                Cesta k souboru cookie název role. Atribut cesty umožňuje vývojáři omezit obor souborů cookie do konkrétního adresářového hierarchie. Výchozí hodnota je "/", který informuje prohlížeče k odeslání ověřovacího lístku souboru cookie na všechny žádosti do domény.                                                                                                                                                                                                                                 |
|    `cookieProtection`     |                                                                                                                                                               Určuje, jaké postupy se používají k ochraně souborů cookie roli mezipaměti. Povolené hodnoty jsou: `All` (výchozí); `Encryption`; `None`; a `Validation`. Vraťte se ke kroku 3 <a id="_anchor_5"> </a> [ *konfigurace ověřování formulářů a témata pokročilé* ](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) kurz pro další informace o těchto úrovních ochrany.                                                                                                                                                                |
|    `cookieRequireSSL`     |                                                                                                                                                                                                                                                                                                   Logická hodnota, která určuje, jestli je k přenosu ověřovacího souboru cookie vyžaduje připojení SSL. Výchozí hodnota je `false`.                                                                                                                                                                                                                                                                                                   |
| `cookieSlidingExpiration` |                                                                                                                                                                                                                                                  Logická hodnota určující, že zda soubor cookie vypršení časového limitu se vynuluje pokaždé, když uživatel navštíví web během jedné relace. Výchozí hodnota je `false`. Tato hodnota je pouze relevantní při `createPersistentCookie` je nastavena na `true`.                                                                                                                                                                                                                                                  |
|      `cookieTimeout`      |                                                                                                                                                                                                                                                                         Určuje dobu v minutách, po jejichž uplynutí vyprší platnost ověřovacího lístku souboru cookie. Výchozí hodnota je `30`. Tato hodnota je pouze relevantní při `createPersistentCookie` je nastavena na `true`.                                                                                                                                                                                                                                                                         |
| `createPersistentCookie`  |                                                                                                                                                                   Logická hodnota určující, zda je soubor cookie roli mezipaměti souborů cookie relace nebo trvalého souboru cookie. Pokud `false` (výchozí), souboru cookie relace se používá, který je odstraněný při zavření prohlížeče. Pokud `true`, trvalý soubor cookie je používaný; jeho platnost vyprší `cookieTimeout` počet minut po vytvoření nebo po předchozí návštěvě závisí na hodnotě `cookieSlidingExpiration`.                                                                                                                                                                    |
|         `domain`          |                                                                                                                                                 Určuje hodnotu domény souboru cookie. Výchozí hodnota je prázdný řetězec, který způsobí, že prohlížeč doménu, ve kterém byl vydán (například www.yourdomain.com). V takovém případě bude soubor cookie <strong>není</strong> odešlou při posílání požadavků na subdomén, třeba admin.yourdomain.com. Pokud chcete soubor cookie, které se mají předat všechny subdomény, budete muset přizpůsobit `domain` atribut ji nastavíte na "vasedomena.com".                                                                                                                                                 |
|    `maxCachedResults`     | Určuje maximální počet názvů rolí, které jsou uložené v mezipaměti v souboru cookie. Výchozí hodnota je 25. `RoleManagerModule` Nevytváří soubor cookie pro uživatele, kteří patří do více než `maxCachedResults` role. V důsledku toho `RolePrincipal` objektu `IsInRole` metoda použije `Roles` třídu k určení rolí uživatele. Z důvodu `maxCachedResults` existuje totiž mnoho Uživatelští agenti nepovoluje soubory cookie, které jsou větší než 4 096 bajtů. Proto tento limit smyslem je snížit pravděpodobnost, že překročení tohoto omezení velikosti. Pokud máte názvy extrémně dlouhou rolí, můžete chtít zvažte zadání menší `maxCachedResults` hodnota; contrariwise, pokud máte role neuvěřitelně krátké názvy, můžete pravděpodobně zvýšíte tuto hodnotu. |

**Tabulka 1:** možnosti konfigurace Role mezipaměti souborů Cookie

Nakonfigurujeme aplikace používají soubory cookie dočasné roli mezipaměti. Provedete to tak, aktualizujte `<roleManager>` element v `Web.config` zahrnout následující atributy vztahující se k souboru cookie:

[!code-xml[Main](role-based-authorization-cs/samples/sample1.xml)]

Aktualizoval(a) jsem `<roleManager>` element tak, že přidáte tři atributy: `cacheRolesInCookie`, `createPersistentCookie`, a `cookieProtection`. Nastavením `cacheRolesInCookie` k `true`, `RoleManagerModule` bude nyní automaticky ukládat do mezipaměti role uživatele v souboru cookie, namísto nutnosti vyhledat informace o rolích uživatele s každým požadavkem. Explicitně nastavit `createPersistentCookie` a `cookieProtection` atributů `false` a `All`v uvedeném pořadí. Technicky vzato nebyla potřeba zadat hodnoty pro tyto atributy, protože jsem právě mu přiřadil/a na jejich výchozí hodnoty, ale jsem je sem zadejte provést explicitně vymazat nepoužívám trvalé soubory cookie a, že je soubor cookie obě zašifrované a ověřit.

To je všechno je to! Rozhraní role se od nynějška, ukládat do mezipaměti rolí uživatelů v souborech cookie. Pokud uživatele prohlížeč nepodporuje soubory cookie, nebo pokud se jejich soubory cookie jsou odstraněny nebo ztráty, nějakým způsobem, bez důležitá záležitost – `RolePrincipal` objektu bude jednoduše použít `Roles` třídy v případě, že je k dispozici žádný soubor cookie (nebo neplatná nebo vypršela její platnost jeden).

> [!NOTE]
> Microsoftu pro vzory &amp; odrazuje skupiny postupy od používání souborů cookie trvalý roli mezipaměti. Vzhledem k tomu, že vlastníkem daného souboru cookie mezipaměti roli je dostatečná prokázat, že členství v roli, pokud se hacker nějakým způsobem můžete získat přístup k souboru cookie platné uživatelské mohl zosobnit uživatele. Pravděpodobnost takových případů zvětší, když je soubor cookie se ukládají v prohlížeči uživatele. Další informace o toto doporučení zabezpečení, stejně jako ostatní otázky zabezpečení, najdete [seznam otázek zabezpečení pro technologii ASP.NET 2.0](https://msdn.microsoft.com/library/ms998375.aspx).


## <a name="step-1-defining-role-based-url-authorization-rules"></a>Krok 1: Definování autorizačních pravidel adres URL na základě rolí

Jak je popsáno v <a id="_msoanchor_6"> </a> [ *autorizace na základě uživatele* ](../membership/user-based-authorization-cs.md) kurzu autorizace adres URL nabízí prostředky pro omezení přístupu k sadě stránek na uživatele podle uživatele nebo roli role základ. Adresa URL autorizační pravidla jsou vypsány v `Web.config` pomocí [ `<authorization>` element](https://msdn.microsoft.com/library/8d82143t.aspx) s `<allow>` a `<deny>` podřízené prvky. Kromě související s uživateli autorizačních pravidel popsaných v předchozích kurzech se každý `<allow>` a `<deny>` může také obsahovat podřízený element:

- Konkrétní roli
- Seznam rolí oddělených čárkou

Například autorizačních pravidel adres URL udělit přístup pro tyto uživatele do role správců a správců, ale odepření přístupu pro všechny ostatní:

[!code-xml[Main](role-based-authorization-cs/samples/sample2.xml)]

`<allow>` Element ve značce výše uvádí, že jsou povoleny role správců a správců `<deny>` element, který instruuje *všechny* uživatelé mají odepřený.

Nakonfigurujeme naši aplikaci tak, aby `ManageRoles.aspx`, `UsersAndRoles.aspx`, a `CreateUserWizardWithRoles.aspx` stránky jsou přístupné pro tyto uživatele v roli správce, zatímco `RoleBasedAuthorization.aspx` stránky zůstala přístupná pro všechny návštěvníky.

K tomu, začněte přidáním `Web.config` do souboru `Roles` složky.


[![Přidat soubor Web.config pro role adresáře](role-based-authorization-cs/_static/image8.png)](role-based-authorization-cs/_static/image7.png)

**Obrázek 3**: Přidejte `Web.config` do souboru `Roles` adresáře ([kliknutím ji zobrazíte obrázek v plné velikosti](role-based-authorization-cs/_static/image9.png))


V dalším kroku přidejte následující kód do konfigurace `Web.config`:

[!code-xml[Main](role-based-authorization-cs/samples/sample3.xml)]

`<authorization>` Prvek `<system.web>` části označuje, že jenom uživatelé v roli správce může přístup k prostředkům technologie ASP.NET v `Roles` adresáře. `<location>` Element definuje alternativní sadu autorizačních pravidel adres URL pro `RoleBasedAuthorization.aspx` stránky, umožňuje všem uživatelům, najdete na stránce.

Po uložení změn `Web.config`, přihlaste se jako uživatel, který se nenachází v roli správce a poté na některém z chráněných stránky. `UrlAuthorizationModule` Zjistí, že nemáte oprávnění k navštívení požadovaný prostředek; v důsledku toho `FormsAuthenticationModule` přesměruje vás na přihlašovací stránku. Na stránce přihlášení vás pak přesměruje na `UnauthorizedAccess.aspx` stránky (viz obrázek 4). Tento poslední přesměrování z přihlašovací stránky k `UnauthorizedAccess.aspx` nastává z důvodu kódu jsme přidali na stránku pro přihlášení v kroku 2 <a id="_msoanchor_7"> </a> [ *autorizace na základě uživatele* ](../membership/user-based-authorization-cs.md) kurzu. Konkrétně se na přihlašovací stránku automaticky přesměruje všem ověřeným uživatelům `UnauthorizedAccess.aspx` pokud obsahuje řetězec dotazu `ReturnUrl` parametr, jako tento parametr určuje, že uživatel dorazily na přihlašovací stránku po pokusu o zobrazení stránky, pracoval bruno není oprávnění k zobrazení.


[![Jenom uživatelé v roli správce můžete zobrazit chráněné stránky](role-based-authorization-cs/_static/image11.png)](role-based-authorization-cs/_static/image10.png)

**Obrázek 4**: jenom uživatelé v roli správce můžete zobrazit na stránkách chráněné ([kliknutím ji zobrazíte obrázek v plné velikosti](role-based-authorization-cs/_static/image12.png))


Odhlásit a pak se přihlaste jako uživatel, který je v roli správce. Teď byste měli moct zobrazit tři stránky chráněné.


[![Navštívit tito UsersAndRoles.aspx stránce vzhledem k tomu, že je v roli správce](role-based-authorization-cs/_static/image14.png)](role-based-authorization-cs/_static/image13.png)

**Obrázek 5**: Tito najdete `UsersAndRoles.aspx` stránce vzhledem k tomu, že je v roli správce ([kliknutím ji zobrazíte obrázek v plné velikosti](role-based-authorization-cs/_static/image15.png))


> [!NOTE]
> Při zadávání autorizačních pravidel adres URL – rolí nebo uživatelů – je důležité mít na paměti, že pravidla jsou analyzované jeden po druhém, shora dolů. Jakmile se najde shoda, je uživatel povolen nebo odepřen přístup, v závislosti na if v byla nalezena shoda `<allow>` nebo `<deny>` elementu. **Pokud není nalezena žádná shoda, uživateli je udělen přístup.** V důsledku toho pokud chcete omezit přístup k jedné nebo více uživatelských účtů, je nutné použít `<deny>` prvek jako poslední prvek v konfiguraci autorizace adresy URL. **Pokud neobsahují autorizačních pravidel adres URL**`<deny>`**elementu, všichni uživatelé budou mít přístup.** Podrobnější informace o tom, jak se analyzují autorizačních pravidel adres URL, vraťte se do "podívat, jak `UrlAuthorizationModule` používá autorizační pravidla udělit nebo odepřít přístup" část <a id="_msoanchor_8"> </a> [  *Autorizace na základě uživatele* ](../membership/user-based-authorization-cs.md) kurzu.


## <a name="step-2-limiting-functionality-based-on-the-currently-logged-in-users-roles"></a>Krok 2: Omezení funkcí na základě rolí pro aktuálně přihlášeného uživatele

Adresa URL autorizační umožňuje snadno určit hrubý autorizační pravidla tohoto stavu jaké identit jsou povolené a které z nich byl odepřen v zobrazení konkrétní stránky (nebo všech stránek ve složce a jejích podsložkách). Ale v některých případech může Chceme všem uživatelům najdete na stránce, ale na stránce funkce na základě rolí hostujícími uživatelů omezit. To může mít za následek zobrazení nebo skrytí dat na základě role uživatele a nabízí další funkce, které uživatelům patřícím do určité role.

Tato pravidla ověřování na základě role nich spočívá v jemné je možné implementovat buď deklarativně nebo prostřednictvím kódu programu (nebo pomocí kombinace obou). V další části uvidíme, jak implementovat deklarativní nich spočívá v jemné autorizace pomocí ovládacího prvku LoginView. Pod se podíváme na programový techniky. Předtím, než abychom se mohli podívat na použití nich spočívá v jemné autorizační pravidla, ale nejprve musíme vytvořit stránku, jehož funkce závisí na roli uživatele na jeho.

Pojďme vytvořit stránku, která obsahuje seznam všech uživatelských účtů v systému v GridView. Uživatelské jméno, e-mailovou adresu, datum posledního přihlášení a připomínky uživatele každý uživatel, bude obsahovat prvku GridView. Kromě zobrazování informací každý uživatel, bude prvku GridView. Zahrnout úpravy a odstranění funkce. Zpočátku budeme vytvořit tuto stránku pomocí úpravy a odstranění k dispozici všem uživatelům. V části "Použití ovládací prvek zobrazení přihlášení" a "Programově omezení funkce" uvidíme, jak povolit nebo zakázat tyto funkce na základě hostujícími uživatelské role.

> [!NOTE]
> Stránky ASP.NET, které se chystáte sestavení používá k zobrazení uživatelských účtů ovládacím prvku GridView. Od tohoto kurzu, kterou řada se zaměřuje na ověřování pomocí formulářů, autorizace, uživatelských účtů a rolí nechci strávit příliš mnoho času diskuze o vnitřní fungování ovládacího prvku GridView. Tento kurz poskytuje konkrétní podrobné pokyny pro nastavení této stránky, ne delve podrobnosti, proč byly provedeny některé možnosti, nebo konkrétní vlastnosti vliv mají na vykresleného výstupu. Pro důkladné přezkoumání ovládacího prvku GridView, podívejte se na můj *[pracovat s daty v ASP.NET 2.0](../../data-access/index.md)* série kurzů.


Začněte otevřením `RoleBasedAuthorization.aspx` stránku `Roles` složky. Přetáhněte GridView ze stránky do návrháře a nastavte jeho `ID` k `UserGrid`. Za chvíli budeme psát kód, který volá `Membership.GetAllUsers` metoda a sváže s výsledný `MembershipUserCollection` objektu do prvku GridView. `MembershipUserCollection` Obsahuje `MembershipUser` objekt pro každý uživatelský účet v systému. `MembershipUser` objekty mají vlastnosti, jako je `UserName`, `Email`, `LastLoginDate`a tak dále.

Předtím, než jsme napsali kód, který váže uživatelské účty do mřížky, Pojďme nejdříve definovat pole prvku GridView. Inteligentní značky prvku GridView, klikněte na odkaz "Upravit sloupce" Spustit dialogové okno pole (viz obrázek 6). Z tohoto místa, zrušte zaškrtnutí políčka "Automatické generování pole" v levém dolním rohu. Protože chceme, aby tento GridView zahrnout úpravy a odstranění možnosti, přidejte CommandField a nastavit jeho `ShowEditButton` a `ShowDeleteButton` vlastnosti na hodnotu True. V dalším kroku přidejte čtyři pole pro zobrazení `UserName`, `Email`, `LastLoginDate`, a `Comment` vlastnosti. Použít vlastnost BoundField pro dvě vlastnosti jen pro čtení (`UserName` a `LastLoginDate`) a vlastností TemplateField v obou polích upravitelné (`Email` a `Comment`).

Mít první zobrazení Vlastnost BoundField `UserName` vlastnost; nastavte jeho `HeaderText` a `DataField` vlastnosti "UserName". Toto pole možné upravovat, takže nastavte jeho `ReadOnly` vlastnost na hodnotu True. Konfigurace `LastLoginDate` Vlastnost BoundField nastavením jeho `HeaderText` na "Poslední přihlášení" a jeho `DataField` na "LastLoginDate". Pojďme formátovat výstup tuto vlastnost BoundField tak, aby se zobrazí pouze datum (namísto datum a čas). Chcete-li to provést, nastavte tuto vlastnost BoundField `HtmlEncode` vlastnost na hodnotu False a jeho `DataFormatString` vlastnost "{0:d}". Také nastavit `ReadOnly` vlastnost na hodnotu True.

Nastavte `HeaderText` vlastnosti dvou vlastností TemplateField "Email" a "Komentář".


[![Pole prvku GridView je možné nakonfigurovat pomocí pole dialogových oken](role-based-authorization-cs/_static/image17.png)](role-based-authorization-cs/_static/image16.png)

**Obrázek 6**: prvku GridView pole může být nakonfigurované přes the dialogové okno pole ([kliknutím ji zobrazíte obrázek v plné velikosti](role-based-authorization-cs/_static/image18.png))


Nyní potřebujeme k definování `ItemTemplate` a `EditItemTemplate` "Email" a "Komentář" vlastností TemplateField. Přidání ovládacího prvku popisku k jednotlivým `ItemTemplate` s a vazby jejich `Text` vlastnosti, které chcete `Email` a `Comment` vlastnosti, v uvedeném pořadí.

TemplateField "Email" přidat textové pole s názvem `Email` k jeho `EditItemTemplate` a vytvořit vazbu jeho `Text` vlastnost `Email` vlastnost pomocí dvousměrnou datovou vazbou. Přidání RequiredFieldValidator a RegularExpressionValidator k `EditItemTemplate` zajistit, že návštěvník úpravou vlastnosti e-mailu zadal platnou e-mailovou adresu. TemplateField "Komentář" přidat ve víceřádkovém textovém poli s názvem `Comment` k jeho `EditItemTemplate`. Nastavit textové pole `Columns` a `Rows` vlastností 40 a 4, v uvedeném pořadí a pak vytvoříte vazbu jeho `Text` vlastnost `Comment` vlastnost pomocí dvousměrnou datovou vazbou.

Po dokončení konfigurace těchto vlastností TemplateField, jejich deklarativní by měl vypadat nějak takto:

[!code-aspx[Main](role-based-authorization-cs/samples/sample4.aspx)]

Při úpravách nebo odstranění uživatelského účtu, potřebujeme vědět, že uživatel `UserName` hodnotu vlastnosti. Nastavte prvku GridView `DataKeyNames` vlastnost "UserName" tak, aby tyto informace jsou k dispozici prostřednictvím prvku GridView `DataKeys` kolekce.

Nakonec přidejte ovládací prvek souhrnu ověření na stránku a nastavte jeho `ShowMessageBox` vlastnost na hodnotu True a vlastnost `ShowSummary` vlastnost na hodnotu False. S těmito nastaveními se souhrnu ověření zobrazí upozornění na straně klienta, pokud uživatel se pokusí upravit uživatelský účet s chybějící nebo neplatná e-mailovou adresu.

[!code-aspx[Main](role-based-authorization-cs/samples/sample5.aspx)]

Nyní jsme dokončili deklarativní tuto stránku. Naše dalším úkolem je sada uživatelských účtů svázání prvku GridView. Přidat metodu s názvem `BindUserGrid` k `RoleBasedAuthorization.aspx` použití modelu code-behind třídy stránky, která vytvoří vazbu `MembershipUserCollection` vrácený `Membership.GetAllUsers` k `UserGrid` ovládacího prvku GridView. Volejte tuto metodu z `Page_Load` obslužné rutiny události při první návštěvě stránky.

[!code-csharp[Main](role-based-authorization-cs/samples/sample6.cs)]

S tímto kódem na místě navštivte stránku prostřednictvím prohlížeče. Jak je vidět na obrázku 7, měli byste vidět GridView výpis informace o jednotlivých uživatelských účtů v systému.


[![UserGrid GridView uvádí informace o jednotlivých uživatelů v systému](role-based-authorization-cs/_static/image20.png)](role-based-authorization-cs/_static/image19.png)

**Obrázek 7**: `UserGrid` GridView uvádí informace o každý uživatel v systému ([kliknutím ji zobrazíte obrázek v plné velikosti](role-based-authorization-cs/_static/image21.png))


> [!NOTE]
> `UserGrid` GridView obsahuje seznam všech uživatelů v nestránkovaném rozhraní. Toto rozhraní jednoduché tabulky není vhodné pro scénáře, kdy existuje několik desítek nebo více uživatelů. Jednou z možností je nakonfigurovat pro povolení stránkování prvku GridView. `Membership.GetAllUsers` Metoda má dvě přetížení: jednu, která nepřijímá žádné vstupní parametry a vrátí všechny uživatele a jednu, která přijímá celočíselné hodnoty pro index stránky a velikost stránky a vrátí pouze zadaný dílčí sadu uživatelů. Druhé přetížení je možné efektivněji stránkovat uživatele vzhledem k tomu, že vrátí jenom na přesné podmnožinu uživatelské účty spíše než *všechny* z nich. Pokud máte tisíce uživatelské účty, můžete chtít zvážit filtru rozhraní, ten, který se zobrazí pouze tito uživatelé, jejichž uživatelské jméno začíná znakem vybrané pro instanci. [ `Membership.FindUsersByName method` ](https://msdn.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) Je ideální pro vytvoření filtru na základě uživatelského rozhraní. Podíváme se na vytváření takových rozhraní v budoucích kurzech.


Ovládací prvek GridView nabízí integrovanou úpravy a odstranění podpory, když je ovládací prvek vázán na správně nakonfigurovaných data správy zdrojového kódu, jako je například SqlDataSource nebo prvku ObjectDataSource. `UserGrid` Prvku GridView, má ale jeho data prostřednictvím kódu programu vázaná; proto jsme musíte napsat kód k provedení těchto dvou úloh. Konkrétně budeme muset vytváření obslužných rutin událostí pro prvku GridView `RowEditing`, `RowCancelingEdit`, `RowUpdating`, a `RowDeleting` události, které jsou aktivovány, pokud návštěvník prvku GridView klikne na tlačítko Upravit, zrušení, aktualizaci, nebo odstranění tlačítka.

Začněte tím, že vytváření obslužných rutin událostí pro prvku GridView `RowEditing`, `RowCancelingEdit`, a `RowUpdating` události a poté přidejte následující kód:

[!code-csharp[Main](role-based-authorization-cs/samples/sample7.cs)]

`RowEditing` a `RowCancelingEdit` obslužné rutiny událostí stačí nastavit prvku GridView `EditIndex` vlastnost a potom obnovení vazby seznam uživatelské účty do mřížky. Probíhá zajímavé věci `RowUpdating` obslužné rutiny události. Tato obslužná rutina události začíná tím, že zajišťuje, že data jsou platná a pak vezme `UserName` hodnotu upravených uživatelského účtu z `DataKeys` kolekce. `Email` a `Comment` textová pole v obou vlastností TemplateField `EditItemTemplate` s jsou pak prostřednictvím kódu programu odkazuje. Jejich `Text` vlastnosti obsahovat upravená e-mailovou adresu a komentář.

Pokud chcete aktualizovat účet uživatele prostřednictvím rozhraní API pro členství, musíme nejdřív získat informace o uživateli, které děláme prostřednictvím volání `Membership.GetUser(userName)`. Vrácený `MembershipUser` objektu `Email` a `Comment` vlastnosti jsou potom aktualizovány hodnoty zadané do dvou textových polí rozhraní pro úpravy. Nakonec se tyto změny jsou uloženy voláním [ `Membership.UpdateUser` ](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx). `RowUpdating` Obslužná rutina události dokončení návratem GridView předem úpravy rozhraní.

Dále vytvořte `RowDeleting` obslužné rutiny události a poté přidejte následující kód:

[!code-csharp[Main](role-based-authorization-cs/samples/sample8.cs)]

Výše obslužná rutina události začne zachytávat `UserName` hodnotu z prvku GridView `DataKeys` kolekce; tato `UserName` hodnota je pak předán do třídy členství [ `DeleteUser` metoda](https://msdn.microsoft.com/library/system.web.security.membership.deleteuser.aspx). `DeleteUser` Metoda odstraní účet uživatele ze systému, včetně dat o členství související (jako je například jaké role tohoto uživatele patří do). Po odstranění uživatele, mřížky `EditIndex` je nastavena na hodnotu -1 (v v případě, že uživatel klikl na odstranění při další řádek v režimu úprav) a `BindUserGrid` metoda je volána.

> [!NOTE]
> Tlačítko Odstranit nevyžaduje, aby jakýkoli druh potvrzení od uživatele před odstraněním účtu uživatele. Můžu vám doporučujeme přidat nějakou formu potvrzení uživatele Chcete-li snížit riziko náhodnému odstranění účtu. Jedním z nejjednodušších způsobů k potvrzení akce je prostřednictvím dialogového okna potvrdit na straně klienta. Další informace o této techniky najdete v tématu [přidání Client-Side potvrzení při odstraňování](https://asp.net/learn/data-access/tutorial-42-cs.aspx).


Ověřte, že tato stránka funguje podle očekávání. Je třeba moct upravovat libovolný uživatel e-mailovou adresu a komentáře, jakož i odstranit libovolný uživatelský účet. Vzhledem k tomu, `RoleBasedAuthorization.aspx` stránka je přístupný pro všechny uživatele, každý uživatel – i anonymní návštěvníků – můžete tuto stránku a upravovat a odstraňovat uživatelské účty! Umožňuje aktualizovat tuto stránku tak, že jenom uživatelé v rolích správců a správců můžete upravit uživatele e-mailovou adresu a přidejte komentář a pouze správci mohou odstranit uživatelský účet.

V části "Použití ovládací prvek zobrazení přihlášení" zjistí pomocí ovládacího prvku LoginView zobrazíte pokyny specifické pro roli uživatele. Pokud uživatel v roli správce navštíví tuto stránku, vám ukážeme pokyny o tom, jak upravit a odstranit uživatele. Pokud uživatel do role správců dosáhne této stránky, budeme se zobrazí pokyny na úpravy uživatelů. A pokud návštěvníka je anonymní, nebo se nenachází v roli správci nebo správci, zobrazí se zpráva s vysvětlením, že se nedají upravit ani odstranit informace o uživatelském účtu. V části "Programově omezení funkce" napíšeme kód, který se prostřednictvím kódu programu zobrazí nebo skryje tlačítko Upravit a odstranit na základě role uživatele.

### <a name="using-the-loginview-control"></a>Použití ovládacího prvku LoginView

Jak jsme viděli v posledních kurzy, ovládacího prvku LoginView je užitečné pro zobrazení různých rozhraní pro ověřený a anonymní uživatele, ale ovládacího prvku LoginView lze také použít k zobrazení různých značek na základě rolí uživatele. Chcete-li zobrazit různé pokyny na základě role uživatele hostujícími použijeme ovládací prvek zobrazení přihlášení.

Začněte tím, že přidáte LoginView výše `UserGrid` ovládacího prvku GridView. Jak jsme jsme probírali dříve, ovládacího prvku LoginView má dvě předdefinované šablony: `AnonymousTemplate` a `LoggedInTemplate`. Zadejte zprávu (BRIEF) v obou těchto šablon, které informuje uživatele, že se nedají upravit ani odstranit všechny informace o uživateli.

[!code-aspx[Main](role-based-authorization-cs/samples/sample9.aspx)]

Kromě `AnonymousTemplate` a `LoggedInTemplate`, mohou zahrnovat ovládacího prvku LoginView *kolekci RoleGroups*, které jsou specifické pro role šablony. Každý RoleGroup obsahuje jedinou vlastnost `Roles`, která určuje, jaké role RoleGroup platí pro. `Roles` Vlastnost lze nastavit do jedné role (jako je "správce") nebo čárkami oddělený seznam rolí (například "správci, správci").

Ke správě kolekci RoleGroups, klikněte na odkaz "Upravit kolekci RoleGroups" z inteligentní značky ovládacího prvku zpřístupnit nahoru Editor kolekce RoleGroup. Přidejte dva nové kolekci RoleGroups. Nastavte první RoleGroup `Roles` vlastnost "Administrators" a na "Nadřízeným" druhé.


[![Správa šablon specifické pro Role LoginView prostřednictvím Editor kolekce RoleGroup.](role-based-authorization-cs/_static/image23.png)](role-based-authorization-cs/_static/image22.png)

**Obrázek 8**: Správa LoginView pro konkrétní Role šablony prostřednictvím the Editor kolekce RoleGroup. ([kliknutím ji zobrazíte obrázek v plné velikosti](role-based-authorization-cs/_static/image24.png))


Kliknutím na OK zavřete Editor kolekce RoleGroup.; Tím se aktualizuje LoginView deklarativní zahrnout `<RoleGroups>` části s `<asp:RoleGroup>` podřízený element pro každé RoleGroup definované v Editor kolekce RoleGroup. Navíc "Zobrazení" rozevírací seznam v inteligentních značek LoginView – které původně uvedeny jenom `AnonymousTemplate` a `LoggedInTemplate` – teď zahrnuje přidání kolekci RoleGroups také.

Upravte kolekci RoleGroups tak, aby uživatelé v roli Vedoucí zobrazené pokyny o tom, jak upravit uživatelské účty, zatímco uživatelé v roli správce jsou uvedeny pokyny pro úpravy a odstranění. Po provedení těchto změn, vaše LoginView deklarativní by měl vypadat nějak takto.

[!code-aspx[Main](role-based-authorization-cs/samples/sample10.aspx)]

Po provedení těchto změn, uložit na stránku a potom navštivte pomocí prohlížeče. Nejprve najdete na stránce jako anonymní uživatel. By měla zobrazit zpráva, "jste se přihlásili do systému. Proto se nedají upravit ani odstranit všechny informace o uživateli." Pak se přihlaste jako ověřený uživatel, ale ten, který není ani do role správců ani správci. Tentokrát je by se zobrazit zpráva "nejste členem skupiny správců nebo správců rolí. Proto se nedají upravit ani odstranit všechny informace o uživateli."

V dalším kroku přihlaste jako uživatel, který je členem role správců. Tentokrát by se měla zobrazit konkrétní role správců zprávy (viz obrázek 9). A pokud jste přihlášení jako uživatel v roli, měli byste vidět konkrétní role Správci zprávy (viz obrázek 10) správců.


[![Bruce se zobrazí zpráva konkrétní Role správců](role-based-authorization-cs/_static/image26.png)](role-based-authorization-cs/_static/image25.png)

**Obrázek 9**: Bruce se zobrazí zpráva konkrétní Role správců ([kliknutím ji zobrazíte obrázek v plné velikosti](role-based-authorization-cs/_static/image27.png))


[![Tito se zobrazí zprávy specifické pro roli správce](role-based-authorization-cs/_static/image29.png)](role-based-authorization-cs/_static/image28.png)

**Obrázek 10**: Tito se zobrazí zprávy specifické pro roli správce ([kliknutím ji zobrazíte obrázek v plné velikosti](role-based-authorization-cs/_static/image30.png))


Jako snímky obrazovky v obrázcích 9 a 10 zobrazit vykreslí LoginView pouze jedna šablona i v případě, že platí více šablon. Bruce a Tito i přihlášení uživatele, ale LoginView vykreslí pouze odpovídající RoleGroup a ne `LoggedInTemplate`. Kromě toho Tito patří do role správců a správců, ale ovládacího prvku LoginView vykreslí šablon specifické pro roli správci místo vedoucí jednoho.

Obrázek 11 znázorňuje použit v ovládacím prvku LoginView stanovit, jakou šablonu k vykreslení pracovního postupu. Všimněte si, že pokud je více než jeden zadaný RoleGroup šablony LoginView vykreslí *první* RoleGroup, který odpovídá. Jinými slovy Pokud jsme měli umístit RoleGroup nadřízeným jako první RoleGroup a správci jako druhý, pak když Tito uživatel tuto stránku mu zobrazoval se vám zpráva správců.


[![Ovládacího prvku LoginView pracovního postupu pro určení, jaké šablony pro vykreslení](role-based-authorization-cs/_static/image32.png)](role-based-authorization-cs/_static/image31.png)

**Obrázek 11**: ovládací prvek zobrazení přihlášení pracovního postupu pro určení, co šablona pro vykreslení ([kliknutím ji zobrazíte obrázek v plné velikosti](role-based-authorization-cs/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>Programová omezení funkcí

Zatímco ovládacího prvku LoginView zobrazí různé pokyny na základě role uživatele na stránce, úpravy a tlačítka Storno zůstávají viditelné pro všechny. Budeme potřebovat programově skrýt tlačítka Edit a Delete pro anonymní uživatelé a uživatelé, kteří jsou v role správců ani správci. Musíme skrýt tlačítko Odstranit pro každého uživatele, který nemá oprávnění správce. Aby to bylo možné napíšeme hodně kód, který programově odkazuje CommandField upravit a odstranit LinkButtons a nastaví jejich `Visible` vlastností `false`, v případě potřeby.

Nejjednodušší způsob, jak prostřednictvím kódu programu odkazovat na ovládací prvky CommandField je nejprve převeďte do šablony. K tomu, klikněte na odkaz "Upravit sloupce" z inteligentních značek v prvku GridView, vyberte CommandField ze seznamu aktuálního pole a klikněte na odkaz "Převést toto pole TemplateField". Tím se změní CommandField TemplateField s `ItemTemplate` a `EditItemTemplate`. `ItemTemplate` Obsahuje úpravy a odstranění LinkButtons při `EditItemTemplate` jsou uloženy aktualizace a LinkButtons zrušit.


[![Převést CommandField TemplateField](role-based-authorization-cs/_static/image35.png)](role-based-authorization-cs/_static/image34.png)

**Obrázek 12**: převést CommandField do TemplateField ([kliknutím ji zobrazíte obrázek v plné velikosti](role-based-authorization-cs/_static/image36.png))


Upravit a odstranit LinkButtons v aktualizaci `ItemTemplate`a nastavte jejich `ID` vlastnosti a hodnoty `EditButton` a `DeleteButton`v uvedeném pořadí.

[!code-aspx[Main](role-based-authorization-cs/samples/sample11.aspx)]

Pokaždé, když vázaná na prvku GridView, prvku GridView zobrazí záznamy v jeho `DataSource` vlastnost a vygeneruje odpovídající `GridViewRow` objektu. Protože každému `GridViewRow` je vytvořen objekt, `RowCreated` událost se aktivuje. Aby bylo možné skrýt tlačítka Upravit a odstranit neoprávnění uživatelé, musíme vytvořit obslužnou rutinu události pro tuto událost a programově odkazovaly na Upravit a odstranit LinkButtons nastavení jejich `Visible` vlastnosti odpovídajícím způsobem.

Vytvořte obslužnou rutinu události `RowCreated` události a poté přidejte následující kód:

[!code-csharp[Main](role-based-authorization-cs/samples/sample12.cs)]

Mějte na paměti, která `RowCreated` událost aktivuje pro *všechny* řádků ovládacího prvku GridView, včetně záhlaví, zápatí, stránkování rozhraní a tak dále. Chceme jenom prostřednictvím kódu programu odkazu upravit a odstranit LinkButtons, pokud jsme pracovali s řádek dat není v režimu úprav (protože odpovídající řádek v režimu úprav má aktualizace a zrušit tlačítka místo upravit a odstranit). Tato kontrola se zpracovává souborem `if` příkazu.

Pokud jsme pracovali s řádek dat, který není v režimu úprav, upravit a odstranit LinkButtons odkazují a jejich `Visible` vlastnosti jsou nastaveny na základě logické hodnoty vrácené `User` objektu `IsInRole(roleName)` metody. Objekt uživatele odkazuje na objekt zabezpečení vytvořené `RoleManagerModule`; v důsledku toho `IsInRole(roleName)` metoda používá k určení, zda aktuální návštěvníka patří do rozhraní API rolí *roleName*.

> [!NOTE]
> Jsme mohli použít role třídy přímo, nahraďte volání `User.IsInRole(roleName)` voláním [ `Roles.IsUserInRole(roleName)` metoda](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx). Jsem se rozhodla používat objekt zabezpečení `IsInRole(roleName)` metoda v tomto příkladě vzhledem k tomu je mnohem efektivnější než přímo pomocí rozhraní API rolí. Dříve v tomto kurzu jsme nakonfigurovali Správce rolí pro ukládání do mezipaměti role uživatele v souboru cookie. Tento soubor cookie dat uložených v mezipaměti je využít pouze při daného objektu zabezpečení `IsInRole(roleName)` metoda je volána, Přímá volání rozhraní API rolí vždy zahrnovat postoupí do úložiště rolí. I v případě, že role nejsou uložené v mezipaměti v souboru cookie, volání objektu zabezpečení `IsInRole(roleName)` metoda je obvykle efektivnější, protože když je volána pro poprvé během požadavku se ukládá do mezipaměti výsledky. Rozhraní API rolí na druhé straně neprovádí žádné ukládání do mezipaměti. Vzhledem k tomu, `RowCreated` událost je aktivována jednou pro každý řádek v prvku GridView, pomocí `User.IsInRole(roleName)` zahrnuje pouze jednu cestu k úložišti role vzhledem k tomu `Roles.IsUserInRole(roleName)` vyžaduje *N* zkracuje dobu odezvy, kde *N* je Počet uživatelských účtů, zobrazují v mřížce.


Tlačítko Upravit `Visible` je nastavena na `true` Pokud je uživatel na této stránce v roli správci nebo správci; jinak je nastavená na `false`. Tlačítko pro odstranění `Visible` je nastavena na `true` pouze v případě, že je uživatel v roli správce.

Vyzkoušejte tuto stránku prostřednictvím prohlížeče. Pokud jste na stránce jako anonymní návštěvníků nebo jako uživatel, který není nadřízeným ani správcem, CommandField je prázdná. je stále existuje, ale jako dynamického zajišťování sliver bez úpravy nebo odstranění tlačítka.

> [!NOTE]
> Je možné skrýt CommandField úplně při bez nadřízeného a bez oprávnění správce je na stránce. Můžu ponechte toto cvičení pro čtečku.


[![Upravit a odstranit tlačítka jsou skryté jiných správců a správců bez](role-based-authorization-cs/_static/image38.png)](role-based-authorization-cs/_static/image37.png)

**Obrázek 13**: úpravy a odstranění tlačítka jsou skryté jiných správců a správců bez ([kliknutím ji zobrazíte obrázek v plné velikosti](role-based-authorization-cs/_static/image39.png))


Pokud uživatel, který patří do role správců (ale ne k roli Administrators) navštíví, uvidí jenom na tlačítko Upravit.


[![Tlačítko Upravit je k dispozici pro vedoucí, je skrytý tlačítko Odstranit](role-based-authorization-cs/_static/image41.png)](role-based-authorization-cs/_static/image40.png)

**Obrázek 14**: zatímco tlačítko Upravit je k dispozici pro vedoucí, tlačítko Odstranit je skrytý ([kliknutím ji zobrazíte obrázek v plné velikosti](role-based-authorization-cs/_static/image42.png))


A pokud správce navštíví, které má přístup k tlačítka úpravy a odstranění.


[![Upravit a odstranit tlačítka jsou k dispozici pouze pro správce](role-based-authorization-cs/_static/image44.png)](role-based-authorization-cs/_static/image43.png)

**Obrázek 15**: úpravy a odstranění tlačítka jsou k dispozici pouze pro správce ([kliknutím ji zobrazíte obrázek v plné velikosti](role-based-authorization-cs/_static/image45.png))


## <a name="step-3-applying-role-based-authorization-rules-to-classes-and-methods"></a>Krok 3: Použití pravidel ověřování na základě Role u tříd a metod

V kroku 2, které jsme omezenou upravovat možnosti u uživatelů v rolích správců a správců a odstraňovat možnosti pouze pro správce. Toho dosáhlo tím, že skryjete prvky přidružené uživatelského rozhraní pro neoprávněné uživatele prostřednictvím programový techniky. Takové míry není zaručeno, že neoprávněný uživatel nebude moct k provedení privilegovaných akcí. Může být prvky uživatelského rozhraní, které jsou přidány později nebo, že jsme si vzpomenout na skrytí neoprávnění uživatelé. Nebo se hacker může zjišťovat další způsob, jak získat stránku ASP.NET provést požadovanou metodu.

Snadný způsob, jak zajistit, že konkrétní funkce nelze přistupovat jakýkoli neoprávněný uživatel je k vyplnění této třídy nebo metody pomocí [ `PrincipalPermission` atribut](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Pokud modul .NET runtime používá třídu nebo spustí jednu z jeho metod, zkontroluje, ujistěte se, že aktuální kontext zabezpečení nemá oprávnění. `PrincipalPermission` Atribut poskytuje mechanismus, pomocí kterého definujeme tato pravidla.

Jsme se podívali na použití `PrincipalPermission` atributu zpět <a id="_msoanchor_9"> </a> [ *autorizace na základě uživatele* ](../membership/user-based-authorization-cs.md) kurzu. Konkrétně jsme viděli, jak k vyplnění prvku GridView `SelectedIndexChanged` a `RowDeleting` obslužná rutina události tak, aby se mohl spustit pouze ověřeným uživatelům a Tito, v uvedeném pořadí. `PrincipalPermission` Atribut funguje stejně dobře s rolemi.

Přiblížíme pomocí `PrincipalPermission` atribut v prvku GridView `RowUpdating` a `RowDeleting` obslužných rutin událostí k zákazu spuštění pro uživatele bez oprávnění. Všechno, co musíme udělat, je přidání odpovídajícího atributu imitovaná každá definice funkce:

[!code-csharp[Main](role-based-authorization-cs/samples/sample13.cs)]

Atribut pro `RowUpdating` obslužná rutina události přikazuje, že jenom uživatelé v rolích správců nebo správců můžete spustit obslužnou rutinu události, kde jako atribut `RowDeleting` obslužná rutina události omezuje spuštění pro uživatele ve Správci role.


> [!NOTE]
> `PrincipalPermission` Atribut je reprezentovaný jako třída v `System.Security.Permissions` oboru názvů. Nezapomeňte si přidat `using System.Security.Permissions` příkazu v horní části souboru třídy modelu code-behind import tohoto oboru názvů.


Pokud nějakým způsobem, který není správcem pokusy o spuštění `RowDeleting` obslužná rutina události, nebo pokud bez správce nebo bez oprávnění správce pokusí spustit `RowUpdating` obslužná rutina události, vyvolá se modul .NET runtime `SecurityException`.


[![Pokud je kontext zabezpečení nemá oprávnění k provádění metody, je vyvolána SecurityException –](role-based-authorization-cs/_static/image47.png)](role-based-authorization-cs/_static/image46.png)

**Obrázek 16**: Pokud kontext zabezpečení nemá oprávnění k provádění metody, `SecurityException` je vyvolána výjimka ([kliknutím ji zobrazíte obrázek v plné velikosti](role-based-authorization-cs/_static/image48.png))


Kromě stránek ASP.NET mají mnoho aplikací také architekturu, která zahrnuje různé vrstvy, jako je například obchodní logiky a vrstvy přístupu k datům. Tyto vrstvy jsou obvykle implementovány jako knihovny tříd a nabízejí třídy a metody pro provádění obchodní logiku a data související funkce. `PrincipalPermission` Atribut je užitečné při použití autorizační pravidla i tyto vrstvy.

Další informace o používání `PrincipalPermission` atribut pro definování pravidel autorizace na třídy a metody, přečtěte si [Scott Guthrie](https://weblogs.asp.net/scottgu/)na blogu [přidáním autorizačních pravidel a obchodních dat pomocí vrstvy `PrincipalPermissionAttributes` ](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Souhrn

V tomto kurzu jsme se podívali na tom, jak zadat hrubý a nich spočívá v jemné autorizační pravidla podle role uživatele. ASP. Funkce autorizace adresy URL pro NET umožňuje vývojářům určit, jaké identit se povoluje nebo odepírá přístup, na jaké stránky. Jak jsme viděli v <a id="_msoanchor_10"> </a> [ *autorizace na základě uživatele* ](../membership/user-based-authorization-cs.md) výukový program, autorizace adres URL pravidla lze použít pro uživatele podle uživatele. Se můžou také uplatnit na základě rolí role, jako jsme viděli v kroku 1 tohoto kurzu.

Nich spočívá v jemné autorizační pravidla mohou být použity deklarativně nebo prostřednictvím kódu programu. V kroku 2 jsme se podívali na pomocí ovládacího prvku LoginView kolekci RoleGroups funkce za účelem vykreslení jiný výstupní na základě rolí hostujícími uživatelů. Také jsme se podívali na způsoby, jak prostřednictvím kódu programu určit, pokud uživatel patří do určité role a postupy odpovídajícím způsobem upravit na stránce funkce.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Přidání obchodní a datové vrstvy pomocí autorizačních pravidel `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Zkoumání ASP.NET 2.0 pro členství, role a profilu: práce s rolemi](http://aspnet.4guysfromrolla.com/articles/121405-1.aspx)
- [Seznam otázek zabezpečení pro technologii ASP.NET 2.0](https://msdn.microsoft.com/library/ms998375.aspx)
- [Technická dokumentace ke službě `<roleManager>` – Element](https://msdn.microsoft.com/library/ms164660.aspx)

### <a name="about-the-author"></a>O autorovi

Scott Meisnerová, Autor více ASP/ASP.NET knih a zakladatelem 4GuysFromRolla.com, má pracovali Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy  *[Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott může být dostupný na adrese [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) nebo prostřednictvím na svém blogu [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Speciální k...

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu zahrnují Suchi Banerjee a Teresy Murphy. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](assigning-roles-to-users-cs.md)
> [další](creating-and-managing-roles-vb.md)
