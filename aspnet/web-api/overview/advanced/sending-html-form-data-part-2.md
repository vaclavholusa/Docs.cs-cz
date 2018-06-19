---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: 'Odeslání dat formuláře HTML v rozhraní ASP.NET Web API: souboru nahrávání a vícedílné zprávy standardu MIME | Microsoft Docs'
author: MikeWasson
description: ''
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
ms.locfileid: "28040139"
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a><span data-ttu-id="e5670-102">Odeslání dat formuláře HTML v rozhraní ASP.NET Web API: souboru nahrávání a vícedílné zprávy standardu MIME</span><span class="sxs-lookup"><span data-stu-id="e5670-102">Sending HTML Form Data in ASP.NET Web API: File Upload and Multipart MIME</span></span>
====================
<span data-ttu-id="e5670-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e5670-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-2-file-upload-and-multipart-mime"></a><span data-ttu-id="e5670-104">Část 2: Odeslání souboru a vícedílné zprávy standardu MIME</span><span class="sxs-lookup"><span data-stu-id="e5670-104">Part 2: File Upload and Multipart MIME</span></span>

<span data-ttu-id="e5670-105">Tento kurz ukazuje, jak k nahrání souborů do webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e5670-105">This tutorial shows how to upload files to a web API.</span></span> <span data-ttu-id="e5670-106">Také popisuje způsob zpracování dat vícedílné zprávy standardu MIME.</span><span class="sxs-lookup"><span data-stu-id="e5670-106">It also describes how to process multipart MIME data.</span></span>

