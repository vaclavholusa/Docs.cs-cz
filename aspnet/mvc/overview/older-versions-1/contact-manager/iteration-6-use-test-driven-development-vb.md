---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
title: "Iterace #6 – použijte testy řízený vývoj (VB) | Microsoft Docs"
author: microsoft
description: "V této šesté iteraci přidáme nové funkce pro naši aplikaci tak, že nejprve zápis testů částí a psaní kódu pro testování částí. V této iteraci..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: e1fd226f-3f8e-4575-a179-5c75b240333d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
msc.type: authoredcontent
ms.openlocfilehash: 9b558df9c0b44f5f76115270d361b6022658f9f9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="iteration-6--use-test-driven-development-vb"></a>Iterace #6 – použijte testy řízený vývoj (VB)
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhněte si kód](iteration-6-use-test-driven-development-vb/_static/contactmanager_6_vb1.zip)

> V této šesté iteraci přidáme nové funkce pro naši aplikaci tak, že nejprve zápis testů částí a psaní kódu pro testování částí. V této iteraci přidáme kontaktní skupiny.


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

V předchozí iteraci aplikace, obraťte se na správce jsme vytvořili testování částí zajistit o bezpečnostní opatření pro naše kódu. Motivace pro testování částí se aby naše kód odolnější vůči změnit. Pomocí jednotkových testů na místě jsme brouka provést změnu kódu a okamžitě vědět, zda jsme zrušili existující funkce.

V této iteraci používáme testování částí pro úplně jinou účel. V této iteraci používáme testování částí v rámci filosofie návrhu aplikace volá *vývoje řízeného testováním*. Můžete provádět testy řízený vývoj, nejprve napsat testy a zapište kód pro testy.

Přesněji řečeno, když cvičení vývoje řízeného testováním, existují tři kroky, které můžete provést při vytváření kódu (červená / Refaktorovat nebo zelená):

1. Zápis testů jednotek, které selžou (červený)
2. Napsat kód, který předává testování částí (zelený)
3. Refaktorovat kódu (Refaktorovat)

Nejprve zápisu testování částí. Testování částí mají vyjádřit svůj záměr pro jak očekáváte, že váš kód se bude chovat. Při prvním vytvoření testu jednotek, má selhat testování částí. Test by měl nezdaří, protože ještě nebyly zapsány žádné aplikační kód, který splňuje test.

V dalším kroku v pořadí pro testování částí předat napsat přesně akorát kód. Cílem je napsat kód laziest, sloppiest a nejrychlejších možné způsobem. Neměli odpady čas přemýšlení o architektuře vaší aplikace. Místo toho měli byste se zaměřit na zápis minimální množství kódu, které jsou nezbytné pro splnění záměr vyjádřená testování částí.

Nakonec po jste napsali dostatek kódu, můžete krok zpět a zvažte přehled architektury aplikace. V tomto kroku přepisování (refactor) kódu využitím návrh softwaru vzory – například použitému vzoru – tak, aby váš kód více udržovatelný. Kód v tomto kroku můžete přepsat pomocí fearlessly vzhledem k tomu, že váš kód je předmětem testování částí.

Existuje mnoho výhod, které jsou výsledkem cvičení testy řízený vývoj. První, testy řízený vývoj vynutí se můžete soustředit na kód, který je ve skutečnosti potřebuje k zapsání. Vzhledem k tomu, že jsou neustále zaměřuje na právě zápis dostatek kódu předat konkrétní test, nebudou moci wandering do plevelem a zápis masivní objemy kód, který se nikdy nepoužívejte.

Druhý návrh metodika "nejdřív otestovat" vynutí psaní kódu z perspektivy použití vašeho kódu. Jinými slovy při cvičení vývoje řízeného testováním, neustále programujete testy z hlediska uživatelů. Proto může způsobit vývoje řízeného testováním čisticí a více nerozumí rozhraní API.

Nakonec vývoje řízeného testováním vynutí zápis testů částí jako součást normální proces vytváření aplikace. Jak se blíží konečný termín projektu, testování je obvykle první věc, kterou nedostane mimo okno. Při cvičení vývoje řízeného testováním na druhé straně, se pravděpodobně být virtuous o zápis testů částí, protože vývoje řízeného testováním provede testování částí centrální proces vytváření aplikace.

