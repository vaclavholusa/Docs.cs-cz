---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
title: Seznámení s aktivační události pro ASP.NET AJAX UpdatePanel | Microsoft Docs
author: scottcate
description: Při práci se v editoru kódu v sadě Visual Studio, můžete si povšimnout (z IntelliSense), jsou dva podřízené elementy ovládací prvek UpdatePanel. Jeden z shod...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2008
ms.topic: article
ms.assetid: faab8503-2984-48a9-8a40-7728461abc50
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
msc.type: authoredcontent
ms.openlocfilehash: f30f2ead402d2f49a89b2caf47cc30b6445d4cfb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890746"
---
<a name="understanding-aspnet-ajax-updatepanel-triggers"></a>Seznámení s ASP.NET AJAX UpdatePanel aktivační události
====================
podle [Scott Dikovat](https://github.com/scottcate)

[Stáhnout PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial02_Triggers_cs.pdf)

> Při práci se v editoru kódu v sadě Visual Studio, můžete si povšimnout (z IntelliSense), jsou dva podřízené elementy ovládací prvek UpdatePanel. Z nichž jeden je element aktivační události, který určuje ovládacích prvků na stránce (nebo uživatelského ovládacího prvku, pokud ji používáte), aktivují částečné vykreslení ovládacího prvku UpdatePanel, ve kterém se nachází element.


## <a name="introduction"></a>Úvod

Technologie ASP.NET společnosti Microsoft poskytuje programovací model objektově orientované a událostmi řízené a jednotek s výhod zkompilovaný kód. Svůj model zpracování na straně serveru, má však několik nevýhody vyplývajících z technologie mnoho z nichž lze řešit novým funkcím zahrnutým v AJAX rozšíření Microsoft ASP.NET 3.5. Tato rozšíření umožňují mnoha nových funkcí plně funkčního klienta, včetně částečným vykreslením stránky bez nutnosti celou stránku obnovení, přístup prostřednictvím klientského skriptu (včetně ASP.NET, rozhraní API pro profilaci) a rozhraní API rozsáhlé klientské webové služby určená pro řadu schémata řízení vidět v sadě serverový ovládací prvek ASP.NET zrcadlení.

Tento dokument White Paper prozkoumá XML aktivace funkce technologie ASP.NET AJAX `UpdatePanel` součásti. XML aktivační události udělte podrobnou kontrolu nad součásti, které může způsobit částečné vykreslování pro konkrétní ovládací prvky UpdatePanel.

Tento dokument White Paper je založen na beta verzi 2 verzi rozhraní .NET Framework 3.5 a Visual Studio 2008. Rozšíření ASP.NET AJAX, dříve sestavení rozšíření zaměřený na technologii ASP.NET 2.0, jsou teď integrované do knihovně tříd rozhraní .NET Framework Base. Tento dokument White Paper také předpokládá, že budou pracovat se Visual Studio 2008, není Visual Web Developer Express a poskytne návody podle uživatelského rozhraní sady Visual Studio (i když bude zcela kompatibilní bez ohledu na to výpis kódu vývojové prostředí).

## <a name="triggers"></a>*Triggery*

Aktivační události pro danou UpdatePanel, ve výchozím nastavení, automaticky zahrnout všechny podřízené ovládací prvky, které vyvolají zpětné volání, včetně (například) ovládací prvky textové pole, které mají své `AutoPostBack` vlastnost nastavena na hodnotu **true**. Ale aktivační události může být také součástí deklarativně pomocí značek; To se provádí v rámci `<triggers>` části deklarace UpdatePanel ovládacího prvku. I když aktivační události je přístupná prostřednictvím `Triggers` vlastnost kolekce se doporučuje registraci žádné aktivační události částečné vykreslování za běhu (například pokud ovládací prvek není k dispozici v době návrhu) pomocí `RegisterAsyncPostBackControl(Control)` metodu Pro vaše stránky v objektu ScriptManager `Page_Load` událostí. Mějte na paměti, jsou bezstavové stránky, a proto byste měli znovu zaregistrovat tyto ovládací prvky pokaždé, když jejich vytvoření.

Zahrnutí automatické podřízené aktivační událost můžete také je možné zakázat (podřízených ovládacích prvků, které vytvářejí postback nespouštějí automaticky vykreslí částečné) nastavením `ChildrenAsTriggers` vlastnost **false**. To vám umožní nejvyšší flexibilitu v přiřazení ovládacích prvků, které konkrétní může vyvolat vykreslení stránky a doporučuje se, tak, aby vývojář bude výslovný souhlas reagovat na událost, nikoli zpracování všech událostí, které by mohlo dojít.

Všimněte si, že když jsou vnořené prvky UpdatePanel, když UpdateMode je nastavená na **podmíněného**, pokud se aktivuje podřízené UpdatePanel, ale nadřazená položka nemá, pak pouze podřízené UpdatePanel se aktualizuje. Ale pokud nadřazená UpdatePanel aktualizovat, pak podřízené UpdatePanel bude také aktualizovat.

## <a name="the-lttriggersgt-element"></a>*&lt;Aktivační události&gt; – Element*

Při práci se v editoru kódu v sadě Visual Studio, můžete si povšimnout (z IntelliSense), jsou dva podřízené elementy z `UpdatePanel` ovládacího prvku. Nejčastěji zaznamenané elementu je `<ContentTemplate>` element, který zapouzdřuje v podstatě obsah, který bude uchovávat pomocí panelu aktualizace (obsahu, pro které jsme povolit částečným vykreslením). Jiného elementu je `<Triggers>` element, který určuje ovládacích prvků na stránce (nebo uživatelského ovládacího prvku, pokud ji používáte), aktivují částečné vykreslení ovládacího prvku UpdatePanel, ve kterém &lt;aktivační události&gt; nachází element.

`<Triggers>` Element může obsahovat libovolný počet každé dva podřízené uzly: `<asp:AsyncPostBackTrigger>` a `<asp:PostBackTrigger>`. Obě přijmou dva atributy `ControlID` a `EventName`a můžete zadat libovolný ovládací prvek v rámci aktuální jednotka zapouzdření (například pokud vaše UpdatePanel ovládací prvek nachází v uživatelský ovládací prvek webu, byste neměli tak, aby odkazovaly na ovládací prvek Stránka, na kterém se bude nacházet uživatelského ovládacího prvku).

`<asp:AsyncPostBackTrigger>` Element je zvláště užitečná v tom, že ho můžete cílit na libovolnou událost z ovládacího prvku, který již existuje jako podřízený *žádné* ovládacího prvku UpdatePanel v jednotce zapouzdření, ne jenom UpdatePanel, pod kterým této aktivační události je podřízená . Proto můžete provést libovolný ovládací prvek se aktivovala aktualizace části stránky.

Podobně `<asp:PostBackTrigger>` element slouží k vykreslení částečná stránka aktivační událost, ale ten, který vyžaduje úplnou round-trip k serveru. Tento element aktivační události lze také vynutit vykreslení celou stránku při ovládacího prvku by jinak normálně aktivovat vykreslování částečná stránka (například když `Button` řízení existuje v `<ContentTemplate>` elementu ovládacího prvku UpdatePanel). PostBackTrigger element znovu, můžete zadat libovolný ovládací prvek, která je podřízená libovolný ovládací prvek UpdatePanel v aktuální jednotce zapouzdření.

## <a name="lttriggersgt-element-reference"></a>*&lt;Aktivační události&gt; odkaz na Element*

*Následníky značek:*

| **Značka** | **Popis** |
| --- | --- |
| &lt;asp:AsyncPostBackTrigger&gt; | Určuje, řízení a událost, která způsobí, že částečná stránka aktualizace pro UpdatePanel, který obsahuje odkaz na tento aktivační události. |
| &lt;asp:PostBackTrigger&gt; | Určuje, řízení a událost, která způsobí, že celou stránku aktualizace (aktualizace celou stránku). Tato značka slouží k vynucení úplné aktualizace, když ovládacího prvku by jinak aktivovat částečným vykreslením. |

## <a name="walkthrough-cross-updatepanel-triggers"></a>*Návod: Mezi UpdatePanel aktivační události*

1. Vytvořte novou stránku ASP.NET s objektem ScriptManager nastavit pro povolení částečným vykreslením. Přidat dvě komponenty UpdatePanel na tuto stránku – v prvním, zahrnují ovládací prvek popisek (Label1) a dvou ovládacích prvků tlačítko (Button1 a Button2). Button1 by mělo být uvedeno klikněte na tlačítko Aktualizovat i a Button2 by mělo být uvedeno klikněte na tlačítko Aktualizovat to nebo něco společně tyto řádky. V druhé UpdatePanel zahrnout pouze ovládací prvek popisek (Label2), ale nastaveno něco jiného než výchozí jej odlišit od jeho ForeColor – vlastnost.
2. Nastaví vlastnost UpdateMode obou UpdatePanel značek k **podmíněného**.

**Výpis 1: Kód pro default.aspx:** 

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample1.aspx)]

