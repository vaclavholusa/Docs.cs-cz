---
uid: webhooks/receiving/receivers
title: Příjemci Webhooky ASP.NET | Microsoft Docs
author: rick-anderson
description: Příjemci Webhooky ASP.NET
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: a8e42521f201f88b0ed433550e8786411b4487b0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30896293"
---
# <a name="aspnet-webhooks-receivers"></a>Příjemci Webhooky ASP.NET

Přijetí Webhooky závisí na to, kdo je odesílatele. Někdy existují další kroky registrace WebHook, jehož ověření skutečně naslouchá odběratele. Některé Webhooky poskytnout model push pull kde požadavku HTTP POST pouze obsahuje odkaz na informace o události, které je pak možné načíst nezávisle. Často modelu zabezpečení se liší odlišují.

Účelem WebHooks Microsoft ASP.NET je jednodušší a konzistentní propojit se rozhraní API bez výdaje mnoho času nad tím, jak zpracovat žádné konkrétní variant Webhooky.

Příjemce Webhooku je zodpovědná za přijímání a ověření Webhooky od určitého odesílatele. Webhooku příjemce může podporovat libovolný počet Webhooků, každou s vlastní konfigurace. WebHook Githubu příjemce může například přijmout Webhooky z různých úložišť GitHub.

## <a name="webhook-receiver-uris"></a>WebHook Receiver URIs

Nainstalováním Microsoft ASP.NET WebHooks získáte obecné Webhooku řadiči, který přijímá požadavky Webhooku od zprostředkovává počet služeb. Pokud dorazí požadavek, vybere odpovídající příjemce, který jste nainstalovali pro zpracování určitého Webhooku odesílatele.

Identifikátor URI tohoto řadiče je identifikátor URI Webhooku registrace pomocí služby a je ve formátu:

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

Z bezpečnostních důvodů mnoho Webhooku příjemci vyžadují, aby identifikátor URI *https* URI a v některých případech musí také obsahovat parametrem další dotaz, který slouží k vynucení, jenom zamýšlený strany může odesílat Webhooky na výše uvedené identifikátor URI .

<em> <receiver> </em> Součást je název příjemce, například <em>githubu</em> nebo <em>slack</em>.

*{Id}* je volitelné identifikátor, který slouží k identifikaci konkrétní konfigurací Webhooku příjemce. To slouží k registraci N Webhooky s konkrétní příjemce. Například následující tři identifikátory URI slouží k registraci pro tři nezávislá Webhooky:

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a>Instalace Webhooku příjemce

Pro příjem Webhooky pomocí WebHooks Microsoft ASP.NET, je nejprve nainstalovat balíček Nuget pro zprostředkovatele Webhooku nebo poskytovatelů, které chcete dostávat Webhooky z. Balíčky Nuget jsou pojmenované [Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) Určuje, kde poslední část službu podporována. Příklad

[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) poskytuje podporu pro příjem Webhooky z Githubu a [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) poskytuje podporu pro příjem Webhooky generované ASP. NET Webhooky.

Ihned můžete najít podpora Dropbox, Githubu, MailChimp, PayPal, tlačná, Salesforce, Slack, Stripe, Trello a WordPress, ale je možné podporovat libovolný počet dalších zprostředkovatelů.

## <a name="configuring-a-webhook-receiver"></a>Konfigurace Webhooku příjemce

Příjemci Webhooku jsou nakonfigurované pomocí [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) rozhraní a konkrétní implementace tohoto rozhraní lze registrovat pomocí kteréhokoli modelu vkládání závislostí. Výchozí implementace používá nastavení aplikace, která může být buď nastavena v souboru Web.config, nebo pokud používáte Azure Web Apps, je možné nastavit prostřednictvím [portálu Azure](https://portal.azure.com/).

![Nastavení aplikace Azure](_static/AzureAppSettings.png)

Formát pro nastavení aplikace klíče je následující:

```
MS_WebHookReceiverSecret_<receiver>
```

Hodnota je čárkami oddělený seznam hodnot odpovídající *{id}* hodnoty, pro které Webhooky byly zaregistrovány, například:

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a>Inicializace Webhooku příjemce

Příjemci Webhooku se inicializují registrací je obvykle ve *WebApiConfig* statické třídy, například:

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
