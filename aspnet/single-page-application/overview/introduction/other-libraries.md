---
uid: single-page-application/overview/introduction/other-libraries
title: "Vědět, než Knockout knihovny? | Microsoft Docs"
author: madskristensen
description: "Vědět, než Knockout knihovny?"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/05/2013
ms.topic: article
ms.assetid: a8367c6d-ef94-4dff-a010-5eff9e6eea96
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/introduction/other-libraries
msc.type: authoredcontent
ms.openlocfilehash: 5a863f50401a4e2bab3f772374b7fd178f6c6cdf
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="know-a-library-other-than-knockout"></a>Vědět, než Knockout knihovny?
====================
podle [Mads Kristensen](https://github.com/madskristensen)

[Jedné stránky aplikace (SPA) šablony](knockoutjs-template.md) je skvělým způsobem, jak začít pracovat, zápis jednostránkové aplikace. Šablona používá [kódem KnockoutJS](http://knockoutjs.com/) k vytvoření vazby dat aplikací DOM elementy.

Ale Knockout není knihovnou JavaScript pouze pro vytváření plně funkčního klienta s aplikací. Další knihovny vyřešit problémy podobné různými způsoby. Možná budete chtít jedna knihovna oproti jinému, takže jsme provedli několik šablon komunity vytvořené k dispozici ke stažení. Každá z těchto šablon používá jinou kombinaci knihovny JavaScript klienta.

Instalace šablony vytvořené komunity, navštíví některý z šablony stránky uvedené níže a klikněte na tlačítko Stáhnout. Šablony jsou uvedeny jako soubory VSIX.

## <a name="backbonejs"></a>BackboneJS

[Šablona SPA Backbone.js](../templates/backbonejs-template.md). Tato šablona obsahuje počáteční kostru pro vývoj [Backbone.js](http://backbonejs.org/) aplikace v architektuře ASP.NET MVC. Ihned poskytuje funkce základní uživatel přihlášení, včetně resetování hesla registrace, přihlášení, uživatele a potvrzení uživatele s základní e-mailových šablon.

## <a name="breezejs"></a>BreezeJS

[BreezeJS](http://www.breezejs.com/?utm_source=ms-spa) otevřeným zdrojem knihovnu pro Správa bohaté dat v klientovi JavaScript. Uloženy zpracovává dotazování, ukládání do mezipaměti, sledování změn, ověření a další. Dvě šablony funkce uloženy:

- [Uloženy/Knockout](../templates/breezeknockout-template.md) šablony rozšiřuje šabloně Knockout SPA znázorňující, jak snadno můžete vytvořit jednostránkové aplikace s uloženy pro správu dat a kódem KnockoutJS pro datovou vazbu.
- [Uloženy/úhlová](../templates/breezeangular-template.md) šablony taky ji rozšiřuje na šabloně Knockout SPA s uloženy, ale pomocí [AngularJS](http://angularjs.org) knihovny pro datové vazby, vkládání závislostí a správu obrazovky.

Kromě toho [aktivní ručníků SPA šablony](../templates/hottowel-template.md) používá BreezeJS.

## <a name="emberjs"></a>EmberJS

[Šablona EmberJS SPA](../templates/emberjs-template.md). Tato šablona používá [členskými](http://emberjs.com/), výkonné knihovna MVC JavaScript, která řeší širokou škálu výzvy pro vytváření aplikací plně funkčního klienta.

Šablona členskými SPA je opětovná implementace Knockout SPA šablony, pomocí EmberJS a Řídítka kola ukázka.

## <a name="hot-towel"></a>Aktivní ručníků

[Aktivní šablony ručníků SPA](../templates/hottowel-template.md). Tato šablona se otevře v několika knihoven jazyka JavaScript, včetně uloženy, Knockout, RequireJS a Twitter Bootstrap.

Porovnání s další šablony zde uvedeny, aktivní ručníků teample poskytuje podrobnější aplikace, ze kterého můžete vytvořit vlastní. Existují další koncepty, které je mít na paměti, ale jakmile je pochopit, tato šablona může být co hledáte. Pokud chcete vytvořit SPA, ale nelze rozhodnete, kam chcete spustit, použijte aktivní ručníků a v sekundách budete mít SPA a všechny nástroje, musíte vytvořit na něm.

## <a name="feature-table"></a>Funkce tabulky

Zde jsou funkce poskytované službou každé šabloně SPA:

|  | ASP.NET SPA | Páteřní | Úhlová/uloženy | Uloženy/KO | Členskými | Aktivní ručníků |
| --- | --- | --- | --- | --- | --- | --- |
| Ukázka ToDo | &#10003; |  | &#10003; | &#10003; | &#10003; |  |
| Úplné šablony |  | &#10003; |  |  |  | &#10003; |
| Navigační a historie |  | &#10003; | &#10003; |  | &#10003; | &#10003; |
| Knihovnami |  |  |  |  |  |  |
| úhlová |  |  | &#10003; |  |  |  |
| &#8195; Páteřní |  | &#10003; |  |  |  |  |
| Uloženy |  |  | &#10003; | &#10003; |  | &#10003; |
| Durandal |  |  |  |  |  | &#10003; |
| Členskými |  |  |  |  | &#10003; |  |
| Knockout | &#10003; |  |  | &#10003; |  | &#10003; |
