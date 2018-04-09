---
title: Články založené na ASP.NET Core projekty vytvořené pomocí jednotlivých uživatelských účtů
author: rick-anderson
description: Zjistit články založené na ASP.NET Core projekty vytvořené pomocí jednotlivých uživatelských účtů.
manager: wpickett
ms.author: riande
ms.date: 11/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/individual
ms.openlocfilehash: 40715debb48c0a7121ce84d7843b8517b0973e74
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>Články založené na ASP.NET Core projekty vytvořené pomocí jednotlivých uživatelských účtů

Jádro ASP.NET Identity je součástí šablony projektů v sadě Visual Studio s parametrem "Jednotlivé uživatelské účty".

Ověření šablony jsou dostupné v rozhraní .NET Core rozhraní příkazového řádku s `-au Individual`:

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

V následujících článcích ukazují, jak použít kód, který vygenerovala v šablonách ASP.NET Core, které používají jednotlivých uživatelských účtů:

* [Dvoufaktorové ověřování přes SMS](xref:security/authentication/2fa)
* [Potvrzení účtu a obnovení hesla v ASP.NET Core](xref:security/authentication/accconfirm)
* [Vytvoření aplikace ASP.NET Core s uživatelskými daty chráněn autorizace](xref:security/authorization/secure-data)
