---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
title: Ověřování s idataerrorinfo – rozhraní (C#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther ukazuje způsob zobrazení chybové zprávy ověření na vlastní implementací rozhraní idataerrorinfo – ve třídu modelu.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 4733b9f1-9999-48fb-8b73-6038fbcc5ecb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: b5028b2e07c4144efa59824885ce96cd8b037dff
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="validating-with-the-idataerrorinfo-interface-c"></a>Ověřování s idataerrorinfo – rozhraní (C#)
====================
podle [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther ukazuje způsob zobrazení chybové zprávy ověření na vlastní implementací rozhraní idataerrorinfo – ve třídu modelu.


Cílem tohoto kurzu je vysvětlit, jeden z přístupů k provádění ověření v aplikaci ASP.NET MVC. Zjistíte, jak zabránit odeslání formuláře HTML bez zadání hodnoty povinných polí formuláře. V tomto kurzu zjistěte, jak provádět ověření pomocí rozhraní IErrorDataInfo.

## <a name="assumptions"></a>Předpoklady

V tomto kurzu budete používám MoviesDB databáze a tabulky databáze filmy. Tato tabulka obsahuje následující sloupce:

<a id="0.5_table01"></a>


| **Název sloupce** | **Datový typ** | **Povolit hodnoty Null** |
| --- | --- | --- |
| ID | celá čísla | False |
| Název | Nvarchar(100) | False |
| Adresář nacházející | Nvarchar(100) | False |
| DateReleased | DateTime | False |


V tomto kurzu používám vygenerovat Moje databáze třídy modelu Microsoft Entity Framework. Třída film generované rozhraní Entity Framework se zobrazí na obrázku 1.


[![Film entity](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)

**Obrázek 01**: film entity ([Kliknutím zobrazit obrázek v plné velikosti](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))


> [!NOTE] 
> 
> Další informace o používání rozhraní Entity Framework pro generování tříd modelu databáze, získáte v že mé kurzu názvem vytváření třídy modelu s použitím Entity Framework.


## <a name="the-controller-class"></a>Třída Kontroleru

Jsme použil řadič domovské na seznamu filmy a vytvořit nové filmy. Kód pro tuto třídu je obsažený v výpis 1.

**Výpis 1 - Controllers\HomeController.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample1.cs)]

Třída kontroleru domovské v výpis 1 obsahuje dvě Create() akce. Je první akcí zobrazí formulář HTML pro vytvoření nové film. Druhou akci Create() provede skutečné vložení nové filmu do databáze. Druhou akci Create() je volána, když se zobrazí první Create() akce formuláře je odeslána na server.

Všimněte si, že druhou akci Create() obsahuje následující řádky kódu:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample2.cs)]

IsValid – vlastnost vrací hodnotu false, když dojde k chybě ověření. V takovém případě se zobrazí znovu vytvořit zobrazení, které obsahuje formulář HTML pro vytváření film.

## <a name="creating-a-partial-class"></a>Vytvoření částečné třídy

Třída film je generován rozhraní Entity Framework. Zobrazí kód pro třídu film Pokud rozbalte soubor MoviesDBModel.edmx v okně Průzkumníka řešení a otevřete soubor MoviesDBModel.Designer.cs v editoru kódu (viz obrázek 2).


[![Kód pro entitu filmu](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)

**Obrázek 02**: kód pro entitu Movie ([Kliknutím zobrazit obrázek v plné velikosti](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))


Třída film je konkrétní třídu. To znamená, že jsme přidejte jinou třídu se stejným názvem rozšířit funkce film třídy. Přidáme ověřovací logiku novou třídu.

Přidání třídy v výpis 2 do složky modelů.

**Výpis 2 - Models\Movie.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample3.cs)]

Všimněte si, že zahrnuje třídy ve výpisu 2 *částečné* modifikátor. Žádné metody nebo vlastnosti, které přidáte do této třídy stát součástí třídy film generované rozhraní Entity Framework.

## <a name="adding-onchanging-and-onchanged-partial-methods"></a>Přidání OnChanging a OnChanged částečné metody

Pokud rozhraní Entity Framework vygeneruje třídu entity, rozhraní Entity Framework přidá částečné metody do třídy automaticky. Rozhraní Entity Framework generuje OnChanging a OnChanged částečné metody, které odpovídají každé vlastnosti třídy.

V případě film třídu Entity Framework vytvoří následující metody:

- OnIdChanging
- OnIdChanged
- OnTitleChanging
- OnTitleChanged
- OnDirectorChanging
- OnDirectorChanged
- OnDateReleasedChanging
- OnDateReleasedChanged

OnChanging metoda je volána správné, než je odpovídající vlastnost změnit. Po změně vlastnost, je volána metoda OnChanged správné.

