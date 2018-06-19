---
uid: mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
title: 'Iterace #4 – zpřístupnění aplikace volně vázány (C#) | Microsoft Docs'
author: microsoft
description: V této třetí iteraci jsme využít výhod několik softwaru vzory návrhu na bylo snazší spravovat a upravovat aplikace obraťte se na správce. Pro...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 829f589f-e201-4f6e-9ae6-08ae84322065
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
msc.type: authoredcontent
ms.openlocfilehash: 33221c6c3326c7034fe013f152579828e2fc8a3a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873976"
---
<a name="iteration-4--make-the-application-loosely-coupled-c"></a>Iterace #4 – zpřístupnění aplikace volně vázány (C#)
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhněte si kód](iteration-4-make-the-application-loosely-coupled-cs/_static/contactmanager_4_cs1.zip)

> V této třetí iteraci jsme využít výhod několik softwaru vzory návrhu na bylo snazší spravovat a upravovat aplikace obraťte se na správce. Například můžeme Refaktorovat naše aplikace pro použití vzoru úložiště a vzoru vkládání závislostí.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Vytvoření aplikace ASP.NET MVC správy kontaktů (C#)

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

V této čtvrté iteraci aplikace, obraťte se na správce jsme Refaktorovat aplikace zpřístupnění aplikace více volně vázány. Pokud aplikace je volně vázány, můžete upravit kód v jedné části aplikace bez nutnosti upravovat kód v dalších částí aplikace. Volně párované aplikace jsou odolnější vůči změnit.

Všechna data přístupu a ověřování logiky, která používá obraťte se na správce aplikace v současné době je obsažený ve třídách. To je vhodné. Vždy, když budete muset upravit část aplikace, riskujete představení chyby do jiné části vaší aplikace. Například pokud upravíte logika ověřování, riskujete Představujeme nové chyby do logika přístupu nebo řadič data.

> [!NOTE] 
> 
> (SRP), třída by měla mít nikdy více než jeden důvod, chcete-li změnit. Kombinování řadiče, ověřování a logiku databáze je masivní narušení jednu zásadu zodpovědnost.


Tady je několik důvodů, které možná budete muset upravit vaší aplikace. Možná budete muset přidat nové funkce do vaší aplikace, možná budete muset oprava chyby v aplikaci nebo možná budete muset upravit způsob implementace funkce vaší aplikace. Aplikace jsou zřídka statické. Budou většinou růst a mutovat v čase.

Představte si třeba, abyste se rozhodli změnit, jak implementovat vaše vrstvou. Pravé nyní, obraťte se na správce aplikace používá Microsoft Entity Framework pro přístup k databázi. Ale můžete rozhodnout k migraci technologií pro přístup k nové nebo alternativní data, jako je například ADO.NET Data Services nebo NHibernate. Ale protože přístupového kódu dat není izolovaná od kód ověřování a řadiče, neexistuje žádný způsob, jak upravit kód přístup dat ve vaší aplikaci bez úpravy jiný kód, který přímo nesouvisí se přístup k datům.

Pokud aplikace je volně vázány, na druhé straně může provedete změny jednu část aplikace, bez zásahu do dalších částí aplikace. Například můžete přepnout technologie pro přístup k datům beze změny logika ověřování nebo kontroleru.

V této iteraci jsme využít několik vzory návrhu softwaru, které nám Refaktorovat obraťte se na správce aplikace do více volného párované aplikace. Když jsme se provádějí, obraťte se na správce, won t udělat nic je dodán t provést před. Ale jsme budete moci změnit aplikace snadněji v budoucnu.

> [!NOTE] 
> 
> Refaktoring je proces přepisování aplikace tak, že není ztratit všechny stávající funkce.


## <a name="using-the-repository-software-design-pattern"></a>Pomocí vzoru návrhu úložiště softwaru

Naše první změna je využít výhod softwaru návrhový vzor, který volá použitému vzoru. Použijeme použitému vzoru izolovat naše data přístupového kódu od zbytku naší aplikace.

Implementace vzoru úložiště vyžaduje, abychom mohli dokončit následující dva kroky:

1. Vytvořit rozhraní
2. Vytvořte konkrétní třídu, která implementuje rozhraní

Nejprve musíme vytvořit rozhraní, které popisuje všechny metody přístupu dat, které je potřeba provést. Rozhraní IContactManagerRepository je obsažený v výpis 1. Toto rozhraní popisuje pět metody: CreateContact(), DeleteContact(), EditContact(), GetContact a ListContacts().

**Výpis 1 - Models\IContactManagerRepositiory.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample1.cs)]

Dále je potřeba vytvořit konkrétní třídu, která implementuje rozhraní IContactManagerRepository. Protože Microsoft Entity Framework se používá pro přístup k databázi, vytvoříme novou třídu s názvem EntityContactManagerRepository. Tato třída je obsažený v výpis 2.

**Výpis 2 - Models\EntityContactManagerRepository.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample2.cs)]

