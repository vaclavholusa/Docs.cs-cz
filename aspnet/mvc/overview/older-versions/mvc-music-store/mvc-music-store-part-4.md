---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: '4. část: Modely a přístup k datům | Dokumentace Microsoftu'
author: jongalloway
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store. 4. část se věnuje modely a přístup k datům.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: ea8fe623a1b59b80fd7f087036b9ed716eafadbe
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402035"
---
<a name="part-4-models-and-data-access"></a>4. část: Modely a přístup k datům
====================
podle [Jon Galloway](https://github.com/jongalloway)

> MVC Music Store jde o kurz, který se seznámíte, podrobné postupy pro vývoj pro web pomocí ASP.NET MVC a sady Visual Studio.  
>   
> Music Store MVC je jednoduché ukázku implementace úložiště prodává hudebních alb online, který implementuje správu základního webu, přihlášení uživatele a nákupního košíku funkce.
> 
> V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store. 4. část se věnuje modely a přístup k datům.


Zatím jsme jste právě byla předávání "fiktivní data" z našich řadičů naše zobrazení šablony. Nyní jsme připraveni připojení skutečná databáze. V tomto kurzu probereme jak používat SQL Server Compact Edition (často označované jako SQL CE) jako naše databázového stroje. SQL CE je databáze založená na bezplatné, embedded, souborech, který nevyžaduje, aby všechny instalace nebo konfigurace, která je to velmi vhodné pro místní vývoj.

## <a name="database-access-with-entity-framework-code-first"></a>Přístup k databázi pomocí Entity Framework Code-First

Použijeme podporu Entity Framework (EF), která je zahrnutá v projektech ASP.NET MVC 3 pro dotazování a aktualizaci databáze. EF je objekt flexibilní relační mapování dat (ORM určené) vývojářům umožňuje dotazování a aktualizace dat uložených v databázi způsobem objektově orientované rozhraní API.

Rozhraní Entity Framework verze 4 podporuje vývoj paradigma volá založeno na kódu. Založeno na kódu můžete vytvořit objekt modelu napsáním jednoduché třídy (označované také jako POCO z objektů CLR "prostý staré") a můžete dokonce vytvořit databázi v reálném čase z vaší třídy.

### <a name="changes-to-our-model-classes"></a>Změny v našich tříd modelu

Budeme v tomto kurzu využití funkce vytvoření databáze v Entity Framework. Než to, ale vytvoříme několik menší změny do našich tříd modelu přidat pár věcí, které budeme dále používat.

#### <a name="adding-the-artist-model-classes"></a>Přidání třídy modelu interpret

Naše alb bude spojená s umělci, tak přidáme jednoduchý model tříd k popisu umělce. Přidejte novou třídu do složky modelů s názvem Artist.cs pomocí kódu níže.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a>Aktualizace našich tříd modelu

Aktualizace třídy alb, jak je znázorněno níže.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

Do třídy rozšířením podle tematických dále, proveďte následující aktualizace.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a>Přidání aplikace\_složka dat

Přidáme aplikace\_adresář dat pro náš projekt k uložení naše soubory databáze serveru SQL Server Express. Aplikace\_dat je speciální adresář v technologii ASP.NET, která už má správné zabezpečení přístupová oprávnění pro přístup k databázi. V nabídce Projekt vyberte Přidat složku ASP.NET a pak aplikaci\_Data.

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a>Vytvoření připojovacího řetězce v souboru web.config

Konfigurační soubor webu přidáme několik řádků, tak, aby rozhraní Entity Framework ví, jak se připojit k databázi. Poklikejte na soubor Web.config umístěný v kořenovém adresáři projektu.

![](mvc-music-store-part-4/_static/image2.png)

Přejděte k dolnímu okraji tento soubor a přidat &lt;connectionStrings&gt; části přímo nad poslední řádek, jak je znázorněno níže.

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a>Přidání třídy kontextu

Klikněte pravým tlačítkem na složku modely a přidejte novou třídu s názvem MusicStoreEntities.cs.

![](mvc-music-store-part-4/_static/image3.png)

Tato třída bude představují kontext databáze Entity Framework, bude zpracování našich vytvořit, číst, aktualizovat a operací odstranění pro USA. Kód pro tuto třídu je uveden níže.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

To je vše – neexistuje žádná další konfigurace, speciální rozhraní, atd. Rozšířením základní třídy DbContext naše MusicStoreEntities třída je schopný zvládnout naše databázové operace pro nás. Teď, když máme, připojili, přidáme několik dalších vlastností do našich tříd modelu využívat některé další informace v databázi.

### <a name="adding-our-store-catalog-data"></a>Přidání naše ukládání katalogu dat

My podnikneme využívat funkce v Entity Framework, která přidá "seed" data na nově vytvořenou databázi. To se vyplní předem náš katalog úložiště se seznamem žánry, umělců nebo alb. Stahování MvcMusicStore Assets.zip – který součástí našich souborů návrhu lokality použitou dříve v tomto kurzu – má soubor třídy s těmito daty počáteční hodnoty, umístěný ve složce s názvem kódu.

V rámci kód / složku modely, vyhledejte soubor SampleData.cs a umístěte ho do složky modely v našem projektu, jak je znázorněno níže.

![](mvc-music-store-part-4/_static/image4.png)

Teď potřebujeme přidat jeden řádek kódu na rozhraní Entity Framework říct SampleData třídy. Poklikejte na soubor Global.asax v kořenovém adresáři projektu otevřete ho a přidejte následující řádek do horní části aplikace\_začátek metody.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

V tuto chvíli jsme dokončili práce potřebné ke konfiguraci Entity Framework pro náš projekt.

## <a name="querying-the-database"></a>Dotazování na databázi

Teď můžeme aktualizovat naše StoreController tak, aby místo použití "fiktivní data" místo toho zavolá naši databázi a všechny jeho informace o dotazování. Začneme deklarací pole na **StoreController** pro uložení instance třídy MusicStoreEntities s názvem storeDB:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a>Aktualizuje se Index Store dáte dotaz na databázi

Třída MusicStoreEntities se spravuje pomocí Entity Framework a zpřístupňuje vlastnost kolekce pro každou tabulku v databázi. Umožňuje aktualizovat naše StoreController Index akci k načtení všech žánry v databázi. Tento postup již jsme provedli podle pevného kódování data řetězce. Teď můžeme místo jednoduše použít na kontext Entity Framework Generes kolekce:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

Je nutné dojde k naší zobrazit šablonu, protože jsme se stále vrací stejné StoreIndexViewModel jsme vrátil před - jsme už jenom vrací živá data z naší databáze nyní žádné změny.

Když jsme spusťte projekt znovu, přejděte na adresu URL "/ Store" jsme zobrazí seznam všech žánry v naší databázi:

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a>Aktualizuje se Store Procházet a podrobnosti o použití živých dat

S/Store/procházet? žánr =*[některé žánr]* metodě akce, Prohledáváme pro rozšířením podle tematických podle názvu. Pouze Očekáváme, že jeden výsledek, protože jsme neustále by neměl mít dvě položky se stejným názvem žánr a používat. Metoda Single() rozšíření v LINQ dotazu pro příslušný objekt žánr takto (nevkládejte to zatím):

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

Jedinou metodu používá výraz Lambda jako parametr, který určuje, že má jeden objekt žánr tak, aby jeho název odpovídá hodnotě, kterou jsme definovali. V případě výše se načítají jeden objekt žánr s názvem hodnotu, která odpovídá roz.

Provedeme výhod funkce služby Entity Framework, která umožňuje určit další související entity, které chceme, aby načíst i když je načten objekt žánr. Tato funkce je volána tvarování výsledek dotazu a umožňuje nám to snížit počet, kolikrát budeme potřebovat pro přístup k databázi chcete načíst všechny informace, které potřebujeme. Chceme předběžného načítání alb pro žánr se nám načíst, abychom budete aktualizovat dotaz mají zahrnout z Genres.Include("Albums") k označení, že má být také související alb. To je mnohem efektivnější, protože ho se načtou naše žánr a alb data v žádosti o izolované databáze.

S vysvětlením eliminuje zde je, jak vypadá naše aktualizované akce kontroleru Procházet:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

Aktualizujeme můžete nyní procházet zobrazení tak, aby alb, které jsou k dispozici v každý žánr Store. Otevřete zobrazit šablonu (ve /Views/Store/Browse.cshtml) a přidejte seznam s odrážkami alb, jak je znázorněno níže.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

Spuštění aplikace a přejdete do/Store/procházet? žánr = Jazz odhalí naše výsledky jsou teď právě načtený z databáze, zobrazení všech alb v našich vybrané žánr.

![](mvc-music-store-part-4/_static/image2.jpg)

Uděláme stejnou změnit na naší/Store/podrobnosti / [id] adresy URL a naše fiktivní data nahraďte databázového dotazu, který načte alba, jejíž ID odpovídá hodnota parametru.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

Spuštění aplikace a přejdete do /Store/Details/1 ukazuje, že naše výsledky se nyní se berou z databáze.

![](mvc-music-store-part-4/_static/image5.png)

Teď, když naší stránce s podrobnostmi o Store je nastaven pro zobrazení alba podle ID alba, můžeme aktualizovat **Procházet** zobrazit odkaz na zobrazení podrobností. Použijeme Html.ActionLink, přesně tak, jak jsme to udělali pro propojení z indexu Store procházet Store na konci předchozí části. Úplný zdrojový Procházet zobrazení se zobrazí pod.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

Nyní jsme schopni z naši stránku Store přejít na stránku Genre, který vypíše dostupná alb, a kliknutím na alba jsme můžete zobrazit podrobnosti o této alba.

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> [Předchozí](mvc-music-store-part-3.md)
> [další](mvc-music-store-part-5.md)