> [!NOTE] 
> 
> Další informace o vývoje řízeného testováním, I doporučujeme, abyste si přečetli Michael peří kniha **efektivní práce s kódem starší**.


V této iteraci přidáme novou funkci pro naši aplikaci obraťte se na správce. Nemůžeme přidat podporu pro skupiny kontaktu. Můžete vytvořit, obraťte se na skupiny a uspořádat do kategorií, jako je například obchodní kontakty a skupiny Friend.

Tato nová funkce přidáme do aplikace pomocí následujících proces vývoje řízeného testováním. Jsme budete nejprve napsat naše testy jednotek a jsme budete psát všechny naše kód pro tyto testy.

## <a name="what-gets-tested"></a>Získá testů, které

Jak již bylo zmíněno v předchozí iteraci, obvykle nemáte zápisu testování částí pro logiku přístup dat nebo zobrazit logiku. Testování částí t zápisu pro logiku přístup dat neuděláte, protože přístup k databázi je relativně pomalé operace. Testování částí t zápisu pro zobrazení logiku neuděláte, protože přístup k zobrazení vyžaduje roztočený až webového serveru, který je relativně pomalé operace. By neměl t zápis testů jednotek, pokud test mohou být provedeny opakovaně velmi rychle

Protože vývoje řízeného testováním vycházejí z testů jednotek, zaměříme původně na zápis řadiče a obchodní logiku. Budeme-li se vyhnout zásahu do databáze nebo zobrazení. Jsme won t změnit databázi nebo vytvořte naše zobrazení až velmi konce tohoto kurzu. Začneme s co může být testována.

## <a name="creating-user-stories"></a>Vytvoření uživatelských scénářů

Při cvičení vývoje řízeného testováním, je vždy nejprve zápis testu. To okamžitě vyvolá Otázka: jak můžete rozhodnout, jaké testovací zápis nejprve? Na tuto otázku odpovědět, je nutné zapsat sadu [ *uživatelských scénářů*](http://en.wikipedia.org/wiki/User_stories).

Uživatelský scénář je velmi stručný popis (obvykle jedna věta) požadavek na software. Mělo by být netechnické popis požadavek zapsán z hlediska uživatelů.

Tady s sadu uživatelských scénářů, které popisují funkce vyžadované službou nové funkce kontaktujte skupiny:

1. Uživatele můžete zobrazit seznam skupin kontaktů.
2. Uživatele můžete vytvořit novou skupinu kontaktu.
3. Uživatel může odstranit existující kontaktní skupiny.
4. Uživatele můžete vybrat skupinu kontaktů, při vytváření nového kontaktu.
5. Uživatele můžete vybrat skupinu kontaktů, při úpravě existující kontakt.
6. Seznam skupin kontaktů se zobrazí v zobrazení indexu.
7. Po kliknutí kontaktujte skupiny, zobrazí se seznam odpovídající kontakty.

Všimněte si, že tento seznam uživatelských scénářů je zcela nerozumí zákazníka. Neexistuje žádné zmínky technickou implementaci podrobnosti.

Při procesu vytváření aplikace, se může stát přesnější sadu uživatelských scénářů. Uživatelský scénář může rozdělit na několik scénářů (požadavky). Například můžete rozhodnout, že vytvoření nové skupiny kontaktní by měla zahrnovat ověření. Odesílání kontaktní skupiny bez názvu by měla vrátit chybu ověření.

Po vytvoření seznam uživatelských scénářů, jste připraveni k zápisu svůj první test jednotky. Začneme vytvořením testování částí pro zobrazení seznamu kontaktů skupin.

## <a name="listing-contact-groups"></a>Seznam skupin kontaktů

Naše první uživatelský scénář je, že uživatel by měl být zobrazit seznam skupin kontaktů. Musíme express tento scénář s testu.

Vytvořit nový test jednotky kliknutím pravým tlačítkem na složku řadiče v projektu ContactManager.Tests výběr **přidat, nové testovací**a výběr **jednotkové testování** šablony (viz obrázek 1). Název nové jednotky testování GroupControllerTest.vb a klikněte na **OK** tlačítko.


[![Přidání testů jednotek GroupControllerTest](iteration-6-use-test-driven-development-vb/_static/image1.jpg)](iteration-6-use-test-driven-development-vb/_static/image1.png)

**Obrázek 01**: Přidání testů jednotek GroupControllerTest ([Kliknutím zobrazit obrázek v plné velikosti](iteration-6-use-test-driven-development-vb/_static/image2.png))


Naše první testování částí je obsažený v výpis 1. Tento test ověřuje, že metoda Index() řadiče skupiny vrátí sadu skupin. Test ověřuje, že kolekce skupin se vrátí v zobrazení data.

**Výpis 1 - Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample1.vb)]

