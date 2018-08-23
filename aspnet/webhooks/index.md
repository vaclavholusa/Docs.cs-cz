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
# <a name="aspnet-webhooks-overview"></a>Přehled ASP.NET – Webhooky

Webhooky se odlehčeného vzoru HTTP poskytuje jednoduché pub/sub model pro vzájemné propojení dohromady webová rozhraní API a služby SaaS. Případě určité události ve službě, oznámení se posílá ve formuláři požadavku HTTP POST pro registrované předplatitele. Požadavek POST obsahuje informace o události, která umožňuje příjemci příslušně na ně reagovat.

Z důvodu jejich jednoduchost Webhooky jsou již vystavené velkým množstvím služeb včetně [Dropboxu](http://dropbox.com/), [Githubu](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/)a mnoho dalších. Například Webhooku může znamenat, že soubor byl změněn v [Dropboxu](http://dropbox.com/), změny kódu byl potvrzen v Githubu nebo platby byl zahájen v [PayPal](http://www.paypal.com/), nebo na kartě se vytvořil v [ Trello](http://www.trello.com/). Možností je nekonečně!

Microsoft ASP.NET WebHooks usnadňuje odesílat i přijímat Webhooků jako součást aplikace ASP.NET:

* Na straně příjmu poskytuje společný model pro příjem a zpracování Webhooků z různých zprostředkovatelů Webhooku. Jde o předpřipravených s podporou [Dropboxu](http://dropbox.com/), [Githubu](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusheru](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) a [Zendesku](https://www.zendesk.com/) je ale také snadné přidat podporu pro víc.

* Na straně odesílání poskytuje podporu pro správu a ukládání předplatná stejně jako u odesílání oznámení událostí na správnou sadu předplatitele. To umožňuje definovat vlastní sadu událostí, můžete se přihlásit k odběru a upozorňovat na co se stane, předplatitele.

Dvě části je možné společně nebo od sebe v závislosti na vašem scénáři. Pokud potřebujete přijímá Webhooky z jiných služeb, můžete použít jenom část příjemce; Pokud chcete vystavit Webhooky pro ostatní uživatele používat, můžete přesně to provést.

Kód, zaměřuje na technologie ASP.NET Web API 2 a ASP.NET MVC 5 a je k dispozici jako [OSS na Githubu](https://github.com/aspnet/WebHooks).

## <a name="webhooks-overview"></a>Přehled Webhooků

Webhooky se vzor, což znamená, že se mění, jak se používá od služby do služby, ale základní myšlenka je stejný. Si můžete představit Webhooků jako modelu jednoduché pub/sub kde uživatel může přihlásit k odběru událostí děje jinde. Oznamování událostí se rozšíří jako požadavky HTTP POST, který obsahuje informace o samotné události.

Odeslání požadavku HTTP POST obvykle obsahuje objekt JSON nebo data formuláře HTML určené Webhooku odesílatele, včetně informací o události Webhooku k aktivaci. Například příklad těla požadavku POST Webhooku z [Githubu](http://www.github.com/) vypadá podobně jako tento jako výsledek vytvoří nový problém v konkrétní úložiště:

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

Požadavek POST tak, aby byl WebHook skutečně zamýšlené odesílatele, je zabezpečené nějakým způsobem a poté ověřeno, příjemce. Například [Webhooky Githubu](https://developer.github.com/webhooks/) zahrnuje *X-Hub-podpis* hlavičku protokolu HTTP s hodnotu hash obsahu žádosti, které je zaškrtnuto implementací příjemce, aby nemuseli se starat o něm.

WebHook flow obecně přejde vypadat přibližně takto:

* Odesílatel Webhooku zpřístupňuje události, které se můžete přihlásit k odběru klienta. Události, které popisují pozorovatelných změny systému, například které nová datová položka byla vložený, aby proces dokončil nebo něco jiného.

* Příjemce Webhooku přihlásí registrace Webhooku, který se skládá ze čtyř akcí:

     1. Identifikátor URI pro kde by měla ve formuláři požadavku HTTP POST; odeslat oznámení události

     2. Sady filtrů popisující konkrétní události, pro které by měl být aktivována WebHook;

     3. Tajný klíč, který se používá k podepsání žádosti HTTP POST;

     4. Další data, která mají být zahrnuty v požadavku HTTP POST. Například to může být další pole hlavičky protokolu HTTP nebo vlastností obsažených v textu požadavku HTTP POST.

* Po události dojde, jsou nalezeny odpovídající registrace Webhooku a odeslání požadavků HTTP POST. Obvykle jsou generování požadavky HTTP POST na opakovat několikrát, pokud pro z nějakého důvodu, že příjemce neodpovídá nebo výsledky požadavku HTTP POST do reakce na chybu.

## <a name="webhooks-processing-pipeline"></a>Webhooky zpracování kanálu

Microsoft ASP.NET WebHooks zpracování kanálu pro příchozí Webhooky vypadá takto:

![ASP.NET – Webhooky zpracování kanálu](_static/WebHookReceivers.png)

Jsou zde dva klíčové koncepty *příjemci* a *obslužné rutiny*:

* *Příjemci* nesou odpovědnost za zpracování konkrétní flavor Webhooku od daného odesílatele a pro vynucení kontroly zabezpečení tak, aby byl požadavek Webhooku skutečně zamýšlené odesílatele.

* *Obslužné rutiny* jsou obvykle, kde uživatelského kódu probíhá zpracování konkrétní Webhooku.

V následujících uzlech jsou tyto koncepty popsané v další podrobnosti.
