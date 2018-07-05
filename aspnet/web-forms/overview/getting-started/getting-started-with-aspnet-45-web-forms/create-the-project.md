---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
title: Vytvoření projektu | Dokumentace Microsoftu
author: Erikre
description: V této sérii kurzů se seznámíte se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a službu Microsoft Visual Studio Express 2013 pro jsme...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 2ce36f78-8ecb-4ab1-b748-6d0ab633ea3f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
msc.type: authoredcontent
ms.openlocfilehash: e5d6fa312ce0375eee5ca456aaea9f088d806cfc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384485"
---
<a name="create-the-project"></a>Vytvoření projektu
====================
podle [Erik Reitan](https://github.com/Erikre)

[Stáhněte si ukázkový projekt Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) nebo [stáhnout elektronickou knihu (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> V této sérii kurzů se seznámíte se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a službu Microsoft Visual Studio Express 2013 for Web. Visual Studio 2013 [projektu se zdrojovým kódem jazyka C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) je k dispozici v této sérii kurzů.


V tomto kurzu vytvoříte, přečtěte si a spusťte výchozí projekt v sadě Visual Studio, které vám umožní seznámit se s funkcí technologie ASP.NET. Zkontrolujte také, prostředí Visual Studio.

## <a name="what-youll-learn"></a>Co se dozvíte:

- Jak vytvořit nový projekt webových formulářů.
- Struktura souborů projektu webových formulářů.
- Jak spustit projekt v sadě Visual Studio.
- Různé funkce aplikace výchozí webových formulářů.
- Některé základní informace o tom, jak pomocí prostředí Visual Studio.

## <a name="creating-the-project"></a>Vytvoření projektu

1. Otevřít Visual Studio.
2. Vyberte **nový projekt** z **souboru** nabídky v sadě Visual Studio. 

    ![Vytvoření projektu – nové položky nabídky projektu](create-the-project/_static/image1.png)
3. Vyberte **šablony**  - &gt; **Visual C#**  - &gt; **webové** šablony skupiny na levé straně.
4. Zvolte **webová aplikace ASP.NET** šablon v prostředním sloupci.  
 V této sérii kurzů používá rozhraní .NET Framework 4.5.2.
5. Pojmenujte svůj projekt *Northwind* a zvolte **OK** tlačítko. 

    ![Vytvoření projektu – dialogové okno nového projektu](create-the-project/_static/image2.png)

    > [!NOTE]
    > Název projektu v této sérii kurzů je **Northwind**. Je doporučeno používat toto *přesné* název projektu tak, aby kód k dispozici v celé řadě kurzů funkce podle očekávání.

6. Klikněte na tlačítko **změna ověřování** tlačítko. Vyberte **jednotlivé uživatelské účty** a klikněte na tlačítko **OK** tlačítko.

7. Vyberte **webových formulářů** šablony a kliknutím **OK** tlačítko.

    ![Vytvoření projektu – nové šablony projektu](create-the-project/_static/image3.png)

Projekt bude trvat nějakou dobu vytvoření. Až to bude připravené, otevřete **Default.aspx** stránky.

![Vytvoření projektu – nové šablony projektu](create-the-project/_static/image4.png)

Můžete přepínat mezi **návrhu** zobrazení a **zdroj** zobrazení výběrem možnosti v dolní části okna center. **Návrh** zobrazení webových stránek ASP.NET, stránky předlohy, obsahu stránky, stránky HTML a uživatelské ovládací prvky zobrazení téměř WYSIWYG. **Zdroj** zobrazí kód HTML pro vaši webovou stránku, kterou můžete upravovat.

> [!TIP] 
> 
> **Pochopení architektury ASP.NET**
> 
> Webové formuláře ASP.NET umožňuje sestavení dynamické weby s využitím známý model přetažení myší, založené na událostech. Návrhová plocha a stovky ovládacích prvků a komponent vám umožní rychle vytvořit sofistikované, výkonné uživatelského rozhraní – weby s přístup k datům. Store Slonovi Wingtip vychází z technologie ASP.NET webové formuláře, ale mnoho popsaných konceptů, které se naučíte v této řadě kurzů se vztahují na všechny technologie ASP.NET.
> 
> Technologie ASP.NET nabízí čtyři primární vývojářských platforem:
> 
> - [Webové formuláře ASP.NET](../../../index.md)  
>  Rozhraní webových formulářů, zaměřuje vývojáři, kteří dávají přednost deklarativní a založené na ovládacím prvku programování, jako je například Microsoft Windows Forms (WinForms) a WPF/XAML nebo Silverlight. Nabízí modelu WYSIWYG vývoj založený na Návrhář, obvykle se s vývojáři, kteří hledají prostředí pro vývoj (RAD) rychlé aplikace pro vývoj pro web. Pokud jsou webové programování a jste obeznámeni s tradiční Microsoft RAD klienta. vývojové nástroje (například pro Visual Basic a Visual C#), můžete rychle vytvořit webovou aplikaci bez nutnosti prostředí v kódu HTML a JavaScript.
> - [ASP.NET MVC](../../../../mvc/index.md)  
>  ASP.NET MVC, zaměřuje vývojáři, kteří mají zájem o vzory a zásad, jako je vývoj řízený testováním, oddělení oblastí zájmu, IOC (Inversion) ovládacího prvku a injektáž závislostí (DI). Toto rozhraní umožňuje oddělení vrstvy obchodní logiky webové aplikace z jeho prezentační vrstvy.
> - [Webové stránky ASP.NET](../../../../web-pages/index.md)  
>  Rozhraní ASP.NET Web Pages, zaměřuje vývojáře, kteří chtějí příběhu jednoduchého webového vývoje podle jazyka PHP. V modelu webové stránky stránky HTML, vytvořit a poté přidejte serverových kód na stránku aby bylo možné dynamicky řízení způsobu vykreslení značek. Webové stránky je navržená speciálně být jednoduché rozhraní framework a je nejjednodušší vstupní bod do technologie ASP.NET pro uživatele, kteří znalost jazyka HTML, ale nemusí mít různé programovací prostředí – například, studenty nebo nadšence. Je také vhodný způsob pro webové vývojáře, kteří ví, PHP nebo podobné rozhraní, pokud chcete začít používat technologie ASP.NET.
> - [ASP.NET jedním jednostránková aplikace](../../../../single-page-application/index.md)  
>  ASP.NET jedním jednostránková aplikace (SPA) pomáhá vytvářet aplikace, které obsahují důležité interakce na straně klienta pomocí HTML 5, CSS 3 a JavaScript. ASP.NET a Web Tools 2012.2 Update dodává novou šablonu pro vytváření jednostránkové aplikace pomocí rozhraní knockout.js a ASP.NET Web API. Kromě nové šablony SPA nové komunitou vytvořených SPA šablony jsou také k dispozici ke stažení.
> 
> Kromě čtyři hlavní vývojářských platforem ASP.NET nabízí také další technologie, které jsou důležité vědět a znát, ale nejsou zahrnuté v této řadě kurzů:
> 
> - [Rozhraní ASP.NET Web API](../../../../web-api/index.md) – architektura pro vytváření služeb HTTP, které jsou poskytovány širokému spektru klientů, včetně prohlížečů a mobilních zařízení.
> - [Funkce SignalR technologie ASP.NET](../../../../signalr/index.md) -knihovnu, která usnadňuje vývoj funkcí v reálném čase.


### <a name="reviewing-the-project"></a>Kontrola projektu

V sadě Visual Studio **Průzkumníka řešení** okno umožňuje spravovat soubory projektu. Pojďme se podívat na složky, které byly přidány do vaší aplikace v **Průzkumníka řešení**. Šablony webové aplikace přidá strukturu základní složky:

![Vytvoření projektu – Průzkumník řešení](create-the-project/_static/image5.png)

Visual Studio vytvoří některé počáteční složky a soubory pro váš projekt. První soubory, které můžete pracovat s později v tomto kurzu jsou následující:

| **Soubor** | **Účel** |
| --- | --- |
| *Default.aspx* | Obvykle první stránka zobrazí, když aplikace běží v prohlížeči. |
| *Site.Master* | Stránka, která vám umožní vytvořit konzistentní chování rozložení a použijte standardní stránek ve vaší aplikaci. |
| *Global.asax* | Volitelný soubor, který obsahuje kód pro reakce na úrovni aplikace nebo relace událostí vyvolaných pomocí technologie ASP.NET nebo moduly protokolu HTTP. |
| *Web.config* | Konfigurační data pro aplikaci. |

### <a name="running-the-default-web-application"></a>Spuštěný výchozí webové aplikace

Výchozí webová aplikace poskytuje bohaté uživatelské prostředí na základě vestavěnou funkci a podporu. Aplikace bez nutnosti jakkoli měnit výchozí projekt webových formulářů, je připraven ke spuštění na místním webovém prohlížeči.

1. Stisknutím klávesy ***F5*** klávesu v sadě Visual Studio.   
 Aplikace bude sestavení a zobrazit ve webovém prohlížeči.  

    ![Vytvoření projektu – výchozí stránka](create-the-project/_static/image6.png)
2. Jakmile budete mít dokončené kontroly spuštěné aplikaci, zavřete okno prohlížeče.

Existují tři hlavní stránky v této výchozí webové aplikace: *Default.aspx* (domů), *About.aspx*, a *Contact.aspx*. Každá z těchto stránek dosažitelný z horního navigačního panelu. Existují také dvě další stránky obsažené ve složce účtu, Register.aspx stránku a stránku Login.aspx. Tyto dvě stránky umožňují používat funkce členství technologie ASP.NET vytvořit, uložit a ověřit přihlašovací údaje uživatele.

## <a name="aspnet-web-forms-background"></a>Webové formuláře ASP.NET na pozadí

Webové formuláře ASP.NET jsou stránky, které jsou založeny na technologii Microsoft ASP.NET, ve kterém kód, který běží na serveru dynamicky generuje webové stránky výstup do prohlížeče nebo klienta zařízení. Na stránce technologie ASP.NET webové formuláře automaticky vykreslí správné kompatibilní s prohlížečem kód HTML pro funkce, jako je styly, rozložení a tak dále. Webové formuláře jsou kompatibilní s libovolný jazyk podporuje .NET common language runtime, jako je například Microsoft Visual Basicu a Microsoft Visual C#. Také, využívající webové formuláře [rozhraní Microsoft .NET Framework](https://msdn.microsoft.com/vstudio/aa496123), která poskytuje výhody, jako je například spravovaném prostředí, bezpečnost typů a dědičnosti.

Při spuštění stránky webových formulářů ASP.NET, stránka prochází životní cyklus, ve kterém provede série kroků zpracování. Tyto kroky zahrnují inicializace vytváření instancí ovládací prvky, obnovení a udržování stavu, spouštění kódu obslužné rutiny události a vykreslování. Jak se podrobněji seznamujete s výkonem webových formulářů ASP.NET, je důležité porozumět [životního cyklu stránky ASP.NET](https://msdn.microsoft.com/library/ms178472(v=vs.100).aspx) tak, aby ve fázi odpovídající životního cyklu pro efekt je, že můžete napsat kód.

Když webový server přijme požadavek na stránku, ho najde na stránce, procesy, odešle do prohlížeče a poté odstraní všechny informace o stránce. Pokud uživatel požaduje znovu stejná stránka, server zopakuje celé sekvenci stránku od začátku. Jinými slovy, je na serveru stránek paměti, že má zpracování stránky jsou bezstavové. Rámec stránky ASP.NET automaticky zpracovává Správa stavu stránky a ovládacích prvků a poskytne vám explicitní způsoby, jak udržovat stav informace specifické pro aplikaci.

> [!TIP] 
> 
> **Funkce webových aplikací v šabloně aplikace webových formulářů**
> 
> Šablona aplikace webových formulářů ASP.NET poskytuje bohatou sadu integrovaných funkcí. To poskytuje nejen vám *Home.aspx* stránky, *About.aspx* stránky, *Contact.aspx* stránce, ale také zahrnuje funkce členství, který registruje uživatele a uloží jejich pověření tak, aby se můžete přihlásit na web. Tento přehled poskytuje další informace o některých funkcích obsažené v šabloně aplikace webových formulářů ASP.NET a jak se používají v aplikaci Wingtip Toys.
> 
> **Členství**
> 
> [ASP.NET](https://msdn.microsoft.com/library/yh26yfzy.aspx) identit ukládá přihlašovací údaje uživatelů v databázi vytvořené aplikací. Pokud vaši uživatelé přihlásí, aplikace ověřuje přihlašovacích údajů tak, že databáze pro čtení. Váš projekt *účet* složka obsahuje soubory, které implementují jednotlivé součásti členství: registrace, přihlášení, změna hesla a autorizaci přístupu. Kromě toho webových formulářů ASP.NET podporuje protokol OAuth a OpenID. Tato vylepšení ověřování umožňuje uživatelům přihlášení na webu pomocí existujících přihlašovacích údajů, z těchto účtů jako Facebook, Twitter, Windows Live a Googlu.
> 
> ![Vytvoření projektu – Průzkumník řešení (ASP.NET Identity)](create-the-project/_static/image7.png)
> 
> Ve výchozím nastavení šablona vytvoří databázi členství pomocí výchozí název databáze v instanci systému SQL Server Express LocalDB, vývoj pro databázový server, který je součástí sady Visual Studio Express 2013 for Web.
> 
> **SQL Server Express LocalDB**
> 
> [SQL Server Express LocalDB](https://technet.microsoft.com/library/hh510202.aspx) je Odlehčená verze systému SQL Server, který obsahuje mnoho funkcí programovatelnosti databáze systému SQL Server. SQL Server Express LocalDB běží v uživatelském režimu a je rychlá, bez nutnosti konfigurace instalace, který obsahuje krátký seznam požadavky na instalaci. V systému Microsoft SQL Server, všechny databáze nebo kód Transact-SQL můžete přesunout z SQL serveru Express LocalDB na SQL Server a SQL Azure bez jakýchkoli kroků upgradu. Ano SQL Server Express LocalDB slouží jako prostředí pro vývojáře pro aplikace pro všechny edice systému SQL Server. SQL Server Express LocalDB umožňuje například uložených procedur, uživatelsky definované funkce a agregace, integrace rozhraní .NET Framework, prostorové typy a jiné funkce, které nejsou k dispozici v systému SQL Server Compact.
> 
> **Stránky předlohy**
> 
> [Hlavní stránku ASP.NET](https://msdn.microsoft.com/library/wtxbf3hh.aspx) definuje konzistentní vzhled a chování pro všechny stránky v aplikaci. Rozložení stránky předlohy se sloučí s obsahem z k vytvoření na poslední stránce, která uživateli se zobrazí jednotlivé stránky obsahu. V aplikaci Wingtip Toys změníte *Site.master* stránku předlohy tak, aby všechny stránky webu na adresář Wingtip Toys sdílet stejný rozlišovací logo a navigační panel.
> 
> **HTML5**
> 
> Šablona aplikace webových formulářů ASP.NET podporuje [HTML5](http://www.w3schools.com/html/html5_intro.asp), což je nejnovější verze značky jazyka HTML. HTML5 podporuje nové prvky a funkce, které usnadňují vývoj webů.
> 
> **Modernizr**
> 
> Pro prohlížeče, které nepodporují HTML5, můžete použít [Modernizr](http://www.modernizr.com/). Modernizr je open source knihovna jazyka JavaScript, který může zjistit, jestli prohlížeč podporuje funkce HTML5 a je povolit, pokud tomu tak není. V šabloně aplikace webových formulářů ASP.NET Modernizr nainstalován jako balíček NuGet.
> 
> **Bootstrap**
> 
> Použijte šablony projektů Visual Studio 2013 [Bootstrap](http://getbootstrap.com/), rozložení a motivy rozhraní vytvořené Twitter. K zajištění přizpůsobivý návrh, což znamená, že rozložení můžete dynamicky přizpůsobit velikosti okna jiný prohlížeč používá Bootstrap CSS3. Funkce motivů Bootstrap můžete také snadno provést změnu v aplikačním vzhled a chování. Výchozí šablony webové aplikace ASP.NET v sadě Visual Studio 2013 obsahuje Bootstrap jako balíček NuGet.
> 
> **Balíčky NuGet**
> 
> Šablona aplikace webových formulářů technologie ASP.NET obsahuje sadu [NuGet](http://www.nuget.org/) balíčky. Tyto balíčky komponentní nakonfigurovánu ve formě open source knihoven a nástrojů. Není k dispozici nejrůznější balíčky vám pomůžou vytvářet a testovat aplikace. Visual Studio umožňuje snadno přidat, odebrat a aktualizovat balíčky NuGet. Vývojáři mohou vytvářet a také přidat balíčky NuGet.
> 
> ![Vytvoření projektu – dialogové okno NuGet](create-the-project/_static/image8.png)
> 
> Při instalaci balíčku NuGet zkopíruje soubory do svého řešení a automaticky provede všechny změny, jsou potřeba, jako je například přidávání odkazů a změna konfigurace související s vaší webovou aplikací. Pokud se rozhodnete k odebrání knihovny NuGet odebere soubory a vrátí zpět všechny změny provedené ve vašem projektu tak, aby je ponecháno žádné nepořádku. Správce balíčků NuGet je k dispozici **nástroje** nabídky v sadě Visual Studio.
> 
> **jQuery**
> 
> [jQuery](http://jquery.com/) je rychlý a stručné knihovna jazyka JavaScript, která zjednodušuje jejich dokumentu HTML, zpracování událostí, animace a interakce Ajax pro vývoj pro web rychlé. Knihovna jQuery JavaScript je součástí šablony aplikace webových formulářů ASP.NET jako balíček NuGet.
> 
> **Nerušivý ověření**
> 
> Program pro ověření integrované ovládací prvky jsou nakonfigurované pro účely logiky ověřování na straně klienta nerušivý JavaScript. To významně snižuje počet JavaScript nevykreslují jako vložené v kódu stránky a snižuje celkové velikosti stránky. Ověření nerušivého globálně přidá k šabloně aplikace webových formulářů ASP.NET na základě nastavení v &lt;appSettings&gt; elementu *Web.config* souboru v kořenovém adresáři aplikace.
> 
> **Entity Framework Code First**
> 
> Kromě funkcí v šabloně aplikace webových formulářů ASP.NET, aplikace Wingtip Toys používá [Entity Framework Code First](https://weblogs.asp.net/scottgu/archive/2010/12/08/announcing-entity-framework-code-first-ctp5-release.aspx), což je knihovna NuGet, který umožňuje vývoj zaměřením na kód, při práci s daty. Jednoduše řečeno, vytvoří databázi část aplikace založené na kódu, který píšete. Používá nástroj Entity Framework, načítání a manipulaci s daty jako objektů se silným typem. Díky tomu se můžete soustředit na obchodní logiku v aplikaci spíše než o jak datům přistupuje.
> 
> Další informace o nainstalovaných knihoven nebo balíčků, které jsou součástí šablony webových formulářů ASP.NET naleznete v seznamu nainstalovaných balíčků NuGet. Chcete-li to provést, v sadě Visual Studio vytvořte nový projekt webových formulářů, vyberte **nástroje**  - &gt; **Správce balíčků knihoven**  - &gt; **spravovat Balíčky NuGet pro řešení**a vyberte **nainstalované balíčky** v **spravovat balíčky NuGet** dialogové okno.


### <a name="touring-visual-studio"></a>Uznávaný sady Visual Studio

Primární windows v sadě Visual Studio zahrnují **Průzkumníka řešení**, **Průzkumníka serveru** (**Průzkumník databáze** v Express), **vlastnosti Okno**, **nástrojů**, **nástrojů**a **okno dokumentu**.

![Vytvoření projektu – dialogové okno NuGet](create-the-project/_static/image9.png)

Další informace o sadě Visual Studio najdete v tématu [vizuální průvodce pro aplikaci Visual Web Developer](https://msdn.microsoft.com/library/ee410104.aspx).

## <a name="summary"></a>Souhrn

V tomto kurzu jste vytvořili, zkontrolovat a spustit výchozí webovou aplikaci webových formulářů. Zkontrolovat různých funkcí aplikace webových formulářů výchozí a zjistili některé základní informace o tom, jak pomocí prostředí Visual Studio. V následujících kurzech bude vytvoření vrstvy přístupu k datům.

## <a name="additional-resources"></a>Další prostředky

[Volba správného programovacího modelu](../../../videos/how-do-i/choosing-the-right-programming-model.md)   
[Web Application Projects versus Web Site Projects](https://msdn.microsoft.com/library/dd547590.aspx)   
[Webové formuláře ASP.NET stránky přehled](https://msdn.microsoft.com/library/428509ah.aspx)

> [!div class="step-by-step"]
> [Předchozí](introduction-and-overview.md)
> [další](create_the_data_access_layer.md)
