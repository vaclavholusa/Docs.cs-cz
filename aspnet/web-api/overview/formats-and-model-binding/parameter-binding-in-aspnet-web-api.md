---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: "Parametr vazby v rozhraní ASP.NET Web API | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2013
ms.topic: article
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 5aa532137436922519c86246ebfa834910ac0d86
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="parameter-binding-in-aspnet-web-api"></a>Parametr vazby v rozhraní ASP.NET Web API
====================
podle [Wasson Jan](https://github.com/MikeWasson)

Když webového rozhraní API volá metodu na řadič, je nutné nastavit hodnoty pro parametry procesu označovaného jako *vazby*. Tento článek popisuje, jak webové rozhraní API váže parametry a jak můžete přizpůsobit proces vytváření vazby.

Ve výchozím nastavení používá webového rozhraní API pro svázání parametrů následující pravidla:

- Pokud má parametr hodnotu typu "jednoduché", se pokusí získat hodnotu z identifikátoru URI webového rozhraní API. Jednoduché typy patří .NET [primitivní typy](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **dvojité**, a tak dále), plus **časový interval**, **Data a času**, **Guid**, **decimal**, a **řetězec**, *plus* žádné Zadejte pomocí převaděče typu, který můžete převést na řetězec. (Další informace o převaděče typů později.)
- Pro komplexní typy, webové rozhraní API se pokusí načíst hodnotu z textu zprávy, použití [formátovací modul typu média](media-formatters.md).

Tady je příklad typické metoda kontroleru webového rozhraní API:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

*Id* parametr &quot;jednoduché&quot; typ, takže webového rozhraní API se pokusí získat hodnotu z identifikátoru URI požadavku. *Položky* parametr je komplexního typu, takže webového rozhraní API používá formátovací modul typu média pro čtení hodnotu z textu požadavku.

K získání hodnoty z identifikátoru URI, vypadá webového rozhraní API v datech trasy a řetězce dotazu identifikátoru URI. Data trasy, která se zaplní při směrování systém analyzuje identifikátor URI a odpovídá trasu. Další informace najdete v tématu [směrování a výběr akce](../web-api-routing-and-actions/routing-and-action-selection.md).

Ve zbývající části tohoto článku I budete ukazují, jak můžete přizpůsobit procesu vázání modelu. Pro komplexní typy ale používejte formátovací moduly typu média, kdykoli je to možné. Klíčovým principem HTTP je, že prostředky jsou zasílány v textu zprávy zadejte reprezentace prostředku pomocí vyjednávání obsahu. Formátovací moduly typu média byly navrženy pro přesně tento účel.

## <a name="using-fromuri"></a>Použití [FromUri]

Chcete-li vynutit webového rozhraní API ke čtení pro komplexní typ z identifikátoru URI, přidejte **[FromUri]** atribut parametr. V následujícím příkladu definuje `GeoPoint` typu, společně s metoda kontroleru, který získá `GeoPoint` z identifikátoru URI.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

Klienta můžete vložit hodnoty zeměpisné šířky a délky v řetězci dotazu a webového rozhraní API je použít k vytvoření `GeoPoint`. Příklad:

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a>Použití [FromBody]

Chcete-li vynutit webového rozhraní API pro jednoduchý typ. číst z textu požadavku, přidejte **[FromBody]** atribut parametr:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

V tomto příkladu webového rozhraní API použije formátovací modul typu média číst hodnotu *název* z textu požadavku. Zde je požadavek klienta příklad.

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

Pokud je parametr [FromBody], webového rozhraní API hlavičku Content-Type použije k výběru formátovacího modulu. V tomto příkladu je typ obsahu &quot;application/json&quot; a textu žádosti je nezpracovaný řetězec formátu JSON (není objekt JSON).

Číst z textu zprávy je povoleno maximálně jeden parametr. Proto to nebude fungovat:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

Z důvodu pro toto pravidlo je, že textu žádosti se pravděpodobně uloží do datového proudu bez vyrovnávací paměti, který může číst pouze jednou.

## <a name="type-converters"></a>Převaděče typů

Webové rozhraní API třídy považovat jednoduchého typu (tak, aby webového rozhraní API se pokusí navázat jej z identifikátoru URI) můžete provést vytvořením **TypeConverter** a poskytování převod řetězce.

Následující kód ukazuje `GeoPoint` třídu, která představuje bod zeměpisné plus **TypeConverter** , která převádí z řetězce `GeoPoint` instance. `GeoPoint` Třída je upraven pomocí **[TypeConverter]** atribut k určení převaděč typů. (Tento příklad byl INSPIROVANÉ Karel místo příspěvku na blogu [postupy k vytvoření vazby na vlastní objekty v podpisy akce v MVC nebo WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

Nyní bude považovat webového rozhraní API `GeoPoint` jako jednoduchý typ, což znamená, se pokusí navázat `GeoPoint` parametry z identifikátoru URI. Nemusíte zahrnovat **[FromUri]** na parametru.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

Klienta můžete vyvolat metodu s identifikátorem URI takto:

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a>Vazače modelů

Flexibilnější možnost než převaděče typů je vytvořit vlastní vazač modelu. S vazač modelu máte přístup k objektům, jako požadavek HTTP, popis akce a nezpracované hodnoty z dat trasy.

Pokud chcete vytvořit vazač modelu, implementovat **IModelBinder** rozhraní. Toto rozhraní definuje jedinou metodu **BindModel**:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

Tady je vazač modelu pro `GeoPoint` objekty.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

Získá vazač modelu nezpracované vstupní hodnoty z *zprostředkovatele hodnot*. Tento návrh odděluje dvě odlišné funkce:

- Zprostředkovatel hodnoty přijímá požadavky protokolu HTTP a naplní slovník páry klíč hodnota.
- Vazač modelu používá k naplnění modelu tohoto slovníku.

Výchozí zprostředkovatele hodnot v rozhraní Web API získá hodnoty z data trasy a řetězce dotazu. Například pokud je identifikátor URI `http://localhost/api/values/1?location=48,-122`, zprostředkovatele hodnot vytvoří následující páry klíč hodnota:

- id = &quot;1&quot;
- umístění = &quot;48,122&quot;

(I mě za předpokladu, že výchozí šablonu trasy, která je &quot;rozhraní api nebo {controller} / {id}&quot;.)

Název parametru pro vazbu je uložen v **ModelBindingContext.ModelName** vlastnost. Vazač modelu vyhledá klíč se tato hodnota ve slovníku. Pokud hodnota existuje a mohou být převedeny na `GeoPoint`, přiřadí vázaná hodnota pro vazač modelu **ModelBindingContext.Model** vlastnost.

Všimněte si, že vazač modelu není omezeno na převod jednoduchého typu. V tomto příkladu vazač modelu nejprve hledá v tabulku známé umístění a pokud to nepomůže, používá převod typů.

**Nastavení vazač modelu**

Existuje několik způsobů, jak nastavit vazač modelu. První, můžete přidat **[ModelBinder]** atribut parametr.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

Můžete také přidat **[ModelBinder]** atributu typu. Webové rozhraní API použije vazač modelu zadaného pro všechny parametry daného typu.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

Nakonec můžete přidat zprostředkovatele vazače modelu, aby **HttpConfiguration**. Zprostředkovatele vazače modelu je jednoduše objekt pro vytváření třídy, která vytvoří vazač modelu. Můžete vytvořit poskytovatele odvozování z [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) třídy. Pokud vaše vazač modelu zpracovává jeden typ, je však jednodušší použít integrované **SimpleModelBinderProvider**, která je určená pro tento účel. Následující kód ukazuje, jak to udělat.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

Ke zprostředkovateli vazby modelu, je stále nutné přidat **[ModelBinder]** atribut parametr webového rozhraní API říct, že mají používat vazač modelu a ne formátovací modul typu média. Ale nyní nemusíte určit typ vazače modelu v atributu:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a>Zprostředkovatele hodnot

Mohu uvedené, že získá vazač modelu hodnoty z zprostředkovatele hodnot. Zapisovat zprostředkovatele vlastní hodnoty, implementovat **IValueProvider** rozhraní. Tady je příklad, který vrátí hodnoty ze souborů cookie v požadavku:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

Musíte taky vytvořit factory zprostředkovatele hodnot odvozené z **ValueProviderFactory** třídy.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

Přidat factory zprostředkovatele hodnot na **HttpConfiguration** následujícím způsobem.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

Webové rozhraní API vytvoří všech zprostředkovatelů hodnot, takže když vazač modelu volá **ValueProvider.GetValue**, vazač modelu obdrží hodnotu z první zprostředkovatele hodnot, který se bude moct ji vytvořit.

Alternativně můžete nastavit factory zprostředkovatele hodnot na úrovni parametr pomocí **položku ValueProvider** atribut následujícím způsobem:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

Tato hodnota informuje webového rozhraní API pro použití vazby modelu s objekt pro vytváření zprostředkovatelů zadanou hodnotu a nechcete použít některou z jiných poskytovatelů registrované hodnoty.

## <a name="httpparameterbinding"></a>HttpParameterBinding

Vazače modelů jsou konkrétní instanci obecnější mechanismus. Pokud si prohlédnete **[ModelBinder]** atribut, zobrazí se, že je odvozena z abstraktní **ParameterBindingAttribute** třídy. Tato třída definuje jedinou metodu **GetBinding**, která vrátí **HttpParameterBinding** objektu:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

**HttpParameterBinding** zodpovídá za vytvoření vazby parametru na hodnotu. U **[ModelBinder]**, atribut vrátí **HttpParameterBinding** implementace, která se používá **IModelBinder** k provedení skutečná vazba. Můžete taky implementovat vlastní **HttpParameterBinding**.

Předpokládejme například, že chcete získat značky etag binárním rozsáhlým z `if-match` a `if-none-match` hlavičky v požadavku. Začneme definováním třídy k reprezentaci značky etag binárním rozsáhlým.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

Budeme také definovat výčet, který označuje, jestli chcete-li získat ETag z `if-match` záhlaví nebo `if-none-match` záhlaví.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

Tady je **HttpParameterBinding** , získá z požadované hlavičky značky ETag a sváže s parametrem typu značka ETag:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

**ExecuteBindingAsync** metoda nemá vazbu. V rámci této metody přidat hodnotu parametru vázané **ActionArgument** slovníku v **objekt HttpActionContext**.

> [!NOTE]
> Pokud vaše **ExecuteBindingAsync** metoda čtení textu zprávy požadavku, přepsat **WillReadBody** vlastnosti tak, aby vrátila hodnotu true. Text žádosti může být, že bez vyrovnávací paměti datový proud, který lze číst pouze jednou, takže webového rozhraní API vynucuje pravidlo, že maximálně jeden vazby může číst obsah zprávy.


Chcete-li použít vlastní **HttpParameterBinding**, můžete definovat atribut, který je odvozen od **ParameterBindingAttribute**. Pro `ETagParameterBinding`, budeme definovat dva atributy, jeden pro `if-match` hlavičky a jeden pro `if-none-match` hlavičky. Obě odvozen od abstraktní základní třídu.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

Tady je metoda kontroleru, který používá `[IfNoneMatch]` atribut.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

Kromě **ParameterBindingAttribute**, je jiná háku pro přidání vlastního **HttpParameterBinding**. Na **HttpConfiguration** objekt, **ParameterBindingRules** vlastnost je kolekce anomymous funkce typu (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**). Například můžete přidat pravidlo, které používá libovolný parametr ETag na metodu GET `ETagParameterBinding` s `if-none-match`:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

Funkce by měla vrátit `null` pro parametry, kde vazby není použitelný.

## <a name="iactionvaluebinder"></a>IActionValueBinder

Modulární služby, se řídí celý proces vazbu parametru **IActionValueBinder**. Výchozí implementaci **IActionValueBinder** provede následující akce:

1. Vyhledejte **ParameterBindingAttribute** na parametru. To zahrnuje **[FromBody]**, **[FromUri]**, a **[ModelBinder]**, nebo vlastní atributy.
2. Jinak, vyhledejte v **HttpConfiguration.ParameterBindingRules** pro funkci, která vrátí hodnotu null **HttpParameterBinding**.
3. Jinak použijte výchozí pravidla, která I popsané. 

    - Pokud typ parametru "jednoduchý" nebo má typ převodník, vazbu z identifikátoru URI. Jde o ekvivalent uvedení **[FromUri]** atribut na parametru.
    - Jinak zkuste parametr číst z textu zprávy. Jde o ekvivalent uvedení **[FromBody]** na parametru.

Pokud byste chtěli, může nahradit celý **IActionValueBinder** služby s vlastní implementaci.

## <a name="additional-resources"></a>Další prostředky

[Ukázka vlastního parametru vazby](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

Jan místo napsali dobrý řadu příspěvcích na blogu o vazbu parametru webového rozhraní API:

- [Jak funguje webového rozhraní API parametru vazby](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [Vazba parametru styl MVC pro webové rozhraní API](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [Postup vytvoření vazby na vlastní objekty v podpisy akce v MVC nebo webového rozhraní API](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [Jak vytvořit vlastní hodnotu poskytovatele v rozhraní Web API](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [Vazba parametru webového rozhraní API pod pokličkou](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
