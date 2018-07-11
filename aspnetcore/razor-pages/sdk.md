---
title: ASP.NET Core Razor SDK
author: Rick-Anderson
description: Zjistěte, jak v ASP.NET Core Razor Pages díky psaní kódu zaměřená na stránce scénáře jednodušší a produktivnější než pomocí MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 04/12/2018
uid: razor-pages/sdk
ms.openlocfilehash: 4dd48b13272ed847ff83e8826e10678d732b53f9
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38215889"
---
# <a name="aspnet-core-razor-sdk"></a>ASP.NET Core Razor SDK

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [](~/includes/2.1-SDK.md)] Zahrnuje `Microsoft.NET.Sdk.Razor` sady MSBuild SDK (Razor SDK). Razor SDK:

* Standardizuje prostředí týkající se vytváření, balení a publikování projektů, které obsahují [Razor](xref:mvc/views/razor) souborů pro projekty ASP.NET Core MVC.
* Obsahuje sadu předdefinovaných cílů, vlastností a položek, které umožňují přizpůsobení kompilace souborech Razor.

## <a name="prerequisites"></a>Požadavky

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="using-the-razor-sdk"></a>Pomocí syntaxe Razor SDK

Většina webových aplikací není potřeba výslovně odkazují na sadu SDK Razor. 

Použití sady SDK Razor k vytvoření knihovny tříd obsahující zobrazení syntaxe Razor nebo stránky Razor:

* Použití `Microsoft.NET.Sdk.Razor` místo `Microsoft.NET.Sdk`:
```xml
<Project SDK="Microsoft.NET.Sdk.Razor">
  ...
</Project>
```

* Obvykle odkaz na balíček pro `Microsoft.AspNetCore.Mvc` je vyžadována v další závislosti, které jsou potřebné k sestavení a kompilace Razor Pages a zobrazeními Razor. Minimálně je potřeba přidat odkazy na balíčky do vašeho projektu:

    * `Microsoft.AspNetCore.Razor.Design` 
    * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
    
 Předchozí balíčky jsou součástí `Microsoft.AspNetCore.Mvc`. Následující kód ukazuje základní *.csproj* soubor, který používá sada Razor SDK k sestavení souborů Razor pro aplikace ASP.NET Core Razor Pages:
    
 [!code-xml[Main](sdk/sample/RazorSDK.csproj)]

### <a name="properties"></a>Vlastnosti

Následující vlastnosti řídit chování sady SDK pro syntaxi Razor jako součást sestavení projektu:

* `RazorCompileOnBuild` : Pokud `true`, zkompiluje a vysílá Razor sestavení jako součást sestavení projektu. Výchozí hodnota je `true`.
* `RazorCompileOnPublish` : Pokud `true`, zkompiluje a vysílá Razor sestavení jako součást publikování projektu. Výchozí hodnota je `true`.

Následující vlastnosti a položky se používají ke konfiguraci vstupů a výstupní sada Razor SDK:

| Položky                                         | Popis                                                                   |
| ------------                                  | -------------                                                                 |
| RazorGenerate                                 | Položka elementy (*.cshtml* soubory), které jsou vstupy do cíle generování kódu. |
| RazorCompile                                  | Položka elementy (soubory .cs), které jsou vstupy do cíle kompilace Razor. Pomocí této ItemGroup můžete určit další soubory se zkompiluje do sestavení Razor. |
| RazorTargetAssemblyAttribute                  | Položka prvků, které slouží ke kódu generovat atributy pro sestavení Razor. Příklad:  <br />`<RazorAssemblyAttribute ` <br />  `Include="System.Reflection.AssemblyMetadataAttribute"`<br />`  _Parameter1="BuildSource" _Parameter2="https://docs.asp.net/">` |
| RazorEmbeddedResource                         | Elementy položky přidané jako vložené prostředky do generovaného sestavení Razor |

