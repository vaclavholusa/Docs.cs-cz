---
uid: mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
title: 'Iterace #5 – testů jednotek vytvořit (VB) | Microsoft Docs'
author: microsoft
description: V páté iteraci jsme snadněji naše aplikace spravovat a upravovat přidáním testování částí. Jsme model třídy modelu našich dat a vytvářet testy částí pro o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: c6e5c036-2265-4fa7-a9eb-47f197bdc262
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
msc.type: authoredcontent
ms.openlocfilehash: fe59792a1e1a7950a318e7e893b3da12d53a8efa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="iteration-5--create-unit-tests-vb"></a>Iterace #5 – vytvoření testování částí (VB)
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhněte si kód](iteration-5-create-unit-tests-vb/_static/contactmanager_5_vb1.zip)

> V páté iteraci jsme snadněji naše aplikace spravovat a upravovat přidáním testování částí. Jsme model třídy modelu našich dat a vytvářet testy částí pro naše řadiče a logiku ověření.


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

V předchozí iteraci aplikace, obraťte se na správce jsme rozdělili aplikaci se víc volně vázány. Jsme oddělené aplikaci do různých řadič, služby a vrstvy úložiště. Jednotlivé úrovně komunikuje s vrstvy pod ním prostřednictvím rozhraní.

Jsme rozdělili aplikace, abyste mohli snadněji spravovat a upravovat aplikace. Například pokud je potřeba použít novou technologií přístupu data, můžete nám jednoduše změnit vrstvy úložiště bez zásahu do řadiče nebo vrstva služby. Tím, že správce kontaktujte volně vázány, jsme sunout provedené aplikace odolnější vůči změnit.

Ale co se stane, když je potřeba přidat novou funkci obraťte se na správce aplikace? Nebo, co se stane, když jsme oprava chyby? Sad, ale i Principy pravdivosti psaní kódu je, že vždy, když touch kódu vytvoříte nebezpečí nové chyby.

Například jemné jeden den, Správce může zobrazit dotaz můžete přidat nové funkce, obraťte se na správce. Jana chce můžete přidat podporu pro skupiny kontaktu. Jana chce vám umožňuje uživatelům uspořádání jejich kontakty do skupin jako je například přátel, obchodních a tak dále.

Chcete-li implementovat tuto novou funkci, budete muset upravit všechny tři vrstvy aplikace, obraťte se na správce. Budete muset přidat další nové funkce v řadičích, vrstvě služby a úložiště. Jakmile začnete měnit kód, riskujete přerušení funkce, který odpracoval před.

Refaktoring naše aplikace na samostatné vrstvy, jako jsme to udělali v předchozí iteraci byl dobré. Bylo dobré, protože umožňuje nám dělat změny na celý vrstvy bez zásahu zbývající aplikace. Ale pokud budete chtít snadněji spravovat a upravovat kód v rámci vrstvy, budete muset vytvářet testy částí pro kód.

Můžete použít jinou jednotku testu jednotlivá jednotka kódu. Tyto jednotky kódu jsou menší než vrstvy celou aplikaci. Testování částí se obvykle používají k ověření, jestli konkrétní metoda ve vašem kódu chová očekávaným způsobem. Například by vytvořit testů jednotek pro metodu CreateContact() vystavené ContactManagerService třídy.

Testování částí pro pracovní aplikace, která je jenom jako bezpečnostní opatření. Vždy, když upravíte kódu v aplikaci, můžete spustit sadu testů jednotek pro zkontrolujte, zda úpravy dělí stávající funkce. Testování částí byl váš kód bezpečné upravit. Testování částí zajistěte všechny kódu v aplikaci odolnější vůči změnit.

V této iteraci přidáme testování částí pro naši aplikaci obraťte se na správce. Tímto způsobem v další iterace jsme můžete přidat skupiny obraťte se na naše aplikace bez obav o pozastavení stávající funkce.

> [!NOTE] 
> 
> Existují různé platformy včetně NUnit, xUnit.net a MbUnit testování částí. V tomto kurzu používáme jednotky testování framework zahrnutá v sadě Visual Studio. Však můžete stejným způsobem použít jednu z těchto alternativní architektury.


## <a name="what-gets-tested"></a>Získá testů, které

V ideálním veškerý kód by být pokryté komponentami testování částí. V ideálním můžete být ideální bezpečnostní opatření. Bude moci upravit každý řádek kódu v aplikaci a okamžitě vědět, spuštěním testů jednotek, zda změna překročila stávající funkce.

Ale jsme nejsou zobrazeny t za provozu v ideálním. V praxi, když zápis testů částí které soustředit se jenom na zápis testů pro obchodní logiky (například logiku ověření). Konkrétně můžete *nepodporují* zápis testů částí data přístup logiky nebo logika zobrazení.

