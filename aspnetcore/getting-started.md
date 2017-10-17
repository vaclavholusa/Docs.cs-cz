---
title: "Začínáme s ASP.NET Core 2.0"
author: rick-anderson
description: "Rychlý kurz, který vytvoří a spustí jednoduchou aplikaci Hello World pomocí ASP.NET Core."
keywords: "ASP.NET Core, kurzu Začínáme"
ms.author: riande
manager: wpickett
ms.date: 08/30/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: 5cba57c23ab66475648b19cba3f9aaa02bea3dba
ms.sourcegitcommit: d56f36805ce70470775da58db63c0965d65660d3
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2017
---
# <a name="getting-started-with-aspnet-core"></a>Začínáme s ASP.NET Core

> [!NOTE]
> Tyto pokyny jsou určeny nejnovější verzi ASP.NET Core. Hledáte začít pracovat se starší verzí? V tématu [1.1 verzi v tomto kurzu](xref:getting-started-1.1).

1. Nainstalujte [.NET Core](https://www.microsoft.com/net/core/).

2. Vytvoření nového projektu .NET Core.

   V systému macOS a Linux otevřete okno terminálu. V systému Windows otevřete příkazový řádek.

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. Spusťte aplikaci.

    Ke spuštění aplikace použijte následující příkazy:

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. Přejděte do [http://localhost: 5000](http://localhost:5000)

6. Otevřete *Pages/About.cshtml* a upravte stránku a zobrazí se zpráva "Hello, world! Je čas na serveru @DateTime.Now ":

    [!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

7. Přejděte do [http://localhost: 5000/o](http://localhost:5000/About) a ověřit změny.

### <a name="next-steps"></a>Další kroky

Kurzy Začínáme najdete v části [kurzy jádro ASP.NET](tutorials/index.md)

Úvod do ASP.NET základní koncepty a architektury, najdete v části [ASP.NET Core ÚVOD](index.md) a [ASP.NET Core Základy](fundamentals/index.md).

Aplikace ASP.NET Core pomocí .NET Core nebo základní rozhraní .NET Framework – knihovna tříd a modulu runtime. Další informace najdete v tématu [výběru mezi .NET Core a rozhraní .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).