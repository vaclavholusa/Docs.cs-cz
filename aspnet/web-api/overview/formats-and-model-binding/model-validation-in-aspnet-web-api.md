---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Model ověřování v rozhraní ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2012
ms.topic: article
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 409a91eceb8baa48a7dded1b850d59a27cec2c60
ms.sourcegitcommit: 5ae0c125ee3bbd324edef3818d1d160f4dd84602
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/17/2018
ms.locfileid: "34224723"
---
<a name="model-validation-in-aspnet-web-api"></a>Ověření modelu v rozhraní ASP.NET Web API
====================
podle [Wasson Jan](https://github.com/MikeWasson)

Když klient odešle data do webového rozhraní API, často budete chtít ověřit data před provedením jakékoli zpracovávání. Tento článek ukazuje, jak opatřit poznámkami modely, použijte poznámky pro ověření dat a zpracování chyb při ověřování v webového rozhraní API.

## <a name="data-annotations"></a>Datových poznámek

V rozhraní ASP.NET Web API, můžete použít atributy z [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) obor názvů nastavit pravidla ověřování pro vlastnosti modelu. Vezměte v úvahu následující modelu:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

Pokud jste použili ověření modelu v architektuře ASP.NET MVC, to by měla vypadat povědomě. **Požadované** atribut uvádí, že `Name` vlastnost nesmí mít hodnotu null. **Rozsah** atribut uvádí, že `Weight` musí být v rozmezí od 0 do 999.

Předpokládejme, že klient odešle požadavek POST s následující reprezentace JSON:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

Uvidíte, že klient nezahrnuli `Name` požadované vlastnosti, která je označena jako. Když webového rozhraní API převede do formátu JSON `Product` instance, ověřuje `Product` proti atributy ověření. V řadiči akci můžete zkontrolovat, zda je model platný:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

Ověření modelu není zaručeno, že data klientů je bezpečné. Další ověření může být vhodné v jiných vrstev aplikace. (Například datová vrstva může vynutit omezení cizích klíčů.) Tento kurz [pomocí webového rozhraní API s platformou Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) jsou zde popsány některé z těchto problémů.

**"Snížení příspěvků"**: snížení účtování se stane, když klient zůstane se některé vlastnosti. Předpokládejme například, že klient odešle následující:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

Zde klienta nebyla zadána hodnota pro `Price` nebo `Weight`. Formátovací modul JSON přiřadí výchozí hodnotu nula na chybějící vlastnosti.

![](model-validation-in-aspnet-web-api/_static/image1.png)

Stav modelu, který je platný, protože nula není platná hodnota pro tyto vlastnosti. Zda se jedná o problém závisí na vašem scénáři. Například v operaci aktualizace, můžete k rozlišení mezi "nula" a "není nastavena." Chcete-li vynutit klientům nastavit hodnotu, zkontrolujte vlastnost s možnou hodnotou Null a nastavte **požadované** atribut:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

**"Typu Overpost"**: klient může také odesílat *Další* dat, než jste očekávali. Příklad:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

Zde JSON obsahuje vlastnosti ("Barva"), která neexistuje v `Product` modelu. V takovém případě formátovací modul JSON jednoduše ignoruje tato hodnota. (Formátovací modul XML nemá stejný.) Přečerpání příspěvků způsobuje problémy, pokud model obsahuje vlastnosti, které mají být jen pro čtení. Příklad:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

Nechcete, aby uživatelům aktualizovat `IsAdmin` vlastnost a sami zvýšení oprávnění správcům! Nejbezpečnější strategie se má používat třídu modelu, který přesně odpovídá má klient povoleno odeslat:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> Příspěvek blogu Brada Wilsona "[ověření vstupu vs. Model ověření v architektuře ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)"má dobrou diskuzi o snížení účtování a přečerpání účtování. I když je požadavek post o ASP.NET MVC 2, problémy jsou stále relevantní pro webového rozhraní API.


## <a name="handling-validation-errors"></a>Zpracování chyb při ověřování

Webové rozhraní API nevrací automaticky chybu klientovi když se ověřování nezdaří. Je akce kontroleru ke kontrole stavu modelu a reagují odpovídajícím způsobem.

Můžete také vytvořit filtr akce pro kontrolu stavu modelu, před vyvoláním akce kontroleru. Následující kód ukazuje příklad:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

Pokud selže ověření modelu, tento filtr vrátí odpověď HTTP, který obsahuje chyby ověření. V takovém případě není vyvolána akce kontroleru.

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

Tento filtr použít na všechny řadiče webového rozhraní API, přidá instanci filtr, který má **HttpConfiguration.Filters** kolekce během konfigurace:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

Další možností je nastavit filtr na jednotlivých řadičích nebo akce kontroleru jako atribut:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
