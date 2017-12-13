---
uid: webhooks/index
title: "ASP.NET: Přehled Webhooky | Microsoft Docs"
author: rick-anderson
description: "Úvod k Webhookům ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 52399c23cdf393a2f7f94661fd48098ced65948c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-overview"></a><span data-ttu-id="d12a9-103">Přehled Webhooky ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d12a9-103">ASP.NET WebHooks overview</span></span>

<span data-ttu-id="d12a9-104">Webhooky je lightweight vzor HTTP poskytuje jednoduché pub nebo dílčí model pro připojení služby webovým rozhraním API a SaaS společně.</span><span class="sxs-lookup"><span data-stu-id="d12a9-104">WebHooks is a lightweight HTTP pattern providing a simple pub/sub model for wiring together Web APIs and SaaS services.</span></span> <span data-ttu-id="d12a9-105">V případě události ve službě je odesláno upozornění ve formě požadavek HTTP POST registrované odběratelům.</span><span class="sxs-lookup"><span data-stu-id="d12a9-105">When an event happens in a service, a notification is sent in the form of an HTTP POST request to registered subscribers.</span></span> <span data-ttu-id="d12a9-106">Požadavek POST obsahuje informace o události, která vám umožní pro příjemce tak, aby fungoval odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="d12a9-106">The POST request contains information about the event which makes it possible for the receiver to act accordingly.</span></span>

