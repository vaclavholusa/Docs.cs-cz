---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: Přidání zobrazení | Dokumentace Microsoftu
author: shanselman
description: Toto je kurz pro začátečníky, který vysvětluje základy ASP.NET MVC. Vytvořte jednoduchou webovou aplikaci, která čte a zapisuje z databáze.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: 942d329cf0451101a24da4c3facd38b2813653d1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365669"
---
<a name="adding-a-view"></a>Přidání zobrazení
====================
podle [Scott Hanselman](https://github.com/shanselman)

> Toto je kurz pro začátečníky, který vysvětluje základy ASP.NET MVC. Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze. Přejděte [výukové centrum pro ASP.NET MVC](../../../index.md) najít další technologie ASP.NET MVC, kurzů a ukázek.


V této části budeme se podívat, jak můžeme nechat Naše třída HelloWorldController čistě zapouzdření generování odpovědi HTML zpátky do klienta pomocí souboru šablony zobrazení.

Začněme tím, že pomocí metodě Index zobrazení šablony. Naše metoda je volána, Index a je v HelloWorldController. Metodě Index() aktuálně vrací hodnotu řetězce a zobrazí se zpráva, která je pevně zakódované v rámci třídy Kontroleru.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

Teď Změníme metodu Index místo toho vypadat takto:

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

Přidejme zobrazit šablonu teď na našem projektu, který používáme pro naši Index() metodu. Chcete-li to provést, klikněte pravým tlačítkem myši kamkoli uprostřed Index – metoda a klikněte na Přidat zobrazení...

![obrázek](getting-started-with-mvc-part3/_static/image1.png)

Tím se otevře dialogové okno "Přidat zobrazení", které nám poskytuje několik možností, jak chceme vytvořit šablony zobrazení, která mohou být využívána metodě indexu. Zatím nemáte něco změnit a stačí kliknout na tlačítko Přidat.

[![Přidat Dialog zobrazení](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)

Po kliknutí na Přidat novou složku a vytvoří nový soubor se zobrazí ve složce řešení, jak je vidět tady. Teď mám složku HelloWorld v zobrazení a soubor Index.aspx uvnitř této složky.

[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)

Nový soubor indexu je také už otevřené a připravený pro úpravy. Přidejte nějaký text v prvním &lt;h2&gt;Index&lt;/h2&gt; jako "Hello World."

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

Spusťte aplikaci a navštivte [ `http://localhost:xx/HelloWorld` ](http://localhostxx) znovu v prohlížeči. Metoda indexu v kontroleru v tomto příkladu nedělalo každé dílo, ale volat "návratový View()" ten označuje, že jsme chtěli použít soubor šablony zobrazení k vykreslení odpověď zpět klientovi. Protože jsme explicitně neurčil název souboru šablony zobrazení pro použití, ASP.NET MVC na výchozí pomocí Index.aspx zobrazení souboru ve složce \Views\HelloWorld. Teď vidíme řetězec, který jsme pevně zakódované v našich zobrazení.

[![Index – Windows Internet Explorer](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)

Vypadá hodně Dobrá. Všimněte si však, že název prohlížeče říká "Index" a velké objemy nadpis na stránce říká "Moje aplikace MVC." Změňme ty.

### <a name="changing-views-and-master-pages"></a>Změna zobrazení a stránky předlohy

Nejprve můžeme změnit text "Moje aplikace MVC." Tento text se sdílí a se zobrazí na každé stránce. Ve skutečnosti zobrazí se na jenom jednom místě v našem kódu, i když je na každé stránce v naší aplikaci. Přejděte do složky /Views/Shared v Průzkumníku řešení a otevřete soubor Site.Master. Tento soubor se nazývá hlavní stránky a je sdílený "prostředí", všechny naše ostatní stránky použít.

Všimněte si, že nějaký text, který říká ContentPlaceholder "MainContent" v tomto souboru.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

Tento zástupný text je, kde všechny stránky, které vytvoříte, zobrazí se, "zabalené" na stránce předlohy. Pokuste se změnit název, pak spusťte aplikaci a získáte více stránek. Můžete si všimnout, že zobrazuje jednu změnu na více stránkách.

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

Teď každé stránky bude mít primární záhlaví –, který je H1 - z "MVC Movie aplikaci." Bílý text v horní části existuje, který je sdílen na všechny stránky, která zpracovává.

Tady je Site.Master v celém rozsahu s naší změnili jsme název:

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

Teď Změníme název Index stránky.

Otevřete /HelloWorld/Index.aspx. Není k dispozici dvě místa, kde změnit. Nejprve: název, který se zobrazuje v nadpisu v prohlížeči, a pak sekundární záhlaví –, který je také H2. Vytvořím je každý trochu jinak, abyste viděli, které verze kódu se změní kterou část aplikace.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

Spusťte aplikaci a navštivte /Movies. Všimněte si, že došlo ke změně názvu prohlížeče, záhlaví primární a sekundární záhlaví. Je snadné vytvořit velké změny ve vaší aplikaci pomocí malé změny do zobrazení.

[![Seznam film – Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)

Naše něco "data" (v tomto případě "Hello World!" pevné byla zpráva) ale programového. Máme V (zobrazení) a My máme C (Kontrolery), ale žádné M (modelu) ještě. Za chvíli provedeme procesem vytvoření databáze a načíst datový model z něj.

## <a name="passing-a-viewmodel"></a>Předání ViewModel

Předtím, než jsme přejděte k databázi a mluvit o modely, ale nejprve se seznámíme "Modely ViewModel." Jedná se o objekty, které představují zobrazení šablony vyžaduje k vykreslení odpověď jazyka HTML zpátky do klienta. Se obvykle vytvářejí a předávány třídu Kontroleru zobrazení šablony a může obsahovat jenom data, která šablona zobrazení vyžaduje – a žádné další.

S naší ukázce Hello World dříve metodě akce Welcome() trvalo název a parametru numTimes a výstup do prohlížeče. Raději než mít řadič se nadále vykreslují tuto odpověď přímo, namísto toho vytvoříme malé třídy, která bude uchovávat data a poté jej nepředávejte šablonu zobrazení k vykreslení odpovědi HTML pomocí ji zpět. Tímto způsobem Kontroleru se týká jednu věc a zobrazit šablonu jiného – umožňuje nám udržovat čisté "oddělené oblasti zájmu" v rámci naší aplikace.

Vraťte se do souboru HelloWorldController.cs a přidejte novou třídu "WelcomeViewModel" a změnit metodu úvodní uvnitř kontrolér. Tady je úplný HelloWorldController.cs s novou třídu do stejného souboru.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

I když je na více řádcích, naše úvodní metoda je ve skutečnosti jenom dva příkazy kódu. První příkaz zabalí naše dva parametry do objekt ViewModel a druhý předá výsledný objekt do zobrazení.

Teď potřebujeme úvodní zobrazit šablonu! V metodě Vítejte klikněte pravým tlačítkem a vyberte Přidat zobrazení. Tentokrát vytvoříme zkontrolujte "Vytvořit zobrazení se silnými typy" a vyberte třídu naše WelcomeViewModel z rozevíracího seznamu. Toto nové zobrazení pouze vědět o WelcomeViewModels a žádné další typy objektů.

> *Poznámka: Budete muset zkompilováno jednou po přidání vašeho WelcomeViewModel pro zobrazení v rozevíracím seznamu.*


Zde je, jak by měla vypadat dialogové okno Přidat zobrazení. Klikněte na tlačítko Přidat. ![Přidat že zobrazení v kruhu](getting-started-with-mvc-part3/_static/image10.png)

Přidejte tento kód v rámci &lt;h2&gt; v nové Welcome.aspx. Vytvoříme Ujistěte se, smyčky a přivítejte tolikrát, kolikrát uživatel říká, že bychom!

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

Všimněte si také, píšete, protože řekli jsme to zobrazení o WelcomeViewModel (jsou vdaná, mějte na paměti?), že jsme získali užitečné Intellisense pokaždé, když budeme odkazovat na náš objekt modelu jako zobrazené na snímku obrazovky níže:

[![NumTime zdrojového kódu](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)

Spusťte aplikaci a navštivte `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` znovu. Nyní přesměrujeme data z adresy URL, je předána do Kontroleru automaticky, zabalí dat do ViewModel Kontroleru a předá objekt do našich zobrazení. Zobrazení, než se uživateli zobrazí data ve formátu HTML.

[![Vítejte – Windows Internet Explorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)

Dobře, to bylo druh "M" pro Model, ale není typ databáze. Pojďme se na to co jsme se naučili a vytvořit databázi videa.

> [!div class="step-by-step"]
> [Předchozí](getting-started-with-mvc-part2.md)
> [další](getting-started-with-mvc-part4.md)
