---
title: "Práce s distribuované mezipaměti ASP.NET Core"
author: ardalis
description: "Naučte se používat distribuované ukládání do mezipaměti ke zlepšení výkonu a škálovatelnosti aplikace ASP.NET Core, zejména v případě, že hostovaný v cloudu nebo server prostředí farmy."
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: performance/caching/distributed
ms.openlocfilehash: a0af4887143f6ed37a1af982ec21a2ad5eae9515
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="working-with-a-distributed-cache-in-aspnet-core"></a>Práce s distribuované mezipaměti ASP.NET Core

Podle [Steve Smith](https://ardalis.com/)

Distribuované mezipaměti může zvýšit výkon a škálovatelnost aplikací ASP.NET Core, zejména v případě, že hostovaný v cloudu nebo server prostředí farmy. Tento článek vysvětluje, jak pracovat s implementace a abstrakce předdefinované distribuované mezipaměti ASP.NET Core.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-a-distributed-cache"></a>Co je distribuované mezipaměti

Distribuované mezipaměti, je sdílen více serverů aplikace (viz [základní informace o ukládání do mezipaměti](memory.md#caching-basics)). Informace v mezipaměti nejsou uloženy v paměti jednotlivých webových serverů a data uložená v mezipaměti je k dispozici pro všechny servery aplikace. To poskytuje několik výhod:

1. Data uložená v mezipaměti je souvislý ve všech webových serverech. Uživatelé neuvidí odlišné výsledky v závislosti na tom, které webový server zpracovává jeho žádost

2. Data uložená v mezipaměti odolává webový server se restartuje a nasazení. Jednotlivé webové servery můžete odebrat nebo přidány bez mezipaměti, které mají vliv.

3. Zdrojového úložiště dat obsahuje méně požadavky na jeho (než s více mezipaměti v paměti nebo ne mezipaměti vůbec).

> [!NOTE]
> Pokud používáte SQL Server distribuované mezipaměti, některé tyto výhody jsou pouze hodnotu true, pokud instance samostatné databáze se používá pro mezipaměť než pro zdroj dat aplikace.

Stejně jako všechny mezipaměť můžete distribuované mezipaměti výrazné zlepšení aplikace odezvy, vzhledem k tomu obvykle můžete načíst data z mezipaměti mnohem rychlejší než z relační databáze (nebo webové služby).

Konfigurace mezipaměti je konkrétní implementace. Tento článek popisuje, jak nakonfigurovat i Redis a ukládá do mezipaměti distribuované systému SQL Server. Bez ohledu na to, které implementace je vybrána, aplikace komunikuje s mezipamětí pomocí společného `IDistributedCache` rozhraní.

## <a name="the-idistributedcache-interface"></a>Rozhraní IDistributedCache

`IDistributedCache` Rozhraní zahrnuje synchronní a asynchronní metody. Rozhraní umožňuje položky přidat, načíst a odebrat z implementace distribuované mezipaměti. `IDistributedCache` Rozhraní zahrnuje následující metody:

**Get, GetAsync**

Přebírá řetězec klíč a načte jako položka v mezipaměti `byte[]` Pokud nalezen v mezipaměti.

**Set, SetAsync**

Přidá položku (jako `byte[]`) do mezipaměti pomocí klíče řetězec.

**Aktualizace, RefreshAsync**

Aktualizuje položky v mezipaměti na základě jeho klíče resetování jeho klouzavé vypršení časového limitu (pokud existuje).

**Odebrat, asynchronně odebere**

Odebere položku mezipaměti na základě jeho klíče.

Chcete-li použít `IDistributedCache` rozhraní:

   1. Do souboru projektu přidáte požadované balíčky NuGet.

   2. Nakonfigurujte konkrétní implementace `IDistributedCache` ve vaší `Startup` třídy `ConfigureServices` metoda a přidejte ji do kontejneru existuje.

   3. Z aplikace [Middleware](../../fundamentals/middleware.md) nebo třídy řadiče MVC, požádejte o instanci `IDistributedCache` z konstruktoru. Instance poskytovaný [vkládání závislostí](../../fundamentals/dependency-injection.md) (DI).

> [!NOTE]
> Není nutné používat Singleton nebo Scoped doba života pro `IDistributedCache` instance (alespoň pro předdefinované implementace). Můžete také vytvořit instance, bez ohledu na jednu může být nutné (místo použití [vkládání závislostí](../../fundamentals/dependency-injection.md)), ale může být váš kód těžší chcete otestovat a je v rozporu [explicitní závislosti Princip](http://deviq.com/explicit-dependencies-principle/).

Následující příklad ukazuje, jak použít instanci `IDistributedCache` v komponentě jednoduché middleware:

[!code-csharp[Main](./distributed/sample/src/DistCacheSample/StartTimeHeader.cs?highlight=15,18,21,27,28,29,30,31)]

Ve výše uvedeném kódu je hodnota uložená v mezipaměti pro čtení, ale nikdy zapsána. V této ukázce je hodnota nastavit pouze při spuštění serveru a nezmění. Ve scénáři s více serverů přepíše nejnovější serveru spusťte všechny předchozí hodnoty, které byly nastavené zásadami jiných serverů. `Get` a `Set` pomocí metody `byte[]` typu. Proto řetězcovou hodnotu převést pomocí `Encoding.UTF8.GetString` (pro `Get`) a `Encoding.UTF8.GetBytes` (pro `Set`).

Následující kód z *Startup.cs* ukazuje, nastaví se hodnota:

[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=2,4,5,6&range=58-66)]

> [!NOTE]
> Vzhledem k tomu `IDistributedCache` je nakonfigurovaný v `ConfigureServices` metoda, je k dispozici `Configure` metoda jako parametr. Přidání jako parametr vám umožní nakonfigurované instance poskytované prostřednictvím DI.

## <a name="using-a-redis-distributed-cache"></a>Použití Redis distribuované mezipaměti

[Redis](https://redis.io/) je úložiště dat v paměti s otevřeným zdrojem, který se často používá jako distribuované mezipaměti. Můžete ji použít místně, a můžete nakonfigurovat [Azure Redis Cache](https://azure.microsoft.com/services/cache/) pro vaše aplikace Azure hostovaná ASP.NET Core. Aplikace ASP.NET Core nakonfiguruje mezipaměti pomocí implementace `RedisDistributedCache` instance.

Konfigurace v implementaci Redis v `ConfigureServices` a k němu přístup v kódu aplikace tím, že požádá o instanci `IDistributedCache` (viz výše uvedeném kódu).

V ukázkovém kódu `RedisCache` implementace se používá, když je server nakonfigurovaný pro `Staging` prostředí. Proto `ConfigureStagingServices` metoda nakonfiguruje `RedisCache`:

[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=8,9,10,11,12,13&range=27-40)]

> [!NOTE]
> Na místním počítači nainstalovat Redis, nainstalujte balíček chocolatey [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) a spusťte `redis-server` z příkazového řádku.

## <a name="using-a-sql-server-distributed-cache"></a>Pomocí systému SQL Server distribuované mezipaměti

Implementace SqlServerCache umožňuje distribuované mezipaměti používat databázi systému SQL Server jako své úložiště zálohování. Chcete-li vytvořit systému SQL Server tabulku můžete použít nástroj sql mezipaměti, nástroj vytvoří tabulku s názvem a schéma, které zadáte.

Chcete-li použít nástroj sql mezipaměti, přidejte `SqlConfig.Tools` k `<ItemGroup>` element *.csproj* souboru a spusťte obnovení dotnet.

[!code-xml[Main](./distributed/sample/src/DistCacheSample/DistCacheSample.csproj?range=23-25)]

Otestujte SqlConfig.Tools spuštěním následujícího příkazu

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create --help
   ```

Nástroj SQL mezipaměti zobrazí využití, možnosti a příkaz nápovědy, teď můžete vytvářet tabulky do systému sql server, spuštěním příkazu "sql mezipaměti vytvořit":

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
   info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
       Table and index were created successfully.
   ```

Vytvoření tabulky má následující schéma:

![SqlServer Cache Table](distributed/_static/SqlServerCacheTable.png)

Podobně jako všechny implementace mezipaměti, musí aplikace získávat a nastavovat hodnoty mezipaměti pomocí instance `IDistributedCache`, nikoli `SqlServerCache`. Ukázka implementuje `SqlServerCache` v `Production` prostředí (tak, že je nakonfigurovaný v `ConfigureProductionServices`).

[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=7,8,9,10,11,12&range=42-56)]

> [!NOTE]
> `ConnectionString` (A volitelně `SchemaName` a `TableName`) by měl obvykle být uloženy mimo zdrojového kódu (například UserSecrets), mohou obsahovat přihlašovací údaje.

## <a name="recommendations"></a>Doporučení

Při rozhodování, kdy zavedení `IDistributedCache` je pro aplikace, vyberte mezi Redis a SQL Server na základě vaší stávající infrastruktury a prostředí, vašim požadavkům na výkon a prostředí vašeho týmu. Pokud váš tým pohodlnější práce s Redis, je ideální volbou. Pokud váš tým upřednostňuje systému SQL Server, může být jisti, že implementace také. Všimněte si, že tradiční řešení ukládání do mezipaměti ukládá data v paměti, která umožňuje rychlé načítání dat. Měli uložit často používaná data do mezipaměti a uložení celého datového v trvalém úložišti back-end, jako je například SQL Server nebo úložiště Azure. Mezipaměť redis systému je řešení ukládání do mezipaměti, která umožňuje vysoké prostupnosti a nízké latence porovnání mezipaměti SQL.

## <a name="additional-resources"></a>Další zdroje

* [Redis Cache v Azure](https://azure.microsoft.com/documentation/services/redis-cache/)
* [Databáze SQL v Azure](https://azure.microsoft.com/documentation/services/sql-database/)
* [Ukládání do mezipaměti webového serveru](xref:performance/caching/memory)
* [Detekovat změny s tokeny změn](xref:fundamentals/primitives/change-tokens)
* [Ukládání odpovědí do mezipaměti](xref:performance/caching/response)
* [Middleware pro ukládání odpovědí do mezipaměti](xref:performance/caching/middleware)
* [Uložení pomocné rutiny značky do mezipaměti](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Pomocná rutina značek v distribuované mezipaměti](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
