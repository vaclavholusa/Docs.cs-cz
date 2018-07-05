---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: '2. část: Kontrolery | Dokumentace Microsoftu'
author: jongalloway
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store. 2. část se věnuje řadiče.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: a854feff675cae302b9927d209808257b7d087cb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381800"
---
<a name="part-2-controllers"></a>2. část: Kontrolery
====================
podle [Jon Galloway](https://github.com/jongalloway)

> MVC Music Store jde o kurz, který se seznámíte, podrobné postupy pro vývoj pro web pomocí ASP.NET MVC a sady Visual Studio.  
>   
> Music Store MVC je jednoduché ukázku implementace úložiště prodává hudebních alb online, který implementuje správu základního webu, přihlášení uživatele a nákupního košíku funkce.  
>   
> V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store. 2. část se věnuje řadiče.


Příchozí adresy URL se s tradičními webovými rozhraními obvykle mapují na soubory na disku. Příklad: požadavek na adresu URL, jako je "/ Products.aspx" nebo "/ Products.php" může být zpracována "Products.aspx" nebo "Products.php" soubor.

Webové rozhraní MVC mapování adres URL do kódu serveru mírně odlišným způsobem. Místo souborů mapování příchozích adres URL, že místo toho mapování adres URL na metody třídy. Tyto třídy se nazývají "Řadiče" a zodpovídají za zpracování příchozích požadavků HTTP, zpracování uživatelského vstupu, načítání a ukládání dat a určení odpověď k odeslání zpátky do klienta (zobrazení HTML, stáhněte si soubor, přesměrovat na jinou Adresa URL atd.).

## <a name="adding-a-homecontroller"></a>Přidání HomeController

Brzy jsme naše aplikace MVC Music Store tak, že přidáte třídu Kontroleru, který bude zpracovávat adresy URL na domovskou stránku našeho webu. Vytvoříme dodržují konvence pojmenování výchozí rozhraní ASP.NET MVC a jeho volání HomeController.

Klikněte pravým tlačítkem na složku "Řadiče" v Průzkumníkovi řešení a vyberte "Add" a "Kontroleru..." příkaz:

![](mvc-music-store-part-2/_static/image1.jpg)

Tím se otevře dialogové okno "Přidat kontroler". Název kontroleru "HomeController" a stiskněte tlačítko Přidat.

![](mvc-music-store-part-2/_static/image1.png)

Tím se vytvoří nový soubor, HomeController.cs, následujícím kódem:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

Spustit co nejsnáze, podíváme nahradit Index – metoda s jednoduchý způsob, který právě vrátí hodnotu typu string. Teď uděláme dvě změny:

- Změnit metodu k vrácení řetězce, místo třídu ActionResult
- Změnit návratový příkaz návrat "Hello z"

Metoda by teď měl vypadat takto:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a>Spuštění aplikace

Teď spustíme webu. Jsme start náš webový server a vyzkoušejte si web pomocí kteréhokoli z následujících akcí:

- Vyberte položku nabídky spustit ladění ladění ⇨
- Klikněte na zelenou šipku tlačítko na panelu nástrojů ![](mvc-music-store-part-2/_static/image2.jpg)
- Použijte klávesovou zkratku, F5.

Některou z výše uvedených kroků se kompilace projektu a poté způsobí serveru ASP.NET Development Server, která je integrovaná do aplikace Visual Web Developer spustit. Oznámení se zobrazí v dolním rohu obrazovky, chcete-li určit, že má spuštění serveru ASP.NET Development Server a zobrazí číslo portu, že je spuštěný pod.

![](mvc-music-store-part-2/_static/image2.png)

Visual Web Developer se pak automaticky otevře okno prohlížeče, jehož adresa URL odkazuje na našem webovém serveru. Umožní vám to rychle vyzkoušet naši webovou aplikaci:

![](mvc-music-store-part-2/_static/image3.png)

Dobře, to bylo poměrně rychlé – jsme vytvořili nový web, přidány tři vložených funkcí a máme text v prohlížeči. Není Atomový vědy, ale je spuštění.

*Poznámka: Visual Web Developer zahrnuje serveru ASP.NET Development Server, který se spustí váš web na číslo náhodný volný "portu". Na snímku obrazovky výše je web spuštěný v `http://localhost:26641/`, takže se používá port 26641. Vaše číslo portu se liší. Když mluvíme o like /Store/Browse adresy URL v tomto kurzu, který přejde po číslo portu. Za předpokladu, že číslo portu 26641, přejdete do/Store/procházet bude znamenat, že přejdete na adresu `http://localhost:26641/Store/Browse`.*

## <a name="adding-a-storecontroller"></a>Přidání StoreController

Přidali jsme jednoduché HomeController, který implementuje domovské stránce našeho webu. Teď přidáme jiný kontroler, který použijeme k implementaci funkcionality procházení našeho music úložiště. Kontroleru úložiště bude podporovat tři scénáře:

- Stránce žánrů Hudba v našich music store
- Procházet stránku, která obsahuje seznam všech hudebních alb v konkrétní žánr
- Stránka s podrobnostmi s informacemi o alba konkrétní Hudba

Začneme tak, že přidáte novou třídu StoreController... Pokud jste tak dosud neučinili, zastaví aplikace tak, že zavření prohlížeče nebo jeho výběru položky nabídky ladění Zastavit ladění ⇨.

Nyní přidejte nové StoreController. Stejným způsobem, jako jsme to udělali s HomeController, Uděláme to tak, že pravým tlačítkem na složku "Řadiče" v Průzkumníkovi řešení a zvolíte Add -&gt;řadič položky nabídky

![](mvc-music-store-part-2/_static/image4.png)

Naše nové StoreController již má metodu "Index". Tato metoda "Index" použijeme k implementaci naší stránce, která obsahuje seznam všech žánry v našich music store. Přidáme také další dvě metody k implementaci těchto dvou dalších scénářích chceme, aby naše StoreController ke zpracování: procházení a podrobnosti.

Tyto metody (Index, procházet a podrobnosti) v rámci Kontroleru se používá označení "Akce Kontroleru" a jak pomocí metody akce () HomeController.Index již víte, své práce je a reagovat na žádosti o adresy URL (obecně) určit, jaký obsah by měly být odeslány zpět do prohlížeče nebo uživatel, který vyvolal adresu URL.

Začneme tak, že změníte theIndex() metoda vrátí řetězec "Hello z Store.Index()" naše implementace StoreController a přidáme Browse() a Details() podobné metody:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

Spusťte projekt znovu a přejděte na následující adresy URL:

- / Store
- / Store/procházení
- / Store/podrobností

Přístup k těmto adresám URL vyvolání metod akce v Kontroleru a vrátí řetězec odpovědi:

![](mvc-music-store-part-2/_static/image5.png)

To je skvělé, ale jedná se pouze konstantní řetězce. Vytvoříme je dynamická, takže využít informace z adresy URL a zobrazit je výstup stránky.

Nejprve Změníme metody akce procházení k načtení hodnoty řetězce dotazu z adresy URL. Můžeme to udělat tak, že přidáte "žánr" parametru k metodě akce. Když to uděláme ASP.NET MVC předá automaticky všechny řetězce dotazu nebo parametry formuláře post s názvem "žánr" k metodě akce, při jeho vyvolání.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

*Poznámka: Pomocí metody nástrojů HttpUtility.HtmlEncode kterého neupravují uživatelský vstup. To zabrání uživatelům v vkládání jazyka Javascript do našich zobrazení s odkazem jako /Store/Browse? Rozšířením podle tematických =&lt;skript&gt;window.location= "http://hackersite.com"&lt;/script&gt;.*

Nyní Pojďme procházet/Store/Procházet? Rozšířením podle tematických = Roz

![](mvc-music-store-part-2/_static/image6.png)

Dále Změníme podrobnosti akce ke čtení a zobrazení vstupní parametr s názvem ID. Na rozdíl od našich předchozí metoda jsme nebude vkládání hodnotu ID jako parametr řetězce dotazu. Místo toho vložíme ho přímo v rámci URL samotného. Příklad: /Store/Details/5.

ASP.NET MVC umožňuje nám to snadno provést nemusíte nic konfigurovat. ASP.NET MVC výchozí konvenci směrování se zachází segment adresy URL po názvu metody akce, jako parametr s názvem "ID". Pokud vaše metoda akce má parametr ID ASP.NET MVC se automaticky předat segment adresy URL pro vás jako parametr.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

Spusťte aplikaci a přejděte do /Store/Details/5:

![](mvc-music-store-part-2/_static/image7.png)

Pojďme rekapitulace, co jsme dosud provedli:

- Jsme vytvořili nový projekt ASP.NET MVC v aplikaci Visual Web Developer
- Výše strukturu složek základní aplikaci ASP.NET MVC
- Jsme jste zjistili, jak spustit našeho webu pomocí serveru ASP.NET Development Server
- Vytvořili jsme dvě třídy Kontroleru: HomeController a StoreController
- Přidali jsme metody akce k naší řadiče, které reagují na požadavky na adresu URL a vrací text do prohlížeče


> [!div class="step-by-step"]
> [Předchozí](mvc-music-store-part-1.md)
> [další](mvc-music-store-part-3.md)
