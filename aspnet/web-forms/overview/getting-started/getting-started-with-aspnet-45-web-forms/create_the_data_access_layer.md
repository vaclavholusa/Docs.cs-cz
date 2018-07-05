---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
title: Vytvoření vrstvy přístupu k datům | Dokumentace Microsoftu
author: Erikre
description: V této sérii kurzů se seznámíte se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a službu Microsoft Visual Studio Express 2013 pro jsme...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 0bbf7a6e-d7eb-4091-91e4-fff892777f32
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
msc.type: authoredcontent
ms.openlocfilehash: 5b4bb5bc89938836bc37e8ebd385fa966bc6f511
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390025"
---
<a name="create-the-data-access-layer"></a>Vytvoření vrstvy přístupu k datům
====================
podle [Erik Reitan](https://github.com/Erikre)

[Stáhněte si ukázkový projekt Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) nebo [stáhnout elektronickou knihu (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> V této sérii kurzů se seznámíte se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a službu Microsoft Visual Studio Express 2013 for Web. Visual Studio 2013 [projektu se zdrojovým kódem jazyka C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) je k dispozici v této sérii kurzů.


Tento kurz popisuje, jak vytvořit, přístup a zkontrolujte data v databázi pomocí webových formulářů ASP.NET a Entity Framework Code First. V tomto kurzu vychází z předchozí kurz o službě "Vytvořit projekt" a je součástí série kurzů Wingtip Slonovi Store. Po dokončení tohoto kurzu, který bude jste vytvořili skupinu tříd přístup k datům, které jsou v *modely* složky projektu.

## <a name="what-youll-learn"></a>Co se dozvíte:

- Postup vytvoření datové modely.
- Jak k inicializaci a přidání dat do databáze.
- Jak aktualizovat a nakonfigurovat aplikaci, aby podporovala databáze.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Toto jsou vlastnosti představené v tomto kurzu:

- Entity Framework Code First
- LocalDB
- Datové poznámky

## <a name="creating-the-data-models"></a>Vytváření datových modelů

[Entity Framework](https://msdn.microsoft.com/data/aa937723) je objektově relační mapování (ORM) rozhraní. To vám umožní pracovat s relačními daty, jako objekty, což eliminuje většinu kódu přístup k datům, která obvykle byste museli napsat. Používá nástroj Entity Framework, můžete zadávat dotazy pomocí [LINQ](https://msdn.microsoft.com/library/bb397926.aspx), načítání a manipulaci s daty jako objektů se silným typem. LINQ poskytuje vzory pro dotazování a aktualizace dat. Používá nástroj Entity Framework umožňuje zaměřit se na vytváření zbývající části aplikace, místo zaměření na data základy přístup. Dále v této sérii vám ukážeme, jak používat data k naplnění dotazy navigace a produktu.

Entity Framework podporuje vývoj paradigma volá *Code First*. Kód nejprve umožňuje definovat pomocí třídy. Třída je konstrukce, která umožňuje vytvořit vlastní typy prostřednictvím seskupování proměnné jiné typy, metody a události. Můžete mapování tříd k existující databázi nebo použít ke generování databáze. V tomto kurzu vytvoříte datových modelů napsáním tříd datových modelů. Potom dáme vám Entity Framework vytvořit databázi v reálném čase z těchto nových tříd.

Začnete vytvořením tříd entit, které definují datových modelů pro aplikaci webových formulářů. Pak vytvoříte kontextu třídu, která spravuje tříd entit a poskytuje přístup k datům v databázi. Vytvoříte také inicializátor třídy, který budete používat k naplnění databáze.

### <a name="entity-framework-and-references"></a>Entity Framework a odkazy

Ve výchozím nastavení, Entity Framework je součástí vytvoříte nový **webová aplikace ASP.NET** pomocí **webových formulářů** šablony. Entity Framework můžete nainstalovaná, odinstaluje a aktualizovat jako balíček NuGet.

Tento balíček NuGet. obsahuje následující **runtime** sestavení v rámci svého projektu:

- EntityFramework.dll – všechny běžné runtime kód používá Entity Framework
- EntityFramework.SqlServer.dll – Microsoft SQL Server zprostředkovatele pro Entity Framework

### <a name="entity-classes"></a>Tříd entit

Třídy vytvoříte pro definování schématu dat se nazývají tříd entit. Pokud jste nový návrh databáze, představit jako definice tabulky databáze tříd entit. Každou vlastnost v třídě určuje sloupec v tabulce databáze. Tyto třídy poskytují jednoduchý, objektově relační rozhraní mezi kódem objektově orientované a struktura tabulky relační databáze.

V tomto kurzu budete Začněte přidáním jednoduché entity třídy představující schémata pro produkty a kategorie. Třída produkty budou obsahovat definice pro každý produkt. Název každého člena třídy produkt bude `ProductID`, `ProductName`, `Description`, `ImagePath`, `UnitPrice`, `CategoryID`, a `Category`. Kategorie třídy bude obsahovat definice pro každou kategorii, který produkt může patřit do, jako jsou auta, loď nebo roviny. Název každého člena třídy kategorie bude `CategoryID`, `CategoryName`, `Description`, a `Products`. Každý produkt bude patřit do jedné kategorie. Tyto entity třídy se přidá k existujícím projektu *modely* složky.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *modely* složku a pak vyberte **přidat**  - &gt; **nová položka**. 

    ![Vytvoření vrstvy přístupu k datům – nové položky nabídky](create_the_data_access_layer/_static/image1.png)

   **Přidat novou položku** se zobrazí dialogové okno.
2. V části **Visual C#** z **nainstalováno** podokna na levé straně vyberte **kód**. 

    ![Vytvoření vrstvy přístupu k datům – nové položky nabídky](create_the_data_access_layer/_static/image2.png)
3. Vyberte **třídy** z podokna uprostřed a názvem Tato nová třída *Product.cs*.
4. Klikněte na tlačítko **přidat**.  
   Nový soubor třídy se zobrazí v editoru.
5. Nahraďte kód následujícím kódem:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample1.cs)]
6. Vytvořit jiné třídy opakováním kroků 1 až 4, ale pojmenujte novou třídu *Category.cs* a nahraďte kód následujícím kódem:  

    [!code-csharp[Main](create_the_data_access_layer/samples/sample2.cs)]

Jak už jsme zmínili, `Category` třídy představuje typ produktu, které aplikace je navržen tak, aby je prodal (například <a id="a"> </a> &quot;auta&quot;, &quot;lodě&quot;, &quot;Rockets&quot;, a tak dále) a `Product` třída představuje jednotlivé produkty (toys) v databázi. Každá instance `Product` objektu, budou odpovídat na řádek v tabulce relačních databází a každá vlastnost třídy produkt bude mapovat na sloupec v tabulce relační databáze. Později v tomto kurzu zkontrolujete produktu data obsažená v databázi.

### <a name="data-annotations"></a>Datové poznámky

Mohli jste si všimnout, že některé členy třídy mají atributy zadání podrobností o členu, například `[ScaffoldColumn(false)]`. Jedná se o *anotacemi dat*. Atributy datového poznámky můžete popisují, jak ověření vstupu uživatele pro tento člen, můžete určit formátování a určete, jak je modelovaná při vytvoření databáze.

### <a name="context-class"></a>Context – třída

Pokud chcete začít používat tříd pro přístup k datům, musí definovat třídu kontextu. Jak už bylo zmíněno dříve, třídy kontextu spravuje tříd entit (například `Product` třídy a `Category` třídy) a poskytuje přístup k datům v databázi.

Tento postup přidá nové třídy C# kontext k *modely* složky.

1. Klikněte pravým tlačítkem myši *modely* složku a pak vyberte **přidat**  - &gt; **nová položka**.   
   **Přidat novou položku** se zobrazí dialogové okno.
2. Vyberte **třídy** v prostředním podokně, pojmenujte ho *ProductContext.cs* a klikněte na tlačítko **přidat**.
3. Nahraďte výchozí kód obsažený ve třídě s následujícím kódem:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample3.cs)]

