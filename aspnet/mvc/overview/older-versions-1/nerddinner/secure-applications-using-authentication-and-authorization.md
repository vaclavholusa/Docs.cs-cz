---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: "Zabezpečení aplikací s použitím ověřování a autorizace | Microsoft Docs"
author: microsoft
description: "Krok 9 ukazuje, jak přidat ověřování a autorizace pro zabezpečení naše aplikace NerdDinner tak, aby uživatelé potřebují k registraci a přihlaste se k webu pro vytvoření..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: a23b2cf4d1728624698c0db49c25ea7efd3af67d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="secure-applications-using-authentication-and-authorization"></a>Zabezpečení aplikací s použitím ověřování a autorizace
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Toto je kroku 9 bezplatný [kurz aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , nevystavíte slabé stránky zabezpečení – prostřednictvím postup sestavení malá, ale dokončení, webové aplikace pomocí ASP.NET MVC 1.
> 
> Krok 9 ukazuje, jak přidat ověřování a autorizace pro zabezpečení naše aplikace NerdDinner tak, aby uživatelé potřebují k registraci a přihlaste se k webu pro vytvoření nové večeří a pouze uživatele, který je hostitelem večeři ho můžete upravit později.
> 
> Pokud používáte ASP.NET MVC 3, doporučujeme provedením [získávání spuštěna s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Hudba úložiště](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.


## <a name="nerddinner-step-9-authentication-and-authorization"></a>NerdDinner krok 9: Ověřování a autorizace

Nyní naše NerdDinner aplikace uděluje každý, kdo návštěvou webu umožňuje vytvářet a upravovat podrobnosti o všech večeři. Umožňuje změnit tak, aby uživatelé potřebují k registraci a přihlaste se k webu k vytvoření nové večeří a přidat omezení tak, aby pouze uživatele, který je hostitelem večeři můžete později upravovat.

Chcete-li povolit tuto funkci použijeme ověřování a autorizace pro zabezpečení naše aplikace.

### <a name="understanding-authentication-and-authorization"></a>Principy ověřování a autorizace

*Ověřování* je proces identifikace a ověření identity klienta přístupu k aplikaci. PUT jednodušeji, je o identifikaci "kdo koncový uživatel je při návštěvě webu". ASP.NET podporuje více způsobů k ověřování uživatelů prohlížeče. Nejběžnější metoda ověření používaná pro internetové webové aplikace, nazývá "Ověřování pomocí formulářů". Ověřování pomocí formulářů umožňuje vývojáři vytvářet formuláře HTML přihlášení v rámci své aplikace a ověřte uživatelské jméno nebo heslo, které koncovým odešle proti databáze nebo jiného úložiště přihlašovacích údajů heslo. Pokud je správná kombinace uživatelského jména a hesla, vývojář na požádání ASP.NET vystavit šifrovaného souboru cookie HTTP k identifikaci uživatele napříč budoucí požadavky. Jsme budete pomocí naší aplikaci NerdDinner ověřování pomocí formulářů.

*Autorizace* je proces pro určení toho, zda ověřený uživatel má oprávnění pro přístup k určitému adresu URL nebo prostředku nebo provedení několika akcí. Například v naší aplikaci NerdDinner jsme budete chtít autorizovat, přístup pouze uživatelé, kteří jsou přihlášeni */večeří/vytvořit* adresy URL a vytvořit nové večeří. Také jsme budete chtít přidat logiku ověřování, aby pouze uživatele, který je hostitelem večeři jej mohli upravit – a všem ostatním uživatelům odepřít přístup pro úpravy.

### <a name="forms-authentication-and-the-accountcontroller"></a>Ověřování pomocí formulářů a AccountController

Výchozí šablona projektu sady Visual Studio pro architekturu ASP.NET MVC automaticky povolí ověřování pomocí formulářů, při vytvoření nové aplikace ASP.NET MVC. Také automaticky přidá na předem vytvořený účet přihlašovací stránky implementace do projektu – které skutečně usnadňuje integraci zabezpečení v rámci lokality.

Když není ověřený uživatel k ní přistupují Site.master stránku zobrazí odkaz "Přihlášení" v pravé horní lokality:

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

Kliknutím na odkaz "Přihlášení" trvá uživateli */Account/přihlášení* adresy URL:

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

Návštěvníci, kteří se nezaregistrovali, můžete provést kliknutím na odkaz "Register" – to bude trvat, aby */Account/registrace* adresy URL a povolení jejich zadejte podrobnosti o účtu:

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

Klepnutím na tlačítko "Register" bude vytvoření nového uživatele v rámci systému členství technologie ASP.NET a ověření uživatele na webu pomocí ověřování pomocí formulářů.

Pokud je uživatel přihlášený, Site.master změní pravé horní části stránky a výstup "Vítejte [username]!" zprávy a vykreslí "Odhlásit uživatele" odkaz místo "přihlášení" jednu. Kliknutím na odkaz "Protokolu vypnuto" neodhlásí uživatele:

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

Výše uvedené funkce přihlášení, odhlášení a registrace je implementována v rámci AccountController třídy, která byla přidána do našich projekt Visual Studio vytvoření projektu. Uživatelské rozhraní pro AccountController je implementovaná pomocí šablon zobrazení v adresáři \Views\Account:

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

Třída AccountController používá systém ověřování ASP.NET pomocí formulářů k vydávání šifrované ověřovací soubory cookie a rozhraní API členství technologie ASP.NET pro ukládání a ověřit uživatelská jména a hesla. Rozhraní API členství technologie ASP.NET je rozšiřitelný a umožňuje žádné přihlašovací údaje úložiště hesel má být použit. ASP.NET se dodává s implementací předdefinované členství zprostředkovatele, které ukládání uživatelského jména a hesla v databázi SQL a do služby Active Directory.

Nakonfigurujeme poskytovatele, kterého členství v naší aplikaci NerdDinner měli používat otevřením souboru "web.config" v kořenovém adresáři projektu a hledá &lt;členství&gt; část v něm. Výchozí soubor web.config po vytvoření projektu přidáno registruje zprostředkovatele členství SQL a nakonfiguruje ho na použití připojovacího řetězce s názvem "ApplicationServices" a zadejte umístění databáze.

Připojovací řetězec "ApplicationServices" výchozí (který je zadán v rámci &lt;connectionStrings&gt; části souboru web.config) je nakonfigurovaný na použití SQL Express. Odkazuje na databázi SQL Express s názvem "ASPNETDB. MDF"v části aplikace" aplikace\_Data "directory. Pokud tato databáze neexistuje prvním rozhraní API členství slouží v aplikaci, ASP.NET automaticky vytvořit databázi a zřídit schéma databáze příslušné členství v něm:

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

Pokud místo použití SQL Express jsme chtěli použít plná instance systému SQL Server (nebo připojení ke vzdálené databázi), všechny by potřebujeme úkolů je aktualizovat připojovací řetězec "ApplicationServices" v souboru web.config a ujistěte se, že schéma odpovídající členství přidala k databázi, kterou se odkazuje na. Můžete spustit "aspnet\_regsql.exe" nástroj v adresáři \Windows\Microsoft.NET\Framework\v2.0.50727\ přidat příslušného schématu pro členství a další služby aplikace ASP.NET do databáze.

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a>Autorizace večeří nebo vytvořit adresu URL, pomocí filtru [Authorize]

Nebyly k dispozici psaní jakéhokoli kódu povolení zabezpečeného ověřování a účet správy implementace NerdDinner aplikace. Uživatelé mohou registrovat nové účty s naše aplikace a přihlášení/odhlášení lokality.

Nyní jsme můžete přidat logiku ověřování do aplikace a používat ověřování stavu a uživatelské jméno návštěvníky řídit, co mohou a nemůže provádět v rámci lokality. Začněme přidáním autorizace logiku do metody akce "Vytvořit" naše DinnersController třídy. Konkrétně, který bude vyžadovat jsme uživatele, kteří používají */večeří/vytvořit* adresu URL, musíte být přihlášení. Pokud uživatel není přihlášený jsme budete přesměrování je na přihlašovací stránku tak, aby se můžete přihlásit.

Implementace této logiky je velmi snadné. Jsme úkolů stačí přidat atribut filtru [autorizovat] na našem metody akce vytvořit takto:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

ASP.NET MVC podporuje možnost vytvářet "filtry akce", které lze použít k implementaci opakovaně použitelné logiku, která může být použita deklarativně na metody akce. Filtr [autorizovat] je jedním z filtrů vestavěná akce poskytované ASP.NET MVC a umožňuje vývojář deklarativně použít autorizační pravidla řadiče třídy a metody akce.

Při použití bez parametrů (jako výše) vynucuje [autorizovat] filtru, musíte být přihlášení uživatele, který vytvořil požadavek metody akce – a se automaticky přesměruje v prohlížeči adresu URL pro přihlášení Pokud nejsou. Při provádění této přesměrování jako argument řetězce dotazu byla předána původně požadovanou URL adresu (například: / Account/přihlášení? ReturnUrl = % 2fDinners % 2fCreate). AccountController se pak přesměruje uživatele zpět na původně požadovanou URL adresu po jejich přihlášení.

Filtr [autorizovat] volitelně podporuje možnost zadejte "Uživatelé" nebo "Role" vlastnost, která umožňuje vyžadovat, že je uživatel i přihlášen v a v seznamu povolených uživatelů nebo členem role zabezpečení povolené. Následující kód například pouze umožňuje dvě konkrétní uživatele, "scottgu" a "billg", přístup k večeří nebo vytvořit adresu URL:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

Vložení konkrétní uživatelská jména kódu obvykle používat poměrně zrušení udržovatelný. Lepším řešením je definovat vyšší úrovně "role" kód zjišťuje proti a potom k mapování uživatelů do role pomocí databáze nebo systému služby active directory (povolení skutečného uživatelského seznamu mapování ukládaly externě z kódu). Technologie ASP.NET obsahuje předdefinovaná role správy rozhraní API a také sadu předdefinovaných zprostředkovatelů rolí (včetně těch, které jsou pro SQL a služby Active Directory), které vám mohou pomoci provést toto mapování uživatelů a rolí. Potom jsme může aktualizujte kód a Povolit jenom uživatelé v rámci role konkrétní "admin" přístup k večeří nebo vytvořit adresu URL:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a>Při vytváření pomocí vlastnosti User.Identity.Name večeří

Můžeme načíst uživatelské jméno aktuálně přihlášeného uživatele žádost pomocí vlastnosti User.Identity.Name zveřejněné na základní třídy Kontroleru.

Starší při implementovali jsme verze HTTP POST metody akce naše Create() jsme měli pevně zakódované vlastnost "HostedBy" večeři statický řetězec. Jsme můžete nyní aktualizovat tento kód, aby místo toho použijte vlastnost User.Identity.Name, jakož i automaticky přidat odpověď pro vytváření večeře hostitele:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

Protože atribut [autorizovat] jsme přidali metodu Create(), ASP.NET MVC zajistí, že provedení metody akce pouze pokud je přihlášení uživatele, kteří navštěvují večeří nebo vytvořit adresu URL na webu. Hodnota vlastnosti User.Identity.Name jako takový bude vždy obsahovat platné uživatelské jméno.

### <a name="using-the-useridentityname-property-when-editing-dinners"></a>Při úpravě pomocí vlastnosti User.Identity.Name večeří

Teď umožňuje přidat některé autorizace logiky, která omezuje uživatele tak, aby mohou upravovat pouze vlastnosti večeří, které jsou hostiteli sami.

Usnadní to, nejprve přidáme metodu helper "IsHostedBy(username)" naše objekt večeři (v rámci Dinner.cs třídu, kterou jsme vytvořili výše). Tato pomocná metoda vrátí hodnotu PRAVDA nebo NEPRAVDA v závislosti na jestli zadané uživatelské jméno odpovídá vlastnost večeři HostedBy a zapouzdří logice nezbytným k provedení porovnání velká a malá písmena řetězce je:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

Potom přidáme [autorizovat] atribut do metody akce Edit() v rámci naší DinnersController třídy. Tím bude zajištěno, že uživatelé musíte být přihlášení k žádosti */Dinners/Edit / [id]* adresy URL.

Kód jsme poté můžete přidat do našich upravit metody, které používá Dinner.IsHostedBy(username) pomocnou metodu k ověření, že přihlášeného uživatele odpovídá večeři hostitele. Pokud uživatel není hostiteli, jsme budete zobrazit "InvalidOwner" a ukončení požadavku. Kód k tomu vypadá níže:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

Jsme klikněte pravým tlačítkem na \Views\Dinners adresář a vyberte Přidat -&gt;zobrazit příkaz nabídky k vytvoření nového zobrazení "InvalidOwner". Budete ho s naplníme následující chybová zpráva:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

A teď když se uživatel pokusí upravit večeři, které budou nevlastníte, že budete získat chybová zpráva:

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

Stejný postup jsme můžete opakovat pro Delete() metody akce v kontroleru zamknout oprávnění k odstraňování večeří také a ujistěte se, že pouze hostitel večeři můžete odstranit.

### <a name="showinghiding-edit-and-delete-links"></a>Zobrazení nebo skrytí upravit a odstranit odkazy

Jsme se propojení na metodu akce upravit a odstranit naše DinnersController třídy z našich podrobnosti adresy URL:

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

Aktuálně jsou jsme se zobrazuje upravit a odstranit odkazy na akce bez ohledu na to, zda je hostitel večeře návštěvník na adresu URL podrobnosti. Umožňuje změnit tak, aby odkazy se zobrazí, pouze pokud je vlastníkem večeře hostujícími uživatele.

Details() metody akce v rámci naší DinnersController načte objekt večeři a předává je jako objekt modelu pro naše zobrazit šablonu:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

Aktualizujeme naše zobrazení šablony podmíněně zobrazit či skrýt odkazy upravit a odstranit pomocí Dinner.IsHostedBy() Pomocná metoda jako níže:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a>Další kroky

Nyní podíváme, jak můžeme provést aktivaci ověřeným uživatelům zasílání zpráv rysy pro večeří pomocí rozhraní AJAX.

>[!div class="step-by-step"]
[Předchozí](implement-efficient-data-paging.md)
[další](use-ajax-to-deliver-dynamic-updates.md)
