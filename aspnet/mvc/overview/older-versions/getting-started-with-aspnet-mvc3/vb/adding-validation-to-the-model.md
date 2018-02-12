---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
title: "Přidání ověřování do modelu (VB) | Microsoft Docs"
author: Rick-Anderson
description: "V tomto kurzu naučit se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, který je..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 878f6c31-972d-45f4-8849-5c633b511409
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: cac86760b90c90a0ea2fad16268f60b5ccf61299
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/12/2018
---
<a name="adding-validation-to-the-model-vb"></a>Přidání ověřování do modelu (VB)
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> V tomto kurzu naučit se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, který je bezplatnou verzi sady Microsoft Visual Studio. Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže. Kliknutím na následující odkaz můžete nainstalovat všechny z nich: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:
> 
> - [Visual Studio Web Developer Express SP1 požadavky](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizace nástrojů rozhraní ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podporu runtime + nástroje)
> 
> Pokud používáte Visual Studio 2010 místo Visual Web Developer 2010, nainstalujte součásti kliknutím na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer se VB.NET zdrojový kód je k dispozici v tomto tématu. [Stáhnout verzi VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Pokud dáváte přednost C#, přepnout [C# verze](../cs/adding-validation-to-the-model.md) tohoto kurzu.


V této části přidáte logiku ověření pro `Movie` modelu a budete ujistěte, že ověřovací pravidla se uplatňují vždy, když uživatel se pokusí vytvořit nebo upravit film pomocí aplikace.

## <a name="keeping-things-dry"></a>Zachování SUCHÝCH věcí

Jeden ze základních principů návrhu architektury ASP.NET MVC je suchého ("nemáte opakujte sami"). ASP.NET MVC umožňuje zadejte funkce nebo chování jenom jednou a potom ho v aplikaci projeví everywhere mít. To snižuje množství kódu, které potřebujete k zápisu a umožňuje kód, který můžete psát mnohem snadnější k údržbě.

Podpora ověřování poskytované ASP.NET MVC a Entity Framework Code First je skvělým příklad SUCHÝCH zásadu v akci. Pravidla ověřování můžete určit deklarativně na jednom místě (ve třídě modelu) a pak se tato pravidla vynucují everywhere v aplikaci.

Podíváme, jak můžete využít výhod tato podpora ověřování v aplikaci film.

## <a name="adding-validation-rules-to-the-movie-model"></a>Přidávání pravidel ověřování modelu filmu

Budete začněte tím, že některé logiku ověření pro přidání `Movie` třídy.

Otevřete *Movie.vb* souboru. Přidat `Imports` příkaz v horní části souboru, který odkazuje [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) obor názvů:

[!code-vb[Main](adding-validation-to-the-model/samples/sample1.vb)]

Obor názvů je součástí rozhraní .NET Framework. Poskytuje integrovanou sadu atributů ověření, které můžete provést deklarativně všechny třídy nebo vlastnost.

Nyní aktualizovat `Movie` třída využít předdefinované [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), a [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) atributů ověření . Použijte následující kód jako příklad umístění pro použití atributy.

[!code-vb[Main](adding-validation-to-the-model/samples/sample2.vb)]

Atributy ověření zadejte chování, které chcete vynutit ve vlastnostech modelu, které se použijí k. `Required` Atribut uvádí, že vlastnost musí mít hodnotu; v této ukázce, musí mít hodnoty pro film `Title`, `ReleaseDate`, `Genre`, a `Price` vlastnosti, aby byla platná. `Range` Atribut omezuje hodnota v zadaném rozsahu. `StringLength` Atribut umožňuje nastavit maximální délka ve vlastnosti string a volitelně jeho minimální délka.

Kód nejprve zajistí, že ověřovací pravidla, které jste zadali na třídu modelu, jsou vyžadována před aplikace uloží změny v databázi. Například následující kód vyvolá výjimku při `SaveChanges` metoda je volána, protože některé požadované `Movie` chybí hodnoty vlastností a je nulová (což je mimo platný rozsah).

[!code-vb[Main](adding-validation-to-the-model/samples/sample3.vb)]

S ověřovacích pravidel automaticky vynucováno rozhraní .NET Framework pomáhá zkontrolujte aplikace robustnější. Také zajistí, že nelze zapomenete a ověřit něco nechtěně umožní chybná data do databáze.

Tady je úplný výpis pro aktualizaci kódu *Movie.vb* souboru:

[!code-vb[Main](adding-validation-to-the-model/samples/sample4.vb)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Chyba ověření uživatelského rozhraní v architektuře ASP.NET MVC

Znovu spusťte aplikaci a přejděte do */Movies* adresy URL.

Klikněte na tlačítko **vytvořit film** odkaz na přidání nové videa. Vyplňte formulář s některé neplatné hodnoty a klikněte **vytvořit** tlačítko.

[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)

Všimněte si, jak formulář automaticky použila barvu pozadí zvýrazněte textová pole, které obsahují neplatná data a má vygenerované chybovou zprávu ověření příslušné vedle každé z nich. Chybové zprávy, které odpovídají řetězce chyba jste zadali při poznámky `Movie` třídy. Chyby se vynucují straně klienta (pomocí jazyka JavaScript) a na straně serveru (v případě, že uživatel, který má JavaScript zakázaná).

Skutečné výhodou je, že nebyla budete muset změnit jeden řádek kódu `MoviesController` třídy nebo v *Create.vbhtml* zobrazení, aby bylo možné povolit toto ověření uživatelského rozhraní. Řadič a zobrazení, které jste vytvořili dříve v tomto kurzu automaticky zachyceny až ověření pravidla, kterou jste zadali pomocí atributů na `Movie` třída modelu.

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Dojde k ověření v vytvořením zobrazování a vytváření metody akce

Může vás zajímat, jak byl vygenerován ověření uživatelské rozhraní bez jakýchkoli aktualizací kód v kontroleru nebo zobrazení. Další výpis znázorňuje, co `Create` metody v `MovieController` třída vypadají. Jsou stejné jako u jak je vytvořen v tomto kurzu.

[!code-vb[Main](adding-validation-to-the-model/samples/sample5.vb)]

První metoda akce zobrazí počáteční vytvořit formulář. Druhý zpracovává post formuláře. Druhý `Create` volání metod `ModelState.IsValid` zkontrolujte, zda video má všechny chyby ověření. Voláním této metody vyhodnotí všechny atributy ověřování, které byly použity k objektu. Pokud objekt má chyby ověření, `Create` metoda znovu zobrazí formulář. Pokud nejsou žádné chyby, metoda nové film uloží v databázi.

Níže je *Create.vbhtml* zobrazit šablonu, která vygeneroval dříve v tomto kurzu. Použije se uvedené výše obě metody akce k zobrazení původního formuláře a znovu ji zobrazit v případě chyby.

[!code-vbhtml[Main](adding-validation-to-the-model/samples/sample6.vbhtml)]

Všimněte si, jak kód používá `Html.EditorFor` pomocná rutina pro výstup `<input>` element pro každou `Movie` vlastnost. U tohoto pomocníka je volání `Html.ValidationMessageFor` metodu helper. Tyto dvě metody helper pracovat objekt modelu, který je předán zobrazení řadičem (v tomto případě `Movie` objekt). Budou automaticky hledat atributů ověření zadané v modelu a zobrazí chybové zprávy podle potřeby.

Co je skutečně dobrý o tento přístup je, že řadičem ani vytvořit šablony zobrazení nic o skutečné ověřovacích pravidel vynucován nebo zná o specifické chybové zprávy zobrazují. Ověřovací pravidla a řetězce chyby se zadávají pouze v `Movie` třídy.

Pokud chcete později změnit logiku ověření, můžete tak učinit na jednom místě. Nebudete mít starat o různých částech aplikace je nekonzistentní s jak se vynucují pravidla – veškerou logiku ověřování definované na jednom místě se použije everywhere. To zajišťuje kód velmi vyčištění a umožňuje snadno spravovat a momentální. . To znamená, že je budete mít plně ctít zásady SUCHÝCH Princip.

## <a name="adding-formatting-to-the-movie-model"></a>Přidání formátování ke film modelu

Otevřete *Movie.vb* souboru. [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Obor názvů poskytuje atributy formátování kromě integrovanou sadu atributů ověření. Se použije pouze [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atribut a [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) Výčtová hodnota, datum vydání a pole cena. Následující kód ukazuje `ReleaseDate` a `Price` vlastnosti s příslušnou [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atribut.

[!code-vb[Main](adding-validation-to-the-model/samples/sample7.vb)]

Alternativně můžete explicitně nastavit [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx) hodnotu. Následující kód ukazuje vlastnost datum vydání s řetězec formátu datum (konkrétně, "d"). To byste použili k určení, že nechcete, aby čas jako součást datum vydání.

[!code-vb[Main](adding-validation-to-the-model/samples/sample8.vb)]

Následující kód formáty `Price` vlastnost jako měnu.

[!code-vb[Main](adding-validation-to-the-model/samples/sample9.vb)]

Kompletní `Movie` třídy jsou uvedeny níže.

[!code-vb[Main](adding-validation-to-the-model/samples/sample10.vb)]

Spusťte aplikaci a přejděte do `Movies` řadiče.

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

V další části řady, jsme budete zkontrolujte, zda aplikace a některá vylepšení pro automaticky generované `Details` a `Delete` metody...

>[!div class="step-by-step"]
[Předchozí](adding-a-new-field.md)
[další](improving-the-details-and-delete-methods.md)
