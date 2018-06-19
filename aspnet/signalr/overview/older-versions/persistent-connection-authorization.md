---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: Ověřování a autorizace pro trvalé připojení SignalR (SignalR 1.x) | Microsoft Docs
author: pfletcher
description: Toto téma popisuje, jak vynutit autorizaci u na trvalé připojení. Obecné informace o integraci do aplikace pomocí SignalR zabezpečení...
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
ms.locfileid: "28036099"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a><span data-ttu-id="b8a5a-104">Ověřování a autorizace pro trvalé připojení SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="b8a5a-104">Authentication and Authorization for SignalR Persistent Connections (SignalR 1.x)</span></span>
====================
<span data-ttu-id="b8a5a-105">podle [Patrik Fletcher](https://github.com/pfletcher), [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b8a5a-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b8a5a-106">Toto téma popisuje, jak vynutit autorizaci u na trvalé připojení.</span><span class="sxs-lookup"><span data-stu-id="b8a5a-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="b8a5a-107">Obecné informace o integraci zabezpečení do aplikace SignalR najdete v tématu [Úvod k zabezpečení](index.md).</span><span class="sxs-lookup"><span data-stu-id="b8a5a-107">For general information about integrating security into a SignalR application, see [Introduction to Security](index.md).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="b8a5a-108">Vynutit autorizaci</span><span class="sxs-lookup"><span data-stu-id="b8a5a-108">Enforce authorization</span></span>

<span data-ttu-id="b8a5a-109">K vynucení pravidel autorizace při použití [připojení PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) je nutné přepsat `AuthorizeRequest` metoda.</span><span class="sxs-lookup"><span data-stu-id="b8a5a-109">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="b8a5a-110">Nelze použít `Authorize` atribut s trvalým připojením.</span><span class="sxs-lookup"><span data-stu-id="b8a5a-110">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="b8a5a-111">`AuthorizeRequest` Metoda je volána rámcem SignalR před každým požadavkem k ověření, že uživatel má oprávnění provést požadovanou akci.</span><span class="sxs-lookup"><span data-stu-id="b8a5a-111">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="b8a5a-112">`AuthorizeRequest` Metoda není volána z klienta; místo toho ověření uživatele prostřednictvím mechanismu standardní ověřování vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="b8a5a-112">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="b8a5a-113">Následující příklad ukazuje, jak omezit požadavky na ověření uživatelé.</span><span class="sxs-lookup"><span data-stu-id="b8a5a-113">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="b8a5a-114">Můžete přidat všechny přizpůsobené autorizace logiku v metodě AuthorizeRequest; například kontrola, zda uživatel patří do určité role.</span><span class="sxs-lookup"><span data-stu-id="b8a5a-114">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
