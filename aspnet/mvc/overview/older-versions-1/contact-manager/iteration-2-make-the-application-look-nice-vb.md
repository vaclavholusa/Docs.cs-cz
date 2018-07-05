---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-vb
title: 'Iterace #2 – Vytvoření aplikace vypadat nice (VB) | Dokumentace Microsoftu'
author: microsoft
description: V této iterace můžeme zlepšit vzhled aplikace tak, že změna výchozích hlavní stránka zobrazení ASP.NET MVC a stylů CSS.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f65cb436-e493-46fd-9608-384b27385aa1
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-vb
msc.type: authoredcontent
ms.openlocfilehash: c1209a925a43bd7846a9dc07ce557c55bb1827ae
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381102"
---
<a name="iteration-2--make-the-application-look-nice-vb"></a>Iterace #2 – Vytvoření aplikace vypadat nice (VB)
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout kód](iteration-2-make-the-application-look-nice-vb/_static/contactmanager_2_vb1.zip)

> V této iterace můžeme zlepšit vzhled aplikace tak, že změna výchozích hlavní stránka zobrazení ASP.NET MVC a stylů CSS.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Vytvoření aplikace pro správu kontaktů ASP.NET MVC (VB)
  

V této sérii kurzů jsme integrovali celou aplikaci kontakt správy od začátku na dokončení. Obraťte se na správce aplikace lze ukládat kontaktní údaje – jména, telefonní čísla a e-mailové adresy – seznam lidí.

Vytváříme aplikaci přes více iterací. S každou iterací zvyšujeme postupně aplikace. Cílem tohoto přístupu s více iterace je vám pomohl pochopit důvod pro každou změnu.

- Iterace #1 – Vytvoření aplikace. V první iteraci vytvoříme Správce kontaktů v Nejjednodušším způsobem, jak je to možné. Přidáváme podporu pro základní databázových operací: vytváření, čtení, aktualizace a odstranění (CRUD).

- Ujistěte se, iterace #2 – vylepšení vzhledu aplikace. V této iterace můžeme zlepšit vzhled aplikace tak, že změna výchozích hlavní stránka zobrazení ASP.NET MVC a stylů CSS.

- Iterace #3 – Přidání ověřovacího formuláře. Ve třetí iterace přidáme ověření základní formulář. Můžeme zabránit neoprávněným osobám v odeslání formuláře bez dokončení vyžadovaná pole formuláře. Také ověření e-mailových adres a telefonních čísel.

- Iterace #4 – vytvoření volně spárované aplikace. V této třetí iterace můžeme využít několik způsobů návrhu v softwaru k bylo snazší spravovat a upravovat aplikace Správce kontaktů. Například Refaktorovat jsme naši aplikaci pomocí vzoru úložiště a vzor vkládání závislostí.

- Iterace #5 – vytvoření testů jednotek. V páté iteraci jsme snadněji naší aplikace spravovat a upravovat tak, že přidáte testy jednotek. Jsme napodobení našich tříd datových modelů a vytváření testů jednotek pro naše řadiče a logiku ověřování.

- Iterace #6 – použití vývoje řízeného testováním. V této iterace šestého přidáme nové funkce do naší aplikace tak, že nejprve zápis testů jednotek a psaní kódu pro testování částí. V této iterace můžeme přidat skupiny kontaktů.

- Iterace #7 – přidání funkcí Ajax. V sedmé iteraci můžeme zlepšit rychlost reakce a výkon naší aplikace tak, že přidáte podporu pro Ajax.

## <a name="this-iteration"></a>Tuto iteraci

Cílem této iterace je k vylepšení vzhledu aplikace Správce kontaktů. Správce kontaktů v současné době používá výchozí hlavní stránka zobrazení ASP.NET MVC a stylů CSS (viz obrázek 1). Tyto don t vypadat chybný, ale nejsou zobrazeny t chcete správce kontaktů vás pod rouškou stejně jako každý jiný web ASP.NET MVC. Chcete nahradit tyto soubory vlastních souborů.


[![Dialogové okno Nový projekt](iteration-2-make-the-application-look-nice-vb/_static/image1.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image1.png)

**Obrázek 01**: výchozí vzhled aplikace ASP.NET MVC ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-2-make-the-application-look-nice-vb/_static/image2.png))


V této iterace můžu se zabývají dva přístupy ke zlepšení jeho vizuální návrh naši aplikaci. Nejprve můžu ukazují, jak využít výhod technologie ASP.NET MVC Galerie ke stažení bezplatné návrh šablony ASP.NET MVC. Galerie ASP.NET MVC vám umožňuje vytvořit profesionální webovou aplikaci, aniž by každé dílo.

