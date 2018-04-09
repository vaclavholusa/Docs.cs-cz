---
uid: web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-cs
title: Více ContentPlaceHolders a výchozí obsah (C#) | Microsoft Docs
author: rick-anderson
description: Prozkoumá, jak přidat více obsahu zástupného do hlavní stránky a také jak určit výchozí obsah v obsahu zástupného.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: b9b9798b-027d-46cc-9636-473378e437ac
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-cs
msc.type: authoredcontent
ms.openlocfilehash: b60017c21b4cf45081893af08e68186009475fd2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="multiple-contentplaceholders-and-default-content-c"></a>Více ContentPlaceHolders a výchozí obsah (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_02_CS.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_02_CS.pdf)

> Prozkoumá, jak přidat více obsahu zástupného do hlavní stránky a také jak určit výchozí obsah v obsahu zástupného.


## <a name="introduction"></a>Úvod

V předchozím kurzu jsme se zaměřili na tom, jak hlavní stránky povolit vývojáře využívající technologii ASP.NET k vytvoření konzistentního rozložení pro celý web. Stránky předlohy definují značky, které jsou společné pro všechny její stránky obsahu a oblastí, které jsou přizpůsobitelné na základě po stránkách. V předchozích kurzu jsme vytvořili jednoduché stránky předlohy (`Site.master`) a dva obsahu stránky (`Default.aspx` a `About.aspx`). Naše stránky předlohy se skládal z dva ContentPlaceHolders s názvem `head` a `MainContent`, které se nacházely v `<head>` elementu a webového formuláře v uvedeném pořadí. Při obsahu stránky měl dvou ovládacích prvků obsahu, jsme zadali pouze kód pro jeden odpovídající `MainContent`.

Jak dokládá pomocí dvou ovládacích prvků ContentPlaceHolder v `Site.master`, hlavní stránky může obsahovat více ContentPlaceHolders. Navíc může hlavní stránce zadejte výchozí kód pro obsahuje rozložení. Stránky obsahu, pak můžete volitelně zadejte vlastní značky nebo použít výchozí značka. V tomto kurzu jsme podívejte se na použití více ovládacích prvků obsahu na hlavní stránce a zjistit, jak definovat výchozí značek v obsahuje rozložení.

## <a name="step-1-adding-additional-contentplaceholder-controls-to-the-master-page"></a>Krok 1: Přidání ovládacích prvků další ContentPlaceHolder stránky předlohy

Řada návrhů webu obsahovat několik oblasti na obrazovce, které jsou přizpůsobené na základě po stránkách. `Site.master`, hlavní stránky jsme vytvořili v předchozím kurzu, obsahuje jeden ContentPlaceHolder v rámci webového formuláře s názvem `MainContent`. Konkrétně se nachází v rámci této ContentPlaceHolder `mainContent` `<div>` element.

Obrázek 1 zobrazuje `Default.aspx` při zobrazení prostřednictvím prohlížeče. Oblast v kroužku červeně je kód specifické pro stránku odpovídající `MainContent`.


[![Oblasti v kroužku ukazuje oblasti aktuálně přizpůsobitelné na základě po stránkách](multiple-contentplaceholders-and-default-content-cs/_static/image2.png)](multiple-contentplaceholders-and-default-content-cs/_static/image1.png)

**Obrázek 01**: oblasti Circled ukazuje oblasti aktuálně přizpůsobit na základě po stránkách ([Kliknutím zobrazit obrázek v plné velikosti](multiple-contentplaceholders-and-default-content-cs/_static/image3.png))


Představte si, že kromě oblast na obrázku 1, také je potřeba přidat položky specifické pro stránku na levém sloupci pod lekce a zprávy oddíly. K tomu, jsme přidejte další ovládací prvek ContentPlaceHolder stránky předlohy. Chcete-li sledovat, otevřete `Site.master` hlavní stránky v aplikaci Visual Web Developer a poté přetáhněte ovládací prvek ContentPlaceHolder z panelu nástrojů na návrháře po části zprávy. Nastavte ContentPlaceHolder `ID` k `LeftColumnContent`.


[![Přidání ovládacího prvku ContentPlaceHolder na levém sloupci stránky předlohy](multiple-contentplaceholders-and-default-content-cs/_static/image5.png)](multiple-contentplaceholders-and-default-content-cs/_static/image4.png)

**Obrázek 02**: Přidání ovládacího prvku ContentPlaceHolder sloupec levé straně stránky předlohy ([Kliknutím zobrazit obrázek v plné velikosti](multiple-contentplaceholders-and-default-content-cs/_static/image6.png))


Po přidání `LeftColumnContent` ContentPlaceHolder k hlavní stránce jsme můžete definovat obsah pro tuto oblast na základě po stránkách zahrnutím obsahu řízení na stránce, jehož `ContentPlaceHolderID` je nastaven na `LeftColumnContent`. Jsme Zkontrolujte tento proces v kroku 2.

## <a name="step-2-defining-content-for-the-new-contentplaceholder-in-the-content-pages"></a>Krok 2: Definování obsah pro nové ContentPlaceHolder v obsahu stránky

Při přidávání nové stránky obsahu na web, aplikaci Visual Web Developer automaticky vytvoří obsah ovládacího prvku na stránce pro každý ContentPlaceHolder ve vybrané stránky předlohy. S přidat `LeftColumnContent` ContentPlaceHolder na hlavní stránku v kroku 1, nové na ASP.NET stránky bude nyní mají tři ovládací prvky obsahu.

Pro znázornění je přidat novou stránku obsahu na kořenový adresář s názvem `MultipleContentPlaceHolders.aspx` která je vázaná `Site.master` stránky předlohy. Visual Web Developer vytvoří tato stránka se následující kód:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample1.aspx)]

