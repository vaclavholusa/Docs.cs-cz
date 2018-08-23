---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
title: Přidání ověření do modelu | Dokumentace Microsoftu
author: Rick-Anderson
description: 'Poznámka: Aktualizovanou verzi tohoto kurzu je k dispozici tady, která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, sledovat a ukázka mnohem jednodušší...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 5d9a2999-fcc4-4c45-a018-271fddf74a3b
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 01190b7bffc0258649373f3be0c75c41af80921d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756369"
---
<a name="adding-validation-to-the-model"></a>Přidání ověření do modelu
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Je k dispozici aktualizovaná verze tohoto kurzu [tady](../../getting-started/introduction/getting-started.md) , která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.


V této části přidáte logiku ověřování k `Movie` model a zajistíte, že ověřovacích pravidel se vynucují kdykoli se uživatel pokusí o vytvoření nebo úprava videa pomocí aplikace.

## <a name="keeping-things-dry"></a>Udržování suchého věcí

Jedním ze základních principů návrhu rozhraní ASP.NET MVC je zkušební (&quot;není opakujte sami&quot;). ASP.NET MVC umožňuje zadat funkci nebo chování pouze jednou a potom jste ho všude, kde se projeví v aplikaci. To snižuje množství kódu, které potřebujete k zápisu a provede kód, který napíšete náchylné k chybám a snadněji udržovat méně chyb.

Podpora ověřování poskytované rozhraní ASP.NET MVC a Entity Framework Code First je skvělé příkladem zásada SUCHÝCH v akci. Ověřovací pravidla můžete zadat pomocí deklarace na jednom místě (ve třídě modelu) a pravidel se vynucují kdekoli v aplikaci.

Podívejme se na tom, jak můžete využít této podpory ověřování v aplikace movie.

## <a name="adding-validation-rules-to-the-movie-model"></a>Přidání pravidel ověřování do modelu Movie

Zobrazí za přibližně tak, že přidáte některé logiku ověřování k `Movie` třídy.

Otevřít *Movie.cs* souboru. Přidat `using` příkazu v horní části souboru, který odkazuje [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) obor názvů:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

Všimněte si, že obor názvů neobsahuje `System.Web`. DataAnnotations poskytuje integrovanou sadu atributů ověření, které můžete provést pomocí deklarace tříd nebo vlastností.

Teď aktualizovat `Movie` třídy, které chcete využít výhod integrovaného [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), a [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) atributů ověření . Použijte následující kód s ukázkovým kde použít atributy.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs?highlight=4,10,13,17)]

Spusťte aplikaci a znovu se zobrazí následující chyba běhu:

***Model zálohování kontextu 'MovieDBContext' byl změněn, protože byla vytvořena databáze. Zvažte použití migrace Code First k aktualizaci databáze ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).***

Migraci budeme používat k aktualizaci schématu. Sestavte řešení a pak otevřete **Konzola správce balíčků** okna a zadejte následující příkazy:

[!code-console[Main](adding-validation-to-the-model/samples/sample3.cmd)]

Po dokončení tohoto příkazu, Visual Studio otevře soubor třídy, která definuje nový `DbMIgration` odvozené třídy se zadaným názvem (*AddDataAnnotationsMig*) a `Up` metoda se zobrazí kód, který aktualizuje omezení schématu. `Title` a `Genre` polí už nejsou s možnou hodnotou Null (to znamená, je nutné zadat hodnotu) a `Rating` pole má maximální délku 5.

Ověření atributy určují chování, které chcete vynutit na, které se použijí pro vlastnosti modelu. `Required` Atribut označuje, že vlastnost musí mít hodnotu; v této ukázce, musí mít hodnoty pro video `Title`, `ReleaseDate`, `Genre`, a `Price` vlastnosti, aby byla platná. `Range` Atribut omezuje hodnotu do zadaného rozsahu. `StringLength` Atribut umožňuje nastavit maximální délka vlastnosti typu string a volitelně jeho minimální délka. Vnitřní typy (například `decimal, int, float, DateTime`) se vyžadují ve výchozím nastavení a nemusí `Required` atribut.

Kód nejprve zajistí, že se vynucují ověřovacích pravidel, které jste zadali na třídu modelu, předtím, než se aplikace uloží změny do databáze. Následující kód například vyvolá výjimku při `SaveChanges` metoda je volána, protože některé požadované `Movie` nebyly nalezeny hodnoty vlastností a cena je nula (což je mimo platný rozsah).

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs?highlight=7-8)]

S ověřovací pravidla automaticky vynucuje sada .NET Framework pomáhá vytvářet vaše aplikace odolnější. Také zajistí, že nelze zapomenete něco ověření a neúmyslně nechat chybná data do databáze.

