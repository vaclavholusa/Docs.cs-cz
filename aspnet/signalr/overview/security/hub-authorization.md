---
uid: signalr/overview/security/hub-authorization
title: "Ověřování a autorizace pro rozbočovače SignalR | Microsoft Docs"
author: pfletcher
description: "Toto téma popisuje, jak omezit, které uživatelé nebo role mají přístup k metod rozbočovače. Verze softwaru použitým v tomto tématu Visual Studio 2013 .NET 4.5 SignalR sunout..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/05/2015
ms.topic: article
ms.assetid: a610c796-c131-473c-baef-2e6c568cb2a2
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: cb0f06a3ca2b39a4a952c33cea70136c7c5af7a8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="authentication-and-authorization-for-signalr-hubs"></a>Ověřování a autorizace pro rozbočovače SignalR
====================
podle [Patrik Fletcher](https://github.com/pfletcher), [tní FitzMacken](https://github.com/tfitzmac)

> Toto téma popisuje, jak omezit, které uživatelé nebo role mají přístup k metod rozbočovače. 
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


## <a name="overview"></a>Přehled

Toto téma obsahuje následující oddíly:

- [Autorizovat atribut](#authorizeattribute)
- [Vyžadovat ověřování pro všechny rozbočovače](#requireauth)
- [Vlastní autorizace](#custom)
- [Předat informace pro ověřování klientů](#passauth)
- [Možnosti ověřování pro klienty .NET](#authoptions)

    - [Soubor cookie ověřování pomocí formulářů](#cookie)
    - [Ověřování systému Windows](#windows)
    - [Hlavička připojení](#header)
    - [Certifikát](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a>Autorizovat atribut

Funkce SignalR poskytuje [Autorizovat](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) atribut k určení, které uživatele nebo role mají přístup do rozbočovače nebo metoda. Tento atribut je umístěn ve `Microsoft.AspNet.SignalR` oboru názvů. Můžete použít `Authorize` atribut rozbočovač nebo konkrétní metody v rozbočovači. Pokud použijete `Authorize` atribut třídy rozbočovače, požadavek na autorizaci zadaný platí pro všechny metody v rozbočovači. Toto téma obsahuje příklady různých typů autorizace požadavky, které můžete použít. Bez `Authorize` atribut připojeného klient může získat žádné veřejnou metodu v rozbočovači.

Pokud jste definovali role ve vaší webové aplikaci s názvem "Admin", můžete zadat jenom uživatelé v dané roli můžete získat přístup k rozbočovači následujícím kódem.

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

Nebo můžete určit, že obsahuje rozbočovači jedna metoda, která je k dispozici všem uživatelům a druhý metoda, která je k dispozici pro ověřené uživatele, jenom jak je uvedeno níže.

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

Následující příklady adres autorizace různé scénáře:

- `[Authorize]`– jen ověření uživatelé
- `[Authorize(Roles = "Admin,Manager")]`– jen ověření uživatelé v zadaných rolí
- `[Authorize(Users = "user1,user2")]`– pouze pro ověřené uživatele s zadaná uživatelská jména
- `[Authorize(RequireOutgoing=false)]`– pouze pro ověřené uživatele můžete vyvolat rozbočovače, ale volání ze serveru zpět klientům nejsou omezeny autorizace, jako je třeba při pouze určitým uživatelům poslat zprávu, ale všechny ostatní mohou přijímat zprávy. Vlastnost hodnotu RequireOutgoing lze použít pouze k centru celý není pro jednotlivce metody v rozbočovači. Pokud hodnotu RequireOutgoing není nastavena na hodnotu false, jsou pouze uživatelé, kteří splní požadavek na autorizaci volat ze serveru.

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a>Vyžadovat ověřování pro všechny rozbočovače

Můžete vyžadovat jeho ověření pro všechny rozbočovače a metody rozbočovačů ve vaší aplikaci pomocí volání [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) metoda při spuštění aplikace. Tuto metodu můžete použít, když máte více rozbočovače a chcete vynutit požadavku na ověřování pro všechny z nich. Pomocí této metody nemůžete zadat požadavky pro role, uživatele nebo odchozí autorizace. Můžete pouze zadat, že přístup k metody rozbočovače, je omezený ověřeným uživatelům. Ale můžete také použít atribut autorizovat centra nebo metody k určení požadavků na další. Všechny požadavky, které zadáte v atributu se přidá do základní požadavek ověřování.

Následující příklad ukazuje spuštění souboru, který omezuje všechny metody rozbočovače ověřeným uživatelům.

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

Když zavoláte `RequireAuthentication()` metoda po zpracování žádost SignalR, vyvolá výjimku SignalR `InvalidOperationException` výjimka. SignalR vyvolá výjimku, protože modul nelze přidat do třídy HubPipeline po kanálu metoda byla volána. Předchozí příklad ukazuje volání `RequireAuthentication` metoda v `Configuration` metodu, která se spustí jednou před zpracování prvního požadavku.

<a id="custom"></a>

## <a name="customized-authorization"></a>Vlastní autorizace

Pokud potřebujete přizpůsobit, jakým způsobem je určován autorizace, můžete vytvořit třídu, která pochází z `AuthorizeAttribute` a přepsat [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) metoda. Pro každou žádost SignalR volá tuto metodu za účelem určení, zda je uživatel oprávnění pro dokončení požadavku. V metodě přepsaného zadejte pomocí potřebné logiky pro váš scénář autorizace. Následující příklad ukazuje, jak vynutit autorizaci u prostřednictvím založené na deklaracích identity.

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a>Předat informace pro ověřování klientů

Musíte použít informace o ověřování v kód, který běží na klientovi. Při volání metody na straně klienta, které předat požadované informace. Například metoda chatovací aplikace může předat jako parametr uživatelské jméno osoby, publikování zprávu, jak je uvedeno níže.

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

Nebo můžete vytvořit objekt tak, aby představují informace o ověřování a tento objekt předat jako parametr, jak je uvedeno níže.

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

Nikdy byste neměli předat id připojení pro jednoho klienta pro ostatní klienty, např. uživatel se zlými úmysly může použít tak, aby napodoboval žádost od tohoto klienta.

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a>Možnosti ověřování pro klienty .NET

Pokud máte klienta .NET, jako je například konzolovou aplikaci, která komunikuje s rozbočovači, který je omezen na ověřené uživatele, můžete předat ověřovací pověření v souboru cookie, záhlaví připojení nebo certifikát. Příklady v této části ukazují, jak použít tyto různé metody pro ověřování uživatele. Nejsou plně funkční aplikace SignalR. Další informace o klientech .NET pomocí nástroje SignalR najdete v tématu [centra API Průvodce – klient .NET](../guide-to-the-api/hubs-api-guide-net-client.md).

<a id="cookie"></a>

### <a name="cookie"></a>Soubor cookie

Když váš klient .NET pracuje s rozbočovači, který používá ověřování ASP.NET pomocí formulářů, musíte ručně nastavit ověřovacího souboru cookie pro připojení. Přidejte k danému souboru cookie `CookieContainer` vlastnost [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) objektu. Následující příklad ukazuje konzolovou aplikaci, která načte soubor cookie ověřování z webové stránky a přidá tento soubor cookie k připojení.

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

Konzolové aplikace odešle pověření **www.contoso.com/RemoteLogin** který může odkazovat na prázdnou stránku, která obsahuje následující souboru kódu na pozadí.

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a>Ověřování systému Windows

Pokud používáte ověřování systému Windows, můžete předat pověření aktuálního uživatele pomocí [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) vlastnost. Nastavte pověření pro připojení na hodnotu DefaultCredentials.

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a>Hlavička připojení

Pokud vaše aplikace není pomocí souborů cookie, můžete předat informace o uživateli v hlavičce připojení. Můžete například předat token v hlavičce připojení.

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

V centru, pak by ověřit token uživatele.

<a id="certificate"></a>

### <a name="certificate"></a>certifikát

Můžete předat klientský certifikát k ověření uživatele. Při vytváření připojení, přidání certifikátu. Následující příklad ukazuje jenom postup přidání klientský certifikát na připojení; úplné konzolové aplikace nejsou zobrazeny. Použije [certifikátu x 509](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) třída, která poskytuje několika různými způsoby, abyste mohli vytvořit certifikát.

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
