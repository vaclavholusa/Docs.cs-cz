---
uid: webhooks/index
title: 'Webhooky ASP.NET: Přehled | Dokumentace Microsoftu'
author: rick-anderson
description: Úvod do ASP.NET – Webhooky.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.openlocfilehash: 702cc0bf0d0bb887c64bec19e1faf249bd96617a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752434"
---
# <a name="aspnet-webhooks-overview"></a><span data-ttu-id="8f2e4-103">Přehled ASP.NET – Webhooky</span><span class="sxs-lookup"><span data-stu-id="8f2e4-103">ASP.NET WebHooks overview</span></span>

<span data-ttu-id="8f2e4-104">Webhooky se odlehčeného vzoru HTTP poskytuje jednoduché pub/sub model pro vzájemné propojení dohromady webová rozhraní API a služby SaaS.</span><span class="sxs-lookup"><span data-stu-id="8f2e4-104">WebHooks is a lightweight HTTP pattern providing a simple pub/sub model for wiring together Web APIs and SaaS services.</span></span> <span data-ttu-id="8f2e4-105">Případě určité události ve službě, oznámení se posílá ve formuláři požadavku HTTP POST pro registrované předplatitele.</span><span class="sxs-lookup"><span data-stu-id="8f2e4-105">When an event happens in a service, a notification is sent in the form of an HTTP POST request to registered subscribers.</span></span> <span data-ttu-id="8f2e4-106">Požadavek POST obsahuje informace o události, která umožňuje příjemci příslušně na ně reagovat.</span><span class="sxs-lookup"><span data-stu-id="8f2e4-106">The POST request contains information about the event which makes it possible for the receiver to act accordingly.</span></span>

