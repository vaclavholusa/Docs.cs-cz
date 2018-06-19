---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Testování řadiče v rozhraní ASP.NET Web API 2 částí | Microsoft Docs
author: MikeWasson
description: Toto téma popisuje některé konkrétní postupy pro testování řadiče ve webovém rozhraní API 2 částí. Než si přečtete Toto téma, můžete chtít přečíst kurz jednotky...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/11/2014
ms.topic: article
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: bda5148a4c1553d70f3173de66371fbb8576e83f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
ms.locfileid: "28039541"
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a>Testování řadiče v rozhraní ASP.NET Web API 2 částí
====================
podle [Wasson Jan](https://github.com/MikeWasson)

> Toto téma popisuje některé konkrétní postupy pro testování řadiče ve webovém rozhraní API 2 částí. Než si přečtete Toto téma, můžete chtít přečíst kurz [jednotky testování webové rozhraní API 2 ASP.NET](unit-testing-with-aspnet-web-api.md), který ukazuje, jak přidat projekt testování částí pro vaše řešení.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Webové rozhraní API 2
> - [Moq](https://github.com/Moq) 4.5.30

> [!NOTE]
> Mohu použít Moq, ale na stejné nápad platí pro všechny mocking framework. Moq 4.5.30 (a novější) podporuje Visual Studio 2017, Roslyn a rozhraní .NET 4.5 a novější verze.

Běžný vzor při testech jednotek je &quot;uspořádat, Application Compatibility Toolkit, assert&quot;:

- Uspořádat: Připravit všechny předpoklady pro test ke spuštění.
- AKT: Proveďte test.
- Assert –: Ověřte, zda test proběhla úspěšně.

V kroku uspořádat budou často používat model nebo objekty zóny se zakázaným inzerováním. Test se zaměřuje na testování jednou z věcí, které minimalizuje počet závislostí.

Zde jsou některé věci, které byste měli testování částí v řadičích webového rozhraní API:

- Akce vrátí správný typ odpovědi.
- Neplatné parametry vrátí odpověď opravte chyby.
- Akce volá metodu správné ve vrstvě úložiště nebo službě.
- Pokud odpověď obsahuje modelu domény, zkontrolujte typ modelu.

Toto jsou některé obecné věcí, které chcete otestovat, ale jsou specifikace závisí na implementaci řadiče. Zejména, zda vaše akce kontroleru vrátí umožňuje velký rozdíl **objekt HttpResponseMessage** nebo **IHttpActionResult**. Další informace o těchto typů výsledků najdete v tématu [výsledky akce ve webovém rozhraní Api 2](../getting-started-with-aspnet-web-api/action-results.md).

## <a name="testing-actions-that-return-httpresponsemessage"></a>Testování akcí, které vracejí objekt HttpResponseMessage

Tady je příklad řadiče jehož akce vrátí **objekt HttpResponseMessage**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

Upozornění kontroleru pomocí vkládání závislostí vložit `IProductRepository`. Který zpřístupňuje řadičem více možností intenzivního testování, protože můžete vložit imitované úložiště. Následující testování částí ověřuje, že `Get` metoda zápisy `Product` na text odpovědi. Předpokládáme, že `repository` je model `IProductRepository`.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

Je důležité nastavit **požadavku** a **konfigurace** na řadiči. V opačném případě, test se nezdaří s **ArgumentNullException** nebo **InvalidOperationException**.

## <a name="testing-link-generation"></a>Testování generování odkazů

`Post` Volání metod **UrlHelper.Link** k vytváření propojení v odpovědi. To vyžaduje trochu další instalační program v testování částí:

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

**UrlHelper** třída potřebuje data adresy URL a trasy na žádost, takže testovací má nastavit hodnoty pro tyto. Další možností je model nebo se zakázaným inzerováním **UrlHelper**. S tímto přístupem, nahradí výchozí hodnotu [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) s model nebo se zakázaným inzerováním verzí, která vrátí pevnou hodnotu.

Pojďme přepisování testu pomocí [Moq](https://github.com/Moq) framework. Nainstalujte `Moq` balíček NuGet do projektu test.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

V této verzi, nemusíte nastavit žádná data trasy, protože model **UrlHelper** vrátí konstantní řetězec.


## <a name="testing-actions-that-return-ihttpactionresult"></a>Testování akcí, které vracejí IHttpActionResult

Ve webovém rozhraní API 2 akce kontroleru vrátit **IHttpActionResult**, což je obdobou **ActionResult** v architektuře ASP.NET MVC. **IHttpActionResult** rozhraní definuje příkaz vzor pro vytváření odpovědí HTTP. Místo vytváření odpovědi přímo, kontroleru vrátí **IHttpActionResult**. Později, vyvolá kanál **IHttpActionResult** k vytvoření odpovědi. Tento přístup je snazší zápis testů jednotek, protože můžete přeskočit spoustu instalační program, který je potřebný pro **objekt HttpResponseMessage**.

Tady je příklad controller jehož akce vrátí **IHttpActionResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

Tento příklad ukazuje několik běžných vzorů pomocí **IHttpActionResult**. Podívejme se, jak k jednotce otestovat je.

### <a name="action-returns-200-ok-with-a-response-body"></a>Akce vrátí hodnotu 200 (OK) se text odpovědi

`Get` Volání metod `Ok(product)` Pokud je nalezena produktu. Ujistěte se, je návratový typ v testu jednotek **OkNegotiatedContentResult** a vrácené výrobek má správné ID.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

Všimněte si, že testování částí neprovede výsledku akce. Můžete předpokládat, že výsledek akce správně vytvoří odpověď HTTP. (To je důvod, proč má svou vlastní testování částí rozhraní Web API!)

### <a name="action-returns-404-not-found"></a>Akce vrátí 404 (Nenalezeno)

`Get` Volání metod `NotFound()` Pokud není nalezena produktu. Jednotka pro tento případ, otestovat jenom kontroly, pokud je návratový typ **NotFoundResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a>Akce vrátí hodnotu 200 (OK) se žádný text odpovědi

`Delete` Volání metod `Ok()` vrátit prázdnou odpověď HTTP 200. Podobně jako v předchozím příkladu testování částí kontroluje návratový typ v tomto případě **OkResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a>Akce vrátí 201 (vytvořeno) s hlavičku umístění

`Post` Volání metod `CreatedAtRoute` vrátit odpověď HTTP 201 s identifikátorem URI v hlavičce umístění. V testu jednotek ověřte, že akce nastaví správné směrování hodnoty.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a>Akce vrátí jiné 2xx s text odpovědi

`Put` Volání metod `Content` vrátit odpověď HTTP 202 (platných) s text odpovědi. Tento případ se podobá vrací 200 (OK), ale testování částí byste taky měli zkontrolovat stavový kód.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a>Další prostředky

- [Rozhraní Entity Framework mocking při rozhraní ASP.NET Web API 2 testování částí](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- [Zápis testů pro služby ASP.NET Web API](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (příspěvek na blogu podle Youssef Moussaoui).
- [Ladění v rozhraní ASP.NET Web API pomocí ladicího programu trasy](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
