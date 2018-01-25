---
uid: web-pages/overview/ui-layouts-and-themes/4-working-with-forms
title: "Práce s formuláře HTML na webech ASP.NET Web Pages (Razor) | Microsoft Docs"
author: tfitzmac
description: "Formulář je část dokumentu HTML, kam jste umístili ovládací prvky vstup uživatele, jako textová pole, zaškrtněte políčka, přepínače a rozevírací seznamy. Použití formulářů shod..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: f3f4b8c8-e8f6-4474-ad94-69228a6c01ee
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/4-working-with-forms
msc.type: authoredcontent
ms.openlocfilehash: 8579c444fd19d1a366349cc09f9f768de23055f8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="working-with-html-forms-in-aspnet-web-pages-razor-sites"></a>Práce s formuláře HTML na webech ASP.NET – webové stránky (Razor)
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento článek popisuje, jak zpracovat formuláře HTML (u textových polí a tlačítka) při práci na webu technologie ASP.NET Web Pages (Razor).
> 
> **Získáte informace:** 
> 
> - Jak vytvořit formuláře HTML.
> - Jak si uživatelského vstupu z formuláře.
> - Postup ověření vstupu uživatele.
> - Postup obnovení hodnot formuláře po odeslání stránky.
> 
> Jsou to ASP.NET programování koncepty představené v článku:
> 
> - `Request` Objektu.
> - Ověření vstupu.
> - Kódování HTML.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Rozhraní ASP.NET Web Pages (Razor) 3
>   
> 
> V tomto kurzu taky spolupracuje se službou ASP.NET Web Pages 2.


## <a name="creating-a-simple-html-form"></a>Vytvoření jednoduchého formuláře HTML

1. Vytvoření nového webu.
2. V kořenové složce vytvořit webovou stránku s názvem *Form.cshtml* a zadejte následující kód:

    [!code-html[Main](4-working-with-forms/samples/sample1.html)]
3. Spusťte stránku v prohlížeči. (Ve službě WebMatrix, v **soubory** pracovní prostor, klikněte pravým tlačítkem na soubor a pak vyberte **spustit v prohlížeči**.) Jednoduchý formulář s tři vstupní pole a **odeslání** je zobrazeno tlačítko.

    ![Snímek obrazovky s tři textová pole formuláře.](4-working-with-forms/_static/image1.jpg)

    V tomto bodu, pokud kliknete **odeslání** tlačítko, nedojde k žádné akci. Chcete-li formuláře užitečné, budete muset přidat nějaký kód, který se spustí na serveru.

## <a name="reading-user-input-from-the-form"></a>Načítání vstupu uživatele z formuláře

Při zpracování formuláře, přidáte kód, který čte hodnoty odeslaná polí a nemá něco s nimi. Tento postup ukazuje, jak číst pole a uživatelský vstup, zobrazí na stránce. (V produkční aplikace, že obecně provádět zajímavějšího akce se vstupem uživatele. Můžete to udělat v článku o práci s databázemi.)

1. V horní části *Form.cshtml* souboru, zadejte následující kód:

    [!code-cshtml[Main](4-working-with-forms/samples/sample2.cshtml)]

    Když uživatel poprvé požádá stránky, zobrazí se pouze prázdný formulář. Uživatel (který bude vám) doplní do formuláře a poté klikne na tlačítko **odeslání**. To odešle (příspěvky) vstupu uživatele na serveru. Ve výchozím nastavení, žádost přejde na stejné stránce (konkrétně, *Form.cshtml*).

    Při odesílání stránky této doby se zobrazují hodnoty, které jste zadali nad formuláře:

    ![Snímek obrazovky, který zobrazuje hodnoty, které jste zadali na stránku.](4-working-with-forms/_static/image2.jpg)

    Podívejte se na kód pro stránku. Prvním použití `IsPost` metoda k určení, jestli je stránka vrácena &#8212; to znamená, zda uživatel klikli **odeslání** tlačítko. Pokud je to příspěvku na `IsPost` vrací hodnotu true. To je standardní způsob na webových stránkách ASP.NET k určení, zda pracujete s úvodního požadavku (požadavek GET) nebo zpětné volání (požadavek POST). (Další informace o GET a POST, najdete v části "HTTP GET a POST a IsPost Property" na bočním panelu v [Úvod k technologii ASP.NET Web Pages programování pomocí syntaxe Razor](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost).)

    Teď se dostáváte hodnoty, které uživatel vyplněno z `Request.Form` objekt a umístí je do proměnných pro pozdější. `Request.Form` Objekt obsahuje všechny hodnoty, které byly odeslány s hledanou stránkou, každý se identifikovanou pomocí klíče. Klíč se o ekvivalent `name` atribut pole formuláře, které chcete číst. Například pro čtení `companyname` pole (textového pole), můžete použít `Request.Form["companyname"]`.

    Formulář hodnoty se uloží v `Request.Form` objektu jako řetězce. Proto když máte pro práci s hodnotou jako číslo nebo datum nebo jiný typ, budete muset převést řetězec na typu. V příkladu `AsInt` metodu `Request.Form` se používá k hodnotu pole zaměstnanci (který obsahuje počtu zaměstnanců) převést na celé číslo.
