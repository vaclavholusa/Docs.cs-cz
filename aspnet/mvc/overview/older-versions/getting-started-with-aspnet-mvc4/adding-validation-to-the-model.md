---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
title: "Přidání ověřování do modelu | Microsoft Docs"
author: Rick-Anderson
description: "Poznámka: Aktualizovanou verzi tohoto kurzu je k dispozici, která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, mnohem jednodušší a postupujte podle ukázku..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 5d9a2999-fcc4-4c45-a018-271fddf74a3b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 6de7d279677c7bbf220b956767a97aaaff8da9a1
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/12/2018
---
<a name="adding-validation-to-the-model"></a>Přidání ověřování do modelu
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Je k dispozici aktualizovaná verze tohoto kurzu [sem](../../getting-started/introduction/getting-started.md) používající ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.


V této části přidáte logiku ověření pro `Movie` modelu a budete ujistěte, že ověřovací pravidla se uplatňují vždy, když uživatel se pokusí vytvořit nebo upravit film pomocí aplikace.

## <a name="keeping-things-dry"></a>Zachování SUCHÝCH věcí

Jeden ze základních principů návrhu architektury ASP.NET MVC je suchého (&quot;nemáte opakujte sami&quot;). ASP.NET MVC umožňuje zadejte funkce nebo chování jenom jednou a potom ho v aplikaci projeví everywhere mít. To snižuje množství kódu, které potřebujete k zápisu a umožňuje kód, který menší chyba náchylné k chybám a jednodušší správu zápisu.

Podpora ověřování poskytované ASP.NET MVC a Entity Framework Code First je skvělým příklad SUCHÝCH zásadu v akci. V jednom místě (ve třídě modelu) můžete určit deklarativně ověřovací pravidla a pravidla jsou vynucována everywhere v aplikaci.

Podíváme, jak můžete využít výhod tato podpora ověřování v aplikaci film.

## <a name="adding-validation-rules-to-the-movie-model"></a>Přidávání pravidel ověřování modelu filmu

Budete začněte tím, že některé logiku ověření pro přidání `Movie` třídy.

Otevřete *Movie.cs* souboru. Přidat `using` příkaz v horní části souboru, který odkazuje [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) obor názvů:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

Všimněte si, obor názvů neobsahuje `System.Web`. DataAnnotations poskytuje integrovanou sadu atributů ověření, které můžete provést deklarativně všechny třídy nebo vlastnost.

Nyní aktualizovat `Movie` třída využít předdefinované [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), a [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) atributů ověření . Použijte následující kód jako příklad umístění pro použití atributy.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs?highlight=4,10,13,17)]

Spusťte aplikaci a zobrazí se znovu k následující chybě čas spuštění:

***Model zálohování kontext 'MovieDBContext' se od vytvoření databáze změnil. Zvažte použití migrace Code First k aktualizaci databáze ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).***

Migrace použijeme k aktualizaci schématu. Sestavte řešení a pak otevřete **Konzola správce balíčků** okna a zadejte následující příkazy:

[!code-console[Main](adding-validation-to-the-model/samples/sample3.cmd)]

Po dokončení tohoto příkazu Visual Studio otevře soubor třídy, který definuje nový `DbMIgration` odvozené třídy se zadaným názvem (*AddDataAnnotationsMig*) a v `Up` metoda vidíte kód, který aktualizuje omezení schématu. `Title` a `Genre` pole jsou již s možnou hodnotou Null (to znamená, je třeba zadat hodnotu) a `Rating` pole má maximální délku 5.

Atributy ověření zadejte chování, které chcete vynutit ve vlastnostech modelu, které se použijí k. `Required` Atribut uvádí, že vlastnost musí mít hodnotu; v této ukázce, musí mít hodnoty pro film `Title`, `ReleaseDate`, `Genre`, a `Price` vlastnosti, aby byla platná. `Range` Atribut omezuje hodnota v zadaném rozsahu. `StringLength` Atribut umožňuje nastavit maximální délka ve vlastnosti string a volitelně jeho minimální délka. Vnitřní typy (například `decimal, int, float, DateTime`) se vyžadují ve výchozím nastavení a nepotřebujete `Required` atribut.

Kód nejprve zajistí, že ověřovací pravidla, které jste zadali na třídu modelu, jsou vyžadována před aplikace uloží změny v databázi. Například následující kód vyvolá výjimku při `SaveChanges` metoda je volána, protože některé požadované `Movie` chybí hodnoty vlastností a je nulová (což je mimo platný rozsah).

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs?highlight=7-8)]

S ověřovacích pravidel automaticky vynucováno rozhraní .NET Framework pomáhá zkontrolujte aplikace robustnější. Také zajistí, že nelze zapomenete a ověřit něco nechtěně umožní chybná data do databáze.

