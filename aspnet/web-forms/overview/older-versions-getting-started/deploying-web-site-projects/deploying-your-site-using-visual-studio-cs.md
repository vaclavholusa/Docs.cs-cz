---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-cs
title: "Nasazení webu pomocí sady Visual Studio (C#) | Microsoft Docs"
author: rick-anderson
description: "Visual Studio obsahuje nástroje pro nasazení webu. Další informace o těchto nástrojích v tomto kurzu."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: cde4ee53-a5d0-4937-a54b-67877e8266c3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-cs
msc.type: authoredcontent
ms.openlocfilehash: 15d3d2c70346abad5addab5c29d4af9f238b39da
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="deploying-your-site-using-visual-studio-c"></a>Nasazení webu pomocí sady Visual Studio (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_04_CS.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial04_DeployingViaVS_cs.pdf)

> Visual Studio obsahuje nástroje pro nasazení webu. Další informace o těchto nástrojích v tomto kurzu.


## <a name="introduction"></a>Úvod

Předchozí kurzu podívat, jak nasadit jednoduchou webovou aplikaci ASP.NET ke zprostředkovateli webového hostitele. Konkrétně tento kurz vám ukázal, jak použít klienta jako FileZilla přenést potřebné soubory z vývojového prostředí do produkčního prostředí. Visual Studio nabízí i integrované nástroje usnadňuje nasazení do webového hostitele zprostředkovatele. V tomto kurzu prozkoumá dva z těchto nástrojů: nástroj Kopírovat web, kde můžete přesunout soubory do a z vzdáleném webovém serveru pomocí protokolu FTP nebo serverová rozšíření FrontPage; a publikovat nástroj, který zkopíruje celý web do zadaného umístění.


