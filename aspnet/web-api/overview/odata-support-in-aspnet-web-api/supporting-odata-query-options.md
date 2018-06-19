---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: Podpora možnosti dotazu OData v rozhraní ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/04/2013
ms.topic: article
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 004c029db6f01627f7cadff26aaf5554ce2b93a5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566710"
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a>Podpora možnosti dotazu OData v rozhraní ASP.NET Web API 2
====================
podle [Wasson Jan](https://github.com/MikeWasson)

OData definuje parametry, které lze použít k úpravě dotaz OData. Klient odešle tyto parametry v řetězci dotazu identifikátoru URI požadavku. Klient používá k řazení výsledků, $orderby parametr:

`http://localhost/Products?$orderby=Name`

Specifikace prostředí OData volá tyto parametry *možnosti dotazu*. Možnosti dotazu OData pro každý kontroler Web API můžete povolit v projektu & #8212; řadičem nemusí být koncový bod OData. To vám dává pohodlný způsob, jak přidat funkce, jako je například filtrování a řazení pro všechny aplikace webového rozhraní API.

Před povolením možnosti dotazu, přečtěte si téma [doprovodné materiály zabezpečení OData](odata-security-guidance.md).

- [Povolení možnosti dotazu OData](#enable)
- [Ukázky dotazů](#examples)
- [Stránkování řízené serverem](#server-paging)
- [Omezení možnosti dotazu](#limiting_query_options)
- [Vyvolání přímo možnosti dotazu](#ODataQueryOptions)
- [Ověření dotazu](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a>Povolení možnosti dotazu OData

Webové rozhraní API podporuje následující možnosti dotazu OData:

| Možnost | Popis |
| --- | --- |
| $expand | Rozšíří vložené entit v relaci. |
| $filter | Vyfiltruje výsledky, založené na podmínce logická hodnota. |
| $inlinecount | Informuje server, aby zahrnují celkový počet odpovídajících entit v odpovědi. (Užitečné pro stránkování na straně serveru.) |
| $orderby | Řazení výsledků. |
| $select | Vybere vlastnosti, které chcete zahrnout do odpovědi. |
| $skip | Přeskočí první n výsledky. |
| $top | Vrátí pouze první n výsledky. |

Pokud chcete používat možnosti dotazu OData, musíte je povolit explicitně. Můžete povolit globálně pro celou aplikaci nebo je povolit pro konkrétní řadiče nebo konkrétní akce.

Chcete-li povolit možnosti dotazu OData globálně, volejte **EnableQuerySupport** na **HttpConfiguration** třída při spuštění:

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

**EnableQuerySupport** metoda umožňuje možnosti dotazu globálně pro všechny akce kontroleru, který vrací **IQueryable** typu. Pokud nechcete, aby možnosti dotazu, které jsou povolené pro celou aplikaci, můžete je povolit pro konkrétní řadič akce přidáním **[Queryable]** atribut do metody akce.

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a>Ukázky dotazů

Tato část uvádí typy dotazů, které je možné pomocí možností dotazu OData. Pro konkrétní podrobnosti o možnosti dotazu, naleznete v dokumentaci OData v [www.odata.org](http://www.odata.org/).

Informace o $rozbalte a $select, najdete v části [pomocí $select, $, rozbalte položku a $value v prostředí ASP.NET Web API OData](using-select-expand-and-value.md).

**Stránkování řízené klienta**

Pro velké entity sady klient může chtít omezit počet výsledků. Klienta může například zobrazit 10 položek najednou, s "Další" odkazy na získat další stránky výsledků. Chcete-li to provést, klient použije možnosti $top a $skip.

`http://localhost/Products?$top=10&$skip=20`

Možnost $top poskytuje maximální počet vrácených položek a počet položek tak, aby přeskočil dává možnost $skip. Předchozí příklad načte položky 21 do 30.

**Filtrování**

Možnost $filter umožňuje filtrovat výsledky podle použití logický výraz klienta. Výrazy filtru jsou velmi výkonné; obsahují logické a aritmetické operátory, řetězec funkce a funkce datum.

| Vrátí všechny produkty s kategorie rovno "Toys". | `http://localhost/Products?$filter=Category`EQ 'Toys. |
| --- | --- |
| Vrátí všechny produkty s cenou menší než 10. | `http://localhost/Products?$filter=Price`lt 10 |
| Logické operátory: vrátit všechny produkty, kde cena > = 5 a cena < = 15. | `http://localhost/Products?$filter=Price`ge 5 a cena le 15 |
| Funkce pro řetězce: vrátit všechny produkty s "zz" v názvu. | `http://localhost/Products?$filter=substringof('zz',Name)` |
| Datum funkce: vrátit všechny produkty s ReleaseDate po 2005. | `http://localhost/Products?$filter=year(ReleaseDate)`gt 2005 |

**Řazení**

Chcete-li seřadit výsledky, použijte filtr $orderby.

| Řazení podle ceny. | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| Řazení podle cenu v sestupném pořadí (nejvyšší po nejnižší). | `http://localhost/Products?$orderby=Price desc` |
| Seřadit podle kategorie a potom seřadit cenu v sestupném pořadí v rámci kategorií. | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a>Stránkování řízené serverem

Pokud databáze obsahuje miliony záznamů, nechcete jejich odeslání všech v jedné datové části. Chcete-li tomu zabránit, server můžete omezit počet položek, které odešle v odpovědi na jedné. Chcete-li povolit serveru stránkování, nastavte **PageSize** vlastnost v **Queryable** atribut. Hodnota je maximální počet vrácených položek.

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

Pokud vaše řadiče vrátí formátu OData, text odpovědi bude obsahovat odkaz na další stránku dat:

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

Klienta můžete použít tento odkaz se načíst na další stránku. Chcete-li zjistit celkový počet záznamů v sadě výsledků dotazu, můžete klienta nastavit možnost dotazu $inlinecount s hodnotou "allpages".

`http://localhost/Products?$inlinecount=allpages`

Hodnota "allpages" informuje server chcete zahrnout do odpovědi celkového počtu:

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> Odkazy na další stránku a vložený počet, který vyžadují formátu OData. Důvodem je, že OData definuje speciální pole v textu odpovědi pro uložení odkazu a počet.


Formáty bez OData, je stále možné podpora další stránky odkazy a vložený počet, pomocí zabalení výsledků dotazu v **PageResult&lt;T&gt;**  objektu. To ale vyžaduje trochu další kód. Tady je příklad:

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

Tady je příklad odpověď JSON:

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a>Omezení možnosti dotazu

Možnosti dotazu přidělte klientovi spoustu kontrolu nad dotaz, který je spuštěn na serveru. V některých případech můžete chtít omezit dostupné možnosti z důvodů zabezpečení nebo výkonu. **[Queryable]** atribut má některé vytvořené ve vlastnostech pro to. Zde jsou některé příklady.

Povolit pouze $skip a $top, pro podporu stránkování a nic jiného:

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

Povolit řazení jenom podle určité vlastnosti, aby se zabránilo řazení pro vlastnosti, které nejsou uloženy v databázi:

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

Povolit funkci logické "eq", ale žádný logický funkce:

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

Nepovolit žádné aritmetické operátory:

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

Můžete omezit možnosti globálně vytvořením **položce QueryableAttribute** instance a předání do **EnableQuerySupport** funkce:

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a>Vyvolání přímo možnosti dotazu

Místo použití **[Queryable]** atribut přímo do kontroleru můžete vyvolat možnosti dotazu. Chcete-li tak učinit, přidejte **ODataQueryOptions** parametr metodu řadiče. V takovém případě nepotřebujete **[Queryable]** atribut.

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

Webové rozhraní API naplní **ODataQueryOptions** řetězec dotazu z identifikátoru URI. Chcete-li použít dotaz, předat **IQueryable** k **metodu** metoda. Metoda vrátí jiné **IQueryable**.

Pro pokročilé scénáře, pokud nemáte **IQueryable** dotaz na zprostředkovatele, můžete zkontrolovat **ODataQueryOptions** a překládat možnosti dotazu do jiného formátu. (Například najdete v příspěvku blogu RaghuRam Nadiminti [dotazů překladu OData na HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), což zahrnuje také [ukázka](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)

<a id="query-validation"></a>
## <a name="query-validation"></a>Ověření dotazu

**[Queryable]** atribut ověří dotaz před jeho provedením. Krok ověření se provádí v **QueryableAttribute.ValidateQuery** metoda. Můžete také upravit proces ověření.

Viz také [doprovodné materiály zabezpečení OData](odata-security-guidance.md).

Nejprve přepsání jeden validátoru třídy, který je definovaný v **Web.Http.OData.Query.Validators** oboru názvů. Například následující třídu validátora zakáže možnost "desc, pro možnost $orderby.

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

Podtřída **[Queryable]** atribut přepsat **ValidateQuery** metoda.

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

Nastavte vlastní atribut buď globálně nebo na řadič:

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

Pokud používáte **ODataQueryOptions** přímo, nastavte validátor na možnosti:

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
