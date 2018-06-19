---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: Upgrade projektů 1.x SignalR na verzi 2 | Microsoft Docs
author: pfletcher
description: Toto téma popisuje postup upgradu existující projekt 1.x SignalR na SignalR 2.x a řešení potíží, které mohou nastat během procesu upgradu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: e372275ae5dd4bbf354db2d02e4407f8c513b7a3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566026"
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a>Upgrade projektů 1.x SignalR na verze 2
====================
podle [Patrik Fletcher](https://github.com/pfletcher)

> Toto téma popisuje postup upgradu existující projekt 1.x SignalR na SignalR 2.x a řešení potíží, které mohou nastat během procesu upgradu.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR verze 1 a 2
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Tento kurz pomocí sady Visual Studio 2012
> 
> 
> Pokud chcete používat Visual Studio 2012 v tomto kurzu, postupujte takto:
> 
> - Aktualizace vašeho [Správce balíčků](http://docs.nuget.org/docs/start-here/installing-nuget) na nejnovější verzi.
> - Nainstalujte [webové platformy](https://www.microsoft.com/web/downloads/platform.aspx).
> - Ve webové platformy, vyhledání a instalace **ASP.NET a webové nástroje 2013.1 pro sadu Visual Studio 2012**. Šablony sady Visual Studio pro třídy SignalR dojde k instalaci, jako **rozbočovače**.
> - Některé šablony (jako například **třídy pro spuštění OWIN**) nebudou k dispozici; pro tyto třídy soubor použít místo.
> 
> 
> ## <a name="questions-and-comments"></a>Dotazy a připomínky
> 
> Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky. Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com/).


SignalR 2 nabízí konzistentní vývojového prostředí napříč platformami serveru pomocí [OWIN](http://owin.org). Tento článek popisuje několik kroků, které jsou potřebné k aktualizaci 1.x SignalR na verze 2.

Když toto je vhodné k upgradu aplikace SignalR 2, SignalR 1.x, i nadále podporována.

Tento kurz popisuje, jak upgradovat hostované webové aplikace SignalR 2. Samoobslužné hostovaných aplikací (ty, které v konzolové aplikaci, služba systému Windows nebo jiný proces serveru hostitele) jsou nyní podporovány pod SignalR 2. Informace o tom, jak začít vytvářet vlastním hostováním aplikací s SignalR 2 najdete v tématu [kurz: SignalR hostování na vlastním](../deployment/tutorial-signalr-self-host.md).

## <a name="contents"></a>Obsah

Následující části popisují úlohy, které jsou spojené s upgrade SignalR projekty a řešení potíží, které může způsobit.

- [Příklad: Upgrade kurzu Začínáme SignalR 2](#example)
- [Řešení potíží s chybami došlo během upgradu](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a>Příklad: Upgrade Začínáme kurz aplikace pro SignalR 2

V této části budete aktualizovat vytvořené v aplikaci [SignalR 1.x verzi kurzu Začínáme](../older-versions/index.md) používat SignalR 2.

1. Po dokončení tohoto kurzu Začínáme, klikněte pravým tlačítkem na projekt a vyberte **vlastnosti**. Ověřte, zda **cílové rozhraní** je nastaven na **rozhraní .NET Framework 4.5.**
2. Otevřete konzolu Správce balíčků. Odebrat SignalR 1.x z projektu pomocí následujícího příkazu:

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. Nainstalujte SignalR 2 pomocí následujícího příkazu:

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. Na stránce HTML aktualizujte odkaz na skript pro SignalR tak, aby odpovídala verzi skriptů teď zahrnutý v projektu.

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. Ve třídě globální aplikaci odeberte volání MapHubs.

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. Klikněte pravým tlačítkem na řešení a vyberte **přidat**, **novou položku...** . V dialogovém okně vyberte **třídy pro spuštění Owin**. Pojmenujte novou třídu **Startup.cs**.

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. Nahraďte obsah souboru Startup.cs následujícím kódem:

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    Atribut sestavení přidá třídu do procesu spuštění na Owin, který provede `Configuration` metoda při spuštění Owin. Naopak volá `MapSignalR` metodu, která vytvoří trasy pro všechny rozbočovače SignalR v aplikaci.
8. Spusťte projekt a zkopírujte adresu URL na hlavní stránku do jiné prohlížeče nebo podokně prohlížeče jako předtím. Každé stránce výzvou k zadání uživatelského jména a zpráv odeslaných z každé stránce musí být viditelná v obou podokna prohlížeče.

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a>Řešení potíží s chybami došlo během upgradu

Tato část popisuje problémy, které mohou nastat během upgradu. Komplexnější seznam chyb a problémů, které mohou nastat s aplikací SignalR naleznete v části [řešení potíží s SignalR](../testing-and-debugging/troubleshooting.md).

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a>'Volání je nejednoznačný mezi následující metody nebo vlastnosti.

K této chybě dojde, pokud odkaz na `Microsoft.AspNet.SignalR.Owin` se neodebere. Tento balíček je zastaralý; odkaz musí být odebrány a verze 1.x SelfHost balíčku musí být odinstalovány.

### <a name="hub-methods-fail-silently"></a>Bezobslužná selžou metody rozbočovače

Ověřte, zda odkazům na skript v vašeho klienta jsou aktuální, a který `OwinStartup` atribut pro třídy pro spuštění má správné třídy a názvy sestavení pro váš projekt. Zkuste také otevřením centra adresa (/ signalr/hubs) ve vašem prohlížeči; všechny chyby, který se zobrazí nabídne další informace o příčinu problému.
