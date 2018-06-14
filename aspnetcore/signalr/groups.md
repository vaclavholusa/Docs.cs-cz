---
title: Spravovat uživatele a skupiny v systému SignalR
author: rachelappel
description: Přehled technologie ASP.NET Core SignalR uživatelů a skupin správy.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/groups
ms.openlocfilehash: 2a2f129863cf7d5cdfa3c0e5b2d901af52144671
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/12/2018
ms.locfileid: "35358460"
---
# <a name="manage-users-and-groups-in-signalr"></a><span data-ttu-id="f44a0-103">Spravovat uživatele a skupiny v systému SignalR</span><span class="sxs-lookup"><span data-stu-id="f44a0-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="f44a0-104">Podle [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="f44a0-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="f44a0-105">SignalR umožňuje zpráv k odeslání do všech připojení přidružené určitému uživateli, jakož i s názvem skupiny připojení.</span><span class="sxs-lookup"><span data-stu-id="f44a0-105">SignalR allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="f44a0-106">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(jak ke stažení)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="f44a0-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="users-in-signalr"></a><span data-ttu-id="f44a0-107">Uživatelé v systému SignalR</span><span class="sxs-lookup"><span data-stu-id="f44a0-107">Users in SignalR</span></span>

<span data-ttu-id="f44a0-108">SignalR umožňuje odesílání zpráv do všech připojení spojené s konkrétním uživatelem.</span><span class="sxs-lookup"><span data-stu-id="f44a0-108">SignalR allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="f44a0-109">Ve výchozím nastavení používá SignalR `ClaimTypes.NameIdentifier` z `ClaimsPrincipal` přidružené k připojení jako identifikátor uživatele.</span><span class="sxs-lookup"><span data-stu-id="f44a0-109">By default SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="f44a0-110">Jeden uživatel může mít víc připojení k aplikaci SignalR.</span><span class="sxs-lookup"><span data-stu-id="f44a0-110">A single user can have multiple connections to a SignalR application.</span></span> <span data-ttu-id="f44a0-111">Uživatel například může být připojen na své ploše a také svůj telefon.</span><span class="sxs-lookup"><span data-stu-id="f44a0-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="f44a0-112">Každé zařízení má samostatné připojení SignalR, ale jsou všechny přidružené se stejným uživatelem.</span><span class="sxs-lookup"><span data-stu-id="f44a0-112">Each device has a separate SignalR connection, but they are all associated with the same user.</span></span> <span data-ttu-id="f44a0-113">Pokud uživateli odeslána zpráva, všechna připojení přidružené k tomuto uživateli se zobrazí zpráva.</span><span class="sxs-lookup"><span data-stu-id="f44a0-113">If a message is sent to the user, all of the connections associated with that user will receive the message.</span></span>

<span data-ttu-id="f44a0-114">Odeslání zprávy pro konkrétního uživatele předáním identifikátor uživatele, který `User` fungovat ve své metodě rozbočovače, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="f44a0-114">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="f44a0-115">Identifikátor uživatele je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="f44a0-115">The user identifier is case-sensitive.</span></span>

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

<span data-ttu-id="f44a0-116">Identifikátor uživatele lze přizpůsobit tak, že vytvoříte `IUserIdProvider`a její v registrací `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f44a0-116">The user identifier can be customized by creating an `IUserIdProvider`, and registering it in `ConfigureServices`.</span></span>

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> <span data-ttu-id="f44a0-117">AddSignalR musí být voláno před registrací vaše vlastní služby SignalR.</span><span class="sxs-lookup"><span data-stu-id="f44a0-117">AddSignalR must be called before registering your custom SignalR services.</span></span>

## <a name="groups-in-signalr"></a><span data-ttu-id="f44a0-118">Skupiny v systému SignalR</span><span class="sxs-lookup"><span data-stu-id="f44a0-118">Groups in SignalR</span></span>

<span data-ttu-id="f44a0-119">Skupinu je kolekce přidružené k názvu připojení.</span><span class="sxs-lookup"><span data-stu-id="f44a0-119">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="f44a0-120">Pro všechna připojení ve skupině nelze odesílat zprávy.</span><span class="sxs-lookup"><span data-stu-id="f44a0-120">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="f44a0-121">Skupiny jsou doporučeným způsobem, jak odeslat připojení nebo více připojení, protože tyto skupiny se spravují přes aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f44a0-121">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="f44a0-122">Připojení může být členem více skupin.</span><span class="sxs-lookup"><span data-stu-id="f44a0-122">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="f44a0-123">Díky tomu skupiny ideální pro podobný chatovací aplikace, kde každý prostor může být reprezentován jako skupinu.</span><span class="sxs-lookup"><span data-stu-id="f44a0-123">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="f44a0-124">Připojení se dají přidat nebo odebrat ze skupiny prostřednictvím `AddToGroupAsync` a `RemoveFromGroupAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="f44a0-124">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="f44a0-125">Členství ve skupině není při opětovném navázání připojení zachován.</span><span class="sxs-lookup"><span data-stu-id="f44a0-125">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="f44a0-126">Připojení je potřeba znovu vstoupit skupině, když je znovu vytvořit.</span><span class="sxs-lookup"><span data-stu-id="f44a0-126">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="f44a0-127">Není možné počet členů skupiny, protože tyto informace nejsou k dispozici, pokud je aplikace škálovat na více serverů.</span><span class="sxs-lookup"><span data-stu-id="f44a0-127">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

> [!NOTE]
> <span data-ttu-id="f44a0-128">Názvy skupin rozlišují velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="f44a0-128">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="f44a0-129">Související informační zdroje</span><span class="sxs-lookup"><span data-stu-id="f44a0-129">Related resources</span></span>

* [<span data-ttu-id="f44a0-130">Začínáme</span><span class="sxs-lookup"><span data-stu-id="f44a0-130">Get started</span></span>](xref:signalr/get-started)
* [<span data-ttu-id="f44a0-131">Centra</span><span class="sxs-lookup"><span data-stu-id="f44a0-131">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="f44a0-132">Publikování do Azure</span><span class="sxs-lookup"><span data-stu-id="f44a0-132">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
