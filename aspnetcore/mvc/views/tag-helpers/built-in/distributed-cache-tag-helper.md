---
title: Distribuovaná mezipaměť pomocná rutina značek v ASP.NET Core
author: pkellner
description: Další informace o použití distribuované mezipaměti pomocná rutina značek v.
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: 1b51164a6d3dab2eeaf64262d6f0d9961bd00d12
ms.sourcegitcommit: 4d5f8680d68b39c411b46c73f7014f8aa0f12026
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/24/2018
ms.locfileid: "47028079"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a>Distribuovaná mezipaměť pomocná rutina značek v ASP.NET Core

Podle [Peter Kellner](http://peterkellner.net) 

Pomocná rutina značek distribuované mezipaměti umožňuje výrazně zlepšit výkon vaší aplikace ASP.NET Core pomocí ukládání do mezipaměti obsah ke zdroji distribuované mezipaměti.

Distribuované mezipaměti pomocná rutina značek v dědí ze základní třídy stejné jako pomocné rutiny značky mezipaměti. Všechny atributy přidružené k pomocné rutiny značky mezipaměti, budou fungovat i na pomocné rutiny značky distribuovat.

Následuje distribuované mezipaměti pomocná rutina značek v **explicitní závislosti Princip** říká **konstruktor vkládání**. Konkrétně `IDistributedCache` rozhraní kontejneru je předána do konstruktoru distribuované mezipaměti pomocné rutiny značky společnosti. Pokud žádné konkrétní konkrétní implementaci `IDistributedCache` byl vytvořen `ConfigureServices`, obvykle nachází v souboru startup.cs a pak distribuované mezipaměti pomocná rutina značek v bude použit stejný zprostředkovatel v paměti pro ukládání dat uložených v mezipaměti jako základní pomocná rutina značek mezipaměti.

## <a name="distributed-cache-tag-helper-attributes"></a>Distribuovaná mezipaměť atributy pomocné rutiny značky

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a>povolené vyprší dne vyprší po vypršení platnosti klouzavé měnit podle hlavičky se liší podle dotazu se liší podle postupu se liší podle cookie se liší podle uživatele se liší podle priority

Viz pomocná rutina značek v mezipaměti pro definice. Pomocná rutina značek v distribuované mezipaměti dědí ze stejné třídy jako pomocná rutina značek v mezipaměti, takže tyto atributy jsou běžné z pomocná rutina značek v mezipaměti.

- - -

### <a name="name-required"></a>název (povinné)

| Typ atributu    | Příklad hodnoty     |
|----------------   |----------------   |
| odkazy řetězců    | "my-distributed-cache-unique-key-101"     |

Požadované `name` atribut se používá jako klíč do mezipaměti ukládají pro každou instanci distribuované mezipaměti pomocné rutiny značky. Na rozdíl od základní mezipaměti pomocné rutiny značky, který přiřazuje klíč ke každé instanci pomocná rutina značek v mezipaměti na základě názvu stránky Razor a umístění pomocná rutina značky razor stránce distribuované mezipaměti pomocná rutina značek v pouze základů svůj klíč atributu `name`

Příklad použití:

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>Implementace distribuované mezipaměti IDistributedCache pomocné rutiny značky

Existují dvě implementace `IDistributedCache` součástí ASP.NET Core. Jeden je založena na SQL Server a druhý je založen na Redis. Podrobnosti o těchto implementacích najdete <xref:performance/caching/distributed>. Obou implementacích zahrnují nastavení instance objektu `IDistributedCache` v ASP.NET Core *Startup.cs*.

Neexistují žádné atributy značky konkrétně spojených s použitím žádnou konkrétní implementaci `IDistributedCache`.

## <a name="additional-resources"></a>Další zdroje

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
