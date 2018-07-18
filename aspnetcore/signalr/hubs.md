---
title: Použití rozbočovače signalr technologie ASP.NET Core
author: tdykstra
description: Další informace o použití rozbočovače signalr technologie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/01/2018
uid: signalr/hubs
ms.openlocfilehash: be39666373e2b099054bb71f4a7fcf17aeb9a01c
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095278"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="251bf-103">Použití rozbočovače signalr pro ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="251bf-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="251bf-104">Podle [Rachel Appel](https://twitter.com/rachelappel) a [Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="251bf-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="251bf-105">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(jak stáhnout)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="251bf-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="251bf-106">Co je rozbočovače SignalR</span><span class="sxs-lookup"><span data-stu-id="251bf-106">What is a SignalR hub</span></span>

<span data-ttu-id="251bf-107">Rozhraní API pro rozbočovače SignalR můžete volat metody pro připojené klienty ze serveru.</span><span class="sxs-lookup"><span data-stu-id="251bf-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="251bf-108">V serverovém kódu definovat metody, které jsou volány klientem.</span><span class="sxs-lookup"><span data-stu-id="251bf-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="251bf-109">V kódu klienta můžete definovat metody, které jsou volány ze serveru.</span><span class="sxs-lookup"><span data-stu-id="251bf-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="251bf-110">Funkce SignalR se postará o všechno, co na pozadí, které umožňuje komunikaci klienta se serverem a server klient v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="251bf-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="251bf-111">Konfigurace rozbočovače SignalR</span><span class="sxs-lookup"><span data-stu-id="251bf-111">Configure SignalR hubs</span></span>

<span data-ttu-id="251bf-112">SignalR middleware vyžaduje některé služby, které jsou nakonfigurované pomocí volání `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="251bf-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="251bf-113">Při přidávání funkce SignalR pro aplikace ASP.NET Core, nastavit trasy SignalR voláním `app.UseSignalR` v `Startup.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="251bf-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="251bf-114">Vytvoření a použití rozbočovače</span><span class="sxs-lookup"><span data-stu-id="251bf-114">Create and use hubs</span></span>

<span data-ttu-id="251bf-115">Vytvoření centra deklarováním třídy, která dědí z `Hub`a přidejte do ní veřejné metody.</span><span class="sxs-lookup"><span data-stu-id="251bf-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="251bf-116">Klienti mohou volat metody, které jsou definovány jako `public`.</span><span class="sxs-lookup"><span data-stu-id="251bf-116">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

<span data-ttu-id="251bf-117">Můžete určit návratový typ a parametry, včetně komplexní typy a pole, stejně jako v jakékoli metodě jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="251bf-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="251bf-118">Funkce SignalR zpracovává serializace a deserializace komplexních objektů a polí v parametry a návratové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="251bf-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-clients-object"></a><span data-ttu-id="251bf-119">Objekt klientů</span><span class="sxs-lookup"><span data-stu-id="251bf-119">The Clients object</span></span>

<span data-ttu-id="251bf-120">Každá instance `Hub` třída nemá vlastnost s názvem `Clients` , který obsahuje následující členy pro komunikaci mezi serverem a klientem:</span><span class="sxs-lookup"><span data-stu-id="251bf-120">Each instance of the `Hub` class has a property named `Clients` that contains the following members for communication between server and client:</span></span>

| <span data-ttu-id="251bf-121">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="251bf-121">Property</span></span> | <span data-ttu-id="251bf-122">Popis</span><span class="sxs-lookup"><span data-stu-id="251bf-122">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="251bf-123">Volá metodu na všechny připojené klienty</span><span class="sxs-lookup"><span data-stu-id="251bf-123">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="251bf-124">Volá metodu na straně klienta, který volal metodu rozbočovače na</span><span class="sxs-lookup"><span data-stu-id="251bf-124">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="251bf-125">Volá metodu na všechny připojené klienty kromě klienta, který volal metodu</span><span class="sxs-lookup"><span data-stu-id="251bf-125">Calls a method on all connected clients except the client that invoked the method</span></span> |


<span data-ttu-id="251bf-126">Kromě toho `Hub.Clients` obsahuje následující dvě metody:</span><span class="sxs-lookup"><span data-stu-id="251bf-126">Additionally, `Hub.Clients` contains the following methods:</span></span>

| <span data-ttu-id="251bf-127">Metoda</span><span class="sxs-lookup"><span data-stu-id="251bf-127">Method</span></span> | <span data-ttu-id="251bf-128">Popis</span><span class="sxs-lookup"><span data-stu-id="251bf-128">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="251bf-129">Volá metodu na všechny připojené klienty s výjimkou zadaných připojení</span><span class="sxs-lookup"><span data-stu-id="251bf-129">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="251bf-130">Volá metodu pro konkrétní připojení klienta</span><span class="sxs-lookup"><span data-stu-id="251bf-130">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="251bf-131">Volá metodu pro konkrétní připojených klientů</span><span class="sxs-lookup"><span data-stu-id="251bf-131">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="251bf-132">Volá metodu pro všechna připojení v určené skupině</span><span class="sxs-lookup"><span data-stu-id="251bf-132">Calls a method to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="251bf-133">Volá metodu pro všechna připojení v určené skupině s výjimkou zadaných připojení</span><span class="sxs-lookup"><span data-stu-id="251bf-133">Calls a method to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="251bf-134">Volá metodu k několika skupinám připojení</span><span class="sxs-lookup"><span data-stu-id="251bf-134">Calls a method to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="251bf-135">Volá metodu pro připojení s výjimkou klienta, který volal metodu rozbočovače na skupinu</span><span class="sxs-lookup"><span data-stu-id="251bf-135">Calls a method to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="251bf-136">Volá metodu pro všechna připojení, které jsou spojené s konkrétním uživatelem</span><span class="sxs-lookup"><span data-stu-id="251bf-136">Calls a method to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="251bf-137">Volá metodu pro všechna připojení přidružená k určení uživatelé</span><span class="sxs-lookup"><span data-stu-id="251bf-137">Calls a method to all connections associated with the specified users</span></span> |

<span data-ttu-id="251bf-138">Každé vlastnosti nebo metody v předchozích tabulkách vrátí objekt s `SendAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="251bf-138">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="251bf-139">`SendAsync` Metoda vám umožňuje zadat název a parametry metody volání.</span><span class="sxs-lookup"><span data-stu-id="251bf-139">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="251bf-140">Odesílání zpráv do klientů</span><span class="sxs-lookup"><span data-stu-id="251bf-140">Send messages to clients</span></span>

<span data-ttu-id="251bf-141">Chcete-li provést volání do konkrétních klientů, použijte vlastnosti `Clients` objektu.</span><span class="sxs-lookup"><span data-stu-id="251bf-141">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="251bf-142">V následujícím příkladu `SendMessageToCaller` metoda ukazuje odesílání zprávy připojení, které volal metodu rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="251bf-142">In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="251bf-143">`SendMessageToGroups` Metoda odešle zprávu do skupin uložených v `List` s názvem `groups`.</span><span class="sxs-lookup"><span data-stu-id="251bf-143">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="251bf-144">Zpracování událostí pro připojení</span><span class="sxs-lookup"><span data-stu-id="251bf-144">Handle events for a connection</span></span>

<span data-ttu-id="251bf-145">Poskytuje rozhraní API pro rozbočovače SignalR `OnConnectedAsync` a `OnDisconnectedAsync` virtuální metody pro správu a sledování připojení.</span><span class="sxs-lookup"><span data-stu-id="251bf-145">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="251bf-146">Přepsat `OnConnectedAsync` virtuální metody pro provádění akcí po připojení klienta k rozbočovači, jako je například přidávání do skupiny.</span><span class="sxs-lookup"><span data-stu-id="251bf-146">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a><span data-ttu-id="251bf-147">Zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="251bf-147">Handle errors</span></span>

<span data-ttu-id="251bf-148">Výjimky vzniklé v metodách vašeho centra se posílají klientovi, který volal metodu.</span><span class="sxs-lookup"><span data-stu-id="251bf-148">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="251bf-149">Na klientovi JavaScript `invoke` metoda vrátí hodnotu [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="251bf-149">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="251bf-150">Když klient obdrží chybu s obslužnou rutinou k němu připojit pomocí promise `catch`, ji má a předají jako JavaScript `Error` objektu.</span><span class="sxs-lookup"><span data-stu-id="251bf-150">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a><span data-ttu-id="251bf-151">Související prostředky</span><span class="sxs-lookup"><span data-stu-id="251bf-151">Related resources</span></span>

* [<span data-ttu-id="251bf-152">Úvod do ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="251bf-152">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="251bf-153">Klient JavaScriptu</span><span class="sxs-lookup"><span data-stu-id="251bf-153">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="251bf-154">Publikování do Azure</span><span class="sxs-lookup"><span data-stu-id="251bf-154">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
