---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Vytvoření databáze | Microsoft Docs
author: microsoft
description: Krok 2 ukazuje postup vytvoření databáze, která uchovává všechny večeře a RSVP data pro naši aplikaci NerdDinner.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: ba28d671bf13ec54b83b876462e2c23f90310037
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869137"
---
<a name="create-a-database"></a>Vytvoření databáze
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Toto je krok 2 ze bezplatný [kurz aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , nevystavíte slabé stránky zabezpečení – prostřednictvím postup sestavení malá, ale dokončení, webové aplikace pomocí ASP.NET MVC 1.
> 
> Krok 2 ukazuje postup vytvoření databáze, která uchovává všechny večeře a RSVP data pro naši aplikaci NerdDinner.
> 
> Pokud používáte ASP.NET MVC 3, doporučujeme provedením [získávání spuštěna s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Hudba úložiště](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.


## <a name="nerddinner-step-2-creating-the-database"></a>NerdDinner krok 2: Vytvoření databáze

Budeme k ukládání všech dat večeři a zasílání zpráv rysy pro naši aplikaci NerdDinner používat databázi.

Následující postup popisuje vytvoření databáze pomocí edice free SQL Server Express (které si můžete snadno nainstalovat pomocí V2 z [instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)). Všechny kód jsme budete zápisu funguje pro SQL Server Express a úplným SQL serverem.

### <a name="creating-a-new-sql-server-express-database"></a>Vytvoření nové databáze SQL Server Express

Jsme budete začněte tím, že pravým tlačítkem myši na našem webový projekt a pak vyberte **Add -&gt;nová položka** příkazu v nabídce:

![](create-a-database/_static/image1.png)

Tím se otevře dialogové okno "Přidat novou položku" sady Visual Studio. Jsme budete filtrovat podle kategorie "Data" a vyberte šablonu, položka "Databáze systému SQL Server":

![](create-a-database/_static/image2.png)

Jsme budete název databáze systému SQL Server Express, kterou chcete vytvořit "NerdDinner.mdf" a stiskněte tlačítko ok. Visual Studio budou vyzváni k zadání Pokud nám chcete přidat tento soubor do našich \App\_adresář dat (která adresáře je již instalace s oprávnění ke čtení a zápis přístupu (ACL)):

![](create-a-database/_static/image3.png)

Budete kliknete na tlačítko Ano, a naše nová databáze bude vytvořen a přidán do našich Průzkumníku řešení:

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a>Vytváření tabulek v rámci naší databáze

Nyní je k dispozici novou prázdnou databázi. Přidejte do něj umožňuje některé tabulky.

K tomu budete jsme přejděte do okna Karta "Průzkumník serveru" v sadě Visual Studio, která umožňuje nám ke správě databází a serverů. SQL Server Express databáze uložené ve \App\_Data složky naše aplikace se automaticky zobrazí v Průzkumníku serveru. Jsme můžete volitelně použít ikonu "Připojení k databázi" horní okno "Průzkumníka serveru" přidat do seznamu také další databáze systému SQL Server (místní i vzdálený):

![](create-a-database/_static/image5.png)

Do databáze hotový NerdDinner – jeden pro uložení naše večeří a dalších sledovat přijetí zasílání zpráv rysy jim přidáme dvě tabulky. Pravým tlačítkem na složku "Tabulky" v rámci naší databáze a zvolením příkazu nabídky "Přidat novou tabulku" můžeme vytvořit nové tabulky:

![](create-a-database/_static/image6.png)

Tím se otevře návrháře tabulky, které umožňuje konfigurovat schéma naše tabulky. Pro naše "Večeří" tabulku přidáme 10 sloupce dat:

![](create-a-database/_static/image7.png)

Chceme mít jedinečné primární klíč pro tabulku sloupce "DinnerID". To jsme můžete nakonfigurovat tak, že kliknete pravým tlačítkem na sloupec "DinnerID" a vyberete položku nabídky "Nastavení primární klíč":

![](create-a-database/_static/image8.png)

Kromě vytváření DinnerID primární klíč, chceme také ji nakonfigurovat jako sloupec "identity", jehož hodnota je zvýšena automaticky při přidání nové řádky dat do tabulky (tj. první řádek vloženého večeři bude mít DinnerID 1, druhý vložit řádek bude mít DinnerID 2 atd).

Můžeme provést zaškrtnutím sloupci "DinnerID" a pomocí editoru "Vlastnosti sloupce" nastavte vlastnost "(je Identity)" ve sloupci "Ano". Budeme používat standardní identity výchozí hodnoty (začínají znakem 1 a zvýšit 1 každý nový řádek večeři):

![](create-a-database/_static/image9.png)

Budete pak uložíme naše tabulku zadáním Ctrl-S nebo pomocí **souboru -&gt;Uložit** příkazu nabídky. Zobrazí se výzva nám název tabulky. Jsme budete pojmenujte ji "Večeří":

![](create-a-database/_static/image10.png)

Naše nová tabulka večeří se potom zobrazí v rámci naší databáze v Průzkumníku serveru.

Potom jsme budete zopakujte výše uvedené kroky a vytvořit tabulku "Zasílání zpráv rysy". Tato tabulka s mít 3 sloupce. Nemůžeme nastavit sloupci RsvpID jako primární klíč a způsobují, že sloupec identity:

![](create-a-database/_static/image11.png)

Jsme budete ji uložit a pojmenujte ho názvu "Zasílání zpráv rysy".

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a>Nastavení cizí klíč relace mezi tabulkami

Nyní je k dispozici dvě tabulky v rámci naší databáze. Naše posledním krokem návrhu schématu bude nastavení "jedna k mnoha" relaci mezi těmito dvěma tabulkami – tak, aby jsme každý řádek večeři přidružit počtu nula či více řádků zasílání zpráv rysy, které se použije na ni. Uděláme konfigurace sloupce v tabulce zasílání zpráv rysy "DinnerID" tak, aby měl relaci cizího klíče na sloupec "DinnerID" v tabulce "Večeří".

K tomu otevřeme zasílání zpráv rysy tabulku v Návrháři tabulky dvojitým kliknutím na soubor v Průzkumníku serveru. Budete zvolte sloupci "DinnerID" v něm, klikněte pravým tlačítkem a vyberte "Relationshps..." příkaz nabídky kontextu:

![](create-a-database/_static/image12.png)

Tím se otevře dialogové okno, které jsme slouží k nastavení relace mezi tabulkami:

![](create-a-database/_static/image13.png)

Jsme budete kliknutím na tlačítko "Přidat" Přidat nový vztah do dialogu. Po přidání vztahu jsme budete rozbalte uzel zobrazení stromu "Tabulky a sloupce Specification" v rámci mřížku vlastností vpravo dialogového okna a potom klikněte na tlačítko "..." napravo od ho:

![](create-a-database/_static/image14.png)

Další dialog, který umožňuje určit, které tabulky a sloupce se účastní relace, jakož i název relace nám umožňují zobrazíte kliknutím na tlačítko "...".

Změníme na "Večeří" primární klíč tabulky a vyberte sloupec "DinnerID" v tabulce večeří jako primární klíč. Naše zasílání zpráv rysy tabulka bude cizího klíče tabulky a zasílání zpráv rysy. Sloupec DinnerID bude přiřazen jako cizí klíč:

![](create-a-database/_static/image15.png)

Každý řádek v tabulce zasílání zpráv rysy teď bude přidružen řádek v tabulce večeři. Systému SQL Server bude udržovat pro nás – referenční integrity a zabránit nám v přidání nového řádku zasílání zpráv rysy, pokud neukazuje na platném řádku večeři. Také nás nebude možné z odstraňování večeři řádku, pokud jsou stále RSVP řádků na ně odkazovat.

### <a name="adding-data-to-our-tables"></a>Přidání dat do našich tabulky

Umožňuje dokončit přidáním ukázková data do našich večeří tabulky. Budeme-li přidat data do tabulky, pravým tlačítkem myši na něm v Průzkumníku serveru a zvolte příkaz "Zobrazit Data tabulky":

![](create-a-database/_static/image16.png)

Přidáme pár řádků večeři dat, které jsme můžete použít později, jako jsme počáteční implementace aplikace:

![](create-a-database/_static/image17.png)

### <a name="next-step"></a>Další krok

Jsme dokončili vytváření databáze. Nyní vytvoříme třídy modelu, které jsme můžete použít k dotazu a aktualizovat ji.

> [!div class="step-by-step"]
> [Předchozí](create-a-new-aspnet-mvc-project.md)
> [další](build-a-model-with-business-rule-validations.md)
