---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-cs
title: "Interakci se stránkou předlohy ze stránky obsahu (C#) | Microsoft Docs"
author: rick-anderson
description: "Prozkoumá tom, jak volat metody, nastavte vlastnosti, atd. hlavní stránky z kódu na stránce obsahu."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: 32d54638-71b2-491d-81f4-f7417a13a62f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 977cdea38d240bcae284968de7d780ec59ab6dfd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="interacting-with-the-master-page-from-the-content-page-c"></a>Interakci se stránkou předlohy ze stránky obsahu (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_06_CS.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_06_CS.pdf)

> Prozkoumá tom, jak volat metody, nastavte vlastnosti, atd. hlavní stránky z kódu na stránce obsahu.


## <a name="introduction"></a>Úvod

V průběhu posledních pět kurzy mít jsme se podívali na tom, jak vytvořit stránku předlohy, definovat oblasti obsahu, vazby stránek ASP.NET na stránku předlohy a specifické pro stránku obsahu. Pokud návštěvník požádá o konkrétní stránky obsahu, obsah a kód hlavní stránky jsou začleněny za běhu, což vede k vykreslování hierarchie jednotné řízení. Proto jsme již viděli jeden způsobem v, které mohou komunikovat stránky předlohy a jeden z jeho obsahu stránky: obsahu stránce vidět značky k transfuse do ovládacích prvků ContentPlaceHolder stránky předlohy.

Co se musí ještě zkontrolujte je, jak stránky předlohy a stránky obsahu mohou komunikovat prostřednictvím kódu programu. Kromě definování značek pro ovládací prvky ContentPlaceHolder stránky předlohy, stránky obsahu můžete přiřadit hodnoty k hlavní stránce veřejné vlastnosti a vyvolání jeho veřejné metody. Podobně hlavní stránky může komunikovat s jeho obsahem stránky. Sice méně častých než interakce mezi jejich deklarativní označení programové interakce mezi hlavní a obsahu stránce, existuje mnoho scénářů, kde je potřeba takový programové interakce.

V tomto kurzu jsme zkontrolujte, jak mohou stránku obsahu prostřednictvím kódu programu komunikovat s jeho stránky předlohy; v dalším kurzu se podíváme na tom, jak mohou stránky předlohy podobně komunikovat s jeho obsahu stránky.

## <a name="examples-of-programmatic-interaction-between-a-content-page-and-its-master-page"></a>Příklady programové interakce mezi stránky obsahu a jeho stránky předlohy

Konkrétní oblast stránky musí být konfigurováno na základě po stránkách, použijeme ContentPlaceHolder ovládacího prvku. Ale co o situacích, kdy se většina stránek zapotřebí pro vydávání určité výstup, ale malý počet stránek muset změnit a zobrazit něco jiného? Jeden takový příklad, který jsme se zaměřili [ *více ContentPlaceHolders a výchozí obsahu* ](multiple-contentplaceholders-and-default-content-cs.md) kurzu zahrnuje zobrazení rozhraní přihlášení na každé stránce. Zatímco většina stránek by měla obsahovat rozhraní přihlášení, se má výjimka potlačit pro několik stránky, jako například: hlavní přihlašovací stránky (`Login.aspx`); na stránce vytvořit účet; a další stránky, které jsou přístupné pro ověřené uživatele. [ *Více ContentPlaceHolders a výchozí obsah* ](multiple-contentplaceholders-and-default-content-cs.md) kurz vám ukázal, jak definovat výchozí obsah pro ContentPlaceHolder na hlavní stránce a pak stránky postupu přepsání v těch, kde výchozí obsah nebyl chtěli.

Další možností je vytvoření veřejné vlastnosti nebo metody v rámci stránky předlohy, která určuje, jestli se mají zobrazit či skrýt rozhraní přihlášení. Například stránky předlohy mohou zahrnovat veřejná vlastnost s názvem `ShowLoginUI` jehož hodnota byl použitý při instalaci `Visible` vlastnost ovládací prvek pro přihlášení na hlavní stránce. Potom prostřednictvím kódu programu nastavit tyto obsahu stránky, kde má být potlačeno uživatelského rozhraní pro přihlášení `ShowLoginUI` vlastnost `false`.

