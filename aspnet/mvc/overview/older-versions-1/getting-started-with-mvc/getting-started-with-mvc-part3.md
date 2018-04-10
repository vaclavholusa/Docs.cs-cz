---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: Přidání zobrazení | Microsoft Docs
author: shanselman
description: Je toto Začátečník kurz, který představuje základní informace o architektuře ASP.NET MVC. Vytvoření jednoduché webové aplikace, která čte a zapisuje z databáze.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: 978d7980274c072ed559b54ed69ab86245b6c5a7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/10/2018
---
<a name="adding-a-view"></a>Přidání zobrazení
====================
podle [Scott Hanselman](https://github.com/shanselman)

> Je toto Začátečník kurz, který představuje základní informace o architektuře ASP.NET MVC. Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze. Přejděte [výukové centrum pro rozhraní ASP.NET MVC](../../../index.md) kurzy a ukázky najdete další ASP.NET MVC.


V této části přidáme podívat, jak může mít Naše třída HelloWorldController řádně zapouzdření generování odpovědi HTML zpět do klienta pomocí souboru šablony zobrazení.

Začněme pomocí našich metoda Index šablonu zobrazení. Naše metoda je volána Index a je v HelloWorldController. Aktuálně naše Index() metoda vrátí řetězec s zprávu, která je pevně kódovaný v rámci třídy Kontroleru.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

Nyní změníme metodu Index místo vypadat takto:

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

Nyní přidejte umožňuje zobrazit šablonu pro naše projekt, který používáme pro naše Index() metoda. K tomu, klikněte pravým tlačítkem na s myší někde uprostřed metodu Index a klikněte na tlačítko Přidat zobrazení...

![obrázek](getting-started-with-mvc-part3/_static/image1.png)

Tím zobrazíte dialog "Přidat zobrazení", který poskytuje nám některé možnosti, jak chcete vytvořit šablonu zobrazení, která mohou být využívána naše metoda Index. Zatím nemáte nic nezmění a stačí kliknout na tlačítko Přidat.

[![Přidat Dialog zobrazení](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)

Po kliknutí na tlačítko Přidat, novou složku a nový soubor se zobrazí ve složce řešení, jak je vidět tady. Nyní je nutné HelloWorld složku v zobrazení a soubor Index.aspx uvnitř této složky.

[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)

Nový soubor indexu je také již otevřen a připravené pro úpravy. Přidejte nějaký text v první části &lt;h2&gt;Index&lt;/h2&gt; jako "Hello, World".

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

Spusťte aplikaci a navštivte [ `http://localhost:xx/HelloWorld` ](http://localhostxx) znovu v prohlížeči. Metoda indexu v kontroleru v tomto příkladu nebyla žádné práci, ale volat "návratový View()", který uvedené, že jsme chtěli použít soubor šablony zobrazení k vykreslení odpověď zpět klientovi. Protože jsme explicitně nezadali název souboru šablony zobrazení používat, ASP.NET MVC uvedena do výchozího stavu pomocí Index.aspx zobrazení souboru ve složce \Views\HelloWorld. Teď vidíme řetězec, který jsme pevně v našem zobrazení.

[![Index – Windows Internet Explorer](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)

Vypadá poměrně funkční. Všimněte si však, že název prohlížeče říká "Index" a big nadpis na stránce říká "Moje aplikace MVC." Umožňuje změnit ty.

### <a name="changing-views-and-master-pages"></a>Změna zobrazení a stránky předlohy

Nejprve změňte text "Moje aplikace MVC." Tento text se sdílí a se zobrazí na každé stránce. Ve skutečnosti zobrazí se na jenom jednom místě v našem kódu, i když je na každé stránce v naší aplikaci. Přejděte do složky /Views/Shared v Průzkumníku řešení a otevřete soubor Site.Master. Tento soubor se nazývá stránky předlohy a je sdílený "prostředí", použít všechny naše jiné stránky.

Všimněte si nějaký text, který uvádí ContentPlaceholder "MainContent" v tomto souboru.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

Tento zástupný text je, kde všechny stránky, které vytvoříte se zobrazí, "zabalené" na hlavní stránce. Pokuste se změnit název, pak spusťte aplikaci a navštivte více stránek. Můžete si všimnout, že jeden změny se zobrazí na více stránkách.

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

Teď každé stránce bude mít primární záhlaví -, který je H1 - "Moje filmová aplikace MVC." Bílý text v horní části existuje, je sdílen na všechny stránky, která zpracovává.

Tady je Site.Master v celé jeho šíři s naše změněn název:

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

Nyní změňte název indexovou stránku.

Otevřete /HelloWorld/Index.aspx. Není k dispozici dvě místa, chcete-li změnit. Nejprve: název, které se zobrazí v prohlížeči, pak se sekundární záhlaví -, který je také H2 - názvu. I budete je každý mírně lišit, abyste viděli, které malou část kódu, kterou část aplikace změny provést.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

Spuštění aplikace a navštivte /Movies. Všimněte si, že došlo ke změně záhlaví prohlížeče, záhlaví primární a sekundární záhlaví. Je snadné provést big změny ve vaší aplikaci s malým změnám zobrazení.

[![Seznam film – Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)

Naše trocha "data" (v tomto případě "Hello World!" pevné byla zpráva), když programového. My V (zobrazení) a My jsme C (řadiče), ale žádné M (modelu) ještě. Projdeme jak krátce vytvořit databázi a z něj načíst data modelu.

## <a name="passing-a-viewmodel"></a>Předávání ViewModel

Před jsme přejděte k databázi a mluvit o modely, ale umožňuje nejprve mluvit o "ViewModels." Tyto jsou objekty, které představují šablonu zobrazení vyžaduje k vykreslení odpovědi HTML zpět do klienta. Budou se obvykle vytvářejí a předaná do zobrazení šablony třídy Kontroleru a smí obsahovat pouze data, která vyžaduje zobrazení šablony - a žádné další.

S naše ukázka HelloWorld dříve naše Welcome() metody akce trvala název a parametr numTimes a výstup do prohlížeče. Místo mít řadič pokračovat k vykreslení této odpovědi přímo, místo toho provedeme malý třída pro uložení dat a předejte ji do šablony zobrazení k vykreslení odpovědi HTML pomocí ji zpět. Tímto způsobem řadičem problémem s jednou z věcí a zobrazit šablonu jiné – to nám umožňuje udržovat čistou "oddělené oblasti zájmu" v naší aplikaci.

Zpět do souboru HelloWorldController.cs a přidejte novou třídu "WelcomeViewModel" a změnit metodu úvodní uvnitř řadiči. Zde je kompletní HelloWorldController.cs s novou třídu ve stejném souboru.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

I když je na více řádků, naše úvodní metoda je ve skutečnosti jenom dva příkazy kódu. První příkaz zabalí naše dva parametry do objekt ViewModel a druhá předává výsledný objekt do zobrazení.

Nyní potřebujeme šablonu úvodní zobrazení! Klikněte pravým tlačítkem v metodě úvodní a vyberte možnost Přidat zobrazení. Tento čas jsme budete zkontrolujte "Vytvořit zobrazení silného typu" a vyberte třídu naše WelcomeViewModel z rozevíracího seznamu. Toto nové zobrazení pouze vědět o WelcomeViewModels a žádné jiné typy objektů.

> *Poznámka: Budete muset sestavili po přidání vašeho WelcomeViewModel pro objeví v rozevíracím seznamu.*


Zde je, jak by měla vypadat vaší dialogové okno Přidat zobrazení. Kliknutím na tlačítko Přidat. ![Přidejte že v zobrazení kroužku](getting-started-with-mvc-part3/_static/image10.png)

Přidejte tento kód v části &lt;h2&gt; ve vaší nové Welcome.aspx. Jsme budete zkontrolujte smyčku a seznamte tolikrát, kolikrát uživatel informacemi o tom, že jsme měli!

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

Všimněte si také, při zadávání, protože to nám sdělili zobrazení o WelcomeViewModel (jsou vdaná, mějte na paměti,?), aby se nám získat užitečné Intellisense pokaždé, když jsme odkazovat na našem modelu objektu jako zobrazené na snímku obrazovky níže:

[![NumTime Source Code](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)

Spusťte aplikaci a navštivte `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` znovu. Teď přesměrováváme data z adresy URL, ho je předán do Kontroleru automaticky, Kontroleru zabalí dat do ViewModel a předá objekt do našich zobrazení. Zobrazení, než se zobrazí data jako kód HTML pro uživatele.

[![Vítá – Windows Internet Explorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)

Dobře, který byl druh "M" pro Model, ale není typ databáze. Podívejme se, co jsme jste se naučili a vytvořit databázi filmy.

> [!div class="step-by-step"]
> [Předchozí](getting-started-with-mvc-part2.md)
> [další](getting-started-with-mvc-part4.md)
