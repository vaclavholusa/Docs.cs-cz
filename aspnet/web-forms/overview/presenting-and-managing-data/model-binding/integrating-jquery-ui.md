---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: Integrace s vazby modelu a webových formulářů JQuery UI ovládací prvek Datepicker | Microsoft Docs
author: tfitzmac
description: Tento kurz řady ukazuje základní aspekty projektu webových formulářů ASP.NET pomocí vazby modelu. Interakce dat umožňuje vazby modelu další přímo-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 126262b440f3e914a7fac3f0b7eeadb4f648d2bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a>Ovládací prvek Datepicker uživatelského rozhraní JQuery integrování vazby modelu a webové formuláře
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento kurz řady ukazuje základní aspekty projektu webových formulářů ASP.NET pomocí vazby modelu. Vazby modelu umožňuje dat interakce více jednoduché než plánování práce s daty objekty zdrojů (například ObjectDataSource nebo SqlDataSource). Tato řada začíná úvodní informace a přesune do dalších pokročilých konceptů v dalších kurzech.
> 
> Tento kurz ukazuje, jak přidat rozhraní JQuery [ovládací prvek Datepicker pomůcky](http://jqueryui.com/datepicker/) webového formuláře a použití modelu vazby k aktualizaci databáze s vybrané hodnoty.
> 
> V tomto kurzu vychází vytvořené v projektu [první](retrieving-data.md) a [druhý](updating-deleting-and-creating-data.md) částí řady.
> 
> Můžete [Stáhnout](https://go.microsoft.com/fwlink/?LinkId=286116) dokončený projekt v jazyce C# nebo VB. Kód ke stažení pracuje s Visual Studio 2012 nebo Visual Studio 2013. Používá šablony sady Visual Studio 2012, která se poněkud liší od šablony sady Visual Studio 2013 uvedené v tomto kurzu.


## <a name="what-youll-build"></a>Co budete sestavení

V tomto kurzu budete:

1. Přidání vlastnosti modelu, k zaznamenání Studentova datum zápisu
2. Povolit uživateli vybrat datum registrace pomocí widgetu ovládací prvek Datepicker uživatelského rozhraní JQuery
3. Vynutit ověřovacích pravidel pro datum zápisu

Ovládací prvek Datepicker uživatelského rozhraní JQuery pomůcky umožňuje uživatelům snadno vyberte datum z kalendáře, která se objeví po uživatel pracuje s polem. Pomocí tohoto widgetu může být pro uživatele snazší než ručním zadáním datum. Integrace widgetu ovládací prvek Datepicker do stránky, která používá vazby modelu pro datové operace vyžaduje jenom malé množství další práci.

## <a name="add-a-new-property-to-the-model"></a>Přidat novou vlastnost modelu

Nejprve budete přidávat **data a času** vlastnosti tak, aby vaše Student modelu a tato změna migrovat do databáze. Otevřete **UniversityModels.cs**a přidejte do modelu Student zvýrazněný kód.

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

**RangeAttribute** je součástí vynutit ověřovacích pravidel pro vlastnost. V tomto kurzu budeme předpokládat že Contoso univerzity byla založena na 1. leden 2013, a proto starší registrace data nejsou platná.

V okně správy balíčků přidat migrace spuštěním příkazu **přidat migrace AddEnrollmentDate**. Všimněte si, že kód migrace přidá do tabulky Student nový sloupec datového typu Datetime. Odpovídat hodnotě, kterou jste zadali v RangeAttribute, přidejte výchozí hodnota pro nový sloupec, jak je znázorněno v následující zvýrazněný kód.

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

Uložte změny do souboru migrace.

Není nutné znovu počáteční hodnoty data. Proto otevřete **Configuration.cs** ve složce migrace a odeberte nebo komentář kód **počáteční hodnoty** metoda. Soubor uložte a zavřete.

Nyní, spusťte příkaz **update-database**. Všimněte si, že sloupec nyní existuje v databázi a všechny existující záznamy mají výchozí hodnota pro EnrollmentDate.

## <a name="add-dynamic-controls-for-enrollment-date"></a>Přidat dynamických ovládacích prvků pro datum zápisu

Nyní přidáte ovládací prvky pro zobrazení a úpravy datum registrace. V tomto okamžiku je upravit hodnotu v textovém poli. Později v tomto kurzu změníte textového pole na ovládací prvek JQuery Datepicker pomůcky.

Nejprve je důležité si uvědomit, že nepotřebujete, aby všechny změny **AddStudent.aspx** souboru. Ovládací prvek DynamicEntity automaticky vykreslí novou vlastnost.

Otevřete **Students.aspx**a přidejte následující zvýrazněný kód.

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

Spusťte aplikaci a Všimněte si, že můžete nastavit hodnotu Datum registrace zadáním datum. Při přidávání nové student:

![Nastavte datum](integrating-jquery-ui/_static/image1.png)

Nebo úpravou existující hodnoty:

![Upravit datum](integrating-jquery-ui/_static/image2.png)

Zadejte datum funguje, ale nemusí být zkušeností zákazníků, který chcete poskytnout. V další části povolíte výběrem data prostřednictvím kalendáře.

## <a name="install-nuget-package-to-work-with-jquery-ui"></a>Nainstalujte balíček NuGet pro práci s uživatelského rozhraní JQuery

**Uživatelského rozhraní šťávy** balíček NuGet umožňuje snadné integrace uživatelského rozhraní JQuery pomůcky do webové aplikace. Tento balíček použít, že ji nainstalujte prostřednictvím balíčku NuGet.

![Přidat šťávy uživatelského rozhraní](integrating-jquery-ui/_static/image3.png)

Verze šťávy uživatelského rozhraní, která po instalaci může dojít ke konfliktu s verzí JQuery ve vaší aplikaci. Než budete pokračovat v tomto kurzu, opakujte spuštění aplikace. Pokud dojde k chybě jazyka JavaScript, budete muset sjednotit verze JQuery. Můžete přidat do složky skriptů (verze 1.8.2 v době psaní tohoto kurzu) očekávaná verze JQuery nebo Site.master zadejte cestu k souboru JQuery.

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a>Přizpůsobení šablony data a času zahrnout ovládací prvek Datepicker pomůcky

Ovládací prvek Datepicker pomůcky přidá do šablony dynamickými daty pro úpravy hodnotu data a času. Přidáním widgetu do šablony je automaticky vykreslen ve formuláři pro přidání nové student a v zobrazení mřížky pro úpravy studenty. Otevřete **data a času\_Edit.ascx**a přidejte následující zvýrazněný kód.

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

V souboru kódu na pozadí nastavíte minimální a maximální kalendářní data pro ovládací prvek DatePicker. Nastavte tyto hodnoty, bude uživatelům zabránit v přechodu pro neplatná data. Načte minimální a maximální hodnoty z **RangeAttribute** vlastnosti data a času, pokud je zadáno. Otevřete **data a času\_Edit.ascx.cs**a přidejte následující zvýrazněný kód na stránku\_Load – metoda.

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

Spuštění webové aplikace a přejděte na stránku AddStudent. Zadejte hodnoty pro pole a Všimněte si, že když kliknete na textové pole pro datum registrace, kalendář je zobrazen.

![Výběr data](integrating-jquery-ui/_static/image4.png)

Vyberte datum a klikněte na **vložit**. RangeAttribute vynucuje ověření na serveru. Nastavením vlastnosti minDate ovládací prvek Datepicker, můžete také použít ověření na straně klienta. Kalendáře neumožňuje uživateli, přejděte na datum před hodnotu minDate.

Pokud chcete upravit záznam v zobrazení mřížky, zobrazí se také kalendáře.

![Ovládací prvek DatePicker v GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a>Závěr

V tomto kurzu jste zjistili, jak začlenit JQuery widget do webové formuláře, který používá vazby modelu.

V dalším [kurzu](using-query-string-values-to-retrieve-data.md), při výběru dat použijete hodnotu řetězce dotazu.

> [!div class="step-by-step"]
> [Předchozí](sorting-paging-and-filtering-data.md)
> [další](using-query-string-values-to-retrieve-data.md)