Nejčastější obsahu a interakce stránky předlohy pravděpodobně nastane, když v je nutné aktualizovat po některé akce vypršel obsahu stránce stránky předlohy zobrazí data. Vezměte v úvahu, že hlavní stránky, který zahrnuje GridView, která zobrazuje pět naposledy přidat záznamy z konkrétní databázové tabulky a že jeden z jeho stránky obsahu zahrnuje rozhraní pro přidání nových záznamů do stejné tabulky.

Pokud uživatel navštíví stránku a přidejte nový záznam, uživatel uvidí, že pěti naposledy přidat záznamy zobrazené na hlavní stránce. Po vyplnění hodnoty pro sloupce nový záznam, uživatel formulář odešle. Za předpokladu, že má rutina GridView na hlavní stránce jeho `EnableViewState` vlastností nastavenou na hodnotu true (výchozí), jeho obsah je znovu načíst ze zobrazení stavu a v důsledku toho se zobrazí pět stejné záznamy, i když novější záznam byla právě přidána do databáze. To může zmást uživatele.

> [!NOTE]
> I v případě, že stav zobrazení GridView zakážete tak, aby znovu připojí k jeho základní zdroj dat na každé zpětné volání, stále nezobrazí záznam právě přidané protože data je vázána na GridView dříve v průběhu životního cyklu stránky než při přidání nového záznamu do datab App Service Environment.


Chcete-li to opravit tak, aby na hlavní stránce se zobrazí právě přidat záznam adresy GridView na zpětné volání, potřebujeme dáte pokyn, aby GridView se svázat svůj zdroj dat *po* nový záznam se přidal do databáze. To vyžaduje interakci mezi obsah a hlavní stránky, protože je rozhraní pro přidávání nového záznamu (a její obslužné rutiny událostí) jsou v obsahu stránce, ale GridView, který je třeba aktualizovat stránku v stránky předlohy.

Protože aktualizovat zobrazení stránky předlohy z obslužné rutiny události na stránce obsahu je jedním z nejčastějších potřeby obsah a interakce stránky předlohy, podíváme se na toto téma podrobněji. Ke stažení pro tento kurz zahrnuje databáze Microsoft SQL Server 2005 Express Edition s názvem `NORTHWIND.MDF` na webu `App_Data` složky. Databáze Northwind ukládá produktu, zaměstnance a informace o prodeji pro fiktivní společnosti Northwind Traders.

Krok 1 nevystavíte slabé stránky zabezpečení prostřednictvím zobrazení pěti naposledy přidat produkty v GridView na hlavní stránce. Krok 2 vytvoří obsahu stránce pro přidání nové produkty. Krok 3 porovná vytvoření veřejné vlastnosti a metody na hlavní stránce a krok 4 ukazuje, jak se prostřednictvím kódu programu rozhraní s tyto vlastnosti a metody ze stránky obsahu.

> [!NOTE]
> V tomto kurzu není pustíte do specifikace práci s daty v technologii ASP.NET. Kroky pro vytvoření stránky předlohy pro zobrazení dat a obsahu stránce pro vkládání dat jsou kompletní, ještě breezy. Pro podrobnější pohled na zobrazení a vkládání dat a použití ovládacích prvků SqlDataSource a GridView najdete v části Další odečty prostředky na konci tohoto kurzu.


## <a name="step-1-displaying-the-five-most-recently-added-products-in-the-master-page"></a>Krok 1: Zobrazení pět naposledy přidat produkty stránka předlohy

Otevřete `Site.master` hlavní stránky a přidejte štítky a ovládacího prvku GridView k `leftContent` `<div>`. Zrušte si jeho `Text` vlastnost, nastavte jeho `EnableViewState` vlastnost na hodnotu false a jeho `ID` vlastnost `GridMessage`; nastavit prvku GridView `ID` vlastnost `RecentProducts`. Z návrháře, dále rozbalte prvku GridView inteligentních značek a vyberte pro vytvoření vazby ke zdroji dat nové. Spustí se Průvodce konfigurací zdroje dat služby. Vzhledem k tomu, že databáze Northwind `App_Data` složky je databáze Microsoft SQL Server, můžete vytvořit SqlDataSource výběrem (viz obrázek 1); název SqlDataSource `RecentProductsDataSource`.


