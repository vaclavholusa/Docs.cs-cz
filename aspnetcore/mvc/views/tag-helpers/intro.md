---
title: Pomocné rutiny značky v ASP.NET Core
author: rick-anderson
description: Zjistěte, jaké jsou pomocné rutiny značky a jejich použití v ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 2/14/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/intro
ms.openlocfilehash: 27246ece3eaaecb708f922bcaaf05658034bce82
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="tag-helpers-in-aspnet-core"></a>Pomocné rutiny značky v ASP.NET Core

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-are-tag-helpers"></a>Jaké jsou pomocné rutiny značky?

Pomocníci značky povolit kódu na straně serveru k účasti ve vytváření a vykreslení elementů HTML v souborech Razor. Například předdefinovaná `ImageTagHelper` můžete připojit číslo verze pro název bitové kopie. Při každé změně bitovou kopii, server vygeneruje nové jedinečné verze pro bitovou kopii, tak je zaručeno, že klienti získat aktuální image (ne zastaralé bitové kopie v mezipaměti). Existuje mnoho předdefinovaných značky pomocníci pro běžné úlohy -, jako je například vytváření formulářů, odkazy, načítání prostředků a další - a i další dostupné ve veřejných úložišť GitHub a jako NuGet balíčky. Pomocníci značky vytvořené v C# a jejich cílových elementů HTML na základě název elementu, název atributu nebo nadřazené značky. Například předdefinovaná `LabelTagHelper` HTML, můžete vybrat `<label>` element při `LabelTagHelper` jsou použity atributy. Pokud jste obeznámeni s [pomocné objekty HTML](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), značka Pomocníci snížit explicitní přechodů mezi HTML a C# v zobrazení syntaxe Razor. V mnoha případech pomocné objekty HTML poskytnout alternativní způsob konkrétní pomocné rutiny značky, ale je důležité vědět, že pomocné rutiny značky není nahradit pomocné rutiny HTML a neexistuje žádná značka Pomocník pro každý pomocné rutiny HTML. [Značka pomocné rutiny ve srovnání s pomocné objekty HTML](#tag-helpers-compared-to-html-helpers) vysvětluje rozdíly podrobněji.

## <a name="what-tag-helpers-provide"></a>Co zadejte značky pomocné rutiny

**Vývoj prostředí HTML-friendly** pro nejvíce část kódu Razor pomocí značky Pomocníci vypadá jako standardní HTML. Front-endu Designer obeznámen s HTML + CSS + JavaScript můžete upravit Razor bez učení syntaxi Razor C#.

