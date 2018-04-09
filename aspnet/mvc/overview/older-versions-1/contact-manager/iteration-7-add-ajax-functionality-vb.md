---
uid: mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-vb
title: 'Iterace #7 – funkce Ajax přidat (VB) | Microsoft Docs'
author: microsoft
description: V sedmého iteraci jsme přidáním podpory pro Ajax zvýšit rychlost reakce a výkon aplikace.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f640e063-150e-453d-8cfc-7e54a6ce0f1e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-vb
msc.type: authoredcontent
ms.openlocfilehash: 35d961ee39d7b87a31c7208645148b45c7b0c563
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="iteration-7--add-ajax-functionality-vb"></a>Iterace #7 – funkce Ajax přidat (VB)
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhněte si kód](iteration-7-add-ajax-functionality-vb/_static/contactmanager_7_vb1.zip)

> V sedmého iteraci jsme přidáním podpory pro Ajax zvýšit rychlost reakce a výkon aplikace.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Vytvoření aplikace ASP.NET MVC správy kontaktů (VB)
  

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

V této iteraci aplikace, obraťte se na správce Refaktorovat jsme naši aplikaci nutné používat Ajax. Využitím Ajax, zajišťujeme, naše aplikace rychleji reagovat. Nemůžeme se můžete vyhnout vykreslování celou stránku, pokud je potřeba aktualizovat jenom určité oblasti na stránce.

Naše zobrazení indexu jsme budete Refaktorovat tak, aby jsme nejsou zobrazeny t potřeba znovu zobrazit celé stránky vždy, když uživatel vybere nové skupině kontaktů. Místo toho když někdo klikne skupinu kontaktů, jsme budete právě aktualizace seznamu kontaktů a ponechejte zbývající stránky samostatně.

Také Změníme způsob, jak naše odstranění propojení funguje. Místo zobrazení samostatné potvrzovací stránku, zobrazujeme dialogové okno potvrzení JavaScript. Pokud můžete potvrdit, že chcete odstranit a obraťte se na, operace HTTP DELETE adresovat serveru z databáze odstranit záznam kontaktu.

Kromě toho jsme bude využít výhod jQuery přidat efekty animace do našich zobrazení indexu. Když je právě nového seznamu kontaktů načtených ze serveru nám budete zobrazí animace.

Nakonec jsme budete využít výhod podpory framework prvku ASP.NET AJAX pro správu historii prohlížeče. Vytvoříme body historie vždy, když jsme provádět volání Ajax aktualizace seznamu kontaktů. Tímto způsobem prohlížeče zpětné a předat dál tlačítka nebude fungovat.

## <a name="why-use-ajax"></a>Proč používat Ajax?

Pomocí rozhraní Ajax má mnoho výhod. Nejprve přidání funkcí Ajax aplikaci výsledkem lepší uživatelské prostředí. V normálním webové aplikace celou stránku musí být účtovány zpět na server každém uživatel provede akci. Při každém provedení akce zámků prohlížeče a uživatel musí Počkejte, dokud se celá stránka načtených a zobrazí znovu.

To by mělo nepřijatelný prostředí v případě aplikace. Ale tradičně, jsme žít s této nepříjemné zkušenosti v případě webové aplikace protože jsme není věděli, že můžeme udělat lépe žádné. Můžeme představit, že se o omezení webových aplikací, když ve skutečnosti se jednalo o omezení naše imaginations.

V aplikaci Ajax neuděláte t potřeba uvést činnost koncového uživatele k zastavení právě k aktualizaci stránky. Místo toho můžete provést asynchronní požadavek na pozadí, aktualizujte stránku. Platnost t neuděláte uživateli čekejte získá aktualizace součástí stránky.

Využitím Ajax, také může zvýšit výkon vaší aplikace. Zvažte, jak funguje obraťte se na správce aplikace teď bez funkci Ajax. Po kliknutí na tlačítko skupinu kontaktů, musí být celý Index zobrazení zobrazí znovu. Seznamu kontaktů a seznamu skupin kontaktní musí načíst ze serveru databáze. Všechna tato data musí být předán napříč sítě z webového serveru do webového prohlížeče.

