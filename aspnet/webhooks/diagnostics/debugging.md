---
uid: webhooks/diagnostics/debugging
title: Ladění Webhooky ASP.NET | Microsoft Docs
author: rick-anderson
description: Postup ladění ASP.NET Webhooky.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: 524cdf0246eda9ef213414923cd23a92a01f211e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
ms.locfileid: "28044861"
---
# <a name="aspnet-webhooks-debugging"></a>ASP.NET Webhooky ladění  

## <a name="debugging-in-azure"></a>Ladění v Azure

K ladění webové aplikace při spuštění v Azure, naleznete v kurzu [řešení potíží s webovou aplikaci v Azure App Service pomocí sady Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="debugging-with-source-and-symbols"></a>Ladění se zdrojem a symboly.

Kromě ladění vlastní kód, je možné k ladění přímo do WebHooks Microsoft ASP.NET a ve skutečnosti všechny .NET. Tento postup funguje bez ohledu na to, jestli při ladění místně nebo vzdáleně. Nejprve nakonfigurujte Visual Studio najít zdroj a symboly přechodem na **ladění** a potom **možnosti a nastavení**. Nastavení možností takto:

![Možnosti a nastavení](_static/SourceSymbols.png)

Pak přidejte odkaz na [symbolsource.org](http://symbolsource.org) pro stahování zdroj a symboly. Přejděte na **symboly** kartě nabídce výše a přidejte následující jako umístění symbolu:

```
http://srv.symbolsource.org/pdb/Public
```

Kromě toho se ujistěte, že adresář mezipaměti obsahuje krátký název; názvy souborů v opačném případě můžete získat příliš dlouho které způsobí symboly, které mají se nenačetl. Ukázka cesta je:

```
C:\SymCache
```

Nastavení by měl vypadat podobně jako tento:

![Ukázka umístění souboru možnosti Symbol](_static/SymSource.png)
