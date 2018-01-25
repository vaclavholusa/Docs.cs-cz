---
uid: mvc/overview/getting-started/introduction/adding-validation
title: "Přidání ověřování | Microsoft Docs"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 9f35ca15-e216-4db6-9ebf-24380b0f31b4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-validation
msc.type: authoredcontent
ms.openlocfilehash: b59965b2fab00cb64db06574d5ca3c6388daa7c8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="adding-validation"></a>Přidání ověřování
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

V tomto v této části přidáte logiku ověření pro `Movie` modelu a budete ujistěte, že ověřovací pravidla se uplatňují vždy, když uživatel se pokusí vytvořit nebo upravit film pomocí aplikace.

## <a name="keeping-things-dry"></a>Zachování SUCHÝCH věcí

Jedním ze základních principů návrhu architektury ASP.NET MVC je [SUCHÝCH](http://en.wikipedia.org/wiki/Don't_repeat_yourself) (&quot;nemáte opakujte sami&quot;). ASP.NET MVC umožňuje zadejte funkce nebo chování jenom jednou a potom ho v aplikaci projeví everywhere mít. To snižuje množství kódu, které potřebujete k zápisu a umožňuje kód, který menší chyba náchylné k chybám a jednodušší správu zápisu.

Podpora ověřování poskytované ASP.NET MVC a Entity Framework Code First je skvělým příklad SUCHÝCH zásadu v akci. V jednom místě (ve třídě modelu) můžete určit deklarativně ověřovací pravidla a pravidla jsou vynucována everywhere v aplikaci.

Podíváme, jak můžete využít výhod tato podpora ověřování v aplikaci film.

## <a name="adding-validation-rules-to-the-movie-model"></a>Přidávání pravidel ověřování modelu filmu

Budete začněte tím, že některé logiku ověření pro přidání `Movie` třídy.

Otevřete *Movie.cs* souboru. Upozornění [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) obor názvů neobsahuje `System.Web`. DataAnnotations poskytuje integrovanou sadu atributů ověření, které můžete provést deklarativně všechny třídy nebo vlastnost. (Také obsahuje formátování atributů, například [datový typ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) který usnadní formátování a neposkytují žádné ověření.)

Nyní aktualizovat `Movie` třída využít předdefinované [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), [regulární výraz](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx), a [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) atributů ověření. Nahraďte `Movie` třídy následujícím kódem:

[!code-csharp[Main](adding-validation/samples/sample1.cs?highlight=5,13-15,18-19,22-23)]

[ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) Atribut Nastaví maximální délku řetězce a nastaví toto omezení v databázi, proto se změní schéma databáze. Klikněte pravým tlačítkem na **filmy** tabulky v **Průzkumníka serveru** a klikněte na tlačítko **otevřete definici tabulky**:

![](adding-validation/_static/image1.png)

Na obrázku výše vidíte všech polí s řetězcem jsou nastaveny na [NVARCHAR (maximum)](https://technet.microsoft.com/library/ms186939.aspx). Migrace použijeme k aktualizaci schématu. Sestavte řešení a pak otevřete **Konzola správce balíčků** okna a zadejte následující příkazy:

[!code-console[Main](adding-validation/samples/sample2.cmd)]

Po dokončení tohoto příkazu Visual Studio otevře soubor třídy, který definuje nový `DbMIgration` odvozené třídy se zadaným názvem (`DataAnnotations`) a v `Up` metoda se zobrazí kód, který aktualizuje schématu omezení:

[!code-csharp[Main](adding-validation/samples/sample3.cs)]

`Genre` Pole je již nejsou s možnou hodnotou Null (to znamená, je třeba zadat hodnotu). `Rating` Pole má maximální délku 5 a `Title` má maximální délku 60. Minimální délka 3 na `Title` a rozsah na `Price` nevytvořila změny schématu.

Zkontrolujte schéma film:

![](adding-validation/_static/image2.png)

Pole řetězce zobrazit nové omezení délky a `Genre` je již zaškrtnuté jako s možnou hodnotou Null.

Atributy ověření zadejte chování, které chcete vynutit ve vlastnostech modelu, které se použijí k. `Required` a `MinimumLength` atributy znamená, že vlastnost musí mít hodnotu; ale nic zabrání zadat mezer splňovat toto ověření uživatele. [Regulární výraz](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) atribut se používá k omezení znaků, může být vstup. Ve výše, kódu `Genre` a `Rating` musí používat pouze písmena (bílé místo, číslice a speciální znaky nejsou povoleny). [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) Atribut omezuje hodnota v zadaném rozsahu. `StringLength` Atribut umožňuje nastavit maximální délka ve vlastnosti string a volitelně jeho minimální délka. Typů hodnot (například `decimal, int, float, DateTime`) jsou ze své podstaty potřeba a nepotřebujete `Required` atribut.

Kód nejprve zajistí, že ověřovací pravidla, které jste zadali na třídu modelu, jsou vyžadována před aplikace uloží změny v databázi. Například následující kód vyvolá výjimku [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception(v=vs.103).aspx) výjimka při `SaveChanges` metoda je volána, protože některé požadované `Movie` chybí hodnoty vlastností:

[!code-csharp[Main](adding-validation/samples/sample4.cs)]

Výše uvedený kód vyvolá následující výjimky:

*Ověření se nezdařilo pro jednu nebo více entit. Naleznete ve vlastnosti 'EntityValidationErrors' Další podrobnosti.*

S ověřovacích pravidel automaticky vynucováno rozhraní .NET Framework pomáhá zkontrolujte aplikace robustnější. Také zajistí, že nelze zapomenete a ověřit něco nechtěně umožní chybná data do databáze.

## <a name="validation-error-ui-in-aspnet-mvc"></a>Chyba ověření uživatelského rozhraní v architektuře ASP.NET MVC

Spusťte aplikaci a přejděte do */Movies* adresy URL.

Klikněte **vytvořit nový** odkaz na přidání nové videa. Vyplňte formulář s některé neplatné hodnoty. Jakmile jQuery ověřování na straně klienta zjistí chybu, zobrazí chybovou zprávu.

![8_validationErrors](adding-validation/_static/image3.png)

> [!NOTE]
> pro podporu ověřování jQuery pro neanglická národní prostředí, které používají čárkou (",") desetinné čárky, je třeba zahrnout NuGet globalize popsané dříve v tomto kurzu.


Všimněte si, jak formulář automaticky použila barvu červené ohraničení zvýrazněte textová pole, které obsahují neplatná data a má vygenerované chybovou zprávu ověření příslušné vedle každé z nich. Chyby se vynucují (pomocí jazyka JavaScript a jQuery) klientské a serverové (v případě, že uživatel, který má JavaScript zakázaná).

Skutečné výhodou je, že nebyla budete muset změnit jeden řádek kódu `MoviesController` třídy nebo v *Create.cshtml* zobrazení, aby bylo možné povolit toto ověření uživatelského rozhraní. Řadič a zobrazení, které jste vytvořili dříve v tomto kurzu automaticky zachyceny až ověření pravidla, kterou jste zadali pomocí atributů ověření ve vlastnostech `Movie` třída modelu. Test ověření pomocí `Edit` metody akce a stejné ověření platí.

Data formuláře není odesílat na server, dokud nejsou žádné chyby ověření straně klienta. Můžete to ověřit umístěním přerušení v metodě HTTP Post, pomocí [nástroj fiddler](http://fiddler2.com/fiddler2/), nebo aplikaci Internet Explorer [nástroje pro vývojáře F12](https://msdn.microsoft.com/ie/aa740478).

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Dojde k ověření v vytvořením zobrazování a vytváření metody akce

Může vás zajímat, jak byl vygenerován ověření uživatelské rozhraní bez jakýchkoli aktualizací kód v kontroleru nebo zobrazení. Další výpis znázorňuje, co `Create` metody v `MovieController` třída vypadají. Jsou stejné jako u jak je vytvořen v tomto kurzu.

[!code-csharp[Main](adding-validation/samples/sample5.cs)]

První (HTTP GET) `Create` metoda akce zobrazí počáteční vytvořit formulář. Druhý (`[HttpPost]`) verze zpracovává post formuláře. Druhý `Create` – metoda ( `HttpPost` verze) volání `ModelState.IsValid` zkontrolujte, zda video má všechny chyby ověření. Voláním této metody vyhodnotí všechny atributy ověřování, které byly použity k objektu. Pokud objekt má chyby ověření, `Create` metoda znovu zobrazí formulář. Pokud nejsou žádné chyby, metoda nové film uloží v databázi. V našem příkladu film **formuláře není odeslána na server, pokud nejsou chyby ověření na straně klienta; zjištěna druhá** `Create` **metoda je volána nikdy**. Pokud zakážete JavaScript v prohlížeči, ověření klienta je zakázané a HTTP POST `Create` volání metod `ModelState.IsValid` zkontrolujte, zda video má všechny chyby ověření.

Můžete nastavit bod přerušení v `HttpPost Create` metoda a ověřte nikdy volání metody, ověřování na straně klienta nebude odesílat data formuláře, pokud jsou zjištěny chyby ověřování. Pokud zakázat JavaScript v prohlížeči a pak odeslání formuláře s chybami, se setkají místo přerušení. Se stále objevuje úplného ověření bez JavaScript. Následující obrázek ukazuje, jak zakázat JavaScript v aplikaci Internet Explorer.

![](adding-validation/_static/image5.png)

![](adding-validation/_static/image6.png)

Následující obrázek ukazuje, jak zakázat JavaScript v prohlížeči FireFox.

![](adding-validation/_static/image7.png)

Následující obrázek ukazuje, jak zakázat JavaScript v prohlížeči Chrome.

![](adding-validation/_static/image8.png)

Níže je *Create.cshtml* zobrazit šablonu, která vygeneroval dříve v tomto kurzu. Použije se uvedené výše obě metody akce k zobrazení původního formuláře a znovu ji zobrazit v případě chyby.

[!code-cshtml[Main](adding-validation/samples/sample6.cshtml?highlight=16-17)]

Všimněte si, jak kód používá `Html.EditorFor` pomocná rutina pro výstup `<input>` element pro každou `Movie` vlastnost. U tohoto pomocníka je volání `Html.ValidationMessageFor` metodu helper. Tyto dvě metody helper pracovat objekt modelu, který je předán zobrazení řadičem (v tomto případě `Movie` objekt). Budou automaticky hledat atributů ověření zadané v modelu a zobrazí chybové zprávy podle potřeby.

Co je skutečně dobrý o tento přístup je, že ani řadičem ani `Create` zobrazit šablonu zná nic o skutečné ověřovacích pravidel vynucován nebo o specifické chybové zprávy zobrazují. Ověřovací pravidla a řetězce chyby se zadávají pouze v `Movie` třídy. Tyto stejné ověřovací pravidla budou automaticky použita pro `Edit` zobrazení a jakékoli další zobrazení šablony můžete vytvořit které upravit modelu.

Pokud chcete později změnit logiku ověření, můžete tak učinit na jednom místě přidáním atributů ověření modelu (v tomto příkladu `movie` třídy). Nebudete mít starat o různých částech aplikace je nekonzistentní s jak se vynucují pravidla – veškerou logiku ověřování definované na jednom místě se použije everywhere. To zajišťuje kód velmi vyčištění a umožňuje snadno spravovat a momentální. . To znamená, že je budete mít plně ctít zásady *SUCHÝCH* zásadu.

## <a name="using-datatype-attributes"></a>Pomocí atributů datový typ

Otevřete *Movie.cs* soubor a zkontrolujte `Movie` třídy. [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Obor názvů poskytuje atributy formátování kromě integrovanou sadu atributů ověření. Provedli jsme již [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) Výčtová hodnota, datum vydání a pole cena. Následující kód ukazuje `ReleaseDate` a `Price` vlastnosti s příslušnou [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atribut.

[!code-csharp[Main](adding-validation/samples/sample7.cs)]

[Datový typ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributy pouze poskytovat pro modul zobrazení pro zobrazení dat (a zadejte atributy, jako `<a>` pro adresy URL a `<a href="mailto:EmailAddress.com">` e-mailu. Můžete použít [regulární výraz](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) atribut pro ověření formátu dat. [Datový typ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atribut slouží k určení datový typ, který je specifičtější než vnitřní typ databáze, jsou ***není*** atributů ověření. V tomto případě chceme jenom udržování přehledu o datum, není datum a čas. [Datový typ výčtu](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) poskytuje pro mnoho typů dat, jako například *datum, čas, telefonní číslo, měny, EmailAddress* a další. `DataType` Atributu můžete také povolit aplikace automaticky poskytnout konkrétní typ funkce. Například `mailto:` může vytvořit odkaz pro [DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx), a datum selektor lze zadat pro [DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) v prohlížečích podporujících [HTML5](http://html5.org/). [Datový typ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributy vysílá standardu HTML 5 [data -](http://ejohn.org/blog/html-5-data-attributes/) (vyslovováno *data dash*) atributy, které můžete porozumět standardu HTML 5 prohlížeče. [Datový typ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributy neposkytují žádné ověření.

`DataType.Date`neurčuje formát data, které se zobrazí. Ve výchozím nastavení, zobrazí se pole dat podle výchozích formátů podle serveru[CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx).

`DisplayFormat` Atribut slouží k explicitnímu zadání formát data:


[!code-csharp[Main](adding-validation/samples/sample8.cs)]


`ApplyFormatInEditMode` Nastavení určuje, že zadané formátování také bude použito při hodnota je zobrazena v textovém poli pro úpravy. (Který není vhodné pro některá pole – například pro hodnoty měny, nemusí chcete symbolu měny do textového pole pro úpravy.)

Můžete použít [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atribut podle sám sebe, ale obecně je vhodné používat [datový typ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) také atribut. `DataType` Přenese tak atribut *sémantiku* dat, jako byl proti na tom, jak vykreslit ho na obrazovce a nabízí následující výhody, které není dostupná s `DisplayFormat`:

- V prohlížeči můžete povolit funkce HTML5 (např. k zobrazení ovládacího prvku kalendář, symbolu měny vhodné národního prostředí, e-mailu odkazy, atd.).
- Ve výchozím nastavení, prohlížeč bude vykreslovat data správný formát na základě vaší[národního prostředí](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- [Datový typ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributu můžete povolit MVC na výběr šablony pravé pole k poskytnutí dat ( [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) pokud používá samotné používá šablonu řetězec). Další informace najdete v tématu Brad Wilson[ASP.NET MVC 2 šablony](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (I když napsané pro MVC 2, tento článek stále platí pro aktuální verzi ASP.NET MVC.)

Pokud použijete `DataType` atribut s poli data, budete muset určit `DisplayFormat` atribut také, aby se zajistilo, že pole správně vykreslení v prohlížeči Chrome. Další informace najdete v tématu [tohoto podprocesu StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

> [!NOTE]
> k ověřování jQuery nefunguje s[rozsah](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) atribut a[data a času](https://msdn.microsoft.com/library/system.datetime.aspx). Například následující kód bude vždy zobrazovat chyby ověření straně klienta, i v případě, že datum je v zadaném rozsahu:
> 
> [!code-csharp[Main](adding-validation/samples/sample9.cs)]
> 
> Budete muset zakázat ověřování jQuery datum používat [rozsah](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) atribut s[data a času](https://msdn.microsoft.com/library/system.datetime.aspx). Je obecně není vhodné zkompilovat pevného data v modely, takže pomocí[rozsah](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) atribut a[data a času](https://msdn.microsoft.com/library/system.datetime.aspx) se nedoporučuje.


Následující kód ukazuje kombinování atributy na jeden řádek:

[!code-csharp[Main](adding-validation/samples/sample10.cs?highlight=6,10)]

V další části řady, jsme budete zkontrolujte, zda aplikace a některá vylepšení pro automaticky generované `Details` a `Delete` metody.

>[!div class="step-by-step"]
[Předchozí](adding-a-new-field.md)
[další](examining-the-details-and-delete-methods.md)
