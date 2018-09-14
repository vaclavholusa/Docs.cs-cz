---
title: Použití rozbočovače signalr technologie ASP.NET Core
author: tdykstra
description: Další informace o použití rozbočovače signalr technologie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/12/2018
uid: signalr/hubs
ms.openlocfilehash: 17e3ee23967bc1097a3121b3e3e5b58cebe3887d
ms.sourcegitcommit: a742b55e4b8276a48b8b4394784554fecd883c84
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45538359"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="0c474-103">Použití rozbočovače signalr pro ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c474-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="0c474-104">Podle [Rachel Appel](https://twitter.com/rachelappel) a [Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="0c474-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="0c474-105">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(jak stáhnout)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="0c474-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="0c474-106">Co je rozbočovače SignalR</span><span class="sxs-lookup"><span data-stu-id="0c474-106">What is a SignalR hub</span></span>

<span data-ttu-id="0c474-107">Rozhraní API pro rozbočovače SignalR můžete volat metody pro připojené klienty ze serveru.</span><span class="sxs-lookup"><span data-stu-id="0c474-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="0c474-108">V serverovém kódu definovat metody, které jsou volány klientem.</span><span class="sxs-lookup"><span data-stu-id="0c474-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="0c474-109">V kódu klienta můžete definovat metody, které jsou volány ze serveru.</span><span class="sxs-lookup"><span data-stu-id="0c474-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="0c474-110">Funkce SignalR se postará o všechno, co na pozadí, které umožňuje komunikaci klienta se serverem a server klient v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="0c474-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="0c474-111">Konfigurace rozbočovače SignalR</span><span class="sxs-lookup"><span data-stu-id="0c474-111">Configure SignalR hubs</span></span>

<span data-ttu-id="0c474-112">SignalR middleware vyžaduje některé služby, které jsou nakonfigurované pomocí volání `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="0c474-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="0c474-113">Při přidávání funkce SignalR pro aplikace ASP.NET Core, nastavit trasy SignalR voláním `app.UseSignalR` v `Startup.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="0c474-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="0c474-114">Vytvoření a použití rozbočovače</span><span class="sxs-lookup"><span data-stu-id="0c474-114">Create and use hubs</span></span>

<span data-ttu-id="0c474-115">Vytvoření centra deklarováním třídy, která dědí z `Hub`a přidejte do ní veřejné metody.</span><span class="sxs-lookup"><span data-stu-id="0c474-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="0c474-116">Klienti mohou volat metody, které jsou definovány jako `public`.</span><span class="sxs-lookup"><span data-stu-id="0c474-116">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

<span data-ttu-id="0c474-117">Můžete určit návratový typ a parametry, včetně komplexní typy a pole, stejně jako v jakékoli metodě jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="0c474-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="0c474-118">Funkce SignalR zpracovává serializace a deserializace komplexních objektů a polí v parametry a návratové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="0c474-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-context-object"></a><span data-ttu-id="0c474-119">Objekt kontextu</span><span class="sxs-lookup"><span data-stu-id="0c474-119">The Context object</span></span>

<span data-ttu-id="0c474-120">`Hub` Třída nemá `Context` vlastnost, která obsahuje následující vlastnosti s informacemi o připojení:</span><span class="sxs-lookup"><span data-stu-id="0c474-120">The `Hub` class has a `Context` property that contains the following properties with information about the connection:</span></span>