> [!NOTE]
> Zahrnout další týkající se nasazení nástroje Visual Studio nabízí [webové projekty instalace](https://msdn.microsoft.com/en-us/library/wx3b589t.aspx) a [webové projekty nasazení](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) Add-In. Webové projekty instalace balíčku obsahu webu a informace o konfiguraci do jednoho souboru MSI. Tato možnost je velmi užitečné pro weby, které jsou nasazeny v rámci sítě intranet nebo pro společnosti, které prodeje předem zabalené webové aplikace, které zákazníkům nainstalovat na své vlastní webové servery. Doplněk projekty nasazení webové je, že že Visual Studio Add-In usnadňující zadání konfigurace rozdíly mezi sestavení pro vývojové prostředí a pro produkční prostředí. Webové projekty instalace nejsou popisovaná v této kurz; Webové projekty nasazení jsou shrnuty v [ *běžné konfigurace rozdíly mezi vývoj a produkční* ](common-configuration-differences-between-development-and-production-cs.md) kurzu.


## <a name="deploying-your-site-using-the-copy-web-site-tool"></a>Nasazení webu pomocí nástroje Kopírovat web

Nástroj pro kopírování webu Visual Studio je funkce podobná samostatné klienta FTP. Stručně řečeno nástroj pro kopírování webu umožňuje připojení k vzdálené webu přes protokol FTP nebo serverová rozšíření FrontPage. Podobně jako na FileZilla uživatelské rozhraní, uživatelské rozhraní webu kopie tvoří dvě podokna: v levém podokně uvádí místní soubory, zatímco v pravém podokně uvádí tyto soubory na cílovém serveru.

> [!NOTE]
> Nástroj pro kopírování webu je dostupná jenom pro webové projekty. Visual Studio nabízí tento nástroj, když pracujete s projektem webové aplikace.

Podívejme se na pomocí nástroje Kopírovat web můžete publikovat aplikaci recenze knihy do produkčního prostředí. Protože nástroj Kopírovat web pracuje pouze s projekty, které používají model webový projekt jsme pouze zkontrolujte s projektem BookReviewsWSP pomocí tohoto nástroje. Otevřete tento projekt.

Spusťte nástroj projektu Kopírovat web kliknutím na ikonu kopírování webu v Průzkumníku řešení (Tato ikona je v kroužku na obrázku 1); Alternativně můžete vybrat možnost Kopírovat webový server z nabídky webu. Buď přístup spustí Kopírovat web uživatelské rozhraní zobrazené na obrázku 1; v levém podokně na obrázku 1 je naplnit, protože se musí ještě připojit ke vzdálenému serveru.


[![Uživatelské rozhraní nástroje Kopírovat web je rozdělené na dvě podokna](deploying-your-site-using-visual-studio-cs/_static/image2.png)](deploying-your-site-using-visual-studio-cs/_static/image1.png)

**Obrázek 1**: kopírování webu nástroj pro uživatelské rozhraní je rozdělené na dvě podokna ([Kliknutím zobrazit obrázek v plné velikosti](deploying-your-site-using-visual-studio-cs/_static/image3.png))


Za účelem nasazení náš web musíme nejprve připojení k poskytovateli webového hostitele. Klikněte na tlačítko Připojit v horní části uživatelského rozhraní Kopírovat webový server. Zobrazí se dialogové okno Otevřít web, na obrázku 2.

Připojení k webu cílové můžou zvolíte jednu ze čtyř možností zleva:

- **Systém souborů** – výběrem této možnosti nasazení vaší lokality do složky nebo sdílené složky přístupné z počítače.
- **Místní služba IIS** – tuto možnost použijte k nasazení webu na webový server IIS v počítači nainstalována.
- **Server FTP** -připojení vzdálený webový server pomocí protokolu FTP.
- **Vzdálené lokality** – připojení k vzdálené webu používá serverová rozšíření FrontPage.

Většina webové hostitele zprostředkovatele podpory FTP, avšak méně nabízí podporu rozšíření FrontPage serveru. Z tohoto důvodu I jste vybrali možnost Server FTP a poté zadat informace o připojení, jak je znázorněno na obrázku 2.


[![Zadejte cílového webu](deploying-your-site-using-visual-studio-cs/_static/image5.png)](deploying-your-site-using-visual-studio-cs/_static/image4.png)

**Obrázek 2**: Zadejte cílového webu ([Kliknutím zobrazit obrázek v plné velikosti](deploying-your-site-using-visual-studio-cs/_static/image6.png))


Po připojení, nástroj pro kopírování webu načte soubory na vzdálené lokality v pravém podokně a uvádí stav každého souboru: nové, Deleted, změněno nebo Unchanged. Soubor můžete zkopírovat z místní lokality na vzdálený server nebo naopak.

Umožňuje přidat novou stránku do projektu BookReviewsWSP a poté ji nasadit jsme viděli nástroj Kopírovat web v akci. Vytvořit novou stránku ASP.NET v sadě Visual Studio v kořenovém adresáři s názvem `Privacy.aspx`. Mít stránky pomocí stránky předlohy `Site.master` a přidejte zásady ochrany osobních údajů vaší lokality přepne na tuto stránku. Obrázek 3 ukazuje Visual Studio po vytvoření této stránky.


[![Přidat novou stránku s názvem &lt;kód&gt;Privacy.aspx&lt;/kódu&gt; do kořenové složky webu](deploying-your-site-using-visual-studio-cs/_static/image8.png)](deploying-your-site-using-visual-studio-cs/_static/image7.png)

**Obrázek 3**: Přidání nové stránky s názvem `Privacy.aspx` do kořenové složky webu ([Kliknutím zobrazit obrázek v plné velikosti](deploying-your-site-using-visual-studio-cs/_static/image9.png))


Pak se vraťte do uživatelského rozhraní Kopírovat webový server. Jak ukazuje obrázek 4, v levém podokně nyní zahrnuje nové soubory - `Policy.aspx` a `Policy.aspx.cs`. Navíc tyto soubory jsou označené ikonu šipky a stav z nové, která určuje, že existují na serveru místní, ale ne na vzdálené lokality.


[![Nástroj pro kopírování webu zahrnuje nový &lt;kód&gt;Privacy.aspx&lt;/kódu&gt; stránky v jeho levém podokně](deploying-your-site-using-visual-studio-cs/_static/image11.png)](deploying-your-site-using-visual-studio-cs/_static/image10.png)

**Obrázek 4**: nástroj pro kopírování webu zahrnuje nový `Privacy.aspx` stránky v jeho levém podokně ([Kliknutím zobrazit obrázek v plné velikosti](deploying-your-site-using-visual-studio-cs/_static/image12.png))


K nasazení nové soubory, vyberte je a pak klikněte na ikonu šipky přenést do vzdálené lokality. Po dokončení přenosu `Policy.aspx` a `Policy.aspx.cs` soubory existují v místních i vzdálených lokalitách se stavem Unchanged.

Nástroj pro kopírování webu společně s výpis soubory, označuje všechny soubory, které se liší od místních i vzdálených lokalitách. Chcete-li zobrazit to v praxi, vraťte se k `Privacy.aspx` stránky a přidejte další pár slov na zásady ochrany osobních údajů. Uložit stránky a pak se vraťte do nástroje Kopírovat webovou stránku. Jak je vidět na obrázku 5, `Privacy.aspx` stránky v levém podokně je ve stavu změněno označující, že je mimo rozsah synchronizace s vzdálené lokality.


[![Nástroj pro kopírování webu znamená, že &lt;kód&gt;Privacy.aspx&lt;/kódu&gt; změně stránky](deploying-your-site-using-visual-studio-cs/_static/image14.png)](deploying-your-site-using-visual-studio-cs/_static/image13.png)

**Obrázek 5**: nástroj pro kopírování webu znamená, že `Privacy.aspx` změně stránky ([Kliknutím zobrazit obrázek v plné velikosti](deploying-your-site-using-visual-studio-cs/_static/image15.png))


Nástroj Kopírovat web také určuje, pokud byl odstraněn soubor od poslední operace kopírování. Odstranit `Privacy.aspx` z místní projektu a aktualizace nástroje Kopírovat webovou stránku. `Privacy.aspx` a `Privacy.aspx.cs` soubory zůstávají uvedené v levém podokně, ale mají stav odstraněno indikující, že má odebraná od poslední operaci kopírování.

## <a name="publishing-a-web-application"></a>Publikování webové aplikace

Jiný způsob, jak nasadit webové aplikace z Visual Studia je použití možností publikovat, která je přístupná prostřednictvím nabídkou sestavit. Možnost publikovat explicitně kompilaci aplikace a pak zkopíruje všechny potřebné soubory až do zadaného vzdálené lokality. Jak jsme krátce zobrazí, je možnost publikovat více než nástroj Kopírovat web bez špičky. Zatímco nástroj Kopírovat web umožňuje zkontrolovat soubory na místních i vzdálených lokalitách a umožňuje odeslání nebo stažení jednotlivé soubory podle potřeby, možnost publikovat nasadí celé webové aplikace.


Kromě zkopírování všechny potřebné soubory na zadané vzdálené lokality, možnost publikovat také explicitně kompilaci aplikace. Vzhledem k tomu, že projekty webových aplikací, musí být explicitně kompilovat by měl mít jako žádné neočekávaném, že možnost publikovat je k dispozici pro projekty webových aplikací. Co může být trochu překvapivé je možnost publikovat je také k dispozici pro webové projekty. Jak jsme uvedli v [ *určení co soubory musí být nasazeny* ](determining-what-files-need-to-be-deployed-cs.md) kurzu webové projekty mohou být explicitně zkompilovány proces, který se označuje jako *předkompilaci*. Tento kurz se zaměřuje na pomocí možností publikovat projekty webových aplikací; budoucí kurzu bude prozkoumejte předkompilaci okamžiku vrátíme podívejte se na možnost publikovat pomocí webové projekty.

> [!NOTE]
> Při publikování možnost je dostupná v sadě Visual Studio pro webové projekty a projekty webových aplikací, Visual Web Developer pouze nabízí možnost publikovat pro projekty webových aplikací.


Podívejme se na nasazení aplikace z adresáře recenze pomocí možnosti publikovat. Začněte otevřením BookReviewsWAP (projekt webové aplikace) v sadě Visual Studio. Z nabídky publikování zvolte BookReviewsWAP sestavení projektu. Otevře dialogové okno s výzvou k cílové umístění, mezi další možnosti konfigurace (viz obrázek 6). Podobně jako pomocí nástroje Kopírovat web můžete zadat umístění, která odkazuje na místní složku, na místní web služby IIS, vzdálený web, který podporuje serverová rozšíření FrontPage nebo adresu serveru FTP. Můžete zvolit, jestli se má nahradit soubory na vzdáleném webovém serveru nasazené soubory nebo odstranit veškerý obsah na vzdálené lokality před publikováním. Můžete také určit, jestli se mají zkopírovat:

- Pouze soubory v projektu potřebné ke spuštění aplikace, které vynechá nepotřebné zdrojový kód a soubory související s projektem.
- Všechny soubory projektu, které obsahuje soubory zdrojového kódu a soubory projektu sady Visual Studio, jako třeba soubor řešení.
- Všechny soubory ve složce projektu zdroj, který zkopíruje všechny soubory ve zdrojové složce projektu, bez ohledu na to, jestli jsou zahrnuté v projektu.

Je také možnost uložení obsahu `App_Data` složky.


[![Zadejte cílového webu](deploying-your-site-using-visual-studio-cs/_static/image17.png)](deploying-your-site-using-visual-studio-cs/_static/image16.png)

**Obrázek 6**: Zadejte cílového webu ([Kliknutím zobrazit obrázek v plné velikosti](deploying-your-site-using-visual-studio-cs/_static/image18.png))


Vzdálené lokality pro aplikaci kontrolní seznam obsahuje soubory nasazené při kopírování BookReviewsWSP projektu prostřednictvím nástroje Kopírovat web. Proto budeme mít možnost publikovat, začněte tím, že odstraníte všechny existující obsah. Také můžeme jednoduše zkopírujete potřebné soubory místo zbytečného produkčního prostředí s nepotřebné zdrojové soubory kódu a projekt. Po zadání těchto možností, klikněte na tlačítko Publikovat. Přes další několik sekund Visual Studio nasadí potřebné soubory do cílové lokality zobrazení jejím průběhu v okně výstupu.

Obrázek 7 znázorňuje soubory na server FTP po dokončení operace publikování. Všimněte si, že byly odeslány pouze stránky značek a podpůrné soubory potřebné severu a straně klientů.


[![Pouze potřebné soubory byly publikovány do produkčního prostředí](deploying-your-site-using-visual-studio-cs/_static/image20.png)](deploying-your-site-using-visual-studio-cs/_static/image19.png)

**Obrázek 7**: pouze potřebné soubory byly publikovány do produkčního prostředí ([Kliknutím zobrazit obrázek v plné velikosti](deploying-your-site-using-visual-studio-cs/_static/image21.png))


Možnost publikovat je méně nuanced nástroj, než nástroj Kopírovat web. Zatímco nástroj Kopírovat web umožňuje zkontrolovat soubory na místních i vzdálených lokalitách a v tématu Jak se liší, poskytuje možnost publikovat takové rozhraní. Nástroj pro kopírování webu navíc umožňuje provádět změny v jednorázové, odesílání nebo odstranění jednotlivých souborů. Možnost publikovat nepovoluje takové jemně odstupňovanou kontrolu; Místo toho publikuje *celý* aplikace. Toto chování má své výhody a nevýhody. Na straně plus víte, při použití možnosti publikování, nebude možné že zapomenete nahrát soubor důležité. Ale zvažte, co se stane, pokud jste provedli změnu malé velké web - s možností publikovat nelze aktualizovat této stránce nebo dvě, která byla změněna, ale místo toho je nutné počkat, když Visual Studio nasadí celý web.

Není, protože některé soubory, jejichž obsah se liší mezi produkčního prostředí a vývojových prostředích musí existovat. Klíče příklad je konfigurační soubor aplikace, `Web.config`. Protože možností publikovat slepě zkopíruje soubory webové aplikace přepíše provozním prostředí vlastní konfigurační soubory s verzí ve vývojovém prostředí. Následné kurzu jsou zde popsány dále v tomto tématu a nabízí tipy pro nasazení webové aplikace, pokud existují tyto rozdíly.

## <a name="summary"></a>Souhrn

Nasazení webu zahrnuje kopírování potřebné soubory z vývojového prostředí do produkčního prostředí. Předchozí kurz vám ukázal, jak přenášet soubory pomocí klienta FTP, jako je FileZilla. V tomto kurzu zkontrolován dva nástroje pro nasazení v sadě Visual Studio: nástroj Kopírovat web a možností publikovat. Nástroj pro kopírování webu je podobná klient FTP v tom, že má dva paned rozhraní vypisování souborů na místním počítači a zadaný vzdálený počítač, který usnadňuje nahrávání nebo stahování souborů mezi dvěma počítači. Možnost publikovat je více bez špičky nástroj, který explicitně zkompiluje projekt a poté nasadí bude celá aplikace k zadanému cíli.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Kopírování webové stránky s nástroje Kopírovat web](https://msdn.microsoft.com/en-us/library/1cc82atw.aspx)
- [Jak I: nasazení webu pomocí nástroje Kopírovat web](../../../videos/how-do-i/how-do-i-deploy-a-web-site-using-the-copy-web-site-tool.md) (Video)
- [Postupy: Publikování projekty webových aplikací](https://msdn.microsoft.com/en-us/library/aa983453.aspx)
- [Postupy: Publikování webů](https://msdn.microsoft.com/en-us/library/20yh9f1b.aspx)
- [Projekty instalace a nasazení v sadě Visual Studio](https://msdn.microsoft.com/en-us/library/wx3b589t.aspx)

>[!div class="step-by-step"]
[Předchozí](deploying-your-site-using-an-ftp-client-cs.md)
[další](common-configuration-differences-between-development-and-production-cs.md)
