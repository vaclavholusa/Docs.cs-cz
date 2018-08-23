---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-cs
title: 'Iterace #6 – použití vývoje řízeného (C#) | Dokumentace Microsoftu'
author: microsoft
description: V této iterace šestého přidáme nové funkce do naší aplikace tak, že nejprve zápis testů jednotek a psaní kódu pro testování částí. V této iterace...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 013c3c26-7dc3-41d1-8064-f233c86008b5
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-cs
msc.type: authoredcontent
ms.openlocfilehash: 3c4358a1b979ab95d8ac25551e21ee95d75e5eae
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754541"
---
<a name="iteration-6--use-test-driven-development-c"></a>Iterace #6 – použití vývoje řízeného (C#)
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout kód](iteration-6-use-test-driven-development-cs/_static/contactmanager_6_cs1.zip)

> V této iterace šestého přidáme nové funkce do naší aplikace tak, že nejprve zápis testů jednotek a psaní kódu pro testování částí. V této iterace můžeme přidat skupiny kontaktů.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Vytvoření aplikace ASP.NET MVC pro správu kontaktů (C#)
  

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

V předchozí iteraci aplikace Správce kontaktů jsme vytvořili testů jednotek, které poskytují bezpečnostní pro našeho kódu. Motivace pro vytváření testů jednotek bylo zajistit odolnost změnit náš kód. S testy jednotek v místě můžeme využívá elastic provádět změny do našeho kódu a okamžitě zjistit, jestli jsme zrušili existující funkce.

V této iterace používáme testů jednotek pro úplně jinou účel. V této iterace používáme jednotkové testy jako součást filozofie návrhu aplikace volá *vývoj řízený testováním*. Můžete Nácvik vývoje řízeného testováním, nejprve psaní testů a teprve pak píšete kód proti testy.

Přesněji řečeno, při využívání vývoj řízený testováním, existují tři kroky, které můžete provést při vytváření kódu (červenou nebo zelenou nebo Refaktorujte):

1. Napsat Jednotkový test, který selže (červený)
2. Napsat kód, který projde testem jednotek (zelený)
3. Refaktoring kódu (Refaktorujte)

Nejprve můžete napsat Jednotkový test. Jednotkový test by měl vyjadřovat chovat svůj záměr pro jak očekáváte, že váš kód. Při prvním vytvoření testování částí, test jednotky by selhat. Test by měl nezdaří, protože dosud jste nenapsali žádný další kód aplikace, která splňuje testu.

Dále napište právě dostatek kódu v pořadí pro test jednotek proběhl úspěšně. Cílem je napsat kód tak, jak laziest, sloppiest a nejrychlejší možné. Neměli ztrácet čas přemýšlení o architektuře vaší aplikace. Místo toho by měla soustředit na psaní minimální množství kódu, které jsou nezbytné pro splnění záměr vyjádřena pomocí testování částí.

Nakonec poté, co jste napsali dostatek kódu, které můžete počkáte a zvážíte architektury aplikace. V tomto kroku, přepište (Refaktorujte) kódu s využitím návrh softwaru vzory – například použitému vzoru – tak, aby váš kód je jednodušší údržbu. Váš kód v tomto kroku můžete ucelená přepsat, protože kódu je pokryta testy jednotek.

Existuje mnoho výhod, které jsou výsledkem ocení vývoj řízený testováním. První vývoj řízený testováním vynutí vám umožní zaměřit se na kód, který má být zapsán skutečně potřebuje. Protože jsou neustále zaměřené na právě psaní dostatek kódu určitého testu předat, nebudou moci wandering do plevelů a zápis obrovského množství kódu, který se nikdy nepoužívejte.

Za druhé návrh metodologie "nejprve otestovat" vynutí psaní kódu z hlediska využití vašeho kódu. Jinými slovy při využívání vývoj řízený testováním, neustále psaní testů z pohledu uživatele. Proto může způsobit testy řízený vývoj rozhraní API přehlednější a pomohou lépe porozumět.

Nakonec vývoj řízený testováním vynutí pro psaní jednotkových testů jako součást normální proces vytváření aplikace. Jak blíží konečný termín projektu testování je obvykle první věc, se kterou jdeme okna. Při využívání vývoj řízený testováním na druhé straně, se pravděpodobně bude porozumí o zápis testů jednotek, protože vývoj řízený testováním provede testování částí centrální proces vytváření aplikace.

> [!NOTE] 
> 
> Další informace o vývoj řízený testováním, můžu jenom doporučit, abyste si přečetli Michael peří knihy **práci efektivně pomocí starší verze kódu**.


V této iterace přidáme nové funkce do naší aplikace Správce kontaktů. Přidáváme podporu pro skupiny kontaktu. Můžete použít kontakt skupiny pro uspořádání vašich kontaktů do kategorií, například obchodní a skupiny typu Friend.

Tato nová funkce přidáme k naší aplikace pomocí následujícího postupu řízeného vývoje. Budeme psát testech jednotek nejprve prostě budeme psát všechny našeho kódu proti tyto testy.

## <a name="what-gets-tested"></a>Co získá testování

Jak jsme probírali v předchozí iteraci, můžete obvykle vytvořte jednotkové testy pro logikou přístupu dat ani zobrazit logiku. T zápis testů jednotek pro logikou přístupu dat zadávat, protože přístup k databázi se poměrně pomalá operace. Vzhledem k tomu vybudujete webového serveru, který je poměrně pomalá operace přístupu k zobrazení vyžaduje zadávat t zápis testů jednotek pro zobrazení logiku. Nesmí obsahovat více t napsat Jednotkový test, pokud test lze spustit znovu a znovu velmi rychle

Protože vývoj řízený testováním doprovází testování částí, zaměříme nejprve na kontroleru a obchodní logiky. Můžeme vyhnout, klepnou na databázi nebo zobrazení. Vyhráli jsme t upravit databázi nebo vytvořte naše zobrazení velmi konce tohoto kurzu. Začneme s co můžete otestovat.

## <a name="creating-user-stories"></a>Vytváření uživatelských scénářů

Při využívání vývoj řízený testováním, vždy začněte psaní testu. To okamžitě vydá dotaz: jak rozhodnout, jaká testovací zapsat nejprve? Na tuto otázku odpovědět, měli byste psát sadu [ **uživatelských scénářů**](http://en.wikipedia.org/wiki/User_stories).

Uživatelský scénář je velmi stručný popis (obvykle jedna věta) požadavky na software. Měla by být netechnické popis požadavek zapsán z pohledu uživatele.

Tady s sadu uživatelských scénářů, které popisují funkce vyžadované nové funkce skupina kontaktu:

1. Uživatel může zobrazit seznam skupin kontaktů.
2. Uživatel může vytvořit novou skupinu kontaktu.
3. Uživatel může odstranit existující skupiny kontaktů.
4. Uživatele můžete vybrat skupinu a kontaktů, při vytváření nového kontaktu.
5. Uživatele můžete vybrat skupinu a kontaktů, při úpravě existujícího kontaktu.
6. Seznam kontaktů skupiny se zobrazí v zobrazení indexu.
7. Po kliknutí kontaktujte skupinu, zobrazí se seznam odpovídajících kontakty.

Všimněte si, že tento seznam uživatelských scénářů je zcela srozumitelné ze strany zákazníka. Není k dispozici žádnou zmínku o podrobnosti o technické implementaci.

Během procesu vytváření vaší aplikace, se může stát přesnější sada uživatelských scénářů. Uživatelský scénář může rozdělit na několik scénářů (požadavky). Například můžete rozhodnout, že vytváří se nová skupina kontaktní by měl zahrnovat ověření. Odesílání kontaktní skupiny bez názvu by měla vrátit chybu ověřování.

Po vytvoření seznamu uživatelských scénářů, jste připraveni napsat první test částí. Začneme vytvořením testování částí pro zobrazení seznamu kontaktů skupin.

## <a name="listing-contact-groups"></a>Výpis skupin kontaktů

Naši první uživatelský scénář je, že uživatel by měl zobrazit seznam skupin kontaktů. Musíme express tento scénář s testem.

Vytvořte nový test jednotek kliknutím pravým tlačítkem složku řadiče v projektu ContactManager.Tests výběr **přidat, otestovat nové**a výběrem možnosti **testování částí** šablony (viz obrázek 1). Název nové jednotky testování GroupControllerTest.cs a klikněte na tlačítko **OK** tlačítko.


[![Přidání testování částí GroupControllerTest](iteration-6-use-test-driven-development-cs/_static/image1.jpg)](iteration-6-use-test-driven-development-cs/_static/image1.png)

**Obrázek 01**: Přidání testování částí GroupControllerTest ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-6-use-test-driven-development-cs/_static/image2.png))


