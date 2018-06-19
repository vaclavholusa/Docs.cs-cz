---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Část 4: Modely a přístup k datům | Microsoft Docs'
author: jongalloway
description: Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště. Část 4 obsahuje modely a přístup k datům.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 76671bbc7050d111b4d156c45584ba5aa4f1ea8f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879475"
---
<a name="part-4-models-and-data-access"></a>Část 4: Modely a přístup k datům
====================
podle [Jon Galloway](https://github.com/jongalloway)

> Úložiště Hudba MVC je kurz aplikace, která představuje a vysvětluje krok za krokem, jak používat rozhraní ASP.NET MVC a Visual Studio pro vývoj webů.  
>   
> Úložiště Hudba MVC je implementace úložiště lightweight ukázkové, který prodává hudebních alb online a implementuje základní Správa serveru, přihlášení uživatele a nákupního košíku funkce.
> 
> Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště. Část 4 obsahuje modely a přístup k datům.


Zatím jsme jste právě byla předání "fiktivní data" z našich řadičů naše zobrazení šablony. Nyní jsme připraveni spojit skutečné databáze. V tomto kurzu jsme budete pokrývajících jak používat SQL Server Compact Edition (často říká SQL CE) jako naše databázového stroje. SQL CE je databáze založená na souborech volné, embedded, která nevyžaduje žádné instalace nebo konfigurace, který je to skutečně vhodné pro místní vývoj.

## <a name="database-access-with-entity-framework-code-first"></a>Přístup k databázi pomocí Entity Framework kódu – první

Použijeme podporu Entity Framework (EF), který je součástí projekty ASP.NET MVC 3 pro dotazování a aktualizaci databáze. EF je relační mapování dat (ORM) rozhraní API, které umožňuje vývojářům dotazů a aktualizace dat uložených v databázi způsobem objektově orientované na objekt flexibilní.

Rozhraní Entity Framework verze 4 podporuje vývoj zlepší, názvem code first. První kód vám umožní vytvořit objekt modelu vytvořením jednoduchého třídy (také označované jako objektů POCO z objektů CLR "prostý starý") a můžete dokonce vytvořit databázi na za chodu z tříd.

### <a name="changes-to-our-model-classes"></a>Změny v našich třídy modelu

Budeme v tomto kurzu využívat funkci vytvoření databáze v Entity Framework. Než to, ale umožňuje provádět několik malých změn naše tříd modelu pro přidání v několik věcí, které budeme později na používat.

#### <a name="adding-the-artist-model-classes"></a>Přidání třídy modelu umělcem

Naše alb bude spojený s umělci, takže přidáme třídu jednoduchého modelu k popisu umělcem. Přidejte novou třídu do modely složku s názvem Artist.cs pomocí kódu vidíte níže.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a>Aktualizace našich třídy modelu

Aktualizace třídy pro alb, jak je uvedeno níže.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

Dále vytvořte následující aktualizace Genre třídy.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a>Přidání aplikace\_složky dat

Přidáme aplikace\_adresář dat do našich projektu pro uložení naše soubory databáze SQL Server Express. Aplikace\_dat je speciální adresář v technologii ASP.NET, která už má správné zabezpečení přístupová oprávnění pro přístup k databázi. V projektu nabídce vyberte Přidat složku ASP.NET a pak aplikaci\_Data.

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a>Vytvoření připojovacího řetězce v souboru web.config

Konfigurační soubor webu přidáme pár řádků, tak, aby rozhraní Entity Framework umí připojit do databáze hotový. Poklikejte na soubor Web.config umístěný v kořenovém adresáři projektu.

![](mvc-music-store-part-4/_static/image2.png)

Přejděte do dolní části tohoto souboru a přidejte &lt;connectionStrings&gt; části přímo nad poslední řádek, jak je uvedeno níže.

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a>Přidání třídy kontextu

Klikněte pravým tlačítkem na složku modely a přidejte novou třídu s názvem MusicStoreEntities.cs.

![](mvc-music-store-part-4/_static/image3.png)

Tato třída bude představují kontext databáze Entity Framework, bude zpracovávat naše vytvořit, číst, aktualizovat a odstranit operací pro nás. Kód pro tuto třídu je uveden níže.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

Je to – neexistuje žádná další konfigurace, speciální rozhraní, atd. Rozšířením základní třídy DbContext Naše třída MusicStoreEntities je schopná zpracovat naše databázových operací pro nás. Teď, když My jsme který připojili, umožňuje přidávat několik další vlastnosti pro naše třídy modelu chcete využít výhod některé další informace o našich databáze.

### <a name="adding-our-store-catalog-data"></a>Přidání naše data katalogu úložiště

Rozhraní Entity Framework, který přidá "počáteční" dat do nově vytvořené databáze jsme bude využít výhod funkce. Toto bude naše katalog store se seznamem žánry, umělci a alb přednastavit. Stahování MvcMusicStore Assets.zip - která součástí naší soubory návrhu lokality použité v tomto kurzu - má soubor třídy s těmito daty počáteční hodnoty, umístěný ve složce s názvem kódu.

V rámci kód / složku modely, vyhledejte soubor SampleData.cs a umístěte jej do složky modely v našem projektu, jak je uvedeno níže.

![](mvc-music-store-part-4/_static/image4.png)

Teď je potřeba přidat jeden řádek kódu rozhraní Entity Framework říct o SampleData třídy. Poklikejte na soubor Global.asax v kořenovém adresáři projektu otevřete a přidejte následující řádek na začátek aplikace\_Start – metoda.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

V tomto okamžiku dokončení práce potřebné ke konfiguraci Entity Framework pro naše projekt.

## <a name="querying-the-database"></a>Dotaz na databázi

Teď umožňuje aktualizovat naše StoreController tak, aby místo použití "fiktivní data" místo zavolá do databáze k dotazování všechny jeho informace. Začneme deklarováním pole na **StoreController** pro uložení instance třídy MusicStoreEntities s názvem storeDB:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a>Aktualizace Index úložiště pro dotazování databáze

Třída MusicStoreEntities se spravuje pomocí rozhraní Entity Framework a zpřístupňuje vlastnost kolekce pro každou tabulku v databázi. Umožňuje aktualizovat naše StoreController indexu akce načtení všech žánry v naší databázi. Dříve jsme se to pevně kódováno datech řetězce. Nyní můžete místo toho právě používáme kontext Entity Framework Generes kolekce:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

Žádné změny, musí dojít naše zobrazit šablonu, protože jsme se stále vrací stejnou StoreIndexViewModel jsme před - jsme se právě vrácení dynamická data z databáze teď vrátil.

Když jsme spusťte projekt znovu a navštívíte adresu "/ ukládání", jsme se zobrazí seznam všech žánry v naší databázi:

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a>Aktualizace procházet úložiště a podrobnosti používat živá data

S/úložiště/procházet? genre =*[některá genre]* metody akce, jsme při hledání Genre podle názvu. Očekáváme pouze jeden výsledek, vzhledem k tomu, že jsme někdy by neměl mít dvě položky na stejný název Genre, a proto jsme můžete použít. Rozšíření Single() v technologii LINQ dotaz na příslušný objekt Genre takto (nevkládejte to ještě):

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

Jeden metoda přebírá jako parametr, který určuje, že má jeden objekt Genre tak, aby jeho název odpovídá hodnotě, kterou jsme jste definovali výrazu Lambda. V případě výše jsme se jeden objekt Genre načítání s hodnotou název odpovídající Disco.

Provedeme výhod funkce rozhraní Entity Framework, která umožňuje nám udávajících dalších entit v relaci, kterou chceme také načíst, pokud je objekt Genre načteny. Tato funkce je volána Shaping výsledek dotazu a umožňuje nám chcete snížit počet, kolikrát je potřeba přístup k databázi načíst všechny informace, které potřebujeme. Chceme předem načíst alb pro Genre načteme, takže jsme budete aktualizovat dotaz zahrnout z Genres.Include("Albums") indikující, že má také související alb. To je efektivnější, protože ho načte naše Genre a alb data v požadavku jedné databáze.

S vysvětlením stranou zde je, jak vypadá naše aktualizované akce kontroleru procházení:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

Nyní jsme můžete aktualizovat zobrazení procházet úložiště tak, aby alb, které jsou k dispozici v jednotlivých Genre. Otevřete zobrazení šablony (v /Views/Store/Browse.cshtml) a přidejte seznamu s odrážkami alb, jak je uvedeno níže.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

Spuštění aplikace a procházení/úložiště / procházet? genre = Jazz ukazuje, které jsou nyní probíhá načtený naše výsledky z databáze, zobrazení všech alb v našem vybrané Genre.

![](mvc-music-store-part-4/_static/image2.jpg)

Vytočit stejné změnit na našem/Store/podrobnosti / [id] adresy URL a nahraďte naše fiktivní data databázový dotaz, který načte Album s ID odpovídá hodnotě parametru.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

Spuštění aplikace a procházení k /Store/Details/1 ukazuje, že naše výsledky se nyní patrně z databáze.

![](mvc-music-store-part-4/_static/image5.png)

Teď, když chcete zobrazit album podle ID alb je nastavili naší stránce s podrobnostmi o úložiště, můžeme aktualizovat **Procházet** zobrazení pro odkaz na zobrazení podrobností. Použijeme Html.ActionLink, přesně, jako jsme to udělali propojení z úložiště indexu procházet úložiště na konci v předchozí části. Úplný zdrojový Procházet zobrazení se zobrazí pod.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

Nemohli jsme teď moct přejít z naší stránce s úložiště na stránku Genre, kde jsou uvedeny dostupné alb, a kliknutím na album jsme můžete zobrazit podrobnosti o této alb.

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> [Předchozí](mvc-music-store-part-3.md)
> [další](mvc-music-store-part-5.md)
