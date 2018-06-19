---
title: Konfigurace přenosné objekt lokalizace v ASP.NET Core
author: sebastienros
description: Tento článek představuje soubory přenosné objektů a popisuje kroky pro jejich používání v aplikaci ASP.NET Core s framework Orchard jádra.
manager: wpickett
ms.author: scaddie
ms.date: 09/26/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/portable-object-localization
ms.openlocfilehash: fbf2afd6fbc07c8068a21be15816aa45618f28d6
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
ms.locfileid: "29904544"
---
# <a name="configure-portable-object-localization-in-aspnet-core"></a>Konfigurace přenosné objekt lokalizace v ASP.NET Core

Podle [Sébastien železnic](https://github.com/sebastienros) a [Scott Addie](https://twitter.com/Scott_Addie)

Tento článek vás provede kroky pro použití přenosných objektu (PO) souborů v aplikaci ASP.NET Core s [Orchard základní](https://github.com/OrchardCMS/OrchardCore) framework.

**Poznámka:** Orchard jádra není produktu společnosti Microsoft. V důsledku toho společnost Microsoft poskytuje žádná podpora pro tuto funkci.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-a-po-file"></a>Co je soubor SP?

Soubory SP distribuují jako textové soubory s přeloženými řetězci pro daný jazyk. Některé výhody používání SP soubory místo toho *RESX* soubory zahrnují:
- Soubory SP podporovat pluralizační; *RESX* soubory nepodporují pluralizační.
- SP soubory nejsou kompilovány jako *RESX* soubory. Jako takový nejsou vyžaduje speciální kroky nástrojů a sestavení.
- Soubory SP fungují dobře u spolupráce online nástrojů pro úpravy.

### <a name="example"></a>Příklad

Tady je ukázkový soubor SP obsahující překlad pro dva řetězce ve francouzštině, včetně s jeho množném čísle:

*fr.po*

```text
#: Services/EmailService.cs:29
msgid "Enter a comma separated list of email addresses."
msgstr "Entrez une liste d'emails séparés par une virgule."

#: Views/Email.cshtml:112
msgid "The email address is \"{0}\"."
msgid_plural "The email addresses are \"{0}\"."
msgstr[0] "L'adresse email est \"{0}\"."
msgstr[1] "Les adresses email sont \"{0}\""
```

Tento příklad používá následující syntaxi:

- `#:`: Komentář označující kontextu řetězec, který má být přeložit. Do jednoho řetězce může přeložit odlišně v závislosti na tom, kde se používá.
- `msgid`: Nepřeložený řetězec.
- `msgstr`: Přeložené řetězce.

V případě podpory pluralizační lze definovat další položky.

- `msgid_plural`: Nepřeložený množném čísle řetězec.
- `msgstr[0]`: Přeložené řetězce pro případ 0.
- `msgstr[N]`: Přeložené řetězce pro případu N.

Naleznete specifikaci souboru SP [zde](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).

## <a name="configuring-po-file-support-in-aspnet-core"></a>Konfigurace SP podpora souborů v ASP.NET Core

Tento příklad vychází z aplikace ASP.NET MVC základní vygeneroval ze šablony projektů Visual Studio 2017.

### <a name="referencing-the-package"></a>Odkazování na balíček

Přidat odkaz na `OrchardCore.Localization.Core` balíček NuGet. Je k dispozici na [MyGet](https://www.myget.org/) ve zdroji balíčku následující: https://www.myget.org/F/orchardcore-preview/api/v3/index.json

*.Csproj* soubor nyní obsahuje řádek podobný následujícímu (číslo verze se liší):

[!code-xml[](localization/sample/POLocalization/POLocalization.csproj?range=9)]

### <a name="registering-the-service"></a>Registrace služby

Přidat požadované služby, aby `ConfigureServices` metodu *Startup.cs*:

[!code-csharp[](localization/sample/POLocalization/Startup.cs?name=snippet_ConfigureServices&highlight=4-21)]

Přidejte požadované middlewaru, který má `Configure` metodu *Startup.cs*:

[!code-csharp[](localization/sample/POLocalization/Startup.cs?name=snippet_Configure&highlight=15)]

Přidejte následující kód do zobrazení Razor výběru. *About.cshtml* se používá v tomto příkladu.

[!code-cshtml[](localization/sample/POLocalization/Views/Home/About.cshtml)]

`IViewLocalizer` Instance je vložit a použita k převodu text "Hello, world!".

### <a name="creating-a-po-file"></a>Vytvoření souboru SP

Vytvořte soubor s názvem  *<culture code>.po* v kořenové složce aplikace. V tomto příkladu je název souboru *fr.po* vzhledem k tomu, že se používá francouzštině:

[!code-text[](localization/sample/POLocalization/fr.po)]

Tento soubor obsahuje řetězec přeložit i řetězec francouzština přeložit. Překlady vraťte se k jejich nadřazené jazykové verzi, v případě potřeby. V tomto příkladu *fr.po* soubor se používá, pokud je požadovaná jazyková verze `fr-FR` nebo `fr-CA`.

### <a name="testing-the-application"></a>Testování aplikace

Spusťte aplikaci a přejděte na adresu URL `/Home/About`. Text **Hello, world!** Zobrazí se.

Přejděte na adresu URL `/Home/About?culture=fr-FR`. Text **Bonjour the World!** Zobrazí se.

## <a name="pluralization"></a>Pluralizační

Soubory SP podporovat pluralizační formulářů, což je užitečné, když se do jednoho řetězce potřebuje přeložit různě v závislosti na kardinalitu. Tato úloha se provádí složitý fakt, že každý jazyk definuje vlastní pravidla vyberte řetězec, který se má použít podle nastavení kardinality.

Balíček Orchard lokalizace poskytuje rozhraní API pro vyvolání tyto různé množném čísle formuláře automaticky.

### <a name="creating-pluralization-po-files"></a>Vytváření pluralizační SP souborů

Přidejte do něj následující obsah do výše uvedených *fr.po* souboru:

```text
msgid "There is one item."
msgid_plural "There are {0} items."
msgstr[0] "Il y a un élément."
msgstr[1] "Il y a {0} éléments."
```

V tématu [co je soubor SP?](#what-is-a-po-file) pro vysvětlení, co každá položka v tomto příkladu představuje.

### <a name="adding-a-language-using-different-pluralization-forms"></a>Přidání jazyk pomocí různých pluralizační formulářů

V předchozím příkladu byly použity řetězce angličtinu a francouzštinu. Angličtinu a francouzštinu máte jenom dva pluralizační formuláře a sdílet stejná pravidla formulář, který je, že kardinalitu jednoho je namapována na první množném čísle. Další Mohutnost je namapována na druhý množném čísle.

Ne všechny jazyky sdílet stejná pravidla. Tento koncept je znázorněn s České jazyk, který má tři množném čísle forms.

Vytvořte `cs.po` následujícím způsobem a Všimněte si, jak pluralizační potřebuje tři různé překlady:

[!code-text[](localization/sample/POLocalization/cs.po)]

Přijmout České lokalizace, přidejte `"cs"` do seznamu podporovaných jazykových verzí v `ConfigureServices` metoda:

```csharp
var supportedCultures = new List<CultureInfo>
{
    new CultureInfo("en-US"),
    new CultureInfo("en"),
    new CultureInfo("fr-FR"),
    new CultureInfo("fr"),
    new CultureInfo("cs")
};
```

Upravit *Views/Home/About.cshtml* souboru k vykreslení množném čísle, lokalizované řetězce pro několik cardinalities:

```cshtml
<p>@Localizer.Plural(1, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(2, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(5, "There is one item.", "There are {0} items.")</p>
```

**Poznámka:** ve scénáři skutečném světě by proměnnou použít k reprezentování počet. Zde jsme zopakujte stejný kód s tři různé hodnoty vystavit velmi konkrétní případ.

Při přepínání jazykové verze, naleznete v následujících tématech:

Pro `/Home/About`:

```html
There is one item.
There are 2 items.
There are 5 items.
```

Pro `/Home/About?culture=fr`:

```html
Il y a un élément.
Il y a 2 éléments.
Il y a 5 éléments.
```

Pro `/Home/About?culture=cs`:

```html
Existuje jedna položka.
Existují 2 položky.
Existuje 5 položek.
```

Všimněte si, že pro Česká jazyková verze, tři překlady se liší. Francouzská a anglické jazykové verze sdílet stejné konstrukce pro dvě poslední přeložené řetězce.

## <a name="advanced-tasks"></a>Pokročilé úlohy

### <a name="contextualizing-strings"></a>Contextualizing řetězce

Aplikace často obsahují řetězce k převodu na několika místech. Do jednoho řetězce může mít různé posunutí v určitých umístění v rámci aplikace (zobrazení syntaxe Razor nebo soubory třídy). Soubor SP podporuje představu o soubor kontext, který slouží ke kategorizaci reprezentované řetězec. Použitím odlišného kontextu, soubor, řetězec lze přeložit odlišně, v závislosti na kontextu souboru (nebo nedostatečná kontextu souboru).

Službu lokalizaci SP použijte název úplné třídy nebo zobrazení, která se používá při překladu řetězec. To se provádí nastavením hodnoty na `msgctxt` položku.

Vezměte v úvahu menší přidání na předchozí *fr.po* příklad. Zobrazení syntaxe Razor nacházející se v *Views/Home/About.cshtml* může být definováno jako kontext souboru nastavením vyhrazený `msgctxt` položka hodnotu:

```text
msgctxt "Views.Home.About"
msgid "Hello world!"
msgstr "Bonjour le monde!"
```

Pomocí `msgctxt` nastavit jako takový, text dochází k převodu při navigaci na `/Home/About?culture=fr-FR`. Překlad nedojde při navigaci na `/Home/Contact?culture=fr-FR`.

Pokud žádná konkrétní položka je nalezena shoda s kontextem daný soubor, Orchard základní nouzový mechanismus, který vyhledá odpovídající soubor SP bez kontextu. Za předpokladu, že se žádný konkrétní soubor kontext definované pro *Views/Home/Contact.cshtml*, navigační k `/Home/Contact?culture=fr-FR` načte soubor SP jako například:

[!code-text[](localization/sample/POLocalization/fr.po)]

### <a name="changing-the-location-of-po-files"></a>Změna umístění souborů PO

Výchozí umístění souborů PO lze změnit v `ConfigureServices`:

```csharp
services.AddPortableObjectLocalization(options => options.ResourcesPath = "Localization");
```

V tomto příkladu jsou soubory SP načteny z *lokalizace* složky.

### <a name="implementing-a-custom-logic-for-finding-localization-files"></a>Implementace vlastní logiky pro hledání lokalizační soubory

Potřeby složitější logikou, vyhledejte SP soubory `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` rozhraní můžete implementovat a zaregistrován jako služba. To je užitečné, když SP soubory mohou být uloženy v různých umístěních, nebo pokud soubory lze nalézt v rámci hierarchie složek.

### <a name="using-a-different-default-pluralized-language"></a>Pomocí pluralized jazyk

Balíček obsahuje `Plural` rozšíření metod, které jsou specifické pro dva množném čísle formuláře. Pro jazyky, které vyžadují další množném vytvořte metody rozšíření. Pomocí metody rozšíření, nebude nutné zadávat jakékoli lokalizace souboru pro výchozí jazyk &mdash; původní řetězce jsou již k dispozici přímo v kódu.

Můžete použít další Obecné `Plural(int count, string[] pluralForms, params object[] arguments)` přetížení, které přijímá pole řetězců překladů.
