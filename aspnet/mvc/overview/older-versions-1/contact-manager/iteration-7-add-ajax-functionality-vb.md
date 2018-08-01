---
uid: mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-vb
title: 'Iterace #7 – přidání funkcí Ajax (VB) | Dokumentace Microsoftu'
author: microsoft
description: V sedmé iteraci můžeme zlepšit rychlost reakce a výkon naší aplikace tak, že přidáte podporu pro Ajax.
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: f640e063-150e-453d-8cfc-7e54a6ce0f1e
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-vb
msc.type: authoredcontent
ms.openlocfilehash: b39a251acc96a78245238da5b82365958223e4c7
ms.sourcegitcommit: a25b572eaed21791230c85416f449f66a405ec19
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/01/2018
ms.locfileid: "39396176"
---
<a name="iteration-7--add-ajax-functionality-vb"></a>Iterace #7 – přidání funkcí Ajax (VB)
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout kód](iteration-7-add-ajax-functionality-vb/_static/contactmanager_7_vb1.zip)

> V sedmé iteraci můžeme zlepšit rychlost reakce a výkon naší aplikace tak, že přidáte podporu pro Ajax.


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

V této iterace aplikace Správce kontaktů Refaktorovat jsme naše aplikace pro použití jazyka Ajax. S využitím jazyka Ajax, uděláme naši aplikaci odezvu. Můžeme se vykreslování celou stránku, když potřebujeme aktualizovat jenom určité oblasti na stránce.

Jsme budete Refaktorovat náš Index zobrazení tak, že jsme nejsou potřeba t znovu zobrazit celé stránky pokaždé, když uživatel vybere nová skupina kontaktů. Místo toho když uživatel klepne skupinu kontaktů, jsme budete jenom aktualizace seznamu kontaktů a zbytek nechte stránky samostatně.

Pokud změníme způsob, jak naše odstranit propojení funguje. Místo samostatných potvrzovací stránku, zobrazíme potvrzovací dialogové okno jazyka JavaScript. Pokud potvrdíte, že chcete odstranit kontakt, operace HTTP DELETE se provádí na serveru z databáze odstranit záznam kontaktu.

Kromě toho jsme bude využívat jQuery přidání efekty animace do našich zobrazení indexu. Pokud nový seznam kontaktů je právě načtený ze serveru nám budete zobrazovat animace.

Nakonec provedeme výhod podpory technologie ASP.NET AJAX framework pro správu historii prohlížeče. Historie bodů vytvoříme pokaždé, když budeme provádět volání Ajax k aktualizaci seznamu kontaktů. Tímto způsobem prohlížeče dopředné a zpětné tlačítka budou fungovat.

## <a name="why-use-ajax"></a>Proč používat Ajax?

Pomocí rozhraní Ajax přináší řadu výhod. Nejprve přidání funkcí Ajax do aplikace výsledkem lepší výkon. V případě běžné webové aplikace celé stránky musí být účtovány zpět na server každém uživatel provede akci. Pokaždé, když provedete nějakou akci, zámky prohlížeče a uživatele musíte počkat, dokud celé stránky je načtena a zobrazí znovu.

To může být nepřijatelné dít v případě aplikace klasické pracovní plochy. Ale tradičně jsme žil s této nepříjemné zkušenosti v případě webové aplikace protože jsme, že můžeme udělat všechny lepší nevěděla. Domníváme se, že byly omezení webových aplikací, když ve skutečnosti byla pouze omezení naše imaginations.

V aplikaci Ajax zadávat t potřeba zpřístupnit činnost koncového uživatele pro zastavení pouze k aktualizaci stránky. Místo toho můžete provádět Asynchronní požadavek na pozadí, aktualizujte stránku. Platnost t zadávat uživateli Počkejte, dokud se aktualizuje části stránky.