2. Spusťte stránku v prohlížeči, vyplňte pole formuláře a klikněte na tlačítko **odeslání**. Na stránce zobrazuje hodnoty, které jste zadali.

> [!TIP] 
> 
> <a id="SB_HTMLEncoding"></a>
> ### <a name="html-encoding-for-appearance-and-security"></a>Kódování pro vzhled a zabezpečení v jazyce HTML
> 
> HTML má speciální používá pro znaků jako `<`, `>`, a `&`. Pokud se zobrazí tyto speciální znaky, kde se nepředpokládá, že poškodí vzhled a funkce webové stránky. Například v prohlížeči interpretuje `<` znak (Pokud je následované mezerou) jako začátek elementu HTML, jako je třeba `<b>` nebo `<input ...>`. Pokud v prohlížeči nerozpozná elementu, jednoduše zahodí řetězec, který začíná `<` dokud nedosáhne něco, znovu rozpozná. Samozřejmě to může způsobit některé divné vykreslování na stránce.
> 
> Kódování HTML nahradí kód, který prohlížeče interpretovat jako správný symbol tyto vyhrazené znaky. Například `<` se nahradí znak `&lt;` a `>` se nahradí znak `&gt;`. V prohlížeči vykreslí jako znaků, které chcete zobrazit tyto náhradní řetězce.
> 
> Je vhodné používat kódování kdykoli zobrazit řetězce v jazyce HTML (vstup) jste získali od uživatele. Pokud to neuděláte, uživatel může pokusit o webovou stránku, spustit skript nebo dělejte něco jiného, ohrožuje zabezpečení vašeho webu nebo který je právě není to, co chcete získat. (To je zvlášť důležité, pokud pořídíte uživatelský vstup, uložte ho někde a následně se zobrazí později &#8212; například jako komentář blog uživatele zkontrolovat, nebo něco jako, který.)
> 
> Aby se zabránilo tyto problémy, rozhraní ASP.NET Web Pages automaticky umístí kódování HTML jakýkoli text obsahu, které výstup z vašeho kódu. Například při zobrazení obsahu proměnné nebo výraz pomocí kódu, například `@MyVar`, rozhraní ASP.NET Web Pages automaticky kóduje výstup.


## <a name="validating-user-input"></a>Ověřování uživatelského vstupu

Uživatelé provádět chyb. Požádejte, aby vyplnit pole a že zapomenete, nebo požádejte, aby zadat počet zaměstnanců a se místo toho zadejte název. Abyste měli jistotu, že formulář bylo vyplněno správně před jeho zpracování, ověření vstupu uživatele.

Tento postup ukazuje, jak ověřit všechna pole tři formuláře a ujistěte se, že uživatel nebyl zůstat prázdné. Také zkontrolujte, zda je hodnota počtu zaměstnanců číslo. Pokud nejsou chyby, budete se zobrazí chybová zpráva, která uživateli říká, které hodnoty neproběhla úspěšně ověření.

1. V *Form.cshtml* souboru, první blok kódu nahraďte následujícím kódem: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample3.cshtml)]

    K ověření vstupu uživatele, můžete použít `Validation` pomocné rutiny. Registrace povinná pole voláním `Validation.RequireField`. Registrace jiné typy ověření voláním `Validation.Add` a zadání pole, které chcete ověřit a typ ověření provést.

    Při spuštění stránky ASP.NET se všemi ověřovacími pro vás. Můžete zkontrolovat výsledky voláním `Validation.IsValid`, která vrací hodnotu true, pokud se vše předán a NEPRAVDA, pokud žádné pole se nezdařilo ověření. Obvykle volání `Validation.IsValid` před provedením jakékoli zpracování na vstupu uživatele.
