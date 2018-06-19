---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Principy spuštění procesu ASP.NET MVC | Microsoft Docs
author: microsoft
description: Zjistěte, jak rozhraní ASP.NET MVC zpracovává žádost prohlížeče krok za krokem.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 5837c6e49709d6b86ee52cd88ffd4759c1850544
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26564523"
---
<a name="understanding-the-aspnet-mvc-execution-process"></a>Principy spuštění procesu ASP.NET MVC
====================
podle [Microsoft](https://github.com/microsoft)

> Zjistěte, jak rozhraní ASP.NET MVC zpracovává žádost prohlížeče krok za krokem.


První předání aplikaci založené na ASP.NET MVC webových požadavků **UrlRoutingModule** objekt, který je modul protokolu HTTP. Tento modul analyzuje požadavku a provede výběr trasy. **UrlRoutingModule** objekt vybere první objekt trasy, který odpovídá aktuální požadavek. (Objekt trasy je třída, která implementuje **RouteBase**, a je obvykle instance **trasy** třídy.) Pokud se shodují se žádné trasy, **UrlRoutingModule** objekt neprovede žádnou akci a vrátit zpět k regulární ASP.NET nebo služby IIS žádosti o zpracování žádosti.

Z vybraného **trasy** objekt, **UrlRoutingModule** získá objektu **IRouteHandler** objekt, který je přidružen **trasy**objektu. Obvykle v aplikaci MVC to bude instance **MvcRouteHandler**. **IRouteHandler** vytvoří instanci **IHttpHandler** objektu a předává je **IHttpContext** objektu. Ve výchozím nastavení **IHttpHandler** instance pro MVC je **MvcHandler** objektu. **MvcHandler** objekt vybere kontroler, který bude žádost zpracovat.

> [!NOTE]
> Při spuštění aplikace ASP.NET MVC Web ve službě IIS 7.0, je vyžadována pro projekty MVC bez přípony názvu souboru. Ale ve službě IIS 6.0, obslužná rutina vyžaduje namapovat příponu názvu souboru .mvc na knihovnu DLL rozhraní ISAPI technologie ASP.NET.


Modul a obslužné rutiny jsou vstupní body rozhraní ASP.NET MVC. Budou provádět následující akce:

- Vyberte příslušný řadič v MVC webovou aplikaci.
- Získejte konkrétní řadič instance.
- Volání kontroleru **Execute** metoda.

Je tomu u provádění na projekt MVC jsou následující:

- Zobrazí první požadavek pro aplikaci 

    - V souboru Global.asax **trasy** objekty jsou přidány na **RouteTable** objektu.
- Provést směrování 

    - **UrlRoutingModule** používá modul pro první odpovídající **trasy** objekt v **RouteTable** kolekce k vytvoření **RouteData** objekt, který pak použije k vytvoření **kontext požadavku** (**IHttpContext**) objektu.
- Vytvořte obslužnou rutinu požadavků MVC 

    - **MvcRouteHandler** vytvoří instanci objektu **MvcHandler** třídy a předává je **kontext požadavku** instance.
- Vytvoření řadiče 

    - **MvcHandler** objektu používá **kontext požadavku** instance k identifikaci **IControllerFactory** objektu (obvykle instance  **DefaultControllerFactory** třída) Chcete-li vytvořit instanci třídy controller s.
- Execute řadič - **MvcHandler** instance volá řadičem s **Execute** metoda. |
- Vyvolání akce 

    - Většina řadičů dědí **řadič** základní třídy. Pro kontrolery, které uděláte tak, že **ControllerActionInvoker** určuje objekt, který je přidružen k řadiči, jakou metodu akce třídy controller volat a potom volá tuto metodu.
- Výsledek spuštění 

    - Metoda typické akce může přijímat vstup uživatele, připravit odpovídající odpověď data a pak spusťte výsledek tak, že vrací typ výsledku. Typy předdefinované výsledků, které mohou být provedeny patří: **ViewResult** (který vykreslí zobrazení a je většina často používané výsledný typ), **RedirectToRouteResult**,  **RedirectResult**, **ContentResult**, **JsonResult**, a **EmptyResult**.
