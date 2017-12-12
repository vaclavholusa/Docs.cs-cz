---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
title: "Přidání ověřování do modelu (C#) | Microsoft Docs"
author: Rick-Anderson
description: "Vytvoření řadiče"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 9af927e2-1c3b-43d9-917d-1df75be3c482
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: a1d6a6468a39f31c3af8779abbbced093288773c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="adding-validation-to-the-model-c"></a>Přidání ověřování do modelu (C#)
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Je k dispozici aktualizovaná verze tohoto kurzu [sem](../../../getting-started/introduction/getting-started.md) používající ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.
> 
> 
> V tomto kurzu naučit se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, který je bezplatnou verzi sady Microsoft Visual Studio. Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže. Kliknutím na následující odkaz můžete nainstalovat všechny z nich: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:
> 
> - [Visual Studio Web Developer Express SP1 požadavky](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizace nástrojů rozhraní ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podporu runtime + nástroje)
> 
> Pokud používáte Visual Studio 2010 místo Visual Web Developer 2010, nainstalujte součásti kliknutím na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer se zdrojový kód C# je k dispozici v tomto tématu. [Stáhnout verzi jazyka C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Pokud dáváte přednost jazyka Visual Basic, přepnout [verze jazyka Visual Basic](../vb/intro-to-aspnet-mvc-3.md) tohoto kurzu.


V této části přidáte logiku ověření pro `Movie` modelu a budete ujistěte, že ověřovací pravidla se uplatňují vždy, když uživatel se pokusí vytvořit nebo upravit film pomocí aplikace.

## <a name="keeping-things-dry"></a>Zachování SUCHÝCH věcí

Jeden ze základních principů návrhu architektury ASP.NET MVC je suchého ("nemáte opakujte sami"). ASP.NET MVC umožňuje zadejte funkce nebo chování jenom jednou a potom ho v aplikaci projeví everywhere mít. To snižuje množství kódu, které potřebujete k zápisu a umožňuje kód, který můžete psát mnohem snadnější k údržbě.

Podpora ověřování poskytované ASP.NET MVC a Entity Framework Code First je skvělým příklad SUCHÝCH zásadu v akci. Pravidla ověřování můžete určit deklarativně na jednom místě (ve třídě modelu) a pak se tato pravidla vynucují everywhere v aplikaci.

Podíváme, jak můžete využít výhod tato podpora ověřování v aplikaci film.

## <a name="adding-validation-rules-to-the-movie-model"></a>Přidávání pravidel ověřování modelu filmu

Budete začněte tím, že některé logiku ověření pro přidání `Movie` třídy.

Otevřete *Movie.cs* souboru. Přidat `using` příkaz v horní části souboru, který odkazuje [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) obor názvů:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

Obor názvů je součástí rozhraní .NET Framework. Poskytuje integrovanou sadu atributů ověření, které můžete provést deklarativně všechny třídy nebo vlastnost.

Nyní aktualizovat `Movie` třída využít předdefinované [ `Required` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), a [ `Range` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.rangeattribute.aspx) atributů ověření . Použijte následující kód jako příklad umístění pro použití atributy.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs)]

Atributy ověření zadejte chování, které chcete vynutit ve vlastnostech modelu, které se použijí k. `Required` Atribut uvádí, že vlastnost musí mít hodnotu; v této ukázce, musí mít hodnoty pro film `Title`, `ReleaseDate`, `Genre`, a `Price` vlastnosti, aby byla platná. `Range` Atribut omezuje hodnota v zadaném rozsahu. `StringLength` Atribut umožňuje nastavit maximální délka ve vlastnosti string a volitelně jeho minimální délka.

Kód nejprve zajistí, že ověřovací pravidla, které jste zadali na třídu modelu, jsou vyžadována před aplikace uloží změny v databázi. Například následující kód vyvolá výjimku při `SaveChanges` metoda je volána, protože některé požadované `Movie` chybí hodnoty vlastností a je nulová (což je mimo platný rozsah).

[!code-csharp[Main](adding-validation-to-the-model/samples/sample3.cs)]

S ověřovacích pravidel automaticky vynucováno rozhraní .NET Framework pomáhá zkontrolujte aplikace robustnější. Také zajistí, že nelze zapomenete a ověřit něco nechtěně umožní chybná data do databáze.

Tady je úplný výpis pro aktualizaci kódu *Movie.cs* souboru:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Chyba ověření uživatelského rozhraní v architektuře ASP.NET MVC

Znovu spusťte aplikaci a přejděte do */Movies* adresy URL.

