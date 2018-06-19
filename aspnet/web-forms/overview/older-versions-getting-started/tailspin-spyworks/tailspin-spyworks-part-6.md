---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Část 6: Členství technologie ASP.NET | Microsoft Docs'
author: JoeStagner
description: Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 6 přidá členství technologie ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: 83e9bc780ea8face3e0f55fdf8c00e13b60f80a7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30886807"
---
<a name="part-6-aspnet-membership"></a>Část 6: Členství technologie ASP.NET
====================
podle [Stagner Jan](https://github.com/JoeStagner)

> Tailspin Spyworks ukazuje, jak neobvykle jednoduché je vytvoření efektivní a škálovatelné aplikace pro platformu .NET. Zobrazuje vypnout používání skvělé nové funkce v rozhraní ASP.NET 4 k sestavení online úložiště, včetně nákupy, najdete v článku věnovaném a správu.
> 
> Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 6 přidá členství technologie ASP.NET.


## <a id="_Toc260221672"></a>  Práce s členství technologie ASP.NET

![](tailspin-spyworks-part-6/_static/image1.png)

Klikněte na možnost zabezpečení

![](tailspin-spyworks-part-6/_static/image1.jpg)

Ujistěte se, že používáme ověřování pomocí formulářů.

![](tailspin-spyworks-part-6/_static/image2.jpg)

Chcete-li vytvořit několik uživatelů pomocí odkazu "Vytvořit uživatele".

![](tailspin-spyworks-part-6/_static/image3.jpg)

Až budete hotoví, naleznete v okně Průzkumníka řešení a obnovte zobrazení.

![](tailspin-spyworks-part-6/_static/image2.png)

Všimněte si, že ASPNETDB. MDF jemné byla vytvořena. Tento soubor obsahuje tabulky, které podporují základní služby technologie ASP.NET, jako je členství.

Nyní jsme zahájí proces najdete v článku věnovaném implementace.

Začněte vytvořením CheckOut.aspx stránky.

Stránka CheckOut.aspx by měl být k dispozici pouze uživatelům, kteří se přihlásili, jsme se omezení přístupu k přihlášení uživatelů a přesměrovává uživatele, kteří nejsou přihlášeni na přihlašovací stránku.

K tomu přidáme následující konfigurační oddíl naše souboru web.config.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

Šablona pro aplikace webových formulářů ASP.NET automaticky přidán k ověřování části našich souboru web.config a vytvořit výchozí přihlašovací stránku.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

Jsme musíte upravit Login.aspx souboru migrace anonymní nákupní košík, když se uživatel přihlásí kódu. Změnit stránce\_událostí načtení následujícím způsobem.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

Pak přidejte obslužnou rutinu události "LoggedIn" takto nastavte název relace na nově přihlášeného uživatele a změnit id dočasné relace v nákupní košík na tohoto uživatele pomocí volání metody MigrateCart Naše třída MyShoppingCart. (Implementována v souboru .cs)

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

Implementujte metodu MigrateCart() podobné výjimky.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

V checkout.aspx použijeme EntityDataSource a GridView v našem podívejte se na stránku tolik, jako jsme to udělali v naší stránce s nákupní košík.

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

Všimněte si, že naše ovládací prvek GridView určuje obslužné rutiny události "ondatabound" s názvem MyList\_RowDataBound můžeme implementovat této obslužné rutiny události takto.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

Tato metoda udržuje spuštění úplného nákupního košíku jako každý řádek je vázán a aktualizuje dolní řádek GridView.

V této fázi jste implementovali jsme prezentace "kontrola" aby bylo možné umístit.

Umožňuje zpracování scénářem prázdný košíku přidáním několika řádků kódu do naší stránce s\_zatížení událostí:

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

Když uživatel klikne na tlačítko "Odeslat" jsme se spusťte následující kód v obslužné rutině události klikněte na tlačítko Odeslat.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

"Maso" procesu odeslání objednávky se provádí v metodě SubmitOrder() naše MyShoppingCart třídy.

SubmitOrder bude:

- Proveďte všechny položky řádku nákupní košík a použít je k vytvoření nového záznamu pořadí a přidružené záznamy Rozpis objednávek.
- Vypočítejte přesouvání datum.
- Clear – nákupní košík.


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

Pro účely této ukázkové aplikaci jsme vám vypočítat datum expedice stačí přidat dva dny na aktuální datum.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

Spuštění aplikace teď bude umožňují nám otestování procesu nákupní od začátku do konce.

> [!div class="step-by-step"]
> [Předchozí](tailspin-spyworks-part-5.md)
> [další](tailspin-spyworks-part-7.md)
