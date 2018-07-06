---
uid: web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-cs
title: Několik prvků ContentPlaceHolder a výchozí obsah (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Zkoumá, jak přidat více zástupné znaky obsahu místě na hlavní stránku, jakož i jak určit výchozí obsah v obsahu místo zástupné znaky.
ms.author: aspnetcontent
ms.date: 05/21/2008
ms.assetid: b9b9798b-027d-46cc-9636-473378e437ac
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-cs
msc.type: authoredcontent
ms.openlocfilehash: 8e3a9146b22557691899bbfe299bc44682d1efae
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804155"
---
<a name="multiple-contentplaceholders-and-default-content-c"></a>Několik prvků ContentPlaceHolder a výchozí obsah (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_02_CS.zip) nebo [stahovat PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_02_CS.pdf)

> Zkoumá, jak přidat více zástupné znaky obsahu místě na hlavní stránku, jakož i jak určit výchozí obsah v obsahu místo zástupné znaky.


## <a name="introduction"></a>Úvod

V předchozím kurzu jsme se zaměřili na tom, jak povolit stránky předlohy vývojáře využívající technologii ASP.NET k vytvoření konzistentního rozložení pro celý web. Stránky předlohy definují značky, které jsou společné pro všechny její stránky obsahu a oblasti, které můžete přizpůsobit na základě stránku po stránce. V předchozím kurzu jsme vytvořili jednoduchou stránku předlohy (`Site.master`) a dvě stránky obsahu (`Default.aspx` a `About.aspx`). Naši stránku předlohy se skládal z dvou prvků ContentPlaceHolder s názvem `head` a `MainContent`, které se nacházely v `<head>` elementu a webové formuláře, v uvedeném pořadí. Zatímco obsahu stránky měli dva ovládací prvky obsahu, jsme zadali pouze kód pro jeden odpovídající `MainContent`.

Jak dokazuje dvou ovládacích prvků ContentPlaceHolder v `Site.master`, hlavní stránky může obsahovat několik prvků ContentPlaceHolder. A co víc si můžou vybrat výchozí značky pro ovládací prvky ContentPlaceHolder na hlavní stránce. Stránky obsahu, pak můžete volitelně zadejte vlastní značky nebo použít výchozí značka. V tomto kurzu jsme podívejte se na použití více ovládacích prvků obsahu na stránce předlohy a zjistit, jak definovat výchozí značek v ovládacích prvcích ContentPlaceHolder.

## <a name="step-1-adding-additional-contentplaceholder-controls-to-the-master-page"></a>Krok 1: Přidání ovládacích prvků ContentPlaceHolder další stránky předlohy

Řada návrhů webu obsahovat několik oblastí obrazovky, která jsou přizpůsobená na základě stránku po stránce. `Site.master`, hlavní stránky jsme vytvořili v předchozím kurzu, obsahuje jeden prvek ContentPlaceHolder v rámci webový formulář s názvem `MainContent`. Konkrétně se nachází v rámci tohoto prvku ContentPlaceHolder `mainContent` `<div>` elementu.

Obrázek 1 ukazuje `Default.aspx` při prohlížení prostřednictvím prohlížeče. Oblast červeně v kruhu je odpovídající kód specifický pro stránku `MainContent`.


[![Oblasti v kroužku ukazuje oblasti aktuálně přizpůsobitelné na základě po stránkách](multiple-contentplaceholders-and-default-content-cs/_static/image2.png)](multiple-contentplaceholders-and-default-content-cs/_static/image1.png)

**Obrázek 01**: na základě stránku po stránce se zobrazí v oblasti aktuálně přizpůsobitelné Circled oblasti ([kliknutím ji zobrazíte obrázek v plné velikosti](multiple-contentplaceholders-and-default-content-cs/_static/image3.png))


Představte si, že kromě oblasti je znázorněno na obrázku 1, musíme také přidat položky specifické pro stránku do levého sloupce pod poznatky a novinky oddíly. K tomu, přidáme jiný ovládací prvek ContentPlaceHolder na stránce předlohy. Pokud chcete postupovat s námi, otevřete `Site.master` hlavní stránky v aplikaci Visual Web Developer a pak přetáhněte ovládací prvek ContentPlaceHolder z panelu nástrojů na Návrhář za část zprávy. Nastavte ContentPlaceHolder `ID` k `LeftColumnContent`.


[![Přidání ovládacího prvku ContentPlaceHolder na levém sloupci stránky předlohy](multiple-contentplaceholders-and-default-content-cs/_static/image5.png)](multiple-contentplaceholders-and-default-content-cs/_static/image4.png)

**Obrázek 02**: Přidání ovládacího prvku ContentPlaceHolder na stránce předlohy levý sloupec ([kliknutím ji zobrazíte obrázek v plné velikosti](multiple-contentplaceholders-and-default-content-cs/_static/image6.png))


Přidání `LeftColumnContent` ContentPlaceHolder na stránce předlohy, jsme můžete definovat obsah pro tuto oblast na základě stránku po stránce zahrnutím obsahem ovládací prvek na stránce, jehož `ContentPlaceHolderID` je nastavena na `LeftColumnContent`. Zkoumáme, tento proces v kroku 2.

## <a name="step-2-defining-content-for-the-new-contentplaceholder-in-the-content-pages"></a>Krok 2: Definování obsahu pro nový prvek ContentPlaceHolder v obsahu stránky

Při přidávání nové stránky obsahu na web, aplikace Visual Web Developer automaticky vytvoří obsah ovládacího prvku na stránce pro každý prvek ContentPlaceHolder na vybrané stránce předlohy. S přidán `LeftColumnContent` ContentPlaceHolder na hlavní stránku v kroku 1, nové na ASP.NET stránky se teď mají tři ovládací prvky obsahu.

Pro znázornění, přidejte novou stránku obsahu do kořenového adresáře s názvem `MultipleContentPlaceHolders.aspx` , který je vázán `Site.master` stránky předlohy. Visual Web Developer vytvoří tuto stránku s následující kód:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample1.aspx)]