Tady je úplný výpis pro aktualizaci kódu *Movie.cs* souboru:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Chyba ověření uživatelského rozhraní v architektuře ASP.NET MVC

Znovu spusťte aplikaci a přejděte do */Movies* adresy URL.

Klikněte **vytvořit nový** odkaz na přidání nové videa. Vyplňte formulář s některé neplatné hodnoty a klikněte **vytvořit** tlačítko.

![8_validationErrors](adding-validation-to-the-model/_static/image1.png)

> [!NOTE]
> pro podporu ověřování jQuery pro neanglická národní prostředí, které používají čárkou (&quot;,&quot;) desetinné čárky, je třeba zahrnout *globalize.js* a konkrétní *cultures/globalize.cultures.js* souboru (z [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) a JavaScript používat `Globalize.parseFloat`. Následující kód ukazuje změny souboru Views\Movies\Edit.cshtml pro práci s &quot;fr-FR&quot; jazyková verze:


[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Všimněte si, jak formulář automaticky použila barvu červené ohraničení zvýrazněte textová pole, které obsahují neplatná data a má vygenerované chybovou zprávu ověření příslušné vedle každé z nich. Chyby se vynucují (pomocí jazyka JavaScript a jQuery) klientské a serverové (v případě, že uživatel, který má JavaScript zakázaná).

Skutečné výhodou je, že nebyla budete muset změnit jeden řádek kódu `MoviesController` třídy nebo v *Create.cshtml* zobrazení, aby bylo možné povolit toto ověření uživatelského rozhraní. Řadič a zobrazení, které jste vytvořili dříve v tomto kurzu automaticky zachyceny až ověření pravidla, kterou jste zadali pomocí atributů ověření ve vlastnostech `Movie` třída modelu.

Možná jste si všimli vlastností `Title` a `Genre`, požadovaný atribut nevynucuje, dokud odesláním formuláře (stiskněte tlačítko **vytvořit** tlačítko), nebo zadejte text do vstupní pole a odebrat ji. V poli což je původně prázdné (například pole pro vytvoření zobrazení) a který má požadovaný atribut a žádné jiné atributy ověření můžete provést následující příkaz a spustit ověření:

1. Klikněte do pole.
2. Zadejte nějaký text.
3. Klikněte na.
4. Kartě zpět do pole.
5. Odeberte text.
6. Klikněte na.

Výše uvedené pořadí aktivují požadované ověření bez stiskne tlačítko pro odeslání. Jednoduše stiskne tlačítko pro odeslání bez nutnosti zadávat žádná pole aktivují ověřování na straně klienta. Data formuláře není odesílat na server, dokud nejsou žádné chyby ověření straně klienta. Toto můžete otestovat uvedení přerušení v metodu Post protokolu HTTP nebo pomocí [nástroj fiddler](http://fiddler2.com/fiddler2/) nebo aplikace Internet Explorer 9 [nástroje pro vývojáře F12](https://msdn.microsoft.com/ie/aa740478).

![](adding-validation-to-the-model/_static/image2.png)

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Dojde k ověření v vytvořením zobrazování a vytváření metody akce

Může vás zajímat, jak byl vygenerován ověření uživatelské rozhraní bez jakýchkoli aktualizací kód v kontroleru nebo zobrazení. Další výpis znázorňuje, co `Create` metody v `MovieController` třída vypadají. Jsou stejné jako u jak je vytvořen v tomto kurzu.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs?highlight=12,15)]

První (HTTP GET) `Create` metoda akce zobrazí počáteční vytvořit formulář. Druhý (`[HttpPost]`) verze zpracovává post formuláře. Druhý `Create` – metoda ( `HttpPost` verze) volání `ModelState.IsValid` zkontrolujte, zda video má všechny chyby ověření. Voláním této metody vyhodnotí všechny atributy ověřování, které byly použity k objektu. Pokud objekt má chyby ověření, `Create` metoda znovu zobrazí formulář. Pokud nejsou žádné chyby, metoda nové film uloží v databázi. V našem příkladu film používáme **formuláře není odeslána na server, pokud nejsou chyby ověření na straně klienta; zjištěna druhá** `Create` **metoda je volána nikdy**. Pokud zakážete JavaScript v prohlížeči, ověření klienta je zakázané a HTTP POST `Create` volání metod `ModelState.IsValid` zkontrolujte, zda video má všechny chyby ověření.

Můžete nastavit bod přerušení v `HttpPost Create` metoda a ověřte nikdy volání metody, ověřování na straně klienta nebude odesílat data formuláře, pokud jsou zjištěny chyby ověřování. Pokud zakázat JavaScript v prohlížeči a pak odeslání formuláře s chybami, se setkají místo přerušení. Se stále objevuje úplného ověření bez JavaScript. Následující obrázek ukazuje, jak zakázat JavaScript v aplikaci Internet Explorer.

![](adding-validation-to-the-model/_static/image3.png)

![](adding-validation-to-the-model/_static/image4.png)

Následující obrázek ukazuje, jak zakázat JavaScript v prohlížeči FireFox.

![](adding-validation-to-the-model/_static/image5.png)

Následující obrázek ukazuje, jak zakázat jazyka JavaScript pomocí prohlížeče Chrome.

![](adding-validation-to-the-model/_static/image6.png)

Níže je *Create.cshtml* zobrazit šablonu, která vygeneroval dříve v tomto kurzu. Použije se uvedené výše obě metody akce k zobrazení původního formuláře a znovu ji zobrazit v případě chyby.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample8.cshtml?highlight=22-23,30-31,38-39,46-47)]