Všimněte si, že EntityContactManagerRepository třída implementuje rozhraní IContactManagerRepository. Třída implementuje všechny pět z metod popsaných v tomto rozhraní.

Může vás zajímat, proč je potřeba zabývat rozhraní. Proč je potřeba vytvořit rozhraní a třídy, která se implementuje?

S jednou výjimkou zbytek naše aplikace budou používat rozhraní a ne na konkrétní třídu. Namísto volání metody vystavené třídě EntityContactManagerRepository, zavoláme vám metody vystavené IContactManagerRepository rozhraní.

Tímto způsobem, můžeme implementovat rozhraní s novou třídu bez nutnosti upravit zbývající část naší aplikace. Například v některé budoucí datum, budeme chtít implementovat třídu DataServicesContactManagerRepository, která implementuje rozhraní IContactManagerRepository. Třída DataServicesContactManagerRepository může použít ADO.NET Data Services pro přístup k databázi místo Microsoft Entity Framework.

V případě, že naše kód aplikace je naprogramovaný tak proti rozhraní IContactManagerRepository místo konkrétní třída EntityContactManagerRepository jsme beze změny všechny zbývající části kódu přepínat konkrétní třídy. Například jsme můžete přepnout ze třídy EntityContactManagerRepository třídě DataServicesContactManagerRepository beze změny dat logiku ověření nebo přístupu.

Programování s rozhraní (abstrakce) namísto konkrétní třídy způsobí naše aplikace odolnější vůči změnit.

> [!NOTE] 
> 
> Výběrem možnosti nabídky Refaktorovat, extrahování rozhraní můžete rychle vytvořit rozhraní z konkrétní třídy v sadě Visual Studio. Můžete například nejprve vytvořit třídu EntityContactManagerRepository a pak použijte extrahování rozhraní má automaticky generovat rozhraní IContactManagerRepository.


## <a name="using-the-dependency-injection-software-design-pattern"></a>Pomocí vzoru návrhu softwaru vkládání závislostí

Teď, když jsme naše data přístupového kódu migraci do samostatné třídy úložiště, je potřeba upravit obraťte se na kontroleru k použití této třídy. Jsme bude využívat softwaru návrhový vzor, který volá vkládání závislostí pro použití třídy úložiště v kontroleru.

Upravené obraťte se na řadič, je součástí výpis 3.

**Výpis 3 - Controllers\ContactController.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample3.cs)]

Všimněte si, že obraťte se na zařízení v výpis 3 má dva konstruktory. První konstruktoru předá konkrétní instanci rozhraní IContactManagerRepository druhý konstruktor. Použití třídy obraťte se na řadič *vkládání závislostí konstruktor*.

Ten a pouze místní, jestli se používá třída EntityContactManagerRepository je v první konstruktoru. Zbývající část třídu používá rozhraní IContactManagerRepository místo konkrétní EntityContactManagerRepository třída.

To usnadňuje přepínač implementace třídy IContactManagerRepository v budoucnu. Pokud chcete použít třídu DataServicesContactRepository místo třídě EntityContactManagerRepository, stačí upravte první konstruktor.

Vkládání závislostí konstruktor také díky třídy kontroleru obraťte se na velmi možností intenzivního testování. V testů jednotek můžete vytvořit instanci kontaktujte řadič předáním imitovanou implementaci třídy IContactManagerRepository. Tato funkce vkládání závislostí bude velmi důležité, abyste nám v další iterace, když jsme vytvářet testy částí pro aplikace obraťte se na správce.

> [!NOTE] 
> 
> Pokud chcete zcela oddělit třídy kontroleru kontakt z konkrétní implementace rozhraní IContactManagerRepository pak můžete využít výhod Framework, která podporuje vkládání závislostí například StructureMap nebo Microsoft Rozhraní Entity Framework (MEF). Využitím vkládání závislostí Framework, budete muset nikdy odkazovat na konkrétní třídy v kódu.


## <a name="creating-a-service-layer"></a>Vytvoření vrstvy služby

Možná jste si všimli, že ověřovací logiku stále smíšený s logiku řadiče ve třídě upravené řadiče v výpis 3. Ze stejného důvodu je vhodné izolovat data logiku přístup je vhodné izolovat ověřovací logiku.

Chcete-li tento problém vyřešit, můžeme vytvořit samostatné [ *vrstvy služby*](http://martinfowler.com/eaaCatalog/serviceLayer.html). Vrstva, služby, je do samostatné vrstvy, který jsme můžete vložit mezi naše kontroleru a třídy úložiště. Vrstva služby obsahuje naše obchodní logiky, včetně všech objektů ověřovací logiku.

ContactManagerService je obsažený v výpis 4. Obsahuje logiku ověření od třídy controller kontaktu.

**Výpis 4 - Models\ContactManagerService.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample4.cs)]

Všimněte si, že v konstruktoru pro ContactManagerService vyžaduje ValidationDictionary. Komunikuje se službou vrstvě řadiče prostřednictvím této ValidationDictionary vrstvě služby. ValidationDictionary v následující části podrobně probereme při probereme Dekoratéra vzor.

