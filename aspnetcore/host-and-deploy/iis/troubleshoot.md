---
title: "Řešení potíží s ASP.NET Core ve službě IIS"
author: guardrex
description: "Zjistěte, jak diagnostikovat problémy s IIS nasazení aplikací ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: c68070a9cba5667504d1ad4927b02b73f83e6573
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>Řešení potíží s ASP.NET Core ve službě IIS

Podle [Luke Latham](https://github.com/guardrex)

O vyřešení problémů s nasazením služby IIS:

* Studie výstup prohlížeče.
* Zkontrolovat, zda systém **aplikace** protokolu prostřednictvím **Prohlížeč událostí**.
* Povolit `stdout` protokolování. **ASP.NET Core modulu** protokolu nachází na v zadané cestě *stdoutLogFile* atribut `<aspNetCore>` element v *web.config*. Všechny složky v cestě uvedené v hodnotě atributu musí existovat v nasazení. Nastavit *stdoutLogEnabled* k `true`. Aplikace, které používají `Microsoft.NET.Sdk.Web` SDK k vytvoření *web.config* souboru výchozí *stdoutLogEnabled* nastavení `false`, takže ručně, zadejte *web.config* souboru nebo upravte soubor. Chcete-li povolit `stdout` protokolování.

Použijte informace z těchto tří zdrojů s [běžné chyby referenční téma](xref:host-and-deploy/azure-iis-errors-reference) ke zjištění problému. Postupujte podle pomoc při řešení potíží poskytuje k vyřešení problému.

Několik běžných chyb, dokud modul nezobrazí v prohlížeči, protokolu aplikace a ASP.NET Core modulu protokolu *startupTimeLimit* (výchozí: 120 sekundách) a *startupRetryCount* (výchozí: 2) byly splněny. Proto počkejte úplné šest minut před deducing které modulu se nepodařilo spustit proces pro aplikaci.

A spusťte aplikaci přímo na Kestrel je jeden rychlý způsob, jak zjistit, zda aplikace funguje správně. Pokud aplikace byl publikován jako [nasazení závislé na framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd), provést `dotnet <assembly_name>.dll` ve složce nasazení, což je fyzická cesta k aplikaci služby IIS. Pokud aplikace byl publikován jako [samostatná nasazení](/dotnet/core/deploying/#self-contained-deployments-scd)spusťte aplikaci je spustitelný soubor přímo z příkazového řádku, `<assembly_name>.exe`, ve složce nasazení. Pokud Kestrel naslouchá na výchozí port 5000, aplikace by měly být dostupné v `http://localhost:5000/`. Pokud aplikace odpovídá za normálních okolností na adrese Kestrel koncový bod, problém je pravděpodobnější související s konfigurací reverzní proxy server a méně pravděpodobně v aplikaci.

Jeden způsob jak určit, jestli je správně funguje reverzní proxy server je provést žádost o jednoduché statických souborů pro šablony stylů, skript nebo bitovou kopii z aplikace statické soubory v *wwwroot* pomocí [Middleware statické soubory](xref:fundamentals/static-files). Pokud aplikace může obsluhovat statické soubory, ale další koncové body a zobrazení MVC selhávají, problém je méně pravděpodobné týkající se konfigurace reverzní proxy server a vyšší pravděpodobnost v rámci aplikace (pro příklad směrování MVC nebo 500 – Vnitřní chyba serveru).

Pokud Kestrel spustí běžným způsobem za služby IIS, ale aplikace se nespustí v systému po úspěšném spuštění místně, proměnné prostředí můžete dočasně zařadí do *web.config* nastavit `ASPNETCORE_ENVIRONMENT` k `Development`. Tak dlouho, dokud prostředí není přepsána ve spuštění aplikace, nastavení proměnné prostředí umožňuje [vývojáře výjimka stránky](xref:fundamentals/error-handling) zobrazí při spuštění aplikace. Nastavení proměnné prostředí pro `ASPNETCORE_ENVIRONMENT` tímto způsobem doporučujeme pouze pro testování nebo pracovní servery, které nejsou vystaveny k Internetu. Nezapomeňte odebrat proměnnou prostředí z *web.config* souboru po dokončení. Informace o nastavení proměnné prostředí prostřednictvím *web.config*, najdete v části [environmentVariables podřízený element aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

Ve většině případů povolení protokolování aplikací pomáhá při řešení potíží s aplikací nebo reverzní proxy server. V tématu [protokolování](xref:fundamentals/logging/index) Další informace.

Poslední tip pro odstraňování potíží se vztahují k aplikacím, které se nepodařilo spustit po upgradu buď .NET Core SDK na vývoj pro počítače nebo balíček verze v aplikaci. V některých případech je možné osamocené balíčky ukončit aplikaci při provádění hlavních aktualizací. Většina těchto problémů je možné vyřešit:

* Odstraňování `bin` a `obj` složek v projektu.
* Probíhá vymazání balíček ukládá do mezipaměti na `%UserProfile%\.nuget\packages\` a `%LocalAppData%\Nuget\v3-cache`.
* Obnovení a projekt znovu sestavit.
* Potvrzení, že předchozí nasazení na serveru úplně odstranit před novým nasazením aplikace.

> [!TIP]
> Je spuštění vhodný způsob k vymazání mezipamětí balíček `dotnet nuget locals all --clear` z příkazového řádku.
> 
> Vymazání mezipaměti balíčku můžete také provést pomocí [nuget.exe](https://www.nuget.org/downloads) nástroj a provádění příkazu `nuget locals all -clear`. *nuget.exe* není připojené instalace s Windows 10 a musí být samostatně získat z webu NuGet.
<!--
> [!TIP]
> A convenient way to clear package caches is to:
>
> * Obtain the *NuGet.exe* tool from [NuGet.org](https://www.nuget.org/).
> * Add the path to *NuGet.exe* to the system PATH.
> * Execute `nuget locals all -clear` from a command prompt.
>
> Alternatively, execute `dotnet nuget locals all --clear` from a command prompt without obtaining *NuGet.exe*. -->

## <a name="additional-resources"></a>Další zdroje

* [Běžné chyby referenční dokumentace pro Azure App Service a IIS s ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Referenční dokumentace k modulu ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