Když zadáte nejprve kód v výpis 1 v sadě Visual Studio, získáte spoustu červenými vlnovkami. Nevytvořili jsme třídy GroupController nebo skupinu.

V tomto okamžiku můžeme i sestavení t naše aplikace, můžeme t spouštění naše první testování částí. Tento s funkčním. Který počítá jako selhání testu. Proto máme teď oprávnění, aby vám začali psát kód aplikace. Musíme napsat dostatek kód pro spuštění našem testu.

Třídy kontroleru skupiny v výpis 2 obsahuje minimální požadované předat testování částí kódu. Akce Index() vrátí seznam staticky programové skupiny (třídy skupiny je definována v výpis 3).

**Výpis 2 - Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample2.vb)]

**Výpis 3 - Models\Group.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample3.vb)]

Po přidáme třídy GroupController a skupiny do našich projektu, naše první testování částí úspěšně dokončen (viz obrázek 2). Provedli jsme minimální práce potřebné k projde testem. Je třeba sváteční.


[![Úspěšné!](iteration-6-use-test-driven-development-vb/_static/image2.jpg)](iteration-6-use-test-driven-development-vb/_static/image3.png)

**Obrázek 02**: úspěšné! () [Kliknutím zobrazit obrázek v plné velikosti](iteration-6-use-test-driven-development-vb/_static/image4.png))


## <a name="creating-contact-groups"></a>Vytváření skupin kontaktů

Nyní se můžeme přesunout k druhý uživatelský scénář. Je potřeba k vytvoření nové skupiny kontaktů. Musíme express svůj záměr s testu.

Test v výpis 4 ověřuje, že volání Create() metoda s novou skupinu přidá do seznamu skupin vráceném metodou Index() skupině. Jinými slovy je-li vytvořit novou skupinu pak bych moct vrátit do nové skupiny ze seznamu skupin vráceném metodou Index().

**Výpis 4 - Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample4.vb)]

Test v výpis 4 volá řadičem skupiny Create() metoda s nový kontakt skupiny. V dalším kroku test ověřuje, volání řadičem skupiny Index() metoda vrátí do nové skupiny v dat zobrazení.

Změny skupiny řadiče v výpis 5 obsahuje minimální změn, které vyžadují předat nový test.

**Výpis 5 - Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample5.vb)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a>Skupiny řadiče v výpis 5 má novou akci Create(). Tato akce přidá skupinu na kolekci skupin. Všimněte si, že byl upraven Index() akce vrátí obsah kolekce skupin.

Ještě jednou jsme provedli minimální množství práce potřebné k předání testování částí. Po jsme tyto změny provést řadičem skupiny, všechny naše jednotky testy průchodu.

## <a name="adding-validation"></a>Přidání ověřování

Tento požadavek nebyl explicitně uvádí uživatelský scénář. Nicméně je možné logicky nutné, aby skupina název. Uspořádání kontakty do skupiny, jinak by být velmi užitečné.

Výpis 6 obsahuje nový test, který vyjadřuje svůj záměr. Tento test ověřuje, že při pokusu o vytvoření skupiny bez nutnosti zadávat název výsledky v chybovou zprávu ověření ve stavu modelu.

**Výpis 6 - Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample6.vb)]

Účelem uspokojení tento test, je potřeba přidat název vlastnosti do třídy Naše skupiny (viz seznam 7). Kromě toho je potřeba přidat jen nepatrnou kousek logiku ověření do kontroleru skupiny s Create() akce (viz seznam 8).

**Výpis 7 – Models\Group.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample7.vb)]

**Výpis 8 - Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample8.vb)]

