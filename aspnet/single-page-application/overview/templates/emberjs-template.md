---
uid: single-page-application/overview/templates/emberjs-template
title: Šablona emberjs | Dokumentace Microsoftu
author: xqiu
description: Šablona emberjs
ms.author: riande
ms.date: 01/30/2013
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: fbc3b1d299ace27d38d895e42b8e3bb3b51b36f8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755010"
---
<a name="emberjs-template"></a>Šablona emberjs
====================
podle [Xinyang Qiu](https://github.com/xqiu)

> Šablona Emberjs MVC je vytvořená systémem Nathan Totten Thiago Santos a Xinyang Qiu.
> 
> [Stažení šablony EmberJS MVC](https://go.microsoft.com/fwlink/?LinkId=282647)


Šablona EmberJS SPA je určena vám pomůžou začít rychle vytvářet interaktivní webové klientské aplikace s využitím EmberJS.

"Jednostránková aplikace" (SPA) je obecný termín pro webovou aplikaci, která načte jednu stránku HTML a následně aktualizuje dynamicky, místo načtení nové stránky na stránku. Po načtení počáteční stránky tato jednostránková aplikace komunikuje se serverem přes odesílání požadavků AJAX.

![](emberjs-template/_static/image1.png)

AJAX není nic nového, ale ještě dnes existují architektury JavaScriptu, které usnadňují tvorbu a udržování rozsáhlé sofistikované aplikace SPA. Navíc HTML 5 a CSS3 jsou to usnadňuje vytváření bohatých uživatelského rozhraní.

Používá šablona Emberjs SPA [členskými](http://emberjs.com/) JavaScript library pro zpracování aktualizace stránky AJAX požadavků. Ember.js používá datové vazby k synchronizaci na stránce s nejnovější data. Tímto způsobem nemusíte psát žádný kód, který vás provede JSON data a aktualizace modelu DOM. Místo toho kam si ukládáte deklarativních atributů ve formátu HTML, který Ember.js instrukce, jak data můžete prezentovat tak.

Na straně serveru, je téměř stejný jako šablona emberjs [šablona KnockoutJS SPA](../introduction/knockoutjs-template.md). ASP.NET MVC používá k poskytování dokumentů HTML a ASP.NET Web API pro zpracování požadavků AJAX od klienta. Další informace o tyto aspekty šablony [šablona KnockoutJS](../introduction/knockoutjs-template.md) dokumentaci. Toto téma se zaměřuje na rozdíly mezi šablony Knockout a šablona emberjs.

## <a name="create-an-emberjs-spa-template-project"></a>Vytvoření projektu šablona EmberJS SPA

Stáhněte a nainstalujte šablony kliknutím na tlačítko Stáhnout výše. Můžete potřebovat restartovat Visual Studio.

V **šablony** vyberte **nainstalované šablony** a rozbalte **Visual C#** uzlu. V části **Visual C#** vyberte **webové**. V seznamu šablon projektu vyberte **webové aplikace ASP.NET MVC 4**. Pojmenujte projekt a klikněte na tlačítko **OK**.

![](emberjs-template/_static/image2.png)

V **nový projekt** průvodce, vyberte **Ember.js SPA projektu**.

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a>Přehled šablon SPA EmberJS

Šablona emberjs používá kombinaci jQuery, Ember.js, Handlebars.js vytvořit plynulé a na interaktivní uživatelské rozhraní.

Ember.js je knihovna jazyka JavaScript, která používá vzor MVC na straně klienta.

- A *šablony*napsaných v jazyce šablonování Handlebars, popisuje uživatelské rozhraní aplikace. V režimu vydání [Handlebars kompilátoru](https://github.com/Myslik/csharp-ember-handlebars) slouží k vytvoření balíčku a kompilaci handlebars šablony.
- A *modelu* ukládá data aplikací, který získá ze serveru (ToDo seznamy a položky seznamu úkolů).
- A *řadič* uloží stav aplikace. Kontrolery často prezentovat data modelu na odpovídající šablony.
- A *zobrazení* přeloží primitivní události z aplikace a předává do kontroleru.
- A *směrovače* spravuje stav aplikace, udržování synchronizace šablony a adresy URL.

Kromě toho knihovna členskými dat je možné synchronizovat objekty JSON (získané ze serveru prostřednictvím rozhraní RESTful API) a modely klienta.

Šablona EmberJS SPA slouží k uspořádání skripty do osm vrstvy:

- webapi\_adapter.js, webapi\_serializer.js: rozšiřuje knihovna členskými dat pro práci s rozhraním ASP.NET Web API.
- Scripts/Helpers.js: Definuje novou členskými Handlebars pomocné rutiny.
- Scripts/App.js: Vytvoří aplikaci a nakonfiguruje adaptéru a serializátor.
- Skripty/aplikace/modely/\*js: definuje modely.
- Skripty aaplikace/zobrazení/\*js: definuje zobrazení.
- Skripty/aplikace/řadiče/\*js: definuje řadiče.
- Skripty/aplikace/trasy, Scripts/app/router.js: Definuje trasy.
- Šablony /\*.hbs: definuje handlebars šablony.

Pojďme se podívat na některé z těchto skriptů podrobněji.

## <a name="models"></a>Modely

Tyto modely jsou definovány ve složce skripty/aplikace/modely. Existují dva soubory modelu: todoItem.js a todoList.js.

**TODO.model.js** definuje modely (prohlížeč) na straně klienta pro seznamy úkolů. Existují dvě třídy modelu: todoItem a seznamu úkolů. V členskými modely jsou podtřídami DS. Model. Model může mít vlastnosti s atributy:

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

Modely lze definovat vztahy s jinými modely:

[!code-css[Main](emberjs-template/samples/sample2.css)]

Modely můžou mít vypočítané vlastnosti, kteří jsou navázáni na další vlastnosti:

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

Modely mohou být pozorovatel funkce, které jsou vyvolány při změně zjištěné vlastnosti:

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a>Zobrazení

Zobrazení jsou definovány ve složce skripty/aplikace/zobrazení. Zobrazení přeloží události z uživatelského rozhraní aplikace. Obslužné rutiny události můžete zpětné volání pro kontroler funkce nebo jednoduše datového kontextu volat přímo.

Například následující kód je z views/TodoItemEditView.js. Definuje události u vstupního textového pole.

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a>Kontroler

Kontrolery jsou definovány ve složce skripty/aplikace/řadiče. K reprezentaci jednoho modelu rozšíření `Ember.ObjectController`:

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

Kontroler může také představovat kolekce modelů rozšířením `Ember.ArrayController`. Například, TodoListController reprezentuje pole prvků `todoList` objekty. Kontroler se seřadí podle ID seznamu úkolů v sestupném pořadí:

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

Kontroler definuje funkci nazvanou `addTodoList`, který vytvoří nový seznam úkolů a přidá jej do pole. Pokud chcete zobrazit, jak tato funkce volána, otevřete soubor šablony s názvem todoListTemplate.html do složky šablony. Následující kód šablony vytvoří vazbu na tlačítko `addTodoList` funkce:

[!code-html[Main](emberjs-template/samples/sample8.html)]

Také obsahuje kontroler `error` vlastnost, která obsahuje chybovou zprávu. Tady je kód šablony zobrazíte chybovou zprávu (také v todoListTemplate.html):

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a>Trasy

Router.js trasy a výchozí šablonu pro zobrazení, nastaví stav aplikace, definuje a odpovídá adresy URL trasy:

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

TodoListRoute.js načte data pro TodoListRoute tak, že přepíšete setupController funkce:

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

Členskými používá konvence pojmenování k přiřazení adres URL, názvy tras, kontrolerů a šablony. Další informace najdete v tématu [ http://emberjs.com/guides/routing/defining-your-routes/ ](http://emberjs.com/guides/routing/defining-your-routes/) v dokumentaci EmberJS.

## <a name="templates"></a>Šablony

Složka šablony obsahuje čtyři šablony:

- Application.HBs: výchozí šablony, který je vykreslen při spuštění aplikace.
- About.HBs: Šablona pro tuto trasu "/ o".
- index.HBs: šablony pro kořenovou trasy "/".
- todoList.hbs: Šablona pro "/ todo" trasy.
- \_navbar.HBs: Šablona definuje navigační nabídky.

Šablona aplikace funguje jako hlavní stránky. Obsahuje záhlaví, zápatí nebo "{{zásuvky}}" Vložit další šablony v v závislosti na trasy. Další informace o šablonách aplikací v členskými najdete v tématu [ http://guides.emberjs.com/v1.10.0/templates/the-application-template// ](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).

"/ Seznam úkolů" Šablona obsahuje dvou výrazů smyčky. Vnější smyčka je `{{#each controller}}`a uvnitř smyčka je `{{#each todos}}`. Následující kód ukazuje vestavěná `Ember.Checkbox` zobrazíte přizpůsobený `App.TodoItemEditView`a odkaz s `deleteTodo` akce.

[!code-html[Main](emberjs-template/samples/sample12.html)]

`HtmlHelperExtensions` Třídy definované v Controllers/HtmlHelperExensions.cs, definuje pomocnou rutinu funkce do mezipaměti a Vložit šablonu souborů při **ladění** je nastavena na **true** v souboru Web.config. Tato funkce je volána z definované v Views/Home/App.cshtml soubor zobrazení ASP.NET MVC:

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

Volat bez argumentů, vykreslí funkci všechny soubory šablon do složky šablony. Můžete také zadat podsložku nebo soubor konkrétní šablony.

Když **ladění** je **false** v souboru Web.config, aplikace obsahuje položku sady "~/bundles/templates". Tato položka sady přidá BundleConfig.cs pomocí knihovny kompilátoru Handlebars:

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