Zadejte nějaký obsah do ovládacího prvku obsahu odkazující `MainContent` prvků ContentPlaceHolder (`Content2`). V dalším kroku přidejte následující kód k `Content3` ovládacího prvku obsahu (který se odkazuje na `LeftColumnContent` ContentPlaceHolder):

[!code-html[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample2.html)]

Po přidání tohoto kódu, najdete na stránce prostřednictvím prohlížeče. Jak ukazuje obrázek 3 kód umístí do `Content3` ovládací prvek obsahu se zobrazí v levém sloupci pod oddíl novinek (v kruhu červeně). Kód umístí do `Content2` se zobrazí v pravé části stránky (v kruhu modře).


[![V levém sloupci nyní zahrnuje obsah specifický pro stránku pod oddílem novinky](multiple-contentplaceholders-and-default-content-cs/_static/image8.png)](multiple-contentplaceholders-and-default-content-cs/_static/image7.png)

**Obrázek 03**: levé straně sloupce teď zahrnuje specifický pro stránku obsahu pod the oddíl novinek ([kliknutím ji zobrazíte obrázek v plné velikosti](multiple-contentplaceholders-and-default-content-cs/_static/image9.png))


### <a name="defining-content-in-existing-content-pages"></a>Definování obsahu ve stávajících obsahu stránek

Vytvoření nové stránky obsahu automaticky zahrnuje ovládací prvek ContentPlaceHolder, kterou jsme přidali v kroku 1. Ale dvou existujících stránek s obsahem - `About.aspx` a `Default.aspx` -nemají ovládací prvek obsahu pro `LeftColumnContent` ContentPlaceHolder. K určení obsahu pro tento prvek ContentPlaceHolder v těchto dvou existujících stránek, potřebujeme přidat ovládací prvek obsahu sami.

