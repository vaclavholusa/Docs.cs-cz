---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: "Povolit automatizované testování částí | Microsoft Docs"
author: microsoft
description: "Krok 12 ukazuje, jak vyvíjet sada testů jednotek automatizované, ověřte funkčnost naše NerdDinner a který nám umožní spolehlivosti provést změny..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: 1a4258054d90b2d5bcc06a63fb6f3b4673a4837d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="enable-automated-unit-testing"></a>Povolit automatické testování částí
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Toto je kroku 12 bezplatný [kurz aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , nevystavíte slabé stránky zabezpečení – prostřednictvím postup sestavení malá, ale dokončení, webové aplikace pomocí ASP.NET MVC 1.
> 
> Krok 12 ukazuje, jak vyvíjet sada testů jednotek automatizované, ověřte funkčnost naše NerdDinner a které nám získáte jistotu provést změny a vylepšení aplikace v budoucnu.
> 
> Pokud používáte ASP.NET MVC 3, doporučujeme provedením [získávání spuštěna s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Hudba úložiště](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.


## <a name="nerddinner-step-12-unit-testing"></a>NerdDinner krok 12: Testování částí

Umožňuje vyvíjet sada testů jednotek automatizované, ověřte funkčnost naše NerdDinner a které nám získáte jistotu provést změny a vylepšení aplikace v budoucnu.

### <a name="why-unit-test"></a>Proč jednotkové testování?

Na jednotce do pracovní jeden ráno máte nečekané flash inspiraci o aplikaci, kterou právě pracujete. Zjistíte, že dojde ke změně, které můžete implementovat, který bude aplikace výrazně lepší. Může to být refaktoring, vyčistí kód, přidává nové funkce nebo opravy chyby.

Otázku, kterou jste confronts až přijedete do vašeho počítače je – "bezpečného je abyste se tomuto vylepšení?" Co v případě provedení změny má vedlejší účinky nebo dělí něco? Změny mohou být jednoduchý a pouze trvat několik minut implementovat, ale co když trvá hodiny ručně otestování všech scénářích aplikace? Co když zapomenete tak, aby pokrýval scénáře a zprovozněním porušený aplikace? Toto vylepšení skutečně vhodné všechny úsilí je zajistit?

Testování částí automatizované můžete zadat bezpečnostní opatření, která umožňuje průběžně vylepšit vaše aplikace a vyhnout se Bojíte kód, který pracujete na. S automatizovaných testů, které rychle ověřte, zda funkce umožňuje kód s jistotou – a umožnit vám umožní provádět vylepšení můžete nemusí jinak mít popisovač možnost provádění. Také pomáhají vytvářet řešení, která výkonnější a delší doba platnosti – které vede k mnohem vyšší návratnost investic.

Rozhraní ASP.NET MVC umožňuje snadno a přirozené funkce jednotky testovací aplikace. Umožňuje také pracovní postup testování řízené vývoj (TDD), který umožňuje na základě vývoj včasného testování.

### <a name="nerddinnertests-project"></a>NerdDinner.Tests projektu

Pokud jsme vytvořili aplikaci NerdDinner na začátku tohoto kurzu, byly zobrazí dialog s dotazem, zda jsme chtěli vytvoření projektu testování částí přejdete společně s projekt aplikace:

![](enable-automated-unit-testing/_static/image1.png)

Jsme zachovány "Ano, vytvoření projektu testování částí" vybraný přepínač – následkem "NerdDinner.Tests" projektu, který se přidává do naše řešení:

![](enable-automated-unit-testing/_static/image2.png)

NerdDinner.Tests projekt odkazuje na sestavení projektu NerdDinner aplikace a umožňuje nám snadno přidat k němu, který ověřte funkčnost aplikace automatizovaných testů.

### <a name="creating-unit-tests-for-our-dinner-model-class"></a>Testování částí pro naše večeři třídy modelu

Umožňuje přidat některé testy pro naše NerdDinner.Tests projekt, který ověřte večeři třídy, na kterou jsme vytvořili, když jsme vytvořili naše vrstva modelu.

Začneme vytvořením novou složku v našem testu projekt s názvem "Modely" kam budete být naše testy související modelu. Jsme budete klikněte pravým tlačítkem na složku a vyberte **Add -&gt;nový Test** příkazu nabídky. Otevře se dialog "Přidat nový Test".

Jsme vybrali "Testování částí" Vytvoření a pojmenujte ji "DinnerTest.cs":

![](enable-automated-unit-testing/_static/image3.png)

Když kliknete na tlačítko "ok" sady Visual Studio bude přidat (a otevřete) soubor DinnerTest.cs do projektu:

![](enable-automated-unit-testing/_static/image4.png)

Výchozí šablony testu jednotek sady Visual Studio má spoustu kotle desku kódu v něm, které lze najít trochu komplikované. Umožňuje vyčistit ji tak, aby obsahovala pouze následující kód:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

Atribut [TestClass] k třídě DinnerTest výše ji identifikuje jako třída, která bude obsahovat testy, jakož i volitelné testovací inicializace a rušením kódu. Přidáním veřejné metody, které mají atribut [TestMethod] na nich jsme můžete definovat testy v něm.

V následující tabulce jsou první dva testy, které přidáme, které vykonávají Naše třída večeři. První test ověřuje, že naše večeři je neplatný, pokud je vytvořen nový večeři bez správně nastavené všechny vlastnosti. Druhý test ověřuje, že je naše večeři platný při večeři má všechny vlastnosti s platné hodnoty:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

Můžete si všimnout výše, že naše testovací názvy jsou velmi explicitní (a poněkud verbose). Jsme se to, protože můžeme vytváření stovek nebo tisíců malé testů a chceme, abyste usnadnili snadnou rychle zjistit záměr a každý z nich chování (obzvláště pokud Těšíme se v seznamu chyb v test runner). Názvy test by měl být pojmenován po funkce, které budou testování. Vyšší používáme "podstatné jméno\_by měl\_příkaz" vzoru pro pojmenovávání.

Nemůžeme se strukturování testy pomocí "AAA" testování vzor – který zastupuje "Uspořádat, Application Compatibility Toolkit, Assert":

- Uspořádat: Nastavení jednotky během testování
- AKT: Prověření jednotky testovaného a zachycení výsledky
- Assert –: Ověřte, že chování

Při psaní příliš mnoho provést testy chceme, abyste nemuseli jednotlivé testy. Místo toho každý test by měl ověřit jenom jeden koncept (což znamená, že výrazně usnadňují přesnou příčinu selhání). Vhodné je a zkuste to mít pouze jeden assert příkaz pro každý test. Pokud máte více než jeden příkaz v testovací metody assert, ujistěte se, že se všechny se používají k testování stejný koncept. Pokud máte pochybnosti, ujistěte se, jiného testu.

### <a name="running-tests"></a>Spouštění testů

Visual Studio 2008 Professional (a vyšších edicích) zahrnuje integrovanou testu, který lze použít pro spouštění projektů Visual Studio jednotkové testování v prostředí IDE. Můžete vybrat jsme **Test -&gt;spustit -&gt;všechny testy v řešení** nabídky příkazu (nebo typu R Ctrl a A) chcete spustit všechny naše testy jednotek. Nebo můžete také jsme můžete umístit naše kurzor v rámci konkrétní test třídu nebo testovací metoda a použít **Test -&gt;spustit -&gt;testy v aktuálním kontextu** příkazu nabídky (nebo typu Ctrl R T) ke spuštění podmnožinu testování částí.

Umožňuje umístit naše kurzor v rámci třídy DinnerTest a zadat "Ctrl R, T" spustit dva testy, které jsme definovali. Když jsme to "Výsledky testu" okno se zobrazí v sadě Visual Studio a ukážeme výsledky našich testů spustit uvedené v něm:

![](enable-automated-unit-testing/_static/image5.png)

*Poznámka: Okno výsledků testu VS nezobrazuje sloupci Název třídy ve výchozím nastavení. Můžete přidat pravým tlačítkem myši v okně výsledků testů a použitím příkazu nabídky Přidat nebo odebrat sloupce.*

Naše dva testy trvalo pouze část druhý spustit – a jak jste může najdete oba prošli. Jsme teď můžete přejít a posílení je tak, že vytvoříte další testy, které ověřte ověření konkrétní pravidlo, jakož i zahrnují dva pomocné metody - IsUserHost() a IsUserRegisterd() – které jsme přidali k třídě večeři. Tyto testy s nastavené pro třídu večeři znamená, že mnohem snazší a bezpečnější přidat nová obchodní pravidla a ověření k němu v budoucnu. Jsme můžete přidat nové pravidlo logiku na večeři a pak během několika sekund ověřte, že ho nebyl přerušený některé z našich předchozí funkce logiku.

Všimněte si, jak pomocí testu popisný název usnadňuje rychle pochopit, co každý test ověření. I doporučujeme používat **nástroje -&gt;možnosti** příkaz nabídky, otevřete testování nástroje -&gt;konfigurační obrazovce pro provádění testů a kontrola, zda "dvakrát klikněte na výsledek testu se nezdařila nebo neprůkazné jednotka zobrazí zaškrtávací políčko bodem selhání v testu". To vám umožní dvakrát klikněte na selhání v okně Výsledky testu a přejít na selhání assert okamžitě.

### <a name="creating-dinnerscontroller-unit-tests"></a>Testování částí DinnersController

Nyní vytvoříme některé testy jednotek, které ověřte funkčnost naše DinnersController. Jsme budete začněte tím, že pravým tlačítkem na složku "Řadiče" v rámci naší testovacího projektu a potom zvolte **Add -&gt;nový Test** příkazu nabídky. Můžeme vytvářet "Testování částí" a pojmenujte ji "DinnersControllerTest.cs".

Vytvoříme dvě testovací metody, které ověřte Details() metody akce na DinnersController. První ověří, jestli zobrazení je vrácena, pokud se požaduje existující večeři. Druhý ověří, že "NotFound" zobrazení je vrácena, pokud se požaduje neexistující večeři:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

Výše uvedený kód zkompiloval vyčistit. Když jsme spustit testy, ale obě nepovede:

![](enable-automated-unit-testing/_static/image6.png)

Pokud se podíváme na chybové zprávy, uvidíme, že testy selhání byl, protože naše třída DinnersRepository se nemohl připojit k databázi. Naše NerdDinner aplikace používá připojovací řetězec do místního souboru systému SQL Server Express, který je umístěn v části \App\_datový adresář projektu NerdDinner aplikace. Protože naše NerdDinner.Tests projektu zkompiluje a spustí v jiném adresáři pak projekt aplikace, relativní cesta umístění naše připojovací řetězec je nesprávný.

Jsme *může* opravit to tak, že zkopírujete soubor databáze SQL Express na našem testovacího projektu a potom k němu přidejte připojovacího řetězce odpovídající test v souboru App.config naše testovacího projektu. To by získat výše uvedenými testy odblokováno a spuštěna.

Testování kódu pomocí databázi skutečné částí ale přináší s ním určité problémy. Konkrétně:

- Se výrazně zpomalí čas provádění testů jednotek. Tím déle trvá ke spuštění testů, tím méně pravděpodobné, že jste je často spustit. V ideálním případě má testů jednotek být schopni spustit v sekundách – a mějte ho se něco, co můžete udělat jako přirozeně jako kompilace projektu.
- Složitá případné instalační program a čištění logiku v rámci testů. Chcete, aby každý testování částí izolované a nezávisle na ostatních (bez vedlejší účinky nebo závislosti). Při práci na skutečné databázi, budete muset mějte na paměti stavu a v něm obnovit mezi testy.

Podívejme se na návrhový vzor názvem "vkládání závislostí", který vám pomůže nám vyřešit tyto problémy a nemuseli použít skutečné databázi pomocí našich testů.

### <a name="dependency-injection"></a>Vkládání závislostí

Nyní DinnersController je úzce "spojeny" k třídě DinnerRepository. "Spojovacích" odkazuje na situaci, kdy třídu explicitně závisí na jiné třídy fungování:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

Protože třída DinnerRepository vyžaduje přístup k databázi, úzce párované závislost DinnersController třída nemá na koncích DinnerRepository až nutnosti nám máte databázi v pořadí pro DinnersController metody akce má být testována.

Tento problém jsme můžete získat díky využití architektury návrhový vzor názvem "vkládání závislostí" – což je přístup, kde jsou závislosti (jako jsou třídy úložiště, které poskytují přístup k datům) vytvořen již implicitně v rámci třídy, které je používají. Místo toho závislosti lze explicitně předat do třídy, která používá pomocí argumenty konstruktoru. Zda závislosti jsou definovány pomocí rozhraní, pak je k dispozici možnost předat implementace "falešných" závislosti pro scénáře testování částí. To umožňuje vytvořit testovací konkrétní závislost implementací, které ve skutečnosti nevyžadují přístup k databázi.

Zobrazení tohoto v akci, můžeme implementovat vkládání závislostí s naše DinnersController.

#### <a name="extracting-an-idinnerrepository-interface"></a>Extrahování rozhraní IDinnerRepository

Chcete-li vytvořit nové rozhraní IDinnerRepository, který zapouzdřuje kontrakt úložiště, které vyžadují naše řadiče se načítají a aktualizují večeří bude našem prvním krokem.

Jsme definovali tento kontrakt rozhraní ručně tak, že kliknete pravým tlačítkem na složku, \Models a pak vyberete **Add -&gt;nová položka** příkazu v nabídce a vytvoření nové rozhraní s názvem IDinnerRepository.cs.

Případně jsme použít extract refaktoring nástroje zabudovaná Visual Studio Professional (a vyšších edicích) chcete automaticky a vytvořit rozhraní pro nás z našich existující třídy DinnerRepository. K extrakci toto rozhraní pomocí VS, jednoduše umístěte kurzor v textovém editoru k třídě DinnerRepository a potom klikněte pravým tlačítkem a zvolte **Refaktorovat -&gt;extrahování rozhraní** příkazu v nabídce:

![](enable-automated-unit-testing/_static/image7.png)

Tato akce spustit dialogové okno "Extrahování rozhraní" a vyzvat nám název rozhraní pro vytváření. Bude použita výchozí IDinnerRepository a automaticky vybrat všechny veřejné metody na stávající třídě DinnerRepository pro přidání do rozhraní:

![](enable-automated-unit-testing/_static/image8.png)

Když kliknete na tlačítko "ok", Visual Studio přidá nové rozhraní IDinnerRepository do naší aplikaci:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

A naše existující třídy DinnerRepository budou aktualizovány tak, aby implementuje rozhraní:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a>Aktualizace DinnersController k podpoře vkládání – konstruktor

Nyní jsme budete aktualizovat třídě DinnersController používat nové rozhraní.

Aktuálně DinnersController je pevně tak, aby jeho pole "dinnerRepository" je vždy DinnerRepository třída:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

Změníme ho tak, aby pole "dinnerRepository" typu IDinnerRepository místo DinnerRepository. Potom přidáme dvě veřejné konstruktory DinnersController. Jeden z konstruktorů umožňuje IDinnerRepository mají být předány jako argument. Druhá je výchozí konstruktor, který používá naše stávající implementaci DinnerRepository:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

Protože rozhraní ASP.NET MVC ve výchozím nastavení vytvoří třídy kontroleru pomocí výchozí konstruktory, naše DinnersController za běhu bude nadále používat třídu DinnerRepository provádí přístup k datům.

Naše testy jednotek, ale jsme teď můžete aktualizovat předávání v implementaci "falešných" večeři úložiště pomocí parametr konstruktoru. Toto úložiště "falešných" večeři nebude vyžadovat přístup k databázi skutečné a místo toho bude používat v paměti ukázková data.

#### <a name="creating-the-fakedinnerrepository-class"></a>Vytvoření třídy FakeDinnerRepository

Umožňuje vytvořit třídu FakeDinnerRepository.

Jsme budete začněte vytvořením "Fakes" adresáře v rámci naší NerdDinner.Tests projektu a potom k němu přidejte novou třídu FakeDinnerRepository (klikněte pravým tlačítkem na složku a zvolte **Add -&gt;novou třídu**):

![](enable-automated-unit-testing/_static/image9.png)

Kód jsme budete aktualizovat tak, aby FakeDinnerRepository třída implementuje rozhraní IDinnerRepository. Pak můžeme klikněte na něj pravým tlačítkem myši a zvolte příkaz "Implementovat rozhraní IDinnerRepository" kontextové nabídky:

![](enable-automated-unit-testing/_static/image10.png)

To způsobí, že Visual Studio automaticky přidat všechny členy rozhraní IDinnerRepository Naše třída FakeDinnerRepository s výchozí implementace "zóny se zakázaným inzerováním":

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

Můžete aktualizujte implementace FakeDinnerRepository pracovat z seznam aplikace v paměti&lt;večeři&gt; kolekce do ní předán jako argument konstruktoru:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

Nyní je k dispozici falešných implementace IDinnerRepository, který nevyžaduje databázi a místo toho můžete pracovat vypnout jenom v paměti seznam objektů večeři.

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a>Pomocí FakeDinnerRepository testování částí

Pojďme se vraťte k testování částí DinnersController, které se dříve nezdařila, protože databáze nebyla k dispozici. Aktualizujeme zkušební metody sloužící FakeDinnerRepository naplněný ukázková data večeři v paměti DinnersController pomocí následující kód:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

A teď Pokud spustíme tyto testy obě předat:

![](enable-automated-unit-testing/_static/image11.png)

Navíc se trvat jenom zlomek druhý ke spuštění a nevyžaduje žádné složité nastavení nebo čištění logiku. Můžeme teď jednotkové testování všechny kódu metoda akce DinnersController (včetně výpis, stránkování, podrobnosti, vytvářet, aktualizovat a odstraňovat) nemusí někdy se připojit k databázi skutečné.

| **Straně tématu: Rozhraní vkládání závislostí** |
| --- |
| Provádění vkládání závislostí ruční (jako jsou jsme výše) funguje bez problémů, ale stát těžší udržovat jako Počet závislostí a zvyšuje komponenty v aplikaci. Pro rozhraní .NET, který vám pomůže i závislosti flexibilitu správu neexistují několik rozhraní vkládání závislostí. Tyto architektury, také někdy označuje jako "inverzi z" (IoC) – kontejnery ovládacích prvků, poskytují mechanismy, které povolíte vyšší úroveň konfigurace podpory pro určení a předání závislosti objektů za běhu (nejčastěji pomocí vkládání – konstruktor ). Některé oblíbenější vkládání závislostí OSS / include IOC rozhraní v rozhraní .NET: AutoFac, Ninject, Spring.NET, StructureMap a Windsor. ASP.NET MVC zpřístupňuje rozhraní API, které umožňují vývojářům účastnit překlad IP adres a vytváření instancí řadičů a která umožňuje vkládání závislostí rozšíření / IoC architektury řádně integraci v rámci tohoto procesu. Pomocí rozhraní DI/IOC by také povolit nám odebrat z našich DinnersController – které by úplně odebrat párování mezi nimi a DinnerRepositorys výchozí konstruktor. Jsme nebude pomocí vkládání závislostí nebo IOC framework s naší NerdDinner aplikace. Ale je něco, co jsme zvažte pro budoucnost Pokud vzrostla NerdDinner-základu kódu a možnosti. |

### <a name="creating-edit-action-unit-tests"></a>Testování částí upravit akce

Nyní vytvoříme některé testy jednotek, které ověřují upravit funkce DinnersController. Začneme testování HTTP GET verzi naše upravit akce:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

Vytvoříme test, který ověřuje, že zobrazení založenou na objekt DinnerFormViewModel je vykreslen zpět v případě, že je požadovaná platné večeři:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

Při spuštění testu, když jsme tu se nezdaří, protože výjimka odkazu s hodnotou null se vyvolá, když metoda úprav přistupuje k vlastnosti User.Identity.Name k provedení kontroly Dinner.IsHostedBy().

Objekt uživatele na základní třídy Kontroleru zapouzdřuje informace o přihlášeného uživatele a nebude naplněn sadou rozhraní ASP.NET MVC, když vytváří řadičem za běhu. Protože se testují DinnersController mimo prostředí s webovým serverem, není nastaven objekt uživatele (proto hodnotu null referenční výjimky).

### <a name="mocking-the-useridentityname-property"></a>Mocking vlastnost User.Identity.Name

Mocking architektury proveďte testování usnadnit tím, že umožňuje nám vytvořit dynamicky falešných verzích závislé objekty, které podporují naše testy. Například můžete použít mocking framework v našem testu akce úpravy dynamicky vytvořit objekt uživatele, který naše DinnersController můžete použít k vyhledání simulované uživatelské jméno. Vyhnete se odkaz s hodnotou null z se vyvolá, když jsme spuštění našich testů.

Existuje mnoho .NET mocking rozhraní, které lze použít s architekturou ASP.NET MVC (zobrazí se seznam z nich zde: [http://www.mockframeworks.com/](http://www.mockframeworks.com/)). Pro testování aplikace NerdDinner použijeme typu open source mocking framework názvem "Moq", který si můžete stáhnout zdarma z [http://www.mockframeworks.com/moq](http://www.mockframeworks.com/moq).

Po stažení, přidáme v našem NerdDinner.Tests projektu odkaz na sestavení Moq.dll:

![](enable-automated-unit-testing/_static/image12.png)

Potom přidáme pomocnou metodu "CreateDinnersControllerAs(username)" do našich testů třídu, která přebírá uživatelské jméno jako parametr a které pak "mocks" Vlastnost User.Identity.Name na instanci DinnersController:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

Moq vyšší se používá k vytvoření objektu model, který fakes objekt parametrem ControllerContext (což je rozhraní ASP.NET MVC předá do třídy Kontroleru vystavit runtime objekty jako uživatel, požadavku a odpovědi a relace). Ve model k označení, že vlastnost HttpContext.User.Identity.Name na parametrem ControllerContext musí vracet uživatelské jméno řetězec, který jsme předaný pomocnou metodu jsme se volání metody "SetupGet".

Jsme můžete model libovolný počet parametrem ControllerContext vlastnosti a metody. Pro znázornění I taky přidali SetupGet() volání pro vlastnost Request.IsAuthenticated (který není ve skutečnosti nutný pro testy níže – ale který pomáhá ilustrují, jak můžete model vlastnosti požadavku). Když jsme se provádějí jsme přiřadit instanci parametrem ControllerContext model DinnersController naše Pomocná metoda vrátí.

Jsme teď může zapisovat testy jednotek, které používají tuto metodu helper k otestování scénářů upravit zahrnující různé uživatele:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

A teď když jsme testy předají:

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a>Testování UpdateModel() scénáře

Vytvořili jsme testy, které se týkají verze HTTP GET akce úpravy. Nyní vytvoříme některé testy, které ověřit verzi upravit akce HTTP POST:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

Zajímavé nové testování scénář, abychom mohli podporovat této metodě akce je jeho využití pomocnou metodu UpdateModel() na základní třídy Kontroleru. Tato pomocná metoda se používá k vytvoření vazby vystavení formuláře naše večeři instance objektu.

Níže jsou dva testy, které ukazuje, jak můžeme zadat formuláře Odeslat hodnoty pro pomocnou metodu UpdateModel() používat. Jsme vám to udělat pomocí vytvoření a naplnění objekt FormCollection a přiřaďte ho pro vlastnost "ValueProvider" v řadiči.

První test ověřuje, že na úspěšné uložit v prohlížeči přesměrován na akce. Druhý test ověřuje, že při odeslání neplatný vstup akci znovu zobrazí zobrazení úpravy znovu s chybovou zprávou.


[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a>Testování Wrap-Up

Jsme jste zahrnutých účastnící se testování třídy řadiče jednotky základní koncepty. Tyto postupy jsme vám pomůže snadno vytvořit stovky jednoduchých testů, které ověřují chování aplikace.

Protože naše řadiče a testy modelu nevyžadují skutečné databáze, jsou velmi rychlé a snadné spuštění. Jsme budete moci provést stovky automatizovaných testů v sekundách a okamžitě získávat zpětnou vazbu, zda změny, které jsme provedli překročila něco. To vám pomůže poskytnout nám průběžně zlepšit, refactor, a upřesněte naší aplikace.

Jsme zahrnutých testování jako poslední téma v této kapitole – ale ne kvůli tomu testování je něco dělat na konci procesu vývoje! Naopak byste měli zapsat automatizovaných testů co nejdříve ve vývojových procesech. To proto umožňuje získat okamžitou zpětnou vazbu když budete vyvíjet, pomáhá vytvoříte promyšlené myslíte o scénářích případů použití vaší aplikace a provede vás k návrhu aplikace s čistou rozvrstvení a spojovacích pamatovat.

Novější kapitoly v knize zabývat testovací řízené vývoj (TDD) a způsobu jeho použití s architekturou ASP.NET MVC. TDD iterativní kódování postupem je, kde můžete psát nejprve testy, které budou splňovat vaše výsledný kód. S TDD začnete každé funkce tak, že vytvoříte test, který ověřuje funkce, které se chystáte implementovat. Zápis jednotka testovací nejprve pomáhá zajistit, že rozumíte jasně funkci a jak by měla fungovat. Pouze po testu je zapsán (a ověříte, že se nezdaří) můžete udělat pak implementovat ověřuje funkce skutečného test. Protože jste již stráví čas přemýšlení o tom, jak by měla fungovat funkce v případě použití, budete mít lépe pochopit požadavky a osvědčené jak pro jejich implementaci. Až skončíte s implementací můžete znovu spustit test – a získat okamžitou zpětnou vazbu jako na tom, jestli funkce funguje správně. Jsme budete zahrnovat TDD více v kapitole 10.

### <a name="next-step"></a>Další krok

Některé konečné wrap až komentáře.

>[!div class="step-by-step"]
[Předchozí](use-ajax-to-implement-mapping-scenarios.md)
[další](nerddinner-wrap-up.md)
