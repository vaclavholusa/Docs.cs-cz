---
uid: single-page-application/overview/introduction/other-libraries
title: Znáte jiné knihovny než Knockout? | Dokumenty Microsoft
author: madskristensen
description: Znáte jiné knihovny než Knockout?
ms.author: riande
ms.date: 02/05/2013
ms.assetid: a8367c6d-ef94-4dff-a010-5eff9e6eea96
msc.legacyurl: /single-page-application/overview/introduction/other-libraries
msc.type: authoredcontent
ms.openlocfilehash: 53c97580b45bb40a6c3256c8038ec5c8b861b69f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754568"
---
<a name="know-a-library-other-than-knockout"></a>Znáte jiné knihovny než Knockout?
====================
podle [Autor: Mads Kristensen](https://github.com/madskristensen)

[Jedné stránky aplikace (SPA) šablony](knockoutjs-template.md) je skvělý způsob, jak začít psát jednostránkové aplikace. Šablona používá [KnockoutJS](http://knockoutjs.com/) svázat data aplikací na elementy modelu DOM.

Ale Knockout není pouze knihovny jazyka JavaScript pro vytváření plně funkčního klienta s aplikací. Další knihovny řešit podobné problémy různými způsoby. Jedna knihovna dát přednost před jiným, abyste provedli jsme několik komunitou vytvořených šablon k dispozici ke stažení. Každá z těchto šablon používá různou kombinaci klientské knihovny pro JavaScript.

K instalaci šablonu vytvořenou komunitou, navštíví některý z šablony stránky uvedených níže a klikněte na tlačítko Stáhnout. Šablony jsou k dispozici jako soubory VSIX.

## <a name="backbonejs"></a>BackboneJS

[Šablona SPA Backbone.js](../templates/backbonejs-template.md). Tato šablona obsahuje počáteční skeleton pro vývoj [Backbone.js](http://backbonejs.org/) aplikace v ASP.NET MVC. Ihned poskytuje funkce základní uživatel přihlášení, včetně resetování hesla registrace, přihlášení, uživatele a potvrzení uživatele s základní e-mailové šablony.

## <a name="breezejs"></a>BreezeJS

[BreezeJS](http://www.breezejs.com/?utm_source=ms-spa) je open source knihovna pro správu velké množství dat na klientovi JavaScript. Rychlé zpracovává dotazování, ukládání do mezipaměti, sledování změn, ověřování a další. Dvě šablony funkce Breeze:

- [Breeze/Knockout](../templates/breezeknockout-template.md) šablony rozšiřuje šabloně Knockout SPA znázorňující, jak snadno můžete vytvořit jednostránkové aplikace s podrobným pro KnockoutJS a správy dat pro datovou vazbu.
- [Breeze/Angular](../templates/breezeangular-template.md) šablony také rozšiřuje šablon Knockout SPA s podrobným, ale používat [AngularJS](http://angularjs.org) knihovny pro správu obrazovky, vkládání závislostí a datové vazby.

Kromě toho [šablona Hot Towel SPA](../templates/hottowel-template.md) používá BreezeJS.

## <a name="emberjs"></a>EmberJS

[Šablona EmberJS SPA](../templates/emberjs-template.md). Tato šablona používá [členskými](http://emberjs.com/), výkonné knihovny MVC JavaScript, která řeší širokou škálu výzev pro vytváření aplikací vzhled plně funkčního klienta.

Šablona členskými SPA je opětovná implementace Knockout SPA šablony, pomocí EmberJS a Handlebars šablon.

## <a name="hot-towel"></a>Hot Towel

[Horké šablonu ručníků SPA](../templates/hottowel-template.md). Tato šablona přináší několik knihoven jazyka JavaScript, včetně Breeze, Knockout, RequireJS a architekturu Twitter Bootstrap.

Ve srovnání s pomocí šablon, které tady najdete, teample Hot Towel poskytuje úplnější aplikace, ze kterého můžete vytvářet vlastní. Existují další koncepty zajímat, ale jakmile je pochopit, tato šablona může být co jste hledali. Pokud chcete vytvořit SPA ale nemůže rozhodnutí, kde začít, použijte Hot Towel a během několika sekund bude nutné aplikace SPA a všechny nástroje musíte dále to rozvíjet.

## <a name="feature-table"></a>Funkce tabulky

Tady jsou funkcí poskytovaných službou každou šablonu jednostránková aplikace:


|                        | ASP.NET SPA | Páteřní | Breeze/Angular | Breeze/KO |  Členskými   | Hot Towel |
|------------------------|-------------|----------|----------------|-----------|----------|-----------|
|      Ukázka ToDo       |  &#10003;   |          |    &#10003;    | &#10003;  | &#10003; |           |
|     Úplné šablony      |             | &#10003; |                |           |          | &#10003;  |
| Navigace a historie |             | &#10003; |    &#10003;    |           | &#10003; | &#10003;  |
|        Knihovnami        |             |          |                |           |          |           |
|        Angular         |             |          |    &#10003;    |           |          |           |
|    &#8195;Páteřní     |             | &#10003; |                |           |          |           |
|         Rychlé         |             |          |    &#10003;    | &#10003;  |          | &#10003;  |
|        Durandal        |             |          |                |           |          | &#10003;  |
|         Členskými          |             |          |                |           | &#10003; |           |
|        Knockout        |  &#10003;   |          |                | &#10003;  |          | &#10003;  |