Na rozdíl od většiny ovládacích prvků technologie ASP.NET v panelu nástrojů Visual Web Developer neobsahuje položku obsahu ovládacího prvku. Jsme můžete ručně zadat v deklarativním označení obsahu ovládacího prvku do zobrazení zdroje, ale je jednodušší a rychlejší přístup k zobrazení návrhu. Otevřít `About.aspx` stránky a přepněte do zobrazení návrhu. Jak ukazuje obrázek 4 `LeftColumnContent` ContentPlaceHolder se zobrazí v okně návrhu; Pokud myší nad ním načte nadpis zobrazovaný: "LeftColumnContent (Master)." Zahrnutí "Hlavní" v názvu označuje, že neexistuje žádný ovládací prvek obsahu, pro tento prvek ContentPlaceHolder na stránce definován. Jestliže existujte ovládací prvek obsahu pro ContentPlaceHolder, stejně jako v případě pro `MainContent`, bude číst název: "*ContentPlaceHolderID* (vlastní)."

Chcete-li přidat ovládací prvek obsahu pro `LeftColumnContent` ContentPlaceHolder na `About.aspx`rozbalte ContentPlaceHolder jeho inteligentních značek a klikněte na odkaz vytvořit vlastní obsah.


[![Zobrazení návrhu pro About.aspx ukazuje LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-cs/_static/image11.png)](multiple-contentplaceholders-and-default-content-cs/_static/image10.png)

**Obrázek 04**: návrhovém zobrazení pro `About.aspx` ukazuje `LeftColumnContent` ContentPlaceHolder ([kliknutím ji zobrazíte obrázek v plné velikosti](multiple-contentplaceholders-and-default-content-cs/_static/image12.png))


Kliknutím na vytvořit vlastní obsah odkaz generuje nezbytné obsah ovládacího prvku na stránku a nastaví její `ContentPlaceHolderID` vlastnost ContentPlaceHolder jeho `ID`. Například klepnutím na odkaz na vytvořit vlastní obsah `LeftColumnContent` v oblasti `About.aspx` na stránku přidá následující kód:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample3.aspx)]

### <a name="omitting-content-controls"></a>Vynechání ovládacích prvků obsahu

ASP.NET nevyžaduje, aby všechny obsahu stránky obsahují ovládací prvky obsahu pro každou jednotlivou ContentPlaceHolder definovány na hlavní stránce. Pokud ovládací prvek obsahu je vynechán, modul ASP.NET používá značky definované v rámci prvku ContentPlaceHolder na stránce předlohy. Tento kód se označuje jako ContentPlaceHolder jeho *výchozí obsah* a je užitečné v situacích, kde se obsah pro některé oblasti je běžné mezi většinou stránek, ale musí lze přizpůsobit pro malý počet stránek. Krok 3 zkoumá určující výchozí obsah na stránce předlohy.

V současné době `Default.aspx` obsahuje dva ovládací prvky obsahu pro `head` a `MainContent` prvků ContentPlaceHolder; nemá ovládací prvek obsahu pro `LeftColumnContent`. V důsledku toho, kdy `Default.aspx` je vykreslen `LeftColumnContent` ContentPlaceHolder jeho výchozí obsah se používá. Protože ještě musíme definovat žádné výchozí obsah pro tento prvek ContentPlaceHolder, výsledkem je, že žádné značky je vygenerován pro tuto oblast. Ověřte toto chování, najdete v tématu `Default.aspx` prostřednictvím prohlížeče. Jak je vidět na obrázku 5, žádné značky je vygenerován v levém sloupci pod oddíl novinek.


[![Žádný obsah je vykreslen pro LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-cs/_static/image14.png)](multiple-contentplaceholders-and-default-content-cs/_static/image13.png)

**Obrázek 05**: žádný obsah je vykreslen pro `LeftColumnContent` ContentPlaceHolder ([kliknutím ji zobrazíte obrázek v plné velikosti](multiple-contentplaceholders-and-default-content-cs/_static/image15.png))


## <a name="step-3-specifying-default-content-in-the-master-page"></a>Krok 3: Určení výchozí obsah na stránce předlohy