Po přidáme funkci Ajax do naší aplikaci, ale jsme se můžete vyhnout opětovné zobrazení celou stránku, když uživatel klikne skupinu kontaktů. Už je potřeba získat skupiny kontaktů z databáze. Můžeme také nejsou zobrazeny potřeba t push celý Index zobrazení v drátové síti. Využitím Ajax, jsme snížit množství práce, kterou musí provést naše databázový server a jsme snížit objem síťového přenosu, které jsou potřeba v naší aplikaci.

## <a name="don-t-be-afraid-of-ajax"></a>Tento t se obávat z Ajax

Někteří vývojáři vyhýbat se používání Ajax, protože se starat o prohlížeče nižší úrovně. Chtějí, abyste měli jistotu, že jejich webových aplikací budou i nadále fungovat při přístupu prohlížeč, který nepodporuje jazyk JavaScript. Protože Ajax závisí na JavaScript, někteří vývojáři vyhýbat se používání Ajax.

Ale pokud nezapomenete o tom, jak implementovat Ajax pak můžete vytvořit aplikace, které pracují s vyšší úrovně a starší verze prohlížeče. Obraťte se na správce aplikace bude fungovat s prohlížečů, které podporují JavaScript a prohlížeče, které nepodporují.

Pokud používáte aplikace obraťte se na správce s prohlížečem, který podporuje JavaScript bude mít lepší uživatelské prostředí. Například když kliknete na skupinu kontaktů, budou aktualizovány pouze oblast stránky, která zobrazuje kontakty.

Pokud na druhé straně, obraťte se na správce aplikace použijete s prohlížečem, který nepodporuje jazyk JavaScript (nebo který má JavaScript zakázaná) bude mít mírně méně vhodné uživatelské prostředí. Například když kliknete na skupinu kontaktů, celého zobrazení Index musí být účtovány zpět do prohlížeče aby bylo možné zobrazit odpovídající seznamu kontaktů.

## <a name="adding-the-required-javascript-files"></a>Přidávání souborů požadovaných JavaScript

Budeme potřebovat použít tři soubory JavaScript přidat funkci Ajax do naší aplikace. Všechny tři tyto soubory jsou zahrnuty do složky Scripts nové aplikace ASP.NET MVC.

Pokud budete chtít použít Ajax na více stránkách v aplikaci je vhodné zahrnout požadované soubory JavaScript do vaší aplikace s zobrazení stránky předlohy. Tímto způsobem soubory JavaScript bude obsahovat všechny stránky v aplikaci automaticky.

Přidejte následující JavaScript zahrnuje uvnitř &lt;head&gt; značky hlavní stránky zobrazení:

[!code-html[Main](iteration-7-add-ajax-functionality-vb/samples/sample1.html)]

## <a name="refactoring-the-index-view-to-use-ajax"></a>Refaktoring zobrazení indexu pomocí rozhraní Ajax

Umožní s spustit změnou naše Index zobrazení tak, aby kliknutím na skupinu kontaktů aktualizuje pouze oblasti zobrazení, které zobrazí kontakty. Červeným rámečkem na obrázku 1 obsahuje oblast, kterou chcete aktualizovat.


[![Aktualizace pouze kontakty](iteration-7-add-ajax-functionality-vb/_static/image1.jpg)](iteration-7-add-ajax-functionality-vb/_static/image1.png)

**Obrázek 01**: aktualizace pouze kontakty ([Kliknutím zobrazit obrázek v plné velikosti](iteration-7-add-ajax-functionality-vb/_static/image2.png))


Prvním krokem je oddělení součástí zobrazení, které chcete aktualizovat asynchronně do samostatné částečné (zobrazení uživatelský ovládací prvek). V části zobrazení Index, který zobrazí seznam kontaktů byl přesunut do částečného v výpis 1.