[![Vytvoření vazby GridView do ovládacího prvku SqlDataSource s názvem RecentProductsDataSource](interacting-with-the-master-page-from-the-content-page-cs/_static/image2.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image1.png)

**Obrázek 01**: vazby na ovládací prvek SqlDataSource s názvem GridView `RecentProductsDataSource` ([Kliknutím zobrazit obrázek v plné velikosti](interacting-with-the-master-page-from-the-content-page-cs/_static/image3.png))


Dalším krokem požádá nám určit, co se připojit k databázi. Vyberte `NORTHWIND.MDF` databáze soubor z rozevíracího seznamu a klikněte na tlačítko Další. Protože je prvním použili jsme tuto databázi, průvodce bude nabízet uložit připojovací řetězec v `Web.config`. Jej uložit připojovací řetězec pomocí názvu `NorthwindConnectionString`.


[![Připojení k databázi Northwind](interacting-with-the-master-page-from-the-content-page-cs/_static/image5.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image4.png)

**Obrázek 02**: připojení k databázi Northwind ([Kliknutím zobrazit obrázek v plné velikosti](interacting-with-the-master-page-from-the-content-page-cs/_static/image6.png))


Průvodce konfigurace zdroje dat poskytuje dva prostředky, které lze zadat dotaz, použít k načtení dat:

- Zadáním vlastního příkazu SQL nebo uloženou proceduru nebo
- Výběr tabulku nebo zobrazení a potom zadáte sloupce, které chcete vrátit

Vzhledem k tomu, že chceme vrátit pouze pět naposledy přidat produktů, je potřeba zadat vlastní příkaz SQL. Použijte následující vyberte dotaz:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample1.sql)]

`TOP 5` – Klíčové slovo vrátí pouze prvních pět záznamů z dotazu. `Products` Primární klíč tabulky `ProductID`, je `IDENTITY` sloupec, který nám zaručuje, že každý nový produkt přidán do tabulky budou mít větší hodnotu než předchozí položce. Proto řazení výsledků podle `ProductID` v sestupném pořadí vrátí produkty od verze naposledy vytvořený ty.


[![Vrátí pět nedávno přidané produkty](interacting-with-the-master-page-from-the-content-page-cs/_static/image8.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image7.png)

**Obrázek 03**: vrátit pět nejvíce nedávno přidán produkty ([Kliknutím zobrazit obrázek v plné velikosti](interacting-with-the-master-page-from-the-content-page-cs/_static/image9.png))


Po dokončení průvodce, Visual Studio vytvoří dvě BoundFields pro GridView zobrazíte `ProductName` a `UnitPrice` pole vrácená z databáze. Hlavní stránka deklarativní v tomto okamžiku by měla obsahovat značek podobný následujícímu:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample2.aspx)]

Jak vidíte, obsahuje kód: ovládací prvek popisek webu (`GridMessage`); GridView `RecentProducts`, s dvěma BoundFields; a SqlDataSource ovládací prvek, který vrátí pěti naposledy přidat produkty.

Tato rutina GridView vytvořen a jeho prvek SqlDataSource nakonfigurované přejděte na webovou stránku prostřednictvím prohlížeče. Jak ukazuje obrázek 4, zobrazí se, že mřížky v levém dolním rohu, které uvádí pět naposledy přidat produkty.


