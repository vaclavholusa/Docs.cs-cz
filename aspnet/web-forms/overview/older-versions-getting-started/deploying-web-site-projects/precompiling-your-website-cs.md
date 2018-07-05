---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-cs
title: Předkompilace webu (C#) | Dokumentace Microsoftu
author: rick-anderson
description: 'Visual Studio nabízí vývojáře využívající technologii ASP.NET dva typy projektů: projekty webových aplikací (WAP) a projekty webů (WSPs). Jeden z klíčových rozdílů betwe...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: ecd5a4de-beb7-4d1d-bbbb-e31003633267
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-cs
msc.type: authoredcontent
ms.openlocfilehash: abe2c3329129259b9d83cb202fe730eda6abc94d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369431"
---
<a name="precompiling-your-website-c"></a>Předkompilace webu (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_15_CS.zip) nebo [stahovat PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial15_Precompiling_cs.pdf)

> Visual Studio nabízí vývojáře využívající technologii ASP.NET dva typy projektů: projekty webových aplikací (WAP) a projekty webů (WSPs). Jedním z hlavní rozdíly mezi dva typy projektů je, že WAP musí mít kód explicitně zkompilován před nasazením, zatímco kód v WSP můžete automaticky zkompilovat na webovém serveru. Nicméně je možné předkompilovat WSP před jejich nasazením. Tento kurz popisuje výhody předkompilace a ukazuje, jak předkompilace webu z Visual Studia a z příkazového řádku.


## <a name="introduction"></a>Úvod

Visual Studio nabízí vývojáře využívající technologii ASP.NET dva různé typy projektů: projekty webových aplikací (WAP) a projekty webu (soubor WSP). Jedním z hlavní rozdíly mezi těmito typy projektů je, že WAP vyžadují *explicitní kompilace* vzhledem k tomu použít WSPs *automatickou kompilaci*, ve výchozím nastavení. S WAP, kompilaci kódu webové aplikace do jednoho sestavení, která je vytvořena na webu `Bin` složky. Nasazení zahrnuje kopírování obsahu značky ( `.aspx.ascx`, a `.master` souborů) v projektu, spolu s v sestavení `Bin` složka; modelu code-behind třídy podotknout není nutné k nasazení. Na druhé straně WSPs nasadíte tak, že zkopírujete na stránkách značek a jejich odpovídající použití modelu code-behind tříd do produkčního prostředí. Použití modelu code-behind třídy se kompilují na vyžádání na webovém serveru.

> [!NOTE]
> Vraťte se do části "Explicitní kompilace Versus automatickou kompilaci" v [ *určující, co soubory musí být nasazeny* kurzu](determining-what-files-need-to-be-deployed-cs.md) pro další informace o rozdílech mezi projektu modely, explicitní a automatické sestavování a vliv nasazení modelu kompilace.


Možnost automatického kompilace se snadno používá. Neexistuje žádné explicitní kompilační krok a upravovat jenom soubory, které byly potřeba k nasazení, že explicitní kompilace vyžaduje nasazení stránky změny kódu a sestavení právě zkompilován. Automatické nasazení však má dva potenciální nedostatky:

- Protože stránky musí být zkompilovány automaticky, když první uživatel, může být krátký, ale významnému zpoždění při vyžádání stránky ASP.NET poprvé po nasazení.
- Automatickou kompilaci vyžaduje, aby obě deklarativní značkami a zdrojový kód k dispozici na webovém serveru. Zkuste možnost to může být, pokud máte v úmyslu na prodej webové aplikace pro zákazníky, kteří ji nainstalovat na své webové servery.

Pokud dvou nad nedostatky jsou moduly pro dělení záležitost, můžete buď přepněte model WAP nebo *předkompilovat* WSP před jejich nasazením. Tento kurz zkoumá možnosti předkompilace nejvhodnější pro prostředí webu a provede předkompilace procesu a nasazení předkompilovaný web.

## <a name="an-overview-of-aspnet-code-generation-and-compilation"></a>Přehled o vytváření a kompilování kódu ASP.NET

Předtím, než se podíváme na dostupné možnosti předkompilace, nejprve se seznámíme vytváření a kompilování kódu, která nastane stránky ASP.NET je požadován poprvé, protože byl vytvořen nebo naposledy aktualizována. Jak už víte, stránek ASP.NET se skládají ze dvou částí: deklarativní v `.aspx` souboru; a část zdrojového kódu, obvykle v souboru třídy samostatné kódu na pozadí (`.aspx.cs`). Kroky prováděné modul runtime, pokud se požaduje stránky ASP.NET, závisí na model kompilace aplikace.

S WAP musí být na stránkách zdrojový kód explicitně zkompilovány do jednoho sestavení před nasazením. Během nasazování toto sestavení a na různých stránkách značek se zkopírují do produkčního prostředí. Pokud požadavek dorazí na server webové stránky ASP.NET, modul runtime vytvoří instanci třídy modelu code-behind na stránce a vyvolá jeho `ProcessRequest` metodu, která spustí životního cyklu stránky a nakonec, vygeneruje stránky obsahu, který je vrácen žadatel. Modul runtime můžete pomocí třídy modelu code-behind stránky ASP.NET fungovat, protože použití modelu code-behind třídy již byl zkompilován do sestavení před jejich nasazením.

WSPs a automatickou kompilaci neexistuje žádné explicitní kompilační krok před jejich nasazením. Místo toho nasazení zahrnuje kopírování deklarativního a zdrojový kód obsah do produkčního prostředí. Dorazí požadavek na server webové stránky ASP.NET poprvé od stránce je vytvoření nebo poslední aktualizace, musíte modul runtime nejprve zkompilovat do sestavení použití modelu code-behind třídy. Tato zkompilovaného sestavení je uložen ve složce `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`, i když umístění této složky je možné přizpůsobit prostřednictvím `<compilation tempDirectory="" />` prvek `<system.web>`, obvykle v `Web.config`. Vzhledem k tomu, že je sestavení uloženo na disk, to není potřeba překompilovat o následných požadavcích na stejnou stránku.

> [!NOTE]
> Dle očekávání, je dojde k mírnému zpoždění při požadování stránky na webu, který používá automatické kompilaci, kterou trvá chvíli, než server ke kompilaci kódu stránky a uložit výsledného sestavení do první (nebo poprvé, protože byla změněna) disk.


Stručně řečeno s explicitní kompilace musíte ke kompilaci zdrojového kódu na webu před nasazením, ukládá modul runtime se nebudou muset provést tento krok. S automatickou kompilaci modul runtime zpracovává kompilace zdrojového kódu na stránkách, ale s cenou mírné inicializace pro první návštěvě stránky, protože byl vytvořen nebo poslední aktualizace.

Ale co o deklarativní části stránky technologie ASP.NET ( `.aspx` souboru)? Je zřejmé, že je vztah mezi `.aspx` soubory a kód v jejich použití modelu code-behind tříd, jako webové ovládací prvky definované v deklarativním označení jsou k dispozici v kódu. Je zřejmé, který obsah `.aspx` soubory výrazně ovlivňuje vykreslované značky generované stránky. Jak modul runtime funguje s text, HTML, a podle syntaxe webového ovládacího prvku `.aspx` soubor ke generování požadované stránky uživatele vykreslený obsah?

Nechci příliš nenechejte na podrobnosti nízké úrovně implementace, které se liší mezi WAP a WSPs, ale řečeno v kostce modul runtime automaticky vygeneruje soubor třídy, která obsahuje různé ovládací prvky webové jako chráněné členy a metody. Tento vygenerovaný soubor je implementovaný jako *částečné třídy* odpovídající třídu kódu na pozadí. ([Částečné třídy](http://www.dotnet-guide.com/partialclasses.html) bylo obsah jednu třídu možné rozdělit do více souborů.) Proto je definována třída použití modelu code-behind na dvou místech: v `.aspx.cs` soubor, který vytvoříte a v této třídě vygeneruje automaticky vytvořené modulem runtime. Tato třída automaticky generované je uložen v `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` složky.

Důležité odnést zde je, že pro stránky ASP.NET k vykreslení modulem runtime i jeho deklarativní a části zdrojového kódu musí být zkompilovány do sestavení. S WAP zdrojový kód je explicitně zkompilovány do sestavení před nasazením, ale deklarativní musí být stále převedeny na kód a zkompilovat modulem runtime na webovém serveru. S použitím automatickou kompilaci WSPs zdrojový kód a deklarativní musí být kompilovány webový server.

Je možné použít explicitní kompilace s modelem WSP. Můžete explicitně zkompilovat část zdrojového kódu, jako je s modelem WAP. A co víc můžete také zkompilovat deklarativní.

## <a name="precompilation-options"></a>Možnosti předkompilace

Rozhraní .NET Framework je součástí [nástroj pro kompilaci aplikací technologie ASP.NET (`aspnet_compiler.exe`)](https://msdn.microsoft.com/library/ms229863.aspx) , která umožňuje kompilaci zdrojového kódu (a dokonce obsah) vytvořené pomocí modelu WSP aplikace ASP.NET. Tento nástroj bylo vydáno se sadou .NET Framework verze 2.0 a je umístěn v `%WINDIR%\Microsoft.NET\Framework\v2.0.50727` složky; můžete používat z příkazového řádku nebo spuštěn z prostředí sady Visual Studio prostřednictvím možnosti Publikovat web v nabídce sestavení.

Nástroj pro kompilaci poskytuje dvě obecné formy kompilace: místní předkompilace a předkompilace pro nasazení. S místní předkompilace spustíte `aspnet_compiler.exe` nástroje z příkazového řádku a zadejte cestu k virtuálnímu adresáři nebo fyzická cesta k webu, který se nachází ve vašem počítači. Nástroj pro kompilaci pak zkompiluje každé stránce technologie ASP.NET v projektu, uložení zkompilované verze v `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` složky stejně jako Pokud na stránkách měl každý byly navštívené poprvé z prohlížeče. Předkompilace na místě můžete urychlit první požadavek provedené nově nasazené stránky technologie ASP.NET na webu, protože zmírňuje runtime nepotřebuje k provedení tohoto kroku. Předkompilace místní však není užitečná pro většinu prostředí weby, protože vyžaduje, že budete moci spouštět programy z webového serveru příkazového řádku. Ve sdíleném hostování prostředí není povolená tato úroveň přístupu.

> [!NOTE]
> Další informace o místě předkompilace, projděte si [How To: předkompilovat webů ASP.NET](https://msdn.microsoft.com/library/ms227972.aspx) a [předkompilace v technologii ASP.NET 2.0](http://www.odetocode.com/Articles/417.aspx).


Místo na stránkách na webu pro kompilaci `Temporary ASP.NET Files` složky, zkompiluje předkompilace pro nasazení na stránkách do adresáře, které si vyberete a ve formátu, který je možné nasadit do produkčního prostředí.

Existují dva typy předkompilace pro nasazení, se budeme věnovat v tomto kurzu: předkompilace aktualizovat uživatelské rozhraní a předkompilace Dal aktualizovat uživatelské rozhraní. Předkompilace se aktualizovat uživatelské rozhraní zůstanou deklarativní v `.aspx`, `.ascx`, a `.master` soubory, což vývojářům zobrazit a v případě potřeby upravit deklarativní na provozním serveru. Generuje předkompilace Dal aktualizovat uživatelské rozhraní `.aspx` stránky, které jsou void veškerý obsah a odebere `.ascx` a `.master` soubory, a tím skrytí deklarativní a zakazují vývojář se to změní produkční prostředí.

### <a name="precompiling-for-deployment-with-an-updatable-user-interface"></a>Předkompilace pro nasazení se aktualizovat uživatelské rozhraní

Nejlepší způsob, jak pochopit předkompilace pro nasazení je příklad v akci. Umožňuje předkompilovat WSP revize adresáře pro nasazení pomocí aktualizovat uživatelské rozhraní. Nástroj ASP.NET kompilace lze vyvolat, v nabídce sestavení sady Visual Studio nebo z příkazového řádku. Tento oddíl se zabývá pomocí nástroje z Visual Studia; v části "předkompilace z příkazového řádku" zjistí spuštění nástroje kompilátoru z příkazového řádku.

Otevřít revizi WSP knihy ve Visual Studiu, přejděte do nabídky sestavení a vyberte možnost nabídky Publikovat Web. Otevře se dialogové okno publikování webu (viz **obrázek 1**), kde můžete určit cílové umístění, určuje, jestli je předkompilovaný web uživatelské rozhraní je aktualizovat a další možnosti nástrojů kompilátoru. Cílové umístění může být vzdáleném webovém serveru nebo FTP server, ale prozatím zvolte složky na pevném disku počítače. Protože chceme poté předkompilovat daný web s aktualizovat uživatelské rozhraní, nechte zaškrtnuté políčko "Umožnit tomuto předkompilovanému webu umožnit aktualizaci modelové" a klikněte na tlačítko OK.

[![](precompiling-your-website-cs/_static/image2.png)](precompiling-your-website-cs/_static/image1.png)

**Obrázek 1**: nástroje kompilace technologie ASP.NET se předkompilovat svůj web do zadané cílové umístění  
 ([Kliknutím ji zobrazíte obrázek v plné velikosti](precompiling-your-website-cs/_static/image3.png))

> [!NOTE]
> V nabídce sestavení možnost publikovat web není k dispozici v aplikaci Visual Web Developer. Pokud používáte aplikaci Visual Web Developer je potřeba použít verzi příkazového řádku nástroje kompilace technologie ASP.NET, který je popsaný v části "předkompilace z příkazového řádku".


Až si předkompilace webu, přejděte do cílového umístění, které jste zadali v dialogovém okně Publikovat Web. Za chvíli porovnání obsahu této složky s obsahem webu. **Obrázek 2** ukazuje recenzí složky webu. Všimněte si, že obsahuje `.aspx` a `.aspx.cs` soubory. Všimněte si také, že `Bin` adresář obsahuje pouze jeden soubor `Elmah.dll`, které jsme přidali [předchozím kurzu](logging-error-details-with-elmah-cs.md)

[![](precompiling-your-website-cs/_static/image5.png)](precompiling-your-website-cs/_static/image4.png)

**Obrázek 2**: adresář projektu obsahuje `.aspx` a `.aspx.cs` souborů; `Bin` složka obsahuje pouze `Elmah.dll`  
 ([Kliknutím ji zobrazíte obrázek v plné velikosti](precompiling-your-website-cs/_static/image6.png))

**Obrázek 3** ukazuje cílové umístění složky, jehož obsah vytvořil nástroj kompilace technologie ASP.NET. Tato složka neobsahuje žádné soubory kódu na pozadí. Kromě toho tato složka `Bin` adresář obsahuje několik sestavení a dva `.compiled` soubory kromě `Elmah.dll` sestavení.

[![](precompiling-your-website-cs/_static/image8.png)](precompiling-your-website-cs/_static/image7.png)

**Obrázek 3**: cílové umístění složky obsahuje soubory pro nasazení  
 ([Kliknutím ji zobrazíte obrázek v plné velikosti](precompiling-your-website-cs/_static/image9.png))

Na rozdíl od explicitní kompilace v WAP předkompilace pro proces nasazení vytvořit jedno sestavení pro celou lokalitu. Místo toho ji dávek společně několik stránek do každého sestavení. Také zkompiluje `Global.asax` do vlastního sestavení, jakož i všechny třídy v souboru (pokud existuje) `App_Code` složky. Soubory, které obsahují deklarativní pro technologii ASP.NET webové stránky, uživatelské ovládací prvky a hlavní stránky (`.aspx`, `.ascx`, a `.master` soubory, v uvedeném pořadí) se kopírují jako – je do cílového umístění adresáře. Podobně platí `Web.config` soubor je zkopírován přes, spolu s žádné statické soubory, jako jsou obrázky, šablony stylů CSS třídy a soubory PDF. Formálnější popis způsob, jakým nástroj pro kompilaci zpracovává různé typy souborů, přečtěte si [soubor zpracování během předkompilace](https://msdn.microsoft.com/library/e22s60h9.aspx).

> [!NOTE]
> Můžete určit, aby nástroj kompilace vytvořte jedno sestavení na stránku ASP.NET, uživatelský ovládací prvek nebo stránka předlohy zaškrtnutím políčka "Používá se oprava pojmenování a jednostránkové sestavení" v dialogovém okně Publikovat Web. S každou stránku ASP.NET, které jsou kompilovány do vlastního sestavení umožňuje jemněji odstupňovanou kontrolu nad nasazením. Například pokud aktualizace jedné webové stránky ASP.NET a potřebné k nasazení tuto změnu, potřebujete jenom nasadíte tuto stránku `.aspx` Souborová služba a přidružené sestavení do produkčního prostředí. Poraďte [postupy: generování přiděleny pevně určené názvy nástrojem kompilace ASP.NET](https://msdn.microsoft.com/library/ms228040.aspx) Další informace.


Cílový adresář umístění obsahuje také soubor, který nebyl součástí předkompilované webového projektu, konkrétně `PrecompiledApp.config`. Tento soubor informuje o tom, že aplikace byla předkompilována modul runtime ASP.NET a určuje, zda byla předkompilována s aktualizovat nebo od poledne aktualizovat UI.

A konečně, věnujte chvíli otevřete jednu z `.aspx` soubory v cílovém umístění pomocí sady Visual Studio nebo textový editor podle výběru. Při předkompilaci pro nasazení s aktualizovat uživatelské rozhraní stránky technologie ASP.NET v cílovém adresáři umístění obsahovat přesně stejný kód jako odpovídající soubory na webu.

### <a name="precompiling-for-deployment-with-a-non-updatable-user-interface"></a>Předkompilace pro nasazení s Dal aktualizovat uživatelské rozhraní

Nástroj ASP.NET compiler také umožňuje předkompilovat pro nasazení s Dal aktualizovat uživatelské rozhraní. Předkompilace webu s Dal aktualizovat uživatelské rozhraní funguje podobně jako předkompilace s aktualizovatelným UI, klíčovým rozdílem, že se stránek ASP.NET, uživatelské ovládací prvky a hlavní stránky v cílovém adresáři jsou odebrána z jejich kódu. Předkompilace webu pro nasazení s Dal aktualizovat uživatelské rozhraní, v nabídce sestavení zvolte možnost publikovat web, ale zrušte zaškrtnutí možnosti "Povolit tomuto předkompilovanému webu umožnit aktualizaci modelové" (Zobrazit **obrázek 4**).

[![](precompiling-your-website-cs/_static/image11.png)](precompiling-your-website-cs/_static/image10.png)

**Obrázek 4**: zrušte zaškrtnutí políčka "Povolit tomuto předkompilovanému webu umožnit aktualizaci modelové" možnost k předkompilaci s aktualizovat bez uživatelského rozhraní  
 ([Kliknutím ji zobrazíte obrázek v plné velikosti](precompiling-your-website-cs/_static/image12.png))

**Obrázek 5** cílové umístění složky se zobrazí po předkompilaci Dal aktualizovat uživatelského rozhraní.

[![](precompiling-your-website-cs/_static/image14.png)](precompiling-your-website-cs/_static/image13.png)

**Obrázek 5**: cílové umístění složky pro nasazení s Dal aktualizovat uživatelského rozhraní  
 ([Kliknutím ji zobrazíte obrázek v plné velikosti](precompiling-your-website-cs/_static/image15.png))

Porovnání **obrázek 3** k **obrázek 5**. Když dvě složky může vypadat stejně, mějte na paměti, že chybí složce Dal aktualizovat uživatelského rozhraní stránky předlohy `Site.master`. A přestože **obrázek 5** zahrnuje různé stránky technologie ASP.NET, je-li zobrazit obsah těchto souborů uvidíte, že jste byla odebrána z jejich deklarativní a nahradí zástupný text: "Toto je značkovací soubor generovaný nástrojem Nástroj pro předkompilaci byste neměli odstraňovat! "

[![](precompiling-your-website-cs/_static/image17.png)](precompiling-your-website-cs/_static/image16.png)

**Obrázek 5**: deklarativní je odebraná ze stránky ASP.NET

`Bin` Složek v **obrázky 3** a **5** více výrazně lišit. Kromě sestavení `Bin` složky **obrázek 5** zahrnuje `.compiled` souboru pro každou stránku, uživatelský ovládací prvek a hlavní stránku ASP.NET.

Předkompilace webu s Dal aktualizovat uživatelské rozhraní je užitečné v situacích, kde nechcete, aby obsah na stránkách ASP.NET provádět změny osoby nebo firmy, který se instaluje nebo spravuje webu v produkčním prostředí. Pokud vytváříte webovou aplikaci ASP.NET, která je prodávat zákazníkům k instalaci na svoje vlastní webové servery, můžete chtít zajistit, že jejich neprovádějte žádné změny vzhledu a chování vašeho webu přímo úpravou `.aspx` je odeslání stránky. Podle předkompilace webu s Dal aktualizovat uživatelské rozhraní, dodávat zástupný symbol `.aspx` stránky jako součást instalace, a tím brání vaši zákazníci zkoumání nebo upravit jejich obsah.

### <a name="precompiling-from-the-command-line"></a>Předkompilace z příkazového řádku

Na pozadí vyvolá dialogové okno publikování webu sady Visual Studio nástroje kompilace technologie ASP.NET (`aspnet_compiler.exe`) předkompilovat webu. Alternativně můžete spustit tento nástroj z příkazového řádku. Ve skutečnosti Pokud používáte aplikaci Visual Web Developer pak je potřeba kompilátoru nástroj spustit z příkazového řádku jako nabídka sestavení Visual Web Developer nezahrnuje možnosti Publikovat Web.

Pokud chcete použít nástroj kompilátoru z příkazového řádku, začněte tím, že přetažení do příkazového řádku a přejděte do adresáře rozhraní framework `%WINDIR%\Microsoft.NET\Framework\v2.0.50727`. Pak do příkazového řádku zadejte následující příkaz:

`aspnet_compiler -p "physical_path_to_app" -v / -f -u "target_location_folder"`

Výše uvedený příkaz spustí nástroj kompilátoru technologie ASP.NET (`aspnet_compiler.exe`) a prostřednictvím `-p` přepnout, dává pokyn umožňuje předkompilovat webu kořenovým adresářem v *fyzické\_cesta\_k\_aplikace*; Tato hodnota bude vypadat přibližně `C:\MySites\BookReviews`a musí být odděleny uvozovky.

`-v` Přepínač určuje virtuální adresáře webu. Pokud váš web je zaregistrovaný jako výchozí web v metabázi služby IIS, můžete vynechat `-p` přepnutí a stačí zadat virtuální adresář aplikace. Pokud používáte `-p` přepnout, hodnota řízení `-v` přepínač označuje kořenového webu a slouží k rozpoznání odkazů na kořenový adresář aplikace. Například pokud zadáte hodnotu `-v /MySite` pak odkazuje v aplikaci na `~/path/file` bude vyhodnocena jako `~/MySite/path/file`. Protože se nachází v kořenovém adresáři na webu firma poskytující hosting lokality recenzí jsem využil(a) přepínač `-v /`.

`-f` Přepínače, pokud jsou k dispozici, nastaví nástroj kompilace přepsat *cílové\_umístění\_složky* adresáře, pokud již existuje. Vynecháte-li `-f` přepínače a cílové umístění složky existuje, bude nástroj pro kompilaci ukončit s chybou: "Chyba ASPRUNTIME: cílový adresář není prázdný. Odstraňte jej ručně nebo vyberte jiný cíl."

`-u` Přepínače, pokud jsou k dispozici, poskytuje nástroj k vytvoření aktualizovat uživatelské rozhraní. Vynechte tento přepínač k poté předkompilovat daný web s Dal aktualizovat uživatelské rozhraní.

A konečně *cílové\_umístění\_složky* je fyzická cesta k adresáři umístění target; tato hodnota bude vypadat přibližně `C:\MySites\Output\BookReviews`a musí být odděleny uvozovky.

## <a name="deploying-the-precompiled-website"></a>Nasazení předkompilovaný web

V tuto chvíli jsme viděli jak předkompilace webu pomocí obou aktualizovat a Dal aktualizovat možnosti uživatelského rozhraní pomocí nástroje kompilace technologie ASP.NET. Však našich příkladů doposud mají předkompilovaný web do místní složky a ne do produkčního prostředí. Dobrou zprávou je, že nasazení předkompilovaný web podrobným a můžete provést pomocí sady Visual Studio nebo prostřednictvím některé další soubor použitý mechanizmus kopírování, například ze samostatného klienta FTP.

Dialogové okno publikování webu (první ukazuje **obrázek 1**) má možnost cílové umístění, která označuje, kde se předkompilované webové soubory zkopírují do. Toto umístění může být vzdáleném webovém serveru nebo serveru FTP. Do tohoto textového pole zadat vzdálený server předkompiluje a nasadí web k zadanému serveru v jednom kroku. Alternativně můžete předkompilace webu do místní složky a ručně zkopírujte obsah této složky do produkčního prostředí přes protokol FTP nebo některé jiné přístup.

Předkompilovaný web automaticky nasazena prostřednictvím dialogové okno publikování webu sady Visual Studio je užitečné pro jednoduché lokality tam, kde nejsou žádné konfigurace rozdíly mezi vývojovou a provozní prostředí. Nicméně, jako jsou uvedené v [ *společné konfigurace rozdíly mezi vývojovou a provozní* kurzu](common-configuration-differences-between-development-and-production-cs.md) není, že tyto rozdíly existovat. Například recenzí webová aplikace používá jiné databázi v produkčním prostředí než ve vývojovém prostředí. Když Visual Studio publikuje na web na vzdálený server slepě zkopíruje informace o konfiguraci souboru ve vývojovém prostředí.

Pro weby s konfigurací rozdíly mezi vývojovou a provozní prostředí může být vhodné poté předkompilovat daný web do místního adresáře, zkopírujte soubory konfigurace specifické pro produkční a zkopírujte obsah předkompilované výstup produkčního prostředí.

Aktualizační program na kopírování souborů z vývojového prostředí do produkčního prostředí najdete v tématu [ *nasazení webu pomocí klienta FTP* ](deploying-your-site-using-an-ftp-client-cs.md) a [  *Nasazení webu pomocí sady Visual Studio* ](determining-what-files-need-to-be-deployed-cs.md) kurzy.

## <a name="summary"></a>Souhrn

Technologie ASP.NET podporuje dva režimy kompilace: automatické a explicitní. Jak je popsáno v předchozích kurzech, projekty webových aplikací (WAP) použijte explicitní kompilace že webových projektů (WSPs) ve výchozím nastavení používají automatickou kompilaci. Nicméně je možné explicitně pomocí nástroje kompilace ASP.NET kompilovat soubor WSP před jejich nasazením.

Tento kurz se zaměřuje na nástroj kompilace předkompilace pro podporu nasazení. Při předkompilaci pro nasazení, nástroj pro kompilaci vytvoří cílové umístění složky, zkompiluje zadaný webové aplikace zdrojový kód a zkopíruje tyto zkompilován sestavení a soubory obsahu do složky, cílové umístění. Chcete-li vytvořit aktualizovatelné nebo Dal aktualizovat uživatelské rozhraní je možné nakonfigurovat nástroj pro kompilaci. Při předkompilaci s možností-aktualizovat uživatelské rozhraní, deklarativní soubory obsahu se odebere. Předkompilace řečeno v kostce, umožňuje nasazení aplikace založené na webový projekt bez zahrnutí všech souborů se zdrojovým kódem a pomocí deklarativní odebrán, v případě potřeby.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Předkompilace webu technologie ASP.NET](https://msdn.microsoft.com/library/ms228015.aspx)
- [Kompilaci v technologii ASP.NET 2.0 a CodeBehind](https://msdn.microsoft.com/magazine/cc163675.aspx)
- [Předkompilace v ASP.NET](http://www.odetocode.com/Articles/417.aspx)
- [Možnosti předkompilovaných webů v ASP.NET](http://www.dotnetperls.com/precompiled)

> [!div class="step-by-step"]
> [Předchozí](logging-error-details-with-elmah-cs.md)
> [další](users-and-roles-on-the-production-website-cs.md)
