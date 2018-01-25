---
title: "Vytváření Pomocníci značky v ASP.NET Core"
author: rick-anderson
description: "Naučte se vytvářet značky pomocné rutiny v ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 01/19/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/authoring
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a1f1b2c2e60a1337c15f019185c764d0a9ada1b5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="author-tag-helpers-in-aspnet-core-a-walkthrough-with-samples"></a>Autor značky pomocné rutiny v ASP.NET Core, návod s ukázky

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="get-started-with-tag-helpers"></a>Začínáme s značky pomocné rutiny

Tento kurz obsahuje úvod do pomocné rutiny programování značky. [Úvod do pomocné rutiny značky](intro.md) popisuje výhody, které poskytují pomocné rutiny značky.

Pomocné rutiny značku je žádné třídu, která implementuje `ITagHelper` rozhraní. Ale při vytváření značky pomocné rutiny, obecně odvozujete od `TagHelper`, tak dává vám přístup k provádění `Process` metoda.

1. Vytvořit nový projekt ASP.NET Core s názvem **AuthoringTagHelpers**. Ověřování nebudete potřebovat pro tento projekt.

2. Vytvořte složku pro uložení Pomocníci značky názvem *TagHelpers*. *TagHelpers* složka je *není* požadováno, ale je možné logicky konvence. Nyní můžeme začít zápis několik jednoduchých značek pomocné rutiny.

## <a name="a-minimal-tag-helper"></a>Minimální pomocné rutiny značky

V této části napíšete značky pomocné rutiny, která aktualizuje značku e-mailu. Příklad:

```html
<email>Support</email>
   ```

Server bude používat naše pomocná značky e-mailu k převedení tohoto kódu do následující:

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

To znamená značku ukotvení pak tento odkaz e-mailu. Můžete to udělat, pokud jsou psaní modul blog a potřebovat k odeslání e-mailu pro marketing, podpory a další kontakty, všechny ke stejné doméně.

1.  Přidejte následující `EmailTagHelper` třídy k *TagHelpers* složky.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]
    
    **Poznámky:**
    
    * Pomocníci značky použít zásady vytváření názvů zacílený elementy název kořenové třídy (minus *TagHelper* část názvu třídy). V tomto příkladu název kořenového **e-mailu**TagHelper je *e-mailu*, proto `<email>` budou cílem značky. Tyto zásady vytváření názvů by měla fungovat pro většinu značky pomocné rutiny, později na zobrazí postup přepsat.
    
    * `EmailTagHelper` Třída odvozená z `TagHelper`. `TagHelper` Třída poskytuje metody a vlastnosti pro zápis značky pomocné rutiny.
    
    * Přepsané `Process` ovládací prvky metoda helper značky jaké jsou při spuštění. `TagHelper` Třída rovněž poskytuje asynchronní verzi (`ProcessAsync`) se stejnými parametry.
    
    * Kontext parametru pro `Process` (a `ProcessAsync`) obsahuje informace související s prováděním aktuální značky HTML.
    
    * Výstupní parametr k `Process` (a `ProcessAsync`) obsahuje stavová prvek HTML reprezentativní sloužící ke generování služby značky HTML a obsah původního zdroje.
    
    * Naše název třídy má příponu **TagHelper**, což je *není* požadováno, ale považuje za osvědčený postup konvence. Může deklarovat třídu jako:
    
    ```csharp
    public class Email : TagHelper
    ```

2.  Chcete-li `EmailTagHelper` třídy k dispozici pro všechny naše zobrazení syntaxe Razor, přidejte `addTagHelper` direktivy k *Views/_ViewImports.cshtml* souboru:[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]
    
    Výše uvedený kód používá syntaxi zástupný znak můžete určit že všechny značky nápovědy v našem sestavení bude k dispozici. První řetězec po `@addTagHelper` určuje značky pomocná načíst (použijte "*" pro všechny značky pomocníky), a druhý řetězec "AuthoringTagHelpers" Určuje sestavení Pomocník značky. Všimněte si také, že na druhém řádku přináší v ASP.NET MVC základní pomocníky značky pomocí syntaxe zástupný znak (tyto pomocné rutiny, které jsou popsané v [Úvod do pomocné rutiny značky](intro.md).) Je `@addTagHelper` direktiva, která zpřístupňuje značky pomocné rutiny do zobrazení Razor. Alternativně můžete zadat plně kvalifikovaný název (FQN) značky Helper, jak je uvedeno níže:
    
```csharp
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```
    
<!--
the following snippet uses TagHelpers3 and should use TagHelpers (not the 3)
    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
-->
    
Přidání značka pomocné rutiny zobrazení pomocí FQN, je nejprve přidat FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) a potom název sestavení (*AuthoringTagHelpers*). Většina vývojářů bude radši chtěli použít zástupný znak syntaxe. [Úvod do pomocné rutiny značky](intro.md) přejde do podrobností na přidání, odebrání, hierarchie a zástupný znak syntaxe pomocná značek.
    
