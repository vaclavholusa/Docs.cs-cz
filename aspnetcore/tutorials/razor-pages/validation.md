---
title: "Přidání ověřování"
author: rick-anderson
description: "Vysvětluje postup přidání ověření do stránky Razor."
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/validation
ms.openlocfilehash: 3632b40abb4a3c2343a17a9f3e08bd28fdcf7174
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="adding-validation-to-a-razor-page"></a>Přidání ověřování na stránku Razor

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

V této části ověření logiku přidán `Movie` modelu. Pravidla ověřování se vynucují vždy, když uživatel vytvoří nebo upraví film.

## <a name="validation"></a>Ověřování

Klíčovým principem vývoj softwaru nazývá [SUCHÝCH](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**neměnit **R**epeat **Y**ourself"). Stránky Razor podporuje vývoj funkce je zadán jednou, kde se projeví v celé aplikaci. SUCHÉHO může pomoci snížit objem kódu v aplikaci. SUCHÉHO díky kód náchylné k chybám a usnadňuje testování a udržovat méně chyby.

Podpora ověřování poskytované stránky Razor a Entity Framework je dobrým příkladem SUCHÝCH zásadu. Ověřovací pravidla jsou deklarativně určené v jednom místě (ve třídě modelu) a pravidla jsou vynucována everywhere v aplikaci.

### <a name="adding-validation-rules-to-the-movie-model"></a>Přidávání pravidel ověřování modelu filmu

Otevřete *Movie.cs* souboru. [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) poskytuje integrovanou sadu atributů ověření, které se použijí deklarativně pro třídu nebo vlastnost. DataAnnotations také obsahuje formátování atributů, například `DataType` který usnadní formátování a neposkytují ověření.

Aktualizace `Movie` třída využívat výhod `Required`, `StringLength`, `RegularExpression`, a `Range` atributů ověření.

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]

Atributy ověření Určete chování, která je vynucená na vlastnosti modelu:

* `Required` a `MinimumLength` atributy znamená, že vlastnost musí mít hodnotu. Ale nic zabraňuje uživateli v zadávání prázdné znaky, které by vyhovovaly omezení ověření pro typ s možnou hodnotou Null. Hodnotu Null [typů hodnot](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/value-types) (například `decimal`, `int`, `float`, a `DateTime`) jsou ze své podstaty potřeba a nepotřebujete `Required` atribut.
* `RegularExpression` Atribut omezuje znaky, které může uživatel zadat. V předchozí kód `Genre` a `Rating` musí používat pouze písmena (prázdné znaky, číslice a speciální znaky nejsou povoleny).
* `Range` Atribut omezuje hodnotu na zadaném rozsahu.
* `StringLength` Atribut Nastaví maximální délku řetězce a volitelně minimální délku. 

S ověřovacích pravidel automaticky vynucováno ASP.NET Core pomáhá zkontrolujte aplikace robustnější. Automatické ověření u modelů pomáhá chránit aplikace, protože nemáte mějte na paměti, aby se projevily při přidání nového kódu.

### <a name="validation-error-ui-in-razor-pages"></a>Chyba ověření uživatelského rozhraní v stránky Razor

Spusťte aplikaci a přejděte na stránkách nebo filmy.

Vyberte **vytvořit nový** odkaz. Vyplňte formulář s některé neplatné hodnoty. Při ověřování na straně klienta jQuery zjistí chybu, zobrazí chybovou zprávu.

![Film zobrazit formulář s několika chybami jQuery ověřování na straně klienta](validation/_static/val.png)