Zadejte nějaký obsah do odkazující na ovládací prvek obsahu `MainContent` ContentPlaceHolders (`Content2`). Dál přidejte následující kód do `Content3` obsahu ovládacího prvku (které odkazy `LeftColumnContent` ContentPlaceHolder):

[!code-html[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample2.html)]

Po přidání tento kód, najdete na stránce prostřednictvím prohlížeče. Jak je vidět na obrázku 3, kód uložena v umístění `Content3` obsahu ovládacího prvku se zobrazí v levém sloupci pod části zprávy (v kroužku červeně). Bude značka umístěna v `Content2` se zobrazí v pravé části stránky (v kroužku modře).


[![V levém sloupci nyní zahrnuje obsahu stránce pod části zprávy](multiple-contentplaceholders-and-default-content-cs/_static/image8.png)](multiple-contentplaceholders-and-default-content-cs/_static/image7.png)

**Obrázek 03**: V levé sloupec nyní zahrnuje specifické pro stránku obsahu pod zprávy část ([Kliknutím zobrazit obrázek v plné velikosti](multiple-contentplaceholders-and-default-content-cs/_static/image9.png))


### <a name="defining-content-in-existing-content-pages"></a>Definování obsahu v existující stránky obsahu

Vytvoření nové stránky obsahu automaticky zahrnuje ovládací prvek ContentPlaceHolder, kterou jsme přidali v kroku 1. Ale dvě existujících stránek s obsahem - `About.aspx` a `Default.aspx` -nemají obsahu ovládací prvek pro `LeftColumnContent` ContentPlaceHolder. Zadat obsah pro tento ContentPlaceHolder v těchto dvou existující stránky, je potřeba přidat ovládací prvek obsahu sebe.