Některé návrhy webu zahrnovat oblasti, jejíž obsah je stejný pro všechny stránky webu s výjimkou jednu nebo dvě výjimky. Vezměte v úvahu web, který podporuje uživatelské účty. Tato lokalita vyžaduje přihlašovací stránku, kde návštěvníci můžete zadat své přihlašovací údaje pro přihlášení na web. Ohledně urychlení jejich zpracování procesu přihlašování, patří Návrháři webu textová pole uživatelské jméno a heslo v levém horním rohu každé stránky umožňuje uživatelům přihlásit se bez nutnosti explicitně najdete na stránce přihlášení. Tyto textová pole uživatelské jméno a heslo jsou užitečné v většinu stránek, ale jsou redundantní na přihlašovací stránce, která již obsahuje textová pole pro přihlašovací údaje uživatele.

Při implementaci tohoto návrhu, můžete vytvořit ovládací prvek ContentPlaceHolder v levém horním rohu stránky předlohy. Každé stránce, které se musí zobrazit textová pole uživatelské jméno a heslo v jejich levý horní roh by vytvořit ovládací prvek obsahu pro tento prvek ContentPlaceHolder a přidat nezbytné rozhraní. Na přihlašovací stránku, na druhé straně byste buď vynechejte přidání ovládacího prvku obsahu pro tento prvek ContentPlaceHolder nebo by vytvořila obsahem ovládacím prvkem bez značek definovaný. Nevýhodou tohoto přístupu je, že máme nezapomeňte přidat textová pole uživatelské jméno a heslo na každou stránku, kterou přidáme do lokality (s výjimkou přihlašovací stránky). To žádá potíže. Jsme pravděpodobně zapomenout přidat těchto textových polí na stránce nebo dvěma nebo horší, jsme neimplementuje rozhraní správně (třeba přidání jedním textbox místo dvou).

Lepším řešením je definovat jako ContentPlaceHolder jeho výchozí obsah textových polí uživatelské jméno a heslo. Díky tomu potřebujeme jen přepsat toto výchozí obsah v několika stránek, které nejsou zobrazeny textová pole uživatelské jméno a heslo (přihlášení stránky, například). Pro ilustraci, určující výchozí obsah pro ovládací prvek ContentPlaceHolder, můžeme implementovat scénář právě probírali.

> [!NOTE]
> Zbývající část tohoto kurzu aktualizuje našeho webu zahrnout rozhraní přihlášení v levém sloupci pro všechny stránky, ale na přihlašovací stránku. V tomto kurzu ale nezkoumá konfigurace webu pro podporu uživatelských účtů. Další informace o tomto tématu najdete Moje [ověřování pomocí formulářů, autorizace, uživatelských účtů a rolí](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) kurzy.


### <a name="adding-a-contentplaceholder-and-specifying-its-default-content"></a>Přidávání ContentPlaceHolder a určení výchozí obsah

Otevřít `Site.master` stránku předlohy a přidejte následující kód do levého sloupce mezi `DateDisplay` popisek a praktických poznatcích části:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample4.aspx)]

Po přidání tento kód by měl vypadat podobně jako na obrázku 6 návrhové zobrazení hlavní stránky.


[![Stránky předlohy se stránkou obsahuje ovládací prvek Login](multiple-contentplaceholders-and-default-content-cs/_static/image17.png)](multiple-contentplaceholders-and-default-content-cs/_static/image16.png)

**Obrázek 06**: hlavní stránka obsahuje ovládací prvek Login ([kliknutím ji zobrazíte obrázek v plné velikosti](multiple-contentplaceholders-and-default-content-cs/_static/image18.png))


