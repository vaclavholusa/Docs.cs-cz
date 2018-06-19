---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: Znovu použijte uživatelské rozhraní pomocí stránky předlohy a částečné. | Microsoft Docs
author: microsoft
description: Krok 7 zjistí způsoby použijeme SUCHÝCH zásadu v rámci našich šablon zobrazení omezit zdvojení kódu, pomocí šablony částečné zobrazení a stránky předlohy.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: ade655f3a4a429360b678d02fb564ac9cf255d42
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870008"
---
<a name="re-use-ui-using-master-pages-and-partials"></a>Znovu použijte uživatelské rozhraní pomocí stránky předlohy a částečné.
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Toto je krok 7 bezplatný [kurz aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , nevystavíte slabé stránky zabezpečení – prostřednictvím postup sestavení malá, ale dokončení, webové aplikace pomocí ASP.NET MVC 1.
> 
> Krok 7 zjistí způsoby použijeme "SUCHÝCH zásada" v našich šablon zobrazení omezit zdvojení kódu, pomocí šablony částečné zobrazení a stránky předlohy.
> 
> Pokud používáte ASP.NET MVC 3, doporučujeme provedením [získávání spuštěna s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Hudba úložiště](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.


## <a name="nerddinner-step-7-partials-and-master-pages"></a>NerdDinner krok 7: Částečné a hlavní stránky

Jedním z filozofiemi návrhu, které pracuje s ASP.NET MVC je Princip "Provést není opakujte sami", který se (často označované jako "Suchého"). SUCHÝCH návrhu pomáhá eliminovat duplikace kódu a logiky, který nakonec aplikace k sestavení rychlejší a jednodušší správu.

Jsme viděli již SUCHÝCH zásady použité v některé z našich NerdDinner scénářů. Několik příkladů: ověřovací logiku je implementována v rámci naší vrstva modelu, který by mohl být vymáhaná v obou upravit a vytvořit scénáře v kontroleru; znovu používáme zobrazit šablonu "NotFound" napříč upravit, podrobnosti a Delete metody akce; Používáme konvence - vzoru pro pojmenovávání pomocí našich šablon zobrazení, které se eliminuje potřeba explicitně zadáte název, když jsme volat metodu helper View(); a jsme se znovu pomocí třídy DinnerFormViewModel pro obě upravit a vytvořit akce scénáře.

Nyní Podíváme se na způsoby použijeme "SUCHÝCH zásada" v našich šablon zobrazení také omezit zdvojení kódu existuje.

### <a name="re-visiting-our-edit-and-create-view-templates"></a>Znovu návštěvou naše upravit a vytvořit zobrazení šablony

Aktuálně používáme dvě šablony jiné zobrazení – "Edit.aspx" a "Create.aspx" – k zobrazení formuláře naše večeři uživatelského rozhraní. Rychlé visual porovnání je označuje, jak podobné jsou. Dole je, jak vytvořit formulář vypadá:

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

A tady je naše "Upravit" formuláře, které bude vypadat takto:

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

Většinu rozdíl je k dispozici? Než text nadpisu a záhlaví ovládacích prvků pro rozložení a vstup formuláře jsou identické.

Pokud jsme otevřít "Edit.aspx" a "Create.aspx" Zobrazit šablony, které jsme najdete, které obsahují řídicí kód rozložení a vstup formuláře identické. Tato duplikace znamená, že jsme skončili nutnosti provádět změny dvakrát kdykoliv jsme zavést nebo změnit novou vlastnost večeři – což není funkční.

### <a name="using-partial-view-templates"></a>Pomocí šablon pro částečné zobrazení

ASP.NET MVC podporuje možnost definovat šablony "částečné zobrazení", které lze použít k zapouzdření zobrazení vykreslování logiku pro dílčí části stránky. "Částečné." Zadejte užitečný způsob, jak definovat logiku vykreslení zobrazení jednou a znovu ho pak použít na více místech v aplikaci.

Pokud chcete "Suchého up" naše Edit.aspx a Create.aspx zobrazení šablony duplikace, můžeme vytvořit šablonu částečné zobrazení s názvem "DinnerForm.ascx", který zapouzdřuje rozložení formuláře a vstupní elementy, které jsou společné pro objekty. Provedeme to tak, že pravým tlačítkem myši na našich nebo zobrazení či večeří adresáře a výběr "Přidat -&gt;zobrazení" příkazu v nabídce:

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

Zobrazí se dialogové okno "Přidat zobrazení". Jsme budete název nového zobrazení jsme chcete vytvořit "DinnerForm", zaškrtněte políčko "Vytvořit částečné zobrazení" v dialogovém okně a znamenat, že jsme předá ho DinnerFormViewModel třídy:

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

Když kliknete na tlačítko "Přidat", Visual Studio vytvoří novou šablonu zobrazení "DinnerForm.ascx" pro nám v adresáři "\Views\Dinners".

Jsme můžete pak zkopírujte a vložte rozložení duplicitní formuláře / vstup řídicí kód z našich šablon zobrazení Edit.aspx/ Create.aspx na naší nové částečné zobrazení šablony "DinnerForm.ascx":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

Můžeme aktualizovat našich šablon zobrazení upravit a vytvořit volání částečné šablony DinnerForm a eliminovat duplikace formuláře. Jsme to můžete udělat volání Html.RenderPartial("DinnerForm") v našich šablon zobrazení:

##### <a name="createaspx"></a>Create.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a>Edit.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

Explicitně kvalifikovat cesta částečné šablony, které mají být při volání metody Html.RenderPartial (například: ~ Views/Dinners/DinnerForm.ascx "). V našem kódu výše ale jsme se využívat výhod založené na konvenci pojmenování vzor v rámci rozhraní ASP.NET MVC a zadání "DinnerForm" jako název částečného k vykreslení. Když jsme to bude vypadat ASP.NET MVC první v adresáři založené na konvenci zobrazení (pro DinnersController by to bylo/zobrazení/večeří). Pokud se nenajde částečné šablony existuje ho bude potom vyhledejte ho v adresáři /Views/Shared.

Při Html.RenderPartial() pomocí jen název částečného zobrazení, ASP.NET MVC předá částečné zobrazení stejné objekty slovník modelu a ViewData používá volání zobrazit šablonu. Alternativně existují přetížené verzích Html.RenderPartial(), které vám umožní předat alternativní objekt modelu nebo ViewData slovník pro částečné zobrazení k použití. To je užitečné pro scénáře, kde chcete předat podmnožinu úplný Model nebo ViewModel.

| **Téma straně: Proč &lt;%%&gt; místo &lt;% = %&gt;?** |
| --- |
| Jeden z jemně věcí možná jste si všimli s výše uvedený kód je, že používáme &lt;%%&gt; blokovat místo &lt;% = %&gt; blokovat při volání metody Html.RenderPartial(). &lt;% = %&gt; bloky v ASP.NET znamenat, že vývojář chce vykreslení zadanou hodnotu (například: &lt;% = "Hello" %&gt; by vykreslení "text Hello"). &lt;%%&gt; bloků místo znamenat, že vývojář chce ke spouštění kódu a že žádné vykreslen výstup v nich je třeba provést explicitně (například: &lt;Response.Write("Hello") %&gt;. Z důvodu používáme &lt;%%&gt; blok s výše uvedený kód naše Html.RenderPartial je, že metoda Html.RenderPartial() nevrací řetězec a místo toho výstupy obsah přímo k volání zobrazit šablonu do výstupního datového proudu. Dělá to z důvodů výkonu efektivitu a pomocí tohoto postupu, tak ho nevyžaduje nutnost vytvoření objektu (potenciálně velký) dočasné řetězce. To snižuje využití paměti a zvyšuje propustnost celkové aplikace. Při použití Html.RenderPartial() je zapomenete přidat středníkem na konci volání, když je v rámci jednoho Obvyklou chybou &lt;%%&gt; bloku. Například tento kód způsobí chybu kompilátoru: &lt;Html.RenderPartial("DinnerForm") %&gt; místo toho musíte k zápisu: &lt;% Html.RenderPartial("DinnerForm"); %&gt; totiž &lt;%%&gt; bloky jsou příkazy nezávislý kódu a při použití kódu jazyka C# příkazy potřeba ukončit středníkem. |

### <a name="using-partial-view-templates-to-clarify-code"></a>O vysvětlení, kód šablon částečné zobrazení

Jsme vytvořili šablonu "DinnerForm" částečné zobrazení, se neduplikovaly logiku vykreslení zobrazení na více místech. Toto je nejčastější příčinou vytvoření šablon částečné zobrazení.

Někdy stále má smysl i v případě, že se se pouze nazývají na jednom místě, vytvoření částečné zobrazení. Velmi složité zobrazit šablony se může stát často mnohem snadněji číst při jejich logiku pro vykreslení zobrazení je extrahována a oddíly do jedné nebo více dobře s názvem částečné šablony.

Zvažte například následující fragment kódu ze souboru Site.master v našem projektu (aplikaci, kterou jsme se vidí krátce). Kód je poměrně jednoduché číst – částečně, protože logiku zobrazíte přihlášení/odhlášení odkaz v horní části je napravo od obrazovky zapouzdřený partial "LogOnUserControl":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

Vždy, když se přistihnete získávání nerozumíte pokusu pochopit kód kódu html nebo v šabloně na zobrazení, zvažte, jestli by nebylo lepší, pokud se některé byl extrahovat a rozdělili do dobře pojmenované částečné zobrazení.

### <a name="master-pages"></a>Stránky předlohy

Také podporuje částečné zobrazení, ASP.NET MVC podporuje také schopnost vytvářet šablony "stránka předlohy", které lze použít k definování běžné rozložení a lokality nejvyšší úrovně html. Zástupný symbol, který ovládací prvky lze přidat na stránku předlohy pro identifikaci replaceable oblasti, které můžete přepsat nebo ", vyplní" zobrazení obsahu. To umožňuje velmi efektivní (a SUCHÝCH) běžné rozložení platí v celé aplikaci.

Ve výchozím nastavení mají nové projekty ASP.NET MVC šablonu stránky předlohy automaticky přidá do nich. Tato stránka předlohy je s názvem "Site.master" a život ve složce \Views\Shared\:

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

Výchozí soubor Site.master vypadá níže. Definuje vnější html v lokalitě, společně s nabídky pro navigaci v horní části. Obsahuje dva prvky replaceable zástupný symbol obsahu – jeden pro název a druhou pro kde by měla být nahrazená primární obsah stránky:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

Všechna zobrazení šablony, které vytvořili jsme pro naši aplikaci NerdDinner ("Seznamu", "Podrobnosti", "Upravit", "Vytvoření", "NotFound", atd) mají byla založená na šabloně této Site.master. To je indikován atribut "MasterPageFile", která byla přidána ve výchozím nastavení do horní části &lt;% @ % stránky&gt; – direktiva Pokud jsme vytvořili naše zobrazení v dialogovém okně "Přidat zobrazení":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

To znamená, že jsme můžete změnit obsah Site.master a mají změny automaticky se použijí a používán, když jsme vykreslení některou z našich šablon zobrazení.

Umožňuje aktualizovat naše Site.master záhlaví části tak, aby hlavičku naše aplikace "NerdDinner" místo "Moje aplikace MVC". Můžeme také aktualizovat naše navigační nabídce tak, že na první kartě je "Najít večeři" (zpracovávaných metody akce HomeController Index()) a umožňuje přidat novou kartu názvem "Hostitele večeři" (zpracovávaných metody akce DinnersController Create()):

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

Jsme uložit soubor Site.master a aktualizace našich prohlížeče uvidíme náš záhlaví změní zobrazit napříč všechna zobrazení v naší aplikaci. Příklad:

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

A */Dinners/Edit / [id]* adresy URL:

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a>Další krok

Částečné a hlavní stránky poskytují velmi flexibilní možnosti, které vám umožní řádně uspořádání zobrazení. Zjistíte, že jejich vyhnuli se duplikování zobrazení obsahu / kód a snadněji číst a spravovat vaše šablony zobrazení.

Pojďme nyní pokroku výpis scénáře, který jsme vytvořili výše a škálovatelné podporu stránkování.

> [!div class="step-by-step"]
> [Předchozí](use-viewdata-and-implement-viewmodel-classes.md)
> [další](implement-efficient-data-paging.md)
