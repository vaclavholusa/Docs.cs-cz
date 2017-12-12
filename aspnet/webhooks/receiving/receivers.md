---
uid: webhooks/receiving/receivers
title: "Příjemci Webhooky ASP.NET | Microsoft Docs"
author: rick-anderson
description: "Příjemci Webhooky ASP.NET"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 8c42db4056dd7a6ef77c7bcbc0eca3b5bf7c87e9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-receivers"></a><span data-ttu-id="94180-103">Příjemci Webhooky ASP.NET</span><span class="sxs-lookup"><span data-stu-id="94180-103">ASP.NET WebHooks receivers</span></span>

<span data-ttu-id="94180-104">Přijetí Webhooky závisí na to, kdo je odesílatele.</span><span class="sxs-lookup"><span data-stu-id="94180-104">Receiving WebHooks depends on who the sender is.</span></span> <span data-ttu-id="94180-105">Někdy existují další kroky registrace WebHook, jehož ověření skutečně naslouchá odběratele.</span><span class="sxs-lookup"><span data-stu-id="94180-105">Sometimes there are additional steps registering a WebHook verifying that the subscriber is really listening.</span></span> <span data-ttu-id="94180-106">Některé Webhooky poskytnout model push pull kde požadavku HTTP POST pouze obsahuje odkaz na informace o události, které je pak možné načíst nezávisle.</span><span class="sxs-lookup"><span data-stu-id="94180-106">Some WebHooks provide a push-to-pull model where the HTTP POST request only contains a reference to the event information which is then to be retrieved independently.</span></span> <span data-ttu-id="94180-107">Často modelu zabezpečení se liší odlišují.</span><span class="sxs-lookup"><span data-stu-id="94180-107">Often the security model varies quite a bit.</span></span>

<span data-ttu-id="94180-108">Účelem WebHooks Microsoft ASP.NET je jednodušší a konzistentní propojit se rozhraní API bez výdaje mnoho času nad tím, jak zpracovat žádné konkrétní variant Webhooky.</span><span class="sxs-lookup"><span data-stu-id="94180-108">The purpose of Microsoft ASP.NET WebHooks is to make it both simpler and more consistent to wire up your API without spending a lot of time figuring out how to handle any particular variant of WebHooks.</span></span>

<span data-ttu-id="94180-109">Příjemce Webhooku je zodpovědná za přijímání a ověření Webhooky od určitého odesílatele.</span><span class="sxs-lookup"><span data-stu-id="94180-109">A WebHook receiver is responsible for accepting and verifying WebHooks from a particular sender.</span></span> <span data-ttu-id="94180-110">Webhooku příjemce může podporovat libovolný počet Webhooků, každou s vlastní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="94180-110">A WebHook receiver can support any number of WebHooks, each with their own configuration.</span></span> <span data-ttu-id="94180-111">WebHook Githubu příjemce může například přijmout Webhooky z různých úložišť GitHub.</span><span class="sxs-lookup"><span data-stu-id="94180-111">For example, the GitHub WebHook receiver can accept WebHooks from any number of GitHub repositories.</span></span>

## <a name="webhook-receiver-uris"></a><span data-ttu-id="94180-112">Identifikátory URI Webhooku příjemce</span><span class="sxs-lookup"><span data-stu-id="94180-112">WebHook Receiver URIs</span></span>

<span data-ttu-id="94180-113">Nainstalováním Microsoft ASP.NET WebHooks získáte obecné Webhooku řadiči, který přijímá požadavky Webhooku od zprostředkovává počet služeb.</span><span class="sxs-lookup"><span data-stu-id="94180-113">By installing Microsoft ASP.NET WebHooks you get a general WebHook controller which accepts WebHook requests from an open-ended number of services.</span></span> <span data-ttu-id="94180-114">Pokud dorazí požadavek, vybere odpovídající příjemce, který jste nainstalovali pro zpracování určitého Webhooku odesílatele.</span><span class="sxs-lookup"><span data-stu-id="94180-114">When a request arrives, it picks the appropriate receiver that you have installed for handling a particular WebHook sender.</span></span>

<span data-ttu-id="94180-115">Identifikátor URI tohoto řadiče je identifikátor URI Webhooku registrace pomocí služby a je ve formátu:</span><span class="sxs-lookup"><span data-stu-id="94180-115">The URI of this controller is the WebHook URI that you register with the service and is of the form:</span></span>

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

<span data-ttu-id="94180-116">Z bezpečnostních důvodů mnoho Webhooku příjemci vyžadují, aby identifikátor URI *https* URI a v některých případech musí také obsahovat parametrem další dotaz, který slouží k vynucení, jenom zamýšlený strany může odesílat Webhooky na výše uvedené identifikátor URI .</span><span class="sxs-lookup"><span data-stu-id="94180-116">For security reasons, many WebHook receivers require that the URI is an *https* URI and in some cases it must also contain an additional query parameter which is used to enforce that only the intended party can send WebHooks to the URI above.</span></span>

<span data-ttu-id="94180-117"> *<receiver>*  Součást je název příjemce, například *githubu* nebo *slack*.</span><span class="sxs-lookup"><span data-stu-id="94180-117">The *<receiver>* component is the name of the receiver, for example *github* or *slack*.</span></span>

