---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: "Část 9: Registrace a ověření | Microsoft Docs"
author: jongalloway
description: "Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště. Část 9 popisuje registraci a ověření."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: 799985f889b1063c53df7bce365ae3989809ba79
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="part-9-registration-and-checkout"></a>Část 9: Registrace a ověření
====================
podle [Jon Galloway](https://github.com/jongalloway)

> Úložiště Hudba MVC je kurz aplikace, která představuje a vysvětluje krok za krokem, jak používat rozhraní ASP.NET MVC a Visual Studio pro vývoj webů.  
>   
> Úložiště Hudba MVC je implementace úložiště lightweight ukázkové, který prodává hudebních alb online a implementuje základní Správa serveru, přihlášení uživatele a nákupního košíku funkce.  
>   
> Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště. Část 9 popisuje registraci a ověření.


V této části se budeme vytvářet CheckoutController, která bude shromažďovat kupujícímu adresy a platebních údajů. Jsme bude od uživatelů zaregistrujte se na náš web před odhlašuje, aby tento řadič bude vyžadovat autorizace vyžadovat.

Uživatelé budou přejděte do procesu najdete v článku věnovaném z jejich nákupní košík kliknutím na tlačítko "Ověření".

![](mvc-music-store-part-9/_static/image1.jpg)

Pokud uživatel není přihlášen, se zobrazí výzva k.

![](mvc-music-store-part-9/_static/image1.png)

Po úspěšném přihlášení uživatele jsou pak uvedeny adresy a platebních zobrazení.

![](mvc-music-store-part-9/_static/image2.png)

Jakmile budou mít vyplněno formuláře a odeslána pořadí, budou se zobrazovat na potvrzovací obrazovce a pořadí.

![](mvc-music-store-part-9/_static/image3.png)

Při pokusu o zobrazení pořadí neexistující nebo pořadí, které nepatří do vám zobrazí zobrazení chyb.

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a>Migrace nákupní košík

Nákupní proces je anonymní, když uživatel klikne na tlačítko Checkout, bude nutné registrace a přihlášení. Uživatelé budou předpokládají, že uchováváme jejich nákupní košík informací mezi návštěvy, takže je potřeba přidružit nákupní košík informace uživatele, když po dokončení registrace nebo přihlášení.

Jedná se ve skutečnosti velmi jednoduché k účelu, protože naše třída ShoppingCart již má metodu, která budou všechny položky v košíku aktuální přidružit k uživatelskému jménu. Právě jsme potřeba volat tuto metodu, když uživatel dokončení registrace nebo přihlášení.

Otevřete **AccountController** třídu, která jsme přidali, když jsme se nastavení členství a autorizace. Přidat pak pomocí příkazu odkazující na MvcMusicStore.Models, přidejte následující metodu MigrateShoppingCart:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

Potom upravte akce post přihlášení k volání MigrateShoppingCart po ověření uživatele, jak je uvedeno níže:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

Ujistěte se, stejné změnu registru post akce ihned po úspěšném vytvoření účtu uživatele:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

Je to – nyní se anonymní nákupní košík automaticky převede na uživatelský účet po úspěšné registraci nebo přihlášení.

## <a name="creating-the-checkoutcontroller"></a>Vytváření CheckoutController

Klikněte pravým tlačítkem na složku řadiče a přidejte nový řadič do projektu s názvem CheckoutController pomocí šablony prázdný kontroler.

![](mvc-music-store-part-9/_static/image5.png)

Nejprve přidejte atribut autorizovat nad deklaraci třídy Controller budou muset uživatelé zaregistrovat, teprve pak najdete v článku věnovaném:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

*Poznámka: Toto je podobná změny, které jsme dříve provedené StoreManagerController, ale v takovém případě atribut autorizovat vyžaduje, aby se uživatel v roli správce. V Kontroleru najdete v článku věnovaném jsme se nutnosti být uživatel přihlášen, ale nejsou vyžadující, aby být správci.*

Z důvodu zjednodušení jsme nebude zabývajících se informace o platebních v tomto kurzu. Místo toho jsme se umožnit uživatelům rezervovat pomocí propagační kód. Tento propagační kód použití konstant s názvem PromoCode jsme se uloží.

Jako StoreController jsme budete pole pro uložení instance třídy MusicStoreEntities s názvem storeDB deklarovat. Aby bylo možné použít třídy MusicStoreEntities, budeme muset přidat, pomocí příkazu pro obor názvů MvcMusicStore.Models. Horní naše najdete v článku věnovaném řadiče se zobrazí níže.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

CheckoutController bude mít řadič takto:

**AddressAndPayment (metoda GET)** zobrazí formulář umožňuje, aby uživatel zadal své informace.

**AddressAndPayment (metodu POST)** se ověření vstupu a pořadí zpracování.

**Dokončení** se zobrazí po uživatel úspěšně dokončil proces checkout. Toto zobrazení bude obsahovat uživatele pořadové číslo, jako potvrzení.

Nejprve umožňuje AddressAndPayment přejmenujte akce kontroleru indexu (který byl vygenerován při jsme vytvořili řadičem). Tato akce kontroleru právě zobrazí formulář najdete v článku věnovaném, není třeba žádné informace o modelu.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

Naše metodu AddressAndPayment POST bude postupovat podle stejného vzoru jsme použili v StoreManagerController: pokusí přijměte odeslání formuláře a dokončení objednávky a znovu zobrazit formulář, pokud se nezdaří.

Po ověření vstupu z formuláře splňuje požadavky naše ověření pořadí, jsme hodnotu formuláře PromoCode zkontrolujte přímo. Za předpokladu, že je vše v pořádku, že jsme se s pořadím uložit aktualizované informace, řekněte ShoppingCart objekt k dokončení procesu pořadí a přesměrování na dokončení akce.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

Po úspěšném dokončení procesu najdete v článku věnovaném uživatelé přesměrováni na akce dokončení kontroleru. Tato akce provede kontrolu jednoduché k ověření, že pořadí skutečně patří do přihlášeného uživatele před zobrazením pořadové číslo jako potvrzení.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

*Poznámka: Zobrazení chyb byl automaticky vytvořen pro nás ve složce /Views/Shared když jsme začali projektu.*

Kód dokončení CheckoutController vypadá takto:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a>Přidání zobrazení AddressAndPayment

Nyní vytvoříme AddressAndPayment zobrazení. Klikněte pravým tlačítkem na jednu z akce kontroleru AddressAndPayment a přidat zobrazení s názvem AddressAndPayment, který je silného typu jako pořadí a používá šablony upravit, jak je uvedeno níže.

![](mvc-music-store-part-9/_static/image6.png)

Toto zobrazení se budou používat dva postupy jsme se podívali na při vytváření zobrazení StoreManagerEdit:

- Chcete-li zobrazit pole formuláře pro model pořadí použijeme Html.EditorForModel()
- Jsme využije ověřovacích pravidel pomocí atributů ověření třídu pořadí

Začneme aktualizací formuláře kód, který použije Html.EditorForModel(), následuje další textové pole pro tento propagační kód. Kód dokončení pro zobrazení AddressAndPayment jsou uvedeny níže.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a>Definice ověřovacích pravidel pro pořadí

Teď, když je naše zobrazení nastavili, jsme nastaví ověřovacích pravidel pro náš model pořadí jako jsme to udělali dřív Album modelu. Klikněte pravým tlačítkem na složku modely a přidejte třídu s názvem pořadí. Kromě ověření atributy, které jsme použili dříve alba také použijeme regulární výraz k ověření e-mailovou adresu uživatele.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

Probíhá pokus o odeslání formuláře s chybějící nebo neplatné informace se nyní zobrazí chybovou zprávu pomocí ověřování na straně klienta.

![](mvc-music-store-part-9/_static/image7.png)

V pořádku máme Hotovo většinu práce pevný pro proces najdete v článku věnovaném; právě máme několik pravděpodobnost a končí na dokončení. Je potřeba přidat dvě jednoduché zobrazení a musíme postará o předání informací košíku během procesu přihlášení.

## <a name="adding-the-checkout-complete-view"></a>Přidání zobrazení najdete v článku věnovaném dokončení

Dokončení najdete v článku věnovaném zobrazení je poměrně jednoduché jako potřebuje pouze pro zobrazení ID pořadí. Klikněte pravým tlačítkem na akce dokončení kontroleru a přidat zobrazení s názvem dokončeno, což je silného typu jako typ int.

![](mvc-music-store-part-9/_static/image8.png)

Nyní budeme aktualizovat zobrazení kód, který zobrazí ID pořadí, jak je uvedeno níže.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a>Aktualizace zobrazení chyb

Výchozí šablony zahrnuje zobrazení chyb ve sdílené složce, zobrazení, aby bylo možné ho znovu použít jinde v lokalitě. Toto zobrazení chyba obsahuje chybu velmi jednoduchý a nepoužívá náš web rozložení, takže jsme ji budete aktualizovat.

Vzhledem k tomu, že toto je obecná chyba stránky, je velmi jednoduchý obsah. Budete zahrnuta zprávu a odkaz přejít na předchozí stránku v historii, pokud chce uživatel zkuste akci znovu.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


>[!div class="step-by-step"]
[Předchozí](mvc-music-store-part-8.md)
[další](mvc-music-store-part-10.md)