S využitím jazyka Ajax, můžete také můžete zvýšit výkon vaší aplikace. Zvažte, jak funguje aplikace Správce kontaktů teď bez funkce Ajax. Když kliknete na skupinu kontaktů, musí zobrazí celé zobrazení Index znovu. V seznamu kontaktů a seznamu kontaktů skupin musí načíst ze serveru databáze. Všechna tato data musí být předán sítí z webového serveru webového prohlížeče.

Poté, co můžeme přidat funkcí Ajax do naší aplikace, ale můžeme se po kliknutí kontaktujte skupinu opětovné zobrazení celou stránku. Musíme už vzít kontaktní skupiny z databáze. Můžeme také nejsou potřeba t push celý Index zobrazení sítí. S využitím jazyka Ajax, jsme snížili množství práce, které musíte provést naše databázový server a jsme snížení objemu síťových přenosů vyžadovaným ze strany naši aplikaci.

## <a name="don-t-be-afraid-of-ajax"></a>Don t se obávat o Ajax

Někteří vývojáři vyhnout, pomocí rozhraní Ajax, protože se starat o starších prohlížečích. Chtějí se ujistěte se, že své webové aplikace budou i nadále fungovat, když přistupuje z prohlížeče, který nepodporuje jazyk JavaScript. Protože Ajax závisí na jazyce JavaScript, vyhněte se někteří vývojáři pomocí rozhraní Ajax.

Ale pokud jste pečlivě o tom, jak implementovat Ajax pak můžete vytvářet aplikace, které využívají službu vyšší úrovně a starších prohlížečích. Kontaktujte správce aplikace bude fungovat s prohlížečích, které podporují jazyka JavaScript a prohlížeče, které ji nesplňují.

Pokud používáte Správce kontaktů aplikace s prohlížečem, který podporuje JavaScript bude mít lepší výkon. Například po kliknutí na skupinu kontaktů, budou aktualizovány pouze oblast stránky, který zobrazuje kontakty.

Používáte, na druhé straně, kontaktujte správce aplikace s prohlížečem, který nepodporuje jazyk JavaScript (nebo, který má zakázaný JavaScript) bude mít méně žádoucí činnost koncového uživatele. Například když kliknete na skupinu kontaktů, celého zobrazení indexu musí být účtovány zpět do prohlížeče mohla zobrazit odpovídající seznamu kontaktů.

## <a name="adding-the-required-javascript-files"></a>Přidávání souborů požadovaných jazyka JavaScript

Potřebujeme budete používat tři soubory jazyka JavaScript pro přidání funkcí Ajax do naší aplikace. Všechny tři tyto soubory jsou zahrnuty ve složce Scripts nové aplikace ASP.NET MVC.

Pokud plánujete použití jazyka Ajax na více stránkách v aplikaci je vhodné zahrnout požadované soubory jazyka JavaScript do vaší aplikace s zobrazení stránky předlohy. Tímto způsobem soubory jazyka JavaScript se zahrnou všechny stránky v aplikaci automaticky.

Přidejte následující JavaScript zahrnuje uvnitř &lt;head&gt; značky hlavní stránku zobrazení:

[!code-html[Main](iteration-7-add-ajax-functionality-vb/samples/sample1.html)]

## <a name="refactoring-the-index-view-to-use-ajax"></a>Refaktoring zobrazení indexu pro použití jazyka Ajax

Umožní začít úpravou náš Index zobrazení tak, že kliknete na skupinu kontaktů aktualizuje pouze oblasti zobrazení, která zobrazuje kontakty s. Červeným rámečkem na obrázku 1 obsahuje oblast, která chcete aktualizovat.


[![Aktualizuje se jenom kontakty](iteration-7-add-ajax-functionality-vb/_static/image1.jpg)](iteration-7-add-ajax-functionality-vb/_static/image1.png)

**Obrázek 01**: aktualizuje pouze kontakty ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-7-add-ajax-functionality-vb/_static/image2.png))


Prvním krokem je oddělit část zobrazení, které chcete aktualizovat asynchronně do samostatných částečné (uživatelský ovládací prvek zobrazení). Část zobrazení indexu, které zobrazí tabulku kontaktů byl přesunut do částečné v informacích 1.