Výpis 1 je součástí naší první test částí. Tento test ověřuje, že metoda Index() skupiny kontroleru vrátí sadu skupin. Test ověří, že kolekci skupin se vrátí v zobrazení data.

**Výpis 1 - Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample1.cs)]

Při prvním zadání kód v informacích 1 v sadě Visual Studio, získáte velké množství červená vlnovka řádky. Jsme nevytvořili GroupController nebo skupinu tříd.

V tuto chvíli můžeme dokonce sestavení t naší aplikace, takže můžeme t spustit naši první test částí. S tuto funkční. Který se počítá jako selhání testů. Proto teď máme oprávnění, aby vám začali psát kód aplikace. Musíme napsat dostatek kód ke spuštění našich testů.

Třída kontroleru skupiny v informacích 2 obsahuje úplné minimální kód potřebný k předání testu jednotek. Akce Index() vrátí staticky programové seznam skupin (skupina je třída definovaná v informacích 3).

**Výpis 2 - Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample2.cs)]

**Výpis 3 - Models\Group.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample3.cs)]

Poté, co jsme do našich projektu přidat třídy GroupController a skupiny, naši první test částí úspěšně dokončí (viz obrázek 2). Jsme udělali minimální práci potřebnou k projde testem. Je čas oslavili.