<span data-ttu-id="d12a9-107">Kvůli jejich jednoduchost Webhooky jsou již vystavené velký počet služeb včetně [Dropbox](http://dropbox.com/), [Githubu](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/)a mnoho dalšího.</span><span class="sxs-lookup"><span data-stu-id="d12a9-107">Because of their simplicity, WebHooks are already exposed by a large number of services including [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), and many more.</span></span> <span data-ttu-id="d12a9-108">Například WebHook, jehož může naznačovat, že soubor se změnila v [Dropbox](http://dropbox.com/), nebo změnit kód byl potvrzen v Githubu, nebo byla inicializována platba v [PayPal](http://www.paypal.com/), nebo na kartě byl vytvořen v [ Trello](http://www.trello.com/).</span><span class="sxs-lookup"><span data-stu-id="d12a9-108">For example, a WebHook can indicate that a file has changed in [Dropbox](http://dropbox.com/), or a code change has been committed in GitHub, or a payment has been initiated in [PayPal](http://www.paypal.com/), or a card has been created in [Trello](http://www.trello.com/).</span></span> <span data-ttu-id="d12a9-109">Existuje mnoho možností!</span><span class="sxs-lookup"><span data-stu-id="d12a9-109">The possibilities are endless!</span></span>

<span data-ttu-id="d12a9-110">Microsoft ASP.NET WebHooks usnadňuje odesílat i přijímat Webhooky jako součást aplikace ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="d12a9-110">Microsoft ASP.NET WebHooks makes it easier to both send and receive WebHooks as part of your ASP.NET application:</span></span>

* <span data-ttu-id="d12a9-111">Na straně příjmu poskytuje společný model pro přijímání a zpracování Webhooky z libovolný počet Webhooku poskytovatelů.</span><span class="sxs-lookup"><span data-stu-id="d12a9-111">On the receiving side, it provides a common model for receiving and processing WebHooks from any number of WebHook providers.</span></span> <span data-ttu-id="d12a9-112">Vrátí z pole s podporou [Dropbox](http://dropbox.com/), [Githubu](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Tlačná](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) a [Zendesk](https://www.zendesk.com/) , ale je snadné přidání podpory pro další.</span><span class="sxs-lookup"><span data-stu-id="d12a9-112">It comes out of the box with support for [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) and [Zendesk](https://www.zendesk.com/) but it is easy to add support for more.</span></span>

* <span data-ttu-id="d12a9-113">Na straně odesílání poskytuje podporu pro správu a ukládání odběry stejně jako u odesílání oznámení událostí na správnou sadu odběratelů.</span><span class="sxs-lookup"><span data-stu-id="d12a9-113">On the sending side it provides support for managing and storing subscriptions as well as for sending event notifications to the right set of subscribers.</span></span> <span data-ttu-id="d12a9-114">To umožňuje definovat vlastní sadu událostí, můžete se přihlásit k odběru a je upozornit, když se stane věcí odběratele.</span><span class="sxs-lookup"><span data-stu-id="d12a9-114">This allows you to define your own set of events that subscribers can subscribe to and notify them when things happens.</span></span>

<span data-ttu-id="d12a9-115">Dvě části můžete použít společně nebo od sebe v závislosti na vašem scénáři.</span><span class="sxs-lookup"><span data-stu-id="d12a9-115">The two parts can be used together or apart depending on your scenario.</span></span> <span data-ttu-id="d12a9-116">Pokud potřebujete přijímat Webhooky z jiných služeb, můžete použít jenom část příjemce; Pokud chcete vystavit Webhooky pro ostatní využívat, pak můžete to udělat jen.</span><span class="sxs-lookup"><span data-stu-id="d12a9-116">If you only need to receive WebHooks from other services then you can use just the receiver part; if you only want to expose WebHooks for others to consume, then you can do just that.</span></span>

<span data-ttu-id="d12a9-117">Kód cílí webovém rozhraní API 2 ASP.NET a ASP.NET MVC 5 a je k dispozici jako [operačních systémů na Githubu](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="d12a9-117">The code targets ASP.NET Web API 2 and ASP.NET MVC 5 and is available as [OSS on GitHub](https://github.com/aspnet/WebHooks).</span></span>

## <a name="webhooks-overview"></a><span data-ttu-id="d12a9-118">Přehled Webhooky</span><span class="sxs-lookup"><span data-stu-id="d12a9-118">WebHooks Overview</span></span>

<span data-ttu-id="d12a9-119">Webhooky je vzor, což znamená, že se liší, jak se používají ze služby pro služby, ale základní Rada je stejný.</span><span class="sxs-lookup"><span data-stu-id="d12a9-119">WebHooks is a pattern which means that it varies how it is used from service to service but the basic idea is the same.</span></span> <span data-ttu-id="d12a9-120">Si můžete představit Webhooky jako model jednoduché pub nebo sub kde uživatel může přihlásit k odběru událostí děje jinde.</span><span class="sxs-lookup"><span data-stu-id="d12a9-120">You can think of WebHooks as a simple pub/sub model where a user can subscribe to events happening elsewhere.</span></span> <span data-ttu-id="d12a9-121">Oznamování událostí rozšířeny jako požadavky HTTP POST obsahující informace o samotné události.</span><span class="sxs-lookup"><span data-stu-id="d12a9-121">The event notifications are propagated as HTTP POST requests containing information about the event itself.</span></span>

<span data-ttu-id="d12a9-122">Požadavek HTTP POST obvykle obsahuje objekt JSON nebo data formuláře HTML určit Webhooku odesílatel, včetně informací o události, která způsobila Webhooku pro aktivaci.</span><span class="sxs-lookup"><span data-stu-id="d12a9-122">Typically the HTTP POST request contains a JSON object or HTML form data determined by the WebHook sender including information about the event causing the WebHook to trigger.</span></span> <span data-ttu-id="d12a9-123">Například příklad těla požadavku POST Webhooku z [Githubu](http://www.github.com/) vypadá jako to v důsledku nový problém otevíráte v konkrétní úložiště:</span><span class="sxs-lookup"><span data-stu-id="d12a9-123">For example, an example of a WebHook POST request body from [GitHub](http://www.github.com/) looks like this as a result of a new issue being opened in a particular repository:</span></span>

```json
{
  "action": "opened",
  "issue": {
      "url": "https://api.github.com/repos/octocat/Hello-World/issues/1347",
      "number": 1347,
      ...
  },
  "repository": {
      "id": 1296269,
      "full_name": "octocat/Hello-World",
      "owner": {
          "login": "octocat",
          "id": 1
          ...
      },
      ...
  },
  "sender": {
      "login": "octocat",
      "id": 1,
      ...
  }
}
```

<span data-ttu-id="d12a9-124">K zajištění, že Webhooku je skutečně od určený odesílatele, je požadavek POST zabezpečené nějakým způsobem a potom ověřit pomocí příjemce.</span><span class="sxs-lookup"><span data-stu-id="d12a9-124">To ensure that the WebHook is indeed from the intended sender, the POST request is secured in some way and then verified by the receiver.</span></span> <span data-ttu-id="d12a9-125">Například [Githubu Webhooky](https://developer.github.com/webhooks/) zahrnuje *X-Hub-podpis* hlavičky protokolu HTTP se hodnota hash obsahu žádosti, které je zaškrtnuto implementací příjemce, takže nemusíte si dělat starosti o něm.</span><span class="sxs-lookup"><span data-stu-id="d12a9-125">For example, [GitHub WebHooks](https://developer.github.com/webhooks/) includes an *X-Hub-Signature* HTTP header with a hash of the request body which is checked by the receiver implementation so you don't have to worry about it.</span></span>

<span data-ttu-id="d12a9-126">Tok Webhooku obecně přejde přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="d12a9-126">The WebHook flow generally goes something like this:</span></span>

* <span data-ttu-id="d12a9-127">Odesílatel Webhooku zpřístupní události, které klient může přihlásit k odběru.</span><span class="sxs-lookup"><span data-stu-id="d12a9-127">The WebHook sender exposes events that a client can subscribe to.</span></span> <span data-ttu-id="d12a9-128">Události, které popisují pozorovatelné změny systému, například které nová datová položka byla vložené, zda byla dokončena proces, nebo něco jiného.</span><span class="sxs-lookup"><span data-stu-id="d12a9-128">The events describe observable changes to the system, for example that a new data item has been inserted, that a process has completed, or something else.</span></span>

* <span data-ttu-id="d12a9-129">Příjemce Webhooku jako odběratel tak, že zaregistrujete Webhooku, který se skládá ze čtyř akcí:</span><span class="sxs-lookup"><span data-stu-id="d12a9-129">The WebHook receiver subscribes by registering a WebHook consisting of four things:</span></span>

     1. <span data-ttu-id="d12a9-130">Identifikátor URI pro kde by měla odeslat oznámení události ve formě požadavek HTTP POST;</span><span class="sxs-lookup"><span data-stu-id="d12a9-130">A URI for where the event notification should be posted in the form of an HTTP POST request;</span></span>

     2. <span data-ttu-id="d12a9-131">Sadu filtrů popisující konkrétní události, pro které má být aktivována Webhooku;</span><span class="sxs-lookup"><span data-stu-id="d12a9-131">A set of filters describing the particular events for which the WebHook should be fired;</span></span>

     3. <span data-ttu-id="d12a9-132">Tajný klíč, který se používá k podepisování požadavku HTTP POST;</span><span class="sxs-lookup"><span data-stu-id="d12a9-132">A secret key which is used to sign the HTTP POST request;</span></span>

     4. <span data-ttu-id="d12a9-133">Další data, která má být zahrnut v požadavku HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="d12a9-133">Additional data which is to be included in the HTTP POST request.</span></span> <span data-ttu-id="d12a9-134">Například to může být další pole hlavičky protokolu HTTP nebo vlastnosti, které jsou zahrnuty v textu požadavku HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="d12a9-134">This can for example be additional HTTP header fields or properties included in the HTTP POST request body.</span></span>

* <span data-ttu-id="d12a9-135">Poté, co se stane událost, nebyly nalezeny odpovídající Webhooku registrace a jsou odeslány požadavky HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="d12a9-135">Once an event happens, the matching WebHook registrations are found and HTTP POST requests are submitted.</span></span> <span data-ttu-id="d12a9-136">Obvykle generování požadavky HTTP POST jsou opakovány několikrát pokud z nějakého důvodu, že příjemce neodpovídá nebo výsledky požadavku HTTP POST ve chybnou odpověď.</span><span class="sxs-lookup"><span data-stu-id="d12a9-136">Typically, the generation of the HTTP POST requests are retried several times if for some reason the recipient is not responding or the HTTP POST request results in an error response.</span></span>

## <a name="webhooks-processing-pipeline"></a><span data-ttu-id="d12a9-137">Webhooky zpracování kanálu</span><span class="sxs-lookup"><span data-stu-id="d12a9-137">WebHooks Processing Pipeline</span></span>

<span data-ttu-id="d12a9-138">Microsoft ASP.NET WebHooks zpracování kanálu pro příchozí Webhooky vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="d12a9-138">The Microsoft ASP.NET WebHooks processing pipeline for incoming WebHooks looks like this:</span></span>

![ASP.NET Webhooky zpracování kanálu](_static/WebHookReceivers.png)

<span data-ttu-id="d12a9-140">Jsou zde dva klíčové koncepty *příjemci* a *obslužné rutiny*:</span><span class="sxs-lookup"><span data-stu-id="d12a9-140">The two key concepts here are *Receivers* and *Handlers*:</span></span>

* <span data-ttu-id="d12a9-141">*Příjemci* zodpovídají pro zpracování konkrétní příchuť Webhooku od daného odesílatele a vynucování zabezpečení kontroly, aby se zajistilo, že skutečně jedná o žádost Webhooku z určený odesílatele.</span><span class="sxs-lookup"><span data-stu-id="d12a9-141">*Receivers* are responsible for handling the particular flavor of WebHook from a given sender and for enforcing security checks to ensure that the WebHook request indeed is from the intended sender.</span></span>

* <span data-ttu-id="d12a9-142">*Obslužné rutiny* jsou obvykle, kde se spouští zpracování konkrétní Webhooku uživatelského kódu.</span><span class="sxs-lookup"><span data-stu-id="d12a9-142">*Handlers* are typically where user code runs processing the particular WebHook.</span></span>

<span data-ttu-id="d12a9-143">V následující uzly jsou tyto koncepty popsané v další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="d12a9-143">In the following nodes these concepts are described in more details.</span></span>