**Listing 1 - Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample2.aspx)]

Všimněte si, že partial v výpis 1 má jiný model, než zobrazení indexu. *Inherits* atribut &lt;% @ % stránky&gt; – direktiva Určuje, že s částečným dědí z ViewUserControl&lt;skupiny&gt; – třída.

Aktualizovaná zobrazení indexu je součástí výpis 2.

**Listing 2 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample3.aspx)]

Existují dvě věci, které byste měli zaznamenat o aktualizaci zobrazení v výpis 2. Všimněte si nejprve veškerý obsah přesunut do s částečným se nahradí volání Html.RenderPartial(). Html.RenderPartial() metoda je volána, když zobrazení indexu prvním vyžádání aby bylo možné zobrazit počáteční sadu kontaktů.

Druhý Všimněte si, že Html.ActionLink() slouží k zobrazení kontaktní skupiny nahradilo Ajax.ActionLink(). Ajax.ActionLink() je volána s následujícími parametry:

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample4.aspx)]

První parametr představuje text, který se zobrazí jako odkaz, druhý parametr představuje hodnoty trasy a třetí parametr představuje možnosti Ajax. V tomto případě používáme možnost UpdateTargetId Ajax tak, aby odkazoval HTML &lt;div&gt; značky, který chcete aktualizovat po dokončení požadavek Ajax. Chceme aktualizovat &lt;div&gt; značky s nového seznamu kontaktů.

Aktualizovaná metoda Index() řadiče kontakt je součástí výpis 3.

**Výpis 3 - Controllers\ContactController.vb (Index metoda)**

[!code-vb[Main](iteration-7-add-ajax-functionality-vb/samples/sample5.vb)]

Aktualizované Index() akce podmíněně vrací jednu ze dvou akcí. Pokud je požadavek Ajax vyvolat Index() akce kontroleru vrátí částečné. Akce Index(), jinak vrátí celého zobrazení.

Všimněte si, že akce Index() nemusí vrátit tolik dat, když vyvolat požadavek Ajax. V kontextu žádost o normální Index akce vrátí seznam všech skupin kontaktů a vybrané skupiny kontaktů. V souvislosti s požadavek Ajax se vrátí akce Index() pouze vybrané skupiny. AJAX znamená méně práce na databázovém serveru.

Naše upravené zobrazení indexu funguje v případě vyšší úrovně a starší verze prohlížeče. Pokud kliknete na tlačítko skupinu kontaktů a prohlížeč podporuje JavaScript, je aktualizovat jenom oblasti zobrazení, který obsahuje seznam kontaktů. Pokud na druhé straně váš prohlížeč nepodporuje jazyk JavaScript, je aktualizována celého zobrazení.


Naše aktualizované zobrazení indexu má jeden problém. Když kliknete na skupinu kontaktů, není zvýrazněn vybrané skupiny. Protože mimo oblast, která se aktualizuje během požadavek Ajax se zobrazí seznam skupin, získat zvýrazněná není pravé skupiny. V další části jsme vám tento problém vyřešit.


## <a name="adding-jquery-animation-effects"></a>Přidání efekty animace jQuery

Za normálních okolností když kliknete na odkaz na webové stránce, můžete indikátor průběhu prohlížeče ke zjištění, zda prohlížeč se aktivně načítají aktualizovaný obsah. Při provádění požadavek Ajax, na druhé straně indikátor průběhu prohlížeče nezobrazuje žádné průběh. To můžete provést za nervové uživatele. Jak poznáte, jestli má pozastaveny prohlížeče?

Existuje několik způsobů, které mohou použít pro uživatele, že probíhají práce při provádění požadavek Ajax. Jeden z přístupů je zobrazíte jednoduché animace. Například je může objevovat se v oblasti při požadavek Ajax zahájí a objevovat se oblast po dokončení žádosti.

Použijeme knihovna jQuery, která je součástí Microsoft ASP.NET MVC framework, chcete-li vytvořit efekty animace. Aktualizovaná zobrazení indexu je součástí výpis 4.

