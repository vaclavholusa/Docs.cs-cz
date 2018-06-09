---
uid: single-page-application/overview/templates/backbonejs-template
title: Páteřní šablony | Microsoft Docs
author: madskristensen
description: Backbone.js SPA šablony
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/04/2013
ms.topic: article
ms.assetid: 00aca413-f067-4108-9bd1-cf21e64a2646
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/backbonejs-template
msc.type: authoredcontent
ms.openlocfilehash: 3b8eabd3cefcb96dc40bbf6cc6e3ee81accb0d7c
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "26566308"
---
<a name="backbone-template"></a>Páteřní šablony
====================
podle [Mads Kristensen](https://github.com/madskristensen)

> Šablona SPA páteřní napsal Kazi Manzur Rashid
> 
> [Stažení šablony SPA Backbone.js](https://go.microsoft.com/fwlink/?LinkId=293631)


Šabloně Backbone.js SPA navržená tak, abyste mohli začít rychle vytvářet interaktivní webové klientské aplikace pomocí [Backbone.js.](http://backbonejs.org/)

Šablona poskytuje počáteční kostru pro vývoj aplikace Backbone.js v architektuře ASP.NET MVC. Ihned poskytuje funkce základní uživatel přihlášení, včetně resetování hesla registrace, přihlášení, uživatele a potvrzení uživatele s základní e-mailových šablon.

Požadavky:

- [Technologie ASP.NET a webové nástroje 2012.2 aktualizace](https://go.microsoft.com/fwlink/?LinkId=282650)

## <a name="create-a-backbone-template-project"></a>Vytvoření projektu šablony páteřní

Stáhněte a nainstalujte šablony kliknutím na výše uvedené tlačítko Stáhnout. Šablona je zabalené jako soubor rozšíření Visual Studio (VSIX). Možná budete muset restartovat Visual Studio.

V **šablony** podokně, vyberte **nainstalovaných šablonách** a rozbalte **Visual C#** uzlu. V části **Visual C#**, vyberte **webové**. V seznamu šablon projektu, vyberte **webové aplikace ASP.NET MVC 4**. Název projektu a klikněte na tlačítko **OK**.

![](backbonejs-template/_static/image1.png)

V **nový projekt** průvodce, vyberte Backbone.js SPA projektu.

![](backbonejs-template/_static/image2.png)

Stisknutím Ctrl + F5 sestavení a spuštění aplikace bez ladění nebo stisknutím klávesy F5 spusťte s laděním.

![](backbonejs-template/_static/image3.png)

Kliknutím na tlačítko "Můj účet" zobrazíte stránku pro přihlášení:

![](backbonejs-template/_static/image4.png)

## <a name="walkthrough-client-code"></a>Návod: Kód klienta

Pojďme začíná na straně klienta. Skripty aplikace klienta jsou umístěny ve složce ~/Scripts/application. Aplikace je napsána v [TypeScript](http://www.typescriptlang.org/) (.ts soubory) které jsou zkompilovány do jazyka JavaScript (.js soubory).

**Aplikace**

`Application` je definována v application.ts. Tento objekt inicializuje aplikaci a funguje jako kořenového oboru názvů. Uchovává informace o konfiguraci a stavu, který je sdílen mezi aplikace, například zda je přihlášený uživatel.

`application.start` Metoda vytvoří modální zobrazení a připojí obslužné rutiny události pro události na úrovni aplikace, jako je například přihlášení uživatele. V dalším kroku vytvoří výchozí směrovač a kontroluje, zda je zadaná adresa URL žádné straně klienta. Pokud ne, je přesměrován na výchozí adresa url (#! /).

**Události**

Události jsou důležité vždy při vývoji volně doplněná součásti. Aplikace často provádět více operací v reakci na akci uživatele. Páteřní poskytuje integrované události s komponenty, například modelu, kolekce a zobrazení. Místo vytváření mezi závislosti mezi tyto součásti, šablona používá model "pub nebo sub": `events` objektu, která je definována v events.ts, funguje jako centra událostí pro publikování a přihlášení k odběru událostí aplikace. `events` Objekt je typu singleton. Následující kód ukazuje, jak chcete objednat předplatné událost a potom aktivovat události:

[!code-csharp[Main](backbonejs-template/samples/sample1.cs)]

**Směrovač**

Směrovač v Backbone.js, poskytuje metody pro směrování stránky na straně klienta a jejich připojením do akce a události. Šablona definuje jeden směrovač, router.ts. Směrovači activable zobrazení vytvoří a udržuje stav při přepnutí zobrazení. (Activable zobrazení jsou popsané v další části). Na začátku projekt má dvě fiktivní zobrazení, domácích a o. Je také zobrazení serveru, který se zobrazí, pokud není znám trasy.

**Zobrazení**

Zobrazení jsou definovány v ~/Scripts/application nebo zobrazení. Existují dva typy zobrazení, activable zobrazení a zobrazení modální dialogové okno. Activable zobrazení jsou vyvolány směrovači. Když se zobrazuje i activable zobrazení, všechna activable zobrazení neaktivní. Chcete-li vytvořit activable zobrazení, rozšířit zobrazení s `Activable` objektu:

[!code-javascript[Main](backbonejs-template/samples/sample2.js)]

Rozšíření s `Activable` přidá dva nové metody pro zobrazení, `activate` a `deactivate`. Směrovači volá tyto metody pro aktivaci a deactive zobrazení.

Modální zobrazení jsou implementované jako [Twitter Bootstrap](http://twitter.github.com/bootstrap/) modální dialogová okna. `Membership` a `Profile` zobrazení jsou modální zobrazení. Model zobrazení může vyvolat všech událostí aplikace. Například v `Navigation` zobrazení, kliknutím na odkaz "Můj účet" uvádí buď `Membership` zobrazení nebo `Profile` zobrazení, v závislosti na tom, jestli je uživatel přihlášen. `Navigation` Připojí obslužné rutiny události pro všechny podřízené elementy, které mají kliknutí `data-command` atribut. Zde je kód HTML:

[!code-html[Main](backbonejs-template/samples/sample3.html)]

Zde je kód v navigation.ts spojit události:

[!code-csharp[Main](backbonejs-template/samples/sample4.cs)]

**Modely**

Modely jsou definovány v ~/Scripts/application/modelů. Všechny modely mají tři základní věci: výchozí atributy, ověřovacích pravidel a koncový bod na straně serveru. Tady je typický příklad:

[!code-javascript[Main](backbonejs-template/samples/sample5.js)]

**Moduly plug-in**

Složka ~/Scripts/application/lib obsahuje několik užitečný jQuery moduly plug-in. Soubor form.ts definuje modulu plug-in pro práci s dat formuláře. Často potřebují k serializaci nebo deserializaci dat formuláře a zobrazit všechny chyby ověření modelu. Modul plug-in form.ts má metody jako `serializeFields`, `deserializeFields`, a `showFieldErrors`. Následující příklad ukazuje, jak k serializaci formuláře k modelu.

[!code-javascript[Main](backbonejs-template/samples/sample6.js)]

Modul plug-in flashbar.ts poskytuje různé druhy zprávy zpětnou vazbu pro uživatele. Metody `$.showSuccessbar`, `$.showErrorbar` a `$.showInfobar`. Na pozadí používá Twitter Bootstrap výstrahy zobrazíte vhodně animovaný zprávy.

Modul plug-in confirm.ts nahrazuje prohlížeče potvrďte dialog, i když se poněkud liší rozhraní API:

[!code-javascript[Main](backbonejs-template/samples/sample7.js)]

## <a name="walkthrough-server-code"></a>Návod: Serverový kód

Nyní Podíváme se na straně serveru.

**Kontrolery**

V aplikaci jednostránkové hraje serveru jenom malé roli v uživatelském rozhraní. Obvykle serveru vykreslí úvodní stránku a pak odesílá a přijímá JSON data.

Šablona má dva řadiče MVC: `HomeController` vykreslí úvodní stránky a `SupportsController` se používá k potvrzení nové uživatelské účty a resetovat hesla. Všechny řadiče v šabloně jsou řadiče rozhraní ASP.NET Web API, které odesílat a přijímat JSON data. Ve výchozím nastavení, aby řadiče pomocí nové `WebSecurity` třída úkoly související s uživatelem. Ale mají také volitelné konstruktory, které umožňují předat delegáty pro tyto úlohy. To je testování jednodušší a umožňuje vám nahradit `WebSecurity` s něco jiného, pomocí kontejner IoC. Tady je příklad:

[!code-csharp[Main](backbonejs-template/samples/sample8.cs)]

## <a name="views"></a>zobrazení

Zobrazení jsou navržené jako modulární: každý oddíl stránky, má svou vlastní vyhrazené zobrazení. V jednostránkové aplikace je běžné zahrnout zobrazení, které nemají žádné odpovídající kontroler. Zobrazení může zahrnovat voláním `@Html.Partial('myView')`, ale to získá zdlouhavé. Pro zjednodušení, že šablona definuje Pomocná metoda, `IncludeClientViews`, který vykreslí všechna zobrazení v zadané složce:

[!code-cshtml[Main](backbonejs-template/samples/sample9.cshtml)]

Pokud není zadán název složky, je výchozí název složky "ClientViews". Pokud klient zobrazení také používá částečné zobrazení, název částečného zobrazení znakem podtržítka (například `_SignUp`). `IncludeClientViews` Metoda vylučuje všechna zobrazení, jejichž název začíná podtržítkem. Chcete-li zahrnout do zobrazení klienta částečné zobrazení, volejte `Html.ClientView('SignUp')` místo `Html.Partial('_SignUp')`.

**Odesílání e-mailu**

K odeslání e-mailu, šablona používá [poštovní](http://aboutcode.net/postal). Ale je poštovní abstrahované od zbytku kódu pomocí `IMailer` rozhraní, takže ho můžete snadno nahradit s jinou implementaci. E-mailových šablon jsou umístěny ve složce zobrazení či E-maily. E-mailová adresa odesílatele je zadaný v souboru web.config v `sender.email` klíče z **appSettings** části. I když `debug="true"` aplikace v souboru web.config, nevyžaduje potvrzení e-mailu uživatele, pro urychlení vývoj.

## <a name="github"></a>GitHub

Můžete také získat šabloně Backbone.js SPA na [Githubu](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa).
