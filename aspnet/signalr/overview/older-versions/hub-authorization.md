---
uid: signalr/overview/older-versions/hub-authorization
title: "Ověřování a autorizace pro rozbočovače SignalR (SignalR 1.x) | Microsoft Docs"
author: pfletcher
description: "Toto téma popisuje, jak omezit, které uživatelé nebo role mají přístup k metod rozbočovače."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 3d2dfc0e-eac2-4076-a468-325d3d01cc7b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: e52e39bf9c66419e18bf78036138d1f15376f2be
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a>Ověřování a autorizace pro rozbočovače SignalR (SignalR 1.x)
====================
podle [Patrik Fletcher](https://github.com/pfletcher), [tní FitzMacken](https://github.com/tfitzmac)

> Toto téma popisuje, jak omezit, které uživatelé nebo role mají přístup k metod rozbočovače.


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

Funkce SignalR poskytuje [Autorizovat](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) atribut k určení, které uživatele nebo role mají přístup do rozbočovače nebo metoda. Tento atribut je umístěn ve `Microsoft.AspNet.SignalR` oboru názvů. Můžete použít `Authorize` atribut rozbočovač nebo konkrétní metody v rozbočovači. Pokud použijete `Authorize` atribut třídy rozbočovače, požadavek na autorizaci zadaný platí pro všechny metody v rozbočovači. Níže jsou zobrazena různé typy autorizace požadavky, které můžete provést. Bez `Authorize` atribut všechny veřejné metody v rozbočovači jsou k dispozici pro klienta, který je připojený k rozbočovači.

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

Můžete vyžadovat jeho ověření pro všechny rozbočovače a metody rozbočovačů ve vaší aplikaci pomocí volání [RequireAuthentication](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) metoda při spuštění aplikace. Tuto metodu můžete použít, když máte více rozbočovače a chcete vynutit požadavku na ověřování pro všechny z nich. Pomocí této metody nelze zadat role, uživatele nebo odchozí autorizace. Můžete pouze zadat, že přístup k metody rozbočovače, je omezený ověřeným uživatelům. Ale můžete také použít atribut autorizovat centra nebo metody k určení požadavků na další. Kromě požadavek na základní ověřování je použité žádné požadavky, které zadáte v atributech.

Následující příklad ukazuje soubor Global.asax, který omezuje všechny metody rozbočovače ověřeným uživatelům.

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

Když zavoláte `RequireAuthentication()` metoda po zpracování žádost SignalR, vyvolá výjimku SignalR `InvalidOperationException` výjimka. Tato výjimka je vyvolána, protože modul nelze přidat do třídy HubPipeline po kanálu metoda byla volána. Předchozí příklad ukazuje volání `RequireAuthentication` metoda v `Application_Start` metodu, která se spustí jednou před zpracování prvního požadavku.

<a id="custom"></a>

## <a name="customized-authorization"></a>Vlastní autorizace

Pokud potřebujete přizpůsobit, jakým způsobem je určován autorizace, můžete vytvořit třídu, která pochází z `AuthorizeAttribute` a přepsat [UserAuthorized](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) metoda. Tato metoda je volána pro každý požadavek k určení, zda je uživatel oprávnění pro dokončení požadavku. V metodě přepsaného zadejte pomocí potřebné logiky pro váš scénář autorizace. Následující příklad ukazuje, jak vynutit autorizaci u prostřednictvím založené na deklaracích identity.

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

Když váš klient .NET pracuje s rozbočovači, který používá ověřování ASP.NET pomocí formulářů, musíte ručně nastavit ověřovacího souboru cookie pro připojení. Přidejte k danému souboru cookie `CookieContainer` vlastnost [HubConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) objektu. Následující příklad ukazuje konzolovou aplikaci, která načte soubor cookie ověřování z webové stránky a přidá tento soubor cookie k připojení. Adresa URL `https://www.contoso.com/RemoteLogin` v příkladu odkazuje na webovou stránku, která je potřeba vytvořit. Stránka by načíst odeslané uživatelské jméno a heslo a pokus o přihlášení uživatele s přihlašovacími údaji.

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

Konzolové aplikace odešle pověření www.contoso.com/RemoteLogin kterého může odkazovat na prázdnou stránku, která obsahuje následující souboru kódu na pozadí.

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a>Ověřování systému Windows

Pokud používáte ověřování systému Windows, můžete předat pověření aktuálního uživatele pomocí [DefaultCredentials](https://msdn.microsoft.com/en-us/library/system.net.credentialcache.defaultcredentials.aspx) vlastnost. Nastavte pověření pro připojení na hodnotu DefaultCredentials.

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a>Hlavička připojení

Pokud vaše aplikace není pomocí souborů cookie, můžete předat informace o uživateli v hlavičce připojení. Můžete například předat token v hlavičce připojení.

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

V centru, pak by ověřit token uživatele.

<a id="certificate"></a>

### <a name="certificate"></a>certifikát

Můžete předat klientský certifikát k ověření uživatele. Při vytváření připojení, přidání certifikátu. Následující příklad ukazuje jenom postup přidání klientský certifikát na připojení; úplné konzolové aplikace nejsou zobrazeny. Použije [certifikátu x 509](https://msdn.microsoft.com/en-us/library/system.security.cryptography.x509certificates.x509certificate.aspx) třída, která poskytuje několika různými způsoby, abyste mohli vytvořit certifikát.

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
