---
uid: mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-vb
title: 'Iterace #4 – vytvoření volně spárované aplikace (VB) | Dokumentace Microsoftu'
author: microsoft
description: V této iterace čtvrtý můžeme využít několik způsobů návrhu v softwaru k bylo snazší spravovat a upravovat aplikace Správce kontaktů. Pro...
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: 92c70297-4430-4e4e-919a-9c2333a8d09a
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-vb
msc.type: authoredcontent
ms.openlocfilehash: 68e7ebf987d03139f63ae9b06a712366a9679bc3
ms.sourcegitcommit: a25b572eaed21791230c85416f449f66a405ec19
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/01/2018
ms.locfileid: "39395958"
---
<a name="iteration-4--make-the-application-loosely-coupled-vb"></a>Iterace #4 – vytvoření volně spárované aplikace (VB)
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout kód](iteration-4-make-the-application-loosely-coupled-vb/_static/contactmanager_4_vb1.zip)

> V této iterace čtvrtý můžeme využít několik způsobů návrhu v softwaru k bylo snazší spravovat a upravovat aplikace Správce kontaktů. Například Refaktorovat jsme naši aplikaci pomocí vzoru úložiště a vzor vkládání závislostí.


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

V tomto čtvrtý iteraci kontaktujte správce aplikace jsme Refaktorovat aplikace, aby byla více volně spárované aplikace. Když je volně spárované aplikace, můžete upravit kód v jedné části aplikace bez nutnosti upravovat kód v ostatních částech aplikace. Volně propojených aplikací jsou odolnější vůči změnit.

V současné době všechny dat přístup a ověřování logikou používanou aplikací správce kontaktů je součástí třídy kontroleru. To není dobrý nápad. Pokaždé, když se budete muset upravit jedné části vaší aplikace, riskujete zavádění chyb do jiné části aplikace. Například při úpravě svoji logiku ověřování, riskujete zavedení nových chyb do logiky data access nebo kontroleru.

> [!NOTE] 
> 
> (SRP), třída by měla mít nikdy více než jedním z důvodů, chcete-li změnit. Kombinování řadič, ověřování a logiku databáze je obrovská porušení principu jednu zodpovědnost.


Tady je několik důvodů, které možná budete muset upravit svou aplikaci. Možná budete muset přidat nové funkce do vaší aplikace, možná budete muset opravit chybu v aplikaci nebo možná budete muset upravit, jak je implementovaná funkce vaší aplikace. Aplikace jsou zřídka statické. Mají tendenci k růstu a mutovat v čase.

Představte si třeba, abyste se rozhodli změnit, jak implementovat vaše vrstvy přístupu k datům. Pravé teď, kontaktujte správce aplikace používá Microsoft Entity Framework pro přístup k databázi. Ale můžete se rozhodnout migrovat do nového nebo alternativní datový přístup technologie, jako je služeb ADO.NET Data Services nebo NHibernate. Ale protože kód přístupu k datům není izolovaná od kód ověřování a kontroler, neexistuje žádný způsob, jak upravit kód přístupu k datům ve vaší aplikaci bez změny jiný kód, který nesouvisí se přímo k přístupu k datům.

Pokud aplikace je volně propojené a na druhé straně můžete provádět změny jedné části aplikace bez zásahu do jiných částí aplikace. Například můžete přepnout technologií přístupu k datům beze změny svoji logiku ověřování nebo kontroleru.

V této iterace můžeme využít několik způsobů návrhu softwaru, které pomáhají k refaktorování naši Správce kontaktů aplikaci do více volně spárované aplikace. Když jsme se vším hotovi, kontaktujte správce vyhráli t dělat nic., který ho nefungoval t proveďte před. Ale budeme moct aplikaci mnohem snazší v budoucnu změnit.

> [!NOTE] 
> 
> Refaktoring je proces přepsání aplikace tak, že neztratí všechny existující funkce.


## <a name="using-the-repository-software-design-pattern"></a>Pomocí vzoru návrhu úložiště softwaru

Naše první změna je využít softwaru návrhový vzor, který volá použitému vzoru. Použijeme vzor úložiště k izolaci náš kód přístupu k datům od zbytku naši aplikaci.

Implementace vzoru úložiště vyžaduje, abychom dokončete následující kroky:

1. Vytvoření rozhraní
2. Vytvořit konkrétní třída, která implementuje rozhraní

Nejprve musíme vytvořit rozhraní, které popisuje všechny metody přístupu data, která potřebujeme k provedení. Rozhraní IContactManagerRepository je obsažen v informacích 1. Popisuje pět metod, toto rozhraní: CreateContact() DeleteContact(), EditContact(), GetContact a ListContacts().

**Výpis 1 - Models\IContactManagerRepository.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample1.vb)]

V dalším kroku budeme muset vytvořit konkrétní třída, která implementuje rozhraní IContactManagerRepository. Protože používáme Microsoft Entity Framework pro přístup k databázi, vytvoříme novou třídu s názvem EntityContactManagerRepository. Tato třída je obsažen v informacích 2.