1. V obslužné rutiny události kliknutí pro Button1 nastavte Label1.Text a Label2.Text na něco závislá na čase (například DateTime.Now.ToLongTimeString()). Pro obslužné rutiny události kliknutí pro Button2 nastavte Label1.Text pouze na hodnotu závislá na čase.

**Výpis 2: Codebehind (oříznut) v default.aspx.cs:** 

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample2.cs)]

1. Stisknutím klávesy F5 sestavit a spustit projekt. Všimněte si, že po kliknutí na tlačítko jak panelů aktualizace, jak popisky změnit text; ale po kliknutí na tlačítko Aktualizovat tento Panel, aktualizuje jenom Label1.


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image2.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image1.png)

([Kliknutím zobrazit obrázek v plné velikosti](understanding-asp-net-ajax-updatepanel-triggers/_static/image3.png))


## <a name="under-the-hood"></a>*Pod pokličkou*

Použití příklad, který jsme právě vytvořený, můžeme provést pohled na činnosti prvku ASP.NET AJAX a fungování aktivační události naše UpdatePanel mezi panely. K tomu, budeme pracovat s zdroji generovaného stránky HTML, jakož i rozšíření Mozilla Firefox volána FireBug - s, jsme můžete snadno zkontrolovat zpětná volání AJAX. Budeme používat také nástroj .NET Reflector pomocí Lutz Roeder. Oba tyto nástroje jsou volně k dispozici online a lze najít pomocí hledání v Internetu.

