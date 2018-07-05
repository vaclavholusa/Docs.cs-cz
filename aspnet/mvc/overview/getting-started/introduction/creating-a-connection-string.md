---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: Vytvoření připojovacího řetězce a práce s verzí SQL Server LocalDB | Dokumentace Microsoftu
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: d6e04de9b1d71a77b4b56b04417d6d4bc24cbd72
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37361665"
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Vytvoření připojovacího řetězce a práce s verzí SQL Server LocalDB
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Vytvoření připojovacího řetězce a práce s verzí SQL Server LocalDB

`MovieDBContext` Třídy, které jste vytvořili zpracovává úlohu s připojením k databázi a mapování `Movie` objekty se záznamy v databázi. Jednu otázku, kterou můžete pokládat, je, jak určit databázi, kterou se připojí k. Nemáte ve skutečnosti zadejte databázi, kterou chcete použít, bude jako výchozí rozhraní Entity Framework pomocí [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb). V této části explicitně přidáme připojovacího řetězce v *Web.config* souboru aplikace.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) je Odlehčená verze SQL serveru Express databázového stroje, který začíná na vyžádání a běží v uživatelském režimu. LocalDB běží v ve speciálním režimu provádění SQL Server Express, která umožňuje pracovat s databází jako *.mdf* soubory. Obvykle jsou uloženy soubory databáze LocalDB *aplikace\_Data* složce webového projektu.

SQL Server Express se nedoporučuje používat v produkčním prostředí webové aplikace. LocalDB zejména by neměl být sloužící k produkčním účelům s webovou aplikací vzhledem k tomu, že není určená pro práci se službou IIS. Nicméně databázi LocalDB, můžete jednoduše migrovat do systému SQL Server nebo SQL Azure.

V sadě Visual Studio 2017 je LocalDB nainstalované ve výchozím nastavení se sadou Visual Studio.

Ve výchozím nastavení, Entity Framework hledá připojovací řetězec se stejným názvem jako třída objektu kontextu (`MovieDBContext` pro tento projekt). Další informace najdete v části [připojovací řetězce SQL serveru pro webové aplikace ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

Otevřete kořenový adresář aplikace *Web.config* souboru je uvedeno níže. (Ne *Web.config* ve *zobrazení* složky.)

![](creating-a-connection-string/_static/image1.png)

Najít `<connectionStrings>` element:

![](creating-a-connection-string/_static/image2.png)

Přidejte následující připojovací řetězec do `<connectionStrings>` prvek *Web.config* souboru.

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

Následující příklad ukazuje část *Web.config* soubor se přidat nový připojovací řetězec:

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

Dva připojovací řetězce jsou velmi podobné. První řetězec připojení jmenuje `DefaultConnection` a používá se pro databázi členství na ovládací prvek, kdo má přístup k aplikaci. Připojovací řetězec, který jste přidali Určuje databázi LocalDB s názvem *Movie.mdf* umístěné v *aplikace\_Data* složky. Jsme nebude použití databáze členství v tomto kurzu, další informace o členství, ověřování a zabezpečení naleznete v části Moje kurzu [vytvoření aplikace ASP.NET MVC pomocí ověření a SQL DB a nasazení do služby Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

Název připojovacího řetězce se musí shodovat s názvem [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) třídy.

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

Doopravdy nepotřebujete přidat `MovieDBContext` připojovací řetězec. Pokud nechcete zadat připojovací řetězec, Entity Framework se vytvoří databáze LocalDB v adresáři uživatele s plně kvalifikovaný název [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) třídy (v tomto případě `MvcMovie.Models.MovieDBContext`). Název můžete databázi cokoli, co vám vyhovuje, za předpokladu, má *. MDF* příponu. Například společnost Microsoft mohla mít název databáze *MyFilms.mdf*.

V dalším kroku vytvoříte novou `MoviesController` třídu, která vám pomůže se zobrazí data o filmech a umožní uživatelům vytvářet nové video výpisy.

> [!div class="step-by-step"]
> [Předchozí](adding-a-model.md)
> [další](accessing-your-models-data-from-a-controller.md)
