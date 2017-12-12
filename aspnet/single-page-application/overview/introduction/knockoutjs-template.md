---
uid: single-page-application/overview/introduction/knockoutjs-template
title: "Jedné stránky aplikací: Šablona kódem KnockoutJS | Microsoft Docs"
author: MikeWasson
description: "Knockout šablony"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: f9c07af0-4b20-4b08-af8f-47fc3df169a2
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/introduction/knockoutjs-template
msc.type: authoredcontent
ms.openlocfilehash: 6e84dcc16345e33fcd3a3f83c4b35bc993c03ca6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="single-page-application-knockoutjs-template"></a>Jedné stránky aplikací: Šablona kódem KnockoutJS
====================
podle [Wasson Jan](https://github.com/MikeWasson)

> Šablony MVC Knockout je součástí technologie ASP.NET a webové nástroje 2012.2
> 
> [Stáhněte ASP.NET a nástroje pro Web 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)


Tato technologie ASP.NET a webové nástroje 2012.2 aktualizace zahrnuje šablona jednostránkové aplikace (SPA) pro technologii ASP.NET MVC 4. Tato šablona je navržená tak, abyste mohli začít rychle vytvářet interaktivní webové klientské aplikace.

"Jednostránkové aplikace" (SPA) je obecný termín pro webovou aplikaci, která načte jediné stránce HTML a pak aktualizuje dynamicky, místo načítání stránky, nové stránky. Po načtení úvodní stránky SPA komunikuje se serverem prostřednictvím požadavky AJAX.

![](knockoutjs-template/_static/image1.png)

AJAX není nic nového, ale existují ještě dnes JavaScript rozhraní, které usnadňují tvorbu a udržování rozsáhlé sofistikované SPA aplikace. Navíc standardu HTML 5 a CSS3 jsou usnadnit vytvoření bohaté uživatelská rozhraní.

Abyste mohli začít, vytvoří šablona SPA ukázkovou aplikaci "Seznam úkolů". V tomto kurzu provedeme Průvodce šablony. Nejprve jsme budete podívejte se na vlastní aplikace seznamu úkolů, a poté zkontrolujte částí technologie, které fungovat.

## <a name="create-a-new-spa-template-project"></a>Vytvoření nového projektu šablony SPA

Požadavky:

- Visual Studio 2012 nebo Visual Studio Express 2012 pro Web
- Aktualizace nástrojů rozhraní ASP.NET Web 2012.2. Můžete nainstalovat aktualizace [zde](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2).

Spuštění sady Visual Studio a vyberte **nový projekt** z úvodní stránky. Nebo z **soubor** nabídce vyberte možnost **nový** a potom **projektu**.

V **šablony** podokně, vyberte **nainstalovaných šablonách** a rozbalte **Visual C#** uzlu. V části **Visual C#**, vyberte **webové**. V seznamu šablon projektu, vyberte **webové aplikace ASP.NET MVC 4**. Název projektu a klikněte na tlačítko **OK**.

![](knockoutjs-template/_static/image2.png)

V **nový projekt** průvodci vyberte **jedné stránky aplikace**.

![](knockoutjs-template/_static/image3.png)

Stisknutím klávesy F5 sestavení a spuštění aplikace. Při prvním spuštění aplikace, zobrazí se přihlašovací obrazovka.

![](knockoutjs-template/_static/image4.png)

Klikněte &quot;zaregistrovat&quot; propojit a vytvořit nového uživatele.

![](knockoutjs-template/_static/image5.png)

Po přihlášení, vytvoří aplikace výchozím seznamu úkolů se dvěma položkami. Klikněte na tlačítko "Přidat seznam úkolů" pro přidání nového seznamu.

![](knockoutjs-template/_static/image6.png)

Přejmenování seznamu, přidejte položky do seznamu a vrátit je. Můžete také odstranit položky nebo odstranit celý seznam. K databázi na serveru jsou automaticky trvalé změny (ve skutečnosti LocalDB nyní, protože aplikace je spuštěn místně).

![](knockoutjs-template/_static/image7.png)

## <a name="architecture-of-the-spa-template"></a>Architektura SPA šablony

Tento diagram znázorňuje hlavní stavební bloky pro aplikaci.

![](knockoutjs-template/_static/image8.png)

ASP.NET MVC na straně serveru slouží HTML a také obstará ověřování pomocí formulářů.

Rozhraní ASP.NET Web API zpracovává všechny požadavky, které se týkají ToDoLists a ToDoItems, včetně získávání, vytváření, aktualizace a odstranění. Klient výměny dat pomocí webového rozhraní API ve formátu JSON.

Entity Framework (EF) je O/RM vrstvy. Ho zprostředkovává mezi objektově orientované world technologie ASP.NET a základní databáze. Databáze používá LocalDB, ale toto můžete změnit v souboru Web.config. Obvykle byste použijte LocalDB pro místní vývoj a pak nasadit do databáze SQL na serveru pomocí migrace code first EF.

Na straně klienta zpracovává knihovně Knockout.js aktualizace stránky z požadavky AJAX. Knockout používá datové vazby k synchronizaci stránky s nejnovější data. Tímto způsobem, nemusíte psát kód, který provede JSON data a aktualizuje modelu DOM. Místo toho uveďte deklarativní atributy ve formátu HTML, který informování Knockout jak data k dispozici.

Big výhod této architektury je ji odděluje prezentační vrstvy od logiku aplikace. Můžete vytvořit část webového rozhraní API bez znalosti nic o tom, jak bude vypadat webové stránky. Na straně klienta, vytvoření "zobrazení modelu" k představují data a zobrazení model používá Knockout k vytvoření vazby v kódu HTML. Který vám umožní snadno změnit HTML beze změny modelu zobrazení. (Podíváme Knockout trochu později.)

## <a name="models"></a>Modely

V projektu sady Visual Studio obsahuje složku modely modely, které se používají na straně serveru. (Na straně klienta jsou také modely, budete se nám získat ty.)

![](knockoutjs-template/_static/image9.png)

**TodoItem seznamu úkolů**

Jsou to databáze modely pro Entity Framework Code First. Všimněte si, že tyto modely mají vlastnosti, které odkazují na sebe navzájem. `ToDoList`obsahuje kolekci ToDoItems a každý `ToDoItem` odkazuje zpět na nadřazené seznamu úkolů. Tyto vlastnosti se nazývají navigačních vlastností a seznam úkolů a jeho položky představují vztah jeden mnoho.

`ToDoItem` Třídy také používá **[cizí klíč]** atribut určíte, že `ToDoListId` je cizí klíč do `ToDoList` tabulky. Tato hodnota informuje EF přidat omezení cizího klíče do databáze.

[!code-csharp[Main](knockoutjs-template/samples/sample1.cs)]

**TodoItemDto TodoListDto**

Tyto třídy zadat data, která bude odeslána do klienta. "DTO" znamená "objekt pro přenos dat." DTO definuje, jak budou serializovány entity do formátu JSON. Obecně platí tady je několik důvodů používat DTOs:

- Pokud chcete řídit, vlastnosti, které se serializují. DTO může obsahovat podmnožinu vlastnosti z modelu domény. To můžete udělat z bezpečnostních důvodů (Chcete-li skrýt citlivá data) nebo jednoduše snížit objem dat, která odešlete.
- Chcete-li změnit tvaru dat – například k vyrovnání složitější datová struktura.
- Chcete-li zachovat veškeré obchodní logiky mimo DTO (oddělené oblasti zájmu).
- Pokud z nějakého důvodu nelze serializovat modely vaší domény. Například cyklické odkazy může způsobit problémy při serializaci objektu existují způsoby, jak zpracovávat tento problém v rozhraní Web API (v tématu [zpracování cyklické odkazy objekt](../../../web-api/overview/formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)); ale pomocí DTO jednoduše zabraňuje zcela problém.

V šabloně SPA DTOs obsahuje stejná data jako modely domény. Jsou to ale stále užitečné, protože vyhnou cyklické odkazy z navigační vlastnosti a jsou prokázat vzoru obecné DTO.

**AccountModels.cs**

Tento soubor obsahuje modely pro členství webu. `UserProfile` Třída definuje schéma pro uživatelské profily v databázi členství. (V tomto případě pouze informace je ID uživatele a uživatelské jméno.) Ostatní třídy modelu v tomto souboru se používají při vytváření formulářů registrace a přihlášení uživatele.

## <a name="entity-framework"></a>Entity Framework

Šablona SPA používá EF Code First. Vývojem Code First můžete definovat modely, nejprve v kódu a pak EF používá model k vytvoření databáze. Můžete také použít EF s existující databázi ([Database First](https://msdn.microsoft.com/en-us/data/jj206878.aspx)).

`TodoItemContext` Je odvozena od třídy ve složce modely **DbContext**. Tato třída poskytuje "spojovací" mezi modely a EF. `TodoItemContext` Obsahuje `ToDoItem` kolekce a `TodoList` kolekce. K dotazování databáze, jednoduše napsat dotaz LINQ proti těchto kolekcí. Můžete zde je ukázka, jak můžete vybrat všechny seznamy úkolů pro uživatele "Alice":

[!code-csharp[Main](knockoutjs-template/samples/sample2.cs)]

Můžete taky přidat nové položky do kolekce, položky, aktualizovat nebo odstranit položky z kolekce a zachová tak změny do databáze.

## <a name="aspnet-web-api-controllers"></a>Rozhraní ASP.NET Web API řadiče

V rozhraní ASP.NET Web API řadiče jsou objekty, které zpracovávají požadavky HTTP. Jak je uvedeno, šablona SPA používá webového rozhraní API pro povolení operací CRUD na `ToDoList` a `ToDoItem` instance. Na řadiče jsou umístěny ve složce řadiče řešení.

![](knockoutjs-template/_static/image10.png)

- `TodoController`: Zpracovává požadavky protokolu HTTP pro položkami seznamu úkolů
- `TodoListController`: Zpracovává požadavky HTTP na seznamu úkolů.

Tyto názvy jsou důležitá, protože webového rozhraní API odpovídá cesta k identifikátoru URI k názvu řadiče. (Informace o tom, jak webového rozhraní API směruje požadavky HTTP na řadiče, najdete v části [směrování v rozhraní ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).)

Podívejme se na `ToDoListController` třídy. Single – datový člen obsahuje:

[!code-csharp[Main](knockoutjs-template/samples/sample3.cs)]

`TodoItemContext` Se používá ke komunikaci s EF, jak je popsáno výše. Metody na řadiči implementovat operace CRUD. Webové rozhraní API mapuje požadavky protokolu HTTP od klienta pro metody kontroleru, následujícím způsobem:

| Požadavek HTTP | Metoda kontroleru | Popis |
| --- | --- | --- |
| ZÍSKAT /api/todo | `GetTodoLists` | Získá kolekci seznamu úkolů. |
| GET/api/todo/*id* | `GetTodoList` | Získá seznam úkolů podle ID |
| Vložení/api/todo/*id* | `PutTodoList` | Aktualizace seznamu úkolů. |
| POST/api/todo | `PostTodoList` | Vytvoří nový seznam úkolů. |
| ODSTRANĚNÍ/api/todo/*id* | `DeleteTodoList` | Odstraní seznamu úkolů. |

Všimněte si, že identifikátory URI pro některé operace obsahovat zástupné symboly pro hodnotu ID. Například pokud chcete odstranit k – seznam s ID 42, identifikátor URI je `/api/todo/42`.

Další informace o používání webového rozhraní API pro operace CRUD najdete v tématu [vytváření webového rozhraní API této operace CRUD podporuje](../../../web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations.md). Kód pro tento řadič je docela jednoduchá. Zde jsou některé zajímavé body:

- `GetTodoLists` Metoda používá dotaz LINQ filtrovat výsledky podle ID přihlášeného uživatele. Tímto způsobem, uživateli se zobrazí jenom data, která patří do mu. Všimněte si také, že příkaz Select se používá k převodu `ToDoList` instance do `TodoListDto` instance.
- Metody PUT a POST zkontrolovat stav modelu před změnou databáze. Pokud **ModelState.IsValid** je nastavena hodnota false, tyto metody vrací HTTP 400, chybný požadavek. Další informace o ověření modelu v rozhraní Web API v [ověření modelu](../../../web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api.md).
- Třída kontroleru je také označených pomocí **[Authorize]** atribut. Tento atribut kontroluje, zda je ověřen požadavek HTTP. Pokud požadavek není ověřen, obdrží klient HTTP 401 Neautorizováno. Další informace o ověřování v [ověřování a autorizace v rozhraní ASP.NET Web API](../../../web-api/overview/security/authentication-and-authorization-in-aspnet-web-api.md).

`TodoController` Třída je velmi podobné `TodoListController`. Největších rozdíl je v nedefinuje žádné metody GET, protože klient získá položkami seznamu úkolů společně s každou seznamu úkolů.

## <a name="mvc-controllers-and-views"></a>Kontrolery a zobrazení MVC

Kontrolery MVC jsou také umístěný ve složce řadiče řešení. `HomeController`vykreslí hlavní HTML pro aplikaci. Zobrazení pro řadič domovské je definována v Views/Home/Index.cshtml. Zobrazení Domovská stránka vykresluje jiný obsah, v závislosti na tom, jestli je uživatel přihlášen:

[!code-cshtml[Main](knockoutjs-template/samples/sample4.cshtml)]

Když se uživatelé přihlásí, uvidí hlavní uživatelské rozhraní. Jinak uvidí panelu přihlášení. Všimněte si, že se stane toto podmíněného vykreslování na straně serveru. Chcete-li skrýt citlivého obsahu na straně klienta, nikdy nepokouší & #8212anything odeslané v odpovědi HTTP se zobrazí uživateli, který sleduje nezpracované zprávy HTTP.

## <a name="client-side-javascript-and-knockoutjs"></a>JavaScript na straně klienta a Knockout.js

Nyní přejdeme z aplikace pro klienta na straně serveru. Šablona SPA používá kombinaci jQuery a Knockout.js vytvořit smooth, interaktivní uživatelské rozhraní. Knockout.js je knihovna JavaScript, který usnadňuje vytvoření vazby HTML k datům. Knockout.js používá vzor názvem "Model-View-ViewModel."

- Model je domény data (ToDo seznamy a položky ToDo).
- Zobrazení je dokumentu HTML.
- Model zobrazení je objekt jazyka JavaScript, který obsahuje data modelu. Model zobrazení je abstrakce kód uživatelského rozhraní. Nemá žádné znalosti reprezentace HTML. Místo toho představuje abstraktní funkce zobrazení, jako je například "seznam položek ToDo".

Zobrazení je vázané na data do modelu zobrazení. Aktualizace do modelu zobrazení, se automaticky promítnou v zobrazení. Vazby fungovat opačným směrem také. Události v modelu DOM (například klikne na) jsou vázané na data pro funkce na modelu zobrazení, které aktivují volání AJAX.

Šablona SPA uspořádává JavaScript na straně klienta do tři vrstvy:

- TODO.DataContext.js: odešle požadavky AJAX.
- TODO.model.js: definuje modely.
- TODO.ViewModel.js: definuje model zobrazení.

![](knockoutjs-template/_static/image11.png)

Tyto soubory skriptů jsou umístěny ve složce skripty nebo aplikace řešení.

![](knockoutjs-template/_static/image12.png)

**TODO.DataContext** zpracovává všechna volání AJAX řadičům webového rozhraní API. (Volání AJAX pro protokolování jsou definovány jinam, v ajaxlogin.js.)

**TODO.model.js** definuje modely na straně klienta (prohlížeč) pro seznamu úkolů. Existují dvě třídy modelu: todoItem a seznamu úkolů.

Mnoho z těchto vlastností ve třídách modelu jsou typu "ko.observable". Pozorovatelné objekty jsou, jak funguje Knockout jeho magic. Z [Knockout dokumentace](http://knockoutjs.com/documentation/introduction.html): existuje zjištěný je "JavaScript objekt, který může upozornit odběratele o změnách." Při hodnotě existuje zjištěný Knockout aktualizuje všechny elementy HTML, které jsou vázány na těchto pozorovatelné objekty. Například todoItem má pozorovatelné objekty vlastností nadpisu a isDone:

[!code-javascript[Main](knockoutjs-template/samples/sample5.js)]

Můžete také odebírat existuje zjištěný v kódu. Například třída todoItem předplatila změny ve vlastnostech "isDone" a "title":

[!code-javascript[Main](knockoutjs-template/samples/sample6.js)]

**Model zobrazení**

Model zobrazení je definována v todo.viewmodel.js. Model zobrazení je centrální bod, kde aplikace sváže prvky stránky HTML data domény. Model zobrazení v šabloně SPA obsahuje pozorovatelné pole todoLists. Následující kód do modelu zobrazení informuje Knockout pro použití vazby:

[!code-javascript[Main](knockoutjs-template/samples/sample7.js)]

## <a name="html-and-data-binding"></a>Kód HTML a datová vazba

Hlavní HTML na stránce je definován v Views/Home/Index.cshtml. Vzhledem k tomu, že používáme vazby dat, je HTML pouze šablonu pro co ve skutečnosti získá vykreslen. Používá knockout *deklarativní* vazby. Prvky stránky lze vázat na data přidáním atribut "data-bind" do elementu. Tady je velmi jednoduchý příklad, provést v dokumentaci k Knockout:

[!code-html[Main](knockoutjs-template/samples/sample8.html)]

V tomto příkladu Knockout aktualizuje obsah  **&lt;span&gt;**  element s hodnotou `myItems.count()`. Vždy, když je tato hodnota se změní, aktualizuje Knockout dokumentu.

Knockout poskytuje několik typů jinou vazbou. Tady jsou některé z vazeb použité v šabloně SPA:

- **foreach**: umožňuje iteraci smyčky a použít stejný kód pro každou položku v seznamu. To slouží k vykreslení seznamu úkolů a položkami seznamu úkolů. V rámci **foreach**, vazby se použijí pro prvky v seznamu.
- **viditelné**: používá se pro přepínání viditelnosti. Skrýt značky, pokud kolekce je prázdný, nebo zobrazit chybová zpráva.
- **Hodnota**: používaných k naplnění hodnoty formuláře.
- **Klikněte na tlačítko**: váže událost click funkce na zobrazení modelu.

## <a name="anti-csrf-protection"></a>Ochrana proti proti útokům CSRF

Proti útokům webů padělání požadavku (CSRF) je útok, při kterém škodlivé weby odešle požadavek na citlivé lokality, kde uživatel momentálně přihlášen. Aby se zabránilo útokům proti útokům CSRF, ASP.NET MVC používá *tokeny proti zfalšování*, nazývané také žádosti o ověření tokenů. Cílem je, že server umístí náhodně generované token do na webové stránce. Když klient odešle data na server, musí obsahovat tuto hodnotu ve zprávě požadavku.

Tokeny proti zfalšování fungovat, protože škodlivý stránky nelze přečíst tokeny uživatele z důvodu zásad stejného původu. (Stejného původu zásady zabránit dokumenty, které jsou hostované na dvou různých lokalit v přístupu k obsahu vzájemně.)

ASP.NET MVC poskytuje integrovanou podporu pro tokeny proti zfalšování, prostřednictvím [AntiForgery](https://msdn.microsoft.com/en-us/library/system.web.helpers.antiforgery.aspx) třídy a [[ValidateAntiForgeryToken]](https://msdn.microsoft.com/en-us/library/system.web.mvc.validateantiforgerytokenattribute.aspx) atribut. Tato funkce není v současné době součástí webového rozhraní API. Šablona SPA však obsahuje vlastní implementaci pro webového rozhraní API. Tento kód je definovaný v `ValidateHttpAntiForgeryTokenAttribute` třídy, který je umístěný ve složce filtry řešení. Další informace o anti-proti v rozhraní Web API útokům CSRF najdete v tématu [útoky brání webů požadavku padělání (proti útokům CSRF)](../../../web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="conclusion"></a>Závěr

Abyste mohli začít rychle zápis moderní, interaktivní webové aplikace slouží k šabloně SPA. Knihovna Knockout.js používá k oddělení prezentace (značka jazyka HTML) z dat a aplikací logiky. Ale Knockout není pouze knihovna JavaScript, kterou můžete použít k vytvoření SPA. Pokud chcete prozkoumat některé další možnosti, podívejte se na [komunity vytvořené šablony SPA](../templates/index.md).
