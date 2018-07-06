---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: Prevence útoků na otevřeném přesměrování (C#) | Dokumentace Microsoftu
author: jongalloway
description: Tento kurz vysvětluje, jak lze zabránit útokům otevřené přesměrování ve svých aplikacích ASP.NET MVC. Tento kurz popisuje změny, které mají za cíl...
ms.author: aspnetcontent
ms.date: 02/27/2014
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: 33d2d050805d9b65741c121cdb2b65a59e1ea392
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813197"
---
<a name="preventing-open-redirection-attacks-c"></a>Prevence útoků na otevřeném přesměrování (C#)
====================
podle [Jon Galloway](https://github.com/jongalloway)

> Tento kurz vysvětluje, jak lze zabránit útokům otevřené přesměrování ve svých aplikacích ASP.NET MVC. Tento kurz popisuje změny, které byly provedeny v AccountController v architektuře ASP.NET MVC 3 a ukazuje, jak je možné použít tyto změny v existujících ASP.NET MVC 1.0 a 2 aplikacemi.


## <a name="what-is-an-open-redirection-attack"></a>Co je útok otevřené přesměrování?

Libovolné webové aplikace, který přesměruje na adresu URL, která je zadáno pomocí požadavku, například data řetězce dotazu nebo formuláře může být potenciálně úmyslně přesměrovat uživatele na škodlivé, externí adresu URL. Toto úmyslné poškozování, se nazývá otevřené přesměrování útoku.

Pokaždé, když se vaše aplikace logiky se přesměruje na zadané adresy URL, je nutné ověřit, že adresa URL pro přesměrování bylo neoprávněně manipulováno. Přihlašovací údaje používané pro ASP.NET MVC 1.0 a ASP.NET MVC 2 ve výchozím nastavení AccountController je ohrožen útoky přesměrování otevřete. Je naštěstí snadné k aktualizaci existující aplikace pomocí opravy z technologie ASP.NET MVC 3 ve verzi Preview.

Informace o tom ohrožení zabezpečení, Podívejme se na fungování přesměrování přihlášení v projektu webové aplikace ASP.NET MVC 2 výchozí. V této aplikaci pokus o navštivte akce kontroleru, který má atribut [Authorize] přesměruje neoprávněným uživatelům /Account/LogOn zobrazení. Toto přesměrování, které /Account/LogOn bude obsahovat řetězec dotazu parametr returnUrl tak, aby uživatel může být vrácen na původně požadovanou URL adresu po jejich úspěšném přihlášení.

Na snímku obrazovky níže můžeme vidět, že pokus o přístup k zobrazení /Account/ChangePassword, když není přihlášený výsledkem přesměrování /Account/LogOn? ReturnUrl = % 2fAccount % 2fChangePassword % 2f.

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

**Obrázek 01**: přihlašovací stránku s otevřené přesměrování

Protože parametr querystring ReturnUrl neověří, útočník ho upravit, abyste vložit libovolnou adresu URL do parametru provádět útok otevřené přesměrování. Chcete-li to ukazuje, můžeme upravit parametr ReturnUrl [ http://bing.com ](http://bing.com), takže bude Výsledná adresa URL pro přihlášení/účet/přihlášení? ReturnUrl =<http://www.bing.com/>. Po úspěšném přihlášení k webu jsme se přesměrují na [ http://bing.com ](http://bing.com). Protože toto přesměrování neověří, může místo toho přejděte na škodlivým webům, která se pokusí přimět uživatele.

### <a name="a-more-complex-open-redirection-attack"></a>Složitější otevřené přesměrování útoku

Protože útočník ví, že jsme se snažíte přihlásit konkrétní web, který nám zranitelný jsou zvlášť nebezpečné útoky otevřené přesměrování [útoku](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx). Například útočník může odeslat škodlivý e-mailů uživatelům webu za účelem zachycení jejich hesla. Podívejme se na tom, jak by to fungovalo v lokalitě NerdDinner. (Mějte na paměti, živého webu NerdDinner je aktualizovaná pro ochranu před útoky otevřené přesměrování.)

Nejprve útočník odešle nám odkaz na stránku pro přihlášení na NerdDinner, která zahrnuje přesměrování na stránku falešných:

[http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

Všimněte si, že návratová adresa URL odkazuje na nerddiner.com, které chybí "n" z dinner aplikace word. V tomto příkladu to je doména, která určuje, že útočník. Když jsme na výše uvedeném odkazu, jsme přejdete na oprávněné aplikace NerdDinner.com přihlašovací stránku.

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

**Obrázek 02**: NerdDinner přihlašovací stránku s otevřené přesměrování

Jsme správně přihlásit, přihlášení akce ASP.NET MVC AccountController nám přesměruje na adresu URL zadanou v parametru querystring returnUrl. V takovém případě je adresu URL, který útočník přešel, což je [ http://nerddiner.com/Account/LogOn ](http://nerddiner.com/Account/LogOn). Pokud jsme velmi watchful, že je velmi pravděpodobné že nebude, zjistíme zvlášť, protože byl pozor, abyste měli jistotu, že útočník jejich falešných stránka vypadá stejně jako legitimní přihlašovací stránky. Tato přihlašovací stránku obsahuje, chybová zpráva požaduje, můžeme přihlásit znovu. Clumsy nám, jsme musí zadali heslo.

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

**Obrázek 03**: založených na zfalšovaných NerdDinner přihlašovací obrazovky

Když znovu jsme naše uživatelské jméno a heslo, falešných přihlašovací stránku uloží informace a nám odešle zpět na oprávněné aplikace NerdDinner.com lokality. V tomto okamžiku lokality aplikace NerdDinner.com již ověřen nám, takže falešných přihlašovací stránku lze přesměrovat přímo na tuto stránku. Konečným výsledkem je, že útočník má naše uživatelské jméno a heslo a jsme neberou v úvahu, že jsme jste poskytli k nim.

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a>Prohlížení zranitelné kód v přihlášení akce AccountController

Kód pro proces přihlášení v aplikaci ASP.NET MVC 2 je uveden níže. Všimněte si, že po úspěšném přihlášení kontroleru vrátí přesměrování adresa returnUrl. Uvidíte, že žádné ověření se provádí proti parametr returnUrl.

**Výpis 1 – ASP.NET MVC 2 přihlášení v akci `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

Nyní Pojďme se podívat na změny na ASP.NET MVC 3 přihlášení akci. Tento kód byl změněn na parametr returnUrl ověřit pomocí volání nové metody System.Web.Mvc.Url pomocná třída s názvem `IsLocalUrl()`.

**Výpis 2 – ASP.NET MVC 3 přihlášení v akci `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

To byl změněn na ověřte parametr návratové adresy URL tak, že volání nové metody ve třídě pomocné rutiny System.Web.Mvc.Url `IsLocalUrl()`.

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a>Ochrana ASP.NET MVC 1,0 a MVC 2 aplikace

Přidáním Pomocná metoda IsLocalUrl() a aktualizuje se přihlášení akce, která má parametr returnUrl ověřit jsme mohou využít výhod změny architektuře ASP.NET MVC 3 naší stávající ASP.NET MVC 1.0 a 2 aplikacemi.

Metoda UrlHelper IsLocalUrl() ve skutečnosti pouze volání do metody v System.Web.WebPages jako toto ověření slouží také v aplikacích ASP.NET Web Pages.

**Výpis 3 – metoda IsLocalUrl() z ASP.NET MVC 3 UrlHelper `class`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

Metoda IsUrlLocalToHost obsahuje logiku skutečné ověřování, jak je uvedeno v informacích 4.

**Část 4 – metoda IsUrlLocalToHost() ze třídy System.Web.WebPages RequestExtensions**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

V naše technologie ASP.NET MVC 1,0 nebo 2 aplikace přidáme IsLocalUrl() metodu AccountController, ale můžete dohlédněte na to, pokud je to možné ho přidat do samostatné pomocné třídě. Verze technologie ASP.NET MVC 3 IsLocalUrl() učiníme dvě malých změn, tak, že bude fungovat uvnitř AccountController. Nejprve Změníme ji z veřejné metody privátní metodu, protože veřejné metody v zařízení můžou mít přístup akce kontroleru. Za druhé upravíme volání, která kontroluje hostitel adresy URL vůči hostitele aplikací. Že volání využívá místní kontext požadavku v třída UrlHelper. Namísto použití této funkce. RequestContext.HttpContext.Request.Url.Host, budeme používat toto. Request.Url.Host. Následující kód ukazuje upravená metoda IsLocalUrl() pro použití s třídou kontroler ASP.NET MVC 1,0 a 2 aplikacemi.

**Výpis 5 – IsLocalUrl() metoda, která je upravit pro použití s třídu Kontroleru MVC**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

Teď, když metoda IsLocalUrl() je na místě, můžeme ji volat z našich akce přihlášení k ověření parametr returnUrl jak je znázorněno v následujícím kódu.

**Výpis 6 – aktualizované přihlašovací metodu, která ověřuje parametr returnUrl**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

Teď můžeme otestovat útok otevřené přesměrování pokusem o přihlášení pomocí externí návratová adresa URL. Použijeme /Account/LogOn? ReturnUrl =<http://www.bing.com/> znovu.

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

**Obrázek 04**: testování aktualizované akce přihlášení

Po úspěšném přihlášení, jsme se přesměrují na akce Kontroleru Home/Index spíše než externí adresu URL.

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

**Obrázek 05**: otevřené přesměrování útoku potlačována

## <a name="summary"></a>Souhrn

Útoky otevřené přesměrování může dojít při přesměrování adresy URL jsou předány jako parametry v adrese URL pro aplikaci. Otevřete ASP.NET MVC 3, šablona obsahuje kód pro ochranu před útoky přesměrování. Můžete přidat tento kód s určitými úpravami a 2 aplikace ASP.NET MVC 1,0. K ochraně před útoky na otevřeném přesměrování při přihlašování do 1.0 technologie ASP.NET a 2 aplikací, přidejte metodu IsLocalUrl() a ověřte parametr returnUrl v akci přihlášení.
