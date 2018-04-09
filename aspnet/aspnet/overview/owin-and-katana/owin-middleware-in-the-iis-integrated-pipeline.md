---
uid: aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
title: Middleware OWIN v IIS integrovaný kanál | Microsoft Docs
author: Praburaj
description: Tento článek ukazuje, jak spustit komponenty middlewaru OWIN (OMCs) v integrovaném kanálu služby IIS a jak nastavit kanál událostí OMC běží na. Měli byste...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2013
ms.topic: article
ms.assetid: d031c021-33c2-45a5-bf9f-98f8fa78c2ab
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
msc.type: authoredcontent
ms.openlocfilehash: 5df70c80084a32c5f61ac9288c8cdbfaaa47f124
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="owin-middleware-in-the-iis-integrated-pipeline"></a>Middleware OWIN v integrovaném kanálu služby IIS
====================
podle [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)

> Tento článek ukazuje, jak spustit komponenty middlewaru OWIN (OMCs) v integrovaném kanálu služby IIS a jak nastavit kanál událostí OMC běží na. Byste si měli projít [Přehled projektu Katana](an-overview-of-project-katana.md) a [OWIN při spuštění třída detekce](owin-startup-class-detection.md) než si přečtete tohoto kurzu. V tomto kurzu byla zapsána od Ricka Andersona ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Jan Ross Praburaj Thiagarajan a Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).


I když [OWIN](an-overview-of-project-katana.md) komponenty middlewaru (OMCs) jsou primárně určený ke spouštění v kanálu bez ohledu na serveru, je možné ke spuštění v integrovaném kanálu IIS také OMC (**klasickém režimu se *není* podporované**). OMC můžete provedeny fungovat v integrovaném kanálu IIS nainstalováním následující balíček z konzoly Správce balíčku (PMC):

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample1.cmd)]

To znamená, že všechny aplikace rozhraní, i ty, které ještě nejsou možné spouštět mimo službu IIS a System.Web, můžete těžit z existující komponenty middlewaru OWIN. 

> [!NOTE]
> Všechny `Microsoft.Owin.Security.*` balíčky přesouvání pomocí nové systém identit v sadě Visual Studio 2013 (například: soubory cookie, Account Microsoft, Google, Facebook, Twitter, [nosného tokenu](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html), OAuth, server ověřování, JWT, Azure Active adresář a služby Active directory federation services) jsou vytvořené jako OMCs a mohou být používány scénáře vlastním hostováním i hostované službou IIS.

## <a name="how-owin-middleware-executes-in-the-iis-integrated-pipeline"></a>Jak middlewaru OWIN, který spouští v integrovaném kanálu služby IIS

Pro konzolové aplikace OWIN, vytvořený kanál aplikací pomocí [konfiguraci spuštění](owin-startup-class-detection.md) je nastaven podle pořadí součásti jsou přidány pomocí `IAppBuilder.Use` metoda. To znamená, kanál OWIN v [Katana](an-overview-of-project-katana.md) runtime zpracuje OMCs v pořadí, by měla zaregistrováno pomocí `IAppBuilder.Use`. V integrovaném kanálu IIS kanál požadavku se skládá z [HttpModules](https://msdn.microsoft.com/library/ms178468(v=vs.85).aspx) předplatné předem definované sadě událostí kanálu jako [BeginRequest](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx), [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), [AuthorizeRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx)atd.

Pokud jsme porovnávat OMC k u [modulu HTTP](https://msdn.microsoft.com/library/zec9k340(v=vs.85).aspx) na světě ASP.NET, musí být zaregistrovaný OMC správné předem definované kanál události. Například modulu HTTP `MyModule` bude získat volána, když požadavek je teď dostupná [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) fáze v kanálu:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample2.cs?highlight=10)]

OMC zapojit se do této stejné, na základě událostí spouštění řazení, aby [Katana](an-overview-of-project-katana.md) kód při spuštění prohledá prostřednictvím [konfiguraci spuštění](owin-startup-class-detection.md) a přihlásí se k odběru všechny komponenty middlewaru pro událost integrovaného kanálu. Například následující kód OMC a registrace vám umožní zobrazit výchozí událost registraci komponenty middlewaru. (Podrobné pokyny pro vytvoření třídy pro spuštění OWIN, najdete v části [OWIN při spuštění třída detekce](owin-startup-class-detection.md).)

1. Vytvořte projekt aplikace prázdný web s názvem **owin2**.
2. Z balíček správce konzoly (pomocí PMC), spusťte následující příkaz: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample3.cmd)]
3. Přidat `OWIN Startup Class` a pojmenujte ji `Startup`. Generovaného kódu nahraďte následujícím (změny se zvýrazněnou):  

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample4.cs?highlight=5-7,15-36)]
4. Stiskněte F5 a spusťte aplikaci.

