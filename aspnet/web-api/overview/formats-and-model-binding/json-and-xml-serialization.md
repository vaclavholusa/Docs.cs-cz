---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: JSON a XML serializace v rozhraní ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2012
ms.topic: article
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: b1fcaf70cc38d73da0a454764520197b97f34b26
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a>JSON a XML serializace v rozhraní ASP.NET Web API
====================
podle [Wasson Jan](https://github.com/MikeWasson)

Tento článek popisuje formátování JSON a XML v rozhraní ASP.NET Web API.

V rozhraní ASP.NET Web API *formátovací modul typu média* je objekt, který můžete:

- Tělo zprávy objekty CLR pro čtení z protokolu HTTP
- Zápis objektů CLR do textu zprávy HTTP

Webové rozhraní API poskytuje formátovacích modulů typů médií pro JSON a XML. Rozhraní framework tyto formátování vloží do kanálu ve výchozím nastavení. Klienti mohou požadovat XML nebo JSON v hlavičce Accept požadavku HTTP.

## <a name="contents"></a>Obsah

- [Formátovací modul typu média JSON](#json_media_type_formatter)

    - [Vlastnosti jen pro čtení](#json_readonly)
    - [Kalendářní data](#json_dates)
    - [Odsazení](#json_indenting)
    - [CamelCase](#json_camelcasing)
    - [Anonymní a slabě typované objektů](#json_anon)
- [Formátovací modul typu média XML](#xml_media_type_formatter)

    - [Vlastnosti jen pro čtení](#xml_readonly)
    - [Kalendářní data](#xml_dates)
    - [Odsazení](#xml_indenting)
    - [Nastavení na typ XML Serializátorů](#xml_pertype)
- [Odebrání JSON nebo formátovací modul XML](#removing_the_json_or_xml_formatter)
- [Zpracování objektu cyklické odkazy](#handling_circular_object_references)
- [Testování serializace objektu](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a>Formátovací modul typu média JSON

Formátování JSON zajišťuje **JsonMediaTypeFormatter** třídy. Ve výchozím nastavení **JsonMediaTypeFormatter** používá [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) knihovna k provedení serializace. Json.NET je projekt třetích stran s otevřeným zdrojem.

Pokud dáváte přednost, můžete nakonfigurovat **JsonMediaTypeFormatter** třídu se má použít **DataContractJsonSerializer** místo Json.NET. Chcete-li tak učinit, nastavte **UseDataContractJsonSerializer** vlastnost **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>Serializace JSON

Tato část popisuje některé konkrétní chování formátovací modul JSON, pomocí výchozího [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializátor. To by neměl být úplnou dokumentaci knihovny Json.NET; Další informace najdete v tématu [Json.NET dokumentaci](http://james.newtonking.com/projects/json/help/).

#### <a name="what-gets-serialized"></a>Získá serializovat co?

Ve výchozím nastavení všechny veřejné vlastnosti a pole jsou součástí serializovaných JSON. Vynechat vlastnost nebo pole, uspořádání její **JsonIgnore** atribut.

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

Pokud dáváte přednost &quot;výslovný souhlas&quot; přístupu, uspořádání třídu pomocí **kontraktu** atribut. Pokud tento atribut je k dispozici, jsou členy ignoruje, pokud mají **DataMember**. Můžete také použít **DataMember** k serializaci soukromé členy.

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>Vlastnosti jen pro čtení

Ve výchozím nastavení se serializovat vlastnosti jen pro čtení.

<a id="json_dates"></a>
### <a name="dates"></a>kalendářní data

Ve výchozím nastavení, Json.NET zapisuje data [ISO 8601](http://www.w3.org/TR/NOTE-datetime) formátu. Data v UTC (Coordinated Universal Time) jsou zapsány s příponou "Z". Data v místním čase zahrnují posun časového pásma. Příklad:

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

Ve výchozím nastavení zachovává Json.NET časové pásmo. To se dá přepsat nastavením vlastnosti DateTimeZoneHandling:

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

Pokud byste radši chtěli použít [formát data Microsoft JSON](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) namísto ISO 8601, nastavte **DateFormatHandling** vlastnosti v nastavení serializátoru:

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a>Odsazení

Zápis zobrazují odsazené JSON, nastavte **formátování** nastavení **Formatting.Indented**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a>CamelCase

Zápis názvy vlastností JSON s camelCase, aniž byste museli měnit datový model, nastavte **CamelCasePropertyNamesContractResolver** na serializátor:

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a>Anonymní a slabě typované objektů

Metoda akce vrátit anonymní objekt a serializovat na JSON. Příklad:

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

Zpráva odpovědi bude obsahovat následujícím kódu JSON:

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

Pokud vaše webové rozhraní API obdrží volně strukturovaná objekty JSON od klientů, může deserializovat v textu žádosti **Newtonsoft.Json.Linq.JObject** typu.

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

Je však obvykle lepší objekty silného typu dat použít. Pak nepotřebujete analyzovat data a získejte výhody ověření modelu.

Serializátor XML nepodporuje anonymní typy nebo **JObject** instance. Pokud tyto funkce pro JSON data, byste měli odebrat formátovací modul XML z kanálu, jak je popsáno dále v tomto článku.

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>Formátovací modul typu média XML

Formátování XML zajišťuje **XmlMediaTypeFormatter** třídy. Ve výchozím nastavení **XmlMediaTypeFormatter** používá **DataContractSerializer** třída provést serializace.

Pokud dáváte přednost, můžete nakonfigurovat **XmlMediaTypeFormatter** používat **XmlSerializer** místo **DataContractSerializer**. Chcete-li tak učinit, nastavte **UseXmlSerializer** vlastnost **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

**XmlSerializer** třída podporuje užší sadu typů než **DataContractSerializer**, ale dává větší kontrolu nad výsledný soubor XML. Zvažte použití **XmlSerializer** Pokud potřebujete tak, aby odpovídaly existujícím schématu XML.

### <a name="xml-serialization"></a>Serializace XML

Tato část popisuje některé konkrétní chování formátovací modul XML pomocí výchozího **DataContractSerializer**.

Ve výchozím nastavení se objektu DataContractSerializer chová následovně:

- Všechny veřejné vlastnosti a pole se serializují. Vynechat vlastnost nebo pole, uspořádání její **IgnoreDataMember** atribut.
- Nejsou serializované privátní a chráněné členy.
- Jen pro čtení vlastnosti nejsou serializované. (Však obsah vlastnost kolekce jen pro čtení se serializují.)
- Třídy a člen názvy jsou zapsány v souboru XML, stejně jako se zobrazují v deklaraci třídy.
- Se používá výchozí obor názvů XML.

Pokud potřebujete větší kontrolu nad serializaci, můžete uspořádání třídu pomocí **kontraktu** atribut. Když tento atribut je k dispozici, je třída serializovat následujícím způsobem:

- &quot;Vyjádřit výslovný souhlas&quot; přístup: pole a vlastnosti nejsou serializované ve výchozím nastavení. K serializaci vlastnost nebo pole, uspořádání její **DataMember** atribut.
- K serializaci privátní nebo chráněného člena, uspořádání její **DataMember** atribut.
- Jen pro čtení vlastnosti nejsou serializované.
- Chcete-li změnit, jak se zobrazuje název třídy v souboru XML, nastavte *název* parametr ve **kontraktu** atribut.
- Chcete-li změnit, jak se zobrazuje název člena v souboru XML, nastavte *název* parametr ve **DataMember** atribut.
- Chcete-li změnit obor názvů XML, nastavte *Namespace* parametr ve **kontraktu** třídy.

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>Vlastnosti jen pro čtení

Jen pro čtení vlastnosti nejsou serializované. Pokud je jen pro čtení vlastnosti pole private zálohování, můžete označit privátní pole s **DataMember** atribut. Tento přístup vyžaduje **kontraktu** atribut na třídu.

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>kalendářní data

Data jsou zapsány ve formátu ISO 8601. Například &quot;2012-05-23T20:21:37.9116538Z&quot;.

<a id="xml_indenting"></a>
### <a name="indenting"></a>Odsazení

Zápis zobrazují odsazené XML, nastavte **odsazovat** vlastnost **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>Nastavení na typ XML Serializátorů

Můžete nastavit různé serializátorů XML pro různé typy CLR. Můžete mít například určitý datový objekt, který vyžaduje **XmlSerializer** z důvodu zpětné kompatibility. Můžete použít **XmlSerializer** pro tento objekt a pokračovat v používání **DataContractSerializer** pro ostatní typy.

Chcete-li nastavit serializátor XML pro určitý typ, volejte **SetSerializer**.

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

Můžete zadat **XmlSerializer** nebo libovolného objektu, která je odvozena z **XmlObjectSerializer**.

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>Odebrání JSON nebo formátovací modul XML

Formátovací modul JSON nebo formátovací modul XML můžete odebrat ze seznamu formátování, pokud nechcete používat. Hlavních důvodů k tomu jsou:

- Chcete-li omezit vaší webové rozhraní API odezvy na určitý typ média. Například můžete rozhodnout podporovat pouze odpovědi JSON a odebrat formátovací modul XML.
- Vlastní formátování nahradit výchozí formátování. Například můžete nahradit formátovací modul JSON vlastní vlastní implementaci formátovací modul JSON.

Následující kód ukazuje, jak odebrat výchozí formátování. Volání to z vaší **aplikace\_spustit** metody definované v souboru Global.asax.

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>Zpracování objektu cyklické odkazy

Ve výchozím nastavení JSON a XML formátování zapisovat všechny objekty jako hodnoty. Pokud dvě vlastnosti odkazovat na stejný objekt, nebo pokud se zobrazí na stejný objekt v kolekci dvakrát, formátovací modul bude serializaci objektu dvakrát. To konkrétního problému obsahuje-li je vaše grafu objektu cykly, protože serializátor vyvolá výjimku, když zjistí smyčku v grafu.

Vezměte v úvahu následující objektové modely a kontroleru.

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

Vyvolání tato akce způsobí, že formátování, které bude vyvolána výjimka, která znamená, že je stav kód 500 (vnitřní chyba serveru) odpověď na klientovi.

Chcete-li zachovat odkazy na objekty ve formátu JSON, přidejte následující kód do **aplikace\_spustit** metoda v souboru Global.asax:

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

Nyní akce kontroleru vrátí formátu JSON, který vypadá takto:

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

Všimněte si, že serializátor přidá &quot;$id&quot; vlastnost, která má oba objekty. Navíc rozpozná, že vlastnost Employee.Department vytvoří smyčku, takže ji nahradí hodnotu odkaz na objekt: {&quot;$ref&quot;:&quot;1&quot;}.

> [!NOTE]
> Odkazy na objekty nejsou standardní ve formátu JSON. Před použitím této funkce, zvažte, jestli vaši klienti budou moci analyzovat výsledky. Může to být lepší jednoduše k odebrání cykly grafu. Odkaz z zaměstnanec zpět do oddělení například není skutečně potřeba v tomto příkladu.


Pokud chcete zachovat odkazy na objekty v kódu XML, máte dvě možnosti. Jednodušší možností je přidat `[DataContract(IsReference=true)]` na třídu modelu. *IsReference* parametr povoluje odkazy na objekty. Nezapomeňte, že **kontraktu** díky serializace výslovný souhlas, takže budete také muset přidat **DataMember** atributy na vlastnosti:

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

Nyní vytvoří formátovací modul XML podobně jako na následující:

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

Pokud chcete, aby se zabránilo atributy na vaší třídě modelu, je další možností: Vytvořte nový typ konkrétní **DataContractSerializer** instance a nastavte *preserveObjectReferences* k **true**  v konstruktoru. Pak nastavení této instance jako serializátor-type na formátovací modul XML typu média. Následující kód ukazují, jak to udělat:

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>Testování serializace objektu

Při navrhování webového rozhraní API je užitečné pro testování, jak budou serializovány datových objektů. Můžete provést bez vytvoření řadiče nebo vyvolání akce kontroleru.

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
