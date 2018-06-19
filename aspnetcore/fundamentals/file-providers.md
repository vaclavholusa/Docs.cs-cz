---
title: Soubor zprostředkovatele v ASP.NET Core
author: ardalis
description: Zjistěte, jak ASP.NET Core abstrahuje přístupu k systému souborů prostřednictvím poskytovatelů souboru.
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/file-providers
ms.openlocfilehash: cdbffdadd9616fe941809d67dc2c0bbd52149561
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
ms.locfileid: "29724568"
---
# <a name="file-providers-in-aspnet-core"></a>Soubor zprostředkovatele v ASP.NET Core

Podle [Steve Smith](https://ardalis.com/)

ASP.NET Core abstrahuje přístupu k systému souborů prostřednictvím poskytovatelů souboru.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="file-provider-abstractions"></a>Abstrakce zprostředkovatele souboru

Soubor poskytovatelé jsou abstrakci přes systémy souborů. Hlavní rozhraní je `IFileProvider`. `IFileProvider` zpřístupní metody pro získání informací o souboru (`IFileInfo`), informace o adresáři (`IDirectoryContents`) a k nastavení oznámení o změnách (pomocí `IChangeToken`).

`IFileInfo` poskytuje metody a vlastnosti o jednotlivé soubory nebo adresáře. Má dvě logická hodnota vlastnosti `Exists` a `IsDirectory`, a také vlastnosti popisující souboru `Name`, `Length` (v bajtech), a `LastModified` datum. Můžete si přečíst ze souboru pomocí jeho `CreateReadStream` metoda.

## <a name="file-provider-implementations"></a>Implementace zprostředkovatele souboru

Tři implementace `IFileProvider` jsou k dispozici: fyzické, vložených a složené. Fyzické poskytovatele používá pro přístup k souborům konkrétního systému. Vložené zprostředkovatele se používá pro přístup k souborům, které jsou součástí sestavení. Složené zprostředkovatele slouží k poskytování kombinovaný přístup k souborů a adresářů z jednoho nebo více poskytovatelů.

### <a name="physicalfileprovider"></a>PhysicalFileProvider

`PhysicalFileProvider` Poskytuje přístup k fyzické systému souborů. Zabalí ho `System.IO.File` typu (pro fyzické zprostředkovatele), oborů všechny cesty k adresáři a její podřízené položky. Tento obor omezuje přístup na určité adresáře a její podřízené položky, brání přístupu k systému souborů mimo tuto hranici. Při vytváření instance tohoto zprostředkovatele, je nutné zadat s cestu k adresáři, který slouží jako základní cesta pro všechny požadavky provedené pro tohoto zprostředkovatele (a která omezuje přístup mimo tuto cestu). V aplikaci ASP.NET Core, můžete vytvořit instanci `PhysicalFileProvider` zprostředkovatele přímo, nebo můžete požádat o `IFileProvider` v řadiči nebo konstruktor služby prostřednictvím [vkládání závislostí](dependency-injection.md). Pozdější přístup obvykle předá flexibilnější a možností intenzivního testování řešení.

Následující ukázka ukazuje, jak vytvořit `PhysicalFileProvider`.


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

Můžete iteraci v rámci jeho obsah adresáře nebo získat informace o konkrétní soubor tím, že poskytuje je její částí.

K vyžádání zprostředkovatele z řadič, zadejte v konstruktoru kontroleru a přiřaďte ho pro místní pole. Použijte místní instanci z vaší metody akce:

[!code-csharp[](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

Pak vytvořte poskytovatele v dané aplikaci `Startup` třídy:

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

V *Index.cshtml* zobrazit, iteraci v rámci `IDirectoryContents` poskytuje:

[!code-html[](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

Výsledek:

![Soubor zprostředkovatele ukázkové aplikace výpis fyzické soubory a složky](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a>EmbeddedFileProvider

`EmbeddedFileProvider` Se používá pro přístup k souborům, které jsou součástí sestavení. V rozhraní .NET Core, vložíte do sestavení se soubory `<EmbeddedResource>` element v *.csproj* souboru:

[!code-json[](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

Můžete použít [expanze názvů vzory](#globbing-patterns) při zadávání soubory pro vložení do sestavení. Tyto vzory použít tak, aby odpovídaly jeden nebo více souborů.

> [!NOTE]
> Pravděpodobně že by někdy budete chtít ve skutečnosti vložení každý soubor .js ve vašem projektu v jeho sestavení; ukázka výše je ukázkový pouze pro účely.

Při vytváření `EmbeddedFileProvider`, předat sestavení, zobrazí se informace pro jeho konstruktor.

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

Demonstruje postup vytvoření výše uvedeném fragmentu `EmbeddedFileProvider` s přístupem k aktuálně prováděné sestavení.

Aktualizace ukázková aplikace sloužící `EmbeddedFileProvider` výsledkem následující výstup:

![Soubor zprostředkovatele ukázkovou aplikaci seznam vložených souborů](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> Vložené prostředky Nevystavujte adresáře. Místo toho je cesta k prostředek (prostřednictvím svého oboru názvů) vložených v jeho název souboru pomocí `.` oddělovačů.

> [!TIP]
> `EmbeddedFileProvider` Konstruktor přijímá volitelný `baseNamespace` parametr. Určení to bude obor volání `GetDirectoryContents` na tyto prostředky v rámci zadaného oboru názvů.

### <a name="compositefileprovider"></a>CompositeFileProvider

`CompositeFileProvider` Kombinuje `IFileProvider` instancí vystavení jednoho rozhraní pro práci se soubory z více poskytovatelů. Při vytváření `CompositeFileProvider`, předáte jeden nebo více `IFileProvider` instance jeho konstruktoru:

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

Aktualizace ukázková aplikace používat `CompositeFileProvider` která obsahuje oba fyzické a embedded zprostředkovatele dříve nakonfigurován, výsledkem následující výstup:

![Soubor zprostředkovatele ukázkovou aplikaci výpis fyzické soubory a složky a vložené soubory](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a>Sledování změn

`IFileProvider` `Watch` Metoda poskytuje způsob, jak sledovat jeden nebo více souborů či adresářů pro změny. Tato metoda přijímá řetězec cesty, které můžete použít [expanze názvů vzory](#globbing-patterns) zadat více souborů a vrátí `IChangeToken`. Tento token zpřístupní `HasChanged` vlastnost, která může být prověřovány, a `RegisterChangeCallback` metoda, která je volána při zjištění změny na zadané cestě řetězec. Všimněte si, že každý token změnu pouze volá jeho přidružené zpětného volání v reakci na jediné změny. Chcete-li povolit konstantní monitorování, můžete použít `TaskCompletionSource` jak je uvedeno níže, nebo znovu vytvořte `IChangeToken` instancí v reakci na změny.

V tomto článku ukázce je konzolová aplikace nakonfigurován k zobrazení zprávy změně textového souboru je:

[!code-csharp[](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

Výsledek po uložení souboru několikrát:

![Příkazové okno po provádění dotnet. Spusťte ukazuje na soubor quotes.txt změny monitorování aplikací a pětkrát došlo ke změně souboru.](file-providers/_static/watch-console.png)

> [!NOTE]
> Některé systémy souborů, jako je například Docker kontejnery a sdílené síťové složky a nemusí spolehlivě odeslat oznámení o změnách. Nastavte `DOTNET_USE_POLLINGFILEWATCHER` proměnnou prostředí `1` nebo `true` k dotazování systému souborů pro změny každé 4 sekundy.

## <a name="globbing-patterns"></a>Vzory expanze názvů

Systémové cesty k souborům použít zástupné znaky názvem *expanze názvů vzory*. Tyto jednoduché vzory slouží k určení skupiny souborů. Dva zástupné znaky jsou `*` a `**`.

**`*`**

   Odpovídá nic na aktuální úrovni složky, nebo libovolný název souboru nebo libovolnou příponou. Shoduje se ukončila příkazem `/` a `.` znaků v cestě k souboru.

<strong><code>**</code></strong>

   Odpovídá všemu napříč více úrovní adresáře. Slouží k rekurzivnímu odpovídat mnoho souborů v rámci hierarchie adresářů.

### <a name="globbing-pattern-examples"></a>Příklady vzor expanze názvů

**`directory/file.txt`**

   Odpovídá konkrétní soubor v konkrétní adresář.

**<code>directory/*.txt</code>**

   Odpovídá všechny soubory s `.txt` rozšíření v konkrétní adresář.

**`directory/*/bower.json`**

   Odpovídá všem `bower.json` soubory v adresářích přesně jednu úroveň níže `directory` adresáře.

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   Odpovídá všechny soubory s `.txt` nalezeno rozšíření kdekoli v části `directory` adresáře.

## <a name="file-provider-usage-in-aspnet-core"></a>Soubor zprostředkovatele využití v ASP.NET Core

Několik částí ASP.NET Core využívat souboru zprostředkovatele. `IHostingEnvironment` zpřístupní obsahu kořenové aplikace a webové kořenový jako `IFileProvider` typy. Middleware statické soubory používá zprostředkovatele souboru k vyhledání statické soubory. Syntaxe Razor umožňuje výrazně využívá `IFileProvider` v vyhledání zobrazení. Pro DotNet publikování funkce používá soubor zprostředkovatele a vzory expanze názvů určit soubory, které je nutné ji publikovat.

## <a name="recommendations-for-use-in-apps"></a>Doporučení pro použití v aplikacích

Pokud vaše aplikace ASP.NET Core vyžaduje přístupu k systému souborů, můžete požádat o instanci `IFileProvider` pomocí vkládání závislostí a pak použít její metody k provádění přístupu, jak je znázorněno v této ukázce. To umožňuje konfiguraci poskytovatele za jednou, když se aplikace spouští a snižuje počet typů implementace, které vaše aplikace vytvoří.
