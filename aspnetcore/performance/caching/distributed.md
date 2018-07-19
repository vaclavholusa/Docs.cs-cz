---
title: Práce s distribuovanou mezipamětí v ASP.NET Core
author: ardalis
description: Zjistěte, jak používat ASP.NET Core distribuované ukládání do mezipaměti ke zlepšení výkonu aplikací a škálovatelnost, obzvláště v prostředí cloudu nebo serveru farmy.
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2017
uid: performance/caching/distributed
ms.openlocfilehash: 9c41a6e008045231bd2e1c1f53a9161e11daafa9
ms.sourcegitcommit: cb0c27fa0184f954fce591d417e6ab2a51d8bb22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/18/2018
ms.locfileid: "39123837"
---
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a>Práce s distribuovanou mezipamětí v ASP.NET Core

Podle [Steve Smith](https://ardalis.com/)

Distribuované mezipaměti může zlepšit výkon a škálovatelnost aplikací ASP.NET Core, zvláště když jsou hostované v cloudu nebo v serverové farmě.

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-a-distributed-cache"></a>Co je to distribuovaná mezipaměť

Distribuované mezipaměti je sdílen více serverů aplikace (viz [mezipaměti Základy](memory.md#caching-basics)). Informace v mezipaměti nejsou uloženy v paměti jednotlivých webových serverů a data uložená v mezipaměti je k dispozici pro všechny servery aplikace. To poskytuje několik výhod:

1. Data uložená v mezipaměti je přeměnit na všech webových serverech. Odlišné výsledky v závislosti na tom, které web server zpracovává jeho žádost se uživatelům nezobrazuje

2. Data uložená v mezipaměti odolává web server se restartuje a nasazení. Jednotlivé webové servery můžete odebrat nebo přidány bez dopadu na mezipaměť.

3. Zdrojového úložiště dat má menší počet požadavků provedených na jeho (než s více mezipaměti v paměti nebo žádnou instanci mezipaměti vůbec).

> [!NOTE]
> Pokud používáte SQL Server distribuované mezipaměti, některé z těchto výhod jsou pouze hodnotu true, pokud instance samostatné databáze se používá pro ukládání do mezipaměti než pro zdroj dat aplikace.

Stejně jako všechny mezipaměti distribuované mezipaměti výrazně reakce můžete zlepšit vaší aplikace, protože obvykle můžete načíst data z mezipaměti mnohem rychleji než relační databáze (nebo webovou službu).

Konfigurace mezipaměti závisí na konkrétní implementaci. Tento článek popisuje, jak nakonfigurovat i Redis a systému SQL Server distribuované ukládání do mezipaměti. Bez ohledu na to, kterou implementaci je vybrána, aplikace komunikuje s mezipamětí pomocí společného `IDistributedCache` rozhraní.

## <a name="the-idistributedcache-interface"></a>Rozhraní IDistributedCache

`IDistributedCache` Rozhraní zahrnuje synchronní a asynchronní metody. Rozhraní umožňuje položky, které chcete přidat, načíst a odebrat z implementace distribuované mezipaměti. `IDistributedCache` Rozhraní zahrnuje následující metody:

**GET, GetAsync**

Vezme s klíčem řetězce a načte položku v mezipaměti jako `byte[]` Pokud nalézt v mezipaměti.

**Sada SetAsync**

Přidá položku (jako `byte[]`) do mezipaměti pomocí řetězce klíče.

**Aktualizace, RefreshAsync**

Aktualizuje položku v mezipaměti na základě jeho klíče resetuje jeho časový limit klouzavé vypršení platnosti (pokud existuje).

**Odebrat, asynchronně odebere**

Odebere položku mezipaměti na základě jeho klíče.

Použít `IDistributedCache` rozhraní:

   1. Přidání požadovaných balíčků NuGet do souboru projektu.

   2. Nakonfigurujte konkrétní implementaci `IDistributedCache` ve vaší `Startup` třídy `ConfigureServices` metoda a přidejte ho do kontejneru existuje.

   3. Z aplikace [Middleware](xref:fundamentals/middleware/index) nebo třídy kontroleru MVC, požádat o instanci `IDistributedCache` z konstruktoru. Instance budou poskytovat [injektáž závislostí](../../fundamentals/dependency-injection.md) (DI).

> [!NOTE]
> Není nutné používat jednotlivý prvek nebo rozsah doba života pro `IDistributedCache` instance (nejméně pro předdefinované implementace). Můžete také vytvořit instanci, bez ohledu na to může být nutné jednu (namísto použití [injektáž závislostí](../../fundamentals/dependency-injection.md)), ale váš kód může být obtížnější testovat a je v rozporu [explicitní závislosti Princip](http://deviq.com/explicit-dependencies-principle/).

Následující příklad ukazuje, jak použít instanci `IDistributedCache` v jednoduché middleware součásti:

[!code-csharp[](distributed/sample/src/DistCacheSample/StartTimeHeader.cs)]

Ve výše uvedeném kódu je hodnota uložená v mezipaměti číst, ale nikdy zapsány. V této ukázce je hodnota nastavena pouze při spuštění serveru a nemění. V případě více serverů v spustit nejnovější server přepíše jakékoli předchozí hodnoty, které byly nastavené zásadami ostatní servery. `Get` a `Set` metody používají `byte[]` typu. Proto se řetězcová hodnota musí být převeden pomocí `Encoding.UTF8.GetString` (pro `Get`) a `Encoding.UTF8.GetBytes` (pro `Set`).

Následující kód z *Startup.cs* ukazuje, nastaví se hodnota:

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet1)]

Protože `IDistributedCache` je nakonfigurovaný v `ConfigureServices` metoda, je k dispozici na `Configure` metodu jako parametr. Přidání jako parametr vám umožní nakonfigurované instance má být poskytnuta prostřednictvím DI.

## <a name="using-a-redis-distributed-cache"></a>Použití Redis distribuované mezipaměti

[Redis](https://redis.io/) je úložiště otevřít zdroj dat v paměti, což se často používá jako distribuované mezipaměti. Můžete používat místně, a můžete nakonfigurovat [Azure Redis Cache](https://azure.microsoft.com/services/cache/) pro vaše aplikace ASP.NET Core hostovaných v Azure. Nakonfiguruje aplikaci ASP.NET Core pomocí implementace mezipaměti `RedisDistributedCache` instance.

Mezipaměti Redis vyžaduje [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis/)

Konfigurace Redis implementace v `ConfigureServices` a k němu přístup v kódu vaší aplikace můžete si vyžádat instance `IDistributedCache` (viz výše uvedený kód).

Ve vzorovém kódu `RedisCache` implementace se používá, když je server nakonfigurovaný pro `Staging` prostředí. Proto `ConfigureStagingServices` konfiguruje metodu `RedisCache`:

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet2)]

K instalaci Redis na místním počítači, nainstalujte chocolatey balíček [ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/) a spusťte `redis-server` z příkazového řádku.

## <a name="using-a-sql-server-distributed-cache"></a>SQL Server pomocí distribuované mezipaměti

Implementace SqlServerCache umožňuje distribuované mezipaměti pro použití jiné databáze systému SQL Server jako svůj záložní úložiště. Vytvoření SQL serveru tabulku můžete použít nástroj mezipaměti sql, nástroj vytvoří tabulku s názvem a schéma, které zadáte.

::: moniker range="< aspnetcore-2.1"

Přidat `SqlConfig.Tools` k `<ItemGroup>` element soubor projektu a spustit `dotnet restore`.

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools" 
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

Otestujte SqlConfig.Tools spuštěním následujícího příkazu:

```console
dotnet sql-cache create --help
```

SqlConfig.Tools zobrazí využití, možnosti a nápovědy k příkazům.

Vytvoření tabulky v SQL serveru spuštěním `sql-cache create` příkaz:

```console
dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
Table and index were created successfully.
```

Vytvořené tabulky má následující schéma:

![Tabulka mezipaměti systému SQL Server](distributed/_static/SqlServerCacheTable.png)

Stejně jako všechny implementace mezipaměti by měla vaše aplikace získávat a nastavovat hodnoty mezipaměti pomocí instance `IDistributedCache`, nikoli `SqlServerCache`. Implementuje vzorek `SqlServerCache` v produkčním prostředí (abyste je nakonfigurován v `ConfigureProductionServices`).

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet3)]