[![GridView zobrazí pět nedávno přidané produkty](interacting-with-the-master-page-from-the-content-page-cs/_static/image11.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image10.png)

**Obrázek 04**: GridView zobrazí pět nejvíce nedávno přidán produktů ([Kliknutím zobrazit obrázek v plné velikosti](interacting-with-the-master-page-from-the-content-page-cs/_static/image12.png))


> [!NOTE]
> Nebojte se, že vyčištění vzhled GridView. Některé návrhy zahrnovat formátování zobrazené `UnitPrice` hodnotu jako měny a pomocí písma a barvy pozadí k vylepšení vzhledu mřížky.


## <a name="step-2-creating-a-content-page-to-add-new-products"></a>Krok 2: Vytvoření stránky obsahu přidat nové produkty

Naše dalším krokem je vytvoření obsahu stránce, ze kterého uživatel může přidat nového produktu na `Products` tabulky. Přidat novou stránku obsahu, aby `Admin` složku s názvem `AddProduct.aspx`, které opravdu pro vytvoření vazby na `Site.master` stránky předlohy. Obrázek 5 ukazuje Průzkumník řešení po přidání této stránky na web.


[![Přidat novou stránku ASP.NET ke složce Admin](interacting-with-the-master-page-from-the-content-page-cs/_static/image14.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image13.png)

**Obrázek 05**: Přidat novou stránku ASP.NET do `Admin` složky ([Kliknutím zobrazit obrázek v plné velikosti](interacting-with-the-master-page-from-the-content-page-cs/_static/image15.png))


Odvolat, že [ *zadáte název, značky Meta a ostatní hlavičky HTML na hlavní stránce* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) kurzu jsme vytvořili vlastní základní stránky třídy s názvem `BasePage` , pokud byla vygenerována nadpis stránky není explicitně nastaven. Přejděte na `AddProduct.aspx` kódu stránky třídy a ji odvodit z `BasePage` (místo z `System.Web.UI.Page`).

Nakonec aktualizujte `Web.sitemap` souboru záznam pro tento účel. Přidejte následující kód pod `<siteMapNode>` pro lekce řízení ID pojmenování problémy:


[!code-xml[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample3.xml)]

Jak je znázorněno na obrázku 6, přidání tohoto `<siteMapNode>` element se odrazí v seznamu lekce.

Vraťte se do `AddProduct.aspx`. V ovládacím prvku obsahu pro `MainContent` ContentPlaceHolder, přidání ovládacího prvku DetailsView a pojmenujte ji `NewProduct`. Vytvoření vazby DetailsView do ovládacího prvku nové SqlDataSource s názvem `NewProductDataSource`. Jako s SqlDataSource v kroku 1, nakonfigurovat průvodce tak, aby používala databázi Northwind a zvolte zadat vlastní příkaz SQL. Protože DetailsView se použije k přidávání položek do databáze, je potřeba zadat oba seznamy `SELECT` příkaz a `INSERT` příkaz. Použijte následující `SELECT` dotazu:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample4.sql)]

Potom na kartě Vložení přidejte následující `INSERT` příkaz:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample5.sql)]

Po dokončení Průvodce přejít na ovládacím prvku DetailsView inteligentních značek a zaškrtnutím políčka "Povolit vložení". Tento postup přidá CommandField do DetailsView s jeho `ShowInsertButton` vlastností nastavenou na hodnotu true. Vzhledem k tomu tento DetailsView se použije výhradně pro vkládání dat, nastavit DetailsView `DefaultMode` vlastnost `Insert`.

To je všechno je k němu! Tato stránka umožňuje otestovat. Navštivte `AddProduct.aspx` prostřednictvím prohlížeče, zadejte název a cena (viz obrázek 6).


[![Přidání nového produktu do databáze](interacting-with-the-master-page-from-the-content-page-cs/_static/image17.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image16.png)

**Obrázek 06**: Přidání nového produktu k databázi ([Kliknutím zobrazit obrázek v plné velikosti](interacting-with-the-master-page-from-the-content-page-cs/_static/image18.png))


Po zadání názvu a ceny pro nového produktu, klikněte na tlačítko Vložit. To způsobí, že formulář odeslat zpět. Na zpětné volání, ovládacího prvku SqlDataSource na `INSERT` spustit příkaz; jeho dva parametry se naplní hodnot zadaných uživatelem v ovládacím prvku DetailsView dvou ovládacích prvků textového pole. Bohužel neexistuje žádné visual zpětnou vazbu, která typu vložení došlo k chybě. Je dobrý tak, aby měl zpráva, potvrzení, že byl přidán nový záznam. Nechat to jako cvičení pro čtečku. Také po přidání nového záznamu z DetailsView GridView na hlavní stránce se zobrazuje stejné pět záznamů jako před; neobsahuje právě přidat záznam. Podíváme, jak chcete-li to opravit v nadcházející krocích.

> [!NOTE]
> Kromě přidání určitou formu visual zpětnou vazbu, která vložení proběhla úspěšně, I by doporučujeme, abyste také aktualizovat rozhraní vložení prvku DetailsView zahrnout ověření. V současné době není k dispozici žádné ověření. Pokud uživatel zadá neplatnou hodnotu pro `UnitPrice` pole, například "příliš nákladné," na zpětné volání být vyvolána výjimka, když se systém pokusí tento řetězec převést na datový typ decimal. Další informace o přizpůsobení vkládání rozhraní, použijte [ *přizpůsobení rozhraní pro úpravu dat* kurzu](../../data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) z mé [práce s kurz datové řady](../../data-access/index.md).


## <a name="step-3-creating-public-properties-and-methods-in-the-master-page"></a>Krok 3: Vytvoření veřejné vlastnosti a metody na hlavní stránce

V kroku 1 jsme přidali ovládací prvek popisek Web s názvem `GridMessage` výše GridView na hlavní stránce. Tento popisek má volitelně zobrazit zprávu. Například po přidání nového záznamu do `Products` tabulky, může chceme zobrazit zprávu, která čte: "*ProductName* byla přidána do databáze." Místo pevný kódu text pro tento popisek na hlavní stránce může chceme zpráva, která má být přizpůsobitelné podle obsahu stránky.

Protože ovládací prvek popisek je implementovaný jako chráněný členské proměnné na hlavní stránce nelze získat přístup přímo ze stránky obsahu. Za účelem práce s popisek v rámci stránky předlohy ze stránky obsahu (nebo k tomuto účelu, všechny ovládací prvek webu na hlavní stránce), je nutné vytvořit na hlavní stránce, který zveřejňuje ovládací prvek webu nebo slouží jako proxy server, ve které jedna z vlastností může být veřejná vlastnost  získat přístup. Přidejte následující syntaxi do třídy kódu stránky předlohy vystavit jmenovky `Text` vlastnost:


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample6.cs)]

