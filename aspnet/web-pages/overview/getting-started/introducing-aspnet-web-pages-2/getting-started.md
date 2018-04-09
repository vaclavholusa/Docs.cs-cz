---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
title: Představení technologie ASP.NET Web Pages – Začínáme | Microsoft Docs
author: tfitzmac
description: Služba WebMatrix se už doporučuje jako integrované vývojové prostředí pro ASP.NET Web Pages. Pomocí sady Visual Studio nebo Visual Studio Code. Tyto pokyny...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: a36d3bdf-ef1b-47a4-b932-3a0cf4cad716
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 5fd67a230f76774e102094f42426b8bb126c0cc6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="introducing-aspnet-web-pages---getting-started"></a>Představení technologie ASP.NET Web Pages – Začínáme
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > 
> > Služba WebMatrix se už doporučuje jako integrované vývojové prostředí pro ASP.NET Web Pages. Použití [sady Visual Studio](../program-asp-net-web-pages-in-visual-studio.md) nebo [Visual Studio Code](https://code.visualstudio.com/).
> 
> 
> Tento pokyny a aplikace získáte přehled o rozhraní ASP.NET Web Pages (verze 2 nebo novější) a syntaxi Razor, jednoduché rozhraní pro vytváření dynamických webů. Také zavádí WebMatrix, nástroj pro vytváření stránek a webů.
> 
> **Úroveň**: nové rozhraní ASP.NET Web Pages.  
> **Předpokládá, že znalosti**: HTML, základní kaskádových stylů (CSS).
> 
> Co se dozvíte z prvního kurzu sady:
> 
> - Jaké technologii ASP.NET Web Pages a co je pro.
> - Co je služba WebMatrix.
> - Jak nainstalovat vše.
> - Postup vytvoření webu pomocí služby WebMatrix.
>   
> 
> Funkce nebo technologie popsané:
> 
> - Webové platformy Microsoft.
> - Služba WebMatrix.
> - *.cshtml* stránky
>   
> 
> Jan Pope byl původně zapsán v tomto kurzu. Tní FitzMacken aktualizaci pro sadu Microsoft WebMatrix 3.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Rozhraní ASP.NET Web Pages (Razor) 2
> - Služba WebMatrix 3


## <a name="what-should-you-know"></a>Co byste vědět?

Nemůžeme se za předpokladu, že jste obeznámeni s:

- **HTML**. Bez důkladné znalosti je povinný. Objasníme nebude HTML, ale nemáte používáme nic komplexní. Poskytujeme odkazy na kurzy HTML, kde myslíme si, že jsou užitečná.
- **Kaskádových stylů (CSS)**. Stejné jako s jazykem HTML.
- **Databáze Basic nápady**. Pokud jste do tabulky se používá pro data a třídění a filtrování dat, který je úroveň znalosti jsme se obecně za předpokladu, že pro tento kurz sadu.

Jsme se také za předpokladu, že byste chtěli dozvědět základní programování. ASP.NET – webové stránky použijte programovací jazyk C#. Nemusíte mít žádné pozadí v programování, právě zájmu v ní. Pokud jste někdy napsali žádné JavaScript na webové stránce před, máte k dispozici dostatek pozadí.

Všimněte si, že pokud jste obeznámeni s programováním, můžete zjistit, že v tomto kurzu zpočátku nastavený přesune pomalu při můžeme uvést programátory urychlit. Jak se nám získat po první několik kurzy, ale bude menší základní programování vysvětlit, a věcí přesune na rychlejší klip.

## <a name="what-do-you-need"></a>Co je potřeba?

Tady je co budete potřebovat:

- Počítač se systémem Windows 8, Windows 7, Windows Server 2008 nebo Windows Server 2012.
- Aktivní internetové připojení.
- Oprávnění správce (požadováno pro instalaci).

## <a name="what-is-aspnet-web-pages"></a>Co je ASP.NET – webové stránky?

ASP.NET – webové stránky je rozhraní, které můžete vytvářet dynamické webové stránky. Jednoduché webové stránky HTML je statický; jeho obsah je určen podle pevné jazyka HTML, který je na stránce. Dynamických stránek jako ty, které vytvoříte pomocí webových stránek ASP.NET umožňují vytvářet obsah stránky za chodu pomocí kódu.

Dynamických stránek umožňují udělat nejrůznějším věcí. Můžete požádat uživatele o vstup pomocí formuláře a potom změnit na stránce zobrazuje nebo jak vypadá. Může trvat informace od uživatele, uložit do databáze a seznam později. Odesílat e-maily z vaší lokality. Můžete pracovat s jinými službami na webu (například služba mapování) a vytvořit stránky, které se integrují informace z těchto zdrojů.

## <a name="what-is-webmatrix"></a>Co je služba WebMatrix?

WebMatrix je nástroj, který se integruje do editoru webové stránky, nástroj databáze, webový server pro testování stránky a funkce pro publikování vašeho webu k Internetu. Služba WebMatrix je zdarma a je snadno nainstalovat a snadno se používá. (Také funguje jen prostý stránky HTML, a také pro jiné technologie, jako je PHP.)

Ve skutečnosti není *mít* použití služby WebMatrix pro práci s ASP.NET Web Pages. Můžete vytvářet stránky pomocí textového editoru, například a testovat stránky pomocí webového serveru, máte přístup. Však služba WebMatrix usnadňuje všechny velmi, tak tyto kurzy budou pomocí služby WebMatrix v průběhu.

## <a name="about-these-tutorials"></a>O tyto kurzy

Tento kurz sada je Úvod do používání rozhraní ASP.NET Web Pages. Celkový počet v této úvodní kurz sady jsou 9 kurzy. To je součástí série kurz sad cyklus od začátečníků webových stránek ASP.NET pro vytváření skutečné, profesionální weby.

Tato první kurz nastavit koncentráty ukazuje základní informace o tom, jak pracovat s ASP.NET Web Pages. Když jste hotovi, můžete pracovat s další kurz sad, které se vyzvedávat kde ukončení této a který další podrobněji prozkoumat webové stránky.

Jsme úmyslně přejděte snadno podrobné vysvětlení. A vždy, když jsme zobrazit něco, pro tento kurz sadu jsme vždy vyberte způsob, jakým Věříme, že se nejjednodušší pochopit. Novější kurz sady přejděte do větší hloubky a zobrazit je efektivnější a flexibilnější přístupy (také další fun). Ale tyto kurzy vyžadují, abyste nejdřív pochopit základy.

Kurz sadu, do které jste začali se vztahuje na tyto funkce z webových stránek ASP.NET:

- Úvod a získávání všechno, co je nainstalována. (Který je v tomto kurzu, které při čtení.)
- Základní informace o programování technologie ASP.NET Web Pages.
- Vytvoření databáze.
- Vytváření a zpracování formulář vstupu uživatele.
- Přidání, aktualizace a odstranění dat v databázi.

## <a name="what-will-you-create"></a>Co se vytvoří?

Nastavit tento kurz a následné ty, které jsou základem web, kde můžete vytvořit seznam filmy, které se vám líbí. Budete moct zadat filmy, upravovat a jejich seznam. Tady je několik stránek, které vytvoříte v tomto kurzu. První z nich uvádí film výpis stránky, která vytvoříte:

![Dokončila filmová aplikace zobrazuje v seznamu filmu](getting-started/_static/image1.png)

A zde je stránka, která umožňuje přidat nové informace film na váš web:

![Dokončení filmová aplikace stránkou Přidat filmu](getting-started/_static/image2.png)

Další kurz sady sestavení v tomto nastavte a přidejte další funkce, jako jsou nahrávání obrázky, když necháte osoby přihlásit, odesílání e-mailu a integrací s sociálních médií.

## <a name="see-this-app-running-on-azure"></a>Tato aplikace spuštěné v Azure

Chcete zobrazit dokončení web spuštěný jako živou webovou aplikaci? Úplnou verzi aplikace můžete nasadit ke svému účtu Azure jednoduše kliknutím na následující tlačítko.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebPagesMovies)

