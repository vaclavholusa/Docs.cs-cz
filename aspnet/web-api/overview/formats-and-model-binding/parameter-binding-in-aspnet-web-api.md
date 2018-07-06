---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Vazby parametrů ve webovém rozhraní API technologie ASP.NET | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/11/2013
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 97785db962691c25bac03f904924321af2f6b500
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810079"
---
<a name="parameter-binding-in-aspnet-web-api"></a>Parametr vazby v rozhraní ASP.NET Web API
====================
podle [Mike Wasson](https://github.com/MikeWasson)

Když webové rozhraní API volá metodu na řadiči, je nutné nastavit hodnoty pro parametry procesu nazývaného *vazby*. Tento článek popisuje, jak webové rozhraní API vytvoří vazbu parametrů a jak můžete přizpůsobit proces vytváření vazby.

Ve výchozím nastavení používá webového rozhraní API pro svázání parametrů následující pravidla:

- Pokud je parametr typu "simple", se pokusí získat hodnotu z identifikátoru URI webového rozhraní API. Jednoduché typy zahrnují .NET [primitivní typy](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**a tak dále), a navíc **TimeSpan**, **Data a času**, **Guid**, **desítkové**, a **řetězec**, *plus* jakékoli Typ s konvertor typu, který lze převést z řetězce. (Další informace o typ převaděče později.)
- Pro komplexní typy, webové rozhraní API se pokusí načíst hodnotu z textu zprávy, použití [formátovací modul typu média](media-formatters.md).

Tady je příklad typické metody kontroleru webového rozhraní API:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

*Id* parametr je &quot;jednoduché&quot; zadejte, takže webového rozhraní API se pokusí získat hodnotu z identifikátoru URI požadavku. *Položky* parametr je komplexní typ, takže webového rozhraní API používá formátovací modul typu média načíst hodnotu z textu požadavku.

Pro získání hodnoty z identifikátoru URI, webové rozhraní API vyhledá v datech trasy a řetězci dotazu identifikátoru URI. Data trasy, která se zaplní při směrování systém analyzuje identifikátoru URI a odpovídá trasu. Další informace najdete v tématu [směrování a výběr akce](../web-api-routing-and-actions/routing-and-action-selection.md).

Ve zbývající části tohoto článku ukážeme, jak můžete přizpůsobit proces vytváření vazby modelu. Pro komplexní typy ale zvažte formátovací moduly typu médií, kdykoli je to možné. Klíčovým principem HTTP je, že prostředky se odesílají v textu zprávy, použití vyjednávání obsahu k určení reprezentaci prostředku. Formátovací moduly typu médií byly navrženy pro přesně tento účel.

## <a name="using-fromuri"></a>Použití [FromUri]

Chcete-li vynutit webového rozhraní API pro čtení komplexní typ z identifikátoru URI, přidejte **[FromUri]** atribut parametru. Následující příklad definuje `GeoPoint` typ spolu s řadiči metodu, která získá `GeoPoint` z identifikátoru URI.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

Klienta můžete umístit hodnoty zeměpisné šířky a délky v řetězci dotazu a webového rozhraní API je použít k vytvoření `GeoPoint`. Příklad:

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a>Použití [FromBody]

Chcete-li vynutit webového rozhraní API pro jednoduchý typ. číst z textu požadavku, přidejte **[FromBody]** atribut parametru:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

V tomto příkladu webového rozhraní API používat formátovací modul typu média načíst hodnotu z *název* z textu požadavku. Tady je příklad žádosti klienta.

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

Pokud má parametr [FromBody], webového rozhraní API používá hlavičku Content-Type pro výběr formátovacím modulem. V tomto příkladu je typem obsahu &quot;application/json&quot; a text požadavku má Nezpracovaný řetězec formátu JSON (není objekt JSON).

Maximálně jeden parametr smí číst z textu zprávy. Takže to nebude fungovat:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

Důvod pro toto pravidlo je, že text požadavku můžou být uložená v datovém proudu bez vyrovnávací paměti, který může číst pouze jednou.

## <a name="type-converters"></a>Převaděče typů

Provedete webového rozhraní API třídy považovat jednoduchý typ (tak, aby webové rozhraní API se pokusí navázat z identifikátoru URI) tak, že vytvoříte **TypeConverter** a převodu řetězce.

Následující kód ukazuje `GeoPoint` třídu, která představuje bod zeměpisné navíc **TypeConverter** , která převede z řetězce na `GeoPoint` instancí. `GeoPoint` Třídy je doplněn **[TypeConverter]** atributy konvertor typu. (V tomto příkladu se inspirovat Mike koutů blogový příspěvek [jak vytvořit vazbu na vlastních objektech v signaturách akce v MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

Nyní bude zacházet s webovým rozhraním API `GeoPoint` jako jednoduchý typ., což znamená, ho se pokusí vytvořit vazbu `GeoPoint` parametry z identifikátoru URI. Nemusíte zahrnovat **[FromUri]** u parametru.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

Klient může vyvolat metodu s identifikátorem URI tímto způsobem:

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a>Vazače modelů

Víc možností než konvertor typu je vytvořit vlastní vazač modelu. Díky vazač modelu máte přístup k objektům, jako požadavek HTTP, popis akce a nezpracované hodnoty z dat trasy.

Chcete-li vytvořit vazač modelu, implementovat **IModelBinder** rozhraní. Toto rozhraní definuje jedinou metodu **BindModel**:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

Tady je vazač modelu pro `GeoPoint` objekty.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

Získá vazač modelu nezpracované vstupní hodnoty z *zprostředkovatele hodnot*. Tento návrh odděluje dvě různé funkce:

- Zprostředkovatel hodnoty trvá požadavek HTTP a naplní slovník párů klíč hodnota.
- Vazač modelu používá k naplnění modelu tohoto slovníku.

Výchozí zprostředkovatel hodnoty v rozhraní Web API získá hodnoty z dat trasy a řetězce dotazu. Například, pokud je identifikátor URI `http://localhost/api/values/1?location=48,-122`, zprostředkovatele hodnot vytvoří následující páry klíč hodnota:

- ID = &quot;1&quot;
- umístění = &quot;48,122&quot;

(I jsem za předpokladu, že výchozí šablona trasy, která je &quot;rozhraní api / {controller} / {id}&quot;.)

Název parametru pro vazbu je uložen v **ModelBindingContext.ModelName** vlastnost. Vazač modelu hledá klíče této hodnoty ve slovníku. Pokud hodnota existuje a mohou být převedeny na `GeoPoint`, vazače modelu přiřadí hodnotu do proměnné vázané **ModelBindingContext.Model** vlastnost.

Všimněte si, že není omezena pouze na převod jednoduchý typ vazače modelu. V tomto příkladu nejprve hledá vazače modelu v tabulce ze známého umístění a pokud selže, používá převod typů.

**Nastavení vazače modelu**

Existuje několik způsobů, jak nastavit vazač modelu. Nejprve můžete přidat **[ModelBinder]** atribut parametru.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

Můžete také přidat **[ModelBinder]** atribut typu. Webové rozhraní API používat vazač modelu zadaného pro všechny parametry typu.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

Nakonec můžete přidat poskytovatel vazač modelu pro **HttpConfiguration**. Poskytovatel vazač modelu je jednoduše třídu objektů factory vytvoří vazač modelu. Můžete vytvořit poskytovatele odvozením z [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) třídy. Pokud vaše vazač modelu zpracovává jeden typ, je ale jednodušší použít integrovaný **SimpleModelBinderProvider**, která je určená pro tento účel. Následující kód ukazuje, jak to provést.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

U nějakého poskytovatele vazby modelu je stále potřeba přidat **[ModelBinder]** atribut parametru webového rozhraní API říct, že by měl používat vazač modelu a formátování typu média. Ale nyní není nutné zadat typ vazače modelu v atributu:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a>Zprostředkovatele hodnot

Jsem se zmiňoval, že získá vazač modelu hodnot zprostředkovatele hodnot. Chcete-li napsat vlastní hodnotu zprostředkovatele, implementovat **IValueProvider** rozhraní. Tady je příklad, který načítá hodnoty ze souborů cookie v požadavku:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

Musíte také vytvořit factory zprostředkovatele hodnot odvozené ze **ValueProviderFactory** třídy.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

Přidat factory zprostředkovatele hodnot pro **HttpConfiguration** následujícím způsobem.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

Webové rozhraní API lze kombinovat všech zprostředkovatelů hodnot, takže když volá vazač modelu **ValueProvider.GetValue**, vazače modelu přijímá hodnotu z první zprostředkovatele hodnot, aby bylo možné ji vytvořit.

Alternativně můžete nastavit factory zprostředkovatele hodnot na úrovni parametr pomocí **položku ValueProvider** atribut následujícím způsobem:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

To říká webového rozhraní API pro použití vazby modelu s factory zprostředkovatele hodnot pro zadaný a ne k používání některé z dalších poskytovatelů registrované hodnoty.

## <a name="httpparameterbinding"></a>HttpParameterBinding

Vazače modelů jsou konkrétní instanci obecnější mechanismus. Když se podíváte na **[ModelBinder]** atribut, zobrazí se, že se odvozuje od abstraktní **ParameterBindingAttribute** třídy. Tato třída definuje jedinou metodu **GetBinding**, která vrátí **HttpParameterBinding** objektu:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

**HttpParameterBinding** zodpovídá za vazby parametru na hodnotu. V případě třídy **[ModelBinder]**, atribut vrátí **HttpParameterBinding** implementace, která se používá **IModelBinder** provádět skutečná vazba. Můžete také implementovat vlastní **HttpParameterBinding**.

Předpokládejme například, že chcete získat značek etag z `if-match` a `if-none-match` hlavičky v požadavku. Začneme tak, že definujete třídu k vyjádření značek ETag.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

Budeme také definovat výčet, který označuje, jestli se má získat značku ETag ze `if-match` záhlaví nebo `if-none-match` záhlaví.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

Tady je **HttpParameterBinding** , který získá z požadovaného záhlaví ETag a provádí vazbu na parametr typu ETag:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

**ExecuteBindingAsync** metoda nemá vazbu. V rámci této metody přidat hodnotu vazby parametru **ActionArgument** slovníku **HttpActionContext**.

> [!NOTE]
> Pokud vaše **ExecuteBindingAsync** metoda načte text zprávy s požadavkem, přepsat **WillReadBody** vlastnost vrátí hodnotu true. Text žádosti může být, že bez vyrovnávací paměti datového proudu, který lze číst pouze jednou, takže webového rozhraní API vynucuje pravidla, že nejvýše jedna vazba můžete číst hlavní část textu zprávy.


Chcete-li použít vlastní **HttpParameterBinding**, můžete definovat, která je odvozena z atributu **ParameterBindingAttribute**. Pro `ETagParameterBinding`, budeme definovat dva atributy, jeden pro `if-match` záhlaví a jeden pro `if-none-match` záhlaví. Obě jsou odvozeny od abstraktní základní třídy.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

Tady je metoda kontroleru, který používá `[IfNoneMatch]` atribut.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

Kromě **ParameterBindingAttribute**, existuje jiný vidlici pro přidání vlastního **HttpParameterBinding**. Na **HttpConfiguration** objektu, **ParameterBindingRules** vlastnost je kolekce anomymous funkce typu (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**). Například můžete přidat pravidlo, které používá žádné značky ETag parametry pro metodu GET `ETagParameterBinding` s `if-none-match`:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

Funkce by měla vrátit `null` pro parametry, pokud vazba není použitelný.

## <a name="iactionvaluebinder"></a>IActionValueBinder

Celý proces vazbu parametru je řízen modulární služby **IActionValueBinder**. Výchozí implementace **IActionValueBinder** provede následující akce:

1. Vyhledejte **ParameterBindingAttribute** u parametru. Jedná se o **[FromBody]**, **[FromUri]**, a **[ModelBinder]**, nebo vlastní atributy.
2. V opačném případě Hledat v **HttpConfiguration.ParameterBindingRules** pro funkci, která vrací jinou hodnotu null **HttpParameterBinding**.
3. Jinak použijte výchozí pravidla, které můžu je popsáno výše. 

    - Pokud typ parametru je "jednoduchý" nebo má konvertor typu, navázat z identifikátoru URI. Jedná se o ekvivalent při vložení **[FromUri]** atribut parametru.
    - V opačném případě pokusu o čtení parametr z textu zprávy. Jedná se o ekvivalent při vložení **[FromBody]** u parametru.

Pokud byste chtěli, může nahradit celou **IActionValueBinder** služba s vlastní implementaci.

## <a name="additional-resources"></a>Další prostředky

[Ukázka vlastního parametru vazby](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

Pozastavení provádění získání Mike napsal dobré řadě blogových příspěvků o vazbu parametru webového rozhraní API:

- [Jak webové rozhraní API nepodporuje parametr vazby](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [Vazby parametru styl MVC pro webové rozhraní API](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [Jak vytvořit vazbu na vlastních objektech v signaturách akce v MVC nebo webového rozhraní API](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [Jak vytvořit vlastní hodnotu zprostředkovatele v rozhraní Web API](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [Vazby parametru webového rozhraní API pohled pod kapotu](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
