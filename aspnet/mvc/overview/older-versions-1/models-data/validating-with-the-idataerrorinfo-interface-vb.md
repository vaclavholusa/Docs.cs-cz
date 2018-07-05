---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
title: Ověřování v rozhraní IDataErrorInfo (VB) | Dokumentace Microsoftu
author: StephenWalther
description: Stephen Walther se dozvíte, jak zobrazit chybové zprávy ověření na vlastních implementací rozhraní IDataErrorInfo v třídě modelu.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 3a8a9d9f-82dd-4959-b7c6-960e9ce95df1
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 9faaab92b7baf0af5f0d8718e1d80e26c29bcca0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399471"
---
<a name="validating-with-the-idataerrorinfo-interface-vb"></a>Ověřování v rozhraní IDataErrorInfo (VB)
====================
podle [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther se dozvíte, jak zobrazit chybové zprávy ověření na vlastních implementací rozhraní IDataErrorInfo v třídě modelu.


Cílem tohoto kurzu je vysvětlit jedním z přístupů k provedení ověření v aplikaci ASP.NET MVC. Zjistíte, jak zabránit odeslání formuláře HTML bez zadání hodnot požadovaných polí. V tomto kurzu se dozvíte, jak provádět ověření pomocí rozhraní IErrorDataInfo.

## <a name="assumptions"></a>Předpoklady

V tomto kurzu použiju MoviesDB databáze a tabulky databáze filmů. Tato tabulka obsahuje následující sloupce:

<a id="0.6_table01"></a>


| **Název sloupce** | **Datový typ** | **Povolit hodnoty Null** |
| --- | --- | --- |
| ID | int | False |
| Název | Nvarchar(100) | False |
| Ředitel | Nvarchar(100) | False |
| DateReleased | DateTime | False |


V tomto kurzu pomocí Microsoft Entity Framework vytvořit Moje databáze třídy modelu. Třída film vygenerovaným rozhraním Entity Framework se zobrazí na obrázku 1.


[![Video entity](validating-with-the-idataerrorinfo-interface-vb/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image1.png)

**Obrázek 01**: entity The Movie ([kliknutím ji zobrazíte obrázek v plné velikosti](validating-with-the-idataerrorinfo-interface-vb/_static/image2.png))


> [!NOTE] 
> 
> Další informace o použití rozhraní Entity Framework pro generování tříd modelu vaší databáze najdete v tématu Moje kurz s názvem vytváření tříd modelu s použitím rozhraní Entity Framework.


## <a name="the-controller-class"></a>Třída Kontroleru

Používáme kontroler Home na filmy seznamu a vytvořit nové filmy. Kód pro tuto třídu je obsažen v informacích 1.

**Výpis 1 - Controllers\HomeController.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample1.vb)]

Třída kontroleru Domovská stránka v informacích 1 obsahuje dvě Create() akce. První akci zobrazí formulář HTML pro vytvoření nové video. Druhou akci Create() provádí skutečné vložit tento nový film do databáze. Druhou akci Create() je voláno, když se odešle formulář zobrazí první Create() akcí na server.

Všimněte si, že druhou akci Create() obsahuje následující řádky kódu:

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample2.vb)]

Vlastnost IsValid vrátí hodnotu false, když dojde k chybě ověřování. V takovém případě se zobrazí znovu vytvořit zobrazení, která obsahuje formulář HTML pro vytváření videa.

## <a name="creating-a-partial-class"></a>Vytvoření částečné třídy

Třída Video je vygenerovaným rozhraním Entity Framework. Pokud rozbalte soubor MoviesDBModel.edmx v okně Průzkumník řešení a otevřete soubor MoviesDBModel.Designer.vb v editoru kódu vidíte kód pro třídu Movie (viz obrázek 2).


[![Kód pro entitu Movie](validating-with-the-idataerrorinfo-interface-vb/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image3.png)

**Obrázek 02**: kód pro entitu Movie ([kliknutím ji zobrazíte obrázek v plné velikosti](validating-with-the-idataerrorinfo-interface-vb/_static/image4.png))


Třída Video je částečnou třídu. To znamená, že můžeme přidat jiné částečné třídy se stejným názvem k rozšíření funkčnosti film třídy. Přidáme náš logiku ověřování novou třídu.

Přidáte třídu v informacích 2 do složky modely.

**Výpis 2 - Models\Movie.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample3.vb)]

Všimněte si, že třída v informacích 2 zahrnuje *částečné* modifikátor. Jakékoliv metody nebo vlastnosti, které přidáte do této třídy, se stanou součástí třídy film vygenerovaným rozhraním Entity Framework.

## <a name="adding-onchanging-and-onchanged-partial-methods"></a>Přidání OnChanging a OnChanged částečné metody

Když Entity Frameworku vygeneruje třídu entity, Entity Framework přidá částečné metody do třídy automaticky. Entity Framework generuje OnChanging a OnChanged částečné metody, které odpovídají každá vlastnost třídy.

V případě třídy film Entity Framework vytvoří následující metody:

- OnIdChanging
- OnIdChanged
- OnTitleChanging
- OnTitleChanged
- OnDirectorChanging
- OnDirectorChanged
- OnDateReleasedChanging
- OnDateReleasedChanged

