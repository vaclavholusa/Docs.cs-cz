---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
title: Vytvořit Data Access Layer | Microsoft Docs
author: Erikre
description: Tento kurz řady naučit se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a Microsoft Visual Studio Express 2013 pro jsme...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 0bbf7a6e-d7eb-4091-91e4-fff892777f32
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
msc.type: authoredcontent
ms.openlocfilehash: 671d1bbf661dfb3e56c6ccd67ce0d383990918d6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="create-the-data-access-layer"></a>Vytvořit Data Access Layer
====================
Podle [Erik Reitan](https://github.com/Erikre)

[Stáhnout adresář Wingtip Toys ukázkového projektu (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) nebo [stáhnout elektronická kniha (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Tento kurz řady naučit se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a Microsoft Visual Studio Express 2013 pro Web. Visual Studio 2013 [projekt pomocí zdrojového kódu C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) dispozici je pro tento kurz řady.


Tento kurz popisuje, jak vytvářet, přístup a zkontrolujte data v databázi pomocí webových formulářů ASP.NET a Entity Framework Code First. V tomto kurzu vychází z předchozí kurzu "Vytvořit projektu" a je součástí řady kurz Wingtip hračka úložiště. Po dokončení tohoto kurzu, který bude jste vytvořili skupinu přístup k datům třídy, které jsou v *modely* složce projektu.

## <a name="what-youll-learn"></a>Získáte informace:

- Postup vytvoření datové modely.
- Jak k inicializaci a počáteční hodnoty databáze.
- Jak aktualizovat a nakonfigurovat aplikaci, aby podporovala databázi.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Tyto jsou funkce, zavedená v tomto kurzu:

- Rozhraní Entity Framework Code First
- LocalDB
- Datových poznámek

## <a name="creating-the-data-models"></a>Vytváření modelů dat

[Rozhraní Entity Framework](https://msdn.microsoft.com/data/aa937723) představuje rozhraní objektu relační mapování (ORM). Umožňuje pracovat s relačních dat jako objekty, odstraňuje většinu kódu přístup k datům, které by obvykle potřebujete k zápisu. Používající rozhraní Entity Framework, můžete vydávat dotazy pomocí [LINQ](https://msdn.microsoft.com/library/bb397926.aspx), načítat a pracovat s daty jako objektů se silným typem. LINQ poskytuje vzory pro dotazování a aktualizace dat. Používající rozhraní Entity Framework umožňuje zaměřit se na vytváření zbytek vaší aplikace, nikoli zaměřené na data základy přístup. Později v kurzu této série jsme budete ukazují, jak tato data použít k naplnění navigaci a produktu dotazy.

Rozhraní Entity Framework podporuje vývoj zlepší, nazývá *Code First*. Kód nejprve umožňuje definovat modely dat pomocí třídy. Třída je konstruktor, který umožňuje vytvořit vlastní typy společně seskupením proměnné jiné typy, metod a události. Můžete třídy map k existující databázi nebo použít ke generování databáze. V tomto kurzu vytvoříte datové modely vytvořením třídy datového modelu. Potom budete umožní vytvořit databázi na za chodu z tyto nové třídy rozhraní Entity Framework.

Začneme s vytváření tříd entit, které definují datové modely pro aplikace webových formulářů. Pak vytvoříte kontextu třídu, která spravuje tříd entit a poskytuje přístup k datům do databáze. Pokud vytvoříte třídu inicializátoru, který budete používat k naplnění databáze.

### <a name="entity-framework-and-references"></a>Rozhraní Entity Framework a odkazy

Ve výchozím nastavení, je součástí rozhraní Entity Framework, když vytvoříte novou **webové aplikace ASP.NET** pomocí **webových formulářů** šablony. Rozhraní Entity Framework můžete nainstalovaný, odinstalovat a aktualizovat, protože balíček NuGet.

Tento balíček NuGet zahrnuje následující **runtime** sestavení v rámci projektu:

- EntityFramework.dll – všechny běžné runtime kód používá rozhraní Entity Framework
- EntityFramework.SqlServer.dll – Microsoft SQL Server zprostředkovatele Entity Framework

### <a name="entity-classes"></a>Třídy entity

Třídy vytvoříte definovat schéma dat se nazývají tříd entit. Pokud jste nový návrh databáze, představit jako definice tabulek databáze tříd entit. Každou vlastnost v třídě určuje sloupec v tabulce databáze. Tyto třídy poskytují jednoduché objekt relační rozhraní mezi kódem objektově orientované a struktura tabulky relační databáze.

V tomto kurzu budete Začněte přidáním jednoduché entity třídy představující schémata pro produkty a kategorie. Třída produkty, bude obsahovat definice pro každý produkt. Název každého člena třídy produktu budou `ProductID`, `ProductName`, `Description`, `ImagePath`, `UnitPrice`, `CategoryID`, a `Category`. Kategorie třídy bude obsahovat definice pro každou kategorii, která produkt patří, například Car, člun nebo roviny. Název každého člena třídy kategorie bude `CategoryID`, `CategoryName`, `Description`, a `Products`. Každý produkt bude patřit do jedné z kategorií. Tyto třídy entita se zařadí do projektu existující *modely* složky.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *modely* složku a potom vyberte **přidat**  - &gt; **novou položku**. 

    ![Data Access Layer - vytvořit nové položky nabídky](create_the_data_access_layer/_static/image1.png)

   **Přidat novou položku** se zobrazí dialogové okno.
2. V části **Visual C#** z **nainstalovaná** podokna na levé straně vyberte **kód**. 

    ![Data Access Layer - vytvořit nové položky nabídky](create_the_data_access_layer/_static/image2.png)
3. Vyberte **třída** v prostředním podokně a pojmenujte tato nová třída *Product.cs*.
4. Klikněte na tlačítko **přidat**.  
   Nový soubor třídy se zobrazí v editoru.
5. Ve výchozím kódu nahraďte následujícím kódem:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample1.cs)]
6. Vytvořit jinou třídu opakováním kroků 1 až 4, ale pojmenujte novou třídu *Category.cs* a ve výchozím kódu nahraďte následujícím kódem:  

    [!code-csharp[Main](create_the_data_access_layer/samples/sample2.cs)]

Jak už jsme zmínili, `Category` třídy představuje typ produktu, který aplikace je navržen tak, aby je prodal (například <a id="a"> </a> &quot;aut&quot;, &quot;lodě&quot;, &quot;Rockets&quot;a tak dále) a `Product` třída reprezentuje jednotlivé produkty (toys) v databázi. Každá instance `Product` objektu bude odpovídat na řádek v tabulce relační databáze, a každou vlastnost třídy produktu se namapuje na sloupec v tabulce relační databáze. Později v tomto kurzu zkontrolujete produktu data obsažená v databázi.

### <a name="data-annotations"></a>Datových poznámek

Jste si všimli splnit určité členy třídy atributů, zadáním podrobností o člena, jako například `[ScaffoldColumn(false)]`. Jedná se o *datových poznámek*. Atributy datového poznámky můžete popisují, jak k ověření vstupu uživatele pro tento člen, můžete určit formátování pro něj a určete, jak je modelován při vytvoření databáze.

### <a name="context-class"></a>Context – třída

Pokud chcete začít používat třídy pro přístup k datům, je nutné zadat třídu kontextu. Jak je uvedeno nahoře, třídy kontextu spravuje tříd entit (například `Product` třídy a `Category` – třída) a poskytuje přístup k datům do databáze.

Tento postup přidá novou C# kontextu třídu k *modely* složky.

1. Klikněte pravým tlačítkem myši *modely* složku a potom vyberte **přidat**  - &gt; **novou položku**.   
   **Přidat novou položku** se zobrazí dialogové okno.
2. Vyberte **třída** v prostředním podokně název *ProductContext.cs* a klikněte na tlačítko **přidat**.
3. Nahraďte kód výchozích obsaženy v třídě následujícím kódem:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample3.cs)]

Tento kód přidá `System.Data.Entity` obor názvů tak, aby měli přístup k základním funkcím Entity Framework, který zahrnuje možnosti dotazu, vložit, aktualizovat a odstranit data ve spolupráci s objektů se silným typem.

`ProductContext` Třída reprezentuje kontext databáze produktu Entity Framework, která zpracovává načítání, ukládání a aktualizace `Product` třídy instance v databázi. `ProductContext` Třída odvozená z `DbContext` základní třída poskytuje rozhraní Entity Framework.

### <a name="initializer-class"></a>Inicializátor – třída

Musíte se ke spuštění některých vlastní logiky k chybě při inicializaci databáze první čas, který se používá kontextu. To vám umožní počáteční data mají být přidány do databáze, tak, že můžete okamžitě zobrazení kategorií a produkty.

Tento postup přidá novou C# inicializátoru třídu k *modely* složky.

1. Vytvořte další `Class` v *modely* složku a pojmenujte ji *ProductDatabaseInitializer.cs*.
2. Nahraďte kód výchozích obsaženy v třídě následujícím kódem:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample4.cs)]