Na rozdíl od většiny ovládacích prvků technologie ASP.NET v panelu nástrojů Visual Web Developer neobsahuje položku obsahu ovládacího prvku. Jsme ručně zadat kód deklarativní obsahu ovládacího prvku do zobrazení zdroje, ale snadnější a rychlejší přístup je použít zobrazení návrhu. Otevřete `About.aspx` stránky a přepněte do zobrazení návrhu. Jak ukazuje obrázek 4, `LeftColumnContent` ContentPlaceHolder se zobrazí v zobrazení návrhu; Pokud jste myši nad ním, načte nadpis zobrazovaný: "LeftColumnContent (Master)." Zahrnutí "Hlavní" v názvu označuje, že je žádný obsahu ovládací prvek pro tento ContentPlaceHolder na stránce definován. Pokud existuje obsah ovládacího prvku pro ContentPlaceHolder, jako v případě `MainContent`, bude číst název: "*ContentPlaceHolderID* (vlastní)."

Přidání obsahu ovládacího prvku pro `LeftColumnContent` ContentPlaceHolder k `About.aspx`, rozbalte ContentPlaceHolder inteligentních značek a klikněte na odkaz vytvořit vlastní obsah.


[![Zobrazení návrhu pro About.aspx uvádí LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-cs/_static/image11.png)](multiple-contentplaceholders-and-default-content-cs/_static/image10.png)

**Obrázek 04**: zobrazení návrhu pro `About.aspx` ukazuje `LeftColumnContent` ContentPlaceHolder ([Kliknutím zobrazit obrázek v plné velikosti](multiple-contentplaceholders-and-default-content-cs/_static/image12.png))


Kliknutím na odkaz na vytvořit vlastní obsah generuje nezbytné obsahu ovládacího prvku na stránku a nastaví její `ContentPlaceHolderID` vlastnost, která má ContentPlaceHolder `ID`. Například klepnutím na odkaz na vytvořit vlastní obsah `LeftColumnContent` oblast v `About.aspx` přidá následující deklarativní na stránku:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample3.aspx)]

### <a name="omitting-content-controls"></a>Vynechání ovládací prvky obsahu

ASP.NET nevyžaduje, aby všechny obsahu stránky obsahují ovládací prvky obsahu pro každou jednotlivou ContentPlaceHolder na hlavní stránce definován. Pokud je vynechán ovládací prvek obsahu, modul ASP.NET používá značku definované v rámci ContentPlaceHolder na hlavní stránce. Tento kód se označuje jako ContentPlaceHolder *výchozí obsah* a je užitečný ve scénářích, kde obsah pro některé oblasti je běžné mezi většina stránek, ale je potřeba přizpůsobit pro malý počet stránek. Krok 3 prozkoumá zadání výchozí obsah na hlavní stránce.

V současné době `Default.aspx` obsahuje dva ovládací prvky obsahu pro `head` a `MainContent` ContentPlaceHolders; nemá obsahu ovládací prvek pro `LeftColumnContent`. V důsledku toho, když `Default.aspx` je vykreslen `LeftColumnContent` se používá pro ContentPlaceHolder výchozí obsah. Vzhledem k tomu, že ještě musíme definovat veškerý obsah, výchozí pro tento ContentPlaceHolder, net efekt je, že je pro tuto oblast vygenerované žádný kód. Chcete-li ověřit, toto chování, navštivte `Default.aspx` prostřednictvím prohlížeče. Jak je vidět na obrázku 5, je v levém sloupci pod části zprávy vygenerované žádný kód.


[![Pro LeftColumnContent ContentPlaceHolder není vykreslován žádný obsah](multiple-contentplaceholders-and-default-content-cs/_static/image14.png)](multiple-contentplaceholders-and-default-content-cs/_static/image13.png)

**Obrázek 05**: je vykreslen žádný obsah `LeftColumnContent` ContentPlaceHolder ([Kliknutím zobrazit obrázek v plné velikosti](multiple-contentplaceholders-and-default-content-cs/_static/image15.png))


## <a name="step-3-specifying-default-content-in-the-master-page"></a>Krok 3: Určení obsahu výchozí stránka předlohy

