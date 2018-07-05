---
uid: aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
title: Middleware OWIN v IIS integrovaný kanál | Dokumentace Microsoftu
author: Praburaj
description: Tento článek popisuje, jak spustit komponenty middlewaru OWIN (OMCs) v integrovaném kanálu IIS a jak nastavit událost kanálu OMC poběží. Měli byste...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2013
ms.topic: article
ms.assetid: d031c021-33c2-45a5-bf9f-98f8fa78c2ab
ms.technology: ''
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
msc.type: authoredcontent
ms.openlocfilehash: 1c2f7a9b948331eae2f5b44f25219adb76a0c745
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379057"
---
<a name="owin-middleware-in-the-iis-integrated-pipeline"></a>Middleware OWIN v integrovaném kanálu IIS
====================
podle [Praburaj manažer](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)

> Tento článek popisuje, jak spustit komponenty middlewaru OWIN (OMCs) v integrovaném kanálu IIS a jak nastavit událost kanálu OMC poběží. Měli byste si přečíst [Přehled projektu Katana](an-overview-of-project-katana.md) a [rozpoznání spouštěcí třídy OWIN](owin-startup-class-detection.md) před čtením tohoto kurzu. V tomto kurzu byla zapsána od Ricka Andersona ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Chris Ross, Manažer Praburaj a Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).


I když [OWIN](an-overview-of-project-katana.md) middlewarových komponent (OMCs) jsou primárně určený ke spuštění v kanálu nezávislá na server, je možné spouštět v integrovaném kanálu IIS i OMC (**klasického režimu je *není* podporované**). OMC lze pracovat v integrovaném kanálu IIS instalací následujících balíčků z konzoly Správce balíčků (PMC):

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample1.cmd)]

To znamená, že všechny aplikační architektury, včetně těch, které ještě nejsou možné spouštět mimo službu IIS a System.Web, můžete těžit z existující komponenty middlewaru OWIN. 

> [!NOTE]
> Všechny `Microsoft.Owin.Security.*` balíčky přesouvání do nového systému identit v sadě Visual Studio 2013 (Příklad: soubory cookie, Account Microsoft, Google, Facebook, Twitter, [nosného tokenu](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html), OAuth, autorizační server, tokenů JWT, Azure Active adresář a služby Active directory federation services) jsou vytvořené jako OMCs a je možné scénáře v místním prostředí i hostované službou IIS.

## <a name="how-owin-middleware-executes-in-the-iis-integrated-pipeline"></a>Jak se spustí OWIN Middleware v integrovaném kanálu IIS

Pro konzolové aplikace OWIN, kanálu aplikace vyvíjené v [konfiguraci spuštění](owin-startup-class-detection.md) nastavena podle pořadí součásti jsou přidány pomocí `IAppBuilder.Use` metody. To znamená, kanál OWIN v [Katana](an-overview-of-project-katana.md) modul runtime bude zpracovávat OMCs v pořadí, které jsou registrované pomocí `IAppBuilder.Use`. V integrovaném kanálu IIS požadavek kanál se skládá z [HttpModules](https://msdn.microsoft.com/library/ms178468(v=vs.85).aspx) předplatné služby, jako předem definovanou sadu události kanálu [BeginRequest](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx), [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), [AuthorizeRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx)atd.

Pokud porovnáme OMC od [HttpModule](https://msdn.microsoft.com/library/zec9k340(v=vs.85).aspx) na světě ASP.NET musí být zaregistrovaný OMC správné předem definovaných kanál události. Například HttpModule `MyModule` bude získat vyvolá se, pokud jde o požadavek [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) fáze v kanálu:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample2.cs?highlight=10)]

Aby OMC k účasti v tento stejný, založený na událostech spuštění řazení [Katana](an-overview-of-project-katana.md) prohledává kód při spuštění [konfiguraci spuštění](owin-startup-class-detection.md) a odebírá všechny komponenty middlewaru pro Integrovaný kanál události. Například následující kód OMC a registrace vám umožní zobrazit registraci události výchozí middlewarových komponent. (Podrobnější pokyny pro vytvoření třídy pro spuštění OWIN, najdete v článku [rozpoznání spouštěcí třídy OWIN](owin-startup-class-detection.md).)

1. Vytvořte prázdný webový projekt aplikace s názvem **owin2**.
2. Z balíčku správce konzoly (konzolu PMC), spusťte následující příkaz: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample3.cmd)]
3. Přidat `OWIN Startup Class` a pojmenujte ho `Startup`. Generovaného kódu nahraďte následujícím (změny se zvýrazní):  

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample4.cs?highlight=5-7,15-36)]
4. Stisknutím klávesy F5 spusťte aplikaci.

