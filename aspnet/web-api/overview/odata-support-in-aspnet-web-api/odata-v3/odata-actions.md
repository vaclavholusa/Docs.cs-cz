---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Podpora akce OData v rozhraní ASP.NET Web API 2 | Dokumentace Microsoftu
author: MikeWasson
description: 'V prostředí OData akce jsou způsob, jak přidat chování na straně serveru, které nejsou snadno definované jako operace CRUD u entit. Mezi použití akce patří: implementace...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: b7a968082587120c2a19be86524f9b2eba80856e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370452"
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a>Podpora akce OData v rozhraní ASP.NET Web API 2
====================
podle [Mike Wasson](https://github.com/MikeWasson)

[Stáhnout dokončený projekt](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> V prostředí OData *akce* představují způsob, jak přidat chování na straně serveru, které nejsou snadno definované jako operace CRUD u entit. Mezi použití akce patří:
> 
> - Implementace složité transakce.
> - Manipulace s několika entit najednou.
> - Povolení aktualizací pouze k určité vlastnosti entity.
> - Odeslání informací na server, který není definovaná v entitě.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Webové rozhraní API 2
> - OData verze 3
> - Entity Framework 6


## <a name="example-rating-a-product"></a>Příklad: Hodnocení produktu

V tomto příkladu chceme umožnit uživatelům hodnocení produktů a potom vystavit průměrné hodnocení pro jednotlivé produkty. V databázi uložíme seznam hodnocení, s klíči produktů.

Tady je model, který bychom může použít k reprezentaci hodnocení v Entity Framework:

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

Nechceme klientům příspěvek, ale `ProductRating` objektu do kolekce "Hodnocení". Intuitivně hodnocení souvisí s produkty kolekce a klienta by mělo být pouze potřeba odeslání hodnocení.

Proto nemusíte používat běžné operace CRUD, definujeme, který může klient vyvolat akci na produktu. V terminologii OData "action" je *vázán* k entitám produktu.

>Akce mají vedlejší účinky na serveru. Z tohoto důvodu jsou vyvolány pomocí požadavků HTTP POST. Akce může mít parametry a návratové typy, které jsou popsány v metadatech služby. Klient odešle parametry v textu požadavku a server odešle vrácená hodnota v textu odpovědi. K vyvolání akce "Frekvence produkt", klient odešle příspěvek na identifikátor URI vypadat asi takto:

[!code-console[Main](odata-actions/samples/sample2.cmd)]

Data v požadavku POST je jednoduše hodnocení produktu:

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a>Deklarovat akce v modelu Entity Data Model

V konfiguraci webového rozhraní API přidáte akci k modelu entity data model (EDM):

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

Tento kód definuje "RateProduct" jako akci, která lze provést u entity produktu. Také deklaruje, že tato akce trvá **int** parametr s názvem "Hodnocení" a vrátí **int** hodnotu.

## <a name="add-the-action-to-the-controller"></a>Přidání akce Kontroleru

Akce "RateProduct" je vázán na entity produktu. K provedení akce, přidejte metodu s názvem `RateProduct` ke kontroleru produkty:

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

Všimněte si, že název metody odpovídá názvu akce v modelu EDM. Metoda má dva parametry:

- *klíč*: klíč produktu pro míru.
- *Parametry*: slovník hodnot parametrů akce.

Pokud používáte výchozích konvencí směrování, parametr klíče musí mít název "klíče". Je také důležité zahrnout **[FromOdataUri]** atributu, jak je znázorněno. Tento atribut oznamuje webového rozhraní API pro použití pravidel syntaxe OData při analýze klíč z identifikátoru URI požadavku.

Použití *parametry* slovníku zobrazíte parametry akce:

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

Pokud klient odešle parametry akce ve správné formátování, hodnota **ModelState.IsValid** má hodnotu true. V takovém případě můžete použít **ODataActionParameters** slovníku k získání hodnot parametrů. V tomto příkladu `RateProduct` akce přijímá jeden parametr s názvem "Hodnocení".

## <a name="action-metadata"></a>Akce metadat

Chcete-li zobrazit metadata služby, odeslat požadavek GET na /odata/$ metadat. Tady je část metadata, která deklaruje `RateProduct` akce:

[!code-xml[Main](odata-actions/samples/sample7.xml)]

**Element FunctionImport** element deklaruje akce. Většina polí není potřeba vysvětlovat, ale dvě stojí za zmínku:

- **Vlastnost IsBindable** znamená, že tato akce může být vyvolána na cílovou entitu, alespoň některé času.
- **Isalwaysbindable pro položku** znamená, že tato akce může být vyvolána vždy na cílovou entitu.

Rozdíl je, že některé akce jsou vždy k dispozici pro klienty, ale jiné akce může záviset na stav entity. Předpokládejme například, že definujete akce "Koupit". Můžete zakoupit pouze položky, která je na skladě. Pokud je položka vyčerpány zásoby, klient nejde vyvolat tuto akci.

Při definování EDM, **akce** metoda vytvoří vždy vazbu akce:

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

Můžu budeme mluvit o not vždy – s možností vazby akce (také nazývané *přechodné* akce) dále v tomto tématu.

## <a name="invoking-the-action"></a>Vyvolání akce

Nyní Podíváme se, jak by měl klient vyvolat tuto akci. Předpokládejme, že klient chce poskytnout hodnocení 2 pro produkt s ID = 4. Tady je ukázková zpráva požadavku, ve formátu JSON pro datovou část požadavku:

[!code-console[Main](odata-actions/samples/sample9.cmd)]

Tady je zpráva odpovědi:

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a>Vytvoření vazby akce na sadu entit

V předchozím příkladu, akce je vázán na jednu entitu: klient hodnotí jednoho produktu. Akci můžete také navázat na kolekci entit. Stačí proveďte následující změny:

V modelu EDM, přidání akce do entity **kolekce** vlastnost.

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

V metodě kontroleru vynechat, nechte *klíč* parametru.

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

Nyní klient vyvolá akci na sadu entit produkty:

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a>Akce s parametry kolekce

Akce může mít parametry, které berou kolekci hodnot. V modelu EDM, použijte **CollectionParameter&lt;T&gt;**  k deklaraci parametru.

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

To deklaruje parametr s názvem "Hodnocení", která vezme kolekci **int** hodnoty. V metodě kontroler stále získána hodnota parametru **ODataActionParameters** objektu, ale teď hodnota je **rozhraní ICollection&lt;int&gt;**  hodnotu:

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a>Přechodné akce

V příkladu "RateProduct" uživatelé vždy ohodnotit produktu, akce je vždycky k dispozici. Ale některá akce závisí na stavu entity. Například ve službě video pronájmu "Rezervace" akce není vždy k dispozici. (Záleží, zda je k dispozici kopii tohoto videa.) Tento typ akce je volána *přechodné* akce.

V metadatech služby přechodné akce má **isalwaysbindable pro položku** rovná na hodnotu false. Který je ve skutečnosti výchozí hodnotu, takže metadata bude vypadat například takto:

[!code-xml[Main](odata-actions/samples/sample16.xml)]

Tady je Proč je to důležité: Pokud akce je přechodná, server musí dali pokyn klientovi, pokud je daná akce dostupná. Dělá to tak, že včetně odkazu na akci v dané entitě. Tady je příklad entity filmu:

[!code-console[Main](odata-actions/samples/sample17.cmd)]

Vlastnost "#CheckOut" obsahuje odkaz na akce CheckOut. Pokud akce není k dispozici, server vynechá odkazu.

Chcete-li deklarovat přechodné akce v modelu EDM, zavolejte **TransientAction** metody:

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

Kromě toho je nutné zadat funkce vracející odkaz akce pro danou entitu. Nastavit tuto funkci voláním **HasActionLink**. Jako výraz lambda lze napsat funkci:

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

Pokud je daná akce dostupná, výraz lambda vrátí odkaz na akci. Serializátor prostředí OData zahrnuje tento odkaz, když dojde k serializaci entit. Pokud akce není k dispozici, funkce vrátí `null`.

## <a name="additional-resources"></a>Další prostředky

[Ukázka akce OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
