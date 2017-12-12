---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: "Použití jazyka HTML5 a kalendáře jQuery UI DatePicker s architekturou ASP.NET MVC – část 3 | Microsoft Docs"
author: Rick-Anderson
description: "V tomto kurzu naučit základní informace o tom, jak pracovat s editor šablon, zobrazení šablon a kalendářem jQuery UI ovládací prvek datepicker v MV ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: dc81961094928025e25cf62ce4d51d12bc67b80c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a>Použití jazyka HTML5 a kalendáře jQuery UI DatePicker s architekturou ASP.NET MVC – část 3
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> V tomto kurzu naučit základní informace o tom, jak pracovat s editor šablon, zobrazení šablon a kalendářem jQuery UI ovládací prvek datepicker v aplikaci ASP.NET MVC Web.


## <a name="working-with-complex-types"></a>Práce s komplexní typy

V této části vytvoříte třídu adresu a zjistěte, jak vytvořit šablonu, můžete ho zobrazit.

V *modely* složky, vytvořte nový soubor třídy s názvem *Person.cs* kam budete umísťovat dva typy: `Person` – třída a `Address` třídy. `Person` Třída bude obsahovat vlastnosti, která je zadán jako `Address`. `Address` Typ je komplexního typu, což znamená, není jeden z předdefinovaných typů jako `int`, `string`, nebo `double`. Místo toho má několik vlastností. Kód pro nové třídy vypadá takto:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

V `Movie` řadiče, přidejte následující `PersonDetail` akce k zobrazení instance osoby:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

Pak přidejte následující kód, který `Movie` řadiče k naplnění `Person` modelu s ukázková data:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

Otevřete *Views\Movies\PersonDetail.cshtml* souboru a přidejte následující kód pro `PersonDetail` zobrazení.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

Stiskněte klávesu Ctrl + F5 a spusťte aplikaci a přejděte do *filmy nebo PersonDetail*.

`PersonDetail` Zobrazení neobsahuje `Address` komplexního typu, jako je můžete zobrazit na tomto snímku obrazovky. (Žádná adresa se zobrazí.)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

`Address` Data modelu není zobrazena, protože je komplexního typu. Chcete-li zobrazit informace o adrese, otevřete *Views\Movies\PersonDetail.cshtml* znovu a přidejte následující kód.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

Kód dokončení pro `PersonDetail` teď zobrazení vypadá takto:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

Spusťte aplikaci znovu a zobrazit `PersonDetail` zobrazení. Informace o adrese se nyní zobrazí:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a>Vytvoření šablony pro komplexní typ.

V této části vytvoříte šablonu, která se použije k vykreslení `Address` komplexního typu. Když vytvoříte šablonu pro `Address` typu, ASP.NET MVC mohou automaticky používat ji k formátování model adresu kdekoli v aplikaci. To vám umožňuje řídit vykreslování `Address` typu z jenom jednom místě v aplikaci.

V *Views\Shared\DisplayTemplates* složky, vytvoření částečné zobrazení silného typu, s názvem **adresu**:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

Klikněte na tlačítko **přidat**a pak otevřete nový *Views\Shared\DisplayTemplates\Address.cshtml* souboru. Nové zobrazení obsahuje následující generovaný kód:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

Spusťte aplikaci a zobrazit `PersonDetail` zobrazení. Tentokrát `Address` šablonu, kterou jste právě vytvořili, se používá k zobrazení `Address` komplexní typ, takže zobrazení vypadá takto:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a>Souhrn: Způsobů určení modelu formát zobrazení a šablony

Už víte, že je možné zadat formát nebo šablonu pro vlastnosti modelu pomocí následujících postupů:

- Použití `DisplayFormat` atribut na vlastnost v modelu. Například následující kód způsobí, že data budou zobrazeny bez čas:

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- Použití [datový typ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) na vlastnost v modelu a zadání datový typ atributu. Například následující kód způsobí, že data budou zobrazeny bez čas.

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    Pokud aplikace obsahuje *date.cshtml* šablonu *Views\Shared\DisplayTemplates* složky nebo *Views\Movies\DisplayTemplates* složku, že šablona slouží k vykreslení `DateTime` vlastnost. V opačném případě předdefinovaného systémového ukázka ASP.NET zobrazí vlastnost jako datum.
- Vytvoření šablony v zobrazení *Views\Shared\DisplayTemplates* složku nebo *Views\Movies\DisplayTemplates* složku, jejíž název odpovídá typu dat, který chcete formátovat. Například, který jste viděli *Views\Shared\DisplayTemplates\DateTime.cshtml* byla použita k vykreslení `DateTime` vlastnosti v modelu, bez přidání atributu do modelu a bez přidání jakékoli značek k zobrazení.
- Pomocí [UIHint](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atributů v modelu, který má zadejte šablonu, kterou chcete zobrazit vlastnosti modelu.
- Přidání explicitně zobrazovaný název šablony na [Html.DisplayFor](https://msdn.microsoft.com/en-us/library/ee407420.aspx) volání v zobrazení.

Přístupů, které použijete, závisí na co musíte udělat v aplikaci. Není kombinovat tyto přístupy k získání přesně druh formátování, které potřebujete.

V další části přepínat trochu zařízením a přesunout z přizpůsobení zobrazení dat k přizpůsobení, jak je zadána. Budete spojit ovládací prvek datepicker jQuery k zobrazení upravit v aplikaci, aby bylo možné poskytnout slick způsob, jak zadat data.

>[!div class="step-by-step"]
[Předchozí](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
[další](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)
