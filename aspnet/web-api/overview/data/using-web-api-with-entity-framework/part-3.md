---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Přidání dat do databáze pomocí migrace Code First | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 733b1c343d774e5fa8757808be07a9ae67481d84
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910496"
---
<a name="use-code-first-migrations-to-seed-the-database"></a>Přidání dat do databáze pomocí migrace Code First
====================
podle [Mike Wasson](https://github.com/MikeWasson)

[Stáhnout dokončený projekt](https://github.com/MikeWasson/BookService)

V této části použijete [migrace Code First](https://msdn.microsoft.com/data/jj591621) v EF naplnit databázi daty testu.

Z **nástroje** příkaz **Správce balíčků NuGet**, vyberte **konzoly Správce balíčků**. V okně konzoly Správce balíčků zadejte následující příkaz:

[!code-console[Main](part-3/samples/sample1.cmd)]

Tento příkaz přidá složku do projektu s názvem migrace a navíc Configuration.cs ve složce migrace s názvem souboru kódu.

![](part-3/_static/image1.png)

Otevřete soubor Configuration.cs. Přidejte následující **pomocí** příkazu.

[!code-csharp[Main](part-3/samples/sample2.cs)]

Pak přidejte následující kód, který **Configuration.Seed** metody:

[!code-csharp[Main](part-3/samples/sample3.cs)]

V okně konzoly Správce balíčků zadejte následující příkazy:

[!code-console[Main](part-3/samples/sample4.cmd)]

První příkaz vygeneruje kód, který vytvoří databázi a druhý příkaz spustí tento kód. Databáze se vytvoří místně, pomocí [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a>Prozkoumejte rozhraní API (volitelné)

Stisknutím klávesy F5 spusťte aplikaci v režimu ladění. Visual Studio spustí službu IIS Express a spustí webovou aplikaci. Visual Studio pak spustí prohlížeč a otevře domovskou stránku aplikace.

Když Visual Studio spustí webový projekt, přiřadí číslo portu. Na následujícím obrázku je číslo portu 50524. Při spuštění aplikace se zobrazí jiné číslo portu.

![](part-3/_static/image3.png)

Na domovské stránce se implementuje pomocí technologie ASP.NET MVC. V horní části stránky je odkaz, který říká "Rozhraní API". Tento odkaz vám přináší na stránku nápovědy pro automaticky generované pro webové rozhraní API. (Další způsob, jakým vygeneruje tuto stránku nápovědy a jak můžete přidat vlastní dokumentaci na stránku, najdete v článku [vytváření stránky nápovědy pro rozhraní ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) Kliknutím na dlaždici Nápověda odkazů na stránky zobrazíte podrobnosti o rozhraní API, včetně formát požadavku a odpovědi.

![](part-3/_static/image4.png)

Rozhraní API umožňuje operace CRUD v databázi. Zde je souhrn rozhraní API.

| Autoři |  |
| --- | -- |
| ZÍSKÁNÍ rozhraní api/autorů | Získáte všichni autoři. |
| Rozhraní api GET/autoři / {id} | Získat Autor podle ID. |
| PŘÍSPĚVEK/api/autorů | Vytvořte nový Autor. |
| Vložení/webové rozhraní API/autoři / {id} | Aktualizujte existující Autor. |
| ODSTRANIT/webové rozhraní API/autoři / {id} | Odstraňte Autor. |

| Knihy |  |
| --- | -- |
| ZÍSKAT /api/books | Získáte všechny knihy. |
| ZÍSKAT/webové rozhraní API/books / {id} | Získat knihu podle ID. |
| Publikovat/api/knihy | Vytvoření nového adresáře. |
| Vložení/webové rozhraní API/books / {id} | Aktualizace existujícího adresáře. |
| ODSTRANIT/webové rozhraní API/books / {id} | Odstraňte knihy. |

## <a name="view-the-database-optional"></a>Zobrazení databáze (nepovinné)

Při spuštění příkazu Update-databázi EF vytváření databáze a volá se, `Seed` metody. Když aplikaci spouštíte místně, pomocí EF [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). Databázi můžete zobrazit v sadě Visual Studio. Z **zobrazení** nabídce vyberte možnost **Průzkumník objektů systému SQL Server**.

![](part-3/_static/image5.png)

V **připojit k serveru** dialogového okna v **název serveru** textové pole, zadejte "(localdb) \v11.0". Nechte **ověřování** možnost "Ověřování Windows". Klikněte na tlačítko **připojit**.

![](part-3/_static/image6.png)

Visual Studio na instanci LocalDB se připojí a zobrazuje existujících databází v okně Průzkumník objektů systému SQL Server. Lze rozbalit uzly zobrazíte tabulky, které EF vytvořili.

![](part-3/_static/image7.png)

Chcete-li zobrazit data, klikněte pravým tlačítkem na tabulku a vyberte **Data zobrazení**.

![](part-3/_static/image8.png)

Následující snímek obrazovky ukazuje výsledky pro tabulku knihy. Všimněte si, že EF naplnit databázi daty počáteční hodnoty a tabulka obsahuje cizí klíč do tabulky autoři.

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> [Předchozí](part-2.md)
> [další](part-4.md)