Klikněte na tlačítko **vytvořit film** odkaz na přidání nové videa. Vyplňte formulář s některé neplatné hodnoty a klikněte **vytvořit** tlačítko.

[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)

Všimněte si, jak formulář automaticky použila barvu pozadí zvýrazněte textová pole, které obsahují neplatná data a má vygenerované chybovou zprávu ověření příslušné vedle každé z nich. Chybové zprávy, které odpovídají řetězce chyba jste zadali při poznámky `Movie` třídy. Chyby se vynucují straně klienta (pomocí jazyka JavaScript) a na straně serveru (v případě, že uživatel, který má JavaScript zakázaná).

Skutečné výhodou je, že nebyla budete muset změnit jeden řádek kódu `MoviesController` třídy nebo v *Create.cshtml* zobrazení, aby bylo možné povolit toto ověření uživatelského rozhraní. Řadič a zobrazení, které jste vytvořili dříve v tomto kurzu automaticky zachyceny až ověření pravidla, kterou jste zadali pomocí atributů na `Movie` třída modelu.

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Dojde k ověření v vytvořením zobrazování a vytváření metody akce

Může vás zajímat, jak byl vygenerován ověření uživatelské rozhraní bez jakýchkoli aktualizací kód v kontroleru nebo zobrazení. Další výpis znázorňuje, co `Create` metody v `MovieController` třída vypadají. Jsou stejné jako u jak je vytvořen v tomto kurzu.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

První metoda akce zobrazí počáteční vytvořit formulář. Druhý zpracovává post formuláře. Druhý `Create` volání metod `ModelState.IsValid` zkontrolujte, zda video má všechny chyby ověření. Voláním této metody vyhodnotí všechny atributy ověřování, které byly použity k objektu. Pokud objekt má chyby ověření, `Create` metoda znovu zobrazí formulář. Pokud nejsou žádné chyby, metoda nové film uloží v databázi.

Níže je *Create.cshtml* zobrazit šablonu, která vygeneroval dříve v tomto kurzu. Použije se uvedené výše obě metody akce k zobrazení původního formuláře a znovu ji zobrazit v případě chyby.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Všimněte si, jak kód používá `Html.EditorFor` pomocná rutina pro výstup `<input>` element pro každou `Movie` vlastnost. U tohoto pomocníka je volání `Html.ValidationMessageFor` metodu helper. Tyto dvě metody helper pracovat objekt modelu, který je předán zobrazení řadičem (v tomto případě `Movie` objekt). Budou automaticky hledat atributů ověření zadané v modelu a zobrazí chybové zprávy podle potřeby.

Co je skutečně dobrý o tento přístup je, že řadičem ani vytvořit šablony zobrazení nic o skutečné ověřovacích pravidel vynucován nebo zná o specifické chybové zprávy zobrazují. Ověřovací pravidla a řetězce chyby se zadávají pouze v `Movie` třídy.

Pokud chcete později změnit logiku ověření, můžete tak učinit na jednom místě. Nebudete mít starat o různých částech aplikace je nekonzistentní s jak se vynucují pravidla – veškerou logiku ověřování definované na jednom místě se použije everywhere. To zajišťuje kód velmi vyčištění a umožňuje snadno spravovat a momentální. . To znamená, že budete plně ctít zásady SUCHÝCH Princip.

## <a name="adding-formatting-to-the-movie-model"></a>Přidání formátování ke film modelu

Otevřete *Movie.cs* souboru. [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) Obor názvů poskytuje atributy formátování kromě integrovanou sadu atributů ověření. Se použije pouze [ `DisplayFormat` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atribut a [ `DataType` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) Výčtová hodnota, datum vydání a pole cena. Následující kód ukazuje `ReleaseDate` a `Price` vlastnosti s příslušnou [ `DisplayFormat` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atribut.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs)]

Alternativně můžete explicitně nastavit [ `DataFormatString` ](https://msdn.microsoft.com/en-us/library/system.string.format.aspx) hodnotu. Následující kód ukazuje vlastnost datum vydání s řetězec formátu datum (konkrétně, "d"). To byste použili k určení, že nechcete, aby čas jako součást datum vydání.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample8.cs)]

Následující kód formáty `Price` vlastnost jako měnu.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

Kompletní `Movie` třídy jsou uvedeny níže.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Spusťte aplikaci a přejděte do `Movies` řadiče.

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

V další části řady, jsme budete zkontrolujte, zda aplikace a některá vylepšení pro automaticky generované `Details` a `Delete` metody.

>[!div class="step-by-step"]
[Předchozí](adding-a-new-field.md)
[další](improving-the-details-and-delete-methods.md)
