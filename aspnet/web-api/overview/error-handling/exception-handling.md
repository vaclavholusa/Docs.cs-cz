---
uid: web-api/overview/error-handling/exception-handling
title: Zpracování výjimek v rozhraní ASP.NET Web API | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: riande
ms.date: 03/12/2012
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: 62e6187cd82252e7d30f21e03cc4d08418fa39ee
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752004"
---
<a name="exception-handling-in-aspnet-web-api"></a>Zpracování výjimek v rozhraní ASP.NET Web API
====================
podle [Mike Wasson](https://github.com/MikeWasson)

Tento článek popisuje chyb a zpracování výjimek v rozhraní ASP.NET Web API.

- [HttpResponseException](#httpresponserexception)
- [Filtry výjimek](#exception_filters)
- [Registrace filtry výjimek](#registering_exception_filters)
- [HttpError](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a>HttpResponseException

Co se stane, když kontroler webového rozhraní API vrátí nezachycenou výjimku? Ve výchozím nastavení většina výjimek jsou přeloženy do odpovědi HTTP se stavovým kódem 500, vnitřní chyba serveru.

**HttpResponseException** typ je zvláštní případ. Tato výjimka vrátí všechny stavový kód HTTP, který zadáte v konstruktoru výjimky. Například následující metoda vrátí 404, nebyl nalezen, pokud *id* parametr není platný.

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

Pro větší kontrolu nad odpověď a můžete také vytvořit celé zprávy odpovědi zahrnout s **HttpResponseException:** 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a>Filtry výjimek

Můžete přizpůsobit, jak webové rozhraní API zpracovává výjimky napsáním *filtr výjimek*. Filtr výjimek se spustí v případě řadiče metoda vyvolá neošetřená výjimka, která je *není* **HttpResponseException** výjimky. **HttpResponseException** typ je zvláštní případ, protože je navržená speciálně pro vrácení odpovědi HTTP.

Filtry výjimek implementovat **System.Web.Http.Filters.IExceptionFilter** rozhraní. Nejjednodušší způsob, jak zápis filtru výjimek je odvozena z **System.Web.Http.Filters.ExceptionFilterAttribute** třídy a přepsat **OnException** metody.

> [!NOTE]
> Filtry výjimek v rozhraní ASP.NET Web API jsou podobné těm v architektuře ASP.NET MVC. Nicméně jsou deklarovány v samostatném oboru názvů a funkce samostatně. Zejména v případě **HandleErrorAttribute** třída používaná v MVC nezpracovává výjimky vyvolané kontrolerů rozhraní Web API.


Tady je filtr, který převede **NotImplementedException** 501 Neimplementováno kód výjimky na stav protokolu HTTP:

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

**Odpovědi** vlastnost **HttpActionExecutedContext** objekt obsahuje zprávy s odpovědí HTTP, které se pošlou do klienta.

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a>Registrace filtry výjimek

Existuje několik způsobů, jak zaregistrovat filtr výjimek webového rozhraní API:

- Akce
- Adaptér
- Globálně

Použít filtr pro určité akce, přidejte filtr jako atribut na akci:

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

Použít filtr pro všechny akce v kontroleru, přidejte filtr jako atribut třídy kontroleru:

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

Chcete-li použít filtr globálně na všechny řadiče webové rozhraní API, přidejte instance filtru, který **GlobalConfiguration.Configuration.Filters** kolekce. Filtry Geosyncservice v této kolekci se použijí na každou akci kontroleru webového rozhraní API.

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

Pokud používáte šablonu projektu "Aplikace pro Web ASP.NET MVC 4" k vytvoření projektu, ukládejte kód webového rozhraní API konfigurace uvnitř `WebApiConfig` třídu, která se nachází v aplikaci\_spouštěcí složka:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a>HttpError

**HttpError** objekt poskytuje konzistentní způsob, jak vrátit informace o chybě v textu odpovědi. Následující příklad ukazuje, jak vrátit stavový kód HTTP 404 (Nenalezeno) pomocí **HttpError** v textu odpovědi.

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

**CreateErrorResponse** je rozšiřující metody definované v **System.Net.Http.HttpRequestMessageExtensions** třídy. Interně **CreateErrorResponse** vytvoří **HttpError** instance a potom vytvoří **objekt HttpResponseMessage** obsahující **HttpError**.

V tomto příkladu Pokud je metoda úspěšná, vrátí produktu v odpovědi HTTP. Pokud není nalezen požadovaný produkt, obsahuje odpověď HTTP, ale **HttpError** v textu požadavku. Odpověď může vypadat nějak takto:

[!code-console[Main](exception-handling/samples/sample9.cmd)]

Všimněte si, **HttpError** byl serializován do formátu JSON v tomto příkladu. Jednou z výhod použití **HttpError** je, že se prochází stejné [vyjednávání obsahu](../formats-and-model-binding/content-negotiation.md) a serializace zpracovat jako žádný jiný model silného typu.

### <a name="httperror-and-model-validation"></a>HttpError a ověření modelu

Pro ověření modelu, můžete předat stav modelu, který má **CreateErrorResponse**, aby zahrnoval chyby ověření v odpovědi:

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

V tomto příkladu může vrátit následující odpověď:

[!code-console[Main](exception-handling/samples/sample11.cmd)]

Další informace o ověření modelu naleznete v tématu [ověření modelu v rozhraní ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).

### <a name="using-httperror-with-httpresponseexception"></a>Pomocí HttpResponseException HttpError

V předchozích příkladech vrátit **objekt HttpResponseMessage** zprávy z akce kontroleru, ale můžete také použít **HttpResponseException** se vraťte **HttpError**. To umožňuje vracet modelem silného typu v případě úspěchu normální při vrácení stále **HttpError** Pokud dojde k chybě:

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