| <span data-ttu-id="0c474-121">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="0c474-121">Property</span></span> | <span data-ttu-id="0c474-122">Popis</span><span class="sxs-lookup"><span data-stu-id="0c474-122">Description</span></span> |
| ------ | ----------- |
| `ConnectionId` | <span data-ttu-id="0c474-123">Získá jedinečný ID pro připojení, přiřazené systémem SignalR.</span><span class="sxs-lookup"><span data-stu-id="0c474-123">Gets the unique ID for the connection, assigned by SignalR.</span></span> <span data-ttu-id="0c474-124">Existuje jedno ID připojení pro každé připojení.</span><span class="sxs-lookup"><span data-stu-id="0c474-124">There is one connection ID for each connection.</span></span>|
| `UserIdentifier` | <span data-ttu-id="0c474-125">Získá [identifikátor uživatele](xref:signalr/groups).</span><span class="sxs-lookup"><span data-stu-id="0c474-125">Gets the [user identifier](xref:signalr/groups).</span></span> <span data-ttu-id="0c474-126">Ve výchozím nastavení, používá SignalR `ClaimTypes.NameIdentifier` z `ClaimsPrincipal` přidružené k připojení jako identifikátor uživatele.</span><span class="sxs-lookup"><span data-stu-id="0c474-126">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> |
| `User` | <span data-ttu-id="0c474-127">Získá `ClaimsPrincipal` spojené s aktuálním uživatelem.</span><span class="sxs-lookup"><span data-stu-id="0c474-127">Gets the `ClaimsPrincipal` associated with the current user.</span></span> |
| `Items` | <span data-ttu-id="0c474-128">Získá kolekci klíč/hodnota, která umožňuje sdílet data v rámci oboru pro toto připojení.</span><span class="sxs-lookup"><span data-stu-id="0c474-128">Gets a key/value collection that can be used to share data within the scope of this connection.</span></span> <span data-ttu-id="0c474-129">Data mohou být uložena v této kolekci a se zachová připojení napříč volání metod rozbočovače na jiný.</span><span class="sxs-lookup"><span data-stu-id="0c474-129">Data can be stored in this collection and it will persist for the connection across different hub method invocations.</span></span> |
| `Features` | <span data-ttu-id="0c474-130">Získá kolekci funkcí, které jsou k dispozici na připojení.</span><span class="sxs-lookup"><span data-stu-id="0c474-130">Gets the collection of features available on the connection.</span></span> <span data-ttu-id="0c474-131">Teď není potřeba tuto kolekci ve většině scénářů, takže ho není podrobně popsány v ještě.</span><span class="sxs-lookup"><span data-stu-id="0c474-131">For now, this collection isn't needed in most scenarios, so it isn't documented in detail yet.</span></span> |
| `ConnectionAborted` | <span data-ttu-id="0c474-132">Získá `CancellationToken` , která upozorní, když je připojení přerušeno.</span><span class="sxs-lookup"><span data-stu-id="0c474-132">Gets a `CancellationToken` that notifies when the connection is aborted.</span></span> |

<span data-ttu-id="0c474-133">`Hub.Context` obsahuje také následující metody:</span><span class="sxs-lookup"><span data-stu-id="0c474-133">`Hub.Context` also contains the following methods:</span></span>

| <span data-ttu-id="0c474-134">Metoda</span><span class="sxs-lookup"><span data-stu-id="0c474-134">Method</span></span> | <span data-ttu-id="0c474-135">Popis</span><span class="sxs-lookup"><span data-stu-id="0c474-135">Description</span></span> |
| ------ | ----------- |
| `GetHttpContext` | <span data-ttu-id="0c474-136">Vrátí `HttpContext` pro připojení, nebo `null` Pokud připojení není přidružený požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="0c474-136">Returns the `HttpContext` for the connection, or `null` if the connection is not associated with an HTTP request.</span></span> <span data-ttu-id="0c474-137">Pro připojení prostřednictvím protokolu HTTP můžete použít tuto metodu se získat informace, jako jsou hlavičky protokolu HTTP a řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="0c474-137">For HTTP connections, you can use this method to get information such as HTTP headers and query strings.</span></span> |
| `Abort` | <span data-ttu-id="0c474-138">Zruší připojení.</span><span class="sxs-lookup"><span data-stu-id="0c474-138">Aborts the connection.</span></span> |

## <a name="the-clients-object"></a><span data-ttu-id="0c474-139">Objekt klientů</span><span class="sxs-lookup"><span data-stu-id="0c474-139">The Clients object</span></span>

<span data-ttu-id="0c474-140">`Hub` Třída nemá `Clients` vlastnost, která obsahuje následující vlastnosti pro komunikaci mezi serverem a klientem:</span><span class="sxs-lookup"><span data-stu-id="0c474-140">The `Hub` class has a `Clients` property that contains the following properties for communication between server and client:</span></span>

