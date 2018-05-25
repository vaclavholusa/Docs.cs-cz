---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Směrování a výběr akce v rozhraní ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2012
ms.topic: article
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 997582263bd48590b74434ee0ffc6be928fa1e08
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a>Směrování a výběr akce v rozhraní ASP.NET Web API
====================
podle [Wasson Jan](https://github.com/MikeWasson)

Tento článek popisuje, jak rozhraní ASP.NET Web API směruje požadavek HTTP na určité akce v kontroleru.

> [!NOTE]
> Souhrnné informace o směrování, najdete v části [směrování v rozhraní ASP.NET Web API](routing-in-aspnet-web-api.md).


Tento článek vypadá na podrobnosti o procesu směrování. Pokud vytvoříte projekt webového rozhraní API a zjistíte, že některé požadavky Nezískávat směrovány očekávaným způsobem, zpravidla Tento článek vám pomůže.

Směrování rozdělená do tří hlavních fází:

1. Odpovídající identifikátor URI pro šablonu trasy.
2. Vybere kontroler.
3. Výběr akce.

Některé části procesu můžete nahradit vlastní vlastní chování. V tomto článku I popisují výchozí chování. Na konci poznámky I míst, kde lze přizpůsobit chování.

## <a name="route-templates"></a>Šablon trasy

Šablona trasy bude vypadat podobně jako cesta URI, ale může mít zástupný symbol hodnoty, vyznačené složené závorky:

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

Když vytvoříte trasu, je zadat výchozí hodnoty pro některé nebo všechny zástupné symboly:

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

Můžete zadat také omezení, které omezují jak segment identifikátoru URI se může shodovat zástupný text:

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

Rozhraní se pokusí vyhledat segmentů v cestě URI do šablony. Literály v šabloně musí přesně shodovat. Zástupný text odpovídá žádnou hodnotu nezadáte omezení. Rozhraní framework neodpovídá dalších částí identifikátoru URI, jako je název hostitele nebo parametry dotazu. Rozhraní framework vybírá první směrování ve směrovací tabulce, která odpovídá identifikátoru URI.

Existují dva speciální zástupné znaky: "{controller}" a "{action}".

- "{controller}" poskytuje název řadiče.
- "{action}" poskytuje název akce. V rozhraní Web API je obvykle konvence vynechejte "{action}".

### <a name="defaults"></a>Ve výchozím nastavení

Pokud zadáte výchozí hodnoty, trasy, která bude odpovídat identifikátor URI, který chybí tyto segmenty. Příklad:

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

Identifikátor URI "`http://localhost/api/products`" odpovídá této trase. Segment "{kategorie}" je přiřazena "vše" Výchozí hodnota.

### <a name="route-dictionary"></a>Slovníku trasy

Pokud rozhraní najde shoda pro identifikátor URI, vytvoří slovník, který obsahuje hodnotu pro každý zástupný symbol. U klíčů se názvy zástupného symbolu, není včetně složené závorky. Hodnoty jsou převzaty z cesta k identifikátoru URI nebo z výchozí hodnoty. Slovník je uložen v **IHttpRouteData** objektu.

V průběhu této fáze odpovídající trasy zvláštní "{controller}" a "{action}" zástupné symboly jsou považovány stejně jako ostatní zástupné symboly. Jednoduše uložených ve slovníku hodnoty ostatních.

Výchozí může mít speciální hodnotu **RouteParameter.Optional**. Pokud zástupný symbol získá přiřazenou tuto hodnotu, hodnotu nebyla přidána do slovníku trasy. Příklad:

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

Cesta k identifikátoru URI "rozhraní api nebo produkty" bude obsahovat slovníku trasy:

- Řadič: "produkty"
- kategorie: "vše"

Pro "rozhraní api nebo produkty/toys/123" ale slovníku trasy bude obsahovat:

- Řadič: "produkty"
- kategorie: "toys"
- ID: "123"

Výchozí hodnoty použít také hodnotu, která není vyskytovat kdekoli v šabloně trasy. Pokud trasa odpovídá, tato hodnota je uložena ve slovníku. Příklad:

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

Pokud cesta k identifikátoru URI je "kořenový/api/8", bude obsahovat slovníku dvou hodnot:

- Řadič: "zákazníci"
- ID: "8"

## <a name="selecting-a-controller"></a>Výběr Kontroleru

Výběr řadiče jsou zpracována **IHttpControllerSelector.SelectController** metoda. Tato metoda přebírá **HttpRequestMessage** instance a vrátí **HttpControllerDescriptor**. Poskytuje výchozí implementaci **DefaultHttpControllerSelector** třídy. Tato třída využívá přehledné algoritmus:

1. Vyhledejte ve slovníku trasy pro klíč "controller".
2. Z hodnoty pro tento klíč a připojte řetězec "Controller" k získání názvu typu kontroleru.
3. Podívejte se na kontroler Web API s tímto názvem typu.

Například pokud slovníku trasy obsahuje dvojice klíč hodnota "controller" = "produkty", pak typ kontroleru je "ProductsController". Pokud neexistuje žádný odpovídající typ nebo více shod, rozhraní vrátí chybu do klienta.

Krok 3 **DefaultHttpControllerSelector** používá **IHttpControllerTypeResolver** rozhraní získat seznam typů kontroleru webového rozhraní API. Výchozí implementaci **IHttpControllerTypeResolver** vrátí všechny veřejné třídy, které implementují (a) **IHttpController**, (b) je abstraktní a (c) mít název, který končí na "Controller".

## <a name="action-selection"></a>Výběr akce

Po výběru kontroleru, rozhraní vybere akci voláním **IHttpActionSelector.SelectAction** metoda. Tato metoda přebírá **HttpControllerContext** a vrátí **HttpActionDescriptor**.

Poskytuje výchozí implementaci **ApiControllerActionSelector** třídy. Vyberte akci, vypadá na následující:

- Metoda HTTP požadavku.
- Zástupný symbol "{action}" v šabloně trasy, pokud je k dispozici.
- Parametry akce v kontroleru.

Před prohlížení algoritmus výběr, je potřeba pochopit některé věci o akce kontroleru.

**Které metody na řadiči, jsou považovány za "akce"?** Když vyberete akci, rozhraní pouze vypadá na veřejné instance metody na řadiči. Navíc vyloučí ["speciální název"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) metody (konstruktory, události, přetížení operátoru a tak dále) a metody zděděno z **objektu ApiController** třídy.

**Metody HTTP.** Rozhraní framework vybere pouze akce, které odpovídají metoda HTTP požadavku, stanoven následujícím způsobem:

1. Zadávat lze metodu HTTP s atributem: **AcceptVerbs**, **HttpDelete**, **třídy MetadataExchangeClientMode**, **HttpHead**,  **Httpoptions měl**, **HttpPatch**, **HttpPost**, nebo **HttpPut**.
2. Jinak Pokud název metody kontroleru začíná "Get", "Post", "Put", "Odstranit", "Head", "Možnosti" nebo "Patch", pak podle konvence akce podporuje dané metody HTTP.
3. Pokud žádná z výše, podporuje metodu POST.

**Parametr vazby.** Vazba parametru je, jak webové rozhraní API vytvoří hodnotu pro parametr. Tady je výchozí pravidlo pro vazbu parametru:

- Jednoduché typy jsou převzaty z identifikátoru URI.
- Komplexní typy jsou převzaty z textu požadavku.

Jednoduché typy zahrnout všechny [primitivní typy rozhraní .NET Framework](https://msdn.microsoft.com/library/system.type.isprimitive), plus **data a času**, **Decimal**, **Guid**, **řetězec** , a **časový interval**. Pro každou akci může číst maximálně jeden parametr textu požadavku.

> [!NOTE]
> Je možné přepsat výchozí pravidla pro vazbu. V tématu [vazbu parametru WebAPI pod pokličkou](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).


S na pozadí zde je algoritmus pro výběr akce.

1. Vytvoří seznam všech akcí na řadiči, který bude odpovídat metoda požadavku HTTP.
2. Pokud slovníku trasy má záznam "action", odeberte akce, jejichž název neodpovídá tuto hodnotu.
3. Zkuste tak, aby odpovídaly parametrů akcí na identifikátor URI, následujícím způsobem: 

    1. Pro každou akci získáte seznam parametrů, které jsou jednoduchý typ, kde vazby získá parametr z identifikátoru URI. Vylučte volitelné parametry.
    2. Z tohoto seznamu pokusí se najít shodu pro název každého parametru ve slovníku trasy nebo v řetězci dotazu identifikátoru URI. Shoduje se malá a velká písmena a nezávisí na jejich pořadí.
    3. Vyberte akci, kde každý parametr v seznamu má odpovídající v identifikátoru URI.
    4. Pokud více této jednu akci splňuje tato kritéria, vyberte jednu s většina odpovídá parametru.
4. Ignorovat akce s **[NonAction]** atribut.

Krok #3 je pravděpodobně matoucí nejvíce. Základní cílem je, že parametr můžete získat svou hodnotu z identifikátoru URI, z textu žádosti nebo z vlastní vazby. Pro parametry, které pocházejí z identifikátoru URI chceme zkontrolujte, zda identifikátor URI ve skutečnosti obsahuje hodnotu pro tento parametr, v cestě (pomocí slovníku trasy) nebo v řetězci dotazu.

Zvažte například následující akce:

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

*Id* vazby parametru na identifikátor URI. Proto může tato akce odpovídá pouze identifikátor URI, který obsahuje hodnotu pro "id", buď ve slovníku trasy, nebo v řetězci dotazu.

Volitelné parametry jsou výjimku, protože jsou volitelné. Volitelný parametr je OK pokud vazby nelze získat hodnotu z identifikátoru URI.

Komplexní typy jsou výjimku různých důvodů. Komplexní typ lze vázat pouze k identifikátoru URI prostřednictvím vlastní vazby. Ale v takovém případě rozhraní nelze předem vědět, jestli by vazby parametru na konkrétní identifikátor URI. Pokud chcete zjistit, třeba, aby vyvolání vazby. Cílem výběr algoritmů je ze statické popis před vyvoláním žádné vazby. Vyberte akci. Komplexní typy jsou tedy vyloučeny z odpovídající algoritmus.

Po výběru se akce, volá se všechny vazby parametrů.

Shrnutí:

- Akce musí odpovídat metodu protokolu HTTP žádosti.
- Název akce musí odpovídat "action" položku ve slovníku trasy, pokud je k dispozici.
- Pro každý parametr akce Pokud parametr jsou převzaty z identifikátoru URI, pak název parametru je nutné nalézt ve slovníku trasy nebo v řetězci dotazu identifikátoru URI. (Volitelné parametry a parametry s komplexními typy nevylučují.)
- Pokuste se shodovat s největším počtem parametrů. Nejlepší shodu může být metoda bez parametrů.

## <a name="extended-example"></a>Rozšířené příklad

Postupy:

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

Kontroler:

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

Požadavek HTTP:

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a>Směrovat odpovídající

Identifikátor URI odpovídá trase s názvem "DefaultApi". Slovníku trasy obsahuje následující položky:

- Řadič: "produkty"
- ID: "1"

Slovníku trasy neobsahuje parametrů řetězce dotazu, "verze" a "Podrobnosti", ale bude stále charakter během výběr akce.

### <a name="controller-selection"></a>Výběr řadiče

Ze zadaného "controller" ve slovníku trasy typ kontroleru je `ProductsController`.

### <a name="action-selection"></a>Výběr akce

Požadavek HTTP je požadavek GET. Akce kontroleru, které podporují GET jsou `GetAll`, `GetById`, a `FindProductsByName`. Slovníku trasy neobsahuje položku pro "action", takže jsme nemusíte odpovídal názvu akce.

V dalším kroku pokusíme se porovnat názvy parametrů pro tyto akce jenom prohlížení akce GET.

| Akce | Parametry shody |
| --- | --- |
| `GetAll` | žádná |
| `GetById` | "id" |
| `FindProductsByName` | "název" |

Všimněte si, že *verze* parametr `GetById` se nepovažuje protože je volitelný parametr.

`GetAll` Metoda odpovídá trivially. `GetById` Také odpovídající metoda, protože obsahuje "id" slovníku trasy. `FindProductsByName` Metoda neodpovídá.

`GetById` Metoda služby wins, protože odpovídá jeden parametr versus žádné parametry pro `GetAll`. Metoda je volána s následující hodnoty parametrů:

- *ID* = 1
- *verze* = 1.5

Všimněte si, že i když *verze* nebyl použit v algoritmu výběr hodnota parametru pochází z řetězce dotazu identifikátoru URI.

## <a name="extension-points"></a>Rozšíření body

Webové rozhraní API poskytuje rozšíření body pro některé části procesu směrování.

| Rozhraní | Popis |
| --- | --- |
| **IHttpControllerSelector** | Vybere kontroler. |
| **IHttpControllerTypeResolver** | Získá seznam typů řadičů. **DefaultHttpControllerSelector** vybere typ kontroleru z tohoto seznamu. |
| **IAssembliesResolver** | Získá seznam sestavení projektu. **IHttpControllerTypeResolver** rozhraní používá tento seznam k vyhledání typů řadičů. |
| **IHttpControllerActivator** | Vytvoří nové instance kontroleru. |
| **IHttpActionSelector** | Vybere akci. |
| **IHttpActionInvoker** | Vyvolá akci. |

Pokud chcete zadat vlastní implementace pro některý z těchto rozhraní, použijte **služby** kolekce na **HttpConfiguration** objektu:

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
