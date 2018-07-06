---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Výsledky akcí ve webovém rozhraní API 2 | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 02/03/2014
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: 74a837cb9f606d78fb516968343d498d3c37c4ed
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808924"
---
<a name="action-results-in-web-api-2"></a>Výsledky akcí ve webovém rozhraní API 2
====================
podle [Mike Wasson](https://github.com/MikeWasson)

Toto téma popisuje, jak rozhraní ASP.NET Web API převede návratovou hodnotu z akce kontroleru do zprávy s odpovědí HTTP.

Akce kontroleru webového rozhraní API může vrátit kterýkoli z následujících:

1. void
2. **Objekt HttpResponseMessage**
3. **IHttpActionResult**
4. Jiný typ

Podle toho, která z nich je vrácena, webové rozhraní API k vytvoření odpovědi HTTP používá jiný mechanismus.

| Návratový typ | Způsob, jak vytvořit webové rozhraní API odpovědi |
| --- | --- |
| void | Vrátí prázdný 204 (žádný obsah) |
| **Objekt HttpResponseMessage** | Převeďte přímo na zprávy s odpovědí HTTP. |
| **IHttpActionResult** | Volání **ExecuteAsync** k vytvoření **objekt HttpResponseMessage**, převeďte do zprávy s odpovědí HTTP. |
| Jiný typ | Zápis serializovaná návratovou hodnotu do datové části odpovědi; Vrátí 200 (OK). |

Zbývající část tohoto tématu popisuje jednotlivé možnosti podrobněji.

## <a name="void"></a>void

Pokud je návratový typ `void`, webové rozhraní API jednoduše vrátí prázdnou odpověď HTTP se stavovým kódem 204 (žádný obsah).

Příklad kontroleru:

[!code-csharp[Main](action-results/samples/sample1.cs)]

Odpověď HTTP:

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a>Objekt HttpResponseMessage

Pokud tato akce vrátí [objekt HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), webového rozhraní API převede návratovou hodnotu přímo do zprávu odpovědi HTTP pomocí vlastnosti **objekt HttpResponseMessage** objekt pro naplnění odpověď.

Tato možnost poskytuje velké množství kontrolu nad zprávy s odpovědí. Například následující akce kontroleru nastaví hlavičku Cache-Control.

[!code-csharp[Main](action-results/samples/sample3.cs)]

Odpověď:

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

Pokud předáte do modelu domény **CreateResponse** metodu, pomocí webového rozhraní API [formátovací modul médií](../formats-and-model-binding/media-formatters.md) zapsat serializovaný model do datové části odpovědi.

[!code-csharp[Main](action-results/samples/sample5.cs)]

Webové rozhraní API používá hlavičky Accept v požadavku k výběru formátovacího modulu. Další informace najdete v tématu [vyjednávání obsahu](../formats-and-model-binding/content-negotiation.md).

## <a name="ihttpactionresult"></a>IHttpActionResult

**IHttpActionResult** rozhraní byl zaveden ve webovém rozhraní API 2. V podstatě definuje **objekt HttpResponseMessage** objekt pro vytváření. Tady jsou některé výhody použití **IHttpActionResult** rozhraní:

- Zjednodušuje [testování částí](../testing-and-debugging/unit-testing-controllers-in-web-api.md) řadičů.
- Přesune běžné logiku pro vytváření odpovědí HTTP na samostatné třídy.
- Díky záměr jasnější, akce kontroleru skrytím nízké úrovně podrobnosti o vytváření odpovědi.

**IHttpActionResult** obsahuje jedinou metodu **ExecuteAsync**, který asynchronně vytvoří **objekt HttpResponseMessage** instance.

[!code-csharp[Main](action-results/samples/sample6.cs)]

Akce kontroleru vrátí-li **IHttpActionResult**, volá webové rozhraní API **ExecuteAsync** metodu pro vytvoření **objekt HttpResponseMessage**. Potom převede **objekt HttpResponseMessage** do zprávy s odpovědí HTTP.

Tady je jednoduchý implementaton z **IHttpActionResult** , který vytváří ve formátu prostého textu odpovědi:

[!code-csharp[Main](action-results/samples/sample7.cs)]

Příklad akce kontroleru:

[!code-csharp[Main](action-results/samples/sample8.cs)]

Odpověď:

[!code-console[Main](action-results/samples/sample9.cmd)]

Častěji, použije **IHttpActionResult** implementace, které jsou definovány v **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** oboru názvů. **Objektu ApiController** třída definuje pomocné metody, které vracejí výsledky těchto předdefinovaných akcí.

V následujícím příkladu, pokud požadavku se neshoduje s ID existujícího produktu, zavolá kontroleru [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) pro vytvoření odpovědi 404 (Nenalezeno). Jinak kontroler volá [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), vytváří se odpověď 200 (OK), který obsahuje produktu.

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a>Jiné návratové typy

Pro všechny ostatní návratové typy webového rozhraní API používá [formátovací modul médií](../formats-and-model-binding/media-formatters.md) k serializaci návratovou hodnotu. Webové rozhraní API zapíše serializované hodnoty do datové části odpovědi. Stavový kód odpovědi je 200 (OK).

[!code-csharp[Main](action-results/samples/sample11.cs)]

Nevýhody tohoto přístupu je, že se nedá vrátit přímo kód chyby, jako je například 404. Však může vrátit **HttpResponseException** pro kódy chyb. Další informace najdete v tématu [zpracování výjimek v rozhraní ASP.NET Web API](../error-handling/exception-handling.md).

Webové rozhraní API používá hlavičky Accept v požadavku k výběru formátovacího modulu. Další informace najdete v tématu [vyjednávání obsahu](../formats-and-model-binding/content-negotiation.md).

Příklad žádosti

[!code-console[Main](action-results/samples/sample12.cmd)]

Příklad odpovědi:

[!code-console[Main](action-results/samples/sample13.cmd)]
