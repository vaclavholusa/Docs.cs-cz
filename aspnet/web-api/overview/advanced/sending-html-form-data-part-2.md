---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: 'Posílání dat formulářů HTML v rozhraní ASP.NET Web API: nahrání souboru a vícedílné zprávy standardu MIME | Dokumentace Microsoftu'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: 7c1c85f462141daf747e23aa4215d47f2d263140
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386705"
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a>Posílání dat formulářů HTML v rozhraní ASP.NET Web API: nahrání souboru a vícedílné zprávy standardu MIME
====================
podle [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-2-file-upload-and-multipart-mime"></a>Část 2: Nahrání souboru a vícedílné zprávy standardu MIME

Tento kurz ukazuje postupy při nahrání souborů do webového rozhraní API. Také popisuje, jak můžete zpracovávat data vícedílné zprávy standardu MIME.

> [!NOTE]
> [Stáhnout dokončený projekt](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).


Tady je příklad z formuláře HTML pro nahrání souboru:

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

Tento formulář obsahuje ovládací prvek textového vstupu a soubor vstupního ovládacího prvku. Pokud formulář obsahuje ovládací prvek vstupu souboru **enctype** atribut by měl vždy být &quot;multipart/formulář data&quot;, která určuje, že bude odeslán formulář jako vícedílné zprávě standardu MIME.

Formát vícedílné zprávě standardu MIME je nejjednodušší pochopit zobrazením příklad žádosti:

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

Tato zpráva je rozdělen do dvou *částí*, jeden pro každý ovládací prvek formuláře. Část hranice jsou označeny řádky, které začínají znakem pomlčky.

> [!NOTE]
> Hranice část obsahuje náhodné komponentu (&quot;41184676334&quot;) k zajištění, že hranici řetězec pravděpodobně neobsahuje omylem uvnitř část zprávy.


Každá část zprávy obsahuje jednu nebo víc hlaviček, za nímž následuje část obsahu.

- Hlavička Content-Disposition zahrnuje název ovládacího prvku. Pro soubory neobsahuje také název souboru.
- Hlavička Content-Type popisuje data v části. Pokud je vynechán této hlavičky, výchozí hodnota je text/plain.

V předchozím příkladu se uživatel nahrál soubor s názvem GrandCanyon.jpg, s typem obsahu image/jpeg; a byla zadána hodnota textový vstup &quot;léto dovolené&quot;.

## <a name="file-upload"></a>Nahrání souboru

Nyní Pojďme se podívat na kontroler Web API, která čte soubory z vícedílné zprávě standardu MIME. Kontroler se asynchronně číst soubory. Webové rozhraní API podporuje asynchronní akce [programovací model založený na úlohách](https://msdn.microsoft.com/library/dd460693.aspx). Nejprve, zde je kód rozhraní .NET Framework 4.5, který podporuje při cílení **asynchronní** a **await** klíčová slova.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

Všimněte si, že akce kontroleru nepřijímá žádné parametry. Důvodem je, jsme zpracovat text požadavku v akci, bez vyvolání formátovací modul typu média.

**IsMultipartContent** metoda zkontroluje, jestli žádost obsahuje vícedílné zprávě standardu MIME. V opačném případě kontroleru vrátí stavový kód HTTP 415 (nepodporovaný typ média).

**MultipartFormDataStreamProvider** třída je pomocný objekt, který přiděluje datové proudy souborů o nahraných souborech. Čtení zprávy vícedílné zprávy standardu MIME, zavolejte **ReadAsMultipartAsync** metody. Tato metoda extrahuje všechny části zprávy a zapisuje je do datových proudů poskytované **MultipartFormDataStreamProvider**.

Po dokončení metody můžete získat informace o souborech z **FileData** vlastnost, která je kolekce z **MultipartFileData** objekty.

- **MultipartFileData.FileName** je název místního souboru na serveru, kam byl uložen soubor.
- **MultipartFileData.Headers** obsahuje část hlavičky (*není* záhlaví požadavku). To můžete použít pro přístup k obsahu\_záhlaví dispozice a Content-Type.

Jak název napovídá, **ReadAsMultipartAsync** je asynchronní metodu. K provedení práce po dokončení metody, použijte [úkol pokračování](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) nebo **await** – klíčové slovo (.NET 4.5).

Tady je verze rozhraní .NET Framework 4.0 předchozího kódu:

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a>Čtení dat ovládacího prvku formuláře

Formulář HTML, který jsem ukazoval dříve měli ovládací prvek textového vstupu.

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

Můžete získat hodnotu z ovládacího prvku **FormData** vlastnost **MultipartFormDataStreamProvider**.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

**FormData** je **NameValueCollection** obsahující dvojice název/hodnota pro ovládací prvky formuláře. Kolekce může obsahovat duplicitní klíče. Vezměte v úvahu tento formulář:

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

Text žádosti může vypadat takto:

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

V takovém případě **FormData** kolekce by obsahovala následující dvojice klíč/hodnota:

- o jízdách: zpátečního převodu
- možnosti: nonstop
- možnosti: kalendářních dat
- licence: okno
