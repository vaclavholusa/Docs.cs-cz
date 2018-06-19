---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Podpora v rozhraní ASP.NET Web API 2 OData akce | Microsoft Docs
author: MikeWasson
description: 'V prostředí OData akce jsou způsob, jak přidat serverové chování, které nejsou snadno definován jako operace CRUD na entity. Některé použití akce patří: implementace...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: acb369ca8f1bab8d7cad14c15f46cfd44beb9fdd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566821"
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a>Podpora v rozhraní ASP.NET Web API 2 OData akce
====================
podle [Wasson Jan](https://github.com/MikeWasson)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> V prostředí OData *akce* představují způsob, jak přidat serverové chování, které nejsou snadno definován jako operace CRUD na entity. Některé použití akce patří:
> 
> - Implementace složité transakce.
> - Manipulace se několik entit najednou.
> - Povolení aktualizací pouze na určité vlastnosti entity.
> - Odesílání informací na server, který není definován v entitě.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Webové rozhraní API 2
> - OData verze 3
> - Entity Framework 6


## <a name="example-rating-a-product"></a>Příklad: Hodnocení produktu

V tomto příkladu chceme, aby mohli uživatelé míra produkty a následně je zpřístupněte průměrné hodnocení pro jednotlivé produkty. V databázi ukládáme seznam hodnocení, s klíči pro produkty.

Tady je model, který bychom může použít k reprezentování hodnocení v Entity Framework:

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

Ale Neradi bychom klientům POST `ProductRating` objektu do kolekce "Hodnocení". Intuitivně hodnocení souvisí s produkty kolekce a klienta pouhým post hodnoty hodnocení.

Proto místo použití normální operace CRUD, jsme definovali akci, která můžete vyvolat klienta na produktu. V terminologii OData je akce *vázaný* na entity produktu.

>Akce mají vedlejší účinky na serveru. Z toho důvodu jsou vyvolány pomocí požadavky HTTP POST. Akce může mít parametry a návratové typy, které jsou popsány v metadata služby. Klient odešle parametry v textu požadavku a server odešle návratovou hodnotu v textu odpovědi. Volání akce "Míra produkt", klient odešle metodu POST SMĚŘUJÍCÍ do identifikátoru URI takto:

[!code-console[Main](odata-actions/samples/sample2.cmd)]

Data v požadavku POST je jednoduše hodnocení produktu:

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a>Deklarovat akce v datovém modelu Entity

V konfiguraci vašeho webového rozhraní API přidejte do datového modelu entity (EDM) akce:

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

Tento kód definuje "RateProduct" jako akci, která lze provést u entity produktu. Také deklaruje, že provede akci **int** parametr s názvem "Hodnocení" a vrátí **int** hodnotu.

## <a name="add-the-action-to-the-controller"></a>Přidejte k řadiči akce

Akce "RateProduct" se váže k entitám produktu. K provedení akce, přidejte metodu s názvem `RateProduct` řadiče produkty:

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

Všimněte si, že metoda název odpovídá názvu akce v modelu EDM. Metoda má dva parametry:

- *klíč*: klíč produktu míru.
- *Parametry*: slovník hodnot parametrů akce.

Pokud používáte výchozích konvencí směrování, parametr klíče musí mít název "klíče". Je také důležité zahrnout **[FromOdataUri]** atributu, jak je vidět. Tento atribut určuje webového rozhraní API používat OData syntaxe pravidla v případě, že analyzuje klíč z identifikátoru URI požadavku.

Použití *parametry* adresář k získání parametrů akcí:

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

Pokud klient odešle parametry akcí ve správné formátování, hodnota **ModelState.IsValid** hodnotu true. V takovém případě můžete použít **ODataActionParameters** slovník k získání hodnot parametrů. V tomto příkladu `RateProduct` akce přijímá jeden parametr s názvem "Hodnocení".

## <a name="action-metadata"></a>Akce metadat

Chcete-li zobrazit metadata služby, odeslat požadavek GET na /odata/$ metadat. Zde je část metadat, který deklaruje `RateProduct` akce:

[!code-xml[Main](odata-actions/samples/sample7.xml)]

**FunctionImport** element deklaruje akce. Většina polí jsou zřejmé, ale dva jsou vhodné poznamenat:

- **Vlastnost IsBindable** znamená akce mohou být vyvolány na cílovou entitu alespoň určitou dobu.
- **IsAlwaysBindable** znamená akce může být vyvolána vždy na cílovou entitu.

Rozdílem je, že některé akce jsou vždy k dispozici pro klienty, ale může další akce závisí na stavu entity. Předpokládejme například, že můžete definovat akce "Nákupu". Pouze si můžete zakoupit položku, která je ve skladu. Pokud položka není skladě, nemůže klient vyvolat této akce.

Když definujete EDM, **akce** metoda vytvoří vždy vázat akce:

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

I mluvit o akcích, není vždy vazbu (také nazývané *přechodný* akce) dále v tomto tématu.

## <a name="invoking-the-action"></a>Vyvolání akce

Nyní si ukážeme, jak by měl klient vyvolat tuto akci. Předpokládejme, že klient chce umožnit hodnocení 2 pro produkt s ID = 4. Tady je příklad zprávu s žádostí o, formátu JSON pro textu žádosti:

[!code-console[Main](odata-actions/samples/sample9.cmd)]

Zde je zpráva odpovědi:

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a>Vytvoření vazby akce na sadu entit

V předchozím příkladu akce vázán na jedné entity: klient hodnotí jednoho produktu. Akce můžete vázat také na kolekci entit. Stačí proveďte následující změny:

V modelu EDM, přidat akci v entitě **kolekce** vlastnost.

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

V metodě řadiče, vynechejte *klíč* parametr.

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

Teď klienta vyvolá akci na sadu entit produkty:

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a>Akce s parametry kolekce

Akce může mít parametry, které berou kolekci hodnot. V modelu EDM, použijte **CollectionParameter&lt;T&gt;**  deklarovat parametr.

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

Tento parametr s názvem "Hodnocení", která vezme kolekci deklaruje **int** hodnoty. V metodě kontroleru, stále získána hodnota parametru **ODataActionParameters** objektu, ale teď hodnota je **ICollection&lt;int&gt;**  hodnotu:

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a>Přechodné akce

V příkladu "RateProduct" uživatelé vždy ohodnotit produktu, takže akce je vždy k dispozici. Ale některé akce závisí na stavu entity. Například ve službě video pronájem akce "Najdete v článku věnovaném" není vždy k dispozici. (To závisí, zda je k dispozici kopii tohoto videa.) Tento typ akce se označuje *přechodný* akce.

V metadatech služby přechodné akce má **IsAlwaysBindable** rovnou na hodnotu false. Který je ve skutečnosti výchozí hodnota, takže metadata bude vypadat například takto:

[!code-xml[Main](odata-actions/samples/sample16.xml)]

Zde uvádíme Proč to záleží: Pokud je přechodný, server musí dali pokyn klientovi, pokud je daná akce dostupná. Dělá to pomocí zahrnutím odkazu na akce v entitě. Tady je příklad pro entitu film:

[!code-console[Main](odata-actions/samples/sample17.cmd)]

Vlastnost "#CheckOut" obsahuje odkaz na akce CheckOut. Pokud akce není dostupná, server vynechá odkaz.

Pokud chcete deklarovat přechodné akce v modelu EDM, zavolejte **TransientAction** metoda:

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

Navíc je nutné zadat funkci, která vrátí pro danou entitu odkaz akce. Nastavení této funkce voláním **HasActionLink**. Jako výrazu lambda může zapisovat funkce:

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

Pokud je daná akce dostupná, výrazu lambda vrátí odkaz na akci. Serializátor prostředí OData zahrnuje tento odkaz se serializuje entity. Když akce není k dispozici, funkce vrátí hodnotu `null`.

## <a name="additional-resources"></a>Další prostředky

[Ukázka akce OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
