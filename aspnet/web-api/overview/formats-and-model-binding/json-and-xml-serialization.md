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
<a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="f3a24-102">JSON a XML serializace v rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="f3a24-102">JSON and XML Serialization in ASP.NET Web API</span></span>
====================
<span data-ttu-id="f3a24-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f3a24-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="f3a24-104">Tento článek popisuje formátování JSON a XML v rozhraní ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="f3a24-104">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="f3a24-105">V rozhraní ASP.NET Web API *formátovací modul typu média* je objekt, který můžete:</span><span class="sxs-lookup"><span data-stu-id="f3a24-105">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="f3a24-106">Tělo zprávy objekty CLR pro čtení z protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="f3a24-106">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="f3a24-107">Zápis objektů CLR do textu zprávy HTTP</span><span class="sxs-lookup"><span data-stu-id="f3a24-107">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="f3a24-108">Webové rozhraní API poskytuje formátovacích modulů typů médií pro JSON a XML.</span><span class="sxs-lookup"><span data-stu-id="f3a24-108">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="f3a24-109">Rozhraní framework tyto formátování vloží do kanálu ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="f3a24-109">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="f3a24-110">Klienti mohou požadovat XML nebo JSON v hlavičce Accept požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="f3a24-110">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="f3a24-111">Obsah</span><span class="sxs-lookup"><span data-stu-id="f3a24-111">Contents</span></span>

