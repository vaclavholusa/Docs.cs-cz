---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
title: Principy aktivačních událostí UpdatePanel technologie ASP.NET AJAX | Dokumentace Microsoftu
author: scottcate
description: Při práci v editoru kódu v sadě Visual Studio, můžete si všimnout (od IntelliSense), že jsou dva podřízené prvky prvku UpdatePanel. Jeden z co...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2008
ms.topic: article
ms.assetid: faab8503-2984-48a9-8a40-7728461abc50
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
msc.type: authoredcontent
ms.openlocfilehash: 180491b6aee188a9dca750d6d325217f1ad5212c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363718"
---
<a name="understanding-aspnet-ajax-updatepanel-triggers"></a>Principy aktivačních událostí UpdatePanel technologie ASP.NET AJAX
====================
podle [– Scott Cate](https://github.com/scottcate)

[Stáhnout PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial02_Triggers_cs.pdf)

> Při práci v editoru kódu v sadě Visual Studio, můžete si všimnout (od IntelliSense), že jsou dva podřízené prvky prvku UpdatePanel. Jedním z nich je element aktivačních událostí, který určuje ovládacích prvků na stránce (nebo uživatelský ovládací prvek, pokud použijete jeden), který se aktivuje částečné vykreslení ovládacího prvku UpdatePanel, ve kterém se nachází element.


## <a name="introduction"></a>Úvod

Technologie ASP.NET společnosti Microsoft přináší objektově orientované a založený na událostech programovací model a sjednotí s výhodami zkompilovaný kód. Svůj model zpracování na straně serveru má však několik nevýhody vyplývajících z technologie, z nichž mnohá vyřešíte tak, že nové funkce zahrnuté v rozšíření Microsoft ASP.NET 3.5 AJAX. Tato rozšíření povolit spoustu nových funkcí vzhled plně funkčního klienta, včetně částečného zobrazení stránek, bez nutnosti úplná aktualizace stránky, možnost přístupu k webovým službám prostřednictvím klientského skriptu (včetně ASP.NET, rozhraní API pro profilaci) a rozsáhlé rozhraní API na straně klienta navržené pro zrcadlení mnoho systémů ovládací prvek v sadě serverový ovládací prvek technologie ASP.NET.

Tento dokument White Paper zkoumá XML triggery funkce technologie ASP.NET AJAX `UpdatePanel` komponenty. Aktivační události XML poskytují detailní kontrolu nad součásti, které může způsobit částečného zobrazení pro konkrétní ovládací prvky UpdatePanel.

Tento dokument White Paper vychází Beta 2 verzi rozhraní .NET Framework 3.5 a Visual Studio 2008. Rozšíření ASP.NET AJAX, dříve sestavení doplňku určenou pro technologii ASP.NET 2.0, jsou teď integrovaná v knihovně tříd rozhraní .NET Framework Base. Tento dokument White Paper také předpokládá, že můžete pracovat s Visual Studio 2008, nikoli Visual Web Developer Express a poskytne návody podle uživatelského rozhraní sady Visual Studio (i když výpis kódu bude zcela kompatibilní, bez ohledu na to vývojové prostředí).

## <a name="triggers"></a>*Triggery*

Aktivační události pro daný prvek UpdatePanel, ve výchozím nastavení, automaticky zahrnulo všechny podřízené ovládací prvky, které vyvolávají zpětné volání, včetně (například) TextBox – ovládací prvky, které mají jejich `AutoPostBack` vlastnost nastavena na hodnotu **true**. Ale aktivačních událostí může být také součástí deklarativně pomocí značky To se provádí v rámci `<triggers>` části deklarace ovládacího prvku UpdatePanel. I když aktivační události je přístupná prostřednictvím `Triggers` vlastnost kolekce se doporučuje registraci žádné aktivační události částečné vykreslování v době běhu (například pokud ovládací prvek není k dispozici v době návrhu) s použitím `RegisterAsyncPostBackControl(Control)` metodu Stránky, v rámci objektu ScriptManager `Page_Load` událostí. Mějte na paměti, že stránky jsou bezstavové, a proto byste měli znovu zaregistrovat tyto ovládací prvky pokaždé, když jsou vytvořeny.

Zahrnutí podřízené automatické aktivační událost lze také zakázat (tak, aby podřízené ovládací prvky, které vytvářejí postbacků nespouštějí automaticky vykreslí částečné) tak, že nastavíte `ChildrenAsTriggers` vlastnost **false**. To vám umožní nejvyšší flexibilitu při přiřazování které konkrétní ovládací prvky mohou vyvolat vykreslení stránky a je doporučena, tak, aby vývojář se vyjádřit výslovný souhlas reagovat na události, namísto zpracování všech událostí, které mohou nastat.

Všimněte si, že když jsou vnořené ovládací prvky UpdatePanel, pokud UpdateMode nastavena na **podmíněného**, pokud se aktivuje podřízený prvek UpdatePanel, ale nadřazená položka nemá, pak pouze podřízené bude prvek UpdatePanel. Ale pokud se nadřazený prvek UpdatePanel je aktualizovat, pak podřízený prvek UpdatePanel bude také aktualizovat.

## <a name="the-lttriggersgt-element"></a>*&lt;Triggery&gt; – Element*

Při práci v editoru kódu v sadě Visual Studio, můžete si všimnout (od IntelliSense), že existují dva podřízené prvky ze `UpdatePanel` ovládacího prvku. Nejčastěji viděli prvek je `<ContentTemplate>` element, který zapouzdřuje v podstatě obsah, který se bude vysílat aktualizace panelem (obsah, u kterého jsme uvolnili částečného zobrazení). Druhý prvek je `<Triggers>` element, který určuje ovládacích prvků na stránce (nebo uživatelský ovládací prvek, pokud použijete jeden), který se aktivuje částečné vykreslení ovládacího prvku UpdatePanel, ve kterém &lt;aktivační události&gt; prvek nachází.

`<Triggers>` Element může obsahovat libovolný počet jednotlivých dva podřízené uzly: `<asp:AsyncPostBackTrigger>` a `<asp:PostBackTrigger>`. Oba dva atributy, přijměte `ControlID` a `EventName`a můžete zadat libovolný ovládací prvek v rámci aktuální jednotky zapouzdření (například pokud se nachází v rámci uživatelského ovládacího prvku webového ovládacího prvku UpdatePanel, byste se neměli pokoušet tak, aby odkazovaly na ovládací prvek Stránka, na kterém se nachází uživatelský ovládací prvek).

`<asp:AsyncPostBackTrigger>` Element je zvláště užitečná v tom, že ho můžete cílit na libovolnou událost z ovládacího prvku, který existuje jako podřízený objekt *jakékoli* ovládací prvek UpdatePanel v jednotce zapouzdření, nejen UpdatePanel, pod kterým je tento trigger podřízený . Díky tomu se libovolný ovládací prvek lze aktivovat aktualizaci částečná stránka.

Podobně platí `<asp:PostBackTrigger>` elementu lze použít aktivační událost vykreslování části stránky, ale ten, který vyžaduje úplné cesty k serveru. Tento prvek aktivační událost také umožňuje vynutit celou stránku vykreslování, pokud ovládací prvek jinak obvykle aktivuje vykreslování části stránky (například, když `Button` ovládací prvek existuje v `<ContentTemplate>` elementu ovládacího prvku UpdatePanel). PostBackTrigger element znovu, můžete zadat libovolný ovládací prvek, který je podřízeným prvkem libovolný ovládací prvek UpdatePanel v aktuální jednotce zapouzdření.

## <a name="lttriggersgt-element-reference"></a>*&lt;Aktivační události&gt; – referenční dokumentace elementu*

*Descendants značky:*

| **Značka** | **Popis** |
| --- | --- |
| &lt;asp:AsyncPostBackTrigger&gt; | Určuje ovládací prvek a událost, která způsobí, že částečná stránka aktualizace pro prvek UpdatePanel, který obsahuje odkaz na tento trigger. |
| &lt;asp:PostBackTrigger&gt; | Určuje ovládací prvek a událost, která způsobí, že aktualizace celou stránku (úplná aktualizace stránky). Toto klíčové slovo slouží k vynucení úplné aktualizace, když ovládací prvek aktivuje v opačném případě částečného zobrazení. |

## <a name="walkthrough-cross-updatepanel-triggers"></a>*Návod: Mezi ovládacím prvkem UpdatePanel triggery*

1. Vytvořte novou stránku ASP.NET pomocí objektu ScriptManager nastavit pro povolení částečného zobrazení. Přidat dvě komponenty UpdatePanel na tuto stránku – v prvním, zahrnují ovládací prvek popisku (Label1) a dva ovládací prvky tlačítka (ovládací prvky Button1 a Button2). Button1 by mělo být uvedeno klikněte na tlačítko Aktualizovat oba a Button2 by mělo být uvedeno klikněte na Aktualizovat to nebo něco spolu tyto řádky. V druhém ovládacím prvkem UpdatePanel obsahovat pouze ovládací prvek popisku (Label2), ale nastavit jeho vlastnosti ForeColor na jinou hodnotu než výchozí, aby ho odlišil.
2. Nastavte vlastnost UpdateMode obě značky ovládacího prvku UpdatePanel **podmíněného**.

**Výpis 1: Kód pro default.aspx:** 

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample1.aspx)]

