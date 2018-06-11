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
ms.openlocfilehash: 6d05cd8c0ee6c9029eb9bbafc15d9b19ee7633de
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252019"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>Články založené na ASP.NET Core projekty vytvořené pomocí jednotlivých uživatelských účtů

Jádro ASP.NET Identity je součástí šablony projektů v sadě Visual Studio s parametrem "Jednotlivé uživatelské účty".

Ověření šablony jsou dostupné v rozhraní .NET Core rozhraní příkazového řádku s `-au Individual`:

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new webapp -au Individual
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

::: moniker-end

V následujících článcích ukazují, jak použít kód, který vygenerovala v šablonách ASP.NET Core, které používají jednotlivých uživatelských účtů:

* [Dvoufaktorové ověřování přes SMS](xref:security/authentication/2fa)
* [Potvrzení účtu a obnovení hesla v ASP.NET Core](xref:security/authentication/accconfirm)
* [Vytvoření aplikace ASP.NET Core s uživatelskými daty chráněn autorizace](xref:security/authorization/secure-data)
