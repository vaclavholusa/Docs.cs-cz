---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Část 7: Přidávání funkcí | Microsoft Docs'
author: JoeStagner
description: Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 7 přidává další funkce, jako je například účet revie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 17f068155f6726047901e2f7d580d375a4e07c87
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="part-7-adding-features"></a>Část 7: Přidání funkcí
====================
podle [Stagner Jan](https://github.com/JoeStagner)

> Tailspin Spyworks ukazuje, jak neobvykle jednoduché je vytvoření efektivní a škálovatelné aplikace pro platformu .NET. Zobrazuje vypnout používání skvělé nové funkce v rozhraní ASP.NET 4 k sestavení online úložiště, včetně nákupy, najdete v článku věnovaném a správu.
> 
> Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 7 přidává další funkce, jako je například účet kontrolní, recenzemi produktů a "Oblíbené položky" a "také zakoupených" uživatelské ovládací prvky.


## <a id="_Toc260221673"></a>  Přidávání funkcí

Když uživatelé procházet naše katalogu, umístěte položek do nákupního košíku a dokončete proces najdete v článku věnovaném, se, že počet podpora funkce, že jsme bude obsahovat vylepšit náš web neexistují.

1. Zkontrolujte účet (seznam objednávky umístěn a zobrazit podrobnosti.)
2. Přidejte nějaký kontextu konkrétní obsah na stránku front.
3. Přidání funkce tak, aby uživatelé zkontrolujte produktů v katalogu.
4. Vytvoření uživatelského ovládacího prvku zobrazení oblíbených položek a místo, které řídí na stránce front.
5. Vytvoření uživatelského ovládacího prvku "Také zakoupili" a přidejte na stránku podrobností produktu.
6. Přidat a obraťte se na stránku.
7. Přidat o stránce.
8. Globální chyby

## <a id="_Toc260221674"></a>  Zkontrolujte účet

Ve složce "Účet" vytvořte dvě stránky .aspx jednu s názvem OrderList.aspx a dalších pojmenované OrderDetails.aspx

OrderList.aspx využije funkci GridView a EntityDataSoure ovládacích prvků, podobně jako dříve máme.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

EntityDataSoure vybere záznamy v tabulce filtrováno uživatelské jméno (viz WhereParameter), který jsme nastavena v proměnné relace, když uživateli přihlášení je.

Všimněte si také tyto parametry v HyperlinkField prvku GridView:

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

Tyto zadejte odkaz na zobrazení podrobností pořadí pro každý produkt zadat jako parametr řetězce dotazu na stránku OrderDetails.aspx pole OrderID.

## <a id="_Toc260221675"></a>  OrderDetails.aspx

Ovládací prvek EntityDataSource budeme používat pro přístup k objednávky a FormView zobrazují data pořadí a jiné EntityDataSource s GridView pro zobrazení všech pořadí položek řádku.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

V souboru kódu za (OrderDetails.aspx.cs) máme dvě málo bity housekeeping.

Nejprve musíme Ujistěte se, že Rozpis objednávek vždy získá OrderId.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

Také potřebujeme k výpočtu a celkový počet položek řádku pořadí zobrazení.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  Domovská stránka

Přidejme nějaký statický obsah na stránku Default.aspx.

Nejprve budete vytvořit složku "Obsah" a v něm složku obrázků (a I bude obsahovat obrázek, který se použije na domovské stránce).

Do dolní zástupného textu stránky Default.aspx přidejte následující kód.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  Recenzemi produktů

Nejprve přidáme tlačítko s odkazem na formulář, který jsme můžete použít k zadání kontrolu produktu.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

Všimněte si, že jsme předali ProductID v řetězci dotazu

Další přidejme stránku s názvem ReviewAdd.aspx

Tato stránka bude používat sadu ovládacího prvku ASP.NET AJAX. Pokud jste již neučinili, si můžete stáhnout z [DevExpress](http://devexpress.com/act) a pokyny o nastavení sady nástrojů pro použití se sadou Visual Studio zde [ https://www.asp.net/learn/ajax-videos/video-76.aspx ](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).

V režimu návrhu přetáhněte ovládací prvky a validátory z panelu nástrojů a sestavení formuláře podobná té následující.

![](tailspin-spyworks-part-7/_static/image2.jpg)

Zápis bude vypadat přibližně takto.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

Teď, když jsme můžete zadat recenze, umožňuje zobrazit tyto recenze na stránce produktu.

Tento kód přidejte na stránku ProductDetails.aspx.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

Spuštěné aplikace teď a přejdete na produkt zobrazuje informace o produktu včetně zákaznické recenze.

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  Oblíbené položky ovládacího prvku (vytvoření uživatelské ovládací prvky)

Chcete-li zvýšit prodej na svém webu přidáme několik funkcí do oblíbených nebo souvisejících produktů "sugestivní prodává".

První z těchto funkcí bude seznam oblíbenější produktu v našem katalog produktů.

Vytvoříme "Uživatelského ovládacího prvku" zobrazíte nejlépe prodávané položky na domovské stránce naše aplikace. Vzhledem k tomu, že to bude ovládacího prvku, jsme ji používat na libovolné stránce jednoduše přetahování a vkládání v návrháři Visual Studio na žádné stránce, které jsme jako ovládací prvek.

V Průzkumníku řešení sady Visual Studio klikněte pravým tlačítkem na název řešení a vytvořte nový adresář s názvem "Ovládacích prvků". I když není nezbytné k tomu, nám pomůže zachovat naše projektu uspořádané podle vytváření všechny naše uživatelské ovládací prvky v adresáři "Ovládacích prvků".

Klikněte pravým tlačítkem na složku, ovládací prvky a zvolte "Nová položka":

![](tailspin-spyworks-part-7/_static/image4.jpg)

Zadejte název pro naše řízení "PopularItems". Všimněte si, že přípona souboru pro uživatelské ovládací prvky .ascx není .aspx.

Naše oblíbených položek uživatelský ovládací prvek bude definován následujícím způsobem.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

Tady používáme metodu, kterou jsme nepoužili ještě v této aplikaci. Používáme ovládacím prvku repeater a místo použití prvku zdroje dat jsme se do výsledků dotazu LINQ to Entities vazbu ovládacím prvku opakovače.

V kódu naše řízení to následujícím způsobem.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

Všimněte si také tohoto důležité řádku v horní části značky naše ovládacího prvku.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

Vzhledem k tomu většinou oblíbených položek nebude změna na základě minutu na minutu přidáme direktivu bolavými zlepšit výkon aplikace. Tato direktiva způsobí, že kód ovládací prvky, který lze spustit pouze když vyprší platnost výstup z mezipaměti ovládacího prvku. Jinak se použije v mezipaměti verzi výstupu ovládacího prvku.

Nyní všechny, které se musí provést je zahrňte do naší stránce s Default.aspc naše nový ovládací prvek.

Použití přetažení umístit instanci ovládacího prvku ve sloupci otevřete v našem výchozího formuláře.

![](tailspin-spyworks-part-7/_static/image5.jpg)

Nyní když jsme spuštění aplikace na domovskou stránku, zobrazí se většinou oblíbených položek.

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  "Taky zakoupili" řízení (uživatelské ovládací prvky s parametry)

Druhý uživatelský ovládací prvek, který vytvoříme bude trvat sugestivní prodávané na další úroveň přidáním kontextu specifické podobě.

Logika pro vypočítat nejvyšší položky "Také zakoupili" je netriviální.

Naše "Také zakoupili" řízení bude vyberte záznamy Rozpis objednávek (dřív pořídili) pro aktuálně vybrané ProductID a získat OrderIDs pro každý jedinečný pořadí, která se nachází.

Potom jsme vybere al produkty z těchto objednávek a součet množství zakoupili. Jsme budete seřadit tento součet objemu produktů a zobrazit prvních pět položek.

S ohledem na složitost tuto logiku, bude tento algoritmus implementaci jako uložené procedury.

T-SQL pro uloženou proceduru je následující.

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

Všimněte si, že tuto uloženou proceduru (SelectPurchasedWithProducts) existovalo v databázi jsme obsažen v naší aplikaci a jsme generování Entity Data Model, který jsme zadali kromě tabulky a zobrazení, které jsme potřeby datového modelu Entity by měla obsahovat tuto uloženou proceduru.

Pro přístup k uloženou proceduru z datového modelu Entity musíme import funkce.

Dvakrát klikněte na datový Model Entity v Průzkumníku řešení otevřete v návrháři a otevřít prohlížeč modelu, potom klikněte pravým tlačítkem myši v návrháři a vyberte možnost "Přidat funkce Import".

![](tailspin-spyworks-part-7/_static/image1.png)

Tím se otevře dialogové okno.

![](tailspin-spyworks-part-7/_static/image2.png)

Vyplňte pole, jak můžete vidět výše, výběr "SelectPurchasedWithProducts" a používat název procedury pro název naše importované funkce.

Klikněte na tlačítko "Ok".

Má to neudělali, které jsme můžete jednoduše programu proti uloženou proceduru jako jsme může jiné položky v modelu.

Ano v našem složce "Ovládací prvky" vytvořte nový uživatelský ovládací prvek s názvem AlsoPurchased.ascx.

Kód pro tento ovládací prvek bude vypadat dobře obeznámeni do ovládacího prvku PopularItems.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

Pozoruhodný rozdíl je, že vzhledem k tomu, že k vykreslení položky se budou lišit podle produktu, nejsou ukládání výstupu.

ProductId bude "vlastnost" do ovládacího prvku.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

V obslužná rutina ovládacího prvku událost PreRender jsme rychlost tři věci.

1. Ujistěte se, že je nastaven ProductID.
2. Viděli, zda jsou všechny produkty, které byly zakoupeny aktuální publikací.
3. Výstup některé položky určené v #. 2.

Všimněte si, jak je snadné volání uložené procedury prostřednictvím modelu.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

Po určení, že jsou "také zakoupili" jsme do výsledků vrácených dotazem jednoduše vazbu opakovače.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

Pokud všechny položky "také zakoupili" nebyly budete jednoduše zobrazujeme dalších oblíbených položek z našich katalogu.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

Zobrazit položky "Také zakoupili", otevřete stránku ProductDetails.aspx a přetáhněte ovládací prvek AlsoPurchased v Průzkumníku řešení tak, aby se zobrazí v této pozici ve značkách.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

Tím se vytvoří odkaz na ovládací prvek v horní části stránky ProductDetails.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

Vzhledem k tomu, že AlsoPurchased uživatelského ovládacího prvku vyžaduje číslo ProductId jsme se nastavit vlastnost ProductID naše ovládacího prvku pomocí příkazu zkušební verze na aktuální položku modelu dat stránky.

![](tailspin-spyworks-part-7/_static/image3.png)

Pokud jsme sestavit a spustit nyní a přejděte do produktu jsme viděli položky, které "Také zakoupili".

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> [Předchozí](tailspin-spyworks-part-6.md)
> [další](tailspin-spyworks-part-8.md)