> [!NOTE]
> `ConnectionString` (A volitelně také `SchemaName` a `TableName`) by měl obvykle být uloženy mimo vaši správy zdrojových kódů (například UserSecrets), mohou obsahovat přihlašovací údaje.

## <a name="recommendations"></a>Doporučení

Při rozhodování o tom, která implementace `IDistributedCache` je přímo pro vaši aplikaci, zvolte mezi Redis a systému SQL Server na základě vaší stávající infrastruktury a prostředí, vašim požadavkům na výkon a prostředí pro váš tým. Pokud váš tým je pohodlnější, práce s využitím Redisu, je skvělou volbou. Pokud váš tým přednost systému SQL Server, máte jistotu, že v této implementaci stejně. Všimněte si, že tradiční řešení ukládání do mezipaměti ukládá data v paměti, která umožňuje rychlé načítání dat. Měli byste ukládání běžně používaných dat v mezipaměti a uložit veškerá data v trvalém úložišti back-end, jako je například SQL Server nebo služby Azure Storage. Redis Cache je řešení ukládání do mezipaměti, která poskytuje vysokou propustností a nízkou latenci v porovnání s mezipaměti SQL.

## <a name="additional-resources"></a>Další zdroje

* [Redis Cache v Azure](https://azure.microsoft.com/documentation/services/redis-cache/)
* [SQL Database v Azure](https://azure.microsoft.com/documentation/services/sql-database/)
* <xref:performance/caching/memory>
* <xref:fundamentals/primitives/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