Všimněte si, že řadičem skupiny Create() akci nyní obsahuje logiku ověřování i databáze. V současné době databázi použitou adaptérem skupiny se skládá z nic jiného než kolekci v paměti.

## <a name="time-to-refactor"></a>Čas Refaktorovat

Třetí krok v červená, zelená/Refaktorovat je součástí Refaktorovat. V tomto okamžiku musíme krok zpět z našich kódu a zvážit, jak jsme Refaktorovat naše aplikace ke zlepšení návrh. Fáze Refaktorovat je fázi, kdy myslíme pevný o nejlepší způsob implementace Principy návrhu softwaru a vzory.

Snažíme se volné změnit naše kód žádným způsobem, který jsme vybrat pro zlepšení návrh kódu. Máme bezpečnostní opatření testů jednotek, které nám zabránit v zastavení stávající funkce.

Teď, skupiny kontroleru je nepořádek z hlediska návrhu dobrý softwaru. Řadičem skupiny obsahuje zamotaných nepořádek ověření a data přístupový kód. Aby se zabránilo narušení jeden zásady odpovědnosti, je potřeba oddělit tyto problémy do různých tříd.

Naše refactored třídy kontroleru skupina je obsažena v výpis 9. K použití vrstvy služby ContactManager se změnilo kontroleru. Toto je stejné vrstvě služby využívající obraťte se na řadiči.

Výpis 10 obsahuje nové metody přidat do vrstvy služby ContactManager pro podporu ověřování, výpis a vytvoření skupin. Rozhraní IContactManagerService se aktualizovalo se o nové metody.

Výpis 11 obsahuje novou třídu FakeContactManagerRepository, která implementuje rozhraní IContactManagerRepository. Na rozdíl od EntityContactManagerRepository třídu, která také implementuje rozhraní IContactManagerRepository naše nová třída FakeContactManagerRepository nekomunikuje s databází. Třída FakeContactManagerRepository používá kolekci v paměti jako proxy server pro databázi. Tato třída použijeme v testech jednotek jako vrstva falešných úložiště.

**Výpis 9 – Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample9.vb)]

**Výpis 10 - Controllers\ContactManagerService.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample10.vb)]

**Výpis 11 – Controllers\FakeContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample11.vb)]


Úprava IContactManagerRepository vyžaduje rozhraní použít k implementaci CreateGroup() a ListGroups() metody ve třídě EntityContactManagerRepository. Laziest a nejrychlejší způsob, jak to udělat, je přidání se zakázaným inzerováním metody, které vypadat takto:

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample12.vb)]


Nakonec tyto změny v návrhu aplikace vyžadovat, aby určité změny pro naše testy jednotek. Nyní potřebujeme používat FakeContactManagerRepository při provádění testování částí. Aktualizované třídy GroupControllerTest je součástí výpis 12.

**Výpis 12 – Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample13.vb)]

Po zajišťujeme, že všechny tyto změny, znovu všechny naše testy průchodu jednotky. Celý cyklus systému Red, zelená/Refaktorovat jsme byly dokončeny. Implementovali jsme první dva uživatelských scénářů. Teď máte podpoře testování částí pro požadavky vyjádřené v uživatelských scénářů. Implementace zbytek uživatelských scénářů zahrnuje opakují na stejné cyklus červená, zelená/Refaktorovat.

## <a name="modifying-our-database"></a>Úprava databáze

Bohužel to i v případě, že jsme splnili všechny požadavky vyjádřená naše testy jednotek, není potřeba našich pracovních. Stále potřebujeme upravit naše databáze.

Je potřeba vytvořit novou tabulku databáze skupiny. Postupujte podle těchto kroků:

1. V okně Průzkumníka serveru klikněte pravým tlačítkem na složku tabulky a vyberte možnost nabídky **přidat novou tabulku**.
2. Zadejte dva sloupce, které jsou popsané dál v Návrháři tabulky.
3. Označte sloupec Id jako primární klíč a sloupec Identity.
4. Uložte novou tabulku s názvem skupiny kliknutím na ikonu disketová.

<a id="0.12_table01"></a>


| **Název sloupce** | **Datový typ** | **Povolit hodnoty Null** |
| --- | --- | --- |
| ID | int | False |
| Název | Nvarchar(50) | False |


