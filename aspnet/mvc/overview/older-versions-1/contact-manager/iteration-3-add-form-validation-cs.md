---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
title: 'Iterace #3 – Přidání ověřovacího formuláře (C#) | Dokumentace Microsoftu'
author: microsoft
description: Ve třetí iterace přidáme ověření základní formulář. Můžeme zabránit neoprávněným osobám v odeslání formuláře bez dokončení vyžadovaná pole formuláře. Můžeme také ověřit emai...
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: 51a0d175-913b-43d8-95e3-840fb96ad1a9
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: 532a5849007a5e8301da7c2801b553b9148679c4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828144"
---
<a name="iteration-3--add-form-validation-c"></a>Iterace #3 – Přidání ověřovacího formuláře (C#)
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout kód](iteration-3-add-form-validation-cs/_static/contactmanager_3_cs1.zip)

> Ve třetí iterace přidáme ověření základní formulář. Můžeme zabránit neoprávněným osobám v odeslání formuláře bez dokončení vyžadovaná pole formuláře. Také ověření e-mailových adres a telefonních čísel.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Vytvoření aplikace ASP.NET MVC pro správu kontaktů (C#)
  

V této sérii kurzů jsme integrovali celou aplikaci kontakt správy od začátku na dokončení. Obraťte se na správce aplikace umožňuje ukládat kontaktní údaje - jména, telefonní čísla a e-mailové adresy – seznam lidí.

Vytváříme aplikaci přes více iterací. S každou iterací zvyšujeme postupně aplikace. Cílem tohoto přístupu s více iterace je vám pomohl pochopit důvod pro každou změnu.

- Iterace #1 – Vytvoření aplikace. V první iteraci vytvoříme Správce kontaktů v Nejjednodušším způsobem, jak je to možné. Přidáváme podporu pro základní databázových operací: vytváření, čtení, aktualizace a odstranění (CRUD).

- Ujistěte se, iterace #2 – vylepšení vzhledu aplikace. V této iterace můžeme zlepšit vzhled aplikace tak, že změna výchozích hlavní stránka zobrazení ASP.NET MVC a stylů CSS.

- Iterace #3 – Přidání ověřovacího formuláře. Ve třetí iterace přidáme ověření základní formulář. Můžeme zabránit neoprávněným osobám v odeslání formuláře bez dokončení vyžadovaná pole formuláře. Také ověření e-mailových adres a telefonních čísel.

- Iterace #4 – vytvoření volně spárované aplikace. V této třetí iterace můžeme využít několik způsobů návrhu v softwaru k bylo snazší spravovat a upravovat aplikace Správce kontaktů. Například Refaktorovat jsme naši aplikaci pomocí vzoru úložiště a vzor vkládání závislostí.

- Iterace #5 – vytvoření testů jednotek. V páté iteraci jsme snadněji naší aplikace spravovat a upravovat tak, že přidáte testy jednotek. Jsme napodobení našich tříd datových modelů a vytváření testů jednotek pro naše řadiče a logiku ověřování.

- Iterace #6 – použití vývoje řízeného testováním. V této iterace šestého přidáme nové funkce do naší aplikace tak, že nejprve zápis testů jednotek a psaní kódu pro testování částí. V této iterace můžeme přidat skupiny kontaktů.

- Iterace #7 – přidání funkcí Ajax. V sedmé iteraci můžeme zlepšit rychlost reakce a výkon naší aplikace tak, že přidáte podporu pro Ajax.


## <a name="this-iteration"></a>Tuto iteraci

V této druhé iterace kontaktujte správce aplikace přidáme ověření základní formulář. Můžeme zabránit neoprávněným osobám v odesílání kontaktu bez zadání hodnoty povinných polí formuláře. Také ověření telefonní čísla a e-mailové adresy (viz obrázek 1).


[![Dialogové okno Nový projekt](iteration-3-add-form-validation-cs/_static/image1.jpg)](iteration-3-add-form-validation-cs/_static/image1.png)

**Obrázek 01**: formulář s ověřováním ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-3-add-form-validation-cs/_static/image2.png))


V této iterace přidáme ověřovací logiku přímo na akce kontroleru. Obecně toto není doporučený postup pro přidání ověřování do aplikace ASP.NET MVC. Lepším řešením je umístit logiku ověřování s aplikací v samostatném [vrstva služby](http://martinfowler.com/eaaCatalog/serviceLayer.html). V další iteraci jsme Refaktorovat kontaktujte správce aplikace provádět jednodušší údržbu aplikace.

V této iterace abychom si to nekomplikovali, jsme psát veškerý kód pro ověření ručně. Místo psaní kódu ověření sami, může využíváme rozhraní ověřování. Například můžete použít Microsoft Enterprise Library ověření aplikace blok (VAB) pro implementace logiky ověření pro vaši aplikaci ASP.NET MVC. Další informace o ověření Application Block, naleznete v tématu:

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>Přidání ověření do zobrazení pro vytváření

Umožní s Začněte přidáním logiku ověřování k zobrazení pro vytváření. Naštěstí protože jsme vygenerován pomocí sady Visual Studio zobrazení pro vytváření, zobrazení pro vytváření již obsahuje veškerou logiku potřebné uživatelské rozhraní pro zobrazení ověřovacích zpráv. Zobrazení pro vytváření je součástí výpis 1.

**Listing 1 - \Views\Contact\Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-cs/samples/sample1.aspx)]

Všimněte si, že volání Html.ValidationSummary() pomocnou metodu, která se zobrazí hned nad formuláře HTML. Pokud existují chybových zpráv ověření, tato metoda se zobrazí zprávy ověření na seznam s odrážkami.