**Výpis 1 - Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample2.aspx)]

Všimněte si, že částečné v informacích 1 má jiný model, než zobrazení indexu. *Inherits* atribut &lt;% @ Page %&gt; direktiva Určuje, že části dědí z ViewUserControl&lt;skupiny&gt; třídy.

Aktualizované zobrazení indexu je obsažen v informacích 2.

**Výpis 2 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample3.aspx)]

Existují dvě věci, které byste si měli všimnout o aktualizaci zobrazení na výpis 2. Napřed si všimněte, že veškerý obsah přesunut do částečnou nahradí volání Html.RenderPartial(). Metoda Html.RenderPartial() se volá při prvním vyžádání zobrazení indexu zobrazíte Počáteční sada kontaktů.

Za druhé Všimněte si, že Html.ActionLink() slouží k zobrazení skupiny kontaktů se nahradil údajem Ajax.ActionLink(). Ajax.ActionLink() je volána s následujícími parametry:

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample4.aspx)]

První parametr představuje text, který se zobrazí jako odkaz, druhý parametr představuje hodnoty trasy a třetí parametr představuje možnosti Ajax. V tomto případě používáme možnost UpdateTargetId Ajax tak, aby odkazoval na HTML &lt;div&gt; značku, kterou chcete aktualizovat, až se dokončí požadavek Ajax. Chceme, aby se aktualizovat &lt;div&gt; značky pomocí nového seznamu kontaktů.

Aktualizovaná metoda Index() kontakt kontroleru je obsažen v informacích 3.

**Výpis 3 - Controllers\ContactController.vb (metoda indexu)**

[!code-vb[Main](iteration-7-add-ajax-functionality-vb/samples/sample5.vb)]

Aktualizované Index() akce podmíněně vrátí jednu ze dvou kroků. Pokud je požadavek Ajax vyvolat Index() akce kontroleru vrátí částečné. V opačném případě vrátí akce Index() celého zobrazení.

Všimněte si, že akce Index() nemusí vrátit tolik dat, když uživatel vyvolá požadavek Ajax. V rámci normálního požadavek akce indexu vrátí seznam všech skupin kontaktů a vybrané skupiny kontaktů. Index() akce v souvislosti s požadavek Ajax se vrátí pouze vybrané skupiny. AJAX znamená méně práce na vašem databázovém serveru.

Naše upravené zobrazení indexu funguje v případě prohlížeče vyšší úrovně a nižší úrovně. Pokud kliknete na skupinu kontaktů a váš prohlížeč podporuje JavaScript, je aktualizován pouze oblasti zobrazení, která obsahuje seznam kontaktů. Pokud na druhé straně váš prohlížeč nepodporuje jazyk JavaScript, se aktualizuje celého zobrazení.


Naše aktualizované zobrazení indexu má jeden problém. Po kliknutí na skupinu kontaktů, není zvýrazní vybrané skupiny. Protože mimo oblast, která se aktualizuje během požadavek Ajax se zobrazí seznam skupin, získejte není zvýrazněný ke správné skupině. Tento problém opravíme v další části.


## <a name="adding-jquery-animation-effects"></a>Přidání efekty animace jQuery

Za normálních okolností po kliknutí na odkaz na webové stránce, slouží indikátor průběhu prohlížeče k detekci, zda v prohlížeči se aktivně načítají aktualizovaný obsah. Při provádění požadavek Ajax, na druhé straně indikátor průběhu prohlížeč není uveden žádný průběh. Díky tomu uživatelé nervové. Jak poznáte, jestli má zmrazené prohlížeče?

Existuje několik způsobů, které můžete určit na uživatele, že práce se provádí při provádění požadavek Ajax. Jedním z přístupů je zobrazíte jednoduché animace. Například je lze zesvětlit oblast při zahájí požadavek Ajax a fade v oblasti po dokončení žádosti.

Pomocí knihovny jQuery, který je součástí rozhraní Microsoft ASP.NET MVC, chcete-li vytvořit efekty animace. Výpis 4 je součástí aktualizované zobrazení indexu.

