---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: '6. část: Členství technologie ASP.NET | Dokumentace Microsoftu'
author: JoeStagner
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 6 přidá členství technologie ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: 6d01a745ca1428f065f564f676d483ee807eb52e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368470"
---
<a name="part-6-aspnet-membership"></a>6. část: Členství technologie ASP.NET
====================
podle [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks ukazuje, jak mimořádně jednoduché je vytvářet výkonné a škálovatelné aplikace pro platformu .NET. Zobrazuje vypnout použití skvělých nových funkcí v technologii ASP.NET 4 k sestavení nebo online úložiště, včetně nákupu, Pokladna a správu.
> 
> V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 6 přidá členství technologie ASP.NET.


## <a id="_Toc260221672"></a>  Práce s členství technologie ASP.NET

![](tailspin-spyworks-part-6/_static/image1.png)

Klikněte na tlačítko zabezpečení

![](tailspin-spyworks-part-6/_static/image1.jpg)

Ujistěte se, že se používá ověřování pomocí formulářů.

![](tailspin-spyworks-part-6/_static/image2.jpg)

Použijte odkaz "Create User" k vytvoření několika uživatelů.

![](tailspin-spyworks-part-6/_static/image3.jpg)

Až budete hotovi, najdete v okně Průzkumník řešení a aktualizujte zobrazení.

![](tailspin-spyworks-part-6/_static/image2.png)

Všimněte si, ASPNETDB. Byl vytvořen MDF bez problémů. Tento soubor obsahuje tabulky, které podporují služby ASP.NET core jako je členství.

Teď můžeme začít implementace proces platby u pokladny.

Začněte tím, že vytvoření CheckOut.aspx stránky.

Na stránce CheckOut.aspx by měl být k dispozici pouze uživatelům, kteří přihlášeni, takže jsme se omezení přístupu k přihlášení uživatelů a přesměrovává uživatele, kteří nejsou přihlášeni na přihlašovací stránku.

Provedete to tak přidáme následující konfigurační oddíl souboru web.config.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

Šablona pro aplikace webových formulářů ASP.NET automaticky přidat oddíl ověřování do souboru web.config a navázat na výchozí přihlašovací stránku.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

Upravíme Login.aspx souboru kódu na pozadí k migraci anonymní nákupního košíku, když se uživatel přihlásí. Změnit na stránce\_událostí načtení následujícím způsobem.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

Pak přidejte obslužnou rutinu události "LoggedIn" následujícím způsobem nastavte název relace na nově přihlášeného uživatele a změnit id dočasné relace v nákupním košíku jménu uživatele voláním metody MigrateCart v našich MyShoppingCart třídy. (Implementováno v souboru .cs)

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

Implementujte metodu MigrateCart() tímto způsobem.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

V checkout.aspx jsme budete používat EntityDataSource a GridView v našich podívejte se na stránku podobně, jako jsme to udělali v našich nákupního košíku.

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

Všimněte si, že naše ovládacího prvku GridView určuje obslužnou rutinu události "ondatabound" s názvem MyList\_RowDataBound můžeme implementovat tuto obslužnou rutinu události následujícím způsobem.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

Tato metoda udržuje průběžný součet z nákupního košíku a každý řádek je vázán aktualizuje dolní řádek prvku GridView.

V této fázi jsme implementovali prezentaci "recenzi" aby bylo možné umístit.

Umožňuje zpracovat scénáři prázdný košíku přidáním několika řádků kódu na naši stránku\_zatížení události:

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

Když uživatel klikne na tlačítko "Odeslat" jsme spustí následující kód v obslužné rutině události klikněte na tlačítko Odeslat.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

"Maso" proces odeslání objednávky je implementovaná v metodě SubmitOrder() naše MyShoppingCart třídy.

SubmitOrder bude:

- Provést všechny položky v nákupním košíku a jejich používání při vytváření nového záznamu pořadí a propojené OrderDetails záznamy.
- Výpočet expediční datum.
- Vymažte nákupního košíku.


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

Pro účely této ukázkové aplikaci výpočtu datum expedice jednoduše přidejte dva dny na aktuální datum.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

Spuštění aplikace teď umožní nám to otestovat nákupní proces od začátku do konce.

> [!div class="step-by-step"]
> [Předchozí](tailspin-spyworks-part-5.md)
> [další](tailspin-spyworks-part-7.md)