Jak je vidět ve výše uvedeném kódu, když je vytvořen a inicializován, databázi `Seed` přepsat a nastavit vlastnost. Když `Seed` vlastnost nastavena, hodnoty z kategorií a produkty, které se používají k naplnění databáze. Pokud se pokusíte aktualizovat data počáteční hodnoty úpravou ve výše uvedeném kódu po vytvoření databáze, neuvidíte žádné aktualizace při spuštění webové aplikace. Důvodem je výše uvedený kód používá provádění `DropCreateDatabaseIfModelChanges` třída rozpoznat, pokud model (schéma) byl změněn před obnovením dat počáteční hodnoty. Pokud jsou provedeny žádné změny `Category` a `Product` tříd entit, databáze nebude znovu inicializována daty počáteční hodnoty.

> [!NOTE] 
> 
> Pokud byste chtěli databáze znovu vytvořit pokaždé, když byla aplikace spuštěná, můžete použít `DropCreateDatabaseAlways` místo `DropCreateDatabaseIfModelChanges` třídy. Ale pro tento kurz řady, použijte `DropCreateDatabaseIfModelChanges` třídy.


V tomto okamžiku v tomto kurzu budete mít *modely* složku s čtyři nové třídy a jeden výchozí třídy:

![Vytvořit Data Access Layer - složku modely](create_the_data_access_layer/_static/image3.png)

