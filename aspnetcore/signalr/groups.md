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
# <a name="manage-users-and-groups-in-signalr"></a>Spravovat uživatele a skupiny v systému SignalR

Podle [Brennan Conroy](https://github.com/BrennanConroy)

SignalR umožňuje zpráv k odeslání do všech připojení přidružené určitému uživateli, jakož i s názvem skupiny připojení.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(jak ke stažení)](xref:tutorials/index#how-to-download-a-sample)

## <a name="users-in-signalr"></a>Uživatelé v systému SignalR

SignalR umožňuje odesílání zpráv do všech připojení spojené s konkrétním uživatelem. Ve výchozím nastavení používá SignalR `ClaimTypes.NameIdentifier` z `ClaimsPrincipal` přidružené k připojení jako identifikátor uživatele. Jeden uživatel může mít víc připojení k aplikaci SignalR. Uživatel například může být připojen na své ploše a také svůj telefon. Každé zařízení má samostatné připojení SignalR, ale jsou všechny přidružené se stejným uživatelem. Pokud uživateli odeslána zpráva, všechna připojení přidružené k tomuto uživateli se zobrazí zpráva.

Odeslání zprávy pro konkrétního uživatele předáním identifikátor uživatele, který `User` fungovat ve své metodě rozbočovače, jak je znázorněno v následujícím příkladu:

> [!NOTE]
> Identifikátor uživatele je malá a velká písmena.

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

Identifikátor uživatele lze přizpůsobit tak, že vytvoříte `IUserIdProvider`a její v registrací `ConfigureServices`.

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> AddSignalR musí být voláno před registrací vaše vlastní služby SignalR.

## <a name="groups-in-signalr"></a>Skupiny v systému SignalR

Skupinu je kolekce přidružené k názvu připojení. Pro všechna připojení ve skupině nelze odesílat zprávy. Skupiny jsou doporučeným způsobem, jak odeslat připojení nebo více připojení, protože tyto skupiny se spravují přes aplikaci. Připojení může být členem více skupin. Díky tomu skupiny ideální pro podobný chatovací aplikace, kde každý prostor může být reprezentován jako skupinu. Připojení se dají přidat nebo odebrat ze skupiny prostřednictvím `AddToGroupAsync` a `RemoveFromGroupAsync` metody.

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

Členství ve skupině není při opětovném navázání připojení zachován. Připojení je potřeba znovu vstoupit skupině, když je znovu vytvořit. Není možné počet členů skupiny, protože tyto informace nejsou k dispozici, pokud je aplikace škálovat na více serverů.

> [!NOTE]
> Názvy skupin rozlišují velká a malá písmena.

## <a name="related-resources"></a>Související informační zdroje

* [Začínáme](xref:signalr/get-started)
* [Centra](xref:signalr/hubs)
* [Publikování do Azure](xref:signalr/publish-to-azure-web-app)