1. V obslužné rutině události kliknutí pro Button1 nastavte Label1.Text i Label2.Text na něco závislá na čase (například DateTime.Now.ToLongTimeString()). Pro obslužné rutiny události kliknutí pro Button2 nastavte pouze Label1.Text hodnotě závislá na čase.

**Výpis 2: Codebehind (oříznut) v default.aspx.cs:** 

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample2.cs)]

1. Stisknutím klávesy F5 sestavte a spusťte projekt. Všimněte si, že po kliknutí na panelech jak aktualizace i popisky, změnit text; ale po kliknutí na tento Panel aktualizace pouze Label1 aktualizuje.


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image2.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image1.png)

([Kliknutím ji zobrazíte obrázek v plné velikosti](understanding-asp-net-ajax-updatepanel-triggers/_static/image3.png))


## <a name="under-the-hood"></a>*Pohled pod kapotu*

Využitím příkladu, který jsme právě vytvořen, můžeme provést podívat, co dělá technologie ASP.NET AJAX a jak pracují naše aktivačních událostí UpdatePanel mezi panely. K tomu, pak ve spolupráci s zdrojový kód vygenerovaný stránky HTML, jakož i rozšíření Mozilla Firefox volat FireBug - s, jsme snadněji zkontrolovat zpětná volání AJAX. Budeme také používat nástroj .NET Reflector podle Lutz Roeder. Oba tyto nástroje jsou volně k dispozici online a lze najít pomocí vyhledávání na Internetu.

