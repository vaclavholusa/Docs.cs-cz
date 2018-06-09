---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Akce a funkce v OData v4, pomocí rozhraní ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: V prostředí OData akce a funkce jsou způsob, jak přidat serverové chování, které nejsou snadno definován jako operace CRUD na entity. Tento kurz ukazuje, jak...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 532362f0c0faaaf0cb0c04726856f0497e5261b5
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "26566773"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a>Akce a funkce v OData v4, pomocí rozhraní ASP.NET Web API 2.2
====================
podle [Wasson Jan](https://github.com/MikeWasson)

> V prostředí OData akce a funkce jsou způsob, jak přidat serverové chování, které nejsou snadno definován jako operace CRUD na entity. Tento kurz ukazuje, jak přidat akcí a funkcí do OData v4 koncový bod, pomocí webového rozhraní API 2.2. Tento kurz je založený na kurz [vytvoření OData v4 koncový bod pomocí rozhraní ASP.NET Web API 2](create-an-odata-v4-endpoint.md)
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Webové rozhraní API 2.2
> - OData v4
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Kurz verze
> 
> OData verze 3, najdete v části [akcí OData ve webovém rozhraní API 2 ASP.NET](../odata-v3/odata-actions.md).


Rozdíl mezi *akce* a *funkce* je, že akce může mít vedlejší účinky, a funkce nepodporují. Akce a funkce může vrátit data. Některé použití akce patří:

- Komplexní transakce.
- Manipulace se několik entit najednou.
- Povolení aktualizací pouze na určité vlastnosti entity.
- Odesílání dat, která není entita.

Funkce jsou užitečné pro vracení informací, které neodpovídá přímo na položku entita nebo kolekce.

Akce (nebo funkce), můžete vybrat jednu entitu nebo kolekce. V terminologii OData, je to *vazby*. Můžete taky nechat &quot;nevázaný&quot; akce/funkce, které se nazývají jako statické operace ve službě.

## <a name="example-adding-an-action"></a>Příklad: Přidání akce

Umožňuje definovat akci, která má hodnocení produktu.

> [!NOTE]
> V tomto kurzu vychází kurzu [vytvoření OData v4 koncový bod pomocí rozhraní ASP.NET Web API 2](create-an-odata-v4-endpoint.md)


Nejprve přidejte `ProductRating` modelu, který má představovat hodnocení.

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

Také přidat **DbSet** k `ProductsContext` třídy, takže EF vytvoří hodnocení tabulku v databázi.

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a>Přidat akci k modelu EDM

V WebApiConfig.cs přidejte následující kód:

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

**EntityTypeConfiguration.Action** metoda akce přidá do datového modelu entity (EDM). **Parametr** metoda určuje parametr s určeným pro akci.

Tento kód také nastaví obor názvů pro EDM. Obor názvů záleží, protože identifikátor URI pro akci zahrnuje název plně kvalifikovaný akce:

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> V typické konfigurace služby IIS tečky v této adresy URL způsobí, že služba IIS vrátí chybu 404. To lze vyřešit tak, že přidáte do souboru Web.Config v následující části:

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a>Přidání metody Kontroleru pro akci

Chcete-li povolit &quot;míra&quot; akce, přidejte následující metodu do `ProductsController`:

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

Všimněte si, že metoda název odpovídá názvu akce. **[HttpPost]** Určuje atribut metodu je metoda HTTP POST.

Volání akce, klient odešle požadavek HTTP POST takto:

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

&quot;Míra&quot; akce je svázat s instancemi produktu, tak, aby identifikátor URI pro akci akce plně kvalifikovaný název připojí k identifikátoru URI entity. (Odvolat, že jsme obor názvů EDM nastavená na &quot;ProductService&quot;, takže je název akce plně kvalifikovaný &quot;ProductService.Rate&quot;.)

Text žádosti obsahuje parametry akcí jako datové části JSON. Webové rozhraní API automaticky převede datové části JSON do **ODataActionParameters** objekt, který je právě slovník hodnot parametrů. Pomocí tohoto slovníku pro přístup k parametrů v metodu řadiče.

Pokud klient odešle parametry akcí v chybného formátování, hodnota **ModelState.IsValid** je false. Zkontrolujte tento příznak ve své metodě kontroleru a vrátí chybu, pokud **IsValid** je false.

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a>Příklad: Přidání funkce

Nyní Pojďme přidat funkci prostředí OData, vrátí nejnákladnější produktu. Jako dříve, prvním krokem je přidání funkce do modelu EDM. V WebApiConfig.cs přidejte následující kód.

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

V takovém případě je vázána kolekci produktů, nikoli jednotlivých instancí produktu funkce. Klienti vyvolání funkce odesláním požadavek GET:

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

Tady je metoda kontroleru pro tuto funkci:

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

Všimněte si, že název metody odpovídá názvu funkce. **[Třídy MetadataExchangeClientMode]** Určuje atribut metodu je metoda HTTP GET.

Zde je odpověď HTTP:

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a>Příklad: Přidání Nesvázanou funkci

Předchozí příklad se funkce vazbou na určitou kolekci. V tomto příkladu další vytvoříme *nevázaný* funkce. Nevázaný funkce se nazývají jako statické operace ve službě. Funkce v tomto příkladu vrací DPH pro danou PSČ.

V souboru WebApiConfig přidejte do modelu EDM funkce:

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

Všimněte si, že jsme volání **funkce** přímo na **Tvůrce ODataModelBuilder**, místo typu entity nebo kolekci. Tato hodnota informuje Tvůrce modelu funkce nevázaný.

Tady je metoda kontroleru, který implementuje funkce:

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

Řadič webového rozhraní API, který umístěte tuto metodu v nezáleží. Může ho dát `ProductsController`, nebo definujte samostatné řadiče. **[ODataRoute]** atribut definuje šablonu identifikátoru URI pro funkci.

Tady je požadavek klienta příklad:

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

Odpověď HTTP:

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
