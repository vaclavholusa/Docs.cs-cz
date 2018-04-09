---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: Použití ViewData a implementace třídy ViewModel | Microsoft Docs
author: microsoft
description: Krok 6 ukazuje jak povolit podporu pro širší formulář Úpravy scénářů a taky popisuje dva přístupy, které slouží k předávání dat z řadičů pro zobrazení:...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: 9ba8758bd6524f3e300f3fd91ef68cfe8a3587a7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="use-viewdata-and-implement-viewmodel-classes"></a>Použití ViewData a implementujte ViewModel třídy
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Toto je krok 6 bezplatný [kurz aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , nevystavíte slabé stránky zabezpečení – prostřednictvím postup sestavení malá, ale dokončení, webové aplikace pomocí ASP.NET MVC 1.
> 
> Krok 6 ukazuje jak povolit podporu pro širší formulář Úpravy scénářů a taky popisuje dva přístupy, které slouží k předávání dat z řadičů pro zobrazení: ViewData a ViewModel.
> 
> Pokud používáte ASP.NET MVC 3, doporučujeme provedením [získávání spuštěna s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Hudba úložiště](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.


## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a>NerdDinner krok 6: ViewData a ViewModel

Jsme zahrnutých počet scénáře vystavení formuláře a popsané postupy pro implementaci vytvářet, aktualizovat a odstraňovat podporu (CRUD). Nyní jsme budete provádět naše implementace DinnersController další a povolit podporu pro širší formulář Úpravy scénářů. Při takovém probereme dva přístupy, které slouží k předávání dat z řadičů pro zobrazení: ViewData a ViewModel.

### <a name="passing-data-from-controllers-to-view-templates"></a>Předávání dat z řadiče zobrazení šablon

Jednou z definující vlastností vzoru MVC je striktní "oddělení obavy" pomáhá vynutit mezi různými součástmi aplikace. Modely, Kontrolery a zobrazení jednotlivých mít dobře definované rolích a zodpovědnostech a komunikaci mezi vzájemně dobře definované způsoby. To pomáhá zvýšit úroveň testovatelnosti a opakované použití kódu.

Pokud se rozhodne třídy Kontroleru k vykreslení odpovědi HTML zpět do klienta, je zodpovědná za explicitně předávání do zobrazení šablony všechna data potřebná pro vykreslení odpovědi. Zobrazení šablony nikdy proveďte veškeré data načtení nebo aplikaci logiky – a místo toho měli omezit sami mít pouze vykreslování kódu, které vycházejí z modelu nebo data předaný řadičem.

Nyní data modelu předávány podle našich DinnersController třída našich šablon zobrazení je jednoduchý a jednoduché – seznam objektů večeři v případě Index() a jeden večeři objektu v případě Details(), Edit(), Create() a Delete(). Jak jsme naše aplikace přidat další možnosti uživatelského rozhraní, často budeme potřebovat předat více než jen tato data k vykreslení odpovědi HTML v našich šablon zobrazení. Můžeme například chtít změňte pole "Země" v rámci naší upravit a vytvořit zobrazení nebudou jazyka HTML textové pole k rozevírací seznam. Místo pevný kódu rozevíracího seznamu názvů země v šabloně zobrazení jsme chtít vygenerovat ze seznamu podporovaných zemích, které jsme naplnit dynamicky. Je nutné zadat způsob, jak předat objekt večeři *a* seznam podporovaných zemích z našich řadiče našich šablon zobrazení.

Podívejme se na jsme se dá dosáhnout dvěma způsoby.

### <a name="using-the-viewdata-dictionary"></a>Pomocí slovníku ViewData

Základní třídy Kontroleru zpřístupňuje vlastnost slovníku "ViewData", která umožňuje předat další datové položky z řadiče zobrazení.

Například pro podporu tohoto scénáře, kde chceme změnit textového pole "Země" v rámci naší upravit zobrazení nebudou jazyka HTML textové pole k rozevírací seznam, budeme aktualizovat naše metoda akce Edit() předat (kromě objekt večeři) SelectList objekt, který lze použít jako m odelu rozevírací seznam zemích.

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

V konstruktoru SelectList výše přijímá seznam okresech k naplnění rozevíracího downlist s, jakož i aktuálně vybrané hodnoty.

Potom jsme můžete aktualizovat naše Edit.aspx zobrazit šablonu pro použití pomocnou metodu Html.DropDownList() místo Html.TextBox() pomocnou metodu, kterou jsme použili dříve:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

Pomocná metoda Html.DropDownList() výše přebírá dva parametry. První je název prvku formuláře HTML na výstup. Druhá je model "SelectList", které jsme předány prostřednictvím ViewData slovníku. Používáme jazyka C# – "klíčové slovo jako" přetypovat type v rámci slovníku jako SelectList.

A teď když jsme spustit naše aplikace a přístup */Dinners/Edit/1* adresy URL v rámci našeho prohlížeče uvidíme, že zobrazíte rozevírací seznam zemí, místo do textového pole. byl aktualizován naše upravit uživatelského rozhraní:

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

Protože jsme také vykreslení zobrazení šablony upravit metodu HTTP POST upravit (ve scénářích při výskytu chyb), jsme budete chtít zajistit, jsme také aktualizovat tuto metodu za účelem přidání SelectList do ViewData při zobrazení šablony v chybě scénáře:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

A teď podporuje náš scénář upravit DinnersController rozevírací seznam.

### <a name="using-a-viewmodel-pattern"></a>Pomocí vzoru ViewModel

Přístup slovník ViewData má výhodu probíhá poměrně rychle a snadno implementovat. Někteří vývojáři nelíbí pomocí řetězcového slovníky, ale vzhledem k tomu, že překlepům může způsobit chyby, které nebudou zachyceny v době kompilace. Beztypové slovníku ViewData také vyžaduje pomocí operátoru "jako" nebo přetypování při použití silného typu jazyk, jako je C#, v zobrazení šablony.

Alternativní způsob, který by mohl používáme je jedním často označuje jako "ViewModel" vzor. Při použití tohoto vzoru vytvoříme silně typované třídy, který jsou optimalizované pro naše zobrazení konkrétní scénáře, a který vystavit vlastnosti pro dynamické hodnoty nebo obsah vyžaduje našich šablon zobrazení. Naše třídy kontroleru můžete naplnit a předat naše zobrazit šablonu použít tyto třídy optimalizované zobrazení. To umožňuje bezpečnost typů, který kontroluje kompilaci a editor intellisense v rámci šablon zobrazení.

Chcete-li například povolit večeři formuláře úpravy scénáře vytvoříme "DinnerFormViewModel" třídy jako níže, který zveřejňuje dvě vlastnosti silného typu: objekt večeři a model SelectList potřebné k naplnění rozevírací seznam zemí:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

Můžeme aktualizujte naše Edit() akce metodu pro vytvoření DinnerFormViewModel pomocí večeři objekt, který načteme z našich úložiště a předejte ji do našich zobrazit šablonu:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

Jsme budete pak aktualizace našich zobrazení šablony, které se očekává "DinnerFormViewModel" místo "Večeři" objektu změnou atribut "dědí" v horní části stránky edit.aspx takto:

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

Jakmile jsme to provést, intellisense "Model" vlastnosti v rámci naší zobrazení šablony budou aktualizovány tak, aby odrážela objektový model, který jsme jsou předání typu DinnerFormViewModel:

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

Můžeme aktualizovat zobrazení kódu pracovat z ho. Všimněte si níže jak jsme nejsou změna názvů elementů vstupu vytváříme (elementy form bude stále se s názvem "Title", "Země") – ale aktualizujeme metody pomocné rutiny HTML k načtení hodnoty pomocí třídy DinnerFormViewModel:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

Také jsme budete aktualizovat naše metodu post upravit pro použití třídy DinnerFormViewModel při vykreslování chyby:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

Můžete také aktualizujeme naše Create() metody akce k opětovnému použití přesné stejné *DinnerFormViewModel* třída povolit zemí rozevírací seznam v těch, které také. Dole je implementace HTTP GET:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

Dole je implementace metodu Create HTTP POST:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

A teď naše upravit a vytvořit obrazovky podporují rozevíracích seznamů pro výběr země.

### <a name="custom-shaped-viewmodel-classes"></a>Ve tvaru vlastní třídy ViewModel

Ve výše uvedené scénáři Naše třída DinnerFormViewModel přímo zveřejňuje objekt modelu večeři jako vlastnost spolu s podpůrné SelectList vlastnost modelu. Tento postup funguje bez problémů pro scénáře, kde rozhraní HTML chceme vytvořit v rámci naší zobrazit šablonu odpovídá relativně úzce naše objekty modelu domény.

Pro scénáře, kde to ale není je případ, jednou z možností, kterou můžete použít pro vytvoření třídy ViewModel ve tvaru vlastní jejichž objektový model více optimalizovaná pro používání v zobrazení – a který může vypadat zcela liší od základní objekt modelu domény. Například může potenciálně vystavit, názvy jinou vlastnost nebo agregační vlastností shromážděných z více objektů modelu.

Může být ve tvaru vlastní třídy ViewModel používají k předávání dat z řadičů pro zobrazení k vykreslení, jakož i ke zpracování dat formuláře odeslat zpět na metodu akce kontroler. Pro tento scénář novější můžete mít metody akce aktualizovat objekt ViewModel s daty odeslání formuláře a potom pomocí ViewModel instance mapy nebo načíst objekt modelu skutečné domény.

Ve tvaru vlastní třídy ViewModel může poskytnout značnou flexibilitu a jsou něco prozkoumat kdykoli najít kód vykreslování v rámci zobrazit šablony nebo kód formuláře textu uvnitř vaší metody akce spuštění získat příliš složitý. Toto je často znaménkem zda modely domény není řádně odpovídají jsou generování uživatelského rozhraní, a že může pomoct zprostředkující třída ViewModel ve tvaru vlastní.

### <a name="next-step"></a>Další krok

Nyní podíváme, jak můžete použít částečné a stránky předlohy pro opakované využití a sdílet uživatelského rozhraní v rámci naší aplikace.

> [!div class="step-by-step"]
> [Předchozí](provide-crud-create-read-update-delete-data-form-entry-support.md)
> [další](re-use-ui-using-master-pages-and-partials.md)
