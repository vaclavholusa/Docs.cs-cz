---
title: "Vlastní formátování v aplikaci ASP.NET MVC základní webové rozhraní API"
author: tdykstra
description: "Zjistěte, jak vytvořit a použít vlastní formátování webové rozhraní API v ASP.NET Core."
ms.author: tdykstra
manager: wpickett
ms.date: 02/08/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/custom-formatters
ms.openlocfilehash: 3a6474fdae29b170978226de74d523b20a16cd0c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="custom-formatters-in-aspnet-core-mvc-web-apis"></a>Vlastní formátování v aplikaci ASP.NET MVC základní webové rozhraní API

podle [tní Dykstra](https://github.com/tdykstra)

Jádro ASP.NET MVC má integrovanou podporu pro výměnu dat ve webové rozhraní API pomocí formátu JSON, XML nebo prostý text. Tento článek ukazuje, jak přidat podporu dalších formátech tak, že vytvoříte vlastní formátování.

[Zobrazení nebo stažení ukázky z webu GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample).

## <a name="when-to-use-custom-formatters"></a>Kdy použít vlastní formátování

Pokud chcete použít vlastní formátování [vyjednávání obsahu](xref:mvc/models/formatting) proces pro podporu typ obsahu, který nepodporuje integrované formátovací moduly (JSON, XML a prostý text).

Například, pokud některé z klientů pro webového rozhraní API může zpracovat [Protobuf](https://github.com/google/protobuf) formátu, můžete chtít použít Protobuf s těmito klienty, protože je efektivnější.  Nebo můžete chtít webového rozhraní API k odeslání kontaktní názvy a adresy v [soubor vCard](https://wikipedia.org/wiki/VCard) formát, běžně používaný formát pro výměnu kontaktní údaje. Ukázková aplikace součástí v tomto článku implementuje jednoduchý soubor vCard formátování.

## <a name="overview-of-how-to-use-a-custom-formatter"></a>Přehled o tom, jak používat vlastní formátování

Zde jsou kroky, jak vytvořit a používat vlastní formátování:

* Vytvořte třídu formátování výstupu, pokud chcete serializovat data k odeslání do klienta.
* Vytvořte třídu vstupní formátovací modul, pokud chcete k deserializaci data přijatá z klienta. 
* Přidání instance formátovací moduly, které `InputFormatters` a `OutputFormatters` kolekcí v [MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).

Následující části obsahují pokyny a příklady kódu pro každý z těchto kroků.

## <a name="how-to-create-a-custom-formatter-class"></a>Jak vytvořit vlastní formátování třídu

Pokud chcete vytvořit formátování:

* Třída odvozena od příslušné základní třídy.
* Zadejte platné médium typy a kódování v konstruktoru.
* Přepsání `CanReadType` / `CanWriteType` metody
* Přepsání `ReadRequestBodyAsync` / `WriteResponseBodyAsync` metody
  
### <a name="derive-from-the-appropriate-base-class"></a>Odvozena od příslušné základní třídy

Text typy médií (například soubor vCard), jsou odvozeny od [TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) nebo [TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) základní třídy.

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

Pro binární typy jsou odvozeny od [InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) nebo [OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter) základní třídy.

### <a name="specify-valid-media-types-and-encodings"></a>Zadejte platné médium typy a kódování

V konstruktoru, určete přidáním do platné médium typy a kódování `SupportedMediaTypes` a `SupportedEncodings` kolekce.

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

> [!NOTE]  
> Nelze provést konstruktor vkládání závislostí v třída formátovacího modulu. Například nelze získat protokolovacího nástroje přidáním protokolovacího nástroje parametr konstruktoru. Pro přístup ke službám, budete muset použít objektu context, který získá předané do vaší metody. Příklad kódu [pod](#read-write) ukazuje, jak to udělat.

### <a name="override-canreadtypecanwritetype"></a>Override CanReadType/CanWriteType 

Zadejte typ můžete deserializovat do nebo z serializovat přepsáním `CanReadType` nebo `CanWriteType` metody. Například může být pouze nebudete moct vytvořit soubor vCard text ze `Contact` a naopak.

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

#### <a name="the-canwriteresult-method"></a>Metoda CanWriteResult

V některých případech je nutné přepsat `CanWriteResult` místo `CanWriteType`. Použití `CanWriteResult` Pokud jsou splněny následující podmínky:

  * Vaše metoda akce vrací třídu modelu.
  * Existují odvozené třídy, které může být vrácena za běhu.
  * Je třeba vědět za běhu, který odvozené, že byla vrácena třída akce.  

Předpokládejme například, vrátí podpis metody akce `Person` typ, ale může vrátit `Student` nebo `Instructor` typ odvozený z `Person`. Pokud chcete, aby vaše formátování, které bude zpracovávat jenom `Student` objekty, zkontrolujte typ [objekt](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) v kontextu objektu poskytnuté `CanWriteResult` metoda. Všimněte si, že není potřeba použít `CanWriteResult` při vrácení metody akce `IActionResult`; v takovém případě `CanWriteType` metoda přijímá typ modulu runtime.

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a>Override ReadRequestBodyAsync/WriteResponseBodyAsync 

Vykonávají samotnou práci deserializaci nebo serializace ve `ReadRequestBodyAsync` nebo `WriteResponseBodyAsync`.  Zvýrazněné řádky v následujícím příkladu ukazují, jak získat služby z kontejneru pro vkládání závislosti (je již nelze získat z konstruktoru parametrů).

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a>Postup konfigurace MVC používat vlastní formátování
 
Pokud chcete používat vlastní formátovací modul, přidejte instance třídy pro formátování `InputFormatters` nebo `OutputFormatters` kolekce.

[!code-csharp[Main](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

Formátovací moduly jsou vyhodnocovány v pořadí, v jakém že jste je vložili. První z nich má přednost před. 

## <a name="next-steps"></a>Další kroky

Najdete v článku [ukázkové aplikace](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample), který implementuje jednoduchý soubor vCard vstup a výstup formátování.  Aplikace čte a zapisuje vCard, který vypadat podobně jako v následujícím příkladu:

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

Chcete-li zobrazit soubor vCard výstup, spusťte aplikaci a odešlete požadavek Get s přijmout záhlaví "text/vcard" k `http://localhost:63313/api/contacts/` (při spuštění ze sady Visual Studio) nebo `http://localhost:5000/api/contacts/` (při spuštění z příkazového řádku).

Pro přidání vizitky ke kolekci v paměti kontaktů, odeslat požadavek Post na stejnou adresu URL, s hlavičku Content-Type "text/vcard" a soubor vCard text v těle naformátován jako v předchozím příkladu.