<span data-ttu-id="8f2e4-107">Z důvodu jejich jednoduchost Webhooky jsou již vystavené velkým množstvím služeb včetně [Dropboxu](http://dropbox.com/), [Githubu](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/)a mnoho dalších.</span><span class="sxs-lookup"><span data-stu-id="8f2e4-107">Because of their simplicity, WebHooks are already exposed by a large number of services including [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), and many more.</span></span> <span data-ttu-id="8f2e4-108">Například Webhooku může znamenat, že soubor byl změněn v [Dropboxu](http://dropbox.com/), změny kódu byl potvrzen v Githubu nebo platby byl zahájen v [PayPal](http://www.paypal.com/), nebo na kartě se vytvořil v [ Trello](http://www.trello.com/).</span><span class="sxs-lookup"><span data-stu-id="8f2e4-108">For example, a WebHook can indicate that a file has changed in [Dropbox](http://dropbox.com/), or a code change has been committed in GitHub, or a payment has been initiated in [PayPal](http://www.paypal.com/), or a card has been created in [Trello](http://www.trello.com/).</span></span> <span data-ttu-id="8f2e4-109">Možností je nekonečně!</span><span class="sxs-lookup"><span data-stu-id="8f2e4-109">The possibilities are endless!</span></span>

<span data-ttu-id="8f2e4-110">Microsoft ASP.NET WebHooks usnadňuje odesílat i přijímat Webhooků jako součást aplikace ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="8f2e4-110">Microsoft ASP.NET WebHooks makes it easier to both send and receive WebHooks as part of your ASP.NET application:</span></span>

* <span data-ttu-id="8f2e4-111">Na straně příjmu poskytuje společný model pro příjem a zpracování Webhooků z různých zprostředkovatelů Webhooku.</span><span class="sxs-lookup"><span data-stu-id="8f2e4-111">On the receiving side, it provides a common model for receiving and processing WebHooks from any number of WebHook providers.</span></span> <span data-ttu-id="8f2e4-112">Jde o předpřipravených s podporou [Dropboxu](http://dropbox.com/), [Githubu](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusheru](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) a [Zendesku](https://www.zendesk.com/) je ale také snadné přidat podporu pro víc.</span><span class="sxs-lookup"><span data-stu-id="8f2e4-112">It comes out of the box with support for [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) and [Zendesk](https://www.zendesk.com/) but it is easy to add support for more.</span></span>

* <span data-ttu-id="8f2e4-113">Na straně odesílání poskytuje podporu pro správu a ukládání předplatná stejně jako u odesílání oznámení událostí na správnou sadu předplatitele.</span><span class="sxs-lookup"><span data-stu-id="8f2e4-113">On the sending side it provides support for managing and storing subscriptions as well as for sending event notifications to the right set of subscribers.</span></span> <span data-ttu-id="8f2e4-114">To umožňuje definovat vlastní sadu událostí, můžete se přihlásit k odběru a upozorňovat na co se stane, předplatitele.</span><span class="sxs-lookup"><span data-stu-id="8f2e4-114">This allows you to define your own set of events that subscribers can subscribe to and notify them when things happens.</span></span>

<span data-ttu-id="8f2e4-115">Dvě části je možné společně nebo od sebe v závislosti na vašem scénáři.</span><span class="sxs-lookup"><span data-stu-id="8f2e4-115">The two parts can be used together or apart depending on your scenario.</span></span> <span data-ttu-id="8f2e4-116">Pokud potřebujete přijímá Webhooky z jiných služeb, můžete použít jenom část příjemce; Pokud chcete vystavit Webhooky pro ostatní uživatele používat, můžete přesně to provést.</span><span class="sxs-lookup"><span data-stu-id="8f2e4-116">If you only need to receive WebHooks from other services then you can use just the receiver part; if you only want to expose WebHooks for others to consume, then you can do just that.</span></span>

<span data-ttu-id="8f2e4-117">Kód, zaměřuje na technologie ASP.NET Web API 2 a ASP.NET MVC 5 a je k dispozici jako [OSS na Githubu](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="8f2e4-117">The code targets ASP.NET Web API 2 and ASP.NET MVC 5 and is available as [OSS on GitHub](https://github.com/aspnet/WebHooks).</span></span>

## <a name="webhooks-overview"></a><span data-ttu-id="8f2e4-118">Přehled Webhooků</span><span class="sxs-lookup"><span data-stu-id="8f2e4-118">WebHooks Overview</span></span>

<span data-ttu-id="8f2e4-119">Webhooky se vzor, což znamená, že se mění, jak se používá od služby do služby, ale základní myšlenka je stejný.</span><span class="sxs-lookup"><span data-stu-id="8f2e4-119">WebHooks is a pattern which means that it varies how it is used from service to service but the basic idea is the same.</span></span> <span data-ttu-id="8f2e4-120">Si můžete představit Webhooků jako modelu jednoduché pub/sub kde uživatel může přihlásit k odběru událostí děje jinde.</span><span class="sxs-lookup"><span data-stu-id="8f2e4-120">You can think of WebHooks as a simple pub/sub model where a user can subscribe to events happening elsewhere.</span></span> <span data-ttu-id="8f2e4-121">Oznamování událostí se rozšíří jako požadavky HTTP POST, který obsahuje informace o samotné události.</span><span class="sxs-lookup"><span data-stu-id="8f2e4-121">The event notifications are propagated as HTTP POST requests containing information about the event itself.</span></span>

<span data-ttu-id="8f2e4-122">Odeslání požadavku HTTP POST obvykle obsahuje objekt JSON nebo data formuláře HTML určené Webhooku odesílatele, včetně informací o události Webhooku k aktivaci.</span><span class="sxs-lookup"><span data-stu-id="8f2e4-122">Typically the HTTP POST request contains a JSON object or HTML form data determined by the WebHook sender including information about the event causing the WebHook to trigger.</span></span> <span data-ttu-id="8f2e4-123">Například příklad těla požadavku POST Webhooku z [Githubu](http://www.github.com/) vypadá podobně jako tento jako výsledek vytvoří nový problém v konkrétní úložiště:</span><span class="sxs-lookup"><span data-stu-id="8f2e4-123">For example, an example of a WebHook POST request body from [GitHub](http://www.github.com/) looks like this as a result of a new issue being opened in a particular repository:</span></span>

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

<span data-ttu-id="8f2e4-124">Požadavek POST tak, aby byl WebHook skutečně zamýšlené odesílatele, je zabezpečené nějakým způsobem a poté ověřeno, příjemce.</span><span class="sxs-lookup"><span data-stu-id="8f2e4-124">To ensure that the WebHook is indeed from the intended sender, the POST request is secured in some way and then verified by the receiver.</span></span> <span data-ttu-id="8f2e4-125">Například [Webhooky Githubu](https://developer.github.com/webhooks/) zahrnuje *X-Hub-podpis* hlavičku protokolu HTTP s hodnotu hash obsahu žádosti, které je zaškrtnuto implementací příjemce, aby nemuseli se starat o něm.</span><span class="sxs-lookup"><span data-stu-id="8f2e4-125">For example, [GitHub WebHooks](https://developer.github.com/webhooks/) includes an *X-Hub-Signature* HTTP header with a hash of the request body which is checked by the receiver implementation so you don't have to worry about it.</span></span>

<span data-ttu-id="8f2e4-126">WebHook flow obecně přejde vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="8f2e4-126">The WebHook flow generally goes something like this:</span></span>

* <span data-ttu-id="8f2e4-127">Odesílatel Webhooku zpřístupňuje události, které se můžete přihlásit k odběru klienta.</span><span class="sxs-lookup"><span data-stu-id="8f2e4-127">The WebHook sender exposes events that a client can subscribe to.</span></span> <span data-ttu-id="8f2e4-128">Události, které popisují pozorovatelných změny systému, například které nová datová položka byla vložený, aby proces dokončil nebo něco jiného.</span><span class="sxs-lookup"><span data-stu-id="8f2e4-128">The events describe observable changes to the system, for example that a new data item has been inserted, that a process has completed, or something else.</span></span>

* <span data-ttu-id="8f2e4-129">Příjemce Webhooku přihlásí registrace Webhooku, který se skládá ze čtyř akcí:</span><span class="sxs-lookup"><span data-stu-id="8f2e4-129">The WebHook receiver subscribes by registering a WebHook consisting of four things:</span></span>

     1. <span data-ttu-id="8f2e4-130">Identifikátor URI pro kde by měla ve formuláři požadavku HTTP POST; odeslat oznámení události</span><span class="sxs-lookup"><span data-stu-id="8f2e4-130">A URI for where the event notification should be posted in the form of an HTTP POST request;</span></span>

     2. <span data-ttu-id="8f2e4-131">Sady filtrů popisující konkrétní události, pro které by měl být aktivována WebHook;</span><span class="sxs-lookup"><span data-stu-id="8f2e4-131">A set of filters describing the particular events for which the WebHook should be fired;</span></span>

     3. <span data-ttu-id="8f2e4-132">Tajný klíč, který se používá k podepsání žádosti HTTP POST;</span><span class="sxs-lookup"><span data-stu-id="8f2e4-132">A secret key which is used to sign the HTTP POST request;</span></span>

     4. <span data-ttu-id="8f2e4-133">Další data, která mají být zahrnuty v požadavku HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="8f2e4-133">Additional data which is to be included in the HTTP POST request.</span></span> <span data-ttu-id="8f2e4-134">Například to může být další pole hlavičky protokolu HTTP nebo vlastností obsažených v textu požadavku HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="8f2e4-134">This can for example be additional HTTP header fields or properties included in the HTTP POST request body.</span></span>

* <span data-ttu-id="8f2e4-135">Po události dojde, jsou nalezeny odpovídající registrace Webhooku a odeslání požadavků HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="8f2e4-135">Once an event happens, the matching WebHook registrations are found and HTTP POST requests are submitted.</span></span> <span data-ttu-id="8f2e4-136">Obvykle jsou generování požadavky HTTP POST na opakovat několikrát, pokud pro z nějakého důvodu, že příjemce neodpovídá nebo výsledky požadavku HTTP POST do reakce na chybu.</span><span class="sxs-lookup"><span data-stu-id="8f2e4-136">Typically, the generation of the HTTP POST requests are retried several times if for some reason the recipient is not responding or the HTTP POST request results in an error response.</span></span>

## <a name="webhooks-processing-pipeline"></a><span data-ttu-id="8f2e4-137">Webhooky zpracování kanálu</span><span class="sxs-lookup"><span data-stu-id="8f2e4-137">WebHooks Processing Pipeline</span></span>

<span data-ttu-id="8f2e4-138">Microsoft ASP.NET WebHooks zpracování kanálu pro příchozí Webhooky vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="8f2e4-138">The Microsoft ASP.NET WebHooks processing pipeline for incoming WebHooks looks like this:</span></span>

![ASP.NET – Webhooky zpracování kanálu](_static/WebHookReceivers.png)

<span data-ttu-id="8f2e4-140">Jsou zde dva klíčové koncepty *příjemci* a *obslužné rutiny*:</span><span class="sxs-lookup"><span data-stu-id="8f2e4-140">The two key concepts here are *Receivers* and *Handlers*:</span></span>

* <span data-ttu-id="8f2e4-141">*Příjemci* nesou odpovědnost za zpracování konkrétní flavor Webhooku od daného odesílatele a pro vynucení kontroly zabezpečení tak, aby byl požadavek Webhooku skutečně zamýšlené odesílatele.</span><span class="sxs-lookup"><span data-stu-id="8f2e4-141">*Receivers* are responsible for handling the particular flavor of WebHook from a given sender and for enforcing security checks to ensure that the WebHook request indeed is from the intended sender.</span></span>

* <span data-ttu-id="8f2e4-142">*Obslužné rutiny* jsou obvykle, kde uživatelského kódu probíhá zpracování konkrétní Webhooku.</span><span class="sxs-lookup"><span data-stu-id="8f2e4-142">*Handlers* are typically where user code runs processing the particular WebHook.</span></span>

<span data-ttu-id="8f2e4-143">V následujících uzlech jsou tyto koncepty popsané v další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="8f2e4-143">In the following nodes these concepts are described in more details.</span></span>
