---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
title: "Ověřování s idataerrorinfo – rozhraní (VB) | Microsoft Docs"
author: StephenWalther
description: "Stephen Walther ukazuje způsob zobrazení chybové zprávy ověření na vlastní implementací rozhraní idataerrorinfo – ve třídu modelu."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 3a8a9d9f-82dd-4959-b7c6-960e9ce95df1
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 1439d470a7fa3cb1171dbdd0b7eec6a6aa52912d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="validating-with-the-idataerrorinfo-interface-vb"></a>Ověřování s idataerrorinfo – rozhraní (VB)
====================
podle [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther ukazuje způsob zobrazení chybové zprávy ověření na vlastní implementací rozhraní idataerrorinfo – ve třídu modelu.


Cílem tohoto kurzu je vysvětlit, jeden z přístupů k provádění ověření v aplikaci ASP.NET MVC. Zjistíte, jak zabránit odeslání formuláře HTML bez zadání hodnoty povinných polí formuláře. V tomto kurzu zjistěte, jak provádět ověření pomocí rozhraní IErrorDataInfo.

## <a name="assumptions"></a>Předpoklady

V tomto kurzu budete používám MoviesDB databáze a tabulky databáze filmy. Tato tabulka obsahuje následující sloupce:

<a id="0.6_table01"></a>


| **Název sloupce** | **Datový typ** | **Povolit hodnoty Null** |
| --- | --- | --- |
| ID | celá čísla | False |
| Název | Nvarchar(100) | False |
| Adresář nacházející | Nvarchar(100) | False |
| DateReleased | DateTime | False |


V tomto kurzu používám vygenerovat Moje databáze třídy modelu Microsoft Entity Framework. Třída film generované rozhraní Entity Framework se zobrazí na obrázku 1.


[![Film entity](validating-with-the-idataerrorinfo-interface-vb/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image1.png)

**Obrázek 01**: film entity ([Kliknutím zobrazit obrázek v plné velikosti](validating-with-the-idataerrorinfo-interface-vb/_static/image2.png))


> [!NOTE] 
> 
> Další informace o používání rozhraní Entity Framework pro generování tříd modelu databáze, získáte v že mé kurzu názvem vytváření třídy modelu s použitím Entity Framework.


## <a name="the-controller-class"></a>Třída Kontroleru

Jsme použil řadič domovské na seznamu filmy a vytvořit nové filmy. Kód pro tuto třídu je obsažený v výpis 1.

**Výpis 1 - Controllers\HomeController.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample1.vb)]

Třída kontroleru domovské v výpis 1 obsahuje dvě Create() akce. Je první akcí zobrazí formulář HTML pro vytvoření nové film. Druhou akci Create() provede skutečné vložení nové filmu do databáze. Druhou akci Create() je volána, když se zobrazí první Create() akce formuláře je odeslána na server.

Všimněte si, že druhou akci Create() obsahuje následující řádky kódu:

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample2.vb)]

IsValid – vlastnost vrací hodnotu false, když dojde k chybě ověření. V takovém případě se zobrazí znovu vytvořit zobrazení, které obsahuje formulář HTML pro vytváření film.

## <a name="creating-a-partial-class"></a>Vytvoření částečné třídy

Třída film je generován rozhraní Entity Framework. Zobrazí kód pro třídu film Pokud rozbalte soubor MoviesDBModel.edmx v okně Průzkumníka řešení a otevřete soubor MoviesDBModel.Designer.vb v editoru kódu (viz obrázek 2).


[![Kód pro entitu filmu](validating-with-the-idataerrorinfo-interface-vb/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image3.png)

**Obrázek 02**: kód pro entitu Movie ([Kliknutím zobrazit obrázek v plné velikosti](validating-with-the-idataerrorinfo-interface-vb/_static/image4.png))


Třída film je konkrétní třídu. To znamená, že jsme přidejte jinou třídu se stejným názvem rozšířit funkce film třídy. Přidáme ověřovací logiku novou třídu.

Přidání třídy v výpis 2 do složky modelů.

**Výpis 2 - Models\Movie.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample3.vb)]

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


**Výpis 3 - Models\Movie.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample4.vb)]

