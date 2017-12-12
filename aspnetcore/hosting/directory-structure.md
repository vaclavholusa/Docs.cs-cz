---
title: "Struktura adresářů ASP.NET Core"
author: guardrex
description: "Struktura adresářů publikované aplikace ASP.NET Core."
keywords: "ASP.NET Core, struktura adresářů"
ms.author: riande
manager: wpickett
ms.date: 03/15/2017
ms.topic: article
ms.assetid: e55eb131-d42e-4bf6-b130-fd626082243c
ms.technology: aspnet
ms.prod: asp.net-core
uid: hosting/directory-structure
ms.openlocfilehash: 60797bff85a44dd10caad4aabc109ee12dedfe61
ms.sourcegitcommit: 7efdc4b6025ad70c15c26bf7451c3c0411123794
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/02/2017
---
# <a name="directory-structure-of-published-aspnet-core-apps"></a>Struktura adresářů publikované aplikace ASP.NET Core

Podle [Luke Latham](https://github.com/guardrex)

V ASP.NET Core, adresář aplikace *publikování*, se skládá z souborů aplikace, konfigurační soubory, statické prostředky, balíčky a modul runtime (pro samostatný aplikace). Toto je stejného adresářové struktury jako předchozí verze technologie ASP.NET, kde bude celá aplikace umístěným uvnitř kořenové složky webu.

| Typ aplikace | Struktura adresářů |
| --- | --- |
| Nasazení závislé na Framework | <ul><li>publikování\*<ul><li>protokoly\* (Pokud je součástí publishOptions)</li><li>odolný systém souborů\*</li><li>Moduly runtime\*</li><li>Zobrazení\* (Pokud je součástí publishOptions)</li><li>Wwwroot\* (Pokud je součástí publishOptions)</li><li>soubory .dll</li><li>MyApp.deps.JSON</li><li>MyApp.dll</li><li>MyApp.pdb</li><li>Moje aplikace. PrecompiledViews.dll (Pokud předkompilace zobrazení syntaxe Razor)</li><li>Moje aplikace. PrecompiledViews.pdb (Pokud předkompilace zobrazení syntaxe Razor)</li><li>MyApp.runtimeconfig.JSON</li><li>soubor Web.config (Pokud je součástí publishOptions)</li></ul></li></ul> |
| Samostatná nasazení | <ul><li>publikování\*<ul><li>protokoly\* (Pokud je součástí publishOptions)</li><li>odolný systém souborů\*</li><li>Zobrazení\* (Pokud je součástí publishOptions)</li><li>Wwwroot\* (Pokud je součástí publishOptions)</li><li>soubory .dll</li><li>MyApp.deps.JSON</li><li>MyApp.exe</li><li>MyApp.pdb</li><li>Moje aplikace. PrecompiledViews.dll (Pokud předkompilace zobrazení syntaxe Razor)</li><li>Moje aplikace. PrecompiledViews.pdb (Pokud předkompilace zobrazení syntaxe Razor)</li><li>MyApp.runtimeconfig.JSON</li><li>soubor Web.config (Pokud je součástí publishOptions)</li></ul></li></ul> |
\*Určuje adresář

Obsah *publikování* představuje adresář *obsahu kořenovou cestu*, také zavolat *základní cesty aplikace*, nasazení. Jakýkoli název je uveden *publikování* directory v nasazení, její umístění slouží jako serveru fyzickou cestu k hostované aplikace. *Wwwroot* adresáře, pokud existuje, obsahuje pouze statické prostředky. *Protokoly* adresář může být součástí nasazení ve vytváření projektu a přidání `<Target>` element uvedené níže, aby vaše *.csproj* souboru nebo fyzicky vytváření adresáře na Server.

```xml
<Target Name="CreateLogsFolder" AfterTargets="Publish">
  <MakeDir Directories="$(PublishDir)Logs" 
           Condition="!Exists('$(PublishDir)Logs')" />
  <WriteLinesToFile File="$(PublishDir)Logs\.log" 
                    Lines="Generated file" 
                    Overwrite="True" 
                    Condition="!Exists('$(PublishDir)Logs\.log')" />
</Target>
```

`<MakeDir>` Element vytvoří prázdnou *protokoly* složky v publikované výstup. Element používá `PublishDir` vlastnosti k určení cílové umístění pro vytvoření složky. Několik metod nasazení, jako je nasazení webu, přeskočte prázdné složky během nasazení. `<WriteLinesToFile>` Element generuje soubor v *protokoly* složky, která zaručí nasazení složky na server. Všimněte si, že vytvoření složky může nezdaří, pokud pracovní proces nemá oprávnění k zápisu do cílové složky.

Adresář nasazení vyžaduje oprávnění ke čtení nebo spouštění při *protokoly* directory vyžaduje oprávnění pro čtení a zápis. Další adresáře, kam budou zapsány prostředky vyžadují oprávnění pro čtení a zápis.