Chcete-li být užitečné, třeba testy jednotek spustit velmi rychle. Snadno můžete shromažďujete stovky (nebo dokonce tisíce) testování částí pro aplikaci. Pokud testy jednotek trvat dlouhou dobu spuštění budete vyhnout, jejich spuštění. Jinými slovy dlouhotrvající testů jednotek jsou nepoužitelná pro účely kódování den.

Z tohoto důvodu je obvykle Nezapisovat testy částí pro kód, který komunikuje s databází. Spuštěným stovky testování částí pro online databáze může být příliš pomalé. Místo toho model databáze a napsat kód, který komunikuje s databází imitované (probereme, že mocking databázi níže).

Podobné z jiného důvodu je obvykle Nezapisovat testování částí pro zobrazení. Chcete-li otestovat zobrazení, musí začne pracovat webovém serveru. Protože roztočený webový server je relativně pomalé proces, není doporučeno vytvářet testy částí pro zobrazení.

Pokud zobrazení obsahuje složitý logiku měli zvážit, přesun logiku do pomocné metody. Můžete napsat testy částí pro pomocné metody, které se provedou bez roztočený webový server.

> [!NOTE] 
> 
> Při psaní testů pro logiku data přístup, nebo zobrazení logiku není vhodné při zápis testů částí při sestavování funkční nebo integrace testů může být velmi cenné tyto testy.


> [!NOTE] 
> 
> ASP.NET MVC je modul zobrazení webových formulářů. Zatímco modul Web Forms zobrazení je závislé na webovém serveru, nemusí být další moduly zobrazení.


## <a name="using-a-mock-object-framework"></a>Pomocí rozhraní Mock objektu

Při vytváření testů jednotek, musíte téměř vždy využít architekturu model objektu. Model objektu rozhraní umožňuje vytvářet mocks a zástupných procedur pro třídy v aplikaci.

Například můžete použít model objektu rozhraní k vygenerování verzi mock vaší třídy úložiště. Tímto způsobem třídu imitované úložiště můžete použít místo třídu skutečné úložiště v testů jednotek. Pomocí mock úložiště umožňuje vyhnout se provádění kódu databáze při provádění testů jednotek.

Visual Studio nezahrnuje framework model objektu. Existuje však několik architektury model objektu komerční s otevřeným zdrojem k dispozici pro rozhraní .NET framework:

