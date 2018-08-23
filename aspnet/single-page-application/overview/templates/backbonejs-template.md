---
uid: single-page-application/overview/templates/backbonejs-template
title: Šablona Backbone | Dokumentace Microsoftu
author: madskristensen
description: Šablona Backbone.js SPA
ms.author: riande
ms.date: 04/04/2013
ms.assetid: 00aca413-f067-4108-9bd1-cf21e64a2646
msc.legacyurl: /single-page-application/overview/templates/backbonejs-template
msc.type: authoredcontent
ms.openlocfilehash: 325c4f5370340b2e223521fada77cf0e78a67b5b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756400"
---
<a name="backbone-template"></a>Šablona Backbone
====================
podle [Autor: Mads Kristensen](https://github.com/madskristensen)

> Šablona Backbone SPA zapsal Kazi Manzur Rashid
> 
> [Stáhněte si šablonu Backbone.js SPA](https://go.microsoft.com/fwlink/?LinkId=293631)


Šablona Backbone.js SPA je navržena tak, jak začít rychle vytvářet interaktivní webové klientské aplikace s využitím [Backbone.js.](http://backbonejs.org/)

Šablona nabízí počáteční skeleton pro vývoj aplikací Backbone.js v architektuře ASP.NET MVC. Ihned poskytuje funkce základní uživatel přihlášení, včetně resetování hesla registrace, přihlášení, uživatele a potvrzení uživatele s základní e-mailové šablony.

Požadavky:

- [Aktualizace technologie ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)

## <a name="create-a-backbone-template-project"></a>Vytvoření projektu páteřní šablony

Stáhněte a nainstalujte šablony kliknutím na tlačítko Stáhnout výše. Šablona je zabalena jako soubor rozšíření aplikace Visual Studio (VSIX). Můžete potřebovat restartovat Visual Studio.

V **šablony** vyberte **nainstalované šablony** a rozbalte **Visual C#** uzlu. V části **Visual C#** vyberte **webové**. V seznamu šablon projektu vyberte **webové aplikace ASP.NET MVC 4**. Pojmenujte projekt a klikněte na tlačítko **OK**.

![](backbonejs-template/_static/image1.png)

V **nový projekt** průvodce, vyberte projekt SPA Backbone.js.

![](backbonejs-template/_static/image2.png)

Stisknutím kláves Ctrl-F5 sestavte a spusťte aplikaci bez ladění nebo stisknutím klávesy F5 spusťte ladění.

![](backbonejs-template/_static/image3.png)

Kliknutím na "Můj účet" zobrazí přihlašovací stránku:

![](backbonejs-template/_static/image4.png)

## <a name="walkthrough-client-code"></a>Návod: Klientský kód

Pojďme začíná na straně klienta. Klientské aplikace skripty jsou umístěny ve složce ~/Scripts/application. Aplikace je napsána v [TypeScript](http://www.typescriptlang.org/) (soubory .ts) které jsou kompilovány do jazyka JavaScript (.js soubory).

**Aplikace**

`Application` je definováno v application.ts. Tento objekt inicializuje aplikaci a funguje jako kořenový obor názvů. Udržuje informace o konfiguraci a stavu, který je sdílen mezi aplikace, jako například, jestli je uživatel přihlášený.

`application.start` Metoda vytvoří modální zobrazení a připojí obslužné rutiny událostí pro události na úrovni aplikace, jako je například přihlášení uživatele. Dále vytvoří výchozí směrovač a zkontroluje, zda je zadán libovolnou adresu URL na straně klienta. Pokud ne, přesměruje na výchozí adresu url (#! /).

**Události**

Události jsou důležité vždy při vývoji volně vázanými komponentami. Aplikace často provádět více operací v reakci na akce uživatele. Páteřní poskytuje integrované události s komponentami, jako je Model, kolekci a zobrazení. Místo vytváření mezi závislosti mezi komponentami, šablony používá model "pub/sub": `events` objekt definovaný v events.ts, funguje jako Centrum událostí pro publikování a přihlášení k odběru událostí aplikace. `events` Objekt je typu singleton. Následující kód ukazuje, jak přihlásit odběr události a poté spuštění události:

[!code-csharp[Main](backbonejs-template/samples/sample1.cs)]

**Směrovače**

Směrovač v Backbone.js, poskytuje metody pro směrování stránky na straně klienta a připojte je ke akcích a událostech. Šablona definuje jeden směrovač v router.ts. Směrovač activable zobrazení vytvoří a udržuje stav při přepnutí zobrazení. (Activable zobrazení jsou popsané v další části). Na začátku projektu má dvě fiktivní zobrazení domácích a o. Má také NotFound zobrazení, které se zobrazí, pokud je trasa není znám.

**Zobrazení**

Zobrazení jsou definovány v ~/Scripts/application a zobrazení. Existují dva druhy, activable zobrazení a zobrazení modálního dialogového okna. Activable zobrazení jsou vyvolány směrovačem. Když activable zobrazení se zobrazí, všech ostatních zobrazeních activable neaktivní. Vytvořit activable zobrazení, rozšíření zobrazení s `Activable` objektu:

[!code-javascript[Main](backbonejs-template/samples/sample2.js)]

Rozšíření s `Activable` přidá dva nové metody pro zobrazení, `activate` a `deactivate`. Směrovač voláním metod aktivace a deactive zobrazení.

Modální zobrazení jsou implementovány jako [architekturu Twitter Bootstrap](http://twitter.github.com/bootstrap/) modální dialogová okna. `Membership` a `Profile` zobrazení jsou modální zobrazení. Zobrazení modelu, jde vyvolat události žádné aplikace. Například v `Navigation` zobrazení, kliknutím na odkaz "Můj účet" obsahuje buď `Membership` zobrazení nebo `Profile` zobrazení, v závislosti na tom, jestli je uživatel přihlášen. `Navigation` Bude k obrazci, klikněte na tlačítko obslužné rutiny událostí pro všechny podřízené prvky, které mají `data-command` atribut. Tady je značka jazyka HTML:

[!code-html[Main](backbonejs-template/samples/sample3.html)]

Zde je kód v navigation.ts k připojení události:

[!code-csharp[Main](backbonejs-template/samples/sample4.cs)]

**Modely**

Tyto modely jsou definovány v ~/Scripts/application/modely. Všechny modely mají tři základní akce: výchozí atributy, ověřovací pravidla a koncový bod na straně serveru. Tady je typickým příkladem:

[!code-javascript[Main](backbonejs-template/samples/sample5.js)]

**Moduly plug-in**

Složka ~/Scripts/application/lib obsahuje několik modulů plug-in jQuery po ruce. Definuje form.ts souboru modulu plug-in pro práci s daty formuláře. Často je potřeba serializovat nebo deserializovat data formuláře a zobrazit všechny chyby ověření modelu. Modul plug-in form.ts, jako má metody `serializeFields`, `deserializeFields`, a `showFieldErrors`. Následující příklad ukazuje, jak k serializaci formuláře k modelu.

[!code-javascript[Main](backbonejs-template/samples/sample6.js)]

Modul plug-in flashbar.ts poskytuje různé typy zpráv se zpětnou vazbou pro uživatele. Metody jsou `$.showSuccessbar`, `$.showErrorbar` a `$.showInfobar`. Na pozadí používá architekturu Twitter Bootstrap výstrahy zobrazíte hezky animovaných zprávy.

Modul plug-in confirm.ts nahradí prohlížeče dialogové okno, potvrzení, i když se rozhraní API se poněkud liší:

[!code-javascript[Main](backbonejs-template/samples/sample7.js)]

## <a name="walkthrough-server-code"></a>Návod: Kód serveru

Nyní Pojďme se podívat na straně serveru.

**Kontrolery**

V jednostránkové aplikaci hraje na server pouze malé roli v uživatelském rozhraní. Obvykle serveru vykreslí stránku pro počáteční a pak odesílá i přijímá JSON data.

Šablona má dva řadiče MVC: `HomeController` vykreslí stránku pro počáteční, a `SupportsController` se používá k potvrzení nové uživatelské účty a resetování hesel. Všechny řadiče v šabloně jsou řadiče rozhraní ASP.NET Web API, které odesílat a přijímat JSON data. Ve výchozím nastavení, pomocí nové řadiče `WebSecurity` pro provádění úlohy související s uživatelem. Však mají také volitelné konstruktory, které umožňují předat delegáty pro tyto úlohy. Tím je testování jednodušší a umožňuje nahradit `WebSecurity` s něco jiného, pomocí kontejner IoC. Tady je příklad:

[!code-csharp[Main](backbonejs-template/samples/sample8.cs)]

## <a name="views"></a>Zobrazení

Zobrazení jsou navržené tak, aby modulární: každý oddíl na stránce má svůj vlastní vyhrazený zobrazení. V případě jednostránkové aplikace je běžné zahrnují zobrazení, které nemají žádné odpovídající kontroler. Zobrazení může obsahovat voláním `@Html.Partial('myView')`, ale to získá únavné. Abychom to usnadnili, Šablona definuje Pomocná metoda `IncludeClientViews`, který vykreslí všechna zobrazení v zadané složce:

[!code-cshtml[Main](backbonejs-template/samples/sample9.cshtml)]

Pokud není zadán název složky, je výchozí název složky "ClientViews". Pokud klienta zobrazí také používá částečná zobrazení, název částečného zobrazení se znakem podtržítka (například `_SignUp`). `IncludeClientViews` Metoda vylučuje všechna zobrazení, jejichž název začíná podtržítkem. Chcete-li zahrnout do zobrazení klienta částečné zobrazení, zavolejte `Html.ClientView('SignUp')` místo `Html.Partial('_SignUp')`.

**Odesílání e-mailu**

K odesílání e-mailová šablona používá [poštovní](http://aboutcode.net/postal). Nicméně je abstrahovaný poštovní od zbývající části kódu pomocí `IMailer` rozhraní, takže můžete snadno nahradit ho záznamem jinou implementaci. E-mailové šablony jsou umístěny ve složce zobrazení/e-mailů. E-mailová adresa odesílatele je zadán v souboru web.config v `sender.email` klíč **appSettings** oddílu. I když `debug="true"` v souboru web.config, aplikace nevyžaduje, aby uživatel e-mailové potvrzení pro urychlení vývoje.

## <a name="github"></a>GitHub

Můžete také najít šablonu Backbone.js SPA na [Githubu](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa).
