---
uid: signalr/overview/older-versions/mapping-users-to-connections
title: "Mapování uživatelů SignalR na připojení v systému SignalR 1.x | Microsoft Docs"
author: pfletcher
description: "Toto téma ukazuje, jak uchovávat informace týkající se uživatelů a jejich připojení."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: ebbc93a8-e6c4-4122-8e0d-3aa42293c747
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 896bf4142ce090e39ed5697ff053cd56728318ed
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="mapping-signalr-users-to-connections-in-signalr-1x"></a><span data-ttu-id="72df7-103">Mapování uživatelů SignalR na připojení v systému SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="72df7-103">Mapping SignalR Users to Connections in SignalR 1.x</span></span>
====================
<span data-ttu-id="72df7-104">podle [Patrik Fletcher](https://github.com/pfletcher), [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="72df7-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="72df7-105">Toto téma ukazuje, jak uchovávat informace týkající se uživatelů a jejich připojení.</span><span class="sxs-lookup"><span data-stu-id="72df7-105">This topic shows how to retain information about users and their connections.</span></span>


## <a name="introduction"></a><span data-ttu-id="72df7-106">Úvod</span><span class="sxs-lookup"><span data-stu-id="72df7-106">Introduction</span></span>

<span data-ttu-id="72df7-107">Každý klient připojení k rozbočovači předá id jedinečný připojení. Můžete načíst tuto hodnotu v `Context.ConnectionId` vlastnost kontext rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="72df7-107">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="72df7-108">Pokud aplikace potřebuje zachovat mapování a mapování uživatele id připojení, můžete použít jednu z těchto možností:</span><span class="sxs-lookup"><span data-stu-id="72df7-108">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- <span data-ttu-id="72df7-109">[Úložiště v paměti](#inmemory), jako je například slovník</span><span class="sxs-lookup"><span data-stu-id="72df7-109">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="72df7-110">Skupiny SignalR pro každého uživatele</span><span class="sxs-lookup"><span data-stu-id="72df7-110">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="72df7-111">[Trvalé, externí úložiště](#database), jako jsou tabulky databáze nebo úložiště Azure table</span><span class="sxs-lookup"><span data-stu-id="72df7-111">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="72df7-112">Těchto jednotlivých instancí se zobrazuje v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="72df7-112">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="72df7-113">Můžete použít `OnConnected`, `OnDisconnected`, a `OnReconnected` metody `Hub` třídy ke sledování stavu připojení uživatele.</span><span class="sxs-lookup"><span data-stu-id="72df7-113">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="72df7-114">Nejlepší metodou pro vaši aplikaci, závisí na:</span><span class="sxs-lookup"><span data-stu-id="72df7-114">The best approach for your application depends on:</span></span>

- <span data-ttu-id="72df7-115">Počet webových serverů, který je hostitelem vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="72df7-115">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="72df7-116">Jestli chcete získat seznam aktuálně připojeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="72df7-116">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="72df7-117">Jestli je potřeba uchovávat informace, skupiny a uživatele po restartování aplikace nebo serveru.</span><span class="sxs-lookup"><span data-stu-id="72df7-117">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="72df7-118">Jestli latence volání externí server je problém.</span><span class="sxs-lookup"><span data-stu-id="72df7-118">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="72df7-119">Následující tabulka ukazuje, jaký přístup se dá použít pro tyto aspekty.</span><span class="sxs-lookup"><span data-stu-id="72df7-119">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="72df7-120">Více než jeden server</span><span class="sxs-lookup"><span data-stu-id="72df7-120">More than one server</span></span> | <span data-ttu-id="72df7-121">Získat seznam aktuálně připojených uživatelů</span><span class="sxs-lookup"><span data-stu-id="72df7-121">Get list of currently connected users</span></span> | <span data-ttu-id="72df7-122">Zachovat informace po restartování</span><span class="sxs-lookup"><span data-stu-id="72df7-122">Persist information after restarts</span></span> | <span data-ttu-id="72df7-123">Optimální výkon</span><span class="sxs-lookup"><span data-stu-id="72df7-123">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="72df7-124">V paměti</span><span class="sxs-lookup"><span data-stu-id="72df7-124">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="72df7-125">Skupiny jednoho uživatele</span><span class="sxs-lookup"><span data-stu-id="72df7-125">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="72df7-126">Trvalé, externí</span><span class="sxs-lookup"><span data-stu-id="72df7-126">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="72df7-127">Úložiště v paměti</span><span class="sxs-lookup"><span data-stu-id="72df7-127">In-memory storage</span></span>

<span data-ttu-id="72df7-128">Následující příklady ukazují, jak uchovávat informace o připojení a uživatele v slovník, který je uložen v paměti.</span><span class="sxs-lookup"><span data-stu-id="72df7-128">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="72df7-129">Adresář používá `HashSet` pro ukládání id připojení. Kdykoli uživatel může mít víc než jedno připojení k aplikaci SignalR.</span><span class="sxs-lookup"><span data-stu-id="72df7-129">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="72df7-130">Například uživatel, který je připojený prostřednictvím více zařízení nebo více než jedné karty prohlížeče by mít více než jeden id připojení.</span><span class="sxs-lookup"><span data-stu-id="72df7-130">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="72df7-131">Pokud aplikace ukončí, všechny informace dojde ke ztrátě, ale ho znovu vyplní podle uživatele znovu navázat připojení.</span><span class="sxs-lookup"><span data-stu-id="72df7-131">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="72df7-132">Úložiště v paměti nefunguje, pokud vaše prostředí zahrnuje více než jednom webovém serveru, protože každý server by mít samostatné kolekce připojení.</span><span class="sxs-lookup"><span data-stu-id="72df7-132">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="72df7-133">První příklad ukazuje třídu, která spravuje mapování mezi uživateli k připojení.</span><span class="sxs-lookup"><span data-stu-id="72df7-133">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="72df7-134">Klíč pro HashSet bude uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="72df7-134">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="72df7-135">Další příklad ukazuje způsob použití třídy mapování připojení od rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="72df7-135">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="72df7-136">Instance třídy je uložen v názvu proměnné `_connections`.</span><span class="sxs-lookup"><span data-stu-id="72df7-136">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="72df7-137">Skupiny jednoho uživatele</span><span class="sxs-lookup"><span data-stu-id="72df7-137">Single-user groups</span></span>

<span data-ttu-id="72df7-138">Můžete vytvořit skupinu pro každého uživatele a pak odešle zprávu do této skupiny, pokud chcete dosáhnout pouze tento uživatel.</span><span class="sxs-lookup"><span data-stu-id="72df7-138">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="72df7-139">Název každé skupiny je jméno uživatele.</span><span class="sxs-lookup"><span data-stu-id="72df7-139">The name of each group is the name of the user.</span></span> <span data-ttu-id="72df7-140">Pokud má uživatel více než jedno připojení, každý id připojení je přidán do skupiny uživatele.</span><span class="sxs-lookup"><span data-stu-id="72df7-140">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="72df7-141">Neměli ručně uživatele odeberete ze skupiny po odpojení.</span><span class="sxs-lookup"><span data-stu-id="72df7-141">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="72df7-142">Tato akce se provádí automaticky rámcem SignalR.</span><span class="sxs-lookup"><span data-stu-id="72df7-142">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="72df7-143">Následující příklad ukazuje, jak implementovat skupiny jednoho uživatele.</span><span class="sxs-lookup"><span data-stu-id="72df7-143">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="72df7-144">Trvalé, externí úložiště</span><span class="sxs-lookup"><span data-stu-id="72df7-144">Permanent, external storage</span></span>

<span data-ttu-id="72df7-145">Toto téma ukazuje, jak chcete použít pro ukládání informací o připojení databáze nebo úložiště tabulek Azure.</span><span class="sxs-lookup"><span data-stu-id="72df7-145">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="72df7-146">Tento přístup funguje, když máte více webových serverů, protože každý webový server může komunikovat s úložišti dat stejné.</span><span class="sxs-lookup"><span data-stu-id="72df7-146">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="72df7-147">Pokud vaše webové servery ukončete pracovní nebo se aplikace restartuje `OnDisconnected` metoda není volána.</span><span class="sxs-lookup"><span data-stu-id="72df7-147">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="72df7-148">Proto je možné, že úložiště dat budou mít záznamy pro ID připojení, které již nejsou platné.</span><span class="sxs-lookup"><span data-stu-id="72df7-148">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="72df7-149">Vyčistěte tyto osamocené záznamy, můžete zrušit platnost jakékoli připojení, která byla vytvořena mimo časový rámec, který je relevantní pro vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="72df7-149">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="72df7-150">Příklady v této části obsahují hodnotu pro sledování v okamžiku vytvoření připojení, ale nezobrazovat postup vyčištění staré záznamy, protože můžete chtít udělat jako proces na pozadí.</span><span class="sxs-lookup"><span data-stu-id="72df7-150">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="72df7-151">Databáze</span><span class="sxs-lookup"><span data-stu-id="72df7-151">Database</span></span>

<span data-ttu-id="72df7-152">Následující příklady ukazují, jak uchovávat informace o připojení a uživatele v databázi.</span><span class="sxs-lookup"><span data-stu-id="72df7-152">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="72df7-153">Můžete použít technologii přístup všech dat; Následující příklad ukazuje, jak definovat modely používající rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="72df7-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="72df7-154">Tyto modely entity odpovídají databáze tabulky a pole.</span><span class="sxs-lookup"><span data-stu-id="72df7-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="72df7-155">Datová struktura může značně lišit v závislosti na požadavcích vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="72df7-155">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="72df7-156">První příklad ukazuje, jak definovat entitu uživatele, který může být přidružen mnoho entit připojení.</span><span class="sxs-lookup"><span data-stu-id="72df7-156">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="72df7-157">Potom z centra, můžete sledovat stav každé připojení kódem vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="72df7-157">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a><span data-ttu-id="72df7-158">Úložiště tabulek Azure</span><span class="sxs-lookup"><span data-stu-id="72df7-158">Azure table storage</span></span>

<span data-ttu-id="72df7-159">Následující příklad Azure table storage je podobné jako v příkladu databáze.</span><span class="sxs-lookup"><span data-stu-id="72df7-159">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="72df7-160">Neobsahuje všechny informace, které by bylo nutné Začínáme s Azure Table Storage Service.</span><span class="sxs-lookup"><span data-stu-id="72df7-160">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="72df7-161">Informace najdete v tématu [postup používání úložiště Table z .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="72df7-161">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="72df7-162">Následující příklad ukazuje tabulka entity pro ukládání informací o připojení.</span><span class="sxs-lookup"><span data-stu-id="72df7-162">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="72df7-163">Oddíly data podle uživatelského jména a identifikuje každé entity podle id připojení, takže uživatel může mít víc připojení kdykoli.</span><span class="sxs-lookup"><span data-stu-id="72df7-163">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<span data-ttu-id="72df7-164">V centru sledovat stav připojení jednotlivých uživatelů.</span><span class="sxs-lookup"><span data-stu-id="72df7-164">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
