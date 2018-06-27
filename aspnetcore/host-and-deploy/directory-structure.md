---
title: Struktura adresářů ASP.NET Core
author: guardrex
description: Další informace o struktura adresářů publikované aplikace ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 04/09/2018
uid: host-and-deploy/directory-structure
ms.openlocfilehash: 8e2693397f826d0e9a36ff52aa1d1d623b31043d
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960824"
---
# <a name="aspnet-core-directory-structure"></a>Struktura adresářů ASP.NET Core

Podle [Luke Latham](https://github.com/guardrex)

V ASP.NET Core, adresáři publikované aplikace *publikování*, se skládá z souborů aplikace, konfigurační soubory, statické prostředky, balíčky a modul runtime (pro [samostatná nasazení](/dotnet/core/deploying/#self-contained-deployments-scd)).


| Typ aplikace | Struktura adresářů |
| -------- | ------------------- |
| [Nasazení závislé na Framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li>Publikování&dagger;<ul><li>Protokoly&dagger; (volitelné, pokud se vyžaduje pro příjem protokoly stdout)</li><li>Zobrazení&dagger; (aplikace MVC; Pokud nejsou předkompilovaných zobrazení)</li><li>Stránky&dagger; (MVC nebo stránky Razor aplikace; Pokud nejsou předkompilovaných stránky)</li><li>Wwwroot&dagger;</li><li>*\.soubory knihoven DLL</li><li>\<název sestavení >. deps.json</li><li>\<název sestavení > .dll</li><li>\<název sestavení > pdb</li><li>\<název sestavení >. PrecompiledViews.dll</li><li>\<název sestavení >. PrecompiledViews.pdb</li><li>\<název sestavení >. runtimeconfig.json</li><li>soubor Web.config (nasazení služby IIS)</li></ul></li></ul> |
| [Samostatná nasazení](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>Publikování&dagger;<ul><li>Protokoly&dagger; (volitelné, pokud se vyžaduje pro příjem protokoly stdout)</li><li>odolný systém souborů&dagger;</li><li>Zobrazení&dagger; (aplikace MVC; Pokud nejsou předkompilovaných zobrazení)</li><li>Stránky&dagger; (MVC nebo stránky Razor aplikace; Pokud nejsou předkompilovaných stránky)</li><li>Wwwroot&dagger;</li><li>\*soubory .dll</li><li>\<název sestavení >. deps.json</li><li>\<název sestavení > .exe</li><li>\<název sestavení > pdb</li><li>\<název sestavení >. PrecompiledViews.dll</li><li>\<název sestavení >. PrecompiledViews.pdb</li><li>\<název sestavení >. runtimeconfig.json</li><li>soubor Web.config (nasazení služby IIS)</li></ul></li></ul> |

&dagger;Určuje adresář

*Publikování* představuje adresář *obsahu kořenovou cestu*, také zavolat *základní cesty aplikace*, nasazení. Jakýkoli název je uveden *publikování* adresáře aplikace nasazené na serveru, její umístění slouží jako serveru fyzickou cestu k hostované aplikace.

*Wwwroot* adresáře, pokud existuje, obsahuje pouze statické prostředky.

Stdout *protokoly* adresář můžete vytvořit pro nasazení pomocí jedné z následujících dvou přístupů:

* Přidejte následující `<Target>` element souboru projektu:

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

* Fyzicky vytvořit *protokoly* adresář na serveru v nasazení.

Adresář nasazení vyžaduje oprávnění pro čtení nebo spouštění. *Protokoly* directory vyžaduje oprávnění pro čtení a zápis. Další adresáře, kde se zapisují soubory vyžadují oprávnění pro čtení a zápis.
