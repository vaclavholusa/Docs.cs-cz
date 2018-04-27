---
title: Jádro ASP.NET Razor SDK
author: Rick-Anderson
description: Zjistěte, jak stránky Razor v ASP.NET Core Díky kódování zaměřené na stránce scénáře jednodušší a zvýšit produktivitu než použití MVC.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/12/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: mvc/razor-pages/sdk
ms.openlocfilehash: 2cbebb12ccd1098e1950aa7eeb22fab4ffc689e6
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/18/2018
---
# <a name="aspnet-core-razor-sdk"></a>Jádro ASP.NET Razor SDK

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[](~/includes/2.1.md)]

[!INCLUDE [](~/includes/2.1-SDK.md)] Zahrnuje `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK). Razor SDK:

* Standardizuje zkušeností v oblasti vytváření, balení a publikování projekty obsahující [Razor](xref:mvc/views/razor) soubory pro projekty založené na ASP.NET MVC jádra.
* Obsahuje sadu předdefinovaných cílů, vlastností a položek, které umožňují vlastní nastavení kompilace soubory Razor.

## <a name="prerequisites"></a>Požadavky

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="using-the-razor-sdk"></a>Pomocí syntaxe Razor SDK

Většina webových aplikací není nutné výslovně odkázat na sadu SDK Razor. 

Použití sady SDK Razor k vytvoření knihovny tříd obsahující zobrazení syntaxe Razor nebo stránky Razor:

* Použití `Microsoft.NET.Sdk.Razor` místo `Microsoft.NET.Sdk`:
```xml
<Project SDK="Microsoft.NET.Sdk.Razor">
  ...
</Project>
```

* Obvykle balíček odkaz na `Microsoft.AspNetCore.Mvc` je vyžadována v další závislosti, které jsou potřebné k vytváření a kompilace stránky Razor a zobrazení syntaxe Razor. Minimálně je potřeba přidat odkazy na balíčky do projektu:

    * `Microsoft.AspNetCore.Razor.Design` 
    * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
    
 Předchozí balíčky jsou součástí `Microsoft.AspNetCore.Mvc`. Následující kód ukazuje základní *.csproj* soubor, který používá syntaxi Razor SDK k sestavení souborů syntaxe Razor pro aplikaci ASP.NET Core Razor stránky:
    
 [!code-xml[Main](sdk/sample/RazorSDK.csproj)]

### <a name="properties"></a>Vlastnosti

Následující vlastnosti řízení chování sady SDK Razor jako součást sestavení projektu:

* `RazorCompileOnBuild` : Pokud `true`, zkompiluje a vysílá Razor sestavení jako součást sestavení projektu. Použije se výchozí hodnota `true`.
* `RazorCompileOnPublish` : Pokud `true`, zkompiluje a vysílá Razor sestavení jako součást publikování projektu. Použije se výchozí hodnota `true`.

Následující vlastnosti a položky se používají ke konfiguraci vstupy a výstupní sady SDK Razor:

| Položky                                         | Popis                                                                   |
| ------------                                  | -------------                                                                 |
| RazorGenerate                                 | Položka elementy (*.cshtml* soubory), které jsou vstupy k cílům generování kódu. |
| RazorCompile                                  | Položka elementům (soubory .cs) vstupy k cílům kompilace Razor. Pomocí této ItemGroup můžete určit další soubory, které sestavují v sestavení Razor. |
| RazorAssemblyAttribute                        | Položka prvky používané ke kódu generovat atributy pro sestavení Razor. Příklad:  <br />`<RazorAssemblyAttribute ` <br />  `Include="System.Reflection.AssemblyMetadataAttribute"`<br />`  _Parameter1="BuildSource" _Parameter2="https://docs.asp.net/">` |
| RazorEmbeddedResource                         | Položka elementy přidané jako vložené prostředky do vygenerované sestavení Razor |

| Vlastnost                                      | Popis                                                                   |
| ------------                                  | -------------                                                                 |
| RazorTargetName                               | Název souboru (bez přípony) sestavení vyprodukované Razor. | 
| RazorOutputPath                               | Výstupní adresář Razor.                                      |
| RazorCompileToolset                           | Použít k určení sady nástrojů sloužící k vytvoření sestavení Razor. Platné hodnoty jsou `Implicit`,, a `PrecompilationTool`. |
| EnableDefaultContentItems                     | Když `true`, obsahuje určité typy souborů, jako například *.cshtml* soubory jako obsah v projektu. Když se odkazuje prostřednictvím Microsoft.NET.Sdk.Web, také zahrnuje všechny soubory pod *wwwroot*a konfigurační soubory.         |
| EnableDefaultRazorGenerateItems               | Když `true`, zahrnuje *.cshtml* souborů z `Content` položky v `RazorGenerate` položky. |
| GenerateRazorTargetAssemblyInfo               | Když `true`, generuje *.cs* obsahující atributy určeného `RazorAssemblyAttribute` a zahrnuje ve výstupu kompilace. |
| EnableDefaultRazorTargetAssemblyInfoAttributes | Když `true`, přidá výchozí sadu atributů sestavení do `RazorAssemblyAttribute`. |
| CopyRazorGenerateFilesToPublishDirectory       | Když `true`, zkopíruje položky RazorGenerate (*.cshtml*) soubory do adresáře publikovat. Soubory Razor obvykle nejsou potřebné pro publikovanou aplikaci, pokud jejich součástí v kompilaci v době sestavení nebo publikování čas. Použije se výchozí hodnota `false`. |
| CopyRefAssembliesToPublishDirectory            | Když `true`, Kopírovat odkaz na sestavení položky k adresáři pro publikování. Referenční sestavení nejsou obvykle případě Razor kompilace v době sestavení nebo publikování čas potřebné pro publikované aplikace. Nastavte na `true`, pokud k publikované aplikaci vyžaduje kompilace runtime, například upraví soubory cshtml za běhu, případně používá vložené zobrazení. Použije se výchozí hodnota `false`. |
| IncludeRazorContentInPack                      | Když `true`, všechny položky obsahu Razor (*.cshtml* soubory) budou označeny pro zařazení do vygenerovaný balíček NuGet. Použije se výchozí hodnota `false`. |
| EmbedRazorGenerateSources | Když `true`, přidá RazorGenerate (*.cshtml*) položky jako vložené soubory generované sestavení Razor. Použije se výchozí hodnota `false`. |
| UseRazorBuildServer                           | Když `true`, využívá proces trvalé sestavení serveru k přesměrování zpracování úloh pracovní generování kódu. Výchozí hodnota `UseSharedCompilation`. |

### <a name="targets"></a>Cíle
Sada SDK Razor definuje dva hlavní cíle:

* `RazorGenerate` – Generuje kód *.cs* soubory z RazorGenerate položky elementy. Použití `RazorGenerateDependsOn` vlastnosti a určit další cílem, který můžete spustit před nebo po tento cíl.
* `RazorCompile` -Zkompiluje generované *.cs* soubory do sestavení Razor. Použití `RazorCompileDependsOn` k určení dalších cíle, které můžete spustit před nebo po tento cíl.