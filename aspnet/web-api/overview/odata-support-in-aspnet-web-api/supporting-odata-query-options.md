---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: Podpora možností dotazů OData v rozhraní ASP.NET Web API 2 | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: riande
ms.date: 02/04/2013
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 8745183125c9dd1dcc7cb0e146367a893bdb0170
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754824"
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a>Podpora možností dotazů OData v rozhraní ASP.NET Web API 2
====================
podle [Mike Wasson](https://github.com/MikeWasson)

OData definuje parametry, které lze použít k úpravě dotazu OData. Klient odešle tyto parametry v řetězci dotazu identifikátoru URI požadavku. Například pokud chcete výsledky seřadit, klient použije parametr $orderby:

`http://localhost/Products?$orderby=Name`

Specifikace prostředí OData volá tyto parametry *možnosti dotazu*. Možnosti dotazu OData pro každý kontroler rozhraní Web API můžete povolit ve vašem projektu &#8212; kontroleru nemusí být koncový bod OData. To umožňuje pohodlný způsob, jak přidat funkce, jako je filtrování a řazení do libovolné aplikace webového rozhraní API.

Před povolením možnosti dotazu, přečtěte si prosím téma [doprovodné materiály zabezpečení OData](odata-security-guidance.md).

- [Povolení možnosti dotazu OData](#enable)
- [Příklady dotazů](#examples)
- [Stránkování řízené serverem](#server-paging)
- [Omezení možnosti dotazu](#limiting_query_options)
- [Přímo vyvoláním možnosti dotazu](#ODataQueryOptions)
- [Ověření dotazů](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a>Povolení možnosti dotazu OData

Webové rozhraní API podporuje následující možnosti dotazu OData:

| Možnost | Popis |
| --- | --- |
| $expand | Rozbalí vložené související entity. |
| $filter | Filtrování výsledků na základě logické podmínky. |
| $inlinecount | Informuje server zahrnout celkový počet odpovídajících entit v odpovědi. (Užitečné pro stránkování na straně serveru.) |
| $orderby | Seřadí výsledky. |
| $select | Vybere vlastnosti, které chcete zahrnout do odpovědi. |
| $skip | Přeskočí prvních n výsledků. |
| $top | Vrací jenom prvních n výsledků. |

Pokud chcete použít možnosti dotazu OData, musíte je povolit explicitně. Můžete povolit globálně pro celou aplikaci nebo je povolit pro konkrétní řadiče nebo konkrétní akce.

Chcete-li povolit možnosti dotazu OData globálně, zavolejte **EnableQuerySupport** na **HttpConfiguration** třída při spuštění:

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

**EnableQuerySupport** metoda umožňuje využívat možnosti dotazu globálně pro všechny akce kontroleru, který vrátí **IQueryable** typu. Pokud nechcete, aby možnosti dotazu, které jsou povolené pro celou aplikaci, můžete povolit je pro konkrétní ovladač akcí tak, že přidáte **[Queryable]** atributu na metodu akce.

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a>Příklady dotazů

Tato část uvádí typy dotazů, které je možné pomocí možností dotazu OData. Pro konkrétní podrobnosti o parametrech dotazů najdete v dokumentaci OData na [www.odata.org](http://www.odata.org/).

Informace o $rozbalte a $select, naleznete v tématu [pomocí $select $expand a $value v ASP.NET Web API OData](using-select-expand-and-value.md).

**Stránkování řízené klienta**

Pro velká entita sady klient může chtít omezit počet výsledků. Klient může například zobrazovat 10 položek současně s "Další" odkazy, chcete-li získat další stránky výsledků. Klient použije k tomu možnosti $top a $skip.

`http://localhost/Products?$top=10&$skip=20`

Možnost $top poskytuje maximální počet vrácených položek a možnost $skip poskytuje počet položek pro přeskočení. Předchozí příklad načte položky 21 až 30.

**Filtrování**

Možnost $filter umožňuje klientovi filtrování výsledků s použitím logického výrazu. Filtr výrazy jsou velmi výkonné; patří mezi ně aritmetické a logické operátory, řetězce funkce a funkce data.

| Vrátí všechny produkty s kategorií rovno "Toys". | `http://localhost/Products?$filter=Category` EQ 'Toys. |
| --- | --- |
| Vrátí všechny produkty s cenou menší než 10. | `http://localhost/Products?$filter=Price` lt 10 |
| Logické operátory: vrátit všechny produkty, kde cena > = 5 a cena < = 15. | `http://localhost/Products?$filter=Price` ge 5 a cena le 15 |
| Řetězec, funkce: vrátí všechny produkty s "zz" v názvu. | `http://localhost/Products?$filter=substringof('zz',Name)` |
| Data funkce: vrátí všechny produkty s ReleaseDate po 2005. | `http://localhost/Products?$filter=year(ReleaseDate)` gt 2005 |

**Řazení**

Chcete-li seřadit výsledky, použijte filtr $orderby.

| Seřadit podle cena. | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| Seřadit podle: cena v sestupném pořadí (nejvyšší k nejnižší). | `http://localhost/Products?$orderby=Price desc` |
| Seřadit podle kategorie a pak řazení podle ceny v sestupném pořadí v rámci kategorie. | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a>Stránkování řízené serverem

Pokud databáze obsahuje milióny záznamů, které nechcete posílat je všechny v jedné datové části. Chcete-li tomu zabránit, serveru můžete omezit počet položek, které odešle v jedné odezvě. Chcete-li povolit stránkování na straně serveru, nastavte **PageSize** vlastnost **Queryable** atribut. Hodnota je maximální počet vrácených položek.

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

Pokud váš kontroler vrací formátu OData, text odpovědi bude obsahovat odkaz na další stránku dat:

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

Klienta můžete použít tento odkaz k načtení další stránky. Informace o tom celkový počet položek v sadě výsledků, můžete klienta nastavit možnost dotazu $inlinecount s hodnotou "allpages".

`http://localhost/Products?$inlinecount=allpages`

Hodnota "allpages" informuje server zahrnout celkový počet v odpovědi:

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> Odkazy na další stránky i vložený počet, který vyžadují formátu OData. Důvodem je, že OData definuje zvláštní pole v těle odpovědi pro uložení odkazu a počet.


Protokolu OData formátů, je stále možné podporovat další stránky odkazy a vložený počet, obalením výsledky dotazu v **PageResult&lt;T&gt;**  objektu. To ale vyžaduje trochu další kód. Tady je příklad:

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

Tady je příklad odpověď JSON:

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a>Omezení možnosti dotazu

Možnosti dotazu dát klientům spoustu kontrolu nad dotaz, který běží na serveru. V některých případech můžete chtít omezit dostupné možnosti z důvodů zabezpečení nebo výkon. **[Queryable]** atribut má některé integrované vlastnosti pro toto. Tady je několik příkladů.

Povolit pouze $skip a $top, pro podporu stránkování a nic jiného:

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

Povolit řazení jenom podle určité vlastnosti, aby se zabránilo řazení na vlastnosti, které nejsou indexovány v databázi:

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

Povolit funkci logické "eq", ale žádné logické funkce:

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

Nejsou povoleny všechny aritmetické operátory:

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

Můžete omezit možnosti globálně tak, že vytváří **položce QueryableAttribute** instance a předá se **EnableQuerySupport** funkce:

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a>Přímo vyvoláním možnosti dotazu

Namísto použití **[Queryable]** atribut, možnosti dotazu můžete vyvolat přímo ve vašem řadiči. Chcete-li to provést, přidejte **ODataQueryOptions** parametru k metodě kontroleru. V takovém případě nepotřebujete **[Queryable]** atribut.

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

Naplní webového rozhraní API **ODataQueryOptions** řetězec dotazu z identifikátoru URI. Použije dotaz, předejte **IQueryable** k **volat metodu** metoda. Metoda vrátí jiný **IQueryable**.

Pro pokročilé scénáře, pokud nemáte **IQueryable** poskytovatele dotazů, můžete zkontrolovat **ODataQueryOptions** a překládat možnosti dotazu do jiného formátu. (Například najdete v příspěvku blogu RaghuRam Nadiminti [dotazů překladu OData na HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), kde najdete také [ukázka](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)

<a id="query-validation"></a>
## <a name="query-validation"></a>Ověření dotazů

**[Queryable]** atribut ověří dotaz před jeho provedením. Krok ověření se provádí v **QueryableAttribute.ValidateQuery** metody. Můžete také přizpůsobit proces ověření.

Viz také [doprovodné materiály zabezpečení OData](odata-security-guidance.md).

Nejprve přepsání jeden validátoru tříd, který je definovaný v **Web.Http.OData.Query.Validators** oboru názvů. Například následující třídu validátora zakáže možnost "desc" pro možnost $orderby.

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

Podtřídy **[Queryable]** atribut přepsání **ValidateQuery** metody.

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

Potom nastavte vlastní atribut buď globálně nebo na řadič:

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

Pokud používáte **ODataQueryOptions** přímo, nastavte ověřovací modul na možnostech:

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