### <a name="configuring-the-application-to-use-the-data-model"></a>Konfigurace aplikace použít datový Model

Teď, když jste vytvořili třídy, které představují data, musíte nakonfigurovat aplikaci, aby používala třídy. V *Global.asax* soubor, přidejte kód, který inicializuje modelu. V *Web.config* přidat informace, které aplikace řekne, co jste databáze budete používat k ukládání dat, která je reprezentována nové třídy datového souboru. *Global.asax* soubor lze použít pro zpracování události aplikace nebo metody. *Web.config* souboru umožňuje řídit konfiguraci webové aplikace ASP.NET.

#### <a name="updating-the-globalasax-file"></a>Aktualizace na soubor Global.asax

K chybě při inicializaci datové modely při spuštění aplikace, aktualizujte `Application_Start` obslužné rutiny v *Global.asax.cs* souboru.

> [!NOTE] 
> 
> V Průzkumníku řešení, můžete vybrat, zda *Global.asax* souboru nebo *Global.asax.cs* soubor pro úpravu *Global.asax.cs* souboru.


1. Přidejte následující kód zvýrazněných v žlutý k `Application_Start` metoda v *Global.asax.cs* souboru.   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample5.cs?highlight=9-10,22-23)]

> [!NOTE] 
> 
> Váš prohlížeč musí podporovat HTML5 zobrazíte kód zvýrazněných v žlutě při prohlížení tento kurz řady v prohlížeči.


