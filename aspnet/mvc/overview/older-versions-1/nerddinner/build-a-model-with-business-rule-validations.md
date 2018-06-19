---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: Vytvoření modelu s obchodní pravidlo ověření | Microsoft Docs
author: microsoft
description: Krok 3 ukazuje, jak vytvořit model, že jsme pomocí obou dotazu a aktualizaci databáze pro naši aplikaci NerdDinner.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: c5a482474fd2f41836f70952306ada5cd9136455
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874938"
---
<a name="build-a-model-with-business-rule-validations"></a>Vytvoření modelu s obchodní pravidlo ověření
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Toto je krok 3 bezplatný [kurz aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , nevystavíte slabé stránky zabezpečení – prostřednictvím postup sestavení malá, ale dokončení, webové aplikace pomocí ASP.NET MVC 1.
> 
> Krok 3 ukazuje, jak vytvořit model, že jsme pomocí obou dotazu a aktualizaci databáze pro naši aplikaci NerdDinner.
> 
> Pokud používáte ASP.NET MVC 3, doporučujeme provedením [získávání spuštěna s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Hudba úložiště](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.


## <a name="nerddinner-step-3-building-the-model"></a>NerdDinner krok 3: Vytvoření modelu

Ve model-view-controller framework termín "model" odkazuje na objekty, které představují data, aplikace a také odpovídající logiku domény, který se integruje s jeho ověření a obchodní pravidla. Model je v mnoha směrech "vysílat" aplikace založené na MVC a jako ukážeme později zásadně jednotky chování ho.

Rozhraní ASP.NET MVC podporuje používání technologií pro přístup k žádné dat a mohou zvolit z mnoha různých bohaté možnosti dat .NET pro implementaci jejich modely, včetně: technologie LINQ to Entities, technologie LINQ to SQL, NHibernate, LLBLGen Pro, SubSonic, WilsonORM nebo právě nezpracovaná ADO. NET DataReaders nebo datové sady.

Pro naši aplikaci NerdDinner jsme se chystáte použít technologie LINQ to SQL k vytvoření jednoduchého modelu, který odpovídá poměrně úzce naše návrhu databáze a přidá některé vlastní ověřovací logiku a obchodní pravidla. Třídy úložiště, která pomáhá rychle abstraktní bude potom implementaci implementace trvalosti dat od ostatních aplikací a umožňuje nám snadno jednotky otestovat.

### <a name="linq-to-sql"></a>Technologie LINQ to SQL

Technologie LINQ to SQL je ORM (objekt relační mapper), který se dodává jako součást rozhraní .NET 3.5.

Technologie LINQ to SQL poskytuje snadný způsob, jak mapovat na třídy rozhraní .NET, které jsme můžete kód proti databázových tabulek. Pro naši aplikaci NerdDinner použijeme ji k mapování tabulek večeří a zasílání zpráv rysy v rámci naší databáze na večeři a zasílání zpráv rysy třídy. Sloupce tabulky večeří a zasílání zpráv rysy bude odpovídat vlastnosti třídy večeři a zasílání zpráv rysy. Každý objekt večeři a zasílání zpráv rysy bude reprezentovat samostatný řádek v rámci večeří nebo zasílání zpráv rysy tabulky v databázi.

Technologie LINQ to SQL umožňuje vyhnout se nutnosti ručně vytvořit příkazy SQL se načítají a aktualizují večeři a zasílání zpráv rysy objekty s daty databáze. Místo toho budeme definovat třídy večeři a zasílání zpráv rysy, jak jsou mapovány do/z databáze a vztahy mezi nimi. Technologie LINQ to SQL se pak dojde stará se o generování odpovídající logiky provádění SQL pro použití v době běhu, když jsme komunikovat a jejich použití.

Podpora jazyka LINQ v jazyce Visual Basic a C# jsme můžete použít k zápisu výrazovou dotazy, které načíst večeři a zasílání zpráv rysy objekty z databáze. To minimalizuje množství dat kód potřebujeme k zápisu, a umožňuje vytvářet skutečně vyčištění aplikace.

### <a name="adding-linq-to-sql-classes-to-our-project"></a>Přidání třídy LINQ to SQL pro naše projektu

Jsme budete začněte tím, že kliknete pravým tlačítkem na složku "Modely" v rámci naší projekt a vyberte **Add -&gt;nová položka** příkazu v nabídce:

![](build-a-model-with-business-rule-validations/_static/image1.png)

Otevře se dialog "Přidat novou položku". Jsme budete filtrovat podle kategorie "Data" a vyberte šablonu "LINQ pro třídy SQL" v něm:

![](build-a-model-with-business-rule-validations/_static/image2.png)

Jsme budete název položky "NerdDinner" a klikněte na tlačítko "Přidat". Visual Studio se přidá soubor NerdDinner.dbml naše \Models adresáře a pak otevřete LINQ to SQL Návrhář relací objektů:

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a>Vytváření tříd modelu dat s LINQ to SQL

Technologie LINQ to SQL umožňuje rychle vytvořit třídy modelu dat z existujícího schématu databáze. Úkolů to jsme budete NerdDinner databázi otevřít v Průzkumníku serveru a vyberte tabulky, chceme modelu v něm:

![](build-a-model-with-business-rule-validations/_static/image4.png)

Jsme můžete přetahovat tabulky do LINQ na plochu návrháře SQL. Při provádění této LINQ to SQL se automaticky vytvoří večeři a zasílání zpráv rysy třídy pomocí schématu tabulek (pomocí vlastnosti třídy, které jsou mapovány na sloupce tabulky databáze):

![](build-a-model-with-business-rule-validations/_static/image5.png)

Ve výchozím nastavení technologie LINQ to SQL návrhář automaticky "převádí do množného čísla" názvy tabulek a sloupců při vytváření třídy, na základě schématu databáze. Například: "Večeří" tabulky v našem příkladu výše výsledkem třídu "Večeři". Tato třída pojmenování pomáhá, aby naše modely v souladu s zásady vytváření názvů v rozhraní .NET a obvykle najít této s návrháře opravu to si vhodné (zejména při přidávání spoustu tabulky). Pokud chcete název třídy nebo vlastnost, která generuje návrháře, i když můžete vždy přepsat a změňte jej na libovolný název, který má být. To provedete tak, že upravíte entity nebo vlastnost název v řádku v Návrháři nebo změnou prostřednictvím mřížku vlastností.

Ve výchozím nastavení technologie LINQ to SQL Návrhář také zkontroluje, zda obsahuje primární klíč, cizí klíče relace tabulek a na jejich základě automaticky vytvoří výchozí "vztah přidružení" mezi třídami jiného modelu, který vytváří. Například když jsme přetáhnout večeří a zasílání zpráv rysy tabulky do LINQ to SQL Návrháře vztah jeden mnoho přidružení mezi nimi byl vyvozen na základě skutečnosti, že v tabulce zasílání zpráv rysy měl cizího klíče do tabulky večeří (to je indikován na šipku v Návrhář):

![](build-a-model-with-business-rule-validations/_static/image6.png)

Výše uvedené přidružení způsobí, že LINQ to SQL pro zasílání zpráv rysy třídu, která vývojářům můžete použít pro přístup k večeři spojené s danou zasílání zpráv rysy přidání silného typu vlastnosti "Večeři". Může to způsobit také třídy pro večeři "RSVPs" vlastnosti kolekce, která umožňuje vývojářům načíst a aktualizovat zasílání zpráv rysy objekty přidružené k určité večeři.

Když jsme vytvořit nový objekt zasílání zpráv rysy a přidat jej do kolekce večeři RSVPs, níže můžete zobrazit příklad technologie intellisense v sadě Visual Studio. Všimněte si, jak technologie LINQ to SQL automaticky přidá kolekci "RSVPs" na objektu večeři:

![](build-a-model-with-business-rule-validations/_static/image7.png)

Přidáním objekt zasílání zpráv rysy do kolekce večeři RSVPs jsme informace o tom, LINQ to SQL pro přidružení cizího klíče vztah mezi večeře a zasílání zpráv rysy řádek v naší databázi:

![](build-a-model-with-business-rule-validations/_static/image8.png)

Pokud chcete způsob, jakým má návrháře modelován nebo s názvem přidružení tabulky, můžete ho přepsat. Právě klikněte na šipku přidružení v návrháři a otevřete jeho vlastnosti prostřednictvím mřížku vlastností přejmenovat, odstranit nebo upravit. Pro naši aplikaci NerdDinner ale pravidla přidružení výchozí fungují dobře u datových tříd modelu, které jsme vytváření a můžeme jednoduše použít výchozí chování.

### <a name="nerddinnerdatacontext-class"></a>NerdDinnerDataContext Class

Třídy rozhraní .NET, které představují modely a databáze vztahy definované pomocí LINQ to SQL návrhář automaticky vytvoří sada Visual Studio. LINQ to SQL DataContext třídy se také generuje pro každou technologii LINQ to SQL Návrháře soubory přidané do řešení. Protože jsme pojmenovali naše technologie LINQ to SQL třída položku "NerdDinner", bude mít název "NerdDinnerDataContext" třídě DataContext vytvořena. Tato třída NerdDinnerDataContext je primární způsob, jakým jsme bude komunikovat s databází.

Naše třída NerdDinnerDataContext zpřístupňuje dvě vlastnosti - "Večeří" a "RSVPs" - představující dvě tabulky, které jsme modelován v databázi. Jsme vám pomůže C# zápis dotazů LINQ s tyto vlastnosti objekty večeři a zasílání zpráv rysy dotazu a načíst z databáze.

Následující kód ukazuje, jak vytvořit instanci objektu NerdDinnerDataContext a provést dotaz LINQ proti ho k získání posloupnost večeří, ke kterým došlo v budoucnu. Visual Studio poskytuje úplné intellisense při zápisu dotaz LINQ, a objektů vrácených z něj jsou silného typu a také podporu technologie intellisense:

![](build-a-model-with-business-rule-validations/_static/image9.png)

Kromě toho, že nám se dotázat na objekty večeři a zasílání zpráv rysy, NerdDinnerDataContext také automaticky zjišťuje všechny změny, které následně provedeme večeři a zasílání zpráv rysy objekty, které jsme načíst jeho prostřednictvím. Tato funkce jsme vám pomůže snadno uložte změny zpět do databáze – bez nutnosti psaní jakéhokoli explicitní aktualizace kódu SQL.

Například následující kód ukazuje, jak použít dotaz LINQ načíst jediný objekt večeři z databáze, aktualizovat dvě vlastnosti večeři a potom uložte změny zpět do databáze:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

Objekt NerdDinnerDataContext v kódu výše automaticky sledovat vlastnost změny provedené v večeři objekt, který jsme načítají. Když jsme volat metodu "SubmitChanges()", provede odpovídající "Aktualizovat" příkazu SQL k databázi a zachovat aktualizovanými hodnotami.

### <a name="creating-a-dinnerrepository-class"></a>Vytvoření třídy DinnerRepository

Pro malé aplikace je někdy jemné do mají řadiče pracovat přímo v technologii LINQ to SQL DataContext třídy a vložit dotazů LINQ v rámci do řadičů. Jak získat větší aplikace, ale tento přístup stane nevhodnou pro údržbu a testování. Také může vést k nám duplikování stejné dotazů LINQ na více místech.

Jeden z přístupů, můžete usnadnit aplikace udržovat a testování je použití vzoru "úložiště". Třídy úložiště pomáhá zapouzdření dat dotazování a logiku trvalosti a rychle přehledů podrobnosti implementace trvalosti dat z aplikace. Kromě vytváření čisticí kód aplikace, pomocí vzoru úložiště můžete usnadňují implementace úložiště dat v budoucnu změnit a lze zefektivnit testování aplikace bez nutnosti skutečné databáze částí.

Pro naši aplikaci NerdDinner budeme definovat třídu DinnerRepository s pod podpis:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

*Poznámka: Dále v této kapitole jsme budete extrahovat rozhraní IDinnerRepository z této třídy a povolte vkládání závislostí s ním na našem řadiče. Na začátku ale přidáme spuštění jednoduché a právě pracovat přímo s třídou DinnerRepository.*

K implementaci této třídy jsme budete klikněte pravým tlačítkem na složku naše "Modely" a zvolte **Add -&gt;nová položka** příkazu nabídky. V dialogovém okně "Přidat novou položku" jsme budete vyberte šablonu "Třída" a "DinnerRepository.cs" název souboru:

![](build-a-model-with-business-rule-validations/_static/image10.png)

Pak můžeme implementovat naše DinnerRespository třídy pomocí následující kód:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a>Načítání, aktualizaci, vkládání a odstraňování pomocí DinnerRepository – třída

Teď, vytvořili jsme Naše třída DinnerRepository, podíváme se na několik příkladů kódu, které ukazují běžné úkoly, které jsme s ním může dělat:

#### <a name="querying-examples"></a>Dotazování příklady

Následující kód načte jeden večeři pomocí DinnerID hodnoty:


[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

Následující kód načte všechny nadcházející večeří a smyčky přes ně:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a>Vložení a aktualizace příklady

Následující kód ukazuje, přidávání dvě nové večeří. Dodatky nebo změny do úložiště nejsou potvrzené do databáze, dokud v něm je volána metoda "Save()". Technologie LINQ to SQL automaticky sbalí všechny změny v databázové transakci – tak, aby všechny změny dojít nebo žádná z nich dělat, když se uloží naše úložiště:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

Následující kód načte existujícího objektu večeři a upraví dvě vlastnosti na něm. Změny se potvrdí zpět do databáze při volání metody "Save()" v našem úložišti:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

Následující kód načte večeři a pak do ní přidá odpověď. Dělá to pomocí kolekce RSVPs objektu večeři technologie LINQ to SQL pro nám vytvořit, (protože mezi těmito dvěma v databázi existuje vztah primární klíč/cizí klíč). Tato změna je trvalá zpět do databáze jako nový řádek tabulky zasílání zpráv rysy při volání metody "Save()" v úložišti:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a>Odstranit příklad

Následující kód načte existujícího objektu večeři a pak označí pro odstranění. Při volání metody "Save()" na úložiště se bude potvrďte odstranění zpět do databáze:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a>Ověřování a obchodní logice pravidla integrování třídy modelu

Integrace ověření a obchodní pravidlo, že logika je klíčovou součástí jakékoli aplikace, která pracuje s daty.

#### <a name="schema-validation"></a>Ověření schématu

Když třídy modelu jsou definovány pomocí LINQ to SQL designer, datové typy vlastností v třídy modelu dat odpovídat datové typy databázové tabulky. Například: Pokud je sloupec "EventDate" v tabulce večeří "datetime", třída modelu dat vytvořených technologie LINQ to SQL bude typu "Datum a čas" (což je předdefinovaný datový typ rozhraní .NET). To znamená, že pokud se pokusíte přiřadit celé číslo nebo logickou hodnotu k němu z kódu a bude vyvolat chybu automaticky pokusíte implicitně převést typu není platný řetězec na jeho za běhu bude docházet k chybám kompilace.

Technologie LINQ to SQL se také automaticky zpracovává útěku hodnot SQL pro vás při použití řetězce – které pomáhá chránit před útoky vkládání SQL při používání.

#### <a name="validation-and-business-rule-logic"></a>Ověřování a obchodní logika pravidla

Ověření schématu je užitečné jako první krok, ale stačí jen zřídka. Většina scénářů reálného vyžaduje možnost určit bohatší logiku ověření, můžete span více vlastností, spouštění kódu a často mají sledování stavu modelu (například: ho Probíhá vytváření/aktualizovat/odstraněný, nebo v rámci stavu specifické pro doménu jako "archivovaný"). Existuje mnoho různých různé vzorce a rozhraní, které lze použít k definování a použít pravidla ověřování na třídy modelu a existuje několik .NET na základě architektury odhlašování došlo použitého usnadní to. Můžete použít prakticky některý z nich v rámci aplikace ASP.NET MVC.

Pro účely aplikace NerdDinner použijeme relativně jednoduché a jednoduché vzor kde zveřejňujeme IsValid – vlastnosti a metody GetRuleViolations() na našich večeři objekt modelu. IsValid – vlastnost vrátí hodnotu PRAVDA nebo NEPRAVDA v závislosti na tom, jestli jsou všechny platné pravidel ověřování a business. Metoda GetRuleViolations() vrátí seznam všech chyb pravidlo.

Jsme budete implementovat IsValid a GetRuleViolations() pro náš model večeři přidáním "konkrétní třídu" do našich projektu. Částečné třídy lze použít k přidání metody nebo vlastnosti nebo události na třídy spravuje pomocí návrháře VS (podobně jako třída večeři generované LINQ to SQL designer) a vyhnout nástroj z messing naše kódem. Jsme můžete přidat nové částečné třídy do našich projektu kliknutím pravým tlačítkem na složku \Models a potom vyberte příkaz nabídky "Přidat novou položku". Pak můžeme zvolte šablonu "Třída" v dialogovém okně "Přidat novou položku" a pojmenujte ji Dinner.cs.

![](build-a-model-with-business-rule-validations/_static/image11.png)

Klepnutím na tlačítko "Přidat" Přidat soubor Dinner.cs naše projektu a otevřete jej v prostředí IDE. Potom jsme můžete implementovat s použitím framework základní pravidlo a ověření vynucení níže uvedeného kódu:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

Několik poznámky o ve výše uvedeném kódu:

- Třída večeři začíná "částečné" klíčové slovo – to znamená, kód jsou v něm obsažena bude technologií LINQ to SQL Návrháře v kombinaci s třídě vygeneruje udržovat a zkompilovat do jedné třídy.
- Třída RuleViolation je pomocná třída, kterou přidáme do projektu, který umožňuje nám poskytnout další podrobnosti o porušení pravidel.
- Metoda Dinner.GetRuleViolations() způsobí, že naše ověření a obchodní pravidla, která vyhodnotí (jsme budete implementovat krátce). Poté vrátí zpět posloupnost RuleViolation objekty, které poskytují další podrobnosti o chybách, pravidlo.
- Vlastnost Dinner.IsValid poskytuje užitečné pomocné vlastnost, která označuje, zda má objekt večeři žádné aktivní RuleViolations. Ho lze proaktivně zkontrolovat vývojáři pomocí objektu večeři v kdykoli (a nevyvolá výjimku).
- Částečné metody Dinner.OnValidate() je háku poskytující technologie LINQ to SQL, které umožňuje upozorněni kdykoliv objekt večeři je nastavit jako trvalý, v databázi. Naše implementace OnValidate() výše zajistí, že večeře nemá žádné RuleViolations před uložením. Pokud je v neplatném stavu, vyvolá výjimku, což způsobí LINQ to SQL ji přerušit.

Tento přístup poskytuje jednoduché rozhraní, které budeme integrovat ověření a obchodní pravidla do. Nyní Pojďme přidat níže pravidla, která metoda naše GetRuleViolations():

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

Vrátit posloupnost žádné RuleViolations používáme "výnos vrátit" funkce jazyka C#. První šesti pravidla kontroly výše jednoduše vynutit, aby vlastnosti řetězce na našem večeři nemůže být null nebo prázdný. Poslední pravidlo je o něco zajímavějšího a volání PhoneValidator.IsValidNumber() Pomocná metoda, jsme do projektu můžete přidat naše ověřit, jestli ContactPhone číslo formát odpovídá večeře na zemi.

Můžeme použít. Podpora je NET regulární výraz k implementaci tuto telefonickou podporu ověřování. Níže je jednoduchá implementace PhoneValidator, která může přidáme pro naše projekt, který umožňuje nám přidat jednotlivé země Regex vzor kontroly:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a>Ověření zpracování a obchodní logiku porušení

Teď, když přidali jsme výše ověření a obchodní pravidlo kód, kdykoli pokusíme se vytvořit nebo aktualizovat večeři, naše ověřovací logiku pravidla vyhodnotit a vynutit.

Vývojářům psát kód jako dole proaktivně určit, jestli je objekt večeři platný a načíst seznam všech porušení v něm bez vyvolání jakékoli výjimky:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

Pokud jsme pokus o uložení večeři v neplatném stavu, bude vyvolána výjimka, když jsme volat metodu Save() u DinnerRepository. K tomu dochází, protože technologie LINQ to SQL automaticky volá metodu částečné naše Dinner.OnValidate(), než ho uloží večeři změny a jsme přidali kód pro Dinner.OnValidate() k vyvolána výjimka, pokud v večeře neexistuje žádné porušení pravidel. Můžeme tuto výjimku zachytit a reaktivně načítat seznam porušení vyřešit:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

Protože naše ověření a obchodní pravidla jsou implementované v rámci naší vrstva modelu a ne v rámci vrstvě uživatelského rozhraní, budou použity se použije ve všech scénářích v naší aplikaci. Jsme později můžete změnit nebo přidat obchodní pravidla a nastavit všechny kód funguje s objekty naše večeři respektovat je.

S flexibilně změnit obchodní pravidla na jednom místě, bez nutnosti tyto změny ripple v celé aplikaci a logika uživatelského rozhraní, je znak správně vytvořená aplikace a výhody, která představuje rozhraní MVC pomáhá podporovat.

### <a name="next-step"></a>Další krok

My jste nyní model, který jsme vám pomůže dotazy na data a aktualizace našich databáze.

Umožňuje přidat některé řadiče a zobrazení pro projekt, který jsme můžete použít k vytvoření HTML uživatelské rozhraní kolem něj.

> [!div class="step-by-step"]
> [Předchozí](create-a-database.md)
> [další](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
