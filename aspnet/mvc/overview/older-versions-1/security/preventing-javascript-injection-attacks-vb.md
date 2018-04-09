---
uid: mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
title: Prevence útoků vkládání JavaScript (VB) | Microsoft Docs
author: StephenWalther
description: Zabránit útoky prostřednictvím injektáže JavaScript a webů skriptování útoky na vás. V tomto kurzu Stephen Walther vysvětluje, jak můžete snadno de...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 9274a72e-34dd-4dae-8452-ed733ae71377
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
msc.type: authoredcontent
ms.openlocfilehash: cb19236b22abd455472621ce74a8cddf9752d6c5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="preventing-javascript-injection-attacks-vb"></a>Prevence útoků vkládání JavaScript (VB)
====================
podle [Stephen Walther](https://github.com/StephenWalther)

[Stáhnout PDF](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_06_VB.pdf)

> Zabránit útoky prostřednictvím injektáže JavaScript a webů skriptování útoky na vás. V tomto kurzu Stephen Walther vysvětluje, jak můžete snadno připraven tyto typy útoků pomocí kódování obsahu v jazyce HTML.


Cílem tohoto kurzu je vysvětlují, jak lze zabránit útokům vkládání JavaScript v aplikacích ASP.NET MVC. Tento kurz popisuje dva přístupy k obraně proti útoku vkládání JavaScript vašeho webu. Zjistíte, jak zabránit útokům vkládání jazyka JavaScript pomocí kódování data, která můžete zobrazit. Také zjistíte, jak zabránit útokům vkládání jazyka JavaScript pomocí kódování data, která je přijmout.

## <a name="what-is-a-javascript-injection-attack"></a>Co je útok vkládání JavaScript?

Vždy, když přijímají vstup uživatele a uživatelský vstup je třeba znovu zobrazit, otevřete svůj web JavaScript vkládání útoky. Podívejme se na konkrétní aplikaci, která je otevřená a prostřednictvím injektáže JavaScript.

Představte si, že jste vytvořili web zpětné vazby zákazníků (viz obrázek 1). Zákazníci můžete navštívit webovou stránku a zadat svůj názor na své zkušenosti s používáním vašeho produkty. Když zákazník odešle jejich zpětnou vazbu, se zobrazí na stránce zpětnou vazbu znovu zpětnou vazbu.


[![Zákaznické zpětné vazby webu](preventing-javascript-injection-attacks-vb/_static/image2.png)](preventing-javascript-injection-attacks-vb/_static/image1.png)

**Obrázek 01**: web zpětné vazby zákazníků ([Kliknutím zobrazit obrázek v plné velikosti](preventing-javascript-injection-attacks-vb/_static/image3.png))


Web zpětné vazby zákazníka používá `controller` v výpis 1. To `controller` obsahuje dvě akce s názvem `Index()` a `Create()`.

**Výpis 1 – `HomeController.vb`**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample1.vb)]

`Index()` Metoda zobrazí `Index` zobrazení. Tato metoda úspěšně projde všemi předchozí od zákazníků `Index` zobrazení načtením zpětné vazby z databáze (pomocí dotazu LINQ to SQL).

`Create()` Metoda vytvoří novou položku zpětné vazby a přidává ji k databázi. Zpráva, kterou zákazník zadá ve formuláři je předána `Create()` metoda v parametru zprávy. Vytvoření položky zpětnou vazbu a zprávy je přiřazená položce zpětné vazby `Message` vlastnost. Položka zpětnou vazbu se odešle do databáze s `DataContext.SubmitChanges()` volání metody. Nakonec návštěvníka je přesměrován zpět `Index` zobrazení, kde se zobrazí všechny zpětnou vazbu.

`Index` Zobrazení je součástí výpis 2.

**Výpis 2 – `Index.aspx`**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample2.aspx)]

`Index` Zobrazení má dvě části. V horní části obsahuje formulář zpětné vazby skutečnou zákazníků. V dolní části obsahuje For... Každý smyčky, který prochází všechny předchozí položky zpětné vazby zákazníků a zobrazí vlastnosti EntryDate a zprávy pro každou položku zpětné vazby.

Web zpětné vazby zákazníka je jednoduchý Web. Bohužel se otevřete JavaScript vkládání útoky založenými na webu.

Představte si, zadejte následující text do formulář zpětné vazby zákazníka:

[!code-html[Main](preventing-javascript-injection-attacks-vb/samples/sample3.html)]

Tento text představuje skript jazyka JavaScript, který zobrazí pole zpráva s výstrahou. Po někdo předá tento skript do zpětné vazby formuláři zprávu <em>poč!</em> Zobrazí se vždy, když každý uživatel navštíví web zpětné vazby zákazníka v budoucnu (viz obrázek 2).


[![Vkládání JavaScript](preventing-javascript-injection-attacks-vb/_static/image5.png)](preventing-javascript-injection-attacks-vb/_static/image4.png)

**Obrázek 02**: vkládání JavaScript ([Kliknutím zobrazit obrázek v plné velikosti](preventing-javascript-injection-attacks-vb/_static/image6.png))


