---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
title: 'Iterace #2 – zpřístupnění aplikace vypadat dobrý (C#) | Microsoft Docs'
author: microsoft
description: V této iteraci můžeme vylepšit vzhled aplikace Úprava výchozí stránky předlohy zobrazení ASP.NET MVC a stylů CSS.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f1173feb-11ee-4017-8f3f-86599ea6ae13
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
msc.type: authoredcontent
ms.openlocfilehash: cad28fb6ff02625674e59674d1ec08d52373c269
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871841"
---
<a name="iteration-2--make-the-application-look-nice-c"></a>Iterace #2 – zpřístupnění aplikace vypadat dobrý (C#)
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhněte si kód](iteration-2-make-the-application-look-nice-cs/_static/contactmanager_2_cs1.zip)

> V této iteraci můžeme vylepšit vzhled aplikace Úprava výchozí stránky předlohy zobrazení ASP.NET MVC a stylů CSS.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Vytvoření aplikace ASP.NET MVC správy kontaktů (C#)
  

Z této série kurzů využijeme celou aplikaci obraťte se na správu od začátku ukončíte. Obraťte se na správce aplikace umožňuje ukládat kontaktní údaje - názvy, telefonní čísla a e-mailové adresy – seznam osob.

Přes několikrát jsme sestavení aplikace. S každé iteraci jsme postupně zlepšení aplikace. Cílem tohoto více iterace přístupu je vám umožní pochopit důvod pro každé změně.

- Iterace #1 – Vytvoření aplikace. V první iteraci vytvoříme obraťte se na správce v nejjednodušší způsob, jak to možné. Nemůžeme přidat podporu pro základní databázových operací: vytvořit, číst, aktualizovat a odstranit (CRUD).

- Iterace #2 – zpřístupnění aplikace vypadat dobrý. V této iteraci můžeme vylepšit vzhled aplikace Úprava výchozí stránky předlohy zobrazení ASP.NET MVC a stylů CSS.

- Iterace #3 – přidání ověřování formuláře. V třetím iteraci přidáme ověření základní formulář. Jsme zabránit neoprávněným osobám v odeslání formuláře bez dokončení vyžadovaná pole formuláře. Můžeme také ověřit e-mailových adres a telefonních čísel.

- Iterace #4 - li aplikaci volně vázány. V této třetí iteraci jsme využít výhod několik softwaru vzory návrhu na bylo snazší spravovat a upravovat aplikace obraťte se na správce. Například můžeme Refaktorovat naše aplikace pro použití vzoru úložiště a vzoru vkládání závislostí.

- Iterace #5 – vytvoření testování částí. V páté iteraci jsme snadněji naše aplikace spravovat a upravovat přidáním testování částí. Jsme model třídy modelu našich dat a vytvářet testy částí pro naše řadiče a logiku ověření.

- Iterace #6 - použití vývoje řízeného testováním. V této šesté iteraci přidáme nové funkce pro naši aplikaci tak, že nejprve zápis testů částí a psaní kódu pro testování částí. V této iteraci přidáme kontaktní skupiny.

- Iterace #7 – přidání funkci Ajax. V sedmého iteraci jsme přidáním podpory pro Ajax zvýšit rychlost reakce a výkon aplikace.

## <a name="this-iteration"></a>Tato iterace

Cílem této iterace je ke zlepšení vzhled aplikace, obraťte se na správce. Obraťte se na správce v současné době používá výchozí stránky předlohy zobrazení ASP.NET MVC a stylů CSS (viz obrázek 1). Tyto tento t vypadat chybný, ale nejsou zobrazeny t chcete obraťte se na správce, aby vypadala stejně jako všechny ostatní webu ASP.NET MVC. Chcete nahradit tyto soubory vlastní soubory.


[![Dialogové okno Nový projekt](iteration-2-make-the-application-look-nice-cs/_static/image1.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image1.png)

**Obrázek 01**: vzhled výchozí aplikace ASP.NET MVC ([Kliknutím zobrazit obrázek v plné velikosti](iteration-2-make-the-application-look-nice-cs/_static/image2.png))


V této iterace I zabývat dva přístupy k vylepšení visual návrh naší aplikace. Nejprve I ukazují, jak chcete využít výhod galerii ASP.NET MVC návrhu ke stažení šablony bezplatnou návrhu ASP.NET MVC. Galerie ASP.NET MVC umožňuje vytvořit profesionální webovou aplikaci, bez jakékoli pracuje.

Rozhodli nechcete použít šablonu z Galerie návrhů MVC ASP.NET pro aplikaci obraťte se na správce. Místo toho měl vlastní návrh vytvořený firma odborníky v oblasti návrhu. V druhé části tohoto kurzu I vysvětlují, jak šlo s odborníky v oblasti návrhu společnosti k vytvoření konečný návrh architektury ASP.NET MVC.

## <a name="the-aspnet-mvc-design-gallery"></a>Galerie ASP.NET MVC

Galerie ASP.NET MVC je volným prostředkem od společnosti Microsoft. ASP.NET MVC Galerie je umístěn na následující adrese:

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

ASP.NET MVC návrhu galerie hostitelem kolekce návrhy volné webu, které byly vytvořené speciálně pro použití v projektu aplikace ASP.NET MVC. Návrhy se odeslaný členové komunity. Návštěvníky do galerie můžete volit pro své oblíbené návrhy (viz obrázek 2).


[![Dialogové okno Nový projekt](iteration-2-make-the-application-look-nice-cs/_static/image2.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image3.png)

**Obrázek 02**: ASP.NET MVC návrhu galerie ([Kliknutím zobrazit obrázek v plné velikosti](iteration-2-make-the-application-look-nice-cs/_static/image4.png))


Jak je napsaný v tomto kurzu, je nejoblíbenější návrhu v galerii návrh s názvem. října podle David Hauser. Tento návrh můžete použít pro projektu aplikace ASP.NET MVC pomocí následujících kroků:

1. Klikněte **Stáhnout** tlačítko October.zip soubor stáhnout do počítače.
2. Klikněte pravým tlačítkem na stažený soubor October.zip a klikněte na **Odblokovat** tlačítko (viz obrázek 3).
3. Rozbalte soubor do složky s názvem. října.
4. Vyberte všechny soubory ve složce DesignTemplate obsažené ve složce říjen, klikněte pravým tlačítkem na soubory a vyberte možnost nabídky **kopie**.
5. Klikněte pravým tlačítkem na uzel projektu ContactManager v okně Průzkumníka řešení Visual Studio a vyberte možnost nabídky **vložení** (viz obrázek 4).
6. Vyberte možnost nabídky Visual Studio **upravit, najít a nahradit, nahraďte rychlé** a nahraďte *[s názvem MyProjectName]* s *ContactManager* (viz obrázek 5).


[![Dialogové okno Nový projekt](iteration-2-make-the-application-look-nice-cs/_static/image3.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image5.png)

**Obrázek 03**: odblokování soubor stažený z webu ([Kliknutím zobrazit obrázek v plné velikosti](iteration-2-make-the-application-look-nice-cs/_static/image6.png))


[![Dialogové okno Nový projekt](iteration-2-make-the-application-look-nice-cs/_static/image4.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image7.png)

**Obrázek 04**: přepsání souborů v Průzkumníku řešení ([Kliknutím zobrazit obrázek v plné velikosti](iteration-2-make-the-application-look-nice-cs/_static/image8.png))


[![Dialogové okno Nový projekt](iteration-2-make-the-application-look-nice-cs/_static/image5.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image9.png)

**Obrázek 05**: nahrazení [ProjectName] ContactManager ([Kliknutím zobrazit obrázek v plné velikosti](iteration-2-make-the-application-look-nice-cs/_static/image10.png))


Po dokončení těchto kroků vaše webové aplikace bude používat nový design. Stránka na obrázku 6 znázorňuje vzhled obraťte se na správce aplikace s návrhem říjen.


[![Dialogové okno Nový projekt](iteration-2-make-the-application-look-nice-cs/_static/image6.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image11.png)

**Obrázek 06**: ContactManager se šablonou říjen ([Kliknutím zobrazit obrázek v plné velikosti](iteration-2-make-the-application-look-nice-cs/_static/image12.png))


## <a name="creating-a-custom-aspnet-mvc-design"></a>Vytváření návrhu vlastní ASP.NET MVC

ASP.NET MVC návrhu galerie má na dobrý výběr různých stylů. Galerie vám poskytne bezproblémově způsob, jak přizpůsobit vzhled aplikace ASP.NET MVC. A samozřejmě Galerie má velký výhodou je zcela zdarma.

Však může být zapotřebí vytvořit zcela jedinečný návrhu pro svůj web. V takovém případě má smysl pro práci s společnosti návrh webu. Rozhodli pro tento postup pro návrh jednotného přihlašování pro aplikace obraťte se na správce.

Můžu si správce kontaktujte z iterace č. 1 a poslali projektu do návrhu společnosti. Není vlastní sadě Visual Studio (shame na nich!), ale, že dodán t představovat problém. Jsou schopny stáhnout Microsoft Visual Web Developer zdarma z [ https://www.asp.net ](https://www.asp.net) webu a otevřete obraťte se na správce aplikace Visual Web Developer. Během několika dní že měl vytvořeného návrhu na obrázku 7.


[![Dialogové okno Nový projekt](iteration-2-make-the-application-look-nice-cs/_static/image7.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image13.png)

**Obrázek 07**: návrhu správce kontaktujte technologie ASP.NET MVC ([Kliknutím zobrazit obrázek v plné velikosti](iteration-2-make-the-application-look-nice-cs/_static/image14.png))


Nový design se skládal z hlavní dva soubory: nový soubor kaskádových stylů a nový soubor předlohové stránky zobrazení. Hlavní stránka zobrazení obsahuje rozložení a sdíleného obsahu pro zobrazení v aplikaci ASP.NET MVC. Například stránky předlohy zobrazení obsahuje záhlaví, navigační karty a zápatí stránky, která se zobrazí na obrázku 7. I přepsal stávající zobrazení stránky předlohy Site.Master ve složce Views\Shared pomocí nového souboru Site.Master společnosti návrhu

Návrh společnosti také vytvořit nové šablony stylů CSS a sadu bitové kopie. I tyto nové soubory umístit do složky obsahu a přepsal existující soubor Site.css. Měli byste umístit všechny statický obsah ve složce obsahu.

Všimněte si, že obsahuje nový design pro obraťte se na správce bitových kopií pro úpravy a odstraňování kontaktů. Upravit a odstranit obrázku zobrazí vedle jednotlivých kontaktů v tabulce HTML kontaktů.

Původně tyto odkazy, pomocí kterých se vykreslují HTML. Pomocník ActionLink() takto:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample1.aspx)]

Metoda Html.ActionLink() nepodporuje bitové kopie (metoda HTML zakóduje text odkazu z bezpečnostních důvodů). Proto I nahradit volání Html.ActionLink() pomocí volání Url.Action() takto:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample2.aspx)]

Html.ActionLink() metoda vykreslí celý hypertextový odkaz. V případě metody Url.Action() na druhé straně vykreslí jenom adresu URL bez &lt;&gt; značky.

Všimněte si kromě toho, že nový design obsahuje karty vybrané a nezaškrtnuté. Například na obrázku 8 **vytvořit nový kontakt** vybraná karta a **Moje kontakty** není vybraná karta.


[![Dialogové okno Nový projekt](iteration-2-make-the-application-look-nice-cs/_static/image8.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image15.png)

**Obrázek 08**: vybraná a zrušit karty ([Kliknutím zobrazit obrázek v plné velikosti](iteration-2-make-the-application-look-nice-cs/_static/image16.png))


Pro podporu vykreslování karty vybrané a nezaškrtnuté, vytvořené vlastní pomocné rutiny HTML s názvem MenuItemHelper. Tato pomocná metoda vykreslí buď &lt;li&gt; značka nebo &lt;li třídy = "vybrána"&gt; značky v závislosti na tom, jestli aktuální kontroleru a akce odpovídá názvu kontroleru a akce předán do pomocné rutiny. Kód pro MenuItemHelper je obsažený v výpis 1.

**Výpis 1 - Helpers\MenuItemHelper.cs**

[!code-csharp[Main](iteration-2-make-the-application-look-nice-cs/samples/sample3.cs)]

Třída TagBuilder MenuItemHelper interně používá k vytvoření &lt;li&gt; značky HTML. Třída TagBuilder je velmi užitečná nástroj třídu, která lze použít, když potřebujete vytvořit novou značku HTML. Obsahuje metody pro přidání atributy, přidání tříd CSS, generování ID a úprava značky s vnitřního kódu HTML.

## <a name="summary"></a>Souhrn

V této iteraci vylepšili visual návrh naše aplikace ASP.NET MVC. Nejprve byly zavedeny do Galerie ASP.NET MVC. Jste zjistili, jak stáhnout šablony bezplatnou návrhu z Galerie návrhu ASP.NET MVC, který můžete použít v aplikacích ASP.NET MVC.

V dalším kroku jsme probrali, jak můžete vytvořit vlastní návrh změnou výchozí soubor šablony stylů CSS a soubor stránky předlohy. Za účelem podpory nový design, jsme měli provádět některé malé změny pro naši aplikaci obraťte se na správce. Například jsme přidali nové pomocné rutiny HTML s názvem MenuItemHelper, která zobrazí vybrané a nezaškrtnuté karty.

V další iterace jsme řešení velmi důležité předmět ověření. Přidáme ověřovacího kódu k naší aplikaci tak, aby uživatele nelze vytvořit nový kontakt bez nutnosti zadávat požadované hodnoty, například uživatel s nejprve a příjmení.

> [!div class="step-by-step"]
> [Předchozí](iteration-1-create-the-application-cs.md)
> [další](iteration-3-add-form-validation-cs.md)
