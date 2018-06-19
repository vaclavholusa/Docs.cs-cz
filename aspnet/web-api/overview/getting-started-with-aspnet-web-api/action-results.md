---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Akce výsledků v rozhraní Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/03/2014
ms.topic: article
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: d0db5c6d45020861d7295ab1db989caee525fff9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036463"
---
<a name="action-results-in-web-api-2"></a>Výsledky akce v rozhraní Web API 2
====================
podle [Wasson Jan](https://github.com/MikeWasson)

Toto téma popisuje, jak rozhraní ASP.NET Web API převádí návratovou hodnotu z akce kontroleru do zprávy odpovědi HTTP.

Akce kontroleru webového rozhraní API se můžete vrátit některé z následujících:

1. void
2. **HttpResponseMessage**
3. **IHttpActionResult**
4. Jiný typ

V závislosti na tom, které z nich se vrátí, webového rozhraní API používá jiný mechanismus k vytvoření odpovědi HTTP.

| Návratový typ | Jak webového rozhraní API vytvoří odpovědi |
| --- | --- |
| void | Vrátí prázdný 204 (ne obsahu) |
| **HttpResponseMessage** | Převeďte přímo na zprávu odpovědi HTTP. |
| **IHttpActionResult** | Volání **ExecuteAsync** vytvořit **objekt HttpResponseMessage**, pak převést na zprávu odpovědi HTTP. |
| Jiný typ | Zápis serializovaných návratovou hodnotu do odpovědi; Vrátí 200 (OK). |

Zbývající část tohoto tématu popisuje jednotlivé možnosti podrobněji.

## <a name="void"></a>void

Pokud je návratový typ `void`, webového rozhraní API jednoduše vrátí prázdnou odpověď HTTP se stavovým kódem 204 (ne obsahu).

Příklad řadič:

[!code-csharp[Main](action-results/samples/sample1.cs)]

Odpověď HTTP:

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a>Objekt HttpResponseMessage

Akce vrátí-li [objekt HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), webového rozhraní API převede návratovou hodnotu přímo do zprávu odpovědi HTTP pomocí vlastnosti **objekt HttpResponseMessage** objektu k naplnění odpověď.

Tato možnost vám přináší značnou kontroly nad zprávu odpovědi. Například následující akce kontroleru nastaví hlavičku Cache-Control.

[!code-csharp[Main](action-results/samples/sample3.cs)]

Odpověď:

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

Pokud předáte modelu domény k **CreateResponse** metoda, používá webového rozhraní API [formátovací modul média](../formats-and-model-binding/media-formatters.md) k zápisu serializovaného modelu do text odpovědi.

[!code-csharp[Main](action-results/samples/sample5.cs)]

Webové rozhraní API pomocí hlavička Accept v požadavku formátování. Další informace najdete v tématu [vyjednávání obsahu](../formats-and-model-binding/content-negotiation.md).

## <a name="ihttpactionresult"></a>IHttpActionResult

**IHttpActionResult** rozhraní byla zavedena ve webovém rozhraní API 2. V podstatě definuje **objekt HttpResponseMessage** objekt pro vytváření. Tady jsou některé výhody používání **IHttpActionResult** rozhraní:

- Zjednodušuje [testování částí](../testing-and-debugging/unit-testing-controllers-in-web-api.md) řadičů.
- Přesune běžné logiku pro vytváření odpovědí HTTP do samostatné třídy.
- Díky záměr jasnější, akce kontroleru skrytím nízké úrovně podrobnosti o vytváření odpovědi.

**IHttpActionResult** obsahuje jedinou metodu **ExecuteAsync**, který asynchronně vytvoří **objekt HttpResponseMessage** instance.

[!code-csharp[Main](action-results/samples/sample6.cs)]

Akce kontroleru vrátí-li **IHttpActionResult**, volání webového rozhraní API **ExecuteAsync** metodu pro vytvoření **objekt HttpResponseMessage**. Potom převede ji **objekt HttpResponseMessage** do zprávy odpovědi HTTP.

Zde je jednoduchý implementaton z **IHttpActionResult** vytvářející Prostý text odpovědi:

[!code-csharp[Main](action-results/samples/sample7.cs)]

Akce kontroleru příklad:

[!code-csharp[Main](action-results/samples/sample8.cs)]

Odpověď:

[!code-console[Main](action-results/samples/sample9.cmd)]

Více často, bude používat **IHttpActionResult** implementace, které jsou definované v  **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)**  oboru názvů. **Objektu ApiController** třída definuje pomocné metody, které vracejí výsledky těchto vestavěná akce.

V následujícím příkladu, pokud požadavek neodpovídá stávající ID produktu, zavolá řadičem [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) k vytvoření odpovědi 404 (není nalezena). Jinak hodnota řadičem volá [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), která vytvoří odpovědi 200 (OK), obsahuje produktu.

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a>Ostatní návratové typy

Pro všechny ostatní návratové typy webového rozhraní API používá [formátovací modul média](../formats-and-model-binding/media-formatters.md) k serializaci návratovou hodnotu. Webové rozhraní API zapíše serializovaná hodnota do text odpovědi. Stavový kód odpovědi je 200 (OK).

[!code-csharp[Main](action-results/samples/sample11.cs)]

Nevýhodou tohoto přístupu je, že nelze vrátit přímo kód chyby, jako je například 404. Ale můžete vyvolat **HttpResponseException** pro kódy chyb. Další informace najdete v tématu [zpracování výjimek v rozhraní ASP.NET Web API](../error-handling/exception-handling.md).

Webové rozhraní API pomocí hlavička Accept v požadavku formátování. Další informace najdete v tématu [vyjednávání obsahu](../formats-and-model-binding/content-negotiation.md).

Příklad požadavku

[!code-console[Main](action-results/samples/sample12.cmd)]

Příklad odpovědi:

[!code-console[Main](action-results/samples/sample13.cmd)]
