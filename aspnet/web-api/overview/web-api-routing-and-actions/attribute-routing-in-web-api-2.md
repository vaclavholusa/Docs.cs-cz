---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: "Atribut směrování v rozhraní ASP.NET Web API 2 | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: ad44ee525601f308498967159e964aa41a2ce00c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="attribute-routing-in-aspnet-web-api-2"></a>Atribut směrování v rozhraní ASP.NET Web API 2
====================
podle [Wasson Jan](https://github.com/MikeWasson)

*Směrování* je, jak webové rozhraní API odpovídá identifikátoru URI k akci. Webové rozhraní API 2 podporuje nový typ směrování, nazývá *směrováním atributů*. Jak již název napovídá, směrováním atributů používá atributy pro definování tras. Atribut směrování vám dává větší kontrolu nad identifikátory URI ve vašem webovém rozhraní API. Například můžete snadno vytvořit identifikátory URI, které popisují hierarchie prostředků.

Starší styl směrování, názvem založené na konvenci směrování, je stále plně podporována. Ve skutečnosti můžete kombinovat obě tyto metody ve stejném projektu.

Toto téma ukazuje, jak povolit směrováním atributů a popisuje různé možnosti pro atribut směrování. Kurz začátku do konce, který používá atribut směrování, najdete v části [vytvořit rozhraní REST API s atribut směrování ve webovém rozhraní API 2](create-a-rest-api-with-attribute-routing.md).


## <a name="prerequisites"></a>Požadavky

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional nebo Enterprise Edition

Případně můžete použijte Správce balíčků NuGet k instalaci nezbytných balíčků. Z **nástroje** nabídky v sadě Visual Studio, vyberte **Správce balíčků knihoven**, pak vyberte **Konzola správce balíčků**. Zadejte následující příkaz v okně konzoly Správce balíčků:

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a>Proč atribut směrování?

První verze součásti webového rozhraní API používá *založené na konvenci* směrování. V tomto typu směrování definujete jeden nebo více šablon trasy, které jsou v podstatě parametry řetězce. Žádost o přijetí rozhraní odpovídá identifikátoru URI pro šablonu trasy. (Další informace o směrování založené na konvenci, najdete v části [směrování v rozhraní ASP.NET Web API](routing-in-aspnet-web-api.md).

Jednou z výhod založené na konvenci směrování je, že šablony jsou definovány na jednom místě a pravidla směrování se použít konzistentně napříč všechny řadiče. Bohužel založené na konvenci směrování důvodu je těžké podporují určité vzorce identifikátor URI, které jsou společné rozhraní RESTful API. Například prostředků často obsahují podřízené prostředky: Zákazníci mají objednávky, filmy mít aktéři, knihy mít autoři a tak dále. Jedná se o přirozené vytvořit identifikátory URI, která tyto vztahy v úvahu:

`/customers/1/orders`

Tento typ identifikátoru URI je těžké vytvořit pomocí založené na konvenci směrování. I když je možné ji provést, není výsledky škálovat dobře, pokud máte mnoho řadiče nebo typy prostředků.

Se směrováním atributů je jednoduchá k definování trasu pro tento identifikátor URI. Jednoduše přidání atributu do akce kontroleru:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

Tady jsou některé vzory, tento atribut směrování díky snadné.

**Správa verzí rozhraní API**

V tomto příkladu "/ api/v1/products" by směrované na řadič jiný než "/ api/v2 nebo produktů".

`/api/v1/products`  
`/api/v2/products`

**Přetížené segmenty identifikátor URI**

V tomto příkladu "1" je číslo objednávky, ale "čekající" mapy do kolekce.

`/orders/1`  
`/orders/pending`

**Více typy parametrů**

V tomto příkladu "1" je číslo objednávky, ale "2013/06/16" Určuje datum.

`/orders/1`  
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a>Povolení směrováním atributů

Chcete-li povolit směrováním atributů, volejte **MapHttpAttributeRoutes** během konfigurace. Tato metoda rozšíření je definována v **System.Web.Http.HttpConfigurationExtensions** třídy.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

Atribut směrování je možné kombinovat s [založené na konvenci](routing-in-aspnet-web-api.md) směrování. Chcete-li definovat založené na konvenci směrování, volejte **MapHttpRoute** metoda.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

Další informace o konfiguraci webového rozhraní API najdete v tématu [konfigurace ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a>Poznámka: Migrace z webového rozhraní API 1

Před webovém rozhraní API 2 šablony projektu webového rozhraní API generovaného kódu takto:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

Pokud je povoleno atribut směrování, tento kód vyvolá výjimku. Pokud upgradujete existující projekt webového rozhraní API používat směrováním atributů, nezapomeňte aktualizovat tento kód konfigurace takto:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> Další informace najdete v tématu [konfiguraci webového rozhraní API s hostování prostředí ASP.NET](../advanced/configuring-aspnet-web-api.md#webhost).


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a>Přidání atributů tras

Tady je příklad trasy definované pomocí atribut:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

Řetězec &quot;zákazníkům / {customerId} / řadí&quot; je šablona identifikátor URI pro trasu. Webové rozhraní API se pokusí vyhledat identifikátoru URI požadavku do šablony. V tomto příkladu jsou literálu segmenty "zákazníci" a "objednávky" a "{customerId}" je parametr proměnné. Následující identifikátory URI odpovídá této šablony:

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

Můžete omezit odpovídající pomocí [omezení](#constraints), které jsou popsány dále v tomto tématu.

Všimněte si, že &quot;{customerId}&quot; odpovídá názvu parametru v šabloně trasy *customerId* parametr v metodě. Když webového rozhraní API vyvolá akce kontroleru, pokusí se vytvořit vazbu parametry trasy. Například pokud je identifikátor URI `http://example.com/customers/1/orders`, webového rozhraní API se pokusí vytvořit vazbu hodnotu "1" *customerId* parametr v akci.

Identifikátor URI šablona může mít několik parametrů:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

Všechny metody kontroleru, které nemají atribut trasy používat založené na konvenci směrování. Tímto způsobem můžete kombinovat oba typy směrování v rámci jednoho projektu.

## <a name="http-methods"></a>Metody HTTP

Webové rozhraní API také vybere akce založené na metodě HTTP požadavku (GET, POST atd.). Ve výchozím nastavení hledá webového rozhraní API velká a malá písmena shody s začátek metoda názvu kontroleru. Například metoda kontroleru s názvem `PutCustomers` odpovídá požadavek HTTP PUT.

Touto konvencí můžete přepsat pomocí architekturu – metoda s jakoukoli následující atributy:

- **[HttpDelete]**
- **Třídy [MetadataExchangeClientMode]**
- **[HttpHead]**
- **[Httpoptions měl]**
- **[HttpPatch]**
- **[HttpPost]**
- **[HttpPut]**

Následující příklad mapuje metodu CreateBook na požadavky HTTP POST.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

Pro všechny ostatní metody HTTP, včetně nestandardní metod, pomocí **AcceptVerbs** atribut, který přebírá seznam metod HTTP.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a>Předpony trasy

Trasy se často v kontroleru všechny začínají stejnou předponou. Příklad:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

Běžné předpony pro celý kontroler můžete nastavit pomocí **[RoutePrefix]** atribut:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

Použijte tildou (~) v atributu metoda přepsat předponu trasy:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

Předpona trasy může obsahovat parametry:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a>Omezení trasy

Omezení trasy vám umožní omezit, jak se splní parametrů v šabloně trasy. Je Obecná syntaxe &quot;{parametr: omezení}&quot;. Příklad:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

Zde první trasy se vybrat pouze pokud &quot;id&quot; segment identifikátoru URI je celé číslo. Druhý trasy, jinak bude vybrána.

Následující tabulka uvádí omezení, které jsou podporovány.

| Omezení | Popis | Příklad |
| --- | --- | --- |
| Alpha | Odpovídá velká nebo malá písmena latinky znaky (a-z, A-Z) | {x: alpha} |
| bool | Odpovídá logickou hodnotu. | {x: bool} |
| Data a času | Odpovídá **data a času** hodnotu. | {x: datetime} |
| decimal | Odpovídá desítkovou hodnotu. | {x: decimal} |
| double | Odpovídá 64bitová hodnota s plovoucí desetinnou čárkou. | {x: double} |
| float | Odpovídá 32bitovou hodnotu s plovoucí desetinnou čárkou. | {x: float} |
| Identifikátor GUID | Odpovídá hodnota identifikátoru GUID. | {x: guid} |
| int | Odpovídá hodnotě 32bitové celé číslo. | {x: int} |
| length | Odpovídá řetězci se zadanou délkou nebo v rámci zadaného rozsahu délek. | {x: length(6)} {x: length(1,20)} |
| long | Odpovídá hodnotě 64bitové celé číslo. | {x: dlouho} |
| max | Odpovídá celé číslo s maximální hodnotou. | {x: max(10)} |
| hodnota MaxLength | Odpovídá řetězec s maximální délkou. | {x: maxlength(10)} |
| min | Odpovídá celé číslo s minimální hodnotou. | {x: min(10)} |
| MinLength | Odpovídá řetězci s minimální délkou. | {x: minlength(10)} |
| range | Odpovídá celé číslo v rozsahu hodnot. | {x: range(10,50)} |
| regulární výraz | Odpovídá regulárnímu výrazu. | {x: regex(^\d{3}-\d{3}-\d{4}$)} |

Všimněte si některá omezení, jako například &quot;min&quot;, trvat argumenty v závorkách. Můžete použít více omezení pro parametr oddělené dvojtečkou.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a>Omezení vlastní trasy

Můžete vytvořit vlastní trasy omezení implementací **IHttpRouteConstraint** rozhraní. Například následující omezení omezuje parametr nenulovou celočíselnou hodnotu.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

Následující kód ukazuje, jak zaregistrovat omezení:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

Teď můžete použít omezení v trasy:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

Můžete také nahradit celý **DefaultInlineConstraintResolver** třída implementací **IInlineConstraintResolver** rozhraní. Díky tomu dojde k nahrazení všech předdefinovaných omezení, pokud vaši implementaci **IInlineConstraintResolver** konkrétně přidává je.

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a>Identifikátor URI volitelné parametry a výchozí hodnoty

Volitelný parametr URI můžete provést přidáním otazník parametru trasy. Pokud je volitelný parametr trasy, je nutné zadat výchozí hodnotu pro parametr metody.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

V tomto příkladu `/api/books/locale/1033` a `/api/books/locale` vrátit stejného zdroje.

Alternativně můžete zadat výchozí hodnotu v šabloně trasy následujícím způsobem:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

Toto je téměř stejný jako předchozí příklad, ale pokud je použita výchozí hodnota je malého rozdílu chování.

- V prvním příkladu ("{lcid?}") výchozí hodnotu 1033 je přiřazen přímo parametru metody, tak parametr bude mít tento přesná hodnota.
- V druhém příkladu ("{lcid = 1033}"), výchozí hodnota "1033" projde procesem vazby modelu. Vazač modelu výchozí převede "1033" číselnou hodnotu 1033. Můžete však zařaďte vlastní vazač modelu, který může dělat něco jiného.

(Ve většině případů, pokud máte v kanálu, vazače modelů vlastní dvě formy bude ekvivalentní.)

<a id="route-names"></a>
## <a name="route-names"></a>Názvy tras

Všechny trasy v rozhraní Web API, má název. Názvy tras jsou užitečné pro generování odkazů, tak, aby odkaz můžete zahrnout do odpovědi HTTP.

Chcete-li zadat název trasy, nastavte **název** vlastnost v atributu. Následující příklad ukazuje, jak nastavit název trasy a také použití název trasy, která při generování odkazu.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a>Pořadí trasy

Když se pokusí rozhraní tak, aby odpovídaly identifikátor URI s trasu, vyhodnotí trasy v určitém pořadí. Chcete-li určit pořadí, nastavte **RouteOrder** vlastnost na atribut trasy. Nižší hodnoty se vyhodnocují jako první. Výchozí hodnota pořadí je nulová.

Zde je, jakým způsobem je určován celkový řazení:

1. Porovnání **RouteOrder** vlastnost atribut trasy.
2. Podívejte se na každý segment identifikátoru URI v šabloně trasy. Pro každý segment pořadí následujícím způsobem: 

    1. Literál segmenty.
    2. Parametry trasy s omezeními.
    3. Parametry trasy bez omezení.
    4. Zástupný parametr segmenty s omezeními.
    5. Zástupný parametr segmenty bez omezení.
3. V případě vazbě, trasy jsou seřazené podle porovnání velká a malá písmena pořadí řetězců ([OrdinalIgnoreCase](https://msdn.microsoft.com/en-us/library/system.stringcomparer.ordinalignorecase.aspx)) v šabloně trasy.

Zde je příklad. Předpokládejme, že definujete řadičem následující:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

Tyto trasy seřazeni následujícím způsobem.

1. objednávky nebo podrobnosti
2. objednávky nebo {id}
3. objednávky nebo {JménoZákazníka}
4. objednávky nebo {\*datum}
5. objednávky / čekající na vyřízení

Všimněte si, že "Podrobnosti" je literál segment a se zobrazuje před "{id}, ale"čekající na vyřízení"zobrazí poslední, protože **RouteOrder** vlastnost je 1. (Tento příklad předpokládá existuje jsou odběratelé s názvem "Podrobnosti" nebo "čeká na vyřízení". Obecně platí pokuste se vyhnout nejednoznačný trasy. V tomto příkladu lepší šablona trasy pro `GetByCustomer` je "zákazníkům / {JménoZákazníka}")
