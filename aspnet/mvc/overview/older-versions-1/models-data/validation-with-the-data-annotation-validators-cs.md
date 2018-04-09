---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
title: Ověřování pomocí datové poznámky validátory (C#) | Microsoft Docs
author: microsoft
description: Výhodou vazač modelu dat poznámky k provedení ověření v rámci aplikace ASP.NET MVC. Naučte se používat s různými typy validátoru...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/29/2009
ms.topic: article
ms.assetid: 7ca8013e-9dfc-4e33-8336-cdccfd5f9414
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
msc.type: authoredcontent
ms.openlocfilehash: 0aca9472094e6a54c7b7cb4ad4f12df64fe12db2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="validation-with-the-data-annotation-validators-c"></a>Ověřování pomocí datové poznámky validátory (C#)
====================
podle [Microsoft](https://github.com/microsoft)

> Výhodou vazač modelu dat poznámky k provedení ověření v rámci aplikace ASP.NET MVC. Naučte se používat s různými typy validátoru atributy a pracovat s nimi v Microsoft Entity Framework.


V tomto kurzu zjistěte, jak používat validátory datové poznámky k provedení ověření v aplikaci ASP.NET MVC. Výhodou použití datové poznámky validátory je umožňují vám provést ověření, jednoduše přidáním jeden nebo více atributů – například požadované nebo StringLength atributu – vlastnost třídy.

Než budete moct použít validátory datové poznámky, je nutné stáhnout vazač modelu dat poznámky. Vazač vzorek dat poznámky modelu můžete stáhnout z webu CodePlex kliknutím [zde](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).


Je důležité si uvědomit, že vazač modelu dat poznámky není oficiální součástí Microsoft ASP.NET MVC framework. I když vazač modelu dat poznámky vytvořený týmem Microsoft ASP.NET MVC, Microsoft nenabízí oficiální produktovou podporu pro vazač modelu dat poznámky popsané a použili v tomto kurzu.


## <a name="using-the-data-annotation-model-binder"></a>Pomocí vazače modelu dat poznámky

Chcete-li použít vazač modelu anotací dat v aplikaci ASP.NET MVC, musíte nejprve přidat odkaz na sestavení Microsoft.Web.Mvc.DataAnnotations.dll a System.ComponentModel.DataAnnotations.dll sestavení. Vyberte možnost nabídky **projektu, přidat odkaz na**. Pak klikněte na **Procházet** kartě a přejděte do umístění, které jste stáhli (a rozbalené) ukázková Data vazač modelu poznámky (v tématu **obrázek 1**).

[![](validation-with-the-data-annotation-validators-cs/_static/image2.png)](validation-with-the-data-annotation-validators-cs/_static/image1.png)

**Obrázek 1**: Přidání odkazu na vazač modelu dat poznámky ([Kliknutím zobrazit obrázek v plné velikosti](validation-with-the-data-annotation-validators-cs/_static/image3.png))

Vyberte sestavu Microsoft.Web.Mvc.DataAnnotations.dll i System.ComponentModel.DataAnnotations.dll sestavení a klikněte na **OK** tlačítko.


Nelze použít System.ComponentModel.DataAnnotations.dll sestavení součástí rozhraní .NET Framework Service Pack 1 vazač modelu dat poznámky. Musíte použít verzi sestavení System.ComponentModel.DataAnnotations.dll součástí ke stažení ukázky vazač modelu dat poznámky.


Nakonec budete muset registraci vazač modelu DataAnnotations v souboru Global.asax. Přidejte následující řádek kódu do aplikace\_Start() obslužné rutiny události tak, aby aplikace\_metodu Start() vypadá takto:

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample1.cs)]

Tento řádek kódu zaregistruje ataAnnotationsModelBinder jako výchozí vazač modelu pro celou aplikaci ASP.NET MVC.

## <a name="using-the-data-annotation-validator-attributes"></a>Pomocí atributů ověření dat poznámky

Pokud používáte vazač modelu dat poznámky, použijete validátoru atributy provést ověření. Obor názvů System.ComponentModel.DataAnnotations zahrnuje následující atributy ověření:

- V rozsahu – umožňuje ověřit, zda hodnota vlastnosti leží mezi zadaný rozsah hodnot.
- Regulární výraz – umožňuje ověřit, zda hodnota vlastnosti odpovídá zadanému regulárnímu výrazu vzoru.
- Požadované – umožňuje označit vlastnost jako povinnou.
- StringLength – umožňuje určit maximální délku ve vlastnosti string.
- Ověření – základní třída pro všechny atributy validátoru.

> [!NOTE] 
> 
> Pokud vaše potřeby ověření nejsou splňuje jakákoliv standardní validátory vždy máte možnost vytvořit vlastní validátor atribut dědění nový atribut validátoru ze základní ověřovací atribut.


Třída produktu v **výpis 1** ukazuje, jak se používají tyto atributy validátoru. Název, popis a UnitPrice vlastnosti jsou označené podle potřeby. Vlastnost název musí mít délku řetězce, která je menší než 10 znaků. Vlastnost UnitPrice musí odpovídat nakonec vzor regulárního výrazu, který představuje částku měny.

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample2.cs)]

