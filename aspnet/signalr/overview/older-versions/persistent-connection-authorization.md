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
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a><span data-ttu-id="0ba81-104">Ověřování a autorizace pro trvalé připojení SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="0ba81-104">Authentication and Authorization for SignalR Persistent Connections (SignalR 1.x)</span></span>
====================
<span data-ttu-id="0ba81-105">podle [Patrik Fletcher](https://github.com/pfletcher), [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="0ba81-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="0ba81-106">Toto téma popisuje, jak vynutit autorizaci u na trvalé připojení.</span><span class="sxs-lookup"><span data-stu-id="0ba81-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="0ba81-107">Obecné informace o integraci zabezpečení do aplikace SignalR najdete v tématu [Úvod k zabezpečení](index.md).</span><span class="sxs-lookup"><span data-stu-id="0ba81-107">For general information about integrating security into a SignalR application, see [Introduction to Security](index.md).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="0ba81-108">Vynutit autorizaci</span><span class="sxs-lookup"><span data-stu-id="0ba81-108">Enforce authorization</span></span>

<span data-ttu-id="0ba81-109">K vynucení pravidel autorizace při použití [připojení PersistentConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) je nutné přepsat `AuthorizeRequest` metoda.</span><span class="sxs-lookup"><span data-stu-id="0ba81-109">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="0ba81-110">Nelze použít `Authorize` atribut s trvalým připojením.</span><span class="sxs-lookup"><span data-stu-id="0ba81-110">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="0ba81-111">`AuthorizeRequest` Metoda je volána rámcem SignalR před každým požadavkem k ověření, že uživatel má oprávnění provést požadovanou akci.</span><span class="sxs-lookup"><span data-stu-id="0ba81-111">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="0ba81-112">`AuthorizeRequest` Metoda není volána z klienta; místo toho ověření uživatele prostřednictvím mechanismu standardní ověřování vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="0ba81-112">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="0ba81-113">Následující příklad ukazuje, jak omezit požadavky na ověření uživatelé.</span><span class="sxs-lookup"><span data-stu-id="0ba81-113">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="0ba81-114">Můžete přidat všechny přizpůsobené autorizace logiku v metodě AuthorizeRequest; například kontrola, zda uživatel patří do určité role.</span><span class="sxs-lookup"><span data-stu-id="0ba81-114">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
