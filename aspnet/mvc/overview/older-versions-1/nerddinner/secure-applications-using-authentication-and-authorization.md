---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: Zabezpečení aplikací ověřováním a autorizací | Dokumentace Microsoftu
author: microsoft
description: Krok 9 ukazuje, jak přidat ověřování a autorizace pro zabezpečení naší aplikace NerdDinner tak, aby uživatelé potřebují k registraci a přihlaste se k webu k vytvoření...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: d5f1b26312f11fd6d4ab500c7f24a4d89d428e38
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757110"
---
<a name="secure-applications-using-authentication-and-authorization"></a>Zabezpečení aplikací ověřováním a autorizací
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Toto je krok 9 bezplatného [kurz vývoje aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , který procházení procházení po tom, jak sestavit malý, ale bylo možné provést, webové aplikace pomocí ASP.NET MVC 1.
> 
> Krok 9 ukazuje, jak přidat ověřování a autorizace pro zabezpečení naší aplikace NerdDinner tak, aby uživatelé potřebují k registraci a přihlášení na web za účelem vytvoření nové večeří a jenom uživatel, který je hostitelem dinner můžete upravit ho později.
> 
> Pokud používáte ASP.NET MVC 3, doporučujeme je provést [získávání začít s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.


## <a name="nerddinner-step-9-authentication-and-authorization"></a>NerdDinner krok 9: Ověřování a autorizace

Teď naše NerdDinner aplikace poskytuje všem uživatelům následujícím webu umožňuje vytvářet a upravovat podrobnosti libovolné dinner. Změňme ji tak, aby uživatelé potřebují k registraci a přihlaste se k webu a vytvořte nový večeří, tak, aby jenom uživatel, který je hostitelem dinner můžete upravit ho později přidat omezení.

Chcete-li tuto možnost povolte, použijeme ověřování a autorizace pro zabezpečení aplikace.

### <a name="understanding-authentication-and-authorization"></a>Principy ověřování a autorizace

*Ověřování* je proces identifikace a ověření identity klienta přístupu k aplikaci. Jednoduše řečeno je o identifikaci ", koncový uživatel je při návštěvě webu". Technologie ASP.NET podporuje více způsobů, jak ověřovat uživatele prohlížeče. Nejběžnější metodu ověřování použitou pro webové aplikace, nazývá "Ověřování pomocí formulářů". Ověřování pomocí formulářů umožňuje vývojářům vytvářet formuláře HTML přihlášení do své aplikace a její ověření uživatelského jména a hesla, které koncový uživatel odešle proti databázi nebo jiných přihlašovacích údajů úložiště hesel. Pokud kombinace uživatelského jména a hesla je správný, Vývojář potom požádejte ASP.NET vydat do zašifrovaného souboru cookie HTTP k identifikaci uživatele napříč budoucí požadavky. My se vám s použitím ověřování pomocí formulářů s naší aplikace NerdDinner.

*Autorizace* je procesu, který určuje, zda ověřený uživatel má oprávnění pro přístup k určité adresy URL/prostředku nebo k provedení nějaké akce. Například v rámci naší aplikace NerdDinner nám budete chtít povolit, že přístup pouze uživatelé, kteří jsou přihlášení */večeří/vytvořit* adresy URL a vytvořit nové večeří. Můžeme také vhodné přidat logiku autorizace tak, aby jenom uživatel, který je hostitelem dinner jej mohli upravit – a všem ostatním uživatelům odepřít přístup pro úpravy.

### <a name="forms-authentication-and-the-accountcontroller"></a>Ověřování pomocí formulářů a AccountController

Výchozí šablona projektu sady Visual Studio pro architekturu ASP.NET MVC automaticky povolí ověřování pomocí formulářů, při vytvoření nové aplikace ASP.NET MVC. Také automaticky přidá na předem sestavených účet přihlašovací stránky implementace do projektu – díky tomu je skutečně snadno integrovat zabezpečení v rámci lokality.

Výchozí stránku předlohy Site.master zobrazí odkaz "Přihlásit" v pravé horní části webu, když nebude ověřený uživatel přístup:

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

Kliknutím na odkaz "Přihlásit" trvá uživateli */účet/přihlášení* adresy URL:

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

Návštěvníci, kteří se ještě nezaregistrovali to tak, že kliknutím na odkaz "Register" – které je pro vás přesměruje */účtu/registrace* adresy URL a umožnit jim zadejte podrobnosti o účtu:

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

Kliknutím na tlačítko "Register" bude vytvoření nového uživatele v rámci systému členství technologie ASP.NET a ověřit uživatele na serveru pomocí ověřování pomocí formulářů.

Pokud je uživatel přihlášený, Site.master změní pravé horní části stránky a výstup "Vítejte [username]!" zpráva a vykreslí "odhlásit" propojení namísto "přihlášení" jeden. Kliknutím na odkaz "Odhlásit" odhlásí uživatele:

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

Výše uvedené přihlášení, odhlášení a registrace funkce je implementovaná v rámci AccountController třídy, která byla přidána do našich projektu ve Visual Studio při vytvoření projektu. V uživatelském rozhraní AccountController je implementováno pomocí zobrazení šablony v adresáři \Views\Account:

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

Třída AccountController využívá systém ověřování formulářů ASP.NET vydat šifrované ověřovací soubory cookie a rozhraní API členství technologie ASP.NET k ukládání a ověřit uživatelská jména a hesla. Rozhraní API členství technologie ASP.NET je rozšiřitelný a umožňuje všechny přihlašovací údaje úložiště hesel má být použit. Technologie ASP.NET se dodává s integrovanou členství zprostředkovatele implementace, které ukládání uživatelského jména a hesla, SQL database, nebo v rámci služby Active Directory.

Nakonfigurujeme kterého zprostředkovatele členství naší aplikace NerdDinner by měl použít soubor "web.config" v kořenovém adresáři projektu otevřete a vyhledáním &lt;členství&gt; části v něm. Výchozí soubor web.config přidat, pokud byl projekt vytvořen registruje zprostředkovatele členství SQL a nakonfiguruje ho, aby použijte připojovací řetězec s názvem "ApplicationServices" a zadejte umístění databáze.

Výchozí připojovací řetězec "ApplicationServices" (který je zadán v rámci &lt;connectionStrings&gt; část souboru web.config) je nakonfigurován na použití SQL Express. Odkazuje na databázi SQL Express s názvem "ASPNETDB. MDF"v rámci vaší aplikace" aplikace\_Data "adresáře. Pokud tuto databázi neexistuje okamžiku, kdy se používá rozhraní API pro členství v rámci aplikace, technologie ASP.NET automaticky vytvořit databázi a zřizování schématu databáze odpovídající členství v rámci něj:

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

Pokud místo SQL Express, jsme chtěli použít plná instance systému SQL Server (nebo připojení ke vzdálené databázi), všechny by potřebujeme úkolů je aktualizovat připojovací řetězec "ApplicationServices" v souboru web.config a ujistěte se, že schéma odpovídající členství byla přidána do databáze, na který odkazuje na. Můžete spustit "aspnet\_regsql.exe" nástroje v adresáři \Windows\Microsoft.NET\Framework\v2.0.50727\ přidat příslušného schématu pro členství a dalších aplikačních služeb ASP.NET s databází.

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a>Autorizace URL večeří/vytvořit pomocí [Authorize] filtr

Psaní kódu zabezpečené ověřování a implementaci účtu správy pro aplikace NerdDinner nebyly k dispozici. Uživatelé můžou registrovat nové účty s naší aplikace a přihlášení/odhlášení lokality.

Teď můžeme přidat logiku ověřování k aplikaci a můžete řídit, co mohou a nemůže provádět v rámci stránky stav ověření a uživatelské jméno návštěvníků. Začněme přidáním autorizace logiku do metody akce "Vytvořit" náš DinnersController třídy. Konkrétně jsme se požadovat, aby uživatele, kteří používají */večeří/vytvořit* adresy URL musíte být přihlášeni. Pokud uživatel není přihlášený přesměrujeme je na přihlašovací stránku tak, aby se můžete přihlásit.

Implementace tuto logiku je poměrně snadné. Všechny potřebujeme úkol má přidat atribut [Authorize] filtr na naše metody akce vytvořit takto:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

ASP.NET MVC podporuje schopnost vytvářet "filtrů akce", které lze použít k implementaci opakovaně použitelné logiku, která je deklarativně použít na metody akce. [Authorize] filtr je jeden z filtrů vestavěná akce poskytované rozhraní ASP.NET MVC a umožňuje vývojáři deklarativně aplikuje autorizační pravidla do třídy kontroleru a metody akce.

Při použití bez parametrů (jako výše) vynucuje [Authorize] filtr, musíte být přihlášeni uživatele, který vytváří požadavek metody akce – a automaticky přesměruje prohlížeč na adresu URL pro přihlášení, nejsou-li. Při provádění této přesměrování původně požadovanou URL adresu je předán jako argument řetězce dotazu (Příklad: / účet/přihlášení? ReturnUrl = % 2fDinners % 2fCreate). AccountController pak přesměruje uživatele zpět na původně požadovanou URL adresu po jejich přihlášení.

[Authorize] filtr volitelně podporuje možnost zadat vlastnost "Users" nebo "Role", který můžete použít tak, aby vyžadovala, že obou přihlášení uživatele a v seznamu povolených uživatelů nebo členem role zabezpečení povolené. Například následující kód povoluje jenom dva konkrétní uživatele, "scottgu" a "billg" na adresu URL večeří nebo vytvoření:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

Vložení konkrétní uživatelská jména v kódu je spíše poměrně zrušení udržovatelného ale. Lepším řešením je definování vyšší úrovně "role", která zkontroluje kód oproti a pak pro mapování uživatelů do role pomocí databáze nebo systému služby active directory (povolení skutečného uživatelského seznamu mapování externě ukládaly z kódu). Technologie ASP.NET obsahuje předdefinovanou roli správy rozhraní API, jakož i předdefinované sady zprostředkovatelů rolí (včetně těch, které jsou pro SQL a službu Active Directory), které vám mohou pomoci provést toto mapování uživatelů a rolí. Jsme pak může aktualizovat kód a Povolit jenom uživatelé v rámci role konkrétní "admin" Otevřít adresu URL večeří nebo vytvoření:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a>Pomocí vlastnosti User.Identity.Name při vytváření večeří

Nemůžeme načíst uživatelské jméno aktuálně přihlášeného uživatele žádost pomocí vlastnosti User.Identity.Name zveřejněné na základní třídu Kontroleru.

Starší když jsme implementovali verze HTTP POST metody akce naše Create() jsme měli pevně zakódované vlastnost "HostedBy" Dinner statický řetězec. Jsme můžete nyní aktualizovat tento kód, aby místo toho použijte vlastnost User.Identity.Name, jakož i automaticky přidat odpověď pro vytváření společnosti Dinner hostitele:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

Protože jsme přidali atribut [Authorize] metodě Create(), ASP.NET MVC zajistí, že spuštění metody akce pouze pokud je uživatel navštívit adresu URL večeří nebo vytvoření přihlášení na webu. Hodnota vlastnosti User.Identity.Name v důsledku toho bude vždy obsahovat platné uživatelské jméno.

### <a name="using-the-useridentityname-property-when-editing-dinners"></a>Pomocí vlastnosti User.Identity.Name při úpravách večeří

Přidejme teď některé logiku autorizace, která omezuje uživatele tak, aby mohou upravovat pouze vlastnosti večeří, které jsou hostiteli sami.

Pro usnadnění, nejprve přidáme metodu helper "IsHostedBy(username)" naše společnost Dinner objektu (v rámci Dinner.cs částečné třídy, které jsme vytvořili výše). Tato pomocná metoda vrátí hodnotu PRAVDA nebo NEPRAVDA v závislosti na tom, jestli zadané uživatelské jméno odpovídá vlastnost Dinner HostedBy a zapouzdří logiku potřebnou k provádění porovnání nerozlišuje velikost písmen řetězců z nich:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

Potom přidáme atribut [Authorize] na Edit() metody akce v rámci naší DinnersController třídy. Tím se zajistí, že uživatelé musíte být přihlášení k žádosti */Dinners/Edit / [id]* adresy URL.

Naše úpravy metodám, které používá Dinner.IsHostedBy(username) pomocnou metodu k ověření, že přihlášeného uživatele odpovídá hostitele Dinner jsme poté můžete přidat kód. Pokud uživatel není hostiteli, vytvoříme zobrazení "InvalidOwner" a ukončení požadavku. Kód k tomu bude vypadat jako následující:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

My pak klikněte pravým tlačítkem na adresář \Views\Dinners a zvolte možnost Add -&gt;zobrazit příkaz k vytvoření nového zobrazení "InvalidOwner". Budete ji naplníme následující chybová zpráva:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

A když se uživatel pokusí upravit dinner, který není vlastníkem, získají nyní první chybovou zprávu:

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

V rámci kontroleru zamezit oprávnění k odstraňování večeří také a zajistili, že pouze hostitele večeři můžete odstranili jsme můžete opakujte stejný postup pro Delete() metody akce.

### <a name="showinghiding-edit-and-delete-links"></a>Zobrazení nebo skrytí upravit a odstranit odkazy

Na metodu akce upravit a odstranit třídy Naše DinnersController jsme propojení z našich podrobnosti adresy URL:

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

Právě jsme se zobrazují upravit a odstranit odkazy na akce bez ohledu na to, zda návštěvníky na adresu URL podrobnosti hostitele společnosti dinner. Umožňuje změnit tak, aby odkazy se zobrazí jenom v případě hostujícími uživatel je vlastníkem společnosti dinner.

Details() metody akce v rámci naší DinnersController načte objekt Dinner a předá jej jako objekt modelu do našich zobrazení šablony:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

Aktualizujeme naše zobrazit šablonu podmíněně zobrazit/skrýt odkazy upravit a odstranit pomocí Dinner.IsHostedBy() Pomocná metoda podobná níže uvedenému příkladu:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a>Další kroky

Nyní Podívejme se na tom, jak jsme povolili ověřeným uživatelům RSVP pro večeří pomocí rozhraní AJAX.

> [!div class="step-by-step"]
> [Předchozí](implement-efficient-data-paging.md)
> [další](use-ajax-to-deliver-dynamic-updates.md)