Tady je úplný výpis pro aktualizaci kódu *Movie.cs* souboru:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Chyba ověření uživatelského rozhraní v architektuře ASP.NET MVC

Znovu spusťte aplikaci a přejděte */Movies* adresy URL.

Klikněte na tlačítko **vytvořit nový** odkaz na přidání nového videa. Vyplňte formulář pomocí některé neplatné hodnoty a pak klikněte na tlačítko **vytvořit** tlačítko.

![8_validationErrors](adding-validation-to-the-model/_static/image1.png)

> [!NOTE]
> pro podporu ověřování jQuery pro neanglická národní prostředí, které používají čárku (&quot;,&quot;) desetinné čárky, je třeba zahrnout *globalize.js* a konkrétní *cultures/globalize.cultures.js* souboru (z [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) a JavaScript, který chcete použít `Globalize.parseFloat`. Následující kód ukazuje změny souboru Views\Movies\Edit.cshtml pracovat &quot;fr-FR&quot; jazykové verze:


[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Všimněte si, jak formulář automaticky použila červené ohraničení zvýrazněte textová pole, které obsahují neplatná data a je vyzářeno chybovou zprávu ověření odpovídající vedle každé z nich. Chyby se vynucují straně klienta (pomocí jazyků JavaScript a jQuery) a na straně serveru (v případě má zakázaný JavaScript).

Skutečné výhodou je, že nemusíte změnit jediný řádek kódu v `MoviesController` třídy nebo *Create.cshtml* zobrazení, chcete-li povolit toto ověření uživatelského rozhraní. Kontroler a zobrazení, které jste vytvořili dříve v tomto kurzu automaticky vybere nahoru ověřovací pravidla, které jste zadali pomocí atributů ověření na vlastnosti `Movie` třída modelu.

Možná jste si všimli vlastností `Title` a `Genre`, povinný atribut není vynucena, dokud se odesláním formuláře (přístupů **vytvořit** tlačítko), nebo zadejte text do vstupního pole a odebrat ho. Pro pole který je původně prázdné (například pole v zobrazení pro vytváření) a který má pouze povinný atribut a žádné jiné atributy ověření můžete provést následující aktivovat ověřování:

1. Kartu do pole.
2. Zadejte nějaký text.
3. Karta navýšení kapacity.
4. Karta zpět do pole.
5. Odeberte text.
6. Karta navýšení kapacity.

Výše uvedené pořadí aktivují požadované ověřovací bez klepnutím na tlačítko Odeslat. Jednoduše klepnutím na tlačítko Odeslat bez nutnosti zadávat žádné z polí se aktivuje ověřování na straně klienta. Data formuláře neodešle na server, dokud nedojde k žádným chybám ověření na straně klienta. Můžete ho otestovat uvedením přerušení metody Post protokolu HTTP nebo pomocí [nástroj fiddler](http://fiddler2.com/fiddler2/) nebo aplikace Internet Explorer 9 [vývojářské nástroje F12 pomáhají](https://msdn.microsoft.com/ie/aa740478).

![](adding-validation-to-the-model/_static/image2.png)

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Jak probíhá ověřování vytvořením zobrazit a vytvořit metody akce

Může vás zajímat, jak byl vygenerován ověření uživatelského rozhraní bez jakýchkoli aktualizací kódu v kontroleru nebo zobrazení. Následující výpis ukazuje, co `Create` metody v `MovieController` vypadají třídy. Jsou to neliší od jak jste je vytvořili dříve v tomto kurzu.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs?highlight=12,15)]

První (HTTP GET) `Create` zobrazí úvodní formulář vytvořit metody akce. Druhý (`[HttpPost]`) verze zpracovává odeslaného formuláře. Druhá `Create` – metoda ( `HttpPost` verze) volání `ModelState.IsValid` ke kontrole, jestli má tento film všechny chyby ověření. Voláním této metody vyhodnotí všechny atributy ověření, které se použily k objektu. Pokud objekt má chyby ověření, `Create` metoda znovu zobrazí formulář. Pokud nejsou žádné chyby, metoda tento nový film uloží v databázi. V našem příkladu film používáme **formuláři není odeslána na server, pokud nejsou zjištěny na straně klienta; chyby ověřování druhého** `Create` **nikdy volána metoda**. Pokud zakážete jazyka JavaScript v prohlížeči, je zakázáno ověřování na straně klienta a HTTP POST `Create` volání metody `ModelState.IsValid` ke kontrole, jestli má tento film všechny chyby ověření.

Lze nastavit bod přerušení v `HttpPost Create` metoda a ověřit se nikdy nevolá metodu, ověřování na straně klienta nebude odeslat data formuláře, když se zjistí chyby ověření. Pokud zakážete jazyka JavaScript v prohlížeči a pak odesláním formuláře s chybami, se setkají bodu přerušení. Vy i přesto úplného ověření bez jazyka JavaScript. Následující obrázek ukazuje, jak zakázat jazyka JavaScript v aplikaci Internet Explorer.

![](adding-validation-to-the-model/_static/image3.png)

![](adding-validation-to-the-model/_static/image4.png)

Následující obrázek ukazuje, jak zakázat jazyka JavaScript v prohlížeči FireFox.

![](adding-validation-to-the-model/_static/image5.png)

Následující obrázek ukazuje, jak zakázat JavaScript s prohlížečem Chrome.

![](adding-validation-to-the-model/_static/image6.png)

Níže je uvedená *Create.cshtml* zobrazit šablonu, která automaticky dříve v tomto kurzu. Používá se pomocí metody akce, které jsou uvedené výše obě počáteční formulář pro zobrazení a znovu ho zobrazit v případě chyby.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample8.cshtml?highlight=22-23,30-31,38-39,46-47)]