Konfigurace spuštění nastaví kanál pomocí tří middlewarových komponent, první dvě zobrazení diagnostických informací a poslední z nich reagování na události (a také zobrazit diagnostické informace). `PrintCurrentIntegratedPipelineStage` Metoda zobrazí tento middleware je vyvolán v události integrovaného kanálu a napište zprávu. Výstup windows zobrazí následující informace:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample5.cmd)]

Modul runtime Katana mapovat všechny komponenty middlewaru OWIN k [PreExecuteRequestHandler](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx) ve výchozím nastavení, která odpovídá událost kanálu IIS [PreRequestHandlerExecute](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx).

## <a name="stage-markers"></a>Fáze značky

Můžete označit OMCs ke spuštění na konkrétní fází kanálu pomocí `IAppBuilder UseStageMarker()` – metoda rozšíření. Chcete-li spustit sadu middlewarových komponent konkrétní fázi, vložit značka fáze hned za poslední částí je sada během registrace. Existují pravidla, na kterou fází má kanál můžete spustit middleware a je nutné spustit pořadí součásti (pravidla jsou vysvětleny dále v tomto kurzu). Přidat `UseStageMarker` metodu `Configuration` kódu, jak je znázorněno níže:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample6.cs?highlight=13,19)]

`app.UseStageMarker(PipelineStage.Authenticate)` Volání nakonfiguruje všechny komponenty middlewaru dříve zaregistrovaný (v tomto případě naší prostředními diagnostických) ke spuštění na fázi ověřování kanálu. Poslední částí middleware (který zobrazuje diagnostiky a reaguje na požadavky) se spustí na `ResolveCache` fáze ( [ResolveRequestCache](https://msdn.microsoft.com/library/system.web.httpapplication.resolverequestcache.aspx) událostí).

Stisknutím klávesy F5 spusťte aplikaci. V okně výstup zobrazuje následující:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample7.cmd)]

## <a name="stage-marker-rules"></a>Pravidla značka fáze

Komponenty middlewaru Owin (OMC) lze nastavit na spuštění při následujících událostech fáze kanálu OWIN:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample8.cs)]

1. Ve výchozím nastavení, OMCs spuštěny při poslední události (`PreHandlerExecute`). To je důvod, proč našeho prvního příkladu kódu zobrazí "PreExecuteRequestHandler".
2. Můžete použít na `app.UseStageMarker` podle metody pro registraci OMC ke spuštění dříve, v jakékoli fázi kanálu OWIN `PipelineStage` výčtu.
3. Kanál OWIN a kanálu IIS je seřazen, proto volání `app.UseStageMarker` musí být v pořadí. Obslužná rutina události nelze nastavit na událost, která předchází poslední událost zaregistrována k `app.UseStageMarker`. Například *po* volání:

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample9.cmd)]

   volání `app.UseStageMarker` předávání `Authenticate` nebo `PostAuthenticate` nebude respektovat, a bude vyvolána žádná výjimka. Spuštění v poslední fázi, která ve výchozím nastavení je OMCs `PreHandlerExecute`. Fáze značky se používají k Ujistěte se, aby běžela dříve. Pokud chcete zadat značky fází mimo pořadí, můžeme zaokrouhlit na dřívější značky. Jinými slovy přidání značky fází říká "Spustit později než fázi X". OMC pro spuštění na nejbližší značky fází přidaní je v kanálu OWIN.
4. Nejdříve volání `app.UseStageMarker` wins. Například, pokud přepnout pořadí `app.UseStageMarker` volání z našich předchozím příkladu:

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample10.cs?highlight=13,19)]

   V okně výstupu se zobrazí: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample11.cmd)]

   OMCs všech spuštění v `AuthenticateRequest` fáze, protože poslední OMC zaregistrované `Authenticate` události a `Authenticate` události předchází všechny další události.
