---
uid: mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
title: 'Iterace #5 – vytvoření testů jednotek (VB) | Dokumentace Microsoftu'
author: microsoft
description: V páté iteraci jsme snadněji naší aplikace spravovat a upravovat tak, že přidáte testy jednotek. Jsme napodobení našich tříd datových modelů a vytváření testů jednotek pro o...
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: c6e5c036-2265-4fa7-a9eb-47f197bdc262
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
msc.type: authoredcontent
ms.openlocfilehash: 49e8b3eb6c0e8fb7199816d0c2b0843e0519872a
ms.sourcegitcommit: a25b572eaed21791230c85416f449f66a405ec19
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/01/2018
ms.locfileid: "39396087"
---
<a name="iteration-5--create-unit-tests-vb"></a>Iterace #5 – vytvoření testů jednotek (VB)
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout kód](iteration-5-create-unit-tests-vb/_static/contactmanager_5_vb1.zip)

> V páté iteraci jsme snadněji naší aplikace spravovat a upravovat tak, že přidáte testy jednotek. Jsme napodobení našich tříd datových modelů a vytváření testů jednotek pro naše řadiče a logiku ověřování.


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

V předchozí iteraci aplikace Správce kontaktů jsme rozdělili do více volně spárované aplikace. Jsme oddělené aplikaci do různých kontroler, služby a vrstvy úložiště. Každá vrstva komunikuje s vrstvy pod ní prostřednictvím rozhraní.

Jsme rozdělili aplikace, aby aplikace lépe spravovat a upravovat. Například pokud bychom někdy potřebovali novou technologii přístup data, můžeme jednoduše změnit vrstvu úložiště bez zásahu do kontroleru nebo vrstva služby. Tím, že správce kontaktů volně propojené a jsme uložit provedené aplikace odolnější vůči změnit.

Ale co se stane, když je potřeba přidat nové funkce do aplikace Správce kontaktů? Nebo co se stane, když jsme opravy chyby? Líto, že odcházíte, ale také ověřené pravdy psaní kódu, který je v každé touch vytvoříte riziko zavlečení nových chyb kódu.

Například jeden den v pořádku, Správce může vás požádat o novou funkci přidat do Správce kontaktů. Můžete přidat podporu pro skupiny kontaktu chce. Jana chce, abyste uživatelům uspořádat si kontakty do skupiny, například přátel, firmy a tak dále.

Aby bylo možné implementovat tuto novou funkci, budete muset změnit všechny tři vrstvy aplikace Správce kontaktů. Bude potřeba přidat nové funkce do řadiče, vrstva služby a úložiště. Jakmile začnete upravovat kód, riskujete dopadem na dřívější kód funkce, které fungovalo.

Refaktoring naší aplikace do samostatných vrstev, jako jsme to udělali v předchozí iteraci byla dobrá věc. To byl dobrá věc, protože umožňuje nám zvýšit celý vrstvy provádět změny bez zásahu do zbývajících částí aplikace. Ale pokud chcete lépe spravovat a upravovat kód v rámci vrstvy, musíte vytvořit testy jednotek pro kód.

Použijete jednotka testu jednotlivých částí kódu. Tyto jednotky kódu jsou menší než vrstvy celé aplikace. Testování částí se obvykle používají k ověření, jestli konkrétní metody v kódu se chová způsobem, který očekáváte. Například by vytvoření testování částí pro metodu CreateContact() vystavené ContactManagerService třídy.

Testy jednotek pro pracovní aplikace, která je jenom jako bezpečnostní. Pokaždé, když upravíte kódu v aplikaci, můžete spustit sady testů jednotek ke kontrole, jestli úpravy přeruší stávající funkce. Testy jednotek byl kód bezpečně upravovat. Testy jednotek Ujistěte se, veškerý kód ve vaší aplikaci odolnější vůči změnit.

V této iterace přidáme k naší aplikace Správce kontaktů testování částí. Tímto způsobem, při příští iteraci, můžeme přidat skupiny kontaktu pro naši aplikaci bez starostí o zásadní stávajících funkcí.

> [!NOTE] 
> 
> Existují různé architektury, včetně MbUnit, NUnit a xUnit.net testování částí. V tomto kurzu používáme jednotkových testů součástí sady Visual Studio. Však stejně snadno můžete jedno z těchto alternativních rozhraní.


## <a name="what-gets-tested"></a>Co získá testování

V ideálním všech kódů by být pokryté komponentami testování částí. V ideálním budete mít dokonalý bezpečnostní. By moci upravit libovolném řádku kódu v aplikaci a vědět, okamžitě, spuštěním testování částí, zda tato změna se podařilo přerušit stávajících funkcí.

