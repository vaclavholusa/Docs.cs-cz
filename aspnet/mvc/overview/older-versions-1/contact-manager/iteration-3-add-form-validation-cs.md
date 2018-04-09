---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
title: 'Iterace #3 – přidat ověřování formuláře (C#) | Microsoft Docs'
author: microsoft
description: V třetím iteraci přidáme ověření základní formulář. Jsme zabránit neoprávněným osobám v odeslání formuláře bez dokončení vyžadovaná pole formuláře. Také jsme ověřit emai...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 51a0d175-913b-43d8-95e3-840fb96ad1a9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: b9353c32b2839fd760513982c5742bb8f521e94a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="iteration-3--add-form-validation-c"></a>Iterace #3 – přidat ověřování formuláře (C#)
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhněte si kód](iteration-3-add-form-validation-cs/_static/contactmanager_3_cs1.zip)

> V třetím iteraci přidáme ověření základní formulář. Jsme zabránit neoprávněným osobám v odeslání formuláře bez dokončení vyžadovaná pole formuláře. Můžeme také ověřit e-mailových adres a telefonních čísel.


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

V této druhé iteraci aplikace, obraťte se na správce přidáme ověření základní formulář. Jsme zabránit neoprávněným osobám v odesílání a obraťte se na bez nutnosti zadávat hodnoty povinných polí formuláře. Můžeme také ověřit telefonní čísla a e-mailové adresy (viz obrázek 1).


[![Dialogové okno Nový projekt](iteration-3-add-form-validation-cs/_static/image1.jpg)](iteration-3-add-form-validation-cs/_static/image1.png)

**Obrázek 01**: formuláře s ověřování ([Kliknutím zobrazit obrázek v plné velikosti](iteration-3-add-form-validation-cs/_static/image2.png))


V této iteraci přidáme logiku ověření přímo k akce kontroleru. Obecně toto není doporučený způsob přidání ověřování do aplikace ASP.NET MVC. Lepším řešením je umístit aplikační logiku ověřování s v samostatném [vrstvy služby](http://martinfowler.com/eaaCatalog/serviceLayer.html). V další iterace jsme Refaktorovat obraťte se na správce aplikace zpřístupnění aplikace více udržovatelný.

V této iteraci účelem zjednodušení, jsme zapisovat všechny ověřovacího kódu ručně. Místo psaní kódu ověření sebe, jsme může využít výhod rozhraní ověřování. Například můžete použít Microsoft Enterprise knihovny ověřování aplikace blok (VAB) implementovat logiku ověření pro vaši aplikaci ASP.NET MVC. Další informace o ověření bloku aplikace naleznete v tématu:

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>Přidání ověření do zobrazení pro vytváření

Umožní s Začněte přidáním logiku ověření pro zobrazení pro vytváření. Naštěstí protože jsme vygenerován zobrazení vytvořit pomocí sady Visual Studio, zobrazení pro vytváření již obsahuje všechny logika potřebné uživatelské rozhraní pro zobrazení ověřovacích zpráv. Zobrazení pro vytváření je obsažený v výpis 1.

**Listing 1 - \Views\Contact\Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-cs/samples/sample1.aspx)]

Všimněte si volání Html.ValidationSummary() Pomocná metoda, která se zobrazí okamžitě nad formuláře HTML. Pokud existují chybových zpráv ověření, pak tato metoda zobrazí v seznamu s odrážkami ověřovacích zpráv.

Všimněte si, navíc k Html.ValidationMessage() volání, které se zobrazují vedle každého pole formuláře. Pomocník ValidationMessage() zobrazí chybovou zprávu ověření jednotlivých. V případě výpis 1 znak hvězdičky se zobrazí, když dojde k chybě ověření.

Html.TextBox() pomocníka nakonec automaticky vykreslí třídu stylů CSS, když dojde k chybě ověření přidružená vlastnost zobrazí pomocné rutiny. Pomocník Html.TextBox() vykreslí třídy s názvem **input-validation-error**.

Při vytváření nové aplikace ASP.NET MVC, šablony stylů s názvem Site.css se automaticky vytvoří ve složce obsahu. Tento list stylu obsahuje následující definice tříd CSS související s vzhled chybových zpráv ověření:

[!code-css[Main](iteration-3-add-form-validation-cs/samples/sample2.css)]

Pole validation-error třída slouží k úpravě stylu výstupu vykreslen metodou Html.ValidationMessage() helper. Input-validation-error třída se používá k úpravě stylu textové pole (vstup) vykreslen metodou Html.TextBox() helper. Ověření summary-errors třída se používá k úpravě stylu neuspořádaný seznam vykreslen metodou Html.ValidationSummary() helper.

