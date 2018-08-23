---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: Serializace JSON a XML v rozhraní ASP.NET Web API | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: riande
ms.date: 05/30/2012
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: 47967e6e1dd0e84b6059c07d7544c0e755fdf510
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754575"
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a>Serializace JSON a XML v rozhraní ASP.NET Web API
====================
podle [Mike Wasson](https://github.com/MikeWasson)

Tento článek popisuje formátování JSON a XML v rozhraní ASP.NET Web API.

V rozhraní ASP.NET Web API *formátovací modul typu média* je objekt, který můžete:

- Tělo zprávy objekty CLR pro čtení z protokolu HTTP
- Zápis objektů CLR do textu zprávy HTTP

Webové rozhraní API poskytuje formátovacích modulů typů médií pro JSON a XML. Rámci těchto formátování vloží do kanálu ve výchozím nastavení. Klienti mohou požadovat JSON nebo XML v hlavičce Accept požadavku HTTP.

## <a name="contents"></a>Obsah

- [Formátovací modul typu média JSON](#json_media_type_formatter)

    - [Vlastnosti jen pro čtení](#json_readonly)
    - [Kalendářní data](#json_dates)
    - [Odsazení](#json_indenting)
    - [CamelCase](#json_camelcasing)
    - [Anonymní a slabě typované objekty](#json_anon)
- [Formátovací modul typu média XML](#xml_media_type_formatter)

    - [Vlastnosti jen pro čtení](#xml_readonly)
    - [Kalendářní data](#xml_dates)
    - [Odsazení](#xml_indenting)
    - [Nastavení na typ XML Serializátorů](#xml_pertype)
- [Odebrání formátovací modul XML nebo JSON](#removing_the_json_or_xml_formatter)
- [Zpracování objektu cyklické odkazy](#handling_circular_object_references)
- [Testování serializaci objektů](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a>Formátovací modul typu média JSON

Formátování JSON je poskytován **JsonMediaTypeFormatter** třídy. Ve výchozím nastavení **JsonMediaTypeFormatter** používá [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) knihovny provést serializace. Json.NET je projekt otevřít zdroj třetích stran.

Pokud dáváte přednost, můžete nakonfigurovat **JsonMediaTypeFormatter** třídu použít **DataContractJsonSerializer** místo Json.NET. Chcete-li to provést, nastavte **UseDataContractJsonSerializer** vlastnost **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>Serializace JSON

Tato část popisuje některé konkrétní chování formátovací modul JSON pomocí výchozího [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializátor. To neměl být úplnou dokumentaci Json.NET knihovny; Další informace najdete v tématu [Json.NET dokumentaci](http://james.newtonking.com/projects/json/help/).

#### <a name="what-gets-serialized"></a>Získá serializován co?

Ve výchozím nastavení všechny veřejné vlastnosti a pole jsou součástí serializovaná JSON. Pokud chcete vynechat, nechte vlastnost nebo pole, uspořádání ji **JsonIgnore** atribut.

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

Pokud chcete &quot;vyjádřit výslovný souhlas&quot; přístup, uspořádání třídy s **kontraktu dat DataContract** atribut. Pokud tento atribut je k dispozici, členové jsou ignorovány, pokud nemají **DataMember**. Můžete také použít **DataMember** k serializaci soukromé členy.

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>Vlastnosti jen pro čtení

Ve výchozím nastavení se serializovat vlastnosti jen pro čtení.

<a id="json_dates"></a>
### <a name="dates"></a>Kalendářní data

Ve výchozím nastavení, Json.NET zapisuje data při [ISO 8601](http://www.w3.org/TR/NOTE-datetime) formátu. Data ve standardu UTC (Coordinated Universal Time) jsou zapsány s příponou "Z". Data v místním čase zahrnují posun časového pásma. Příklad:

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

Ve výchozím nastavení zachovává Json.NET časové pásmo. Toto můžete přepsat tak, že nastavíte vlastnost DateTimeZoneHandling:

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

Pokud byste radši chtěli použít [formát data Microsoft JSON](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) namísto formátu ISO 8601, nastavte **DateFormatHandling** vlastnost pro nastavení serializátoru:

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a>Odsazení

Chcete-li zapsat odsazený JSON, nastavte **formátování** nastavení **Formatting.Indented**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a>CamelCase

Chcete-li zapsat názvy vlastností JSON s camelCase, beze změny datového modelu, nastavte **CamelCasePropertyNamesContractResolver** na serializátoru:

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a>Anonymní a slabě typované objekty

Metody akce můžete vrátit anonymní objekt a serializaci do formátu JSON. Příklad:

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

Tělo zprávy odpovědi bude obsahovat následující JSON:

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

Pokud vaše webové rozhraní API přijímá volně strukturovaná objekty JSON od klientů, při deserializaci v textu žádosti **Newtonsoft.Json.Linq.JObject** typu.

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

Je však obvykle vhodnější použít objekty dat silného typu. Pak není nutné analyzovat data, a získáte výhody ověření modelu.

Serializátor XML nepodporuje anonymní typy nebo **JObject** instancí. Pokud použijete tyto funkce pro vaše data JSON, měli byste odebrat formátovací modul XML z kanálu, jak je popsáno dále v tomto článku.

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>Formátovací modul typu média XML

Formát XML je poskytován **XmlMediaTypeFormatter** třídy. Ve výchozím nastavení **XmlMediaTypeFormatter** používá **DataContractSerializer** pro provádění serializace.

Pokud dáváte přednost, můžete nakonfigurovat **XmlMediaTypeFormatter** používat **XmlSerializer** místo **DataContractSerializer**. Chcete-li to provést, nastavte **UseXmlSerializer** vlastnost **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

**XmlSerializer** třída podporuje užší sadu typů než **DataContractSerializer**, ale dává větší kontrolu nad výsledného kódu XML. Zvažte použití **XmlSerializer** potřebujete odpovídat stávajícím schématu XML.

### <a name="xml-serialization"></a>Serializace XML

Tato část popisuje některé konkrétní chování formátovací modul XML pomocí výchozího **DataContractSerializer**.

Ve výchozím objektu DataContractSerializer chová takto:

- Všechny vlastnosti veřejné čtení a zápis a pole se serializují. Pokud chcete vynechat, nechte vlastnost nebo pole, uspořádání ji **IgnoreDataMember** atribut.
- Soukromé a chráněné členy nejsou serializována.
- Vlastnosti jen pro čtení nejsou serializována. (Ale obsah vlastnosti kolekce jen pro čtení se serializují.)
- Názvy tříd a členů jsou zapsány v souboru XML, přesně tak, jak jsou uvedeny v deklaraci třídy.
- Výchozí obor názvů XML se používá.

Pokud potřebujete větší kontrolu nad serializace, můžete uspořádání třídy s **kontraktu dat DataContract** atribut. Když tento atribut je k dispozici, je třída serializované následujícím způsobem:

- &quot;Vyjádřit výslovný souhlas&quot; přístup: pole a vlastnosti nejsou serializované ve výchozím nastavení. K serializaci vlastnost nebo pole, uspořádání ji **DataMember** atribut.
- K serializaci soukromého nebo chráněného členu, uspořádání ji **DataMember** atribut.
- Vlastnosti jen pro čtení nejsou serializována.
- Chcete-li změnit, jak se zobrazí název třídy v souboru XML, nastavte *název* parametr **kontraktu dat DataContract** atribut.
- Chcete-li změnit, jak se zobrazí název členu v souboru XML, nastavte *název* parametr **DataMember** atribut.
- Chcete-li změnit obor názvů XML, nastavte *Namespace* parametr **kontraktu dat DataContract** třídy.

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>Vlastnosti jen pro čtení

Vlastnosti jen pro čtení nejsou serializována. Pokud je vlastnost jen pro čtení privátní pole zálohování, můžete označit privátní pole s **DataMember** atribut. Tento přístup vyžaduje **kontraktu dat DataContract** pomocí atributu.

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>Kalendářní data

Data jsou napsané ve formátu ISO 8601. Například &quot;2012-05-23T20:21:37.9116538Z&quot;.

<a id="xml_indenting"></a>
### <a name="indenting"></a>Odsazení

Chcete-li zapsat odsazený XML, nastavte **odsazení** vlastnost **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>Nastavení na typ XML Serializátorů

Můžete nastavit různé serializátory XML pro různé typy CLR. Například můžete mít určitý datový objekt, který vyžaduje **XmlSerializer** z důvodu zpětné kompatibility. Můžete použít **XmlSerializer** pro tento objekt a dál používat **DataContractSerializer** u jiných typů.

Chcete-li nastavit serializátor XML pro určitý typ, zavolejte **SetSerializer**.

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

Můžete zadat **XmlSerializer** nebo libovolný objekt, který je odvozen od **XmlObjectSerializer**.

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>Odebrání formátovací modul XML nebo JSON

Formátování JSON nebo formátovací modul XML můžete odebrat z seznam formátovacích modulů, pokud nechcete k jejich použití. Hlavní důvody k tomu jsou:

- Chcete-li omezit vaše webové rozhraní API odpovědi na určitý typ média. Například můžete rozhodnout pro podporu pouze odpověďmi ve formátu JSON a odebrat formátovací modul XML.
- Nahradit výchozí formátování pomocí vlastního formátovacího modulu. Může například nahradit vlastní implementaci formátování JSON formátování JSON.

Následující kód ukazuje, jak odebrat výchozí formátování. Volání z vaší **aplikace\_Start** metoda, definována v souboru Global.asax.

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>Zpracování objektu cyklické odkazy

Ve výchozím nastavení formátování JSON a XML zápis všech objektů jako hodnoty. Pokud dvě vlastnosti odkazují na stejný objekt nebo pokud se zobrazí na stejný objekt v kolekci dvakrát, formátovací modul se serializovat objekt dvakrát. Totiž konkrétního problému. Pokud váš graf objektu obsahuje cykly, serializátoru, který vyvolá výjimku, když zjistí smyčku v grafu.

Vezměte v úvahu následující objektové modely a kontroleru.

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

Vyvolání tato akce způsobí, že formátovací modul k vyvolání výjimky, která se přeloží na stavový kód 500 (vnitřní chyba serveru) odpověď klientovi.

Pokud chcete zachovat odkazy na objekty ve formátu JSON, přidejte následující kód k **aplikace\_Start** metody v souboru Global.asax:

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

Nyní akce kontroleru vrátí JSON, který vypadá takto:

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

Všimněte si, že serializátor přidá &quot;$id&quot; vlastnost na oba objekty. Navíc rozpozná, že vlastnost Employee.Department vytvoří smyčku, takže nahradí hodnotu odkazu na objekt: {&quot;$ref&quot;:&quot;1&quot;}.

> [!NOTE]
> Odkazy na objekty nejsou standardní ve formátu JSON. Před použitím této funkce, zvažte, jestli vaši klienti budou moci analyzovat výsledky. Může být lepší jednoduše odstranit z grafu cykly. Například odkaz z zaměstnance zpět do oddělení, není v tomto příkladu skutečně nutná.


Pokud chcete zachovat odkazy na objekty ve formátu XML, máte dvě možnosti. Jednodušší možností je přidat `[DataContract(IsReference=true)]` do vaší třídy modelu. *IsReference* parametr povoluje odkazy na objekty. Nezapomeňte, že **kontraktu dat DataContract** díky serializace vyjádřit výslovný souhlas, takže budete muset přidat **DataMember** atributy vlastnosti:

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

Nyní vytvoří formátovací modul XML, podobně jako následující:

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

Pokud chcete se vyhnout atributy v třídě modelu, existuje další možnost: vytvořit nový typ konkrétní **DataContractSerializer** instance a nastavte *preserveObjectReferences* k **true**  v konstruktoru. Nastavte tuto instanci jako serializátor-type na formátovací modul typu média XML. Následující kód ukazuje, jak to provést toto:

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>Testování serializaci objektů

Při návrhu webového rozhraní API, je užitečné pro testování, jak bude serializována datové objekty. Můžete to provést bez vytvoření kontroleru nebo vyvolání akce kontroleru.

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