Po přidání nového záznamu do `Products` tabulku ze stránky obsahu `RecentProducts` GridView na hlavní stránce rebind svůj základní zdroj dat je potřeba. Se svázat volání GridView jeho `DataBind` metoda. Vzhledem k tomu, že rutina GridView na hlavní stránce není přístupné prostřednictvím kódu programu na stránky obsahu, budeme muset vytvořit veřejnou metodu na hlavní stránce, která při volání, znovu připojí data k GridView. Přidejte následující metodu do třídy stránky předlohy kódu:


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample7.cs)]

S `GridMessageText` vlastnost a `RefreshRecentProductsGrid` metodu, všechny stránky obsahu prostřednictvím kódu programu nastavit nebo načíst hodnotu `GridMessage` popisku `Text` vlastnost nebo rebind data, která mají `RecentProducts` GridView. Krok 4 prověří jak získat přístup k hlavní stránce veřejné vlastnosti a metody ze stránky obsahu.

> [!NOTE]
> Nezapomeňte označit vlastnosti a metody jako stránky předlohy `public`. Pokud jste není označují explicitně tyto vlastnosti a metody jako `public`, nebudou přístupné ze stránky obsahu.


## <a name="step-4-calling-the-master-pages-public-members-from-a-content-page"></a>Krok 4: Volání veřejné členy stránky předlohy ze stránky obsahu

Nyní, když stránky předlohy nezbytné veřejné vlastnosti a metody, je připraven k vyvolání těchto vlastností a metod z `AddProduct.aspx` stránky obsahu. Konkrétně je potřeba nastavit stránku předlohy `GridMessageText` vlastnost a volání jeho `RefreshRecentProductsGrid` metoda po přidání nového produktu do databáze. Všechna data webové ovládací prvky ASP.NET vyvolání události bezprostředně před a po dokončení různé úkoly, které usnadňují vývojáři stránek určitou akci programový před nebo po úlohu. Například pokud koncový uživatel klikne na tlačítko Vložit prvku DetailsView, na odeslat zpět vyvolá DetailsView jeho `ItemInserting` událostí před zahájením vkládání pracovního postupu. Potom vloží záznam do databáze. Následující, vyvolá DetailsView jeho `ItemInserted` událostí. Proto, aby pracovaly se stránkou předlohy po přidání nového produktu, vytvoření obslužné rutiny události pro prvku DetailsView `ItemInserted` událostí.

Existují dva způsoby, které stránky obsahu můžete programově rozhraní s jeho hlavní stránky:

- Pomocí `Page.Master` vlastnost, která vrací volného typu odkaz na hlavní stránce, nebo
- Zadejte cestu nebo typu souboru stránky předlohy stránky prostřednictvím `@MasterType` direktivy; to automaticky přidá vlastnost silného typu na stránku s názvem `Master`.

Podívejme se na obou přístupů.

### <a name="using-the-loosely-typedpagemasterproperty"></a>Pomocí volného typu`Page.Master`vlastnost

