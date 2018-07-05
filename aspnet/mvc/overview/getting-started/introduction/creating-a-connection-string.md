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
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="25eeb-102">Vytvoření připojovacího řetězce a práce s verzí SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="25eeb-102">Creating a Connection String and Working with SQL Server LocalDB</span></span>
====================
<span data-ttu-id="25eeb-103">Podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="25eeb-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="25eeb-104">Vytvoření připojovacího řetězce a práce s verzí SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="25eeb-104">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="25eeb-105">`MovieDBContext` Třídy, které jste vytvořili zpracovává úlohu s připojením k databázi a mapování `Movie` objekty se záznamy v databázi.</span><span class="sxs-lookup"><span data-stu-id="25eeb-105">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="25eeb-106">Jednu otázku, kterou můžete pokládat, je, jak určit databázi, kterou se připojí k.</span><span class="sxs-lookup"><span data-stu-id="25eeb-106">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="25eeb-107">Nemáte ve skutečnosti zadejte databázi, kterou chcete použít, bude jako výchozí rozhraní Entity Framework pomocí [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="25eeb-107">You don't actually have to specify which database to use, Entity Framework will default to using [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="25eeb-108">V této části explicitně přidáme připojovacího řetězce v *Web.config* souboru aplikace.</span><span class="sxs-lookup"><span data-stu-id="25eeb-108">In this section we'll explicitly add a connection string in the *Web.config* file of the application.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="25eeb-109">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="25eeb-109">SQL Server Express LocalDB</span></span>

<span data-ttu-id="25eeb-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) je Odlehčená verze SQL serveru Express databázového stroje, který začíná na vyžádání a běží v uživatelském režimu.</span><span class="sxs-lookup"><span data-stu-id="25eeb-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) is a lightweight version of the SQL Server Express Database Engine that starts on demand and runs in user mode.</span></span> <span data-ttu-id="25eeb-111">LocalDB běží v ve speciálním režimu provádění SQL Server Express, která umožňuje pracovat s databází jako *.mdf* soubory.</span><span class="sxs-lookup"><span data-stu-id="25eeb-111">LocalDB runs in a special execution mode of SQL Server Express that enables you to work with databases as *.mdf* files.</span></span> <span data-ttu-id="25eeb-112">Obvykle jsou uloženy soubory databáze LocalDB *aplikace\_Data* složce webového projektu.</span><span class="sxs-lookup"><span data-stu-id="25eeb-112">Typically, LocalDB database files are kept in the *App\_Data* folder of a web project.</span></span>

<span data-ttu-id="25eeb-113">SQL Server Express se nedoporučuje používat v produkčním prostředí webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="25eeb-113">SQL Server Express is not recommended for use in production web applications.</span></span> <span data-ttu-id="25eeb-114">LocalDB zejména by neměl být sloužící k produkčním účelům s webovou aplikací vzhledem k tomu, že není určená pro práci se službou IIS.</span><span class="sxs-lookup"><span data-stu-id="25eeb-114">LocalDB in particular should not be used for production with a web application because it is not designed to work with IIS.</span></span> <span data-ttu-id="25eeb-115">Nicméně databázi LocalDB, můžete jednoduše migrovat do systému SQL Server nebo SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="25eeb-115">However, a LocalDB database can be easily migrated to SQL Server or SQL Azure.</span></span>

<span data-ttu-id="25eeb-116">V sadě Visual Studio 2017 je LocalDB nainstalované ve výchozím nastavení se sadou Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="25eeb-116">In Visual Studio 2017, LocalDB is installed by default with Visual Studio.</span></span>