Konfigurace spuštění nastaví kanál pomocí komponenty tři middlewaru, první dvě zobrazení diagnostických informací a naposledy reagování na události (a také zobrazit diagnostické informace). `PrintCurrentIntegratedPipelineStage` Metoda zobrazí události integrovaného kanálu je volána, tento middleware a zprávu. Výstup windows zobrazí následující informace:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample5.cmd)]

Modul runtime Katana namapované všechny komponenty middlewaru OWIN k [PreExecuteRequestHandler](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx) ve výchozím nastavení, která odpovídá událostí kanálu IIS [PreRequestHandlerExecute](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx).

## <a name="stage-markers"></a>Fáze značek

Můžete označit OMCs provést na konkrétní fázemi kanálu pomocí `IAppBuilder UseStageMarker()` metoda rozšíření. Chcete-li spustit sadu komponenty middlewaru během konkrétní fáze, vložit značky fází hned po poslední součást je sada během registrace. Existují pravidla, na které fázi kanálu můžete provést middleware a je nutné spustit komponenty pořadí (pravidla jsou vysvětleny později v tomto kurzu). Přidat `UseStageMarker` metodu `Configuration` code, jak je uvedeno níže:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample6.cs?highlight=13,19)]

`app.UseStageMarker(PipelineStage.Authenticate)` Volání nakonfiguruje všechny komponenty middlewaru dříve zaregistrovaný (v tomto případě naše dvě diagnostiky součásti) ke spuštění na fázi ověřování kanálu. Poslední komponenta middlewaru, (která zobrazí diagnostiky a reaguje na požadavky) se spustí na `ResolveCache` fázi ( [ResolveRequestCache](https://msdn.microsoft.com/library/system.web.httpapplication.resolverequestcache.aspx) událostí).

Stiskněte F5 a spusťte aplikaci. Ve výstupním okně zobrazí následující:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample7.cmd)]

## <a name="stage-marker-rules"></a>Fáze značky pravidla

Komponenty middlewaru Owin (OMC) může být nakonfigurována pro spuštění v následující události fáze kanálu OWIN:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample8.cs)]

1. Ve výchozím nastavení spustí OMCs na poslední událost (`PreHandlerExecute`). To je důvod, proč naše první ukázkový kód zobrazí "PreExecuteRequestHandler".
2. Můžete použít `app.UseStageMarker` metody pro registraci OMC spustit dříve, v jakékoli fázi kanálu OWIN uvedené v `PipelineStage` výčtu.
3. Kanál OWIN a IIS kanálu je seřadit, proto volání `app.UseStageMarker` musí být v pořadí. Obslužné rutiny události nelze nastavit na událost, která předchází poslední událost, které jsou registrovány k `app.UseStageMarker`. Například *po* volání:

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample9.cmd)]

   volání `app.UseStageMarker` předávání `Authenticate` nebo `PostAuthenticate` nebudou přijmout, a bude vyvolána žádná výjimka. Spustit v poslední fázi, který ve výchozím nastavení je OMCs `PreHandlerExecute`. Fáze značek jsou používány k zobrazení, aby se spouštěly dříve. Pokud zadáte fázi značek mimo pořadí, jsme zaokrouhlit na dřívější značky. Jinými slovy přidání značka fáze, říká "Spustit později než fáze X". OMC na spuštění v nejdřívější značky fází po je přidán do kanálu OWIN.
4. Nejdřívější fáze volání `app.UseStageMarker` wins. Například, pokud jste přešli pořadí `app.UseStageMarker` volání z našich předchozí příklad:

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample10.cs?highlight=13,19)]

   Ve výstupním okně se zobrazí: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample11.cmd)]

   Všechny spuštění v OMCs `AuthenticateRequest` dvoufázové instalace, protože poslední OMC zaregistrovaná pomocí `Authenticate` událostí a `Authenticate` událostí předchází všechny ostatní události.
