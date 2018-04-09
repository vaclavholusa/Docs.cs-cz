---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
title: Pomocí CascadingDropDown s databází (VB) | Microsoft Docs
author: wenz
description: Ovládací prvek CascadingDropDown v Toolkitu AJAX rozšiřuje ovládací prvek rozevírací seznam tak, aby změny v jedné rozevírací seznam zatížení přidružené hodnoty v anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 97a3d33c-c856-43f3-8acb-f1ccbc48221a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 4fd5d2b28540f6e2f751da9fb9d0df5f9995c20b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="using-cascadingdropdown-with-a-database-vb"></a>Pomocí CascadingDropDown s databází (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1VB.pdf)

> Ovládací prvek CascadingDropDown v Toolkitu AJAX rozšiřuje ovládací prvek rozevírací seznam tak, aby se změny v jedné rozevírací seznam zatížení přidružené hodnoty v jiné rozevírací seznam. Aby tato možnost fungovala musí být vytvořeny speciální webové služby.


## <a name="overview"></a>Přehled

Ovládací prvek CascadingDropDown v Toolkitu AJAX rozšiřuje ovládací prvek rozevírací seznam tak, aby se změny v jedné rozevírací seznam zatížení přidružené hodnoty v jiné rozevírací seznam. (Například jeden seznam obsahuje seznam nám stavy a další seznamu je pak vyplněn hlavní města v tomto stavu.) Aby tato možnost fungovala musí být vytvořeny speciální webové služby.

## <a name="steps"></a>Kroky

První řadě zdroj dat je vyžadován. Tato ukázka používá databázi AdventureWorks a Microsoft SQL Server 2005 Express Edition. Databáze je volitelná součást instalaci sady Visual Studio (včetně express edition) a je také k dispozici jako samostatný soubor ke stažení v rámci [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Databázi AdventureWorks je součástí sad SQL Server 2005 ukázky a ukázkové databáze (stáhnout na adrese [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Nejjednodušší způsob, jak nastavit databáze je použití Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) a připojte `AdventureWorks.mdf` soubor databáze.

Tato ukázka předpokládáme, že je název instance systému SQL Server 2005 Express Edition `SQLEXPRESS` a se nachází na stejném počítači jako webový server; toto je také výchozí nastavení. Pokud vaše instalace se liší, budete muset přizpůsobit informace o připojení pro databázi.

Chcete aktivovat funkce ASP.NET AJAX a sady nástrojů ovládacího prvku `ScriptManager` řízení musíte umístit kdekoli na stránce (ale uvnitř &lt; `form` &gt; element):

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample1.aspx)]

V dalším kroku jsou potřebné dvě prvky rozevírací seznam. V této ukázce používáme dodavatele a kontaktních informací z AdventureWorks, proto jsme vytvořit jeden seznam pro jsou k dispozici dodavatelé a jeden pro kontakty, k dispozici:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample2.aspx)]

Dva Extender CascadingDropDown pak, je nutné přidat na stránku. Jeden doplní v prvním seznamu (dodavatelé) a jiného výplní seznamu druhý (kontakty). Musí být nastavena následující atributy:

- `ServicePath`: Adresa URL webové služby doručování položky seznamu
- `ServiceMethod`: Webové metody doručování položky seznamu
- `TargetControlID`: ID rozevíracího seznamu
- `Category`: Informace o kategoriích, které je odeslána do webové metody při volání
- `PromptText`: Text zobrazí v případě, že asynchronní načítání seznamu dat ze serveru
- `ParentControlID`: (nepovinný) nadřazené rozevírací seznam této aktivační události načítání aktuálního seznamu.

V závislosti na programovací jazyk použitý změní se název příslušné webové služby, ale všechny ostatní hodnoty atributů jsou stejné. Tady je element CascadingDropDown pro v prvním rozevíracím seznamu:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample3.aspx)]

Ovládací prvek Extender pro druhý seznamu je nutné nastavit `ParentControlID` atributů tak, že vyberete položku v seznamu aktivačních událostí dodavatelé načítání přidružených elementů v seznamu kontaktů.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample4.aspx)]

Ve webové službě, která je nastavena takto se pak provádí samotnou práci. Všimněte si, že `[ScriptService]` je použit atribut, jinak prvku ASP.NET AJAX nemůže vytvořit proxy server JavaScript pro přístup k webové metody z kódu klientský skript.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample5.aspx)]

Podpis metody webové volá CascadingDropDown vypadá takto:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample6.vb)]

Takže návratová hodnota musí být pole typu `CascadingDropDownNameValue` je definované Toolkitu. `GetVendors()` Metoda je poměrně snadno implementovat: kód připojuje k databázi AdventureWorks a dotazy na prvních 25 dodavatele. První parametr ve `CascadingDropDownNameValue` konstruktor popisek položky seznamu, druhý je jeho hodnota (hodnota atributu ve formátu HTML na &lt; `option` &gt; element). Zde je kód:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample7.vb)]

Získávání přidružené kontakty pro dodavatele (název metody: `GetContactsForVendor()`) je trochu trickier. Nejprve je třeba stanovit dodavatele, který byl vybrán v prvním rozevíracím seznamu. Toolkitu definuje Pomocná metoda pro tuto úlohu: `ParseKnownCategoryValuesString()` metoda vrátí `StringDictionary` element s daty rozevíracího seznamu:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample8.vb)]

Z bezpečnostních důvodů musí nejdřív ověřit tato data. Ano, pokud je položka dodavatele (protože `Category` první CascadingDropDown elementu je nastavena na `"Vendor"`), může načíst ID vybraného dodavatele:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample9.vb)]

Zbývající část metody je poměrně jednoduché, potom. ID dodavatele slouží jako parametr pro dotaz SQL, který načte všechny přidružené kontakty pro příslušného dodavatele. Ještě jednou, metoda vrátí pole typu `CascadingDropDownNameValue`.

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample10.vb)]

Načtení stránky ASP.NET a po nějakou dobu seznamu dodavatele vyplněno 25 položky. Vyberte jednu položku a Všimněte si, jak je druhý rozevíracího seznamu vyplněn data.


[![V prvním seznamu se vyplní automaticky](using-cascadingdropdown-with-a-database-vb/_static/image2.png)](using-cascadingdropdown-with-a-database-vb/_static/image1.png)

V prvním seznamu se vyplní automaticky ([Kliknutím zobrazit obrázek v plné velikosti](using-cascadingdropdown-with-a-database-vb/_static/image3.png))


[![Druhý seznamu je vyplněna podle výběru v seznamu první](using-cascadingdropdown-with-a-database-vb/_static/image5.png)](using-cascadingdropdown-with-a-database-vb/_static/image4.png)

Druhý seznamu je vyplněna podle výběru v seznamu první ([Kliknutím zobrazit obrázek v plné velikosti](using-cascadingdropdown-with-a-database-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Předchozí](filling-a-list-using-cascadingdropdown-vb.md)
> [další](presetting-list-entries-with-cascadingdropdown-vb.md)
