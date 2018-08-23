---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
title: Vytvoření tříd modelu pomocí Entity Framework (VB) | Dokumentace Microsoftu
author: microsoft
description: V tomto kurzu se dozvíte, jak používat technologie ASP.NET MVC s Entity Framework společnosti Microsoft. Zjistíte, jak používat průvodce Entity k vytvoření ADO.NET Entity Da...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: ff8322c9-12f3-4e24-aba6-a38046b9bb0d
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
msc.type: authoredcontent
ms.openlocfilehash: c1f64f57d4c23fe225a8268042104254e17dc456
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753221"
---
<a name="creating-model-classes-with-the-entity-framework-vb"></a>Vytvoření tříd modelu pomocí Entity Framework (VB)
====================
podle [Microsoft](https://github.com/microsoft)

> V tomto kurzu se dozvíte, jak používat technologie ASP.NET MVC s Entity Framework společnosti Microsoft. Zjistíte, jak používat průvodce Entity k vytvoření datového modelu Entity ADO.NET. V průběhu tohoto kurzu jsme vytvořit webovou aplikaci, která ukazuje, jak vybrat, vkládání, aktualizace a odstranění dat z databáze pomocí Entity Frameworku.


Cílem tohoto kurzu je vysvětlují, jak můžete vytvořit datové třídy přístup pomocí Microsoft Entity Framework při sestavování aplikace ASP.NET MVC. Tento kurz předpokládá žádné předchozí informace o Microsoft Entity Framework. Na konci tohoto kurzu budete vědět, jak použít rozhraní Entity Framework pro výběr, vkládání, aktualizaci a odstranění záznamů databáze.

Microsoft Entity Framework je nástroj mapování relací objektů (O/RM), která umožňuje automaticky generovat vrstvy přístupu k datům z databáze. Entity Framework vám umožní předcházet pracné vytváření tříd pro přístup k vaší data ručně.

> [!NOTE] 
> 
> Neexistuje žádná základní připojení mezi ASP.NET MVC a Entity Framework společnosti Microsoft. Existuje několik alternativ k Entity Frameworku, který vám pomůže s architekturou ASP.NET MVC. Například můžete vytvořit pomocí jiných nástrojů O/RM, jako je například Microsoft LINQ to SQL nebo NHibernate, SubSonic třídách modelu MVC.


Aby bylo možné ukazují, jak můžete Microsoft Entity Framework s architekturou ASP.NET MVC, vytvoříme jednoduchou ukázkovou aplikaci. Vytvoříme aplikace Movie Database, která umožňuje zobrazit a upravovat záznamy databáze filmů.

Tento kurz předpokládá, že máte Visual Studio 2008 nebo Visual Web Developer 2008 s aktualizací Service Pack 1. Chcete-li použít rozhraní Entity Framework musíte aktualizací Service Pack 1. Visual Studio 2008 Service Pack 1 nebo s aktualizací Service Pack 1 Visual Web Developer si můžete stáhnout z následující adresy:

> [https://www.asp.net/downloads/](https://www.asp.net/downloads)


## <a name="creating-the-movie-sample-database"></a>Vytvoření ukázkové databáze filmů

Databáze filmů aplikace používá databázové tabulky s názvem filmy, který obsahuje následující sloupce:

| Název sloupce | Datový typ | Povolit hodnoty Null? | Je primární klíč? |
| --- | --- | --- | --- |
| ID | int | False | Hodnota TRUE |
| Název | Nvarchar(100) | False | False |
| Ředitel | Nvarchar(100) | False | False |

V této tabulce můžete přidat do projektu aplikace ASP.NET MVC pomocí následujících kroků:

1. Klikněte pravým tlačítkem na aplikaci\_složce dat v okně Průzkumníka řešení a vyberte možnost nabídky **přidat, nová položka.**
2. Z **přidat novou položku** dialogu **databázi systému SQL Server**, zadejte název MoviesDB.mdf databáze a klikněte na **přidat** tlačítko.
3. Poklikejte na soubor MoviesDB.mdf otevření okna Průzkumník serveru/Průzkumník databáze.
4. Rozbalte MoviesDB.mdf připojení k databázi, klikněte pravým tlačítkem na složku tabulky a vyberte možnost nabídky **přidat novou tabulku**.
5. V Návrháři tabulek přidejte Id, název a ředitel pro sloupce.
6. Klikněte na tlačítko **Uložit** tlačítko (má ikonu disketová) uložit novou tabulku s názvem videa.

Jakmile vytvoříte tabulku databáze filmů, měli byste přidat nějaká ukázková data do tabulky. Klikněte pravým tlačítkem na tabulku filmy a vyberte možnost nabídky **zobrazit Data tabulky**. Můžete zadat data o filmech falešné do mřížky, která se zobrazí.

## <a name="creating-the-adonet-entity-data-model"></a>Vytvoření datového modelu ADO.NET Entity

Pokud chcete používat rozhraní Entity Framework, budete muset vytvořit Entity Data Model. Můžete využít Visual Studio *Průvodce datovým modelem Entity* k automatickému vygenerování modelu Entity Data Model z databáze.

Postupujte podle těchto kroků:

1. Klikněte pravým tlačítkem na složku modely v okně Průzkumník řešení a vyberte možnost nabídky **přidat, nová položka**.
2. V **přidat novou položku** dialogového okna, vyberte kategorii dat (viz obrázek 1).
3. Vyberte **datový Model Entity ADO.NET** šablony, dejte název MoviesDBModel.edmx modelu Entity Data Model a klikněte na tlačítko **přidat** tlačítko. Kliknutím **přidat** tlačítko spustí Průvodce datovým modelem.
4. V **výběr obsahu modelu** krok, zvolte **Generovat z databáze** možnost a klikněte na tlačítko **Další** tlačítko (viz obrázek 2).
5. V **vyberte datové připojení** kroku vyberte MoviesDB.mdf připojení k databázi, zadejte entity, nastavení připojení MoviesDBEntities pojmenujte a klikněte na tlačítko **Další** tlačítko (viz obrázek 3).
6. V **zvolte vaše databázové objekty** kroku, vyberte v tabulce databáze filmů a klikněte na tlačítko **Dokončit** tlačítko (viz obrázek 4).

Po dokončení těchto kroků se otevře ADO.NET Entity Data Model Designer (návrháři entit).

**Obrázek 1 – Vytvoření nového modelu Entity Data Model**

![clip_image002](creating-model-classes-with-the-entity-framework-vb/_static/image1.jpg)

**Obrázek 2 – zvolte Model obsahu krok**

![clip_image004](creating-model-classes-with-the-entity-framework-vb/_static/image2.jpg)

**Obrázek 3: Vyberte datové připojení**

![clip_image006](creating-model-classes-with-the-entity-framework-vb/_static/image3.jpg)

**Obrázek 4 – Zvolte vaše databázové objekty**

![clip_image008](creating-model-classes-with-the-entity-framework-vb/_static/image4.jpg)

## <a name="modifying-the-adonet-entity-data-model"></a>Úprava ADO.NET Entity Data Model

Po vytvoření modelu Entity Data Model můžete upravit model s využitím v návrháři entit (viz obrázek 5). V návrháři entit můžete kdykoli otevřít dvojitým kliknutím na soubor MoviesDBModel.edmx obsažené ve složce modely v rámci okna Průzkumník řešení.

**Obrázek 5 – ADO.NET Entity Data Model Designer**

![clip_image010](creating-model-classes-with-the-entity-framework-vb/_static/image5.jpg)

Například v návrháři entit můžete změnit názvy tříd, které Průvodce entitního modelu dat generuje. Průvodce vytvoří novou třídu přístupu dat s názvem videa. Jinými slovy Průvodce přiřadil třídy velmi stejný název jako databázové tabulky. Protože tato třída budeme používat k reprezentaci určité instance film, měli bychom změnit název třídy z videa, která se video.

Pokud chcete přejmenovat třídu entity, můžete dvakrát klikněte na název třídy v návrháři entit a zadejte nový název (viz obrázek 6). Alternativně můžete změnit název entity v okně Vlastnosti po výběru entity v návrháři entit.

**Obrázek 6 – Změna názvu entity**

![clip_image012](creating-model-classes-with-the-entity-framework-vb/_static/image6.jpg)

Nezapomeňte si uložit modelu Entity Data Model po provedení změny kliknutím na tlačítko Save (ikonu diskety). Na pozadí v návrháři entit generuje sadu tříd jazyka Visual Basic .NET. Tyto třídy můžete zobrazit tak, že otevřete soubor MoviesDBModel.Designer.vb z okna Průzkumníka řešení.


Neupravujte kód v souboru Designer.vb, protože vaše změny budou přepsány při příštím použití v návrháři entit. Pokud chcete k rozšíření funkčnosti tříd entit, které jsou definovány v souboru Designer.vb pak můžete vytvořit *částečné třídy* v samostatných souborů.


#### <a name="selecting-database-records-with-the-entity-framework"></a>Výběr záznamů Database s Entity Framework

Můžeme pustit do vytvoření aplikace Movie Database tak, že vytvoříte stránku, která se zobrazí seznam záznamů video. Kontroler Home v informacích 1 zpřístupňuje akci s názvem Index(). Akce Index() vrátí všechny záznamy video z databázové tabulky Movie s využitím rozhraní Entity Framework.

**Výpis 1 – Controllers\HomeController.vb**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample1.vb)]

