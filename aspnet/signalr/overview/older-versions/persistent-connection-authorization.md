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
ms.openlocfilehash: 2e97dfd03c61b110325c41a992b4af490fcd17de
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a>Ověřování a autorizace pro trvalé připojení SignalR (SignalR 1.x)
====================
podle [Patrik Fletcher](https://github.com/pfletcher), [tní FitzMacken](https://github.com/tfitzmac)

> Toto téma popisuje, jak vynutit autorizaci u na trvalé připojení. Obecné informace o integraci zabezpečení do aplikace SignalR najdete v tématu [Úvod k zabezpečení](index.md).


## <a name="enforce-authorization"></a>Vynutit autorizaci

K vynucení pravidel autorizace při použití [připojení PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) je nutné přepsat `AuthorizeRequest` metoda. Nelze použít `Authorize` atribut s trvalým připojením. `AuthorizeRequest` Metoda je volána rámcem SignalR před každým požadavkem k ověření, že uživatel má oprávnění provést požadovanou akci. `AuthorizeRequest` Metoda není volána z klienta; místo toho ověření uživatele prostřednictvím mechanismu standardní ověřování vaší aplikace.

Následující příklad ukazuje, jak omezit požadavky na ověření uživatelé.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

Můžete přidat všechny přizpůsobené autorizace logiku v metodě AuthorizeRequest; například kontrola, zda uživatel patří do určité role.