Můžeme ale nejsou t za provozu v ideálním. V praxi, při psaní testů jednotek můžete soustředit na psaní testů pro vaši obchodní logiku (například logiku ověřování). Konkrétně můžete *nejsou* vytvořte jednotkové testy pro vaše data přístup logiku nebo logice zobrazení.

Aby byly užitečné, musí spustit testy jednotek velmi rychle. Snadno můžete shromažďujete stovkách (nebo dokonce tisících) testů jednotek pro aplikace. Pokud trvat dlouhou dobu pro spuštění testů jednotek budete vyhnout, jejich spuštění. Jinými slovy jsou zbytečné pro každodenní kódování, účely dlouhodobě spuštěných testů jednotek.

Z tohoto důvodu se obvykle nenapíšete testů jednotek pro kód, který komunikuje s databází. Provozování testů jednotek pro živé databáze může být pomalý. Místo toho napodobení databáze a napsat kód, který komunikuje s mock databáze (podíváme na to napodobování databázi níže).

Ze stejného důvodu obvykle nenapíšete testů jednotek pro zobrazení. Aby bylo možné testovat zobrazení, musíte aktivovat webový server. Vzhledem k tomu vybudujete webový server je poměrně pomalý proces, vytváření testů jednotek pro zobrazení se nedoporučuje.

Pokud zobrazení obsahuje složitější logiku byste zvážit, přesun logiku do metody Helper. Můžete psát testy jednotek pro pomocné metody, které jsou spuštěny bez rozbíhání webový server.

> [!NOTE] 
> 
> Při psaní testů pro logikou přístupu k datům nebo zobrazení logiky není vhodné při psaní testů jednotek, tyto testy může být velmi důležité, při vytváření funkční nebo integraci testů.


> [!NOTE] 
> 
> ASP.NET MVC je webové formuláře zobrazovací modul. Webové formuláře zobrazovací modul je závislé na webovém serveru, nemusí být ostatní moduly zobrazení.


## <a name="using-a-mock-object-framework"></a>Pomocí Mock objektu Framework

Při vytváření testů jednotek, musíte téměř vždy využít architekturu napodobení objektu. Napodobení objektu rozhraní umožňuje vytvářet mocks a zástupných procedur pro třídy ve vaší aplikaci.

Například můžete použít rozhraní napodobení objekt ke generování verzi mock třídy úložiště. Tímto způsobem třídu mock úložiště můžete použít namísto třídy skutečné úložiště v rámci testování součástí. Pomocí mock úložiště umožňuje vyhnout se provádění kódu databáze při provádění testu jednotek.

Visual Studio nezahrnuje framework napodobení objektu. Existují však několik komerční s otevřeným zdrojovým kódem napodobení objekt k dispozici pro rozhraní .NET framework:

1. Moq - toto rozhraní je k dispozici v rámci licence BSD open source. Můžete si stáhnout Moq z [ https://code.google.com/p/moq/ ](https://code.google.com/p/moq/).
2. Rhino Mocks – toto rozhraní je k dispozici v rámci licence BSD open source. Můžete si stáhnout Rhino Mocks z [ http://ayende.com/projects/rhino-mocks.aspx ](http://ayende.com/projects/rhino-mocks.aspx).
3. Typemock Odpojovač – to je komerční rozhraní. Můžete stáhnout zkušební verzi z [ http://www.typemock.com/ ](http://www.typemock.com/).

V tomto kurzu jsem se rozhodla používat Moq. Však stejně snadno můžete Rhino Mocks nebo Typemock Odpojovač vytvořit model objektů aplikace Správce kontaktů.

Před použitím Moq, budete muset provést následující kroky:

1. .
2. Předtím, než můžete rozbalit soubor ke stažení, ujistěte se, že pravým tlačítkem myši na soubor a klikněte na tlačítko s popiskem **Odblokovat** (viz obrázek 1).
3. Rozbalte stažený soubor.
4. Přidat odkaz na sestavení Moq do testovacího projektu tak, že vyberete možnost nabídky **projektu, přidejte odkaz** otevřít **přidat odkaz** dialogového okna. Na kartě Procházet přejděte do složky, kde odblokujte Moq a vyberte Moq.dll sestavení. Klikněte na tlačítko **OK** tlačítko (viz obrázek 2).


[![Odblokování Moq](iteration-5-create-unit-tests-vb/_static/image1.jpg)](iteration-5-create-unit-tests-vb/_static/image1.png)

**Obrázek 01**: odblokování Moq ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-5-create-unit-tests-vb/_static/image2.png))


[![Po přidání Moq odkazy](iteration-5-create-unit-tests-vb/_static/image2.jpg)](iteration-5-create-unit-tests-vb/_static/image3.png)

**Obrázek 02**: odkazy po přidání Moq ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-5-create-unit-tests-vb/_static/image4.png))


## <a name="creating-unit-tests-for-the-service-layer"></a>Vytváření testů jednotek pro vrstvu služby