[![Úspěch!](iteration-6-use-test-driven-development-cs/_static/image2.jpg)](iteration-6-use-test-driven-development-cs/_static/image3.png)

**Obrázek 02**: Úspěch! () [Kliknutím ji zobrazíte obrázek v plné velikosti](iteration-6-use-test-driven-development-cs/_static/image4.png))


## <a name="creating-contact-groups"></a>Vytvoření skupiny kontaktů

Teď můžeme přesunout k druhý uživatelský scénář. Musíme být schopni vytvářet nové skupiny kontaktu. Potřebujeme vyjádřit svůj záměr s testem.

Test v informacích 4 ověřuje, že volání Create() metodu s novou skupinu přidá do seznamu skupin vrácený metodou Index() skupinu. Jinými slovy je-li vytvořit novou skupinu pak by měl možné vrátit novou skupinu ze seznamu skupin vrácený metodou Index().

**Část 4 – Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample4.cs)]

Test v informacích 4 volá řadič skupiny Create() metodu se vytvoří nový kontakt skupiny. V dalším kroku test ověří, volání řadič skupiny Index() metoda vrátí novou skupinu v zobrazení data.

Upravené skupiny řadič v informacích 5 obsahuje minimální změny nutné předat nový test.

**Výpis 5 - Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample5.cs)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a>Skupiny kontroleru v zobrazení 5 má nová akce Create(). Tato akce přidá skupinu na kolekci skupin. Všimněte si, že byla změněna Index() akce vrátí obsah kolekci skupin.

Znovu jsme provedli úplné minimální množství práce potřebné k předání testu jednotek. Jakmile provedeme tyto změny skupiny kontroleru, všechny naše jednotky testy průchodu.

## <a name="adding-validation"></a>Přidání ověřování

Tento požadavek nebyl explicitně nastavená v uživatelský scénář. Je však vhodné nutné, aby název skupiny. Uspořádání kontakty do skupin, v opačném případě by být velmi užitečné.

Výpis 6 obsahuje nový test, který vyjadřuje tohoto záměru. Tento test ověřuje, že při pokusu o vytvoření skupiny bez zadávání názvu výsledkem chybovou zprávu ověření do stavu modelu.

**Výpis 6 - Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample6.cs)]

Aby to vyhovovalo tento test, že je potřeba přidat vlastnost názvu třídy naší skupiny (viz seznam 7). Kromě toho potřebujeme přidat malý bit logiku ověřování k kontroleru skupiny s Create() akce (viz seznam 8).