> [!NOTE]
> <span data-ttu-id="e5670-107">[Stáhněte si dokončený projekt](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span><span class="sxs-lookup"><span data-stu-id="e5670-107">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span></span>


<span data-ttu-id="e5670-108">Tady je příklad formuláře HTML pro nahrání souboru:</span><span class="sxs-lookup"><span data-stu-id="e5670-108">Here is an example of an HTML form for uploading a file:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

<span data-ttu-id="e5670-109">Tento formulář obsahuje ovládacího prvku typu textový vstup a soubor vstupního ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="e5670-109">This form contains a text input control and a file input control.</span></span> <span data-ttu-id="e5670-110">Pokud formulář obsahuje prvku vstupní soubor **enctype** atribut by měl mít vždy &quot;multipart/formulář data&quot;, která určuje, že formulář bude odeslána jako vícedílné zprávě standardu MIME.</span><span class="sxs-lookup"><span data-stu-id="e5670-110">When a form contains a file input control, the **enctype** attribute should always be &quot;multipart/form-data&quot;, which specifies that the form will be sent as a multipart MIME message.</span></span>

<span data-ttu-id="e5670-111">Formát vícedílné zprávě standardu MIME je nejjednodušší pochopit prohlížením požadavek příklad:</span><span class="sxs-lookup"><span data-stu-id="e5670-111">The format of a multipart MIME message is easiest to understand by looking at an example request:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

<span data-ttu-id="e5670-112">Tato zpráva je rozdělené do dvou *částí*, jednu pro každý ovládací prvek formuláře.</span><span class="sxs-lookup"><span data-stu-id="e5670-112">This message is divided into two *parts*, one for each form control.</span></span> <span data-ttu-id="e5670-113">Část hranice jsou označeny řádky, které začínají pomlčky.</span><span class="sxs-lookup"><span data-stu-id="e5670-113">Part boundaries are indicated by the lines that start with dashes.</span></span>

> [!NOTE]
> <span data-ttu-id="e5670-114">Část hranice zahrnovat náhodných součást (&quot;41184676334&quot;) k zajištění, že řetězec hranic nezobrazí omylem uvnitř část zprávy.</span><span class="sxs-lookup"><span data-stu-id="e5670-114">The part boundary includes a random component (&quot;41184676334&quot;) to ensure that the boundary string does not accidentally appear inside a message part.</span></span>


<span data-ttu-id="e5670-115">Každá část zprávy obsahuje jednu nebo víc hlaviček, za nímž následuje obsah součásti.</span><span class="sxs-lookup"><span data-stu-id="e5670-115">Each message part contains one or more headers, followed by the part contents.</span></span>

- <span data-ttu-id="e5670-116">Hlavička Content-Disposition zahrnuje název ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="e5670-116">The Content-Disposition header includes the name of the control.</span></span> <span data-ttu-id="e5670-117">Pro soubory také obsahuje název souboru.</span><span class="sxs-lookup"><span data-stu-id="e5670-117">For files, it also contains the file name.</span></span>
- <span data-ttu-id="e5670-118">Hlavička Content-Type popisuje data v části.</span><span class="sxs-lookup"><span data-stu-id="e5670-118">The Content-Type header describes the data in the part.</span></span> <span data-ttu-id="e5670-119">Pokud tuto hlavičku je vynechán, výchozí hodnota je text/plain.</span><span class="sxs-lookup"><span data-stu-id="e5670-119">If this header is omitted, the default is text/plain.</span></span>

<span data-ttu-id="e5670-120">V předchozím příkladu uživatel nahrát soubor s názvem GrandCanyon.jpg, s typ obsahu, který bitovou kopii nebo jpeg; a hodnota textový vstup byla &quot;letní dovolená&quot;.</span><span class="sxs-lookup"><span data-stu-id="e5670-120">In the previous example, the user uploaded a file named GrandCanyon.jpg, with content type image/jpeg; and the value of the text input was &quot;Summer Vacation&quot;.</span></span>

## <a name="file-upload"></a><span data-ttu-id="e5670-121">Nahrávání souborů</span><span class="sxs-lookup"><span data-stu-id="e5670-121">File Upload</span></span>

<span data-ttu-id="e5670-122">Nyní podíváme, kontroleru webového rozhraní API, který čte soubory z vícedílné zprávě standardu MIME.</span><span class="sxs-lookup"><span data-stu-id="e5670-122">Now let's look at a Web API controller that reads files from a multipart MIME message.</span></span> <span data-ttu-id="e5670-123">Řadičem bude číst soubory asynchronně.</span><span class="sxs-lookup"><span data-stu-id="e5670-123">The controller will read the files asynchronously.</span></span> <span data-ttu-id="e5670-124">Webové rozhraní API podporuje asynchronní akce s použitím [založený na úlohách programovací model](https://msdn.microsoft.com/library/dd460693.aspx).</span><span class="sxs-lookup"><span data-stu-id="e5670-124">Web API supports asynchronous actions using the [task-based programming model](https://msdn.microsoft.com/library/dd460693.aspx).</span></span> <span data-ttu-id="e5670-125">Nejprve Toto je kód, pokud cílíte na rozhraní .NET Framework 4.5, která podporuje **asynchronní** a **await** klíčová slova.</span><span class="sxs-lookup"><span data-stu-id="e5670-125">First, here is the code if you are targeting .NET Framework 4.5, which supports the **async** and **await** keywords.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

<span data-ttu-id="e5670-126">Všimněte si, že akce kontroleru nepřijímá žádné parametry.</span><span class="sxs-lookup"><span data-stu-id="e5670-126">Notice that the controller action does not take any parameters.</span></span> <span data-ttu-id="e5670-127">Je to způsobeno jsme zpracovat datovou část požadavku uvnitř akce, bez vyvolání formátovací modul typu média.</span><span class="sxs-lookup"><span data-stu-id="e5670-127">That's because we process the request body inside the action, without invoking a media-type formatter.</span></span>

<span data-ttu-id="e5670-128">**IsMultipartContent** metoda ověří, zda požadavek obsahuje vícedílné zprávě standardu MIME.</span><span class="sxs-lookup"><span data-stu-id="e5670-128">The **IsMultipartContent** method checks whether the request contains a multipart MIME message.</span></span> <span data-ttu-id="e5670-129">Pokud ne, kontroleru vrátí stavový kód HTTP 415 (nepodporovaný typ média).</span><span class="sxs-lookup"><span data-stu-id="e5670-129">If not, the controller returns HTTP status code 415 (Unsupported Media Type).</span></span>

<span data-ttu-id="e5670-130">**MultipartFormDataStreamProvider** třída je pomocný objekt, který přiděluje souborů datových proudů pro odeslané soubory.</span><span class="sxs-lookup"><span data-stu-id="e5670-130">The **MultipartFormDataStreamProvider** class is a helper object that allocates file streams for uploaded files.</span></span> <span data-ttu-id="e5670-131">Číst vícedílné zprávě standardu MIME, volání **ReadAsMultipartAsync** metoda.</span><span class="sxs-lookup"><span data-stu-id="e5670-131">To read the multipart MIME message, call the **ReadAsMultipartAsync** method.</span></span> <span data-ttu-id="e5670-132">Tato metoda extrahuje všechny části zprávy a zapisuje je do datové proudy poskytované **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="e5670-132">This method extracts all of the message parts and writes them into the streams provided by the **MultipartFormDataStreamProvider**.</span></span>

<span data-ttu-id="e5670-133">Po dokončení metody můžete získat informace o souborech z **FileData** vlastnosti, která je kolekce z **MultipartFileData** objekty.</span><span class="sxs-lookup"><span data-stu-id="e5670-133">When the method completes, you can get information about the files from the **FileData** property, which is a collection of **MultipartFileData** objects.</span></span>

- <span data-ttu-id="e5670-134">**MultipartFileData.FileName** je název místního souboru na serveru, kam byl uložen soubor.</span><span class="sxs-lookup"><span data-stu-id="e5670-134">**MultipartFileData.FileName** is the local file name on the server, where the file was saved.</span></span>
- <span data-ttu-id="e5670-135">**MultipartFileData.Headers** obsahuje část hlavičky (*není* hlaviček požadavku).</span><span class="sxs-lookup"><span data-stu-id="e5670-135">**MultipartFileData.Headers** contains the part header (*not* the request header).</span></span> <span data-ttu-id="e5670-136">Můžete použít pro přístup k obsahu\_záhlaví dispozice a Content-Type.</span><span class="sxs-lookup"><span data-stu-id="e5670-136">You can use this to access the Content\_Disposition and Content-Type headers.</span></span>

<span data-ttu-id="e5670-137">Jak již název naznačuje, **ReadAsMultipartAsync** je asynchronní metodu.</span><span class="sxs-lookup"><span data-stu-id="e5670-137">As the name suggests, **ReadAsMultipartAsync** is an asynchronous method.</span></span> <span data-ttu-id="e5670-138">Chcete-li provést nějakou práci po dokončení metody, použijte [úkolů pokračování](https://msdn.microsoft.com/library/ee372288.aspx) (rozhraní .NET 4.0) nebo **await** – klíčové slovo (.NET 4.5).</span><span class="sxs-lookup"><span data-stu-id="e5670-138">To perform work after the method completes, use a [continuation task](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) or the **await** keyword (.NET 4.5).</span></span>

<span data-ttu-id="e5670-139">Tady je verze rozhraní .NET Framework 4.0 předchozí kód:</span><span class="sxs-lookup"><span data-stu-id="e5670-139">Here is the .NET Framework 4.0 version of the previous code:</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a><span data-ttu-id="e5670-140">Čtení dat ovládacího prvku formuláře</span><span class="sxs-lookup"><span data-stu-id="e5670-140">Reading Form Control Data</span></span>

<span data-ttu-id="e5670-141">Formuláře HTML, který I vám ukázal dříve obsahovala ovládacího prvku typu textový vstup.</span><span class="sxs-lookup"><span data-stu-id="e5670-141">The HTML form that I showed earlier had a text input control.</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

<span data-ttu-id="e5670-142">Můžete získat hodnotu z ovládacího prvku **FormData** vlastnost **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="e5670-142">You can get the value of the control from the **FormData** property of the **MultipartFormDataStreamProvider**.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

<span data-ttu-id="e5670-143">**FormData** je **NameValueCollection** obsahující dvojice název/hodnota pro ovládacích prvků formuláře.</span><span class="sxs-lookup"><span data-stu-id="e5670-143">**FormData** is a **NameValueCollection** that contains name/value pairs for the form controls.</span></span> <span data-ttu-id="e5670-144">Kolekce může obsahovat duplicitní klíče.</span><span class="sxs-lookup"><span data-stu-id="e5670-144">The collection can contain duplicate keys.</span></span> <span data-ttu-id="e5670-145">Vezměte v úvahu tento formulář:</span><span class="sxs-lookup"><span data-stu-id="e5670-145">Consider this form:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

<span data-ttu-id="e5670-146">Text žádosti může vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="e5670-146">The request body might look like this:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

<span data-ttu-id="e5670-147">V takovém případě **FormData** kolekce by obsahovat následující páry klíč – hodnota:</span><span class="sxs-lookup"><span data-stu-id="e5670-147">In that case, the **FormData** collection would contain the following key/value pairs:</span></span>

- <span data-ttu-id="e5670-148">cesta: round-trip</span><span class="sxs-lookup"><span data-stu-id="e5670-148">trip: round-trip</span></span>
- <span data-ttu-id="e5670-149">možnosti: nonstop</span><span class="sxs-lookup"><span data-stu-id="e5670-149">options: nonstop</span></span>
- <span data-ttu-id="e5670-150">možnosti: kalendářních dat</span><span class="sxs-lookup"><span data-stu-id="e5670-150">options: dates</span></span>
- <span data-ttu-id="e5670-151">stanici: okna</span><span class="sxs-lookup"><span data-stu-id="e5670-151">seat: window</span></span>