**Listing 4 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample6.aspx)]

Všimněte si, že aktualizovaný Index zobrazení obsahuje tři nové funkce jazyka JavaScript. První dvě funkce pomocí jQuery objevovat a po kliknutí na tlačítko Nová skupina kontaktů objevovat v seznamu kontaktů. Třetí funkce zobrazí chybové hlášení výsledky požadavek Ajax v chybu (například časový limit sítě).

První funkce také postará zvýraznění vybrané skupiny. Třída = vybraný atribut se přidá na nadřazený element (LI element) elementu klikli. Znovu jQuery usnadňuje vyberte správné elementu a přidejte třídu CSS.

Tyto skripty jsou svázané s odkazy skupiny pomocí parametru Ajax.ActionLink() AjaxOptions. Aktualizované volání metody Ajax.ActionLink() vypadá takto:

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample7.aspx)]

## <a name="adding-browser-history-support"></a>Přidání podpory prohlížeče historie

Za normálních okolností při kliknutí na odkaz aktualizace stránky se aktualizuje historii prohlížeče. Tímto způsobem, kliknutím na tlačítko Zpět prohlížeče a přesunout zpět v čase do předchozího stavu stránky. Například pokud klikněte na skupinu kontaktní přátel a pak klikněte na skupinu kontaktní firmy, kliknutím na tlačítko Zpět prohlížeče a přejděte zpátky do stavu stránky, pokud nebyla vybrána skupina kontaktní přátel.

Bohužel se provádí požadavek Ajax prohlížeče historie automaticky neaktualizuje. Pokud kliknete na tlačítko skupinu kontaktů a s požadavek Ajax se načíst seznam odpovídající kontakty, není aktualizován historii prohlížeče. Nelze použít na tlačítko Zpět v prohlížeči přejděte zpátky na skupinu kontaktů po výběru kontaktní novou skupinu.

Pokud chcete, aby uživatelé mohli použít prohlížeč zpět tlačítko po provedení požadavky Ajax budete muset provést trochu další práci. Je třeba využít výhod funkce správy prohlížeče historie součástí rozhraní ASP.NET AJAX.

Prohlížeč historie prvku ASP.NET AJAX, musíte udělat tři věci:

1. Povolte prohlížeč historie nastavení enableBrowserHistory vlastnost na hodnotu true.
2. Při změně stavu zobrazení pomocí volání metody addHistoryPoint(), uložte body historie.
3. Rekonstrukci stav zobrazení, když je vyvolána událost přejděte.

Aktualizovaná zobrazení indexu je součástí výpis 5.

**Listing 5 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample8.aspx)]

Ve výpisu 5 je ve funkci pageInit() povolené prohlížeče historie. Funkce pageInit() slouží také k nastavení obslužné rutiny události pro událost přejděte. Přejděte událost se vyvolá, vždy, když prohlížeč dál nebo na tlačítko zpět způsobí, že stav stránky, chcete-li změnit.

BeginContactList() metoda je volána, když kliknete skupinu kontaktů. Tato metoda vytvoří nový bod historie voláním metody addHistoryPoint(). Id skupiny kontaktů klikli je přidán k historii.

Id skupiny se načtou z atribut expando na odkaz kontaktní skupiny. Odkaz je vykreslen s následující volání Ajax.ActionLink().

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample9.aspx)]

Poslední parametr předaný Ajax.ActionLink() přidá expando atribut s názvem groupid odkaz (malá písmena pro kompatibilitu XHTML).

Pokud se uživatel dotkne prohlížeče zpět nebo tlačítko přeposlat, je vyvolána událost přejděte a navigate() metoda je volána. Tato metoda aktualizace kontakty zobrazí na stránce tak, aby odpovídaly stav stránky, která odpovídá bod historie prohlížeče předaný metodě přejděte.

## <a name="performing-ajax-deletes"></a>Provádění Ajax odstraní