Tento prvek ContentPlaceHolder `QuickLoginUI`, má ovládací prvek webové přihlášení jako výchozí obsah. Ovládací prvek Login zobrazí uživatelské rozhraní, které výzvu k zadání uživatelského jména a hesla společně s tlačítko přihlásit. Po kliknutí na tlačítko přihlásit, ovládací prvek Login interně ověřuje přihlašovací údaje uživatele do rozhraní API služby členství. Pokud chcete použít tento ovládací prvek Login v praxi, musíte nakonfigurovat tak, aby používal členství. Toto téma je nad rámec tohoto kurzu; získat Moje [ověřování pomocí formulářů, autorizace, uživatelských účtů a rolí](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) kurzy pro další informace o vytváření webové aplikace, která podporuje uživatelské účty.

Můžete přizpůsobit vzhled nebo chování ovládací prvek Login. Nastavím dvou vlastností: `TitleText` a `FailureAction`. `TitleText` Hodnotu vlastnosti, výchozí nastavení je "Přihlásit", se zobrazí v horní části ovládacího prvku uživatelského rozhraní. Tak, aby zobrazil text "Sign In" jako tuto vlastnost nastavili `<h3>` elementu. `FailureAction` Vlastnost určuje, co dělat, když přihlašovací údaje uživatele jsou neplatné. Použije se výchozí hodnota `Refresh`, která nechá uživatele na stejné stránce a zobrazí zpráva o selhání v rámci ovládacího prvku pro přihlášení. Jsem změnil na `RedirectToLoginPage`, který odešle uživatele na přihlašovací stránku v případě neplatné přihlašovací údaje. Nechci posílat uživatele na přihlašovací stránku, když se uživatel pokusí přihlásit se z některé stránky, ale selže, protože přihlašovací stránky může obsahovat další pokyny a možnosti, které se snadno nevejde do levého sloupce. Například přihlašovací stránky může zahrnovat možnosti obnovit zapomenuté heslo nebo vytvořit nový účet.

### <a name="creating-the-login-page-and-overriding-the-default-content"></a>Vytváří se na přihlašovací stránku a přepisuje výchozí obsah

Na hlavní stránce dokončení naším dalším krokem je vytvoření přihlašovací stránku. Přidání stránky ASP.NET do kořenového adresáře vašeho webu s názvem `Login.aspx`, jeho pro vazbu `Site.master` stránky předlohy. Tím vytvoříte stránku s čtyři ovládací prvky obsahu, jeden pro každou prvků ContentPlaceHolder definované v `Site.master`.

Přidejte ovládací prvek pro přihlášení `MainContent` ovládacího prvku obsahu. Podobně, můžete bez obav žádný obsah k přidání `LeftColumnContent` oblasti. Ale ujistěte se, že chcete nechat pro ovládací prvek obsahu `QuickLoginUI` ContentPlaceHolder prázdný. To zajistí, aby ovládací prvek se nezobrazí v levém sloupci na přihlašovací stránku přihlášení.

Po definování obsah `MainContent` a `LeftColumnContent` oblastí, deklarativní vaší přihlašovací stránce by měl vypadat nějak takto:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample5.aspx)]

Obrázek 7 znázorňuje tuto stránku při prohlížení prostřednictvím prohlížeče. Protože tato stránka udává ovládací prvek obsahu pro `QuickLoginUI` ContentPlaceHolder, přepíše výchozí obsahu zadaného na stránce předlohy. Výsledkem je, že ovládací prvek Login zobrazí v zobrazení (viz obrázek 6) není na této stránce vykresleno návrhu stránky předlohy.


[![Na přihlašovací stránku Represses QuickLoginUI ContentPlaceHolder výchozí obsah](multiple-contentplaceholders-and-default-content-cs/_static/image20.png)](multiple-contentplaceholders-and-default-content-cs/_static/image19.png)

**Obrázek 07**: přihlašovací stránce Represses `QuickLoginUI` ContentPlaceHolder jeho výchozí obsah ([kliknutím ji zobrazíte obrázek v plné velikosti](multiple-contentplaceholders-and-default-content-cs/_static/image21.png))


### <a name="using-the-default-content-in-new-pages"></a>Použití výchozí obsah v nové stránky

