---
title: Správa uživatelů a skupin v knihovně SignalR
author: rachelappel
description: Přehled ASP.NET Core SignalR uživatelů a skupin správy.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: 3e5e310c84bc3ed5790d5b67a917bd54162ea163
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402522"
---
# <a name="manage-users-and-groups-in-signalr"></a><span data-ttu-id="8037f-103">Správa uživatelů a skupin v knihovně SignalR</span><span class="sxs-lookup"><span data-stu-id="8037f-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="8037f-104">Podle [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="8037f-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="8037f-105">Funkce SignalR umožňuje zprávy k odeslání do všech připojení, které jsou spojené s konkrétním uživatelem, jakož i pojmenovaným skupinám připojení.</span><span class="sxs-lookup"><span data-stu-id="8037f-105">SignalR allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="8037f-106">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(jak stáhnout)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="8037f-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="users-in-signalr"></a><span data-ttu-id="8037f-107">Uživatelé v knihovně SignalR</span><span class="sxs-lookup"><span data-stu-id="8037f-107">Users in SignalR</span></span>

<span data-ttu-id="8037f-108">Funkce SignalR umožňuje odesílání zpráv do všech připojení, které jsou spojené s konkrétním uživatelem.</span><span class="sxs-lookup"><span data-stu-id="8037f-108">SignalR allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="8037f-109">Ve výchozím nastavení, používá SignalR `ClaimTypes.NameIdentifier` z `ClaimsPrincipal` přidružené k připojení jako identifikátor uživatele.</span><span class="sxs-lookup"><span data-stu-id="8037f-109">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="8037f-110">Jeden uživatel může mít více připojení k aplikaci s knihovnou SignalR.</span><span class="sxs-lookup"><span data-stu-id="8037f-110">A single user can have multiple connections to a SignalR app.</span></span> <span data-ttu-id="8037f-111">Například může být připojen uživatel na svém počítači, stejně jako svůj telefon.</span><span class="sxs-lookup"><span data-stu-id="8037f-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="8037f-112">Každé zařízení má samostatného připojení SignalR, ale jsou všechny související se stejným uživatelem.</span><span class="sxs-lookup"><span data-stu-id="8037f-112">Each device has a separate SignalR connection, but they're all associated with the same user.</span></span> <span data-ttu-id="8037f-113">Pokud je uživateli odeslána zpráva, všechna připojení přidružená k tomuto uživateli zobrazí zpráva.</span><span class="sxs-lookup"><span data-stu-id="8037f-113">If a message is sent to the user, all of the connections associated with that user receive the message.</span></span> <span data-ttu-id="8037f-114">Identifikátor uživatele pro připojení je přístupný `Context.UserIdentifier` vlastnosti v centru.</span><span class="sxs-lookup"><span data-stu-id="8037f-114">The user identifier for a connection can be accessed by the `Context.UserIdentifier` property in your hub.</span></span>

<span data-ttu-id="8037f-115">Odeslání zprávy do konkrétního uživatele tím, že předáte identifikátor uživatele, který `User` fungovat ve své metodě rozbočovače, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="8037f-115">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="8037f-116">Identifikátor uživatele je velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="8037f-116">The user identifier is case-sensitive.</span></span>

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

<span data-ttu-id="8037f-117">Identifikátor uživatele je možné přizpůsobit tak, že vytvoříte `IUserIdProvider`a registrace v `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8037f-117">The user identifier can be customized by creating an `IUserIdProvider`, and registering it in `ConfigureServices`.</span></span>

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> <span data-ttu-id="8037f-118">AddSignalR musí být volána před registrací vaše vlastní služby SignalR.</span><span class="sxs-lookup"><span data-stu-id="8037f-118">AddSignalR must be called before registering your custom SignalR services.</span></span>

## <a name="groups-in-signalr"></a><span data-ttu-id="8037f-119">Skupinami v knihovně SignalR</span><span class="sxs-lookup"><span data-stu-id="8037f-119">Groups in SignalR</span></span>

<span data-ttu-id="8037f-120">Skupina je kolekce připojení přidružená k názvu.</span><span class="sxs-lookup"><span data-stu-id="8037f-120">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="8037f-121">Pro všechna připojení ve skupině nelze odesílat zprávy.</span><span class="sxs-lookup"><span data-stu-id="8037f-121">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="8037f-122">Skupiny jsou doporučeným způsobem, jak odesílat připojení nebo více připojení, protože tyto skupiny jsou spravována aplikací.</span><span class="sxs-lookup"><span data-stu-id="8037f-122">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="8037f-123">Připojení může být členem více skupin.</span><span class="sxs-lookup"><span data-stu-id="8037f-123">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="8037f-124">Díky tomu skupiny ideální pro něco jako je chatovací aplikaci, kde každý prostor může být reprezentována jako skupinu.</span><span class="sxs-lookup"><span data-stu-id="8037f-124">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="8037f-125">Připojení může být přidán či odebrán ze skupiny přes `AddToGroupAsync` a `RemoveFromGroupAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="8037f-125">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="8037f-126">Členství ve skupině se nezachová při opětovném navázání připojení.</span><span class="sxs-lookup"><span data-stu-id="8037f-126">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="8037f-127">Připojení je potřeba znovu vstoupit skupině znovu zřízeno.</span><span class="sxs-lookup"><span data-stu-id="8037f-127">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="8037f-128">Není možné počet členů skupiny, protože tyto informace není k dispozici v případě, že aplikace je škálovaný na více serverů.</span><span class="sxs-lookup"><span data-stu-id="8037f-128">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

> [!NOTE]
> <span data-ttu-id="8037f-129">Názvy skupin rozlišují malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="8037f-129">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="8037f-130">Související prostředky</span><span class="sxs-lookup"><span data-stu-id="8037f-130">Related resources</span></span>

* [<span data-ttu-id="8037f-131">Začínáme</span><span class="sxs-lookup"><span data-stu-id="8037f-131">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="8037f-132">Centra</span><span class="sxs-lookup"><span data-stu-id="8037f-132">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="8037f-133">Publikování do Azure</span><span class="sxs-lookup"><span data-stu-id="8037f-133">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