Všimněte si, že řadič v informacích 1 obsahuje konstruktor. Konstruktor inicializuje pole třídu úrovně s názvem \_db. \_Db pole představuje entity databáze vygenerovaným rozhraním Entity Framework společnosti Microsoft. \_Db pole je instance třídy MoviesDBEntities, který byl vygenerován v návrháři entit.

\_Db pole se používá v rámci akce Index() k načtení záznamů v tabulce databáze filmů. Výraz \_db. MovieSet představuje všechny záznamy v tabulce databáze filmů. Metoda ToList() slouží k převedení sadu filmy do obecné kolekce filmů objektů: seznam (video z).

Záznamy film se načítají pomocí LINQ to Entities. Akce Index() ve výpisu 1 používá technologii LINQ *syntaxe využívající metody* k načtení sady záznamů v databázi. Pokud dáváte přednost, můžete použít LINQ *syntaxe dotazu* místo. Velmi stejnou věc udělat následující dva příkazy:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample2.vb)]

Syntaxe podle toho, která LINQ – syntaxe využívající metody nebo syntaxe dotazů –, která vás nejvíce intuitivní. Není žádný rozdíl ve výkonu mezi dva přístupy – jediným rozdílem je styl.

Zobrazení výpisu 2 se používá k zobrazení záznamů video.