Prošetření kódu ze zdrojového kódu stránky zobrazí téměř nic neobvyklého; ovládací prvky UpdatePanel jsou vykresleny jako `<div>` kontejnery a My můžete zobrazit skript prostředků obsahuje zadaný ve `<asp:ScriptManager>`. Existují také některé nové AJAX konkrétní volání do správce PageRequestManager, která jsou interní v klientské knihovně skriptu AJAX. A konečně, můžeme vidět dva kontejnery UpdatePanel – jeden u vygenerované `<input>` tlačítka s nimi `<asp:Label>` ovládací prvky se vykresluje jako `<span>` kontejnery. (Je-li si prohlédnout v FireBug stromové struktuře modelu DOM, můžete si všimnout, že popisky jsou neaktivní označíte, že nejsou vytváření viditelného obsahu).

Klikněte na tlačítko Aktualizovat tento Panel a Všimněte si, že hlavní prvek UpdatePanel aktualizuje aktuální čas serveru. V FireBug zvolte kartu konzoly tak, aby můžete zkontrolovat žádost. Nejprve zkontrolujte parametry požadavku POST:


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image5.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image4.png)

([Kliknutím ji zobrazíte obrázek v plné velikosti](understanding-asp-net-ajax-updatepanel-triggers/_static/image6.png))


