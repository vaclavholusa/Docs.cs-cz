---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-cs
title: "Předkompilace webu (C#) | Microsoft Docs"
author: rick-anderson
description: "Visual Studio nabízí vývojářům ASP.NET dva typy projektů: projekty webových aplikací (WAP) a webových projektů (WSPs). Jeden z hlavní rozdíly betwe..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: ecd5a4de-beb7-4d1d-bbbb-e31003633267
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-cs
msc.type: authoredcontent
ms.openlocfilehash: 094bcbd2ccebeeed1f620476b6bd6df67047562f
ms.sourcegitcommit: c07fb5cb5df0a12f9fe6735fcbc90964608fa687
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/14/2017
---
<a name="precompiling-your-website-c"></a>Předkompilace webu (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_15_CS.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial15_Precompiling_cs.pdf)

> Visual Studio nabízí vývojářům ASP.NET dva typy projektů: projekty webových aplikací (WAP) a webových projektů (WSPs). Jedním z hlavní rozdílů mezi dva typy projektů je, že WAP musí mít kód explicitně kompilovat před nasazením, zatímco kód WSP můžete automaticky zkompilován na webovém serveru. Je však možné předkompilovat WSP před jejich nasazením. V tomto kurzu jsou zde popsány výhody předkompilaci a ukazuje, jak předkompilovat webovou stránku z Visual Studia a z příkazového řádku.


## <a name="introduction"></a>Úvod

Visual Studio nabízí vývojářům ASP.NET dva různé typy projektů: projekty webových aplikací (WAP) a projekty webového serveru (WSP). Jedním z hlavní rozdílů mezi tyto typy projektů je WAP vyžadují, aby se *explicitní kompilace* zatímco používat WSPs *automatickou kompilaci*, ve výchozím nastavení. S WAP, kompilaci kódu webové aplikace do jednoho sestavení, která je vytvořena na webu `Bin` složky. Nasazení zahrnuje kopírování obsahu značek ( `.aspx.ascx`, a `.master` souborů) v projektu, společně s sestavení v `Bin` složky; modelu code-behind není nutné nasadit soubory třídy sami. Na druhé straně nasadíte WSPs zkopírováním stránky značek a jejich odpovídající třídy kódu do produkčního prostředí. Třídy kódu kompilovány na vyžádání na webovém serveru.

> [!NOTE]
> Zpět naleznete v části "Explicitní kompilace Versus automatickou kompilaci" v [ *určení co soubory musí být nasazeny* kurzu](determining-what-files-need-to-be-deployed-cs.md) pro další informace o rozdílech mezi projektu modely, explicitní a automatické kompilace a jak ovlivňuje kompilace modelu nasazení.


Možnost automatické kompilace je snadno použitelný. Neexistuje žádné explicitní kompilační krok a upravovat jenom soubory, které byly třeba k nasazení, zatímco explicitní kompilace vyžaduje nasazení změněné značek stránek a sestavení právě zkompilovat. Automatické nasazení však má dva potenciální nedostatky:

- Protože stránky musí být zkompilovány automaticky, když jsou nejprve navštívili, může být krátký ale významnému zpoždění vyžádání stránky ASP.NET po nasazení poprvé.
- Automatickou kompilaci vyžaduje, aby obě deklarativní značkami a zdrojový kód přítomen na webovém serveru. Může to být rozdělení nežádoucí možnost, pokud plánujete prodávané webové aplikace pro zákazníky, kteří se nainstaluje na své webové servery.

Pokud se dva výše nedostatků jsou moduly pozornosti dělení, můžete buď přepnout do modelu WAP nebo *předkompilovat* WSP před jejich nasazením. V tomto kurzu prozkoumá možnosti předkompilace nejvhodnější pro hostované webu a provede proces předkompilace a nasazení předkompilovaných webu.

## <a name="an-overview-of-aspnet-code-generation-and-compilation"></a>Přehled vytváření a kompilování kódu ASP.NET

