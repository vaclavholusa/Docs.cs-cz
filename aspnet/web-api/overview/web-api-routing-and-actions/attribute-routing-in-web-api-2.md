---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Atribut směrování v rozhraní ASP.NET Web API 2 | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 22eb2fd748d52ec95e813ada8b1bf3b4826ad573
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348478"
---
<a name="attribute-routing-in-aspnet-web-api-2"></a>Směrování atributů v rozhraní ASP.NET Web API 2
====================
podle [Mike Wasson](https://github.com/MikeWasson)

*Směrování* je, jak webové rozhraní API odpovídá identifikátor URI pro akci. Webové rozhraní API 2 podporuje nový typ směrování, nazývá *směrováním atributů*. Jak již název napovídá, směrování atributů používá atributy pro definování tras. Směrování atributů vám dává větší kontrolu nad identifikátory URI webového rozhraní API. Například můžete snadno vytvořit identifikátory URI, které popisují hierarchie prostředků.

Starší styl směrování, volá se, založené na konvenci směrování, je stále plně podporovány. Ve skutečnosti můžete kombinovat obě tyto metody ve stejném projektu.

Toto téma ukazuje, jak povolit směrování atributů a popisuje různé možnosti pro směrování atributů. Kurz začátku do konce, který používá atribut směrování, najdete v tématu [vytvořit rozhraní REST API se směrováním atributů ve webovém rozhraní API 2](create-a-rest-api-with-attribute-routing.md).

## <a name="prerequisites"></a>Požadavky

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional nebo Enterprise edition

Alternativně můžete použijte Správce balíčků NuGet nainstalujte potřebné balíčky. Z **nástroje** v aplikaci Visual Studio, vyberte v nabídce **Správce balíčků NuGet**, vyberte **konzoly Správce balíčků**. V okně konzoly Správce balíčků zadejte následující příkaz:

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a>Proč atribut směrování?

První verze webového rozhraní API používá *podle úmluvy* směrování. V tomto typu směrování můžete definovat jeden nebo více šablon trasy, které jsou v podstatě s parametry řetězce. Žádost o přijetí rozhraní odpovídá identifikátoru URI pro šablonu trasy. (Další informace o směrování založené na konvenci, naleznete v tématu [směrování v rozhraní ASP.NET Web API](routing-in-aspnet-web-api.md).

Jednou z výhod založené na konvenci směrování je, že šablony jsou definovány na jednom místě a pravidla směrování konzistentní napříč všemi řadiči. Bohužel založené na konvenci směrování je obtížné podporovat některé URI vzorce, které jsou obvyklé v rozhraní RESTful API. Například prostředky často obsahují podřízené prostředky: Zákazníci mají objednávky, actors mají filmy, knihy jste autoři a tak dále. Je přirozeně k vytvoření identifikátorů URI, které zahrnují tyto vztahy:

`/customers/1/orders`

Tento typ identifikátoru URI je těžké vytvořit pomocí směrování na základě konvence. I když to můžete udělat, výsledky neškálují, pokud máte mnoho řadičů nebo typy prostředků.

Se směrováním atributů, že je jednoduché definovat trasu pro tento identifikátor URI. Jednoduše přidejte atribut do akce kontroleru:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

Tady jsou některé vzory, které atribut směrování umožňuje snadné.

**Správa verzí rozhraní API**

V tomto příkladu "/ api/v1/produktů" bude směrovat do jiného řadiče než "/ api/v2/produktů".

`/api/v1/products`
`/api/v2/products`

**Přetížení identifikátoru URI segmenty**

V tomto příkladu "1" je číslo objednávky, ale "čekající na vyřízení" mapuje na kolekci.

`/orders/1`
`/orders/pending`

**Více typy parametrů**

V tomto příkladu "1" je číslo objednávky, ale "06/2013/16" Určuje datum.

`/orders/1`
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a>Povolení směrování atributů

Chcete-li povolit směrování atributů, zavolejte **MapHttpAttributeRoutes** během konfigurace. Tato metoda rozšíření je definována v **System.Web.Http.HttpConfigurationExtensions** třídy.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

Směrování atributů je možné kombinovat s [podle úmluvy](routing-in-aspnet-web-api.md) směrování. Chcete-li definovat založené na konvenci směrování, zavolejte **MapHttpRoute** metody.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

Další informace o konfiguraci webového rozhraní API najdete v tématu [konfigurace ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a>Poznámka: Migrace z webového rozhraní API 1

Před webovým rozhraním API 2 šablony projektu webového rozhraní API generovaný kód následujícím způsobem:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

Pokud je povoleno směrování atributů, tento kód vyvolá výjimku. Pokud provádíte upgrade existujícího projektu webového rozhraní API používat směrování atributů, nezapomeňte aktualizovat tento kód konfigurace takto:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> Další informace najdete v tématu [konfigurace webového rozhraní API s hostování v technologii ASP.NET](../advanced/configuring-aspnet-web-api.md#webhost).


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a>Přidávání atributů trasy

Tady je příklad postupu definovány pomocí atributu:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

Řetězec &quot;zákazníci / {Idzakaznika} / orders&quot; je šablona identifikátoru URI pro trasu. Webové rozhraní API se pokusí shodovat se identifikátoru URI požadavku do šablony. V tomto příkladu "zákazníci" a "orders" je literál segmentů a "{Idzakaznika}" je parametrů proměnných. Následující identifikátory URI by tato šablona porovnat:

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

Můžete omezit porovnávání pomocí [omezení](#constraints), které jsou popsány dále v tomto tématu.

Všimněte si, že &quot;{Idzakaznika}&quot; parametrů v šabloně postupu odpovídá názvu *customerId* parametr metody. Webové rozhraní API vyvolá akce kontroleru, pokusí se vytvořit vazbu parametry trasy. Například, pokud je identifikátor URI `http://example.com/customers/1/orders`, webové rozhraní API se pokusí vytvořit vazbu hodnotu "1" *customerId* parametr v akci.

Šablona identifikátoru URI může mít několik parametrů:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

Všechny metody kontroleru, které nemají atribut trasy pomocí směrování na základě konvence. Díky tomu můžete kombinovat oba typy směrování ve stejném projektu.

## <a name="http-methods"></a>Metody HTTP

Webové rozhraní API také vybere akce podle metody HTTP požadavku (GET, POST atd.). Ve výchozím nastavení hledá webového rozhraní API nerozlišují se začátkem metoda názvu kontroleru. Například kontroler metodu s názvem `PutCustomers` odpovídá požadavek HTTP PUT.

Tato konvence můžete přepsat pomocí upravení metodu s jakoukoli následující atributy:

- **[HttpDelete]**
- **[HttpGet]**
- **[HttpHead]**
- **[Httpoptions měl]**
- **[HttpPatch]**
- **[HttpPost]**
- **[HttpPut]**

V následujícím příkladu metoda CreateBook mapuje na požadavky HTTP POST.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

Pro všechny ostatní metody HTTP, včetně nestandardní metody, použijte **AcceptVerbs** atribut, který přebírá seznam metod HTTP.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a>Předpony trasy

Trasy se často v kontroleru všechny začínají stejnou předponou. Příklad:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

Běžnou předponu můžete nastavit pro celý kontroler pomocí **[parametr RoutePrefix]** atribut:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

Použijte tilda (~) na atribut method přepsání předpony trasy:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

Předpona trasy může obsahovat parametry:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a>Omezení trasy

Omezení trasy umožňují omezit, jak jsou porovnány parametry v šabloně trasy. Je Obecná syntaxe &quot;{parametr: omezení}&quot;. Příklad:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

Tady, první trasa se vybrat pouze pokud &quot;id&quot; segment identifikátoru URI je celé číslo. V opačném případě bude vybrán Druhá trasa.

Následující tabulka uvádí omezení, které jsou podporovány.

| Omezení | Popis | Příklad |
| --- | --- | --- |
| systém Alpha | Odpovídá velká nebo malá latinská abeceda znaky (a-z, A-Z) | {x: alfa} |
| bool | Odpovídá hodnotu typu Boolean. | {x: bool} |
| Datum a čas | Shody **data a času** hodnotu. | {x: datetime} |
| decimal | Odpovídá desítkovou hodnotu. | {x: decimal} |
| double | Odpovídá 64 bitů hodnotu s plovoucí desetinnou čárkou. | {x: double} |
| float | Odpovídá 32bitová hodnota s plovoucí desetinnou čárkou. | {x: float} |
| identifikátor GUID | Odpovídá hodnotě identifikátor GUID. | {x: guid} |
| int | Odpovídá hodnotě 32bitové celé číslo. | {x: int} |
| length | Odpovídá řetězci se zadanou délkou nebo do zadaného rozsahu délek. | {x: length(6)} {x: length(1,20)} |
| long | Odpovídá hodnotě 64bitové celé číslo. | {x: long} |
| max | Odpovídá celé číslo s maximální hodnotou. | {x: max(10)} |
| MaxLength | Odpovídá řetězci s maximální délkou. | {x: maxlength(10)} |
| min | Odpovídá celé číslo s minimální hodnotou. | {x: min(10)} |
| MinLength | Odpovídá řetězci s minimální délkou. | {x: minlength(10)} |
| range | Odpovídá celé číslo v rozsahu hodnot. | {x: range(10,50)} |
| regulární výraz | Odpovídá regulárnímu výrazu. | {x: regex(^\d{3}-\d{3}-\d{4}$)} |

Všimněte si, že některá omezení, jako například &quot;min&quot;, nepřebírají argumenty v závorkách. Více omezení můžete použít parametr, oddělených středníkem.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a>Omezení vlastní trasy

Můžete vytvořit vlastní trasy omezení implementací **IHttpRouteConstraint** rozhraní. Například následující omezení omezuje parametr na nenulovou celočíselnou hodnotu.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

Následující kód ukazuje, jak zaregistrovat omezení:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

Teď můžete použít omezení v trasy:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

Můžete také nahradit celý **DefaultInlineConstraintResolver** třídy implementací **IInlineConstraintResolver** rozhraní. Tím dojde k nahrazení všech předdefinovaných omezujících podmínek, není-li vaše implementace **IInlineConstraintResolver** konkrétně se přidají.

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a>Parametry identifikátoru URI nepovinný a výchozí hodnoty

Volitelný parametr URI můžete provést přidáním otazník do parametru trasy. Pokud je určitý parametr trasy nepovinný, je nutné definovat výchozí hodnotu pro parametr metody.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

V tomto příkladu `/api/books/locale/1033` a `/api/books/locale` vrátit stejný prostředek.

Alternativně můžete zadat výchozí hodnotu v šabloně trasy následujícím způsobem:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

Toto je téměř stejný jako předchozí příklad, ale neexistuje malého rozdílu chování při použití výchozí hodnotu.

- V prvním příkladu ("{lcid?}") je přiřazen výchozí hodnotu 1033 přímo do parametru metody tak, že parametr bude mít tento přesná hodnota.
- V druhém příkladu ("{lcid = 1033}"), výchozí hodnotu "1033" projde procesem vazby modelu. Vazač modelu výchozí se převede na číselnou hodnotu 1033 "1033". Může však zapojte vlastní vazač modelu, který může dělat něco jiného.

(Ve většině případů, pokud nemáte vlastního modelu pořadače ve vašem kanálu dvě různými formami bude ekvivalentní.)

<a id="route-names"></a>
## <a name="route-names"></a>Názvy tras

Všechny trasy v rozhraní Web API, má název. Názvy tras jsou užitečné pro generování odkazů, takže můžete uvést odkaz v odpovědi HTTP.

Chcete-li zadat název trasy, nastavte **název** vlastnost pro atribut. Následující příklad ukazuje, jak nastavit název trasy a také použití název trasy, která při generování odkazu.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a>Pořadí trasy

Rozhraní se pokusí tak, aby odpovídaly identifikátor URI s trasy, je vyhodnocen jako trasy v určitém pořadí. Chcete-li určit pořadí, nastavte **pořadí** vlastnost pro atribut trasy. Nižší hodnoty jsou vyhodnocen jako první. Výchozí hodnota pořadí je nula.

Zde je, jak se určuje celkový počet pořadí:

1. Porovnání **pořadí** vlastnost atribut trasy.
2. Podívejte se na každý segment identifikátoru URI v šabloně trasy. Pro každý segment pořadí následujícím způsobem:

    1. Literál segmenty.
    2. Parametry trasy s omezeními.
    3. Parametry trasy bez omezení.
    4. Zástupný parametr segmenty s omezeními.
    5. Parametr segmenty zástupný znak bez omezení.
3. V případě rovnosti, trasy jsou řazeny podle porovnání nerozlišuje velikost písmen řetězců podle pořadového čísla ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) šablony trasy.

Zde je příklad. Předpokládejme, že můžete definovat následující kontroler:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

Tyto postupy jsou uspořádaná následujícím způsobem.

1. Příkazy a podrobnosti
2. objednávky / {id}
3. objednávky / {customerName}
4. objednávky / {\*datum}
5. objednávky / čekající na vyřízení

Všimněte si, že "details" je segmentů literálů a před {id}"se zobrazí, ale"až"se zobrazí poslední vzhledem k tomu, **pořadí** vlastnost je 1. (Tento příklad předpokládá existuje se žádní zákazníci s názvem "details" nebo "čeká na vyřízení". Obecně platí snažte se vyhnout nejednoznačný trasy. V tomto příkladu lepší šablona trasy pro `GetByCustomer` je "zákazníkům / {customerName}")