**Výpis 7 - Models\Group.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample7.cs)]

**Výpis 8 - Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample8.cs)]

Všimněte si, že kontroler skupiny Create() akci nyní obsahuje logiku ověřování a databáze. V současné době databázi používaný řadičem skupina se skládá z nic jiného než kolekci v paměti.

## <a name="time-to-refactor"></a>Čas refaktorace

Třetí krok v červená, zelená/Refaktorujte je součástí Refaktorovat. V tomto okamžiku musíme kroku zpět z našeho kódu a zvažte, jak jsme Refaktorovat naši aplikaci ke zlepšení jejich návrhu. Refaktorujte fází je fáze, ve kterém by se pevný o nejlepší způsob implementace Principy návrhu softwaru a vzory.

Máme volné úpravy našeho kódu žádným způsobem, který jsme zvolte ke zlepšení návrh kódu. Máme bezpečnostní testů jednotek, které nám brání zásadní stávajících funkcí.

V tuto chvíli, skupiny kontroleru je hustá doprava z hlediska návrhu funkční software. Kontroler skupina obsahuje zamotaných nepořádku v ověřování a kód přístupu k datům. Pokud chcete vyhnout, bychom při tom porušili jedné zásadě odpovědnosti, potřebujeme tyto obavy rozdělit do různých tříd.

Naše refaktorovaný třída kontroleru rozhraní skupiny je součástí výpis 9. Použití ContactManager Vrstva služby byla změněna kontroleru. Toto je stejnou úroveň služby, který jsme použili u kontaktu kontroleru.

Výpis 10 obsahuje nové metody, které jsou přidány do ContactManager vrstvy služby pro podporu ověřování, zobrazení a vytváření skupin. Rozhraní IContactManagerService byl aktualizován zahrnout nové metody.

Výpis 11 obsahuje novou třídu FakeContactManagerRepository, která implementuje rozhraní IContactManagerRepository. Na rozdíl od EntityContactManagerRepository třídy, která také implementuje rozhraní IContactManagerRepository naši novou třídu FakeContactManagerRepository komunikaci s databází. Třída FakeContactManagerRepository používá kolekci v paměti jako proxy server pro databázi. Použijeme tuto třídu jako vrstvu falešné úložiště v testech jednotek.

**Výpis 9 - Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample9.cs)]

**Výpis 10 - Controllers\ContactManagerService.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample10.cs)]

**Výpis 11 – Controllers\FakeContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample11.cs)]

Úprava IContactManagerRepository vyžaduje rozhraní použijte pro implementaci metody CreateGroup() a ListGroups() ve třídě EntityContactManagerRepository. Laziest a nejrychlejší způsob, jak to provést, je přidání metody zástupných procedur, které vypadat nějak takto:   

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample12.cs)]


Nakonec se tyto změny v návrhu aplikace od nás vyžadovat provést určité změny pro naše testy jednotek. Nyní potřebujeme používat FakeContactManagerRepository při provádění testů jednotek. Aktualizované GroupControllerTest třídy jsou obsaženy v informacích 12.

**Výpis 12 - Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample13.cs)]

Po jejich Usnadňujeme změní, znovu, všechny naše testy jednotek prošly. Jsme dokončili celý cyklus červená, zelená/Refaktorovat. Implementovali jsme první dva uživatelské scénáře. Nyní je k dispozici podpora testování částí pro požadavky uživatelské scénáře. Implementace zbytek uživatelských scénářů zahrnuje opakující se stejný cyklus červená, zelená/Refaktorovat.

## <a name="modifying-our-database"></a>Změna databáze

Bohužel i když jsme splnili všechny požadavky vyjádřený v testech jednotek, není Hotovo naší práci. Stále potřebujeme upravit naší databázi.

Potřebujeme vytvořit novou tabulku databáze skupiny. Postupujte podle těchto kroků:

1. V okně Průzkumníka serveru, klikněte pravým tlačítkem na složku tabulky a vyberte možnost nabídky **přidat novou tabulku**.
2. Zadejte dva sloupce je popsáno níže v Návrháři tabulek.
3. Označte sloupec Id jako primární klíč a sloupec Identity.
4. Kliknutím na ikonu disketová uložte novou tabulku s názvem skupiny.