2. Aktualizace `<body>` element přidáním tří volání `Html.ValidationMessage` metoda takto:

    [!code-cshtml[Main](4-working-with-forms/samples/sample4.cshtml?highlight=8,13,18)]

    K zobrazení chybových zpráv ověření, můžete volat Html.`ValidationMessage` a předejte ji název pole, které chcete zprávu pro.
3. Spuštění stránky. Nechte pole prázdné a klikněte na **odeslání**. Zobrazí chybové zprávy.

    ![Snímek obrazovky zobrazující chybové zprávy zobrazí, pokud není uživatelský vstup projít ověřením.](4-working-with-forms/_static/image3.jpg)
4. Přidat řetězec (například "ABC") do **počet zaměstnanců** pole a klikněte na tlačítko **odeslání** znovu. Tentokrát uvidíte chybu, která označuje, že řetězec není ve správném formátu, a to, celé číslo.

    ![Snímek obrazovky zobrazující chybové zprávy zobrazí, pokud uživatelé zadat řetězec pro pole zaměstnanci.](4-working-with-forms/_static/image4.jpg)

ASP.NET – webové stránky poskytuje další možnosti pro ověřování uživatelského vstupu, včetně možnost automaticky provádět ověření pomocí klientského skriptu, aby uživatelé získali okamžitou zpětnou vazbu v prohlížeči. Najdete v článku [další prostředky](#Additional_Resources) části později pro další informace.

## <a name="restoring-form-values-after-postbacks"></a>Obnovení po postback hodnot formuláře

Při testování stránky v předchozí části Možná jste si všimli, pokud jste měli k chybě ověření, vše, co že jste zadali (ne jenom neplatná data) byl pryč, jste museli znovu zadejte hodnoty pro všechna pole. To ukazuje bod důležité: po odeslání stránky, jeho zpracování a potom vykreslení stránky, stránka je znovu vytvořit od začátku. Jak už jste viděli, to znamená, že všechny hodnoty, které byly na stránce, pokud byla odeslána budou ztraceny.

Problém můžete vyřešit snadno, ale. Máte přístup k hodnotám, které byly odeslány (v `Request.Form` objektu, tak při vykreslení stránky, můžete vyplnit tyto hodnoty zpět do pole formuláře.

1. V *Form.cshtml* souboru, nahraďte `value` atributy `<input>` elementů pomocí `value` atribut.: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample5.cshtml?highlight=13,19,25)]

    `value` Atribut `<input>` elementy byla nastavena dynamicky načíst hodnotu pole z `Request.Form` objektu. Při prvním požadavku na stránku hodnoty `Request.Form` objekt jsou všechny prázdné. Toto je dobře, protože tímto způsobem formuláře je prázdná.
2. Spusťte stránku v prohlížeči, vyplňte pole formuláře nebo zůstat prázdné a klikněte na tlačítko **odeslání**. Zobrazí se stránka, která zobrazuje odeslaná hodnoty.

    ![forms-5](4-working-with-forms/_static/image5.jpg)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Další prostředky

- [1,001 způsoby, jak získat vstup od uživatelů webu](https://msdn.microsoft.com/library/ms971057.aspx)
- [Pomocí formulářů a zpracování uživatelského vstupu](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)
- [Ověřování uživatelských vstupů na webech s webovými stránkami ASP.NET](https://go.microsoft.com/fwlink/?LinkId=253002)
- [Použití automatického dokončování v formuláře HTML](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)
