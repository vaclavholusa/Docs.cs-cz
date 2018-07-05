---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
title: Ověřování validátory datových poznámek (VB) | Dokumentace Microsoftu
author: microsoft
description: Využijte výhod vazač modelu dat poznámky k provedení ověření v rámci aplikace ASP.NET MVC. Zjistěte, jak použít různé typy ověřování...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/29/2009
ms.topic: article
ms.assetid: 0d23ff2b-f2ec-434a-be3b-1180beeccba3
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
msc.type: authoredcontent
ms.openlocfilehash: 94410d0e8ad132a4e7a31ac01163663b597c3225
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391415"
---
<a name="validation-with-the-data-annotation-validators-vb"></a>Ověřování validátory datových poznámek (VB)
====================
podle [Microsoft](https://github.com/microsoft)

> Využijte výhod vazač modelu dat poznámky k provedení ověření v rámci aplikace ASP.NET MVC. Zjistěte, jak použít různé typy atributů ověřovacího modulu a pracovat s nimi v Entity Framework společnosti Microsoft.


V tomto kurzu se dozvíte, jak používat validátory dat. Poznámka k provedení ověření v aplikaci ASP.NET MVC. Výhodou použití validátory anotace dat je, že umožňují provést ověření, jednoduše tak, že přidáte atribut StringLength nebo jeden nebo více atributů – například požadované – pro vlastnost třídy.

Před použitím validátory Data poznámky, je nutné stáhnout vazač modelu dat poznámky. Ukázky vazače modelu poznámky dat si můžete stáhnout z webu CodePlex kliknutím [tady](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).


Je důležité pochopit, že vazač modelu anotací dat není oficiálním součást rozhraní Microsoft ASP.NET MVC. I když vazač modelu dat poznámky vytvořil tým Microsoft ASP.NET MVC, Microsoft nenabízí oficiální technická podpora pro vazač modelu dat poznámky popsané a použité v tomto kurzu.


## <a name="using-the-data-annotation-model-binder"></a>Použití vazače modelu dat poznámky

Chcete-li použít vazač modelu anotací dat v aplikaci ASP.NET MVC, musíte nejprve přidat odkaz na sestavení Microsoft.Web.Mvc.DataAnnotations.dll a System.ComponentModel.DataAnnotations.dll sestavení. Vyberte možnost nabídky **projektu, přidejte odkaz**. Klepnutím na tlačítko **Procházet** kartu a přejděte do umístění, které jste stáhli (a odblokujte) ukázka vazač modelu dat poznámky (naleznete v tématu **obrázek 1**).

[![](validation-with-the-data-annotation-validators-vb/_static/image2.png)](validation-with-the-data-annotation-validators-vb/_static/image1.png)

**Obrázek 1**: Přidání odkazu do vazače modelu dat poznámky ([kliknutím ji zobrazíte obrázek v plné velikosti](validation-with-the-data-annotation-validators-vb/_static/image3.png))

Vyberte Microsoft.Web.Mvc.DataAnnotations.dll sestavení a sestavení System.ComponentModel.DataAnnotations.dll a klikněte na tlačítko **OK** tlačítko.


Nelze použít System.ComponentModel.DataAnnotations.dll sestavení součástí .NET Framework Service Pack 1 s vazač modelu dat poznámky. Musíte použít verzi sestavení System.ComponentModel.DataAnnotations.dll součástí ke stažení ukázková Data poznámky pro vazač modelu.


Nakonec budete muset zaregistrovat DataAnnotations vazače modelu v souboru Global.asax. Přidejte následující řádek kódu do aplikace\_obslužná rutina události Start() tak, aby aplikace\_metodu Start() vypadá takto:

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample1.vb)]

Tento řádek kódu DataAnnotationsModelBinder zaregistruje jako výchozí vazač modelu pro celou aplikaci ASP.NET MVC.

## <a name="using-the-data-annotation-validator-attributes"></a>Pomocí atributů program pro ověření dat poznámky

