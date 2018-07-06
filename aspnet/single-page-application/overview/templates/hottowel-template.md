---
uid: single-page-application/overview/templates/hottowel-template
title: Šablona Hot Towel | Dokumentace Microsoftu
author: madskristensen
description: HotTowel šablony
ms.author: aspnetcontent
ms.date: 02/09/2013
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: 6145a34286ef113002b92d722e227005657098d9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825134"
---
<a name="hot-towel-template"></a>Šablona Hot Towel
====================
podle [Autor: Mads Kristensen](https://github.com/madskristensen)

> John Papa zapisuje horké šablonu ručníků MVC
> 
> Vyberte, kterou verzi ke stažení:
> 
> [Šablona Hot Towel MVC pro sadu Visual Studio 2012](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [Šablona Hot Towel MVC pro Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> Hot Towel: Vzhledem k tomu, že nechcete přejděte do aplikace SPA bez jediného!


Chcete sestavit SPA, ale nelze rozhodnout, kde začít? Použít Hot Towel a během několika sekund budete mít SPA a všechny nástroje, které potřebujete k vytváření na ní!

Hot Towel vytvoří skvělý výchozí bod pro vytváření jedné stránce aplikace (SPA) pomocí technologie ASP.NET. Okamžité vám nabízí modulární struktura kódu, navigace zobrazení, vytváření datových vazeb, správu velké množství dat a používání stylů pro jednoduché, ale elegantní. Hot Towel nabízí vše, co potřebujete k vytváření jednostránková aplikace, abyste se mohli soustředit na svou aplikaci, nikoli zajistí funkčnost systému.

> Další informace o vytváření SPA z [John Papa videa, kurzy a kurzů Pluralsight](http://johnpapa.net/spa?vsix).


## <a name="application-structure"></a>Struktury aplikace

Hot Towel SPA obsahuje složku aplikace, který obsahuje soubory jazyka JavaScript a HTML, které definují aplikaci.

Ve složce aplikace:

- Durandal
- služby
- modely viewmodels
- zobrazení

Složky aplikace obsahuje kolekci modulů. Tyto moduly zapouzdřují funkce a deklaraci závislosti na dalších modulech. Zobrazení složky obsahuje kód HTML pro vaši aplikaci a modely viewmodels složka obsahuje prezentaci logiku pro zobrazení (běžný vzor MVVM). Složka služby je ideální pro bydlení žádné společné služby, které vaše aplikace může potřebovat například načítání dat protokolu HTTP nebo místní úložiště interakce. Je běžné, že více modely viewmodels opětovné použití kódu z modulů služby.

## <a name="aspnet-mvc-server-side-application-structure"></a>Struktury aplikace na straně serveru ASP.NET MVC

Hot Towel staví na zkušenosti a výkonné strukturu ASP.NET MVC.

- Aplikace\_Start
- Obsah
- Kontrolery
- Modely
- Skripty
- Zobrazení

## <a name="featured-libraries"></a>Vybrané knihovny

- ASP.NET MVC
- Rozhraní API pro ASP.NET Web
- Optimalizace prostředí ASP.NET – sdružování a minifikace
- [Breeze.js](http://Breezejs.com) -velké množství dat správy
- [Durandal.js](http://Durandaljs.com) – procházení a zobrazení sestavení
- [Rozhraní Knockout.js](http://Knockoutjs.com) – datové vazby
- [Require.js](http://requirejs.org) -modularitu AMD a optimalizace
- [Toastr.js](http://jpapa.me/c7toastr) – místní zprávy
- [Twitter Bootstrap](http://twitter.github.com/bootstrap/) – robustní stylu CSS

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a>Instalace přes SPA šablona Hot Towel Visual Studio 2012

Hot Towel je možné nainstalovat jako šablony sady Visual Studio 2012. Stačí kliknout na `File`  |  `New Project` a zvolte `ASP.NET MVC 4 Web Application`. Vyberte "Hot Towel jednostránkové aplikace" šablony a spusťte!

## <a name="installing-via-the-nuget-package"></a>Instalace prostřednictvím balíčku NuGet.

Hot Towel je také balíček NuGet, která rozšiřuje stávající prázdný projekt ASP.NET MVC. Stačí nainstalovat pomocí nástroje Nuget a spusťte!

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a>Jak vytvořit na Hot Towel

Začněte jednoduše přidávat kód!

1. Přidejte vlastní kód na straně serveru, pokud možno Entity Framework a WebAPI (což ve skutečnosti lesku pomocí Breeze.js)
2. Přidat zobrazení `App/views` složky
3. Přidat modely viewmodels k `App/viewmodels` složky
4. Přidání kódu HTML a Knockout datové vazby k novým zobrazením
5. Aktualizace tras navigace v `shell.js`

## <a name="walkthrough-of-the-htmljavascript"></a>Návod, HTML/JavaScript

### <a name="viewshottowelindexcshtml"></a>Views/HotTowel/index.cshtml

index.cshtml je výchozí trasy a zobrazení pro aplikaci MVC. Obsahuje všechny značky meta pro standardní, odkazy šablon stylů css a JavaScript odkazy, které očekáváte. Text obsahuje jediný `<div>` což je, kde veškerý obsah (zobrazení) budou umístěné při jsou požadovány. `@Scripts.Render` Používá ke spuštění úvodní bod pro kód aplikace, která je obsažena v Require.js `main.js` souboru. Úvodní obrazovka dostane ukazují, jak vytvořit úvodní obrazovky při načtení aplikace.

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a>App/main.js

`main.js` Soubor obsahuje kód, který se spustí poté, co vaše aplikace je načtena. Je to, kde chcete definovat navigace trasy, nastavte vaše spuštění zobrazení a provést libovolné instalační/spuštění například Příprava dat vaší aplikace.

`main.js` Soubor definuje několik modulů durandal ke spuštění aplikace spustit. Příkaz definovat pomáhá vyřešit závislosti modulů, aby byly k dispozici pro funkci. Nejdříve jsou povoleny zprávy ladění, které odesílat zprávy o událostech, že aplikace provádí v okně konzoly. Kód app.start říká rozhraní framework durandal ke spuštění aplikace. Konvence jsou nastavené tak, aby durandal pozná, všechna zobrazení a modely viewmodels jsou obsaženy ve stejné složce s názvem. Nakonec `app.setRoot` Marku zatížení `shell` pomocí předdefinované šablony `entrance` animace.

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a>Zobrazení

Zobrazení jsou součástí `App/views` složky.

### <a name="shellhtml"></a>shell.html

`shell.html` Obsahuje hlavní rozložení pro kódu HTML. Všechny ostatní zobrazení se skládá někde v části vašeho `shell` zobrazení. Poskytuje Hot Towel `shell` tři tyto oblasti: zápatí, záhlaví a oblast obsahu. Každá z těchto oblastí je načtena s tvoří jiných zobrazení při požadavku na obsah.

`compose` Vazby pro záhlaví a zápatí nich není pevně nastavená v Hot Towel přejděte `nav` a `footer` zobrazení, v uvedeném pořadí. Vytvořit vazbu pro oddíl `#content` je vázán na `router` modulu aktivní položky. Jinými slovy po kliknutí na navigační odkaz je odpovídající zobrazení je načten v této oblasti.

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a>nav.html

`nav.html` Obsahuje odkazy pro tato jednostránková aplikace. To je, kde struktura nabídky je možné použít, třeba. Často je jím data vázaná (pomocí Knockout) k `router` modul zobrazení navigace, kterou jste definovali v `shell.js`. Knockout vyhledá data-bind atributy a aby vazba `shell` viewmodel, aby se zobrazovaly trasy navigace a chcete-li zobrazit ukazatel průběhu (pomocí architekturu Twitter Bootstrap) Pokud `router` modul provádí navigaci z jednoho zobrazení do jiného (viz `router.isNavigating`).

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a>Home.HTML a details.html

Tato zobrazení obsahují kód jazyka HTML pro vlastní zobrazení. Když `home` odkaz v `nav` dojde ke kliknutí na nabídky zobrazení, `home` zobrazení se umístí do oblasti obsahu `shell` zobrazení. Tato zobrazení můžete rozšířit nebo nahradit vlastní zobrazení.

### <a name="footerhtml"></a>footer.html

`footer.html` Obsahuje kód HTML, který se zobrazí v zápatí, v dolní části `shell` zobrazení.

## <a name="viewmodels"></a>Modely ViewModels

Modely ViewModel se nacházejí v `App/viewmodels` složky.

### <a name="shelljs"></a>shell.js

`shell` Viewmodel obsahuje vlastnosti a funkce, které jsou vázány `shell` zobrazení. Často je jím nalezených vazby navigační nabídce (viz `router.mapNav` logiky).

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a>Home.js a details.js

Tyto modely viewmodels obsahovat vlastností a funkcí, které jsou vázány `home` zobrazení. také obsahuje prezentaci logiku pro zobrazení a je skutečným pojidlem mezi daty a zobrazení.

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a>Služby

Služby se nacházejí ve složce aplikace nebo služeb Team Foundation. V ideálním případě můžete umístit vaše budoucí služby jako službě dataservice modul, který je zodpovědný za načtení a publikování vzdálených dat.

### <a name="loggerjs"></a>logger.js

Poskytuje Hot Towel `logger` modulu ve složce služby. `logger` Modul je ideální pro protokolování zpráv do konzoly a pro uživatele v místní informační zprávy.
