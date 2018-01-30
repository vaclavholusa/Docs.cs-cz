---
title: "Články založené na projekty vytvořené pomocí jednotlivých uživatelských účtů"
author: rick-anderson
description: "Tento dokument obsahuje seznam článků založené na projekty vytvořené pomocí jednotlivých uživatelských účtů."
manager: wpickett
ms.author: riande
ms.date: 11/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/individual
ms.openlocfilehash: aee18fa08fbc5c8452ca2b401d32858edaf55e7c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="articles-based-on-projects-created-with-individual-user-accounts"></a>Články založené na projekty vytvořené pomocí jednotlivých uživatelských účtů

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