**Bohaté prostředí technologie IntelliSense pro vytvoření značky HTML a Razor** jde sharp naproti tomu do pomocné rutiny HTML, předchozí postup k vytvoření serverové značek v zobrazení syntaxe Razor. [Značka pomocné rutiny ve srovnání s pomocné objekty HTML](#tag-helpers-compared-to-html-helpers) vysvětluje rozdíly podrobněji. [Podporu technologie IntelliSense pro značku Pomocníci](#intellisense-support-for-tag-helpers) vysvětluje IntelliSense prostředí. I vývojáři došlo se syntaxí Razor C# jsou zvýšit produktivitu při používání značky Pomocníci než psaní kódu jazyka C# Razor.

**Tak, aby vás zvýšit produktivitu a moct vytvářet robustnější a spolehlivé a udržovatelného kódu pomocí informace, které jsou k dispozici pouze na serveru** v minulosti mantra o aktualizaci bitové kopie byl například změnit název obrázku při změně obrázek. Bitové kopie by měl být intenzivně do mezipaměti z důvodů výkonu, a pokud změníte název bitové kopie, riskujete, že klienti získávání kopii zastaralé. V minulosti po obrázku bylo upraveno, musel změnit název a každý odkaz na obrázek ve webové aplikaci potřeba aktualizovat. Ne jenom je to velmi práce že náročné, je také náchylný (můžete může neproběhly odkaz, zadejte omylem nesprávný řetězec atd.) Integrované `ImageTagHelper` lze provést pro vás automaticky. `ImageTagHelper` Můžete připojit z verze číslo název bitové kopie, takže při každé změně bitovou kopii, server automaticky vygeneruje nové verze jedinečný pro bitovou kopii. Klienti jsou zaručit získat aktuální image. Tato úspory odolné a práci se dodává v podstatě volné pomocí `ImageTagHelper`.

Pomocné rutiny nejvíce integrovaných značky cíli standardní elementů HTML a poskytuje serverové atributy elementu. Například `<input>` element používaných v zobrazeních. v mnoha *zobrazení nebo účet* složka obsahuje `asp-for` atribut. Tento atribut extrahuje název vlastnosti zadaného modelu do vykreslené kódu HTML. Vezměte v úvahu Razor zobrazení s modelem následující:

```csharp
public class Movie
{
    public int ID { get; set; }
    public string Title { get; set; }
    public DateTime ReleaseDate { get; set; }
    public string Genre { get; set; }
    public decimal Price { get; set; }
}
```

Následující kód Razor:

```cshtml
<label asp-for="Movie.Title"></label>
```

Generuje následující HTML:

```html
<label for="Movie_Title">Title</label>
```

`asp-for` Atribut je k dispozici ve `For` vlastnost [LabelTagHelper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.labeltaghelper?view=aspnetcore-2.0). V tématu [Autor značky Pomocníci](xref:mvc/views/tag-helpers/authoring) Další informace.

## <a name="managing-tag-helper-scope"></a>Správa značka pomocná oboru

Značka pomocné rutiny oboru řídí kombinaci `@addTagHelper`, `@removeTagHelper`a "!" výslovný nesouhlas s znak.

<a name="add-helper-label"></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a>`@addTagHelper` zpřístupní značky pomocné rutiny

Pokud vytvoříte novou webovou aplikaci ASP.NET Core s názvem *AuthoringTagHelpers* (bez jakéhokoli ověřování), následující *Views/_ViewImports.cshtml* soubor bude přidán do projektu:

[!code-cshtml[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]

`@addTagHelper` – Direktiva zpřístupní Pomocníci značky k zobrazení. V takovém případě je soubor zobrazení *Views/_ViewImports.cshtml*, který ve výchozím nastavení dědí všechny soubory zobrazení v *zobrazení* složku a podadresářích; zpřístupnění značky pomocné rutiny. Výše uvedený kód používá syntaxi zástupný znak ("\*") Chcete-li určit, že všechny značky pomocníky v zadaném sestavení (*Microsoft.AspNetCore.Mvc.TagHelpers*) bude k dispozici pro každý soubor zobrazení v *zobrazení* nebo dílčí adresáři. První parametr po `@addTagHelper` určuje značky Pomocníci načíst (používáme "\*" pro všechny značky pomocníky), a druhý parametr "Microsoft.AspNetCore.Mvc.TagHelpers" Určuje sestavení obsahující pomocné rutiny značky. *Microsoft.AspNetCore.Mvc.TagHelpers* je sestavení pro předdefinované Pomocníci značky základní technologie ASP.NET.

Ke zveřejnění všechny značky Pomocníci v tomto projektu (která vytvoří sestavení s názvem *AuthoringTagHelpers*), byste použili následující:

[!code-cshtml[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]

Pokud projekt obsahuje `EmailTagHelper` s výchozí obor názvů (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), můžete zadat plně kvalifikovaný název (FQN) Helper značky:

```cshtml
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

Přidání značka pomocné rutiny zobrazení pomocí FQN, je nejprve přidat FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) a potom název sestavení (*AuthoringTagHelpers*). Většina vývojářů dávají přednost používání "\*" syntaxi se zástupnými znaky. Zástupný znak syntaxe umožňuje zástupný znak "\*" jako příponu v FQN. Například některé z následujících direktivy budou předány `EmailTagHelper`:

```cshtml
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

Jak je uvedeno nahoře, přidání `@addTagHelper` direktivy k *Views/_ViewImports.cshtml* souboru zpřístupní pomocná značky ke všem souborům zobrazení v *zobrazení* adresář a dílčí adresáře. Můžete použít `@addTagHelper` v souborech konkrétní zobrazení, pokud se chcete přihlásit k vystavení pomocníka značky, který má jenom tato zobrazení.

<a name="remove-razor-directives-label"></a>

### <a name="removetaghelper-removes-tag-helpers"></a>`@removeTagHelper` Odebere značky pomocné rutiny

`@removeTagHelper` Má stejné dva parametry jako `@addTagHelper`, a odebere značky pomocné rutiny, která byla přidána dříve. Například `@removeTagHelper` použít konkrétní zobrazení odebere zadané pomocné rutiny značky ze zobrazení. Pomocí `@removeTagHelper` v *Views/Folder/_ViewImports.cshtml* souboru odebere zadané pomocné rutiny značku ze všech zobrazení v *složky*.

### <a name="controlling-tag-helper-scope-with-the-viewimportscshtml-file"></a>Řízení obor značky Pomocník s *_ViewImports.cshtml* souboru

Můžete přidat *_ViewImports.cshtml* libovolné složky zobrazení a zobrazení, použije modul direktivy z obou tento soubor a *Views/_ViewImports.cshtml* souboru. Pokud jste přidali prázdnou *Views/Home/_ViewImports.cshtml* souboru *Domů* zobrazení, by žádná změna protože *_ViewImports.cshtml* soubor sčítání. Všechny `@addTagHelper` direktivy přidáte *Views/Home/_ViewImports.cshtml* souboru (které nejsou ve výchozím *Views/_ViewImports.cshtml* souboru) by vystavit tyto značky pomocné rutiny pro zobrazení pouze v *Domů* složky.

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a>Zrušení jednotlivé elementy

Můžete zakázat pomocné rutiny značky na úrovni element výslovný nesouhlas s znakem pomocná značky ("!"). Například `Email` ověření je zakázané v `<span>` znakem výslovný nesouhlas s pomocná značky:

```cshtml
<!span asp-validation-for="Email" class="text-danger"></!span>
```

Znak výslovný nesouhlas s pomocná značky je nutné použít pro počáteční a koncovou značku. (Editoru Visual Studio automaticky přidá znak vyjádření výslovného nesouhlasu s uzavírací značku při přidávání jednoho ke značce otevírání). Po přidání znak vyjádření výslovného nesouhlasu s elementu a atributů značky Pomocník se už nezobrazuje v rozlišovací písmo.

<a name="prefix-razor-directives-label"></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a>Pomocí `@tagHelperPrefix` aby explicitní použití pomocníka značky

`@tagHelperPrefix` – Direktiva umožňuje zadat řetězec předpony značky, chcete-li povolit podporu pomocníků značky a vytvoření značky pomocná využití explicitní. Můžete například přidat následující kód do *Views/_ViewImports.cshtml* souboru:

```cshtml
@tagHelperPrefix th:
```
Následující obrázek kód předponu značky pomocná nastavena na `th:`, takže jenom elementy, pomocí předponu `th:` podporu pomocníků značky (povolené značky pomocné prvky mít rozlišovací písmo). `<label>` a `<input>` prvky mít předponu značky pomocná a povolenými značky Pomocník při `<span>` není element.

![obrázek](intro/_static/thp.png)

Stejná hierarchie pravidla, které platí pro `@addTagHelper` platí také pro `@tagHelperPrefix`.

## <a name="intellisense-support-for-tag-helpers"></a>Podporu technologie IntelliSense pro značku pomocné rutiny

Když vytvoříte novou webovou aplikaci ASP.NET v sadě Visual Studio, přidává balíček NuGet "Microsoft.AspNetCore.Razor.Tools". Toto je balíček, který přidá značku pomocné nástroje.

Vezměte v úvahu zápis HTML `<label>` elementu. Jakmile zadáte `<l` v editoru Visual Studio IntelliSense zobrazí odpovídající prvky:

![obrázek](intro/_static/label.png)

Nejenže můžete získat HTML – Nápověda, ale na ikonu ("@" symbol with "<>" v něm).

![obrázek](intro/_static/tagSym.png)

identifikuje element jako cílem pomocníků značky. Čistý elementů HTML (například `fieldset`) ikona "<>" zobrazení.

Prostý jazyk HTML `<label>` značka zobrazí značky HTML (s výchozí motiv sady Visual Studio barvu) hnědá písmeny, atributy červeně, a atribut hodnoty modře.

![obrázek](intro/_static/LableHtmlTag.png)

Po zadání `<label`, IntelliSense zobrazí seznam dostupné atributy HTML nebo šablon stylů CSS a atributy cílové pomocná značky:

![obrázek](intro/_static/labelattr.png)

Dokončování IntelliSense umožňuje zadat klávesy tab můžete dokončit příkaz s vybranou hodnotu:

![obrázek](intro/_static/stmtcomplete.png)

Jakmile je zadán pomocný značka atribut, značky a atribut písem změnit. Použití výchozí sady Visual Studio "Blue" nebo "Light" barvu motivu, písmo je tučné fialová. Pokud používáte "Tmavý" motiv je písmo tučné šedozelená. Bitové kopie v tomto dokumentu byly pořízeny pomocí výchozí motiv.

![obrázek](intro/_static/labelaspfor2.png)

Můžete zadat sady Visual Studio *CompleteWord* zástupce (Ctrl + mezerník je [výchozí](https://docs.microsoft.com/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) uvnitř dvojitých uvozovek (""), a je nyní v jazyce C#, stejně, jako by byl v třídě C#. IntelliSense zobrazí všechny metody a vlastnosti v modelu stránky. Metody a vlastnosti jsou k dispozici, protože je vlastnost typu `ModelExpression`. Na obrázku níže I mě úpravy `Register` zobrazení, proto `RegisterViewModel` je k dispozici.

![obrázek](intro/_static/intellemail.png)

IntelliSense zobrazí seznam vlastností a metod, které jsou k dispozici pro model na stránce. Bohaté prostředí IntelliSense pomáhá vyberte třídu CSS:

![obrázek](intro/_static/iclass.png)

![obrázek](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a>Pomocníci značky ve srovnání s pomocné objekty HTML

Pomocníci značky přiřadit elementů HTML v zobrazení syntaxe Razor, zatímco [pomocné rutiny HTML](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) jsou vyvolány jako metody spolu s HTML v zobrazení syntaxe Razor. Vezměte v úvahu následující kód Razor, který vytvoří popisek HTML pomocí třídy CSS "Popisek":

```cshtml
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

Na (`@`) symbol informuje Razor začátek kódu. Následující dva parametry ("FirstName" a "křestní jméno:") jsou řetězce, takže [IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense) nemůže pomoct. Poslední argument:

```cshtml
new {@class="caption"}
```

Slouží k reprezentaci atributy anonymní objekt. Protože <strong>– třída</strong> je vyhrazené klíčové slovo v jazyce C#, můžete použít `@` symbol, který má vynutit C# interpretovat "@class=" jako symbol (název vlastnosti). Do front-endu návrháře (někdo obeznámeni s HTML + CSS + JavaScript a další technologie klienta, ale není obeznámeni s C# a Razor), je většina řádku cizí. Celý řádek musí být vytvořené bez pomoci z IntelliSense.

Pomocí `LabelTagHelper`, lze zapsat značku stejné jako:

![obrázek](intro/_static/label2.png)

S verzí značky Helper, jakmile zadáte `<l` v editoru Visual Studio IntelliSense zobrazí odpovídající prvky:

![obrázek](intro/_static/label.png)

IntelliSense umožňuje zapsat celý řádek. `LabelTagHelper` Také výchozí hodnoty nastavení obsah `asp-for` atribut hodnotu ("FirstName") "Jméno"; Převede ve formátu camelCase vlastnosti věty skládá z názvu vlastnosti s místem, kde dochází k každý nový velké písmeno. V následující kód:

![obrázek](intro/_static/label2.png)

generuje:

```cshtml
<label class="caption" for="FirstName">First Name</label>
```

Ve formátu camelCase-k obsahu použita větu nepoužívá, pokud přidáte obsah tak, aby `<label>`. Příklad:

![obrázek](intro/_static/1stName.png)

generuje:

```cshtml
<label class="caption" for="FirstName">Name First</label>
```

Následující obrázek kód ukazuje části formuláře *Views/Account/Register.cshtml* zobrazení syntaxe Razor generované ze starší verze šablony 4.5.x MVC ASP.NET, kde je zahrnutá v sadě Visual Studio 2015.

![obrázek](intro/_static/regCS.png)

Editoru Visual Studio zobrazí kód C# s šedé pozadí. Například `AntiForgeryToken` pomocné rutiny HTML:

```cshtml
@Html.AntiForgeryToken()
```

Zobrazí se šedé pozadí se. Většinu kódu značek v zobrazení registrace je C#. Porovnání, ekvivalentní přístup pomocí pomocné rutiny značky:

![obrázek](intro/_static/regTH.png)

Kód je mnohem čisticí a snadněji číst, upravit a udržovat než přístup pomocné rutiny HTML. Kód jazyka C# se snižuje na minimum, který potřebujete vědět o serveru. Editoru Visual Studio zobrazí značek cílem pomocné rutiny značky rozlišovací písmeny.

Vezměte v úvahu *e-mailu* skupiny:

[!code-csharp[](intro/sample/Register.cshtml?range=12-18)]

Jednotlivým atributům "asp-" má hodnotu "E-mailu", ale "E-mailu" není řetězec. V tomto kontextu je "E-mailu" C# výraz vlastnost modelu pro `RegisterViewModel`.

Editoru Visual Studio umožňuje zapsat **všechny** z kódu v metodě značky pomocná registrace formuláře, zatímco Visual Studio poskytuje žádná nápověda pro většinu kódu v metodě pomocné rutiny HTML. [Podporu technologie IntelliSense pro značku Pomocníci](#intellisense-support-for-tag-helpers) obsahuje podrobnosti o práci s Pomocníci značky v editoru Visual Studio.

## <a name="tag-helpers-compared-to-web-server-controls"></a>Pomocníci značky ve srovnání s ovládací prvky webového serveru

* Pomocníci značky nevlastníte na element, který jste spojené s; jednoduše účastnit při vykreslování element a obsahu. ASP.NET [ovládací prvky webového serveru](https://msdn.microsoft.com/library/7698y1f0.aspx) deklarovat a vyvolat na stránce.

* [Ovládací prvky webového serveru](https://msdn.microsoft.com/library/zsyt68f1.aspx) mít netriviální životního cyklu, které mohou ztížit vývoji a ladění.

* Ovládací prvky webového serveru můžete přidat další funkce elementů modelu DOM (Document Object) klient pomocí ovládacího prvku klienta. Pomocníci značky mít žádné modelu DOM.

* Ovládací prvky webového serveru zahrnují automatickou detekci prohlížeče. Pomocníci značky nemají žádné informace o prohlížeči.

* Více Pomocníci značka může fungovat u stejného elementu (v tématu [zabraňující pomocná značky konflikty](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/authoring#avoiding-tag-helper-conflicts) ) při obvykle nelze vytvořit ovládací prvky webového serveru.

* Značka pomocné rutiny můžete upravit značku a obsah prvků HTML, kterou jste rozsah, ale nemáte upravit přímo cokoliv jiného na stránce. Ovládací prvky webového serveru mají méně konkrétní obor a můžou provádět akce, které ovlivňují dalších částí vaší stránky; povolení nezamýšleným vedlejší účinky.

* Ovládací prvky webového serveru pomocí převaděče typu převod řetězců na objekty. S značky podpůrných rutin pracujete nativně v jazyce C#, takže nemusíte typ – převod.

* Ovládací prvky webového serveru pomocí [System.ComponentModel](https://docs.microsoft.com/dotnet/api/system.componentmodel) k implementaci chování za běhu a návrhu součásti a ovládacích prvků. `System.ComponentModel` obsahuje základní třídy a rozhraní pro implementace atributů, převaděčů typů vazeb na zdroje dat a licencování součásti. Kontrastu, do pomocné rutiny značky, které obvykle odvozena od `TagHelper`a `TagHelper` základní třída zpřístupňuje pouze dvě metody, `Process` a `ProcessAsync`.

## <a name="customizing-the-tag-helper-element-font"></a>Přizpůsobení písma elementu značky pomocné rutiny

Můžete přizpůsobit písma a zabarvení z **nástroje** > **možnosti** > **prostředí** > **písem a barvy**:

![obrázek](intro/_static/fontoptions2.png)

## <a name="additional-resources"></a>Další zdroje

* [Autor značky pomocné rutiny](xref:mvc/views/tag-helpers/authoring)
* [Práce s formuláři ](xref:mvc/views/working-with-forms)
* [TagHelperSamples na Githubu](https://github.com/dpaquette/TagHelperSamples) obsahuje ukázky značky Pomocníka pro práci s [Bootstrap](http://getbootstrap.com/).