Prozkoumání zdrojového kódu stránky zobrazuje téměř nic neobvyklého; ovládací prvky UpdatePanel se vykresluje jako `<div>` kontejnery a jsme viděli skriptu prostředků obsahuje zadaný pomocí `<asp:ScriptManager>`. Existují také některé nové AJAX konkrétní volání PageRequestManager které jsou interní klientské knihovny skriptu AJAX. Nakonec vidíme dva kontejnery UpdatePanel – jednu s vygenerované `<input>` tlačítka s dva `<asp:Label>` ovládací prvky se vykresluje jako `<span>` kontejnery. (Je-li si prohlédnout strom CODEDOM v FireBug, si všimněte, že popisky jsou neaktivní označíte, že jejich nejsou generovala viditelný obsah).

Klikněte na tlačítko Aktualizovat tento Panel a Všimněte si, že nejvyšší UpdatePanel bude aktualizována na aktuální čas serveru. V FireBug vyberte kartu konzoly tak, aby můžete zkontrolovat žádost. Nejprve zkontrolujte parametry požadavku POST:


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image5.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image4.png)

([Kliknutím zobrazit obrázek v plné velikosti](understanding-asp-net-ajax-updatepanel-triggers/_static/image6.png))


Všimněte si, že UpdatePanel má uvedené kódu AJAX na straně serveru, přesněji byla aktivována, které ovládací prvek stromu pomocí parametru ScriptManager1: `Button1` z `UpdatePanel1` ovládacího prvku. Nyní klikněte na tlačítko jak panelů aktualizace. Potom zkoumání odpovědi, vidíme oddělený kanálu řady proměnných nastavit v řetězci; Konkrétně vidíme nejvyšší UpdatePanel `UpdatePanel1`, má celého jeho HTML odesláno prohlížeči. Skript AJAX klientské knihovny nahradí původní HTML prvku UpdatePanel obsahu nového obsahu prostřednictvím `.innerHTML` vlastnost, a, server odešle změněný obsah ze serveru ve formátu HTML.

