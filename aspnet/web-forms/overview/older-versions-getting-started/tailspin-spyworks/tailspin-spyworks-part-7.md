---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: '7. část: Přidání funkcí | Dokumentace Microsoftu'
author: JoeStagner
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 7 přidává další funkce, jako je například účet revie...
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 47402ccdfb702dc1bb1bdb4e634a7cd6f5ebc235
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831645"
---
<a name="part-7-adding-features"></a>7. část: Přidání funkcí
====================
podle [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks ukazuje, jak mimořádně jednoduché je vytvářet výkonné a škálovatelné aplikace pro platformu .NET. Zobrazuje vypnout použití skvělých nových funkcí v technologii ASP.NET 4 k sestavení nebo online úložiště, včetně nákupu, Pokladna a správu.
> 
> V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 7 přidává další funkce, jako je například účet revize, recenzemi produktů a "Oblíbené položky" a "také zakoupené" uživatelské ovládací prvky.


## <a id="_Toc260221673"></a>  Přidání funkce

I když můžou uživatelé procházet náš katalog, umístěte položky do nákupního košíku a dokončete proces platby u pokladny, jsou že počet podpůrné funkce zavedeme vylepšit náš web.

1. Účet kontroly (seznamu objednávek umístit a zobrazit podrobnosti.)
2. Přidejte nějaký obsah konkrétní kontext na přední stránce.
3. Přidáte funkci, která umožní uživatelům revize produktů v katalogu.
4. Vytvoření uživatelského ovládacího prvku se zobrazí na přední stránce oblíbených položek a místo, které řídí.
5. Vytvořte uživatelský ovládací prvek "Také zakoupili" a přidejte na stránku podrobností produktu.
6. Přidat kontakt stránky.
7. Přidat o stránce.
8. Globální zpracování chyb

## <a id="_Toc260221674"></a>  Účet kontroly

Ve složce "Účet" vytvořte dvě stránky ASPX, jednu s názvem OrderList.aspx a dalších pojmenované OrderDetails.aspx

OrderList.aspx bude využívat ovládací prvky GridView a EntityDataSoure podobně, jako máme k dispozici dříve.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

EntityDataSoure vybírá záznamy v tabulce objednávky filtrované podle uživatelského jména (viz WhereParameter), které jsme si nastavili v proměnné relace při uživateli přihlášení uživatele.

Všimněte si také v HyperlinkField prvku GridView. Tyto parametry:

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

Tyto zadejte odkaz na mohli zobrazit podrobnosti pro jednotlivé produkty, které zadáte jako parametr řetězce dotazu na stránku OrderDetails.aspx pole OrderID.

## <a id="_Toc260221675"></a>  OrderDetails.aspx

Ovládací prvek EntityDataSource budeme používat pro přístup k objednávky a FormView zobrazují data objednávky a jiné EntityDataSource s GridView pro zobrazení položek řádku všechny objednávky.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

V souboru kódu za (OrderDetails.aspx.cs) máme dva bity malý údržbu.

Nejdřív potřebujeme Ujistěte se, že OrderDetails vždy získá OrderId.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

Musíme také vypočítávají a zobrazují celkový počet položek řádku pořadí.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  Na domovské stránce

Přidejme nějaký statický obsah na stránku Default.aspx.

Nejprve vytvoříte složku "Obsah" a v něm složku obrázky (a jsem bude obsahovat obrázek, který se použije na domovské stránce.)

V dolní části zástupný symbol stránku Default.aspx přidejte následující kód.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  Revize produktu

Nejdřív přidáme tlačítko s odkazem na formulář, který jsme můžete použít k zadání recenze produktu.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

Všimněte si, že jsme v řetězci dotazu prochází ProductID

Další přidáme stránku s názvem ReviewAdd.aspx

Tato stránka bude používat ASP.NET AJAX Control Toolkit. Pokud jste tak ještě neučinili, můžete ji stáhnout [DevExpress](http://devexpress.com/act) a pokyny o nastavení sady nástrojů pro použití se službou Visual Studio zde [ https://www.asp.net/learn/ajax-videos/video-76.aspx ](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).

V režimu návrhu přetáhněte z panelu nástrojů ovládacích prvků a validátory a podobná té následující formulář vytvářet.

![](tailspin-spyworks-part-7/_static/image2.jpg)

Značky budou vypadat asi takhle nějak.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

Teď můžeme zadat revize, vám umožní zobrazit tyto kontroly na stránce produktu.

Přidejte tento kód na stránku ProductDetails.aspx.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

Spuštění naší aplikace teď a přejdete do produktu se zobrazí informace o produktech, včetně zapracováním připomínek zákazníků.

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  Oblíbené položky ovládacího prvku (vytváření uživatelských ovládacích prvků)

Za účelem zvýšení prodeje na webové stránce přidáme několik funkcí "sugestivní sell" Oblíbené nebo souvisejících produktů.

První z těchto funkcí bude zahrnovat další oblíbené produktu v našich katalog produktů.

Vytvoříme "Uživatelský ovládací prvek" k zobrazení nejvyšší prodeje položky na domovské stránce naši aplikaci. Vzhledem k tomu, že to bude ovládací prvek, jsme jednoduše přetahováním ovládací prvek v návrháři aplikace Visual Studio stránku, který budeme používat na libovolné stránce.

V Průzkumníku řešení sady Visual Studio klikněte pravým tlačítkem myši na název řešení a vytvořte nový adresář s názvem "Prvky". I když není potřeba udělat, bude pomůžeme našem projektu uspořádané podle vytváření všech našich uživatelské ovládací prvky v adresáři "Ovládací prvky".

Klikněte pravým tlačítkem na složku Ovládací prvky a zvolte možnost "Nová položka":

![](tailspin-spyworks-part-7/_static/image4.jpg)

Zadejte název pro naše ovládací prvek "PopularItems". Všimněte si, že přípona souboru pro uživatelské ovládací prvky .ascx není .aspx.

Naše Oblíbené položky uživatelský ovládací prvek bude definovaná následujícím způsobem.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

Tady používáme metoda, kterou jsme nepoužili ještě v této aplikaci. Používáme ovládací prvek repeater a namísto použití ovládací prvek zdroje dat jsme provádíte navázání ovládacím prvku opakovače s výsledky dotazu LINQ to Entities.

V kódu naše ovládací prvek provedeme to následujícím způsobem.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

Všimněte si také tato důležité řádku v horní části kódu naše ovládacího prvku.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

Vzhledem k tomu, že Nejoblíbenější položky nebudou měnit na základě minut na minutu můžeme přidat direktivu bolavými ke zlepšení výkonu našich aplikací. Tato direktiva způsobí, že kód ovládacích prvků na spustit pouze když vyprší platnost výstup z mezipaměti ovládacího prvku. V opačném případě se použije verze uložené v mezipaměti ovládacího prvku výstupu.

Nyní je vše, co musíme udělat zahrňte naši stránku Default.aspc náš nový ovládací prvek.

Použití přetažení umístit ve sloupci otevřít v našem formuláři výchozí instanci ovládacího prvku.

![](tailspin-spyworks-part-7/_static/image5.jpg)

Nyní když jsme spuštění aplikace na domovské stránce zobrazí Oblíbené položky.

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  "Také zakoupili" ovládací prvek (uživatelské ovládací prvky s parametry)

Druhý uživatelský ovládací prvek, který vytvoříme bude trvat sugestivní prodej na vyšší úroveň přidáním specifičnosti kontextu.

Logika pro výpočet "Také zakoupili" položky na nejvyšší úrovni je netriviální.

Naše "Také zakoupili" ovládací prvek bude vyberte záznamy OrderDetails (jste dříve zakoupili) pro aktuálně vybrané ProductID a získejte OrderIDs pro každý jedinečných pořadí, ve kterém se nachází.

Pak bude vybereme al produktů ze všech objednávek a množství zakoupených součet. Vytvoříme řadit tento součet množství produktů a zobrazte prvních pět položek.

Vzhledem ke složitosti tuto logiku, jsme jako uloženou proceduru implementuje tento algoritmus.

T-SQL pro úložnou proceduru vypadá takto.

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

Všimněte si, že tuto uloženou proceduru (SelectPurchasedWithProducts) existoval v databázi, když jsme zahrnuli v naší aplikaci a jsme generování modelu Entity Data Model, který jsme zadali kromě tabulky a zobrazení, které jsme potřebovali, modelu Entity Data Model tuto uloženou proceduru by měla obsahovat.

Pro přístup k uloženou proceduru z modelu Entity Data Model potřebujeme import funkce.

Dvakrát klikněte na datový Model Entity v Průzkumníku řešení otevřete v návrháři a otevřít prohlížeč modelu pak v Návrháři klikněte pravým tlačítkem a vyberte "Přidání importované funkce".

![](tailspin-spyworks-part-7/_static/image1.png)

Tím se otevře toto dialogové okno.

![](tailspin-spyworks-part-7/_static/image2.png)

Vyplňte pole, jak vidíte výše, vyberete "SelectPurchasedWithProducts" a použijte název procedury pro název naší importované funkce.

Klikněte na tlačítko "Ok".

Udělání to, které můžete jednoduše programujeme uloženou proceduru jako jsme může být jakoukoli jinou položku v modelu.

Tedy ve složce naše "Řídí" vytvořte nový uživatelský ovládací prvek s názvem AlsoPurchased.ascx.

Značky pro tento ovládací prvek bude velmi povědomá do ovládacího prvku PopularItems.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

Významný rozdíl spočívá v tom, která nejsou ukládání do vyrovnávací paměti výstup vzhledem k tomu, že k vykreslení položky se budou lišit podle produktu.

ProductId bude "vlastnosti" do ovládacího prvku.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

V obslužné rutině ovládacího prvku událost PreRender jsme rychlost udělat tři věci.

1. Ujistěte se, že je nastavena ProductID.
2. Zkontrolujte, jestli všechny produkty, které jste zakoupili s aktuálním předplatným.
3. Výstupní některé položky, jak je stanoveno v #2.

Všimněte si, jak snadné je volat úložnou proceduru prostřednictvím modelu.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

Po určení, která existuje "také nakupujete" jsme můžete jednoduše vytvořit vazbu opakovače s výsledky vrácené dotazem.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

Pokud se všechny položky "také zakoupili" nebyly budete jednoduše zobrazují další oblíbené položky z našich katalogu.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

Chcete-li zobrazit položky "Také koupit", otevřete stránku ProductDetails.aspx a přetáhněte ovládací prvek AlsoPurchased z Průzkumníka řešení, aby se zobrazovala na této pozici v kódu.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

Tím se vytvoří odkaz na ovládací prvek v horní části stránky ProductDetails.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

Protože AlsoPurchased uživatelský ovládací prvek vyžaduje ProductId číslo nastavíme vlastnost ProductID naše ovládacího prvku pomocí příkazu zkušební verze na aktuální položku modelu data stránky.

![](tailspin-spyworks-part-7/_static/image3.png)

Když jsme sestavit a spustit nyní a přejděte do produktu vidíme položky "Také zakoupili".

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> [Předchozí](tailspin-spyworks-part-6.md)
> [další](tailspin-spyworks-part-8.md)
