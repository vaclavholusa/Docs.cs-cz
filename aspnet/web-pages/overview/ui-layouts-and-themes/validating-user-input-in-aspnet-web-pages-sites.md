---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: Ověřování uživatelského vstupu v rozhraní ASP.NET Web Pages lokalit (Razor) | Microsoft Docs
author: tfitzmac
description: Tento článek popisuje, jak ověřit informace získáte od uživatelů &mdash; to znamená, abyste měli jistotu, že uživatelé zadat platné informace ve formátu HTML forms v přihlašovacím...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 34f703e6db70ac79c22f4a50d4cfd4e2326b4c74
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a>Ověřování uživatelského vstupu v lokalitách rozhraní ASP.NET Web Pages (Razor)
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento článek popisuje, jak ověřit informace získáte od uživatelů &mdash; to znamená, abyste měli jistotu, že uživatelé zadat platné informace ve formátu HTML forms v stránku ASP.NET Web Pages (Razor).
> 
> Získáte informace:
> 
> - Jak zkontrolovat, že vstup uživatele odpovídá ověřovací kritéria, které definujete.
> - Jak určit, zda byly úspěšně všechny testy pro ověření.
> - Postupy: zobrazení chyb při ověřování (a způsob jejich formátování).
> - Jak ověřit data, která nepřejde do stavu přímo od uživatelů.
> 
> Jsou to ASP.NET programování koncepty představené v článku:
> 
> - `Validation` Pomocné rutiny.
> - `Html.ValidationSummary` a `Html.ValidationMessage` metody.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Rozhraní ASP.NET Web Pages (Razor) 3
>   
> 
> V tomto kurzu taky spolupracuje se službou ASP.NET Web Pages 2.


Tento článek obsahuje následující části:

- [Přehled ověřování uživatelského vstupu](#Overview_of_User_Input_Validation)
- [Ověřování uživatelského vstupu](#Validating_User_Input)
- [Přidání ověřování na straně klienta](#Adding_Client-Side_Validation)
- [Formátování chyby ověření](#Formatting_Validation_Errors)
- [Ověřování dat, která nepřejde do stavu přímo od uživatelů](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a>Přehled ověřování uživatelského vstupu

Pokud se požádat uživatele k zadání informací na stránce – například do formuláře – je důležité se ujistit, zda jsou hodnoty, které jsou zadány platné. Například nechcete zpracovat formulář, který chybí důležitých informací.

Při zadávání hodnot do formuláře HTML, jsou hodnoty, které budou zadejte řetězce. V mnoha případech jsou hodnoty, které budete potřebovat některé jiné datové typy, jako celá čísla nebo kalendářní data. Proto máte také zajistit, že hodnoty, které uživatelé zadají lze správně převést na příslušné datové typy.

Také může mít určitá omezení hodnot. I když uživatelé správně zadejte celé číslo, například může musíte zajistit, že hodnota spadá do určitého rozsahu.

![Chyby ověření, které používají třídy CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> **Důležité** ověřování uživatelského vstupu je také důležité pro zabezpečení. Pokud omezíte hodnoty, které mohou uživatelé zadat ve formulářích, se snižuje riziko, že někdo můžete zadat hodnotu, která může ohrozit zabezpečení vašeho webu.


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a>Ověřování uživatelského vstupu

V technologii ASP.NET Web Pages 2, můžete použít `Validator` pomocná rutina pro testovací uživatelský vstup. Základní postup je provést následující akce:

1. Určete, které vstupní elementy (pole), kterou chcete ověřit.

    Obvykle ověření hodnot v `<input>` prvky ve formuláři. Je však vhodné ověřit veškerý vstup, vstupní i který přichází z omezené element jako `<select>` seznamu. To pomáhá zajistit, že nemáte uživatelům obejít ovládací prvky na stránce a odeslání formuláře.
2. V kódu stránky přidat jednotlivé ověřovací kontroly pro každý vstupní element pomocí metody `Validation` pomocné rutiny.

    Chcete-li vyhledat povinná pole, použijte `Validation.RequireField(field, [error message])` (pro jednotlivá pole) nebo `Validation.RequireFields(field1, field2, ...))` (pro seznam polí). Pro jiné typy ověřování, použijte `Validation.Add(field, ValidationType)`. Pro `ValidationType`, můžete použít tyto možnosti:

    `Validator.DateTime ([error message])`  
   `Validator.Decimal([error message])`  
   `Validator.EqualsTo(otherField [, error message])`  
   `Validator.Float([error message])`  
   `Validator.Integer([error message])`  
   `Validator.Range(min, max [, error message])`  
   `Validator.RegEx(pattern [, error message])`  
   `Validator.Required([error message])`  
   `Validator.StringLength(length)`  
   `Validator.Url([error message])`
3. Při odeslání stránky, zkontrolujte, zda byla ověření úspěšná kontrolou `Validation.IsValid`:

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    Pokud nejsou žádné chyby ověření, můžete přeskočit zpracování normální stránky. Například pokud účelem stránky je aktualizace databáze, můžete si udělat dokud byly opraveny všechny chyby ověřování.
4. Pokud nejsou chyby ověření, zobrazení chybové zprávy v značek pomocí `Html.ValidationSummary` nebo `Html.ValidationMessage`, nebo obojí.

Následující příklad ukazuje na stránku, který znázorňuje tyto kroky.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

Pokud chcete zjistit, jak funguje ověření, spusťte tuto stránku a úmyslně uděláte chyby. Zde je například vzhled stránky, pokud zapomenete zadat název postupu, pokud zadáte došlo, a při zadání neplatná data:

![Chyby ověření na vykreslené stránce](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a>Přidání ověřování na straně klienta

Ve výchozím nastavení, uživatelský vstup je ověřen po odeslání stránky – to znamená, ověření se provádí v serverovém kódu. Nevýhodou tohoto přístupu je, že uživatelé neznáte, že udělali chybu až po jejich odeslání stránky. Pokud formulář je dlouhá nebo komplexní, může být nepraktické uživatele zasílání zpráv o chybách, až po odeslání stránky.

Můžete přidat podporu k provedení ověření v klientského skriptu. V takovém případě ověření se provádí, jak uživatelé pracují v prohlížeči. Předpokládejme například, že zadáte, hodnota musí být celé číslo. Pokud uživatel zadá hodnotu – celé číslo, chyba je uvádět, jakmile uživatel opustí vstupní pole. Uživatelé získají okamžitou zpětnou vazbu, která je vhodné pro ně. Ověřování na základě klienta může také snížit počet pokusů, které má uživatel k odeslání formuláře a opravte několik chyb.

> [!NOTE]
> I když používáte ověřování na straně klienta, ověření se provádí vždy také v serverovém kódu. Provedení ověření v serverovém kódu je bezpečnostní opatření, v případě, že uživatelé obejít ověřování na základě klienta.


1. Zaregistrujte následující knihovny jazyka JavaScript na stránce:  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   Dva knihoven jsou načíst z síti pro doručování obsahu (CDN), takže nemusíte nutně mít je na počítači nebo serveru. Ale musí mít místní kopii *jquery.validate.unobtrusive.js*. Pokud dosud nepracujete pomocí služby WebMatrix šablony (jako je **Starter Site** ), který obsahuje knihovny, vytvořit web webové stránky, který je založen na **Starter Site**. Zkopírujte *.js* souboru k aktuální lokalitě.
2. V kód pro každý element, který jste se ověřování, že přidáte volání `Validation.For(field)`. Tato metoda vysílá atributy, které používají ověřování na straně klienta. (Místo generování skutečný kód jazyka JavaScript, metoda vysílá atributů, například `data-val-...`. Ověření nerušivého klienta, který používá jQuery pro práci podporu těchto atributů.)

Na následující stránce ukazuje, jak přidat funkce ověření klienta v příkladu uvedena výše.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

Ne všechny ověřovací kontroly spouští na klientovi. Ověření typu dat (celé číslo, datum a tak dále) na konkrétní nespouštět na straně klienta. Následující kontroly fungovat na klientovi a serveru:

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

V tomto příkladu nebude fungovat test pro platné datum v kódu klienta. Test se však provést v serverovém kódu.

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a>Formátování chyby ověření

Můžete řídit, jak se zobrazí chyby ověření definováním třídy CSS, které mají následující názvy vyhrazené:

- `field-validation-error`. Definuje výstup `Html.ValidationMessage` metoda při zobrazuje chybu.
- `field-validation-valid`. Definuje výstup `Html.ValidationMessage` v případě, že se nezobrazí žádná chyba.
- `input-validation-error`. Definuje jak `<input>` elementy vykresleny, když dojde k chybě. (Například tuto třídu můžete použít k nastavení barvy pozadí &lt;vstupní&gt; element na jinou barvu, pokud je jeho hodnota je neplatná.) Tato třída šablon stylů CSS se používá pouze během ověření klienta (v ASP.NET Web Pages 2).
- `input-validation-valid`. Definuje vzhled `<input>` prvky, pokud se nezobrazí žádná chyba.
- `validation-summary-errors`. Definuje výstup `Html.ValidationSummary` metoda zobrazit seznam chyb.
- `validation-summary-valid`. Definuje výstup `Html.ValidationSummary` v případě, že se nezobrazí žádná chyba.

Následující `<style>` bloku jsou pravidla pro chybové podmínky.

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

Pokud zahrnete tohoto bloku stylu příklad stránky z dříve v článku, zobrazení chyb bude vypadat jako na následujícím obrázku:

![Chyby ověření, které používají třídy CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> Pokud nepoužíváte ověření klienta v ASP.NET Web Pages 2, pro třídy CSS `<input>` elementy (`input-validation-error` a `input-validation-valid` nemají žádný vliv.


### <a name="static-and-dynamic-error-display"></a>Statické a dynamické chybových zpráv

Pravidla šablon stylů CSS vstoupí v párech, jako například `validation-summary-errors` a `validation-summary-valid`. Těmto párům umožňují definovat pravidla pro obě podmínky: chybový stav a podmínku "normální" (bez chyb). Je důležité si uvědomit, že vždy vykreslení značky pro zobrazení chyb, i když nejsou žádné chyby. Například, pokud stránka nemá `Html.ValidationSummary` metoda do kódu stránky zdroj bude obsahovat následující kód, i v případě, že při prvním požadavku na stránku:

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

Jinými slovy `Html.ValidationSummary` vždy vykreslí metoda `<div>` elementu a seznam, i když v seznamu chyb je prázdný. Podobně `Html.ValidationMessage` vždy vykreslí metoda `<span>` element jako zástupný symbol pro chybu jednotlivá pole, i když se nezobrazí žádná chyba.

V některých situacích s chybovou zprávou, může způsobit stránky přeformátování a může způsobit elementy na stránce pohyb. Pravidla šablon stylů CSS, která končit `-valid` umožňují definovat rozložení, které pomáhají zabránit tento problém. Například můžete definovat `field-validation-error` a `field-validation-valid` zároveň mít stejné pevnou velikost. Tímto způsobem oblasti zobrazení pro toto pole je statická a tok stránky se nezmění, pokud se zobrazí chybová zpráva.

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a>Ověřování dat, která nepřejde do stavu přímo od uživatelů

Někdy je nutné ověřit informace, které nepřejde do stavu přímo z formuláře HTML. Typickým příkladem je stránka, kde je hodnota předaná řetězce dotazu, jako v následujícím příkladu:

`http://server/myapp/EditClassInformation?classid=1022`

V takovém případě chcete Ujistěte se, že hodnota, která je předána na stránku (tady 1022 pro hodnotu `classid`) je platný. Nelze použít přímo `Validation` pomocná rutina pro toto ověření proveďte. Můžete však použít jiné funkce ověřování systému, jako je možnost zobrazit chybové zprávy ověření.

> [!NOTE] 
> 
> **Důležité** vždy ověřte hodnoty, které jste získali z *žádné* zdroje, včetně hodnot pole formuláře, hodnoty řetězce dotazu a hodnoty souboru cookie. Je snadné pro uživatele, kteří mají tyto hodnoty změnit (případně pro škodlivý účely). Proto je nutné zkontrolovat tyto hodnoty ochráníte vaší aplikace.


Následující příklad ukazuje, jak může ověřit hodnotu, která je předán v řetězci dotazu. Kód testuje, zda hodnota není prázdné a že je celé číslo.

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

Všimněte si, že test se provádí při žádost není odeslání formuláře (`if(!IsPost)`). Tento test by úspěšně prošel zpracováním při prvním požadavku na stránku, ale není požadavek po odeslání formuláře.

Chcete-li zobrazit tuto chybu, můžete přidat chyba do seznamu chyb ověřování voláním `Validation.AddFormError("message")`. Pokud tato stránka obsahuje volání `Html.ValidationSummary` metoda, chyba se zobrazí existuje, stejně jako chyba ověření uživatelského vstupu.

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Další prostředky

[Práce s formuláře HTML na webech ASP.NET – webové stránky](https://go.microsoft.com/fwlink/?LinkID=202892)