| <span data-ttu-id="0c474-141">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="0c474-141">Property</span></span> | <span data-ttu-id="0c474-142">Popis</span><span class="sxs-lookup"><span data-stu-id="0c474-142">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="0c474-143">Volá metodu na všechny připojené klienty</span><span class="sxs-lookup"><span data-stu-id="0c474-143">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="0c474-144">Volá metodu na straně klienta, který volal metodu rozbočovače na</span><span class="sxs-lookup"><span data-stu-id="0c474-144">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="0c474-145">Volá metodu na všechny připojené klienty kromě klienta, který volal metodu</span><span class="sxs-lookup"><span data-stu-id="0c474-145">Calls a method on all connected clients except the client that invoked the method</span></span> |


<span data-ttu-id="0c474-146">`Hub.Clients` obsahuje také následující metody:</span><span class="sxs-lookup"><span data-stu-id="0c474-146">`Hub.Clients` also contains the following methods:</span></span>

| <span data-ttu-id="0c474-147">Metoda</span><span class="sxs-lookup"><span data-stu-id="0c474-147">Method</span></span> | <span data-ttu-id="0c474-148">Popis</span><span class="sxs-lookup"><span data-stu-id="0c474-148">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="0c474-149">Volá metodu na všechny připojené klienty s výjimkou zadaných připojení</span><span class="sxs-lookup"><span data-stu-id="0c474-149">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="0c474-150">Volá metodu pro konkrétní připojení klienta</span><span class="sxs-lookup"><span data-stu-id="0c474-150">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="0c474-151">Volá metodu pro konkrétní připojených klientů</span><span class="sxs-lookup"><span data-stu-id="0c474-151">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="0c474-152">Volá metodu pro všechna připojení v určené skupině</span><span class="sxs-lookup"><span data-stu-id="0c474-152">Calls a method to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="0c474-153">Volá metodu pro všechna připojení v určené skupině s výjimkou zadaných připojení</span><span class="sxs-lookup"><span data-stu-id="0c474-153">Calls a method to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="0c474-154">Volá metodu k několika skupinám připojení</span><span class="sxs-lookup"><span data-stu-id="0c474-154">Calls a method to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="0c474-155">Volá metodu pro připojení s výjimkou klienta, který volal metodu rozbočovače na skupinu</span><span class="sxs-lookup"><span data-stu-id="0c474-155">Calls a method to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="0c474-156">Volá metodu pro všechna připojení, které jsou spojené s konkrétním uživatelem</span><span class="sxs-lookup"><span data-stu-id="0c474-156">Calls a method to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="0c474-157">Volá metodu pro všechna připojení přidružená k určení uživatelé</span><span class="sxs-lookup"><span data-stu-id="0c474-157">Calls a method to all connections associated with the specified users</span></span> |

