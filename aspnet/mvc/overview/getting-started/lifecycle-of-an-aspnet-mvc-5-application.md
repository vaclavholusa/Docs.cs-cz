---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: Životní cyklus aplikace ASP.NET MVC 5 | Dokumentace Microsoftu
author: cephalin
description: Stáhněte si dokument PDF, který grafy životní cyklus aplikace ASP.NET MVC 5. Tento dokument životní cyklus poskytuje podrobný pohled MVC životního cyklu...
ms.author: riande
ms.date: 02/28/2014
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 5523217ae5674ac4e5898a150f9e5f0ae3a70d26
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755992"
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a>Životní cyklus aplikace ASP.NET MVC 5
====================
podle [Cephas dků](https://github.com/cephalin)

[Stáhněte si dokument PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

Tady si můžete stáhnout soubor PDF dokument, který grafy životního cyklu každou aplikaci ASP.NET MVC 5, příjem HTTP žádost o odeslání odpovědi HTTP zpátky do klienta. Je navržená jako vzdělávací nástroj pro ty, kteří začínají s ASP.NET MVC a také jako referenční informace pro uživatele, kteří potřebují k podrobnostem specifických aspektů aplikace. Dokument PDF má následující funkce:

- Relevantní [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) fází, které vám pomohou pochopit, kde MVC integruje do [životního cyklu aplikací ASP.NET](https://msdn.microsoft.com/library/bb470252.aspx).
- Souhrnný přehled životního cyklu aplikace MVC, které vám umožní pochopit hlavní fáze, které prochází každá aplikace MVC v kanálu zpracování požadavků.  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- Zobrazení podrobností, která zobrazuje cvičení dolů do podrobností v kanálu zpracování požadavků. Můžete porovnat vyšší úroveň zobrazení a zobrazení podrobností, abyste viděli, jak podrobnosti životního cyklu se shromáždí do různých fází. [Stáhněte si PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) větší zobrazení.
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- Umístění a účel všechny přepisovatelné metody v [řadič](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) objektu v kanálu zpracování požadavku. Může nebo nemusí být potřeba přepsat všechny jedna metoda, ale je důležité porozumět jejich role v životního cyklu aplikací, takže můžete psát kód ve fázi odpovídající životní cyklus pro efekt, který chcete.
- Úžasné nahoru diagramy zobrazující, jak je vyvolán jednotlivých typů filtrů (ověřování, autorizace, akce a výsledku).
- Odkaz na článek užitečný nebo blogu z každého bodu zájmu v zobrazení podrobností.


## <a name="next-steps"></a>Další kroky

Splňuje tento dokument vašim požadavkům? Uvítáme vaše zpětná vazba. Pokud máte na každou otázku na životní cyklus ASP.NET MVC ve vaší aplikaci [Stackoverflow](http://stackoverflow.com/help) a [fóra ASP.NET MVC](https://forums.asp.net/1146.aspx) jsou užitečná místa, kde dotaz. Postupujte podle [mě](https://twitter.com/Cephas_MSFT) na twitteru, můžete získat aktualizace na můj poslední kurzy.