Nyní klikněte na tlačítko jak panelů aktualizace a podívejte se na výsledky ze serveru. Výsledky jsou velmi podobné – obě komponenty UpdatePanel příjem nových HTML ze serveru. Stejně jako u předchozí zpětné volání, je odeslána stav další stránky.

Jak jsme můžete vidět, protože žádný speciální kód je využívat k provedení zpětné volání AJAX, je schopen zachytit zpětná vystavení formuláře bez jakékoli další kód klientské knihovny skriptu AJAX. Ovládací prvky serveru automaticky využívat JavaScript tak, aby se neodešlou automaticky formuláře – ASP.NET automaticky vloží kód pro ověřování formuláře a stavu již, především dosáhnout zahrnutí prostředků automatické skriptu, PostBackOptions – třída a třída ClientScriptManager.

Zvažte ovládací prvek zaškrtávací políčko; Zkontrolujte zpětný překlad – třída v rozhraní .NET Reflector. Uděláte to tak, zajistěte, aby byl váš sestavení System.Web otevřené a přejděte do `System.Web.UI.WebControls.CheckBox` třída, otevřete `RenderInputTag` metoda. Vyhledejte podmíněného, která kontroluje `AutoPostBack` vlastnost:


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image8.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image7.png)

([Kliknutím zobrazit obrázek v plné velikosti](understanding-asp-net-ajax-updatepanel-triggers/_static/image9.png))


Pokud je zapnutá automatické zpětné volání `CheckBox` řízení (prostřednictvím automatického odesílání vlastnost je true), výsledné `<input>` je proto vykreslen s ASP.NET událostí zpracování skript v jeho `onclick` atribut. Zachycení odeslání formuláře, pak umožňuje prvku ASP.NET AJAX na stránku vložit nonintrusively, pomáhá vyhnout všechny potenciální nejnovější změny, které můžou nastat s využitím náhradní řetězec možná nepřesný. Navíc díky *žádné* vlastní ovládací prvek ASP.NET využívat power prvku ASP.NET AJAX bez jakékoli další kód pro podporu jeho použití v rámci kontejner UpdatePanel.

`<triggers>` Funkce odpovídá na hodnoty v volání PageRequestManager inicializovat \_updateControls (Všimněte si, že skript klientské knihovny ASP.NET AJAX využívá konvence této metody, události a názvy polí, které začínají podtržítkem jsou označeny jako vnitřní a nejsou určené pro použití mimo samotné knihovny). Pomocí něho jsme můžete sledovat ovládacích prvků, které jsou určeny k způsobit zpětná volání AJAX.

Umožňuje přidat například dva další ovládací prvky na stránku, a jeden prvek mimo komponenty UpdatePanel zcela a jeden v rámci prvku UpdatePanel nechá. Nemůžeme přidat ovládací prvek zaškrtávací políčko v horní UpdatePanel a vyřadit rozevírací seznam s počtem barvy definované v seznamu. Zde je kód nové:

**Výpis 3: Nový kód**

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample3.aspx)]

A tady je nové kódu:

**Výpis 4: Codebehind**

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample4.cs)]