> [!NOTE] 
> 
> Třídy seznamu vlastností stylu popsaných v této části, chcete-li přizpůsobit vzhled chybových zpráv ověření, můžete upravit.


## <a name="adding-validation-logic-to-the-create-action"></a>Přidání logiku ověření pro vytvořit akce

Teď, zobrazení pro vytváření nikdy zobrazí chybových zpráv ověření, protože jsme nebyly zapsány logiku pro generování všechny zprávy. Aby bylo možné zobrazit chybové zprávy ověření, budete muset přidat chybové zprávy do ModelState.

> [!NOTE] 
> 
> Metoda UpdateModel() přidá chybové zprávy do ModelState automaticky když dojde k chybě při přiřazování hodnotu pole formuláře k vlastnosti. Například pokud se pokusíte přiřadit řetězec "apple" datum narození vlastnosti, která přijímá hodnoty DateTime, pak metoda UpdateModel() přidá chybu do ModelState.


Metodu Create() upravené výpis 2 obsahuje novou část, která ověřuje vlastnosti třídy, obraťte se před vložením nového kontaktu do databáze.

**Výpis 2 - Controllers\ContactController.cs (vytvoření s ověřování)**

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample3.cs)]

V části ověření vynucuje čtyř různých ověřovacích pravidel:

- Vlastnost FirstName musí mít délku větší než nula (a nemůže skládat pouze z mezer)
- Vlastnost LastName musí mít délku větší než nula (a nemůže skládat pouze z mezer)
- Pokud telefon vlastnost má hodnotu (má délku větší než 0) a vlastnost Phone musí odpovídat regulárnímu výrazu.
- Pokud e-mailu vlastnost má hodnotu (má délku větší než 0) a vlastnost e-mailu musí odpovídat regulárnímu výrazu.

Když je porušení ověřovacího pravidla, chybová zpráva se přidá do ModelState pomocí metody AddModelError(). Když přidáte zprávu ModelState, je třeba zadat název vlastnosti a text chybovou zprávu ověření. Tato chybová zpráva se zobrazí v zobrazení pomocí pomocné metody Html.ValidationSummary() a Html.ValidationMessage().

Po provedení ověřovacích pravidel, se kontroluje vlastnost IsValid ModelState. IsValid – vlastnost vrací hodnotu false, pokud nějaké chybové zprávy ověření byly přidány do ModelState. Pokud ověření selže, se zobrazí formulář vytvořit znovu s chybové zprávy.

> [!NOTE] 
> 
> Zobrazí chybové regulárních výrazů pro ověření telefonní číslo a e-mailovou adresu z úložiště regulárnímu výrazu v [*http://regexlib.com*](http://regexlib.com)


## <a name="adding-validation-logic-to-the-edit-action"></a>Přidání logiku ověření pro akci Upravit

Akce Edit() aktualizuje a kontaktovat. Akce Edit() je potřeba provést stejné ověření jako Create() akce. Místo duplikování stejný kód ověřování, jsme by měl Refaktorovat obraťte se na řadič, aby Create() i Edit() akce volání stejnou metodu ověření.

Upravené třídy kontroleru kontakt je součástí výpis 3. Tato třída obsahuje novou ValidateContact() metodu, která je volána v rámci Create() i Edit() akce.

**Výpis 3 - Controllers\ContactController.cs**

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample4.cs)]

## <a name="summary"></a>Souhrn

V této iteraci jsme přidali ověřování základní formuláře pro naši aplikaci obraťte se na správce. Ověřovací logiku zabrání uživatelům v odesílání nový kontakt nebo úpravou existující kontakt bez nutnosti zadávat hodnoty pro vlastnosti jméno a příjmení. Kromě toho musí uživatelé zadat platné telefonní čísla a e-mailové adresy.

V této iteraci jsme přidali logiku ověření pro naši aplikaci obraťte se na správce Nejsnadnějším způsobem, který je možné. Však kombinování ověřovací logiku do řadiče logiku vytvoří problémy pro nás na dlouhou dobu. Naše aplikace bude obtížné spravovat a upravovat v čase.

V další iterace jsme bude Refaktorovat naše logiku ověření a logiku přístupu k řadiči pro naše. Provedeme výhod několik zásady designu softwaru povolit nám vytvořte více volného párované a více udržovatelného aplikaci.

> [!div class="step-by-step"]
> [Předchozí](iteration-2-make-the-application-look-nice-cs.md)
> [další](iteration-4-make-the-application-loosely-coupled-cs.md)
