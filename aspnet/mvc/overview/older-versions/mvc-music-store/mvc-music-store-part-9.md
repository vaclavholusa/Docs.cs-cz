---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: '9. část: Registrace a pokladna | Dokumentace Microsoftu'
author: jongalloway
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store. Část 9 pokrývá registrace a pokladna.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: b3babf1d935774b0ef93d6ab02c8b295998f8afc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756070"
---
<a name="part-9-registration-and-checkout"></a>9. část: Registrace a pokladna
====================
podle [Jon Galloway](https://github.com/jongalloway)

> MVC Music Store jde o kurz, který se seznámíte, podrobné postupy pro vývoj pro web pomocí ASP.NET MVC a sady Visual Studio.  
>   
> Music Store MVC je jednoduché ukázku implementace úložiště prodává hudebních alb online, který implementuje správu základního webu, přihlášení uživatele a nákupního košíku funkce.  
>   
> V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store. Část 9 pokrývá registrace a pokladna.


V této části budeme vytvářet CheckoutController, která bude shromažďovat adresu zákazník a platební údaje. Začneme vyžadovat uživatelům registrovat před rezervace, takže tento řadič bude vyžadovat ověření.

Uživatelé přejdete na proces platby u pokladny z jejich nákupního košíku klepnutím na tlačítko "Rezervace".

![](mvc-music-store-part-9/_static/image1.jpg)

Pokud uživatel není přihlášen, zobrazí se výzva k.

![](mvc-music-store-part-9/_static/image1.png)

Po úspěšném přihlášení uživatele se zobrazí zobrazení adres a platba.

![](mvc-music-store-part-9/_static/image2.png)

Jakmile máte formuláři vyplněný a odesláním objednávky, se bude zobrazovat na potvrzovací obrazovce objednávky.

![](mvc-music-store-part-9/_static/image3.png)

Při pokusu o zobrazení pořadí neexistující nebo pořadí, které nepatří uživateli se zobrazí zobrazení chyb.

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a>Migrace nákupního košíku

Během procesu nákupu je anonymní, když uživatel klikne na tlačítko rezervovat, je třeba k registraci a přihlášení. Uživatelé se očekávají, že budeme udržovat své nákupní košík informací mezi návštěvy, takže bude potřeba po dokončení registrace nebo přihlášení nákupního košíku informace přidruží k uživateli.

Jde ve skutečnosti velmi jednoduchý postup, jak naše ShoppingCart třída již má metodu, která se přidruží ke všech položek v košíku aktuální uživatelské jméno. Právě jsme potřeba volat tuto metodu, když uživatel dokončí registraci nebo přihlášení.

Otevřít **AccountController** třídu, která jsme přidali jsme byly nastavení členství a ověřování. Přidat pak pomocí příkazu odkazující na MvcMusicStore.Models, přidejte následující metodu MigrateShoppingCart:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

Dále upravte akci po přihlášení k volání MigrateShoppingCart po ověření uživatele, jak je znázorněno níže:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

Proveďte stejnou změnu do registru odeslat akci, ihned po úspěšném vytvoření účtu uživatele:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

Je to – teď je anonymní nákupního košíku se automaticky přesunou do uživatelského účtu po úspěšné registraci nebo přihlášení.

## <a name="creating-the-checkoutcontroller"></a>Vytváří CheckoutController

Klikněte pravým tlačítkem na složku řadiče a přidejte nový kontroler do projektu s názvem CheckoutController pomocí šablony prázdný kontroler.

![](mvc-music-store-part-9/_static/image5.png)

Nejprve přidejte atribut Authorize nad deklaraci třídy Kontroleru budou muset uživatelé zaregistrovat rezervaci:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

*Poznámka: Toto je podobný změnu, kterou jsme dříve provedené StoreManagerController, ale v takovém případě vyžadován atribut ověřit, že se uživatel v roli správce. V Kontroleru Checkout jsme už by být přihlášeni uživatele ale nejsou by se správci.*

Z důvodu zjednodušení společnost Microsoft nebude pracující s platební informace v tomto kurzu. Místo toho jsme se povolení uživatele, aby pomocí propagační kód. Jsme tento propagační kód pomocí konstanta s názvem PromoCode uloží.

Stejně jako v StoreController jsme budete deklarovat pole pro uložení instance třídy MusicStoreEntities s názvem storeDB. Aby bylo možné použít třídy MusicStoreEntities, budeme muset přidat, pomocí příkazu pro obor názvů MvcMusicStore.Models. Horní Checkout kontroleru se zobrazí pod.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

CheckoutController bude mít následující akce kontroleru:

**AddressAndPayment (metodu GET)** zobrazí formulář umožňuje uživateli zadat informace o jejich.

**AddressAndPayment (metodu POST)** ověření vstupu, který se zpracování objednávky.

**Kompletní** zobrazí poté, co uživatel úspěšně dokončil proces platby u pokladny. Toto zobrazení bude obsahovat uživatele pořadové číslo, s potvrzením.

Nejprve do AddressAndPayment přejmenujme akce kontroleru indexu (který se vygeneroval při jsme vytvořili kontroleru). Tato akce kontroleru zobrazí pouze formulář checkout, nevyžaduje žádné informace o modelu.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

Naše metodu AddressAndPayment POST bude postupují stejným způsobem jsme použili v StoreManagerController: pokusí přijmout odeslání formuláře a dokončení objednávky a znovu zobrazte formulář, pokud se nezdaří.

Po ověření vstupu formuláře nesplňuje naše požadavky na ověření pro objednávky, jsme se přímo zkontrolujte hodnotu PromoCode formuláře. Za předpokladu, že je všechno správně, že jsme se uloží aktualizované informace s pořadím, řekněte ShoppingCart objektu k dokončení procesu objednávání a přesměrování na dokončení akce.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

Po úspěšném dokončení procesu registrace budete přesměrováni na akce kontroleru kompletní uživatelů. Tato akce provede jednoduchou kontrolu pro ověření, že pořadí ve skutečnosti patří do přihlášeného uživatele před zobrazením pořadové číslo jako potvrzení.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

*Poznámka: Zobrazení chyb byl automaticky vytvořen pro nás ve složce /Views/Shared když jsme začali s projektu.*

Kompletní kód CheckoutController vypadá takto:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a>Přidání zobrazení AddressAndPayment

Teď vytvoříme AddressAndPayment zobrazení. Klikněte pravým tlačítkem na jednu z akcí kontroleru AddressAndPayment a přidat zobrazení s názvem AddressAndPayment, což je silně typováno jako pořadí a používá šablonu upravit, jak je znázorněno níže.

![](mvc-music-store-part-9/_static/image6.png)

Toto zobrazení způsobí, že použití dvou technik zvažovali jsme i při vytváření zobrazení StoreManagerEdit:

- Použijeme Html.EditorForModel() k zobrazení polí formuláře pro model pořadí
- Společnost Microsoft bude využívat ověřovacích pravidel pomocí atributů ověření třídu pořadí

Začneme aktualizací kód formuláře, který použije Html.EditorForModel(), za nímž následuje další textové pole pro propagační kód. Kompletní kód AddressAndPayment zobrazení je uveden níže.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a>Definování pravidel ověřování pro pořadí

Teď, když náš zobrazení je nastaven, nastavíme ověřovacích pravidel pro náš model pořadí jako jsme to udělali dříve alba modelu. Klikněte pravým tlačítkem na složku modely a přidejte třídu pojmenovanou pořadí. Kromě ověřování atributů, které jsme použili dříve alba také použijeme regulární výraz k ověření e-mailovou adresu uživatele.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

Pokus o odeslání formuláře s chybějící nebo neplatné informace se nyní zobrazí chybovou zprávu pomocí ověřování na straně klienta.

![](mvc-music-store-part-9/_static/image7.png)

Dobře jsme provedli většinu těžkou práci pro proces platby u pokladny; stačilo několik číslům umocněným na druhou a končí na dokončení. Je potřeba přidat dvě jednoduché zobrazení a musíme starat o předání košíku informace během procesu přihlášení.

## <a name="adding-the-checkout-complete-view"></a>Přidání Checkout úplné zobrazení

Rezervace kompletní přehled je docela jednoduché, protože potřebuje pouze k zobrazení ID objednávky. Klikněte pravým tlačítkem na akce kontroleru kompletní a přidat zobrazení s názvem dokončeno, což je silně typováno jako celé číslo

![](mvc-music-store-part-9/_static/image8.png)

Nyní aktualizujeme zobrazit kód pro zobrazení ID objednávky, jak je znázorněno níže.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a>Aktualizace zobrazení The chyb

Výchozí šablona zahrnuje zobrazení chyb ve sdílené složce zobrazení tak, aby se dá znovu použít jinde v lokalitě. Toto zobrazení chyba obsahuje chybu velmi jednoduchý a nepoužívá náš web rozložení, abychom vám ho aktualizovat.

Protože toto je stránka obecné chybě, je velmi jednoduchý obsah. Budete zahrnujeme zprávu a odkaz přejděte na předchozí stránku v historii, pokud chce uživatel znovu opakujte akci.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


> [!div class="step-by-step"]
> [Předchozí](mvc-music-store-part-8.md)
> [další](mvc-music-store-part-10.md)
