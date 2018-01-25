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
ms.openlocfilehash: 25d1c1c9954baaca9ef91eff3dd3c853930a5893
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="a5307-102">Vytvoření připojovacího řetězce a práci s LocalDB serveru SQL</span><span class="sxs-lookup"><span data-stu-id="a5307-102">Creating a Connection String and Working with SQL Server LocalDB</span></span>
====================
<span data-ttu-id="a5307-103">Podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="a5307-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="a5307-104">Vytvoření připojovacího řetězce a práci s LocalDB serveru SQL</span><span class="sxs-lookup"><span data-stu-id="a5307-104">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="a5307-105">`MovieDBContext` Třídy, které jste vytvořili zpracovává úlohu s připojením k databázi a mapování `Movie` objekty záznamy v databázi.</span><span class="sxs-lookup"><span data-stu-id="a5307-105">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="a5307-106">Jeden otázku může, ale je uveden postup určit se připojí k databázi.</span><span class="sxs-lookup"><span data-stu-id="a5307-106">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="a5307-107">Nemáte ve skutečnosti zadejte databázi, ke které použít, bude jako výchozí rozhraní Entity Framework pomocí [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="a5307-107">You don't actually have to specify which database to use, Entity Framework will default to using [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="a5307-108">V této části explicitně přidáme připojovací řetězec *Web.config* souboru aplikace.</span><span class="sxs-lookup"><span data-stu-id="a5307-108">In this section we'll explicitly add a connection string in the *Web.config* file of the application.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="a5307-109">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="a5307-109">SQL Server Express LocalDB</span></span>

<span data-ttu-id="a5307-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) je Odlehčená verze SQL Server Express databázový stroj, který se spustí na vyžádání a běží v uživatelském režimu.</span><span class="sxs-lookup"><span data-stu-id="a5307-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) is a lightweight version of the SQL Server Express Database Engine that starts on demand and runs in user mode.</span></span> <span data-ttu-id="a5307-111">LocalDB běží v režimu speciální spuštění systému SQL Server Express, který umožňuje pracovat s databázemi jako *.mdf* soubory.</span><span class="sxs-lookup"><span data-stu-id="a5307-111">LocalDB runs in a special execution mode of SQL Server Express that enables you to work with databases as *.mdf* files.</span></span> <span data-ttu-id="a5307-112">Obvykle jsou zachovány soubory databáze LocalDB v *aplikace\_Data* složky webového projektu.</span><span class="sxs-lookup"><span data-stu-id="a5307-112">Typically, LocalDB database files are kept in the *App\_Data* folder of a web project.</span></span>

<span data-ttu-id="a5307-113">SQL Server Express se nedoporučuje používat v produkční webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a5307-113">SQL Server Express is not recommended for use in production web applications.</span></span> <span data-ttu-id="a5307-114">LocalDB zejména nepoužívejte pro produkční prostředí s webovou aplikací protože není určeno k práci se službou IIS.</span><span class="sxs-lookup"><span data-stu-id="a5307-114">LocalDB in particular should not be used for production with a web application because it is not designed to work with IIS.</span></span> <span data-ttu-id="a5307-115">Však databáze LocalDB můžete snadno migrovat na server SQL Server nebo SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="a5307-115">However, a LocalDB database can be easily migrated to SQL Server or SQL Azure.</span></span>

<span data-ttu-id="a5307-116">V aplikaci Visual Studio 2017 LocalDB je nainstalována ve výchozím nastavení pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a5307-116">In Visual Studio 2017, LocalDB is installed by default with Visual Studio.</span></span>