**Část 4 – Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample6.aspx)]

Všimněte si, že aktualizovaná zobrazení indexu obsahuje tři nové funkce jazyka JavaScript. První dvě funkce pomocí jQuery zesvětlit a fade v seznamu kontaktů, po kliknutí na nová skupina kontaktů. Třetí funkce zobrazí chybová zpráva při výsledky požadavku Ajax k chybě (například vypršel časový limit sítě).

První funkce také postará zvýraznění vybranou skupinu. Třída = vybraný atribut se přidá na nadřazený element (LI element) elementu kliknutí. Znovu jQuery umožňuje snadno vybrat správný prvek a přidat třídu šablony stylů CSS.

Tyto skripty jsou svázány se propojení skupiny pomocí parametru Ajax.ActionLink() AjaxOptions. Aktualizované volání metody Ajax.ActionLink() vypadá takto:

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample7.aspx)]

## <a name="adding-browser-history-support"></a>Přidání podpory prohlížeče historie

Za normálních okolností po kliknutí na odkaz aktualizovat stránku, se aktualizuje historii prohlížeče. Tímto způsobem je můžete kliknout na prohlížeči tlačítko Zpět a vrátit v čase do předchozího stavu stránky. Například pokud klikněte na skupinu kontaktní přátel a potom klikněte na skupinu kontaktní Business, můžete kliknutím na prohlížeči tlačítko Zpět chcete vrátit do stavu stránky při nebyla vybrána skupina kontaktní přátel.

Bohužel provádí požadavek Ajax prohlížeče historie automaticky neaktualizuje. Pokud kliknete na skupinu kontaktů a načíst seznam odpovídajících kontakty s požadavek Ajax, není aktualizován historii prohlížeče. Přejděte zpátky na skupinu kontaktů po výběru nové skupiny kontaktů nelze použít prohlížeči tlačítko Zpět.

Pokud chcete tlačítko, aby je uživatelé mohli použít prohlížeč zpět po provedení odesílání požadavků Ajax musíte provést trochu více práce. Budete muset využít výhod funkce správy prohlížeče historie integrováno v rozhraní ASP.NET AJAX Framework.

Prohlížeč historie jazyka ASP.NET AJAX, je třeba provést tři věci:

1. Povolte prohlížeč historie tak, že nastavíte vlastnost enableBrowserHistory na hodnotu true.
2. Uložte historii body při změně stavu zobrazení pomocí volání metody addHistoryPoint().
3. Rekonstrukce stav zobrazení, když je vyvolána událost navigace.

Aktualizované zobrazení indexu je obsažen v informacích 5.

**Výpis 5 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample8.aspx)]

V výpis 5 je historii prohlížeče povolili ve funkci pageInit(). Funkce pageInit() slouží také k nastavení obslužnou rutinu události pro událost navigace. Pokaždé, když prohlížeč-vpřed nebo tlačítko zpět způsobí, že se stav na stránce změnit, je vyvolána událost navigace.

BeginContactList() metoda je volána, když kliknete na skupinu kontaktů. Tato metoda vytvoří nový bod historie zavoláním metody addHistoryPoint(). Id skupiny kontaktů kliknutí se přidá do historie.

Id skupiny je načten z "expando" atribut propojení kontaktní skupiny. Odkaz je vykreslen pomocí následujícího volání Ajax.ActionLink().

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample9.aspx)]

Poslední parametr předaný Ajax.ActionLink() přidá "expando" atribut s názvem skupiny na propojení (malá písmena pro kompatibilitu XHTML).

Když uživatel dosáhne prohlížeče zpět nebo tlačítko Předat dál, je vyvolána událost navigace a navigate() metoda je volána. Tato metoda aktualizuje kontakty na této stránce tak, aby odpovídaly stavu stránky, která odpovídá bod historie prohlížeče předaný metodě Navigovat zobrazena.

## <a name="performing-ajax-deletes"></a>Odstraní provádění Ajax

