---
title: "Úvod do ASP.NET Core"
author: rick-anderson
description: "Nabízí úvodní informace o ASP.NET Core."
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 09/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: index
ms.openlocfilehash: a075c63fddb9e28a1da37d4ef6647808a0dcb583
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="introduction-to-aspnet-core"></a>Úvod do ASP.NET Core

Autoři: [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) a [Shaun Luttin](https://twitter.com/dicshaunary)

ASP.NET Core je platformově univerzální, vysoce výkonná architektura typu [open-source](https://github.com/aspnet/home), která slouží k vytváření moderních cloudových aplikací připojených k internetu. S ASP.NET Core můžete:

* Vytvářet webové aplikace a služby, aplikace [IoT](https://www.microsoft.com/en-us/internet-of-things/) a mobilní back-endy.
* Používat oblíbené vývojářské nástroje v systémech Windows, Mac OS a Linux.
* Nasazovat v cloudu nebo místně.
* Používat ke spuštění [.NET Core nebo .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).

## <a name="why-use-aspnet-core"></a>Proč používat ASP.NET Core?

Miliony vývojářů si zvykli používat (a dále používají) ASP.NET k vytváření webových aplikací. ASP.NET Core je přepracovanou verzí ASP.NET se změněnou architekturou. Výsledkem je odlehčené modulární prostředí.

ASP.NET Core nabízí následující výhody:

* Jednotné prostředí pro vytváření webového uživatelského rozhraní a webových rozhraní API.
* Integrace [moderní architektury klienta](xref:client-side/index) a vývojových pracovních postupů.
* [Konfigurační systém](xref:fundamentals/configuration/index) založený na prostředí, který je připravený pro cloud.
* Integrovaná [injektáž závislostí](xref:fundamentals/dependency-injection).
* Odlehčený, vysoce výkonný, modulární kanál požadavků HTTP.
* Možnost hostování ve službě IIS nebo samoobslužné hostování ve vlastním procesu.
* Umožňuje spuštění v prostředí [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server), které podporuje opravdovou souběžnou správu různých verzí aplikace.
* Nabízí nástroje, které usnadňují vývoj moderních webů.
* Možnost kompilace a spuštění v systémech Windows, Mac OS a Linux.
* Architektura zaměřená na open-source a komunitu.

Architektura ASP.NET Core se celá dodává v balíčcích [NuGet](https://www.nuget.org/). To umožňuje optimalizovat aplikaci a zahrnout jen ty balíčky, které potřebujete. K výhodám menší plochy aplikace patří důkladnější zabezpečení, menší údržba a větší výkon.

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>Vytváření webových rozhraní API a webového uživatelského rozhraní v ASP.NET Core MVC

Architektura ASP.NET Core MVC nabízí funkce, které umožňují vytvářet [webová rozhraní API](xref:tutorials/index#building-web-apis) a [webové aplikace](xref:tutorials/index#building-web-applications):

* [Vzor Model-View-Controller (MVC)](xref:mvc/overview) přispívá k tomu, aby vaše webová rozhraní API a webové aplikace byly [testovatelné](testing/index.md).
* [Stránky Razor](xref:mvc/razor-pages/index) (nově ve verzi 2.0) představují stránkovaný programový model, se kterým je vytváření webového uživatelského rozhraní jednodušší a produktivnější.
* [Syntaxe Razor](xref:mvc/views/razor) poskytuje jazyk, který je produktivnější jak pro [stránky Razor](xref:mvc/razor-pages/index), tak pro [zobrazení MVC](xref:mvc/views/overview).
* [Pomocné rutiny značky](xref:mvc/views/tag-helpers/intro) umožňují, aby se kód na straně serveru v souborech Razor podílel na vytváření a vykreslování prvků HTML.
* Díky integrované podpoře [různých datových formátů a vyjednávání obsahu](mvc/models/formatting.md) mohou vaše webová rozhraní API používat různí klienti, včetně prohlížečů a mobilních zařízení.
* [Vazby modelu](xref:mvc/models/model-binding) automaticky mapují data požadavků HTTP na parametry metod akce.
* [Ověření modelu](xref:mvc/models/validation) automaticky ověří stranu klienta i stranu serveru.

## <a name="client-side-development"></a>Vývoj klientské strany

Architektura ASP.NET Core je navržená tak, aby bylo možné ji bez problémů integrovat do různých klientských rozhraní, včetně [AngularJS](xref:client-side/angular), [KnockoutJS](xref:client-side/knockout) a [Bootstrap](xref:client-side/bootstrap). Podrobnější informace najdete v části [Vývoj klientské strany](client-side/index.md).

## <a name="next-steps"></a>Další kroky

Další informace naleznete v následujících materiálech:

* [Kurzy k ASP.NET Core](xref:tutorials/index)
* [Základy ASP.NET Core](xref:fundamentals/index)
* [Týdenní přehled novinek v komunitě ASP.NET](https://live.asp.net/) se zabývá pokroky a plány týmu a nabízí nové blogové příspěvky a software třetích stran.