Všimněte si, že prvku UpdatePanel uvedl do kódu jazyka AJAX na straně serveru přesně které ovládací prvek stromu se vyvolala prostřednictvím parametru ScriptManager1: `Button1` z `UpdatePanel1` ovládacího prvku. Nyní klikněte na tlačítko aktualizace i panelů. Potom zkoumání odpovědi, vidíme oddělených kanálu řadu proměnné nastavené v řetězci; Konkrétně můžeme vidět nejvyšší UpdatePanel `UpdatePanel1`, má podkladové jeho HTML odesláno prohlížeči. Skript klientské knihovny AJAX nahradí původní HTML prvku UpdatePanel obsahu s použitím nového obsahu prostřednictvím `.innerHTML` vlastnost, a proto server odešle změněný obsah ze serveru ve formátu HTML.

Nyní klikněte na tlačítko jak panelů aktualizace a podívejte se na výsledky ze serveru. Výsledky jsou velmi podobné – obě komponenty UpdatePanel příjem nových HTML ze serveru. Stejně jako u předchozí zpětné volání, je odeslána stav další stránky.

Jak vidíme, protože žádný zvláštní kód slouží k provádění zpětné volání AJAX, je schopna zachytit zpětného odeslání formuláře bez jakékoli další kód skriptu klientské knihovny AJAX. Serverové ovládací prvky automaticky využívat jazyka JavaScript tak, aby se neodešlou automaticky formuláře – ASP.NET automaticky vkládá kód pro ověřování formuláře a stav již, především dosahuje automatický skript prostředků zahrnutí PostBackOptions třídy a třídu ClientScriptManager.

Například vezměte v úvahu ovládacím prvku zaškrtávací políčko; Prozkoumejte zpětný překlad třídy v rozhraní .NET Reflector. Uděláte to tak, ujistěte se, že vaše sestavení System.Web otevřete a přejděte do `System.Web.UI.WebControls.CheckBox` třídy, otevřete `RenderInputTag` metody. Hledat s podmínkou, která zkontroluje, `AutoPostBack` vlastnost:


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image8.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image7.png)

([Kliknutím ji zobrazíte obrázek v plné velikosti](understanding-asp-net-ajax-updatepanel-triggers/_static/image9.png))


Pokud je povoleno automatické zpětného odeslání na `CheckBox` řízení (prostřednictvím AutoPostBack vlastnosti je true), výsledné `<input>` je tedy vykreslen pomocí technologie ASP.NET událost zpracování skriptu v jeho `onclick` atribut. Zachycení odeslání formuláře, pak umožňuje technologie ASP.NET AJAX vložit do stránky nonintrusively, a usnadnit tak vyhnuli potenciální rozbíjející změny, které mohou nastat s využitím pravděpodobně nepřesnou řetězec nahrazení. Kromě toho díky tomu *jakékoli* vlastní ovládací prvek ASP.NET využívat sílu technologie ASP.NET AJAX bez jakékoli další kód pro podporu jeho použití v rámci kontejneru prvku UpdatePanel.

`<triggers>` Funkce odpovídá hodnoty inicializován při volání funkce PageRequestManager \_updateControls (Všimněte si, že knihovna skriptu klienta ASP.NET AJAX používá tato konvence této metody, události a názvy polí, které začínají začínající podtržítkem jsou označeny jako vnitřní a nejsou určené pro použití mimo samotné knihovny). Pomocí něho můžete podle našich zkušeností které ovládací prvky jsou určeny k způsobit, že zpětná volání AJAX.

Na stránce, ponechat jeden ovládací prvek mimo komponenty UpdatePanel zcela a ponechat v rámci ovládacího prvku UpdatePanel jednoho například přidáme další dva ovládací prvky. Budeme přidat ovládací prvek v rámci horní UpdatePanel a vyřadit DropDownList s celou řadou barvám definovaným v rámci seznamu. Toto je nový zápis:

**Výpis 3: Nové značky**

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample3.aspx)]

A tady je nová kódu:

**Část 4: Codebehind**

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample4.cs)]

