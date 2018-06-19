---
uid: web-api/overview/error-handling/exception-handling
title: Zpracování výjimek v rozhraní ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2012
ms.topic: article
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: c65ddcca012840d70ab5a33af92edb30041be971
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566392"
---
<a name="exception-handling-in-aspnet-web-api"></a>Zpracování výjimek v rozhraní ASP.NET Web API
====================
podle [Wasson Jan](https://github.com/MikeWasson)

Tento článek popisuje chybu a zpracování výjimek v rozhraní ASP.NET Web API.

- [HttpResponseException](#httpresponserexception)
- [Filtry výjimek](#exception_filters)
- [Registrace filtry výjimek](#registering_exception_filters)
- [HttpError](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a>HttpResponseException

Co se stane, když kontroleru webového rozhraní API obsahuje nezachycenou výjimku? Ve výchozím nastavení jsou většina výjimek přeložit na odpovědi HTTP s kódem stavu 500, vnitřní chyba serveru.

**HttpResponseException** typ je zvláštní případ. Tato výjimka vrátí kód stavu protokolu HTTP, který určíte v konstruktoru výjimka. Například následující metodu vrací 404, nebyl nalezen, pokud *id* parametr není platný.

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

Pro větší kontrolu nad odpovědi, můžete také vytvořit celé zprávy odpovědi a zahrnout jej s **HttpResponseException:** 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a>Filtry výjimek

Můžete přizpůsobit, jak webové rozhraní API ošetřuje výjimky napsáním *filtru výjimek*. Filtr výjimek je spuštěn, když metoda kontroleru vrátí všechny neošetřená výjimka, která je *není* **HttpResponseException** výjimka. **HttpResponseException** typ je zvláštní případ, protože je určená speciálně pro vrácení odpovědi HTTP.

Filtry výjimek implementovat **System.Web.Http.Filters.IExceptionFilter** rozhraní. Nejjednodušší způsob, jak zápis filtru výjimek je odvozena od **System.Web.Http.Filters.ExceptionFilterAttribute** třídy a přepsat **OnException** metoda.

> [!NOTE]
> Filtry výjimek v rozhraní ASP.NET Web API jsou podobné těm, které v architektuře ASP.NET MVC. Však jsou deklarovány v samostatný obor názvů a funkce samostatně. Konkrétně **HandleErrorAttribute** třída používaná v MVC nezpracovává výjimky vyvolané řadiče webového rozhraní API.


Tady je filtr, který převádí **NotImplementedException –** 501, není implementováno kód výjimky do stavu HTTP:

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

**Odpovědi** vlastnost **HttpActionExecutedContext** objekt obsahuje zprávu odpovědi HTTP, která bude odeslána do klienta.

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a>Registrace filtry výjimek

Existuje několik způsobů, jak zaregistrovat filtr výjimek webového rozhraní API:

- Akce
- Adaptérem
- Globálně

Pokud chcete použít filtr na určité akce, přidejte filtr jako atribut na akci:

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

Pokud chcete použít filtr pro všechny akce v kontroleru, přidejte filtr do třídy kontroleru jako atribut:

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

Pokud chcete použít filtr globálně na všechny řadiče webového rozhraní API, přidá instanci filtr, který má **GlobalConfiguration.Configuration.Filters** kolekce. Filtry výjimkou v této kolekci platí pro všechny akce kontroleru webového rozhraní API.

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

Pokud použijete k vytvoření projektu šablony projektu "ASP.NET MVC 4 webové aplikace", uveďte kódu webového rozhraní API konfigurace uvnitř `WebApiConfig` třídy, která se nachází v aplikaci\_spouštěcí složka:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a>HttpError

**HttpError** objekt zajišťuje konzistentní způsob, jak informace o chybě v textu odpovědi. Následující příklad ukazuje, jak vrátit stavový kód protokolu HTTP 404 (Nenalezeno) s **HttpError** v textu odpovědi.

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

**CreateErrorResponse** metody rozšíření je definována v **System.Net.Http.HttpRequestMessageExtensions** třídy. Interně **CreateErrorResponse** vytvoří **HttpError** instance a poté vytvoří **objekt HttpResponseMessage** obsahující **HttpError**.

V tomto příkladu Pokud je metoda úspěšné, vrátí produktu v odpovědi HTTP. Pokud není nalezen požadovaný produkt, obsahuje odpověď HTTP, ale **HttpError** v textu požadavku. Odpovědi může vypadat následovně:

[!code-console[Main](exception-handling/samples/sample9.cmd)]

Všimněte si, že **HttpError** byl serializován do formátu JSON v tomto příkladu. Jednou z výhod použití **HttpError** je, že projde stejné [vyjednávání obsahu](../formats-and-model-binding/content-negotiation.md) a serializace zpracovat jako jakákoli jiná modelem silného typu.

### <a name="httperror-and-model-validation"></a>HttpError a ověření modelu

Pro ověření modelu, můžete předat stav modelu, který má **CreateErrorResponse**, zahrnout chyby ověření v odpovědi:

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

V tomto příkladu může vrátit následující odpověď:

[!code-console[Main](exception-handling/samples/sample11.cmd)]

Další informace o ověření modelu naleznete v tématu [ověření modelu v rozhraní ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).

### <a name="using-httperror-with-httpresponseexception"></a>Pomocí HttpResponseException HttpError

V předchozích příkladech vrátit **objekt HttpResponseMessage** zprávy z akce kontroleru, ale můžete také použít **HttpResponseException** vrátit **HttpError**. Umožňuje vracet modelem silného typu v případě úspěchu normální při vrácení stále **HttpError** Pokud dojde k chybě:

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