Některé návrhy webu zahrnují oblast, jejichž obsah je stejný pro všechny stránky webu s výjimkou jedno nebo dvě výjimky. Vezměte v úvahu web, který podporuje uživatelské účty. Takový web vyžaduje přihlašovací stránku, kde návštěvníci můžete zadat své přihlašovací údaje pro přihlášení do lokality. Urychlit v procesu přihlašování, jsou webovým vývojářům mohou zahrnovat textových polí uživatelské jméno a heslo v levém horním rohu každé stránky, aby uživatelé mohli přihlásit bez nutnosti explicitně najdete na stránce přihlášení. Tyto uživatelské jméno a heslo textová pole jsou užitečné v většina stránek, jsou redundantní na přihlašovací stránce, která již obsahuje textových polí pro přihlašovací údaje uživatele.

Při implementaci tohoto návrhu, můžete vytvořit prvek ContentPlaceHolder v levém horním rohu stránky předlohy. Každé stránce, které jsou nutné pro zobrazení textových polí uživatelské jméno a heslo v jejich levém horním rohu by vytvoření ovládacího prvku obsahu pro tento ContentPlaceHolder a přidejte nezbytné rozhraní. Na přihlašovací stránku, na druhé straně by buď vynechejte přidání ovládacího prvku obsahu pro tento ContentPlaceHolder nebo by vytvořit obsah ovládacího prvku pomocí žádné značek definovaný. Nevýhodou tohoto přístupu je, že se musí nezapomeňte přidat textových polí uživatelské jméno a heslo na každé stránce, které jsme přidat na web (s výjimkou přihlašovací stránku). To se žádostí o řešení problémů. Nemohli jsme pravděpodobně zapomenete přidat tyto textových polí na stránce nebo dva nebo horší, jsme nemusí implementovat rozhraní správně (třeba přidání jedním textbox místo dvou).

Lepší řešení je jako výchozí obsah ContentPlaceHolder definovat textových polí uživatelské jméno a heslo. Díky tomu musíme pouze přepsat toto výchozí obsah v těchto několik stránek, které nezobrazují textových polí uživatelské jméno a heslo (přihlašovací stránce pro instanci). Pro ilustraci zadání výchozí obsah ovládacího prvku ContentPlaceHolder, můžeme implementovat scénář právě popsané.

> [!NOTE]
> Zbývající část tohoto kurzu aktualizuje naše webové stránky a patří rozhraní přihlášení v levém sloupci pro všechny stránky, ale na přihlašovací stránku. V tomto kurzu však není Zkontrolujte konfiguraci webu pro podporu uživatelské účty. Další informace v tomto tématu najdete v části Moje [ověřování pomocí formulářů, ověřování, uživatelské účty a role](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) kurzy.


### <a name="adding-a-contentplaceholder-and-specifying-its-default-content"></a>Přidání ContentPlaceHolder a zadání jeho výchozí obsah

Otevřete `Site.master` hlavní stránky a přidejte následující kód do levého sloupce mezi `DateDisplay` popisek a lekce části:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample4.aspx)]

Po přidání tento kód hlavní stránky zobrazení návrhu by měl vypadat na obrázku 6.


[![Stránky předlohy obsahuje ovládací prvek přihlášení](multiple-contentplaceholders-and-default-content-cs/_static/image17.png)](multiple-contentplaceholders-and-default-content-cs/_static/image16.png)

**Obrázek 06**: stránky předlohy obsahuje ovládací prvek přihlášení ([Kliknutím zobrazit obrázek v plné velikosti](multiple-contentplaceholders-and-default-content-cs/_static/image18.png))


