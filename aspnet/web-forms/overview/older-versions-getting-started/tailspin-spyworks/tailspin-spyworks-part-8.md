---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: '8. část: Závěrečné stránky, zpracování výjimek a závěr | Dokumentace Microsoftu'
author: JoeStagner
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 8 přidá stránku kontaktní informace o stránce a výjimek...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 3b49ee53e82933de9b50960779c28ca6ab7441e5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757076"
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a>8. část: Závěrečné stránky, zpracování výjimek a závěr
====================
podle [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks ukazuje, jak mimořádně jednoduché je vytvářet výkonné a škálovatelné aplikace pro platformu .NET. Zobrazuje vypnout použití skvělých nových funkcí v technologii ASP.NET 4 k sestavení nebo online úložiště, včetně nákupu, Pokladna a správu.
> 
> V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 8 přidá stránku kontaktní informace o stránce a zpracování výjimek. Toto je uzavření řady.


## <a id="_Toc260221680"></a>  Obraťte se na stránku (odesílání e-mailu z technologie ASP.NET)

Vytvoří novou stránku s názvem ContactUs.aspx

Pomocí návrháře, vytvořte následující formulář s ohledem ToolkitScriptManager a ovládací prvek editoru z AjaxdControlToolkit zvláštní poznámka. .

![](tailspin-spyworks-part-8/_static/image1.jpg)

Dvakrát klikněte na tlačítko "Odeslat" a vygenerovat obslužnou rutinu události click v souboru kódu na pozadí a implementovat metodu pro odeslání kontaktní informace jako e-mailu.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

Tento kód vyžaduje, že soubor web.config obsahovat záznam v konfiguračním oddílu, který určuje název serveru SMTP pro odeslání e-mailu.

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>  O stránku

Vytvoření stránky s názvem AboutUs.aspx a přidejte libovolný obsah, který vám vyhovuje.

## <a id="_Toc260221682"></a>  Globální obslužné rutiny výjimek

Nakonec v celé aplikaci budeme mít vyvolané výjimky a existují nepředvídatelnými to studenou také příčina neošetřené výjimky v naší webové aplikaci.

Chceme, aby nikdy nezpracovanou výjimku, který se má zobrazit na webu návštěvníka.

![](tailspin-spyworks-part-8/_static/image2.jpg)

Kromě ještěrů uživatelské prostředí se neošetřené výjimky lze také potíže se zabezpečením.

Pro vyřešení tohoto problému jsme implementuje globální obslužné rutiny výjimek.

Chcete-li to provést, otevřete soubor Global.asax a mějte na paměti následující obslužnou rutinu události předběžně vygenerované.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

Přidejte kód k implementaci aplikace\_obslužná rutina chyb následujícím způsobem.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

Pak přidejte stránku s názvem Error.aspx k řešení a přidejte tento fragment kódu.

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

Teď na stránce\_načíst extrakce obslužné rutiny události chybové zprávy z objektu žádosti.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>  Uzavření

Viděli jsme, že využitím součástí ASP.NET WebForms lze snadno vytvořit sofistikované web se přístup k databázi, členství, AJAX, atd. poměrně rychle.

Snad v tomto kurzu, má udělené vám nástroje, které potřebujete, abyste mohli začít vytvářet své vlastní webových formulářů ASP.NET aplikace!

> [!div class="step-by-step"]
> [Předchozí](tailspin-spyworks-part-7.md)
