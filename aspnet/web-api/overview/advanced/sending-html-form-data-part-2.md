---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: "Odeslání dat formuláře HTML v rozhraní ASP.NET Web API: souboru nahrávání a vícedílné zprávy standardu MIME | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: 331d0e520a1fd8ec84aecd09a9c9e6d286c5893b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a>Odeslání dat formuláře HTML v rozhraní ASP.NET Web API: souboru nahrávání a vícedílné zprávy standardu MIME
====================
podle [Wasson Jan](https://github.com/MikeWasson)

## <a name="part-2-file-upload-and-multipart-mime"></a>Část 2: Odeslání souboru a vícedílné zprávy standardu MIME

Tento kurz ukazuje, jak k nahrání souborů do webového rozhraní API. Také popisuje způsob zpracování dat vícedílné zprávy standardu MIME.

> [!NOTE]
> [Stáhněte si dokončený projekt](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).


Tady je příklad formuláře HTML pro nahrání souboru:

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

Tento formulář obsahuje ovládacího prvku typu textový vstup a soubor vstupního ovládacího prvku. Pokud formulář obsahuje prvku vstupní soubor **enctype** atribut by měl mít vždy &quot;multipart/formulář data&quot;, která určuje, že formulář bude odeslána jako vícedílné zprávě standardu MIME.

Formát vícedílné zprávě standardu MIME je nejjednodušší pochopit prohlížením požadavek příklad:

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

Tato zpráva je rozdělené do dvou *částí*, jednu pro každý ovládací prvek formuláře. Část hranice jsou označeny řádky, které začínají pomlčky.

> [!NOTE]
> Část hranice zahrnovat náhodných součást (&quot;41184676334&quot;) k zajištění, že řetězec hranic nezobrazí omylem uvnitř část zprávy.


Každá část zprávy obsahuje jednu nebo víc hlaviček, za nímž následuje obsah součásti.

- Hlavička Content-Disposition zahrnuje název ovládacího prvku. Pro soubory také obsahuje název souboru.
- Hlavička Content-Type popisuje data v části. Pokud tuto hlavičku je vynechán, výchozí hodnota je text/plain.

V předchozím příkladu uživatel nahrát soubor s názvem GrandCanyon.jpg, s typ obsahu, který bitovou kopii nebo jpeg; a hodnota textový vstup byla &quot;letní dovolená&quot;.

## <a name="file-upload"></a>Nahrávání souborů

Nyní podíváme, kontroleru webového rozhraní API, který čte soubory z vícedílné zprávě standardu MIME. Řadičem bude číst soubory asynchronně. Webové rozhraní API podporuje asynchronní akce s použitím [založený na úlohách programovací model](https://msdn.microsoft.com/library/dd460693.aspx). Nejprve Toto je kód, pokud cílíte na rozhraní .NET Framework 4.5, která podporuje **asynchronní** a **await** klíčová slova.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

Všimněte si, že akce kontroleru nepřijímá žádné parametry. Je to způsobeno jsme zpracovat datovou část požadavku uvnitř akce, bez vyvolání formátovací modul typu média.

**IsMultipartContent** metoda ověří, zda požadavek obsahuje vícedílné zprávě standardu MIME. Pokud ne, kontroleru vrátí stavový kód HTTP 415 (nepodporovaný typ média).

**MultipartFormDataStreamProvider** třída je pomocný objekt, který přiděluje souborů datových proudů pro odeslané soubory. Číst vícedílné zprávě standardu MIME, volání **ReadAsMultipartAsync** metoda. Tato metoda extrahuje všechny části zprávy a zapisuje je do datové proudy poskytované **MultipartFormDataStreamProvider**.

Po dokončení metody můžete získat informace o souborech z **FileData** vlastnosti, která je kolekce z **MultipartFileData** objekty.

- **MultipartFileData.FileName** je název místního souboru na serveru, kam byl uložen soubor.
- **MultipartFileData.Headers** obsahuje část hlavičky (*není* hlaviček požadavku). Můžete použít pro přístup k obsahu\_záhlaví dispozice a Content-Type.

Jak již název naznačuje, **ReadAsMultipartAsync** je asynchronní metodu. Chcete-li provést nějakou práci po dokončení metody, použijte [úkolů pokračování](https://msdn.microsoft.com/library/ee372288.aspx) (rozhraní .NET 4.0) nebo **await** – klíčové slovo (.NET 4.5).

Tady je verze rozhraní .NET Framework 4.0 předchozí kód:

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a>Čtení dat ovládacího prvku formuláře

Formuláře HTML, který I vám ukázal dříve obsahovala ovládacího prvku typu textový vstup.

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

Můžete získat hodnotu z ovládacího prvku **FormData** vlastnost **MultipartFormDataStreamProvider**.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

**FormData** je **NameValueCollection** obsahující dvojice název/hodnota pro ovládacích prvků formuláře. Kolekce může obsahovat duplicitní klíče. Vezměte v úvahu tento formulář:

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

Text žádosti může vypadat například takto:

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

V takovém případě **FormData** kolekce by obsahovat následující páry klíč – hodnota:

- cesta: round-trip
- možnosti: nonstop
- možnosti: kalendářních dat
- stanici: okna
