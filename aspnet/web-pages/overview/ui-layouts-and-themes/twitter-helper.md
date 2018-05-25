---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Pomocník s webovými stránkami ASP.NET Twitter | Microsoft Docs
author: tfitzmac
description: Toto téma a aplikace ukazují, jak přidat do projektu služby WebMatrix 3 pomocné rutiny služby Twitter. Obsahuje kód Pomocník Twitter a ukazuje způsob volání pomocné rutiny...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 07d9c4d485c42b78a42c54c9740af5f67cb44763
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/25/2018
---
<a name="twitter-helper-with-aspnet-web-pages"></a>Pomocník s webovými stránkami ASP.NET Twitter
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Toto téma a aplikace ukazují, jak přidat do projektu služby WebMatrix 3 pomocné rutiny služby Twitter. Obsahuje kód Pomocník Twitter a ukazuje způsob volání pomocné metody.
> 
> Tento kód pro soubor Twitter.cshtml vyvinula **Tian Panoramování** společnosti Microsoft.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Rozhraní ASP.NET Web Pages (Razor) 3
>   
> 
> V tomto kurzu taky spolupracuje se službou ASP.NET Web Pages 2.


## <a name="introduction"></a>Úvod

Toto téma ukazuje, jak přidat Twitter pomocné rutiny do vaší aplikace a používat syntaxi Razor k volání pomocné metody. Pomocník Twitter usnadňuje začlenění Twitter tlačítka a pomůcky ve vaší aplikaci. Chcete-li použít modul widget Twitter, jako je například časová osa uživatele nebo výsledky hledání hashtagu, musíte nejdřív vytvořit [pomůcky na Twitteru](https://twitter.com/settings/widgets). Po vytvoření vaší pomůcky, obdržíte id pomůcky. Toto id pomůcky předejte jako parametr při volání pomocné metody, které se zobrazí pomůcky.

Toto téma je určen pro verze 1.1 rozhraní API služby Twitter. Přidáním přímo kód Pomocník Twitter do projektu, můžete aktualizovat kód pomocného objektu, pokud se změní rozhraní API služby Twitter.

Informace o instalaci služby WebMatrix najdete v tématu [Představení technologie ASP.NET Web Pages 2 – Začínáme](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).

## <a name="add-twitter-helper-to-your-project"></a>Do projektu přidejte Pomocník Twitter

Pokud chcete přidat Pomocník Twitter, nejprve přidejte složku s názvem **aplikace\_kód** do projektu. Pak vytvořte soubor s názvem **Twitter.cshtml**.

![Složka App_Code](twitter-helper/_static/image1.png)

Ve výchozím kódu v Twitter.cshtml nahraďte následujícím kódem.

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a>Volání metody Twitter z webových stránek

Následující příklad ukazuje, jak používat služby Twitter pomocné metody ze stránky ve vašem projektu. V projektu můžete k nahrazení hodnoty parametrů s hodnotami, které jsou relevantní pro vaše potřeby. ID zadané pomůcky můžete prozkoumat fungování metody, ale budete chtít vygenerovat vlastní pomůcky pro váš projekt.

Ne všechny následující parametry jsou povinné. Volitelné parametry slouží k přizpůsobení zobrazení tlačítka nebo pomůcky. Například použijte tlačítko pouze vyžaduje uživatelské jméno pro postupujte podle, ale příklad ukazuje, jak chcete zahrnout počet délky a jak určit velikost tlačítko a jazyk.

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a>Zobrazit výsledky

Výše uvedený kód vytvoří následující tlačítka a pomůcky. Tato tlačítka a pomůcky jsou plně funkční, není snímky obrazovky. Použijte tlačítko se zobrazí ve španělštině, protože parametr language byla nastavena na **es**.

### <a name="follow-button"></a>Použijte tlačítko

[Postupujte podle @aspnet)](https://twitter.com/aspnet)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>

### <a name="tweet-button"></a>Tlačítko tweet

[Tweet](https://twitter.com/share)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>

### <a name="user-timeline-profile"></a>Časová osa uživatele (profil)

[Tweetů podle @aspnet](https://twitter.com/aspnet)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>

### <a name="favorites"></a>Oblíbené položky

[Pomocí Oblíbené Tweetů @Microsoft](https://twitter.com/Microsoft/favorites)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>

### <a name="list"></a>Seznam

[Tweetů z @Microsoft/MS \_příjemce\_pruhy](https://twitter.com/microsoft/ms-consumer-brands/)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>

### <a name="search"></a>Hledat

[Tweety o &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>