3.  Aktualizovat kód v *Views/Home/Contact.cshtml* souboru se tyto změny:

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

4.  Spusťte aplikaci a pomocí oblíbeném prohlížeči zobrazíte zdrojový kód HTML a ověřte značek e-mailu jsou nahrazeny značka ukotvení (například `<a>Support</a>`). *Podpora* a *Marketing* se vykresluje jako odkazy, ale nemají `href` atribut tak, aby byly funkční. To jsme budete opravíme v další části.

Poznámka: Jako značky HTML a atributy, značky, názvy tříd a atributů v Razor a C# nejsou malá a velká písmena.

## <a name="setattribute-and-setcontent"></a>SetAttribute a SetContent

V této části budete aktualizujeme `EmailTagHelper` tak, aby se vytvoří značku ukotvení platné e-mailu. Aktualizujeme budete ji tak, aby informace ze zobrazení syntaxe Razor (ve formě `mail-to` atribut) a použít jej v generování ukotvení.

Aktualizace `EmailTagHelper` třídy následujícím kódem:

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

**Poznámky:**

* Jsou použita Pascal třídy a vlastnosti názvy pro značku Pomocníci přeložit na jejich [nižší případ kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101). Proto používat `MailTo` atributů, které budete používat `<email mail-to="value"/>` ekvivalentní.

* Poslední řádek nastaví dokončené obsah naše pomocníka minimálně funkční značky.

* Zvýrazněný řádek ukazuje syntaxi pro přidání atributy:

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

Tento přístup funguje pro atribut "href", dokud aktuálně neexistuje v kolekci atributů. Můžete také `output.Attributes.Add` metody Přidat pomocný atribut příznaku na konec kolekce atributů značky.

1.  Aktualizovat kód v *Views/Home/Contact.cshtml* souboru se tyto změny:[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]

2.  Spusťte aplikaci a ověřte, že vygeneruje správné odkazy.
    
    > [!NOTE]
    >Pokud byste chtěli zápis, e-mailu značky samoobslužné zavření (`<email mail-to="Rick" />`), by také být samouzavírací finální výstup. Chcete-li povolit možnost zapisovat značky s počáteční značky (`<email mail-to="Rick">`) musí uspořádání třídy následujícím kódem:
    >
    > [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]
    
    S samouzavírací značky pomocníka e-mailu, bude výstup `<a href="mailto:Rick@contoso.com" />`. Samouzavírací značky ukotvení nejsou platné HTML, proto byste neměli chtít vytvořit, ale můžete chtít vytvořit značku pomocné rutiny, která je samouzavírací. Pomocníci značky nastavit typ `TagMode` vlastnost po přečtení značku.
    
### <a name="processasync"></a>ProcessAsync

V této části jsme budete zápisu Pomocník asynchronní e-mailu.

1.  Nahraďte `EmailTagHelper` třídy následujícím kódem:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

    **Poznámky:**

    * Tato verze používá asynchronní `ProcessAsync` metoda. Asynchronní `GetChildContentAsync` vrátí `Task` obsahující `TagHelperContent`.

    * Použití `output` parametr načíst obsah elementu HTML.

2.  Proveďte následující změny k *Views/Home/Contact.cshtml* , značka pomocné rutiny můžete získat e-mailu, cílový soubor.

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

3.  Spusťte aplikaci a ověřte, že se generuje odkazy platnou e-mailovou.

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a>RemoveAll, PreContent.SetHtmlContent a PostContent.SetHtmlContent

1.  Přidejte následující `BoldTagHelper` třídy k *TagHelpers* složky.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

    **Poznámky:**
    
    * `[HtmlTargetElement]` Atribut předává atribut parametr, který určuje, že libovolný prvek HTML, která obsahuje atribut HTML s názvem "bold" bude odpovídat, a `Process` přepsání metody ve třídě, poběží. Ve výběru `Process` metoda odebere atribut "bold" a obklopuje kód obsahující s `<strong></strong>`.
    
    * Vzhledem k tomu, že nechcete nahradit stávající značky obsahu, musíte napsat otevření `<strong>` značku s `PreContent.SetHtmlContent` metoda a zavření `</strong>` značku s `PostContent.SetHtmlContent` metoda.
    
2.  Změnit *About.cshtml* zobrazení tak, aby obsahovala `bold` hodnota atributu. Dokončený kód je uveden níže.

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

3.  Spusťte aplikaci. Oblíbeném prohlížeči můžete použít ke kontrole zdroji a ověřit kód.

    `[HtmlTargetElement]` Výše uvedený atribut pouze cílem jazyka HTML, který poskytuje název atributu z "bold". `<bold>` Element nebyla upravena podle značky pomocné rutiny.