Vaše počáteční odpověď prostřednictvím injektáže JavaScript teď, může být apathy. Může si myslíte, že prostřednictvím injektáže JavaScript jsou jednoduše typ *poškození vzhledu* útoku. Možná se domníváte, že žádná mohou provádět všechny skutečně evil zapsáním útoku vkládání JavaScript.

Bohužel se hacker můžete provést některé skutečně, skutečně evil věcí vložením JavaScript do webu. Vkládání útoku JavaScript můžete použít k provedení útoku webů Skriptování. V rámci útoku skriptování mezi ukrást důvěrných informací uživatelů a poslat informace o jiný web.

Například můžete použít se hacker útoku vkládání JavaScript ke krádeži hodnoty souborů cookie prohlížeče od jiných uživatelů. Pokud citlivých informací – třeba hesla, čísla platebních karet nebo čísla sociálního pojištění – je uložený v prohlížeči soubory cookie, pak se hacker slouží ke krádeži tyto informace útoku vkládání JavaScript. Nebo, pokud uživatel zadá citlivé informace obsažené v stránky, který byl napaden s útoku JavaScript hacker do získat data formuláře a jeho odeslání na jiný web pomocí jazyka JavaScript vloženého pole formuláře.

*Je potřeba Vystrašené*. Vážně trvat prostřednictvím injektáže JavaScript a chránit důvěrné informace uživatele. V následujících dvou částech probereme dvě techniky, které můžete použít bránit aplikace ASP.NET MVC před útoky vkládání JavaScript.

## <a name="approach-1-html-encode-in-the-view"></a>Způsob #1: Kódování HTML v zobrazení

Jednou snadno metodu, jak zabránit útokům vkládání JavaScript je HTML kódování žádná data, zadá uživatelé webu, když jste data v zobrazení znovu. Aktualizovaný `Index` zobrazení v výpis 3 následuje tento přístup.

**Výpis 3 – `Index.aspx` (kódovaný jazykem HTML)**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample4.aspx)]

Všimněte si, že hodnota `feedback.Message` jsou ve formátu HTML kódovaný předtím, než je hodnota zobrazena následujícím kódem:

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample5.aspx)]

Jaké jsou střední do formátu HTML zakódujte řetězec? Při HTML kódování řetězec, nebezpečné znaky, jako `<` a `>` nahrazuje odkazy entity HTML jako `&lt;` a `&gt;`. Takže pokud řetězec `<script>alert("Boo!")</script>` jsou ve formátu HTML s kódováním jej získá převést na `&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`. Řetězec s kódováním už provede jako skript jazyka JavaScript v případě interpretovat prohlížeče. Místo toho získat stránku neškodné na obrázku 3.


[![Útok nepotlačí JavaScript](preventing-javascript-injection-attacks-vb/_static/image8.png)](preventing-javascript-injection-attacks-vb/_static/image7.png)

**Obrázek 03**: nepotlačí útoku JavaScript ([Kliknutím zobrazit obrázek v plné velikosti](preventing-javascript-injection-attacks-vb/_static/image9.png))


Všimněte si, že v `Index` zobrazit výpis 3 pouze hodnota `feedback.Message` je zakódován. Hodnota `feedback.EntryDate` není kódován. Potřebujete jenom zakódovat data zadaná uživatelem. Protože hodnota EntryDate byl vygenerován v kontroleru, není nutné do formátu HTML kódování tuto hodnotu.

## <a name="approach-2-html-encode-in-the-controller"></a>Způsob #2: Kódování HTML v Kontroleru

Místo kódování data při zobrazení dat v zobrazení HTML, můžete HTML zakódovat data těsně před odesláním dat do databáze. Tento druhý postup je provedena u `controller` výpis 4.

**Výpis 4 – `HomeController.cs` (kódovaný jazykem HTML)**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample6.vb)]

Všimněte si, že hodnota zprávy je před odesláním hodnoty do databáze v rámci kódovaný jazykem HTML `Create()` akce. Když se zpráva zobrazí znovu, v zobrazení, zpráva je kódovaný jazykem HTML a všechny JavaScript vložit ve zprávě není proveden.

By měl obvykle upřednostnit první postup popsaný v tomto kurzu přes tento druhý přístup. U tento druhý postup je ale problém, v níž se kódovaný jazykem HTML dat v databázi. Jinými slovy data databáze je dirtied s zábavného vypadající znaky.

Proč je to chybný? Pokud byste někdy potřebovali pro zobrazení dat databáze v něco jiného než na webové stránce, budou mít problémy. Například můžete zobrazit už snadno dat v aplikaci Windows Forms.

## <a name="summary"></a>Souhrn

Účelem tohoto kurzu bylo vystrašení můžete o potenciálních zákazníků útoku vkládání JavaScript. Dva přístupy k obraně aplikace ASP.NET MVC před útoky vkládání JavaScript popsané v tomto kurzu: můžete buď HTML kódování uživatel odeslal data v zobrazení, nebo můžete HTML kódování uživatel odeslal data v kontroleru.

> [!div class="step-by-step"]
> [Předchozí](authenticating-users-with-windows-authentication-vb.md)
