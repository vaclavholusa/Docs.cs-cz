---
uid: webhooks/receiving/receivers
title: ASP.NET – Webhooky příjemci | Dokumentace Microsoftu
author: rick-anderson
description: ASP.NET – Webhooky příjemců
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.openlocfilehash: 376cb3e3fdc0bc7bd248da1f57e1064fb27b3cef
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754777"
---
# <a name="aspnet-webhooks-receivers"></a><span data-ttu-id="9e5fd-103">ASP.NET – Webhooky příjemců</span><span class="sxs-lookup"><span data-stu-id="9e5fd-103">ASP.NET WebHooks receivers</span></span>

<span data-ttu-id="9e5fd-104">Přijímá Webhooky závisí na to, kdo je odesílatele.</span><span class="sxs-lookup"><span data-stu-id="9e5fd-104">Receiving WebHooks depends on who the sender is.</span></span> <span data-ttu-id="9e5fd-105">Někdy je potřeba další kroky registrace Webhooku ověření, že je ve skutečnosti naslouchání odběratele.</span><span class="sxs-lookup"><span data-stu-id="9e5fd-105">Sometimes there are additional steps registering a WebHook verifying that the subscriber is really listening.</span></span> <span data-ttu-id="9e5fd-106">Některé Webhooky poskytují model push pull kde požadavku HTTP POST obsahuje pouze odkaz na informace o události, které je pak načíst nezávisle.</span><span class="sxs-lookup"><span data-stu-id="9e5fd-106">Some WebHooks provide a push-to-pull model where the HTTP POST request only contains a reference to the event information which is then to be retrieved independently.</span></span> <span data-ttu-id="9e5fd-107">Model zabezpečení často mění poměrně trochu.</span><span class="sxs-lookup"><span data-stu-id="9e5fd-107">Often the security model varies quite a bit.</span></span>

<span data-ttu-id="9e5fd-108">Účelem WebHooks Microsoft ASP.NET je jednodušší a konzistentnější propojí vaše rozhraní API bez mnoho času vymýšlení na zpracování jakékoli varianta Webhooky.</span><span class="sxs-lookup"><span data-stu-id="9e5fd-108">The purpose of Microsoft ASP.NET WebHooks is to make it both simpler and more consistent to wire up your API without spending a lot of time figuring out how to handle any particular variant of WebHooks.</span></span>

<span data-ttu-id="9e5fd-109">Příjemce Webhooku je zodpovědná za přijímání a ověření Webhooky od konkrétního odesílatele.</span><span class="sxs-lookup"><span data-stu-id="9e5fd-109">A WebHook receiver is responsible for accepting and verifying WebHooks from a particular sender.</span></span> <span data-ttu-id="9e5fd-110">WebHook příjemce může podporovat libovolný počet Webhooků, každý s vlastní konfigurací.</span><span class="sxs-lookup"><span data-stu-id="9e5fd-110">A WebHook receiver can support any number of WebHooks, each with their own configuration.</span></span> <span data-ttu-id="9e5fd-111">WebHook Githubu příjemce může například přijmout Webhooků z různých úložišť GitHub.</span><span class="sxs-lookup"><span data-stu-id="9e5fd-111">For example, the GitHub WebHook receiver can accept WebHooks from any number of GitHub repositories.</span></span>

## <a name="webhook-receiver-uris"></a><span data-ttu-id="9e5fd-112">Identifikátory URI příjemce Webhooku</span><span class="sxs-lookup"><span data-stu-id="9e5fd-112">WebHook Receiver URIs</span></span>

<span data-ttu-id="9e5fd-113">Instalace Microsoft ASP.NET WebHooks získáte řadiči obecný WebHook, který přijímá požadavky Webhooku od konkrétní počet služeb.</span><span class="sxs-lookup"><span data-stu-id="9e5fd-113">By installing Microsoft ASP.NET WebHooks you get a general WebHook controller which accepts WebHook requests from an open-ended number of services.</span></span> <span data-ttu-id="9e5fd-114">Když přijde požadavek, vybere vhodné příjemce, kterou jste nainstalovali pro zpracování konkrétního odesílatele Webhooku.</span><span class="sxs-lookup"><span data-stu-id="9e5fd-114">When a request arrives, it picks the appropriate receiver that you have installed for handling a particular WebHook sender.</span></span>

<span data-ttu-id="9e5fd-115">Identifikátor URI tohoto kontroleru je identifikátor URI Webhooku, která můžete zaregistrovat do služby a má formát:</span><span class="sxs-lookup"><span data-stu-id="9e5fd-115">The URI of this controller is the WebHook URI that you register with the service and is of the form:</span></span>

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

<span data-ttu-id="9e5fd-116">Z bezpečnostních důvodů vyžadují mnoho příjemci Webhooku, že je identifikátor URI *https* identifikátor URI a v některých případech musí obsahovat také další dotaz parametr, který se používá k vynucení, který pouze zamýšlené strany můžete poslat Webhooky výše identifikátoru URI .</span><span class="sxs-lookup"><span data-stu-id="9e5fd-116">For security reasons, many WebHook receivers require that the URI is an *https* URI and in some cases it must also contain an additional query parameter which is used to enforce that only the intended party can send WebHooks to the URI above.</span></span>

<span data-ttu-id="9e5fd-117"><em> <receiver> </em> Komponenta je jméno příjemce, například <em>githubu</em> nebo <em>slack</em>.</span><span class="sxs-lookup"><span data-stu-id="9e5fd-117">The <em><receiver></em> component is the name of the receiver, for example <em>github</em> or <em>slack</em>.</span></span>

