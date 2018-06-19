---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Vytvoření nového projektu ASP.NET MVC | Microsoft Docs
author: microsoft
description: Krok 1 ukazuje, jak zavést základní strukturu NerdDinner aplikace.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: d15ca67f0ddd8db6842bc5112996ae2dee433536
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869254"
---
<a name="create-a-new-aspnet-mvc-project"></a>Vytvoření nového projektu ASP.NET MVC
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Toto je krok 1 ze bezplatný [kurz aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , nevystavíte slabé stránky zabezpečení – prostřednictvím postup sestavení malá, ale dokončení, webové aplikace pomocí ASP.NET MVC 1.
> 
> Krok 1 ukazuje, jak zavést základní strukturu NerdDinner aplikace.
> 
> Pokud používáte ASP.NET MVC 3, doporučujeme provedením [získávání spuštěna s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Hudba úložiště](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.


## <a name="nerddinner-step-1-file-gtnew-project"></a>NerdDinner krok 1: Soubor -&gt;nový projekt

Začneme budete naše aplikace NerdDinner výběrem **souboru -&gt;nový projekt** položky nabídky v sadě Visual Studio 2008 nebo volné Visual Web Developer 2008 Express.

Tím se otevře dialogové okno "Nový projekt". K vytvoření nové aplikace ASP.NET MVC, jsme budete vyberte uzel "Web" na levé straně dialogového okna a potom vyberte šablonu projektu "Webové aplikace ASP.NET MVC" na pravé straně:

![](create-a-new-aspnet-mvc-project/_static/image1.png)

*Důležité: Zajistěte, aby si stáhli a nainstalovali ASP.NET MVC – jinak se nezobrazí v dialogovém okně Nový projekt. Můžete použít V2 z [instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx) Pokud jste ho ještě nenainstalovali (ASP.NET MVC je k dispozici v rámci "webové platformy -&gt;rozhraní a moduly runtime" části).*

Jsme budete název nového projektu, které jsme se chystáte vytvořit "NerdDinner" a klikněte na tlačítko "ok" k jeho vytvoření.

Když jsme klikněte na tlačítko "ok" sady Visual Studio zobrazíte další dialogové okno, které nám pro volitelné vytvoření projektu testů jednotek pro nové aplikace vyzve. Tato projektu testování částí umožňuje vytvořit automatizované testy, které ověřují funkce a chování aplikace (něco jsme zaměříme jak úkolů později v tomto kurzu).

![](create-a-new-aspnet-mvc-project/_static/image2.png)

"Test framework" rozevíracího seznamu v dialogovém okně výše bude zahrnovat všechny dostupné ASP.NET jednotky testovací šablon projektu MVC v počítači nainstalován. Verze si můžete stáhnout pro NUnit, MBUnit a XUnit. Vestavěnou architekturou testu jednotek sady Visual Studio je také podporována.

*Poznámka: Visual Studio Unit Test Framework je dostupná jenom s Visual Studio 2008 Professional a vyšších verzí. Pokud používáte VS 2008 Standard Edition nebo Visual Web Developer 2008 Express je nutné stáhnout a nainstalovat rozšíření NUnit, MBUnit nebo XUnit pro architekturu ASP.NET MVC v pořadí pro toto dialogové okno, která se má zobrazit. Dialogové okno se nezobrazí, pokud nejsou k dispozici žádné rozhraní test nainstalována.*

Jsme budete používat výchozí název "NerdDinner.Tests" k testovacímu projektu, který vytváříme a použijete možnost "Visual Studio jednotkové testování" framework. Když kliknete na tlačítko "ok" sady Visual Studio vytvoří řešení pro nám s dva projekty v ní – jeden pro naše webové aplikace a jeden pro naše testy:

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a>Zkoumání NerdDinner strukturu adresáře

Když vytvoříte novou aplikaci ASP.NET MVC pomocí sady Visual Studio, automaticky se přidá počet souborů a adresářů do projektu:

![](create-a-new-aspnet-mvc-project/_static/image4.png)

Projekty ASP.NET MVC ve výchozím nastavení mají šesti nejvyšší úrovně adresáře:

| **Adresář** | **Účel** |
| --- | --- |
| **/ Řadiče** | Kam jste umístili řadiče třídy, které zpracovávají požadavky na adresu URL |
| **/ Modely** | Kam jste umístili třídy, které představují a pracovat s daty |
| **Nebo zobrazení.** | Kde umístit soubory šablon uživatelského rozhraní, které jsou zodpovědní za vykreslování výstup |
| **A skriptů** | Kde umístit soubory knihovny JavaScript a skripty (.js) |
| **A obsah** | Kam jste umístili šablon stylů CSS a soubory obrázků a další obsah bez dynamické/JavaScript |
| **/ Aplikace\_dat** | Tam, kde se ukládají datové soubory chcete pro čtení a zápis. |

ASP.NET MVC nevyžaduje, aby tuto strukturu. Ve skutečnosti vývojáře, kteří pracují na velké aplikace bude obvykle oddílu aplikace se ve více projektech, aby se daly více spravovat (například: třídy modelu dat se často přejděte v projektu knihovny tříd samostatné z webové aplikace). Výchozí projekt struktury však poskytuje dobrý výchozí adresář konvenci, jsme můžete zachovat čisté riziko z hlediska naší aplikace.

Když jsme rozbalte adresář /Controllers jsme najdete, Visual Studio přidali dvě třídy controller – HomeController a AccountController – ve výchozím nastavení do projektu:

![](create-a-new-aspnet-mvc-project/_static/image5.png)

Když jsme rozšířit adresáři /Views, jsme najdete tři podadresářích – / Home, /Account a /Shared –, jakož i několik šablonu, kterou soubory v nich byly také k projektu nepřidají ve výchozím nastavení:

![](create-a-new-aspnet-mvc-project/_static/image6.png)

Když jsme rozšířit/Content a/Scripts adresářů, jsme najdete podporu souboru Site.css, která je použita k formátování všechny HTML na webu a také můžete povolit prvku ASP.NET AJAX a knihovnu jQuery knihovny jazyka JavaScript v aplikaci:

![](create-a-new-aspnet-mvc-project/_static/image7.png)

Když jsme rozbalte projekt NerdDinner.Tests jsme najdete dvou tříd, které obsahují testování částí pro naše řadiče třídy:

![](create-a-new-aspnet-mvc-project/_static/image8.png)

Tyto soubory výchozí přidat Visual Studio poskytnout nám základní strukturu pro funkční aplikaci - kompletní s domovskou stránku, o stránku, účet přihlášení/odhlášení nebo registrace stránky a neošetřené chybová stránka (všechny drátové zatížení a pracovní mimo pole).

### <a name="running-the-nerddinner-application"></a>Spuštění aplikace NerdDinner

Můžeme spustit projekt tak, že buď zvolíte **ladění -&gt;spustit ladění** nebo **ladění -&gt;spustit bez ladění** položky nabídky:

![](create-a-new-aspnet-mvc-project/_static/image9.png)

Tím spustíte předdefinované ASP.NET webový server, který se dodává s Visual Studio a spuštění aplikace:

![](create-a-new-aspnet-mvc-project/_static/image10.png)

Níže je domovská stránka pro naše nový projekt (adresa URL: "/") při spuštění:

![](create-a-new-aspnet-mvc-project/_static/image11.png)

Kliknutím na kartu "O" zobrazí o stránce (adresa URL: "/ domácí, / o"):

![](create-a-new-aspnet-mvc-project/_static/image12.png)

Kliknutím na odkaz "Přihlášení" v pravé horní trvá na přihlašovací stránku (adresa URL: "/ Account/přihlášení")

![](create-a-new-aspnet-mvc-project/_static/image13.png)

Pokud nemáme přihlašovací účet jsme klikněte na odkaz registrace (adresa URL: "/ Account/Register") Chcete-li vytvořit:

![](create-a-new-aspnet-mvc-project/_static/image14.png)

Kód k provedení výše Domů, o a odhlášení Register funkce byla přidána ve výchozím nastavení, když jsme vytvořili naše nový projekt. Použijeme ji jako výchozí bod aplikace.

### <a name="testing-the-nerddinner-application"></a>Testování aplikace NerdDinner

Pokud se používá edice Professional nebo vyšší verze sady Visual Studio 2008, jsme můžete použít integrovanou jednotku testování podpora rozhraní IDE v sadě Visual Studio k testování projektu:

![](create-a-new-aspnet-mvc-project/_static/image15.png)

Vybrat jednu z výše uvedených možností otevřete podokno "Výsledky testů" v prostředí IDE a poskytnout nám průchodu nebo selhání stavu na testování částí 27 součástí naší nový projekt, které se týkají integrovanou funkci:

![](create-a-new-aspnet-mvc-project/_static/image16.png)

Později v tomto kurzu jsme budete komunikovat více informací o automatizované testování a přidávat další jednotky testy, které se týkají funkce aplikace, kterou jsme implementovat.

### <a name="next-step"></a>Další krok

Jste nyní My struktura základní aplikace na místě. Nyní Pojďme [vytvořit databázi k ukládání dat naše aplikace](create-a-database.md).

> [!div class="step-by-step"]
> [Předchozí](introducing-the-nerddinner-tutorial.md)
> [další](create-a-database.md)
