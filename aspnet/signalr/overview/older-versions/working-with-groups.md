---
uid: signalr/overview/older-versions/working-with-groups
title: "Práce se skupinami v systému SignalR 1.x | Microsoft Docs"
author: pfletcher
description: "Toto téma popisuje, jak uchovávat informace o členství ve skupině s rozhraním API rozbočovače."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: 22929efd-68c9-4609-b76d-f8ba42fda01e
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 04da74f23663313e70e54fd4f2f9e5f005791cff
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="working-with-groups-in-signalr-1x"></a><span data-ttu-id="59230-103">Práce se skupinami v systému SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="59230-103">Working with Groups in SignalR 1.x</span></span>
====================
<span data-ttu-id="59230-104">podle [Patrik Fletcher](https://github.com/pfletcher), [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="59230-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="59230-105">Toto téma popisuje postup přidání uživatelů do skupiny a zachovat informace o členství ve skupině.</span><span class="sxs-lookup"><span data-stu-id="59230-105">This topic describes how to add users to groups and persist group membership information.</span></span>


## <a name="overview"></a><span data-ttu-id="59230-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="59230-106">Overview</span></span>

<span data-ttu-id="59230-107">Skupiny v systému SignalR poskytují metodu pro zprávy všesměrové vysílání pro zadaný podmnožiny připojených klientů.</span><span class="sxs-lookup"><span data-stu-id="59230-107">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="59230-108">Skupina může mít libovolný počet klientů a klienta může být členem skupiny libovolný počet skupin.</span><span class="sxs-lookup"><span data-stu-id="59230-108">A group can have any number of clients, and a client can be a member of any number of groups.</span></span> <span data-ttu-id="59230-109">Nemáte nemusel vytvářet skupiny.</span><span class="sxs-lookup"><span data-stu-id="59230-109">You don't have to explicitly create groups.</span></span> <span data-ttu-id="59230-110">Ve skutečnosti skupina se automaticky vytvoří při prvním zadejte její název ve volání Groups.Add a je odstraněn, když odeberete poslední připojení z členství v ní.</span><span class="sxs-lookup"><span data-stu-id="59230-110">In effect, a group is automatically created the first time you specify its name in a call to Groups.Add, and it is deleted when you remove the last connection from membership in it.</span></span> <span data-ttu-id="59230-111">Úvod do používání skupin, najdete v části [Správa členství ve skupině z třídy rozbočovače](index.md) v rozhraní API centra – Příručka pro Server.</span><span class="sxs-lookup"><span data-stu-id="59230-111">For an introduction to using groups, see [How to manage group membership from the Hub class](index.md) in the Hubs API - Server Guide.</span></span>

<span data-ttu-id="59230-112">Neexistuje žádné rozhraní API pro získání seznamu členství ve skupinách nebo seznam skupin.</span><span class="sxs-lookup"><span data-stu-id="59230-112">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="59230-113">SignalR odešle zprávy do klientů a skupin založené na modelu pub nebo sub a serveru neumožňuje spravovat seznam skupin nebo členství ve skupinách.</span><span class="sxs-lookup"><span data-stu-id="59230-113">SignalR sends messages to clients and groups based on a pub/sub model, and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="59230-114">To pomáhá maximalizovat škálovatelnost, protože při každém přidání uzlu do webové farmy, nějaký stav, který udržuje SignalR musí být rozšířena do nového uzlu.</span><span class="sxs-lookup"><span data-stu-id="59230-114">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<span data-ttu-id="59230-115">Při přidávání uživatele do skupiny pomocí `Groups.Add` metoda, uživatel obdrží zprávy směrované do této skupiny pro dobu trvání aktuálního připojení, ale není trvalý členství uživatele v dané skupině mimo aktuální připojení.</span><span class="sxs-lookup"><span data-stu-id="59230-115">When you add a user to a group using the `Groups.Add` method, the user receives messages directed to that group for the duration of the current connection, but the user's membership in that group is not persisted beyond the current connection.</span></span> <span data-ttu-id="59230-116">Pokud chcete trvale uchovávat informace o skupinách a členství ve skupině, je třeba uložit data v úložišti, jako je databáze nebo úložiště tabulek Azure.</span><span class="sxs-lookup"><span data-stu-id="59230-116">If you want to permanently retain information about groups and group membership, you must store that data in a repository such as a database or Azure table storage.</span></span> <span data-ttu-id="59230-117">Potom pokaždé, když uživatel připojí k vaší aplikaci, můžete načíst z úložiště, které skupiny, které uživatel patří do a ručně přidejte tohoto uživatele do těchto skupin.</span><span class="sxs-lookup"><span data-stu-id="59230-117">Then, each time a user connects to your application, you retrieve from the repository which groups the user belongs to, and manually add that user to those groups.</span></span>

<span data-ttu-id="59230-118">Když znovu obnovovat po dočasné přerušení, uživatel automaticky znovu připojí dřív přiřazené skupiny.</span><span class="sxs-lookup"><span data-stu-id="59230-118">When reconnecting after a temporary disruption, the user automatically re-joins the previously-assigned groups.</span></span> <span data-ttu-id="59230-119">Automaticky opětovné připojování ke skupině platí pouze při opětovné připojení, není při vytvoření nového připojení.</span><span class="sxs-lookup"><span data-stu-id="59230-119">Automatically rejoining a group only applies when reconnecting, not when establishing a new connection.</span></span> <span data-ttu-id="59230-120">Token digitálně podepsané je předán z klienta, který obsahuje seznam dřív přiřazené skupin.</span><span class="sxs-lookup"><span data-stu-id="59230-120">A digitally-signed token is passed from the client that contains the list of previously-assigned groups.</span></span> <span data-ttu-id="59230-121">Pokud chcete ověřit, zda uživatel patří do požadované skupiny, můžete přepsat výchozí chování.</span><span class="sxs-lookup"><span data-stu-id="59230-121">If you want to verify whether the user belongs to the requested groups, you can override the default behavior.</span></span>

<span data-ttu-id="59230-122">Toto téma obsahuje následující části:</span><span class="sxs-lookup"><span data-stu-id="59230-122">This topic includes the following sections:</span></span>

- [<span data-ttu-id="59230-123">Přidávání a odebírání uživatelů</span><span class="sxs-lookup"><span data-stu-id="59230-123">Adding and removing users</span></span>](#add)
- [<span data-ttu-id="59230-124">Volání metody členové skupiny</span><span class="sxs-lookup"><span data-stu-id="59230-124">Calling members of a group</span></span>](#call)
- [<span data-ttu-id="59230-125">Ukládání do databáze členství ve skupině</span><span class="sxs-lookup"><span data-stu-id="59230-125">Storing group membership in a database</span></span>](#storedatabase)
- [<span data-ttu-id="59230-126">Ukládání členství ve skupině ve službě Azure table storage</span><span class="sxs-lookup"><span data-stu-id="59230-126">Storing group membership in Azure table storage</span></span>](#storeazuretable)
- [<span data-ttu-id="59230-127">Ověření členství ve skupině při opětovném připojení</span><span class="sxs-lookup"><span data-stu-id="59230-127">Verifying group membership when reconnecting</span></span>](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a><span data-ttu-id="59230-128">Přidávání a odebírání uživatelů</span><span class="sxs-lookup"><span data-stu-id="59230-128">Adding and removing users</span></span>

<span data-ttu-id="59230-129">Chcete-li přidat nebo odebrat uživatele ze skupiny, zavolejte [přidat](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) nebo [odebrat](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) metody a předejte jí id uživatele připojení a názvu skupiny jako parametry.</span><span class="sxs-lookup"><span data-stu-id="59230-129">To add or remove users from a group, you call the [Add](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) or [Remove](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods, and pass in the user's connection id and group's name as parameters.</span></span> <span data-ttu-id="59230-130">Nemusíte ručně odeberte uživatele ze skupiny po ukončení připojení.</span><span class="sxs-lookup"><span data-stu-id="59230-130">You do not need to manually remove a user from a group when the connection ends.</span></span>

<span data-ttu-id="59230-131">Následující příklad ukazuje `Groups.Add` a `Groups.Remove` metody používané v metodách rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="59230-131">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

<span data-ttu-id="59230-132">`Groups.Add` a `Groups.Remove` metody provedení asynchronně.</span><span class="sxs-lookup"><span data-stu-id="59230-132">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span>

<span data-ttu-id="59230-133">Pokud chcete okamžitě odeslání zprávy do klienta pomocí skupiny a přidání klienta do skupiny, budete muset zajistěte, aby nejprve dokončí metodu Groups.Add.</span><span class="sxs-lookup"><span data-stu-id="59230-133">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the Groups.Add method finishes first.</span></span> <span data-ttu-id="59230-134">Následující příklady kódu ukazují, jak to udělat, jeden pomocí kódu, který funguje v rozhraní .NET 4.5 a jeden pomocí kódu, který funguje v rozhraní .NET 4.</span><span class="sxs-lookup"><span data-stu-id="59230-134">The following code examples show how to do that, one by using code that works in .NET 4.5, and one by using code that works in .NET 4.</span></span>

#### <a name="asynchronous-net-45-example"></a><span data-ttu-id="59230-135">Asynchronní .NET 4.5 Příklad</span><span class="sxs-lookup"><span data-stu-id="59230-135">Asynchronous .NET 4.5 Example</span></span>

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

#### <a name="asynchronous-net-4-example"></a><span data-ttu-id="59230-136">Asynchronní .NET 4 příklad</span><span class="sxs-lookup"><span data-stu-id="59230-136">Asynchronous .NET 4 Example</span></span>

[!code-csharp[Main](working-with-groups/samples/sample3.cs?highlight=3-4)]

<span data-ttu-id="59230-137">Obecně platí, by neměla zahrnovat `await` při volání metody `Groups.Remove` metoda protože id připojení, který se pokoušíte odebrat může nadále již nebudou dostupné.</span><span class="sxs-lookup"><span data-stu-id="59230-137">In general, you should not include `await` when calling the `Groups.Remove` method because the connection id that you are trying to remove might no longer be available.</span></span> <span data-ttu-id="59230-138">V takovém případě `TaskCanceledException` je vyvolána po vypršením časového limitu požadavku. Pokud vaše aplikace musíte zajistit, uživatel byl odebrán ze skupiny před odesláním zprávy do skupiny, můžete přidat `await` před Groups.Remove a pak catch `TaskCanceledException` výjimka, která mohou být vyvolány.</span><span class="sxs-lookup"><span data-stu-id="59230-138">In that case, `TaskCanceledException` is thrown after the request times out. If your application must ensure that the user has been removed from the group before sending a message to the group, you can add `await` before Groups.Remove, and then catch the `TaskCanceledException` exception that might be thrown.</span></span>

<a id="call"></a>

## <a name="calling-members-of-a-group"></a><span data-ttu-id="59230-139">Volání metody členové skupiny</span><span class="sxs-lookup"><span data-stu-id="59230-139">Calling members of a group</span></span>

<span data-ttu-id="59230-140">Mohou zasílat zprávy do všech členů skupiny nebo pouze členové zadané skupiny, jak je znázorněno v následujících příkladech.</span><span class="sxs-lookup"><span data-stu-id="59230-140">You can send messages to all of the members of a group or only specified members of the group, as shown in the following examples.</span></span>

- <span data-ttu-id="59230-141">**Všechny** připojených klientů v zadané skupině.</span><span class="sxs-lookup"><span data-stu-id="59230-141">**All** connected clients in a specified group.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample4.css)]
- <span data-ttu-id="59230-142">Všichni připojení klienti v zadané skupině **s výjimkou zadaných klientů**, podle ID připojení.</span><span class="sxs-lookup"><span data-stu-id="59230-142">All connected clients in a specified group **except the specified clients**, identified by connection ID.</span></span> 

    [!code-csharp[Main](working-with-groups/samples/sample5.cs)]
- <span data-ttu-id="59230-143">Všichni připojení klienti v zadané skupině **s výjimkou volajícího klienta**.</span><span class="sxs-lookup"><span data-stu-id="59230-143">All connected clients in a specified group **except the calling client**.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample6.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a><span data-ttu-id="59230-144">Ukládání do databáze členství ve skupině</span><span class="sxs-lookup"><span data-stu-id="59230-144">Storing group membership in a database</span></span>

<span data-ttu-id="59230-145">Následující příklady ukazují, jak uchovávat informace skupiny a uživatele v databázi.</span><span class="sxs-lookup"><span data-stu-id="59230-145">The following examples show how to retain group and user information in a database.</span></span> <span data-ttu-id="59230-146">Můžete použít technologii přístup všech dat; Následující příklad ukazuje, jak definovat modely používající rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="59230-146">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="59230-147">Tyto modely entity odpovídají databáze tabulky a pole.</span><span class="sxs-lookup"><span data-stu-id="59230-147">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="59230-148">Datová struktura může značně lišit v závislosti na požadavcích vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="59230-148">Your data structure could vary considerably depending on the requirements of your application.</span></span> <span data-ttu-id="59230-149">Tento příklad obsahuje třídu s názvem `ConversationRoom` který bude pro aplikaci, která umožňuje uživatelům připojit konverzace o různých tématům, jako je například sportu nebo zahrady jedinečné.</span><span class="sxs-lookup"><span data-stu-id="59230-149">This example includes a class named `ConversationRoom` which would be unique to an application that enables users to join conversations about different subjects, such as sports or gardening.</span></span> <span data-ttu-id="59230-150">Tento příklad také obsahuje třídu pro připojení.</span><span class="sxs-lookup"><span data-stu-id="59230-150">This example also includes a class for the connections.</span></span> <span data-ttu-id="59230-151">Třída připojení není nezbytně nutné pro sledování členství ve skupině, ale často je součástí robustní řešení ke sledování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="59230-151">The connection class is not absolutely required for tracking group membership but is frequently part of robust solution to tracking users.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<span data-ttu-id="59230-152">Potom v rozbočovači, můžete načíst informace o skupiny a uživatele z databáze a ručně přidejte uživatele do příslušných skupin.</span><span class="sxs-lookup"><span data-stu-id="59230-152">Then, in the hub, you can retrieve the group and user information from the database and manually add the user to the appropriate groups.</span></span> <span data-ttu-id="59230-153">V příkladu nezahrnuje kód pro sledování připojení uživatelů.</span><span class="sxs-lookup"><span data-stu-id="59230-153">The example does not include code for tracking the user connections.</span></span> <span data-ttu-id="59230-154">V tomto příkladu `await` – klíčové slovo není použita před `Groups.Add` vzhledem k tomu, že členové skupiny není okamžitě odeslána zpráva.</span><span class="sxs-lookup"><span data-stu-id="59230-154">In this example, the `await` keyword is not applied before `Groups.Add` because a message is not immediately sent to members of the group.</span></span> <span data-ttu-id="59230-155">Pokud chcete odeslat zprávu na všechny členy skupiny okamžitě po přidání nového člena, chcete použít `await` – klíčové slovo a ujistěte se, asynchronní operace byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="59230-155">If you want to send a message to all members of the group immediately after adding the new member, you would want to apply the `await` keyword to make sure the asynchronous operation has completed.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a><span data-ttu-id="59230-156">Ukládání členství ve skupině ve službě Azure table storage</span><span class="sxs-lookup"><span data-stu-id="59230-156">Storing group membership in Azure table storage</span></span>

<span data-ttu-id="59230-157">Použití úložiště tabulek Azure k ukládání informací skupiny a uživatele je podobný používání databáze.</span><span class="sxs-lookup"><span data-stu-id="59230-157">Using Azure table storage to store group and user information is similar to using a database.</span></span> <span data-ttu-id="59230-158">Následující příklad ukazuje entitu tabulky, která ukládá uživatelské jméno a název skupiny.</span><span class="sxs-lookup"><span data-stu-id="59230-158">The following example shows a table entity that stores the user name and group name.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<span data-ttu-id="59230-159">V centru je načíst přiřazené skupiny, když se uživatel připojí.</span><span class="sxs-lookup"><span data-stu-id="59230-159">In the hub, you retrieve the assigned groups when the user connects.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a><span data-ttu-id="59230-160">Ověření členství ve skupině při opětovném připojení</span><span class="sxs-lookup"><span data-stu-id="59230-160">Verifying group membership when reconnecting</span></span>

<span data-ttu-id="59230-161">Ve výchozím nastavení SignalR automaticky znovu přiřadí uživatele do příslušných skupin po opětovném připojení z dočasné přerušení, jako je například když je připojení vyřazen a znovu vytvořit předtím, než vyprší časový limit připojení. Informace o skupině uživatele je předán do tokenu při opětovné připojení, a tento token se ověřuje na serveru.</span><span class="sxs-lookup"><span data-stu-id="59230-161">By default, SignalR automatically re-assigns a user to the appropriate groups when reconnecting from a temporary disruption, such as when a connection is dropped and re-established before the connection times out. The user's group information is passed in a token when reconnecting, and that token is verified on the server.</span></span> <span data-ttu-id="59230-162">Informace o procesu ověření pro opětovné připojování ke skupinám uživatelů najdete v tématu [při opětovné připojení, opětovné připojování ke skupinám](index.md).</span><span class="sxs-lookup"><span data-stu-id="59230-162">For information about the verification process for rejoining users to groups, see [Rejoining groups when reconnecting](index.md).</span></span>

<span data-ttu-id="59230-163">Obecně byste měli používat výchozí chování automaticky opětovné připojování ke, že skupin v připojení.</span><span class="sxs-lookup"><span data-stu-id="59230-163">In general, you should use the default behavior of automatically rejoining groups on reconnect.</span></span> <span data-ttu-id="59230-164">SignalR skupiny nejsou určeny jako bezpečnostní mechanismus pro omezení přístupu k citlivým datům.</span><span class="sxs-lookup"><span data-stu-id="59230-164">SignalR groups are not intended as a security mechanism for restricting access to sensitive data.</span></span> <span data-ttu-id="59230-165">Pokud vaše aplikace musí Zkontrolujte členství ve skupině uživatele při opětovné připojení, ale můžete přepsat výchozí chování.</span><span class="sxs-lookup"><span data-stu-id="59230-165">However, if your application must double-check a user's group membership when reconnecting, you can override the default behavior.</span></span> <span data-ttu-id="59230-166">Změna výchozí chování můžete přidat zatížení k vaší databázi protože členství ve skupině uživatele musí být načtena pro každý opětovné připojení, ne jenom když se uživatel připojí.</span><span class="sxs-lookup"><span data-stu-id="59230-166">Changing the default behavior can add a burden to your database because a user's group membership must be retrieved for each reconnection rather than just when the user connects.</span></span>

<span data-ttu-id="59230-167">Pokud členství ve skupině musí ověřit na znovu připojit, vytvořte nový modul kanálu rozbočovače, který vrátí seznam hodnot přiřazených skupin, jak vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="59230-167">If you must verify group membership on reconnect, create a new hub pipeline module that returns a list of assigned groups, as shown below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample11.cs)]

<span data-ttu-id="59230-168">Tento modul potom přidáte do kanálu rozbočovače, jak je znázorněno dole.</span><span class="sxs-lookup"><span data-stu-id="59230-168">Then, add that module to the hub pipeline, as highlighted below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample12.cs?highlight=10)]
