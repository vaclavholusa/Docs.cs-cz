---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Vytvoření databáze | Dokumentace Microsoftu
author: microsoft
description: Krok 2 ukazuje postup vytvoření databáze, která uchovává všechny společnosti dinner a potvrďte svou účast ještě data pro naši aplikaci NerdDinner.
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: 2a2ed8c91aa3ec0da63cd8e8686ff601f6e66374
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818963"
---
<a name="create-a-database"></a>Vytvoření databáze
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Toto je krok 2 bezplatné [kurz vývoje aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , který procházení procházení po tom, jak sestavit malý, ale bylo možné provést, webové aplikace pomocí ASP.NET MVC 1.
> 
> Krok 2 ukazuje postup vytvoření databáze, která uchovává všechny společnosti dinner a potvrďte svou účast ještě data pro naši aplikaci NerdDinner.
> 
> Pokud používáte ASP.NET MVC 3, doporučujeme je provést [získávání začít s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.


## <a name="nerddinner-step-2-creating-the-database"></a>NerdDinner krok 2: Vytvoření databáze

Popíšeme databázi k ukládání všech dat Dinner a RSVP pro naši aplikaci NerdDinner.

Následující postup popisuje vytvoření databáze pomocí bezplatné edice systému SQL Server Express (které si můžete snadno nainstalovat pomocí verzí V2 [instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)). Budeme psát kód spolupracuje s SQL Server Express a plnou instalaci SQL serveru.

### <a name="creating-a-new-sql-server-express-database"></a>Vytvoření nové databáze systému SQL Server Express

Použijeme začněte tím, že pravým tlačítkem myši na náš webový projekt a pak vyberte **Add -&gt;nová položka** příkazu nabídky:

![](create-a-database/_static/image1.png)

Tím se otevře dialogové okno sady Visual Studio "Přidat novou položku". Vytvoříme filtrovat podle kategorie "Data" a vyberte šablonu položky "SQL Server Database":

![](create-a-database/_static/image2.png)

Používáme bude název databáze systému SQL Server Express, které chceme vytvořit "NerdDinner.mdf" a stiskněte ok. Visual Studio se pak požádejte nás pokud chceme přidat tento soubor do našich \App\_adresář dat (což je adresář již instalace pomocí oprávnění ke čtení a zápis přístupu (ACL)):

![](create-a-database/_static/image3.png)

Můžeme vám tlačítko "Ano" a naši novou databázi, bude vytvořen a přidán do našich Průzkumníku řešení:

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a>Vytváření tabulek v rámci naší databázi

Nyní je k dispozici nové prázdné databáze. Přidejme do ní několik tabulek.

K tomu budete přejdeme na kartě okna "Průzkumník serveru" v sadě Visual Studio, které umožňuje spravovat databáze a servery. SQL Server Express databáze uložené ve \App\_složky dat naší aplikace se automaticky zobrazí v Průzkumníku serveru. Jsme můžete volitelně použít ikonu "Připojení k databázi" v horní části okna "Průzkumník serveru" přidat další databáze systému SQL Server (místní a vzdálené) i do seznamu:

![](create-a-database/_static/image5.png)

Přidáme dvě tabulky na naše databáze NerdDinner – jeden pro uložení naše večeří a druhou pro sledování přijetí reakce na ně. Vytvoříme nové tabulky tak, že pravým tlačítkem na složku "Tabulky" v rámci naší databázi a zvolte příkaz "Přidat novou tabulku" nabídky:

![](create-a-database/_static/image6.png)

Tím se otevře návrháře tabulky, která umožňuje konfigurovat schématu naší tabulky. Pro náš "Večeří" přidáme 10 sloupce dat:

![](create-a-database/_static/image7.png)

Chceme, aby sloupec "DinnerID" na jedinečné primární klíč pro tabulku. Můžeme to nakonfigurovat tak, že kliknete pravým tlačítkem na sloupec "DinnerID" a zvolíte položku nabídky "Nastavit primární klíč":

![](create-a-database/_static/image8.png)

Kromě toho DinnerID primární klíč, můžeme také vhodné ho nakonfigurovat jako sloupec "identity", jehož hodnota je automatický navýšeno při přidání nové řádky dat do tabulky (to znamená, první řádek vloženého Dinner bude mít DinnerID 1, druhý vložit řádek bude mít DinnerID 2 atd).

Můžeme to udělat tak, že vyberete sloupec "DinnerID" a potom použít editor "Vlastnosti sloupce" nastavit vlastnost "(je identita)" sloupce na "Ano". Budeme používat standardní identity výchozí hodnoty (začínají znakem 1 a zvýší 1 na každý řádek novou Dinner):

![](create-a-database/_static/image9.png)

Potom uložíme také našeho zadáním Ctrl + S nebo s použitím **souboru -&gt;Uložit** příkazu nabídky. Zobrazí se výzva nám název tabulky. Používáme bude název "Večeří":

![](create-a-database/_static/image10.png)

Naše nová tabulka večeří se pak zobrazí v rámci naší databázi v Průzkumníku serveru.

Pak vytvoříme opakujte předchozí postup a vytvořte tabulku "RSVP". Tato tabulka s obsahovat 3 sloupce. Budeme nastavit sloupec RsvpID jako primární klíč a také pomáhají zajistit sloupec identity:

![](create-a-database/_static/image11.png)

Můžeme vám ho uložte a přiřaďte jí název "RSVP".

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a>Vytvoření cizího klíče relace mezi tabulkami

Nyní je k dispozici dvě tabulky v databázi. Naše posledním krokem návrhu schématu bude nastavení "jedna k mnoha" relaci mezi těmito dvěma tabulkami – tak, aby nám každý řádek Dinner přidružit nula nebo více řádků reakce, vztahující se k němu. Uděláme to tím, že nakonfigurujete tabulce RSVP "DinnerID" sloupci vztah cizího klíče na sloupci "DinnerID" v tabulce "Večeří".

Provedete to tak otevřeme RSVP tabulku v Návrháři tabulky na něj poklikejte v Průzkumníku serveru. Pak vybereme sloupec "DinnerID" v ní, klikněte pravým tlačítkem a zvolte příkaz "Relationshps …" kontextové nabídky:

![](create-a-database/_static/image12.png)

Tím se otevře dialogové okno, které můžete použít k nastavení relace mezi tabulkami:

![](create-a-database/_static/image13.png)

Budete kliknete na tlačítko "Přidat" do dialogového okna Přidat novou relaci. Po přidání relace jsme budete napravo od dialogového okna rozbalte uzel stromové zobrazení "Tabulky a sloupce specifikace" v mřížce vlastností a pak klikněte na tlačítko "..." napravo od jeho:

![](create-a-database/_static/image14.png)

Kliknutím na tlačítko "..." se zobrazí další dialog, který umožňuje určit, které tabulky a sloupce jsou součástí relace, jakož i umožňují nám název relace.

Změníme na "Večeří" primární klíč tabulky a vyberte "DinnerID" sloupce v tabulce večeří jako primární klíč. Naše reakce tabulka bude tabulka cizího klíče a RSVP. Sloupec DinnerID bude přiřazen jako cizí klíč:

![](create-a-database/_static/image15.png)

Každý řádek v tabulce RSVP teď bude spojená s řádek v tabulce večeři. SQL Server se udržují referenční integritu pro nás – a zabránit nám přidáváte nový řádek RSVP neukazuje na platném řádku Dinner. To zároveň zabrání nám odstraňování Dinner řádku, pokud jsou stále potvrďte svou účast na něj odkazující řádky.

### <a name="adding-data-to-our-tables"></a>Přidání dat do naší tabulky

Pojďme dokončit tak, že přidáte nějaká ukázková data do našich večeří tabulky. Můžeme přidat data do tabulky kliknutím pravým tlačítkem na něj v Průzkumníkovi serveru a zvolte příkaz "Zobrazit Data tabulky":

![](create-a-database/_static/image16.png)

Přidáme několik řádků Dinner data, která můžeme použít později, jako jsme počáteční implementaci aplikace:

![](create-a-database/_static/image17.png)

### <a name="next-step"></a>Dalším krokem

Jsme dokončili vytváření databáze. Pojďme teď vytvořit tříd modelu, pomocí kterých můžeme pro dotazování a aktualizaci.

> [!div class="step-by-step"]
> [Předchozí](create-a-new-aspnet-mvc-project.md)
> [další](build-a-model-with-business-rule-validations.md)
