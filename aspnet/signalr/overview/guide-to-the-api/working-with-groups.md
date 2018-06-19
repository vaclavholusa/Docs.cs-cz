---
uid: signalr/overview/guide-to-the-api/working-with-groups
title: Práce se skupinami v systému SignalR | Microsoft Docs
author: pfletcher
description: Toto téma popisuje, jak uchovávat informace o členství ve skupině s rozhraním API rozbočovače.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: cd378ecd-3e9e-4236-b902-65916d85a048
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 11f5be1ac4e74b692f0db3daac971a2c9d74a64c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
ms.locfileid: "28042219"
---
<a name="working-with-groups-in-signalr"></a><span data-ttu-id="cf7d5-103">Práce se skupinami v systému SignalR</span><span class="sxs-lookup"><span data-stu-id="cf7d5-103">Working with Groups in SignalR</span></span>
====================
<span data-ttu-id="cf7d5-104">podle [Patrik Fletcher](https://github.com/pfletcher), [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="cf7d5-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="cf7d5-105">Toto téma popisuje postup přidání uživatelů do skupiny a zachovat informace o členství ve skupině.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-105">This topic describes how to add users to groups and persist group membership information.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="cf7d5-106">Verze softwaru použitým v tomto tématu</span><span class="sxs-lookup"><span data-stu-id="cf7d5-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="cf7d5-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="cf7d5-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="cf7d5-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="cf7d5-108">.NET 4.5</span></span>
> - <span data-ttu-id="cf7d5-109">SignalR verze 2</span><span class="sxs-lookup"><span data-stu-id="cf7d5-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="cf7d5-110">Předchozí verze tohoto tématu</span><span class="sxs-lookup"><span data-stu-id="cf7d5-110">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="cf7d5-111">Informace o předchozích verzích SignalR najdete v tématu [starší verze funkce SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="cf7d5-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="cf7d5-112">Dotazy a připomínky</span><span class="sxs-lookup"><span data-stu-id="cf7d5-112">Questions and comments</span></span>
> 
> <span data-ttu-id="cf7d5-113">Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="cf7d5-114">Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="cf7d5-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="cf7d5-115">Přehled</span><span class="sxs-lookup"><span data-stu-id="cf7d5-115">Overview</span></span>

<span data-ttu-id="cf7d5-116">Skupiny v systému SignalR poskytují metodu pro zprávy všesměrové vysílání pro zadaný podmnožiny připojených klientů.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-116">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="cf7d5-117">Skupina může mít libovolný počet klientů a klienta může být členem skupiny libovolný počet skupin.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-117">A group can have any number of clients, and a client can be a member of any number of groups.</span></span> <span data-ttu-id="cf7d5-118">Nemáte nemusel vytvářet skupiny.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-118">You don't have to explicitly create groups.</span></span> <span data-ttu-id="cf7d5-119">Ve skutečnosti skupina se automaticky vytvoří při prvním zadejte její název ve volání Groups.Add a je odstraněn, když odeberete poslední připojení z členství v ní.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-119">In effect, a group is automatically created the first time you specify its name in a call to Groups.Add, and it is deleted when you remove the last connection from membership in it.</span></span> <span data-ttu-id="cf7d5-120">Úvod do používání skupin, najdete v části [Správa členství ve skupině z třídy rozbočovače](hubs-api-guide-server.md#groupsfromhub) v rozhraní API centra – Příručka pro Server.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-120">For an introduction to using groups, see [How to manage group membership from the Hub class](hubs-api-guide-server.md#groupsfromhub) in the Hubs API - Server Guide.</span></span>

<span data-ttu-id="cf7d5-121">Neexistuje žádné rozhraní API pro získání seznamu členství ve skupinách nebo seznam skupin.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-121">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="cf7d5-122">SignalR odešle zprávy do klientů a skupin založené na modelu pub nebo sub a serveru neumožňuje spravovat seznam skupin nebo členství ve skupinách.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-122">SignalR sends messages to clients and groups based on a pub/sub model, and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="cf7d5-123">To pomáhá maximalizovat škálovatelnost, protože při každém přidání uzlu do webové farmy, nějaký stav, který udržuje SignalR musí být rozšířena do nového uzlu.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-123">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<span data-ttu-id="cf7d5-124">Při přidávání uživatele do skupiny pomocí `Groups.Add` metoda, uživatel obdrží zprávy směrované do této skupiny pro dobu trvání aktuálního připojení, ale není trvalý členství uživatele v dané skupině mimo aktuální připojení.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-124">When you add a user to a group using the `Groups.Add` method, the user receives messages directed to that group for the duration of the current connection, but the user's membership in that group is not persisted beyond the current connection.</span></span> <span data-ttu-id="cf7d5-125">Pokud chcete trvale uchovávat informace o skupinách a členství ve skupině, je třeba uložit data v úložišti, jako je databáze nebo úložiště tabulek Azure.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-125">If you want to permanently retain information about groups and group membership, you must store that data in a repository such as a database or Azure table storage.</span></span> <span data-ttu-id="cf7d5-126">Potom pokaždé, když uživatel připojí k vaší aplikaci, můžete načíst z úložiště, které skupiny, které uživatel patří do a ručně přidejte tohoto uživatele do těchto skupin.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-126">Then, each time a user connects to your application, you retrieve from the repository which groups the user belongs to, and manually add that user to those groups.</span></span>

<span data-ttu-id="cf7d5-127">Když znovu obnovovat po dočasné přerušení, uživatel automaticky znovu připojí dřív přiřazené skupiny.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-127">When reconnecting after a temporary disruption, the user automatically re-joins the previously-assigned groups.</span></span> <span data-ttu-id="cf7d5-128">Automaticky opětovné připojování ke skupině platí pouze při opětovné připojení, není při vytvoření nového připojení.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-128">Automatically rejoining a group only applies when reconnecting, not when establishing a new connection.</span></span> <span data-ttu-id="cf7d5-129">Token digitálně podepsané je předán z klienta, který obsahuje seznam dřív přiřazené skupin.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-129">A digitally-signed token is passed from the client that contains the list of previously-assigned groups.</span></span> <span data-ttu-id="cf7d5-130">Pokud chcete ověřit, zda uživatel patří do požadované skupiny, můžete přepsat výchozí chování.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-130">If you want to verify whether the user belongs to the requested groups, you can override the default behavior.</span></span>

<span data-ttu-id="cf7d5-131">Toto téma obsahuje následující části:</span><span class="sxs-lookup"><span data-stu-id="cf7d5-131">This topic includes the following sections:</span></span>

- [<span data-ttu-id="cf7d5-132">Přidávání a odebírání uživatelů</span><span class="sxs-lookup"><span data-stu-id="cf7d5-132">Adding and removing users</span></span>](#add)
- [<span data-ttu-id="cf7d5-133">Volání metody členové skupiny</span><span class="sxs-lookup"><span data-stu-id="cf7d5-133">Calling members of a group</span></span>](#call)
- [<span data-ttu-id="cf7d5-134">Ukládání do databáze členství ve skupině</span><span class="sxs-lookup"><span data-stu-id="cf7d5-134">Storing group membership in a database</span></span>](#storedatabase)
- [<span data-ttu-id="cf7d5-135">Ukládání členství ve skupině ve službě Azure table storage</span><span class="sxs-lookup"><span data-stu-id="cf7d5-135">Storing group membership in Azure table storage</span></span>](#storeazuretable)
- [<span data-ttu-id="cf7d5-136">Ověření členství ve skupině při opětovném připojení</span><span class="sxs-lookup"><span data-stu-id="cf7d5-136">Verifying group membership when reconnecting</span></span>](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a><span data-ttu-id="cf7d5-137">Přidávání a odebírání uživatelů</span><span class="sxs-lookup"><span data-stu-id="cf7d5-137">Adding and removing users</span></span>

<span data-ttu-id="cf7d5-138">Chcete-li přidat nebo odebrat uživatele ze skupiny, zavolejte [přidat](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) nebo [odebrat](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) metody a předejte jí id uživatele připojení a názvu skupiny jako parametry.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-138">To add or remove users from a group, you call the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) or [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods, and pass in the user's connection id and group's name as parameters.</span></span> <span data-ttu-id="cf7d5-139">Nemusíte ručně odeberte uživatele ze skupiny po ukončení připojení.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-139">You do not need to manually remove a user from a group when the connection ends.</span></span>

<span data-ttu-id="cf7d5-140">Následující příklad ukazuje `Groups.Add` a `Groups.Remove` metody používané v metodách rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-140">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

<span data-ttu-id="cf7d5-141">`Groups.Add` a `Groups.Remove` metody provedení asynchronně.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-141">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span>

<span data-ttu-id="cf7d5-142">Pokud chcete okamžitě odeslání zprávy do klienta pomocí skupiny a přidání klienta do skupiny, budete muset zajistěte, aby nejprve dokončí metodu Groups.Add.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-142">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the Groups.Add method finishes first.</span></span> <span data-ttu-id="cf7d5-143">Následující příklady kódu ukazují, jak to udělat.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-143">The following code examples show how to do that.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

<span data-ttu-id="cf7d5-144">Obecně platí, by neměla zahrnovat `await` při volání metody `Groups.Remove` metoda protože id připojení, který se pokoušíte odebrat může nadále již nebudou dostupné.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-144">In general, you should not include `await` when calling the `Groups.Remove` method because the connection id that you are trying to remove might no longer be available.</span></span> <span data-ttu-id="cf7d5-145">V takovém případě `TaskCanceledException` je vyvolána po vypršením časového limitu požadavku. Pokud vaše aplikace musíte zajistit, uživatel byl odebrán ze skupiny před odesláním zprávy do skupiny, můžete přidat `await` před Groups.Remove a pak catch `TaskCanceledException` výjimka, která mohou být vyvolány.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-145">In that case, `TaskCanceledException` is thrown after the request times out. If your application must ensure that the user has been removed from the group before sending a message to the group, you can add `await` before Groups.Remove, and then catch the `TaskCanceledException` exception that might be thrown.</span></span>

<a id="call"></a>

## <a name="calling-members-of-a-group"></a><span data-ttu-id="cf7d5-146">Volání metody členové skupiny</span><span class="sxs-lookup"><span data-stu-id="cf7d5-146">Calling members of a group</span></span>

<span data-ttu-id="cf7d5-147">Mohou zasílat zprávy do všech členů skupiny nebo pouze členové zadané skupiny, jak je znázorněno v následujících příkladech.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-147">You can send messages to all of the members of a group or only specified members of the group, as shown in the following examples.</span></span>

- <span data-ttu-id="cf7d5-148">**Všechny** připojených klientů v zadané skupině.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-148">**All** connected clients in a specified group.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample3.css)]
- <span data-ttu-id="cf7d5-149">Všichni připojení klienti v zadané skupině **s výjimkou zadaných klientů**, podle ID připojení.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-149">All connected clients in a specified group **except the specified clients**, identified by connection ID.</span></span> 

    [!code-csharp[Main](working-with-groups/samples/sample4.cs)]
- <span data-ttu-id="cf7d5-150">Všichni připojení klienti v zadané skupině **s výjimkou volajícího klienta**.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-150">All connected clients in a specified group **except the calling client**.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample5.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a><span data-ttu-id="cf7d5-151">Ukládání do databáze členství ve skupině</span><span class="sxs-lookup"><span data-stu-id="cf7d5-151">Storing group membership in a database</span></span>

<span data-ttu-id="cf7d5-152">Následující příklady ukazují, jak uchovávat informace skupiny a uživatele v databázi.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-152">The following examples show how to retain group and user information in a database.</span></span> <span data-ttu-id="cf7d5-153">Můžete použít technologii přístup všech dat; Následující příklad ukazuje, jak definovat modely používající rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="cf7d5-154">Tyto modely entity odpovídají databáze tabulky a pole.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="cf7d5-155">Datová struktura může značně lišit v závislosti na požadavcích vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-155">Your data structure could vary considerably depending on the requirements of your application.</span></span> <span data-ttu-id="cf7d5-156">Tento příklad obsahuje třídu s názvem `ConversationRoom` který bude pro aplikaci, která umožňuje uživatelům připojit konverzace o různých tématům, jako je například sportu nebo zahrady jedinečné.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-156">This example includes a class named `ConversationRoom` which would be unique to an application that enables users to join conversations about different subjects, such as sports or gardening.</span></span> <span data-ttu-id="cf7d5-157">Tento příklad také obsahuje třídu pro připojení.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-157">This example also includes a class for the connections.</span></span> <span data-ttu-id="cf7d5-158">Třída připojení není nezbytně nutné pro sledování členství ve skupině, ale často je součástí robustní řešení ke sledování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-158">The connection class is not absolutely required for tracking group membership but is frequently part of robust solution to tracking users.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample6.cs)]

<span data-ttu-id="cf7d5-159">Potom v rozbočovači, můžete načíst informace o skupiny a uživatele z databáze a ručně přidejte uživatele do příslušných skupin.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-159">Then, in the hub, you can retrieve the group and user information from the database and manually add the user to the appropriate groups.</span></span> <span data-ttu-id="cf7d5-160">V příkladu nezahrnuje kód pro sledování připojení uživatelů.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-160">The example does not include code for tracking the user connections.</span></span> <span data-ttu-id="cf7d5-161">V tomto příkladu `await` – klíčové slovo není použita před `Groups.Add` vzhledem k tomu, že členové skupiny není okamžitě odeslána zpráva.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-161">In this example, the `await` keyword is not applied before `Groups.Add` because a message is not immediately sent to members of the group.</span></span> <span data-ttu-id="cf7d5-162">Pokud chcete odeslat zprávu na všechny členy skupiny okamžitě po přidání nového člena, chcete použít `await` – klíčové slovo a ujistěte se, asynchronní operace byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-162">If you want to send a message to all members of the group immediately after adding the new member, you would want to apply the `await` keyword to make sure the asynchronous operation has completed.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a><span data-ttu-id="cf7d5-163">Ukládání členství ve skupině ve službě Azure table storage</span><span class="sxs-lookup"><span data-stu-id="cf7d5-163">Storing group membership in Azure table storage</span></span>

<span data-ttu-id="cf7d5-164">Použití úložiště tabulek Azure k ukládání informací skupiny a uživatele je podobný používání databáze.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-164">Using Azure table storage to store group and user information is similar to using a database.</span></span> <span data-ttu-id="cf7d5-165">Následující příklad ukazuje entitu tabulky, která ukládá uživatelské jméno a název skupiny.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-165">The following example shows a table entity that stores the user name and group name.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<span data-ttu-id="cf7d5-166">V centru je načíst přiřazené skupiny, když se uživatel připojí.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-166">In the hub, you retrieve the assigned groups when the user connects.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a><span data-ttu-id="cf7d5-167">Ověření členství ve skupině při opětovném připojení</span><span class="sxs-lookup"><span data-stu-id="cf7d5-167">Verifying group membership when reconnecting</span></span>

<span data-ttu-id="cf7d5-168">Ve výchozím nastavení SignalR automaticky znovu přiřadí uživatele do příslušných skupin po opětovném připojení z dočasné přerušení, jako je například když je připojení vyřazen a znovu vytvořit předtím, než vyprší časový limit připojení. Informace o skupině uživatele je předán do tokenu při opětovné připojení, a tento token se ověřuje na serveru.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-168">By default, SignalR automatically re-assigns a user to the appropriate groups when reconnecting from a temporary disruption, such as when a connection is dropped and re-established before the connection times out. The user's group information is passed in a token when reconnecting, and that token is verified on the server.</span></span> <span data-ttu-id="cf7d5-169">Informace o procesu ověření pro opětovné připojování ke skupinám uživatelů najdete v tématu [při opětovné připojení, opětovné připojování ke skupinám](../security/introduction-to-security.md#rejoingroup).</span><span class="sxs-lookup"><span data-stu-id="cf7d5-169">For information about the verification process for rejoining users to groups, see [Rejoining groups when reconnecting](../security/introduction-to-security.md#rejoingroup).</span></span>

<span data-ttu-id="cf7d5-170">Obecně byste měli používat výchozí chování automaticky opětovné připojování ke, že skupin v připojení.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-170">In general, you should use the default behavior of automatically rejoining groups on reconnect.</span></span> <span data-ttu-id="cf7d5-171">SignalR skupiny nejsou určeny jako bezpečnostní mechanismus pro omezení přístupu k citlivým datům.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-171">SignalR groups are not intended as a security mechanism for restricting access to sensitive data.</span></span> <span data-ttu-id="cf7d5-172">Pokud vaše aplikace musí Zkontrolujte členství ve skupině uživatele při opětovné připojení, ale můžete přepsat výchozí chování.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-172">However, if your application must double-check a user's group membership when reconnecting, you can override the default behavior.</span></span> <span data-ttu-id="cf7d5-173">Změna výchozí chování můžete přidat zatížení k vaší databázi protože členství ve skupině uživatele musí být načtena pro každý opětovné připojení, ne jenom když se uživatel připojí.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-173">Changing the default behavior can add a burden to your database because a user's group membership must be retrieved for each reconnection rather than just when the user connects.</span></span>

<span data-ttu-id="cf7d5-174">Pokud členství ve skupině musí ověřit na znovu připojit, vytvořte nový modul kanálu rozbočovače, který vrátí seznam hodnot přiřazených skupin, jak vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-174">If you must verify group membership on reconnect, create a new hub pipeline module that returns a list of assigned groups, as shown below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<span data-ttu-id="cf7d5-175">Tento modul potom přidáte do kanálu rozbočovače, jak je znázorněno dole.</span><span class="sxs-lookup"><span data-stu-id="cf7d5-175">Then, add that module to the hub pipeline, as highlighted below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample11.cs?highlight=4)]
