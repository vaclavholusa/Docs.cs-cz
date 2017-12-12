---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: "Část 2: Řadiče | Microsoft Docs"
author: jongalloway
description: "Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště. Část 2 popisuje řadiče."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: bdafd751e996e759d516d0fa25b09eff21241ed7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="part-2-controllers"></a>Část 2: řadiče
====================
podle [Jon Galloway](https://github.com/jongalloway)

> Úložiště Hudba MVC je kurz aplikace, která představuje a vysvětluje krok za krokem, jak používat rozhraní ASP.NET MVC a Visual Studio pro vývoj webů.  
>   
> Úložiště Hudba MVC je implementace úložiště lightweight ukázkové, který prodává hudebních alb online a implementuje základní Správa serveru, přihlášení uživatele a nákupního košíku funkce.  
>   
> Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště. Část 2 popisuje řadiče.


Tradiční webové rozhraní jsou příchozí adresy URL obvykle mapovány na soubory na disku. Příklad: žádost o adresu URL, například "/ Products.aspx" nebo "/ Products.php" může zpracovat soubor "Products.aspx" nebo "Products.php".

Webové rozhraní MVC mapování adres URL na serverový kód mírně jiným způsobem. Místo mapování příchozí adresy URL k souborům, mapují místo adresy URL pro metody na třídách. Tyto třídy, se nazývají "Řadiče" a jsou zodpovědná za zpracování příchozí požadavky protokolu HTTP a zpracování uživatelského vstupu, načítání a ukládání dat a určení odpověď k odeslání zpět do klienta (zobrazení HTML, stažení souboru, přesměrování na jiný Adresa URL atd.).

## <a name="adding-a-homecontroller"></a>Přidání HomeController

Začneme budete naše aplikace MVC Hudba úložiště přidáním řadiče třídu, která bude zpracovávat adresy URL na domovskou stránku tohoto webu. Jsme budete postupovat podle výchozí zásady vytváření názvů rozhraní ASP.NET MVC a volání HomeController.

"Řadiče" složky v Průzkumníku řešení klikněte pravým tlačítkem a vyberte "Přidat" a "Řadiče..." příkaz:

![](mvc-music-store-part-2/_static/image1.jpg)

Otevře se dialog "Přidat kontroler". Název kontroleru "HomeController" a klikněte na tlačítko Přidat.

![](mvc-music-store-part-2/_static/image1.png)

Tím se vytvoří nový soubor, HomeController.cs, s následujícím kódem:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

Pokud chcete spustit co nejsnáze, můžeme nahraďte metodu Index jednoduché metodu, která právě vrátí řetězec. Vytočit dvě změny:

- Změnit metodu vrátit řetězec místo třídu ActionResult
- Změnit návratový vrátit "Hello z Domů"

Metoda by měl nyní vypadat takto:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a>Spuštění aplikace

Teď umožňuje spuštění tohoto webu. Můžeme spustí naše webový server a vyzkoušejte webu pomocí kteréhokoli z následujících::

- Zvolte ladění ⇨ spustit ladění položku nabídky
- Klikněte na tlačítko zelenou šipku na panelu nástrojů![](mvc-music-store-part-2/_static/image2.jpg)
- Použijte klávesovou zkratku, F5.

Pomocí kterékoli z výše uvedené kroky budou kompilaci naše projektu a potom způsobit, že je ASP.NET Development Server, který je integrovaný do aplikace Visual Web Developer spustit. Oznámení se zobrazí v dolním rohu obrazovky indikující, že je ASP.NET Development Server byla spuštěna a zobrazí číslo portu, zda je spuštěna pod.

![](mvc-music-store-part-2/_static/image2.png)

Visual Web Developer se pak automaticky otevře okno prohlížeče, jejichž adresa URL odkazuje na našem webu. To vám umožní nám pro rychlé vyzkoušení naše webové aplikace:

![](mvc-music-store-part-2/_static/image3.png)

V pořádku, který byl poměrně rychle – jsme vytvořili nový web, přidat tři vložených funkcí a My jsme text v prohlížeči. Není Atomový vědecké účely, ale je spuštění.

*Poznámka: Visual Web Developer obsahuje vývojový Server ASP.NET, který se spustí vašeho webu na číslo volné náhodných "portu". Na snímku obrazovky výše, lokality se spouští ve `http://localhost:26641/`, takže používá port 26641. Vaše číslo portu se liší. Když v souvislosti se například /Store/Browse adresy URL v tomto kurzu, který přejde po číslo portu. Za předpokladu, že číslo portu 26641, procházení/úložiště / procházení bude znamenat, že procházení k `http://localhost:26641/Store/Browse`.*

## <a name="adding-a-storecontroller"></a>Přidání StoreController

Jsme přidali jednoduché HomeController, který implementuje domovské stránce tohoto webu. Nyní Pojďme přidat jiného řadiče, který použijeme k implementaci funkci procházení naše Hudba úložiště. Naše řadič úložiště bude podporovat tři scénáře:

- Stránka výpis žánry Hudba v našem úložišti Hudba
- Procházet stránky, který se zobrazí seznam všech hudebních alb v konkrétní genre
- Stránka Podrobnosti, která se zobrazují informace o konkrétní Hudba album

Začneme přidáním nové třídy StoreController... Pokud jste to ještě neudělali, zastaví aplikace buď zavření prohlížeče nebo výběrem položky nabídky Zastavte ladění ladění ⇨.

Nyní přidejte nové StoreController. Stejně jako jsme to udělali s HomeController, provedeme to pravým tlačítkem na složku "Řadiče" v Průzkumníku řešení a zvolením Add -&gt;řadiče položky nabídky

![](mvc-music-store-part-2/_static/image4.png)

Naše nové StoreController již metodu "Index". Tato metoda "Index" použijeme k implementaci na naší stránce výpis, který obsahuje seznam všech žánry v našem úložišti Hudba. Přidáme také dva další metody k implementaci dva scénáře chceme, aby naše StoreController pro zpracování: procházení a podrobnosti.

Tyto metody (Index, procházet a podrobnosti) v rámci Kontroleru se používá označení "Akce Kontroleru" a jak jste už viděli s metodou akce HomeController.Index (), jejich úlohy je a reagovat na požadavky na adresu URL (obecně) určit, který obsah by měly být odeslány zpět do prohlížeče nebo uživatele, který volá adresu URL.

Začneme naše implementace StoreController změnou theIndex() metoda vrátí řetězec "Hello z Store.Index()" a přidáme podobné metody pro Browse() a Details():

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

Spusťte projekt znovu a přejděte na následující adresy URL:

- / Úložiště
- / Úložiště nebo procházení
- / / Podrobnosti úložiště

Přístup k tyto adresy URL vyvolání metod akce v rámci Kontroleru a vrátí řetězec odpovědí:

![](mvc-music-store-part-2/_static/image5.png)

To je výborné, ale jedná se o řetězce pouze konstantní. Umožňuje převést na dynamický, tak, aby se trvat informace z adresy URL a zobrazit ji ve výstupu stránky.

Nejprve Změníme metody akce procházet načíst hodnotu řetězce dotazu z adresy URL. Jsme to můžete provést tak, že přidáte parametr "genre" na našem metodu akce. Když jsme to ASP.NET MVC automaticky předat žádné parametry post řetězci dotazu nebo formuláře při vyvolání s názvem "genre" na našem metodu akce.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

*Poznámka: Používáme metodu HttpUtility.HtmlEncode nástroj pro úpravu vstup uživatele. To zabrání uživatelům vložení Javascript do našich zobrazení s odkazem jako /Store/Browse? Genre =&lt;skriptu&gt;window.location= 'http://hackersite.com'&lt;/script&gt;.*

Teď umožňuje procházet/úložiště / Procházet? Genre = Disco

![](mvc-music-store-part-2/_static/image6.png)

Změňte další akce ke čtení a zobrazení vstupní parametr s názvem ID. Na rozdíl od našich předchozí metoda jsme nebude vložení hodnota ID jako parametr řetězce dotazu. Místo toho jsme budete vložte přímo v rámci adresy URL samotné. Příklad: /Store/Details/5.

ASP.NET MVC umožňuje nám snadno to provést bez nutnosti měnit žádnou konfiguraci. ASP.NET MVC výchozí konvenci směrování se zachází segment adresy URL jako parametr s názvem "ID" po název metody akce. Pokud má parametr s názvem ID své metodě akce, poté ASP.NET MVC automaticky předá segment adresy URL je jako parametr.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

Spusťte aplikaci a přejděte do /Store/Details/5:

![](mvc-music-store-part-2/_static/image7.png)

Pojďme recap, co máme jste Hotovo, pokud:

- Vytvořili jsme nový projekt ASP.NET MVC v aplikaci Visual Web Developer
- Jsme probrali struktura složek základní architektury ASP.NET MVC
- Jsme jste se naučili spuštění naše webu pomocí vývojový Server ASP.NET
- Vytvořili jsme dvě třídy Controller: HomeController a StoreController
- Přidali jsme metody akce na našem řadiče, které reagují na požadavky na adresu URL a vrací text do prohlížeče


>[!div class="step-by-step"]
[Předchozí](mvc-music-store-part-1.md)
[další](mvc-music-store-part-3.md)
