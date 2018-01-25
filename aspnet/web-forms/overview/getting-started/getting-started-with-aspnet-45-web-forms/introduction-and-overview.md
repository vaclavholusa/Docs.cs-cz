---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: "Začínáme s 4.5 webových formulářů ASP.NET a Visual Studio 2013 | Microsoft Docs"
author: Erikre
description: "Tento podrobný kurz řady naučit se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a Microsoft Visual Studio Expres..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 6ae398f94c0ac3872601bdc8fd935f6d285793db
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a>Začínáme s 4.5 webových formulářů ASP.NET a Visual Studio 2013
====================
Podle [Erik Reitan](https://github.com/Erikre)

[Stáhnout adresář Wingtip Toys ukázkového projektu (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) nebo [stáhnout elektronická kniha (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Tento podrobný kurz řady naučit se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a Microsoft Visual Studio Express 2013 pro Web. [ASP.NET – webové formuláře kvízu s časovým limitem](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> Otestujte svoje znalosti a posílit klíčové koncepty provedením kvízu ASP.NET Web Forms. Tato kvízu s časovým limitem byl speciálně z obsahu, které jsou součástí tohoto kurzu řady. Každý dotaz v kvízu obsahuje vysvětlení spolu s odkazy na další pokyny.


## <a name="introduction"></a>Úvod

Tato série kurzů vás provede kroky potřebné k vytváření aplikací webových formulářů ASP.NET pomocí Visual Studio Express 2013 pro Web a technologie ASP.NET 4.5.

Aplikace vytvoříte jmenuje **adresář Wingtip Toys**. Je zjednodušená příklad úložiště front web, který se prodává položky online. Tento kurz řady označuje nové funkce, které jsou k dispozici v technologii ASP.NET 4.5.

Komentáře jsou úvodní a vytočit veškeré úsilí aktualizovat tento kurz řady podle vaší návrhy.

### <a name="download-completed-project"></a>Stažení dokončit projektu

Můžete si stáhnout projekt C#, který obsahuje dokončené kurzu.

- [Začínáme s 4.5 webových formulářů ASP.NET a Visual Studio 2013 – adresář Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a>Zkontrolujte obsah provedením související kvízu webových formulářů ASP.NET

Po dokončení tohoto kurzu své znalosti a posílit klíčové koncepty provedením [ASP.NET Web Forms kvízu](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001). Tato kvízu s časovým limitem byl speciálně z obsahu, které jsou součástí tohoto kurzu řady. Každý dotaz v kvízu obsahuje vysvětlení spolu s odkazy na další pokyny.

- [ASP.NET – webové formuláře kvízu s časovým limitem](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a>Cílová skupina

Předpokládanou cílovou skupinou série tento kurz je zkušeného vývojáře, kteří jsou nové webových formulářů ASP.NET. Vývojář zájem o tento kurz řady musí mít následující dovedností:

- Neznáte objekt zaměřené na konkrétní programovací jazyk (mimoprocesová aplikace VOLÁNA)
- Seznamte s koncepty vývoje webu (HTML, CSS, JavaScript)
- Seznamte s koncepty relační databáze
- Seznamte s koncepty n vrstvá architektura

Pokud vás zajímá kontrola oblasti uvedené výše, vezměte v úvahu kontrola následující obsah:

- [Začínáme s jazykem Visual C#](https://msdn.microsoft.com/library/a72418yk.aspx)
- [Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)
- [Relační databáze](http://en.wikipedia.org/wiki/Relational_database)
- [Třívrstvá architektura](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a>Funkce aplikací

Funkce webového formuláře ASP.NET uvedené v této série zahrnují:

- Projekt webové aplikace (není webový projekt)
- webové formuláře
- Stránky předlohy, konfigurace
- Bootstrap
- Rozhraní Entity Framework Code nejprve LocalDB
- Ověření žádosti
- Silného typu ovládací prvky datových, Model vazby datových poznámek a hodnota zprostředkovatelů
- Protokol SSL a OAuth
- ASP.NET Identity, konfiguraci a autorizaci
- Ověření nerušivého
- Směrování
- Zpracování chyb ASP.NET

### <a name="application-scenarios-and-tasks"></a>Scénáře aplikací a úloh

Úlohy předvedená v této série zahrnují:

- Vytváření, kontrola a spuštění nového projektu
- Vytvoření struktury databáze
- Inicializace a synchronizace replik indexů databáze
- Přizpůsobení uživatelského rozhraní pomocí stylů, grafiky a stránky předlohy
- Přidání stránky a navigace
- Zobrazení nabídky podrobnosti a data produktu.
- Vytváření nákupní košík
- Podpora přidání SSL a OAuth
- Přidání způsob platby
- Včetně roli správce a uživatele k aplikaci
- Omezení přístupu k konkrétní stránky a složky
- Nahrání souboru do webové aplikace
- Implementace ověřování vstupu
- Registrace trasy pro webové aplikace
- Implementace zpracování chyb a protokolování chyb

## <a name="overview"></a>Přehled

Pokud jste novým uživatelem webových formulářů ASP.NET ale mít znalost koncepce programování, máte správné kurzu. Pokud jste již obeznámeni s webovými formuláři ASP.NET, můžete využívat výhod tento kurz řady nové funkce, které jsou k dispozici v technologii ASP.NET 4.5. Pokud jste obeznámeni s programování konceptů a webových formulářů ASP.NET, najdete v části Další kurzy součástí webové formuláře [Začínáme](../../../index.md) části na webu technologie ASP.NET.

Konkrétní **nejnovější** technologie ASP.NET 4.5 funkce poskytované v této webové formuláře kurz řady zahrnují následující:

- Jednoduché uživatelského rozhraní pro vytváření projektů v rámci této nabídky [podporu pro více rozhraní ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (webových formulářů, MVC a webového rozhraní API).
- [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), rozložení a motivů rozhraní, které zajišťuje přizpůsobivý návrh a motivů.
- [ASP.NET Identity](../../../../identity/index.md), nový systém členství technologie ASP.NET, který funguje stejně ve všech rozhraní ASP.NET a funguje s webového hostingu softwaru než služby IIS.
- [Rozhraní Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), aktualizace rozhraní Entity Framework, který umožňuje načíst a pracovat s daty jako silného typu objektů, přístup k datům asynchronně, zpracování chyb přechodný připojení a protokolu příkazů SQL.

Úplný seznam funkcí technologie ASP.NET 4.5, najdete v části [technologie ASP.NET a webové nástroje pro poznámky k verzi 2013 Visual Studio](../../../../visual-studio/overview/2013/release-notes.md).

### <a name="the-wingtip-toys-sample-application"></a>Adresář Wingtip Toys ukázkové aplikace

Následující snímek obrazovky poskytují rychlý přehled rozhraní ASP.NET Web forms aplikace, která se vytvoří tento kurz řady. Při spuštění aplikace z Visual Studio Express 2013 pro Web, zobrazí se následující domovskou stránku webové.

![Adresář Wingtip Toys – výchozí stránka](introduction-and-overview/_static/image1.png)

Můžete zaregistrovat jako nový uživatel nebo se přihlásit jako stávajícího uživatele. Navigace je poskytovaný v horní části pro každou kategorii produktu načítání dostupných produktů z databáze.

Odkaz produktů vyberete, bude moci zobrazit seznam všech dostupných produktů.

![Adresář Wingtip Toys - produktů](introduction-and-overview/_static/image2.png)

Najdete v části Podrobnosti o jednotlivých produktů vyberete některý z uvedených produktů.

![Adresář Wingtip Toys – podrobnosti o produktu](introduction-and-overview/_static/image3.png)

Jako uživatel můžete zaregistrovat a přihlaste se pomocí funkce výchozí šablony webových formulářů. V tomto kurzu taky vysvětluje, jak se přihlásit pomocí existujícího účtu z Gmailu. Kromě toho se může přihlásit jako správce přidávat a odebírat produkty z databáze.

![Adresář Wingtip Toys - přihlášení](introduction-and-overview/_static/image4.png)

Jakmile jste přihlášení jako uživatel, můžete přidat produkty nákupní košík a ověření pomocí služby PayPal. Všimněte si, že tato ukázková aplikace je určený ke fungovat s izolovaného prostoru PayPal pro vývojáře. Žádná transakce skutečné peníze bude probíhat.

![Adresář Wingtip Toys – nákupní košík](introduction-and-overview/_static/image5.png)

PayPal bude tak jasné, váš účet, pořadí a platebních údajů.

![Adresář Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

Po návratu z PayPal, můžete zkontrolovat a dokončit vaši objednávku.

![Adresář Wingtip Toys - Zkontrolujte pořadí](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>Požadavky

Než začnete, ujistěte se, že máte v počítači nainstalován následující software:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) nebo [sady Microsoft Visual Studio Express 2013 pro Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Rozhraní .NET Framework se instaluje automaticky.

Tento kurz řady používá Microsoft Visual Studio Express 2013 pro Web. K dokončení tohoto kurzu řady můžete použít buď Microsoft Visual Studio Express 2013 pro Web nebo Microsoft Visual Studio 2013.

> [!NOTE] 
> 
> Microsoft Visual Studio 2013 a Microsoft Visual Studio Express 2013 pro Web se často se označuje jako Visual Studio v rámci tohoto kurzu řady.


Pokud již máte nainstalované verze sady Visual Studio, proces instalace vedle stávající verzi nainstalovat Visual Studio 2013 nebo Microsoft Visual Studio Express 2013 pro Web. Weby, které jste vytvořili v dřívějších verzích lze otevřít v sadě Visual Studio 2013 a pokračovat v otevírání v předchozích verzích.

> [!NOTE] 
> 
> Tento návod předpokládá, že jste vybrali *vývoj webů* kolekce nastavení při prvním spuštění sady Visual Studio. Další informace najdete v tématu [postupy: nastavení prostředí vyberte vývoj webové](https://msdn.microsoft.com/library/ff521558.aspx).


## <a name="download-the-sample-application"></a>Stažení ukázkové aplikace

Po instalaci požadavky, jste připraveni se začne vytvářet nový webový projekt, který se zobrazí v řadě tento kurz. Pokud byste chtěli **volitelně** spusťte ukázkovou aplikaci, která vytvoří tento kurz řady, můžete ji stáhnout z webu MSDN ukázky. Tento soubor ke stažení obsahuje následující:

- Ukázkovou aplikaci v *Northwind* složky.
- Prostředky použít k vytvoření ukázkové aplikace v *Northwind prostředky* složku *Northwind* složky.

#### <a name="download-the-file-from-msdn-samples-site"></a>Soubor stáhněte z webu MSDN ukázky:

[Začínáme s 4.5 webových formulářů ASP.NET a Visual Studio 2013 – adresář Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 

Stažení *.zip* souboru. Zobrazíte dokončený projekt, který vytvoří tento kurz řady, vyhledejte a vyberte *C#*složku *.zip* souboru. Uložit *C#* folderto složky použijete při práci s projekty sady Visual Studio 2013. Ve výchozím nastavení složka projektů Visual Studio 2013 je následující:

**C:\Users\*****&lt;username&gt;*****\Documents\Visual Studio 2013\Projects**

Přejmenujte ***C#*** složku pro ***Northwind***.

> [!NOTE]
> Pokud již máte složku s názvem *Northwind* ve složce projekty dočasně přejmenujte tento existující složku před přejmenováním *C#* složku pro *Northwind*.


Chcete-li spustit dokončený projekt, otevřete *Northwind* složku a dvojím kliknutím *WingtipToys.sln* souboru. Otevřete projekt Visual Studio 2013. Pak klikněte pravým tlačítkem *Default.aspx* souborů v okně Průzkumníka řešení a v místní nabídce klikněte na tlačítko zobrazit v prohlížeči.

### <a name="tutorial-support-and-comments"></a>Kurz podporu a komentáře

Pomocí informací v části otázky a A součástí [Začínáme s webovými formuláři ASP.NET 4.5 a Visual Studio 2013 – adresář Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) vzorek (C#) pro jakékoli dotazy nebo připomínky.

Komentáře k tento kurz řady jsou úvodní a při aktualizaci tento kurz řady veškeré úsilí, budou provedeny brát v účtu opravy nebo návrhy na vylepšení, které jsou k dispozici v kurzu komentáře.

Když dojde k chybě během vývoje nebo na webu nefunguje správně, chybové zprávy, které může poskytnout komplexní různá vodítka na zdroj problému nebo nemusí vysvětlují, jak ji odstranit. Abychom vám pomohli s některé běžné scénáře problém, můžete také použít [ASP.NET fóra](https://forums.asp.net/) nebo části otázky a A součástí [Začínáme s webovými formuláři ASP.NET 4.5 a Visual Studio 2013 – adresář Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) () C#) ukázka. Pokud se zobrazí chybové hlášení, nebo něco nefunguje tak, jak projděte následující kurzy, nezapomeňte zkontrolovat výše uvedené umístění.

>[!div class="step-by-step"]
[Next](create-the-project.md)