Myšlenkou Tato stránka je, že rozevíracího seznamu vybere jeden ze tří barvy zobrazíte druhého popisku, že zaškrtávací políčko určuje, jestli je tučný i Určuje, zda se zobrazí popisky datum i čas. Pole by nemělo způsobit aktualizaci AJAX, ale měli rozevíracího seznamu, i když není umístěna v rámci ovládacího prvku UpdatePanel.


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image11.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image10.png)

([Kliknutím ji zobrazíte obrázek v plné velikosti](understanding-asp-net-ajax-updatepanel-triggers/_static/image12.png))


Jako je zřejmý na snímku obrazovky výše, byla nejnovější tlačítko dá kliknout pravým tlačítkem tento Panel aktualizace, která aktualizuje hlavní dobu nezávisle na dolní čas. Datum se také vypnout mezi kliknutí, protože datum je viditelný v dolní popisek. A konečně zájmu je barva dolního popisku: později, než text popisku, který ukazuje, že stav ovládacího prvku je důležité, aktualizace a uživatelé očekávají, že bude zachována přes zpětná volání AJAX. *Ale*, čas nebyla aktualizována. Čas se automaticky naplnit prostřednictvím trvalou dostupnost \_ \_pole Stav zobrazení stránky zamezit interpretaci modulem ASP.NET runtime, když ovládací prvek se znovu vykreslený na serveru. Do kódu serveru technologie ASP.NET AJAX nebylo rozpoznáno, ve kterém jsou metody ovládací prvky změnu stavu; jednoduše znovu naplní ze zobrazení stavu a poté spuštění události, které jsou vhodné.

Je třeba zdůraznit, však, že došlo I inicializovat čas v rámci stránky\_události načtení času by byl navýšen správně. V důsledku toho by vývojáři dávejte pozor na to, že odpovídající kód je spuštěn během obslužné rutiny události a vyhněte se použití stránky\_načíst při obslužnou rutinu události ovládacího prvku může být vhodné.

## <a name="summary"></a>Souhrn

Ovládacího prvku UpdatePanel technologie ASP.NET AJAX rozšíření je flexibilní a můžete využít několik metod pro určení události ovládacích prvků, které byste ji aktualizovat. Podporuje se automaticky aktualizovat podle jemu podřízených ovládacích prvků, ale může také reagovat na události ovládacích prvků na stránce jinde.

Pokud chcete snížit riziko pro zatížení serveru, je doporučeno `ChildrenAsTriggers` vlastnost ovládacího prvku UpdatePanel nastavit na `false`, a události se rozhodli do spíše než zahrnuté ve výchozím nastavení. To zabrání také všechny nepotřebné události z potenciálně nežádoucí účinky, včetně ověření a změn na vstupní pole. Tyto druhy chyb může být obtížné určit, protože se stránka aktualizuje transparentně uživateli, a nemusí proto být hned zjevné příčiny.

Prozkoumáním vnitřní fungování formuláře technologie ASP.NET AJAX konečná modelu zachycení, jsme byli schopni zjistit, že využívá rozhraní již poskytovaných technologií ASP.NET. Přitom je zachová maximální kompatibility s ovládacími prvky navrženými pomocí stejné architektury a intrudes minimálně na jakékoli další JavaScript zapsán pro danou stránku.

## <a name="bio"></a>Životopis

Rob Paveza je vedoucí vývojář aplikace .NET v Terralever ([www.terralever.com](http://www.terralever.com)), přední interaktivní marketingové podniku v Tempe, AZ. Může být dosáhl v [ robpaveza@gmail.com ](mailto:robpaveza@gmail.com), a je umístěn na svém blogu [ http://geekswithblogs.net/robp/ ](http://geekswithblogs.net/robp/).

Scott Cate má práce s Microsoft webových technologiích od roku 1997 a je prezident myKB.com ([www.myKB.com](http://www.myKB.com)) kde mu se specializuje na technologie ASP.NET psaní aplikací, zaměřuje na znalostní báze softwarová řešení založených na. Scott můžete kontaktovat prostřednictvím e-mailové adrese [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) nebo na svém blogu [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Předchozí](understanding-partial-page-updates-with-asp-net-ajax.md)
> [další](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