Před změnou odpovídající vlastnosti je volána metoda OnChanging vpravo. Po změně vlastnosti je volána metoda OnChanged vpravo.

Můžete využít výhod těchto částečné metody třídy film přidat logiku ověřování. Aktualizace třídy filmu v informacích 3 ověřuje, že nadpis a ředitel pro vlastnosti se přiřazují neprázdných hodnot.

> [!NOTE] 
> 
> Částečná metoda je metoda definovaná ve třídě, není nutná pro implementaci. Pokud není implementovat částečnou metodu kompilátor odebere podpis metody a všechny volání metody tady nejsou žádné běhové náklady spojené s částečnou metodu. V editoru Visual Studio Code, můžete přidat částečná metoda zadáním klíčového slova *částečné* následovaný mezerami, chcete-li zobrazit seznam částečných zobrazení k implementaci.


**Výpis 3 - Models\Movie.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample4.vb)]

Například pokud se pokusíte přiřadit vlastnosti název prázdný řetězec, chybovou zprávu přiřadí se mu na slovník s názvem \_chyby.

V tomto okamžiku nic ve skutečnosti se stane, když přiřadíte prázdný řetězec pro vlastnost názvu a chyba se přidá do soukromého \_pole chyby. Musíme implementovat v rozhraní IDataErrorInfo umožní vystavit tyto chyby ověření do rozhraní ASP.NET MVC.

## <a name="implementing-the-idataerrorinfo-interface"></a>Implementace rozhraní IDataErrorInfo

V rozhraní IDataErrorInfo byl součástí rozhraní .NET framework od první verze. Toto rozhraní je velmi jednoduché rozhraní:

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample5.vb)]

Pokud třída implementuje rozhraní IDataErrorInfo, rozhraní ASP.NET MVC bude používat toto rozhraní, při vytváření instance třídy. Například kontroler Home Create() akce přijímá instanci třídy filmu:

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample6.vb)]

Architektura ASP.NET MVC vytvoří instanci film předán Create() akce pomocí vazače modelu (DefaultModelBinder). Vazač modelu zodpovídá za vytvoření instance objektu film navázáním pole formuláře HTML na instanci objektu video.

DefaultModelBinder zjišťuje, zda třída implementuje rozhraní IDataErrorInfo. Pokud třída implementuje toto rozhraní vazače modelu vyvolá IDataErrorInfo.this indexeru pro každou vlastnost třídy. Pokud indexeru vrátí chybovou zprávu přidá vazač modelu tato chybová zpráva pro modelování stav automaticky.

DefaultModelBinder také kontroluje IDataErrorInfo.Error vlastnost. Tato vlastnost je určena k reprezentaci chyby ověřování podle jiné vlastnosti přidružené k třídě. Můžete například chtít vynutit ověřovací pravidlo, které závisí na hodnotách více vlastností třídy Video. V takovém případě by vrátit chybu ověření z vlastnosti chyby.

Aktualizované třídy filmu v informacích 4 implementuje rozhraní IDataErrorInfo.

**Část 4 – Models\Movie.vb (implementuje IDataErrorInfo)**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample7.vb)]

V informacích 4 kontroluje vlastnost indexeru \_předaný kolekce chyb, pokud obsahuje klíč, který odpovídá názvu vlastnosti indexeru. Pokud se nezobrazí žádná chyba ověření přidružený k vlastnosti je vrácen prázdný řetězec.

Není nutné upravovat kontroler Home žádným způsobem pomocí upravené třídy film. Stránky zobrazené na obrázku 3 znázorňuje, co se stane, když je zadána žádná hodnota pro název nebo ředitel pole formuláře.


[![Automatické vytváření metody akce](validating-with-the-idataerrorinfo-interface-vb/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image5.png)

**Obrázek 03**: formulář s chybějící hodnoty ([kliknutím ji zobrazíte obrázek v plné velikosti](validating-with-the-idataerrorinfo-interface-vb/_static/image6.png))


Všimněte si, že hodnota DateReleased proběhne automaticky. Protože vlastnost DateReleased nepřijímá hodnoty NULL, DefaultModelBinder Chyba ověřování pro tuto vlastnost automaticky při vygeneruje nemá hodnotu. Pokud chcete upravit chybové zprávy pro vlastnost DateReleased budete muset vytvořit vlastní vazač modelu.

## <a name="summary"></a>Souhrn

V tomto kurzu jste zjistili, jak používat rozhraní IDataErrorInfo ke generování chybových zpráv ověření. Nejprve jsme vytvořili částečné třídy film, který rozšiřuje funkce částečné třídy film vygenerovaným rozhraním Entity Framework. Dále jsme přidali logiku ověřování k film třídy OnTitleChanging() OnDirectorChanging() částečné metody a. Nakonec jsme implementovali v rozhraní IDataErrorInfo aby bylo možné zveřejnit tyto zprávy ověření na rozhraní ASP.NET MVC.

> [!div class="step-by-step"]
> [Předchozí](performing-simple-validation-vb.md)
> [další](validating-with-a-service-layer-vb.md)
