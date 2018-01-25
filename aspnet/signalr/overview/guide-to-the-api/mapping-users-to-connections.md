---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: "Mapování uživatelů SignalR na připojení | Microsoft Docs"
author: tfitzmac
description: "Toto téma ukazuje, jak uchovávat informace týkající se uživatelů a jejich připojení. Patrik Fletcher pomohl zápisu v tomto tématu. Použité v tomto tématu verze softwaru..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/30/2014
ms.topic: article
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: c4f95a3b65c57dd7cb7c5c7f1ee09daa17fa9616
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="mapping-signalr-users-to-connections"></a><span data-ttu-id="9ace9-105">Mapování uživatelů SignalR na připojení</span><span class="sxs-lookup"><span data-stu-id="9ace9-105">Mapping SignalR Users to Connections</span></span>
====================
<span data-ttu-id="9ace9-106">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="9ace9-106">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="9ace9-107">Toto téma ukazuje, jak uchovávat informace týkající se uživatelů a jejich připojení.</span><span class="sxs-lookup"><span data-stu-id="9ace9-107">This topic shows how to retain information about users and their connections.</span></span>
> 
> <span data-ttu-id="9ace9-108">Patrik Fletcher pomohl zápisu v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="9ace9-108">Patrick Fletcher helped write this topic.</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="9ace9-109">Verze softwaru použitým v tomto tématu</span><span class="sxs-lookup"><span data-stu-id="9ace9-109">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="9ace9-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="9ace9-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="9ace9-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="9ace9-111">.NET 4.5</span></span>
> - <span data-ttu-id="9ace9-112">SignalR verze 2</span><span class="sxs-lookup"><span data-stu-id="9ace9-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="9ace9-113">Předchozí verze tohoto tématu</span><span class="sxs-lookup"><span data-stu-id="9ace9-113">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="9ace9-114">Informace o předchozích verzích SignalR najdete v tématu [starší verze funkce SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="9ace9-114">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="9ace9-115">Dotazy a připomínky</span><span class="sxs-lookup"><span data-stu-id="9ace9-115">Questions and comments</span></span>
> 
> <span data-ttu-id="9ace9-116">Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="9ace9-116">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="9ace9-117">Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="9ace9-117">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="introduction"></a><span data-ttu-id="9ace9-118">Úvod</span><span class="sxs-lookup"><span data-stu-id="9ace9-118">Introduction</span></span>

