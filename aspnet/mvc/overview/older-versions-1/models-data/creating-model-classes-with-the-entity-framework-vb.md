---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
title: "Vytvoření třídy modelu pomocí rozhraní Entity Framework (VB) | Microsoft Docs"
author: microsoft
description: "V tomto kurzu zjistěte, jak používat rozhraní ASP.NET MVC s Microsoft Entity Framework. Dozvíte, jak používat průvodce Entity k vytvoření ADO.NET Entity Da..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: ff8322c9-12f3-4e24-aba6-a38046b9bb0d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
msc.type: authoredcontent
ms.openlocfilehash: efc190d856fe9ebf1c09e0ae4758aabb1e3254dc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="creating-model-classes-with-the-entity-framework-vb"></a>Vytvoření třídy modelu pomocí rozhraní Entity Framework (VB)
====================
podle [Microsoft](https://github.com/microsoft)

> V tomto kurzu zjistěte, jak používat rozhraní ASP.NET MVC s Microsoft Entity Framework. Zjistíte, jak používat průvodce Entity k vytvoření ADO.NET Entity Data Model. V průběhu tohoto kurzu jsme sestavení webové aplikace, které ukazuje, jak vybrat, vložit, aktualizovat a odstraňovat data databáze s použitím rozhraní Entity Framework.


Cílem tohoto kurzu je vysvětlují, jak můžete vytvořit datové třídy přístup pomocí rozhraní Entity Framework Microsoft při vytváření aplikace ASP.NET MVC. Tento kurz předpokládá žádné předchozí informace o Microsoft Entity Framework. Na konci tohoto kurzu budete vědět, jak pomocí rozhraní Entity Framework můžete vybrat, vložit, aktualizovat a odstraňovat záznamů databáze.

Rozhraní Entity Framework Microsoft je nástroj na objekt relační mapování (O/RM), která umožňuje automaticky generovat vrstva přístupu k datům z databáze. Rozhraní Entity Framework umožňuje vyhnout pracné vytváření tříd přístup data ručně.

> [!NOTE] 
> 
> Neexistuje žádná základní připojení mezi ASP.NET MVC a Microsoft Entity Framework. Existuje několik alternativy k Entity Frameworku, který můžete použít s architekturou ASP.NET MVC. Například můžete vytvořit pomocí jiných nástrojů O/RM, například Microsoft LINQ to SQL, NHibernate nebo SubSonic tříd modelu MVC.


Chcete-li ilustrují, jak můžete použít rozhraní Entity Framework Microsoft s architekturou ASP.NET MVC, využijeme jednoduché ukázkovou aplikaci. Vytvoříme film databázovou aplikaci, která umožňuje zobrazit a upravit film záznamů databáze.

Tento kurz předpokládá, že máte Visual Studio 2008 nebo Visual Web Developer 2008 s aktualizací Service Pack 1. Chcete-li použít rozhraní Entity Framework musíte aktualizací Service Pack 1. Visual Studio 2008 Service Pack 1 nebo Visual Web Developer s aktualizací Service Pack 1 si můžete stáhnout z následující adresy:

> [https://www.ASP.NET/downloads/](https://www.asp.net/downloads)


## <a name="creating-the-movie-sample-database"></a>Vytváření film ukázkové databáze

Aplikace film databáze používá tabulku databáze s názvem filmy, která obsahuje následující sloupce:

| Název sloupce | Datový typ | Povolit hodnoty Null? | Je primární klíč? |
| --- | --- | --- | --- |
| ID | int | False | Hodnota TRUE |
| Název | Nvarchar(100) | False | False |
| Adresář nacházející | Nvarchar(100) | False | False |

V této tabulce můžete přidat k projektu ASP.NET MVC pomocí následujících kroků:

1. Klikněte pravým tlačítkem na aplikaci\_složky dat v okna Průzkumníka řešení a vyberte možnost nabídky **přidat, nové položky.**
2. Z **přidat novou položku** dialogové okno, vyberte **databáze systému SQL Server**, dejte název MoviesDB.mdf databáze a klikněte **přidat** tlačítko.
3. Poklikejte na soubor MoviesDB.mdf a otevřete okno Průzkumníka serveru Průzkumníka a databáze.
4. Rozbalte MoviesDB.mdf připojení k databázi, klikněte pravým tlačítkem na složku tabulky a vyberte možnost nabídky **přidat novou tabulku**.
5. Návrháře tabulky přidejte Id, názvu a ředitel sloupce.
6. Klikněte na tlačítko **Uložit** tlačítko (má ikonu disketová) pro uložení v nové tabulce s názvem filmy.

Po vytvoření databázové tabulky filmy, měli byste přidat ukázková data do tabulky. Klikněte pravým tlačítkem v tabulce filmy a vyberte možnost nabídky **zobrazit Data tabulky**. Můžete zadat data falešných film v mřížce, který se zobrazí.

## <a name="creating-the-adonet-entity-data-model"></a>Vytvoření modelu ADO.NET Entity Data Model

Chcete-li použít rozhraní Entity Framework, musíte vytvořit datového modelu Entity. Můžete využít výhod sady Visual Studio *Entity Data Model Wizard* má automaticky generovat datového modelu Entity z databáze.

Postupujte podle těchto kroků:

1. Klikněte pravým tlačítkem na složku modely v okně Průzkumníka řešení a vyberte možnost nabídky **přidat, nové položky**.
2. V **přidat novou položku** dialogovém okně, vyberte kategorii dat (viz obrázek 1).
3. Vyberte **ADO.NET Entity Data Model** šablony, dejte název MoviesDBModel.edmx datového modelu Entity a klikněte na **přidat** tlačítko. Kliknutím **přidat** tlačítko spustí průvodce k datovému modelu.
4. V **zvolte obsah modelu** kroku, vyberte **generování z databáze** možnost a klikněte na tlačítko **Další** tlačítko (viz obrázek 2).
5. V **vybrat datové připojení** krok, vyberte připojení k databázi MoviesDB.mdf, zadejte entity nastavení připojení MoviesDBEntities a klikněte na **Další** tlačítko (viz obrázek 3).
6. V **zvolte si databázové objekty** krok, vyberte v tabulce databáze film a klikněte **Dokončit** tlačítko (viz obrázek 4).

Po dokončení těchto kroků se otevře ADO.NET Entity Data Model Designer (Návrhář Entity).

**Obrázek 1 – Vytvoření nové Entity Data Model**

![clip_image002](creating-model-classes-with-the-entity-framework-vb/_static/image1.jpg)

**Obrázek 2 – zvolte Model obsah krok**

![clip_image004](creating-model-classes-with-the-entity-framework-vb/_static/image2.jpg)

**Obrázek 3: zvolit datové připojení**

![clip_image006](creating-model-classes-with-the-entity-framework-vb/_static/image3.jpg)

**Obrázek 4 – zvolte databázové objekty**

![clip_image008](creating-model-classes-with-the-entity-framework-vb/_static/image4.jpg)

## <a name="modifying-the-adonet-entity-data-model"></a>Úprava ADO.NET Entity Data Model

Po vytvoření datového modelu Entity, můžete měnit modelu, a využít tak výhod Entity Designer (viz obrázek 5). Entity Designer můžete kdykoli otevřít dvojitým kliknutím na soubor MoviesDBModel.edmx obsažené ve složce modely v rámci okna Průzkumníka řešení.

**Obrázek 5 – ADO.NET Entity Data Model Designer**

![clip_image010](creating-model-classes-with-the-entity-framework-vb/_static/image5.jpg)

Například můžete Entity Designer změnit názvy tříd, které generuje Průvodce datového modelu Entity. Průvodce vytvoří novou třídu přístup dat s názvem filmy. Jinými slovy Průvodce přiřadil třída velmi stejný název jako databázové tabulky. Vzhledem k tomu, že tato třída použijeme k reprezentaci určité instance film, jsme přejmenujte třídy z filmy na film.

Pokud chcete přejmenovat třídu entity, můžete dvakrát klikněte na název třídy Entity Designer a zadejte nový název (viz obrázek 6). Alternativně můžete změnit název entity v okně vlastností po výběru entity v Entity designeru.

**Obrázek 6 – změna název entity**

![clip_image012](creating-model-classes-with-the-entity-framework-vb/_static/image6.jpg)

Nezapomeňte uložit datového modelu Entity po provedení změny kliknutím na tlačítko Uložit (ikona disketu). Entity Designer na pozadí, generuje sadu tříd jazyka Visual Basic .NET. Tyto třídy můžete zobrazit tak, že otevřete soubor MoviesDBModel.Designer.vb z okna Průzkumníka řešení.


Neupravujte kód v souboru Designer.vb, protože vaše změny se přepíše při dalším spuštění použijte Entity Designer. Pokud chcete rozšířit funkce tříd entit, které jsou definované v souboru Designer.vb potom můžete vytvořit *částečné třídy* v samostatných souborů.


#### <a name="selecting-database-records-with-the-entity-framework"></a>Výběr záznamů databáze s platformou Entity Framework

Začněme, sestavení aplikace film databáze tak, že vytvoříte stránky, který zobrazí seznam film záznamy. Domovské řadiče v výpis 1 zpřístupní akci s názvem Index(). Akce Index() vrátí všechny záznamy film z tabulky databáze film a využívají k rozhraní Entity Framework.

**Výpis 1 – Controllers\HomeController.vb**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample1.vb)]