> [!NOTE]
> Nelze zadat desetinných míst nebo čárkami v `Price` pole. Pro podporu [k ověřování jQuery](https://jqueryvalidation.org/) v neanglická národní prostředí, které používají čárkou (",") pro desetinné čárky a formát data neanglických USA, musíte provést kroky globalizace aplikace. V tématu [další prostředky](#additional-resources) Další informace. Teď zadejte jenom celá čísla, jako je 10.

Všimněte si, jak formulář automaticky vykreslí chybovou zprávu ověření v každé pole, která obsahuje neplatnou hodnotu. Chyby se vynucují straně klienta (pomocí jazyka JavaScript a jQuery) a na straně serveru (Pokud má uživatel JavaScript zakázaná).

Významné výhodou je, že **žádné** změn kódu byly nezbytné na stránkách vytvořit nebo upravit. Jakmile DataAnnotations byly použité pro model, bylo povoleno ověření uživatelského rozhraní. Stránky Razor vytvořené v tomto kurzu automaticky převzata pravidla ověřování (pomocí atributů ověření ve vlastnostech `Movie` třída modelu). Ověření testovacího pomocí stránce Upravit stejné ověření platí.

Data formuláře není odeslány na server, dokud nejsou žádné chyby ověření na straně klienta. Ověření formuláře, které dat není vystavil jeden nebo více z následujících postupů:

* Chápat přerušení `OnPostAsync` metoda. Odeslání formuláře (vyberte **vytvořit** nebo **Uložit**). Bod rozdělení se nikdy přístupů.
* Použití [nástroj Fiddler](http://www.telerik.com/fiddler).
* Pomocí nástrojů pro vývojáře prohlížeče pro monitorování síťového provozu.

### <a name="server-side-validation"></a>Ověřování na straně serveru

Pokud je zakázáno JavaScript v prohlížeči, odesláním formuláře s chybami bude odeslána na server.

Volitelné, ověřování na straně serveru testu:

* Zakážete JavaScript v prohlížeči. Pokud zakážete nelze JavaScript v prohlížeči, zkuste jiný prohlížeč.
* Nastavit bod přerušení `OnPostAsync` metoda vytvoření nebo úprava stránky.
* Odeslání formuláře se chyby ověření.
* Ověřte, zda že stav modelu, který je neplatný:

  ```csharp
   if (!ModelState.IsValid)
   {
      return Page();
   }
  ```

Následující kód ukazuje část *Create.cshtml* stránky, který vygeneroval dříve v tomto kurzu. Použije se stránkami vytvořit a upravit k zobrazení původního formuláře a znovu zobrazit formulář v případě chyby.

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]

[Vstupní značka pomocná](xref:mvc/views/working-with-forms) používá [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) atributů a atributů HTML, které jsou potřebné pro architekturu jQuery ověření na straně klienta vytváří. [Pomocná rutina pro ověření značky](xref:mvc/views/working-with-forms#the-validation-tag-helpers) zobrazí chyby ověření. V tématu [ověření](xref:mvc/models/validation) Další informace.

Vytvořit a upravit stránky obsahují žádné ověřovací pravidla. Ověřovací pravidla a řetězce chyby se zadávají pouze v `Movie` třídy. Tato pravidla ověření budou automaticky použita pro stránky Razor, který upravit `Movie` modelu.

Když logiku ověření je nutné změnit, se provádí pouze v modelu. Ověření konzistentně platí v celé aplikaci (logiku ověření je definována v jednom místě). Ověřování na jednom místě vám pomůže uchovávat kód vyčištění a je jednodušší a aktualizace.

## <a name="using-datatype-attributes"></a>Pomocí atributů datový typ

Zkontrolujte `Movie` třídy. `System.ComponentModel.DataAnnotations` Obor názvů poskytuje atributy formátování kromě integrovanou sadu atributů ověření. `DataType` Je použit atribut `ReleaseDate` a `Price` vlastnosti.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

`DataType` Atributy pouze poskytovat pro modul zobrazení pro zobrazení dat (a poskytuje atributy, jako `<a>` pro adresy URL a `<a href="mailto:EmailAddress.com">` e-mailu). Použití `RegularExpression` atribut pro ověření formátu dat. `DataType` Atribut slouží k určení datový typ, který je specifičtější než vnitřní typ databáze. `DataType` atributy nejsou atributů ověření. V ukázkové aplikaci se zobrazí pouze datum bez čas.

`DataType` Výčtu poskytuje pro mnoho typů dat, jako je například datum, čas, telefonní číslo, měny, EmailAddress a další. `DataType` Atributu můžete také povolit aplikace automaticky poskytnout konkrétní typ funkce. Například `mailto:` může vytvořit odkaz pro `DataType.EmailAddress`. Selektor datum lze zadat pro `DataType.Date` v prohlížečích podporujících HTML5. `DataType` Atributy vysílá standardu HTML 5 `data-` (výrazný data dash) atributy, které využívají standardu HTML 5 prohlížeče. `DataType` Atributy provést **není** žádné ověřování.

`DataType.Date` neuvádí formát data, které se zobrazí. Ve výchozím nastavení, zobrazí se pole dat podle výchozích formátů podle serveru `CultureInfo`.

`DisplayFormat` Atribut slouží k explicitnímu zadání formát data:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

`ApplyFormatInEditMode` Nastavení určuje, že formátování, které bude použito při hodnota je zobrazena pro úpravy. Toto chování není vhodné pro některá pole. Například v hodnoty měny, pravděpodobně nechcete symbolu měny v úpravy uživatelského rozhraní.

`DisplayFormat` Atribut lze použít samostatně, ale obecně je vhodné používat `DataType` atribut. `DataType` Atribut přenese tak sémantika data a jak vykreslit ho na obrazovce a nabízí následující výhody, které není dostupná s DisplayFormat:

* V prohlížeči můžete povolit funkce HTML5 (např. k zobrazení ovládacího prvku kalendář, symbolu měny vhodné národního prostředí, e-mailu odkazy, atd.)
* Ve výchozím nastavení bude v prohlížeči vykreslovat data ve správném formátu podle národního prostředí.
* `DataType` Atributu můžete povolit rozhraní ASP.NET Core vybrat šablonu pravé pole k vykreslení data. `DisplayFormat` Pokud používá samotné používá šablonu řetězec.

Poznámka: k ověřování jQuery nefunguje s `Range` atribut a `DateTime`. Například následující kód vždy zobrazí chybu ověřování na straně klienta i v případě, že datum je v zadaném rozsahu:

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

Je obecně není vhodné zkompilovat pevného data v modely, takže pomocí `Range` atribut a `DateTime` se nedoporučuje.

Následující kód ukazuje kombinování atributy na jeden řádek:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]

[Začínáme s stránky Razor a EF základní](xref:data/ef-rp/intro) ukazuje pokročilejší EF základních operací s stránky Razor.

### <a name="publish-to-azure"></a>Publikování v Azure

V tématu [publikování webové aplikace ASP.NET Core Azure App Service pomocí sady Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) pokyny, jak publikovat tuto aplikaci do Azure.

## <a name="additional-resources"></a>Další zdroje

* [Práce s formuláři](xref:mvc/views/working-with-forms)
* [Globalizace a lokalizace](xref:fundamentals/localization)
* [Úvod do pomocné rutiny značky](xref:mvc/views/tag-helpers/intro)
* [Vytváření Pomocníci značky](xref:mvc/views/tag-helpers/authoring)

>[!div class="step-by-step"]
[Předchozí: Přidání nové pole](xref:tutorials/razor-pages/new-field)
[Další: nahrávání souborů](xref:tutorials/razor-pages/uploading-files)
