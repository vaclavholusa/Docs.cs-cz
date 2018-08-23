---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: Konfigurace rozhraní ASP.NET Web API 2 | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: riande
ms.date: 03/31/2014
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 81119b35fb375bb2299edc4f08289a89aecfcd88
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755295"
---
<a name="configuring-aspnet-web-api-2"></a>Konfigurace rozhraní ASP.NET Web API 2
====================
podle [Mike Wasson](https://github.com/MikeWasson)

Toto téma popisuje postup konfigurace webového rozhraní API ASP.NET.

- [Nastavení konfigurace](#settings)
- [Konfigurace webového rozhraní API s hostování v technologii ASP.NET](#webhost)
- [Konfigurace webového rozhraní API s vlastním hostováním OWIN](#selfhost)
- [Globální webové rozhraní API služby](#services)
- [Konfigurace na Kontroleru](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a>Nastavení konfigurace

Nastavení konfigurace webového rozhraní API jsou definovány v [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) třídy.

| Člen | Popis |
| --- | --- |
| **Překladače závislostí** | Povolí vkládání závislostí pro řadiče. Zobrazit [použitím překladač závislostí webového rozhraní API](dependency-injection.md). |
| **Filtry** | Filtry akcí. |
| **Formátovací moduly** | [Formátovací moduly typu médií](../formats-and-model-binding/media-formatters.md). |
| **IncludeErrorDetailPolicy** | Určuje, zda server by měl obsahovat podrobnosti o chybě, například zprávy o výjimkách nebo trasování zásobníku v odpovědích HTTP. Zobrazit [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)). |
| **Inicializátor** | Funkce, která provede konečnou inicializaci **HttpConfiguration**. |
| **MessageHandlers** | [Obslužné rutiny zpráv HTTP](http-message-handlers.md). |
| **ParameterBindingRules** | Kolekce pravidel pro vazby parametrů na akce kontroleru. |
| **Vlastnosti** | Obecná vlastnost kontejneru. |
| **Trasy** | Kolekce tras. Zobrazit [směrování v rozhraní ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md). |
| **Služby** | Kolekce služeb. Zobrazit [služby](#services). |


## <a name="prerequisites"></a>Požadavky

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional nebo Enterprise Edition.

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a>Konfigurace webového rozhraní API s hostování v technologii ASP.NET

V aplikaci technologie ASP.NET, nakonfigurujte rozhraní Web API voláním [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) v **aplikace\_Start** metody. **Konfigurovat** metoda přijímá delegát s jedním parametrem typu **HttpConfiguration**. Proveďte všechny vaše konfiguračním uvnitř delegáta.

Tady je příklad použití anonymního delegáta:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

V sadě Visual Studio 2017 "Webovou aplikaci ASP.NET" Šablona projektu automaticky nastaví kód konfigurace-li možnost "Webového rozhraní API" v **nový projekt ASP.NET** dialogového okna.

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

Šablona projektu vytvoří soubor s názvem WebApiConfig.cs uvnitř aplikace\_spouštěcí složka. Tento soubor kódu definuje delegáta, ve kterém byste měli umístit kódu konfigurace webového rozhraní API.

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

Šablona projektu také přidá kód, který volá delegáta z **aplikace\_Start**.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a>Konfigurace webového rozhraní API s vlastním hostováním OWIN

Pokud jste samoobslužné hostování s OWIN, vytvořte nový **HttpConfiguration** instance. Provádět žádnou konfiguraci v této instanci a pak předejte instanci **Owin.UseWebApi** – metoda rozšíření.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

Tento kurz [použití OWIN Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) ukazuje kompletní postup.

<a id="services"></a>
## <a name="global-web-api-services"></a>Globální webové rozhraní API služby

**HttpConfiguration.Services** kolekce obsahuje sadu služeb global services, které webové rozhraní API používá k provádění různých úloh, jako je například řadič výběru a vyjednávání obsahu.

> [!NOTE]
> **Služby** kolekce není mechanismus pro obecné účely pro dokáže vložit službu zjišťování nebo závislost. Ukládá pouze typy služeb, které jsou známé rozhraní Web API.


**Služby** kolekce je inicializována s výchozí sadou služeb a může poskytnout vlastní vlastní implementace. Některé služby podporují více instancí, zatímco jiné můžou mít jenom jednu instanci. (Ale také může poskytovat služby na úrovni kontroleru; viz [konfigurace na Kontroleru](#percontrollerconfig).

Jednou instancí služby


| Služba | Popis |
| --- | --- |
| **IActionValueBinder** | Získá vazbu pro parametr. |
| **IApiExplorer** | Získá popisy rozhraní API aplikace. Zobrazit [vytváření stránek nápovědy webového rozhraní API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IAssembliesResolver** | Získá seznam sestavení pro aplikaci. Zobrazit [směrování a výběr akce](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IBodyModelValidator** | Ověří model, který je číst z textu požadavku formátovací modul typu média. |
| **IContentNegotiator** | Provede vyjednávání obsahu. |
| **IDocumentationProvider** | Poskytuje dokumentaci pro rozhraní API. Výchozí hodnota je **null**. Zobrazit [vytváření stránek nápovědy webového rozhraní API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IHostBufferPolicySelector** | Určuje, zda by měl hostitel vyrovnávací paměti obsah entity zprávy HTTP. |
| **IHttpActionInvoker** | Vyvolá akce kontroleru. Zobrazit [směrování a výběr akce](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpActionSelector** | Vybírá akci kontroleru. Zobrazit [směrování a výběr akce](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerActivator** | Aktivuje kontroleru. Zobrazit [směrování a výběr akce](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerSelector** | Vybere kontroler. Zobrazit [směrování a výběr akce](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerTypeResolver** | Obsahuje seznam typů kontroleru webového rozhraní API v aplikaci. Zobrazit [směrování a výběr akce](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **ITraceManager** | Inicializuje rámce pro trasování. Zobrazit [trasování v rozhraní ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **ITraceWriter** | Poskytuje zapisovač trasování. Výchozí hodnota je zápis trasování "no-op". Zobrazit [trasování v rozhraní ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **IModelValidatorCache** | Poskytuje mezipaměť validátorů modelů. |

Více instancí služby


|                 Služba                 |                                                                                                              Popis                                                                                                               |
|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <strong>IFilterProvider</strong>     |                                                                                           Vrátí seznam filtrů pro akce kontroleru.                                                                                           |
|  <strong>ModelBinderProvider</strong>   |                                                                                                Vrátí vazač modelu pro daného typu.                                                                                                |
| <strong>ModelMetadataProvider</strong>  |                                                                                                     Poskytuje metadata pro model.                                                                                                     |
| <strong>ModelValidatorProvider</strong> |                                                                                                   Poskytuje validátor modelu.                                                                                                    |
|  <strong>ValueProviderFactory</strong>  | Vytvoří zprostředkovatele hodnot. Další informace najdete v tématu Mike koutů blogovém příspěvku [vytvoření zprostředkovatele vlastní hodnoty v WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx) |

Chcete-li přidat vlastní implementaci na více instancí služby, zavolejte **přidat** nebo **vložit** na **služby** kolekce:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

Chcete-li nahradit vlastní implementaci služby jednou instancí, zavolejte **nahradit** na **služby** kolekce:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a>Konfigurace na Kontroleru

Můžete přepsat následující nastavení na základě na kontroleru:

- Formátovací moduly typu médií
- Parametr vazby pravidla
- Služby

Uděláte to tak, definujte vlastní atribut, který implementuje **IControllerConfiguration** rozhraní. Potom použijte atribut pro kontroler.

Následující příklad nahradí výchozí formátovací moduly typu médií vlastní formátovací modul.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

**IControllerConfiguration.Initialize** metoda přebírá dva parametry:

- **HttpControllerSettings** objektu
- **HttpControllerDescriptor** objektu

**HttpControllerDescriptor** obsahuje popis kontroler, který můžete prozkoumat k informačním účelům (například k rozlišení mezi dvěma řadiči).

Použití **HttpControllerSettings** objekt konfigurace kontroleru. Tento objekt obsahuje dílčí parametry konfigurace, které můžete přepsat na základě na kontroleru. Všechna nastavení, která Neměnit výchozí na globální **HttpConfiguration** objektu.