Cílem této stránky je rozevíracího seznamu vybere mezi tři barvy a zobrazí druhý popis, zaškrtněte políčko určuje, zda je tučné i jestli štítky zobrazují data a také čas. Zaškrtněte políčko by nemělo způsobit aktualizaci AJAX, ale měli rozevíracího seznamu, i když není umístěné mezi UpdatePanel.


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image11.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image10.png)

([Kliknutím zobrazit obrázek v plné velikosti](understanding-asp-net-ajax-updatepanel-triggers/_static/image12.png))


Jako, je třeba na výše uvedené snímku obrazovky byla nejnovějšího tlačítko kliknout pravým tlačítkem tento Panel aktualizace, která aktualizuje nejvyšší dobu nezávisle na dolní čas. Datum se také vypnout mezi kliknutím, protože datum je zobrazené v popisku dolní. Nakonec zájmu je barva popisků dolní: byl dříve než text popisku, který ukazuje, že stav řízení je důležité, aktualizovat a uživatelé očekávají, že je možné zachovat prostřednictvím zpětná volání AJAX. *Ale*, čas nebyl aktualizován. Čas se automaticky znovu jej naplnit prostřednictvím perzistence \_ \_pole stavu zobrazení stránky se interpretovat modulem ASP.NET runtime při ovládacího prvku se znovu vykreslené na serveru. Kód prvku ASP.NET AJAX server nemůže rozpoznat, ve kterém jsou metody ovládací prvky změny stavu; ho jednoduše znovu naplní ze zobrazení stavu a poté spustí události, které jsou vhodné.

Je být zdůraznit, ale, že měl I inicializovat čas v rámci dané stránky\_zatížení událostí čas by byla zvýšena správně. V důsledku toho by vývojáři buďte opatrní, že příslušný kód je spuštěn při obslužné rutiny události a vyhněte se použití stránky\_načíst při obslužnou rutinu události ovládacího prvku může být vhodné.

## <a name="summary"></a>Souhrn

Ovládacího prvku ASP.NET AJAX rozšíření UpdatePanel je univerzální a může využívat několik metod pro identifikaci události ovládacího prvku, které by nemělo způsobit ho aktualizovat. Podporuje se automaticky aktualizován jeho podřízených ovládacích prvků, ale můžete také reakce na události ovládacího prvku jinde na stránce.

Chcete-li snížit potenciální pro zatížení serveru, je vhodné, `ChildrenAsTriggers` vlastnost UpdatePanel být nastavená na `false`, a že události se vyjádřit výslovný souhlas do místo zahrnuté ve výchozím nastavení. To zabrání také všechny nepotřebné události z potenciálně nežádoucí účinky, včetně ověření a změny vstupní pole. Tyto typy chyb může být obtížné izolovat, protože stránky aktualizace transparentně uživateli a nemusí proto být hned zjevné příčinu.

Prověřením vnitřního chodu formuláře ASP.NET AJAX konečná modelu zachycení, nebylo možné určit, že ho využívá rozhraní již poskytované ASP.NET. V to uděláte, se zachovává maximální kompatibility s ovládacími prvky určené pomocí stejné framework a intrudes minimálně na žádné další JavaScript napsané pro stránku.

## <a name="bio"></a>Bio

Rob Paveza je senior vývojář aplikace .NET v Terralever ([www.terralever.com](http://www.terralever.com)), počáteční interaktivní marketing firma v Tempe, AZ. Dosažitelný v [ robpaveza@gmail.com ](mailto:robpaveza@gmail.com), a se nachází v tomto blogu [ http://geekswithblogs.net/robp/ ](http://geekswithblogs.net/robp/).

Scott Dikovat pracuje s technologií Microsoft Web od 1997 a je ředitel myKB.com ([www.myKB.com](http://www.myKB.com)) kde mu se specializuje na psaní ASP.NET na základě aplikací, které jsou zaměřené na řešení softwaru znalostní báze Knowledge Base. Scott nelze kontaktovat prostřednictvím e-mailu v [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) nebo jeho blog na [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Předchozí](understanding-partial-page-updates-with-asp-net-ajax.md)
> [další](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