4. Komentář `[HtmlTargetElement]` atribut řádku a použije výchozí cílení na `<bold>` značky, který je značka jazyka HTML ve formátu `<bold>`. Pamatujte si, že výchozí zásady vytváření názvů bude shodovat s názvem třídy **Bold**TagHelper k `<bold>` značky.

5. Spusťte aplikaci a ověřte, zda `<bold>` značky zpracovává pomocná značky.

Architekturu třídu s více `[HtmlTargetElement]` atributy výsledky v logické nebo cíle. Například pomocí kódu níže, tučné značky nebo tučné atribut bude odpovídat.

[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

Po přidání více atributů stejný příkaz, modul runtime je považuje za logickým operátorem a. Například následující kód HTML element musí mít název "bold" s atributem s názvem "bold" (`<bold bold />`) tak, aby odpovídaly.

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

Můžete také `[HtmlTargetElement]` Chcete-li změnit název cílové elementu. Například pokud jste chtěli `BoldTagHelper` cíl `<MyBold>` značky, byste použili následující atribut:

```csharp
[HtmlTargetElement("MyBold")]
   ```

## <a name="pass-a-model-to-a-tag-helper"></a>Model předat značky pomocné rutiny

1.  Přidat *modely* složky.

2.  Přidejte následující `WebsiteContext` třídy k *modely* složky:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

3.  Přidejte následující `WebsiteInformationTagHelper` třídy k *TagHelpers* složky.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]
    
    **Poznámky:**
    
    * Jak je uvedeno nahoře, značka pomocné překládá názvy tříd použita Pascal C# a vlastnosti pro značku pomocné rutiny do [nižší případ kebab](http://wiki.c2.com/?KebabCase). Proto používat `WebsiteInformationTagHelper` v Razor, napíšete `<website-information />`.
    
    * Cílový element s nejsou explicitně identifikace `[HtmlTargetElement]` atribut, tak výchozí `website-information` budou cílem. Pokud jste provedli následující atribut (Poznámka: není kebab případ ale odpovídá názvu třídy):
    
    ```csharp
    [HtmlTargetElement("WebsiteInformation")]
    ```
    
    Nižší případu značky kebab `<website-information />` nebude shodovat. Pokud chcete použít `[HtmlTargetElement]` atribut byste použili kebab případu, jak je uvedeno níže:
    
    ```csharp
    [HtmlTargetElement("Website-Information")]
    ```
    
    * Prvky, které jsou samouzavírací nemají žádný obsah. V tomto příkladu kódu Razor používat samouzavírací značky, ale bude vytvářet pomocná značky [části](http://www.w3.org/TR/html5/sections.html#the-section-element) element (který není samouzavírací a jsou zápisu obsahu uvnitř `section` element). Proto je nutné nastavit `TagMode` k `StartTagAndEndTag` k zápisu výstupu. Alternativně můžete Zakomentovat řádek nastavení `TagMode` a zapsat značku s ukončovací značku. (Příklad značek je zadáno později v tomto kurzu).
    
    * `$` (Dolaru) na následujícím řádku používá [interpolované řetězce](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):
    
    ```cshtml
    $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
    ```

4.  Přidejte následující kód do *About.cshtml* zobrazení. Zvýrazněná značka zobrazí informace o tomto webu.
    
    [!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-)]
    
    >[!NOTE]
    > V kódu Razor, vidíte níže:
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]
    > 
    >Zná Razor `info` atribut je třída, není řetězec, a vy chcete napsat kód C#. Zasílány žádné pomocný atribut příznaku jiné než řetězec bez `@` znak.
    
5.  Spusťte aplikaci a přejděte do zobrazení o chcete zobrazit informace o webovém serveru.

    >[!NOTE]
    >Můžete použít následující kód s uzavírací značku a odebrat řádek s `TagMode.StartTagAndEndTag` v pomocná značky:
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]

## <a name="condition-tag-helper"></a>Podmínka vyhodnocena jako značka pomocné rutiny

Pomocník značky podmínku vykreslí výstup, když uplyne hodnotu true.

1.  Přidejte následující `ConditionTagHelper` třídy k *TagHelpers* složky.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

2.  Nahraďte obsah *Views/Home/Index.cshtml* soubor s následující kód:

    ```cshtml
    @using AuthoringTagHelpers.Models
    @model WebsiteContext
    
    @{
        ViewData["Title"] = "Home Page";
    }
    
    <div>
        <h3>Information about our website (outdated):</h3>
        <website-information info=@Model />
        <div condition="@Model.Approved">
            <p>
                This website has <strong surround="em"> @Model.Approved </strong> been approved yet.
                Visit www.contoso.com for more information.
            </p>
        </div>
    </div>
    ```
    
