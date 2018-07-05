---
uid: mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-cs
title: Prevence útoků založených na Injektáži JavaScriptu (C#) | Dokumentace Microsoftu
author: StephenWalther
description: Zabránit útoky prostřednictvím injektáže jazyka JavaScript a skriptování napříč weby útoky na vás. V tomto kurzu Stephen Walther vysvětluje, jak můžete snadno de...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: d0136da6-81a4-4815-b002-baa84744c09e
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-cs
msc.type: authoredcontent
ms.openlocfilehash: 2d307e1034dca4893cd45baf7d54edb7544829e3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381887"
---
<a name="preventing-javascript-injection-attacks-c"></a>Prevence útoků založených na Injektáži JavaScriptu (C#)
====================
podle [Stephen Walther](https://github.com/StephenWalther)

[Stáhnout PDF](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_06_CS.pdf)

> Zabránit útoky prostřednictvím injektáže jazyka JavaScript a skriptování napříč weby útoky na vás. V tomto kurzu Stephen Walther vysvětluje, jak můžete snadno, aby zhatila tyto typy útoků pomocí kódování obsahu v jazyce HTML.


Cílem tohoto kurzu je vysvětlují, jak můžete zabránit útoků založených na injektáži JavaScriptu v aplikacích ASP.NET MVC. Tento kurz popisuje dvou přístupů týkající se ochrana vašeho webu vůči útoku prostřednictvím injektáže skriptu JavaScript. Zjistíte, jak zabránit útoků založených na injektáži JavaScriptu pomocí kódování data, která můžete zobrazit. Také se dozvíte, jak zabránit útoků založených na injektáži JavaScriptu kódování, který můžete přijímat data.

## <a name="what-is-a-javascript-injection-attack"></a>Co je útok prostřednictvím injektáže JavaScript?

Pokaždé, když se přijímají vstup uživatele a opětovnému zobrazení uživatelského vstupu, otevřete webovou stránku pro útoků založených na injektáži JavaScriptu. Podívejme se na konkrétní aplikaci, která je otevřená pro útoků založených na injektáži JavaScriptu.

Představte si, že vytvoříte web zpětné vazby zákazníka (viz obrázek 1). Zákazníky můžete přejděte na webovou stránku a zadejte zpětnou vazbu o svých zkušenostech s použitím vašich produktů. Když zákazník odešle jejich zpětné vazby, se zobrazí na stránce zpětnou vazbu znovu zpětnou vazbu.


[![Web zpětné vazby zákazníka](preventing-javascript-injection-attacks-cs/_static/image2.png)](preventing-javascript-injection-attacks-cs/_static/image1.png)

**Obrázek 01**: web zpětné vazby zákazníka ([kliknutím ji zobrazíte obrázek v plné velikosti](preventing-javascript-injection-attacks-cs/_static/image3.png))


Web zpětné vazby zákazníka používá `controller` v informacích 1. To `controller` obsahuje dvě akce s názvem `Index()` a `Create()`.

**Výpis 1 – `HomeController.cs`**

[!code-csharp[Main](preventing-javascript-injection-attacks-cs/samples/sample1.cs)]

`Index()` Metoda zobrazí `Index` zobrazení. Tato metoda projde všechny předchozí od zákazníků `Index` zobrazení při získání zpětné vazby z databáze (pomocí LINQ to SQL dotazu).

`Create()` Metoda vytvoří novou položku zpětné vazby a přidá do databáze. Zpráva, kterou zákazník zadá ve formuláři je předán `Create()` metoda v parametru zprávy. Vytvoření položky zpětné vazby a položku zpětné vazby je přiřazena zpráva `Message` vlastnost. Položku zpětné vazby je odeslán do databáze s `DataContext.SubmitChanges()` volání metody. Nakonec návštěvníka přesměrován zpět `Index` zobrazení, kde se zobrazí všechny zpětnou vazbu.

`Index` Je zobrazení obsažené v informacích 2.

**Výpis 2 – `Index.aspx`**

[!code-aspx[Main](preventing-javascript-injection-attacks-cs/samples/sample2.aspx)]

`Index` Zobrazení má dvě části. V horní části obsahuje formulář zpětné vazby skutečných zákazníků. V dolní části obsahuje For … Každou smyčku, která prochází všechny předchozí položky zpětné vazby zákazníků a zobrazí vlastnosti EntryDate a zpráv pro každou položku zpětné vazby.

Web zpětné vazby zákazníka je jednoduchý Web. Bohužel je otevřená pro útoků založených na injektáži JavaScriptu na webu.

Představte si, zadejte následující text do formuláře zpětné vazby zákazníka:

[!code-html[Main](preventing-javascript-injection-attacks-cs/samples/sample3.html)]

Tento text představuje skript jazyka JavaScript, která zobrazuje do pole zpráva s výstrahou. Jakmile někdo odešle tento skript do zpětné vazby formuláře, zpráva <em>poč!</em> se zobrazí pokaždé, když se každý uživatel navštíví web zpětné vazby zákazníka v budoucnu (viz obrázek 2).


[![Vkládání jazyka JavaScript](preventing-javascript-injection-attacks-cs/_static/image5.png)](preventing-javascript-injection-attacks-cs/_static/image4.png)

**Obrázek 02**: injektáž jazyka JavaScript ([kliknutím ji zobrazíte obrázek v plné velikosti](preventing-javascript-injection-attacks-cs/_static/image6.png))


Vaše první odezvy do útoků založených na injektáži JavaScriptu teď může být apathy. Si možná myslíte, že jsou útoků založených na injektáži JavaScriptu jednoduše typem *pouze poškození vzhledu* útoku. Může domnívat, že nikdo dělat vše, co skutečně evil provedením útoku prostřednictvím injektáže skriptu JavaScript.

Bohužel se hacker můžete provést některé skutečně, ve skutečnosti evil věci vložením jazyka JavaScript do webu. Útok prostřednictvím injektáže JavaScript můžete použít k provedení útoku skriptování mezi weby (XSS). V s útoky skriptování napříč weby ukrást důvěrných informací uživatelů a odešle informace na jiný web.

Například můžete použít se hacker útok prostřednictvím injektáže JavaScript ke krádeži hodnoty soubory cookie v prohlížeči od jiných uživatelů. Pokud citlivým informacím--třeba hesla, čísla platebních karet nebo čísla sociálního pojištění – je uložený v prohlížeči soubory cookie, pak se hacker slouží ke krádeži tyto informace útok prostřednictvím injektáže skriptu JavaScript. Nebo, pokud uživatel zadá citlivých informací obsažených na stránce, který má pomocí jazyka JavaScript útoku, ohrožený hacker do vzít data formuláře a jeho odeslání na jiný web pomocí vloženého JavaScript pole formuláře.

*Buďte prosím děsili toho*. Vážně trvat útoků založených na injektáži JavaScriptu a chránit důvěrné informace uživatele. V následujících dvou částech se podíváme na dvě techniky, které vám pomůže chránit vaše aplikace ASP.NET MVC z útoků založených na injektáži JavaScriptu.

## <a name="approach-1-html-encode-in-the-view"></a>Způsob #1: S kódováním HTML v zobrazení

Jednou z snadný způsob prevence útoků založených na injektáži JavaScriptu je HTML kódování jakékoli údaje zadávané uživateli webu při opětovné zobrazení dat v zobrazení. Aktualizovaný `Index` zobrazení v informacích 3 následuje tento přístup.

**Výpis 3 – `Index.aspx` (kódovaný jazykem HTML)**

[!code-aspx[Main](preventing-javascript-injection-attacks-cs/samples/sample4.aspx)]

Všimněte si, že hodnota `feedback.Message` je HTML kódováním než hodnota se zobrazí s následujícím kódem:

[!code-aspx[Main](preventing-javascript-injection-attacks-cs/samples/sample5.aspx)]

Jaké jsou mean do formátu HTML zakódujte řetězec? Při HTML kódování řetězce, nebezpečné znaky, jako `<` a `>` nahrazují HTML odkazy na entity, jako `&lt;` a `&gt;`. Takže když řetězec `<script>alert("Boo!")</script>` je ve formátu HTML s kódováním získá k převést `&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`. Kódovaný řetězec se již provádí jako skriptu JavaScript při prohlížečem interpretovány. Místo toho můžete získat neškodné stránku na obrázku 3.


[![Nepotlačí útoku jazyka JavaScript](preventing-javascript-injection-attacks-cs/_static/image8.png)](preventing-javascript-injection-attacks-cs/_static/image7.png)

**Obrázek 03**: nepotlačí útoku jazyka JavaScript ([kliknutím ji zobrazíte obrázek v plné velikosti](preventing-javascript-injection-attacks-cs/_static/image9.png))


Všimněte si, že v `Index` zobrazit výpis 3 pouze hodnotu `feedback.Message` je zakódován. Hodnota `feedback.EntryDate` není kódován. Potřebujete jenom zakódovat data zadaná uživatelem. Protože hodnota EntryDate byl vygenerován v kontroleru, není nutné do formátu HTML kódujete tuto hodnotu.

## <a name="approach-2-html-encode-in-the-controller"></a>Způsob #2: S kódováním HTML v Kontroleru

Namísto HTML kódování dat při zobrazení dat v zobrazení, ve formátu HTML můžete kódovat data pouze před odesláním dat do databáze. Tento druhý postup je provedena v případě třídy `controller` v informacích 4.

**Část 4 – `HomeController.cs` (kódovaný jazykem HTML)**

[!code-csharp[Main](preventing-javascript-injection-attacks-cs/samples/sample6.cs)]

Všimněte si, že hodnota zprávy je kódovaný jako předtím, než hodnota se odesílá do databáze v rámci HTML `Create()` akce. Pokud zpráva se zobrazí znovu v zobrazení, je zpráva kódovaný jazykem HTML a jakékoli JavaScript vložený ve zprávě není spuštěn.

Obvykle by měl upřednostnit prvního přístupu přes tento druhý postup popsané v tomto kurzu. Problém s tímto přístupem druhý je, že skončíte s kódováním HTML daty v databázi. Jinými slovy vaše data databáze je změněných zábavných vypadající znaky.

Proč je to chybný? Pokud byste někdy potřebovali pro zobrazení dat databáze něco jiného než na webové stránce, bude mít problémy. Například můžete zobrazit už snadno data v aplikaci Windows Forms.

## <a name="summary"></a>Souhrn

Účelem tohoto kurzu bylo vystrašení vás o potenciálních zákazníků útok prostřednictvím injektáže jazyka JavaScript. Popsané v tomto kurzu dvě metody pro tyto aplikace ASP.NET MVC proti útoků založených na injektáži JavaScriptu: buď ve formátu HTML můžete kódovat uživatel odeslal data v zobrazení nebo je můžete HTML kódování uživatel odeslal data v kontroleru.

> [!div class="step-by-step"]
> [Předchozí](authenticating-users-with-windows-authentication-cs.md)
> [další](authenticating-users-with-forms-authentication-vb.md)