Tento kód přidá `System.Data.Entity` obor názvů, abyste měli přístup ke všem funkcím základní Entity Framework, která zahrnuje možnost dotazu, vložit, aktualizovat a odstraňovat data ve spolupráci s objektů se silným typem.

`ProductContext` Třída reprezentuje kontext databáze produktu Entity Framework, která zpracovává načítání, ukládání a aktualizaci `Product` třídy instancí v databázi. `ProductContext` Třída odvozena z `DbContext` základní třída poskytované rozhraním Entity Framework.

### <a name="initializer-class"></a>Inicializátor třídy

Je potřeba spustit některé vlastní logiku inicializace databáze první čas, který se používá kontextu. To vám umožní počáteční data mají být přidány do databáze, tak, aby si můžete okamžitě prohlédnout produktů a kategorie.

Tento postup přidá nové třídy C# inicializátor pro *modely* složky.

1. Vytvořte další `Class` v *modely* složku a pojmenujte ho *ProductDatabaseInitializer.cs*.
2. Nahraďte výchozí kód obsažený ve třídě s následujícím kódem:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample4.cs)]

Jak je vidět ve výše uvedeném kódu, když je vytvořen a inicializován, databáze `Seed` vlastnost je přepsat a nastavit. Když `Seed` je hodnota nastavena, hodnoty z kategorie a produkty, které se používají k naplnění databáze. Pokud se pokusíte k aktualizaci dat počáteční hodnoty tak, že upravíte výše uvedený kód po vytvoření databáze, neuvidíte žádné aktualizace při spuštění webové aplikace. Důvodem je to výše uvedený kód používá provádění `DropCreateDatabaseIfModelChanges` třídy rozpoznat, pokud model (schéma) byl změněn před obnovením dat počáteční hodnoty. Pokud jsou provedeny žádné změny `Category` a `Product` tříd entit, databáze nebude opakování inicializace odběrů pomocí dat počáteční hodnoty.