Umožní s začněte vytvořením sady testů jednotek pro vrstvu služby s aplikací naše Správce kontaktů. Použijeme tyto testy k ověření naší logiku ověřování.

Vytvořte novou složku s názvem modely v projektu ContactManager.Tests. V dalším kroku klikněte pravým tlačítkem na složku modely a vyberte **přidat, nový Test**. **Přidat nový Test** se zobrazí dialogové okno je znázorněno na obrázku 3. Vyberte **testování částí** šablony a pojmenujte nový test ContactManagerServiceTest.vb. Klikněte na tlačítko **OK** tlačítko Přidat nový test do projektu Test.

> [!NOTE] 
> 
> Obecně byste měli strukturu složky pro testovací projekt tak, aby odpovídaly struktuře složky vašeho projektu ASP.NET MVC. Například umístit kontroleru testů ve složce řadiče testů modelu ve složce modely a tak dále.


[![Models\ContactManagerServiceTest.cs](iteration-5-create-unit-tests-vb/_static/image3.jpg)](iteration-5-create-unit-tests-vb/_static/image5.png)

**Obrázek 03**: Models\ContactManagerServiceTest.cs ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-5-create-unit-tests-vb/_static/image6.png))


Na začátku chceme otestovat CreateContact() metoda použitá v třídě ContactManagerService. Vytvoříme následujících pět testů:

- CreateContact() - testů tohoto CreateContact() vrátí hodnotu true, pokud platný kontakt je předán do metody.
- CreateContactRequiredFirstName() - testy, chybová zpráva se přidá do stavu modelu při kontaktu s chybějící křestní jméno se předá metodě CreateContact().
- CreateContactRequredLastName() - testy, chybová zpráva se přidá do stavu modelu při kontaktu s chybějící příjmení se předá metodě CreateContact().
- CreateContactInvalidPhone() - testy, chybová zpráva se přidá do stavu modelu při kontaktu s neplatným telefonním číslem se předá metodě CreateContact().
- CreateContactInvalidEmail() - testy, chybová zpráva se přidá do stavu modelu při kontaktu s neplatnou e-mailovou adresu se předá metodě CreateContact()...

První test ověří, že platný kontakt nevygeneruje chybu ověřování. Zbývající testy zkontrolujte všech ověřovacích pravidel.

Kód pro tyto testy je obsažen v informacích 1.

**Výpis 1 - Models\ContactManagerServiceTest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample1.vb)]


Protože používáme třídy kontakt v informacích 1, potřebujeme přidat odkaz na rozhraní Entity Framework společnosti Microsoft do našich testovacího projektu. Přidáte odkaz na sestavení System.Data.Entity.


Výpis 1 obsahuje metodu s názvem Initialize(), který je upraven pomocí atributu [TestInitialize]. Tato metoda je volána automaticky před spuštěním každého testu jednotek (5krát je volána bezprostředně před každou testování částí). Metodu Initialize() vytvoří mock úložiště s následující řádek kódu:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample2.vb)]

Tento řádek kódu používá ke generování mock úložiště z rozhraní IContactManagerRepository Moq framework. Mock úložiště se používá namísto skutečné EntityContactManagerRepository pro zabránění přístupu k databázi, když se spustí každý Jednotkový test. Mock úložiště implementuje metodu rozhraní IContactManagerRepository, ale t don metody skutečně provádět.

> [!NOTE] 
> 
> Pokud používáte rozhraní framework Moq, je rozdíl mezi \_mockRepository a \_mockRepository.Object. První odkazuje na třídu model (IContactManagerRepository z), která obsahuje metody pro určení, jak se bude chovat mock úložiště. Druhá možnost odkazuje na skutečné mock úložiště, který implementuje rozhraní IContactManagerRepository.


Při vytváření instance třídy ContactManagerService mock úložiště slouží v metodu Initialize(). Všechny jednotlivé jednotkových testů používat tuto instanci třídy ContactManagerService.

Výpis 1 obsahuje pět metod, které odpovídají jednotlivým testů jednotek. Každá z těchto metod je upravený pomocí atributu [TestMethod]. Při spuštění testů jednotek, se nazývá jakoukoli metodu, která má tento atribut. Jinými slovy jakoukoli metodu, která je upravena pomocí atributu [TestMethod] je testování částí.

První test částí s názvem CreateContact(), ověří, že CreateContact() volání vrátí hodnotu true při vytvoření platné instance třídy kontakt je předán metodě. Test vytvoří instanci třídy kontakt, volá metodu CreateContact() a ověřuje, že CreateContact() vrátí hodnotu true.