**Listing 2 – Views\Home\Index.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample3.aspx)]

Obsahuje zobrazení, ve výpisu 2 **pro každou** smyčku, která prochází každý záznam filmů a zobrazí hodnoty vlastnosti Title a ředitel pro video záznam. Všimněte si, že úpravu a odstranění odkazu se zobrazí vedle každého záznamu. Kromě toho na odkaz přidat video se zobrazí v dolní části zobrazení (viz obrázek 7).

**Obrázek 7 – zobrazení indexu**

![clip_image014](creating-model-classes-with-the-entity-framework-vb/_static/image7.jpg)

Index zobrazení je *typu zobrazení*. Obsahuje zobrazení indexu &lt;% @ Page %&gt; směrnice, který obsahuje atribut Inherits. Atribut Inherits přetypování ViewData.Model vlastnost, která má silného typu obecného seznamu kolekce objektů film – seznam (video z).

## <a name="inserting-database-records-with-the-entity-framework"></a>Vkládání záznamů Database s Entity Framework

Entity Framework můžete použít k tomu, aby na vkládání nových záznamů do databázové tabulky. Výpis 3 obsahuje dvě nové akce, které jsou přidány do třídy kontroleru Domovská stránka, která vám pomůže se vkládání nových záznamů do databázové tabulky Movie.

**Výpis 3 – Controllers\HomeController.vb (metody Add)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample4.vb)]

První akci Add() jednoduše vrací zobrazení. Zobrazení formuláře pro přidání nové databáze filmů obsahuje záznam (viz obrázek 8). Při odeslání formuláře se vyvolá druhou akci Add().

Všimněte si, že druhou akci Add() je upravena pomocí atributů AcceptVerbs. Tuto akci lze vyvolat pouze při provádění operace HTTP POST. Jinými slovy tato akce se dá vyvolat jen při odesílání formuláře HTML.

Druhou akci Add() vytvoří novou instanci třídy film Entity Framework pomocí metody ASP.NET MVC TryUpdateModel(). Metoda TryUpdateModel() přijímá pole v FormCollection předaný metodě Add() a přiřadí hodnoty těchto polí formuláře HTML na třídu video.


Při použití rozhraní Entity Framework, je třeba zadat "prázdný seznam" vlastnosti při použití metody TryUpdateModel nebo UpdateModel k aktualizaci vlastností třídu entity.


