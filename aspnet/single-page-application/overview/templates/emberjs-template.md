---
uid: single-page-application/overview/templates/emberjs-template
title: "Šablona EmberJS | Microsoft Docs"
author: xqiu
description: "EmberJS šablony"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1fb7633aee288be648d4f9681b43c8911b7dbab9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="emberjs-template"></a>EmberJS šablony
====================
podle [Xinyang Qiu](https://github.com/xqiu)

> Šablony MVC EmberJS se zapíše Nathan Totten, Thiago Santos a Xinyang Qiu.
> 
> [Stažení šablony EmberJS MVC](https://go.microsoft.com/fwlink/?LinkId=282647)


Šablona EmberJS SPA je navržená tak, aby začít rychle vytvářet interaktivní webové klientské aplikace pomocí EmberJS.

"Jednostránkové aplikace" (SPA) je obecný termín pro webovou aplikaci, která načte jediné stránce HTML a pak aktualizuje dynamicky, místo načítání stránky, nové stránky. Po načtení úvodní stránky SPA komunikuje se serverem prostřednictvím požadavky AJAX.

![](emberjs-template/_static/image1.png)

AJAX není nic nového, ale existují ještě dnes JavaScript rozhraní, které usnadňují tvorbu a udržování rozsáhlé sofistikované SPA aplikace. Navíc standardu HTML 5 a CSS3 jsou usnadnit vytvoření bohaté uživatelská rozhraní.

Šablona SPA EmberJS používá [členskými](http://emberjs.com/) knihovna JavaScript pro zpracování aktualizace stránky z požadavky AJAX. Ember.js používá datové vazby k synchronizaci stránky s nejnovější data. Tímto způsobem, nemusíte psát kód, který provede JSON data a aktualizuje modelu DOM. Místo toho uveďte deklarativní atributy ve formátu HTML, který informování Ember.js jak data k dispozici.

Na straně serveru, je téměř shodné s šabloně EmberJS [kódem KnockoutJS SPA šablony](../introduction/knockoutjs-template.md). ASP.NET MVC používá k obsluze dokumentů HTML a ASP.NET Web API pro zpracování žádostí AJAX z klienta. Další informace o těchto aspektů šablony najdete v části [kódem KnockoutJS šablony](../introduction/knockoutjs-template.md) dokumentaci. Toto téma se zaměřuje na rozdíly mezi šablony Knockout a EmberJS šablony.

## <a name="create-an-emberjs-spa-template-project"></a>Vytvoření projektu EmberJS SPA šablony

Stáhněte a nainstalujte šablony kliknutím na výše uvedené tlačítko Stáhnout. Možná budete muset restartovat Visual Studio.

V **šablony** podokně, vyberte **nainstalovaných šablonách** a rozbalte **Visual C#** uzlu. V části **Visual C#**, vyberte **webové**. V seznamu šablon projektu, vyberte **webové aplikace ASP.NET MVC 4**. Název projektu a klikněte na tlačítko **OK**.

![](emberjs-template/_static/image2.png)

V **nový projekt** průvodci vyberte **Ember.js SPA projektu**.

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a>Přehled šablon SPA EmberJS

Šablona EmberJS používá kombinaci jQuery, Ember.js, Handlebars.js vytvořit smooth, interaktivní uživatelské rozhraní.

Ember.js je knihovna JavaScript, která používá vzor MVC na straně klienta.

- A *šablony*, napsané v jazyce ukázka Řídítka kola, popisuje uživatelské rozhraní aplikace. V režimu vydání [Řídítka kola kompilátoru](https://github.com/Myslik/csharp-ember-handlebars) se používá k sady a kompilaci Řídítka kola šablony.
- A *modelu* ukládá data aplikací, který získá ze serveru (ToDo seznamy a položky ToDo).
- A *řadič* uloží stav aplikace. Řadiče často prezentují data modelu odpovídající šablon.
- A *zobrazení* překládá primitivní události z aplikace a předá tyto řadiče.
- A *směrovač* spravuje stav aplikace, udržování synchronizace adresy URL a šablony.

Kromě toho knihovny členskými dat slouží k synchronizaci objektů JSON (získaný ze serveru prostřednictvím rozhraní RESTful API) a modely klienta.

Šablona EmberJS SPA uspořádává skripty do osm vrstvy:

- webapi\_adapter.js, webapi\_serializer.js: rozšíření knihovny členskými dat pro práci s rozhraním ASP.NET Web API.
- Scripts/Helpers.js: Definuje nový Řídítka členskými kola pomocné rutiny.
- Scripts/App.js: Vytvoří aplikaci a nakonfiguruje adaptéru a serializátor.
- Aplikace, skripty nebomodely/\*.js: definuje modely.
- Skripty nebo aplikace nebozobrazení/\*.js: definuje zobrazení.
- Skripty/aplikace/řadiče nebo\*.js: definuje do řadičů.
- Skripty a aplikace/směruje, Scripts/app/router.js: Definuje trasy.
- Šablony /\*.hbs: definuje Řídítka kola šablony.

Podívejme se na některé z těchto skriptů podrobněji.

## <a name="models"></a>Modely

Modely jsou definovány ve složce aplikace, skripty nebo modely. Existují dva soubory modelu: todoItem.js a todoList.js.

**TODO.model.js** definuje modely na straně klienta (prohlížeč) pro seznamu úkolů. Existují dvě třídy modelu: todoItem a seznamu úkolů. V členskými jsou modely měly podtřídy DS. Model. Model může mít vlastnosti s atributy:

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

Modely můžete definovat vztahy s jinými modely:

[!code-css[Main](emberjs-template/samples/sample2.css)]

Modely můžete počítaný vlastnosti, které vytvořit vazbu na ostatní vlastnosti:

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

Modely může mít pozorovatel funkce, které jsou vyvolány, když se změní zjištěnou vlastnost:

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a>zobrazení

Zobrazení jsou definovány ve složce skripty nebo aplikace nebo zobrazení. Zobrazení překládá události z aplikace uživatelského rozhraní. Obslužné rutiny události můžete zpětné volání pro funkce řadiče nebo jednoduše kontextu dat volat přímo.

Například následující kód je z views/TodoItemEditView.js. Definuje, událostí zpracování pro vstupní textové pole.

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a>Řadiče

Ve složce skripty/aplikace/řadiče jsou definovány v řadičích. Představují jeden model, rozšířit `Ember.ObjectController`:

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

Řadič může také představovat kolekce modelů rozšířením `Ember.ArrayController`. Například TodoListController představuje pole `todoList` objekty. Řadičem seřadí podle ID seznamu úkolů v sestupném pořadí:

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

Řadičem definuje funkci s názvem `addTodoList`, který vytvoří nový seznam úkolů a přidá ji do pole. Informace o tom, jak tuto funkci volala, otevřete soubor šablony s názvem todoListTemplate.html ve složce šablony. Následující kód šablony váže tlačítko `addTodoList` funkce:

[!code-html[Main](emberjs-template/samples/sample8.html)]

Také obsahuje kontroler `error` vlastnosti, která obsahuje chybovou zprávu. Tady je kód šablony zobrazíte chybovou zprávu (také ve todoListTemplate.html):

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a>Trasy

Router.js definuje trasy a výchozí šablony, které chcete zobrazit, nastaví stav aplikací a odpovídá adresy URL trasy:

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

TodoListRoute.js načte data pro TodoListRoute přepsáním setupController funkce:

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

Členskými používá zásady vytváření názvů k přiřazení adres URL, názvy tras, řadiče a šablony. Další informace najdete v tématu [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) v dokumentaci EmberJS.

## <a name="templates"></a>Šablony

Složka šablon obsahuje čtyři šablony:

- Application.HBs: výchozí šablonu, která se zobrazí při spuštění aplikace.
- About.HBs: Šablona pro tuto trasu "/ o".
- index.HBs: šablony pro kořenovou "/" trasy.
- todoList.hbs: šablonu pro "/ todo" trasy.
- \_navbar.HBs: Šablona definuje navigační nabídce.

Šablona aplikací funguje jako hlavní stránky. Obsahuje záhlaví, zápatí a "{{výstupu}}" Chcete-li vložit další šablony v v závislosti na trasy. Další informace o šablonách aplikace v členskými najdete v tématu [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).

"/ TodoList" Šablona obsahuje dvou výrazů smyčky. Mimo smyčka je `{{#each controller}}`a uvnitř smyčka je `{{#each todos}}`. Následující kód ukazuje integrované `Ember.Checkbox` zobrazit, přizpůsobený `App.TodoItemEditView`a odkaz s `deleteTodo` akce.

[!code-html[Main](emberjs-template/samples/sample12.html)]

`HtmlHelperExtensions` Třídy, které jsou definované v Controllers/HtmlHelperExensions.cs, definuje pomocné rutiny funkce ukládat do mezipaměti a vložte šablonu souborů při **ladění** je nastaven na **true** v souboru Web.config. Tato funkce je volána ze souboru zobrazení ASP.NET MVC definované v Views/Home/App.cshtml:

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

Voláno bez argumentů, vykreslí funkce všechny šablony soubory ve složce šablony. Můžete také zadat určitá šablona souboru nebo podsložku.

Když **ladění** je **false** v souboru Web.config, aplikace obsahuje položku sady "~/bundles/templates". Tuto položku sady je přidaný do BundleConfig.cs pomocí knihovny kompilátoru Řídítka kola:

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
