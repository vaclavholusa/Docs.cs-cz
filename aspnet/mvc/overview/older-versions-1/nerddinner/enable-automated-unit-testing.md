---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: Povolení automatického testování jednotek | Dokumentace Microsoftu
author: microsoft
description: Krok 12 ukazuje, jak vyvíjet sadu automatizované testy jednotky, která ověřte naše NerdDinner funkce a které nám umožní provedou změny...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: 2247dc2e6d22cc0d5ddba97dfe6c7d2d1b0e49be
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819404"
---
<a name="enable-automated-unit-testing"></a>Povolení automatického testování jednotek
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Toto je kroku 12 bezplatného [kurz vývoje aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , který procházení procházení po tom, jak sestavit malý, ale bylo možné provést, webové aplikace pomocí ASP.NET MVC 1.
> 
> Krok 12 ukazuje, jak vyvíjet sadu automatizované testy jednotky, která ověřit naše funkce NerdDinner a dávají nám provedou změny a vylepšení pro aplikaci v budoucnosti.
> 
> Pokud používáte ASP.NET MVC 3, doporučujeme je provést [získávání začít s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.


## <a name="nerddinner-step-12-unit-testing"></a>NerdDinner krok 12: Testování částí

Pojďme vyvíjet sadu automatizované testy jednotky, která ověřit naše funkce NerdDinner a dávají nám provedou změny a vylepšení pro aplikaci v budoucnosti.

### <a name="why-unit-test"></a>Proč pro testování částí?

Na disku do práce jeden ráno máte i s náhlými pracovat bez velkých příprav dávku inspirace o aplikaci, kterou právě pracujete. Je dobré si uvědomit, že dojde ke změně, které můžete implementovat, které se aplikace výrazně lepší. Může být refaktoring, který vyčistí kód, přidává nové funkce nebo opravuje chybu.

Na otázku, kterou jste confronts až přijedete počítače je – "Jak bezpečné je ho tak, aby toto vylepšení?" Co když provádění změny má vedlejší účinky nebo přeruší něco? Tato změna může být jednoduchý a trvat jenom pár minut a implementaci, ale co když trvá hodiny můžete ručně vyzkoušet všechny scénáře aplikací? Co když zapomenete pro scénář a nefunkční aplikace přejde do produkčního prostředí? Toto vylepšení ve skutečnosti vyplácet všechny je vidět?

Automatizované testy jednotky můžete zadat bezpečnostní, která umožňuje průběžně vylepšovat vaše aplikace a vyhnout se bát kód, který právě pracujete. Provádí se s automatizované testy, které rychle ověřte, že funkce vám umožní kódování s jistotou – a umožněte k vylepšování vám nemusí jinak mají popisovač příjemné. Kromě toho pomáhají vytvářet řešení, které jsou snadněji spravovatelné a mají delší životnost – které vede k mnohem větší návratnost investic.

Architektura ASP.NET MVC umožňuje snadné a přirozené do aplikace funkce testování částí. Umožňuje také otestovat řízené vývoj (TDD) pracovního postupu, který umožňuje včasného testování založené na vývoj.

### <a name="nerddinnertests-project"></a>NerdDinner.Tests projektu

Když naše aplikace NerdDinner jsme vytvořili na začátku tohoto kurzu, se zobrazí výzva se dialogové okno s dotazem, jestli jsme chtěli vytvořit projekt testování částí se vyhýbáte projekt aplikace:

![](enable-automated-unit-testing/_static/image1.png)

Společnost Microsoft uchovávat "Ano, vytvořit projekt testování částí" vybraný přepínač –, což vedlo "NerdDinner.Tests" projektu se přidávají do našich řešení:

![](enable-automated-unit-testing/_static/image2.png)

NerdDinner.Tests projekt odkazuje na sestavení projektu aplikace NerdDinner a umožňuje nám to snadno přidat automatizovaných testů, který ověřit funkčnost aplikace.

### <a name="creating-unit-tests-for-our-dinner-model-class"></a>Vytváření testů jednotek pro naše třída modelu Dinner

Přidejme do našich NerdDinner.Tests projektu, zkontrolujte, Dinner třídy, které jsme vytvořili, když jsme vytvořili naši modelu vrstvy některé testy.

Začneme tím, že vytvoříte novou složku v rámci naší testovací projekt s názvem "Modely" kde jsme budete umístit testech týkající se modelu. Vytvoříme klikněte pravým tlačítkem myši na složku a zvolte **Add -&gt;nový Test** příkazu nabídky. Tím se otevře dialogové okno "Přidat nový Test".

My jsme vybrali k vytvoření testování částí"" a pojmenujte ho "DinnerTest.cs":

![](enable-automated-unit-testing/_static/image3.png)

Když kliknete na tlačítko "ok" Visual Studio přidejte (a otevřete) DinnerTest.cs soubor do projektu:

![](enable-automated-unit-testing/_static/image4.png)

Výchozí šablonu testů jednotek sady Visual Studio má spoustu kotle talíře kódu v rámci, které se mi najít trochu komplikované. Pojďme vyčistěte ho tak, aby obsahovala pouze kód uvedený níže:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

Atribut [TestClass] ve třídě DinnerTest výše ji identifikuje jako třída, která bude obsahovat testy, stejně jako volitelný testovací inicializace a jejich vyřazování z provozu kódu. Přidáním veřejné metody, které mají atributu [TestMethod] na nich jsme můžete definovat testy v rámci něj.

V následující tabulce jsou první dva testy, které přidáme, které vykonávají naší třídy Dinner. První test ověří, naše společnost Dinner je neplatný, pokud se vytvoří nová společnost Dinner bez všechny vlastnosti nastavena správně. Druhý test ověří, jestli je naše společnost Dinner platné při večeři má všechny vlastnosti nastavené platnými hodnotami:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

Můžete si všimnout výše, naše názvy testů jsou velmi explicitní (a částečně podrobné). Protože jsme může skončit vytváření stovek nebo tisíců malé testů, a chceme, aby byl snadno a rychle tak určit záměr a chování každého z nich (zejména při Těšíme se v seznamu chyb v nástroji test runner) to provádíme. Názvy testů by měl být pojmenován po funkčnost, kterou testují. Výše se používá "podstatné jméno\_by měl\_příkaz" vzor pro pojmenování.

Jsme vložili struktury testy pomocí "AAA" testování vzor – což je zkratka pro "Uspořádat, Act, kontrolní výraz":

- Uspořádat: Nastavení testování částí
- ACT: Vykonávat jednotky v rámci testu a zaznamenat výsledky
- Vyhodnocení: Ověřte, že chování

Při psaní testů, které chceme, abyste se vyhnuli nutnosti jednotlivé testy provést příliš mnoho. Místo toho každý test by měl ověřit jenom jeden koncept (což způsobí, že je mnohem snadnější identifikovat příčiny selhání). Dobrým vodítkem je vyzkoušet a mít jenom jeden výraz příkazu pro každý test. Pokud máte více než jeden kontrolní výraz příkazu v testovací metodě, ujistěte se, že se všechny používají k testování stejný koncept. Pokud máte pochybnosti, ujistěte se, jiného testu.

### <a name="running-tests"></a>Spouštění testů

Visual Studio 2008 Professional (a vyšší edice) zahrnuje integrované testu, který slouží ke spuštění testování částí v aplikaci Visual Studio projektů v rámci rozhraní IDE. Můžeme vybrat **Test -&gt;Run -&gt;všechny testy v řešení** nabídky příkazu (nebo type Ctrl R, A) ke spuštění všech našich testů jednotek. Také jsme umístěte naše kurzor v rámci konkrétní testovací třídě nebo testovací metody a použít **Test -&gt;Run -&gt;testy v rámci aktuálního kontextu** příkaz nabídky (nebo type Ctrl R, T) spustit podmnožinu testů jednotek.

Pojďme umístění naše kurzoru v rámci třídy DinnerTest a typ "Ctrl R, T" na dva spouštět testy, které jsme definovali. Při provádění to "Výsledky testu" okna se zobrazí v rámci sady Visual Studio a uvidíme výsledky našich testovacích běhů uvedené v rámci něj:

![](enable-automated-unit-testing/_static/image5.png)

*Poznámka: V okně Výsledky testu VS nezobrazuje ve výchozím nastavení sloupce název třídy. To můžete přidat kliknutím pravým tlačítkem v okně Výsledky testu a pomocí příkazu nabídky Přidat nebo odebrat sloupce.*

Naše dva testy trvaly jenom zlomek sekundy ke spuštění – a jako vy můžete viz oba předán. Teď můžeme přejít a rozšířit tak, že vytvoříte další testy, které ověřte ověření příslušné pravidlo, jakož i pokrývají dvě metody helper - IsUserHost() a IsUserRegisterd() –, který jsme přidali do třídy Dinner. Tyto testy s místem pro třídy Dinner filtrovacího řetězce se mnohem jednodušší a bezpečnější přidat nová obchodní pravidla a ověření k němu v budoucnu. Můžeme přidat naše nové logice pravidla Dinner a během několika sekund ověřte, že ji ještě fungovat některé naše předchozí funkce logic.

Všimněte si, jak pomocí testu popisný název usnadňuje rychle pochopit, co je ověření každého testu. Můžu jenom doporučit pomocí **nástroje -&gt;možnosti** příkaz nabídky, otevřete na testovací nástroje -&gt;konfigurační obrazovce pro spuštění testu a kontrola, zda "dvakrát kliknout výsledky testů jednotek neúspěšné nebo počet neprůkazných: Zobrazí zaškrtávací políčko bodu selhání při testu rozhraní". To vám umožní klikněte dvakrát na chybu v okně Výsledky testu a okamžitý přechod na selhání kontrolní výraz.

### <a name="creating-dinnerscontroller-unit-tests"></a>Vytváření testů jednotek DinnersController

Nyní vytvoříme některé testy jednotek, které ověřují naše DinnersController funkce. Začneme kliknutím pravým tlačítkem na složku "Řadiče" v rámci naší testovacího projektu a klikněte na tlačítko **Add -&gt;nový Test** příkazu nabídky. Vytvoříme "Testování částí" a pojmenujte ho "DinnersControllerTest.cs".

Vytvoříme dvě testovací metody, které ověřují Details() metody akce na DinnersController. První ověří, že zobrazení se vrátí, pokud se požaduje existující Dinner. Druhý se ověří, že "Serveru" zobrazení se vrátí, pokud se požaduje neexistující Dinner:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

Výše uvedený kód se zkompiluje čištění. Když spustíme testy, ale oba selhání:

![](enable-automated-unit-testing/_static/image6.png)

Když se podíváte na chybové zprávy, uvidíme, že byl důvod testy se nezdařilo, protože naše třída DinnersRepository se nemohl připojit k databázi. Naše aplikace NerdDinner používá připojovací řetězec do místního souboru systému SQL Server Express, která se nachází v části \App\_datový adresář projektu aplikace NerdDinner. Protože naše NerdDinner.Tests projekt zkompiluje a spustí v jiném adresáři poté projekt aplikace, relativní cestu umístění naše připojovací řetězec je nesprávný.

Jsme *může* opravit tak, že zkopírujete soubor databáze SQL Express na naše testovací projekt a potom k němu přidejte odpovídajícím testovacím řetězcových připojení v souboru App.config naše testovacího projektu. To byste získali výše uvedenými testy se odblokuje a spuštěný.

Testování částí kódu pomocí skutečná databáze, ale přináší určité problémy. Konkrétně:

- Může výrazně zpomalit spuštění testů jednotek. Tím déle trvá spuštění testů, tím méně pravděpodobné, že jsou v jejich spuštění často. V ideálním případě byste měli být schopni spustit v sekundách – a bylo možné něco, co uděláte jako přirozeně jako kompilaci projektu testů jednotek.
- To komplikuje instalačním a úklidovém logiky v rámci testů. Chcete, aby každý Jednotkový test chcete být izolované a nezávisle na ostatních (se žádné vedlejší účinky nebo závislostmi). Při práci na skutečné databázi máte dávejte stavu a resetujte si ho mezi testy.

Podívejme se na návrhový vzor, který se nazývá "injektáž závislostí", který vám pomůže tyto problémy obejít a vyhněte se nutnosti skutečná databáze pomocí našich testů.

### <a name="dependency-injection"></a>Injektáž závislostí

Právě teď DinnersController je úzce "s velkou provázaností" DinnerRepository třídy. "Párování" odkazuje na situaci, kde třída explicitně závisí na jiné třídy fungování:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

Protože třída DinnerRepository vyžaduje přístup k databázi, vysoce provázané závislost DinnersController třída nemá na DinnerRepository končí by nám umožnily databáze mohl DinnersController metody akce má být testována.

Tento problém jsme můžete získat pomocí vzoru návrhu volá "injektáž závislostí" – což je přístup, ve kterém se závislosti (stejně jako třídy úložiště, které poskytují přístup k datům) vytvoří už nebude implicitně v rámci třídy, které je používají. Místo toho závislosti můžete explicitně předán do třídy, která je používá pomocí argumentů konstruktoru. Pokud tyto závislosti jsou definovány pomocí rozhraní, máme pak flexibilitu a zajistěte tak předání implementace "falešnou" závislostí pro scénáře testování částí. To umožňuje vytvořit implementace závislosti daného testu, které skutečně nevyžadují přístup k databázi.

Tento údaj zobrazíte v akci, můžeme implementovat injektáž závislostí s naší DinnersController.

#### <a name="extracting-an-idinnerrepository-interface"></a>Extrahování rozhraní IDinnerRepository

Naše prvním krokem bude vytvoření nového IDinnerRepository rozhraní, který zapouzdřuje kontrakt úložiště, které se načítají a aktualizují večeří vyžadují naše řadiče.

Jsme tento kontrakt rozhraní definovat ručně tak, že pravým tlačítkem na složku \Models a potom kliknete **Add -&gt;nová položka** příkaz nabídky a vytváří se nové rozhraní s názvem IDinnerRepository.cs.

Také jsme můžete použít automaticky refaktoring nástroje integrovaná do sady Visual Studio Professional (a vyšší edice) k extrahování a vytvoření rozhraní pro nás z naší stávající DinnerRepository třídy. Extrahovat toto rozhraní pomocí VS, jednoduše umístěte kurzor do textového editoru ve třídě DinnerRepository klikněte pravým tlačítkem myši a zvolte **Refaktorujte -&gt;extrahování rozhraní** příkazu nabídky:

![](enable-automated-unit-testing/_static/image7.png)

To spustí dialogové okno "Extrahování rozhraní" a nám výzvu k zadání názvu rozhraní pro vytváření. Bude ve výchozím nastavení IDinnerRepository a automaticky vybrat všechny veřejné metody na stávající třídě DinnerRepository přidat do rozhraní:

![](enable-automated-unit-testing/_static/image8.png)

Když kliknete na tlačítko "ok", Visual Studio přidá nové rozhraní IDinnerRepository do naší aplikace:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

A naše existující třídu DinnerRepository aktualizuje tak, že implementuje rozhraní:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a>Aktualizuje se DinnersController pro podporu vkládání konstruktor

Nyní aktualizujeme třídu DinnersController nové rozhraní.

Aktuálně DinnersController je pevně zakódováno, tak, aby jeho "dinnerRepository" pole je vždy DinnerRepository třídy:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

Tak, aby pole "dinnerRepository" typu IDinnerRepository místo DinnerRepository ho Změníme. Potom přidáme dvě veřejné konstruktory DinnersController. Jeden z konstruktorů umožňuje IDinnerRepository mají být předány jako argument. Druhé je výchozí konstruktor, který využívá naše stávající implementaci DinnerRepository:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

Vzhledem k tomu, že technologie ASP.NET MVC ve výchozím nastavení vytvoří třídu kontroleru pomocí výchozí konstruktory, naše DinnersController za běhu bude nadále používat třídu DinnerRepository přístup k datům.

Naše testy jednotek, ale jsme teď můžete aktualizovat a zajistěte tak předání na "falešnou" dinner implementace úložiště pomocí parametru konstruktoru. Toto úložiště "falešnou" dinner nebude vyžadovat přístup ke skutečné databázi a místo toho použije v paměti ukázková data.

#### <a name="creating-the-fakedinnerrepository-class"></a>Vytvoření třídy FakeDinnerRepository

Pojďme vytvořit třídu FakeDinnerRepository.

Vytvoříme nejprve vytvořit adresář "Fakes" v rámci naší NerdDinner.Tests projektu a potom k němu přidejte novou třídu FakeDinnerRepository (klikněte pravým tlačítkem na složku a zvolte **Add -&gt;novou třídu**):

![](enable-automated-unit-testing/_static/image9.png)

Aktualizujeme kód tak, aby FakeDinnerRepository třída implementuje rozhraní IDinnerRepository. Pak můžeme klepněte na něj pravým tlačítkem myši a zvolte příkaz "Implementovat rozhraní IDinnerRepository" kontextové nabídky:

![](enable-automated-unit-testing/_static/image10.png)

To způsobí, že Visual Studio automaticky přidat všechny členy rozhraní IDinnerRepository do našich FakeDinnerRepository třídy s výchozí implementace "zástupných procedur":

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

Společnost Microsoft může aktualizovat také FakeDinnerRepository implementace pro práci z přehledu v paměti&lt;Dinner&gt; kolekce do něho předaný jako argument konstruktoru:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

Teď máme falešné IDinnerRepository implementace, která nevyžaduje databázi a místo toho můžete pracovat vypnout v paměti seznam objektů Dinner.

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a>FakeDinnerRepository pomocí testů jednotek

Vraťme se k DinnersController jednotkové testy, které se dříve nezdařila, protože databáze nebyla dostupná. Abychom mohli aktualizovat testovací metody používat FakeDinnerRepository vyplní ukázkovými daty večeře v paměti do DinnersController pomocí níže uvedeného kódu:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

A teď jsme spuštění těchto testů, oba předejte:

![](enable-automated-unit-testing/_static/image11.png)

Nejlepší na tom budou trvat jenom zlomek sekundy ke spuštění a nevyžadují žádnou logiku složité instalační/čistící. Můžeme teď testování částí, všechny naše DinnersController akce kód metody (včetně výpis, stránkování, podrobností, vytvářet, aktualizovat a odstraňovat) bez přichystanou pro připojení ke skutečné databázi.

| **Na straně téma: Rozhraní injektáž závislostí** |
| --- |
| Provádí vkládání závislostí ruční (podobně jako jsme výše) funguje správně, ale budou obtížnější spravovat jako Počet závislostí a zvyšuje komponenty v aplikaci. Několik architektur injektáž závislostí existovat pro .NET, která poskytuje ještě větší flexibilita díky správy závislostí. Tyto architektury, také někdy označuje jako "inverzi z" (Inversion) – kontejnery ovládacích prvků, poskytují mechanismy, které umožňují další úroveň podporu konfigurace pro zadávání a předávání objektů za běhu (nejčastěji pomocí konstruktoru injektáž závislostí ). Některé další oblíbené injektáž závislostí OSS / include IOC rozhraní v rozhraní .NET: AutoFac Ninject, Spring.NET, StructureMap a Windsor. ASP.NET MVC poskytuje rozhraní API, který vývojářům umožňuje účastnit rozlišení a vytváření instancí kontrolerů a který umožňuje injektáž závislostí pro rozšíření / rozhraní IoC čistě integraci v rámci tohoto procesu. Pomocí rozhraní DI/IOC by také nám umožňují odebrání naše DinnersController – které by úplně odebrat párování mezi ním a DinnerRepositorys výchozí konstruktor. Nebudeme používat injektáž závislostí / framework IOC s naší aplikace NerdDinner. Ale to je něco, co jsme zvažte v budoucnosti Pokud zvětšoval NerdDinner základ kódu a možnosti. |

### <a name="creating-edit-action-unit-tests"></a>Vytváření testů jednotek akce úpravy

Nyní vytvoříme některé testy jednotek, které ověřují funkce DinnersController pro úpravy. Začneme testovací verze HTTP GET naše akce úpravy:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

Test, který ověřuje, že zobrazení se opírá o DinnerFormViewModel objekt je vykreslen zpět, když je požadováno platné dinner vytvoříme:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

Když spustíme testu, ale jsme zjistíte, že se to nezdaří, protože výjimka nulového odkazu je vyvolána, když metoda úprav přistupuje k vlastnosti User.Identity.Name provádět kontrolu Dinner.IsHostedBy().

Objekt uživatele na základní třídu Kontroleru zapouzdřuje informace o přihlášeného uživatele a je vyplněn ASP.NET MVC, když vytváří kontroleru za běhu. Protože se testují DinnersController mimo prostředí webového serveru, uživatele objekt není nastavený (tedy odkazem s hodnotou null výjimka).

### <a name="mocking-the-useridentityname-property"></a>Napodobování vlastnost User.Identity.Name

Napodobování architektury provést testování usnadnit tím, že nám dynamicky se vytvářejí falešné verze závislé objekty, které podporují testech. Například můžete použít napodobování framework v testovacím akce Upravit dynamicky se vytvářejí objekt uživatele, naše DinnersController můžete použít k vyhledání simulovaný uživatelské jméno. Tím se vyhnete odkaz s hodnotou null z je vyvolána, když jsme naše testu.

Existuje mnoho .NET napodobování architektur, které lze použít s ASP.NET MVC (zobrazí se seznam je tady: [ http://www.mockframeworks.com/ ](http://www.mockframeworks.com/)). Testování aplikace NerdDinner použijeme open source napodobování rámec se nazývá "Moq", který si můžete stáhnout zdarma z [ http://www.mockframeworks.com/moq ](http://www.mockframeworks.com/moq).

Po stažení, přidáme v našem NerdDinner.Tests projektu odkaz na sestavení Moq.dll:

![](enable-automated-unit-testing/_static/image12.png)

Potom přidáme Pomocná metoda "CreateDinnersControllerAs(username)" pro naše testovací třídy, který přebírá uživatelské jméno jako parametr a které pak "mocks" User.Identity.Name vlastnost u DinnersController instance:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

Moq výše se používá k vytvoření objektu model, který napodobenin objekt parametrem ControllerContext (což je technologie ASP.NET MVC předá do třídy Kontroleru ke zveřejnění objekty modulu runtime jako uživatel, požadavku a odpovědi a relace). Metoda "SetupGet" voláme na model k označení, že vlastnost HttpContext.User.Identity.Name parametrem ControllerContext by měla vrátit řetězec uživatelské jméno, které jsme předaný metodě pomocné rutiny.

Společnost Microsoft může napodobení libovolný počet parametrem ControllerContext vlastnosti a metody. Pro ilustraci, to jsme taky přidali SetupGet() volání pro vlastnost Request.IsAuthenticated (což není skutečně potřeba pro testy níže – ale což omezuje ukazují, jak můžete napodobení vlastnosti požadavku). Když jsme se vším hotovi jsme přiřadit instanci cvičné parametrem ControllerContext DinnersController naše Pomocná metoda vrátí.

Můžeme se teď dá zapisovat jednotkové testy používající tuto metodu helper otestovat úpravy scénáře zahrnující různých uživatelů:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

A teď, když spustíme testy se předají:

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a>Scénáře testování UpdateModel()

Vytvořili jsme testy, které se týkají verze HTTP GET akce úpravy. Nyní vytvoříme některé testy, které ověřují verze HTTP POST akci Upravit:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

Zajímavé nové testovací scénář pro nás pro podporu s touto metodou akce je jeho použití UpdateModel() pomocnou metodu v základní třídě Controller. Tato pomocná metoda se používá k vytvoření vazby odeslaného formuláře naše společnost Dinner instanci objektu.

Níže jsou dva testy, které ukazuje, jak můžeme zadat formuláře, pošle hodnoty UpdateModel() pomocné metody pro použití. Jsme budete k tomu vytváří se a naplňuje objekt FormCollection a pak ji přiřaďte vlastnost "Položka ValueProvider" v řadiči.

První test ověří, že při úspěšném uložení prohlížeč přesměrován na podrobnosti o akci. Druhý test ověří, že když se pošle neplatný vstup akce znovu zobrazí zobrazení pro úpravy znovu s chybovou zprávou.


[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a>Testování Wrap-Up

Věnovali jsme základní koncepty zapojené do třídy kontroleru testování jednotek. Můžeme použít tyto postupy můžete snadno vytvořit stovky jednoduchých testů, které ověří chování naši aplikaci.

Protože naše kontroléru a testovacích modelu nevyžadují skutečná databáze, jsou extrémně rychlé a snadné spuštění. Budeme moct spustit stovky automatizované testy během několika sekund a okamžitě získat zpětnou vazbu o tom, zda změny, které jsme udělali se podařilo přerušit něco. To vám pomůže nám poskytnout jistotu potřebnou pro průběžně vylepšovat, refaktorace a upřesňovat naši aplikaci.

Jsme probrali testování jako poslední téma v této kapitole – ale ne vzhledem k tomu testování se něco by měl provádět na konci procesu vývoje! Naopak měli byste psát automatizované testy co nejdříve do vašeho vývojového procesu. To uděláte tak umožňuje načíst okamžitou zpětnou vazbu při vývoji, pomáhá vytvoříte promyšlené uvažovat o scénáře použití vaší aplikace a provede vás k návrhu vaší aplikace pomocí čisté vrstvení a párování v úvahu.

Novější kapitolu v knize se o testu řízeného vývoje (TDD) a jak ji používat s architekturou ASP.NET MVC. TDD je iterativní způsobem kódování, kde první psát testy, které se bude splňovat vaše výsledný kód. S TDD začnete tak, že vytvoříte test, který ověří funkčnost, kterou se chystáte implementace jednotlivých funkcí. Zápis jednotka testu nejprve pomáhá zajistit jasně porozumět funkci a jak by mělo fungovat. Pouze poté, co se zapíše testu (a si ověříte, že rutina selže) se pak implementovat funkci skutečný test ověří. Vzhledem k tomu, že jste již stráveného času přemýšlení o případ použití, z jak funkci má práce, budete mít lepší přehled požadavků a nejlepší způsob pro jejich implementaci. Jakmile budete hotovi s prováděním můžete znovu spustit test – a získat okamžitou zpětnou vazbu na tom, jestli funkce funguje správně. Probereme TDD informace najdete v kapitole 10.

### <a name="next-step"></a>Dalším krokem

Některé konečné zalamování řádků do komentáře.

> [!div class="step-by-step"]
> [Předchozí](use-ajax-to-implement-mapping-scenarios.md)
> [další](nerddinner-wrap-up.md)
