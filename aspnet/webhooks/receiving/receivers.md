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
# <a name="aspnet-webhooks-receivers"></a>ASP.NET – Webhooky příjemců

Přijímá Webhooky závisí na to, kdo je odesílatele. Někdy je potřeba další kroky registrace Webhooku ověření, že je ve skutečnosti naslouchání odběratele. Některé Webhooky poskytují model push pull kde požadavku HTTP POST obsahuje pouze odkaz na informace o události, které je pak načíst nezávisle. Model zabezpečení často mění poměrně trochu.

Účelem WebHooks Microsoft ASP.NET je jednodušší a konzistentnější propojí vaše rozhraní API bez mnoho času vymýšlení na zpracování jakékoli varianta Webhooky.

Příjemce Webhooku je zodpovědná za přijímání a ověření Webhooky od konkrétního odesílatele. WebHook příjemce může podporovat libovolný počet Webhooků, každý s vlastní konfigurací. WebHook Githubu příjemce může například přijmout Webhooků z různých úložišť GitHub.

## <a name="webhook-receiver-uris"></a>Identifikátory URI příjemce Webhooku

Instalace Microsoft ASP.NET WebHooks získáte řadiči obecný WebHook, který přijímá požadavky Webhooku od konkrétní počet služeb. Když přijde požadavek, vybere vhodné příjemce, kterou jste nainstalovali pro zpracování konkrétního odesílatele Webhooku.

Identifikátor URI tohoto kontroleru je identifikátor URI Webhooku, která můžete zaregistrovat do služby a má formát:

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

Z bezpečnostních důvodů vyžadují mnoho příjemci Webhooku, že je identifikátor URI *https* identifikátor URI a v některých případech musí obsahovat také další dotaz parametr, který se používá k vynucení, který pouze zamýšlené strany můžete poslat Webhooky výše identifikátoru URI .

<em> <receiver> </em> Komponenta je jméno příjemce, například <em>githubu</em> nebo <em>slack</em>.

*{Id}* je volitelný identifikátor, který slouží k identifikaci konkrétní konfigurace příjemce Webhooku. To je možné zaregistrovat N Webhooky u konkrétního příjemce. Například následující tři identifikátory URI slouží k registraci pro tři nezávislá Webhooků:

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a>Instalace příjemce Webhooku

Pro příjem Webhooků pomocí Microsoft ASP.NET WebHooks, je nejprve nainstalovat balíček Nuget pro poskytovatele WebHook nebo chcete dostávat Webhooků z poskytovatelů. Balíčky Nuget jsou pojmenovány [Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) označuje, kde poslední část služby nepodporuje. Příklad

[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) poskytuje podporu pro příjem Webhooků z Githubu a [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) poskytuje podporu pro příjem Webhooky generovaných ASP. NET Webhooky.

Okamžité najdete podporu pro Dropbox, GitHub, MailChimp, PayPal, Pusheru, Salesforce, Slack, Stripe, Trello a WordPress, ale je možné pro podporu libovolný počet dalších poskytovatelů.

## <a name="configuring-a-webhook-receiver"></a>Konfigurace Webhooku příjemce

Příjemci Webhooku se konfigurují [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) konkrétní implementace rozhraní a rozhraní lze registrovat pomocí jakéhokoli modelu vkládání závislostí. Výchozí implementace používá nastavení aplikace, které je možné nastavit buď v souboru Web.config, nebo pokud používáte Azure Web Apps, můžete nastavit prostřednictvím [webu Azure Portal](https://portal.azure.com/).

![Nastavení aplikace Azure](_static/AzureAppSettings.png)

Formát pro nastavení aplikace klíče je následující:

```
MS_WebHookReceiverSecret_<receiver>
```

Hodnota je čárkou oddělený seznam hodnot, které odpovídají *{id}* hodnoty, u kterých Webhooky jsou zaregistrovány, například:

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a>Inicializace příjemce Webhooku

WebHook příjemci jsou inicializovány tak, že zaregistrujete, obvykle v *WebApiConfig* statické třídy, například:

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