Můžete využít výhod těchto částečné metody třídy film přidat logiku ověření. Aktualizace třídy film v výpis 3 ověřuje, že vlastnosti nadpisu a ředitel přiřazené neprázdných hodnot.

> [!NOTE] 
> 
> Částečné metody je metoda definovaný ve třídě, která nepotřebujete k implementaci. Pokud nemáte implementovat částečné metodu kompilátor odebere podpis metody a všechna volání do metody tedy s sebou se neplatí běhu přidružené k částečné metodě. V editoru Visual Studio Code, můžete přidat částečné metoda zadáním klíčové slovo *částečné* následované mezerou zobrazíte seznam částečné k implementaci.


**Výpis 3 - Models\Movie.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample4.cs)]

Například pokud se pokusíte přiřadit prázdný řetězec pro vlastnost název, potom chybovou zprávu je přiřazen do slovníku nazvaného \_chyby.

V tomto okamžiku nic ve skutečnosti se stane, když přiřadíte prázdný řetězec pro vlastnost název a chyba se přidá privátní \_chyby pole. Musíme implementovat rozhraní idataerrorinfo – vystavit tyto chyby ověření do rozhraní ASP.NET MVC.

## <a name="implementing-the-idataerrorinfo-interface"></a>Implementace idataerrorinfo – rozhraní

Idataerrorinfo – rozhraní byl součástí rozhraní .NET framework od první verze. Toto rozhraní je velmi jednoduché rozhraní:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample5.cs)]

Pokud třída implementuje idataerrorinfo – rozhraní, rozhraní ASP.NET MVC používat toto rozhraní při vytvoření instance třídy. Instance třídy film lze například řadičem domovské Create() akce:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample6.cs)]

Rozhraní ASP.NET MVC vytvoří instanci film předaný akce Create() pomocí vazač modelu (DefaultModelBinder). Vazač modelu zodpovídá za vytvoření instance objektu film pomocí vytvoření vazby pole formuláře HTML na instanci objektu film.

DefaultModelBinder zjišťuje, zda třída implementuje idataerrorinfo – rozhraní. Pokud toto rozhraní implementuje třídu vazač modelu vyvolá IDataErrorInfo.this indexeru pro každou vlastnost třídy. Pokud indexeru vrátí chybovou zprávu přidá vazač modelu tato chybová zpráva pro modelování stavu automaticky.

DefaultModelBinder také zkontroluje vlastnost IDataErrorInfo.Error. Tato vlastnost slouží k reprezentaci konkrétní ověření jiných vlastností chyby související s třídou. Například můžete chtít vynutit ověřovací pravidlo, které závisí na hodnotách více vlastností třídy film. V takovém případě by vrátit chybu ověření z vlastnosti chyby.

Aktualizované film třídy ve výpisu 4 implementuje idataerrorinfo – rozhraní.

**Výpis 4 - Models\Movie.cs (implementuje idataerrorinfo –)**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample7.cs)]

Výpis 4 kontroluje vlastnost indexeru \_kolekce chyb, které chcete zobrazit, pokud obsahuje klíč, který odpovídá názvu vlastnosti předaný indexeru. Pokud se nezobrazí žádná chyba ověření přidružená vlastnost je vrácen prázdný řetězec.

Není nutné upravit řadičem domovské žádným způsobem použití upravené třídy film. Stránka zobrazená na obrázku 3 znázorňuje, co se stane, když je pro pole formuláře název nebo ředitel zadána žádná hodnota.


[![Automatické vytváření metody akce](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)

**Obrázek 03**: formulář s chybějící hodnoty ([Kliknutím zobrazit obrázek v plné velikosti](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))


Všimněte si, že automaticky ověření DateReleased hodnoty. Vzhledem k tomu, že vlastnost DateReleased nepřijímá hodnoty NULL, DefaultModelBinder generuje automaticky chybu ověření pro tuto vlastnost tak, pokud nemá hodnotu uvedené. Pokud chcete upravit chybovou zprávu pro vlastnost DateReleased budete muset vytvořit vlastní vazač modelu.

## <a name="summary"></a>Souhrn

V tomto kurzu jste se naučili idataerrorinfo – rozhraní použít ke generování chybových zpráv ověření. Nejdřív jsme vytvořili částečné film třídu, která rozšiřuje funkce film třídu generované rozhraní Entity Framework. V dalším kroku jsme přidali logiku ověření film třída OnTitleChanging() a OnDirectorChanging() částečné metody. Nakonec implementovali jsme idataerrorinfo – rozhraní tak, aby získal tyto zprávy ověření na rozhraní ASP.NET MVC.

> [!div class="step-by-step"]
> [Předchozí](performing-simple-validation-cs.md)
> [další](validating-with-a-service-layer-cs.md)
