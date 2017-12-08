---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: "Webové osvědčené postupy pro vývoj (vytváření reálných cloudových aplikací s Azure) | Microsoft Docs"
author: MikeWasson
description: "Cloudové aplikace skutečné World sestavení s Azure elektronická kniha je založena na prezentace vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které můžete mu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: a40a3779ddc416e141dd27b665f43830a43590b1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a>Vývoj webové osvědčených postupů (vytváření reálných cloudových aplikací s Azure)
====================
podle [Karel Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [tní Dykstra](https://github.com/tdykstra)

[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronická kniha](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Vytváření reálného světa cloudových aplikací s Azure** elektronická kniha je založena na prezentaci vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které vám mohou pomoci být úspěšná, vývoj webových aplikací pro cloud. Informace o elektronická kniha najdete v tématu [první kapitoly](introduction.md).


První tři vzory byly o nastavení procesu agile vývoj; zbývající jsou o architektuře a kódu. Tato funkce je kolekce osvědčené postupy pro vývoj pro web:

- [Bezstavové webové servery](#stateless) za službou Vyrovnávání zatížení inteligentní.
- [Vyhněte se stav relace](#sessionstate) (nebo pokud se nelze vyhnout ji, použijte distribuované mezipaměti, nikoli databáze).
- [Používají název CDN](#cdn) na hraniční mezipaměti statických souborů prostředků (obrázky, skripty).
- [Použití rozhraní .NET 4.5 asynchronní podpora](#async) k zabránění blokování volání.

Tyto postupy jsou platné pro všechny vývoj webů, ne jenom pro cloudové aplikace, ale jsou obzvláště důležité pro cloudové aplikace. Fungují společně vám pomůže zajistit optimální používání vysoce flexibilní škálování cloudové prostředí nabídnuté. Pokud nemáte použijte tyto postupy, spustíte do omezení při pokusu o škálování aplikace.

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a>Bezstavové webová vrstva za službou Vyrovnávání zatížení inteligentní

*Bezstavové webová vrstva* znamená neukládejte žádná data aplikací v systému webového serveru paměti nebo souboru. Zachování webová vrstva bezstavové umožňuje zajistit lepší prostředí zákazníků i ušetřit peníze:

- Pokud je webová vrstva bezstavové a se nachází za službou Vyrovnávání zatížení, můžete rychle reagovat na změny v provozu aplikací dynamicky přidáním nebo odebráním serverů. V cloudovém prostředí, kde platíte jenom pro prostředky serveru pro tak dlouho, dokud je ve skutečnosti použijete můžete tuto schopnost reagovat na změny požadavků na převede na velmi velké úspory.
- Bezstavové webovou vrstvu je z pohledu architektury mnohem jednodušší pro horizontální škálování aplikace. Který příliš vám umožní reagovat na škálování potřebám rychleji a méně investovat do vývoje a testování v procesu.
- Cloudové servery, jako na místní servery, je třeba opravit a restartovat občas; a pokud se webová úroveň bezstavové, přesměrování provozu, kdy server přestane fungovat dočasně nezpůsobí chyby nebo neočekávanému chování.

Většina aplikací reálného potřebují k uložení stavu relace webu; hlavní bod je neukládat je na webovém serveru. Můžete uložit stav jinými způsoby, například v klienta v souborech cookie nebo mimo proces straně serveru ve stavu relace ASP.NET pomocí zprostředkovatele mezipaměti. Můžete ukládat soubory v [Windows Azure Blob storage](unstructured-blob-storage.md) namísto místního systému souborů.

Jako příklad, jak je snadné škálování aplikace v systému Windows Azure Web Sites webovou vrstvu, bezstavové, najdete v článku **škálování** kartě pro webu systému Windows Azure v portálu pro správu:

![Karta škálování](web-development-best-practices/_static/image1.png)

Pokud chcete přidat webové servery, můžete přetáhnout posuvníku počet instance právě vpravo. Nastavte na hodnotu 5 a klikněte na tlačítko **Uložit**, a během několika sekund máte 5 webové servery v systému Windows Azure zpracování provoz vašeho webu.

![Pět instancí](web-development-best-practices/_static/image2.png)

Stejně snadno můžete nastavit počet instancí dolů 3 nebo zpět do 1. Při změně měřítka zpět, spustíte tak šetřit peníze okamžitě, protože Windows Azure účtuje o minutu, ne podle hodin.

Windows Azure pro automatické zvýšení nebo snížení počtu webových serverů na základě využití procesoru, můžete zjistit také. V následujícím příkladu kdy přestane využití procesoru nižší než 60 %, se sníží počet webových serverů na nejméně 2 a pokud využití procesoru překročí 80 %, až do maximálního počtu 4 se zvýší počet webových serverů.

![Škálování podle využití procesoru](web-development-best-practices/_static/image3.png)

Nebo co když víte, že váš web bude pouze zaneprázdněn během pracovní doby? Windows Azure a používat víc serverů během na denní a snížit na jednom serveru večerů, počet přenocování a víkendů poznáte. Následující řadu snímky obrazovky ukazuje, jak nastavit webu pro spouštění na jednom serveru mimo špičku a 4 servery během pracovní doby od 8: 00 do 17: 00.

![Škálování podle plánu.](web-development-best-practices/_static/image4.png)

![Nastavení časů plánu](web-development-best-practices/_static/image5.png)

![Denní plán](web-development-best-practices/_static/image6.png)

![Plán weeknight](web-development-best-practices/_static/image7.png)

![Plán víkendu](web-development-best-practices/_static/image8.png)

A samozřejmě všechny to lze provést ve skriptech i na portálu.

Možnost aplikace pro rozšíření Škálováním je skoro neomezený v systému Windows Azure, tak dlouho, dokud se vyhnout překážky pro dynamicky přidáním nebo odebráním virtuální počítače serveru, tak webovou vrstvu bezstavové.

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a>Vyhněte se stav relace

Je často není praktické v reálných cloudové aplikace předejdete ukládání určitou formu stavu pro uživatelské relace, ale některé přístupy vliv na výkon a škálovatelnost více než jiné. Pokud máte k uložení stavu, je nejlepším řešením zachovat malé množství stavu a ukládat v souborech cookie. Pokud se nejedná o to vhodné, další nejlepším řešením je používání stavu relace ASP.NET u poskytovatele pro [v paměti, distribuované mezipaměti](distributed-caching.md#sessionstate). Nejhorší řešení z hlediska škálovatelnosti a výkonu, je použít databázi založenou na poskytovatele stavu relace.

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a>Používají název CDN pro ukládání do mezipaměti statických souborů prostředků

CDN je zkratka pro Content Delivery Network. Zadejte statický soubor prostředky jako obrázků a souborů skriptů k poskytovateli CDN a zprostředkovatel ukládá do mezipaměti tyto soubory v datových centrech po celém světě tak, aby bez ohledu na osoby přístup k aplikaci, získají poměrně rychle odpovědi a s nízkou latencí pro uložená v mezipaměti prostředky. To urychluje celkový čas načítání stránky a snižuje se zatížení na webové servery. Sítím CDN jsou zvlášť důležité, pokud se snažíte široce geograficky distribuované cílovou skupinu.

Windows Azure má název CDN a dalších sítím CDN můžete použít v aplikaci, která běží v systému Windows Azure nebo webhosting prostředí.

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a>Použití rozhraní .NET 4.5 asynchronní podpora k zabránění blokování volání

Rozhraní .NET 4.5 rozšířené programovacích jazyků C# a VB aby bylo možné zpracovávat úlohy asynchronně mnohem jednodušší. Výhodou asynchronní programování není právě pro paralelní zpracování situace, jako je například když chcete současně ji více volání webové služby. Umožňuje webovému serveru provádět efektivněji a spolehlivé podmínky vysokého zatížení. Webový server má jenom omezený počet vláken, které jsou k dispozici, a podmínky vysokého zatížení při všech vláken se používají, příchozí požadavky nutné čekat, až se uvolní vláken. Pokud kód aplikace nemůže pracovat s úkoly, jako jsou databáze dotazy a volání webové služby asynchronně, velký počet vláken jsou zbytečně svázané, zatímco server čeká odpověď vstupně-výstupní operace. Toto nastavení omezuje objem provozu, který server dokáže zpracovat podmínky vysokého zatížení. Asynchronní programování s vláken, které čekají na webové služby nebo databázi vrátit data jsou uvolněny až nové žádosti o služby dokud dat. přijetí. V zaneprázdněn webového serveru stovkami nebo tisíci požadavky pak budou zpracovány neprodleně které by jinak čekání na vláken být uvolněna.

Jak už jste viděli dříve, je jako snadné snížení počtu webových serverů, jak se má zvýšit jejich zpracování webové stránky. Takže pokud server můžete dosáhnout větší propustnost, není třeba jako řadu je a snížit náklady, protože potřebujete menší počet serverů pro danou provoz svazek než jinak by.

Podpora pro asynchronní programovací model rozhraní .NET 4.5 je součástí technologie ASP.NET 4.5 pro webových formulářů, MVC a webového rozhraní API; v rozhraní Entity Framework 6 a [rozhraní API systému Windows Azure Storage](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx).

### <a name="async-support-in-aspnet-45"></a>Asynchronní podpora v technologii ASP.NET 4.5

V technologii ASP.NET 4.5 podpora pro asynchronní programování se přidal nejenom na jazyk, ale také architektury MVC, webových formulářů a webového rozhraní API. Například metoda akce kontroleru rozhraní ASP.NET MVC přijímá data z webové žádosti a předá data zobrazení, která vytvoří HTML k odeslání do prohlížeče. Metoda akce je často potřeba získat data z databáze nebo webové službě, aby bylo možné zobrazit na webové stránce nebo k uložení dat zadaný na webové stránce. V těchto případech je snadno provádět asynchronní metody akce: místo návrat *ActionResult* objektu, vrátíte *úloh&lt;ActionResult&gt;*  a označit metody pomocí *asynchronní* – klíčové slovo. Uvnitř metody při řádek kódu se spustí operace, která zahrnuje čekací dobu, můžete ji označit with – klíčové slovo await.

Zde je jednoduchý akce metodu, která volá metodu úložiště pro dotaz na databázi:

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

A tady je stejnou metodu, která zpracovává volání databáze asynchronně:

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

Kompilátor skrytě generuje kód odpovídající asynchronní. Při aplikace provede volání `FindTaskByIdAsync`, díky ASP.NET `FindTask` žádosti a unwinds pracovní vlákno zpřístupní zpracovat další žádost. Když `FindTask` se provádí požadavek, vlákno je restartovat, aby pokračovat ve zpracovávání kód, který následuje za toto volání. Během provizorní mezi tím, kdy `FindTask` inicializuje požadavek a pokud se vrátí data, máte k dispozici pro práci se užitečné, který jinak by být vázáno až čekání na odpověď vlákna.

Je některé režijní náklady pro asynchronní kód, ale podmínky nízké zatížení, je tento režijní náklady na zanedbatelný, při v podmínkách vysokým zatížením, které budete moct zpracovávat požadavky, které by jinak zablokována čekáním dostupných vláken.

Bylo možné provést tento druh asynchronní programování od verze 1.1 ASP.NET, ale bylo obtížné napsat, problematických a obtížně se ladění. Teď, když jsme si zjednodušená kódování pro něj v technologii ASP.NET 4.5, neexistuje žádný důvod nechcete provést už.

### <a name="async-support-in-entity-framework-6"></a>Asynchronní podpora v Entity Framework 6

V rámci asynchronní podpora v 4.5 jsme dodaný podpora asynchronní volání webové služby, sockets a systém souborů vstupně-výstupních operací, ale Nejběžnější vzor pro webové aplikace je narazila na databázi a naše data knihovny nepodporovala asynchronní. Nyní Entity Framework 6 přidává podporu async pro přístup k databázi.

Všechny metody, které způsobily dotazu nebo příkaz k odeslání do databáze v Entity Framework 6 mají asynchronní verze. Tady příklad ukazuje asynchronní verzi *najít* metoda.

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

A tato asynchronní podpora funguje nejen pro vložení, odstranění, aktualizace a jednoduchý najde, spolupracuje také s dotazy LINQ:

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

Došlo `Async` verzi `ToList` metoda vzhledem k tomu, že v tomto kódu, který je metoda, která způsobí, že dotaz k odeslání do databáze. `Where` a `OrderByDescending` metody konfigurovat jenom dotaz, při `ToListAsync` metoda provede daný dotaz a ukládá odpovědi v `result` proměnné.

## <a name="summary"></a>Souhrn

Můžete implementovat osvědčené postupy webové vývoj podle zde uvedeného v jakékoli webové programování framework a jakékoli cloudové prostředí, ale máme nástroje v technologii ASP.NET a Windows Azure, abyste usnadnili snadnou. Pokud budete postupovat podle tyto vzory, můžete snadno škálovat webovou vrstvu a budete minimalizovat vaše náklady, protože každý server bude moct zvládli větší provoz.

[Další kapitoly](single-sign-on.md) zjistí jak cloudu umožňuje scénářů přihlašování.

## <a name="resources"></a>Prostředky

Další informace najdete v následujících materiálech.

Bezstavové webové servery:

- [Microsoft Patterns and Practices - automatické škálování pokyny](https://msdn.microsoft.com/en-us/library/dn589774.aspx).
- [Zakázání ARR Instance spřažení v systému Windows Azure weby](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/). Příspěvek blogu podle Erez Benari vysvětluje spřažení relace v weby systému Windows Azure.

CDN:

- [Bezporuchový: Vytváření škálovatelné, odolné cloudové služby](https://channel9.msdn.com/Series/FailSafe). Série videí devět částí Ulrich Homann, Mercuri matolin a moduly SIMM značky. Přečtěte si diskuzi CDN v díl 3 počínaje 1:34:00.
- [Vzor Microsoft Patterns a postupy statického obsahu hostování](https://msdn.microsoft.com/en-us/library/dn589776.aspx)
- [CDN recenze](http://www.cdnreviews.com/). Přehled mnoho sítím CDN.

Asynchronní programování:

- [Použití asynchronních metod v architektuře ASP.NET MVC 4](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md). Kurz od Ricka Andersona.
- [Asynchronní programování pomocí modifikátoru Async a operátoru Await (C# a Visual Basic)](https://msdn.microsoft.com/en-us/library/vstudio/hh191443.aspx). MSDN dokumentu white paper, popisuje důvody asynchronní programování, jak to funguje v technologii ASP.NET 4.5 a jak napsat kód pro implementaci.
- [Entity Framework asynchronní dotaz a uložit](https://msdn.microsoft.com/en-us/data/jj819165)
- [Postup vytvoření webové aplikace ASP.NET pomocí modifikátoru Async](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7). Video prezentace podle Rowan Lukeš. Zahrnuje grafické ukázce jak asynchronního programování může usnadnit výraznému nárůstu propustnosti webového serveru podmínky vysokého zatížení.
- [Bezporuchový: Vytváření škálovatelné, odolné cloudové služby](https://channel9.msdn.com/Series/FailSafe). Série videí devět částí Ulrich Homann, Mercuri matolin a moduly SIMM značky. Diskuze o vlivu asynchronního programování na škálovatelnost najdete v části díl 4 a díl 8.
- [Magic použití asynchronních metod v technologii ASP.NET 4.5 a důležité gotcha](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx). Příspěvek blogu Scotta Hanselmana, kde, hlavně o použití modifikátoru async v aplikace webových formulářů ASP.NET.

Pro více webových vývoj osvědčené postupy najdete v následujících materiálech:

- [Oprava je ukázková aplikace - osvědčené postupy](the-fix-it-sample-application.md#bestpractices). Příloha do této e knihy uvádí počet osvědčené postupy, které byly implementovány v aplikaci, opravte ji.
- [Kontrolní seznam webových vývojářů](http://webdevchecklist.com/asp.net)

>[!div class="step-by-step"]
[Předchozí](continuous-integration-and-continuous-delivery.md)
[další](single-sign-on.md)
