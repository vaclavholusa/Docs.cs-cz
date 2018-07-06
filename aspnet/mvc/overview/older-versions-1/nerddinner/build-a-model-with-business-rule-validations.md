---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: Sestavení modelu s ověřením obchodních pravidel | Dokumentace Microsoftu
author: microsoft
description: Krok 3 ukazuje, jak vytvořit model, že jsme pomocí obou dotazů a aktualizace databáze pro naši aplikaci NerdDinner.
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: 4bed4dd794c7c34551cd3c7543e08ed12d83505a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836766"
---
<a name="build-a-model-with-business-rule-validations"></a>Sestavení modelu s ověřením obchodních pravidel
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Toto je kroku 3 tohoto bezplatného [kurz vývoje aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , který procházení procházení po tom, jak sestavit malý, ale bylo možné provést, webové aplikace pomocí ASP.NET MVC 1.
> 
> Krok 3 ukazuje, jak vytvořit model, že jsme pomocí obou dotazů a aktualizace databáze pro naši aplikaci NerdDinner.
> 
> Pokud používáte ASP.NET MVC 3, doporučujeme je provést [získávání začít s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.


## <a name="nerddinner-step-3-building-the-model"></a>NerdDinner krok 3: Sestavení modelu

V rozhraní framework model-view-controller termín "model" odkazuje na objekty, jež reprezentují data z aplikace, stejně jako odpovídající logiku domény, která se integruje obchodních pravidel ověřování a s ním. Model se ve spoustě ohledů "Srdce" aplikace využívající architekturu MVC a jak uvidíme dále zásadním způsobem řídí chování ho.

Architektura ASP.NET MVC podporuje používání technologií pro přístup k libovolné data a vývojáři můžete vybrat z celé řady možností bohaté .NET dat k implementaci jejich modelů, včetně: technologie LINQ to Entities, LINQ to SQL, NHibernate, LLBLGen Pro, SubSonic, WilsonORM nebo jenom nezpracovaná ADO. NET čtečky dat nebo datových sad.

Pro naši aplikaci NerdDinner budeme používat technologie LINQ to SQL vytvořit jednoduchý model, který odpovídá poměrně blízko našich návrhu databáze a přidá některé vlastní ověřovací logiky a obchodní pravidla. Jsme pak implementuje třídu úložiště, která pomáhá okamžitě abstraktní implementace trvalosti dat od zbývající části aplikace a umožňuje nám to snadno jednotky testování.

### <a name="linq-to-sql"></a>Technologie LINQ to SQL

Technologie LINQ to SQL je ORM (relační mapování objektů), která je dodávána jako součást rozhraní .NET 3.5.

Technologie LINQ to SQL poskytuje snadný způsob, jak mapovat na třídy rozhraní .NET, kterou jsme můžete psát kód s využitím databázových tabulek. Naše aplikace NerdDinner použijeme ji k mapování tabulek večeří a reakce v rámci naší databázi do třídy Dinner a reakce. Sloupce tabulky večeří a reakce bude odpovídat vlastnosti třídy Dinner a reakce. Každý objekt Dinner a RSVP bude reprezentovat samostatných řádků v rámci večeří nebo RSVP tabulek v databázi.

Technologie LINQ to SQL umožňuje vyhnout se tak nutnosti ručně vytvořit příkazy SQL se načítají a aktualizují Dinner a RSVP objekty pomocí dat z databáze. Místo toho budeme definovat třídy Dinner a reakce, jak jsou mapovány do/z databáze a vztahy mezi nimi. Technologie LINQ to SQL se potom vezme péče o generování odpovídající logiky provádění SQL pro použití za běhu při budeme pracovat a jejich použití.

Podpora jazyka LINQ v jazyce Visual Basic a C# jsme můžete použít k zápisu výrazové dotazů, které načítají Dinner a RSVP objekty z databáze. Tím se minimalizují množství dat kódu musíme napsat, a umožňuje vytvářet hezky čistý aplikace.

### <a name="adding-linq-to-sql-classes-to-our-project"></a>Přidání třídy LINQ to SQL na našem projektu

Použijeme začněte tím, že pravým tlačítkem na složku "Modely" v našem projektu a vyberte **Add -&gt;nová položka** příkazu nabídky:

![](build-a-model-with-business-rule-validations/_static/image1.png)

Tím se otevře dialogové okno "Přidat novou položku". Vytvoříme filtrovat podle kategorie "Data" a vyberte šablonu "LINQ na třídy SQL" v rámci něj:

![](build-a-model-with-business-rule-validations/_static/image2.png)

Jsme bude název položky "NerdDinner" a klikněte na tlačítko "Přidat". Visual Studio přidejte NerdDinner.dbml soubor v adresáři naše \Models a potom otevřete LINQ to SQL Návrhář relací objektů:

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a>Vytváření datových tříd modelu pomocí LINQ to SQL

Technologie LINQ to SQL umožňuje nám to rychle vytvoření tříd datových modelů z existující schéma databáze. Úkol to nám budete NerdDinner databázi otevřít v Průzkumníku serveru a vyberte tabulky, chceme pro modelování v ní:

![](build-a-model-with-business-rule-validations/_static/image4.png)

Jsme pak můžete přetáhnout tabulky do LINQ na plochu návrháře SQL. Při provádění této LINQ to SQL se automaticky vytvoří Dinner a RSVP třídy pomocí schématu tabulky (pomocí vlastnosti třídy, které jsou mapovány na sloupce tabulky databáze):

![](build-a-model-with-business-rule-validations/_static/image5.png)

Ve výchozím nastavení LINQ to SQL Návrháře automaticky "do množného" názvy tabulek a sloupců při vytváření třídy na základě schématu databáze. Příklad: ve třídě "Dinner" výsledkem je tabulka "Večeří" v našem příkladu výše. Tato třída pojmenování jednodušeji naše modely konzistentní s zásady vytváření názvů .NET a obvykle najít tento s návrháře oprava to tak pohodlné (zejména při přidávání velké tabulky). Pokud se vám název třída nebo vlastnost, která generuje návrháře, i když můžete kdykoli přepsat nebo ho změnit na libovolný název, který chcete. Můžete provést buď tak, že upravíte entity nebo vlastnosti název v řádku v Návrháři nebo úpravou prostřednictvím mřížku vlastností.

Ve výchozím nastavení LINQ to SQL designer také kontroluje primární klíč/cizího klíče tabulky a na jejich základě automaticky vytvoří výchozí "vztah přidružení" mezi třídami jiný model, který vytvoří. Například, když jsme přetáhli večeří a RSVP tabulky do technologie LINQ to SQL Návrhář vztah jeden mnoho přidružení mezi těmito dvěma byl vyvozen založeno na skutečnosti, že v tabulce RSVP měl cizího klíče tabulky večeří (to je indikován na šipku v Návrhář):

![](build-a-model-with-business-rule-validations/_static/image6.png)

Výše uvedené přidružení způsobí LINQ pro SQL za účelem přidání silného typu vlastnosti "Dinner" RSVP třídu, která mohou vývojáři pro přístup k večeři spojené s danou reakce. Způsobí také mají "RSVPs" kolekce vlastnost, která vývojářům umožňuje načtení a aktualizaci RSVP objekty přidružené k určité Dinner třídy večeři.

Po vytvoření nového objektu RSVP a přidat do kolekce večeři RSVPs níže můžete vidět příklad technologie intellisense v sadě Visual Studio. Všimněte si, jak technologie LINQ to SQL automaticky přidá "RSVPs" kolekce na objekt Dinner:

![](build-a-model-with-business-rule-validations/_static/image7.png)

Přidáním objektu reakce večeři RSVPs kolekce jsme informace o tom, LINQ to SQL pro přidružení cizího klíče relace mezi RSVP řádků v databázi a společnosti Dinner:

![](build-a-model-with-business-rule-validations/_static/image8.png)

Pokud vám nevyhovují, jak má Návrhář modelovat nebo s názvem tabulky přidružení, lze jej přepsat. Stačí klikněte na šipku přidružení v návrháři a získat přístup k vlastnostem pomocí mřížky vlastností, které chcete přejmenovat, odstranit nebo upravit ho. Pro naši aplikaci NerdDinner však výchozí pravidla pro přidružení fungují dobře u tříd datových modelů, které vytváříme a můžeme jednoduše použít výchozí chování.

### <a name="nerddinnerdatacontext-class"></a>Třída NerdDinnerDataContext

Visual Studio automaticky vytvoří třídy rozhraní .NET, které představují modely a databáze vztahy definované pomocí LINQ to SQL návrháře. LINQ na třídy SQL DataContext se také vygeneruje pro každou LINQ na soubor návrháře SQL přidat k řešení. Protože jsme pojmenovali naše technologie LINQ to SQL tříd – položka "NerdDinner", třídy DataContext vytvořili nazývá "NerdDinnerDataContext". Tato třída NerdDinnerDataContext je primární způsob, jakým jsme bude komunikovat s databází.

Naše třída NerdDinnerDataContext zveřejňuje dvě vlastnosti - "Večeří" a "RSVPs" – které představují dvě tabulky, které jsme modelován v rámci databáze. Můžeme použít C# pro zápis dotazů LINQ s těmito vlastnostmi na dotazování a načtení Dinner a RSVP objekty z databáze.

Následující kód ukazuje, jak vytvořit instanci objektu NerdDinnerDataContext a provádění dotazu LINQ na jeho získání posloupnost večeří, ke kterým dochází v budoucnu. Visual Studio obsahuje plnou podporou technologie intellisense při psaní dotazu LINQ a objektů vrácených z něj jsou silného typu a také podporu technologie intellisense:

![](build-a-model-with-business-rule-validations/_static/image9.png)

Mimo možnosti nám se dotázat na večeři a RSVP objekty, NerdDinnerDataContext také automaticky sleduje všechny změny, které následně provedeme Dinner a RSVP objekty, které se nám načíst přes něj. Můžeme použít tuto funkci a snadno uložte změny zpět do databáze - bez nutnosti psát jakýkoli kód explicitní aktualizace SQL.

Například následující kód ukazuje, jak pomocí dotazu LINQ získal jeden objekt Dinner z databáze, aktualizace dvou vlastností Dinner a potom uložte změny zpět do databáze:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

Objekt NerdDinnerDataContext v kódu nad automaticky sleduje vlastnosti změny provedené v Dinner objekt, který jsme získali z něj. Když budeme volat metodu "SubmitChanges()", provede odpovídající "" aktualizace příkazu SQL na databázi k uchování aktualizovanými hodnotami.

### <a name="creating-a-dinnerrepository-class"></a>Vytvoření třídy DinnerRepository

U malých aplikací, je někdy můžete mít řadiče pracovat přímo s LINQ na třídy SQL DataContext a dotazů LINQ v rámci Kontrolery pro vložení. Při získávání aplikací větší, ale tento přístup se stane náročné na údržbu a testování. Může také vést k nám duplikace stejného dotazů LINQ na více místech.

Jedním z přístupů, které můžete zpřístupnit aplikace usnadňuje údržbu a testování, je použití vzoru "úložiště". Třídy úložiště umožňuje zapouzdřit podrobnosti implementace trvalost dat z aplikace dotazování dat a logiku trvalosti a přehledů okamžitě. Kromě toho kód aplikace, čištění, pomocí vzoru úložiště můžete bylo snazší v budoucnu změnit implementace úložiště dat a může být snazší usnadnění testování aplikace bez nutnosti skutečná databáze.

Pro naši aplikaci NerdDinner budeme definovat třídu DinnerRepository s pod podpis:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

*Poznámka: Dále v této kapitole vytvoříme extrahovat rozhraní IDinnerRepository z této třídy a povolit injektáž závislostí s ním na naše řadiče. Než začneme ale budeme začít jednoduchými věcmi a právě pracovat přímo s třídou DinnerRepository.*

K implementaci této třídy vytvoříme klikněte pravým tlačítkem na složku naše "Modely" a zvolte **Add -&gt;nová položka** příkazu nabídky. V dialogovém okně "Přidat novou položku" budete výběru šablony "Třída" a "DinnerRepository.cs" Zadejte název souboru:

![](build-a-model-with-business-rule-validations/_static/image10.png)

Pak můžeme implementovat naše DinnerRespository třídy pomocí níže uvedeného kódu:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a>Načítání, aktualizaci, vkládání a odstranění horizontálních oddílů pomocí třídy DinnerRepository

Teď, když jsme vytvořili naši DinnerRepository třídy, Podívejme se na několik příkladů kódu, které ukazují běžné úkoly, které jsme s ním může dělat:

#### <a name="querying-examples"></a>Dotazování příklady

Následující kód načte jeden Dinner pomocí DinnerID hodnoty:


[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

Následující kód načte všechny nadcházející večeří a smyček nad nich:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a>INSERT a Update příklady

Následující kód ukazuje přidání dvou nových večeří. Přidání/změny do úložiště nejsou potvrdily v databázi, dokud v něm je volána metoda "Save()". Technologie LINQ to SQL zalomen všechny změny v databázové transakci – tak, aby všechny změny stát nebo žádná z nich dělat, když se uloží naše úložiště:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

Následující kód načte existující objekt Dinner a dvě vlastnosti v něm upraví. Při volání metody "Save()" v našem úložišti se změny potvrdí zpět do databáze:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

Následující kód načte dinner a pak do ní přidá odpověď. Dělá to pomocí kolekce RSVPs Dinner objektu, který technologie LINQ to SQL vytváří (protože vztahu primární key/cizího klíče mezi těmito dvěma v databázi). Tato změna je trvalá zpět do databáze jako nový řádek tabulky RSVP při volání metody "Save()" úložiště:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a>Odstranit příklad

Následující kód načte existující objekt Dinner a pak označí ho odstraníme. Při volání metody "Save()" v daném úložišti potvrdí odstranění zpět do databáze:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a>Integrace ověřování a obchodní logiku pravidla s tříd modelu

Integrace ověřování a obchodní pravidlo, že logika je klíčovou součástí každou aplikaci, která pracuje s daty.

#### <a name="schema-validation"></a>Ověřování schématu

Když tříd modelu jsou definovány pomocí LINQ to SQL návrháře, datových typů vlastností v tříd datových modelů odpovídají datové typy databázové tabulky. Příklad: Pokud je ve sloupci "EventDate" v tabulce večeří "datetime", třída modelu dat vytvořené pomocí LINQ to SQL budou typu "Datum a čas" (což je integrované datový typ .NET). To znamená, že se zobrazí chyby při kompilaci, když se pokusíte přiřadit celé číslo nebo logickou hodnotu k ní z kódu, a to vyvolá chybu automaticky při pokusu o pro implicitní převod typu není platný řetězec k němu za běhu.

Technologie LINQ to SQL budou také automaticky zpracovává útěku hodnoty SQL za vás při používání řetězců – které pomáhá chránit před útoky prostřednictvím injektáže SQL při používání.

#### <a name="validation-and-business-rule-logic"></a>Ověřování a obchodní logiku pravidla

Ověřování schématu je užitečné jako první krok, ale stačí jen zřídka. Většina scénářů reálného světa vyžaduje schopnost určit bohatší logiku ověřování, který span více vlastností, provádění kódu a sledování stavu modelu mají často (například: je to vytváří/aktualizovat/odstraněný, nebo v rámci stavu specifického pro doménu například "archivované"). Existuje široká škála různých vzorů a architektur, které slouží k definování a použití pravidel ověřování do tříd modelu a několik .NET na základě rozhraní produktům, které můžete použít pro usnadnění. Můžete použít prakticky libovolný z nich v rámci aplikace ASP.NET MVC.

Pro účely naší aplikace NerdDinner použijeme vzor relativně jednoduché a přímočaré kde zveřejňujeme IsValid vlastnost a metodu GetRuleViolations() na náš objekt modelu večeři. Vlastnost IsValid vrátí hodnotu PRAVDA nebo NEPRAVDA v závislosti na tom, jestli jsou všechny platné ověření a obchodních pravidel. Metoda GetRuleViolations() zobrazí seznam všech chyb pro pravidlo.

IsValid a GetRuleViolations() budete implementaci pro náš model Dinner přidáním "částečné třídy" do projektu. Částečné třídy lze použít k přidání metody/vlastnosti/události na třídy udržuje Návrhář VS (např. třídy Dinner generované LINQ to SQL Návrháře) a vyhnout se nástroj z messing pomocí našeho kódu. Můžeme přidat nové částečné třídy do našich projektu kliknutím pravým tlačítkem na složku \Models a zvolte příkaz "Přidat novou položku". Potom jsme můžete zvolit šablonu "Třída" v dialogovém okně "Přidat novou položku" a pojmenujte ho Dinner.cs.

![](build-a-model-with-business-rule-validations/_static/image11.png)

Kliknutím na tlačítko "Přidat" Přidat Dinner.cs soubor do projektu a otevřete ho v rámci rozhraní IDE. Potom jsme můžete implementovat s použitím rozhraní framework základní pravidla a ověření vynucení níže uvedeného kódu:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

Několik poznámek o kódu uvedeného výše:

- Třídy Dinner začíná "částečné" klíčové slovo – což znamená, že kód obsažený v něm budou kombinovat s třídou generována zachovány technologií LINQ to SQL návrháře a zkompilovány do jednu třídu.
- Třída RuleViolation je pomocná třída, kterou přidáme do projektu, který nám umožňuje poskytovat podrobnosti o porušení pravidla.
- Metoda Dinner.GetRuleViolations() způsobí, že naše ověření a obchodních pravidel, který se má vyhodnotit (jsme budete implementovat je za chvíli). Poté vrátí zpět posloupnost RuleViolation objekty, které poskytují další podrobnosti o případných chybách pravidlo.
- Vlastnost Dinner.IsValid poskytuje pohodlné pomocné vlastnost, která označuje, zda má objekt Dinner žádné aktivní RuleViolations. To lze proaktivně zkontrolovat vývojáři, kteří pomocí objektu Dinner kdykoli (a nevyvolá výjimku).
- Částečná metoda Dinner.OnValidate() je hák, která poskytuje technologii LINQ to SQL, která umožňuje být upozornění vždy, když objekt Dinner je nastavit jako trvalý, v databázi. Naše implementace OnValidate() výše zajistí, že večeře nemá žádné RuleViolations před uložením. Pokud je v neplatném stavu, vyvolá výjimku, která způsobí, že LINQ to SQL přerušení transakce.

Tento přístup poskytuje jednoduché rozhraní, které budeme integrovat ověření a obchodních pravidel do. Teď přidáme následující pravidla, která metodě GetRuleViolations():

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

Funkce "yield return" jazyka C# se používá k vrácení sekvence všechny RuleViolations. První šest pravidla kontroly nad jednoduše vynutit, že vlastnosti řetězce na náš web Dinner nemůže být null ani prázdný. Poslední pravidlo je o něco zajímavější a volání PhoneValidator.IsValidNumber() Pomocná metoda, že můžeme přidat na náš projekt a ověřte, zda ContactPhone číslo formátu shody společnosti Dinner vaší země.

Můžeme použít. Podpora NET pro regulární výraz k implementaci této telefonické ověření. Níže je jednoduchý PhoneValidator implementace, která můžeme přidat do projektu, která umožňuje přidat specifické pro zemi kontroly vzor regulárního výrazu:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a>Ověření zpracování a obchodní logiky porušení

Teď, přidali jsme ve výše uvedeném ověřování a obchodní pravidlo kódu, pokaždé, když se pokusíme vytvořit nebo aktualizovat večeři, bude náš ověřovací logiky pravidla vyhodnotí a vynucovat.

Vývojářům psát kód jako níže proaktivně určit, jestli je platný objekt Dinner a načíst seznam všechna narušení v něm bez vyvolání výjimky:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

Pokud jsme pokus o uložení večeře v neplatném stavu, bude vyvolána výjimka, když jsme volání metody Save() na DinnerRepository. K tomu dojde, vzhledem k tomu před uložením změn webu Dinner a jsme přidali kód do Dinner.OnValidate() vyvolat výjimku, pokud je porušení pravidel v společnosti Dinner technologie LINQ to SQL automaticky volá naše Dinner.OnValidate() částečnou metodu. Můžeme tuto výjimku zachytit a reaktivně načítat seznam porušení opravit:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

Vzhledem k tomu, že naše ověření a obchodních pravidel jsou implementovány v rámci naší modelu vrstvy a není ve vrstvě uživatelského rozhraní, budou se použít a použít v rámci všech scénářů v rámci naší aplikace. Můžete později změnit nebo přidat obchodní pravidla a mít veškerý kód, že funguje s objekty naše společnost Dinner případném dalším sdílení dodržovat je.

S flexibilitu při změně obchodních pravidel na jednom místě, bez nutnosti tyto změny ripple v celé aplikaci a logika uživatelského rozhraní, je znak kvalitní aplikace a výhody, které rozhraní MVC pomáhá podporovat.

### <a name="next-step"></a>Dalším krokem

Teď máme modelu, který můžeme použít jak dotazování a aktualizace databáze.

Teď přidáme několik kontrolerů a zobrazení pro projekt, který můžeme použít k vytvoření uživatelského rozhraní HTML prostředí kolem něj.

> [!div class="step-by-step"]
> [Předchozí](create-a-database.md)
> [další](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