Všimněte si, jak tento kód použije `Html.EditorFor` pomocné rutiny pro výstup `<input>` – element pro každé `Movie` vlastnost. Vedle tohoto pomocníka je volání `Html.ValidationMessageFor` Pomocná metoda. Tyto dvě metody helper pracovat s modelu objektu, který je předán adaptérem k zobrazení (v tomto případě `Movie` objekt). Zobrazují se automaticky pro ověření atributy určené v modelu a zobrazí chybové zprávy podle potřeby.

Co je hodně příjemné o tento přístup je, že kontroler ani zobrazit šablonu vytvořit cokoli o skutečné ověřovacích pravidel se vynucují nebo zná o určité chybové zprávy zobrazeny. Ověřovací pravidla a chybové řetězce se zadávají pouze v `Movie` třídy. Tyto stejné ověřovací pravidla se automaticky použijí k zobrazení pro úpravy a žádné jiné zobrazení šablony, které vytvoříte, které upravit model.

Pokud chcete později změnit logiku ověřování, můžete k tomu v přesně jedno místo přidávání atributů ověření do modelu (v tomto příkladu `movie` třídy). Není nutné se starat o různé části aplikace, s jakým způsobem se vynucují pravidla nekonzistentní – veškerou logiku ověřování definované na jednom místě, který se použít kdekoli. To zajišťuje velmi čistým kódem a umožňuje snadno vyvíjet a udržovat. A to znamená, že je budete mít plně dodržením zásada SUCHÝCH.

## <a name="adding-formatting-to-the-movie-model"></a>Přidání formátování ke modelu Movie

Otevřít *Movie.cs* souboru a zkoumat `Movie` třídy. [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Obor názvů obsahuje atributy formátování kromě integrovaná sada atributů ověření. Už jsme použili [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) hodnota výčtu datum vydání a na pole price. Následující kód ukazuje `ReleaseDate` a `Price` vlastnosti příslušnou [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atribut.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) Atributy nejsou atributů ověření, používají se k informování zobrazovací modul, jak vykreslení kódu HTML. V příkladu výše `DataType.Date` atribut zobrazuje data film jako pouze data bez času. Například následující [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atributy není ověření formátu dat:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Atributy uvedené výše pouze poskytnout nápovědu pro modul zobrazení pro zobrazení dat (a zadat atributy například &lt;&gt; pro adresy URL a &lt;href =&quot;mailto:EmailAddress.com&quot; &gt; e-mailu. Můžete použít [regulární výraz](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) atribut pro ověření formátu data.

Alternativním přístupem k použití `DataType` atributy, můžete explicitně nastavit [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx) hodnotu. Následující kód ukazuje vlastnost datum vydání verze s řetězec formátu data (konkrétně &quot;d&quot;). Můžete využít k určení, že nechcete čas jako součást datum vydání.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample11.cs)]

Kompletní `Movie` třídy je uveden níže.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample12.cs)]

Spusťte aplikaci a přejděte `Movies` kontroleru. Datum vydání a cena přehledně naformátovaná. Následující obrázek ukazuje datum vydání verze a pomocí cena &quot;fr-FR&quot; jako jazykovou verzi.

![8_format_SM](adding-validation-to-the-model/_static/image7.png)

Následující obrázek ukazuje stejná data, zobrazí se výchozí jazykovou verzi (English US).

![](adding-validation-to-the-model/_static/image8.png)

V další části série, přidáme zkontrolujte, zda aplikace a vylepšování některé automaticky generované `Details` a `Delete` metody.

> [!div class="step-by-step"]
> [Předchozí](adding-a-new-field-to-the-movie-model-and-table.md)
> [další](examining-the-details-and-delete-methods.md)
