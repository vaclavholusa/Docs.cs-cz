---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Vytváření MVC 3 aplikace s Razor a Nerušivý JavaScript | Microsoft Docs
author: microsoft
description: Seznam uživatelů ukázkovou webovou aplikaci ukazuje, jak jednoduché je vytvoření aplikace ASP.NET MVC 3 pomocí zobrazovací modul Razor. Aplikace s ukázka...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/01/2010
ms.topic: article
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: 9b273f6827cad2078b581d6da7b127198dfddaa5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a>Vytváření MVC 3 aplikace s Razor a Nerušivý JavaScript
====================
podle [Microsoft](https://github.com/microsoft)

> Seznam uživatelů ukázkovou webovou aplikaci ukazuje, jak jednoduché je vytvoření aplikace ASP.NET MVC 3 pomocí zobrazovací modul Razor. Ukázkovou aplikaci ukazuje, jak používat nový zobrazovací modul Razor s architekturou ASP.NET MVC verze 3 a Visual Studio 2010 k vytvoření fiktivních webu seznam uživatelů, která zahrnuje funkce, jako je vytváření, zobrazení, úpravy a odstraňování uživatelů.
> 
> Tento kurz popisuje kroky, které byly provedeny k vytvoření ukázkové aplikace ASP.NET MVC 3 seznam uživatelů. Projekt sady Visual Studio se zdrojovým kódem C# a VB je k dispozici v tomto tématu: [Stáhnout](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114). Pokud máte dotazy týkající se tohoto kurzu, prosím, aby post [mvcforum](https://forums.asp.net/1146.aspx).


## <a name="overview"></a>Přehled

Aplikace, kterou budete sestavení je web seznamu jednoduché uživatele. Uživatele můžete zadat, zobrazit a aktualizovat informace o uživateli.

![Ukázka lokality](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

Stáhnete dokončený projekt jazyka Visual Basic a C# [zde](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).

## <a name="creating-the-web-application"></a>Vytváření webové aplikace

Pokud chcete spustit tento kurz, otevřete Visual Studio 2010 a vytvořte nový projekt pomocí *webové aplikace ASP.NET MVC 3* šablony. Název aplikace &quot;Mvc3Razor&quot;.

[![Nový projekt MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)

V **nový ASP.NET MVC 3 projekt** dialogovém okně, vyberte **Internetové aplikace**vyberte zobrazovací modul Razor a pak klikněte na tlačítko **OK**.

![Dialogové okno Nový projekt ASP.NET MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

V tomto kurzu nebudete používat poskytovatele členství prostředí ASP.NET, takže můžete odstranit všechny soubory přidružené k přihlášení a členství. V **Průzkumníku**, odeberte tyto soubory a adresáře:

- *Controllers\AccountController*
- *Models\AccountModels*
- *Views\Shared\\_LogOnPartial*
- *Views\Account* (a všechny soubory v tomto adresáři)

![Soln Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

Upravit  <em>\_Layout.cshtml</em> souboru a nahraďte kód uvnitř `<div>` element s názvem `logindisplay` se zprávou <em>&quot;</em>zakázáno přihlášení&quot;. Následující příklad ukazuje kód nové:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a>Přidání modelu

V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *modely* složky, vyberte **přidat**a potom klikněte na **– třída**.

![Nový uživatel Mdl – třída](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

Název třídy `UserModel`. Nahraďte obsah *UserModel* soubor s následujícím kódem:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

`UserModel` Třída reprezentuje uživatele. Každý člen třídy je opatřen poznámkou [požadované](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) atribut z [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) oboru názvů. Atributy v [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) obor názvů ověřování automatické a server straně klienta pro webové aplikace.

Otevřete `HomeController` třídu a přidejte `using` direktivy, takže může získat přístup `UserModel` a `Users` třídy:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

Právě po `HomeController` deklarace, přidejte následující komentář a odkaz na `Users` třídy:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

`Users` Třída je jednodušší, v paměti datové úložiště, které budete používat v tomto kurzu. V reálné aplikaci byste použili databázi k uložit informace o uživateli. Několik prvních řádků `HomeController` souboru jsou uvedeny v následujícím příkladu:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

Sestavení aplikace tak, aby uživatelského modelu se bude k dispozici pro Průvodce generováním uživatelského rozhraní v dalším kroku.

## <a name="creating-the-default-view"></a>Vytváření výchozí zobrazení

Dalším krokem je přidat metodu akce a zobrazení uživatelů.

Odstraňte existující *Views\Home\Index* souboru. Vytvoří nový *Index* soubor k zobrazení uživatelů.

V `HomeController` třídy, nahraďte obsah `Index` metoda následujícím kódem:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

Klepněte pravým tlačítkem myši `Index` metoda a pak klikněte na tlačítko **přidat zobrazení**.

![Přidání zobrazení](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

Vyberte **vytvořit zobrazení silného typu** možnost. Pro **zobrazit třída dat**, vyberte **Mvc3Razor.Models.UserModel**. (Pokud nevidíte **Mvc3Razor.Models.UserModel** v **zobrazit třída dat** pole, které potřebujete k vytvoření projektu.) Ujistěte se, že bude zobrazovací modul je nastavená na **Razor**. Nastavit **zobrazit obsah** k **seznamu** a pak klikněte na **přidat**.

![Přidání zobrazení indexu](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

Nové zobrazení automaticky scaffolds uživatelská data, která je předána `Index` zobrazení. Zkontrolujte nově vygenerovaný *Views\Home\Index* souboru. **Vytvořit nový**, **upravit**, **podrobnosti**, a **odstranit** nefungují odkazy, ale zbývající části stránky je funkční. Spuštění stránky. Zobrazí seznam uživatelů.

![Index stránky](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

Otevřete *Index.cshtml* souboru a nahraďte `ActionLink` kód pro **upravit**, **podrobnosti**, a **odstranit** následujícím kódem :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

Uživatelské jméno se používá jako ID najít vybraný záznam v **upravit**, **podrobnosti**, a **odstranit** odkazy.

## <a name="creating-the-details-view"></a>Vytváření zobrazení podrobností

Dalším krokem je přidání `Details` metody akce a zobrazení, aby bylo možné zobrazit podrobné informace o uživateli.

![Podrobnosti](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

Přidejte následující `Details` metoda domácí řadiče:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

Klepněte pravým tlačítkem myši `Details` metoda a potom vyberte <strong>přidat zobrazení</strong>. Ověřte, zda <strong>zobrazit třída dat</strong> obsahuje pole <strong>Mvc3Razor.Models.UserModel</strong><em>.</em> Nastavit <strong>zobrazit obsah</strong> k <strong>podrobnosti</strong> a pak klikněte na <strong>přidat</strong>.

![Přidání zobrazení podrobností](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

Spusťte aplikaci a vyberte podrobnosti odkaz. Automatické generování uživatelského rozhraní zobrazuje každou vlastnost v modelu.

![Podrobnosti](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a>Vytvoření zobrazení pro úpravy

Přidejte následující `Edit` metoda domácí řadiče.

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

Přidání zobrazení jako v předchozích krocích, ale nastavit **zobrazit obsah** k **upravit**.

![Přidání zobrazení upravit](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

Spusťte aplikaci a upravit první a poslední název jednoho uživatele. Pokud jste porušují `DataAnnotation` omezení, které se aplikovaly `UserModel` třída, při odeslání formuláře, zobrazí se chyby ověření, které jsou vytvářeny v serverovém kódu. Například, pokud změníte jméno &quot;Ann&quot; k &quot;A&quot;, že při odeslání formuláře, na formuláři se zobrazí následující chyba:

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

V tomto kurzu jste považuje uživatelské jméno jako primární klíč. Proto nelze změnit vlastnost název uživatele. V *Edit.cshtml* souboru, hned za `Html.BeginForm` příkazu nastavte uživatelské jméno jako skryté políčko. To způsobí, že vlastnost, která má být předán v modelu. Následující fragment kódu ukazuje umístění `Hidden` příkaz:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

Nahraďte `TextBoxFor` a `ValidationMessageFor` značek pro uživatelské jméno, `DisplayFor` volání. `DisplayFor` Metoda zobrazí vlastnost jako element jen pro čtení. Následující příklad ukazuje kód dokončené. Původní `TextBoxFor` a `ValidationMessageFor` volání jsou vloženy do komentáře syntaxe Razor začít komentáře a end komentář znaků (`@* *@`)

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a>Povolení ověřování na straně klienta

Pokud chcete povolit ověřování na straně klienta v architektuře ASP.NET MVC 3, je nutné nastavit dvěma příznaky a musí obsahovat tři soubory JavaScript.

Otevřít aplikaci *Web.config* souboru. Ověřte `that ClientValidationEnabled` a `UnobtrusiveJavaScriptEnabled` jsou nastaveny na hodnotu true v nastavení aplikace. Následující fragment z kořenového adresáře *Web.config* souboru zobrazuje správné nastavení:

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

Nastavení `UnobtrusiveJavaScriptEnabled` na hodnotu true, umožňuje nerušivý Ajax a ověření nerušivého klienta. Použijete-li nerušivý ověřování, jsou pravidla ověřování převedena na atributy HTML5. Názvy atributů jazyka HTML5 se může skládat jenom malá písmena, čísla a pomlčky.

Nastavení `ClientValidationEnabled` k ověřování na straně klienta true povoluje. Nastavením těchto klíčů v aplikaci *Web.config* souboru, povolení ověření klienta a nerušivý JavaScript pro celou aplikaci. Můžete také povolit nebo zakázat tato nastavení v jednotlivých zobrazeních nebo metody kontroleru pomocí následujícího kódu:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

Musíte taky zahrnout několik souborů JavaScript ve vykresleném zobrazení. Snadný způsob, jak zahrnout všechna zobrazení jazyka JavaScript je pro jejich přidávání k *Views\Shared\\_Layout.cshtml* souboru. Nahraďte `<head>` element  *\_Layout.cshtml* soubor s následujícím kódem:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

První dva skripty jQuery jsou hostované pomocí Microsoft Ajax Content Delivery Network (CDN). Využitím Microsoft Ajax CDN, může výrazně zlepšit výkon první podle aplikací.

Spusťte aplikaci a klikněte na odkaz pro úpravy. Zobrazení zdrojového kódu stránky v prohlížeči. Zdroj prohlížeče ukazuje počet atributů formuláře `data-val` (pro ověření dat). Pokud je povoleno ověření klienta a nerušivý JavaScript, obsahovat vstupní pole s pravidlem ověření klienta `data-val="true"` atributů ověření nerušivého klienta aktivovat. Například `City` pole v modelu byla označených pomocí [požadované](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) atributů, což vede k HTML vidět v následujícím příkladu:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

Pro každé pravidlo ověření klienta se přidá atribut, který má formulář `data-val-rulename="message"`. Pomocí `City` pole příkladu výše, vyžaduje ověření klienta pravidlo generuje `data-val-required` atribut a zpráva &quot;The města pole je povinné&quot;. Spusťte aplikaci a úpravě jeden z uživatelů, zrušte `City` pole. Kartě mimo pole zobrazí chybovou zprávu ověření na straně klienta.

![Požadované města](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

Podobně, pro každý parametr v pravidle ověření klienta, přidání atributu má formuláře `data-val-rulename-paramname=paramvalue`. Například `FirstName` vlastnost je opatřen poznámkou [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) atribut a určuje minimální délku 3 a maximální délku 8. Pravidlo ověření dat s názvem `length` má název parametru `max` a hodnota parametru 8. Následující příklad zobrazuje HTML, který se vygeneruje pro `FirstName` pole při úpravách jeden z uživatelů:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

Další informace o ověření nerušivého klienta, naleznete v příspěvku [Nerušivý ověření klienta v architektuře ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) v blogu Brada Wilsona.

> [!NOTE]
> V ASP.NET MVC 3 Beta budete muset někdy formulář odeslat, aby bylo možné spustit ověřování na straně klienta. To může být změněn pro finální verzi.


## <a name="creating-the-create-view"></a>Vytvoření zobrazení pro vytváření

Dalším krokem je přidání `Create` metody akce a zobrazení, aby bylo možné zajistit, aby uživatel k vytvoření nového uživatele. Přidejte následující `Create` metoda domácí řadiče:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

Přidání zobrazení jako v předchozích krocích, ale nastavit **zobrazit obsah** k **vytvořit**.

![Vytvoření zobrazení](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

Spusťte aplikaci, vyberte **vytvořit** propojit, a přidání nového uživatele. `Create` Metoda automaticky využívá výhod ověření na straně klienta a na straně serveru. Zkuste zadat uživatelské jméno, které obsahuje prázdné znaky, jako například &quot;Ben X&quot;. Když kartě mimo pole uživatelského jména, chyba ověřování na straně klienta (`White space is not allowed`) se zobrazí.

## <a name="add-the-delete-method"></a>Přidejte metodu Delete

K dokončení tohoto kurzu, přidejte následující `Delete` metoda domácí řadiče:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

Přidat `Delete` zobrazení jako v předchozích krocích nastavení **zobrazit obsah** k **odstranit**.

![Odstranit zobrazení](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

Nyní máte jednoduchý, ale plně funkční aplikaci ASP.NET MVC 3 s ověřování.