Všechny webové stránky ASP.NET musí být odvozeny od `Page` třídy, která se nachází v `System.Web.UI` oboru názvů. `Page` Obsahuje třídy [ `Master` vlastnost](https://msdn.microsoft.com/library/system.web.ui.page.master.aspx) , vrátí odkaz na hlavní stránky. Pokud stránky nemá na hlavní stránce `Master` vrátí `null`.

`Master` Vlastnost vrací objekt typu [ `MasterPage` ](https://msdn.microsoft.com/library/system.web.ui.masterpage.aspx) (také umístěný v `System.Web.UI` oboru názvů) což je základní typ, ze kterého všechny hlavní stránky odvozen z. Proto pro použití veřejné vlastnosti nebo metody definované v našem webu stránky předlohy jsme musíte vysílat `MasterPage` objekt vrácený `Master` vlastnost příslušného typu. Protože jsme pojmenovali naše soubor předlohové stránky `Site.master`, se s názvem třídy kódu `Site`. Proto následující kód přetypování `Page.Master` vlastnost, která má instance třídy lokality.


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample8.cs)]

Teď, když máme převedena volného typu `Page.Master` vlastnost, která má `Site` typ jsme může odkazovat vlastnosti a metody, které jsou specifické pro lokalitu. Jak je vidět na obrázku 7, veřejné vlastnosti `GridMessageText` se zobrazí v rozevírací IntelliSense.


[![IntelliSense zobrazí veřejné vlastnosti a metody naše stránky předlohy](interacting-with-the-master-page-from-the-content-page-cs/_static/image20.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image19.png)

**Obrázek 07**: IntelliSense zobrazí veřejné vlastnosti a metody naše stránky předlohy ([Kliknutím zobrazit obrázek v plné velikosti](interacting-with-the-master-page-from-the-content-page-cs/_static/image21.png))


> [!NOTE]
> Pokud název souboru hlavní stránky `MasterPage.master` je název třídy kódu stránky předlohy `MasterPage`. To může vést k nejednoznačný kód při přetypování z typu `System.Web.UI.MasterPage` k vaší `MasterPage` třídy. Stručně řečeno budete muset plně kvalifikaci typu, které jsou přetypování, což může být poněkud složité, při použití modelu webový projekt. Můj návrh může být buď se ujistěte, že při vytváření stránky předlohy ji pojmenujete jiné než `MasterPage.master` nebo i lepší vytvořit odkaz silného typu k hlavní stránce.


### <a name="creating-a-strongly-typed-reference-with-themastertypedirective"></a>Vytváření silného typu odkaz s`@MasterType`– direktiva

Pokud najít úzce uvidíte, třída kódu stránky ASP.NET je konkrétní třídu (Poznámka: `partial` – klíčové slovo v definici třídy). Částečné třídy byly zavedeny v C# a Visual Basic with.NET Framework 2.0 a stručně řečeno, povolit pro členy třídy a definovat v rámci více souborů. Třída souboru kódu na pozadí - `AddProduct.aspx.cs`, například – obsahuje kód, který jsme vývojář stránky vytvořit. Kromě kódu modul ASP.NET s vlastnostmi automaticky vytvoří soubor samostatné třídy a obslužné rutiny událostí v tom, že převede deklarativní na stránky hierarchii tříd.

Automatické vytváření kódu, k níž dojde vždy, když je navštívené stránky ASP.NET dláždí některé možnosti místo zajímavé a užitečné. V případě hlavní stránky, pokud nám říct modulu ASP.NET, jaké stránky předlohy je stále používán naší obsahu stránce generuje silného typu `Master` vlastnost pro nás.

Použití [ `@MasterType` – direktiva](https://msdn.microsoft.com/library/ms228274.aspx) k informování modul ASP.NET typu obsahu stránce stránky předlohy. `@MasterType` – Direktiva může přijmout, buď název typu stránky předlohy nebo jeho cesta k souboru. Chcete-li určit, že `AddProduct.aspx` stránka používá `Site.master` jako jeho stránku předlohy, přidejte následující direktivu na začátek `AddProduct.aspx`:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample9.aspx)]

Tato direktiva dá pokyn modulu ASP.NET pro přidání odkazu na silného typu k hlavní stránce prostřednictvím vlastnost s názvem `Master`. S `@MasterType` direktivy na místě, můžete říkáme `Site.master` hlavní stránky veřejné vlastnosti a metody, které jsou přímo pomocí `Master` vlastnost bez žádné přetypování.

> [!NOTE]
> V případě vynechání `@MasterType` – direktiva, syntaxe `Page.Master` a `Master` vrátit samé: objekt na hlavní stránky a volného typu. Pokud zahrnete `@MasterType` – direktiva pak `Master` vrátí silného typu odkaz na zadaný stránky předlohy. `Page.Master`, ale stále vrátí volného typu odkaz. Pro podrobnější pohled na důvod, proč je tomu a jak `Master` vlastnost je vytvořený při `@MasterType` – direktiva je zahrnuté naleznete [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx)na položce blogu [ `@MasterType` v technologii ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx).


### <a name="updating-the-master-page-after-adding-a-new-product"></a>Aktualizaci stránky předlohy po přidání nového produktu

Teď, když budeme vědět, jak má být vyvolán veřejné vlastnosti a metody ze stránky obsahu stránky předlohy, je připraven k aktualizaci `AddProduct.aspx` stránky tak, aby stránky předlohy se aktualizují po přidání nového produktu. Na začátku kroku 4 jsme vytvořili obslužné rutiny události pro ovládací prvek DetailsView `ItemInserting` událost, která spustí ihned po nového produktu se přidal do databáze. Přidejte následující kód do této obslužné rutiny události:


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample10.cs)]

