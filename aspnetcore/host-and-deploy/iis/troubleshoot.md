---
title: Řešení potíží s ASP.NET Core ve službě IIS
author: guardrex
description: Zjistěte, jak diagnostikovat problémy s Internetové informační služby (IIS) nasazení aplikací ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: e44892d2022ca1a176cee9d027e220e196c6572d
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>Řešení potíží s ASP.NET Core ve službě IIS

Podle [Luke Latham](https://github.com/guardrex)

Tento článek obsahuje pokyny o tom, jak diagnostikovat ASP.NET Core problém při spuštění aplikace při hostování s [Internetové informační služby (IIS)](/iis). Informace v tomto článku se vztahují na hostování ve službě IIS na serveru Windows a Windows Desktop.

V sadě Visual Studio, výchozí projekt ASP.NET Core [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hostování během ladění. A *502.5 selhání procesu* k tomu dojde při ladění místně může být troubleshooted pomocí doporučení v tomto tématu.

Řešení potíží další témata:

[Řešení potíží s ASP.NET Core ve službě Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)  
I když používá služby App Service [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module) a služby IIS pro hostitele aplikace v tématu vyhrazené pokyny, které jsou specifické pro aplikaci služby.

[Zpracování chyb](xref:fundamentals/error-handling)  
Můžete zjistit, jak se budou zpracovávat chyby v aplikacích ASP.NET Core během vývoje v místním systému.

[Další informace k ladění pomocí sady Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger)  
Toto téma představuje funkce ladicího programu sady Visual Studio.

## <a name="app-startup-errors"></a>Chyby při spuštění aplikace

**502.5 zpracování selhání**  
Pracovní proces se nezdaří. Aplikaci nelze spustit.

Základní modul technologie ASP.NET pokusí spustit pracovní proces, ale se nepodaří spustit. Příčinu selhání spuštění procesu lze určit obvykle položek v [protokolu událostí aplikace](#application-event-log) a [ASP.NET Core modulu stdout protokolu](#aspnet-core-module-stdout-log).

*502.5 selhání procesu* chybová stránka je vrácena, pokud nesprávnou konfiguraci aplikace nebo hostování způsobí, že pracovní proces selhání:

![Okno prohlížeče zobrazující stránku 502.5 selhání procesu](troubleshoot/_static/process-failure-page.png)

**500 Internal Server Error**  
Spuštění aplikace, ale chybu brání splnění požadavku server.

Při spuštění nebo při vytváření odpovědi, k této chybě dojde v kódu aplikace. Odpovědi může obsahovat žádný obsah, nebo se může zobrazit odpověď *500 – Vnitřní chyba serveru* v prohlížeči. V protokolu událostí aplikace obvykle stavy normálně spustil aplikaci. Z hlediska serveru, který je správná. Aplikace se spustit, ale nemůže generovat platnou odpověď. [Spuštění aplikace příkazového řádku](#run-the-app-at-a-command-prompt) na serveru nebo [povolit protokol stdout ASP.NET Core modulu](#aspnet-core-module-stdout-log) k vyřešení tohoto problému.

**Obnovení připojení**

Pokud dojde k chybě po odeslání hlaviček, je příliš pozdní pro server k odeslání **500 – Vnitřní chyba serveru** když dojde k chybě. Často se to stane, když dojde k chybě během serializace komplexních objektů pro odpověď. Tento typ chyby se zobrazí jako *obnovení připojení* chyba na straně klienta. [Protokolování aplikací](xref:fundamentals/logging/index) mohou pomoci při odstraňování těchto typů chyb.

## <a name="default-startup-limits"></a>Výchozí omezení spuštění

Modul základní ASP.NET je konfigurován s výchozím *startupTimeLimit* 120 sekundách. Pokud necháte nastavenou výchozí hodnotu, aplikace může trvat až dvě minuty spustit před modul protokoly selhání procesu. Informace o konfiguraci modulu najdete v tématu [atributy elementu aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

## <a name="troubleshoot-app-startup-errors"></a>Řešení chyb při spuštění aplikace

### <a name="application-event-log"></a>Protokol událostí aplikace

Přístup k protokolu událostí aplikace:

1. Otevření nabídky Start, vyhledejte **Prohlížeč událostí**a pak vyberte **Prohlížeč událostí** aplikace.
1. V **Prohlížeč událostí**, otevřete **protokoly systému Windows** uzlu.
1. Vyberte **aplikace** otevřít protokol událostí aplikace.
1. Vyhledejte chyby související s selhání aplikace. Chyby mít hodnotu *modulu IIS AspNetCore* nebo *služby IIS Express AspNetCore modulu* v *zdroj* sloupce.

### <a name="run-the-app-at-a-command-prompt"></a>Spuštění aplikace příkazového řádku

Mnoho chyb spuštění nepřispívají užitečné informace v protokolu událostí aplikace. Příčinu chyby můžete najít spuštěním aplikace na příkazovém řádku v hostitelském systému.

**Nasazení závislé na Framework**

Pokud je aplikace [nasazení závislé na framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd):

1. Na příkazovém řádku přejděte do složky pro nasazení a spuštění aplikace spuštěním sestavení aplikace s *dotnet.exe*. V následujícím příkazu nahraďte název sestavení aplikace pro \<assembly_name >: `dotnet .\<assembly_name>.dll`.
1. Výstup z aplikace, zobrazuje všechny chyby konzoly se zapíše do okna konzoly.
1. Dojde-li chyby při vytváření požadavku na aplikaci, vytvořte žádost na hostitele a port, kde Kestrel naslouchá. Pomocí výchozího hostitele a post, vydalo požadavek na `http://localhost:5000/`. Pokud aplikace odpovídá za normálních okolností na adrese Kestrel koncový bod, problém je pravděpodobnější související s konfigurací reverzní proxy server a méně pravděpodobně v aplikaci.

**Samostatná nasazení**

Pokud je aplikace [samostatná nasazení](/dotnet/core/deploying/#self-contained-deployments-scd):

1. Na příkazovém řádku přejděte do složky pro nasazení a spustit spustitelný soubor aplikace. V následujícím příkazu nahraďte název sestavení aplikace pro \<assembly_name >: `<assembly_name>.exe`.
1. Výstup z aplikace, zobrazuje všechny chyby konzoly se zapíše do okna konzoly.
1. Dojde-li chyby při vytváření požadavku na aplikaci, vytvořte žádost na hostitele a port, kde Kestrel naslouchá. Pomocí výchozího hostitele a post, vydalo požadavek na `http://localhost:5000/`. Pokud aplikace odpovídá za normálních okolností na adrese Kestrel koncový bod, problém je pravděpodobnější související s konfigurací reverzní proxy server a méně pravděpodobně v aplikaci.

### <a name="aspnet-core-module-stdout-log"></a>ASP.NET Core modulu stdout protokolu

Povolení a zobrazit protokoly stdout:

1. Přejděte do složky pro nasazení webu v hostitelském systému.
1. Pokud *protokoly* složky není přítomna, vytvořte složku. Pokyny o tom, jak povolit MSBuild k vytvoření *protokoly* složky v nasazení automaticky, najdete [adresářovou strukturu](xref:host-and-deploy/directory-structure) tématu.
1. Upravit *web.config* souboru. Nastavit **stdoutLogEnabled** k `true` a změňte **stdoutLogFile** cesta tak, aby odkazoval *protokoly* složky (například `.\logs\stdout`). `stdout` v cestě je předpona názvu souboru protokolu. Časové razítko, id procesu a přípona souboru se přidávají automaticky při vytvoření protokolu. Pomocí `stdout` jako předpona názvu souboru, má název souboru typické protokolu *stdout_20180205184032_5412.log*. 
1. Uložte aktualizovaný *web.config* souboru.
1. Vytvořte žádost na aplikaci.
1. Přejděte na *protokoly* složky. Najít a otevřít poslední stdout protokol.
1. Studie v protokolu chyb.

**Důležité!** Zakážete stdout protokolování po dokončení odstraňování potíží.

1. Upravit *web.config* souboru.
1. Nastavit **stdoutLogEnabled** k `false`.
1. Uložte soubor.

> [!WARNING]
> Nepodařilo se zakázat protokol stdout může vést k selhání aplikace nebo serveru. Neexistuje žádné omezení velikosti souboru protokolu nebo počet soubory protokolů vytvořené.
>
> Pro běžné protokolování v aplikaci ASP.NET Core, použijte knihovnu protokolování, který omezuje velikost souboru protokolu a otočí protokoly. Další informace najdete v tématu [poskytovatelů třetích stran protokolování](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="enabling-the-developer-exception-page"></a>Povolení stránce vývojáře výjimky

`ASPNETCORE_ENVIRONMENT` [Proměnné prostředí lze přidat do souboru web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) a spusťte aplikaci ve vývojovém prostředí. Tak dlouho, dokud není prostředí přepsána ve spuštění aplikace pomocí `UseEnvironment` nastavení proměnné prostředí umožňuje na tvůrce hostitele, [vývojáře výjimka stránky](xref:fundamentals/error-handling) zobrazí při spuštění aplikace.

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

Nastavení proměnné prostředí pro `ASPNETCORE_ENVIRONMENT` doporučujeme použít pouze pro použití v přípravy a testování serverů, které nejsou vystaveny v Internetu. Odebrat proměnnou prostředí z *web.config* soubor po vyřešení potíží. Informace o nastavení proměnných prostředí *web.config*, najdete v části [environmentVariables podřízený element aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

## <a name="common-startup-errors"></a>Běžné chyby při spuštění 

Najdete v článku [ASP.NET Core běžné chyby odkaz](xref:host-and-deploy/azure-iis-errors-reference). Většina běžné problémy, které brání spuštění aplikací jsou popsané v referenčním tématu.

## <a name="slow-or-hanging-app"></a>Pomalá nebo měny aplikace

Pokud aplikace reaguje pomalu nebo přestane reagovat na vyžádání, získání a analyzovat [souboru s výpisem](/visualstudio/debugger/using-dump-files). Souborů výpisu paměti se dají získat pomocí kteréhokoli z následujících nástrojů:

* [ProcDump](/sysinternals/downloads/procdump)
* [Nástroj DebugDiag](https://www.microsoft.com/download/details.aspx?id=49924)
* WinDbg: [stáhnout nástroje pro ladění pro systém Windows](https://developer.microsoft.com/windows/hardware/download-windbg), [WinDbg ladění pomocí](/windows-hardware/drivers/debugger/debugging-using-windbg)

## <a name="remote-debugging"></a>Vzdálené ladění

V tématu [vzdáleného ladění ASP.NET Core ve vzdáleném počítači služby IIS v aplikaci Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) v dokumentaci sady Visual Studio.

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/) poskytuje telemetrie z aplikace hostované službou IIS, včetně protokolování a funkce generování sestav. Application Insights lze pouze sestavy chyb, které se provádějí až po aplikace spustí, když je k dispozici funkcí protokolování aplikace. Další informace najdete v tématu [Application Insights pro ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="additional-troubleshooting-advice"></a>Další pomoc při řešení potíží

Někdy se nezdaří funkční aplikaci okamžitě po provedení upgradu buď .NET Core SDK na vývoj pro počítače nebo balíček verze v aplikaci. V některých případech je možné osamocené balíčky ukončit aplikaci při provádění hlavních aktualizací. Většina těchto problémů odstraněny podle těchto pokynů:

1. Odstranit *bin* a *obj* složek.
1. Zrušte balíček ukládá do mezipaměti na *% UserProfile %\\.nuget\\balíčky* a *LocalAppData %\\Nuget\\v3 mezipaměti*.
1. Obnovit a znovu sestavte projekt.
1. Potvrzení, že předchozí nasazení na serveru byla úplně odstranit před opětovného nasazení aplikace.

> [!TIP]
> Je spuštění vhodný způsob k vymazání mezipamětí balíček `dotnet nuget locals all --clear` z příkazového řádku.
> 
> Vymazání mezipaměti balíčku můžete také provést pomocí [nuget.exe](https://www.nuget.org/downloads) nástroj a provádění příkazu `nuget locals all -clear`. *nuget.exe* není připojené instalace s operačním systémem Windows a musí být získána odděleně od [webu NuGet](https://www.nuget.org/downloads).

## <a name="additional-resources"></a>Další zdroje

* [Úvod do zpracování chyb v ASP.NET Core](xref:fundamentals/error-handling)
* [Běžné chyby referenční dokumentace pro Azure App Service a IIS s ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Referenční dokumentace k modulu ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Řešení potíží s ASP.NET Core ve službě Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)