Všimněte si kromě toho, že ContactManagerService implementuje rozhraní IContactManagerService. Vždycky měli snažit programu proti rozhraní místo konkrétní třídy. Jiné třídy v aplikaci obraťte se na správce s třídou ContactManagerService není komunikovat přímo. Místo toho s jedinou výjimkou je zbývající aplikace, obraťte se na správce programovat proti IContactManagerService rozhraní.

Rozhraní IContactManagerService je obsažený v výpis 5.

**Výpis 5 - Models\IContactManagerService.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample5.cs)]

Upravené třídy kontroleru kontakt je součástí výpis 6. Všimněte si, že obraťte se na řadič už komunikuje s úložištěm ContactManager. Místo toho obraťte se na řadič komunikuje se službou ContactManager. Jednotlivé úrovně je izolovaná co nejvíce z jiných vrstev.

**Výpis 6 - Controllers\ContactController.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample6.cs)]

Naše aplikace už běží afoul o jeden odpovědnost princip (SRP). Obraťte se na řadič v výpis 6 bylo provedeno oříznutí každých zodpovědnosti než řízení toku provádění aplikací. Veškerou logiku ověření bylo odstraněno z řadiče kontaktu a nabídnutých do vrstvy služby. Veškerou logiku databáze bylo posunuto do vrstvy úložiště.

## <a name="using-the-decorator-pattern"></a>Použití vzoru Dekoratéra

Chceme mít možnost zcela oddělit naše vrstvy služby z našich vrstvy řadiče. V zásadě jsme byste měli mít zkompilovat naše vrstvy služby v samostatném sestavení z našich řadiče vrstvy bez nutnosti se přidat odkaz na naše aplikace MVC.

Naše vrstvy služby však musí být schopni projít ověřením chybové zprávy zpět na vrstvě řadiče. Jak jsme povolit vrstvy služby pro komunikaci chybových zpráv ověření bez spojovacích řadiče a vrstva služby? Jsme můžete využít výhod vzor návrhu softwaru s názvem [Dekoratéra vzor](http://en.wikipedia.org/wiki/Decorator_pattern).

Řadič ModelStateDictionary, s názvem ModelState používá k reprezentaci chyby ověření. Proto může být tendenci předat ModelState z vrstvy řadiče vrstvě služby. Však pomocí ModelState ve vrstvě služby by provést vrstvě služby závisí na funkci aplikace rozhraní ASP.NET MVC. To může být chybná, protože jednou budete, můžete chtít použít vrstvě služby s aplikací WPF namísto aplikace ASP.NET MVC. V takovém případě wouldn t chcete odkazovat na rozhraní ASP.NET MVC pro použití třídy ModelStateDictionary.

Vzor Dekoratéra umožňuje zalomení existující třídy v novou třídu kvůli implementaci rozhraní. Naše obraťte se na správce projekt obsahuje třídu ModelStateWrapper obsažené v výpis 7. Třída ModelStateWrapper implementuje rozhraní výpis 8.

**Listing 7 - Models\Validation\ModelStateWrapper.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample7.cs)]

**Výpis 8 - Models\Validation\IValidationDictionary.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample8.cs)]

Pokud trvat zblízka na výpis 5 zobrazí se, že vrstvy služby ContactManager používá rozhraní IValidationDictionary výhradně. Služba ContactManager není závislá na třídě ModelStateDictionary. Obraťte se na řadič vytvoří službu ContactManager, řadičem zabalí jeho ModelState takto:

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample9.cs)]

## <a name="summary"></a>Souhrn

V této iteraci jsme nepřidali žádné nové funkce do aplikace obraťte se na správce. Cílem této iterace bylo Refaktorovat obraťte se na správce aplikace tak, že se snadněji spravovat a upravovat.

Implementovali jsme nejprve vzor návrhu úložiště softwaru. Jsme migrovaly všechny přístupového kódu data do samostatné třídy ContactManager úložiště.

Také jsme ověřovací logiku oddělený od řadiče logiku. Jsme vytvořili samostatné služby vrstva, která obsahuje všechny naše ověřovacího kódu. Vrstva řadiče komunikuje s vrstvě služby a vrstvy služby komunikuje s vrstvy úložiště.

Pokud jsme vytvořili vrstvy služby, vzali jsme výhod vzoru Dekoratéra izolovat ModelState z našich vrstvy služby. V našem vrstvě služby jsme naprogramovaný tak proti rozhraní IValidationDictionary místo ModelState.

Nakonec vzali jsme výhod vzor návrhu softwaru s názvem vzoru vkládání závislostí. Tento vzor umožňuje nám programu proti rozhraní (abstrakce) namísto konkrétní třídy. Implementace vzoru návrhu vkládání závislostí také díky kódu více možností intenzivního testování. V další iterace přidáme na našem projekt testování částí.

> [!div class="step-by-step"]
> [Předchozí](iteration-3-add-form-validation-cs.md)
> [další](iteration-5-create-unit-tests-cs.md)
