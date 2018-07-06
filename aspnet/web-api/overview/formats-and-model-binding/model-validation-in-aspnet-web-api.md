---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Ověření v rozhraní ASP.NET Web API modelu | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/20/2012
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: be681a97a65a95a6bb83196d29c60ae750a523a1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820010"
---
<a name="model-validation-in-aspnet-web-api"></a>Ověření modelu v rozhraní ASP.NET Web API
====================
podle [Mike Wasson](https://github.com/MikeWasson)

Když klient odešle data do webového rozhraní API, často budete chtít ověřit data před provedením jakékoli zpracování. Tento článek popisuje postup přidání poznámek ke své modely, použijte poznámky ověřování dat a zpracování chyb ověření webového rozhraní API.

## <a name="data-annotations"></a>Datové poznámky

V rozhraní ASP.NET Web API, můžete použít atributy [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) oboru názvů k nastavení ověřovacích pravidel pro vlastnosti v modelu. Vezměte v úvahu následující model:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

Pokud používáte ověření modelu v architektuře ASP.NET MVC, to by měla vypadat povědomě. **Vyžaduje** atribut uvádí, že `Name` vlastnost nesmí mít hodnotu null. **Rozsah** atribut uvádí, že `Weight` musí být v rozmezí od 0 do 999.

Předpokládejme, že klient odešle požadavek POST s následující reprezentaci JSON:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

Uvidíte, že klient nenabízela `Name` vlastnost, která je označena jako povinné. Pokud webové rozhraní API převede text JSON do `Product` instance, ověří `Product` proti atributy ověření. Do vaší akce kontroleru můžete zkontrolovat, zda je model platný:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

Ověření modelu nezaručuje, že klient data budou v bezpečí. V jiných vrstvách aplikace může být nutné další ověřování. (Například datová vrstva může vynutit omezení cizího klíče.) Tento kurz [pomocí webového rozhraní API s Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) zkoumá některé z těchto problémů.

**"Nadměrném účtování"**: snížení účtování se stane, když klient vynechány některé vlastnosti. Předpokládejme například, že klient odešle následující:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

Tady, klient nezadali hodnoty `Price` nebo `Weight`. Formátování JSON přiřadí výchozí hodnota nula, která chybí vlastnosti.

![](model-validation-in-aspnet-web-api/_static/image1.png)

Stav modelu, který je platný, protože nula je platná hodnota pro tyto vlastnosti. Ať už se jedná o problém závisí na vaší situaci. Například v operaci aktualizace, můžete chtít rozlišovat mezi "0" a "není nastavený." Vynuťte u klientů nastavit hodnotu, zkontrolujte vlastnost s možnou hodnotou Null a nastavené **vyžaduje** atribut:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

**"Over-pass-the účtování"**: klienta můžete také odeslat *Další* dat, než jste očekávali. Příklad:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

Tady, JSON obsahuje vlastnost ("Color"), která neexistuje v `Product` modelu. V takovém případě formátování JSON jednoduše ignoruje tuto hodnotu. (Formátovací modul XML dělá to samé.) Útoky over-pass-the účtování způsobuje problémy, pokud model obsahuje vlastnosti, které jste chtěli být jen pro čtení. Příklad:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

Nechcete, aby uživatelům aktualizovat `IsAdmin` vlastnost a sami zvýšit oprávnění na správce. Nejbezpečnější možností je používat třídu modelu, který přesně odpovídá co klientovi může odeslat:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> Příspěvek na blogu Brada Wilsona "[ověření vstupu vs. Ověření v architektuře ASP.NET MVC modelu](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)"je dobré diskuzi o navýšení účtování a snížení účtování. I když je příspěvek o ASP.NET MVC 2, problémy jsou stále relevantní pro webové rozhraní API.


## <a name="handling-validation-errors"></a>Zpracování chyb při ověřování

Webové rozhraní API nevrací automaticky chybu klientovi když se ověřování nezdaří. Záleží akce kontroleru ke kontrole stavu modelu a reagují odpovídajícím způsobem.

Můžete také vytvořit filtr akce ke kontrole stavu modelu před vyvolání akce kontroleru. Následující kód ukazuje příklad:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

Pokud selže ověření modelu, tento filtr vrátí odpověď HTTP, který obsahuje chyby ověření. V takovém případě není vyvolána akce kontroleru.

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

Chcete-li použít tento filtr na všechny řadiče webové rozhraní API, přidejte instance filtru, který **HttpConfiguration.Filters** kolekce během konfigurace:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

Další možností je nastavit filtr na jednotlivých řadičích nebo akce kontroleru jako atribut:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