1. Moq – toto rozhraní je k dispozici v rámci licence BSD s otevřeným zdrojem. Můžete si stáhnout Moq z [ https://code.google.com/p/moq/ ](https://code.google.com/p/moq/).
2. Rhino Mocks – toto rozhraní je k dispozici v části s otevřeným zdrojem BSD licencí. Můžete si stáhnout Rhino Mocks z [ http://ayende.com/projects/rhino-mocks.aspx ](http://ayende.com/projects/rhino-mocks.aspx).
3. Typemock Odpojovač - Toto je komerční framework. Můžete si stáhnout zkušební verzi z [ http://www.typemock.com/ ](http://www.typemock.com/).

V tomto kurzu rozhodli používat Moq. Ale můžete stejným způsobem použít Rhino Mocks nebo Typemock Odpojovač vytvořit model objektů aplikace obraťte se na správce.

Než budete moct použít Moq, je třeba provést následující kroky:

1. .
2. Předtím, než jste rozbalte stahování, ujistěte se, klikněte pravým tlačítkem na soubor a klikněte na tlačítko s názvem bez přípony **Odblokovat** (viz obrázek 1).
3. Rozbalte stahování.
4. Přidat odkaz na sestavení Moq do projektu testovací výběrem možnosti nabídky **projektu, přidat odkaz na** otevřete **přidat odkaz na** dialogové okno. Na kartě Procházet přejděte do složky, kde jste rozbalené Moq a vyberte Moq.dll sestavení. Klikněte **OK** tlačítko (viz obrázek 2).


[![Odblokování Moq](iteration-5-create-unit-tests-vb/_static/image1.jpg)](iteration-5-create-unit-tests-vb/_static/image1.png)

**Obrázek 01**: odblokování Moq ([Kliknutím zobrazit obrázek v plné velikosti](iteration-5-create-unit-tests-vb/_static/image2.png))


[![Po přidání Moq odkazy](iteration-5-create-unit-tests-vb/_static/image2.jpg)](iteration-5-create-unit-tests-vb/_static/image3.png)

**Obrázek 02**: odkazy po přidání Moq ([Kliknutím zobrazit obrázek v plné velikosti](iteration-5-create-unit-tests-vb/_static/image4.png))


## <a name="creating-unit-tests-for-the-service-layer"></a>Testování částí pro vrstvu služby

Umožní s začněte vytvořením sadu testů jednotek pro vrstvu služby s aplikací naše obraťte se na správce. Chcete-li ověřit ověřovací logiku použijeme tyto testy.

Vytvořte novou složku s názvem modely v ContactManager.Tests projektu. V dalším kroku klikněte pravým tlačítkem na složku modely a vyberte **přidat, nový Test**. **Přidat nový Test** otevře se dialogové okno znázorněný na obrázku 3. Vyberte **jednotkové testování** šablony a pojmenujte nový test ContactManagerServiceTest.vb. Klikněte **OK** tlačítko přidáte nový test pro projekt Test.

> [!NOTE] 
> 
> Obecně platí budete chtít struktura složek testovacího projektu tak, aby odpovídaly struktura složek projektu ASP.NET MVC. Například řadič testy umístit do složky řadiče, model testů ve složce modely a tak dále.


[![Models\ContactManagerServiceTest.cs](iteration-5-create-unit-tests-vb/_static/image3.jpg)](iteration-5-create-unit-tests-vb/_static/image5.png)

**Obrázek 03**: Models\ContactManagerServiceTest.cs ([Kliknutím zobrazit obrázek v plné velikosti](iteration-5-create-unit-tests-vb/_static/image6.png))


Na začátku chceme test CreateContact() metody vystavené ContactManagerService třídy. Vytvoříme následujících pět testů:

- CreateContact() - testy této CreateContact() vrátí hodnotu true, pokud je platný kontakt předaný metodě.
- CreateContactRequiredFirstName() - testy, zda je přidána chybová zpráva do stavu modelu, když kontakt s chybějící křestní jméno je předaný metodě CreateContact().
- CreateContactRequredLastName() - testy, zda je přidána chybová zpráva do stavu modelu, když kontakt s chybějící příjmení je předaný metodě CreateContact().
- CreateContactInvalidPhone() - testy, zda je přidána chybová zpráva do stavu modelu, když kontakt s neplatným telefonním číslem je předaný metodě CreateContact().
- CreateContactInvalidEmail() - testy, zda je přidána chybová zpráva do stavu modelu, když kontakt s neplatnou e-mailovou adresou je předaný metodě CreateContact()...

První test ověřuje, že a obraťte se na platný nevygeneruje chybu ověření. Zbývající testy zkontrolujte jednotlivých pravidel ověřování.

Kód pro tyto testy je obsažený v výpis 1.

**Listing 1 - Models\ContactManagerServiceTest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample1.vb)]


Protože třída kontaktu jsme se používá v výpis 1, je potřeba přidat odkaz na rozhraní Entity Framework Microsoft do našich testovacího projektu. Přidáte odkaz na sestavení System.Data.Entity.


Výpis 1 obsahuje metodu s názvem inicializaci(), který je upraven pomocí atributu [TestInitialize]. Tato metoda je volána automaticky, než se spustí všechny testy jednotek (nazývá se 5krát bezprostředně před všechny testy jednotek). Metoda inicializaci() vytvoří imitované úložiště s následující řádek kódu:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample2.vb)]

Tento řádek kódu používá ke generování imitované úložiště z rozhraní IContactManagerRepository Moq framework. Imitované úložiště se používá namísto skutečné EntityContactManagerRepository předejdete přístup k databázi, když se spustí každý testování částí. Implementuje metodu rozhraní IContactManagerRepository imitované úložiště, ale t Tento metody ve skutečnosti provádět žádné kroky.

> [!NOTE] 
> 
> Pokud používáte Moq framework, je rozdíl mezi \_mockRepository a \_mockRepository.Object. První odkazuje na třídu model (IContactManagerRepository z), která obsahuje metody pro určení, jak se bude chovat imitované úložiště. K tomu odkazuje na skutečnou imitované úložiště, který implementuje rozhraní IContactManagerRepository.


Imitované úložiště se používá v metodě inicializaci() při vytváření instance třídy ContactManagerService. Všechny testy jednotlivých jednotek použít tuto instanci třídy ContactManagerService.

Výpis 1 obsahuje pět metody, které odpovídají jednotlivým testování částí. Každá z těchto metod opatřen atributem [TestMethod]. Při spuštění testování částí, se nazývá libovolné metody, která má tento atribut. Jinými slovy libovolné metody, která opatřen atributem [TestMethod] je testování částí.

První testování částí s názvem CreateContact(), ověřuje, že volání CreateContact() vrátí hodnotu true, pokud je platnou instanci třídy, obraťte se na předaný metodě. Test vytvoří instanci třídy kontaktu, volá metodu CreateContact() a ověřuje, že CreateContact() vrátí hodnotu true.

