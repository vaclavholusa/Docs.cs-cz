---
title: Verze kompatibility pro ASP.NET Core MVC
author: rick-anderson
description: Zjistěte, jak třídu pro spuštění v ASP.NET Core konfiguruje služby a kanál žádosti o aplikace.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/20/2018
uid: mvc/compatibility-version
ms.openlocfilehash: bedeeba07dcca19176e779f3541c445e94efcd07
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/22/2018
ms.locfileid: "41910021"
---
# <a name="compatibility-version-for-aspnet-core-mvc"></a>Verze kompatibility pro ASP.NET Core MVC

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

<xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> Metoda umožňuje aplikacím vyjádřit výslovný souhlas nebo výslovný nesouhlas s potenciálně rozbíjející změny chování zavedení v ASP.NET Core MVC 2.1 nebo novější. Potenciálně rozbíjející změny chování jsou obecně v způsob, jakým se chová subsystému MVC a jak **kódu** je volána modulem runtime. Vyjádření výslovného souhlasu, získáte nejnovější chování a dlouhodobé chování ASP.NET Core.

Následující kód nastaví režim kompatibility ASP.NET Core 2.1:

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup.cs?name=snippet1)]

Doporučujeme, abyste testování aplikace pomocí nejnovější verze (`CompatibilityVersion.Version_2_1`). Předpokládáme, že většina aplikací nebudou mít nejnovější změny chování pomocí nejnovější verze.

Aplikace, které volají `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` chráněná před potenciálně rozbíjející změny chování zavedené v ASP.NET Core 2.1 MVC a novější verze 2.x. Tato ochrana:

* Se nevztahují na všechny změny, 2.1 nebo novější, je určen potenciálně rozbíjející změny v chování modulu runtime ASP.NET Core v subsystému MVC.
* Nevztahuje se na další hlavní verze.

Výchozí kompatibilitu pro ASP.NET Core 2.1 a vyšší 2.x aplikací, které toho **není** volání `SetCompatibilityVersion` je 2.0 kompatibility. To znamená, že není volání `SetCompatibilityVersion` je stejný jako volání funkce `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.

Následující kód nastaví režim kompatibility ASP.NET Core 2.1, s výjimkou následujícího chování:

* [AllowCombiningAuthorizeFilters](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)
* [InputFormatterExceptionPolicy](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup2.cs?name=snippet1)]

Pro aplikace, dojde k rozbíjející změny chování, pomocí příslušné kompatibilita přepínačů:

* Umožňuje použít nejnovější verzi a vyjádřit výslovný nesouhlas zvláštní nejnovější změny chování.
* Získáte čas na aktualizaci aplikace, funguje s nejnovějšími změnami.

[MvcOptions](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs) komentáře zdrojové třídy mají dobrou vysvětlení co změnil a proč změny jsou vylepšení pro většinu uživatelů.

V některé budoucí datum, bude [verze technologie ASP.NET Core 3.0](https://github.com/aspnet/Home/wiki/Roadmap). Ve verzi 3.0 se odebere staré chování podporuje přepínače kompatibility. Domníváme, že se že jedná pozitivní změny, které téměř všechny uživatele. Zavedením teď tyto změny, mohou nyní využívat většinu aplikací a ostatní bude mít čas aktualizovat svoje aplikace.