Při použití vazače modelu dat poznámky pomocí atributů ověřovacího modulu provést ověření. Obor názvů System.ComponentModel.DataAnnotations zahrnuje následující atributy program pro ověření:

- V rozsahu – umožňuje ověřit, jestli hodnota vlastnosti leží mezi zadaný rozsah hodnot.
- ReqularExpression – umožňuje ověřit, jestli hodnota vlastnosti odpovídá zadanému regulárnímu výrazu vzoru.
- Požadováno – umožňuje označit vlastnost jako povinnou.
- StringLength – umožňuje určit maximální délka pro vlastnosti typu string.
- Ověřování – základní třída pro všechny atributy program pro ověření.

> [!NOTE] 
> 
> Při splnění potřeb ověřování nejsou žádné standardní validátory vždy máte možnost vytvořit vlastní validátor atribut děděním nový atribut ověření z atributu základní ověřování.


Třída produktu v **výpis 1** ukazuje způsob použití těchto atributů ověřovacího modulu. Název, popis a UnitPrice vlastnosti jsou označeny podle potřeby. Vlastnost Name musí mít délku řetězce, který je kratší než 10 znaků. A konečně vlastnost UnitPrice musí odpovídat vzoru regulárního výrazu, který představuje částku měny.

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample2.vb)]

**Výpis 1**: Models\Product.vb

Třída produktu ukazuje, jak používat jeden další atribut: atribut DisplayName. Atributu DisplayName můžete změnit název vlastnosti, když je vlastnost zobrazena v chybové zprávě. Místo zobrazení chybová zpráva "je povinné pole UnitPrice" můžete zobrazit chybová zpráva "Price pole je povinné.".

> [!NOTE] 
> 
> Pokud chcete kompletně přizpůsobit zprávu o chybě zobrazený validátor můžete přiřadit vlastní chybová zpráva na vlastnost ErrorMessage validátoru takto: `<Required(ErrorMessage:="This field needs a value!")>`


Můžete použít třídu produktu v **výpis 1** s Create() akce kontroleru v **výpis 2**. Tato akce kontroleru znovu zobrazí zobrazení pro vytváření, když ze stavů modelu obsahuje nějaké chyby.

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample3.vb)]

**Výpis 2**: Controllers\ProductController.vb

Nakonec můžete vytvořit zobrazení v **výpis 3** kliknutím pravým tlačítkem na Create() akci a vyberte možnost nabídky **přidat zobrazení**. Vytvořte zobrazení silného typu pomocí třídy produktu jako třídy modelu. Vyberte **vytvořit** z zobrazení obsahu rozevíracího seznamu (viz **obrázek 2**).

[![](validation-with-the-data-annotation-validators-vb/_static/image5.png)](validation-with-the-data-annotation-validators-vb/_static/image4.png)

**Obrázek 2**: Přidání zobrazení pro vytváření

[!code-aspx[Main](validation-with-the-data-annotation-validators-vb/samples/sample4.aspx)]

**Listing 3**: Views\Product\Create.aspx

> [!NOTE] 
> 
> Id pole odebrat z formuláře vytvořit generovaných **přidat zobrazení** nabídky. Protože pole Id odpovídá sloupec Identity, které nechcete povolit uživatelům zadat hodnotu pro toto pole.


Odeslání formuláře pro vytváření produktu a nezadáte hodnoty pro požadované pole a pak chybu ověření zprávy v **obrázek 3** jsou zobrazeny.

[![](validation-with-the-data-annotation-validators-vb/_static/image7.png)](validation-with-the-data-annotation-validators-vb/_static/image6.png)

**Obrázek 3**: Chybí povinná pole.

Pokud zadáte neplatný peněžní částce pak chybová zpráva v **obrázek 4** se zobrazí.

[![](validation-with-the-data-annotation-validators-vb/_static/image9.png)](validation-with-the-data-annotation-validators-vb/_static/image8.png)