<span data-ttu-id="a5307-117">Ve výchozím nastavení, vypadá rozhraní Entity Framework pro řetězec připojení se stejným názvem jako objekt context – třída (`MovieDBContext` pro tento projekt).</span><span class="sxs-lookup"><span data-stu-id="a5307-117">By default, the Entity Framework looks for a connection string named the same as the object context class (`MovieDBContext` for this project).</span></span> <span data-ttu-id="a5307-118">Další informace najdete v části [připojovací řetězce SQL serveru pro webové aplikace ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).</span><span class="sxs-lookup"><span data-stu-id="a5307-118">For more information see [SQL Server Connection Strings for ASP.NET Web Applications](https://msdn.microsoft.com/library/jj653752.aspx).</span></span>

<span data-ttu-id="a5307-119">Otevřete kořenový adresář aplikace *Web.config* souboru vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="a5307-119">Open the application root *Web.config* file shown below.</span></span> <span data-ttu-id="a5307-120">(Není *Web.config* v soubor *zobrazení* složky.)</span><span class="sxs-lookup"><span data-stu-id="a5307-120">(Not the *Web.config* file in the *Views* folder.)</span></span>

![](creating-a-connection-string/_static/image1.png)

<span data-ttu-id="a5307-121">Najít `<connectionStrings>` element:</span><span class="sxs-lookup"><span data-stu-id="a5307-121">Find the `<connectionStrings>` element:</span></span>

![](creating-a-connection-string/_static/image2.png)

<span data-ttu-id="a5307-122">Přidejte následující připojovací řetězec k `<connectionStrings>` element v *Web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="a5307-122">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

<span data-ttu-id="a5307-123">Následující příklad ukazuje část *Web.config* soubor s přidat nový připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="a5307-123">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

<span data-ttu-id="a5307-124">Dva připojovací řetězce jsou velmi podobné.</span><span class="sxs-lookup"><span data-stu-id="a5307-124">The two connection strings are very similar.</span></span> <span data-ttu-id="a5307-125">První připojovací řetězec názvem `DefaultConnection` a používá se pro databázi členství řídit, kdo má přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a5307-125">The first connection string is named `DefaultConnection` and is used for the membership database to control who can access the application.</span></span> <span data-ttu-id="a5307-126">Určuje připojovací řetězec, který jste přidali LocalDB databáze s názvem *Movie.mdf* umístěný v *aplikace\_Data* složky.</span><span class="sxs-lookup"><span data-stu-id="a5307-126">The connection string you've added specifies a LocalDB database named *Movie.mdf* located in the *App\_Data* folder.</span></span> <span data-ttu-id="a5307-127">Jsme nebude používat databázi členství v tomto kurzu, další informace o členství, ověřování a zabezpečení, najdete v části Moje kurzu [vytvoření aplikace ASP.NET MVC pomocí ověřování a databázi SQL a nasazení do Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="a5307-127">We won't use the membership database in this tutorial, for more information on membership, authentication and security, see my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span>

<span data-ttu-id="a5307-128">Název připojovacího řetězce musí odpovídat názvu [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) třídy.</span><span class="sxs-lookup"><span data-stu-id="a5307-128">The name of the connection string must match the name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class.</span></span>

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

<span data-ttu-id="a5307-129">Chcete-li přidat nepotřebujete ve skutečnosti `MovieDBContext` připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="a5307-129">You don't actually need to add the `MovieDBContext` connection string.</span></span> <span data-ttu-id="a5307-130">Pokud nezadáte připojovací řetězec, rozhraní Entity Framework vytvoří databáze LocalDB v adresáři uživatele s plně kvalifikovaný název [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) – třída (v tomto případě `MvcMovie.Models.MovieDBContext`).</span><span class="sxs-lookup"><span data-stu-id="a5307-130">If you don't specify a connection string, Entity Framework will create a LocalDB database in the users directory with the fully qualified name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class (in this case `MvcMovie.Models.MovieDBContext`).</span></span> <span data-ttu-id="a5307-131">Můžete pojmenovat databázi nic se vám líbí, dokud obsahuje *. MDF* příponu.</span><span class="sxs-lookup"><span data-stu-id="a5307-131">You can name the database anything you like, as long as it has the *.MDF* suffix.</span></span> <span data-ttu-id="a5307-132">Například můžeme mohla mít název databáze *MyFilms.mdf*.</span><span class="sxs-lookup"><span data-stu-id="a5307-132">For example, we could name the database *MyFilms.mdf*.</span></span>

<span data-ttu-id="a5307-133">Dále budete vytvářet nové `MoviesController` třídu, která můžete použít k zobrazení dat film a povolit uživatelům vytvářet nové výpisy film.</span><span class="sxs-lookup"><span data-stu-id="a5307-133">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a5307-134">[Předchozí](adding-a-model.md)
[další](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="a5307-134">[Previous](adding-a-model.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
