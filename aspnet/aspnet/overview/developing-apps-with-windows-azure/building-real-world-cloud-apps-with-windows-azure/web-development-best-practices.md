---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: Web osvědčené postupy pro vývoj (vytváření skutečných cloudových aplikací s Azure) | Dokumentace Microsoftu
author: MikeWasson
description: Vytváření reálného světa cloudových aplikací s Azure e kniha je založená na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které se dají mu...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: 930b9be35ef2e0dd85cee8f6584b9e90c80933b9
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577896"
---
<a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a>Osvědčené postupy při vývoji webové (vytváření skutečných cloudových aplikací s Azure)
====================
podle [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Petr Dykstra](https://github.com/tdykstra)

[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronickou knihu](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Vytváření reálného světa cloudových aplikací s Azure** e knihy je založena na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které vám pomůžou být úspěšný vývoj webových aplikací v cloudu. Informace o e kniha najdete v tématu [první kapitoly](introduction.md).


První tři vzory byly informace o nastavení procesu agilního vývoje. ostatní jsou o architektuře a kódu. Tohle je kolekce osvědčené postupy pro vývoj pro web:

- [Bezstavové webové servery](#stateless) za nástrojem pro vyrovnávání zatížení inteligentní.
- [Vyhněte se stav relace](#sessionstate) (nebo pokud se nemůžete vyhnout, použijte distribuované mezipaměti, nikoli databázi).
- [Použití CDN](#cdn) na hraniční mezipaměti statické souborové prostředky (obrázky, skripty).
- [Použití .NET 4.5 asynchronní podpory](#async) k zabránění blokování volání.

Tyto postupy jsou platné pro všechny vývoj webu, ne jenom pro cloudové aplikace, ale jsou obzvláště důležité pro cloudové aplikace. Fungují společně vám pomůže zajistit optimální využití vysoce flexibilní škálování nabízí cloudové prostředí. Pokud nemáte postupovat podle těchto postupů, je potřeba spustit do omezení při pokusu o škálování vaší aplikace.

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a>Bezstavové webové vrstvy za nástrojem pro vyrovnávání zatížení inteligentní

*Bezstavová webová vrstva* znamená, že neukládejte žádná data aplikací do systému webového serveru paměti nebo soubor. Udržování Bezstavová webová vrstva umožňuje poskytovat lepší prostředí pro zákazníky i ušetřit:

- Pokud je Bezstavová webová vrstva a se nachází za nástrojem pro vyrovnávání zatížení, můžete rychle reagovat na změny v provozu aplikace dynamicky přidáním nebo odebráním servery. V cloudovém prostředí, kde budete platit jenom za prostředky serveru pro za předpokladu, je skutečně používáte může překládat této schopnosti reagovat na změny v poptávce do velké úspory.
- Je Bezstavová webová vrstva architektonicky mnohem jednodušší pro horizontální navýšení kapacity aplikace. Který příliš vám umožní reagovat na potřeby škálování rychlejší a levnější utratit za vývoj a testování v procesu.
- Cloudové servery, jako je místní servery, je potřeba opravit a příležitostně; restartovat a pokud je Bezstavová webová vrstva, přesměrování provozu, pokud server přestane fungovat dočasně nebude způsobovat chyby nebo neočekávané chování.

Většina reálné aplikace potřebují k uložení stavu relace webové; hlavní bod je neukládat na webovém serveru. Můžete ukládání stavu jinými způsoby, třeba na straně klienta v souborech cookie nebo mimo proces serverové pomocí mezipaměť zprostředkovatele stavu relace ASP.NET. Můžete ukládat soubory v [Windows Azure Blob storage](unstructured-blob-storage.md) místo místního systému souborů.

Jako příklad toho, jak je snadné provést škálování aplikace ve Windows Azure Web Sites, pokud je Bezstavová webová vrstva, najdete v článku **škálování** kartu pro Windows Azure webovou stránku v portálu pro správu:

![Karta škálování](web-development-best-practices/_static/image1.png)

Pokud chcete přidat webové servery, vám stačí přetáhnout posuvník počet instancí na pravé straně. Nastavte na hodnotu 5 a klikněte na tlačítko **Uložit**, a během několika sekund máte 5 webových serverů ve Windows Azure zpracování provoz vašeho webu.

![Pět instancí](web-development-best-practices/_static/image2.png)

Stejně snadno můžete nastavit počet instancí na 3 nebo zase dolů na 1. Když horizontálně snížíte kapacitu back, začnete šetřit peníze okamžitě, protože Windows Azure se účtuje po minutách, ne po hodinách.

Můžete také říct Windows Azure a automaticky zvýšit nebo snížit počet webových serverů na základě využití procesoru. V následujícím příkladu při využití procesoru nižší než 60 %, se nejméně 2 sníží počet webových serverů, a pokud využití procesoru překročí 80 %, bude až do maximálního počtu 4 zvýšením počtu webových serverů.

![Škálování podle využití procesoru](web-development-best-practices/_static/image3.png)

Nebo co když víte, že váš web bude pouze zaneprázdněný během pracovní doby? Windows Azure ke spuštění více serverů během na denní a snížit na jeden server večerů, počet přenocování a o víkendech, můžete zjistit. Následující řadu snímky obrazovky ukazuje, jak nastavit na webu pro spouštění na jednom serveru mimo špičku a 4 servery během pracovní doby v 8: 00 do 17: 00.

![Škálování podle plánu.](web-development-best-practices/_static/image4.png)

![Nastavení časů plánu](web-development-best-practices/_static/image5.png)

![Denní plán](web-development-best-practices/_static/image6.png)

![Weeknight plán](web-development-best-practices/_static/image7.png)

![Plán víkendu](web-development-best-practices/_static/image8.png)

A samozřejmě to vše lze provést ve skriptech i na portálu.

Možnost aplikace pro horizontální navýšení kapacity je skoro neomezený počet v Windows Azure, tak dlouho, dokud byste se vyhnout překážky dynamické přidávání nebo odebírání virtuální počítače serveru, udržováním bezstavové webové vrstvy.

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a>Vyhněte se stav relace

Je často nepraktické ve skutečných cloudových aplikací, aby ukládání nějakou formu stav uživatelských relací, ale některé přístupy, vliv na výkon a škálovatelnost více než jiné. Pokud máte k uložení stavu, nejlepším řešením je zachovat malé množství stavu a uloží je v souborech cookie. Pokud to není proveditelné, další nejlepším řešením je používání stavu relace ASP.NET u poskytovatele pro [distribuované mezipaměti v paměti](distributed-caching.md#sessionstate). Nejhorší řešení z pohledu škálovatelnosti a výkonu je pro použití jiné databáze zajišťuje zprostředkovatel stavu relací.

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a>Použití CDN k ukládání do mezipaměti statické souborové prostředky

CDN je zkratka pro Content Delivery Network. Poskytovatel CDN můžete poskytovat statické souborové prostředky, jako jsou obrázky a soubory skriptů a zprostředkovatel ukládá do mezipaměti tyto soubory v datových centrech po celém světě tak, aby bez ohledu na to uživatelům přístup k aplikaci, dostanou poměrně rychlé odpovědi a nízkou latencí pro v mezipaměti prostředky. Tím zrychluje celkový čas načtení stránky a snižuje zatížení serverů webové. Sítě CDN jsou obzvláště důležité, pokud se snažíte ty, kteří je široce geograficky distribuované.

Windows Azure má CDN a ostatní sítě CDN můžete použít v aplikaci, která běží v systému Windows Azure nebo libovolné webové hostitelské prostředí.

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a>Použít k zabránění blokování volání podpory asynchronních operací v rozhraní .NET 4.5

Rozhraní .NET 4.5 rozšířené programovacích jazyků C# a VB aby mnohem jednodušší pro asynchronní zpracování úloh. Výhody asynchronního programování je jen pro paralelní zpracování situacích, například pokud chcete aktivovat více volání webové služby současně. Umožňuje webovému serveru provádět efektivněji a spolehlivé za podmínek vysoké zatížení. Webový server má jenom omezený počet vláken, které jsou k dispozici a za podmínek vysoké zatížení při všechna vlákna se používají, příchozí požadavky muset počkat, dokud se uvolnit vlákna. Pokud kód aplikace nemá zpracování úkolů, jako je databáze dotazy a volání webové služby asynchronně, několika vlákny jsou zbytečně vázané, zatímco čekáte na serveru při reakci na vstupně-výstupních operací. To omezuje objem provozu, který server může zpracovat za podmínek vysoké zatížení. S asynchronním programování, jsou uvolněny vlákna, která čekají na webové služby nebo databáze pro vrácení dat až po zpracování nových požadavků až do data přijetí. Zaneprázdněný webovém serveru stovky nebo tisíce požadavků pak může být zpracován okamžitě které by jinak čekání na vlákna uvolnit.

Jak jste viděli již dříve, je snadné snížení počtu webových serverů zpracování vašeho webu, protože se jedná o zvýšení. Proto pokud server můžete dosáhnout vyšší propustnost, není nutné jako mnohé z nich a snížit náklady, protože je budete potřebovat pro danou objemem menší počet serverů, než byste jinak.

Podpora pro asynchronní programovací model rozhraní .NET 4.5 je součástí technologie ASP.NET 4.5 pro webové formuláře, MVC a webového rozhraní API; v rozhraní Entity Framework 6 a [rozhraní API služby Windows Azure Storage](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx).

### <a name="async-support-in-aspnet-45"></a>Asynchronní podpory v technologii ASP.NET 4.5

V technologii ASP.NET 4.5 podpora pro asynchronní programování se přidal nejenom na jazyk, ale také k architektury MVC, webových formulářů a webového rozhraní API. Například metoda akce kontroleru ASP.NET MVC přijímá data z webového požadavku a předá data zobrazení, ve kterém pak vytvoří HTML k odeslání do prohlížeče. Často potřebuje metodě akce k získání dat z databáze nebo webové služby, aby bylo možné zobrazit na webové stránce nebo k uložení dat zadaný na webové stránce. V těchto scénářích je snadné vytvořit asynchronní metodu akce: místo vrácení *ActionResult* objektu, vrátíte *úloh&lt;ActionResult&gt;*  a označte metodu s *asynchronní* – klíčové slovo. Uvnitř metody při jediného řádku kódu zahajuje operace, která zahrnuje čekací doba, můžete ji označit jako klíčové slovo await.

Tady je jednoduchý akce metodu, která volá metodu úložiště pro dotaz na databázi:

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

A tady je stejnou metodu, která zpracovává asynchronní volání databáze:

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

Pod pokličkou kompilátor vygeneruje odpovídající asynchronní kód. Pokud aplikace provádí volání do `FindTaskByIdAsync`, díky ASP.NET `FindTask` žádosti a unwinds pracovního vlákna je k dispozici pro zpracování dalšího požadavku. Když `FindTask` se provádí požadavek, vlákno je restartovat, aby pokračovat ve zpracování kódu, který následuje za toto volání. Během prozatímní mezi tím, kdy `FindTask` zahájen požadavek a až je vrácená data, budete mít k dispozici dělat užitečnou práci, která jinak by spojený se čekat odpověď vlákno.

Je režijní náklady pro asynchronní kód, ale při nízké zatížení, je režijní náklady na tento zanedbatelný, při za podmínek vysoké zatížení, že jste ke zpracování požadavků, které by jinak zablokována čekáním na dostupná vlákna.

Bylo možné provádět tento druh asynchronní programování od ASP.NET 1.1, ale bylo obtížné je napsat, problematických a obtížné ladit. Teď, když jsme zjednodušili kódování pro něj v technologii ASP.NET 4.5, neexistuje žádný důvod není k tomu už.

### <a name="async-support-in-entity-framework-6"></a>Asynchronní podpory v Entity Framework 6

Jako součást podpory asynchronních operací v 4.5 jsme dodáno podpora asynchronní volání webové služby, soketů a systém souborů vstupně-výstupních operací, ale je nejběžnější vzor pro webové aplikace k volání databáze a našich knihoven data nepodporoval asynchronní. Entity Framework 6 přidává podporu async pro přístup k databázi.

Všechny metody, které způsobují dotazu nebo příkaz k odeslání do databáze v Entity Framework 6 mají asynchronní verze. Tento příklad ukazuje asynchronní verzi *najít* metody.

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

A tato asynchronní podpora funguje nejen pro vložení, odstranění, aktualizace a jednoduché najde, také funguje s dotazy LINQ:

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

Je `Async` verzi `ToList` metoda vzhledem k tomu, že v tomto kódu, který je metoda, která způsobí, že dotaz k odeslání do databáze. `Where` a `OrderByDescending` metody pouze konfigurace dotazu, zatímco `ToListAsync` metoda spustí dotaz a uloží odpovědi v `result` proměnné.

## <a name="summary"></a>Souhrn

Můžete implementovat osvědčené postupy webového vývoje podle zde uvedeného v jakékoli webové programování rozhraní a všechny cloudové prostředí, ale máme nástroje v technologii ASP.NET a Windows Azure, které usnadňuje. Pokud budete postupovat podle těchto vzorců, můžete snadno škálovat webovou vrstvu a výdajů budete minimalizovat, protože každý server bude možné aby zvládla větší provoz.

[Další kapitolu](single-sign-on.md) zkoumá, jak cloud umožňuje scénáře, jednotné přihlašování.

## <a name="resources"></a>Prostředky

Další informace najdete v následující prostředky.

Bezstavové webové servery:

- [Microsoft Patterns and Practices – pokyny k automatickému škálování](https://msdn.microsoft.com/library/dn589774.aspx).
- [Zakázání ARR Instance vztahů ve webech Windows Azure](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/). Příspěvek na blogu od Ereze Benari vysvětluje, spřažení relace v modelu weby Windows Azure.

CDN:

- [Bezporuchový: Sestavování škálovatelných, odolných cloudových služeb](https://channel9.msdn.com/Series/FailSafe). Série videí devět částí Ulrich Homann, Marc Mercuri a Mark Simms. Viz diskuze CDN v epizodě 3 počínaje 1:34:00.
- [Model Microsoft Patterns a postupy statický obsah hostování](https://msdn.microsoft.com/library/dn589776.aspx)
- [Revize CDN](http://www.cdnreviews.com/). Přehled sítě CDN za mnoho.

Asynchronní programování:

- [Použití asynchronních metod v architektuře ASP.NET MVC 4](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md). Kurz od Ricka Andersona.
- [Asynchronní programování pomocí modifikátoru Async a operátoru Await (C# a Visual Basic)](https://msdn.microsoft.com/library/vstudio/hh191443.aspx). Dokumentu MSDN white paper, který popisuje důvody pro asynchronní programování, jak to funguje v technologii ASP.NET 4.5 a tom, jak napsat kód pro jeho implementaci.
- [Entity Framework asynchronního dotazu a uložit](https://msdn.microsoft.com/data/jj819165)
- [Jak vytvořit webové aplikace ASP.NET pomocí asynchronní](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7). Videoprezentace podle Rowan Miller. Obsahuje grafické ukázku jak asynchronního programování usnadnit výrazné zvýšení propustnosti webového serveru za podmínek vysoké zatížení.
- [Bezporuchový: Sestavování škálovatelných, odolných cloudových služeb](https://channel9.msdn.com/Series/FailSafe). Série videí devět částí Ulrich Homann, Marc Mercuri a Mark Simms. Diskuze o dopadu asynchronního programování na škálovatelnost naleznete v tématu epizodě 4 a 8 epizodu.
- [Použití asynchronních metod v ASP.NET 4.5 a důležité gotcha Kouzlem](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx). Blogový příspěvek Scotta Hanselmana, především o použití modifikátoru async aplikace webových formulářů ASP.NET.

Pro další webové osvědčené postupy pro vývoj naleznete v následujících zdrojích:

- [Oprava ukázková aplikace – osvědčené postupy](the-fix-it-sample-application.md#bestpractices). Příloha k e kniha uvádí několik osvědčených postupů, které byly implementovány v aplikaci opravit.
- [Kontrolní seznam web pro vývojáře](http://webdevchecklist.com/asp.net)

> [!div class="step-by-step"]
> [Předchozí](continuous-integration-and-continuous-delivery.md)
> [další](single-sign-on.md)
