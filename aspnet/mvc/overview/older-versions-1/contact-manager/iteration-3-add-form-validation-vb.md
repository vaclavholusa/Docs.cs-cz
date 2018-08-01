---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
title: 'Iterace #3 – Přidání ověřovacího formuláře (VB) | Dokumentace Microsoftu'
author: microsoft
description: Ve třetí iterace přidáme ověření základní formulář. Můžeme zabránit neoprávněným osobám v odeslání formuláře bez dokončení vyžadovaná pole formuláře. Můžeme také ověřit emai...
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: 4805e75a-7911-46e3-b11b-229a6eed245e
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: a02178bfb662f180343ad32a6453b910e8e70471
ms.sourcegitcommit: a25b572eaed21791230c85416f449f66a405ec19
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/01/2018
ms.locfileid: "39396205"
---
<a name="iteration-3--add-form-validation-vb"></a>Iterace #3 – Přidání ověřovacího formuláře (VB)
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout kód](iteration-3-add-form-validation-vb/_static/contactmanager_3_vb1.zip)

> Ve třetí iterace přidáme ověření základní formulář. Můžeme zabránit neoprávněným osobám v odeslání formuláře bez dokončení vyžadovaná pole formuláře. Také ověření e-mailových adres a telefonních čísel.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Vytvoření aplikace pro správu kontaktů ASP.NET MVC (VB)
  

V této sérii kurzů jsme integrovali celou aplikaci kontakt správy od začátku na dokončení. Obraťte se na správce aplikace umožňuje ukládat kontaktní údaje - jména, telefonní čísla a e-mailové adresy – seznam lidí.

Vytváříme aplikaci přes více iterací. S každou iterací zvyšujeme postupně aplikace. Cílem tohoto přístupu s více iterace je vám pomohl pochopit důvod pro každou změnu.

- Iterace #1 – Vytvoření aplikace. V první iteraci vytvoříme Správce kontaktů v Nejjednodušším způsobem, jak je to možné. Přidáváme podporu pro základní databázových operací: vytváření, čtení, aktualizace a odstranění (CRUD).

- Ujistěte se, iterace #2 – vylepšení vzhledu aplikace. V této iterace můžeme zlepšit vzhled aplikace tak, že změna výchozích hlavní stránka zobrazení ASP.NET MVC a stylů CSS.

- Iterace #3 – Přidání ověřovacího formuláře. Ve třetí iterace přidáme ověření základní formulář. Můžeme zabránit neoprávněným osobám v odeslání formuláře bez dokončení vyžadovaná pole formuláře. Také ověření e-mailových adres a telefonních čísel.

- Iterace #4 – vytvoření volně spárované aplikace. V této iterace čtvrtý můžeme využít několik způsobů návrhu v softwaru k bylo snazší spravovat a upravovat aplikace Správce kontaktů. Například Refaktorovat jsme naši aplikaci pomocí vzoru úložiště a vzor vkládání závislostí.

- Iterace #5 – vytvoření testů jednotek. V páté iteraci jsme snadněji naší aplikace spravovat a upravovat tak, že přidáte testy jednotek. Jsme napodobení našich tříd datových modelů a vytváření testů jednotek pro naše řadiče a logiku ověřování.

- Iterace #6 – použití vývoje řízeného testováním. V této iterace šestého přidáme nové funkce do naší aplikace tak, že nejprve zápis testů jednotek a psaní kódu pro testování částí. V této iterace můžeme přidat skupiny kontaktů.

- Iterace #7 – přidání funkcí Ajax. V sedmé iteraci můžeme zlepšit rychlost reakce a výkon naší aplikace tak, že přidáte podporu pro Ajax.


## <a name="this-iteration"></a>Tuto iteraci

V této druhé iterace kontaktujte správce aplikace přidáme ověření základní formulář. Můžeme zabránit neoprávněným osobám v odesílání kontaktu bez zadání hodnoty povinných polí formuláře. Také ověření telefonní čísla a e-mailové adresy (viz obrázek 1).


[![Dialogové okno Nový projekt](iteration-3-add-form-validation-vb/_static/image1.jpg)](iteration-3-add-form-validation-vb/_static/image1.png)

**Obrázek 01**: formulář s ověřováním ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-3-add-form-validation-vb/_static/image2.png))


V této iterace přidáme ověřovací logiku přímo na akce kontroleru. Obecně toto není doporučený postup pro přidání ověřování do aplikace ASP.NET MVC. Lepším řešením je umístit logiku ověřování s aplikací v samostatném [vrstva služby](http://martinfowler.com/eaaCatalog/serviceLayer.html). V další iteraci jsme Refaktorovat kontaktujte správce aplikace provádět jednodušší údržbu aplikace.

V této iterace abychom si to nekomplikovali, jsme psát veškerý kód pro ověření ručně. Místo psaní kódu ověření sami, může využíváme rozhraní ověřování. Například můžete použít Microsoft Enterprise Library ověření aplikace blok (VAB) pro implementace logiky ověření pro vaši aplikaci ASP.NET MVC. Další informace o ověření Application Block, naleznete v tématu:

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>Přidání ověření do zobrazení pro vytváření

Umožní s Začněte přidáním logiku ověřování k zobrazení pro vytváření. Naštěstí protože jsme vygenerován pomocí sady Visual Studio zobrazení pro vytváření, zobrazení pro vytváření již obsahuje veškerou logiku potřebné uživatelské rozhraní pro zobrazení ověřovacích zpráv. Zobrazení pro vytváření je součástí výpis 1.

**Listing 1 - \Views\Contact\Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-vb/samples/sample1.aspx)]

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

**Výpis 2 - Controllers\ContactController.vb (vytvoření s ověřováním)**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample2.vb)]

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

**Výpis 3 - Controllers\ContactController.vb**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample3.vb)]

## <a name="summary"></a>Souhrn

V této iterace jsme přidali základní formulář ověření naší aplikace Správce kontaktů. Naše logiku ověřování zabraňuje uživatelům odeslání nového kontaktu nebo úpravy stávajících kontaktu bez zadání hodnoty pro vlastnosti FirstName a LastName. Dále uživatelé musí zadat platné telefonní čísla a e-mailové adresy.

V této iterace jsme přidali logiku ověřování k naší aplikace Správce kontaktů v nejjednodušší způsob, jak je to možné. Ale kombinování naše logiku ověřování do našich logice kontroleru vytvoří problémy nám v dlouhodobém horizontu. Naše aplikace bude obtížnější spravovat a upravovat v čase.

V další iteraci jsme se naše řadiče pro Refaktorovat náš logiku ověřování a logiky přístupu k databázi. Provedeme výhod několika Principy návrhu softwaru umožňují vytvořit více volně a snadněji spravovatelné aplikace.

> [!div class="step-by-step"]
> [Předchozí](iteration-2-make-the-application-look-nice-vb.md)
> [další](iteration-4-make-the-application-loosely-coupled-vb.md)