> [!NOTE] 
> 
> Pokud byste chtěli databáze, kterou chcete znovu vytvořit pokaždé, když byla aplikace spuštěná, můžete použít `DropCreateDatabaseAlways` místo na třídě `DropCreateDatabaseIfModelChanges` třídy. Ale pro tuto řadu kurzů použít `DropCreateDatabaseIfModelChanges` třídy.


V tuto chvíli v tomto kurzu budete mít *modely* složky čtyři nové třídy a jednu výchozí třídu:

![Vytvoření vrstvy přístupu k datům – složku modely](create_the_data_access_layer/_static/image3.png)

### <a name="configuring-the-application-to-use-the-data-model"></a>Konfigurace aplikace pro použití datového modelu

Teď, když jste vytvořili třídy, které představují data, musíte nakonfigurovat aplikaci pro použití třídy. V *Global.asax* souboru, přidejte kód, který inicializuje model. V *Web.config* přidat informace, které se říká aplikace co databázi, kterou budete používat k ukládání dat, která je reprezentována nové třídy datového souboru. *Global.asax* soubor lze použít ke zpracování událostí aplikace nebo metody. *Web.config* souboru umožňuje řídit konfiguraci webové aplikace ASP.NET.

#### <a name="updating-the-globalasax-file"></a>Aktualizuje se soubor Global.asax

Inicializovat datové modely při spuštění aplikace, budete aktualizovat `Application_Start` obslužné rutiny v *Global.asax.cs* souboru.

> [!NOTE] 
> 
> V Průzkumníku řešení, můžete vybrat, zda *Global.asax* souboru nebo *Global.asax.cs* soubor pro úpravu *Global.asax.cs* souboru.


