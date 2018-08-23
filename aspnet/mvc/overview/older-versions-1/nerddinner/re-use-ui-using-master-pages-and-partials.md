---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: Opakované použití uživatelského rozhraní pomocí stránek předloh a částečných zobrazení | Dokumentace Microsoftu
author: microsoft
description: Krok 7 zjistí způsoby, jak můžete použít suchého zásady v rámci naší zobrazení šablon pro odstranění duplicit kódu, pomocí šablony částečné zobrazení a stránky předlohy.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: 0da32e6ac38f10df6e581517989b3b1fd2f2328c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753125"
---
<a name="re-use-ui-using-master-pages-and-partials"></a>Opakované použití uživatelského rozhraní pomocí stránek předloh a částečných zobrazení
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Toto je krok 7 bezplatného [kurz vývoje aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , který procházení procházení po tom, jak sestavit malý, ale bylo možné provést, webové aplikace pomocí ASP.NET MVC 1.
> 
> Krok 7 zjistí způsoby, jak můžete použít "Zásada SUCHÝCH" v rámci naší zobrazení šablon pro odstranění duplicit kódu, pomocí šablony částečné zobrazení a stránky předlohy.
> 
> Pokud používáte ASP.NET MVC 3, doporučujeme je provést [získávání začít s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.


## <a name="nerddinner-step-7-partials-and-master-pages"></a>NerdDinner krok 7: Stránek předloh a částečných zobrazení

Jednou z filozofiemi návrh, který ASP.NET MVC poskytuje výkonný je "Proveďte není opakujte sami" principu (obvykle označuje jako "Zkušební"). Návrh suchého pomáhá eliminovat konflikty duplikace kódu a logiky, takže v konečném důsledku díky aplikací rychlejší sestavení a jednodušší správu.

Už jsme viděli suchého zásady použitý v některé z našich NerdDinner scénářů. Několik příkladů: Naše logiku ověřování je implementován v rámci našich modelu vrstvy, který umožňuje vynucovat pro obě upravit a vytvořit scénáře v kontroleru; znovu používáme "Serveru" Zobrazit šablonu pro všechny metody akce upravit, podrobnosti a Delete; pomocí našich šablon zobrazení, které není potřeba explicitně zadat název, pokud jsme volat metodu helper View(); se používá konvence - vzoru pro pojmenovávání a My se znovu pomocí třídy DinnerFormViewModel pro obě upravit a vytvořit akce scénáře.

Nyní Podívejme se na způsoby, jak můžete použít "Zásada SUCHÝCH" v rámci naší zobrazit šablony také eliminovat dojde k duplikaci kódu existuje.

### <a name="re-visiting-our-edit-and-create-view-templates"></a>Znovu navštívit naše upravit a vytvořit zobrazení šablony

Aktuálně používáme dvě šablony jiné zobrazení – "Edit.aspx" a "Create.aspx" – k zobrazení formuláře naše společnost Dinner uživatelského rozhraní. Rychlé vizuální porovnání je zvýrazní, jak podobné jsou. Níže je, jak vytvořit formulář vypadat:

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

A tady je náš "Edit" formulář vypadá jako:

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

Není moc rozdíl je k dispozici? Kromě textu nadpisu a záhlaví jsou stejné ovládací prvky pro vstup a rozložení formuláře.

Otevíráme "Edit.aspx" a "Create.aspx" Zobrazit šablony, na které narazíme budete, které obsahují kód identické formuláře rozložení a vstupní ovládací prvek. Tato duplikace znamená, že jsme skončit nutnosti provádět změny dvakrát kdykoli jsme zavést nebo změnit novou vlastnost Dinner – který ovšem není vhodné.

### <a name="using-partial-view-templates"></a>Pomocí šablony částečné zobrazení

ASP.NET MVC podporuje možnost definovat šablony "částečné zobrazení", které můžete použít k zapouzdření logiku pro vykreslení zobrazení pro dílčí části stránky. "Částečné." užitečný způsob, jak definovat logiku pro vykreslení zobrazení jednou a pak znovu použít na více místech v aplikaci.

Abychom "Zkušební up" náš Edit.aspx a Create.aspx zobrazení šablony duplikování, můžeme vytvořit šablonu částečné zobrazení s názvem "DinnerForm.ascx", který zapouzdřuje rozložení formuláře a vstupní prvky, které jsou společné pro. Provedeme to tak, že kliknete pravým tlačítkem na náš večeří adresář/zobrazení/a zvolíte "Add -&gt;zobrazení" příkazu nabídky:

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

Zobrazí se dialogové okno "Přidat zobrazení". Jsme budete pojmenujte nové zobrazení jsme chcete vytvořit "DinnerForm", zaškrtněte políčko "Vytvořit částečné zobrazení" v rámci dialogového okna a označuje, že jsme se předáváme DinnerFormViewModel třídy:

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

Když kliknete na tlačítko "Přidat", Visual Studio vytvořte novou šablonu zobrazení "DinnerForm.ascx" nám v adresáři "\Views\Dinners".

My pak zkopírovat a Vložit rozložení formuláře duplicitní / vstup ovládacího prvku kódu z našich šablon zobrazení Edit.aspx/ Create.aspx na naše nové šablony částečné zobrazení "DinnerForm.ascx":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

Aktualizujeme můžete poté zobrazit šablony pro volání šablony částečné DinnerForm a vyloučení duplicity formuláře naše Edit and Create. Můžeme to udělat pomocí volání Html.RenderPartial("DinnerForm") v rámci naší zobrazit šablony:

##### <a name="createaspx"></a>Create.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a>Edit.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

Můžete explicitně kvalifikovat cesta částečné šablony, které chcete, aby při volání metody Html.RenderPartial (například: ~ Views/Dinners/DinnerForm.ascx "). V našem kódu výše ale jsou výhod založené na konvenci pojmenování v rámci technologie ASP.NET MVC jsme stačí zadat "DinnerForm" jako název části pro vykreslení. Když to uděláme ASP.NET MVC bude hledat v adresáři zobrazení podle úmluvy první (DinnersController jde/zobrazení/večeří). Pokud se nenajde částečné šablony existuje ho bude vyhledejte ho v adresáři /Views/Shared.

Při volání Html.RenderPartial() s pouze název částečného zobrazení ASP.NET MVC předá částečného zobrazení stejné objekty slovníku modelu a ViewData používá volání zobrazit šablonu. Alternativně jsou přetížené verze Html.RenderPartial(), které vám umožní předat alternativní objekt modelu a/nebo slovníku ViewData pro částečné zobrazení k použití. To je užitečné pro scénáře, kde chcete předat podmnožinu úplný Model/ViewModel.

| **Téma na straně: Proč &lt;%%&gt; místo &lt;% = %&gt;?** |
| --- |
| Jedním z malý věci, možná jste si všimli s výše uvedený kód je, že používáme &lt;%%&gt; blokovat místo &lt;% = %&gt; blokování při volání metody Html.RenderPartial(). &lt;% = %&gt; bloky v technologii ASP.NET znamenat, že vývojář chce, aby se pro vykreslení zadané hodnoty (například: &lt;% = "Vítáme uživatele" %&gt; vykreslení "Hello"). &lt;%%&gt; bloky oznamují ale, že vývojář chce spouštět kód a že žádné vykreslen výstup v nich je nutné provést explicitně (například: &lt;Response.Write("Hello") %&gt;. Z důvodu používáme &lt;%%&gt; blok s naší výše uvedený kód Html.RenderPartial totiž Html.RenderPartial() metoda nevrací řetězec a místo toho vypíše obsah přímo do volání šablony zobrazení výstupního datového proudu. Dělá to z důvodů výkonu efektivitu a tímto způsobem, takže se eliminuje nutnost vytvoření objektu (potenciálně velmi velké) dočasný řetězec. Tím se sníží využití paměti a zvyšuje propustnost celkové aplikace. Při použití Html.RenderPartial() je zapomenout přidat středníky na konci volání, pokud je v rámci jednoho běžnou chybou &lt;%%&gt; bloku. Například tento kód způsobí chybu kompilátoru: &lt;Html.RenderPartial("DinnerForm") %&gt; místo toho budete muset napsat: &lt;% Html.RenderPartial("DinnerForm"); %&gt; důvodem je, že &lt;%%&gt; bloky jsou příkazy samostatná kódu a při použití kódu jazyka C# příkazy musí být ukončen direktivou středníkem. |

### <a name="using-partial-view-templates-to-clarify-code"></a>Upřesněte svůj kód pomocí šablony částečné zobrazení

Jsme vytvořili šablony částečné zobrazení "DinnerForm", která má systém předchází vzniku duplicitní logiky vykreslování zobrazení na více místech. Toto je nejběžnějším důvodem k vytvoření šablony částečné zobrazení.

Někdy je stále vhodné vytvořit částečné zobrazení, i když jsou se jen volá na jednom místě. Velmi složité zobrazit šablony se může stát často mnohem snazší přečíst, pokud je extrahována a rozdělená na jeden nebo více dobře s názvem šablony částečné jejich logiku pro vykreslení zobrazení.

Představme si třeba, následující fragment kódu ze souboru Site.master v našem projektu (což bude mít se díváme na za chvíli). Kód je poměrně jednoduché číst – částečně proto, že v horní části propojit logiku k zobrazení přihlášení/odhlášení pravé části obrazovky je zapouzdřené v části "LogOnUserControl":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

Pokaždé, když si uvědomíte, získávání zaměňovat pokusu pochopit kód html nebo kód v rámci zobrazení šablony, zvažte, jestli by nebylo bude jasnější, když některé z jeho byla extrahována a implementovány do dobře pojmenované částečné zobrazení.

### <a name="master-pages"></a>Stránky předlohy

Kromě podpory částečná zobrazení, ASP.NET MVC podporuje také možnost vytvářet šablony "stránka předlohy", které lze použít k definování obecné rozložení a lokality nejvyšší úrovně html. Zástupný symbol, který je potom možné přidat ovládací prvky na stránce předlohy k identifikaci nahraditelné oblastí, které můžete přepsat nebo "vyplnění" podle zobrazení obsahu. To umožňuje velmi efektivní (a suchého) běžné rozložení můžete použít v aplikaci.

Nové projekty ASP.NET MVC mají ve výchozím nastavení šablona stránky předlohy k nim přidány automaticky. Tato stránka předlohy se s názvem "Site.master" a žije ve složce \Views\Shared\:

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

Výchozí soubor Site.master vypadá níže. Definuje vnější html webu a nabídky pro navigaci v horní části. Obsahuje dva prvky replaceable zástupný symbol obsahu – jeden pro název a druhou pro kde by měla být nahrazená primární obsahu stránky:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

Všechny zobrazit šablony, kterou jsme vytvořili pro naši aplikaci NerdDinner ("List", "Podrobnosti", "Edit", "Vytvořit", "Serveru" atd.) mají byly založeny na šabloně Site.master. Je toto označeno pomocí atributu "MasterPageFile", který byl přidán ve výchozím nastavení do horní části &lt;% @ Page %&gt; směrnice, i když jsme vytvořili naši zobrazení pomocí dialogového okna "Přidat zobrazení":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

To znamená, že můžeme změnit obsah Site.master a mají změny automaticky se použije a používán, když jsme některé z našich šablon zobrazení vykreslení.

Umožňuje aktualizovat naše Site.master záhlaví tak, aby záhlaví naši aplikaci je "NerdDinner" místo "Moje aplikace MVC". Teď také aktualizovat naše navigační nabídce tak, aby první karta je "Najít webu Dinner" (zpracované metody akce HomeController Index()) a přidejme na nové kartě se nazývá "Hostitele večeři" (zpracované metody akce DinnersController Create()):

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

Když jsme uložit soubor Site.master a aktualizaci prohlížeče uvidíme náš záhlaví změn ve všech zobrazeních v rámci naší aplikace. Příklad:

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

A */Dinners/Edit / [id]* adresy URL:

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a>Dalším krokem

Částečných zobrazení a stránky předlohy poskytují velmi flexibilní možnosti, které vám umožní čistě uspořádání zobrazení. Zjistíte, aby umožňují předcházet duplikování zobrazení obsahu / kódu a usnadňují a zjednodušíte si zobrazit šablony.

Pojďme teď návštěvě výpis scénáře, který jsme vytvořili dříve a povolit škálovatelné podporu stránkování.

> [!div class="step-by-step"]
> [Předchozí](use-viewdata-and-implement-viewmodel-classes.md)
> [další](implement-efficient-data-paging.md)