**Výpis 1**: Models\Product.cs

Třída produktu ukazuje, jak používat jeden další atribut: atribut DisplayName. Atribut DisplayName umožňuje upravit název vlastnosti, když je vlastnost zobrazena v chybové zprávě. Místo zobrazení chybová zpráva "je požadované pole UnitPrice" můžete zobrazit chybová zpráva "cena pole je požadováno".

> [!NOTE] 
> 
> Pokud chcete úplně přizpůsobit chybové zprávy zobrazí validátor můžete přiřadit vlastní chybová zpráva pro vlastnost ErrorMessage validátoru takto: `<Required(ErrorMessage:="This field needs a value!")>`


Můžete použít třídu produktu v **výpis 1** s akce kontroleru Create() v **výpis 2**. Tato akce kontroleru znovu vytvořit zobrazení zobrazí, pokud stav modelu, který obsahuje všechny chyby.

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample3.cs)]

**Listing 2**: Controllers\ProductController.vb

Nakonec můžete vytvořit zobrazení v **výpis 3** akce Create() kliknete pravým tlačítkem a výběrem možnosti nabídky **přidat zobrazení**. Vytvoření zobrazení silného typu pomocí třídy produktu jako třídu modelu. Vyberte **vytvořit** z zobrazení obsahu rozevíracího seznamu (viz **na obrázku 2**).

[![](validation-with-the-data-annotation-validators-cs/_static/image5.png)](validation-with-the-data-annotation-validators-cs/_static/image4.png)

**Obrázek 2**: Přidání zobrazení pro vytváření

[!code-aspx[Main](validation-with-the-data-annotation-validators-cs/samples/sample4.aspx)]

**Listing 3**: Views\Product\Create.aspx

> [!NOTE] 
> 
> Odebrat z formuláře vytvořit generované pole Id **přidat zobrazení** možnost nabídky. Protože v poli Id odpovídá sloupec Identity, nechcete povolit uživatelům zadat hodnotu pro toto pole.


Pokud odesláním formuláře pro vytváření produktu a nezadáte hodnoty pro povinná pole, pak chybu ověření zprávy v **obrázek 3** jsou zobrazeny.

[![](validation-with-the-data-annotation-validators-cs/_static/image7.png)](validation-with-the-data-annotation-validators-cs/_static/image6.png)

**Obrázek 3**: Chybí povinná pole

Pokud zadáte neplatný měna dobu a potom chybovou zprávu ve **obrázek 4** se zobrazí.

[![](validation-with-the-data-annotation-validators-cs/_static/image9.png)](validation-with-the-data-annotation-validators-cs/_static/image8.png)

