---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-cs
title: Nasazení webu pomocí sady Visual Studio (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Visual Studio obsahuje nástroje pro nasazení webu. Další informace o těchto nástrojích v tomto kurzu.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: cde4ee53-a5d0-4937-a54b-67877e8266c3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-cs
msc.type: authoredcontent
ms.openlocfilehash: d549c615ea822d58ae71876ff3acd28c5f773d8a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399757"
---
<a name="deploying-your-site-using-visual-studio-c"></a>Nasazení webu pomocí sady Visual Studio (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_04_CS.zip) nebo [stahovat PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial04_DeployingViaVS_cs.pdf)

> Visual Studio obsahuje nástroje pro nasazení webu. Další informace o těchto nástrojích v tomto kurzu.


## <a name="introduction"></a>Úvod

V předchozím kurzu podívali se na tom, jak nasadit jednoduchou webovou aplikaci ASP.NET ke zprostředkovateli webového hostitele. Konkrétně tento kurz vám ukázal, jak použít klienta jako Filezilly přenést do produkčního prostředí potřebné soubory z vývojového prostředí. Visual Studio také nabízí integrované nástroje pro usnadnění nasazení ke zprostředkovateli webového hostitele. Tento kurz zkoumá dva z těchto nástrojů: nástroj Kopírovat web, kde můžete přesunout soubory do a ze vzdáleného webového serveru pomocí protokolu FTP nebo rozšíření serveru FrontPage; a publikovat nástroj, který kopíruje celého webu do zadaného umístění.


> [!NOTE]
> Další nástroje týkající se nasazení, které nabízí Visual Studio patří [webové projekty instalace](https://msdn.microsoft.com/library/wx3b589t.aspx) a [projekty nasazení webu](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) Add-In. Webové projekty instalace balíčku obsahu webu a informace o konfiguraci do jednoho souboru MSI. Tato možnost je zvláště užitečná pro weby, které jsou nasazené v rámci sítě intranet nebo pro společnosti, které se prodávají předem zabalené webové aplikace, které zákazníci nainstalovat na své vlastní webové servery. Je doplněk projekty nasazení Web Visual Studio Add-In, která usnadňuje určení rozdíly mezi konfigurací sestavení pro vývojové prostředí a produkční prostředí. Webové projekty instalace nejsou popsané v této sérii kurzů; Projekty nasazení webu jsou shrnuté v [ *společné konfigurace rozdíly mezi vývojovou a provozní* ](common-configuration-differences-between-development-and-production-cs.md) kurzu.


## <a name="deploying-your-site-using-the-copy-web-site-tool"></a>Nasazení webu pomocí nástroje pro kopírování webu

Nástroj pro kopírování webu sady Visual Studio je funkce podobný samostatný klient FTP. Řečeno v kostce nástroj pro kopírování webu vám umožní připojit na vzdálený web přes FTP nebo rozšíření serveru FrontPage. Podobně jako u Filezilly uživatelské rozhraní, zkopírujte webové stránky uživatelského rozhraní se skládá ze dvou podoken: v levém podokně zobrazí místní soubory a pravém podokně je seznam těchto souborů na cílovém serveru.

> [!NOTE]
> Nástroj Kopírovat web dostupná jenom pro webové projekty. Visual Studio nabízí tento nástroj při práci s projekt webové aplikace.

Pojďme se podívat na použití nástroj Kopírovat web k publikování recenze knihy aplikaci do produkčního prostředí. Protože nástroj Kopírovat web funguje jenom s projekty, které používají webový projekt modelu jsme dokáže vyšetřit jen s projektem BookReviewsWSP pomocí tohoto nástroje. Otevřte tento projekt.

Spustit projekt nástroj Kopírovat web kliknutím na ikonu kopírování webu v Průzkumníku řešení (Tato ikona je kruhu na obrázku 1). Alternativně můžete vybrat možnost Kopírovat web z nabídky na webu. Kterýkoliv přístup spustí uživatelské rozhraní Kopírovat web na obrázku 1; v levém podokně na obrázku 1 je naplnit, protože musíme ještě připojit ke vzdálenému serveru.


[![Nástroj Kopírovat web uživatelské rozhraní je rozdělen do dvou podoken](deploying-your-site-using-visual-studio-cs/_static/image2.png)](deploying-your-site-using-visual-studio-cs/_static/image1.png)

**Obrázek 1**: uživatelské rozhraní pro kopírování webu nástroje je rozdělen do dvou podoken ([kliknutím ji zobrazíte obrázek v plné velikosti](deploying-your-site-using-visual-studio-cs/_static/image3.png))


Pokud chcete nasadit náš web, musíme nejprve připojte ke zprostředkovateli webového hostitele. Klikněte na tlačítko Připojit v horní části uživatelského rozhraní pro kopírování webu. Zobrazí se dialogové okno otevřít webovou stránku, je znázorněno na obrázku 2.

Výběrem jedné ze čtyř možností od levého okraje, můžete se připojit k cílového webu:

- **Systém souborů** – výběrem této možnosti nasaďte svůj web do složky nebo síťové sdílené složky přístupné z počítače.
- **Místní služba IIS** – tuto možnost použijte k nasazení webu na webový server IIS v počítači nainstalována.
- **Server FTP** – připojit na vzdálený web pomocí FTP.
- **Vzdálený server** – připojit na vzdálený web pomocí rozšíření serveru FrontPage.

Většina webové hostitele zprostředkovatele podpory FTP, ale méně nabízí podporu rozšíření serveru FrontPage. Z tohoto důvodu jsme vybrali možnost Server FTP a potom zadat informace o připojení, jak je znázorněno na obrázku 2.


[![Zadejte cílový web](deploying-your-site-using-visual-studio-cs/_static/image5.png)](deploying-your-site-using-visual-studio-cs/_static/image4.png)

**Obrázek 2**: Zadejte cílový web ([kliknutím ji zobrazíte obrázek v plné velikosti](deploying-your-site-using-visual-studio-cs/_static/image6.png))


Po připojení se nástroj pro kopírování webu načte soubory ve vzdálené lokalitě v pravém podokně a uvádí stav každého souboru: nové, Deleted, změněné nebo Unchanged. Můžete zkopírovat soubor z místní lokality do vzdálené lokality nebo naopak.

Přidejme novou stránku do projektu BookReviewsWSP a potom ji nasadíte tak, aby jsme nástroj Kopírovat web v akci. Vytvoření nové stránky technologie ASP.NET v sadě Visual Studio v kořenovém adresáři s názvem `Privacy.aspx`. Stránka na hlavní stránce `Site.master` a přidat zásady ochrany osobních údajů vašeho webu na tuto stránku. Obrázek 3 ukazuje Visual Studio po vytvoření této stránky.


[![Přidejte novou stránku s názvem &lt;kód&gt;Privacy.aspx&lt;/code&gt; do kořenové složky webu](deploying-your-site-using-visual-studio-cs/_static/image8.png)](deploying-your-site-using-visual-studio-cs/_static/image7.png)

**Obrázek 3**: přidejte novou stránku s názvem `Privacy.aspx` do kořenové složky webu ([kliknutím ji zobrazíte obrázek v plné velikosti](deploying-your-site-using-visual-studio-cs/_static/image9.png))


Pak se vraťte do uživatelského rozhraní pro kopírování webu. Jak je vidět na obrázku 4, v levém podokně teď obsahuje nové soubory – `Policy.aspx` a `Policy.aspx.cs`. A co víc tyto soubory jsou označeny ikonou šipky a stavu z nového označující, že existují v místní lokalitě, ale není na vzdáleném webu.


[![Nástroj pro kopírování webu obsahuje nový &lt;kód&gt;Privacy.aspx&lt;/code&gt; stránky v jeho levé podokno](deploying-your-site-using-visual-studio-cs/_static/image11.png)](deploying-your-site-using-visual-studio-cs/_static/image10.png)

**Obrázek 4**: nástroj pro kopírování webu obsahuje nový `Privacy.aspx` stránky v jeho levé podokno ([kliknutím ji zobrazíte obrázek v plné velikosti](deploying-your-site-using-visual-studio-cs/_static/image12.png))


K nasazení nové soubory, vyberte je a pak klikněte na ikonu šipky pro přenos do vzdálené lokality. Po dokončení převodu `Policy.aspx` a `Policy.aspx.cs` soubory existují na místních i vzdálených lokalitách stav Unchanged.

Nástroj Kopírovat web spolu s výpisem nové soubory, zvýrazní všechny soubory, které se liší mezi místními a vzdálenými lokalitami. Chcete-li zobrazit toto v akci, vraťte se na `Privacy.aspx` stránky a přidejte několik více slov do zásady ochrany osobních údajů. Uložit na stránku a pak se vraťte na nástroj pro kopírování webu. Obrázek 5 ukazuje, `Privacy.aspx` stránky v levém podokně je ve stavu změněné označující, že je synchronizovaný s vzdálené lokality.


[![Nástroj pro kopírování webu znamená to, že &lt;kód&gt;Privacy.aspx&lt;/code&gt; stránky se změnil.](deploying-your-site-using-visual-studio-cs/_static/image14.png)](deploying-your-site-using-visual-studio-cs/_static/image13.png)

**Obrázek 5**: nástroj pro kopírování webu znamená to, že `Privacy.aspx` stránky se změnil ([kliknutím ji zobrazíte obrázek v plné velikosti](deploying-your-site-using-visual-studio-cs/_static/image15.png))


Nástroj Kopírovat web také znamená, pokud se soubor odstranil od poslední operace kopírování. Odstranit `Privacy.aspx` z místní projekt a aktualizovat nástroj pro kopírování webu. `Privacy.aspx` a `Privacy.aspx.cs` soubory zůstanou uvedené v levém podokně, ale mají stav odstraněno označující, že byly odstraněny od poslední operace kopírování.

## <a name="publishing-a-web-application"></a>Publikování webové aplikace

Dalším způsobem, jak nasadit webovou aplikaci z Visual Studia je použití možností publikovat, která je přístupná prostřednictvím nabídky sestavení. Možnost publikovat explicitně zkompiluje aplikaci a pak zkopíruje všechny potřebné soubory až do zadaného vzdáleného serveru. Ukážeme za chvíli, je možnost publikovat více než nástroj Kopírovat web bez špičky. Vzhledem k tomu nástroj Kopírovat web umožňuje prozkoumat souborů na místních i vzdálených lokalitách a umožňuje odeslání nebo stažení jednotlivých souborů, podle potřeby, možnost publikovat nasadí celé webové aplikace.


Kromě zkopírování všech požadovaných souborů do zadaného vzdálené lokality, možnost publikovat také explicitně kompilaci aplikace. Vzhledem k tomu, že projekty webových aplikací musí být explicitně zkompilován by měl mít jako žádným překvapením, že možnost publikovat je k dispozici pro projekty webových aplikací. Co může být trochu překvapivé je, že možnost publikovat i k dispozici pro webové projekty. Jak je uvedeno v [ *určující, co soubory musí být nasazeny* ](determining-what-files-need-to-be-deployed-cs.md) kurzu webových projektů mohou být zkompilovány explicitně procesem, který se označuje jako *předkompilace*. Tento kurz se zaměřuje na pomocí možností publikovat projekty webových aplikací; budoucí kurzu prozkoumá předkompilace, v tomto okamžiku se vrátíme se podívat na možnost publikovat pomocí webových projektů.

> [!NOTE]
> Možnost publikovat je k dispozici v sadě Visual Studio pro webové projekty a projekty webových aplikací, nabízí Visual Web Developer pouze možnost publikovat projekty webových aplikací.


Podívejme se na nasazení aplikace recenzí pomocí možností publikovat. Začněte otevřením BookReviewsWAP (projektu webové aplikace) v sadě Visual Studio. Z nabídky Publikovat zvolte BookReviewsWAP sestavení projektu. Tím se zobrazí dialogové okno, které zobrazí výzvu k zadání cílové umístění, mezi další možnosti konfigurace (viz obrázek 6). Podobně jako pomocí nástroje pro kopírování webu můžete zadat umístění, která odkazuje na místní složku, místní web služby IIS, vzdálený web, který podporuje rozšíření serveru FrontPage, nebo adresu serveru FTP. Můžete zvolit, jestli se má nahradit nasazených souborů soubory na vzdáleném webovém serveru nebo odstranit veškerý obsah na vzdáleném webu před publikováním. Můžete také určit, jestli se má kopírovat:

- Pouze soubory v projektu potřebných ke spuštění aplikace, která vynechává nepotřebné zdrojový kód a soubory související s projektem.
- Všechny soubory projektu, které zahrnuje soubory zdrojového kódu a soubory projektu sady Visual Studio, jako je soubor řešení.
- Všechny soubory ve zdrojové složce projektu, který zkopíruje všechny soubory ve zdrojové složce projektu bez ohledu na to, jestli jsou zahrnuté v projektu.

Je také možnost nahrát obsah `App_Data` složky.


[![Zadejte cílový web](deploying-your-site-using-visual-studio-cs/_static/image17.png)](deploying-your-site-using-visual-studio-cs/_static/image16.png)

**Obrázek 6**: Zadejte cílový web ([kliknutím ji zobrazíte obrázek v plné velikosti](deploying-your-site-using-visual-studio-cs/_static/image18.png))


Pro aplikaci recenze knihy vzdálené lokality obsahuje soubory při kopírování projektu BookReviewsWSP přes nástroj pro kopírování webu nasadit. Proto teď mít možnost publikovat, začněte tím, že odstranění veškerý existující obsah. Navíc jenom zkopírujeme potřebné soubory spíše než nebudou zbytečně zabírat produkčního prostředí s nepotřebné zdrojový kód a soubory projektu. Po zadání těchto možností, klikněte na tlačítko Publikovat. Za další několik sekund Visual Studio nasadí potřebné soubory do cílové lokality zobrazuje jeho průběh v okně výstup.

Obrázek 7 znázorňuje souborů na server FTP po dokončení operace publikování. Všimněte si, že byly odeslány pouze na stránkách značek a soubory podpory potřebné sever - a -na straně klienta.


[![Jenom potřebné soubory byly publikovány do produkčního prostředí](deploying-your-site-using-visual-studio-cs/_static/image20.png)](deploying-your-site-using-visual-studio-cs/_static/image19.png)

**Obrázek 7**: jen potřebné soubory byly publikovány do produkčního prostředí ([kliknutím ji zobrazíte obrázek v plné velikosti](deploying-your-site-using-visual-studio-cs/_static/image21.png))


Možnost publikování je méně odlišování nástroj než nástroj Kopírovat web. Vzhledem k tomu nástroj Kopírovat web umožňuje kontrolu souborů na místních i vzdálených lokalitách a zobrazit, jak se liší, poskytuje možnost publikovat dané rozhraní není. Kromě toho nástroj Kopírovat web můžete provádět jednorázové změny, Nahráním či odstranění jednotlivých souborů. Možnost publikovat neumožňuje takové detailní; Místo toho publikuje *celý* aplikace. Toto chování má své výhody a nevýhody. Na straně plus vědět při použití možnosti publikování můžete nebude zapomínání k nahrání souboru důležité. Zvažte, co se stane, že pokud jste provedli menší změnu na velmi velké web - s možností publikovat nelze aktualizovat tuto stránku nebo dvě, který byl změněn, ale místo toho musíte počkat, když sada Visual Studio nasadí celý web.

Není, že tam být některé soubory, jejichž obsah se liší mezi produkční a vývojových prostředích. Klíče příklad je konfigurační soubor aplikace, `Web.config`. Protože možnost publikovat slepě zkopíruje soubory webové aplikace přepíše přizpůsobené konfigurační soubory v provozním prostředí s verzí ve vývojovém prostředí. Následujícím kurzu zkoumá dále v tomto tématu a nabízí tipy pro nasazení webové aplikace, pokud existují tyto rozdíly.

## <a name="summary"></a>Souhrn

Nasazení webu zahrnuje kopírování potřebné soubory z vývojového prostředí do produkčního prostředí. Předchozí kurz vám ukázal, jak přenášet soubory pomocí klienta FTP jako Filezilly. V tomto kurzu prozkoumat dva nástroje pro nasazení v sadě Visual Studio: nástroj Kopírovat web a možností publikovat. Nástroj pro kopírování webu je podobný klienta FTP v tom, že má paned dvě rozhraní, vypisování souborů na místním počítači a zadaného vzdáleného počítače, který umožňuje snadno nahrávat nebo stahovat soubory mezi dvěma počítači. Možnost publikování je více bez špičky nástroj, který explicitně zkompiluje projekt a pak nasadí do určeného cíle celé aplikace.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Kopírování webu pomocí nástroje pro kopírování webu](https://msdn.microsoft.com/library/1cc82atw.aspx)
- [Jak mohu: nasadit webový server pomocí nástroje pro kopírování webu](../../../videos/how-do-i/how-do-i-deploy-a-web-site-using-the-copy-web-site-tool.md) (Video)
- [Postupy: Publikování projektů webových aplikací](https://msdn.microsoft.com/library/aa983453.aspx)
- [Postupy: Publikování webů](https://msdn.microsoft.com/library/20yh9f1b.aspx)
- [Nastavení a nasazení projektů v sadě Visual Studio](https://msdn.microsoft.com/library/wx3b589t.aspx)

> [!div class="step-by-step"]
> [Předchozí](deploying-your-site-using-an-ftp-client-cs.md)
> [další](common-configuration-differences-between-development-and-production-cs.md)