Všimněte si, že řadiče v výpis 1 obsahuje konstruktor. Konstruktor inicializuje na úrovni pole s názvem \_db. \_Db pole představuje entity databáze vygenerované Microsoft Entity Framework. \_Db pole je instance třídy MoviesDBEntities, který byl vytvořen v Entity designeru.

\_Db pole v rámci akce Index() slouží k načtení záznamů z filmy databázové tabulky. Výraz \_db. MovieSet představuje všechny záznamy z filmy databázové tabulky. Metoda ToList() slouží k převedení sadu filmy do obecnou kolekci objektů film: seznamu (film z).

Záznamy film se načítají pomocí LINQ to Entities. Akce Index() v výpis 1 používá LINQ *syntaxe využívající metody* načíst sadu záznamů databáze. Pokud dáváte přednost, můžete použít LINQ *syntaxe dotazu* místo. Velmi stejnou věc udělat následující dva příkazy:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample2.vb)]

Syntaxí kteroukoli LINQ – syntaxe využívající metody nebo syntaxe dotazů –, které najít nejvíce intuitivní. Není žádný rozdíl ve výkonu mezi dva přístupy – jediným rozdílem je, styl.

Zobrazení v výpis 2 se používá k zobrazení film záznamy.

**Výpis 2 – Views\Home\Index.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample3.aspx)]