Viz výše uvedený kód při spuštění aplikace, aplikace určuje, že je přístupný inicializátoru, který se spustí při prvním data. Dva další obory názvů jsou nutné pro přístup `Database` objektu a `ProductDatabaseInitializer` objektu.

 Úprava souboru Web.Config. 

I když Entity Framework Code First vygeneruje databáze pro vás ve výchozím umístění při počáteční data se zobrazí v databázi, přidání vlastní informace o připojení k vaší aplikaci vám kontrolu nad umístění databáze. Zadejte toto připojení k databázi pomocí připojovacího řetězce do aplikace *Web.config* souboru v kořenovém adresáři projektu. Můžete přidat nový připojovací řetězec, můžete nastavit umístění databáze (*wingtiptoys.mdf*) má být sestaven v adresáři data aplikace (*aplikace\_Data*), místo jeho výchozí umístění. Provedení této změny vám umožní najít a zkontrolujte soubor databáze později v tomto kurzu.

1. V **Průzkumníku řešení**, najít a otevřít *Web.config* souboru.
2. Přidat připojovací řetězec zvýrazněných v žlutý k `<connectionStrings>` části *Web.config* následujícím způsobem:  

    [!code-xml[Main](create_the_data_access_layer/samples/sample6.xml?highlight=4-7)]

Při prvním spuštění aplikace, se bude vytvoření databáze na umístění, které připojovací řetězec. Ale před spuštěním aplikace, Vytvořme ji nejdřív.

## <a name="building-the-application"></a>Sestavení aplikace

Abyste měli jistotu, že všechny třídy a změny webové aplikace fungovat, měli byste vytvořit aplikaci.

1. Z **ladění** nabídce vyberte možnost **sestavení Northwind**.  
 **Výstup** okno se zobrazí, a pokud všechny se i, se zobrazí *byla úspěšná* zprávy.  

    ![Vytvořit Data Access Layer - oknech výstupu](create_the_data_access_layer/_static/image4.png)

Pokud spustíte došlo k chybě, znovu zkontrolujte výše uvedené kroky. Informace v **výstup** okno označí, který soubor má problém a kdy v souboru změnu je potřeba. Tyto informace vám umožní určit, jaká část výše uvedené kroky nutné k revizi a pevné ve vašem projektu.

## <a name="summary"></a>Souhrn

V tomto kurzu řady můžete mít vytvořit datový model, jakož i, přidat kód, který se použije k inicializaci a počáteční hodnoty databáze. Také jste nakonfigurovali aplikaci, aby používala datové modely, když je aplikace spuštěna.

V dalším kurzu budete aktualizovat uživatelského rozhraní, přidat navigační a načtení dat z databáze. Výsledkem bude databáze automaticky vytváří podle tříd entit, které jste vytvořili v tomto kurzu.

## <a name="additional-resources"></a>Další prostředky

[Přehled Entity Framework](https://msdn.microsoft.com/library/bb399567.aspx)   
[Příručka začátečníka ADO.NET Entity Framework](https://msdn.microsoft.com/data/ee712907)   
[První vývoj pomocí rozhraní Entity Framework Code](http://www.msteched.com/2010/Europe/DEV212) (video)   
[Kód první vztahy rozhraní Fluent API](https://msdn.microsoft.com/data/hh134698)   
[Kód první datových poznámek](https://msdn.microsoft.com/data/gg193958)  
[Vylepšení produktivitu rozhraní Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)

> [!div class="step-by-step"]
> [Předchozí](create-the-project.md)
> [další](ui_and_navigation.md)