V dalším kroku Add() akci provádí nějaké jednoduchý formulář ověření. Akce ověří, zda název a ředitel pro vlastnosti mají hodnoty. Pokud dojde k chybě ověřování, se přidá chybovou zprávu ověření pro ModelState.

Pokud zde nejsou žádné chyby ověření nového záznamu video se přidá do tabulky databáze filmů s použitím Entity Framework. Nový záznam se přidá do databáze s následujícími dvěma řádky kódu:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample5.vb)]

První řádek kódu přidá nové video entity na sadu filmy sledované rozhraním Entity Framework. Druhý řádek kódu uloží jakékoli změny byly provedeny na filmy sledované zpět do podkladové databáze.

**Obrázek 8: Přidat zobrazení**

![clip_image016](creating-model-classes-with-the-entity-framework-vb/_static/image8.jpg)

## <a name="updating-database-records-with-the-entity-framework"></a>Aktualizace záznamů Database s Entity Framework

Můžete postupovat podle téměř stejným způsobem upravit záznam v databázi s rozhraním Entity Framework jako si přístup, který jsme právě a potom k vložení nového záznamu v databázi. Výpis 4 obsahuje dvě nové akce kontroleru s názvem Edit(). Vrátí první akci Edit() formuláře HTML pro úpravu záznamu video. Druhou akci Edit() pokusy o aktualizaci databáze.

**Část 4 – Controllers\HomeController.vb (metod Edit)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample6.vb)]

Druhou akci Edit() spustí načtením video záznam z databáze, která odpovídá Id film, který právě upravujete. Následující technologie LINQ to Entities příkaz vezme prvního záznamu v databázi, která odpovídá konkrétní Id:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample7.vb)]

V dalším kroku metoda TryUpdateModel() umožňuje přiřadit vlastnosti entity film hodnoty pole formuláře HTML. Všimněte si, že je seznamu povolených dodané zadávat přesné vlastnosti k aktualizaci.

V dalším kroku se provádí nějaké jednoduché ověření Pokud chcete ověřit, zda mají obě název filmu ředitel vlastnosti a hodnoty. Pokud žádnou vlastnost chybí hodnota, chybovou zprávu ověření je přidán do ModelState a ModelState.IsValid vrátí hodnotu false.

Nakonec pokud nejsou žádné chyby ověření, pak podkladové databázové tabulky filmy je aktualizován změny zavoláním metody SaveChanges().

Při úpravě záznamů v databázi, je potřeba předat Id záznamu upravovaný na akce kontroleru, který provádí aktualizace databáze. Akce kontroleru, jinak nebude vědět, který záznam se má aktualizovat v podkladové databázi. Zobrazení pro úpravy součástí výpis 5 obsahuje skryté pole formuláře, který představuje Id databáze záznamu, který právě upravujete.

**Listing 5 – Views\Home\Edit.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a>Odstraňování záznamů databáze s rozhraním Entity Framework

Finální databázová operace, která potřebujeme k zpracování v tomto kurzu, odstraňuje záznamy v databázi. Akce kontroleru v zobrazení 6 slouží k odstranění záznamu v konkrétní databázi.

**Výpis 6 – \Controllers\HomeController.vb (akci odstranění)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample9.vb)]

Akce Delete() nejdřív načte videa, entity, která odpovídá Id předány do akce. V dalším kroku video odstraněna z databáze pomocí volání metody DeleteObject(), za nímž následuje metoda SaveChanges(). Nakonec je uživatel přesměrován zpět na zobrazení indexu.

## <a name="summary"></a>Souhrn

Účelem tohoto kurzu bylo ukazují, jak se dají vytvářet databázově řízeného webových aplikací s využitím ASP.NET MVC a Entity Framework společnosti Microsoft. Jste zjistili, jak sestavit aplikaci, která umožňuje vybrat, vložit, aktualizovat a odstraňovat záznamy v databázi.

Nejprve jsme zmínili, jak můžete Průvodce datovým modelem Entity k vygenerování datového modelu Entity ze sady Visual Studio. V dalším kroku se dozvíte, jak použít technologii LINQ to Entities k načtení sady záznamů v databázi z databázové tabulky. Nakonec jsme použili rozhraní Entity Framework pro vložení, aktualizace a odstranění záznamů databáze.

> [!div class="step-by-step"]
> [Předchozí](validation-with-the-data-annotation-validators-cs.md)
> [další](creating-model-classes-with-linq-to-sql-vb.md)
