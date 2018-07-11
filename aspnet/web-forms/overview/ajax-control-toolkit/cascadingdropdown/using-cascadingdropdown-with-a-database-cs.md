---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
title: Použití ovládacího prvku CascadingDropDown s databází (C#) | Dokumentace Microsoftu
author: wenz
description: Sada nástrojů AJAX Control Toolkit ovládacího prvku CascadingDropDown rozšiřuje ovládací prvek DropDownList tak, aby se změny v jedné DropDownList zatížení související hodnoty v anoth...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 684f0c28-a490-4e5b-b5e5-5dfb77464b49
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 9930319d2f9443ef3b50a87c7dd3d42b879168c9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818745"
---
<a name="using-cascadingdropdown-with-a-database-c"></a>Použití ovládacího prvku CascadingDropDown s databází (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.cs.zip) nebo [stahovat PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1CS.pdf)

> Sada nástrojů AJAX Control Toolkit ovládacího prvku CascadingDropDown rozšiřuje ovládací prvek DropDownList tak, aby se změny v jedné DropDownList zatížení související hodnoty v jiném DropDownList. V pořadí, aby to fungovalo musí být vytvořeny speciální webové služby.


## <a name="overview"></a>Přehled

Sada nástrojů AJAX Control Toolkit ovládacího prvku CascadingDropDown rozšiřuje ovládací prvek DropDownList tak, aby se změny v jedné DropDownList zatížení související hodnoty v jiném DropDownList. (Například jeden seznam obsahuje seznam nám stavy a dalším seznamu je pak vyplněna hlavních měst v tomto stavu.) V pořadí, aby to fungovalo musí být vytvořeny speciální webové služby.

## <a name="steps"></a>Kroky

Za prvé zdroj dat je povinný. Tato ukázka používá databázi AdventureWorks a Microsoft SQL Server 2005 Express Edition. Databáze je volitelná součást instalace sady Visual Studio (včetně express edition) a jsou také dostupné jako samostatný soubor ke stažení v rámci [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Databáze AdventureWorks je součástí sad SQL Server 2005 ukázky a Sample Databases (stáhnout na [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Nejjednodušší způsob, jak nastavit databázi je použít Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) a připojit `AdventureWorks.mdf` databázový soubor.

V tomto příkladu předpokládáme, že název instance systému SQL Server 2005 Express Edition `SQLEXPRESS` a je umístěn ve stejném počítači jako webový server; to je taky výchozí nastavení. Pokud vaše nastavení se liší, je nutné upravit informace o připojení pro databázi.

K aktivaci funkce technologie ASP.NET AJAX a Control Toolkit `ScriptManager` ovládací prvek je třeba umístit kdekoli na stránce (ale v rámci &lt; `form` &gt; element):

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample1.aspx)]

V dalším kroku jsou povinné dvou ovládacích prvků DropDownList. V této ukázce používáme dodavatele a kontaktní údaje z AdventureWorks, proto vytvoříme jeden seznam dostupných dodavatelů a jeden pro dostupných kontaktů:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample2.aspx)]

Pak dvě zařízení Extender CascadingDropDown musí přidat na stránku. Jeden vyplní seznamu první (dodavatelé) a druhý vyplní druhém seznamu (kontakty). Musí být nastaveny následující atributy:

- `ServicePath`: Adresa URL webová služba doručování položky seznamu
- `ServiceMethod`: Metoda webové zajištění položky seznamu
- `TargetControlID`: ID z rozevíracího seznamu
- `Category`: Informace o kategoriích, které je odeslána do webové metody při volání
- `PromptText`: Text zobrazovaný v případě asynchronní načítání seznamu data ze serveru
- `ParentControlID`: (nepovinný) nadřazené rozevírací seznam této aktivační události načítání aktuálního seznamu.

V závislosti na programovací jazyk se používá se změní název příslušné webové služby, ale všechny ostatní hodnoty atributů jsou stejné. Tady je prvku CascadingDropDown pro první rozevírací seznam:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample3.aspx)]

Ovládací prvek extenderů pro druhý seznam potřeba nastavit `ParentControlID` atribut tak, že vyberete položku v seznamu triggerů dodavatelů načítání přidružených elementů v seznamu kontaktů.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample4.aspx)]

Ve webové službě, která se nastavuje takto se pak provádí samotnou práci. Všimněte si, `[ScriptService]` atribut se používá, jinak technologie ASP.NET AJAX nelze vytvořit proxy server JavaScript pro přístup k webové metody v kódu skriptu na straně klienta.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample5.aspx)]

Podpis metody webové volány CascadingDropDown vypadá takto:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample6.cs)]

Takže vrácená hodnota musí být pole typu `CascadingDropDownNameValue` je definován Control Toolkit. `GetVendors()` Metoda je poměrně snadno implementovat: kód se připojí k databázi AdventureWorks a dotazy prvních 25 dodavatelů. První parametr v `CascadingDropDownNameValue` konstruktor titulek položky seznamu, je druhý řádek je jeho hodnota (hodnotu atributu v HTML &lt; `option` &gt; element). Zde je kód:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample7.cs)]

Získávání přidružené kontakty pro dodavatele (název metody: `GetContactsForVendor()`) je o něco trickier. Za prvé musí být určena dodavatele, který byl vybrán v prvním rozevíracím seznamu. Definuje pomocnou metodu pro tuto úlohu Control Toolkit: `ParseKnownCategoryValuesString()` metoda vrátí hodnotu `StringDictionary` element s daty rozevíracího seznamu:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample8.cs)]

Z bezpečnostních důvodů se musí nejdřív ověřit tato data. Ano, pokud je položka dodavatele (protože `Category` prvního prvku CascadingDropDown je nastavena na `"Vendor"`), ID vybraného dodavatele může načíst:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample9.cs)]

Zbývající část metody je poměrně přímočaré, potom. ID dodavatele se používá jako parametr dotazu SQL, který načte všechny přidružené kontakty pro příslušného dodavatele. Ještě jednou, metoda vrátí pole typu `CascadingDropDownNameValue`.

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample10.cs)]

Načtení stránky technologie ASP.NET a po nějakou dobu seznamu dodavatele vyplněno 25 položky. Vyberte jednu položku a Všimněte si, jak je druhý rozevírací seznam naplněný daty.


[![První seznam se vyplní automaticky](using-cascadingdropdown-with-a-database-cs/_static/image2.png)](using-cascadingdropdown-with-a-database-cs/_static/image1.png)

První seznam se vyplní automaticky ([kliknutím ji zobrazíte obrázek v plné velikosti](using-cascadingdropdown-with-a-database-cs/_static/image3.png))


[![Druhý seznam se vyplní podle výběru v prvním seznamu](using-cascadingdropdown-with-a-database-cs/_static/image5.png)](using-cascadingdropdown-with-a-database-cs/_static/image4.png)

Druhý seznam se vyplní podle výběru v prvním seznamu ([kliknutím ji zobrazíte obrázek v plné velikosti](using-cascadingdropdown-with-a-database-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Předchozí](filling-a-list-using-cascadingdropdown-cs.md)
> [další](presetting-list-entries-with-cascadingdropdown-cs.md)