Například pokud se pokusíte přiřadit prázdný řetězec pro vlastnost název, potom chybovou zprávu je přiřazen do slovníku nazvaného \_chyby.

V tomto okamžiku nic ve skutečnosti se stane, když přiřadíte prázdný řetězec pro vlastnost název a chyba se přidá privátní \_chyby pole. Musíme implementovat rozhraní idataerrorinfo – vystavit tyto chyby ověření do rozhraní ASP.NET MVC.

## <a name="implementing-the-idataerrorinfo-interface"></a>Implementace idataerrorinfo – rozhraní

Idataerrorinfo – rozhraní byl součástí rozhraní .NET framework od první verze. Toto rozhraní je velmi jednoduché rozhraní:

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample5.vb)]

Pokud třída implementuje idataerrorinfo – rozhraní, rozhraní ASP.NET MVC používat toto rozhraní při vytvoření instance třídy. Instance třídy film lze například řadičem domovské Create() akce:

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample6.vb)]

Rozhraní ASP.NET MVC vytvoří instanci film předaný akce Create() pomocí vazač modelu (DefaultModelBinder). Vazač modelu zodpovídá za vytvoření instance objektu film pomocí vytvoření vazby pole formuláře HTML na instanci objektu film.

DefaultModelBinder zjišťuje, zda třída implementuje idataerrorinfo – rozhraní. Pokud toto rozhraní implementuje třídu vazač modelu vyvolá IDataErrorInfo.this indexeru pro každou vlastnost třídy. Pokud indexeru vrátí chybovou zprávu přidá vazač modelu tato chybová zpráva pro modelování stavu automaticky.

DefaultModelBinder také zkontroluje vlastnost IDataErrorInfo.Error. Tato vlastnost slouží k reprezentaci konkrétní ověření jiných vlastností chyby související s třídou. Například můžete chtít vynutit ověřovací pravidlo, které závisí na hodnotách více vlastností třídy film. V takovém případě by vrátit chybu ověření z vlastnosti chyby.

Aktualizované film třídy ve výpisu 4 implementuje idataerrorinfo – rozhraní.

**Výpis 4 - Models\Movie.vb (implementuje idataerrorinfo –)**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample7.vb)]

Výpis 4 kontroluje vlastnost indexeru \_kolekce chyb, které chcete zobrazit, pokud obsahuje klíč, který odpovídá názvu vlastnosti předaný indexeru. Pokud se nezobrazí žádná chyba ověření přidružená vlastnost je vrácen prázdný řetězec.

Není nutné upravit řadičem domovské žádným způsobem použití upravené třídy film. Stránka zobrazená na obrázku 3 znázorňuje, co se stane, když je pro pole formuláře název nebo ředitel zadána žádná hodnota.


[![Automatické vytváření metody akce](validating-with-the-idataerrorinfo-interface-vb/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image5.png)

**Obrázek 03**: formulář s chybějící hodnoty ([Kliknutím zobrazit obrázek v plné velikosti](validating-with-the-idataerrorinfo-interface-vb/_static/image6.png))


Všimněte si, že automaticky ověření DateReleased hodnoty. Vzhledem k tomu, že vlastnost DateReleased nepřijímá hodnoty NULL, DefaultModelBinder generuje automaticky chybu ověření pro tuto vlastnost tak, pokud nemá hodnotu uvedené. Pokud chcete upravit chybovou zprávu pro vlastnost DateReleased budete muset vytvořit vlastní vazač modelu.

## <a name="summary"></a>Souhrn

V tomto kurzu jste se naučili idataerrorinfo – rozhraní použít ke generování chybových zpráv ověření. Nejdřív jsme vytvořili částečné film třídu, která rozšiřuje funkce film třídu generované rozhraní Entity Framework. V dalším kroku jsme přidali logiku ověření film třída OnTitleChanging() a OnDirectorChanging() částečné metody. Nakonec implementovali jsme idataerrorinfo – rozhraní tak, aby získal tyto zprávy ověření na rozhraní ASP.NET MVC.

>[!div class="step-by-step"]
[Předchozí](performing-simple-validation-vb.md)
[další](validating-with-a-service-layer-vb.md)