Jsem se rozhodla používat šablony pro správce kontaktů aplikaci z Galerie ASP.NET MVC. Místo toho měla vlastní návrh vytvořené profesionální návrhářský podniku. V druhé části tohoto kurzu můžu vysvětlují, jak jsem pracoval ve společnosti profesionální návrhářský vytvořit konečný návrh rozhraní ASP.NET MVC.

## <a name="the-aspnet-mvc-design-gallery"></a>Galerie ASP.NET MVC

Galerie ASP.NET MVC je bezplatný zdroj poskytnutých microsoftem. Galerie ASP.NET MVC je umístěn na následující adrese:

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

ASP.NET MVC Galerie hostitelem kolekce návrhů bezplatné webů, které byly vytvořeny speciálně pro použití v projektu aplikace ASP.NET MVC. Návrhy, se nahraje členové komunity. Návštěvníci do galerie můžete hlasovat pro své oblíbené návrhy (viz obrázek 2).


[![Dialogové okno Nový projekt](iteration-2-make-the-application-look-nice-vb/_static/image2.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image3.png)

**Obrázek 02**: The Galerie ASP.NET MVC ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-2-make-the-application-look-nice-vb/_static/image4.png))


Jak se píše v tomto kurzu, je nejoblíbenější návrhu v galerii s názvem října podle Davida Hauser návrh. Tento návrh můžete použít pro projektu aplikace ASP.NET MVC pomocí následujících kroků:

1. Klikněte na tlačítko **Stáhnout** tlačítko a stáhněte si soubor October.zip k vašemu počítači.
2. Klikněte pravým tlačítkem na stažený soubor October.zip a klikněte na tlačítko **Odblokovat** tlačítko (viz obrázek 3).
3. Rozbalte tento soubor do složky s názvem dne.
4. Vyberte všechny soubory ze složky DesignTemplate obsažené ve složce. října, klikněte pravým tlačítkem na soubor a vyberte možnost nabídky **kopírování**.
5. Klikněte pravým tlačítkem na uzel projektu ContactManager v okně Průzkumník řešení Visual Studio a vyberte možnost nabídky **vložit** (viz obrázek 4).
6. Vyberte možnost nabídky sady Visual Studio **upravit, najít a nahradit, rychlého nahrazení** a nahraďte *[MyProjectName]* s *ContactManager* (viz obrázek 5).


[![Dialogové okno Nový projekt](iteration-2-make-the-application-look-nice-vb/_static/image3.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image5.png)

**Obrázek 03**: odblokování soubor stažený z webu ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-2-make-the-application-look-nice-vb/_static/image6.png))


[![Dialogové okno Nový projekt](iteration-2-make-the-application-look-nice-vb/_static/image4.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image7.png)

**Obrázek 04**: přepsání souborů v Průzkumníku řešení ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-2-make-the-application-look-nice-vb/_static/image8.png))


[![Dialogové okno Nový projekt](iteration-2-make-the-application-look-nice-vb/_static/image5.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image9.png)

**Obrázek 05**: nahrazení ContactManager [názevprojektu] ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-2-make-the-application-look-nice-vb/_static/image10.png))


Po dokončení těchto kroků bude vaše webová aplikace používat nový návrh. Na stránce na obrázku 6 znázorňuje vzhledu aplikace Správce kontaktů s návrhem. října.


[![Dialogové okno Nový projekt](iteration-2-make-the-application-look-nice-vb/_static/image6.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image11.png)

**Obrázek 06**: ContactManager šablonou dne ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-2-make-the-application-look-nice-vb/_static/image12.png))


## <a name="creating-a-custom-aspnet-mvc-design"></a>Vytváření návrhu vlastní technologie ASP.NET MVC

Galerie ASP.NET MVC je dobré výběru různých stylů. Galerie vám poskytne Bezproblémová způsob, jak přizpůsobit vzhled vašich aplikací ASP.NET MVC. A samozřejmě Galerie má velkou výhodou úplně zadarmo.

Ale můžete potřebovat vytvořit zcela jedinečný pro váš web. V takovém případě je vhodné pro práci s společnosti návrh webu. Jsem se rozhodla pro tento postup pro návrh aplikace Správce kontaktů.

Můžu si správce kontaktů z iterace č. 1 a poslali projektu do návrhu společnosti. Jejich nevlastnila sady Visual Studio (shame na ně!), ale tento kód nefungoval t představovat problém. Studenti mohli zdarma stáhnout z Microsoft Visual Web Developer [ https://www.asp.net ](https://www.asp.net) web a otevřete Správce kontaktů aplikaci Visual Web Developer. V několika dnů jejich měli vytvořen návrhu na obrázku 7.


[![Dialogové okno Nový projekt](iteration-2-make-the-application-look-nice-vb/_static/image7.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image13.png)

**Obrázek 07**: návrhovém Správce kontaktů ASP.NET MVC ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-2-make-the-application-look-nice-vb/_static/image14.png))


