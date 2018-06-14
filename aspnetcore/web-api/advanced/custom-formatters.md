---
title: Vlastní formátování v webového rozhraní API ASP.NET Core
author: rick-anderson
description: Zjistěte, jak vytvořit a použít vlastní formátování webové rozhraní API v ASP.NET Core.
manager: wpickett
ms.author: tdykstra
ms.date: 02/08/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: web-api/advanced/custom-formatters
ms.openlocfilehash: ec38a988a73278481de6535c627b2479a9805387
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/11/2018
ms.locfileid: "34452578"
---
# <a name="custom-formatters-in-aspnet-core-web-api"></a>Vlastní formátování v webového rozhraní API ASP.NET Core

Podle [tní Dykstra](https://github.com/tdykstra)

Jádro ASP.NET MVC má integrovanou podporu pro výměnu dat ve webové rozhraní API pomocí formátu JSON, XML nebo prostý text. Tento článek ukazuje, jak přidat podporu dalších formátech tak, že vytvoříte vlastní formátování.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-custom-formatters"></a>Kdy použít vlastní formátování

Pokud chcete použít vlastní formátování [vyjednávání obsahu](xref:web-api/advanced/formatting#content-negotiation) proces pro podporu typ obsahu, který nepodporuje integrované formátovací moduly (JSON, XML a prostý text).

Například, pokud některé z klientů pro webového rozhraní API může zpracovat [Protobuf](https://github.com/google/protobuf) formátu, můžete chtít použít Protobuf s těmito klienty, protože je efektivnější. Nebo můžete chtít webového rozhraní API k odeslání kontaktní názvy a adresy v [soubor vCard](https://wikipedia.org/wiki/VCard) formát, běžně používaný formát pro výměnu kontaktní údaje. Ukázková aplikace součástí v tomto článku implementuje jednoduchý soubor vCard formátování.

## <a name="overview-of-how-to-use-a-custom-formatter"></a>Přehled o tom, jak používat vlastní formátování

Zde jsou kroky, jak vytvořit a používat vlastní formátování:

* Vytvořte třídu formátování výstupu, pokud chcete serializovat data k odeslání do klienta.
* Vytvořte třídu vstupní formátovací modul, pokud chcete k deserializaci data přijatá z klienta.
* Přidání instance formátovací moduly, které `InputFormatters` a `OutputFormatters` kolekcí v [MvcOptions](/dotnet/api/microsoft.aspnetcore.mvc.mvcoptions).

Následující části obsahují pokyny a příklady kódu pro každý z těchto kroků.

## <a name="how-to-create-a-custom-formatter-class"></a>Jak vytvořit vlastní formátování třídu

Pokud chcete vytvořit formátování:

* Třída odvozena od příslušné základní třídy.
* Zadejte platné médium typy a kódování v konstruktoru.
* Přepsání `CanReadType` / `CanWriteType` metody
* Přepsání `ReadRequestBodyAsync` / `WriteResponseBodyAsync` metody
  
### <a name="derive-from-the-appropriate-base-class"></a>Odvozena od příslušné základní třídy

Text typy médií (například soubor vCard), jsou odvozeny od [TextInputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) nebo [TextOutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) základní třídy.

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

Pro binární typy jsou odvozeny od [InputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.inputformatter) nebo [OutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformatter) základní třídy.

### <a name="specify-valid-media-types-and-encodings"></a>Zadejte platné médium typy a kódování

V konstruktoru, určete přidáním do platné médium typy a kódování `SupportedMediaTypes` a `SupportedEncodings` kolekce.

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

> [!NOTE]
> Nelze provést konstruktor vkládání závislostí v třída formátovacího modulu. Například nelze získat protokolovacího nástroje přidáním protokolovacího nástroje parametr konstruktoru. Pro přístup ke službám, budete muset použít objektu context, který získá předané do vaší metody. Příklad kódu [pod](#read-write) ukazuje, jak to udělat.

### <a name="override-canreadtypecanwritetype"></a>Přepsání CanReadType/CanWriteType

Zadejte typ můžete deserializovat do nebo z serializovat přepsáním `CanReadType` nebo `CanWriteType` metody. Například může být pouze nebudete moct vytvořit soubor vCard text ze `Contact` a naopak.

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

#### <a name="the-canwriteresult-method"></a>Metoda CanWriteResult

V některých případech je nutné přepsat `CanWriteResult` místo `CanWriteType`. Použití `CanWriteResult` Pokud jsou splněny následující podmínky:

* Vaše metoda akce vrací třídu modelu.
* Existují odvozené třídy, které může být vrácena za běhu.
* Je třeba vědět za běhu, který odvozené, že byla vrácena třída akce.

Předpokládejme například, vrátí podpis metody akce `Person` typ, ale může vrátit `Student` nebo `Instructor` typ odvozený z `Person`. Pokud chcete, aby vaše formátování, které bude zpracovávat jenom `Student` objekty, zkontrolujte typ [objekt](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) v kontextu objektu poskytnuté `CanWriteResult` metoda. Všimněte si, že není potřeba použít `CanWriteResult` při vrácení metody akce `IActionResult`; v takovém případě `CanWriteType` metoda přijímá typ modulu runtime.

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a>Override ReadRequestBodyAsync/WriteResponseBodyAsync

Vykonávají samotnou práci deserializaci nebo serializace ve `ReadRequestBodyAsync` nebo `WriteResponseBodyAsync`. Zvýrazněné řádky v následujícím příkladu ukazují, jak získat služby z kontejneru pro vkládání závislosti (je již nelze získat z konstruktoru parametrů).

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a>Postup konfigurace MVC používat vlastní formátování

Pokud chcete používat vlastní formátovací modul, přidejte instance třídy pro formátování `InputFormatters` nebo `OutputFormatters` kolekce.

[!code-csharp[](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

Formátovací moduly jsou vyhodnocovány v pořadí, v jakém že jste je vložili. První z nich má přednost před.

## <a name="next-steps"></a>Další kroky

Najdete v článku [ukázkové aplikace](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample), který implementuje jednoduchý soubor vCard vstup a výstup formátování. Aplikace čte a zapisuje vCard, který vypadat podobně jako v následujícím příkladu:

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
