---
uid: web-forms/overview/older-versions-security/roles/role-based-authorization-vb
title: "Ověření na základě role (VB) | Microsoft Docs"
author: rick-anderson
description: "V tomto kurzu začíná podívejte se na tom, jak rozhraní role přidruží role uživatele jeho kontext zabezpečení. Pak prozkoumá jak se má použít na základě rolí URL..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: 83b4f5a4-4f5a-4380-ba33-f0b5c5ac6a75
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/roles/role-based-authorization-vb
msc.type: authoredcontent
ms.openlocfilehash: 973e5705fc36b13e5e6ec861dd2ca6adfc0f50fe
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="role-based-authorization-vb"></a>Ověření na základě role (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.11.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial11_RoleAuth_vb.pdf)

> V tomto kurzu začíná podívejte se na tom, jak rozhraní role přidruží role uživatele jeho kontext zabezpečení. Pak zkontroluje jak se má použít na základě rolí autorizačních pravidel adres URL. Následující, že se podíváme na pomocí deklarativní a programové znamená pro změnu zobrazená data a funkce nabízené stránky ASP.NET.


## <a name="introduction"></a>Úvod

V <a id="_msoanchor_1"> </a> [ *autorizace na základě uživatele* ](../membership/user-based-authorization-vb.md) kurzu jsme viděli postup použijte k určení, kterými se uživatelé mohou najdete konkrétní sadu stránek autorizace adres URL. S stačí jenom trocha značek v `Web.config`, jsme může vyzvat technologie ASP.NET najdete stránce pouze ověřeným uživatelům umožnit. Nebo jsme může stanovují, že byly mohou pouze uživatelé, Tito a Bob nebo znamenat, že byly povoleny všechny ověřené uživatele s výjimkou Sam.

Kromě autorizace adres URL jsme také prohlédli deklarativní a programové techniky pro řízení dat, které zobrazuje a funkce nabízené stránky v závislosti na uživatele, kteří navštěvují. Konkrétně jsme vytvořili stránky, který je uvedený obsah aktuální adresář. Každý, kdo může najdete na této stránce, ale pouze ověření uživatelé mohou zobrazit tyto soubory obsahu a pouze Tito může odstranit soubory.

Použití autorizační pravidla na základě uživatele uživatelů můžou růst do při důvodů účetnictví. Více udržovatelný možných přístupů je použít ověřování založené na rolích. Dobrá zpráva je, že nástroje naše k dispozici pro použití autorizačních pravidel fungovat stejně jako u uživatelských účtů spolu s rolemi. Autorizačních pravidel adres URL můžete zadat role místo uživatelů. Ovládací prvek LoginView, který vykreslí různých výstupu pro ověřený a anonymním uživatelům, můžete nakonfigurovat k zobrazení různých obsahu na základě rolí přihlášeného uživatele. A rozhraní API rolí obsahuje metody pro určení rolí přihlášeného uživatele.

V tomto kurzu začíná podívejte se na tom, jak rozhraní role přidruží role uživatele jeho kontext zabezpečení. Pak zkontroluje jak se má použít na základě rolí autorizačních pravidel adres URL. Následující, že se podíváme na pomocí deklarativní a programové znamená pro změnu zobrazená data a funkce nabízené stránky ASP.NET. Můžeme začít!

## <a name="understanding-how-roles-are-associated-with-a-users-security-context"></a>Pochopení jak role jsou přidružené kontextu zabezpečení uživatele

Vždy, když požadavek vstoupí kanálu ASP.NET je přidružen kontext zabezpečení, který obsahuje informace o identifikaci žadatel. Pokud používáte ověřování pomocí formulářů, na ověřovací lístek slouží jako identity token. Jak již bylo zmíněno <a id="_msoanchor_2"> </a> [ *Přehled ověřování založené na formulářích* ](../introduction/an-overview-of-forms-authentication-vb.md) a <a id="_msoanchor_3"> </a> [ *formulářů Konfigurace ověřování a rozšířené témata* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) kurzy, `FormsAuthenticationModule` je zodpovědná za určení identitu žadatele, který provede během [ `AuthenticateRequest` událostí](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.authenticaterequest.aspx).

Pokud se najde platný, – platnost ověřovací lístek, `FormsAuthenticationModule` dekóduje, aby zjistily identitu žadatele. Vytvoří novou `GenericPrincipal` objektu a přiřadí k tomu `HttpContext.User` objektu. Účelem objekt zabezpečení, jako například `GenericPrincipal`, je identifikace jméno ověřeného uživatele a co uživatel patří do role. Je zřejmé ve skutečnosti, že všechny hlavní objekty mají tento účel `Identity` vlastnost a `IsInRole(roleName)` metoda. `FormsAuthenticationModule`, Ale není zájem o zaznamenávání informací o rolích a `GenericPrincipal` objekt vytvoří neurčuje žádné role.

