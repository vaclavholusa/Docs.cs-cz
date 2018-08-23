---
uid: single-page-application/overview/introduction/knockoutjs-template
title: 'Jednostránková aplikace: Šablona KnockoutJS | Dokumentace Microsoftu'
author: MikeWasson
description: Šablona knockout
ms.author: riande
ms.date: 01/30/2013
ms.assetid: f9c07af0-4b20-4b08-af8f-47fc3df169a2
msc.legacyurl: /single-page-application/overview/introduction/knockoutjs-template
msc.type: authoredcontent
ms.openlocfilehash: 328046363666944f121dedc1883bbe83f5b079d2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756876"
---
<a name="single-page-application-knockoutjs-template"></a>Jednostránková aplikace: Šablona KnockoutJS
====================
podle [Mike Wasson](https://github.com/MikeWasson)

> Šablona MVC Knockout je součástí technologie ASP.NET and Web Tools 2012.2
> 
> [Stáhněte si ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)


ASP.NET and Web Tools 2012.2 obsahuje šablona jednostránkové aplikace (SPA) pro technologii ASP.NET MVC 4. Tato šablona slouží k vám pomůžou začít rychle vytvářet interaktivní webové klientské aplikace.

"Jednostránková aplikace" (SPA) je obecný termín pro webovou aplikaci, která načte jednu stránku HTML a následně aktualizuje dynamicky, místo načtení nové stránky na stránku. Po načtení počáteční stránky tato jednostránková aplikace komunikuje se serverem přes odesílání požadavků AJAX.

![](knockoutjs-template/_static/image1.png)

AJAX není nic nového, ale ještě dnes existují architektury JavaScriptu, které usnadňují tvorbu a udržování rozsáhlé sofistikované aplikace SPA. Navíc HTML 5 a CSS3 jsou to usnadňuje vytváření bohatých uživatelského rozhraní.

Abyste mohli začít, šablona jednostránková aplikace vytvoří ukázkovou aplikaci "Seznam úkolů". V tomto kurzu provedeme prohlídku s průvodcem šablony. Nejprve jsme budete podívejte se na vlastní aplikace seznamu úkolů a pak zkontrolujte částí technologie, které jí umožňují fungovat.

## <a name="create-a-new-spa-template-project"></a>Vytvoření nového projektu šablony aplikace SPA

Požadavky:

- Visual Studio 2012 nebo Visual Studio Express 2012 pro Web
- ASP.NET Web Tools 2012.2 update. Můžete nainstalovat aktualizace [tady](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2).

Spusťte sadu Visual Studio a vyberte **nový projekt** z úvodní stránky. Nebo z **souboru** nabídce vyberte možnost **nový** a potom **projektu**.

V **šablony** vyberte **nainstalované šablony** a rozbalte **Visual C#** uzlu. V části **Visual C#** vyberte **webové**. V seznamu šablon projektu vyberte **webové aplikace ASP.NET MVC 4**. Pojmenujte projekt a klikněte na tlačítko **OK**.

![](knockoutjs-template/_static/image2.png)

V **nový projekt** průvodce, vyberte **jednostránkové aplikace**.

![](knockoutjs-template/_static/image3.png)

Stisknutím klávesy F5 sestavte a spusťte aplikaci. Při prvním spuštění aplikace, zobrazí přihlašovací obrazovka.

![](knockoutjs-template/_static/image4.png)

Klikněte na tlačítko &quot;zaregistrovat&quot; propojit a vytvořit nového uživatele.

![](knockoutjs-template/_static/image5.png)

Po přihlášení se aplikace vytvoří výchozí seznam úkolů se dvěma položkami. Klikněte na tlačítko "Přidat seznam úkolů" pro přidání nového seznamu.

![](knockoutjs-template/_static/image6.png)

Přejmenovat seznamu, přidejte položky do seznamu a zkontrolujte. Můžete také odstranit položky nebo odstranit celý seznam. Změny se automaticky ukládají do databáze na serveru (ve skutečnosti LocalDB v tomto okamžiku, protože aplikace je spuštěn místně).

![](knockoutjs-template/_static/image7.png)

## <a name="architecture-of-the-spa-template"></a>Architektura šablon SPA

Tento diagram znázorňuje hlavní stavební bloky pro aplikaci.

![](knockoutjs-template/_static/image8.png)

ASP.NET MVC na straně serveru slouží HTML a zpracovává také ověřování pomocí formulářů.

Rozhraní ASP.NET Web API zpracuje všechny žádosti, které se týkají ToDoLists a ToDoItems, včetně načítání, vytváření, aktualizace nebo odstranění. Klient vyměňuje data s webovým rozhraním API ve formátu JSON.

Entity Framework (EF) je O/RM vrstvy. To zprostředkovává mezi world objektově orientované technologie ASP.NET a základní databáze. Databáze používá LocalDB, ale toto můžete změnit v souboru Web.config. Obvykle by použít databázi LocalDB pro místní vývoj a potom nasadíte do služby SQL database na serveru, pomocí migrace code first EF.

Na straně klienta knihovny rozhraní Knockout.js zpracovává aktualizace stránky AJAX požadavků. Knockout používá datové vazby k synchronizaci na stránce s nejnovější data. Tímto způsobem nemusíte psát žádný kód, který vás provede JSON data a aktualizace modelu DOM. Místo toho kam si ukládáte deklarativních atributů ve formátu HTML, který Knockout instrukce, jak data můžete prezentovat tak.

Velkou výhodou této architektury je, že odděluje prezentační vrstvy z aplikace logiky. Můžete vytvořit části webové rozhraní API bez znalosti nic o vzhled webové stránky. Na straně klienta, můžete vytvořit "model zobrazení" pro reprezentaci dat a model zobrazení Knockout používá k vytvoření vazby v kódu HTML. Který umožňuje snadno změnit beze změny modelu zobrazení HTML. (Podíváme na Knockout o něco později.)

## <a name="models"></a>Modely

V projektu sady Visual Studio obsahuje složku modely modely, které se používají na straně serveru. (Na straně klienta jsou také modely; těm, které dostaneme.)

![](knockoutjs-template/_static/image9.png)

**TodoItem, seznamu úkolů**

Toto jsou modely databáze pro Entity Framework Code First. Všimněte si, že tyto modely mají vlastnosti, které odkazují na sebe navzájem. `ToDoList` obsahuje kolekci objektů Todoitem a každý `ToDoItem` odkazuje zpět na nadřazeného seznamu úkolů. Tyto vlastnosti se nazývají navigačních vlastností a představují vztah jeden mnoho seznam úkolů a jeho položek úkolů.

`ToDoItem` Třídy také používá **[klíč ForeignKey]** atribut určíte, že `ToDoListId` je cizí klíč do `ToDoList` tabulky. To říká EF přidat omezení cizího klíče do databáze.

[!code-csharp[Main](knockoutjs-template/samples/sample1.cs)]

**TodoItemDto, TodoListDto**

Tyto třídy definují data, která se odešlou do klienta. "Objekt DTO" znamená "objekt pro přenos dat." Objekt DTO definuje, jak se entity serializovat do formátu JSON. Obecně platí je několik důvodů, proč používat DTO:

- K řízení vlastnosti, které se serializují. Objekt DTO může obsahovat podmnožinu vlastností z doménového modelu. To můžete udělat z bezpečnostních důvodů (ke skrytí citlivých dat) nebo jednoduše snížit objem dat, která odešlete.
- Chcete-li změnit tvar dat – například k vyrovnání složitější datová struktura.
- Chcete-li zachovat veškeré obchodní logiky mimo DTO (oddělení oblastí zájmu).
- Pokud z nějakého důvodu nelze serializovat doménových modelů. Například cyklické odkazy může způsobit potíže při serializaci objektu existují způsoby, jak zpracovat tento problém v rozhraní Web API (naleznete v tématu [zpracování cyklické odkazy objektu](../../../web-api/overview/formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)); ale pomocí objekt DTO jednoduše se vyhnete problému úplně se vynechá.

V šabloně aplikace SPA DTO obsahuje stejná data jako doménových modelů. Jsou však stále užitečná protože vyhnou cyklické odkazy z navigační vlastnosti a jejich předvedení obecný vzor objekt DTO.

**AccountModels.cs**

Tento soubor obsahuje modely pro členství v síti. `UserProfile` Třída definuje schéma pro uživatelské profily v členství DB. (V tomto případě pouze informace je ID uživatele a uživatelské jméno.) Jiných tříd modelu v tomto souboru se používají k vytváření formulářů registrace a přihlášení uživatele.

## <a name="entity-framework"></a>Entity Framework

Šablona jednostránková aplikace používá platforem EF Code First. Ve vývoji Code First nejprve definovat vzorů v kódu a pak EF používá model k vytvoření databáze. Můžete také použít EF s existující databází ([Database First](https://msdn.microsoft.com/data/jj206878.aspx)).

`TodoItemContext` Je odvozena z třídy ve složce modely **DbContext**. Tato třída poskytuje "spojovací" mezi modely a EF. `TodoItemContext` Obsahuje `ToDoItem` kolekce a `TodoList` kolekce. K dotazování databáze, můžete jednoduše zapsat dotazu LINQ na tyto kolekce. Tady je například jak můžete vybrat všechny seznamy úkolů pro uživatel "Alice":

[!code-csharp[Main](knockoutjs-template/samples/sample2.cs)]

Můžete také přidat nové položky do kolekce, aktualizovat položky, nebo odstranit položky z kolekce a zachová tak změny do databáze.

## <a name="aspnet-web-api-controllers"></a>Kontrolery rozhraní ASP.NET Web API

V rozhraní ASP.NET Web API řadiče jsou objekty, které zpracovávají požadavky HTTP. Jak už bylo zmíněno, šablona jednostránková aplikace používá webového rozhraní API pro povolení operací CRUD na `ToDoList` a `ToDoItem` instancí. Kontrolery jsou umístěny ve složce řadiče řešení.

![](knockoutjs-template/_static/image10.png)

- `TodoController`: Zpracovává požadavky HTTP pro položky seznamu úkolů
- `TodoListController`: Zpracovává požadavky HTTP pro seznamy úkolů.

Tyto názvy jsou důležité, protože odpovídá identifikátoru URI cestu k názvu kontroleru webového rozhraní API. (Informace o tom, jak webové rozhraní API směruje požadavky HTTP na řadiče, naleznete v tématu [směrování v rozhraní ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).)

Podívejme se `ToDoListController` třídy. Obsahuje single – datový člen:

[!code-csharp[Main](knockoutjs-template/samples/sample3.cs)]

`TodoItemContext` Slouží ke komunikaci s EF, jak je popsáno výše. Metody v řadiči provádění operací CRUD. Webové rozhraní API mapuje požadavky HTTP od klienta pro metody kontroleru, následujícím způsobem:

| Požadavek HTTP | Metoda kontroleru | Popis |
| --- | --- | --- |
| ZÍSKAT /api/todo | `GetTodoLists` | Získá kolekci seznamů úkolů. |
| GET/webové rozhraníAPI/todo/*id* | `GetTodoList` | Získá seznam úkolů podle ID |
| Vložení/webové rozhraníAPI/todo/*id* | `PutTodoList` | Aktualizuje seznam úkolů. |
| Publikovat/api/todo | `PostTodoList` | Vytvoří nový seznam úkolů. |
| ODSTRANĚNÍ nebo webové rozhraníAPI/todo/*id* | `DeleteTodoList` | Odstranění seznamu úkolů. |

Všimněte si, že identifikátory URI pro některé operace obsahovaly zástupné symboly pro hodnotu ID. Například k odstranění na seznamu s ID 42, identifikátor URI je `/api/todo/42`.

Další informace o používání webového rozhraní API pro operace CRUD najdete v tématu [této operace CRUD podporuje vytvoření webového rozhraní API](../../../web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations.md). Kód pro tento kontroler je poměrně jednoduché. Tady jsou některé zajímavé body:

- `GetTodoLists` Metoda používá dotaz LINQ pro filtrování výsledků podle ID přihlášeného uživatele. Tímto způsobem, uživateli se zobrazí pouze data, která patří do nového zaměstnance odkázat. Všimněte si také, že příkaz Select se používá k převodu `ToDoList` instance do `TodoListDto` instancí.
- Metody PUT a příspěvku zkontrolovat stav modelu před změnou databáze. Pokud **ModelState.IsValid** má hodnotu false, vrátí tyto metody HTTP, chybný požadavek 400. Další informace o ověření modelu v rozhraní Web API v [ověření modelu](../../../web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api.md).
- Třída kontroleru je doplněn také **[Authorize]** atribut. Tento atribut kontroluje, zda je ověřený požadavek HTTP. Pokud požadavek nebude ověřený, obdrží klient HTTP 401 Neautorizováno. Další informace o ověřování v [ověřování a autorizace v rozhraní ASP.NET Web API](../../../web-api/overview/security/authentication-and-authorization-in-aspnet-web-api.md).

`TodoController` Třída je velmi podobný `TodoListController`. Největším rozdílem je, že nedefinuje žádné metody GET, protože klient se zobrazí položky úkolů společně s každou seznam úkolů.

## <a name="mvc-controllers-and-views"></a>Kontrolery a zobrazení MVC

Kontrolery MVC jsou také umístěny ve složce řadiče řešení. `HomeController` vykreslí hlavní HTML pro aplikaci. Zobrazení pro kontroler Home je definováno v Views/Home/Index.cshtml. Domovská stránka zobrazení vykreslí rozdílný obsah v závislosti na tom, jestli je uživatel přihlášen:

[!code-cshtml[Main](knockoutjs-template/samples/sample4.cshtml)]

Když se uživatelé přihlásí, zobrazí se jim hlavní uživatelské rozhraní. Zobrazí se jim v opačném případě panelu přihlášení. Všimněte si, že se stane toto podmíněné vykreslování na straně serveru. Nikdy se pokusí skrýt citlivý obsah na straně klienta & #8212anything odesílané v odpovědi protokolu HTTP je viditelná pro uživatele, který sleduje nezpracované zprávy HTTP.

## <a name="client-side-javascript-and-knockoutjs"></a>JavaScript na straně klienta a knihovnou Knockout.js

Teď přejdeme na straně serveru do klienta aplikace. Šablona jednostránková aplikace používá kombinaci jQuery a rozhraní Knockout.js vytvořit plynulé a na interaktivní uživatelské rozhraní. Rozhraní Knockout.js je knihovna jazyka JavaScript, která umožňuje jednoduše vázat na data ve formátu HTML. Rozhraní Knockout.js používá vzor nazývá "Model-View-ViewModel."

- Data domény (ToDo seznamy a položky seznamu úkolů) je model.
- Zobrazení je dokument HTML.
- Model zobrazení je objekt jazyka JavaScript, která obsahuje data modelu. Model zobrazení je abstrakce kód uživatelského rozhraní. Nemá žádné znalosti jazyka HTML představující. Místo toho představuje abstraktní funkce zobrazení, jako je například "seznam položek ToDo".

Zobrazení je vázán na data modelu zobrazení. Aktualizace zobrazení modelu se automaticky projeví v zobrazení. Vazby fungují opačným směrem. Události v modelu DOM (například kliknutí) jsou vázané na data na funkce na model zobrazení, která aktivuje volání AJAX.

Jednostránková aplikace šablony slouží k uspořádání JavaScript na straně klienta do tři vrstvy:

- TODO.DataContext.js: odešle žádost o AJAX.
- TODO.model.js: definuje modely.
- TODO.ViewModel.js: definuje model zobrazení.

![](knockoutjs-template/_static/image11.png)

Tyto soubory skriptu jsou umístěny ve složce skripty nebo aplikace řešení.

![](knockoutjs-template/_static/image12.png)

**TODO.DataContext** zpracovává všechna volání AJAX do kontrolerů rozhraní Web API. (Volání AJAX pro přihlášení je definována jinde, v ajaxlogin.js.)

**TODO.model.js** definuje modely (prohlížeč) na straně klienta pro seznamy úkolů. Existují dvě třídy modelu: todoItem a seznamu úkolů.

Mnoho vlastností ve třídách modelu jsou typu "ko.observable". Pozorovatelné objekty jsou, jak jeho magic Knockout. Z [Knockout dokumentaci](http://knockoutjs.com/documentation/introduction.html): existuje zjištěný je "objekt jazyka JavaScript, který může upozornit předplatitele o změnách." Při změně hodnoty pozorovat, aktualizuje Knockout elementy HTML, které jsou vázány na těchto pozorovatelné objekty. Pozorovatelné objekty pro vlastnosti název a isDone má například todoItem:

[!code-javascript[Main](knockoutjs-template/samples/sample5.js)]

Můžete také odebírat existuje zjištěný v kódu. Například třída todoItem se přihlásí k odběru změn ve vlastnostech "isDone" a "title":

[!code-javascript[Main](knockoutjs-template/samples/sample6.js)]

**Model zobrazení**

Model zobrazení je definováno v todo.viewmodel.js. Model zobrazení je ústřední bod, kde aplikace propojena prvky stránky HTML dat domény. V šabloně SPA obsahuje model zobrazení pozorovatelných pole todoLists. Následující kód v zobrazení modelu říká Knockout použít vazby:

[!code-javascript[Main](knockoutjs-template/samples/sample7.js)]

## <a name="html-and-data-binding"></a>HTML a vytváření datových vazeb

Hlavní HTML na stránce je definován v Views/Home/Index.cshtml. Vzhledem k tomu, že používáme datovou vazbu, kód HTML je jenom šablony pro co ve skutečnosti získá vykreslen. Používá knockout *deklarativní* vazby. Prvky stránky lze vázat na data tak, že do elementu přidáte atribut "data-bind". Tady je velmi jednoduchý příklad, provést v dokumentaci ke službě Knockout:

[!code-html[Main](knockoutjs-template/samples/sample8.html)]

V tomto příkladu Knockout aktualizuje obsah **&lt;span&gt;** element s hodnotou `myItems.count()`. Pokaždé, když se tato hodnota se změní, Knockout dokument aktualizuje.

Knockout obsahuje několik typů jinou vazbou. Tady jsou některé vazby používá v šabloně jednostránková aplikace:

- **foreach**: umožňuje iteraci smyčky a použít stejné značky pro každou položku v seznamu. To slouží k vykreslení seznamy úkolů a položkami seznamu úkolů. V rámci **foreach**, vazby jsou použity na prvky ze seznamu.
- **viditelné**: Umožňuje přepnout viditelnost. Skrýt značky, pokud kolekce je prázdná nebo zviditelnit chybová zpráva.
- **Hodnota**: použitých k naplnění hodnot formuláře.
- **Klikněte na tlačítko**: váže událost click k funkci v modelu zobrazení.

## <a name="anti-csrf-protection"></a>Ochrana proti CSRF

Padělání žádosti mezi weby (CSRF) je útok, kde škodlivý web odešle požadavek na zranitelné lokality, kde uživatel je momentálně přihlášený. Aby se zabránilo útokům CSRF, ASP.NET MVC používá *tokenů proti padělání*, nazývané také žádosti o ověření tokenů. Cílem je, že server umístí náhodně vygenerovaný token do webové stránky. Když klient odešle data na server, musí obsahovat tuto hodnotu do zprávy požadavku.

Tokenů proti padělání pracovat, protože škodlivý stránku nelze číst tokeny uživatele z důvodu zásadami stejného původu. (Stejného zdroje kvůli zásadám dokumenty hostovaný na dvou různých lokalit v přístupu k obsahu druhé strany.)

ASP.NET MVC poskytuje integrovanou podporu pro tokeny proti zfalšování, až [AntiForgery](https://msdn.microsoft.com/library/system.web.helpers.antiforgery.aspx) třídy a [[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute.aspx) atribut. Tato funkce není v současné době součástí webového rozhraní API. Šablona SPA však zahrnuje vlastní implementaci webového rozhraní API. Tento kód je definovaný v `ValidateHttpAntiForgeryTokenAttribute` třídy, který je umístěn ve složce filtry řešení. Další informace o anti-CSRF v rozhraní Web API najdete v tématu [brání webů požádat o ÚTOKŮ Csrf útoky](../../../web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="conclusion"></a>Závěr

Jednostránková aplikace šablona je určena vám pomůžou začít rychle vytváření moderních, interaktivních webových aplikací. Knihovně rozhraní Knockout.js používá k oddělení prezentaci (značka jazyka HTML) z dat a aplikací logiky. Ale Knockout není pouze knihovny JavaScriptu, která vám pomůže vytvořit SPA. Pokud chcete prozkoumat některé další možnosti, podívejte se na [komunitou vytvořených šablon SPA](../templates/index.md).
