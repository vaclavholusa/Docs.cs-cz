---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 'Část 8: Poslední stránky, výjimek a uzavření | Microsoft Docs'
author: JoeStagner
description: Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 8 přidá kontaktní stránky, o stránku a výjimky...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: f82294aab0616012393cf3e10f932f6d1ad0cdb6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a>Část 8: Poslední stránky, výjimek a uzavření
====================
podle [Stagner Jan](https://github.com/JoeStagner)

> Tailspin Spyworks ukazuje, jak neobvykle jednoduché je vytvoření efektivní a škálovatelné aplikace pro platformu .NET. Zobrazuje vypnout používání skvělé nové funkce v rozhraní ASP.NET 4 k sestavení online úložiště, včetně nákupy, najdete v článku věnovaném a správu.
> 
> Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 8 přidá kontaktní stránky, o stránku a výjimek. Toto je uzavření řady.


## <a id="_Toc260221680"></a>  Obraťte se na stránce (odesílající e-mailu z prostředí ASP.NET)

Vytvořit novou stránku s názvem ContactUs.aspx

Pomocí návrháře, vytvořte následující formulář vyjádření speciální ToolkitScriptManager a z AjaxdControlToolkit ovládacího prvku Editor. .

![](tailspin-spyworks-part-8/_static/image1.jpg)

Dvakrát klikněte na tlačítko "Odeslat" generování obslužnou rutinu události kliknutí v souboru kódu a implementovat metodu pro odeslání kontaktní informace jako e-mailu.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

Tento kód vyžaduje, aby souboru web.config obsahuje položku v konfiguračním oddílu, který určuje serveru SMTP pro odesílání e-mailu.

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>  O stránku

Vytvořte stránku s názvem AboutUs.aspx a přidejte libovolný obsah se vám líbí.

## <a id="_Toc260221682"></a>  Globální obslužné rutiny výjimek

Nakonec v celé aplikaci jsme mít vyvolání výjimky a existují nepředvídatelnými to cold také příčina neošetřené výjimky v našem webové aplikace.

Chceme nikdy k neošetřené výjimce, který se má zobrazit na webu návštěvníka.

![](tailspin-spyworks-part-8/_static/image2.jpg)

Kromě se strašlivých uživatelské prostředí neošetřených výjimek lze také potíže se zabezpečením.

Chcete-li vyřešit tento problém bude implementaci globální obslužné rutiny výjimek.

Chcete-li to provést, otevřete soubor Global.asax a poznamenejte si obslužná rutina následující předem vygenerovaných událostí.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

Přidejte kód pro implementaci aplikace\_obslužnou rutinu chyby následujícím způsobem.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

Pak přidejte stránku s názvem Error.aspx k řešení a přidejte tento fragment značkového kódu.

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

Nyní na stránce\_načíst extrakce obslužné rutiny událostí chybové zprávy z objektu žádosti.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>  Uzavření

Jste viděli, že webových formulářů ASP.NET umožňuje snadno vytvořit sofistikované web s přístupem k databázi, členství, AJAX, atd. poměrně rychle.

Zpravidla v tomto kurzu jste obdrželi nástroje, které potřebujete, abyste mohli začít vytváření vlastních webových formulářů ASP.NET aplikací!

> [!div class="step-by-step"]
> [Předchozí](tailspin-spyworks-part-7.md)