Tato ContentPlaceHolder `QuickLoginUI`, má ovládací prvek webového přihlášení jako jeho výchozí obsah. Ovládací prvek pro přihlášení zobrazí uživatelské rozhraní, které výzvu k zadání uživatelského jména a hesla společně s tlačítko přihlásit. Po kliknutí na tlačítko Přihlásit ovládací prvek pro přihlášení interně ověřuje přihlašovací údaje uživatele členství rozhraní API. Pokud chcete použít tento ovládací prvek přihlášení v praxi, pak musíte nakonfigurovat webový server pomocí členství. Toto téma je nad rámec tohoto kurzu; odkazovat na můj [ověřování pomocí formulářů, ověřování, uživatelské účty a role](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) kurzy pro další informace o vytváření webové aplikace, která podporuje uživatelské účty.

Nebojte se, že přizpůsobení chování nebo vzhledu ovládacího prvku přihlášení. I nastavili dva jeho vlastnosti: `TitleText` a `FailureAction`. `TitleText` Hodnotu vlastnosti, který se standardně "Přihlášení", se zobrazí v horní části uživatelské rozhraní ovládacího prvku. Tak, aby se zobrazí jako text "Přihlásit" Tato vlastnost nastavili `<h3>` elementu. `FailureAction` Vlastnost určuje, co dělat, když přihlašovací údaje uživatele jsou neplatné. Je standardně nastavena na hodnotu `Refresh`, který nechá uživatele na stejné stránce a zobrazí se zpráva selhání v rámci ovládací prvek pro přihlášení. Po změně její `RedirectToLoginPage`, který odešle uživatele na přihlašovací stránku v případě neplatné přihlašovací údaje. Nechci odesílat uživatele na přihlašovací stránku, když se uživatel pokusí přihlásit se z některé jinou stránku, ale selže, protože přihlašovací stránky může obsahovat další pokyny a možnosti, které by vyhovovaly snadno v levém sloupci. Například přihlašovací stránky může zahrnovat možnosti obnovit zapomenuté heslo, nebo vytvořit nový účet.

### <a name="creating-the-login-page-and-overriding-the-default-content"></a>Vytváření přihlašovací stránku a přepsáním výchozí obsah

Hlavní stránky dokončení naše dalším krokem je vytvoření přihlašovací stránky. Přidat stránku ASP.NET do vašeho webu kořenový adresář s názvem `Login.aspx`, jeho vazby `Site.master` stránky předlohy. Tím se vytvoří na stránce s čtyři ovládací prvky obsahu, jeden pro každou ContentPlaceHolders definované v `Site.master`.

Přidání ovládacího prvku přihlášení k `MainContent` obsahu ovládacího prvku. Podobně libosti přidat žádný obsah k `LeftColumnContent` oblast. Ale nezapomeňte ponechat obsahu ovládací prvek pro `QuickLoginUI` ContentPlaceHolder prázdný. To zajistí přihlášení ovládací prvek se nezobrazí v levém sloupci přihlašovací stránky.

Po definování obsah `MainContent` a `LeftColumnContent` oblastí, deklarativní vaší přihlašovací stránce by měl vypadat takto:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample5.aspx)]

Obrázek 7 znázorňuje tuto stránku při zobrazení prostřednictvím prohlížeče. Protože tato stránka udává obsahu ovládací prvek pro `QuickLoginUI` ContentPlaceHolder, přepíše výchozí obsah určené na hlavní stránce. Net efekt je, že ovládací prvek pro přihlášení se zobrazí v návrhu stránky předlohy (viz obrázek 6) není v této stránce vykreslit zobrazení.


[![Na přihlašovací stránku Represses QuickLoginUI ContentPlaceHolder výchozí obsah](multiple-contentplaceholders-and-default-content-cs/_static/image20.png)](multiple-contentplaceholders-and-default-content-cs/_static/image19.png)

**Obrázek 07**: Represses The přihlašovací stránky `QuickLoginUI` ContentPlaceHolder na výchozí obsahu ([Kliknutím zobrazit obrázek v plné velikosti](multiple-contentplaceholders-and-default-content-cs/_static/image21.png))


### <a name="using-the-default-content-in-new-pages"></a>Použití výchozí obsah v nové stránky