Dále je potřeba odstranit všechna data z tabulky kontaktů (jinak, jsme won t možné vytvořit relaci mezi tabulkami kontakty a skupiny). Postupujte podle těchto kroků:

1. Klikněte pravým tlačítkem na tabulku kontaktů a vyberte možnost nabídky **zobrazit Data tabulky**.
2. Odstraňte všechny řádky.

Dále je potřeba definovat relaci mezi tabulkou databáze skupiny a stávající databázové tabulky kontaktů. Postupujte podle těchto kroků:

1. Klikněte dvakrát na tabulku kontaktů v okně Průzkumníka serveru otevřete návrháře tabulky.
2. Do tabulky kontaktů s názvem GroupId umožňuje přidáte nový sloupec celé číslo.
3. Kliknutím na tlačítko vztah otevřete dialogové okno relace cizího klíče (viz obrázek 3).
4. Kliknutím na tlačítko Přidat.
5. Klikněte na tlačítko se třemi tečkami, který se zobrazí vedle tlačítko tabulky a sloupce specifikace.
6. V dialogovém okně tabulky a sloupce, vyberte skupiny jako primární klíč tabulky a Id jako sloupec primárního klíče. Vyberte kontakty jako tabulka cizího klíče a GroupId jako sloupec cizího klíče (viz obrázek 4). Klikněte na tlačítko OK.
7. V části **INSERT a UPDATE specifikace**, vyberte hodnotu **Cascade** pro **odstranit pravidlo**.
8. Klikněte na tlačítko Zavřít zavřete dialogové okno relace cizího klíče.
9. Klikněte na tlačítko Uložit a uložte změny do tabulky kontaktů.


[![Vytvoření relace tabulky databáze](iteration-6-use-test-driven-development-vb/_static/image3.jpg)](iteration-6-use-test-driven-development-vb/_static/image5.png)

**Obrázek 03**: vytvoření databázové tabulky relace ([Kliknutím zobrazit obrázek v plné velikosti](iteration-6-use-test-driven-development-vb/_static/image6.png))


[![Určení relace mezi tabulkami](iteration-6-use-test-driven-development-vb/_static/image4.jpg)](iteration-6-use-test-driven-development-vb/_static/image7.png)

**Obrázek 04**: určení relace mezi tabulkami ([Kliknutím zobrazit obrázek v plné velikosti](iteration-6-use-test-driven-development-vb/_static/image8.png))


### <a name="updating-our-data-model"></a>Aktualizace našeho datového modelu

Dále je potřeba aktualizovat našeho datového modelu představují nové databázové tabulky. Postupujte podle těchto kroků:

1. Poklikejte na soubor ContactManagerModel.edmx ve složce modely otevřete Entity Designer.
2. Klikněte pravým tlačítkem na plochu návrháře a vyberte možnost nabídky **aktualizovat Model z databáze**.
3. V Průvodci aktualizací vyberte skupiny, tabulky a klikněte dokončit tlačítko (viz obrázek 5).
4. Klikněte pravým tlačítkem na entity skupiny a vyberte možnost nabídky **přejmenovat**. Změňte název *skupiny* entity, která má *skupiny* (singulární).
5. Klikněte pravým tlačítkem na navigační vlastnost skupin, které se zobrazuje v dolní části entity Kontakt. Změňte název *skupiny* navigační vlastnost pro *skupiny* (singulární).


[![Aktualizace model Entity Framework z databáze](iteration-6-use-test-driven-development-vb/_static/image5.jpg)](iteration-6-use-test-driven-development-vb/_static/image9.png)

**Obrázek 05**: aktualizace model Entity Framework z databáze ([Kliknutím zobrazit obrázek v plné velikosti](iteration-6-use-test-driven-development-vb/_static/image10.png))


Po dokončení těchto kroků bude reprezentovat datový model kontakty a skupiny tabulek. Entity Designer by měl zobrazit obě entity (viz obrázek 6).


[![Entity Designer zobrazení skupiny a obraťte se na](iteration-6-use-test-driven-development-vb/_static/image6.jpg)](iteration-6-use-test-driven-development-vb/_static/image11.png)

**Obrázek 06**: zobrazení skupiny a obraťte se na Entity Designer ([Kliknutím zobrazit obrázek v plné velikosti](iteration-6-use-test-driven-development-vb/_static/image12.png))