Chceme zobrazit ovládací prvek Login v levém sloupci pro všechny stránky s výjimkou přihlašovací stránky. Za tím účelem všechny stránky obsahu s výjimkou přihlašovací stránku by měly vynechat, nechte ovládací prvek obsahu pro `QuickLoginUI` ContentPlaceHolder. Vynecháním obsahu ovládacího prvku ContentPlaceHolder jeho výchozí obsah bude použit.

Existujících stránek s obsahem - `Default.aspx`, `About.aspx`, a `MultipleContentPlaceHolders.aspx` -nezahrnují ovládací prvek obsahu pro `QuickLoginUI` vzhledem k tomu, že byly vytvořeny před jsme přidali ovládací prvek ContentPlaceHolder na stránce předlohy. Tyto existujících stránek, proto není potřeba aktualizovat. Ale obsahovat ovládací prvek obsahu pro nové stránky přidat k webu `QuickLoginUI` ContentPlaceHolder ve výchozím nastavení. Proto musíme nezapomeňte odebrat tyto ovládací prvky obsahu pokaždé, když jsme přidejte novou stránku obsahu (Pokud nám chcete přepsat ContentPlaceHolder jeho výchozí obsah, třeba v případě přihlašovací stránky).

Chcete-li odebrat ovládací prvek obsahu, můžete ručně odstranit jeho deklarativním označení ze zobrazení zdroje nebo, v návrhovém zobrazení zvolte výchozí odkaz na hlavní obsah z jeho inteligentních značek. Kterýkoliv přístup odebere ovládací prvek obsahu ze stránky a produkuje stejné net vliv.

Obrázek 8 ukazuje `Default.aspx` při prohlížení prostřednictvím prohlížeče. Vzpomeňte si, že `Default.aspx` má jenom dvě ovládacích prvků obsahu uvedených v jeho deklarativním označení – jeden pro `head` a jeden pro `MainContent`. V důsledku toho výchozí obsah pro `LeftColumnContent` a `QuickLoginUI` prvků ContentPlaceHolder jsou zobrazeny.


[![Zobrazují výchozí obsah pro LeftColumnContent a prvků ContentPlaceHolder QuickLoginUI](multiple-contentplaceholders-and-default-content-cs/_static/image23.png)](multiple-contentplaceholders-and-default-content-cs/_static/image22.png)

**Obrázek 08**: The výchozí obsah pro `LeftColumnContent` a `QuickLoginUI` prvků ContentPlaceHolder zobrazují ([kliknutím ji zobrazíte obrázek v plné velikosti](multiple-contentplaceholders-and-default-content-cs/_static/image24.png))


## <a name="summary"></a>Souhrn

Model hlavní stránky ASP.NET umožňuje libovolný počet prvků ContentPlaceHolder na stránce předlohy. Navíc prvků ContentPlaceHolder zahrnují výchozí obsah, který je vygenerován v případě, že neexistuje žádná odpovídající ovládací prvek na stránce obsahu obsahu. V tomto kurzu jsme viděli, jak přidat další ovládací prvky ContentPlaceHolder na stránce předlohy a definování ovládacích prvků obsahu pro tyto nové prvků ContentPlaceHolder nové i stávající stránek v ASP.NET. Zvažovali jsme i také určení výchozích obsahu v ContentPlaceHolder, což je užitečné v situacích, kde pouze minority stránek, které potřebujete k přizpůsobení opačném případě standardizované obsah v rámci určité oblasti.

V dalším kurzu prozkoumáme `head` ContentPlaceHolder podrobněji, zobrazuje jak prostřednictvím kódu programu a deklarativně definovat názvu, metaznaček a dalších hlaviček HTML na základě stránku po stránce.

Všechno nejlepší programování!

### <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor více ASP/ASP.NET knih a Zakladatel 4GuysFromRolla.com pracuje s Microsoft webových technologií od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami technologie ASP.NET 3.5 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott může být dostupný na adrese [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím na svém blogu [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla Suchi Banerjee. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Předchozí](creating-a-site-wide-layout-using-master-pages-cs.md)
> [další](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)
