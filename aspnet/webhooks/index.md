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
# <a name="aspnet-webhooks-overview"></a>Přehled Webhooky ASP.NET

Webhooky je lightweight vzor HTTP poskytuje jednoduché pub nebo dílčí model pro připojení služby webovým rozhraním API a SaaS společně. V případě události ve službě je odesláno upozornění ve formě požadavek HTTP POST registrované odběratelům. Požadavek POST obsahuje informace o události, která vám umožní pro příjemce tak, aby fungoval odpovídajícím způsobem.

Kvůli jejich jednoduchost Webhooky jsou již vystavené velký počet služeb včetně [Dropbox](http://dropbox.com/), [Githubu](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/)a mnoho dalšího. Například WebHook, jehož může naznačovat, že soubor se změnila v [Dropbox](http://dropbox.com/), nebo změnit kód byl potvrzen v Githubu, nebo byla inicializována platba v [PayPal](http://www.paypal.com/), nebo na kartě byl vytvořen v [ Trello](http://www.trello.com/). Existuje mnoho možností!

Microsoft ASP.NET WebHooks usnadňuje odesílat i přijímat Webhooky jako součást aplikace ASP.NET:

* Na straně příjmu poskytuje společný model pro přijímání a zpracování Webhooky z libovolný počet Webhooku poskytovatelů. Vrátí z pole s podporou [Dropbox](http://dropbox.com/), [Githubu](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Tlačná](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) a [Zendesk](https://www.zendesk.com/) , ale je snadné přidání podpory pro další.

* Na straně odesílání poskytuje podporu pro správu a ukládání odběry stejně jako u odesílání oznámení událostí na správnou sadu odběratelů. To umožňuje definovat vlastní sadu událostí, můžete se přihlásit k odběru a je upozornit, když se stane věcí odběratele.

Dvě části můžete použít společně nebo od sebe v závislosti na vašem scénáři. Pokud potřebujete přijímat Webhooky z jiných služeb, můžete použít jenom část příjemce; Pokud chcete vystavit Webhooky pro ostatní využívat, pak můžete to udělat jen.

Kód cílí webovém rozhraní API 2 ASP.NET a ASP.NET MVC 5 a je k dispozici jako [operačních systémů na Githubu](https://github.com/aspnet/WebHooks).

## <a name="webhooks-overview"></a>Přehled Webhooky

Webhooky je vzor, což znamená, že se liší, jak se používají ze služby pro služby, ale základní Rada je stejný. Si můžete představit Webhooky jako model jednoduché pub nebo sub kde uživatel může přihlásit k odběru událostí děje jinde. Oznamování událostí rozšířeny jako požadavky HTTP POST obsahující informace o samotné události.

Požadavek HTTP POST obvykle obsahuje objekt JSON nebo data formuláře HTML určit Webhooku odesílatel, včetně informací o události, která způsobila Webhooku pro aktivaci. Například příklad těla požadavku POST Webhooku z [Githubu](http://www.github.com/) vypadá jako to v důsledku nový problém otevíráte v konkrétní úložiště:

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

K zajištění, že Webhooku je skutečně od určený odesílatele, je požadavek POST zabezpečené nějakým způsobem a potom ověřit pomocí příjemce. Například [Githubu Webhooky](https://developer.github.com/webhooks/) zahrnuje *X-Hub-podpis* hlavičky protokolu HTTP se hodnota hash obsahu žádosti, které je zaškrtnuto implementací příjemce, takže nemusíte si dělat starosti o něm.

Tok Webhooku obecně přejde přibližně takto:

* Odesílatel Webhooku zpřístupní události, které klient může přihlásit k odběru. Události, které popisují pozorovatelné změny systému, například které nová datová položka byla vložené, zda byla dokončena proces, nebo něco jiného.

* Příjemce Webhooku jako odběratel tak, že zaregistrujete Webhooku, který se skládá ze čtyř akcí:

     1. Identifikátor URI pro kde by měla odeslat oznámení události ve formě požadavek HTTP POST;

     2. Sadu filtrů popisující konkrétní události, pro které má být aktivována Webhooku;

     3. Tajný klíč, který se používá k podepisování požadavku HTTP POST;

     4. Další data, která má být zahrnut v požadavku HTTP POST. Například to může být další pole hlavičky protokolu HTTP nebo vlastnosti, které jsou zahrnuty v textu požadavku HTTP POST.

* Poté, co se stane událost, nebyly nalezeny odpovídající Webhooku registrace a jsou odeslány požadavky HTTP POST. Obvykle generování požadavky HTTP POST jsou opakovány několikrát pokud z nějakého důvodu, že příjemce neodpovídá nebo výsledky požadavku HTTP POST ve chybnou odpověď.

## <a name="webhooks-processing-pipeline"></a>Webhooky zpracování kanálu

Microsoft ASP.NET WebHooks zpracování kanálu pro příchozí Webhooky vypadá takto:

![ASP.NET Webhooky zpracování kanálu](_static/WebHookReceivers.png)

Jsou zde dva klíčové koncepty *příjemci* a *obslužné rutiny*:

* *Příjemci* zodpovídají pro zpracování konkrétní příchuť Webhooku od daného odesílatele a vynucování zabezpečení kontroly, aby se zajistilo, že skutečně jedná o žádost Webhooku z určený odesílatele.

* *Obslužné rutiny* jsou obvykle, kde se spouští zpracování konkrétní Webhooku uživatelského kódu.

V následující uzly jsou tyto koncepty popsané v další podrobnosti.
