---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Úvod do kurzu NerdDinner | Dokumentace Microsoftu
author: shanselman
description: Nejlepší způsob, jak další nové rozhraní je s ním něco sestavení. Tento kurz vás provede postupem vytvoření malé, ale dokončení aplikace pomocí konfigurace ASP.NE...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: d5efab525841b5c526aa3b656f27b1c42cc74648
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753236"
---
<a name="introducing-the-nerddinner-tutorial"></a>Úvod do kurzu NerdDinner
====================
podle [Scott Hanselman](https://github.com/shanselman)

[Stáhnout PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Nejlepší způsob, jak další nové rozhraní je s ním něco sestavení. Tento kurz vás provede postupy vytvoření malé, ale dokončení aplikace pomocí ASP.NET MVC 1 a představuje některé základními koncepcemi jeho fungování.
> 
> Pokud používáte ASP.NET MVC 3, doporučujeme je provést [získávání začít s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.


## <a name="nerddinner-tutorial"></a>Do kurzu NerdDinner

Nejlepší způsob, jak další nové rozhraní je s ním něco sestavení. Tento kurz vás provede postupy vytvoření malé, ale dokončení aplikace pomocí ASP.NET MVC a zavádí některé základními koncepcemi jeho fungování.

Aplikace, kterou budeme vytvářet se nazývá "NerdDinner". NerdDinner poskytuje snadný způsob uživatelům najít a uspořádat večeří online:

![](introducing-the-nerddinner-tutorial/_static/image1.png)

NerdDinner umožňuje vytvářet, upravovat a odstraňovat večeří registrovaných uživatelů. Vynucuje konzistentní sadu pravidel ověřování a obchodní napříč aplikací:

![](introducing-the-nerddinner-tutorial/_static/image2.png)

Návštěvníci můžete hledat nadcházející večeří se nachází v jejich mapování na základě AJAX:

![](introducing-the-nerddinner-tutorial/_static/image3.png)

Kliknutím webu dinner přejdou na stránku podrobností kde lze získat informace Další informace:

![](introducing-the-nerddinner-tutorial/_static/image4.png)

Pokud se zúčastnili dinner můžou přihlásit nebo zaregistrovat na webu:

![](introducing-the-nerddinner-tutorial/_static/image5.png)

Můžete pak kliknout na odkaz na reakce na základě jazyka AJAX k Zúčastněte se akce:

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a>Implementace NerdDinner

Budeme zahájíte naší aplikace NerdDinner pomocí souboru -&gt;příkaz Nový projekt v sadě Visual Studio k vytvoření zcela nového projektu ASP.NET MVC. Pak postupně přidáme funkce a prvky. Na cestě probereme:

1. [Jak vytvořit nový projekt ASP.NET MVC](# "vytvořte nový projekt ASP.NET MVC")
2. [Jak vytvořit databázi](# "vytvoření databáze")
3. [Jak vytvořit model s ověřením obchodních pravidel](# "sestavení modelu s ověřením obchodních pravidel")
4. [Použití kontrolerů a zobrazení seznamu a podrobností uživatelského rozhraní implementovat](# "použití Kontrolerů a zobrazení k implementaci uživatelského rozhraní seznamu a podrobností")
5. [Postup zajištění akcí CRUD (vytváření, čtení, aktualizace nebo odstranění) podporujících zápis dat do formuláře](# "poskytují CRUD (vytváření, čtení, Update, Delete) Data formuláře položky podporu")
6. [Použití slovníku ViewData a implementace tříd ViewModel](# "použití slovníku ViewData a implementace tříd ViewModel")
7. [Jak znovu pomocí uživatelského rozhraní pomocí stránek předloh a částečných zobrazení](# "stránek opakované použití uživatelského rozhraní předloh a částečných zobrazení")
8. [Implementace efektivního stránkování dat](# "implementovat efektivní Data stránkování")
9. [Zabezpečení aplikací ověřováním a autorizací](# "zabezpečené aplikace pomocí ověřování a autorizace")
10. [Použití jazyka AJAX k dynamickým aktualizacím](# "použití jazyka AJAX k doručení dynamických aktualizací")
11. [Použití jazyka AJAX k implementaci scénářů mapování](# "použití jazyka AJAX k implementaci scénářů mapování")
12. [Jak povolit automatické testování částí](# "povolte automatizované testování částí")

Můžete vytvořit svoji vlastní kopii NerdDinner úplně od začátku po dokončení každého kroku jsme názorný postup v této kapitole. Alternativně můžete stáhnout úplnou verzi zdrojového kódu: [NerdDinner na Githubu](https://github.com/AspNetMVPSamples/NerdDinner). Můžete také v případě potřeby také [stáhnout zdarma PDF verzi tohoto kurzu](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) Pokud si chcete přečíst kurz v režimu offline.

Visual Studio 2008 nebo na bezplatné Visual Web Developer 2008 Express můžete použít k sestavení aplikace. Pro databázi můžete použít SQL Server nebo bezplatný systém SQL Server Express.

ASP.NET MVC, Visual Web Developer 2008 Express a SQL Server Express (vše zdarma) pomocí verzí V2 můžete nainstalovat [instalačního programu webové platformy](https://www.microsoft.com/web/downloads/platform.aspx)

### <a name="now-lets-get-started"></a>Nyní můžeme začít...

Teď, když jsme si popsali, co je NerdDinner, Pojďme vyhrňte rukávy naše a napsání kódu.

Začneme budete pomocí souboru -&gt;nový projekt v sadě Visual Studio k vytvoření aplikace NerdDinner.

> [!div class="step-by-step"]
> [Next](create-a-new-aspnet-mvc-project.md)
