---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: "Použití migrace Code First počáteční hodnoty databázi | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: df75a69644033cc76fee86b5a9692ab65beb4d01
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="use-code-first-migrations-to-seed-the-database"></a>Použití migrace Code First počáteční hodnoty databáze
====================
podle [Wasson Jan](https://github.com/MikeWasson)

[Stáhněte si dokončený projekt](https://github.com/MikeWasson/BookService)

V této části budete používat [migrace Code First](https://msdn.microsoft.com/en-us/data/jj591621) v EF počáteční hodnoty databáze s testovacích datech.

Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**, pak vyberte **Konzola správce balíčků**. V okně konzoly Správce balíčků zadejte následující příkaz:

[!code-console[Main](part-3/samples/sample1.cmd)]

Tento příkaz přidá složku s názvem migrace do projektu a kód soubor s názvem Configuration.cs ve složce migrace.

![](part-3/_static/image1.png)

Otevřete soubor Configuration.cs. Přidejte následující **pomocí** příkaz.

[!code-csharp[Main](part-3/samples/sample2.cs)]

Pak přidejte následující kód, který **Configuration.Seed** metoda:

[!code-csharp[Main](part-3/samples/sample3.cs)]

V okně konzoly Správce balíčků zadejte následující příkazy:

[!code-console[Main](part-3/samples/sample4.cmd)]

První příkaz vygeneruje kód, který vytvoří databázi a v druhém příkazu spustí tento kód. Databáze je vytvořená místně, pomocí [LocalDB](https://msdn.microsoft.com/en-us/library/hh510202.aspx).

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a>Prozkoumejte rozhraní API (volitelné)

Stisknutím klávesy F5 spusťte aplikaci v režimu ladění. Visual Studio spustí službu IIS Express a spustí webovou aplikaci. Visual Studio pak spustí prohlížeč a otevře domovskou stránku aplikace.

Po spuštění webového projektu sady Visual Studio přiřadí číslo portu. Na obrázku níže číslo portu je 50524. Při spuštění aplikace se zobrazí jiné číslo portu.

![](part-3/_static/image3.png)

Domovská stránka je implementovaná pomocí ASP.NET MVC. V horní části stránky je odkaz, který uvádí "API". Tento odkaz vám přináší na stránku nápovědy automaticky generovaných pro webové rozhraní API. (Další způsob, jakým vygeneruje tuto stránku nápovědy a jak můžete přidat vlastní dokumentaci na stránku najdete v tématu [vytváření pomoci stránek pro ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) Chcete-li zobrazit podrobnosti o rozhraní API, včetně formát požadavku a odpovědi odkazů na stránky se můžete kliknutím na tlačítko Nápověda.

![](part-3/_static/image4.png)

Rozhraní API umožňuje operace CRUD v databázi. Zde je souhrn rozhraní API.

| Autoři |  |
| --- | -- |
| ZÍSKAT rozhraní api nebo autoři | Získáte všechny autory. |
| Rozhraní api nebo autoři GET / {id} | Získat Autor podle ID. |
| POST/api/autoři | Vytvořte nový autora. |
| UVEĎTE /api/autoři / {id} | Aktualizujte stávající autora. |
| Odstranit /api/autoři / {id} | Odstraňte Autor. |

| Knihy |  |
| --- | -- |
| ZÍSKAT /api/books | Získání všech knih. |
| ZÍSKAT /api/books / {id} | Získání seznamu pomocí ID. |
| ODESÍLÁNÍ/api/seznamů | Vytvoření nového seznamu. |
| UVEĎTE /api/books / {id} | Aktualizujte stávající adresáře. |
| Odstranit /api/books / {id} | Odstranění seznamu. |

## <a name="view-the-database-optional"></a>Zobrazení databáze (volitelné)

Když jste spustili příkaz Update-Database, EF databáze a názvem `Seed` metoda. Když aplikaci spouštíte místně, použije EF [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). Databázi můžete zobrazit v sadě Visual Studio. Z **zobrazení** nabídce vyberte možnost **Průzkumník objektů systému SQL Server**.

![](part-3/_static/image5.png)

V **připojit k serveru** dialogové okno, v **název serveru** textové pole, zadejte "(localdb) \v11.0". Ponechte **ověřování** možnost jako "Ověřování systému Windows". Klikněte na tlačítko **připojit**.

![](part-3/_static/image6.png)

Visual Studio připojí na instanci LocalDB a ukazuje vaše stávající databáze, v okně Průzkumník objektů systému SQL Server. Můžete rozbalit uzly zobrazíte tabulek, které EF vytvořeny.

![](part-3/_static/image7.png)

Chcete-li zobrazit data, klikněte pravým tlačítkem na tabulku a vyberte **Data zobrazení**.

![](part-3/_static/image8.png)

Následující snímek obrazovky ukazuje výsledky pro knihy tabulky. Všimněte si, že EF naplní databázi s počáteční hodnoty data a tabulka obsahuje cizí klíč k tabulce Autoři.

![](part-3/_static/image9.png)

>[!div class="step-by-step"]
[Předchozí](part-2.md)
[další](part-4.md)