Pokud je povoleno rozhraní rolí, [ `RoleManagerModule` ](https://msdn.microsoft.com/en-us/library/system.web.security.rolemanagermodule.aspx) modulu HTTP kroky po `FormsAuthenticationModule` a identifikuje role ověřeného uživatele při [ `PostAuthenticateRequest` událostí](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.postauthenticaterequest.aspx), které Aktivuje se po `AuthenticateRequest` událostí. Pokud se jedná o žádost z ověřeného uživatele, `RoleManagerModule` přepíše `GenericPrincipal` objekt vytvořený `FormsAuthenticationModule` a nahradí ji s [ `RolePrincipal` objekt](https://msdn.microsoft.com/en-us/library/system.web.security.roleprincipal.aspx). `RolePrincipal` Třída používá k určení rolí, které uživatel patří do rozhraní API rolí.

Obrázek 1 znázorňuje pracovním postupem kanálu ASP.NET při použití ověřování pomocí formulářů a rozhraní role. `FormsAuthenticationModule` Provede první, identifikuje uživatele na základě jeho lístek ověřování a vytvoří novou `GenericPrincipal` objektu. Dále `RoleManagerModule` kroky a přepíše `GenericPrincipal` objektu s `RolePrincipal` objektu.

Pokud anonymní uživatel navštíví web, ani `FormsAuthenticationModule` ani `RoleManagerModule` vytvoří objekt zabezpečení.


[![Události kanálu ASP.NET pro ověřeného uživatele při použití ověřování pomocí formulářů a rozhraní role](role-based-authorization-vb/_static/image2.png)](role-based-authorization-vb/_static/image1.png)

**Obrázek 1**: události kanálu ASP.NET pro k ověření uživatele při použití ověřování pomocí formulářů a rozhraní role ([Kliknutím zobrazit obrázek v plné velikosti](role-based-authorization-vb/_static/image3.png))


### <a name="caching-role-information-in-a-cookie"></a>Ukládání do mezipaměti informace o rolích do souboru cookie

`RolePrincipal` Objektu `IsInRole(roleName)` volání metod `Roles`.`GetRolesForUser` Chcete-li získat role pro uživatele, aby bylo možné určit, zda uživatel je členem skupiny *roleName*. Při použití `SqlRoleProvider`, výsledkem dotazu do databáze úložiště rolí. Při použití autorizačních pravidel adres URL na základě rolí `RolePrincipal`na `IsInRole` bude volána metoda u každého požadavku na stránku, který je chráněný pomocí autorizačních pravidel adres URL na základě rolí. Nemusí používat k vyhledávání informací role v databázi u každého požadavku `Roles` framework zahrnuje možnost pro ukládání do mezipaměti role uživatele v souboru cookie.

Pokud rozhraní role je nakonfigurovaný pro ukládání do mezipaměti role uživatele do souboru cookie `RoleManagerModule` během kanálu ASP.NET vytvoří soubor cookie [ `EndRequest` událostí](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.endrequest.aspx). Tento soubor cookie se používá v následných žádostí v `PostAuthenticateRequest`, který je při `RolePrincipal` je vytvořen objekt. Pokud soubor cookie platný a jestli nevypršela platnost, je analyzovat a používaných k naplnění role uživatele, a tím ukládání dat v souboru cookie `RolePrincipal` z nutnosti provádět volání `Roles` třídu k určení role uživatele. Obrázek 2 znázorňuje tento pracovní postup.


[![Informace o rolích uživatele mohou být uloženy do souboru cookie pro zlepšení výkonu](role-based-authorization-vb/_static/image5.png)](role-based-authorization-vb/_static/image4.png)

**Obrázek 2**: pole uživatelské Role může být uložena v souboru Cookie ke zlepšení výkonu ([Kliknutím zobrazit obrázek v plné velikosti](role-based-authorization-vb/_static/image6.png))


Mechanismus role mezipaměti souborů cookie je ve výchozím nastavení zakázaná. Může být povoleno prostřednictvím `<roleManager>`; značek konfigurace v `Web.config`. Jsme probrali pomocí [ `<roleManager>` element](https://msdn.microsoft.com/en-us/library/ms164660.aspx) chcete zadat zprostředkovatele rolí v <a id="_msoanchor_4"> </a> [ *vytváření a Správa rolí* ](creating-and-managing-roles-vb.md) kurzu Proto byste již měli mít tento element ve vaší aplikaci `Web.config` souboru. Nastavení ukládání do mezipaměti souborů cookie role jsou zadané jako atributy `<roleManager>`; element a jsou shrnuty v tabulce 1.

> [!NOTE]
> Nastavení konfigurace, které jsou uvedené v tabulce 1 zadejte vlastnosti výsledný soubor cookie mezipaměti role. Další informace o soubory cookie, funkce a jejich různé vlastnosti čtení [tohoto kurzu soubory cookie](http://www.quirksmode.org/js/cookies.html).


| **Vlastnost** | **Popis** |
| --- | --- |
| `cacheRolesInCookie` | Logická hodnota, která označuje, zda se používá ukládání do mezipaměti souborů cookie. Použije se výchozí hodnota `false`. |
| `cookieName` | Název souboru cookie mezipaměti role. Výchozí hodnota je ". ASPXROLES". |
| `cookiePath` | Cesta k souboru cookie název role. Atribut path umožňuje vývojáři k omezení oboru soubor cookie pro konkrétní adresář hierarchie. Výchozí hodnota je "/", která informuje prohlížeč odeslat lístek ověřovacího souboru cookie na všechny žádosti do domény. |
| `cookieProtection` | Určuje, jaké postupy se používají k ochraně souborů cookie mezipaměti role. Povolené hodnoty jsou: `All` (výchozí); `Encryption`; `None`; a `Validation`. Odkazovat zpět do kroku 3 <a id="_anchor_5"> </a> [ *konfiguraci ověřování formulářů a rozšířené témata* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) kurz další informace o těchto úrovních ochrany. |
| `cookieRequireSSL` | Logická hodnota, která určuje, jestli přenos ověřovacího souboru cookie vyžaduje připojení SSL. Výchozí hodnota je `false`. |
| `cookieSlidingExpiration` | Logická hodnota, která určuje, že zda časový limit souboru cookie se vynuluje pokaždé, když uživatel navštíví web během jedné relace. Výchozí hodnota je `false`. Tato hodnota je pouze v příslušném případě `createPersistentCookie` je nastaven na `true`. |
| `cookieTimeout` | Určuje dobu v minutách, po kterém vyprší platnost ověřovacího lístku souboru cookie. Výchozí hodnota je `30`. Tato hodnota je pouze v příslušném případě `createPersistentCookie` je nastaven na `true`. |
| `createPersistentCookie` | Logická hodnota, která určuje, zda role mezipaměti souborů cookie je soubor cookie relace nebo trvalého souboru cookie. Pokud `false` (výchozí), se používá soubor cookie relace, které se odstraní při zavření prohlížeče. Pokud `true`, je použita trvalého souboru cookie; jeho platnost vyprší `cookieTimeout` počet minut po jeho vytvoření nebo po předchozí návštěvě, v závislosti na hodnotě `cookieSlidingExpiration`. |
| `domain` | Určuje hodnotu domény souboru cookie. Výchozí hodnota je řetězec prázdný, což způsobí, že prohlížeči domény, ze kterého byl vydán (například www.yourdomain.com). V takovém případě bude soubor cookie **není** zasílá po provedení žádosti subdomény, jako je například admin.yourdomain.com. Pokud chcete soubor cookie, které mají být předány všechny subdomény, budete muset přizpůsobit `domain` atribut, jeho nastavení na hodnotu "vase_domena.com". |
| `maxCachedResults` | Určuje maximální počet názvů rolí, které jsou uložené v mezipaměti v souboru cookie. Výchozí hodnota je 25. `RoleManagerModule` Nevytváří soubor cookie pro uživatele, kteří patří do více než `maxCachedResults` role. V důsledku toho `RolePrincipal` objektu `IsInRole` metoda použije `Roles` třídu k určení role uživatele. Z důvodu `maxCachedResults` existuje totiž mnoho Uživatelští agenti nepovoluje soubory cookie, které jsou větší než 4 096 bajtů. Proto tento cap rozumí se sníží pravděpodobnost, což představuje překročení toto omezení velikosti. Pokud máte role extrémně dlouhé názvy, můžete chtít zvažte zadání menší `maxCachedResults` hodnoty; contrariwise, pokud máte role velmi krátké názvy, můžete pravděpodobně zvýšit tuto hodnotu. |

**Tabulka 1**: možnosti konfigurace Role mezipaměti souborů Cookie

Umožňuje nakonfigurovat naší aplikaci používat soubory cookie mezipaměti dočasnou role. K tomu, aktualizovat `<roleManager>` element v `Web.config` zahrnout následující atributy souboru cookie související:

[!code-xml[Main](role-based-authorization-vb/samples/sample1.xml)]

Můžu aktualizovat `<roleManager>`; element tak, že přidáte tři atributy: `cacheRolesInCookie`, `createPersistentCookie`, a `cookieProtection`. Nastavením `cacheRolesInCookie` k `true`, `RoleManagerModule` bude nyní automaticky ukládat do mezipaměti role uživatele v souboru cookie, namísto nutnosti vyhledat informace o rolích uživatele na každý požadavek. Explicitně nastavit `createPersistentCookie` a `cookieProtection` atributy pro `false` a `All`, v uvedeném pořadí. Technicky nebylo nutné zadat hodnoty pro tyto atributy, protože je právě přiděleny na výchozí hodnoty, ale I je zde zadejte aby explicitně vymazat nepoužívám trvalé soubory cookie a zda je soubor cookie obě zašifrované a ověřit.

To je všechno je k němu! Role uživatelů v souborech cookie v mezipaměti od nynějška, rozhraní role. Pokud uživatele prohlížeč nepodporuje soubory cookie, nebo pokud své soubory cookie jsou odstraněna nebo ke ztrátě, nějakým způsobem, je žádné big pozornosti – `RolePrincipal` objektu bude jednoduše používat `Roles` – třída v případě, že je k dispozici žádný soubor cookie (nebo některého neplatná nebo vypršela její platnost).

> [!NOTE]
> Společnosti Microsoft vzory &amp; postupy skupiny nedoporučuje pomocí role trvalé mezipaměti souborů cookie. Vzhledem k tomu, že vlastnictví souboru cookie mezipaměti role je dostačující k prokázání členství role, pokud se hacker nějakým způsobem můžete získat přístup k souboru cookie platné uživatelské mu zosobnit uživatele. Pravděpodobnost této situaci se zvyšuje, pokud je soubor cookie trvalý na prohlížeče uživatele. Další informace o toto doporučení zabezpečení, jakož i další otázky zabezpečení, najdete v části [seznamu otázky zabezpečení pro technologii ASP.NET 2.0](https://msdn.microsoft.com/en-us/library/ms998375.aspx).


## <a name="step-1-defining-role-based-url-authorization-rules"></a>Krok 1: Definování autorizačních pravidel adres URL na základě rolí

Jak je popsáno v <a id="_msoanchor_6"> </a> [ *autorizace na základě uživatele* ](../membership/user-based-authorization-vb.md) kurzu autorizace adres URL nabízí způsob omezení přístupu k sadě stránek na uživatele podle uživatele nebo roli role základ. Autorizačních pravidel adres URL jsou vypsány v `Web.config` pomocí [ `<authorization>` element](https://msdn.microsoft.com/en-us/library/8d82143t.aspx) s `<allow>` a `<deny>` podřízené elementy. Kromě související s uživateli autorizačních pravidel popsaných v předchozí kurzy každý `<allow>` a `<deny>` podřízený element může také zahrnovat:

- Konkrétní roli
- Čárkami oddělený seznam rolí

Například autorizačních pravidel adres URL udělení přístupu k tito uživatelé v rolích správce a vedoucí, ale odepření přístupu pro všechny ostatní:

[!code-xml[Main](role-based-authorization-vb/samples/sample2.xml)]

`<allow>` Element ve výše uvedených značek stavy, že jsou povolené role správců a vedoucí; `<deny>`; dá pokyn element, který *všechny* uživatelé mají odepřený.

Umožňuje nakonfigurovat aplikaci tak, aby `ManageRoles.aspx`, `UsersAndRoles.aspx`, a `CreateUserWizardWithRoles.aspx` stránky jsou pouze uživatelé, v roli správce při `RoleBasedAuthorization.aspx` nadále dostupná pro všechny návštěvníky.

K tomu, začněte přidáním `Web.config` do souboru `Roles` složky.


[![Přidá soubor Web.config k adresáři role](role-based-authorization-vb/_static/image8.png)](role-based-authorization-vb/_static/image7.png)

**Obrázek 3**: přidání `Web.config` do souboru `Roles` adresáře ([Kliknutím zobrazit obrázek v plné velikosti](role-based-authorization-vb/_static/image9.png))


Dál přidejte následující kód konfigurace k `Web.config`:

[!code-xml[Main](role-based-authorization-vb/samples/sample3.xml)]

`<authorization>` Element v `<system.web>` části označuje, že jenom uživatelé v roli správce může přístup k prostředkům technologie ASP.NET v `Roles` adresáře. `<location>` Element definuje alternativní sadu autorizačních pravidel adres URL pro `RoleBasedAuthorization.aspx` stránky, umožňuje všem uživatelům najdete na stránce.

Po uložení změn do `Web.config`, přihlášení jako uživatel, který se nenachází v roli správce a potom se pokusíte navštíví některý z chráněných stránky. `UrlAuthorizationModule` Zjistí, že nemáte oprávnění k navštívení požadovaný prostředek; v důsledku toho `FormsAuthenticationModule` vás přesměruje na přihlašovací stránku. Na přihlašovací stránku vás pak přesměruje na `UnauthorizedAccess.aspx` stránce (viz obrázek 4). Tento poslední přesměrování z přihlašovací stránky k `UnauthorizedAccess.aspx` k tomu dochází kvůli kód jsme přidali na přihlašovací stránku v kroku 2 <a id="_msoanchor_7"> </a> [ *autorizace na základě uživatele* ](../membership/user-based-authorization-vb.md) kurzu. Na konkrétní stránku pro přihlášení automaticky přesměruje všem ověřeným uživatelům `UnauthorizedAccess.aspx` Pokud řetězec dotazu obsahuje `ReturnUrl` parametru, jako tento parametr určuje, že uživatel na přihlašovací stránku po pokusu o zobrazení stránky mu nebyla dostali oprávnění k zobrazení.


[![Chráněné stránky lze zobrazit pouze uživatele v roli správce](role-based-authorization-vb/_static/image11.png)](role-based-authorization-vb/_static/image10.png)

**Obrázek 4**: pouze uživatelé v roli správce můžete zobrazit na stránkách chráněné ([Kliknutím zobrazit obrázek v plné velikosti](role-based-authorization-vb/_static/image12.png))


Odhlásit a potom se přihlaste jako uživatel, který je v roli správce. Nyní měli být schopní zobrazit tři chráněné stránky.


[![Tito najdete UsersAndRoles.aspx stránky protože se v roli správce](role-based-authorization-vb/_static/image14.png)](role-based-authorization-vb/_static/image13.png)

**Obrázek 5**: Tito najdete `UsersAndRoles.aspx` stránce vzhledem k tomu, že je v roli správce ([Kliknutím zobrazit obrázek v plné velikosti](role-based-authorization-vb/_static/image15.png))


> [!NOTE]
> Při zadávání autorizačních pravidel adres URL – rolí nebo uživatelů – je důležité mít na paměti, že pravidla jsou analyzovaných jeden po druhém, shora dolů. Jakmile je nalezena shoda, je uživatel povolen nebo odepřen přístup, podle toho, pokud v byla nalezena shoda `<allow>` nebo `<deny>` elementu. **Pokud není nalezena žádná shoda, uživateli je udělen přístup.** V důsledku toho, pokud chcete omezit přístup k jedné nebo víc uživatelských účtů, je nezbytné použít `<deny>` element jako posledním prvkem v konfiguraci ověřování adresy URL. **Pokud nebudou obsahovat vaše autorizačních pravidel adres URL**`<deny>`**elementu, všichni uživatelé bude udělen přístup.** Podrobnější informace o tom, jak se analyzují autorizačních pravidel adres URL, najdete v části zpět "podívejte se na to, jak `UrlAuthorizationModule` používá pravidla autorizace udělit nebo odepřít přístup" oddílu <a id="_msoanchor_8"> </a> [  *Autorizace uživatele na základě* ](../membership/user-based-authorization-vb.md) kurzu.


## <a name="step-2-limiting-functionality-based-on-the-currently-logged-in-users-roles"></a>Krok 2: Omezení funkce na základě rolí aktuálně přihlášeného uživatele

Adresa URL autorizace díky usnadňují zadejte hrubým autorizační pravidla tímto stavem jaké identity jsou povoleny a které z nich je odepřeno zobrazení konkrétní stránky (nebo všechny stránky ve složce a jejích podsložkách). Ale v některých případech může Chceme všem uživatelům najdete stránce, ale omezit tak jeho funkčnost stránky na základě rolí hostujícími uživatelů. To může mít za následek zobrazení nebo skrytí dat na základě role uživatele, nebo nabídky další funkce pro uživatele, kteří patří do určité role.

Tato pravidla autorizace na základě rolí spočívá v jemné dá implementovat buď deklarativně nebo prostřednictvím kódu programu (nebo prostřednictvím některé kombinaci obou). V další části jsme se zobrazí jak implementovat deklarativní spočívá v jemné autorizace prostřednictvím ovládacího prvku LoginView. Následující, se podíváme na programový techniky. Podíváme můžete na použití spočívá v jemné autorizační pravidla, ale nejprve musíme vytvořit stránku, jejichž funkce závisí na roli uživatele návštěvou ho.

Umožňuje vytvořit stránku, která obsahuje seznam všech uživatelských účtů v systému v GridView. GridView bude zahrnovat uživatelské jméno, e-mailovou adresu, datum posledního přihlášení a komentáře o uživateli jednotlivých uživatelů. Kromě zobrazení informací o jednotlivých uživatelů, budou GridView zahrnují upravit a odstranit možnosti. Původně jsme vytvoříte tuto stránku s upravit a odstranit funkce je k dispozici pro všechny uživatele. V části "Použití LoginView řídicí" a "Prostřednictvím kódu programu omezení funkce" jsme se zobrazí postup povolení nebo zakázání tyto funkce na základě role uživatele, který web navštíví.

> [!NOTE]
> Stránka ASP.NET, které jsme se chystáte vytvořit pomocí ovládacího prvku GridView zobrazí uživatelské účty. Od tohoto kurzu, které řady se zaměřuje na ověřování pomocí formulářů, ověřování, uživatelské účty a rolí nechci se příliš mnoho času hovoříte o vnitřního chodu ovládacího prvku GridView. Při tomto kurzu obsahuje konkrétní podrobné pokyny pro nastavení této stránky, není pustíte do podrobnosti proč byly provedeny některé možnosti, nebo konkrétní vlastnosti vliv mají na vykresleném výstupu. Pro důkladné posouzení ovládacího prvku GridView, podívejte se na Moje  *[práci s daty v technologii ASP.NET 2.0](../../data-access/index.md)*  kurz řady.


Začněte otevřením `RoleBasedAuthorization.aspx` stránku `Roles` složky. Přetáhněte GridView ze stránky na Designer a sadu jeho `ID` k `UserGrid`. Za chvíli jsme psát kód, který volá `Membership`.`GetAllUsers` Metoda a sváže výsledná `MembershipUserCollection` objekt, který chcete GridView. `MembershipUserCollection` Obsahuje `MembershipUser` objekt pro každý uživatelský účet v systému. `MembershipUser` objekty mají vlastnosti, například `UserName`,`Email`,`LastLoginDate` a tak dále.

Před jsme napsat kód, který váže uživatelské účty do mřížky, nejdříve definujme prvku GridView pole. Z prvku GridView inteligentních značek, klikněte na odkaz "Upravit sloupce" Spustit dialogové okno pole (viz obrázek 6). Z tohoto místa zrušte zaškrtnutí políčka "automaticky generovat pole" v levém dolním rohu. Vzhledem k tomu, že má být tato rutina GridView zahrnují úpravy a odstraňování možnosti, přidání CommandField a nastavení jeho `ShowEditButton` a `ShowDeleteButton` vlastnosti na hodnotu True. V dalším kroku přidejte čtyři pole pro zobrazení `UserName`, `Email`, `LastLoginDate`, a `Comment` vlastnosti. Použít BoundField pro dvě vlastnosti jen pro čtení (`UserName` a `LastLoginDate`) a TemplateFields pro dvě upravitelné pole (`Email` a `Comment`).

Mít první zobrazení BoundField `UserName` vlastnost; nastavte její `HeaderText` a `DataField` vlastnosti, které chcete "UserName". Toto pole nebude možné upravovat, takže nastavit jeho `ReadOnly` vlastnost na hodnotu True. Konfigurace `LastLoginDate` BoundField nastavením jeho `HeaderText` "Poslední přihlášení" a jeho `DataField` na "LastLoginDate". Pojďme formátu výstup této BoundField tak, aby se zobrazí jenom datum (namísto datum a čas). Chcete-li dosáhnout, nastavte tento BoundField `HtmlEncode` vlastnost na hodnotu False a jeho `DataFormatString` vlastnost "{0: d}". Také nastavit `ReadOnly` vlastnost na hodnotu True.

Nastavte `HeaderText` vlastnosti dvě TemplateFields "E-mailu" a "Komentář".


[![Rutina GridView na pole můžete nakonfigurovat přes dialogové okno pole](role-based-authorization-vb/_static/image17.png)](role-based-authorization-vb/_static/image16.png)

**Obrázek 6**: The GridView pole může být nakonfigurovaný prostřednictvím dialogové okno pole ([Kliknutím zobrazit obrázek v plné velikosti](role-based-authorization-vb/_static/image18.png))


Nyní potřebujeme k definování `ItemTemplate` a `EditItemTemplate` "E-mailu" a "Komentář" TemplateFields. Přidání ovládacího prvku popisek pro každý z `ItemTemplates` a vytvořte vazbu jejich `Text` vlastnosti, které chcete `Email` a `Comment` vlastnosti, v uvedeném pořadí.

Pro "E-mailu" TemplateField, přidejte textové pole s názvem `Email` k jeho `EditItemTemplate` a vytvořte vazbu jeho `Text` vlastnost, která má `Email` vlastnost pomocí obousměrné vazby dat. Přidání RequiredFieldValidator a RegularExpressionValidator k `EditItemTemplate` zajistit, že návštěvník úpravy vlastnost e-mailu zadal platný e-mailovou adresu. Pro "Komentář" TemplateField, přidejte Víceřádkový textové pole s názvem `Comment` k jeho `EditItemTemplate`. Nastavit textového pole `Columns` a `Rows` vlastností 40 a 4, v uvedeném pořadí a pak vytvořte vazbu jeho `Text` vlastnost, která má `Comment` vlastnost pomocí obousměrné vazby dat.

Po dokončení konfigurace těchto TemplateFields, jejich deklarativní značek by měl vypadat takto:

[!code-aspx[Main](role-based-authorization-vb/samples/sample4.aspx)]

Při úpravě nebo odstranění uživatelského účtu, potřebujeme vědět, že uživatel `UserName` hodnotu vlastnosti. Nastavte GridView `DataKeyNames` vlastnost "UserName" tak, aby tyto informace jsou k dispozici prostřednictvím prvku GridView `DataKeys` kolekce.

Nakonec přidejte ValidationSummary ovládacího prvku na stránku a nastavit jeho `ShowMessageBox` vlastnost na hodnotu True a jeho `ShowSummary` vlastnost na hodnotu False. S těmito nastaveními ValidationSummary zobrazí upozornění na straně klienta, pokud se uživatel pokusí upravit uživatelský účet s e-mailovou adresu chybí nebo je neplatný.

[!code-aspx[Main](role-based-authorization-vb/samples/sample5.aspx)]

Nyní jsme dokončili deklarativní tuto stránku. Naše dalším krokem je vytvoření vazby sadu uživatelské účty na GridView. Přidejte metodu s názvem `BindUserGrid` k `RoleBasedAuthorization.aspx` třídy kódu stránky, která se váže `MembershipUserCollection` vrácený `Membership.GetAllUsers` k `UserGrid` GridView. Volat tuto metodu z `Page_Load` obslužné rutiny události při první návštěvě stránky.

[!code-vb[Main](role-based-authorization-vb/samples/sample6.vb)]

S tímto kódem na místě najdete na stránce prostřednictvím prohlížeče. Jak je vidět na obrázku 7, měli byste vidět GridView výpis informace o všechny uživatelské účty v systému.


[![Rutina UserGrid GridView uvádí informace o jednotlivých uživatelů v systému](role-based-authorization-vb/_static/image20.png)](role-based-authorization-vb/_static/image19.png)

**Obrázek 7**: `UserGrid` GridView uvádí informace o každý uživatel v systému ([Kliknutím zobrazit obrázek v plné velikosti](role-based-authorization-vb/_static/image21.png))


> [!NOTE]
> `UserGrid` GridView obsahuje seznam všech uživatelů v nestránkovaného rozhraní. Toto rozhraní jednoduché mřížky není vhodný pro scénáře, kde je několik desítek nebo více uživatelů. Jednou z možností je konfigurovat GridView pro povolení stránkování. `Membership.GetAllUsers` Metoda má dva přetížení: ten, který přijímá žádné vstupní parametry a vrátí všechny uživatele a ten, který přebírá celočíselné hodnoty pro index stránky a velikost stránky a vrátí pouze zadané podmnožině uživatelů. Druhý přetížení umožňuje efektivněji stránky do uživatele vzhledem k tomu, že se vrátí jenom přesné podmnožině uživatelské účty místo *všechny* z nich. Pokud máte tisíce uživatelské účty, můžete zvážit rozhraní založené na filtr, který se zobrazí pouze uživatelům, jejichž uživatelské jméno začíná znakem vybrané pro instanci. [ `Membership.FindUsersByName` Metoda](https://msdn.microsoft.com/en-us/library/system.web.security.membership.findusersbyname.aspx) je ideální pro vytvoření filtru na základě uživatelské rozhraní. Podíváme se na vytvoření takového rozhraní v budoucnu kurzu.


Ovládací prvek GridView nabízí integrovanou úpravy a odstraňování podpory, když je ovládací prvek vázán na ovládací prvek zdroje dat správně nakonfigurované, například SqlDataSource nebo ObjectDataSource. `UserGrid` GridView, má však jeho data prostřednictvím kódu programu vázaná; proto jsme musíte napsat kód pro provést tyto dvě úlohy. Konkrétně budeme muset vytváření obslužných rutin událostí pro prvku GridView `RowEditing`, `RowCancelingEdit`, `RowUpdating`, a `RowDeleting` události, které jsou aktivována, jestliže návštěvník klepne GridView úpravy, zrušit, aktualizace nebo odstranění tlačítka.

Začněte tím, že vytváření obslužných rutin událostí pro prvku GridView `RowEditing`, `RowCancelingEdit`, a `RowUpdating` události a poté přidejte následující kód:

[!code-vb[Main](role-based-authorization-vb/samples/sample7.vb)]

`RowEditing` a `RowCancelingEdit` obslužné rutiny událostí jednoduše nastavit prvku GridView `EditIndex` vlastnost a potom obnovení vazby seznam uživatelské účty k mřížce. Probíhá zajímavé věcem `RowUpdating` obslužné rutiny události. Tuto obslužnou rutinu události spustí tím zajistí, že data platná a pak získá `UserName` hodnotu upravená uživatelského účtu z `DataKeys` kolekce. `Email` a `Comment` textových polí ve dvou TemplateFields `EditItemTemplate` s jsou pak prostřednictvím kódu programu odkazuje. Jejich `Text` vlastnosti obsahovat upravená e-mailovou adresu a komentář.

Aby bylo možné aktualizovat k uživatelskému účtu pomocí rozhraní API členství musíme nejdřív získat informace o uživateli, který provedeme prostřednictvím volání `Membership.GetUser(userName)`. Vrácený `MembershipUser` objektu `Email` a `Comment` vlastnosti jsou potom aktualizovány hodnot zadaných do dvou textových polí z rozhraní úpravy. Nakonec se tyto změny se ukládají s volání [ `Membership.UpdateUser` ](https://msdn.microsoft.com/en-us/library/system.web.security.membership.updateuser.aspx). `RowUpdating` Obslužné rutiny události dokončí vrácení rutina GridView k jeho předem úpravy rozhraní.

Dále vytvořte `RowDeleting` RowDeleting obslužné rutiny události a poté přidejte následující kód:

[!code-vb[Main](role-based-authorization-vb/samples/sample8.vb)]

Výše uvedené obslužné rutiny události začne metodou `UserName` hodnotu z prvku GridView `DataKeys` kolekce; tato `UserName` hodnota je předána do třídy členství [ `DeleteUser` metoda](https://msdn.microsoft.com/en-us/library/system.web.security.membership.deleteuser.aspx). `DeleteUser` Metoda odstraní účet uživatele ze systému, včetně členství v souvisejících dat (například jaké role patří tento uživatel). Po odstranění uživatele, mřížky `EditIndex` je nastaven na hodnotu -1 (v případě, že uživatel klepl odstranit při další řádek v režimu úprav) a `BindUserGrid` metoda je volána.

> [!NOTE]
> Tlačítko Odstranit nevyžaduje žádné řazení potvrzení od uživatele před odstraněním účtu uživatele. I doporučujeme přidat určitou formu potvrzení uživatele k omezit možnost náhodnému odstranění účtu. Jedním z nejjednodušších způsobů, jak akci potvrďte je pomocí dialogového okna potvrdit na straně klienta. Další informace o této technice najdete v tématu [přidání Client-Side potvrzení při odstranění](https://asp.net/learn/data-access/tutorial-42-vb.aspx).


Ověřte, zda tato stránka fungují podle očekávání. Nyní byste měli mít upravit kteréhokoli uživatele e-mailovou adresu a komentáře a také odstranit libovolný uživatelský účet. Vzhledem k tomu `RoleBasedAuthorization.aspx` stránka je přístupné všem uživatelům, každý uživatel – i anonymní návštěvníky – najdete na této stránce a upravit a odstranit uživatelské účty! Umožňuje aktualizovat tuto stránku tak, že jenom uživatelé v rolích dohledu a správci můžete upravit e-mailovou adresu uživatele a komentářů a pouze správci mohou odstranit uživatelský účet.

V části "Použití LoginView řídicí" zjistí pomocí ovládacího prvku LoginView zobrazíte pokyny k roli uživatele. Pokud uživatel v roli správce navštíví tuto stránku, jsme se zobrazí pokyny o tom, jak upravit a odstranit uživatele. Pokud uživatel v roli dohledu dosáhne této stránce, jsme se zobrazí pokyny k úpravám uživatele. A pokud návštěvníka je anonymní, nebo se nenachází v roli dohledu nebo správci, jsme se zobrazí zpráva, která vysvětluje, že se nedají upravit ani odstranit informace o uživatelském účtu. V části "Prostřednictvím kódu programu omezení funkce" jsme psát kód, který prostřednictvím kódu programu zobrazí nebo skryje tlačítka Upravit a odstranit, na základě role uživatele.

### <a name="using-the-loginview-control"></a>Použití ovládacího prvku LoginView

Jak jsme viděli v posledních kurzy, ovládacího prvku LoginView je užitečná pro zobrazení různých rozhraní pro ověřený a anonymní uživatele, ale ovládací prvek LoginView lze také zobrazit různých značek na základě rolí uživatele. K zobrazení různých pokynů na základě role uživatele, který web navštíví použijeme LoginView ovládacího prvku.

Spuštění přidáním LoginView výše `UserGrid` GridView. Jak jsme probrali dříve, ovládacího prvku LoginView má dvě předdefinované šablony: `AnonymousTemplate` a `LoggedInTemplate`. Zadejte stručný zprávu v obou těchto šablon, které informuje uživatele, že se nedají upravit ani odstranit nějaké informace o uživateli.

[!code-aspx[Main](role-based-authorization-vb/samples/sample9.aspx)]

Kromě `AnonymousTemplate` a `LoggedInTemplate`, může zahrnovat ovládacího prvku LoginView *kolekci RoleGroups*, které jsou specifické pro roli šablony. Každý RoleGroup obsahuje jednu vlastnost `Roles`, která určuje, jaké role RoleGroup platí pro. `Roles` Vlastnost lze nastavit do jedné role (např. "Administrators") nebo do čárkami oddělený seznam rolí (např. "Administrators, vedoucí").

Abyste mohli spravovat kolekci RoleGroups, klikněte na odkaz "Upravit kolekci RoleGroups" ze inteligentní značky ovládacího prvku, aby si Editor kolekce RoleGroup. Přidejte dva nové kolekci RoleGroups. Nastavit první RoleGroup `Roles` vlastnost a "Administrators" na "Vedoucí" sekundu.


[![Správa šablon specifické Role LoginView prostřednictvím Editor kolekce RoleGroup.](role-based-authorization-vb/_static/image23.png)](role-based-authorization-vb/_static/image22.png)

**Obrázek 8**: Spravovat LoginView specifickou rolí šablony prostřednictvím Editor kolekce RoleGroup ([Kliknutím zobrazit obrázek v plné velikosti](role-based-authorization-vb/_static/image24.png))


Kliknutím na OK zavřete Editor kolekce RoleGroup; Tím se aktualizuje LoginView deklarativní zahrnout `<RoleGroups>` část s `<asp:RoleGroup>` podřízený element pro každou RoleGroup definované v editoru kolekce RoleGroup. Kromě toho, z rozevírací nabídky "Zobrazení" seznamu v inteligentních značek LoginView – které původně uvedené jenom na `AnonymousTemplate` a `LoggedInTemplate` – nyní zahrnuje přidání kolekci RoleGroups také.

Upravte kolekci RoleGroups tak, aby byly zobrazené pokyny o tom, jak upravit uživatelské účty, zatímco uživatelé v roli správce jsou uvedeny pokyny pro úpravy a odstraňování uživatelů v roli dohledu. Po provedení těchto změn, vaše LoginView deklarativní by měl vypadat takto.

[!code-aspx[Main](role-based-authorization-vb/samples/sample10.aspx)]

Po provedení těchto změn, uložte na stránku a potom navštivte prostřednictvím prohlížeče. Nejprve najdete na stránce jako anonymní uživatel. By měla zobrazit zpráva, "nejste přihlášeni k systému. Proto se nedají upravit ani odstranit nějaké informace o uživateli." Pak se přihlaste jako ověřený uživatel, ale ten, který je ani v roli dohledu ani správci. Tentokrát měla zobrazit zpráva, "nejste členem role vedoucí nebo správci. Proto se nedají upravit ani odstranit nějaké informace o uživateli."

V dalším kroku přihlaste jako uživatel, který je členem role orgánů dohledu. Nyní byste měli vidět vedoucí specifickou rolí zprávy (viz obrázek 9). A pokud se přihlásit jako uživatel v roli, měli byste vidět správci specifickou rolí zprávy (viz obrázek 10) správců.


[![Bruce se zobrazí zprávy specifické pro roli dohledu](role-based-authorization-vb/_static/image26.png)](role-based-authorization-vb/_static/image25.png)

**Obrázek 9**: Bruce se zobrazí zprávy specifické pro roli dohledu ([Kliknutím zobrazit obrázek v plné velikosti](role-based-authorization-vb/_static/image27.png))


[![Tito se zobrazí zprávy specifické pro roli správce](role-based-authorization-vb/_static/image29.png)](role-based-authorization-vb/_static/image28.png)

**Obrázek 10**: Tito se zobrazí zprávy specifické pro roli správce ([Kliknutím zobrazit obrázek v plné velikosti](role-based-authorization-vb/_static/image30.png))


Jako snímky obrazovky v útvarů 9 a 10 zobrazit vykreslí LoginView jenom jedna šablona i v případě, že platí více šablon. Bruce a Tito i přihlášení uživatele, ale LoginView vykreslí pouze odpovídající RoleGroup a ne `LoggedInTemplate`. Kromě toho Tito patří do role správců a vedoucí, ale ovládací prvek LoginView vykreslí šablony specifické role Správci místo vedoucí jeden.

Obrázek 11 znázorňuje používají k určení jakou šablonu k vykreslení ovládacího prvku LoginView pracovního postupu. Všimněte si, že pokud je více než jeden zadaný RoleGroup šabloně LoginView vykreslí *první* RoleGroup, který odpovídá. Jinými slovy Pokud jsme měl umístit RoleGroup dohledu jako první RoleGroup a správci jako druhý, pak když Tito navštívili tuto stránku mohl by najdete v části vedoucí zprávy.


[![Ovládací prvek LoginView pracovního postupu pro určení, jakou šablonu vykreslování](role-based-authorization-vb/_static/image32.png)](role-based-authorization-vb/_static/image31.png)

**Obrázek 11**: LoginView řízení pracovního postupu pro určení, co šablonu k vykreslení ([Kliknutím zobrazit obrázek v plné velikosti](role-based-authorization-vb/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>Prostřednictvím kódu programu omezení funkce

Když tento ovládací prvek LoginView zobrazí různé pokyny na základě role uživatele na stránce, úpravy a tlačítka Storno zůstanou viditelné pro všechny. Musíme prostřednictvím kódu programu skrýt tlačítka Upravit a odstranit pro anonymní návštěvníky a uživatelé, kteří jsou v roli dohledu ani správci. Musíme skrýt tlačítko Odstranit pro každého, kdo není správce. Aby to bylo možné jsme zapíše bit kódu, který prostřednictvím kódu programu odkazuje CommandField upravit a odstranit LinkButtons a nastaví jejich `Visible` vlastnosti, které chcete `False`, v případě potřeby.

Nejjednodušší způsob, jak programově odkazovat ovládacích prvků v CommandField je nejdřív ho převést na šablonu. K tomu, klikněte na odkaz "Upravit sloupce" z prvku GridView inteligentních značek, vyberte CommandField ze seznamu aktuální pole a klikněte na odkaz "Převést toto pole TemplateField". To se změní CommandField TemplateField s `ItemTemplate` a `EditItemTemplate`. `ItemTemplate` Obsahuje upravit a odstranit LinkButtons při `EditItemTemplate` ve uložený aktualizace a LinkButtons zrušit.


[![Převést CommandField TemplateField](role-based-authorization-vb/_static/image35.png)](role-based-authorization-vb/_static/image34.png)

**Obrázek 12**: převést CommandField do TemplateField ([Kliknutím zobrazit obrázek v plné velikosti](role-based-authorization-vb/_static/image36.png))


Aktualizace upravit a odstranit LinkButtons v `ItemTemplate`, nastavení jejich `ID` vlastnosti hodnoty `EditButton` a `DeleteButton`, v uvedeném pořadí.

[!code-aspx[Main](role-based-authorization-vb/samples/sample11.aspx)]

Vždy, když data vázán na GridView, GridView zobrazí záznamy v jeho `DataSource` vlastnost a vygeneruje odpovídající `GridViewRow` objektu. Protože každému `GridViewRow` je vytvořen objekt, `RowCreated` událost je aktivována. Chcete-li skrýt tlačítka Upravit a odstranit neoprávnění uživatelé, je potřeba vytvořit obslužnou rutinu události pro tuto událost a programově odkazovat upravit a odstranit LinkButtons nastavení jejich `Visible` vlastnosti odpovídajícím způsobem.

Vytvoření obslužné rutiny události `RowCreated` události a poté přidejte následující kód:

[!code-vb[Main](role-based-authorization-vb/samples/sample12.vb)]

Mějte na paměti, že `RowCreated` událost aktivuje pro *všechny* GridView řádků, včetně záhlaví, zápatí, pager rozhraní a tak dále. Chceme prostřednictvím kódu programu odkazu upravit a odstranit LinkButtons, pokud jsme se zabývají řádek dat není v režimu úprav, (protože řádek v režimu úprav má aktualizace a zrušit tlačítka místo upravit a odstranit). Tato kontrola se provádí `If` příkaz.

Pokud jsme se zabývají na řádek dat, který není v režimu úprav, upravit a odstranit LinkButtons odkazují a jejich `Visible` vlastnosti jsou nastaveny na základě na logické hodnoty vrácené `User` objektu `IsInRole(roleName)` metoda. `User` Objekt odkazuje na objekt zabezpečení vytvořené `RoleManagerModule`; v důsledku toho `IsInRole(roleName)` metoda používá k určení, zda aktuální návštěvníka patří do rozhraní API rolí *roleName*.

> [!NOTE]
> Může mít použili jsme třída role přímo, nahrazení volání `User.IsInRole(roleName)` pomocí volání [ `Roles.IsUserInRole(roleName)` metoda](https://msdn.microsoft.com/en-us/library/system.web.security.roles.isuserinrole.aspx). Rozhodli používat objekt zabezpečení `IsInRole(roleName)` metoda v tomto příkladu vzhledem k tomu, že je efektivnější než přímo pomocí rozhraní API rolí. V tomto kurzu jsme nakonfigurovali Správce rolí pro ukládání do mezipaměti role uživatele v souboru cookie. Tento soubor cookie data jako do mezipaměti je využít pouze při objektu zabezpečení `IsInRole(roleName)` metoda je volána; přímé volání rozhraní API rolí vždy zahrnovat cestu k úložišti role. I v případě, že role se neukládají do mezipaměti v souboru cookie, volání objekt zabezpečení `IsInRole(roleName)` metoda je obvykle je efektivnější, protože když je volána pro poprvé během požadavku se ukládá do mezipaměti výsledky. Rozhraní API rolí na druhé straně neprovede žádné ukládání do mezipaměti. Protože `RowCreated` událost je aktivována, jednou pro každý řádek v GridView, pomocí `User.IsInRole(roleName)` zahrnuje pouze jediná cesta k úložišti role, zatímco `Roles.IsUserInRole(roleName)` vyžaduje *N* služebních cest, kde *N* je Počet uživatelských účtů zobrazené v mřížce.


Tlačítko Upravit `Visible` je nastavena na `True` Pokud uživatele, kteří navštěvují Tato stránka je v roli správce nebo vedoucí; v opačném případě je nastavený na `False`. Tlačítko Odstranit `Visible` je nastavena na `True` pouze pokud je uživatel v roli správce.

Otestujte tuto stránku prostřednictvím prohlížeče. Pokud jako anonymní návštěvníka nebo jako uživatel, který není nadřízeným ani správce, najdete na stránce, CommandField je prázdná. stále existuje, ale jako dynamické následující bez úpravy nebo odstranění tlačítka.

> [!NOTE]
> Je možné skrýt CommandField zcela při bez nadřízeného a bez oprávnění správce je na stránce. Nechat to jako cvičení pro čtečku.


[![Upravit a odstranit tlačítka jsou skryto bez dohledu a všichni uživatelé](role-based-authorization-vb/_static/image38.png)](role-based-authorization-vb/_static/image37.png)

**Obrázek 13**: Upravit a odstranit tlačítka jsou skryto bez dohledu a všichni uživatelé ([Kliknutím zobrazit obrázek v plné velikosti](role-based-authorization-vb/_static/image39.png))


Pokud uživatel, který patří k roli dohledu (ale ne do role Správci) navštíví, zjistí, jenom na tlačítko Upravit.


[![Tlačítko Upravit je k dispozici pro vedoucí, je skrytý na tlačítko Odstranit](role-based-authorization-vb/_static/image41.png)](role-based-authorization-vb/_static/image40.png)

**Obrázek 14**: při the tlačítka Upravit je k dispozici pro vedoucí, je skrytý na tlačítko Odstranit ([Kliknutím zobrazit obrázek v plné velikosti](role-based-authorization-vb/_static/image42.png))


A pokud správce navštíví, která má přístup k tlačítka Upravit a odstranit.


[![Upravit a odstranit tlačítka jsou k dispozici pouze pro správce](role-based-authorization-vb/_static/image44.png)](role-based-authorization-vb/_static/image43.png)

**Obrázek 15**: Upravit a odstranit tlačítka jsou k dispozici pouze pro správce ([Kliknutím zobrazit obrázek v plné velikosti](role-based-authorization-vb/_static/image45.png))


## <a name="step-3-applying-role-based-authorization-rules-to-classes-and-methods"></a>Krok 3: Použití třídy a metody na základě rolí autorizační pravidla

V kroku 2, které jsme omezené upravit funkce, které uživatelé v rolích dohledu a správci a odstraňte možnosti pouze správcům. Toho dosáhlo tím skrytí prvky přidružené uživatelského rozhraní pro neoprávněné uživatele prostřednictvím programový techniky. Tato opatření není zaručeno, že neoprávněný uživatel nebude možné provádět privilegované akce. Může být prvky uživatelského rozhraní, které jsou přidány později nebo že jsme zapomněli skrýt neoprávnění uživatelé. Nebo se hacker může zjistit nějakým způsobem získat stránku ASP.NET a provést požadovanou metodu.

Snadný způsob, jak Ujistěte se, že konkrétní funkce nelze získat přístup, neoprávněný uživatel je pro uspořádání třídy nebo metoda s [ `PrincipalPermission` atribut](https://msdn.microsoft.com/en-us/library/system.security.permissions.principalpermissionattribute.aspx). Pokud modul runtime rozhraní .NET používá třídu nebo spustí jeden z jeho metody, zkontroluje, ujistěte se, zda aktuální kontext zabezpečení má oprávnění. `PrincipalPermission` Atribut poskytuje mechanismus, pomocí kterého definujeme tato pravidla.

Jsme se podívali na použití `PrincipalPermission` atribut zpět <a id="_msoanchor_9"> </a> [ *autorizace na základě uživatele* ](../membership/user-based-authorization-vb.md) kurzu. Konkrétně jsme viděli postupy pro uspořádání prvku GridView `SelectedIndexChanged` a `RowDeleting` obslužné rutiny události tak, aby se může spustit pouze pomocí ověřeným uživatelům a Tito, v uvedeném pořadí. `PrincipalPermission` Atribut funguje stejně dobře s rolemi.

Pojďme demonstruje použití `PrincipalPermission` atribut v prvku GridView `RowUpdating` a `RowDeleting` obslužné rutiny událostí zakázat spouštění pro uživatele bez oprávnění. Je budeme muset udělat přidat atribut příslušné na každý definice funkce:

[!code-vb[Main](role-based-authorization-vb/samples/sample13.vb)]

Atribut pro `RowUpdating` obslužné rutiny události stanoví, můžete jenom uživatelé v rolích správci nebo vedoucí spouštění obslužné rutiny události, kde je jako atribut na `RowDeleting` obslužné rutiny události omezuje na uživatele ve Správci provedení role.


> [!NOTE]
> `PrincipalPermission` Atribut je reprezentována jako třída v `System.Security.Permissions` oboru názvů. Nezapomeňte přidat `Imports System.Security.Permissions` příkaz v horní části souboru kódu třídy pro import tento obor názvů.


Pokud nějakým způsobem, který není správcem pokusí spustit `RowDeleting` obslužné rutiny události nebo pokud není nadřízeného nebo bez oprávnění správce pokusí spustit `RowUpdating` obslužné rutiny události, bude vyvolána modul runtime rozhraní .NET `SecurityException`.


[![Pokud kontext zabezpečení nemá oprávnění k provedení metody, je vyvolána výjimka zabezpečení](role-based-authorization-vb/_static/image47.png)](role-based-authorization-vb/_static/image46.png)

**Obrázek 16**: Pokud kontext zabezpečení nemá oprávnění k provedení metody, `SecurityException` je vyvolána ([Kliknutím zobrazit obrázek v plné velikosti](role-based-authorization-vb/_static/image48.png))


Kromě stránek ASP.NET mnoho aplikací mít také architekturu, která zahrnuje různé vrstvy, jako je například obchodní logiku a Data přístup vrstvy. Tyto vrstvy jsou obvykle implementovány jako knihovny tříd a nabízet třídy a metody pro provádění obchodní logiku a data související funkce. `PrincipalPermission` Atribut je vhodná pro použití autorizačních pravidel na také tyto vrstvy.

Další informace o používání `PrincipalPermission` atribut definovat pravidla ověřování na třídy a metody, získáte na [Scott Guthrie](https://weblogs.asp.net/scottgu/)na položce blogu [přidáním autorizačních pravidel pro obchodní a datové vrstvy pomocí `PrincipalPermissionAttributes` ](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Souhrn

V tomto kurzu jsme se podívali na tom, jak zadat hrubým a spočívá v jemné autorizační pravidla založená na role uživatele. SOUBOR ASP. Funkce autorizace adresy URL pro NET umožňuje vývojáři stránky zadejte co identity se povoluje nebo odepírá přístup k jaké stránky. Jak jsme viděli zpět v <a id="_msoanchor_10"> </a> [ *autorizace na základě uživatele* ](../membership/user-based-authorization-vb.md) kurz, autorizace adres URL pravidla lze použít na základě uživatele uživatelů. Je možné je použít na základě rolí role jako jsme viděli v kroku 1 tohoto kurzu.

Spočívá v jemné autorizační pravidla mohou být použity deklarativně nebo prostřednictvím kódu programu. V kroku 2 jsme se podívali na použití funkce kolekci RoleGroups ovládací prvek LoginView k vykreslení odlišný výstup na základě rolí hostujícími uživatelů. Také jsme se podívali na způsoby, jak určit prostřednictvím kódu programu, pokud uživatel patří do určité role a postup odpovídajícím způsobem upravit na stránce funkce.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Přidávání do obchodní a datové vrstvy pomocí autorizačních pravidel`PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Zkoumání ASP.NET 2.0 je členství, role a profil: práce s rolemi](http://aspnet.4guysfromrolla.com/articles/121405-1.aspx)
- [Seznam bezpečnostní otázku pro technologii ASP.NET 2.0](https://msdn.microsoft.com/en-us/library/ms998375.aspx)
- [Technická dokumentace k `<roleManager>` – Element](https://msdn.microsoft.com/en-us/library/ms164660.aspx)

### <a name="about-the-author"></a>O autorovi

Scott Meisnerová, vytvořit více knih ASP/ASP.NET a zakladatele 4GuysFromRolla.com, má byla od 1998 práce s technologií Microsoft Web. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k  *[Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott lze dosáhnout za [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) nebo prostřednictvím svého blogu v [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Zvláštní poděkování...

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři pro účely tohoto kurzu zahrnují Suchi Banerjee a Teresy Murphy. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](assigning-roles-to-users-vb.md)
