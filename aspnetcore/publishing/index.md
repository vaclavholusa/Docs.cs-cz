---
title: "Přehled hostování a nasazení - ASP.NET Core"
author: tdykstra
description: "Přehled o tom, jak nastavit hostitelská prostředí a nasazení aplikací ASP.NET Core k nim."
keywords: "Jádro ASP.NET"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: f0930c68-4d17-4748-adbf-801e17601eb6
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/index
ms.openlocfilehash: 0de459128426c4d027606951592b1fe3fdd24fd9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="hosting-and-deployment-overview-for-aspnet-core-apps"></a>Přehled hostování a nasazení pro aplikace ASP.NET Core

Zde jsou hlavní kroky při nasazení aplikace ASP.NET Core do hostitelského prostředí:

* Publikujte aplikaci do složky na hostitelském serveru.
* Nastavte správce procesu, který spustí aplikaci, když mají různé požadavky a restartuje ho po jeho spadne nebo se server nerestartuje.
* Nastavte reverzní proxy server, který předává požadavky na aplikaci.

## <a name="publish-to-a-folder"></a>Publikovat do složky 

[Dotnet publikování](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) rozhraní příkazového řádku příkaz kompilovaný kód aplikace a zkopíruje soubory potřebné ke spuštění aplikace do *publikování* složky. Při nasazení ze sady Visual Studio `dotnet publish` krok provést pro vás automaticky před soubory se zkopírují do cíle nasazení.

### <a name="folder-contents"></a>Obsah složky

*Publikování* složka obsahuje *.exe* a *.dll* soubory pro aplikaci, její závislosti a volitelně modul runtime rozhraní .NET.

.NET Core aplikace lze publikovat jako *nezávislý* nebo *framework závislé*. Pokud je aplikace samostatná, *.dll* soubory, které obsahují modul runtime rozhraní .NET jsou součástí *publikování* složky.  Pokud je aplikace závislá framework, soubory modulu runtime .NET nejsou zahrnuty, protože aplikace obsahuje odkaz na verzi rozhraní .NET, který je nainstalován v počítači. Výchozí model nasazení je závislá na framework. Další informace najdete v tématu [nasazení aplikace .NET Core](https://docs.microsoft.com/dotnet/articles/core/deploying/index).

Kromě *.exe* a *.dll* soubory, *publikování* složky pro aplikace ASP.NET Core obvykle obsahuje konfigurační soubory, statické prostředky a zobrazení MVC.  Další informace najdete v tématu [adresářovou strukturu](xref:hosting/directory-structure).

## <a name="set-up-a-process-manager"></a>Nastavení správce procesu

Aplikace ASP.NET Core je konzolovou aplikaci, která má spustit, když server se spustí a restartovat po dojde k chybě. K automatizaci spustí a restartování, je třeba proces správce. Nejběžnější správci proces pro ASP.NET Core jsou [Nginx](xref:publishing/linuxproduction) a [Apache](xref:publishing/apache-proxy) v systému Linux, a [IIS](xref:publishing/iis) a [služba systému Windows](xref:hosting/windows-service) v systému Windows.

## <a name="set-up-a-reverse-proxy"></a>Nastavit reverzní proxy server

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Pokud vaše aplikace používá [Kestrel](xref:fundamentals/servers/kestrel) webový server, můžete použít [Nginx](xref:publishing/linuxproduction), [Apache](xref:publishing/apache-proxy), nebo [IIS](xref:publishing/iis) jako reverzní proxy server. Reverzní proxy server přijímá požadavky HTTP z Internetu a předává je Kestrel po některé předběžné zpracování. Další informace najdete v tématu [použití Kestrel s reverzní proxy server](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#when-to-use-kestrel-with-a-reverse-proxy).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Pokud vaše aplikace používá [Kestrel](xref:fundamentals/servers/kestrel) webový server a se zveřejní k Internetu, je nutné použít [Nginx](xref:publishing/linuxproduction), [Apache](xref:publishing/apache-proxy), nebo [IIS](xref:publishing/iis) jako zpětného proxy server. Reverzní proxy server přijímá požadavky HTTP z Internetu a předává je Kestrel po některé předběžné zpracování. Hlavním důvodem pro používání reverzní proxy server je zabezpečení. Další informace najdete v tématu [použití Kestrel s reverzní proxy server](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy).

---

## <a name="using-visual-studio-and-msbuild-to-automate-deployment"></a>Pomocí sady Visual Studio a nástroje MSBuild k automatizaci nasazení

Nasazení často vyžaduje další úkoly kromě kopírování výstupu z `dotnet publish` na server. Například můžete chtít zahrnout další soubory v *publikování* složku nebo vyloučit soubory z něj. Visual Studio použije nástroje MSBuild pro nasazení webu a můžete přizpůsobit MSBuild udělat celou řadu dalších úloh během nasazení. Další informace najdete v tématu [profily publikování v sadě Visual Studio](xref:publishing/web-publishing-vs) a [pomocí nástroje MSBuild a Team Foundation Build](http://msbuildbook.com/) adresáře.

Můžete nasadit přímo ze sady Visual Studio do služby Azure App Service pomocí [funkci Publikovat Web](xref:tutorials/publish-to-azure-webapp-using-vs) nebo pomocí [integrovanou podporu Git](xref:publishing/azure-continuous-deployment). Visual Studio Team Services podporuje [průběžné nasazování do služby Azure App Service](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure).

## <a name="publishing-to-azure"></a>Publikování do služby Azure

V tématu [publikování webové aplikace ASP.NET Core Azure App Service pomocí sady Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) pokyny, jak publikovat tuto aplikaci do Azure pomocí sady Visual Studio.  Aplikace lze také publikovat na Azure z [příkazového řádku](xref:tutorials/publish-to-azure-webapp-using-cli).

## <a name="additional-resources"></a>Další zdroje

Informace o používání Docker jako hostitelské prostředí najdete v tématu [hostitele ASP.NET Core aplikací v nástroji Docker](xref:publishing/docker).
