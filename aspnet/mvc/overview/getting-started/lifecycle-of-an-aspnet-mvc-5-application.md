---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: "Životní cyklus aplikace ASP.NET MVC 5 | Microsoft Docs"
author: cephalin
description: "Stáhněte si dokument PDF, který grafy životního cyklu aplikace ASP.NET MVC 5. Tento dokument životního cyklu poskytuje podrobný pohled životního cyklu MVC..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/28/2014
ms.topic: article
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 50d58d10c11677fa72ede6a03e686cbde4cbae1d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a>Životní cyklus aplikace ASP.NET MVC 5
====================
podle [Cephas odkazů](https://github.com/cephalin)

[Stáhněte si dokument PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

Zde si můžete stáhnout dokument PDF, který grafy životní cyklus každá aplikace ASP.NET MVC 5, příjem HTTP žádost o odeslání odpovědi HTTP zpět do klienta. Je určený jako výukových nástroj pro ty, kteří jsou nové do architektury ASP.NET MVC i taky jako odkaz pro ty, kteří potřebují přejdete do konkrétních aspektů aplikace. Dokument PDF má následující funkce:

- Relevantní [třídě HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) tak, aby vám pomohou pochopit, kde MVC integruje do [životního cyklu aplikace ASP.NET](https://msdn.microsoft.com/library/bb470252.aspx).
- Široký přehled o životním cyklu aplikace MVC, kde můžete porozumět hlavních fází, které každá aplikace MVC projdou v kanálu zpracování požadavků.  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- Zobrazení podrobností, které obsahuje projde dolů na podrobné informace o kanálu zpracování požadavků. Souhrnné zobrazení a zobrazení podrobností, které chcete zobrazit, jak se shromažďují životní cykly podrobnosti do různých fází, můžete porovnat. [Stáhnout PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) zobrazíte větší zobrazení.
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- Umístění a účel všechny přepisovatelné metody v [řadič](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) objektu v kanálu zpracování požadavku. Může nebo nemusí mít potřebu potlačí všechny jednu metodu, ale je důležité porozumět jejich role v průběhu životního cyklu aplikací, takže můžete napsat kód ve fázi odpovídající životní cyklus účinky, které chcete.
- Diagramy napěněnou up znázorňující, jak každý typ filtru (ověřování, autorizace, akci a výsledek) je vyvolána.
- Propojit užitečné článku nebo blog z každého bodu zájem o podrobné zobrazení.


## <a name="next-steps"></a>Další kroky

Splňuje tento dokument vašim potřebám? Uvítáme vaše názory. Pokud všechny otázky, které máte na rozhraní ASP.NET MVC životního cyklu ve vaší aplikaci, [Stackoverflow](http://stackoverflow.com/help) a [ASP.NET MVC fóra](https://forums.asp.net/1146.aspx) jsou skvělý místech požádat. Postupujte podle [mi](https://twitter.com/Cephas_MSFT) na twitteru, abyste získali aktualizace na můj nejnovější kurzy.