<span data-ttu-id="0c474-158">Každé vlastnosti nebo metody v předchozích tabulkách vrátí objekt s `SendAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="0c474-158">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="0c474-159">`SendAsync` Metoda vám umožňuje zadat název a parametry metody volání.</span><span class="sxs-lookup"><span data-stu-id="0c474-159">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="0c474-160">Odesílání zpráv do klientů</span><span class="sxs-lookup"><span data-stu-id="0c474-160">Send messages to clients</span></span>

<span data-ttu-id="0c474-161">Chcete-li provést volání do konkrétních klientů, použijte vlastnosti `Clients` objektu.</span><span class="sxs-lookup"><span data-stu-id="0c474-161">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="0c474-162">V následujícím příkladu `SendMessageToCaller` metoda ukazuje odesílání zprávy připojení, které volal metodu rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="0c474-162">In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="0c474-163">`SendMessageToGroups` Metoda odešle zprávu do skupin uložených v `List` s názvem `groups`.</span><span class="sxs-lookup"><span data-stu-id="0c474-163">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="strongly-typed-hubs"></a><span data-ttu-id="0c474-164">Silného typu rozbočovače</span><span class="sxs-lookup"><span data-stu-id="0c474-164">Strongly typed hubs</span></span>

<span data-ttu-id="0c474-165">Nevýhodou použití `SendAsync` je, že spoléhá na magický řetězec a určit metodu klienta k volání.</span><span class="sxs-lookup"><span data-stu-id="0c474-165">A drawback of using `SendAsync` is that it relies on a magic string to specify the client method to be called.</span></span> <span data-ttu-id="0c474-166">Kvůli tomu open kód chyby za běhu, pokud název metody je zadáno chybně nebo chybějící z klienta.</span><span class="sxs-lookup"><span data-stu-id="0c474-166">This leaves code open to runtime errors if the method name is misspelled or missing from the client.</span></span>

<span data-ttu-id="0c474-167">O alternativu k použití `SendAsync` silného typu, je `Hub` s <xref:Microsoft.AspNetCore.SignalR.Hub`1>.</span><span class="sxs-lookup"><span data-stu-id="0c474-167">An alternative to using `SendAsync` is to strongly type the `Hub` with <xref:Microsoft.AspNetCore.SignalR.Hub`1>.</span></span> <span data-ttu-id="0c474-168">V následujícím příkladu `ChatHub` metody klienta bylo extrahováno ven do rozhraní volá `IChatClient`.</span><span class="sxs-lookup"><span data-stu-id="0c474-168">In the following example, the `ChatHub` client methods have been extracted out into an interface called `IChatClient`.</span></span>  

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

<span data-ttu-id="0c474-169">Toto rozhraní je možné Refaktorovat předchozí `ChatHub` příklad.</span><span class="sxs-lookup"><span data-stu-id="0c474-169">This interface can be used to refactor the preceding `ChatHub` example.</span></span>

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

<span data-ttu-id="0c474-170">Pomocí `Hub<IChatClient>` povolí kompilaci kontrolu metodu klienta.</span><span class="sxs-lookup"><span data-stu-id="0c474-170">Using `Hub<IChatClient>` enables compile-time checking of the client methods.</span></span> <span data-ttu-id="0c474-171">To brání problémy způsobené pomocí magic řetězců, protože `Hub<T>` můžete poskytnout přístup k metodám definované v rozhraní.</span><span class="sxs-lookup"><span data-stu-id="0c474-171">This prevents issues caused by using magic strings, since `Hub<T>` can only provide access to the methods defined in the interface.</span></span>

<span data-ttu-id="0c474-172">Pomocí silného typu `Hub<T>` zakáže možnost používat `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="0c474-172">Using a strongly typed `Hub<T>` disables the ability to use `SendAsync`.</span></span>

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="0c474-173">Zpracování událostí pro připojení</span><span class="sxs-lookup"><span data-stu-id="0c474-173">Handle events for a connection</span></span>

<span data-ttu-id="0c474-174">Poskytuje rozhraní API pro rozbočovače SignalR `OnConnectedAsync` a `OnDisconnectedAsync` virtuální metody pro správu a sledování připojení.</span><span class="sxs-lookup"><span data-stu-id="0c474-174">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="0c474-175">Přepsat `OnConnectedAsync` virtuální metody pro provádění akcí po připojení klienta k rozbočovači, jako je například přidávání do skupiny.</span><span class="sxs-lookup"><span data-stu-id="0c474-175">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a><span data-ttu-id="0c474-176">Zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="0c474-176">Handle errors</span></span>

<span data-ttu-id="0c474-177">Výjimky vzniklé v metodách vašeho centra se posílají klientovi, který volal metodu.</span><span class="sxs-lookup"><span data-stu-id="0c474-177">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="0c474-178">Na klientovi JavaScript `invoke` metoda vrátí hodnotu [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="0c474-178">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="0c474-179">Když klient obdrží chybu s obslužnou rutinou k němu připojit pomocí promise `catch`, ji má a předají jako JavaScript `Error` objektu.</span><span class="sxs-lookup"><span data-stu-id="0c474-179">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a><span data-ttu-id="0c474-180">Související prostředky</span><span class="sxs-lookup"><span data-stu-id="0c474-180">Related resources</span></span>

* [<span data-ttu-id="0c474-181">Úvod do ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="0c474-181">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="0c474-182">Klient JavaScriptu</span><span class="sxs-lookup"><span data-stu-id="0c474-182">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="0c474-183">Publikování do Azure</span><span class="sxs-lookup"><span data-stu-id="0c474-183">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
