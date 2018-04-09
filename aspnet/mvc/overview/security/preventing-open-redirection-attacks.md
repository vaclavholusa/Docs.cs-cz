---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: Prevence útoků otevřete přesměrování (C#) | Microsoft Docs
author: jongalloway
description: Tento kurz popisuje, jak lze zabránit útokům otevřete přesměrování v aplikacích ASP.NET MVC. Tento kurz popisuje změny, které byly provedeny...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: ec1cd1791eb6d32e7c1ea50bc6626929cad2960e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="preventing-open-redirection-attacks-c"></a>Prevence útoků otevřete přesměrování (C#)
====================
podle [Jon Galloway](https://github.com/jongalloway)

> Tento kurz popisuje, jak lze zabránit útokům otevřete přesměrování v aplikacích ASP.NET MVC. Tento kurz popisuje změny, které byly provedeny v AccountController v architektuře ASP.NET MVC 3 a ukazuje, jak můžete tyto změny použít na vaše stávající technologie ASP.NET MVC 1.0 a 2 aplikace.


## <a name="what-is-an-open-redirection-attack"></a>Co je útok otevřete přesměrování?

Webové aplikace, který přesměruje na adresu URL, která je zadána prostřednictvím požadavku například řetězci dotazu nebo formuláře dat může potenciálně manipulováno k přesměrování uživatelů na externí, škodlivý URL. Tato manipulaci se nazývá útok otevřete přesměrování.

Vždy, když vaše aplikace logiky přesměruje na zadané adrese URL, je nutné ověřit, že adresu URL pro přesměrování nikdo neoprávněně nemanipuloval. Přihlášení, které se používá ve výchozím AccountController pro ASP.NET MVC 1.0 a ASP.NET MVC 2 je zranitelný vůči útokům přesměrování otevřete. Naštěstí je snadné k aktualizaci existující aplikace oprav z verze Preview ASP.NET MVC 3.

Zjistit chybu podíváme, jak funguje přesměrování přihlášení v projektu webové aplikace ASP.NET MVC 2 výchozí. V této aplikaci pokusu navštívit akce kontroleru, který má atribut [autorizovat] přesměruje neoprávnění uživatelé /Account/LogOn zobrazení. Tato přesměrování, které /Account/LogOn bude obsahovat parametr řetězce dotazu returnUrl tak, aby uživatel se může vracet na původně požadovanou URL adresu po jejich úspěšně přihlášeni.

Na tomto snímku obrazovky uvidíme, že pokus o přístup k zobrazení /Account/ChangePassword, když není přihlášen výsledkem přesměrování na /Account/LogOn? ReturnUrl = % 2fAccount % 2fChangePassword % 2f.

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

**Obrázek 01**: přihlašovací stránku s otevřete přesměrování

Vzhledem k tomu, že parametr řetězce dotazu ReturnUrl není ověřená, útočník ho upravit, abyste vložit libovolnou adresu URL do parametru k provedení útoku otevřete přesměrování. Ukazuje to jsme můžete změnit parametr ReturnUrl na [ http://bing.com ](http://bing.com), takže výsledný přihlašovací adresa URL bude/Account/přihlášení? ReturnUrl =<http://www.bing.com/>. Po úspěšně přihlašování k webu, jsme se přesměrují na [ http://bing.com ](http://bing.com). Vzhledem k tomu, že toto přesměrování není ověřen, může místo toho přejděte na škodlivé weby, které se pokouší přesvědčit uživatele, aby.

### <a name="a-more-complex-open-redirection-attack"></a>Složitější otevřete útoku přesměrování

Otevřete přesměrování útoky jsou obzvláště nebezpečné, protože útočník ví, že Pokoušíme se přihlaste se k určitému webu, který je zranitelný nám [útoky phishing](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx). Například útočník může odeslat škodlivý e-mailů webu uživatelům ve snaze zaznamenat jejich hesla. Podíváme, jak to bude fungovat na webu NerdDinner. (Všimněte si, že byl aktualizován živý web NerdDinner chránit před útoky otevřete přesměrování.)

Nejprve útočník odešle nám odkaz na přihlašovací stránku na NerdDinner, která zahrnuje přesměrování, které jejich padělané stránky:

[http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

Všimněte si, že návratová adresa URL odkazuje na nerddiner.com, které chybí "n" z večeři aplikace word. V tomto příkladu je to doméně, která určuje, že útočník. Když jsme získat přístup na výše uvedený odkaz, jsme se prováděné na oprávněné NerdDinner.com přihlašovací stránku.

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

**Obrázek 02**: NerdDinner přihlašovací stránku s otevřete přesměrování

Když jsme správně přihlásit, akce ASP.NET MVC AccountController přihlášení nám přesměruje na adresu URL zadanou v parametru řetězce dotazu returnUrl. V takovém případě je adresu URL, kterou má zadán útočník, který je [ http://nerddiner.com/Account/LogOn ](http://nerddiner.com/Account/LogOn). Pokud jsme velmi watchful, že je velmi pravděpodobné, že nebude, zjistíme zvlášť, protože útočník byl pečlivě zkontrolujte, zda jejich padělané stránka vypadá úplně stejně jako legitimní přihlašovací stránku. Tento přihlašovací stránku obsahuje, chyba zprávu požadavku, nemůžeme přihlásit znovu. Clumsy nás, jsme musí zadali heslo.

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

**Obrázek 03**: Forged NerdDinner přihlašovací obrazovku

Když jsme znovu naše uživatelské jméno a heslo, padělané přihlašovací stránky uloží informace a odešle nám zpět na web legitimní NerdDinner.com. V tomto okamžiku NerdDinner.com lokality již ověřen nám, takže přímo na této stránce můžete přesměrovat padělané přihlašovací stránku. Konečným výsledkem je, že útočník má naše uživatelské jméno a heslo a jsme neberou v úvahu, že jsme jste zadali na ně.

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a>Prohlížení kód citlivé na přihlášení akce AccountController

Kód pro přihlášení akce v aplikaci ASP.NET MVC 2 je uveden níže. Všimněte si, že po úspěšném přihlášení kontroleru vrátí přesměrování returnUrl. Uvidíte, že žádné ověření se provádí před returnUrl parametr.

**Výpis 1 – akce ASP.NET MVC 2 přihlášení v `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

Nyní Podíváme se na změny akce ASP.NET MVC 3 přihlášení. Tento kód byl změněn na ověření parametru returnUrl voláním nové metody v System.Web.Mvc.Url pomocná třída s názvem `IsLocalUrl()`.

**Výpis 2 – akce ASP.NET MVC 3 přihlášení v `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

To se změnil na ověření parametr návratové adresy URL voláním nové metody v System.Web.Mvc.Url pomocná třída, `IsLocalUrl()`.

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a>Ochrana rozhraní ASP.NET MVC 1,0 a MVC 2 aplikace

Přidáním pomocnou metodu IsLocalUrl() a aktualizaci akce přihlášení k ověření parametru returnUrl jsme můžete využít výhod ASP.NET MVC 3 změny v našem existující ASP.NET MVC 1.0 a 2 aplikace.

Metoda UrlHelper IsLocalUrl() ve skutečnosti jenom volání do metody v System.Web.WebPages jako toto ověření používá i rozhraní ASP.NET Web Pages.

**Výpis 3 – metoda IsLocalUrl() z ASP.NET MVC 3 UrlHelper `class`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

Metoda IsUrlLocalToHost obsahuje logiku samotné ověření, jak je znázorněno v výpis 4.

**Výpis 4 – metoda IsUrlLocalToHost() z System.Web.WebPages RequestExtensions – třída**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

V našich ASP.NET MVC 1.0 nebo 2 aplikace přidáme IsLocalUrl() metoda do AccountController, ale jste doporučujeme, aby ho přidat do samostatné pomocná třída Pokud je to možné. Jsme tak, že bude pracovat uvnitř AccountController budou dva malé změny ve verzi ASP.NET MVC 3 IsLocalUrl(). Nejprve Změníme jeho z veřejné metody privátní metodu, vzhledem k tomu, že jsou přístupné veřejné metody v řadiče jako akce kontroleru. Za druhé upravíme volání, která kontroluje hostitele adresy URL pro hostitele aplikací. Aby se volání využívá místní kontext požadavku pole Třída UrlHelper. Nepoužívejte. RequestContext.HttpContext.Request.Url.Host, použijeme to. Request.Url.Host. Následující kód ukazuje změny metoda IsLocalUrl() pro použití s třídou řadiče v ASP.NET MVC 1.0 a 2 aplikace.

**Výpis 5 – IsLocalUrl() metoda, která je upravit pro použití s třídu MVC jsou řadič MVC**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

Teď, když metoda IsLocalUrl() je na místě, jsme ji volat z našich akce přihlášení k ověření parametru returnUrl, jak je znázorněno v následujícím kódu.

**Výpis 6 – aktualizované přihlášení metodu, která ověří parametr returnUrl**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

Nyní jsme můžete otestovat útoku otevřete přesměrování probíhá pokus o přihlášení pomocí externí návratovou adresu URL. Umožňuje použít /Account/LogOn? ReturnUrl =<http://www.bing.com/> znovu.

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

**Obrázek 04**: testování aktualizované akce přihlášení

Po úspěšném přihlášení se jsme se přesměrují do akce Kontroleru domovské nebo indexu, nikoli na externí adresu URL.

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

**Obrázek 05**: Otevřete přesměrování útoku potlačována

## <a name="summary"></a>Souhrn

Otevřete přesměrování útoky může dojít, když přesměrování adresy URL jsou předány jako parametry v adrese URL pro aplikaci. Otevřete ASP.NET MVC 3, šablona obsahuje kód pro ochranu proti útokům přesměrování. Můžete přidat tento kód se některé změny ASP.NET MVC 1.0 a 2 aplikace. Chránit před útoky otevřete přesměrování při přihlašování do ASP.NET 1.0 a 2 aplikací, přidejte metodu IsLocalUrl() a ověření parametru returnUrl v akci pro přihlášení.