1. Přidejte následující kód zvýrazněné žlutou barvou na `Application_Start` metodu *Global.asax.cs* souboru.   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample5.cs?highlight=9-10,22-23)]

> [!NOTE] 
> 
> Váš prohlížeč musí podporovat HTML5, chcete-li zobrazit kód zvýrazněn žlutě při prohlížení v této sérii kurzů v prohlížeči.


Jak je znázorněno výše uvedený kód při spuštění aplikace, aplikace určuje, že je přístupná inicializátoru, který se spustí při prvním spuštění data. Dva další obory názvů jsou nutné pro přístup k `Database` objektu a `ProductDatabaseInitializer` objektu.

 Úprava souboru Web.Config. 

Ačkoli Entity Framework Code First databáze za vás ve výchozím umístění při vygeneruje počáteční data se načtou databáze, přidání své vlastní informace o připojení k vaší aplikaci nabízí kontrolu nad umístění databáze. Zadejte toto připojení k databázi pomocí připojovacího řetězce v aplikačním *Web.config* souboru v kořenovém adresáři projektu. Když přidáte nový připojovací řetězec, můžete nastavit umístění databáze (*wingtiptoys.mdf*) má být sestaven v adresáři aplikace data (*aplikace\_Data*), místo jeho výchozí umístění. Tato změna vám umožní vyhledat a zkontrolujte soubor databáze dále v tomto kurzu.

1. V **Průzkumníka řešení**, najít a otevřít *Web.config* souboru.
2. Přidat připojovací řetězec, který je zvýrazněn žlutě, aby `<connectionStrings>` část *Web.config* to následujícím způsobem:  

    [!code-xml[Main](create_the_data_access_layer/samples/sample6.xml?highlight=4-7)]

Při prvním spuštění aplikace, sestaví databáze v umístění zadaném připojovacím řetězcem. Ale před spuštěním aplikace, můžeme ji nejdřív sestavit.

## <a name="building-the-application"></a>Sestavení aplikace

Pokud chcete mít jistotu, že všechny třídy a změny do vaší webové aplikace fungovat, měli byste vytvořit aplikaci.

1. Z **ladění** nabídce vyberte možnost **sestavení Northwind**.  
 **Výstup** se zobrazí okno, a pokud všechny se podařilo, zobrazí se *úspěšné* zprávy.  

    ![Vytvoření vrstvy přístupu k datům – výstupní Windows](create_the_data_access_layer/_static/image4.png)

Pokud narazíte na chybu, zkontrolujte znovu proveďte následující kroky. Informace v **výstup** okno označí soubor, který problém, kde v souboru je požadována změna. Tyto informace vám umožní určit, jaká část výše uvedené kroky potřeba zkontrolovali a opravili ve vašem projektu.

## <a name="summary"></a>Souhrn

V tomto kurzu této série budete mít vytvoření datového modelu, jakož i, přidat kód, který se použije k inicializaci a přidání dat do databáze. Také jste nakonfigurovali aplikaci, aby používala datové modely při spuštění aplikace.

V dalším kurzu budete aktualizaci uživatelského rozhraní, přidání navigace a načtení dat z databáze. Výsledkem bude databáze se automaticky vytvoří podle tříd entit, které jste vytvořili v tomto kurzu.

## <a name="additional-resources"></a>Další prostředky

[Přehled Entity Framework](https://msdn.microsoft.com/library/bb399567.aspx)   
[Průvodce pro začátečníky ADO.NET Entity Framework](https://msdn.microsoft.com/data/ee712907)   
[První vývoj pomocí rozhraní Entity Framework Code](http://www.msteched.com/2010/Europe/DEV212) (video)   
[Kód první vztahy Fluent API](https://msdn.microsoft.com/data/hh134698)   
[Kód první datové poznámky](https://msdn.microsoft.com/data/gg193958)  
[Vylepšení produktivity pro Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)

> [!div class="step-by-step"]
> [Předchozí](create-the-project.md)
> [další](ui_and_navigation.md)
