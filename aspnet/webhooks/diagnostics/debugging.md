---
uid: webhooks/diagnostics/debugging
title: ASP.NET – Webhooky ladění | Dokumentace Microsoftu
author: rick-anderson
description: Postup ladění ASP.NET – Webhooky.
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 25fa47202dd31d6ebd7a0e179075333108515274
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37801985"
---
# <a name="aspnet-webhooks-debugging"></a>ASP.NET – Webhooky ladění  

## <a name="debugging-in-azure"></a>Ladění v Azure

Ladění webové aplikace při spuštění v Azure, najdete v tématu výukového programu [řešení problémů s webovou aplikací ve službě Azure App Service pomocí sady Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="debugging-with-source-and-symbols"></a>Ladění se zdrojem a symboly

Kromě ladění vlastního kódu, je možné ladit přímo do WebHooks Microsoft ASP.NET a ve skutečnosti všechny technologie .NET. Tento postup funguje, bez ohledu na to, zda ladit místně nebo vzdáleně. Nejprve nakonfigurujte Visual Studio a najít tak, že přejdete do zdroje a symbolů **ladění** a potom **možnosti a nastavení**. Nastavení možností takto:

![Možnosti a nastavení](_static/SourceSymbols.png)

Pak přidejte odkaz na [symbolsource.org](http://symbolsource.org) pro stahování zdroje a symbolů. Přejděte **symboly** kartu nabídky výše a přidejte následující položky jako umístění symbolů:

```
http://srv.symbolsource.org/pdb/Public
```

Kromě toho se ujistěte, že adresář mezipaměti obsahuje krátký název; názvy souborů v opačném případě můžete získat příliš dlouho která způsobí, že symboly nelze načíst. Je ukázková cesta:

```
C:\SymCache
```

Nastavení by měl vypadat nějak takto:

![Ukázka umístění souboru možnosti symbolu](_static/SymSource.png)