3.  Nahraďte `Index` metoda v `Home` řadiče následujícím kódem:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

4.  Spusťte aplikaci a přejděte na domovskou stránku. Kód v podmínku `div` nebude vykreslen. Připojit řetězec dotazu `?approved=true` na adresu URL (například `http://localhost:1235/Home/Index?approved=true`). `approved`je nastavena na hodnotu true a podmíněného zobrazí značek.

>[!NOTE]
>Použití [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operátor zadat atribut, který se cílové místo zadání řetězec, jako jste to udělali s pomocnou rutinou tučné značky:
>
>[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
>
>[Nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operátor bude chránit kód by se někdy být rozdělili (chceme změnit název, který má `RedCondition`).

### <a name="avoid-tag-helper-conflicts"></a>Nedocházelo ke konfliktům značky pomocné rutiny

V této části napíšete pár automatické propojení Pomocníci značky. První nahradí značek, který obsahuje adresu URL začínající HTTP na HTML anchor značky obsahující stejnou adresu URL (a proto je odkaz na adresu URL). Druhý bude totéž proveďte pro adresu URL začínající WWW.

Protože tyto dvě pomocné rutiny jsou úzce související a je může v budoucnu Refaktorovat, jsme budete zachovat ve stejném souboru.

1.  Přidejte následující `AutoLinkerHttpTagHelper` třídy k *TagHelpers* složky.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

    >[!NOTE]
    >`AutoLinkerHttpTagHelper` Třídy cíle `p` elementy a používá [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) vytvořit ukotvení.

2.  Přidejte následující kód do konce *Views/Home/Contact.cshtml* souboru:

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

3.  Spusťte aplikaci a ověřte pomocná značky správně vykreslí ukotvení.

4.  Aktualizace `AutoLinker` třída zahrnout `AutoLinkerWwwTagHelper` který převede www text značku ukotvení, který také obsahuje původní text www. Aktualizovaný kódu je označený níže:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

5.  Spusťte aplikaci. Všimněte si www text je vykreslen jako odkaz, ale nemá HTTP text. Když vložíte přerušení v obou třídy, se zobrazí pomocná třída značky HTTP nejprve spustí. Problém je, že pomocná výstup značky se uloží do mezipaměti a při spuštění Pomocníka značky WWW přepíše uložené v mezipaměti výstup z pomocníka značky HTTP. Později v tomto kurzu vidíte postup řízení značky Pomocníci spustit v pořadí. Kód jsme budete oprava s následujícími službami:

    [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

    >[!NOTE]
    >V prvním vydání značky pomocníků automatické propojení vy máte obsah cílového následujícím kódem:
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
    >
    >To znamená, že zavoláte `GetChildContentAsync` pomocí `TagHelperOutput` předaný do `ProcessAsync` metoda. Jak je uvedeno dříve, protože výstup se uloží do mezipaměti, poslední označit pomocná rutina pro spuštění služby wins. Vyřešeny, že problém s následujícím kódem:
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
    >
    >Výše uvedený kód kontroluje, zda změnil obsah, a pokud ano, získá obsah z výstupní vyrovnávací paměť.

6.  Spusťte aplikaci a ověřte, že dva odkazy fungují podle očekávání. Když se může objevit, že je naše automaticky linkeru značky pomocná správnosti a úplnosti, má jemně problém. Pokud se spustí Pomocník značky WWW první, nebudou webové odkazy správné. Aktualizujte kód tak, že přidáte `Order` přetížení řídit pořadí spuštěnou ve značce. `Order` Vlastnost určuje pořadí zpracování relativně k další Pomocníci značky cílení na stejného elementu. Výchozí hodnota pořadí je nulová a instancí s nižšími hodnotami jsou nejprve spustit.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]
    
    Výše uvedený kód bude zaručit, pomocná značky HTTP spuštěná před pomocná značky WWW. Změna `Order` k `MaxValue` a ověřte, zda kód vygeneruje pro značku WWW je nesprávný.

## <a name="inspect-and-retrieve-child-content"></a>Zkontrolujte a načítat obsah podřízené

Pomocníci značky poskytují několik vlastností, které k načtení obsahu.

-  Výsledek `GetChildContentAsync` můžete připojit k `output.Content`.
-  Si můžete prohlédnout výsledek `GetChildContentAsync` s `GetContent`.
-  Pokud změníte `output.Content`, těle TagHelper nebude proveden nebo vykresluje Pokud zavoláte `GetChildContentAsync` jako naše ukázka linkeru automaticky:

[!code-csharp[Main](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

-  Více volá, aby se `GetChildContentAsync` vrací stejnou hodnotu a nebude znovu spustit `TagHelper` body Pokud předáte hodnotu false parametr označující nepoužívat výsledky uložené v mezipaměti.
