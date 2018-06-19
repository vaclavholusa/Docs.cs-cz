---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Představujeme v kurzu NerdDinner | Microsoft Docs
author: shanselman
description: Nejlepší způsob, jak další nové architektury je sestavení něco s ním. Tento kurz vás provede postup vytvoření malé, ale dokončení aplikace pomocí ASP.NE...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 3d925a7dc89fc0c742468653c5c138a0f1d71231
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868552"
---
<a name="introducing-the-nerddinner-tutorial"></a>Představujeme v kurzu NerdDinner
====================
podle [Scott Hanselman](https://github.com/shanselman)

[Stáhnout PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Nejlepší způsob, jak další nové architektury je sestavení něco s ním. V tomto kurzu provede postup sestavení malá, ale dokončení aplikace pomocí ASP.NET MVC 1 a zavádí některé základní koncepty za ní.
> 
> Pokud používáte ASP.NET MVC 3, doporučujeme provedením [získávání spuštěna s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Hudba úložiště](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.


## <a name="nerddinner-tutorial"></a>Kurz NerdDinner

Nejlepší způsob, jak další nové architektury je sestavení něco s ním. V tomto kurzu provede postup sestavení malá, ale dokončení aplikace pomocí rozhraní ASP.NET MVC a zavádí některé základní koncepty za ní.

Aplikace, kterou přidáme sestavení se nazývá "NerdDinner". NerdDinner poskytuje snadný způsob pro osoby k vyhledání a uspořádání večeří online:

![](introducing-the-nerddinner-tutorial/_static/image1.png)

NerdDinner umožňuje vytvářet, upravovat a odstraňovat večeří registrovaní uživatelé. Vynucuje sadu pravidel ověřování a obchodní konzistentní napříč aplikace:

![](introducing-the-nerddinner-tutorial/_static/image2.png)

Návštěvníky můžete použít mapování na základě AJAX pro vyhledání nadcházející večeří teď téměř je:

![](introducing-the-nerddinner-tutorial/_static/image3.png)

Kliknutím na večeři přejdou na stránce s podrobnostmi o kde můžete zjistit další informace:

![](introducing-the-nerddinner-tutorial/_static/image4.png)

Pokud jsou zajímat, kteří se účastní večeři můžou přihlásit nebo zaregistrovat na webu:

![](introducing-the-nerddinner-tutorial/_static/image5.png)

Můžete pak klikněte na odkaz na základě AJAX zasílání zpráv rysy k účasti na událost:

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a>Implementace NerdDinner

Přidáme zahájíte NerdDinner aplikace pomocí souboru -&gt;příkaz Nový projekt v sadě Visual Studio k vytvoření nové projektu ASP.NET MVC. Potom postupně přidáme funkce a funkce. Na této cestě jsme se zabývat těmito tématy:

1. [Postup vytvoření nového projektu ASP.NET MVC](# "vytvořte nový projekt ASP.NET MVC")
2. [Postup vytvoření databáze](# "vytvoření databáze")
3. [Jak vytvořit model se obchodní pravidla ověření](# "sestavení Model se obchodní pravidla ověření")
4. [Postup implementace výpis/podrobnosti uživatelského rozhraní pomocí řadiče a zobrazení](# "pomocí řadiče a zobrazení implementovat výpis/podrobnosti uživatelského rozhraní")
5. [Jak poskytnout CRUD (vytvořit, číst, aktualizovat, odstraňovat) data tvoří položka podporu](# "poskytují CRUD (vytvořit, číst, Update, Delete) Data formuláře Položka podporu")
6. [Jak používat ViewData a implementaci třídy ViewModel](# "použít ViewData a implementovat ViewModel třídy")
7. [Jak znovu pomocí uživatelského rozhraní pomocí hlavní stránky a částečné](# "znovu pomocí uživatelského rozhraní pomocí hlavní stránky a částečné.")
8. [Postupy pro implementaci stránkování efektivní dat](# "implementovat efektivní Data stránkování")
9. [Jak zabezpečit aplikace s použitím ověřování a autorizace](# "zabezpečené aplikace pomocí ověřování a autorizace")
10. [Jak používat k poskytování dynamické aktualizace AJAX](# "použít AJAX poskytovat dynamické aktualizace")
11. [Jak používat AJAX implementovat scénáře mapování](# "použít AJAX implementovat scénáře mapování")
12. [Postup povolení automatizované testování částí](# "povolit automatizované testování částí")

Můžete vytvořit svoji vlastní kopii NerdDinner od začátku pomocí každý krok jsme návod v této kapitole. Alternativně můžete stáhnout dokončenou verzi zdrojový kód: [NerdDinner na Githubu](https://github.com/AspNetMVPSamples/NerdDinner). Můžete také volitelně také [volné PDF verzi v tomto kurzu Stáhnout](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) Pokud si chcete přečíst kurz do offline režimu.

Visual Studio 2008 nebo volné Visual Web Developer 2008 Express slouží k sestavení aplikace. SQL Server nebo volné Server SQL Express můžete použít pro databázi.

Můžete nainstalovat rozhraní ASP.NET MVC, Visual Web Developer 2008 Express a SQL Server Express (všechny zdarma) pomocí V2 z [instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)

### <a name="now-lets-get-started"></a>Nyní můžeme začít...

Teď, když jsme jste popsané, co je NerdDinner, můžeme souhrnné naše rukávy a napsat kód.

Budete Začneme s použitím souboru -&gt;nový projekt v sadě Visual Studio k vytvoření aplikace NerdDinner.

> [!div class="step-by-step"]
> [Next](create-a-new-aspnet-mvc-project.md)