Potřebujete účet Azure k nasazení tohoto řešení do Azure. Pokud již účet nemáte, máte následující možnosti:

- [Zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -získáte kredity, můžete použít k vyzkoušení placených služeb Azure a i po jejich použití až můžete účet ponechat a používat bezplatné služby Azure.
- [Aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -vaše předplatné MSDN vám dává kredity každý měsíc, které můžete použít pro placené služby Azure.

## <a name="installing-everything"></a>Instalace vše

Všechno, co můžete nainstalovat pomocí instalačního programu webové platformy společnosti Microsoft. V důsledku toho nainstalovat Instalační program a použít jej k instalaci všechno ostatní.

Pomocí webových stránek, je nutné mít minimálně Windows XP s aktualizací SP3 nainstalována nebo Windows Server 2008 nebo novější.

Na [stránka Web Pages](../../../index.md) webu ASP.NET, klikněte na tlačítko **nainstalovat**.

![Zobrazuje webu ASP.NET Web &quot;instalace služby WebMatrix&quot; tlačítko](getting-started/_static/image3.png)

Jste vyzváni k přijmout licenční podmínky a prohlášení o ochraně osobních údajů před instalací služby WebMatrix.

![Přijměte termín k zahájení instalace](getting-started/_static/image4.png)

Klikněte na tlačítko **spustit** spusťte instalaci. (Pokud chcete uložit instalační program, klikněte na tlačítko **Uložit** a znovu spusťte instalační program ze složky, kam jste jej uložili.)

![](getting-started/_static/image5.png)

Instalace webové platformy se zobrazí, je připravena k instalaci služby WebMatrix. Klikněte na tlačítko **nainstalovat**.

![](getting-started/_static/image6.png)

Proces instalace obrázky na co se má nainstalovat do počítače a zahájí proces. V závislosti na tom, co přesně musí být nainstalovaná proces může trvat od chvíli několik minut. Vyberte **souhlasím** přijmout licenční podmínky.

## <a name="hello-webmatrix"></a>Hello, WebMatrix

Až skončíte, proces instalace můžete spustit službu WebMatrix. Pokud tomu tak není, v systému Windows, z **spustit** nabídky, spusťte **Microsoft WebMatrix**.

Při prvním spuštění služby WebMatrix, budete mít možnost se přihlásit do služby Microsoft Azure pomocí účtu Microsoft. Po přihlášení, obdržíte 10 bezplatných webových aplikací prostřednictvím Azure. Tyto aplikace bezplatných webových poskytnout vhodný způsob k testování aplikace. Pokud ještě nemáte účet Azure, ale máte předplatné MSDN, můžete [aktivovat výhody pro předplatné MSDN](https://www.windowsazure.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604). Jinak můžete vytvořit Bezplatný zkušební účet si během několika minut. Podrobnosti najdete v tématu [bezplatná zkušební verze Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

Nemáte nyní přihlásit a pokračujte v tomto kurzu. Není-li není nyní, bude stále mít možnost se přihlásit později. Poslední [tématu](publishing.md) v tomto kurzu řady vysvětluje postup nasazení webu do Azure, proto by potřeba Přihlaste se k dokončení tohoto tématu.

V tomto okamžiku buď přihlásit pomocí účtu Microsoft nebo vyberte **teď ne** v pravém dolním rohu.

![Přihlásit se](getting-started/_static/image7.png)

Pokud chcete začít, můžete vytvořit prázdný web a přidat stránku. Novější kurzu v této sadě budete přehrát s jedním z předdefinovaných webu šablony.

V okně start klikněte na **nový**.

![Služba WebMatrix úvodní obrazovky](getting-started/_static/image8.png)

Šablony jsou předem souborů a stránek pro různé typy webů. Pokud chcete zobrazit všechny šablony, které jsou k dispozici ve výchozím nastavení, vyberte možnost galerie šablon.

![Vyberte šablonu Galerie](getting-started/_static/image9.png)

V **rychlý Start** vyberte **prázdný web** z **ASP.NET** skupiny a název nové lokality "WebPagesMovies".

![Služba WebMatrix rychlý Start okno s vybraná šablona prázdný web](getting-started/_static/image10.png)

Klikněte na tlačítko **Další**.

Pokud jste se přihlásili k účtu Microsoft, budete mít možnost vytvoření webu v Azure. Na základě názvu vašeho webu, výchozí název **WebPagesMovies.azurewebsites.net** navržený; však vykřičník označuje, že tento název není k dispozici v systému Windows Azure. Pro jednoduchost, vyberte **přeskočit** obejít nyní vytváření webu v Azure. Dále v této série publikujeme webu do Azure.

![Vytvoření azure lokality](getting-started/_static/image11.png)

Služba WebMatrix vytvoří a otevře web:

![Otevřete nový web WebPagesMovies ve službě WebMatrix](getting-started/_static/image12.png)

V horní části je panel nástrojů Rychlý přístup a pásu karet. V dolní části vlevo, uvidíte modulu pro výběr pracovního prostoru kde přepínat mezi úlohy (**lokality**, **soubory**, **databáze**, **sestavy**). Na pravé straně se podokno obsahu pro editor a pro sestavy. A v dolní části se zobrazí příležitostně oznamovací pruh pro zprávy.

Další informace o službě WebMatrix a jeho funkce při procházení tyto kurzy dozvíte.

## <a name="creating-a-web-page"></a>Vytvoření webové stránky

Abyste se seznámili s WebMatrix a webové stránky ASP.NET, vytvoříte jednoduchou stránku.

V modulu pro výběr pracovního prostoru, vyberte **soubory** pracovního prostoru. Tento pracovní prostor umožňuje pracovat se soubory a složky. V levém podokně ukazuje soubor strukturu serveru. Změny pásu karet zobrazíte úloh souvisejících s souboru.

![Pracovní prostor souboru ve službě WebMatrix](getting-started/_static/image13.png)

Na pásu karet klikněte na šipku v části **nový** a pak klikněte na **nový soubor**.

![Pomocí &quot;nový&quot; příkaz na pásu karet k vytvoření nového souboru](getting-started/_static/image14.png)

Služba WebMatrix zobrazí seznam typů souborů. Vyberte **CSHTML**a v **název** zadejte "HelloWorld". Na stránce CSHTML představuje stránku ASP.NET Web Pages.

![Vytváří se nová stránka CSHTML s názvem HelloWorld.cshtml](getting-started/_static/image15.png)

Click **OK**.

Služba WebMatrix stránky vytvoří a otevře v editoru.

![Novou stránku Hello World v editoru WebMatrix](getting-started/_static/image16.png)

Jak vidíte, tato stránka obsahuje většinou obyčejnou kódování HTML, s výjimkou blok v horní části, které vypadá takto:

[!code-cshtml[Main](getting-started/samples/sample1.cshtml)]

To je pro přidání kódu, jak se krátce zobrazí.

Všimněte si, že různé části stránky &mdash; názvy elementů, atributy a text a bloku v horní části – jsou všechny v různých barev. To se označuje jako *zvýraznění syntaxe*, a umožňuje jednodušší, aby vše jasné. Je jedna z funkcí, které usnadňuje práci s webovými stránkami ve službě WebMatrix.

Přidat obsah `<head>` a `<body>` prvky, jako v následujícím příkladu. (Pokud chcete, můžete právě zkopírujte následující blok a celý existující stránku nahraďte tento kód.)

[!code-cshtml[Main](getting-started/samples/sample2.cshtml)]

V panelu nástrojů Rychlý přístup nebo v **soubor** nabídky, klikněte na tlačítko **Uložit**.

![Tlačítko Uložit ve službě WebMatrix nástrojů Rychlý přístup](getting-started/_static/image17.png)

## <a name="testing-the-page"></a>Testování stránky

V **soubory** pracovního prostoru, klikněte pravým tlačítkem myši *HelloWorld.cshtml* a pak klikněte na tlačítko **spustit v prohlížeči**.

![Spuštění stránky pomocí služby WebMatrix pásu karet tlačítko Spustit](getting-started/_static/image18.png)

Služba WebMatrix spustí předdefinované webový server (IIS Express), který můžete použít k testování stránek ve vašem počítači. (Bez služby IIS Express ve službě WebMatrix, museli byste publikovat na webový server stránku někde předtím, než může otestovat ji.) Stránce se zobrazí ve výchozím prohlížeči.

![&quot;Hello World&quot; stránky, které jsou spuštěné v prohlížeči](getting-started/_static/image19.png)

Všimněte si, že při testování stránky ve službě WebMatrix, adresu URL v prohlížeči je podobný `http://localhost:33651/HelloWorld.cshtml.` název *localhost* odkazuje na místním serveru, což znamená, že stránce obsloužených webový server, který je ve vašem počítači. Jak jsme uvedli, služba WebMatrix obsahuje program webového serveru s názvem služby IIS Express, která se spouští při spuštění stránky.

Počet po *localhost* (například *localhost:33651*) odkazuje *číslo portu* ve vašem počítači. Toto je počet "kanál" používající službu IIS Express pro tento konkrétní web. Číslo portu je vybrán náhodně z rozsahu 1024 až 65536, při vytváření webu a je jiný pro každý web, který vytvoříte. (Při testování vlastního webu, číslo portu skoro určitě bude jinou hodnotu než 33561.) Když použijete jiný port pro každý web, IIS Express můžete ponechat přímých který stránek je rozhovoru s.

Novější při publikování webu veřejné webový server, se již nezobrazují *localhost* v adrese URL. V tomto bodě se zobrazí více klasickou adresu URL jako `http://myhostingsite/mywebsite/HelloWorld.cshtml` nebo bez ohledu na stránce. Dozvíte více o publikování lokality později z této série kurzu.

## <a name="adding-some-server-side-code"></a>Přidání některé kódu na straně serveru

Zavřete prohlížeč a přejděte zpět na stránku ve službě WebMatrix.

Přidá řádek do bloku kódu tak, aby vypadal jako následující kód:

[!code-cshtml[Main](getting-started/samples/sample3.cshtml)]

Toto je jenom trocha kódu Razor. Je nejspíš jasné, že získá aktuální datum a čas a uloží tuto hodnotu do *proměnná* s názvem `currentDateTime`. Další informace o syntaxi Razor v dalším kurzu.

V těle stránce po `<p>Hello World!</p>` elementu, přidejte následující:

[!code-html[Main](getting-started/samples/sample4.html)]

Tento kód získá hodnotu, která vložíte do `currentDateTime` proměnné v horní části a vloží je do kódu stránky. `@` Znak označí kód webové stránky ASP.NET na stránce.

Spuštění stránky znovu (WebMatrix uloží změny, můžete před spuštěním stránce). Tentokrát uvidíte datum a čas na stránce.

![&quot;Hello World&quot; spuštěná v prohlížeči s dynamicky generovaném času zobrazení stránky](getting-started/_static/image20.png)

Chvíli počkejte a pak aktualizujte stránku v prohlížeči. Datum a čas zobrazení se aktualizuje.

V prohlížeči podívejte se na stránce zdroj. To vypadá následující kód:

[!code-html[Main](getting-started/samples/sample5.html)]

Všimněte si, že `@{ }` bloku v horní části nejsou. Také Všimněte si, že datum a čas zobrazení zobrazuje skutečné řetězec znaků (`1/18/2012 2:49:50 PM` nebo jakoukoli), nikoli `@currentDateTime` stejně, jako jste měli *.cshtml* stránky. Co se stalo, zde je, že při spuštění stránky ASP.NET zpracovány všechny kód (velmi málo v tomto případě), byla označena `@`. Kód vytvoří výstup a tento výstup byl vložen do stránky.

## <a name="this-is-what-aspnet-web-pages-are-about"></a>Toto je ASP.NET – webové stránky jsou o

Při čtení, rozhraní ASP.NET Web Pages vytváří dynamického webového obsahu, co jste se seznámili s tady je na nápad. Stránka, kterou jste právě vytvořili obsahuje stejné jazyka HTML, který jste se seznámili s před. Může také obsahovat kód, který může provádět nejrůznějším úlohy. V tomto příkladu ho nebyla jednoduchý úkol získat aktuální datum a čas. Jak už jste viděli, můžete intersperse kódu HTML k vytváření výstupu na stránce. Když někdo požadavky *.cshtml* stránku v prohlížeči, ASP.NET zpracovává stránky, když je stále v rukou webového serveru. ASP.NET vloží výstup kódu (pokud existuje) na stránce ve formátu HTML. Po dokončení zpracování kódu, ASP.NET výsledná stránka odešle do prohlížeče. Všechny prohlížeče někdy získá jsou ve formátu HTML. Zde je diagram:

![Koncepční toku jak ASP.NET dynamicky vygeneruje HTML](getting-started/_static/image21.png)

Cílem je jednoduchý, ale řadu zajímavé úkolů, které může provádět kód a existuje mnoho způsobů zajímavé, ve kterých můžete dynamicky přidat obsah HTML na stránku. A ASP.NET *.cshtml* stránky, jako jakoukoli stránku HTML, můžou taky patřit kód, který běží v (kód JavaScript a jQuery) přímo z prohlížeče. Budete prozkoumejte všechny tyto věci v této sadě kurz a následné ty.

## <a name="coming-up-next"></a>Objevuje další

V dalším kurzu této série prozkoumejte další programování technologie ASP.NET Web Pages.

## <a name="additional-resources"></a>Další prostředky

[Vytvoření webu ASP.NET od začátku](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch). Kurz, který je konkrétně se jedná o pomocí služby WebMatrix (ne webové stránky ASP.NET). Přejde do trochu další podrobnosti o některých dalších funkcí služby WebMatrix, který nebude nabídneme v tomto kurzu.

> [!div class="step-by-step"]
> [Next](intro-to-web-pages-programming.md)
