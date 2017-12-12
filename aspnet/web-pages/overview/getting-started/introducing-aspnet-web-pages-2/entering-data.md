---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
title: "Představení technologie ASP.NET Web Pages – zadávání dat databáze pomocí formulářů | Microsoft Docs"
author: tfitzmac
description: "V tomto kurzu se dozvíte, jak vytvořit formuláře položky a pak zadejte data, která můžete získat z formuláře do databázové tabulky při použití technologie ASP.NET Web Pages (..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: d37c93fc-25fd-4e94-8671-0d437beef206
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
msc.type: authoredcontent
ms.openlocfilehash: b74eecb16b2c4695bb417816b90f701f724cc9d0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---entering-database-data-by-using-forms"></a>Představení technologie ASP.NET Web Pages – zadávání dat databáze pomocí formulářů
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> V tomto kurzu se dozvíte, jak vytvořit formuláře položku a pak zadejte data, která můžete získat z formuláře do databázové tabulky při použití technologie ASP.NET Web Pages (Razor). Předpokládá, že jste dokončili řady prostřednictvím [základní informace z formuláře HTML na webových stránkách ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251581).
> 
> Získáte informace:
> 
> - Informace o tom, jak zpracovat vstupní formuláře.
> - Postup přidání dat (Vložit) v databázi.
> - Jak zajistit, aby uživatelé zadali požadovaná hodnota ve formuláři (postup ověření vstupu uživatele).
> - Jak zobrazit chyby ověření.
> - Jak přejít na jinou stránku na aktuální stránce.
>   
> 
> Funkce nebo technologie popsané:
> 
> - `Database.Execute` Metoda.
> - SQL `Insert Into` – příkaz
> - `Validation` Pomocné rutiny.
> - `Response.Redirect` Metoda.


## <a name="what-youll-build"></a>Co budete sestavení

V tomto kurzu dříve, ukázal, jak vytvořit databázi, jste zadali databázových dat úpravou databázi přímo ve službě WebMatrix, práce **databáze** pracovního prostoru. Ve většině aplikací, který není praktické způsob, jak ukládat data do databáze, ale. Proto v tomto kurzu vytvoříte webové rozhraní, která umožní sobě ani o nikom zadejte data a uložit ho do databáze.

Vytvoříte stránky, kde můžete zadat nový filmy. Stránka bude obsahovat zadávacím formulářem, který má pole (polí), kde můžete zadat název filmu, genre a rok. Stránka bude vypadat této stránce:

!['Přidat film' stránku v prohlížeči](entering-data/_static/image1.png)

Do textových polí bude HTML `<input>` prvky, které bude vypadat jako tento kód:

`<input type="text" name="genre" value="" />`

## <a name="creating-the-basic-entry-form"></a>Vytvoření základního formuláře

Vytvoření stránky s názvem *AddMovie.cshtml*.

Nahraďte, co je v souboru s následující kód. Přepsat všechno; blok kódu v horní části přidáte za chvíli.

[!code-cshtml[Main](entering-data/samples/sample1.cshtml)]

Tento příklad ukazuje typické HTML pro vytvoření formuláře. Používá `<input>` elementy pro textová pole a tlačítko pro odeslání. Titulky pro textová pole jsou vytvořeny pomocí standardních `<label>` elementy. `<fieldset>` a `<legend>` elementy put dobrý okolo formuláře.

