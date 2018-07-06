---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Akce a funkce v OData v4 pomocí rozhraní ASP.NET Web API 2.2 | Dokumentace Microsoftu
author: MikeWasson
description: V prostředí OData akce a funkce jsou způsob, jak přidat chování na straně serveru, které nejsou snadno definované jako operace CRUD u entit. Tento kurz ukazuje, jak...
ms.author: aspnetcontent
ms.date: 06/27/2014
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: a34361a2b298a6cb1efaa386d0a60011f4a578fa
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834971"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a>Akce a funkce v OData v4 pomocí rozhraní ASP.NET Web API 2.2
====================
podle [Mike Wasson](https://github.com/MikeWasson)

> V prostředí OData akce a funkce jsou způsob, jak přidat chování na straně serveru, které nejsou snadno definované jako operace CRUD u entit. Tento kurz ukazuje postupy při přidání akcí a funkcí do koncového bodu OData v4, pomocí Web API 2.2. Tento kurz vychází z kurzu [vytvořit OData v4 koncový bod pomocí rozhraní ASP.NET Web API 2](create-an-odata-v4-endpoint.md)
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
> OData verze 3, naleznete v tématu [akcí OData, které v prostředí ASP.NET Web API 2](../odata-v3/odata-actions.md).


Rozdíl mezi *akce* a *funkce* je, že akce může mít vedlejší účinky a funkce nepodporují. Akce a funkce může vrátit data. Mezi použití akce patří:

- Složité transakce.
- Manipulace s několika entit najednou.
- Povolení aktualizací pouze k určité vlastnosti entity.
- Odesílání dat, která není entita.

Funkce jsou užitečné pro vracení informací, které neodpovídá přímo na entitu nebo kolekci.

Akce (nebo funkce) můžete cílit na jednu entitu nebo kolekci. V terminologii OData, je to *vazby*. Můžete taky nechat &quot;nevázaného&quot; akce/funkce, které se nazývají jako statické operace ve službě.

## <a name="example-adding-an-action"></a>Příklad: Přidání akce

Umožňuje definovat akci, která hodnocení produktu.

> [!NOTE]
> V tomto kurzu vychází z kurzu [vytvořit OData v4 koncový bod pomocí rozhraní ASP.NET Web API 2](create-an-odata-v4-endpoint.md)


Nejprve přidejte `ProductRating` modelu k reprezentaci hodnocení.

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

Také přidat **DbSet** k `ProductsContext` třídy, aby EF vytvoří hodnocení tabulky v databázi.

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a>Přidání akce do modelu EDM

V WebApiConfig.cs přidejte následující kód:

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

**EntityTypeConfiguration.Action** metoda přidá akci do modelu entity data model (EDM). **Parametr** metody určuje parametr typu pro akci.

Tento kód také nastaví obor názvů pro modelu EDM. Obor názvů je důležité, protože identifikátor URI pro akce obsahuje akce plně kvalifikovaný název:

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> V typické konfiguraci služby IIS způsobí tečky v této adresy URL IIS vrátí chybu 404. Můžete to vyřešit přidáním následujícího oddílu do souboru Web.Config:

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a>Přidání Kontroleru metody akce

Povolit &quot;míra&quot; akce, přidejte následující metodu do `ProductsController`:

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

Všimněte si, že název metody odpovídá názvu akce. **[HttpPost]** atribut určuje, že metoda je metodou HTTP POST.

Abyste mohli vyvolat akce, klient odešle požadavek HTTP POST podobný tomuto:

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

&quot;Míra&quot; akce je svázat s instancemi produktu, takže akce plně kvalifikovaný název připojí k identifikátoru URI entity je identifikátoru URI pro akci. (Připomínáme, že nastavíme na obor názvů EDM &quot;ProductService&quot;, takže je název akce plně kvalifikovaný &quot;ProductService.Rate&quot;.)

Text žádosti obsahuje parametry akce jako datovou část JSON. Webové rozhraní API automaticky převede datové části JSON do **ODataActionParameters** objekt, který je právě slovník hodnot parametrů. Pomocí tohoto slovníku pro přístup k parametrům ve své metodě kontroleru.

Pokud klient odešle parametry akce v chybného formátování, hodnota **ModelState.IsValid** má hodnotu false. Zkontrolujte tento příznak ve své metodě kontroleru a vrátí chybu, pokud **IsValid** má hodnotu false.

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a>Příklad: Přidání funkce

Nyní Pojďme přidat funkci prostředí OData, která vrací nejdražší produktu. Jako dříve, prvním krokem je přidání funkce do modelu EDM. V WebApiConfig.cs přidejte následující kód.

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

V takovém případě je funkce vázaná kolekci produktů, spíše než jednotlivé instance produktu. Klienti vyvolat funkci odesláním požadavku GET:

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

Tady je metoda kontroleru pro tuto funkci:

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

Všimněte si, že název metody odpovídá názvu funkce. **[HttpGet]** atribut určuje, že metoda je metoda HTTP GET.

Tady je odpovědi HTTP:

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a>Příklad: Přidání nepřipojeného – funkce

V předchozím příkladu se funkce vazbou na určitou kolekci. V tomto příkladu další vytvoříme *nevázaného* funkce. Nevázaný funkce jsou volány jako statické operace ve službě. Funkce v tomto příkladu vrátí daň z prodeje pro danou poštovní směrovací číslo.

V souboru WebApiConfig přidáte funkce do modelu EDM:

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

Všimněte si, že jsme volání **funkce** přímo na **Tvůrce ODataModelBuilder**, místo typu entity nebo kolekci. Říká Tvůrce modelu, že funkce není vázaný.

Tady je metoda kontroleru, který implementuje funkce:

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

Které kontroler Web API je umístit tuto metodu v nezáleží. Může ho dát `ProductsController`, nebo definujte samostatný kontroleru. **[ODataRoute]** atribut definuje šablonu identifikátoru URI pro funkci.

Tady je příklad žádosti klienta:

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

Odpověď HTTP:

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