Obsahuje zobrazení v výpis 2 **pro každou** smyčky, který iteruje v rámci všech vybraných záznamech film a zobrazuje hodnoty vlastností nadpisu a ředitel film záznamu. Všimněte si, že vedle každý záznam zobrazí odkaz upravit a odstranit. Kromě toho se zobrazí odkaz s přidat film v dolní části zobrazení (viz obrázek 7).

**Obrázek 7 – zobrazení indexu**

![clip_image014](creating-model-classes-with-the-entity-framework-vb/_static/image7.jpg)

Index zobrazení *zadali zobrazení*. Obsahuje zobrazení indexu &lt;% @ % stránky&gt; direktiva, která obsahuje atribut Inherits. Atribut Inherits vrhá ViewData.Model vlastnost silného typu Obecné seznamu kolekci objektů film – seznam (film z).

## <a name="inserting-database-records-with-the-entity-framework"></a>Vkládání záznamů databáze s platformou Entity Framework

Rozhraní Entity Framework můžete snadno vkládání nových záznamů do databázové tabulky. Výpis 3 obsahuje dvě nové akce, které jsou přidány do třídy kontroleru Home, můžete použít k vkládání nových záznamů do tabulky databáze film.

**Výpis 3 – Controllers\HomeController.vb (Přidat metody)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample4.vb)]

Je první akcí Add() jednoduše vrátí zobrazení. Zobrazení obsahuje formulář pro novou databázi film přidávání záznamů (viz obrázek 8). Při odesílání formuláře, je vyvolána druhá Add() akce.

Všimněte si, že druhou akci Add() opatřen atributem AcceptVerbs. Tato akce může vyvolat pouze při provádění operace HTTP POST. Jinými slovy tato akce může být volána, pouze když publikování formuláře HTML.

Druhou akci Add() vytvoří novou instanci třídy Entity Framework film pomocí metody ASP.NET MVC TryUpdateModel(). Metoda TryUpdateModel() trvá polí v FormCollection předaný metodě Add() a přiřadí k třídě film hodnoty těchto polí formuláře HTML.


Pokud používáte rozhraní Entity Framework, je nutné zadat "seznam povolených" vlastností, které při použití metody TryUpdateModel nebo UpdateModel aktualizovat vlastnosti třídu entity.