<a id="0.11_table01"></a>


| **Název sloupce** | **Datový typ** | **Povolit hodnoty Null** |
| --- | --- | --- |
| ID | int | False |
| Název | nvarchar(50) | False |


V dalším kroku budeme potřebovat odstranit všechna data z tabulky kontaktů (v opačném případě vyhráli jsme t moct vytvářet relace mezi tabulkami kontakty a skupiny). Postupujte podle těchto kroků:

1. Klikněte pravým tlačítkem na tabulku kontaktů a vyberte možnost nabídky **zobrazit Data tabulky**.
2. Odstraňte všechny řádky.

Dále musíme definovat vztah mezi skupiny databázové tabulky a stávající databáze tabulky kontaktů. Postupujte podle těchto kroků:

1. Klikněte dvakrát na tabulku kontaktů v okně Průzkumníka serveru pro otevření Návrháře tabulky.
2. Do tabulky kontaktů s názvem GroupId přidáte nový sloupec celé číslo.
3. Klikněte na tlačítko relace a otevřete dialogové okno vztahy cizího klíče (viz obrázek 3).
4. Klikněte na tlačítko Přidat.
5. Klikněte na tlačítko se třemi tečkami, který se zobrazí vedle tlačítka, tabulky a sloupce specifikace.
6. V dialogovém okně tabulky a sloupce vyberte skupiny jako tabulku primárního klíče a Id jako sloupec primárního klíče. Vyberte kontakty jako tabulka cizího klíče a ID skupiny jako sloupec cizího klíče (viz obrázek 4). Klikněte na tlačítko OK.
7. V části **INSERT a UPDATE specifikace**, vyberte hodnotu **Cascade** pro **odstranit pravidlo**.
8. Kliknutím na tlačítko Zavřít zavřete dialogové okno vztahy cizího klíče.
9. Kliknutím na tlačítko Uložit uložte změny do tabulky kontaktů.


[![Vytvoření relace tabulky databáze](iteration-6-use-test-driven-development-cs/_static/image3.jpg)](iteration-6-use-test-driven-development-cs/_static/image5.png)

**Obrázek 03**: vytvoření databázové tabulky relace ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-6-use-test-driven-development-cs/_static/image6.png))


[![Určení relací mezi tabulkami](iteration-6-use-test-driven-development-cs/_static/image4.jpg)](iteration-6-use-test-driven-development-cs/_static/image7.png)

**Obrázek 04**: Určení relací mezi tabulkami ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-6-use-test-driven-development-cs/_static/image8.png))


### <a name="updating-our-data-model"></a>Aktualizuje se naše datového modelu

Dále musíme aktualizovat naše datový model, který představuje nové databázové tabulky. Postupujte podle těchto kroků:

1. Poklikejte na soubor ContactManagerModel.edmx ve složce modely otevřete v návrháři entit.
2. Klikněte pravým tlačítkem na plochu návrháře a vyberte možnost nabídky **aktualizace modelů z databáze**.
3. V Průvodci aktualizacemi vyberte skupiny tabulky a po dokončení klikněte na tlačítko (viz obrázek 5).
4. Klikněte pravým tlačítkem na skupiny entit a vyberte možnost nabídky **přejmenovat**. Změňte název *skupiny* entitu, kterou chcete *skupiny* (singulární).
5. Klikněte pravým tlačítkem na skupiny navigační vlastnost, která se zobrazí v dolní části entitu kontakt. Změňte název *skupiny* navigační vlastnost pro *skupiny* (singulární).


[![Aktualizace modelu Entity Framework z databáze](iteration-6-use-test-driven-development-cs/_static/image5.jpg)](iteration-6-use-test-driven-development-cs/_static/image9.png)

**Obrázek 05**: aktualizace modelu Entity Framework z databáze ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-6-use-test-driven-development-cs/_static/image10.png))


Po dokončení těchto kroků bude reprezentovat datového modelu kontakty a skupiny tabulek. V návrháři entit by se měla zobrazit obě entity (viz obrázek 6).


[![Zobrazení skupiny a kontakt v návrháři entit](iteration-6-use-test-driven-development-cs/_static/image6.jpg)](iteration-6-use-test-driven-development-cs/_static/image11.png)

