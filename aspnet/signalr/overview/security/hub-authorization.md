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
<a name="authentication-and-authorization-for-signalr-hubs"></a><span data-ttu-id="3eaba-104">Ověřování a autorizace pro rozbočovače SignalR</span><span class="sxs-lookup"><span data-stu-id="3eaba-104">Authentication and Authorization for SignalR Hubs</span></span>
====================
<span data-ttu-id="3eaba-105">podle [Patrik Fletcher](https://github.com/pfletcher), [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3eaba-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="3eaba-106">Toto téma popisuje, jak omezit, které uživatelé nebo role mají přístup k metod rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="3eaba-106">This topic describes how to restrict which users or roles can access hub methods.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="3eaba-107">Verze softwaru použitým v tomto tématu</span><span class="sxs-lookup"><span data-stu-id="3eaba-107">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="3eaba-108">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="3eaba-108">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="3eaba-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="3eaba-109">.NET 4.5</span></span>
> - <span data-ttu-id="3eaba-110">SignalR verze 2</span><span class="sxs-lookup"><span data-stu-id="3eaba-110">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="3eaba-111">Předchozí verze tohoto tématu</span><span class="sxs-lookup"><span data-stu-id="3eaba-111">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="3eaba-112">Informace o předchozích verzích SignalR najdete v tématu [starší verze funkce SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="3eaba-112">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="3eaba-113">Dotazy a připomínky</span><span class="sxs-lookup"><span data-stu-id="3eaba-113">Questions and comments</span></span>
> 
> <span data-ttu-id="3eaba-114">Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="3eaba-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="3eaba-115">Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="3eaba-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="3eaba-116">Přehled</span><span class="sxs-lookup"><span data-stu-id="3eaba-116">Overview</span></span>

<span data-ttu-id="3eaba-117">Toto téma obsahuje následující oddíly:</span><span class="sxs-lookup"><span data-stu-id="3eaba-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="3eaba-118">Autorizovat atribut</span><span class="sxs-lookup"><span data-stu-id="3eaba-118">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="3eaba-119">Vyžadovat ověřování pro všechny rozbočovače</span><span class="sxs-lookup"><span data-stu-id="3eaba-119">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="3eaba-120">Vlastní autorizace</span><span class="sxs-lookup"><span data-stu-id="3eaba-120">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="3eaba-121">Předat informace pro ověřování klientů</span><span class="sxs-lookup"><span data-stu-id="3eaba-121">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="3eaba-122">Možnosti ověřování pro klienty .NET</span><span class="sxs-lookup"><span data-stu-id="3eaba-122">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="3eaba-123">Soubor cookie ověřování pomocí formulářů</span><span class="sxs-lookup"><span data-stu-id="3eaba-123">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="3eaba-124">Ověřování systému Windows</span><span class="sxs-lookup"><span data-stu-id="3eaba-124">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="3eaba-125">Hlavička připojení</span><span class="sxs-lookup"><span data-stu-id="3eaba-125">Connection header</span></span>](#header)
    - [<span data-ttu-id="3eaba-126">Certifikát</span><span class="sxs-lookup"><span data-stu-id="3eaba-126">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="3eaba-127">Autorizovat atribut</span><span class="sxs-lookup"><span data-stu-id="3eaba-127">Authorize attribute</span></span>

<span data-ttu-id="3eaba-128">Funkce SignalR poskytuje [Autorizovat](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) atribut k určení, které uživatele nebo role mají přístup do rozbočovače nebo metoda.</span><span class="sxs-lookup"><span data-stu-id="3eaba-128">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="3eaba-129">Tento atribut je umístěn ve `Microsoft.AspNet.SignalR` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="3eaba-129">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="3eaba-130">Můžete použít `Authorize` atribut rozbočovač nebo konkrétní metody v rozbočovači.</span><span class="sxs-lookup"><span data-stu-id="3eaba-130">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="3eaba-131">Pokud použijete `Authorize` atribut třídy rozbočovače, požadavek na autorizaci zadaný platí pro všechny metody v rozbočovači.</span><span class="sxs-lookup"><span data-stu-id="3eaba-131">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="3eaba-132">Toto téma obsahuje příklady různých typů autorizace požadavky, které můžete použít.</span><span class="sxs-lookup"><span data-stu-id="3eaba-132">This topic provides examples of the different types of authorization requirements that you can apply.</span></span> <span data-ttu-id="3eaba-133">Bez `Authorize` atribut připojeného klient může získat žádné veřejnou metodu v rozbočovači.</span><span class="sxs-lookup"><span data-stu-id="3eaba-133">Without the `Authorize` attribute, a connected client can access any public method on the hub.</span></span>

<span data-ttu-id="3eaba-134">Pokud jste definovali role ve vaší webové aplikaci s názvem "Admin", můžete zadat jenom uživatelé v dané roli můžete získat přístup k rozbočovači následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="3eaba-134">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="3eaba-135">Nebo můžete určit, že obsahuje rozbočovači jedna metoda, která je k dispozici všem uživatelům a druhý metoda, která je k dispozici pro ověřené uživatele, jenom jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="3eaba-135">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="3eaba-136">Následující příklady adres autorizace různé scénáře:</span><span class="sxs-lookup"><span data-stu-id="3eaba-136">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="3eaba-137">`[Authorize]`– jen ověření uživatelé</span><span class="sxs-lookup"><span data-stu-id="3eaba-137">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="3eaba-138">`[Authorize(Roles = "Admin,Manager")]`– jen ověření uživatelé v zadaných rolí</span><span class="sxs-lookup"><span data-stu-id="3eaba-138">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="3eaba-139">`[Authorize(Users = "user1,user2")]`– pouze pro ověřené uživatele s zadaná uživatelská jména</span><span class="sxs-lookup"><span data-stu-id="3eaba-139">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="3eaba-140">`[Authorize(RequireOutgoing=false)]`– pouze pro ověřené uživatele můžete vyvolat rozbočovače, ale volání ze serveru zpět klientům nejsou omezeny autorizace, jako je třeba při pouze určitým uživatelům poslat zprávu, ale všechny ostatní mohou přijímat zprávy.</span><span class="sxs-lookup"><span data-stu-id="3eaba-140">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="3eaba-141">Vlastnost hodnotu RequireOutgoing lze použít pouze k centru celý není pro jednotlivce metody v rozbočovači.</span><span class="sxs-lookup"><span data-stu-id="3eaba-141">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="3eaba-142">Pokud hodnotu RequireOutgoing není nastavena na hodnotu false, jsou pouze uživatelé, kteří splní požadavek na autorizaci volat ze serveru.</span><span class="sxs-lookup"><span data-stu-id="3eaba-142">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="3eaba-143">Vyžadovat ověřování pro všechny rozbočovače</span><span class="sxs-lookup"><span data-stu-id="3eaba-143">Require authentication for all hubs</span></span>

<span data-ttu-id="3eaba-144">Můžete vyžadovat jeho ověření pro všechny rozbočovače a metody rozbočovačů ve vaší aplikaci pomocí volání [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) metoda při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="3eaba-144">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="3eaba-145">Tuto metodu můžete použít, když máte více rozbočovače a chcete vynutit požadavku na ověřování pro všechny z nich.</span><span class="sxs-lookup"><span data-stu-id="3eaba-145">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="3eaba-146">Pomocí této metody nemůžete zadat požadavky pro role, uživatele nebo odchozí autorizace.</span><span class="sxs-lookup"><span data-stu-id="3eaba-146">With this method, you cannot specify requirements for role, user, or outgoing authorization.</span></span> <span data-ttu-id="3eaba-147">Můžete pouze zadat, že přístup k metody rozbočovače, je omezený ověřeným uživatelům.</span><span class="sxs-lookup"><span data-stu-id="3eaba-147">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="3eaba-148">Ale můžete také použít atribut autorizovat centra nebo metody k určení požadavků na další.</span><span class="sxs-lookup"><span data-stu-id="3eaba-148">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="3eaba-149">Všechny požadavky, které zadáte v atributu se přidá do základní požadavek ověřování.</span><span class="sxs-lookup"><span data-stu-id="3eaba-149">Any requirement you specify in an attribute is added to the basic requirement of authentication.</span></span>

<span data-ttu-id="3eaba-150">Následující příklad ukazuje spuštění souboru, který omezuje všechny metody rozbočovače ověřeným uživatelům.</span><span class="sxs-lookup"><span data-stu-id="3eaba-150">The following example shows a Startup file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="3eaba-151">Když zavoláte `RequireAuthentication()` metoda po zpracování žádost SignalR, vyvolá výjimku SignalR `InvalidOperationException` výjimka.</span><span class="sxs-lookup"><span data-stu-id="3eaba-151">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="3eaba-152">SignalR vyvolá výjimku, protože modul nelze přidat do třídy HubPipeline po kanálu metoda byla volána.</span><span class="sxs-lookup"><span data-stu-id="3eaba-152">SignalR throws this exception because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="3eaba-153">Předchozí příklad ukazuje volání `RequireAuthentication` metoda v `Configuration` metodu, která se spustí jednou před zpracování prvního požadavku.</span><span class="sxs-lookup"><span data-stu-id="3eaba-153">The previous example shows calling the `RequireAuthentication` method in the `Configuration` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="3eaba-154">Vlastní autorizace</span><span class="sxs-lookup"><span data-stu-id="3eaba-154">Customized authorization</span></span>

<span data-ttu-id="3eaba-155">Pokud potřebujete přizpůsobit, jakým způsobem je určován autorizace, můžete vytvořit třídu, která pochází z `AuthorizeAttribute` a přepsat [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) metoda.</span><span class="sxs-lookup"><span data-stu-id="3eaba-155">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="3eaba-156">Pro každou žádost SignalR volá tuto metodu za účelem určení, zda je uživatel oprávnění pro dokončení požadavku.</span><span class="sxs-lookup"><span data-stu-id="3eaba-156">For each request, SignalR invokes this method to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="3eaba-157">V metodě přepsaného zadejte pomocí potřebné logiky pro váš scénář autorizace.</span><span class="sxs-lookup"><span data-stu-id="3eaba-157">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="3eaba-158">Následující příklad ukazuje, jak vynutit autorizaci u prostřednictvím založené na deklaracích identity.</span><span class="sxs-lookup"><span data-stu-id="3eaba-158">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="3eaba-159">Předat informace pro ověřování klientů</span><span class="sxs-lookup"><span data-stu-id="3eaba-159">Pass authentication information to clients</span></span>

<span data-ttu-id="3eaba-160">Musíte použít informace o ověřování v kód, který běží na klientovi.</span><span class="sxs-lookup"><span data-stu-id="3eaba-160">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="3eaba-161">Při volání metody na straně klienta, které předat požadované informace.</span><span class="sxs-lookup"><span data-stu-id="3eaba-161">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="3eaba-162">Například metoda chatovací aplikace může předat jako parametr uživatelské jméno osoby, publikování zprávu, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="3eaba-162">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="3eaba-163">Nebo můžete vytvořit objekt tak, aby představují informace o ověřování a tento objekt předat jako parametr, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="3eaba-163">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="3eaba-164">Nikdy byste neměli předat id připojení pro jednoho klienta pro ostatní klienty, např. uživatel se zlými úmysly může použít tak, aby napodoboval žádost od tohoto klienta.</span><span class="sxs-lookup"><span data-stu-id="3eaba-164">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="3eaba-165">Možnosti ověřování pro klienty .NET</span><span class="sxs-lookup"><span data-stu-id="3eaba-165">Authentication options for .NET clients</span></span>

<span data-ttu-id="3eaba-166">Pokud máte klienta .NET, jako je například konzolovou aplikaci, která komunikuje s rozbočovači, který je omezen na ověřené uživatele, můžete předat ověřovací pověření v souboru cookie, záhlaví připojení nebo certifikát.</span><span class="sxs-lookup"><span data-stu-id="3eaba-166">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="3eaba-167">Příklady v této části ukazují, jak použít tyto různé metody pro ověřování uživatele.</span><span class="sxs-lookup"><span data-stu-id="3eaba-167">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="3eaba-168">Nejsou plně funkční aplikace SignalR.</span><span class="sxs-lookup"><span data-stu-id="3eaba-168">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="3eaba-169">Další informace o klientech .NET pomocí nástroje SignalR najdete v tématu [centra API Průvodce – klient .NET](../guide-to-the-api/hubs-api-guide-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="3eaba-169">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="3eaba-170">Soubor cookie</span><span class="sxs-lookup"><span data-stu-id="3eaba-170">Cookie</span></span>

<span data-ttu-id="3eaba-171">Když váš klient .NET pracuje s rozbočovači, který používá ověřování ASP.NET pomocí formulářů, musíte ručně nastavit ověřovacího souboru cookie pro připojení.</span><span class="sxs-lookup"><span data-stu-id="3eaba-171">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="3eaba-172">Přidejte k danému souboru cookie `CookieContainer` vlastnost [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) objektu.</span><span class="sxs-lookup"><span data-stu-id="3eaba-172">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="3eaba-173">Následující příklad ukazuje konzolovou aplikaci, která načte soubor cookie ověřování z webové stránky a přidá tento soubor cookie k připojení.</span><span class="sxs-lookup"><span data-stu-id="3eaba-173">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="3eaba-174">Konzolové aplikace odešle pověření **www.contoso.com/RemoteLogin** který může odkazovat na prázdnou stránku, která obsahuje následující souboru kódu na pozadí.</span><span class="sxs-lookup"><span data-stu-id="3eaba-174">The console app posts the credentials to **www.contoso.com/RemoteLogin** which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="3eaba-175">Ověřování systému Windows</span><span class="sxs-lookup"><span data-stu-id="3eaba-175">Windows authentication</span></span>

<span data-ttu-id="3eaba-176">Pokud používáte ověřování systému Windows, můžete předat pověření aktuálního uživatele pomocí [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) vlastnost.</span><span class="sxs-lookup"><span data-stu-id="3eaba-176">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="3eaba-177">Nastavte pověření pro připojení na hodnotu DefaultCredentials.</span><span class="sxs-lookup"><span data-stu-id="3eaba-177">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="3eaba-178">Hlavička připojení</span><span class="sxs-lookup"><span data-stu-id="3eaba-178">Connection header</span></span>

<span data-ttu-id="3eaba-179">Pokud vaše aplikace není pomocí souborů cookie, můžete předat informace o uživateli v hlavičce připojení.</span><span class="sxs-lookup"><span data-stu-id="3eaba-179">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="3eaba-180">Můžete například předat token v hlavičce připojení.</span><span class="sxs-lookup"><span data-stu-id="3eaba-180">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="3eaba-181">V centru, pak by ověřit token uživatele.</span><span class="sxs-lookup"><span data-stu-id="3eaba-181">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="3eaba-182">certifikát</span><span class="sxs-lookup"><span data-stu-id="3eaba-182">Certificate</span></span>

<span data-ttu-id="3eaba-183">Můžete předat klientský certifikát k ověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="3eaba-183">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="3eaba-184">Při vytváření připojení, přidání certifikátu.</span><span class="sxs-lookup"><span data-stu-id="3eaba-184">You add the certificate when creating the connection.</span></span> <span data-ttu-id="3eaba-185">Následující příklad ukazuje jenom postup přidání klientský certifikát na připojení; úplné konzolové aplikace nejsou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="3eaba-185">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="3eaba-186">Použije [certifikátu x 509](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) třída, která poskytuje několika různými způsoby, abyste mohli vytvořit certifikát.</span><span class="sxs-lookup"><span data-stu-id="3eaba-186">It uses the [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
