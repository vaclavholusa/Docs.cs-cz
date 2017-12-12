---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: "Vytvoření připojovacího řetězce a práci s SQL serveru LocalDB | Microsoft Docs"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 41f1f30d86406580ab9fc7278a94d9c291913f9a
ms.sourcegitcommit: d1d8071d4093bf2444b5ae19d6e45c3d187e338b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/19/2017
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Vytvoření připojovacího řetězce a práci s LocalDB serveru SQL
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Vytvoření připojovacího řetězce a práci s LocalDB serveru SQL

`MovieDBContext` Třídy, které jste vytvořili zpracovává úlohu s připojením k databázi a mapování `Movie` objekty záznamy v databázi. Jeden otázku může, ale je uveden postup určit se připojí k databázi. Nemáte ve skutečnosti zadejte databázi, ke které použít, bude jako výchozí rozhraní Entity Framework pomocí [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb). V této části explicitně přidáme připojovací řetězec *Web.config* souboru aplikace.

## <a name="sql-server-express-localdb"></a>Databáze SQL Server Express LocalDB

[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) je Odlehčená verze SQL Server Express databázový stroj, který se spustí na vyžádání a běží v uživatelském režimu. LocalDB běží v režimu speciální spuštění systému SQL Server Express, který umožňuje pracovat s databázemi jako *.mdf* soubory. Obvykle jsou zachovány soubory databáze LocalDB v *aplikace\_Data* složky webového projektu.

SQL Server Express se nedoporučuje používat v produkční webové aplikace. LocalDB zejména nepoužívejte pro produkční prostředí s webovou aplikací protože není určeno k práci se službou IIS. Však databáze LocalDB můžete snadno migrovat na server SQL Server nebo SQL Azure.

V aplikaci Visual Studio 2017 LocalDB je nainstalována ve výchozím nastavení pomocí sady Visual Studio.

Ve výchozím nastavení, vypadá rozhraní Entity Framework pro řetězec připojení se stejným názvem jako objekt context – třída (`MovieDBContext` pro tento projekt). Další informace najdete v části [připojovací řetězce SQL serveru pro webové aplikace ASP.NET](https://msdn.microsoft.com/en-us/library/jj653752.aspx).

Otevřete kořenový adresář aplikace *Web.config* souboru vidíte níže. (Není *Web.config* v soubor *zobrazení* složky.)

![](creating-a-connection-string/_static/image1.png)

Najít `<connectionStrings>` element:

![](creating-a-connection-string/_static/image2.png)

Přidejte následující připojovací řetězec k `<connectionStrings>` element v *Web.config* souboru.

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

Následující příklad ukazuje část *Web.config* soubor s přidat nový připojovací řetězec:

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

Dva připojovací řetězce jsou velmi podobné. První připojovací řetězec názvem `DefaultConnection` a používá se pro databázi členství řídit, kdo má přístup k aplikaci. Určuje připojovací řetězec, který jste přidali LocalDB databáze s názvem *Movie.mdf* umístěný v *aplikace\_Data* složky. Jsme nebude používat databázi členství v tomto kurzu, další informace o členství, ověřování a zabezpečení, najdete v části Moje kurzu [vytvoření aplikace ASP.NET MVC pomocí ověřování a databázi SQL a nasazení do Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

Název připojovacího řetězce musí odpovídat názvu [DbContext](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=vs.103).aspx) třídy.

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

Chcete-li přidat nepotřebujete ve skutečnosti `MovieDBContext` připojovací řetězec. Pokud nezadáte připojovací řetězec, rozhraní Entity Framework vytvoří databáze LocalDB v adresáři uživatele s plně kvalifikovaný název [DbContext](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=vs.103).aspx) – třída (v tomto případě `MvcMovie.Models.MovieDBContext`). Můžete pojmenovat databázi nic se vám líbí, dokud obsahuje *. MDF* příponu. Například můžeme mohla mít název databáze *MyFilms.mdf*.

Dále budete vytvářet nové `MoviesController` třídu, která můžete použít k zobrazení dat film a povolit uživatelům vytvářet nové výpisy film.

>[!div class="step-by-step"]
[Předchozí](adding-a-model.md)
[další](accessing-your-models-data-from-a-controller.md)