Všimněte si, navíc volání Html.ValidationMessage(), které se zobrazují vedle každé pole formuláře. Pomocná rutina ValidationMessage() zobrazí chybovou zprávu ověření jednotlivých. V případě výpis 1 se zobrazí hvězdička, když dojde k chybě ověřování.

Html.TextBox() pomocné rutiny automaticky vykreslí nakonec třídu šablony stylů CSS, když dojde k chybě ověřování přidružený k vlastnosti zobrazí pomocná rutina. Pomocná rutina Html.TextBox() vykreslí třídu s názvem **Chyba ověření vstupu**.

Při vytváření nové aplikace ASP.NET MVC, šablony stylů s názvem Site.css je automaticky vytvořen ve složce obsahu. Tato šablona stylů obsahuje následující definice šablony stylů CSS třídy související s vzhled chybových zpráv ověření:

[!code-css[Main](iteration-3-add-form-validation-cs/samples/sample2.css)]

Chyba ověření pole Třída se používá k úpravě stylu výstupu vykreslen metodou Html.ValidationMessage() helper. Chyba ověření vstupu třída se používá k formátování textového pole (vstup) vykreslen metodou Html.TextBox() helper. Chyby ověření souhrn třída se používá k úpravě stylu neuspořádaný seznam vykreslen metodou Html.ValidationSummary() helper.

> [!NOTE] 
> 
> Můžete upravit třídy List stylu popsané v této části pro přizpůsobení vzhledu chybových zpráv ověření.


## <a name="adding-validation-logic-to-the-create-action"></a>Přidat logiku ověřování k vytvoření akce

V tuto chvíli, zobrazení pro vytváření nikdy nezobrazí chybových zpráv ověření, protože jsme nenapsali logiku pro generování všechny zprávy. Aby bylo možné zobrazit chybových zpráv ověření, budete muset přidat chybové zprávy ModelState.

> [!NOTE] 
> 
> Metoda UpdateModel() přidá chybové zprávy do ModelState automaticky při dojde k chybám přiřazení hodnoty pole formuláře na vlastnost. Například pokud se pokusíte přiřadit řetězec "apple" datum narození vlastnost, která přijímá hodnoty data a času, pak metoda UpdateModel() přidá chybu do ModelState.


Upravená Metoda Create() výpis 2 obsahuje nový oddíl, který ověřuje vlastnosti třídy kontakt před nový kontakt se vloží do databáze.

**Výpis 2 - Controllers\ContactController.cs (vytvoření s ověřováním)**

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample3.cs)]

V části ověření vynucuje čtyř různých ověřovacích pravidel:

- Vlastnost FirstName musí mít délku větší než nula (a nemůže skládat pouze z mezer)
- Větší než nula. délka musí být vlastnost LastName (a nemůže skládat pouze z mezer)
- Pokud má hodnotu vlastnosti telefonu (má délku větší než 0) a vlastnost telefon musí odpovídat regulárnímu výrazu.
- Pokud vlastnost e-mailu obsahuje hodnotu (má délku větší než 0) a vlastnost e-mailu musí odpovídat regulárnímu výrazu.

Po porušení pravidla ověřování je přidána chybová zpráva na ModelState pomocí metody AddModelError(). Při přidání zprávy do ModelState zadáte název vlastnosti a text chybovou zprávu ověření. Tato chybová zpráva se zobrazí v zobrazení Html.ValidationSummary() a Html.ValidationMessage() pomocné metody.

Po provedení ověřovacích pravidel, je vlastnost IsValid ModelState zaškrtnuto. Vlastnost IsValid vrátí hodnotu false, pokud byly přidány do ModelState všech chybových zpráv ověření. Pokud se ověření nezdaří, se zobrazí formulář vytvořit znovu s chybovými zprávami.

> [!NOTE] 
> 
> Zobrazilo se mi regulárních výrazů pro ověření telefonní číslo a e-mailová adresa z úložiště regulárních výrazů v [*http://regexlib.com*](http://regexlib.com)


## <a name="adding-validation-logic-to-the-edit-action"></a>Přidat logiku ověřování k akci Upravit

Akce Edit() aktualizuje kontakt. Akce Edit() musí provádět stejné ověřování jako Create() akce. Namísto duplikování stejný kód pro ověření, jsme měli Refaktorovat kontaktujte řadič tak, aby akce Create() i Edit() volání stejné metody ověřování.

Upravené třídy kontroleru kontakt je obsažen v informacích 3. Tato třída obsahuje novou ValidateContact() metodu, která je volána v rámci Create() i Edit() akcí.

**Výpis 3 - Controllers\ContactController.cs**

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample4.cs)]

## <a name="summary"></a>Souhrn

V této iterace jsme přidali základní formulář ověření naší aplikace Správce kontaktů. Naše logiku ověřování zabraňuje uživatelům odeslání nového kontaktu nebo úpravy stávajících kontaktu bez zadání hodnoty pro vlastnosti FirstName a LastName. Dále uživatelé musí zadat platné telefonní čísla a e-mailové adresy.

V této iterace jsme přidali logiku ověřování k naší aplikace Správce kontaktů v nejjednodušší způsob, jak je to možné. Ale kombinování naše logiku ověřování do našich logice kontroleru vytvoří problémy nám v dlouhodobém horizontu. Naše aplikace bude obtížnější spravovat a upravovat v čase.

V další iteraci jsme se naše řadiče pro Refaktorovat náš logiku ověřování a logiky přístupu k databázi. Provedeme výhod několika Principy návrhu softwaru umožňují vytvořit více volně a snadněji spravovatelné aplikace.

> [!div class="step-by-step"]
> [Předchozí](iteration-2-make-the-application-look-nice-cs.md)
> [další](iteration-4-make-the-application-loosely-coupled-cs.md)
