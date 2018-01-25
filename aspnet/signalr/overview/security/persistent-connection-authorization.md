---
uid: signalr/overview/security/persistent-connection-authorization
title: "Ověřování a autorizace pro trvalé připojení SignalR | Microsoft Docs"
author: pfletcher
description: "Toto téma popisuje, jak vynutit autorizaci u na trvalé připojení. Obecné informace o integraci do aplikace pomocí SignalR zabezpečení..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: d559cfa21f6444b2361fd003b9ce3d2c9c6c57a4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="authentication-and-authorization-for-signalr-persistent-connections"></a>Ověřování a autorizace pro trvalé připojení SignalR
====================
podle [Patrik Fletcher](https://github.com/pfletcher), [tní FitzMacken](https://github.com/tfitzmac)

> Toto téma popisuje, jak vynutit autorizaci u na trvalé připojení. Obecné informace o integraci zabezpečení do aplikace SignalR najdete v tématu [Úvod k zabezpečení](introduction-to-security.md). 
> 
> ## <a name="software-versions-used-in-this-topic"></a>Verze softwaru použitým v tomto tématu
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR verze 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Předchozí verze tohoto tématu
> 
> Informace o předchozích verzích SignalR najdete v tématu [starší verze funkce SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Dotazy a připomínky
> 
> Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky. Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com/).


## <a name="enforce-authorization"></a>Vynutit autorizaci

K vynucení pravidel autorizace při použití [připojení PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) je nutné přepsat `AuthorizeRequest` metoda. Nelze použít `Authorize` atribut s trvalým připojením. `AuthorizeRequest` Metoda je volána rámcem SignalR před každým požadavkem k ověření, že uživatel má oprávnění provést požadovanou akci. `AuthorizeRequest` Metoda není volána z klienta; místo toho ověření uživatele prostřednictvím mechanismu standardní ověřování vaší aplikace.

Následující příklad ukazuje, jak omezit požadavky na ověření uživatelé.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

Můžete přidat všechny přizpůsobené autorizace logiku v metodě AuthorizeRequest; například kontrola, zda uživatel patří do určité role.
