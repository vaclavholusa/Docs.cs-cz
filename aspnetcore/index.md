---
title: Úvod do ASP.NET Core
author: rick-anderson
description: Nechte si představit ASP.NET Core, což je platformově univerzální, vysoce výkonná architektura typu open-source, která slouží k vytváření moderních cloudových aplikací připojených k internetu.
ms.author: riande
ms.date: 02/28/2018
ms.technology: aspnetcore
ms.topic: conceptual
uid: index
ms.openlocfilehash: 3b55390e23f538a298e9fe97c9678fe6841b818c
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272867"
---
# <a name="introduction-to-aspnet-core"></a>Úvod do ASP.NET Core

Autoři: [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) a [Shaun Luttin](https://twitter.com/dicshaunary)

ASP.NET Core je platformově univerzální, vysoce výkonná architektura typu [open-source](https://github.com/aspnet/home), která slouží k vytváření moderních cloudových aplikací připojených k internetu. S ASP.NET Core můžete:

* Vytvářet webové aplikace a služby, aplikace [IoT](https://www.microsoft.com/internet-of-things/) a mobilní back-endy.
* Používat oblíbené vývojářské nástroje v systémech Windows, Mac OS a Linux.
* Nasazovat v cloudu nebo místně
* Používat ke spuštění [.NET Core nebo .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).

## <a name="why-use-aspnet-core"></a>Proč používat ASP.NET Core?

Miliony vývojářů už dříve k vytváření webových aplikací používali [ASP.NET 4.x](https://docs.microsoft.com/aspnet/overview) (což bude platit i nadále). ASP.NET Core je přepracovanou verzí ASP.NET 4.x se změněnou architekturou. Výsledkem je odlehčené a modulárnější prostředí.

ASP.NET Core nabízí následující výhody:

* Jednotné prostředí pro vytváření webového uživatelského rozhraní a webových rozhraní API.
* Integrace [moderní architektury klienta](xref:client-side/index) a vývojových pracovních postupů
* [Konfigurační systém](xref:fundamentals/configuration/index) založený na prostředí, který je připravený pro cloud.
* Integrovaná [injektáž závislostí](xref:fundamentals/dependency-injection).
* Odlehčený, [vysoce výkonný](https://github.com/aspnet/benchmarks), modulární kanál požadavků HTTP
* Možnost hostování ve službě [IIS](xref:host-and-deploy/iis/index), [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), [Docker](xref:host-and-deploy/docker/index) nebo samoobslužné hostování ve vlastním procesu
* Souběžná správa různých verzí aplikace, když cílovou platformou je [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)
* Nabízí nástroje, které usnadňují vývoj moderních webů.
* Možnost kompilace a spuštění v systémech Windows, Mac OS a Linux.
* Architektura zaměřená na open-source a [komunitu](https://live.asp.net/)

Architektura ASP.NET Core se celá dodává v balíčcích [NuGet](https://www.nuget.org/). Pomocí balíčků NuGet můžete svou aplikaci optimalizovat tak, aby obsahovala jen nezbytné závislosti. Ve skutečnosti vyžadují aplikace ASP.NET Core 2.x, jejichž cílem je .NET Core, jen [jeden balíček NuGet](xref:fundamentals/metapackage). K výhodám menší plochy aplikace patří důkladnější zabezpečení, menší údržba a větší výkon.

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>Vytváření webových rozhraní API a webového uživatelského rozhraní v ASP.NET Core MVC

Architektura ASP.NET Core MVC nabízí funkce umožňující vytvářet [webová rozhraní API](xref:tutorials/index#build-web-apis) a [webové aplikace](xref:tutorials/index#build-web-apps):

* [Vzor Model-View-Controller (MVC)](xref:mvc/overview) přispívá k tomu, aby vaše webová rozhraní API a webové aplikace byly [testovatelné](xref:test/index).
* [Stránky Razor](xref:razor-pages/index) (nově ve verzi ASP.NET Core 2.0) představují stránkovaný programový model, se kterým je vytváření webového uživatelského rozhraní jednodušší a produktivnější.
* [Kód Razor](xref:mvc/views/razor) poskytuje produktivní syntaxi jak pro [stránky Razor](xref:razor-pages/index), tak pro [zobrazení MVC](xref:mvc/views/overview).
* [Pomocné rutiny značky](xref:mvc/views/tag-helpers/intro) umožňují, aby se kód na straně serveru v souborech Razor podílel na vytváření a vykreslování prvků HTML.
* Díky integrované podpoře [různých datových formátů a vyjednávání obsahu](xref:web-api/advanced/formatting) mohou vaše webová rozhraní API používat různí klienti, včetně prohlížečů a mobilních zařízení.
* [Vazby modelu](xref:mvc/models/model-binding) automaticky mapují data požadavků HTTP na parametry metod akce.
* [Ověření modelu](xref:mvc/models/validation) automaticky ověří stranu klienta i stranu serveru.

## <a name="client-side-development"></a>Vývoj klientské strany

Architektura ASP.NET Core se hladce integruje do oblíbených klientských rozhraní a knihoven, včetně [Angular](xref:spa/angular), [React](xref:spa/react) a [Bootstrap](xref:client-side/bootstrap). Další informace najdete v článku [Vývoj na straně klienta](xref:client-side/index).

## <a name="aspnet-core-targeting-net-framework"></a>Cílení ASP.NET Core na .NET Framework

Cílem ASP.NET Core může být .NET Core nebo .NET Framework. Aplikace ASP.NET Core, jejichž cílem je .NET Framework, nejsou multiplatformní, ale běží jen ve Windows. V ASP.NET Core neplánujeme odebrat podporu pro cílení na .NET Framework. ASP.NET Core se obecně skládá z knihoven [.NET Standard](/dotnet/standard/net-standard). Aplikace vytvořené pomocí .NET Standard 2.0 běží všude, kde se podporuje .NET Standard 2.0.

Cílení na .NET Core má několik výhod, které přibývají s každou vydanou verzí. Mezi výhody .NET Core oproti .NET Framework patří:

* Mutliplatformní: Běží v systémech macOS, Linux a Windows.
* Vyšší výkon
* Souběžná správa verzí
* Nová rozhraní API
* Open source

Ze všech sil se snažíme doplnit rozhraní API z .NET Framework do .NET Core. Sada [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) zpřístupnila v .NET Core tisíce rozhraní API, která byla určena jen pro Windows. Tato rozhraní API nebyla v .NET Core 1.x dostupná.

## <a name="next-steps"></a>Další kroky

Další informace naleznete v následujících materiálech:

* [Začínáme se stránkami Razor](xref:tutorials/razor-pages/razor-pages-start)
* [Kurzy k ASP.NET Core](xref:tutorials/index)
* [Základy ASP.NET Core](xref:fundamentals/index)
* [Týdenní přehled novinek v komunitě ASP.NET](https://live.asp.net/) se zabývá pokrokem v týmových projektech a týmovými plány. Nabízí nové blogové příspěvky a software třetích stran.