Všimněte si, jak kód používá `Html.EditorFor` pomocná rutina pro výstup `<input>` element pro každou `Movie` vlastnost. U tohoto pomocníka je volání `Html.ValidationMessageFor` metodu helper. Tyto dvě metody helper pracovat objekt modelu, který je předán zobrazení řadičem (v tomto případě `Movie` objekt). Budou automaticky hledat atributů ověření zadané v modelu a zobrazí chybové zprávy podle potřeby.

Co je skutečně dobrý o tento přístup je, že řadičem ani vytvořit šablony zobrazení nic o skutečné ověřovacích pravidel vynucován nebo zná o specifické chybové zprávy zobrazují. Ověřovací pravidla a řetězce chyby se zadávají pouze v `Movie` třídy. Tyto stejné ověřovací pravidla budou automaticky použita k zobrazení upravit a jakékoli další zobrazení šablony, které můžete vytvořit které upravit modelu.

Pokud chcete později změnit logiku ověření, můžete tak učinit na jednom místě přidáním atributů ověření modelu (v tomto příkladu `movie` třídy). Nebudete mít starat o různých částech aplikace je nekonzistentní s jak se vynucují pravidla – veškerou logiku ověřování definované na jednom místě se použije everywhere. To zajišťuje kód velmi vyčištění a umožňuje snadno spravovat a momentální. . To znamená, že je budete mít plně ctít zásady SUCHÝCH Princip.

## <a name="adding-formatting-to-the-movie-model"></a>Přidání formátování ke film modelu

Otevřete *Movie.cs* soubor a zkontrolujte `Movie` třídy. [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Obor názvů poskytuje atributy formátování kromě integrovanou sadu atributů ověření. Provedli jsme již [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) Výčtová hodnota, datum vydání a pole cena. Následující kód ukazuje `ReleaseDate` a `Price` vlastnosti s příslušnou [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atribut.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) Atributy nejsou atributů ověření, používají se k informování zobrazovací modul, jak k vykreslení kódu HTML. V příkladu nahoře `DataType.Date` atribut zobrazí data film jako kalendářní data pouze bez čas. Například následující [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atributy nemáte ověření formátu dat:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Atributy uvedené výše pouze poskytovat pro modul zobrazení pro zobrazení dat (a zadejte atributy, jako &lt;&gt; pro adresy URL a &lt;href =&quot;mailto:EmailAddress.com&quot; &gt; e-mailu. Můžete použít [regulární výraz](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) atribut pro ověření formátu dat.

Alternativní způsob použití `DataType` atributy, můžete explicitně nastavit [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx) hodnotu. Následující kód ukazuje vlastnost datum vydání s řetězec formátu datum (konkrétně, &quot;d&quot;). To byste použili k určení, že nechcete, aby čas jako součást datum vydání.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample11.cs)]

Kompletní `Movie` třídy jsou uvedeny níže.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample12.cs)]

Spusťte aplikaci a přejděte do `Movies` řadiče. Datum vydání a ceny jsou vhodně formátovaná. Následující obrázek ukazuje datum vydání a cena pomocí &quot;fr-FR&quot; jako jazykovou verzi.

![8_format_SM](adding-validation-to-the-model/_static/image7.png)

Následující obrázek ukazuje stejná data, zobrazí se výchozí jazykovou verzi (anglické US).

![](adding-validation-to-the-model/_static/image8.png)

V další části řady, jsme budete zkontrolujte, zda aplikace a některá vylepšení pro automaticky generované `Details` a `Delete` metody.

>[!div class="step-by-step"]
[Předchozí](adding-a-new-field-to-the-movie-model-and-table.md)
[další](examining-the-details-and-delete-methods.md)