**Výpis 2 - Models\EntityContactManagerRepository.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample2.vb)]

Všimněte si, že EntityContactManagerRepository třída implementuje rozhraní IContactManagerRepository. Třída implementuje všech pět z metod popsaných v tomto rozhraní.

Může vás zajímat, proč musíme zabývat rozhraní. Proč potřebujeme vytvořit rozhraní a třídy, která ho implementuje?

S jednou výjimkou zbývající část naší aplikace budou používat rozhraní a ne na konkrétní třídu. Namísto volání metody vystavené objektem třídy EntityContactManagerRepository, zavoláme vám metody vystavené objektem IContactManagerRepository rozhraní.

Tímto způsobem můžeme implementovat rozhraní s novou třídu aniž byste museli upravovat zbytek naši aplikaci. Například v některé budoucí datum, budeme chtít implementovat DataServicesContactManagerRepository třídu, která implementuje rozhraní IContactManagerRepository. Třída DataServicesContactManagerRepository může použít datových služeb ADO.NET pro přístup k databázi namísto Microsoft Entity Framework.

Pokud je náš kód aplikace programovat proti rozhraní IContactManagerRepository místo konkrétní třída EntityContactManagerRepository jsme beze změny všechny zbývající část našeho kódu přepnout konkrétní třídy. Například můžeme přepnout ze třídy EntityContactManagerRepository na třídu DataServicesContactManagerRepository beze změny našich dat přístup nebo ověřovací logiku.

Programování v rozhraní (abstrakce) místo konkrétních tříd díky naší aplikace odolnější vůči změnit.

> [!NOTE] 
> 
> Můžete rychle vytvořit rozhraní z konkrétní třídy v sadě Visual Studio tak, že vyberete možnost nabídky Refaktorujte, extrahování rozhraní. Můžete například nejprve vytvořit třídu EntityContactManagerRepository a pak použijte extrahování rozhraní k automatickému vygenerování IContactManagerRepository rozhraní.


## <a name="using-the-dependency-injection-software-design-pattern"></a>Pomocí vzoru návrhu softwaru injektáž závislostí

Teď, když náš kód přístupu k datům jsme migrovali do samostatné třídy úložiště, musíme upravit kontakt kontroleru k použití této třídy. My podnikneme výhod softwaru návrhový vzor, který volá injektáž závislostí pomocí třídy úložiště v kontroleru.

Upravené kontakt kontroleru je součástí výpis 3.

**Výpis 3 - Controllers\ContactController.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample3.vb)]

Všimněte si, že kontroler kontakt v informacích 3 má dva konstruktory. První konstruktor předá konkrétní instance objektu rozhraní IContactManagerRepository druhý konstruktor. Kontaktujte řadič třídě používá *injektáž závislostí konstruktor*.

Jeden a pouze místo, že se používá třída EntityContactManagerRepository je v první konstruktor. Zbývající část třídy používá rozhraní IContactManagerRepository místo konkrétní třída EntityContactManagerRepository.

To umožňuje snadno přepínat implementace třídy IContactManagerRepository v budoucnu. Pokud chcete používat třídu DataServicesContactRepository namísto třídy EntityContactManagerRepository, stačí upravte první konstruktor.

Injektáž závislostí konstruktor také díky třídy kontroleru kontakt velmi testovatelné. V testování částí lze vytvořit instanci kontroleru kontakt předáním imitované implementace třídy IContactManagerRepository. Při sestavování testů jednotek pro aplikace Správce kontaktů, bude tato funkce vkládání závislostí pro nás velmi důležité při příští iteraci.

> [!NOTE] 
> 
> Pokud chcete úplně oddělit třídy kontroleru kontaktu z konkrétní implementace rozhraní IContactManagerRepository pak můžete využít rozhraní Framework, která podporuje vkládání závislostí, jako je například StructureMap nebo Microsoftu Entity Framework (MEF). S využitím vkládání závislostí framework nikdy musí odkazovat na konkrétní třídy v kódu.


## <a name="creating-a-service-layer"></a>Vytvoření vrstvy služby

Jste si možná všimli, že naše logiku ověřování stále zkombinovaný s naší logice kontroleru ve třídě upravené kontroleru v zobrazení 3. Ze stejného důvodu, že je vhodné izolovat naše logikou přístupu k datům je vhodné izolovat naše logiku ověřování.

Chcete-li vyřešit tento problém, můžeme vytvořit samostatné [vrstva služby](http://martinfowler.com/eaaCatalog/serviceLayer.html). Vrstva služby je samostatné vrstvy, který jsme může vložit mezi naše kontroleru a třídy úložiště. Vrstva služby obsahuje naše obchodní logiku, včetně všech našich logiku ověřování.

Výpis 4 je součástí ContactManagerService. Obsahuje logiku ověřování od třídy controller, kontaktu.

**Část 4 – Models\ContactManagerService.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample4.vb)]

Všimněte si, že konstruktor pro ContactManagerService vyžaduje ValidationDictionary. Vrstva služby komunikuje s vrstvou řadič prostřednictvím této ValidationDictionary. Když si popíšeme vzor Dekoratér probereme ValidationDictionary podrobně v následující části.