**Obrázek 06**: zobrazení skupiny a kontakt v návrháři entit ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-6-use-test-driven-development-cs/_static/image12.png))


### <a name="creating-our-repository-classes"></a>Vytváření naší třídy úložiště

Dále musíme implementovat třídu naše úložiště. V průběhu této iterace jsme přidali několik nových metod rozhraní IContactManagerRepository při psaní kódu ke splnění našich testů jednotek. Konečná verze rozhraní IContactManagerRepository je obsažen v informacích 14.

**Výpis 14 - Models\IContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample14.cs)]

Jsme haven t ve skutečnosti implementovat libovolnou metodu týkající se práce se skupinami kontaktu. V současné době EntityContactManagerRepository třída obsahuje metody zástupných procedur pro každou skupinu kontaktů metod uvedených v rozhraní IContactManagerRepository. Například metoda ListGroups() nyní vypadá takto:

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample15.cs)]

Povolené metody zástupných procedur abychom naši aplikaci zkompiluje a předat jednotkové testy. Ale nyní je čas při implementaci těchto metod. Konečná verze třídy EntityContactManagerRepository je obsažen v informacích 13.

**Výpis 13 – Models\EntityContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample16.cs)]

### <a name="creating-the-views"></a>Vytváření zobrazení

Aplikace ASP.NET MVC při použití výchozí modul zobrazení ASP.NET. Don t tedy vytvářet zobrazení v reakci na konkrétní Jednotkový test. Ale vzhledem k tomu, že aplikace může být užitečná, zobrazení, můžeme t dokončit tuto iteraci bez vytvoření a úprava zobrazení obsažená v aplikaci Správce kontaktů.

Potřebujeme vytvořit následující nová zobrazení pro správu kontaktů skupin (viz obrázek 7):

- Views\Group\Index.aspx – zobrazí seznam skupin kontaktů
- Views\Group\Delete.aspx – formulář zobrazí potvrzení k odstranění skupiny kontaktů


[![Zobrazení skupiny Index](iteration-6-use-test-driven-development-cs/_static/image7.jpg)](iteration-6-use-test-driven-development-cs/_static/image13.png)

**Obrázek 07**: skupinovým indexem zobrazení ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-6-use-test-driven-development-cs/_static/image14.png))


Potřebujeme upravit následující stávající zobrazení, aby zahrnovaly skupiny kontaktů:

- Views\Home\Create.aspx
- Views\Home\Edit.aspx
- Views\Home\Index.aspx

Zobrazí se změny zobrazení pohledem na aplikace sady Visual Studio, který doprovází tento kurz. Například obrázek 8 znázorňuje zobrazení indexu kontaktu.


[![Zobrazení indexu kontaktu](iteration-6-use-test-driven-development-cs/_static/image8.jpg)](iteration-6-use-test-driven-development-cs/_static/image15.png)

**Obrázek 08**: Zobrazení indexu kontakt ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-6-use-test-driven-development-cs/_static/image16.png))


## <a name="summary"></a>Souhrn

V této iterace jsme přidali nové funkce do naší aplikace Správce kontaktů podle návrhu metodologie vývoj řízený testováním aplikace. Začali jsme tím, že vytvoříte sadu uživatelských scénářů. Vytvořili jsme sadu testů jednotek, která odpovídá požadavky podle uživatelských scénářů. Nakonec jsme napsali jenom dostatek kódu ke splnění požadavků vyjádřena pomocí testů jednotek.

Poté, co nám podařilo dokončit zápis dostatek kódu ke splnění požadavků vyjádřena pomocí testů jednotek, aktualizovali jsme naše databáze a zobrazení. Jsme přidali novou tabulku skupiny do databáze a aktualizace datového modelu Entity Framework. Můžeme také vytvořit a upravit sadu zobrazení.

Při příští iteraci – poslední iterace – přepsat jsme naši aplikaci využívat technologie Ajax. S využitím jazyka Ajax, vám můžeme zlepšit rychlost reakce a výkon aplikace, kontaktujte správce.

> [!div class="step-by-step"]
> [Předchozí](iteration-5-create-unit-tests-cs.md)
> [další](iteration-7-add-ajax-functionality-cs.md)
