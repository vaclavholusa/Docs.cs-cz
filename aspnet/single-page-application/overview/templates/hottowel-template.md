---
uid: single-page-application/overview/templates/hottowel-template
title: "Aktivní ručníků šablony | Microsoft Docs"
author: madskristensen
description: "HotTowel šablony"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/09/2013
ms.topic: article
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: bfc6e2c884c422f44e8be5f4f29554ae86f7ecb6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="hot-towel-template"></a>Aktivní ručníků šablony
====================
podle [Mads Kristensen](https://github.com/madskristensen)

> Aktivní šablony MVC ručníků jsou zapsána Papa Jan
> 
> Vyberte, kterou verzi ke stažení:
> 
> [Šablony MVC aktivní ručníků pro sadu Visual Studio 2012](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [Šablony MVC aktivní ručníků pro Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)


> Aktivní ručníků: Vzhledem k tomu, že nechcete, aby přejít na SPA bez jeden!


Chcete vytvářet SPA, ale nemůže rozhodnout, kde začít? Použít aktivní ručníků a v sekundách budete mít SPA a všechny nástroje, které potřebujete k vytváření na něm!

Aktivní ručníků vytvoří skvělý výchozí bod pro sestavení jedné stránce aplikace (SPA) s technologií ASP.NET. Ihned můžete poskytuje modulární struktura pro kód, zobrazení navigační, vazby dat, Správa bohaté dat a jednoduchý, ale elegantní stylů. Aktivní ručníků poskytuje vše, co potřebujete k vytvoření SPA, abyste se mohli zaměřit na aplikace, není vložení.

> Další informace o vytváření SPA z [Jan Papa videa, kurzy a kurzů Pluralsight](http://johnpapa.net/spa?vsix).


## <a name="application-structure"></a>Struktura aplikace

Aktivní ručníků SPA poskytuje aplikaci složku, která obsahuje soubory JavaScript a HTML, které definují aplikaci.

Ve složce aplikace:

- Durandal
- služby
- viewmodels
- zobrazení

Složky aplikace obsahuje kolekci modulů. Tyto moduly zapouzdření funkce a na dalších modulů deklarovat závislosti. Složka zobrazení HTML pro vaši aplikaci a složce viewmodels obsahuje prezentační logiku pro zobrazení (běžný vzor modelem MVVM). Složka služby je ideální pro nachází běžné služby, které vaše aplikace může být nutné například načítání dat protokolu HTTP nebo interakci s místní úložiště. Chcete-li znovu použít kód z modulů služby je běžné pro více viewmodels.

## <a name="aspnet-mvc-server-side-application-structure"></a>Struktury aplikace straně serveru ASP.NET MVC

Aktivní ručníků staví na strukturu známé a výkonné rozhraní ASP.NET MVC.

- Aplikace\_Start
- Obsah
- Kontrolery
- Modely
- Skripty
- zobrazení

## <a name="featured-libraries"></a>Vybrané knihovny

- ASP.NET MVC
- Rozhraní API pro ASP.NET Web
- ASP.NET – webové optimalizace - sdružování a minimalizace
- [Breeze.js](http://Breezejs.com) -Správa bohaté dat
- [Durandal.js](http://Durandaljs.com) -navigaci a zobrazení složení
- [Knockout.js](http://Knockoutjs.com) -datové vazby
- [Require.js](http://requirejs.org) -modularitu s AMD a optimalizace
- [Toastr.js](http://jpapa.me/c7toastr) -automaticky otevírané okno zprávy
- [Twitter Bootstrap](http://twitter.github.com/bootstrap/) – robustní stylu CSS

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a>Instalace pomocí šablony sady Visual Studio 2012 aktivní ručníků SPA

Aktivní ručníků je možné nainstalovat jako šablony sady Visual Studio 2012. Stačí kliknout na `File`  |  `New Project` a zvolte `ASP.NET MVC 4 Web Application`. Pak vyberte ' aktivní ručníků jedné stránky aplikace "šablony a spusťte!

## <a name="installing-via-the-nuget-package"></a>Instalace prostřednictvím balíčku NuGet

Aktivní ručníků je také balíčku NuGet, který rozšiřuje existujícího projektu ASP.NET MVC prázdný. Stačí nainstalovat pomocí nástroje Nuget a spusťte!

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a>Jak vytvářet na aktivní ručníků?

Jednoduše spustíte, přidání kódu!

1. Přidejte vlastní serverový kód, pokud možno Entity Framework a WebAPI (které skutečně Vylepšete s Breeze.js)
2. Přidat zobrazení, která `App/views` složky
3. Přidat viewmodels k `App/viewmodels` složky
4. Přidat kód HTML a Knockout vazby dat k novým zobrazením
5. Aktualizace tras navigace v`shell.js`

## <a name="walkthrough-of-the-htmljavascript"></a>Návod, HTML/JavaScript

### <a name="viewshottowelindexcshtml"></a>Views/HotTowel/index.cshtml

index.cshtml je výchozí trasy a zobrazení pro aplikaci MVC. Obsahuje všechny značky meta pro standardní, odkazy šablon stylů css a JavaScript odkazy, které očekáváte. Text obsahuje jediný `<div>` tedy, kde veškerý obsah (zobrazení) budou umístěné okamžiku vyžádání. `@Scripts.Render` Require.js používá ke spuštění vstupu bod pro kód aplikace, který je součástí `main.js` souboru. Úvodní obrazovka zajišťuje ukazují, jak vytvářet úvodní obrazovky při aplikace načte.

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a>App/Main.js

`main.js` Soubor obsahuje kód, který se spustí hned, jak vaše aplikace je načtena. Toto je, kde chcete definovat navigační trasy, nastavte vaše po spuštění zobrazení a provést všechny instalační program nebo zavádění například priming data aplikace.

`main.js` Soubor definuje několik durandal na moduly, které pomůžou akci aplikace spustit. Příkaz definovat pomůže vyřešit závislosti moduly, takže jsou k dispozici pro tuto funkci. Nejprve jsou povolené zprávy ladění, který odesílat zprávy o událostech, které aplikace provádí v okně konzoly. Kód app.start informuje durandal framework a spusťte aplikaci. Se názvů jsou nastavené tak, aby durandal zná všechna zobrazení a viewmodels jsou součástí stejné s názvem složky, v uvedeném pořadí. Nakonec `app.setRoot` zatížení se spustí `shell` pomocí předdefinované šablony `entrance` animace.

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a>zobrazení

Zobrazení jsou součástí `App/views` složky.

### <a name="shellhtml"></a>Shell.HTML

`shell.html` Obsahuje hlavní rozložení pro kódu HTML. Všechna vaše zobrazení se skládá někde v straně vaší `shell` zobrazení. Poskytuje aktivní ručníků `shell` s tři tyto oblasti: zápatí, záhlaví a oblast obsahu. Každý z těchto oblastí je načtena s dalšími zobrazeními vyžádání tvoří obsah.

`compose` Vazby pro záhlaví a zápatí se obtížně programového v aktivní ručníků tak, aby odkazoval `nav` a `footer` zobrazení, v uvedeném pořadí. Vytvářené vazby pro oddíl `#content` je vázána `router` modulu active položky. Jinými slovy po kliknutí na tlačítko je načtena navigační odkaz je odpovídající zobrazení v této oblasti.

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a>NAV.HTML

`nav.html` Obsahuje navigační odkazy zabezpečené ověřování HESLA. Toto je strukturu nabídky můžete umístění, například. Často je jím data vázaná (pomocí Knockout) k `router` modulu zobrazení navigace definované v `shell.js`. Knockout vypadá pro atributy data-bind a aby se váže `shell` viewmodel k zobrazení navigační trasy a zobrazit komponenta progressbar (pomocí služby Twitter Bootstrap) Pokud `router` modulu zrovna navigaci z jednoho zobrazení na jiné (viz `router.isNavigating`).

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a>Home.HTML a details.html

Tato zobrazení obsahovat HTML pro vlastní zobrazení. Když `home` na odkaz v `nav` zobrazení nabídky po kliknutí na, `home` zobrazení bude uložena v oblasti obsahu `shell` zobrazení. Tato zobrazení můžete rozšířit nebo nahradit vlastní zobrazení.

### <a name="footerhtml"></a>Footer.HTML

`footer.html` Obsahuje HTML, který se zobrazí v zápatí, v dolní části `shell` zobrazení.

## <a name="viewmodels"></a>ViewModels

ViewModels se nacházejí v `App/viewmodels` složky.

### <a name="shelljs"></a>Shell.js

`shell` Viewmodel obsahuje vlastnosti a funkce, které jsou vázány `shell` zobrazení. Často je jím kde se nacházejí vazby navigační nabídky (viz `router.mapNav` logiku).

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a>Home.js a details.js

Tyto viewmodels obsahovat vlastnosti a funkce, které jsou vázány `home` zobrazení. také obsahuje prezentační logiku pro zobrazení a je pojidlem mezi daty a zobrazení.

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a>Služby

Služby se nacházejí ve složce aplikace nebo služby. V ideálním případě vaše budoucí služby, jako je například dataservice modul, který zodpovídá za načítání a publikování vzdálených dat, může být umístěny.

### <a name="loggerjs"></a>Logger.js

Poskytuje aktivní ručníků `logger` modulu ve složce služby. `logger` Modul je ideální pro protokolování zpráv do konzoly a uživateli v překryvu informační zprávy.
