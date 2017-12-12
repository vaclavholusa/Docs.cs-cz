---
title: "Distribuované mezipaměti značky pomocná | Microsoft Docs"
author: pkellner
description: "Ukazuje, jak pracovat s pomocná značky mezipaměti"
keywords: "ASP.NET Core, značka pomocné rutiny"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a022
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: 462c3775677924fc7b9b715cd6de75fe53ada89e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="distributed-cache-tag-helper"></a>Pomocník značky distribuované mezipaměti

Podle [Petr Kellner](http://peterkellner.net) 


Pomocník pro značku distribuované mezipaměti umožňuje výrazně zlepšit výkon vaší aplikace ASP.NET Core pomocí ukládání do mezipaměti jeho obsah ke zdroji distribuované mezipaměti.

Pomocník distribuované mezipaměti značky dědí ze stejné základní třídy jako pomocný značky mezipaměti.  Všechny atributy, které jsou spojené s pomocnou rutinou značky mezipaměti budou také fungovat na pomocná distribuované značky.


Pomocník distribuované mezipaměti značka odpovídá **explicitní závislosti Princip** známé jako **konstruktor vkládání**.  Konkrétně `IDistributedCache` rozhraní kontejneru je předána do distribuované mezipaměti značky pomocné rutiny pro konstruktor.  Pokud žádné konkrétní konkrétní implementaci `IDistributedCache` byla vytvořena v `ConfigureServices`, obvykle najít v souboru startup.cs, pak pomocná distribuované mezipaměti značky použije stejný zprostředkovatel v paměti pro ukládání data uložená v mezipaměti jako základní značky pomocná mezipaměti.

## <a name="distributed-cache-tag-helper-attributes"></a>Distribuované mezipaměti atributů pomocná značky

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a>povoleno na vyprší platnost vyprší po vypršení platnosti klouzavé měnit podle hlavičky se liší podle dotazu se liší podle trasy se liší podle cookie se liší podle uživatele lišit podle priority

Definice naleznete v tématu Pomocník značky mezipaměti. Pomocník značky distribuované mezipaměti dědí z stejnou třídou jako mezipaměti značky pomocná tak tyto atributy jsou společné z mezipaměti značky pomocníka.

- - -

### <a name="name-required"></a>název (povinné)

| Typ atributu    | Příklad hodnoty     |
|----------------   |----------------   |
| odkazy řetězců    | "my-distributed-cache-unique-key-101"     |

Požadované `name` atribut slouží jako klíč do této mezipaměti uložené pro každou instanci pomocná značky distribuované mezipaměti.  Na rozdíl od základní pomocná značky mezipaměti, který přiřazuje klíč každý mezipaměti značky pomocná instance na základě názvu stránky Razor a umístění pomocníka značky, na stránce razor pomocná distribuované mezipaměti značky pouze základny jeho klíče na atributu`name`

Příklad použití:

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>Distribuované mezipaměti značky pomocná IDistributedCache implementace

Existují dvě implementace `IDistributedCache` součástí ASP.NET Core.  Je na základě **systému Sql Server** a druhý je založena na **Redis**. Podrobnosti o těchto implementace lze najít na prostředek dále pojmenovaný "pracovní s distribuované mezipaměti". Obě implementace zahrnovat nastavení instanci `IDistributedCache` v ASP.NET Core **startup.cs**.

Neexistují žádné atributů značky konkrétně související s používáním žádnou konkrétní implementaci `IDistributedCache`.



- - -



## <a name="additional-resources"></a>Další zdroje

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