| Vlastnost                                      | Popis                                                                   |
| ------------                                  | -------------                                                                 |
| RazorTargetName                               | Název souboru (bez přípony) sestavení vytvořené metodou Razor. | 
| RazorOutputPath                               | Výstupní adresář Razor.                                      |
| RazorCompileToolset                           | Slouží k určení sady nástrojů použitý k vytvoření sestavení Razor. Platné hodnoty jsou `Implicit`,, a `PrecompilationTool`. |
| EnableDefaultContentItems                     | Když `true`, obsahuje určité typy souborů, jako například *.cshtml* soubory jako obsah v projektu. K dispozici prostřednictvím Microsoft.NET.Sdk.Web, také zahrnuje všechny soubory pod *wwwroot*a konfigurační soubory.         |
| EnableDefaultRazorGenerateItems               | Když `true`, zahrnuje *.cshtml* souborů z `Content` položky v `RazorGenerate` položky. |
| GenerateRazorTargetAssemblyInfo               | Když `true`, generuje *.cs* soubor, který obsahuje atributy určené `RazorAssemblyAttribute` a zahrnuje výstupu kompilace. |
| EnableDefaultRazorTargetAssemblyInfoAttributes | Když `true`, přidá výchozí sadu atributů sestavení, které mají `RazorAssemblyAttribute`. |
| CopyRazorGenerateFilesToPublishDirectory       | Když `true`, zkopíruje položky RazorGenerate (*.cshtml*) soubory do adresáře publikovat. Obvykle není třeba souborech Razor pro publikovanou aplikaci, pokud se účastní v sestavování v době sestavení nebo publikovat čas. Výchozí hodnota je `false`. |
| CopyRefAssembliesToPublishDirectory            | Když `true`, zkopírovat odkaz na sestavení položky do adresáře publikovat. Obvykle se referenční sestavení potřebná pro publikovanou aplikaci dojde v okamžiku sestavení nebo publikovat čas kompilace Razor. Nastavte na `true`, pokud vaše publikované aplikace vyžaduje kompilace modulu runtime, například upraví soubory cshtml za běhu, nebo používá vložené zobrazení. Výchozí hodnota je `false`. |
| IncludeRazorContentInPack                      | Když `true`, všechny položky obsahu Razor (*.cshtml* soubory) se označí k zařazení vygenerovaný balíček NuGet. Výchozí hodnota je `false`. |
| EmbedRazorGenerateSources | Když `true`, přidá RazorGenerate (*.cshtml*) položky jako vložené soubory do generovaného sestavení Razor. Výchozí hodnota je `false`. |
| UseRazorBuildServer                           | Když `true`, využívá proces serveru trvalé sestavení přesměrovat pracovní generování kódu. Výchozí hodnota je hodnota `UseSharedCompilation`. |

### <a name="targets"></a>Cíle
Sada Razor SDK definuje dva hlavní cíle:

* `RazorGenerate` – Generuje kód *.cs* soubory z RazorGenerate položky elementy. Použití `RazorGenerateDependsOn` vlastnosti a určit další cílí, který můžete spustit před nebo po tento cíl.
* `RazorCompile` -Zkompiluje generované *.cs* soubory v sestavení Razor. Použití `RazorCompileDependsOn` zadat další cíle, které můžete spustit před nebo po tento cíl.

### <a name="runtime-compilation-of-razor-views"></a>Kompilace modulu runtime zobrazení Razor

* Ve výchozím nastavení nebude sada Razor SDK publikovat referenční sestavení, které jsou nutné k provedení kompilace modulu runtime. Výsledkem je selhání kompilace při aplikační model využívá kompilace modulu runtime&mdash;například aplikace používá vložený zobrazení nebo změny zobrazení po publikování aplikace. Nastavte `CopyRefAssembliesToPublishDirectory` k `true` můžete pokračovat v publikování referenční sestavení.

* Pro webové aplikace, ujistěte se vaše aplikace cílí `Microsoft.NET.Sdk.Web` SDK.