**Obrázek 4**: Neplatná peněžní částce

## <a name="using-data-annotation-validators-with-the-entity-framework"></a>Použití dat anotace validátory sadou Entity Framework

Pokud používáte Microsoft Entity Framework pro generování tříd modelu dat nelze použít atributy validátor přímo do vaší třídy. Protože Návrhář Entity Framework generuje třídy modelu, všechny změny, které provedete tříd modelu budou přepsány při příštím v Návrháři provedete nějaké změny.

Pokud chcete použít validátory s třídami vygenerovaným rozhraním Entity Framework budete muset vytvořit meta datových tříd. Použijete validátory na meta třída dat místo použití validátory ke skutečné třídě.

Představte si například, že jste vytvořili třídu film používající nástroj Entity Framework (viz **obrázek 5**). Představte si kromě toho, že chcete vytvořit název filmu a ředitel pro vlastnosti požadované vlastnosti. V takovém případě můžete vytvořit částečné třídy a meta třída dat v **výpis 4**.

[![](validation-with-the-data-annotation-validators-vb/_static/image11.png)](validation-with-the-data-annotation-validators-vb/_static/image10.png)

**Obrázek 5**: Třída film vygenerovaným rozhraním Entity Framework

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample5.vb)]

**Výpis 4**: Models\Movie.vb

Soubor v **výpis 4** obsahuje dvě třídy s názvem filmů a MovieMetaData. Třída Video je částečnou třídu. Odpovídá vygenerovaným rozhraním Entity Framework, která je součástí souboru DataModel.Designer.vb částečné třídy.

Rozhraní .NET framework v současné době nepodporuje částečné vlastnosti. Proto neexistuje žádný způsob, jak použití atributů ověření pro vlastnosti film třídy definované v souboru DataModel.Designer.vb použitím atributů ověřovacího modulu na vlastnosti třídy film definované v souboru v **výpis 4**.

Všimněte si, že video částečné třídy je doplněn o MetadataType atribut, který odkazuje na třídu MovieMetaData. MovieMetaData třída obsahuje vlastnosti proxy pro vlastnosti třídy Video.

Program pro ověření atributy jsou použity k vlastnostem třídy MovieMetaData. Vlastnosti nadpisu, ředitel a DateReleased jsou označeny jako požadované vlastnosti. Vlastnost ředitel, musíte být přiřazeni řetězec, který obsahuje méně než 5 znaků. Nakonec se atribut DisplayName použít pro vlastnost DateReleased zobrazit chybová zpráva jako "datum vydání pole je povinné." místo chyby "DateReleased pole je povinné."

> [!NOTE] 
> 
> Všimněte si, že proxy vlastnosti ve třídě MovieMetaData, není nutné představují stejné typy jako odpovídající vlastnosti ve třídě video. Vlastnost ředitel je například řetězcová vlastnost ve třídě filmů a vlastnosti objektu ve třídě MovieMetaData.


Na stránce v **obrázek 6** znázorňuje chybové zprávy vracené při zadání neplatné hodnoty pro vlastnosti video.

[![](validation-with-the-data-annotation-validators-vb/_static/image13.png)](validation-with-the-data-annotation-validators-vb/_static/image12.png)

**Obrázek 6**: validátory pomocí Entity Frameworku ([kliknutím ji zobrazíte obrázek v plné velikosti](validation-with-the-data-annotation-validators-vb/_static/image14.png))

## <a name="summary"></a>Souhrn

V tomto kurzu jste zjistili, jak využít výhod vazač modelu dat poznámky k provedení ověření v rámci aplikace ASP.NET MVC. Jste zjistili, jak používat různé typy atributů ověřovacího modulu, jako je například požadovaný a StringLength atributů. Také jste zjistili, jak pomocí těchto atributů při práci s rozhraním Entity Framework společnosti Microsoft.

> [!div class="step-by-step"]
> [Předchozí](validating-with-a-service-layer-vb.md)