Výše uvedený kód používá, i volného typu `Page.Master` vlastnosti a silného typu `Master` vlastnost. Všimněte si, že `GridMessageText` je nastavena na "*ProductName* přidat k mřížce..." Produkt právě přidané hodnoty jsou k dispozici prostřednictvím `e.Values` kolekce; jak můžete vidět, právě přidané `ProductName` hodnotu přistupuje prostřednictvím `e.Values["ProductName"]`.

Obrázek 8 ukazuje `AddProduct.aspx` stránka ihned po novém produktu - Scott na Soda - byla přidána do databáze. Všimněte si, že je název právě přidané produktu uvedených v popisku stránky předlohy a že GridView aktualizaci produktu a jeho cenu.


[![Popisek a zobrazit GridView produktu právě přidané stránky předlohy](interacting-with-the-master-page-from-the-content-page-cs/_static/image23.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image22.png)

**Obrázek 08**: popisek hlavní stránky a GridView zobrazit Just-Added produktu ([Kliknutím zobrazit obrázek v plné velikosti](interacting-with-the-master-page-from-the-content-page-cs/_static/image24.png))


## <a name="summary"></a>Souhrn

V ideálním případě stránky předlohy a stránky jeho obsahu jsou zcela oddělené od sebe navzájem a nevyžadují žádné úrovně interakce. Při hlavní stránky a stránky obsahu by se měly navrhovat s Pamatujte, že cílem, existuje několik běžných scénářů, ve kterých musí obsahu stránce rozhraní s jeho stránky předlohy. Jedním z nejčastějších důvodů soustředí kolem aktualizace konkrétní části stránku předlohy pro zobrazení na základě některé akce, který, k čemu na stránce obsahu.

Dobrá zpráva je, že je relativně jednoduché tak, aby měl stránky obsahu prostřednictvím kódu programu interakci s jeho stránky předlohy. Začněte vytvořením veřejné vlastnosti nebo metody na hlavní stránce zapouzdřující funkce, které je potřeba vyvolat stránku obsahu. Pak na stránce obsahu přístup k hlavní stránce vlastnosti a metody prostřednictvím volného typu `Page.Master` vlastnost nebo použijte `@MasterType` – direktiva k vytvoření odkazu silného typu k hlavní stránce.

V dalším kurzu jsme zkontrolujte jak vám má prostřednictvím kódu programu interakci s jedním z jeho stránky obsahu stránky předlohy.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Přístup k informacím a aktualizace dat v technologii ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [ASP.NET hlavní stránky: Tipy, triky a depeše](http://www.odetocode.com/articles/450.aspx)
- [`@MasterType`v technologii ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx)
- [Předávání informací mezi obsah a stránky předlohy](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Práce s daty v kurzech ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor více knih ASP/ASP.NET a zakladatele 4GuysFromRolla.com, pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologie ASP.NET 3.5 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott lze dosáhnout za [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu v [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl Zack Petr. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](control-id-naming-in-content-pages-cs.md)
[další](interacting-with-the-content-page-from-the-master-page-cs.md)