<span data-ttu-id="9e5fd-118">*{Id}* je volitelný identifikátor, který slouží k identifikaci konkrétní konfigurace příjemce Webhooku.</span><span class="sxs-lookup"><span data-stu-id="9e5fd-118">The *{id}* is an optional identifier which can be used to identify a particular WebHook receiver configuration.</span></span> <span data-ttu-id="9e5fd-119">To je možné zaregistrovat N Webhooky u konkrétního příjemce.</span><span class="sxs-lookup"><span data-stu-id="9e5fd-119">This can be used to register N WebHooks with a particular receiver.</span></span> <span data-ttu-id="9e5fd-120">Například následující tři identifikátory URI slouží k registraci pro tři nezávislá Webhooků:</span><span class="sxs-lookup"><span data-stu-id="9e5fd-120">For example, the following three URIs can be used to register for three independent WebHooks:</span></span>

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a><span data-ttu-id="9e5fd-121">Instalace příjemce Webhooku</span><span class="sxs-lookup"><span data-stu-id="9e5fd-121">Installing a WebHook Receiver</span></span>

<span data-ttu-id="9e5fd-122">Pro příjem Webhooků pomocí Microsoft ASP.NET WebHooks, je nejprve nainstalovat balíček Nuget pro poskytovatele WebHook nebo chcete dostávat Webhooků z poskytovatelů.</span><span class="sxs-lookup"><span data-stu-id="9e5fd-122">To receive WebHooks using Microsoft ASP.NET WebHooks, you first install the Nuget package for the WebHook provider or providers you want to receive WebHooks from.</span></span> <span data-ttu-id="9e5fd-123">Balíčky Nuget jsou pojmenovány [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) označuje, kde poslední část služby nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="9e5fd-123">The Nuget packages are named [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) where the last part indicates the service supported.</span></span> <span data-ttu-id="9e5fd-124">Příklad</span><span class="sxs-lookup"><span data-stu-id="9e5fd-124">For example</span></span>

<span data-ttu-id="9e5fd-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) poskytuje podporu pro příjem Webhooků z Githubu a [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) poskytuje podporu pro příjem Webhooky generovaných ASP. NET Webhooky.</span><span class="sxs-lookup"><span data-stu-id="9e5fd-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) provides support for receiving WebHooks from GitHub and [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) provides support for receiving WebHooks generated by ASP.NET WebHooks.</span></span>

<span data-ttu-id="9e5fd-126">Okamžité najdete podporu pro Dropbox, GitHub, MailChimp, PayPal, Pusheru, Salesforce, Slack, Stripe, Trello a WordPress, ale je možné pro podporu libovolný počet dalších poskytovatelů.</span><span class="sxs-lookup"><span data-stu-id="9e5fd-126">Out of the box you can find support for Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello, and WordPress but it is possible to support any number of other providers.</span></span>

## <a name="configuring-a-webhook-receiver"></a><span data-ttu-id="9e5fd-127">Konfigurace Webhooku příjemce</span><span class="sxs-lookup"><span data-stu-id="9e5fd-127">Configuring a WebHook Receiver</span></span>

<span data-ttu-id="9e5fd-128">Příjemci Webhooku se konfigurují [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) konkrétní implementace rozhraní a rozhraní lze registrovat pomocí jakéhokoli modelu vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="9e5fd-128">WebHook Receivers are configured through the [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) inteface and particular implementations of that interface can be registered using any dependency injection model.</span></span> <span data-ttu-id="9e5fd-129">Výchozí implementace používá nastavení aplikace, které je možné nastavit buď v souboru Web.config, nebo pokud používáte Azure Web Apps, můžete nastavit prostřednictvím [webu Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="9e5fd-129">The default implementation uses Application Settings which can either be set in the Web.config file, or, if using Azure Web Apps, can be set through the [Azure Portal](https://portal.azure.com/).</span></span>

![Nastavení aplikace Azure](_static/AzureAppSettings.png)

<span data-ttu-id="9e5fd-131">Formát pro nastavení aplikace klíče je následující:</span><span class="sxs-lookup"><span data-stu-id="9e5fd-131">The format for Application Setting keys is as follows:</span></span>

```
MS_WebHookReceiverSecret_<receiver>
```

<span data-ttu-id="9e5fd-132">Hodnota je čárkou oddělený seznam hodnot, které odpovídají *{id}* hodnoty, u kterých Webhooky jsou zaregistrovány, například:</span><span class="sxs-lookup"><span data-stu-id="9e5fd-132">The value is a comma-separated list of values matching the *{id}* values for which WebHooks have been registered, for example:</span></span>

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a><span data-ttu-id="9e5fd-133">Inicializace příjemce Webhooku</span><span class="sxs-lookup"><span data-stu-id="9e5fd-133">Initializing a WebHook Receiver</span></span>

<span data-ttu-id="9e5fd-134">WebHook příjemci jsou inicializovány tak, že zaregistrujete, obvykle v *WebApiConfig* statické třídy, například:</span><span class="sxs-lookup"><span data-stu-id="9e5fd-134">WebHook Receivers are initialized by registering them, typically in the *WebApiConfig* static class, for example:</span></span>

```csharp
namespace WebHookReceivers
{
    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            // Load receivers
            config.InitializeReceiveGitHubWebHooks();
        }
    }
}
```
