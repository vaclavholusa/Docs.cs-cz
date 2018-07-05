---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: Použití slovníku ViewData a implementace tříd ViewModel | Dokumentace Microsoftu
author: microsoft
description: Krok 6 ukazuje jak povolit podporu pro bohatší formulář Úpravy scénářů, které se probírá také dva přístupy, které slouží k předání dat z řadiče zobrazení:...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: edc62246cdc5e5df51c369a70b47dab92c9ecc1c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365120"
---
<a name="use-viewdata-and-implement-viewmodel-classes"></a>Použití slovníku ViewData a implementace tříd ViewModel
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Toto je kroku 6 v rámci bezplatného [kurz vývoje aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , který procházení procházení po tom, jak sestavit malý, ale bylo možné provést, webové aplikace pomocí ASP.NET MVC 1.
> 
> Krok 6 ukazuje jak povolit podporu pro bohatší formulář Úpravy scénářů, které se probírá také dva přístupy, které slouží k předání dat z řadiče zobrazení: ViewData a ViewModel.
> 
> Pokud používáte ASP.NET MVC 3, doporučujeme je provést [získávání začít s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.


## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a>NerdDinner krok 6: ViewData a ViewModel

Jsme zahrnutých počet scénáře vystavení formuláře a popsáno, jak implementovat vytvoření, aktualizace a odstranění (CRUD) podpory. Teď vytvoříme naši implementaci DinnersController jít dál a povolit podporu pro bohatší formulář Úpravy scénářů. Při to probereme dva přístupy, které slouží k předání dat z řadiče zobrazení: ViewData a ViewModel.

### <a name="passing-data-from-controllers-to-view-templates"></a>Předání dat z řadičů pro zobrazení šablony

Jednou z definující charakteristiky vzor MVC je striktní "oddělené oblasti zájmu" pomáhá vynucovat mezi různými součástmi aplikace. Modely, Kontrolerů a zobrazení jednotlivých jasně definovaných odpovědnosti a rolích a komunikaci mezi vzájemně způsoby dobře definované. To pomáhá podporovat testovatelnost a opakované použití kódu.

Pokud se rozhodne třídu Kontroleru k vykreslení odpověď jazyka HTML zpátky do klienta, je zodpovědná za explicitně předávání zobrazit šablonu, všechna data potřebné k vykreslení odpovědi. Zobrazit šablony by nikdy neprovádějte žádné data načítání nebo aplikaci logiky a místo toho by měla omezit samy o sobě stačí, když kód pro vykreslování, která vychází z modelu/dat do něho předaný kontrolerem.

Právě teď modelu data předávána podle našich DinnersController třídy pro naše zobrazení šablony je jednoduché a přímočaré – seznam objektů večeře v případě Index() a jeden Dinner objekt v případě Details(), Edit(), Create() a Delete(). Protože budeme přidávat další funkce uživatelského rozhraní pro aplikace, často budeme potřebovat předat více než jen tato data k vykreslení HTML odpovědi v naší zobrazit šablony. Například může chceme změnit pole "Země" v rámci naší upravit a vytvořit zobrazení jazyka HTML textové pole pro dropdownlist nebudou. Namísto pevně zakódovat Rozevírací seznam názvů země v šabloně zobrazení jsme chtít vygenerovat ze seznamu podporovaných zemích, které jsme naplnit dynamicky. Potřebujeme způsob, jak předat objekt Dinner *a* seznam podporovaných zemích z kontroleru pro naše zobrazení šablony.

Pojďme se podívat na dva způsoby, jak jsme toho dosáhli.

### <a name="using-the-viewdata-dictionary"></a>Použití slovníku ViewData

Základní třída Kontroleru zpřístupňuje vlastnost "ViewData" slovník, který slouží k předání dalších datových položek z řadičů pro zobrazení.

Například scénář, ve kterém chceme změnit textového pole "Země" v zobrazení pro úpravy nebudou jazyka HTML textové pole pro dropdownlist účelem jsme aktualizovat metodě akce Edit() předat SelectList objekt, který může sloužit jako m (navíc k večeři object) odelu dropdownlist zemích.

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

Konstruktor třídy SelectList výše přijímá seznam counties k naplnění rozevíracího downlist s, jakož i aktuálně vybrané hodnoty.

Potom jsme můžete aktualizovat šablonu zobrazení Edit.aspx Pomocná metoda Html.DropDownList() nahrazujícím Html.TextBox() Pomocná metoda, kterou jsme použili dříve:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

Výše uvedené metody helper Html.DropDownList() přebírá dva parametry. První je název elementu formuláře HTML na výstup. Druhým je "SelectList" model, který jsme předány prostřednictvím slovníku ViewData. Používáme jazyka C# – "klíčové slovo jako" k přetypování typu v rámci slovníku jako SelectList.

A teď jsme spouštění našich aplikací a přístup */Dinners/Edit/1* adresu URL do prohlížeče uvidíme, že naše úpravy uživatelského rozhraní se aktualizovala na zobrazení dropdownlist zemí místo textové pole:

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

Protože jsme také vykreslit úpravy zobrazení šablony z metodu HTTP POST upravit (ve scénářích, když dojde k chybě), jsme vhodné Ujistěte se, že jsme také aktualizovat tuto metodu za účelem přidání SelectList do slovníku ViewData při vykreslení zobrazení šablony v chybových scénářů:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

A teď podporuje úpravy scénáři DinnersController DropDownList.

### <a name="using-a-viewmodel-pattern"></a>Použití vzoru ViewModel

Přístup slovníku ViewData výhodou probíhá poměrně rychle a snadno se implementuje. Někteří vývojáři nelíbí se vám pomocí založené na řetězci slovníky, ale protože překlepy může vést k chybám, které nebude zachycena v době kompilace. Netypové slovníku ViewData vyžaduje také pomocí operátoru "jako" nebo přetypování při použití silně typovaný jazyk, jako je C#, v šabloně zobrazení.

Alternativním přístupem, které bychom mohli použít patří označovaný také jako vzor "ViewModel". Při použití tohoto modelu se nám vytvořit třídy silného typu, které jsou optimalizované pro naše konkrétní zobrazení scénáře a které vystavit vlastnosti pro dynamické hodnoty/obsah potřebný pomocí našich šablon zobrazení. Naše třídy kontroleru můžete naplnit a předat zobrazit šablonu pro použití těchto tříd optimalizovaných pro zobrazení. Díky tomu bezpečnost typů, kontrola za kompilace a intellisense editoru v rámci šablon zobrazení.

Například chcete-li povolit dinner formulář úprav scénáře, můžeme vytvořit "DinnerFormViewModel" třídy jako níže, který poskytuje dvě vlastnosti se silnými typy: objekt Dinner a model SelectList potřebné k naplnění dropdownlist zemích:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

Jsme pak aktualizujte metodě Edit() akce vytvoření DinnerFormViewModel pomocí Dinner objekt, který se nám načíst z našeho úložiště a pak ji předejte do našich zobrazení šablony:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

My se vám pak aktualizace našich zobrazit šablonu tak, že se očekává, že "DinnerFormViewModel" místo "Dinner" objekt změnou atributu "dědí" v horní části stránky edit.aspx takto:

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

Jakmile provedeme příkaz to, intellisense vlastnost "Model" v rámci naší zobrazení šablony se bude aktualizovat tak, aby odrážely objektový model DinnerFormViewModel typ, který jsme se předáním:

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

Budeme aktualizovat naše zobrazit kód pro práci z něj. Všimněte si, že níže toho, jak se mění názvy elementů vstupu vytváříme (prvky formuláře bude stále mít název "Title", "Země") – ale aktualizujeme metody pomocné rutiny HTML pro načtení hodnoty horizontálních oddílů pomocí třídy DinnerFormViewModel:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

Také aktualizujeme naše metodu post upravit pomocí třídy DinnerFormViewModel při chyb při vykreslování:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

Můžete také aktualizujeme naše Create() metody akce k opětovnému použití přesně stejné *DinnerFormViewModel* třídy umožňující zemí DropDownList souvisejícím adresářům také. Tady je implementace protokolu HTTP GET:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

Tady je implementace metody POST protokolu HTTP vytvořit:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

A teď podporují náš Edit and Create obrazovky rozevíracích seznamů pro výběr příslušné země.

### <a name="custom-shaped-viewmodel-classes"></a>Ve tvaru vlastních tříd ViewModel

Ve výše uvedeném scénáři Naše třída DinnerFormViewModel přímo zveřejňuje objekt modelu Dinner jako vlastnost, společně s podpůrnou SelectList vlastnosti modelu. Tento postup funguje pro scénáře, ve kterém chceme vytvořit v rámci naší šablona zobrazení uživatelského rozhraní HTML odpovídá relativně úzce naše objekty modelu domény.

Pro scénáře, kdy to není případ, jednou z možností, které můžete použít je pro vytvoření třídy ViewModel ve tvaru vlastní jehož objektový model více optimalizované pro spotřebu zobrazením – a výsledek bude vypadat zcela liší od základní objekt modelu domény. Například by mohla potenciálně vystavit názvy různých vlastností a/nebo agregační vlastností shromážděných z více objektů modelu.

Může být ve tvaru vlastních tříd ViewModel používají k předávání dat z řadičů pro zobrazení k vykreslení, jakož i pomáhaly zvládat data formuláře odeslat zpět do metody akce kontroler. Pro tento scénář dále může být metoda akce aktualizovat objekt ViewModel s daty publikování formuláře a pak použijte k mapování nebo načtení objektu modelu skutečné domény ViewModel instanci.

Ve tvaru vlastních tříd ViewModel může poskytnout značnou flexibilitu a jsou něco, co můžete prozkoumat pokaždé, když zjistíte v kódu vykreslování v rámci šablony zobrazení nebo formuláře účtování kód uvnitř své metody akce spuštění zobrazíte příliš složitý. To je často znaménko doménových modelů, ke které neodpovídají přímo v uživatelském rozhraní jsou generovány a že může pomoct zprostředkující třída ViewModel ve tvaru vlastní.

### <a name="next-step"></a>Dalším krokem

Nyní Podívejme se na jak můžete použít částečných zobrazení a stránky předlohy a znovu použít a sdílet napříč naší aplikace uživatelského rozhraní.

> [!div class="step-by-step"]
> [Předchozí](provide-crud-create-read-update-delete-data-form-entry-support.md)
> [další](re-use-ui-using-master-pages-and-partials.md)
