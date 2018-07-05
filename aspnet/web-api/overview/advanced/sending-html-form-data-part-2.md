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
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a><span data-ttu-id="80bde-102">Posílání dat formulářů HTML v rozhraní ASP.NET Web API: nahrání souboru a vícedílné zprávy standardu MIME</span><span class="sxs-lookup"><span data-stu-id="80bde-102">Sending HTML Form Data in ASP.NET Web API: File Upload and Multipart MIME</span></span>
====================
<span data-ttu-id="80bde-103">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="80bde-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-2-file-upload-and-multipart-mime"></a><span data-ttu-id="80bde-104">Část 2: Nahrání souboru a vícedílné zprávy standardu MIME</span><span class="sxs-lookup"><span data-stu-id="80bde-104">Part 2: File Upload and Multipart MIME</span></span>

<span data-ttu-id="80bde-105">Tento kurz ukazuje postupy při nahrání souborů do webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="80bde-105">This tutorial shows how to upload files to a web API.</span></span> <span data-ttu-id="80bde-106">Také popisuje, jak můžete zpracovávat data vícedílné zprávy standardu MIME.</span><span class="sxs-lookup"><span data-stu-id="80bde-106">It also describes how to process multipart MIME data.</span></span>

> [!NOTE]
> <span data-ttu-id="80bde-107">[Stáhnout dokončený projekt](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span><span class="sxs-lookup"><span data-stu-id="80bde-107">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span></span>


<span data-ttu-id="80bde-108">Tady je příklad z formuláře HTML pro nahrání souboru:</span><span class="sxs-lookup"><span data-stu-id="80bde-108">Here is an example of an HTML form for uploading a file:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

<span data-ttu-id="80bde-109">Tento formulář obsahuje ovládací prvek textového vstupu a soubor vstupního ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="80bde-109">This form contains a text input control and a file input control.</span></span> <span data-ttu-id="80bde-110">Pokud formulář obsahuje ovládací prvek vstupu souboru **enctype** atribut by měl vždy být &quot;multipart/formulář data&quot;, která určuje, že bude odeslán formulář jako vícedílné zprávě standardu MIME.</span><span class="sxs-lookup"><span data-stu-id="80bde-110">When a form contains a file input control, the **enctype** attribute should always be &quot;multipart/form-data&quot;, which specifies that the form will be sent as a multipart MIME message.</span></span>

<span data-ttu-id="80bde-111">Formát vícedílné zprávě standardu MIME je nejjednodušší pochopit zobrazením příklad žádosti:</span><span class="sxs-lookup"><span data-stu-id="80bde-111">The format of a multipart MIME message is easiest to understand by looking at an example request:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

<span data-ttu-id="80bde-112">Tato zpráva je rozdělen do dvou *částí*, jeden pro každý ovládací prvek formuláře.</span><span class="sxs-lookup"><span data-stu-id="80bde-112">This message is divided into two *parts*, one for each form control.</span></span> <span data-ttu-id="80bde-113">Část hranice jsou označeny řádky, které začínají znakem pomlčky.</span><span class="sxs-lookup"><span data-stu-id="80bde-113">Part boundaries are indicated by the lines that start with dashes.</span></span>

> [!NOTE]
> <span data-ttu-id="80bde-114">Hranice část obsahuje náhodné komponentu (&quot;41184676334&quot;) k zajištění, že hranici řetězec pravděpodobně neobsahuje omylem uvnitř část zprávy.</span><span class="sxs-lookup"><span data-stu-id="80bde-114">The part boundary includes a random component (&quot;41184676334&quot;) to ensure that the boundary string does not accidentally appear inside a message part.</span></span>


<span data-ttu-id="80bde-115">Každá část zprávy obsahuje jednu nebo víc hlaviček, za nímž následuje část obsahu.</span><span class="sxs-lookup"><span data-stu-id="80bde-115">Each message part contains one or more headers, followed by the part contents.</span></span>

- <span data-ttu-id="80bde-116">Hlavička Content-Disposition zahrnuje název ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="80bde-116">The Content-Disposition header includes the name of the control.</span></span> <span data-ttu-id="80bde-117">Pro soubory neobsahuje také název souboru.</span><span class="sxs-lookup"><span data-stu-id="80bde-117">For files, it also contains the file name.</span></span>
- <span data-ttu-id="80bde-118">Hlavička Content-Type popisuje data v části.</span><span class="sxs-lookup"><span data-stu-id="80bde-118">The Content-Type header describes the data in the part.</span></span> <span data-ttu-id="80bde-119">Pokud je vynechán této hlavičky, výchozí hodnota je text/plain.</span><span class="sxs-lookup"><span data-stu-id="80bde-119">If this header is omitted, the default is text/plain.</span></span>

<span data-ttu-id="80bde-120">V předchozím příkladu se uživatel nahrál soubor s názvem GrandCanyon.jpg, s typem obsahu image/jpeg; a byla zadána hodnota textový vstup &quot;léto dovolené&quot;.</span><span class="sxs-lookup"><span data-stu-id="80bde-120">In the previous example, the user uploaded a file named GrandCanyon.jpg, with content type image/jpeg; and the value of the text input was &quot;Summer Vacation&quot;.</span></span>

## <a name="file-upload"></a><span data-ttu-id="80bde-121">Nahrání souboru</span><span class="sxs-lookup"><span data-stu-id="80bde-121">File Upload</span></span>

<span data-ttu-id="80bde-122">Nyní Pojďme se podívat na kontroler Web API, která čte soubory z vícedílné zprávě standardu MIME.</span><span class="sxs-lookup"><span data-stu-id="80bde-122">Now let's look at a Web API controller that reads files from a multipart MIME message.</span></span> <span data-ttu-id="80bde-123">Kontroler se asynchronně číst soubory.</span><span class="sxs-lookup"><span data-stu-id="80bde-123">The controller will read the files asynchronously.</span></span> <span data-ttu-id="80bde-124">Webové rozhraní API podporuje asynchronní akce [programovací model založený na úlohách](https://msdn.microsoft.com/library/dd460693.aspx).</span><span class="sxs-lookup"><span data-stu-id="80bde-124">Web API supports asynchronous actions using the [task-based programming model](https://msdn.microsoft.com/library/dd460693.aspx).</span></span> <span data-ttu-id="80bde-125">Nejprve, zde je kód rozhraní .NET Framework 4.5, který podporuje při cílení **asynchronní** a **await** klíčová slova.</span><span class="sxs-lookup"><span data-stu-id="80bde-125">First, here is the code if you are targeting .NET Framework 4.5, which supports the **async** and **await** keywords.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

<span data-ttu-id="80bde-126">Všimněte si, že akce kontroleru nepřijímá žádné parametry.</span><span class="sxs-lookup"><span data-stu-id="80bde-126">Notice that the controller action does not take any parameters.</span></span> <span data-ttu-id="80bde-127">Důvodem je, jsme zpracovat text požadavku v akci, bez vyvolání formátovací modul typu média.</span><span class="sxs-lookup"><span data-stu-id="80bde-127">That's because we process the request body inside the action, without invoking a media-type formatter.</span></span>

<span data-ttu-id="80bde-128">**IsMultipartContent** metoda zkontroluje, jestli žádost obsahuje vícedílné zprávě standardu MIME.</span><span class="sxs-lookup"><span data-stu-id="80bde-128">The **IsMultipartContent** method checks whether the request contains a multipart MIME message.</span></span> <span data-ttu-id="80bde-129">V opačném případě kontroleru vrátí stavový kód HTTP 415 (nepodporovaný typ média).</span><span class="sxs-lookup"><span data-stu-id="80bde-129">If not, the controller returns HTTP status code 415 (Unsupported Media Type).</span></span>

<span data-ttu-id="80bde-130">**MultipartFormDataStreamProvider** třída je pomocný objekt, který přiděluje datové proudy souborů o nahraných souborech.</span><span class="sxs-lookup"><span data-stu-id="80bde-130">The **MultipartFormDataStreamProvider** class is a helper object that allocates file streams for uploaded files.</span></span> <span data-ttu-id="80bde-131">Čtení zprávy vícedílné zprávy standardu MIME, zavolejte **ReadAsMultipartAsync** metody.</span><span class="sxs-lookup"><span data-stu-id="80bde-131">To read the multipart MIME message, call the **ReadAsMultipartAsync** method.</span></span> <span data-ttu-id="80bde-132">Tato metoda extrahuje všechny části zprávy a zapisuje je do datových proudů poskytované **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="80bde-132">This method extracts all of the message parts and writes them into the streams provided by the **MultipartFormDataStreamProvider**.</span></span>

<span data-ttu-id="80bde-133">Po dokončení metody můžete získat informace o souborech z **FileData** vlastnost, která je kolekce z **MultipartFileData** objekty.</span><span class="sxs-lookup"><span data-stu-id="80bde-133">When the method completes, you can get information about the files from the **FileData** property, which is a collection of **MultipartFileData** objects.</span></span>

- <span data-ttu-id="80bde-134">**MultipartFileData.FileName** je název místního souboru na serveru, kam byl uložen soubor.</span><span class="sxs-lookup"><span data-stu-id="80bde-134">**MultipartFileData.FileName** is the local file name on the server, where the file was saved.</span></span>
- <span data-ttu-id="80bde-135">**MultipartFileData.Headers** obsahuje část hlavičky (*není* záhlaví požadavku).</span><span class="sxs-lookup"><span data-stu-id="80bde-135">**MultipartFileData.Headers** contains the part header (*not* the request header).</span></span> <span data-ttu-id="80bde-136">To můžete použít pro přístup k obsahu\_záhlaví dispozice a Content-Type.</span><span class="sxs-lookup"><span data-stu-id="80bde-136">You can use this to access the Content\_Disposition and Content-Type headers.</span></span>

<span data-ttu-id="80bde-137">Jak název napovídá, **ReadAsMultipartAsync** je asynchronní metodu.</span><span class="sxs-lookup"><span data-stu-id="80bde-137">As the name suggests, **ReadAsMultipartAsync** is an asynchronous method.</span></span> <span data-ttu-id="80bde-138">K provedení práce po dokončení metody, použijte [úkol pokračování](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) nebo **await** – klíčové slovo (.NET 4.5).</span><span class="sxs-lookup"><span data-stu-id="80bde-138">To perform work after the method completes, use a [continuation task](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) or the **await** keyword (.NET 4.5).</span></span>

<span data-ttu-id="80bde-139">Tady je verze rozhraní .NET Framework 4.0 předchozího kódu:</span><span class="sxs-lookup"><span data-stu-id="80bde-139">Here is the .NET Framework 4.0 version of the previous code:</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a><span data-ttu-id="80bde-140">Čtení dat ovládacího prvku formuláře</span><span class="sxs-lookup"><span data-stu-id="80bde-140">Reading Form Control Data</span></span>

<span data-ttu-id="80bde-141">Formulář HTML, který jsem ukazoval dříve měli ovládací prvek textového vstupu.</span><span class="sxs-lookup"><span data-stu-id="80bde-141">The HTML form that I showed earlier had a text input control.</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

<span data-ttu-id="80bde-142">Můžete získat hodnotu z ovládacího prvku **FormData** vlastnost **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="80bde-142">You can get the value of the control from the **FormData** property of the **MultipartFormDataStreamProvider**.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

<span data-ttu-id="80bde-143">**FormData** je **NameValueCollection** obsahující dvojice název/hodnota pro ovládací prvky formuláře.</span><span class="sxs-lookup"><span data-stu-id="80bde-143">**FormData** is a **NameValueCollection** that contains name/value pairs for the form controls.</span></span> <span data-ttu-id="80bde-144">Kolekce může obsahovat duplicitní klíče.</span><span class="sxs-lookup"><span data-stu-id="80bde-144">The collection can contain duplicate keys.</span></span> <span data-ttu-id="80bde-145">Vezměte v úvahu tento formulář:</span><span class="sxs-lookup"><span data-stu-id="80bde-145">Consider this form:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

<span data-ttu-id="80bde-146">Text žádosti může vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="80bde-146">The request body might look like this:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

<span data-ttu-id="80bde-147">V takovém případě **FormData** kolekce by obsahovala následující dvojice klíč/hodnota:</span><span class="sxs-lookup"><span data-stu-id="80bde-147">In that case, the **FormData** collection would contain the following key/value pairs:</span></span>

- <span data-ttu-id="80bde-148">o jízdách: zpátečního převodu</span><span class="sxs-lookup"><span data-stu-id="80bde-148">trip: round-trip</span></span>
- <span data-ttu-id="80bde-149">možnosti: nonstop</span><span class="sxs-lookup"><span data-stu-id="80bde-149">options: nonstop</span></span>
- <span data-ttu-id="80bde-150">možnosti: kalendářních dat</span><span class="sxs-lookup"><span data-stu-id="80bde-150">options: dates</span></span>
- <span data-ttu-id="80bde-151">licence: okno</span><span class="sxs-lookup"><span data-stu-id="80bde-151">seat: window</span></span>