<span data-ttu-id="9ace9-119">Každý klient připojení k rozbočovači předá id jedinečný připojení. Můžete načíst tuto hodnotu v `Context.ConnectionId` vlastnost kontext rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="9ace9-119">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="9ace9-120">Pokud aplikace potřebuje zachovat mapování a mapování uživatele id připojení, můžete použít jednu z těchto možností:</span><span class="sxs-lookup"><span data-stu-id="9ace9-120">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- [<span data-ttu-id="9ace9-121">Uživatelské ID zprostředkovatele (SignalR 2)</span><span class="sxs-lookup"><span data-stu-id="9ace9-121">The User ID Provider (SignalR 2)</span></span>](#IUserIdProvider)
- <span data-ttu-id="9ace9-122">[Úložiště v paměti](#inmemory), jako je například slovník</span><span class="sxs-lookup"><span data-stu-id="9ace9-122">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="9ace9-123">Skupiny SignalR pro každého uživatele</span><span class="sxs-lookup"><span data-stu-id="9ace9-123">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="9ace9-124">[Trvalé, externí úložiště](#database), jako jsou tabulky databáze nebo úložiště Azure table</span><span class="sxs-lookup"><span data-stu-id="9ace9-124">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="9ace9-125">Těchto jednotlivých instancí se zobrazuje v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="9ace9-125">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="9ace9-126">Můžete použít `OnConnected`, `OnDisconnected`, a `OnReconnected` metody `Hub` třídy ke sledování stavu připojení uživatele.</span><span class="sxs-lookup"><span data-stu-id="9ace9-126">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="9ace9-127">Nejlepší metodou pro vaši aplikaci, závisí na:</span><span class="sxs-lookup"><span data-stu-id="9ace9-127">The best approach for your application depends on:</span></span>

- <span data-ttu-id="9ace9-128">Počet webových serverů, který je hostitelem vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ace9-128">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="9ace9-129">Jestli chcete získat seznam aktuálně připojeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="9ace9-129">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="9ace9-130">Jestli je potřeba uchovávat informace, skupiny a uživatele po restartování aplikace nebo serveru.</span><span class="sxs-lookup"><span data-stu-id="9ace9-130">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="9ace9-131">Jestli latence volání externí server je problém.</span><span class="sxs-lookup"><span data-stu-id="9ace9-131">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="9ace9-132">Následující tabulka ukazuje, jaký přístup se dá použít pro tyto aspekty.</span><span class="sxs-lookup"><span data-stu-id="9ace9-132">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="9ace9-133">Více než jeden server</span><span class="sxs-lookup"><span data-stu-id="9ace9-133">More than one server</span></span> | <span data-ttu-id="9ace9-134">Získat seznam aktuálně připojených uživatelů</span><span class="sxs-lookup"><span data-stu-id="9ace9-134">Get list of currently connected users</span></span> | <span data-ttu-id="9ace9-135">Zachovat informace po restartování</span><span class="sxs-lookup"><span data-stu-id="9ace9-135">Persist information after restarts</span></span> | <span data-ttu-id="9ace9-136">Optimální výkon</span><span class="sxs-lookup"><span data-stu-id="9ace9-136">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="9ace9-137">ID uživatele zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="9ace9-137">UserID Provider</span></span> | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="9ace9-138">V paměti</span><span class="sxs-lookup"><span data-stu-id="9ace9-138">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="9ace9-139">Skupiny jednoho uživatele</span><span class="sxs-lookup"><span data-stu-id="9ace9-139">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| <span data-ttu-id="9ace9-140">Trvalé, externí</span><span class="sxs-lookup"><span data-stu-id="9ace9-140">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a><span data-ttu-id="9ace9-141">IUserID zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="9ace9-141">IUserID provider</span></span>

<span data-ttu-id="9ace9-142">Tato funkce umožňuje uživatelům určit, co je ID uživatele podle IRequest prostřednictvím nové rozhraní IUserIdProvider.</span><span class="sxs-lookup"><span data-stu-id="9ace9-142">This feature allows users to specify what the userId is based on an IRequest via a new interface IUserIdProvider.</span></span>

<span data-ttu-id="9ace9-143">**IUserIdProvider**</span><span class="sxs-lookup"><span data-stu-id="9ace9-143">**The IUserIdProvider**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="9ace9-144">Ve výchozím nastavení, bude implementace, která používá uživatele `IPrincipal.Identity.Name` jako uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="9ace9-144">By default, there will be an implementation that uses the user's `IPrincipal.Identity.Name` as the user name.</span></span> <span data-ttu-id="9ace9-145">Chcete-li toto nastavení změnit, zaregistrujte vaši implementaci `IUserIdProvider` s globální hostitelem při spuštění aplikace:</span><span class="sxs-lookup"><span data-stu-id="9ace9-145">To change this, register your implementation of `IUserIdProvider` with the global host when your application starts:</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<span data-ttu-id="9ace9-146">Z v rozbočovači, budete moct odesílat zprávy pro tyto uživatele prostřednictvím rozhraní API následující:</span><span class="sxs-lookup"><span data-stu-id="9ace9-146">From within a hub, you'll be able to send messages to these users via the following API:</span></span>

<span data-ttu-id="9ace9-147">**Odesílání zprávy pro konkrétního uživatele**</span><span class="sxs-lookup"><span data-stu-id="9ace9-147">**Sending a message to a specific user**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="9ace9-148">Úložiště v paměti</span><span class="sxs-lookup"><span data-stu-id="9ace9-148">In-memory storage</span></span>

<span data-ttu-id="9ace9-149">Následující příklady ukazují, jak uchovávat informace o připojení a uživatele v slovník, který je uložen v paměti.</span><span class="sxs-lookup"><span data-stu-id="9ace9-149">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="9ace9-150">Adresář používá `HashSet` pro ukládání id připojení. Kdykoli uživatel může mít víc než jedno připojení k aplikaci SignalR.</span><span class="sxs-lookup"><span data-stu-id="9ace9-150">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="9ace9-151">Například uživatel, který je připojený prostřednictvím více zařízení nebo více než jedné karty prohlížeče by mít více než jeden id připojení.</span><span class="sxs-lookup"><span data-stu-id="9ace9-151">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="9ace9-152">Pokud aplikace ukončí, všechny informace dojde ke ztrátě, ale ho znovu vyplní podle uživatele znovu navázat připojení.</span><span class="sxs-lookup"><span data-stu-id="9ace9-152">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="9ace9-153">Úložiště v paměti nefunguje, pokud vaše prostředí zahrnuje více než jednom webovém serveru, protože každý server by mít samostatné kolekce připojení.</span><span class="sxs-lookup"><span data-stu-id="9ace9-153">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="9ace9-154">První příklad ukazuje třídu, která spravuje mapování mezi uživateli k připojení.</span><span class="sxs-lookup"><span data-stu-id="9ace9-154">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="9ace9-155">Klíč pro HashSet bude uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="9ace9-155">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="9ace9-156">Další příklad ukazuje způsob použití třídy mapování připojení od rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="9ace9-156">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="9ace9-157">Instance třídy je uložen v názvu proměnné `_connections`.</span><span class="sxs-lookup"><span data-stu-id="9ace9-157">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="9ace9-158">Skupiny jednoho uživatele</span><span class="sxs-lookup"><span data-stu-id="9ace9-158">Single-user groups</span></span>

<span data-ttu-id="9ace9-159">Můžete vytvořit skupinu pro každého uživatele a pak odešle zprávu do této skupiny, pokud chcete dosáhnout pouze tento uživatel.</span><span class="sxs-lookup"><span data-stu-id="9ace9-159">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="9ace9-160">Název každé skupiny je jméno uživatele.</span><span class="sxs-lookup"><span data-stu-id="9ace9-160">The name of each group is the name of the user.</span></span> <span data-ttu-id="9ace9-161">Pokud má uživatel více než jedno připojení, každý id připojení je přidán do skupiny uživatele.</span><span class="sxs-lookup"><span data-stu-id="9ace9-161">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="9ace9-162">Neměli ručně uživatele odeberete ze skupiny po odpojení.</span><span class="sxs-lookup"><span data-stu-id="9ace9-162">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="9ace9-163">Tato akce se provádí automaticky rámcem SignalR.</span><span class="sxs-lookup"><span data-stu-id="9ace9-163">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="9ace9-164">Následující příklad ukazuje, jak implementovat skupiny jednoho uživatele.</span><span class="sxs-lookup"><span data-stu-id="9ace9-164">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="9ace9-165">Trvalé, externí úložiště</span><span class="sxs-lookup"><span data-stu-id="9ace9-165">Permanent, external storage</span></span>

<span data-ttu-id="9ace9-166">Toto téma ukazuje, jak chcete použít pro ukládání informací o připojení databáze nebo úložiště tabulek Azure.</span><span class="sxs-lookup"><span data-stu-id="9ace9-166">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="9ace9-167">Tento přístup funguje, když máte více webových serverů, protože každý webový server může komunikovat s úložišti dat stejné.</span><span class="sxs-lookup"><span data-stu-id="9ace9-167">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="9ace9-168">Pokud vaše webové servery ukončete pracovní nebo se aplikace restartuje `OnDisconnected` metoda není volána.</span><span class="sxs-lookup"><span data-stu-id="9ace9-168">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="9ace9-169">Proto je možné, že úložiště dat budou mít záznamy pro ID připojení, které již nejsou platné.</span><span class="sxs-lookup"><span data-stu-id="9ace9-169">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="9ace9-170">Vyčistěte tyto osamocené záznamy, můžete zrušit platnost jakékoli připojení, která byla vytvořena mimo časový rámec, který je relevantní pro vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ace9-170">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="9ace9-171">Příklady v této části obsahují hodnotu pro sledování v okamžiku vytvoření připojení, ale nezobrazovat postup vyčištění staré záznamy, protože můžete chtít udělat jako proces na pozadí.</span><span class="sxs-lookup"><span data-stu-id="9ace9-171">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="9ace9-172">Databáze</span><span class="sxs-lookup"><span data-stu-id="9ace9-172">Database</span></span>

<span data-ttu-id="9ace9-173">Následující příklady ukazují, jak uchovávat informace o připojení a uživatele v databázi.</span><span class="sxs-lookup"><span data-stu-id="9ace9-173">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="9ace9-174">Můžete použít technologii přístup všech dat; Následující příklad ukazuje, jak definovat modely používající rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9ace9-174">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="9ace9-175">Tyto modely entity odpovídají databáze tabulky a pole.</span><span class="sxs-lookup"><span data-stu-id="9ace9-175">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="9ace9-176">Datová struktura může značně lišit v závislosti na požadavcích vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ace9-176">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="9ace9-177">První příklad ukazuje, jak definovat entitu uživatele, který může být přidružen mnoho entit připojení.</span><span class="sxs-lookup"><span data-stu-id="9ace9-177">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

<span data-ttu-id="9ace9-178">Potom z centra, můžete sledovat stav každé připojení kódem vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="9ace9-178">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a><span data-ttu-id="9ace9-179">Úložiště tabulek Azure</span><span class="sxs-lookup"><span data-stu-id="9ace9-179">Azure table storage</span></span>

<span data-ttu-id="9ace9-180">Následující příklad Azure table storage je podobné jako v příkladu databáze.</span><span class="sxs-lookup"><span data-stu-id="9ace9-180">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="9ace9-181">Neobsahuje všechny informace, které by bylo nutné Začínáme s Azure Table Storage Service.</span><span class="sxs-lookup"><span data-stu-id="9ace9-181">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="9ace9-182">Informace najdete v tématu [postup používání úložiště Table z .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="9ace9-182">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="9ace9-183">Následující příklad ukazuje tabulka entity pro ukládání informací o připojení.</span><span class="sxs-lookup"><span data-stu-id="9ace9-183">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="9ace9-184">Oddíly data podle uživatelského jména a identifikuje každé entity podle id připojení, takže uživatel může mít víc připojení kdykoli.</span><span class="sxs-lookup"><span data-stu-id="9ace9-184">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

<span data-ttu-id="9ace9-185">V centru sledovat stav připojení jednotlivých uživatelů.</span><span class="sxs-lookup"><span data-stu-id="9ace9-185">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