<span data-ttu-id="94180-118">*{Id}* je volitelné identifikátor, který slouží k identifikaci konkrétní konfigurací Webhooku příjemce.</span><span class="sxs-lookup"><span data-stu-id="94180-118">The *{id}* is an optional identifier which can be used to identify a particular WebHook receiver configuration.</span></span> <span data-ttu-id="94180-119">To slouží k registraci N Webhooky s konkrétní příjemce.</span><span class="sxs-lookup"><span data-stu-id="94180-119">This can be used to register N WebHooks with a particular receiver.</span></span> <span data-ttu-id="94180-120">Například následující tři identifikátory URI slouží k registraci pro tři nezávislá Webhooky:</span><span class="sxs-lookup"><span data-stu-id="94180-120">For example, the following three URIs can be used to register for three independent WebHooks:</span></span>

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a><span data-ttu-id="94180-121">Instalace Webhooku příjemce</span><span class="sxs-lookup"><span data-stu-id="94180-121">Installing a WebHook Receiver</span></span>

<span data-ttu-id="94180-122">Pro příjem Webhooky pomocí WebHooks Microsoft ASP.NET, je nejprve nainstalovat balíček Nuget pro zprostředkovatele Webhooku nebo poskytovatelů, které chcete dostávat Webhooky z.</span><span class="sxs-lookup"><span data-stu-id="94180-122">To receive WebHooks using Microsoft ASP.NET WebHooks, you first install the Nuget package for the WebHook provider or providers you want to receive WebHooks from.</span></span> <span data-ttu-id="94180-123">Balíčky Nuget jsou pojmenované [Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) Určuje, kde poslední část službu podporována.</span><span class="sxs-lookup"><span data-stu-id="94180-123">The Nuget packages are named [Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) where the last part indicates the service supported.</span></span> <span data-ttu-id="94180-124">Příklad</span><span class="sxs-lookup"><span data-stu-id="94180-124">For example</span></span>

<span data-ttu-id="94180-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) poskytuje podporu pro příjem Webhooky z Githubu a [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) poskytuje podporu pro příjem Webhooky generované ASP. NET Webhooky.</span><span class="sxs-lookup"><span data-stu-id="94180-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) provides support for receiving WebHooks from GitHub and [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) provides support for receiving WebHooks generated by ASP.NET WebHooks.</span></span>

<span data-ttu-id="94180-126">Ihned můžete najít podpora Dropbox, Githubu, MailChimp, PayPal, tlačná, Salesforce, Slack, Stripe, Trello a WordPress, ale je možné podporovat libovolný počet dalších zprostředkovatelů.</span><span class="sxs-lookup"><span data-stu-id="94180-126">Out of the box you can find support for Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello, and WordPress but it is possible to support any number of other providers.</span></span>

## <a name="configuring-a-webhook-receiver"></a><span data-ttu-id="94180-127">Konfigurace Webhooku příjemce</span><span class="sxs-lookup"><span data-stu-id="94180-127">Configuring a WebHook Receiver</span></span>

<span data-ttu-id="94180-128">Příjemci Webhooku jsou nakonfigurované pomocí [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) rozhraní a konkrétní implementace tohoto rozhraní lze registrovat pomocí kteréhokoli modelu vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="94180-128">WebHook Receivers are configured through the [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) inteface and particular implementations of that interface can be registered using any dependency injection model.</span></span> <span data-ttu-id="94180-129">Výchozí implementace používá nastavení aplikace, která může být buď nastavena v souboru Web.config, nebo pokud používáte Azure Web Apps, je možné nastavit prostřednictvím [portálu Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="94180-129">The default implementation uses Application Settings which can either be set in the Web.config file, or, if using Azure Web Apps, can be set through the [Azure Portal](https://portal.azure.com/).</span></span>

![Nastavení aplikace Azure](_static/AzureAppSettings.png)

<span data-ttu-id="94180-131">Formát pro nastavení aplikace klíče je následující:</span><span class="sxs-lookup"><span data-stu-id="94180-131">The format for Application Setting keys is as follows:</span></span>

```
MS_WebHookReceiverSecret_<receiver>
```

<span data-ttu-id="94180-132">Hodnota je čárkami oddělený seznam hodnot odpovídající *{id}* hodnoty, pro které Webhooky byly zaregistrovány, například:</span><span class="sxs-lookup"><span data-stu-id="94180-132">The value is a comma-separated list of values matching the *{id}* values for which WebHooks have been registered, for example:</span></span>

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a><span data-ttu-id="94180-133">Inicializace Webhooku příjemce</span><span class="sxs-lookup"><span data-stu-id="94180-133">Initializing a WebHook Receiver</span></span>

<span data-ttu-id="94180-134">Příjemci Webhooku se inicializují registrací je obvykle ve *WebApiConfig* statické třídy, například:</span><span class="sxs-lookup"><span data-stu-id="94180-134">WebHook Receivers are initialized by registering them, typically in the *WebApiConfig* static class, for example:</span></span>

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