**Obrázek 4**: Neplatná měna velikost

## <a name="using-data-annotation-validators-with-the-entity-framework"></a>Validátory poznámky Data pomocí rozhraní Entity Framework

Pokud používáte Microsoft Entity Framework pro generování tříd modelu dat nelze použít atributy validátoru přímo do vaší třídy. Protože Entity Framework Designer generuje třídy modelu, všechny změny provedené u třídy modelu budou přepsány při příštím provedení změn v návrháři.

Pokud chcete použít validátory s třídami generované rozhraní Entity Framework budete muset vytvořit datové třídy metadat. Validátory můžete použít k třídě meta data místo použití validátory ke skutečné třídě.

Představte si například, že jste vytvořili film třídy pomocí rozhraní Entity Framework (viz **obrázek 5**). Představte si kromě toho, že chcete, aby název filmu a ředitel vlastnosti požadované vlastnosti. V takovém případě můžete vytvořit částečné třídy a třídy dat meta v **výpis 4**.

[![](validation-with-the-data-annotation-validators-cs/_static/image11.png)](validation-with-the-data-annotation-validators-cs/_static/image10.png)

**Obrázek 5**: Třída film generované Entity Framework

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample5.cs)]

**Výpis 4**: Models\Movie.cs

Soubor v **výpis 4** obsahuje dvě třídy s názvem film a MovieMetaData. Třída film je konkrétní třídu. Odpovídá třídu generované Entity Frameworku, který je obsažen v souboru DataModel.Designer.vb.

Rozhraní .NET framework v současné době nepodporuje částečné vlastnosti. Proto neexistuje žádný způsob, jak použít atributy ověření pro vlastnosti film třídy definované v souboru DataModel.Designer.vb aplikací atributů ověření vlastnosti třídy filmu, které jsou definované v souboru v **výpis 4**.

Všimněte si, že třídu film opatřen atributem MetadataType, která odkazuje na třídu MovieMetaData. Třída MovieMetaData obsahuje vlastnosti proxy pro vlastnosti třídy film.

Validátor atributy jsou použity k vlastnostem třídy rozhraní MovieMetaData. Název, ředitele a DateReleased vlastnosti jsou označeny jako požadované vlastnosti. Vlastnost ředitel musí být přiřazen řetězec, který obsahuje méně než 5 znaků. Nakonec je použit atribut DisplayName vlastnost DateReleased zobrazíte chybovou zprávu jako "datum vydání pole je povinné." místo chyba "DateReleased pole je požadováno."

> [!NOTE] 
> 
> Všimněte si, že proxy vlastnosti ve třídě MovieMetaData, není nutné představují stejné typy jako odpovídající vlastnosti ve třídě film. Vlastnost ředitel je například řetězcová vlastnost ve třídě film a vlastnosti objektu ve třídě MovieMetaData.


Stránka v **obrázek 6** znázorňuje chybové zprávy vracené při zadání neplatné hodnoty pro vlastnosti film.

[![](validation-with-the-data-annotation-validators-cs/_static/image13.png)](validation-with-the-data-annotation-validators-cs/_static/image12.png)

**Obrázek 6**: validátory pomocí rozhraní Entity Framework ([Kliknutím zobrazit obrázek v plné velikosti](validation-with-the-data-annotation-validators-cs/_static/image14.png))

## <a name="summary"></a>Souhrn

V tomto kurzu jste zjistili, jak chcete využít výhod vazač modelu dat poznámky k provedení ověření v rámci aplikace ASP.NET MVC. Jste zjistili, jak používat různé typy ověření jako je povinná a StringLength atributů. Také jste zjistili, jak používat tyto atributy při práci se službou Microsoft Entity Framework.

> [!div class="step-by-step"]
> [Předchozí](validating-with-a-service-layer-cs.md)
> [další](creating-model-classes-with-the-entity-framework-vb.md)