Chceme zobrazit ovládací prvek pro přihlášení v levém sloupci pro všechny stránky s výjimkou přihlašovací stránku. Jak toho docílit, musí všechny stránky obsahu s výjimkou přihlašovací stránku vynechejte obsahu ovládací prvek pro `QuickLoginUI` ContentPlaceHolder. Díky vynechání ovládací prvek obsahu, bude místo toho použít ContentPlaceHolder výchozí obsah.

Existujících stránek s obsahem - `Default.aspx`, `About.aspx`, a `MultipleContentPlaceHolders.aspx` -nezahrnují obsahu ovládací prvek pro `QuickLoginUI` vzhledem k tomu, že byly vytvořeny před jsme přidali tento prvek ContentPlaceHolder na hlavní stránku. Tyto existující stránky proto není potřeba aktualizovat. Však nové stránky na web přidat obsahují obsahu ovládací prvek pro `QuickLoginUI` ContentPlaceHolder ve výchozím nastavení. Máme proto nezapomeňte odebrat tyto ovládací prvky obsahu pokaždé, když přidáme nové stránky obsahu (Pokud nám chcete přepsat ContentPlaceHolder výchozí obsah, jako v případě přihlašovací stránky).

Pokud chcete odebrat obsah ovládacího prvku, můžete ručně odstranit její deklarativní ze zobrazení zdroje nebo, ze zobrazení návrhu, zvolte výchozí do hlavního serveru odkaz na obsah z jeho inteligentních značek. Buď přístup odebere ovládacího prvku obsahu ze stránky a vytváří stejný net vliv.

Obrázek 8 ukazuje `Default.aspx` při zobrazení prostřednictvím prohlížeče. Odvolat, `Default.aspx` má jenom dvě ovládací prvky obsahu zadané v jeho deklarativní – jednu pro `head` a jeden pro `MainContent`. V důsledku toho obsahu pro výchozí hodnota `LeftColumnContent` a `QuickLoginUI` ContentPlaceHolders jsou zobrazeny.


[![Se zobrazí výchozí obsahu pro LeftColumnContent a QuickLoginUI ContentPlaceHolders](multiple-contentplaceholders-and-default-content-cs/_static/image23.png)](multiple-contentplaceholders-and-default-content-cs/_static/image22.png)

**Obrázek 08**: výchozí obsah pro `LeftColumnContent` a `QuickLoginUI` ContentPlaceHolders zobrazují ([Kliknutím zobrazit obrázek v plné velikosti](multiple-contentplaceholders-and-default-content-cs/_static/image24.png))


## <a name="summary"></a>Souhrn

Model hlavní stránky ASP.NET umožňuje pro libovolný počet ContentPlaceHolders na hlavní stránce. Co je další, ContentPlaceHolders zahrnují výchozí obsah, který je vygenerované v případě, že neexistuje žádná odpovídající obsah ovládacího prvku obsahu stránce. V tomto kurzu jsme viděli, jak se zahrnuje další ovládací prvky ContentPlaceHolder na hlavní stránce a definování ovládacích prvků obsahu pro tyto nové ContentPlaceHolders v nové i existující stránky ASP.NET. Můžeme také prohlédli určení výchozího obsahu v ContentPlaceHolder, což je užitečné v situacích, kde pouze výjimečných stránky potřeby přizpůsobit jinak standardizované obsah v určité oblasti.

V dalším kurzu podíváme `head` ContentPlaceHolder podrobněji, zobrazuje jak deklarativně a programově definovat název značky meta pro a jiné hlavičky HTML na základě po stránkách.

Radostí programování!

### <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor více knih ASP/ASP.NET a zakladatele 4GuysFromRolla.com, pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologie ASP.NET 3.5 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott lze dosáhnout za [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu v [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl Suchi Banerjee. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Předchozí](creating-a-site-wide-layout-using-master-pages-cs.md)
> [další](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)