Aktuálně, chcete-li odstranit a kontaktovat, budete muset klikněte na odkaz odstranit a pak klikněte na tlačítko Odstranit zobrazí na stránce potvrzení odstranění (viz obrázek 2). Tohle vypadá jako mnoho požadavků stránky jednoduché jako odstraňování záznamů databáze něco udělat.


[![Stránka potvrzení odstranění](iteration-7-add-ajax-functionality-vb/_static/image2.jpg)](iteration-7-add-ajax-functionality-vb/_static/image3.png)

**Obrázek 02**: stránka potvrzení odstranění ([Kliknutím zobrazit obrázek v plné velikosti](iteration-7-add-ajax-functionality-vb/_static/image4.png))


Je tempting stránku potvrzení odstranění vynechat a odstraňovat a obraťte se přímo ze zobrazení indexu. Neměli byste toto riziko a protože tak tento přístup, otevře se vaše aplikace a celistvosti. Obecně platí tento t chcete provést operaci HTTP GET při vyvolání akce, která změní stav webové aplikace. Při provádění typu odstranění, budete chtít provést HTTP POST nebo ještě lepší operace HTTP DELETE.

Odstraní propojení je obsažený v částečné ContactList. Aktualizovanou verzi částečné ContactList je obsažený v výpis 6.

**Listing 6 - Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample10.aspx)]

Odstraňte odkaz je vykreslen s následující volání pro metodu Ajax.ImageActionLink():

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample11.aspx)]

> [!NOTE] 
> 
> Ajax.ImageActionLink() není standardní součástí rozhraní ASP.NET MVC. Ajax.ImageActionLink() je vlastní pomocné metody, zahrnutý v projektu obraťte se na správce.


Parametr AjaxOptions má dvě vlastnosti. Nejprve potvrdit vlastnost slouží k zobrazení dialogové okno místní JavaScript potvrzení. Druhý HttpMethod vlastnost se používá k provedení určité operace HTTP DELETE.

Výpis 7 obsahuje novou AjaxDelete() akci, která byla přidána k obraťte se na řadiči.

**Výpis 7 – Controllers\ContactController.vb (AjaxDelete)**   

[!code-vb[Main](iteration-7-add-ajax-functionality-vb/samples/sample12.vb)]

Akce AjaxDelete() opatřen atributem AcceptVerbs. Tento atribut brání provedení akce volaná s výjimkou všechny operace HTTP než operace HTTP DELETE. Konkrétně nemohou vyvolat tuto akci s HTTP GET.

Po odstranění záznamů databáze, je třeba zobrazit aktualizovaný seznam kontaktů, který neobsahuje odstraněného záznamu. Metoda AjaxDelete() vrátí částečné ContactList a aktualizovaného seznamu kontaktů.

## <a name="summary"></a>Souhrn

V této iteraci jsme přidali funkci Ajax pro naši aplikaci obraťte se na správce. Jsme Ajax používají ke zlepšení rychlost reakce a výkon aplikace.

Nejdřív jsme rozdělili zobrazení indexu tak, aby kliknutím na skupinu kontaktů neaktualizuje celého zobrazení. Místo toho klikněte na skupinu kontaktů aktualizuje jenom seznamu kontaktů.

V dalším kroku jsme použili efekty animace jQuery a vykreslit objevovat v seznamu kontaktů. Přidání animace do Ajax aplikace umožňuje uživatelům aplikace ekvivalentem indikátor průběhu prohlížeče.

Také jsme přidali prohlížeče historie podporu pro naši aplikaci Ajax. Můžeme povolit uživatelům klikněte na tlačítko Zpět v prohlížeči a předávat je chcete-li změnit stav zobrazení indexu.

Nakonec jsme vytvořili odstraní propojení, které podporuje operace HTTP DELETE. Provedením Ajax odstranění jsme povolit uživatelům odstranit záznamů databáze bez nutnosti uživateli požadavek na potvrzovací stránku další odstranit.

> [!div class="step-by-step"]
> [Předchozí](iteration-6-use-test-driven-development-vb.md)
