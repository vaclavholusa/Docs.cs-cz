---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Použití jazyka HTML5 a kalendáře jQuery UI Datepicker s architekturou ASP.NET MVC – část 3 | Dokumentace Microsoftu
author: Rick-Anderson
description: V tomto kurzu se seznámíte se základy práce pomocí editoru šablon, šablony zobrazení a kalendářem jQuery uživatelského rozhraní prvkem datepicker v MV ASP.NET...
ms.author: aspnetcontent
ms.date: 08/29/2011
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: 27bbc4e1df6e26eed66680d596d13863dfab5db0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812267"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a>Použití jazyka HTML5 a kalendáře jQuery UI Datepicker s architekturou ASP.NET MVC – část 3
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> V tomto kurzu se seznámíte se základy práce pomocí editoru šablon, šablony zobrazení a kalendářem jQuery UI datepicker v aplikaci MVC rozhraní ASP.NET Web.


## <a name="working-with-complex-types"></a>Práce s komplexní typy

V této části vytvoříte třídu adresu a zjistěte, jak vytvořit šablonu, která ho zobrazit.

V *modely* složku, vytvořte nový soubor třídy *Person.cs* místo, kam budete dáte dva typy: `Person` třídy a `Address` třídy. `Person` Třídy bude obsahovat vlastnosti, která je zadána jako `Address`. `Address` Typ je komplexní typ, což znamená není jeden z předdefinovaných typů, jako je `int`, `string`, nebo `double`. Místo toho má několik vlastností. Kód pro nové třídy vypadá takto:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

V `Movie` kontroleru, přidejte následující `PersonDetail` akce k zobrazení instance osoby:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

Pak přidejte následující kód, který `Movie` kontroleru k naplnění `Person` modelů s ukázkovými daty:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

Otevřít *Views\Movies\PersonDetail.cshtml* a přidejte následující kód pro `PersonDetail` zobrazení.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

Stisknutím kláves Ctrl + F5 spusťte aplikaci a přejděte do *filmy/PersonDetail*.

`PersonDetail` Zobrazení neobsahuje `Address` komplexní typ, jako je vidět na tomto snímku obrazovky. (Žádná adresa se zobrazí.)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

`Address` Datového modelu se nezobrazí, protože se jedná o komplexní typ. Chcete-li zobrazit informace o adrese, otevřete *Views\Movies\PersonDetail.cshtml* znovu a přidejte následující kód.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

Kompletní kód pro `PersonDetail` teď zobrazení vypadá takto:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

Spusťte aplikaci znovu spustit a zobrazit `PersonDetail` zobrazení. Informace o adrese se nyní zobrazí:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a>Vytvoření šablony pro komplexní typ.

V této části vytvoříte šablonu, která se použije k vykreslení `Address` komplexního typu. Když vytvoříte šablonu pro `Address` typ, ASP.NET MVC automaticky slouží k formátování modelu adresu kdekoli v aplikaci. To vám umožňuje řídit vykreslování `Address` typ z jediného místa v aplikaci.

V *views\shared\displaytemplates za účelem nalezení* složku vytvořit částečné zobrazení silného typu, s názvem **adresu**:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

Klikněte na tlačítko **přidat**a pak otevřete nový *Views\Shared\DisplayTemplates\Address.cshtml* souboru. Nové zobrazení obsahuje následující vygenerované značky:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

Spusťte aplikaci a zobrazit `PersonDetail` zobrazení. Tentokrát `Address` šablonu, kterou jste právě vytvořili, se používá k zobrazení `Address` komplexní typ, tak zobrazení vypadá takto:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a>Shrnutí: Způsoby, jak určit formát zobrazení modelu a šablony

Už víte, že můžete určit formát nebo šablonu pro vlastnosti modelu pomocí následujících postupů:

- Použití `DisplayFormat` atribut na vlastnost v modelu. Například následující kód způsobí, že data budou zobrazeny bez času:

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- Použití [datový typ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atribut na vlastnost v modelu a určení datového typu. Například následující kód způsobí, že data budou zobrazeny bez času.

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    Pokud aplikace obsahuje soubor *date.cshtml* šablony v *views\shared\displaytemplates za účelem nalezení* složky nebo *Views\Movies\DisplayTemplates* složky, tato šablona se použije k vykreslení `DateTime` vlastnost. Integrované systémové šablon ASP.NET jinak zobrazí vlastnost jako datum.
- Vytvoření šablony v zobrazení *views\shared\displaytemplates za účelem nalezení* složky nebo *Views\Movies\DisplayTemplates* složku, jejíž název odpovídá datový typ, který má být zformátován. Například jste si všimli, že *Views\Shared\DisplayTemplates\DateTime.cshtml* byla použita k vykreslení `DateTime` vlastnosti v modelu, bez přidání atributu do modelu a bez nutnosti přidávat žádné značky k zobrazení.
- Použití [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atribut v modelu, který má zadat šablonu, kterou chcete zobrazit vlastnosti modelu.
- Explicitním přidáním zobrazovaný název šablony, čímž [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) volání v zobrazení.

Přístup, který použijete, závisí na co je potřeba udělat v aplikaci. Není například neobvyklé kombinovat těchto přístupů, jaká formátování, které potřebujete získat.

V další části, bude přepnout trochu zařízením a přesouvat přizpůsobení zobrazení dat k přizpůsobení, jak se zadají. Budete připojení datepicker jQuery na úpravy zobrazení v aplikaci, abyste uhlazený způsob, jak zadat data.

> [!div class="step-by-step"]
> [Předchozí](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [další](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)
