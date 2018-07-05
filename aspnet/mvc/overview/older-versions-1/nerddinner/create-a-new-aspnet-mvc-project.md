---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Vytvoření nového projektu ASP.NET MVC | Dokumentace Microsoftu
author: microsoft
description: Krok 1 ukazuje, jak změnit základní struktury aplikace NerdDinner na místě.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 9f5e0b3f82d113fc72ab4002ec8d06ad8444dceb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374273"
---
<a name="create-a-new-aspnet-mvc-project"></a>Vytvoření nového projektu ASP.NET MVC
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Toto je krok 1 bezplatný [kurz vývoje aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , který procházení procházení po tom, jak sestavit malý, ale bylo možné provést, webové aplikace pomocí ASP.NET MVC 1.
> 
> Krok 1 ukazuje, jak změnit základní struktury aplikace NerdDinner na místě.
> 
> Pokud používáte ASP.NET MVC 3, doporučujeme je provést [získávání začít s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.


## <a name="nerddinner-step-1-file-gtnew-project"></a>NerdDinner krok 1: Soubor -&gt;nový projekt

Naše aplikace NerdDinner jsme zobrazí za přibližně tak, že vyberete **souboru -&gt;nový projekt** položky nabídky v rámci sady Visual Studio 2008 nebo na bezplatné Visual Web Developer 2008 Express.

Tím se otevře dialogové okno "Nový projekt". K vytvoření nové aplikace ASP.NET MVC, budete výběru uzel "Web" na levé straně dialogového okna a pak vyberte šablonu projektu "Webová aplikace ASP.NET MVC" na pravé straně:

![](create-a-new-aspnet-mvc-project/_static/image1.png)

*Důležité: Ujistěte se, že jste stáhli a nainstalovali ASP.NET MVC – jinak se nezobrazí v dialogovém okně Nový projekt. Můžete použít verzí V2 [instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx) pokud ho ještě nemáte nainstalované (ASP.NET MVC je k dispozici v rámci "webové platformy –&gt;rámce a moduly runtime" části).*

Jsme budete pojmenujte nový projekt, který budeme vytvářet "NerdDinner" a potom klikněte na tlačítko "ok" k jeho vytvoření.

Když jsme na tlačítko "ok" Visual Studio se zobrazí další dialog, který zobrazí výzvu, volitelně vytvořit projekt testování částí pro novou aplikaci. Tento projekt testu jednotky umožňuje vytvořit automatizované testy, které ověřují funkce a chování našich aplikace (něco si probereme jak úkolů později v tomto kurzu).

![](create-a-new-aspnet-mvc-project/_static/image2.png)

"Testovací rozhraní" rozevírací seznam v dialogovém okně výše se vyplní všechny dostupné technologie ASP.NET jednotka testu šablon projektu MVC na počítači nainstalovaný. Verze si můžete stáhnout pro XUnit, NUnit a MBUnit. Vestavěnou architekturou testování částí Visual Studio je také podporována.

*Poznámka: Rozhraní testování částí Visual Studio je k dispozici pouze s Visual Studio 2008 Professional a vyšší verze. Pokud používáte VS 2008 Standard Edition nebo Visual Web Developer 2008 Express je potřeba stáhnout a nainstalovat rozšíření NUnit, MBUnit nebo XUnit pro architekturu ASP.NET MVC v pořadí pro toto dialogové okno má být zobrazen. Dialogové okno nezobrazí, pokud nejsou k dispozici žádné testovací rozhraní nainstalované.*

Použijeme výchozí název "NerdDinner.Tests" pro testovací projekt, který jsme vytvořili a pomocí možnosti "Visual Studio testování částí" rozhraní framework. Když kliknete na tlačítko "ok" Visual Studio vytvoří řešení pro nás s dva projekty v něm – jeden pro naši webovou aplikaci a jeden pro naše testy částí:

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a>Zkoumání adresářovou strukturu NerdDinner

Při vytváření nové aplikace ASP.NET MVC pomocí sady Visual Studio automaticky přidá několik souborů a adresářů do projektu:

![](create-a-new-aspnet-mvc-project/_static/image4.png)

Projekty ASP.NET MVC ve výchozím nastavení mají šest adresářů nejvyšší úrovně:

| **Adresář** | **Účel** |
| --- | --- |
| **/ Řadiče** | Místo, kam dáte třídách, které zpracovávají požadavky pro adresy URL |
| **/ Modelů** | Místo, kam dáte tříd, které představují a manipulaci s daty |
| **/ Zobrazení** | Místo, kam dáte soubory šablon uživatelského rozhraní, které jsou zodpovědné za vykreslování výstup |
| **/ Skripty** | Místo, kam dáte soubory knihoven jazyka JavaScript a skripty (.js) |
| **/ Obsahu** | Místo, kam dáte šablon stylů CSS a obrázkové soubory a jiný obsah než dynamické/JavaScript |
| **/ Aplikace\_dat** | Tam, kde se ukládají datové soubory byste měli pro čtení a zápisu. |

ASP.NET MVC nevyžaduje, aby tuto strukturu. Ve skutečnosti vývojáře, kteří pracují na velkých aplikací se obvykle oddílu aplikace nahoru ve více projektech, aby lépe zvládnutelné (například: tříd datových modelů často přejít v projektu knihovny samostatné třídy z webové aplikace). Výchozí strukturu projektu, ale poskytuje dobré výchozí adresář konvenci, můžeme použít naše aplikace priority udržovat čisté.

Když jsme rozbalte adresář /Controllers jsme, že Visual Studio přidá dvě třídy kontroleru – HomeController a AccountController – ve výchozím nastavení se do projektu najdete:

![](create-a-new-aspnet-mvc-project/_static/image5.png)

Rozbalením adresáři /Views jsme jsme najdete tři podadresáře – / Home, / Account a /Shared –, jakož i několik šablon, které soubory v nich byly také přidali do projektu ve výchozím nastavení:

![](create-a-new-aspnet-mvc-project/_static/image6.png)

Rozbalením/Content a adresáře/Scripts jsme najdete podporu Site.css soubor, který slouží k úpravě stylu HTML v lokalitě, jakož i knihoven jazyka JavaScript, které můžete povolit a jQuery v ASP.NET AJAX v rámci aplikace:

![](create-a-new-aspnet-mvc-project/_static/image7.png)

Když jsme rozbalte projekt NerdDinner.Tests jsme najdete dvě třídy, které obsahují testy jednotek pro naše třídy kontroleru:

![](create-a-new-aspnet-mvc-project/_static/image8.png)

Tyto výchozí soubory přidané pomocí sady Visual Studio nabízí základní strukturu pro fungující aplikaci – dokončeno s domovské stránky, stránky, stránky účtu přihlášení/odhlášení/registrace a stránku neošetřené chybě (všechny drátové nahoru a v provozu mimo pole).

### <a name="running-the-nerddinner-application"></a>Spuštění aplikace NerdDinner

Provozujeme projektu výběrem buď **ladění –&gt;spustit ladění** nebo **ladění –&gt;spustit bez ladění** položky nabídky:

![](create-a-new-aspnet-mvc-project/_static/image9.png)

Tím spustíte integrovaný ASP.NET Web-server, který je součástí sady Visual Studio a spuštění aplikace:

![](create-a-new-aspnet-mvc-project/_static/image10.png)

Tady je domovská stránka pro náš nový projekt (adresa URL: "/") během spuštění:

![](create-a-new-aspnet-mvc-project/_static/image11.png)

Klikněte na kartu "O" zobrazí o stránce (adresa URL: "/ Home, nebo brzo"):

![](create-a-new-aspnet-mvc-project/_static/image12.png)

Kliknutím na odkaz "Přihlásit" v pravém horním trvá na přihlašovací stránku (adresa URL: "/ účet/přihlášení")

![](create-a-new-aspnet-mvc-project/_static/image13.png)

Pokud nemáme přihlašovací účet, můžeme kliknout na odkaz registrovat (adresa URL: "/ účtu/registrace") k jejímu vytvoření:

![](create-a-new-aspnet-mvc-project/_static/image14.png)

Kód pro implementaci nahoře na domovské stránce a odhlášení / register funkce přidané ve výchozím nastavení, když jsme vytvořili náš nový projekt. Použijeme jej jako výchozí bod naši aplikaci.

### <a name="testing-the-nerddinner-application"></a>Testování aplikace NerdDinner

Pokud se používá edice Professional nebo vyšší verzi sady Visual Studio 2008, můžeme použít integrované testování podpora integrované vývojové prostředí sady Visual Studio k testování projektu:

![](create-a-new-aspnet-mvc-project/_static/image15.png)

Výběrem jedné z výše uvedených možností otevřete podokno "Výsledků testů" integrovaného vývojového prostředí a poskytovat nám stav úspěšného/neúspěšného 27 jednotkovými testy součástí náš nový projekt, které zahrnují integrované funkce:

![](create-a-new-aspnet-mvc-project/_static/image16.png)

Později v tomto kurzu vytvoříme se jimi více o automatizované testování a přidejte další jednotkové testy, které se týkají funkčnost aplikace, které můžeme implementovat.

### <a name="next-step"></a>Dalším krokem

Teď máme strukturu základní aplikaci v místě. Pojďme teď [vytvořit databázi pro ukládání dat našich aplikací](create-a-database.md).

> [!div class="step-by-step"]
> [Předchozí](introducing-the-nerddinner-tutorial.md)
> [další](create-a-database.md)