Zbývající testy ověřte, zda při volání metody CreateContact() neplatný kontaktují potom metoda vrátí hodnotu false a stav modelu, který je přidána chybová zpráva ověření očekávané. Například CreateContactRequiredFirstName() test vytvoří instanci třídy, obraťte se s prázdný řetězec pro vlastnost jeho FirstName. V dalším kroku CreateContact() metoda volána s neplatnou kontakt. Nakonec test ověřuje, že CreateContact() vrací hodnotu false a že stav modelu, který obsahuje chybovou zprávu ověření očekávané "jméno je povinné."

Testování částí v výpis 1 můžete spustit výběrem možnosti nabídky **Test, spustit všechny testy v řešení (CTRL + R, A)**. Výsledky testů se zobrazí v okně výsledků testu (viz obrázek 4).


[![Výsledky testu](iteration-5-create-unit-tests-vb/_static/image4.jpg)](iteration-5-create-unit-tests-vb/_static/image7.png)

**Obrázek 04**: výsledky testu ([Kliknutím zobrazit obrázek v plné velikosti](iteration-5-create-unit-tests-vb/_static/image8.png))


## <a name="creating-unit-tests-for-controllers"></a>Testování částí pro řadiče

Aplikace ASP.NET MVC řízení toku interakce s uživatelem. Při testování řadič, budete chtít otestovat, zda řadičem vrací pravé akce výsledek a zobrazení data. Můžete také chtít otestovat, zda řadič komunikuje s třídy modelu způsobem očekává.

Například výpis 2 obsahuje dvě testování částí pro kontaktujte řadič Create() metoda. První testování částí ověřuje, že když a obraťte se na platný předána metodě Create() pak metodu Create() přesměruje na Index akci. Jinými slovy když uplyne a obraťte se na platný, Metoda Create() by měl vrátit RedirectToRouteResult, která představuje akci Index.

Jsme nejsou zobrazeny chcete t testování vrstvě služby ContactManager, když se testují vrstvě řadiče. Proto jsme model vrstvy služby s následující kód v metodě inicializovat:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample3.vb)]

V testu jednotek CreateValidContact() můžeme model chování volání vrstvě služby CreateContact() metoda s následující řádek kódu:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample4.vb)]

Tento řádek kódu způsobí, že služba imitované ContactManager vrátí hodnotu true v případě jeho CreateContact() metoda je volána. Ve vrstvě služby mocking, abychom mohli otestovat chování kontroleru bez nutnosti spouštět všechny kód ve vrstvě služby.

Druhý testování částí ověřuje, že akce Create() vrátí zobrazení pro vytváření, pokud je neplatný kontaktujte předaný metodě. Jsme způsobit vrstvě služby CreateContact() metoda vrátí hodnotu false s následující řádek kódu:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample5.vb)]

Pokud Metoda Create() chová jako Očekáváme, měla by při vrstvě služby vrátí hodnotu false vrátit zobrazení pro vytváření. Tímto způsobem řadičem můžete zobrazit chybové zprávy ověření v zobrazení pro vytváření a uživatel má možnost opravit tento neplatné obraťte se na vlastnosti.


Pokud máte v plánu vytvářet testy částí pro řadičů budete muset vrátit zobrazení explicitní názvy z vaší akce kontroleru. Například nevrátí zobrazení takto:

Vrátí View()

Místo toho vrátíte zobrazení takto:

Vrátí View("Create")

Pokud si nejste explicitní při vrácení zobrazení ViewResult.ViewName vlastnost vrací prázdný řetězec.


**Výpis 2 - Controllers\ContactControllerTest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample6.vb)]

## <a name="summary"></a>Souhrn

V této iteraci jsme vytvořili testování částí pro naši aplikaci obraťte se na správce. Tyto testy jednotek jsme můžete spustit kdykoli ověřit, že naše aplikace stále chová způsobem, který se očekává. Testování částí fungují jako bezpečnostní opatření pro naši aplikaci to nám bezpečně upravit aplikaci v budoucnu.

Jsme vytvořili dvě sady testů jednotek. Nejdřív jsme testovali ověřovací logiku vytvořením testování částí pro naše vrstvu služby. V dalším kroku jsme testovali logiku řízení toku vytvořením testování částí pro naše řadiče vrstvu. Při testování naše vrstvy služby, jsme izolované naše testy pro naše vrstvu služby z našich vrstvy úložiště mocking naše vrstvy úložiště. Při testování vrstvě řadiče, jsme izolované naše testy pro naše vrstvu řadič mocking vrstvě služby.

V další iterace jsme upravit aplikaci obraťte se na správce, tak, aby podporoval skupiny kontaktu. Tato nová funkce přidáme do aplikace pomocí procesu návrhu softwaru s názvem testy řízený vývoj.

> [!div class="step-by-step"]
> [Předchozí](iteration-4-make-the-application-loosely-coupled-vb.md)
> [další](iteration-6-use-test-driven-development-vb.md)