### <a name="creating-our-repository-classes"></a>Vytváření tříd naše úložiště

V dalším kroku musíme implementovat třídu naše úložiště. V průběhu této iterace jsme přidali několik nových metod rozhraní IContactManagerRepository při psaní kódu pro uspokojení naše testy jednotek. Finální verzi rozhraní IContactManagerRepository je obsažený v výpis 14.

**Výpis 14 - Models\IContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample14.vb)]

Jsme některou z metod týkající se práce s skupiny kontaktů v našem skutečné třídě EntityContactManagerRepository nebyla některá t ve skutečnosti implementována. V současné době třída EntityContactManagerRepository má se zakázaným inzerováním metody pro každou skupinu kontaktů metody uvedené v IContactManagerRepository rozhraní. Například metoda ListGroups() aktuálně vypadá takto:

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample15.vb)]

Metody se zakázaným inzerováním povoleno nám zkompilovat naší aplikaci a předejte testování částí. Ale nyní je čas ve skutečnosti implementovat tyto metody. Finální verzi třída EntityContactManagerRepository je součástí výpis 13.

**Výpis 13 – Models\EntityContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample16.vb)]

### <a name="creating-the-views"></a>Vytvoření zobrazení

Aplikace ASP.NET MVC při použití výchozí modul zobrazení ASP.NET. Ano tento t vytvářet zobrazení v odpovědi na test konkrétní jednotka. Ale vzhledem k aplikaci by být nelze bez zobrazení, můžeme t dokončit tento iterace bez vytvoření a úprava zobrazení obsažené v aplikaci obraťte se na správce.

Je potřeba vytvořit následující nové zobrazení pro správu skupin kontaktů (viz obrázek 7):

- Views\Group\Index.aspx - zobrazí seznam skupin kontaktů
- Views\Group\Delete.aspx - zobrazí potvrzení formuláře pro odstranění skupiny kontaktů


[![Zobrazení skupiny indexu](iteration-6-use-test-driven-development-vb/_static/image7.jpg)](iteration-6-use-test-driven-development-vb/_static/image13.png)

**Obrázek 07**: Zobrazení indexu skupiny ([Kliknutím zobrazit obrázek v plné velikosti](iteration-6-use-test-driven-development-vb/_static/image14.png))


Je potřeba změnit následující stávající zobrazení, aby zahrnovaly kontaktní skupiny:

- Views\Home\Create.aspx
- Views\Home\Edit.aspx
- Views\Home\Index.aspx

Zobrazí se změny zobrazení prohlížením aplikace Visual Studio, který doprovází v tomto kurzu. Například obrázek 8 znázorňuje zobrazení obraťte se na Index.


[![Obraťte se na Index zobrazení](iteration-6-use-test-driven-development-vb/_static/image8.jpg)](iteration-6-use-test-driven-development-vb/_static/image15.png)

**Obrázek 08**: zobrazení v obraťte se na Index ([Kliknutím zobrazit obrázek v plné velikosti](iteration-6-use-test-driven-development-vb/_static/image16.png))


## <a name="summary"></a>Souhrn

V této iteraci jsme přidali nové funkce pro naše obraťte se na správce aplikace podle návrhu metodika vývoje řízeného testováním aplikace. Spuštění tak, že vytvoříte sadu uživatelských scénářů. Jsme vytvořili sada testů jednotek, které odpovídají požadavkům vyjádřená uživatelských scénářů. Nakonec napsali jsme právě dostatek kód vyhovět jejich požadavkům vyjádřená testování částí.

Po jsme dokončení zápisu dostatek kódu vyhovět jejich požadavkům vyjádřená testování částí aktualizovali jsme naši databáze a zobrazení. Jsme přidali novou tabulku skupiny do databáze hotový a aktualizovat datového modelu Entity Framework. Můžeme také vytvořit a upravit sadu zobrazení.

V další iterace – poslední iterace – přepisování jsme naši aplikaci chcete využít výhod Ajax. Využitím Ajax, jsme budete zvýšit rychlost reakce a výkonu aplikace, obraťte se na správce.

>[!div class="step-by-step"]
[Předchozí](iteration-5-create-unit-tests-vb.md)
[další](iteration-7-add-ajax-functionality-vb.md)
