---
title: "Účel řetězců v ASP.NET Core"
author: rick-anderson
description: "Tento dokument popisuje účel řetězec hierarchie a víceklientský ve vztahu k ochrany dat ASP.NET Core rozhraní API."
keywords: "ASP.NET Core, ochrany dat, účel řetězce"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 9d18c287-e0e6-4541-b09c-7fed45c902d9
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/purpose-strings-multitenancy
ms.openlocfilehash: ce506710042bfb76015c3af991b17e018b78e970
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="purpose-hierarchy-and-multi-tenancy-in-aspnet-core"></a>Účel hierarchie a víceklientský v ASP.NET Core

Protože `IDataProtector` implicitně je také `IDataProtectionProvider`, účely dají se propojit. V tomto smyslu `provider.CreateProtector([ "purpose1", "purpose2" ])` je ekvivalentní `provider.CreateProtector("purpose1").CreateProtector("purpose2")`.

To umožňuje některé zajímavé hierarchické vztahy prostřednictvím systému ochrany dat. V dřívějších příkladu [Contoso.Messaging.SecureMessage](purpose-strings.md#data-protection-contoso-purpose), můžete volat komponentu SecureMessage `provider.CreateProtector("Contoso.Messaging.SecureMessage")` jednou počáteční a mezipaměti výsledek do privátního `_myProvide` pole. Budoucí ochrany mohou být vytvořeny prostřednictvím volání `_myProvider.CreateProtector("User: username")`, a tato ochrana se použije pro zabezpečení jednotlivých zpráv.

To může také překlopit. Zvažte jednu logickou aplikaci, které hostitele více klientů (systém správy obsahu zdá se, že přiměřené) a každý klient můžete nakonfigurovat vlastní ověřování a stavu systému správy. Aplikace zastřešující má jednoho hlavní zprostředkovatele, a volá `provider.CreateProtector("Tenant 1")` a `provider.CreateProtector("Tenant 2")` poskytnout vlastní izolované řez systému ochrany dat každého klienta. Klienti potom může odvozena vlastní jednotlivých ochrany na základě svých vlastních potřeb, ale bez ohledu na to, jak pevného snaží uživatel nemůže vytvořit ochrany, které jsou v konfliktu s žádným jiným klientem v systému. Graficky to je reprezentována jako níže.

![Pro účely více klientů](purpose-strings-multitenancy/_static/purposes-multi-tenancy.png)

>[!WARNING]
> Předpokladem zastřešující ovládací prvky aplikace, které rozhraní API jsou k dispozici pro jednotlivé klienty a že klienti nelze spustit libovolný kód na serveru. Pokud klienta můžete spustit libovolný kód, privátní reflexe pro přerušení izolace záruky, které může fungovat, nebo může jenom číst materiál hlavního klíče přímo a odvodí ať podklíče chtějí mít.

Systém ochrany dat ve skutečnosti používá řazení víceklientský ve výchozím nastavení se na pole. Ve výchozím nastavení je jejich obsah uložen ve složce profilu uživatele účet pracovního procesu (nebo registru pro identity fondu aplikací služby IIS). Ale je ve skutečnosti poměrně společné pro jeden účet použít ke spuštění více aplikací, a proto by tyto aplikace skončili sdílení hlavní Forward. Chcete-li tento problém vyřešit, systému ochrany dat automaticky vloží identifikátor jedinečný každou aplikaci jako první prvek v řetězu celkové účel. Tento implicitní účel slouží k [izolovat jednotlivých aplikací](xref:security/data-protection/configuration/overview#per-application-isolation) jeden z druhého tak, že efektivně považuje každou aplikaci, jak vypadá jedinečný klienta v rámci systému a proces tvorby ochrana stejný jako na obrázku výše.
