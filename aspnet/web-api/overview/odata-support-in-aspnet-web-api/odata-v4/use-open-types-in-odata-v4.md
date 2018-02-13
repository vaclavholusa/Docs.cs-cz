---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: "Otevřete typy v OData v4 s rozhraním ASP.NET Web API | Microsoft Docs"
author: microsoft
description: "V prostředí OData v4 je otevřený typ stuctured typu, který obsahuje dynamické vlastnosti, kromě všechny vlastnosti, které jsou deklarované v definici typu. Otevřete..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2014
ms.topic: article
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: fe67b9a11a82b55d5f3e0e5f1b0cee10a58833d2
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/12/2018
---
<a name="open-types-in-odata-v4-with-aspnet-web-api"></a>Otevřete typy v OData v4 s rozhraním ASP.NET Web API
====================
podle [Microsoft](https://github.com/microsoft)

> V prostředí OData v4 *otevřete typ* je stuctured typu, který obsahuje dynamické vlastnosti, kromě všechny vlastnosti, které jsou deklarované v definici typu. Otevřete typy umožňují zvýšit flexibilitu datové modely. Tento kurz ukazuje, jak používat otevřete typy v prostředí ASP.NET Web API OData.
> 
> Tento kurz předpokládá, že již víte, jak vytvořit koncový bod OData v rozhraní ASP.NET Web API. Pokud ne, spustit načtením [vytvořit koncový bod OData v4](create-an-odata-v4-endpoint.md) první.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Web API OData 5.3
> - OData v4


První, některé terminologie OData:

- Typ entity: strukturovaného typu s klíčem.
- Komplexní typ: strukturovaného typu bez klíče.
- Otevřete typu: typ s dynamických vlastností. Typy entit a komplexní typy možné otevřít.

Hodnota dynamické vlastnosti, může být primitivní typ, komplexní typ nebo typ výčtu; nebo kolekci jakýkoli z těchto typů. Další informace o typech otevřete najdete v tématu [OData v4 specifikace](http://www.odata.org/documentation/odata-version-4-0/).

## <a name="install-the-web-odata-libraries"></a>Instalace webové knihovny OData

Použití Správce balíčků NuGet k instalaci nejnovějších knihoven Web API OData. Z okna konzoly Správce balíčků:

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a>Definování typů CLR

Začněte definováním modelech EDM jako typy CLR.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

Když je vytvořena Entity Data Model (EDM),

- `Category`je typ výčtu.
- `Address`je komplexního typu. (Nemá klíče, takže není typ entity.)
- `Customer`je typ entity. (Má klíč.)
- `Press`je otevřený komplexního typu.
- `Book`je typu open entity.

Pokud chcete vytvořit otevřený typ, typ CLR musí mít vlastnost typu `IDictionary<string, object>`, který obsahuje dynamických vlastností.

## <a name="build-the-edm-model"></a>Vytvoření modelu EDM

Pokud používáte **ODataConventionModelBuilder** pro vytvoření modelu EDM `Press` a `Book` se automaticky přidají jako otevřete typy, založené na přítomnost `IDictionary<string, object>` vlastnost.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

Můžete také vytvořit EDM explicitně, pomocí **Tvůrce ODataModelBuilder**.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a>Přidání Kontroleru OData

Dál přidejte kontroleru OData. V tomto kurzu použijeme zjednodušené řadiče, podporuje jenom získat POST požadavky a používá seznam aplikace v paměti k ukládání entit.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

Všimněte si, že první `Book` instance nemá žádné dynamické vlastnosti. Druhý `Book` instance má následující dynamické vlastnosti:

- "Publikovat": primitivní typ
- "Autoři": kolekce primitivní typy
- "OtherCategories": kolekce výčtové typy.

Navíc `Press` vlastnost této `Book` instance má následující dynamické vlastnosti:

- "Blog": primitivní typ
- "Address": komplexního typu.

## <a name="query-the-metadata"></a>Dotazu Metadata

Chcete-li získat dokument metadat OData, odešlete požadavek GET na `~/$metadata`. Text odpovědi by měl vypadat podobně jako tento:

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

Z dokumentu metadat můžete zjistit, který:

- Pro `Book` a `Press` typy, hodnota `OpenType` atribut hodnotu true. `Customer` a `Address` typy nemají tento atribut.
- `Book` Typ entity má tři deklarované vlastnosti: ISBN, název a stiskněte klávesu. OData metadata nezahrnuje `Book.Properties` vlastnost z třídy CLR.
- Podobně `Press` komplexní typ má jenom dvě deklarované vlastnosti: název a kategorie. Metadata se nenachází `Press.DynamicProperties` vlastnost z třídy CLR.

## <a name="query-an-entity"></a>Dotaz Entity

Potřebujete kniha s ISBN rovno "978-0-7356-7942-9", pošlete požadavek GET na `~/Books('978-0-7356-7942-9')`. Text odpovědi by měla vypadat podobně jako následující. (Odsazeny, aby byl čitelnější.)

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

Všimněte si, že dynamické vlastnosti jsou zahrnuty vložené s deklarované vlastnosti.

## <a name="post-an-entity"></a>POST Entity

Přidání entity adresáře, odeslání požadavek POST do `~/Books`. Klienta můžete nastavit dynamické vlastnosti v datová část požadavku.

Tady je příklad požadavek. Poznámka: vlastnosti "Cena" a "Publikováno".

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

Pokud jste nastavili zarážka v metodě řadiče, uvidíte, webového rozhraní API přidat tyto vlastnosti, které chcete `Properties` slovníku.

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a>Další prostředky

[Ukázka typu OData Open](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