Nový design se skládal z dva hlavní soubory: nový soubor šablony stylů CSS a nový soubor předlohové stránky zobrazení. Hlavní stránka zobrazení obsahuje rozložení a sdíleného obsahu pro zobrazení v aplikaci ASP.NET MVC. Například hlavní stránky zobrazení obsahuje záhlaví, navigačních karet a zápatí, která se zobrazí na obrázku 7. Můžu přepsal existující Site.Master zobrazení stránky předlohy v složku Views\Shared pomocí nového souboru Site.Master společnosti návrhu

Návrh společnosti také vytvoří nové šablony stylů CSS a sadu bitových kopií. Můžu tyto nové soubory umístěny ve složce obsahu a přepsal existující soubor Site.css. Veškerý statický obsah byste měli umístit do složky obsahu.

Všimněte si, že nový návrh pro správce kontaktů obsahuje Image pro úpravy a odstraňování kontaktů. Upravit a odstranit image se zobrazí vedle každého kontaktu v tabulce HTML kontaktů.

Původně byly tyto odkazy, které byly generovány s HTML. Pomocné rutiny ActionLink() takto:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-vb/samples/sample1.aspx)]

Metoda Html.ActionLink() nepodporuje bitové kopie (metoda HTML zakóduje text odkazu z bezpečnostních důvodů). Proto jsem nahradí volání Html.ActionLink() s voláními Url.Action() takto:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-vb/samples/sample2.aspx)]

Metoda Html.ActionLink() vykreslí celý hypertextový odkaz. Metoda Url.Action() na druhé straně vykreslí jenom adresy URL bez &lt;&gt; značky.

Všimněte si kromě toho, nový návrh obsahuje karty vybrané a nezaškrtnuté. Například na obrázku 8 **vytvořit nový kontakt** vybraná karta a **kontaktů** není vybraná karta.


[![Dialogové okno Nový projekt](iteration-2-make-the-application-look-nice-vb/_static/image8.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image15.png)

**Obrázek 08**: vybraná a zrušení výběru karty ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-2-make-the-application-look-nice-vb/_static/image16.png))


Pro podporu vykreslování karet vybrané a nezaškrtnuté, jsem vytvořil vlastní pomocné rutiny HTML s názvem MenuItemHelper. Tato pomocná metoda vykreslí buď &lt;li&gt; značku nebo &lt;li třídy = "vybraný"&gt; značky v závislosti na tom, jestli aktuální kontroleru a akce odpovídá názvu kontroleru a akce předané do pomocné rutiny. Kód MenuItemHelper je obsažen v informacích 1.

**Výpis 1 – Helpers\MenuItemHelper.vb**

[!code-vb[Main](iteration-2-make-the-application-look-nice-vb/samples/sample3.vb)]

Třída TagBuilder MenuItemHelper interně používá k vytvoření &lt;li&gt; značky HTML. Třída TagBuilder je velmi užitečný nástroj pro třídu, která můžete použít pokaždé, když se budete muset vytvořit novou značku HTML. Obsahuje metody pro přidávání atributů, přidávání tříd šablon stylů CSS, generování ID a úprava značky s vnitřního kódu HTML.

## <a name="summary"></a>Souhrn

V této iterace vylepšili jsme vizuální návrh naši aplikaci ASP.NET MVC. Nejdřív jste se seznámili s ASP.NET MVC galerie. Jste zjistili, jak stáhnout zdarma návrh šablon z Galerie návrhu MVC ASP.NET, můžete použít ve svých aplikacích ASP.NET MVC.

Dále jsme zmínili, jak můžete vytvořit vlastní návrh tak, že upravíte výchozí soubor šablony stylů CSS a hlavního zobrazení stránkovacího souboru. Aby bylo možné podporovat nový návrh, jsme měli provést nějaké drobné změny do naší aplikace Správce kontaktů. Například přidali jsme nové pomocné rutiny HTML s názvem MenuItemHelper, která zobrazuje vybrané a nevybrané karty.

V další iteraci jsme překonávání velmi důležité předmětem ověření. Přidáme kód pro ověření do naší aplikace tak, aby uživatele nelze vytvořit nový kontakt bez poskytnutí požadované hodnoty, například osoba s prvním a příjmení.

> [!div class="step-by-step"]
> [Předchozí](iteration-1-create-the-application-vb.md)
> [další](iteration-3-add-form-validation-vb.md)