V dalším kroku Add() akci provádí některé základní ověřování. Akce ověří, zda název i ředitel vlastnosti mají hodnoty. Pokud dojde k chybě ověření, se přidá chybovou zprávu ověření pro ModelState.

Pokud nejsou žádné chyby ověření nový záznam film přidat do tabulky databáze filmy pomocí rozhraní Entity Framework. Nový záznam je přidán do databáze s následující dva řádky kódu:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample5.vb)]

První řádek kódu přidá novou entitu film sadu filmy sledovaných pomocí rozhraní Entity Framework. Druhý řádek kódu uloží na filmy sledovaných zpět do základní databáze byly provedeny jakékoli změny.

**Obrázek 8: Přidat zobrazení**

![clip_image016](creating-model-classes-with-the-entity-framework-vb/_static/image8.jpg)

## <a name="updating-database-records-with-the-entity-framework"></a>Aktualizace záznamů v databázi pomocí Entity Framework

Můžete provést téměř stejný přístup upravit záznam v databázi pomocí Entity Framework jako přístup, který jsme právě a vložit nový záznam v databázi. Výpis 4 obsahuje dvě nové akce kontroleru s názvem Edit(). Je první akcí Edit() vrátí formuláře HTML pro úpravu záznamu film. Druhou akci Edit() pokusy o aktualizaci databáze.

**Výpis 4 – Controllers\HomeController.vb (upravit metody)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample6.vb)]

Druhou akci Edit() spustí načtením film záznam z databáze, která odpovídá Id film Upravovaný. Následující technologie LINQ to Entities příkaz získá na první záznam databáze, který odpovídá konkrétní Id:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample7.vb)]

V dalším kroku metoda TryUpdateModel() umožňuje přiřadit vlastnosti entity film hodnoty polí formuláře HTML. Všimněte si, seznamu povolených jsou dodané zadávat přesné vlastnosti aktualizace.

V dalším kroku se provádí některé jednoduché ověřování a ověření, že název filmu i ředitel vlastnosti hodnoty. Pokud buď vlastnost chybí hodnota, chybovou zprávu ověření se přidá do ModelState a ModelState.IsValid vrátí hodnotu false.

Nakonec pokud nejsou žádné chyby ověření, potom základní tabulky databáze filmy je aktualizovány všechny změny pomocí volání metody SaveChanges().

Při úpravě záznamů databáze, musíte předat s Id záznamu upravovaný k akci kontroleru, který provádí aktualizaci databáze. Akce kontroleru, jinak nebude vědět, které záznam aktualizovat v podkladové databázi. Upravit zobrazení, obsažené v výpis 5, obsahuje skryté pole formuláře představující Id upravovaný záznamů databáze.

**Výpis 5 – Views\Home\Edit.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a>Odstraňování záznamů databáze s platformou Entity Framework

Operace konečné databáze, která potřebujeme tohoto problému v tomto kurzu, je odstranění záznamů databáze. Akce kontroleru v výpis 6 slouží k odstranění konkrétní databázový záznam.

**Výpis 6 – \Controllers\HomeController.vb (akci odstranění)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample9.vb)]

Akce Delete() nejprve načte film entita, která odpovídá Id předaný akce. V dalším kroku film odstraněna z databáze pomocí volání metody DeleteObject() následuje metodu SaveChanges(). Nakonec je uživatel přesměrován zpět na zobrazení indexu.

## <a name="summary"></a>Souhrn

Účelem tohoto kurzu bylo ukazují, jak můžete vytvořit databázových aplikací webové aplikace pomocí ASP.NET MVC a Microsoft Entity Framework. Jste zjistili, jak sestavit aplikaci, která umožňuje vybrat, vložit, aktualizovat a odstraňovat záznamů databáze.

Nejdřív jsme probrali použití průvodce Entity Data Model generovat Model dat Entity z Visual Studia. V dalším kroku zjistíte, jak používat technologie LINQ to Entities načíst sadu záznamů databáze z tabulky databáze. Nakonec jsme použili rozhraní Entity Framework vložit, aktualizovat a odstraňovat záznamů databáze.

>[!div class="step-by-step"]
[Předchozí](validation-with-the-data-annotation-validators-cs.md)
[další](creating-model-classes-with-linq-to-sql-vb.md)
