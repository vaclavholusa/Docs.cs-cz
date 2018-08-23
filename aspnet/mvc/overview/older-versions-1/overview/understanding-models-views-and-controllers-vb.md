---
uid: mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-vb
title: Principy modelů, zobrazení a Kontrolerů (VB) | Dokumentace Microsoftu
author: StephenWalther
description: Ztrácíte přehled o modelů, zobrazení a Kontrolerů? V tomto kurzu Stephen Walther vás seznámí s různé části aplikace ASP.NET MVC.
ms.author: riande
ms.date: 08/19/2008
ms.assetid: a106374a-5e74-4fd0-9ac0-1a32280e5d0d
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-vb
msc.type: authoredcontent
ms.openlocfilehash: 15d4e7d7b6a2662296b8e3647cd60187de580789
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755448"
---
<a name="understanding-models-views-and-controllers-vb"></a>Principy modelů, zobrazení a Kontrolerů (VB)
====================
podle [Stephen Walther](https://github.com/StephenWalther)

> Ztrácíte přehled o modelů, zobrazení a Kontrolerů? V tomto kurzu Stephen Walther vás seznámí s různé části aplikace ASP.NET MVC.


Tento kurz obsahuje podrobný přehled ASP.NET MVC modelů, zobrazení a kontrolerů. Jinými slovy, najdete v něm M ", V" a jazyka C "v architektuře ASP.NET MVC.

Po přečtení tohoto kurzu, měli byste porozumět, jak fungují společně různé části aplikace ASP.NET MVC. Měli byste porozumět, jak architektury aplikace ASP.NET MVC se liší od aplikace webových formulářů ASP.NET nebo aplikace ASP.

## <a name="the-sample-aspnet-mvc-application"></a>Ukázkovou aplikaci ASP.NET MVC

Výchozí šablony sady Visual Studio pro vytváření webových aplikací ASP.NET MVC zahrnuje jednoduchost ukázkové aplikace, která slouží k pochopení různé části aplikace ASP.NET MVC. Můžeme využít výhod této jednoduché aplikace najdete v tomto kurzu.

Vytvoření nové aplikace ASP.NET MVC pomocí šablony MVC spuštěním sady Visual Studio 2008 a vyberte možnost nabídky soubor, nový projekt (viz obrázek 1). V dialogovém okně Nový projekt, vyberte vašem oblíbeném programovacím jazyce podle typů projektu (Visual Basic nebo C#) a vyberte **webové aplikace ASP.NET MVC** v rámci šablony. Klikněte na tlačítko OK.


[![Dialogové okno nového projektu](understanding-models-views-and-controllers-vb/_static/image1.jpg)](understanding-models-views-and-controllers-vb/_static/image1.png)

**Obrázek 01**: dialogové okno nového projektu ([kliknutím ji zobrazíte obrázek v plné velikosti](understanding-models-views-and-controllers-vb/_static/image2.png))


Při vytváření nové aplikace ASP.NET MVC **vytvořit projekt testování částí** se zobrazí dialogové okno (viz obrázek 2). Tento dialog umožňuje vytvořit samostatný projekt v řešení pro testování vašich aplikací ASP.NET MVC. Vyberte možnost **Ne, nevytvářejte projekt testování částí** a klikněte na tlačítko **OK** tlačítko.


[![Vytvořte testovací jednotky dialogové okno](understanding-models-views-and-controllers-vb/_static/image2.jpg)](understanding-models-views-and-controllers-vb/_static/image3.png)

**Obrázek 02**: dialogové okno Vytvoření testu jednotek ([kliknutím ji zobrazíte obrázek v plné velikosti](understanding-models-views-and-controllers-vb/_static/image4.png))


Aplikace se vytvoří po nové technologie ASP.NET MVC. Zobrazí se několik složek a souborů v okně Průzkumník řešení. Zejména uvidíte tři složky s názvem modelů, zobrazení a Kontrolerů. Jak může uhodnout z názvy složek, tyto složky obsahují soubory pro implementaci modelů, zobrazení a kontrolerů.

Pokud rozbalíte složce řadiče, měli byste vidět soubor s názvem AccountController.vb a soubor s názvem HomeController.vb. Pokud rozbalíte složce zobrazení, měli byste vidět tři podsložky s názvem účtu, Home a sdílené. Pokud rozbalíte domovské složky, zobrazí se vám dva další soubory s názvem About.aspx a Index.aspx (viz obrázek 3). Tyto soubory tvoří ukázkové aplikace je součástí výchozí šablony ASP.NET MVC.


[![V okně Průzkumník řešení](understanding-models-views-and-controllers-vb/_static/image3.jpg)](understanding-models-views-and-controllers-vb/_static/image5.png)

**Obrázek 03**: okně Průzkumníka řešení ([kliknutím ji zobrazíte obrázek v plné velikosti](understanding-models-views-and-controllers-vb/_static/image6.png))


Ukázkovou aplikaci můžete spustit tak, že vyberete možnost nabídky **ladit, spustit ladění**. Alternativně můžete stisknutím klávesy F5.

Při prvním spuštění aplikace ASP.NET, zobrazí se dialogové okno na obrázku 4, která se doporučuje, abyste povolili režimu ladění. Klikněte na tlačítko OK a bude aplikace spuštěna.


[![Ladění není povoleno dialogového okna](understanding-models-views-and-controllers-vb/_static/image4.jpg)](understanding-models-views-and-controllers-vb/_static/image7.png)

**Obrázek 04**: ladění není povoleno dialogového okna ([kliknutím ji zobrazíte obrázek v plné velikosti](understanding-models-views-and-controllers-vb/_static/image8.png))


Při spuštění aplikace ASP.NET MVC, Visual Studio spustí aplikaci ve webovém prohlížeči. Ukázková aplikace se skládá pouze dvě stránky: indexovou stránku a na stránce o. Při prvním spuštění aplikace, zobrazí se stránka indexu (viz obrázek 5). Můžete přejít na stránku o kliknutím na odkaz nabídce v horní části přímo z aplikace.


[![Index stránky](understanding-models-views-and-controllers-vb/_static/image5.jpg)](understanding-models-views-and-controllers-vb/_static/image9.png)

**Obrázek 05**: indexovou stránku ([kliknutím ji zobrazíte obrázek v plné velikosti](understanding-models-views-and-controllers-vb/_static/image10.png))


Všimněte si, že adresy URL do adresního řádku prohlížeče. Například když kliknete na odkaz o nabídce, adresy URL v adresním řádku prohlížeče se změní na **/Home/About**.

Pokud zavřete okno prohlížeče a vraťte se do sady Visual Studio, nebudete mít k nalezení souboru s domovem cesta/o. Soubory neexistují. Jak je to možné?

## <a name="a-url-does-not-equal-a-page"></a>Adresa URL se nerovná stránku

Při sestavování tradiční aplikace webových formulářů ASP.NET nebo Active Server Pages aplikace existuje shoda mezi adresy URL a na stránce. Pokud jste požádali o stránku s názvem SomePage.aspx ze serveru, pak měli lépe existovat stránku na disk s názvem SomePage.aspx. Pokud se soubor SomePage.aspx neexistuje, dojde bez zbytečných prvků **404 – Stránka nebyla nalezena** chyby.

Při sestavování aplikace ASP.NET MVC, naproti tomu neexistuje žádná souvislost mezi adresu URL, kterou zadáte do panelu Adresa v prohlížeči a soubory, které můžete najít ve vaší aplikaci. V aplikaci ASP.NET MVC adresa URL odpovídá místo na disku na stránce akce kontroleru.

V tradiční aplikace ASP.NET nebo ASP prohlížeč požadavek mapovaný na stránky. V aplikaci ASP.NET MVC naproti tomu prohlížeč požadavky se mapují na akce kontroleru. Aplikace webových formulářů technologie ASP.NET je zaměřené na obsah. Aplikace ASP.NET MVC, je naopak zaměřenou na logiku aplikace.

## <a name="understanding-aspnet-routing"></a>Vysvětlení směrování ASP.NET

Získá mapovat žádost prohlížeče na akce kontroleru prostřednictvím funkce technologie ASP.NET framework, volá se, *směrování ASP.NET*. Směrování ASP.NET používá rozhraní ASP.NET MVC do *trasy* příchozí požadavky na akce kontroleru.

Směrovací tabulku směrování ASP.NET používá ke zpracování příchozích požadavků. Tato tabulka trasy se vytvoří při prvním spuštění vaší webové aplikace. Směrovací tabulky se nastavení v souboru Global.asax. Výchozí soubor MVC Global.asax je součástí výpis 1.

**Výpis 1 - Global.asax**

[!code-vb[Main](understanding-models-views-and-controllers-vb/samples/sample1.vb)]

Když poprvé spustí aplikace technologie ASP.NET, aplikace\_volání metody Start(). Ve výpisu 1 Tato metoda volá metodu RegisterRoutes() a metoda RegisterRoutes() vytvoří výchozí směrovací tabulka.

Výchozí směrovací tabulka se skládá z jednoho z nich. Všechny příchozí požadavky této výchozí trase rozdělí do tří segmentů (segment adresy URL se něco mezi lomítka). První segment se mapuje na název kontroleru, druhý segment se mapuje na název akce a posledního segmentu je namapována na parametr předaný akce s názvem ID.

Představte si třeba následující adresu URL:

/ Produkt/podrobnosti/3

Tato adresa URL je analyzován do tří parametrů takto:

Kontroler = produktu

Akce = podrobnosti

ID = 3

Výchozí trasy definované v souboru Global.asax obsahuje výchozí hodnoty pro všemi třemi parametry. Je výchozí kontroler Home, výchozí akce je Index a výchozí hodnota Id je prázdný řetězec. U tyto výchozí hodnoty na paměti zvažte, jak je analyzován na následující adrese URL:

/ Zaměstnance

Tato adresa URL je analyzován do tří parametrů takto:

Kontroler = zaměstnance

Akce = Index

ID =

Nakonec otevřete aplikaci ASP.NET MVC bez poskytnutí libovolnou adresu URL (například `http://localhost`) pak adresa URL se zpracuje tímto způsobem:

Kontroler = Home

Akce = Index

ID =

Požadavek se přesměruje na akci Index() HomeController třídy.

## <a name="understanding-controllers"></a>Vysvětlení Kontrolerů

Kontroler je zodpovědná za řízení tak, jak uživatel komunikuje s aplikaci MVC. Kontroler obsahuje logiku řízení toku pro aplikace ASP.NET MVC. Kontroler Určuje, jaké odpověď k odeslání zpět na uživatele, když uživatel provede požadavek prohlížeče.

Kontroler je právě třídy (například třídy jazyka Visual Basic nebo C#). Ukázka aplikace ASP.NET MVC zahrnuje řadič HomeController.vb umístěný ve složce řadiče. Obsah souboru HomeController.vb je uveden v informacích 2.

**Výpis 2 - HomeController.cs**

[!code-vb[Main](understanding-models-views-and-controllers-vb/samples/sample2.vb)]

Všimněte si, HomeController má dvě metody s názvem Index() a About(). Tyto dvě metody odpovídají dvě akce vystavené kontroleru. Adresa URL/Home/Index vyvolá metodu HomeController.Index() a adresy URL/Home/o vyvolá metodu HomeController.About().

Všechny veřejné metody v kontroleru je vystavena jako akce kontroleru. Musíte být opatrní při to. To znamená, že všechny veřejné metody obsažené v kontroleru, každý, kdo má přístup k Internetu jde vyvolat zadáním správné adresy URL do prohlížeče.

## <a name="understanding-views"></a>Principy zobrazení

Akce dvě kontroleru vystavené třídu HomeController Index() a About(), jak vrátit zobrazení. Zobrazení obsahuje kód HTML a obsah, který je odesláno prohlížeči. Zobrazení je ekvivalentem stránky při práci s aplikací ASP.NET MVC.

Je třeba vytvořit zobrazení na správném místě. Akce HomeController.Index() vrátí zobrazení nachází v následujícím umístění:

\Views\Home\Index.aspx

Akce HomeController.About() vrátí zobrazení nachází v následujícím umístění:

\Views\Home\About.aspx

Obecně platí Pokud budete chtít vrátit zobrazení pro akce kontroleru, musíte vytvořit podsložku ve složce zobrazení se stejným názvem jako řadiče. V podsložce musíte vytvořit soubor .aspx se stejným názvem jako akce kontroleru.

Soubor výpisu 3 obsahuje About.aspx zobrazení.

**Výpis 3 - About.aspx**

[!code-aspx[Main](understanding-models-views-and-controllers-vb/samples/sample3.aspx)]

Pokud budete ignorovat prvního řádku v zobrazení 3, většina ostatních zobrazení se skládá z standardní HTML. Obsah zobrazení můžete upravit tak, že zadáte veškeré kódování HTML, který chcete, aby zde.

Zobrazení je velmi podobný stránky ASP nebo ASP.NET webové formuláře. Zobrazení může obsahovat obsah ve formátu HTML a skripty. Napíšete skripty v oblíbeném .NET programovací jazyk (například C# nebo Visual Basic .NET). Pomocí skriptů zobrazují dynamický obsah, jako je například dat z databáze.

## <a name="understanding-models"></a>Principy modelů

Mít jsme probírali řadiče a mít jsme probírali zobrazení. Poslední téma, které potřebujeme fattica je modely. Co je MVC model?

MVC model obsahuje všechny vaše aplikační logiky, která není součástí zobrazení nebo kontroleru. Model by měl obsahovat všechny vaše obchodní logiky aplikace, logiku ověřování a logiky přístupu k databázi. Například pokud Microsoft Entity Framework používají pro přístup k vaší databázi, potom by vytvoříte třídy Entity Framework (soubor .edmx) ve složce modely.

Zobrazení může obsahovat pouze logiku související generování uživatelského rozhraní. Kontroler může obsahovat jenom úplné minimální logiku potřebnou k vrácení z druhého zobrazení nebo přesměrovat uživatele na jinou akci (řízení toku). Všechno ostatní, měly by být součástí modelu.

Obecně platí je nutné snažit pro fat modely a tenké kontrolery. Vaše metody kontroleru by měl obsahovat jenom pár řádků kódu. Získá příliš fat-akce kontroleru, pak zvažte přesunutí logiky na novou třídu ve složce modely.

## <a name="summary"></a>Souhrn

V tomto kurzu vám poskytuje základní přehled různých součástí ASP.NET MVC webové aplikace. Jste se naučili, jak směrování ASP.NET mapuje příchozí požadavky na prohlížeč pro určitý kontroler akce. Jste se naučili, jak orchestrovat řadiče jak zobrazení se vrátí do prohlížeče. Nakonec jste zjistili, jak modely obsahovat obchodní aplikace, ověření a logiky přístupu k databázi.