Předtím, než se podíváme na dostupné možnosti předkompilace, můžeme nejprve mluvit o generování kódu a kompilace, která nastane, když stránky ASP.NET je požadován poprvé, protože byly vytvořeny nebo poslední aktualizace. Jak víte, stránek ASP.NET se skládají ze dvou částí: deklarativní v `.aspx` souboru a část zdrojového kódu, obvykle v souboru třídy samostatné kódu (`.aspx.cs`). Kroků prováděných modulem runtime, pokud se požaduje stránku ASP.NET závisí na model kompilace aplikace.

S WAP musí být na stránkách zdrojového kódu explicitně zkompilovány do jednoho sestavení před nasazením. Během nasazení toto sestavení a značek stránky se zkopírují do produkčního prostředí. Pokud dorazí požadavek na webový server pro stránku ASP.NET, modul runtime vytvoří instanci třídy kódu stránky a vyvolá jeho `ProcessRequest` metoda, která spustí životního cyklu stránky a nakonec, generuje stránky obsahu, který je vrácen do žadatel. Modul runtime můžete pracovat s třídy kódu stránky ASP.NET, protože třída kódu byl již zkompilovány do sestavení před nasazením.

WSPs a automatickou kompilaci neexistuje žádné explicitní kompilační krok před nasazením. Místo toho nasazení zahrnuje kopírování deklarativního i zdrojový kód obsah do produkčního prostředí. Když přijat požadavek na webový server pro stránku ASP.NET poprvé od stránce vytvořila nebo poslední aktualizace, modul runtime musí napřed zkompilovat třídě kódu do sestavení. Toto kompilované sestavení je uložen ve složce `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`, i když umístění této složky lze přizpůsobit pomocí `<compilation tempDirectory="" />` element `<system.web>`, obvykle v `Web.config`. Protože sestavení je uložen na disk, nemusí být znovu kompilován na následné žádosti na stejné stránce.

> [!NOTE]
> Jak jste zvyklí, dojde k mírnému zpoždění požadavku na stránku poprvé (nebo poprvé vzhledem k tomu, že byla změněna) v lokalitě, která používá automatickou kompilaci, jak dlouho trvá chvíli pro server pro kompilaci kódu stránky a uložit výsledné sestavení do disk.


Stručně řečeno s explicitní kompilace musíte kompilace zdrojového kódu webu před nasazením, ukládání modulu runtime odpadne k provedení tohoto kroku. Modul runtime zpracovává s automatickou kompilaci kompilace zdrojového kódu na stránkách, ale s náklady mírné inicializace pro první návštěvě stránce vzhledem k tomu, že byl vytvořen nebo poslední aktualizace.

Ale co informace o deklarativní části stránek ASP.NET ( `.aspx` souboru)? Je zřejmé, že je vztah mezi `.aspx` souborů a kódu v jejich kódu třídy, jako ovládací prvky webového definované v deklarativní jsou dostupné v kódu. Je zřejmé, obsah `.aspx` soubory výrazně ovlivňuje vykreslované značky generované stránky. Jak se text, HTML, funguje modulu runtime a syntaxe ovládacího prvku webového definované v `.aspx` soubor ke generování k požadované stránce je vykreslen obsahu?

