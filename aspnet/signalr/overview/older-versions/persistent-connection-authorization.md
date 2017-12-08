---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: "Ověřování a autorizace pro trvalé připojení SignalR (SignalR 1.x) | Microsoft Docs"
author: pfletcher
description: "Toto téma popisuje, jak vynutit autorizaci u na trvalé připojení. Obecné informace o integraci do aplikace pomocí SignalR zabezpečení..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 4c036ddf1e20e3a3be7b043d90b594292013f6c2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a>Ověřování a autorizace pro trvalé připojení SignalR (SignalR 1.x)
====================
podle [Patrik Fletcher](https://github.com/pfletcher), [tní FitzMacken](https://github.com/tfitzmac)

> Toto téma popisuje, jak vynutit autorizaci u na trvalé připojení. Obecné informace o integraci zabezpečení do aplikace SignalR najdete v tématu [Úvod k zabezpečení](index.md).


## <a name="enforce-authorization"></a>Vynutit autorizaci

K vynucení pravidel autorizace při použití [připojení PersistentConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) je nutné přepsat `AuthorizeRequest` metoda. Nelze použít `Authorize` atribut s trvalým připojením. `AuthorizeRequest` Metoda je volána rámcem SignalR před každým požadavkem k ověření, že uživatel má oprávnění provést požadovanou akci. `AuthorizeRequest` Metoda není volána z klienta; místo toho ověření uživatele prostřednictvím mechanismu standardní ověřování vaší aplikace.

Následující příklad ukazuje, jak omezit požadavky na ověření uživatelé.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

Můžete přidat všechny přizpůsobené autorizace logiku v metodě AuthorizeRequest; například kontrola, zda uživatel patří do určité role.
