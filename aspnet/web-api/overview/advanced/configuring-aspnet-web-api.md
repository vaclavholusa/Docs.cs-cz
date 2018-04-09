---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: Konfigurace rozhraní ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: de2396710fb9434c84bf14a2faa37b98154f34d8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="configuring-aspnet-web-api-2"></a>Konfigurace rozhraní ASP.NET Web API 2
====================
podle [Wasson Jan](https://github.com/MikeWasson)

Toto téma popisuje postup konfigurace webového rozhraní API ASP.NET.

- [Nastavení konfigurace](#settings)
- [Konfigurace webového rozhraní API s hostování prostředí ASP.NET](#webhost)
- [Konfiguraci webového rozhraní API pomocí vlastní hostování OWIN](#selfhost)
- [Globální webové rozhraní API služby](#services)
- [Konfigurace Kontroleru](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a>Nastavení konfigurace

Nastavení konfigurace webového rozhraní API jsou definovány v [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) třídy.

| Člen | Popis |
| --- | --- |
| **Překladače závislostí** | Umožňuje vkládání závislostí pro řadiče. V tématu [pomocí překladače závislostí webového rozhraní API](dependency-injection.md). |
| **Filtry** | Filtry akcí. |
| **Formátovací moduly** | [Formátovací moduly typu média](../formats-and-model-binding/media-formatters.md). |
| **IncludeErrorDetailPolicy** | Určuje, zda server by měla obsahovat podrobnosti o chybě, například zprávy o výjimkách nebo trasování zásobníku zpráv odpovědí HTTP. V tématu [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)). |
| **Inicializátor** | Funkci, která provede konečnou inicializaci **HttpConfiguration**. |
| **MessageHandlers** | [Obslužné rutiny zpráv HTTP](http-message-handlers.md). |
| **ParameterBindingRules** | Kolekce pravidel pro vazby parametrů na akce kontroleru. |
| **Vlastnosti** | Obecná vlastnost kontejneru. |
| **Trasy** | Kolekce tras. V tématu [směrování v rozhraní ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md). |
| **Služby** | Kolekce služeb. V tématu [služby](#services). |


## <a name="prerequisites"></a>Požadavky

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional nebo Enterprise Edition.

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a>Konfigurace webového rozhraní API s hostování prostředí ASP.NET

V aplikaci ASP.NET, nakonfigurujte rozhraní Web API voláním [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) v **aplikace\_spustit** metoda. **Konfigurace** metoda přebírá delegáta pomocí jediného parametru typu **HttpConfiguration**. Všechny vaše konfiguračním uvnitř delegát proveďte.

Tady je příklad použití delegáta anonymní:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

V aplikaci Visual Studio 2017 šablona projektu "webové aplikace ASP.NET" automaticky nastaví kód konfigurace, pokud zvolíte možnost "Webového rozhraní API" v **nový projekt ASP.NET** dialogové okno.

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

Šablona projektu vytvoří soubor s názvem WebApiConfig.cs uvnitř aplikace\_spouštěcí složka. Tento soubor kód definuje delegáta, kde byste měli umístit kódu webového rozhraní API konfigurace.

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

Šablona projektu také přidá kód, který vyvolá delegáta z **aplikace\_spustit**.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a>Konfiguraci webového rozhraní API pomocí vlastní hostování OWIN

Pokud jste samoobslužné hostování s OWIN, vytvořte novou **HttpConfiguration** instance. V této instanci provádět žádnou konfiguraci a poté předat instanci systému na **Owin.UseWebApi** metoda rozšíření.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

Tento kurz [OWIN použití Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) ukazuje úplné kroky.

<a id="services"></a>
## <a name="global-web-api-services"></a>Globální webové rozhraní API služby

**HttpConfiguration.Services** kolekce obsahuje sadu služeb global services, která webového rozhraní API používá k provádění různých úloh, jako jsou řadiče výběr a vyjednávání obsahu.

> [!NOTE]
> **Služby** kolekce není pro obecné účely mechanismus pro službu zjišťování nebo závislost vkládání. Ukládá pouze typy služeb, které jsou známé rozhraní Web API.


**Služby** kolekce je inicializována s výchozí sadou služeb a může poskytnout vlastní vlastních implementací. Některé služby podporuje víc instancí, zatímco ostatní může mít pouze jednu instanci. (Však také poskytuje služby na úrovni kontroleru; viz [-Controller konfigurace](#percontrollerconfig).

Jednou instancí služby


| Služba | Popis |
| --- | --- |
| **IActionValueBinder** | Získá vazbu pro parametr. |
| **IApiExplorer** | Získá popisy rozhraní API vystavené aplikace. V tématu [vytváření stránky nápovědy pro webové rozhraní API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IAssembliesResolver** | Získá seznam sestavení pro aplikaci. V tématu [směrování a výběr akce](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IBodyModelValidator** | Ověří model, který je číst z textu žádosti formátovací modul typu média. |
| **IContentNegotiator** | Provede vyjednávání obsahu. |
| **IDocumentationProvider** | Poskytuje dokumentaci k rozhraní API. Výchozí hodnota je **null**. V tématu [vytváření stránky nápovědy pro webové rozhraní API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IHostBufferPolicySelector** | Určuje, zda by měl hostitel vyrovnávací paměti obsah entity zprávy HTTP. |
| **IHttpActionInvoker** | Vyvolá akce kontroleru. V tématu [směrování a výběr akce](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpActionSelector** | Vybere akci kontroleru. V tématu [směrování a výběr akce](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerActivator** | Aktivuje řadiči. V tématu [směrování a výběr akce](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerSelector** | Vybere kontroler. V tématu [směrování a výběr akce](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerTypeResolver** | Obsahuje seznam typů kontroleru webového rozhraní API v aplikaci. V tématu [směrování a výběr akce](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **ITraceManager** | Inicializuje rámce pro trasování. V tématu [trasování v rozhraní ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **ITraceWriter** | Poskytuje zapisovač trasování. Výchozí hodnota je zapisovač trasování "no-op". V tématu [trasování v rozhraní ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **IModelValidatorCache** | Poskytuje mezipaměť validátory modelu. |

Více instancí služby


|                 Služba                 |                                                                                                              Popis                                                                                                               |
|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <strong>IFilterProvider</strong>     |                                                                                           Vrátí seznam filtrů pro akce kontroleru.                                                                                           |
|  <strong>ModelBinderProvider</strong>   |                                                                                                Vrátí vazač modelu pro daného typu.                                                                                                |
| <strong>ModelMetadataProvider</strong>  |                                                                                                     Poskytuje metadata pro model.                                                                                                     |
| <strong>ModelValidatorProvider</strong> |                                                                                                   Poskytuje validátor pro model.                                                                                                    |
|  <strong>ValueProviderFactory</strong>  | Vytvoří zprostředkovatele hodnot. Další informace najdete v tématu Karel místo blogu [vytvoření zprostředkovatele vlastní hodnoty v WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx) |

Chcete-li přidat vlastní implementaci služby s více instancemi, volejte **přidat** nebo **vložit** na **služby** kolekce:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

Pokud chcete nahradit vlastní implementaci jedné instance služby, volání **nahradit** na **služby** kolekce:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a>Konfigurace Kontroleru

Můžete přepsat na základě-controller následující nastavení:

- Formátovací moduly typu média
- Parametr vazby pravidla
- Služby

Uděláte to tak definovat vlastní atribut, který implementuje **IControllerConfiguration** rozhraní. Pak použijte atribut kontroleru.

Následující příklad nahradí formátovací moduly typu média výchozí vlastní formátování.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

**IControllerConfiguration.Initialize** metoda přebírá dva parametry:

- **HttpControllerSettings** objektu
- **HttpControllerDescriptor** objektu

**HttpControllerDescriptor** obsahuje popis řadiči, který můžete zkontrolovat pro informační účely (k rozlišení dvou řadičích indikované).

Použití **HttpControllerSettings** objekt, který chcete nakonfigurovat kontroleru. Tento objekt obsahuje podmnožinu konfigurační parametry, které je možné přepsat na základě-controller. Všechna nastavení, která Neměnit výchozí na globální **HttpConfiguration** objektu.
