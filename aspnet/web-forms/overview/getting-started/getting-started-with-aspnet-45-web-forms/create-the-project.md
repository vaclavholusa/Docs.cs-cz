---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
title: "Vytvořte projekt | Microsoft Docs"
author: Erikre
description: "Tento kurz řady naučit se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a Microsoft Visual Studio Express 2013 pro jsme..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 2ce36f78-8ecb-4ab1-b748-6d0ab633ea3f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
msc.type: authoredcontent
ms.openlocfilehash: 094733dcbe31486385dda2f8b44ba77a17486c82
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="create-the-project"></a>Vytvoření projektu
====================
Podle [Erik Reitan](https://github.com/Erikre)

[Stáhnout adresář Wingtip Toys ukázkového projektu (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) nebo [stáhnout elektronická kniha (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Tento kurz řady naučit se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a Microsoft Visual Studio Express 2013 pro Web. Visual Studio 2013 [projekt pomocí zdrojového kódu C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) dispozici je pro tento kurz řady.


V tomto kurzu vytvoříte, přečtěte si a spustit výchozí projekt v sadě Visual Studio, které vám umožní seznámit se s funkcí technologie ASP.NET. Navíc zkontrolujete prostředí Visual Studio.

## <a name="what-youll-learn"></a>Získáte informace:

- Postup vytvoření nového projektu webové formuláře.
- Struktura souborů projektu webové formuláře.
- Jak spustit projekt v sadě Visual Studio.
- Různých funkcí aplikace výchozí Web forms.
- Některé základní informace o tom, jak pomocí prostředí Visual Studio.

## <a name="creating-the-project"></a>Vytvoření projektu

1. Otevřete Visual Studio.
2. Vyberte **nový projekt** z **souboru** nabídky v sadě Visual Studio. 

    ![Vytvoření projektu – nové položky nabídky projektu](create-the-project/_static/image1.png)
3. Vyberte **šablony**  - &gt; **Visual C#**  - &gt; **webové** skupiny šablony na levé straně.
4. Vyberte **webové aplikace ASP.NET** šablony v prostředním sloupci.  
 Tento kurz řady používá rozhraní .NET Framework 4.5.2.
5. Název projektu *Northwind* a zvolte **OK** tlačítko. 

    ![Vytvoření projektu – dialogové okno Nový projekt](create-the-project/_static/image2.png)

    > [!NOTE]
    > Název projektu v řadě tento kurz je **Northwind**. Doporučuje se použít toto *přesný* projektu název tak, aby kód poskytovaná v kurzu řady funkcí podle očekávání.
6. Potom vyberte **webových formulářů** šablony a zvolte **vytvoření projektu** tlačítko.  

    ![Vytvoření projektu – nové šablony projektu](create-the-project/_static/image3.png)

Projekt bude trvat nějakou dobu vytvoření. Až bude program připraven, otevřete **Default.aspx** stránky.

![Vytvoření projektu – nové šablony projektu](create-the-project/_static/image4.png)

Můžete přepínat mezi **návrhu** zobrazení a **zdroj** zobrazit výběrem možnosti v dolní části okna center. **Návrh** zobrazení zobrazí ASP.NET – webové stránky, hlavní stránky, stránky obsahu, stránky HTML a uživatelské ovládací prvky pomocí blížící se WYSIWYG zobrazení. **Zdroj** zobrazení se zobrazí kód HTML pro webové stránky, které můžete upravit.

> [!TIP] 
> 
> **Vysvětlení rozhraní ASP.NET**
> 
> Webové formuláře ASP.NET umožňuje sestavení dynamické weby s využitím známý model přetahování myší, založeného na událostech. Návrhová plocha a stovky ovládacích prvků a komponent vám umožní rychle vytvořit sofistikované řízené uživatelského rozhraní weby s výkonným přístup k datům. Úložiště hračka Wingtip je založeno na webových formulářů ASP.NET, ale mnoho koncepty, kterým se seznámíte v této série kurzu platí pro všechny technologie ASP.NET.
> 
> Technologie ASP.NET nabízí čtyři primární vývoj architektury:
> 
> - [ASP.NET – webové formuláře](../../../index.md)  
>  Webové formuláře framework se zaměřuje na vývojáře, kteří raději deklarativní a založený na řízení programování, jako je například Microsoft Windows Forms (WinForms) a WPF nebo XAML nebo Silverlight. Nabízí WYSIWYG vývoje řízeného Návrhář modelu, tak se s vývojáři hledá prostředí pro vývoj (RAD) rychlé aplikace pro vývoj webů. Pokud jsou pro nové webové programování a jsou obeznámeni s tradiční Microsoft RAD vývoj nástroji klienta (například Visual Basic a Visual C#), můžete rychle vytvořit webovou aplikaci bez nutnosti prostředí v rámci HTML a JavaScript.
> - [ASP.NET MVC](../../../../mvc/index.md)  
>  ASP.NET MVC cílí vývojáře, kteří mají zájem o vzory a principy vývoje řízeného testováním, oddělené oblasti zájmu, v inverzi řízení (IoC) a vkládání závislostí (DI). Toto rozhraní podporuje, oddělení vrstvu obchodní logiky webové aplikace z jeho prezentační vrstvy.
> - [Webové stránky ASP.NET](../../../../web-pages/index.md)  
>  Rozhraní ASP.NET Web Pages cílí vývojáře, kteří chtějí scénáře vývoj jednoduchého webového, podle PHP. V modelu webové stránky vytvoření stránky HTML a poté přidejte serverový kód na stránku Chcete-li dynamicky řízení způsobu vykreslení značek. Webové stránky je určená speciálně pro být jednoduché rozhraní, a je nejjednodušší vstupním bodem do technologie ASP.NET pro uživatele, kteří znalost jazyka HTML, ale nemusí mít širokou programovací prostředí – například, studenty nebo hobbyists. Je také vhodné vývojářům webů, kteří znají, PHP nebo podobné architektury spuštění pomocí technologie ASP.NET.
> - [Aplikace ASP.NET jedné stránky](../../../../single-page-application/index.md)  
>  ASP.NET jedné stránky aplikace (SPA) pomáhá vytvářet aplikace, které zahrnují významné interakce na straně klienta pomocí standardu HTML 5, 3 šablon stylů CSS a JavaScript. Technologie ASP.NET a webové nástroje 2012.2 aktualizace dodává novou šablonu pro vytváření jednostránkové aplikace s použitím knockout.js a ASP.NET Web API. Kromě novou šablonu SPA jsou nové šablony vytvořené komunity SPA také k dispozici ke stažení.
> 
> Kromě čtyři hlavní vývoj architektury nabízí ASP.NET i další technologie, které jsou důležité vědět, a znáte, ale nejsou součástí tohoto kurzu řady:
> 
> - [Rozhraní ASP.NET Web API](../../../../web-api/index.md) – architektura pro vytváření služeb HTTP, které využity širokou škálou klientů včetně prohlížečů a mobilních zařízení.
> - [Funkce SignalR technologie ASP.NET](../../../../signalr/index.md) -knihovnu, která usnadňuje vývoj funkce webu v reálném čase.


### <a name="reviewing-the-project"></a>Kontrola projektu

V sadě Visual Studio **Průzkumníku řešení** okno umožňuje spravovat soubory pro projekt. Podívejme se na složky, které byly přidány do vaší aplikace v **Průzkumníku řešení**. Šablony webové aplikace přidá do základní složky struktury:

![Vytvoření projektu – Průzkumník řešení](create-the-project/_static/image5.png)

Visual Studio vytvoří některé počáteční složek a souborů pro váš projekt. První soubory, které budete pracovat s později v tomto kurzu jsou následující:

| **Soubor** | **Účel** |
| --- | --- |
| *Default.aspx* | Obvykle první stránka zobrazí, když je aplikace spuštěna v prohlížeči. |
| *Site.Master* | Stránka, která vám umožní vytvořit konzistentní rozložení a použijte standardní chování stránky v aplikaci. |
| *Global.asax* | Volitelný soubor, který obsahuje kód pro reagování na úrovni aplikace a úrovni relace události aktivované technologií ASP.NET nebo vytváření modulů HTTP. |
| *Web.config* | Konfigurační data pro aplikaci. |

### <a name="running-the-default-web-application"></a>Spuštění výchozí webové aplikace

Výchozí webové aplikace nabízí bohaté prostředí na základě integrovanou funkci a podporu. Bez uložení změn na výchozí Web forms projekt aplikace je připraven ke spuštění v místním webovém prohlížeči.

1. Stiskněte ***F5*** klíče při v sadě Visual Studio.   
 Aplikace bude sestavovat a zobrazení ve webovém prohlížeči.  

    ![Vytvoření projektu – výchozí stránka](create-the-project/_static/image6.png)
2. Jakmile se dokončené zkontrolujte běžící aplikaci, zavřete okno prohlížeče.

Existují tři hlavní stránky v této výchozí webové aplikace: *Default.aspx* (domů), *About.aspx*, a *Contact.aspx*. Všechny tyto stránek dosažitelný z horním navigačním panelu. Existují také dva další stránky obsažené ve složce účet, na stránku Register.aspx a stránku Login.aspx. Tyto dvě stránky umožňují používat funkce členství technologie ASP.NET vytvořit, uložit a ověřit přihlašovací údaje uživatele.

## <a name="aspnet-web-forms-background"></a>ASP.NET – webové formuláře pozadí

Webové formuláře ASP.NET jsou stránky, které jsou založené na technologii Microsoft ASP.NET, ve kterém kód, který běží na serveru dynamicky vygeneruje výstup webové stránky do prohlížeče nebo klienta zařízení. Stránku webových formulářů ASP.NET automaticky vykreslí správný HTML kompatibilní s prohlížečem funkcí, jako jsou styly, rozložení a tak dále. Webové formuláře jsou kompatibilní se žádný jazyk podporuje modulu .NET CLR, jako je například Microsoft Visual Basic a Microsoft Visual C#. Navíc webové formuláře jsou postaveny na [rozhraní Microsoft .NET Framework](https://msdn.microsoft.com/vstudio/aa496123), která poskytuje výhody například spravovaném prostředí, bezpečnost typů a dědičnosti.

Při spuštění stránky webových formulářů ASP.NET, stránka prochází životním cyklem, ve které provádí řadu kroků zpracování. Tyto kroky zahrnují inicializace vytváření instancí ovládacích prvků, obnovení a udržování stavu, spuštěná kód obslužné rutiny událostí a vykreslování. Seznámení s výkonem webových formulářů ASP.NET, je důležité porozumět [životního cyklu stránky ASP.NET](https://msdn.microsoft.com/library/ms178472(v=vs.100).aspx) tak, aby můžete ve fázi odpovídající životního cyklu pro účinek, který chcete napsat kód.

Když webový server obdrží požadavek stránku, se najde stránky, procesy, odešle do prohlížeče a poté odstraní všechny informace o stránku. Pokud uživatel požaduje stejné stránce znovu, server zopakuje sekvenci celou stránku od začátku. Jinými slovy, má server nedostatek paměti stránek má zpracování stránky jsou bezstavové. Automaticky zpracovává Udržovat stav stránku a jeho ovládacích prvků technologie ASP.NET a poskytne vám explicitní způsobů, jak udržovat stav informace specifické pro aplikaci.

> [!TIP] 
> 
> **Funkce webových aplikací v šabloně aplikace webových formulářů**
> 
> Šablona aplikaci webových formulářů technologie ASP.NET poskytuje bohatou sadu integrovanou funkci. Nejenom, poskytne vám s *Home.aspx* stránka, *About.aspx* stránky, *Contact.aspx* stránky, ale také zahrnuje funkce členství, který registruje uživatele a uloží své přihlašovací údaje, aby se můžete přihlásit na váš web. Tento přehled poskytuje další informace o některé funkce obsažené v šabloně Forms webové aplikace ASP.NET a jak se používají v aplikaci adresář Wingtip Toys.
> 
> **Členství**
> 
> [ASP.NET](https://msdn.microsoft.com/library/yh26yfzy.aspx) Identity ukládá přihlašovací údaje uživatelů v databázi vytvořené aplikací. Pokud vaši uživatelé přihlásí, aplikace ověří jejich přihlašovacích údajů načtením databáze. Váš projekt *účet* složka obsahuje soubory, které implementují různých částí členství: registrace, přihlášení, změna hesla a autorizaci přístupu. Kromě toho webových formulářů ASP.NET podporuje OAuth a OpenID. Tato vylepšení ověřování umožňují uživatelům přihlásit se do vaší lokality pomocí stávající přihlašovací údaje z těchto účtů jako Facebook, Twitter, Windows Live a Google.
> 
> ![Vytvoření projektu – Průzkumník řešení (ASP.NET Identity)](create-the-project/_static/image7.png)
> 
> Ve výchozím nastavení vytvoří šablona dílčí databázi členství pomocí výchozí název databáze na instanci systému SQL Server Express LocalDB, vývoj databázový server, který se dodává s Visual Studio Express 2013 pro Web.
> 
> **SQL Server Express LocalDB**
> 
> [SQL Server Express LocalDB](https://technet.microsoft.com/library/hh510202.aspx) je Odlehčená verze systému SQL Server, který obsahuje mnoho funkcí programovatelnosti databáze systému SQL Server. SQL Server Express LocalDB běží v uživatelském režimu a má rychlé, bez nutnosti konfigurace instalace, který má seznam krátké požadavky pro instalaci. V systému Microsoft SQL Server, všechny databáze nebo kód jazyka Transact-SQL můžete přesunout z SQL serveru Express LocalDB do systému SQL Server a SQL Azure bez upgradu kroků. Ano SQL Server Express LocalDB slouží jako vývojářského prostředí pro aplikace cílený na všech edicích systému SQL Server. SQL Server Express LocalDB umožňuje jako uložené procedury, uživatelem definované funkce a agregace, integrace rozhraní .NET Framework, prostorové typy a dalších funkcí, které nejsou k dispozici v systému SQL Server Compact.
> 
> **Stránky předlohy**
> 
> [Hlavní stránky ASP.NET](https://msdn.microsoft.com/library/wtxbf3hh.aspx) definuje konzistentní vzhled a chování pro všechny stránky v aplikaci. Rozložení stránky předlohy sloučí s obsahem z jednotlivé stránky obsahu k vytvoření poslední stránce, která uživateli se zobrazí. V aplikaci adresář Wingtip Toys upravíte *Site.master* hlavní stránky tak, aby všechny stránky webu adresář Wingtip Toys sdílet stejný rozlišovací logo a navigační panel.
> 
> **HTML5**
> 
> Šablona aplikaci webových formulářů ASP.NET podporuje [HTML5](http://www.w3schools.com/html/html5_intro.asp), což je nejnovější verzi značka jazyka HTML. HTML5 podporuje nové prvky a funkce, které usnadňují vytváření webů.
> 
> **Modernizr**
> 
> U počítačů, které nepodporují HTML5, můžete použít [Modernizr](http://www.modernizr.com/). Modernizr je knihovna JavaScript open source, který může zjistit, zda prohlížeč podporuje funkce HTML5 a je povolit, pokud neexistuje. V šabloně aplikaci webových formulářů ASP.NET je nainstalována Modernizr jako balíčku NuGet.
> 
> **Bootstrap**
> 
> Šablony projektů Visual Studio 2013 pomocí [Bootstrap](http://getbootstrap.com/), rozložení a motivů rozhraní vytvořené Twitter. K poskytování přizpůsobivý návrh, což znamená, že rozložení se může dynamicky přizpůsobit velikost okna jiný prohlížeč používá Bootstrap CSS3. Funkce motivů Bootstrap je také můžete snadno ovlivňuje změnu aplikace vzhled a chování. Ve výchozím nastavení zahrnuje šablony webové aplikace ASP.NET v sadě Visual Studio 2013 Bootstrap jako balíčku NuGet.
> 
> **Balíčky NuGet**
> 
> Šablona aplikaci webových formulářů ASP.NET obsahuje sadu [NuGet](http://www.nuget.org/) balíčky. Tyto balíčky komponentních nakonfigurovánu ve formě opensourcové knihovny a nástroje. Není širokou škálu balíčky, které vám pomohou vytvořit a testování aplikací. Visual Studio lze snadno přidávat, odstraňovat a aktualizovat balíčky NuGet. Vývojáři můžou vytvářet a také přidat balíčky NuGet.
> 
> ![Vytvoření projektu – dialogové okno NuGet](create-the-project/_static/image8.png)
> 
> Při instalaci balíčku NuGet zkopíruje soubory do vašeho řešení a vytvoří automaticky potřebné jakékoli změny, jako je například přidávání odkazů a změna konfigurace přidružené k webové aplikaci. Pokud se rozhodnete odebrat knihovny, NuGet odebere soubory a obrátí jakékoli změny provedené ve vašem projektu tak, aby je ponechán bez zbytečné soubory. NuGet najdete na webu **nástroje** nabídky v sadě Visual Studio.
> 
> **jQuery**
> 
> [jQuery](http://jquery.com/) je rychlé a stručným knihovna JavaScript, která zjednodušuje procházení dokumentu HTML, zpracování událostí, animace a interakce Ajax pro vývoj webů rychlé. Knihovna jQuery JavaScript je obsažena v šabloně aplikaci webových formulářů ASP.NET jako balíčku NuGet.
> 
> **Ověření nerušivého**
> 
> Ovládací prvky pro integrované ověřování byla nakonfigurována pro použití nerušivý JavaScript pro ověřování na straně klienta. To významně snižuje velikost JavaScript vykreslovány do kódu stránky a snižuje celkové velikosti stránky. Nerušivý ověření je globálně přidat do šablony ASP.NET aplikaci Web Forms podle nastavení v &lt;appSettings&gt; element *Web.config* soubor v kořenovém adresáři aplikace.
> 
> **Rozhraní Entity Framework Code First**
> 
> Kromě toho funkce v šabloně aplikaci webových formulářů ASP.NET, aplikace adresář Wingtip Toys používá [Entity Framework Code First](https://weblogs.asp.net/scottgu/archive/2010/12/08/announcing-entity-framework-code-first-ctp5-release.aspx), což je NuGet knihovny, která umožňuje kód zaměřená na vývoj při práci s daty. Jednoduše PUT, vytvoří databázi součásti aplikace založené na kód, který můžete psát. Pomocí rozhraní Entity Framework, načtěte a pracovat s daty jako objektů se silným typem. Díky tomu se můžete soustředit na obchodní logiku v aplikaci, nikoli podrobnosti o tom, jak je přístup k datům.
> 
> Další informace o nainstalovaných knihoven nebo balíčků, které jsou součástí šablony webových formulářů ASP.NET najdete v seznamu nainstalovaných balíčků NuGet. To provedete v sadě Visual Studio vytvořte nový projekt webové formuláře, vyberte **nástroje**  - &gt; **Správce balíčků knihoven**  - &gt; **spravovat Balíčky NuGet pro řešení**a vyberte **nainstalované balíky** v **spravovat balíčky NuGet** dialogové okno.


### <a name="touring-visual-studio"></a>Jízdních sady Visual Studio

Zahrnout primární windows v sadě Visual Studio **Průzkumníku řešení**, **Průzkumníka serveru** (**Průzkumník databáze** ve Express), **vlastnosti Okno**, **sada nástrojů**, **nástrojů**a **okno dokumentu**.

![Vytvoření projektu – dialogové okno NuGet](create-the-project/_static/image9.png)

Další informace o sadě Visual Studio najdete v tématu [vizuální průvodce Visual Web Developer](https://msdn.microsoft.com/library/ee410104.aspx).

## <a name="summary"></a>Souhrn

V tomto kurzu jste vytvořili, zkontrolovat a spustit výchozí aplikace webových formulářů. Zkontrolovat různých funkcí aplikace výchozí Web forms a se naučili základy o tom, jak pomocí prostředí Visual Studio. V následujících kurzech vytvoříte vrstva přístupu k datům.

## <a name="additional-resources"></a>Další prostředky

[Výběr správné programovací Model](../../../videos/how-do-i/choosing-the-right-programming-model.md)   
[Projekty webových aplikací versus webové projekty](https://msdn.microsoft.com/library/dd547590.aspx)   
[ASP.NET – webové formuláře stránky přehled](https://msdn.microsoft.com/library/428509ah.aspx)

>[!div class="step-by-step"]
[Předchozí](introduction-and-overview.md)
[další](create_the_data_access_layer.md)