Nechci se příliš nenechejte na podrobnosti implementace nízké úrovně, které se liší od WSPs WAP, ale stručně řečeno modulu runtime automaticky vygeneruje soubor třídy, který obsahuje různé ovládací prvky webového jako chráněné členy a metody. Tento soubor generovaný je implementovaný jako *třídu* k třídě odpovídající kódu. ([Částečné třídy](http://www.dotnet-guide.com/partialclasses.html) povolit pro obsah jedné třídy možné rozdělit do několika souborů.) Proto je třída kódu definovaná na dvou místech: v `.aspx.cs` soubor, který vytvoříte a v této třídě automaticky generovaný vytvořili modulem runtime. Tato třída automaticky generovaný je uložen v `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` složky.

Důležité ulici, zde je, že pro stránku ASP.NET k vykreslení modulem runtime i jeho deklarativní a zdrojový kód části musí být zkompilovány do sestavení. S WAP zdrojový kód je explicitně zkompilovat do sestavení před nasazením, ale deklarativní je stále nutné převést do kódu a zkompilují modul runtime na webovém serveru. Se WSPs pomocí automatickou kompilaci zdrojový kód a deklarativní musí být zkompilovány webový server.

Je možné použít explicitní kompilace s modelem WSP. Části zdrojového kódu explicitně jako kompilovat s modelem WAP. Navíc můžete také zkompilovat deklarativní.

## <a name="precompilation-options"></a>Možnosti předkompilace

Rozhraní .NET Framework se dodává s [nástroje kompilace technologie ASP.NET (`aspnet_compiler.exe`)](https://msdn.microsoft.com/en-us/library/ms229863.aspx) , umožňuje kompilovat zdrojový kód (a i obsah) vytvořené pomocí modelu WSP aplikace ASP.NET. Tento nástroj byla vydána v rozhraní .NET Framework verze 2.0 a je umístěn ve `%WINDIR%\Microsoft.NET\Framework\v2.0.50727` složky; můžete použít z příkazového řádku nebo spustí z Visual Studia prostřednictvím v nabídce sestavení možnost Publikovat Web.

Kompilace nástroj poskytuje dvě obecné formy kompilace: předkompilaci a předkompilaci pro nasazení na místě. S místní předkompilaci spustíte `aspnet_compiler.exe` nástroj z příkazového řádku a zadejte cestu k virtuálnímu adresáři nebo fyzická cesta webu, který se nachází ve vašem počítači. Nástroj kompilace pak zkompiluje každou stránku ASP.NET v projektu ukládání kompilované verze v `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` složky stejně jako Pokud stránky měl každý byla navštívené poprvé z prohlížeče. Předkompilace v místě urychlit první požadavek provést na nově nasazené stránky ASP.NET na váš web, protože ho nebude runtime z museli provést tento krok. Předkompilace v místě však není užitečná pro většinu hostované weby, protože vyžaduje, že budete moci spouštět programy z webového serveru příkazového řádku. Ve sdíleném hostování prostředí není povolena, tato úroveň přístupu.

> [!NOTE]
> Další informace o předkompilaci v místě, podívejte se na [postupy: předkompilovat weby technologie ASP.NET](https://msdn.microsoft.com/en-us/library/ms227972.aspx) a [předkompilaci v technologii ASP.NET 2.0](http://www.odetocode.com/Articles/417.aspx).


Místo kompilace stránky na webu na `Temporary ASP.NET Files` složce zkompiluje předkompilaci pro nasazení na stránkách k adresáři dle vlastního výběru a to ve formátu, který lze nasadit do produkčního prostředí.

Existují dva typy předkompilaci pro nasazení, které nám prozkoumat v tomto kurzu: předkompilace aktualizovat uživatelské rozhraní a předkompilace aktualizovatelné uživatelské rozhraní. Deklarativní v opustí předkompilaci s aktualizovat uživatelské rozhraní `.aspx`, `.ascx`, a `.master` soubory, a tím umožní vývojář k zobrazení a v případě potřeby upravit deklarativní na provozním serveru. Generuje předkompilaci s aktualizovatelné uživatelské rozhraní `.aspx` stránek, které jsou void veškerý obsah a odebere `.ascx` a `.master` soubory, a tím skrytí deklarativní a zakazují vývojář změna z produkčního prostředí.

### <a name="precompiling-for-deployment-with-an-updatable-user-interface"></a>Předkompilace pro nasazení s aktualizovat uživatelské rozhraní

Nejlepší způsob, jak porozumět předkompilaci pro nasazení je příklad v akci. Pojďme předkompilovat WSP recenze adresáře pro nasazení pomocí aktualizovat uživatelské rozhraní. Nástroj kompilace technologie ASP.NET můžete vyvolat z nabídky sestavení sady Visual Studio nebo pomocí příkazového řádku. Tento oddíl se zabývá pomocí nástroje z Visual Studia; v části "předkompilace z příkazového řádku" zjistí spuštěním nástroje kompilátoru z příkazového řádku.

Otevřete WSP zkontrolujte adresáře v sadě Visual Studio, přejděte do nabídky sestavení a vyberte možnost nabídky Publikovat Web. Spustí se dialogové okno Publikovat web (najdete v části **obrázek 1**), kde můžete určit cílové umístění, zda předkompilovaných webů uživatelské rozhraní je aktualizovat a další možnosti nástroje kompilátoru. Cílové umístění může být na vzdáleném webovém serveru nebo FTP server, ale prozatím zvolte složky na pevném disku. Vzhledem k tomu, že chceme předkompilovat s aktualizovat uživatelské rozhraní, nechte zaškrtnuté políčko "Povolit tento předkompilovaných webů aktualizovat" a klikněte na tlačítko OK.

[![](precompiling-your-website-cs/_static/image2.png)](precompiling-your-website-cs/_static/image1.png)

**Obrázek 1**: nástroje kompilace technologie ASP.NET se předkompilovat webu do zadané cílové umístění  
 ([Kliknutím zobrazit obrázek v plné velikosti](precompiling-your-website-cs/_static/image3.png))

> [!NOTE]
> V nabídce sestavení možnost publikovat web není k dispozici v aplikaci Visual Web Developer. Pokud používáte Visual Web Developer, budete muset používat verzi nástroje kompilace technologie ASP.NET, která je popsaná v části "předkompilace z příkazového řádku".


Po předkompilace web, přejděte do cílového umístění, které jste zadali v dialogovém okně Publikovat Web. Porovnání obsahu tuto složku s obsahem webu chvíli trvat. **Obrázek 2** ukazuje složky recenze adresáře webu. Poznámka: obsahuje oba `.aspx` a `.aspx.cs` soubory. Rovněž Pamatujte, že `Bin` directory obsahuje pouze jeden soubor `Elmah.dll`, které jsme přidali v [předchozí kurzu](logging-error-details-with-elmah-cs.md)

[![](precompiling-your-website-cs/_static/image5.png)](precompiling-your-website-cs/_static/image4.png)

**Obrázek 2**: adresář projektu obsahuje `.aspx` a `.aspx.cs` soubory; `Bin` složka obsahuje právě`Elmah.dll`  
 ([Kliknutím zobrazit obrázek v plné velikosti](precompiling-your-website-cs/_static/image6.png))

**Obrázek 3** ukazuje cílové umístění složky, jejichž obsah se vytvořily ve nástroje kompilace technologie ASP.NET. Tato složka neobsahuje žádné soubory kódu. Kromě toho tato složka `Bin` directory zahrnuje několik sestavení a dvě `.compiled` soubory kromě `Elmah.dll` sestavení.

[![](precompiling-your-website-cs/_static/image8.png)](precompiling-your-website-cs/_static/image7.png)

**Obrázek 3**: cílové umístění složky pro nasazení obsahuje soubory  
 ([Kliknutím zobrazit obrázek v plné velikosti](precompiling-your-website-cs/_static/image9.png))

Na rozdíl od explicitní kompilace v WAP nevytváří předkompilaci pro proces nasazení jednoho sestavení pro celou lokalitu. Místo toho ji dávek společně několik stránek do každé sestavení. Také zkompiluje `Global.asax` do vlastního sestavení a také všechny třídy v souboru (pokud existuje) `App_Code` složky. Soubory, které mají deklarativní pro technologii ASP.NET webové stránky a uživatelské ovládací prvky, hlavní stránky (`.aspx`, `.ascx`, a `.master` souborů, v uvedeném pořadí) jsou kopírovány jako-je do cílového umístění adresáře. Podobně `Web.config` soubor zkopírován přes, společně s všechny statické soubory, například bitové kopie, tříd CSS a soubory PDF. Více formální popis toho, jak Nástroj kompilace zpracovává různé typy souborů, použijte [souboru zpracování během ASP.NET předkompilaci](https://msdn.microsoft.com/en-us/library/e22s60h9.aspx).

> [!NOTE]
> Můžete určit, aby nástroj kompilace vytvořit jedno sestavení na stránky ASP.NET, uživatelský ovládací prvek nebo stránky předlohy zaškrtnutím políčka "Používá fixní pojmenování a jednostránkové sestavení" z dialogového okna Publikovat Web. S každou stránku ASP.NET kompilovat do vlastního sestavení umožňuje jemně odstupňovanou kontrolu nad nasazení. Například pokud aktualizovat jednu webovou stránku ASP.NET a potřebné pro nasazení tato změna, můžete potřebovat pouze nasazení této stránce `.aspx` souborů a přidružené sestavení do produkčního prostředí. Poraďte se [postupy: generování pevných názvů nástrojem kompilace ASP.NET](https://msdn.microsoft.com/en-us/library/ms228040.aspx) Další informace.


Cílový adresář umístění také obsahuje soubor, který nebyl součástí předkompilovaných webový projekt, a to `PrecompiledApp.config`. Tento soubor informuje modulem runtime ASP.NET, aby byl předkompilované aplikace a jestli byl předkompilovaných aktualizovat nebo poledne aktualizovat v uživatelském rozhraní.

Nakonec za chvíli otevřít některou z `.aspx` soubory v cílovém umístění pomocí sady Visual Studio nebo textový editor výběru. Při předkompilaci pro nasazení s aktualizovat uživatelské rozhraní, stránky ASP.NET v adresáři cílové umístění obsahovat kód přesný stejné jako odpovídající soubory na webu.

### <a name="precompiling-for-deployment-with-a-non-updatable-user-interface"></a>Předkompilace pro nasazení s aktualizovatelné uživatelské rozhraní

Nástroj ASP.NET kompilátoru lze také předkompilovat pro nasazení s aktualizovatelné uživatelského rozhraní. Předkompilace webu v aktualizovatelné uživatelském rozhraní funguje podobně jako předkompilace s aktualizovat rozhraní, klíčovým rozdílem, že stránek ASP.NET, uživatelských ovládacích prvků a hlavní stránky v cílovém adresáři se odstraní a z jejich kódu. Předkompilovat web pro nasazení s aktualizovatelné uživatelského rozhraní, vyberte v nabídce sestavení možnost publikovat web, ale zrušte zaškrtnutí políčka "Povolit tento předkompilovaných webů aktualizovat" (viz **obrázek 4**).

[![](precompiling-your-website-cs/_static/image11.png)](precompiling-your-website-cs/_static/image10.png)

**Obrázek 4**: zrušte zaškrtnutí políčka "Povolit tento předkompilovaných webů aktualizovat" možnost k předkompilovat s aktualizovat bez uživatelského rozhraní  
 ([Kliknutím zobrazit obrázek v plné velikosti](precompiling-your-website-cs/_static/image12.png))

**Obrázek 5** ukazuje cílové umístění složky po předkompilace aktualizovatelné uživatelského rozhraní.

[![](precompiling-your-website-cs/_static/image14.png)](precompiling-your-website-cs/_static/image13.png)

**Obrázek 5**: cílové umístění složky pro nasazení s aktualizovatelné uživatelského rozhraní  
 ([Kliknutím zobrazit obrázek v plné velikosti](precompiling-your-website-cs/_static/image15.png))

Porovnání **obrázek 3** k **obrázek 5**. Při dvě složky může vypadají stejně, Všimněte si, že chybí aktualizovatelné složky uživatelského rozhraní stránky předlohy `Site.master`. A při **obrázek 5** zahrnuje různé stránky ASP.NET, je-li zobrazit obsah těchto souborů uvidíte, že jste se odstraní z jejich deklarativní kódu a nahradí zástupný text: "Toto je soubor značky generované Nástroj předkompilace a nesmí být odstraněna! "

[![](precompiling-your-website-cs/_static/image17.png)](precompiling-your-website-cs/_static/image16.png)

**Obrázek 5**: deklarativní byla odebrána ze stránky ASP.NET

`Bin` Složek v **obrázky 3** a **5** více podstatně liší. Kromě sestavení `Bin` složky v **obrázek 5** zahrnuje `.compiled` soubor pro každou stránku ASP.NET, uživatelský ovládací prvek a stránky předlohy.

Předkompilace web s aktualizovatelné uživatelského rozhraní je užitečné v situacích, kde nechcete, aby obsah na stránkách ASP.NET a upravit osobě či společnosti, který nainstaluje nebo spravuje webu v provozním prostředí. Pokud vytvoříte webovou aplikaci ASP.NET, který prodeje zákazníkům nainstalovat na své vlastní webové servery, je vhodné zajistit, že neupravujte vzhledu a chování vašeho webu přímou úpravou `.aspx` stránky dodáte je. Podle předkompilace vašeho webu v aktualizovatelné uživatelském rozhraní, dodávat zástupného textu `.aspx` stránky jako součást instalace, že se vaši zákazníci z zkoumání nebo úpravy jejich obsahu.

### <a name="precompiling-from-the-command-line"></a>Předkompilace z příkazového řádku

Na pozadí, vyvolá dialogové okno publikování webu Visual Studio nástroje kompilace technologie ASP.NET (`aspnet_compiler.exe`) předkompilovat webu. Alternativně můžete vyvolat tento nástroj z příkazového řádku. Ve skutečnosti Pokud používáte Visual Web Developer pak musíte spustit nástroj kompilátoru z příkazového řádku, jako nabídkou sestavit aplikaci Visual Web Developer neobsahuje možnost Publikovat Web.

Chcete-li použít nástroj kompilátoru z příkazového řádku, spusťte odstranit na příkazový řádek a přejdete k adresáři framework `%WINDIR%\Microsoft.NET\Framework\v2.0.50727`. Potom do příkazového řádku zadejte následující příkaz:

`aspnet_compiler -p "physical_path_to_app" -v / -f -u "target_location_folder"`

Výše uvedeném příkazu spustí nástroj kompilátoru ASP.NET (`aspnet_compiler.exe`) a přes `-p` přepnout, dá pokyn ji předkompilovat web root na *fyzické\_cesta\_k\_aplikace*; Tato hodnota bude podobný `C:\MySites\BookReviews`a musí být v uvozovkách.

`-v` Přepínač určuje virtuální adresář webu. Pokud je server zaregistrován jako výchozí web v metabázi služby IIS, pak můžete vynechat `-p` přepínače a právě zadejte virtuální adresář aplikace. Pokud použijete `-p` přepnout, hodnota budete pokračovat `-v` přepínač označuje kořenovém adresáři webu a slouží k přeložení odkazů na kořenový adresář aplikace. Například pokud zadáte hodnotu `-v /MySite` pak odkazuje v aplikaci `~/path/file` bude vyřešen jako `~/MySite/path/file`. Protože lokality recenze adresáře se nachází v kořenovém adresáři na můj webového hostingu společnosti I použili přepínač `-v /`.

`-f` Přepínač, pokud existuje, doporučuje Nástroj kompilace přepsat *cíl\_umístění\_složky* adresáře, pokud již existuje. V případě vynechání `-f` přepínač a cílové umístění složce již existuje, bude Nástroj kompilace skončit s chybou: "Chyba ASPRUNTIME: cílový adresář není prázdný. "Odstraňte jej ručně nebo vyberte jiný cíl."

`-u` Přepínače, pokud existuje, informuje nástroj pro vytvoření aktualizovat uživatelské rozhraní. Vynechte tento přepínač předkompilovat k lokalitě pomocí aktualizovatelné uživatelské rozhraní.

Nakonec *cíl\_umístění\_složky* je fyzická cesta k adresáři cílové umístění; tato hodnota bude něco podobného jako `C:\MySites\Output\BookReviews`a musí být v uvozovkách.

## <a name="deploying-the-precompiled-website"></a>Nasazení webu předkompilovaných

V tuto chvíli jsme viděli jak předkompilovat webu pomocí obou aktualizovat a aktualizovatelné možnosti uživatelského rozhraní pomocí nástroje kompilace technologie ASP.NET. Ale příklady doposud mít předkompilovaných webu do místní složky a ne do produkčního prostředí. Dobrá zpráva je, že nasazení předkompilovaných webu je uloženy, lze provést pomocí sady Visual Studio nebo prostřednictvím některé další kopie mechanismus souborů, například z samostatné FTP klienta.

Dialogové okno Publikovat web (nejprve ukazuje **obrázek 1**) má možnost cílové umístění, které označuje, kde se soubory předkompilovaných webové stránky zkopírují do. Toto umístění může být na vzdáleném webovém serveru nebo FTP server. Vzdálený server v rámci tomuto textovému poli překompiluje a nasadí na webu na zadaný server v jednom kroku. Alternativně můžete předkompilovat webu do místní složky a ručně zkopírujte obsah této složce do produkčního prostředí prostřednictvím protokolu FTP nebo některé další přístup.

Předkompilované webu automaticky nasazeny pomocí sady Visual Studio dialogové okno publikování webu je užitečné pro jednoduché lokality tam, kde nejsou žádné konfigurace rozdíly mezi vývoj a produkční prostředí. Ale, jak je uvedeno v [ *běžné konfigurace rozdíly mezi vývoj a produkční* kurzu](common-configuration-differences-between-development-and-production-cs.md) je běžné pro tyto rozdíly existovat. Například kniha recenze webová aplikace používá jiné databázi v provozním prostředí, než ve vývojovém prostředí. Když Visual Studio publikuje webu na vzdálený server slepě zkopíruje si informace o konfiguraci souboru ve vývojovém prostředí.

Pro weby s konfigurační rozdíly mezi vývoj a produkční prostředí může být vhodné předkompilovat do místního adresáře, zkopírujte soubory konfigurace specifické pro produkční a zkopírujte obsah předkompilovaných výstup do produkční.

Aktualizační program na kopírování souborů z vývojového prostředí do produkčního prostředí najdete v části [ *nasazení webu pomocí klienta FTP* ](deploying-your-site-using-an-ftp-client-cs.md) a [  *Nasazení vašeho webu pomocí sady Visual Studio* ](determining-what-files-need-to-be-deployed-cs.md) kurzy.

## <a name="summary"></a>Souhrn

Technologie ASP.NET podporuje dva režimy kompilace: automatické a explicitní. Jak je popsáno v předchozí kurzy, projekty webových aplikací (WAP) použijte explicitní kompilace zatímco webové projekty (WSPs) ve výchozím nastavení používá automatickou kompilaci. Je však možné explicitně zkompilovat WSP před nasazením pomocí nástroje kompilace technologie ASP.NET.

Tento kurz se zaměřuje na předkompilaci kompilace nástroj pro podporu nasazení. Při předkompilaci pro nasazení, Nástroj kompilace vytvoří cílové umístění složky, zkompiluje zadaný webové aplikace zdrojového kódu a zkopíruje tyto zkompilovat sestavení a soubory obsahu do cílové složky umístění. Chcete-li vytvořit aktualizovatelné nebo aktualizovatelné uživatelské rozhraní lze nakonfigurovat nástroj kompilace. Pokud předkompilace s možnost aktualizovatelné uživatelského rozhraní, odebere se deklarativní do souborů obsahu. Stručně řečeno předkompilaci umožňuje nasazení aplikace na základě webový projekt bez zahrnutí všechny soubory zdrojového kódu a deklarativní odebrán, v případě potřeby.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Předkompilace webové stránky ASP.NET](https://msdn.microsoft.com/en-us/library/ms228015.aspx)
- [CodeBehind a kompilace technologie ASP.NET 2.0](https://msdn.microsoft.com/en-us/magazine/cc163675.aspx)
- [Předkompilaci technologie ASP.NET](http://www.odetocode.com/Articles/417.aspx)
- [Možnosti předkompilovaných webů technologie ASP.NET](http://www.dotnetperls.com/precompiled)

>[!div class="step-by-step"]
[Předchozí](logging-error-details-with-elmah-cs.md)
[další](users-and-roles-on-the-production-website-cs.md)