V současné době Chcete-li odstranit kontakt, budete muset kliknout na odkaz pro odstranění a poté klikněte na tlačítko Odstranit zobrazí na stránce potvrzení odstranění (viz obrázek 2). Vypadá to, že jako velké množství žádostí stránky něco jednoduchého jako odstraňuje se záznam v databázi.


[![Na stránce potvrzení odstranění](iteration-7-add-ajax-functionality-vb/_static/image2.jpg)](iteration-7-add-ajax-functionality-vb/_static/image3.png)

**Obrázek 02**: na stránce potvrzení odstranění ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-7-add-ajax-functionality-vb/_static/image4.png))


Je lákavé přejděte na stránku potvrzení odstranění a odstranit a kontaktovat přímo ze zobrazení pro Index. Neměli byste tento pokušení a vzhledem k tomu, že si tento postup se otevře aplikace bezpečnostní díry. Obecně platí don t chcete provést operaci HTTP GET při vyvolání akce, která změní stav vaší webové aplikace. Při provádění odstranění, chcete provádět metody POST protokolu HTTP, nebo ještě lépe, operace HTTP DELETE.

Odkaz pro odstranění je součástí částečné ContactList. Najít aktualizovanou verzi částečné ContactList je obsažen v informacích 6.

**Výpis 6 - Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample10.aspx)]

Odstranit odkaz je vykreslen pomocí následujícího volání metody Ajax.ImageActionLink():

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample11.aspx)]

> [!NOTE] 
> 
> Ajax.ImageActionLink() není standardní součástí rozhraní ASP.NET MVC. Ajax.ImageActionLink() je vlastní pomocné metody zahrnutý v projektu správce kontaktů.


Parametr AjaxOptions má dvě vlastnosti. Nejprve potvrdit vlastnost slouží k zobrazení potvrzovací dialogové okno automaticky otevírané okno jazyka JavaScript. Za druhé Vlastnost HttpMethod slouží k provádění operací HTTP DELETE.

Výpis 7 obsahuje novou AjaxDelete() akci, která byla přidána k řadiči kontaktu.

**Výpis 7 - Controllers\ContactController.vb (AjaxDelete)**   

[!code-vb[Main](iteration-7-add-ajax-functionality-vb/samples/sample12.vb)]

Akce AjaxDelete() je opatřen atributem AcceptVerbs. Tento atribut brání akce s výjimkou vyvolání jakékoli operace HTTP než operace HTTP DELETE. Zejména nejde vyvolat tuto akci s HTTP GET.

Po odstranění záznamu v databázi, je třeba zobrazit aktualizovaný seznam kontaktů, který neobsahuje odstraněný záznam. Metoda AjaxDelete() vrátí částečné ContactList aktualizovaný seznam kontaktů.

## <a name="summary"></a>Souhrn

V této iterace jsme přidali funkcí Ajax do naší aplikace Správce kontaktů. Jsme použili Ajax zlepšit rychlost reakce a výkon naší aplikace.

Nejprve jsme rozdělili zobrazení indexu tak, aby kliknutí kontaktujte skupinu neaktualizuje celého zobrazení. Místo toho kliknutí kontaktujte skupinu aktualizace pouze v seznamu kontaktů.

Dále jsme použili efekty animace jQuery zesvětlit a fade v seznamu kontaktů. Přidání animace do aplikace Ajax je možné poskytnout uživatelům aplikace ekvivalentem indikátor průběhu prohlížeče.

Přidali jsme také podpora prohlížeče historie do naší aplikace Ajax. Povolili jsme uživatelům klikněte na prohlížeč zpět a vpřed tlačítka pro změnu stavu zobrazení indexu.

Nakonec jsme vytvořili odkaz pro odstranění, který podporuje operace HTTP DELETE. Provedením Ajax odstraní povolíme uživatelům odstranit záznamy v databázi bez nutnosti uživateli požadavek na potvrzovací stránku další odstranit.

> [!div class="step-by-step"]
> [Předchozí](iteration-6-use-test-driven-development-vb.md)