Všimněte si kromě toho, že ContactManagerService implementuje rozhraní IContactManagerService. Je nutné vždy snažit se programovat proti konkrétní třídy, ale rozhraní. Další třídy v aplikaci Správce kontaktů nemají možnost zasahovat ContactManagerService třídy přímo. Místo toho s jedinou výjimkou je zbytek kontaktujte správce aplikace programovat proti IContactManagerService rozhraní.

Rozhraní IContactManagerService je obsažen v informacích 5.

**Výpis 5 - Models\IContactManagerService.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample5.vb)]

Upravené třídy kontroleru kontakt je obsažen v informacích 6. Všimněte si, že kontroler kontakt už komunikuje s úložišti ContactManager. Místo toho řadiče kontakt komunikuje ContactManager služby. Každá vrstva je izolovaný co nejvíc z jiných vrstev.

**Výpis 6 - Controllers\ContactController.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample6.vb)]

Naše aplikace už běží afoul o jedné zásadě odpovědnost (SRP). Každý zodpovědnosti než řízení toku provádění aplikace bylo provedeno oříznutí řadič kontakt v 6 výpis. Veškerou logiku ověření byl odebrán z kontroleru kontakt a vloženy do vrstvy služeb. Veškerou logiku databáze bylo vloženo do vrstvy úložiště.

## <a name="using-the-decorator-pattern"></a>Použití vzoru Dekoratéru

Chcete být schopni zcela oddělit naše vrstva služby z našich vrstvy kontroleru. V zásadě jsme měli ke zkompilování naše služby vrstvy v samostatné sestavení z našich vrstvy řadič, aniž by bylo nutné přidat odkaz na naši aplikaci MVC.

Naše vrstva služby však musí být moct projít ověřením chybové zprávy zpět do kontroleru vrstvy. Jak jsme povolit vrstvy služby pro komunikaci chybových zpráv ověření bez párování kontroleru a vrstva služby? Můžeme využít návrhový vzor softwaru s názvem [Dekoratér vzor](http://en.wikipedia.org/wiki/Decorator_pattern).

Kontroler ModelStateDictionary s názvem ModelState používá k reprezentaci chyby ověření. Proto můžete mít tendenci k předání ModelState z vrstvy kontroleru k vrstvě služby. Nicméně pomocí ModelState ve vrstvě služby s žádným vrstvě služby závisí na funkce rozhraní ASP.NET MVC. To může být chybná, protože někdy, můžete chtít použít vrstvě služby s WPF aplikace namísto aplikace ASP.NET MVC. V takovém případě wouldn t chcete odkazovat na rozhraní ASP.NET MVC pomocí třídy ModelStateDictionary.

Vzor Dekoratér umožňuje zabalit existující třídy v nové třídy kvůli implementaci rozhraní. Kontaktujte správce projektu obsahuje třídu ModelStateWrapper součástí výpis 7. ModelStateWrapper třída implementuje rozhraní ve výpisu 8.

**Výpis 7 - Models\Validation\ModelStateWrapper.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample7.vb)]

**Výpis 8 - Models\Validation\IValidationDictionary.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample8.vb)]

Pokud nepodniknete pořádně prohlédněte výpis 5 pak uvidíte, že vrstva služby ContactManager používá výhradně IValidationDictionary rozhraní. Služba ContactManager není závislá na třídu ModelStateDictionary. Kontaktujte řadič vytvoří službu ContactManager, zabalí kontroleru jeho ModelState takto:

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample9.vb)]

## <a name="summary"></a>Souhrn

V této iterace jsme nepřidali žádné nové funkce do aplikace Správce kontaktů. Cílem této iterace bylo Refaktorovat Správce kontaktů aplikaci tak, že se snadněji spravovat a upravovat.

Nejprve implementovali jsme vzor návrhu úložiště softwaru. Jsme migrovali všechny kód přístupu k datům do samostatné třídy ContactManager úložiště.

Také jsme naše logiku ověřování izolovaně od našich logice kontroleru. Vytvořili jsme vrstvu samostatnou službu, která obsahuje všechny naše kód pro ověření. Kontroler vrstvy komunikuje se vrstva služby a vrstva služby komunikuje s vrstvou úložiště.

Když jsme vytvořili vrstva služby, jsme využil vzor Dekoratér izolovat ModelState z našich vrstvy služby. V našem vrstvě služby jsme programovat proti rozhraní IValidationDictionary místo ModelState.

Nakonec jsme využil návrhový vzor softwaru s názvem vzorek vkládání závislostí. Tento model umožňuje programovat proti konkrétní třídy, ale rozhraní (abstrakce). Implementace vzoru návrhu injektáž závislostí také díky našeho kódu více možností intenzivního testování. V další iteraci přidáme do našich projektu testování částí.

> [!div class="step-by-step"]
> [Předchozí](iteration-3-add-form-validation-vb.md)
> [další](iteration-5-create-unit-tests-vb.md)