Zbývající testy ověřte, že při volání metody CreateContact() neplatný kontakt pak metoda vrátí hodnotu false a chybových zpráv ověření očekávané se přidá do stavu modelu. Například CreateContactRequiredFirstName() test vytvoří instanci třídy kontaktu s prázdným řetězcem pro jeho vlastnost FirstName. V dalším kroku je volána metoda CreateContact() neplatný kontakt. Nakonec test ověří, že CreateContact() vrátí hodnotu false a že ze stavů modelu obsahuje chybovou zprávu ověření očekávané "jméno je povinné."

Můžete spustit testy jednotek v informacích 1 tak, že vyberete možnost nabídky **testovací běh, všechny testy v řešení (CTRL + R, A)**. Výsledky testů se zobrazí v okně Výsledky testu (viz obrázek 4).


[![Výsledky testu](iteration-5-create-unit-tests-vb/_static/image4.jpg)](iteration-5-create-unit-tests-vb/_static/image7.png)

**Obrázek 04**: výsledky testů ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-5-create-unit-tests-vb/_static/image8.png))


## <a name="creating-unit-tests-for-controllers"></a>Vytváření testů jednotek pro Kontrolery

Aplikace ASP.NET MVC řízení toku interakci s uživatelem. Při testování kontroleru, budete chtít otestovat, jestli kontroler vrací pravé akce výsledek a zobrazit data. Můžete také otestovat, jestli řadič komunikuje s tříd modelu způsobem očekávání.

Například výpis 2 obsahuje dva testy jednotek pro kontroler kontakt Create() metoda. První test částí ověřuje, že když platný kontakt se předá metodě Create(), pak metoda Create() přesměruje na akce indexu. Jinými slovy při předání platný kontakt, Create() metoda by měla vrátit RedirectToRouteResult, představující akce indexu.

Budeme zadávat t chcete testovací vrstva služby ContactManager při testujeme vrstvě kontroleru. Proto jsme napodobení vrstvě služby s následujícím kódem v metodě inicializace:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample3.vb)]

V testu jednotek CreateValidContact() jsme napodobení chování volání vrstva služby CreateContact() metodu s následující řádek kódu:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample4.vb)]

Tento řádek kódu způsobí, že vrací hodnotu true, když je zavolána metoda CreateContact() mock ContactManager služba. Podle napodobování vrstva služby, abychom mohli otestovat chování kontroleru aniž by bylo nutné provést všechny kódu ve vrstvě služeb.

Druhý test jednotek ověřuje, že akce Create() vrátí zobrazení pro vytváření při neplatný kontakt je předán metodě. Jsme způsobit vrstva služby CreateContact() metoda vrátí hodnotu false s následující řádek kódu:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample5.vb)]

Pokud Metoda Create() chová jako Očekáváme, že pak měla by vrátit zobrazení pro vytváření při vrstva služby vrátí hodnotu false. Tímto způsobem kontroleru můžete zobrazit chybové zprávy ověření v zobrazení pro vytváření a uživatel tak možnost opravit tuto neplatné vlastnosti kontaktu.


Pokud máte v plánu testů jednotek pro vaše řadiče sestavení budete muset vrátit explicitní zobrazit názvy z akce kontroleru. Například nesmí vracet zobrazení takto:

Vrátí View()

Místo toho vrátí zobrazení takto:

Vrátí View("Create")

Pokud si nejste explicitní při vrácení zobrazení ViewResult.ViewName vlastnost vrací prázdný řetězec.


**Výpis 2 - Controllers\ContactControllerTest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample6.vb)]

## <a name="summary"></a>Souhrn

V této iterace jsme vytvořili testů jednotek pro naši aplikaci Správce kontaktů. V každém okamžiku k ověření, že naše aplikace stále chová způsobem, který Očekáváme, že můžeme spustit tyto testy jednotek. Testování částí fungovat jako bezpečnostní pro naši aplikaci, která umožňuje nám bezpečně upravovat naši aplikaci v budoucnosti.

Vytvořili jsme dvě sady testů jednotek. Nejprve jsme otestovali naše logiku ověřování pomocí testů jednotek pro naše vrstvu služby. Dále jsme testovali naše logiky řízení toku ve vytváření testů jednotek pro naše vrstvu kontroleru. Při testování vrstva naše služby, jsme izolována testech pro naše služby vrstvu z našich vrstvy úložiště napodobování naše vrstvy úložiště. Při testování vrstvě kontroleru, jsme izolována testech pro naše řadič vrstvu napodobování vrstva služby.

V další iteraci upravíme Správce kontaktů aplikaci tak, aby podporoval skupiny kontaktu. Tato nová funkce přidáme k naší aplikaci pomocí procesu návrhu softwaru s názvem vývoj řízený testováním.

> [!div class="step-by-step"]
> [Předchozí](iteration-4-make-the-application-loosely-coupled-vb.md)
> [další](iteration-6-use-test-driven-development-vb.md)
