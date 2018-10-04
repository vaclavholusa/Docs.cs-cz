---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
title: Přidání ověření do modelu (VB) | Dokumentace Microsoftu
author: Rick-Anderson
description: V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, což je...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 878f6c31-972d-45f4-8849-5c633b511409
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 88f2ebcc98adcb48da36a7ad85df603b550fe4b1
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576363"
---
<a name="adding-validation-to-the-model-vb"></a>Přidání ověření do modelu (VB)
====================
Podle [Rick Anderson]((https://twitter.com/RickAndMSFT))

> V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, což je bezplatná verze sady Microsoft Visual Studio. Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže. Nainstalujte všechny z nich kliknutím na následující odkaz: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:
> 
> - [Visual Studio Web Developer Express SP1 požadavky](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 nástroje Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podpora modulu runtime a nástroje)
> 
> Pokud používáte Visual Studio 2010 namísto Visual Web Developer 2010, nainstalujte příslušné požadované součásti po kliknutí na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt aplikace Visual Web Developer se zdrojovým kódem VB.NET je k dispozici v tomto tématu. [Stáhněte si verzi VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Pokud dáváte přednost C#, přejděte [verze jazyka C#](../cs/adding-validation-to-the-model.md) tohoto kurzu.


V této části přidáte logiku ověřování k `Movie` model a zajistíte, že ověřovacích pravidel se vynucují kdykoli se uživatel pokusí o vytvoření nebo úprava videa pomocí aplikace.

## <a name="keeping-things-dry"></a>Udržování suchého věcí

Jedním ze základních principů návrhu rozhraní ASP.NET MVC je zkušební ("není opakujte sami"). ASP.NET MVC umožňuje zadat funkci nebo chování pouze jednou a potom jste ho všude, kde se projeví v aplikaci. To snižuje množství kódu, které potřebujete k zápisu a provede kód, který napíšete mnohem snazší údržbu.

Podpora ověřování poskytované rozhraní ASP.NET MVC a Entity Framework Code First je skvělé příkladem zásada SUCHÝCH v akci. Ověřovací pravidla můžete zadat pomocí deklarace na jednom místě (ve třídě modelu) a pak se tato pravidla vynucují kdekoli v aplikaci.

Podívejme se na tom, jak můžete využít této podpory ověřování v aplikace movie.

## <a name="adding-validation-rules-to-the-movie-model"></a>Přidání pravidel ověřování do modelu Movie

Zobrazí za přibližně tak, že přidáte některé logiku ověřování k `Movie` třídy.

Otevřít *Movie.vb* souboru. Přidat `Imports` příkazu v horní části souboru, který odkazuje [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) obor názvů:

[!code-vb[Main](adding-validation-to-the-model/samples/sample1.vb)]

Obor názvů je součástí rozhraní .NET Framework. Poskytuje integrovanou sadu atributů ověření, které můžete provést pomocí deklarace tříd nebo vlastností.

Teď aktualizovat `Movie` třídy, které chcete využít výhod integrovaného [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), a [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) atributů ověření . Použijte následující kód s ukázkovým kde použít atributy.

[!code-vb[Main](adding-validation-to-the-model/samples/sample2.vb)]

Ověření atributy určují chování, které chcete vynutit na, které se použijí pro vlastnosti modelu. `Required` Atribut označuje, že vlastnost musí mít hodnotu; v této ukázce, musí mít hodnoty pro video `Title`, `ReleaseDate`, `Genre`, a `Price` vlastnosti, aby byla platná. `Range` Atribut omezuje hodnotu do zadaného rozsahu. `StringLength` Atribut umožňuje nastavit maximální délka vlastnosti typu string a volitelně jeho minimální délka.

Kód nejprve zajistí, že se vynucují ověřovacích pravidel, které jste zadali na třídu modelu, předtím, než se aplikace uloží změny do databáze. Následující kód například vyvolá výjimku při `SaveChanges` metoda je volána, protože některé požadované `Movie` nebyly nalezeny hodnoty vlastností a cena je nula (což je mimo platný rozsah).

[!code-vb[Main](adding-validation-to-the-model/samples/sample3.vb)]

S ověřovací pravidla automaticky vynucuje sada .NET Framework pomáhá vytvářet vaše aplikace odolnější. Také zajistí, že nelze zapomenete něco ověření a neúmyslně nechat chybná data do databáze.

Tady je úplný výpis pro aktualizaci kódu *Movie.vb* souboru:

[!code-vb[Main](adding-validation-to-the-model/samples/sample4.vb)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Chyba ověření uživatelského rozhraní v architektuře ASP.NET MVC

Znovu spusťte aplikaci a přejděte */Movies* adresy URL.

Klikněte na tlačítko **vytvořit filmu** odkaz na přidání nového videa. Vyplňte formulář pomocí některé neplatné hodnoty a pak klikněte na tlačítko **vytvořit** tlačítko.

[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)

Všimněte si, jak má formulář automaticky použít barvu pozadí zvýrazněte textová pole, které obsahují neplatná data a je vyzářeno chybovou zprávu ověření odpovídající vedle každé z nich. Chybové zprávy odpovídat chybové řetězce, které jste zadali, když s poznámkami `Movie` třídy. Chyby se vynucují straně klienta (pomocí JavaScriptu) a na straně serveru (v případě má zakázaný JavaScript).

Skutečné výhodou je, že nemusíte změnit jediný řádek kódu v `MoviesController` třídy nebo *Create.vbhtml* zobrazení, chcete-li povolit toto ověření uživatelského rozhraní. Kontroler a zobrazení, které jste vytvořili dříve v tomto kurzu automaticky vybere nahoru ověřovací pravidla, které jste zadali pomocí atributů na `Movie` třída modelu.

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Jak probíhá ověřování vytvořením zobrazit a vytvořit metody akce

Může vás zajímat, jak byl vygenerován ověření uživatelského rozhraní bez jakýchkoli aktualizací kódu v kontroleru nebo zobrazení. Následující výpis ukazuje, co `Create` metody v `MovieController` vypadají třídy. Jsou to neliší od jak jste je vytvořili dříve v tomto kurzu.

[!code-vb[Main](adding-validation-to-the-model/samples/sample5.vb)]

První metoda akce zobrazí počáteční vytvořit formulář. Druhá zpracovává odeslaného formuláře. Druhá `Create` volání metody `ModelState.IsValid` ke kontrole, jestli má tento film všechny chyby ověření. Voláním této metody vyhodnotí všechny atributy ověření, které se použily k objektu. Pokud objekt má chyby ověření, `Create` metoda znovu zobrazí formulář. Pokud nejsou žádné chyby, metoda tento nový film uloží v databázi.

Níže je uvedená *Create.vbhtml* zobrazit šablonu, která automaticky dříve v tomto kurzu. Používá se pomocí metody akce, které jsou uvedené výše obě počáteční formulář pro zobrazení a znovu ho zobrazit v případě chyby.

[!code-vbhtml[Main](adding-validation-to-the-model/samples/sample6.vbhtml)]

Všimněte si, jak tento kód použije `Html.EditorFor` pomocné rutiny pro výstup `<input>` – element pro každé `Movie` vlastnost. Vedle tohoto pomocníka je volání `Html.ValidationMessageFor` Pomocná metoda. Tyto dvě metody helper pracovat s modelu objektu, který je předán adaptérem k zobrazení (v tomto případě `Movie` objekt). Zobrazují se automaticky pro ověření atributy určené v modelu a zobrazí chybové zprávy podle potřeby.

Co je hodně příjemné o tento přístup je, že kontroler ani zobrazit šablonu vytvořit cokoli o skutečné ověřovacích pravidel se vynucují nebo zná o určité chybové zprávy zobrazeny. Ověřovací pravidla a chybové řetězce se zadávají pouze v `Movie` třídy.

Pokud chcete později změnit logiku ověřování, udělat na přesně jedno místo. Není nutné se starat o různé části aplikace, s jakým způsobem se vynucují pravidla nekonzistentní – veškerou logiku ověřování definované na jednom místě, který se použít kdekoli. To zajišťuje velmi čistým kódem a umožňuje snadno vyvíjet a udržovat. A to znamená, že je budete mít plně dodržením zásada SUCHÝCH.

## <a name="adding-formatting-to-the-movie-model"></a>Přidání formátování ke modelu Movie

Otevřít *Movie.vb* souboru. [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Obor názvů obsahuje atributy formátování kromě integrovaná sada atributů ověření. Které chcete použít [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atribut a [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) hodnota výčtu datum vydání a na pole price. Následující kód ukazuje `ReleaseDate` a `Price` vlastnosti příslušnou [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atribut.

[!code-vb[Main](adding-validation-to-the-model/samples/sample7.vb)]

Alternativně můžete explicitně nastavit [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx) hodnotu. Následující kód ukazuje vlastnost datum vydání verze s řetězec formátu data (konkrétně, "d"). Můžete využít k určení, že nechcete čas jako součást datum vydání.

[!code-vb[Main](adding-validation-to-the-model/samples/sample8.vb)]

Následující kód formáty `Price` vlastnosti jako datový typ Měna.

[!code-vb[Main](adding-validation-to-the-model/samples/sample9.vb)]

Kompletní `Movie` třídy je uveden níže.

[!code-vb[Main](adding-validation-to-the-model/samples/sample10.vb)]

Spusťte aplikaci a přejděte `Movies` kontroleru.

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

V další části série, přidáme zkontrolujte, zda aplikace a vylepšování některé automaticky generované `Details` a `Delete` metody...

> [!div class="step-by-step"]
> [Předchozí](adding-a-new-field.md)
> [další](improving-the-details-and-delete-methods.md)
