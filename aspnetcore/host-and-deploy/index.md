---
title: Hostitelství a nasazení ASP.NET Core
author: rick-anderson
description: Zjistěte, jak nastavit hostitelská prostředí a nasazení aplikace ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 08/07/2017
uid: host-and-deploy/index
ms.openlocfilehash: 024275be3fc5db3f2ed2f7cea1582a1a5f12bda7
ms.sourcegitcommit: 32f5ee0690604d451f61e9a5c28881c9fcf85738
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/29/2018
ms.locfileid: "47454751"
---
# <a name="host-and-deploy-aspnet-core"></a>Hostitelství a nasazení ASP.NET Core

Obecný postup nasazení aplikace ASP.NET Core do hostitelského prostředí:

* Publikujte aplikaci do složky na hostitelském serveru.
* Nastavte správce procesu, který se spustí aplikace při žádosti o doručení a restartuje aplikaci poté, co ho dojde k chybě nebo se server nerestartuje.
* Pokud se požaduje konfigurace reverzního proxy serveru, nastavení reverzní proxy server, který předává požadavky do aplikace.

## <a name="publish-to-a-folder"></a>Publikovat do složky

[Dotnet publikovat](/dotnet/articles/core/tools/dotnet-publish) příkazu rozhraní příkazového řádku zkompiluje kód aplikace a zkopíruje soubory potřebné ke spuštění aplikace do *publikovat* složky. Při nasazení ze sady Visual Studio [dotnet publikovat](/dotnet/core/tools/dotnet-publish) kroku dojde automaticky před soubory se zkopírují do cíle nasazení.

### <a name="folder-contents"></a>Obsah složky

*Publikovat* složka obsahuje *.exe* a *.dll* souborů pro aplikace, jeho závislosti a volitelně modul .NET runtime.

Aplikace .NET Core můžete publikovat jako *samostatná* nebo *závisí na architektuře* aplikace. Pokud je samostatná, aplikace *.dll* soubory, které obsahují modul .NET runtime jsou součástí *publikovat* složky. Pokud je aplikace závisí na architektuře, nejsou zahrnuté soubory modulu runtime .NET, protože aplikace obsahuje odkaz na verzi rozhraní .NET, která je nainstalována na serveru. Výchozí model nasazení je závisí na architektuře. Další informace najdete v tématu [nasazení aplikace .NET Core](/dotnet/articles/core/deploying/index).

Kromě *.exe* a *.dll* soubory, *publikovat* složku pro aplikace ASP.NET Core obvykle obsahuje konfigurační soubory, statických prostředků a zobrazení MVC. Další informace najdete v tématu [adresářovou strukturu](xref:host-and-deploy/directory-structure).

## <a name="set-up-a-process-manager"></a>Nastavit správce procesu

Aplikace ASP.NET Core je konzolová aplikace, které musí být spuštěna, když na server se spustí a restartovaný, pokud ho dojde k chybě. Chcete-li automatizovat spustí a restartuje, je potřeba správce procesu. Nejběžnější vedoucí proces pro ASP.NET Core jsou:

* Linux
  * [Server Nginx](xref:host-and-deploy/linux-nginx)
  * [Apache](xref:host-and-deploy/linux-apache)
* Windows
  * [SLUŽBA IIS](xref:host-and-deploy/iis/index)
  * [Windows Service](xref:host-and-deploy/windows-service)

## <a name="set-up-a-reverse-proxy"></a>Nastavit reverzní proxy server

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Pokud aplikace využívá [Kestrel](xref:fundamentals/servers/kestrel) webový server [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), nebo [IIS](xref:host-and-deploy/iis/index) může sloužit jako reverzní proxy server. Reverzní proxy server přijímá požadavky HTTP z Internetu a předává je na Kestrel po některé předběžného zpracování.

Buď konfiguraci&mdash;s nebo bez něj reverzní proxy server&mdash;je platný a podporované konfigurace pro hostování pro ASP.NET Core 2.0 nebo novější. Další informace najdete v tématu [použití Kestrel s reverzní proxy server](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Pokud aplikace využívá [Kestrel](xref:fundamentals/servers/kestrel) webový server a budou zveřejněny na Internetu, použijte [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), nebo [IIS](xref:host-and-deploy/iis/index) jako reverzní proxy server. Reverzní proxy server přijímá požadavky HTTP z Internetu a předává je na Kestrel po některé předběžného zpracování. Hlavním důvodem pro pomocí reverzního proxy je zabezpečení. Další informace najdete v tématu [použití Kestrel s reverzní proxy server](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy).

---

## <a name="proxy-server-and-load-balancer-scenarios"></a>Proxy server a scénáře pro nástroj pro vyrovnávání zatížení

Další konfigurace může být nezbytný pro aplikací hostovaných za službou proxy servery a nástroje pro vyrovnávání zatížení. Bez další konfigurace nemusí aplikaci mít přístup k schématu (HTTP/HTTPS) a Vzdálená IP adresa původu žádost. Další informace najdete v tématu [konfigurace ASP.NET Core práci se servery proxy a nástroje pro vyrovnávání zatížení](xref:host-and-deploy/proxy-load-balancer).

## <a name="using-visual-studio-and-msbuild-to-automate-deployment"></a>Pomocí sady Visual Studio a nástroje MSBuild k automatizaci nasazení

Nasazení často vyžaduje další úkoly kromě kopírování výstup z [dotnet publikovat](/dotnet/core/tools/dotnet-publish) na server. Například může být nutné nebo vyloučeny ze dalších souborů *publikovat* složky. Visual Studio používá MSBuild pro nasazení webu a je možné přizpůsobit MSBuild k provádění mnoha jiných úloh během nasazování. Další informace najdete v tématu [publikační profily v sadě Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) a [pomocí nástroje MSBuild a Team Foundation Build](http://msbuildbook.com/) knihy.

S použitím [funkci Publikovat Web](xref:tutorials/publish-to-azure-webapp-using-vs) nebo [integrovanou podporu Git](xref:host-and-deploy/azure-apps/azure-continuous-deployment), aplikace se dají nasadit přímo z Visual Studio do služby Azure App Service. Podporuje služby Azure DevOps [průběžné nasazování do služby Azure App Service](/azure/devops/pipelines/targets/webapp).

## <a name="publishing-to-azure"></a>Publikování do Azure

Zobrazit [publikovat webovou aplikaci ASP.NET Core do služby Azure App Service pomocí sady Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) pokyny o tom, jak publikovat aplikaci do Azure pomocí sady Visual Studio. Aplikace můžete publikovat také do Azure z [příkazového řádku](/azure/app-service/app-service-web-get-started-dotnet).

## <a name="host-in-a-web-farm"></a>Hostování ve webové farmě

Informace o konfiguraci pro hostování aplikací ASP.NET Core v prostředí webové farmy (například nasazení více instancí vaší aplikace pro zajištění škálovatelnosti), najdete v části <xref:host-and-deploy/web-farm>.

## <a name="additional-resources"></a>Další zdroje

Informace o použití Dockeru jako hostitelské prostředí najdete v tématu [hostitele ASP.NET Core aplikace v Dockeru](xref:host-and-deploy/docker/index).