<span data-ttu-id="25eeb-117">Ve výchozím nastavení, Entity Framework hledá připojovací řetězec se stejným názvem jako třída objektu kontextu (`MovieDBContext` pro tento projekt).</span><span class="sxs-lookup"><span data-stu-id="25eeb-117">By default, the Entity Framework looks for a connection string named the same as the object context class (`MovieDBContext` for this project).</span></span> <span data-ttu-id="25eeb-118">Další informace najdete v části [připojovací řetězce SQL serveru pro webové aplikace ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).</span><span class="sxs-lookup"><span data-stu-id="25eeb-118">For more information see [SQL Server Connection Strings for ASP.NET Web Applications](https://msdn.microsoft.com/library/jj653752.aspx).</span></span>

<span data-ttu-id="25eeb-119">Otevřete kořenový adresář aplikace *Web.config* souboru je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="25eeb-119">Open the application root *Web.config* file shown below.</span></span> <span data-ttu-id="25eeb-120">(Ne *Web.config* ve *zobrazení* složky.)</span><span class="sxs-lookup"><span data-stu-id="25eeb-120">(Not the *Web.config* file in the *Views* folder.)</span></span>

![](creating-a-connection-string/_static/image1.png)

<span data-ttu-id="25eeb-121">Najít `<connectionStrings>` element:</span><span class="sxs-lookup"><span data-stu-id="25eeb-121">Find the `<connectionStrings>` element:</span></span>

![](creating-a-connection-string/_static/image2.png)

<span data-ttu-id="25eeb-122">Přidejte následující připojovací řetězec do `<connectionStrings>` prvek *Web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="25eeb-122">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

<span data-ttu-id="25eeb-123">Následující příklad ukazuje část *Web.config* soubor se přidat nový připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="25eeb-123">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

<span data-ttu-id="25eeb-124">Dva připojovací řetězce jsou velmi podobné.</span><span class="sxs-lookup"><span data-stu-id="25eeb-124">The two connection strings are very similar.</span></span> <span data-ttu-id="25eeb-125">První řetězec připojení jmenuje `DefaultConnection` a používá se pro databázi členství na ovládací prvek, kdo má přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="25eeb-125">The first connection string is named `DefaultConnection` and is used for the membership database to control who can access the application.</span></span> <span data-ttu-id="25eeb-126">Připojovací řetězec, který jste přidali Určuje databázi LocalDB s názvem *Movie.mdf* umístěné v *aplikace\_Data* složky.</span><span class="sxs-lookup"><span data-stu-id="25eeb-126">The connection string you've added specifies a LocalDB database named *Movie.mdf* located in the *App\_Data* folder.</span></span> <span data-ttu-id="25eeb-127">Jsme nebude použití databáze členství v tomto kurzu, další informace o členství, ověřování a zabezpečení naleznete v části Moje kurzu [vytvoření aplikace ASP.NET MVC pomocí ověření a SQL DB a nasazení do služby Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="25eeb-127">We won't use the membership database in this tutorial, for more information on membership, authentication and security, see my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span>

<span data-ttu-id="25eeb-128">Název připojovacího řetězce se musí shodovat s názvem [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) třídy.</span><span class="sxs-lookup"><span data-stu-id="25eeb-128">The name of the connection string must match the name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class.</span></span>

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

<span data-ttu-id="25eeb-129">Doopravdy nepotřebujete přidat `MovieDBContext` připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="25eeb-129">You don't actually need to add the `MovieDBContext` connection string.</span></span> <span data-ttu-id="25eeb-130">Pokud nechcete zadat připojovací řetězec, Entity Framework se vytvoří databáze LocalDB v adresáři uživatele s plně kvalifikovaný název [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) třídy (v tomto případě `MvcMovie.Models.MovieDBContext`).</span><span class="sxs-lookup"><span data-stu-id="25eeb-130">If you don't specify a connection string, Entity Framework will create a LocalDB database in the users directory with the fully qualified name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class (in this case `MvcMovie.Models.MovieDBContext`).</span></span> <span data-ttu-id="25eeb-131">Název můžete databázi cokoli, co vám vyhovuje, za předpokladu, má *. MDF* příponu.</span><span class="sxs-lookup"><span data-stu-id="25eeb-131">You can name the database anything you like, as long as it has the *.MDF* suffix.</span></span> <span data-ttu-id="25eeb-132">Například společnost Microsoft mohla mít název databáze *MyFilms.mdf*.</span><span class="sxs-lookup"><span data-stu-id="25eeb-132">For example, we could name the database *MyFilms.mdf*.</span></span>

<span data-ttu-id="25eeb-133">V dalším kroku vytvoříte novou `MoviesController` třídu, která vám pomůže se zobrazí data o filmech a umožní uživatelům vytvářet nové video výpisy.</span><span class="sxs-lookup"><span data-stu-id="25eeb-133">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="25eeb-134">[Předchozí](adding-a-model.md)
> [další](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="25eeb-134">[Previous](adding-a-model.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
