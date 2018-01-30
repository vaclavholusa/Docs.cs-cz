---
title: "Hostování a nasazení základní technologie ASP.NET"
author: tdykstra
description: "Zjistěte, jak nastavit hostitelská prostředí a nasazení aplikací ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/index
ms.openlocfilehash: 7d8ba912da4c0e543bd4dd56632cdc41706814d1
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="host-and-deploy-aspnet-core"></a>Hostování a nasazení základní technologie ASP.NET

Obecný postup nasazení aplikace ASP.NET Core do hostitelského prostředí:

* Publikujte aplikaci do složky na hostitelském serveru.
* Nastavte správce procesu, který se spustí aplikace při žádosti o doručení a restartování aplikace po jeho spadne nebo se server nerestartuje.
* Nastavte reverzní proxy server, který předává požadavky na aplikaci.

## <a name="publish-to-a-folder"></a>Publikovat do složky 

[Dotnet publikování](/dotnet/articles/core/tools/dotnet-publish) rozhraní příkazového řádku příkaz kompilovaný kód aplikace a zkopíruje soubory potřebné ke spuštění aplikace do *publikování* složky. Při nasazení ze sady Visual Studio `dotnet publish` kroku dojde automaticky před soubory se zkopírují do cíle nasazení.

### <a name="folder-contents"></a>Obsah složky

*Publikování* složka obsahuje *.exe* a *.dll* soubory pro aplikaci, její závislosti a volitelně modul runtime rozhraní .NET.

.NET Core aplikace lze publikovat jako *nezávislý* nebo *framework závislé* aplikace. Pokud je aplikace samostatná, *.dll* soubory, které obsahují modul runtime rozhraní .NET jsou součástí *publikování* složky. Pokud je aplikace závislá framework, soubory modulu runtime rozhraní .NET nejsou zahrnuté, protože aplikace obsahuje odkaz na verzi rozhraní .NET, který je nainstalován na serveru. Výchozí model nasazení je závislá na framework. Další informace najdete v tématu [nasazení aplikace .NET Core](/dotnet/articles/core/deploying/index).

Kromě *.exe* a *.dll* soubory, *publikování* složky pro aplikace ASP.NET Core obvykle obsahuje konfigurační soubory, statické prostředky a zobrazení MVC. Další informace najdete v tématu [adresářovou strukturu](xref:host-and-deploy/directory-structure).

## <a name="set-up-a-process-manager"></a>Nastavení správce procesu

Aplikace ASP.NET Core je konzolovou aplikaci, která musí být spuštěna, když server se spustí a restartovat, pokud ji dojde k chybě. Automatizovat spustí a restartování, proces manager je potřeba. Nejběžnější správci proces pro ASP.NET Core jsou:

* Linux
  * [Nginx](xref:host-and-deploy/linux-nginx)
  * [Apache](xref:host-and-deploy/linux-apache)
* Windows
  * [IIS](xref:host-and-deploy/iis/index)
  * [Windows Service](xref:host-and-deploy/windows-service)

## <a name="set-up-a-reverse-proxy"></a>Nastavit reverzní proxy server

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Pokud aplikace používá [Kestrel](xref:fundamentals/servers/kestrel) webový server, [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), nebo [IIS](xref:host-and-deploy/iis/index) slouží jako reverzní proxy server. Reverzní proxy server přijímá požadavky HTTP z Internetu a předává je Kestrel po některé předběžné zpracování. Další informace najdete v tématu [použití Kestrel s reverzní proxy server](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#when-to-use-kestrel-with-a-reverse-proxy).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Pokud aplikace používá [Kestrel](xref:fundamentals/servers/kestrel) webový server a bude přístup k Internetu, použijte [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), nebo [IIS](xref:host-and-deploy/iis/index) jako reverzní proxy server. Reverzní proxy server přijímá požadavky HTTP z Internetu a předává je Kestrel po některé předběžné zpracování. Hlavním důvodem pro používání reverzní proxy server je zabezpečení. Další informace najdete v tématu [použití Kestrel s reverzní proxy server](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy).

---

## <a name="using-visual-studio-and-msbuild-to-automate-deployment"></a>Pomocí sady Visual Studio a nástroje MSBuild k automatizaci nasazení

Nasazení často vyžaduje další úkoly kromě kopírování výstupu z `dotnet publish` na server. Například může další soubory, které vyžaduje nebo vyloučit z *publikování* složky. Visual Studio použije nástroje MSBuild pro nasazení webu a MSBuild lze přizpůsobit udělat celou řadu dalších úloh během nasazení. Další informace najdete v tématu [profily publikování v sadě Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) a [pomocí nástroje MSBuild a Team Foundation Build](http://msbuildbook.com/) adresáře.

Pomocí [funkci Publikovat Web](xref:tutorials/publish-to-azure-webapp-using-vs) nebo [integrovanou podporu Git](xref:host-and-deploy/azure-apps/azure-continuous-deployment), aplikace se dá nasadit přímo ze sady Visual Studio na Azure App Service. Visual Studio Team Services podporuje [průběžné nasazování do služby Azure App Service](/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?tabs=vsts).

## <a name="publishing-to-azure"></a>Publikování do služby Azure

V tématu [publikování webové aplikace ASP.NET Core Azure App Service pomocí sady Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) pokyny o tom, jak publikovat aplikaci do Azure pomocí sady Visual Studio. Aplikace lze také publikovat na Azure z [příkazového řádku](xref:tutorials/publish-to-azure-webapp-using-cli).

## <a name="additional-resources"></a>Další zdroje

Informace o používání Docker jako hostitelské prostředí najdete v tématu [hostitele ASP.NET Core aplikací v nástroji Docker](xref:host-and-deploy/docker/index).
