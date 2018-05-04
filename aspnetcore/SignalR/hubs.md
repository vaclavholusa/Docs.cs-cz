---
title: Použití centra v ASP.NET Core SignalR
author: rachelappel
description: Další informace o použití centra v ASP.NET Core SignalR.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/01/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubs
ms.openlocfilehash: e23d7ef6d5e5e93d5fc69ad4c845a6a896836170
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/03/2018
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="8f890-103">Použití centra v systému SignalR pro ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8f890-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="8f890-104">Podle [Rachel Appel](https://twitter.com/rachelappel) a [kevina Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="8f890-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

<span data-ttu-id="8f890-105">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(jak ke stažení)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="8f890-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="8f890-106">Co je rozbočovače SignalR</span><span class="sxs-lookup"><span data-stu-id="8f890-106">What is a SignalR hub</span></span>

<span data-ttu-id="8f890-107">Rozhraní API rozbočovače SignalR umožňuje volat metody pro připojené klienty ze serveru.</span><span class="sxs-lookup"><span data-stu-id="8f890-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="8f890-108">V serverovém kódu definujete metody, které se nazývají klientem.</span><span class="sxs-lookup"><span data-stu-id="8f890-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="8f890-109">V klientském kódu definujete metody, které se nazývají ze serveru.</span><span class="sxs-lookup"><span data-stu-id="8f890-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="8f890-110">SignalR postará vše na pozadí, které umožňuje v reálném čase komunikaci klienta se serverem a server klient.</span><span class="sxs-lookup"><span data-stu-id="8f890-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="8f890-111">Konfigurace rozbočovače SignalR</span><span class="sxs-lookup"><span data-stu-id="8f890-111">Configure SignalR hubs</span></span>

<span data-ttu-id="8f890-112">SignalR middleware vyžaduje některé služby, které jsou nakonfigurované voláním `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="8f890-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=37)]

<span data-ttu-id="8f890-113">Při přidávání funkce SignalR do aplikace ASP.NET Core, instalační program SignalR trasy voláním `app.UseSignalR` v `Startup.Configure` metoda.</span><span class="sxs-lookup"><span data-stu-id="8f890-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=56-59)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="8f890-114">Vytváření a používání centra</span><span class="sxs-lookup"><span data-stu-id="8f890-114">Create and use hubs</span></span>

<span data-ttu-id="8f890-115">Vytvoření rozbočovač deklarováním třídy, která dědí z `Hub`a přidejte do ní veřejné metody.</span><span class="sxs-lookup"><span data-stu-id="8f890-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="8f890-116">Klienti mohou volat metody, které jsou definovány jako `public`.</span><span class="sxs-lookup"><span data-stu-id="8f890-116">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

<span data-ttu-id="8f890-117">Můžete zadat návratový typ a parametry, včetně komplexní typy a pole, jako byste v libovolné metody C#.</span><span class="sxs-lookup"><span data-stu-id="8f890-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="8f890-118">SignalR zpracovává serializace a deserializace komplexní objekty a pole v parametry a návratové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8f890-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-clients-object"></a><span data-ttu-id="8f890-119">Objekt klientů</span><span class="sxs-lookup"><span data-stu-id="8f890-119">The Clients object</span></span>

<span data-ttu-id="8f890-120">Každá instance `Hub` třída má vlastnost s názvem `Clients` obsahující následující členy pro komunikaci mezi serverem a klientem:</span><span class="sxs-lookup"><span data-stu-id="8f890-120">Each instance of the `Hub` class has a property named `Clients` that contains the following members for communication between server and client:</span></span>

| <span data-ttu-id="8f890-121">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="8f890-121">Property</span></span> | <span data-ttu-id="8f890-122">Popis</span><span class="sxs-lookup"><span data-stu-id="8f890-122">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="8f890-123">Volání metody na všech připojených klientů</span><span class="sxs-lookup"><span data-stu-id="8f890-123">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="8f890-124">Volání metody na straně klienta, který volá metodu rozbočovače</span><span class="sxs-lookup"><span data-stu-id="8f890-124">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="8f890-125">Volání metody na všechny připojené klienty kromě klienta, která volá metodu</span><span class="sxs-lookup"><span data-stu-id="8f890-125">Calls a method on all connected clients except the client that invoked the method</span></span> |

<span data-ttu-id="8f890-126">Kromě toho `Hub` třída obsahuje následující metody:</span><span class="sxs-lookup"><span data-stu-id="8f890-126">Additionally, the `Hub` class contains the following methods:</span></span>

| <span data-ttu-id="8f890-127">Metoda</span><span class="sxs-lookup"><span data-stu-id="8f890-127">Method</span></span> | <span data-ttu-id="8f890-128">Popis</span><span class="sxs-lookup"><span data-stu-id="8f890-128">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="8f890-129">Volání metody na všechny připojené klienty s výjimkou zadaných připojení</span><span class="sxs-lookup"><span data-stu-id="8f890-129">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="8f890-130">Volání metody na konkrétní připojený klient</span><span class="sxs-lookup"><span data-stu-id="8f890-130">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="8f890-131">Volání metody na konkrétní připojené klienty</span><span class="sxs-lookup"><span data-stu-id="8f890-131">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="8f890-132">Odešle zprávu do všech připojení v určené skupině</span><span class="sxs-lookup"><span data-stu-id="8f890-132">Sends a message to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="8f890-133">Odešle zprávu do všech připojení v určené skupině, s výjimkou zadaných připojení</span><span class="sxs-lookup"><span data-stu-id="8f890-133">Sends a message to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="8f890-134">Odešle zprávu do více skupin pro připojení</span><span class="sxs-lookup"><span data-stu-id="8f890-134">Sends a message to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="8f890-135">Odešle zprávu do připojení s výjimkou klienta, která volá metodu rozbočovače na skupinu</span><span class="sxs-lookup"><span data-stu-id="8f890-135">Sends a message to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="8f890-136">Odešle zprávu do všech připojení spojené s konkrétním uživatelem</span><span class="sxs-lookup"><span data-stu-id="8f890-136">Sends a message to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="8f890-137">Odešle zprávu do všech připojení přidružené k určení uživatelé</span><span class="sxs-lookup"><span data-stu-id="8f890-137">Sends a message to all connections associated with the specified users</span></span> |

<span data-ttu-id="8f890-138">Každé vlastnosti nebo metody v předchozích tabulkách vrátí objekt s `SendAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="8f890-138">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="8f890-139">`SendAsync` Metoda umožňuje zadat název a parametry metody klienta k volání.</span><span class="sxs-lookup"><span data-stu-id="8f890-139">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="8f890-140">Odesílat zprávy pro klienty</span><span class="sxs-lookup"><span data-stu-id="8f890-140">Send messages to clients</span></span>

<span data-ttu-id="8f890-141">Volání na konkrétní klienty, použijte vlastnosti `Clients` objektu.</span><span class="sxs-lookup"><span data-stu-id="8f890-141">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="8f890-142">V následujícím v následujícím příkladu `SendMessageToCaller` metoda ukazuje odesílání zprávy připojení, která volá metodu rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="8f890-142">In the following In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="8f890-143">`SendMessageToGroups` Metoda odešle zprávu do skupin, které jsou uložené v `List` s názvem `groups`.</span><span class="sxs-lookup"><span data-stu-id="8f890-143">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="8f890-144">Zpracování událostí pro připojení</span><span class="sxs-lookup"><span data-stu-id="8f890-144">Handle events for a connection</span></span>

<span data-ttu-id="8f890-145">Poskytuje rozhraní API rozbočovače SignalR `OnConnectedAsync` a `OnDisconnectedAsync` virtuální metody ke správě a sledování připojení.</span><span class="sxs-lookup"><span data-stu-id="8f890-145">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="8f890-146">Přepsání `OnConnectedAsync` virtuální metoda k provádění akcí, když se klient připojí k rozbočovači, jako je například přidávání do skupiny.</span><span class="sxs-lookup"><span data-stu-id="8f890-146">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a><span data-ttu-id="8f890-147">Zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="8f890-147">Handle errors</span></span>

<span data-ttu-id="8f890-148">Výjimky vzniklé v metodách vašeho centra se posílají klientovi, který volá metodu.</span><span class="sxs-lookup"><span data-stu-id="8f890-148">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="8f890-149">Na straně klienta JavaScript `invoke` metoda vrátí [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="8f890-149">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="8f890-150">Když klient obdrží chybu s obslužnou rutinou připojené pomocí promise `catch`, má vyvolat a předá jako JavaScriptu `Error` objektu.</span><span class="sxs-lookup"><span data-stu-id="8f890-150">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=22)]

## <a name="related-resources"></a><span data-ttu-id="8f890-151">Související informační zdroje</span><span class="sxs-lookup"><span data-stu-id="8f890-151">Related resources</span></span>

[<span data-ttu-id="8f890-152">Úvod do základní ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="8f890-152">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)