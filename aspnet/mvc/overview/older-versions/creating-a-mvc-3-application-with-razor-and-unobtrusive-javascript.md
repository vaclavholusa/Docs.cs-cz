---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Vytváření MVC 3 aplikace s Razor a Nerušivý JavaScript | Dokumentace Microsoftu
author: microsoft
description: Seznam uživatelů ukázkovou webovou aplikaci ukazuje, jak snadné je vytvoření aplikace ASP.NET MVC 3 pomocí zobrazovací modul Razor. Ukázková aplikace s...
ms.author: aspnetcontent
ms.date: 11/01/2010
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: 136c87cba70525da53c1f74576c50c12f8759539
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840460"
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a>Vytváření MVC 3 aplikace s Razor a Nerušivý JavaScript
====================
podle [Microsoft](https://github.com/microsoft)

> Seznam uživatelů ukázkovou webovou aplikaci ukazuje, jak snadné je vytvoření aplikace ASP.NET MVC 3 pomocí zobrazovací modul Razor. Ukázková aplikace ukazuje, jak nový zobrazovací modul Razor s architekturou ASP.NET MVC verze 3 a Visual Studio 2010 použít k vytvoření fiktivní web seznamu uživatelů, který obsahuje funkce, jako je vytváření, zobrazení, úpravy a odstraňování uživatelů.
> 
> Tento kurz popisuje kroky, které byly provedeny, aby bylo možné vytvořit seznam uživatelů ukázkovou aplikaci ASP.NET MVC 3. Projekt sady Visual Studio se zdrojovým kódem jazyka C# a VB je k dispozici v tomto tématu: [Stáhnout](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114). Pokud máte dotazy týkající se v tomto kurzu, zveřejněte je [mvcforum](https://forums.asp.net/1146.aspx).


## <a name="overview"></a>Přehled

Aplikace, kterou budete vytvářet je jednoduché uživatelské web seznamu. Uživatelům můžete zadat, zobrazit a aktualizovat informace o uživateli.

![Ukázkový web](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

Můžete si stáhnout dokončený projekt jazyka Visual Basic a C# [tady](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).

## <a name="creating-the-web-application"></a>Vytvoření webové aplikace

Chcete-li zahájit kurz, otevřete Visual Studio 2010 a vytvořte nový projekt pomocí *webové aplikace ASP.NET MVC 3* šablony. Pojmenujte aplikaci &quot;Mvc3Razor&quot;.

[![Nový projekt MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)

V **nového projektu ASP.NET MVC 3** dialogového okna, vyberte **internetovou aplikaci**vyberte zobrazovací modul Razor a potom klikněte na tlačítko **OK**.

![Dialogové okno nového projektu ASP.NET MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

V tomto kurzu nebudete používat poskytovatele členství prostředí ASP.NET, takže můžete odstranit všechny soubory přidružené k přihlášení a s členstvím. V **Průzkumníka řešení**, odebrat následující soubory a adresáře:

- *Controllers\AccountController*
- *Models\AccountModels*
- *Views\Shared\\_LogOnPartial*
- *Views\Account* (a všechny soubory v tomto adresáři)

![Soln Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

Upravit  <em>\_Layout.cshtml</em> souboru a nahraďte kód uvnitř `<div>` element s názvem `logindisplay` zprávou <em>&quot;</em>přihlašovací jméno zakázané&quot;. Následující příklad ukazuje novou značku:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a>Přidání modelu

V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *modely* složky, vyberte **přidat**a potom klikněte na **třídy**.

![Nová třída Mdl uživatele](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

Název třídy `UserModel`. Nahraďte obsah *UserModel* souboru následujícím kódem:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

`UserModel` Třída reprezentuje uživatele. Každý člen třídy, je opatřen poznámkou [vyžaduje](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) atribut z [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) oboru názvů. Atributy v [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) obor názvů ověřování automatické a server straně klienta pro webové aplikace.

Otevřít `HomeController` třídu a přidejte `using` direktiv, aby měli přístup ke `UserModel` a `Users` třídy:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

Hned za `HomeController` prohlášení, přidejte následující komentář a odkaz na `Users` třídy:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

`Users` Třída je úložiště dat zjednodušené, v paměti, které budete používat v tomto kurzu. V reálné aplikaci byste použili databázi k ukládání informací o uživateli. Několik prvních řádků tohoto `HomeController` souboru jsou uvedeny v následujícím příkladu:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

Sestavte aplikaci tak, aby model uživatelů budou mít k dispozici Průvodce generování uživatelského rozhraní v dalším kroku.

## <a name="creating-the-default-view"></a>Vytváří se výchozí zobrazení

Dalším krokem je přidání metody akce a zobrazení tak, aby uživatelé služby.

Odstranit existující *Views\Home\Index* souboru. Vytvoří nový *Index* soubor k zobrazení uživatelů.

V `HomeController` třídy, nahraďte obsah `Index` metodu s následujícím kódem:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

Klepněte pravým tlačítkem myši `Index` metody a pak klikněte na tlačítko **přidat zobrazení**.

![Přidání zobrazení](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

Vyberte **vytvoření zobrazení se silnými typy** možnost. Pro **zobrazení dat třídy**vyberte **Mvc3Razor.Models.UserModel**. (Pokud se nezobrazí **Mvc3Razor.Models.UserModel** v **zobrazení dat třídy** pole, budete muset sestavit projekt.) Ujistěte se, že modul zobrazení nastavený na **Razor**. Nastavte **zobrazit obsah** k **seznamu** a potom klikněte na tlačítko **přidat**.

![Přidání zobrazení Index](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

Nové zobrazení vygeneruje uživatelské automaticky rozhraní uživatelská data, která je předána `Index` zobrazení. Prozkoumejte nově vygenerovaný *Views\Home\Index* souboru. **Vytvořit nový**, **upravit**, **podrobnosti**, a **odstranit** odkazy nefungují, ale zbývající části stránky je funkční. Spuštění stránky. Zobrazí seznam uživatelů.

![Indexová stránka](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

Otevřít *Index.cshtml* soubor a nahradit `ActionLink` kód pro **upravit**, **podrobnosti**, a **odstranit** následujícím kódem :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

Uživatelské jméno se používá jako ID najít vybraný záznam v **upravit**, **podrobnosti**, a **odstranit** odkazy.

## <a name="creating-the-details-view"></a>Vytváření zobrazení podrobností

Dalším krokem je přidání `Details` metody akce a zobrazení, aby bylo možné zobrazit podrobnosti o uživateli.

![Podrobnosti](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

Přidejte následující `Details` metodu pro kontroler home:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

Klepněte pravým tlačítkem myši `Details` metody a pak vyberte <strong>přidat zobrazení</strong>. Ověřte, že <strong>zobrazení dat třídy</strong> pole obsahuje <strong>Mvc3Razor.Models.UserModel</strong><em>.</em> Nastavte <strong>zobrazit obsah</strong> k <strong>podrobnosti</strong> a potom klikněte na tlačítko <strong>přidat</strong>.

![Přidat zobrazení podrobností](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

Spusťte aplikaci a vyberte odkaz podrobnosti. Automatické generování uživatelského rozhraní zobrazuje každou vlastnost v modelu.

![Podrobnosti](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a>Vytvoření zobrazení pro úpravy

Přidejte následující `Edit` metodu pro kontroler home.

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

Přidání zobrazení jako v předchozích krocích, ale nastavit **zobrazit obsah** k **upravit**.

![Přidání zobrazení pro úpravy](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

Spusťte aplikaci a upravte první a poslední název jednoho z uživatelů. Pokud jste porušují `DataAnnotation` omezení, které se použily `UserModel` třídy, při odeslání formuláře, zobrazí se chyby ověření, které jsou vytvářeny v kódu serveru. Například, pokud změníte křestní jméno &quot;Ann&quot; k &quot;A&quot;při odeslání formuláře, ve formuláři se zobrazí následující chyba:

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

V tomto kurzu se zpracuje uživatelské jméno jako primární klíč. Proto nelze změnit název vlastnosti uživatele. V *Edit.cshtml* souboru hned za `Html.BeginForm` prohlášení, nastavte uživatelské jméno jako skryté pole. To způsobí, že vlastnost, která má být předán v modelu. Následující fragment kódu ukazuje umístění `Hidden` – příkaz:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

Nahradit `TextBoxFor` a `ValidationMessageFor` značek pro uživatelské jméno `DisplayFor` volání. `DisplayFor` Metoda zobrazí vlastnosti jako prvek jen pro čtení. Následující příklad ukazuje dokončené značky. Původní `TextBoxFor` a `ValidationMessageFor` volání, jsou zakomentované znaky začít komentář a konec komentáře syntaxe Razor (`@* *@`)

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a>Povolení ověřování na straně klienta

Pokud chcete povolit ověřování na straně klienta v architektuře ASP.NET MVC 3, je nutné nastavit dva příznaky a musí obsahovat tři soubory jazyka JavaScript.

Otevřete aplikaci prvku *Web.config* souboru. Ověřte `that ClientValidationEnabled` a `UnobtrusiveJavaScriptEnabled` jsou nastaveny na hodnotu true v nastavení aplikace. Následující fragment z kořenového adresáře *Web.config* souboru zobrazuje správné nastavení:

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

Nastavení `UnobtrusiveJavaScriptEnabled` na hodnotu true umožňuje nerušivý Ajax a ověření nerušivého klienta. Použijete-li nerušivý ověření, ověřovacích pravidel jsou převedena na atributy HTML5. Názvy atributů HTML5 může být tvořen pouze malá písmena, číslice a spojovníky.

Nastavení `ClientValidationEnabled` na hodnotu true umožňuje ověřování na straně klienta. Nastavením těchto klíčů v aplikaci *Web.config* souboru jste povolili ověřování na straně klienta a nerušivý JavaScript pro celou aplikaci. Můžete také povolit nebo zakázat tato nastavení v jednotlivých zobrazeních nebo v metodách kontroleru pomocí následujícího kódu:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

Také musíte zahrnout několik souborů JavaScriptu vykreslené zobrazení. Snadný způsob, jak ve všech zobrazeních přidejte kód JavaScript je pro jejich přidání do *Views\Shared\\_Layout.cshtml* souboru. Nahradit `<head>` elementu  *\_Layout.cshtml* souboru následujícím kódem:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

První dva skripty jQuery jsou hostované ve Microsoft Ajax Content Delivery Network (CDN). S využitím Microsoft Ajax CDN může výrazně zlepšit výkon stiskněte první aplikací.

Spusťte aplikaci a klikněte na odkaz pro úpravy. Zobrazte zdroj stránky v prohlížeči. Zdrojového kódu prohlížeče ukazuje mnoho atributů ve formátu `data-val` (pro ověření dat). Pokud je povoleno ověřování na straně klienta a nerušivý JavaScript, obsahovat vstupní pole pomocí pravidla ověřování na straně klienta `data-val="true"` atribut k aktivaci ověření nerušivého klienta. Například `City` byl doplněn pole v modelu [vyžaduje](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) atributů, což vede k HTML je znázorněno v následujícím příkladu:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

Pro každé pravidlo ověřování na straně klienta se přidá atribut, který má tvar `data-val-rulename="message"`. Pomocí `City` pole uvedená výše, generuje pravidlo vyžaduje ověřování na straně klienta, například `data-val-required` atribut a zpráva &quot;je povinné pole Město&quot;. Spusťte aplikaci a úpravě jednoho z uživatelů, zrušte `City` pole. Když kartě mimo pole se zobrazí chybovou zprávu ověření na straně klienta.

![Město vyžaduje](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

Podobně pro každý parametr pravidla ověřování na straně klienta, přidání atributu, který má tvar `data-val-rulename-paramname=paramvalue`. Například `FirstName` vlastnost je opatřen poznámkou [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) atribut a určuje minimální délku 3 a maximální délce 8. Pravidlo ověřování dat s názvem `length` má název parametru `max` a hodnota parametru 8. Následující příklad zobrazuje kód HTML, který je generován pro `FirstName` pole při úpravě jednoho z uživatelů:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

Další informace o ověření nerušivého klienta naleznete v příspěvku [Nerušivý ověření klienta v architektuře ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) v blogu Brada Wilsona.

> [!NOTE]
> V ASP.NET MVC 3 Beta musíte někdy odesláním formuláře, aby bylo možné spustit ověřování na straně klienta. To může být změněn ve finální verzi.


## <a name="creating-the-create-view"></a>Vytvoření zobrazení pro vytváření

Dalším krokem je přidání `Create` metody akce a zobrazení, chcete-li povolit uživatelům vytvoření nového uživatele. Přidejte následující `Create` metodu pro kontroler home:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

Přidání zobrazení jako v předchozích krocích, ale nastavit **zobrazit obsah** k **vytvořit**.

![Vytvoření zobrazení](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

Spusťte aplikaci, vyberte **vytvořit** propojit, a přidejte nového uživatele. `Create` Metoda automaticky využívá výhod ověření na straně klienta i stranu serveru. Zkuste zadat uživatelské jméno, který obsahuje prázdné znaky, jako například &quot;Ben X&quot;. Pokud kartu mimo pole uživatelské jméno na chybu ověřování na straně klienta (`White space is not allowed`) se zobrazí.

## <a name="add-the-delete-method"></a>Přidání metody odstranění

Pro absolvování tohoto kurzu, přidejte následující `Delete` metodu pro kontroler home:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

Přidat `Delete` zobrazení jako v předchozích krocích, nastavení **zobrazit obsah** k **odstranit**.

![Odstranit zobrazení](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

Nyní máte jednoduchý, ale plně funkční aplikaci ASP.NET MVC 3 s ověřením.
