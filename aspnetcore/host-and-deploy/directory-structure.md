---
title: Struktura adresářů ASP.NET Core
author: guardrex
description: V tématu strukturu adresáře publikovaných aplikací ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/directory-structure
ms.openlocfilehash: 2a6ee4fefcc6d23b1c893a40b7b1be9edfcf9732
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-core-directory-structure"></a>Struktura adresářů ASP.NET Core

Podle [Luke Latham](https://github.com/guardrex)

V ASP.NET Core, adresář aplikace *publikování*, se skládá z souborů aplikace, konfigurační soubory, statické prostředky, balíčky a modul runtime (pro samostatný aplikace).


|            Typ aplikace            |                                                                                                                                                                                                                                                     Struktura adresářů                                                                                                                                                                                                                                                      |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nasazení závislé na Framework | <ul><li>Publikování\*<ul><li>protokoly\* (Pokud je součástí publishOptions)</li><li>odolný systém souborů\*</li><li>Moduly runtime\*</li><li>Zobrazení\* (Pokud je součástí publishOptions)</li><li>Wwwroot\* (Pokud je součástí publishOptions)</li><li>soubory .dll</li><li>myapp.deps.json</li><li>myapp.dll</li><li>myapp.pdb</li><li>Moje aplikace. PrecompiledViews.dll (Pokud předkompilace zobrazení syntaxe Razor)</li><li>Moje aplikace. PrecompiledViews.pdb (Pokud předkompilace zobrazení syntaxe Razor)</li><li>myapp.runtimeconfig.json</li><li>soubor Web.config (Pokud je součástí publishOptions)</li></ul></li></ul> |
|   Samostatná nasazení    |          <ul><li>Publikování\*<ul><li>protokoly\* (Pokud je součástí publishOptions)</li><li>odolný systém souborů\*</li><li>Zobrazení\* (Pokud je součástí publishOptions)</li><li>Wwwroot\* (Pokud je součástí publishOptions)</li><li>soubory .dll</li><li>myapp.deps.json</li><li>myapp.exe</li><li>myapp.pdb</li><li>Moje aplikace. PrecompiledViews.dll (Pokud předkompilace zobrazení syntaxe Razor)</li><li>Moje aplikace. PrecompiledViews.pdb (Pokud předkompilace zobrazení syntaxe Razor)</li><li>myapp.runtimeconfig.json</li><li>soubor Web.config (Pokud je součástí publishOptions)</li></ul></li></ul>           |

\* Určuje adresář

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