Všimněte si, že se na této stránce `<form>` element používá `post` hodnotu `method` atribut. V předchozích kurzu jste vytvořili formulář, který používá `get` metoda. Který byl správné, protože i když formulář odeslat hodnoty na server, žádost nebyly provedeny žádné změny. Všechny neodpovídala načítání dat byla různými způsoby. Ale v této stránce můžete *bude* proveďte změny – se chystáte přidat nové záznamy v databázi. Proto měli používat tento formulář `post` metoda. (Další informace o rozdílu mezi `GET` a `POST` operací, najdete v článku[GET, POST a zabezpečení protokolu HTTP příkaz](https://go.microsoft.com/fwlink/?LinkId=251581#GET,_POST,_and_HTTP_Verb_Safety) bočním panelu v předchozí kurzu.)

Všimněte si, že má každého textového pole `name` – element (`title`, `genre`, `year`). Jak už jste viděli v předchozí kurzu, tyto názvy jsou důležité, protože tyto názvy musí mít, abyste získali vstup uživatele později. Můžete použít všechny názvy. Je vhodné používat smysluplný názvy, které vám pomůžou mějte na paměti, jaká data se kterými pracujete.

`value` Atribut jednotlivých `<input>` element obsahuje bit kódu Razor (například `Request.Form["title"]`). Zjistili verzi platí to v předchozích kurzu zachovat hodnota zadaná do textového pole (pokud existuje), po odeslání formuláře.

## <a name="getting-the-form-values"></a>Získání hodnot formuláře

Dále přidáte kód, který zpracovává formuláře. V přehledu můžete to udělat následující:

1. Zkontrolujte, zda je odeslání stránky (byla odeslána). Chcete spustit pouze, když uživatelé nejsou při prvním spuštění stránky klikli na tlačítko kódu.
2. Získáte hodnoty, které uživatel zadal do textových polí. V takovém případě vzhledem k tomu, že je pomocí formuláře `POST` operace, získat hodnot z formulářů `Request.Form` kolekce.
3. Vložení hodnoty jako nový záznam v *filmy* databázové tabulky.

Na začátek souboru přidejte následující kód:

[!code-cshtml[Main](entering-data/samples/sample2.cshtml)]

Několik prvních řádků vytvářet proměnné (`title`, `genre`, a `year`) pro uložení hodnot z do textových polí. Na řádku `if(IsPost)` zajišťuje, že jsou nastavené proměnné *pouze* když uživatelé kliknou na **přidat film** tlačítko – to znamená, když formulář byl odeslán.

Jak už jste viděli v dřívější kurzu, získat hodnotu v textovém poli pomocí výrazu jako `Request.Form["name"]`, kde *název* je název `<input>` elementu.

Názvy proměnných (`title`, `genre`, a `year`) jsou libovolný. Jako názvy, které přiřadíte `<input>` prvky, můžete volat je jakkoli chcete. (Názvy proměnných nemusí odpovídat názvu atributy `<input>` prvky na formuláři.) Ale stejně jako u `<input>` elementy, je vhodné používat názvy proměnných, které zahrnují data, která obsahují. Při psaní kódu konzistentní názvy bylo snazší pro vás mějte na paměti, jaká data se kterými pracujete.

## <a name="adding-data-to-the-database"></a>Přidání dat do databáze

V kódu blokovat jste právě přidanou, jenom *uvnitř* složená závorka ( `}` ) z `if` blokovat (nikoli pouze uvnitř bloku kódu), přidejte následující kód:

[!code-csharp[Main](entering-data/samples/sample3.cs)]

V tomto příkladu je podobný kódu, který jste použili v předchozí kurzu k načtení a zobrazení data. Řádek, který začíná `db =` otevře databáze, jako je před a na další řádek definuje příkazu jazyka SQL, znovu jako viděli dříve. Ale tentokrát definuje SQL `Insert Into` příkaz. Následující příklad ukazuje Obecná syntaxe `Insert Into` příkaz:

`INSERT INTO table (column1, column2, column3, ...) VALUES (value1, value2, value3, ...)`

Jinými slovy zadáte tabulku, kterou chcete použít příkaz insert, pak sloupce k vložení do seznamu a potom zobrazí seznam hodnot pro vložení. (Jak je uvedeno před, SQL není velká a malá písmena ale někteří uživatelé velká počáteční klíčová slova, aby bylo snazší číst příkaz.)

Sloupce, které chcete vložit do jsou již uveden v příkazu – `(Title, Genre, Year)`. Zajímavé část je, jak získat hodnoty z textová pole na `VALUES` část příkazu. Místo skutečných hodnot, uvidíte `@0`, `@1`, a `@2`, které jsou samozřejmě zástupné symboly. Při spuštění příkazu (na `db.Execute` řádku), předat hodnoty, které jste získali z do textových polí.

**Důležité!** Mějte na paměti, že je jediným způsobem, kdy by měla obsahovat data online zadaná uživatelem v příkazu jazyka SQL je používat zástupné symboly, jak kterou tady vidíte (`VALUES(@0, @1, @2)`). Pokud jste řetězení uživatelský vstup do příkazu SQL, otevřete sami na útok prostřednictvím injektáže SQL, jak je popsáno v [základy formuláře v aplikaci ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251581) (předchozí kurzu).

Stále uvnitř `if` blokovat, přidejte následující řádek po `db.Execute` řádku:

[!code-css[Main](entering-data/samples/sample4.css)]

Po nové film byla vložena do databáze, tento přeskakování můžete (přesměrování) *filmy* stránky, zobrazí se na video, které jste zadali. `~` Operátor znamená "root webu." ( `~` Operátor lze použít pouze v stránek ASP.NET, není ve formátu HTML obecně.)

Blok dokončení kódu vypadá v tomto příkladu:

[!code-cshtml[Main](entering-data/samples/sample5.cshtml)]

## <a name="testing-the-insert-command-so-far"></a>Testování příkaz Insert (dosavadní)

Ještě neprovádí, ale teď je vhodná doba k testování.

Ve stromovém zobrazení souborů ve službě WebMatrix, klikněte pravým tlačítkem myši *AddMovie.cshtml* a pak klikněte na tlačítko **spustit v prohlížeči**.

!['Přidat film' stránku v prohlížeči](entering-data/_static/image2.png)

(Pokud skončili na jinou stránku v prohlížeči, ujistěte se, že adresa URL je `http://localhost:nnnnn/AddMovie`), kde  *nnnnn*  je číslo portu, který používáte.)

Obdrželi jste chybovou stránku? Pokud ano, pečlivě si přečtěte a ujistěte se, že vypadá kód přesně co byl uveden výše.

Zadejte film ve formátu &mdash; použít například "Občan Kratochvílová", "Obrázkům" a "1941". (Nebo jiná.) Pak klikněte na tlačítko **přidat film**.

Pokud všechno proběhne správně, budete přesměrováni na *filmy* stránky. Ujistěte se, že je uvedený nový film.

![Filmy stránky zobrazující nově přidány filmu](entering-data/_static/image3.png)

## <a name="validating-user-input"></a>Ověřování uživatelského vstupu

Přejděte zpět *AddMovie* stránky a potom ho spusťte znovu. Zadejte jiný film, ale tentokrát, zadejte jenom název &mdash; zadejte například "Přihlášení ' v the deště". Pak klikněte na tlačítko **přidat film**.

Budete přesměrováni na *filmy* stránku znovu. Můžete najít nové film, ale je neúplný.

![Filmy stránky zobrazující nové video, které chybí některé hodnoty](entering-data/_static/image4.png)

Pokud jste vytvořili *filmy* tabulce explicitně uvedli jste, že žádné pole může mít hodnotu null. Zde máte formuláře položky pro nové filmy a pole jste ponechat prázdné. To je chyba.

V takovém případě nebyla ve skutečnosti vyvolat databáze (nebo *throw*) k chybě. Nebyla zadáte genre nebo rok, takže kód *AddMovie* stránka považována za takzvané tyto hodnoty *prázdné řetězce*. Když SQL `Insert Into` byl spuštěn příkaz, pole genre a roce neměly užitečné data v nich, ale jejich nebyly hodnotu null.

Samozřejmě nechcete umožnit uživatelům zadání půl prázdný film informace do databáze. Řešení je k ověření vstupu uživatele. Na začátku ověření bude jednoduše Ujistěte se, že uživatel zadal hodnotu pro všechna pole (to znamená, že žádný z nich obsahuje prázdný řetězec).

> [!TIP] 
> 
> **Hodnotu Null, prázdný řetězec**
> 
> Při programování, je rozdíl mezi různé názory "žádná hodnota." Obecně platí, je hodnota *null* Pokud nikdy byla nastavena nebo inicializovat žádným způsobem. Na rozdíl od proměnné, která očekává textová data (řetězce), může být nastaven na *prázdný řetězec*. V takovém případě hodnota není null. je právě byla explicitně nastaven na řetězec znaků, jehož délka je nulová. Tyto dva příkazy ukazují, rozdíl:
> 
> [!code-csharp[Main](entering-data/samples/sample6.cs)]
> 
> Ho má obtížnější než, ale důležité je, že `null` představuje řazení neurčeném stavu.
> 
> Teď a potom je důležité si uvědomit, přesně když je hodnota null a kdy je právě prázdný řetězec. V kódu *AddMovie* , dostanete hodnoty do textových polí pomocí `Request.Form["title"]` a tak dále. Při prvním spuštění stránky (před kliknutím na tlačítko), hodnota `Request.Form["title"]` má hodnotu null. Ale při odeslání formuláře, `Request.Form["title"]` získá hodnotu `title` textové pole. Informace o tom, ale není null; prázdné textové pole právě v ní má prázdný řetězec. Proto při spuštění kódu v reakci na tlačítko klikněte, `Request.Form["title"]` se nachází prázdný řetězec.
> 
> Proč je důležité tento rozdíl? Pokud jste vytvořili *filmy* tabulce explicitně uvedli jste, že žádné pole může mít hodnotu null. Ale zde máte formuláře položky pro nové filmy a pole jste ponechat prázdné. To bude přiměřeně by uživatel očekával databázi stěžovat si při pokusu o uložení nové filmy, které nebyly k dispozici hodnoty pro genre nebo rok. Je to bodem ale &mdash; i v případě, že tyto textová pole ponecháte prázdné, nejsou hodnoty null; jsou prázdné řetězce. Výsledkem je, budete moci ukládat nové filmy do databáze s těmito sloupci prázdný &mdash; , ale není null! &mdash;hodnoty. Proto je nutné provést se, že si uživatelé odeslat řetězec prázdný, což lze provést pomocí ověřování vstupu uživatele.


### <a name="the-validation-helper"></a>Pomocná rutina pro ověření

Rozhraní ASP.NET Web Pages obsahuje pomocné rutiny &mdash; `Validation` pomocná &mdash; můžete zajistit, že uživatelé zadat data, která vyhovuje vašim požadavkům. `Validation` Helper je jedním z pomocné rutiny, které je součástí pro webové stránky ASP.NET, takže nemusíte jej nainstalovat jako balíček pomocí NuGet, způsob instalace Pomocníka Gravatar v dřívější kurzu.

K ověření vstupu uživatele, můžete to udělat následující:

- Pomocí kódu můžete určit, že chcete požadovat hodnoty do polí na stránce.
- Uvést testu do kód tak, aby informace film, jsou přidány do databáze pouze v případě, že je všechno správně ověří.
- Přidejte kód do kódu mají zobrazovat chybové zprávy.

V bloku kódu v *AddMovie* doprava nahoru v horní části před deklarace proměnných a přidejte následující kód:

[!code-csharp[Main](entering-data/samples/sample7.cs)]

Volání `Validation.RequireField` jednou pro každé pole (`<input>` element) místo vyžadují položku. Můžete také přidat vlastní chybovou zprávu pro každé volání, jak je znázorněno zde. (Jsme nejrůznější zprávy jenom k zobrazení, které můžete vložit nic, co se vám líbí existuje).

Pokud dojde k potížím, ale chcete zabránit nové informace film z vkládání do databáze. V `if(IsPost)` blokovat, použijte `&&` (logické a) Chcete-li přidat další podmínku, která se testuje `Validation.IsValid()`. Když jste hotovi, celek `if(IsPost)` bloku vypadá jako tento kód:

[!code-csharp[Main](entering-data/samples/sample8.cs)]

Pokud dojde k chybě ověření s žádným z pole, která jste zaregistrovali pomocí `Validation` pomocné rutiny, `Validation.IsValid` metoda vrátí hodnotu false. A v takovém případě žádný kód v tomto bloku se spustí, takže žádné položky neplatný film se vloží do databáze. A samozřejmě nejsou přesměrován na *filmy* stránky.

Kompletní kód bloku, včetně ověřovacího kódu teď vypadá v tomto příkladu:

[!code-cshtml[Main](entering-data/samples/sample9.cshtml?highlight=10)]

## <a name="displaying-validation-errors"></a>Zobrazení chyb při ověřování

Posledním krokem je zobrazit chybové zprávy. Můžete zobrazit jednotlivé zprávy pro jednotlivé chyby ověření, nebo můžete zobrazit souhrn, nebo obojí. V tomto kurzu, můžete to udělat i tak, abyste viděli, jak to funguje.

Vedle každého `<input>` element, který jste ověřování, volání `Html.ValidationMessage` metoda a předejte ji název `<input>` element se ověřování. Můžete zadat `Html.ValidationMessage` metoda práva, kde chcete, zobrazí se chybová zpráva. Při spuštění stránky, `Html.ValidationMessage` metoda vykreslí `<span>` elementu, kde bude přejděte chybu ověření. (Pokud se nezobrazí žádná chyba `<span>` prvek je vykreslovaný, ale neexistuje žádný text v ní.)

Změňte kód na stránce tak, že obsahují `Html.ValidationMessage` metoda pro každou ze tří `<input>` elementy na stránce, jako jsou v tomto příkladu:

[!code-cshtml[Main](entering-data/samples/sample10.cshtml?highlight=3,8,13)]

Jak funguje souhrn najdete taky přidejte následující kód a kódu hned po `<h1>Add a Movie</h1>` elementu na stránce:

[!code-cshtml[Main](entering-data/samples/sample11.cshtml)]

Ve výchozím nastavení `Html.ValidationSummary` metoda zobrazí v seznamu všech ověřovacích zpráv ( `<ul>` element, který je uvnitř `<div>` element). Stejně jako u `Html.ValidationMessage` metody se vždy vykreslí značku pro souhrn ověření; Pokud nebudou nalezeny žádné chyby, jsou vykreslovány žádné položky seznamu.

Souhrn může být alternativní způsob zobrazení ověřovacích zpráv místo pomocí `Html.ValidationMessage` metodu pro zobrazení jednotlivé chyby specifické pro pole. Nebo můžete použít souhrn a podrobnosti. Nebo můžete použít `Html.ValidationSummary` metodu pro zobrazení Obecná chyba a potom použijte jednotlivých `Html.ValidationMessage` volání zobrazíte podrobnosti.

Kompletní stránku teď vypadá v tomto příkladu:

[!code-cshtml[Main](entering-data/samples/sample12.cshtml)]

Je to. Nyní můžete otestovat stránce Přidání film, ale ponechat si jeden nebo více polí. Když to uděláte, se zobrazí chybová zpráva zobrazení:

![Přidat stránku film zobrazení chybové zprávy ověření](entering-data/_static/image5.png)

## <a name="styling-the-validation-error-messages"></a>Práce se styly chybových zpráv ověření

Uvidíte, že nejsou žádné chybové zprávy, ale jejich nemáte zvýraznění skutečně velmi dobře. Snadný způsob, jak formátování chybové zprávy, když není k dispozici.

K formátování jednotlivých chybové zprávy, které jsou zobrazeny `Html.ValidationMessage`, vytvořit třídu stylu CSS s názvem `field-validation-error`. K definování vzhledu pro souhrn ověření, vytvořit třídu stylu CSS s názvem `validation-summary-errors`.

Chcete-li zjistit, jak tento postup funguje, přidejte `<style>` element uvnitř `<head>` části stránky. Potom zadejte styl třídy s názvem `field-validation-error` a `validation-summary-errors` obsahující následující pravidla:

[!code-cshtml[Main](entering-data/samples/sample13.cshtml?highlight=4-17)]

Za normálních okolností byste pravděpodobně umístili informace o stylu do samostatné *.css* souboru, ale pro zjednodušení můžete vložit je na stránce pro nyní. (Později v tomto kurzu skladu přesunete pravidla šablon stylů CSS do samostatné *.css* souboru.)

Pokud dojde k chybě ověření `Html.ValidationMessage` metoda vykreslí `<span>` element, který zahrnuje `class="field-validation-error"`. Přidáním definici stylu pro tuto třídu můžete nakonfigurovat, zprávu, která bude vypadat takto. Pokud nejsou chyby, `ValidationSummary` metoda podobně dynamicky vykreslí atribut `class="validation-summary-errors"`.

Znovu spustit stránky a úmyslně vynechte několik polí. Chyby jsou nyní výraznější. (Ve skutečnosti se jste overdone, ale který se používá pouze pro zobrazit, co můžete dělat.)

![Přidat film stránky zobrazující chyby ověřování, které mají byl navržen tak](entering-data/_static/image6.png)

## <a name="adding-a-link-to-the-movies-page"></a>Přidání odkazu na stránku filmy

Jeden posledním krokem je to vhodné pro zajištění *AddMovie* stránky z původní film výpis.

Otevřete *filmy* stránku znovu. Po zavření `</div>` značky, který následuje `WebGrid` pomocné rutiny, přidejte následující kód:

[!code-cshtml[Main](entering-data/samples/sample14.cshtml)]

Jak už jste viděli před, ASP.NET interpretuje `~` operátor jako kořenového adresáře webu. Nemusíte používat `~` operátor; můžete použít kód `<a href="./AddMovie">Add a movie</a>` nebo nějakým způsobem zadat cestu, která funguje s technologií HTML. Ale `~` operátor je dobré obecný přístup při vytváření odkazů pro stránky Razor, protože umožňuje flexibilnější webu – Pokud přesunete aktuální stránku do podsložky, budou stále moct odkaz *AddMovie* stránky. (Nezapomeňte, že `~` operátor funguje jen v *.cshtml* stránky. ASP.NET se rozumí, ale není standardní HTML.)

Pokud jste hotovi, spusťte *filmy* stránky. Tato stránka bude vypadat:

![Filmy stránku s odkazem na stránku, filmy přidat.](entering-data/_static/image7.png)

Klikněte na tlačítko **přidat video** a ujistěte se, že přejde na odkaz *AddMovie* stránky.

## <a name="coming-up-next"></a>Objevuje další

V dalším kurzu budete zjistěte, jak umožnit uživatelům upravit data, která je již v databázi.

## <a name="complete-listing-for-addmovie-page"></a>Úplný seznam AddMovie stránky

[!code-cshtml[Main](entering-data/samples/sample15.cshtml)]

## <a name="additional-resources"></a>Další prostředky

- [Úvod do rozhraní ASP.NET Web programování pomocí syntaxe Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Vložit do příkazu SQL](http://www.w3schools.com/sql/sql_insert.asp) na webu W3Schools
- [Ověřování uživatelského vstupu v rozhraní ASP.NET Web Pages lokality](https://go.microsoft.com/fwlink/?LinkId=253002). Další informace o práci s `Validation` pomocné rutiny.

>[!div class="step-by-step"]
[Předchozí](form-basics.md)
[další](updating-data.md)