- [<span data-ttu-id="f3a24-112">Formátovací modul typu média JSON</span><span class="sxs-lookup"><span data-stu-id="f3a24-112">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="f3a24-113">Vlastnosti jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="f3a24-113">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="f3a24-114">Kalendářní data</span><span class="sxs-lookup"><span data-stu-id="f3a24-114">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="f3a24-115">Odsazení</span><span class="sxs-lookup"><span data-stu-id="f3a24-115">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="f3a24-116">CamelCase</span><span class="sxs-lookup"><span data-stu-id="f3a24-116">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="f3a24-117">Anonymní a slabě typované objektů</span><span class="sxs-lookup"><span data-stu-id="f3a24-117">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="f3a24-118">Formátovací modul typu média XML</span><span class="sxs-lookup"><span data-stu-id="f3a24-118">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="f3a24-119">Vlastnosti jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="f3a24-119">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="f3a24-120">Kalendářní data</span><span class="sxs-lookup"><span data-stu-id="f3a24-120">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="f3a24-121">Odsazení</span><span class="sxs-lookup"><span data-stu-id="f3a24-121">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="f3a24-122">Nastavení na typ XML Serializátorů</span><span class="sxs-lookup"><span data-stu-id="f3a24-122">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="f3a24-123">Odebrání JSON nebo formátovací modul XML</span><span class="sxs-lookup"><span data-stu-id="f3a24-123">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="f3a24-124">Zpracování objektu cyklické odkazy</span><span class="sxs-lookup"><span data-stu-id="f3a24-124">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="f3a24-125">Testování serializace objektu</span><span class="sxs-lookup"><span data-stu-id="f3a24-125">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="f3a24-126">Formátovací modul typu média JSON</span><span class="sxs-lookup"><span data-stu-id="f3a24-126">JSON Media-Type Formatter</span></span>

<span data-ttu-id="f3a24-127">Formátování JSON zajišťuje **JsonMediaTypeFormatter** třídy.</span><span class="sxs-lookup"><span data-stu-id="f3a24-127">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="f3a24-128">Ve výchozím nastavení **JsonMediaTypeFormatter** používá [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) knihovna k provedení serializace.</span><span class="sxs-lookup"><span data-stu-id="f3a24-128">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="f3a24-129">Json.NET je projekt třetích stran s otevřeným zdrojem.</span><span class="sxs-lookup"><span data-stu-id="f3a24-129">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="f3a24-130">Pokud dáváte přednost, můžete nakonfigurovat **JsonMediaTypeFormatter** třídu se má použít **DataContractJsonSerializer** místo Json.NET.</span><span class="sxs-lookup"><span data-stu-id="f3a24-130">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="f3a24-131">Chcete-li tak učinit, nastavte **UseDataContractJsonSerializer** vlastnost **true**:</span><span class="sxs-lookup"><span data-stu-id="f3a24-131">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="f3a24-132">Serializace JSON</span><span class="sxs-lookup"><span data-stu-id="f3a24-132">JSON Serialization</span></span>

<span data-ttu-id="f3a24-133">Tato část popisuje některé konkrétní chování formátovací modul JSON, pomocí výchozího [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializátor.</span><span class="sxs-lookup"><span data-stu-id="f3a24-133">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="f3a24-134">To by neměl být úplnou dokumentaci knihovny Json.NET; Další informace najdete v tématu [Json.NET dokumentaci](http://james.newtonking.com/projects/json/help/).</span><span class="sxs-lookup"><span data-stu-id="f3a24-134">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="f3a24-135">Získá serializovat co?</span><span class="sxs-lookup"><span data-stu-id="f3a24-135">What Gets Serialized?</span></span>

<span data-ttu-id="f3a24-136">Ve výchozím nastavení všechny veřejné vlastnosti a pole jsou součástí serializovaných JSON.</span><span class="sxs-lookup"><span data-stu-id="f3a24-136">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="f3a24-137">Vynechat vlastnost nebo pole, uspořádání její **JsonIgnore** atribut.</span><span class="sxs-lookup"><span data-stu-id="f3a24-137">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="f3a24-138">Pokud dáváte přednost &quot;výslovný souhlas&quot; přístupu, uspořádání třídu pomocí **kontraktu** atribut.</span><span class="sxs-lookup"><span data-stu-id="f3a24-138">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="f3a24-139">Pokud tento atribut je k dispozici, jsou členy ignoruje, pokud mají **DataMember**.</span><span class="sxs-lookup"><span data-stu-id="f3a24-139">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="f3a24-140">Můžete také použít **DataMember** k serializaci soukromé členy.</span><span class="sxs-lookup"><span data-stu-id="f3a24-140">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="f3a24-141">Vlastnosti jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="f3a24-141">Read-Only Properties</span></span>

<span data-ttu-id="f3a24-142">Ve výchozím nastavení se serializovat vlastnosti jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="f3a24-142">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="f3a24-143">kalendářní data</span><span class="sxs-lookup"><span data-stu-id="f3a24-143">Dates</span></span>

<span data-ttu-id="f3a24-144">Ve výchozím nastavení, Json.NET zapisuje data [ISO 8601](http://www.w3.org/TR/NOTE-datetime) formátu.</span><span class="sxs-lookup"><span data-stu-id="f3a24-144">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="f3a24-145">Data v UTC (Coordinated Universal Time) jsou zapsány s příponou "Z".</span><span class="sxs-lookup"><span data-stu-id="f3a24-145">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="f3a24-146">Data v místním čase zahrnují posun časového pásma.</span><span class="sxs-lookup"><span data-stu-id="f3a24-146">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="f3a24-147">Příklad:</span><span class="sxs-lookup"><span data-stu-id="f3a24-147">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="f3a24-148">Ve výchozím nastavení zachovává Json.NET časové pásmo.</span><span class="sxs-lookup"><span data-stu-id="f3a24-148">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="f3a24-149">To se dá přepsat nastavením vlastnosti DateTimeZoneHandling:</span><span class="sxs-lookup"><span data-stu-id="f3a24-149">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="f3a24-150">Pokud byste radši chtěli použít [formát data Microsoft JSON](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) namísto ISO 8601, nastavte **DateFormatHandling** vlastnosti v nastavení serializátoru:</span><span class="sxs-lookup"><span data-stu-id="f3a24-150">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="f3a24-151">Odsazení</span><span class="sxs-lookup"><span data-stu-id="f3a24-151">Indenting</span></span>

<span data-ttu-id="f3a24-152">Zápis zobrazují odsazené JSON, nastavte **formátování** nastavení **Formatting.Indented**:</span><span class="sxs-lookup"><span data-stu-id="f3a24-152">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="f3a24-153">CamelCase</span><span class="sxs-lookup"><span data-stu-id="f3a24-153">Camel Casing</span></span>

<span data-ttu-id="f3a24-154">Zápis názvy vlastností JSON s camelCase, aniž byste museli měnit datový model, nastavte **CamelCasePropertyNamesContractResolver** na serializátor:</span><span class="sxs-lookup"><span data-stu-id="f3a24-154">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="f3a24-155">Anonymní a slabě typované objektů</span><span class="sxs-lookup"><span data-stu-id="f3a24-155">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="f3a24-156">Metoda akce vrátit anonymní objekt a serializovat na JSON.</span><span class="sxs-lookup"><span data-stu-id="f3a24-156">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="f3a24-157">Příklad:</span><span class="sxs-lookup"><span data-stu-id="f3a24-157">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="f3a24-158">Zpráva odpovědi bude obsahovat následujícím kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="f3a24-158">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="f3a24-159">Pokud vaše webové rozhraní API obdrží volně strukturovaná objekty JSON od klientů, může deserializovat v textu žádosti **Newtonsoft.Json.Linq.JObject** typu.</span><span class="sxs-lookup"><span data-stu-id="f3a24-159">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="f3a24-160">Je však obvykle lepší objekty silného typu dat použít.</span><span class="sxs-lookup"><span data-stu-id="f3a24-160">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="f3a24-161">Pak nepotřebujete analyzovat data a získejte výhody ověření modelu.</span><span class="sxs-lookup"><span data-stu-id="f3a24-161">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="f3a24-162">Serializátor XML nepodporuje anonymní typy nebo **JObject** instance.</span><span class="sxs-lookup"><span data-stu-id="f3a24-162">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="f3a24-163">Pokud tyto funkce pro JSON data, byste měli odebrat formátovací modul XML z kanálu, jak je popsáno dále v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="f3a24-163">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="f3a24-164">Formátovací modul typu média XML</span><span class="sxs-lookup"><span data-stu-id="f3a24-164">XML Media-Type Formatter</span></span>

<span data-ttu-id="f3a24-165">Formátování XML zajišťuje **XmlMediaTypeFormatter** třídy.</span><span class="sxs-lookup"><span data-stu-id="f3a24-165">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="f3a24-166">Ve výchozím nastavení **XmlMediaTypeFormatter** používá **DataContractSerializer** třída provést serializace.</span><span class="sxs-lookup"><span data-stu-id="f3a24-166">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="f3a24-167">Pokud dáváte přednost, můžete nakonfigurovat **XmlMediaTypeFormatter** používat **XmlSerializer** místo **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="f3a24-167">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="f3a24-168">Chcete-li tak učinit, nastavte **UseXmlSerializer** vlastnost **true**:</span><span class="sxs-lookup"><span data-stu-id="f3a24-168">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="f3a24-169">**XmlSerializer** třída podporuje užší sadu typů než **DataContractSerializer**, ale dává větší kontrolu nad výsledný soubor XML.</span><span class="sxs-lookup"><span data-stu-id="f3a24-169">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="f3a24-170">Zvažte použití **XmlSerializer** Pokud potřebujete tak, aby odpovídaly existujícím schématu XML.</span><span class="sxs-lookup"><span data-stu-id="f3a24-170">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="f3a24-171">Serializace XML</span><span class="sxs-lookup"><span data-stu-id="f3a24-171">XML Serialization</span></span>

<span data-ttu-id="f3a24-172">Tato část popisuje některé konkrétní chování formátovací modul XML pomocí výchozího **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="f3a24-172">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="f3a24-173">Ve výchozím nastavení se objektu DataContractSerializer chová následovně:</span><span class="sxs-lookup"><span data-stu-id="f3a24-173">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="f3a24-174">Všechny veřejné vlastnosti a pole se serializují.</span><span class="sxs-lookup"><span data-stu-id="f3a24-174">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="f3a24-175">Vynechat vlastnost nebo pole, uspořádání její **IgnoreDataMember** atribut.</span><span class="sxs-lookup"><span data-stu-id="f3a24-175">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="f3a24-176">Nejsou serializované privátní a chráněné členy.</span><span class="sxs-lookup"><span data-stu-id="f3a24-176">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="f3a24-177">Jen pro čtení vlastnosti nejsou serializované.</span><span class="sxs-lookup"><span data-stu-id="f3a24-177">Read-only properties are not serialized.</span></span> <span data-ttu-id="f3a24-178">(Však obsah vlastnost kolekce jen pro čtení se serializují.)</span><span class="sxs-lookup"><span data-stu-id="f3a24-178">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="f3a24-179">Třídy a člen názvy jsou zapsány v souboru XML, stejně jako se zobrazují v deklaraci třídy.</span><span class="sxs-lookup"><span data-stu-id="f3a24-179">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="f3a24-180">Se používá výchozí obor názvů XML.</span><span class="sxs-lookup"><span data-stu-id="f3a24-180">A default XML namespace is used.</span></span>

<span data-ttu-id="f3a24-181">Pokud potřebujete větší kontrolu nad serializaci, můžete uspořádání třídu pomocí **kontraktu** atribut.</span><span class="sxs-lookup"><span data-stu-id="f3a24-181">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="f3a24-182">Když tento atribut je k dispozici, je třída serializovat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f3a24-182">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="f3a24-183">&quot;Vyjádřit výslovný souhlas&quot; přístup: pole a vlastnosti nejsou serializované ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="f3a24-183">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="f3a24-184">K serializaci vlastnost nebo pole, uspořádání její **DataMember** atribut.</span><span class="sxs-lookup"><span data-stu-id="f3a24-184">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="f3a24-185">K serializaci privátní nebo chráněného člena, uspořádání její **DataMember** atribut.</span><span class="sxs-lookup"><span data-stu-id="f3a24-185">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="f3a24-186">Jen pro čtení vlastnosti nejsou serializované.</span><span class="sxs-lookup"><span data-stu-id="f3a24-186">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="f3a24-187">Chcete-li změnit, jak se zobrazuje název třídy v souboru XML, nastavte *název* parametr ve **kontraktu** atribut.</span><span class="sxs-lookup"><span data-stu-id="f3a24-187">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="f3a24-188">Chcete-li změnit, jak se zobrazuje název člena v souboru XML, nastavte *název* parametr ve **DataMember** atribut.</span><span class="sxs-lookup"><span data-stu-id="f3a24-188">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="f3a24-189">Chcete-li změnit obor názvů XML, nastavte *Namespace* parametr ve **kontraktu** třídy.</span><span class="sxs-lookup"><span data-stu-id="f3a24-189">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="f3a24-190">Vlastnosti jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="f3a24-190">Read-Only Properties</span></span>

<span data-ttu-id="f3a24-191">Jen pro čtení vlastnosti nejsou serializované.</span><span class="sxs-lookup"><span data-stu-id="f3a24-191">Read-only properties are not serialized.</span></span> <span data-ttu-id="f3a24-192">Pokud je jen pro čtení vlastnosti pole private zálohování, můžete označit privátní pole s **DataMember** atribut.</span><span class="sxs-lookup"><span data-stu-id="f3a24-192">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="f3a24-193">Tento přístup vyžaduje **kontraktu** atribut na třídu.</span><span class="sxs-lookup"><span data-stu-id="f3a24-193">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="f3a24-194">kalendářní data</span><span class="sxs-lookup"><span data-stu-id="f3a24-194">Dates</span></span>

<span data-ttu-id="f3a24-195">Data jsou zapsány ve formátu ISO 8601.</span><span class="sxs-lookup"><span data-stu-id="f3a24-195">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="f3a24-196">Například &quot;2012-05-23T20:21:37.9116538Z&quot;.</span><span class="sxs-lookup"><span data-stu-id="f3a24-196">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="f3a24-197">Odsazení</span><span class="sxs-lookup"><span data-stu-id="f3a24-197">Indenting</span></span>

<span data-ttu-id="f3a24-198">Zápis zobrazují odsazené XML, nastavte **odsazovat** vlastnost **true**:</span><span class="sxs-lookup"><span data-stu-id="f3a24-198">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="f3a24-199">Nastavení na typ XML Serializátorů</span><span class="sxs-lookup"><span data-stu-id="f3a24-199">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="f3a24-200">Můžete nastavit různé serializátorů XML pro různé typy CLR.</span><span class="sxs-lookup"><span data-stu-id="f3a24-200">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="f3a24-201">Můžete mít například určitý datový objekt, který vyžaduje **XmlSerializer** z důvodu zpětné kompatibility.</span><span class="sxs-lookup"><span data-stu-id="f3a24-201">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="f3a24-202">Můžete použít **XmlSerializer** pro tento objekt a pokračovat v používání **DataContractSerializer** pro ostatní typy.</span><span class="sxs-lookup"><span data-stu-id="f3a24-202">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="f3a24-203">Chcete-li nastavit serializátor XML pro určitý typ, volejte **SetSerializer**.</span><span class="sxs-lookup"><span data-stu-id="f3a24-203">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="f3a24-204">Můžete zadat **XmlSerializer** nebo libovolného objektu, která je odvozena z **XmlObjectSerializer**.</span><span class="sxs-lookup"><span data-stu-id="f3a24-204">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="f3a24-205">Odebrání JSON nebo formátovací modul XML</span><span class="sxs-lookup"><span data-stu-id="f3a24-205">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="f3a24-206">Formátovací modul JSON nebo formátovací modul XML můžete odebrat ze seznamu formátování, pokud nechcete používat.</span><span class="sxs-lookup"><span data-stu-id="f3a24-206">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="f3a24-207">Hlavních důvodů k tomu jsou:</span><span class="sxs-lookup"><span data-stu-id="f3a24-207">The main reasons to do this are:</span></span>

- <span data-ttu-id="f3a24-208">Chcete-li omezit vaší webové rozhraní API odezvy na určitý typ média.</span><span class="sxs-lookup"><span data-stu-id="f3a24-208">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="f3a24-209">Například můžete rozhodnout podporovat pouze odpovědi JSON a odebrat formátovací modul XML.</span><span class="sxs-lookup"><span data-stu-id="f3a24-209">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="f3a24-210">Vlastní formátování nahradit výchozí formátování.</span><span class="sxs-lookup"><span data-stu-id="f3a24-210">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="f3a24-211">Například můžete nahradit formátovací modul JSON vlastní vlastní implementaci formátovací modul JSON.</span><span class="sxs-lookup"><span data-stu-id="f3a24-211">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="f3a24-212">Následující kód ukazuje, jak odebrat výchozí formátování.</span><span class="sxs-lookup"><span data-stu-id="f3a24-212">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="f3a24-213">Volání to z vaší **aplikace\_spustit** metody definované v souboru Global.asax.</span><span class="sxs-lookup"><span data-stu-id="f3a24-213">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="f3a24-214">Zpracování objektu cyklické odkazy</span><span class="sxs-lookup"><span data-stu-id="f3a24-214">Handling Circular Object References</span></span>

<span data-ttu-id="f3a24-215">Ve výchozím nastavení JSON a XML formátování zapisovat všechny objekty jako hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f3a24-215">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="f3a24-216">Pokud dvě vlastnosti odkazovat na stejný objekt, nebo pokud se zobrazí na stejný objekt v kolekci dvakrát, formátovací modul bude serializaci objektu dvakrát.</span><span class="sxs-lookup"><span data-stu-id="f3a24-216">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="f3a24-217">To konkrétního problému obsahuje-li je vaše grafu objektu cykly, protože serializátor vyvolá výjimku, když zjistí smyčku v grafu.</span><span class="sxs-lookup"><span data-stu-id="f3a24-217">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="f3a24-218">Vezměte v úvahu následující objektové modely a kontroleru.</span><span class="sxs-lookup"><span data-stu-id="f3a24-218">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="f3a24-219">Vyvolání tato akce způsobí, že formátování, které bude vyvolána výjimka, která znamená, že je stav kód 500 (vnitřní chyba serveru) odpověď na klientovi.</span><span class="sxs-lookup"><span data-stu-id="f3a24-219">Invoking this action will cause the formatter to thrown an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="f3a24-220">Chcete-li zachovat odkazy na objekty ve formátu JSON, přidejte následující kód do **aplikace\_spustit** metoda v souboru Global.asax:</span><span class="sxs-lookup"><span data-stu-id="f3a24-220">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="f3a24-221">Nyní akce kontroleru vrátí formátu JSON, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="f3a24-221">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="f3a24-222">Všimněte si, že serializátor přidá &quot;$id&quot; vlastnost, která má oba objekty.</span><span class="sxs-lookup"><span data-stu-id="f3a24-222">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="f3a24-223">Navíc rozpozná, že vlastnost Employee.Department vytvoří smyčku, takže ji nahradí hodnotu odkaz na objekt: {&quot;$ref&quot;:&quot;1&quot;}.</span><span class="sxs-lookup"><span data-stu-id="f3a24-223">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="f3a24-224">Odkazy na objekty nejsou standardní ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="f3a24-224">Object references are not standard in JSON.</span></span> <span data-ttu-id="f3a24-225">Před použitím této funkce, zvažte, jestli vaši klienti budou moci analyzovat výsledky.</span><span class="sxs-lookup"><span data-stu-id="f3a24-225">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="f3a24-226">Může to být lepší jednoduše k odebrání cykly grafu.</span><span class="sxs-lookup"><span data-stu-id="f3a24-226">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="f3a24-227">Odkaz z zaměstnanec zpět do oddělení například není skutečně potřeba v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="f3a24-227">For example, the link from Employee back to Department is not really needed in this example.</span></span>


<span data-ttu-id="f3a24-228">Pokud chcete zachovat odkazy na objekty v kódu XML, máte dvě možnosti.</span><span class="sxs-lookup"><span data-stu-id="f3a24-228">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="f3a24-229">Jednodušší možností je přidat `[DataContract(IsReference=true)]` na třídu modelu.</span><span class="sxs-lookup"><span data-stu-id="f3a24-229">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="f3a24-230">*IsReference* parametr povoluje odkazy na objekty.</span><span class="sxs-lookup"><span data-stu-id="f3a24-230">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="f3a24-231">Nezapomeňte, že **kontraktu** díky serializace výslovný souhlas, takže budete také muset přidat **DataMember** atributy na vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="f3a24-231">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="f3a24-232">Nyní vytvoří formátovací modul XML podobně jako na následující:</span><span class="sxs-lookup"><span data-stu-id="f3a24-232">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="f3a24-233">Pokud chcete, aby se zabránilo atributy na vaší třídě modelu, je další možností: Vytvořte nový typ konkrétní **DataContractSerializer** instance a nastavte *preserveObjectReferences* k **true**  v konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="f3a24-233">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="f3a24-234">Pak nastavení této instance jako serializátor-type na formátovací modul XML typu média.</span><span class="sxs-lookup"><span data-stu-id="f3a24-234">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="f3a24-235">Následující kód ukazují, jak to udělat:</span><span class="sxs-lookup"><span data-stu-id="f3a24-235">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="f3a24-236">Testování serializace objektu</span><span class="sxs-lookup"><span data-stu-id="f3a24-236">Testing Object Serialization</span></span>

<span data-ttu-id="f3a24-237">Při navrhování webového rozhraní API je užitečné pro testování, jak budou serializovány datových objektů.</span><span class="sxs-lookup"><span data-stu-id="f3a24-237">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="f3a24-238">Můžete provést bez vytvoření řadiče nebo vyvolání akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="f3a24-238">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
