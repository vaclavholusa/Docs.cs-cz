---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Testování jednotek Kontrolerů v rozhraní ASP.NET Web API 2 | Dokumentace Microsoftu
author: MikeWasson
description: Toto téma popisuje některé konkrétní postupy pro testování jednotek kontrolerů webového rozhraní API 2. Před čtením tohoto tématu, můžete chtít přečtěte si kurz jednotky...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/11/2014
ms.topic: article
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 1a3cfa1962a5f914fd2393088bec4424f6453d07
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389380"
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a>Testování jednotek Kontrolerů v rozhraní ASP.NET Web API 2
====================
podle [Mike Wasson](https://github.com/MikeWasson)

> Toto téma popisuje některé konkrétní postupy pro testování jednotek kontrolerů webového rozhraní API 2. Před čtením tohoto tématu, můžete chtít přečtěte si kurz [jednotky testování ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), který ukazuje, jak přidat projekt testování částí do vašeho řešení.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Webové rozhraní API 2
> - [Moq](https://github.com/Moq) 4.5.30

> [!NOTE]
> Můžu použít Moq, ale platí stejné nápad pro libovolné napodobování architektury. Moq 4.5.30 (a vyšší) podporuje Visual Studio 2017, Roslyn a .NET 4.5 a novější verze.

Běžným vzorem při testech jednotek je &quot;uspořádat, act, vyhodnocení&quot;:

- Uspořádat: Nastavte všechny požadované součásti pro test spuštěn.
- ACT: Provádění testu.
- Vyhodnocení: Ověřte, že test bylo úspěšné.

V kroku uspořádat budou často používat model nebo zástupné objekty. Které minimalizuje počet závislostí, takže test se zaměřuje na testování jednou z věcí.

Zde jsou některé kroky ve vašich kontrolerech webového rozhraní API by měl test jednotek:

- Tato akce vrátí správný typ odpovědi.
- Neplatné parametry vrátit odpověď chyby opravte.
- Akce volá metodu správné ve vrstvě úložiště nebo službě.
- Pokud odpověď obsahuje model domény, zkontrolujte typ modelu.

Toto jsou některé obecné možnosti testování, ale specifika závisí na vaší implementace kontroleru. Zejména je velký rozdíl, jestli vaše akce kontroleru vrátit **objekt HttpResponseMessage** nebo **IHttpActionResult**. Další informace o těchto typů výsledků najdete v tématu [výsledky akcí ve webovém rozhraní Api 2](../getting-started-with-aspnet-web-api/action-results.md).

## <a name="testing-actions-that-return-httpresponsemessage"></a>Testování akcí, které vracejí objekt HttpResponseMessage

Tady je příklad řadiče jehož akcí vrácení **objekt HttpResponseMessage**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

Všimněte si, že kontroler používá injektáž závislostí vkládat `IProductRepository`. Díky tomu se kontroler více možností intenzivního testování, protože můžete vložit mock úložiště. Následující test jednotky ověřuje, že `Get` metoda zápisy `Product` do těla odpovědi. Předpokládejme, že `repository` je model `IProductRepository`.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

Je důležité nastavit **žádosti** a **konfigurace** na kontroleru. V opačném případě test se nezdaří pomocí **ArgumentNullException** nebo **InvalidOperationException**.

## <a name="testing-link-generation"></a>Testování generování odkazů

`Post` Volání metody **UrlHelper.Link** k vytvoření odkazů v odpovědi. To vyžaduje trochu další nastavení v testu jednotek:

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

**UrlHelper** třídy musí žádost o adresu URL a data trasy, aby test má k nastavení hodnot pro tyto. Další možností je model nebo zástupné procedury **UrlHelper**. S tímto přístupem nahradíte na výchozí hodnotu [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) s verzí model nebo zástupné procedury, která vrací pevná hodnota.

Umožňuje přepsat pomocí testu [Moq](https://github.com/Moq) rozhraní framework. Nainstalujte `Moq` NuGet balíčku v testovém projektu.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

V této verzi není nutné nastavit všechna data trasy, protože model **UrlHelper** vrátí konstanty typu řetězec.


## <a name="testing-actions-that-return-ihttpactionresult"></a>Testování akcí, které vracejí IHttpActionResult

Ve webovém rozhraní API 2, akce kontroleru vrátit **IHttpActionResult**, což je obdobou **ActionResult** v architektuře ASP.NET MVC. **IHttpActionResult** rozhraní definuje vzor příkaz pro vytvoření odpovědi protokolu HTTP. Místo vytváření odpověď přímo, kontroler vrací **IHttpActionResult**. Později, kanál vyvolá **IHttpActionResult** k vytvoření odpovědi. Tento přístup usnadňuje pro psaní jednotkových testů, protože můžete přeskočit spoustu instalační program, který je nezbytný pro **objekt HttpResponseMessage**.

Tady je příklad controller jehož akcí vrácení **IHttpActionResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

Tento příklad ukazuje některé běžné vzory pomocí **IHttpActionResult**. Podívejme se, jak k jednotce pro jejich testování.

### <a name="action-returns-200-ok-with-a-response-body"></a>Akce vrátí 200 (OK) v textu odpovědi

`Get` Volání metody `Ok(product)` Pokud je nalezen produkt. V testu jednotek, ujistěte se, že je návratový typ **OkNegotiatedContentResult** a vrácené produktu má správný identifikátor.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

Všimněte si, že Jednotkový test neprovede výsledku akce. Můžete předpokládat, že výsledek akce správně vytvoří odpověď HTTP. (To je důvod, proč rozhraní webového rozhraní API má svůj vlastní testování částí!)

### <a name="action-returns-404-not-found"></a>Akce vrátí 404 (Nenalezeno)

`Get` Volání metody `NotFound()` Pokud není nalezen produkt. Pro tento případ, test jednotky jenom kontroly, pokud je návratový typ **NotFoundResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a>Akce vrátí 200 (OK) s žádnou odpovědí

`Delete` Volání metody `Ok()` vrátit prázdnou odpověď HTTP 200. Podobně jako v předchozím příkladu, testování částí kontroluje návratovým typem, v tomto případě **OkResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a>Akce vrátí 201 (vytvořeno) se hlavička umístění

`Post` Volání metody `CreatedAtRoute` vrátit odpověď HTTP 201 s identifikátorem URI v hlavičce umístění. V testu jednotek ověřte, že tato akce nastaví správné směrování hodnoty.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a>Akce vrátí jiný 2xx s tělo odpovědi

`Put` Volání metody `Content` vrátit odpověď HTTP 202 (přijato) s tělo odpovědi. Tento případ se podobá vrací 200 (OK), ale testování částí byste taky měli zkontrolovat stavový kód.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a>Další prostředky

- [Vytvoření modelu Entity Framework při testování rozhraní ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- [Zápis testů pro rozhraní ASP.NET Web API službu](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (příspěvek na blogu od Youssef Moussaoui).
- [Ladění v rozhraní ASP.NET Web API s ladicím programem trasy](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
